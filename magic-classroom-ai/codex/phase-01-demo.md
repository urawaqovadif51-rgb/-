# Codex Phase 01 — 领导演示 Demo 执行指南

## 目标
先完成可演示版本，不等待完整后台。

第一优先级：Scene01 开场动画。
第二优先级：CoursePlayer 和第二、三教学场景。
第三优先级：补齐 8 场景完整链路。

## 4 小时 Scene01 冲刺
### 0:00~0:30
- 初始化 React/Vite/TypeScript。
- 安装必要依赖。
- 建目录。
- 导入或创建临时占位素材。

### 0:30~1:30
- IntroScene 布局。
- 背景。
- 标题。
- 导师。
- 英国地图。
- Start Journey。

### 1:30~2:30
- Framer Motion 入场。
- 呼吸/漂浮。
- 地图热点呼吸。
- Start Button glow。
- 粒子层。

### 2:30~3:15
- Toolbar。
- 音频。
- Fullscreen。
- Scene Transition。

### 3:15~4:00
- 1366×768 / 1920×1080 调整。
- reduced motion。
- 素材 fallback。
- build。
- 演示部署。

## 4 小时内不要做
- Three.js 大型 3D。
- 复杂角色骨骼动画。
- 后端。
- 数据库。
- 真实 AI 对话。
- 大量视频。

## Codex 启动指令
```text
你正在执行 Magic Classroom AI Phase 01。

请先阅读：
- magic-classroom-ai/README.md
- magic-classroom-ai/docs/01-PRD-产品需求文档.md
- magic-classroom-ai/docs/03-技术架构设计.md
- magic-classroom-ai/docs/04-UI设计规范.md
- magic-classroom-ai/docs/06-数据结构规范.md
- magic-classroom-ai/docs/08-Codex开发任务清单.md

从 Task 001 开始执行。
一次只完成一个 Task。
完成任务后必须运行适用的 lint/typecheck/build，并汇报结果。
除非任务要求，不要提前开发后台和 AI 生成系统。
```

## Scene01 素材最小集合
- 1 张 16:9 原创英伦奇幻夜景。
- 1 张透明背景原创 AI 导师。
- 1 个英国 SVG 地图。
- 3~4 个工具栏 SVG 图标。
- 1 首有明确授权的 BGM。

没有最终素材时先使用占位资源完成结构，后续替换 URL 即可。

## 演示检查
- 浏览器提前打开并缓存。
- 准备全屏。
- BGM 默认静音，演示时点击后开启。
- 准备第二个本地链接/静态文件作为网络故障备用。
- 不在领导面前现场跑开发服务器更新依赖。
