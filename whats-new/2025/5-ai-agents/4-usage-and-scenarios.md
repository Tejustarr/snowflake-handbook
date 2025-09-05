# Snowflake AI Agents — Usage & Scenarios

Concrete usage patterns and realistic scenarios.


## Use Case: Automated Support Triage
1. Agent listens for new tickets in `raw.support_tickets`.
2. Agent classifies urgency, tags ticket, and writes back to `operational.tickets`.

```sql
-- Agent script (conceptual)
create or replace ai agent ticket_triage as
  using model openai.gpt4
  on table raw.support_tickets
  when new_rows > 0
  do call classify_and_write();
```

---

[Previous](./3-setup.md) • [Next](./5-testing-and-validation.md)
