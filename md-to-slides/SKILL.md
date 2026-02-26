---
name: md-to-slides
description: >
  Convert a Markdown outline document into a multi-page HTML presentation (slideshow).
  Use this when the user wants to create a visually polished, browser-based slide deck
  from a structured Markdown file. Supports themed styling (e.g., cyberpunk, corporate,
  minimalist), Chart.js data visualization, keyboard/touch navigation, 16:9 aspect ratio,
  iframe-based page architecture, and per-slide layout planning.
---

# MD-to-Slides: Markdown 大纲 → HTML 幻灯片演示文稿

## When to Use

### ✅ 适用场景
- 用户提供了一份 **Markdown 格式的大纲/提纲/文章**，希望生成可演示的幻灯片
- 用户要求生成 **HTML 格式的演示文稿**（而非 PowerPoint）
- 用户指定了视觉风格主题（赛博朋克、极简、商务等）
- 需要包含 **数据图表、动画、键盘翻页** 等交互能力
- 需要 **16:9 画幅、无滚动条、严格控制单页内容量**

### ❌ 不适用场景
- 用户需要 `.pptx` / `.key` 等原生幻灯片格式（应使用 office 工具链）
- 输入不是结构化大纲，而是散文/长文（需先让用户提炼大纲）
- 纯文字打印材料，不需要视觉呈现

---

## Instructions

### 阶段一：大纲研读与分页规划（Planning）

1. **深入研读大纲内容**
   - 逐节分析 Markdown 大纲的层级结构（H1/H2/H3、列表、引用、表格）
   - 识别内容类型：标题、核心概念、人名、数据、金句、对比、列表、表格
   - 标记内容密度——单节内容过多时必须拆分为多页

2. **确定总页数与每页定位**
   - 根据大纲节数初定页数（通常 1 节 = 1 页）
   - 对内容过重的节做拆分（如"两个发现"拆为两页）
   - 每页分配一个明确的 **页面定位**（封面页 / 问题引入页 / 对比页 / 数据冲击页 / 方法论页 / 总结页 等）

3. **为每页生成布局规划 MD 文档**（存入 `slides/` 子目录）

   每个规划文档必须包含以下结构：

   ```markdown
   # 第N页：[页面名称] 布局规划

   ## 页面定位
   [一句话描述该页的作用和视觉目标]

   ## 布局方案
   - [描述空间划分：左右分栏 / 上下结构 / 三列卡片 / 全屏居中 等]
   - [描述背景装饰元素]

   ## 内容层级
   ### 顶部标签
   - [页码/分类标签]

   ### 主标题（字号/颜色/特效）
   - [标题文字]

   ### 核心内容区
   - [详细内容，标注哪些是高亮、引用、数据、标签]

   ### 底部/补充
   - [金句/脚注/提示]

   ## 特效
   - [入场动画、数据动画、发光效果等]
   ```

4. **内容量控制铁律**
   - 每页内容必须严格控制在 **16:9 画幅** 内，不允许出现滚动条
   - 如果放不下，**必须拆分页面**，绝不压缩字号到不可读
   - 正文字体不小于 13px，标题不小于 18px
   - 单页文字总量建议不超过 120 个汉字（不含标签和装饰文字）

---

### 阶段二：风格系统设计（Theming）

5. **根据用户指定主题确定配色方案**

   预置主题参考（用户未指定时询问）：

   | 主题 | 背景 | 主色 | 强调色 | 辅助色 | 字体风格 |
   |------|------|------|--------|--------|----------|
   | **赛博朋克** | `#050510` 深空 | `#00f5ff` 霓虹青 | `#ff006e` 品红 | `#7b2fff` 紫 | Orbitron + 思源黑体 |
   | **极简商务** | `#ffffff` 白 | `#1a1a2e` 深蓝 | `#e94560` 红 | `#0f3460` 靛 | Inter + 思源宋体 |
   | **暗夜学术** | `#1a1a2e` 深蓝 | `#e8e8e8` 浅灰 | `#ffd700` 金 | `#4a90d9` 蓝 | Source Serif + 思源黑体 |
   | **科技未来** | `#0c0c1d` 深紫 | `#00d4ff` 电蓝 | `#ff6b35` 橙 | `#00ff88` 绿 | Space Grotesk + 思源黑体 |

6. **生成统一 CSS 文件** (`css/style.css`)

   必须包含以下模块：
   - CSS 变量系统（`:root` 声明所有颜色/字体/阴影）
   - 背景系统（网格线/扫描线/光晕/粒子等装饰）
   - 标题层级系统（主标题/副标题/节标题/页码标签，各级字号差异明显）
   - 卡片系统（不同主色的卡片边框+发光+渐变背景）
   - 文字高亮系统（霓虹发光/强调色/引用块）
   - 标签/Badge 系统
   - 表格样式
   - 列表样式
   - 动画关键帧库（fadeInUp / fadeInLeft / fadeInRight / slideInDown / neonPulse / expandWidth / scanLight 等）
   - 动画延迟工具类（`.anim-delay-1` ~ `.anim-delay-6`）
   - 分隔线/装饰元素
   - 角标装饰

---

### 阶段三：页面生成（Building）

7. **生成每个子页面 HTML**（存入 `pages/` 目录）

   每个子页面遵循以下结构模板：

   ```html
   <!DOCTYPE html>
   <html lang="zh-CN">
   <head>
     <meta charset="UTF-8">
     <title>[幻灯片标题] - [页面名]</title>
     <link rel="stylesheet" href="../css/style.css">
     <style>
       /* 本页专属样式（布局、特殊动画等）*/
     </style>
   </head>
   <body>
     <div class="slide">
       <!-- 背景层 -->
       <div class="cyber-grid"></div>
       <div class="scan-line"></div>
       <div class="corner-tl"></div>
       <div class="corner-br"></div>

       <!-- 页头：页码标签 + 分隔线 -->
       <div class="slide-header">
         <span class="page-tag">NN / SECTION_NAME</span>
         <div class="divider" style="flex:1"></div>
       </div>

       <!-- 主内容区 -->
       <div class="content-area">
         <!-- 根据布局规划排布 -->
       </div>
     </div>

     <script>
       // 本页专属动效（计数器、图表等）
     </script>
   </body>
   </html>
   ```

8. **页面类型与布局模式对照表**

   根据页面定位选择对应布局：

   | 页面定位 | 推荐布局 | 关键元素 |
   |----------|----------|----------|
   | 封面页 | 全屏垂直居中 | 大标题渐变发光 + 副标题 + 装饰光晕 |
   | 问题引入页 | 左装饰 + 右文字 | 大号符号(?) + 引用块 |
   | 对比页 | 左右双栏 + 中间 VS | 两张对比卡片 + 数据高亮 |
   | 数据冲击页 | 左数据 + 右图表 | Chart.js 图表 + 大号数据字 |
   | 多要素展示页 | 三列卡片 | 等宽卡片 + 不同色系 |
   | 概念剖析页 | 上下双卡 + 侧表格 | 标签组 + 对比表 + 进度条 |
   | 方法论/建议页 | 左引用 + 右列表 | 名人引用卡片 + 编号建议列表 |
   | 总结页 | 三区域 + 底部金句 | 否定项(删除线) + 要素卡片 + 大号金句 |

9. **数据可视化规则**
   - 遇到数值对比数据 → 使用 **Chart.js**（CDN 引入）生成柱状图/雷达图
   - 遇到比例/占比数据 → 使用 CSS 进度条动画
   - 遇到大数字 → 使用 **计数器动画**（JS setInterval 递增）
   - 图表配色必须使用主题色变量
   - 图表背景透明，网格线极淡

10. **动效规则**
    - 每页入场动画采用 `animation: fadeInXxx 0.6~0.8s ease [delay] both`
    - 不同元素用递增的 delay（0.2s 间隔）营造依次入场效果
    - 核心标题/数据可加 `neonPulse` 持续发光动画
    - 横幅/条块可加 `scanLight` 扫光效果
    - 避免过度动画——每页持续动画元素不超过 2 个

---

### 阶段四：主入口与导航系统（Navigation）

11. **生成 `index.html` 主入口文件**

    必须实现以下功能模块：

    **a) 16:9 响应式容器**
    ```javascript
    // 根据窗口尺寸计算最大 16:9 区域
    function resizeFrame() {
      const vw = window.innerWidth, vh = window.innerHeight;
      let w, h;
      if (vw / vh > 16/9) { h = vh * 0.96; w = h * 16/9; }
      else { w = vw * 0.97; h = w * 9/16; }
      frameWrap.style.width = w + 'px';
      frameWrap.style.height = h + 'px';
    }
    ```

    **b) iframe 页面加载**
    - 维护 slides 数组：`['pages/01_cover.html', 'pages/02_xxx.html', ...]`
    - 切换时先显示过渡遮罩（`opacity 0→1→0`，总时长 ~400ms）

    **c) 键盘控制**

    | 按键 | 功能 |
    |------|------|
    | `→` / `↓` / `Space` / `PageDown` | 下一页 |
    | `←` / `↑` / `PageUp` | 上一页 |
    | `Home` | 跳到首页 |
    | `End` | 跳到末页 |
    | `F` | 切换全屏 |
    | `Escape` | 退出全屏 |

    **d) 辅助交互**
    - 鼠标滚轮翻页（带 600ms 冷却防抖）
    - 触摸屏左右滑动翻页（滑动距离 > 50px 触发）
    - 底部 2px 进度条
    - 右下角小型导航按钮（上一页 / 页码 / 下一页 / 全屏）
    - 首次加载 3.5 秒后自动消失的快捷键提示 toast

---

### 阶段五：质量检查清单（QA Checklist）

12. **逐项检查**

    - [ ] 所有页面无纵向滚动条（`overflow: hidden`）
    - [ ] 所有文字在 16:9 画幅内完整可见
    - [ ] 正文字体 ≥ 13px，标题 ≥ 18px
    - [ ] 图表可正常渲染（Chart.js CDN 可达）
    - [ ] 键盘翻页、全屏功能正常
    - [ ] 页码指示器正确显示（01/NN 格式）
    - [ ] 进度条比例正确
    - [ ] 过渡动画流畅、无闪烁
    - [ ] CSS 变量统一，无硬编码颜色
    - [ ] 所有中文字体回退链正确

---

## Examples

### 输入示例
```
用户：帮我把这个 markdown 大纲做成 HTML 幻灯片，风格要赛博朋克

[附带一份 .md 大纲文件，包含 8 个 H2 节]
```

### 输出结构示例
```
project-root/
├── index.html                 # 主入口（iframe容器+导航）
├── css/
│   └── style.css              # 统一主题样式
├── pages/
│   ├── 01_cover.html          # 封面
│   ├── 02_question.html       # 问题引入
│   ├── 03_comparison.html     # 对比展示
│   ├── 04_data.html           # 数据图表
│   ├── ...
│   └── 08_conclusion.html     # 总结
└── slides/
    ├── 01_cover.md            # 封面布局规划
    ├── 02_question.md         # 问题页布局规划
    └── ...                    # 每页一个规划文档
```

### 执行流程示例
```
Phase 1 → 研读大纲，输出："共规划 8 页，各页定位为：封面/问题/对比/数据1/数据2/剖析/方法论/总结"
Phase 2 → 生成 slides/*.md（每页布局规划）
Phase 3 → 生成 css/style.css（赛博朋克主题）
Phase 4 → 逐个生成 pages/*.html
Phase 5 → 生成 index.html（导航系统）
Phase 6 → 输出使用说明
```

---

## References

### 外部依赖
- **Chart.js** (v4.4+): `https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js`
- **Google Fonts**: Orbitron / Noto Sans SC / Inter / Source Serif（按主题选用）

### 本地服务器启动方式（iframe 需要）
```bash
# 方式 1：VS Code Live Server 插件
# 方式 2：
npx serve ./project-root
# 方式 3：
python -m http.server 8080 --directory ./project-root
```

---

## Guardrails

1. **内容溢出保护**：任何单页内容超过 16:9 可视区域时，必须拆分为多页，严禁添加滚动条或缩小字号至不可读。
2. **CDN 降级**：若用户声明无法联网，Chart.js 图表改为纯 CSS 柱状图/进度条实现。
3. **无大纲时拒绝执行**：如果用户未提供结构化 Markdown 大纲，应主动询问或要求先提供大纲，不猜测内容。
4. **主题确认**：如果用户未指定视觉风格，主动列出可选主题供用户选择，不默认执行。
5. **文件路径确认**：生成前确认用户期望的输出目录路径，避免覆盖已有文件。
