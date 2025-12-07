# 🔐 Security Policy — StarOne Config Repo

This document defines rules for safely handling configuration files, especially those containing sensitive data.

---

## 🚨 Reporting Security Issues

If you discover a security risk:

1. Do **not** open a public GitHub issue.
2. Email the security team:

```
security@starone.com
```

3. Include:
   - Description of the issue
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if any)

We will acknowledge within **48 hours**.

---

## 🔐 Secret Management Rules

### ❌ Never Store Secrets in Git
This includes:

- API keys  
- DB passwords  
- JWT secrets  
- OAuth secrets  
- Kafka auth credentials  
- Private certificates  

**Use Vault or Kubernetes Secrets instead.**

---

## ✔ Allowed in Config Repo

| Type | Allowed |
|------|---------|
| Feature flags | ✔ |
| Service URLs | ✔ |
| Log configs | ✔ |
| Timeouts, limits | ✔ |
| Public configs | ✔ |

---

## ❌ Not Allowed in Config Repo

| Type | Forbidden |
|------|-----------|
| Secrets / Tokens / Passwords | ❌ |
| Private keys | ❌ |
| Database credentials | ❌ |
| OAuth client secrets | ❌ |
| Cloud provider keys | ❌ |

---

## 🏷 Secret Placeholder Example

```
jwt:
  secret: ${JWT_SECRET}   # stored in Vault/K8s
```

---

## 🔄 Rotation Requirements

All secrets must be rotated:

- **Every 90 days**
- Immediately after incident
- After employee offboarding
- After environment migration

---

## 🔍 Auditing

We perform:

- Quarterly config audits  
- Automated scans for committed secrets  
- Dependency security checks  

If issues are found → repository access may be restricted.

---

## 👮 Enforcement

Violations may result in:

- PR rejection  
- Revocation of write access  
- Security review

---

## 📄 Related Docs

- `CONFIG_CONTRIBUTING.md`
- `CONFIG_NAMING_CONVENTIONS.md`
- `ENVIRONMENT_GUIDE.md`
