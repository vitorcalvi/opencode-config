---
description: ONE COMMAND FULL POWER - analyze, plan, decompose, parallel execute, verify, ship
subtask: true
---

# ULTRA - One Command Full Power

The ultimate workflow command. Analyzes, plans, decomposes into parallel tasks, executes with resource optimization, verifies, and ships.

## Input

```
/ultra <what you want> [mode]
```

**Modes:**
- `auto` (default) - Full auto, minimal gates, maximum speed
- `gate` - Stop at key decision points for your approval
- `plan` - Plan only, don't execute
- `dry` - Show what would happen, no changes

---

## Protocol

### PHASE 1: ANALYZE (Instant)

```
[ULTRA] Analyzing request...
├── Scope: [identified scope]
├── Files: [files that will be touched]
├── Risks: [potential issues detected]
└── Approach: [high-level strategy]
```

### PHASE 2: DECOMPOSE (Parallel Graph)

```
[ULTRA] Building execution graph...

WAVE 1 (parallel):
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│ Task 1.1        │ │ Task 1.2        │ │ Task 1.3        │
│ [description]   │ │ [description]   │ │ [description]   │
│ ~2min           │ │ ~1min           │ │ ~3min           │
└─────────────────┘ └─────────────────┘ └─────────────────┘

WAVE 2 (depends on WAVE 1):
┌─────────────────────────────────────┐
│ Task 2.1 - Integration              │
│ [description]                       │
│ ~2min                               │
└─────────────────────────────────────┘

WAVE 3 (final):
┌─────────────────────────────────────┐
│ Task 3.1 - Verification & Ship      │
│ [description]                       │
│ ~1min                               │
└─────────────────────────────────────┘

Total: 4 tasks | 3 waves | Est. 5-8 min
Workers: [auto-detected based on CPU]
```

### PHASE 3: EXECUTE (Full Power)

**If mode=auto (default):**

```
[ULTRA] EXECUTING FULL POWER...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

WAVE 1 [████████████████████] 3/3 ✓
├── Task 1.1 ✓ (1m 42s)
├── Task 1.2 ✓ (58s)
└── Task 1.3 ✓ (2m 31s)

WAVE 2 [████████████████████] 1/1 ✓
└── Task 2.1 ✓ (1m 15s)

WAVE 3 [████████████████████] 1/1 ✓
└── Task 3.1 ✓ (45s)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ULTRA] COMPLETE ✓
Time: 6m 21s | Tasks: 5/5 | Failed: 0
```

**If mode=gate:**

```
[ULTRA] Gate 1/3 - Wave 1 Complete
├── ✓ Task 1.1
├── ✓ Task 1.2
└── ✓ Task 1.3

Next: Wave 2 (integration)
[continue|review|modify|stop] >
```

### PHASE 4: VERIFY (Auto)

```
[ULTRA] VERIFICATION...
├── Lint: ✓ Passed
├── Types: ✓ No errors
├── Tests: ✓ 47/47 passed
├── Build: ✓ Success
└── Security: ✓ No issues

All quality gates passed ✓
```

### PHASE 5: SHIP (Ready)

```
[ULTRA] READY TO SHIP

Changes:
├── src/auth/login.ts (modified)
├── src/auth/middleware.ts (created)
├── src/api/routes.ts (modified)
└── tests/auth.test.ts (created)

Commit message:
feat(auth): implement JWT authentication with refresh tokens

- Add login/logout endpoints
- Create auth middleware for protected routes
- Implement token refresh flow
- Add comprehensive test coverage

[commit|review|abort] >
```

---

## Full Examples

### Auto Mode (Maximum Speed)
```
/ultra implement user authentication with OAuth2 and JWT
```
→ Analyzes, plans, executes in parallel, verifies, ready to commit

### Gate Mode (Full Control)
```
/ultra migrate to TypeScript strict mode gate
```
→ Stops at each wave for your approval

### Plan Mode (See Before Doing)
```
/ultra refactor API layer to use tRPC plan
```
→ Shows full plan, waits for your go-ahead

### Dry Run (Safe Preview)
```
/ultra upgrade all dependencies to latest dry
```
→ Shows exactly what would happen, no changes

---

## Resource Optimization

Auto-detects and optimizes:

| Resource | Detection | Optimization |
|----------|-----------|--------------|
| CPU Cores | `sysctl -n hw.ncpu` | Parallel workers = 75% cores |
| Memory | `sysctl -n hw.memsize` | Memory-aware task scheduling |
| Disk | `df -h .` | Warns if <1GB available |
| Git State | `git status` | Stashes if dirty, auto-restores |

---

## Failure Recovery

```
[ULTRA] ⚠️ Task 2.1 FAILED
Error: Type error in src/api.ts:42

Auto-recovery options:
1. [fix] - Auto-fix the type error
2. [retry] - Retry the task
3. [skip] - Skip and continue (may cause issues)
4. [abort] - Stop and rollback

[ULTRA] attempting auto-fix...
├── Analyzing error...
├── Applying fix...
└── Retrying task...

[ULTRA] Task 2.1 ✓ (auto-recovered)
```

---

## Power Features

1. **AUTO-DECOMPOSE** - Breaks any request into optimal parallel tasks
2. **RESOURCE-AWARE** - Uses your hardware efficiently
3. **SELF-HEALING** - Auto-recovers from common failures
4. **VERIFIED** - Runs quality gates automatically
5. **SHIP-READY** - Generates commit, just approve

---

## The One Command

```
/ultra <what you want>
```

That's it. The AI handles:
- Analysis
- Planning
- Decomposition
- Parallel execution
- Error recovery
- Verification
- Commit preparation

You just say what you want. ULTRA delivers.

---

**EXAMPLES:**

```
/ultra add dark mode to the entire app
/ultra fix all TypeScript errors
/ultra implement real-time notifications with WebSockets
/ultra optimize bundle size under 100KB
/ultra add comprehensive tests for auth module
/ultra convert all class components to hooks
/ultra set up CI/CD pipeline with GitHub Actions
```

**ONE COMMAND. FULL POWER.**
