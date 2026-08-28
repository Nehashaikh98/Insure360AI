# Insure360 AI – Cortex Search Specification

## Purpose
Provide retrieval over unstructured customer interaction content when the application needs evidence from transcripts.

## Recommended Source
`INSURE360_DB.RAW.INTERACTIONS`

Primary searchable fields:
- CUSTOMER_ID
- INTERACTION_ID
- INTERACTION_DATE
- CHANNEL
- TRANSCRIPT

## Example Retrieval Questions
- What did the customer say about cancellation?
- Find recent interactions mentioning unresolved claims.
- Show interactions with complaints about service quality.
- Find the latest conversation for customer CUST-1001.

## Search Metadata
Use customer and interaction metadata as filters:
- CUSTOMER_ID
- CHANNEL
- INTERACTION_DATE

## AI Enrichment
Interaction transcripts should also be processed into:
- SENTIMENT
- SENTIMENT_SCORE
- AI_CLASSIFICATION
- CANCELLATION_SIGNAL
- URGENCY

## Design Principle
Use Cortex Search for transcript evidence/retrieval. Use the governed customer semantic layer for customer-level analytical metrics.
