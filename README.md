# Hello World API（Spring Boot 3.2.0）

基于 Spring Boot 3.2.0 + Spring Security 的最小示例项目。

## 需求

1. 提供 `GET /api/hello` 接口，需要经过 Spring Security 认证，返回 `Hello World`。
2. 提供用户名-密码登录接口 `POST /api/login`。

## 技术栈

- Java 17（Spring Boot 3.2.0 要求 17+，**JDK 8 无法运行**）
- Spring Boot 3.2.0
- Spring Security

## 登录账号

| 用户名 | 密码   |
| ------ | ------ |
| test   | 123456 |

## 运行

端口：`8088`（见 `src/main/resources/application.properties`）

```bash
mvn clean spring-boot:run
```

或直接运行打包好的 jar：

```bash
java -jar target/hello-world-0.0.1-SNAPSHOT.jar
```

> Windows 下请确保 `JAVA_HOME` 指向 JDK 17（例如 `C:\Program Files\Java\jdk-17`）。

## 接口说明

| 方法 | 路径         | 说明                                                       | 认证                     |
| ---- | ------------ | ---------------------------------------------------------- | ------------------------ |
| GET  | `/api/hello` | 返回 `Hello World`                                          | HTTP Basic（test/123456）|
| POST | `/api/login` | 登录，body `{"username":"test","password":"123456"}`，成功返回 `Login success` | 无需认证 |

## 测试

### ① 登录（正确密码）→ 200，返回 `Login success`

```bash
curl -i -X POST http://localhost:8088/api/login \
  -H "Content-Type: application/json" \
  -d "{\"username\":\"test\",\"password\":\"123456\"}"
```

### ② 登录（错误密码）→ 401

```bash
curl -i -X POST http://localhost:8088/api/login \
  -H "Content-Type: application/json" \
  -d "{\"username\":\"test\",\"password\":\"wrong\"}"
```

> `AuthenticationManager.authenticate()` 校验失败会抛出 `BadCredentialsException`（属于 `AuthenticationException`），
> 被 Spring Security 的 `ExceptionTranslationFilter` 捕获后交由 `BasicAuthenticationEntryPoint` 处理，因此返回 401。

### ③ 访问 HelloWorld（带 Basic 认证）→ 200，返回 `Hello World`

```bash
curl -i -u test:123456 http://localhost:8088/api/hello
```

### ④ 访问 HelloWorld（无认证）→ 401

```bash
curl -i http://localhost:8088/api/hello
```

### 结果对照

| # | 请求                              | 预期状态码 | 预期响应体      |
|---|-----------------------------------|-----------|-----------------|
| ① | 登录 test/123456                  | 200       | `Login success` |
| ② | 登录错误密码                      | 401       | —               |
| ③ | `GET /api/hello` + Basic 认证     | 200       | `Hello World`   |
| ④ | `GET /api/hello` 无认证           | 401       | —               |
