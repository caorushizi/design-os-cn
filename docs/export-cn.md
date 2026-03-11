# 导出 (Export)

当您的设计完成后，导出所有实现 Agent （或团队）构建产品所需的内容。

## 何时导出

当满足以下条件时，就可以导出了：

- 已定义好产品愿景和路线图
- 至少有一个板块包含了屏幕设计
- 您对设计方向感到满意

您可以在任何时候进行导出 —— 它不一定非得是“完成的（complete）”。导出将生成您当前设计的一份快照（snapshot）。随着您添加更多板块，您之后随时可以再次导出。

## 运行导出操作

```
/export-product
```

该导出命令会：

1. **检查前置条件** — 验证所需文件是否存在
2. **收集所有设计资产** — 组件，类型定义(types)，数据，标记(tokens)
3. **生成实现说明** — 包含现成的提示词（prompts）
4. **生成测试说明** — 每个板块的 TDD 规范
5. **创建导出打包** — 一个完整的 `product-plan/` 目录
6. **创建一个 zip 压缩文件** — `product-plan.zip` 以方便下载

## 包含哪些内容

### 现成的提示词 (Prompts)

```
product-plan/prompts/
├── one-shot-prompt.md     # 供完整实现一次生成的提示词
└── section-prompt.md      # 用于逐个板块按步骤实现的提示词模板
```

这些是预先编写好的提示词，您可以将它们复制/粘贴到您的编码 agent中。它们引用了各个说明文件（instructions），并指导您的 Agent 在实现之前审查设计并提出澄清问题。

### 说明文档 (Instructions)

```
product-plan/
├── product-overview.md              # 产品摘要（始终要被提供以此获取上下文）
└── instructions/
    ├── one-shot-instructions.md     # 合并在一起的所有里程碑说明
    └── incremental/                 # 逐里程碑的增量实现说明
        ├── 01-shell.md              # 设计标记 (tokens) + 应用外壳 (shell)
        ├── 02-[section-id].md        # 每个板块一个（例如：02-invoices.md）
        └── ...
```

**product-overview.md** 提供了有关完整产品的上下文信息 — 实现在任何一个会话时始终都要包含它。

**one-shot-instructions.md** 将所有的里程碑合并到一个文档中。当需要进行完整实现的构建时，可以将其和 `one-shot-prompt.md` 一起使用。

**增量说明 (Incremental instructions)** 将工作分解成各个里程碑。使用这些配合 `section-prompt.md` 进行逐步的实现。

### 设计系统 (Design System)

```
product-plan/design-system/
├── tokens.css           # CSS 自定义属性 (custom properties)
├── tailwind-colors.md   # Tailwind 配置指南
└── fonts.md             # 谷歌字体 (Google Fonts) 设置
```

### 数据形状 (Data Shapes)

```
product-plan/data-shapes/
├── README.md            # UI 数据的契约概述
└── overview.ts          # 结合了的统一的类型参考（应用于所有板块）
```

### 外壳组件 (Shell Components)

```
product-plan/shell/
├── README.md            # 设计意图
├── components/
│   ├── AppShell.tsx     # 布局的主外部包装器
│   ├── MainNav.tsx      # 导航栏
│   ├── UserMenu.tsx     # 用户菜单
│   └── index.ts         # Exports 导出相关
└── screenshot.png       # 视觉上的展示参考（如果截了图的话）
```

### 板块组件 (Section Components)

对于每个版块：

```
product-plan/sections/[section-id]/
├── README.md            # 功能概述，用户使用流程 (user flows)
├── tests.md             # 用户界面 UI 行为测试规范
├── components/
│   ├── [Component].tsx  # 将要是能够导出的被外界读取利用的代码组件
│   └── index.ts         # Exports 导出
├── types.ts             # 描述了参数的数据类型 TypeScript Interfaces
├── sample-data.json     # 测试样板数据
└── screenshot.png       # 截图视觉参考 (如果有截图生成)
```

### 测试说明 (Test Instructions)

每个板块都有一个包含有和具体工具框架解耦的、无关型的测试规范的编写导引：`tests.md`。

- **用户操作流程 (User flow tests)** —— 核心用例所涉及交互状态之正例成功、反例失败时的测试分支
- **空窗态及加载中 (Empty state tests)** —— 查无相关数据记录列表下的用户体验测试等
- **前端子控件交互测试 (Component interaction tests)** —— 针对需要用户鼠标或键盘点击敲击所交互反应行为的元素

这些指导阐述了去验证“去测验验证的是发生发生了怎样的表象结果”（WHAT），而不是怎么在代码实现方面用底层级语言编写逻辑测试“怎样去做（HOW），因而你那充当软件研发职能工程师角色的 Agent，即可很便利的将其调配转译成属于你们业务项目的例如像 Jest, Vitest, Playwright, Cypress, RSpec, Minitest, PHPUnit 等各类库、各式自动化系统下兼容通过的标准流程。

## 关于组件的一些细节 (About the Components)

导出的这些组件是具有如此属性特质的：

- **借助于属性 Props 控制** —— 所传入下发承接的数据来源和诸如按钮点击那般触发回调操作动作皆依托 Props 的规范完成传递，而绝不对外有侵入的内部进行诸如此类硬编码地 `import` API 数据引入调用
- **高度轻量灵活易兼容可配置可携迁移 (Portable)** —— 在任一种符合 React 开发约束的方案库内都即插即用，也不对本 Design OS 有所谓寄生性质的依赖牵绊
- **成品度极高 (Complete)** —— 完全精良装满 CSS 外观，符合自适应各种屏幕大小终端且兼容黑暗显示模式等特长
- **可用在实际工程当中 (Production-ready)** —— 他们并非原型交互动画演示（prototypes），更不仅是模拟假想视觉设计（mockups）

```tsx
// Components 即接受到外部送达喂给到它的业务所需资料（data），也能向外发出信号事件（callbacks作为props的一分子传进）
<InvoiceList
  invoices={data}
  onView={(id) => navigate(`/invoices/${id}`)}
  onEdit={(id) => navigate(`/invoices/${id}/edit`)}
  onDelete={(id) => confirmDelete(id)}
  onCreate={() => navigate('/invoices/new')}
/>
```

您负责最终开发代码工程实现的智能体助手它的使命目标工作内容即是：
- 替以上的调用触发的回传动作函数去牵线接上属于实现项目中具备着转场路由与后端发起后台接口申请沟通联系等真正执行业务代码行为职责的功能系统方案
- 去拿您服务端后端真实提供输送着的内容替换填充取代掉原来的那些作演示样板示范的数据材料
- 让原本简单的在失败或是仍在缓冲尚未完成等待时候应该处理的报错反馈显示和加载过度动画处理得当周密规范起来
- 像诸如新注册账号第一次打开或者系统所有信息皆已删空（即无历史业务沉淀痕迹）处于零初始信息量局面下应该为客户显示怎样良好舒适的空状态情况处理界面等等都补充完整
- 在底层构造建立搭建好为这整个全套UI能够运行而匹配必须提供的数据来源后端服务端接口
- 遵循前文提及给到你那套测验行为要求手册去编写真正的在研发项目运行期间时刻起保障约束维护意义的软件工程测试（TDD方法）

## 怎样使用这份生成的导出结果 (Using the Export)

请查阅：[代码实现 (Codebase Implementation)](codebase-implementation.md)，从而得到更加深刻全面的如何在你们现实真切的落地项目系统中投入这套产出设计代码的操作建议。
