---
description: Auto-decompose task + gated execution with stops at decision points
subtask: true
---

# Swarm - Auto-Decompose + Gated Execution

Automatically decompose a complex task into parallel subtasks and execute with gate stops for user control.

## Input Arguments

The task to swarm: **$ARGUMENTS**

## Usage

```
/swarm <task_description> [options]
```

Options:
- `--max-workers=N` - Maximum parallel workers (default: 5)
- `--no-gates` - Skip gate confirmations (dangerous)
- `--dry-run` - Plan only, don't execute
- `--continue` - Resume from last gate

## Protocol

### Phase 1: Task Decomposition

1. **Analyze the Request**: Understand scope, dependencies, and parallelization opportunities
2. **Identify Atomic Units**: Break into smallest independently executable pieces
3. **Build Dependency Graph**: Map which tasks depend on others
4. **Group by Wave**: Organize tasks that can run in parallel

### Phase 2: Plan Output

```markdown
## Swarm Plan: [Task Summary]

### Wave 1 (Parallel - 3 tasks)
| ID | Task | Est. Time | Dependencies |
|----|------|-----------|--------------|
| 1.1 | [Task description] | 2m | none |
| 1.2 | [Task description] | 3m | none |
| 1.3 | [Task description] | 1m | none |

### Wave 2 (Parallel - 2 tasks)
| ID | Task | Est. Time | Dependencies |
|----|------|-----------|--------------|
| 2.1 | [Task description] | 4m | 1.1, 1.2 |
| 2.2 | [Task description] | 2m | 1.3 |

### Wave 3 (Sequential - 1 task)
| ID | Task | Est. Time | Dependencies |
|----|------|-----------|--------------|
| 3.1 | [Final integration] | 5m | 2.1, 2.2 |

### Gate Points
- **Gate 0**: Before Wave 1 (START)
- **Gate 1**: After Wave 1
- **Gate 2**: After Wave 2
- **Gate 3**: Before completion (FINAL)

### Resource Estimate
- Total tasks: 6
- Parallel waves: 3
- Estimated time: 8-12 minutes
- Max workers: 5
```

### Phase 3: Gate Protocol

**STOP at each gate and ask:**

```
## 🚧 Gate [N]: [Gate Name]

### Completed
- [x] Task 1.1 ✓
- [x] Task 1.2 ✓
- [x] Task 1.3 ✓

### Next Wave Preview
- [ ] Task 2.1 - [description]
- [ ] Task 2.2 - [description]

### Options
1. **continue** - Proceed to next wave
2. **review** - Review changes before continuing
3. **modify** - Adjust the plan
4. **stop** - Stop execution here

What would you like to do?
```

### Phase 4: Parallel Execution

When proceeding from a gate:

```markdown
## Executing Wave [N]

### Spawning Workers
- Worker 1: Task 2.1
- Worker 2: Task 2.2

### Progress
[Real-time updates on task progress]

### Results
- Task 2.1: ✓ Complete
- Task 2.2: ✓ Complete

### Next: Gate [N+1]
```

## Execution Rules

1. **ALWAYS stop at gates** unless `--no-gates` is specified
2. **WAIT for user input** at each gate before proceeding
3. **REPORT failures immediately** - don't hide errors
4. **ALLOW plan modification** at any gate
5. **SUPPORT resume** from any gate point

## Example Session

```
User: /swarm refactor authentication module

[Phase 1: Decomposition]
- Identifying components...
- Building dependency graph...
- Creating execution waves...

[Phase 2: Plan Output]
## Swarm Plan: Refactor Authentication Module
[... detailed plan ...]

[Phase 3: Gate 0]
🚧 Gate 0: START
Ready to begin Wave 1 with 3 parallel tasks.
> continue

[Phase 4: Wave 1 Execution]
- Spawning workers...
- Task 1.1: Extract auth utils → ✓
- Task 1.2: Create auth middleware → ✓
- Task 1.3: Update tests → ✓

🚧 Gate 1: Wave 1 Complete
> continue

[... continues through all waves ...]

## Swarm Complete
- Total tasks: 6
- Completed: 6
- Failed: 0
- Time: 8m 32s
```

## Failure Recovery

If a task fails:

```markdown
## ⚠️ Task Failure

### Failed Task
- **ID**: 2.1
- **Error**: [Error description]

### Options
1. **retry** - Retry the failed task
2. **skip** - Skip and continue (may cause issues)
3. **abort** - Stop swarm execution
4. **fix** - Attempt automatic fix

What would you like to do?
```

---

**CRITICAL**: Gates exist for safety. Only skip with `--no-gates` if you understand the risks!
