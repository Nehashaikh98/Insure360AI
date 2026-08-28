# Insure360 AI – Semantic View Specification

## Semantic View
`INSURE360_DB.ANALYTICS.SV_CUSTOMER_360`

### Source
`INSURE360_DB.ANALYTICS.VW_CUSTOMER_SEMANTIC_SOURCE`

### Grain
One row per customer.

## Dimensions
- CUSTOMER_ID – unique customer
- CUSTOMER_NAME – customer full name
- CUSTOMER_SEGMENT – customer business segment
- RISK_LEVEL – customer risk category
- RISK_REASON – risk explanation
- NEXT_BEST_ACTION – recommended action
- ACTION_REASON – recommendation explanation

## Facts
- RISK_SCORE
- TOTAL_PREMIUM
- OPEN_CLAIMS
- OPEN_COMPLAINTS
- OVERDUE_PAYMENTS
- DAYS_TO_RENEWAL
- ACTIVE_POLICIES
- NEGATIVE_INTERACTIONS
- CANCELLATION_SIGNALS
- HIGH_URGENCY_INTERACTIONS

## Governed Metrics

### TOTAL_CUSTOMERS
Number of distinct customers.

### HIGH_RISK_CUSTOMERS
Customers with RISK_LEVEL = HIGH.

### MEDIUM_RISK_CUSTOMERS
Customers with RISK_LEVEL = MEDIUM.

### LOW_RISK_CUSTOMERS
Customers with RISK_LEVEL = LOW.

### AVERAGE_RISK_SCORE
Average customer risk score.

### TOTAL_PREMIUM_AMOUNT
Total portfolio premium.

### PREMIUM_AT_RISK
Premium belonging to HIGH-risk customers.

### CUSTOMERS_WITH_CANCELLATION_INTENT
Customers having at least one cancellation signal.

### CUSTOMERS_WITH_OPEN_CLAIMS
Customers having unresolved claims.

### CUSTOMERS_WITH_OPEN_COMPLAINTS
Customers having unresolved complaints.

### CLAIM_ESCALATION_CUSTOMERS
Customers recommended for claim escalation.

### RETENTION_CUSTOMERS
Customers recommended for retention calls.

### CROSS_SELL_OPPORTUNITIES
Customers recommended for cross-sell.

## Validation Questions
1. How many customers are high risk?
2. How much premium is at risk?
3. Which customers should we prioritize?
4. Which customers are showing cancellation intent?
5. Which customers require claim escalation?
6. Which customers need retention calls?
7. What are our cross-sell opportunities?
8. Why is a particular customer high risk?

## Implementation Note
Create the Snowflake Semantic View using the account's currently supported semantic-view DDL/API. Keep the business definitions above as the authoritative specification.
