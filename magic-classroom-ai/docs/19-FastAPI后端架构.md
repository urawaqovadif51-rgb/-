# 19. FastAPI 后端架构

## 1. 技术栈
- Python 3.12+
- FastAPI
- Pydantic v2
- SQLAlchemy 2.x
- Alembic
- PostgreSQL
- Redis（MVP 可选，AI Job/缓存阶段启用）
- S3 兼容对象存储

## 2. 目录结构
```text
backend/
├── app/
│   ├── main.py
│   ├── api/
│   │   └── v1/
│   ├── core/
│   ├── db/
│   ├── models/
│   ├── schemas/
│   ├── services/
│   ├── repositories/
│   ├── workers/
│   └── tests/
├── alembic/
├── pyproject.toml
└── Dockerfile
```

## 3. 分层职责
- api：HTTP 参数、鉴权入口、状态码。
- schemas：Pydantic 输入输出模型。
- services：业务逻辑。
- repositories：数据库访问。
- models：ORM。
- workers：AI 生成、解析、异步任务。
- core：配置、安全、日志、异常。

禁止把复杂业务全部写进路由函数。

## 4. 鉴权
MVP 可使用 JWT access + refresh；正式版需支持撤销/刷新策略、密码哈希、登录限流。

角色：student、teacher、school_admin、platform_admin。

## 5. 多租户
所有学校级业务表必须可通过 `tenant_id` 或等价关系限定范围。服务层查询默认带租户约束，避免越权读取。

## 6. 数据库
- PostgreSQL 为主库。
- Alembic 管理迁移。
- 生产环境不得自动 `create_all` 替代迁移。
- 已发布 CourseVersion 尽量不可变，新修改生成新版本。

## 7. 对象存储
前端不通过 API 中转大文件上传，优先：
1. 请求预签名 URL。
2. 浏览器直传对象存储。
3. 调用 complete API 登记 Asset。

## 8. AI Job
AI 生成任务采用状态机：
`queued -> parsing -> planning -> generating -> validating -> completed/failed`

长任务不得阻塞普通 HTTP worker。MVP 可先使用简单后台任务，正式版迁移 Celery/RQ/Arq 或云队列。

## 9. 日志
结构化日志至少包含：request_id、user_id（可用时）、tenant_id、route、status、latency。
不得记录密码、token、Secret Key。

## 10. 异常
统一业务异常 → 统一 JSON 错误；不要把数据库原始异常直接返回浏览器。

## 11. 测试
最低覆盖：
- Auth
- Tenant isolation
- Course CRUD
- Publish
- Asset upload complete
- Public course
- Learning session
- Generation job 状态流

## 12. 健康检查
- `/health/live`
- `/health/ready`

readiness 可检查数据库；不要把第三方 AI 暂时不可用直接等同整个站点不可用。

## 13. 安全底线
- CORS 白名单。
- 上传类型/大小限制。
- 登录、AI 生成、公开写接口限流。
- Secret 只从环境变量/密钥服务读取。
- ORM 查询参数化。
- 管理接口显式权限检查。
