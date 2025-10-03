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
- **Ecommerce**: Manages online store operations with microservices (e.g., cart-service, order-service).
  - Dependencies: MySQL, Redis.
  - See [Ecommerce Structure](#ecommerce-structure) for details.

## 📂 Repository Structure

This repository contains configuration files for multiple applications as well as shared/global configs. 
Each application has environment-specific (`dev`, `prod`) and core settings, along with documentation.

```
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



