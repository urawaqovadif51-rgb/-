# Codex 开发入口（START HERE）

Codex 开发本项目时，先阅读：
1. `README.md`
2. `docs/01-PRD-产品需求文档.md`
3. `docs/03-技术架构设计.md`
4. `docs/08-Codex开发任务清单.md`
5. `codex/phase-01-demo.md`
6. `examples/magic-journey-britain/course.json`

## 当前第一目标
完成可给学校领导演示的 `Magic Journey · Explore Britain` 互动课程 Demo，优先完成 Scene 01。

## 开发原则
- 先运行、再优化；但不能把课程内容写死在组件中。
- 每完成一个任务先运行 lint/typecheck/build，再进入下一个任务。
- 不擅自替换整体技术栈。
- 不在前端保存任何密钥。
- 发现需求冲突时，以 PRD + 技术架构 + 当前阶段文档为优先。
- 图片、音频缺失时使用明确 placeholder/fallback，不能因为素材缺失导致开发停止。
- 不直接复制具体影视作品的角色、美术或商标资产；使用原创奇幻学院视觉。

## Scene 01 最小可交付
- 16:9 开场页。
- 原创奇幻学院夜景背景。
- 标题 Magic Journey / Explore Britain。
- 原创 AI 导师展示。
- 英国地图。
- 背景粒子。
- Start Journey。
- 声音开关。
- 全屏。
- 点击 Start 进入 Scene 02 占位页。

## 每个任务完成后的输出格式
```text
Task: TASK-XXX
Status: DONE / BLOCKED
Files changed:
- ...
Validation:
- npm run lint
- npm run typecheck
- npm run build
Notes:
- ...
```

## 遇到素材缺失
创建 `/public/assets/placeholders/`，使用渐变、简单 SVG 或几何占位素材。代码和交互先完成，后续再替换最终视觉资源。

## 第一轮结束条件
当 Scene 01 可稳定打开、动画运行、Start 可跳转、音频与全屏基础功能完成，并且 build 通过，即可提交第一轮演示版本。
