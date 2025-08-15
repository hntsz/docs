## 新成员 Git 工作流程示例（From Clone to PR）

### 场景：

你是新入职的前端开发，需要为项目增加一个“登录按钮”的 UI 交互功能，任务编号为 #238（如果有）。

---

### 步骤一：克隆项目仓库

```bash
git clone https://github.com/your-org/your-project.git
```

---

### 步骤二：拉取最新的 `develop`, `staging`, `main` 分支并创建功能分支

- 如果你的功能属于已经计划中开发工作的内容范围，选择 `develop` or `staging`。
- 如果你的功能属于没有计划的临时功能添加，选择 `main`。

```bash
git checkout develop
git pull origin develop

# 创建 feature 分支，命名为 feature/login-button
git checkout -b feature/login-button
```

---

### 步骤三：进行开发，并进行适当提交

开发完成后，按照提交规范进行提交（Conventional Commits + 任务编号）：

```bash
git add src/components/LoginButton.tsx
git commit -m "feat(login): add login button UI #238"
```

---

### 步骤四：同步远程并推送功能分支

```bash
git push origin feature/login-button
```

---

### 步骤五：在 GitHub 上创建 Pull Request

1. 打开项目主页；
2. 点击 “Compare & pull request”；
3. 选择将 `feature/login-button` 合并到 `develop` 或 `staging`；
4. 填写 PR 标题（可沿用 commit message），并在描述中附上：

   * 实现功能简述
   * 对应任务号（如：`Closes #238`）
   * 是否影响现有功能，是否需要测试验证

---

### 步骤六：CI 通过后，自行合并（如评估风险较低）

如无冲突且测试通过，可点击 “Merge” 按钮合并 PR 至 `develop`。随后可删除分支。

---

### 步骤七：日常维护（建议操作）

保持你的本地 develop 分支同步：

```bash
git checkout develop
git pull origin develop
```

如长时间开发未合并，可在分支中 `rebase` 最新 develop 保持更新：

```bash
git checkout feature/login-button
git pull --rebase origin develop
```

