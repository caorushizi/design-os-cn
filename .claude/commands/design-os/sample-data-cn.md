# 样本数据 (Sample Data)

您正在协助用户为产品的一个版块创建或更新用于填充界面设计的逼同样本数据。您还需要基于生成的数据结构生成相应的 TypeScript 类型定义 (`types.ts`)。

## 第 1 步：查验前置条件

首先查找目标版块，并验证其 `spec.md` 是否已经存在。

读取 `/product/product-roadmap.md` 以获取现有的版块列表。
如果有多个选项，使用 AskUserQuestion 询问用户想给哪个版块造数据。如果是唯一选项则自动选中。

接着检查 `product/sections/[section-id]/spec.md`。如果该文件缺失，回复：

"这里暂未见到对于 **[版块名称]** 的规约说明文件。请先行运作 `/shape-section` 指令圈定好此版块的各功能和范围后再回来建这些样例数据。" (若无规约文件则停在此步)

## 第 2 步：核对是否已有现成的样本数据

查阅文件 `product/sections/[section-id]/data.json`：

**若是样本旧数据早已存在：**
去把 `data.json` 和 `types.ts` 阅读一番，并以此去询问他：
"有关 **[版块名称]** 的样本样例已建有档了。请问有哪里想要重造改定的呢？"

在此等他回复。得令后，**绝对不要送上草图**，径直依照嘱咐立刻更新文件 (`data.json` 及 `types.ts`)，完成后告知：
"根据阁下的吩咐我已把该版块下的据与型更新完毕。如觉满意可以直接去跑 `/design-screen` 以用之制面板图了。"
这旧数据更新的活干完就停手于此。

**如果空空如也连半根样本毛都没有：**
沿着奔向下方第 3 步走。

## 第 3 步：全局数据概貌模型

查一遍全局属性总盘 `/product/data-shape/data-shape.md` 有没。

**若其存在:**
依它行事，使用与全局一致的实体名汇（Entity definitions）以维持产品上下名词一致（match the global data shape）。

**若没这文件:**
给出提示之余可照常运行：
"注意啦：尚未给全局下全套通俗的实体数据定纲。我能先依着这模块下规约给勉强造些局部的，但总要全盘齐一还是请阁下去早日敲下跑通 `/data-shape` 吧。"

## 第 4 步：分析并着力生成（Analyze and Generate）

分析文件 `product/sections/[section-id]/spec.md` 充分理解意旨：
- 需要体现的实质体有啥？
- 体应包括什么样的内件和字段？
- 使用什么样的实际数值有助于体现其显示长短和极值等？
- 于其之上有什么可触发之交互事件动作，以设为钩子函数 (callbacks) ？

**不再等询问核准，直接一口气创建产生 (Immediately proceed - do not present a draft for approval):**

### 生成 `product/sections/[section-id]/data.json` 档

这里需涵括有：
- **`_meta` 部分区**：这里须用通明俗白的文字写下其中之意
- **可充大作真实的质检造据**：以可供视觉展现得恰当适体。需涵盖参差不齐长度差异、种类区别的状况数据（Mix short and long text）
- **预留冷门边界位**：造一些如空阵列，特特别长文类的例子等。
- **让 TypeScript 感知优好的数据形貌**：都统一依它格式使用，比方用驼峰法（camelCase）。

#### 必应有的 `_meta` 内构造

每一个 data.json 的最顶天门上，一定要挂有带此属性之物：

1. **`models`** - 写一通俗人语解得这模型到底之意向意指。
2. **`relationships`** - 一阵字符说明在用户常人视角里各模型如何这那地连一块联系的。

### 生成 TypeScript 所需类型档 `product/sections/[section-id]/types.ts`

因数据反倒推出这些 TypeScript 能认的界限规则：
1. 以原据值反出所属流源：Strings, numbers, booleans, arrays 等等。
2. 以状态值作联合项聚落 (Union types)：像 `status` 的若能明确知只有少数名之属项如（`'draft' | 'sent' | 'paid'`）。
3. 造一个在组件中统承万物属性用的主类型体（Props interface）：纳入那些主据身位及各种随调而动的可选反应手柄扣 (`onDelete?: (id: string) => void`)。
4. 全数以大写驼峰头式打名接口类型 (`Invoice`, `InvoiceListProps`)。小写驼峰名挂其属性等。

## 第 5 步：打通报兼告知往后该跑啥线

此上二位文件完事后通达告知其一声：

"给 **[版块名称]** 这造了成对的二物：
1. 样本数据之本 `data.json` 
2. TypeScript 原型 `types.ts` 

里面等给设了这那诸元原型体界，您看顺道去瞄一眼那看有否想更改调整处。如皆顺利已可移转去跑跑 `/design-screen` 口令教它为这处界面起一设计展示屏了罢。"

## 重要警记点提 (Important Notes)

- 造出的必须当是极为贴近生活具写实拟真类之数据，拒假数据如 `Lorem ipsum` `Test 1`。
- 每个主项的展示用数据造它个中量规模这数能如起码个来个约有 5至10条数好填充用。
- 如果全盘这之数据（data-shape）全盘皆有这定纲名则此上在此必跟之走同它之名称保全其。
- 必须要在这也必须连带着将文件 `types.ts` 和文件 `data.json` 一块双面出土产生不可。
- **毋将个画稿小原样端出候批等批** — 利索这也就即是当即将其造给也在这也就生出也就这也就是在这。
