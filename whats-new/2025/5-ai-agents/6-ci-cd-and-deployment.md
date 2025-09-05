# Snowflake AI Agents — CI/CD & Deployment

Automated deployment patterns, scheduling, and rollback strategies.


## CI Flow for AI Agents
- Validate agent scripts in PR (linting, dry-run).
- Deploy agent definitions via SnowSQL or REST API during merge.

```yaml
# GitHub Action snippet to deploy agent
- name: Deploy AI Agent
  run: |
    snowsql -a ${{ secrets.SF_ACCOUNT }} -u ${{ secrets.SF_USER }} -p ${{ secrets.SF_PASSWORD }} -q "
    create or replace ai agent support_classifier as using model 'gpt4' ;"
```

---

[Previous](./5-testing-and-validation.md) • [Next](./7-performance-and-best-practices.md)
