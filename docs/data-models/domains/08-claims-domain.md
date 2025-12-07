# Claims Domain
## Logical Data Model - Entity Definitions

**Domain:** Claims  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Transaction & Event Domain

---

## Domain Overview

**Purpose:** Manage claims submission, processing, investigation, and settlement for life insurance death benefits and other policy payouts.

**Scope:** This domain encompasses death claims, claim adjudication, beneficiary payouts, claim reserves, and related financial transactions. It also covers policy surrenders and cash value withdrawals that result in payments.

**Business Value:**
- Claims processing efficiency
- Mortality experience analysis
- Reserve adequacy management
- Fraud detection support
- Beneficiary service quality
- Regulatory compliance for claims handling

---

## Entity Catalog

### CLAIM

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim |
| **Entity Definition** | A request for payment of benefits under a life insurance policy, triggered by the death of the insured or other covered event. Claims initiate the adjudication process leading to payment or denial. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Claim Number |
| **Key Relationships** | - Filed against Policy (Policy Domain)<br>- Filed by Claimant<br>- For Insured (Party Domain)<br>- Has Claim Status<br>- Has Claim Type<br>- Results in Claim Payouts |
| **Assumptions** | - Claim initiated upon notification of death<br>- One claim per death event per policy |
| **Open Questions** | - How are multi-policy claims for same insured handled? |

---

### CLAIM_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Type |
| **Entity Definition** | A classification of Claims based on the covered event or benefit being claimed. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Claim |
| **Assumptions** | - Types: DEATH, ACCELERATED_DEATH_BENEFIT, ACCIDENTAL_DEATH, WAIVER_OF_PREMIUM, TERMINAL_ILLNESS |
| **Open Questions** | - None at this time |

---

### CLAIM_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Status |
| **Entity Definition** | The current state of a Claim in the adjudication process, from initial notification through final disposition. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Assigned to Claim<br>- Has valid Status Transitions |
| **Assumptions** | - Core statuses: REPORTED, UNDER_REVIEW, PENDING_DOCUMENTS, IN_INVESTIGATION, APPROVED, DENIED, CLOSED, REOPENED |
| **Open Questions** | - Should contested claims have separate status track? |

---

### CLAIM_STATUS_HISTORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Status History |
| **Entity Definition** | A historical record of all status changes for a Claim, enabling workflow analysis, cycle time measurement, and audit trails. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Claim Number + Status Change Timestamp |
| **Key Relationships** | - Belongs to Claim<br>- Records From Status and To Status |
| **Assumptions** | - All transitions logged with timestamp and user |
| **Open Questions** | - None at this time |

---

### CLAIMANT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claimant |
| **Entity Definition** | The party who files a Claim and communicates with the insurance company during the claims process. The claimant may or may not be a beneficiary. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Claim Number + Claimant Identifier |
| **Key Relationships** | - Files Claim<br>- References Party (Party Domain)<br>- May be Beneficiary |
| **Assumptions** | - Claimant provides required documentation<br>- Claimant receives claim status communications |
| **Open Questions** | - None at this time |

---

### DEATH_CERTIFICATE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Death Certificate |
| **Entity Definition** | An official document certifying the death of the Insured, required for death claim processing. Death certificates provide cause of death and date of death. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Certificate Identifier |
| **Key Relationships** | - Submitted for Claim<br>- Issued by Jurisdiction (Geography Domain)<br>- Has Death Cause classification |
| **Assumptions** | - Original or certified copy required<br>- Certificate is validated for authenticity |
| **Open Questions** | - What validation is performed on death certificates? |

---

### CAUSE_OF_DEATH

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Cause of Death |
| **Entity Definition** | A standardized classification of the underlying cause of the insured's death, used for mortality analysis and claims adjudication. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Cause Code (ICD-10 based) |
| **Key Relationships** | - Recorded on Death Certificate<br>- Used for Mortality analysis<br>- May affect claim coverage |
| **Assumptions** | - Uses ICD-10 coding standard<br>- Mapped to broader cause categories for analysis |
| **Open Questions** | - None at this time |

---

### CLAIM_DOCUMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Document |
| **Entity Definition** | A document submitted or generated as part of the claims process, including death certificate, claim forms, medical records, and correspondence. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Document Identifier |
| **Key Relationships** | - Associated with Claim<br>- Has Document Type<br>- Has Receipt Date |
| **Assumptions** | - Documents stored electronically<br>- Document completeness tracked |
| **Open Questions** | - What is the document retention period? |

---

### CLAIM_DOCUMENT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Document Type |
| **Entity Definition** | A classification of Claim Documents based on their content and purpose in the claims process. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Document Type Code |
| **Key Relationships** | - Classifies Claim Document |
| **Assumptions** | - Types: DEATH_CERTIFICATE, CLAIM_FORM, CLAIMANT_ID, MEDICAL_RECORDS, AUTOPSY_REPORT, POLICE_REPORT, CORRESPONDENCE |
| **Open Questions** | - None at this time |

---

### CLAIM_REQUIREMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Requirement |
| **Entity Definition** | A specific document or information needed to process a Claim, with tracking of receipt and review status. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Requirement Identifier |
| **Key Relationships** | - Required for Claim<br>- Has Requirement Type<br>- Has Requirement Status |
| **Assumptions** | - Requirements may be standard or conditional<br>- Outstanding requirements delay processing |
| **Open Questions** | - None at this time |

---

### CLAIM_INVESTIGATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Investigation |
| **Entity Definition** | A formal inquiry conducted to verify the validity of a Claim, triggered by fraud indicators, contestability period, or coverage questions. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Investigation Identifier |
| **Key Relationships** | - Conducted for Claim<br>- Has Investigation Type<br>- Has Investigation Status<br>- May involve Third-Party Investigator<br>- Results in Investigation Finding |
| **Assumptions** | - Investigations required for contestable claims<br>- Investigation findings documented |
| **Open Questions** | - What triggers mandatory investigation? |

---

### INVESTIGATION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Investigation Type |
| **Entity Definition** | A classification of Claim Investigations based on the reason for investigation or scope of inquiry. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Claim Investigation |
| **Assumptions** | - Types: CONTESTABILITY, FRAUD_SUSPECTED, CAUSE_OF_DEATH, BENEFICIARY_DISPUTE, COVERAGE_QUESTION |
| **Open Questions** | - None at this time |

---

### INVESTIGATION_FINDING

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Investigation Finding |
| **Entity Definition** | The conclusion of a Claim Investigation, documenting findings and recommendation for claim disposition. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Finding Identifier |
| **Key Relationships** | - Results from Claim Investigation<br>- Has Finding Type<br>- Supports Claim Decision |
| **Assumptions** | - Findings are documented with supporting evidence<br>- Findings inform claim decision |
| **Open Questions** | - None at this time |

---

### CLAIM_DECISION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Decision |
| **Entity Definition** | The formal determination on a Claim, indicating approval, partial approval, or denial, along with the rationale and any benefit calculations. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Decision Identifier |
| **Key Relationships** | - Made for Claim<br>- Has Decision Outcome<br>- Made by Decision Maker<br>- May have Denial Reason |
| **Assumptions** | - Decisions require appropriate authority<br>- Denials require documented rationale |
| **Open Questions** | - What decision authority levels exist? |

---

### DECISION_OUTCOME

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Decision Outcome |
| **Entity Definition** | The type of determination made on a Claim - approval, partial approval, or denial. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Outcome Code |
| **Key Relationships** | - Classifies Claim Decision |
| **Assumptions** | - Outcomes: APPROVED_FULL, APPROVED_PARTIAL, DENIED, PENDING_LITIGATION |
| **Open Questions** | - None at this time |

---

### DENIAL_REASON

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Denial Reason |
| **Entity Definition** | The reason for denying a Claim, documenting the specific grounds for non-payment. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Reason Code |
| **Key Relationships** | - Classifies Claim Decision denial |
| **Assumptions** | - Reasons: MATERIAL_MISREPRESENTATION, SUICIDE_EXCLUSION, POLICY_LAPSED, COVERAGE_EXCLUDED, FRAUD, BENEFICIARY_INELIGIBLE |
| **Open Questions** | - None at this time |

---

### CLAIM_PAYOUT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Payout |
| **Entity Definition** | A payment made to a beneficiary or other payee as settlement of a Claim. Multiple payouts may be made for a single claim (multiple beneficiaries). |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Payout Identifier |
| **Key Relationships** | - Made for Claim<br>- Paid to Beneficiary (Party Domain)<br>- Has Payout Type<br>- Uses Payment Method |
| **Assumptions** | - Payout amounts per beneficiary designation percentage<br>- Various payout options available |
| **Open Questions** | - What payout options are offered? |

---

### PAYOUT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payout Type |
| **Entity Definition** | A classification of Claim Payouts based on the benefit type or payment structure. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Claim Payout |
| **Assumptions** | - Types: LUMP_SUM, INSTALLMENT, ANNUITY, RETAINED_ASSET_ACCOUNT |
| **Open Questions** | - None at this time |

---

### PAYOUT_OPTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payout Option |
| **Entity Definition** | A settlement option chosen by or offered to a beneficiary for receiving death benefit proceeds, such as lump sum or structured payments. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Option Code |
| **Key Relationships** | - Selected for Claim Payout |
| **Assumptions** | - Options defined by product and state<br>- Beneficiary may have choice |
| **Open Questions** | - None at this time |

---

### BENEFICIARY_VERIFICATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Beneficiary Verification |
| **Entity Definition** | The process of confirming the identity and eligibility of a beneficiary to receive claim proceeds, including identity verification and documentation review. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Verification Identifier |
| **Key Relationships** | - Performed for Beneficiary<br>- Part of Claim processing<br>- Has Verification Status |
| **Assumptions** | - All beneficiaries verified before payout<br>- Verification requirements vary by amount |
| **Open Questions** | - What are verification requirements by payout amount? |

---

### CLAIM_RESERVE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Reserve |
| **Entity Definition** | The estimated liability amount set aside for an open Claim, representing the expected payout. Reserves are adjusted as claim information develops. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Reserve Identifier |
| **Key Relationships** | - Established for Claim<br>- Has Reserve Type<br>- Links to Financial reserves (Financial Domain) |
| **Assumptions** | - Initial reserve set at claim notification<br>- Reserve adjusted as information develops |
| **Open Questions** | - What is the initial reserving methodology? |

---

### RESERVE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Reserve Type |
| **Entity Definition** | A classification of Claim Reserves based on the type of liability being reserved. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Claim Reserve |
| **Assumptions** | - Types: CASE_RESERVE, IBNR (Incurred But Not Reported), REOPENED_RESERVE |
| **Open Questions** | - None at this time |

---

### RESERVE_CHANGE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Reserve Change |
| **Entity Definition** | A change to the Claim Reserve amount, recording adjustments as claim information develops or decisions are made. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Change Identifier |
| **Key Relationships** | - Adjusts Claim Reserve<br>- Has Change Reason |
| **Assumptions** | - All changes documented with rationale<br>- Reserve history maintained |
| **Open Questions** | - None at this time |

---

### CLAIM_APPEAL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Appeal |
| **Entity Definition** | A formal request by a claimant to reconsider a Claim Decision, typically following a denial. Appeals initiate a review process. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Appeal Identifier |
| **Key Relationships** | - Filed for Claim<br>- Filed by Claimant<br>- Has Appeal Status<br>- Results in Appeal Decision |
| **Assumptions** | - Appeals have defined timeframes<br>- Appeal review independent of original decision |
| **Open Questions** | - What is the appeal filing window? |

---

### APPEAL_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Appeal Status |
| **Entity Definition** | The current state of a Claim Appeal in the review process. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Assigned to Claim Appeal |
| **Assumptions** | - Statuses: FILED, UNDER_REVIEW, ADDITIONAL_INFO_REQUESTED, UPHELD, REVERSED, WITHDRAWN |
| **Open Questions** | - None at this time |

---

### CONTESTABILITY_REVIEW

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Contestability Review |
| **Entity Definition** | A review of the underwriting file for a Claim within the contestability period, checking for material misrepresentation on the application. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Review Identifier |
| **Key Relationships** | - Conducted for Claim<br>- Reviews Application (Underwriting Domain)<br>- Has Review Finding |
| **Assumptions** | - Required for claims in first 2 years<br>- May result in rescission or denial |
| **Open Questions** | - None at this time |

---

### INTERPLEADER

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Interpleader |
| **Entity Definition** | A legal action filed by the insurance company when there is a dispute among potential beneficiaries, depositing funds with the court to determine rightful recipient. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Interpleader Identifier |
| **Key Relationships** | - Filed for Claim<br>- Involves multiple Beneficiary claimants<br>- Has Court jurisdiction |
| **Assumptions** | - Filed to avoid multiple liability<br>- Funds held pending court decision |
| **Open Questions** | - None at this time |

---

### CLAIM_LITIGATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Claim Litigation |
| **Entity Definition** | A lawsuit filed by a claimant or beneficiary disputing a Claim Decision or seeking additional damages. Litigation is tracked for legal and financial management. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Litigation Identifier |
| **Key Relationships** | - Related to Claim<br>- Filed by Claimant/Beneficiary<br>- Has Litigation Status<br>- Has Legal Counsel |
| **Assumptions** | - Litigation handled by legal department<br>- Reserves adjusted for litigation exposure |
| **Open Questions** | - None at this time |

---

### SURRENDER_REQUEST

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Surrender Request |
| **Entity Definition** | A request by a policy owner to surrender a permanent life policy in exchange for the cash surrender value. Not technically a claim but results in policy payout. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Request Identifier |
| **Key Relationships** | - Submitted for Policy (Policy Domain)<br>- Results in Surrender (Policy Domain)<br>- Generates Payout |
| **Assumptions** | - Surrender value calculated per policy terms<br>- Surrender charges may apply |
| **Open Questions** | - What is the surrender processing SLA? |

---

### WITHDRAWAL_REQUEST

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Withdrawal Request |
| **Entity Definition** | A request by a policy owner to withdraw a portion of the cash value from a permanent life policy. Withdrawals reduce policy value without terminating coverage. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Request Identifier |
| **Key Relationships** | - Submitted for Policy (Policy Domain)<br>- Reduces Policy Value<br>- Generates Payout |
| **Assumptions** | - Maximum withdrawal limited by policy value<br>- May have tax implications |
| **Open Questions** | - What withdrawal limits apply? |

---

## Conceptual Entity-Relationship Diagram

```
                                    ┌─────────────────┐
                                    │     POLICY      │
                                    │ (Policy Domain) │
                                    └────────┬────────┘
                                             │ filed against
                                             ▼
┌─────────────────┐              ┌─────────────────┐              ┌─────────────────┐
│   CLAIMANT      │─────────────►│     CLAIM       │◄─────────────│  CLAIM_TYPE     │
│                 │   files      │                 │  classified  │                 │
└─────────────────┘              └────────┬────────┘  by          └─────────────────┘
                                          │
          ┌───────────────┬───────────────┼───────────────┬───────────────┐
          │               │               │               │               │
          ▼               ▼               ▼               ▼               ▼
┌─────────────────┐ ┌───────────┐ ┌───────────────┐ ┌───────────┐ ┌───────────────┐
│ CLAIM_STATUS    │ │  CLAIM    │ │    CLAIM      │ │  CLAIM    │ │   CLAIM       │
│   HISTORY       │ │ DOCUMENT  │ │  REQUIREMENT  │ │ RESERVE   │ │ INVESTIGATION │
└─────────────────┘ └─────┬─────┘ └───────────────┘ └─────┬─────┘ └───────┬───────┘
                          │                               │               │
                          ▼                               ▼               ▼
                   ┌───────────────┐             ┌──────────────┐ ┌───────────────┐
                   │CLAIM_DOCUMENT │             │RESERVE_CHANGE│ │ INVESTIGATION │
                   │    TYPE       │             │              │ │   FINDING     │
                   └───────────────┘             └──────────────┘ └───────────────┘

┌─────────────────┐              ┌─────────────────┐              ┌─────────────────┐
│ DEATH_CERTIF.   │─────────────►│     CLAIM       │◄─────────────│ CLAIM_DECISION  │
│                 │  submitted   │                 │  made for    │                 │
└────────┬────────┘  for         └─────────────────┘              └────────┬────────┘
         │                                                                 │
         ▼                                                                 ▼
┌─────────────────┐                                               ┌─────────────────┐
│ CAUSE_OF_DEATH  │                                               │DECISION_OUTCOME │
│                 │                                               │                 │
└─────────────────┘                                               └────────┬────────┘
                                                                           │
                                                                           ▼
                                                                  ┌─────────────────┐
                                                                  │ DENIAL_REASON   │
                                                                  │                 │
                                                                  └─────────────────┘

┌─────────────────┐              ┌─────────────────┐              ┌─────────────────┐
│   BENEFICIARY   │◄─────────────│  CLAIM_PAYOUT   │─────────────►│  PAYOUT_TYPE    │
│ (Party Domain)  │  paid to     │                 │  classified  │                 │
└─────────────────┘              └─────────────────┘  by          └─────────────────┘
         ▲                               │
         │                               │
┌────────┴────────┐                      ▼
│  BENEFICIARY    │              ┌─────────────────┐
│  VERIFICATION   │              │  PAYOUT_OPTION  │
└─────────────────┘              └─────────────────┘

┌─────────────────┐              ┌─────────────────┐              ┌─────────────────┐
│  CLAIM_APPEAL   │─────────────►│  APPEAL_STATUS  │              │CONTESTABILITY   │
│                 │              │                 │              │    REVIEW       │
└─────────────────┘              └─────────────────┘              └─────────────────┘

┌─────────────────┐              ┌─────────────────┐
│  INTERPLEADER   │              │CLAIM_LITIGATION │
│                 │              │                 │
└─────────────────┘              └─────────────────┘

┌─────────────────┐              ┌─────────────────┐
│   SURRENDER     │              │   WITHDRAWAL    │
│    REQUEST      │              │    REQUEST      │
└─────────────────┘              └─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Claim | Policy | Policy | Claim filed against policy |
| Claim | Party/Customer | Insured | Claim for insured's death |
| Claimant | Party/Customer | Party | Claimant is a party |
| Claim Payout | Party/Customer | Beneficiary | Payout to beneficiary |
| Claim Payout | Premium & Billing | Payment | Payout as payment transaction |
| Claim Reserve | Financial Performance | Reserve | Reserve is financial liability |
| Contestability Review | Underwriting | Application | Reviews original application |
| Contestability Review | Time/Calendar | Contestability Period | Within contestability window |
| Claim Investigation | Fraud Detection | Fraud Alert | Investigation may detect fraud |
| Death Certificate | Geography | State | Certificate issued by state |
| Surrender Request | Policy | Surrender | Creates surrender transaction |
| Surrender Request | Policy | Policy Value | Pays out cash value |
| Withdrawal Request | Policy | Policy Value | Reduces cash value |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Claim | Transactional | Benefit request |
| Claim Type | Reference | Claim classification |
| Claim Status | Reference | Processing state |
| Claim Status History | Transactional | Status change log |
| Claimant | Transactional | Claim filing party |
| Death Certificate | Transactional | Death documentation |
| Cause of Death | Reference | Death cause classification |
| Claim Document | Transactional | Supporting documents |
| Claim Document Type | Reference | Document classification |
| Claim Requirement | Transactional | Required information |
| Claim Investigation | Transactional | Validity inquiry |
| Investigation Type | Reference | Investigation classification |
| Investigation Finding | Transactional | Investigation conclusion |
| Claim Decision | Transactional | Claim determination |
| Decision Outcome | Reference | Decision classification |
| Denial Reason | Reference | Denial justification |
| Claim Payout | Transactional | Benefit payment |
| Payout Type | Reference | Payment classification |
| Payout Option | Reference | Settlement option |
| Beneficiary Verification | Transactional | Identity confirmation |
| Claim Reserve | Transactional | Liability estimate |
| Reserve Type | Reference | Reserve classification |
| Reserve Change | Transactional | Reserve adjustment |
| Claim Appeal | Transactional | Decision reconsideration |
| Appeal Status | Reference | Appeal state |
| Contestability Review | Transactional | Application review |
| Interpleader | Transactional | Beneficiary dispute filing |
| Claim Litigation | Transactional | Lawsuit tracking |
| Surrender Request | Transactional | Cash-out request |
| Withdrawal Request | Transactional | Partial cash withdrawal |

**Total Entities: 30**

---

## Assumptions Log

1. One claim per death event per policy
2. Claim initiated upon notification of death
3. All beneficiaries verified before payout
4. Contestability review required for claims in first 2 years
5. Investigations required for certain claim types
6. Reserve set at claim notification
7. Appeals have defined timeframes
8. Documents stored electronically
9. ICD-10 used for cause of death coding

---

## Open Questions

1. How are multi-policy claims for same insured handled?
2. Should contested claims have separate status track?
3. What validation is performed on death certificates?
4. What is the document retention period for claims?
5. What triggers mandatory investigation?
6. What decision authority levels exist?
7. What payout options are offered to beneficiaries?
8. What are verification requirements by payout amount?
9. What is the initial reserving methodology?
10. What is the appeal filing window?
11. What is the surrender processing SLA?
12. What withdrawal limits apply?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
