# DESIGN.md — SemakSah 查真

> 格式参考 [awesome-design-md](https://github.com/voltagent/awesome-design-md)（Stitch DESIGN.md 扩展格式）。
> 所有数值取自 `index.html` 里的 `<style>` 和 `:root` 变量。改样式时请同步更新本文件。

## 1. Visual Theme & Atmosphere

- **定位**：马来西亚中立的保健品配料表对比工具。"不听代理吹，看配料表说话"。
- **气质**：冷静、克制、可信，像一份清晰的检测报告，不像卖货页面。
- **关键词**：中立 · 证据 · 清楚 · 移动优先 · 低干扰。
- **不要**：营销感（大渐变、促销红、倒计时、闪烁徽章）、品牌化插画、情绪化的颜色。
- 颜色只用来**表达含义**（主操作、风险等级），不用来装饰。

## 2. Color Palette & Roles

所有颜色必须通过 CSS 变量引用，不要在新代码里写死色值。

### 品牌色（唯一主色）

| 变量 | 色值 | 用途 |
|---|---|---|
| `--primary` | `#1a7f6b` | 中性青绿。主按钮、进度条、选中边框、焦点边框、评分条 |
| `--primary-dark` | `#12594b` | Logo、链接、选中文字、按钮按下态 |
| `--primary-light` | `#e6f4f1` | 选中背景、AI 输出框、主提示框、表格分组行 |

### 中性色

| 变量 | 色值 | 用途 |
|---|---|---|
| `--text` | `#1f2937` | 正文 |
| `--text-2` | `#6b7280` | 次要文字、说明、标签 |
| `--line` | `#e5e7eb` | 边框、分隔线、进度条底色 |
| `--bg` | `#ffffff` | 页面和卡片背景 |
| `--bg-soft` | `#f8fafc` | 柔和背景：提示框、导航栏、表头、空状态 |

### 风险等级色（语义色，只用于风险 / 状态）

每个等级都是"深色文字 + 浅色背景 + 边框"三件套：

| 等级 | 文字 / 标签 | 背景 | 边框 | 含义 |
|---|---|---|---|---|
| RED 红 | `--red` `#b91c1c` | `--red-bg` `#fef2f2` | `--red-line` `#fecaca` | 有风险，建议就医或咨询专业人士 |
| ORANGE 橙 | `--orange` `#b45309` | `--orange-bg` `#fffbeb` | `--orange-line` `#fde68a` | 需要注意 / 建议 |
| YELLOW 黄 | `--yellow` `#854d0e` | `--yellow-bg` `#fefce8` | `--orange-line` | 信息不完整、剂量初稿、待核实 |
| GREEN 绿 | `--green` `#166534` | `--green-bg` `#f0fdf4` | `--line` | 基础正常 |

规则：
- 绿色风险色 `--green` **不等于**品牌色 `--primary`，两者不要混用。
- 红色只表示风险，**不能**用于促销、强调或按钮。
- 风险颜色永远搭配文字说明（如 "⚠"、等级标签），不能只靠颜色传达含义。

## 3. Typography Rules

- **字体栈**：`-apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif`。系统字体，不加载网络字体。
- **正文**：16px，行高 1.65。
- **层级**：

| 元素 | 字号 | 字重 / 备注 |
|---|---|---|
| `h1` | 22px | 行高 1.4 |
| `h2` | 18px | |
| `h3` | 16px | |
| Logo | 18px | 700，`--primary-dark` |
| 选项 / 折叠标题 | 15px | 折叠标题 600 |
| 次要文字 `.muted` | 14px | `--text-2` |
| 小字 `.small`、表格 | 13–13.5px | |
| 页脚、标签 chip | 12.5px | `--text-2` |
| 等级标签、徽章 | 12px | 等级标签 700 |

- 加粗只用 600 和 700 两档。
- 输入框字号至少 16px，避免 iOS 自动放大。
- **三语**：中文 `zh`、马来文 `ms`（BM）、英文 `en`。所有界面文字走 `t()` 翻译函数，不能写死在 HTML 里。马来文和英文通常比中文长 30–60%，布局要留出余量，不要依赖固定宽度。

## 4. Component Stylings

### 按钮 `.btn`
- 默认全宽，padding `14px 18px`，圆角 10px，16px / 600。
- **主按钮**：`--primary` 底，白字；按下 `--primary-dark`。
- **次按钮** `.secondary`：白底，`--primary` 边框，`--primary-dark` 文字。
- **幽灵按钮** `.ghost`：白底，`--line` 边框，`--text-2` 文字。
- 禁用：透明度 .45。
- 一行多个按钮用 `.btn-row`（flex，间距 10px，均分）。

### 卡片 `.card` / `.pcard`
- 1px `--line` 边框，圆角 12px，padding 16px，上下间距 12px。
- `.card.soft` 用 `--bg-soft` 底。
- **没有阴影**，层次只靠边框和背景色。
- 产品卡图片区：无图 90px 虚线占位，有图 130px 实线、`object-fit: contain`。

### 风险横幅 `.banner.{red|orange|yellow|green}`
- 圆角 12px，padding 16px，用对应等级的背景 + 边框。
- 顶部是胶囊形等级标签 `.tier-tag`：等级色底、白字、12px / 700、圆角 99px。
- 红色横幅置顶显示。

### 提示 / 理由
- `.notice`：`--bg-soft` 底、圆角 10px、14px `--text-2`；`.notice.primary` 换成品牌浅色。
- `.reason.good`：前缀 "✓ "（`--primary-dark`）。
- `.reason.warn`：前缀 "⚠ "（`--orange`）。
- `.reason.strong-warn`：红色底框。
- `.reason.toppick`：品牌浅色底框。
- `.advice-item`：左侧 3px 色条（默认橙，`.red` 为红）。
- `.dose-draft`：黄色条，标记"剂量初稿、待核实"的数据。

### 选择类
- 选项 `.opt`：白底、1px 边框、圆角 10px；选中后边框 `--primary`、背景 `--primary-light`。单选 / 复选框用 `accent-color: var(--primary)`。
- 可点选 chip `.sel-chip`：胶囊形；`.on` 状态为品牌浅色 + 600。
- 只读 chip `.tag-chip`：胶囊形、`--bg-soft` 底。

### 徽章 `.badge`
胶囊形、12px。`.ok` 绿、`.warn` 红、`.lib` 品牌色（已收录）、`.draft` 橙（草稿）。

### 输入框 `.search-input`
全宽，padding `12px 14px`，16px，圆角 10px；聚焦时只把边框换成 `--primary`，不加光晕。

### 进度 / 评分条
- 问卷进度条高 6px，评分条高 8px；底色 `--line`，填充 `--primary`，全圆角。

### 对比表 `table.cmp`
- 外层包 `.cmp-scroll` 允许横向滚动。
- 1px 边框，单元格 padding `8px 10px`，最小宽度 110px。
- 表头和行标题用 `--bg-soft`；分组行用 `--primary-light` + `--primary-dark` 700。

### 折叠 `details`
1px 边框，圆角 10px；`summary` 15px / 600。

### AI 功能（内测）
- `.ai-box`：品牌浅色底 + `--primary` 边框，顶部白底胶囊 `.ai-badge` 标明是 AI 生成内容。
- 底部 `.ai-foot` 用虚线分隔，放免责说明。
- 加载用 `.ai-skel` 骨架条（呼吸动画 1.2s）。

### 导航
- `nav.topnav`：吸顶，`--bg-soft` 底，可横向滚动。当前项为 `--primary-dark` 600 + 2px `--primary` 下划线。
- 语言切换在导航右侧，当前语言用品牌浅色底。

### 状态
- 空状态 `.empty-state` 和加载 `.loading`：虚线边框、`--bg-soft` 底、居中、14px `--text-2`。

## 5. Layout Principles

- **单栏**，内容最大宽度 **640px**，水平居中。
- 页面 padding `16px 16px 48px`。
- 间距只用这些档位：2 / 4 / 6 / 8 / 10 / 12 / 14 / 16 / 18 / 20 / 28 / 32 / 40px。卡片之间 12px，章节之间（`h2` 上方）28px。
- 圆角三档：**8px**（小框、图片）、**10px**（按钮、输入框、选项、提示）、**12px**（卡片、横幅）；胶囊 99px。
- 品类入口用 `.cat-grid`：`repeat(auto-fit, minmax(180px, 1fr))`，间距 10px。

## 6. Depth & Elevation

- **整站不用 `box-shadow`**。层级靠：1px 边框 → 浅色背景 → 加粗边框（AI 主入口卡 `.ai-hero` 用 2px `--primary`）。
- 唯一叠在内容上方的是吸顶导航（`z-index: 10`）。
- 不用弹窗遮罩。需要更多信息时用 `details` 折叠或跳转视图。

## 7. Do's and Don'ts

**Do**
- 用 CSS 变量取色。
- 每条结论都给出依据（配料、剂量、来源），保持"证据优先"。
- 风险提示用对应的语义色组件，并配文字。
- 草稿 / 未核实数据明确标出（`.dose-draft`、`.badge.draft`）。
- AI 生成内容放进 `.ai-box`，带 AI 标签和免责说明。
- 新增文字同时提供 zh / ms / en 三种翻译。
- 加 `.no-print` 隐藏打印时不需要的元素；卡片和横幅打印时不分页。

**Don't**
- 不加新的品牌色或渐变。
- 不用阴影、毛玻璃、大面积插画。
- 不用红色做按钮或营销强调。
- 不用"最好""必买""神效"这类推销词，也不偏向任何品牌。
- 不引入外部字体、CSS 框架或图标库，保持单文件、轻量。
- 不把颜色当成唯一的信息载体。

## 8. Responsive Behavior

- **移动优先**：640px 单栏在手机上就是全宽，桌面端居中留白，不额外做多栏布局。
- 按钮默认全宽，方便单手点击；可点区域高度至少约 44px。
- 宽内容（对比表、导航）用 `overflow-x: auto` 横向滚动，**页面本身不能横向滚动**。
- 品类网格自动换行（`auto-fit`）。
- 打印：隐藏 `.no-print` 和内测横幅，正文缩到 13px。

## 9. Agent Prompt Guide

给 AI 助手的说明：

> 你在给 SemakSah 查真写界面。这是一个中立的马来西亚保健品配料表对比工具，所有代码在单文件 `index.html` 里，数据在 `data/*.json`。
> 只使用本文件定义的 CSS 变量和现有组件类（`.card`、`.btn`、`.banner`、`.badge`、`.notice`、`.opt`、`table.cmp` 等）。先复用现有类；确实需要新组件时，沿用同样的边框 / 圆角 / 间距规则，并把它补充到本文件第 4 节。
> 唯一主色是 `--primary` 青绿；红、橙、黄、绿只用来表示风险等级。不用阴影、渐变和外部依赖。
> 所有界面文字通过 `t()` 提供中文、马来文、英文三种翻译。
> 语气客观、基于证据，不推销任何产品。

**快速示例**

```html
<div class="card">
  <h3>产品名</h3>
  <p class="muted">持证方 · 规格</p>
  <div class="reason good">剂量达到参考值</div>
  <div class="reason warn">含有咖啡因</div>
  <button class="btn secondary">加入对比</button>
</div>
```
