# When to Use n8n

> **Icarus Nova** | Decision framework for when to use n8n vs. alternatives.

## Overview

This document provides a decision framework for determining when n8n is the right choice versus alternatives like custom code, schedulers, or BPM platforms. This demonstrates architectural judgment, not tool fanaticism.

## Decision Framework

### Use n8n When

#### ✅ Orchestration of Multi-Step Processes

**Scenario:**
- Multiple systems need to be coordinated
- Steps have dependencies
- Error handling and retries are needed
- Visibility into process execution is important

**Example:**
- Legacy system migration with extraction, transformation, validation, and loading steps
- Integration workflow connecting CRM, ERP, and billing systems

**Why n8n:**
- Visual workflow makes complex processes understandable
- Built-in error handling and retries
- Execution history provides visibility

#### ✅ System Integration with Multiple Protocols

**Scenario:**
- Need to connect systems with different protocols (REST, SOAP, MQ, etc.)
- Multiple authentication mechanisms
- Data format transformations needed
- Integration logic changes frequently

**Example:**
- Integrating on-premises systems with cloud services
- Connecting legacy systems with modern APIs

**Why n8n:**
- Extensive connector library
- Visual integration design
- Easy to modify integrations

#### ✅ Event-Driven Workflows

**Scenario:**
- React to events from multiple sources
- Route events to appropriate handlers
- Coordinate event processing workflows
- Handle event failures

**Example:**
- Processing webhook events from multiple systems
- Routing events based on content
- Coordinating event-driven migrations

**Why n8n:**
- Webhook support
- Event routing capabilities
- Error handling for event processing

#### ✅ Migration Orchestration

**Scenario:**
- Coordinating data migration from legacy systems
- Managing migration dependencies
- Handling partial failures
- Providing migration visibility

**Example:**
- Migrating customer data from legacy CRM to new platform
- Coordinating database migration with application updates

**Why n8n:**
- Visual workflow for migration steps
- Error handling and recovery
- Execution tracking

### Do NOT Use n8n When

#### ❌ Business Logic Implementation

**Scenario:**
- Complex business rules and calculations
- Domain-specific validations
- Business logic that changes frequently
- Logic that needs unit testing

**Alternative:**
- Custom code in services
- Business rules engine
- Domain-specific services

**Why Not n8n:**
- Business logic should be in code for testability
- Complex logic is hard to maintain in flows
- Version control and testing are limited

#### ❌ High-Volume Data Processing

**Scenario:**
- Processing millions of records
- High-throughput requirements
- Real-time data processing
- Complex data transformations

**Alternative:**
- Dedicated data processing services
- ETL tools (Apache Spark, etc.)
- Stream processing platforms

**Why Not n8n:**
- Not designed for high-volume processing
- Performance limitations
- Better suited for orchestration than processing

#### ❌ Real-Time Requirements

**Scenario:**
- Sub-second response requirements
- Low-latency processing
- Real-time decision making
- Synchronous request-response

**Alternative:**
- Direct API calls
- Real-time processing services
- Event streaming platforms

**Why Not n8n:**
- Workflow execution adds latency
- Not suitable for real-time requirements
- Better for async orchestration

#### ❌ Complex State Management

**Scenario:**
- Complex state machines
- Long-running processes with state
- State that needs to be queryable
- Distributed state management

**Alternative:**
- State machines (AWS Step Functions, etc.)
- Workflow engines with state management
- Custom state management services

**Why Not n8n:**
- Limited state management capabilities
- State not easily queryable
- Better tools for state management

## Comparison with Alternatives

### n8n vs. Custom Code

**Use n8n When:**
- Workflow changes frequently
- Non-developers need to understand workflows
- Multiple systems need coordination
- Visual workflow is beneficial

**Use Custom Code When:**
- Complex business logic
- High-performance requirements
- Tight integration with codebase
- Extensive testing needed

### n8n vs. Schedulers (Cron, etc.)

**Use n8n When:**
- Multi-step workflows
- Error handling and retries needed
- Visibility into execution
- Dynamic workflow routing

**Use Schedulers When:**
- Simple scheduled tasks
- Single-step operations
- No error handling needed
- Minimal visibility requirements

### n8n vs. BPM Platforms

**Use n8n When:**
- Technical workflows
- System integration focus
- Developer-friendly tooling
- Cost-effective solution needed

**Use BPM When:**
- Business process management
- Human workflow involvement
- Process modeling and optimization
- Enterprise BPM requirements

### n8n vs. API Gateway

**Use n8n When:**
- Multi-step orchestration
- Workflow coordination
- Error handling and retries
- Visual workflow design

**Use API Gateway When:**
- API routing and management
- Rate limiting and throttling
- API security
- API versioning

## Decision Matrix

| Scenario | n8n | Custom Code | Scheduler | BPM | API Gateway |
|----------|-----|-------------|-----------|-----|-------------|
| Multi-step orchestration | ✅ | ✅ | ❌ | ✅ | ❌ |
| Business logic | ❌ | ✅ | ❌ | ✅ | ❌ |
| High-volume processing | ❌ | ✅ | ❌ | ❌ | ❌ |
| Real-time requirements | ❌ | ✅ | ❌ | ❌ | ✅ |
| System integration | ✅ | ✅ | ❌ | ✅ | ✅ |
| Visual workflow | ✅ | ❌ | ❌ | ✅ | ❌ |
| Frequent changes | ✅ | ⚠️ | ⚠️ | ⚠️ | ⚠️ |
| Error handling | ✅ | ✅ | ❌ | ✅ | ⚠️ |

## Hybrid Approaches

### n8n + Custom Services

**Pattern:**
- n8n orchestrates workflows
- Custom services implement business logic
- n8n calls services for processing
- Services return results to n8n

**Benefits:**
- Best of both worlds
- Business logic in testable code
- Orchestration in visual workflows

### n8n + API Gateway

**Pattern:**
- API Gateway handles API routing
- n8n orchestrates backend workflows
- API Gateway calls n8n webhooks
- n8n coordinates backend services

**Benefits:**
- API management at gateway
- Workflow orchestration in n8n
- Clear separation of concerns

## Best Practices

### When Using n8n

1. **Keep Flows Simple**: Focus on orchestration, not logic
2. **Externalize Logic**: Business logic in services
3. **Idempotent Flows**: Safe to retry
4. **Observable**: Full visibility
5. **Version Control**: All flows in Git

### When NOT Using n8n

1. **Complex Logic**: Use custom code
2. **High Performance**: Use dedicated services
3. **Real-Time**: Use direct APIs
4. **State Management**: Use state machines

## Related Documents

- [Orchestration Vision](./orchestration-vision.md)
- [Flow Design Principles](./flow-design-principles.md)
- [ADR-0001: Why n8n as Orchestrator](../adr/0001-why-n8n-as-orchestrator.md)

---

**Last Updated:** 2024  
**Maintained by:** Icarus Nova Architecture Team  
**Version:** 1.0
