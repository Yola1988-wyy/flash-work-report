# 原子组件目录

> 当 7 个 slide 模板不能完美匹配 MD 内容时,从这里挑组件**组合**进一个自定义 slide。**禁止发明新组件**。
> 所有组件已在 scaffold.html 的 CSS 中定义好,直接使用 class 即可。

## 通用 slide 骨架

```html
<section class="slide bg-light" data-title="短标题">
  <div class="slide-inner">
    <div class="chrome-top">
      <div class="brand"><span class="dot"></span>Flash HR · 季度工作汇报</div>
      <div>2026 第一季度</div>
    </div>

    <!-- shead -->
    <!-- 内容主体(用 grid-2/grid-3/grid-ba/grid-util 之一,或自定义 display:grid) -->

    <div class="chrome-bot"><span></span><span>-- / --</span></div>
  </div>
</section>
```

深色背景版:把 `bg-light` 换成 `bg-dark`,顶部用 `<img src="assets/logo-White.png" class="logo logo-dark">` 替代品牌行。

---

## 章节头(shead)

```html
<div class="shead">
  <div class="accent"></div>
  <div>
    <div class="snum au">06</div>
    <h2 class="au">职能部门支持:<span class="hl">油费虚假报销增强校验</span></h2>
    <div class="kicker au">副标题/统计区间/补充说明</div>
  </div>
  <div class="tags au1">
    <span class="tag solid">提效</span>
    <span class="tag solid">优化体验</span>
  </div>
</div>
```

- `.accent`:左侧黄色竖条
- `.snum`:章节编号("01"/"02"/...)
- `<span class="hl">`:标题里的高亮关键词(黄色背景)
- `.kicker`:副标题
- `.tag.solid`:实心黄色标签;`.tag`(无 solid):空心标签

---

## KPI 卡(单数据巨号)

```html
<div class="card kpi">
  <div class="kpi-accent"></div>
  <div class="num">
    <span data-counter-to="106">0</span>
    <span class="up">↑</span>
  </div>
  <div class="lbl">完成上线总数</div>
  <div class="note">辅助说明,1~2 行短句。</div>
</div>
```

- `data-counter-to`:数字目标,scaffold.js 会自动跑 0→target 动画
- `.up`:可选,黄色上箭头表示正向

---

## metric-card(中号指标卡)

```html
<!-- 黄字深底版 -->
<div class="metric-card accent">
  <div class="v">+16.7<span class="u">%</span></div>
  <div class="l">水电费支付比例提升</div>
  <div class="n">上线后 2 月 → 3 月对比。</div>
</div>

<!-- 普通白底版 -->
<div class="metric-card">
  <div class="v">−20<span class="u">%</span></div>
  <div class="l">请假率降低</div>
  <div class="n">备注。</div>
</div>

<!-- 浅黄底结论版(只有 .l + .n) -->
<div class="metric-card" style="background:#FFFCE6;border-color:var(--yellow-deep)">
  <div class="l">关键结论</div>
  <div class="n" style="color:#5a5340;font-size:13.5px;margin-top:8px;line-height:1.55">结论文字。</div>
</div>
```

多个 metric-card 通常用 `.mstack` 容器包(纵向堆叠,gap 16px)。

---

## barlist(水平横条列表)

```html
<div class="card">
  <div class="card-h">
    <span class="t">业务方上线需求分布</span>
    <span class="m">降序排列 · 共 151 项</span>
  </div>
  <div class="barlist"
       data-bars='[{"l":"HR Product","v":39},{"l":"C&B","v":27}]'
       data-bars-topn="3"></div>
</div>
```

- `data-bars`:JSON 数组,按 v 降序
- `data-bars-topn`:前 N 个用深灰色,第 1 个用黄色,其余用墨色

---

## donut(环形图 + legend)

```html
<div class="card">
  <div class="card-h">
    <span class="t">优先级分布</span>
    <span class="m">共 153 项</span>
  </div>
  <div class="donut-wrap">
    <div class="donut" id="donutXX"
         data-donut='[{"l":"P1","v":114},{"l":"P0","v":19}]'
         data-donut-colors='["#FFEA33","#15171C","#9AA1AD","#D6DAE1"]'
         data-donut-legend="donutXXLegend"></div>
    <ul class="donut-legend" id="donutXXLegend"></ul>
  </div>
</div>
```

- `id` 与 `data-donut-legend` 必须全 deck 唯一
- `data-donut-colors` 可省,缺省用默认色板

---

## gbars(分组水平柱状,如 2 月 vs 3 月对比)

```html
<div class="gbars">
  <div class="gbar-row">
    <div class="gl">普通付款</div>
    <div class="gbar feb">
      <div class="barwrap">
        <div class="track"></div>
        <div class="fill" data-w="20.3" style="--i:0">
          <span class="mtag">2月</span>
        </div>
      </div>
      <span class="vlbl">263</span>
    </div>
    <div class="gbar mar">
      <div class="barwrap">
        <div class="track"></div>
        <div class="fill" data-w="24.2" style="--i:1">
          <span class="mtag">3月</span>
        </div>
      </div>
      <span class="vlbl">314<span class="delta">+19%</span></span>
    </div>
  </div>
  <!-- 重复 gbar-row 数行 -->
</div>
```

- `.gbar.feb` = 前期(灰色);`.gbar.mar` = 后期(黄色)
- `data-w` = 0~100 的百分比(相对最大值)
- `--i` = 入场动画延迟序号(同 slide 内递增)

---

## line-chart(折线图)

```html
<div class="card" style="padding:12px">
  <div class="card-h">
    <span class="t">报销油费金额趋势</span>
    <span class="m">单位:万元</span>
  </div>
  <div class="line-chart-wrap" id="lineChartXX"
       data-line='[{"l":"1月","v":273.5},{"l":"2月","v":193.7}]'
       data-line-color="#FFEA33"
       data-line-ymax="300"
       data-line-animate="true"></div>
</div>
```

- `id` 必须唯一
- `data-line-ymax` 可省;`data-line-color` 缺省 #FFEA33;`data-line-animate` 缺省 true

---

## eff-card(效率对比卡)

```html
<div class="card eff-card">
  <div class="eff-flag"><span class="chip">TH</span>泰国</div>
  <div class="eff-name">泰国 Hub</div>
  <div class="eff-grid">
    <div class="eff-stat"><div class="v acc">126</div><div class="l">操作总次数</div></div>
    <div class="eff-stat"><div class="v">41</div><div class="l">批量操作次数 · 32.54%</div></div>
    <div class="eff-stat"><div class="v">239</div><div class="l">批量处理工号数</div></div>
    <div class="eff-stat">
      <div class="v">−198</div>
      <div class="l">节省手动操作</div>
      <div class="n">约节省 <b>1.1 小时</b></div>
    </div>
  </div>
</div>
```

- 固定 2x2 网格;左上 `.v.acc`(黄色),其余白色
- 第 4 个 stat 可带 `.n` 备注

---

## team-card(团队/方法论卡)

```html
<div class="card team-card">
  <div class="team-tag"><span class="ix">A</span>团队成长</div>
  <div class="team-title">团队成长</div>
  <div class="team-item">
    <div class="n">01</div>
    <div class="b">正文,<b>关键词</b> 用 b 标签会自动加黄绿底高亮。</div>
  </div>
  <div class="team-item">
    <div class="n">02</div>
    <div class="b">第二条要点。</div>
  </div>
</div>
```

- `.ix` 字母徽章("A"/"B"),`.n` 编号("01"/"02"/"03")
- `.b` 内 `<b>` 自动高亮

---

## eff-foot(底部强调结论条)

```html
<div class="eff-foot">
  按每个工号从邮件复制到系统提交约耗时 20 秒估算 —— 仅本统计窗口,菲律宾即节省约 <b>22.05 小时</b>、泰国约 <b>1.1 小时</b>;放大到全年,收益将持续复利。
</div>
```

深色底卡片,放在 slide 底部做总结,`<b>` 自动黄色加粗。

---

## 入场动画类(加在子元素上)

| class | 延迟 |
|---|---|
| `.au` | 0 |
| `.au1` | 80ms |
| `.au2` | 160ms |
| `.au3` | 240ms |
| `.au4` | 320ms |
| `.fade` | 0(纯淡入,800ms) |

按视觉重要性逐级延迟。

---

## 网格容器

| class | 用途 |
|---|---|
| `.grid-3` | 三列(.85fr 1.45fr .9fr),KPI + 中等 + 小 |
| `.grid-2` | 两列等分 |
| `.grid-ba` | 两列(.62fr 1.5fr),小左大右 |
| `.grid-util` | 两列(.78fr 1.4fr),中左大右 |

不在表中的布局:用 inline `style="display:grid;grid-template-columns:1fr 1fr;gap:10px;flex:1;min-height:0"` 自定义(参考 50-quad-line.html 的 2×2)。

---

## 高亮规则速查

| 想要的效果 | 用法 |
|---|---|
| 章节标题里的关键词加黄色背景 | `<span class="hl">关键词</span>` |
| 正文/结论里的关键数据加粗 + 高亮 | `<b>关键数据</b>`(在 .team-item .b 内或 .eff-foot 内自动高亮) |
| 强调一个百分比 | `<span class="delta">+19%</span>`(深黄,12px,旁边小字) |
| 标签徽章 | `<span class="tag solid">提效</span>` 或 `<span class="tag">提效</span>` |
