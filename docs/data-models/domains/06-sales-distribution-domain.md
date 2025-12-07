# Sales & Distribution Domain
## Logical Data Model - Entity Definitions

**Domain:** Sales & Distribution  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Transaction & Event Domain

---

## Domain Overview

**Purpose:** Track customer acquisition, expansion sales, sales funnel progression, and distribution channels for both new business and existing customer growth.

**Scope:** This domain encompasses quotes, leads, sales funnel stages, marketing campaigns, distribution channels, attribution, and sales performance tracking for direct-to-consumer life insurance.

**Business Value:**
- Conversion funnel analysis and optimization
- Channel effectiveness and ROI measurement
- Marketing campaign performance tracking
- Customer acquisition cost analysis
- Cross-sell/upsell opportunity identification
- Lead source attribution

---

## Entity Catalog

### QUOTE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Quote |
| **Entity Definition** | A premium estimate provided to a prospect or customer for a specific product configuration. Quotes capture the proposed coverage, terms, and pricing before a formal application is submitted. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Quote Identifier |
| **Key Relationships** | - Requested by Prospect/Customer (Party Domain)<br>- For specific Product (Product Domain)<br>- May convert to Application (Underwriting Domain)<br>- Originated from Lead<br>- Attributed to Channel and Campaign |
| **Assumptions** | - Quotes have limited validity period<br>- Multiple quotes may exist per prospect |
| **Open Questions** | - What is the quote validity period? |

---

### QUOTE_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Quote Status |
| **Entity Definition** | The current state of a Quote in the sales funnel, from initial creation through conversion or expiration. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Assigned to Quote |
| **Assumptions** | - Core statuses: CREATED, VIEWED, SAVED, CONVERTED, EXPIRED, ABANDONED |
| **Open Questions** | - None at this time |

---

### QUOTE_CONFIGURATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Quote Configuration |
| **Entity Definition** | The specific product settings and options selected for a Quote, including coverage amount, term length, riders, and payment mode. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Quote Identifier + Configuration Version |
| **Key Relationships** | - Belongs to Quote<br>- References Product options |
| **Assumptions** | - Multiple configurations may be saved per quote<br>- Configuration includes all pricing inputs |
| **Open Questions** | - None at this time |

---

### LEAD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Lead |
| **Entity Definition** | A potential customer who has expressed interest in life insurance or has been identified as a prospect through marketing activities. Leads are tracked through the sales funnel until conversion or disqualification. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Lead Identifier |
| **Key Relationships** | - Becomes Prospect (Party Domain) when qualified<br>- Originates from Lead Source<br>- Attributed to Campaign<br>- Assigned to Channel<br>- May generate Quotes |
| **Assumptions** | - Leads may be created from multiple sources<br>- Lead quality is scored and tracked |
| **Open Questions** | - How are duplicate leads handled? |

---

### LEAD_SOURCE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Lead Source |
| **Entity Definition** | The origin of a Lead, identifying how the prospect was acquired or where they first engaged with the business. Lead sources enable attribution analysis. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Source Code |
| **Key Relationships** | - Originates Leads<br>- Grouped by Source Category |
| **Assumptions** | - Sources: ORGANIC_SEARCH, PAID_SEARCH, SOCIAL, EMAIL, REFERRAL, PARTNER, DIRECT |
| **Open Questions** | - None at this time |

---

### LEAD_SOURCE_CATEGORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Lead Source Category |
| **Entity Definition** | A high-level grouping of Lead Sources for aggregate analysis and reporting. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Category Code |
| **Key Relationships** | - Groups Lead Sources |
| **Assumptions** | - Categories: PAID, ORGANIC, PARTNER, DIRECT |
| **Open Questions** | - None at this time |

---

### LEAD_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Lead Status |
| **Entity Definition** | The current state of a Lead in the qualification and conversion process. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Assigned to Lead |
| **Assumptions** | - Core statuses: NEW, CONTACTED, QUALIFIED, NURTURING, CONVERTED, DISQUALIFIED, DEAD |
| **Open Questions** | - None at this time |

---

### LEAD_SCORE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Lead Score |
| **Entity Definition** | A calculated score representing the quality and conversion likelihood of a Lead, based on demographic fit, engagement signals, and behavioral data. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Lead Identifier + Score Date |
| **Key Relationships** | - Calculated for Lead<br>- Produced by Lead Scoring Model |
| **Assumptions** | - Scores are recalculated as engagement changes<br>- Higher scores indicate better conversion potential |
| **Open Questions** | - What scoring model will be used? |

---

### SALES_FUNNEL_STAGE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Sales Funnel Stage |
| **Entity Definition** | A defined stage in the customer acquisition journey, from initial awareness through policy purchase. Stages enable funnel analysis and conversion optimization. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Stage Code |
| **Key Relationships** | - Tracks Lead/Prospect progression<br>- Has defined sequence/order |
| **Assumptions** | - Stages: AWARENESS, INTEREST, CONSIDERATION, QUOTE, APPLICATION, UNDERWRITING, PURCHASE |
| **Open Questions** | - None at this time |

---

### FUNNEL_PROGRESSION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Funnel Progression |
| **Entity Definition** | A record of a Lead or Prospect moving from one Sales Funnel Stage to another, capturing conversion timestamps and enabling funnel velocity analysis. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Progression Identifier |
| **Key Relationships** | - Tracks Lead/Prospect movement<br>- From Stage to Stage<br>- Records timestamp and channel |
| **Assumptions** | - Progression can be forward or backward (fall back)<br>- Time in stage is calculated from progression records |
| **Open Questions** | - None at this time |

---

### CHANNEL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Channel |
| **Entity Definition** | A distribution channel through which sales and customer interactions occur. For direct-to-consumer insurance, channels include website, mobile app, call center, and partners. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Channel Code |
| **Key Relationships** | - Used for Sales attribution<br>- Used for Service interactions (Customer Engagement Domain)<br>- Used for Payment channels (Premium & Billing Domain)<br>- Has Channel Type |
| **Assumptions** | - Cross-cutting dimension used across multiple domains<br>- Channels: WEBSITE, MOBILE_APP, CALL_CENTER, PARTNER, DIRECT_MAIL |
| **Open Questions** | - None at this time |

---

### CHANNEL_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Channel Type |
| **Entity Definition** | A classification of Channels based on their nature - digital vs. traditional, self-service vs. assisted. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Channel |
| **Assumptions** | - Types: DIGITAL_SELF_SERVICE, DIGITAL_ASSISTED, VOICE, PHYSICAL |
| **Open Questions** | - None at this time |

---

### CAMPAIGN

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Campaign |
| **Entity Definition** | A marketing initiative designed to generate leads, drive engagement, or promote specific products. Campaigns have defined budgets, timeframes, and performance targets. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Campaign Identifier |
| **Key Relationships** | - Generates Leads<br>- Attributed to Quotes and Applications<br>- Has Campaign Type<br>- Uses Channels<br>- Has Budget allocation |
| **Assumptions** | - Campaigns may span multiple channels<br>- Attribution models determine credit allocation |
| **Open Questions** | - What attribution model will be used? |

---

### CAMPAIGN_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Campaign Type |
| **Entity Definition** | A classification of Campaigns based on their objective and tactics. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Campaign |
| **Assumptions** | - Types: ACQUISITION, RETENTION, CROSS_SELL, BRAND, WINBACK, SEASONAL |
| **Open Questions** | - None at this time |

---

### CAMPAIGN_CHANNEL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Campaign Channel |
| **Entity Definition** | The association of a Campaign with specific marketing channels used for distribution, enabling channel-level campaign performance analysis. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Campaign Identifier + Channel Code |
| **Key Relationships** | - Links Campaign to Channel<br>- Tracks channel-specific spend and performance |
| **Assumptions** | - Budget may be allocated differently by channel<br>- Performance tracked by channel |
| **Open Questions** | - None at this time |

---

### ATTRIBUTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Attribution |
| **Entity Definition** | The assignment of conversion credit to marketing touchpoints (campaigns, channels, sources) that influenced a customer's purchase decision. Supports marketing ROI analysis. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Attribution Identifier |
| **Key Relationships** | - Links Conversion to Campaign/Channel/Source<br>- Uses Attribution Model<br>- Has Attribution Weight |
| **Assumptions** | - Multi-touch attribution supported<br>- Credit may be split across touchpoints |
| **Open Questions** | - Which attribution model(s) will be implemented? |

---

### ATTRIBUTION_MODEL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Attribution Model |
| **Entity Definition** | A defined methodology for allocating conversion credit to marketing touchpoints. Different models weight touchpoints differently (first-touch, last-touch, linear, etc.). |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Model Code |
| **Key Relationships** | - Used for Attribution calculations |
| **Assumptions** | - Models: FIRST_TOUCH, LAST_TOUCH, LINEAR, TIME_DECAY, POSITION_BASED |
| **Open Questions** | - None at this time |

---

### TOUCHPOINT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Touchpoint |
| **Entity Definition** | A single interaction between a prospect/customer and the business during the sales journey. Touchpoints are tracked for attribution and customer journey analysis. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Touchpoint Identifier |
| **Key Relationships** | - Associated with Lead/Prospect<br>- Occurs via Channel<br>- May be attributed to Campaign<br>- Has Touchpoint Type |
| **Assumptions** | - Digital touchpoints captured automatically<br>- Offline touchpoints may require manual capture |
| **Open Questions** | - How are offline touchpoints captured? |

---

### TOUCHPOINT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Touchpoint Type |
| **Entity Definition** | A classification of Touchpoints based on the nature of the interaction. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Touchpoint |
| **Assumptions** | - Types: WEBSITE_VISIT, AD_CLICK, EMAIL_OPEN, CALL, CHAT, QUOTE_REQUEST, APPLICATION_START |
| **Open Questions** | - None at this time |

---

### SALE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Sale |
| **Entity Definition** | A completed policy purchase transaction, representing the successful conversion of a prospect to a customer. Sales track the final attribution and revenue details. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Sale Identifier |
| **Key Relationships** | - Results from Application approval (Underwriting Domain)<br>- Creates Policy (Policy Domain)<br>- Attributed to Channel, Campaign, and Source<br>- Has Sale Type |
| **Assumptions** | - Sale recorded at policy issue<br>- Revenue metrics calculated from premium |
| **Open Questions** | - None at this time |

---

### SALE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Sale Type |
| **Entity Definition** | A classification of Sales based on whether it represents new customer acquisition or growth from an existing customer. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Sale |
| **Assumptions** | - Types: NEW_BUSINESS, CROSS_SELL, UPSELL, COVERAGE_INCREASE, REINSTATEMENT |
| **Open Questions** | - None at this time |

---

### CUSTOMER_ACQUISITION_COST

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer Acquisition Cost |
| **Entity Definition** | The calculated cost to acquire a customer through specific channels or campaigns, including marketing spend, operational costs, and commissions. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | CAC Calculation Identifier |
| **Key Relationships** | - Calculated for Sale<br>- Aggregated by Channel, Campaign<br>- Input to Financial analysis (Financial Domain) |
| **Assumptions** | - CAC includes direct and allocated costs<br>- Calculated at various levels (total, channel, campaign) |
| **Open Questions** | - What costs are included in CAC calculation? |

---

### PARTNER

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Partner |
| **Entity Definition** | An external business entity that refers leads or facilitates sales through their platform or customer base. Partners may include affiliates, aggregators, or strategic alliances. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Partner Identifier |
| **Key Relationships** | - Generates Leads<br>- Has Partner Agreement<br>- Receives Commission (Financial Domain)<br>- Has Partner Type |
| **Assumptions** | - Partners have contractual agreements<br>- Partner performance is tracked |
| **Open Questions** | - What partner types will be supported? |

---

### PARTNER_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Partner Type |
| **Entity Definition** | A classification of Partners based on their business relationship and integration model. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Partner |
| **Assumptions** | - Types: AFFILIATE, AGGREGATOR, EMBEDDED, STRATEGIC, REFERRAL |
| **Open Questions** | - None at this time |

---

### PARTNER_AGREEMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Partner Agreement |
| **Entity Definition** | The contractual terms governing a Partner relationship, including commission rates, service levels, and performance requirements. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Agreement Identifier |
| **Key Relationships** | - Governs Partner relationship<br>- Defines commission terms<br>- Has effective period |
| **Assumptions** | - Agreements are versioned<br>- Terms may vary by product or volume |
| **Open Questions** | - None at this time |

---

### CONVERSION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Conversion |
| **Entity Definition** | An event representing progress through the sales funnel - a lead becoming qualified, a quote being generated, an application submitted, or a policy purchased. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Conversion Identifier |
| **Key Relationships** | - Tracks Lead/Prospect/Quote progression<br>- Has Conversion Type<br>- Attributed to touchpoints |
| **Assumptions** | - Multiple conversion types tracked separately<br>- Conversion rates calculated between stages |
| **Open Questions** | - None at this time |

---

### CONVERSION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Conversion Type |
| **Entity Definition** | A classification of Conversions based on the milestone achieved in the sales funnel. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Conversion |
| **Assumptions** | - Types: LEAD_QUALIFIED, QUOTE_GENERATED, QUOTE_SAVED, APPLICATION_STARTED, APPLICATION_SUBMITTED, POLICY_ISSUED |
| **Open Questions** | - None at this time |

---

### CROSS_SELL_OPPORTUNITY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Cross-Sell Opportunity |
| **Entity Definition** | An identified opportunity to sell additional products or coverage to an existing customer, based on their profile, holdings, and propensity models. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Opportunity Identifier |
| **Key Relationships** | - Identified for Customer (Party Domain)<br>- Recommends Product (Product Domain)<br>- Has Opportunity Score<br>- Has Opportunity Status |
| **Assumptions** | - Opportunities generated by analytics/models<br>- Tracked through disposition |
| **Open Questions** | - What models generate cross-sell opportunities? |

---

### OPPORTUNITY_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Opportunity Status |
| **Entity Definition** | The current state of a Cross-Sell Opportunity from identification through conversion or closure. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Assigned to Cross-Sell Opportunity |
| **Assumptions** | - Statuses: IDENTIFIED, CONTACTED, QUOTED, CONVERTED, DECLINED, EXPIRED |
| **Open Questions** | - None at this time |

---

### REFERRAL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Referral |
| **Entity Definition** | A lead generated through recommendation by an existing customer or partner. Referrals track the referring party and any incentives provided. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Referral Identifier |
| **Key Relationships** | - Made by Referrer (Party Domain)<br>- Creates Lead<br>- May earn Referral Reward |
| **Assumptions** | - Referral program terms define eligibility<br>- Rewards paid upon qualifying events |
| **Open Questions** | - Will a customer referral program be offered? |

---

## Conceptual Entity-Relationship Diagram

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  LEAD_SOURCE    │────────►│      LEAD       │◄────────│   CAMPAIGN      │
│   CATEGORY      │         │                 │         │                 │
└────────┬────────┘         └────────┬────────┘         └────────┬────────┘
         │                           │                           │
         ▼                           │                           ▼
┌─────────────────┐                  │                  ┌─────────────────┐
│  LEAD_SOURCE    │──────────────────┘                  │ CAMPAIGN_TYPE   │
└─────────────────┘                                     └─────────────────┘
                                     │
         ┌───────────────────────────┼───────────────────────────┐
         │                           │                           │
         ▼                           ▼                           ▼
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  LEAD_STATUS    │         │   LEAD_SCORE    │         │ FUNNEL_         │
│                 │         │                 │         │ PROGRESSION     │
└─────────────────┘         └─────────────────┘         └────────┬────────┘
                                                                 │
                                                                 ▼
                                                        ┌─────────────────┐
                                                        │SALES_FUNNEL     │
                                                        │   STAGE         │
                                                        └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  PROSPECT       │────────►│     QUOTE       │────────►│  APPLICATION    │
│ (Party Domain)  │requests │                 │converts │(Underwriting)   │
└─────────────────┘         └────────┬────────┘         └────────┬────────┘
                                     │                           │
                                     ▼                           │
                            ┌─────────────────┐                  │
                            │ QUOTE_CONFIG    │                  │
                            └─────────────────┘                  │
                                                                 ▼
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  CHANNEL_TYPE   │────────►│    CHANNEL      │◄────────│     SALE        │
│                 │         │                 │         │                 │
└─────────────────┘         └─────────────────┘         └────────┬────────┘
                                     │                           │
                                     ▼                           ▼
                            ┌─────────────────┐         ┌─────────────────┐
                            │CAMPAIGN_CHANNEL │         │   SALE_TYPE     │
                            └─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  TOUCHPOINT     │────────►│  ATTRIBUTION    │◄────────│ ATTRIBUTION     │
│                 │         │                 │         │    MODEL        │
└────────┬────────┘         └─────────────────┘         └─────────────────┘
         │
         ▼
┌─────────────────┐
│ TOUCHPOINT_TYPE │
└─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│ PARTNER_TYPE    │────────►│    PARTNER      │────────►│ PARTNER_        │
│                 │         │                 │         │ AGREEMENT       │
└─────────────────┘         └─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│ CUSTOMER_       │◄────────│     SALE        │
│ ACQUISITION_COST│         │                 │
└─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│CROSS_SELL       │◄────────│   CUSTOMER      │         │  CONVERSION     │
│ OPPORTUNITY     │         │ (Party Domain)  │         │                 │
└────────┬────────┘         └─────────────────┘         └────────┬────────┘
         │                                                       │
         ▼                                                       ▼
┌─────────────────┐                                     ┌─────────────────┐
│ OPPORTUNITY     │                                     │CONVERSION_TYPE  │
│   STATUS        │                                     │                 │
└─────────────────┘                                     └─────────────────┘

┌─────────────────┐
│   REFERRAL      │
└─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Lead | Party/Customer | Prospect | Lead becomes prospect when qualified |
| Quote | Party/Customer | Customer/Prospect | Quote requested by customer/prospect |
| Quote | Product | Product | Quote for specific product |
| Sale | Policy | Policy | Sale creates policy |
| Sale | Underwriting | Application | Sale results from approved application |
| Channel | Customer Engagement | Interaction | Service interactions use channels |
| Channel | Premium & Billing | Payment | Payments made through channels |
| Campaign | Financial Performance | Marketing Expense | Campaign costs tracked |
| Partner | Financial Performance | Commission | Partner commissions paid |
| Customer Acquisition Cost | Financial Performance | Expense | CAC is financial metric |
| Cross-Sell Opportunity | Party/Customer | Customer | Opportunities for existing customers |
| Touchpoint | Customer Engagement | Interaction | Customer journey touchpoints |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Quote | Transactional | Premium estimate for prospect |
| Quote Status | Reference | Quote lifecycle state |
| Quote Configuration | Transactional | Product options in quote |
| Lead | Transactional | Potential customer |
| Lead Source | Reference | Lead origin |
| Lead Source Category | Reference | Source grouping |
| Lead Status | Reference | Lead qualification state |
| Lead Score | Transactional | Lead quality score |
| Sales Funnel Stage | Reference | Acquisition journey stage |
| Funnel Progression | Transactional | Stage movement record |
| Channel | Reference | Distribution/interaction channel |
| Channel Type | Reference | Channel classification |
| Campaign | Master | Marketing initiative |
| Campaign Type | Reference | Campaign classification |
| Campaign Channel | Transactional | Campaign-channel association |
| Attribution | Transactional | Conversion credit assignment |
| Attribution Model | Reference | Attribution methodology |
| Touchpoint | Transactional | Customer interaction |
| Touchpoint Type | Reference | Interaction classification |
| Sale | Transactional | Completed purchase |
| Sale Type | Reference | New vs. expansion sale |
| Customer Acquisition Cost | Transactional | Acquisition cost calculation |
| Partner | Master | External business partner |
| Partner Type | Reference | Partner classification |
| Partner Agreement | Master | Partner contract terms |
| Conversion | Transactional | Funnel milestone event |
| Conversion Type | Reference | Milestone classification |
| Cross-Sell Opportunity | Transactional | Expansion sales opportunity |
| Opportunity Status | Reference | Opportunity state |
| Referral | Transactional | Customer/partner referral |

**Total Entities: 30**

---

## Assumptions Log

1. Quotes have defined validity periods
2. Multi-touch attribution is supported
3. Channels are cross-cutting dimension used across domains
4. Campaigns may span multiple channels
5. Partner relationships governed by contractual agreements
6. Customer acquisition cost includes direct and allocated costs
7. Cross-sell opportunities generated by propensity models
8. Digital touchpoints captured automatically
9. Lead scores recalculated as engagement changes

---

## Open Questions

1. What is the quote validity period?
2. How are duplicate leads handled?
3. What lead scoring model will be used?
4. What attribution model(s) will be implemented?
5. How are offline touchpoints captured?
6. What costs are included in CAC calculation?
7. What partner types will be supported?
8. What models generate cross-sell opportunities?
9. Will a customer referral program be offered?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
