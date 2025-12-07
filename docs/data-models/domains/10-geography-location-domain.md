# Geography/Location Domain
## Logical Data Model - Entity Definitions

**Domain:** Geography/Location  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Supporting & Analytical Domain

---

## Domain Overview

**Purpose:** Provide geographic context for market analysis, regulatory compliance, territorial rating, and performance reporting across all business operations.

**Scope:** This domain encompasses geographic hierarchies from address to region, regulatory jurisdictions, sales territories, market areas, and demographic data associated with geography.

**Business Value:**
- Geographic distribution analysis
- Market penetration measurement
- Territorial rating and pricing
- State regulatory compliance
- Regional performance analysis
- Expansion opportunity identification

---

## Entity Catalog

### GEOGRAPHIC_AREA

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Geographic Area |
| **Entity Definition** | A supertype entity representing any defined geographic unit, from individual addresses to broad regions. Geographic Area provides a common structure for all spatial hierarchies. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Area Identifier |
| **Key Relationships** | - Has Area Type<br>- May belong to Parent Geographic Area<br>- Contains Child Geographic Areas |
| **Assumptions** | - Areas form a strict hierarchy<br>- One standard hierarchy maintained |
| **Open Questions** | - Will multiple hierarchy views be needed? |

---

### AREA_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Area Type |
| **Entity Definition** | A classification of Geographic Areas based on their level in the geographic hierarchy. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Geographic Area |
| **Assumptions** | - Types: ADDRESS, ZIP_CODE, CITY, COUNTY, STATE, REGION, COUNTRY |
| **Open Questions** | - None at this time |

---

### COUNTRY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Country |
| **Entity Definition** | A sovereign nation that provides the highest level of the standard geographic hierarchy. For domestic insurance, primarily United States with potential for territories. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Country Code (ISO 3166-1) |
| **Key Relationships** | - Contains Regions<br>- Contains States<br>- Is a subtype of Geographic Area |
| **Assumptions** | - ISO standard codes used<br>- US territories included |
| **Open Questions** | - Will international coverage be offered? |

---

### REGION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Region |
| **Entity Definition** | A grouping of States into broader geographic regions for executive reporting and strategic analysis. Regions are company-defined groupings. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Region Code |
| **Key Relationships** | - Contains States<br>- Belongs to Country<br>- Is a subtype of Geographic Area |
| **Assumptions** | - Regions defined by company<br>- Examples: Northeast, Southeast, Midwest, Southwest, West |
| **Open Questions** | - What regional structure will be used? |

---

### STATE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | State |
| **Entity Definition** | A US state, territory, or district representing a primary regulatory and geographic unit. States are the key level for insurance regulatory compliance. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | State Code (USPS 2-letter) |
| **Key Relationships** | - Contains Counties<br>- Belongs to Region<br>- Is a subtype of Geographic Area<br>- Has Regulatory Jurisdiction<br>- Has State Demographics |
| **Assumptions** | - All 50 states plus DC and territories<br>- USPS codes used |
| **Open Questions** | - None at this time |

---

### COUNTY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | County |
| **Entity Definition** | A county, parish, or equivalent administrative subdivision within a State. Counties provide a useful level for demographic and market analysis. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | FIPS County Code |
| **Key Relationships** | - Contains Cities/ZIP Codes<br>- Belongs to State<br>- Is a subtype of Geographic Area<br>- Has County Demographics |
| **Assumptions** | - FIPS codes used for unique identification<br>- Includes independent cities |
| **Open Questions** | - None at this time |

---

### CITY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | City |
| **Entity Definition** | An incorporated municipality or recognized place name. Cities may span multiple counties and ZIP codes. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | City Identifier (State + City Name) |
| **Key Relationships** | - Contains ZIP Codes (many-to-many)<br>- Located in County/State<br>- Is a subtype of Geographic Area |
| **Assumptions** | - City boundaries may cross counties<br>- Unincorporated places included |
| **Open Questions** | - None at this time |

---

### ZIP_CODE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | ZIP Code |
| **Entity Definition** | A US Postal Service ZIP code representing a mail delivery area. ZIP codes are commonly used for address standardization and territorial rating. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | ZIP Code (5-digit) |
| **Key Relationships** | - Located in City/County/State<br>- Contains Addresses<br>- Is a subtype of Geographic Area<br>- Has ZIP Demographics |
| **Assumptions** | - 5-digit ZIP codes used<br>- ZIP+4 stored with address |
| **Open Questions** | - Will ZIP+4 level be needed for analysis? |

---

### ZIP_CODE_PLUS4

| Attribute | Value |
|-----------|-------|
| **Entity Name** | ZIP Code Plus4 |
| **Entity Definition** | An extended ZIP code with 4-digit suffix for more precise delivery location identification. Used for address standardization. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | ZIP+4 Code (9-digit) |
| **Key Relationships** | - Belongs to ZIP Code<br>- Associated with Addresses |
| **Assumptions** | - Maintained for address standardization<br>- Not primary analysis level |
| **Open Questions** | - None at this time |

---

### POSTAL_ADDRESS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Postal Address |
| **Entity Definition** | A standardized street address for mail delivery, linked to the geographic hierarchy. Addresses are validated and standardized against postal databases. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Address Identifier |
| **Key Relationships** | - Located in ZIP Code<br>- Associated with Party (Party Domain)<br>- Is a subtype of Geographic Area |
| **Assumptions** | - Addresses standardized via USPS database<br>- Both original and standardized versions stored |
| **Open Questions** | - What address validation service will be used? |

---

### REGULATORY_JURISDICTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Regulatory Jurisdiction |
| **Entity Definition** | A geographic area under the authority of a specific regulatory body, typically corresponding to a State for insurance regulation. Jurisdictions determine applicable laws and filing requirements. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Jurisdiction Code |
| **Key Relationships** | - Maps to State(s)<br>- Governs Product availability<br>- Determines Filing requirements (Compliance Domain) |
| **Assumptions** | - Primarily state-level for insurance<br>- Multi-state jurisdictions may exist for some requirements |
| **Open Questions** | - None at this time |

---

### LICENSING_JURISDICTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Licensing Jurisdiction |
| **Entity Definition** | A regulatory jurisdiction for insurance company licensing and agent/producer licensing. Each jurisdiction has specific licensing requirements and fees. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Jurisdiction Code |
| **Key Relationships** | - Maps to State<br>- Requires Company License<br>- Requires Agent License (if applicable) |
| **Assumptions** | - Primarily state-level<br>- DC and territories included |
| **Open Questions** | - None at this time |

---

### SALES_TERRITORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Sales Territory |
| **Entity Definition** | A defined geographic area used for sales management, performance tracking, and resource allocation. Territories may be aligned with states, regions, or custom definitions. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Territory Identifier |
| **Key Relationships** | - Contains Geographic Areas (ZIP, County, State)<br>- Assigned to Sales organization<br>- Used for Performance tracking |
| **Assumptions** | - Territories may not align with geographic hierarchy<br>- Multiple territory schemes may exist |
| **Open Questions** | - How are territories defined and managed? |

---

### TERRITORY_ASSIGNMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Territory Assignment |
| **Entity Definition** | The mapping of specific Geographic Areas (typically ZIP codes) to Sales Territories, enabling territory-based analysis and reporting. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Territory Identifier + Area Identifier |
| **Key Relationships** | - Links Territory to Geographic Areas |
| **Assumptions** | - Assignments are non-overlapping within a scheme<br>- Historical assignments tracked |
| **Open Questions** | - None at this time |

---

### MARKET_AREA

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Market Area |
| **Entity Definition** | A defined geographic market for strategic analysis, often based on metropolitan statistical areas (MSA) or designated market areas (DMA). |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Market Area Code |
| **Key Relationships** | - Contains Geographic Areas<br>- Has Market Demographics<br>- May overlap with other Market Areas |
| **Assumptions** | - Based on standard definitions (MSA, DMA)<br>- Used for market analysis |
| **Open Questions** | - Which market area definitions will be used? |

---

### METROPOLITAN_AREA

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Metropolitan Area |
| **Entity Definition** | A metropolitan statistical area (MSA) as defined by the US Office of Management and Budget, representing an urban core with surrounding counties. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | MSA Code |
| **Key Relationships** | - Contains Counties<br>- Is a type of Market Area<br>- Has MSA Demographics |
| **Assumptions** | - Standard OMB definitions used<br>- Updated periodically with census |
| **Open Questions** | - None at this time |

---

### RATING_TERRITORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Rating Territory |
| **Entity Definition** | A geographic area used for insurance rating purposes, where location-specific factors may affect premium rates. Not typically used for life insurance but may apply to some products. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Rating Territory Code |
| **Key Relationships** | - Contains Geographic Areas<br>- Applied to Product rating (Product Domain) |
| **Assumptions** | - Life insurance typically does not territory-rate<br>- May apply to certain rider coverages |
| **Open Questions** | - Will territorial rating be used for any products? |

---

### URBAN_RURAL_CLASSIFICATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Urban/Rural Classification |
| **Entity Definition** | A classification of geographic areas as urban, suburban, or rural based on population density and census definitions. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Classification Code |
| **Key Relationships** | - Applied to Geographic Areas<br>- Used for demographic analysis |
| **Assumptions** | - Census-based definitions<br>- Classifications: URBAN, SUBURBAN, RURAL |
| **Open Questions** | - None at this time |

---

### STATE_DEMOGRAPHIC

| Attribute | Value |
|-----------|-------|
| **Entity Name** | State Demographic |
| **Entity Definition** | Demographic and socioeconomic data associated with a State, including population, median income, and other census-based metrics. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | State Code + Data Year |
| **Key Relationships** | - Associated with State<br>- Sourced from Census/ACS data |
| **Assumptions** | - Updated annually or per census<br>- Key metrics defined |
| **Open Questions** | - What demographic metrics will be maintained? |

---

### ZIP_DEMOGRAPHIC

| Attribute | Value |
|-----------|-------|
| **Entity Name** | ZIP Demographic |
| **Entity Definition** | Demographic and socioeconomic data associated with a ZIP code, enabling micro-level market analysis and targeting. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | ZIP Code + Data Year |
| **Key Relationships** | - Associated with ZIP Code<br>- Sourced from Census/commercial data |
| **Assumptions** | - ZIP-level data from census or third-party<br>- May include enhanced data |
| **Open Questions** | - What third-party demographic sources will be used? |

---

### GEOCODE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Geocode |
| **Entity Definition** | The latitude and longitude coordinates for a geographic location, enabling spatial analysis and mapping. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Geocode Identifier |
| **Key Relationships** | - Associated with Postal Address or Geographic Area |
| **Assumptions** | - Coordinates from geocoding service<br>- Precision varies by level |
| **Open Questions** | - What geocoding service will be used? |

---

### TIME_ZONE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Time Zone |
| **Entity Definition** | The time zone applicable to a geographic area, used for scheduling, communication timing, and timestamp normalization. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Time Zone Code |
| **Key Relationships** | - Associated with Geographic Areas |
| **Assumptions** | - Standard IANA time zone identifiers<br>- Daylight saving time handled |
| **Open Questions** | - None at this time |

---

### DISTANCE_BAND

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Distance Band |
| **Entity Definition** | A classification of distances from a reference point (e.g., urban center), used for analysis of customer distribution patterns. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Band Code |
| **Key Relationships** | - Applied to locations relative to reference point |
| **Assumptions** | - Bands defined by distance ranges<br>- Reference points defined by use case |
| **Open Questions** | - What distance bands will be defined? |

---

## Conceptual Entity-Relationship Diagram

```
                                    ┌─────────────────┐
                                    │    COUNTRY      │
                                    │                 │
                                    └────────┬────────┘
                                             │ contains
                                             ▼
                                    ┌─────────────────┐
                                    │     REGION      │
                                    │                 │
                                    └────────┬────────┘
                                             │ contains
                                             ▼
                                    ┌─────────────────┐         ┌─────────────────┐
                                    │     STATE       │────────►│  REGULATORY     │
                                    │                 │         │  JURISDICTION   │
                                    └────────┬────────┘         └─────────────────┘
                                             │
                          ┌──────────────────┼──────────────────┐
                          │                  │                  │
                          ▼                  ▼                  ▼
                 ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
                 │     COUNTY      │ │ STATE_DEMOGR.   │ │   LICENSING     │
                 │                 │ │                 │ │  JURISDICTION   │
                 └────────┬────────┘ └─────────────────┘ └─────────────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │      CITY       │
                 │                 │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐         ┌─────────────────┐
                 │    ZIP_CODE     │────────►│ ZIP_DEMOGRAPHIC │
                 │                 │         │                 │
                 └────────┬────────┘         └─────────────────┘
                          │
          ┌───────────────┼───────────────┐
          │               │               │
          ▼               ▼               ▼
┌─────────────────┐ ┌─────────────┐ ┌─────────────────┐
│ ZIP_CODE_PLUS4  │ │   GEOCODE   │ │ POSTAL_ADDRESS  │
│                 │ │             │ │                 │
└─────────────────┘ └─────────────┘ └─────────────────┘

┌─────────────────┐              ┌─────────────────┐
│ GEOGRAPHIC_AREA │◄─────────────│   AREA_TYPE     │
│   (supertype)   │  classified  │                 │
└─────────────────┘  by          └─────────────────┘

┌─────────────────┐              ┌─────────────────┐
│SALES_TERRITORY  │◄─────────────│   TERRITORY     │
│                 │   contains   │   ASSIGNMENT    │
└─────────────────┘              └─────────────────┘

┌─────────────────┐              ┌─────────────────┐
│  MARKET_AREA    │              │ METROPOLITAN    │
│                 │              │     AREA        │
└─────────────────┘              └─────────────────┘

┌─────────────────┐              ┌─────────────────┐
│ RATING_TERRITORY│              │ URBAN_RURAL     │
│                 │              │ CLASSIFICATION  │
└─────────────────┘              └─────────────────┘

┌─────────────────┐              ┌─────────────────┐
│   TIME_ZONE     │              │ DISTANCE_BAND   │
│                 │              │                 │
└─────────────────┘              └─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Postal Address | Party/Customer | Address | Address contact point |
| State | Product | Product State Availability | Products available by state |
| Regulatory Jurisdiction | Compliance | Regulatory Filing | Filings by jurisdiction |
| Licensing Jurisdiction | Compliance | License | Licensing by jurisdiction |
| State | Policy | Policy | Policy jurisdiction |
| ZIP Code | Party/Customer | Address | Address ZIP code |
| Rating Territory | Product | Rate Table | Territory-based rates |
| Sales Territory | Sales & Distribution | Sale | Sales by territory |
| State | Premium & Billing | Billing | State-specific billing rules |
| State | Claims | Death Certificate | Certificate issuing state |
| Market Area | Sales & Distribution | Campaign | Campaign targeting |
| ZIP Demographic | Sales & Distribution | Lead | Lead targeting |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Geographic Area | Reference | Supertype for all geographic units |
| Area Type | Reference | Geographic level classification |
| Country | Reference | Highest-level geography |
| Region | Reference | Multi-state grouping |
| State | Reference | Primary regulatory geography |
| County | Reference | Sub-state administrative unit |
| City | Reference | Municipality |
| ZIP Code | Reference | Postal delivery area |
| ZIP Code Plus4 | Reference | Extended postal code |
| Postal Address | Master | Standardized street address |
| Regulatory Jurisdiction | Reference | Regulatory authority area |
| Licensing Jurisdiction | Reference | Licensing authority area |
| Sales Territory | Reference | Sales management geography |
| Territory Assignment | Reference | Territory-area mapping |
| Market Area | Reference | Strategic market geography |
| Metropolitan Area | Reference | MSA definition |
| Rating Territory | Reference | Insurance rating geography |
| Urban/Rural Classification | Reference | Population density class |
| State Demographic | Reference | State-level demographics |
| ZIP Demographic | Reference | ZIP-level demographics |
| Geocode | Reference | Lat/long coordinates |
| Time Zone | Reference | Time zone assignment |
| Distance Band | Reference | Distance classification |

**Total Entities: 23**

---

## Assumptions Log

1. Geographic areas form strict hierarchy
2. USPS and FIPS codes used as standards
3. Addresses validated against postal database
4. State is primary regulatory jurisdiction
5. Territories may not align with standard geography
6. Metropolitan areas use OMB definitions
7. Demographics updated per census or annually
8. Geocodes from standard geocoding service
9. Life insurance typically does not territory-rate

---

## Open Questions

1. Will multiple hierarchy views be needed?
2. Will international coverage be offered?
3. What regional structure will be used?
4. Will ZIP+4 level be needed for analysis?
5. What address validation service will be used?
6. How are sales territories defined and managed?
7. Which market area definitions will be used (MSA, DMA)?
8. Will territorial rating be used for any products?
9. What demographic metrics will be maintained?
10. What third-party demographic sources will be used?
11. What geocoding service will be used?
12. What distance bands will be defined?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
