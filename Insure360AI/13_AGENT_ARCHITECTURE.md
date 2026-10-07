# Insure360 — Agent Architecture

## Goal
Provide an AI assistant with governed access to customer intelligence, risk, recovery context, and next-best-action outputs.

## Suggested logical architecture

```text
User
  |
  v
Agent / Co-work Assistant
  |
  +--> Semantic layer: SV_CUSTOMER_360
  |
  +--> Analytical views:
  |      VW_NEXT_BEST_ACTION
  |      VW_CUSTOMER_RISK
  |      VW_CUSTOMER_INTELLIGENCE
  |      VW_CUSTOMER_360
  |      VW_INTERACTION_RECOVERY
  |
  +--> Retrieval layer:
         Interaction transcripts / summaries
```

## Agent responsibilities
1. Identify the customer or customer cohort.
2. Retrieve structured customer metrics from the semantic/analytical layer.
3. Retrieve supporting interaction context when needed.
4. Explain risk and recommended action using the supplied business rules.
5. Distinguish observed facts from recommendations.
6. Avoid inventing missing customer, claim, policy, or interaction details.

## High-value workflows
- Customer health review
- Retention prioritization
- Claim escalation triage
- Service recovery
- Payment follow-up
- Renewal outreach
- Executive portfolio risk analysis

## Guardrails
- Respect row-level/customer-level access controls configured outside these definitions.
- Treat `NEXT_BEST_ACTION` as a rule-based recommendation, not an autonomous commitment.
- Surface the reason fields when explaining a recommendation.
- Use current source data when available.
