# YOLOv8 视觉识别项目

基于 YOLOv8n 的视觉识别 Web 应用，支持 PC 端和移动端 H5，使用 Vue 3 + Tailwind CSS 构建。

## 项目简介

本项目是一个基于 YOLOv8n 神经网络模型的视觉识别系统，提供智能物体检测和标记功能。通过 Web 端界面，用户可以上传图片或使用摄像头进行实时物体识别。

### 核心特性

- 🎯 **基于 YOLOv8n 模型** - 使用轻量级 YOLOv8n 模型，提供快速准确的物体识别
- 💻 **PC 端支持** - 完美适配桌面浏览器，提供良好的交互体验
- 📱 **移动端 H5** - 响应式设计，适配各种移动设备屏幕
- 🎨 **Tailwind CSS v4** - 现代化样式解决方案，支持主题定制
- ⚡ **Vue 3 + Vite** - 高性能前端框架，快速开发和部署
- 🔄 **自动版本管理** - GitHub Actions 自动化版本发布
- 🗑️ **自动清理分支** - PR 合并后自动删除已合并分支，保持仓库整洁

## 版本信息

| 项目 | 版本 |
|------|------|
| 当前版本 | [![npm version](https://badge.fury.io/js/yolo_v8.svg)](https://badge.fury.io/js/yolo_v8) |
| Node.js | >= 16.x |
| Vue | ^3.5.24 |
| Vite | ^7.2.4 |
| Tailwind CSS | ^4.1.18 |

### 版本更新记录

查看 [CHANGELOG.md](./CHANGELOG.md) 了解详细的版本更新历史。

## GitHub Flow 工作流程

本项目采用 **GitHub Flow** 进行版本管理和协作开发。

### 工作流程

1. **创建功能分支**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **开发和提交**
   ```bash
   git add .
   git commit -m "feat: 添加新功能"
   ```

   遵循以下提交规范：
   - `feat:` 新功能（触发 minor 版本更新）
   - `fix:` 修复bug（触发 patch 版本更新）
   - `feat!:` 破坏性变更（触发 major 版本更新）

3. **推送分支**
   ```bash
   git push origin feature/your-feature-name
   ```

4. **创建 Pull Request**
   - 在 GitHub 上创建 PR 到 `main` 分支
   - 填写 PR 描述，说明变更内容
   - 等待代码审查和合并

5. **自动发布**
    - PR 合并到 `main` 分支后，GitHub Actions 自动触发
    - 自动更新版本号（根据提交类型）
    - 自动生成 CHANGELOG
    - 自动创建 Git Tag
    - 自动创建 GitHub Release
    - **自动删除已合并的分支** ⭐（保持仓库整洁）

### 自动化版本管理

当代码合并到 `main` 分支时，GitHub Actions 会自动：

- ✅ 分析提交信息确定版本类型（major/minor/patch）
- ✅ 更新 `package.json` 中的版本号
- ✅ 更新 `README.md` 版本记录
- ✅ 生成 `CHANGELOG.md` 更新日志
- ✅ 创建 Git Tag（vX.X.X 格式）
- ✅ 创建 GitHub Release
- ✅ 推送所有变更到远程仓库
- ✅ **自动删除已合并的分支** ⭐（保护 main/master 分支）

### 分支策略

- `main`: 主分支，始终保持稳定状态
- `feature/*`: 功能开发分支
- `fix/*`: bug 修复分支
- `hotfix/*`: 紧急修复分支（较少使用）

### 提交信息示例

```bash
# 新功能 - 触发 minor 版本更新
git commit -m "feat(recognition): 添加实时摄像头识别功能"

# 修复bug - 触发 patch 版本更新
git commit -m "fix(mobile): 修复移动端适配问题"

# 破坏性变更 - 触发 major 版本更新
git commit -m "feat(api)!: 重构识别接口，不兼容旧版本"
```

## 技术栈

- **前端框架**: Vue 3.5+
- **构建工具**: Vite 7.2+
- **样式方案**: Tailwind CSS 4.1+
- **编程语言**: JavaScript
- **视觉模型**: YOLOv8n （已集成）

## 项目结构

```
yolo_v8/
├── public/              # 静态资源
├── src/
│   ├── assets/          # 资源文件
│   ├── components/      # 组件目录
│   ├── App.vue          # 根组件
│   ├── main.css         # 全局样式（Tailwind）
│   └── main.js          # 入口文件
├── index.html           # HTML 模板
├── package.json         # 依赖配置
└── README.md           # 项目文档
```

## 快速开始

### 环境要求

- Node.js 16.x 或更高版本
- npm 或 yarn 包管理器

### 安装依赖

```bash
npm install
```

### 开发模式

```bash
npm run dev
```

项目将在 `http://localhost:5173` 启动（实际端口可能不同）

### 生产构建

```bash
npm run build
```

构建产物将生成在 `dist/` 目录

### 预览生产版本

```bash
npm run preview
```

## 功能规划

### 已完成

- ✅ 项目基础脚手架搭建
- ✅ Tailwind CSS v4 配置
- ✅ PC 端和移动端响应式布局
- ✅ 基础页面结构
- ✅ GitHub Actions 自动版本管理
- ✅ GitHub Flow 工作流程

### 待开发功能

- ⏳ YOLOv8n 模型集成
- ⏳ 图片上传和识别功能
- ⏳ 实时摄像头识别
- ⏳ 识别结果展示和标记
- ⏳ 识别历史记录
- ⏳ 批量图片处理
- ⏳ 导出识别结果
- ⏳ 性能优化和加载提示



## 浏览器兼容性

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## 移动端适配

项目采用响应式设计，支持以下屏幕尺寸：

- 桌面端：≥ 1024px
- 平板端：768px - 1023px
- 移动端：< 768px

## 文档索引

| 文档 | 说明 |
|------|------|
| [README.md](./README.md) | 项目介绍和快速开始 |
| [CONTRIBUTING.md](./CONTRIBUTING.md) | 开发指南和提交规范 |
| [GETTING_STARTED.md](./GETTING_STARTED.md) | 快速启动指南 |
| [GITHUB_SETUP.md](./GITHUB_SETUP.md) | GitHub 仓库配置指南 |
| [AUTO_DELETE_BRANCH.md](./AUTO_DELETE_BRANCH.md) | 自动删除分支功能说明 |
| [CHANGELOG.md](./CHANGELOG.md) | 版本更新日志 |
| [PROJECT_INIT_REPORT.md](./PROJECT_INIT_REPORT.md) | 项目初始化报告 |

## License

MIT

## 联系方式

如有问题或建议，欢迎提交 Issue。
