# 设计板块 (Designing Sections)

完成[产品规划](product-planning.md)后，您就可以准备设计各个板块了。按路线图（roadmap）上的顺序在每个板块中工作，为每一个板块完成以下步骤。

## 1. 勾勒板块形状 (Shape the Section)

```
/shape-section
```

定义该板块的功能，并生成其示例数据 —— 一步到位。如果您有多个板块，系统会询问您要处理哪一个。

这是一个对话过程，用于建立：

- **概述** — 这个板块的目的是什么（2-3句话）
- **用户流程 (User flows)** — 主要的动作和步骤交互
- **UI 需求** — 需要特定的布局、模式或组件
- **范围边界 (Scope boundaries)** — 有意排除了哪些内容

分享您的任何笔记或想法。AI 会就用户操作、要显示的信息以及 UI 模式提出澄清问题。重点关注用户体验和界面需求 —— 不要涉及后端或数据库的详细信息。

它还会询问您这个板块是应该在应用程序外壳（shell）内显示（大多数板块都是这样），还是作为独立页面显示（例如登录（landing）页面或嵌入式小部件）。

一旦获得足够的信息，AI 就会编写规范并自动生成示例数据和 TypeScript 类型：

- **示例数据** — 5-10 条逼真的记录，包含多样的内容、边缘情况以及描述每个实体的 `_meta` 部分
- **TypeScript 类型** — 每个实体的数据接口（interfaces），加上包含回调函数（用于处理 action）的 Props 接口

**创建了：**
- `product/sections/[section-id]/spec.md` — 板块规范 (specification)
- `product/sections/[section-id]/data.json` — 带有 `_meta` 描述的示例数据
- `product/sections/[section-id]/types.ts` — TypeScript 接口 (interfaces)

**如果以后需要更新示例数据：** 运行 `/sample-data` 来修改数据结构或示例记录。

## 2. 设计界面屏幕 (Design the Screen)

```
/design-screen
```

为该板块构建实际的 React 组件。这是规范（spec）和示例数据变成工作 UI 的地方。

### 产生了什么

**可导出的组件** (基于 props 的、便携（portable）的)：

主组件及任何子组件都通过 props 接受数据和回调函数。这就是将要被导出到您代码库的内容。

```tsx
// 示例：组件接收 props，从不直接导入（import）数据
export function InvoiceList({
  invoices,
  onView,
  onEdit,
  onDelete,
  onCreate
}: InvoiceListProps) {
  // ...
}
```

**预览包装器 (Preview wrapper)** (仅用于 Design OS):

一个导入示例数据并将其提供给组件的 wrapper，以便您可以在 Design OS 中看到它运行的情况。

### 设计要求

所有的屏幕屏幕都包含：

- **移动端响应式** — Tailwind 响应式前缀 (`sm:`, `md:`, `lg:`)
- **亮色和暗色模式** — 使用 `dark:` 变体
- **应用了设计标记(tokens)** — 您的调色板和字体排版选择
- **所有规范(spec)要求** — 实现了每一个用户流程和 UI 要求

### 多个视图 (Multiple Views)

如果规范暗示需要有多个视图（列表视图，详情视图，表单等），我们将询问您首先构建哪一个。再次运行 `/design-screen` 即可创建更多的视图。

**创建了：**
- `src/sections/[section-id]/components/[ViewName].tsx` — 主组件
- `src/sections/[section-id]/components/[SubComponent].tsx` — 根据需要可能有的子组件
- `src/sections/[section-id]/components/index.ts` — 组件导出
- `src/sections/[section-id]/[ViewName].tsx` — 预览包装器 wrapper

**注意：** 创建屏幕设计后，请重新启动开发服务器以查看更改。

## 3. 截取屏幕截图（可选）

```
/screenshot-design
```

为您的屏幕设计截图以备文档使用。截图将和规范（spec）以及数据文件保存在一起。

这个命令会：
1. 自动启动开发服务器
2. 导航到您的界面屏幕
3. 隐藏 Design OS 的导航栏
4. 捕获完整页面的屏幕截图

屏幕截图可用于：
- 在实现过程中的视觉参考
- 文档和移交资料
- 跨主要版块比较设计

**要求前提：** Playwright MCP server。如果未安装，您将收到设置说明的提示。

**创建了：** `product/sections/[section-id]/[screen-name].png`

## 对每个主要版块 (Section) 重复上述步骤

按顺序完成您的路线图板块。每个板块都在您建立的基础之上构建，并受益于您的全局数据形状（data shape）和设计标记的一致性。

## 下一步是什么

当所有板块设计完成后，您就可以准备导出了。请参阅 [导出 (Export)](export.md) 获取如何生成完整移交包的信息。
