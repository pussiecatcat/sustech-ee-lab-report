# 南科大电子系实验报告 Skill

本 Skill 旨在把南方科技大学电子与电气工程系课程实验和项目的材料，整理成一份可以直接提交的实验报告：读指导书和原始数据，做必要的计算、仿真和绘图，组织分析与正文，排好版，导出 PDF，并在交付前按实际页面检查一遍。

它支持中英文写作，可选电子系官方 Word 模板或其他模板，并按你手上材料的完整程度提供三种工作模式。Claude Code 与 Codex 都能用。

> 这是个人维护的辅助工具，不是院系官方发布，也不代表院系的最新要求。详见[来源、版权与使用范围](#来源版权与使用范围)。

## 能帮你做什么

- **从材料做到成稿。** 给指导书、原始数据和截图，完成数据处理、仿真、绘图、分析、撰写、排版与检查。
- **只写不排，或只排不写。** 手上已有实验结果，只要分析和正文；或者已有成稿，只要排版和导出。
- **按官方模板出稿。** 套用电子系模板的结构与页眉，导出 PDF，可选附可编辑的 Word / LaTeX / Typst 源文件。
- **守住证据链。** 区分实测、理论计算、仿真与示意；不把估计数据当实测，不伪造运行结果，不替你编造实验经历。

## 什么时候适用

适合课程实验报告、项目报告、指导书对应的实验报告，以及已有报告的排版与导出。

不适合通用学术论文、文献综述和数学建模竞赛论文——它们有各自的写作规范与结构，用本 Skill 会套错体裁。

## 怎么用

在 Claude Code 中调用 `/sustech-ee-lab-report`（Codex 中用 `$sustech-ee-lab-report`），附上材料即可：

> 使用 /sustech-ee-lab-report。这里是实验指导书和原始数据，请使用中文、全局模式，不使用模板，交付 PDF 和 Typst 源工程。

> 使用 /sustech-ee-lab-report，只排版这份已有报告，采用官方模板，保留英文正文，仅交付 PDF。

### 首次使用会确认这几项

| 选择 | 选项 |
|---|---|
| 模板 | 电子系官方模板 / 其他指定模板 / 不使用模板 |
| 课程或项目名称 | 显示在页眉的课程名或项目名；用官方模板时必问 |
| 正文语言 | 中文 / 英文 |
| 工作模式 | 排版 / 撰写 / 全局 |
| 交付格式 | 仅 PDF / PDF + Word / PDF + LaTeX / PDF + Typst / Other（自行输入） |

没提前指定的选项，Skill 会在读完材料后一次性问齐，不会边做边打断你。同一门课的后续实验会沿用上次的课程名、模板、语言和模式，只确认这次变化的部分。

可编辑源文件是可选交付物。Other 可以填其他格式或组合；如果交互工具已有自由输入入口，会直接用它。

## 三种模式

| 模式 | 你提供什么 | 它做什么 |
|---|---|---|
| 排版 | 已有正文、公式和图表 | 保持内容含义，整理层级、样式、图表、编号、引用、分页与导出；发现技术疑点单独列出 |
| 撰写 | 指导书及已有实验结果 | 梳理任务、解释结果、组织并撰写正文，做支撑文字的简单核算与图表整理，完成基本排版和导出 |
| 全局 | 指导书、原始数据、代码、截图等任意阶段材料 | 从材料梳理开始，按需完成数据处理、编程或仿真、计算、绘图、分析、撰写、排版及最终检查 |

排版模式不会擅自改动实验数值或重写结论；撰写模式若发现需要系统分析原始数据或新增仿真，会先说明所需工作并确认是否扩大范围。全局模式不逐环节征求许可，会复用可靠的已有成果。

## 交付什么

| 交付格式 | 内容 |
|---|---|
| 仅 PDF | 报告本身，不附可编辑源文件 |
| PDF + Word | 附 `.docx` |
| PDF + LaTeX | 附 `.tex` 源工程及构建所需资源 |
| PDF + Typst | 附 `.typ` 源工程及构建所需资源 |
| Other | 自行指定，例如 Markdown、HTML 或多种源格式 |

全局模式通常还会附上实际用到的分析代码、参数、处理后的数据和图文件。这些实验结果附件与报告源文件分开处理——你可以只要报告、不要任何附件。

## 需要什么环境

Skill 本身只提供工作指引和模板资源，不内置任何编译器。实际能做到哪一步，取决于你机器上装了什么：

- **走官方模板或要 Word 源文件**：需要 Word。DOCX 导出 PDF 通过 Word 自动化完成，不需要手工打开。
- **走 LaTeX 或 Typst**：需要对应的编译器，以及可用的中文字体。
- **做数据处理和绘图**：需要 Python、MATLAB 等。

`SKILL.md` 声明的 `allowed-tools` 是 `Read`、`Write`、`Edit`、`Glob`、`Grep`、`Bash`。其中 `Bash` 用于调用 Typst、LaTeX、Python/MATLAB 和文档转换工具；这份清单不会让本 Skill 自动获得硬件访问、外部上传或破坏性操作的授权。

## 安装

本仓库根目录就是技能目录，包含 `SKILL.md`、`agents/`、`references/` 和 `assets/`。将整个仓库放入个人技能目录，**文件夹名称保持为 `sustech-ee-lab-report`（全小写）**——仓库名是 `SUSTech-EE-Lab-Report`，但技能标识符必须是小写 kebab-case，目录名与 `SKILL.md` 里的 `name` 不一致会导致技能无法加载。下面的命令已经显式指定了目标目录，`git clone` 不会按仓库名自动建目录。

**Claude Code**（技能目录 `~/.claude/skills`），调用语法为 `/sustech-ee-lab-report`：

```powershell
git clone https://github.com/pussiecatcat/SUSTech-EE-Lab-Report.git (Join-Path $env:USERPROFILE '.claude\skills\sustech-ee-lab-report')
```

**Codex**（技能目录 `~/.codex/skills`），调用语法为 `$sustech-ee-lab-report`：

```powershell
$skillRoot = if ($env:CODEX_HOME) { Join-Path $env:CODEX_HOME 'skills' } else { Join-Path $env:USERPROFILE '.codex\skills' }
git clone https://github.com/pussiecatcat/SUSTech-EE-Lab-Report.git (Join-Path $skillRoot 'sustech-ee-lab-report')
```

`agents/openai.yaml`（技能展示名称与默认调用提示）只被 Codex 读取；在 Claude Code 下该文件不起作用，保留无害。其余文件两个环境通用，`SKILL.md` 的 frontmatter 只使用 `name`、`description` 和 `allowed-tools`，三者在两边都有效。

如果目标目录已有同名技能，先保留自己的修改，再决定如何更新。也可以下载仓库 ZIP，解压后将技能目录放入上述位置。

## 文件说明

| 文件 | 用途 |
|---|---|
| [SKILL.md](SKILL.md) | 入口选择、模式边界和基本规则 |
| [agents/openai.yaml](agents/openai.yaml) | 技能展示名称与默认调用提示（仅 Codex 读取） |
| [assets/official-template.docx](assets/official-template.docx) | 可选的官方 Word 模板原件 |
| [references/analysis-and-writing.md](references/analysis-and-writing.md) | 数据处理、计算、绘图和写作方法 |
| [references/layout-and-output.md](references/layout-and-output.md) | 交付选择与 Word、LaTeX、Typst 工作路径 |
| [references/official-template.md](references/official-template.md) | 模板结构、正文嵌在单元格里带来的排版行为、页眉与标题分工、适配方式 |
| [references/reference-lessons.md](references/reference-lessons.md) | 从公开课程报告提炼的可借鉴方法 |
| [references/final-check.md](references/final-check.md) | 内容、结果、实际页面和交付文件检查 |
| [NOTES.md](NOTES.md) | 试跑记录、实测结论与尚未验证的范围 |

## 已验证到什么程度

技能结构和资源链接已检查，「官方模板 → Word → PDF」这条路径做过一次真实材料的端到端试跑，LaTeX 与 Typst 各做过最小文档、多文件工程和文献库的试跑，几条关键结论都带对照实验复现过。

尚未验证：没有用它产出过一份完整长度的课程报告；中文国标之外的引用形式、参考文献去重和多文献类型混排未试跑。完整记录见 [NOTES.md](NOTES.md)。

## 来源、版权与使用范围

- **不是院系发布物。** 本仓库由个人维护，与南方科技大学电子与电气工程系没有隶属或背书关系，也不代表院系的最新要求。
- **官方模板是外部提供的原件。** `assets/official-template.docx` 是《电子系实验报告模版.docx》的原始字节副本，版权归原权利人所有，本仓库不对其主张任何权利。院系对各课程的模板要求可能随时更新，**使用前以课程当次下发的文件为准**；是否使用该模板由每份报告单独选择，不选就不动它。
- **参考报告只取方法。** [references/reference-lessons.md](references/reference-lessons.md) 只从公开的课程报告提炼写作方法，不复制其中的数据、结论、成绩、个人经历或版式。
- **许可证是 MIT，但官方模板除外。** 技能本身（`SKILL.md`、`README.md`、`references/`、`agents/openai.yaml`）按 [LICENSE](LICENSE) 里的 MIT 条款授权。**`assets/official-template.docx` 不在该授权范围内**——它是第三方文件的副本，版权仍归原权利人，此处只作为工作资源提供；要连同它一起再分发，需自行确认是否有权这样做。模板原件按原始字节保留，不要为了这个目的去改动它。

## 参考资料

写作方法参考 [SUSTech EE Course Hero](https://github.com/SUSTech-EE-Course-Hero/SUSTech-EE-Course-Hero) 索引中的公开课程报告，具体见 [reference-lessons.md](references/reference-lessons.md)。只提炼任务与结果对应、参数比较、图文结合和具体调试记录等方法，不机械复制其结果、篇幅、个人经历或版式。
