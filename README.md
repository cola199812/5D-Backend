# Hello World API (Spring Boot 3.2.0)

基于 Spring Boot 3.2.0 + Spring Security 的示例项目。

## 需求

1. 提供 GET 接口 `/api/hello`，需要经过 Spring Security 认证，返回 `Hello World`。
2. 提供用户名-密码登录接口 `/api/login`。

## 技术栈

- Java 17
- Spring Boot 3.2.0
- Spring Security

## 登录账号

| 用户名 | 密码   |
| ------ | ------ |
| test   | 123456 |

## 运行

```bash
mvn spring-boot:run
```

## 接口说明

| 方法 | 路径        | 说明                             | 认证                     |
| ---- | ----------- | -------------------------------- | ------------------------ |
| GET  | `/api/hello` | 返回 `Hello World`                | HTTP Basic（test/123456）|
| POST | `/api/login` | 登录，body: `{"username":"test","password":"123456"}` | 无需认证 |

### 调用示例

```bash
# 登录
curl -X POST http://localhost:8088/api/login \
  -H "Content-Type: application/json" \
  -d '{"username":"test","password":"123456"}'

# 访问受保护的 HelloWorld 接口（Basic 认证）
curl -u test:123456 http://localhost:8088/api/hello
```
