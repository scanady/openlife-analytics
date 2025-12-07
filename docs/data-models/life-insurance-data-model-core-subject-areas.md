# Direct-to-Consumer Life Insurance
## Logical Data Model - Top-Level Subject Areas

**Version:** 1.0  
**Date:** December 6, 2025  
**Purpose:** Enterprise data model supporting business intelligence and analytics

---

## Overview

This document defines the top-level subject area domains for a logical data model designed to support comprehensive business intelligence and analytics capabilities for a direct-to-consumer life insurance business. These 13 domains represent the major subject areas around which detailed entities, attributes, and relationships will be organized.

---

## 1. Party/Customer Domain

**Purpose:** Manage all information about individuals and organizations that interact with the business.

**Key Subject Areas:**
- Individual customers and prospects
- Beneficiaries and contingent beneficiaries
- Legal guardians and trustees
- Customer demographics and profiles
- Contact information (addresses, phone, email)
- Customer relationships and household structures
- Identity verification and KYC (Know Your Customer) data
- Customer segmentation and classification
- Customer lifetime value (CLV) metrics

**Primary Analytics:**
- Customer acquisition and retention rates
- Demographic distribution and trends
- Customer lifetime value analysis
- Household penetration metrics
- Geographic concentration

---

## 2. Product Domain

**Purpose:** Define all life insurance products, features, and offerings available to customers.

**Key Subject Areas:**
- Life insurance product catalog (term, whole, universal, variable)
- Riders and supplementary benefits
- Product features and coverage options
- Pricing structures and rate tables
- Product hierarchies and classifications
- Product lifecycle (launch, active, retired)
- Product profitability metrics
- Competitive positioning

**Primary Analytics:**
- Product mix and performance
- Product profitability analysis
- Product penetration by segment
- Rider attachment rates
- Cross-sell and upsell opportunities

---

## 3. Policy Domain

**Purpose:** Manage the lifecycle and details of insurance policy contracts.

**Key Subject Areas:**
- Policy contracts and agreements
- Coverage details and face amounts
- Policy status (active, lapsed, terminated, in-force)
- Effective dates and policy terms
- Policy modifications and endorsements
- Ownership and assignment changes
- Policy anniversaries and key dates
- Grace periods and reinstatements
- Policy reserves and cash values

**Primary Analytics:**
- In-force policy counts and face amounts
- Policy persistency and lapse rates
- Policy vintage analysis
- Policy modification trends
- Surrender rate analysis

---

## 4. Underwriting Domain

**Purpose:** Manage risk assessment, classification, and underwriting decisions.

**Key Subject Areas:**
- Underwriting applications and submissions
- Medical history and health questionnaires
- Risk classification and ratings
- Underwriting decisions (approved, declined, modified)
- Underwriting rules and guidelines
- Medical exams and laboratory results
- Third-party data (MIB, prescription, motor vehicle)
- Risk scores and mortality ratings
- Underwriting exceptions and overrides

**Primary Analytics:**
- Approval/decline rates
- Underwriting cycle time
- Risk distribution analysis
- Mortality experience vs. expected
- Underwriting quality metrics

---

## 5. Sales & Distribution Domain

**Purpose:** Track customer acquisition, expansion sales, sales funnel, and distribution channels.

**Key Subject Areas:**
- Quote generation and pricing
- Application funnel (quotes, applications, issued)
- **New business acquisition** (first policy sales)
- **Customer expansion** (additional policies, up-sell, cross-sell, coverage increases)
- Sales type classification (new vs. existing customer)
- **Distribution channels** (website, mobile app, partner sites, phone/call center, direct mail)
- Channel attribution and customer journey mapping
- Marketing campaigns and programs
- Lead generation and attribution
- Conversion rates by stage and channel
- Customer acquisition costs (CAC)
- Partner/affiliate relationships
- Sales representative performance (if applicable)
- Cross-channel interactions and touchpoints

**Primary Analytics:**
- Conversion funnel analysis (new vs. expansion)
- Channel effectiveness and ROI by sales type
- Marketing campaign performance
- Lead source attribution
- Cost per acquisition by channel
- Quote-to-issue ratios
- Customer expansion revenue and rates
- Cross-sell/up-sell conversion rates
- Policies per customer
- Channel preference by customer segment
- Cross-channel customer journey analysis

---

## 6. Premium & Billing Domain

**Purpose:** Manage premium calculations, billing, payments, and collections.

**Key Subject Areas:**
- Premium calculations and schedules
- Modal premiums (monthly, quarterly, annual)
- Payment transactions and methods
- **Payment channels** (ACH, credit card, check, mobile payment, recurring billing)
- Billing cycles and due dates
- Payment collections and receipts
- Late payments and grace periods
- Premium rate changes
- Automatic payment arrangements
- Lapse for non-payment
- Reinstatement premiums
- Payment channel preferences and migration

**Primary Analytics:**
- Premium income and growth rates
- Payment persistency rates
- Collection effectiveness
- Payment method/channel distribution
- Lapse rate analysis
- Premium yield metrics
- Payment channel cost-to-serve
- Automatic payment adoption rates

---

## 7. Claims Domain

**Purpose:** Manage claims submission, processing, and settlement.

**Key Subject Areas:**
- Death claims and notifications
- Claim submissions and documentation
- Beneficiary verification
- Claim adjudication and investigation
- Claim approvals and denials
- Payout amounts and settlements
- Surrender requests and processing
- Partial withdrawals and loans
- Contestability period claims
- Claim reserves and IBNR (Incurred But Not Reported)

**Primary Analytics:**
- Claims incidence and frequency
- Claims severity and amounts
- Claims processing time
- Claim denial rates and reasons
- Actual vs. expected mortality
- Claims experience by product/vintage

---

## 8. Fraud Detection Domain

**Purpose:** Identify, investigate, and prevent fraudulent activity across the customer lifecycle.

**Key Subject Areas:**
- Fraud alerts and suspicious activity detection
- Application fraud (identity theft, misrepresentation)
- Claims fraud (staged deaths, beneficiary fraud)
- Premium fraud (payment fraud, chargebacks)
- Fraud investigation cases and outcomes
- Fraud rules and scoring models
- Third-party fraud data sources
- Fraud patterns and typologies
- Investigator assignments and workflow
- Fraud loss prevention and recovery

**Primary Analytics:**
- Fraud detection rates and false positives
- Fraud loss amounts and trends
- Fraud by type and channel
- Investigation cycle time
- Prevention effectiveness and ROI
- Fraud pattern identification

---

## 9. Geography/Location Domain

**Purpose:** Provide geographic context for market analysis, regulatory compliance, and territorial performance.

**Key Subject Areas:**
- Geographic hierarchies (address, ZIP, city, county, state, region)
- State regulatory jurisdictions
- Market territories and sales regions
- Territorial rating factors
- Licensing jurisdictions
- Geographic demographics and census data
- Competitive market presence
- Regional economic indicators
- Urban vs. rural classifications

**Primary Analytics:**
- Geographic distribution of policies and premiums
- Market penetration by territory
- Regional profitability and loss ratios
- State-level regulatory compliance
- Geographic expansion opportunities
- Territorial pricing adequacy

---

## 10. Financial Performance Domain

**Purpose:** Track financial results, profitability, and regulatory reserves.

**Key Subject Areas:**
- Premium income (earned and unearned)
- Policy reserves (statutory and GAAP)
- Claim reserves and liabilities
- Commissions and acquisition costs
- Operating expenses by category
- Investment income and assets
- Reinsurance ceded and assumed
- Profitability metrics (product, cohort, vintage)
- Embedded value and new business value
- Capital and surplus

**Primary Analytics:**
- Premium growth and trends
- Loss ratios and combined ratios
- Profitability by product/segment
- Expense ratios
- Return on equity (ROE)
- Embedded value growth

---

## 11. Customer Engagement Domain

**Purpose:** Track customer interactions, service delivery, and satisfaction across all touchpoints.

**Key Subject Areas:**
- **Service channels** (self-service portal, mobile app, phone/call center, email, chat, mail)
- Customer interactions and contact history
- Service requests and case management
- Issue resolution and escalations
- Channel-specific engagement metrics
- Digital engagement (logins, portal usage, mobile app sessions)
- Policy servicing transactions
- Customer communications and preferences
- Channel preferences and migration patterns
- Customer satisfaction scores (CSAT, NPS)
- Customer effort scores (CES)
- Complaint tracking and resolution
- Omnichannel customer journey tracking

**Primary Analytics:**
- Contact volume and trends by channel
- First-call resolution rates
- Average handling time by channel
- Customer satisfaction trends
- Digital adoption and migration rates
- Service cost per interaction by channel
- Channel preference by customer segment
- Cross-channel behavior patterns
- Self-service containment rates

---

## 12. Compliance & Regulatory Domain

**Purpose:** Ensure regulatory compliance and support audit requirements.

**Key Subject Areas:**
- Regulatory reporting requirements
- State insurance department filings
- NAIC (National Association of Insurance Commissioners) data
- Statutory accounting and reporting
- Licensing and appointments
- Solvency and capital requirements
- Privacy and data protection compliance (GDPR, CCPA)
- Anti-money laundering (AML) monitoring
- Audit trails and data lineage
- Policy documentation and records retention

**Primary Analytics:**
- Compliance metrics and KPIs
- Regulatory capital ratios
- Solvency monitoring
- Audit findings and remediation tracking
- Data quality and completeness

---

## 13. Time/Calendar Domain

**Purpose:** Provide temporal context for all analytics and reporting.

**Key Subject Areas:**
- Date dimension (day, week, month, quarter, year)
- Business calendars and reporting periods
- Fiscal vs. calendar periods
- Policy anniversaries and key dates
- Holiday calendars
- Vintage/cohort groupings
- Policy duration bands
- Age at issue and attained age bands

**Primary Analytics:**
- Time-series trend analysis
- Period-over-period comparisons
- Cohort and vintage analysis
- Seasonality patterns
- Policy duration analysis

---

## Data Model Principles

### Integration Points
- All domains should be connected through common keys (Customer ID, Policy ID, Product ID, etc.)
- Time domain should be referenced across all fact tables
- Party domain serves as the central hub for customer-related analysis
- **Channel is a cross-cutting dimension** used across Sales & Distribution, Customer Engagement, and Premium & Billing domains
- **Geography/Location** provides context across multiple domains for territorial analysis

### Analytics Support
The model should enable:
- Multi-dimensional analysis across products, channels, geography, and time
- Customer journey and lifecycle analysis (acquisition through expansion)
- **Omnichannel customer behavior analysis** across sales, service, and payment touchpoints
- **New business vs. customer expansion** performance tracking
- Cohort and vintage profiling
- Predictive modeling for lapse, claims, fraud, and customer behavior
- Financial forecasting and scenario planning
- **Channel attribution and migration analysis**

### Scalability Considerations
- Design for both current state and historical trending
- Support slowly changing dimensions (SCD Type 2) for key entities
- Enable granular transaction-level analysis while supporting aggregated reporting
- Plan for future data sources and third-party integrations
- Accommodate evolving channel strategies and new customer touchpoints

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Created top-level subject area framework |

---

*This document serves as the foundation for developing a comprehensive enterprise data model. Each subject area should be further refined with detailed entity definitions, attributes, and relationships based on specific business requirements.*
