# `/api/cli` 接口契约

CLI 所有远程请求必须统一访问 `/api/cli/*`。后端可以复用现有服务，但 CLI 不直接访问 `/canvas`、`/run`、`/toolbar-image`。

## 认证

所有需要用户上下文的接口使用：

```http
Authorization: Bearer <ACCESS_KEY>
```

Access Key 由用户在 Web 头像菜单中的 `Access Key` 弹窗创建并复制。

## 接口列表

| 接口 | 说明 |
|------|------|
| `GET /api/cli/models/image?project_id=<id>` | 获取 CLI 可用生图模型 |
| `POST /api/cli/images/generate` | 提交生图任务 |
| `GET /api/cli/tasks/{task_id}` | 查询标准化任务状态 |
| `POST /api/cli/tasks/{task_id}/settle` | 结算任务 |
| `POST /api/cli/markdown/images/plan` | 生成 Markdown 图片提示词计划 |

## 错误格式

后端错误应返回 `success: false` 和 `message`。CLI 会优先展示 `message`，其次展示 `detail`。

## 稳定性规则

- 不暴露 Web 前端内部字段作为 CLI 必填字段。
- 任务结果统一输出 `image_urls`，不要让 CLI 解析多种内部结构。
- 模型字段使用 snake_case，便于公开文档保持稳定。
- 新增 CLI 能力时优先扩展 `/api/cli`，不要让 CLI 绕过契约层。
