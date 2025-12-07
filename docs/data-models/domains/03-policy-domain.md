# Policy Domain
## Logical Data Model - Entity Definitions

**Domain:** Policy  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Core Foundation Domain

---

## Domain Overview

**Purpose:** Manage the lifecycle and details of insurance policy contracts, from issuance through termination, including all coverage details, modifications, and key policy events.

**Scope:** This domain encompasses policy contracts, coverage configurations, beneficiary designations, policy status tracking, modifications, endorsements, and policy value components (for permanent products).

**Business Value:**
- Complete policy lifecycle visibility
- Enable in-force and persistency analytics
- Support policy servicing operations
- Foundation for premium, claims, and financial analysis
- Regulatory compliance and reporting

---

## Entity Catalog

### POLICY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy |
| **Entity Definition** | A legally binding insurance contract between the insurance company and the policy owner, providing life insurance coverage for a designated insured person. The policy defines coverage terms, benefits, premiums, and conditions. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Policy Number |
| **Key Relationships** | - Issued for one Product Version (Product Domain)<br>- Has one Policy Owner (Party Domain)<br>- Has one Insured (Party Domain)<br>- Has one or more Coverages<br>- Has one or more Beneficiary Designations<br>- Has current Policy Status<br>- May have one or more Policy Riders |
| **Assumptions** | - Policy Number is unique and immutable<br>- A policy has exactly one insured person<br>- Joint policies are not currently supported |
| **Open Questions** | - Will joint life policies be offered in the future? |

---

### COVERAGE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Coverage |
| **Entity Definition** | The death benefit protection provided by a Policy, specifying the face amount and coverage details. For policies with multiple coverage components (base + riders), each is tracked separately. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Policy Number + Coverage Type Code |
| **Key Relationships** | - Belongs to exactly one Policy<br>- References Coverage Type<br>- May be modified by Coverage Change |
| **Assumptions** | - Base coverage and rider coverage are tracked separately<br>- Face amount may change over policy life |
| **Open Questions** | - How to handle decreasing term coverage amounts? |

---

### COVERAGE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Coverage Type |
| **Entity Definition** | A classification of Coverage indicating whether it is base policy coverage or a specific type of rider coverage. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Coverage Type Code |
| **Key Relationships** | - Classifies Coverage |
| **Assumptions** | - Core types: BASE_DEATH_BENEFIT, ACCIDENTAL_DEATH, CHILD_TERM, WAIVER_OF_PREMIUM, etc. |
| **Open Questions** | - None at this time |

---

### POLICY_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Status |
| **Entity Definition** | The current state of a Policy in its lifecycle. Status changes are triggered by business events such as premium payment, lapse, death, surrender, or administrative actions. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Assigned to Policy<br>- Has valid Status Transitions |
| **Assumptions** | - Core statuses: PENDING, ACTIVE, LAPSED, GRACE_PERIOD, REINSTATED, SURRENDERED, MATURED, DEATH_CLAIM, TERMINATED |
| **Open Questions** | - Should pending sub-statuses be separate entities? |

---

### POLICY_STATUS_HISTORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Status History |
| **Entity Definition** | A historical record of all status changes for a Policy, capturing when each status was effective and the reason for the change. Enables lifecycle analysis and audit. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Status Change Timestamp |
| **Key Relationships** | - Belongs to Policy<br>- References From Status and To Status<br>- May reference triggering Event |
| **Assumptions** | - All status transitions are logged with timestamp and reason<br>- History is immutable |
| **Open Questions** | - None at this time |

---

### POLICY_OWNER

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Owner |
| **Entity Definition** | The Party who owns the policy and has legal rights to modify the policy, designate beneficiaries, and receive policy values. The owner is responsible for premium payments. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Policy Number + Ownership Effective Date |
| **Key Relationships** | - References Party (Party Domain)<br>- Assigned to Policy<br>- May change through Ownership Transfer |
| **Assumptions** | - Owner may be different from Insured<br>- Corporate/trust ownership is supported |
| **Open Questions** | - How to handle minor owners with custodians? |

---

### INSURED

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Insured |
| **Entity Definition** | The Individual whose life is covered by the insurance policy. The death of the Insured triggers the death benefit payment. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Policy Number (one insured per policy) |
| **Key Relationships** | - References Individual (Party Domain)<br>- Assigned to exactly one Policy<br>- Subject of Underwriting assessment |
| **Assumptions** | - Insured cannot be changed after policy issue<br>- Insured must be an Individual, not an Organization |
| **Open Questions** | - None at this time |

---

### PAYOR

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payor |
| **Entity Definition** | The Party responsible for making premium payments on a Policy. The Payor may be the same as the Owner or a different party (e.g., parent paying for child's policy). |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Policy Number + Payor Effective Date |
| **Key Relationships** | - References Party (Party Domain)<br>- Assigned to Policy<br>- Makes Payments (Premium & Billing Domain) |
| **Assumptions** | - Default Payor is the Policy Owner<br>- Payor can be changed |
| **Open Questions** | - None at this time |

---

### BENEFICIARY_DESIGNATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Beneficiary Designation |
| **Entity Definition** | The specification of who will receive death benefit proceeds from a Policy, including the beneficiary party, designation level (primary/contingent), and percentage allocation. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Policy Number + Beneficiary Identifier + Designation Level + Effective Date |
| **Key Relationships** | - Belongs to Policy<br>- References Beneficiary (Party Domain)<br>- Has Designation Level |
| **Assumptions** | - Multiple beneficiaries can share proceeds by percentage<br>- Contingent beneficiaries receive if primary cannot<br>- Designations can be revocable or irrevocable |
| **Open Questions** | - How to handle per stirpes designations? |

---

### DESIGNATION_LEVEL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Designation Level |
| **Entity Definition** | A classification indicating the priority order of beneficiaries - primary beneficiaries receive first, contingent receive if primary cannot. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Level Code |
| **Key Relationships** | - Classifies Beneficiary Designation |
| **Assumptions** | - Levels: PRIMARY, CONTINGENT, TERTIARY |
| **Open Questions** | - None at this time |

---

### POLICY_RIDER

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Rider |
| **Entity Definition** | An optional benefit added to a base Policy, representing the attachment of a Rider product to provide supplemental coverage. Tracks rider-specific terms and status. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Policy Number + Rider Identifier |
| **Key Relationships** | - Attached to exactly one Policy<br>- References Rider (Product Domain)<br>- Has own Coverage amount<br>- Has own Status |
| **Assumptions** | - Riders can be added at issue or post-issue<br>- Riders may have different termination dates than base policy |
| **Open Questions** | - Can riders be cancelled independently? |

---

### POLICY_TERM

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Term |
| **Entity Definition** | For term life policies, the defined period of coverage from effective date to expiration. Captures the term length selected and key term dates. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Policy Number |
| **Key Relationships** | - Defines term for Policy<br>- References Term Length (Product Domain) |
| **Assumptions** | - Term is set at issue and typically cannot be changed<br>- Conversion options may be available before term expiration |
| **Open Questions** | - How to handle term renewals vs. new policies? |

---

### POLICY_VALUE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Value |
| **Entity Definition** | For permanent life policies, the accumulated cash value and related value components as of a specific date. Includes surrender value, loan value, and accumulated interest. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Valuation Date |
| **Key Relationships** | - Belongs to Policy (permanent products only)<br>- Used for Surrender/Loan transactions |
| **Assumptions** | - Values are calculated periodically (monthly or per statement)<br>- Term policies do not have cash values |
| **Open Questions** | - What valuation frequency will be used? |

---

### POLICY_LOAN

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Loan |
| **Entity Definition** | A loan taken against the cash value of a permanent life policy. The loan accrues interest and reduces the death benefit if not repaid. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Loan Identifier |
| **Key Relationships** | - Taken against one Policy<br>- Reduces Policy Value<br>- Has Loan Repayments |
| **Assumptions** | - Loan cannot exceed available loan value<br>- Interest compounds until repaid |
| **Open Questions** | - What loan interest rate methodology will be used? |

---

### POLICY_LOAN_REPAYMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Loan Repayment |
| **Entity Definition** | A payment made to reduce the outstanding balance of a Policy Loan, including allocation between principal and interest. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Repayment Identifier |
| **Key Relationships** | - Applied to one Policy Loan |
| **Assumptions** | - Repayments are optional<br>- Partial repayments allowed |
| **Open Questions** | - None at this time |

---

### POLICY_MODIFICATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Modification |
| **Entity Definition** | A change to an existing Policy that alters its terms, coverage, or parties. Modifications are owner-initiated changes that may require underwriting approval. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Modification Identifier |
| **Key Relationships** | - Applied to one Policy<br>- References Modification Type<br>- May require Underwriting (Underwriting Domain)<br>- Results in Policy change |
| **Assumptions** | - All modifications are logged with effective dates<br>- Some modifications may change premiums |
| **Open Questions** | - What modifications require new underwriting? |

---

### MODIFICATION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Modification Type |
| **Entity Definition** | A classification of Policy Modifications based on what aspect of the policy is being changed. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Modification Type Code |
| **Key Relationships** | - Classifies Policy Modification |
| **Assumptions** | - Core types: FACE_AMOUNT_CHANGE, BENEFICIARY_CHANGE, OWNER_CHANGE, PAYOR_CHANGE, ADDRESS_CHANGE, RIDER_ADD, RIDER_REMOVE, MODE_CHANGE |
| **Open Questions** | - None at this time |

---

### ENDORSEMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Endorsement |
| **Entity Definition** | A formal amendment to a Policy that documents changes to terms, conditions, or coverage. Endorsements become part of the policy contract and are legally binding. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Endorsement Identifier |
| **Key Relationships** | - Attached to one Policy<br>- Results from Policy Modification<br>- References Endorsement Type |
| **Assumptions** | - Endorsements are numbered sequentially per policy<br>- Endorsement documents are retained |
| **Open Questions** | - None at this time |

---

### ENDORSEMENT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Endorsement Type |
| **Entity Definition** | A classification of Endorsements based on the nature of the policy amendment. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Endorsement Type Code |
| **Key Relationships** | - Classifies Endorsement |
| **Assumptions** | - Types align with Modification Types that generate endorsements |
| **Open Questions** | - None at this time |

---

### POLICY_ANNIVERSARY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Anniversary |
| **Entity Definition** | An annual milestone date for a Policy, typically the anniversary of the issue date. Anniversaries may trigger rate changes, value calculations, or renewal activities. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Policy Number + Anniversary Year |
| **Key Relationships** | - Belongs to Policy<br>- Links to Date (Time/Calendar Domain)<br>- May trigger Events |
| **Assumptions** | - Anniversary date calculated from policy effective date<br>- Used for age-based calculations |
| **Open Questions** | - None at this time |

---

### GRACE_PERIOD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Grace Period |
| **Entity Definition** | A time period following a premium due date during which coverage remains in force despite non-payment. Grace periods provide policyholders opportunity to make late payments. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Grace Period Start Date |
| **Key Relationships** | - Triggered for Policy<br>- Has defined duration<br>- Ends with Payment or Lapse |
| **Assumptions** | - Grace period is typically 30-31 days<br>- Coverage continues during grace period |
| **Open Questions** | - Does grace period vary by product or state? |

---

### REINSTATEMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Reinstatement |
| **Entity Definition** | The process of restoring a lapsed Policy to active status. Reinstatement typically requires payment of back premiums and may require evidence of insurability. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Reinstatement Identifier |
| **Key Relationships** | - Restores one Policy<br>- May require Underwriting (Underwriting Domain)<br>- Requires Payment (Premium & Billing Domain) |
| **Assumptions** | - Reinstatement window is typically 3-5 years from lapse<br>- Medical underwriting may be required |
| **Open Questions** | - What is the reinstatement window by product? |

---

### CONVERSION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Conversion |
| **Entity Definition** | The exercise of a conversion option allowing a term policy to be converted to a permanent policy without new underwriting, based on the original underwriting class. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Conversion Identifier |
| **Key Relationships** | - Converts one Policy (term)<br>- Creates new Policy (permanent)<br>- Preserves Underwriting Class |
| **Assumptions** | - Conversion must occur before policy expiration or age limit<br>- Converted policy uses attained age rates |
| **Open Questions** | - What products are available for conversion? |

---

### SURRENDER

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Surrender |
| **Entity Definition** | The voluntary termination of a permanent life policy by the owner in exchange for the cash surrender value. Surrender ends the insurance coverage. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Surrender Identifier |
| **Key Relationships** | - Terminates one Policy<br>- Pays out Surrender Value<br>- Creates Payment (Premium & Billing Domain) |
| **Assumptions** | - Surrender charges may apply in early policy years<br>- Outstanding loans reduce surrender value |
| **Open Questions** | - What are the surrender charge schedules? |

---

### FREE_LOOK_PERIOD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Free Look Period |
| **Entity Definition** | A regulatory-mandated period after policy delivery during which the policyholder can cancel the policy for a full refund of premiums paid. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Free Look Start Date |
| **Key Relationships** | - Applies to newly issued Policy<br>- Duration varies by State (Geography Domain) |
| **Assumptions** | - Typically 10-30 days depending on state<br>- Cancellation during free look is not a lapse |
| **Open Questions** | - None at this time |

---

### POLICY_DOCUMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Document |
| **Entity Definition** | A document associated with a Policy, including the original contract, endorsements, correspondence, and supporting materials. Documents are retained for compliance and service. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Document Identifier |
| **Key Relationships** | - Associated with one Policy<br>- Has Document Type<br>- Supports Compliance requirements (Compliance Domain) |
| **Assumptions** | - Documents are stored electronically<br>- Retention periods vary by document type |
| **Open Questions** | - What is the document retention policy? |

---

### DOCUMENT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Document Type |
| **Entity Definition** | A classification of Policy Documents based on their content and purpose. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Document Type Code |
| **Key Relationships** | - Classifies Policy Document |
| **Assumptions** | - Core types: POLICY_CONTRACT, ENDORSEMENT, APPLICATION, CORRESPONDENCE, ID_VERIFICATION, MEDICAL_RECORDS |
| **Open Questions** | - None at this time |

---

## Conceptual Entity-Relationship Diagram

```
                                    ┌─────────────────┐
                                    │  PRODUCT_       │
                                    │  VERSION        │
                                    │ (Product Domain)│
                                    └────────┬────────┘
                                             │ issued for
                                             ▼
┌─────────────────┐    owns    ┌─────────────────┐    insures    ┌─────────────────┐
│  POLICY_OWNER   │───────────►│     POLICY      │◄──────────────│    INSURED      │
│ (Party Domain)  │            │                 │               │ (Party Domain)  │
└─────────────────┘            └────────┬────────┘               └─────────────────┘
                                        │
         ┌──────────────┬───────────────┼───────────────┬──────────────┐
         │              │               │               │              │
         ▼              ▼               ▼               ▼              ▼
┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│  COVERAGE   │ │BENEFICIARY  │ │POLICY_RIDER │ │POLICY_STATUS│ │  POLICY     │
│             │ │DESIGNATION  │ │             │ │  HISTORY    │ │   TERM      │
└──────┬──────┘ └──────┬──────┘ └──────┬──────┘ └─────────────┘ └─────────────┘
       │               │               │
       ▼               ▼               ▼
┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│ COVERAGE    │ │DESIGNATION  │ │   RIDER     │
│   TYPE      │ │   LEVEL     │ │(Product Dom)│
└─────────────┘ └─────────────┘ └─────────────┘

┌─────────────────┐                             ┌─────────────────┐
│     PAYOR       │────────────────────────────►│     POLICY      │
│ (Party Domain)  │       pays for              │                 │
└─────────────────┘                             └─────────────────┘

┌─────────────────┐        ┌─────────────────┐        ┌─────────────────┐
│    POLICY       │───────►│     POLICY      │        │  MODIFICATION   │
│  MODIFICATION   │        │                 │        │     TYPE        │
└────────┬────────┘        └─────────────────┘        └─────────────────┘
         │                                                     ▲
         │ generates                                           │
         ▼                                                     │
┌─────────────────┐        ┌─────────────────┐                │
│  ENDORSEMENT    │───────►│  ENDORSEMENT    │────────────────┘
│                 │        │     TYPE        │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│  POLICY_VALUE   │───────►│     POLICY      │
│                 │        │   (permanent)   │
└────────┬────────┘        └─────────────────┘
         │
         │ supports
         ▼
┌─────────────────┐        ┌─────────────────┐
│  POLICY_LOAN    │◄───────│  POLICY_LOAN    │
│                 │        │   REPAYMENT     │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐        ┌─────────────────┐
│  GRACE_PERIOD   │───────►│     POLICY      │◄───────│ REINSTATEMENT   │
└─────────────────┘        └─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐        ┌─────────────────┐
│   CONVERSION    │───────►│     POLICY      │◄───────│   SURRENDER     │
└─────────────────┘        └─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐        ┌─────────────────┐
│  FREE_LOOK      │───────►│     POLICY      │◄───────│ POLICY_DOCUMENT │
│    PERIOD       │        │                 │        └────────┬────────┘
└─────────────────┘        └─────────────────┘                 │
                                                               ▼
┌─────────────────┐                                   ┌─────────────────┐
│ POLICY_ANNIVER- │───────►     POLICY                │  DOCUMENT_TYPE  │
│     SARY        │                                   └─────────────────┘
└─────────────────┘

┌─────────────────┐
│  POLICY_STATUS  │
└─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Policy | Product | Product Version | Policy issued for specific product version |
| Policy | Party/Customer | Customer | Policy owner is a customer |
| Policy | Party/Customer | Individual | Insured is an individual |
| Policy | Party/Customer | Beneficiary | Beneficiaries designated on policy |
| Policy Rider | Product | Rider | Rider product attached to policy |
| Policy | Underwriting | Application | Policy issued from approved application |
| Policy | Premium & Billing | Premium Schedule | Policy has premium payment schedule |
| Policy | Claims | Claim | Claims filed against policy |
| Policy Loan | Premium & Billing | Payment | Loan disbursement and repayment transactions |
| Surrender | Premium & Billing | Payment | Surrender value payment |
| Policy Modification | Underwriting | Risk Assessment | Some modifications require underwriting |
| Policy | Financial Performance | Reserve | Reserves calculated for policies |
| Policy Anniversary | Time/Calendar | Date | Anniversary linked to calendar |
| Free Look Period | Geography | State | Duration varies by state |
| Policy Document | Compliance | Audit Trail | Documents support compliance |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Policy | Master | Core insurance contract |
| Coverage | Master | Death benefit protection |
| Coverage Type | Reference | Coverage classification |
| Policy Status | Reference | Policy lifecycle state |
| Policy Status History | Transactional | Status change log |
| Policy Owner | Master | Policy ownership |
| Insured | Master | Covered individual |
| Payor | Master | Premium payment responsibility |
| Beneficiary Designation | Master | Death benefit recipients |
| Designation Level | Reference | Beneficiary priority |
| Policy Rider | Master | Attached rider coverage |
| Policy Term | Master | Term policy duration |
| Policy Value | Transactional | Cash value for permanent policies |
| Policy Loan | Transactional | Loan against cash value |
| Policy Loan Repayment | Transactional | Loan repayment transactions |
| Policy Modification | Transactional | Policy change requests |
| Modification Type | Reference | Change type classification |
| Endorsement | Transactional | Formal policy amendment |
| Endorsement Type | Reference | Amendment classification |
| Policy Anniversary | Dimensional | Annual policy milestone |
| Grace Period | Transactional | Late payment window |
| Reinstatement | Transactional | Lapsed policy restoration |
| Conversion | Transactional | Term to permanent conversion |
| Surrender | Transactional | Policy cash-out termination |
| Free Look Period | Transactional | Cancellation window |
| Policy Document | Transactional | Associated documents |
| Document Type | Reference | Document classification |

**Total Entities: 27**

---

## Assumptions Log

1. Policy Number is unique and immutable once assigned
2. A policy has exactly one insured person (no joint policies)
3. Owner, insured, and payor may be different parties
4. Multiple beneficiaries can share proceeds by percentage
5. All policy modifications are logged with effective dates
6. Grace period provides coverage during non-payment window
7. Reinstatement may require underwriting
8. Conversion preserves original underwriting class
9. Documents are stored electronically with retention periods

---

## Open Questions

1. Will joint life policies be offered in the future?
2. How to handle decreasing term coverage amounts?
3. Should pending policy sub-statuses be separate entities?
4. How to handle minor owners with custodians?
5. How to handle per stirpes beneficiary designations?
6. Can riders be cancelled independently of base policy?
7. How to handle term policy renewals vs. new policies?
8. What valuation frequency for cash value calculations?
9. What is the reinstatement window by product?
10. What products are available for term conversions?
11. What are the surrender charge schedules?
12. What is the document retention policy?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
