# Idempotency Patterns

> **Icarus Nova** | Patterns for ensuring idempotent n8n workflows.

## Overview

This document describes patterns for ensuring n8n workflows are idempotent, enabling safe retries and preventing duplicate processing.

## What is Idempotency?

### Definition

**Idempotency**: An operation is idempotent if performing it multiple times has the same effect as performing it once.

**Example:**
- ✅ Idempotent: Setting a value to "X" (always results in "X")
- ❌ Not Idempotent: Incrementing a counter (results in different value each time)

### Why Idempotency Matters

**Benefits:**
- Safe to retry workflows
- Resilience to failures
- No duplicate processing
- Predictable behavior
- Better error recovery

## Idempotency Patterns

### 1. Idempotency Keys

**Pattern:**
- Generate unique idempotency key for each workflow execution
- Include idempotency key in all service calls
- Services check idempotency key before processing
- Services return same result for same key

**Implementation:**
```
1. Generate idempotency key (UUID or hash)
2. Include key in workflow input
3. Pass key to all service calls
4. Services check key before processing
5. Services return cached result if key seen before
```

**Example:**
```json
{
  "idempotencyKey": "550e8400-e29b-41d4-a716-446655440000",
  "customerId": "12345",
  "action": "migrate"
}
```

### 2. Duplicate Detection

**Pattern:**
- Check if work item already processed
- Use unique identifiers to detect duplicates
- Skip processing if duplicate detected
- Return existing result if duplicate

**Implementation:**
```
1. Extract unique identifier from input
2. Check if identifier already processed
3. If processed, return existing result
4. If not processed, process and mark as processed
```

**Example:**
- Check if customer ID already migrated
- If migrated, return migration result
- If not migrated, perform migration

### 3. Safe Reprocessing

**Pattern:**
- Workflows can be safely reprocessed
- Reprocessing produces same result
- No side effects from reprocessing
- Idempotent operations only

**Implementation:**
- Use idempotent service operations
- Check state before processing
- Skip if already in desired state
- Return consistent results

### 4. State-Based Idempotency

**Pattern:**
- Check current state before processing
- Only process if state allows
- Skip if already in target state
- Return current state if already correct

**Example:**
```
1. Check customer migration status
2. If "migrated", return success
3. If "pending", perform migration
4. If "failed", retry migration
```

## Implementation Strategies

### Service-Level Idempotency

**Approach:**
- Services implement idempotency
- Services check idempotency keys
- Services return cached results
- Services skip duplicate operations

**Benefits:**
- Idempotency at service level
- Works across all callers
- Centralized idempotency logic

### Flow-Level Idempotency

**Approach:**
- Flows check for duplicates
- Flows generate idempotency keys
- Flows handle idempotency checks
- Flows coordinate idempotent operations

**Benefits:**
- Idempotency in orchestration layer
- Works with non-idempotent services
- Flow-level control

### Hybrid Approach

**Approach:**
- Combine service and flow-level idempotency
- Flows generate keys, services check keys
- Multiple layers of idempotency
- Defense in depth

## Idempotency Keys

### Key Generation

**Strategies:**
- **UUID**: Unique identifier for each execution
- **Hash**: Hash of input data
- **Composite**: Combination of identifiers
- **Time-based**: Include timestamp (with caution)

**Recommendation:**
- Use UUID for unique executions
- Use hash for deterministic keys
- Include context in key

### Key Format

**Structure:**
```
{workflow-name}-{unique-id}-{timestamp}
```

**Example:**
```
migrate-customer-550e8400-e29b-41d4-a716-446655440000-20240115
```

### Key Storage

**Storage:**
- Store keys with results
- TTL for key storage
- Cleanup old keys
- Query keys for duplicate detection

## Duplicate Detection

### Detection Strategies

**By Unique Identifier:**
- Use natural unique identifiers
- Check if identifier processed
- Example: Customer ID, Order ID

**By Content Hash:**
- Hash input content
- Check if hash processed
- Example: Hash of customer data

**By Composite Key:**
- Combine multiple identifiers
- Check composite key
- Example: Customer ID + Migration Date

### Detection Implementation

**Pattern:**
```
1. Extract unique identifier
2. Query processing history
3. If found, return existing result
4. If not found, process and store result
```

## Best Practices

### Idempotency Best Practices

1. **Always Use Idempotency Keys**: Generate keys for all workflows
2. **Check for Duplicates**: Detect duplicates before processing
3. **Idempotent Services**: Use idempotent service operations
4. **State Checks**: Check state before processing
5. **Consistent Results**: Return same result for same input
6. **Key Management**: Manage idempotency keys properly

### Anti-Patterns

**Avoid:**
- ❌ No idempotency keys
- ❌ Not checking for duplicates
- ❌ Non-idempotent operations
- ❌ Inconsistent results
- ❌ No key management

## Related Documents

- [Flow Design Principles](./flow-design-principles.md)
- [Error Handling Strategy](./error-handling-strategy.md)
- [ADR-0003: Idempotent Flows Only](../adr/0003-idempotent-flows-only.md)

---

**Last Updated:** 2024  
**Maintained by:** Icarus Nova Architecture Team  
**Version:** 1.0
