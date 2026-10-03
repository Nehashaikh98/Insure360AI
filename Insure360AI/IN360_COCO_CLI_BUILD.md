# Insure360 AI — Customer Intelligence & Next Best Action Platform
## Complete CoCo Web Build Guide — End-to-End

**Hackathon:** Customer 360 and Next Best Action Engine  
**Project:** Insure360 AI  
**Role:** ACCOUNTADMIN  
**Warehouses:** INSURE360_WH / COMPUTE_WH  
**Database:** INSURE360_DB  
**Schemas:** RAW, AI, ANALYTICS  
**Primary Semantic View:** INSURE360_DB.ANALYTICS.SV_CUSTOMER_360  
**Cortex Agent:** INSURE360_AGENT  
**Development Assistant:** Snowflake CoCo Web in Snowsight  

---

# How to Use This Guide

This document is the complete end-to-end build and demonstration reference for the **Insure360 AI — Customer Intelligence & Next Best Action Platform**.

The solution addresses the Snowflake Hackathon challenge:

> **Customer 360 and Next Best Action Engine**

The objective is to combine structured insurance/customer information with unstructured customer interaction data to create a unified Customer 360.

Snowflake Cortex AI is used to understand customer conversations and derive intelligence such as:

- Customer sentiment
- Interaction classification
- Cancellation signals
- Urgency signals

These AI-derived signals are combined with traditional business signals such as:

- Policies
- Premium
- Claims
- Complaints
- Payments
- Renewal information

The combined intelligence is then used to generate:

- Customer 360
- Customer Intelligence
- Explainable Customer Risk
- Risk Reason
- Next Best Action
- Action Reason

The final intelligence is exposed through:

- Snowflake Semantic Views
- Cortex Analyst
- Cortex Agent

Snowflake CoCo is used as the development assistant throughout the implementation.

---

# Solution Objective

Insurance organizations typically maintain customer information across multiple operational systems.

For example:

```text
Customer Master
Policies
Claims
Payments
Complaints
Call Center Interactions
```

These datasets individually provide only part of the customer story.

A customer may appear healthy from structured information alone but may have recently called customer service and said:

> "My claim has been pending for weeks. I am extremely disappointed and I am thinking about cancelling my policy."

Traditional analytics can store this conversation but does not naturally understand its business meaning.

Insure360 AI solves this by combining:

```text
STRUCTURED DATA
+
UNSTRUCTURED CUSTOMER CONVERSATIONS
+
CORTEX AI
+
EXPLAINABLE BUSINESS RULES
+
SEMANTIC ANALYTICS
+
CONVERSATIONAL AI
```

to provide a unified and actionable customer view.

---

# High-Level Business Flow

```text
RAW CUSTOMER DATA
        |
        v
CUSTOMER INTERACTIONS
        |
        v
CORTEX AI
        |
        v
INTERACTION INTELLIGENCE
        |
        +-----------------------------+
        |                             |
        v                             v
STRUCTURED CUSTOMER DATA        AI-DERIVED SIGNALS
        |                             |
        +--------------+--------------+
                       |
                       v
                 CUSTOMER 360
                       |
                       v
             CUSTOMER INTELLIGENCE
                       |
                       v
              EXPLAINABLE RISK
                       |
                       v
              NEXT BEST ACTION
                       |
                       v
               SEMANTIC VIEW
                       |
                       v
               CORTEX ANALYST
                       |
                       v
                CORTEX AGENT
                       |
                       v
                BUSINESS USER
```

---

# Prerequisites & Dependencies Checklist

```text
BEFORE EXECUTING THE BUILD STEPS, ENSURE:

1. ROLES & PERMISSIONS

   - ACCOUNTADMIN or equivalent role is available
   - Role has CREATE DATABASE privileges
   - Role has CREATE SCHEMA privileges
   - Role has CREATE VIEW privileges
   - Role can create/use Semantic Views
   - Role can access Cortex AI
   - Role can access Cortex Analyst
   - Role can create/use Cortex Agents


2. DATABASE

   INSURE360_DB


3. SCHEMAS

   INSURE360_DB.RAW
   INSURE360_DB.AI
   INSURE360_DB.ANALYTICS


4. WAREHOUSE

   INSURE360_WH

   or

   COMPUTE_WH

   X-Small warehouse is sufficient for the current hackathon dataset.


5. CORTEX CAPABILITIES

   Cortex AI
   Cortex Analyst
   Cortex Agent
   Semantic Views


6. DEVELOPMENT ASSISTANT

   Snowflake CoCo Web in Snowsight


7. SOURCE DATA

   Structured:

   Customers
   Policies
   Claims
   Complaints
   Payments

   Unstructured:

   Customer interaction transcripts


8. DATA QUALITY

   CUSTOMER_ID must be consistent across all customer-related datasets.


9. CORE RAW OBJECTS

   INSURE360_DB.RAW.CUSTOMERS
   INSURE360_DB.RAW.POLICIES
   INSURE360_DB.RAW.CLAIMS
   INSURE360_DB.RAW.COMPLAINTS
   INSURE360_DB.RAW.PAYMENTS
   INSURE360_DB.RAW.INTERACTIONS


10. AI OBJECT

   INSURE360_DB.AI.INTERACTION_INSIGHTS


11. ANALYTICS OBJECTS

   INSURE360_DB.ANALYTICS.VW_CUSTOMER_360
   INSURE360_DB.ANALYTICS.VW_CUSTOMER_INTELLIGENCE
   INSURE360_DB.ANALYTICS.VW_CUSTOMER_RISK
   INSURE360_DB.ANALYTICS.VW_NEXT_BEST_ACTION
   INSURE360_DB.ANALYTICS.VW_CUSTOMER_DECISIONS
   INSURE360_DB.ANALYTICS.VW_CUSTOMER_SEMANTIC_SOURCE
   INSURE360_DB.ANALYTICS.SV_CUSTOMER_360
```

---

# Step 1 — Foundation

## 1.1 Database and Schemas

The solution uses a three-layer Snowflake architecture.

```text
INSURE360_DB

├── RAW
│
├── AI
│
└── ANALYTICS
```

### RAW Schema

```text
INSURE360_DB.RAW
```

Purpose:

Store source-aligned structured and unstructured insurance data.

---

### AI Schema

```text
INSURE360_DB.AI
```

Purpose:

Store AI-derived intelligence generated from customer interactions.

---

### ANALYTICS Schema

```text
INSURE360_DB.ANALYTICS
```

Purpose:

Store Customer 360, customer intelligence, risk, Next Best Action, decision and semantic objects.

---

## 1.2 Foundation SQL

```sql
USE ROLE ACCOUNTADMIN;

CREATE DATABASE IF NOT EXISTS INSURE360_DB;

CREATE SCHEMA IF NOT EXISTS INSURE360_DB.RAW;

CREATE SCHEMA IF NOT EXISTS INSURE360_DB.AI;

CREATE SCHEMA IF NOT EXISTS INSURE360_DB.ANALYTICS;

USE WAREHOUSE INSURE360_WH;
```

---

# Step 2 — RAW Data Layer

The RAW layer represents the original business information before analytical or AI transformation.

The core source domains are:

```text
CUSTOMERS
POLICIES
CLAIMS
COMPLAINTS
PAYMENTS
INTERACTIONS
```

---

# 2.1 CUSTOMER Master

## Table

```text
INSURE360_DB.RAW.CUSTOMERS
```

### Business Description

Stores the master profile for every customer.

### Logical Data Dictionary

| Column | Type | Business Description |
|---|---|---|
| CUSTOMER_ID | VARCHAR | Unique customer identifier |
| CUSTOMER_NAME | VARCHAR | Customer full name |
| AGE | NUMBER | Customer age |
| CITY | VARCHAR | Customer city |
| CUSTOMER_SEGMENT | VARCHAR | Customer business segment |
| JOIN_DATE | DATE | Date customer joined |
| EMAIL | VARCHAR | Customer email |
| PHONE | VARCHAR | Customer contact number |

### Grain

```text
One row per customer
```

---

# 2.2 POLICIES

## Table

```text
INSURE360_DB.RAW.POLICIES
```

### Business Description

Stores insurance policies owned by customers.

### Logical Data Dictionary

| Column | Type | Business Description |
|---|---|---|
| POLICY_ID | VARCHAR | Unique policy identifier |
| CUSTOMER_ID | VARCHAR | Customer owning the policy |
| POLICY_TYPE | VARCHAR | Type of insurance policy |
| PREMIUM_AMOUNT | NUMBER(12,2) | Policy premium amount |
| START_DATE | DATE | Policy start date |
| END_DATE | DATE | Policy expiry/renewal date |
| POLICY_STATUS | VARCHAR | Current policy status |

### Grain

```text
One row per policy
```

A customer may therefore have multiple policy records.

---

# 2.3 CLAIMS

## Table

```text
INSURE360_DB.RAW.CLAIMS
```

### Business Description

Stores insurance claims submitted by customers.

### Logical Data Dictionary

| Column | Type | Business Description |
|---|---|---|
| CLAIM_ID | VARCHAR | Unique claim identifier |
| POLICY_ID | VARCHAR | Associated policy |
| CUSTOMER_ID | VARCHAR | Associated customer |
| CLAIM_DATE | DATE | Claim submission date |
| CLAIM_AMOUNT | NUMBER(12,2) | Amount claimed |
| CLAIM_STATUS | VARCHAR | Current claim status |
| CLAIM_REASON | VARCHAR | Reason/category of claim |

### Grain

```text
One row per claim
```

---

# 2.4 COMPLAINTS

## Table

```text
INSURE360_DB.RAW.COMPLAINTS
```

### Business Description

Stores customer complaints and service issues.

### Logical Data Dictionary

| Column | Type | Business Description |
|---|---|---|
| COMPLAINT_ID | VARCHAR | Unique complaint identifier |
| CUSTOMER_ID | VARCHAR | Related customer |
| COMPLAINT_DATE | DATE | Complaint creation date |
| COMPLAINT_TYPE | VARCHAR | Complaint category |
| COMPLAINT_STATUS | VARCHAR | Current complaint status |
| DESCRIPTION | VARCHAR | Complaint details |

### Grain

```text
One row per complaint
```

---

# 2.5 PAYMENTS

## Table

```text
INSURE360_DB.RAW.PAYMENTS
```

### Business Description

Stores premium/payment activity associated with customers and policies.

### Logical Data Dictionary

| Column | Type | Business Description |
|---|---|---|
| PAYMENT_ID | VARCHAR | Unique payment identifier |
| POLICY_ID | VARCHAR | Associated policy |
| CUSTOMER_ID | VARCHAR | Associated customer |
| DUE_DATE | DATE | Payment due date |
| PAYMENT_DATE | DATE | Actual payment date |
| AMOUNT | NUMBER(12,2) | Payment amount |
| PAYMENT_STATUS | VARCHAR | Paid, overdue or pending |

### Grain

```text
One row per payment
```

---

# 2.6 CUSTOMER INTERACTIONS

## Table

```text
INSURE360_DB.RAW.INTERACTIONS
```

### Business Description

Stores customer conversations and interaction transcripts.

This is the primary source of **unstructured data** in Insure360.

### Logical Data Dictionary

| Column | Type | Business Description |
|---|---|---|
| INTERACTION_ID | VARCHAR | Unique interaction identifier |
| CUSTOMER_ID | VARCHAR | Related customer |
| INTERACTION_DATE | TIMESTAMP | Interaction timestamp |
| CHANNEL | VARCHAR | Interaction channel |
| TRANSCRIPT | VARCHAR | Customer conversation text |
| AGENT_NAME | VARCHAR | Service representative |
| DURATION_MINUTES | NUMBER | Interaction duration |

### Grain

```text
One row per interaction
```

---

# Why INTERACTIONS Is Important

Consider this transcript:

```text
"My claim has been pending for weeks.
I am extremely disappointed.
If this isn't resolved I may cancel my policy."
```

SQL can store the sentence.

But we want to understand:

```text
Is the customer unhappy?

Is the customer considering cancellation?

Is this interaction urgent?

Is this related to a claim?

Does this interaction increase customer risk?
```

This is where Cortex AI enters the architecture.

---

# Step 3 — Cortex AI Interaction Intelligence

The AI layer converts unstructured customer language into structured business signals.

The source is:

```text
INSURE360_DB.RAW.INTERACTIONS
```

The output is:

```text
INSURE360_DB.AI.INTERACTION_INSIGHTS
```

---

# 3.1 RAW → AI Flow

```text
INSURE360_DB.RAW.INTERACTIONS

            |
            | TRANSCRIPT
            v

      SNOWFLAKE CORTEX AI

      ┌──────────────────────┐
      │ Sentiment Analysis   │
      │ Classification       │
      │ Cancellation Signal  │
      │ Urgency Detection    │
      └──────────────────────┘

            |
            v

INSURE360_DB.AI.INTERACTION_INSIGHTS
```

---

# 3.2 Sentiment Analysis

Snowflake Cortex sentiment functionality is used to determine customer sentiment from interaction text.

Example:

```sql
SNOWFLAKE.CORTEX.SENTIMENT(TRANSCRIPT)
```

Conceptually:

```text
Customer:

"I am extremely unhappy with the delay."

                ↓

        Cortex Sentiment

                ↓

            NEGATIVE
```

The sentiment signal helps identify customer dissatisfaction.

---

# 3.3 Interaction Classification

AI classification is used to categorize customer interactions into meaningful business categories.

Example:

```text
"My claim has been pending and nobody is responding."

                ↓

            AI_CLASSIFY

                ↓

         CLAIM_ESCALATION
```

A result can conceptually look like:

```text
INT001
CUST001
{"labels":["CLAIM_ESCALATION"]}
```

---

# 3.4 AI-Derived Signals

The AI layer derives signals such as:

```text
SENTIMENT

INTERACTION CLASSIFICATION

CANCELLATION SIGNAL

URGENCY
```

Example:

```text
INTERACTION_ID       INT001

CUSTOMER_ID          CUST001

SENTIMENT            NEGATIVE

CLASSIFICATION       CLAIM_ESCALATION

CANCELLATION_SIGNAL  TRUE

URGENCY              HIGH
```

---

# 3.5 AI Output

## Table

```text
INSURE360_DB.AI.INTERACTION_INSIGHTS
```

### Business Description

Stores structured AI-derived intelligence extracted from customer interaction transcripts.

The important conceptual outputs include:

```text
INTERACTION_ID

CUSTOMER_ID

SENTIMENT / SENTIMENT SCORE

INTERACTION CLASSIFICATION

CANCELLATION SIGNAL

URGENCY
```

### Grain

```text
One row per analyzed customer interaction
```

---

# Important Design Principle

The distinction between RAW and AI is:

```text
RAW = What the customer said

AI = What we understood from what the customer said
```

The original transcript remains available in RAW.

The AI-derived interpretation is stored separately.

This provides traceability and allows AI intelligence to be recalculated without modifying source data.

---

# Step 4 — Customer 360 Analytics

The next stage combines structured insurance information.

The first major analytical view is:

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_360
```

---

# 4.1 Customer 360

## View

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_360
```

### Purpose

Create one consolidated structured business record per customer.

The view brings together:

```text
CUSTOMERS
   +
POLICIES
   +
CLAIMS
   +
COMPLAINTS
   +
PAYMENTS
```

---

## Customer 360 Measures

Important outputs include:

```text
CUSTOMER_ID

CUSTOMER_NAME

CUSTOMER_SEGMENT

TOTAL_PREMIUM

ACTIVE_POLICIES

OPEN_CLAIMS

OPEN_COMPLAINTS

OVERDUE_PAYMENTS

DAYS_TO_RENEWAL
```

---

## Example

```text
CUSTOMER_ID          CUST001

CUSTOMER_NAME        Aarav Sharma

CUSTOMER_SEGMENT     PREMIUM

TOTAL_PREMIUM        75000

ACTIVE_POLICIES      2

OPEN_CLAIMS          1

OPEN_COMPLAINTS      1

OVERDUE_PAYMENTS     0

DAYS_TO_RENEWAL      15
```

This represents the customer's **structured Customer 360**.

---

# Step 5 — Customer Intelligence

Customer 360 alone does not include everything learned from customer conversations.

Therefore we enrich it with:

```text
INSURE360_DB.AI.INTERACTION_INSIGHTS
```

to create:

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_INTELLIGENCE
```

---

# 5.1 Customer Intelligence View

## View

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_INTELLIGENCE
```

### Purpose

Combine structured Customer 360 with AI-derived customer interaction signals.

---

# Data Flow

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_360
                    |
                    |
                    +-------------------+
                                        |
                                        v
                           CUSTOMER INTELLIGENCE
                                        ^
                                        |
                    +-------------------+
                    |
INSURE360_DB.AI.INTERACTION_INSIGHTS
```

---

# AI Measures Added

Important AI-derived measures include:

```text
NEGATIVE_INTERACTIONS

CANCELLATION_SIGNALS

HIGH_URGENCY_INTERACTIONS
```

---

# Example

```text
CUSTOMER_ID                 CUST001

TOTAL_PREMIUM               75000

ACTIVE_POLICIES             2

OPEN_CLAIMS                 1

OPEN_COMPLAINTS             1

OVERDUE_PAYMENTS            0

NEGATIVE_INTERACTIONS       2

CANCELLATION_SIGNALS        1

HIGH_URGENCY_INTERACTIONS   1
```

At this stage, Insure360 understands both:

```text
WHAT IS HAPPENING WITH THE CUSTOMER?

and

WHAT IS THE CUSTOMER TELLING US?
```

---

# Step 6 — Explainable Customer Risk

The next stage calculates customer risk.

## View

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_RISK
```

---

# 6.1 Risk Inputs

Risk is determined using a combination of structured and AI-derived signals.

Structured signals include:

```text
OPEN_CLAIMS

OPEN_COMPLAINTS

OVERDUE_PAYMENTS
```

AI-derived signals include:

```text
NEGATIVE_INTERACTIONS

CANCELLATION_SIGNALS

HIGH_URGENCY_INTERACTIONS
```

---

# 6.2 Risk Flow

```text
OPEN CLAIMS
       |
OPEN COMPLAINTS
       |
OVERDUE PAYMENTS
       |
NEGATIVE INTERACTIONS
       |
CANCELLATION SIGNALS
       |
HIGH URGENCY INTERACTIONS
       |
       v
  RISK SCORING
       |
       v
   RISK_SCORE
       |
       v
   RISK_LEVEL
       |
       v
   RISK_REASON
```

---

# 6.3 Risk Thresholds

The governed risk thresholds are:

```text
HIGH

RISK_SCORE >= 60
```

```text
MEDIUM

RISK_SCORE >= 30
AND
RISK_SCORE < 60
```

```text
LOW

RISK_SCORE < 30
```

---

# 6.4 Risk Output

The risk view exposes:

```text
RISK_SCORE

RISK_LEVEL

RISK_REASON
```

Example:

```text
CUSTOMER_ID    CUST001

RISK_SCORE     75

RISK_LEVEL     HIGH

RISK_REASON    Customer has an open claim,
               negative interactions and
               cancellation intent.
```

---

# Why the Risk Model Is Explainable

An important design decision in Insure360 is:

> Cortex AI understands customer conversations, but the final customer risk decision remains governed and explainable.

We do not simply ask an LLM:

```text
"Is this customer risky?"
```

Instead:

```text
Structured Signals
        +
AI-Derived Signals
        |
        v
Governed Risk Logic
        |
        v
Risk Score
        |
        v
Risk Level
        |
        v
Risk Reason
```

This makes the result easier to:

- Explain
- Audit
- Govern
- Validate
- Present to business users

---

# Step 7 — Next Best Action

Identifying risk is only useful if the business knows what to do next.

Therefore Insure360 creates:

```text
INSURE360_DB.ANALYTICS.VW_NEXT_BEST_ACTION
```

---

# 7.1 Purpose

Convert customer state, risk and interaction intelligence into a recommended operational action.

---

# 7.2 Supported Actions

| Next Best Action | Business Purpose |
|---|---|
| CLAIM_ESCALATION | Prioritize a claim requiring urgent intervention |
| RETENTION_CALL | Contact a customer showing cancellation risk |
| SERVICE_RECOVERY | Resolve a negative customer experience |
| PAYMENT_REMINDER | Address overdue premium/payment |
| RENEWAL_OUTREACH | Engage customer approaching renewal |
| CROSS_SELL | Identify suitable additional product opportunity |
| NO_IMMEDIATE_ACTION | Customer does not currently require intervention |

---

# 7.3 Example Decision

```text
CUSTOMER

CUST001

        |
        v

HIGH RISK

        +
        
CANCELLATION INTENT

        +
        
NEGATIVE INTERACTION

        |
        v

NEXT_BEST_ACTION

RETENTION_CALL
```

Another example:

```text
OPEN CLAIM

        +

CLAIM ESCALATION SIGNAL

        +

HIGH URGENCY

        |
        v

CLAIM_ESCALATION
```

---

# 7.4 Action Explainability

The output contains:

```text
NEXT_BEST_ACTION

ACTION_REASON
```

Example:

```text
NEXT_BEST_ACTION

CLAIM_ESCALATION


ACTION_REASON

Customer has an unresolved claim and high-urgency
negative interaction requiring immediate intervention.
```

Therefore Insure360 does not only answer:

```text
What should we do?
```

It also answers:

```text
Why should we do it?
```

---

# Step 8 — Customer Decision Layer

## View

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_DECISIONS
```

### Purpose

Provide the final business-ready customer decision layer.

---

# Decision Flow

```text
WHO IS THE CUSTOMER?
        |
        v
WHAT POLICIES DO THEY HAVE?
        |
        v
WHAT IS THEIR PREMIUM VALUE?
        |
        v
DO THEY HAVE CLAIMS?
        |
        v
DO THEY HAVE COMPLAINTS?
        |
        v
ARE PAYMENTS OVERDUE?
        |
        v
WHAT ARE THEY SAYING?
        |
        v
ARE THEY UNHAPPY?
        |
        v
ARE THEY CONSIDERING CANCELLATION?
        |
        v
WHAT IS THEIR RISK?
        |
        v
WHY ARE THEY AT RISK?
        |
        v
WHAT SHOULD WE DO?
        |
        v
WHY SHOULD WE DO IT?
```

This is the core business intelligence generated by Insure360.

---

# Step 9 — Semantic Source

Before exposing the model to Cortex Analyst, the final customer intelligence is organized into a clean semantic source.

## View

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_SEMANTIC_SOURCE
```

---

# 9.1 Purpose

Provide a clean one-row-per-customer source for the semantic model.

---

# 9.2 Semantic Source Fields

```text
CUSTOMER_ID

CUSTOMER_NAME

CUSTOMER_SEGMENT

RISK_SCORE

RISK_LEVEL

RISK_REASON

TOTAL_PREMIUM

OPEN_CLAIMS

OPEN_COMPLAINTS

OVERDUE_PAYMENTS

DAYS_TO_RENEWAL

ACTIVE_POLICIES

NEGATIVE_INTERACTIONS

CANCELLATION_SIGNALS

HIGH_URGENCY_INTERACTIONS

NEXT_BEST_ACTION

ACTION_REASON
```

---

# 9.3 Grain

```text
One row per customer
```

The current model represents a **point-in-time Customer 360**.

---

# Step 10 — Semantic View

## Semantic View

```text
INSURE360_DB.ANALYTICS.SV_CUSTOMER_360
```

The Semantic View gives Cortex Analyst governed business meaning over the physical analytical data.

---

# 10.1 Logical Table

```text
CUSTOMER
```

---

# 10.2 Primary Key

```text
CUSTOMER_ID
```

---

# 10.3 Dimensions

Dimensions describe the customer or decision.

```text
CUSTOMER_ID

CUSTOMER_NAME

CUSTOMER_SEGMENT

RISK_LEVEL

RISK_REASON

NEXT_BEST_ACTION

ACTION_REASON
```

---

# 10.4 Facts

Facts represent measurable customer values.

```text
RISK_SCORE

TOTAL_PREMIUM

OPEN_CLAIMS

OPEN_COMPLAINTS

OVERDUE_PAYMENTS

DAYS_TO_RENEWAL

ACTIVE_POLICIES

NEGATIVE_INTERACTIONS

CANCELLATION_SIGNALS

HIGH_URGENCY_INTERACTIONS
```

---

# 10.5 Governed Metrics

The semantic model exposes business metrics such as:

```text
TOTAL_CUSTOMERS

HIGH_RISK_CUSTOMERS

MEDIUM_RISK_CUSTOMERS

LOW_RISK_CUSTOMERS

AVERAGE_RISK_SCORE

TOTAL_PREMIUM_AMOUNT

PREMIUM_AT_RISK

CUSTOMERS_WITH_CANCELLATION_INTENT

CUSTOMERS_WITH_OPEN_CLAIMS

CUSTOMERS_WITH_OPEN_COMPLAINTS

CLAIM_ESCALATION_CUSTOMERS

RETENTION_CUSTOMERS

CROSS_SELL_OPPORTUNITIES
```

---

# Step 11 — Metric Catalog

## MET-001 — Total Customers

```text
Metric:

TOTAL_CUSTOMERS
```

Formula:

```sql
COUNT(DISTINCT CUSTOMER.CUSTOMER_ID)
```

Purpose:

Total number of unique customers represented in Customer 360.

---

## MET-002 — High Risk Customers

```text
Metric:

HIGH_RISK_CUSTOMERS
```

Formula:

```sql
COUNT(
    DISTINCT CASE
        WHEN CUSTOMER.RISK_LEVEL = 'HIGH'
        THEN CUSTOMER.CUSTOMER_ID
    END
)
```

Business Rule:

```text
RISK_SCORE >= 60
```

---

## MET-003 — Medium Risk Customers

Formula:

```sql
COUNT(
    DISTINCT CASE
        WHEN CUSTOMER.RISK_LEVEL = 'MEDIUM'
        THEN CUSTOMER.CUSTOMER_ID
    END
)
```

Business Rule:

```text
30 <= RISK_SCORE < 60
```

---

## MET-004 — Low Risk Customers

Formula:

```sql
COUNT(
    DISTINCT CASE
        WHEN CUSTOMER.RISK_LEVEL = 'LOW'
        THEN CUSTOMER.CUSTOMER_ID
    END
)
```

Business Rule:

```text
RISK_SCORE < 30
```

---

## MET-005 — Average Risk Score

Formula:

```sql
AVG(CUSTOMER.RISK_SCORE)
```

---

## MET-006 — Total Premium

Formula:

```sql
SUM(CUSTOMER.TOTAL_PREMIUM)
```

---

## MET-007 — Premium At Risk

Formula:

```sql
SUM(
    CASE
        WHEN CUSTOMER.RISK_LEVEL = 'HIGH'
        THEN CUSTOMER.TOTAL_PREMIUM
        ELSE 0
    END
)
```

Purpose:

Measure the total premium value associated with high-risk customers.

---

## MET-008 — Customers With Cancellation Intent

Formula:

```sql
COUNT(
    DISTINCT CASE
        WHEN CUSTOMER.CANCELLATION_SIGNALS > 0
        THEN CUSTOMER.CUSTOMER_ID
    END
)
```

---

## MET-009 — Customers With Open Claims

Business Logic:

```text
OPEN_CLAIMS > 0
```

---

## MET-010 — Customers With Open Complaints

Business Logic:

```text
OPEN_COMPLAINTS > 0
```

---

## MET-011 — Claim Escalation Customers

Business Logic:

```text
NEXT_BEST_ACTION = 'CLAIM_ESCALATION'
```

---

## MET-012 — Retention Customers

Business Logic:

```text
NEXT_BEST_ACTION = 'RETENTION_CALL'
```

---

## MET-013 — Cross-Sell Opportunities

Business Logic:

```text
NEXT_BEST_ACTION = 'CROSS_SELL'
```

---

# Step 12 — Cortex Analyst

Cortex Analyst provides natural-language analytical access over the governed Semantic View.

The Analyst uses:

```text
INSURE360_DB.ANALYTICS.SV_CUSTOMER_360
```

---

# 12.1 Cortex Analyst Flow

```text
BUSINESS QUESTION

        |
        v

CORTEX ANALYST

        |
        v

INSURE360_DB.ANALYTICS.SV_CUSTOMER_360

        |
        v

GOVERNED BUSINESS DEFINITIONS

        |
        v

GENERATED SQL

        |
        v

SNOWFLAKE

        |
        v

BUSINESS ANSWER
```

---

# 12.2 Verified Business Questions

Representative validated questions include:

```text
1. How many high risk customers are there?

2. What is the premium at risk?

3. Which customers have cancellation intent?

4. Which customers require claim escalation?

5. Which customers need a retention call?

6. Which customers are cross sell opportunities?

7. Show the top 5 customers by risk score.

8. Show high risk customers with risk reason
   and next best action.
```

---

# Why the Semantic View Matters

Without a semantic layer, Cortex would need to determine independently what business concepts mean.

For example:

```text
"What is a high-risk customer?"
```

Insure360 already defines this:

```text
RISK_SCORE >= 60
```

Similarly:

```text
"What is premium at risk?"
```

The semantic layer defines it as premium belonging to HIGH-risk customers.

Therefore:

```text
Semantic View
=
Governed Business Meaning
```

---

# Step 13 — Cortex Agent

## Agent

```text
INSURE360_AGENT
```

### Purpose

Provide a conversational interface over Customer 360, customer risk and Next Best Action intelligence.

---

# 13.1 Agent Tool

The Agent uses Cortex Analyst backed by:

```text
INSURE360_DB.ANALYTICS.SV_CUSTOMER_360
```

---

# 13.2 Agent Architecture

```text
BUSINESS USER

       |
       v

INSURE360_AGENT

       |
       v

CORTEX ANALYST

       |
       v

INSURE360_DB.ANALYTICS.SV_CUSTOMER_360

       |
       v

INSURE360 ANALYTICS

       |
       v

GOVERNED RESPONSE
```

---

# 13.3 Recommended Agent Instructions

```text
You are Insure360 AI, an insurance Customer Intelligence
and Next Best Action assistant.

Use the governed Customer 360 semantic view for analytical questions.

RULES:

1. Use governed semantic metrics for analytical questions.

2. Never fabricate customer information or analytical results.

3. When discussing customer risk, explain RISK_REASON.

4. When recommending a next best action, explain ACTION_REASON.

5. Distinguish AI-derived interaction signals from the
   rule-based customer risk score.

6. If the semantic model does not contain enough information
   to answer a question, clearly state the limitation.

7. Prefer concise, business-oriented explanations.

8. Use customer names and business context where appropriate.

9. Do not invent metrics outside the governed semantic model.
```

---

# Step 14 — Agent Publishing

Publishing the Agent does **not** create a second Agent object.

Instead, versions exist inside the same Agent.

Conceptually:

```text
INSURE360_AGENT

       |
       +----------------------+
       |                      |
       v                      v

     DRAFT             PUBLISHED VERSIONS

                            |
                            +--- Version 1

                            |
                            +--- Version 2
                                  |
                                  v
                                IN USE
```

The published version marked:

```text
IN USE
```

is the version receiving traffic.

---

# Demo Usage

For the hackathon or manager demo:

```text
AI & ML
   |
   v
Agents
   |
   v
INSURE360_AGENT
   |
   v
Preview
```

Use Preview to demonstrate the conversational experience.

Publishing again is only required when the Agent configuration has changed and a new version needs to be deployed.

---

# Step 15 — CoCo Usage

Snowflake CoCo has a different responsibility from Cortex Agent.

---

# CoCo

```text
DEVELOPMENT ASSISTANT
```

Used by:

```text
Developer
```

for:

```text
SQL Development

Data Model Review

Analytics Design

Semantic Source Review

Semantic View Design

Dimension Design

Fact Design

Metric Design

SQL Validation

Cortex Analyst Question Design

Agent Configuration

Troubleshooting

Documentation
```

---

# Cortex Agent

```text
BUSINESS RUNTIME ASSISTANT
```

Used by:

```text
Business User
```

for:

```text
Customer Analysis

Risk Analysis

Customer Prioritization

Next Best Action

Natural Language Analytics
```

---

# CoCo vs Cortex Agent

```text
                     DEVELOPMENT

Developer  <------->  CoCo

                       |
                       v

                 Build Insure360

                       |
                       |
-------------------------------------------------

                       |
                       v

                      RUNTIME

Business User -----> Cortex Agent

                       |
                       v

                  Cortex Analyst

                       |
                       v

                  Semantic View

                       |
                       v

                   Insure360
```

---

# Step 16 — Object-to-Business-Function Mapping

| Object | Business Function | Grain |
|---|---|---|
| INSURE360_DB.RAW.CUSTOMERS | Customer master | Customer |
| INSURE360_DB.RAW.POLICIES | Policy portfolio | Policy |
| INSURE360_DB.RAW.CLAIMS | Claims activity | Claim |
| INSURE360_DB.RAW.COMPLAINTS | Customer complaints | Complaint |
| INSURE360_DB.RAW.PAYMENTS | Payment behavior | Payment |
| INSURE360_DB.RAW.INTERACTIONS | Customer conversations | Interaction |
| INSURE360_DB.AI.INTERACTION_INSIGHTS | AI interpretation of interactions | Interaction |
| INSURE360_DB.ANALYTICS.VW_CUSTOMER_360 | Structured Customer 360 | Customer |
| INSURE360_DB.ANALYTICS.VW_CUSTOMER_INTELLIGENCE | Customer 360 + AI signals | Customer |
| INSURE360_DB.ANALYTICS.VW_CUSTOMER_RISK | Explainable customer risk | Customer |
| INSURE360_DB.ANALYTICS.VW_NEXT_BEST_ACTION | Recommended customer action | Customer |
| INSURE360_DB.ANALYTICS.VW_CUSTOMER_DECISIONS | Business-ready decision record | Customer |
| INSURE360_DB.ANALYTICS.VW_CUSTOMER_SEMANTIC_SOURCE | Semantic model source | Customer |
| INSURE360_DB.ANALYTICS.SV_CUSTOMER_360 | Governed business semantic model | Customer |

---

# Step 17 — Complete Data Lineage

```text
INSURE360_DB.RAW.CUSTOMERS
             |
INSURE360_DB.RAW.POLICIES
             |
INSURE360_DB.RAW.CLAIMS
             |
INSURE360_DB.RAW.COMPLAINTS
             |
INSURE360_DB.RAW.PAYMENTS
             |
             +------------------------------+
                                            |
                                            v
                         ANALYTICS.VW_CUSTOMER_360
                                            |
                                            |
                                            +----------------------+
                                                                   |
                                                                   v
INSURE360_DB.RAW.INTERACTIONS                         CUSTOMER INTELLIGENCE
             |                                                     ^
             v                                                     |
        CORTEX AI                                                  |
             |                                                     |
             v                                                     |
INSURE360_DB.AI.INTERACTION_INSIGHTS ------------------------------+
                                                                   |
                                                                   v
                                                ANALYTICS.VW_CUSTOMER_RISK
                                                                   |
                                                                   v
                                            ANALYTICS.VW_NEXT_BEST_ACTION
                                                                   |
                                                                   v
                                            ANALYTICS.VW_CUSTOMER_DECISIONS
                                                                   |
                                                                   v
                                      ANALYTICS.VW_CUSTOMER_SEMANTIC_SOURCE
                                                                   |
                                                                   v
                                            ANALYTICS.SV_CUSTOMER_360
                                                                   |
                                                                   v
                                                    CORTEX ANALYST
                                                                   |
                                                                   v
                                                     INSURE360_AGENT
                                                                   |
                                                                   v
                                                      BUSINESS USER
```

---

# Step 18 — Deployment Order

```text
PHASE 1 — FOUNDATION

→ Create INSURE360_DB

→ Create INSURE360_DB.RAW

→ Create INSURE360_DB.AI

→ Create INSURE360_DB.ANALYTICS

→ Configure warehouse


PHASE 2 — RAW DATA

→ Create/load INSURE360_DB.RAW.CUSTOMERS

→ Create/load INSURE360_DB.RAW.POLICIES

→ Create/load INSURE360_DB.RAW.CLAIMS

→ Create/load INSURE360_DB.RAW.COMPLAINTS

→ Create/load INSURE360_DB.RAW.PAYMENTS

→ Create/load INSURE360_DB.RAW.INTERACTIONS


PHASE 3 — CORTEX AI

→ Process interaction transcripts

→ Run sentiment analysis

→ Run interaction classification

→ Derive cancellation signals

→ Derive urgency signals

→ Populate INSURE360_DB.AI.INTERACTION_INSIGHTS


PHASE 4 — CUSTOMER 360

→ Create INSURE360_DB.ANALYTICS.VW_CUSTOMER_360

→ Validate structured customer aggregation


PHASE 5 — CUSTOMER INTELLIGENCE

→ Combine Customer 360 with AI signals

→ Create INSURE360_DB.ANALYTICS.VW_CUSTOMER_INTELLIGENCE


PHASE 6 — RISK

→ Create INSURE360_DB.ANALYTICS.VW_CUSTOMER_RISK

→ Validate:

   HIGH >= 60

   MEDIUM >= 30 AND < 60

   LOW < 30


PHASE 7 — NEXT BEST ACTION

→ Create INSURE360_DB.ANALYTICS.VW_NEXT_BEST_ACTION

→ Validate NEXT_BEST_ACTION

→ Validate ACTION_REASON


PHASE 8 — CUSTOMER DECISION LAYER

→ Create INSURE360_DB.ANALYTICS.VW_CUSTOMER_DECISIONS


PHASE 9 — SEMANTIC LAYER

→ Create INSURE360_DB.ANALYTICS.VW_CUSTOMER_SEMANTIC_SOURCE

→ Create INSURE360_DB.ANALYTICS.SV_CUSTOMER_360

→ Configure dimensions

→ Configure facts

→ Configure metrics


PHASE 10 — CORTEX ANALYST

→ Connect Cortex Analyst to:

   INSURE360_DB.ANALYTICS.SV_CUSTOMER_360

→ Configure verified questions

→ Test natural-language queries


PHASE 11 — CORTEX AGENT

→ Configure INSURE360_AGENT

→ Connect Cortex Analyst tool

→ Configure Agent instructions

→ Test Agent

→ Publish Agent

→ Set intended published version IN USE


PHASE 12 — VALIDATION

→ Test one customer end-to-end

→ Validate AI signals

→ Validate risk

→ Validate NBA

→ Validate Semantic View

→ Validate Cortex Analyst

→ Validate Cortex Agent


PHASE 13 — DEMO

→ Show RAW interaction

→ Show AI interpretation

→ Show Customer 360

→ Show Risk

→ Show Next Best Action

→ Show Cortex Agent
```

---

# Step 19 — Validation Checklist

| # | Validation | Method | Expected Result |
|---|---|---|---|
| 1 | Customer data loaded | Query INSURE360_DB.RAW.CUSTOMERS | Customer records available |
| 2 | Policy data loaded | Query INSURE360_DB.RAW.POLICIES | Policy records available |
| 3 | Claims loaded | Query INSURE360_DB.RAW.CLAIMS | Claim records available |
| 4 | Complaints loaded | Query INSURE360_DB.RAW.COMPLAINTS | Complaint records available |
| 5 | Payments loaded | Query INSURE360_DB.RAW.PAYMENTS | Payment records available |
| 6 | Interactions loaded | Query INSURE360_DB.RAW.INTERACTIONS | Transcript text available |
| 7 | AI processing successful | Query INSURE360_DB.AI.INTERACTION_INSIGHTS | AI signals available |
| 8 | Customer 360 created | Query VW_CUSTOMER_360 | Customer aggregation available |
| 9 | AI enrichment created | Query VW_CUSTOMER_INTELLIGENCE | AI signals aggregated |
| 10 | Risk generated | Query VW_CUSTOMER_RISK | Score, level and reason available |
| 11 | NBA generated | Query VW_NEXT_BEST_ACTION | Action and reason available |
| 12 | Semantic source available | Query VW_CUSTOMER_SEMANTIC_SOURCE | One row/customer |
| 13 | Semantic View active | Inspect SV_CUSTOMER_360 | Dimensions/facts/metrics available |
| 14 | Analyst responds | Ask verified query | Governed response |
| 15 | Agent responds | Agent Preview | Valid grounded response |
| 16 | Agent published | Check version history | Intended version marked IN USE |

---

# Step 20 — End-to-End Customer Example

Consider:

```text
CUSTOMER

CUST001
```

Structured information:

```text
TOTAL PREMIUM

75,000


ACTIVE POLICIES

2


OPEN CLAIMS

1


OPEN COMPLAINTS

1


OVERDUE PAYMENTS

0
```

The customer then contacts the insurer.

---

## RAW Interaction

Stored in:

```text
INSURE360_DB.RAW.INTERACTIONS
```

Transcript:

```text
"My claim has been pending for weeks.

I am extremely disappointed.

If this isn't resolved soon,
I am considering cancelling my policy."
```

---

## Cortex AI Processing

```text
TRANSCRIPT

      |
      v

CORTEX AI

      |
      +-------------------------+
      |                         |
      v                         v

SENTIMENT                  CLASSIFICATION

NEGATIVE                   CLAIM_ESCALATION


      +-------------------------+
      |                         |
      v                         v

CANCELLATION               URGENCY

TRUE                       HIGH
```

---

## AI Output

Stored in:

```text
INSURE360_DB.AI.INTERACTION_INSIGHTS
```

Conceptual result:

```text
INTERACTION_ID

INT001


CUSTOMER_ID

CUST001


SENTIMENT

NEGATIVE


CLASSIFICATION

CLAIM_ESCALATION


CANCELLATION SIGNAL

TRUE


URGENCY

HIGH
```

---

## Customer Intelligence

Structured information:

```text
OPEN CLAIM             1

OPEN COMPLAINT         1
```

AI information:

```text
NEGATIVE INTERACTION   1

CANCELLATION SIGNAL    1

HIGH URGENCY           1
```

Combined:

```text
CUSTOMER INTELLIGENCE
```

---

## Risk

The risk engine evaluates these signals.

Output:

```text
RISK_LEVEL

HIGH
```

along with:

```text
RISK_REASON
```

explaining the contributing customer signals.

---

## Next Best Action

The system evaluates:

```text
OPEN CLAIM

+

CLAIM ESCALATION

+

HIGH URGENCY

+

NEGATIVE CUSTOMER EXPERIENCE
```

and recommends:

```text
CLAIM_ESCALATION
```

with an:

```text
ACTION_REASON
```

explaining why the intervention is recommended.

---

## Business User

The manager can now ask:

```text
"Show high-risk customers requiring claim escalation."
```

Flow:

```text
Manager

   |
   v

INSURE360_AGENT

   |
   v

Cortex Analyst

   |
   v

SV_CUSTOMER_360

   |
   v

Governed Customer Data

   |
   v

Business Answer
```

This demonstrates the complete:

```text
SIGNAL → INTELLIGENCE → RISK → ACTION
```

journey.

---

# Step 21 — Manager / Judge Demo Flow

The recommended demonstration sequence is:

---

## Demo 1 — Explain the Business Problem

Say:

```text
Insurance companies have customer information distributed
across policies, claims, payments, complaints and interactions.

Looking at one source alone does not provide the complete
customer picture.

Insure360 AI combines these sources into a unified Customer 360,
uses Cortex AI to understand customer conversations, calculates
explainable customer risk, recommends the Next Best Action,
and exposes the intelligence through a Cortex Agent.
```

---

# Demo 2 — Show RAW Interaction

Open:

```text
INSURE360_DB.RAW.INTERACTIONS
```

Show a customer transcript containing a meaningful business signal.

Explain:

```text
This is the original unstructured customer conversation.

At this stage we know what the customer said,
but we have not yet converted it into business intelligence.
```

---

# Demo 3 — Show Cortex AI Intelligence

Open:

```text
INSURE360_DB.AI.INTERACTION_INSIGHTS
```

Show the same interaction after AI processing.

Highlight:

```text
Sentiment

Classification

Cancellation signal

Urgency
```

Say:

```text
RAW tells us what the customer said.

The AI layer tells us what we understood
from what the customer said.
```

---

# Demo 4 — Show Customer 360

Open:

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_360
```

Highlight:

```text
TOTAL_PREMIUM

ACTIVE_POLICIES

OPEN_CLAIMS

OPEN_COMPLAINTS

OVERDUE_PAYMENTS

DAYS_TO_RENEWAL
```

Explain:

```text
This consolidates the structured operational customer information.
```

---

# Demo 5 — Show Customer Intelligence

Open:

```text
INSURE360_DB.ANALYTICS.VW_CUSTOMER_INTELLIGENCE
```

Highlight:

```text
NEGATIVE_INTERACTIONS

CANCELLATION_SIGNALS

HIGH_URGENCY_INTERACTIONS
```

Explain:

```text
At this stage structured Customer 360
and AI-derived customer intelligence come together.
```

---

# Demo 6 — Show Risk and Next Best Action

Show:

```text
RISK_SCORE

RISK_LEVEL

RISK_REASON

NEXT_BEST_ACTION

ACTION_REASON
```

This should be one of the main business moments in the demo.

Explain:

```text
We do not stop at saying that the customer is risky.

We explain why the customer is risky
and what the business should do next.
```

---

# Demo 7 — Explain the Semantic View

Open:

```text
INSURE360_DB.ANALYTICS.SV_CUSTOMER_360
```

Explain:

```text
The Semantic View provides governed business meaning.

It defines dimensions, facts and metrics so Cortex Analyst
understands business concepts consistently.
```

Give an example:

```text
HIGH RISK CUSTOMER

=

RISK_SCORE >= 60
```

Therefore Cortex does not need to invent what "high risk" means.

---

# Demo 8 — Cortex Agent

Open:

```text
INSURE360_AGENT
```

Use Preview.

---

## Question 1

```text
How many high risk customers are there?
```

Purpose:

Demonstrate governed metric retrieval.

---

## Question 2

```text
Show the top 5 customers by risk score
with their risk reason and next best action.
```

Purpose:

Demonstrate ranking + explainability + action.

---

## Question 3

```text
Which customers have cancellation intent
and what action should we take?
```

Purpose:

Demonstrate the connection between AI-derived customer intent and operational action.

---

# Demo Closing Statement

```text
The value of Insure360 is not simply bringing customer data together.

We combine structured operational information with AI-derived
signals from customer conversations.

We turn those signals into explainable customer risk and
recommended Next Best Actions.

Finally, we expose this governed intelligence through Cortex
Analyst and Cortex Agent so business users can move from
customer data to business decision using natural language.
```

---

# Step 22 — Architecture Diagram

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                              BUSINESS USER                                  │
│                                                                             │
│                                     │                                       │
│                                     ▼                                       │
│                           ┌────────────────────┐                            │
│                           │  INSURE360_AGENT   │                            │
│                           │   Cortex Agent     │                            │
│                           └─────────┬──────────┘                            │
│                                     │                                       │
│                                     ▼                                       │
│                           ┌────────────────────┐                            │
│                           │  Cortex Analyst    │                            │
│                           └─────────┬──────────┘                            │
│                                     │                                       │
│                                     ▼                                       │
│                  INSURE360_DB.ANALYTICS.SV_CUSTOMER_360                    │
│                                     │                                       │
├─────────────────────────────────────┼───────────────────────────────────────┤
│                                     ▼                                       │
│                                                                             │
│                           ANALYTICS LAYER                                   │
│                                                                             │
│                   VW_CUSTOMER_SEMANTIC_SOURCE                              │
│                              ▲                                              │
│                              │                                              │
│                    VW_CUSTOMER_DECISIONS                                   │
│                              ▲                                              │
│                              │                                              │
│                    VW_NEXT_BEST_ACTION                                     │
│                              ▲                                              │
│                              │                                              │
│                       VW_CUSTOMER_RISK                                     │
│                              ▲                                              │
│                              │                                              │
│                 VW_CUSTOMER_INTELLIGENCE                                   │
│                              ▲                                              │
│                              │                                              │
│                       VW_CUSTOMER_360                                      │
│                                                                             │
├─────────────────────────────┬───────────────────────────────────────────────┤
│                             │                                               │
│          RAW LAYER          │                 AI LAYER                      │
│                             │                                               │
│  CUSTOMERS                  │                                               │
│  POLICIES                   │                                               │
│  CLAIMS                     │                                               │
│  COMPLAINTS                 │                                               │
│  PAYMENTS                   │                                               │
│                             │                                               │
│  INTERACTIONS ──────────────┼──────► CORTEX AI                             │
│                             │              │                                │
│                             │              ▼                                │
│                             │       INTERACTION_INSIGHTS                    │
│                             │              │                                │
│                             └──────────────┼────────────────────────────────┤
│                                            │                                │
│                                            ▼                                │
│                                   CUSTOMER INTELLIGENCE                     │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

Development Assistant: Snowflake CoCo Web

Platform: Snowflake
```

---

# Step 23 — Technology Stack

| Layer | Technology |
|---|---|
| Cloud Data Platform | Snowflake |
| Data Storage | Snowflake Tables |
| Transformation | Snowflake SQL |
| Unstructured Intelligence | Snowflake Cortex AI |
| Sentiment | SNOWFLAKE.CORTEX.SENTIMENT |
| Classification | AI_CLASSIFY |
| Business Analytics | Snowflake Views |
| Semantic Modeling | Snowflake Semantic Views |
| Natural Language Analytics | Cortex Analyst |
| Conversational Intelligence | Cortex Agent |
| Development Assistant | Snowflake CoCo |
| Optional UI | Streamlit in Snowflake |

---

# Step 24 — Key Design Decisions

## Decision 1 — Separate RAW and AI

RAW contains original source information.

AI contains derived interpretation.

Benefits:

```text
Traceability

Reprocessing

Auditability

Separation of source truth from AI interpretation
```

---

## Decision 2 — AI Understands Language, Rules Determine Risk

AI is strongest where traditional SQL is weak:

```text
Understanding natural language
```

Therefore Cortex AI handles:

```text
Sentiment

Intent

Cancellation language

Urgency
```

Risk scoring remains governed.

This provides:

```text
Explainability

Auditability

Business control
```

---

## Decision 3 — Risk Reason Is Stored

Returning:

```text
RISK_LEVEL = HIGH
```

is not enough.

Therefore Insure360 also provides:

```text
RISK_REASON
```

so business users understand the decision.

---

## Decision 4 — Action Reason Is Stored

Returning:

```text
NEXT_BEST_ACTION = RETENTION_CALL
```

is not enough.

Therefore Insure360 also provides:

```text
ACTION_REASON
```

so users understand why the recommendation exists.

---

## Decision 5 — Semantic View Governs Business Meaning

Cortex Analyst queries:

```text
INSURE360_DB.ANALYTICS.SV_CUSTOMER_360
```

instead of interpreting multiple raw tables independently.

This creates:

```text
Consistent metrics

Consistent definitions

Governed analytics

Safer natural-language querying
```

---

# Step 25 — Current Scope and Limitations

## Point-in-Time Model

The primary Semantic View represents:

```text
One row per customer
```

at the current point in time.

Therefore the current semantic model is optimized for questions such as:

```text
Who is high risk?

Who has cancellation intent?

What premium is at risk?

Who needs claim escalation?

What action should we take?
```

---

## Historical Trends

A dedicated time dimension is not currently part of the primary Customer 360 Semantic View.

Therefore questions such as:

```text
How has customer risk changed month by month for the last year?
```

would require additional historical/time-grained modeling.

---

## Risk Model

The current model is:

```text
Rule-Based Explainable Risk
```

It is not currently:

```text
Predictive ML Churn Probability
```

This is intentional for the hackathon solution because the recommendation remains transparent.

---

## Next Best Action

NBA is based on governed customer signals and business logic.

It is not currently a reinforcement-learning recommendation engine.

---

## Cortex Search

Cortex Search is not required for the current core architecture because the primary use case is governed analytics over structured Customer 360 data.

A future enhancement could introduce:

```text
Policy Documents

Product Documents

Claim Procedures

Retention Playbooks

Compliance Documents
```

and expose them through Cortex Search.

---

## Streamlit

Streamlit is optional.

The core end-to-end solution is already:

```text
RAW

↓

AI

↓

CUSTOMER 360

↓

RISK

↓

NEXT BEST ACTION

↓

SEMANTIC VIEW

↓

CORTEX ANALYST

↓

CORTEX AGENT
```

A dashboard can be added as an operational UI enhancement later.

---

# Step 26 — Future Enhancements

Potential future extensions include:

```text
1. Historical Customer Risk Trends

2. Predictive Churn Model

3. Customer Lifetime Value

4. Product Recommendation Model

5. Policy Document Cortex Search

6. Retention Playbook Cortex Search

7. Agent + Search hybrid reasoning

8. Real-time interaction ingestion

9. Real-time risk recalculation

10. Streamlit Executive Dashboard

11. Agent embedded in Streamlit

12. Customer-level timeline

13. Agent feedback loop

14. NBA effectiveness tracking

15. A/B testing of retention actions
```

---

# Step 27 — Recommended Project Documentation

```text
COCO_CLI_HACKATHON/

│
├── README.md
│
├── INSURE360_COCO_BUILD.md
│
├── 01_PROJECT_OVERVIEW.md
│
├── 02_SOLUTION_ARCHITECTURE.md
│
├── 03_BUSINESS_GLOSSARY.md
│
├── 04_DATA_MODEL.md
│
├── 05_DATA_DICTIONARY.md
│
├── 06_RAW_DDL.sql
│
├── 07_SAMPLE_DATA.sql
│
├── 08_CORTEX_AI.sql
│
├── 09_CUSTOMER_360.sql
│
├── 10_RISK_MODEL.sql
│
├── 11_NEXT_BEST_ACTION.sql
│
├── 12_SEMANTIC_SOURCE.sql
│
├── 13_SEMANTIC_VIEW.sql
│
├── 14_METRIC_CATALOG.md
│
├── 15_VERIFIED_QUESTIONS.md
│
├── 16_AGENT_ARCHITECTURE.md
│
├── 17_AGENT_TEST_CASES.md
│
├── 18_GOVERNANCE.md
│
├── 19_COCO_USAGE.md
│
├── 20_DEPLOYMENT_GUIDE.md
│
└── 21_DEMO_SCRIPT.md
```

---

# File Reference

| File | Purpose |
|---|---|
| README.md | Project entry point |
| INSURE360_COCO_BUILD.md | Complete end-to-end master build guide |
| 01_PROJECT_OVERVIEW.md | Challenge statement and project objectives |
| 02_SOLUTION_ARCHITECTURE.md | Complete solution architecture |
| 03_BUSINESS_GLOSSARY.md | Governed insurance/customer terminology |
| 04_DATA_MODEL.md | Entity relationships and table grains |
| 05_DATA_DICTIONARY.md | Column-level documentation |
| 06_RAW_DDL.sql | RAW table creation |
| 07_SAMPLE_DATA.sql | Hackathon demonstration data |
| 08_CORTEX_AI.sql | Cortex AI interaction processing |
| 09_CUSTOMER_360.sql | Customer 360 analytical logic |
| 10_RISK_MODEL.sql | Explainable risk model |
| 11_NEXT_BEST_ACTION.sql | NBA rules |
| 12_SEMANTIC_SOURCE.sql | Semantic source definition |
| 13_SEMANTIC_VIEW.sql | SV_CUSTOMER_360 |
| 14_METRIC_CATALOG.md | Governed metric definitions |
| 15_VERIFIED_QUESTIONS.md | Cortex Analyst verified questions |
| 16_AGENT_ARCHITECTURE.md | Cortex Agent architecture |
| 17_AGENT_TEST_CASES.md | Agent validation scenarios |
| 18_GOVERNANCE.md | Explainability and governance |
| 19_COCO_USAGE.md | CoCo development workflow |
| 20_DEPLOYMENT_GUIDE.md | Deployment order |
| 21_DEMO_SCRIPT.md | Manager/judge demonstration |

---

# Executive Summary

**Insure360 AI** is a Snowflake-native Customer Intelligence and Next Best Action platform.

The solution addresses four major business problems.

---

## 1. Fragmented Customer Information

Customer information exists across:

```text
Customers

Policies

Claims

Complaints

Payments

Interactions
```

Insure360 consolidates this information into a unified Customer 360.

---

## 2. Unstructured Customer Conversations

Important customer signals are hidden inside interaction transcripts.

Cortex AI converts those conversations into structured intelligence such as:

```text
Sentiment

Classification

Cancellation Intent

Urgency
```

---

## 3. Data Without Action

Traditional dashboards may tell the business:

```text
This customer has an open claim.
```

Insure360 goes further:

```text
This customer is HIGH risk.

WHY?

Open claim + negative interaction + cancellation intent.

WHAT SHOULD WE DO?

CLAIM_ESCALATION.

WHY?

The customer requires urgent claim intervention.
```

---

## 4. Complex Analytics Access

Business users should not need to understand SQL or Snowflake schemas.

Therefore:

```text
Semantic View

        ↓

Cortex Analyst

        ↓

Cortex Agent
```

allows users to ask questions naturally.

For example:

```text
"Which customers are at highest risk and why?"
```

or:

```text
"Which customers have cancellation intent
and what action should we take?"
```

---

# Final Value Chain

```text
RAW DATA

     ↓

CORTEX AI

     ↓

AI INTERACTION INTELLIGENCE

     ↓

CUSTOMER 360

     ↓

CUSTOMER INTELLIGENCE

     ↓

EXPLAINABLE RISK

     ↓

NEXT BEST ACTION

     ↓

SEMANTIC VIEW

     ↓

CORTEX ANALYST

     ↓

CORTEX AGENT

     ↓

BUSINESS DECISION
```

---

# One-Line Project Pitch

> **Insure360 AI combines structured insurance data with AI-derived intelligence from customer conversations to create a unified Customer 360, calculate explainable customer risk, recommend the Next Best Action, and provide governed conversational analytics through Snowflake Cortex.**

---

# Demo Closing Message

> **Insure360 does not stop at showing customer data. It understands what the customer is saying, combines that intelligence with operational signals, explains who is at risk and why, recommends what the business should do next, and makes the entire decision layer accessible through natural language.**

---

*Built with Snowflake CoCo, Cortex AI, Cortex Analyst, Semantic Views and Cortex Agent*  

**Hackathon:** Customer 360 and Next Best Action Engine  
**Project:** Insure360 AI  
**Database:** INSURE360_DB  
**Schemas:** RAW, AI, ANALYTICS  
**Semantic View:** INSURE360_DB.ANALYTICS.SV_CUSTOMER_360  
**Agent:** INSURE360_AGENT
