# Customer Engagement Domain
## Logical Data Model - Entity Definitions

**Domain:** Customer Engagement  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Supporting & Analytical Domain

---

## Domain Overview

**Purpose:** Capture all customer interactions across channels, service requests, communication preferences, and engagement analytics to support customer experience management.

**Scope:** This domain encompasses customer service interactions, communication channels, service requests, customer communications, satisfaction measurement, and engagement tracking.

**Business Value:**
- Customer experience measurement
- Service quality monitoring
- Multi-channel engagement tracking
- Communication preference management
- Customer retention analytics
- Voice of customer insights

---

## Entity Catalog

### CUSTOMER_INTERACTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer Interaction |
| **Entity Definition** | A discrete engagement between a Customer and the insurance company through any channel. Interactions are the atomic unit of customer engagement measurement. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Interaction Identifier |
| **Key Relationships** | - Initiated by Customer (Party/Customer Domain)<br>- Occurs through Channel<br>- Has Interaction Type<br>- May relate to Policy (Policy Domain)<br>- May result in Service Request |
| **Assumptions** | - All channel interactions captured<br>- Interactions linked to customer identity |
| **Open Questions** | - How to track anonymous interactions? |

---

### INTERACTION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Interaction Type |
| **Entity Definition** | A classification of Customer Interactions based on the purpose or nature of the engagement. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Customer Interaction |
| **Assumptions** | - Types: INQUIRY, SERVICE_REQUEST, COMPLAINT, CLAIM_INQUIRY, PAYMENT, POLICY_CHANGE, QUOTE, FEEDBACK |
| **Open Questions** | - None at this time |

---

### COMMUNICATION_CHANNEL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Communication Channel |
| **Entity Definition** | A medium through which customers interact with the company, including phone, email, chat, mobile app, web portal, mail, and in-person. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Channel Code |
| **Key Relationships** | - Used for Customer Interaction<br>- Has channel capabilities and hours |
| **Assumptions** | - All channels tracked consistently<br>- Channel capabilities defined |
| **Open Questions** | - What channels are supported? |

---

### SERVICE_REQUEST

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Service Request |
| **Entity Definition** | A formal request from a Customer for service or action, typically requiring tracking and resolution. Service Requests have defined workflows and SLAs. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Request Identifier |
| **Key Relationships** | - Submitted by Customer (Party/Customer Domain)<br>- Has Request Type<br>- Has Request Status<br>- May relate to Policy (Policy Domain)<br>- Assigned to Agent/Queue |
| **Assumptions** | - Service requests tracked to resolution<br>- SLAs defined by request type |
| **Open Questions** | - What SLA framework? |

---

### SERVICE_REQUEST_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Service Request Type |
| **Entity Definition** | A classification of Service Requests based on the nature of the request and required handling. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Service Request<br>- Has defined SLA |
| **Assumptions** | - Types: NAME_CHANGE, ADDRESS_CHANGE, BENEFICIARY_CHANGE, PAYMENT_INQUIRY, POLICY_INQUIRY, DOCUMENT_REQUEST, COMPLAINT, CANCELLATION |
| **Open Questions** | - None at this time |

---

### SERVICE_REQUEST_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Service Request Status |
| **Entity Definition** | The current state of a Service Request in its lifecycle from submission to resolution. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Applied to Service Request |
| **Assumptions** | - Statuses: SUBMITTED, ASSIGNED, IN_PROGRESS, PENDING_CUSTOMER, PENDING_INFO, RESOLVED, CLOSED, ESCALATED |
| **Open Questions** | - None at this time |

---

### CUSTOMER_COMMUNICATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer Communication |
| **Entity Definition** | An outbound message sent to a Customer, including notices, statements, marketing, and transactional communications. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Communication Identifier |
| **Key Relationships** | - Sent to Customer (Party/Customer Domain)<br>- Has Communication Type<br>- Sent via Channel<br>- May relate to Policy (Policy Domain) |
| **Assumptions** | - All outbound communications logged<br>- Delivery tracking where possible |
| **Open Questions** | - What delivery tracking is available? |

---

### COMMUNICATION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Communication Type |
| **Entity Definition** | A classification of Customer Communications based on the purpose and content type. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Customer Communication |
| **Assumptions** | - Types: POLICY_NOTICE, BILLING_STATEMENT, CLAIM_STATUS, WELCOME, RENEWAL, MARKETING, REGULATORY, SURVEY |
| **Open Questions** | - None at this time |

---

### COMMUNICATION_PREFERENCE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Communication Preference |
| **Entity Definition** | A Customer's stated preferences for receiving communications, including preferred channels, frequency, and opt-in/opt-out status. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Customer Identifier + Preference Type |
| **Key Relationships** | - Held by Customer (Party/Customer Domain)<br>- For Communication Type<br>- Specifies Channel preference |
| **Assumptions** | - Preferences captured and maintained<br>- Regulatory opt-out honored |
| **Open Questions** | - What preference categories? |

---

### COMMUNICATION_TEMPLATE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Communication Template |
| **Entity Definition** | A pre-defined template used to generate Customer Communications, ensuring consistent messaging and branding. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Template Identifier |
| **Key Relationships** | - Used for Communication Type<br>- Has Template Version<br>- Supports multiple Channels |
| **Assumptions** | - Templates managed centrally<br>- Version control maintained |
| **Open Questions** | - None at this time |

---

### CUSTOMER_FEEDBACK

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer Feedback |
| **Entity Definition** | Direct feedback received from a Customer about their experience, whether solicited (survey) or unsolicited (complaint, compliment). |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Feedback Identifier |
| **Key Relationships** | - Provided by Customer (Party/Customer Domain)<br>- Has Feedback Type<br>- May relate to Interaction |
| **Assumptions** | - All feedback captured and categorized<br>- Used for service improvement |
| **Open Questions** | - None at this time |

---

### FEEDBACK_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Feedback Type |
| **Entity Definition** | A classification of Customer Feedback based on the nature and source of the feedback. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Customer Feedback |
| **Assumptions** | - Types: NPS_SURVEY, CSAT_SURVEY, COMPLAINT, COMPLIMENT, SUGGESTION, REVIEW |
| **Open Questions** | - None at this time |

---

### CUSTOMER_SURVEY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer Survey |
| **Entity Definition** | A structured questionnaire sent to Customers to gather feedback on specific aspects of their experience. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Survey Identifier |
| **Key Relationships** | - Sent to Customer<br>- Has Survey Type<br>- Generates Survey Response<br>- May trigger after Interaction |
| **Assumptions** | - Surveys sent per defined triggers<br>- Response rates tracked |
| **Open Questions** | - What survey triggers? |

---

### SURVEY_RESPONSE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Survey Response |
| **Entity Definition** | A completed response to a Customer Survey, including scored and free-text answers. Survey Responses are the primary input for satisfaction metrics. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Response Identifier |
| **Key Relationships** | - Response to Survey<br>- Contains Response Items<br>- Generates Satisfaction Score |
| **Assumptions** | - Responses captured completely<br>- Anonymous option available |
| **Open Questions** | - None at this time |

---

### SATISFACTION_SCORE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Satisfaction Score |
| **Entity Definition** | A calculated metric representing customer satisfaction, such as NPS (Net Promoter Score), CSAT, or CES (Customer Effort Score). |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Score Identifier |
| **Key Relationships** | - Calculated from Survey Response<br>- Has Score Type<br>- For Customer/Segment/Period |
| **Assumptions** | - Standard metrics calculated<br>- Benchmarking available |
| **Open Questions** | - What satisfaction metrics will be tracked? |

---

### SCORE_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Score Type |
| **Entity Definition** | A classification of Satisfaction Scores based on the measurement methodology. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Satisfaction Score |
| **Assumptions** | - Types: NPS, CSAT, CES, TRANSACTIONAL_NPS |
| **Open Questions** | - None at this time |

---

### COMPLAINT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Complaint |
| **Entity Definition** | A formal expression of dissatisfaction from a Customer requiring investigation and resolution. Complaints are tracked separately for regulatory and quality purposes. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Complaint Identifier |
| **Key Relationships** | - Filed by Customer (Party/Customer Domain)<br>- Has Complaint Category<br>- Has Complaint Status<br>- May escalate to Regulatory Complaint (Compliance Domain) |
| **Assumptions** | - All complaints logged<br>- Resolution tracked |
| **Open Questions** | - What escalation triggers? |

---

### COMPLAINT_CATEGORY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Complaint Category |
| **Entity Definition** | A classification of Complaints based on the subject matter or area of concern. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Category Code |
| **Key Relationships** | - Classifies Complaint |
| **Assumptions** | - Categories: SERVICE_QUALITY, CLAIMS_HANDLING, BILLING, POLICY_TERMS, SALES_PRACTICES, COMMUNICATION, PRIVACY |
| **Open Questions** | - None at this time |

---

### COMPLAINT_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Complaint Status |
| **Entity Definition** | The current state of a Complaint in its lifecycle from receipt to resolution. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Applied to Complaint |
| **Assumptions** | - Statuses: RECEIVED, INVESTIGATING, PENDING_RESPONSE, RESOLVED, CLOSED, ESCALATED |
| **Open Questions** | - None at this time |

---

### CUSTOMER_JOURNEY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer Journey |
| **Entity Definition** | The sequence of interactions and touchpoints a Customer experiences with the company over time, from awareness through retention. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Journey Identifier |
| **Key Relationships** | - Experienced by Customer<br>- Follows Journey Type<br>- Contains Touchpoints |
| **Assumptions** | - Journeys reconstructed from interactions<br>- Key journeys defined |
| **Open Questions** | - What key journeys to track? |

---

### JOURNEY_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Journey Type |
| **Entity Definition** | A classification of Customer Journeys based on the primary goal or lifecycle stage. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Type Code |
| **Key Relationships** | - Classifies Customer Journey |
| **Assumptions** | - Types: QUOTE_TO_PURCHASE, ONBOARDING, CLAIM_EXPERIENCE, RENEWAL, SERVICE_REQUEST |
| **Open Questions** | - None at this time |

---

### TOUCHPOINT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Touchpoint |
| **Entity Definition** | A specific point of interaction or experience within a Customer Journey. Touchpoints are analyzed for friction and improvement opportunities. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Touchpoint Identifier |
| **Key Relationships** | - Part of Customer Journey<br>- Has Touchpoint Type<br>- Maps to Interaction |
| **Assumptions** | - Key touchpoints identified<br>- Experience measured at touchpoints |
| **Open Questions** | - None at this time |

---

### CASE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Case |
| **Entity Definition** | A complex customer matter requiring coordination across multiple interactions, possibly spanning multiple service requests. Cases provide a wrapper for complex issues. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Case Identifier |
| **Key Relationships** | - Opened for Customer<br>- Contains Service Requests<br>- Has Case Status<br>- Assigned to Case Manager |
| **Assumptions** | - Cases used for complex issues<br>- Case management workflow defined |
| **Open Questions** | - When to create case vs. service request? |

---

### CASE_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Case Status |
| **Entity Definition** | The current state of a Case in its lifecycle. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Applied to Case |
| **Assumptions** | - Statuses: OPEN, ACTIVE, PENDING, RESOLVED, CLOSED |
| **Open Questions** | - None at this time |

---

### SERVICE_AGENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Service Agent |
| **Entity Definition** | An employee or contractor who handles customer interactions and service requests. Service Agents are measured on performance and quality. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Agent Identifier |
| **Key Relationships** | - Is a Party Role (Party/Customer Domain)<br>- Assigned to Interactions/Requests<br>- Has Agent Skills<br>- Member of Team/Queue |
| **Assumptions** | - Agents tracked for performance<br>- Skills-based routing available |
| **Open Questions** | - None at this time |

---

### AGENT_SKILL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Agent Skill |
| **Entity Definition** | A capability or certification held by a Service Agent, used for skills-based routing and development. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Agent Identifier + Skill Code |
| **Key Relationships** | - Held by Service Agent<br>- Required for Request Types |
| **Assumptions** | - Skills defined and maintained<br>- Used for routing |
| **Open Questions** | - None at this time |

---

### SERVICE_QUEUE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Service Queue |
| **Entity Definition** | A logical grouping for routing and managing service work, such as phone queue, email queue, or specialty queue. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Queue Identifier |
| **Key Relationships** | - Contains Service Requests<br>- Staffed by Service Agents<br>- Has SLA targets |
| **Assumptions** | - Queues defined by channel and specialty<br>- Queue metrics tracked |
| **Open Questions** | - None at this time |

---

### SERVICE_LEVEL_AGREEMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Service Level Agreement |
| **Entity Definition** | A defined target for service performance, such as response time or resolution time, for specific request types or channels. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | SLA Identifier |
| **Key Relationships** | - Applied to Request Type/Channel<br>- Has SLA Metric and Target<br>- Measured for compliance |
| **Assumptions** | - SLAs defined by business<br>- Performance tracked against SLA |
| **Open Questions** | - None at this time |

---

### SLA_COMPLIANCE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | SLA Compliance |
| **Entity Definition** | A record of whether a Service Request met or missed its SLA target. SLA Compliance is used for service quality reporting. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Service Request Identifier |
| **Key Relationships** | - Measured for Service Request<br>- Against Service Level Agreement |
| **Assumptions** | - Compliance tracked automatically<br>- Miss reasons captured |
| **Open Questions** | - None at this time |

---

### CUSTOMER_SENTIMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Customer Sentiment |
| **Entity Definition** | An assessed emotional tone or attitude of a Customer during or after an interaction, typically derived from text or voice analysis. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Sentiment Identifier |
| **Key Relationships** | - Assessed for Customer<br>- During Interaction<br>- Has Sentiment Score |
| **Assumptions** | - Sentiment analysis capability exists<br>- Used for experience insights |
| **Open Questions** | - What sentiment analysis approach? |

---

### ENGAGEMENT_SCORE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Engagement Score |
| **Entity Definition** | A calculated metric representing the overall engagement level of a Customer with the company, based on interaction frequency, channel usage, and recency. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Customer Identifier + Score Date |
| **Key Relationships** | - Calculated for Customer<br>- Based on Interactions<br>- Used for segmentation |
| **Assumptions** | - Engagement scoring model defined<br>- Updated periodically |
| **Open Questions** | - What engagement scoring model? |

---

## Conceptual Entity-Relationship Diagram

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│    CUSTOMER     │────────►│   CUSTOMER      │────────►│ INTERACTION     │
│ (Party Domain)  │  has    │  INTERACTION    │         │    TYPE         │
└─────────────────┘         └────────┬────────┘         └─────────────────┘
                                     │
                          ┌──────────┼──────────┐
                          │          │          │
                          ▼          ▼          ▼
               ┌─────────────┐ ┌─────────────┐ ┌─────────────────┐
               │SERVICE      │ │COMMUNICATION│ │ CUSTOMER        │
               │  REQUEST    │ │  CHANNEL    │ │   FEEDBACK      │
               └──────┬──────┘ └─────────────┘ └────────┬────────┘
                      │                                 │
           ┌──────────┼──────────┐                      ▼
           ▼          ▼          ▼             ┌─────────────────┐
    ┌───────────┐ ┌───────────┐ ┌───────────┐  │  FEEDBACK_TYPE  │
    │SR_TYPE    │ │SR_STATUS  │ │SERVICE    │  └─────────────────┘
    └───────────┘ └───────────┘ │  QUEUE    │
                               └───────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│    CUSTOMER     │────────►│COMMUNICATION    │         │COMMUNICATION    │
│ (Party Domain)  │  sent   │                 │────────►│    TYPE         │
└─────────────────┘         └────────┬────────┘         └─────────────────┘
                                     │
                          ┌──────────┴──────────┐
                          ▼                     ▼
               ┌─────────────────┐   ┌─────────────────┐
               │  COMMUNICATION  │   │  COMMUNICATION  │
               │   PREFERENCE    │   │    TEMPLATE     │
               └─────────────────┘   └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│ CUSTOMER_SURVEY │────────►│ SURVEY_RESPONSE │────────►│SATISFACTION     │
│                 │         │                 │  yields │    SCORE        │
└─────────────────┘         └─────────────────┘         └────────┬────────┘
                                                                 │
                                                                 ▼
                                                        ┌─────────────────┐
                                                        │   SCORE_TYPE    │
                                                        └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│    COMPLAINT    │────────►│   COMPLAINT     │         │   COMPLAINT     │
│                 │         │    CATEGORY     │         │    STATUS       │
└─────────────────┘         └─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│CUSTOMER_JOURNEY │────────►│  JOURNEY_TYPE   │         │   TOUCHPOINT    │
│                 │         │                 │         │                 │
└────────┬────────┘         └─────────────────┘         └─────────────────┘
         │
         └─────────────────────────────────────────────────────┘
                                  contains

┌─────────────────┐         ┌─────────────────┐
│      CASE       │────────►│   CASE_STATUS   │
│                 │         │                 │
└────────┬────────┘         └─────────────────┘
         │
         ├──────────────────┬──────────────────┐
         ▼                  ▼                  ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│ SERVICE_REQUEST │ │  SERVICE_AGENT  │ │ AGENT_SKILL     │
│                 │ │                 │ │                 │
└─────────────────┘ └─────────────────┘ └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│SERVICE_LEVEL    │────────►│  SLA_COMPLIANCE │
│   AGREEMENT     │         │                 │
└─────────────────┘         └─────────────────┘

┌─────────────────┐         ┌─────────────────┐
│   CUSTOMER      │         │  ENGAGEMENT     │
│   SENTIMENT     │         │    SCORE        │
└─────────────────┘         └─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Customer Interaction | Party/Customer | Customer | Interaction with customer |
| Service Request | Policy | Policy | Request regarding policy |
| Service Request | Claims | Claim | Request regarding claim |
| Customer Communication | Policy | Policy | Communication about policy |
| Complaint | Compliance/Regulatory | Regulatory Complaint | Escalation if regulatory |
| Service Agent | Party/Customer | Party Role | Agent as party role |
| Customer Journey | Sales & Distribution | Lead, Quote | Quote-to-purchase journey |
| Customer Journey | Claims | Claim | Claim experience journey |
| Customer Feedback | Policy | Policy | Feedback about policy service |
| Communication Preference | Party/Customer | Contact Point | Preferred contact channels |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Customer Interaction | Transactional | Customer engagement event |
| Interaction Type | Reference | Interaction classification |
| Communication Channel | Reference | Engagement medium |
| Service Request | Transactional | Service/action request |
| Service Request Type | Reference | Request classification |
| Service Request Status | Reference | Request state |
| Customer Communication | Transactional | Outbound message |
| Communication Type | Reference | Message classification |
| Communication Preference | Master | Channel/frequency preferences |
| Communication Template | Reference | Message template |
| Customer Feedback | Transactional | Customer input |
| Feedback Type | Reference | Feedback classification |
| Customer Survey | Transactional | Feedback questionnaire |
| Survey Response | Transactional | Completed survey |
| Satisfaction Score | Transactional | NPS/CSAT metrics |
| Score Type | Reference | Metric classification |
| Complaint | Transactional | Formal dissatisfaction |
| Complaint Category | Reference | Complaint classification |
| Complaint Status | Reference | Complaint state |
| Customer Journey | Transactional | Experience sequence |
| Journey Type | Reference | Journey classification |
| Touchpoint | Transactional | Journey interaction point |
| Case | Transactional | Complex issue wrapper |
| Case Status | Reference | Case state |
| Service Agent | Master | Customer service employee |
| Agent Skill | Reference | Agent capability |
| Service Queue | Reference | Work routing group |
| Service Level Agreement | Reference | Performance target |
| SLA Compliance | Transactional | SLA performance record |
| Customer Sentiment | Transactional | Emotional assessment |
| Engagement Score | Transactional | Engagement metric |

**Total Entities: 31**

---

## Assumptions Log

1. All customer interactions captured across channels
2. Service requests tracked to resolution
3. Outbound communications logged
4. Communication preferences maintained
5. Customer feedback captured and analyzed
6. Complaints tracked separately for regulatory purposes
7. Customer journeys reconstructed from interactions
8. Service agents tracked for performance
9. SLAs defined and measured

---

## Open Questions

1. How to track anonymous interactions?
2. What channels are supported?
3. What SLA framework?
4. What delivery tracking is available for communications?
5. What preference categories?
6. What satisfaction metrics will be tracked?
7. What escalation triggers for complaints?
8. What key customer journeys to track?
9. When to create case vs. service request?
10. What sentiment analysis approach?
11. What engagement scoring model?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
