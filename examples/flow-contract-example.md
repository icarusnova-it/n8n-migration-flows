# Flow Contract Example

> **Icarus Nova** | Example of flow input/output contracts and versioning (NOT production code).

## Overview

This document shows how to define contracts for n8n flows, including input/output schemas, versioning, and expectations. This demonstrates professional flow design.

## Flow Contract

### Flow: Migrate Customer Data

**Version:** 1.0  
**Last Updated:** 2024-01-15  
**Status:** Production

## Input Contract

### Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["customerId", "migrationType"],
  "properties": {
    "customerId": {
      "type": "string",
      "pattern": "^[0-9]{5}$",
      "description": "Customer ID from legacy system (5 digits)"
    },
    "migrationType": {
      "type": "string",
      "enum": ["full", "incremental"],
      "description": "Type of migration to perform"
    },
    "correlationId": {
      "type": "string",
      "pattern": "^[a-z-]+-[0-9]{8}-[a-f0-9]{8}$",
      "description": "Correlation ID for tracing (optional, auto-generated if not provided)"
    },
    "idempotencyKey": {
      "type": "string",
      "description": "Idempotency key for safe retries (optional, auto-generated if not provided)"
    }
  }
}
```

### Example Input

```json
{
  "customerId": "12345",
  "migrationType": "full",
  "correlationId": "migrate-customer-20240115-550e8400",
  "idempotencyKey": "migrate-customer-12345-20240115"
}
```

### Validation Rules

- Customer ID must be 5 digits
- Migration type must be "full" or "incremental"
- Correlation ID format validated
- All required fields present

## Output Contract

### Success Response

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["status", "customerId", "newCustomerId"],
  "properties": {
    "status": {
      "type": "string",
      "enum": ["success"],
      "description": "Migration status"
    },
    "customerId": {
      "type": "string",
      "description": "Original customer ID"
    },
    "newCustomerId": {
      "type": "string",
      "description": "New customer ID in target system"
    },
    "migrationDate": {
      "type": "string",
      "format": "date-time",
      "description": "Migration completion timestamp"
    },
    "correlationId": {
      "type": "string",
      "description": "Correlation ID for tracing"
    }
  }
}
```

### Error Response

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["status", "error"],
  "properties": {
    "status": {
      "type": "string",
      "enum": ["error"],
      "description": "Migration status"
    },
    "error": {
      "type": "object",
      "required": ["code", "message"],
      "properties": {
        "code": {
          "type": "string",
          "description": "Error code"
        },
        "message": {
          "type": "string",
          "description": "Human-readable error message"
        },
        "details": {
          "type": "object",
          "description": "Additional error details"
        }
      }
    },
    "correlationId": {
      "type": "string",
      "description": "Correlation ID for tracing"
    }
  }
}
```

### Example Outputs

**Success:**
```json
{
  "status": "success",
  "customerId": "12345",
  "newCustomerId": "new-67890",
  "migrationDate": "2024-01-15T10:30:00Z",
  "correlationId": "migrate-customer-20240115-550e8400"
}
```

**Error:**
```json
{
  "status": "error",
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Customer data validation failed",
    "details": {
      "field": "email",
      "reason": "Invalid email format"
    }
  },
  "correlationId": "migrate-customer-20240115-550e8400"
}
```

## Versioning

### Version Strategy

**Format:** `MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes to contract
- **MINOR**: New optional fields, backward compatible
- **PATCH**: Bug fixes, no contract changes

### Version History

**v1.0.0 (2024-01-15):**
- Initial version
- Basic migration support

**v1.1.0 (2024-02-01):**
- Added optional `dryRun` parameter
- Added `metadata` field to output

**v2.0.0 (2024-03-01):**
- Breaking: Changed `customerId` format
- Breaking: Removed `migrationType` enum value "partial"

## Expectations

### Performance

- **Expected Execution Time**: < 30 seconds
- **Timeout**: 60 seconds
- **Retry Policy**: 3 attempts with exponential backoff

### Reliability

- **Success Rate**: > 99%
- **Idempotency**: Fully idempotent
- **Error Recovery**: Automatic retry for transient errors

### Observability

- **Correlation ID**: Required in all operations
- **Logging**: All steps logged
- **Metrics**: Execution metrics tracked

## Related Documents

- [Flow Design Principles](../docs/flow-design-principles.md)
- [Idempotency Patterns](../docs/idempotency-patterns.md)
- [Observability and Logging](../docs/observability-and-logging.md)

---

**Note:** This is a conceptual example for educational purposes. Production implementations should include proper error handling, security, compliance, and performance optimizations specific to your environment.

**Last Updated:** 2024  
**Maintained by:** Icarus Nova Architecture Team
