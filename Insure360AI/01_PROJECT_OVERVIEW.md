# 01 - Project Overview

## Snowflake CoCo CLI Hackathon

### Insure360 AI

---

## Challenge Statement

Insurance companies have customer information fragmented across policies, claims, payments, interactions, call transcripts, emails and service requests. This makes it difficult for agents and business teams to understand the complete customer context and decide what to do next.

---

## Solution Proposed

Build an AI agent that creates a unified Customer 360 profile, understands both structured and unstructured customer interactions, identifies risks/opportunities, and recommends the Next Best Action (NBA).
---

## Ontology: Entity Relationship Model

```
                 ┌─────────────────────┐
                 │   Customer Sources  │
                 └──────────┬──────────┘
                            │
        ┌───────────────────┼───────────────────┐
        ▼                   ▼                   ▼
   Policy Data         Claims Data       Interaction Data
   Premium Data        Payment Data      Call Transcripts
        │                   │                   │
        └───────────────────┼───────────────────┘
                            ▼
                  ┌───────────────────┐
                  │     Snowflake     │
                  │ Customer 360 Data │
                  └─────────┬─────────┘
                            │
                 ┌──────────┴──────────┐
                 │ AI / Semantic Layer │
                 └──────────┬──────────┘
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
       Customer AI     Risk Analysis    RAG / Search
       Profiling       & Prediction     Unstructured
             │              │              │
             └──────────────┼──────────────┘
                            ▼
                 ┌─────────────────────┐
                 │  INSURE360 AGENT    │
                 └──────────┬──────────┘
                            ▼
                  ┌───────────────────┐
                  │ Next Best Action  │
                  └───────────────────┘
```

## Solution Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  CONVERSATIONAL LAYER                   │
│            (Cortex Analyst + Cortex Agents)             │
├─────────────────────────────────────────────────────────┤
│                   SEMANTIC VIEWS                        │
│         (Governed business definitions & metrics)       |
├─────────────────────────────────────────────────────────┤
│                   GOLD SCHEMA                           │
│       (Star schema - Facts & Dimensions)                │
├─────────────────────────────────────────────────────────┤
│                   RAW SCHEMA                            │
│          (Source-aligned ingested data)                 │
├─────────────────────────────────────────────────────────┤
│              SOURCE SYSTEMS                             │
│Customer|Policies|Claims|Complaints|Interactions|Sentiment
└─────────────────────────────────────────────────────────┘
```
# Insure360 AI — Customer 360 & Next Best Action

## Core Entities

### 1. CUSTOMER

The master customer entity representing the policyholder/customer.

**Key attributes:**

* CUSTOMER_ID
* CUSTOMER_NAME
* DATE_OF_BIRTH
* CITY
* REGION
* JOIN_DATE
* CUSTOMER_SEGMENT
* EMAIL
* PHONE

### 2. POLICY

Represents insurance policies owned by a customer.

**Key attributes:**

* POLICY_ID
* CUSTOMER_ID
* POLICY_TYPE
* POLICY_START_DATE
* POLICY_END_DATE
* PREMIUM_AMOUNT
* COVERAGE_AMOUNT
* POLICY_STATUS
* PAYMENT_FREQUENCY

**Example policy types:**

* Motor
* Health
* Life
* Home
* Travel

### 3. CLAIM

Represents claims raised against customer policies.

**Key attributes:**

* CLAIM_ID
* POLICY_ID
* CUSTOMER_ID
* CLAIM_DATE
* CLAIM_TYPE
* CLAIM_AMOUNT
* CLAIM_STATUS
* SETTLEMENT_AMOUNT
* SETTLEMENT_DATE
* CLAIM_DESCRIPTION

### 4. PAYMENT

Represents premium payment behavior.

**Key attributes:**

* PAYMENT_ID
* CUSTOMER_ID
* POLICY_ID
* DUE_DATE
* PAYMENT_DATE
* AMOUNT
* PAYMENT_STATUS
* PAYMENT_METHOD

### 5. CUSTOMER_INTERACTION

Represents customer touchpoints across calls, emails, chat and web.

**Key attributes:**

* INTERACTION_ID
* CUSTOMER_ID
* INTERACTION_DATE
* CHANNEL
* INTERACTION_TYPE
* AGENT_ID
* CALL_DURATION
* TRANSCRIPT_FILE

### 6. COMPLAINT

Represents formal customer complaints and service issues.

**Key attributes:**

* COMPLAINT_ID
* CUSTOMER_ID
* RELATED_CLAIM_ID
* COMPLAINT_DATE
* CATEGORY
* STATUS
* PRIORITY
* RESOLUTION_DATE

### 7. INTERACTION_INSIGHT

AI-generated intelligence extracted from unstructured interactions.

**Key attributes:**

* INTERACTION_ID
* CUSTOMER_ID
* SENTIMENT
* SENTIMENT_SCORE
* INTENT
* URGENCY
* CANCELLATION_INTENT
* SUMMARY

This entity connects the unstructured customer conversation with the structured Customer 360 model.

### 8. CUSTOMER_360

A consolidated analytical view of the customer.

**Key attributes:**

* CUSTOMER_ID
* CUSTOMER_NAME
* ACTIVE_POLICIES
* TOTAL_PREMIUM
* TOTAL_CLAIMS
* OPEN_CLAIMS
* TOTAL_CLAIM_AMOUNT
* LATE_PAYMENTS
* TOTAL_COMPLAINTS
* OPEN_COMPLAINTS
* RECENT_INTERACTIONS
* LAST_SENTIMENT
* DAYS_TO_RENEWAL
* CUSTOMER_TENURE
* CUSTOMER_VALUE
* RETENTION_RISK
* CROSS_SELL_ELIGIBLE
* NEXT_BEST_ACTION

### 9. NEXT_BEST_ACTION

Represents the recommended action for a customer based on their current situation.

**Possible actions:**

* CLAIM_ESCALATION
* SERVICE_RECOVERY
* RETENTION_CALL
* RENEWAL_OFFER
* PAYMENT_REMINDER
* CROSS_SELL
* UPSELL
* LOYALTY_REWARD
* NO_ACTION

---

## Key Relationships

```text
CUSTOMER
   │
   ├───────────────< POLICY
   │                    │
   │                    └───────────────< CLAIM
   │
   ├───────────────< PAYMENT
   │
   ├───────────────< CUSTOMER_INTERACTION
   │                                      │
   │                                      └── INTERACTION_INSIGHT
   │
   └───────────────< COMPLAINT
                          │
                          └── RELATED_CLAIM
```

### Primary relationships

**Customer → Policy**

* One customer can own multiple policies.
* `CUSTOMER.CUSTOMER_ID → POLICY.CUSTOMER_ID`

**Policy → Claim**

* One policy can have multiple claims.
* `POLICY.POLICY_ID → CLAIM.POLICY_ID`

**Customer → Payment**

* One customer can have multiple premium payments.
* `CUSTOMER.CUSTOMER_ID → PAYMENT.CUSTOMER_ID`

**Customer → Interaction**

* One customer can have multiple interactions.
* `CUSTOMER.CUSTOMER_ID → CUSTOMER_INTERACTION.CUSTOMER_ID`

**Interaction → AI Insight**

* Each interaction can generate AI-derived sentiment, intent and risk signals.
* `CUSTOMER_INTERACTION.INTERACTION_ID → INTERACTION_INSIGHT.INTERACTION_ID`

**Customer → Complaint**

* One customer can have multiple complaints.
* `CUSTOMER.CUSTOMER_ID → COMPLAINT.CUSTOMER_ID`

**Complaint → Claim**

* A complaint may be associated with a particular claim.
* `COMPLAINT.RELATED_CLAIM_ID → CLAIM.CLAIM_ID`

**Customer → Next Best Action**

* Each customer receives a current recommended action based on their 360 profile.

---

## Canonical Metrics

The important principle is that business metrics should be calculated consistently in Snowflake rather than independently by the AI.

### Customer Value

```text
Customer Value =
Total Premium Paid
+ Cross-Sell Potential
- Claims Cost
```

### Customer Tenure

```text
Customer Tenure =
Current Date - Customer Join Date
```

### Claims Frequency

```text
Claims Frequency =
Number of Claims / Years as Customer
```

### Open Claim Ratio

```text
Open Claim Ratio =
Open Claims / Total Claims
```

### Complaint Rate

```text
Complaint Rate =
Number of Complaints / Customer Tenure
```

### Payment Reliability

```text
Payment Reliability =
On-Time Payments / Total Payments × 100
```

### Renewal Proximity

```text
Days to Renewal =
Policy End Date - Current Date
```

### Retention Risk Score

An explainable score combining customer behavior and interaction signals.

Example:

```text
Open Complaint                 +20
Negative Sentiment             +25
Cancellation Intent            +30
Repeated Customer Contacts     +10
Long-Pending Claim             +20
Upcoming Renewal               +10
Late Payment                   +05
```

Risk classification:

```text
0–29      LOW
30–59     MEDIUM
60+       HIGH
```

### Next Best Action

The recommended action is derived from customer state.

Examples:

```text
Cancellation Intent + Open Claim
→ CLAIM_ESCALATION

Negative Sentiment + Open Complaint
→ SERVICE_RECOVERY

High Risk + Upcoming Renewal
→ RETENTION_CALL

Low Risk + Upcoming Renewal
→ RENEWAL_OFFER

Positive Sentiment + Single Product + High Value
→ CROSS_SELL

Overdue Premium
→ PAYMENT_REMINDER
```

The key differentiator is that the recommendation is **explainable** rather than an opaque AI prediction.

---

## Personas Served

### 1. Customer Service Agent

**Primary questions:**

* Why is this customer unhappy?
* What issue should I resolve first?
* What should I do next?

**Primary capability:**
Customer 360 + Service Recovery.

---

### 2. Retention / CRM Manager

**Primary questions:**

* Which customers are likely to churn?
* Who should we contact today?
* Why is this customer at risk?

**Primary capability:**
Retention Risk + Next Best Action.

---

### 3. Sales / Relationship Manager

**Primary questions:**

* Which customers are eligible for cross-selling?
* What product should I offer?
* Which high-value customers have only one product?

**Primary capability:**
Personalization + Cross-Sell / Upsell.

---

### 4. Claims Manager

**Primary questions:**

* Which customers have unresolved claims?
* Which claims are causing customer dissatisfaction?
* Which claims require priority intervention?

**Primary capability:**
Claims + Interaction Intelligence.

---

### 5. Business / Management

**Primary questions:**

* How many customers are high risk?
* What are the major reasons for customer dissatisfaction?
* What actions should the business prioritize?

**Primary capability:**
Enterprise Customer 360 + AI insights.

---

## Technology Stack

### Data Platform

**Snowflake**

* Database
* Schemas
* Tables
* Views
* Analytical queries
* Customer 360 data model

### AI / Intelligence

**Snowflake Cortex**

Use for:

* Sentiment analysis
* Call transcript summarization
* Intent extraction
* Cancellation-intent detection
* Natural-language interaction
* AI-generated explanations

### Conversational AI

**Cortex Analyst / Cortex Agent**

Use for:

* Natural-language questions
* Customer 360 queries
* Multi-table reasoning
* Business-user interaction
* Next Best Action explanation

### Development Assistant

**Snowflake CoCo CLI**

Use for:

* Data exploration
* SQL generation
* Data analysis
* AI-assisted development
* Agent/application development workflow

### Application

**Streamlit**

Recommended application experience:

```
INSURE360 AI
│
├── Executive Overview
│
├── Customer 360
│
├── Next Best Actions
│
└── AI Copilot
```

### Synthetic Data

Use fully synthetic customer information:

* Customers
* Policies
* Claims
* Payments
* Interactions
* Complaints
* Call transcripts

No real customer PII should be used.

---

## Final Solution Positioning

### **Insure360 AI**

**From Customer Signals → Customer Understanding → Next Best Action**

The solution combines:

```text
STRUCTURED DATA
Customer
Policy
Claims
Payments
Complaints

        +

UNSTRUCTURED DATA
Call Transcripts
Customer Conversations

        ↓

SNOWFLAKE + CORTEX AI

        ↓

CUSTOMER 360

        ↓

SENTIMENT + INTENT + RISK

        ↓

NEXT BEST ACTION

        ↓

AI COPILOT

        ↓

BUSINESS ACTION
```

## Database Structure

```
Insure360
├── RAW          -- Source-aligned tables (ingestion layer)
├── GOLD         -- Star schema (facts & dimensions)
└── SEMANTIC     -- Semantic views (business meaning layer)
```

---

## Project Deliverables

1. **Data Model** — Star schema with 13 dimensions and 13 fact tables
2. **Data Dictionary** — Full column-level documentation with relationships (`data_dictionary.md`)
3. **DDL Scripts** — Table creation scripts (`gold_tables_ddl.sql`)
4. **Semantic Views** — Governed semantic layer encoding ontology and metrics
5. **Cortex Agent** — Conversational interface for cross-domain analytics
6. **Demo** — Multi-persona queries proving metric consistency
