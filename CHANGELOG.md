## [0.1.0] - 2026-02-09

### 提交记录
- Merge pull request #10 from Neon-ai-test/flx_2
- ci(workflows): 改进版本发布工作流的健壮性和日志记录
- Merge pull request #9 from Neon-ai-test/flx-1
- ci: 优化自动合并和版本发布工作流
- Merge pull request #7 from Neon-ai-test/fix-修复twcss依赖
- ci: 优化自动合并和版本管理流程
- Merge pull request #6 from Neon-ai-test/fix-修复twcss依赖
- ci(workflows): 优化自动合并工作流并移除重复分支删除逻辑
- Merge pull request #5 from Neon-ai-test/fix-修复twcss依赖
- ci: 更新工作流使用 VERSION_TOKEN 并移除冗余配置
- Merge pull request #4 from Neon-ai-test/fix-修复twcss依赖
- ci: 添加Git配置并更新发布工作流
- Merge pull request #3 from Neon-ai-test/fix-修复twcss依赖
- ci: 移除独立的auto-approve工作流并优化版本管理流程
- Merge pull request #2 from Neon-ai-test/fix-修复twcss依赖
- feat(ci): 添加自动审批和合并工作流
- feat: 添加并配置Tailwind CSS及相关依赖
- docs: 更新 README 添加自动删除分支功能说明
- docs: 添加自动删除分支功能说明文档
- feat: 添加自动删除已合并分支功能
- docs: 添加项目初始化完成报告
- docs: 添加 GitHub 仓库配置指南

# 更新日志

本文档记录了项目的所有重要变更。

## [未发布]

## [版本格式] - YYYY-MM-DD

### 提交记录
- 提交内容1
- 提交内容2

---

## 版本说明

本项目遵循 [语义化版本 2.0.0](https://semver.org/lang/zh-CN/) 规范：

- **MAJOR** (主版本号): 破坏性变更或不兼容的API修改
- **MINOR** (次版本号): 向后兼容的功能性新增
- **PATCH** (修订号): 向后兼容的问题修正

## 版本更新触发规则

根据提交信息自动判断版本更新类型：

- `major`: 提交信息包含 `BREAKING CHANGE`, `major`, `!:`, `破坏性`, `重大更新` 等关键词
- `minor`: 提交信息包含 `feat`, `feature`, `新增`, `添加`, `new`, `add` 等关键词
- `patch`: 其他类型的提交（默认）

## 提交信息规范建议

为了更好地自动化版本管理，建议使用以下提交信息格式：

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type 类型

- `feat`: 新功能
- `fix`: 修复bug
- `docs`: 文档更新
- `style`: 代码格式调整（不影响功能）
- `refactor`: 重构
- `perf`: 性能优化
- `test`: 测试相关
- `chore`: 构建/工具链更新
- `revert`: 回滚

### 示例

```bash
# 新功能 - 会触发 minor 版本更新
git commit -m "feat(recognition): 添加摄像头实时识别功能"

# 修复bug - 会触发 patch 版本更新
git commit -m "fix(ui): 修复移动端显示问题"

# 破坏性变更 - 会触发 major 版本更新
git commit -m "feat(api)!: 重构识别结果接口，不兼容旧版本"
```
