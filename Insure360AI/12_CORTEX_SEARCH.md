# Insure360 — Cortex Search / Retrieval Design

## Purpose
The supplied schema contains interaction transcripts in `RAW.CUSTOMER_INTERACTIONS.TRANSCRIPT` and AI-generated interaction summaries in `AI.INTERACTION_INSIGHTS.INTERACTION_SUMMARY` and `RESOLUTION_SUMMARY`.

These fields are natural candidates for semantic retrieval.

## Recommended searchable content

Primary:
- `CUSTOMER_INTERACTIONS.TRANSCRIPT`

Supporting:
- `INTERACTION_SUMMARY`
- `RESOLUTION_SUMMARY`
- `CLAIM_DESCRIPTION`

## Recommended metadata filters
- `CUSTOMER_ID`
- `INTERACTION_ID`
- `INTERACTION_DATE`
- `CHANNEL`
- `INTERACTION_TYPE`
- `AGENT_ID`
- `SENTIMENT_LABEL`
- `INTENT`
- `URGENCY`
- `CANCELLATION_INTENT`
- `ISSUE_RELATIONSHIP`
- `RESOLUTION_STATUS`

## Example retrieval questions
- Find prior interactions for a customer mentioning a claim delay.
- Find interactions with cancellation language.
- Find prior unresolved issues related to the current complaint.
- Find resolution summaries for similar claims.
- Find high-urgency interactions for a customer.

## Important implementation note
No Cortex Search service definition was included in the supplied SQL. This document therefore specifies the retrieval design rather than claiming that a Cortex Search service already exists.
