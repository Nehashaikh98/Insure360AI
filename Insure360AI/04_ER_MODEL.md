# Insure360 AI – ER Model

## Entities

```text
CUSTOMERS
---------
PK CUSTOMER_ID

POLICIES
--------
PK POLICY_ID
FK CUSTOMER_ID

CLAIMS
------
PK CLAIM_ID
FK POLICY_ID
FK CUSTOMER_ID

COMPLAINTS
----------
PK COMPLAINT_ID
FK CUSTOMER_ID

PAYMENTS
--------
PK PAYMENT_ID
FK POLICY_ID
FK CUSTOMER_ID

INTERACTIONS
------------
PK INTERACTION_ID
FK CUSTOMER_ID

INTERACTION_INSIGHTS
--------------------
PK INTERACTION_ID
FK CUSTOMER_ID
```

## Relationships

```text
CUSTOMERS 1 ───── N POLICIES
CUSTOMERS 1 ───── N CLAIMS
POLICIES  1 ───── N CLAIMS

CUSTOMERS 1 ───── N COMPLAINTS
CUSTOMERS 1 ───── N PAYMENTS
POLICIES  1 ───── N PAYMENTS

CUSTOMERS 1 ───── N INTERACTIONS
INTERACTIONS 1 ── 1 INTERACTION_INSIGHTS
```

## Derived Layers

```text
RAW entities
   |
   +--> VW_CUSTOMER_360
   |
   +--> AI.INTERACTION_INSIGHTS
              |
              +--> VW_CUSTOMER_INTELLIGENCE
                         |
                         +--> Risk Engine
                         |
                         +--> VW_NEXT_BEST_ACTION
                                    |
                                    +--> VW_CUSTOMER_DECISIONS
                                               |
                                               +--> VW_CUSTOMER_SEMANTIC_SOURCE
                                                          |
                                                          +--> SV_CUSTOMER_360
```
