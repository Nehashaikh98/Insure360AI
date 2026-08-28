# Insure360 AI – Multi-Hop Questions

These questions are designed to demonstrate reasoning across multiple Customer 360 domains.

## Hop 1 – Customer → Policy → Renewal
> Which customers have active policies renewing within 30 days?

Expected path:
Customer → Policy → Renewal date.

## Hop 2 – Customer → Claim → Service Risk
> Which customers have open claims and are high risk?

Expected path:
Customer → Claim → Risk.

## Hop 3 – Customer → Interaction → Cancellation → Risk
> Which customers are showing cancellation intent and why are they high risk?

Expected path:
Customer → Interaction → Interaction Insight → Cancellation Signal → Risk Reason.

## Hop 4 – Customer → Complaint → Interaction → Service Recovery
> Which customers have open complaints and recent negative interactions?

Expected path:
Customer → Complaint + Interaction → Sentiment.

## Hop 5 – Customer → Payment → Risk → Action
> Which customers have overdue payments and what should we do next?

Expected path:
Customer → Payment → Risk → Next Best Action.

## Hop 6 – Customer → Claim → Interaction → Cancellation → Action
> Which customers have unresolved claims, recent cancellation intent and should receive a claim escalation?

Expected path:
Customer → Claim → Interaction → Cancellation Signal → Next Best Action.

## Hop 7 – Portfolio → Risk → Premium
> How much premium is at risk and which customers contribute most?

Expected path:
Portfolio → Customer → Risk → Premium.

## Hop 8 – Customer → Product Opportunity
> Which low-risk customers with only one active policy are cross-sell opportunities?

Expected path:
Customer → Policies → Risk → Next Best Action.

## Judge Demonstration
Use one complex question, then ask:
1. Why?
2. What evidence supports that?
3. What should the business do next?

This demonstrates Customer 360 → AI intelligence → explainable risk → action.
