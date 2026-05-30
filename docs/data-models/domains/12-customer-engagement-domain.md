# Customer Engagement Domain
## Logical Data Model - Entity Definitions

**Domain:** Customer Engagement  
**Version:** 1.1  
**Date:** December 6, 2025  
**Status:** Phase 2 - Attribute Definition (P2 Entities)  
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

## Attribute Definitions - Phase 2 (P2) Entities

This section defines the detailed attributes for all Phase 2 (Operations) entities in the Customer Engagement domain.

---

### CUSTOMER_INTERACTION Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| interaction_id | VARCHAR(36) | No | Unique identifier for the interaction. | Primary Key. UUID format recommended. |
| customer_id | VARCHAR(36) | Yes | Customer who initiated/participated in interaction. | Foreign Key to CUSTOMER. Null for anonymous. |
| party_id | VARCHAR(36) | Yes | Party if customer not yet established. | Foreign Key to PARTY. For prospects. |
| interaction_type_code | VARCHAR(20) | No | Type/purpose of the interaction. | Foreign Key to INTERACTION_TYPE. |
| channel_code | VARCHAR(20) | No | Channel through which interaction occurred. | Foreign Key to COMMUNICATION_CHANNEL. |
| interaction_direction_code | VARCHAR(20) | No | Direction of the interaction. | Valid values: 'INBOUND', 'OUTBOUND', 'BIDIRECTIONAL'. |
| interaction_start_timestamp | TIMESTAMP | No | When the interaction began. | Required for duration calculation. |
| interaction_end_timestamp | TIMESTAMP | Yes | When the interaction concluded. | Null if still in progress. |
| interaction_duration_seconds | INTEGER | Yes | Duration of interaction in seconds. | Calculated field. |
| interaction_status_code | VARCHAR(20) | No | Current status of interaction. | Valid values: 'IN_PROGRESS', 'COMPLETED', 'ABANDONED', 'TRANSFERRED'. |
| queue_wait_seconds | INTEGER | Yes | Time spent waiting in queue. | For phone/chat interactions. |
| policy_id | VARCHAR(36) | Yes | Related policy if applicable. | Foreign Key to POLICY (Policy Domain). |
| claim_id | VARCHAR(36) | Yes | Related claim if applicable. | Foreign Key to CLAIM (Claims Domain). |
| service_request_id | VARCHAR(36) | Yes | Service request created from interaction. | Foreign Key to SERVICE_REQUEST. |
| assigned_agent_id | VARCHAR(36) | Yes | Agent who handled the interaction. | Foreign Key to SERVICE_AGENT. |
| transfer_count | INTEGER | No | Number of times interaction was transferred. | Default: 0. |
| first_contact_resolution_flag | CHAR(1) | Yes | Issue resolved in first contact. | Valid values: 'Y', 'N'. |
| interaction_summary | VARCHAR(2000) | Yes | Summary/notes about the interaction. | Free text from agent or system. |
| contact_reason_code | VARCHAR(50) | Yes | Primary reason for contact. | Standardized reason codes. |
| contact_reason_detail | VARCHAR(500) | Yes | Detailed description of contact reason. | Free text elaboration. |
| disposition_code | VARCHAR(50) | Yes | How the interaction was concluded. | Standardized disposition codes. |
| callback_requested_flag | CHAR(1) | No | Customer requested callback. | Valid values: 'Y', 'N'. Default: 'N'. |
| callback_scheduled_timestamp | TIMESTAMP | Yes | When callback is scheduled. | Required if callback requested. |
| external_reference_id | VARCHAR(100) | Yes | Reference ID from external system. | For telephony/CRM integration. |
| session_id | VARCHAR(100) | Yes | Web/app session identifier. | For digital channel interactions. |
| device_type_code | VARCHAR(20) | Yes | Device used for digital interactions. | Valid values: 'DESKTOP', 'MOBILE', 'TABLET', 'APP'. |
| recording_available_flag | CHAR(1) | Yes | Call/screen recording available. | Valid values: 'Y', 'N'. |
| recording_reference | VARCHAR(200) | Yes | Reference to recording storage. | Path or URL to recording. |
| quality_reviewed_flag | CHAR(1) | No | Interaction has been quality reviewed. | Valid values: 'Y', 'N'. Default: 'N'. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### INTERACTION_TYPE Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| interaction_type_code | VARCHAR(20) | No | Unique code for the interaction type. | Primary Key. Uppercase, underscore-separated. |
| interaction_type_name | VARCHAR(100) | No | Display name for the type. | Human-readable name. |
| interaction_type_description | VARCHAR(500) | Yes | Detailed description. | Explains type purpose. |
| interaction_category_code | VARCHAR(20) | No | Category grouping. | Valid values: 'SERVICE', 'SALES', 'CLAIMS', 'BILLING', 'GENERAL'. |
| requires_follow_up_flag | CHAR(1) | No | Type typically requires follow-up. | Valid values: 'Y', 'N'. |
| creates_service_request_flag | CHAR(1) | No | Type typically creates service request. | Valid values: 'Y', 'N'. |
| fcr_eligible_flag | CHAR(1) | No | Eligible for first contact resolution. | Valid values: 'Y', 'N'. |
| expected_duration_minutes | INTEGER | Yes | Expected average duration. | For planning and SLA. |
| priority_level | INTEGER | No | Default priority for this type. | 1=Highest, 5=Lowest. Default: 3. |
| display_sequence | INTEGER | Yes | Order for display in UI. | Lower numbers appear first. |
| active_flag | CHAR(1) | No | Type is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| effective_date | DATE | No | Date type became available. | Reference data versioning. |
| expiration_date | DATE | Yes | Date type was retired. | Null = currently active. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

**Standard Interaction Type Values:**

| Code | Name | Category | Creates SR | FCR Eligible |
|------|------|----------|------------|--------------|
| INQUIRY | General Inquiry | GENERAL | N | Y |
| SERVICE_REQUEST | Service Request | SERVICE | Y | N |
| COMPLAINT | Complaint | SERVICE | Y | N |
| CLAIM_INQUIRY | Claim Inquiry | CLAIMS | N | Y |
| CLAIM_FILING | Claim Filing | CLAIMS | Y | N |
| PAYMENT | Payment Inquiry | BILLING | N | Y |
| POLICY_CHANGE | Policy Change Request | SERVICE | Y | N |
| QUOTE_REQUEST | Quote Request | SALES | Y | N |
| FEEDBACK | Customer Feedback | GENERAL | N | Y |
| DOCUMENT_REQUEST | Document Request | SERVICE | Y | Y |

---

### COMMUNICATION_CHANNEL Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| channel_code | VARCHAR(20) | No | Unique code for the channel. | Primary Key. Uppercase, underscore-separated. |
| channel_name | VARCHAR(100) | No | Display name for the channel. | Human-readable name. |
| channel_description | VARCHAR(500) | Yes | Detailed description. | Explains channel characteristics. |
| channel_category_code | VARCHAR(20) | No | Category of channel. | Valid values: 'VOICE', 'DIGITAL', 'WRITTEN', 'IN_PERSON'. |
| is_synchronous_flag | CHAR(1) | No | Real-time interaction channel. | Valid values: 'Y', 'N'. |
| is_self_service_flag | CHAR(1) | No | Customer self-service channel. | Valid values: 'Y', 'N'. |
| supports_inbound_flag | CHAR(1) | No | Supports inbound interactions. | Valid values: 'Y', 'N'. |
| supports_outbound_flag | CHAR(1) | No | Supports outbound interactions. | Valid values: 'Y', 'N'. |
| hours_of_operation | VARCHAR(200) | Yes | When channel is available. | Text description or JSON. |
| average_response_time_minutes | INTEGER | Yes | Expected response time. | For asynchronous channels. |
| cost_per_interaction | DECIMAL(10,2) | Yes | Average cost per interaction. | For channel economics. |
| preferred_for_types | VARCHAR(500) | Yes | Interaction types best suited. | Comma-separated type codes. |
| requires_authentication_flag | CHAR(1) | No | Requires customer authentication. | Valid values: 'Y', 'N'. |
| supports_attachments_flag | CHAR(1) | No | Supports file attachments. | Valid values: 'Y', 'N'. |
| supports_secure_messaging_flag | CHAR(1) | No | Supports secure/encrypted messages. | Valid values: 'Y', 'N'. |
| tracking_capability_code | VARCHAR(20) | Yes | Level of tracking available. | Valid values: 'FULL', 'PARTIAL', 'NONE'. |
| display_sequence | INTEGER | Yes | Order for display in UI. | Lower numbers appear first. |
| active_flag | CHAR(1) | No | Channel is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| effective_date | DATE | No | Date channel became available. | Reference data versioning. |
| expiration_date | DATE | Yes | Date channel was retired. | Null = currently active. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

**Standard Communication Channel Values:**

| Code | Name | Category | Synchronous | Self-Service |
|------|------|----------|-------------|--------------|
| PHONE | Phone Call | VOICE | Y | N |
| EMAIL | Email | DIGITAL | N | Y |
| WEB_CHAT | Web Chat | DIGITAL | Y | N |
| WEB_PORTAL | Web Portal | DIGITAL | N | Y |
| MOBILE_APP | Mobile App | DIGITAL | N | Y |
| SMS | Text Message | DIGITAL | N | Y |
| MAIL | Postal Mail | WRITTEN | N | N |
| FAX | Fax | WRITTEN | N | N |
| IN_PERSON | In-Person | IN_PERSON | Y | N |
| VIDEO_CALL | Video Call | VOICE | Y | N |
| SOCIAL_MEDIA | Social Media | DIGITAL | N | N |
| IVR | Interactive Voice Response | VOICE | Y | Y |

---

### SERVICE_REQUEST Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| service_request_id | VARCHAR(36) | No | Unique identifier for the request. | Primary Key. UUID format recommended. |
| service_request_number | VARCHAR(20) | No | Business-friendly request number. | Unique. System-generated. Display identifier. |
| customer_id | VARCHAR(36) | No | Customer who submitted the request. | Foreign Key to CUSTOMER. |
| request_type_code | VARCHAR(20) | No | Type of service request. | Foreign Key to SERVICE_REQUEST_TYPE. |
| request_status_code | VARCHAR(20) | No | Current status of the request. | Foreign Key to SERVICE_REQUEST_STATUS. |
| priority_code | VARCHAR(20) | No | Priority level of the request. | Valid values: 'CRITICAL', 'HIGH', 'MEDIUM', 'LOW'. Default: 'MEDIUM'. |
| source_channel_code | VARCHAR(20) | No | Channel where request originated. | Foreign Key to COMMUNICATION_CHANNEL. |
| source_interaction_id | VARCHAR(36) | Yes | Interaction that created the request. | Foreign Key to CUSTOMER_INTERACTION. |
| policy_id | VARCHAR(36) | Yes | Related policy if applicable. | Foreign Key to POLICY (Policy Domain). |
| claim_id | VARCHAR(36) | Yes | Related claim if applicable. | Foreign Key to CLAIM (Claims Domain). |
| case_id | VARCHAR(36) | Yes | Parent case if part of case. | Foreign Key to CASE. |
| request_subject | VARCHAR(200) | No | Brief subject/title of request. | Summary for display. |
| request_description | VARCHAR(4000) | Yes | Detailed description of request. | Full details from customer. |
| requested_completion_date | DATE | Yes | Customer's requested completion date. | Optional customer preference. |
| sla_target_timestamp | TIMESTAMP | Yes | Target completion per SLA. | Calculated from type SLA. |
| submitted_timestamp | TIMESTAMP | No | When request was submitted. | System-populated. |
| acknowledged_timestamp | TIMESTAMP | Yes | When request was acknowledged. | First response time. |
| assigned_timestamp | TIMESTAMP | Yes | When request was assigned. | Assignment tracking. |
| in_progress_timestamp | TIMESTAMP | Yes | When work began on request. | Work start tracking. |
| resolved_timestamp | TIMESTAMP | Yes | When request was resolved. | Resolution time tracking. |
| closed_timestamp | TIMESTAMP | Yes | When request was closed. | Final closure time. |
| assigned_queue_id | VARCHAR(36) | Yes | Queue request is assigned to. | Foreign Key to SERVICE_QUEUE. |
| assigned_agent_id | VARCHAR(36) | Yes | Agent assigned to request. | Foreign Key to SERVICE_AGENT. |
| escalation_level | INTEGER | No | Current escalation level. | 0=None, 1=First level, etc. Default: 0. |
| escalated_timestamp | TIMESTAMP | Yes | When last escalated. | Escalation tracking. |
| escalation_reason | VARCHAR(500) | Yes | Reason for escalation. | Required when escalated. |
| resolution_code | VARCHAR(50) | Yes | How request was resolved. | Standardized resolution codes. |
| resolution_description | VARCHAR(2000) | Yes | Description of resolution. | Details of how resolved. |
| customer_notified_flag | CHAR(1) | No | Customer notified of resolution. | Valid values: 'Y', 'N'. Default: 'N'. |
| customer_notified_timestamp | TIMESTAMP | Yes | When customer was notified. | Required if notified. |
| reopen_count | INTEGER | No | Number of times reopened. | Default: 0. |
| last_reopen_timestamp | TIMESTAMP | Yes | When last reopened. | Reopen tracking. |
| related_request_id | VARCHAR(36) | Yes | Related/duplicate request. | Foreign Key to SERVICE_REQUEST. |
| external_reference_id | VARCHAR(100) | Yes | External system reference. | For integration tracking. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### SERVICE_REQUEST_TYPE Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| request_type_code | VARCHAR(20) | No | Unique code for the request type. | Primary Key. Uppercase, underscore-separated. |
| request_type_name | VARCHAR(100) | No | Display name for the type. | Human-readable name. |
| request_type_description | VARCHAR(500) | Yes | Detailed description. | Explains type and handling. |
| request_category_code | VARCHAR(20) | No | Category grouping. | Valid values: 'POLICY_SERVICE', 'BILLING', 'CLAIMS', 'GENERAL', 'COMPLAINT'. |
| default_priority_code | VARCHAR(20) | No | Default priority for type. | Valid values: 'CRITICAL', 'HIGH', 'MEDIUM', 'LOW'. |
| default_sla_id | VARCHAR(36) | Yes | Default SLA for this type. | Foreign Key to SERVICE_LEVEL_AGREEMENT. |
| target_response_hours | INTEGER | Yes | Target first response time. | Hours to first acknowledgment. |
| target_resolution_hours | INTEGER | Yes | Target resolution time. | Hours to resolution. |
| requires_approval_flag | CHAR(1) | No | Type requires approval to process. | Valid values: 'Y', 'N'. |
| approval_level_required | INTEGER | Yes | Level of approval needed. | 1=Supervisor, 2=Manager, etc. |
| auto_assign_queue_id | VARCHAR(36) | Yes | Queue for auto-assignment. | Foreign Key to SERVICE_QUEUE. |
| required_skills | VARCHAR(200) | Yes | Skills required to handle. | Comma-separated skill codes. |
| creates_workflow_flag | CHAR(1) | No | Creates automated workflow. | Valid values: 'Y', 'N'. |
| workflow_template_id | VARCHAR(36) | Yes | Workflow template to use. | Required if creates workflow. |
| regulatory_flag | CHAR(1) | No | Has regulatory implications. | Valid values: 'Y', 'N'. |
| regulatory_response_days | INTEGER | Yes | Regulatory response deadline. | Days if regulatory. |
| display_sequence | INTEGER | Yes | Order for display in UI. | Lower numbers appear first. |
| active_flag | CHAR(1) | No | Type is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| effective_date | DATE | No | Date type became available. | Reference data versioning. |
| expiration_date | DATE | Yes | Date type was retired. | Null = currently active. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

**Standard Service Request Type Values:**

| Code | Name | Category | Priority | Response Hours |
|------|------|----------|----------|----------------|
| NAME_CHANGE | Name Change | POLICY_SERVICE | MEDIUM | 24 |
| ADDRESS_CHANGE | Address Change | POLICY_SERVICE | MEDIUM | 24 |
| BENEFICIARY_CHANGE | Beneficiary Change | POLICY_SERVICE | HIGH | 24 |
| PAYMENT_INQUIRY | Payment Inquiry | BILLING | MEDIUM | 24 |
| POLICY_INQUIRY | Policy Inquiry | POLICY_SERVICE | LOW | 48 |
| DOCUMENT_REQUEST | Document Request | GENERAL | MEDIUM | 24 |
| COMPLAINT | Complaint | COMPLAINT | HIGH | 4 |
| CANCELLATION | Cancellation Request | POLICY_SERVICE | HIGH | 24 |
| REINSTATEMENT | Reinstatement Request | POLICY_SERVICE | HIGH | 24 |
| LOAN_REQUEST | Policy Loan Request | POLICY_SERVICE | MEDIUM | 48 |
| WITHDRAWAL | Withdrawal Request | POLICY_SERVICE | HIGH | 24 |

---

### SERVICE_REQUEST_STATUS Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| request_status_code | VARCHAR(20) | No | Unique code for the status. | Primary Key. Uppercase, underscore-separated. |
| request_status_name | VARCHAR(100) | No | Display name for the status. | Human-readable name. |
| request_status_description | VARCHAR(500) | Yes | Detailed description. | Explains status meaning. |
| status_category_code | VARCHAR(20) | No | Category of status. | Valid values: 'OPEN', 'IN_WORK', 'PENDING', 'CLOSED'. |
| is_initial_status_flag | CHAR(1) | No | Can be initial status. | Valid values: 'Y', 'N'. |
| is_terminal_status_flag | CHAR(1) | No | Is a final/closed status. | Valid values: 'Y', 'N'. |
| stops_sla_clock_flag | CHAR(1) | No | Stops SLA clock when in this status. | Valid values: 'Y', 'N'. |
| requires_customer_action_flag | CHAR(1) | No | Waiting on customer action. | Valid values: 'Y', 'N'. |
| allowed_next_statuses | VARCHAR(200) | Yes | Valid next status transitions. | Comma-separated status codes. |
| auto_escalate_after_hours | INTEGER | Yes | Hours before auto-escalation. | Null = no auto-escalation. |
| send_notification_flag | CHAR(1) | No | Send notification on status change. | Valid values: 'Y', 'N'. |
| notification_template_id | VARCHAR(36) | Yes | Template for notification. | Required if send notification. |
| display_color_code | VARCHAR(10) | Yes | Color for UI display. | Hex color code. |
| display_sequence | INTEGER | Yes | Order for display in UI. | Lower numbers appear first. |
| active_flag | CHAR(1) | No | Status is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

**Standard Service Request Status Values:**

| Code | Name | Category | Initial | Terminal | Stops SLA |
|------|------|----------|---------|----------|-----------|
| SUBMITTED | Submitted | OPEN | Y | N | N |
| ASSIGNED | Assigned | OPEN | N | N | N |
| IN_PROGRESS | In Progress | IN_WORK | N | N | N |
| PENDING_CUSTOMER | Pending Customer | PENDING | N | N | Y |
| PENDING_INFO | Pending Information | PENDING | N | N | Y |
| PENDING_APPROVAL | Pending Approval | PENDING | N | N | N |
| RESOLVED | Resolved | CLOSED | N | N | N |
| CLOSED | Closed | CLOSED | N | Y | Y |
| CANCELLED | Cancelled | CLOSED | N | Y | Y |
| ESCALATED | Escalated | IN_WORK | N | N | N |

---

### CUSTOMER_COMMUNICATION Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| communication_id | VARCHAR(36) | No | Unique identifier for the communication. | Primary Key. UUID format recommended. |
| communication_number | VARCHAR(20) | Yes | Business-friendly reference number. | System-generated if applicable. |
| customer_id | VARCHAR(36) | No | Recipient customer. | Foreign Key to CUSTOMER. |
| party_id | VARCHAR(36) | Yes | Recipient party if not customer. | Foreign Key to PARTY. |
| communication_type_code | VARCHAR(20) | No | Type of communication. | Foreign Key to COMMUNICATION_TYPE. |
| channel_code | VARCHAR(20) | No | Delivery channel. | Foreign Key to COMMUNICATION_CHANNEL. |
| template_id | VARCHAR(36) | Yes | Template used to generate. | Foreign Key to COMMUNICATION_TEMPLATE. |
| policy_id | VARCHAR(36) | Yes | Related policy if applicable. | Foreign Key to POLICY (Policy Domain). |
| claim_id | VARCHAR(36) | Yes | Related claim if applicable. | Foreign Key to CLAIM (Claims Domain). |
| service_request_id | VARCHAR(36) | Yes | Related service request. | Foreign Key to SERVICE_REQUEST. |
| communication_subject | VARCHAR(200) | Yes | Subject line or title. | For emails and letters. |
| communication_body | VARCHAR(4000) | Yes | Main content/body text. | Actual message content. |
| communication_body_html | TEXT | Yes | HTML formatted body. | For rich email content. |
| scheduled_send_timestamp | TIMESTAMP | Yes | When scheduled to send. | For future-dated communications. |
| actual_send_timestamp | TIMESTAMP | Yes | When actually sent. | Populated after sending. |
| delivery_status_code | VARCHAR(20) | No | Current delivery status. | Valid values: 'PENDING', 'SENT', 'DELIVERED', 'BOUNCED', 'FAILED', 'OPENED'. |
| delivery_timestamp | TIMESTAMP | Yes | When delivered (if known). | From delivery tracking. |
| bounce_reason_code | VARCHAR(50) | Yes | Reason for bounce/failure. | Populated if bounced/failed. |
| bounce_detail | VARCHAR(500) | Yes | Detailed bounce information. | Technical bounce details. |
| opened_flag | CHAR(1) | Yes | Communication was opened/read. | Valid values: 'Y', 'N'. |
| opened_timestamp | TIMESTAMP | Yes | When first opened. | From tracking pixel or read receipt. |
| open_count | INTEGER | Yes | Number of times opened. | For engagement tracking. |
| clicked_flag | CHAR(1) | Yes | Links were clicked. | Valid values: 'Y', 'N'. |
| first_click_timestamp | TIMESTAMP | Yes | When first link clicked. | Click tracking. |
| click_count | INTEGER | Yes | Total click count. | All links combined. |
| unsubscribed_flag | CHAR(1) | No | Recipient unsubscribed. | Valid values: 'Y', 'N'. Default: 'N'. |
| unsubscribe_timestamp | TIMESTAMP | Yes | When unsubscribed. | Populated if unsubscribed. |
| from_address | VARCHAR(200) | Yes | Sender address/number. | Email from or phone number. |
| to_address | VARCHAR(200) | No | Recipient address/number. | Email to, phone, or postal address. |
| cc_addresses | VARCHAR(500) | Yes | CC recipients. | Comma-separated for email. |
| bcc_addresses | VARCHAR(500) | Yes | BCC recipients. | Comma-separated for email. |
| attachment_count | INTEGER | No | Number of attachments. | Default: 0. |
| has_attachment_flag | CHAR(1) | No | Has attachments. | Valid values: 'Y', 'N'. |
| priority_flag | CHAR(1) | No | High priority communication. | Valid values: 'Y', 'N'. Default: 'N'. |
| requires_response_flag | CHAR(1) | No | Response expected. | Valid values: 'Y', 'N'. Default: 'N'. |
| response_due_date | DATE | Yes | When response is due. | Required if response expected. |
| regulatory_required_flag | CHAR(1) | No | Regulatory requirement. | Valid values: 'Y', 'N'. |
| batch_id | VARCHAR(36) | Yes | Batch job that sent this. | For bulk communications. |
| external_message_id | VARCHAR(200) | Yes | External system message ID. | From email service, SMS gateway. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### COMMUNICATION_TYPE Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| communication_type_code | VARCHAR(20) | No | Unique code for the type. | Primary Key. Uppercase, underscore-separated. |
| communication_type_name | VARCHAR(100) | No | Display name for the type. | Human-readable name. |
| communication_type_description | VARCHAR(500) | Yes | Detailed description. | Explains type and usage. |
| communication_category_code | VARCHAR(20) | No | Category grouping. | Valid values: 'TRANSACTIONAL', 'MARKETING', 'REGULATORY', 'SERVICE', 'OPERATIONAL'. |
| is_marketing_flag | CHAR(1) | No | Is marketing communication. | Valid values: 'Y', 'N'. Affects opt-out handling. |
| is_regulatory_flag | CHAR(1) | No | Is regulatory required. | Valid values: 'Y', 'N'. Cannot be opted out. |
| requires_delivery_proof_flag | CHAR(1) | No | Requires delivery confirmation. | Valid values: 'Y', 'N'. |
| default_channel_code | VARCHAR(20) | Yes | Preferred delivery channel. | Foreign Key to COMMUNICATION_CHANNEL. |
| allowed_channels | VARCHAR(200) | Yes | Channels that can be used. | Comma-separated channel codes. |
| retention_days | INTEGER | Yes | Days to retain communication. | For data retention policy. |
| default_template_id | VARCHAR(36) | Yes | Default template to use. | Foreign Key to COMMUNICATION_TEMPLATE. |
| requires_opt_in_flag | CHAR(1) | No | Requires explicit opt-in. | Valid values: 'Y', 'N'. |
| honor_do_not_contact_flag | CHAR(1) | No | Honor DNC preferences. | Valid values: 'Y', 'N'. N for regulatory. |
| display_sequence | INTEGER | Yes | Order for display in UI. | Lower numbers appear first. |
| active_flag | CHAR(1) | No | Type is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| effective_date | DATE | No | Date type became available. | Reference data versioning. |
| expiration_date | DATE | Yes | Date type was retired. | Null = currently active. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

**Standard Communication Type Values:**

| Code | Name | Category | Marketing | Regulatory |
|------|------|----------|-----------|------------|
| POLICY_NOTICE | Policy Notice | REGULATORY | N | Y |
| BILLING_STATEMENT | Billing Statement | TRANSACTIONAL | N | N |
| PREMIUM_DUE | Premium Due Notice | TRANSACTIONAL | N | Y |
| LAPSE_WARNING | Lapse Warning | REGULATORY | N | Y |
| CLAIM_STATUS | Claim Status Update | TRANSACTIONAL | N | N |
| WELCOME | Welcome Communication | TRANSACTIONAL | N | N |
| RENEWAL_NOTICE | Renewal Notice | TRANSACTIONAL | N | Y |
| MARKETING | Marketing Communication | MARKETING | Y | N |
| SURVEY | Survey Invitation | MARKETING | Y | N |
| SERVICE_UPDATE | Service Request Update | SERVICE | N | N |
| ANNUAL_STATEMENT | Annual Statement | REGULATORY | N | Y |
| PRIVACY_NOTICE | Privacy Notice | REGULATORY | N | Y |

---

### COMMUNICATION_PREFERENCE Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| preference_id | VARCHAR(36) | No | Unique identifier for the preference. | Primary Key. UUID format recommended. |
| customer_id | VARCHAR(36) | No | Customer who set preference. | Foreign Key to CUSTOMER. |
| party_id | VARCHAR(36) | Yes | Party if not yet customer. | Foreign Key to PARTY. |
| preference_type_code | VARCHAR(20) | No | Type of preference. | Valid values: 'CHANNEL', 'FREQUENCY', 'CONTENT', 'OPT_OUT'. |
| communication_type_code | VARCHAR(20) | Yes | Specific communication type. | Foreign Key to COMMUNICATION_TYPE. Null = all types. |
| channel_code | VARCHAR(20) | Yes | Preferred/opted-out channel. | Foreign Key to COMMUNICATION_CHANNEL. |
| preference_value | VARCHAR(100) | No | The preference setting. | Depends on preference type. |
| opt_in_flag | CHAR(1) | No | Opted in (Y) or out (N). | Valid values: 'Y', 'N'. |
| opt_in_source_code | VARCHAR(50) | Yes | How opt-in was captured. | Valid values: 'WEB', 'PHONE', 'PAPER', 'APP', 'EMAIL'. |
| opt_in_timestamp | TIMESTAMP | Yes | When opted in. | Required if opted in. |
| opt_out_source_code | VARCHAR(50) | Yes | How opt-out was captured. | Valid values: 'WEB', 'PHONE', 'PAPER', 'APP', 'EMAIL', 'UNSUBSCRIBE_LINK'. |
| opt_out_timestamp | TIMESTAMP | Yes | When opted out. | Required if opted out. |
| opt_out_reason_code | VARCHAR(50) | Yes | Reason for opting out. | Standardized reason codes. |
| frequency_limit_code | VARCHAR(20) | Yes | Frequency limit preference. | Valid values: 'DAILY', 'WEEKLY', 'MONTHLY', 'QUARTERLY'. |
| max_communications_per_period | INTEGER | Yes | Maximum allowed per period. | Combined with frequency limit. |
| preferred_day_of_week | VARCHAR(20) | Yes | Preferred day for communications. | Valid values: 'MONDAY' through 'SUNDAY'. |
| preferred_time_of_day | VARCHAR(20) | Yes | Preferred time window. | Valid values: 'MORNING', 'AFTERNOON', 'EVENING'. |
| language_preference_code | VARCHAR(10) | Yes | Preferred language. | ISO 639-1 language code. |
| format_preference_code | VARCHAR(20) | Yes | Preferred format. | Valid values: 'HTML', 'PLAIN_TEXT', 'LARGE_PRINT'. |
| paperless_flag | CHAR(1) | Yes | Prefers paperless delivery. | Valid values: 'Y', 'N'. |
| verified_flag | CHAR(1) | No | Preference has been verified. | Valid values: 'Y', 'N'. Default: 'N'. |
| verified_timestamp | TIMESTAMP | Yes | When preference was verified. | Required if verified. |
| effective_date | DATE | No | When preference takes effect. | Start of preference validity. |
| expiration_date | DATE | Yes | When preference expires. | End of preference validity. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### COMPLAINT Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| complaint_id | VARCHAR(36) | No | Unique identifier for the complaint. | Primary Key. UUID format recommended. |
| complaint_number | VARCHAR(20) | No | Business-friendly complaint number. | Unique. System-generated. |
| customer_id | VARCHAR(36) | No | Customer who filed complaint. | Foreign Key to CUSTOMER. |
| complainant_party_id | VARCHAR(36) | Yes | Party if not the customer. | Foreign Key to PARTY. For third-party complaints. |
| complaint_category_code | VARCHAR(20) | No | Category of complaint. | Foreign Key to COMPLAINT_CATEGORY. |
| complaint_status_code | VARCHAR(20) | No | Current status. | Foreign Key to COMPLAINT_STATUS. |
| priority_code | VARCHAR(20) | No | Priority level. | Valid values: 'CRITICAL', 'HIGH', 'MEDIUM', 'LOW'. |
| source_channel_code | VARCHAR(20) | No | Channel where complaint received. | Foreign Key to COMMUNICATION_CHANNEL. |
| source_interaction_id | VARCHAR(36) | Yes | Originating interaction. | Foreign Key to CUSTOMER_INTERACTION. |
| service_request_id | VARCHAR(36) | Yes | Related service request. | Foreign Key to SERVICE_REQUEST. |
| policy_id | VARCHAR(36) | Yes | Related policy. | Foreign Key to POLICY (Policy Domain). |
| claim_id | VARCHAR(36) | Yes | Related claim. | Foreign Key to CLAIM (Claims Domain). |
| complaint_subject | VARCHAR(200) | No | Brief subject of complaint. | Summary for display. |
| complaint_description | VARCHAR(4000) | No | Detailed complaint description. | Full details from customer. |
| customer_expectation | VARCHAR(2000) | Yes | What customer expects as resolution. | Desired outcome. |
| received_timestamp | TIMESTAMP | No | When complaint was received. | System-populated. |
| acknowledged_timestamp | TIMESTAMP | Yes | When complaint was acknowledged. | First response time. |
| target_resolution_date | DATE | No | Target date for resolution. | Based on category and regulatory requirements. |
| actual_resolution_timestamp | TIMESTAMP | Yes | When complaint was resolved. | Resolution time tracking. |
| closed_timestamp | TIMESTAMP | Yes | When complaint was closed. | Final closure time. |
| assigned_agent_id | VARCHAR(36) | Yes | Agent handling complaint. | Foreign Key to SERVICE_AGENT. |
| escalation_level | INTEGER | No | Current escalation level. | 0=None, 1=Supervisor, 2=Manager, 3=Executive. Default: 0. |
| escalated_timestamp | TIMESTAMP | Yes | When last escalated. | Escalation tracking. |
| escalation_reason | VARCHAR(500) | Yes | Reason for escalation. | Required when escalated. |
| regulatory_flag | CHAR(1) | No | Has regulatory involvement. | Valid values: 'Y', 'N'. Default: 'N'. |
| regulatory_body_code | VARCHAR(20) | Yes | Which regulatory body involved. | Required if regulatory. |
| regulatory_case_number | VARCHAR(50) | Yes | Regulator's case number. | External reference. |
| regulatory_response_due_date | DATE | Yes | Response due to regulator. | Required if regulatory. |
| root_cause_code | VARCHAR(50) | Yes | Root cause category. | Standardized root cause codes. |
| root_cause_description | VARCHAR(1000) | Yes | Root cause explanation. | Detailed analysis. |
| resolution_code | VARCHAR(50) | Yes | How complaint was resolved. | Standardized resolution codes. |
| resolution_description | VARCHAR(2000) | Yes | Description of resolution. | Details of resolution provided. |
| compensation_provided_flag | CHAR(1) | No | Compensation was provided. | Valid values: 'Y', 'N'. Default: 'N'. |
| compensation_amount | DECIMAL(15,2) | Yes | Monetary compensation amount. | Required if compensation provided. |
| compensation_type_code | VARCHAR(20) | Yes | Type of compensation. | Valid values: 'REFUND', 'CREDIT', 'WAIVER', 'PAYMENT', 'OTHER'. |
| customer_satisfied_flag | CHAR(1) | Yes | Customer satisfied with resolution. | Valid values: 'Y', 'N', 'U' (unknown). |
| follow_up_survey_sent_flag | CHAR(1) | No | Follow-up survey sent. | Valid values: 'Y', 'N'. Default: 'N'. |
| reopen_count | INTEGER | No | Times complaint was reopened. | Default: 0. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

---

### COMPLAINT_CATEGORY Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| complaint_category_code | VARCHAR(20) | No | Unique code for the category. | Primary Key. Uppercase, underscore-separated. |
| complaint_category_name | VARCHAR(100) | No | Display name for the category. | Human-readable name. |
| complaint_category_description | VARCHAR(500) | Yes | Detailed description. | Explains category scope. |
| parent_category_code | VARCHAR(20) | Yes | Parent category for hierarchy. | Foreign Key to COMPLAINT_CATEGORY. |
| severity_level | INTEGER | No | Default severity (1-5). | 1=Lowest, 5=Highest. |
| default_priority_code | VARCHAR(20) | No | Default priority for category. | Valid values: 'CRITICAL', 'HIGH', 'MEDIUM', 'LOW'. |
| regulatory_reportable_flag | CHAR(1) | No | Must be reported to regulator. | Valid values: 'Y', 'N'. |
| target_resolution_days | INTEGER | No | Target days to resolve. | Standard resolution timeframe. |
| escalation_threshold_hours | INTEGER | Yes | Hours before auto-escalation. | Null = no auto-escalation. |
| requires_root_cause_flag | CHAR(1) | No | Root cause analysis required. | Valid values: 'Y', 'N'. |
| requires_corrective_action_flag | CHAR(1) | No | Corrective action required. | Valid values: 'Y', 'N'. |
| responsible_department_code | VARCHAR(20) | Yes | Department responsible. | For routing and reporting. |
| display_sequence | INTEGER | Yes | Order for display in UI. | Lower numbers appear first. |
| active_flag | CHAR(1) | No | Category is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| effective_date | DATE | No | Date category became available. | Reference data versioning. |
| expiration_date | DATE | Yes | Date category was retired. | Null = currently active. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

**Standard Complaint Category Values:**

| Code | Name | Severity | Resolution Days | Regulatory Reportable |
|------|------|----------|-----------------|----------------------|
| SERVICE_QUALITY | Service Quality | 2 | 10 | N |
| CLAIMS_HANDLING | Claims Handling | 4 | 15 | Y |
| CLAIMS_DELAY | Claims Delay | 4 | 10 | Y |
| CLAIMS_DENIAL | Claims Denial | 5 | 15 | Y |
| BILLING | Billing Issues | 3 | 10 | N |
| BILLING_ERROR | Billing Error | 3 | 5 | N |
| POLICY_TERMS | Policy Terms | 4 | 15 | Y |
| SALES_PRACTICES | Sales Practices | 5 | 15 | Y |
| MISREPRESENTATION | Misrepresentation | 5 | 15 | Y |
| COMMUNICATION | Communication Issues | 2 | 10 | N |
| PRIVACY | Privacy Concerns | 4 | 10 | Y |
| DISCRIMINATION | Discrimination | 5 | 15 | Y |
| AGENT_CONDUCT | Agent Conduct | 4 | 15 | Y |

---

### COMPLAINT_STATUS Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| complaint_status_code | VARCHAR(20) | No | Unique code for the status. | Primary Key. Uppercase, underscore-separated. |
| complaint_status_name | VARCHAR(100) | No | Display name for the status. | Human-readable name. |
| complaint_status_description | VARCHAR(500) | Yes | Detailed description. | Explains status meaning. |
| status_category_code | VARCHAR(20) | No | Category of status. | Valid values: 'OPEN', 'IN_WORK', 'PENDING', 'CLOSED'. |
| is_initial_status_flag | CHAR(1) | No | Can be initial status. | Valid values: 'Y', 'N'. |
| is_terminal_status_flag | CHAR(1) | No | Is a final/closed status. | Valid values: 'Y', 'N'. |
| stops_sla_clock_flag | CHAR(1) | No | Stops SLA clock. | Valid values: 'Y', 'N'. |
| requires_customer_response_flag | CHAR(1) | No | Waiting on customer. | Valid values: 'Y', 'N'. |
| requires_regulatory_report_flag | CHAR(1) | No | Requires regulatory reporting. | Valid values: 'Y', 'N'. |
| allowed_next_statuses | VARCHAR(200) | Yes | Valid next status transitions. | Comma-separated status codes. |
| auto_escalate_after_hours | INTEGER | Yes | Hours before auto-escalation. | Null = no auto-escalation. |
| send_notification_flag | CHAR(1) | No | Send notification on change. | Valid values: 'Y', 'N'. |
| notification_template_id | VARCHAR(36) | Yes | Template for notification. | Required if send notification. |
| display_color_code | VARCHAR(10) | Yes | Color for UI display. | Hex color code. |
| display_sequence | INTEGER | Yes | Order for display in UI. | Lower numbers appear first. |
| active_flag | CHAR(1) | No | Status is currently active. | Valid values: 'Y', 'N'. Default: 'Y'. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

**Standard Complaint Status Values:**

| Code | Name | Category | Initial | Terminal | Stops SLA |
|------|------|----------|---------|----------|-----------|
| RECEIVED | Received | OPEN | Y | N | N |
| ACKNOWLEDGED | Acknowledged | OPEN | N | N | N |
| INVESTIGATING | Under Investigation | IN_WORK | N | N | N |
| PENDING_RESPONSE | Pending Customer Response | PENDING | N | N | Y |
| PENDING_INFO | Pending Information | PENDING | N | N | Y |
| RESOLVED | Resolved | CLOSED | N | N | N |
| CLOSED | Closed | CLOSED | N | Y | Y |
| CLOSED_DUPLICATE | Closed - Duplicate | CLOSED | N | Y | Y |
| ESCALATED | Escalated | IN_WORK | N | N | N |
| REGULATORY_REVIEW | Regulatory Review | IN_WORK | N | N | N |

---

### SERVICE_AGENT Attributes

| Attribute Name | Data Type | Nullable | Description | Business Rules |
|----------------|-----------|----------|-------------|----------------|
| service_agent_id | VARCHAR(36) | No | Unique identifier for the agent. | Primary Key. UUID format recommended. |
| party_id | VARCHAR(36) | No | Link to party record. | Foreign Key to PARTY. |
| employee_id | VARCHAR(20) | No | Employee ID number. | Unique. HR system reference. |
| agent_type_code | VARCHAR(20) | No | Type of service agent. | Valid values: 'INTERNAL', 'CONTRACTOR', 'OUTSOURCED', 'VIRTUAL'. |
| agent_status_code | VARCHAR(20) | No | Current agent status. | Valid values: 'ACTIVE', 'INACTIVE', 'ON_LEAVE', 'TERMINATED', 'TRAINING'. |
| display_name | VARCHAR(100) | No | Name displayed to customers. | May differ from legal name. |
| email_address | VARCHAR(200) | No | Agent's work email. | For notifications and routing. |
| phone_extension | VARCHAR(20) | Yes | Phone extension number. | For phone routing. |
| hire_date | DATE | No | Date agent was hired. | Employment start date. |
| termination_date | DATE | Yes | Date agent was terminated. | Populated when terminated. |
| supervisor_agent_id | VARCHAR(36) | Yes | Supervising agent. | Foreign Key to SERVICE_AGENT. |
| team_code | VARCHAR(20) | Yes | Team assignment. | For reporting and routing. |
| department_code | VARCHAR(20) | Yes | Department assignment. | For reporting. |
| location_code | VARCHAR(20) | Yes | Work location. | For reporting and time zones. |
| time_zone_code | VARCHAR(50) | Yes | Agent's time zone. | IANA time zone. |
| default_queue_id | VARCHAR(36) | Yes | Primary queue assignment. | Foreign Key to SERVICE_QUEUE. |
| max_concurrent_chats | INTEGER | Yes | Maximum simultaneous chats. | For chat routing. |
| max_concurrent_cases | INTEGER | Yes | Maximum open cases. | For workload management. |
| handles_phone_flag | CHAR(1) | No | Handles phone calls. | Valid values: 'Y', 'N'. |
| handles_chat_flag | CHAR(1) | No | Handles chat. | Valid values: 'Y', 'N'. |
| handles_email_flag | CHAR(1) | No | Handles email. | Valid values: 'Y', 'N'. |
| handles_complaints_flag | CHAR(1) | No | Handles complaints. | Valid values: 'Y', 'N'. |
| handles_escalations_flag | CHAR(1) | No | Can receive escalations. | Valid values: 'Y', 'N'. |
| licensed_flag | CHAR(1) | No | Has insurance license. | Valid values: 'Y', 'N'. |
| license_number | VARCHAR(50) | Yes | Insurance license number. | Required if licensed. |
| license_state_codes | VARCHAR(200) | Yes | States where licensed. | Comma-separated state codes. |
| certification_codes | VARCHAR(200) | Yes | Professional certifications. | Comma-separated codes. |
| languages_supported | VARCHAR(200) | Yes | Languages agent speaks. | Comma-separated ISO codes. |
| quality_score | DECIMAL(5,2) | Yes | Current quality score. | 0.00-100.00. Updated periodically. |
| csat_score | DECIMAL(5,2) | Yes | Customer satisfaction score. | 0.00-100.00. Updated periodically. |
| average_handle_time_seconds | INTEGER | Yes | Average handle time. | Updated periodically. |
| first_contact_resolution_rate | DECIMAL(5,2) | Yes | FCR percentage. | 0.00-100.00. Updated periodically. |
| availability_status_code | VARCHAR(20) | Yes | Current availability. | Valid values: 'AVAILABLE', 'BUSY', 'AWAY', 'OFFLINE', 'DO_NOT_DISTURB'. |
| availability_updated_timestamp | TIMESTAMP | Yes | When availability changed. | Real-time tracking. |
| created_by | VARCHAR(50) | No | User or system that created the record. | System-populated at insert. |
| created_timestamp | TIMESTAMP | No | Date and time record was created. | System-populated at insert. |
| updated_by | VARCHAR(50) | No | User or system that last updated the record. | System-populated at update. |
| updated_timestamp | TIMESTAMP | No | Date and time record was last updated. | System-populated at update. |

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

| Entity | Type | Phase | Description |
|--------|------|-------|-------------|
| Customer Interaction | Transactional | P2 | Customer engagement event |
| Interaction Type | Reference | P2 | Interaction classification |
| Communication Channel | Reference | P2 | Engagement medium |
| Service Request | Transactional | P2 | Service/action request |
| Service Request Type | Reference | P2 | Request classification |
| Service Request Status | Reference | P2 | Request state |
| Customer Communication | Transactional | P2 | Outbound message |
| Communication Type | Reference | P2 | Message classification |
| Communication Preference | Master | P2 | Channel/frequency preferences |
| Communication Template | Reference | P3 | Message template |
| Customer Feedback | Transactional | P3 | Customer input |
| Feedback Type | Reference | P3 | Feedback classification |
| Customer Survey | Transactional | P3 | Feedback questionnaire |
| Survey Response | Transactional | P3 | Completed survey |
| Satisfaction Score | Transactional | P3 | NPS/CSAT metrics |
| Score Type | Reference | P3 | Metric classification |
| Complaint | Transactional | P2 | Formal dissatisfaction |
| Complaint Category | Reference | P2 | Complaint classification |
| Complaint Status | Reference | P2 | Complaint state |
| Customer Journey | Transactional | P4 | Experience sequence |
| Journey Type | Reference | P4 | Journey classification |
| Touchpoint | Transactional | P3 | Journey interaction point |
| Case | Transactional | P3 | Complex issue wrapper |
| Case Status | Reference | P3 | Case state |
| Service Agent | Master | P2 | Customer service employee |
| Agent Skill | Reference | P3 | Agent capability |
| Service Queue | Reference | P3 | Work routing group |
| Service Level Agreement | Reference | P3 | Performance target |
| SLA Compliance | Transactional | P3 | SLA performance record |
| Customer Sentiment | Transactional | P4 | Emotional assessment |
| Engagement Score | Transactional | P4 | Engagement metric |

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
| 1.1 | December 6, 2025 | Initial | Added P2 entity attribute definitions (13 entities) |

---

*Next Phase: Attribute definition for P3/P4 entities, then Relationship definition*
