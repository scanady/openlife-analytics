# Cross-Domain Entity Mapping
## Life Insurance Data Model - Entity Relationships Across Domains

**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification Complete

---

## Document Purpose

This document provides a comprehensive cross-reference of entities across all 13 subject area domains in the life insurance logical data model. It serves as a master reference for understanding how entities relate across domain boundaries.

---

## Domain Summary

| # | Domain | Entity Count | Primary Focus |
|---|--------|-------------|---------------|
| 1 | Party/Customer | 12 | People and organizations |
| 2 | Product | 12 | Insurance offerings |
| 3 | Policy | 14 | Policy lifecycle |
| 4 | Time/Calendar | 10 | Temporal dimensions |
| 5 | Underwriting | 14 | Risk assessment |
| 6 | Sales & Distribution | 14 | Customer acquisition |
| 7 | Premium & Billing | 14 | Financial transactions |
| 8 | Claims | 14 | Benefit fulfillment |
| 9 | Fraud Detection | 12 | Risk mitigation |
| 10 | Geography/Location | 12 | Spatial dimensions |
| 11 | Financial Performance | 31 | Financial metrics |
| 12 | Customer Engagement | 31 | Customer experience |
| 13 | Compliance/Regulatory | 36 | Regulatory adherence |
| | **TOTAL** | **226** | |

---

## Cross-Domain Relationship Matrix

### Policy as Central Hub

The **Policy** entity serves as the central hub connecting most domains:

```
                                    ┌─────────────────┐
                                    │    PRODUCT      │
                                    │     Domain      │
                                    └────────┬────────┘
                                             │
                                    instance of
                                             │
┌─────────────────┐                         ▼                    ┌─────────────────┐
│  UNDERWRITING   │───application────►┌─────────────────┐◄──────│  PARTY/CUSTOMER │
│     Domain      │   results in      │                 │ owned │     Domain      │
└─────────────────┘                   │     POLICY      │  by   └─────────────────┘
                                      │                 │
┌─────────────────┐                   │  (Policy Domain)│        ┌─────────────────┐
│    SALES &      │────quote to───────│                 │◄───────│   GEOGRAPHY     │
│  DISTRIBUTION   │    policy         │                 │location│     Domain      │
└─────────────────┘                   └────────┬────────┘        └─────────────────┘
                                               │
              ┌────────────────────────────────┼────────────────────────────────┐
              │                                │                                │
              ▼                                ▼                                ▼
┌─────────────────┐               ┌─────────────────┐               ┌─────────────────┐
│   PREMIUM &     │               │     CLAIMS      │               │   FINANCIAL     │
│    BILLING      │               │     Domain      │               │  PERFORMANCE    │
└─────────────────┘               └─────────────────┘               └─────────────────┘
              │                                │                                │
              └────────────────────────────────┼────────────────────────────────┘
                                               │
                          ┌────────────────────┴────────────────────┐
                          ▼                                         ▼
             ┌─────────────────┐                        ┌─────────────────┐
             │     FRAUD       │                        │   COMPLIANCE    │
             │   DETECTION     │                        │   REGULATORY    │
             └─────────────────┘                        └─────────────────┘
```

---

## Domain-to-Domain Relationship Detail

### 1. Party/Customer Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Policy | Party owns/is insured under Policy | Customer → Policy |
| Policy | Party is beneficiary of Policy | Beneficiary → Policy Beneficiary Designation |
| Product | Party has product preferences | Customer → Product |
| Underwriting | Party submits Application | Individual → Application |
| Sales & Distribution | Party is Lead | Customer → Lead |
| Premium & Billing | Party makes Payments | Customer → Payment |
| Claims | Party is Claimant | Beneficiary → Claimant |
| Customer Engagement | Party has Interactions | Customer → Customer Interaction |
| Compliance | Party has Consent | Customer → Privacy Consent |
| Compliance | Producer has License | Producer → Producer License |
| Geography | Party has Address | Party → Address |
| Fraud Detection | Party may be on Watchlist | Party → Fraud Watchlist |

### 2. Product Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Policy | Policy is instance of Product | Product → Policy |
| Underwriting | Product has eligibility rules | Product → Product Eligibility Rule |
| Sales & Distribution | Product is quoted | Product → Quote |
| Premium & Billing | Product determines rates | Product Rate Table → Premium Schedule |
| Financial Performance | Product has profitability | Product → Profitability Metric |
| Compliance | Product forms require filing | Product → Form Filing |
| Compliance | Product rates require filing | Product → Rate Filing |

### 3. Policy Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Party/Customer | Policy owned by Customer | Policy → Customer |
| Product | Policy is instance of Product | Policy → Product |
| Underwriting | Application becomes Policy | Application → Policy |
| Premium & Billing | Policy has Premium Schedule | Policy → Premium Schedule |
| Claims | Claim filed against Policy | Policy → Claim |
| Financial Performance | Policy has Reserve | Policy → Policy Reserve |
| Customer Engagement | Service Request about Policy | Policy → Service Request |
| Compliance | Policy referenced in Complaint | Policy → Regulatory Complaint |
| Geography | Policy has risk location | Policy → Address |
| Time/Calendar | Policy has duration/anniversary | Policy → Date |

### 4. Time/Calendar Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Policy | Policy has effective/expiry dates | Date → Policy |
| Premium & Billing | Payment has transaction date | Date → Payment |
| Claims | Claim has occurrence date | Date → Claim |
| Financial Performance | Reporting uses Fiscal Period | Fiscal Period → Financial Transaction |
| Compliance | Filings have deadlines | Date → Regulatory Filing |
| All Domains | Dimensional time reference | Date → All transactional entities |

### 5. Underwriting Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Party/Customer | Application from Individual | Individual → Application |
| Product | Application for Product | Application → Product |
| Policy | Approved Application → Policy | Application → Policy |
| Sales & Distribution | Application from Quote | Quote → Application |
| Fraud Detection | Fraud check during UW | Application → Fraud Alert |
| Compliance | UW rules per jurisdiction | Underwriting Rule → Regulatory Requirement |

### 6. Sales & Distribution Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Party/Customer | Lead is potential Customer | Lead → Customer |
| Product | Quote for Product | Quote → Product |
| Underwriting | Quote → Application | Quote → Application |
| Policy | Quote → Policy conversion | Quote → Policy |
| Premium & Billing | Commission from sale | Sale → Commission Transaction |
| Financial Performance | Commission as expense | Commission Transaction → Commission Expense |
| Customer Engagement | Marketing Campaign response | Campaign → Campaign Response |

### 7. Premium & Billing Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Policy | Premium for Policy | Premium Schedule → Policy |
| Party/Customer | Payment from Customer | Payment → Customer |
| Financial Performance | Premium as revenue | Premium Transaction → Premium Revenue |
| Financial Performance | Commission from Premium | Commission Transaction → Commission Expense |
| Customer Engagement | Billing inquiry interaction | Premium Notice → Customer Interaction |
| Compliance | Payment audit trail | Payment → Audit Trail |

### 8. Claims Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Policy | Claim against Policy | Claim → Policy |
| Party/Customer | Claimant is Beneficiary | Claimant → Beneficiary |
| Premium & Billing | Claim payout transaction | Claim Payment → Payment |
| Financial Performance | Claim as benefit payment | Claim Payment → Benefit Payment |
| Fraud Detection | Claim triggers fraud check | Claim → Fraud Alert |
| Customer Engagement | Claim status interaction | Claim → Customer Interaction |
| Compliance | Claim complaint to regulator | Claim → Regulatory Complaint |

### 9. Fraud Detection Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Underwriting | Fraud check on Application | Application → Fraud Alert |
| Claims | Fraud check on Claim | Claim → Fraud Alert |
| Party/Customer | Party on Watchlist | Party → Fraud Watchlist |
| Premium & Billing | Suspicious payment pattern | Payment → Fraud Pattern |
| Compliance | SAR filing | Fraud Case → Suspicious Activity Report |

### 10. Geography/Location Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Party/Customer | Party has Address | Party → Address |
| Policy | Policy risk location | Policy → Address |
| Sales & Distribution | Territory assignment | Sales Territory → Partner |
| Compliance | Jurisdiction for licensing | Regulatory Jurisdiction → Producer License |
| Compliance | Filing by jurisdiction | State → Regulatory Filing |
| Product | Product availability by state | State → Product Availability |

### 11. Financial Performance Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Policy | Reserve per Policy | Policy → Policy Reserve |
| Premium & Billing | Premium becomes Revenue | Premium Transaction → Premium Revenue |
| Claims | Claim becomes Benefit | Claim Payment → Benefit Payment |
| Sales & Distribution | Commission as Expense | Commission Transaction → Commission Expense |
| Product | Profitability by Product | Product → Profitability Metric |
| Time/Calendar | Reporting by Period | Financial Period → Fiscal Period |

### 12. Customer Engagement Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Party/Customer | Interaction with Customer | Customer → Customer Interaction |
| Policy | Service Request about Policy | Policy → Service Request |
| Claims | Claim inquiry | Claim → Customer Interaction |
| Premium & Billing | Billing inquiry | Premium Notice → Customer Interaction |
| Compliance | Complaint escalation | Complaint → Regulatory Complaint |
| Sales & Distribution | Quote-to-purchase journey | Quote → Customer Journey |

### 13. Compliance/Regulatory Domain Relationships

| Related Domain | Relationship | Key Entities Involved |
|---------------|--------------|----------------------|
| Party/Customer | Producer has License | Producer → Producer License |
| Party/Customer | Customer gives Consent | Customer → Privacy Consent |
| Geography | Filing to Jurisdiction | Regulatory Filing → State |
| Product | Rate/Form Filing | Product → Rate Filing, Form Filing |
| Customer Engagement | Complaint escalation | Complaint → Regulatory Complaint |
| Fraud Detection | SAR Filing | Fraud Case → Suspicious Activity Report |
| All Domains | Audit Trail | Audit Trail → All entities |

---

## Key Entity Integration Points

### Master Data Entities (Cross-Domain Reference)

| Entity | Home Domain | Referenced By |
|--------|-------------|---------------|
| Customer | Party/Customer | All transactional domains |
| Product | Product | Policy, Sales, Financial |
| Policy | Policy | Claims, Billing, Financial, Engagement |
| Date | Time/Calendar | All transactional entities |
| Address | Geography | Party, Policy |
| State | Geography | Compliance, Product |

### Shared Transactional Patterns

| Pattern | Entities | Domains |
|---------|----------|---------|
| Money Flow | Payment, Premium, Commission, Benefit | Billing, Sales, Financial, Claims |
| Investigation | UW Assessment, Claim Investigation, Fraud Investigation, AML Case | Underwriting, Claims, Fraud, Compliance |
| Request/Case | Service Request, Claim, Fraud Case, DSR, AML Case | Engagement, Claims, Fraud, Compliance |
| Status Tracking | Policy Status, Claim Status, Request Status, Filing Status | Policy, Claims, Engagement, Compliance |

---

## Entity Type Distribution

| Entity Type | Count | Percentage |
|-------------|-------|------------|
| Transactional | 127 | 56% |
| Reference Data | 72 | 32% |
| Master Data | 27 | 12% |
| **Total** | **226** | **100%** |

---

## Domain Dependencies

### Core Foundation (Must be defined first)
1. **Party/Customer** - All transactions involve parties
2. **Product** - Policies are product instances
3. **Policy** - Central business entity
4. **Time/Calendar** - All events are temporal

### Dependent Domains (Build on foundation)
5. **Underwriting** - Depends on Party, Product, Policy
6. **Sales & Distribution** - Depends on Party, Product
7. **Premium & Billing** - Depends on Policy, Party
8. **Claims** - Depends on Policy, Party

### Supporting Domains (Cross-cutting concerns)
9. **Fraud Detection** - Monitors Underwriting, Claims, Billing
10. **Geography/Location** - Referenced by Party, Policy, Compliance
11. **Financial Performance** - Aggregates from Billing, Claims
12. **Customer Engagement** - Tracks all customer touchpoints
13. **Compliance/Regulatory** - Governs all operations

---

## Implementation Recommendations

### Phase 1: Core Foundation
- Implement Party/Customer, Product, Policy, Time/Calendar domains first
- Establish master data management patterns
- Define shared reference data standards

### Phase 2: Transactional Core
- Implement Underwriting, Sales, Billing, Claims
- Integrate with core entities
- Establish event-driven patterns

### Phase 3: Supporting Functions
- Implement Fraud, Geography, Financial Performance
- Build analytics and reporting
- Implement monitoring and alerting

### Phase 4: Experience & Compliance
- Implement Customer Engagement, Compliance
- Complete audit trail integration
- Enable regulatory reporting

---

## Open Integration Questions

1. **Event Sourcing**: Should domain events drive cross-domain updates?
2. **Data Ownership**: How to handle shared entities (e.g., Address)?
3. **Consistency**: Eventual vs. strong consistency across domains?
4. **Integration Pattern**: API-first, event-driven, or hybrid?
5. **Master Data**: Centralized MDM or federated?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Cross-domain mapping complete |

---

*This document will be updated as entity attributes and relationships are refined in subsequent phases.*
