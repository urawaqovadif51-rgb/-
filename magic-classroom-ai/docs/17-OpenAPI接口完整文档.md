# 17. OpenAPI 接口完整文档

> 目标：给 Codex、前端和后端提供一致的 API 契约。正式实现以 `openapi/openapi.yaml` 为机器可读真源。

## 1. 基本约定
- Base URL：`/api/v1`
- Content-Type：`application/json`
- 鉴权：Bearer Token
- 时间：ISO 8601 UTC
- ID：UUID 字符串
- 分页：`page` + `page_size`
- 错误：统一错误结构

统一错误：
```json
{
  "error": {
    "code": "COURSE_NOT_FOUND",
    "message": "Course not found",
    "request_id": "..."
  }
}
```

## 2. Auth
### POST /auth/login
输入：邮箱/用户名与密码。
输出：access_token、refresh_token、user。

### POST /auth/refresh
刷新 access token。

### GET /me
返回当前用户及角色、租户信息。

## 3. Courses
### GET /courses
按当前租户返回课程列表。支持 `status`、`q`、分页。

### POST /courses
创建课程草稿。

### GET /courses/{course_id}
课程详情。

### PATCH /courses/{course_id}
修改基础信息。

### DELETE /courses/{course_id}
软删除课程；不建议物理删除已发布课程。

### POST /courses/{course_id}/duplicate
复制课程形成新草稿。

## 4. Course Versions
### POST /courses/{course_id}/versions
从当前草稿生成版本。

### GET /courses/{course_id}/versions
版本列表。

### GET /courses/{course_id}/versions/{version_id}
版本详情。

### POST /courses/{course_id}/publish
发布指定版本。

## 5. Scenes
### GET /courses/{course_id}/scenes
返回场景列表。

### POST /courses/{course_id}/scenes
新增场景。

### PATCH /scenes/{scene_id}
更新场景配置。

### DELETE /scenes/{scene_id}
删除草稿场景。

### POST /courses/{course_id}/scenes/reorder
批量调整顺序。

## 6. Assets
### POST /assets/upload-url
返回对象存储预签名上传 URL。

### POST /assets/complete
上传完成后登记 Asset。

### GET /assets
素材列表，支持课程、类型和关键词过滤。

### DELETE /assets/{asset_id}
未被引用时允许删除；被引用时返回冲突。

## 7. Templates
### GET /templates
返回当前可用模板。

### GET /templates/{template_id}
模板定义、版本和支持的 schema。

平台管理员后续扩展模板 CRUD。

## 8. AI Generation
### POST /generation/jobs
创建 AI 课程生成任务。

请求示例：
```json
{
  "course_id": "uuid",
  "source_asset_ids": ["uuid"],
  "mode": "course-draft",
  "preferences": {
    "scene_count": 8,
    "style": "fantasy-academy"
  }
}
```

### GET /generation/jobs/{job_id}
查询状态：queued / parsing / planning / generating / validating / completed / failed / cancelled。

### POST /generation/jobs/{job_id}/retry
失败重试。

### POST /generation/jobs/{job_id}/cancel
取消未结束任务。

## 9. Public Player
### GET /public/courses/{slug}
返回已发布的精简播放器配置。不得泄露教师备注、私有素材信息、内部 AI 信息。

### POST /public/courses/{slug}/sessions
创建学习会话。

### PATCH /public/sessions/{session_id}
更新进度、分数和状态。

### POST /public/sessions/{session_id}/interactions
批量记录交互事件。

## 10. Analytics
### GET /courses/{course_id}/analytics/summary
教师查看课程学习概览。

最低指标：学习人数、完成率、平均进度、平均分、场景完成率。

## 11. 权限矩阵
- Student：只读已发布课程、写自己的学习会话。
- Teacher：管理本租户自己的课程和素材。
- School Admin：管理本校用户、课程与统计。
- Platform Admin：管理模板和平台级配置。

任何权限都必须由服务端验证，不依赖前端隐藏按钮。

## 12. 幂等与并发
- 发布、AI Job 创建等高价值操作支持 `Idempotency-Key`。
- PATCH 更新可增加 `updated_at`/版本字段做乐观并发控制。

## 13. API 验收
- OpenAPI schema 可被工具解析。
- 400/401/403/404/409/422/429/500 有统一结构。
- 列表接口支持分页。
- 公开接口不返回敏感字段。
- 所有写操作有权限校验。
- 后端实现和前端类型应尽量由 OpenAPI 自动生成或校验，避免手工漂移。
