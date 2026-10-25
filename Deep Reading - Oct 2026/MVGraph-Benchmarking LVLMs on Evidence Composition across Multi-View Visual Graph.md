---
user_id: "cheng tan"
paper_id: 15498
arxiv_id: "2610.12961"
title: "MVGraph: Benchmarking LVLMs on Evidence Composition across Multi-View Visual Graph"
publish_date: "2026-10-12"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.12961.pdf"
pdf_url: "https://arxiv.org/pdf/2610.12961"
abs_url: "https://arxiv.org/abs/2610.12961"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T13:12:15"
---
# MVGraph: Benchmarking LVLMs on Evidence Composition across Multi-View Visual Graph

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-view graph reasoning · large vision-language models · benchmark · cross-view evidence composition

## 一句话总结

MVGraph 是一个专门评测 LVLM 在“跨多张视觉图视图组合证据”这一能力上的基准，包含 8k 个图论任务实例和 64k 个分阶段诊断任务实例，通过对 15 个 LVLM 的系统评测发现当前模型在跨视图边对齐和关系级证据组合上存在显著瓶颈。

## 摘要

> We introduce MVGraph, a benchmark for evaluating large vision-language models (LVLMs) on multi-graph reasoning where the evidence required by a query is distributed across multiple interrelated visual graph views. Although such evidence commonly resides in source-specific graphs, temporal snapshots, or local observations, existing graph reasoning benchmarks remain confined to a single self-contained structure, assessing only graph perception and reasoning while leaving the alignment and composition of evidence across views underexplored. MVGraph comprises 8k instances spanning 8 \textit{graph-theoretic} tasks, abstracted from two multi-graph scenarios (partial and temporal graph views). Each problem is programmatically constructed and verified to be unsolvable from a single view. Beyond end-to-end evaluation, we decompose multi-graph reasoning into four complementary capabilities---Perception, Alignment, Composition, and Reasoning---and design 29 diagnostic tasks with 64k instances to probe each one, along with golden intermediate representations for evaluating the intermediate process. Across 15 LVLMs, we reveal the difficulty of multi-graph reasoning and identify cross-view edge alignment and relation-level composition as prominent bottlenecks through stage-oriented diagnostics. Meanwhile, temporal constraints and increasing view counts expose further capability boundaries. Code and data are available at https://github.com/AAAndy-Zhu/MVGraph.

Q1: 这篇论文试图解决什么问题？

论文指出，现实中的图数据查询往往不能在单一自包含图上完成，而需要跨多个图视图收集、对齐并组合证据。这种需求出现在三个典型场景中：(1) 来源或关系特定的图——同一系统的不同方面由不同图刻画；(2) 时间快照——拓扑与属性随时间演化，查询需要综合多个时刻的信息；(3) 局部观测——每张图只覆盖相关节点和边的一部分。然而现有图推理基准（包括面向 LLM 的文本序列化图推理和面向 LVLM 的视觉图推理）默认给定单一完整图结构，只评估图感知与图推理，未覆盖“证据分布在不同视图”这一核心设定。论文认为 LVLM 需要具备的不仅是单图上的结构感知，还包括跨图对应关系的建立（cross-graph correspondences）与跨图互补信息的组合（combining complementary information）。此外，端到端准确率无法解释模型失败在哪一环节，因此论文将多图推理拆分为感知、对齐、组合、推理四个阶段，以便定位瓶颈。论文还强调时间图视图场景不能退化为对观察到的边做无差别并集（indiscriminate union）后再推理，因为时间约束（如边出现的顺序、跨快照的一致性）本身携带语义。

Q2: 有哪些相关研究？

论文属于图结构化推理评测与视觉图推理两条研究线的交叉。相关背景包括：(1) 图作为关系结构的抽象，已广泛应用于分子性质建模（Gilmer et al., 2017）、网络规模推荐（Ying et al., 2018）、交通预测（Li et al., 2018）与知识库推理（Xu et al., 2026b）；这些应用通常被形式化为在单图上的计算。(2) LLM 在文本序列化图上的图论任务表现，包括连通性、路径查找与组合优化（Wang et al., 2023; Chen et al., 2024）。(3) LVLM 将图渲染为视觉图，提供统一接口，能承载难以文本序列化的多模态图、视觉 grounding 知识库等（Das et al., 2024; Wei et al., 论文引用未完整给出）。这些工作虽然把图推理延伸到了视觉模态，但仍局限于单张自包含结构。MVGraph 的差异点在于：引入“证据组合”维度，评测跨图证据对齐与组合；引入时间图视图；引入阶段化诊断任务与黄金中间表示。由于论文正文可见部分有限，作者对更多近期 LVLM 图推理基准的具体评述未能完整获取，相关内容建议回原文核对。

Q3: 论文如何解决这个问题？

MVGraph 的构建方法分为四个层次。第一，任务定义：从两个多图场景（partial graph views 与 temporal graph views）抽象出 8 个图论任务。部分视图场景中，若干图像分别暴露底层图的不同部分，模型必须跨视图拼装任务相关结构；时间视图场景中，图像描绘同一图在不同时刻的快照，模型必须理解演化并遵循时间约束。第二，程序化构造与验证：每个实例由程序生成，并验证其满足任务的结构前提（如无环性、有效边容量、二分性），同时验证参考答案确实需要跨视图证据，确保实例无法从单一视图求解。第三，实体标识设计：实体标识符（节点用语义中性的 E*，关系用 R*）跨视图保持一致，但每个图像独立布局，因此对应实体不能仅凭位置对齐；这迫使模型依靠语义标识而非位置线索。第四，阶段化诊断：设计 29 个诊断任务、64k 个实例，针对感知、对齐、组合、推理四种能力分别探测；同时提供黄金中间表示（golden intermediate representations）用于评估中间过程。对于感知、对齐、组合诊断，模型仅接收图图像与查询；对于推理诊断，则提供任务相关的结构化信息（如算法的中间产物），以便隔离“推理”环节，排除低层感知与组合噪声。

Q4: 论文做了哪些实验？

论文在 15 个 LVLM 上进行评测，覆盖端到端多图推理与阶段化诊断两部分。端到端评测使用 8k 个实例、8 个图论任务，任务包括（从证据片段可辨认的）最大流（MF）、稳定拓扑排序（STS）、共享节点（A-SN）、移除节点（A-RN）、新增节点（A-VSN）、新增边（A-AE）等；诊断评测使用 64k 个实例、29 个诊断任务，分为感知、对齐、组合、推理四组。实验设计还包含两类干预：(1) 提供显式结构信息（explicit structural information），观察其是否能改善端到端表现，从而区分“视觉感知处理分布式证据的困难”与“高层推理困难”；(2) 改变视图数量，观察组合性能随视图数增长的退化趋势。对于时间图任务，论文特别设计了不能通过无差别并集解决的约束，评估时间耦合推理。

Q5: 发现了什么实验现象？

主要实验现象包括：(1) 当前 LVLM 在多图推理上整体表现困难，不仅在图结构感知上失败，也在跨视图对齐边关系与组合证据上失败；(2) 提供显式结构信息能提升若干部分视图图论任务的性能，说明“处理分布式视觉证据”这一感知/组合环节的困难是端到端表现不佳的重要成因，而非纯粹的高层推理缺陷；(3) 时间耦合推理在提供显式结构信息后仍然困难，说明时间约束带来的推理负担是独立且更难的能力边界；(4) 视图数量增加时，多个模型的组合性能下降，说明跨视图组合存在规模敏感性；(5) 阶段化诊断将瓶颈定位到跨视图边对齐（cross-view edge alignment）与关系级组合（relation-level composition），这两个环节比节点级对齐或感知更脆弱。需要注意的是，论文摘要明确给出“跨视图边对齐与关系级组合是突出瓶颈”，而具体每个模型的得分表、各诊断任务的分项数据、以及 15 个模型的具体排行在目前可见材料中未完整呈现，建议回原文核对详细结果。

Q6: 有什么可以进一步探索的点？

论文结论与设计隐含多个可探索方向：(1) 关系级对齐与组合的专门机制：既然边对齐与关系级组合是主要瓶颈，可探索基于结构化中间表示（如显式边对应矩阵、跨视图消息传递）的 LVLM 增强方法，或在视觉编码器中引入跨图像交互；(2) 时间图推理的专门建模：时间耦合任务在提供结构信息后仍困难，说明需要时间顺序敏感的位置编码、事件序列建模或显式时间约束解码；(3) 视图数量扩展下的组合策略：组合性能随视图数下降，需要研究如何让模型对证据来源进行显式追踪与按需聚合，而非隐式记忆；(4) 黄金中间表示的可学习利用：论文提供了黄金中间表示，可用于训练模型产生与消费中间表示，实现可解释的分阶段推理；(5) 跨模态与多模态图的扩展：将多视图设定推广到多模态图、视觉 grounding 知识库等更复杂的证据类型；(6) 更细的诊断维度：例如区分节点级对齐与边级对齐、区分关系组合与事实组合、加入反事实或扰动视图以测试组合的鲁棒性；(7) 训练策略：用程序化生成的 8k+64k 实例做数据增强或课程学习，提升 LVLM 的多图证据组合能力；(8) 评测可靠性：探讨程序化验证与人工校验的结合、答案分布对准确率的污染、以及不同渲染布局对公平性的影响。

Q7: 总结一下论文的主要内容

MVGraph 是一个面向 LVLM 的多视图视觉图推理评测基准。论文的论证主线是：真实世界图查询的证据往往分散在多个相互关联的图视图中，而现有单图基准忽略了“跨视图证据组合”这一关键能力。为此，论文从部分视图与时间视图两个场景抽象出 8 个图论任务，程序化生成并验证 8k 个端到端实例，保证每个问题都无法从单一视图求解；节点使用语义中性且跨视图一致的 E* 标识符、关系使用 R* 标识符，同时各视图独立布局，排除位置对齐的捷径。为了解释端到端失败原因，论文将多图推理分解为感知、对齐、组合、推理四阶段，设计 29 个诊断任务与 64k 实例，并为任务提供黄金中间表示以评估中间过程。在 15 个 LVLM 上评测后，论文报告了三个核心发现：第一，当前模型在多图推理端到端任务上表现困难；第二，阶段化诊断表明跨视图边对齐与关系级组合是主要瓶颈；第三，提供显式结构信息能改善部分视图任务，但时间耦合推理仍然困难，并且随视图数量增加多个模型的组合性能下降。论文由此提出关系级对齐、忠实证据组合与时间推理是改进多图理解的重要方向。论文还提及使用 ChatGPT 进行语言润色，作者对最终内容负责；论文当前以 ICLR 2027 审稿论文形式发布（arXiv 2610.12961v1）。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体研究的相关性：MVGraph 的四阶段能力分解（感知-对齐-组合-推理）提供了一种可诊断的评测设计范式，可用于智能体在多来源证据环境中的工具选择、信息聚合与推理评估。

## 基本信息

- 作者：Yingjie Zhu, Han Cheng, Xuefeng Bai, Youcheng Pan, Yang Xiang, Kehai Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.CL
- 日期：2026-10-12
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.12961`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要依据论文元数据、摘要、Introduction/Conclusion 片段与检索证据综合完成；部分实验数值与模型名单证据不足，已在相应字段标注需回原文核对。
