# Magic Classroom AI

Magic Classroom AI（魔法课堂）是一套基于 AI 的沉浸式互动数字课程平台。

目标：通过 AI、互动动画、游戏化学习和数字教材技术，将传统教材、教案或结构化教学内容转化为可交互、可复用、可批量生产的网页课程。

## 当前阶段

### Phase 1：领导演示 Demo
- 首个示范课程：Magic Journey · Explore Britain
- 最高优先级：Scene 01 开场动画（序号1）
- 目标：快速形成可演示的完整学习体验

### Phase 2：产品化平台
- JSON 驱动课程播放器
- 课程模板系统
- 教师课程管理
- 素材管理
- 版本与发布

### Phase 3：AI 自动生成课程
- 教材解析
- AI 课程规划
- 模板匹配
- JSON 草稿生成
- 人工审核
- 发布

## Codex 从这里开始

首次让 Codex 开发时，按以下顺序阅读：

1. `codex/START-HERE.md`
2. `docs/16-Codex总执行Prompt.md`
3. `docs/01-PRD-产品需求文档.md`
4. `docs/03-技术架构设计.md`
5. `docs/06-数据结构规范.md`
6. `docs/08-Codex开发任务清单.md`
7. 当前任务对应的 `codex/tasks/*.md`

第一项任务优先读取：

`codex/tasks/scene-01-intro.md`

后续场景统一任务书：

`codex/tasks/scene-02-to-08.md`

## 文档导航

### 产品与规划
- `docs/01-PRD-产品需求文档.md`
- `docs/02-产品架构设计.md`
- `docs/12-产品路线图.md`
- `docs/14-正式上线Checklist.md`

### UI 与组件
- `docs/04-UI设计规范.md`
- `docs/05-组件设计规范.md`

### 技术与数据
- `docs/03-技术架构设计.md`
- `docs/06-数据结构规范.md`
- `docs/07-API接口设计.md`
- `docs/15-数据库ER设计.md`
- `docs/17-OpenAPI接口完整文档.md`
- `docs/18-前端项目初始化规范.md`
- `docs/19-FastAPI后端架构.md`
- `docs/20-Docker与部署设计.md`
- `docs/21-CI-CD自动部署方案.md`
- `docs/23-数据库迁移与初始化规范.md`
- `docs/24-测试用例清单.md`

### AI / Codex
- `docs/08-Codex开发任务清单.md`
- `docs/09-AI提示词库.md`
- `docs/16-Codex总执行Prompt.md`
- `codex/phase-01-demo.md`
- `codex/phase-02-product.md`
- `codex/tasks/scene-01-intro.md`
- `codex/tasks/scene-02-to-08.md`

### 运营、上线与合规
- `docs/10-服务器部署方案.md`
- `docs/11-测试验收标准.md`
- `docs/13-Git工作流与提交规范.md`
- `docs/14-正式上线Checklist.md`
- `docs/22-素材版权与合规规范.md`

## 机器可执行规范

- OpenAPI：`openapi/openapi.yaml`
- 课程 JSON Schema：`schemas/course.schema.json`
- 本地容器编排：`docker-compose.yml`
- CI：`.github/workflows/ci.yml`
- 环境变量模板：`.env.example`

注意：当前 `docker-compose.yml` 和 CI workflow 属于工程骨架，在 `apps/web` 与 `apps/api` 实际初始化后由 Codex 对齐真实命令和依赖。

## 示例课程

### 英国示范课程
`examples/magic-journey-britain/course.json`

用于第一版领导演示，包含 Scene 01～08。

### 第二门模板验证课程
`examples/magic-journey-china/course.json`

用于验证模板系统不是针对单一英国课程硬编码。

## JSON Schema

`schemas/course.schema.json`

所有课程配置应尽量通过该 Schema 校验。后续 schemaVersion 发生不兼容变化时必须升级主版本。

## 环境变量

复制：

```bash
cp .env.example .env
```

任何 Secret、数据库密码、AI API Key 都不得提交到 GitHub。

## 第一阶段推荐技术栈

前端：React + TypeScript + Vite + Tailwind CSS + Framer Motion + React Router + Zustand

后端产品化阶段：FastAPI + PostgreSQL + Redis + S3 兼容对象存储

## 开发基本原则

1. 课程内容与平台代码分离。
2. 配置驱动优先，不为每门课复制 React 页面。
3. 组件和 Scene 模板可复用。
4. 第一版控制范围，不以复杂 3D 为前置条件。
5. AI 生成结果必须允许人工审核。
6. 不把密钥写入代码。
7. 不使用未经授权的影视角色、官方美术或来源不明素材。
8. 每个 Codex 任务结束后执行 lint/typecheck/test/build，并汇报改动文件和未完成项。
9. 数据库变更必须通过 migration。
10. API 契约优先以 OpenAPI 对齐前后端。

## 当前最优先开发顺序

```text
P0
1. 项目初始化
2. CoursePlayer
3. Course JSON 读取与校验
4. Scene 01 Intro
5. Scene 02 Map Explore
6. Scene 03 Timeline
7. 场景导航/音频/全屏
8. Demo 部署

P1
9. Reading / Quiz / Matching / Summary / Result
10. 教师课程管理基础
11. FastAPI + PostgreSQL
12. Asset 上传
13. 发布版本

P2
14. AI 教材解析
15. AI 课程生成
16. 多学校/租户
17. 完整分析与运维
```

## 开发前检查

在真正开始生成代码前，请先确认：
- `apps/web` 是否已经初始化。
- `apps/api` 是否已经初始化。
- CI 中的脚本名称与 package.json/requirements 实际一致。
- `.env` 只存在本地或部署 Secret 中。
- Demo 使用的素材均为原创、授权或可合法使用素材。
