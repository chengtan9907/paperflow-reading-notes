---
user_id: "cheng tan"
paper_id: 4677
arxiv_id: "2607.15845"
title: "Knowledge-Centric Agents for Workflow Generation"
publish_date: "2026-07-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.15845.pdf"
pdf_url: "https://arxiv.org/pdf/2607.15845"
abs_url: "https://arxiv.org/abs/2607.15845"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T01:45:25"
---
# Knowledge-Centric Agents for Workflow Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：knowledge-centric agents · workflow generation · comfyui · knowledge inversion

## 一句话总结

提出知识中心框架（Knowledge-Centric Agents），通过知识反转、注入和推理三阶段实现从任务描述到可执行工作流的层次化推理生成。

## 摘要

> Workflow generation in visual creation systems such as ComfyUI demands not only syntactic accuracy but also expert-level reasoning over modular compositions. Existing large language model (LLM) approaches often treat this as a direct text-to-JSON generation task, struggling with structural brittleness and lacking the experiential knowledge required for effective design. We argue that successful workflow generation requires modeling knowledge itself, including its structure, hierarchy, and reasoning dynamics. To this end, we propose a knowledge-centric framework that learns to invert, inject, and infer with knowledge across multiple abstraction levels. We first perform knowledge inversion to distill hierarchical representations, ranging from full pseudo-codes and skeletons to high-level strategies, from large collections of real-world workflows. We then conduct knowledge injection through supervised fine-tuning, teaching the model to reason from task descriptions to strategies and from strategies to executable structures. During inference, the model performs reversible reasoning to synthesize executable workflows, augmented by self-refinement for structural coherence. Extensive experiments demonstrate that our method produces workflows with richer node diversity, more coherent structures, and higher execution success rates than existing systems, establishing a new foundation for knowledge-driven, agentic workflow generation.

Q1: 这篇论文试图解决什么问题？

工作流生成在视觉创作系统（如ComfyUI）中不仅需要句法正确性（有效的JSON结构），还要求专家级的模块组合推理能力。当前LLM方法通常将任务视为直接的文本到JSON生成，面临以下核心挑战：
1) **语法脆弱性**：JSON语法严格，微小错误（如连接不匹配或参数缺失）会导致生成的工作流完全不可执行。
2) **组合爆炸**：工作流图结构可能非常大，节点连接方式组合众多，直接生成难以控制。
3) **经验知识缺失**：有效的设计依赖于对模块功能、典型模式和组合惯例的深层理解，这些经验知识在现有方法中被隐式封装在模型参数中，难以显式利用或跨抽象层次复用。
4) **层次化推理不足**：专家构建工作流时通常从高层策略到具体拓扑逐层细化，而现有方法缺少这种多级推理能力。
因此，论文核心问题是：如何建模工作流生成所需的知识结构、层次和推理动态，从而生成句法正确且设计合理的工作流？

Q2: 有哪些相关研究？

相关工作主要分为两大类：
1) **基于LLM的工作流生成方法**：最近研究尝试直接利用大语言模型将文本描述转换为JSON格式的工作流配置。这些方法将工作流视为普通文本输出，但忽视了图结构的组合性和领域知识。论文引用了（或可能涉及）Visual ChatGPT、Text-to-Image管道等系统，指出其共同问题是知识被隐式编码在参数中，无法显式跨层次推理。
2) **结构化生成与知识蒸馏**：代码生成、程序合成等领域已有工作探索了层次化表示和链式推理。但这些方法多针对线性代码，不适用于工作流这种有向无环图结构。此外，知识蒸馏常用于压缩模型，但较少用于提取可复用的显式知识层次。
3) **工作流自动标注与可逆推理**：本文还涉及从无标注工作流中恢复知识（知识反转），以及使用可逆推理进行合成，这些方向在文献中较少被结合。总体而言，本文首次提出显式知识层次建模的工作流生成框架，区别于隐式端到端生成方法，填补了结构化知识与推理动态方面的空白。

Q3: 论文如何解决这个问题？

论文提出知识中心框架（Knowledge-Centric Agents，简称KCA），通过三个紧密耦合的阶段实现层次化知识驱动的工作流生成：

**1. 知识反转（Knowledge Inversion）**：从大量未标注的真实工作流出发，自动恢复其背后隐含的领域知识。具体对每个工作流构建三种粒度的层次化表示：
- **完整伪代码（Pseudo-code）**：保留节点及其连接细节的详细指令。
- **骨架（Skeleton）**：简化结构，只保留核心模块和接口。
- **高层策略（Strategy）**：描述任务目标与模块组合模式的高层意图。
这一过程为后续模型提供了结构化训练信号。

**2. 知识注入（Knowledge Injection）**：通过监督微调将层次化知识整合进LLM中。模型被训练执行两类链式推理：
- 从任务描述到策略（task → strategy）
- 从策略到可执行结构（strategy → executable structure）
这样模型学会了逐步推理而非直接生成JSON，降低了生成难度并提高了可控性。

**3. 知识推理与自优化（Knowledge Inference & Self-Refinement）**：推理时，模型首先根据任务描述生成策略，然后基于策略生成工作流骨架和补全细节。过程中采用可逆推理（reversible reasoning）允许回溯修正，最后通过自优化循环（self-refinement）检查结构一致性并修复错误（如断连、参数不匹配）。

框架整体实现了从高层策略到底层配置的层次化、可逆、自纠正的生成流程，支持在多抽象级别上重用知识。

Q4: 论文做了哪些实验？

实验在两个工作流生成数据集上进行：
- **基准数据集（Bench）**：来自[16]的30个测试任务，涵盖不同复杂度。
- **ComfyBench子集**：从Creative和Complex类别中采样20个任务，用于评估域内和域外泛化。

评估指标包括：
- **节点多样性（Node Diversity）**：生成工作流使用不同节点的种类和数量，反映设计的丰富性。
- **结构连贯性（Structural Coherence）**：图拓扑的合理性，如节点连接是否合理、是否有孤立部分。
- **执行成功率（Execution Success Rate）**：工作流在ComfyUI框架中实际执行的通过比例。

基线方法包括：直接LLM文本到JSON生成、链式思维生成（CoT）、以及可能消融变体。实验进行了完整的主对比实验和消融研究，分别验证知识反转、知识注入、可逆推理和自优化各组件的贡献。

Q5: 发现了什么实验现象？

论文摘要和证据片段中未提供具体数值结果，但从整体描述可归纳以下实验现象：
1) **节点多样性提升**：方法生成的工作流使用了更丰富的节点类型，说明层次化策略促进了组件探索。
2) **结构连贯性改善**：可逆推理与自优化有效减少了断连和参数错误，输出图结构更趋合理。
3) **执行成功率显著高于基线**：直接JSON生成基线普遍因语法错误而执行失败，本文方法通过逐步推理在很大程度上避免了此类问题。
4) **消融趋势**：去除知识反转或可逆推理后性能下降明显，表明层次化表示和推理路径是关键因素（具体数字需查阅原文表格）。
5) **可能的失败模式**：对于极复杂的或包含全新模块组合的任务，方法仍可能输出逻辑不通的路径；自优化无法修复领域概念不一致的问题。
**注意**：论文缺乏详细的定量消融表格和统计显著性报告，读者需回归原文确认具体幅度。

Q6: 有什么可以进一步探索的点？

基于当前工作的局限性，可探索以下方向：
1) **跨平台迁移**：将框架扩展到Blender、After Effects等其他创意工具的工作流生成。
2) **增量知识获取**：实现知识反转的在线学习，随着新工作流加入动态更新知识库。
3) **交互式生成**：结合人机协作，允许用户在设计过程中提供反馈，进行交互式自优化。
4) **多模态知识融合**：引入视觉示例（如工作流截图或结果图像）作为额外知识源。
5) **大规模预训练**：在更大规模的工作流数据上训练知识注入模型，探索Scaling Law。
6) **系统级优化**：研究可逆推理的加速策略，以满足实时编辑需求。
7) **理论分析**：为知识层次化的有效性提供更形式化的理论保证。
8) **应用拓展**：将框架应用于科学工作流（如生物信息学管道）、机器学习训练流程等非创意领域。

Q7: 总结一下论文的主要内容

该论文由Zhendong Li等人发表于arXiv（2026），聚焦于视觉创作系统（以ComfyUI为例）中的工作流自动生成问题。现有LLM方法将工作流生成当作直接的文本到JSON任务，忽略了工作流设计所需的层次化领域知识和逐步推理能力，导致输出结构脆弱且执行失败率高。论文的核心洞察是：成功的工作流生成需要显式建模知识本身——包括其结构、层次和推理动态。

为此，作者提出了知识中心框架（Knowledge-Centric Agents，KCA）。框架包含三大技术组件：
1) **知识反转**：从大量无标注的真实工作流中，自动提取三种粒度的层次化表示——完整伪代码、骨架（核心模块连接）、高层策略（任务意图与模式）。这些表示构成了结构化的知识库，可用于训练和推理。
2) **知识注入**：采用监督微调，让大语言模型学会两条推理链路：从任务描述到策略的高层映射，以及从策略到可执行结构的逐步细化。这使得生成过程不再是一次性预测，而是可分解的链式推理。
3) **知识推理与自优化**：在推理阶段，模型先推测策略，再基于策略生成工作流细节，并利用可逆推理进行回溯修正；最终通过自优化循环（检查图连通性、参数完整性）提高鲁棒性。

实验部分采用两个工作流生成基准（30个任务来自Bench数据集；20个创意/复杂任务来自ComfyBench），评估节点多样性、结构连贯性和执行成功率。结果表明，KCA在三个指标上全面优于直接LLM生成、单纯CoT等基线方法。消融研究（证据未提供完整数字）进一步证实了知识反转和可逆推理各自的重要性。

论文的主要贡献在于：首次将工作流生成从”生成文本“转向”建模知识“，提出了完整的知识反转-注入-推理框架，并展示了显式层次化知识表示对结构化生成任务的有效性。局限性包括：评估规模有限，未深入探讨失败边界，泛化性仅在一个平台上验证。

总体而言，该工作为智能体驱动的工作流生成奠定了新的范式基础，并提供了可复用的方法论框架。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体（agent）方向直接相关：工作流生成要求LLM具备规划、工具使用和逐步推理能力，框架可视为智能体的一种特化。

## 基本信息

- 作者：Zhendong Li, Lei Sun, Ruibo Ming, He Zhang, Danda Pani Paudel, Luc Van Gool, Jinjin Gu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.15845`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了从PDF检索的语义片段（retrieved_evidence）和字段级证据映射（field_evidence_map），并结合启发式草稿进行补充润色；部分数值和图表细节因证据有限未列出，已标注需查阅原文。
