---
name: performance-review
description: "Review code for performance issues. Use when reviewing for performance, efficiency, hot paths, N+1 queries, memory leaks, or scalability problems."
---

# Performance Review

Identify performance issues in the code. Check every item below that applies.

## Checklist

### Data Access
- N+1 query patterns (loop issuing one query per iteration)
- Missing indexes suggested by query patterns
- Loading entire tables/collections when a subset suffices
- Unbounded queries without pagination or limits
- Redundant queries for data already available

### Memory
- Unbounded collections growing without limits
- Large object allocations in hot paths
- Missing disposal of unmanaged resources (streams, connections, handles)
- String concatenation in loops (vs. StringBuilder/equivalent)
- Caching without eviction policy

### Concurrency
- Blocking calls on async paths (`.Result`, `.Wait()`, `Task.Run` wrapping sync)
- Lock contention on hot paths
- Missing cancellation token propagation
- Thread-unsafe shared state without synchronization
- Async void except event handlers

### Algorithmic
- O(n²) or worse where O(n log n) or O(n) is achievable
- Linear search where a hash lookup fits
- Repeated computation of the same value (missing memoization)
- Unnecessary sorting or full scans

### I/O & Network
- Synchronous I/O on request threads
- Missing connection pooling or reuse
- Unbatched network calls that could be batched
- Missing timeouts on external calls
- Large payloads without compression or streaming

## Output

For each finding:
- State the performance impact (latency, memory, throughput, scalability)
- Show the problematic code
- Estimate severity: how bad is it under realistic load?
- Provide the fix with corrected code or approach
