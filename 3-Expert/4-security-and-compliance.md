# Security and Compliance in Snowflake

Snowflake is designed with **security as a core principle**.  
It provides enterprise-grade features for protecting data at rest, in transit, and in use, while also meeting global compliance standards.

---

## 1. Security Architecture

### End-to-End Encryption
- **At Rest** → All data is encrypted using AES-256.  
- **In Transit** → TLS encryption protects data between clients and Snowflake.  
- **In Use** → Temporary data (e.g., query results, caches) is also encrypted.  

### Key Management
- Snowflake uses **hierarchical key management**.  
- Keys are rotated automatically on a regular basis.  
- Customers can opt for **Tri-Secret Secure** (bring your own key + Snowflake + cloud provider key).  

---

## 2. Authentication and Access Control

### Authentication Options
- Username & password  
- Multi-Factor Authentication (MFA)  
- Single Sign-On (SSO) via SAML or OAuth  
- External identity providers (Okta, Azure AD, Ping Identity, etc.)  

### Role-Based Access Control (RBAC)
- Access is granted to **roles**, not directly to users.  
- Roles can be nested to create hierarchies.  
- Privileges include `USAGE`, `SELECT`, `INSERT`, etc.  

---

## 3. Network Security

- **Network Policies** → Restrict access based on IP ranges.  
- **Private Connectivity** → Direct connections via AWS PrivateLink, Azure Private Link, or GCP Private Service Connect.  
- **Virtual Private Snowflake (VPS)** → A dedicated Snowflake deployment for highly regulated customers.  

---

## 4. Data Protection Features

- **Dynamic Data Masking** → Hide sensitive fields at query time based on user role.  
  ```sql
  CREATE MASKING POLICY ssn_mask AS
  (val STRING) RETURNS STRING ->
    CASE
      WHEN CURRENT_ROLE() IN ('HR_ROLE') THEN val
      ELSE 'XXX-XX-XXXX'
    END;

  ALTER TABLE employees MODIFY COLUMN ssn
  SET MASKING POLICY ssn_mask;
  ```

- **Row Access Policies** → Restrict which rows a role can query.  
- **Secure Views** → Expose only required columns and rows.  

---

## 5. Compliance Standards

Snowflake meets a wide range of compliance certifications, including:  
- **SOC 1, SOC 2, SOC 3**  
- **ISO 27001, ISO 27017, ISO 27018**  
- **HIPAA** (for healthcare data)  
- **PCI DSS** (for payment card data)  
- **FedRAMP** (for U.S. government use)  
- **GDPR** and **CCPA** support for data privacy regulations  

📌 This makes Snowflake suitable for industries like **finance, healthcare, and government**.

---

## 6. Monitoring and Auditing

- **Access History** → Logs every query, including who ran it and when.  
- **Login History** → Captures authentication attempts.  
- **Query History** → Provides visibility into workloads.  
- **Event Tables** → Newer feature for structured security/audit logs.  

---

## 7. Best Practices

- Always use **SSO + MFA** for authentication.  
- Enforce **least privilege** with RBAC.  
- Apply **masking policies** for PII and sensitive data.  
- Use **network policies** to limit access.  
- Regularly review **audit logs** for suspicious activity.  
- For highly regulated environments, consider **Tri-Secret Secure**.  

---

## 📌 Key Takeaway
Snowflake provides **end-to-end encryption, RBAC, data masking, and compliance certifications** out of the box.  
With proper governance policies, it can meet the strictest **security and regulatory requirements** in industries like finance, healthcare, and government.
