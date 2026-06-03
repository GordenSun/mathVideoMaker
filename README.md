# mathVideoMaker

一个 Cursor Agent Skill：根据需求**同时生成「数学讲解视频」（Manim 渲染成 MP4）和「交互式网页」（自包含 HTML）**，并把实现细节打磨到位，让能力较弱、甚至没有视觉能力的模型也能稳定产出准确、生动的内容。

参考实现：[ManimCommunity/manim](https://github.com/ManimCommunity/manim)（基于 Manim Community v0.20+）。

## 它能做什么
- **讲解视频**：用 Manim 把一个数学/物理概念的**推导过程**动画化，渲染成 MP4。
- **交互式网页**：自包含单文件 HTML（KaTeX 公式 + canvas 交互），让观众拖参数、即时看变化。
- 二者**共享同一套设计**（概念、记号、参数、配色），互为补充：视频讲明白，网页玩明白。

## 设计要点（为什么这个 Skill 不一样）
- **讲「为什么」**：强制演示推导/证明，而不是只展示结论（含"遮住旁白也能看懂"的硬性自检）。
- **无视觉也能保证质量**：把"看图纠错"换成文字化检查——
  - `SafeScene` 渲染时自动打印 `[layout]` 布局警告（出界/重叠）；
  - `check_text.py` 静态查字体缺字形（防方框 □）；
  - `check_web.py` 静态查网页（公式/id/滑块/canvas/布局/JS 语法）。
- **机械化布局 + 生动化**：安全区 + `fit_content` 防出界；深色配色 + 辉光 + 强调动效；字幕带底衬。

## 效果演示
小学题「□ + 28 = □ × 5」讲解视频：

<video src="https://github.com/GordenSun/mathVideoMaker/raw/main/examples/BoxDivide.mp4" controls muted width="640"></video>

> 若播放器未显示，点这里查看/下载：[examples/BoxDivide.mp4](examples/BoxDivide.mp4)。同一主题还会生成配套的交互式网页（拖动 □ 让天平平衡）。

## 安装
把本仓库的 `skills/math-explainer/` 放到你的项目（或个人目录）的 `.cursor/skills/` 下：

```bash
git clone https://github.com/GordenSun/mathVideoMaker.git

# 方式一：作为项目技能（随项目生效）
cp -R mathVideoMaker/skills/math-explainer  你的项目/.cursor/skills/

# 方式二：作为个人技能（所有项目可用）
cp -R mathVideoMaker/skills/math-explainer  ~/.cursor/skills/
```

## 环境（仅生成视频需要；纯网页可跳过）
macOS / Linux：
```bash
bash .cursor/skills/math-explainer/scripts/setup_manim.sh   # 装 manim + ffmpeg + 系统库 + fonttools
python3 .cursor/skills/math-explainer/scripts/check_env.py  # 体检
```
> macOS 关键依赖：`cairo`、`pango`、`pkg-config`（pip 构建 pycairo 必需，脚本已包含）。公式可选 LaTeX；没有 LaTeX 时用 `Text`/`frow` 即可。

## 用法
在 Cursor 里直接对 Agent 说，例如：
- “做一个视频，讲解勾股定理”
- “给小学生讲解 5×□ = 4×□ + 7”
- “讲 y=n·x² 随 n 变化”

Agent 会读 `SKILL.md` 按阶段流程产出**视频 + 配套网页**，成品放在主题文件夹根目录（`<主题>/<场景名>.mp4` 与 `<主题>/index.html`）。

## 目录结构
```
mathVideoMaker/
├── skills/math-explainer/         # 把它复制到 .cursor/skills/ 下使用
│   ├── SKILL.md                   # 主编排：阶段流程 + 检查关卡
│   ├── references/                # 教学法/分镜、Manim 指南、配方、网页指南
│   ├── scripts/                   # setup / check_env / check_text / check_web / render
│   └── templates/                 # 场景骨架、mathviz 护栏、黄金范例、网页与分镜模板
└── examples/                      # 效果演示视频
```

---

感谢 [LinuxDO](https://linux.do) 社区的支持
