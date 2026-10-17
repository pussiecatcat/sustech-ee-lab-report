# 试跑记录与验证范围

本文件记录这个 Skill 实际验证过什么、结论从哪来、以及哪些还没验证。技能本身的行为规范在 [SKILL.md](SKILL.md)，使用说明在 [README.md](README.md)，这里只放开发与验证记录。

试跑的源文件留在本地 `work/` 目录，已被 `.gitignore` 忽略，不随仓库分发。

## 第一轮：真实材料端到端（2026-10-16）

以 EE323 Lab 2（离散时间系统）的真实材料完成了一次端到端试跑：从指导书和原始数据出发，经 MATLAB 计算与绘图、撰写、官方模板排版，到用 Word 导出 PDF 并逐页检查，产出 13 页英文报告。这次试跑验证了「官方模板 → Word → PDF」这条路径，并得到两条结论：

- 官方模板把整篇正文放在一个跨列的合并单元格里，Word 在该结构中不遵守 `w:tblHeader` 与 `w:keepNext`，会带来表头不重复、标题留在页脚、表格从表头后断开等问题；成因、对策与「不要用硬分页修」的理由见 [references/official-template.md](references/official-template.md)。
- 图的可读性必须按印制尺寸核算，不能只看源图是否清晰：半栏插入 7 in 宽的源图，等效字号只有 3 pt 左右；核算公式与重画源图的取舍见 [references/analysis-and-writing.md](references/analysis-and-writing.md)。

## 第二轮：对照组与两条输出路径（2026-10-17）

### 一、给上一轮的核心结论补上对照组

上一轮说「正文嵌在合并单元格里会让 Word 忽略 `w:tblHeader`」，但那只是观察，没有排除其他解释。这一轮做了对照复现：同样一张 45 行表格、同样在首行写入 `w:tblHeader`（已确认该属性确实存在于保存后的 XML 里，不是没写进去），放进官方模板的合并单元格时跨页后表头不重现，放在普通文档顶层时表头正常重现。两次唯一的差异就是嵌套与否，因此嵌套结构确实是原因，该结论成立。

顺带确认模板的 `styles.xml` 里连 `Table Grid` 都没有，用 python-docx 直接按名字取表样式会报 `KeyError`。

### 二、补齐两条输出路径

第一轮用内容完全相同的短篇（标题 + 中英混排 + 编号公式 + 插图 + 表格 + 结论），第二轮加了多文件工程、bib 文献库和一张跨页长表，版式一律按官方模板的几何设置（A4，上下 25.4 mm、左右 31.75 mm）。

- **Typst 通过。** 字体写成逐字形回退的列表即可（拉丁用 Times、汉字落到微软雅黑），导出时字型全部嵌入。`#bibliography(..., style: "ieee")` 不需要外部工具链就能生成 [1][2][3] 式引用；但该 CSL 样式会按文档语言本地化（输出「3 版」「卷 61, 期 6, 页 692--702」并用《》包期刊名），课程严格要求英文 IEEE 版式时要核对渲染结果。
- **LaTeX 通过，ctexart 有四个坑。** `10.5pt` 选项被静默忽略，五号必须写成 `zihao=5`（ctex 的 `zihao` 只认 `5`、`-4`、`false`，写 `-5` 直接报错）；中文没有可用斜体字形，`\emph{}` 静默退化为正体；同样内容 ctexart 比 Typst 多占一页；浮动体会把图推进参考文献页。
- **多文件 + biber 通过。** `\input` 分节、图放子目录、`biblatex`/`biber` 都正常，引用解析为 [1][2][3] 并生成参考文献；`latexmk -xelatex main.tex` 一条命令跑完整序列，不要手工漏跑轮次，漏跑产生的 `undefined references` 是中间态而非错误。
- **浮动体会把图推进参考文献页。** 实测正文第 1 页写着「如图 1 所示」，图却漂到第 3 页并落在参考文献标题下面。缩图（13.8 cm → 8 cm）后题注回到引用处且页数不变；`float` 的 `[H]` 会多出一页并留半页空白，`placeins` 的 `\FloatBarrier` 无效。页数受限时不要指望浮动体参数，先减内容。
- **绘图中文会静默丢字。** matplotlib 默认字体没有汉字字形，中文标签变成空框且只报 warning。回退到 SimHei 时还要关掉 `unicode_minus`。见 [references/analysis-and-writing.md](references/analysis-and-writing.md)。
- **DOCX → PDF 的自动化前提。** 这台机器没有 LibreOffice，Word COM 是唯一可行的自动导出方式，已实测可用，不需要手工打开 Word。

各项细节见 [references/layout-and-output.md](references/layout-and-output.md)。

## 仍未验证的范围

- 还没有用它产出过一份完整长度的课程报告。
- 引用只测了数字顺序制和中文国标（`gbt7714`），参考文献去重与多文献类型混排未试跑。
- 上一轮提到的 `w:keepNext` 被忽略、标题留在页脚、H1 把整行推到下一页这几条，本轮只复现了 `w:tblHeader` 这一条。
- 中文字体只验证了宋体与微软雅黑两组，其他字体未逐一核对。
