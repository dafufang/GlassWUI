# GlassWUI · LifeWork 未来玻璃设计系统

深靛蓝宇宙背景 + 紫罗兰→霓虹粉渐变 + 毛玻璃面板，一套为「生活 × 工作」一体化产品打磨的深色玻璃拟态设计语言。源自 LifeWork 双模式工作台与产品落地页，沉淀为可复用的设计令牌、组件库与单文件 HTML 骨架。

## 设计理念

> 信息像悬浮在深夜宇宙里的玻璃卡片，光从背后透过来。
> 克制的色彩、克制的动效，让数据安静地说话。

- **深色优先**：深靛蓝 `#0A0B1A` 打底，配紫/粉/青三色径向极光，层次来自光晕而非边框。
- **玻璃拟态**：卡片是 `bg-white/5` 的毛玻璃，`backdrop-blur(16px) saturate(150%)` + 1px hairline 描边，浮在背景光之上。
- **单一渐变语言**：所有 CTA 与强调信息统一用 `linear-gradient(115deg, #7C3AED, #EC4899)`，第三色 `#06B6D4` 只做差异化。
- **Apple 发布会气质**：Inter 无衬线 + JetBrains Mono 等宽数字，指数缓动 `cubic-bezier(.22,1,.36,1)`，动效只动 transform 与 opacity。

## 色彩系统

| 令牌 | 暗色 | 亮色 | 用途 |
|---|---|---|---|
| `--paper` | `#0A0B1A` | `#F3F4FA` | 页面底色 |
| `--panel` | `rgba(255,255,255,.055)` | `rgba(255,255,255,.72)` | 玻璃面板 |
| `--ink` | `#F8FAFC` | `#13152E` | 主文字 |
| `--violet` | `#7C3AED` | 同 | 主色 / 渐变起点 |
| `--pink` | `#EC4899` | 同 | 渐变终点 |
| `--cyan` | `#06B6D4` | 同 | 第三色 |
| `--grad` | `linear-gradient(115deg,#7C3AED,#EC4899)` | 同 | CTA / 标题 |
| `--red/--amber/--green` | `#F87171/#FBBF24/#34D399` | 同 | 状态色（配 tint 底） |

语义状态色使用「半透明 tint 底 + 浅色文字」组合（如 `rgba(239,68,68,.16)` + `#F87171`），双主题下都可读，禁止硬编码浅色底。

## 字体

- **正文 / 标题**：`Inter`（回退 `-apple-system, PingFang SC, Microsoft YaHei`）
- **数字 / 代码**：`JetBrains Mono`（`font-variant-numeric: tabular-nums`）

## 组件清单

| 组件 | 说明 |
|---|---|
| 毛玻璃卡片 | `glass` 类：blur + hairline + 圆角 18px |
| 渐变 CTA | `btn-primary`：紫粉渐变 + 发光阴影 + 悬浮上移 |
| 分段开关 | `seg`：内嵌渐变选中态（生活/工作切换） |
| 统计卡 | `stats/stat`：等宽数字 + 说明行，移动端 2 列 |
| 指数环 | SVG 手绘进度环，零依赖 |
| 表单控件 | 主题化 input/select/textarea，option 显式配色 |
| 自定义玻璃下拉 | 自绘下拉列表，渲染到 body，规避系统高亮混搭 |
| 侧边栏导航 | 毛玻璃侧栏 + 渐变选中态 + 角标 |
| Toast / 弹窗 | 玻璃气泡与遮罩，遮罩带 backdrop-blur |
| Lucide 图标库 | 24 viewBox 内联 SVG，stroke 1.8 |

## 快速开始

把 `references/template.html` 打开即是一个完整可运行的页面（玻璃导航 + Hero + 统计卡 + 特性网格 + 双主题切换 + 入场动画，零外部依赖）。

```html
<!-- 1. 复制 design-tokens.css 到 <style> 顶部（双主题变量 + 极光背景） -->
<link rel="stylesheet" href="references/design-tokens.css">
<!-- 2. 按 components.md 拼装组件 -->
<div class="glass" style="padding:20px">
  <div class="eyebrow">今日支出</div>
  <div class="num">¥4,320</div>
</div>
```

## 硬规则

- **零 em-dash**：文案不使用 `—`（U+2014 / U+2013）。
- **图标**：一律内联 Lucide 风格 SVG，禁止 CDN、禁止 Emoji 充当图标。
- **动效**：只动 `transform` / `opacity`；入场动画用 IntersectionObserver 加 `.in` 类 + `--d` 错峰；`prefers-reduced-motion` 下全部塌缩为静态。
- **主题**：暗色默认，`html[data-theme="light"]` 覆盖变量；`color-scheme` 跟随主题；`select option` 显式背景色。
- **布局**：固定浮层（下拉/弹窗）渲染到 `document.body` 末尾，避开 `backdrop-filter` 祖先的 containing block 陷阱；SVG 作为 grid/flex item 时包一层 div + 固定 track 宽度；移动端触控目标 ≥ 44px、无横向溢出。

## 文件结构

```
GlassWUI/
├── README.md                       # 本文档
├── SKILL.md                        # 技能定义（WorkBuddy 加载入口）
└── references/
    ├── design-tokens.css           # 双主题设计令牌 + 极光背景 + 基础类
    ├── components.md               # 组件库：结构 + 样式 + 交互要点
    └── template.html               # 可运行骨架模板（零依赖）
```

## 在 WorkBuddy 中使用

本仓库同时是一个 WorkBuddy 技能（`lifework-glass-ui`）。安装到 `~/.workbuddy/skills/lifework-glass-ui/` 后，说一句「用未来玻璃风格做个 XX 页」即可自动加载整套设计语言生成页面。

## License

MIT
