# 测试用例

## 用例编号约定

`TC-<模块>-<序号>`，例：`TC-AUTH-001`

## 模块：AUTH

### TC-AUTH-001 登录成功

**前置**：用户已注册，密码正确
**步骤**：
1. 调用 POST /auth/login，传入正确邮箱和密码
**预期**：
- HTTP 200
- 响应包含 token
- 数据库 user.last_login_at 已更新

**自动化**：是 / 文件位置 `4_Codes/api/test/auth.test.ts`

---

### TC-AUTH-002 密码错误

**前置**：用户已注册
**步骤**：
1. 用错误密码登录
**预期**：
- HTTP 401
- 错误码 `INVALID_CREDENTIALS`
- 不返回任何用户信息

**自动化**：是

---

## 模块：……
