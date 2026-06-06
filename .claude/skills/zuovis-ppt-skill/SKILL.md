---
name: zuovis-ppt-skill
description: 生成精美的 HTML 幻灯片演示文稿。当用户需要制作演示文稿、PPT、slides、汇报材料、演讲稿、读书分享、产品介绍、数据分析报告时，务必使用此 skill。即使只提到"做个ppt"、"帮我做个slides"、"整理成演示文稿"等，也应触发此 skill。内置四种专业主题：商务现代白（企业汇报/数据分析）、商务现代黑（科技峰会/产品发布）、蓝墨茶（文艺分享/读书笔记/人文讲座）、蓝墨茶·夜（暗色文艺/夜间阅读/深度人文）。
---

# Zuovis PPT Skill

生成独立的、精美的 HTML 幻灯片文件。根据用户内容自动选择最佳主题和内容类型，输出可键盘/点击导航的完整 HTML 文档。

## 设计系统

三层结构：通用规范定义所有主题共享的基础，商务/文艺各定义自身专属配置。资源文件位于 `assets/themes/{theme-id}/`。

### 通用设计规范（所有主题适用）

以下规范不区分主题，任何生成的 slide 都必须遵守。

**画布**：100vw × 100vh，slide 绝对定位重叠排列，`opacity` 切换可见性。

**圆角**：所有卡片、引用块、表格统一使用 Squircle 超椭圆圆角。通过 figma-squircle CDN（`<script type="module">`）在运行时应用 `clip-path: path(...)`。各主题的 border-radius 作为 fallback。

**导航**：点击右 40% 前进、左 40% 后退，键盘 ← → 翻页。统一包含在 `template.html` 的导航 JS 中。

**不重叠机制**：所有 slide 使用 CSS flex 纵向布局。标题区和页码区 `flex-shrink:0` 固定高度，内容区 `flex:1` 自动拿满剩余空间。标题不换行 → 区域高度不变 → 内容空间可精确预算，不会重叠。

**封面与尾页**：不看页码。封面=主标题+副标题+主讲人+时间。尾页=四字结束词（如"谢谢聆听"）+可选补充文字+主讲人+时间。

---

### 商务主题（business-modern / business-modern-dark）

设计语言：**现代、专业、数据驱动**。适用于企业汇报、数据分析、产品发布、技术分享。

#### 色彩

| 角色 | business-modern | business-dark |
|------|:--:|:--:|
| 背景 | `#ffffff` | `#0F172A` |
| 主色 | `#10B981` | `#10B981` |
| 辅色 | `#34D399` | `#34D399` |
| 文字 | `#0F172A` | `#F1F5F9` |
| 次要文字 | `#1E293B` | `#94A3B8` |
| 玻璃卡底 | `rgba(255,255,255,0.25)` | `rgba(30,41,59,0.6)` |

主色只用绿色系 `#10B981` → `#84CC16`。高亮数据卡片用 `#10B981 → #34D399` 渐变背景 + 白色文字。

#### 字体

```yaml
字族: 阿里巴巴普惠体 → -apple-system → sans-serif
封面标题: 3.5rem / 700 / line-height 1.2
页面标题: 2rem / 600 / line-height 1.3 · ≤15字 · 一行不换行
副标题:   1.5rem / 400 / ≤25字 · 一行不换行
正文:     1.2rem / line-height 1.8~2.0
页码:     0.8rem
```

#### 页面结构（三区制）

```
┌─ title-region（flex-shrink:0）──┐  标题 + 6px 绿色竖条装饰
├─ summary-region（flex-shrink:0）─┤  副标题
├─ content-region（flex:1）────────┤  内容垂直居中，仅一种类型
├─ page-number-region（flex-shrink:0）─┤  右对齐
└──────────────────────────────────┘
```

区间 `--region-gap: 0.2cm`，区内 `--region-padding: 0.1cm`。slide padding 上下 0.8cm / 左右 2cm。背景四色弥散渐变。

#### 内容类型（9种，全部可用）

| 类型 | 规格 | 最大数量 |
|------|------|---------|
| data-cards | 3-4张 flex 均分，最大数加 `.highlighted`（绿渐变+上浮+白字） | 标题≤6字/数值≤6位/单位≤2字/说明≤15字 |
| data-chart | 左 info(260px) + 右 ECharts，**基于数据特征自动选择图表类型**，见下方规则 | info≤3条/条≤40字，图表min-height 200px |
| comparison-feature | 左右凹卡(480px×340px) + VS徽章(100px绿圆) | 左右各**4条**特性，条≤35字 |
| comparison-table | 绿渐变表头 + 毛玻璃表体 + `.highlight-cell` | ≤8行+≤5列，格≤20字 |
| timeline | 横向绿轨 + 圆点 + squircle卡(200px宽) | 3-5节点，年≤6字/标题≤10字/描述≤25字 |
| architecture | 纯SVG viewBox缩放，3层内单列，4-5层两列 | ≤6层 |
| quote | 毛玻璃大卡(max 800px) + ::before引号 | 1-2条，正文≤80字/出处≤15字 |
| list | ≤4横向卡 / 5-8两列网格 | 标题≤10字/描述≤40字 |
| paragraph | max-width 900px | 2-4段，段≤120字，3种绿高亮 |

**data-chart 图表类型选择规则**：根据数据特征自动选择，不需要用户指定。

| 数据特征 | 图表类型 | 示例 | ECharts 配置要点 |
|---------|:--:|------|------|
| 类别对比（品牌销量、部门业绩） | **bar** 柱状图 | 比亚迪 vs 特斯拉 vs 吉利 | 绿色渐变柱，圆角顶，数据标签在上 |
| 时间序列趋势（年度增长、季度变化） | **line** 折线图 | 2021-2025 营收趋势 | `smooth: true` 贝塞尔曲线，非硬折线；`symbol: 'circle'` 数据点；渐变填充面积 |
| 占比分布（市场份额、收入构成） | **pie** 饼图 | 各业务线收入占比 | 环形 `radius: ['40%','70%']`，绿色系配色，标签显示百分比 |
| 多维评估（多指标雷达扫描） | **radar** 雷达图 | 产品竞争力五维评估 | 半透明填充 `opacity: 0.3`，绿色描边 |

折线图使用 `smooth: true`（贝塞尔曲线插值），不是硬折线连接。柱状图使用 `animationDuration: 1500` + `animationEasing: 'elasticOut'` 弹性动画。

#### 效果与质感

| 效果 | 实现 |
|------|------|
| 轮廓阴影 | CSS `filter: drop-shadow()`，跟随 Squircle 曲线 |
| 毛玻璃 | `backdrop-filter: blur(20px) saturate(180%)` |
| 入场动画 | inline keyframes：fadeInDown / fadeIn / fadeInUp / zoomIn |
| 数字滚动 | JS easeOutCubic 动画 |
| 图标 | 内联 SVG（17 个 Lucide 图标映射） |

#### 资源清单

| 文件 | 说明 |
|------|------|
| `template.html` | 骨架 + 导航 + squircle |
| `style.css` | 完整样式（1228行/1220行） |
| `charts.js` | ECharts 主题配置 |

---

### 文艺主题（aisumicha）

设计语言：**温润、克制、以字为美**。适用于读书分享、文学鉴赏、哲学思辨、个人随笔。

#### 色彩

| 角色 | 值 | 约束 |
|------|------|------|
| 底色 | `#f5f4ed` | 暖米纸，不用纯白 |
| 卡面 | `#faf9f5` | 象牙色 |
| 主色 | `#1B365D` | 油墨蓝，仅此一种强调色 |
| 文字 | `#141413` | 近乎黑 |
| 灰调 | `#3d3d3a / #504e49 / #6b6a64` | 全暖调，禁止冷蓝灰 |
| Tag | `#EEF2F7 ~ #D6E1EE` | 实色，禁止 rgba |

#### 字体

```yaml
正文字族: ChosunilboNM → 思源宋体 → Georgia → serif
封面标题: 4rem / 500 / line-height 1.05
页面标题: 2rem / 500 / line-height 1.2
正文:     1rem / 400 / line-height 1.8
```

英文 serif 通吃标题和正文。标题 500w，正文 400w。行距三档：1.1-1.3(紧凑) / 1.4-1.45(密排) / 1.5-1.8(阅读)。

#### 页面结构（单栏制）

```
┌──────────────────────────────────┐
│  壹（.section-num）              │
│  内容标题（.content-title）       │
│                                  │
│  内容（.content-body 内流动）      │
│                                  │
│                         3 / 8    │  .page-number 绝对定位右下
└──────────────────────────────────┘
```

无三区分离。`.content-wrapper` 65%宽左对齐。slide padding 上下 0.6cm / 左右 2.5cm。背景 parchment + SVG 纸张纹理。

封面：`.eyebrow`(眉批) → `.cover-title` + `.brand`(油墨蓝) → `.tagline` → `.cover-meta`
尾页：`.end-title` → `.end-subtitle` → `.end-meta`

#### 内容类型（6种可用，3种禁用）

禁用：`data-chart`、`comparison-feature`、`architecture`。

| 类型 | 实现 | 限制 |
|------|------|------|
| paragraph | `.content-body` > `p` | 2-4段，段≤100字 |
| quote | `.quote-block`（ring shadow） | 1-2条，条≤80字 |
| list | `.content-list` > `li`（圆点装饰） | ≤5项 |
| data-cards | `.data-grid`(3列) / `.data-grid-2`(2列) | ≤6张，数值 1.75rem |
| comparison-table | `.compare-table`（暖灰表头） | ≤5行+≤4列 |
| timeline | 竖向 `.timeline` > `.timeline-item` | ≤4项 |

#### 效果与质感

| 效果 | 实现 |
|------|------|
| 纸张纹理 | SVG feTurbulence noise（opacity 0.025） |
| 卡片阴影 | `box-shadow: 0 0 0 1px` ring shadow |
| 强调 | `.highlight`（油墨蓝 500w），无其他高亮色 |
| 动画 | 仅 fadeIn 一种，0.4s ease-out |
| 图标 | 无 |

#### 资源清单

| 文件 | 说明 |
|------|------|
| `template.html` | 骨架 + 导航 + squircle |
| `style.css` | 完整样式（~500行） |
| `charts.js` | 不需要 |

---

### 文艺主题深色版（aisumicha-dark）

aisumicha 的暗色变体。设计语言一致（温润、克制、以字为美），仅色彩反转。适用于夜间阅读场景、深色调人文分享。

#### 色彩（暗色暖调）

| 角色 | 值 | 约束 |
|------|------|------|
| 底色 | `#1C1B1A` | 暖墨黑，不用纯黑 |
| 卡面 | `#262523` | 微暖深灰 |
| 主色 | `#8BAFCE` | 月光油墨蓝，仅此一种强调色 |
| 文字 | `#E5E2D8` | 暖白，不用纯白 |
| 灰调 | `#B0ACA4 / #9E9A92 / #8A867E` | 全暖调，禁止冷蓝灰 |
| Tag | `#2A3545 ~ #3A4A5C` | 实色 |

#### 与浅色版的差异

- 纸张纹理 opacity 从 0.025 降为 0.015（暗色背景下更微妙）
- ring shadow 包边：`rgba(255,255,255,0.06)`（浅色版用 `#e5e3d8` 实色）
- 其他完全一致：字体、字号、行距、页面结构、内容类型（6种）、动画

#### 资源清单

| 文件 | 说明 |
|------|------|
| `template.html` | 骨架 + 导航 + squircle |
| `style.css` | 完整样式（~500行） |

## 工作流程

**重要**：必须分步与用户交互确认，不能跳步。每一步都必须使用 `AskUserQuestion` 让用户确认后再继续。

---

### Step 1: 主题选择

收到用户话题后，分析内容领域，**先给出推荐**，再用 `AskUserQuestion` 让用户确认。

推荐逻辑：
- **商业/企业/数据/产品/市场** → `business-modern`（商务现代·白）
- **科技/技术/AI/发布会/演讲** → `business-modern-dark`（商务现代·黑）
- **文学/艺术/哲学/历史/人文/读书/随笔（浅色）** → `aisumicha`（蓝墨茶·文青）
- **文学/艺术/哲学/历史/人文/读书/随笔 + 夜间/暗色偏好** → `aisumicha-dark`（蓝墨茶·夜）

用 `AskUserQuestion` 询问，格式如下：

```
header: "幻灯片风格"
question: "根据你的内容，推荐使用「{推荐主题名}」风格。用这个还是换一个？"
选项:
  - "{推荐主题名}（推荐）"
  - "换商务现代·白（企业汇报/数据分析）"
  - "换商务现代·黑（科技峰会/产品发布）"
  - "换蓝墨茶·文青（读书分享/人文讲座）"
  - "换蓝墨茶·夜（暗色文艺/夜间阅读）"
```

用户选完后，简单确认一句话继续。

---

### Step 2: 内容素材

用 `AskUserQuestion` 询问用户是否有现成素材：

```
header: "内容素材"
question: "有没有现成的文字、数据或参考资料？没有的话我直接根据话题来规划。"
选项:
  - "没有，直接开始（推荐）"（multiSelect: false）
  - "我有素材，发给你"（multiSelect: false）
```

- 如果用户选「没有」→ 进入 Step 3，基于话题自主创建内容
- 如果用户选「有素材」→ 等待用户发送内容，收到后再进入 Step 3

---

### Step 3: 生成大纲并确认

根据话题和素材（如有），生成完整大纲。规则：
- 第一页必须是 `cover`，最后一页必须是 `end`
- 中间 3-10 页内容页，总页数控制在 5-14 页
- 每页选择合适的 `content_type`
- `summary` 是一句话概括（≤30 字）

**aisumicha 内容限制**：引用≤2条、时间线≤4项、表格≤5行、列表≤5项、数据卡片≤6张，禁止图表和 comparison-feature。aisumicha 窄栏排版（65%宽），内容过多会溢出。

**展示大纲给用户**，用清晰的列表格式：

```
📋 大纲预览（共 N 页）

1. 封面 — 「标题」
   副标题：xxx

2. 数据卡片 — 「核心指标」
   一句话：xxx

3. 特点对比 — 「特斯拉 vs 比亚迪」
   一句话：xxx

...

N. 尾页 — 「谢谢」
```

然后用 `AskUserQuestion` 确认：

```
header: "确认大纲"
question: "大纲这样安排可以吗？"
选项:
  - "确认，开始生成（推荐）"
  - "需要修改（请说明改哪里）"
```

- 如果「需要修改」→ 让用户说明，调完再展示新大纲，再次确认
- 如果「确认」→ 进入 Step 4
- 生成前，逐页对照「设计系统」中当前主题的内容类型限制检查大纲

### Step 4: 生成每页 HTML

**关键原则**：严格按上方「设计系统」中当前主题的规范和限制生成 HTML。使用主题 CSS 中定义的精确类名。content-region 内只放一种 content_type。

每页 slide 的 CSS 类名固定：
- 封面：`<div class="slide cover-slide">`
- 内容页：`<div class="slide content-slide">`
- 尾页：`<div class="slide end-slide">`

更多细节和每种 content_type 的 HTML 模板见 `references/html-templates.md`。

**aisumicha 警告**：使用完全不同的 CSS 类名体系（见 `references/html-templates.md` aisumicha 专节），生成前务必先读。

---

### Step 5: 组装最终 HTML

1. 读取 `assets/themes/{theme-id}/template.html`
2. 读取 `assets/themes/{theme-id}/style.css`
3. 如果主题有 `charts.js` 且有图表页，读取 `assets/themes/{theme-id}/charts.js`
4. 替换模板占位符：
   - `{{TITLE}}` → 演示文稿标题
   - `{{THEME_STYLE}}` → **直接插入** style.css 完整内容
   - `{{SLIDES_CONTENT}}` → 所有 slide div 拼接
   - `{{CHART_CONFIG}}` → charts.js 内容（无则留空）
   - `{{CHART_INIT}}` → 每个图表页的 ECharts 初始化脚本（格式见 references）
   - `{{FONT_PATH}}` → 字体路径，用 `./fonts/`
5. 清理外部 CDN 依赖：
   - 删除 Lucide CDN script — 改用内联 SVG（见 `references/html-templates.md` 图标映射表）
   - 删除 animate.css CDN link — 内联必要关键帧
   - ECharts CDN script 保留（图表渲染必须）
   - figma-squircle CDN（`<script type="module">`）保留（Squircle 圆角，仅 business 主题，9KB）
6. 删除 @font-face 声明中不存在本地字体的声明，保留系统 fallback
7. 将最终 HTML 写入 `{标题}.html`，然后告知用户文件路径

## 内联资源速查

### animate.css 关键帧（仅 business-modern/business-modern-dark 需要）
内联 fadeInDown、fadeIn、fadeInUp、zoomIn 四个关键帧及对应的 `.animate__*` 类。完整 CSS 见 `references/html-templates.md`。

### Lucide 图标（替换 `<i data-lucide="...">`）
将所有 `<i data-lucide="xxx">` 替换为内联 SVG。完整映射表见 `references/html-templates.md`。

### ECharts 初始化
每张图表页需要独立的 `<script>` 初始化块。图表配色统一使用主题绿色系：`#10B981`, `#34D399`, `#059669`, `#14B8A6`, `#84CC16`。模板见 `references/html-templates.md`。

## 字体处理

- **business-modern / business-modern-dark**：@font-face 声明阿里巴巴普惠体，fallback 到 `-apple-system, BlinkMacSystemFont, sans-serif`
- **aisumicha / aisumicha-dark**：@font-face 声明ChosunilboNM + 思源宋体，fallback 到 `Georgia, serif`
- 本地没有字体文件时，直接删除 @font-face 声明块，使用 fallback

## 输出

- 独立 HTML 文件，浏览器直接打开即可展示
- 点击右 40% 区域前进，左 40% 区域后退
- 键盘 ← → 键导航
- 数据卡片有数字滚动动画、图表自动渲染
