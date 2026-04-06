# 🛠️ StarOne Central Configuration Repository

This repository contains all **externalized configuration** for the StarOne microservices ecosystem.  
It is used by **Spring Cloud Config Server** across all environments: Local, Dev, Stage, and Prod.

---

## 📌 Purpose

- Centralize configuration management  
- Ensure consistency across services  
- Avoid hardcoded values inside applications  
- Support configuration versioning and rollback  
- Provide environment isolation (local/dev/stage/prod)  
- Allow Git-based configuration for dynamic updates

---

## 📁 Folder Structure

```
configs/
 ├── docs/                    # Documentation
 ├── global/                  # Config shared across all services
 ├── infra/                   # Gateway, Eureka, tracing, monitoring
 ├── auth-service/            # Auth service configs
 ├── sportstats-player-service/
 ├── sportstats-match-service/
 ├── bookshow-movie-service/
 ├── ... (other microservices)
 └── README.md
```

---

## 🌐 Environment Handling

### Local (Native Filesystem Mode)
```
spring.cloud.config.server.native.search-locations=classpath:/configs/
```

### Dev/Stage/Prod (Git Mode)
```
spring.cloud.config.server.git.uri=https://github.com/starone/starone-central-configs
```

Environment files follow:

```
application.yml
application-dev.yml
application-stage.yml
application-prod.yml
```

---

## 🧩 Service-Specific Configuration

Each microservice follows:

```
<service-name>/
 ├── application.yml
 ├── application-dev.yml
 ├── application-stage.yml
 └── application-prod.yml
```

---

## 🧪 Validating YAML

Run:

```
yamllint .
```

Or use the provided script:

```
scripts/validate-configs.sh
```

---

## 📄 See Also

- `CONFIG_NAMING_CONVENTIONS.md`
- `ENVIRONMENT_GUIDE.md`
- `CONFIG_CONTRIBUTING.md`
- `SECURITY_POLICY.md`
