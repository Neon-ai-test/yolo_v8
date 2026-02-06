# GitHub 仓库配置指南

本文档指导你如何将本地上传到 GitHub 并配置 GitHub Actions。

## 第一步：创建 GitHub 仓库

1. 访问 [GitHub](https://github.com/new) 创建新仓库
2. 仓库名称：`yolo_v8`（或自定义）
3. 设置为 Public 或 Private（根据需要）
4. **不要**初始化 README、.gitignore 或 license（项目已有这些文件）
5. 点击 "Create repository"

## 第二步：推送本地代码到 GitHub

### 方法一：使用 HTTPS

```bash
# 添加远程仓库（替换 YOUR_USERNAME 为你的 GitHub 用户名）
git remote add origin https://github.com/YOUR_USERNAME/yolo_v8.git

# 推送到 GitHub
git push -u origin main
```

### 方法二：使用 SSH

```bash
# 生成 SSH 密钥（如果还没有）
ssh-keygen -t ed25519 -C "your_email@example.com"

# 添加到 GitHub 账户
# 复制 ~/.ssh/id_ed25519.pub 的内容
# 访问 GitHub Settings > SSH and GPG keys > New SSH key

# 添加远程仓库
git remote add origin git@github.com:YOUR_USERNAME/yolo_v8.git

# 推送到 GitHub
git push -u origin main
```

## 第三步：配置 GitHub 仓库保护规则

### 1. 进入仓库设置

- 进入 GitHub 仓库页面
- 点击 "Settings" > "Branches"

### 2. 添加分支保护规则

- 点击 "Add branch protection rule"
- Branch name pattern: `main`
- 勾选以下选项：
  - ✅ Require a pull request before merging
  - ✅ Require approvals: 设置为 1（至少需要一个人审查）
  - ✅ Dismiss stale PR approvals when new commits are pushed
  - ✅ Require status checks to pass before merging
  - ✅ Require branches to be up to date before merging
  - ✅ Do not allow bypassing the above settings
  - ✅ Require linear history
- 点击 "Create"

### 3. 限制直接推送

- 在分支保护规则中，勾选：
  - ✅ Restrict who can push to matching branches
  - 添加你的账户为允许推送的用户

## 第四步：配置 GitHub Actions

### 1. 启用 Actions

- 进入 "Settings" > "Actions" > "General"
- 在 "Actions permissions" 中选择：
  - ✅ Allow all actions and reusable workflows
- 点击 "Save"

### 2. 验证工作流

GitHub Actions 工作流文件已配置：
- `.github/workflows/version.yml`

当推送到 `main` 分支时，会自动触发：
- 版本号更新
- CHANGELOG 生成
- Git 标签创建
- GitHub Release 创建

### 3. 查看工作流

- 进入仓库的 "Actions" 标签页
- 可以看到所有工作流运行记录

## 第五步：创建第一个功能分支

```bash
# 创建功能分支
git checkout -b feature/your-feature-name

# 进行开发...

# 提交代码
git add .
git commit -m "feat: 添加新功能"

# 推送分支
git push origin feature/your-feature-name
```

## 第六步：创建 Pull Request

1. 访问 GitHub 仓库
2. 点击 "Compare & pull request"
3. 填写 PR 信息：
   - 标题：简洁描述变更
   - 描述：详细说明变更内容和原因
   - 关联 Issue（如果有的话）
4. 点击 "Create pull request"
5. 等待代码审查和合并

## 第七步：验证自动化版本管理

合并 PR 后，检查：

1. **Actions 运行**
   - 进入 "Actions" 标签页
   - 查看工作流是否成功运行

2. **版本号更新**
   - 查看 `package.json` 中的 version
   - 应该已经更新

3. **Tag 创建**
   - 查看 "Code" 标签页 > "Tags"
   - 应该看到新的版本标签

4. **Release 创建**
   - 查看 "Releases" 标签页
   - 应该看到新的 Release

5. **文档更新**
   - 查看 `README.md` 和 `CHANGELOG.md`
   - 应该有新的版本记录

## 常见问题

### Q: 推送时提示认证失败

**解决方法**:
- 使用 Personal Access Token:
  1. GitHub Settings > Developer settings > Personal access tokens > Tokens (classic)
  2. 生成新 token，勾选 `repo` 权限
  3. 使用 token 作为密码推送

### Q: Actions 运行失败

**解决方法**:
- 检查 Actions 日志查看具体错误
- 确保 `package.json` 中的 version 字段格式正确
- 确保 Git 配置正确

### Q: 版本号没有自动更新

**解决方法**:
- 检查提交信息是否包含版本更新关键词
- 查看 Actions 日志中的版本类型判断
- 手动触发版本更新：
  ```bash
  git commit --allow-empty -m "chore(release): patch release"
  ```

### Q: 如何修改 Actions 工作流

编辑 `.github/workflows/version.yml` 文件，修改后提交即可。

## 下一步

1. 📖 阅读 [CONTRIBUTING.md](./CONTRIBUTING.md) 了解开发规范
2. 🚀 创建功能分支开始开发
3. 🔍 测试自动化版本管理流程
4. 📚 根据需要调整工作流配置

## 参考资源

- [GitHub 文档](https://docs.github.com/)
- [GitHub Actions 文档](https://docs.github.com/en/actions)
- [GitHub Flow](https://guides.github.com/introduction/flow/)
- [语义化版本](https://semver.org/)
