---
name: lifework-glass-ui
description: LifeWork「未来玻璃」设计系统。深靛蓝宇宙背景 #0A0B1A + 径向极光，紫罗兰→霓虹粉渐变 #7C3AED→#EC4899 做 CTA 与标题、第三色 #06B6D4，文字 #F8FAFC，毛玻璃卡片（bg-white/5 + backdrop-blur + hairline 边框），Inter + JetBrains Mono 字体，Apple 发布会气质，双主题（暗色默认/亮色可切）。当用户要新做页面/落地页/工作台、换肤、或要求「未来玻璃 / 玻璃拟态 / 紫粉青 / LifeWork 风格 / 深色毛玻璃 / 高级感深色 UI」时使用。提供完整设计令牌、组件库、骨架模板，可生成单文件 HTML（原生 CSS/JS，零依赖）。硬规则：图标内联 Lucide 风格 SVG 不用 CDN/Emoji；动画只动 transform/opacity；入场用 IntersectionObserver；prefers-reduced-motion 塌缩为静态；零 em-dash。
agent_created: true
---

# LifeWork 未来玻璃设计系统

为「生活 × 工作」一体化产品打磨出的深色毛玻璃设计语言，源自 `life-desk.html`（双模式工作台）与 `life-work-landing.html`（落地页）。生成任何新页面时直接复用，保证与 LifeWork 产品线视觉一致。

## 设计语言总览

| 维度 | 值 |
|---|---|
| 背景 | 深靛蓝 `#0A0B1A` + 径向极光（紫/粉/青三色低透明度光斑）+ 细网格 |
| 主渐变 | `linear-gradient(115deg, #7C3AED, #EC4899)` — CTA、标题、选中态 |
| 第三色 | `#06B6D4`（青）— 差异化信息、次要高亮 |
| 文字 | `#F8FAFC`（主）/ 半透明灰（次） |
| 玻璃卡 | `rgba(255,255,255,.055)` 底 + `backdrop-filter: blur(16px) saturate(150%)` + 1px hairline 边框 |
| 字体 | `Inter`（sans）/ `JetBrains Mono`（mono，数字与代码）+ 系统回退 |
| 圆角 | 卡片 16-18px、按钮 10-12px、胶囊 999px — 全站一致 |
| 动效 | `cubic-bezier(.22,1,.36,1)` 指数缓动，仅 transform/opacity |

## 使用流程

1. 读 `references/design-tokens.css` 取完整 CSS 变量（双主题）。
2. 按 `references/components.md` 的组件拼装页面结构（玻璃卡/渐变 CTA/分段开关/统计卡/表单/下拉等）。
3. 需要快速起步时复制 `references/template.html` 骨架，往里填内容。
4. 交付前自查：`零 em-dash`、图标全部内联 SVG、动画只动 transform/opacity、双主题均可用、移动端无横向溢出、触控目标 ≥ 44px。

## 硬规则（必须遵守）

- **图标**：手写 Lucide 风格 SVG（24 viewBox、stroke 1.8-2、stroke-linecap/join round），禁止 CDN、禁止 Emoji 当图标。
- **文案**：零 em-dash（U+2014）与 en-dash（U+2013）；中文语境用「——」也避免，用冒号/破折号简化。
- **动画**：hover/入场/切换只动 `transform` 与 `opacity`；入场动画统一 `IntersectionObserver` 加 `.in` 类 + `--d` 错峰延迟。
- **可访问性**：`@media (prefers-reduced-motion: reduce)` 把所有动画塌缩为静态；触控目标 ≥ 44px（移动端）。
- **布局**：桌面宽屏自适应、移动端断点 640px；fixed 浮层（下拉/弹窗）渲染到 `document.body` 末尾，避免 `backdrop-filter` 祖先破坏 fixed 定位（containing block 陷阱）。
- **SVG 布局**：SVG 作为 grid/flex item 不可靠，一律包一层 div + 显式 track 宽度（如 `grid-template-columns:84px 1fr`）。
- **主题**：暗色为默认；亮色主题通过 `html[data-theme="light"]` 覆盖变量；`color-scheme` 跟随主题（原生控件适配）；`select option` 显式设背景色防白底白字。
- **语义色**：红/橙/绿做状态色时用「半透明 tint + 浅色文字」组合（如 `rgba(239,68,68,.16)` 底 + `#F87171` 字），双主题都可读，禁硬编码浅色底。

## 文件结构

```
lifework-glass-ui/
├── SKILL.md                     # 本文件
└── references/
    ├── design-tokens.css        # 双主题完整 CSS 变量（暗色默认/亮色切换）
    ├── components.md            # 组件库：结构 + 样式 + 交互要点
    └── template.html            # 最小可运行骨架（含全部变量与基础组件演示）
```

## 适用场景

- 新建产品落地页、官网（配合 Impeccable 规范做版式）
- 新建数据工作台 / 仪表盘 / 管理后台
- 为既有单文件 HTML 应用换肤成未来玻璃风格
- 生成营销页、介绍页、工具页（单文件交付）
