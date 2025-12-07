# Central Configuration Repository
---
This repository serves as the centralized storage for configuration files used across all STARONE applications and services. It ensures consistency, version control, and streamlined management of configurations for different environments (e.g., development, staging, production).

## 📂 Purpose
- Provide a single source of truth for application and service configurations.
- Enable version-controlled updates to configurations.
- Facilitate collaboration and review of configuration changes across teams.

## 📂 Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Adding a New Project or Microservice](#adding-a-new-project-or-microservice)
- [Contributing](#contributing)
- [Troubleshooting](#troubleshooting)
- [License](#license)


## 🌐 Overview

This repo serves as the single source of truth for configurations across multiple microservices-based projects. Key features:
- **Global Commons**: Shared configs (e.g., `common/base.yaml`) for logging, timeouts, etc.
- **Project-Specific Configs**: Each project (e.g., sportstats) has its own shared infrastructure (e.g., MySQL, Kafka) and microservice settings.
- **Environment Overrides**: Supports dev, staging, prod environments.
- **Automation**: Validation scripts and CI/CD integration for reliability.

## 🗂️ Projects

List of projects managed in this repo:
- **Sportstats**: Tracks sports data with microservices (e.g., user-service, stats-service).
  - Dependencies: MySQL, PostgreSQL, Kafka.
  - See [Sportstats Structure](#sportstats-structure) for details.
- **Bookshow**: Manages online show booking operations with microservices (e.g., movie-service, order-service).
  - Dependencies: MySQL, Redis.
  - See [Ecommerce Structure](#ecommerce-structure) for details.

## 📂 Repository Structure

This repository contains configuration files for multiple applications as well as shared/global configs. 
Each application has environment-specific (`dev`, `prod`) and core settings, along with documentation.

```
📂 starone-central-configs-repo/
├── 📄 README.md                             # How to consume, how to contribute
├── 📄 VERSION                               # ← MUST have (e.g. v9.3.0) – semantic version
├── 📄 .gitignore
├── 📄 .editorconfig
├── 📄 .gitattributes
│
├── 📂 common/                                    # Global defaults for ALL projects
│   ├── 📄 base-spring.yaml
│   ├── 📄 base-logging.yaml
│   ├── 📄 base-observability.yaml
│   ├── 📄 base-resilience.yaml
│   └── 📂 security/
│       ├── 📄 oauth2.yaml
│       └── 📄 rbac-defaults.yaml
│
├── 📂 components/                                # Global infra building blocks
│   ├── 📂 mysql/
│   │   └── 📄 defaults.yaml
│   ├── 📂 postgresql/
│   ├── 📂 redis/
│   ├── 📂 kafka/
│   ├── 📂 elasticsearch/
│   └── 📂 mongodb/
│
├── 📂 environments/                              # Global environment overrides
│   ├── 📄 local.yaml
│   ├── 📄 dev.yaml
│   ├── 📄 staging.yaml
│   └── 📄 prod.yaml
│
├── 📂 .github/
│   ├── 📂 workflows/
│   │   ├── 📄 config-validate.yml
│   │   ├── 📄 sync-check.yml
│   │   ├── 📄 lint-yaml.yml               # New: Lint YAML safety
│   │   ├── 📄 sync-labels.yaml
│   │   └── 📄 validate-issue-prs.yml
│   └── 📄 labels.yaml
│  
├── 📂 common/                       # ✔ global shared config for ALL projects
│   ├── 📄 base.yaml                 # ✔ master defaults (logging, timeouts, spring)
│   ├── 📂 ci-cd/
│   │   └── 📄 github-actions-template.yaml
│   ├── 📂 deployment/
│   │   ├── 📄 base-dockerfile
│   │   └── 📄 base-k8s.yaml         # New: recommended shared k8s defaults
│   ├── 📂 infra/
│   │   ├── 📄 kafka.yaml
│   │   ├── 📄 redis.yaml
│   │   ├── 📄 mysql.yaml
│   │   └── 📄 monitoring.yaml       # New: prometheus / grafana base config
│   └── 📂 security/
│       ├── 📄 base-security.yaml
│       └── 📄 rbac.yaml
│  
├── 📂 environments/                  # ✔ global env overrides (NOT project-specific)
│   ├── 📂 dev/
│   │   └── 📄 global.yaml
│   ├── 📂 staging/
│   │   └── 📄 global.yaml
│   └── 📂 prod/
│       └── 📄 global.yaml
│ 
├── 📂 projects/                                  # Per-project & per-service overrides
│   ├── 📂 sportstats/
│   │   ├── 📄 project.yaml                   # project-level defaults
│   │   └── 📂 services/
│   │       ├── 📂 user-service/
│   │       │   ├── 📄 base.yaml
│   │       │   ├── 📄 dev.yaml
│   │       │   ├── 📄 staging.yaml
│   │       │   └── 📄 prod.yaml
│   │       ├── 📂 stats-service/
│   │       │   ├── 📄 base.yaml
│   │       │   ├── 📄 dev.yaml
│   │       │   ├── 📄 staging.yaml
│   │       │   └── 📄 prod.yaml
│   │       └── ...
│   ├── 📂 dhs/
│   │   └── same pattern...
│   └── 📂 another-project/ ...
│  
├── 📂 templates/                                 # Docker, Helm, compose templates
│   ├── 📄 docker-compose.base.yml
│   ├── 📄 helm-values.template.yaml
│   └── 📄 kubernetes-deployment.template.yaml
│  
├── 📂 tools/                                     # Scripts consumed by application repos
│   ├── 📄 validate.sh
│   ├── 📄 render-compose.sh
│   └── 📄 check-version.sh
│  
├── 📂 .github/
│   └── workflows/
│       ├── 📄 validate-configs.yml
│       ├── 📄 require-version-bump.yml       # blocks PR if VERSION not bumped
│       ├── 📄 lint-yaml.yml
│       └── 📄 labeler.yml
│  
└── 📂 docs/                                   
    ├── 📄 CHANGELOG.md
    ├── 📄 CONFIG_CONTRIBUTING.md
    ├── 📄 CONFIG_NAMING_CONVENTIONS.md
    ├── 📄 CONFIG_README.md
    ├── 📄 ENVIRONMENT_GUIDE.md
    ├── 📄 SECURITY_POLICY.md
    ├── 📄 service-registry.yaml
    └── 📂 examples/
        └── 📄 add-new-service.md

```
## 🏟️ Sportstats Structure

- `common/docker-compose.yaml`: Defines MySQL, PostgreSQL, Kafka.
- `services/user-service/`: User management configs.
- `services/stats-service/`: Stats processing configs.
    
## 🛒 dhs Structure

- `common/docker-compose.yaml`: Defines MySQL, Redis.
- `services/cart-service/`: Shopping cart configs.
- `services/order-service/`: Order processing configs.
    
## 🔧 Prerequisites

- **Git**: For cloning and managing the repo.
- **Docker**: To run docker-compose files.
- **yq**: For merging YAML configs (sudo apt install yq or equivalent).
- **yamllint**: For validation (pip install yamllint).
- **Optional**: Vault for secrets (e.g., AWS Secrets Manager).

## 📂 Getting Started
Steps to set up or access the repo.

- **Clone the repo**:
- bash 
    ```
    git clone https://github.com/SachinSalunkeLead/starone-config-repo.git
    ```

- **Install dependencies (e.g., for validation tools)**:
- bash
    ```
    pip install yamllint
    ```

- **Validate configs**:
- bash
    ```
    ./tools/validate-configs.sh
    ```
## 🚀 Usage
- **Pull Configs**: In a microservice repo (e.g., sportstats-user-service):
- bash
    ```
    git submodule add https://github.com/your-org/central-configs configs
    ```

- **Run Docker Compose**:
- bash
    ```
    docker-compose -f configs/projects/sportstats/common/docker-compose.yaml -f configs/projects/sportstats/services/user-service/ docker-compose.override.yaml up
    ```

- **Merge Configs**: Combine global, project, and service configs
- bash
    ```
    yq eval-all '. as $item ireduce ({}; . * $item)' configs/common/base.yaml configs/projects/sportstats/base/config.yaml > merged.yaml
    ```

- **Secrets**: Use .env files or a vault for variables like ${MYSQL_ROOT_PASSWORD}.

## ➕ Adding a New Project or Microservice
Guide users on extending the repo.

- **New Project**:
    - Create projects/new-project/.
    - Add common/docker-compose.yaml for shared infra.
    - Create base/config.yaml and environment folders (dev/, staging/, prod/).
    - Add services/ for microservices.

- **New Microservice**:
    - Create projects/<project>/services/new-service/.
    - Add base/config.yaml and docker-compose.override.yaml.
    - Update project README.
    - See docs/examples/add-service.md for a template.
- Use templates in Resources.

## 🤝 Contributing
Link to contribution guidelines and process.

- Follow CONTRIBUTING.md for details.
- Run validation: ./tools/compose-validate.sh.
- Submit PRs with one review minimum (see Tools).

## 🛠️ Troubleshooting
- Invalid YAML: Run yamllint **/*.yaml.
- Docker Compose errors: Check with docker-compose -f <file> config.
- Missing secrets: Ensure .env or vault is configured.

Open an issue for help.

## 📜 License
This project is licensed under the MIT License.

## 📚 Resources
- Adding a Service Template
- Architecture Diagram
- Tools

## 🛠️ Tools
- `validate-configs.sh`: Lints YAML files.
- `compose-validate.sh`: Validates docker-compose files.

### Usage Notes
- **Customization**: Replace placeholders (e.g., `StarOne`) with your GitHub org name. Add project-specific examples if needed (e.g., sportstats’ docker-compose).
- **Tone**: Keep it professional yet approachable, as seen in open-source repos like Kubernetes or Spring.
- **Maintenance**: Update when adding projects or changing workflows. Keep examples current.
- **Markdown Best Practices**: Use headers, code blocks, and links for clarity. Avoid clutter.
---



