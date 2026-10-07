# Insure360 — Project Overview

## Purpose
Insure360 is a customer-centric insurance analytics and AI decision-support model. The supplied database design combines customer, policy, claim, complaint, payment, and interaction data with AI-derived interaction insights.

The analytical layer is designed to support:
- Customer 360 views
- Customer health and interaction intelligence
- Customer risk scoring
- Issue/recovery tracking across interactions
- Next-best-action recommendations
- Executive KPI and portfolio-level analysis
- Natural-language analytics through a semantic view

## Database domains

| Database / Schema | Role |
|---|---|
| `INSURE360_DB.RAW` | Source operational/customer data |
| `INSURE360_DB.AI` | AI-enriched interaction intelligence |
| `INSURE360_DB.ANALYTICS` | Curated customer analytics, decision views, and semantic model |

## Core flow

`RAW tables → VW_CUSTOMER_360 → VW_CUSTOMER_INTELLIGENCE → VW_CUSTOMER_RISK → VW_NEXT_BEST_ACTION → SV_CUSTOMER_360`

Interaction recovery is exposed separately through `VW_INTERACTION_RECOVERY`.

## Source scope
This documentation is generated from the table definitions supplied in the request and the supplied database-view SQL text. Where a business definition was not explicitly provided, the documentation describes the behavior encoded in the supplied SQL rather than inventing an external business rule.

## Key design principles
1. Keep raw source attributes close to their operational meaning.
2. Deduplicate interaction and AI-insight records before customer-level aggregation.
3. Centralize customer-level aggregation in `VW_CUSTOMER_360`.
4. Add AI-derived interaction signals in `VW_CUSTOMER_INTELLIGENCE`.
5. Convert signals into a bounded risk score in `VW_CUSTOMER_RISK`.
6. Convert risk and health signals into intervention actions in `VW_NEXT_BEST_ACTION`.
7. Expose the resulting business concepts through `SV_CUSTOMER_360`.
