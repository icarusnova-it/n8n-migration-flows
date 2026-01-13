# Orchestration Architecture

> **Icarus Nova** | Architecture diagram showing n8n as orchestration layer.

## Architecture Diagram

```mermaid
graph TB
    subgraph "Business Applications"
        App1[Application 1]
        App2[Application 2]
    end
    
    subgraph "Business Logic Services"
        Service1[Validation Service]
        Service2[Transformation Service]
        Service3[Processing Service]
    end
    
    subgraph "Orchestration Layer (n8n)"
        n8n[n8n Platform]
        Flow1[Migration Flow]
        Flow2[Integration Flow]
        Flow3[Error Recovery Flow]
    end
    
    subgraph "External Systems"
        Legacy[Legacy Systems]
        Cloud[Cloud Services]
        APIs[External APIs]
    end
    
    subgraph "Storage"
        DB[(Database)]
        Queue[Message Queue]
    end
    
    subgraph "Observability"
        Logs[Logging]
        Metrics[Metrics]
        Traces[Traces]
    end
    
    App1 --> Service1
    App2 --> Service2
    Service1 --> n8n
    Service2 --> n8n
    Service3 --> n8n
    
    n8n --> Flow1
    n8n --> Flow2
    n8n --> Flow3
    
    Flow1 --> Legacy
    Flow2 --> Cloud
    Flow3 --> APIs
    
    Flow1 --> Service3
    Flow2 --> Service1
    
    n8n --> DB
    n8n --> Queue
    
    n8n --> Logs
    n8n --> Metrics
    n8n --> Traces
    
    style n8n fill:#90EE90
    style Service1 fill:#87CEEB
    style Service2 fill:#87CEEB
    style Service3 fill:#87CEEB
```

## Architecture Layers

### Business Applications
- User-facing applications
- Trigger workflows
- Receive workflow results

### Business Logic Services
- Core business logic
- Data validation
- Data transformation
- Data processing

### Orchestration Layer (n8n)
- Workflow orchestration
- Error handling
- Retry logic
- Routing

### External Systems
- Legacy systems
- Cloud services
- External APIs
- Databases

### Storage
- Workflow state
- Message queues
- Data storage

### Observability
- Logging
- Metrics
- Tracing
- Alerting

## Key Principles

1. **n8n as Orchestrator**: n8n orchestrates, services execute
2. **Thin Orchestration**: Minimal logic in flows
3. **Externalized Logic**: Business logic in services
4. **Observable**: Full visibility
5. **Resilient**: Error handling and retries

## Related Documents

- [C4 System Context](./c4-system-context.md)
- [Orchestration Vision](../docs/orchestration-vision.md)

---

**Last Updated:** 2024  
**Maintained by:** Icarus Nova Architecture Team  
**Version:** 1.0
