# Codex Phase 02：产品化执行文档

> 前置条件：Phase 01 Demo 已可运行，并完成 Scene 01~至少 Scene 03 的基础演示。

## 目标
将一次性 Demo 重构为可长期维护、可批量生成课程的产品基础架构。

## 必须完成
1. 建立 CoursePlayer。
2. 所有课程内容从 JSON/接口读取。
3. 建立 SceneRenderer，根据 scene.type 选择模板。
4. 建立统一状态管理：当前场景、课程进度、分数、奖励、音频。
5. 抽离公共 UI 组件与动画。
6. 建立 AssetResolver，统一管理课程素材地址。
7. 建立错误边界和资源 fallback。
8. 增加课程 schema 校验。
9. 添加至少两个课程配置，验证模板复用能力。
10. 为后续后端 API 留出 repository/service 层。

## 推荐目录
```text
src/
  app/
  components/
  features/
    course-player/
    scenes/
    rewards/
    audio/
  templates/
    intro/
    map-explore/
    timeline/
    reading/
    matching/
    summary/
    result/
  schemas/
  services/
  stores/
  types/
  utils/
  styles/
public/
  assets/
```

## 关键接口
```ts
export interface CourseConfig {
  schemaVersion: string
  course: CourseMeta
  theme?: ThemeConfig
  audio?: AudioConfig
  teacher?: TeacherConfig
  scenes: SceneConfig[]
}

export interface SceneConfig {
  id: string
  order: number
  type: string
  title?: string
  [key: string]: unknown
}
```

## SceneRenderer 规则
- 不允许在主播放器里写大量 if/else 页面逻辑。
- 优先使用 registry/map：scene.type → template component。
- 未识别类型进入 UnsupportedScene fallback。
- 每个模板自行校验其专属配置。

## 数据策略
Demo：静态 JSON。
MVP：GET /api/v1/courses/:id/player-config。
正式版：课程版本发布后生成不可变快照，学生端读取发布版本。

## 第二阶段验收
- 同一个 intro 模板加载两份不同 JSON 后可展示不同标题、背景、导师和地图。
- 新增课程不修改模板源码。
- 课程配置错误不会导致全站白屏。
- 场景可前进/后退并记录进度。
- 刷新页面后可恢复当前课程进度（本地或服务端）。
- 图片、音频均通过统一资源解析层访问。

## 禁止事项
- 禁止把 Britain 文案写死在组件中。
- 禁止把 API Key 写入前端。
- 禁止为每门课复制一套 Scene 代码。
- 禁止以大量绝对定位替代正常响应式布局。
- 禁止为了 Demo 效果引入难以维护的大型 3D 依赖，除非确有必要。
