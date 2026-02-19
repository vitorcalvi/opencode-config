---
name: api-docs
description: Generate comprehensive API documentation from code, create OpenAPI/Swagger specs, write usage examples, and maintain documentation consistency
---

# API Documentation Skill

## What I Do
- Generate API documentation from code annotations
- Create OpenAPI/Swagger specifications
- Write usage examples and tutorials
- Maintain documentation consistency
- Document error responses
- Generate SDK examples in multiple languages

## When to Use
Use when:
- User asks to document APIs
- New endpoints need documentation
- OpenAPI spec generation is needed
- Documentation review is requested
- README or guides need creation

## Documentation Components

### 1. Endpoint Documentation
```markdown
### POST /api/users

Create a new user.

**Authentication**: Required (Bearer token)

**Request Body**:
```json
{
  "name": "string (required)",
  "email": "string (required)",
  "role": "string (optional, enum: [user, admin])"
}
```

**Response (201)**:
```json
{
  "id": "uuid",
  "name": "string",
  "email": "string",
  "role": "string",
  "created_at": "datetime"
}
```

**Errors**:
- 400: Invalid input
- 401: Unauthorized
- 409: Email already exists
```

### 2. OpenAPI Specification
```yaml
openapi: 3.0.3
info:
  title: My API
  version: 1.0.0
paths:
  /users:
    get:
      summary: List users
      parameters:
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
      responses:
        '200':
          description: Successful response
```

### 3. Code Examples
**Python**:
```python
import requests

response = requests.get(
    'https://api.example.com/users',
    headers={'Authorization': 'Bearer token'},
    params={'limit': 10}
)
users = response.json()
```

**JavaScript**:
```javascript
const response = await fetch('https://api.example.com/users', {
  headers: { 'Authorization': 'Bearer token' }
});
const users = await response.json();
```

## Documentation Sections

### 1. Overview
- API purpose and scope
- Base URL and versioning
- Authentication methods
- Rate limiting

### 2. Authentication
- How to obtain tokens
- Token refresh flow
- Scopes and permissions

### 3. Endpoints
- Grouped by resource
- Consistent structure
- Clear examples

### 4. Error Handling
- Standard error format
- HTTP status codes
- Error codes and messages

### 5. SDKs & Libraries
- Official client libraries
- Community contributions
- Installation and usage

## Documentation Generation Workflow
1. **Scan codebase** for API definitions
2. **Extract endpoints** from code/routes
3. **Gather schemas** from models/validators
4. **Generate OpenAPI** specification
5. **Create documentation** from spec
6. **Add examples** and guides
7. **Validate** consistency and completeness

## Safety Pattern
1. **Read-only generation** - Don't modify source
2. **Verify examples** - Test code snippets
3. **Preserve formatting** - Match project style
4. **Draft-first** - Show docs before saving
5. **Version control** - Track documentation changes

## Example Prompts
```
"Generate API documentation for the user endpoints"
"Create an OpenAPI spec from the routes"
"Document the authentication API with examples"
"Write documentation for the payment gateway"
"Update the API docs to include new endpoints"
```

## Performance Best Practices (Built-In)

### Resource Management
1. **Memory Efficiency**
   - Process API specs in chunks for large APIs
   - Use streaming for large documentation generation
   - Clear intermediate results promptly
   - Limit concurrent operations based on available memory

2. **CPU Optimization**
   - Use efficient algorithms for spec generation (O(n) where possible)
   - Leverage native tools (codesearch, grep) over manual iteration
   - Parallelize independent endpoint documentation when possible
   - Use appropriate model for task complexity

3. **I/O Optimization**
   - Batch file operations (read specs, write docs)
   - Use glob patterns instead of individual file reads
   - Cache frequently accessed specs and models

### Execution Patterns
1. **Timeout Handling**
   - Set appropriate timeouts based on API size (default: 30000ms)
   - Implement progressive timeouts for large API generation
   - Log timeout events for debugging

2. **Error Recovery**
   - Implement automatic retry with exponential backoff
   - Graceful degradation on partial documentation failures
   - Clear state between retries

3. **Caching Strategy**
   - Cache parsed API specs
   - Implement cache invalidation on file changes
   - Use Memory MCP for cross-session caching of generated docs

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
   Tier 2 (parallel):  W2-W9 — 8 parallel code-gen workers
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
- API queries and best practices
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
- Use for: Semantic code search, similarity matching, embeddings-based lookup
- Configuration: QDRANT_URL=http://192.168.20.10:6333

#### Memory (Persistent State)
- Use for: Storing skill results, cross-session context, historical data
- Configuration: MEMORY_FILE_PATH=/Users/vitorcalvi/.config/opencode/memory.json

#### Filesystem (File Operations)
- Use for: File reads, writes, directory traversal, file watching
- Configuration: npx -y @modelcontextprotocol/server-filesystem /

#### Git (Version Control)
- Use for: Git operations, history analysis, blame, diff generation
