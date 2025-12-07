# Time/Calendar Domain
## Logical Data Model - Entity Definitions

**Domain:** Time/Calendar  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Core Foundation Domain

---

## Domain Overview

**Purpose:** Provide temporal context for all analytics and reporting, enabling time-based analysis, trending, cohort comparisons, and period-based aggregations.

**Scope:** This domain encompasses calendar dates, business periods, fiscal calendars, policy duration bands, age bands, and time-related dimensional entities that support analytics across all other domains.

**Business Value:**
- Standardized time dimensions for consistent reporting
- Enable period-over-period comparisons
- Support cohort and vintage analysis
- Facilitate seasonality and trend analysis
- Foundation for all time-series analytics

---

## Entity Catalog

### DATE_DIMENSION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Date Dimension |
| **Entity Definition** | A comprehensive representation of a calendar date with multiple attributes for analysis, including day, week, month, quarter, year, and various calendar flags. This is the primary date reference for all analytics. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Date Key (typically YYYYMMDD format) |
| **Key Relationships** | - Referenced by all transactional and fact tables<br>- Links to Fiscal Period<br>- Links to Holiday Calendar |
| **Assumptions** | - Pre-populated for range needed (e.g., 1900-2100)<br>- Includes both calendar and fiscal attributes |
| **Open Questions** | - What date range should be pre-populated? |

---

### CALENDAR_MONTH

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Calendar Month |
| **Entity Definition** | A representation of a calendar month, providing month-level attributes for reporting and aggregation. Enables month-over-month and year-over-year analysis. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Year-Month Key (YYYYMM format) |
| **Key Relationships** | - Contains Date Dimensions<br>- Belongs to Calendar Quarter<br>- Used for monthly reporting |
| **Assumptions** | - Standard Gregorian calendar months |
| **Open Questions** | - None at this time |

---

### CALENDAR_QUARTER

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Calendar Quarter |
| **Entity Definition** | A representation of a calendar quarter (Q1-Q4), providing quarter-level attributes for reporting. Enables quarterly business reviews and period comparisons. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Year-Quarter Key (YYYYQ format) |
| **Key Relationships** | - Contains Calendar Months<br>- Belongs to Calendar Year<br>- Used for quarterly reporting |
| **Assumptions** | - Q1: Jan-Mar, Q2: Apr-Jun, Q3: Jul-Sep, Q4: Oct-Dec |
| **Open Questions** | - None at this time |

---

### CALENDAR_YEAR

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Calendar Year |
| **Entity Definition** | A representation of a calendar year, providing year-level attributes for annual reporting, budgeting, and long-term trend analysis. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Year Key (YYYY format) |
| **Key Relationships** | - Contains Calendar Quarters<br>- Used for annual reporting and comparisons |
| **Assumptions** | - Standard Gregorian calendar year |
| **Open Questions** | - None at this time |

---

### FISCAL_PERIOD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Fiscal Period |
| **Entity Definition** | A financial reporting period that may differ from calendar periods based on the company's fiscal year definition. Supports financial reporting requirements. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Fiscal Period Key |
| **Key Relationships** | - Maps to Calendar dates<br>- Used for Financial reporting (Financial Domain)<br>- Aligns with statutory reporting |
| **Assumptions** | - Fiscal year may differ from calendar year<br>- Fiscal periods align with financial close cycles |
| **Open Questions** | - What is the company's fiscal year end? |

---

### FISCAL_QUARTER

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Fiscal Quarter |
| **Entity Definition** | A fiscal quarter within the company's fiscal year, used for financial reporting and quarterly business analysis. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Fiscal Year + Fiscal Quarter Number |
| **Key Relationships** | - Contains Fiscal Periods<br>- Belongs to Fiscal Year |
| **Assumptions** | - Four quarters per fiscal year |
| **Open Questions** | - None at this time |

---

### FISCAL_YEAR

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Fiscal Year |
| **Entity Definition** | The company's fiscal year used for financial reporting, which may start on a date other than January 1. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Fiscal Year Key |
| **Key Relationships** | - Contains Fiscal Quarters<br>- Used for annual financial reporting |
| **Assumptions** | - Fiscal year is 12 months<br>- Year designation follows company convention |
| **Open Questions** | - Is fiscal year = calendar year, or different? |

---

### HOLIDAY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Holiday |
| **Entity Definition** | A day designated as a holiday that may affect business operations, customer behavior, or service level expectations. Holidays support operational planning and seasonality analysis. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Date Key + Holiday Type Code |
| **Key Relationships** | - Associated with Date Dimension<br>- May vary by Region/State (Geography Domain) |
| **Assumptions** | - Federal, state, and company holidays tracked<br>- Multiple holidays may occur on same date |
| **Open Questions** | - Which holidays affect business operations? |

---

### HOLIDAY_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Holiday Type |
| **Entity Definition** | A classification of holidays based on their nature and impact on business operations. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Holiday Type Code |
| **Key Relationships** | - Classifies Holiday |
| **Assumptions** | - Types: FEDERAL, STATE, COMPANY, RELIGIOUS, OBSERVANCE |
| **Open Questions** | - None at this time |

---

### WEEK

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Week |
| **Entity Definition** | A representation of a calendar week for weekly reporting and analysis. Week boundaries are defined according to a standard convention (ISO or US). |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Year-Week Key |
| **Key Relationships** | - Contains Date Dimensions<br>- Used for weekly operational reporting |
| **Assumptions** | - Week starts on Sunday (US) or Monday (ISO) - to be determined |
| **Open Questions** | - Which week convention should be used (ISO vs US)? |

---

### REPORTING_PERIOD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Reporting Period |
| **Entity Definition** | A defined period used for business reporting, which may be standard (monthly, quarterly) or custom (e.g., 4-4-5 retail calendar). Provides flexibility for different reporting needs. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Reporting Period Key |
| **Key Relationships** | - Maps to Date Dimensions<br>- Used for Business reporting<br>- May align with Regulatory reporting (Compliance Domain) |
| **Assumptions** | - Multiple reporting period types may coexist<br>- Periods are non-overlapping within a type |
| **Open Questions** | - What non-standard reporting periods are needed? |

---

### POLICY_DURATION_BAND

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Duration Band |
| **Entity Definition** | A grouping of policy durations (time since policy issue) into bands for analysis. Duration bands enable cohort analysis of policy performance over time. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Duration Band Code |
| **Key Relationships** | - Used to classify Policies (Policy Domain)<br>- Used for Persistency analysis |
| **Assumptions** | - Common bands: 0-1 year, 1-2 years, 2-3 years, 3-5 years, 5-10 years, 10+ years<br>- Bands are mutually exclusive |
| **Open Questions** | - What duration bands are needed for analysis? |

---

### AGE_BAND

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Age Band |
| **Entity Definition** | A grouping of ages into bands for demographic analysis. Age bands enable analysis by age groups for marketing, underwriting, and experience studies. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Age Band Code |
| **Key Relationships** | - Used to classify Individuals (Party Domain)<br>- Used for Underwriting analysis (Underwriting Domain)<br>- Used for Claims analysis (Claims Domain) |
| **Assumptions** | - Different age bands may be used for different purposes (marketing vs. actuarial)<br>- Standard actuarial age bands defined |
| **Open Questions** | - What age band definitions are needed? |

---

### AGE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Age Type |
| **Entity Definition** | A classification of how age is calculated or used - distinguishing between issue age (age at policy purchase) and attained age (current age). |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Age Type Code |
| **Key Relationships** | - Qualifies age-related calculations |
| **Assumptions** | - Types: ISSUE_AGE, ATTAINED_AGE, NEAREST_BIRTHDAY, LAST_BIRTHDAY |
| **Open Questions** | - Which age calculation method is used for rating? |

---

### VINTAGE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Vintage |
| **Entity Definition** | A grouping of policies or customers by the time period they were acquired, enabling cohort-based performance analysis. Vintages are typically year or year-quarter based. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Vintage Key |
| **Key Relationships** | - Used to classify Policies (Policy Domain)<br>- Used to classify Customers (Party Domain)<br>- Used for Cohort analysis |
| **Assumptions** | - Vintage typically based on policy issue date<br>- Annual or quarterly vintage granularity |
| **Open Questions** | - Should vintage be annual or quarterly? |

---

### POLICY_YEAR

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Policy Year |
| **Entity Definition** | The year of a policy's life, measured from the issue date. Policy Year 1 is the first year after issue, Policy Year 2 is the second year, etc. Used for experience and persistency analysis. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Policy Year Number |
| **Key Relationships** | - Applied to Policy analysis<br>- Used for Lapse/Persistency studies<br>- Used for Commission calculations (Financial Domain) |
| **Assumptions** | - Policy Year 1 starts at policy effective date<br>- Used for surrender charge and commission schedules |
| **Open Questions** | - None at this time |

---

### BUSINESS_DAY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Business Day |
| **Entity Definition** | A day on which the business is open for operations, excluding weekends and holidays. Used for SLA calculations and operational metrics. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Date Key |
| **Key Relationships** | - Subset of Date Dimension<br>- Excludes Holidays<br>- Used for Service Level tracking (Customer Engagement Domain) |
| **Assumptions** | - Monday-Friday excluding company holidays<br>- May vary by department or function |
| **Open Questions** | - Do any operations run on weekends? |

---

### TIME_OF_DAY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Time of Day |
| **Entity Definition** | A classification of times within a day for analysis of intraday patterns, such as call center volume by hour or web traffic by time period. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Time Key |
| **Key Relationships** | - Used with Date Dimension for timestamp analysis<br>- Used for Operational analysis (Customer Engagement Domain) |
| **Assumptions** | - Granularity: hour or 15-minute intervals<br>- Based on company timezone |
| **Open Questions** | - What timezone is standard for reporting? |

---

### EFFECTIVE_DATE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Effective Date Type |
| **Entity Definition** | A classification of the types of effective dates used in insurance, distinguishing between policy effective date, coverage effective date, rider effective date, etc. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Date Type Code |
| **Key Relationships** | - Qualifies dates across all domains |
| **Assumptions** | - Types: POLICY_EFFECTIVE, COVERAGE_EFFECTIVE, RIDER_EFFECTIVE, MODIFICATION_EFFECTIVE, TERMINATION_EFFECTIVE |
| **Open Questions** | - None at this time |

---

### CONTESTABILITY_PERIOD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Contestability Period |
| **Entity Definition** | The initial period after policy issue (typically 2 years) during which the insurer can contest claims based on material misrepresentation. Critical for claims processing. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Period Definition |
| **Key Relationships** | - Applied to Policies (Policy Domain)<br>- Used for Claims processing (Claims Domain)<br>- May vary by State (Geography Domain) |
| **Assumptions** | - Standard period is 2 years<br>- Suicide exclusion period may differ |
| **Open Questions** | - What states have different contestability periods? |

---

### SUICIDE_EXCLUSION_PERIOD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Suicide Exclusion Period |
| **Entity Definition** | The period after policy issue during which death by suicide is excluded from coverage or results in return of premium only. Used for claims adjudication. |
| **Entity Type** | Dimensional |
| **Primary Identifier(s)** | Period Definition |
| **Key Relationships** | - Applied to Policies (Policy Domain)<br>- Used for Claims processing (Claims Domain)<br>- May vary by State (Geography Domain) |
| **Assumptions** | - Standard period is 1-2 years<br>- May differ from contestability period |
| **Open Questions** | - What are state variations for suicide exclusion? |

---

## Conceptual Entity-Relationship Diagram

```
                              ┌─────────────────┐
                              │  CALENDAR_YEAR  │
                              └────────┬────────┘
                                       │ contains
                                       ▼
                              ┌─────────────────┐
                              │CALENDAR_QUARTER │
                              └────────┬────────┘
                                       │ contains
                                       ▼
                              ┌─────────────────┐
                              │ CALENDAR_MONTH  │
                              └────────┬────────┘
                                       │ contains
                                       ▼
┌─────────────────┐           ┌─────────────────┐           ┌─────────────────┐
│      WEEK       │◄──────────│ DATE_DIMENSION  │──────────►│    HOLIDAY      │
│                 │ contains  │                 │  may be   │                 │
└─────────────────┘           └────────┬────────┘           └────────┬────────┘
                                       │                             │
                                       │                             ▼
                                       │                    ┌─────────────────┐
                                       │                    │  HOLIDAY_TYPE   │
                                       │                    └─────────────────┘
                                       │
          ┌────────────────────────────┼────────────────────────────┐
          │                            │                            │
          ▼                            ▼                            ▼
┌─────────────────┐          ┌─────────────────┐          ┌─────────────────┐
│  BUSINESS_DAY   │          │ FISCAL_PERIOD   │          │ REPORTING_PERIOD│
│                 │          │                 │          │                 │
└─────────────────┘          └────────┬────────┘          └─────────────────┘
                                      │ belongs to
                                      ▼
                             ┌─────────────────┐
                             │ FISCAL_QUARTER  │
                             └────────┬────────┘
                                      │ belongs to
                                      ▼
                             ┌─────────────────┐
                             │   FISCAL_YEAR   │
                             └─────────────────┘

┌─────────────────┐          ┌─────────────────┐          ┌─────────────────┐
│  TIME_OF_DAY    │          │   POLICY_YEAR   │          │    VINTAGE      │
│                 │          │                 │          │                 │
└─────────────────┘          └─────────────────┘          └─────────────────┘

┌─────────────────┐          ┌─────────────────┐          ┌─────────────────┐
│    AGE_BAND     │◄─────────│    AGE_TYPE     │          │POLICY_DURATION  │
│                 │ qualifies│                 │          │     BAND        │
└─────────────────┘          └─────────────────┘          └─────────────────┘

┌─────────────────┐          ┌─────────────────┐
│ EFFECTIVE_DATE  │          │CONTESTABILITY   │
│     TYPE        │          │    PERIOD       │
└─────────────────┘          └─────────────────┘

┌─────────────────┐
│SUICIDE_EXCLUSION│
│    PERIOD       │
└─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Date Dimension | All Domains | All Transactions | Primary date reference for all events |
| Fiscal Period | Financial Performance | Financial Statements | Aligns with financial reporting |
| Policy Duration Band | Policy | Policy | Classifies policy age for analysis |
| Age Band | Party/Customer | Individual | Classifies individuals for demographics |
| Age Band | Underwriting | Risk Assessment | Used for mortality analysis |
| Age Band | Claims | Claim | Used for claims experience |
| Vintage | Policy | Policy | Groups policies by acquisition period |
| Vintage | Party/Customer | Customer | Groups customers by acquisition period |
| Policy Year | Policy | Policy | Policy life year for experience |
| Policy Year | Financial Performance | Commission | Commission schedules by policy year |
| Business Day | Customer Engagement | Service Request | SLA calculations |
| Contestability Period | Claims | Claim | Determines contestability status |
| Suicide Exclusion Period | Claims | Claim | Determines exclusion status |
| Holiday | Customer Engagement | Service Level | Affects SLA expectations |
| Reporting Period | Compliance | Regulatory Filing | Reporting deadlines |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Date Dimension | Dimensional | Primary calendar date reference |
| Calendar Month | Dimensional | Month-level aggregation |
| Calendar Quarter | Dimensional | Quarter-level aggregation |
| Calendar Year | Dimensional | Year-level aggregation |
| Fiscal Period | Dimensional | Financial reporting period |
| Fiscal Quarter | Dimensional | Fiscal quarter |
| Fiscal Year | Dimensional | Fiscal year |
| Holiday | Reference | Holiday calendar |
| Holiday Type | Reference | Holiday classification |
| Week | Dimensional | Weekly aggregation |
| Reporting Period | Dimensional | Custom reporting periods |
| Policy Duration Band | Dimensional | Policy age groupings |
| Age Band | Dimensional | Age range groupings |
| Age Type | Reference | Age calculation methods |
| Vintage | Dimensional | Acquisition cohort groupings |
| Policy Year | Dimensional | Year in policy life |
| Business Day | Dimensional | Non-holiday weekdays |
| Time of Day | Dimensional | Intraday time periods |
| Effective Date Type | Reference | Date type classification |
| Contestability Period | Dimensional | Claims contest window |
| Suicide Exclusion Period | Dimensional | Suicide exclusion window |

**Total Entities: 21**

---

## Assumptions Log

1. Date dimension is pre-populated for required historical and future range
2. Calendar follows standard Gregorian calendar
3. Fiscal year definition may differ from calendar year
4. Multiple age band sets may be defined for different uses
5. Duration bands are mutually exclusive and collectively exhaustive
6. Business days exclude weekends and designated holidays
7. Contestability period is typically 2 years from issue
8. Vintage granularity to be determined (annual or quarterly)

---

## Open Questions

1. What date range should be pre-populated in date dimension?
2. What is the company's fiscal year end?
3. Is fiscal year same as calendar year?
4. Which week convention should be used (ISO Monday start vs US Sunday start)?
5. What non-standard reporting periods are needed?
6. What duration band definitions are needed?
7. What age band definitions are needed (marketing, actuarial)?
8. Which age calculation method is used for rating (nearest birthday, last birthday)?
9. Should vintage be annual or quarterly?
10. What is the standard timezone for reporting?
11. Do any operations run on weekends?
12. What states have different contestability periods?
13. What are state variations for suicide exclusion periods?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
