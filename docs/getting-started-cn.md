# 快速开始 (Getting Started)

## 克隆仓库代码

```bash
git clone https://github.com/buildermethods/design-os.git my-project-design
cd my-project-design
```

将 `my-project-design` 替换为您想给您的设计工作区命名的任何名称。

## 移除原始远程仓库关联

```bash
git remote remove origin
```

现在您就拥有了一个干净的本地实例备用。

## 安装依赖

```bash
npm install
```

## 启动开发服务器

```bash
npm run dev
```

在您的浏览器中打开 [http://localhost:5173](http://localhost:5173)。

## 打开 Claude Code

在同一个项目目录下，启动 Claude Code (或者您的其它 AI 助手)：

```bash
claude
```

现在您就可以开始设计了。运行 `/product-vision` 以开始定义您的产品。

---

## 可选：保存为您自己的模板

如果您想在未来的项目中重用 Design OS：

1. 推送到您自己的 GitHub 仓库：

```bash
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
git push -u origin main
```

2. 访问您在 GitHub 上的仓库，点击 **Settings (设置)**，然后勾选 **Template repository (模板仓库)**。

现在您就能使用 GitHub 的 "Use this template" 按钮来创建新的实例了。
