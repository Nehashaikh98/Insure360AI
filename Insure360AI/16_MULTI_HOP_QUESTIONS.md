# Insure360 — Multi-hop Questions

These questions intentionally require the assistant to connect multiple entities, analytical views, and/or retrieval context.

1. **Cancellation + claim + health**
   - Find customers with cancellation signals.
   - Check whether they have open claims.
   - Check customer health.
   - Return customers needing immediate claim escalation.

2. **Complaint + interaction evidence**
   - Find open complaints.
   - Join to negative interactions.
   - Retrieve supporting transcript/summary context.
   - Recommend service recovery.

3. **Payment + retention**
   - Find overdue payments.
   - Check customer risk and cancellation signals.
   - Prioritize payment reminders for customers with elevated risk.

4. **Renewal + risk**
   - Find policies expiring within 30 days.
   - Filter to low-risk customers.
   - Recommend renewal outreach.

5. **Interaction lifecycle**
   - Find a recent interaction.
   - Identify its previous interaction.
   - Compare sentiment.
   - Determine whether the issue was resolved.

6. **Premium exposure**
   - Aggregate total premium.
   - Calculate estimated premium at risk from the next-best-action view.
   - Break exposure down by risk level and recommended intervention.

7. **Executive service recovery**
   - Count customers with open complaints and negative interactions.
   - Compare with rapidly deteriorating customers.
   - Identify the share requiring critical/high-priority action.

8. **Claim escalation prioritization**
   - Find open claims.
   - Check high-urgency interaction signals.
   - Check cancellation signals and health deterioration.
   - Rank by intervention score.
