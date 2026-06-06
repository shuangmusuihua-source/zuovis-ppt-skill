# Zuovis PPT Skill

AI 驱动的幻灯片生成 skill，内置商务现代（浅色/暗色）和蓝墨茶文艺两种设计系统。

## 快速开始

在 Claude Code 中，输入 `/zuovis-ppt-skill <话题>` 即可启动。

## 设计系统

| 主题 | 类型 | 风格 |
|------|------|------|
| business-modern | 商务汇报 | 白色、绿色系、毛玻璃、Squircle 圆角 |
| business-modern-dark | 科技/发布会 | 深色、绿色荧光、毛玻璃、Squircle 圆角 |
| aisumicha | 读书/文艺 | 暖米纸、油墨蓝、Serif 字体、纸张纹理 |

## 文件结构

```
├── SKILL.md                         # Skill 主文件（设计系统 + 工作流程）
├── references/
│   └── html-templates.md           # 9 种内容类型 HTML 模板 + 图标映射
└── assets/themes/
    ├── business-modern/            # 商务现代·白
    │   ├── template.html
    │   ├── style.css
    │   ├── charts.js
    │   └── showcase.html           # 主题全览（调试用）
    ├── business-modern-dark/       # 商务现代·黑
    │   ├── template.html
    │   ├── style.css
    │   ├── charts.js
    │   └── showcase.html
    └── aisumicha/                  # 蓝墨茶·文青
        ├── template.html
        ├── style.css
        └── showcase.html
```

## 功能

- 3 种内置主题，自动匹配 + 用户确认
- 9 种内容类型（数据卡片、图表、对比、时间线、架构图、引用、列表、段落）
- Squircle 超椭圆圆角（figma-squircle）
- SVG 架构图（viewBox 自动缩放）
- 分步交互确认（主题 → 素材 → 大纲 → 生成）
- 独立 HTML 输出（键盘/点击翻页，零外部依赖除 ECharts + figma-squircle CDN）
