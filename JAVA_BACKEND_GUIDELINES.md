## Java 后端开发规范（Spring Boot 项目适用）

### 一、代码风格规范

#### 1. 基础规范

* **统一使用 UTF-8 编码，缩进为 4 个空格。**
* 每个类都应有清晰的职责（单一职责原则）。

#### 2. 命名规范（参考阿里巴巴规范）

| 类型  | 命名规则示例                       | 说明                              |
| --- | ---------------------------- | ------------------------------- |
| 类名  | `UserServiceImpl`            | 使用大驼峰，常用后缀如 DTO、VO、BO、DO、Mapper |
| 方法名 | `getUserById()`              | 小驼峰，动宾结构                        |
| 常量  | `MAX_RETRY_TIMES`            | 全大写+下划线分隔                       |
| 变量名 | `userId`、`userList`          | 小驼峰命名                           |
| 包名  | `com.example.project.module` | 全小写，不使用下划线                      |

#### 3. 推荐使用格式化工具

建议集成以下任意一种代码格式校验：

* [Alibaba Java Code Guidelines plugin](https://plugins.jetbrains.com/plugin/10046-alibaba-java-coding-guidelines)

---

### 二、包结构建议

参考 Spring Boot 常见结构，或者见具体项目的规范

```
com.example.project
├── controller       // 接口层
├── service          // 业务逻辑层
│   └── impl
├── mapper           // MyBatis 映射接口
├── domain           // 实体类（DO/PO）
├── dto              // 数据传输对象（DTO）
├── vo               // 前端返回对象（VO）
├── config           // 配置类
├── exception        // 自定义异常类
├── common           // 通用类（常量、工具类、枚举）
└── Application.java // 启动类
```

---

### 三、日志规范

统一使用 **SLF4J + Logback**（通过 Lombok 的 `@Slf4j` 注解简化调用）

* 所有 `catch` 块必须打印日志：

```java
catch (Exception e) {
    log.error("操作失败，userId: {}", userId, e);
}
```

* 不允许打印敏感数据（如密码、token）；
* 禁止使用 `System.out.println`。

---

### 四、异常处理建议

建议使用**统一异常类 + 全局异常处理器**模式：

```java
// 自定义业务异常
public class BizException extends RuntimeException {
    private final int code;
    private final String message;
}

// 全局处理器
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(BizException.class)
    public ResponseEntity<ApiResponse<?>> handleBiz(BizException e) {
        return ApiResponse.fail(e.getCode(), e.getMessage());
    }
}
```

请参考具体项目的具体做法。

---

### 五、统一返回值结构

定义统一响应对象（推荐）：

```java
@Data
public class ApiResponse<T> {
    private int code;
    private String message;
    private T data;

    public static <T> ApiResponse<T> success(T data) { ... }
    public static ApiResponse<?> fail(int code, String message) { ... }
}
```

控制器返回：

```java
@GetMapping("/user/{id}")
public ApiResponse<UserVO> getUser(@PathVariable Long id) {
    UserVO vo = userService.getUserById(id);
    return ApiResponse.success(vo);
}
```

请参考具体项目的具体做法。

---

### 六、Lombok 使用建议

推荐使用的注解：

* `@Data`：实体类简化 getter/setter
* `@Slf4j`：日志输出
* `@Builder`：链式构造

---

### 七、其它建议

* MyBatis Mapper XML 使用 `resultMap` 显式映射字段；
* 不直接拼接 SQL，使用 `#{}`
* Maven 统一使用国内镜像仓库；
* 合理使用注解如 `@Validated` 进行参数校验；
* 对复杂方法建议添加 JavaDoc 注释说明用途与参数含义。