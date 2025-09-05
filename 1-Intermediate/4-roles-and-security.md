# Roles and Security in Snowflake

Snowflake uses a **Role-Based Access Control (RBAC)** model for managing users, roles, and privileges.  
This ensures that only authorized people can access specific data and resources.

---

## 1. Key Concepts

### Users
- Represents an individual person or service account.  
- Each user is assigned a default role.  
- Users authenticate via:
  - Username/password
  - MFA (Multi-Factor Authentication)
  - SSO/OAuth integration

### Roles
- Define **what actions** a user can perform.  
- Roles can be assigned to users or other roles (role hierarchy).  
- Privileges are always granted to roles, **not directly to users**.

### Privileges
- Specific permissions (e.g., `SELECT`, `INSERT`, `USAGE`) granted on objects.  
- Example: A role may have `SELECT` on a table but not `INSERT`.

---

## 2. Built-in System Roles

Snowflake comes with predefined roles:

- **ACCOUNTADMIN**
  - Full account-level access (superuser).
  - Can manage all security, billing, and objects.
- **SYSADMIN**
  - Manages databases, schemas, and warehouses.
  - Commonly used for day-to-day admin tasks.
- **USERADMIN**
  - Manages users and roles.
- **PUBLIC**
  - A default role with minimal privileges.
  - Automatically assigned to every user.

---

## 3. Creating and Assigning Roles

Example: Create a new role and grant it access to a schema.

```sql
-- Create a new role
CREATE ROLE analyst;

-- Grant schema usage
GRANT USAGE ON DATABASE sales_db TO ROLE analyst;
GRANT USAGE ON SCHEMA sales_db.analytics TO ROLE analyst;

-- Grant read-only access to a table
GRANT SELECT ON TABLE sales_db.analytics.customers TO ROLE analyst;

-- Assign role to a user
GRANT ROLE analyst TO USER arjun;
```

---

## 4. Switching Roles

A user can have multiple roles but only one active at a time.

```sql
-- Check current role
SELECT CURRENT_ROLE();

-- Switch to another role
USE ROLE analyst;
```

---

## 5. Security Features

- **Encryption**
  - All data is encrypted at rest and in transit.
- **Multi-Factor Authentication (MFA)**
  - Adds an extra security layer for login.
- **Single Sign-On (SSO)**
  - Integrates with identity providers (Okta, Azure AD, etc.).
- **Network Policies**
  - Restrict login access based on allowed IP addresses.
- **Session Policies**
  - Control session timeouts and inactivity limits.

---

## 6. Best Practices

- Follow **least privilege** principle → grant only what is necessary.  
- Create **separate roles** for read-only, ETL, and admin tasks.  
- Never use `ACCOUNTADMIN` for day-to-day queries.  
- Regularly review and audit role grants.  
- Use **MFA + SSO** for stronger authentication.  

---

## 📌 Key Takeaway
Snowflake’s RBAC model makes it easy to control **who can access what**.  
By properly defining users, roles, and privileges — and following best practices —  
you ensure both **security and governance** while keeping access simple for teams.
