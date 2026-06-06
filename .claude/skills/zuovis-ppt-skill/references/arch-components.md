# 架构图组件参考

用 HTML/CSS 组件拼装架构图，不手写 SVG 坐标。AI 根据架构描述选择合适的零件自行组装。

## 组件清单

### 容器

**arch-box** — 带标题的圆角边框容器
```html
<div class="arch-box">
  <div class="arch-box-header">平台名称</div>
  <div class="arch-box-body"><!-- 内容 --></div>
</div>
```
- `.arch-box-header` 可选，不需要标题时可省略
- `.arch-box-body` 内放子组件

**arch-section** — 虚线分区（用于分组内部模块）
```html
<div class="arch-section">
  <span class="arch-section-title">分区名</span>
  <!-- 内容 -->
</div>
```

**arch-sidebar** — 左侧渐变色标签 + 右侧内容区
```html
<div class="arch-sidebar">
  <div class="arch-sidebar-label">标签文字<br/>可换行</div>
  <div class="arch-sidebar-body"><!-- 内容 --></div>
</div>
```

### 内容

**arch-module** — 模块卡片（带色条 header）
```html
<div class="arch-module">
  <div class="arch-module-header">模块名</div>
  <div class="arch-module-body">描述或子内容</div>
</div>
```

**arch-card** — 图标卡片（上方图标 + 下方说明，适合能力矩阵）
```html
<div class="arch-card">
  <div class="arch-card-icon"><svg>...</svg></div>
  <div class="arch-card-label">卡片说明</div>
</div>
```

**arch-node** — 外部节点（pill 形状，适合数据源等外挂组件）
```html
<div class="arch-node">
  <div class="arch-node-icon"><svg>...</svg></div>
  节点名称
</div>
<!-- 数据库节点加 .db 类，图标变琥珀色 -->
<div class="arch-node db">
  <div class="arch-node-icon"><svg><!-- 数据库图标 --></svg></div>
  MySQL
</div>
```

**arch-bar** — 全宽色条（适合网关、安全行等）
```html
<div class="arch-bar">网关名称 — 简要描述</div>
```

### 布局

| 类名 | 用途 |
|------|------|
| `arch-row` | 水平排列（flex，gap:12px） |
| `arch-grid arch-grid-N` | N 列网格（N=2,3,4,6） |
| `arch-stack` | 纵向堆叠（flex-column，gap:10px） |
| `arch-pair` | 左右对比（flex，gap:16px） |

### 连接

**arch-arrow-down** — 层间向下箭头
```html
<div class="arch-arrow-down">
  <svg viewBox="0 0 20 14"><line x1="10" y1="0" x2="10" y2="10" stroke="var(--arch-primary)" stroke-width="2"/><polygon points="4,8 10,14 16,8" fill="var(--arch-primary-light)"/></svg>
</div>
```

**SVG 连线层** — 用于复杂连线（曲线、分叉等）
```html
<svg class="arch-wires" viewBox="0 0 W H" preserveAspectRatio="none">
  <path d="M x1 y1 C cx1 cy1, cx2 cy2, x2 y2" stroke="var(--arch-primary)" stroke-width="1.5" fill="none"/>
</svg>
```
- `arch-wires` 类使 SVG 覆盖在容器上方，pointer-events:none 不阻挡交互
- 适合中心平台 + 外部节点之间的曲线连线

## CSS 变量

所有组件使用 CSS 变量，跟随主题色：

| 变量 | business-modern | business-dark |
|------|:--:|:--:|
| `--arch-bg` | `#ECFDF5` | `#0D2818` |
| `--arch-primary` | `#10B981` | `#10B981` |
| `--arch-primary-light` | `#34D399` | `#34D399` |
| `--arch-text` | `#0F172A` | `#F1F5F9` |
| `--arch-muted` | `#475569` | `#94A3B8` |
| `--arch-subtle` | `#94A3B8` | `#64748B` |
| `--arch-border` | `#D1FAE5` | `#064E3B` |

## 组装指南

生成时按以下步骤：

1. 分析架构描述 → 判断属于哪种结构（层级/中心平台/能力矩阵/混合）
2. 选择合适的容器 → arch-box 还是 arch-sidebar
3. 用 arch-section 分组内部模块
4. 用 arch-module / arch-card / arch-node / arch-bar 填充内容
5. 用 arch-row / arch-grid / arch-stack 控制排列
6. 层间加 arch-arrow-down，复杂连线画 SVG arch-wires

**关键原则**：组件可以嵌套。arch-box-body 里可以放 arch-section，arch-section 里可以放 arch-row + arch-module。灵活组合，不限于固定模板。
