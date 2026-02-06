# 自动删除已合并分支功能说明

## 功能概述

GitHub Actions 现在会在 Pull Request 合并到 `main` 分支后，自动删除已合并的功能分支，保持仓库整洁。

## 工作原理

1. **检测合并事件**
   - 当代码合并到 `main` 分支时触发 GitHub Actions
   - 获取最新的 merge commit 信息

2. **提取分支名称**
   - 从 merge commit 中提取源分支名称
   - 格式示例：`Merge pull request #123 from username/feature-branch`

3. **安全检查**
   - 验证分支名称不为空
   - 确保不是 `main` 或 `master` 分支
   - 仅删除功能分支（`feature/*`、`fix/*` 等）

4. **删除分支**
   - 删除远程分支
   - 输出删除结果日志

## 保护规则

以下分支**不会被自动删除**：
- `main`
- `master`
- 任何以 `main` 或 `master` 开头的分支

## 使用示例

### 正常流程

```bash
# 1. 创建功能分支
git checkout -b feature/new-feature

# 2. 开发和提交
git commit -m "feat: 添加新功能"

# 3. 推送分支
git push origin feature/new-feature

# 4. 创建 PR 并合并
# PR 合并后，GitHub Actions 自动删除 feature/new-feature 分支
```

### 保留分支

如果需要保留某个已合并的分支：

#### 方法一：本地保留副本
```bash
# 合并前，创建备份
git branch backup/my-feature my-feature

# 合并 PR 后，原分支被删除，但备份分支保留
```

#### 方法二：事后重新创建
```bash
# 合并后，如果需要继续开发
git fetch origin
git checkout -b my-feature origin/my-feature
```

## 禁用功能

如需禁用自动删除分支功能：

1. 编辑 `.github/workflows/version.yml`
2. 注释或删除 "Delete merged branch" 步骤
3. 提交并推送更改

```yaml
# - name: Delete merged branch
#   run: |
#     # ... 删除逻辑
```

## 常见问题

### Q: 为什么我的分支没有被删除？

可能的原因：
1. 分支是 `main` 或 `master`（受保护）
2. 分支不是通过 PR 合并的
3. Actions 工作流运行失败
4. 分支在删除前已被手动删除

### Q: 如何恢复被删除的分支？

```bash
# 如果本地还有分支，直接推送
git push origin my-feature

# 如果本地也没有，从历史记录恢复
git reflog  # 查找分支的 commit hash
git checkout -b my-feature <commit-hash>
git push origin my-feature
```

### Q: 可以禁用特定分支的自动删除吗？

目前不支持，可以通过以下方式实现：
- 将分支重命名为受保护分支名称（不推荐）
- 禁用自动删除功能
- 合并后手动重新创建分支

## 相关文档

- [CONTRIBUTING.md](./CONTRIBUTING.md) - 完整开发指南
- [GITHUB_SETUP.md](./GITHUB_SETUP.md) - GitHub 配置指南
- [PROJECT_INIT_REPORT.md](./PROJECT_INIT_REPORT.md) - 项目初始化报告

## 更新日志

- **2025-02-07**: 新增自动删除已合并分支功能
