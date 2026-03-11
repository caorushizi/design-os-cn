# Design OS 的 Agent 指南

Design OS 是一个**产品规划和设计工具**，它可以帮助用户定义他们的产品愿景，勾勒他们的数据形状，设计他们的 UI，并准备导出包以便在一个独立的代码库中实现。

> **重要提示**：Design OS 是一个规划工具，而非最终的产品代码库。这里生成的屏幕界面设计和组件旨在被导出，并集成到您实际产品的代码库中。

---

## 理解 Design OS 上下文

当在 Design OS 中工作时，请注意两个截然不同的上下文：

### 1. Design OS 应用程序
用于显示和管理规划文件的 React 应用程序。当修改 Design OS UI 本身时：
- 文件位于 `src/` (components, pages, utilities)
- 使用 Design OS 设计系统 (stone palette, DM Sans, etc.)
- 提供用于查看规范 (specs)、屏幕设计、导出等的界面。

### 2. 产品设计（屏幕设计与导出）
您正在规划和设计的产品。当创建屏幕设计和导出时：
- 屏幕设计组件位于 `src/sections/[section-name]/` 和 `src/shell/`
- 产品定义文件位于 `product/`
- 导出被打包到 `product-plan/` 以集成到一个独立的代码库
- 遵循在每个板块（section）规范中指定的设计要求

---

## 快速入门 — 规划流程

Design OS 遵循一个结构化的规划序列：

### 1. 产品愿景 (`/product-vision`)
在一次对话流程中定义您的产品概述、路线图板块（roadmap sections）以及数据形状。回答澄清问题后，所有三个文件都会自动生成。
**输出:** `product/product-overview.md`, `product/product-roadmap.md`, `product/data-shape/data-shape.md`

使用 `/product-roadmap`, `/data-shape` 可在这两个文件初始创建后分别更新它们。

### 2. 设计系统 (`/design-tokens`)
选择您的颜色调色板 (来自 Tailwind) 和排版字体 (来自 Google Fonts)。这些标记（tokens）应用于所有的屏幕设计。
**输出:** `product/design-system/colors.json`, `product/design-system/typography.json`

### 3. 应用外壳 (`/design-shell`)
设计包裹所有板块的持久性导航和布局。
**输出:** `product/shell/spec.md`, `src/shell/components/`

### 4. 对于每个板块 (Section):
- `/shape-section` — 定义规范（spec）并生成示例数据和类型（types）
- `/sample-data` — 更新示例数据和类型（即使已经创建）
- `/design-screen` — 创建屏幕设计
- `/screenshot-design` — 截取屏幕截图

### 5. 导出 (`/export-product`)
生成包含所有组件、类型和移交文档的完整导出包。
**输出:** `product-plan/`

---

## 文件结构

```
product/                           # 产品定义 (便携的)
├── product-overview.md            # 产品描述，问题/解决方案，功能特性
├── product-roadmap.md             # 包含标题和描述的板块 (sections) 列表
│
├── data-shape/                    # 产品数据形状
│   └── data-shape.md              # 实体名称、描述和关系
│
├── design-system/                 # 设计标记 (tokens)
│   ├── colors.json                # { primary, secondary, neutral }
│   └── typography.json            # { heading, body, mono }
│
├── shell/                         # 应用外壳
│   └── spec.md                    # 外壳规范 (specification)
│
└── sections/
    └── [section-name]/
        ├── spec.md                # 板块规范
        ├── data.json              # 屏幕设计使用的示例数据
        ├── types.ts               # TypeScript 接口 (interfaces)
        └── *.png                  # 截图

src/
├── shell/                         # 外壳设计组件
│   ├── components/
│   │   ├── AppShell.tsx
│   │   ├── MainNav.tsx
│   │   ├── UserMenu.tsx
│   │   └── index.ts
│   └── ShellPreview.tsx
│
└── sections/
    └── [section-name]/
        ├── components/            # 可导出的组件
        │   ├── [Component].tsx
        │   └── index.ts
        └── [ViewName].tsx         # 预览包装器 (wrapper)

product-plan/                      # 导出包 (生成的)
├── README.md                      # 快速启动指南
├── product-overview.md            # 产品摘要
├── prompts/                       # 现成的编码 Agents 提示词
│   ├── one-shot-prompt.md         # 完整实现的提示词
│   └── section-prompt.md          # 增量实现的提示词模板
├── instructions/                  # 实现说明
│   ├── one-shot-instructions.md   # 合并的所有里程碑
│   └── incremental/               # 逐里程碑的说明
│       ├── 01-shell.md
│       └── [NN]-[section-id].md   # 特定板块的说明
├── design-system/                 # 标记，颜色，字体
├── data-shapes/                   # UI 数据契约 (组件期望的数据类型)
├── shell/                         # 外壳组件
└── sections/                      # 板块组件 (带有 各自的 tests.md)
```

---

## 设计要求

创建屏幕设计时，请遵循以下准则：

- **移动端响应式**: 使用 Tailwind 的响应式前缀 (`sm:`, `md:`, `lg:`, `xl:`) 来确保布局在不同的屏幕尺寸上都能正确适配。

- **亮色和暗色模式**: 给所有颜色使用 `dark:` 变体。测试以确保所有 UI 元素在两种模式下都可见且易读。

- **使用设计标记 (Design Tokens)**: 当产品定义了设计标记时，应用产品的颜色调色板和排版样式。否则，使用 `stone` 作为中性色，使用 `lime` 作为强调色进行回退处理。

- **基于 Props 的组件**: 所有屏幕设计组件必须通过 props 接收数据和回调。绝不要在可导出的组件中直接导入数据（import data）。

- **板块的屏幕设计中没有导航**: 板块（Section）的屏幕界面设计不应包含起导航作用的设计元素（navigation chrome）。应用外壳（shell）将处理所有导航。

---

## Tailwind CSS 规范

这些规则适用于 Design OS 应用程序及其生成的所有屏幕设计/组件：

- **Tailwind CSS v4**: 我们始终使用 Tailwind CSS v4（而不是 v3）。不要引用或创建 v3 版本的代码模式。

- **没有 tailwind.config.js**: Tailwind CSS v4 不使用 `tailwind.config.js` 文件。决不引用、创建或修改它。

- **使用内置功能类**: 避免编写自定义的 CSS。坚持使用 Tailwind 的内置功能类（utility classes）进行所有样式设置。

- **使用内置颜色**: 避免定义自定义颜色。使用 Tailwind 内置的颜色功能类（如 `stone-500`, `lime-400`, `red-600`）。

---

## 四大支柱

Design OS 围绕四个主要领域进行组织：

1. **产品概述 (Product Overview)** — "是什么" 和 "为什么"
   - 产品名称和描述
   - 问题及其解决方案
   - 关键功能
   - 板块 (Sections)/路线图

2. **数据形状 (Data Shape)** — 系统的 "名词"
   - 核心实体的名称和描述
   - 实体之间的概念关系
   - 在各个板块中共享的一致命名词汇表

3. **设计系统 (Design System)** — "外观和体验"
   - 调色板 (Tailwind colors)
   - 排版 (Google Fonts)

4. **应用外壳 (Application Shell)** — 持久的包围框架
   - 全局导航结构
   - 用户菜单
   - 布局模式

加上 **板块 (Sections)** — 单个的功能（features），每个都有 specification，数据，以及屏幕设计。

---

## 设计系统范围

Design OS 将它本身的 UI 跟所设计的产品之间进行了关注点分离：

- **Design OS UI**: 始终使用 stone/lime 调色板和 DM Sans 字体排版
- **产品界面设计**: 使用为产品定义的设计标记（若可行）
- **外壳 (Shell)**: 使用产品设计标记来预览完整的应用体验

---

## 导出与移交

`/export-product` 命令生成了一个 UI 设计移交包：

- **开箱即用的提示词 (prompts)**: 预先编写好的提示词，可直接复制粘贴给编码 agent
  - `one-shot-prompt.md`: 供在单个会话中进行完整实现的提示词
  - `section-prompt.md`: 用于逐个板块（section-by-section）实现的模板
- **实现说明**: 专注于各个里程碑 UI 的指南
  - `product-overview.md`: 总是提供以用于上下文了解
  - `one-shot-instructions.md`: 结合在一起的所有里程碑
  - 增量实现的说明存放于 `instructions/incremental/` 下
- **测试规范**: 每个版块包含带有界面行为规范的 `tests.md`
- **可移植组件**: 基于 props 的统一组件
- **数据结构**: TypeScript 接口，定义了组件期望接收的外观及功能性传参（Props）格式及类型推断等。

这种移交侧重于 UI 设计、产品要求和用户流程。后端架构、数据建模和业务逻辑的决策留给了负责实现的 agent。这些提示词能引导 agent 在开始构建之前询问关于技术栈和需求的澄清性问题。

---

## 设计系统 (Design OS Application)

Design OS 应用程序本身使用一种 "Refined Utility (精致实用)" 的美学体系：

- **排版**: DM Sans 用户标题和正文，IBM Plex Mono 用于代码
- **颜色**: Stone 调色板用于中性色 (暖灰色)，lime 控制强调
- **布局**: 内容宽度最大 800px，充足的留白
- **卡片 (Cards)**: 极简边框 (1px)，微妙的阴影，充足的内边距 padding
- **动效**: 微妙的淡入效果 (200ms)，没有弹跳类动画 (bouncy animations)
