# 运行要求 (Requirements)

## 运行 Design OS

Design OS 运行在您的本地机器上。您将需要：

- **Node.js** (v18 或更高版本)
- **npm** (随 Node.js 一起安装)
- **一个 AI 编码助手** — Design OS 使用斜杠命令（slash commands）来指导设计过程。推荐使用 Claude Code，但您也可以在任何支持自定义命令或提示词提示的 AI 编码工具中（如 Cursor, Windsurf, Codex 等）调用 Design OS 命令。

## 安装您导出的组件

当您导出您的设计时，您会得到可用于生产环境（production-ready）的 React 组件。您的目标代码库需要：

### 基本要求

- **React** (v18 或更高版本)
- **Tailwind CSS** (v4) — 组件使用 Tailwind 功能类（utility classes）进行样式设置

### 后端

您的后端可以是任何技术栈——Rails、Laravel、Next.js API 路由、Python、Go，任何都可以。Design OS 仅处理前端 UI 设计层。
