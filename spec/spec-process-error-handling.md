---
title: Process Specification - Error Handling & Retry Logic
date_created: 2025-08-21
owner: Genomics-Partnership-Wales
tags: [process, error-handling, retry, reliability, .NET]
# Process Specification - Error Handling & Retry Logic

# Introduction

This specification defines the error handling and retry logic for the Genomics Results HL7 Sender. It ensures robust, auditable, and compliant management of failures in file processing, metadata retrieval, message generation, and delivery.

## 1. Purpose & Scope

To provide a clear, testable process for error detection, logging, notification, and automated retry, minimizing manual intervention and ensuring compliance with NHS and GDS standards. Intended for developers, testers, and support staff.

## 2. Definitions

- **Retry Policy**: Configurable rules for how and when to retry failed operations
- **Transient Error**: Temporary failure (e.g., network outage)
- **Permanent Error**: Non-recoverable failure (e.g., invalid data)
- **Audit Trail**: Log of all error events and actions taken

## 3. Requirements, Constraints & Guidelines

- **REQ-001**: All errors must be logged with timestamp, context, and stack trace
- **REQ-002**: Transient errors must trigger automated retry according to policy
- **REQ-003**: Permanent errors must be logged and flagged for manual review
- **REQ-004**: All retries must be auditable and limited to N attempts
- **REQ-005**: Admins must be notified of repeated or critical failures
- **SEC-001**: Error logs must not contain patient-identifiable data
- **CON-001**: Retry policy must be configurable without code changes
- **GUD-001**: Use structured logging for all error and retry events

## 4. Interfaces & Data Contracts

- **Error Logging Interface**: Structured log entries (timestamp, error type, context, stack trace)
- **Retry Policy Interface**: Configurable (max attempts, backoff, error types)
- **Notification Interface**: Email or dashboard alert for critical errors
- **Audit Trail Interface**: Exportable log of all error and retry events

**Example Log Entry:**
```json
{
  "timestamp": "2025-08-21T10:15:00Z",
  "errorType": "DeliveryFailure",
  "context": "HL7 message delivery to endpoint X",
  "attempt": 3,
  "maxAttempts": 5,
  "stackTrace": "..."
}
```

## 5. Acceptance Criteria

- **AC-001**: Given a transient error, When a retry is triggered, Then the system retries up to N times and logs each attempt
- **AC-002**: Given a permanent error, When detected, Then the system logs and flags for manual review
- **AC-003**: All error and retry events are auditable and exportable

## 6. Test Automation Strategy

- **Test Levels**: Unit (error classification), Integration (retry logic), End-to-End (full error scenarios)
- **Frameworks**: MSTest, Moq, FluentAssertions
- **Test Data Management**: Simulate transient and permanent errors
- **CI/CD Integration**: Automated error scenario tests in GitHub Actions
- **Coverage Requirements**: 100% for error handling logic

## 7. Rationale & Context

Automated error handling and retry reduce manual workload, improve reliability, and ensure compliance. Structured, auditable logs support troubleshooting and regulatory requirements.

## 8. Dependencies & External Integrations

### External Systems
- **EXT-001**: Email or dashboard notification system

### Infrastructure Dependencies
- **INF-001**: Persistent log storage (file, database, or cloud)

### Compliance Dependencies
- **COM-001**: NHS Wales, GDPR, GDS

## 9. Examples & Edge Cases

```json
// Example: Retry exhausted
{
  "timestamp": "2025-08-21T10:20:00Z",
  "errorType": "DeliveryFailure",
  "context": "HL7 message delivery to endpoint X",
  "attempt": 5,
  "maxAttempts": 5,
  "status": "exhausted"
}

// Example: Permanent error
{
  "timestamp": "2025-08-21T10:25:00Z",
  "errorType": "InvalidMetadata",
  "context": "SQL query for LabID WAL1234567",
  "status": "permanent"
}
```

## 10. Validation Criteria

- All error and retry events are logged, auditable, and exportable
- Retry logic is configurable and works as specified
- No patient data is present in logs

## 11. Related Specifications / Further Reading

- [spec-architecture-hl7-sender.md](spec-architecture-hl7-sender.md)
- [NHS Digital Logging Standards](https://digital.nhs.uk/developer/guides-and-documentation/standards)
- [GDS Service Manual - Incident Management](https://www.gov.uk/service-manual/technology/incident-management)
