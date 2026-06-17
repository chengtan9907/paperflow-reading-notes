---
user_id: "cheng tan"
paper_id: 426
arxiv_id: "2606.16603v1"
title: "VeriGraph: Towards Verifiable Data-Analytic Agents"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16603v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16603v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:07:07"
---
# VeriGraph: Towards Verifiable Data-Analytic Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agents · evidence graph · data analysis · verification

## 一句话总结

VeriGraph提出了一种基于异构证据有向无环图的神经符号推理框架，通过将数据密集型LLM智能体的执行过程重构为逐步可追踪的证据图，实现输出的可审计性与可验证性。

## 摘要

> LLM-based agents have demonstrated strong capabilities in data-intensive analytical tasks, yet their outputs are rarely verifiable: a reliance on linear text trajectories makes their reasoning difficult to audit. In particular, deterministic computations over raw data and semantic deductions over natural-language claims are often entangled in an unstructured stream, leaving numerical conclusions hard to reproduce and qualitative judgments hard to inspect. To address this, we propose VeriGraph, a traceable neuro-symbolic reasoning framework that enables agents to construct an explicit heterogeneous evidence directed acyclic graph (DAG) during execution. VeriGraph introduces three evidence-expansion primitives, namely computational, grounding, and derivational expansion, to connect raw data, interpreter variables, computed results, and natural-language claims in a unified graph. Under this formulation, structural traceability is reduced to graph reachability from raw data sources to terminal claims, while semantic support is measured by claim-level evidence evaluation. To improve graph construction, we further design a graph-based policy optimization strategy with a composite reward that jointly supervises answer correctness, computational integrity, and derivational coherence. Experiments on four benchmarks show that VeriGraph-8B achieves the highest overall score among all baselines. More importantly, VeriGraph produces auditable evidence graphs with substantially stronger claim grounding, achieving a 87.61\% Grounding Rate under our claim-level evidence support evaluation. These results suggest that explicit evidence-graph construction is a promising path toward verifiable data-analytic agents. Our code is available at https://github.com/ignorejjj/VeriGraph.

Q1: 这篇论文试图解决什么问题？

现有基于LLM的数据分析智能体（如CodeAct、ReAct）依赖线性的文本轨迹，将原始数据上的确定性计算与自然语言语义推理混杂在非结构化流中，导致数值结论难以复现、定性判断难以检查。这类系统的输出缺乏可审计的证据链：从原始数据到最终答案的每一步推理无法显式追溯，使得用户无法验证结论的正确性来源。亟需一种能够显式连接原始数据、中间变量、计算结果和语言声明的方法，使智能体的推理过程可核查、可复现。

Q2: 有哪些相关研究？

相关研究主要包括：(1) LLM数据分析智能体：通过代码解释器或SQL引擎执行任务，现有工作改进管线设计或扩展训练数据，但未优化证据链接；(2) 可信生成与后验验证：通过文本归因或检索增强提供语义支持，但通常将证据视为文本片段而非带有变量来源的确定性计算；(3) 结构推理系统：将问题编译为程序或符号查询，但灵活性有限；(4) 基于验证器的方法：引入更严格的检查但缺乏全局图结构；(5) 近期的图框架：评估工具智能体轨迹、通过DAG节点验证推理、用关键证据图强化医疗推理等，这些工作凸显了图结构的价值，但未将其作为训练和可审计性的第一类目标。

Q3: 论文如何解决这个问题？

VeriGraph将智能体与环境的交互重新解释为增量构建异构证据有向无环图（DAG）的过程。每个节点可以是数据节点（原始数据、变量值等）或声明节点（自然语言陈述）。边表示三种扩展原语：(a) 计算扩展（computational expansion）：从数据节点和变量值生成新的计算节点；(b) 基础扩展（grounding expansion）：将变量值与自然语言声明绑定；(c) 推导扩展（derivational expansion）：基于已有声明推导新声明，通常通过LLM推理。图构建内置于代码动作空间中，智能体通过生成代码执行原子操作，每个操作产生相应的图结构更新。可追溯性简化为从原始数据源到终端声明的图可达性。为优化图构建，论文设计图基策略优化（Graph-based Policy Optimization），使用复合奖励函数：答案正确性奖励（基于判分器）、计算完整性奖励（每一步代码执行是否成功）、推导一致性奖励（每个推导步骤的合理性，通过独立验证器判断）。训练分为两个阶段：原子行为监督微调（atomic SFT）学习基本图原语操作，轨迹SFT学习完整图构建轨迹，最后通过RL优化整体性能。

Q4: 论文做了哪些实验？

在四个基准上评估：TableBench（约700单表问答）、InfiAgent-DABench（257单CSV问题）、DSBench（466多表长上下文任务）、以及从DABstep构造的100例多步研究任务（DAB-Step Research）。评估维度：(1) 答案准确率（LLM判分）; (2) Grounding Rate（GR）：衡量答案中的声明是否能从方法暴露的证据工件中恢复。基线包括：CodeAct（Qwen3-32B/GPT-5.2等）、Prompt-Veri（加入结构化提示的CodeAct）、以及多种ReAct变体。消融实验包括：去掉原子SFT、去掉轨迹SFT、去掉RL阶段、仅用结果奖励（outcome-only RL）等。还进行了鲁棒性实验（不同骨干模型Qwen3-4B/14B）和图可解释性分析。

Q5: 发现了什么实验现象？

(1) VeriGraph-8B在Overall score上达到73.68，超越所有基线（CodeAct @ Qwen3-32B 72.35等），Grounding Rate达到87.61%，显著高于最佳ReAct基线（+14.04pp）。(2) 消融实验显示，轨迹SFT最为关键：去掉后Overall下降至37.78，GR虽升至95.01%（因图更简单？但准确率大幅下降），表明轨迹SFT平衡了准确性与可审计性；去掉原子SFT或RL阶段也会降低性能，但影响较小；仅用结果奖励导致Overall 65.35和GR 76.46，证明过程奖励的重要性。(3) 提示结构（Prompt-Veri）能提升GR但总体准确率仍低于训练后的VeriGraph，表明模型必须学习何时及如何具体化证据。(4) 图拓扑反映任务类型：研究任务图最大（多计算节点需提升为声明），数据任务多计算边，TableBench声明层密集。(5) 图大小与任务难度正相关：得分随图增长而下降，说明大图反映证据负担而非冗余。(6) 鲁棒性实验表明，在小模型（Qwen3-4B）上VeriGraph同样带来收益，框架不依赖单一骨干。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：(1) 图质量自动评估：当前GR依赖LLM判分，未来可设计无参考的图质量指标；(2) 扩展到多模态数据：将图像、表格等异构数据纳入证据图；(3) 与外部验证器结合：如图结构作为验证器的输入，实现闭环验证；(4) 大规模推理缩放：验证图构建能否随模型规模提升而获益；(5) 动态数据场景：处理实时更新数据时的图维护问题；(6) 与其他可解释性方法（如Chain-of-Thought）的融合；(7) 减轻图构建的计算开销。

Q7: 总结一下论文的主要内容

该论文针对LLM数据分析智能体输出不可验证的问题，提出了VeriGraph框架。核心思想是将智能体的执行过程重构成一个增量构建的异构证据有向无环图（DAG），其中节点包括原始数据、变量、计算结果和自然语言声明，边通过三种原语操作（计算扩展、基础扩展、推导扩展）连接。这种结构化表示使得每项结论的溯源简化为图可达性检查，从而支持细粒度的审计。为了训练智能体构建高质量的证据图，论文设计了两阶段训练：首先通过原子SFT让模型学会执行基础的图原语操作，然后通过轨迹SFT学习完整的图构建过程，最后使用基于图的策略优化（复合奖励函数）进一步微调。复合奖励联合监督答案正确性（最终答案得分）、计算完整性（每一步代码执行是否正确）和推导一致性（每个推导步骤是否合理）。实验在四个涵盖QA、数据分析和多步研究的基准上进行。结果表明，VeriGraph-8B在总体得分上超越所有基线（包括CodeAct和ReAct强变体），同时其Grounding Rate高达87.61%，这意味着近九成的最终声明可以直接从证据图中恢复。消融研究揭示了各项训练组件的重要性，其中轨迹SFT对平衡准确性与可审计性最为关键。图分析显示，图拓扑反映了任务类型和难度，且图大小与任务难度正相关，验证了图作为内部难度信号的有效性。此外，框架在不同骨干模型上表现一致，表明其普遍适用性。论文的结论是，将证据结构作为第一类优化目标（而非后验解释）显著提升了智能体的可验证性。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该工作直接针对LLM数据分析智能体的可验证性问题，属于Agent研究方向；

## 基本信息

- 作者：Jiajie Jin, Zhao Yang, Wenle Liao, Yuyang Hu, Guanting Dong, Xiaoxi Li, Yutao Zhu, Zhicheng Dou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16603v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文PDF的语义检索证据片段，包括摘要、引言、方法、结果和讨论部分的匹配内容，并在此基础上进行润色和补全。
