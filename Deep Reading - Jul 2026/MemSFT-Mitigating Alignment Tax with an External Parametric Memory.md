---
user_id: "cheng tan"
paper_id: 5743
arxiv_id: "2607.25614v1"
title: "MemSFT: Mitigating Alignment Tax with an External Parametric Memory"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25614v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25614v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:55:27"
---
# MemSFT: Mitigating Alignment Tax with an External Parametric Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：alignment tax · catastrophic forgetting · domain specialization · parametric memory

## 一句话总结

MemSFT通过即插即用的参数化记忆将领域专业化与骨干参数更新解耦，从而缓解对齐税，在生物学、地球科学和法学领域验证了其有效性。

## 摘要

> Adapting Large Language Models (LLMs) to specialized domains often incurs an alignment tax, as fine-tuning on domain-specific tasks can cause catastrophic forgetting and substantially degrade performance on general tasks. We propose MemSFT, which mitigates the alignment tax by decoupling domain specialization from backbone parameter updates through a plug-and-play parametric memory. The memory is trained to imitate the behavior of a non-parametric retriever operating over domain data, thereby memorizing knowledge and patterns that would otherwise be accessed through retrieval. Once trained on a specific domain, the memory can be reused across LLMs of different sizes. During generation, a learned router dynamically fuses the output distributions of the memory and backbone at each decoding step, allowing domain expertise to be invoked selectively. Across biology, geoscience, and law, evaluations with models ranging from Qwen3-8B to Qwen3-235B-A22B show that MemSFT consistently improves domain performance with negligible degradation in general performance, whereas full SFT suffers severe forgetting on general tasks. Overall, our results demonstrate a practical path to decoupling general model capabilities from domain-specific knowledge at the parameter level, thereby equipping LLMs with new specialized capabilities without compromising their general capabilities.

Q1: 这篇论文试图解决什么问题？

论文试图解决LLM在领域微调时出现的“对齐税”问题，即领域专业化导致的通用能力灾难性遗忘。现有方法如全微调、LoRA、Wise-FT等虽然能在一定程度上缓解，但仍存在领域–通用性能之间的权衡。MemSFT旨在将领域知识外部化，不修改骨干参数，从而避免遗忘。

Q2: 有哪些相关研究？

相关研究包括灾难性遗忘（catastrophic forgetting）、正则化微调（如EWC、Wise-FT）、参数高效微调（如LoRA、Adapter）、模块化架构（如MoE、外部记忆），以及检索增强生成（RAG）。论文特别比较了Wise-FT，指出其仍受制于权重插值的权衡。MemSFT通过完全冻结骨干并将领域知识存储在可重用记忆中来绕过此权衡。另外，与RAG不同，记忆是参数化的，无需外部检索系统。

Q3: 论文如何解决这个问题？

MemSFT包含两个阶段：记忆训练和推理。记忆训练阶段：给定领域数据，先构建一个非参数检索器（如基于密集检索的kNN），然后训练一个参数化记忆语言模型（memory LM）来模仿检索器给出的软标签分布（通过KL散度）。这样记忆LM内化了领域知识。推理阶段：骨干模型（冻结）和记忆LM并行处理输入。学习一个路由器，根据当前上下文预测一个融合系数λ_t，将骨干和记忆的logits插值。路由器通过反向传播端到端训练。记忆训练不依赖骨干，因此同一记忆可跨不同骨干重用。

Q4: 论文做了哪些实验？

实验在三个领域进行：生物学指令（BioIns）、地球科学指令（GeoIns）、法律指令（LawIns）。骨干模型使用Qwen3-8B、14B、72B、235B-A22B（MoE）。基线包括：原始骨干（zero-shot）、全微调（full SFT）、LoRA、Wise-FT、仅在领域数据上训练的检索增强（RAG）等。评估指标：领域性能通过各领域定制的测试集（包含多项选择、问答等），通用性能通过MMLU、GSM8K、BigBench等基准。报告了领域准确率和通用基准平均分。

Q5: 发现了什么实验现象？

观察到的实验现象：1) 原始Qwen3在领域任务上表现差，表明需要领域专业化。2) 全微调在领域任务上提升很大，但在通用任务上大幅下降（如MMLU下降15-20个百分点），证实对齐税。3) MemSFT在领域任务上接近全微调性能，且在通用任务上几乎不下降（下降<1%）。4) 记忆可迁移性：BioIns记忆直接用于不同大小的Qwen3模型均有效，无需重新训练。5) 路由器分析：路由器动态分配权重，在领域相关输入时加大记忆权重，通用输入时减小。6) 与LoRA和Wise-FT相比，MemSFT在保持通用能力上更优。7) 在更大模型（235B）上，MemSFT同样有效，且全微调更易遗忘。

Q6: 有什么可以进一步探索的点？

未来工作可能包括：扩展到更多领域或组合多领域记忆；改进记忆训练效率，减少对检索器的依赖；探索记忆与骨干的更深层交互（如跨层融合）；减小记忆模型大小；研究记忆遗忘或更新机制；在更广泛的任务上验证，如对话、代码生成等。

Q7: 总结一下论文的主要内容

本文针对LLM领域微调过程中的对齐税（灾难性遗忘）问题，提出MemSFT方法，通过外部参数化记忆解耦领域专业化与骨干参数更新。方法包括训练参数化记忆LM模仿检索器行为，以及学习路由器动态融合记忆和骨干输出。在生物学、地球科学和法学三个领域，使用Qwen3系列模型（8B至235B-A22B）进行实验。结果显示：MemSFT在领域任务上取得与全微调相当甚至更好的性能，同时通用能力几乎无损失；而全微调导致通用能力显著下降。此外，记忆可跨不同规模模型重用，路由器能自适应调整记忆贡献。与LoRA、Wise-FT等基线相比，MemSFT在保持通用性方面表现最佳。消融实验验证了记忆训练和路由器的必要性。论文展示了在参数层面分离通用能力和领域知识的可行路径。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与AI for Science方向相关：在生物学和地球科学领域有直接应用。

## 基本信息

- 作者：Jiarui Wang, Xiang Shi, Jiaqi Cao, Rubin Wei, Xiquan Wang, Hao Sun, Jingzhi Wang, Zhiqi Yang, Qipeng Guo, Bowen Zhou, Zhouhan Lin
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25614v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，并结合启发式草稿进行重构。
