# 导出产品 (Export Product)

您正在协助用户将其构思完备的产品界面设计打包装配成可供开发实现的代码交接包。这将会汇聚所有用以将界面设计融合集成入实际业务库所必需的文件。

## 第 1 步：查实核对准入的前提条件

校验最基础、必不可少的条件：

**绝必不可缺 (Required):**
- `/product/product-overview.md` — 产品总体概览
- `/product/product-roadmap.md` — 蓝图板块分列
- 在 `src/sections/[section-id]/` 目录下至少存在一个界面设计组件

**强烈建议要有 (没这会报警告但仍可继续):**
- `/product/data-shape/data-shape.md` — 实体属性面貌
- `/product/design-system/colors.json` — 颜色标记设定
- `/product/design-system/typography.json` — 字体规范统列
- `src/shell/components/AppShell.tsx` — 统大外层的这之大壳衣

如果缺少必备项就告知一声，并停在此步。

如果缺少的是推荐项，报出警告后仍可继续进行：
"提醒：有些建议具有的文件缺失（如实体定义、设计标识或外壳设计等）。您可以无妨继续接着造，但是补齐这些会让移交更完全。"

## 第 2 步：征集用于导出的信息材料

通读并收集下列资源文件：

1. `/product/product-overview.md` 
2. `/product/product-roadmap.md` 
3. `/product/data-shape/data-shape.md` (倘若有)
4. `/product/design-system/colors.json` (倘若有)
5. `/product/design-system/typography.json` (倘若有)
6. `/product/shell/spec.md` (倘若有)
7. 各个板块内的： `spec.md`, `data.json`, `types.ts`
8. 所有在 `src/sections/` 和 `src/shell/` 内界面的相关代码

## 第 3 步：建立打包输出的目录架构

创立一个 `product-plan/` 大文件夹，并在其下建立如下构造：

```
product-plan/
├── README.md                    # 开箱即能用之指南
├── product-overview.md          # 也是产品总览
│
├── prompts/                     # 代码 Agent 即拿即用的提示词
│   ├── one-shot-prompt.md       # 一口气成就完的提示词
│   └── section-prompt.md        # 分块分步的提示词
│
├── instructions/                # 步骤操作指导
│   ├── one-shot-instructions.md # 各阶段目标合一
│   └── incremental/             # 拆分为逐步的阶段
│       ├── 01-shell.md
│       ├── 02-[头一个首版块].md
│       └── ...
│
├── design-system/               # 设计系统与规则标志
│   ├── tokens.css
│   ├── tailwind-colors.md
│   └── fonts.md
│
├── data-shapes/                 # 接口契约界面数据
│   ├── README.md
│   └── overview.ts
│
├── shell/                       # 系统大包裹外衣
│   ├── README.md
│   ├── components/
│   │   ├── ...
│   └── screenshot.png
│
└── sections/                    # 各个大板块分区
    └── [section-id]/
        ├── README.md
        ├── tests.md               # 用来做UI行为模式的测试用例文件
        ├── components/
        │   ├── [Component].tsx
        │   └── index.ts
        ├── types.ts
        ├── sample-data.json
        └── screenshot.png
```

## 第 4 步至第 11 步

将上述各文件的具体内容生成相应的 Markdown、Typescript、JSON 和 CSS 代码写入该目录。由于内容极多，执行要求是将那些模板内容做精确的文本替换并落盘，不要草图核准。具体生成包括指示说明文档、设计资产说明、数据结构约定等。各指导文档内容要求：

1. `product-overview.md`: 涵盖总体描述、设计的页面块及实体说明。
2. `instruction/*`: 分步给实现 Agent 提供具体的行为指导说明，包括要开发的范围、接口传入、预期的用户交互流程、空白态应对等，每完成一个 Milestone 都需满足测试规则。
3. `tokens.css` 及 `fonts.md` 等设计规则: 将先前确定的主色次色和字体，转换成对应的样式表或者配置文件。
4. `data-shapes/overview.ts`: 汇总所有模型数据供实现层做接口。
5. 将在 `src/sections` 和 `src/shell` 下的原本 React 组件代码原封不动搬运至 `product-plan` 中，但要将其中的相对引入路径改写修正以适配独立导出环境。

完成所有代码拷贝和组装后，通告用户产品导出完成。
