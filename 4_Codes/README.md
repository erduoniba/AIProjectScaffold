# 4_Codes — 代码实现

代码实际放置目录。可以是：

- 单仓：`4_Codes/` 下直接是项目根
- 多包：`4_Codes/web/`、`4_Codes/api/`、`4_Codes/mobile/`
- Monorepo：`4_Codes/` 下用 pnpm workspaces / turborepo

## AI 编码守则

1. **先骨架后血肉**：先建项目、装依赖、跑起 hello world，再写业务
2. **MVP 主路径优先**：先把核心 happy path 端到端跑通，再补边缘
3. **不过度抽象**：三处相似 ≠ 必须抽象；先重复，后重构
4. **不写 TODO 占位代码**：要么实现，要么不写。已知缺口写到 `5_Tests/` 或 issue
5. **不留 console.log**：调试痕迹必须清理
6. **不用 `--no-verify`**：钩子失败先修复，不要绕过
7. **小步提交**：一个 commit 一件事

## 推荐项目内文档

代码项目内放：

- `README.md` — 启动说明
- `CLAUDE.md` — 项目特定的 AI 协作守则（覆盖 AI_GUIDE.md 通用规则）
- `.env.example` — 环境变量样例

## 与上层文档的链接

代码 README 必须链回：
- [PRD](../2_Prds/prd.md)
- [架构](../3_Designs/architecture.md)
- [API 设计](../3_Designs/api_design.md)
