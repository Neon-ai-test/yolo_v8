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
  - ✅ Require approvals: 设置为 1（至少需要一个人审查，会被自动审批满足）
  - ✅ Dismiss stale PR approvals when new commits are pushed（可选，根据需要）
  - ✅ Require status checks to pass before merging
  - ✅ Require branches to be up to date before merging
  - ✅ Do not allow bypassing the above settings
  - ✅ Require linear history
  - ✅ Allow auto-merge（启用自动合并）
- 点击 "Create"

### 3. 限制直接推送

- 在分支保护规则中，勾选：
  - ✅ Restrict who can push to matching branches
  - 添加你的账户和 GitHub Actions 机器人（github-actions[bot]）为允许推送的用户

### 4. 配置自动合并标签（可选）

如果使用标签控制自动合并，需要：
- 在仓库 Settings > Labels 中创建标签 `automerge`
- 当需要自动合并时，给 PR 打上 `automerge` 标签

## 第四步：配置 GitHub Actions

### 1. 启用 Actions

- 进入 "Settings" > "Actions" > "General"
- 在 "Actions permissions" 中选择：
  - ✅ Allow all actions and reusable workflows
- 点击 "Save"

### 2. 验证工作流

GitHub Actions 工作流文件已配置：
- `.github/workflows/version.yml` - 版本管理工作流
- `.github/workflows/auto-approve.yml` - 自动审批工作流 ⭐
- `.github/workflows/auto-merge.yml` - 自动合并工作流 ⭐

**工作流触发时机：**

1. **当创建或更新 Pull Request 时**：
   - `auto-approve.yml` 自动审批 PR
   - `auto-merge.yml` 检查是否满足自动合并条件
   - 如果满足条件，自动合并 PR 到 main 分支

2. **当推送到 `main` 分支时**：
   - `version.yml` 触发版本管理流程
   - 自动更新版本号
   - 自动生成 CHANGELOG
   - 自动创建 Git Tag
   - 自动创建 GitHub Release
   - 自动删除已合并的分支

### 3. 工作流说明

#### 自动审批（auto-approve.yml）

当 Pull Request 创建或更新时：
- ✅ GitHub Actions 自动审批 PR
- ✅ 添加审批评论："Auto-approved by GitHub Actions"
- ✅ 满足分支保护规则中的审批要求

#### 自动合并（auto-merge.yml）

当 Pull Request 被审批后：
- ✅ 检查是否满足合并条件
- ✅ 自动合并 PR 到 main 分支
- ✅ 使用 merge 方式（保留完整历史）

#### 版本管理（version.yml）

当代码合并到 `main` 分支时：
- ✅ 版本号更新
- ✅ CHANGELOG 生成
- ✅ Git 标签创建
- ✅ GitHub Release 创建
- ✅ **自动删除已合并的分支** ⭐

### 4. 配置自动合并方式

有两种方式启用自动合并：

#### 方式一：通过标签（推荐）

1. 在 PR 中添加 `automerge` 标签
2. 等待自动审批和自动合并

```bash
# 创建 PR 时通过标题或描述添加标签
# 或者在 PR 页面手动添加 automerge 标签
```

#### 方式二：通过配置默认启用

在仓库设置中：
1. 进入 Settings > General
2. 找到 "Pull Requests" 部分
3. 启用 "Allow auto-merge"

启用后，所有 PR 都可以自动合并（如果满足条件）。

### 5. 查看工作流

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

## 第七步：验证自动化流程

### 创建和验证 Pull Request

1. **创建 Pull Request**
   - 在 GitHub 上创建 PR 到 `main` 分支
   - 填写 PR 描述，说明变更内容

2. **验证自动审批** ⭐
   - 查看 PR 页面的 "Checks" 和 "Reviews" 标签
   - 应该看到 "Auto-approved by GitHub Actions" 的审批
   - 分支保护规则的审批要求应该已满足

3. **验证自动合并** ⭐
   - 如果配置了自动合并：
     - PR 应该会自动合并（几秒到几分钟内）
     - 查看 PR 状态，应该显示 "Merged"
   - 或者手动添加 `automerge` 标签触发自动合并

4. **验证版本管理**
   合并后，检查：

   **Actions 运行**
   - 进入 "Actions" 标签页
   - 查看工作流是否成功运行

   **版本号更新**
   - 查看 `package.json` 中的 version
   - 应该已经更新

   **Tag 创建**
   - 查看 "Code" 标签页 > "Tags"
   - 应该看到新的版本标签

   **Release 创建**
   - 查看 "Releases" 标签页
   - 应该看到新的 Release

   **文档更新**
   - 查看 `README.md` 和 `CHANGELOG.md`
   - 应该有新的版本记录

   **分支自动删除** ⭐
   - 查看 "Code" 标签页 > "Branches"
   - 已合并的功能分支应该已被自动删除
   - main/master 分支保持不变

### 完整自动化流程

```
1. 创建分支并推送代码
   ↓
2. 创建 Pull Request
   ↓
3. Auto-Approve 工作流自动审批 ✅
   ↓
4. Auto-Merge 工作流自动合并 ✅
   ↓
5. Version Management 工作流触发：
   - 更新版本号 ✅
   - 生成 CHANGELOG ✅
   - 创建 Git Tag ✅
   - 创建 GitHub Release ✅
   - 删除已合并分支 ✅
```

整个过程完全自动化，无需人工干预！

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

编辑 `.github/workflows/` 下的对应 `.yml` 文件，修改后提交即可。

### Q: 如何禁用自动审批功能

编辑 `.github/workflows/auto-approve.yml`，注释或删除工作流内容，或直接删除该文件。

### Q: 如何禁用自动合并功能

编辑 `.github/workflows/auto-merge.yml`，注释或删除工作流内容，或直接删除该文件。

### Q: 为什么 PR 没有自动审批？

可能的原因：
1. `auto-approve.yml` 工作流运行失败
2. GitHub Actions 权限不足（检查 `pull-requests: write` 权限）
3. 分支保护规则配置不正确

查看 Actions 日志获取详细信息。

### Q: 为什么 PR 没有自动合并？

可能的原因：
1. PR 没有被审批（自动审批失败）
2. 检查（checks）未通过
3. 分支未更新到最新状态
4. 没有添加 `automerge` 标签（如果使用标签控制）
5. `auto-merge.yml` 工作流运行失败

查看 PR 页面的状态和 Actions 日志。

### Q: 如何让特定 PR 不自动合并？

方法一：不要添加 `automerge` 标签
方法二：在 PR 标题或描述中添加 `[no-merge]` 标记
方法三：暂时禁用 `auto-merge.yml` 工作流

### Q: 如何禁用自动删除分支功能

编辑 `.github/workflows/version.yml`，注释或删除 "Delete merged branch" 步骤：

```yaml
# - name: Delete merged branch
#   run: |
#     # ... 删除逻辑
```

### Q: 为什么我的分支没有被删除？

可能的原因：
1. 分支名称是 `main` 或 `master`（受保护）
2. 分支不是通过 PR 合并的
3. Actions 工作流运行失败
4. 分支在删除前已被手动删除

查看 Actions 日志获取详细信息。

### Q: 如何保留已合并的分支

如果需要保留某个分支，可以：

1. **在本地保留分支**：
   ```bash
   # 合并前，在本地保留分支的副本
   git branch backup/my-feature my-feature
   ```

2. **从远程重新创建**：
   ```bash
   # 合并后，从远程获取分支
   git fetch origin origin/feature-branch:feature-branch
   ```

3. **禁用自动删除**：临时禁用自动删除功能

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
