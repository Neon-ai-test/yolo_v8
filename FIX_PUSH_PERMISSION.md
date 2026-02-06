# 解决版本管理工作流推送失败问题

## 问题描述

错误信息：`GH013: Repository rule violations found for refs/heads/main`

原因：分支保护规则禁止直接推送到 main 分支，但版本管理工作流需要直接推送变更。

## 解决方案

### 方案一：关闭推送限制（最简单，推荐）

**步骤：**

1. 访问 GitHub 仓库设置
   - 进入仓库主页
   - 点击 **Settings**
   - 左侧菜单点击 **Branches**

2. 编辑 main 分支保护规则
   - 找到 main 分支的规则
   - 点击 **Edit** 或铅笔图标

3. 取消推送限制
   - 找到 **Restrict who can push to matching branches**
   - ❌ **取消勾选** 该选项

4. 保存规则
   - 点击页面底部的 **Save changes**

**说明：**
即使取消此限制，仍然保留了 "Require a pull request before merging" 规则，所以普通开发者仍然无法直接推送，只有 GitHub Actions 机器人可以。

**验证：**
保存后，版本管理工作流应该可以正常推送了。

### 方案二：使用 Personal Access Token（需要更多配置）

如果你必须限制推送权限，可以使用此方案：

#### 1. 创建 Personal Access Token

1. 访问 GitHub Settings
   - 点击右上角头像 > **Settings**
   - 左侧菜单最下方点击 **Developer settings**
   - 点击 **Personal access tokens** > **Tokens (classic)**

2. 生成新 token
   - 点击 **Generate new token (classic)**
   - **Note**: 输入描述，如 "Auto version management"
   - **Expiration**: 选择过期时间（建议 90 天）
   - **Select scopes**: 勾选以下权限：
     - ✅ `repo` (完整仓库访问权限)
     - ✅ `workflow` (工作流权限)
   - 点击 **Generate token**
   - **重要**: 复制 token（只显示一次！）

#### 2. 添加到 GitHub Secrets

1. 访问仓库的 Secrets 设置
   - 进入仓库主页
   - 点击 **Settings**
   - 左侧菜单点击 **Secrets and variables** > **Actions**

2. 添加新的 secret
   - 点击 **New repository secret**
   - **Name**: 输入 `ADMIN_TOKEN`
   - **Secret**: 粘贴刚才复制的 PAT
   - 点击 **Add secret**

#### 3. 修改工作流文件

编辑 `.github/workflows/version.yml`：

找到 "Checkout code" 步骤，修改 `token` 参数：

```yaml
- name: Checkout code
  uses: actions/checkout@v4
  with:
    fetch-depth: 0
    token: ${{ secrets.ADMIN_TOKEN }}  # 使用 ADMIN_TOKEN 而不是 GITHUB_TOKEN
```

#### 4. 同时修改其他使用 token 的地方

如果工作流中有其他地方使用了 `GITHUB_TOKEN` 用于推送，也需要改为 `ADMIN_TOKEN`。

## 方案对比

| 方案 | 优点 | 缺点 | 推荐度 |
|------|------|------|--------|
| 允许机器人推送 | • 简单<br>• 不需要管理额外 secret<br>• 官方推荐方式 | • 需要仓库管理员权限 | ⭐⭐⭐⭐⭐ |
| 使用 PAT | • 灵活<br>• 可以绕过所有限制 | • 需要管理 secret<br>• Token 需要定期更新<br>• 安全性较低 | ⭐⭐⭐ |

## 安全建议

如果使用 PAT：

1. **定期轮换** - 每 90 天更新一次
2. **最小权限** - 只授予必要的权限
3. **监控使用** - 定期检查 token 使用情况
4. **撤销不用** - 不需要时立即删除

## 验证修复

配置完成后，重新触发工作流：

1. 如果 PR 已经存在，重新推送触发
2. 或者创建新的 PR 测试
3. 检查 Actions 日志确认推送成功

## 常见问题

### Q: 为什么不能直接推送到 main 分支？

A: 这是分支保护规则的安全性要求，防止意外破坏主分支。但为了自动化版本管理，需要给 GitHub Actions 特殊权限。

### Q: 添加 `github-actions[bot]` 安全吗？

A: 安全。这是 GitHub 官方提供的机器人，专门用于自动化任务。你仍然保留自己的推送权限。

### Q: PAT 会泄露吗？

A: 如果正确使用 GitHub Secrets，token 不会泄露。但要注意：
- 不要在代码中硬编码
- 不要在 PR 描述中显示
- 定期轮换 token

### Q: 可以两种方案同时使用吗？

A: 可以但不推荐。使用方案一就足够了，同时使用会增加复杂性和安全风险。

## 相关文档

- [GitHub Actions 权限](https://docs.github.com/en/actions/security-guides/automatic-token-authentication)
- [分支保护规则](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-branch-protection-rules)
- [Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
