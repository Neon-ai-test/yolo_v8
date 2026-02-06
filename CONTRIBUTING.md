# 开发指南

本文档提供项目开发的详细指南，包括 GitHub Flow 工作流程、提交规范和自动化版本管理。

## 目录

- [GitHub Flow 工作流程](#github-flow-工作流程)
- [分支管理](#分支管理)
- [提交规范](#提交规范)
- [自动化版本管理](#自动化版本管理)
- [常见问题](#常见问题)

## GitHub Flow 工作流程

GitHub Flow 是一个简单且高效的分支管理工作流，适合持续部署的开发模式。

### 完整流程

```
1. 从 main 分支创建功能分支
   ↓
2. 在功能分支上进行开发和提交
   ↓
3. 推送功能分支到远程仓库
   ↓
4. 创建 Pull Request
   ↓
5. 代码审查和讨论
   ↓
6. 合并 Pull Request 到 main
   ↓
7. GitHub Actions 自动触发版本发布
```

## 分支管理

### 主分支 (main)

- **用途**: 主开发分支，始终保持可部署状态
- **保护规则**:
  - 禁止直接推送
  - 必须通过 Pull Request 合并
  - 需要通过代码审查
  - 需要通过 CI 检查

### 功能分支 (feature/*)

- **命名规范**: `feature/<功能名称>`
- **示例**:
  - `feature/recognition`
  - `feature/mobile-adaptation`
  - `feature/image-upload`

### 修复分支 (fix/*)

- **命名规范**: `fix/<问题描述>`
- **示例**:
  - `fix/mobile-display`
  - `fix/api-error`

### 热修复分支 (hotfix/*)

- **命名规范**: `hotfix/<紧急问题描述>`
- **用途**: 生产环境紧急问题修复
- **示例**:
  - `hotfix/critical-bug`

## 提交规范

### 提交信息格式

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type 类型

| Type | 说明 | 触发版本更新 |
|------|------|-------------|
| `feat` | 新功能 | minor |
| `fix` | 修复 bug | patch |
| `docs` | 文档更新 | patch |
| `style` | 代码格式（不影响功能）| patch |
| `refactor` | 重构 | patch |
| `perf` | 性能优化 | patch |
| `test` | 测试相关 | patch |
| `chore` | 构建/工具链更新 | patch |
| `revert` | 回滚 | patch |

### Scope 范围

| Scope | 说明 |
|-------|------|
| `recognition` | 识别功能 |
| `ui` | 用户界面 |
| `mobile` | 移动端 |
| `api` | API 接口 |
| `perf` | 性能 |
| `docs` | 文档 |

### Subject 标题

- 使用动词开头
- 首字母小写
- 不超过 50 字符
- 不要以句号结尾

### Body 正文

- 详细描述变更内容
- 说明变更原因
- 每行不超过 72 字符

### Footer 页脚

- 破坏性变更: `BREAKING CHANGE:`
- 关联 Issue: `Closes #123`
- 关联 PR: `Refs #456`

### 提交示例

```bash
# 新功能
git commit -m "feat(recognition): 添加实时摄像头识别功能

集成 ONNX Runtime，支持通过摄像头实时识别物体"

# 修复 bug
git commit -m "fix(mobile): 修复移动端缩放问题

由于 viewport 配置不正确导致移动端显示异常"

# 文档更新
git commit -m "docs: 更新 GitHub Flow 文档

补充自动化版本管理的详细说明"

# 破坏性变更
git commit -m "feat(api)!: 重构识别结果接口

BREAKING CHANGE: 识别结果数据结构发生变更，需要更新客户端代码"

# 关联 Issue
git commit -m "fix(ui): 修复按钮点击无效问题

Closes #123"
```

## 自动化版本管理

### 版本号格式

遵循 [语义化版本 2.0.0](https://semver.org/lang/zh-CN/) 规范：

```
MAJOR.MINOR.PATCH
```

- **MAJOR**: 破坏性变更或不兼容的API修改
- **MINOR**: 向后兼容的功能性新增
- **PATCH**: 向后兼容的问题修正

### 版本更新触发规则

根据最新提交的信息自动判断版本更新类型：

#### MAJOR 更新触发条件

提交信息包含以下任一关键词：
- `BREAKING CHANGE`
- `major`
- `!:`
- `破坏性`
- `重大更新`

#### MINOR 更新触发条件

提交信息包含以下任一关键词：
- `feat`
- `feature`
- `新增`
- `添加`
- `new`
- `add`

#### PATCH 更新（默认）

其他所有类型的提交

### 自动化流程

当代码合并到 `main` 分支时，GitHub Actions 执行以下步骤：

1. **获取当前版本号**
   - 从 `package.json` 读取版本

2. **分析提交信息**
   - 获取自上次标签以来的所有提交
   - 根据关键词判断版本类型

3. **计算新版本号**
   - 根据 version type 递增对应部分

4. **更新版本信息**
   - 更新 `package.json` 中的 version
   - 更新 `README.md` 版本记录
   - 生成 `CHANGELOG.md` 更新日志

5. **创建 Git 标签**
   - 创建格式为 `vX.X.X` 的标签

6. **提交变更**
   - 自动提交所有版本相关文件
   - 推送到远程仓库

7. **创建 GitHub Release**
   - 自动创建 Release
   - 包含版本信息和变更日志

### 手动触发版本更新

如果需要手动触发版本更新（不依赖提交信息），可以在 main 分支创建一个特殊提交：

```bash
# 触发 major 更新
git commit --allow-empty -m "chore(release): major release"

# 触发 minor 更新
git commit --allow-empty -m "chore(release): minor release"

# 触发 patch 更新
git commit --allow-empty -m "chore(release): patch release"
```

## 常见问题

### Q: 如何回滚到上一个版本？

```bash
# 查看历史版本
git tag

# 回滚到指定版本
git checkout v1.0.0

# 创建回滚分支
git checkout -b rollback-v1.0.0
```

### Q: 如何跳过自动版本更新？

如果不想触发自动版本更新（例如：文档更新、小改动），可以：

1. 在 PR 标题中添加 `[skip ci]`
2. 或者在提交信息中包含 `skip ci`

### Q: 如何修改自动化版本管理规则？

修改 `.github/workflows/version.yml` 文件中的判断逻辑。

### Q: 如何查看版本更新历史？

查看以下文件：
- `CHANGELOG.md`: 详细的变更日志
- `package.json`: 当前版本号
- GitHub Releases: 所有已发布版本

### Q: 版本号冲突怎么办？

如果手动修改了版本号导致冲突：

```bash
# 重置到远程最新状态
git fetch origin
git reset --hard origin/main

# 重新合并
```

## 最佳实践

1. **保持提交信息清晰**
   - 使用规范的提交格式
   - 详细描述变更内容

2. **小步快跑**
   - 频繁提交，保持每次提交粒度适中
   - 便于代码审查和问题定位

3. **及时合并**
   - 功能完成后尽快合并
   - 避免长期存在过时的功能分支

4. **代码审查**
   - 所有 PR 必须经过审查
   - 确保代码质量和一致性

5. **测试覆盖**
   - 新功能必须包含测试
   - 确保 CI 检查通过

## 参考资源

- [GitHub Flow 官方文档](https://guides.github.com/introduction/flow/)
- [语义化版本规范](https://semver.org/lang/zh-CN/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [GitHub Actions 文档](https://docs.github.com/en/actions)
