# Insure360 AI – Ontology Model

## Central Business Entity
`Customer`

## Entities
```text
Customer
Policy
Claim
Payment
Complaint
Interaction
Interaction Insight
Customer Risk
Next Best Action
Customer Decision
```

## Relationships
```text
Customer OWNS Policy
Policy HAS Claim
Policy HAS Payment
Customer RAISES Complaint
Customer HAS Interaction
Interaction PRODUCES Interaction Insight
Customer HAS Risk
Customer RECEIVES Next Best Action
Customer HAS Customer Decision
```

## Ontology Traversal Example

Question:
> Why is customer CUST-1001 high risk?

```text
Customer
  -> Policies
  -> Claims
  -> Complaints
  -> Payments
  -> Interactions
  -> Interaction Insights
  -> Risk Signals
  -> Risk Score
  -> Risk Reason
```

Question:
> Which customers should we prioritize?

```text
Customer
  -> Risk
  -> Next Best Action
  -> Priority
```

## Governance Principle
The ontology provides a consistent business vocabulary. Risk and action definitions should be implemented in governed analytical objects rather than generated ad hoc by the conversational model.
