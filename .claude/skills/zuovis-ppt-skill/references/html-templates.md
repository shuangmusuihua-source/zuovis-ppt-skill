# HTML 模板参考

## 内容页通用结构（business-modern / business-modern-dark）

```html
<div class="slide content-slide">
  <div class="title-region">
    <div class="title">
      <span class="title-decor"></span>
      <span>页面标题</span>
    </div>
  </div>
  <div class="summary-region">
    <div class="summary">一句话总结（不超过30字）</div>
  </div>
  <div class="content-region">
    <!-- 具体内容 HTML，见下方各类型 -->
  </div>
  <div class="page-number-region">
    <span class="page-number">N / TOTAL</span>
  </div>
</div>
```

四大区域：title-region（标题+绿色竖条装饰）、summary-region（一句话总结）、content-region（flex:1 自动垂直居中）、page-number-region（右对齐页码）。

---

## 封面页 (cover)

```html
<div class="slide cover-slide">
  <div class="cover-title animate__animated animate__fadeInDown">演示文稿主标题</div>
  <div class="cover-subtitle animate__animated animate__fadeIn">副标题 / 一句话描述</div>
  <div class="cover-meta animate__animated animate__fadeInUp">
    <div class="cover-author">作者名称 / 机构</div>
    <div class="cover-date">2026年6月</div>
  </div>
</div>
```

封面无 page-number-region。animate__ 类绑定了入场动画。

---

## 尾页 (end)

```html
<div class="slide end-slide">
  <div class="end-title animate__animated animate__fadeInDown">谢谢</div>
  <div class="end-subtitle animate__animated animate__fadeIn">感谢聆听</div>
  <div class="end-meta animate__animated animate__fadeInUp">
    <div class="end-author">作者</div>
    <div class="end-date">2026年6月</div>
  </div>
</div>
```

---

## 数据卡片 (data-cards)

建议 3-4 张卡片。数值最大的一张添加 `highlighted` 类（变绿色渐变背景+上移效果）。

```html
<div class="data-cards">
  <div class="data-card highlighted">
    <div class="card-header">
      <svg><!-- 内联 SVG 图标 --></svg>
      <span class="card-title">年度营收</span>
    </div>
    <div class="card-divider"></div>
    <div class="data-value">
      <span data-target="1280">1,280</span>
      <span class="data-label-inline">亿</span>
    </div>
    <div class="data-label">同比增长 35%</div>
  </div>
  <div class="data-card">
    <div class="card-header">
      <svg><!-- 内联 SVG 图标 --></svg>
      <span class="card-title">月活用户</span>
    </div>
    <div class="card-divider"></div>
    <div class="data-value">
      <span data-target="856">856</span>
      <span class="data-label-inline">万</span>
    </div>
    <div class="data-label">较去年增长 12%</div>
  </div>
  <div class="data-card">
    <div class="card-header">
      <svg><!-- 内联 SVG 图标 --></svg>
      <span class="card-title">市场份额</span>
    </div>
    <div class="card-divider"></div>
    <div class="data-value">
      <span data-target="23.5">23.5</span>
      <span class="data-label-inline">%</span>
    </div>
    <div class="data-label">行业排名第一</div>
  </div>
</div>
```

`data-target` 属性用于数字滚动动画（页面加载时从 0 滚到目标值）。

关键 CSS 类：
- `.data-cards` — flex 水平容器
- `.data-card` — 毛玻璃卡片，hover 上移 4px
- `.data-card.highlighted` — 绿色渐变背景、白色文字、上移 6px
- `.card-header` — 卡片头部（图标+标题行）
- `.card-divider` — 绿色分割线
- `.data-value` — 大号数值
- `.data-label-inline` — 单位文字
- `.data-label` — 底部说明文字

---

## 图表页 (data-chart)

左右两列布局：左侧文字说明，右侧 ECharts 图表。

```html
<div class="chart-layout">
  <div class="chart-info">
    <div class="chart-info-title">营收连续五年增长，复合增长率达 35%</div>
    <div class="chart-info-item">
      <span class="key-bold">2025年营收：</span>1,280 亿元，同比增长 42%
    </div>
    <div class="chart-info-item">
      <span class="key-color">核心驱动力：</span>AI 业务营收占比首次突破 30%
    </div>
    <div class="chart-info-item">
      <span class="key-bg">海外市场</span>贡献增速达 58%，成为第二增长曲线
    </div>
  </div>
  <div class="chart-chart-card">
    <div class="card-header">
      <svg><!-- 图表图标 SVG --></svg>
      <span class="card-title">年度营收趋势（2021-2025）</span>
    </div>
    <div class="card-divider"></div>
    <div class="chart-container">
      <div class="echarts-chart" id="chart-1"></div>
    </div>
  </div>
</div>
```

关键 CSS 类：
- `.chart-layout` — flex 左右两列
- `.chart-info` — 左侧文字区（260px 固定宽）
- `.key-bold` — 加粗强调
- `.key-color` — 绿色强调
- `.key-bg` — 绿色半透明背景标签
- `.chart-chart-card` — 右侧图表毛玻璃卡片
- `.echarts-chart` — ECharts 渲染容器

---

## 特点对比 (comparison-feature)

两个对象特性对比，SVG clip-path 实现左右卡片凹陷造型。

```html
<div class="feature-compare-layout">
  <div class="feature-card left">
    <div class="feature-card-header">
      <div class="feature-card-icon"><svg><!-- 图标 SVG --></svg></div>
      <div>
        <div class="feature-card-title">特斯拉</div>
        <div class="feature-card-subtitle">纯视觉方案</div>
      </div>
    </div>
    <div class="feature-card-divider"></div>
    <ul class="feature-list">
      <li>
        <div class="check-icon"><svg><!-- check SVG --></svg></div>
        <div class="feature-text"><strong>成本优势：</strong>无需激光雷达，单车成本降低 40%</div>
      </li>
    </ul>
  </div>
  <div class="vs-badge"><svg viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="18" cy="18" r="2"/><circle cx="6" cy="6" r="2"/><path d="M13 6h3a2 2 0 0 1 2 2v7"/><path d="M11 18H8a2 2 0 0 1-2-2V9"/></svg></div>
  <div class="feature-card right">
    <!-- 同上结构，右卡片 header 和 list 靠右对齐 -->
  </div>
</div>
```

关键 CSS 类：
- `.feature-compare-layout` — flex 水平布局，居中对齐
- `.feature-card.left` — 左卡片，clip-path 右侧凹陷
- `.feature-card.right` — 右卡片，clip-path 左侧凹陷（header 和 list 自动反向对齐）
- `.feature-card-icon` — 绿色渐变圆形图标容器
- `.feature-card-title` / `.feature-card-subtitle` — 卡片标题行
- `.feature-card-divider` — 绿色分割线
- `.feature-list` > `li` — 特性列表，每行带绿色 check 图标
- `.vs-badge` — 中间绿色渐变圆形徽章，内嵌 git-compare 双向箭头图标
- `.check-icon` — 绿色圆形对勾

---

## 表格对比 (comparison-table)

```html
<div class="compare-table-wrapper">
  <table class="compare-table">
    <thead>
      <tr>
        <th>对比维度</th>
        <th>对象A</th>
        <th>对象B</th>
        <th>对象C</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>价格（万元）</td>
        <td>32.99</td>
        <td class="highlight-cell">24.99</td>
        <td>28.50</td>
      </tr>
      <!-- 更多行 -->
    </tbody>
  </table>
</div>
```

关键 CSS 类：
- `.compare-table-wrapper` — 毛玻璃圆角容器
- `.compare-table thead` — 绿色渐变表头，白色文字
- `.highlight-cell` — 最优值绿色加粗
- `.company-name` — 首列维度名可加此样式

---

## 时间线 (timeline)

横向时间轴，建议 3-5 个节点。

```html
<div class="timeline-horizontal">
  <div class="timeline-track">
    <div class="timeline-line-h"></div>
    <div class="timeline-nodes">
      <div class="timeline-node">
        <div class="timeline-dot-h"></div>
        <div class="timeline-card">
          <div class="timeline-year-h">2021</div>
          <div class="timeline-title-h">公司成立</div>
          <div class="timeline-desc-h">获天使轮融资 5000 万</div>
        </div>
      </div>
      <!-- 更多节点 -->
    </div>
  </div>
</div>
```

关键 CSS 类：
- `.timeline-horizontal` — 横向时间线容器
- `.timeline-track` — 轨道容器
- `.timeline-line-h` — 绿色渐变横线（绝对定位）
- `.timeline-nodes` — flex 水平分布节点
- `.timeline-dot-h` — 绿色圆点（第一个节点颜色更深）
- `.timeline-card` — 毛玻璃卡片，hover 上浮 8px
- `.timeline-year-h` — 绿色年份
- `.timeline-title-h` — 节点标题
- `.timeline-desc-h` — 节点描述

---

## 架构图 (architecture)

使用 **HTML/CSS 组件拼装**，不手写 SVG 坐标。根据架构描述选择合适的零件自由组装，支持层级架构、中心平台、能力矩阵等多种样式。

完整组件文档、CSS 类名、组装指南见 `references/arch-components.md`。生成架构页前务必先读该文件。

### 三种基础样式

| 样式 | 适用场景 | 核心组件 |
|------|------|------|
| 层级架构 | 分层系统（如网络协议栈、技术平台） | arch-box + arch-stack + arch-arrow-down |
| 中心平台 | 中台架构、数据平台（外部节点 + 内部模块） | arch-box + arch-section + arch-node + arch-module + arch-wires |
| 能力矩阵 | 功能展示、产品能力概览 | arch-sidebar + arch-grid + arch-card |

### 快速参考

```html
<!-- 层级架构 -->
<div class="arch-stack">
  <div class="arch-box"><!-- L1 ... --></div>
  <div class="arch-arrow-down"><svg>...</svg></div>
  <div class="arch-box"><!-- L2 ... --></div>
  <div class="arch-arrow-down"><svg>...</svg></div>
  <div class="arch-section"><span class="arch-section-title">L3 分组</span><!-- 两列 arch-module --></div>
  <div class="arch-arrow-down"><svg>...</svg></div>
  <div class="arch-bar">L4 ...</div>
</div>

<!-- 中心平台 (+ 外部节点) -->
<div style="display:flex;gap:16px;position:relative">
  <div class="arch-stack"><!-- 左侧节点 -->
    <div class="arch-node db"><div class="arch-node-icon"><svg>...</svg></div>MySQL</div>
  </div>
  <svg class="arch-wires" viewBox="..."><path d="..."/></svg>
  <div class="arch-box" style="flex:1">
    <div class="arch-box-header">平台名称</div>
    <div class="arch-box-body">
      <div class="arch-section"><span class="arch-section-title">分区</span><!-- arch-module × N --></div>
    </div>
  </div>
</div>

<!-- 能力矩阵 -->
<div class="arch-sidebar">
  <div class="arch-sidebar-label">标签</div>
  <div class="arch-sidebar-body">
    <div class="arch-grid arch-grid-6"><!-- arch-card × N --></div>
  </div>
</div>
```

### 生成规则

1. **放在 content-region 内**，架构图自适应 flex:1 空间
2. **组件自由组合**，不限于固定模板——层级用 arch-stack，分区用 arch-section，卡片用 arch-card
3. **连线用 arch-wires**（SVG 贝塞尔曲线），放在容器内 position:absolute
4. **暗色主题**：CSS 变量自动切换（`--arch-bg` / `--arch-text` / `--arch-border`），组件结构不变
5. **aisumicha 不支持架构图**

---

## 引用块 (quote)

```html
<div class="quote-block">
  <div class="quote-text">真正的发现之旅不在于寻找新风景，而在于拥有新的眼睛。</div>
  <div class="quote-author">马塞尔·普鲁斯特</div>
</div>
```

CSS 类：`.quote-block`（毛玻璃圆角容器，::before 伪元素装饰引号）、`.quote-text`（大号引用文字）、`.quote-author`（右对齐作者，自动加 `——` 前缀）。

---

## 列表 (list)

### 默认布局（>4 项，两列网格）

```html
<div class="list-content">
  <div class="list-item">
    <div class="list-decor" data-index="1"></div>
    <div class="list-text"><span class="highlight">要点标题：</span>详细描述文字</div>
  </div>
  <div class="list-item">
    <div class="list-decor" data-index="2"></div>
    <div class="list-text"><span class="highlight">要点标题：</span>详细描述文字</div>
  </div>
  <!-- 更多 list-item -->
</div>
```

`data-index` 值自动显示为绿色圆形编号。每项带入场动画（stagger 延迟）。

### 横向布局（≤4 项，单行卡片）

给 `.list-content` 加 `layout-horizontal` 类，list-item 结构改为：

```html
<div class="list-content layout-horizontal">
  <div class="list-item">
    <div class="list-item-header">
      <div class="list-decor" data-index="1"></div>
      <div class="list-title">标题</div>
    </div>
    <div class="list-divider"></div>
    <div class="list-text">描述文字</div>
  </div>
</div>
```

CSS 类：
- `.list-content` — flex-wrap 网格容器
- `.list-content.layout-horizontal` — 单行横向布局
- `.list-decor[data-index]` — 自动显示绿色渐变编号圆形
- `.highlight` — 绿色加粗突出
- `.list-title` — 横向布局标题
- `.list-divider` — 横向布局分割线

---

## 段落 (paragraph)

三种高亮样式混合使用。

```html
<div class="paragraph-content">
  <p>2025年全球新能源汽车市场持续高速增长，全年销量突破<span class="highlight-text">1,800万辆</span>，同比增长 32%。中国市场以 <span class="highlight-text">1,280万辆</span> 的销量占比超过 70%，继续领跑全球。</p>
  <p>在技术路线方面，<span class="highlight-bg">纯电动</span>仍是主流，但<span class="highlight-bg">插电混动</span>增速显著加快。比亚迪凭借 <span class="highlight-bold">DM-i 超级混动技术</span>，全年销量突破 400 万辆，登顶全球新能源销量冠军。</p>
</div>
```

三种高亮：
- `highlight-text` — 绿色加粗文字
- `highlight-bg` — 绿色半透明背景标签（圆角 padding）
- `highlight-bold` — 加粗 + 绿色下划线

---

## 动画 CSS（内联到最终 HTML）

### business-modern / business-modern-dark 必需动画

```css
@keyframes fadeInDown {
  from { opacity: 0; transform: translate3d(0, -30px, 0); }
  to { opacity: 1; transform: translate3d(0, 0, 0); }
}
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
@keyframes fadeInUp {
  from { opacity: 0; transform: translate3d(0, 30px, 0); }
  to { opacity: 1; transform: translate3d(0, 0, 0); }
}
@keyframes zoomIn {
  from { opacity: 0; transform: scale3d(0.95, 0.95, 0.95); }
  to { opacity: 1; transform: scale3d(1, 1, 1); }
}
.animate__animated { animation-duration: 0.6s; animation-fill-mode: both; }
.animate__fadeInDown { animation-name: fadeInDown; }
.animate__fadeIn { animation-name: fadeIn; }
.animate__fadeInUp { animation-name: fadeInUp; }
.animate__zoomIn { animation-name: zoomIn; }
```

### aisumicha 不需要 animate.css

aisumicha 有自己的 fadeIn 动画，直接定义在 style.css 中，无需额外内联。

---

## Lucide 图标内联 SVG 映射

**重要**：生成最终 HTML 时，必须将所有 `<i data-lucide="xxx"></i>` 替换为对应的内联 SVG。icon 属性：`viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"`，在 `.card-header` 或 `.feature-card-icon` 中使用时 size 为 1em。

### 常用图标

**trending-up** (上升趋势)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="23 6 13.5 15.5 8.5 10.5 1 18"></polyline><polyline points="17 6 23 6 23 12"></polyline></svg>
```

**users** (用户)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 21v-2a4 4 0 0 0-4-4H6a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M22 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>
```

**dollar-sign** (金额)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="12" x2="12" y1="2" y2="22"></line><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"></path></svg>
```

**bar-chart-3** (柱状图)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"></path><path d="M18 17V9"></path><path d="M13 17V5"></path><path d="M8 17v-3"></path></svg>
```

**activity** (活动)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 12h-4l-3 9L9 3l-3 9H2"></path></svg>
```

**zap** (闪电/快速)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="13 2 3 14 12 14 11 22 21 10 12 10 13 2"></polygon></svg>
```

**target** (目标)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"></circle><circle cx="12" cy="12" r="6"></circle><circle cx="12" cy="12" r="2"></circle></svg>
```

**check** (对勾)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg>
```

**swords** (对决/对比)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="14.5 17.5 3 6 3 3 6 3 17.5 14.5"></polyline><line x1="13" x2="19" y1="19" y2="13"></line><line x1="16" x2="20" y1="16" y2="20"></line><line x1="19" x2="21" y1="21" y2="19"></line><polyline points="14.5 6.5 18 3 21 3 21 6 17.5 9.5"></polyline><line x1="5" x2="9" y1="14" y2="18"></line><line x1="7" x2="4" y1="17" y2="20"></line><line x1="3" x2="5" y1="19" y2="21"></line></svg>
```

**shield** (盾牌/安全)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M20 13c0 5-3.5 7.5-7.66 8.95a1 1 0 0 1-.67-.01C7.5 20.5 4 18 4 13V6a1 1 0 0 1 1-1c2 0 4.5-1.2 6.24-2.72a1.06 1.06 0 0 1 1.52 0C14.51 3.81 17 5 19 5a1 1 0 0 1 1 1z"></path></svg>
```

**globe** (全球/网络)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"></circle><line x1="2" x2="22" y1="12" y2="12"></line><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"></path></svg>
```

**server** (服务器)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="20" height="8" x="2" y="2" rx="2" ry="2"></rect><rect width="20" height="8" x="2" y="14" rx="2" ry="2"></rect><line x1="6" x2="6.01" y1="6" y2="6"></line><line x1="6" x2="6.01" y1="18" y2="18"></line></svg>
```

**database** (数据库)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><ellipse cx="12" cy="5" rx="9" ry="3"></ellipse><path d="M3 5V19A9 3 0 0 0 21 19V5"></path><path d="M3 12A9 3 0 0 0 21 12"></path></svg>
```

**pie-chart** (饼图)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21.21 15.89A10 10 0 1 1 8 2.83"></path><path d="M22 12A10 10 0 0 0 12 2v10z"></path></svg>
```

**shopping-cart** (购物车)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="8" cy="21" r="1"></circle><circle cx="19" cy="21" r="1"></circle><path d="M2.05 2.05h2l2.66 12.42a2 2 0 0 0 2 1.58h9.78a2 2 0 0 0 1.95-1.57l1.65-7.43H5.12"></path></svg>
```

**lightbulb** (灯泡/洞察)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 14c.2-1 .7-1.7 1.5-2.5 1-.9 1.5-2.2 1.5-3.5A6 6 0 0 0 6 8c0 1 .2 2.2 1.5 3.5.7.7 1.3 1.5 1.5 2.5"></path><path d="M9 18h6"></path><path d="M10 22h4"></path></svg>
```

**git-compare** (对比/比较)：
```html
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="18" cy="18" r="3"></circle><circle cx="6" cy="6" r="3"></circle><path d="M13 6h3a2 2 0 0 1 2 2v7"></path><path d="M11 18H8a2 2 0 0 1-2-2V9"></path></svg>
```

---

## ECharts 图表初始化脚本模板

每个图表页（data-chart）需要生成对应的初始化脚本。放在最终 HTML 的底部、slide 导航脚本之后。

### 柱状图 (bar)
```html
<script>
(function() {
  var chartDom = document.getElementById('chart-N');
  if (!chartDom) return;
  var myChart = echarts.init(chartDom);
  var option = {
    tooltip: { trigger: 'axis' },
    grid: { left: '3%', right: '4%', bottom: '3%', containLabel: true },
    xAxis: { type: 'category', data: ['2021', '2022', '2023', '2024', '2025'] },
    yAxis: { type: 'value' },
    series: [{
      type: 'bar',
      data: [320, 450, 680, 900, 1280],
      itemStyle: {
        borderRadius: [6, 6, 0, 0],
        color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
          { offset: 0, color: '#34D399' },
          { offset: 1, color: '#10B981' }
        ])
      }
    }]
  };
  myChart.setOption(option);
  window.addEventListener('resize', function() { myChart.resize(); });
})();
</script>
```

### 折线图 (line)
```html
<script>
(function() {
  var chartDom = document.getElementById('chart-N');
  if (!chartDom) return;
  var myChart = echarts.init(chartDom);
  var option = {
    tooltip: { trigger: 'axis' },
    grid: { left: '3%', right: '4%', bottom: '3%', containLabel: true },
    xAxis: { type: 'category', data: ['Q1', 'Q2', 'Q3', 'Q4'] },
    yAxis: { type: 'value' },
    series: [{
      type: 'line',
      data: [120, 200, 150, 280],
      smooth: true,
      lineStyle: { color: '#10B981', width: 3 },
      itemStyle: { color: '#10B981' },
      areaStyle: {
        color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
          { offset: 0, color: 'rgba(16, 185, 129, 0.3)' },
          { offset: 1, color: 'rgba(16, 185, 129, 0.02)' }
        ])
      }
    }]
  };
  myChart.setOption(option);
  window.addEventListener('resize', function() { myChart.resize(); });
})();
</script>
```

### 饼图 (pie)
```html
<script>
(function() {
  var chartDom = document.getElementById('chart-N');
  if (!chartDom) return;
  var myChart = echarts.init(chartDom);
  var option = {
    tooltip: { trigger: 'item' },
    series: [{
      type: 'pie',
      radius: ['40%', '70%'],
      itemStyle: { borderRadius: 6, borderColor: '#fff', borderWidth: 2 },
      label: { show: true, formatter: '{b}: {d}%' },
      data: [
        { value: 45, name: '国内市场' },
        { value: 30, name: '亚太市场' },
        { value: 15, name: '欧洲市场' },
        { value: 10, name: '其他' }
      ],
      color: ['#10B981', '#34D399', '#059669', '#14B8A6']
    }]
  };
  myChart.setOption(option);
  window.addEventListener('resize', function() { myChart.resize(); });
})();
</script>
```

N 从 1 开始递增。

---

## aisumicha 专用类名体系

aisumicha 完全不同于 business-modern 系列，使用独立的 CSS 类名：

### 封面
```html
<div class="slide cover-slide">
  <div class="cover-content">
    <div class="eyebrow">KAMISHIBAI · 纸芝居</div>
    <div class="cover-title"><span class="brand">文学</span>中的孤独美学</div>
    <div class="tagline">从马尔克斯到村上春树，孤独如何塑造现代文学的灵魂</div>
    <div class="cover-meta">
      <div class="cover-author">李明远</div>
      <div>2026 年 6 月 · 读书分享会</div>
    </div>
  </div>
</div>
```

### 内容页
```html
<div class="slide content-slide">
  <div class="content-wrapper">
    <div class="section-num">壹</div>
    <div class="content-title">孤独的双重性</div>
    <div class="content-body">
      <p>在马尔克斯笔下，孤独既是<span class="highlight">诅咒也是馈赠</span>。布恩迪亚家族七代人的孤独史，实际上是拉丁美洲被世界遗忘的隐喻。</p>
    </div>
  </div>
  <div class="page-number">3 / 10</div>
</div>
```

### 数据卡片
```html
<div class="data-grid">
  <div class="data-card">
    <div class="data-value">1982</div>
    <div class="data-label">马尔克斯获诺贝尔文学奖</div>
  </div>
  <div class="data-card">
    <div class="data-value">5,000万+</div>
    <div class="data-label">《百年孤独》全球销量</div>
  </div>
</div>
```

### 时间线（竖向）
```html
<div class="timeline">
  <div class="timeline-item">
    <div class="timeline-marker"></div>
    <div class="timeline-content">
      <div class="timeline-date">1967</div>
      <div class="timeline-title">《百年孤独》出版</div>
      <div class="timeline-desc">魔幻现实主义巅峰之作问世</div>
    </div>
  </div>
</div>
```

### 引用块
```html
<div class="quote-block">
  <div class="quote-text">生命中曾经有过的所有灿烂，原来终究都需要用寂寞来偿还。</div>
  <div class="quote-author">马尔克斯《百年孤独》</div>
</div>
```

### 对比表格
```html
<table class="compare-table">
  <thead><tr><th>作家</th><th>代表作</th><th>孤独主题</th></tr></thead>
  <tbody>
    <tr><td>马尔克斯</td><td>百年孤独</td><td>家族的宿命式孤独</td></tr>
  </tbody>
</table>
```

### 列表
```html
<ul class="content-list">
  <li>第一要点：详细描述</li>
  <li>第二要点：详细描述</li>
</ul>
```

### 尾页
```html
<div class="slide end-slide">
  <div class="end-content">
    <div class="end-title">谢谢聆听</div>
    <div class="end-subtitle">阅读，是与伟大的灵魂对话</div>
    <div class="end-meta">
      <div>李明远 · 2026 年 6 月</div>
    </div>
  </div>
</div>
```

aisumicha 页码用绝对定位：`<div class="page-number">N / TOTAL</div>`，直接放在 slide 内但在 content-wrapper 外。封面和尾页 `.page-number` 自动隐藏。
