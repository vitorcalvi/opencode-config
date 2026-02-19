---
name: code-review
description: Perform thorough code reviews focusing on code quality, security, performance, maintainability, and best practices without making direct changes
---

# Code Review Skill

## What I Do
- Analyze code for quality and best practices
- Identify potential bugs and edge cases
- Evaluate performance implications
- Flag security concerns
- Suggest improvements with examples
- Provide constructive, actionable feedback

## When to Use
Use when:
- User asks to review code
- Pull request analysis is needed
- Code quality assessment is requested
- Security audit is requested
- Performance review is needed

## Review Categories

### 1. Code Quality
- [ ] Naming conventions (variables, functions, classes)
- [ ] Function length and complexity
- [ ] Code duplication
- [ ] Separation of concerns
- [ ] SOLID principles adherence

### 2. Error Handling
- [ ] Exception handling patterns
- [ ] Error propagation strategy
- [ ] Logging practices
- [ ] User-friendly error messages

### 3. Security
- [ ] Input validation and sanitization
- [ ] Authentication/authorization checks
- [ ] SQL injection prevention
- [ ] XSS protection
- [ ] Sensitive data exposure
- [ ] Dependency vulnerabilities

### 4. Performance
- [ ] Database query optimization
- [ ] Unnecessary computations
- [ ] Memory usage patterns
- [ ] Network call efficiency
- [ ] Caching opportunities

### 5. Testing
- [ ] Test coverage gaps
- [ ] Edge case coverage
- [ ] Mock/fixture quality
- [ ] Test isolation

## Review Format

```
## Code Review Summary

### Overall Assessment
[Positive/Negative with rationale]

### Critical Issues 🔴
1. **[Issue Name]**
   - File: `path/to/file`
   - Line: N
   - Problem: [Description]
   - Impact: [Severity]
   - Suggestion: [Fix with code example]

### Warnings 🟡
[Same format as above]

### Suggestions 💡
[Same format as above]

### Praise ✅
- [What was done well]

## Action Items
- [ ] Fix critical issue 1
- [ ] Address warning 2
- [ ] Consider suggestion 3
```

## Safety Pattern
1. **READ-ONLY analysis** - Never modify code
2. **Evidence-based** - Quote actual code snippets
3. **Constructive tone** - Focus on improvement
4. **Prioritized feedback** - Critical first
5. **Provide examples** - Show how to fix, don't just criticize

## Example Prompts
```
"Review the authentication module"
"Code review of PR changes in src/auth/"
"Check for security issues in user input handling"
"Analyze performance of the database queries"
"Do a quick review of the error handling"
```

## Performance Best Practices (Built-In)

### Resource Management
1. **Memory Efficiency**
   - Process code in chunks for large codebases
   - Use streaming APIs when available
   - Clear unused references promptly
   - Limit concurrent operations based on available memory

2. **CPU Optimization**
   - Use efficient algorithms (O(n) where possible)
   - Leverage native tools (grep, codesearch) over manual iteration
   - Parallelize independent code reviews when possible
   - Use appropriate model for task complexity

3. **I/O Optimization**
   - Batch file operations
   - Use glob patterns instead of individual file reads
   - Cache frequently accessed code

### Execution Patterns
1. **Timeout Handling**
   - Set appropriate timeouts based on codebase size (default: 30000ms)
   - Implement progressive timeouts for large codebases
   - Log timeout events for debugging

2. **Error Recovery**
   - Implement automatic retry with exponential backoff
   - Graceful degradation on partial failures
   - Clear state between retries

3. **Caching Strategy**
   - Cache analyzed code patterns
   - Implement cache invalidation on file changes
   - Use Memory MCP for cross-session caching

## Distributed Orchestration (10-Worker Farm)

### Architecture
- **10 OpenCode workers** on Alpine Linux (ports 4001-4010)
- **Branch-per-worker**: Each worker operates on isolated branch
- **Shared workspace**: `/home/ubuntu/workspace` for coordination
- **Dependency gates**: `.ok_*` files for task coordination

### Resource Limits (Per Worker)
- **CPU**: 1.5 cores maximum
- **RAM**: 512MB maximum
- **Timeout**: 120 seconds per task
- **Network**: Shared dokploy-network

### Orchestration Patterns
1. **Fan-Out DAG**
   ```
   Tier 1 (setup):     W1 — git clone, create branches
   Tier 2 (parallel):  W2-W9 — 8 parallel code-review workers
   Tier 3 (merge):     W10 — merge branches, verify, test
   ```

2. **Health Checks**
   - HTTP 401/200 = healthy (server running)
   - HTTP 000 = unhealthy (container down)
   - Retry once after 10 seconds
   - Redistribute task if still unhealthy

3. **Error Recovery**
   - Timeout: Redistribute to available worker
   - Failure: Create `.err_*` gate, log error
   - Conflict: Log conflicting files, suggest resolution

## Integration Patterns

### Agent Collaboration

#### When to use sisyphus
- Task requires orchestration and planning
- Multi-step workflows with dependencies
- Architecture decisions and tradeoff analysis

#### When to use opencoder
- Direct implementation tasks
- Code generation and refactoring
- File writes and edits

#### When to use oracle
- Research and documentation lookups
- Best practices and style guides
- External code exploration

#### When to use speed_reader
- Quick codebase exploration
- Summarization and analysis
- High-volume file scanning

#### When to use fast_coder_grok
- Large codebase analysis (>100 files)
- Complex multi-step implementations
- Architectural changes requiring reasoning loops

### MCP Integration

#### Qdrant (Vector Search)
- Use for: Semantic code search, pattern matching, embeddings-based lookup
- Configuration: QDRANT_URL=http://192.168.20.10:6333

#### Memory (Persistent State)
- Use for: Storing review results, cross-session context, historical data
- Configuration: MEMORY_FILE_PATH=/Users/vitorcalvi/.config/opencode/memory.json

#### Filesystem (File Operations)
- Use for: File reads, writes, directory traversal
- Configuration: npx -y @modelcontextprotocol/server-filesystem /

#### Git (Version Control)
- Use for: Git operations, history analysis, blame, diff generation
