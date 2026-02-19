---
description: Run listed tasks in parallel with resource limits
subtask: true
---

# Parallel - Resource-Limited Parallel Task Runner

Execute multiple independent tasks in parallel with configurable resource limits and coordination.

## Input Arguments

Tasks to run in parallel: **$ARGUMENTS**

## Usage

```
/parallel <task1> | <task2> | <task3> [options]
```

Separate tasks with `|` or provide a task list.

Options:
- `--workers=N` - Max parallel workers (default: auto-detect CPU cores)
- `--timeout=N` - Timeout per task in seconds (default: 300)
- `--fail-fast` - Stop all tasks if any fails
- `--continue-on-error` - Continue even if tasks fail

## Protocol

### Step 1: Parse Tasks

Extract individual tasks from the input:

```
Input: "install deps | run lint | run tests | build"

Tasks:
1. install deps
2. run lint  
3. run tests
4. build
```

### Step 2: Detect Resources

```bash
# Auto-detect available resources
if [ "$(uname -s)" = "Darwin" ]; then
  CPU_CORES=$(sysctl -n hw.ncpu)
  MEM_GB=$(sysctl -n hw.memsize | awk '{print int($1/1024/1024/1024)}')
else
  CPU_CORES=$(nproc 2>/dev/null || echo 4)
  MEM_GB=$(free -g 2>/dev/null | awk '/^Mem:/{print $2}' || echo 8)
fi

# Default workers to 75% of cores, minimum 2
DEFAULT_WORKERS=$(( CPU_CORES * 3 / 4 ))
[ $DEFAULT_WORKERS -lt 2 ] && DEFAULT_WORKERS=2

echo "Available: ${CPU_CORES} cores, ${MEM_GB}GB RAM"
echo "Default workers: ${DEFAULT_WORKERS}"
```

### Step 3: Plan Execution

```markdown
## Parallel Execution Plan

### Tasks (4 total)
| # | Task | Status | Worker |
|---|------|--------|--------|
| 1 | install deps | pending | any |
| 2 | run lint | pending | any |
| 3 | run tests | pending | any |
| 4 | build | pending | any |

### Configuration
- **Max Workers**: 4
- **Timeout**: 300s per task
- **Fail Mode**: continue-on-error
- **Estimated Time**: ~2-5 minutes

### Ready to Execute
Type `proceed` to start parallel execution, or `modify` to adjust.
```

### Step 4: Execute in Parallel

Spawn tasks across workers:

```bash
# Using background processes
execute_task() {
  local task_id=$1
  local task_cmd="$2"
  local log_file="./.parallel_task_${task_id}.log"
  
  echo "[Task $task_id] Starting: $task_cmd"
  
  START_TIME=$(date +%s)
  
  # Execute with timeout
  if timeout $TIMEOUT bash -c "$task_cmd" > "$log_file" 2>&1; then
    END_TIME=$(date +%s)
    DURATION=$((END_TIME - START_TIME))
    echo "[Task $task_id] ✓ Complete (${DURATION}s)"
    return 0
  else
    EXIT_CODE=$?
    echo "[Task $task_id] ✗ Failed (exit: $EXIT_CODE)"
    return $EXIT_CODE
  fi
}

# Spawn workers
PIDS=()
for i in "${!TASKS[@]}"; do
  while [ $(jobs -r | wc -l) -ge $MAX_WORKERS ]; do
    sleep 0.5
  done
  execute_task $i "${TASKS[$i]}" &
  PIDS+=($!)
done

# Wait for all
for pid in "${PIDS[@]}"; do
  wait $pid
done
```

### Step 5: Report Results

```markdown
## Parallel Execution Complete

### Summary
- **Total Tasks**: 4
- **Successful**: 3
- **Failed**: 1
- **Total Time**: 3m 22s

### Task Results
| # | Task | Status | Duration | Notes |
|---|------|--------|----------|-------|
| 1 | install deps | ✅ | 45s | |
| 2 | run lint | ✅ | 12s | |
| 3 | run tests | ✅ | 2m 8s | 142 passed |
| 4 | build | ❌ | 17s | Type error in src/api.ts |

### Failed Task Details
**Task 4: build**
\`\`\`
src/api.ts:42:5 - error TS2345: Argument of type 'string' is not assignable...
\`\`\`

### Logs
- Task 1: ./.parallel_task_1.log
- Task 2: ./.parallel_task_2.log
- Task 3: ./.parallel_task_3.log
- Task 4: ./.parallel_task_4.log

### Next Steps
1. Review the build error in Task 4
2. Fix the type issue in src/api.ts
3. Re-run: /parallel build
```

## Resource Management

### Memory Limits
For memory-intensive tasks:

```bash
# Estimate memory per task
MEM_PER_TASK=$(( MEM_GB * 1024 / MAX_WORKERS ))
echo "Memory per worker: ${MEM_PER_TASK}MB"

# Warn if low memory
if [ $MEM_PER_TASK -lt 512 ]; then
  echo "WARNING: Low memory per worker. Consider reducing --workers"
fi
```

### CPU Throttling
Prevent system overload:

```bash
# Use taskset on Linux to limit CPU affinity
if [ "$(uname -s)" = "Linux" ]; then
  taskset -c 0-$((MAX_WORKERS-1)) bash -c "$task_cmd"
fi
```

## Examples

### Basic Parallel
```
/parallel npm install | npm run lint | npm test
```

### With Resource Limits
```
/parallel heavy-task-1 | heavy-task-2 | heavy-task-3 --workers=2 --timeout=600
```

### Fail-Fast Mode
```
/parallel critical-task-1 | critical-task-2 --fail-fast
```

### Complex Commands
```
/parallel npm run build:web | npm run build:api | npm run build:docs --workers=3
```

## Important Notes

1. **INDEPENDENT TASKS**: Only run tasks that don't depend on each other
2. **RESOURCE AWARE**: Don't exceed available CPU/memory
3. **TIMEOUT**: Always set reasonable timeouts
4. **LOGS**: Check individual logs for detailed output
5. **CLEANUP**: Remove `.parallel_task_*.log` files when done

---

**TIP**: Use `/swarm` for tasks with dependencies, `/parallel` for independent tasks.
