---
name: opencode-lint
description: Automated linting/fixing for code quality, style, and best practices
---

# OpenCode Lint Skill

## What I Do
- Run automated linting on codebases
- Identify code quality issues
- Auto-fix common linting problems
- Enforce consistent coding style
- Detect potential bugs and anti-patterns
- Suggest best practices

## When to Use
Use when:
- User asks to lint code
- Code style issues are mentioned
- Auto-fix suggestions are needed
- Best practices enforcement is requested
- Code quality check is needed

## Supported Linters

### JavaScript/TypeScript
- **ESLint**: Standard JS/TS linting
- **Prettier**: Code formatting
- **TSLint**: Legacy TS linting (if present)

### Python
- **Pylint**: Python code analysis
- **Flake8**: Style guide enforcement
- **Black**: Code formatting
- **Mypy**: Type checking
- **Ruff**: Fast Python linter

### Go
- **golint**: Go linting
- **gofmt**: Go formatting
- **staticcheck**: Static analysis

### Rust
- **clippy**: Rust linter
- **rustfmt**: Rust formatting

### Java
- **Checkstyle**: Java style checking
- **PMD**: Code quality analysis
- **SpotBugs**: Bug detection

### Ruby
- **RuboCop**: Ruby style guide
- **StandardRB**: Opinionated Ruby linting

## Lint Categories

### 1. Code Style
- Indentation and spacing
- Naming conventions
- Line length limits
- Trailing whitespace
- Quote style consistency
- Semicolons (JS/TS)

### 2. Code Quality
- Unused variables/imports
- Dead code detection
- Complex logic simplification
- Function length limits
- Nesting depth limits
- Code duplication

### 3. Best Practices
- Modern syntax usage
- Deprecated APIs
- Performance anti-patterns
- Security issues
- Error handling patterns
- Async/await best practices

### 4. Type Safety
- Type annotations
- Any type usage
- Type inference issues
- Interface compliance
- Generic constraints

## Auto-Fix Capabilities

### Safe Auto-Fixes
- Import organization
- Trailing whitespace removal
- Quote style normalization
- Semicolon insertion/removal
- Variable renaming
- Template literal conversion

### Suggested Fixes
- Unused code removal
- Simplification suggestions
- Modern syntax upgrades
- Error handling improvements
- Performance optimizations

## Linting Workflow

### 1. Detection
```
1. Identify project language
2. Find linter configuration
3. Run linter on target files
4. Parse linter output
5. Categorize issues by severity
```

### 2. Analysis
```
1. Group similar issues
2. Identify patterns
3. Prioritize critical issues
4. Check for auto-fix availability
5. Estimate fix complexity
```

### 3. Fixing
```
1. Apply safe auto-fixes
2. Suggest manual fixes
3. Generate fix commands
4. Verify fixes don't break code
5. Re-lint to confirm resolution
```

## Configuration Files

### ESLint (.eslintrc.json)
```json
{
  "extends": ["eslint:recommended"],
  "rules": {
    "no-console": "warn",
    "no-unused-vars": "error"
  }
}
```

### Prettier (.prettierrc)
```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2
}
```

### Pylint (.pylintrc)
```ini
[MESSAGES CONTROL]
disable=C0111,W0212

[BASIC]
max-line-length=120
```

### Flake8 (setup.cfg)
```ini
[flake8]
max-line-length = 120
ignore = E203, W503
```

## Common Issues and Fixes

### Unused Variables
```javascript
// ❌ Before
const unused = 5;
return 10;

// ✅ After
return 10;
```

### Console Logging
```javascript
// ❌ Before
console.log('Debug info');

// ✅ After
logger.info('Debug info');
```

### Missing Type Annotation
```typescript
// ❌ Before
function add(a, b) {
  return a + b;
}

// ✅ After
function add(a: number, b: number): number {
  return a + b;
}
```

## Integration with Tools

### Pre-commit Hooks
```bash
# husky + lint-staged
{
  "src/**/*.ts": ["eslint --fix", "prettier --write"]
}
```

### CI/CD Pipelines
```yaml
- name: Lint
  run: npm run lint
```

### IDE Integration
- VS Code: ESLint extension
- JetBrains: Code inspection
- Vim/Neovim:ALE plugin

## Safety Pattern

1. **Dry-run first** - Show issues before fixing
2. **Backup files** - Keep originals before auto-fix
3. **Incremental fixes** - Fix one category at a time
4. **Verify changes** - Run tests after fixes
5. **Version control** - Commit fixes separately
6. **Review suggestions** - Don't auto-fix everything blindly

## Example Prompts

```
"Lint the entire codebase"
"Fix ESLint errors in src/"
"Auto-format with Prettier"
"Check for unused code"
"Run Python linting with Pylint"
"Apply auto-fixes for TypeScript"
```

## Output Format

```
## Linting Results

### Critical Issues 🔴
- `file.ts:10` - Unused variable 'x'
- `api.js:45` - Missing error handling

### Warnings 🟡
- `utils.ts:23` - Line too long (120 > 100)
- `main.py:56` - Missing docstring

### Style Issues 📝
- `component.jsx:8` - Inconsistent quotes
- `service.go:34` - Trailing whitespace

### Auto-Fixes Applied ✅
- 12 import statements organized
- 8 trailing whitespace removed
- 5 quote styles normalized

### Manual Fixes Required ⚠️
1. Refactor complex function (file.ts:67)
2. Add type annotations (api.ts:12)
```
