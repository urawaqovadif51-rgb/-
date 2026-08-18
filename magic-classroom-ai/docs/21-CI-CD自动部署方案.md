# 21. CI/CD 自动部署方案

## 1. 目标
每次 Pull Request 自动检查质量，合并主分支后自动构建并部署到测试/生产环境，降低人工发布错误。

## 2. PR 阶段
建议 GitHub Actions 执行：

前端：
```bash
npm ci
npm run lint
npm run typecheck
npm run test
npm run build
```

后端：
```bash
pip install -r requirements.txt
pytest
ruff check .
mypy app
```

可再增加：
- JSON Schema 校验示例课程。
- OpenAPI 文件校验。
- Secret 扫描。
- 依赖漏洞扫描。

## 3. main 分支
合并到 main 后：
1. 重新运行测试。
2. 构建前后端产物/镜像。
3. 为镜像打 commit SHA tag。
4. 推送镜像仓库。
5. 自动部署 Staging。
6. Smoke Test。
7. 生产环境可配置人工审批后部署。

## 4. 环境
建议：
- Development：开发者本地。
- Staging：自动部署，用于验收。
- Production：正式环境。

各环境使用独立数据库、对象存储 bucket 和密钥。

## 5. Branch Protection
main 建议开启：
- 禁止直接 push。
- 必须 PR。
- 必须 CI 通过。
- 至少 1 次 review（团队扩大后启用）。
- 禁止 force push。

## 6. 版本
早期使用语义版本：`v0.x.y`。
正式版本：`v1.0.0`。
每个生产发布记录 changelog、commit SHA、数据库 migration revision。

## 7. 回滚
- 保留最近若干镜像版本。
- Staging smoke test 不通过则停止生产部署。
- 生产异常时切回上一个稳定镜像。
- 数据库迁移必须在上线前验证兼容性。

## 8. Demo 阶段简化
4 小时 Demo 可只配置前端 build 检查 + 静态托管自动发布，不要求完整 Docker CI。
