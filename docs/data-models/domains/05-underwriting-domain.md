# Underwriting Domain
## Logical Data Model - Entity Definitions

**Domain:** Underwriting  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Transaction & Event Domain

---

## Domain Overview

**Purpose:** Manage risk assessment, classification, and underwriting decisions for life insurance applications, including data collection, evaluation, and decision processes.

**Scope:** This domain encompasses the underwriting application process, risk assessment activities, medical examinations, third-party data sources, underwriting decisions, and evidence of insurability requirements.

**Business Value:**
- Risk selection and classification accuracy
- Underwriting cycle time optimization
- Mortality experience analysis
- Compliance with underwriting guidelines
- Support for automated and accelerated underwriting

---

## Entity Catalog

### APPLICATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Application |
| **Entity Definition** | A formal request for life insurance coverage submitted by a prospect or existing customer. The application captures information needed for underwriting evaluation and forms the basis for the insurance contract if approved. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Application Number |
| **Key Relationships** | - Submitted by Prospect/Customer (Party Domain)<br>- For specific Product (Product Domain)<br>- Results in Policy if approved (Policy Domain)<br>- Undergoes Risk Assessment<br>- Has Application Status<br>- May have one or more Application Questions |
| **Assumptions** | - Applications are immutable once submitted<br>- Amendments create new application versions |
| **Open Questions** | - How to handle partial/incomplete applications? |

---

### APPLICATION_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Application Status |
| **Entity Definition** | The current state of an Application in the underwriting process. Status tracks progress from submission through final decision. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Assigned to Application<br>- Has valid Status Transitions |
| **Assumptions** | - Core statuses: SUBMITTED, IN_REVIEW, PENDING_REQUIREMENTS, REFERRED, APPROVED, DECLINED, WITHDRAWN, INCOMPLETE |
| **Open Questions** | - Should sub-statuses be separate entities? |

---

### APPLICATION_STATUS_HISTORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Application Status History |
| **Entity Definition** | A historical record of all status changes for an Application, enabling workflow analysis, cycle time measurement, and audit trails. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Application Number + Status Change Timestamp |
| **Key Relationships** | - Belongs to Application<br>- Records From Status and To Status |
| **Assumptions** | - All transitions logged with timestamp and user/system |
| **Open Questions** | - None at this time |

---

### APPLICATION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Application Type |
| **Entity Definition** | A classification of Applications based on the nature of the insurance request, distinguishing between new business, reinstatement, conversion, or increase applications. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Application Type Code |
| **Key Relationships** | - Classifies Application |
| **Assumptions** | - Types: NEW_BUSINESS, REINSTATEMENT, CONVERSION, COVERAGE_INCREASE, RIDER_ADD |
| **Open Questions** | - None at this time |

---

### APPLICATION_CHANNEL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Application Channel |
| **Entity Definition** | The channel through which an Application was submitted, enabling analysis of channel performance and application quality by source. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Channel Code |
| **Key Relationships** | - Classifies Application source<br>- Links to Channel (Sales & Distribution Domain) |
| **Assumptions** | - Channels: WEBSITE, MOBILE_APP, CALL_CENTER, PAPER, PARTNER_API |
| **Open Questions** | - None at this time |

---

### PROPOSED_INSURED

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Proposed Insured |
| **Entity Definition** | The individual proposed for insurance coverage in an Application. Captures point-in-time information about the proposed insured including demographics and health disclosures at application. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Application Number |
| **Key Relationships** | - References Individual (Party Domain)<br>- Subject of Application<br>- Undergoes Risk Assessment |
| **Assumptions** | - Demographic data is captured at application time<br>- May differ from current Individual record |
| **Open Questions** | - None at this time |

---

### RISK_ASSESSMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Risk Assessment |
| **Entity Definition** | The comprehensive evaluation of mortality risk for a Proposed Insured, encompassing all data gathering, analysis, and risk scoring activities that inform the underwriting decision. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Assessment Identifier |
| **Key Relationships** | - Performed for one Application<br>- Uses Health Questionnaire responses<br>- Uses Medical Exam results<br>- Uses Third-Party Data<br>- Results in Risk Classification<br>- Produces Risk Score |
| **Assumptions** | - Assessment aggregates all risk data sources<br>- Multiple assessments possible if requirements change |
| **Open Questions** | - How to handle reassessments? |

---

### RISK_CLASSIFICATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Risk Classification |
| **Entity Definition** | The mortality risk category assigned to a Proposed Insured based on underwriting evaluation. Classification determines premium rates and may result in standard, preferred, or substandard ratings. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Classification Code |
| **Key Relationships** | - Assigned through Risk Assessment<br>- Maps to Underwriting Class (Product Domain)<br>- Determines premium rates |
| **Assumptions** | - Classifications: PREFERRED_PLUS, PREFERRED, STANDARD_PLUS, STANDARD, SUBSTANDARD (table rated), DECLINE |
| **Open Questions** | - How many table ratings for substandard? |

---

### RISK_FACTOR

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Risk Factor |
| **Entity Definition** | A specific characteristic, condition, or behavior that affects mortality risk assessment. Risk factors are evaluated individually and collectively to determine overall risk. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Risk Factor Code |
| **Key Relationships** | - Identified in Risk Assessment<br>- Has Risk Factor Category<br>- May contribute to Risk Score |
| **Assumptions** | - Factors include medical conditions, lifestyle, occupation, avocations<br>- Each factor has defined risk impact |
| **Open Questions** | - How frequently are risk factor definitions updated? |

---

### RISK_FACTOR_CATEGORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Risk Factor Category |
| **Entity Definition** | A high-level grouping of Risk Factors for analysis and reporting purposes. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Category Code |
| **Key Relationships** | - Groups Risk Factors |
| **Assumptions** | - Categories: MEDICAL, LIFESTYLE, OCCUPATIONAL, AVOCATIONAL, FAMILY_HISTORY, FINANCIAL |
| **Open Questions** | - None at this time |

---

### IDENTIFIED_RISK_FACTOR

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Identified Risk Factor |
| **Entity Definition** | A Risk Factor that has been identified for a specific Proposed Insured during the Risk Assessment process, including severity and impact. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Assessment Identifier + Risk Factor Code |
| **Key Relationships** | - Links Risk Assessment to Risk Factor<br>- Has severity/impact rating |
| **Assumptions** | - Multiple factors may be identified per assessment<br>- Factors may be additive or multiplicative in impact |
| **Open Questions** | - How are multiple factors combined? |

---

### HEALTH_QUESTIONNAIRE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Health Questionnaire |
| **Entity Definition** | A structured set of health and lifestyle questions completed by the Proposed Insured as part of the application process. Responses are used for risk assessment. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Questionnaire Identifier |
| **Key Relationships** | - Completed for one Application<br>- Contains Questionnaire Responses<br>- Input to Risk Assessment |
| **Assumptions** | - Questions may vary by product and coverage amount<br>- Responses are legally binding representations |
| **Open Questions** | - How many questionnaire versions exist? |

---

### QUESTIONNAIRE_RESPONSE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Questionnaire Response |
| **Entity Definition** | An individual answer to a specific question in the Health Questionnaire, capturing the response value and any required details. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Questionnaire Identifier + Question Identifier |
| **Key Relationships** | - Belongs to Health Questionnaire<br>- References Question Definition |
| **Assumptions** | - Responses may trigger follow-up questions<br>- Certain responses trigger additional requirements |
| **Open Questions** | - None at this time |

---

### QUESTION_DEFINITION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Question Definition |
| **Entity Definition** | A defined question in the underwriting questionnaire library, including question text, response type, and any rules for triggering follow-up questions or requirements. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Question Identifier |
| **Key Relationships** | - Used in Health Questionnaire<br>- May trigger Requirements<br>- Belongs to Question Category |
| **Assumptions** | - Questions are versioned for regulatory compliance<br>- Questions may be conditional based on prior answers |
| **Open Questions** | - None at this time |

---

### MEDICAL_EXAM

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Medical Exam |
| **Entity Definition** | A paramedical or medical examination conducted on the Proposed Insured to gather objective health information including vitals, lab work, and physical assessment. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Exam Identifier |
| **Key Relationships** | - Ordered for one Application<br>- Conducted by Exam Vendor<br>- Contains Exam Results<br>- Input to Risk Assessment |
| **Assumptions** | - Exams may be required based on age and coverage amount<br>- May include blood, urine, and physical measurements |
| **Open Questions** | - What exam thresholds apply by age/amount? |

---

### EXAM_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Exam Type |
| **Entity Definition** | A classification of Medical Exams based on the scope and components included. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Exam Type Code |
| **Key Relationships** | - Classifies Medical Exam |
| **Assumptions** | - Types: PARAMED, FULL_MEDICAL, LABS_ONLY, EKG, STRESS_TEST |
| **Open Questions** | - None at this time |

---

### EXAM_RESULT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Exam Result |
| **Entity Definition** | An individual measurement or finding from a Medical Exam, including the value, reference range, and any abnormal flags. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Exam Identifier + Result Type Code |
| **Key Relationships** | - Belongs to Medical Exam<br>- References Result Type |
| **Assumptions** | - Results are stored with units and reference ranges<br>- Abnormal values are flagged |
| **Open Questions** | - None at this time |

---

### EXAM_RESULT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Exam Result Type |
| **Entity Definition** | A defined type of measurement or finding that can be captured in a Medical Exam, including normal ranges and units. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Result Type Code |
| **Key Relationships** | - Classifies Exam Result |
| **Assumptions** | - Types: HEIGHT, WEIGHT, BLOOD_PRESSURE, CHOLESTEROL, GLUCOSE, LIVER_FUNCTION, etc. |
| **Open Questions** | - None at this time |

---

### EXAM_VENDOR

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Exam Vendor |
| **Entity Definition** | A third-party company that conducts medical examinations on behalf of the insurance company. Vendors are contracted to perform exams in specified geographic areas. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Vendor Identifier |
| **Key Relationships** | - Conducts Medical Exams<br>- Has Service Areas (Geography Domain) |
| **Assumptions** | - Multiple vendors may serve different regions<br>- Vendor performance is tracked |
| **Open Questions** | - How are vendors assigned to exams? |

---

### THIRD_PARTY_DATA_REQUEST

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Third-Party Data Request |
| **Entity Definition** | A request for external data from third-party sources to supplement underwriting information. Includes MIB, prescription history, motor vehicle records, and other data services. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Request Identifier |
| **Key Relationships** | - Made for one Application<br>- Sent to Third-Party Data Source<br>- Returns Third-Party Data Response<br>- Input to Risk Assessment |
| **Assumptions** | - Requests require applicant consent<br>- Multiple data sources may be queried |
| **Open Questions** | - Which third-party sources will be integrated? |

---

### THIRD_PARTY_DATA_SOURCE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Third-Party Data Source |
| **Entity Definition** | An external data provider that supplies information used in underwriting decisions. Each source provides specific types of risk-related data. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Source Identifier |
| **Key Relationships** | - Receives Third-Party Data Requests<br>- Provides Third-Party Data Responses |
| **Assumptions** | - Sources: MIB, PRESCRIPTION_HISTORY, MVR, CREDIT, ELECTRONIC_HEALTH_RECORDS |
| **Open Questions** | - None at this time |

---

### THIRD_PARTY_DATA_RESPONSE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Third-Party Data Response |
| **Entity Definition** | The data returned from a Third-Party Data Source in response to a request, including the raw data, status, and any findings or codes. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Response Identifier |
| **Key Relationships** | - Fulfills Third-Party Data Request<br>- Contains data findings<br>- Input to Risk Assessment |
| **Assumptions** | - Response may indicate no record found<br>- Data is stored in normalized format |
| **Open Questions** | - How long is third-party data retained? |

---

### MIB_CODE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | MIB Code |
| **Entity Definition** | A coded record from the Medical Information Bureau (MIB) indicating prior insurance application history and health conditions. MIB codes are industry-standard risk indicators. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Application Number + MIB Code Value |
| **Key Relationships** | - Returned in MIB Response<br>- Maps to Risk Factors<br>- Input to Risk Assessment |
| **Assumptions** | - MIB codes are standardized industry-wide<br>- Codes may require investigation |
| **Open Questions** | - None at this time |

---

### PRESCRIPTION_HISTORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Prescription History |
| **Entity Definition** | A record of prescription medications filled by the Proposed Insured, obtained from pharmacy databases. Prescriptions provide insight into medical conditions and treatments. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Application Number + Prescription Identifier |
| **Key Relationships** | - Returned in Prescription Data Response<br>- May indicate Risk Factors<br>- Input to Risk Assessment |
| **Assumptions** | - History typically covers 2-5 years<br>- Medications mapped to conditions |
| **Open Questions** | - What lookback period is used? |

---

### UNDERWRITING_DECISION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Underwriting Decision |
| **Entity Definition** | The formal decision made on an Application, indicating approval, decline, or offer with modified terms. The decision is based on the completed Risk Assessment. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Decision Identifier |
| **Key Relationships** | - Made for one Application<br>- Based on Risk Assessment<br>- Made by Decision Maker (automated or human)<br>- Results in offered or declined terms |
| **Assumptions** | - Decision may be subject to approval hierarchy<br>- Modified offers include ratings or exclusions |
| **Open Questions** | - What decisions require senior approval? |

---

### DECISION_OUTCOME

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Decision Outcome |
| **Entity Definition** | The type of underwriting decision rendered, indicating the overall result of the underwriting process. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Outcome Code |
| **Key Relationships** | - Classifies Underwriting Decision |
| **Assumptions** | - Outcomes: APPROVED_AS_APPLIED, APPROVED_MODIFIED, DECLINED, POSTPONED, INCOMPLETE |
| **Open Questions** | - None at this time |

---

### UNDERWRITING_EXCEPTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Underwriting Exception |
| **Entity Definition** | A deviation from standard underwriting guidelines that requires documented justification and appropriate approval. Exceptions enable flexibility while maintaining audit trails. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Exception Identifier |
| **Key Relationships** | - Related to Underwriting Decision<br>- Requires Exception Approval<br>- Has Exception Reason |
| **Assumptions** | - Exceptions require documented rationale<br>- Approval authority varies by exception type |
| **Open Questions** | - What exception approval levels exist? |

---

### UNDERWRITING_REQUIREMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Underwriting Requirement |
| **Entity Definition** | A specific document, exam, or information needed to complete the underwriting process. Requirements are ordered based on risk factors, coverage amount, and age. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Requirement Identifier |
| **Key Relationships** | - Ordered for one Application<br>- Has Requirement Type<br>- Has Requirement Status<br>- May be fulfilled by various sources |
| **Assumptions** | - Requirements may be ordered dynamically<br>- Outstanding requirements block decisions |
| **Open Questions** | - How are requirement waiver decisions made? |

---

### REQUIREMENT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Requirement Type |
| **Entity Definition** | A classification of Underwriting Requirements based on the type of information or evidence needed. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Requirement Type Code |
| **Key Relationships** | - Classifies Underwriting Requirement |
| **Assumptions** | - Types: APS (Attending Physician Statement), PARAMED, LABS, EKG, MVR, FINANCIAL_DOCS, etc. |
| **Open Questions** | - None at this time |

---

### REQUIREMENT_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Requirement Status |
| **Entity Definition** | The current state of an Underwriting Requirement in its fulfillment lifecycle. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Tracks Underwriting Requirement state |
| **Assumptions** | - Statuses: ORDERED, PENDING, RECEIVED, WAIVED, CANCELLED |
| **Open Questions** | - None at this time |

---

### UNDERWRITING_GUIDELINE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Underwriting Guideline |
| **Entity Definition** | A documented rule or policy that governs underwriting decisions, including requirements, limitations, and rating criteria for specific conditions or risk factors. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Guideline Identifier |
| **Key Relationships** | - Applied to Risk Assessment<br>- Referenced by Underwriting Decision<br>- Has effective date range |
| **Assumptions** | - Guidelines are versioned and dated<br>- Different guidelines may apply by product |
| **Open Questions** | - How are guidelines maintained and updated? |

---

### RISK_SCORE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Risk Score |
| **Entity Definition** | A numerical score calculated for a Proposed Insured representing their mortality risk level. Scores may be produced by predictive models or algorithms. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Score Identifier |
| **Key Relationships** | - Calculated for Risk Assessment<br>- Produced by Score Model<br>- Input to Underwriting Decision |
| **Assumptions** | - Multiple score types may exist (mortality, lapse, etc.)<br>- Scores have defined thresholds for action |
| **Open Questions** | - What scoring models will be used? |

---

### SCORE_MODEL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Score Model |
| **Entity Definition** | A predictive model or algorithm used to calculate Risk Scores. Models are versioned and validated for accuracy. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Model Identifier + Version |
| **Key Relationships** | - Produces Risk Scores<br>- Has validation metrics |
| **Assumptions** | - Models are subject to regulatory review<br>- Model versions tracked for reproducibility |
| **Open Questions** | - What model governance process exists? |

---

### ACCELERATED_UNDERWRITING_ELIGIBILITY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Accelerated Underwriting Eligibility |
| **Entity Definition** | The determination of whether an applicant qualifies for accelerated (simplified) underwriting without traditional medical exams, based on data-driven risk assessment. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Application Number |
| **Key Relationships** | - Determined for Application<br>- Based on Risk Score and criteria<br>- Affects Requirement determination |
| **Assumptions** | - Eligibility based on age, amount, and risk signals<br>- Non-eligible applicants go to traditional underwriting |
| **Open Questions** | - What are the accelerated UW criteria? |

---

## Conceptual Entity-Relationship Diagram

```
                                    ┌─────────────────┐
                                    │   INDIVIDUAL    │
                                    │ (Party Domain)  │
                                    └────────┬────────┘
                                             │ is proposed insured
                                             ▼
┌─────────────────┐    submitted    ┌─────────────────┐    results in   ┌─────────────────┐
│APPLICATION_TYPE │───────────────►│   APPLICATION   │───────────────►│     POLICY      │
└─────────────────┘                 │                 │                 │ (Policy Domain) │
                                    └────────┬────────┘                 └─────────────────┘
                                             │
         ┌───────────────┬──────────────────┼──────────────────┬──────────────────┐
         │               │                  │                  │                  │
         ▼               ▼                  ▼                  ▼                  ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────────┐ ┌──────────────┐
│ APPLICATION  │ │   HEALTH     │ │  RISK        │ │  UNDERWRITING    │ │  MEDICAL     │
│   STATUS     │ │QUESTIONNAIRE │ │ ASSESSMENT   │ │   REQUIREMENT    │ │    EXAM      │
│   HISTORY    │ │              │ │              │ │                  │ │              │
└──────────────┘ └──────┬───────┘ └──────┬───────┘ └────────┬─────────┘ └──────┬───────┘
                        │                │                  │                  │
                        ▼                │                  ▼                  ▼
               ┌──────────────┐          │         ┌──────────────┐    ┌──────────────┐
               │QUESTIONNAIRE │          │         │ REQUIREMENT  │    │ EXAM_RESULT  │
               │  RESPONSE    │          │         │    TYPE      │    │              │
               └──────────────┘          │         └──────────────┘    └──────┬───────┘
                                         │                                    │
                                         ▼                                    ▼
┌──────────────┐              ┌─────────────────┐               ┌──────────────────┐
│ IDENTIFIED   │◄─────────────│ RISK_ASSESSMENT │              │  EXAM_RESULT     │
│ RISK_FACTOR  │              │                 │               │     TYPE         │
└──────┬───────┘              └────────┬────────┘               └──────────────────┘
       │                               │
       ▼                               │
┌──────────────┐                       │         ┌─────────────────────┐
│ RISK_FACTOR  │                       │         │ THIRD_PARTY_DATA    │
│              │                       │         │     REQUEST         │
└──────┬───────┘                       │         └──────────┬──────────┘
       │                               │                    │
       ▼                               │                    ▼
┌──────────────┐                       │         ┌─────────────────────┐
│ RISK_FACTOR  │                       │         │ THIRD_PARTY_DATA    │
│  CATEGORY    │                       │         │     SOURCE          │
└──────────────┘                       │         └─────────────────────┘
                                       │
         ┌─────────────────────────────┼─────────────────────────────┐
         │                             │                             │
         ▼                             ▼                             ▼
┌─────────────────┐         ┌─────────────────┐           ┌─────────────────┐
│  RISK_SCORE     │         │  UNDERWRITING   │           │ ACCELERATED_UW  │
│                 │         │    DECISION     │           │   ELIGIBILITY   │
└────────┬────────┘         └────────┬────────┘           └─────────────────┘
         │                           │
         ▼                           ▼
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  SCORE_MODEL    │         │DECISION_OUTCOME │         │  UNDERWRITING   │
│                 │         │                 │         │   EXCEPTION     │
└─────────────────┘         └─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│   RISK         │◄─────────│ UNDERWRITING    │
│ CLASSIFICATION  │         │   GUIDELINE     │
└─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│   EXAM_TYPE    │───────►  │  MEDICAL_EXAM   │
└─────────────────┘         └─────────────────┘
                                    │
                                    ▼
                            ┌─────────────────┐
                            │   EXAM_VENDOR   │
                            └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│   MIB_CODE     │◄─────────│ THIRD_PARTY     │
│                 │         │ DATA_RESPONSE   │
└─────────────────┘         └────────┬────────┘
                                     │
┌─────────────────┐                  │
│  PRESCRIPTION  │◄──────────────────┘
│    HISTORY     │
└─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Application | Party/Customer | Prospect/Customer | Application submitted by prospect |
| Application | Product | Product | Application for specific product |
| Application | Policy | Policy | Approved application results in policy |
| Application | Sales & Distribution | Quote | Application follows quote acceptance |
| Proposed Insured | Party/Customer | Individual | Person to be insured |
| Risk Classification | Product | Underwriting Class | Classification determines rate class |
| Underwriting Decision | Policy | Policy | Decision determines policy issue |
| Medical Exam | Geography | Address | Exam location |
| Risk Score | Fraud Detection | Fraud Alert | Scores may trigger fraud review |
| Application | Compliance | Audit Trail | Application activities audited |
| Underwriting Guideline | Compliance | Regulatory Requirement | Guidelines reflect regulations |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Application | Transactional | Request for insurance coverage |
| Application Status | Reference | Application workflow state |
| Application Status History | Transactional | Status change log |
| Application Type | Reference | Application purpose classification |
| Application Channel | Reference | Submission channel |
| Proposed Insured | Transactional | Person to be insured |
| Risk Assessment | Transactional | Overall risk evaluation |
| Risk Classification | Reference | Mortality risk category |
| Risk Factor | Reference | Individual risk characteristic |
| Risk Factor Category | Reference | Risk factor grouping |
| Identified Risk Factor | Transactional | Factor found for applicant |
| Health Questionnaire | Transactional | Health questions and answers |
| Questionnaire Response | Transactional | Individual question answers |
| Question Definition | Reference | Question library |
| Medical Exam | Transactional | Physical examination |
| Exam Type | Reference | Exam scope classification |
| Exam Result | Transactional | Individual exam findings |
| Exam Result Type | Reference | Measurement type definition |
| Exam Vendor | Reference | Exam service provider |
| Third-Party Data Request | Transactional | External data request |
| Third-Party Data Source | Reference | External data provider |
| Third-Party Data Response | Transactional | External data received |
| MIB Code | Transactional | MIB findings |
| Prescription History | Transactional | Medication history |
| Underwriting Decision | Transactional | Final UW decision |
| Decision Outcome | Reference | Decision type |
| Underwriting Exception | Transactional | Guideline deviation |
| Underwriting Requirement | Transactional | Required evidence |
| Requirement Type | Reference | Requirement classification |
| Requirement Status | Reference | Requirement state |
| Underwriting Guideline | Reference | UW rules and policies |
| Risk Score | Transactional | Calculated risk score |
| Score Model | Reference | Scoring algorithm |
| Accelerated UW Eligibility | Transactional | Simplified UW qualification |

**Total Entities: 34**

---

## Assumptions Log

1. Applications are immutable once submitted
2. Multiple risk assessments may occur if requirements change
3. Medical exams are ordered based on age and amount thresholds
4. Third-party data requires applicant consent
5. Risk classifications follow industry-standard nomenclature
6. Underwriting guidelines are versioned and dated
7. Risk scores are produced by validated predictive models
8. Accelerated underwriting has defined eligibility criteria
9. All underwriting activities are logged for audit

---

## Open Questions

1. How to handle partial/incomplete applications?
2. Should application sub-statuses be separate entities?
3. How many table ratings for substandard classifications?
4. How frequently are risk factor definitions updated?
5. How many questionnaire versions exist?
6. What exam thresholds apply by age and amount?
7. How are exam vendors assigned to exams?
8. Which third-party data sources will be integrated?
9. How long is third-party data retained?
10. What prescription history lookback period is used?
11. What decisions require senior approval?
12. What exception approval levels exist?
13. How are requirement waivers decided?
14. How are underwriting guidelines maintained?
15. What scoring models will be used?
16. What model governance process exists?
17. What are the accelerated UW criteria?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
