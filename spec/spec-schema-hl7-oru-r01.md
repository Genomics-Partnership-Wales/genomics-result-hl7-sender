---
title: Schema Specification - HL7 v2.5.1 ORU^R01 Message
version: 1.0
date_created: 2025-08-21
owner: Genomics-Partnership-Wales
tags: [schema, HL7, healthcare, interoperability]


This specification defines the HL7 v2.5.1 ORU^R01 message schema for use in the Genomics Results HL7 Sender. It provides a machine-readable, unambiguous contract for message structure, required segments, and field-level requirements for interoperability with NHS and partner systems.

## 1. Purpose & Scope




- **HL7**: Health Level 7, a set of international standards for healthcare data exchange
- **ORU^R01**: Observation Result Unsolicited message type
- **MSH**: Message Header segment
- **PID**: Patient Identification segment
- **OBR**: Observation Request segment
- **OBX**: Observation Result segment

## 3. Requirements, Constraints & Guidelines

- **REQ-001**: All HL7 messages must conform to v2.5.1 ORU^R01 structure
- **REQ-002**: All required segments (MSH, PID, OBR, OBX) must be present
- **REQ-003**: All fields must be populated according to NHS Wales data dictionary
- **REQ-004**: Message must be valid XML (if using XML encoding)
- **CON-001**: No patient-identifiable data may be persisted after message delivery
- **SEC-001**: Message content must be protected in transit (TLS/HTTPS)

## 4. Interfaces & Data Contracts

| Segment | Required | Description |
|---------|----------|-------------|
| MSH     | Yes      | Message Header |
| PID     | Yes      | Patient Identification |
| OBR     | Yes      | Observation Request |
| OBX     | Yes      | Observation Result (repeats allowed) |

**Example XML Structure:**

```xml
<ORU_R01>
  <MSH>...</MSH>
  <PID>...</PID>
  <OBR>...</OBR>
  <OBX>...</OBX>
</ORU_R01>
```

## 5. Acceptance Criteria

- **AC-001**: Given valid metadata, When a message is generated, Then the XML must validate against the HL7 v2.5.1 schema
- **AC-002**: All required segments and fields are present and correctly populated
- **AC-003**: Message passes NHS Wales interoperability tests

## 6. Test Automation Strategy

- **Test Levels**: Unit (segment construction), Integration (full message validation)
- **Frameworks**: MSTest, XML schema validation tools
- **Test Data Management**: Use synthetic patient data for tests
- **CI/CD Integration**: Automated schema validation in GitHub Actions
- **Coverage Requirements**: 100% for message construction logic

## 7. Rationale & Context

Standardized HL7 messages ensure reliable, interoperable data exchange with NHS and partner systems. XML encoding is used for compatibility with downstream systems.

## 8. Dependencies & External Integrations

### External Systems

- **EXT-001**: NHS Wales HL7 receivers (HTTPS/TCP/IP)

### Data Dependencies

- **DAT-001**: NHS Wales data dictionary for field mapping

### Compliance Dependencies

- **COM-001**: HL7 v2.5.1, NHS Wales interoperability standards

## 9. Examples & Edge Cases

```xml
<ORU_R01>
  <MSH>
    <MSH.1>|</MSH.1>
    <MSH.2>^~\&amp;</MSH.2>
    ...
  </MSH>
  <PID>
    <PID.3>123456789</PID.3>
    ...
  </PID>
  <OBR>
    <OBR.4>TestCode</OBR.4>
    ...
  </OBR>
  <OBX>
    <OBX.3>ResultType</OBX.3>
    <OBX.5>ResultValue</OBX.5>
    ...
  </OBX>
</ORU_R01>
```

## 10. Validation Criteria

- All generated messages validate against the HL7 v2.5.1 schema
- All required fields are present and correct

## 11. Related Specifications / Further Reading

- [HL7 v2.5.1 ORU^R01 Standard](https://www.hl7.org/implement/standards/product_brief.cfm?product_id=185)
- [NHS Wales Data Dictionary](https://www.nhswalesdictionary.wales.nhs.uk/)
