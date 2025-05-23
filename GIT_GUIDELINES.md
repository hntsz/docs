## Git 使用规范

### 一、适用范围

本规范适用于本团队在 GitHub 上托管的所有项目，旨在确保代码管理一致性、提高协作效率，并配合 CI/CD 流程顺利运行。

如果需要快速了解，请参考 [新成员 Git 工作流程示例](GIT_QUICKSTART.md)。

---

### 二、分支管理策略

#### 分支模型：**简化 Git Flow**

我们采用简化版的 Git Flow，结合自动化部署流程，定义以下主要分支：

* **`main`**：正式发布分支，仅包含已经上线的稳定代码。
* **`staging`**：用于集成测试的预发布分支，代码推送后触发自动部署到测试环境。
* **`develop`**：日常开发的集成分支。
* **`feature/*`**：新功能开发，基于 `develop` 创建，完成后合并回 `develop`。
* **`hotfix/*`**：紧急修复线上问题，直接基于 `main` 创建，修复后应同时合并回 `main` 和 `develop`。

> 特殊项目（如客户仓库）应参考客户提供的 Git 规范调整分支命名与流程。

---

### 三、提交规范（Commit Convention）

团队统一采用 **[Conventional Commits](https://www.conventionalcommits.org/)** 规范，提交信息格式如下：

```
<type>(<scope>): <subject> #任务编号

<body>（可选）

<footer>（可选）
```

示例：

```
feat(auth): 添加用户登录接口 #123

- 支持用户名密码登录
- 添加 jwt token 返回机制
```

#### 常用 type 类型：

* `feat`：新功能
* `fix`：Bug 修复
* `docs`：文档更新
* `style`：代码格式调整（不影响逻辑）
* `refactor`：重构（不影响外部行为）
* `test`：增加或修改测试
* `chore`：构建工具或辅助功能变更

#### 任务编号约定：

使用 `#123` 的形式引用任务系统编号（如 Jira、Tapd、禅道等），如未集成自动识别，则建议在 PR 描述中补充任务信息。

---

### 四、合并与 Pull Request 规范

* 所有代码**必须通过 Pull Request / Merge Request 合并**，不允许直接向 `main`、`develop` 等核心分支推送。

* 提交 PR 时建议遵循以下流程：

  1. 拉取最新 `develop`，基于其创建分支；
  2. 命名分支：`feature/login-ui`、`bugfix/login-crash` 等；
  3. 完成开发后提交 PR，并附带清晰描述和任务编号；
  4. 合并后删除已完成分支。

* **Code Review**：当前不强制评审，但推荐：

  * 关键模块由至少一人审核；
  * 后续可引入自动化流程如 CODEOWNERS 管理 Reviewer；
  * 保持良好的团队沟通，逐步推动规范化评审。

* **合并权限**：

  * 可自我合并，但需确保：

    * CI 检查通过；
    * 代码无冲突；
    * 评估风险较小或已被关注。

---

### 五、发布流程与 Tag 

#### 发布建议流程：

1. 功能开发完成 → 合并到 `develop`；
2. `develop` → 合并到 `staging`（自动部署测试环境）；
3. 测试通过 → 合并 `staging` 到 `main`；
4. 在 `main` 打 Tag（如 `v1.2.0`）→ 触发正式发布流程。

#### Git Tag 规范：

* 采用语义化版本号（SemVer）：`vX.Y.Z`

  * `X`：主版本号，破坏性更新；
  * `Y`：次版本号，新增功能；
  * `Z`：修复 bug。

示例：

```
v1.0.0
v1.1.3
```

---

### 六、禁止强推（Force Push）

* 所有分支 **禁止使用 `git push --force`**，除非在明确的单人维护分支中（如草稿 PR）。
* 若出现需重置历史的情况，请提前与团队沟通，并通过安全方式（如新建分支）处理。

---

### 七、冲突与合并策略

* 合并前**务必同步目标分支**（`git pull --rebase origin develop`），避免无意义合并节点。
* 如遇冲突，请务必**手动检查每一处改动**，确保不影响他人工作。
* 建议使用 `rebase` 保持提交历史清晰，避免出现无意义的 merge commit。

---

### 八、Git Hooks / 工具建议

* 使用 `.gitignore` 明确排除文件，避免不必要的文件提交。