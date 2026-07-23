---
user_id: "cheng tan"
paper_id: 5044
arxiv_id: "2607.19056"
title: "Vector-Bench: Can Models Surgically Edit SVG Code?"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.19056.pdf"
pdf_url: "https://arxiv.org/pdf/2607.19056"
abs_url: "https://arxiv.org/abs/2607.19056"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T13:58:55"
---
# Vector-Bench: Can Models Surgically Edit SVG Code?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：svg editing · vector graphics · benchmark · instruction following

## 一句话总结

我们提出了Vector-Bench，一个紧凑且困难的SVG代码编辑基准，包含40个修复任务，并通过34个模型端点的评估揭示了修复进度与规范忠实编辑之间的显著差距。

## 摘要

> Instruction-based vector editing requires two capabilities: making a requested change and leaving everything else alone. The second is easy to miss when an output is judged only as a raster image. We introduce Vector-Bench, a compact, difficult benchmark of 40 SVG repair tasks. Each task pairs a corrupted SVG program with an author-written visual instruction, a hidden target program, 5.05 annotated repairs on average, and an average of 60.55 protected objects. Instructions describe visible defects without exposing element identifiers, coordinates, color codes, or path data. We define a deterministic binary specification reward: requested repairs use attribute-aware perceptual tolerances, while unrequested rendering- or application-relevant structure must remain semantically unchanged and the result must be a valid SVG. Canonical target equality and stricter source fidelity are retained as diagnostics. Validity-gated repair progress, a near-complete tier, and valid-output Unintended Change Rate (UCR) explain partial outcomes. We evaluate 34 model endpoints (25 listed as open-weight, 5 inexpensive controls, and 4 frontier closed endpoints) over 1360 requests. The strongest endpoint reaches only 15.0% full specification success, despite 43.7% mean repair progress, showing that apparent repair progress and specification-faithful editing remain substantially different. All prompts, outputs, scoring code, costs, and per-task reports are released.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决指令式SVG代码编辑中一个关键但易被忽略的问题：模型需要同时满足两个相互冲突的目标——根据指令进行指定修改，同时保持其余部分严格不变。现有评估通常仅基于光栅图像判断，无法检测到未请求区域的细微变化，导致模型可能在视觉上看起来正确但实际违反了编辑规范。因此，需要一个精确、密集的基准来量化模型在“手术刀式”编辑中的表现，特别是其保持编辑范围的能力。

Q2: 有哪些相关研究？

相关研究分为向量生成和向量编辑基准两个方向。
1. **向量生成**：IconShop（Wu等，2023）从文本生成向量图标；StarVector（Rodriguez等，2023）从图像和文本生成SVG代码；LLM4SVG研究语言模型对复杂向量的理解与生成。
2. **向量编辑基准**：VectorEdits（Kuchař等，2025）和SVGEdit-Bench（Nishina & Matsui，2024）是已有的指令式向量编辑基准，但Vector-Bench更注重任务密度和未请求区域的保护评估。此外，多模态语言模型在代码生成和视觉指令跟随方面的进展也为SVG编辑提供了基础能力。

Q3: 论文如何解决这个问题？

本文通过构建一个密集标注、小规模但高难度的基准Vector-Bench来系统评估模型的指令式SVG编辑能力。每个任务包括一个损坏的SVG（S_0）、一个作者编写的修复指令（x）、一个隐藏的目标SVG（S*）、一组期望的对象属性修复（E）以及一组必须保持不变的受保护对象（U）。
- **评估指标**：核心是二元规范奖励，由两部分组成：① 请求修复成功率（使用属性感知的感知容差判断修复是否到位）；② 未请求部分不得有任何语义改变（包括渲染和应用相关结构），且输出必须是有效SVG。此外，定义统计指标如有效性门控修复进度（validity-gated repair progress）、接近完整层级（near-complete tier）和有效输出未预期改变率（Unintended Change Rate, UCR）来描述部分成功情况。
- **任务设计**：40个任务涵盖车站、港湾、露营地、市场等场景，平均每个任务包含5.05个修复和60.55个保护对象。指令仅描述视觉缺陷，不暴露内部标识符或坐标信息，确保模型必须理解语义。
- **评估流程**：34个模型端点（包括LLM和多模态模型）通过API或本地部署进行测试，总共1360个请求，每个任务对所有模型进行相同指令测试。

Q4: 论文做了哪些实验？

论文评估了34个模型端点，包括25个列明为开源的模型（如Llama、Qwen等系列变体）、5个低成本控制模型（如小型模型）和4个前沿封闭端点（如GPT-4o、Claude等）。每个端点针对40个任务各提交一次输出，共1360个请求。实验使用自动化评分pipeline计算规范奖励、UCR、修复进度等指标。
- **主要结果**：最强端点（封闭前沿模型）仅达到15.0%的完全规范成功，平均修复进度为43.7%。
- **部分结果**：仅考虑修复正确性时（忽略未请求部分），部分模型能达到较高修复进度，但UCR较高。
- **成本分析**：记录了每个端点的API调用成本，展示了性能与成本之间的权衡。
- **错误分析**：提供了修复进度与未预期改变的关系图，揭示许多模型在提高修复进度的同时导致更多未预期修改。

Q5: 发现了什么实验现象？

1. **修复进度与规范成功存在显著差距**：最强模型平均修复进度43.7%，但规范成功率仅15.0%，说明模型虽然能进行正确的修复，但往往难以控制编辑范围，导致对未请求区域的意外修改。
2. **未预期改变率（UCR）较高**：许多模型在修复的同时无意中改变了受保护对象，表明保持不变的能力是主要瓶颈。
3. **小型控制模型表现不佳**：低成本控制模型几乎无法成功，表明此任务需要高层次的语义理解和结构化代码操作。
4. **开放性分析**：不同模型在处理不同任务时表现差异大，某些任务对几乎所有模型都困难（如涉及多个细粒度颜色变化的任务）。
5. **有效性门控**：部分输出因无效SVG语法被惩罚，揭示了模型在SVG语法正确性方面的不足。

Q6: 有什么可以进一步探索的点？

1. **扩展任务规模和多样性**：当前40个任务规模较小，未来可扩大任务集，增加更多复杂场景和编辑类型。
2. **改进评估指标**：当前规范奖励是二元的，未来可引入更细粒度的分级指标，更好地区分部分成功。
3. **利用保护对象语义**：任务中的受保护对象集合U可以用于指导模型学习“不修改区域”，未来可探索在训练或inference时注入U信息。
4. **多模态融合**：结合视觉和代码信息，使模型更好地理解SVG结构和指令的对应关系。
5. **鲁棒性测试**：研究模型对指令细微变化、SVG中无关元素的鲁棒性。
6. **实际应用扩展**：将基准应用于交互式向量设计工具，评估人类与模型协作的编辑效率。

Q7: 总结一下论文的主要内容

本文提出Vector-Bench，一个专注于指令式SVG代码编辑的基准，旨在严格评估模型同时执行指定修改和保持其余部分不变的能力。基准包含40个精心设计的SVG修复任务，每个任务提供损坏的SVG、自然语言指令、隐藏目标、期望修复列表和保护对象集。指令仅描述视觉缺陷，避免泄露内部标识符，迫使模型依赖语义理解。评估引入二元规范奖励，不仅检查修复是否正确，还强制未请求区域语义不变且输出有效。另有修复进度、UCR等统计量说明部分结果。实验评估了34个模型端点，发现最强的封闭模型也仅达到15%的完全规范成功，而平均修复进度却达43.7%，凸显维护编辑忠实性的巨大挑战。论文公开了所有提示、输出、评估代码和成本数据，为后续研究提供标准化测试平台。该工作补充了现有向量编辑基准的不足，强调了“保持不变”能力的重要性，并对语言模型在结构化视觉代码操作中的局限性提供了实证洞察。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该方法可启发其他结构化代码编辑任务（如JSON、配置文件）中的精确指令跟随评估。

## 基本信息

- 作者：Yug Aditi Gupta, Prannay Hebbar
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.19056`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段。
