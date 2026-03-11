# 产品规划 (Product Planning)

Design OS 的第一阶段为您产品奠定基础。在设计任何界面屏幕之前，请先完成这些步骤。

## 1. 产品愿景 (Product Vision)

```
/product-vision
```

在一次连贯的对话流程中定义您产品的核心身份定位。您将确立：

- **产品名称** — 一个清晰，容易记住的名字
- **产品描述** — 用 1-3 句话抓住产品精华
- **问题与解决方案** — 您正着眼解决什么痛点及其解决之道
- **核心功能特性** — 使这一切成为可能的主要功能
- **路线图板块 (Roadmap sections)** — 产品中 3-5 个独立的功能区域
- **数据外形关联 (Data shape)** — 核心的实体要素（动静“名词”们）及他们相互间的关系

分享您的任何笔记、想法或对于您要构建内容的初步构思。AI 将提出澄清问题，涵盖您的愿景、产品的主要区域以及用户将要处理的核心内容。一旦它获得了足够的信息，它会自动编写这下面这三个文件。

**创建了：**
- `product/product-overview.md` — 产品描述，问题/解决方案，功能特性
- `product/product-roadmap.md` — 按照开发优先级排序的 3-5 个版块 (sections)
- `product/data-shape/data-shape.md` — 核心实体及他们间的关系

**如果稍后要单独更新修改：**
- `/product-roadmap` — 增加、移除或重新排序调整这些板块
- `/data-shape` — 增加、移除或更新实体和相关联系

## 2. 设计系统标识标记 (Design Tokens)

```
/design-tokens
```

选择出产品的视觉特征：

### 颜色体系

从 Tailwind 内置色板中挑选颜色：

- **主要色 (Primary)** — 用于按钮、链接、关键性操作的主导色彩（如 `blue`, `indigo`, `emerald`, `lime`）
- **次要色 (Secondary)** — 诸如用于标签页及内容的高亮衬托辅助色彩（如 `violet`, `amber`, `teal`）
- **中性色 (Neutral)** — 供背景板、一般的文本字体，边框所使用（如 `slate`, `gray`, `zinc`, `stone`）

### 字体排印学 (Typography)

挑选自 Google Fonts:

- **标头题字 (Heading)** — 各种级别的大小标题等展示用字体（如 `DM Sans`, `Inter`, `Space Grotesk`）
- **常规正文 (Body)** — 涵盖诸段落文本及一般的界面用户文字（如 `Inter`, `Source Sans 3`, `Nunito Sans`）
- **等宽代码 (Mono)** — 使用在呈现场景如代码段以及专门属性术语场景里（如 `JetBrains Mono`, `Fira Code`）

该 AI 助手依据您的这项目工程属类，给您送去参选建议并帮腔出主意以助找见合您产品的一套视觉排印样式。

**创建了：** `product/design-system/colors.json`, `product/design-system/typography.json`

## 3.  程序套壳的构筑定版 (Application Shell)

```
/design-shell
```

设计布局那套将一直存在伴随着且打包容括了其余各个细节分块区域的总界面导航布局外衣包裹。可从如下普遍常用的经典排板样式作出决断采信：

- **侧边栏导航 (Sidebar Navigation)** — 在界面左侧垂直铺开罗列了菜单，内容放在了右边展现。通常使用在后台管理面版控制台，亦或拥有繁多分门别类的体系巨细无遗功能众多的应用里。
- **顶栏导航 (Top Navigation)** — 横向上排开于首顶部，往下内容依次排版顺延展示。常常对于偏单页或简约工具亦或带些营销宣传意味或者板块稀少的工程很受用有效。
- **极简顶眉 (Minimal Header)** — 只摆着产品的品牌名称 LOGO 及客户自身操作相关的选调清单（用户菜单）。这很能集中用在有着向导般固定式步骤流转或是干单一业务勾当之场合。

另外还需要您决定这一些方面的安排：

- 该用啥子办法展示置放跟“客户本人相关操作管理”功能等列表 (User menu) 
- 各种移动通讯端的设备自适应缩放（responsive behavior）逻辑
- 是否仍有额外补充余地添加其它的跳转项，如设立个前往专门设定面板，查阅提供疑难解答援助等链接。

“壳体”是用可执行可看能操作的那些 React 组件代码给落实实现表达出来供包裹裹挟您其余板块的容器。

**创建了：**
- `product/shell/spec.md` — 壳配置的参数要求细节定义
- `src/shell/components/AppShell.tsx` — 总壳结构的渲染入口组件
- `src/shell/components/MainNav.tsx` — 引航导航控件
- `src/shell/components/UserMenu.tsx` — 有关使用者自身的选项设定菜单类
- `src/shell/ShellPreview.tsx` — 提供给本宿主内去实现画面效果核看预审效果用的模块代码

## 下阶段要做的

凭借基底已被筑立稳固的基础，终于具备了充足条件对各个个体板块开始逐个界面操刀构思。阅读 [设计板块细节 (Designing Sections)](design-section.md) 进行下一段步骤任务。
