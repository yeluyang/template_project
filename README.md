# Project Template

```tree
# Layers are ordered top-down by dependency: each layer may depend on layers below it, but never the reverse.
# Dependencies target interfaces (ports), not concrete implementations — except for shared packages: utils/, configs/, common/.
./
├── boot/ # Application entry points — handles environment initialization and dependency injection. Each subdirectory represents a separate executable.
│   ├── some_cronjob_boot/
│   └── some_server_boot/
├── tools/ # Project-specific tooling: build scripts, operational utilities, client CLIs, code generators, etc.
│   └── some_tools/ # Isolated subdirectory for tools with non-trivial logic spanning multiple files
├── app/ # Application layer — orchestrates use cases by composing domain services and repository operations
│   ├── cronjob/ # Scheduled task (cron job) use cases
│   ├── handler/ # RPC/HTTP request handlers (controller layer)
│   ├── middleware/ # Transport middleware (e.g., RPC interceptors, HTTP middleware hooks)
│   ├── srv/
│   │   └── some_app_service_impl/ # Application service implementations
│   └── interfaces.xx # Application service interfaces (ports)
├── domain/ # Domain layer — core business logic, independent of infrastructure and transport concerns
│   ├── entity/ # Aggregate roots and complex domain entity clusters
│   │   └── some_domain_entity/
│   ├── srv/ # Domain service implementations
│   │   └── some_domain_service_impl/
│   ├── entities.xx # Simple domain entity definitions (value objects, standalone entities)
│   └── interfaces.xx # Domain service interfaces (ports)
├── repo/ # Repository (persistence) layer — encapsulates all data access logic (CRUD operations)
│   ├── entity/
│   │   └── some_persistent_entity/ # Complex or interrelated persistent entity clusters
│   ├── some_data_repository_impl/ # Repository interface implementations
│   ├── entities.xx # Simple persistent entity definitions (data models / table mappings)
│   └── interfaces.xx # Repository interfaces (ports)
├── infra/ # Infrastructure adapter layer — wraps external systems (Redis, MySQL, etc.) behind project-specific interfaces
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
│   │   ├── clients.xx # RDS client/connection registry — distinguishes between different database backends
│   │   └── interfaces.xx # Unified relational database interface — abstracts over MySQL, PostgreSQL, SQLite, etc.
│   └── rpc/
├── utils/ # Business-aware shared utilities — cross-cutting helpers tied to specific business scenarios, reused across multiple layers
│   ├── other_utils/
│   └── errs/
│       ├── codes.xx # Project-wide error code definitions
│       └── error.xx # Custom error types tailored to the project
├── configs/ # Configuration definitions
│   └── static/ # Static configuration assets (embedded resource files)
├── api/
│   ├── idl/ # Interface definition files (e.g., .proto, .thrift)
│   └── gen/ # Auto-generated code from IDL definitions
└── common/ # Business-agnostic shared libraries — fully generic code reusable across any project
   ├── other_common/
   └── utils/
```
