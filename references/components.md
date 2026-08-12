# 未来玻璃 · 组件库

所有组件基于 `design-tokens.css` 的变量。结构示例为原生 HTML + CSS，可直接复制进单文件页面。

## 1. 毛玻璃卡片

```html
<div class="glass" style="padding:20px">
  <div class="eyebrow">本月支出</div>
  <div class="num" style="font-size:27px;margin:6px 0 4px;color:var(--red)">¥4,320</div>
  <div class="bar"><i style="width:86%;background:var(--grad)"></i></div>
</div>
```
```css
.eyebrow{font-size:11px;letter-spacing:.12em;text-transform:uppercase;color:var(--ink-3)}
.bar{height:5px;border-radius:99px;background:var(--line-soft);overflow:hidden}
.bar i{display:block;height:100%;border-radius:99px}
```

## 2. 渐变 CTA / 分段开关

```html
<button class="btn-primary">立即开始</button>

<div class="seg">
  <button class="on" data-v="life">生活</button>
  <button data-v="work">工作</button>
</div>
```
```css
.seg{display:inline-flex;padding:3px;border-radius:12px;background:var(--panel);border:1px solid var(--line);gap:2px}
.seg button{font-size:13px;padding:6px 14px;border-radius:9px;border:0;background:transparent;color:var(--ink-2);cursor:pointer;transition:all var(--ease) var(--d)}
.seg button.on{background:var(--grad);color:#fff;font-weight:600}
```
交互：点击切换 `.on` 到被点按钮；`data-v` 作为模式值，由 JS 读取执行切换。

## 3. 统计卡（看板）

```html
<div class="stats">
  <div class="stat"><div class="k">今日到期</div><div class="v">2</div><div class="n">未完成 · 今天要交</div></div>
  <div class="stat hot"><div class="k">逾期任务</div><div class="v">1</div><div class="n">尽快清掉</div></div>
  <div class="stat"><div class="k">完成率</div><div class="v">71<span class="u">%</span></div><div class="n">共 7 项</div></div>
</div>
```
```css
.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:10px}
.stat{background:var(--panel-2);border:1px solid var(--line);border-radius:14px;padding:11px 14px}
.stat .k{color:var(--ink-3);font-size:11.5px;letter-spacing:.3px}
.stat .v{color:var(--ink);font-size:23px;font-weight:700;margin-top:3px;line-height:1.1;font-family:var(--mono)}
.stat .v .u{font-size:12px;color:var(--ink-3)}
.stat .n{color:var(--ink-3);font-size:11px;margin-top:4px}
.stat.hot .v{color:var(--red)}
@media(max-width:640px){ .stats{grid-template-columns:repeat(2,1fr);gap:8px} }
```

## 4. 指数环（SVG，零依赖）

```js
function ring(pct, size, color){
  var r=(size-8)/2, c=2*Math.PI*r, off=c*(1-Math.min(100,Math.max(0,pct))/100);
  return '<svg viewBox="0 0 '+size+' '+size+'" style="width:'+size+'px;height:'+size+'px">'+
    '<circle cx="'+size/2+'" cy="'+size/2+'" r="'+r+'" fill="none" stroke="var(--line-soft)" stroke-width="8"/>'+
    '<circle cx="'+size/2+'" cy="'+size/2+'" r="'+r+'" fill="none" stroke="'+color+'" stroke-width="8" stroke-linecap="round" '+
      'stroke-dasharray="'+c.toFixed(1)+'" stroke-dashoffset="'+off.toFixed(1)+'" transform="rotate(-90 '+size/2+' '+size/2+')"/>'+
    '<text x="'+size/2+'" y="'+(size/2+7)+'" text-anchor="middle" font-family="var(--mono)" font-size="'+(size*0.27)+'" font-weight="700" fill="var(--ink)">'+pct+'%</text>'+
    '<text x="'+size/2+'" y="'+(size/2+22)+'" text-anchor="middle" font-size="10" fill="var(--ink-3)">完成率</text></svg>';
}
```
注意：SVG 作为布局 item 时包一层 `<div style="flex-shrink:0">`，grid 用 `grid-template-columns:84px 1fr` 固定宽度。

## 5. 表单控件

```css
input, select, textarea{
  background:var(--panel-2); border:1px solid var(--line); border-radius:10px;
  color:var(--ink); font-size:14px; padding:9px 12px; min-height:40px;
  transition:border-color var(--ease) var(--d);
}
input:focus, select:focus, textarea:focus{ outline:none; border-color:var(--violet) }
/* 关键：option 显式设背景，防暗色下白底白字 */
select option{background:var(--paper);color:var(--ink)}
select option:hover, select option:checked{background:var(--panel-2)}
/* label 行 */
.f{display:grid;gap:5px}
.f span{font-size:12px;color:var(--ink-3)}
```

## 6. 自定义玻璃下拉（替代原生 select 的混搭高亮）

原生 `<option>` 的系统高亮色（选中项浅灰底黑字）无法被 CSS 覆盖，暗色下会与深色列表混搭。方案：原生 select 隐藏保留，自绘列表渲染到 `document.body` 末尾。

要点（详见 life-desk.html 的 csel v2 实现）：
1. `.csel` 包按钮 + `.csel-list`（fixed 定位到 body，`position:fixed` 基于按钮 `getBoundingClientRect()`，z-index 999）。
2. **必须渲染到 body 末尾**：`backdrop-filter` 祖先会把 fixed 变成相对该祖先（containing block 陷阱），列表坐标全错。
3. 选中后回写 `select.selectedIndex` 并 `dispatchEvent(new Event('change',{bubbles:true}))`，现有 JS 零改动。
4. 关闭：点击外部 / Esc / resize；滚动关闭需 150ms 防抖（排除聚焦滚动误伤），且排除 `.csel-list` 内部滚动。
5. MutationObserver 监听 body 自动包装动态 select。

## 7. 侧边栏导航（工作台）

```css
.sidebar{width:224px;flex:0 0 224px;background:var(--panel);backdrop-filter:blur(16px);
  border-right:1px solid var(--line);display:flex;flex-direction:column}
.nav{padding:12px 10px;flex:1;overflow-y:auto}
.nav-i{display:flex;align-items:center;gap:10px;padding:9px 11px;border-radius:10px;
  color:var(--ink-2);font-size:13.5px;background:none;border:0;cursor:pointer;width:100%}
.nav-i:hover{background:var(--panel-2);color:var(--ink)}
.nav-i.on{background:var(--grad);color:#fff;box-shadow:var(--sh-glow)}
.nav-bdg{margin-left:auto;background:var(--red-l);color:var(--red);font-size:11px;
  padding:1px 7px;border-radius:20px;min-width:20px;text-align:center}
```

## 8. Toast / 通知

```css
#toast{position:fixed;left:50%;transform:translateX(-50%) translateY(-12px);top:16px;z-index:999;
  background:var(--panel-2);border:1px solid var(--line);backdrop-filter:blur(14px);
  border-radius:12px;padding:10px 18px;color:var(--ink);font-size:13.5px;
  opacity:0;pointer-events:none;transition:opacity .3s var(--ease),transform .3s var(--ease)}
#toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
```

## 9. 弹窗

```css
.mask{position:fixed;inset:0;background:rgba(4,6,20,.55);backdrop-filter:blur(6px);z-index:100;
  display:flex;align-items:center;justify-content:center}
.modal{width:min(480px,calc(100vw-32px));background:var(--paper-2);border:1px solid var(--line);
  border-radius:var(--r-lg);box-shadow:var(--sh);overflow:hidden}
.mh{padding:16px 18px;border-bottom:1px solid var(--line)}
.mb{padding:16px 18px;max-height:60vh;overflow-y:auto}
.mf{padding:14px 18px;border-top:1px solid var(--line);display:flex;justify-content:flex-end;gap:8px}
```
注意：`.mask` 有 `backdrop-filter`，弹窗内 fixed 浮层（如自定义下拉）必须渲染到 body。

## 10. 图标（Lucide 风格内联 SVG）

```html
<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"
     stroke-linecap="round" stroke-linejoin="round">
  <path d="M20 6L9 17l-5-5"/>  <!-- 对勾 -->
</svg>
```
常用：对勾 `M20 6L9 17l-5-5`、加号 `M12 5v14M5 12h14`、太阳（圆 r4 + 8 条射线）、月亮 `M21 12.8A9 9 0 1 1 11.2 3a7 7 0 0 0 9.8 9.8z`、齿轮（圆 r3 + 齿形 path）、星星 `M12 2l3 6.6 7 .8-5.2 4.8 1.4 6.9L12 17.8 5.8 21l1.4-6.9L2 9.4l7-.8z`。

## 11. 双主题切换

```js
function toggleTheme(){
  var cur = document.documentElement.getAttribute('data-theme') || 'dark';
  document.documentElement.setAttribute('data-theme', cur === 'dark' ? 'light' : 'dark');
}
// 初始：读取 localStorage 或默认 dark
```
CSS 层面只需 `html[data-theme="light"]` 覆盖变量（见 design-tokens.css），组件自动适配。
