---
user_id: "cheng tan"
paper_id: 10007
arxiv_id: "2608.30712v1"
title: "GUIDE: Guiding Internal Evidence with Language Instructions"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30712v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30712v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:44:12"
---
# GUIDE: Guiding Internal Evidence with Language Instructions

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal instruction following · evidence control · parameter-efficient adaptation · instruction-conditioned gating

## 一句话总结

GUIDE通过语言指令控制多模态模型的内部证据使用，结合分组参数高效适应与指令条件门控，实现证据依赖的可控调节。

## 摘要

> Large multimodal models follow instructions about what to generate, but not necessarily about what evidence to rely on. Hence, models may continue to depend on shortcut-associated cues even when instructions suggest otherwise. We introduce GUIDE, a framework for controlling internal evidence usage through language instructions. GUIDE combines grouped parameter-efficient adaptation with instruction-conditioned gating to modulate multimodal evidence pathways during reasoning and generation. We further introduce a pathway-level evaluation framework that characterizes instruction-conditioned evidence modulation through reliance sensitivity, controlled perturbation analysis, pathway modulation, and autoregressive decoding dynamics. Across multimodal reasoning, classification, and generation, GUIDE induces structured and instruction-aligned redistribution of evidence reliance while largely preserving task behavior. Experiments on GQA, TextVQA, MM-IMDb, CREMA-D, RAVDESS, and Flickr30K show that GUIDE improves robustness under targeted evidence perturbations and enables controllable modulation across diverse multimodal settings. This suggests that multimodal instruction following can extend beyond output control toward regulating how different evidence sources contribute to model predictions.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：多模态模型的指令跟随通常只控制输出内容，而不控制内部证据的依赖关系，导致模型即使收到相反指令，仍会依赖与捷径相关的线索。具体而言，现有指令跟随范式中，指令被用来约束生成目标，但模型内部如何利用不同模态或特征通路的信息并不受指令调控。这带来两个后果：一是模型可能在大规模训练中学到捷径关联，在分布偏移或对抗扰动下产生脆弱预测；二是用户无法通过自然语言指示模型“更依赖图像纹理”或“更少依赖语言先验”这类证据偏好。因此，论文提出问题：能否通过语言指令调节模型的证据使用方式？如何在保持任务性能的同时实现这种调节？以及如何量化和评估指令对证据依赖的影响？

Q2: 有哪些相关研究？

相关研究可合理分为几个方向。第一，多模态指令跟随：已有工作如LLaVA等通过指令微调让模型生成符合指令的响应，但指令通常限定输出内容，而非证据来源；本工作补充了证据控制维度。第二，参数高效微调（PEFT）：LoRA、Adapter等方法以少量参数适应下游任务，本工作采用分组参数高效适应，将不同证据通路关联到不同参数组，这属于对PEFT的扩展。第三，模型可解释性与归因：许多研究通过梯度、注意力或因果干预分析模型依赖，但主要用于事后解释，而非主动控制；本工作的通路级评估框架借鉴了这些思想，用于衡量指令条件调节效果。第四，鲁棒性与捷径学习：已有研究表明模型会利用数据集捷径，本工作提供了一种通过指令对抗捷径的途径。然而，检索片段中未具体列举参考文献，上述分类属于合理推断，具体相关工作需查阅原文。

Q3: 论文如何解决这个问题？

论文的解决方案GUIDE包含两个核心组件：分组参数高效适应（grouped parameter-efficient adaptation）和指令条件门控（instruction-conditioned gating）。分组参数高效适应是指将模型内部参数按证据类别或模态划分为多个适应组，每组通过低秩或Adapter方式独立优化，以形成功能上分化的证据通路。指令条件门控根据用户提供的语言指令生成门控信号，动态调节各证据通路的贡献权重，从而在推理和生成过程中实现证据依赖的重分配。这种设计使得模型既能保留原有任务能力，又能根据指令调整证据来源。为了系统评估这种控制能力，论文还提出了通路级评估框架，包括四个组成部分：一是依赖敏感性（reliance sensitivity），衡量输出对指令变化的敏感程度；二是受控扰动分析（controlled perturbation analysis），通过人为扰动特定证据源来观察预测变化；三是通路调节（pathway modulation），直接干预中间表征以验证通路功能；四是自回归解码动态（autoregressive decoding dynamics），跟踪生成过程中每个token的证据支持变化。整体上，GUIDE在保持任务行为的同时，将指令的影响力从输出层扩展到内部证据分布。

Q4: 论文做了哪些实验？

实验在多模态推理、分类和生成三类任务上进行，覆盖六个数据集。多模态推理使用GQA和TextVQA，测试需要视觉与语言联合推理的场景；分类任务使用MM-IMDb（推测为多模态电影类型分类）、CREMA-D（音视频情感识别）和RAVDESS（音视频情感识别），这些数据集具有明确的可扰动证据类别；生成任务使用Flickr30K（图像描述生成），评估开放生成中的证据控制。评估框架依据四个维度展开：依赖敏感性、受控扰动分析、通路调节和自回归解码动态。具体实验设置包括：在原始数据和受到针对性扰动（如遮挡、噪声或模态替换）的数据上比较模型表现；通过门控权重变化分析指令对证据通路的调制；以及对比GUIDE与基线模型在保持任务性能方面的差异。需要注意，检索片段仅给出了数据集和框架名称，未包含具体实验配置和指标细节，上述描述部分属于合理推断，需查阅原文确认。

Q5: 发现了什么实验现象？

从摘要和结论片段可以归纳出以下主要观察：一是GUIDE能够诱导结构化且与指令对齐的证据依赖重新分配，说明指令确实能改变模型内部证据权重，而不仅仅是输出内容；二是这种调节在大体上保留了任务行为，表明控制证据使用并不必然牺牲性能；三是在针对性证据扰动下，GUIDE比未受控模型具有更好的鲁棒性，说明它可以抑制对捷径线索的依赖；四是该效果在推理、分类和生成多种设置中均出现，显示了一定的通用性。由于检索证据中未提供具体量化数据，以上观察均为定性描述，具体数值和趋势需查阅论文的实验结果章节。

Q6: 有什么可以进一步探索的点？

基于论文陈述和局限性，未来研究方向包括：第一，探索更原则性的因果干预和定位方法，以更精确地识别和控制证据通路，而不是依赖分组路由这种操作性机制；第二，在更大规模、更开放的多模态智能体场景中评估GUIDE，验证其在真实部署中的可扩展性和稳健性；第三，研究如何显式解耦不同语义因素，实现证据通路的完全隔离，避免语义重叠；第四，将GUIDE扩展到更多模态组合（如文本-3D、文本-点云）和更复杂的推理任务；第五，结合生物或科学领域应用，如医学影像报告生成中的证据控制，这可能与用户画像中的ai-for-science方向相关（推测）。

Q7: 总结一下论文的主要内容

GUIDE是'Guiding Internal Evidence with Language Instructions'的缩写，论文提出了一套通过自然语言指令控制多模态模型内部证据使用的框架。研究动机源于一个现象：大型多模态模型能够遵循指令生成特定内容，但指令往往无法影响模型内部依赖哪些证据或线索。即使指令建议忽略某些捷径，模型仍可能固守训练习得的统计关联，导致预测脆弱。为此，作者提出了GUIDE，其核心思想是将证据使用变为一种可调节的对象。技术上，GUIDE结合了分组参数高效适应与指令条件门控：前者在参数层面构建多条可独立优化的证据通路，后者利用语言指令动态调节各通路的激活强度。这一设计让模型在推理和生成时，能够根据指令重新平衡不同模态或特征的贡献。为了验证这种控制的可量化性，作者设计了一个通路级评估框架，包含依赖敏感性、受控扰动分析、通路调节和自回归解码动态四个维度，分别从行为敏感度、因果扰动、内在机制和生成过程四个方面刻画指令对证据依赖的调制。实验覆盖了多模态推理（GQA、TextVQA）、分类（MM-IMDb、CREMA-D、RAVDESS）和生成（Flickr30K）六个基准。结果表明，GUIDE在保持任务行为的同时，实现了证据依赖的结构化重分配，并在针对性扰动下提升了鲁棒性。论文的结论是，指令跟随的范畴可以从输出控制扩展到证据调节，这为提升多模态模型的可靠性和可控性提供了新途径。需要指出，由于检索证据仅覆盖摘要、引言、结论和部分实验描述，本总结中关于技术细节和实验结果的表述部分依赖合理推断，具体实现请参考原文。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的生成方向（权重0.1）直接相关，GUIDE在Flickr30K生成任务上展示了证据控制能力。

## 基本信息

- 作者：Soyeon Caren Han, Hyunsuk Chung, Jinwoo Kim, Seungyeon Ji, Kyungreem Han
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30712v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据片段（摘要、引言、结论、部分实验），并根据这些片段和摘要进行了合理推断；具体数值和未提及细节需查阅原文确认。
