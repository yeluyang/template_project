# Architecture

## Dependency Rules

```text
bootstrap (composition root)
├── handlers ─> app interfaces
├── app impl ─> domain + repo interfaces
├── repo impl ─> domain entities + infra interfaces
└── infra impl

api, common ─> shared where needed
```

- `bootstrap/` is the composition root. It may depend on concrete implementations to assemble an executable.
- Outside `bootstrap/`, behavioral dependencies target interfaces rather than concrete implementations.
- `handlers/` translates transport or scheduling input and invokes application interfaces; it contains no business logic.
- `app/` orchestrates use cases through domain services and repository interfaces.
- `domain/` contains business entities and rules and does not depend on delivery or infrastructure code.
- `repo/` implements persistence using domain entities and infrastructure clients.
- `infra/` wraps external systems behind project-owned interfaces.
- `api/` and `common/` contain shared contracts and utilities. Keep business rules in `domain/`.

## Project Structure

```text
.
├── bootstrap/                     # Executable assembly and dependency injection
│   └── some_server_bootstrap/
├── tools/                         # Build scripts, CLIs, and code generators
│   └── some_tools/
├── handlers/                      # Transport and scheduled-task adapters
│   ├── cronjob/
│   ├── grpc/
│   │   └── middleware/
│   │       └── some_handler_middleware_impl/
│   └── http/
│       └── middleware/
├── app/                           # Application use-case orchestration
│   ├── impl/
│   └── interfaces.xx
├── domain/                        # Core business model and rules
│   ├── entity/
│   │   ├── some_domain_entity/
│   │   └── entities.xx
│   └── srv/
│       ├── impl/
│       └── interfaces.xx
├── repo/                          # Persistence ports and implementations
│   ├── impl/
│   └── interfaces.xx
├── infra/                         # External-system clients and adapters
│   └── rds/
│       ├── impl/
│       │   ├── mysql/
│       │   │   └── impl.xx
│       │   └── postgres/
│       │       └── impl.xx
│       ├── clients.xx
│       └── interfaces.xx
├── common/                        # Shared errors and generic utilities
│   ├── errs/
│   │   ├── codes.xx
│   │   └── error.xx
│   └── utils/
├── api/                           # IDL and generated API code
│   ├── idl/
│   └── gen/
└── docs/                          # Project documentation
```

## Layer Overview

| Layer     | Path         | Responsibility                                                           |
| --------- | ------------ | ------------------------------------------------------------------------ |
| Bootstrap | `bootstrap/` | Initializes the environment, wires dependencies, and starts executables. |
| Tools     | `tools/`     | Hosts project-specific build and operational tooling.                    |
| Handlers  | `handlers/`  | Adapts RPC, HTTP, and scheduled inputs to application calls.             |
| App       | `app/`       | Defines and implements application use cases.                            |
| Domain    | `domain/`    | Owns business entities, services, and rules.                             |
| Repo      | `repo/`      | Defines persistence ports and implements data access.                    |
| Infra     | `infra/`     | Wraps databases and other external systems.                              |
| Common    | `common/`    | Provides shared errors and business-agnostic utilities.                  |
| API       | `api/`       | Stores interface definitions and generated code.                         |
| Docs      | `docs/`      | Stores project documentation.                                            |
