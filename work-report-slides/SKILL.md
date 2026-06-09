---
name: work-report-slides
description: 把工作汇报 MD 内容生成与 Flash HR Q1 季度汇报 index.html 风格 100% 一致的 HTML slide deck。适用场景:用户提供季度/月度/项目工作汇报 markdown 内容,要求生成 HTML slide、幻灯片、演示文稿、PPT;或提到"工作汇报 slide"、"Flash HR 汇报模板"、"季度汇报 HTML"、"把这份 md 做成幻灯片"、"用我的模板做汇报"等。强制设计令牌(暖黄 #FFEA33 + 深底 #0F1115 + Sora/Noto Sans SC),封面与感谢页布局固定,中间页按 7 种内容模板挑选。
---

# work-report-slides

把任意工作汇报 MD 生成与 `工作汇报2/index.html` 风格 **100% 一致**的 HTML slide。

**核心约束**:封面 + 感谢页布局固定;中间页从 7 个模板挑;设计令牌锁死;由你(Claude)做"翻译",不做"重新设计"。

---

## Part 1 · 设计令牌速查(不可改动)

```
主色      #FFEA33    深色  #EAD400    软色  #FFF7B8
暗底      #0F1115    亮底  #F4F5F7    卡片  #FFFFFF
墨        #15171C    板岩  #646B79    弱    #9AA1AD    线 #ECEDF1
字体      Sora(数字/英文)+ Noto Sans SC(中文)
圆角      18px (card) / 99px (tag) / 8px (badge) / 7px (bar-track)
阴影      0 1px 2px rgba(16,18,25,.04) + 0 10px 30px rgba(16,18,25,.05)
动画      cubic-bezier(.22,1,.36,1) 600ms
slide 内边距   56/72/60 px(<1280 降到 40/48/52)
```

字体已在 scaffold.html 通过 Google Fonts 加载,无需额外配置。

---

## Part 2 · 文件清单 & 工作流

```
work-report-slides/
├─ SKILL.md            ← 本文档
├─ scaffold.html       ← 脚手架(head/CSS/JS/sidenav/modal,中间留 SLIDES INSERTION POINT)
├─ slides/
│  ├─ 00-cover.html             ← 封面(固定)
│  ├─ 10-section-overview.html  ← KPI + 横条 + 环形(grid-3)
│  ├─ 20-before-after.html      ← 指标 stack + 前后对比图(grid-ba)
│  ├─ 30-eff-grid.html          ← 双对象效率(grid-2 + 底注)
│  ├─ 40-util-bars.html         ← 指标 stack + 月度分组柱状(grid-util)
│  ├─ 50-quad-line.html         ← 2×2 折线图(+ 底注)
│  ├─ 60-team.html              ← 双栏团队/方法论卡片(grid-2)
│  └─ 99-thanks.html            ← 感谢页(固定)
├─ components.md       ← 原子组件目录(模板未覆盖时拼接用)
└─ examples/sample-input.md     ← 示例 MD(基于 index.html 反推)
```

### 工作流(收到 MD 后逐步执行)

1. **读 MD,理解结构**:抽取标题、汇报人/部门/日期、各章节内容。
2. **拆章节并选模板**:按 Part 3 决策树为每个章节挑模板编号。
3. **组装 deck**:
   - 复制 `scaffold.html`
   - 在 `<!-- SLIDES INSERTION POINT -->` 处按顺序插入 slide,首张必须是 `00-cover.html` 且带 `class="active"`,末张是 `99-thanks.html`
   - 每个 slide 替换全部 `{{TOKEN}}` 占位符
4. **填顶部 chrome 与品牌信息**:除封面/感谢页外,每张 slide 的顶部 `<div class="chrome-top">` 使用相同的品牌行(如 `Flash HR · 季度工作汇报` + 期次)。
5. **检查清单**(详见 Part 4):页码、ID 唯一性、JSON 合法性、资源路径。
6. **输出**:把生成的 HTML 写到**用户当前工作目录**(`pwd` 决定的目录),默认文件名 `report-YYYYQX.html`,与 `assets/` 同级摆放。

---

## Part 3 · 模板决策树(挑哪张)

按以下顺序判断,**先匹配先用**:

| 内容特征 | 选用模板 |
|---|---|
| 必有,deck 第 1 张 | **00-cover** |
| 必有,deck 末张 | **99-thanks** |
| 一个统领大数(如"完成 106 项") + 一组按业务方/类别排序的数量 + 一组优先级/类型占比 | **10-section-overview** |
| 改善幅度指标(请假率、迟到率等) + 优化前/后截图 | **20-before-after** |
| 两个同结构对象横向对比(两地区、两产品、两团队),每个 4 项指标 | **30-eff-grid** |
| 2~3 个核心 KPI + 同对象的月度/期间分项对比(如"普通付款 / 报销 / 总计" 2 月 vs 3 月) | **40-util-bars** |
| 4 个等权重的趋势指标(如金额、驳回率、识别率、准确率)| **50-quad-line** |
| 2 类各 2~3 条文字要点(团队成长 / 方法论 / 经验复盘) | **60-team** |
| 都不符合 | 优先尝试拆分内容到上述模板;若实在不行,用 `components.md` 中的原子组件**组合**,**禁止发明新组件** |

**特殊情况**:
- 一个章节同时有"大数 + 横条 + 占比",但没有占比 → 仍用 **10**,把环形卡换成另一个 barlist 或 metric-card
- 内容比模板槽多 → 精简文字,**不要缩字号**,**不要破环结构**
- 内容比模板槽少 → 留白,**不要塞水填充**

---

## Part 4 · 填空协议 & 风格红线

### 填空协议

- **占位符语法**:`{{TOKEN_NAME}}`,严格替换
- **每张 slide 顶部 HTML 注释**列出该模板全部 TOKEN 和数据属性的含义,**生成时务必删除注释**
- **chrome-top 品牌行**:除封面/感谢页外,所有 slide 用同一句品牌 + 期次
- **chrome-bot 页码**:留 `-- / --`,scaffold.js 会自动填充
- **data-title**:每张 slide 都必须设,内容是该 slide 的短标题(右侧 dot 悬停 tooltip 用)
- **KPI 计数器**:在 KPI 数字 span 上加 `data-counter-to="106"`,初始内容写 `0`
- **图表数据**:用 `data-bars='[...]'` / `data-donut='[...]'` / `data-line='[...]'` JSON 属性承载,JSON 必须合法(用单引号包裹双引号属性,字符串内的双引号需转义或避免使用)
- **ID 唯一性**:donut/lineChart 的 ID 全 deck 内不能重名
- **`<span class="hl">关键词</span>`**:章节标题里关键词加黄色高亮底
- **`<b>关键词</b>`**:文字内强调用 `<b>`,会自动加高亮(team-card 的 .b 内、eff-foot 内)

### 风格红线(违反即不合格)

| 不要 | 应该 |
|---|---|
| 改 CSS `:root` 变量 | 保持 scaffold.html 不动 |
| 引入新字体 | 只用 Sora + Noto Sans SC |
| 用其他颜色当主色 | 强调一律用 `#FFEA33` 黄 |
| 用 emoji 替代图标 | 原本就有的 ✓/⇄/🔔 保留;不新增 |
| 替换或重画 Flash Logo | 只用 `assets/logo-Black.png`(浅底)和 `assets/logo-White.png`(深底) |
| 改卡片圆角、阴影、内边距 | 用既有 class,不写自定义 style 覆盖尺寸 |
| 内容溢出时缩字号 | 精简文字,或拆分到两个 slide |
| 把 9 张 slide 强行塞够 | 实际 6~7 张也可,模板按需挑 |
| 发明新 slide 类型 | 用 `components.md` 拼,或换个模板 |

---

## Part 5 · 资源约定 & 输出位置

### Logo & 品牌(强制)

**永远使用 Flash Express 官方 Logo**,不要替换、不要新画、不要用其他品牌符号:

- **深色背景(`bg-dark`,即封面 + 感谢页)**:使用 `assets/logo-White.png`(白色 FLASH 字 + 黄闪电镂空版)
- **浅色背景**:若需在浅色 slide 加 logo,使用 `assets/logo-Black.png`(黑色 FLASH 字 + 黄闪电 + EXPRESS 副标)
- **内容页 chrome-top 默认不放图标 logo**,只用文字 brand 行(`<span class="dot"></span>Flash HR · 季度工作汇报`),与 index.html 保持一致

两个 Logo 文件应始终存在于 `<生成目录>/assets/` 中。若用户的工作目录没有 `assets/logo-White.png` 与 `assets/logo-Black.png`,**主动提醒用户从 `工作汇报2/assets/` 复制过来**,不要尝试用文字、其他图片或 emoji 替代 logo。

### 其他图片资源

- 模板里所有图片用相对路径:`assets/before.png` / `assets/after.png` / 其它本期图片
- 生成的 HTML 必须与 `assets/` 同级摆放
- 若用户没指定输出目录,默认放到当前工作目录(通常是 `工作汇报2/` 或新建的 `工作汇报-YYYYQX/`)
- 若用户提供了新的 before/after 截图,自动放到 `assets/` 目录;若没提供且 MD 内容确需对比,**问用户要图**,不要捏造

### 输出文件名

- 默认 `report-YYYYQX.html`(如 `report-2026Q2.html`)
- 若用户指定文件名,按用户指定
- 若用户要求覆盖既有 `index.html`,**先确认**

---

## Part 6 · 速查清单(生成前过一遍)

- [ ] 首张 slide 是 `00-cover.html`,带 `class="active"`
- [ ] 末张 slide 是 `99-thanks.html`
- [ ] 每张 slide 设了 `data-title`
- [ ] 每张内容 slide 顶部品牌行用同一句话
- [ ] 所有 `{{TOKEN}}` 已替换,无残留
- [ ] 所有 HTML 注释(模板顶部的说明)已删除
- [ ] 所有 `data-bars`/`data-donut`/`data-line` JSON 合法
- [ ] donut 与 lineChart 的 ID 全 deck 内唯一
- [ ] chrome-bot 页码用 `-- / --` 占位
- [ ] KPI 数字 span 带 `data-counter-to`,初始内容 `0`
- [ ] `<title>` 已替换为本期汇报标题
- [ ] `assets/` 路径正确

完成后,引导用户双击 HTML 打开浏览器查看,提示快捷键:← / → / Space 翻页,Home/End 跳首末,Esc 关 Modal。
