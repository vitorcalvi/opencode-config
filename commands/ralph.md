---
description: Decompose prompt into plan; execute only if execute=true & dry_run=false
subtask: true
---

# Ralph Loop - Self-Referential Development Loop

Execute a self-referential development loop that decomposes the user's request into a plan and executes it with gated control.

## Input Arguments

The user's request: **$ARGUMENTS**

## Execution Parameters

Before proceeding, check these parameters in the request or ask the user:

| Parameter | Default | Purpose |
|-----------|---------|---------|
| `execute` | `false` | If `true`, execute the plan. If `false`, only generate the plan. |
| `dry_run` | `true` | If `true`, show what would be done without making changes. |
| `max_iterations` | `10` | Maximum loop iterations before stopping. |
| `gate_points` | `true` | Stop at each gate point for user confirmation. |

## Protocol

### Phase 1: Decomposition

1. **Parse the Request**: Identify the core objective, constraints, and success criteria
2. **Identify Subtasks**: Break down into atomic, verifiable units
3. **Detect Dependencies**: Map which subtasks depend on others
4. **Create Execution Graph**: Order subtasks by dependency

### Phase 2: Plan Generation

Output a structured plan:

```markdown
## Ralph Plan: [Objective]

### Subtasks
1. [Task 1] - deps: none - status: pending
2. [Task 2] - deps: [1] - status: pending
3. [Task 3] - deps: [1, 2] - status: pending

### Gate Points
- Gate 1: After Task 1 completion
- Gate 2: After Task 2 completion
- Final Gate: Before marking complete

### Execution Mode
- execute: [true/false]
- dry_run: [true/false]
- gate_points: [true/false]
```

### Phase 3: Execution (if execute=true AND dry_run=false)

**ONLY EXECUTE IF BOTH CONDITIONS ARE MET:**
- `execute=true` (explicitly requested)
- `dry_run=false` (not in simulation mode)

**If dry_run=true**: Output what would be done, do NOT make changes.

**If execute=false**: Stop after plan generation.

**If gate_points=true**: Pause at each gate for user confirmation.

### Phase 4: Loop Continuation

After each iteration:
1. Evaluate completion status
2. If incomplete and iterations < max_iterations: continue
3. If blocked: ask user for guidance
4. If complete: output summary and exit

## Output Format

```
## Ralph Status: [PLANNING|EXECUTING|BLOCKED|COMPLETE|DRY_RUN]

### Current Phase
[Current phase description]

### Progress
- [x] Completed task 1
- [ ] Pending task 2
- [ ] Blocked task 3 (reason)

### Next Action
[What will happen next]

### Gate Decision Required
[If at a gate point, ask for confirmation]
```

## Examples

### Dry Run Mode
```
/ralph implement user authentication
```
→ Generates plan only, no execution

### Execute Mode
```
/ralph implement user authentication execute=true dry_run=false
```
→ Generates plan AND executes with gates

### Skip Gates
```
/ralph implement user authentication execute=true dry_run=false gate_points=false
```
→ Executes without stopping at gates

---

**CRITICAL**: Never execute changes unless `execute=true` AND `dry_run=false` are explicitly set!
