# @yxai/code

意心Code - Claude Code 可视化交互界面

一个基于 Express + WebSocket + Claude Agent SDK 的可视化交互界面，让你可以通过 Web 界面与 Claude AI 进行交互。

## 特性

- 🚀 极简架构：Express 静态服务 + WebSocket + Claude Agent SDK
- 🎨 可视化界面：友好的 Web UI
- 🔌 WebSocket 实时通信
- 📦 零构建：仅依赖 Node.js，无需构建工具
- 🌐 自动打开浏览器

## 安装

```bash
npm install -g @yxai/code
```

## 使用

安装后，直接运行命令：

```bash
yxaiCode
```

程序会自动：
1. 启动服务器（默认端口 3456）
2. 打开浏览器访问 http://localhost:3456

### 命令行选项

```bash
# 显示帮助信息
yxaiCode --help

# 显示版本号
yxaiCode --version

# 指定端口号
yxaiCode --port 8080
```

### 环境变量

```bash
# 使用环境变量指定端口
PORT=8080 yxaiCode
```

## 系统要求

- Node.js >= 18.0.0

## 开发

```bash
# 克隆仓库
git clone https://gitee.com/ccnetcore/yxcode.git
cd yxcode

# 安装依赖
npm install

# 本地测试
npm link
yxaiCode

# 取消链接
npm unlink -g @yxai/code
```

## 许可证

MIT
