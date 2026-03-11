# 捕获屏幕设计截图 (Screenshot Design)

您正在协助用户捕获他们刚刚创建的屏幕界面设计的截图。该截图将保存在产品对应的板块文件夹中，以作为配套的设计留底文档。

## 资格核验：查探 Playwright MCP 现存与否

在开启截图进程前，先验证有没有可用的 Playwright MCP 工具。去找找名为 `browser_take_screenshot` 或者是 `mcp__playwright__browser_take_screenshot` 的工具。

如果 Playwright MCP 服务并未就位，请如实原样一字不落地将如下段落呈现给用户看（请勿做任何自作聪明的拼写“修正”）：

***
To capture screenshots, I need the Playwright MCP server installed. Please run:

```
claude mcp add playwright npx @playwright/mcp@latest
```

Then restart this Claude Code session and run `/screenshot-design` again.
***

切记原样输出，不要替换什么包名或者修改指令。
如果没有 Playwright MCP，后续的所有操作请立即停止！

## 第 1 步：辨认出是要抓取哪张面板

首先摸透这截图的对象是谁。

查阅 `/product/product-roadmap.md` 以获知所有存在的分块，再去看看 `src/sections/` 目录下现有存活的屏幕设计都有哪些。

若全局只存有一个，则自动选取。倘若其数量众多，便使用 AskUserQuestion 工具去问询：

"请问今天您意欲为哪一副屏幕设计留下截图留影呢？"

随手把名单亮上出来让其选：
- [板块名称] / [屏幕图形名]

## 第 2 步：起动本地的开发服务端

自行以 Bash 起跑开发服务：运行 `npm run dev`（后台跑着，即可不用堵死接下来的流程）。

**切勿** 询问用户是否开启了服务，或者去指使他们开。这必须由您自己来操作！
等服务器启动就位后，稍微等上那么几秒钟，然后再去请求屏幕设计图的 URL。

## 第 3 步：截走留下取图留影

差遣 Playwright MCP 去导航到具体网页并截图。
页面的固用网址为： `http://localhost:3000/sections/[section-id]/screen-designs/[screen-design-name]`

1. 先让 `browser_navigate` 导航前往此 URL
2. 等待直到页面完整地承托载毕
3. **点击在导航栏上的那颗“隐藏（Hide）”按键**，在拍照前收隐导航条。（它带有特征属性 `data-hide-header` 以供定位）
4. 以 `browser_take_screenshot` 扣下那无导航拦的网页全景。

**取照规矩 (Screenshot specifications):**
- 于电脑端的全框画幅中摄录 (推荐宽定值为 1280px)。
- 务求要用 **带向下展开的全长页面（full page screenshot）** 去兜录长轴卷页面的风光 (而不仅限眼前的视口口子 viewport）。
- 以保最佳质感的 PNG 档保存之。

所以，在发派 `browser_take_screenshot` 时紧记带上 `fullPage: true`。

## 第 4 步：存收此图

Playwright MCP 有个固疾：只会抛存档案于其自家庭院之下 (`.playwright-mcp/`)。你得听之任之后，再搬之入库。

1. **第一手**，传召 `browser_take_screenshot` 时就附一单纯的单名（无附带路径）：
   - 使诸如 `dashboard.png` 或 `invoice-list.png` 之流。
   - 文件便落存于 `.playwright-mcp/[文件单名].png`。

2. **之后一招**，呼唤大 Bash 去其巢下搬运将图挪送于产品的属地内：
   ```bash
   cp .playwright-mcp/[此文件名].png product/sections/[section-id]/[此文件名].png
   ```

**文件命名俗例:** `[面像名号]-[属性别变类].png`

示例如下：
- `invoice-list.png` (主面图)
- `invoice-list-dark.png` (夜间暗版变体图)
- `invoice-detail.png`

假如户主白黑都兼爱求需的，请一一照抓满足其想。

## 第 5 步：收工奏报通明

将消息明禀：

"我已把抓截好的图形留档存放妥于 `product/sections/[section-id]/[您的文件名].png`。

此相截图内，为那划属在 **[归属版名标题]** 区块下的 **[面容大名]** 之显示设计界面大观。"

如对方兴起意犹未尽仍有祈求别类图面（夜深版，他变等）：

"有劳阁下发旨，是否还须在下去抓它个不一样的相来？比方讲：
- 暗寂夜行的变体
- 手游移端等缩口屏
- 另别种种态向（空、读候等等它态）"

## 要点警之 (Important Notes)

- 务由你亲自立转出开发服务端 - 切不用那使唤调遣口令用户
- 图本伴着那 `spec.md` 及 `data.json` 并肩置入 `product/sections/[section-id]/` 同宿之下
- 所付命之相片书名需顾名便知何门何物所录何等品相（暗版或是缩微版等）
- 出图的定宽应齐头一致好叫通看规整和谐
- 总别忘 `fullPage` 以兜括天下万物收尽滚轮长卷底
- 忙歇手头事，大可将其跑开的服务进程抽板停刀赐死关除也可
