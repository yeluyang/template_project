# 架构设计

## 依赖规则

```text
bootstrap（组合根）
├── handlers ─> app 接口
├── app impl ─> domain + repo 接口
├── repo impl ─> infra 接口
└── infra impl

api, common ─> 供各层按需使用
```

- `bootstrap/` 是组合根，负责装配可执行程序，因此可以直接依赖具体实现。
- 除 `bootstrap/` 外，各层之间的行为依赖应面向接口，不应直接依赖具体实现。
- `handlers/` 负责将 RPC、HTTP 或定时任务等输入转换为对 app 接口的调用，不承载业务逻辑。
- `app/` 通过 domain service 和 repo 接口编排应用用例。
- `domain/` 包含业务实体与规则，不依赖交付层或基础设施代码。
- `repo/` 基于 domain 实体和 infra client 实现数据持久化。
- `infra/` 通过项目内部定义的接口封装数据库等外部系统。
- `api/` 和 `common/` 存放共享契约与通用工具；业务规则应保留在 `domain/` 中。

## 项目结构

```text
.
├── bootstrap/                     # 可执行程序装配与依赖注入
│   └── some_server_bootstrap/
├── tools/                         # 构建脚本、CLI 和代码生成器
│   └── some_tools/
├── handlers/                      # 传输与定时任务适配器
│   ├── cronjob/
│   ├── grpc/
│   │   └── middleware/
│   │       └── some_handler_middleware_impl/
│   └── http/
│       └── middleware/
├── app/                           # 应用用例编排
│   ├── impl/
│   └── interfaces.xx
├── domain/                        # 核心业务模型与规则
│   ├── entity/
│   │   ├── some_domain_entity/
│   │   └── entities.xx
│   └── srv/
│       ├── impl/
│       └── interfaces.xx
├── repo/                          # 数据持久化接口及其实现
│   ├── impl/
│   └── interfaces.xx
├── infra/                         # 外部系统 client 与适配器
│   └── rds/
│       ├── impl/
│       │   ├── mysql/
│       │   │   └── impl.xx
│       │   └── postgres/
│       │       └── impl.xx
│       ├── clients.xx
│       └── interfaces.xx
├── common/                        # 共享错误定义与通用工具
│   ├── errs/
│   │   ├── codes.xx
│   │   └── error.xx
│   ├── types/
│   └── utils/
├── api/                           # IDL 与生成的 API 代码
│   ├── idl/
│   └── gen/
└── docs/                          # 项目文档
```

## 分层概览

| 分层      | 路径         | 职责                                           |
| --------- | ------------ | ---------------------------------------------- |
| Bootstrap | `bootstrap/` | 初始化运行环境、组装依赖并启动可执行程序。     |
| Tools     | `tools/`     | 存放项目专用的构建和运维工具。                 |
| Handlers  | `handlers/`  | 将 RPC、HTTP 和定时任务等输入适配为 app 调用。 |
| App       | `app/`       | 定义并实现应用用例。                           |
| Domain    | `domain/`    | 负责定义并维护业务实体、服务和规则。           |
| Repo      | `repo/`      | 定义持久化接口并实现数据访问。                 |
| Infra     | `infra/`     | 封装数据库及其他外部系统。                     |
| Common    | `common/`    | 提供共享错误定义和与业务无关的通用工具。       |
| API       | `api/`       | 存放接口定义和生成代码。                       |
| Docs      | `docs/`      | 存放项目文档。                                 |

## 数据结构

- 领域实体统一定义在 `domain/entity/` 中。
- 各层接口专用的入参和出参数据结构，应与对应的接口定义放在一起。
- 跨层、跨业务场景复用的通用数据结构定义在 `common/types/` 中。
