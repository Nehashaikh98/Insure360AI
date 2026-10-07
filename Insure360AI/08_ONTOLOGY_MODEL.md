# Insure360 — Ontology Model

## Entity concepts

- **Customer** — central business entity.
- **Policy** — insurance relationship owned by a customer.
- **Claim** — financial/service event against a policy.
- **Complaint** — customer service issue, optionally related to a claim.
- **Payment** — monetary transaction against a customer/policy.
- **Interaction** — customer contact event.
- **Interaction Insight** — AI interpretation of an interaction.
- **Customer Health** — derived condition based on recent AI interaction signals.
- **Customer Risk** — derived risk score and level.
- **Intervention** — recommended action and service level.
- **Premium Exposure** — total premium and estimated premium at risk.

## Relationships

```text
Customer
 ├─ owns → Policy
 │          ├─ receives → Payment
 │          └─ generates → Claim
 │                         └─ may relate to → Complaint
 ├─ raises → Complaint
 └─ has → Interaction
            └─ enriched by → Interaction Insight

Customer
 └─ aggregates into → Customer 360
                       ├─ Health
                       ├─ Risk
                       └─ Next Best Action
```

## Semantic vocabulary
The semantic layer exposes the customer entity and selected measures/dimensions from `VW_NEXT_BEST_ACTION`, including risk, premium exposure, customer health, intervention action, and action priority.
