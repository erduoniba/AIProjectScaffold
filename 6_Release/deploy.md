# 部署文档

## 环境

| 环境     | URL | 部署方式      | 触发      |
| -------- | --- | ------------- | --------- |
| dev      |     | 自动          | push dev  |
| staging  |     | 自动          | push main |
| prod     |     | 手动批准      | tag v*    |

## 环境变量清单

| Key                | 必填 | 说明              | 示例                |
| ------------------ | ---- | ----------------- | ------------------- |
| DATABASE_URL       | Y    | 数据库连接串      | postgres://...      |
| REDIS_URL          | Y    | Redis 连接串      | redis://...         |
| JWT_SECRET         | Y    | JWT 签名密钥      | （openssl rand -hex 32） |
| SENTRY_DSN         | N    | 错误追踪          |                     |

## 部署步骤

1. 合并 PR 到 main
2. CI 跑通
3. 打 tag：`git tag v0.1.0 && git push --tags`
4. 等部署流水线通过
5. 健康检查：`curl https://.../healthz`

## 回滚

```bash
# 立即回滚到上一个 tag
kubectl rollout undo deployment/<name> -n <ns>
# 或重新部署旧 tag
```

数据库迁移回滚：

- 所有迁移必须可逆，提供 down 脚本
- 不可逆迁移（如 drop column）必须分两次发布：v1 停止使用列，v2 删除
