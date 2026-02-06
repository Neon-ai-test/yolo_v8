# 自动审批和合并功能说明

## 功能概述

本项目配置了完全自动化的 PR 审批和合并流程，无需人工干预即可完成代码合并和版本发布。

## 工作流文件

### 1. Auto Approve (`.github/workflows/auto-approve.yml`)

自动审批 Pull Request，满足分支保护规则的审批要求。

**触发条件：**
- Pull Request 被创建
- Pull Request 被重新打开
- Pull Request 有新的提交

**功能：**
- 自动添加 "Approved" 审批
- 审批评论："Auto-approved by GitHub Actions"

### 2. Auto Merge (`.github/workflows/auto-merge.yml`)

自动合并满足条件的 Pull Request。

**触发条件：**
- Pull Request 被创建、更新或重新打开
- Pull Request 收到审批

**功能：**
- 检查合并条件
- 自动合并 PR 到 main 分支
- 保留完整提交历史（merge commit）

### 3. Version Management (`.github/workflows/version.yml`)

合并后自动管理版本和发布。

**触发条件：**
- 代码合并到 main 分支

**功能：**
- 更新版本号
- 生成 CHANGELOG
- 创建 Git Tag
- 创建 GitHub Release
- 删除已合并的分支

## 完整自动化流程

```
┌─────────────────────────────────────────────────────────┐
│ 1. 开发者创建功能分支并推送代码                        │
│    git checkout -b feature/new-feature               │
│    git push origin feature/new-feature                │
└─────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────┐
│ 2. 开发者在 GitHub 上创建 Pull Request                │
│    从 feature/new-feature → main                      │
└─────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────┐
│ 3. Auto Approve 工作流触发                          │
│    - GitHub Actions 自动审批 PR                      │
│    - 添加审批评论："Auto-approved by GitHub Actions"   │
└─────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────┐
│ 4. Auto Merge 工作流触发                             │
│    - 检查合并条件（审批 + 检查通过）                 │
│    - 自动合并 PR 到 main 分支                         │
│    - PR 状态更新为 "Merged"                          │
└─────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────┐
│ 5. Version Management 工作流触发                      │
│    - 分析提交信息确定版本类型                          │
│    - 更新 package.json 版本号                         │
│    - 生成 CHANGELOG.md                                │
│    - 创建 Git Tag (vX.X.X)                           │
│    - 创建 GitHub Release                              │
│    - 删除 feature/new-feature 分支                    │
└─────────────────────────────────────────────────────────┘
```

## 配置步骤

### 1. 分支保护规则设置

进入 GitHub 仓库：Settings > Branches

**添加 main 分支保护规则：**

```yaml
Branch name pattern: main
Require a pull request before merging:
  - Require approvals: 1
  - Dismiss stale PR approvals when new commits are pushed (可选)
Require status checks to pass before merging:
  - Require branches to be up to date before merging
  - Require status checks to pass before merging
Do not allow bypassing the above settings
Require linear history
Allow auto-merge (重要！)
Restrict who can push to matching branches:
  - 添加你的账户
  - 添加 github-actions[bot]
```

### 2. 启用 Actions 权限

进入 Settings > Actions > General

**Actions permissions:**
- 选择 "Allow all actions and reusable workflows"

**Workflow permissions:**
- 选择 "Read and write permissions"
- 勾选 "Allow GitHub Actions to create and approve pull requests"

### 3. 配置自动合并方式

#### 方式一：通过标签控制（推荐）

1. 创建 `automerge` 标签
2. 给需要自动合并的 PR 添加此标签
3. 其他 PR 需要手动合并

#### 方式二：全局启用

1. 进入 Settings > General
2. 找到 "Pull Requests" 部分
3. 启用 "Allow auto-merge"

启用后，所有满足条件的 PR 都会自动合并。

## 使用示例

### 标准流程

```bash
# 1. 创建功能分支
git checkout -b feature/new-feature

# 2. 开发和提交
git add .
git commit -m "feat: 添加新功能"

# 3. 推送分支
git push origin feature/new-feature

# 4. 在 GitHub 上创建 PR
# 5. PR 自动审批
# 6. PR 自动合并
# 7. 自动发布版本
# 8. 自动删除分支
# 完成！无需任何人工操作
```

### 阻止自动合并

如果某个 PR 不想自动合并：

```bash
# 在 PR 标题中添加 [no-merge] 标记
git commit -m "feat: [no-merge] 暂时不合并的功能"
```

或者在 GitHub 上：
- 不要添加 `automerge` 标签
- 手动关闭自动合并功能

## 权限要求

### GitHub Secrets

确保仓库有以下权限：

```yaml
GITHUB_TOKEN:
  - contents: write    # 创建 Tag、Release
  - pull-requests: write  # 审批和合并 PR
```

### 分支保护

`github-actions[bot]` 需要能够：
- 审批 Pull Request
- 合并 Pull Request
- 推送到 main 分支

## 常见问题

### Q: 自动审批失败

**检查清单：**
1. 工作流文件是否正确
2. Actions 权限是否配置
3. 分支保护规则是否允许机器人审批

**解决方法：**
- 检查 Actions 日志
- 确认 `pull-requests: write` 权限
- 在分支保护规则中允许机器人审批

### Q: 自动合并失败

**检查清单：**
1. PR 是否已审批
2. 检查是否通过
3. 分支是否更新到最新
4. 是否添加了 `automerge` 标签

**解决方法：**
- 查看 PR 状态页面
- 确保所有检查都通过
- 更新分支到最新状态
- 添加 `automerge` 标签（如果使用标签控制）

### Q: 版本管理失败

**检查清单：**
1. 提交信息格式是否正确
2. package.json 是否有效
3. 是否有 Git 写权限

**解决方法：**
- 遵循提交规范（feat/fix/等）
- 确保 `contents: write` 权限
- 查看 Version Management 日志

### Q: 分支未被删除

**检查清单：**
1. 是否通过 PR 合并
2. 分支名称是否为受保护分支

**解决方法：**
- 确保使用 PR 合并
- 不要合并 main/master 分支
- 查看删除分支步骤的日志

## 禁用功能

### 禁用自动审批

```bash
# 删除或重命名工作流文件
rm .github/workflows/auto-approve.yml
git add .
git commit -m "chore: 禁用自动审批"
git push
```

### 禁用自动合并

```bash
# 删除或重命名工作流文件
rm .github/workflows/auto-merge.yml
git add .
git commit -m "chore: 禁用自动合并"
git push
```

### 禁用自动删除分支

编辑 `.github/workflows/version.yml`：

```yaml
# 注释掉删除分支的步骤
# - name: Delete merged branch
#   run: |
#     ...
```

## 最佳实践

1. **使用清晰的提交信息**
   - 遵循提交规范
   - 详细描述变更内容

2. **保持 PR 小而专注**
   - 每个 PR 只做一件事
   - 便于代码审查（即使自动化）

3. **确保检查通过**
   - 本地测试后再推送
   - 监控 CI/CD 检查状态

4. **监控自动化流程**
   - 定期查看 Actions 运行记录
   - 及时处理失败的工作流

5. **保持文档更新**
   - 更新 CHANGELOG
   - 维护 API 文档

## 相关文档

- [GITHUB_SETUP.md](./GITHUB_SETUP.md) - GitHub 仓库配置完整指南
- [CONTRIBUTING.md](./CONTRIBUTING.md) - 开发规范和提交指南
- [AUTO_DELETE_BRANCH.md](./AUTO_DELETE_BRANCH.md) - 自动删除分支功能
- [README.md](./README.md) - 项目说明

## 更新日志

- **2025-02-07**: 新增自动审批和合并功能
- **2025-02-07**: 完全自动化流程，无需人工干预
