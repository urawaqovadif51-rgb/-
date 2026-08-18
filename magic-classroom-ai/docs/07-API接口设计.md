# Magic Classroom AI — API 接口设计

## 1. 约定
- Base URL：`/api/v1`
- JSON UTF-8
- 时间使用 ISO 8601 UTC
- 鉴权由服务端统一处理
- 错误格式统一

```json
{
  "error": {
    "code": "COURSE_NOT_FOUND",
    "message": "Course not found",
    "requestId": "req_xxx",
    "details": null
  }
}
```

## 2. Auth
### POST /auth/login
输入：email、password。
输出：用户摘要与安全会话信息。

### POST /auth/logout
销毁当前会话。

### GET /auth/me
返回当前用户、角色和租户。

## 3. Courses
### GET /courses
参数：page、pageSize、status、query。

### POST /courses
创建课程草稿。

### GET /courses/{courseId}
获取课程基础信息和当前编辑版本。

### PATCH /courses/{courseId}
修改课程元数据。

### POST /courses/{courseId}/duplicate
复制课程。

### POST /courses/{courseId}/archive
归档。

## 4. Course Versions
### GET /courses/{courseId}/versions
列出版本。

### POST /courses/{courseId}/versions
基于当前草稿创建版本。

### GET /courses/{courseId}/versions/{versionId}
获取完整 CourseConfig。

### POST /courses/{courseId}/validate
执行 Schema、素材引用、场景完整性校验。

### POST /courses/{courseId}/publish
发布通过验证的新版本。

输出应包含稳定访问 URL 和 publishedVersion。

## 5. Scenes
第一版可整体保存 CourseConfig；当编辑器复杂后提供细粒度接口。

### POST /courses/{courseId}/scenes
新增场景。

### PATCH /courses/{courseId}/scenes/{sceneId}
修改场景。

### DELETE /courses/{courseId}/scenes/{sceneId}
删除场景。

### POST /courses/{courseId}/scenes/reorder
批量更新顺序。

## 6. Assets
### POST /assets/upload-url
请求 presigned upload URL。

输入：filename、mimeType、sizeBytes、purpose。

服务端必须校验允许类型和大小。

### POST /assets/complete
确认上传完成并创建 Asset 元数据。

### GET /assets
按课程/owner/type 查询。

### DELETE /assets/{assetId}
只有未被引用或符合删除策略时允许删除。

## 7. Templates
### GET /templates
教师端获取可用模板。

### GET /templates/{templateId}
返回模板 metadata 和配置 schema。

平台管理员可扩展：创建、版本、启停。

## 8. AI Generation
### POST /generation-jobs
创建生成任务。

输入示例：
```json
{
  "sourceAssetId": "asset_01",
  "grade": "middle-school",
  "language": "zh-CN",
  "mode": "full-course"
}
```

输出：jobId、status。

### GET /generation-jobs/{jobId}
查询状态、stage、progress、失败原因。

### POST /generation-jobs/{jobId}/cancel
取消仍可取消的任务。

### POST /generation-jobs/{jobId}/apply
教师确认后将生成结果应用为课程草稿。

## 9. Learning
### POST /learning-sessions
开始学习会话。

### PATCH /learning-sessions/{sessionId}
更新 currentScene/progress 等必要状态。

### POST /learning-sessions/{sessionId}/interactions
记录答题或关键交互。

### POST /learning-sessions/{sessionId}/complete
完成课程。

## 10. Analytics
教师：
- GET /courses/{courseId}/analytics/summary
- GET /courses/{courseId}/analytics/sessions

只返回当前权限范围内的数据。

## 11. Admin
学校管理员：
- GET /tenant/members
- POST /tenant/invitations
- PATCH /tenant/members/{id}/role

平台管理员接口单独受更高权限保护。

## 12. 幂等
对以下接口建议支持 Idempotency-Key：
- 创建生成任务
- 发布课程
- 创建支付/配额类操作（未来）

## 13. 分页
统一：page、pageSize；返回 total、items。大数据量后可改 cursor，但一个版本内保持一致。

## 14. 安全要求
- 每个 course/asset/job 查询都做 tenant/owner 权限校验。
- 不相信前端传入的 role/tenantId。
- 上传 URL 短时有效。
- AI Provider Key 永不返回客户端。
- 管理操作写审计日志。

## 15. API 版本
破坏性变更通过 `/api/v2` 或明确兼容策略处理，不悄悄改变已发布客户端依赖。
