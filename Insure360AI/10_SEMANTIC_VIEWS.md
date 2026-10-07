# Insure360 — Semantic Views

## Analytical views

### `VW_CUSTOMER_360`
Customer-level operational aggregation across customers, policies, claims, complaints, and payments.

It provides:
- Policy counts and premium
- Next policy end date
- Claim counts and amounts
- Complaint counts and priority
- Payment counts and overdue amounts
- Days to renewal

### `VW_CUSTOMER_INTELLIGENCE`
Extends `VW_CUSTOMER_360` with AI interaction intelligence:
- Sentiment
- Intent
- Urgency
- Cancellation signals
- Recent negative/high-urgency/cancellation counts
- Interaction risk velocity
- Customer health status

The source SQL deduplicates raw interactions by latest `INTERACTION_DATE` and AI insights by latest `PROCESSED_AT` per interaction.

### `VW_CUSTOMER_RISK`
Adds a 0–100 customer risk score, risk level, and human-readable risk reason.

Risk components encoded in the supplied SQL:
- Open claim: +20
- Open complaint: +15
- Overdue payment: +10
- Negative interaction: +15
- Cancellation signal: +30
- High urgency: +10
- Health deterioration: +5 / +10 / +15 depending on state

The score is capped at 100.

### `VW_NEXT_BEST_ACTION`
Combines customer intelligence and risk to produce:
- Intervention score/tier
- Estimated premium at risk
- Next best action
- Action type
- Priority
- Recommended channel
- SLA
- Action reason

### `VW_INTERACTION_RECOVERY`
Provides interaction lifecycle context:
- Previous interaction
- Previous sentiment
- Sentiment trend
- Issue relationship
- Resolution status
- Resolution flag
- Human-readable resolution reason

### `SV_CUSTOMER_360`
Semantic view built over `VW_NEXT_BEST_ACTION`.

It exposes:
- Customer dimensions
- Risk and intervention facts
- Business metrics for portfolio-level questions

See `06_DDL_SPEC.sql` for the supplied SQL definitions.
