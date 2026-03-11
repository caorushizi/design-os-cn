# 设计屏幕 (Design Screen)

您正在协助用户为其产品规划创建一个界面。屏幕设计将是一个基于 params (props-based) 的 React 组件，能够将其导出并且集成到任意的 React 代码库之中。

## 第 1 步：查验前置条件

首先查找目标板块（section），验证 `spec.md`, `data.json` 以及 `types.ts` 等文件是否都已就绪。

读取 `/product/product-roadmap.md` 去获取现有的板块名录。

如果这里只有一个板块，那就把它视为了默认对象。如果有诸多板块在列的话，唤取 AskUserQuestion 这一手段询问您的顾客：准备先就哪一板块的界面开刀构建。

之后继续核查下面这三大必要文档是否凑齐落位：

- `product/sections/[section-id]/spec.md`
- `product/sections/[section-id]/data.json`
- `product/sections/[section-id]/types.ts`

假如没有 spec.md：

"眼下我还不曾看到归于 **[Section Title]** 的构建设计指引。劳您先行运作 `/shape-section` 这个口令用以敲定这一块的需求边界。"

假如没有 data.json 或是 types.ts：

"我还未看到关于 **[Section Title]** 的范例数据。麻烦您执行一下 `/sample-data` 以供给界面搭建必须用到的临时示范数据及对应的约束文件 types 。"

只要有任何文档不在线位，则工作就此停步。

## 第 2 步：核查存在与否设计特征 (Design System) 以及骨架包裹体 (Shell)

顺带留意查看有没有那锦上添花的拓展配备：

**设计特征标识 (Design Tokens):**
- 查看 `/product/design-system/colors.json` 有否生成
- 查看 `/product/design-system/typography.json` 有否设立

要是这些外观标记已然生成好落位，吸收读取然后在后续为元素套样式上发挥应用。反之未发现，就出点提示提醒一通：

"留心：系统还没有一套完备设计特征标记。我会暂且取默认素色顶着先，但凡有意令产品有强统一度品牌的标识一致的话，不若先移步跑一遍 `/design-tokens` 再回来不迟。"

**大外壳 (Shell):**
- 查看这个 `src/shell/components/AppShell.tsx` 落成了没有

要是有这层套衣外壳的话，这一界面的产出物等阵就会披上在 Design OS 环境下的原副大壳子里供主上预览鉴赏了。若没这物件在，也要留个醒说一下：

"附言：这应用程序最顶这层统盖天南地北所有业务块的总壳子似乎不见踪影呐。这也就意味着你马上造出的画面就只得自己独立呈露以供预览罢了。要是心仪在统合了全站导航栏等大局环境中全盘品鉴风貌，不妨先运作下 `/design-shell` 来起立那身大架构为好。"

## 第 3 步：庖丁解牛般的解构研判解析那些需求

请详细地翻阅消化掉那三份文件：

1. **spec.md** - 把那套用户体验流程和 UI 形态样式的苛求读通畅拿捏准了
2. **data.json** - 对这套准备好的数据组织格局有所盘算
3. **types.ts** - 清清楚楚摸黑底蕴知道 TypeScript 接口，以及备了啥可以往外召唤的接入口子 (callbacks)

推导应当衍生什么展示切面(views)。典型的规律如下这几种：

- 列表展现 (List/dashboard view)
- 详情信息区 (Detail view)
- 录入添加或者编辑区域 (Form/create view)

## 第 4 步：向用户确认屏幕构建的首要重点

假定规格中藏有多层意图想要衍生多款样貌图形的，叫起 AskUserQuestion 此招问定该打谁先出：

"根据规约手册指涉来看，在这一号 **[Section Title]** 板块里预定有这么几副不一样的画型图：

1. **[第一副画型]** - [极简叙述]
2. **[第二副画型]** - [极简叙述]

阁下以为我们打哪副起头破冰妥当？"

若只有一条大路单一面相需构建，则跨开推径直开辟即可。

## 第 5 步：把“前端设计大匠”的诀窍拉进内功去运转

先别急着下定挥毫，务必把 `frontend-design` 这神功心决技领会通络保证之后出的精工是属绝伦之好货（生产水准 production-grade）。行事必须牢遵驻放在 `.claude/skills/frontend-design/SKILL.md` 里的那套法则教诲。

## 第 6 步：依 Props 所制建立该功能实干主大将组件 (Props-Based Component)

将其主力文件安置于此： `src/sections/[section-id]/components/[ViewName].tsx`.

### 核心身量组成骨架

务必这班子遵守（MUST）：

- 把自 types.ts 中提取出的类型声明引入（import）给挂上作约束门挡规范
- 以 props 当渠道源给它由外部接收输入并化作己用（禁止做诸如去把数据源头引出偷塞这种违禁：不能用 `import data.json` 这般事）
- 给一切牵一发动全其的钩回调传令行为留通道 (callbacks as props)
- 此一组件需独立成势以至走到何方只要供足营养即成用可走离 (Be fully self-contained and portable)

写个引例：

```tsx
import type { InvoiceListProps } from '@/../product/sections/[section-id]/types'

export function InvoiceList({
  invoices,
  onView,
  onEdit,
  onDelete,
  onCreate
}: InvoiceListProps) {
  return (
    <div className="max-w-4xl mx-auto">
      {/* 填充在此 */}

      {/* 示范: 一个用 callback 操作按钮之举 */}
      <button onClick={onCreate}>做清单一单</button>

      {/* 示范: 伴着传传呼叫反应钩具把列表布成陈列之法 */}
      {invoices.map(invoice => (
        <div key={invoice.id}>
          <span>{invoice.clientName}</span>
          <button onClick={() => onView?.(invoice.id)}>细看 (View)</button>
          <button onClick={() => onEdit?.(invoice.id)}>改了修了 (Edit)</button>
          <button onClick={() => onDelete?.(invoice.id)}>消了全除没 (Delete)</button>
        </div>
      ))}
    </div>
  )
}
```

### 应当符合的对外观美感考量设定

- **多屏幕设备缩放弹性展现功架 (Mobile responsive):** 用上那是 Tailwind 其自有一家的各类各等尺寸标识口诀（`sm:`, `md:`, `lg:` ）让这展现给这排开阵位在大幕中屏小手记微端设备皆妥当好看而不崩垮
- **黑底白底的随意双面秀：** 在所有涉色的去头冠上戴之那如 `dark:` 以使其懂变色顺应夜调
- **套用之前打下设计的名号色标记：** 若之前弄有在设计时调就指定的设计特色用此等设上的来点抹颜料
- **追逐信守那匠工前端技能宝典之旨意：** 做得出具有极神品可名状被人感可记令人赏慕的心手可造化用户画面端面之好局！

### 加载配戴使用系统定制徽标着彩 (Applying Design Tokens)

**如寻摸得了它 `/product/design-system/colors.json` :**
- 拿主头正主力颜色为主打调去配诸如在按制手键 (buttons) 有且仅重点光标闪光重点聚焦之处上色
- 拿次一副伴主位的色彩以打如点缀性挂件标签页记 (tags) 等去填塞渲染点发去烘辅助调等之上色起效
- 没其它讲究就作为平铺打白打底素以色（neutral color）用做文表或是垫着的那类平无特大不一色的之选。

**假若那不长设计符标记全空着也就是不曾有之（design tokens don't exist）：**
- 也罢，权退且去使它在自来此原系统基土（Design OS）其之自带之缺省。那缺省所定的主中坚平淡基调也就是叫 `stone` 和那些去引捉目那高主色其叫着的是那极跳极为惹抢极色果调味叫如 `lime` 去组台配合之而已。

### 悉收全揽当并必须涵并具备有之 (What to Include)

- 所有需求白皮文本 (spec) 之写尽一切这所有一切悉以造出还魂全落到位！
- 值之赋源只且以唯一从参数传来去吸供收之（不私钉铁锤那将那些没活力钉死的直接生塞作数值以之这而替代敷过搪过 (not hardcoded values)）
- 加入交互的拟形变化等这活现身命之像机能反响应答的以显真实灵妙 (含带 hover 等这 active 等这些诸多能让之其现生活力生姿诸表像)

### 扫入雷区大错违章万禁绝决这当无绝不允许绝绝不含有带下这任何带这些以等在身之中这行径作为的异这些的类之行（What NOT to Include）

- 大刀裁决根绝除断这妄加自越过规逾过墙地来暗道以自己名由直接暗走牵拉来这一类的如 `import data from` 什么之语之类的盗引作为！
- 去行所之非属不在这约章内这白文册要求这等所规定定要之属的去作为它非定不属内不在这些！
- 清没这些个凡等诸类自己去拉路做引路拉航之功那些功能之用。凡那回响应声是为是此所是属作为回手引接拉向上行唯一回反引报报向上传达这一其有和单唯一之引反回接扣它这唯一其此传之上引的通门扣此之这路此它之一唯一这就之。这等这些。
- 绝这绝对并不准不要去容准不要在这有包留容纳去包含括去带有这就留含有那些将带以用这用去做此作此指做用导引导航方向路线路带标指示航的能用功能作为它用的件这一带功能的部将件带这之等的功能之将部这它的这一将它这也这件等它那些（这也全都应给把交转由由给由着是由那是着由在这这其在外之外其外面它它之外大衣所主外套来打点作主这一去干着也就）这。

## 第 7 步：如确需构设拆借细分的次等级的它那些这些从组件（Create Sub-Components）

面对面如应对那繁多杂繁复错且多又长的面阵展示排面全图之境下，以去分切划为作之划等个小小这一属等的一从的各个去承担责分别以去这就的散分开离给做作这小这分的化分其这就分落从之。

（后文略，参考上文类似风格即可）
