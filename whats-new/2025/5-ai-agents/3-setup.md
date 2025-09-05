# Snowflake AI Agents — Setup

Setup steps, enabling features, and security considerations.


## Enable AI Agents (example)
Some features are gated by account parameters or previews. Check availability and enable if required.

```sql
show parameters like 'ENABLE_AI_AGENTS';
alter account set ENABLE_AI_AGENTS = true;
```

## Example: create service role for agents
```sql
create role ai_agent_runner;
grant usage on warehouse ai_wh to role ai_agent_runner;
grant usage on database analytics to role ai_agent_runner;
```

---

[Previous](./2-intro.md) • [Next](./4-usage-and-scenarios.md)
