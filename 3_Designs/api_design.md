# API 设计

## 风格

- [ ] RESTful
- [ ] GraphQL
- [ ] RPC

## 通用约定

- 基础路径：`/api/v1`
- 鉴权：`Authorization: Bearer <token>`
- 内容类型：`application/json`
- 时间格式：ISO 8601 UTC
- 分页：`?page=1&page_size=20`，响应中带 `total`

## 错误响应

```json
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "User not found",
    "details": {}
  }
}
```

错误码表：

| code                 | HTTP | 说明           |
| -------------------- | ---- | -------------- |
| INVALID_PARAM        | 400  | 参数校验失败   |
| UNAUTHORIZED         | 401  | 未登录         |
| FORBIDDEN            | 403  | 无权限         |
| RESOURCE_NOT_FOUND   | 404  | 资源不存在     |
| INTERNAL_ERROR       | 500  | 服务内部错误   |

## 端点

### POST /api/v1/auth/login

**入参**
```json
{ "email": "a@b.com", "password": "..." }
```

**出参 200**
```json
{ "token": "...", "user": { "id": "...", "email": "..." } }
```

**错误**
- 401 INVALID_CREDENTIALS

---

### GET /api/v1/users/:id

…
