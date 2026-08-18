# Codex Taskbook：Scene 02～08

> 前置：先完成 `scene-01-intro.md`、CoursePlayer 基础、课程 JSON 类型和模板 registry。

## Scene 02：Map Explore
### 目标
从课程 JSON 读取地图、热点和内容卡片，支持 hover、focus、点击探索。

### 组件
- `MapExploreScene`
- `MapCanvas`
- `MapHotspot`
- `RegionInfoCard`
- `SceneProgress`

### 必做
- SVG/图片地图可切换。
- 热点坐标配置化。
- 点击热点显示内容。
- 已访问热点状态。
- 全部核心热点访问后允许继续。
- 键盘可操作。

### 验收
修改 JSON 热点数量/位置/文案，不改组件即可渲染。

---

## Scene 03：Timeline
### 目标
通用时间轴教学模板。

### 组件
- `TimelineScene`
- `TimelineTrack`
- `TimelineNode`
- `TimelineDetail`

### 必做
- 节点顺序配置化。
- 当前节点高亮。
- 节点支持标题、时期、摘要、图片可选。
- 桌面横向，小屏可切换纵向。
- next/previous 与 CoursePlayer 协同。

### 验收
无图、有图、长文本三种配置均不破版。

---

## Scene 04：Reading Challenge
### 目标
阅读材料 + 单选题互动。

### 组件
- `ReadingScene`
- `ReadingPanel`
- `SingleChoiceQuestion`
- `FeedbackPanel`

### 必做
- 文章内容配置化。
- 题目、选项、答案、分数从 JSON 读取。
- 防止重复加分。
- 完成题目后记录 interaction。
- Demo 模式可本地记录，正式模式调用 API。

### 验收
正确/错误/重复提交均符合预期。

---

## Scene 05：Matching Game
### 目标
左右匹配的轻量游戏场景。

### 必做
- 匹配数据配置化。
- 鼠标点击完成配对；不强制拖拽作为唯一交互方式。
- 键盘可操作。
- 错误配对有轻量反馈。
- 全部匹配后完成并奖励。

### 非目标
第一版不做复杂物理拖拽或 Canvas 游戏引擎。

---

## Scene 06：AI Guide
### Demo 模式
使用 scripted suggestions 和预设响应，保证领导演示稳定。

### 正式模式
预留 `AiGuideService` 接口：
```ts
interface AiGuideService {
  ask(input: { courseId: string; sceneId: string; question: string }): Promise<{ answer: string }>;
}
```

### 必做
- 建议问题按钮。
- 输入框。
- loading/error/empty 状态。
- Demo 和 API 模式可切换。
- 回答不可直接渲染未经处理的危险 HTML。
- 正式产品需要内容审核、速率限制和教师控制。

### 验收
无网络时 Demo scripted 模式仍可演示。

---

## Scene 07：Summary
### 目标
汇总本课知识点和完成情况。

### 必做
- points 配置化。
- 已完成场景展示。
- 可显示本课关键词和奖励。
- CTA 进入结果页。

---

## Scene 08：Result
### 目标
明确完成状态、得分、奖励和下一步。

### 必做
- 总分。
- 完成度。
- 奖励列表。
- Restart。
- Back to course list（有后台时）。
- Demo 模式可显示演示完成文案。

### 验收
重开课程时按产品规则正确清空/新建 session。

---

# 公共工程要求

## Scene Registry
建立：
```ts
const sceneRegistry = {
  intro: IntroScene,
  'map-explore': MapExploreScene,
  timeline: TimelineScene,
  reading: ReadingScene,
  matching: MatchingScene,
  'ai-guide': AiGuideScene,
  summary: SummaryScene,
  result: ResultScene,
} as const;
```

未知类型必须展示 `UnsupportedScene`，不得整个播放器崩溃。

## Runtime State
统一保存：
- currentSceneIndex
- visitedSceneIds
- score
- rewards
- answers
- audioMuted
- sessionId

不要让每个 Scene 自己建立互不兼容的课程总状态。

## Analytics Event
预留：
- scene_view
- scene_complete
- hotspot_open
- question_answer
- matching_complete
- ai_question
- course_complete

Demo 可以 console/local adapter，正式版替换 API adapter。

## 测试
每个模板至少：
- 1 个 happy path 单元/组件测试。
- 1 个空数据/fallback 测试。
- 关键交互测试。

## Codex执行顺序
1. Scene Registry
2. Runtime Store
3. Scene02
4. Scene03
5. Scene04
6. Scene05
7. Scene07
8. Scene08
9. Scene06 Demo
10. Scene06 API adapter
11. 完整 CoursePlayer E2E

每完成一个 Scene：执行 lint、typecheck、test、build，并提交独立 commit。
