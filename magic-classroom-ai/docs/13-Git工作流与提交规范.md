# Git 工作流与提交规范

## 1. 目标
确保 Codex、人工开发者和后续团队在同一仓库中协作时，代码可追踪、可回滚、可评审。

## 2. 分支策略
- `main`：稳定分支，只接受已验证改动。
- `develop`：产品集成分支（团队规模较小时可暂时省略）。
- `feature/<name>`：功能开发。
- `fix/<name>`：缺陷修复。
- `docs/<name>`：文档改动。

Demo 初期可采用 `main + feature/*` 的简化方案。

## 3. Codex 工作要求
每个 Codex 任务原则上单独分支、单独提交。

示例：
- `feature/scene-01-intro`
- `feature/course-player`
- `feature/map-scene`

不要让 Codex 在同一次任务中无关地修改大量模块。

## 4. Commit Message
建议采用 Conventional Commits：

```text
feat: add reusable intro scene
fix: handle audio autoplay fallback
docs: add deployment checklist
refactor: extract course config loader
test: add course schema validation tests
chore: update dependencies
```

允许增加 scope：

```text
feat(scene-intro): add map pulse animation
fix(audio): stop background music on unmount
```

## 5. 提交粒度
一个 commit 只解决一个清晰问题。

禁止：
```text
update files
fix stuff
changes
```

## 6. Pull Request 模板要求
PR 描述至少包含：
- 目的。
- 主要改动。
- 如何验证。
- 截图/录屏（UI 改动时）。
- 已知风险。
- 未完成项。

## 7. 合并前检查
- `npm run build` 成功。
- TypeScript 无 error。
- lint/test 通过（如果项目已配置）。
- 无密钥、密码、token。
- 新增课程配置通过 JSON Schema 校验。
- UI 在 1920×1080 和 1366×768 检查。
- 没有不必要的大文件提交进 Git。

## 8. 大文件
图片、音频、视频原则上进入对象存储或专门资源目录。正式项目不要长期把大量二进制素材堆进 Git 历史。

Demo 阶段少量压缩素材可以临时提交，但应控制体积。

## 9. 版本标签
里程碑建议：
- `v0.1.0-demo`
- `v0.2.0-course-player`
- `v0.5.0-mvp`
- `v1.0.0`

## 10. 回滚原则
发现演示阻断问题时优先回滚到上一稳定 commit，而不是在 main 上持续叠加未经验证的补丁。
