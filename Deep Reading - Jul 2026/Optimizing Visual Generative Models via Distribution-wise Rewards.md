---
user_id: "cheng tan"
paper_id: 2470
arxiv_id: "2607.02291"
title: "Optimizing Visual Generative Models via Distribution-wise Rewards"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02291.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02291"
abs_url: "https://arxiv.org/abs/2607.02291"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T21:59:51"
---
# Optimizing Visual Generative Models via Distribution-wise Rewards

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：distribution-wise reward · reinforcement learning · visual generation · reward hacking

## 一句话总结

提出一种通过分布级奖励微调生成模型的框架，使用子集替换策略高效计算分布奖励，缓解样本奖励带来的奖励破解和模式坍塌问题，显著提升图像生成质量。

## 摘要

> Conventional reinforcement learning strategies for visual generation typically employ sample-wise reward functions, yet this practice frequently results in reward hacking that degrades image diversity and introduces visual anomalies. To address these limitations, we present a novel framework that finetunes generative models using distribution-wise rewards, ensuring better alignment with real-world data distributions. Unlike rewards that evaluate samples individually, distribution-wise reward accounts for the data distribution of the samples, mitigating the mode collapse problem that occurs when all samples optimize towards the same direction independently. To overcome the prohibitive computational cost of estimating these rewards, we introduce a subset-replace strategy that efficiently provides reward signals by updating only a small subset of a generated reference set. Additionally, we apply RL to optimize post-hoc model merging coefficients, potentially mitigating the train-inference inconsistency caused by introducing stochastic differential equation (SDE) in regular RL practices. Extensive experiments show our approach significantly improves FID-50K across various base models, from 8.30 to 5.77 for SiT and from 3.74 to 3.52 for EDM2. Qualitative evaluation also confirms that our method enhances perceptual quality while preserving sample diversity.

Q1: 这篇论文试图解决什么问题？

论文试图解决两个问题：1) 样本级奖励函数在RL微调视觉生成模型时导致奖励破解（diversity下降、视觉异常），因为所有独立样本朝同一方向优化，引发模式坍塌；2) 现有RL方法中SDE训练与ODE推理的不一致性，以及分布奖励计算的高昂成本。

Q2: 有哪些相关研究？

论文引用了多种现有RL微调扩散模型的方法，如Fan等人将去噪过程建模为MDP并采用样本级奖励；Liu等人、Xue等人、Li等人也使用类似框架。此外，后验模型合并系数优化是相关方向。现有工作普遍面临reward hacking和模式坍塌问题。

Q3: 论文如何解决这个问题？

提出分布级奖励（distribution-wise reward），不再对每个样本独立打分，而是考虑生成样本集合的整体分布，使用分布相似性度量（如FID）作为奖励。为降低计算成本，提出子集替换策略：维护一个参考集，每次迭代只替换其中的一小部分生成样本，再计算这些样本与真实分布的奖励。此外，将RL应用于后验模型合并系数的优化，避免SDE训练与ODE推理的不一致性。

Q4: 论文做了哪些实验？

在SiT-XL/2和EDM2模型上进行图像生成实验，使用FID-50K评估；进行了定性结果可视化（Figures 6-10）；还进行了后验模型合并系数的优化实验，与常见基线（如直接RL优化）对比。

Q5: 发现了什么实验现象？

方法显著降低FID：SiT从8.30降至5.77（提升约30%），EDM2从3.74降至3.52；定性结果显示视觉质量提升且无模式坍塌；后验合并优化优于直接RL优化，缓解了SDE/ODE不一致性。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：1) 将分布奖励扩展到其他生成任务（如视频、3D、文本生成）；2) 探索更高效的奖励计算策略（如基于学习的分辨器）；3) 结合更丰富的分布度量（如超越FID的感知指标）；4) 应用于更大规模的生成模型；5) 理论分析子集替换策略的偏差-方差权衡。

Q7: 总结一下论文的主要内容

本文针对视觉生成模型RL微调中样本级奖励导致的奖励破解和模式坍塌问题，提出分布级奖励框架。首先指出样本级奖励使得所有生成样本独立优化，容易陷入局部最优并丧失多样性。为此，作者设计分布级奖励，将生成样本的整体分布与真实分布对比（如FID）作为奖励信号，从而鼓励多样性。由于直接计算完整分布的奖励成本过高，引入子集替换策略：维护一个固定大小的参考集，每次迭代仅用新生成的样本替换其中一部分，再基于该子集计算分布奖励，大幅降低计算量。此外，针对常规RL中SDE训练与ODE推理的不一致性，作者利用RL优化后验模型合并系数，避免性能差距。实验在SiT和EDM2两个扩散模型上验证，FID-50K获得显著改善，定性结果也展示了更好的感知质量，同时未牺牲多样性。论文还展示了后验合并优化的有效性。该工作为生成模型的对齐提供了一种稳健有效的奖励设计思路。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与生成方向直接相关（用户画像中generation权重0.1）。

## 基本信息

- 作者：Ruihang Li, Mengde Xu, Shuyang Gu, Leigang Qu, Fuli Feng, Han Hu, Wenjie Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CV
- 日期：2026-07-03
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02291`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，并结合摘要和引言信息进行合理推断。
