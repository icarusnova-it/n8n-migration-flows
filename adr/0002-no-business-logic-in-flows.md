# ADR-0002: No Business Logic in Flows

## Status

**Accepted** - This decision ensures business logic remains in services, not in n8n flows.

## Context

n8n flows can contain logic, but we need to decide where business logic should live. Options:
- Business logic in flows (n8n)
- Business logic in services (code)
- Hybrid approach

Business logic in flows creates:
- Testing challenges
- Version control limitations
- Maintenance difficulties
- Reusability issues

## Decision

**All business logic must be externalized to services. n8n flows should contain only orchestration logic.**

Flows will:
1. **Orchestrate** workflow steps
2. **Route** data between systems
3. **Handle errors** and retries
4. **Call services** for business logic
5. **Coordinate** multi-step processes

Flows will NOT:
- Implement business rules
- Perform data transformations (beyond simple mapping)
- Execute calculations
- Validate business data (beyond basic format checks)

## Consequences

### Positive

✅ **Testability**: Business logic in code is testable  
✅ **Reusability**: Services can be reused across flows  
✅ **Maintainability**: Logic changes don't require flow changes  
✅ **Version Control**: Code has better version control  
✅ **Code Review**: Logic changes go through code review  
✅ **Performance**: Services can be optimized independently  

### Negative

❌ **More Services**: Need to create services for logic  
❌ **More Calls**: Flows make more service calls  
❌ **Latency**: Additional network calls add latency  

### Mitigation

- Create reusable services
- Batch operations where possible
- Use efficient service design
- Cache where appropriate

## Implementation

### Flow Pattern

**Pattern:**
```
Flow → Service (Business Logic) → Flow → Service → Flow
```

**Example:**
```
1. Flow: Extract customer data
2. Service: Validate customer data (business logic)
3. Flow: Transform customer data (simple mapping)
4. Service: Calculate customer metrics (business logic)
5. Flow: Load customer data
```

### Service Responsibilities

**Services Handle:**
- Business rule validation
- Data transformations
- Calculations
- Complex validations
- Business logic

**Flows Handle:**
- Workflow orchestration
- Error handling
- Retry logic
- Routing
- Coordination

## Related Decisions

- [ADR-0001: Why n8n as Orchestrator](./0001-why-n8n-as-orchestrator.md)
- [Flow Design Principles](../docs/flow-design-principles.md)

## References

- [Orchestration Vision](../docs/orchestration-vision.md)
- [When to Use n8n](../docs/when-to-use-n8n.md)

---

**Date**: 2024  
**Deciders**: Icarus Nova Architecture Team  
**Status**: Accepted
