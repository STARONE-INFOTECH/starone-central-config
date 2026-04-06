# 🤝 Contributing to StarOne Config Repository

Thank you for helping improve the configuration ecosystem for our microservices!

This guide explains how to safely create or modify configuration files.

---

## 📝 Requirements Before Contributing

- Understand Spring Cloud Config conventions  
- Follow `CONFIG_NAMING_CONVENTIONS.md`  
- Never commit sensitive data (see `SECURITY_POLICY.md`)  
- Validate YAML before submitting a PR  
- Create changes per service folder (no mixed PRs)

---

## 📌 Branching Strategy

- `main` → production configurations  
- `development` → dev/stage configs  
- feature branches → personal branches

Example:

```
feature/update-auth-rates
```

---

## ✔ Making Changes

### 1. Update the correct environment file
Example:

```
auth-service/application-dev.yml
```

### 2. Keep YAML structure consistent

### 3. Validate using:

```
yamllint .
```

or:

```
scripts/validate-configs.sh
```

### 4. Document major changes in `CHANGELOG.md`.

---

## ✔ Pull Request Rules

- One PR per logical config change  
- Must pass linting  
- Must link to a Jira task  
- Must include summary of changes  
- Must not contain secrets  
- Reviewer must confirm environment parity

---

## ✔ Example PR Summary

```
### Summary
- Increased session timeout
- Added new CORS domain for web app

### Impact
Affects auth-service dev environment only.

### Rollback
Revert to previous version in Git history.
```

---

## 📄 Related Docs
- `SECURITY_POLICY.md`
- `CONFIG_NAMING_CONVENTIONS.md`
- `ENVIRONMENT_GUIDE.md`
