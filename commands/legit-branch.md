---
description: Create filesystem snapshot ./branches/<name> (overlay|copy|rsync)
subtask: true
---

# Legit Branch - Filesystem Snapshot Creation

Create a filesystem-level snapshot for safe experimentation without git worktrees.

## Input Arguments

Snapshot name and options: **$ARGUMENTS**

## Usage

```
/legit-branch <name> [mode]
```

- `name` (required): Name for the snapshot directory
- `mode` (optional): Snapshot mode - `overlay` (default), `copy`, or `rsync`

## Snapshot Modes

| Mode | Speed | Space | Use Case |
|------|-------|-------|----------|
| `overlay` | Instant | Minimal | Quick experiments, filesystem supports overlayfs |
| `copy` | Medium | Full | Maximum compatibility, isolated copy |
| `rsync` | Fast | Full | Large codebases, incremental sync capable |

## Protocol

### Step 1: Detect Environment

```bash
# Determine OS for overlay support
OS=$(uname -s)
SUPPORTS_OVERLAY=false

if [ "$OS" = "Linux" ]; then
  # Check for overlayfs support
  if command -v mount.fuse.overlay &> /dev/null || [ -d /sys/fs/overlay ]; then
    SUPPORTS_OVERLAY=true
  fi
fi

echo "Platform: $OS"
echo "Overlay support: $SUPPORTS_OVERLAY"
```

### Step 2: Parse Arguments

```bash
SNAPSHOT_NAME="$1"
MODE="${2:-copy}"  # Default to copy for maximum compatibility

# Validate mode
case "$MODE" in
  overlay|copy|rsync) ;;
  *) 
    echo "ERROR: Invalid mode '$MODE'. Use: overlay, copy, or rsync"
    exit 1
    ;;
esac

# On macOS, overlay is not natively supported
if [ "$OS" = "Darwin" ] && [ "$MODE" = "overlay" ]; then
  echo "WARNING: overlay mode not supported on macOS, falling back to copy"
  MODE="copy"
fi
```

### Step 3: Create Snapshot

#### Mode: Copy (Default, most compatible)

```bash
# Create branches directory
mkdir -p ./branches

# Define paths
SOURCE_DIR="$(pwd)"
SNAPSHOT_PATH="./branches/${SNAPSHOT_NAME}"

# Create full copy (exclude common non-essential directories)
echo "Creating copy snapshot..."
rsync -a --exclude='.git' --exclude='node_modules' --exclude='.next' \
      --exclude='dist' --exclude='build' --exclude='.cache' \
      "${SOURCE_DIR}/" "${SNAPSHOT_PATH}/"

echo "✓ Copy snapshot created at: ${SNAPSHOT_PATH}"
```

#### Mode: Rsync (For large codebases)

```bash
# Create branches directory
mkdir -p ./branches

# Define paths
SNAPSHOT_PATH="./branches/${SNAPSHOT_NAME}"

# Use rsync with progress
echo "Creating rsync snapshot..."
rsync -av --progress \
      --exclude='.git' --exclude='node_modules' --exclude='.next' \
      --exclude='dist' --exclude='build' --exclude='.cache' \
      "./" "${SNAPSHOT_PATH}/"

echo "✓ Rsync snapshot created at: ${SNAPSHOT_PATH}"
```

#### Mode: Overlay (Linux only)

```bash
# Create branches directory
mkdir -p ./branches

# Define paths
SNAPSHOT_PATH="./branches/${SNAPSHOT_NAME}"
LOWER_DIR="$(pwd)"
UPPER_DIR="./branches/.overlay_${SNAPSHOT_NAME}/upper"
WORK_DIR="./branches/.overlay_${SNAPSHOT_NAME}/work"

# Create overlay directories
mkdir -p "${UPPER_DIR}" "${WORK_DIR}" "${SNAPSHOT_PATH}"

# Mount overlay
sudo mount -t overlay overlay \
  -o lowerdir="${LOWER_DIR}",upperdir="${UPPER_DIR}",workdir="${WORK_DIR}" \
  "${SNAPSHOT_PATH}"

echo "✓ Overlay snapshot created at: ${SNAPSHOT_PATH}"
echo "NOTE: Requires sudo for mount. Changes are isolated."
```

### Step 4: Output Instructions

```markdown
## Snapshot Created Successfully

### Details
- **Mode**: ${MODE}
- **Path**: ./branches/${SNAPSHOT_NAME}
- **Source**: ${SOURCE_DIR}

### To switch to snapshot:
\`\`\`bash
cd ./branches/${SNAPSHOT_NAME}
\`\`\`

### To return to main:
\`\`\`bash
cd ../../
\`\`\`

### To sync changes back (if desired):
\`\`\`bash
# From snapshot directory
rsync -av ./ ../../  # Be careful - this overwrites!
\`\`\`

### To remove snapshot:
\`\`\`bash
rm -rf ./branches/${SNAPSHOT_NAME}
# If overlay mode:
sudo umount ./branches/${SNAPSHOT_NAME}
rm -rf ./branches/.overlay_${SNAPSHOT_NAME}
\`\`\`
```

## Important Notes

1. **ISOLATION**: Changes in snapshot do NOT affect main directory
2. **GIT**: .git is excluded - snapshot shares git history
3. **DEPENDENCIES**: node_modules excluded - run `npm install` in snapshot if needed
4. **CLEANUP**: Manual cleanup required when done

## Examples

```bash
# Quick copy snapshot
/legit-branch experiment-ui

# Rsync for large project
/legit-branch large-refactor rsync

# Overlay on Linux (instant)
/legit-branch quick-test overlay
```

---

**NOTE**: Snapshots are filesystem-level, not git-level. Use for safe file experimentation.
