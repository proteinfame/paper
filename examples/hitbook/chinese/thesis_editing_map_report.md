# thesis.pdf 可编辑文件对应关系报告

生成时间：2026-05-06  
项目目录：`/home/xry/paper/20260505/hithesis-3.1e/examples/hitbook/chinese`  
目标 PDF：`thesis.pdf`

## 1. 总体结论

`thesis.pdf` 不是直接编辑的文件。它由 `thesis.tex` 作为总入口装配，经 `hithesisbook.cls`、`hithesisbook.cfg`、`hithesis.sty` 控制格式后编译生成。

主要可编辑入口如下：

| 内容类型 | 优先编辑文件 | 说明 |
|---|---|---|
| 论文结构、章节装配顺序 | `thesis.tex` | 控制封面、目录、正文、后置部分是否出现以及出现顺序 |
| 封面信息、中文摘要、英文摘要、关键词 | `front/cover.tex` | `\hitsetup` 和 `cabstract`、`eabstract` 环境 |
| 物理量名称及符号表 | `front/denotation.tex` | 表格内容 |
| 正文第 1 章 | `body/regu.tex` | 研究生学位论文撰写规范 |
| 正文第 2 章 | `body/introduction.tex` | 示例文档，含图片、表格、公式、索引、捐助页 |
| 正文第 3 章 | `body/recruit.tex` | 课题组招生招聘内容 |
| 结论 | `back/conclusion.tex` | `conclusions` 环境 |
| 参考文献数据 | `reference.bib` | 文献条目；样式由 `thesis.tex` 中 `\bibliographystyle{hithesis}` 控制 |
| 附录 | `back/appA.tex` | 当前实际启用的附录 |
| 创新性成果 | `back/publications.tex` | `publication` 环境 |
| 索引页 | `back/ceindex.tex`，正文中的 `\sindex` 命令 | 索引页本身和索引项来源分离 |
| 原创性声明和使用权限 | `front/cover.tex`，`hithesisbook.cfg`，`hithesisbook.cls` | 声明页由模板宏 `\authorization` 自动生成 |
| 致谢 | `back/acknowledgements.tex` | `acknowledgements` 环境 |
| 个人简历 | `back/resume.tex` | `resume` 环境 |
| 图片资源 | `figures/`、根目录 EPS 文件 | 正文图片主要来自 `figures/golfer.eps`、`zfb.eps` |

编译日志显示当前输出为 50 页：`thesis.log` 中 `Output written on thesis.xdv (50 pages, 772932 bytes).`

## 2. 总入口与装配关系

`thesis.tex` 的有效装配顺序如下：

| PDF 区段 | 入口命令 | 位置 | 作用 |
|---|---|---:|---|
| 文档类与模板选项 | `\documentclass[fontset=fandol,type=bachelor,campus=harbin]{hithesisbook}` | `thesis.tex:12` | 控制本科/校区/字体等全局行为 |
| 加载自定义宏包 | `\usepackage{hithesis}` | `thesis.tex:113` | 加载示例自定义命令、绘图/算法等扩展 |
| 图片搜索目录 | `\graphicspath{{figures/}}` | `thesis.tex:115` | 未带路径的图片优先从 `figures/` 查找 |
| 前置部分开始 | `\frontmatter` | `thesis.tex:118` | 封面、摘要、目录等 |
| 封面与摘要数据 | `\input{front/cover}` | `thesis.tex:119` | 读取封面字段、摘要、关键词 |
| 生成封面和摘要 | `\makecover` | `thesis.tex:120` | 模板宏生成封面、中文摘要、英文摘要 |
| 物理量表 | `\input{front/denotation}` | `thesis.tex:121` | 读取物理量名称及符号表 |
| 目录 | `\tableofcontents` | `thesis.tex:122` | 根据 `.toc` 自动生成 |
| 正文开始 | `\mainmatter` | `thesis.tex:123` | 阿拉伯页码正文 |
| 第 1 章 | `\include{body/regu}` | `thesis.tex:124` | 写作规范 |
| 第 2 章 | `\include{body/introduction}` | `thesis.tex:125` | 示例文档 |
| 第 3 章 | `\include{body/recruit}` | `thesis.tex:126` | 招生招聘 |
| 后置部分开始 | `\backmatter` | `thesis.tex:127` | 结论、文献、附录等 |
| 结论 | `\include{back/conclusion}` | `thesis.tex:128` | 结论页 |
| 参考文献样式 | `\bibliographystyle{hithesis}` | `thesis.tex:132` | 参考文献排版样式 |
| 参考文献数据 | `\bibliography{reference}` | `thesis.tex:158` | 使用 `reference.bib` |
| 附录开始 | `\begin{appendix}` | `thesis.tex:159` | 附录编号切换 |
| 附录内容 | `\input{back/appA.tex}` | `thesis.tex:160` | 当前启用附录 |
| 创新性成果 | `\include{back/publications}` | `thesis.tex:162` | 成果列表 |
| 答辩决议表 | `\resolution` | `thesis.tex:164` | 模板宏生成，仅博士条件下生效 |
| 索引 | `\include{back/ceindex}` | `thesis.tex:167` | 中文/英文索引 |
| 原创性声明和使用权限 | `\authorization` | `thesis.tex:169` | 模板宏生成 |
| 致谢 | `\include{back/acknowledgements}` | `thesis.tex:172` | 致谢 |
| 个人简历 | `\include{back/resume}` | `thesis.tex:173` | 个人简历 |

注意：`thesis.aux`、`thesis.toc`、`thesis.bbl`、`thesis.ind`、各目录下 `.aux` 等是编译生成文件，不应作为主要编辑入口。

## 3. PDF 目录到可编辑文件映射

以下页码以 `thesis.toc` 中显示的 PDF 页码为准。封面页不在目录中。

### 3.1 前置部分

| PDF 部分 | PDF 页码 | 可编辑文件 | 关键位置 | 说明 |
|---|---:|---|---:|---|
| 封面、第 1/第 2 封面 | 封面页，无目录页码 | `front/cover.tex` | `3-66` | 标题、作者、导师、学号、学院、日期、关键词等字段 |
| 中文摘要 | I | `front/cover.tex` | `68-78` | `cabstract` 环境正文；关键词来自 `ckeywords` |
| Abstract | II | `front/cover.tex` | `80-98` | `eabstract` 环境正文；关键词来自 `ekeywords` |
| 物理量名称及符号表 | III | `front/denotation.tex` | `1-13` | `denotation` 环境中的表格 |
| 目录 | 目录页 | 自动生成 | `thesis.tex:122` | 修改各章 `\chapter`、`\section` 后重新编译生成 |

封面和摘要的版式宏来自：

| 模板功能 | 文件位置 | 说明 |
|---|---:|---|
| `\makecover` | `hithesisbook.cls:1594-1626` | 生成封面，并调用摘要生成逻辑 |
| 中文/英文摘要生成 | `hithesisbook.cls:1635-1652` | 输出摘要标题、摘要正文、关键词 |
| `denotation` 环境 | `hithesisbook.cls:1653-1659` | 生成物理量表标题和表编号逻辑 |

### 3.2 正文部分

| PDF 部分 | PDF 页码 | 可编辑文件 | 起始位置 | 说明 |
|---|---:|---|---:|---|
| 第 1 章 哈尔滨工业大学研究生学位论文撰写规范 | 1 | `body/regu.tex` | `3` | 整章内容 |
| 1.1 内容要求 | 1 | `body/regu.tex` | `18` | 内容要求总节 |
| 1.1.1 题目 | 1 | `body/regu.tex` | `20` | 题目要求 |
| 1.1.2 摘要与关键词 | 1 | `body/regu.tex` | `31` | 摘要和关键词要求 |
| 1.1.2.1 摘要 | 1 | `body/regu.tex` | `32` | 摘要说明 |
| 1.1.2.2 关键词 | 2 | `body/regu.tex` | `64` | 关键词说明 |
| 1.1.3 目录 | 2 | `body/regu.tex` | `69` | 目录说明 |
| 1.1.4 论文正文 | 2 | `body/regu.tex` | `73` | 正文说明 |
| 1.1.4.1 绪论 | 3 | `body/regu.tex` | `77` | 绪论说明 |
| 1.1.4.2 论文主体 | 3 | `body/regu.tex` | `85` | 主体说明 |
| 1.1.4.3 结论 | 3 | `body/regu.tex` | `95` | 结论说明 |
| 1.1.5 参考文献 | 4 | `body/regu.tex` | `113` | 文献要求 |
| 1.1.6 攻读学位期间取得创新性成果 | 4 | `body/regu.tex` | `126` | 成果要求 |
| 1.1.7 原创性声明及使用权限 | 4 | `body/regu.tex` | `133` | 声明说明 |
| 1.1.8 致谢 | 4 | `body/regu.tex` | `136` | 致谢说明 |
| 1.1.9 个人简历 | 4 | `body/regu.tex` | `139` | 简历说明 |
| 1.2 书写规定 | 5 | `body/regu.tex` | `142` | 书写规定总节 |
| 1.2.1 论文正文字数 | 5 | `body/regu.tex` | `143` | 字数规定 |
| 1.2.2 论文书写 | 5 | `body/regu.tex` | `146` | 书写规定 |
| 1.2.3 摘要与关键词 | 5 | `body/regu.tex` | `152` | 摘要格式 |
| 1.2.4 目录 | 5 | `body/regu.tex` | `161` | 目录格式 |
| 1.2.5 论文正文 | 6 | `body/regu.tex` | `177` | 正文格式 |
| 1.2.5.1 章节及各章标题 | 6 | `body/regu.tex` | `178` | 标题格式 |
| 1.2.5.2 层次 | 6 | `body/regu.tex` | `183` | 层次格式 |
| 1.2.6 引用文献标注 | 6 | `body/regu.tex` | `187` | 引用示例，引用条目在 `reference.bib` |
| 1.2.7 名词术语 | 6 | `body/regu.tex` | `204` | 术语要求 |
| 1.2.8 物理量标注 | 7 | `body/regu.tex` | `210` | 物理量要求 |
| 1.2.8.1 物理量的名称和符号 | 7 | `body/regu.tex` | `211` | 名称和符号 |
| 1.2.8.2 物理量计量单位 | 7 | `body/regu.tex` | `217` | 单位 |
| 1.2.9 外文字母的正体与斜体用法 | 7 | `body/regu.tex` | `227` | 字体说明 |
| 1.2.10 数字 | 7 | `body/regu.tex` | `232` | 数字用法 |
| 1.2.11 公式 | 7 | `body/regu.tex` | `236` | 公式与式号；示例公式在 `251-263` |
| 1.2.12 插表 | 8 | `body/regu.tex` | `265` | 表格规范 |
| 1.2.12.1 图题及图中说明 | 9 | `body/regu.tex` | `296` | 图题规范 |
| 1.2.12.2 插图编排 | 10 | `body/regu.tex` | `301` | 插图规范 |
| 1.2.13 参考文献 | 10 | `body/regu.tex` | `305` | 文献格式说明 |
| 1.2.14 附录 | 11 | `body/regu.tex` | `334` | 附录说明 |
| 1.2.15 攻读学位期间取得创新性成果 | 11 | `body/regu.tex` | `343` | 成果格式 |
| 1.2.16 索引 | 11 | `body/regu.tex` | `346` | 索引说明 |
| 1.2.17 个人简历 | 11 | `body/regu.tex` | `348` | 简历说明 |
| 1.2.18 书脊 | 11 | `body/regu.tex` | `351` | 书脊说明 |
| 1.2.19 其他 | 12 | `body/regu.tex` | `354` | 其他说明 |

### 3.3 第 2 章示例文档

| PDF 部分 | PDF 页码 | 可编辑文件 | 起始位置 | 说明 |
|---|---:|---|---:|---|
| 第 2 章 示例文档 | 13 | `body/introduction.tex` | `3` | 整章内容 |
| 2.1 关于数字 | 13 | `body/introduction.tex` | `11` | 数字说明 |
| 2.2 索引示例 | 14 | `body/introduction.tex` | `24` | 使用 `\sindex` 生成索引项 |
| 2.3 术语排版举例 | 14 | `body/introduction.tex` | `31` | 术语命令示例 |
| 2.4 引用 | 14 | `body/introduction.tex` | `39` | `\cite`、`\inlinecite` 示例 |
| 2.5 定理和定义等 | 14 | `body/introduction.tex` | `55` | theorem/definition 等环境 |
| 2.6 图片 | 15 | `body/introduction.tex` | `86` | 图片总节 |
| 2.6.1 博士毕业论文双语题注 | 15 | `body/introduction.tex` | `97` | `golfer` 图片，`figures/golfer.eps` |
| 2.6.2 本硕论文题注 | 16 | `body/introduction.tex` | `114` | `golfer` 图片 |
| 2.6.3 并排图和子图 | 17 | `body/introduction.tex` | `121` | 并排图/子图总节 |
| 2.6.3.1 并排图 | 17 | `body/introduction.tex` | `122` | 多个 `minipage` 图片示例 |
| 2.6.3.2 子图 | 17 | `body/introduction.tex` | `173` | `subfigure`、TikZ 标注、横置图片 |
| 2.7 如何做出符合规范的漂亮的图 | 22 | `body/introduction.tex` | `273` | 作图说明 |
| 2.7.1 Tikz作图举例 | 22 | `body/introduction.tex` | `277` | TikZ 绘图源码 |
| 2.7.2 R作图 | 22 | `body/introduction.tex` | `319` | R 作图说明 |
| 2.8 表格 | 22 | `body/introduction.tex` | `330` | 表格总节 |
| 2.8.1 普通表格的绘制方法 | 23 | `body/introduction.tex` | `340` | `table` + `tabular` 示例 |
| 2.8.2 长表格的绘制方法 | 24 | `body/introduction.tex` | `362` | `longtable` 示例 |
| 2.8.3 列宽可调表格的绘制方法 | 25 | `body/introduction.tex` | `426` | `tabularx` 示例 |
| 2.8.3.1 表格内某单元格内容过长的情况 | 25 | `body/introduction.tex` | `431` | 宽度可调表格示例 |
| 2.8.3.2 对物理量符号进行注释的情况 | 26 | `body/introduction.tex` | `454` | 公式注释表格 |
| 2.8.3.3 排版横版表格的举例 | 27 | `body/introduction.tex` | `480` | `sideways` 横版表格 |
| 2.9 公式 | 27 | `body/introduction.tex` | `502` | 公式说明 |
| 2.10 其他杂项 | 27 | `body/introduction.tex` | `506` | 杂项总节 |
| 2.10.1 右翻页 | 27 | `body/introduction.tex` | `508` | `\cleardoublepage` 说明 |
| 2.10.2 算法 | 27 | `body/introduction.tex` | `515` | 算法说明 |
| 2.10.3 脚注 | 27 | `body/introduction.tex` | `528` | 脚注说明 |
| 2.10.4 源码 | 27 | `body/introduction.tex` | `531` | 源码排版说明 |
| 2.10.5 思源宋体 | 29 | `body/introduction.tex` | `535` | 字体说明 |
| 2.10.6 专业绘图工具 | 29 | `body/introduction.tex` | `542` | 绘图工具链接，含 `\label{drawtool}` |
| 2.10.7 术语词汇管理 | 29 | `body/introduction.tex` | `554` | glossaries 说明 |
| 2.10.8 TeX 源码编辑器 | 29 | `body/introduction.tex` | `557` | 编辑器说明 |
| 2.10.9 LaTeX 排版重要原则 | 29 | `body/introduction.tex` | `561` | 格式/内容分离说明 |
| 2.11 关于捐助 | 29 | `body/introduction.tex` | `567` | 使用根目录 `zfb.eps`；当前日志提示 `\ref{zfb}` 未定义 |

### 3.4 第 3 章招生招聘内容

| PDF 部分 | PDF 页码 | 可编辑文件 | 起始位置 | 说明 |
|---|---:|---|---:|---|
| 第 3 章 哈尔滨工业大学初砚硕课题组招(聘)全日制硕士、博士、助理研究员 | 31 | `body/recruit.tex` | `3` | 整章内容 |
| 3.1 导师简介 | 31 | `body/recruit.tex` | `5` | 导师介绍 |
| 3.2 主要研究方向 | 31 | `body/recruit.tex` | `9` | 研究方向 |
| 3.3 学术成果 | 31 | `body/recruit.tex` | `13` | 成果介绍 |
| 3.4 团队介绍 | 31 | `body/recruit.tex` | `18` | 团队介绍 |
| 3.5 其他介绍 | 31 | `body/recruit.tex` | `22` | 工具链和平台介绍 |
| 3.6 硕博士招生要求 | 32 | `body/recruit.tex` | `34` | `itemize` 列表 |
| 3.7 助理研究员招聘需求和待遇 | 32 | `body/recruit.tex` | `42` | 招聘需求 |
| 3.8 申请方式 | 32 | `body/recruit.tex` | `54` | 邮箱 |

### 3.5 后置部分

| PDF 部分 | PDF 页码 | 可编辑文件 | 起始位置 | 说明 |
|---|---:|---|---:|---|
| 结论 | 33 | `back/conclusion.tex` | `2` | `conclusions` 环境内容 |
| 参考文献 | 34 | `reference.bib`、`thesis.tex` | `reference.bib:3`，`thesis.tex:132,158` | 条目来自 `reference.bib`，样式为 `hithesis.bst` |
| 附录 1 带章节的附录 | 35 | `back/appA.tex` | `3` | 附录第 1 章 |
| 1.1 附录节的内容 | 35 | `back/appA.tex` | `7` | 附录图片和公式示例 |
| 附录 2 这个星球上最好的免费Linux软件列表 | 36 | `back/appA.tex` | `25` | 附录第 2 章 |
| 2.1 系统 | 36 | `back/appA.tex` | `26` | 系统软件说明 |
| 2.1.1 hifvwm的优点 | 36 | `back/appA.tex` | `31` | 枚举列表 |
| 2.2 其他 | 36 | `back/appA.tex` | `48` | 其他软件链接 |
| 2.3 vim | 37 | `back/appA.tex` | `66` | `lstlisting` 示例 |
| 攻读学士 学位期间取得创新性成果 | 38 | `back/publications.tex` | `2` | 论文、专利、项目获奖列表 |
| 索引 | 39 | `back/ceindex.tex`、正文 `\sindex` | `back/ceindex.tex:1` | `\printsubindex*` 自动打印索引 |
| 哈尔滨工业大学本科毕业论文（设计）原创性声明和使用权限 | 40 | `front/cover.tex`、`hithesisbook.cfg`、`hithesisbook.cls` | `front/cover.tex:23,35`，`hithesisbook.cfg:346-370`，`hithesisbook.cls:1849-1892` | 页面由 `\authorization` 宏自动生成，标题/正文模板在 cfg/cls |
| 致谢 | 41 | `back/acknowledgements.tex` | `2` | `acknowledgements` 环境内容 |
| 个人简历 | 42 | `back/resume.tex` | `3` | `resume` 环境内容 |

## 4. 图片与外部资源对应关系

当前编译实际读取到的项目内图片资源：

| 资源 | 被使用位置 | 说明 |
|---|---|---|
| `figures/golfer.eps` | `body/introduction.tex:100,117,132,138,147,153,162,168,185,188,194,197,211,214,226,257-263`；`back/appA.tex:13` | 示例高尔夫图片 |
| `zfb.eps` | `body/introduction.tex:572` | 捐助二维码图片，位于项目根目录 |
| `figures/empty-resolution.pdf` | `hithesisbook.cls:1917` | 博士答辩决议表电子版占位图；当前文档为本科，`\resolution` 在博士条件下才输出 |
| `hrb-bachelor-bottommark.eps` | `hithesisbook.cfg:308` | 哈尔滨本科封面底部标记 |
| `bthesistitle.eps`、`shenzhenbthesistitle.eps`、`hitlogo.eps` | 模板封面相关资源 | 由模板封面宏按校区/类型选择使用 |

新增图片建议放入 `figures/`，然后用 `\includegraphics{文件名}` 引用；如果放在项目根目录，也可以直接引用，但不利于资源归档。

## 5. 参考文献、索引、术语的编辑入口

### 5.1 参考文献

| 操作 | 编辑位置 |
|---|---|
| 新增/修改文献条目 | `reference.bib` |
| 修改引用位置 | 正文中的 `\cite{...}`、`\inlinecite{...}`，当前主要在 `body/regu.tex` 和 `body/introduction.tex` |
| 修改参考文献样式 | `thesis.tex:132`，当前为 `\bibliographystyle{hithesis}` |
| 修改 BibTeX 样式文件 | `hithesis.bst`，一般不建议改，除非明确要改全局文献格式 |

当前 `reference.bib` 中已有条目包括：`cnproceed`、`cnarticle`、`Lin1992`、`xin1994`、`zhao1998`、`Chen1992`、`hithesis2017`。

### 5.2 索引

| 操作 | 编辑位置 |
|---|---|
| 增删索引页 | `thesis.tex:167` 的 `\include{back/ceindex}` |
| 修改索引页生成方式 | `back/ceindex.tex:1-4` |
| 新增索引词 | 正文中的 `\sindex[...]` 命令，例如 `body/introduction.tex:28,41` |
| 修改索引环境版式 | `hithesisbook.cls:1808-1815` |

### 5.3 术语和自定义命令

`body/introduction.tex:31-37` 展示了术语命令用法。相关命令定义通常在 `hithesis.sty` 或模板类文件中；修改前应先搜索命令定义，避免只改展示文本而未改实际宏。

## 6. 自动生成文件与不建议直接编辑的文件

| 文件 | 类型 | 处理建议 |
|---|---|---|
| `thesis.pdf` | 编译产物 | 不直接编辑，修改 `.tex` 后重新编译 |
| `thesis.toc` | 目录缓存 | 不直接编辑，修改章节标题后重新编译 |
| `thesis.aux`、`body/*.aux`、`back/*.aux` | 交叉引用缓存 | 不直接编辑 |
| `thesis.bbl` | BibTeX 生成的参考文献结果 | 不直接编辑，改 `reference.bib` |
| `thesis.ind`、`thesis-china.ind`、`thesis-english.ind` | 索引生成结果 | 不直接编辑，改 `\sindex` 或 `back/ceindex.tex` |
| `thesis.log`、`thesis.fls`、`thesis.fdb_latexmk` | 编译日志/依赖记录 | 只读诊断用途 |
| `thesis.xdv`、`thesis.synctex.gz` | 编译中间产物/同步定位 | 不直接编辑 |

## 7. 当前编译诊断提示

从 `thesis.log` 可见当前编译有以下值得关注的问题：

| 类型 | 位置/信息 | 建议 |
|---|---|---|
| 未定义引用 | `Reference 'zfb' on page 29 undefined on input line 568` | `body/introduction.tex:568` 使用 `\ref{zfb}`，但图注 `\bicaption[Donation]...` 未提供 `zfb` 标签。可将引用改为已有标签或为该图补充对应标签。 |
| glossaries 警告 | `No \printglossary or \printglossaries found` | 如果不需要术语表可忽略；如果需要术语表，应添加打印命令。 |
| 字体替代警告 | 多处 `Font shape ... not available` | 当前能生成 PDF；如需严格字体一致性，再检查字体配置。 |
| 浮动体位置调整 | 多处 `!h float specifier changed to !ht` | LaTeX 自动调整图片/表格位置；通常不影响内容。 |

## 8. 按修改目标快速定位

| 想修改的内容 | 直接编辑 |
|---|---|
| 论文中文标题 | `front/cover.tex:20-23` |
| 作者姓名 | `front/cover.tex:28` |
| 导师 | `front/cover.tex:29` |
| 学号 | `front/cover.tex:36` |
| 日期 | `front/cover.tex:35` |
| 中文关键词 | `front/cover.tex:64` |
| 英文关键词 | `front/cover.tex:65` |
| 中文摘要正文 | `front/cover.tex:68-78` |
| 英文摘要正文 | `front/cover.tex:80-98` |
| 正文章节增删和顺序 | `thesis.tex:124-126` |
| 第 1 章内容 | `body/regu.tex` |
| 第 2 章内容 | `body/introduction.tex` |
| 第 3 章内容 | `body/recruit.tex` |
| 结论 | `back/conclusion.tex` |
| 文献条目 | `reference.bib` |
| 附录 | `back/appA.tex` |
| 成果列表 | `back/publications.tex` |
| 致谢 | `back/acknowledgements.tex` |
| 简历 | `back/resume.tex` |
| 原创性声明中的论文题目 | `front/cover.tex:23` |
| 原创性声明固定文本 | `hithesisbook.cfg:346-370`，不建议常规论文写作时修改 |
| 全局版式 | `hithesisbook.cls`、`hithesisbook.cfg`，不建议常规论文写作时修改 |

