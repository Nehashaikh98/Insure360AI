# Insure360 — Domain Model

## Domains

### Customer
`CUSTOMERS` is the master customer entity.

Key attributes:
- `CUSTOMER_ID`
- `CUSTOMER_NAME`
- `CUSTOMER_SEGMENT`
- `CITY`
- `REGION`
- `JOIN_DATE`

### Policy
`POLICIES` represents policies owned by customers.

Relationship:
- Customer 1 → many Policies

### Claims
`CLAIMS` represents claims associated with customers and policies.

Relationships:
- Customer 1 → many Claims
- Policy 1 → many Claims

### Complaints
`COMPLAINTS` represents customer complaints and can optionally reference a claim.

Relationships:
- Customer 1 → many Complaints
- Claim 1 → many Complaints (logical relationship through `RELATED_CLAIM_ID`)

### Payments
`PAYMENTS` represents payments associated with customers and policies.

Relationships:
- Customer 1 → many Payments
- Policy 1 → many Payments

### Customer interactions
`CUSTOMER_INTERACTIONS` represents contacts between customers and service/agent channels.

Relationships:
- Customer 1 → many Interactions
- Agent identity is stored as `AGENT_ID`

### AI interaction insights
`INTERACTION_INSIGHTS` enriches interactions with AI-derived attributes.

Logical relationship:
- Interaction 1 → many insight records over processing time
- Analytical views select the latest insight per `INTERACTION_ID`

## Customer intelligence lifecycle

`Customer`
→ policy/claim/complaint/payment aggregation
→ `VW_CUSTOMER_360`
→ AI interaction aggregation
→ `VW_CUSTOMER_INTELLIGENCE`
→ risk scoring
→ `VW_CUSTOMER_RISK`
→ intervention decisioning
→ `VW_NEXT_BEST_ACTION`
