# Magic Classroom AI — Codex 开发任务清单

> 目标：将项目拆成可交给 Codex 顺序执行、可验收、可回滚的任务。
> 原则：每个 Task 尽量控制在 30~120 分钟内，完成后提交 Git，再继续下一项。

## 0. 总体规则
- 不一次生成整个项目。
- 每个任务开始前先读取 README、PRD、技术架构和当前目录。
- 每个任务必须说明：目标、修改文件、验收标准、测试方式。
- 完成后运行 lint/typecheck/test/build。
- 禁止把密钥写入仓库。
- 不直接复制受版权保护影视作品的角色、美术和音乐素材。

# Phase 1：领导演示 Demo

## Task 001 — 初始化前端工程
目标：创建 React + Vite + TypeScript 项目。

要求：
- 安装 React Router、Framer Motion、Zustand。
- 建立 ESLint/Prettier 基础规则。
- 配置路径别名 `@/`。

验收：
- `npm run dev` 正常。
- `npm run build` 成功。
- TypeScript 无错误。

## Task 002 — 建立目录架构
创建：
- src/app
- src/pages
- src/components
- src/features
- src/templates
- src/course-engine
- src/config
- src/data
- src/assets
- src/hooks
- src/store
- src/types
- src/utils

验收：目录职责写入 README。

## Task 003 — 全局设计 Token
建立 CSS variables / Tailwind token：背景、表面、金色强调色、文字、间距、圆角、阴影、层级、动画时长。

验收：禁止在业务组件里重复散落大量魔法数字颜色值。

## Task 004 — AppShell 与路由
路由至少包括：
- `/demo/britain`
- `/demo/britain/scene/:sceneId`
- 404

验收：直接刷新场景 URL 不白屏。

## Task 005 — CourseConfig 类型
定义 Course、Scene、Asset、Interaction、Reward 的 TypeScript 类型。

验收：示范课程 JSON 可通过类型检查。

## Task 006 — Demo Course JSON
建立 `magic-journey-britain` 示例配置，包含 8 个场景骨架。

验收：所有主要文案和素材路径来自配置，不写死在页面。

## Task 007 — CoursePlayer 骨架
实现：读取课程、当前场景、next/prev、progress、错误 fallback。

验收：可在 8 个空场景间前后切换。

## Task 008 — Scene Registry
建立 `scene.type -> template component` 注册表。

验收：未知 scene.type 显示明确错误卡片，不导致崩溃。

# Scene 01 开场动画

## Task 009 — IntroScene 页面布局
完成 16:9 沉浸式布局：背景、左侧标题、中间原创导师、右侧地图、右侧工具栏、开始按钮。

验收：1920×1080、1366×768 均正常。

## Task 010 — 背景分层
实现天空/远景/前景/暗角/光效分层，并提供素材缺失 fallback。

验收：无素材时仍可读、可操作。

## Task 011 — 标题动画
使用 Framer Motion 实现标题、副标题和介绍文案进入。

验收：reduced motion 下自动简化。

## Task 012 — 原创 AI 导师组件
静态角色图 + 轻微漂浮/呼吸，角色名称和图片均配置化。

验收：无图片时显示占位，不破坏布局。

## Task 013 — BritainMap 组件
SVG/图片地图，支持节点或区域高亮。

验收：至少 England、Scotland、Wales、Northern Ireland 数据结构可配置。

## Task 014 — Magic Start Button
Hover、focus、press、发光效果；点击触发场景切换。

验收：键盘 Enter/Space 可触发。

## Task 015 — Particle Background
轻量粒子效果，不阻塞主线程。

验收：低性能模式/减少动画可关闭。

## Task 016 — Toolbar
目录、声音、全屏。

验收：有 aria-label、tooltip、focus 状态。

## Task 017 — Audio Manager
BGM 状态、音量、静音；首次用户交互后播放。

验收：不依赖强制 autoplay。

## Task 018 — Fullscreen
封装 Fullscreen API 并处理浏览器不支持/拒绝。

## Task 019 — Scene Transition
场景淡出/淡入并避免快速连点造成状态错乱。

## Task 020 — Scene01 性能优化
图片压缩、懒加载、预加载关键资源。

验收：Chrome Performance 无明显长任务由动画持续造成。

# 其余 Demo 场景

## Task 021 — MapExplore 模板
支持地图热点、信息卡、完成状态。

## Task 022 — Timeline 模板
支持时间节点、图文面板、顺序浏览。

## Task 023 — Reading 模板
支持文章、重点词、问题区。

## Task 024 — Quiz 模板
单选/多选基础实现。

## Task 025 — Matching 模板
拖拽或点击匹配，提供键盘替代方式。

## Task 026 — Summary 模板
知识点总结和复习按钮。

## Task 027 — Result 模板
分数、金币/徽章、完成状态。

## Task 028 — Demo 全链路
8 个场景可按顺序完成，结果页可返回首页。

## Task 029 — Demo 数据持久化
localStorage 保存 currentScene、score、audio preference。

## Task 030 — Demo 发布
Vercel/Cloudflare Pages 等静态部署，提供环境说明。

# Phase 2：产品化 MVP

## Task 031 — Monorepo/前后端目录规划
建议 apps/web、apps/api、packages/shared、packages/course-schema。

## Task 032 — 后端初始化
FastAPI 或 Node/Nest 二选一；选择后在 ADR 记录原因。

## Task 033 — PostgreSQL 数据库
建立 migration 系统。

## Task 034 — 用户模型
User、Role、Tenant/School。

## Task 035 — 身份认证
安全登录、session/JWT 策略、密码哈希。

## Task 036 — RBAC 权限
student/teacher/school_admin/platform_admin。

## Task 037 — Course 数据模型
课程基础信息、状态、owner、tenant。

## Task 038 — CourseVersion
版本化课程 JSON；发布版本不可被直接覆盖。

## Task 039 — Scene 数据存储
场景既可作为 JSONB，也需明确 schemaVersion。

## Task 040 — Asset 模型
文件元数据、对象存储 key、MIME、大小、owner。

## Task 041 — 对象存储接入
S3/OSS/COS 兼容抽象层。

## Task 042 — 上传接口
限制类型、大小、后缀；服务端校验。

## Task 043 — 教师课程列表
分页、搜索、状态筛选。

## Task 044 — 创建课程
基础信息表单。

## Task 045 — 场景编辑器骨架
左侧场景列表、中部预览、右侧属性面板。

## Task 046 — 场景排序
支持排序并持久化。

## Task 047 — 场景配置表单
根据 scene.type 动态加载 schema 表单。

## Task 048 — 预览模式
未发布版本可预览。

## Task 049 — 发布流程
draft → validation → published。

## Task 050 — 发布 URL
稳定课程访问地址和版本策略。

## Task 051 — 复制课程
复制配置，不重复复制公共素材。

## Task 052 — 模板管理模型
Template + version + schema。

## Task 053 — 模板管理后台
平台管理员查看和启停模板。

## Task 054 — 学习 Session
记录用户/匿名学习会话。

## Task 055 — InteractionRecord
记录答题/点击等必要学习事件。

## Task 056 — 基础教师统计
完成数、平均进度、基础得分。

## Task 057 — 审计日志
对发布、删除、权限变更等记录审计。

## Task 058 — 错误处理标准
统一 API error envelope 和前端错误边界。

## Task 059 — Observability
结构化日志、request id、基本 metrics。

## Task 060 — MVP 部署
前端、API、DB、对象存储分别部署。

# Phase 3：AI 课程生成

## Task 061 — GenerationJob 模型
状态：queued/running/needs_review/succeeded/failed/cancelled。

## Task 062 — 教材上传入口
只接收允许类型并限制大小。

## Task 063 — 文档解析服务
抽取文本、页码/章节元信息。

## Task 064 — 内容结构化 Prompt
生成课程主题、目标、章节和知识点。

## Task 065 — Scene Planner
根据知识点选择 intro/map/timeline/reading/quiz 等模板。

## Task 066 — JSON 生成器
输出符合 Course Schema 的结构化结果。

## Task 067 — JSON 校验与修复
Schema validate；失败时有限次数 repair，不无限循环。

## Task 068 — 教师 Review UI
AI 草稿必须可以人工修改。

## Task 069 — Prompt 版本化
保存 promptVersion/model/provider/parameters。

## Task 070 — 成本记录
记录 token/请求量/估算费用。

## Task 071 — 任务队列
耗时生成走异步队列，不阻塞 HTTP 请求。

## Task 072 — 重试与幂等
同一 job 不重复创建多个课程版本。

## Task 073 — 内容安全校验
对生成内容进行基础规则检查和人工审核入口。

## Task 074 — 素材建议
AI 给出素材需求列表，不自动使用不明授权素材。

## Task 075 — AI 素材生成接口抽象
供应商可替换，不把具体 provider 写死在业务层。

# Phase 4：规模化

## Task 076 — 多租户隔离测试
保证学校数据不可跨租户读取。

## Task 077 — CDN 与缓存
课程静态 JSON 和素材 CDN 化。

## Task 078 — Redis
仅在确有缓存/队列/限流需求时引入。

## Task 079 — Rate Limit
登录、上传、AI 生成等关键接口限流。

## Task 080 — 配额系统
按学校/套餐管理存储、生成次数等配额。

## Task 081 — 数据备份
数据库自动备份和恢复演练。

## Task 082 — CI
PR 自动 lint/typecheck/test/build。

## Task 083 — CD
main/tag 部署，生产环境需审批策略。

## Task 084 — E2E
覆盖登录→创建→编辑→预览→发布→学生学习。

## Task 085 — 压测
根据实际并发模型进行，不用“注册人数”等同并发人数。

## Task 086 — 安全检查
依赖漏洞、权限、上传、secret scanning。

## Task 087 — Feature Flags
逐步开放 AI、实验模板等功能。

## Task 088 — 数据导出
教师/学校按权限导出必要数据。

## Task 089 — 国际化
界面和课程内容分离，为中英双语预留。

## Task 090 — 正式发布清单
域名、HTTPS、监控、备份、隐私说明、故障预案、回滚全部通过。

# Codex 每次执行任务的统一提示词

```text
你正在开发 Magic Classroom AI。

先阅读：
1. magic-classroom-ai/README.md
2. magic-classroom-ai/docs/01-PRD-产品需求文档.md
3. 与当前任务相关的技术/数据规范

只执行指定 Task，不擅自实现后续功能。

执行流程：
1. 检查现有代码和目录。
2. 给出简短实施计划。
3. 修改代码。
4. 运行 lint、typecheck、test、build 中适用的命令。
5. 修复由本任务引入的问题。
6. 总结修改文件、测试结果、已知限制。

开发原则：配置驱动、可复用、TypeScript 严格类型、可访问性、性能、错误降级、安全，不把课程内容硬编码在组件里。
```
