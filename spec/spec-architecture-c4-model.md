---
title: Architecture Design - C4 Model for Genomics Results HL7 Sender
date_created: 2025-08-21
owner: Genomics-Partnership-Wales
tags: [architecture, C4, diagram, DDD, .NET, HL7]

This specification provides a C4 model architecture diagram and description for the Genomics Results HL7 Sender. The C4 model clarifies the system's context, containers, components, and key relationships for developers, architects, and stakeholders.

## 1. Purpose & Scope

To visually and textually describe the system architecture using the C4 model, supporting implementation, onboarding, and review. Intended for technical and non-technical stakeholders.

## 2. Definitions


## 3. Requirements, Constraints & Guidelines


## 4. C4 Model Diagrams & Descriptions

### 4.1 Context Diagram

```mermaid
C4Context
    title System Context: Genomics Results HL7 Sender
    Person(labuser, "Lab User", "Uploads result files and monitors status")
    System(lims, "LIMS (SQL Server)", "Provides metadata for result files")
    System_Ext(nhs, "NHS Wales HL7 Receiver", "Receives HL7 messages via HTTPS/TCP/IP")
    System_Boundary(sender, "Genomics Results HL7 Sender") {
      Container(ui, "Dashboard UI", ".NET, GDS", "User interface for monitoring and admin")
      Container(service, "HL7 Sender Service", ".NET", "Processes files, queries LIMS, generates and delivers HL7 messages")
      ContainerDb(logdb, "Audit/Log DB", "SQL Server", "Stores logs and audit trail")
    }
    Rel(labuser, ui, "Uses")
    Rel(ui, service, "Configures, monitors")
    Rel(service, lims, "Queries metadata")
    Rel(service, nhs, "Delivers HL7 messages")
    Rel(service, logdb, "Writes logs/audit")
```

### 4.2 Container Diagram

```mermaid
C4Container
    title Container Diagram: Genomics Results HL7 Sender
    Container(ui, "Dashboard UI", ".NET, GDS", "User/admin interface")
    Container(service, "HL7 Sender Service", ".NET", "File watcher, LIMS client, HL7 generator, delivery engine")
    ContainerDb(logdb, "Audit/Log DB", "SQL Server", "Logs, audit trail")
    Container(lims, "LIMS (SQL Server)", "Metadata source")
    Container(nhs, "NHS Wales HL7 Receiver", "HL7 endpoint")
    Rel(ui, service, "Configures, monitors")
    Rel(service, lims, "Metadata queries")
    Rel(service, nhs, "HL7 delivery (HTTPS/TCP/IP)")
    Rel(service, logdb, "Logs/audit")
```

### 4.3 Component Diagram (HL7 Sender Service)

```mermaid
C4Component
    title Component Diagram: HL7 Sender Service
    Component(filewatcher, "File Watcher", "Monitors result folder")
    Component(limsclient, "LIMS Client", "Queries metadata from LIMS")
    Component(hl7gen, "HL7 Generator", "Builds HL7 ORU^R01 messages")
    Component(delivery, "Delivery Engine", "Sends HL7 via HTTPS/TCP/IP (MLLP)")
    Component(logger, "Logger/Audit", "Structured logging, audit trail")
    Rel(filewatcher, limsclient, "Triggers metadata query")
    Rel(limsclient, hl7gen, "Passes metadata")
    Rel(hl7gen, delivery, "Passes HL7 message")
    Rel(delivery, logger, "Logs delivery events")
    Rel(filewatcher, logger, "Logs file events")
```

## 5. Acceptance Criteria


## 6. Test Automation Strategy


## 7. Rationale & Context

The C4 model provides a clear, layered view of the system, supporting maintainability, onboarding, and compliance.

## 8. Dependencies & External Integrations


## 9. Examples & Edge Cases


## 10. Validation Criteria


## 11. Related Specifications / Further Reading

