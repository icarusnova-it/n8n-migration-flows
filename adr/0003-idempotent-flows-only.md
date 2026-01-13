# ADR-0003: Idempotent Flows Only

## Status

**Accepted** - This decision ensures all n8n flows are idempotent for safe retries.

## Context

Workflows can fail and need to be retried. Non-idempotent workflows can cause:
- Duplicate processing
- Data inconsistencies
- Incorrect results
- System corruption

We need to ensure workflows can be safely retried without side effects.

## Decision

**All n8n flows must be idempotent. Non-idempotent flows are not allowed in production.**

Flows must:
1. **Generate idempotency keys** for each execution
2. **Check for duplicates** before processing
3. **Use idempotent service operations**
4. **Return consistent results** for same input
5. **Support safe reprocessing**

## Consequences

### Positive

✅ **Safe Retries**: Workflows can be safely retried  
✅ **Resilience**: Better error recovery  
✅ **No Duplicates**: Prevents duplicate processing  
✅ **Predictable**: Consistent behavior  
✅ **Reliability**: More reliable workflows  

### Negative

❌ **Additional Complexity**: Need to implement idempotency  
❌ **Performance Overhead**: Idempotency checks add overhead  
❌ **Storage Requirements**: Need to store idempotency keys  

### Mitigation

- Efficient idempotency checks
- Time-limited key storage
- Optimize key lookups
- Use efficient data structures

## Implementation

### Idempotency Keys

**Generation:**
- Generate unique key per execution
- Include in all service calls
- Store with execution results
- Use for duplicate detection

**Format:**
```
{workflow-name}-{timestamp}-{random-id}
```

### Duplicate Detection

**Pattern:**
1. Generate idempotency key
2. Check if key already processed
3. If processed, return existing result
4. If not processed, process and store result

### Service Idempotency

**Requirements:**
- Services must be idempotent
- Services check idempotency keys
- Services return cached results
- Services skip duplicate operations

## Related Decisions

- [Idempotency Patterns](../docs/idempotency-patterns.md)
- [Error Handling Strategy](../docs/error-handling-strategy.md)

## References

- [Flow Design Principles](../docs/flow-design-principles.md)

---

**Date**: 2026  
**Deciders**: Icarus Nova Architecture Team  
**Status**: Accepted
