---
title: Architecture Specification - Genomics Results HL7 Sender
date_created: 2025-08-21
owner: Genomics-Partnership-Wales
tags: [architecture, design, .NET, DDD, HL7, NHS, genomics]
---

# Introduction

This specification defines the architecture for the Genomics Results HL7 Sender, an on-premises automation tool for NHS Wales. The system automates the extraction, transformation, and delivery of genomics results as HL7 v2.5.1 ORU^R01 messages, integrating with a Laboratory Information Management System (LIMS) via SQL Server and delivering messages to external systems over HTTPS or TCP/IP. The architecture follows Domain-Driven Design (DDD) and Clean Architecture principles, ensuring maintainability, scalability, and compliance with NHS and GDS standards.

## 1. Purpose & Scope

This specification provides a technical blueprint for implementing the Genomics Results HL7 Sender. It is intended for software architects, developers, and DevOps engineers responsible for design, implementation, and maintenance. Assumes a senior .NET team with DDD/Clean Architecture experience.

## 2. Definitions

- **HL7**: Health Level 7, a set of international standards for transfer of clinical and administrative data
- **ORU^R01**: HL7 message type for unsolicited transmission of observation results
- **LIMS**: Laboratory Information Management System
- **DDD**: Domain-Driven Design
- **Clean Architecture**: Architectural pattern emphasizing separation of concerns and dependency inversion
- **GDS**: Government Digital Service (UK)
- **GIT**: GitHub version control
- **CI/CD**: Continuous Integration/Continuous Deployment

## 3. Requirements, Constraints & Guidelines

- **REQ-001**: The system shall monitor a specified folder for new PDF files
- **REQ-002**: The system shall parse filenames to extract unique laboratory identifiers
- **REQ-003**: The system shall query SQL Server for metadata using the identifier
- **REQ-004**: The system shall generate HL7 v2.5.1 ORU^R01 messages in XML
- **REQ-005**: The system shall send HL7 messages via HTTPS or TCP/IP to external systems
- **REQ-006**: The system shall log all processing steps and errors
- **REQ-007**: The system shall provide error handling and retry logic
- **REQ-008**: The system shall support configuration of folder paths, endpoints, and retry policies
- **SEC-001**: All data in transit must use secure protocols (HTTPS/TLS)
- **SEC-002**: The system must comply with NHS Wales, GDPR, and GDS security standards
- **CON-001**: Solution must run on-premises on Windows Server/VM
- **CON-002**: Only .NET and C# are permitted for core implementation
- **GUD-001**: Follow DDD and Clean Architecture for all business logic and boundaries
- **GUD-002**: Use GitHub for version control and CI/CD
- **PAT-001**: Use repository and service patterns for data access and business logic

## 4. Interfaces & Data Contracts

- **File System Interface**: Watches folder, triggers on new PDF
- **LIMS SQL Server Interface**: Parameterized queries for metadata retrieval
- **HL7 Message Generator**: Converts metadata to HL7 v2.5.1 ORU^R01 XML
- **Transport Layer**: Sends HL7 messages via HTTPS (REST POST) or TCP/IP (MLLP)
- **Configuration Interface**: UI or config file for folder paths, endpoints, retry settings
- **Logging/Audit Interface**: Structured logs, error reports, audit trail export

**HL7 ORU^R01 Example (XML):**
```xml
<ORU_R01>
  <MSH>...</MSH>
  <PID>...</PID>
  <OBR>...</OBR>
  <OBX>...</OBX>
</ORU_R01>
```

## 5. Acceptance Criteria

- **AC-001**: Given a new PDF in the monitored folder, When the filename is valid, Then the system shall process and deliver the HL7 message within 2 minutes
- **AC-002**: The system shall log all errors and support retry for failed deliveries
- **AC-003**: The system shall support configuration changes without code redeployment
- **AC-004**: All data transfers must be encrypted and auditable

## 6. Test Automation Strategy

- **Test Levels**: Unit (domain logic), Integration (SQL, HL7, transport), End-to-End (full workflow)
- **Frameworks**: MSTest, FluentAssertions, Moq
- **Test Data Management**: Use test DB and file system mocks; clean up after tests
- **CI/CD Integration**: Automated tests in GitHub Actions
- **Coverage Requirements**: 85%+ for domain/application layers
- **Performance Testing**: Simulate 1000 results/day throughput

## 7. Rationale & Context

- DDD and Clean Architecture ensure maintainability and testability for NHS-scale systems
- On-premises deployment is required for data protection and compliance
- Supporting both HTTPS and TCP/IP (MLLP) maximizes interoperability with NHS and partner systems
- Structured logging and audit trails are critical for compliance and troubleshooting

## 8. Dependencies & External Integrations

### External Systems
- **EXT-001**: LIMS SQL Server - Metadata source
- **EXT-002**: External HL7 receiver (HTTPS/TCP/IP) - Message destination

### Third-Party Services
- **SVC-001**: None required (all core logic in .NET)

### Infrastructure Dependencies
- **INF-001**: Windows Server/VM (on-premises)
- **INF-002**: Network access to SQL Server and external endpoints

### Data Dependencies
- **DAT-001**: LIMS database schema (tables, fields for metadata)

### Technology Platform Dependencies
- **PLT-001**: .NET 8+, C#, MSTest, GitHub Actions

### Compliance Dependencies
- **COM-001**: NHS Wales, GDPR, GDS

## 9. Examples & Edge Cases

```code
// Example: Filename parsing
// Input: "WAL1234567_20250821.pdf" => LabID: WAL1234567

// Example: HL7 message delivery failure
// If HTTPS endpoint is down, system retries up to N times, logs error, and notifies admin
```

## 10. Validation Criteria

- All requirements and acceptance criteria are covered by automated and manual tests
- Security and compliance checks are documented and passed
- System demonstrates 1000 results/day throughput in test
- All logs and audit trails are accessible and exportable

## 11. Related Specifications / Further Reading

- [HL7 v2.5.1 ORU^R01 Standard](https://www.hl7.org/implement/standards/product_brief.cfm?product_id=185)
- [GDS Service Manual](https://www.gov.uk/service-manual)
- [NHS Digital Standards](https://digital.nhs.uk/developer/guides-and-documentation/standards)
- [Clean Architecture by Uncle Bob](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)
