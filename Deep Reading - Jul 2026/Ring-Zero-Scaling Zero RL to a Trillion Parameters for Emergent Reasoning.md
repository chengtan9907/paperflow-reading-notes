---
user_id: "cheng tan"
paper_id: 3838
arxiv_id: "2607.12395v1"
title: "Ring-Zero: Scaling Zero RL to a Trillion Parameters for Emergent Reasoning"
institution: "Ant Group (蚂蚁集团), Tsinghua University (清华大学)"
publish_date: "2026-07-14"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.12395v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.12395v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-16T01:54:35"
---
# Ring-Zero: Scaling Zero RL to a Trillion Parameters for Emergent Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：zero rl · scaling laws · trillion parameters · emergent reasoning

## 一句话总结

本研究通过算法与系统优化，成功将无需人工标注的零样本强化学习（Zero RL）扩展至万亿参数规模，揭示了模型在推理过程中的涌现行为及训练动力学。

## 摘要

> Reinforcement learning with verifiable rewards without human-annotated data, often referred to as zero RL, has emerged as a powerful paradigm for eliciting chain-of-thought reasoning. However, due to computational constraints, existing studies are largely restricted to small models, leaving the training dynamics and emergent capabilities at a large scale unexplored. To meaningfully explore this frontier, we aim to elicit high-quality reasoning behaviors from the model. However, we find that naive scaling often suffers from poor readability, token redundancy, and a lack of adaptive reasoning depth. To address these challenges, we present a stable and efficient training pipeline, incorporating algorithmic and system optimizations such as clipped importance sampling, training-inference ratio correction, and mixed-precision control. Our experiments offer three key findings that validate the "bitter lesson" of scaling: (1) scaling to 1T parameters significantly enhances sample efficiency and performance ceilings; (2) the training process progresses sequentially through an initial discovery phase followed by a sharpening phase; and (3) the model spontaneously develops advanced cognitive behaviors, including anthropomorphism, structured formatting, self-verification, parallel reasoning, and context anxiety, rendering hand-crafted heuristics redundant. Evaluated on seven mathematical benchmarks, Ring-2.5-1T-Zero achieves competitive performance. Additionally, to assess CoT quality beyond final-answer correctness, we propose a structured evaluation framework across three dimensions: comprehensibility, reproducibility, and efficiency, where our model demonstrates clear advantages in producing structured and concise reasoning traces. By sharing our observed emergent phenomena, we hope to provide the community with deeper insights into scaling behaviors, particularly at the 1-trillion scale.

Q1: 这篇论文试图解决什么问题？

该研究旨在解决在大规模（万亿参数）环境下进行零样本强化学习（Zero RL）时遇到的核心瓶颈：
1. **训练稳定性挑战**：在 1T 规模下，强化学习算法对超参数极度敏感。传统的策略梯度方法在处理低概率 Token 时容易产生剧烈的梯度波动，导致训练崩溃。
2. **原生缩放的副作用**：研究发现，简单的模型扩容并不能直接带来高质量的推理。模型往往会出现“Token 膨胀”现象，即为了获取奖励而生成大量冗余、无意义的推理步骤，导致可读性极差。
3. **推理深度的僵化**：现有模型通常无法根据问题的难易程度动态调整推理长度，在简单问题上浪费计算资源，而在复杂问题上推理不足。
4. **评价维度的单一性**：现有的评估主要关注最终答案的正确性，忽略了推理过程（CoT）本身的逻辑严密性、可解释性和效率，缺乏对推理质量的深度度量。

Q2: 有哪些相关研究？

论文将相关研究分为以下几个维度进行对比：
1. **Zero RL 与思维链（CoT）**：回顾了从 OpenAI o1 到 DeepSeek-R1 等利用强化学习提升推理能力的工作，指出这些工作大多集中在较小规模或未公开 1T 规模下的训练细节。
2. **强化学习稳定性研究**：讨论了 PPO、GRPO 等算法在稳定性上的权衡。本文借鉴了裁剪重要性采样（Clipped Importance Sampling）的思想，并针对超大规模参数进行了适配。
3. **Scaling Laws 在推理任务中的应用**：分析了计算量、参数量与推理性能之间的关系，探讨了“苦涩的教训（Bitter Lesson）”在推理领域的体现。
4. **推理质量评估**：对比了传统的基于准确率的评估与新兴的基于 LLM-as-a-Judge 的过程评估方法。

Q3: 论文如何解决这个问题？

研究团队构建了一个名为 Ring-Zero 的高效训练流水线，其核心技术方案包括：
1. **算法优化（稳定性增强）**：
 - **裁剪重要性采样（Clipped Importance Sampling）**：限制策略更新的幅度，防止模型在探索过程中偏离过远。
 - **训练-推理比例修正（Ratio Correction）**：针对大规模分布式训练中的采样偏差进行校正，确保梯度估计的准确性。
 - **KL 散度约束**：引入动态 KL 惩罚项，防止模型在强化学习过程中发生严重的模式坍塌（Mode Collapse）。
2. **系统优化（效率提升）**：
 - **混合精度控制**：在保证数值稳定性的前提下，利用 FP8/BF16 混合精度加速万亿参数模型的训练。
 - **自适应推理模式**：在训练后期引入多种推理模式，使模型能够根据输入问题的复杂度动态选择推理深度。
3. **三维推理评估框架**：
 - **可理解性（Comprehensibility）**：评估推理轨迹是否易于人类阅读和逻辑追踪。
 - **可复现性（Reproducibility）**：评估推理步骤是否能被其他模型或系统稳定复现。
 - **效率（Efficiency）**：评估模型在达成正确答案时所消耗的 Token 数量，惩罚冗余生成。

Q4: 论文做了哪些实验？

实验设计详尽，旨在验证 1T 规模下的 Scaling 效应：
1. **模型配置**：核心模型为 Ring-2.5-1T-Zero，参数量达到 1 万亿。
2. **基准测试**：在 GSM8K、MATH、AIME、AMC 等 7 个主流数学推理基准上进行评估。
3. **对比基线**：包括不同规模的 Base 模型、经过 SFT 的模型以及其他开源的 RL 推理模型（如 DeepSeek-R1 系列）。
4. **消融实验**：重点考察了 KL 惩罚、比例修正以及预先进行的自监督蒸馏（Self-distillation）对训练稳定性的贡献。
5. **CoT 质量评估**：采用 LLM-as-a-Judge 协议，对模型生成的推理轨迹进行多维度打分。

Q5: 发现了什么实验现象？

论文记录了多个关键的实验现象：
1. **两阶段训练动力学**：
 - **发现阶段（Discovery Phase）**：模型初步探索出能够获得奖励的推理路径，此时性能提升较快，但推理轨迹较为混乱。
 - **磨砺阶段（Sharpening Phase）**：模型开始优化推理路径，去除冗余，逻辑变得更加严密和结构化。
2. **涌现的高级认知行为**：
 - **拟人化（Anthropomorphism）**：模型在推理中表现出类似人类的语气，如“让我再检查一下”、“这里可能出错了”。
 - **自我验证（Self-verification）**：在得出结论前，模型会自发地回溯之前的步骤进行验算。
 - **语境焦虑（Context Anxiety）**：当面对信息不足的问题时，模型会表现出对不确定性的识别，并尝试通过多路径推理来覆盖可能性。
3. **Scaling Trend**：1T 模型在样本效率上远超小模型，达到相同准确率所需的训练步数显著减少，且性能上限更高。
4. **负结果与张力**：发现如果不加干预，模型会产生“长度惯性”，即倾向于通过增加长度来换取微小的奖励提升，这会导致推理效率下降。

Q6: 有什么可以进一步探索的点？

1. **跨领域迁移**：将 1T 规模的 Zero RL 经验从数学和代码领域扩展到科学发现（AI for Science）和通用逻辑推理领域。
2. **推理成本优化**：虽然 1T 模型性能强劲，但推理成本极高，未来需探索如何通过蒸馏将涌现的推理能力迁移到更小的模型中。
3. **动态计算分配**：进一步研究如何让模型在推理时根据问题难度精准分配计算量（Compute-optimal Inference）。
4. **多模态推理**：将可验证奖励机制引入多模态大模型，实现视觉与逻辑的联合强化学习。

Q7: 总结一下论文的主要内容

本文是关于超大规模强化学习推理能力的深度探索。研究团队成功将 Zero RL 训练流水线扩展至万亿参数规模，并系统性地解决了大规模训练中的稳定性与推理质量问题。通过引入裁剪重要性采样、比例修正和 KL 约束，Ring-Zero 克服了万亿参数模型在强化学习中的发散风险。实验不仅证明了 1T 模型在数学推理任务上的领先地位，更重要的是揭示了大规模模型在没有人工干预的情况下，能够自发学习到诸如自我纠错、结构化思考和拟人化表达等高级认知功能。论文提出的三维评估框架为未来推理模型的评价提供了新的标准。这项工作验证了“苦涩的教训”，即通过大规模算力和通用算法的结合，可以实现超越手工设计启发式方法的智能涌现。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于关注大模型 Scaling Law 的研究者，本文提供了 1T 规模 RL 的实战经验。

## 基本信息

- 作者：Xinyu Tang, Gangqiang Cao, Yurou Liu, Yuliang Zhan, Xiaochong Lan, Yifan Li, Yuchen Yan, Han Peng, Zican Dong, Zhenduo Zhang, Tianshu Wang, Xinyu Kong, Zujie Wen, Wayne Xin Zhao, Zhiqiang Zhang, Jun Zhou
- 机构：Ant Group (蚂蚁集团), Tsinghua University (清华大学)
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-14
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2607.12395v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于训练稳定性策略（4.4.2）、CoT 质量评估（4.3）以及 1T 规模下的实验观察（5.4）等核心章节。
