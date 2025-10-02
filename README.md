# Central Configuration Repository

This repository serves as the centralized storage for configuration files used across all STARONE applications and services. It ensures consistency, version control, and streamlined management of configurations for different environments (e.g., development, staging, production).

## Purpose
- Provide a single source of truth for application and service configurations.
- Enable version-controlled updates to configurations.
- Facilitate collaboration and review of configuration changes across teams.

---
# 📂 Repository Configuration Structure

This repository contains configuration files for multiple applications as well as shared/global configs.  
Each application has environment-specific (`dev`, `prod`) and core settings, along with documentation.

---

## 📂 Directory Layout

```text
📂 starone-central-configs-repo/
├── 📄 README.md                # Purpose, usage, how to add projects/services
├── 📄 LICENSE.md               # e.g., MIT
├── 📄 CONTRIBUTING.md          # Rules for adding projects, microservices, PR process
├── 📄 CHANGELOG.md             # Track config changes
├── 📄 .gitignore               # Ignore .env, temp files
├── 📂 .github/                 # GitHub workflows for validation
│   ├── 📂 workflows/
│   │   ├── 📄 config-validate.yml  # Lint YAML, docker-compose
│   │   └── 📄 **sync-check.yml**   # **New: Ensure projects/services reference commons correctly**
│   ├── 📂 ISSUE_TEMPLATE/
│   │   └── 📄 config-change.md
│   └── 📄 PULL_REQUEST_TEMPLATE.md
├── 📂 common/                  # Global configs shared across ALL projects
│   ├── 📄 base.yaml            # Global defaults (e.g., logging, timeouts)
│   ├── 📂 ci-cd/
│   │   └── 📄 github-actions-template.yaml
│   ├── 📂 deployment/
│   │   └── 📄 base-dockerfile
│   ├── 📂 infra/               # Global infra configs (e.g., Kafka if used by multiple projects)
│   │   ├── 📄 docker-compose-base.yaml  # Base compose for shared services
│   │   ├── 📂 kafka/
│   │   │   └── 📄 config.yaml
│   │   ├── 📂 redis/
│   │   │   └── 📄 config.yaml
│   │   └── 📂 **monitoring/**  # **New: Shared monitoring configs (e.g., Prometheus)**
│   │       └── 📄 prometheus.yaml
│   └── 📂 security/
│       └── 📄 base-rbac.yaml
├── 📂 projects/                # One folder per project
│   ├── 📂 sportstats/          # Project 1: Sportstats with microservices
│   │   ├── 📂 common/          # Project-specific shared configs
│   │   │   ├── 📄 docker-compose.yaml  # Defines MySQL, PostgreSQL, Kafka for sportstats
│   │   │   ├── 📂 mysql/
│   │   │   │   └── 📄 config.yaml
│   │   │   ├── 📂 postgresql/
│   │   │   │   └── 📄 config.yaml
│   │   │   ├── 📂 kafka/
│   │   │   │   └── 📄 config.yaml
│   │   │   └── 📂 **other/**   # **New: Other shared infra (e.g., Elasticsearch)**
│   │   │       └── 📄 config.yaml
│   │   ├── 📂 base/            # Project-wide defaults for all services
│   │   │   └── 📄 config.yaml
│   │   ├── 📂 dev/             # Project-wide env overrides
│   │   │   └── 📄 overrides.yaml
│   │   ├── 📂 staging/
│   │   │   └── 📄 overrides.yaml
│   │   ├── 📂 prod/
│   │   │   └── 📄 overrides.yaml
│   │   ├── 📂 services/        # Microservices for sportstats
│   │   │   ├── 📂 user-service/
│   │   │   │   ├── 📂 base/
│   │   │   │   │   └── 📄 config.yaml  # Extends project base
│   │   │   │   ├── 📂 dev/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📂 staging/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📂 prod/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📄 docker-compose.override.yaml  # Service-specific compose overrides
│   │   │   │   └── 📄 README.md
│   │   │   ├── 📂 stats-service/  # Similar structure
│   │   │   │   ├── 📂 base/
│   │   │   │   │   └── 📄 config.yaml
│   │   │   │   ├── 📂 dev/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📂 staging/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📂 prod/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📄 docker-compose.override.yaml
│   │   │   │   └── 📄 README.md
│   │   │   └── 📂**more-services/**  # **Add as needed (e.g., auth-service)**
│   │   └── 📄 README.md        # Docs: Microservices overview, how to add new ones
│   ├── 📂 dhs/       # **Project 2: Another project with microservices**
│   │   ├── 📂 common/          # Project-specific shared infra
│   │   │   ├── 📄 docker-compose.yaml  # MySQL, Redis, etc.
│   │   │   ├── 📂 mysql/
│   │   │   │   └── 📄 config.yaml
│   │   │   └── 📂 redis/
│   │   │       └── 📄 config.yaml
│   │   ├── 📂 base/
│   │   │   └── 📄 config.yaml
│   │   ├── 📂 dev/
│   │   │   └── 📄 overrides.yaml
│   │   ├── 📂 staging/
│   │   │   └── 📄 overrides.yaml
│   │   ├── 📂 prod/
│   │   │   └── 📄 overrides.yaml
│   │   ├── 📂 services/
│   │   │   ├── 📂 **cart-service/**  # Example microservice
│   │   │   │   ├── 📂 base/
│   │   │   │   │   └── 📄 config.yaml
│   │   │   │   ├── 📂 dev/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📂 staging/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📂 prod/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📄 docker-compose.override.yaml
│   │   │   │   └── 📄 README.md
│   │   │   ├── 📂 **order-service/**  # Another microservice
│   │   │   │   ├── 📂 base/
│   │   │   │   │   └── 📄 config.yaml
│   │   │   │   ├── 📂 dev/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📂 staging/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📂 prod/
│   │   │   │   │   └── 📄 overrides.yaml
│   │   │   │   ├── 📄 docker-compose.override.yaml
│   │   │   │   └── 📄 README.md
│   │   └── README.md
│   └── 📂 **...**              # Add more projects as needed
├── 📂 environments/            # Global env configs, if needed
│   ├── 📂 dev/
│   │   └── 📄 global.yaml
│   ├── 📂 staging/
│   │   └── 📄 global.yaml
│   └── 📂 prod/
│       └── 📄 global.yaml
├── 📂 tools/                   # Scripts for validation/merging
│   ├── 📄 validate-configs.sh
│   └── 📄 **compose-validate.sh**  # Validate docker-compose setups
└── 📂 docs/
    ├── 📄 architecture.md      # Diagram: Config flow across projects/services
    └── 📂 **examples/**        # **New: Example configs for new projects/services**
        └── 📄 add-service.md

```

---

## 📌 Notes

- **App Configs**  
  Each application (`app1`, `app2`, `app3`) has its own configuration directory containing:
  - `dev.yaml` → Development-specific configuration
  - `prod.yaml` → Production configuration
  - `settings.yaml` / `config.json` → Core application settings
  - `env.json` / `secrets.env` → Environment variables (sensitive data should be managed securely)
  - `README.md` → Explanation of how to use configs for the respective app  

- **Shared Configs**  
  - `/shared/global.yaml` → Common settings used across multiple applications  

- **Documentation**  
  - `/docs/README.md` → Repository overview and instructions  
  - `/docs/CONTRIBUTING.md` → Contribution guidelines  

---

✅ This structure ensures:
- Clear separation between apps  
- Easy environment management (`dev`, `prod`)  
- Centralized shared configurations  
- Proper documentation for maintainability  

