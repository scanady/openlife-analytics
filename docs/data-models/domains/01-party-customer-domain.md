# Party/Customer Domain
## Logical Data Model - Entity Definitions

**Domain:** Party/Customer  
**Version:** 1.1  
**Date:** December 6, 2025  
**Status:** Phase 2 - Attribute Definition (P1 Entities)  
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

## Attribute Definitions - Phase 1 (P1) Entities

This section defines the detailed attributes for all Phase 1 (Foundation) entities in the Party/Customer domain.

---

### PARTY Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| party_id | VARCHAR(36) | No | Unique identifier for the party record. | Primary Key. UUID format recommended. System-generated. |
| party_type_code | VARCHAR(20) | No | Discriminator indicating if party is Individual or Organization. | Valid values: 'INDIVIDUAL', 'ORGANIZATION'. Immutable after creation. |
| party_status_code | VARCHAR(20) | No | Current status of the party record. | Valid values: 'ACTIVE', 'INACTIVE', 'DECEASED', 'DISSOLVED', 'MERGED'. Default: 'ACTIVE'. |
| party_source_system_code | VARCHAR(20) | No | System of record that originated the party. | Reference to source system code table. |
| party_source_id | VARCHAR(50) | Yes | Original identifier from source system. | Used for cross-reference and data lineage. |
| tax_id_type_code | VARCHAR(20) | Yes | Type of tax identification number. | Valid values: 'SSN', 'EIN', 'ITIN', 'FOREIGN'. |
| tax_id_number | VARCHAR(20) | Yes | Encrypted tax identification number. | Must be encrypted at rest. Format validated based on type. |
| tax_id_verified_flag | CHAR(1) | Yes | Indicates if tax ID has been verified. | Valid values: 'Y', 'N'. Default: 'N'. |
| tax_id_verified_date | DATE | Yes | Date tax ID was last verified. | Required when verified_flag = 'Y'. |
| preferred_language_code | VARCHAR(10) | Yes | Party's preferred language for communications. | ISO 639-1 language code. Default from locale. |
| preferred_currency_code | VARCHAR(3) | Yes | Party's preferred currency. | ISO 4217 currency code. Default: 'USD'. |
| do_not_contact_flag | CHAR(1) | No | Master flag to suppress all marketing contact. | Valid values: 'Y', 'N'. Default: 'N'. |
| duplicate_suspect_flag | CHAR(1) | No | Indicates party may be duplicate of another. | Valid values: 'Y', 'N'. Default: 'N'. |
| master_party_id | VARCHAR(36) | Yes | If duplicate, points to master party record. | Foreign key to party. Populated when merged. |
| data_quality_score | DECIMAL(5,2) | Yes | Computed data completeness/quality score. | Range 0.00-100.00. Updated by DQ processes. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |
| record_version | INTEGER | No | Optimistic locking version number. | Incremented on each update. Default: 1. |

---

### INDIVIDUAL Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| party_id | VARCHAR(36) | No | Unique identifier linking to Party. | Primary Key. Foreign Key to PARTY.party_id. |
| legal_first_name | VARCHAR(100) | No | Individual's legal first name. | As shown on legal documents. |
| legal_middle_name | VARCHAR(100) | Yes | Individual's legal middle name(s). | May include multiple middle names. |
| legal_last_name | VARCHAR(100) | No | Individual's legal last name/surname. | As shown on legal documents. |
| name_suffix | VARCHAR(20) | Yes | Name suffix (Jr., Sr., III, etc.). | Standardized values preferred. |
| name_prefix | VARCHAR(20) | Yes | Name prefix/title (Mr., Mrs., Dr., etc.). | Standardized values preferred. |
| preferred_first_name | VARCHAR(100) | Yes | Preferred/commonly used first name. | Used for personalized communications. |
| full_legal_name | VARCHAR(300) | Yes | Concatenated full legal name. | Computed field or stored for search optimization. |
| birth_date | DATE | No | Individual's date of birth. | Must be in the past. Required for underwriting. |
| death_date | DATE | Yes | Date of death if deceased. | Must be after birth_date. Triggers status change. |
| gender_code | VARCHAR(20) | Yes | Individual's gender. | Valid values: 'MALE', 'FEMALE', 'NON_BINARY', 'UNDISCLOSED'. |
| marital_status_code | VARCHAR(20) | Yes | Current marital status. | Valid values: 'SINGLE', 'MARRIED', 'DIVORCED', 'WIDOWED', 'DOMESTIC_PARTNER', 'SEPARATED'. |
| citizenship_country_code | VARCHAR(3) | Yes | Primary country of citizenship. | ISO 3166-1 alpha-3 country code. |
| residency_country_code | VARCHAR(3) | Yes | Country of legal residence. | ISO 3166-1 alpha-3 country code. |
| residency_state_code | VARCHAR(10) | Yes | State/province of legal residence. | For US, use 2-character state code. |
| occupation_code | VARCHAR(20) | Yes | Standardized occupation classification. | Reference to occupation code table. |
| occupation_description | VARCHAR(200) | Yes | Free-text occupation description. | As stated by individual. |
| employer_name | VARCHAR(200) | Yes | Current employer name. | For employed individuals. |
| annual_income_amount | DECIMAL(15,2) | Yes | Stated annual income. | Used for underwriting and suitability. |
| annual_income_currency_code | VARCHAR(3) | Yes | Currency of annual income. | ISO 4217 currency code. Default: 'USD'. |
| net_worth_amount | DECIMAL(15,2) | Yes | Stated net worth. | Used for suitability determination. |
| net_worth_currency_code | VARCHAR(3) | Yes | Currency of net worth. | ISO 4217 currency code. Default: 'USD'. |
| tobacco_use_flag | CHAR(1) | Yes | Indicates if individual uses tobacco. | Valid values: 'Y', 'N', 'U' (unknown). |
| tobacco_use_type_code | VARCHAR(20) | Yes | Type of tobacco use if applicable. | Valid values: 'CIGARETTE', 'CIGAR', 'PIPE', 'SMOKELESS', 'VAPE', 'NONE'. |
| tobacco_last_use_date | DATE | Yes | Date of last tobacco use. | For former users. |
| drivers_license_number | VARCHAR(50) | Yes | Driver's license number (encrypted). | Encrypted at rest. For identity verification. |
| drivers_license_state_code | VARCHAR(10) | Yes | State that issued driver's license. | Required if license number provided. |
| passport_number | VARCHAR(50) | Yes | Passport number (encrypted). | Encrypted at rest. For identity verification. |
| passport_country_code | VARCHAR(3) | Yes | Country that issued passport. | ISO 3166-1 alpha-3. Required if passport number provided. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### ORGANIZATION Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| party_id | VARCHAR(36) | No | Unique identifier linking to Party. | Primary Key. Foreign Key to PARTY.party_id. |
| legal_name | VARCHAR(300) | No | Organization's full legal name. | As registered with governing authority. |
| doing_business_as_name | VARCHAR(300) | Yes | Trade name or DBA name. | May differ from legal name. |
| organization_type_code | VARCHAR(20) | No | Type of organization. | Valid values: 'CORPORATION', 'LLC', 'PARTNERSHIP', 'TRUST', 'ESTATE', 'NONPROFIT', 'GOVERNMENT', 'OTHER'. |
| legal_structure_code | VARCHAR(20) | Yes | Detailed legal structure. | Valid values: 'C_CORP', 'S_CORP', 'LLC_SINGLE', 'LLC_MULTI', 'LP', 'LLP', 'SOLE_PROP', 'IRREVOCABLE_TRUST', 'REVOCABLE_TRUST', 'CHARITABLE_TRUST'. |
| formation_date | DATE | Yes | Date organization was legally formed. | Must be in the past. |
| dissolution_date | DATE | Yes | Date organization was dissolved. | Must be after formation_date. Triggers status change. |
| formation_state_code | VARCHAR(10) | Yes | State/jurisdiction of formation. | For US entities, 2-character state code. |
| formation_country_code | VARCHAR(3) | Yes | Country of formation. | ISO 3166-1 alpha-3 country code. |
| employer_id_number | VARCHAR(20) | Yes | Federal Employer ID Number (encrypted). | EIN format: XX-XXXXXXX. Encrypted at rest. |
| ein_verified_flag | CHAR(1) | Yes | Indicates if EIN has been verified. | Valid values: 'Y', 'N'. Default: 'N'. |
| state_registration_number | VARCHAR(50) | Yes | State registration or charter number. | Varies by state. |
| naics_code | VARCHAR(10) | Yes | North American Industry Classification code. | 6-digit NAICS code. |
| sic_code | VARCHAR(10) | Yes | Standard Industrial Classification code. | 4-digit SIC code (legacy). |
| industry_description | VARCHAR(200) | Yes | Industry description text. | Derived from NAICS or manually entered. |
| employee_count | INTEGER | Yes | Number of employees. | Used for group sizing. |
| annual_revenue_amount | DECIMAL(15,2) | Yes | Annual revenue. | Used for business valuation. |
| annual_revenue_currency_code | VARCHAR(3) | Yes | Currency of annual revenue. | ISO 4217 currency code. Default: 'USD'. |
| fiscal_year_end_month | INTEGER | Yes | Month fiscal year ends. | Valid values: 1-12. |
| is_publicly_traded_flag | CHAR(1) | Yes | Indicates if publicly traded company. | Valid values: 'Y', 'N'. |
| stock_ticker_symbol | VARCHAR(10) | Yes | Stock exchange ticker symbol. | Required if publicly traded. |
| stock_exchange_code | VARCHAR(20) | Yes | Exchange where stock is traded. | Valid values: 'NYSE', 'NASDAQ', 'AMEX', 'OTC', etc. |
| parent_organization_id | VARCHAR(36) | Yes | Parent organization party_id. | Foreign key to PARTY.party_id. For subsidiaries. |
| ultimate_parent_org_id | VARCHAR(36) | Yes | Ultimate parent organization party_id. | Foreign key to PARTY.party_id. Top of hierarchy. |
| website_url | VARCHAR(500) | Yes | Organization's website URL. | Must be valid URL format. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### PARTY_ROLE Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| party_role_id | VARCHAR(36) | No | Unique identifier for the party role. | Primary Key. UUID format recommended. |
| party_id | VARCHAR(36) | No | Party holding this role. | Foreign Key to PARTY.party_id. |
| party_role_type_code | VARCHAR(20) | No | Type of role being held. | Foreign Key to PARTY_ROLE_TYPE.party_role_type_code. |
| role_effective_date | DATE | No | Date role became effective. | Must be <= current date. |
| role_expiration_date | DATE | Yes | Date role expires or ended. | Must be > effective_date. Null = currently active. |
| role_status_code | VARCHAR(20) | No | Current status of the role. | Valid values: 'ACTIVE', 'INACTIVE', 'SUSPENDED', 'TERMINATED'. |
| role_context_entity_type | VARCHAR(50) | Yes | Entity type providing context for role. | Examples: 'POLICY', 'CLAIM', 'APPLICATION'. |
| role_context_entity_id | VARCHAR(36) | Yes | Identifier of context entity. | References the entity where role applies. |
| primary_role_flag | CHAR(1) | No | Indicates if this is primary role of type. | Valid values: 'Y', 'N'. One primary per type per party. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### PARTY_ROLE_TYPE Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| party_role_type_code | VARCHAR(20) | No | Unique code for the role type. | Primary Key. Uppercase, underscore-separated. |
| party_role_type_name | VARCHAR(100) | No | Display name for the role type. | Human-readable name. |
| party_role_type_description | VARCHAR(500) | Yes | Detailed description of the role. | Explains role's business purpose. |
| role_category_code | VARCHAR(20) | No | Category grouping for the role type. | Valid values: 'CUSTOMER', 'POLICY', 'UNDERWRITING', 'CLAIMS', 'SALES', 'COMPLIANCE'. |
| applicable_party_type | VARCHAR(20) | Yes | Restricts role to party type. | Valid values: 'INDIVIDUAL', 'ORGANIZATION', 'ANY'. Default: 'ANY'. |
| allows_multiple_flag | CHAR(1) | No | Can party have multiple roles of this type. | Valid values: 'Y', 'N'. |
| requires_context_flag | CHAR(1) | No | Does role require context entity. | Valid values: 'Y', 'N'. |
| display_sequence | INTEGER | Yes | Order for display in UI. | Lower numbers appear first. |
| active_flag | CHAR(1) | No | Indicates if role type is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| effective_date | DATE | No | Date role type became available. | Reference data versioning. |
| expiration_date | DATE | Yes | Date role type was retired. | Null = currently active. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

**Standard Party Role Type Values:**

| Code | Name | Category | Applicable Party Type |
|------|------|----------|----------------------|
| APPLICANT | Policy Applicant | POLICY | ANY |
| INSURED | Insured Person | POLICY | INDIVIDUAL |
| POLICY_OWNER | Policy Owner | POLICY | ANY |
| PAYOR | Premium Payor | POLICY | ANY |
| BENEFICIARY | Policy Beneficiary | POLICY | ANY |
| CLAIMANT | Claims Claimant | CLAIMS | ANY |
| AGENT | Licensed Agent | SALES | INDIVIDUAL |
| AGENCY | Insurance Agency | SALES | ORGANIZATION |
| UNDERWRITER | Underwriter | UNDERWRITING | INDIVIDUAL |
| PHYSICIAN | Attending Physician | UNDERWRITING | INDIVIDUAL |
| EMPLOYER_SPONSOR | Group Sponsor | POLICY | ORGANIZATION |
| TRUSTEE | Trust Trustee | POLICY | ANY |
| POWER_OF_ATTORNEY | Power of Attorney | COMPLIANCE | INDIVIDUAL |

---

### CUSTOMER Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| customer_id | VARCHAR(36) | No | Unique identifier for the customer. | Primary Key. UUID format recommended. |
| party_id | VARCHAR(36) | No | Party who is the customer. | Foreign Key to PARTY.party_id. |
| customer_number | VARCHAR(20) | No | Business-friendly customer number. | Unique. System-generated. Display identifier. |
| customer_type_code | VARCHAR(20) | No | Type of customer. | Valid values: 'RETAIL', 'GROUP', 'INSTITUTIONAL', 'WHOLESALE'. |
| customer_status_code | VARCHAR(20) | No | Current customer status. | Valid values: 'ACTIVE', 'INACTIVE', 'PROSPECT', 'FORMER', 'DECLINED'. |
| acquisition_date | DATE | Yes | Date customer was acquired. | Date of first policy issue or enrollment. |
| acquisition_channel_code | VARCHAR(20) | Yes | Channel through which customer was acquired. | Valid values: 'DIRECT_WEB', 'DIRECT_PHONE', 'AGENT', 'REFERRAL', 'GROUP', 'WORKSITE'. |
| acquisition_campaign_id | VARCHAR(36) | Yes | Marketing campaign that acquired customer. | Foreign Key to CAMPAIGN (Sales Domain). |
| first_policy_date | DATE | Yes | Date of customer's first policy. | Populated when first policy is issued. |
| most_recent_policy_date | DATE | Yes | Date of most recent policy. | Updated when new policy issued. |
| total_policy_count | INTEGER | No | Count of all policies ever owned. | Derived/aggregated field. Default: 0. |
| active_policy_count | INTEGER | No | Count of currently active policies. | Derived/aggregated field. Default: 0. |
| total_premium_amount | DECIMAL(15,2) | Yes | Total premium across all policies. | Derived/aggregated field. |
| total_face_amount | DECIMAL(15,2) | Yes | Total face value across all policies. | Derived/aggregated field. |
| customer_lifetime_value | DECIMAL(15,2) | Yes | Calculated customer lifetime value. | Updated by analytics processes. |
| customer_value_segment_code | VARCHAR(20) | Yes | Value-based segment. | Valid values: 'PLATINUM', 'GOLD', 'SILVER', 'BRONZE'. |
| customer_lifecycle_stage_code | VARCHAR(20) | Yes | Customer lifecycle stage. | Valid values: 'NEW', 'GROWING', 'MATURE', 'AT_RISK', 'LAPSED', 'REACTIVATED'. |
| churn_risk_score | DECIMAL(5,2) | Yes | Predicted churn probability. | Range 0.00-100.00. Updated by ML models. |
| next_best_action_code | VARCHAR(50) | Yes | Recommended next action. | Updated by recommendation engine. |
| assigned_service_rep_id | VARCHAR(36) | Yes | Assigned customer service rep. | Foreign Key to Party (employee). |
| last_contact_date | DATE | Yes | Date of last customer contact. | Updated by engagement tracking. |
| last_login_timestamp | TIMESTAMP | Yes | Last customer portal login. | Updated by portal activity. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### BENEFICIARY Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| beneficiary_id | VARCHAR(36) | No | Unique identifier for the beneficiary. | Primary Key. UUID format recommended. |
| party_id | VARCHAR(36) | No | Party who is the beneficiary. | Foreign Key to PARTY.party_id. |
| policy_id | VARCHAR(36) | No | Policy for which beneficiary is designated. | Foreign Key to POLICY (Policy Domain). |
| beneficiary_type_code | VARCHAR(20) | No | Type of beneficiary. | Valid values: 'PRIMARY', 'CONTINGENT', 'TERTIARY'. |
| beneficiary_class_code | VARCHAR(20) | No | Classification of beneficiary. | Valid values: 'INDIVIDUAL', 'TRUST', 'ESTATE', 'CHARITY', 'ORGANIZATION'. |
| designation_type_code | VARCHAR(20) | No | How beneficiary was designated. | Valid values: 'NAMED', 'PER_STIRPES', 'PER_CAPITA', 'CLASS'. |
| benefit_percentage | DECIMAL(5,2) | No | Percentage of benefit. | Must sum to 100% within type. Range 0.01-100.00. |
| benefit_amount | DECIMAL(15,2) | Yes | Specific dollar amount if applicable. | Alternative to percentage. |
| relationship_to_insured_code | VARCHAR(20) | Yes | Relationship to the insured. | Valid values: 'SPOUSE', 'CHILD', 'PARENT', 'SIBLING', 'BUSINESS_PARTNER', 'TRUST', 'ESTATE', 'CHARITY', 'OTHER'. |
| irrevocable_flag | CHAR(1) | No | Indicates if beneficiary is irrevocable. | Valid values: 'Y', 'N'. Default: 'N'. |
| restricted_flag | CHAR(1) | No | Indicates if beneficiary change is restricted. | Valid values: 'Y', 'N'. Default: 'N'. |
| restriction_reason | VARCHAR(500) | Yes | Reason for restriction. | Required if restricted_flag = 'Y'. |
| designation_date | DATE | No | Date beneficiary was designated. | Date of beneficiary change request. |
| effective_date | DATE | No | Date designation became effective. | May differ from designation date. |
| expiration_date | DATE | Yes | Date designation ended. | Populated when replaced or policy terminated. |
| beneficiary_status_code | VARCHAR(20) | No | Current status of designation. | Valid values: 'ACTIVE', 'SUPERSEDED', 'REVOKED', 'EXPIRED'. |
| verification_status_code | VARCHAR(20) | Yes | Beneficiary verification status. | Valid values: 'VERIFIED', 'PENDING', 'UNVERIFIED', 'FAILED'. |
| verification_date | DATE | Yes | Date beneficiary was verified. | Required when verified. |
| special_instructions | VARCHAR(1000) | Yes | Special distribution instructions. | Free text for complex situations. |
| minor_flag | CHAR(1) | Yes | Indicates if beneficiary is a minor. | Valid values: 'Y', 'N'. Triggers custodian requirement. |
| custodian_party_id | VARCHAR(36) | Yes | Custodian for minor beneficiary. | Foreign Key to PARTY.party_id. Required if minor. |
| utma_ugma_state_code | VARCHAR(10) | Yes | State for UTMA/UGMA if applicable. | Required for minor custodial designations. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### CONTACT_POINT Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| contact_point_id | VARCHAR(36) | No | Unique identifier for the contact point. | Primary Key. UUID format recommended. |
| party_id | VARCHAR(36) | No | Party who owns this contact point. | Foreign Key to PARTY.party_id. |
| contact_point_type_code | VARCHAR(20) | No | Type of contact point. | Foreign Key to CONTACT_POINT_TYPE. |
| contact_value | VARCHAR(500) | No | The contact value (phone, email, etc.). | Format depends on type. Validated accordingly. |
| contact_value_standardized | VARCHAR(500) | Yes | Standardized/normalized contact value. | System-generated standardized version. |
| contact_point_purpose_code | VARCHAR(20) | Yes | Purpose/usage of this contact. | Valid values: 'PERSONAL', 'BUSINESS', 'EMERGENCY', 'BILLING'. |
| primary_flag | CHAR(1) | No | Indicates primary contact of this type. | Valid values: 'Y', 'N'. One primary per type per party. |
| preferred_flag | CHAR(1) | No | Party's preferred contact method. | Valid values: 'Y', 'N'. |
| verified_flag | CHAR(1) | No | Contact point has been verified. | Valid values: 'Y', 'N'. Default: 'N'. |
| verification_date | DATE | Yes | Date contact was last verified. | Required when verified_flag = 'Y'. |
| verification_method_code | VARCHAR(20) | Yes | How contact was verified. | Valid values: 'EMAIL_CONFIRM', 'SMS_CONFIRM', 'PHONE_CALL', 'MAIL_RETURN', 'AGENT_VERIFIED'. |
| active_flag | CHAR(1) | No | Contact point is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| invalid_flag | CHAR(1) | No | Contact point has been marked invalid. | Valid values: 'Y', 'N'. Default: 'N'. |
| invalid_reason_code | VARCHAR(20) | Yes | Reason contact is invalid. | Valid values: 'BOUNCED', 'DISCONNECTED', 'WRONG_NUMBER', 'UNDELIVERABLE', 'OPTED_OUT'. |
| invalid_date | DATE | Yes | Date contact was marked invalid. | Required when invalid_flag = 'Y'. |
| effective_date | DATE | No | Date contact point became effective. | Start of validity period. |
| expiration_date | DATE | Yes | Date contact point expires/ended. | End of validity period. Null = no expiration. |
| do_not_contact_flag | CHAR(1) | No | Suppress all contact to this point. | Valid values: 'Y', 'N'. Default: 'N'. |
| consent_given_flag | CHAR(1) | Yes | Explicit consent to use contact. | Valid values: 'Y', 'N'. For GDPR/TCPA compliance. |
| consent_date | DATE | Yes | Date consent was given. | Required when consent_given_flag = 'Y'. |
| consent_source | VARCHAR(100) | Yes | Source/documentation of consent. | Audit trail for consent. |
| last_used_date | DATE | Yes | Date contact point was last used. | Updated by communication systems. |
| last_successful_contact_date | DATE | Yes | Date of last successful contact. | Successful delivery/response. |
| bounce_count | INTEGER | Yes | Number of bounces/failures. | For emails and postal mail. |
| source_system_code | VARCHAR(20) | Yes | System that provided contact point. | For data lineage. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### CONTACT_POINT_TYPE Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| contact_point_type_code | VARCHAR(20) | No | Unique code for contact type. | Primary Key. Uppercase, underscore-separated. |
| contact_point_type_name | VARCHAR(100) | No | Display name for the type. | Human-readable name. |
| contact_point_type_description | VARCHAR(500) | Yes | Detailed description. | Explains usage and format. |
| contact_category_code | VARCHAR(20) | No | Category of contact type. | Valid values: 'ADDRESS', 'PHONE', 'EMAIL', 'DIGITAL'. |
| format_pattern | VARCHAR(200) | Yes | Regex pattern for validation. | Used to validate contact_value format. |
| format_description | VARCHAR(200) | Yes | Human-readable format description. | Example: "10-digit US phone number". |
| requires_verification_flag | CHAR(1) | No | Type requires verification. | Valid values: 'Y', 'N'. |
| verification_method_codes | VARCHAR(200) | Yes | Allowed verification methods. | Comma-separated list of method codes. |
| is_address_flag | CHAR(1) | No | Type represents physical address. | Valid values: 'Y', 'N'. Links to ADDRESS entity. |
| supports_electronic_delivery_flag | CHAR(1) | No | Can be used for e-delivery. | Valid values: 'Y', 'N'. |
| display_sequence | INTEGER | Yes | Order for display in UI. | Lower numbers appear first. |
| active_flag | CHAR(1) | No | Type is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| effective_date | DATE | No | Date type became available. | Reference data versioning. |
| expiration_date | DATE | Yes | Date type was retired. | Null = currently active. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

**Standard Contact Point Type Values:**

| Code | Name | Category | Address Flag | Format Description |
|------|------|----------|--------------|-------------------|
| EMAIL | Email Address | EMAIL | N | Valid email format (RFC 5322) |
| MOBILE_PHONE | Mobile Phone | PHONE | N | 10-digit US phone or international E.164 |
| HOME_PHONE | Home Phone | PHONE | N | 10-digit US phone or international E.164 |
| WORK_PHONE | Work Phone | PHONE | N | 10-digit US phone with optional extension |
| FAX | Fax Number | PHONE | N | 10-digit US phone |
| MAILING_ADDRESS | Mailing Address | ADDRESS | Y | Postal address components |
| RESIDENTIAL_ADDRESS | Residential Address | ADDRESS | Y | Postal address components |
| WORK_ADDRESS | Work Address | ADDRESS | Y | Postal address components |
| SMS | SMS/Text Number | DIGITAL | N | Mobile number capable of SMS |

---

### ADDRESS Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| address_id | VARCHAR(36) | No | Unique identifier for the address. | Primary Key. UUID format recommended. |
| contact_point_id | VARCHAR(36) | No | Link to parent contact point. | Foreign Key to CONTACT_POINT.contact_point_id. |
| address_type_code | VARCHAR(20) | No | Type of address. | Valid values: 'MAILING', 'RESIDENTIAL', 'BUSINESS', 'PO_BOX', 'MILITARY', 'INTERNATIONAL'. |
| address_line_1 | VARCHAR(200) | No | Primary street address line. | Street number and name. |
| address_line_2 | VARCHAR(200) | Yes | Secondary address line. | Apt, Suite, Building, Floor, etc. |
| address_line_3 | VARCHAR(200) | Yes | Third address line. | Additional addressing for complex locations. |
| city_name | VARCHAR(100) | No | City or town name. | Required for domestic addresses. |
| state_province_code | VARCHAR(10) | Yes | State or province code. | US: 2-char state code. CA: 2-char province code. |
| state_province_name | VARCHAR(100) | Yes | State or province full name. | For display purposes. |
| postal_code | VARCHAR(20) | No | Postal/ZIP code. | US: 5-digit or ZIP+4. International varies. |
| postal_code_extension | VARCHAR(10) | Yes | Postal code extension. | US: 4-digit extension if available. |
| country_code | VARCHAR(3) | No | Country code. | ISO 3166-1 alpha-3 code. Default: 'USA'. |
| country_name | VARCHAR(100) | Yes | Country full name. | For display purposes. |
| county_name | VARCHAR(100) | Yes | County/parish name. | For territorial rating. |
| county_fips_code | VARCHAR(10) | Yes | County FIPS code. | US county identification. |
| latitude | DECIMAL(10,7) | Yes | Latitude coordinate. | WGS84 decimal degrees. -90 to +90. |
| longitude | DECIMAL(10,7) | Yes | Longitude coordinate. | WGS84 decimal degrees. -180 to +180. |
| geocode_accuracy_code | VARCHAR(20) | Yes | Accuracy level of geocode. | Valid values: 'ROOFTOP', 'INTERPOLATED', 'CENTROID', 'APPROXIMATE'. |
| geocode_date | DATE | Yes | Date address was geocoded. | When lat/long was determined. |
| census_tract | VARCHAR(20) | Yes | Census tract identifier. | US Census Bureau tract ID. |
| census_block | VARCHAR(20) | Yes | Census block identifier. | US Census Bureau block ID. |
| metropolitan_area_code | VARCHAR(20) | Yes | Metropolitan statistical area code. | CBSA code. |
| time_zone_code | VARCHAR(50) | Yes | Time zone identifier. | IANA time zone (e.g., 'America/New_York'). |
| standardized_flag | CHAR(1) | No | Address has been standardized. | Valid values: 'Y', 'N'. Default: 'N'. |
| standardization_date | DATE | Yes | Date address was standardized. | When USPS/postal standardization applied. |
| standardization_source_code | VARCHAR(20) | Yes | Service used for standardization. | Valid values: 'USPS_CASS', 'SMARTY', 'GOOGLE', 'MELISSA'. |
| deliverable_flag | CHAR(1) | Yes | Address is mail deliverable. | Valid values: 'Y', 'N'. From postal validation. |
| residential_delivery_flag | CHAR(1) | Yes | Residential delivery indicator. | Valid values: 'Y', 'N'. From USPS. |
| vacant_flag | CHAR(1) | Yes | Address is vacant. | Valid values: 'Y', 'N'. From USPS/NCOA. |
| dpv_code | VARCHAR(10) | Yes | Delivery Point Validation code. | USPS DPV confirmation status. |
| address_hash | VARCHAR(64) | Yes | Hash of standardized address. | For duplicate detection. |
| original_address_line_1 | VARCHAR(200) | Yes | Original address before standardization. | Preserved for audit. |
| original_city_name | VARCHAR(100) | Yes | Original city before standardization. | Preserved for audit. |
| original_state_code | VARCHAR(10) | Yes | Original state before standardization. | Preserved for audit. |
| original_postal_code | VARCHAR(20) | Yes | Original postal code before standardization. | Preserved for audit. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

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

| Entity | Type | Phase | Description |
|--------|------|-------|-------------|
| Party | Master | P1 | Supertype for all individuals and organizations |
| Individual | Master | P1 | Natural person subtype of Party |
| Organization | Master | P1 | Legal entity subtype of Party |
| Party Role | Reference | P1 | Function/capacity of Party interaction |
| Party Role Type | Reference | P1 | Classification of role types |
| Customer | Master | P1 | Party with policy relationship |
| Prospect | Master | P2 | Party with interest but no policy |
| Beneficiary | Master | P1 | Party designated for death benefits |
| Household | Master | P3 | Grouping of related individuals |
| Household Membership | Master | P3 | Individual-Household association |
| Party Relationship | Master | P2 | Connection between two Parties |
| Relationship Type | Reference | P2 | Classification of relationships |
| Contact Point | Master | P1 | Communication method for Party |
| Contact Point Type | Reference | P1 | Classification of contact methods |
| Address | Master | P1 | Physical location contact point |
| Customer Segment | Reference | P3 | Classification grouping of customers |
| Customer Segment Assignment | Transactional | P3 | Customer-to-segment mapping |
| Identity Verification | Transactional | P2 | KYC verification record |
| Communication Preference | Master | P2 | Party contact preferences |

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
| 1.1 | December 6, 2025 | Initial | Added P1 entity attribute definitions (10 entities) |

---

*Next Phase: Attribute definition for P2/P3/P4 entities, then Relationship definition*
