---
name: code-refactor
description: Automated code refactoring and quality improvement skill that analyzes code structure and suggests or applies refactoring patterns.
---

# Code Refactor Skill

## Purpose
Automated code refactoring and quality improvement skill that analyzes code structure and suggests or applies refactoring patterns.

## Configuration
- **max_complexity**: Maximum allowed cyclomatic complexity (default: 10)
- **max_function_length**: Maximum lines per function (default: 50)
- **max_class_length**: Maximum lines per class (default: 500)
- **min_coverage**: Minimum code coverage percentage (default: 80%)
- **similarity_threshold**: Minimum similarity threshold for duplicate detection (default: 85%)
- **dry_run_by_default**: Whether to perform dry-runs by default (default: false)
- **auto_commit**: Whether to auto-commit refactored code (default: false)

## Capabilities
1. **Complexity Analysis**
   - Calculate cyclomatic complexity
   - Identify functions/methods exceeding complexity threshold
   - Suggest decomposition strategies

2. **Code Smell Detection**
   - Long parameter lists
   - Duplicate code blocks
   - Dead code identification
   - God object anti-patterns

3. **Refactoring Patterns**
   - Extract method/class
   - Replace conditionals with polymorphism
   - Introduce design patterns where appropriate
   - Reduce nesting depth

4. **Duplicate Detection**
   - Find similar code blocks
   - Suggest extraction to shared utilities
   - Calculate similarity scores

5. **Quality Metrics**
   - Code coverage analysis
   - Maintainability index calculation
   - Technical debt estimation

## Usage

### Simple Refactoring
```
Refactor this function to reduce complexity and improve readability
```

### With Specific Parameters
```
Refactor the payment processing module with:
- max_complexity: 15
- min_coverage: 70%
- Enable dry-run mode
```

### Batch Refactoring
```
Refactor the entire services directory, focusing on:
1. Functions exceeding 50 lines
2. Classes with complexity > 12
3. Duplicate code patterns above 85% similarity
```

## Safety Rules
- Always run `dry_run` first for large codebases
- Preserve existing public APIs
- Maintain test compatibility
- Do not change external interfaces without explicit request
- Create rollback points before major refactoring

## Best Practices
1. Incremental refactoring over massive rewrites
2. Ensure tests pass after each refactoring step
3. Document changes made and rationale
4. Maintain backward compatibility when possible
5. Focus on code smell elimination, not style changes

## Integration
Works seamlessly with:
- LSP tools for code analysis
- Test runners for validation
- Git operations for version control
- Memory MCP for context persistence

## Output
- **Detailed analysis**: Complexity metrics, smell detection results
- **Refactoring plan**: Step-by-step recommendations
- **Applied changes**: Diff view of modifications
- **Test results**: Validation of refactored code
- **Metrics report**: Before/after comparison
