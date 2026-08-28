# Insure360 AI – Domain Model

## Business Domains

### Customer
The customer is the central business entity.

### Policy
Insurance coverage held by a customer.

### Claim
A claim submitted against a policy.

### Complaint
A customer service issue raised by a customer.

### Payment
Premium payment associated with a policy.

### Interaction
Customer-service interaction containing structured metadata and unstructured transcript content.

### Interaction Intelligence
AI-derived interpretation of interaction transcripts.

### Customer Risk
Explainable customer-level risk calculated from structured and AI-derived signals.

### Next Best Action
Recommended business action derived from customer intelligence and risk.

## Domain Relationships

```text
CUSTOMER
  |
  +----< POLICY
  |        |
  |        +----< CLAIM
  |        |
  |        +----< PAYMENT
  |
  +----< COMPLAINT
  |
  +----< INTERACTION
              |
              v
      INTERACTION_INSIGHTS
              |
              v
        CUSTOMER RISK
              |
              v
       NEXT BEST ACTION
```

## Analytical Grain
The final decision and semantic source layers are one row per customer.
