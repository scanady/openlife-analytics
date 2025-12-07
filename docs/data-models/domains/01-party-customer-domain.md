# Party/Customer Domain
## Logical Data Model - Entity Definitions

**Domain:** Party/Customer  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Core Foundation Domain

---

## Domain Overview

**Purpose:** Manage all information about individuals and organizations that interact with the business, including customers, prospects, beneficiaries, and other stakeholders.

**Scope:** This domain encompasses the identification, demographics, contact information, relationships, and roles of all parties associated with the life insurance business. It serves as the central hub for customer-related analysis across the enterprise.

**Business Value:**
- Single source of truth for customer identity
- Enable customer lifecycle and lifetime value analysis
- Support household-level analytics and marketing
- Facilitate regulatory compliance (KYC, AML)
- Enable customer segmentation and personalization

---

## Entity Catalog

### PARTY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Party |
| **Entity Definition** | A supertype entity representing any individual or organization that has a relationship with the business. Party serves as the foundation for all people and organizations tracked in the system, including customers, prospects, beneficiaries, and business partners. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Party Identifier (system-generated unique identifier) |
| **Key Relationships** | - Has one or more Party Role assignments<br>- May have one or more Contact Points<br>- May belong to one or more Households<br>- May participate in one or more Party Relationships |
| **Assumptions** | - Party is abstract; all instances are either Individual or Organization<br>- A single Party may have multiple roles (e.g., customer and beneficiary) |
| **Open Questions** | - Should deceased individuals remain as Party records or be archived? |

---

### INDIVIDUAL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Individual |
| **Entity Definition** | A natural person who interacts with the business in any capacity. Individuals may be customers, prospects, beneficiaries, insureds, or other stakeholders. This entity captures person-specific attributes distinct from organizations. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Party Identifier (inherited from Party) |
| **Key Relationships** | - Is a subtype of Party<br>- May have one or more Individual Identity Verifications<br>- May have Demographic Profile<br>- May be the Insured on one or more Policies (Policy Domain) |
| **Assumptions** | - Full legal name is captured; nicknames/preferred names are optional<br>- Date of birth is required for individuals |
| **Open Questions** | - How to handle name changes (marriage, legal changes)? |

---

### ORGANIZATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Organization |
| **Entity Definition** | A legal entity such as a corporation, partnership, trust, or government body that interacts with the business. Organizations may be policy owners, beneficiaries, or business partners. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Party Identifier (inherited from Party) |
| **Key Relationships** | - Is a subtype of Party<br>- May have one or more Organization Identifiers (EIN, DUNS)<br>- May be Policy Owner (Policy Domain)<br>- May be Beneficiary on one or more Policies |
| **Assumptions** | - Trusts are treated as Organizations<br>- Organization structure/hierarchy is not tracked at this time |
| **Open Questions** | - Should we track corporate hierarchies (parent/subsidiary)? |

---

### PARTY_ROLE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Party Role |
| **Entity Definition** | The specific function or capacity in which a Party interacts with the business. A single Party may have multiple roles simultaneously or over time (e.g., a person may be both a customer and a beneficiary). |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Party Identifier + Role Type Code + Effective Date |
| **Key Relationships** | - Assigned to exactly one Party<br>- References Party Role Type<br>- May be associated with specific Policies, Claims, or Applications depending on role |
| **Assumptions** | - Roles have effective and expiration dates for historical tracking<br>- Common roles: Customer, Prospect, Insured, Owner, Beneficiary, Payor, Agent |
| **Open Questions** | - Should "Lead" be a distinct role or a prospect status? |

---

### PARTY_ROLE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Party Role Type |
| **Entity Definition** | A reference entity defining the types of roles a Party can have with the business. Provides standardized classification of party functions. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Role Type Code |
| **Key Relationships** | - Referenced by Party Role<br>- May have hierarchy (primary/secondary roles) |
| **Assumptions** | - Core role types: CUSTOMER, PROSPECT, INSURED, OWNER, BENEFICIARY, PAYOR, CONTINGENT_BENEFICIARY, TRUSTEE, GUARDIAN |
| **Open Questions** | - None at this time |

---

### CUSTOMER

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer |
| **Entity Definition** | A Party who has purchased or currently holds one or more insurance policies. Customer represents the business relationship aspect of a Party, tracking acquisition, status, and value metrics. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Customer Identifier (may be same as Party Identifier) |
| **Key Relationships** | - Associated with exactly one Party<br>- May own one or more Policies (Policy Domain)<br>- Has Customer Segment assignment<br>- Has calculated Customer Lifetime Value |
| **Assumptions** | - A Party becomes a Customer when their first policy is issued<br>- Customer status can be Active, Lapsed, Terminated, or Winback |
| **Open Questions** | - How long after all policies terminate does Customer status change? |

---

### PROSPECT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Prospect |
| **Entity Definition** | A Party who has expressed interest in the business's products but has not yet purchased a policy. Prospects may have requested quotes, started applications, or been acquired through marketing campaigns. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Prospect Identifier |
| **Key Relationships** | - Associated with exactly one Party<br>- May have one or more Quotes (Sales & Distribution Domain)<br>- May have one or more Applications (Underwriting Domain)<br>- Originated from Lead Source |
| **Assumptions** | - A Prospect converts to Customer upon policy issuance<br>- Prospect records are retained for historical analysis |
| **Open Questions** | - What is the retention period for unconverted prospect data? |

---

### BENEFICIARY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Beneficiary |
| **Entity Definition** | A Party designated to receive death benefit proceeds from a life insurance policy. Beneficiaries may be individuals, organizations, trusts, or estates. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Beneficiary Identifier |
| **Key Relationships** | - Associated with exactly one Party<br>- Designated on one or more Policies (Policy Domain)<br>- Has Beneficiary Designation (primary/contingent)<br>- May receive Claim Payouts (Claims Domain) |
| **Assumptions** | - Beneficiary percentage allocations are stored with policy designation<br>- Estate and Trust beneficiaries are modeled as Organizations |
| **Open Questions** | - How to handle minor beneficiaries and custodian relationships? |

---

### HOUSEHOLD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Household |
| **Entity Definition** | A grouping of Individuals who share a common residence or are related through family relationships. Households enable analysis of family-level insurance penetration and cross-sell opportunities. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Household Identifier |
| **Key Relationships** | - Contains one or more Individuals (via Household Membership)<br>- Has primary Address (Geography Domain)<br>- Aggregates Policies across members |
| **Assumptions** | - Household determination is based on address matching and relationship data<br>- An Individual may belong to only one Household at a time |
| **Open Questions** | - How to handle individuals with multiple residences?<br>- Should adult children be separate households? |

---

### HOUSEHOLD_MEMBERSHIP

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Household Membership |
| **Entity Definition** | The association of an Individual with a Household, including their role within that household (head, spouse, dependent, etc.) and the time period of membership. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Household Identifier + Party Identifier + Effective Date |
| **Key Relationships** | - Links Individual to Household<br>- References Household Role Type |
| **Assumptions** | - Membership has effective and end dates for historical tracking<br>- One member per household is designated as Head of Household |
| **Open Questions** | - How to handle blended families and complex custody arrangements? |

---

### PARTY_RELATIONSHIP

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Party Relationship |
| **Entity Definition** | A defined connection between two Parties, capturing the nature of their relationship (spouse, parent-child, business partner, etc.). Relationships are directional and typed. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | From Party Identifier + To Party Identifier + Relationship Type Code + Effective Date |
| **Key Relationships** | - Links two Party entities<br>- References Relationship Type |
| **Assumptions** | - Relationships are stored directionally (A to B) with inverse implied<br>- Family relationships support beneficiary validation |
| **Open Questions** | - Should relationship verification be required for beneficiary designation? |

---

### RELATIONSHIP_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Relationship Type |
| **Entity Definition** | A reference entity defining the types of relationships that can exist between Parties. Includes both family and business relationships. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Relationship Type Code |
| **Key Relationships** | - Referenced by Party Relationship |
| **Assumptions** | - Core types: SPOUSE, PARENT, CHILD, SIBLING, GRANDPARENT, GRANDCHILD, DOMESTIC_PARTNER, BUSINESS_PARTNER, EMPLOYER, TRUSTEE_OF |
| **Open Questions** | - None at this time |

---

### CONTACT_POINT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Contact Point |
| **Entity Definition** | A means of contacting a Party, including postal addresses, phone numbers, email addresses, and other communication channels. A Party may have multiple contact points of various types. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Contact Point Identifier |
| **Key Relationships** | - Belongs to exactly one Party<br>- References Contact Point Type<br>- Address contact points link to Geography Domain |
| **Assumptions** | - One contact point per type may be designated as Primary<br>- Contact points have valid/invalid status and verification dates |
| **Open Questions** | - How to handle Do Not Contact preferences at contact point level? |

---

### CONTACT_POINT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Contact Point Type |
| **Entity Definition** | A reference entity defining the types of contact methods available. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Contact Point Type Code |
| **Key Relationships** | - Referenced by Contact Point |
| **Assumptions** | - Core types: MAILING_ADDRESS, RESIDENTIAL_ADDRESS, WORK_ADDRESS, HOME_PHONE, MOBILE_PHONE, WORK_PHONE, EMAIL, SMS |
| **Open Questions** | - Should social media handles be contact points? |

---

### ADDRESS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Address |
| **Entity Definition** | A physical or mailing location associated with a Party. Addresses are a specialized type of Contact Point with structured location components enabling geographic analysis. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Address Identifier |
| **Key Relationships** | - Is a subtype of Contact Point<br>- Links to Geographic Area (Geography Domain)<br>- Used for Territorial Rating (Product Domain) |
| **Assumptions** | - Addresses are standardized and validated against postal databases<br>- International addresses are supported |
| **Open Questions** | - Should we store both original and standardized address versions? |

---

### CUSTOMER_SEGMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer Segment |
| **Entity Definition** | A classification grouping of Customers based on shared characteristics, behaviors, or value metrics. Segments enable targeted marketing, pricing, and service strategies. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Segment Code |
| **Key Relationships** | - Assigned to Customers<br>- Used for Campaign targeting (Sales & Distribution Domain)<br>- May influence Service Level (Customer Engagement Domain) |
| **Assumptions** | - Customers may belong to multiple segment types (value segment, lifecycle segment, etc.)<br>- Segment assignments are recalculated periodically |
| **Open Questions** | - What segmentation models will be implemented initially? |

---

### CUSTOMER_SEGMENT_ASSIGNMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer Segment Assignment |
| **Entity Definition** | The assignment of a Customer to a specific Segment, with effective dates to track segment migration over time. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Customer Identifier + Segment Code + Effective Date |
| **Key Relationships** | - Links Customer to Customer Segment |
| **Assumptions** | - Historical segment assignments are retained for trend analysis |
| **Open Questions** | - None at this time |

---

### IDENTITY_VERIFICATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Identity Verification |
| **Entity Definition** | A record of identity verification activities performed for a Party, supporting KYC (Know Your Customer) and regulatory compliance requirements. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Verification Identifier |
| **Key Relationships** | - Performed for exactly one Party (Individual)<br>- May use third-party Verification Service<br>- Supports Compliance requirements (Compliance Domain) |
| **Assumptions** | - Multiple verification attempts may exist for one Party<br>- Verification methods include document, electronic, and knowledge-based |
| **Open Questions** | - What verification vendors will be integrated? |

---

### COMMUNICATION_PREFERENCE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Communication Preference |
| **Entity Definition** | A Party's stated preferences for how and when they wish to be contacted, including channel preferences, frequency limits, and opt-out designations. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Party Identifier + Preference Type Code |
| **Key Relationships** | - Belongs to exactly one Party<br>- References Communication Channel (Customer Engagement Domain)<br>- Applied to Campaigns (Sales & Distribution Domain) |
| **Assumptions** | - Preferences are honored across all communication channels<br>- Regulatory opt-outs override marketing preferences |
| **Open Questions** | - How to handle conflicting preferences across channels? |

---

## Conceptual Entity-Relationship Diagram

```
                                    ┌─────────────────┐
                                    │  RELATIONSHIP   │
                                    │     TYPE        │
                                    └────────┬────────┘
                                             │
                                             │ classifies
                                             ▼
┌─────────────────┐                ┌─────────────────┐
│  PARTY_ROLE     │◄───────────────│     PARTY       │──────────────────┐
│     TYPE        │   has roles    │   (supertype)   │                  │
└────────┬────────┘                └────────┬────────┘                  │
         │                                  │                           │
         │ classifies                       │ is-a                      │
         ▼                                  ▼                           │
┌─────────────────┐                ┌───────────────────┐                │
│   PARTY_ROLE    │                │                   │                │
│                 │                ▼                   ▼                │
└─────────────────┘        ┌──────────────┐   ┌──────────────┐         │
                           │  INDIVIDUAL  │   │ ORGANIZATION │         │
                           └──────┬───────┘   └──────────────┘         │
                                  │                                     │
                    ┌─────────────┼─────────────┐                      │
                    ▼             ▼             ▼                      │
            ┌───────────┐ ┌───────────┐ ┌───────────┐                  │
            │ CUSTOMER  │ │ PROSPECT  │ │BENEFICIARY│                  │
            └─────┬─────┘ └───────────┘ └───────────┘                  │
                  │                                                     │
                  ▼                                                     │
        ┌─────────────────┐                                            │
        │CUSTOMER_SEGMENT │                                            │
        │   ASSIGNMENT    │                                            │
        └────────┬────────┘                                            │
                 │                                                      │
                 ▼                                                      │
        ┌─────────────────┐                                            │
        │CUSTOMER_SEGMENT │                                            │
        └─────────────────┘                                            │
                                                                        │
┌─────────────────┐        ┌─────────────────┐                         │
│   HOUSEHOLD     │◄───────│   HOUSEHOLD     │◄────────────────────────┘
│                 │        │   MEMBERSHIP    │    belongs to
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│ PARTY_RELATION- │───────►│     PARTY       │
│      SHIP       │        │  (from/to)      │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐        ┌─────────────────┐
│ CONTACT_POINT   │───────►│     PARTY       │        │ CONTACT_POINT   │
│                 │        │                 │        │     TYPE        │
└────────┬────────┘        └─────────────────┘        └─────────────────┘
         │                                                     ▲
         │ is-a                                                │
         ▼                                                     │
┌─────────────────┐                                           │
│    ADDRESS      │───────────────────────────────────────────┘
└─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│   IDENTITY      │───────►│   INDIVIDUAL    │
│  VERIFICATION   │        │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│ COMMUNICATION   │───────►│     PARTY       │
│   PREFERENCE    │        │                 │
└─────────────────┘        └─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Party | Policy | Policy | Party may be Owner, Insured, or Payor on Policy |
| Beneficiary | Policy | Beneficiary Designation | Beneficiary is designated on Policy |
| Beneficiary | Claims | Claim Payout | Beneficiary receives claim proceeds |
| Customer | Sales & Distribution | Quote | Customer/Prospect requests quotes |
| Customer | Sales & Distribution | Lead | Prospect originates from Lead |
| Customer | Customer Engagement | Interaction | Customer has service interactions |
| Customer | Premium & Billing | Payment | Customer makes premium payments |
| Prospect | Underwriting | Application | Prospect submits underwriting application |
| Address | Geography | Geographic Area | Address maps to geographic hierarchy |
| Party | Compliance | AML Screening | Party is screened for compliance |
| Customer Segment | Sales & Distribution | Campaign | Segments are targeted by campaigns |
| Household | Financial Performance | Household Value | Household aggregates CLV metrics |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Party | Master | Supertype for all individuals and organizations |
| Individual | Master | Natural person subtype of Party |
| Organization | Master | Legal entity subtype of Party |
| Party Role | Reference | Function/capacity of Party interaction |
| Party Role Type | Reference | Classification of role types |
| Customer | Master | Party with policy relationship |
| Prospect | Master | Party with interest but no policy |
| Beneficiary | Master | Party designated for death benefits |
| Household | Master | Grouping of related individuals |
| Household Membership | Master | Individual-Household association |
| Party Relationship | Master | Connection between two Parties |
| Relationship Type | Reference | Classification of relationships |
| Contact Point | Master | Communication method for Party |
| Contact Point Type | Reference | Classification of contact methods |
| Address | Master | Physical location contact point |
| Customer Segment | Reference | Classification grouping of customers |
| Customer Segment Assignment | Transactional | Customer-to-segment mapping |
| Identity Verification | Transactional | KYC verification record |
| Communication Preference | Master | Party contact preferences |

**Total Entities: 19**

---

## Assumptions Log

1. Party is an abstract supertype - all instances are either Individual or Organization
2. A Party may have multiple concurrent roles (customer, beneficiary, insured)
3. Customer status begins when first policy is issued
4. Prospect records are retained after conversion for historical analysis
5. Household membership is mutually exclusive (one household at a time)
6. Addresses are standardized against postal validation services
7. Identity verification is required for regulatory compliance
8. Segment assignments are tracked historically for trend analysis

---

## Open Questions

1. How should deceased individuals be handled - active records or archived?
2. What is the retention period for unconverted prospect data?
3. How to handle individuals with multiple residences for household assignment?
4. Should adult children living separately be distinct households?
5. What identity verification vendors will be integrated?
6. Should relationship verification be required for beneficiary designation?
7. What customer segmentation models will be implemented?
8. How to handle minor beneficiaries and custodian relationships?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
