# Insure360 AI – Agent Architecture

## Agent
`INSURE360_DB.AI.INSURE360_AGENT`

## Purpose
Provides a natural-language conversational interface over governed Customer 360 analytics.

## Core Components
```text
Business User
     |
     v
INSURE360_AGENT
     |
     +--> Cortex Analyst
     |       |
     |       v
     |   SV_CUSTOMER_360
     |       |
     |       v
     |   Governed Metrics
     |
     +--> Optional transcript retrieval
             |
             v
        Cortex Search
             |
             v
        Interaction Evidence
```

## Semantic Model
`INSURE360_DB.ANALYTICS.SV_CUSTOMER_360`

## Semantic Source
`INSURE360_DB.ANALYTICS.VW_CUSTOMER_SEMANTIC_SOURCE`

## Example Questions
- How many customers are high risk?
- How much premium is at risk?
- Which customers should we prioritize?
- Which customers are showing cancellation intent?
- Which customers require claim escalation?
- Which customers need retention calls?
- What are our cross-sell opportunities?
- Why is a particular customer high risk?

## Agent Behavior
1. Interpret the business question.
2. Use governed semantic definitions for analytical questions.
3. Retrieve transcript evidence when the question asks about customer interactions.
4. Clearly distinguish calculated metrics from AI-derived interaction signals.
5. Return concise business explanations.
6. Do not invent customer facts that are absent from Snowflake.

## Guardrails
- No unsupported customer attributes.
- No opaque risk explanation.
- Risk reason should trace to documented signals.
- Next Best Action should come from the governed action logic.
