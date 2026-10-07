# Insure360 — Master / Reference Data

The supplied DDL contains descriptive/status fields but no separate reference-data tables. The following values are therefore documented as **code/value conventions inferred from the supplied analytical SQL**, not as authoritative master-data tables.

## Status conventions used by views

### Claim status
Open claim logic:
- `OPEN`
- `PENDING`
- `IN_PROGRESS`

### Complaint status
Open complaint logic:
- `OPEN`
- `PENDING`
- `IN_PROGRESS`

### Payment status
Overdue payment logic:
- `OVERDUE`
- `LATE`
- `PENDING`

### Complaint priority
High-priority complaint logic:
- `HIGH`
- `CRITICAL`

### Interaction sentiment
Negative interaction logic:
- `NEGATIVE`

### Interaction urgency
High urgency logic:
- `HIGH`

### AI intents used by customer intelligence
- `CLAIM_ESCALATION`
- `PAYMENT_ISSUE`

### Customer health status
- `RAPIDLY_DETERIORATING`
- `DETERIORATING`
- `WATCH`
- `STABLE`

### Risk level
- `HIGH`
- `MEDIUM`
- `LOW`

### Intervention tier
- `IMMEDIATE`
- `HIGH`
- `MEDIUM`
- `LOW`

### Next-best-action values
- `CLAIM_ESCALATION`
- `RETENTION_CALL`
- `SERVICE_RECOVERY`
- `PAYMENT_REMINDER`
- `RENEWAL_OUTREACH`
- `CROSS_SELL`
- `NO_IMMEDIATE_ACTION`

### Recommended channels
- `PHONE`
- `EMAIL`
- `PHONE_OR_EMAIL`
- `NO_CONTACT_REQUIRED`
