# Native Apps & Containers — Setup

Setup steps, enabling features, and security considerations.


## Prepare container registry & roles
```sql
create role app_deployer;
grant usage on integration registry_integration to role app_deployer;
```
## Example: register image and create app
```sql
create or replace external location my_registry url='https://registry.example.com/myrepo/';
create or replace native app my_app from image 'registry.example.com/myrepo:latest';
```

---

[Previous](./2-intro.md) • [Next](./4-usage-and-scenarios.md)
