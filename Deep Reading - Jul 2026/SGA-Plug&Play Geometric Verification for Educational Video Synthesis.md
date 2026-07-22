---
user_id: "cheng tan"
paper_id: 4723
arxiv_id: "2607.18116"
title: "SGA: Plug&Play Geometric Verification for Educational Video Synthesis"
publish_date: "2026-07-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.18116.pdf"
pdf_url: "https://arxiv.org/pdf/2607.18116"
abs_url: "https://arxiv.org/abs/2607.18116"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T19:22:03"
---
# SGA: Plug&Play Geometric Verification for Educational Video Synthesis

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：symbolic geometric agent · manim · educational video synthesis · scene graph

## 一句话总结

提出符号几何智能体（SGA），一种即插即用的空间验证模块，通过部分执行LLM生成的Manim动画代码构建符号场景图，检测几何遮挡冲突并自动进行代码精炼，同时引入无渲染的Manim视觉质量分数（MVQS）用于评估布局完整性。实验表明SGA在MMMC-Code基准上实现了最高73.11的MVQS，相比原始基线提升16.1%，并在7/8实验配置中一致改善。

## 摘要

> Recent work leverages Large Language Models (LLMs) to generate executable code for pedagogical animations using libraries such as Manim. However, ensuring spatial correctness and visual legibility remains challenging, as existing frameworks emphasize pedagogical content while overlooking geometric occlusions. We propose the Symbolic Geometric Agent (SGA), a plug-and-play module for code-centric animation pipelines that intercepts LLM-generated code, performs partial execution to extract symbolic scene graphs, and applies targeted refinement when spatial conflicts are detected. We further introduce the Manim Visual Quality Score (MVQS), a deterministic rendering-free proxy for spatial integrity. Experiments on the MMMC-Code benchmark across four LLM backbones and two agentic pipelines show that SGA achieves a peak MVQS of 73.11 (Code2Video + GPT-5.1), corresponding to a 16.1% relative improvement over the raw baseline, and improves MVQS in 7 of 8 backbone x pipeline configurations.

Q1: 这篇论文试图解决什么问题？

当前基于LLM的教育视频合成方法（如TheoremExplainAgent和Code2Video）能够生成可执行的Manim代码，但生成结果在空间布局上存在严重缺陷，例如文字标签重叠、几何图形相互遮挡、关键元素位置不当等。这些几何问题在教育场景中尤其有害，会导致学习者误解概念或分散注意力。用户评估显示，即使代码具有较高的事实准确性和逻辑连贯性，其视觉组织得分在所有评估维度中最低（Ku et al., 2025）。因此，亟需一种能够自动识别和修正空间冲突的机制，而现有的基于VLM的评估方法需要完整渲染视频，开销较大。SGA旨在通过符号推理在代码层面解决空间验证问题，无需渲染即可检测冲突。

Q2: 有哪些相关研究？

教育视频合成领域，TheoremExplainAgent（Ku等, 2025）和Code2Video（Chen等, 2025）等框架利用LLM直接生成动画代码，但缺乏空间正确性保障。视觉质量评估方面，现有的方法多依赖渲染后的人工评分或VLM判断，耗时且不精确。SGA区别于这些工作，是一种轻量级的即插即用模块，专注于几何层面，且不需要实际渲染。此外，场景图在计算机视觉中常用于图像推理，SGA将其应用于动画代码的符号验证。与基于规则的方法相比，SGA更灵活，通过部分执行解析实际对象位置。

Q3: 论文如何解决这个问题？

SGA模块由五个组件组成：（1）代码截取器，捕获LLM生成的Manim代码；（2）部分执行器，在安全的沙盒环境中执行代码到场景构建完成但不渲染视频，提取对象位置属性；（3）场景图构建器，将对象及其空间关系（包围盒、层次、锚点）表示为符号图；（4）冲突检测器，基于定义好的规则（如重叠面积比例、边界溢出、最小间距）识别几何冲突；（5）代码重写器，根据冲突类型生成修改后的代码。此外，MVQS指标从场景图中计算三个子分数：比例和谐度、对分布和对齐度，最终加权得到0-100分数，无需渲染即可反映视觉质量。SGA作为中间层集成到现有流水线中，修正后的代码再交由Manim渲染最终视频。

Q4: 论文做了哪些实验？

实验在MMMC-Code基准上进行，该基准包含多种数学概念的动画生成任务。实验采用两个基础流水线：TheoremExplainAgent（TEA）和Code2Video（C2V）。每个流水线使用四个不同的LLM骨干（包括GPT-4o、GPT-5.1等）。评估指标是MVQS，同时报告了基于VLM的感知评分和渲染效率。实验设置中包含消融研究，比较SGA开启和关闭的效果。计算效率分析比较了SGA和VLM评估的时间成本。

Q5: 发现了什么实验现象？

实验观察到以下现象：
1. 在MMMC-Code基准上，未采用SGA的原始流水线输出的动画在视觉组织维度得分最低，表明几何冲突是普遍问题（引自Ku et al., 2025）。
2. 加入SGA后，在8个骨干×流水线配置中，有7个配置的MVQS得到显著提升，其中Code2Video+GPT-5.1组合取得最高MVQS 73.11，相对基线提升16.1%。仅有一个配置未观察到提升，可能与原始代码已经具有良好布局有关。
3. 计算效率分析显示，SGA的每步处理时间为5-15秒，而基于VLM的评估需要30-90秒，实现了约6-18倍的加速。

Q6: 有什么可以进一步探索的点？

当前SGA仅限于2D几何基元，未来可以扩展到3D场景和更复杂的图形（如曲线、非刚性形变）。此外，目前仅支持Manim引擎，可以适配其他动画库（如Three.js、Blender Python API）。代码重写策略目前基于规则，未来可以结合学习算法自适应选择修正策略。MVQS作为代理指标，可探索与人类感知对齐的加权方式。在流水线集成方面，可以设计更紧密的耦合，将SGA嵌入为端到端训练的一部分。跨语言和跨文化内容生成也值得研究。最后，SGA的失败模式（如破坏代码语义）需要更系统的分析。

Q7: 总结一下论文的主要内容

本文提出符号几何智能体（SGA），用于改善基于LLM的教育动画代码生成中的空间正确性。教育动画生成通常使用Manim库，LLM能生成代码但经常产生几何遮挡和布局问题。SGA作为即插即用模块，通过截获代码、部分执行、构建场景图、检测冲突和生成重写代码来修正这些问题。同时提出MVQS，一种无需渲染的视觉质量指标。实验在MMMC-Code基准上集成两个最新流水线（TEA和C2V）和四个LLM骨干，显示SGA在7/8配置中提升了MVQS，最高提升16.1%，并且计算效率比VLM评估高6-18倍。文章还讨论了当前局限：限于2D Manim场景、规则缺乏灵活性、可能引入新的错误等。总体而言，SGA为代码生成中的几何验证提供了一种有效且实用的方法。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在Abstract和Introduction部分。

## 基本信息

- 作者：Lopez Jhon, Hinojosa Carlos, Ghanem Bernard
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CV, cs.GR, cs.MA, cs.MM
- 日期：2026-07-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.18116`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于heuristic_draft和PDF检索证据进行润色和补全，其中实验现象和效率数据直接引用证据片段，背景和局限性根据证据扩展。
