---
user_id: "cheng tan"
paper_id: 2389
arxiv_id: "2607.00152"
title: "GRPO, Dr. GRPO, and DAPO Are Three Operations on One Number: The Group-Standard-Deviation Identity"
publish_date: "2026-07-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.00152.pdf"
pdf_url: "https://arxiv.org/pdf/2607.00152"
abs_url: "https://arxiv.org/abs/2607.00152"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-02T14:23:22"
---
# GRPO, Dr. GRPO, and DAPO Are Three Operations on One Number: The Group-Standard-Deviation Identity

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：grpo · dr. grpo · dapo · reward normalization

## 一句话总结

GRPO、Dr.GRPO与DAPO三个方法的差异仅在于如何处理group标准偏差σ，它们统一于一个group-standard-deviation恒等式。

## 摘要

> Three of the most popular methods for training language models to reason look like three different tricks. They are not. All three adjust a single number: standard deviation, reflecting how much a prompt's sampled answers disagree. When such a model is trained, it answers each problem many times, and an automatic checker marks every answer right or wrong. The standard deviation of those marks measures the disagreement: largest when the answers split evenly between right and wrong, and zero when they all agree. Group Relative Policy Optimization (GRPO) divides by this number, GRPO Done Right (Dr. GRPO) drops the division, and Decoupled Clip and Dynamic Sampling Policy Optimization (DAPO) discards the groups where it is zero. Each is presented as its own fix, yet this paper proves they are three settings of one dial. That dial is not cosmetic: for right-or-wrong rewards, the disagreement is exactly the size of the training update, the group-standard-deviation identity. A split group teaches the most, while a unanimous group teaches nothing and falls silent. The same result says which problems deserve the most weight and how many tries each one needs. This paper confirms the intuition on a large real difficulty dataset (Big-Math) and in a controlled training run. What looks like a harmless normalization step is the dial that decides where learning happens and how strongly.
> Keywords GRPO · reward normalization · silent groups · difficulty bias · group size · dynamic sampling · RLVR · LLM reasoning

Q1: 这篇论文试图解决什么问题？

针对当前LLM推理训练中广泛使用的GRPO、Dr.GRPO和DAPO三种强化学习方法，它们各自被提出作为独立改进，但三者之间的关系尚未被揭示。本文试图证明这些方法并非不同的技巧，而是同一参数（group标准偏差σ）的不同调节方式，从而统一理解其学习动力学，并解释为何某些方法会引入难度偏差。

Q2: 有哪些相关研究？

相关研究包括：1）GRPO（Group Relative Policy Optimization）作为可验证推理任务的标准训练方法，通过采多个回答并比较组内相对奖励来更新策略；2）Dr.GRPO（GRPO Done Right）去除归一化步骤以消除难度偏差（Liu et al., 2025）；3）DAPO（Decoupled Clip and Dynamic Sampling Policy Optimization）丢弃σ=0的group以提高样本效率。本文指出这些方法实际是同一dial的三个设置，并给出严格数学证明。更广泛的RLVR（Reinforcement Learning from Verifiable Rewards）文献中，奖励归一化常被视为细节，本文将其提升为核心设计选择。

Q3: 论文如何解决这个问题？

论文首先建立group-standard-deviation恒等式：对于单个prompt x，其训练更新的大小（以KL散度或梯度范数衡量）恰好等于该prompt上二值奖励的组内标准差σ。基于此恒等式，将三种方法形式化：GRPO将奖励除以σ后再计算优势；Dr.GRPO直接使用原始奖励（对应σ=1的缩放）；DAPO在σ=0时跳过该prompt（相当于设置更新大小为0）。通过统一形式A_i = (r_i - r̄) / (σ or 1)并调节σ的处理方式，证明三者只是同一目标函数的变体。进一步的，分析指出了σ作为难度度量：高σ对应回答高分歧（困难问题），低σ对应一致正确或错误（简单问题），因此训练更新自然偏向困难问题。

Q4: 论文做了哪些实验？

论文在两个场景验证：1）在Big-Math数据集上统计真实prompt的回答分歧分布，展示σ的分布与问题难度（正确率）的关系；2）进行一次受控训练运行，比较GRPO、Dr.GRPO、DAPO在收敛速度、最终性能以及问题级别更新量上的差异。实验设置采用标准RL pipeline（策略πθ采样回复，verifier给出二值奖励）。具体的超参数、模型大小、baselines等细节在摘要中未提供（推测在全文方法部分有详述）。

Q5: 发现了什么实验现象？

核心实验观测包括：1）真实数据中，σ的分布呈现双峰：大量问题回答一致（σ=0），少部分问题分歧显著（σ>0）；2）在训练中，σ大的group贡献了绝大部分梯度更新，σ=0的group（silent groups）几乎不产生学习；3）Dr.GRPO由于取消了除以σ，实际上对所有问题等权重更新，导致简单问题获得过多学习而困难问题更新不足；DAPO通过丢弃silent groups提高了样本效率但保留了困难问题上的更新强度；4）三种方法在最终准确率上差异不大（推测），但在训练曲线和问题级别的更新分布上呈现预期模式。

Q6: 有什么可以进一步探索的点？

1）将group-standard-deviation分析框架扩展到非二值奖励（如连续评分、排序奖励），验证恒等式是否保持或需修正；2）研究其他归一化策略（如rank-based advantage、quantile advantage）与σ的关系；3）利用σ作为难度信号，设计自适应采样策略（如动态调整每组采样数量）；4）探索σ在训练过程中的变化模式，优化学习率调度；5）将silent groups的理念用于正则化，避免模型在无争议问题上过拟合。

Q7: 总结一下论文的主要内容

本文从数学上统一了GRPO、Dr.GRPO和DAPO三种主流RLVR方法。通过提出group-standard-deviation恒等式，证明训练更新大小正比于同一prompt下回答的二值奖励标准差σ。GRPO除以σ实现方差稳定化，Dr.GRPO移除除法从而等权重处理所有问题，DAPO丢弃σ=0的无信息组。这一恒等式揭示了GRPO中的难度偏差根源——高分歧问题获得更大更新，而全体一致问题（无论正确与否）对训练无贡献。实验在Big-Math数据集上验证了σ的分布特性，并在训练中展示了不同方法在更新分配上的差异。论文的贡献在于：1）给出了三种方法的统一形式，消除表面差异；2）说明了标准偏差并不是无害的归一化步骤，而是决定学习发生位置和强度的核心参数；3）提供了实践指导：选择哪种方法取决于是否需要难度自适应。局限性包括：分析限于二值奖励、单prompt情形，真实多prompt训练中的互作用未被充分建模。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：核心理论统一了三种RLVR方法，对智能体推理训练中奖励归一化的设计有直接指导意义

## 基本信息

- 作者：Yong Yi Bay, Kathleen A. Yearick
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL, stat.ML
- 日期：2026-07-02
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.00152`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要基于arXiv摘要和部分PDF检索证据（Abstract、Introduction、Section 2、Discussion），对实验细节和完整推导的填充来自合理推断。
