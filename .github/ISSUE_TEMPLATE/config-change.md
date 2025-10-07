---
name: "⚙️ Configuration Change"
about: Add or update configuration in the central config repository
title: "[Config]: <describe change>"
labels: ["central-config-repo", "config-server", "infrastructure", "todo"]
---

## 🧩 Description
Describe the configuration change you are proposing.

Example:
- Adding new project environment configuration.
- Updating global Kafka or Redis settings.
- Adding secure vault path for credentials.

## 📂 Files / Paths Affected
List files or directories affected:
```
common/base.yaml
projects/sportstats/dev/overrides.yaml
```

## 🔒 Secrets / Sensitive Data
- [ ] No secrets included in this PR
- [ ] Secrets stored in Vault (path: `secret/starone/dev`)
- [ ] Updated environment variables documented

## ✅ Acceptance Criteria
- [ ] Configuration validated with YAML lint.
- [ ] CI/CD config validation pipeline passed.
- [ ] Configs are correctly loaded by Spring Cloud Config Server.
- [ ] No breaking changes for dependent microservices.

## 🧠 Notes
Include links to related issues or documentation.

## 🏷️ Labels
`central-config-repo`, `config-server`, `infrastructure`

