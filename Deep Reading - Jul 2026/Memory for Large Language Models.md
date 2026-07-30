---
user_id: "cheng tan"
paper_id: 5763
arxiv_id: "2607.25380v1"
title: "Memory for Large Language Models"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25380v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25380v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:57:59"
---
# Memory for Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：memory · large language models · taxonomy · implicit memory

## 一句话总结

本文提出一个系统性的、以架构为中心的大语言模型内存分类法，沿表示、更新动态和持久性三个正交轴构建，并通过内存视角整合分散的架构创新。

## 摘要

> Memory has evolved into a foundational architectural dimension in large language models (LLMs), shifting from an implicit byproduct of computation to a spectrum of explicit, controllable mechanisms. While recent advances introduce diverse strategies---spanning transient attention, recurrent state dynamics, parameter-efficient adaptations, and scalable lookup storage---this rapid evolution has led to a highly fragmented research landscape. In this survey, we present a systematic, architecture-centric taxonomy of memory in LLMs. Our framework characterizes memory along three orthogonal axes: representation (implicit versus explicit), update dynamics (offline versus online), and persistence (short-term versus long-term). We further formalize the granular mechanisms dictating memory writing, routing, state transitions, and consolidation. This unified perspective elucidates the conceptual boundaries between computation-coupled and independently addressable memory, effectively bridging disparate architectural paradigms. Additionally, we critically analyze hybrid memory architectures, system-level efficiency trade-offs, and multi-dimensional evaluation methodologies. By consolidating these scattered advancements into a cohesive framework, this survey charts the trajectory of memory-centric LLM design and provides a principled foundation for future innovations in scalable and adaptive language modeling.

Q1: 这篇论文试图解决什么问题？

当前LLM中内存机制的研究高度碎片化，各种方法（如暂态注意力、循环状态、参数高效适应、可扩展查找存储）缺乏统一的比较框架。领域内存在基本同构的问题（存储什么、何时以及如何更新、如何与前向计算交互、保留时长、存储粒度），但难以跨方法直接比较。因此需要一个原则性的结构化框架来整合这些分散的进展。

Q2: 有哪些相关研究？

相关研究涵盖多种内存策略：基于注意力的工作记忆（Transformer）、循环状态动态（如线性注意力、状态空间模型）、参数高效微调（Adapter、LoRA）、可扩展的外部查找存储（如记忆网络、检索增强架构）。此外，也存在其他内存中心调查，但本文以架构为中心的分类法统一了通常分离于长上下文、高效序列建模、测试时学习和外部内存文献中的机制。论文还与其他综述进行了定位对比。

Q3: 论文如何解决这个问题？

论文提出了一个三维分类法来刻画LLM内存机制：1) 表示维度：隐式内存（通过计算动态实现，如注意力、循环状态）vs 显式内存（独立可寻址存储，如键值记忆、参数池）；2) 更新动态维度：离线内存（仅在训练时更新）vs 在线内存（可在推理时更新）；3) 持久性维度：短期内存（有限步保留）vs 长期内存（跨长久保留）。在此基础上，形式化了内存的写入、路由、状态转换和整合等机制。论文还分析了混合内存架构的设计空间，讨论了系统效率权衡（如容量、延迟、计算开销），并提出了多维度评估框架。

Q4: 论文做了哪些实验？

作为综述论文，本文没有进行全新的实验。为了验证分类法的实用性，论文对多种代表性模型进行了分类和对比，包括标准Transformer、线性注意力模型（如Mamba、RWKV）、参数高效方法（如IA3、SSF）、显式记忆模型（如KNN-LM、Memorizing Transformer、Memory Networks）等。论文通过案例研究展示了不同内存机制在语言建模、长上下文理解、少样本学习等任务上的效果和效率权衡，但未提供统一的定量基准比较。

Q5: 发现了什么实验现象？

观测到的关键现象和趋势包括：1) 简单的注意力工作内存在多种任务上已表现优异，但集成多种内存机制可以带来效率、可扩展性和适应性上的进一步收益。2) 隐式内存（如循环状态）适合持续计算型任务，但难以独立寻址和读取；显式内存（如外部存储）提供更灵活的控制，但增加系统复杂性。3) 离线更新内存（如预训练权重）稳定但缺乏适应性；在线更新内存（如测试时微调、模型编辑）更灵活但面临遗忘和效率挑战。4) 短期和长期内存的划分取决于更新和驱逐策略，二者之间可以转换（如通过重复刷新将短期信息变为长期）。5) 不同内存机制之间存在互补性，混合架构（如Transformer额外挂载显式记忆）表现出潜力但尚未充分发挥。

Q6: 有什么可以进一步探索的点？

论文为未来研究提供了几个方向：1) 混合内存架构的优化，如何有机融合隐式和显式内存；2) 系统效率的进一步提升，包括硬件适配、稀疏访问和并行化；3) 更全面的评估方法论，覆盖内存效率、可扩展性、鲁棒性和可解释性；4) 自适应内存管理机制，根据任务动态调整内存参数；5) 将内存视角扩展到多模态、智能体系统和持续学习场景；6) 理论理解隐式内存的表达能力与局限。

Q7: 总结一下论文的主要内容

这篇综述论文以架构为中心，系统梳理了大型语言模型中的内存机制。论文首先指出内存已从计算的隐式副产品演变为显式可控的架构维度，但领域研究高度碎片化。为此，论文提出了一个三维分类法：表示（隐式/显式）、更新动态（离线/在线）和持久性（短期/长期）。沿着这些轴，论文详细形式化了内存的写入、路由、状态转换和整合等机制。在隐式内存方面，分析了标准注意力、线性注意力、状态空间模型和循环单元等；在显式内存方面，讨论了参数高效适应、外部可寻址存储和测试时学习等。论文还重点分析了混合架构（如Transformer+显式记忆）的设计和系统效率权衡，并提出了多维度评估框架。最后，论文总结了隐式内存的结构性局限以及显式内存的边界条件，为未来可扩展和自适应语言建模提供了原则性基础。整体而言，本综述为理解LLM内存提供了统一的设计空间和术语体系，有助于比较不同方法并指导新架构开发。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文为LLM内存研究提供了统一框架，有助于比较不同方法的优劣，尤其适用于关注架构设计的研究者。

## 基本信息

- 作者：Sining Zhoubian, Dan Zhang, Evgeny Kharlamov, Jie Tang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25380v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答参考了PDF检索证据。
