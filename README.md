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

1. `global/application.yml`  
   → Contains universal settings like the Spring Banner, standard Error Formats, and basic Actuator endpoints that every service in the galaxy must follow.(Logging, Observability, Actuator)

2. `global/application-{profile}.yml`
   → Reusable infrastructure settings. This is where your Postgres, Kafka, and Security (JWT) configs live. A service only "imports" the tech it needs.

3. `shared/{tech}/{tech}-{profile}.yml`  
   → Reusable infrastructure settings. This is where your Postgres, Kafka, and Security (JWT) configs live. A service only "imports" the tech it needs.

4. `{domain}/{service}/{service}.yml`  
   → Defines the service’s unique identity: the spring.application.name, context-paths, and business-specific properties that don't change across environments.

5. `{domain}/{service}/{service}-{profile}.yml`  
   → Highest File Priority. This is where you define the specific DB schema name, service-specific ports, or feature flags that change between dev, qa, and prod.

6. `Environment Variables / Secret Injection (Runtime)`  
   → Absolute Highest Priority. Used for passwords injected at runtime (K8s Secrets) or emergency overrides via -D arguments.

---

## 🗂️ Repository Structure

```
starone-central-config/
├── global/
│   ├── application-dev.yml
│   ├── application-qa.yml
│   ├── application-uat.yml
│   └── application-prod.yml
│
├── shared/
│   ├── kafka/
│   │   ├── kafka-dev.yml
│   │   ├── kafka-qa.yml
│   │   └── kafka-prod.yml
│   ├── postgres/
│   │   ├── postgres-dev.yml
│   │   ├── postgres-qa.yml
│   │   └── postgres-prod.yml
│   ├── redis/
│   │   ├── redis-dev.yml
│   │   ├── redis-qa.yml
│   │   └── redis-prod.yml
│   ├── security/
│   │   ├── jwt-dev.yml
│   │   └── jwt-prod.yml
│   └── observability/
│       ├── logging-dev.yml
│       ├── logging-qa.yml
│       └── logging-prod.yml
│
├── dhs/
│   ├── inventory-service/
│   │   ├── inventory-service-dev.yml
│   │   ├── inventory-service-qa.yml
│   │   └── inventory-service-prod.yml
│   ├── billing-service/
│   ├── order-service/
│   ├── auth-service/
│   ├── notification-service/
│   └── finance-service/
│
├── bookshow/
│   ├── booking-service/
│   ├── payment-service/
│   ├── theater-service/
│   ├── catalog-service/
│   ├── notification-service/
│   └── coupon-service/
│
└── README.md
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