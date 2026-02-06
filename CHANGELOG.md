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
