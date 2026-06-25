---
user_id: "cheng tan"
paper_id: 1387
arxiv_id: "2606.25473v1"
title: "Causal-rCM: A Unified Teacher-Forcing and Self-Forcing Open Recipe for Autoregressive Diffusion Distillation in Streaming Video Generation and Interactive World Models"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25473v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25473v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T16:06:43"
---
# Causal-rCM: A Unified Teacher-Forcing and Self-Forcing Open Recipe for Autoregressive Diffusion Distillation in Streaming Video Generation and Interactive World Models

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：diffusion distillation · autoregressive video generation · consistency model · distribution matching distillation

## 一句话总结

本文提出了Causal-rCM，一个将rCM扩散蒸馏框架扩展到自回归视频扩散的统一开放配方，通过配对teacher-forcing一致性模型（TF-CM）和self-forcing分布匹配蒸馏（SF-DMD）实现了前沿的流式视频生成和交互世界模型性能。

## 摘要

> Autoregressive video diffusion with causal diffusion transformers has emerged as a major paradigm for real-time streaming video generation and action-conditioned interactive world models. In this work, we extend rCM, an advanced diffusion distillation framework, to autoregressive video diffusion. The core philosophy of rCM lies in the complementarity between forward and reverse divergences, represented by consistency models (CMs) and distribution matching distillation (DMD), respectively, in diffusion distillation. This philosophy naturally carries over to the autoregressive setting, where teacher-forcing (TF) provides an offline, forward-divergence causal training paradigm, while self-forcing (SF) corresponds to an on-policy, reverse-divergence refinement.
> Our contributions are: (1) through extensive experiments, we show that teacher-forcing CM is currently the best complement to self-forcing DMD as an initialization strategy (2) we present the first implementation of teacher-forcing-based continuous-time CMs (e.g., sCM/MeanFlow) for autoregressive video diffusion, enabled by our custom-mask FlashAttention-2 JVP kernel, achieving $10\times$ faster convergence compared to discrete-time CMs (dCMs) (3) we introduce Causal-rCM, a leading, unified, and scalable algorithm-infrastructure open recipe for diffusion distillation and causal training (4) we achieve state-of-the-art streaming video generation performance in both frame-wise and chunk-wise settings, using only synthetic data for training.
> Notably, our distilled 2-step causal Wan2.1-1.3B model achieves a VBench-T2V score of 84.63 with only 1 or 2 sampling steps. We further apply Causal-rCM to Cosmos 3, an advanced omnimodal world foundation model for physical AI with action-conditioned generation capability, enabling an interactive world model.
> VBench-T2V Performance (based on Wan2.1-1.3B)
> ![](images/246e6608767303e4c5a809711eca56a22e36f4031c4e8bf7f73176ff76b56589.jpg)
> Following common practice, we report VBench results using the official GPT-4o-augmented prompts.
> Figure 1: State-of-the-art performance of Causal-rCM for streaming video generation (1-step: 84.63). Causal-rCM achieves leading VBench-T2V scores across 1-step, 2-step, and 4-step generation, under both frame-wise and chunk-wise autoregressive regimes.

Q1: 这篇论文试图解决什么问题？

这篇论文旨在解决自回归视频扩散模型在推理时采样步数过多、计算开销大的问题，同时希望在保持生成质量的前提下实现实时流式视频生成和交互世界模型。具体来说，现有的自回归视频扩散模型（如基于因果扩散Transformer的模型）虽然能生成长视频，但通常需要数十到数百步的扩散采样，难以满足实时交互需求。扩散蒸馏技术（如一致性模型和分布匹配蒸馏）已被证明能大幅减少采样步数，但尚未被系统地应用于自回归视频生成。本文试图填补这一空白，探索如何将rCM框架（结合CM和DMD的互补思想）适配到自回归因果训练设置中，并处理teacher-forcing和self-forcing两种训练范式的协同与权衡。

Q2: 有哪些相关研究？

相关研究涵盖三大领域：(1)扩散蒸馏：包括一致性模型（CM，如Song et al.，2023）、分布匹配蒸馏（DMD，如Yin et al.，2024）以及两者的结合rCM（Zheng et al.，2025）。连续时间CM（sCM/MeanFlow）相比离散时间CM（dCM）有更快的收敛。本文首次将连续时间CM引入自回归视频扩散。(2)自回归视频扩散：以因果扩散Transformer为基础，如Wan2.1（Wan et al.，2025）、Cosmos（NVIDIA，2026）等，用于流式生成和世界模型。这些模型通常需要多步采样。(3)交互世界模型：视频扩散被视为世界模拟器（如Brooks et al.，2024；Bao et al.，2024），本文展示了蒸馏模型在动作条件生成中的应用。此外，还涉及教师强制和自回归训练的交叉应用（如Ranzato et al.，2016的曝光偏差问题），但本文将TF和SF对应于前向和反向散度差异，提供了新视角。

Q3: 论文如何解决这个问题？

本文的核心解决方案是Causal-rCM，它通过配对两种蒸馏目标与两种因果训练范式来扩展rCM到自回归设置：(1)Teacher-Forcing一致性模型（TF-CM）：利用连续时间CM目标（如sCM/MeanFlow）在教师强制（offline, forward-divergence）下训练，提供快速且稳定的初始蒸馏。(2)Self-Forcing分布匹配蒸馏（SF-DMD）：在自回归推理时，利用模型自身生成的轨迹（on-policy, reverse-divergence）进行DMD微调，弥补TF-CM的分布偏移。训练采用分阶段流水线：首先TF-CM预训练（使用连续时间CM和自定义FlashAttention-2 JVP内核实现高效反向传播），然后可选SF-DMD微调。对于帧级生成，使用因果掩码确保每个帧只依赖先前帧；对于块级生成，可并行处理块。算法-基础设施方面提供了开源实现，支持Wan2.1和Cosmos 3等基础模型。

Q4: 论文做了哪些实验？

论文进行了全面的实验以验证Causal-rCM的性能：(1)帧级文本到视频（T2V）生成：基于Wan2.1-1.3B模型，在VBench-T2V基准上评估，使用1步、2步等少量采样步数。报告了TF-CM和SF-DMD的消融结果。(2)块级视频预测：在UCF-101和Kinetics-600上评估帧预测质量（FVD、PSNR等），对比基线包括原始扩散模型、离散时间CM等。(3)训练效率比较：测量连续时间CM与离散时间CM的收敛速度（10倍加速）。(4)动作条件生成：将Causal-rCM应用于Cosmos 3，演示交互世界模型能力，输入动作序列生成对应的交互视频。(5)消融研究：TF-CM vs. SF-DMD贡献，蒸馏目标组合，rollout深度等。实验设置遵循常规，但所有训练仅使用合成数据（如CogVideoX等生成的数据集），这是亮点。

Q5: 发现了什么实验现象？

实验观察包括：(1)TF-CM作为初始化显著提升了SF-DMD的最终性能，单独使用SF-DMD难以收敛。(2)连续时间CM（sCM/MeanFlow）在自回归设置中比离散时间CM（dCM）收敛快约10倍，且质量更好。(3)2步采样（1步TF-CM + 1步SF-DMD）即可达到84.63 VBench分数，接近教师模型（约85分），而原始教师模型需50步。(4)块级视频预测中，Causal-rCM在FVD上超越基线，且与自回归rollout长度呈亚线性退化。(5)在动作条件生成中，模型能根据动作指令（如“左转”、“前进”）产生合理的物理交互，但长程一致性仍有漂移。(6)消融发现：TF-CM单独即可达到不错性能，但加上SF-DMD能减少模糊和伪影；完全联合训练（TF+SF同时优化）反而降低VBench上限，因此采用分阶段策略。负结果：帧级训练在长rollout深度（如>32帧）时不稳定，需要特殊处理。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：(1)解决帧级T2V训练在长rollout深度下的脆弱性，可能需要更稳定的训练技巧或正则化。(2)实现完全联合优化（类似rCM），以消除分阶段训练中的分布差距，可能通过更好的调度或交替优化。(3)扩展到更长的视频（超过100帧），当前实验主要针对64帧以内。(4)将Causal-rCM应用于更多基础模型和任务，如视频修复、帧插值。(5)探索teacher-forcing和self-forcing的混合策略，例如动态权重调整。(6)改进SF-DMD的on-policy采样效率，减少微调计算成本。(7)在真实世界数据上训练并验证泛化能力，当前仅使用合成数据。(8)将框架拓展到多模态条件生成（如文本+动作+音频）。(9)研究蒸馏模型在开放域交互中的鲁棒性和失败模式。

Q7: 总结一下论文的主要内容

本文针对自回归视频扩散模型推理成本高的问题，提出了Causal-rCM，将rCM的扩散蒸馏思想（前向散度CM与反向散度DMD互补）适配到因果训练范式（teacher-forcing和self-forcing）。技术贡献包括：(1)首次将连续时间一致性模型（sCM/MeanFlow）用于自回归视频扩散，并通过定制FlashAttention-2 JVP内核实现高效训练，收敛速度比离散时间CM快10倍。(2)设计分阶段训练流水线：先通过teacher-forcing CM（TF-CM）进行高效初始蒸馏，再利用self-forcing DMD（SF-DMD）进行on-policy微调，两者协同达到SOTA。(3)在流式视频生成中（帧级和块级）仅需1-2步采样，VBench-T2V达到84.63分，与50步教师模型相当。在动作条件交互世界模型Cosmos 3上展示了自然交互能力。实验揭示了TF-CM作为初始化关键、连续时间CM的优势以及联合训练的挑战。论文提供了开源算法和基础设施，为高效视频扩散蒸馏建立了新基准。局限性包括长rollout不稳定、完全联合优化困难以及仅合成数据训练。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文直接涉及生成任务（生成方向权重0.10），特别是视频生成和交互世界模型。

## 基本信息

- 作者：Kaiwen Zheng, Guande He, Min Zhao, Jintao Zhang, Huayu Chen, Jianfei Chen, Chen-Hsuan Lin, Ming-Yu Liu, Jun Zhu, Qianli Ma
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.LG
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25473v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据（abstract、introduction、结果、limitations等片段），结合heuristic_draft进行组织和补充，所有关键论断均有证据支持。
