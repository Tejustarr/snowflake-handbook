# Setting up dbt with Snowflake — detailed & secure

This guide walks through dbt Cloud and dbt Core with extra attention to secure config, key-pair auth, and an actionable GitHub Actions workflow.

---

## Quick checklist before you start
- Snowflake account & admin access to create roles and users.
- A dedicated service user for dbt runs (recommended).
- GitHub (or GitLab) repo for dbt project.
- CLI environment with Python 3.9+ (for dbt core) or dbt Cloud.
- Secrets manager or GitHub Secrets for credentials.

---

## Install dbt Core (Snowflake adapter)

```bash
python -m pip install --upgrade pip
pip install "dbt-snowflake"  # pin version as needed, e.g., dbt-snowflake==1.5.0
```

Use virtualenv/venv or Conda to isolate.

---

## profiles.yml — robust examples

**Password (env var) auth** — recommended for most setups:

```yaml
snowflake_project:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: "ABC12345.us-east-1"   # account locator (no https)
      user: "DBT_SERVICE_USER"
      password: "{{ env_var('SNOWFLAKE_PASSWORD') }}"
      role: "DBT_TRANSFORM_ROLE"
      warehouse: "DBT_WH"
      database: "ANALYTICS_DB"
      schema: "DBT_DEV"
      threads: 4
      client_session_keep_alive: False
```

**Key-pair (RSA) authentication** — for stronger, non-password auth:

```yaml
snowflake_project:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: "ABC12345.us-east-1"
      user: "DBT_SERVICE_USER"
      private_key_path: "~/.ssh/snowflake_dbt_key.pem"  # or use env var with key contents
      private_key_passphrase: "{{ env_var('SF_KEY_PASSPHRASE') }}"
      role: "DBT_TRANSFORM_ROLE"
      warehouse: "DBT_WH"
      database: "ANALYTICS_DB"
      schema: "DBT_DEV"
      threads: 4
      client_session_keep_alive: False
```

**Notes:** YAML is indentation-sensitive — use spaces only. Prefer environment variables for secrets (CI/CD will inject them from secret stores).

---

## Creating service user & role (Snowflake SQL sample)

```sql
-- run as SECURITYADMIN/ACCOUNTADMIN (example)
CREATE USER dbt_service_user PASSWORD = 'XXXXXXXX' MUST_CHANGE=false;
CREATE ROLE dbt_transform_role;
GRANT ROLE SYSADMIN TO ROLE dbt_transform_role;
GRANT ROLE dbt_transform_role TO USER dbt_service_user;
GRANT USAGE ON WAREHOUSE dbt_wh TO ROLE dbt_transform_role;
GRANT USAGE ON DATABASE analytics_db TO ROLE dbt_transform_role;
GRANT USAGE ON SCHEMA analytics_db.dbt_dev TO ROLE dbt_transform_role;
-- grant necessary table privileges as needed
```

Prefer to craft least-privilege grants for production.

---

## Initialize a dbt project (CLI)

```bash
dbt init my_project
cd my_project
# create models/ , snapshots/ , macros/ directories as needed
dbt debug --profiles-dir ~/.dbt
```

---

## GitHub Actions: secure profiles creation (example)

This action writes a minimal `profiles.yml` from secrets in CI and runs dbt commands.

```yaml
name: dbt CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v4
        with: python-version: '3.11'
      - run: pip install dbt-snowflake
      - name: Create profiles.yml
        env:
          SF_ACCOUNT: ${{ secrets.SF_ACCOUNT }}
          SF_USER: ${{ secrets.SF_USER }}
          SF_PASSWORD: ${{ secrets.SF_PASSWORD }}
          SF_ROLE: ${{ secrets.SF_ROLE }}
          SF_WAREHOUSE: ${{ secrets.SF_WAREHOUSE }}
          SF_DATABASE: ${{ secrets.SF_DATABASE }}
          SF_SCHEMA: ${{ secrets.SF_SCHEMA }}
        run: |
          mkdir -p ~/.dbt
          cat > ~/.dbt/profiles.yml <<EOF
          snowflake_project:
            target: ci
            outputs:
              ci:
                type: snowflake
                account: "${SF_ACCOUNT}"
                user: "${SF_USER}"
                password: "${SF_PASSWORD}"
                role: "${SF_ROLE}"
                warehouse: "${SF_WAREHOUSE}"
                database: "${SF_DATABASE}"
                schema: "${SF_SCHEMA}"
                threads: 1
                client_session_keep_alive: False
          EOF
      - run: dbt debug --profiles-dir ~/.dbt
      - run: dbt compile --profiles-dir ~/.dbt
      - run: dbt test --profiles-dir ~/.dbt
```

---

## Final tips for setup
- Use a separate `dev` schema per developer or use ephemeral environments.  
- Pin adapter versions in `requirements.txt` or CI to ensure reproducibility.  
- For larger orgs, store credentials in a vault (HashiCorp Vault, AWS Secrets Manager) and inject in CI.

---

Next: [models-and-transformations.md](./models-and-transformations.md) — deep dive into patterns and recipes.