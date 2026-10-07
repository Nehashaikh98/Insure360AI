# Insure360 — Build / Deployment Guide

## 1. Prerequisites
- Snowflake database `INSURE360_DB`
- Schemas `RAW`, `AI`, and `ANALYTICS`
- Permissions to create tables/views/semantic views in the target schemas

## 2. Apply table DDL
Run the table definitions in `06_DDL_SPEC.sql`.

The supplied request contained `CUSTOMER_INTERACTIONS` twice. The build file intentionally defines it once.

## 3. Apply analytical views
Run the `CREATE OR REPLACE VIEW` statements after the raw and AI tables exist.

Dependency order:
1. `VW_CUSTOMER_360`
2. `VW_CUSTOMER_INTELLIGENCE`
3. `VW_CUSTOMER_RISK`
4. `VW_NEXT_BEST_ACTION`
5. `VW_INTERACTION_RECOVERY`

The semantic view depends on `VW_NEXT_BEST_ACTION`.

## 4. Apply semantic view
Create `SV_CUSTOMER_360` after `VW_NEXT_BEST_ACTION`.

## 5. Validation checks

### Structural
```sql
SHOW TABLES IN SCHEMA INSURE360_DB.RAW;
SHOW TABLES IN SCHEMA INSURE360_DB.AI;
SHOW VIEWS IN SCHEMA INSURE360_DB.ANALYTICS;
```

### Row-level sanity
```sql
SELECT COUNT(*) FROM INSURE360_DB.RAW.CUSTOMERS;
SELECT COUNT(*) FROM INSURE360_DB.RAW.POLICIES;
SELECT COUNT(*) FROM INSURE360_DB.RAW.CLAIMS;
SELECT COUNT(*) FROM INSURE360_DB.RAW.COMPLAINTS;
SELECT COUNT(*) FROM INSURE360_DB.RAW.PAYMENTS;
SELECT COUNT(*) FROM INSURE360_DB.RAW.CUSTOMER_INTERACTIONS;
SELECT COUNT(*) FROM INSURE360_DB.AI.INTERACTION_INSIGHTS;
```

### Customer 360
```sql
SELECT *
FROM INSURE360_DB.ANALYTICS.VW_CUSTOMER_360
LIMIT 20;
```

### Risk
```sql
SELECT RISK_LEVEL, COUNT(*)
FROM INSURE360_DB.ANALYTICS.VW_CUSTOMER_RISK
GROUP BY RISK_LEVEL;
```

### Next best action
```sql
SELECT NEXT_BEST_ACTION, COUNT(*)
FROM INSURE360_DB.ANALYTICS.VW_NEXT_BEST_ACTION
GROUP BY NEXT_BEST_ACTION
ORDER BY COUNT(*) DESC;
```

### Semantic layer
Validate that `SV_CUSTOMER_360` can expose its declared facts, dimensions, and metrics through the target Snowflake semantic-view tooling.

## 6. Source fidelity note
The analytical SQL in this package is preserved from the supplied view-definition text. The documentation does not silently alter its business rules.
