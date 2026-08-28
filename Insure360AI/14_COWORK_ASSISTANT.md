# Insure360 AI – CoWork Assistant Specification

## Purpose
Use CoCo/CoWork as an AI-assisted development companion for Insure360 AI.

## Recommended Development Tasks

### Data
- Generate synthetic customer/policy/claim/payment/complaint data.
- Validate row counts and relationships.
- Identify duplicate customer or policy IDs.

### SQL
- Generate and refine Customer 360 SQL.
- Validate aggregations.
- Test risk scoring logic.
- Validate Next Best Action rules.

### Semantic Layer
- Review dimensions and facts.
- Refine metric descriptions.
- Generate candidate business questions.
- Validate semantic mappings.

### AI
- Develop transcript analysis prompts/SQL.
- Refine sentiment and cancellation signal extraction.
- Test edge cases.

### Testing
Ask CoWork to:
- generate test SQL;
- identify join duplication;
- compare governed metric definitions;
- create regression queries;
- inspect explainability of risk output.

## Suggested CoWork Prompts

```text
Review VW_CUSTOMER_360 for double counting caused by one-to-many joins.

Generate validation SQL proving that TOTAL_PREMIUM only includes active policies.

Check whether every high-risk customer has at least one documented risk signal.

Generate test cases for all Next Best Action categories.

Review the semantic source and identify ambiguous metric definitions.

Generate natural-language business questions that can be answered from SV_CUSTOMER_360.
```

## Important
CoWork should assist development and refinement; the final business definitions remain governed by the project specification.
