# ADR-0001: Why n8n as Orchestrator

## Status

**Accepted** - This decision establishes n8n as the orchestration layer, not the core platform.

## Context

We need to decide on the orchestration approach for migrations and integrations. Options include:
- Custom code orchestration
- Workflow engines (n8n, Zapier, etc.)
- BPM platforms
- Schedulers (Cron, etc.)

We need an orchestration solution that:
- Enables rapid workflow development
- Provides visual workflow design
- Supports error handling and retries
- Offers good observability
- Is cost-effective

## Decision

**We will use n8n as the orchestration layer for migrations and integrations, not as a core business logic platform.**

n8n will:
1. **Orchestrate workflows** between systems
2. **Coordinate multi-step processes** with error handling
3. **Route data and events** between systems
4. **Provide visibility** into workflow execution
5. **Handle retries and errors** at the orchestration level

n8n will NOT:
- Implement business logic (logic in services)
- Process high volumes of data (use dedicated services)
- Replace API gateways (use API gateways)
- Replace schedulers for simple tasks (use schedulers)

## Consequences

### Positive

✅ **Rapid Development**: Visual workflows enable faster development  
✅ **Maintainability**: Visual workflows easier to understand and maintain  
✅ **Integration Library**: Extensive connector library reduces custom code  
✅ **Operational Features**: Built-in retry, error handling, logging  
✅ **Cost-Effective**: Open-source with reasonable enterprise features  
✅ **Flexibility**: Easy to modify workflows without code changes  

### Negative

❌ **Not for Business Logic**: Complex logic should be in services  
❌ **Performance Limitations**: Not for high-volume processing  
❌ **Testing Limitations**: Harder to unit test than code  
❌ **Vendor Lock-in**: Some dependency on n8n platform  

### Mitigation

- Business logic in services, not flows
- Use n8n for orchestration, services for processing
- Focus on integration testing
- Keep flows simple and maintainable

## Implementation

### Orchestration Pattern

**Architecture:**
```
Business Applications
    ↓
Business Logic Services (Core Logic)
    ↓
n8n Orchestration Layer (Coordination)
    ↓
External Systems (APIs, Databases, etc.)
```

### Flow Design

**Principles:**
- Flows orchestrate, services execute
- Minimal logic in flows
- Externalize business logic
- Focus on coordination

## Related Decisions

- [ADR-0002: No Business Logic in Flows](./0002-no-business-logic-in-flows.md)
- [Orchestration Vision](../docs/orchestration-vision.md)
- [When to Use n8n](../docs/when-to-use-n8n.md)

## References

- [n8n Documentation](https://docs.n8n.io/)
- [Orchestration Patterns](../docs/orchestration-vision.md)

---

**Date**: 2026  
**Deciders**: Icarus Nova Architecture Team  
**Status**: Accepted
