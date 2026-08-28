# Insure360 AI – Metric Catalog

| Metric | Business Definition | Primary Source |
|---|---|---|
| TOTAL_CUSTOMERS | Number of distinct customers | CUSTOMER_ID |
| HIGH_RISK_CUSTOMERS | Customers with HIGH risk | RISK_LEVEL |
| MEDIUM_RISK_CUSTOMERS | Customers with MEDIUM risk | RISK_LEVEL |
| LOW_RISK_CUSTOMERS | Customers with LOW risk | RISK_LEVEL |
| AVERAGE_RISK_SCORE | Average customer risk score | RISK_SCORE |
| TOTAL_PREMIUM_AMOUNT | Total portfolio premium | TOTAL_PREMIUM |
| PREMIUM_AT_RISK | Premium belonging to HIGH-risk customers | TOTAL_PREMIUM + RISK_LEVEL |
| CUSTOMERS_WITH_CANCELLATION_INTENT | Customers with >=1 cancellation signal | CANCELLATION_SIGNALS |
| CUSTOMERS_WITH_OPEN_CLAIMS | Customers with unresolved claims | OPEN_CLAIMS |
| CUSTOMERS_WITH_OPEN_COMPLAINTS | Customers with unresolved complaints | OPEN_COMPLAINTS |
| CLAIM_ESCALATION_CUSTOMERS | Customers with NEXT_BEST_ACTION = CLAIM_ESCALATION | NEXT_BEST_ACTION |
| RETENTION_CUSTOMERS | Customers with NEXT_BEST_ACTION = RETENTION_CALL | NEXT_BEST_ACTION |
| CROSS_SELL_OPPORTUNITIES | Customers with NEXT_BEST_ACTION = CROSS_SELL | NEXT_BEST_ACTION |

## Governance Rules
- Do not redefine a metric differently in individual prompts.
- Use the semantic layer as the source of truth.
- Customer-level metrics must respect the one-row-per-customer semantic source.
- Risk classification must use the documented thresholds.
