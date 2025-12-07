# Financial Performance Domain
## Logical Data Model - Entity Definitions

**Domain:** Financial Performance  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Supporting & Analytical Domain

---

## Domain Overview

**Purpose:** Track financial metrics including reserves, revenue, expenses, profitability, and financial reporting requirements for insurance operations.

**Scope:** This domain encompasses actuarial reserves, premium revenue, operating expenses, commissions, profitability metrics, financial periods, and statutory/GAAP reporting structures.

**Business Value:**
- Financial performance monitoring
- Reserve adequacy assessment
- Profitability analysis by product and segment
- Expense management
- Regulatory financial reporting
- Investor and management reporting

---

## Entity Catalog

### POLICY_RESERVE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Reserve |
| **Entity Definition** | The actuarial liability held for a specific Policy, representing the present value of future benefits less future premiums. Reserves are calculated per actuarial standards. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Reserve Date + Reserve Basis |
| **Key Relationships** | - Calculated for Policy (Policy Domain)<br>- Has Reserve Type<br>- Calculated per Reserve Basis<br>- Contributes to Aggregate Reserve |
| **Assumptions** | - Reserves calculated monthly or quarterly<br>- Multiple reserve bases maintained (Statutory, GAAP, Tax) |
| **Open Questions** | - What reserve valuation frequency? |

---

### RESERVE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Reserve Type |
| **Entity Definition** | A classification of Policy Reserves based on the purpose or nature of the reserve liability. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Policy Reserve |
| **Assumptions** | - Types: BASIC_RESERVE, DEFICIENCY_RESERVE, ADDITIONAL_RESERVE, UNEARNED_PREMIUM, CLAIM_RESERVE |
| **Open Questions** | - None at this time |

---

### RESERVE_BASIS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Reserve Basis |
| **Entity Definition** | The accounting or reporting framework under which reserves are calculated, determining the methodology, assumptions, and disclosure requirements. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Basis Code |
| **Key Relationships** | - Applied to Reserve calculations<br>- Determines valuation methodology |
| **Assumptions** | - Bases: STATUTORY, GAAP, TAX, ECONOMIC |
| **Open Questions** | - None at this time |

---

### AGGREGATE_RESERVE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Aggregate Reserve |
| **Entity Definition** | The total reserve liability for a defined grouping (product, line, company) as of a valuation date, representing the sum of individual Policy Reserves plus any additional reserves. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Aggregation Level + Reserve Date + Reserve Basis |
| **Key Relationships** | - Aggregates Policy Reserves<br>- Grouped by Product, Line, etc.<br>- Reported per Reserve Basis |
| **Assumptions** | - Aggregations calculated from policy-level<br>- Reconciles to financial statements |
| **Open Questions** | - None at this time |

---

### PREMIUM_REVENUE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Premium Revenue |
| **Entity Definition** | The income recognized from premium payments, representing the primary revenue source for insurance operations. Premium revenue is recognized over the coverage period. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Revenue Identifier |
| **Key Relationships** | - Derived from Premium Payments (Premium & Billing Domain)<br>- Earned over Coverage period<br>- Reported per Accounting Basis |
| **Assumptions** | - Written vs. earned premium tracked<br>- Revenue recognition per accounting standards |
| **Open Questions** | - What revenue recognition methodology? |

---

### EARNED_PREMIUM

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Earned Premium |
| **Entity Definition** | The portion of premium revenue that has been recognized as earned for the coverage period that has elapsed. Earned premium is the basis for income statement reporting. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Period + Accounting Basis |
| **Key Relationships** | - Calculated from Premium Revenue<br>- For specific Policy and Period<br>- Basis for revenue reporting |
| **Assumptions** | - Earned pro-rata over coverage period<br>- Unearned portion is liability |
| **Open Questions** | - None at this time |

---

### WRITTEN_PREMIUM

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Written Premium |
| **Entity Definition** | The total premium for policies issued or renewed during a period, representing new premium obligations regardless of when coverage or payment occurs. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Issue/Renewal Date |
| **Key Relationships** | - Recorded at policy issue/renewal<br>- Contributes to Written Premium volume |
| **Assumptions** | - Written at policy effective date<br>- Includes full policy term premium |
| **Open Questions** | - None at this time |

---

### INVESTMENT_INCOME

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Investment Income |
| **Entity Definition** | Revenue generated from the investment of insurance company assets, including interest, dividends, and realized gains. Investment income supplements premium revenue. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Income Identifier |
| **Key Relationships** | - Derived from Investment Portfolio<br>- Allocated to Lines of Business<br>- Reported per Accounting Basis |
| **Assumptions** | - Income allocated to lines<br>- Realized vs. unrealized distinguished |
| **Open Questions** | - How is investment income allocated to lines? |

---

### OPERATING_EXPENSE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Operating Expense |
| **Entity Definition** | Costs incurred in the operation of the insurance business, including acquisition expenses, maintenance expenses, and overhead. Expenses are allocated to products and periods. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Expense Identifier |
| **Key Relationships** | - Has Expense Type<br>- Allocated to Products/Lines<br>- Recorded in Financial Period |
| **Assumptions** | - Expenses allocated using defined methods<br>- Fixed vs. variable categorization |
| **Open Questions** | - What expense allocation methodology? |

---

### EXPENSE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Expense Type |
| **Entity Definition** | A classification of Operating Expenses based on the nature of the cost. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Operating Expense |
| **Assumptions** | - Types: ACQUISITION, MAINTENANCE, OVERHEAD, CLAIMS, INVESTMENT, TAXES |
| **Open Questions** | - None at this time |

---

### COMMISSION_EXPENSE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Commission Expense |
| **Entity Definition** | Compensation paid to distribution partners for sales, representing a significant acquisition cost. Commission expense includes first-year and renewal commissions. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Commission Identifier |
| **Key Relationships** | - Paid for Policy sale/renewal<br>- Paid to Partner (Sales & Distribution Domain)<br>- Per Commission Structure (Product Domain)<br>- Is an Acquisition Expense |
| **Assumptions** | - First-year vs. renewal tracked<br>- Subject to chargeback for early lapse |
| **Open Questions** | - None at this time |

---

### ACQUISITION_COST

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Acquisition Cost |
| **Entity Definition** | The total cost to acquire a new policy, including commissions, underwriting expenses, marketing costs, and other direct acquisition expenses. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number |
| **Key Relationships** | - Calculated for Policy<br>- Includes Commission, Marketing, UW costs<br>- May be deferred (DAC) |
| **Assumptions** | - Costs identified by policy<br>- DAC methodology defined |
| **Open Questions** | - What costs included in DAC? |

---

### DEFERRED_ACQUISITION_COST

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Deferred Acquisition Cost (DAC) |
| **Entity Definition** | Acquisition costs that are capitalized and amortized over the expected life of the policy rather than expensed immediately. DAC is an asset on the balance sheet. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Period |
| **Key Relationships** | - Derived from Acquisition Cost<br>- Amortized over policy life<br>- Tested for recoverability |
| **Assumptions** | - GAAP-specific treatment<br>- Amortization method defined |
| **Open Questions** | - What DAC amortization method? |

---

### BENEFIT_PAYMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Benefit Payment |
| **Entity Definition** | An outflow of funds for policy benefits, including death benefits, surrenders, and other policy payments. Benefit payments are a primary expense for life insurance. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Payment Identifier |
| **Key Relationships** | - Made for Claim (Claims Domain) or Surrender (Policy Domain)<br>- Has Benefit Type<br>- Reduces Reserve |
| **Assumptions** | - Tracks all benefit outflows<br>- Reconciles to claims and surrender transactions |
| **Open Questions** | - None at this time |

---

### BENEFIT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Benefit Type |
| **Entity Definition** | A classification of Benefit Payments based on the nature of the benefit paid. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Benefit Payment |
| **Assumptions** | - Types: DEATH_BENEFIT, SURRENDER_VALUE, LOAN_DISBURSEMENT, MATURITY_BENEFIT, DIVIDEND |
| **Open Questions** | - None at this time |

---

### PROFITABILITY_METRIC

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Profitability Metric |
| **Entity Definition** | A calculated measure of financial performance for a defined segment (product, cohort, channel), such as loss ratio, expense ratio, or profit margin. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Metric Identifier |
| **Key Relationships** | - Calculated for Segment<br>- Has Metric Type<br>- Calculated for Period |
| **Assumptions** | - Standard metrics defined<br>- Calculated at various levels |
| **Open Questions** | - What profitability metrics will be tracked? |

---

### PROFITABILITY_METRIC_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Profitability Metric Type |
| **Entity Definition** | A classification of Profitability Metrics based on what aspect of profitability is being measured. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Profitability Metric |
| **Assumptions** | - Types: LOSS_RATIO, EXPENSE_RATIO, COMBINED_RATIO, PROFIT_MARGIN, ROE, IRR, EMBEDDED_VALUE |
| **Open Questions** | - None at this time |

---

### LOSS_RATIO

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Loss Ratio |
| **Entity Definition** | A profitability metric calculated as benefits paid plus reserve changes divided by earned premium. Loss ratio measures the portion of premium consumed by claims. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Segment + Period |
| **Key Relationships** | - Calculated from Benefits and Premium<br>- Key performance indicator<br>- Is a Profitability Metric subtype |
| **Assumptions** | - Calculated monthly/quarterly/annually<br>- Tracked by product and cohort |
| **Open Questions** | - None at this time |

---

### EXPENSE_RATIO

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Expense Ratio |
| **Entity Definition** | A profitability metric calculated as operating expenses divided by earned or written premium. Expense ratio measures operational efficiency. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Segment + Period |
| **Key Relationships** | - Calculated from Expenses and Premium<br>- Key performance indicator<br>- Is a Profitability Metric subtype |
| **Assumptions** | - Calculated at various levels<br>- Acquisition vs. maintenance separated |
| **Open Questions** | - None at this time |

---

### FINANCIAL_PERIOD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Financial Period |
| **Entity Definition** | A defined period for financial reporting and analysis, aligned with the company's fiscal calendar and regulatory reporting cycles. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Period Identifier |
| **Key Relationships** | - Links to Time dimension (Time/Calendar Domain)<br>- Used for all financial reporting |
| **Assumptions** | - Periods align with fiscal year<br>- Monthly, quarterly, annual periods |
| **Open Questions** | - None at this time |

---

### ACCOUNTING_BASIS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Accounting Basis |
| **Entity Definition** | The accounting framework under which financial statements are prepared, determining recognition, measurement, and disclosure rules. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Basis Code |
| **Key Relationships** | - Applied to all financial entities<br>- Determines reporting treatment |
| **Assumptions** | - Bases: STATUTORY, GAAP, TAX, IFRS |
| **Open Questions** | - Will IFRS reporting be needed? |

---

### GENERAL_LEDGER_ACCOUNT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | General Ledger Account |
| **Entity Definition** | A chart of accounts entry used for recording financial transactions and producing financial statements. GL accounts provide the structure for financial reporting. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Account Number |
| **Key Relationships** | - Receives transaction postings<br>- Has Account Type<br>- Mapped to financial statement lines |
| **Assumptions** | - Standard chart of accounts defined<br>- Accounts mapped to multiple bases |
| **Open Questions** | - None at this time |

---

### GL_ACCOUNT_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | GL Account Type |
| **Entity Definition** | A classification of General Ledger Accounts based on their nature in financial reporting. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies General Ledger Account |
| **Assumptions** | - Types: ASSET, LIABILITY, EQUITY, REVENUE, EXPENSE |
| **Open Questions** | - None at this time |

---

### FINANCIAL_TRANSACTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Financial Transaction |
| **Entity Definition** | A recorded financial event that affects the general ledger, including premium receipts, benefit payments, expense accruals, and reserve changes. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Transaction Identifier |
| **Key Relationships** | - Posts to General Ledger Accounts<br>- Has Transaction Type<br>- Occurs in Financial Period |
| **Assumptions** | - All transactions posted to GL<br>- Double-entry accounting maintained |
| **Open Questions** | - None at this time |

---

### FINANCIAL_TRANSACTION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Financial Transaction Type |
| **Entity Definition** | A classification of Financial Transactions based on the nature of the financial event. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Financial Transaction |
| **Assumptions** | - Types: PREMIUM_RECEIPT, BENEFIT_PAYMENT, EXPENSE_ACCRUAL, RESERVE_CHANGE, COMMISSION_PAYMENT |
| **Open Questions** | - None at this time |

---

### BUDGET

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Budget |
| **Entity Definition** | A financial plan establishing expected revenue, expenses, and other financial metrics for a future period. Budgets enable variance analysis and performance tracking. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Budget Identifier |
| **Key Relationships** | - For Financial Period<br>- By Segment/Line/Product<br>- Compared to Actuals |
| **Assumptions** | - Annual budget with quarterly detail<br>- Budget by cost center and product |
| **Open Questions** | - What budgeting methodology? |

---

### BUDGET_LINE_ITEM

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Budget Line Item |
| **Entity Definition** | A specific line item in a Budget, representing expected values for a particular GL account, segment, or metric. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Budget Identifier + Line Identifier |
| **Key Relationships** | - Belongs to Budget<br>- Maps to GL Account or Metric |
| **Assumptions** | - Line items at appropriate granularity<br>- Support variance analysis |
| **Open Questions** | - None at this time |

---

### VARIANCE_ANALYSIS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Variance Analysis |
| **Entity Definition** | A comparison of actual financial results to budgeted or expected amounts, with calculation of variances and variance explanations. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Analysis Identifier |
| **Key Relationships** | - Compares Actual to Budget<br>- For Financial Period<br>- Produces Variance amounts |
| **Assumptions** | - Variance calculated monthly<br>- Materiality thresholds defined |
| **Open Questions** | - None at this time |

---

### EXPERIENCE_STUDY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Experience Study |
| **Entity Definition** | An actuarial analysis comparing actual mortality, lapse, or other experience to expected assumptions. Experience studies inform pricing and reserving. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Study Identifier |
| **Key Relationships** | - Analyzes Policy experience<br>- By Product, Cohort, Period<br>- Has Study Type |
| **Assumptions** | - Studies conducted periodically<br>- Results inform assumption updates |
| **Open Questions** | - What study frequency? |

---

### EXPERIENCE_STUDY_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Experience Study Type |
| **Entity Definition** | A classification of Experience Studies based on the type of experience being analyzed. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Experience Study |
| **Assumptions** | - Types: MORTALITY, LAPSE_PERSISTENCY, EXPENSE, MORBIDITY |
| **Open Questions** | - None at this time |

---

### ACTUAL_TO_EXPECTED

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Actual to Expected |
| **Entity Definition** | A ratio comparing actual experience to expected experience based on assumptions, used to assess assumption accuracy and emerging experience. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | A/E Calculation Identifier |
| **Key Relationships** | - Calculated in Experience Study<br>- For specific assumption (mortality, lapse)<br>- By Segment and Period |
| **Assumptions** | - A/E ratios monitored regularly<br>- Triggers for assumption reviews |
| **Open Questions** | - None at this time |

---

## Conceptual Entity-Relationship Diagram

```
┌─────────────────┐                             ┌─────────────────┐
│     POLICY      │────────────────────────────►│  POLICY_RESERVE │
│ (Policy Domain) │                             │                 │
└─────────────────┘                             └────────┬────────┘
                                                         │
                          ┌──────────────────────────────┼──────────────────┐
                          │                              │                  │
                          ▼                              ▼                  ▼
                 ┌─────────────────┐          ┌─────────────────┐  ┌─────────────────┐
                 │  RESERVE_TYPE   │          │  RESERVE_BASIS  │  │AGGREGATE_RESERVE│
                 │                 │          │                 │  │                 │
                 └─────────────────┘          └─────────────────┘  └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│ PREMIUM_REVENUE │────────►│ EARNED_PREMIUM  │         │ WRITTEN_PREMIUM │
│                 │         │                 │         │                 │
└─────────────────┘         └─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│ INVESTMENT      │         │ OPERATING       │────────►┌─────────────────┐
│   INCOME        │         │   EXPENSE       │         │  EXPENSE_TYPE   │
└─────────────────┘         └─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│   COMMISSION    │────────►│  ACQUISITION    │────────►│    DEFERRED     │
│    EXPENSE      │  part   │     COST        │  may be │ACQUISITION COST │
└─────────────────┘  of     └─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│ BENEFIT_PAYMENT │────────►│  BENEFIT_TYPE   │
│                 │         │                 │
└─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│ PROFITABILITY   │────────►│ PROFITABILITY   │
│    METRIC       │         │  METRIC_TYPE    │
└────────┬────────┘         └─────────────────┘
         │
         ├──────────────────┬──────────────────┐
         ▼                  ▼                  ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│   LOSS_RATIO    │ │  EXPENSE_RATIO  │ │   (other metrics)│
│                 │ │                 │ │                 │
└─────────────────┘ └─────────────────┘ └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│FINANCIAL_PERIOD │         │ ACCOUNTING_     │
│                 │         │    BASIS        │
└─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│  GENERAL_LEDGER │────────►│ GL_ACCOUNT_TYPE │         │   FINANCIAL     │
│    ACCOUNT      │         │                 │         │  TRANSACTION    │
└─────────────────┘         └─────────────────┘         └────────┬────────┘
                                                                 │
                                                                 ▼
                                                        ┌─────────────────┐
                                                        │   FINANCIAL     │
                                                        │TRANSACTION_TYPE │
                                                        └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│     BUDGET      │────────►│BUDGET_LINE_ITEM │         │VARIANCE_ANALYSIS│
│                 │         │                 │         │                 │
└─────────────────┘         └─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│EXPERIENCE_STUDY │────────►│EXPERIENCE_STUDY │         │ACTUAL_TO        │
│                 │         │     TYPE        │         │   EXPECTED      │
└─────────────────┘         └─────────────────┘         └─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Policy Reserve | Policy | Policy | Reserve calculated per policy |
| Premium Revenue | Premium & Billing | Payment Transaction | Revenue from premium payments |
| Commission Expense | Sales & Distribution | Partner | Commission paid to partners |
| Commission Expense | Product | Commission Structure | Rates from product |
| Benefit Payment | Claims | Claim Payout | Benefits from claims |
| Benefit Payment | Policy | Surrender | Benefits from surrenders |
| Financial Period | Time/Calendar | Fiscal Period | Aligns with fiscal calendar |
| Experience Study | Policy | Policy | Studies analyze policy experience |
| Operating Expense | Customer Engagement | Service Cost | Service delivery costs |
| Budget | Time/Calendar | Calendar Year | Budget for fiscal year |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Policy Reserve | Transactional | Policy-level actuarial liability |
| Reserve Type | Reference | Reserve classification |
| Reserve Basis | Reference | Accounting framework |
| Aggregate Reserve | Transactional | Rolled-up reserve liability |
| Premium Revenue | Transactional | Premium income |
| Earned Premium | Transactional | Recognized premium revenue |
| Written Premium | Transactional | Premium at policy inception |
| Investment Income | Transactional | Investment earnings |
| Operating Expense | Transactional | Business operating costs |
| Expense Type | Reference | Expense classification |
| Commission Expense | Transactional | Distribution compensation |
| Acquisition Cost | Transactional | Policy acquisition costs |
| Deferred Acquisition Cost | Transactional | Capitalized acquisition costs |
| Benefit Payment | Transactional | Policy benefit outflows |
| Benefit Type | Reference | Benefit classification |
| Profitability Metric | Transactional | Performance measure |
| Profitability Metric Type | Reference | Metric classification |
| Loss Ratio | Transactional | Claims to premium ratio |
| Expense Ratio | Transactional | Expenses to premium ratio |
| Financial Period | Reference | Reporting period |
| Accounting Basis | Reference | Accounting framework |
| General Ledger Account | Reference | Chart of accounts entry |
| GL Account Type | Reference | Account classification |
| Financial Transaction | Transactional | GL posting |
| Financial Transaction Type | Reference | Transaction classification |
| Budget | Master | Financial plan |
| Budget Line Item | Master | Budget detail |
| Variance Analysis | Transactional | Actual vs budget |
| Experience Study | Transactional | Actuarial analysis |
| Experience Study Type | Reference | Study classification |
| Actual to Expected | Transactional | Experience ratio |

**Total Entities: 31**

---

## Assumptions Log

1. Reserves calculated at policy level
2. Multiple accounting bases maintained (Statutory, GAAP, Tax)
3. Revenue recognized per accounting standards
4. Expenses allocated to products/lines
5. Profitability metrics calculated at various levels
6. GL structure supports multiple reporting bases
7. Budget process is annual with quarterly detail
8. Experience studies conducted periodically
9. A/E ratios monitored for assumption adequacy

---

## Open Questions

1. What reserve valuation frequency?
2. What revenue recognition methodology (GAAP vs. LDTI)?
3. How is investment income allocated to lines?
4. What expense allocation methodology?
5. What costs are included in DAC?
6. What DAC amortization method?
7. What profitability metrics will be tracked?
8. Will IFRS reporting be needed?
9. What budgeting methodology?
10. What experience study frequency?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
