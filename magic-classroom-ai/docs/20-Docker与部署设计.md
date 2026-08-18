# 20. Docker 与部署设计

## 1. 目标
统一开发、测试和生产环境，降低“本地能跑、服务器不能跑”的概率。

## 2. 服务划分
- `frontend`：构建 React 静态文件。
- `backend`：FastAPI。
- `postgres`：数据库。
- `redis`：缓存/异步任务。
- `worker`：AI/文档解析后台任务。
- 对象存储：优先使用外部 S3 兼容服务，不建议和应用绑在同一容器中。

## 3. 本地 docker-compose
建议包含：backend、postgres、redis；frontend 开发阶段可继续 `npm run dev`，生产预览再容器化。

## 4. Frontend Dockerfile
多阶段构建：Node 安装依赖并 `npm run build`，最终由 Nginx/Caddy/静态托管服务提供 dist。

## 5. Backend Dockerfile
- Python slim 基础镜像。
- 非 root 用户运行。
- 安装锁定依赖。
- 启动 FastAPI ASGI server。
- 容器启动前执行迁移应由部署流程显式控制，避免多副本同时跑迁移。

## 6. Production 拓扑
小规模：CDN/静态前端 + 1~2 个 API 实例 + PostgreSQL + Redis + 对象存储。

规模增长后：负载均衡 + 多 API 实例 + 独立 worker + 托管数据库/Redis + CDN。

## 7. Secret
不得写入镜像、Dockerfile、仓库或 compose 明文。生产使用部署平台 Secret/环境变量。

## 8. 数据持久化
数据库和 Redis 的持久数据不得依赖容器临时文件层。PostgreSQL 必须有持久卷/托管数据库与备份。

## 9. 发布顺序
1. CI 测试通过。
2. 构建不可变镜像/tag。
3. 备份数据库。
4. 执行迁移。
5. 部署后端。
6. 健康检查。
7. 发布前端。
8. Smoke Test。
9. 观察错误率和延迟。

## 10. 回滚
- 应用镜像保留上一个稳定版本。
- 数据库迁移必须设计 downgrade 或前向兼容方案。
- 前端静态版本可快速切换。

## 11. Demo
领导 Demo 不需要先上完整 Docker 集群。第一版可静态托管前端；真实后端需求出现后再启用本设计。
