# 🌐 Environment Configuration Guide

This document explains how StarOne services load configuration in each environment.

---

## 🏠 Local Environment

Config server runs in native mode:

```
spring.profiles.active=native
spring.cloud.config.server.native.search-locations=classpath:/configs/
```

Developers use local YAML files.

---

## 🧪 Dev Environment

Dev environment uses Git-based configs:

```
spring.cloud.config.server.git.uri=https://github.com/starone/starone-central-configs
```

Services load configs like:

```
http://config-server/dev/<service-name>.yml
```

---

## 🚀 Stage Environment

Used for QA validation and near-production testing.

- Uses stage branch or tagged versions
- Closely mirrors prod
- Stricter change approvals

---

## 🏭 Production Environment

- Read-only Git access for pipeline  
- Configs must be fully validated  
- Secrets resolved from Vault/Kubernetes  
- Changes must go through mandatory review

---

## 🔁 Reloading Configurations

If actuator refresh is enabled:

```
POST /actuator/refresh
```

Otherwise, redeploy service.

---

## 🪪 Environment Variables

Every environment may override config values:

```
SPRING_PROFILES_ACTIVE=prod
DB_HOST=prod-db.starone.com
```

These override Git configs.

---

## 📄 Related Docs

- `CONFIG_README.md`
- `CONFIG_NAMING_CONVENTIONS.md`
- `CONFIG_CONTRIBUTING.md`
- `SECURITY_POLICY.md`
