# Compliance & Regulatory Domain
## Logical Data Model - Entity Definitions

**Domain:** Compliance & Regulatory  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Supporting & Analytical Domain

---

## Domain Overview

**Purpose:** Track regulatory filings, compliance requirements, audit trails, licensing, and regulatory examinations to ensure adherence to insurance regulations.

**Scope:** This domain encompasses state and federal regulatory filings, compliance monitoring, audit documentation, producer licensing, regulatory examinations, and privacy/consent management.

**Business Value:**
- Regulatory compliance assurance
- Filing deadline management
- Examination readiness
- License status monitoring
- Audit trail maintenance
- Privacy and consent management
- Regulatory risk mitigation

---

## Entity Catalog

### REGULATORY_FILING

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Regulatory Filing |
| **Entity Definition** | A formal submission to a regulatory authority, including annual statements, rate filings, form filings, and other required reports. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Filing Identifier |
| **Key Relationships** | - Submitted to Regulatory Authority<br>- Has Filing Type<br>- Has Filing Status<br>- For Jurisdiction (Geography Domain)<br>- For Reporting Period (Time/Calendar Domain) |
| **Assumptions** | - All regulatory filings tracked<br>- Filing deadlines monitored |
| **Open Questions** | - What filing types are required? |

---

### FILING_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Filing Type |
| **Entity Definition** | A classification of Regulatory Filings based on the nature and purpose of the submission. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Regulatory Filing<br>- Has required frequency and deadline |
| **Assumptions** | - Types: ANNUAL_STATEMENT, QUARTERLY_STATEMENT, RATE_FILING, FORM_FILING, MARKET_CONDUCT_REPORT, COMPLAINT_REPORT, FINANCIAL_EXAM_RESPONSE |
| **Open Questions** | - None at this time |

---

### FILING_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Filing Status |
| **Entity Definition** | The current state of a Regulatory Filing in its lifecycle from preparation to acceptance. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Applied to Regulatory Filing |
| **Assumptions** | - Statuses: DRAFT, PREPARED, SUBMITTED, UNDER_REVIEW, ACCEPTED, REJECTED, AMENDED |
| **Open Questions** | - None at this time |

---

### REGULATORY_AUTHORITY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Regulatory Authority |
| **Entity Definition** | A government agency or body with authority to regulate insurance operations, such as state insurance departments or federal agencies. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Authority Identifier |
| **Key Relationships** | - Receives Regulatory Filings<br>- Conducts Regulatory Examinations<br>- Governs Jurisdiction |
| **Assumptions** | - Primary regulators: state insurance departments<br>- Federal agencies for specific areas |
| **Open Questions** | - None at this time |

---

### REGULATORY_REQUIREMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Regulatory Requirement |
| **Entity Definition** | A specific obligation imposed by regulation or statute that the company must comply with. Requirements are tracked for compliance monitoring. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Requirement Identifier |
| **Key Relationships** | - Imposed by Regulatory Authority<br>- For Jurisdiction<br>- Has Compliance Status<br>- May require Regulatory Filing |
| **Assumptions** | - Requirements catalogued and maintained<br>- Compliance tracked per requirement |
| **Open Questions** | - How are requirements identified and tracked? |

---

### COMPLIANCE_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Compliance Status |
| **Entity Definition** | A record of the company's current compliance posture with respect to a specific Regulatory Requirement. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Requirement Identifier + Assessment Date |
| **Key Relationships** | - For Regulatory Requirement<br>- Has Status value<br>- Documented with evidence |
| **Assumptions** | - Status values: COMPLIANT, NON_COMPLIANT, PARTIALLY_COMPLIANT, UNDER_REVIEW<br>- Regular assessment performed |
| **Open Questions** | - What assessment frequency? |

---

### COMPLIANCE_ASSESSMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Compliance Assessment |
| **Entity Definition** | A formal evaluation of compliance with regulatory requirements, conducted internally or by external parties. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Assessment Identifier |
| **Key Relationships** | - Assesses Regulatory Requirements<br>- Has Assessment Type<br>- Produces findings/results |
| **Assumptions** | - Regular assessments conducted<br>- Findings tracked to remediation |
| **Open Questions** | - None at this time |

---

### COMPLIANCE_FINDING

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Compliance Finding |
| **Entity Definition** | A specific issue or gap identified during a Compliance Assessment, requiring documentation and potentially remediation. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Finding Identifier |
| **Key Relationships** | - Identified in Compliance Assessment<br>- Has Severity level<br>- May require Remediation Action |
| **Assumptions** | - All findings documented<br>- Severity drives priority |
| **Open Questions** | - None at this time |

---

### REMEDIATION_ACTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Remediation Action |
| **Entity Definition** | A planned or completed action to address a Compliance Finding, bringing the organization into compliance. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Action Identifier |
| **Key Relationships** | - Addresses Compliance Finding<br>- Has Action Status<br>- Assigned to owner<br>- Has due date |
| **Assumptions** | - Actions tracked to completion<br>- Verification of effectiveness |
| **Open Questions** | - None at this time |

---

### REGULATORY_EXAMINATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Regulatory Examination |
| **Entity Definition** | A formal examination conducted by a Regulatory Authority, including financial examinations and market conduct examinations. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Examination Identifier |
| **Key Relationships** | - Conducted by Regulatory Authority<br>- Has Examination Type<br>- Has Examination Status<br>- Produces Examination Findings |
| **Assumptions** | - Exams tracked comprehensively<br>- Findings managed through resolution |
| **Open Questions** | - None at this time |

---

### EXAMINATION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Examination Type |
| **Entity Definition** | A classification of Regulatory Examinations based on the scope and focus of the examination. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Regulatory Examination |
| **Assumptions** | - Types: FINANCIAL_EXAM, MARKET_CONDUCT_EXAM, TARGET_EXAM, COMPREHENSIVE_EXAM |
| **Open Questions** | - None at this time |

---

### EXAMINATION_FINDING

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Examination Finding |
| **Entity Definition** | A specific issue or observation identified by regulators during a Regulatory Examination. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Finding Identifier |
| **Key Relationships** | - Identified in Regulatory Examination<br>- Has Severity/Priority<br>- Requires Response/Remediation |
| **Assumptions** | - All findings tracked<br>- Response deadlines monitored |
| **Open Questions** | - None at this time |

---

### PRODUCER_LICENSE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Producer License |
| **Entity Definition** | A state-issued license authorizing an individual or entity to sell insurance products in that jurisdiction. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | License Number + Jurisdiction |
| **Key Relationships** | - Held by Producer/Agent (Party/Customer Domain)<br>- Issued by Jurisdiction<br>- Has License Type<br>- Has License Status |
| **Assumptions** | - Licenses tracked per producer per state<br>- Expiration and renewal monitored |
| **Open Questions** | - None at this time |

---

### LICENSE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | License Type |
| **Entity Definition** | A classification of Producer Licenses based on the lines of authority granted. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Producer License |
| **Assumptions** | - Types: LIFE_ONLY, ACCIDENT_HEALTH, VARIABLE_PRODUCTS, LIFE_ACCIDENT_HEALTH |
| **Open Questions** | - None at this time |

---

### LICENSE_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | License Status |
| **Entity Definition** | The current state of a Producer License. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Applied to Producer License |
| **Assumptions** | - Statuses: ACTIVE, INACTIVE, EXPIRED, SUSPENDED, REVOKED, PENDING |
| **Open Questions** | - None at this time |

---

### LICENSE_APPOINTMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | License Appointment |
| **Entity Definition** | A formal relationship between a licensed producer and an insurance company, authorizing the producer to represent the company. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Appointment Identifier |
| **Key Relationships** | - For Producer License<br>- With Insurer (company)<br>- In Jurisdiction<br>- Has Appointment Status |
| **Assumptions** | - Appointments required in most states<br>- Appointment status tracked |
| **Open Questions** | - None at this time |

---

### COMPANY_LICENSE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Company License |
| **Entity Definition** | A state-issued certificate of authority authorizing the insurance company to conduct business in that jurisdiction. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | License Number + Jurisdiction |
| **Key Relationships** | - Issued by Jurisdiction (Geography Domain)<br>- Has License Type<br>- Has License Status |
| **Assumptions** | - Licenses tracked per state<br>- Annual renewals managed |
| **Open Questions** | - None at this time |

---

### AUDIT_TRAIL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Audit Trail |
| **Entity Definition** | A chronological record of system activities, documenting who did what, when, and to which records. Audit trails support compliance and investigations. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Trail Identifier |
| **Key Relationships** | - Records activity for Entity/Record<br>- By User/System<br>- Has Action Type<br>- At Timestamp |
| **Assumptions** | - All significant activities logged<br>- Retention per regulatory requirements |
| **Open Questions** | - What retention period? |

---

### AUDIT_ACTION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Audit Action Type |
| **Entity Definition** | A classification of actions recorded in Audit Trails. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Audit Trail entries |
| **Assumptions** | - Types: CREATE, UPDATE, DELETE, VIEW, APPROVE, REJECT, LOGIN, LOGOUT |
| **Open Questions** | - None at this time |

---

### PRIVACY_CONSENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Privacy Consent |
| **Entity Definition** | A record of consent provided by a Customer for specific data processing activities, as required by privacy regulations. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Consent Identifier |
| **Key Relationships** | - Given by Customer (Party/Customer Domain)<br>- For Consent Purpose<br>- Has Consent Status<br>- Effective for time period |
| **Assumptions** | - Consent captured and tracked<br>- Withdrawal supported |
| **Open Questions** | - What privacy regulations apply? |

---

### CONSENT_PURPOSE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Consent Purpose |
| **Entity Definition** | The specific purpose for which customer consent is requested, such as marketing, data sharing, or analytics. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Purpose Code |
| **Key Relationships** | - Applied to Privacy Consent |
| **Assumptions** | - Purposes: MARKETING_EMAIL, MARKETING_PHONE, DATA_SHARING, ANALYTICS, THIRD_PARTY_OFFERS |
| **Open Questions** | - None at this time |

---

### DATA_SUBJECT_REQUEST

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Data Subject Request |
| **Entity Definition** | A formal request from a Customer exercising privacy rights, such as access, correction, deletion, or portability. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Request Identifier |
| **Key Relationships** | - Submitted by Customer (Party/Customer Domain)<br>- Has Request Type<br>- Has Request Status<br>- Has regulatory deadline |
| **Assumptions** | - DSR tracked per regulations<br>- Response deadlines monitored |
| **Open Questions** | - None at this time |

---

### DSR_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | DSR Type |
| **Entity Definition** | A classification of Data Subject Requests based on the right being exercised. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Data Subject Request |
| **Assumptions** | - Types: ACCESS, CORRECTION, DELETION, PORTABILITY, OPT_OUT_SALE, RESTRICT_PROCESSING |
| **Open Questions** | - None at this time |

---

### REGULATORY_COMPLAINT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Regulatory Complaint |
| **Entity Definition** | A complaint escalated to or filed directly with a Regulatory Authority, requiring formal response and tracking. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Complaint Identifier |
| **Key Relationships** | - Filed by/on behalf of Customer (Party/Customer Domain)<br>- Received by Regulatory Authority<br>- Regarding Policy (Policy Domain)<br>- Has Complaint Status<br>- Linked to Customer Complaint (Customer Engagement Domain) |
| **Assumptions** | - All regulatory complaints tracked<br>- Response deadlines monitored |
| **Open Questions** | - None at this time |

---

### REGULATORY_COMPLAINT_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Regulatory Complaint Status |
| **Entity Definition** | The current state of a Regulatory Complaint. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Applied to Regulatory Complaint |
| **Assumptions** | - Statuses: RECEIVED, INVESTIGATING, RESPONSE_SUBMITTED, CLOSED, REOPENED |
| **Open Questions** | - None at this time |

---

### RATE_FILING

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Rate Filing |
| **Entity Definition** | A regulatory submission for approval of premium rates, including supporting actuarial justification. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Filing Identifier |
| **Key Relationships** | - For Product (Product Domain)<br>- Submitted to Jurisdiction<br>- Has Filing Status<br>- Contains Rate Changes |
| **Assumptions** | - Rate filings required per state<br>- Prior approval vs. file-and-use varies |
| **Open Questions** | - What jurisdictions require prior approval? |

---

### FORM_FILING

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Form Filing |
| **Entity Definition** | A regulatory submission for approval of policy forms, applications, and other customer-facing documents. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Filing Identifier |
| **Key Relationships** | - For Document/Form (Policy Domain)<br>- Submitted to Jurisdiction<br>- Has Filing Status |
| **Assumptions** | - Forms require state approval<br>- Interstate Compact may apply |
| **Open Questions** | - None at this time |

---

### COMPLIANCE_POLICY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Compliance Policy |
| **Entity Definition** | An internal policy document establishing standards and procedures for regulatory compliance in a specific area. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Policy Identifier |
| **Key Relationships** | - Addresses Regulatory Requirements<br>- Has Version/Effective Date<br>- Approved by governance |
| **Assumptions** | - Policies maintained and current<br>- Version control in place |
| **Open Questions** | - None at this time |

---

### COMPLIANCE_TRAINING

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Compliance Training |
| **Entity Definition** | A training course or program required for compliance purposes, such as anti-money laundering or privacy training. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Training Identifier |
| **Key Relationships** | - Required by Regulatory Requirement<br>- Has Training Content<br>- Has completion requirements |
| **Assumptions** | - Required training tracked<br>- Completion monitored |
| **Open Questions** | - None at this time |

---

### TRAINING_COMPLETION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Training Completion |
| **Entity Definition** | A record of an individual completing required Compliance Training. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Employee/Producer Identifier + Training Identifier |
| **Key Relationships** | - By Employee/Producer<br>- For Compliance Training<br>- Has completion date |
| **Assumptions** | - Completion tracked per individual<br>- Expiration/renewal tracked |
| **Open Questions** | - None at this time |

---

### SANCTIONS_SCREENING

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Sanctions Screening |
| **Entity Definition** | A check of a Party against sanctions lists (OFAC, etc.) to ensure compliance with anti-terrorism and sanctions regulations. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Screening Identifier |
| **Key Relationships** | - For Party (Party/Customer Domain)<br>- Against Sanctions List<br>- Has Screening Result |
| **Assumptions** | - Screening at onboarding and periodic<br>- Match resolution documented |
| **Open Questions** | - What screening frequency? |

---

### SANCTIONS_LIST

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Sanctions List |
| **Entity Definition** | An official list of sanctioned individuals and entities maintained by a government authority. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | List Identifier |
| **Key Relationships** | - Published by Authority<br>- Used for Sanctions Screening |
| **Assumptions** | - Lists: OFAC_SDN, OFAC_CONSOLIDATED, UN_SANCTIONS, EU_SANCTIONS |
| **Open Questions** | - None at this time |

---

### AML_CASE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | AML Case |
| **Entity Definition** | An anti-money laundering investigation case triggered by suspicious activity or screening alerts. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Case Identifier |
| **Key Relationships** | - Regarding Customer (Party/Customer Domain)<br>- Triggered by Sanctions Screening or Suspicious Activity<br>- Has Case Status<br>- May result in SAR filing |
| **Assumptions** | - AML cases tracked comprehensively<br>- SAR filing tracked |
| **Open Questions** | - None at this time |

---

### SUSPICIOUS_ACTIVITY_REPORT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Suspicious Activity Report |
| **Entity Definition** | A formal filing with FinCEN reporting suspicious activity that may indicate money laundering or other financial crimes. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | SAR Identifier |
| **Key Relationships** | - Filed for AML Case<br>- Submitted to FinCEN<br>- Has Filing Status |
| **Assumptions** | - SAR filing tracked<br>- Confidentiality maintained |
| **Open Questions** | - None at this time |

---

### COMPLIANCE_CALENDAR

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Compliance Calendar |
| **Entity Definition** | A schedule of compliance obligations, deadlines, and recurring activities to ensure timely completion of regulatory requirements. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Calendar Identifier |
| **Key Relationships** | - Contains Compliance Events<br>- Linked to Regulatory Requirements<br>- Tracks deadlines |
| **Assumptions** | - Calendar maintained and monitored<br>- Alerts for upcoming deadlines |
| **Open Questions** | - None at this time |

---

### COMPLIANCE_CALENDAR_EVENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Compliance Calendar Event |
| **Entity Definition** | A specific event or deadline on the Compliance Calendar requiring action. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Event Identifier |
| **Key Relationships** | - On Compliance Calendar<br>- For Regulatory Requirement<br>- Has due date and status |
| **Assumptions** | - Events tracked to completion<br>- Overdue alerts generated |
| **Open Questions** | - None at this time |

---

## Conceptual Entity-Relationship Diagram

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  REGULATORY     │────────►│   FILING_TYPE   │         │  FILING_STATUS  │
│    FILING       │         │                 │         │                 │
└────────┬────────┘         └─────────────────┘         └─────────────────┘
         │
         ├──────────────────┬──────────────────┐
         ▼                  ▼                  ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│  REGULATORY     │ │  RATE_FILING    │ │  FORM_FILING    │
│   AUTHORITY     │ │                 │ │                 │
└─────────────────┘ └─────────────────┘ └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  REGULATORY     │────────►│  COMPLIANCE     │────────►│   COMPLIANCE    │
│  REQUIREMENT    │         │    STATUS       │         │   ASSESSMENT    │
└─────────────────┘         └─────────────────┘         └────────┬────────┘
                                                                 │
                                                                 ▼
                                                        ┌─────────────────┐
                                                        │   COMPLIANCE    │
                                                        │    FINDING      │
                                                        └────────┬────────┘
                                                                 │
                                                                 ▼
                                                        ┌─────────────────┐
                                                        │  REMEDIATION    │
                                                        │    ACTION       │
                                                        └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  REGULATORY     │────────►│  EXAMINATION    │         │  EXAMINATION    │
│  EXAMINATION    │         │     TYPE        │         │    FINDING      │
└─────────────────┘         └─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  PRODUCER       │────────►│  LICENSE_TYPE   │         │ LICENSE_STATUS  │
│   LICENSE       │         │                 │         │                 │
└────────┬────────┘         └─────────────────┘         └─────────────────┘
         │
         ▼
┌─────────────────┐         ┌─────────────────┐
│    LICENSE      │         │   COMPANY       │
│  APPOINTMENT    │         │    LICENSE      │
└─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│   AUDIT_TRAIL   │────────►│AUDIT_ACTION_TYPE│
│                 │         │                 │
└─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│ PRIVACY_CONSENT │────────►│CONSENT_PURPOSE  │         │DATA_SUBJECT     │
│                 │         │                 │         │   REQUEST       │
└─────────────────┘         └─────────────────┘         └────────┬────────┘
                                                                 │
                                                                 ▼
                                                        ┌─────────────────┐
                                                        │    DSR_TYPE     │
                                                        └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│  REGULATORY     │────────►│  REGULATORY     │
│   COMPLAINT     │         │ COMPLAINT_STATUS│
└─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│  COMPLIANCE     │         │  COMPLIANCE     │
│    POLICY       │         │   TRAINING      │
└─────────────────┘         └────────┬────────┘
                                     │
                                     ▼
                            ┌─────────────────┐
                            │   TRAINING      │
                            │  COMPLETION     │
                            └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│   SANCTIONS     │────────►│ SANCTIONS_LIST  │         │    AML_CASE     │
│   SCREENING     │         │                 │         │                 │
└─────────────────┘         └─────────────────┘         └────────┬────────┘
                                                                 │
                                                                 ▼
                                                        ┌─────────────────┐
                                                        │   SUSPICIOUS    │
                                                        │ ACTIVITY_REPORT │
                                                        └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│   COMPLIANCE    │────────►│   COMPLIANCE    │
│    CALENDAR     │  has    │ CALENDAR_EVENT  │
└─────────────────┘         └─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Regulatory Filing | Time/Calendar | Reporting Period | Filing for period |
| Regulatory Filing | Geography | Jurisdiction | Filed in jurisdiction |
| Producer License | Party/Customer | Producer/Agent | License held by producer |
| Producer License | Geography | State | Issued by state |
| Company License | Geography | State | Issued by state |
| Privacy Consent | Party/Customer | Customer | Consent from customer |
| Data Subject Request | Party/Customer | Customer | Request from customer |
| Regulatory Complaint | Customer Engagement | Complaint | Escalated complaint |
| Regulatory Complaint | Policy | Policy | Complaint regarding policy |
| Rate Filing | Product | Product | Rates for product |
| Sanctions Screening | Party/Customer | Party | Screening of party |
| AML Case | Party/Customer | Customer | Investigation of customer |
| Compliance Calendar | Time/Calendar | Calendar | Aligns with calendar |
| Audit Trail | All Domains | Various | Tracks changes to records |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Regulatory Filing | Transactional | Regulatory submission |
| Filing Type | Reference | Filing classification |
| Filing Status | Reference | Filing state |
| Regulatory Authority | Reference | Government regulator |
| Regulatory Requirement | Reference | Compliance obligation |
| Compliance Status | Transactional | Compliance posture |
| Compliance Assessment | Transactional | Compliance evaluation |
| Compliance Finding | Transactional | Issue identified |
| Remediation Action | Transactional | Corrective action |
| Regulatory Examination | Transactional | Regulator exam |
| Examination Type | Reference | Exam classification |
| Examination Finding | Transactional | Exam issue |
| Producer License | Master | Agent authorization |
| License Type | Reference | License classification |
| License Status | Reference | License state |
| License Appointment | Master | Producer-company relationship |
| Company License | Master | Company authorization |
| Audit Trail | Transactional | Activity log |
| Audit Action Type | Reference | Action classification |
| Privacy Consent | Transactional | Data processing consent |
| Consent Purpose | Reference | Consent classification |
| Data Subject Request | Transactional | Privacy rights request |
| DSR Type | Reference | DSR classification |
| Regulatory Complaint | Transactional | Regulator complaint |
| Regulatory Complaint Status | Reference | Complaint state |
| Rate Filing | Transactional | Rate approval submission |
| Form Filing | Transactional | Form approval submission |
| Compliance Policy | Reference | Internal policy |
| Compliance Training | Reference | Required training |
| Training Completion | Transactional | Training record |
| Sanctions Screening | Transactional | OFAC/sanctions check |
| Sanctions List | Reference | Sanctions source |
| AML Case | Transactional | Money laundering investigation |
| Suspicious Activity Report | Transactional | SAR filing |
| Compliance Calendar | Reference | Deadline schedule |
| Compliance Calendar Event | Transactional | Specific deadline |

**Total Entities: 36**

---

## Assumptions Log

1. All regulatory filings tracked with deadlines
2. Compliance requirements catalogued
3. Regular compliance assessments performed
4. Findings tracked to remediation
5. Producer and company licenses tracked
6. Audit trails maintained per regulations
7. Privacy consent and DSRs tracked
8. Regulatory complaints formally tracked
9. AML and sanctions screening in place
10. Compliance calendar maintained

---

## Open Questions

1. What filing types are required?
2. How are requirements identified and tracked?
3. What assessment frequency for compliance?
4. What retention period for audit trails?
5. What privacy regulations apply (CCPA, etc.)?
6. What jurisdictions require prior rate approval?
7. What sanctions screening frequency?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
