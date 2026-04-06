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
- **Global Commons**: Shared configs (e.g., `common/base.yml`) for logging, timeouts, etc.
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
├── 📄 VERSION                   # (v9.3.0)
├── 📂 common/                   # Shared by EVERY service in StarOne & DHS
│   ├── 📄 spring-base.yml       # Port, Actuator, Banner
│   ├── 📄 logging.yml           # Logback/ELK patterns
│   ├── 📄 observability.yml     # Micrometer/Zipkin/Prometheus
│   └── 📂 security/
│       ├── 📄 oauth2.yml
│       └── 📄 rbac.yml
├── 📂 components/               # Third-party "Ready-to-use" blocks
│   ├── 📂 mysql/
│   │   └── 📄 defaults.yml      # Driver, Pool size, Dialect
│   ├── 📂 kafka/
│   └── 📂 redis/
├── 📂 environments/             # Global environment overrides
│   ├── 📄 dev.yml               # Global Dev DB URLs
│   └── 📄 prod.yml              # Global Production limits
├── 📂 projects/                 # Business-specific hierarchies
│   ├── 📂 starone-galaxy/       # Project: StarOne Galaxy
│   │   ├── 📄 project.yml       # Shared by BookShow, SportStats, VaultIron
│   │   └── 📂 bookshow/         # Application: BookShow
│   │       ├── 📂 booking-svc/  # Microservice
│   │       │   ├── 📄 base.yml  # Svc-specific logic
│   │       │   └── 📄 dev.yml   # Svc-specific dev overrides
│   │       └── 📂 movie-svc/
│   └── 📂 dhs/                  # Project: Distributed Hub & Sales
│       ├── 📄 project.yml
│       └── 📂 sales-svc/
└── 📂 tools/                    # DevOps Scripts (Validation/Linting)

```
## 🏟️ Sportstats Structure

- `common/docker-compose.yml`: Defines MySQL, PostgreSQL, Kafka.
- `services/user-service/`: User management configs.
- `services/stats-service/`: Stats processing configs.
    
## 🛒 dhs Structure

- `common/docker-compose.yml`: Defines MySQL, Redis.
- `services/cart-service/`: Shopping cart configs.
- `services/order-service/`: Order processing configs.
    
## 🔧 Prerequisites

- **Git**: For cloning and managing the repo.
- **Docker**: To run docker-compose files.
- **yq**: For merging yml configs (sudo apt install yq or equivalent).
- **ymllint**: For validation (pip install ymllint).
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
    pip install ymllint
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
    docker-compose -f configs/projects/sportstats/common/docker-compose.yml -f configs/projects/sportstats/services/user-service/ docker-compose.override.yml up
    ```

- **Merge Configs**: Combine global, project, and service configs
- bash
    ```
    yq eval-all '. as $item ireduce ({}; . * $item)' configs/common/base.yml configs/projects/sportstats/base/config.yml > merged.yml
    ```

- **Secrets**: Use .env files or a vault for variables like ${MYSQL_ROOT_PASSWORD}.

## ➕ Adding a New Project or Microservice
Guide users on extending the repo.

- **New Project**:
    - Create projects/new-project/.
    - Add common/docker-compose.yml for shared infra.
    - Create base/config.yml and environment folders (dev/, staging/, prod/).
    - Add services/ for microservices.

- **New Microservice**:
    - Create projects/<project>/services/new-service/.
    - Add base/config.yml and docker-compose.override.yml.
    - Update project README.
    - See docs/examples/add-service.md for a template.
- Use templates in Resources.

## 🤝 Contributing
Link to contribution guidelines and process.

- Follow CONTRIBUTING.md for details.
- Run validation: ./tools/compose-validate.sh.
- Submit PRs with one review minimum (see Tools).

## 🛠️ Troubleshooting
- Invalid yml: Run ymllint **/*.yml.
- Docker Compose errors: Check with docker-compose -f <file> config.
- Missing secrets: Ensure .env or vault is configured.

Open an issue for help.

## 📜 License
This project is licensed under the MIT License.

## 📚 Resources 

- [Architecture Diagram](docs/architecture.md) 

- [Adding a New Service Template](docs/examples/add-new-service.md) 

- [Environment Guide](docs/ENVIRONMENT_GUIDE.md) 

## 🛠️ Tools
- `validate-configs.sh`: Lints yml files.
- `compose-validate.sh`: Validates docker-compose files.




