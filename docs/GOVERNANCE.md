# 🏷 Configuration Naming Conventions

This guide standardizes how services and configuration files are named in the StarOne ecosystem.

---

## 📁 Folder Naming Rules

Use **kebab-case**:

```
auth-service/
bookshow-movie-service/
sportstats-player-service/
```

---

## 📄 File Naming Rules

Environment files:

```
application.yml
application-local.yml
application-dev.yml
application-stage.yml
application-prod.yml
```

---

## 🌐 Key Naming

Use **dot notation**:

```
server.port=8080
spring.datasource.url=...
logging.level.com.starone=debug
```

---

## 🧩 Shared Config Naming

Global configs:

```
global/application.yml
```

Infra configs:

```
infra/gateway.yml
infra/eureka.yml
infra/zipkin.yml
```

---

## ✔ Recommended Structure

A microservice:

```
my-service/
 ├── application.yml
 ├── application-dev.yml
 ├── application-stage.yml
 └── application-prod.yml
```

---

## ❌ Do Not

- Use camelCase keys  
- Put environment name inside folders  
- Add uppercase letters  
- Create custom file naming patterns

---

## 📄 Related Docs
- `CONFIG_README.md`
- `CONFIG_CONTRIBUTING.md`
