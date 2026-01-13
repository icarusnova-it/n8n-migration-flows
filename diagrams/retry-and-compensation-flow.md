# Retry and Compensation Flow

> **Icarus Nova** | Error handling with retries and compensation.

## Retry and Compensation Flow Diagram

```mermaid
sequenceDiagram
    participant n8n
    participant Service1
    participant Service2
    participant Service3
    participant DLQ
    
    Note over n8n,DLQ: Retry and Compensation Flow
    
    n8n->>Service1: Step 1: Operation A
    Service1-->>n8n: Success
    
    n8n->>Service2: Step 2: Operation B
    Service2-->>n8n: Transient Error
    
    Note over n8n,Service2: Retry Logic
    n8n->>n8n: Wait (Exponential Backoff)
    n8n->>Service2: Retry Operation B (Attempt 2)
    Service2-->>n8n: Transient Error
    
    n8n->>n8n: Wait (Exponential Backoff)
    n8n->>Service2: Retry Operation B (Attempt 3)
    Service2-->>n8n: Success
    
    n8n->>Service3: Step 3: Operation C
    Service3-->>n8n: Permanent Error
    
    Note over n8n,Service1: Compensation
    n8n->>Service1: Compensate Operation A
    Service1-->>n8n: Compensation Success
    
    Note over n8n,DLQ: Dead Letter Queue
    n8n->>DLQ: Send to Dead Letter Queue
    n8n->>n8n: Log Error
```

## Retry Strategy

### Exponential Backoff

**Pattern:**
- Attempt 1: Immediate
- Attempt 2: Wait 1s
- Attempt 3: Wait 2s
- Attempt 4: Wait 4s
- Attempt 5: Wait 8s (capped)

### Retry Configuration

**Parameters:**
- Max retries: 3-5
- Initial delay: 1s
- Max delay: 60s
- Backoff multiplier: 2

## Compensation Strategy

### Compensation Pattern

**When to Compensate:**
- Permanent error after partial success
- Cannot complete workflow
- Need to rollback changes

**Compensation Steps:**
1. Identify completed operations
2. Execute compensation for each
3. Verify compensation success
4. Log compensation actions

## Dead Letter Queue

### Dead Letter Queue Pattern

**Purpose:**
- Store failed workflows
- Enable manual review
- Prevent data loss
- Support reprocessing

**Contents:**
- Original input
- Error details
- Retry attempts
- Compensation status

## Related Documents

- [Error Handling Strategy](../docs/error-handling-strategy.md)
- [Failure Isolation](./failure-isolation.md)

---

**Last Updated:** 2024  
**Maintained by:** Icarus Nova Architecture Team  
**Version:** 1.0
