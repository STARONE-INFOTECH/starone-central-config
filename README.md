# 📂 StarOne Central Configs Repository

### 🌌 The Distributed "Source of Truth" for the StarOne Galaxy Ecosystem

This repository serves as the **centralized, version-controlled configuration management system** for all microservices within the **StarOne Galaxy organization**.

It is designed to integrate seamlessly with **Spring Cloud Config Server**, enabling:
- Dynamic configuration updates
- Environment-specific overrides
- Zero rebuild deployments (same JAR across environments)
- Central governance and consistency

---

## 🏗️ Architecture & Inheritance Model

This repository follows a **Layered Configuration Inheritance Model**, where configurations are merged in a defined priority order.

### 🔄 Configuration Resolution Order (Low → High Priority)

1. `application.yml`  
   → Global defaults (Logging, Observability, Actuator)

2. `spring-base.yml`  
   → Core Spring Boot defaults (Banner, common endpoints)

3. `shared/*.yml`  
   → Reusable infrastructure traits (DB, Security, Eureka)

4. `{domain}/{service}.yml`  
   → Service-specific base configuration

5. `{domain}/{service}-{profile}.yml`  
   → Environment overrides (**Highest Priority**)

---

## 🗂️ Repository Structure

```
📂 starone-central-configs-repo/
├── 📄 VERSION
├── 📄 application.yml
├── 📄 spring-base.yml
├── 📂 shared/
│   ├── 📄 mysql-db.yml
│   ├── 📄 security-base.yml
│   ├── 📄 eureka-dhs.yml
│   └── 📄 eureka-bookshow.yml
├── 📂 dhs/
│   ├── 📄 dhs-auth-svc.yml
│   ├── 📄 dhs-gateway.yml
│   └── 📄 inventory-svc.yml
├── 📂 bookshow/
│   ├── 📄 bookshow-auth-svc.yml
│   ├── 📄 bookshow-gateway.yml
│   └── 📄 booking-svc.yml
└── 📂 scripts/
    └── 📄 validate-yaml.sh
```

---

## 🌍 Environment Management

We use **Spring Profiles** to promote the same application artifact across environments.

| Profile | Purpose              | Override File              | Default Applied |
|--------|---------------------|---------------------------|-----------------|
| default | Shared logic         | `{service}.yml`           | ✅ Yes          |
| dev     | Local development    | `{service}-dev.yml`       | ❌ No           |
| qa      | Testing / staging    | `{service}-qa.yml`        | ❌ No           |
| prod    | Production           | `{service}-prod.yml`      | ❌ No           |

---

## 🚀 Usage Guide

### 👨‍💻 For Developers: Adding a New Service

#### 📁 Step 1: Create Configuration File

- Place it under:
  - `/dhs` → DHS domain
  - `/bookshow` → BookShow domain

#### 🏷️ Naming Convention

The filename **must match**:

```yaml
spring.application.name
```

Example:
```
order-service.yml
order-service-dev.yml
order-service-qa.yml
order-service-prod.yml
```

---

#### 🔗 Step 2: Import Shared Configs

Leverage reusable configurations:

```yaml
spring:
  config:
    import: "optional:configserver:shared/mysql-db.yml"
```

---

### ⚙️ For DevOps: Config Server Setup

Configure your **Spring Cloud Config Server**:

```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/SachinSalunkeLead/starone-config-repo.git
          search-paths: 'shared,dhs,bookshow'
```

---

## 🔌 Architecture Summary

- **Config Server** → Central configuration provider
- **Git Repository** → Source of truth
- **Microservices** → Pull configs at runtime
- **Profiles** → Control environment behavior
- **Shared Traits** → Promote reuse and consistency

---

## 🛠️ Maintenance & Validation

### ✅ YAML Validation

Run lint checks before committing:

```bash
./scripts/validate-yaml.sh
```

---

### 📦 Version Tracking

Update the `VERSION` file on every change:

```
v9.3.0 → Added Redis caching config for booking-svc
```

---

## 🧱 Best Practices

- Keep configs **DRY** using `shared/`
- Avoid duplication across services
- Use environment overrides minimally
- Do NOT store secrets in plain text
- Maintain backward compatibility
- Validate before merging

---

## 🔐 Security Guidelines

- Use **Vault / Secrets Manager** for credentials
- Avoid hardcoding sensitive data
- Encrypt sensitive properties when required
- Restrict repo access via IAM

---

## 🧪 CI/CD Integration

- YAML validation via script
- Version enforcement using `VERSION`
- Config promotion via Git branching strategy
- Compatible with GitOps workflows

---

## 🤝 Contribution Guidelines

- Create feature branches
- Validate YAML before PR
- Update `VERSION`
- Require peer review before merge to `main`

---

## 📜 License

This project is licensed under the **MIT License**.

---

## 📬 Support

For issues, improvements, or onboarding:
- Contact the Platform Engineering Team
- Or raise a Pull Request

---

### 🚀 StarOne Principle

> *"Build once. Configure everywhere. Deploy without fear."*

---