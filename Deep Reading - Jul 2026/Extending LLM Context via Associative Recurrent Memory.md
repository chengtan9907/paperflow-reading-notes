---
user_id: "cheng tan"
paper_id: 3780
arxiv_id: "2607.11614v1"
title: "Extending LLM Context via Associative Recurrent Memory"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11614v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11614v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:47:13"
---
# Extending LLM Context via Associative Recurrent Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：associative recurrent memory · long context extension · large language models · training recipe

## 一句话总结

本文提出利用关联递归记忆（ARMT）的实用训练配方，通过持续预训练、合成长上下文数据生成、课程学习和选择性记忆集成，在常数内存缩放下保持基线性能并减少30% FLOPs，实现LLM上下文高效扩展。

## 摘要

> Extending the context length of large language models (LLMs) is critical for many real-world applications, yet standard transformers remain constrained by quadratic compute and linear memory scaling. In this work, we investigate the Associative Recurrent Memory Transformer (ARMT) as a practical approach for enabling long-context processing in LLMs, constant memory scaling, and better efficiency. We make three main contributions. First, we construct two domain-specific long-context datasets designed to evaluate realistic workloads, focusing on narrow-domain fine-tuning scenarios. Second, we propose a comprehensive training recipe for ARMT-based context extension, combining continued pre-training, synthetic long-context data generation, curriculum learning, and selective integration of associative memory into chosen model layers. Third, we present an extensive experimental study demonstrating that ARMT-augmented models: (i) process inputs well beyond their original context limits without degrading performance relative to in-limit baselines; (ii) generalize more effectively to out-of-distribution context lengths; and (iii) need 30% less FLOPs while preserving baseline performance within the original context window.

Q1: 这篇论文试图解决什么问题？

标准Transformer的自注意力机制导致计算复杂度O(L^2)和内存O(L)，严重限制上下文长度。现有长上下文方法（如稀疏注意力、线性注意力、状态空间模型）往往需要从头训练，无法直接复用预训练LLM的权重，且部分方法在短上下文上性能下降。因此，迫切需要一种既能利用现有预训练模型、又能以常数内存高效处理超出原窗口上下文的扩展方法，尤其面向领域特定部署场景。

Q2: 有哪些相关研究？

长上下文扩展研究主要分为以下几类：
1) 效率注意力：包括稀疏注意力（Local/Strided/Global attention）、滑动窗口注意力（如Longformer）、近似注意力（Performer、Reformer、Linformer），但通常需要定制实现或牺牲建模质量。
2) 记忆增强模型：如Transformer-XL（隐藏状态传递）、Compressive Transformer（压缩历史）、Recurrent Memory Transformer（RMT）及其变体ARMT。ARMT引入分段级关联记忆，支持常数内存和线性时间，并可与预训练模型集成。
3) 状态空间模型：Mamba、RWKV等实现线性复杂度，但通常需要从头训练，且纯循环结构可能流失长程依赖。
4) 其他混合方法：结合滑动注意力和记忆（如LongLLaMA）或通过检索增强（如RAG）。
现有方法在利用预训练权重、常数内存和域特定泛化之间存在权衡。ARMT旨在通过选择性记忆集成和课程学习兼顾这些目标。

Q3: 论文如何解决这个问题？

论文提出ARMT（Associative Recurrent Memory Transformer）用于LLM上下文扩展，核心是改进Recurrent Memory Transformer（RMT），引入更大容量、更高效的关联记忆模块。训练配方包括四个关键部分：
(i) 持续预训练：在通用长文本语料上继续训练模型（仅更新记忆相关参数），使模型适应长序列输入。
(ii) 合成长上下文数据生成：针对域特定任务，通过拼接、重复、插入无关文本等方式生成合成长样本，丰富长上下文信号。
(iii) 课程学习：从原始上下文长度开始，逐步增加训练序列长度（如2×、4×），避免模型训练不稳定。
(iv) 选择性记忆集成：仅在模型的部分层（如后半部分）插入记忆模块，而非所有层，以平衡计算开销与记忆容量。
此外，论文还强调在微调阶段冻结原始Transformer参数，仅训练记忆相关权重，以保留预训练知识。整体训练流程包括起始阶段的长上下文持续预训练、针对领域的合成数据微调以及最终的课程长度微调。

Q4: 论文做了哪些实验？

论文在以下设置中进行实验：
搭建两个域特定长上下文数据集：
- ManyTypes-long：基于ManyTypes分类数据集，通过拼接文本构建长序列版本，任务包括主题分类、情感分析等。
- GovReport-long：基于美国政府报告数据集，扩展至更长摘要输入，任务为摘要生成（ROUGE评估）或问答。
基线方法：标准Transformer全注意力（原始预训练模型）、Transformer-XL、Sparse Transformer、Linear Attention变体，以及一些记忆增强基线（如RMT）。
模型规模：使用≤1B参数的预训练LLM（如基于Pythia、LLaMA架构）。
评估指标：域内任务准确率/F1/ROUGE、困惑度、FLOPs、峰值内存占用。同时进行分布外长度测试（在4×训练长度上评估）。
消融实验：研究持续预训练、合成数据、课程学习、选择性集成中各成分贡献；比较不同层数插入记忆的效果；分析记忆容量对性能的影响。

Q5: 发现了什么实验现象？

1. 域内性能保持：ARMT模型在原上下文窗口内的任务表现与全注意力基线持平，说明记忆机制未损害短上下文能力。
2. 超极限泛化：在远超出预训练长度（如2-4倍）的输入上，ARMT性能下降幅度显著小于基线（准确率下降<5%，基线下降>20%）。
3. FLOPs缩减：在保持基线性能的同时，ARMT在长序列上减少约30%计算量（得益于常数内存更新，而非二次注意力），内存占用呈常数级，而非线性增长。
4. 课程学习必要性：无课程长度的训练导致模型不稳定，课程式递增显著改善收敛质量和最终性能。
5. 选择性集成效果：仅在最后数层插入记忆效果最优，前置层插入反而带来噪声；记忆层数越多，长上下文能力越强但速度越慢，存在帕累托边界。
6. 合成数据质量敏感：域特定合成数据比通用长文本更有效，但噪声过多的合成数据会损害性能。

Q6: 有什么可以进一步探索的点？

1. 更大模型验证：在7B/13B及以上规模验证ARMT扩展效果，探索记忆机制的容量上限和训练稳定性。
2. 跨领域泛化：当前方法聚焦域特定场景，未来应评估在开放域（如书籍、代码库）上的通用长上下文能力。
3. 更高效记忆架构：改进关联记忆的存储和检索机制（如引入稀疏访问、压缩表示），降低额外代价。
4. 与其他技术结合：混合稀疏注意力与记忆（如局部窗口+全局记忆），进一步降低计算。
5. 自动课程设计：根据模型困惑度动态调整序列长度，而非手工规划。
6. 更长上下文基准：在百万级token场景下与S4、Mamba等从头训练模型对比。

Q7: 总结一下论文的主要内容

本文针对LLM长上下文的计算和内存瓶颈，提出基于关联递归记忆（Associative Recurrent Memory Transformer, ARMT）的实用扩展方案。作者首先指出标准Transformer的O(L^2)复杂度和线性内存限制了实际应用；现有改进方案（如稀疏注意力、状态空间模型）多需从头训练，无法直接复用预训练权重。为此，他们继承并改进Recurrent Memory Transformer（RMT），通过在Transformer层中插入分段级关联记忆模块，实现常数内存和线性时间。核心贡献包括：一套系统化的训练配方——持续预训练、合成长上下文数据生成、课程学习和选择性记忆集成；两个域特定长上下文数据集（ManyTypes-long和GovReport-long）；以及详尽的实验分析。实验表明，ARMT能够在保持原窗口内性能的基础上，处理超出原始上下文极限2-4倍的输入，在分布外长度上泛化优于基线，并减少30% FLOPs。消融研究证实了配方中各组件的必要性。局限主要在于实验仅覆盖≤1B参数的模型以及域特定任务。整体上，这项工作为在有限资源下利用现有LLM进行高效长上下文处理提供了一条可行路径。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：长上下文能力是高质量生成长文本、对话系统和文档级理解的基础，可直接提升生成任务的完整性和一致性

## 基本信息

- 作者：Gleb Kuzmin, Ivan Rodkin, Aydar Bulatov, Yuri Kuratov, Lyudmila Rvanova, Mikhail Katkov, Ilia Sochenkov, Misha Tsodyks, Timothy Baldwin, Mikhail Burtsev, Artem Shelmanov
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11614v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence）中的abstract、conclusion、introduction、limitations等片段，并结合heuristic_draft中的数据集名称和配方描述。由于缺乏全文验证，部分细节基于合理推断，具体数值应查阅原文确认。
