# Insure360 — ER Model

## Logical ER model

```text
CUSTOMERS
   |
   +----< POLICIES
   |        |
   |        +----< PAYMENTS
   |        |
   |        +----< CLAIMS
   |                 |
   |                 +----< COMPLAINTS
   |
   +----< COMPLAINTS
   |
   +----< CUSTOMER_INTERACTIONS
               |
               +----< INTERACTION_INSIGHTS
```

## Key joins

| Parent | Child | Join |
|---|---|---|
| `CUSTOMERS` | `POLICIES` | `CUSTOMER_ID` |
| `CUSTOMERS` | `CLAIMS` | `CUSTOMER_ID` |
| `POLICIES` | `CLAIMS` | `POLICY_ID` |
| `CUSTOMERS` | `COMPLAINTS` | `CUSTOMER_ID` |
| `CLAIMS` | `COMPLAINTS` | `CLAIM_ID = RELATED_CLAIM_ID` |
| `CUSTOMERS` | `PAYMENTS` | `CUSTOMER_ID` |
| `POLICIES` | `PAYMENTS` | `POLICY_ID` |
| `CUSTOMERS` | `CUSTOMER_INTERACTIONS` | `CUSTOMER_ID` |
| `CUSTOMER_INTERACTIONS` | `INTERACTION_INSIGHTS` | `INTERACTION_ID` |

The supplied DDL does not declare database-level primary/foreign-key constraints; the keys above are logical relationships used by the analytical SQL.
