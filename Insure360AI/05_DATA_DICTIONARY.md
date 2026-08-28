# Insure360 AI – Data Dictionary

## RAW.CUSTOMERS
| Column | Type | Description |
|---|---|---|
| CUSTOMER_ID | VARCHAR | Unique identifier |
| CUSTOMER_NAME | VARCHAR | Full customer name |
| AGE | NUMBER | Customer age |
| CITY | VARCHAR | Customer city/location |
| CUSTOMER_SEGMENT | VARCHAR | Business/value segment such as Premium or Standard |
| JOIN_DATE | DATE | Date customer joined insurer |
| EMAIL | VARCHAR | Customer email |
| PHONE | VARCHAR | Customer contact number |

## RAW.POLICIES
| Column | Type | Description |
|---|---|---|
| POLICY_ID | VARCHAR | Unique policy identifier |
| CUSTOMER_ID | VARCHAR | Customer owning policy |
| POLICY_TYPE | VARCHAR | Type of insurance policy |
| PREMIUM_AMOUNT | NUMBER(12,2) | Annual premium |
| START_DATE | DATE | Policy start date |
| END_DATE | DATE | Policy end/renewal date |
| POLICY_STATUS | VARCHAR | ACTIVE, LAPSED or CANCELLED |

## RAW.CLAIMS
| Column | Type | Description |
|---|---|---|
| CLAIM_ID | VARCHAR | Unique claim identifier |
| POLICY_ID | VARCHAR | Associated policy |
| CUSTOMER_ID | VARCHAR | Associated customer |
| CLAIM_DATE | DATE | Claim submission date |
| CLAIM_AMOUNT | NUMBER(12,2) | Amount requested |
| CLAIM_STATUS | VARCHAR | OPEN, APPROVED, REJECTED or CLOSED |
| CLAIM_REASON | VARCHAR | Claim category/reason |

## RAW.COMPLAINTS
| Column | Type | Description |
|---|---|---|
| COMPLAINT_ID | VARCHAR | Unique complaint identifier |
| CUSTOMER_ID | VARCHAR | Customer |
| COMPLAINT_DATE | DATE | Complaint date |
| COMPLAINT_TYPE | VARCHAR | Complaint category |
| COMPLAINT_STATUS | VARCHAR | OPEN, IN_PROGRESS or RESOLVED |
| DESCRIPTION | VARCHAR | Complaint description |

## RAW.PAYMENTS
| Column | Type | Description |
|---|---|---|
| PAYMENT_ID | VARCHAR | Unique payment identifier |
| POLICY_ID | VARCHAR | Associated policy |
| CUSTOMER_ID | VARCHAR | Associated customer |
| DUE_DATE | DATE | Payment due date |
| PAYMENT_DATE | DATE | Actual payment date |
| AMOUNT | NUMBER(12,2) | Payment amount |
| PAYMENT_STATUS | VARCHAR | PAID, PENDING or OVERDUE |

## RAW.INTERACTIONS
| Column | Type | Description |
|---|---|---|
| INTERACTION_ID | VARCHAR | Unique interaction identifier |
| CUSTOMER_ID | VARCHAR | Customer |
| INTERACTION_DATE | TIMESTAMP | Interaction date/time |
| CHANNEL | VARCHAR | CALL, EMAIL or CHAT |
| TRANSCRIPT | VARCHAR | Unstructured interaction transcript |
| AGENT_NAME | VARCHAR | Customer-service agent |
| DURATION_MINUTES | NUMBER | Interaction duration |

## AI.INTERACTION_INSIGHTS
| Column | Description |
|---|---|
| INTERACTION_ID | Source interaction |
| CUSTOMER_ID | Related customer |
| SENTIMENT_SCORE | AI-derived sentiment score |
| SENTIMENT | AI-derived sentiment |
| AI_CLASSIFICATION | Classification returned by AI_CLASSIFY |
| CANCELLATION_SIGNAL | Cancellation/churn intent indicator |
| URGENCY | AI-derived urgency |

## Analytical Outputs
`VW_CUSTOMER_360`, `VW_CUSTOMER_INTELLIGENCE`, `VW_NEXT_BEST_ACTION`, `VW_CUSTOMER_DECISIONS`, and `VW_CUSTOMER_SEMANTIC_SOURCE` are customer-grain analytical objects.
