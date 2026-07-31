---
user_id: "cheng tan"
paper_id: 5994
arxiv_id: "2607.26760v1"
title: "Metis: Memory Foundation Model"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26760v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26760v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:16:27"
---
# Metis: Memory Foundation Model

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：memory foundation model · native memory · long-term memory · agent memory

## 一句话总结

本文首次提出记忆基础模型（memory foundation model），从形式化定义原生记忆（native memory）出发，构建了原型系统Metis，将记忆作为持续演化的内部状态集成到基础模型中。

## 摘要

> Recent advances in AI agents have increasingly internalized native capabilities into their underlying foundation models, giving rise to multimodal foundation models and large reasoning models. However, agent memory is still primarily implemented through external modules, leaving the native memory capability largely unexplored. In this paper, we take a first step toward this direction by introducing memory foundation models, which empower foundation models with native memory capabilities. We formalize native memory from two perspectives: a persistent and dynamically evolving memory state within the backbone, and native memory procedures that autonomously store and utilize information through model computation. We show that native memory offers advantages in architecture, end-to-end optimization, and efficiency. Based on this formulation, we propose Metis, the first prototype of memory foundation models. Metis introduces a new architecture that equips a foundation model with a native memory state, allowing historical information to be compressed into the model and accessed through memory attention. We construct large-scale memory-specific training data and introduce multiple optimization objectives to acquire these native memory procedures through mid-training. The online memory maintenance of Metis is gradient-free, and the memory update requires only a forward pass. At inference time, all learned model weights remain frozen, while the native memory states are autonomously transformed through standard forward computation. Through extensive experiments, we show that Metis exhibits native memory capabilities and further provide a detailed analysis of its strengths, limitations, and behaviors. To facilitate future research on memory foundation models, we release our project and model checkpoints.
> Date: July 30, 2026
> Correspondence: lizy@memtensor.cn, xu.chen@ruc.edu.cn
> Author Legend: \*Co-first authors, †Corresponding authors
> Code: https://github.com/MemTensor/Metis
> Model: https://huggingface.co/collections/IAAR-Shanghai/metis

Q1: 这篇论文试图解决什么问题？

1. 当前AI智能体（agent）的记忆大多依赖外部模块（如RAG、向量数据库、外部存储），这些模块与基础模型分离，导致架构非端到端、优化不联合、效率瓶颈等问题。
2. 虽然基础模型已内化了多模态和推理等能力，但记忆作为智能体核心能力之一仍停留在外部组件层面，缺乏原生支持。
3. 现有尝试（如长上下文Transformer、记忆增强网络）要么受限于固定上下文长度，要么未扩展到大规模基础模型，且不能完全自主地存储和检索。
4. 因此，论文旨在填补这一空白：将记忆作为基础模型的内在能力（native memory），使其具备持久、动态且通过前向计算自主管理的记忆系统。

Q2: 有哪些相关研究？

1. **外部记忆模块**：智能体系统中常用的记忆机制，如MemGPT、Generative Agents、RAG等，通过向量数据库、缓存或结构化存储实现长期记忆，但存在信息检索延迟、非端到端训练、容量管理复杂等局限。
2. **长上下文Transformer**：如Transformer-XL、Compressive Transformer、Infini-Attention等，通过状态循环或压缩机制扩展有效上下文，但通常受限于固定长度或无法实现选择性遗忘，且记忆状态与模型权重分离。
3. **记忆增强神经网络**：NTM、DNC等引入外部可读写记忆矩阵，但未在大规模预训练场景下成功，二次训练成本高，且与当代基础模型架构（如注意力）的融合不成熟。
4. **基础模型内化能力的趋势**：多模态基础模型、推理基础模型等将原本外部的处理模块（视觉编码器、推理链）内化，本文延续这一趋势，首次系统性地将记忆内化。
5. **持续学习与记忆**：相关工作在避免灾难性遗忘方面有探索，但本文侧重记忆的存取机制而非只关注稳定性。
（注意：由于检索片段有限，其余相关研究基于常见文献推断；具体引文需查阅原文。）

Q3: 论文如何解决这个问题？

1. **原生记忆的形式化定义**：
 - **记忆状态**：一个持久且随时间演化的内部状态向量，存在于模型骨干网络各层之间，随输入更新。
 - **记忆程序**：通过模型前向计算实现的信息自主存储和检索过程，无需外部模块干预。
2. **Metis架构设计**：
 - 在标准Transformer中引入**Metis块**：每个Transformer层之后附加一个记忆状态接口，历史信息的压缩表示通过**记忆注意力**（Memory Attention）机制与当前输入交互。
 - **原生记忆状态**（Native Memory State）作为可学习参数初始化，在每次前向传播中被更新，后续层可以通过注意力读取该状态。
3. **训练方法**：
 - 预训练阶段：保持标准语言模型目标（如下一词预测）。
 - **中期训练（mid-training）**：构造大规模记忆专用训练数据（包括长程依赖问答、多轮对话、摘要、记忆召回等任务），并引入多目标优化损失，包括记忆存储损失（如何高效压缩信息）、检索损失（如何准确获取已存储信息）等，使模型学习记忆程序。
4. **在线使用**：
 - 记忆更新仅需一次前向传播（gradient-free），无需反向传播。
 - 推理时所有模型权重冻结，原生记忆状态自主演化。
5. **与外部记忆的关键区别**：记忆状态在模型内部，通过模型计算自动管理，不需要显式的读写指令或外部存储。

Q4: 论文做了哪些实验？

论文实验部分尚未获详细检索证据，根据摘要和常见实践推断可能包括以下内容（确切设计需查阅原文）：
1. **基准任务**：选取需要长期记忆的多种任务，例如长文本QA、多轮对话依赖、上下文指代消解、摘要一致性评估等。
2. **基线对比**：可能包括普通Transformer（无长期记忆）、带有外部检索的模型（如RAG）、长上下文Transformer（如Longformer、Transformer-XL）以及特定记忆模型（如MemGPT）。
3. **消融研究**：控制记忆状态维度、注意力机制变体（如是否使用记忆注意力）、训练数据组成、优化目标组合等。
4. **扩展性分析**：不同模型规模（参数数量）下记忆能力的变化，以及记忆序列长度对性能的影响。
5. **效率评估**：计算开销（FLOPs、推理时延）与外部记忆方法的对比。
（具体数值、数据集名、超参数需原文补充。）

Q5: 发现了什么实验现象？

由于未能获取完整实验结果，以下为合理推断：
1. **记忆能力验证**：Metis在需要长期依赖的任务上显著优于无记忆Transformer，展示了内化记忆的有效性。
2. **与外部记忆比较**：在端到端优化方面，Metis可能更高效且性能稳定；但在需要精确检索大量事实的任务中，外部方法可能仍占优。
3. **消融趋势**：记忆状态维度过小会限制容量；训练数据多样性帮助记忆程序泛化；多目标优化平衡存储与检索。
4. **扩展行为**：随着模型增大，记忆能力进一步提升；记忆长度增加时性能平滑下降而非突变。
5. **失败案例**：在需要长时间跨度精确数字记忆的任务上，Metis可能出错；存在信息混淆或记忆衰退现象。
（以上推断需通过原文实验章节确认或修正。）

Q6: 有什么可以进一步探索的点？

1. **扩大规模**：在更大参数量、更大数据量下验证记忆能力，探索涌现行为。
2. **多模态记忆**：将原生记忆扩展到图像、视频、语音等多模态输入。
3. **推理与记忆结合**：将记忆内化与推理内化（如链式思维）融合，构建更强智能体。
4. **遗忘与更新机制**：设计显式的遗忘策略或选择性更新，避免信息过时或冲突。
5. **可解释性**：分析记忆状态的高维表示，理解模型存储和决策过程。
6. **混合系统**：探索原生记忆与外部记忆协同工作的架构，兼顾效率与精度。
7. **下游任务迁移**：将预训练的记忆基础模型应用于对话系统、推荐、机器人等场景，评测其实用价值。

Q7: 总结一下论文的主要内容

论文《Metis: Memory Foundation Model》首次系统和形式化地提出记忆基础模型的概念，旨在将智能体的记忆能力从外部模块转移到基础模型内部，实现原生记忆。研究者首先定义了‘原生记忆’的两个核心要素：一是持久、动态演化的记忆状态（嵌入在模型骨架中的内部表示），二是通过模型前向计算自主完成信息存储和利用的记忆程序。基于这一定义，他们设计了Metis——第一个记忆基础模型原型。Metis在Transformer架构中引入‘Metis块’，使模型具备原生记忆状态，并通过记忆注意力机制访问历史压缩信息。训练分为两阶段：预训练语言模型后，利用大规模记忆专用数据集（如长程QA、对话、摘要）进行中期训练（mid-training），采用多目标优化让模型学会存储和检索。运行时记忆更新只需前向传播，无需梯度，权重冻结，记忆状态自动演化。实验部分虽未获详细数据，但摘要称Metis展现了原生记忆能力，并分析了其优势与局限。论文贡献包括：首次形式化原生记忆、提出原型架构、实验验证、开源资源。讨论中指出了外部记忆的局限以及原生记忆的潜在好处（端到端优化、效率）。尽管是初步探索，但为后续记忆基础模型研究奠定了基础。相关研究涉及外部记忆模块、长上下文Transformer、记忆增强网络等，本文填补了内化记忆的空白。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与AI Agent领域高度相关，直接解决agent长期记忆内化问题。

## 基本信息

- 作者：Zeyu Zhang, Ziliang Guo, Yihang Sun, Xichong Zhang, Xixuan Hao, Zehao Lin, Yang Zhang, Xiaoyan Zhao, Tong Shen, Bo Tang, Zhi-Qin John Xu, Junchi Yan, Haofen Wang, Xu Chen, Feiyu Xiong, Zhiyu Li, Tat-Seng Chua
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26760v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要依据论文摘要和检索命中片段（Abstract、Introduction、Conclusion的部分内容），未获取完整PDF，因此部分字段（如实验具体数值、基线结果、详细数据）存在缺口，需查阅原文确认。
