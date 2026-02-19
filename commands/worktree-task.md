---
description: Create isolated git worktree ./worktrees/<name> (no push to remote)
subtask: true
---

# Worktree Task - Isolated Git Worktree Creation

Create an isolated git worktree for parallel development without affecting the main repository.

## Input Arguments

The worktree name/task: **$ARGUMENTS**

## Usage

```
/worktree-task <name> [base_branch]
```

- `name` (required): Name for the worktree directory and branch
- `base_branch` (optional): Base branch to create from (default: current branch)

## Protocol

### Step 1: Pre-flight Checks

```bash
# Verify we're in a git repository
git rev-parse --is-inside-work-tree || { echo "ERROR: Not a git repository"; exit 1; }

# Get current branch
CURRENT_BRANCH=$(git branch --show-current)

# Ensure working directory is clean
if ! git diff-index --quiet HEAD --; then
  echo "WARNING: Working directory has uncommitted changes"
  echo "Consider stashing: git stash"
fi
```

### Step 2: Create Worktree Directory

```bash
# Create worktrees directory if it doesn't exist
mkdir -p ./worktrees

# Define paths
WORKTREE_NAME="$ARGUMENTS"
WORKTREE_PATH="./worktrees/${WORKTREE_NAME}"
BASE_BRANCH="${2:-$(git branch --show-current)}"
BRANCH_NAME="task/${WORKTREE_NAME}"
```

### Step 3: Create Worktree

```bash
# Create the worktree with a new branch
git worktree add -b "${BRANCH_NAME}" "${WORKTREE_PATH}" "${BASE_BRANCH}"

echo "✓ Worktree created at: ${WORKTREE_PATH}"
echo "✓ Branch: ${BRANCH_NAME}"
echo "✓ Based on: ${BASE_BRANCH}"
```

### Step 4: Output Instructions

```markdown
## Worktree Created Successfully

### Details
- **Path**: ./worktrees/${WORKTREE_NAME}
- **Branch**: task/${WORKTREE_NAME}
- **Base**: ${BASE_BRANCH}
- **Remote**: NOT pushed (local only)

### To switch to this worktree:
\`\`\`bash
cd ./worktrees/${WORKTREE_NAME}
\`\`\`

### To return to main repo:
\`\`\`bash
cd ../../
\`\`\`

### To remove when done:
\`\`\`bash
git worktree remove ./worktrees/${WORKTREE_NAME}
git branch -d task/${WORKTREE_NAME}
\`\`\`

### To push (when ready):
\`\`\`bash
cd ./worktrees/${WORKTREE_NAME}
git push -u origin task/${WORKTREE_NAME}
\`\`\`
```

## Important Rules

1. **NO PUSH**: Never push the worktree branch to remote automatically
2. **ISOLATED**: Worktree is completely isolated from main repo
3. **NAMING**: Use `task/` prefix for branch names
4. **CLEANUP**: Provide cleanup instructions for when work is done

## Example Output

```
/worktree-task feature-auth main
```

Creates:
- Directory: `./worktrees/feature-auth`
- Branch: `task/feature-auth` based on `main`
- No remote push

---

**NOTE**: This command creates local-only worktrees for isolated development.
