---
goal: Implementation Plan for Genomics Results HL7 Sender MVP
version: 1.0
date_created: 2025-08-21
owner: Genomics-Partnership-Wales
status: 'Planned'
tags: [feature, architecture, MVP, genomics, HL7, .NET, NHS]
---

# Introduction

![Status: Planned](https://img.shields.io/badge/status-Planned-blue)

This implementation plan defines the atomic, executable steps required to deliver the MVP for the Genomics Results HL7 Sender. The goal is to automate the extraction, transformation, and delivery of genomics results as HL7 v2.5.1 ORU^R01 messages, integrating with LIMS (SQL Server) and delivering to external systems over HTTPS or TCP/IP. The plan is structured for autonomous execution by AI agents or humans, with explicit requirements, validation criteria, and no ambiguity.

## 1. Requirements & Constraints

- **REQ-001**: Monitor a specified folder for new PDF files
- **REQ-002**: Parse filenames to extract unique laboratory identifiers
- **REQ-003**: Query SQL Server for metadata using the identifier
- **REQ-004**: Generate HL7 v2.5.1 ORU^R01 messages in XML
- **REQ-005**: Send HL7 messages via HTTPS or TCP/IP to external systems
- **REQ-006**: Log all processing steps and errors
- **REQ-007**: Provide error handling and retry logic
- **REQ-008**: Support configuration of folder paths, endpoints, and retry policies
- **SEC-001**: All data in transit must use secure protocols (HTTPS/TLS)
- **SEC-002**: Must comply with NHS Wales, GDPR, and GDS security standards
- **CON-001**: Solution must run on-premises on Windows Server/VM
- **CON-002**: Only .NET and C# are permitted for core implementation
- **GUD-001**: Follow DDD and Clean Architecture for all business logic and boundaries
- **GUD-002**: Use GitHub for version control and CI/CD
- **PAT-001**: Use repository and service patterns for data access and business logic

## 2. Implementation Steps

### Implementation Phase 1

- GOAL-001: Establish core file monitoring, metadata retrieval, and HL7 message generation

| Task | Description | Completed | Date |
|------|-------------|-----------|------|
| TASK-001 | Implement folder watcher for new PDF files (`/src/FileWatcher.cs`) |  |  |
| TASK-002 | Implement filename parser for laboratory ID extraction (`/src/FilenameParser.cs`) |  |  |
| TASK-003 | Implement SQL Server metadata query logic (`/src/LimsMetadataService.cs`) |  |  |
| TASK-004 | Implement HL7 ORU^R01 XML message generator (`/src/Hl7MessageGenerator.cs`) |  |  |
| TASK-005 | Implement configuration loader for folder paths, endpoints, retry (`/src/ConfigLoader.cs`) |  |  |

### Implementation Phase 2

- GOAL-002: Implement delivery, error handling, logging, and configuration UI/file

| Task | Description | Completed | Date |
|------|-------------|-----------|------|
| TASK-006 | Implement HTTPS and TCP/IP (MLLP) HL7 message delivery (`/src/TransportService.cs`) |  |  |
| TASK-007 | Implement structured logging and audit trail (`/src/Logger.cs`, `/src/AuditTrailExporter.cs`) |  |  |
| TASK-008 | Implement error handling and retry logic (`/src/ErrorHandler.cs`) |  |  |
| TASK-009 | Implement configuration UI or file-based config (`/src/ConfigUI.cs` or `/config/appsettings.json`) |  |  |
| TASK-010 | Integrate all modules and implement end-to-end workflow (`/src/Program.cs`) |  |  |

## 3. Alternatives

- **ALT-001**: Use a cloud-based solution (rejected for on-premises/NHS compliance)
- **ALT-002**: Use a third-party HL7 integration engine (rejected for full .NET/C# control and maintainability)

## 4. Dependencies

- **DEP-001**: Access to LIMS SQL Server instance
- **DEP-002**: Access to external HL7 receiver endpoint (HTTPS/TCP/IP)
- **DEP-003**: Windows Server/VM for deployment
- **DEP-004**: .NET 8+ SDK and runtime

## 5. Files

- **FILE-001**: `/src/FileWatcher.cs` - Folder monitoring logic
- **FILE-002**: `/src/FilenameParser.cs` - Filename parsing logic
- **FILE-003**: `/src/LimsMetadataService.cs` - SQL Server metadata retrieval
- **FILE-004**: `/src/Hl7MessageGenerator.cs` - HL7 XML message generation
- **FILE-005**: `/src/TransportService.cs` - Message delivery logic
- **FILE-006**: `/src/Logger.cs` - Structured logging
- **FILE-007**: `/src/AuditTrailExporter.cs` - Audit trail export
- **FILE-008**: `/src/ErrorHandler.cs` - Error handling and retry
- **FILE-009**: `/src/ConfigLoader.cs` - Configuration loader
- **FILE-010**: `/src/ConfigUI.cs` or `/config/appsettings.json` - Configuration UI/file
- **FILE-011**: `/src/Program.cs` - Main workflow integration

## 6. Testing

- **TEST-001**: Unit tests for all modules (MSTest, `/tests/`)
- **TEST-002**: Integration tests for SQL, HL7, and transport layers
- **TEST-003**: End-to-end workflow tests (file to HL7 delivery)
- **TEST-004**: Performance tests for 1000 results/day throughput
- **TEST-005**: Security and compliance tests (encryption, audit, GDPR)

## 7. Risks & Assumptions

- **RISK-001**: Changes in file naming or metadata schema
- **RISK-002**: Network or endpoint outages
- **RISK-003**: Security review delays
- **ASSUMPTION-001**: File naming convention is consistent and unique
- **ASSUMPTION-002**: SQL Server contains all required metadata
- **ASSUMPTION-003**: External system is available and accepts HL7 v2.5.1 ORU^R01 XML

## 8. Related Specifications / Further Reading

- [PRD: Genomics Results HL7 Sender](../prd.md)
- [UI/UX Design Document](../ui-ux-design.md)
- [Architecture Specification](../spec/spec-architecture-hl7-sender.md)
- [HL7 v2.5.1 ORU^R01 Standard](https://www.hl7.org/implement/standards/product_brief.cfm?product_id=185)
- [GDS Service Manual](https://www.gov.uk/service-manual)
- [NHS Digital Standards](https://digital.nhs.uk/developer/guides-and-documentation/standards)
- [Clean Architecture by Uncle Bob](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)
