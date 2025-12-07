# Premium & Billing Domain
## Logical Data Model - Entity Definitions

**Domain:** Premium & Billing  
**Version:** 1.0  
**Date:** December 6, 2025  
**Status:** Phase 1 - Entity Identification  
**Phase:** Transaction & Event Domain

---

## Domain Overview

**Purpose:** Manage premium calculations, billing schedules, payment processing, and collections for life insurance policies.

**Scope:** This domain encompasses premium schedules, billing cycles, payment transactions, payment methods, collection activities, and premium-related policy events (lapse, grace period, reinstatement).

**Business Value:**
- Premium revenue tracking and forecasting
- Payment persistency analysis
- Collection effectiveness optimization
- Payment channel cost analysis
- Lapse prediction and prevention
- Cash flow management

---

## Entity Catalog

### PREMIUM_SCHEDULE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Premium Schedule |
| **Entity Definition** | The defined schedule of premium payments for a Policy, specifying the amount, frequency, and due dates. Premium schedules may change over the policy life due to rate changes or modifications. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Policy Number + Schedule Effective Date |
| **Key Relationships** | - Belongs to Policy (Policy Domain)<br>- Based on Rate Table (Product Domain)<br>- Generates Billing Items<br>- Has Payment Mode |
| **Assumptions** | - Schedule created at policy issue<br>- New schedule version on rate changes |
| **Open Questions** | - How are scheduled premium increases handled? |

---

### PAYMENT_MODE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payment Mode |
| **Entity Definition** | The frequency at which premium payments are due and collected. Payment mode affects the modal premium amount and may include mode fees. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Mode Code |
| **Key Relationships** | - Applied to Premium Schedule<br>- Determines billing frequency |
| **Assumptions** | - Modes: ANNUAL, SEMI_ANNUAL, QUARTERLY, MONTHLY<br>- Modal factors may apply to non-annual modes |
| **Open Questions** | - What modal factors are applied? |

---

### BILLING_CYCLE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Billing Cycle |
| **Entity Definition** | A recurring period during which billing items are generated and statements produced. Billing cycles organize premium collection activities. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Cycle Identifier |
| **Key Relationships** | - Contains Billing Items<br>- Generates Statements |
| **Assumptions** | - Cycles may be daily, weekly, or monthly<br>- Policies assigned to specific cycle dates |
| **Open Questions** | - How are billing cycle dates assigned to new policies? |

---

### BILLING_ITEM

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Billing Item |
| **Entity Definition** | An individual charge or credit on a policy account, representing a premium due, fee, adjustment, or other financial transaction. Billing items are the basis for statements and payments. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Billing Item Identifier |
| **Key Relationships** | - Generated for Policy<br>- Has Item Type<br>- Included in Statement<br>- Paid by Payment Transaction |
| **Assumptions** | - Items have due dates and aging<br>- Multiple items may be consolidated for payment |
| **Open Questions** | - None at this time |

---

### BILLING_ITEM_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Billing Item Type |
| **Entity Definition** | A classification of Billing Items based on the nature of the charge or credit. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Item Type Code |
| **Key Relationships** | - Classifies Billing Item |
| **Assumptions** | - Types: PREMIUM, RIDER_PREMIUM, POLICY_FEE, LATE_FEE, REINSTATEMENT_FEE, ADJUSTMENT, REFUND |
| **Open Questions** | - None at this time |

---

### STATEMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Statement |
| **Entity Definition** | A periodic summary of account activity and balances sent to the policyholder, showing charges, payments, and amount due. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Statement Identifier |
| **Key Relationships** | - Generated for Policy<br>- Contains Billing Items<br>- Shows payment due |
| **Assumptions** | - Statements generated per billing cycle<br>- May be delivered via multiple channels |
| **Open Questions** | - What statement delivery channels are supported? |

---

### PAYMENT_TRANSACTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payment Transaction |
| **Entity Definition** | A financial transaction representing the receipt of premium payment from a policyholder. Payments are applied to outstanding billing items on the policy account. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Transaction Identifier |
| **Key Relationships** | - Applied to Policy<br>- Pays Billing Items<br>- Uses Payment Method<br>- Received through Payment Channel<br>- Made by Payor (Policy Domain) |
| **Assumptions** | - Payments may be partial or overpayments<br>- Payment date vs. processed date tracked |
| **Open Questions** | - None at this time |

---

### PAYMENT_STATUS

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payment Status |
| **Entity Definition** | The current state of a Payment Transaction in the processing lifecycle. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Status Code |
| **Key Relationships** | - Assigned to Payment Transaction |
| **Assumptions** | - Statuses: PENDING, PROCESSED, POSTED, RETURNED, REVERSED, REFUNDED |
| **Open Questions** | - None at this time |

---

### PAYMENT_METHOD

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payment Method |
| **Entity Definition** | The instrument or mechanism used to make a payment, such as bank account, credit card, or check. Payment methods are registered to parties for recurring payments. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Method Identifier |
| **Key Relationships** | - Registered to Party (Party Domain)<br>- Used for Payment Transactions<br>- Has Method Type |
| **Assumptions** | - Methods are validated and stored securely<br>- Multiple methods may be registered |
| **Open Questions** | - What payment methods are supported? |

---

### PAYMENT_METHOD_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payment Method Type |
| **Entity Definition** | A classification of Payment Methods based on the payment instrument. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Method Type Code |
| **Key Relationships** | - Classifies Payment Method |
| **Assumptions** | - Types: BANK_ACCOUNT (ACH), CREDIT_CARD, DEBIT_CARD, CHECK, MONEY_ORDER |
| **Open Questions** | - None at this time |

---

### PAYMENT_CHANNEL

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payment Channel |
| **Entity Definition** | The channel through which a payment is received, such as online portal, mobile app, phone, or mail. Payment channels enable cost and preference analysis. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Channel Code |
| **Key Relationships** | - Used for Payment Transactions<br>- Links to Channel (Sales & Distribution Domain) |
| **Assumptions** | - Channels: WEB_PORTAL, MOBILE_APP, IVR, CALL_CENTER, MAIL, LOCKBOX |
| **Open Questions** | - None at this time |

---

### AUTOMATIC_PAYMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Automatic Payment |
| **Entity Definition** | An arrangement for premiums to be automatically withdrawn from a Payment Method on scheduled dates. Automatic payments improve persistency and reduce collection costs. |
| **Entity Type** | Master Data |
| **Primary Identifier(s)** | Auto-Pay Identifier |
| **Key Relationships** | - Configured for Policy<br>- Uses Payment Method<br>- Has Schedule<br>- Generates Payment Transactions |
| **Assumptions** | - Policyholders must authorize automatic payment<br>- Withdrawal dates align with billing dates |
| **Open Questions** | - What is the auto-pay enrollment rate target? |

---

### PAYMENT_ALLOCATION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payment Allocation |
| **Entity Definition** | The distribution of a Payment Transaction amount across outstanding Billing Items, documenting how the payment was applied to specific charges. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Allocation Identifier |
| **Key Relationships** | - Links Payment Transaction to Billing Items |
| **Assumptions** | - Allocation follows defined priority rules<br>- Overpayments create credits or are refunded |
| **Open Questions** | - What are the payment allocation priority rules? |

---

### PAYMENT_RETURN

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Payment Return |
| **Entity Definition** | A record of a Payment Transaction that was returned (e.g., NSF, closed account, invalid card). Returns may trigger follow-up collection actions. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Return Identifier |
| **Key Relationships** | - Associated with Payment Transaction<br>- Has Return Reason<br>- May trigger Collection Action |
| **Assumptions** | - Returns reverse original payment<br>- Return fees may be assessed |
| **Open Questions** | - What return fee policy applies? |

---

### RETURN_REASON

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Return Reason |
| **Entity Definition** | The reason code for a Payment Return, indicating why the payment failed. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Reason Code |
| **Key Relationships** | - Classifies Payment Return |
| **Assumptions** | - Reasons: NSF, CLOSED_ACCOUNT, INVALID_ACCOUNT, STOP_PAYMENT, EXPIRED_CARD, DECLINED |
| **Open Questions** | - None at this time |

---

### COLLECTION_ACTION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Collection Action |
| **Entity Definition** | An action taken to collect overdue premiums, such as reminder notices, phone calls, or payment plan offers. Collection actions are sequenced based on aging. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Action Identifier |
| **Key Relationships** | - Taken for Policy<br>- Has Action Type<br>- Has Outcome |
| **Assumptions** | - Actions follow defined collection workflow<br>- Actions tracked for effectiveness analysis |
| **Open Questions** | - What is the collection action sequence? |

---

### COLLECTION_ACTION_TYPE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Collection Action Type |
| **Entity Definition** | A classification of Collection Actions based on the method and intensity of the collection effort. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Action Type Code |
| **Key Relationships** | - Classifies Collection Action |
| **Assumptions** | - Types: REMINDER_EMAIL, REMINDER_SMS, REMINDER_CALL, WARNING_NOTICE, FINAL_NOTICE, PAYMENT_PLAN_OFFER |
| **Open Questions** | - None at this time |

---

### DELINQUENCY

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Delinquency |
| **Entity Definition** | A record of a Policy with overdue premium payments, tracking the duration and amount of delinquency. Delinquency status triggers collection and may lead to lapse. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Delinquency Start Date |
| **Key Relationships** | - Affects Policy<br>- Has Delinquency Stage<br>- Triggers Collection Actions<br>- May result in Grace Period or Lapse |
| **Assumptions** | - Delinquency begins on first missed due date<br>- Cured when all overdue amounts paid |
| **Open Questions** | - None at this time |

---

### DELINQUENCY_STAGE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Delinquency Stage |
| **Entity Definition** | The severity level of a Delinquency based on aging, determining the collection intensity and policy status impact. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Stage Code |
| **Key Relationships** | - Classifies Delinquency |
| **Assumptions** | - Stages: 1-30_DAYS, 31-60_DAYS, 61-90_DAYS, GRACE_PERIOD, LAPSE_PENDING |
| **Open Questions** | - None at this time |

---

### GRACE_PERIOD_BILLING

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Grace Period Billing |
| **Entity Definition** | A billing event representing a policy entering grace period due to non-payment. Grace period allows continued coverage while providing opportunity to pay. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Grace Period Identifier |
| **Key Relationships** | - Triggered for Policy<br>- Links to Grace Period (Policy Domain)<br>- Has duration and expiration date |
| **Assumptions** | - Grace period typically 30-31 days<br>- Coverage continues during grace period |
| **Open Questions** | - Does grace period vary by payment mode? |

---

### LAPSE_FOR_NONPAYMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Lapse for Non-Payment |
| **Entity Definition** | An event recording a policy lapse due to failure to pay premiums within the grace period. Lapse terminates coverage and may be reversible through reinstatement. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Lapse Event Identifier |
| **Key Relationships** | - Affects Policy (Policy Domain)<br>- Results from expired Grace Period<br>- May be reversed by Reinstatement |
| **Assumptions** | - Lapse is effective at grace period expiration<br>- Some policies may have automatic lapse protection |
| **Open Questions** | - What automatic lapse protection options exist? |

---

### REINSTATEMENT_PAYMENT

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Reinstatement Payment |
| **Entity Definition** | A payment transaction specifically for reinstating a lapsed policy, including any back premiums, fees, and interest required for reinstatement. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Payment Identifier |
| **Key Relationships** | - Associated with Reinstatement (Policy Domain)<br>- Includes back premiums and fees |
| **Assumptions** | - Full reinstatement amount required<br>- May be subject to underwriting approval |
| **Open Questions** | - How is reinstatement interest calculated? |

---

### REFUND

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Refund |
| **Entity Definition** | A payment made to a policyholder returning premium amounts, such as for cancellation, overpayment, or rate adjustment. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Refund Identifier |
| **Key Relationships** | - Issued for Policy<br>- Has Refund Reason<br>- Paid to Party (Party Domain) |
| **Assumptions** | - Refunds processed within defined timeframes<br>- Refund method may differ from payment method |
| **Open Questions** | - What is the refund processing SLA? |

---

### REFUND_REASON

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Refund Reason |
| **Entity Definition** | The reason for issuing a Refund to a policyholder. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Reason Code |
| **Key Relationships** | - Classifies Refund |
| **Assumptions** | - Reasons: FREE_LOOK_CANCELLATION, OVERPAYMENT, RATE_REDUCTION, POLICY_MODIFICATION, DUPLICATE_PAYMENT |
| **Open Questions** | - None at this time |

---

### PREMIUM_RATE_CHANGE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Premium Rate Change |
| **Entity Definition** | An event recording a change in premium rate for a policy, whether due to scheduled increases, rate class change, or policy modification. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Rate Change Identifier |
| **Key Relationships** | - Applied to Policy<br>- Creates new Premium Schedule<br>- Has Change Reason |
| **Assumptions** | - Rate changes require advance notice<br>- May be scheduled (age-based) or event-driven |
| **Open Questions** | - What advance notice is required for rate changes? |

---

### RATE_CHANGE_REASON

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Rate Change Reason |
| **Entity Definition** | The reason for a Premium Rate Change on a policy. |
| **Entity Type** | Reference Data |
| **Primary Identifier(s)** | Reason Code |
| **Key Relationships** | - Classifies Premium Rate Change |
| **Assumptions** | - Reasons: AGE_INCREASE, COVERAGE_CHANGE, RIDER_CHANGE, RATE_CLASS_CHANGE, RENEWAL |
| **Open Questions** | - None at this time |

---

### ACCOUNT_BALANCE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Account Balance |
| **Entity Definition** | The current financial position of a policy account, including amounts due, credits, and payment status. Balance is updated with each billing and payment transaction. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Policy Number + Balance Date |
| **Key Relationships** | - Calculated for Policy<br>- Reflects Billing Items and Payments |
| **Assumptions** | - Balance updated real-time or near-real-time<br>- Historical balances retained for analysis |
| **Open Questions** | - None at this time |

---

### PREMIUM_NOTICE

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Premium Notice |
| **Entity Definition** | A communication sent to a policyholder regarding premium due, providing payment amount, due date, and payment options. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Notice Identifier |
| **Key Relationships** | - Generated for Policy<br>- Related to Billing Item<br>- Delivered via Communication Channel |
| **Assumptions** | - Notices sent before due date<br>- Multiple notice types (due, reminder, final) |
| **Open Questions** | - How far in advance are notices sent? |

---

### COMMISSION

| Attribute | Value |
|-----------|-------|
| **Entity Name** | Commission |
| **Entity Definition** | Compensation paid to distribution partners or agents based on premium collected. For direct-to-consumer, commissions may apply to partner referrals or affiliates. |
| **Entity Type** | Transactional |
| **Primary Identifier(s)** | Commission Identifier |
| **Key Relationships** | - Calculated on Premium collected<br>- Paid to Partner (Sales & Distribution Domain)<br>- Based on Commission Structure (Product Domain) |
| **Assumptions** | - Commission rates vary by year and product<br>- Chargebacks may apply for early lapses |
| **Open Questions** | - What commission chargeback rules apply? |

---

## Conceptual Entity-Relationship Diagram

```
┌─────────────────┐                             ┌─────────────────┐
│     POLICY      │────────────────────────────►│ PREMIUM_SCHEDULE│
│ (Policy Domain) │                             │                 │
└─────────────────┘                             └────────┬────────┘
         │                                               │
         │                                               ▼
         │                                      ┌─────────────────┐
         │                                      │  PAYMENT_MODE   │
         │                                      └─────────────────┘
         │
         ▼
┌─────────────────┐        ┌─────────────────┐        ┌─────────────────┐
│  BILLING_ITEM   │───────►│  BILLING_ITEM   │        │  BILLING_CYCLE  │
│                 │        │     TYPE        │        │                 │
└────────┬────────┘        └─────────────────┘        └─────────────────┘
         │
         │◄──────────────────────────────────────────────────┐
         │                                                   │
         ▼                                                   │
┌─────────────────┐        ┌─────────────────┐        ┌──────┴──────────┐
│   STATEMENT     │        │ PAYMENT_        │◄───────│    PAYMENT      │
│                 │        │  ALLOCATION     │        │  TRANSACTION    │
└─────────────────┘        └─────────────────┘        └────────┬────────┘
                                                              │
                           ┌──────────────────────────────────┼───────────────┐
                           │                                  │               │
                           ▼                                  ▼               ▼
                   ┌─────────────────┐              ┌─────────────────┐ ┌─────────────────┐
                   │ PAYMENT_METHOD  │              │ PAYMENT_CHANNEL │ │ PAYMENT_STATUS  │
                   │                 │              │                 │ │                 │
                   └────────┬────────┘              └─────────────────┘ └─────────────────┘
                            │
                            ▼
                   ┌─────────────────┐
                   │ PAYMENT_METHOD  │
                   │     TYPE        │
                   └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│   AUTOMATIC     │───────►│ PAYMENT_METHOD  │
│    PAYMENT      │  uses  │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│ PAYMENT_RETURN  │───────►│  RETURN_REASON  │
│                 │        │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐        ┌─────────────────┐
│  DELINQUENCY    │───────►│  DELINQUENCY    │        │ COLLECTION      │
│                 │        │     STAGE       │        │    ACTION       │
└────────┬────────┘        └─────────────────┘        └────────┬────────┘
         │                                                     │
         │                                                     ▼
         │                                            ┌─────────────────┐
         │                                            │ COLLECTION      │
         │                                            │  ACTION_TYPE    │
         │                                            └─────────────────┘
         │
         ▼
┌─────────────────┐        ┌─────────────────┐        ┌─────────────────┐
│ GRACE_PERIOD    │───────►│  LAPSE_FOR      │───────►│  REINSTATEMENT  │
│    BILLING      │  leads │  NONPAYMENT     │  may   │    PAYMENT      │
└─────────────────┘  to    └─────────────────┘reverse └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│     REFUND      │───────►│  REFUND_REASON  │
│                 │        │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│ PREMIUM_RATE    │───────►│ RATE_CHANGE     │
│    CHANGE       │        │    REASON       │
└─────────────────┘        └─────────────────┘

┌─────────────────┐        ┌─────────────────┐
│ ACCOUNT_BALANCE │        │ PREMIUM_NOTICE  │
│                 │        │                 │
└─────────────────┘        └─────────────────┘

┌─────────────────┐
│   COMMISSION    │
│                 │
└─────────────────┘
```

---

## Cross-Domain Entity Mapping

| Entity | Related Domain | Related Entity | Relationship Description |
|--------|---------------|----------------|-------------------------|
| Premium Schedule | Policy | Policy | Schedule belongs to policy |
| Premium Schedule | Product | Rate Table | Rates from product rate table |
| Payment Transaction | Party/Customer | Payor | Payor makes payments |
| Payment Method | Party/Customer | Party | Payment method registered to party |
| Payment Channel | Sales & Distribution | Channel | Payment through channel |
| Lapse for Non-Payment | Policy | Policy Status | Lapse changes policy status |
| Grace Period Billing | Policy | Grace Period | Links to policy grace period |
| Reinstatement Payment | Policy | Reinstatement | Payment for reinstatement |
| Commission | Sales & Distribution | Partner | Commission paid to partners |
| Commission | Product | Commission Structure | Rates from product structure |
| Commission | Financial Performance | Expense | Commission is financial expense |
| Premium Rate Change | Product | Rate Table | New rates from rate table |
| Account Balance | Financial Performance | Revenue | Premium revenue recognition |

---

## Entity Summary

| Entity | Type | Description |
|--------|------|-------------|
| Premium Schedule | Master | Policy premium payment schedule |
| Payment Mode | Reference | Premium payment frequency |
| Billing Cycle | Reference | Billing period definition |
| Billing Item | Transactional | Individual charge/credit |
| Billing Item Type | Reference | Charge type classification |
| Statement | Transactional | Account summary document |
| Payment Transaction | Transactional | Premium payment received |
| Payment Status | Reference | Payment processing state |
| Payment Method | Master | Payment instrument |
| Payment Method Type | Reference | Instrument classification |
| Payment Channel | Reference | Payment receipt channel |
| Automatic Payment | Master | Auto-pay arrangement |
| Payment Allocation | Transactional | Payment-to-item application |
| Payment Return | Transactional | Failed payment record |
| Return Reason | Reference | Failure reason code |
| Collection Action | Transactional | Collection activity |
| Collection Action Type | Reference | Collection method type |
| Delinquency | Transactional | Overdue payment record |
| Delinquency Stage | Reference | Delinquency severity |
| Grace Period Billing | Transactional | Grace period event |
| Lapse for Non-Payment | Transactional | Lapse event |
| Reinstatement Payment | Transactional | Reinstatement payment |
| Refund | Transactional | Premium refund |
| Refund Reason | Reference | Refund classification |
| Premium Rate Change | Transactional | Rate change event |
| Rate Change Reason | Reference | Change reason code |
| Account Balance | Transactional | Policy account status |
| Premium Notice | Transactional | Payment due notice |
| Commission | Transactional | Distribution compensation |

**Total Entities: 29**

---

## Assumptions Log

1. Premium schedules created at policy issue
2. Modal factors may apply for non-annual payment modes
3. Payments may be partial or overpayments
4. Multiple payment methods may be registered per party
5. Collection actions follow defined workflow
6. Grace period typically 30-31 days
7. Refunds processed within defined SLAs
8. Commission rates vary by year and product
9. Account balance updated near-real-time

---

## Open Questions

1. How are scheduled premium increases handled?
2. What modal factors are applied for non-annual modes?
3. How are billing cycle dates assigned to new policies?
4. What statement delivery channels are supported?
5. What payment methods are supported?
6. What is the auto-pay enrollment rate target?
7. What are the payment allocation priority rules?
8. What return fee policy applies?
9. What is the collection action sequence?
10. Does grace period vary by payment mode?
11. What automatic lapse protection options exist?
12. How is reinstatement interest calculated?
13. What is the refund processing SLA?
14. What advance notice is required for rate changes?
15. How far in advance are premium notices sent?
16. What commission chargeback rules apply?

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | December 6, 2025 | Initial | Entity identification and definition phase |

---

*Next Phase: Attribute definition for all entities*
