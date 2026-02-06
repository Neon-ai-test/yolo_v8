# 项目启动说明

## 快速启动

```bash
# 进入项目目录
cd yolo_v8

# 安装依赖（首次运行需要）
npm install

# 启动开发服务器
npm run dev
```

## 访问地址

开发服务器启动后，在浏览器中访问：
- 本地地址: http://localhost:5173
- 局域网地址: http://[本机IP]:5173 (使用 --host 参数)

## 常用命令

```bash
# 开发模式
npm run dev

# 生产构建
npm run build

# 预览生产版本
npm run preview
```

## 开发提示

1. 项目已配置 Tailwind CSS v4，可直接使用 Tailwind 类名
2. 响应式断点：sm(640px)、md(768px)、lg(1024px)、xl(1280px)
3. 移动端已禁用缩放：user-scalable=no
4. 主样式文件：src/main.css
5. 入口文件：src/main.js
