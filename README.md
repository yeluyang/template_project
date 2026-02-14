# Project Template

## Dependency Rules

- Layers are ordered **top-down** by dependency: each layer may depend on layers below it, but **never** the reverse.
- Dependencies target **interfaces** (ports), not concrete implementations — except for shared packages: `api/`, `utils/`, `configs/`, `common/`.

## Project Structure

```tree
.
├── boot/                          # Entry points & dependency injection
│   ├── some_cronjob_boot/
│   └── some_server_boot/
│
├── tools/                         # Build scripts, CLIs, code generators
│   └── some_tools/
│
├── app/                           # Application layer (use case orchestration)
│   ├── cronjob/                   #   Scheduled task use cases
│   ├── handler/                   #   RPC / HTTP request handlers
│   ├── middleware/                #   Transport middleware & interceptors
│   ├── srv/
│   │   └── some_app_service_impl/
│   └── interfaces.xx              #   Application service interfaces (ports)
│
├── domain/                        # Domain layer (core business logic)
│   ├── entity/                    #   Aggregate roots & entity clusters
│   │   └── some_domain_entity/
│   ├── srv/                       #   Domain service implementations
│   │   └── some_domain_service_impl/
│   ├── entities.xx                #   Value objects & standalone entities
│   └── interfaces.xx              #   Domain service interfaces (ports)
│
├── repo/                          # Repository layer (data access / CRUD)
│   ├── entity/
│   │   └── some_persistent_entity/
│   ├── some_data_repository_impl/
│   ├── entities.xx                #   Data models / table mappings
│   └── interfaces.xx              #   Repository interfaces (ports)
│
├── infra/                         # Infrastructure adapters (external systems)
│   ├── cache/
│   ├── kv/
│   ├── mq/
│   ├── oss/
│   ├── other_infra/
│   ├── rds/
│   │   ├── mysql/
│   │   │   └── impl.xx
│   │   ├── postgres/
│   │   │   └── impl.xx
│   │   ├── clients.xx             #   Client / connection registry
│   │   └── interfaces.xx          #   Unified RDS interface
│   └── rpc/
│
├── utils/                         # Business-aware shared utilities
│   ├── other_utils/
│   └── errs/
│       ├── codes.xx               #   Error code definitions
│       └── error.xx               #   Custom error types
│
├── configs/                       # Configuration definitions
│   └── static/                    #   Embedded resource files
│
├── api/                           # API definitions
│   ├── idl/                       #   IDL files (.proto, .thrift, …)
│   └── gen/                       #   Auto-generated code
│
└── common/                        # Business-agnostic shared libraries
    ├── other_common/
    └── utils/
```

## Layer Overview

| Layer       | Path       | Responsibility                                                                                                              |
| ----------- | ---------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Boot**    | `boot/`    | Application entry points — environment initialization and dependency injection. Each subdirectory is a separate executable. |
| **Tools**   | `tools/`   | Project-specific tooling: build scripts, operational utilities, client CLIs, code generators.                               |
| **App**     | `app/`     | Application layer — orchestrates use cases by composing domain services and repository operations.                          |
| **Domain**  | `domain/`  | Domain layer — core business logic, independent of infrastructure and transport concerns.                                   |
| **Repo**    | `repo/`    | Repository layer — encapsulates all data access logic (CRUD operations).                                                    |
| **Infra**   | `infra/`   | Infrastructure adapters — wraps external systems (Redis, MySQL, MQ, etc.) behind project-specific interfaces.               |
| **Utils**   | `utils/`   | Business-aware shared utilities — cross-cutting helpers tied to specific business scenarios, reused across layers.          |
| **Configs** | `configs/` | Configuration definitions and static assets.                                                                                |
| **API**     | `api/`     | Interface definitions (IDL) and auto-generated code.                                                                        |
| **Common**  | `common/`  | Business-agnostic shared libraries — fully generic, reusable across any project.                                            |
