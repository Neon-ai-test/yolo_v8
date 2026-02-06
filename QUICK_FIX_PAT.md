# 快速修复：使用 PAT 解决推送权限问题

## 问题

分支保护规则中找不到 "Restrict who can push" 选项，导致 GitHub Actions 无法推送变更。

## 解决方案：使用 Personal Access Token（5分钟完成）

### 步骤 1：创建 Personal Access Token

1. **访问 GitHub Token 设置**
   - 点击右上角头像
   - 选择 **Settings**
   - 左侧滚动到最底部，点击 **Developer settings**
   - 点击 **Personal access tokens** → **Tokens (classic)**

2. **生成新 Token**
   - 点击 **Generate new token (classic)** 按钮
   - **Note**: 输入 `yolo_v8-version-management`（方便识别）
   - **Expiration**: 选择 `90 days` 或 `No expiration`
   - **Select scopes**（权限设置）：
     - ✅ 勾选 `repo`（这会包含所有仓库相关权限）
   - 点击页面底部的 **Generate token**

3. **复制 Token**
   - ⚠️ **重要**: Token 只显示一次，立即复制！
   - 复制完整的 token（以 `ghp_` 开头）

### 步骤 2：添加到 GitHub Secrets

1. **访问仓库 Secrets**
   - 回到 `yolo_v8` 仓库主页
   - 点击 **Settings**
   - 左侧点击 **Secrets and variables** → **Actions**
   - 点击 **New repository secret**

2. **添加 Secret**
   - **Name**: 输入 `VERSION_TOKEN`（大小写要完全一致）
   - **Secret**: 粘贴刚才复制的 token
   - 点击 **Add secret**

### 步骤 3：修改工作流文件

编辑 `.github/workflows/version.yml`，找到 "Checkout code" 步骤：

```yaml
- name: Checkout code
  uses: actions/checkout@v4
  with:
    fetch-depth: 0
    token: ${{ secrets.VERSION_TOKEN }}  # 改为 VERSION_TOKEN
```

同样修改 `.github/workflows/auto-merge.yml`（如果有 token 相关配置）。

### 步骤 4：推送更改

```bash
git add .
git commit -m "fix(ci): 使用 PAT 解决推送权限问题"
git push origin fix-修复twcss依赖
```

## 验证

1. 提交后，创建或更新 PR
2. PR 会自动审批和合并
3. 合并后，检查 Actions 日志
4. 应该能看到 "Changes pushed successfully"

## 常见问题

### Q: Token 会过期怎么办？

A: Token 过期后：
1. 重复步骤 1 生成新 token
2. 在 Secrets 设置中删除旧的 `VERSION_TOKEN`
3. 添加新的 `VERSION_TOKEN`

### Q: 为什么不用 `GITHUB_TOKEN`？

A: `GITHUB_TOKEN` 受分支保护规则限制，无法直接推送。PAT 有更高权限，可以绕过此限制。

### Q: PAT 安全吗？

A: 只要注意以下几点就安全：
- ✅ 不要在代码中硬编码
- ✅ 使用 GitHub Secrets 存储
- ✅ 不要分享给他人
- ✅ 定期轮换（每 90 天）

### Q: 可以用 `github-actions[bot]` 吗？

A: 这个账户无法手动添加。正确的方法是使用 PAT 或禁用推送限制。但既然找不到该选项，PAT 是最佳选择。

## 完成！

按照以上步骤完成后，GitHub Actions 就可以正常推送版本变更了。
