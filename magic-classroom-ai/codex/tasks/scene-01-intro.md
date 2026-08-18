# Codex Task：Scene 01 开场动画（序号1）

## 任务目标
在现有 Magic Classroom AI 前端工程中实现 `Scene01Intro`，用于领导演示并作为后续课程模板的可复用 Intro Scene。

## 技术要求
- React + TypeScript + Vite
- Tailwind CSS
- Framer Motion
- React Router
- 所有课程内容从配置读取，不允许把英国课程文案、素材路径、地图数据硬编码进组件
- 不依赖 Three.js 完成第一版
- 不使用具体受版权保护影视作品角色或官方资产；使用原创/授权素材

## 建议目录
```text
src/
├── components/scene-intro/
│   ├── IntroScene.tsx
│   ├── IntroBackground.tsx
│   ├── IntroTitle.tsx
│   ├── GuideCharacter.tsx
│   ├── RegionMap.tsx
│   ├── SceneToolbar.tsx
│   ├── StartJourneyButton.tsx
│   └── MagicParticles.tsx
├── hooks/
│   ├── useFullscreen.ts
│   └── useSceneAudio.ts
├── types/
│   └── course.ts
└── scenes/
    └── IntroScenePage.tsx
```

## 输入数据
组件接收 `IntroSceneConfig`：
```ts
export interface IntroSceneConfig {
  id: string;
  title: string;
  subtitle?: string;
  description?: string;
  background?: string;
  teacher?: {
    name: string;
    asset?: string;
    greeting?: string;
  };
  map?: {
    asset?: string;
    regions?: Array<{ id: string; label: string }>;
  };
  effects?: {
    particles?: boolean;
    titleGlow?: boolean;
    mapPulse?: boolean;
    characterFloat?: boolean;
  };
  primaryAction: {
    label: string;
    action: "next-scene" | "route";
    target?: string;
  };
}
```

## UI 分区
1. 全屏容器：`min-h-screen`，主画布按 16:9 视觉设计。
2. 背景：课程背景图 + 暗色渐变遮罩 + 轻粒子。
3. 左侧信息区：标题、副标题、介绍文案。
4. 中部角色区：原创 AI 导师角色。
5. 右侧地图区：SVG/图片地图和区域标签。
6. 右侧工具栏：目录、声音、全屏。
7. 主 CTA：Start Journey。

## 动画要求
### P0 必做
- 页面首次载入淡入 500~800ms。
- 标题从轻微下移 + opacity 0 到正常状态。
- 导师角色 3~5 秒循环轻微上下漂浮，幅度不超过 8px。
- Start 按钮轻微 glow，hover 时 scale 不超过 1.04。
- 地图热点呼吸动画。
- 背景粒子缓慢漂移，不挡交互。

### P1 可选
- 云层平移。
- 地图区域 hover 高亮。
- 页面切换时 overlay fade。

### 禁止
- 大幅抖动、闪烁频率过快、影响阅读的持续 zoom。
- 在低端设备生成过多 DOM 粒子。

## 可访问性
- 所有图标按钮必须有 `aria-label`。
- Start Journey 可通过 Tab 聚焦并按 Enter/Space 激活。
- 检测 `prefers-reduced-motion: reduce`，关闭循环漂浮、脉冲、复杂粒子。
- 文本与背景确保基本可读对比度。

## 音频
- 默认不依赖 autoplay。
- 第一次用户交互后才允许播放 BGM。
- 声音按钮状态：muted / unmuted。
- 组件卸载时清理音频事件。

## 全屏
封装 `useFullscreen`：
- `document.documentElement.requestFullscreen()`
- `document.exitFullscreen()`
- 监听 `fullscreenchange`
- 不支持时按钮可隐藏或禁用

## 素材失败 fallback
- 背景加载失败：使用 CSS 渐变背景。
- 导师加载失败：使用占位卡片。
- 地图加载失败：使用文本区域列表。
- BGM 失败：静默降级，不阻塞页面。

## 性能预算（Demo）
- 首屏主背景建议 <= 1.5MB。
- 导师 WebP 建议 <= 600KB。
- SVG 地图 <= 300KB。
- 不一次预加载后续全部课程素材。
- 动画优先 transform/opacity。

## 响应式
### 1920×1080
完整三栏视觉。

### 1366×768
保证标题、导师、地图和 CTA 均可见，减少留白。

### 平板
地图可缩小；工具栏不覆盖主 CTA。

### 手机
第一版允许简化布局：标题 → 导师 → CTA；地图可折叠或移动到下方。

## 4 小时 Demo 执行顺序
### 00:00–00:20
初始化/检查工程、依赖和路由。

### 00:20–01:10
完成 IntroScene 静态布局和课程配置读取。

### 01:10–02:00
接入背景、角色、地图、CTA、工具栏。

### 02:00–02:45
添加 Framer Motion P0 动画和粒子。

### 02:45–03:15
接入声音、全屏、Start Journey 路由。

### 03:15–03:45
适配 1920×1080 / 1366×768，处理资源 fallback。

### 03:45–04:00
运行 build、测试、修复阻断问题。

## 验收标准
- `npm run build` 通过。
- 无 TypeScript error。
- Scene 01 可以从示例 `course.json` 渲染。
- 修改 JSON 标题/背景/按钮文字后无需改组件即可生效。
- Start Journey 能进入 Scene 02 或配置目标。
- 声音、全屏可用并可降级。
- 1920×1080 和 1366×768 可用。
- reduced-motion 下无持续复杂动画。
- 无受版权保护官方角色/官方影视素材写死在代码或资源中。

## Codex 完成后必须输出
1. 改动文件列表。
2. 启动/构建命令。
3. 尚未完成的 P1/P2 项。
4. 已知风险。
5. 截图验证建议尺寸。
