# Insure360 — Metric Catalog

| Metric | Definition |
|---|---|
| Total Customers | `COUNT(DISTINCT CUSTOMER_ID)` |
| High Risk Customers | Count where `RISK_LEVEL = 'HIGH'` |
| Medium Risk Customers | Count where `RISK_LEVEL = 'MEDIUM'` |
| Low Risk Customers | Count where `RISK_LEVEL = 'LOW'` |
| Average Risk Score | `AVG(RISK_SCORE)` |
| Total Premium Amount | `SUM(TOTAL_PREMIUM)` |
| Premium at Risk | Sum of total premium for high-risk customers in the semantic metric definition |
| Customers With Cancellation Intent | Count where `CANCELLATION_SIGNALS > 0` |
| Customers With Open Claims | Count where `OPEN_CLAIMS > 0` |
| Customers With Open Complaints | Count where `OPEN_COMPLAINTS > 0` |
| Claim Escalation Customers | Count where next best action is `CLAIM_ESCALATION` |
| Retention Customers | Count where next best action is `RETENTION_CALL` |
| Cross-sell Opportunities | Count where next best action is `CROSS_SELL` |
| Total Estimated Premium at Risk | `SUM(ESTIMATED_PREMIUM_AT_RISK)` |
| Service Recovery Customers | Count where action is `SERVICE_RECOVERY` |
| Payment Reminder Customers | Count where action is `PAYMENT_REMINDER` |
| Renewal Outreach Customers | Count where action is `RENEWAL_OUTREACH` |
| Rapidly Deteriorating Customers | Count where health is `RAPIDLY_DETERIORATING` |
| Deteriorating Customers | Count where health is `DETERIORATING` |
| Customers Requiring Critical Action | Count where `ACTION_PRIORITY = 'CRITICAL'` |
| Customers Requiring High Priority Action | Count where `ACTION_PRIORITY = 'HIGH'` |
| Interaction Risk Velocity | Recent negative + recent high urgency + 2 × recent cancellation signals |
