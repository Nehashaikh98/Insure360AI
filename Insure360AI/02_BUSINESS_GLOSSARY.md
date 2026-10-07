# Insure360 — Business Glossary

| Term | Definition / source behavior |
|---|---|
| Customer | A record in `RAW.CUSTOMERS`, identified by `CUSTOMER_ID`. |
| Policy | Insurance contract represented by `RAW.POLICIES`, identified by `POLICY_ID`. |
| Claim | Insurance claim represented by `RAW.CLAIMS`, identified by `CLAIM_ID`. |
| Complaint | Customer complaint represented by `RAW.COMPLAINTS`, identified by `COMPLAINT_ID`. |
| Payment | Policy/customer payment represented by `RAW.PAYMENTS`, identified by `PAYMENT_ID`. |
| Customer interaction | Customer contact represented by `RAW.CUSTOMER_INTERACTIONS`, identified by `INTERACTION_ID`. |
| Interaction insight | AI-derived sentiment, intent, urgency, cancellation, and resolution attributes in `AI.INTERACTION_INSIGHTS`. |
| Open claim | A claim whose status is `OPEN`, `PENDING`, or `IN_PROGRESS`. |
| Open complaint | A complaint whose status is `OPEN`, `PENDING`, or `IN_PROGRESS`. |
| Overdue payment | A payment whose status is `OVERDUE`, `LATE`, or `PENDING` in the supplied view logic. |
| Cancellation signal | An AI interaction where `CANCELLATION_INTENT` is true. |
| High-urgency interaction | An AI interaction whose `URGENCY` is `HIGH`. |
| Negative interaction | An AI interaction whose `SENTIMENT_LABEL` is `NEGATIVE`. |
| Interaction risk velocity | `recent negative + recent high urgency + 2 × recent cancellation signals`. |
| Customer health status | `RAPIDLY_DETERIORATING`, `DETERIORATING`, `WATCH`, or `STABLE`, based on recent AI signals. |
| Risk score | A bounded 0–100 customer risk score built from customer-level risk indicators. |
| Risk level | `HIGH` for score ≥ 60, `MEDIUM` for 30–59, otherwise `LOW`. |
| Intervention score | Risk score plus additional health, recent-interaction, cancellation, and renewal pressure points, bounded at 100. |
| Intervention tier | `IMMEDIATE` ≥ 80, `HIGH` ≥ 60, `MEDIUM` ≥ 30, otherwise `LOW`. |
| Next best action | Rule-based recommended customer action such as claim escalation, retention call, service recovery, payment reminder, renewal outreach, or cross-sell. |
| Estimated premium at risk | Business-rule estimate: 100% of premium for high risk, 50% for medium risk, 10% otherwise. |
| Sentiment trend | Comparison of current interaction sentiment score to the previous customer interaction. |
| Issue resolved | Boolean flag derived from `RESOLUTION_STATUS = 'RESOLVED'`. |
