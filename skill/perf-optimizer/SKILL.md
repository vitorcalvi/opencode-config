---
name: perf-optimizer
description: Automated performance analysis and optimization skill that identifies bottlenecks and suggests improvements.
---

# Performance Optimizer Skill

## Purpose
Automated performance analysis and optimization skill that identifies bottlenecks and suggests improvements.

## Configuration
- **response_time_threshold**: Maximum acceptable response time in ms (default: 200)
- **memory_threshold**: Maximum acceptable memory usage in % (default: 80%)
- **cpu_threshold**: Maximum acceptable CPU usage in % (default: 70%)
- **slow_query_threshold**: Threshold for slow database queries in ms (default: 500)
- **auto_profile**: Whether to automatically profile performance (default: false)
- **profile_duration**: Duration of profiling sessions in seconds (default: 60)
- **baseline_file**: File to store performance baseline (default: .perf-baseline.json)

## Capabilities
1. **Response Time Analysis**
   - Measure API endpoint response times
   - Identify slow endpoints
   - Track response time percentiles (p50, p95, p99)

2. **Resource Usage Monitoring**
   - CPU utilization tracking
   - Memory usage analysis
   - I/O operation monitoring
   - Network throughput measurement

3. **Database Performance**
   - Slow query detection
   - Index analysis and suggestions
   - N+1 query identification
   - Connection pool optimization

4. **Frontend Performance**
   - Bundle size analysis
   - Largest contentful paint (LCP) tracking
   - Cumulative layout shift (CLS) monitoring
   - First input delay (FID) measurement

5. **Caching Strategy**
   - Cache hit/miss ratio analysis
   - Cache invalidation strategy review
   - CDN integration recommendations
   - Redis/Memcached optimization

6. **Concurrency Optimization**
   - Thread pool sizing
   - Async/await patterns
   - Parallel execution opportunities
   - Rate limiting analysis

## Usage

### Basic Performance Analysis
```
Analyze the performance of the main API endpoints
```

### With Specific Thresholds
```
Profile the payment service with:
- response_time_threshold: 500ms
- memory_threshold: 85%
- cpu_threshold: 75%
- Enable auto-profiling for 60 seconds
```

### Database Optimization
```
Optimize database queries for:
- Slow queries exceeding 200ms
- Missing indexes
- N+1 query patterns
- Connection pool configuration
```

### Frontend Optimization
```
Optimize frontend bundle performance:
- Analyze bundle size and split points
- Improve LCP, FID, CLS metrics
- Implement code splitting strategies
- Optimize asset loading
```

## Performance Profiling

### Automatic Profiling
```javascript
// Enable auto-profiling in config
{
  "auto_profile": true,
  "profile_duration": 60
}

// This will automatically:
// 1. Start profiling on service start
// 2. Collect metrics for configured duration
// 3. Generate performance report
// 4. Update baseline file
```

### Manual Profiling
```
Profile the checkout process for 2 minutes:
1. Capture CPU, memory, I/O metrics
2. Track database query performance
3. Monitor external API calls
4. Generate detailed report
```

## Baseline Comparison
```json
// .perf-baseline.json structure
{
  "endpoints": {
    "/api/users": {
      "p50": 120,
      "p95": 200,
      "p99": 350,
      "baseline_date": "2026-01-08"
    }
  },
  "resources": {
    "cpu": 45,
    "memory": 60
  }
}

// Comparisons will show deviations from baseline
```

## Optimization Patterns
1. **Database Optimizations**
   - Add appropriate indexes
   - Use query caching
   - Optimize JOIN operations
   - Implement connection pooling

2. **API Optimizations**
   - Implement response compression
   - Use CDN for static assets
   - Optimize serialization/deserialization
   - Implement rate limiting

3. **Frontend Optimizations**
   - Code splitting and lazy loading
   - Image optimization
   - Bundle size reduction
   - Critical CSS extraction

4. **Caching Strategies**
   - Implement HTTP caching headers
   - Use in-memory caching for hot data
   - Cache database query results
   - Implement cache invalidation

## Safety Rules
- Never optimize without baseline measurement first
- Always test optimizations in staging
- Roll back optimizations that degrade performance
- Monitor metrics after every optimization
- Document all performance experiments

## Best Practices
1. Measure before optimizing
2. Focus on high-impact, low-effort optimizations first
3. Establish performance budgets
4. Continuously monitor and iterate
5. Consider trade-offs (e.g., consistency vs performance)

## Integration
Works seamlessly with:
- APM tools (Application Performance Monitoring)
- Database profilers
- Lighthouse for web performance
- Monitoring MCP for real-time metrics
- Memory MCP for historical data

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
- Use for: Semantic code search, similarity matching, embeddings-based lookup
- Configuration: QDRANT_URL=http://192.168.20.10:6333

#### Memory (Persistent State)
- Use for: Storing performance results, cross-session context, historical data
- Configuration: MEMORY_FILE_PATH=/Users/vitorcalvi/.config/opencode/memory.json

#### Filesystem (File Operations)
- Use for: File reads, writes, directory traversal, file watching
- Configuration: npx -y @modelcontextprotocol/server-filesystem /

#### Git (Version Control)
- Use for: Git operations, history analysis, blame, diff generation
## Output
- **Performance Report**: Comprehensive metrics and analysis
- **Optimization Recommendations**: Prioritized list of improvements
- **Before/After Comparison**: Performance impact of changes
- **Baseline Updates**: Updated performance baselines
- **Alert Rules**: Automated alerting configuration
