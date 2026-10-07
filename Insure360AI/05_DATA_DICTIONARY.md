# Insure360 — Data Dictionary

## RAW.CLAIMS

| Column | Type | Description |
|---|---|---|
| CLAIM_ID | VARCHAR(20) | Claim identifier |
| POLICY_ID | VARCHAR(20) | Related policy identifier |
| CUSTOMER_ID | VARCHAR(20) | Related customer identifier |
| CLAIM_DATE | DATE | Claim date |
| CLAIM_TYPE | VARCHAR(50) | Claim category/type |
| CLAIM_AMOUNT | NUMBER(14,2) | Claimed amount |
| CLAIM_STATUS | VARCHAR(30) | Claim lifecycle status |
| SETTLEMENT_AMOUNT | NUMBER(14,2) | Settlement amount |
| SETTLEMENT_DATE | DATE | Settlement date |
| CLAIM_DESCRIPTION | VARCHAR(500) | Claim description |

## RAW.COMPLAINTS

| Column | Type | Description |
|---|---|---|
| COMPLAINT_ID | VARCHAR(20) | Complaint identifier |
| CUSTOMER_ID | VARCHAR(20) | Related customer |
| RELATED_CLAIM_ID | VARCHAR(20) | Optional related claim |
| COMPLAINT_DATE | DATE | Complaint date |
| CATEGORY | VARCHAR(50) | Complaint category |
| STATUS | VARCHAR(20) | Complaint status |
| PRIORITY | VARCHAR(20) | Complaint priority |
| RESOLUTION_DATE | DATE | Complaint resolution date |

## RAW.CUSTOMERS

| Column | Type | Description |
|---|---|---|
| CUSTOMER_ID | VARCHAR(20) | Customer identifier |
| CUSTOMER_NAME | VARCHAR(100) | Customer name |
| DATE_OF_BIRTH | DATE | Date of birth |
| GENDER | VARCHAR(20) | Gender |
| CITY | VARCHAR(50) | City |
| REGION | VARCHAR(50) | Region |
| JOIN_DATE | DATE | Customer join date |
| CUSTOMER_SEGMENT | VARCHAR(30) | Customer segment |
| EMAIL | VARCHAR(100) | Email |
| PHONE | VARCHAR(20) | Phone |

## RAW.CUSTOMER_INTERACTIONS

| Column | Type | Description |
|---|---|---|
| INTERACTION_ID | VARCHAR(20) | Interaction identifier |
| CUSTOMER_ID | VARCHAR(20) | Related customer |
| INTERACTION_DATE | TIMESTAMP_NTZ(9) | Interaction timestamp |
| CHANNEL | VARCHAR(20) | Interaction channel |
| INTERACTION_TYPE | VARCHAR(50) | Interaction type |
| AGENT_ID | VARCHAR(20) | Agent identifier |
| CALL_DURATION_SEC | NUMBER(38,0) | Call duration in seconds |
| TRANSCRIPT | VARCHAR(5000) | Interaction transcript |

## RAW.POLICIES

| Column | Type | Description |
|---|---|---|
| POLICY_ID | VARCHAR(20) | Policy identifier |
| CUSTOMER_ID | VARCHAR(20) | Related customer |
| POLICY_TYPE | VARCHAR(30) | Policy type |
| POLICY_START_DATE | DATE | Policy start |
| POLICY_END_DATE | DATE | Policy end |
| PREMIUM_AMOUNT | NUMBER(12,2) | Premium amount |
| COVERAGE_AMOUNT | NUMBER(14,2) | Coverage amount |
| POLICY_STATUS | VARCHAR(20) | Policy status |
| PAYMENT_FREQUENCY | VARCHAR(20) | Payment frequency |

## RAW.PAYMENTS

| Column | Type | Description |
|---|---|---|
| PAYMENT_ID | VARCHAR(20) | Payment identifier |
| CUSTOMER_ID | VARCHAR(20) | Related customer |
| POLICY_ID | VARCHAR(20) | Related policy |
| DUE_DATE | DATE | Payment due date |
| PAYMENT_DATE | DATE | Payment date |
| AMOUNT | NUMBER(12,2) | Payment amount |
| PAYMENT_STATUS | VARCHAR(20) | Payment status |
| PAYMENT_METHOD | VARCHAR(30) | Payment method |

## AI.INTERACTION_INSIGHTS

| Column | Type | Description |
|---|---|---|
| INTERACTION_ID | VARCHAR(20) | Related interaction |
| CUSTOMER_ID | VARCHAR(20) | Related customer |
| SENTIMENT_SCORE | FLOAT | AI sentiment score |
| SENTIMENT_LABEL | VARCHAR(20) | AI sentiment label |
| INTENT | VARCHAR(50) | AI-detected intent |
| URGENCY | VARCHAR(20) | AI-detected urgency |
| CANCELLATION_INTENT | BOOLEAN | Cancellation signal |
| INTERACTION_SUMMARY | VARCHAR(1000) | AI interaction summary |
| PROCESSED_AT | TIMESTAMP_NTZ(9) | Insight processing timestamp |
| ISSUE_RELATIONSHIP | VARCHAR(30) | Relationship to an issue/lifecycle |
| RESOLUTION_STATUS | VARCHAR(20) | Resolution state |
| RESOLVES_INTERACTION_ID | VARCHAR(20) | Interaction resolved by this interaction |
| RESOLUTION_SUMMARY | VARCHAR(1000) | AI resolution summary |
