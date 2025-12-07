# Product Domain
## Logical Data Model - Entity Definitions

**Domain:** Product  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Core Foundation Domain

---

## Domain Overview

**Purpose:** Define all life insurance products, features, and offerings available to customers, including product structure, pricing components, and lifecycle management.

**Scope:** This domain encompasses the product catalog, riders, features, pricing structures, and product hierarchies. It provides the foundation for understanding what the business sells and how products are configured.

**Business Value:**
- Centralized product information for consistent analysis
- Enable product performance and profitability analysis
- Support product mix optimization
- Enable cross-sell and upsell opportunity identification
- Foundation for pricing and underwriting rules

---

## Entity Catalog

### PRODUCT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product |
| **Entity Definition** | A life insurance offering that can be sold to customers. Product represents a specific insurance solution with defined coverage, terms, pricing, and features. Products may be term life, whole life, universal life, or variable life insurance. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Product Identifier |
| **Key Relationships** | - Belongs to one Product Line<br>- Has one or more Product Versions<br>- May have one or more eligible Riders<br>- Referenced by Policy (Policy Domain)<br>- Has Product Features |
| **Assumptions** | - Products have distinct versions for state/regulatory variations<br>- A Product may be available in multiple states with variations |
| **Open Questions** | - How to handle state-specific product variations vs. distinct products? |

---

### PRODUCT_VERSION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Version |
| **Entity Definition** | A specific version of a Product that represents a point-in-time configuration of features, pricing, and terms. New versions are created when significant changes are made, while older versions remain for existing policies. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Product Identifier + Version Number |
| **Key Relationships** | - Belongs to exactly one Product<br>- Has effective date range<br>- Referenced by Policy at time of issue |
| **Assumptions** | - Policies reference the Product Version active at issue date<br>- Version changes trigger new version rather than modifying existing |
| **Open Questions** | - What changes require new version vs. amendment? |

---

### PRODUCT_LINE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Line |
| **Entity Definition** | A high-level grouping of related Products that share common characteristics or target similar market segments. Product Lines organize the product portfolio for management and analysis. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Product Line Code |
| **Key Relationships** | - Contains one or more Products<br>- Belongs to Product Category |
| **Assumptions** | - Examples: Term Life, Whole Life, Universal Life, Final Expense |
| **Open Questions** | - None at this time |

---

### PRODUCT_CATEGORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Category |
| **Entity Definition** | The highest level of product classification, grouping Product Lines into broad insurance categories. Used for executive-level reporting and portfolio analysis. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Category Code |
| **Key Relationships** | - Contains one or more Product Lines |
| **Assumptions** | - For life insurance: Individual Life, Group Life (if applicable) |
| **Open Questions** | - Will the business expand beyond individual life insurance? |

---

### PRODUCT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Type |
| **Entity Definition** | A classification of Products based on their fundamental insurance structure and characteristics. Distinguishes between term, permanent, and hybrid product structures. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Product Type Code |
| **Key Relationships** | - Classifies Products<br>- Determines available features and riders |
| **Assumptions** | - Core types: TERM, WHOLE_LIFE, UNIVERSAL_LIFE, VARIABLE_UNIVERSAL, INDEXED_UNIVERSAL, FINAL_EXPENSE |
| **Open Questions** | - None at this time |

---

### RIDER

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Rider |
| **Entity Definition** | An optional supplementary benefit that can be added to a base life insurance Product to provide additional coverage or features. Riders modify or enhance the base policy coverage. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Rider Identifier |
| **Key Relationships** | - May be attached to one or more Products (via Product Rider Eligibility)<br>- Referenced by Policy Rider (Policy Domain)<br>- Has Rider Type classification |
| **Assumptions** | - Riders have their own pricing separate from base product<br>- Some riders may be bundled or mutually exclusive |
| **Open Questions** | - How to handle rider-to-rider dependencies? |

---

### RIDER_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Rider Type |
| **Entity Definition** | A classification of Riders based on the type of benefit or coverage they provide. Used for analysis and reporting of rider attachment patterns. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Rider Type Code |
| **Key Relationships** | - Classifies Riders |
| **Assumptions** | - Core types: ACCELERATED_DEATH_BENEFIT, WAIVER_OF_PREMIUM, ACCIDENTAL_DEATH, CHILD_TERM, GUARANTEED_INSURABILITY, LONG_TERM_CARE, RETURN_OF_PREMIUM |
| **Open Questions** | - None at this time |

---

### PRODUCT_RIDER_ELIGIBILITY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Rider Eligibility |
| **Entity Definition** | The association defining which Riders are available for a specific Product, including any restrictions or requirements for rider attachment. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Product Identifier + Rider Identifier |
| **Key Relationships** | - Links Product to Rider<br>- May have eligibility rules and restrictions |
| **Assumptions** | - Eligibility may vary by state or underwriting class<br>- Some rider combinations may be prohibited |
| **Open Questions** | - Should eligibility rules be stored here or in a rules engine? |

---

### PRODUCT_FEATURE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Feature |
| **Entity Definition** | A specific characteristic, benefit, or capability included in a Product that differentiates it or provides value to customers. Features are inherent to the product and not optional like riders. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Feature Identifier |
| **Key Relationships** | - Associated with one or more Products (via Product Feature Assignment)<br>- Classified by Feature Type |
| **Assumptions** | - Features are included in base product pricing<br>- Examples: Guaranteed renewability, Conversion option, Living benefits |
| **Open Questions** | - How to distinguish features from product characteristics? |

---

### PRODUCT_FEATURE_ASSIGNMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Feature Assignment |
| **Entity Definition** | The association of a Product Feature with a specific Product, indicating that feature is included in the product offering. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Product Identifier + Feature Identifier |
| **Key Relationships** | - Links Product to Product Feature |
| **Assumptions** | - All assigned features are included in base product |
| **Open Questions** | - None at this time |

---

### FEATURE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Feature Type |
| **Entity Definition** | A classification of Product Features based on the type of benefit or capability they provide. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Feature Type Code |
| **Key Relationships** | - Classifies Product Features |
| **Assumptions** | - Core types: COVERAGE_FEATURE, FLEXIBILITY_FEATURE, GUARANTEE_FEATURE, VALUE_FEATURE |
| **Open Questions** | - None at this time |

---

### COVERAGE_OPTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Coverage Option |
| **Entity Definition** | A predefined face amount or coverage level available for a Product. Coverage Options define the amounts customers can select when purchasing a policy. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Coverage Option Identifier |
| **Key Relationships** | - Available for one or more Products<br>- Selected for Policy (Policy Domain) |
| **Assumptions** | - Options may be bands (100K-250K) or specific amounts<br>- Available options may vary by underwriting class |
| **Open Questions** | - Should coverage be continuous or predefined bands? |

---

### TERM_LENGTH

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Term Length |
| **Entity Definition** | For term life products, the duration of coverage in years. Defines the available term periods customers can select. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Term Length Code |
| **Key Relationships** | - Available for Term Products<br>- Selected for Policy (Policy Domain) |
| **Assumptions** | - Common terms: 10, 15, 20, 25, 30 years<br>- Some products may offer custom terms |
| **Open Questions** | - Will annual renewable term be offered? |

---

### PRODUCT_STATE_AVAILABILITY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product State Availability |
| **Entity Definition** | The regulatory approval status and availability of a Product in a specific state or jurisdiction. Tracks where products can be sold and any state-specific variations. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Product Identifier + State Code |
| **Key Relationships** | - Links Product to State (Geography Domain)<br>- May reference state-specific Product Version |
| **Assumptions** | - Products must be filed and approved in each state<br>- Availability has effective and end dates |
| **Open Questions** | - How to track pending state approvals? |

---

### PRODUCT_LIFECYCLE_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Lifecycle Status |
| **Entity Definition** | The current status of a Product in its lifecycle, from development through retirement. Indicates whether a product is available for new sales. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Lifecycle Status Code |
| **Key Relationships** | - Assigned to Products |
| **Assumptions** | - Core statuses: DEVELOPMENT, APPROVED, ACTIVE, CLOSED_TO_NEW_BUSINESS, RETIRED |
| **Open Questions** | - None at this time |

---

### RATE_TABLE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Rate Table |
| **Entity Definition** | A structured set of premium rates for a Product, typically varying by age, gender, tobacco status, and underwriting class. Rate Tables are the basis for premium calculations. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Rate Table Identifier |
| **Key Relationships** | - Belongs to Product Version<br>- Contains Rate Table Entries<br>- Used for Premium Calculation (Premium & Billing Domain) |
| **Assumptions** | - Rates are stored per thousand of coverage<br>- Multiple rate tables may exist for different rating factors |
| **Open Questions** | - Will rates be age-banded or specific ages? |

---

### RATE_TABLE_ENTRY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Rate Table Entry |
| **Entity Definition** | A single rate value within a Rate Table, representing the premium rate for a specific combination of rating factors (age, gender, tobacco status, underwriting class). |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Rate Table Identifier + Rating Factor Combination |
| **Key Relationships** | - Belongs to Rate Table |
| **Assumptions** | - Entry represents rate per $1,000 of coverage per period |
| **Open Questions** | - None at this time |

---

### UNDERWRITING_CLASS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Underwriting Class |
| **Entity Definition** | A risk classification assigned to applicants based on underwriting assessment, determining the premium rate tier. Classes range from preferred to substandard. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Underwriting Class Code |
| **Key Relationships** | - Determines Rate Table selection<br>- Assigned during Underwriting (Underwriting Domain)<br>- Referenced by Policy |
| **Assumptions** | - Core classes: PREFERRED_PLUS, PREFERRED, STANDARD_PLUS, STANDARD, SUBSTANDARD (with table ratings) |
| **Open Questions** | - How many substandard table ratings will be used? |

---

### PRODUCT_COMMISSION_STRUCTURE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Commission Structure |
| **Entity Definition** | The commission rates and structure applicable to a Product for sales compensation purposes. Defines how agents or channels are compensated for product sales. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Commission Structure Identifier |
| **Key Relationships** | - Associated with Product<br>- Applied to Sales (Sales & Distribution Domain)<br>- Used for Financial calculations (Financial Domain) |
| **Assumptions** | - May vary by distribution channel<br>- Includes first-year and renewal commissions |
| **Open Questions** | - Are direct-to-consumer sales commissioned differently? |

---

### PRODUCT_COMPARISON

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Product Comparison |
| **Entity Definition** | A defined comparison between two Products, capturing competitive positioning and differentiation points. Supports sales enablement and product selection guidance. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Comparison Identifier |
| **Key Relationships** | - Links two Products<br>- Used for Sales tools (Sales & Distribution Domain) |
| **Assumptions** | - Comparisons are maintained for active products<br>- May include competitor product comparisons |
| **Open Questions** | - Should competitor products be modeled? |

---

## Conceptual Entity-Relationship Diagram

```
┌─────────────────┐
│    PRODUCT      │
│    CATEGORY     │
└────────┬────────┘
         │ contains
         ▼
┌─────────────────┐
│  PRODUCT_LINE   │
└────────┬────────┘
         │ contains
         ▼
┌─────────────────┐        ┌─────────────────┐
│    PRODUCT      │◄───────│  PRODUCT_TYPE   │
│                 │        └─────────────────┘
└────────┬────────┘
         │
    ┌────┴────┬──────────────┬──────────────┬──────────────┐
    │         │              │              │              │
    ▼         ▼              ▼              ▼              ▼
┌─────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────┐
│PRODUCT  │ │PRODUCT   │ │PRODUCT   │ │ RATE     │ │  PRODUCT     │
│VERSION  │ │RIDER     │ │FEATURE   │ │ TABLE    │ │  STATE       │
│         │ │ELIGIBILITY│ │ASSIGNMENT│ │          │ │ AVAILABILITY │
└─────────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘ └──────────────┘
                 │            │            │
                 ▼            ▼            ▼
            ┌─────────┐ ┌──────────┐ ┌──────────┐
            │  RIDER  │ │ PRODUCT  │ │RATE_TABLE│
            │         │ │ FEATURE  │ │  ENTRY   │
            └────┬────┘ └────┬─────┘ └──────────┘
                 │           │
                 ▼           ▼
            ┌─────────┐ ┌──────────┐
            │ RIDER   │ │ FEATURE  │
            │  TYPE   │ │   TYPE   │
            └─────────┘ └──────────┘

┌─────────────────┐        ┌─────────────────┐
│   COVERAGE      │───────►│    PRODUCT      │
│    OPTION       │        │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│   TERM_LENGTH   │───────►│ PRODUCT (Term)  │
│                 │        │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│ UNDERWRITING    │◄───────│  RATE_TABLE     │
│    CLASS        │        │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│    PRODUCT      │───────►│    PRODUCT      │
│   LIFECYCLE     │        │                 │
│    STATUS       │        │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│    PRODUCT      │───────►│    PRODUCT      │
│   COMMISSION    │        │                 │
│   STRUCTURE     │        │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐
│    PRODUCT      │
│   COMPARISON    │──────► Two Products
└─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Product | Policy | Policy | Policy is issued for a specific Product |
| Product Version | Policy | Policy | Policy references Product Version at issue |
| Rider | Policy | Policy Rider | Riders attached to issued policies |
| Coverage Option | Policy | Coverage | Policy has selected coverage amount |
| Term Length | Policy | Policy | Term policy has selected term length |
| Underwriting Class | Underwriting | Risk Assessment | Underwriting assigns risk class |
| Rate Table | Premium & Billing | Premium Calculation | Rates determine premium amounts |
| Product | Sales & Distribution | Quote | Products are quoted to prospects |
| Product State Availability | Geography | State | Products available by state |
| Product Commission Structure | Financial Performance | Commission Expense | Commissions calculated per structure |
| Product | Claims | Claim | Claims filed against product policies |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Product | Master | Core insurance offering |
| Product Version | Master | Point-in-time product configuration |
| Product Line | Reference | High-level product grouping |
| Product Category | Reference | Highest-level classification |
| Product Type | Reference | Fundamental product structure type |
| Rider | Master | Optional supplementary benefit |
| Rider Type | Reference | Rider classification |
| Product Rider Eligibility | Reference | Product-Rider availability mapping |
| Product Feature | Reference | Included product characteristic |
| Product Feature Assignment | Reference | Product-Feature association |
| Feature Type | Reference | Feature classification |
| Coverage Option | Reference | Available face amount options |
| Term Length | Reference | Term duration options |
| Product State Availability | Reference | State-level product availability |
| Product Lifecycle Status | Reference | Product lifecycle stage |
| Rate Table | Reference | Premium rate structure |
| Rate Table Entry | Reference | Individual rate values |
| Underwriting Class | Reference | Risk classification tiers |
| Product Commission Structure | Reference | Sales compensation structure |
| Product Comparison | Reference | Product differentiation data |

**Total Entities: 20**

---

## Assumptions Log

1. Products have versioned configurations for change management
2. State-specific variations are handled through Product State Availability
3. Riders are priced separately from base products
4. Rate tables use standard per-$1,000 rating methodology
5. Underwriting classes follow industry-standard tier nomenclature
6. Product lifecycle is tracked from development through retirement
7. Commission structures may vary by distribution channel
8. Features are included in base product; riders are optional add-ons

---

## Open Questions

1. How to handle state-specific product variations vs. distinct products?
2. What changes require a new product version vs. amendment?
3. How to handle rider-to-rider dependencies and exclusions?
4. Should coverage be offered as continuous amounts or predefined bands?
5. Will annual renewable term products be offered?
6. How many substandard table ratings will be supported?
7. How are direct-to-consumer sales commissioned vs. agent sales?
8. Should competitor products be modeled for comparison?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
