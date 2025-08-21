# PRD: Genomics Results HL7 Sender

## 1. Executive Summary

**Elevator Pitch**: The Genomics Results HL7 Sender is an on-premises automation tool for NHS Wales that monitors a folder for new genomics result PDFs, extracts a unique laboratory identifier from the filename, queries a SQL Server database for metadata, and generates an HL7 v2.5.1 ORU^R01 message (in XML format) to send securely via HTTPS to an external system. This solution eliminates manual data entry, reduces turnaround time, and supports scaling to high daily volumes.

**Problem Statement**: Manual processing of genomics results is slow, error-prone, and not scalable. Staff must manually extract data from PDFs, look up metadata, and communicate results to external systems, leading to delays and potential errors.

**Solution Overview**: Automate the end-to-end process by detecting new result files, extracting identifiers, retrieving metadata, generating HL7 messages, and delivering them securely and reliably to external systems, with error handling and retry logic.

## 2. Target Audience

**Primary Users**: Genomics lab staff, bioinformaticians, and clinicians in NHS Wales.

**User Personas**:
- **Lab Technician**: Responsible for uploading result PDFs and ensuring data is processed.
- **Bioinformatician**: Oversees data integrity and troubleshooting.
- **Clinician**: Consumes results in downstream systems for patient care.

**Market Size**: NHS Wales genomics labs; scalable to other NHS regions if successful.

## 3. Product Scope

**In Scope**:
- Monitor a specified folder for new PDF files
- Parse filenames for unique laboratory identifiers
- Query SQL Server for metadata
- Generate HL7 v2.5.1 ORU^R01 messages in XML
- Send messages via HTTPS to an external system
- Error handling and retry logic
- Logging and audit trail

**Out of Scope**:
- Manual data entry interfaces
- Processing of non-PDF file types
- Cloud deployment (initially)

**Future Considerations**:
- Support for additional file types or message formats
- Cloud or hybrid deployment
- Integration with other NHS systems

## 4. Functional Requirements

**Core Features**:
- Folder monitoring for new PDFs
- Filename parsing for laboratory ID
- SQL Server metadata retrieval
- HL7 v2.5.1 ORU^R01 XML message generation
- HTTPS message delivery
- Error handling and retry (configurable)
- Logging and audit

**Secondary Features**:
- Configurable folder path and endpoints
- Notification on failure (e.g., email alert)
- Dashboard for monitoring (future)

**Integration Requirements**:
- SQL Server (on-premises)
- External system accepting HL7 v2.5.1 ORU^R01 via HTTPS

## 5. Non-Functional Requirements

**Performance**:
- Process up to 1000 results per day
- Message generation and delivery within 2 minutes per result

**Security**:
- NHS Wales and GDPR compliance
- Secure HTTPS transmission
- Local access controls

**Reliability**:
- 99.5% uptime
- Automatic retries on failure
- Robust error logging

**Usability**:
- Minimal manual intervention
- Accessible logs and error reports
- WCAG 2.2 Level AA for any UI components

## 6. User Experience

**User Journey**:
1. Lab technician saves a genomics result PDF to the monitored folder
2. System detects the new file and parses the filename
3. System queries SQL Server for metadata
4. HL7 message is generated and sent via HTTPS
5. Success or error is logged; errors trigger retry and notification

**Key User Stories**:
- As a lab technician, I want new result files to be processed automatically so that I don’t have to enter data manually.
- As a bioinformatician, I want to see logs and error reports so that I can troubleshoot issues.
- As a clinician, I want results to appear quickly in downstream systems so that I can make timely decisions.
- As an admin, I want the system to retry failed deliveries so that transient issues don’t require manual intervention.
- As an IT manager, I want the system to comply with NHS Wales security and privacy standards.

**UI/UX Guidelines**:
- Simple configuration UI (if needed)
- Accessible logs (text or web-based)
- Clear error messages

## 7. Success Metrics

**Key Performance Indicators**:
- >99% of results processed and delivered within 2 minutes
- <0.5% manual intervention required

**User Engagement Metrics**:
- Number of errors requiring manual resolution
- Time to resolution for failed deliveries

**Business Metrics**:
- Reduction in manual processing time
- Increased throughput (results/day)

## 8. Technical Considerations

**Platform Requirements**:
- On-premises Windows VM

**Technology Preferences**:
- .NET and C#

**Third-party Dependencies**:
- SQL Server
- External HL7 receiver (HTTPS endpoint)

**Data Requirements**:
- Store logs, audit trails, and error reports
- No persistent storage of patient data beyond processing

## 9. Timeline & Milestones

**MVP Timeline**: Complete by end of September 2025

**Key Milestones**:
- Folder monitoring and file parsing: 1 week
- SQL Server integration: 1 week
- HL7 message generation: 1 week
- HTTPS delivery and error handling: 1 week
- Testing and deployment: 1 week

**Dependencies**:
- Access to SQL Server and external endpoint
- NHS Wales IT security review

## 10. Assumptions & Risks

**Key Assumptions**:
- File naming convention is consistent and unique
- SQL Server contains all required metadata
- External system is available and accepts HL7 v2.5.1 ORU^R01 XML

**Identified Risks**:
- Changes in file naming or metadata schema
- Network or endpoint outages
- Security review delays

**Open Questions**:
- Will future versions require cloud or hybrid deployment?
- Are there additional notification or dashboard requirements?
