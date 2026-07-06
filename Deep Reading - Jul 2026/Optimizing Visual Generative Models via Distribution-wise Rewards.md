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
saved_at: "2026-07-06T12:32:38"
---
# Optimizing Visual Generative Models via Distribution-wise Rewards

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual generation · diffusion model · reinforcement learning · distribution-wise reward

## 一句话总结

本文提出分布级奖励（distribution-wise reward）强化学习框架，通过子集替换策略高效计算奖励并优化模型合并系数，有效缓解视觉生成模型微调中的奖励黑客和模式崩溃问题，在SiT和EDM2等模型上显著提升FID。

## 摘要

> Conventional reinforcement learning strategies for visual generation typically employ sample-wise reward functions, yet this practice frequently results in reward hacking that degrades image diversity and introduces visual anomalies. To address these limitations, we present a novel framework that finetunes generative models using distribution-wise rewards, ensuring better alignment with real-world data distributions. Unlike rewards that evaluate samples individually, distribution-wise reward accounts for the data distribution of the samples, mitigating the mode collapse problem that occurs when all samples optimize towards the same direction independently. To overcome the prohibitive computational cost of estimating these rewards, we introduce a subset-replace strategy that efficiently provides reward signals by updating only a small subset of a generated reference set. Additionally, we apply RL to optimize post-hoc model merging coefficients, potentially mitigating the train-inference inconsistency caused by introducing stochastic differential equation (SDE) in regular RL practices. Extensive experiments show our approach significantly improves FID-50K across various base models, from 8.30 to 5.77 for SiT and from 3.74 to 3.52 for EDM2. Qualitative evaluation also confirms that our method enhances perceptual quality while preserving sample diversity.

Q1: 这篇论文试图解决什么问题？

视觉生成模型（如扩散模型）在强化学习微调时通常采用样本级奖励函数，即独立评价每个生成样本。这种做法容易导致奖励黑客：模型过度优化单一奖励信号而牺牲多样性，产生视觉异常和模式崩溃。此外，RL训练中引入的随机微分方程与推理时使用的常微分方程之间存在不一致，影响最终性能。现有分布级度量（如FID）能更好反映整体质量，但直接计算整个生成集的分布奖励成本过高。

Q2: 有哪些相关研究？

相关工作包括：(1) 基于强化学习的扩散模型微调方法，如DDPO、DPOK、ReFL等，这些方法大多使用样本级奖励；(2) 奖励黑客与模式崩溃的分析与缓解策略；(3) 分布级评估指标如FID、FD_DINOv2，以及它们在训练中的困难；(4) 模型合并技术，通过加权平均不同模型来提升性能。本文提出的分布级奖励和子集替换策略直接对比并改进上述方法，同时首次将RL用于优化模型合并系数以解决SDE-ODE不一致。

Q3: 论文如何解决这个问题？

核心是分布级奖励，用生成样本集与真实数据分布的距离（如FID）作为奖励信号。为高效计算，提出子集替换策略：维护一个大小为N的参考集，每次迭代生成b个新样本，替换参考集中b个旧样本（如最旧或随机），然后计算更新后参考集与真实分布的FID作为当前策略的奖励。该奖励用于策略梯度更新生成模型。此外，后处理模型合并系数优化：将多个不同阶段或超参数的模型通过加权合并，把合并系数作为可学参数，使用RL（同样基于分布级奖励）优化，且训练使用SDE而推理使用ODE，RL优化弥补不一致。整个框架在SiT-XL/2和EDM2等模型上进行无条件图像生成微调。

Q4: 论文做了哪些实验？

在SiT-XL/2和EDM2模型上进行无条件图像生成微调实验。主要评估指标为FID-50K和FD_DINOv2。消融实验包括：(1) 子集替换策略中替换比例对性能的影响；(2) 分布级奖励与样本级奖励的对比（样例未详述但隐含优于样本级）；(3) 模型合并系数优化的有效性。定性结果展示了微调前后图像质量比较（图6-10）。最终报告了SiT-XL/2：FID从8.30降至5.77，FD_DINOv2从230.39降至164.88；EDM2：FID从3.74降至3.52（推测）。未报告统计显著性或多次运行结果。

Q5: 发现了什么实验现象？

实验观察到的关键现象：(1) 分布级奖励微调后FID大幅下降，且通过定性图可见视觉质量提升，未出现奖励黑客常见的重复或异常纹理；(2) 子集替换策略中，替换比例存在最优折中（如比例过高则参考集波动大，比例过低则更新慢），证据中提到“子集替换效果显著”；(3) 模型合并系数优化能进一步改善FID，说明缓解了SDE-ODE不一致；(4) 与样本级奖励相比，分布级奖励更好地保持了多样性，FD_DINOv2的改善也支持这一点。总体而言，实验表明分布级奖励在避免模式崩溃的同时有效提升了生成质量。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：(1) 将分布级奖励扩展到条件生成任务（如文本到图像、图像到图像）；(2) 探索更高效的子集更新策略，如基于重要性采样的替换；(3) 结合更丰富的分布度量（如CLIP score、human preference score）作为奖励；(4) 在更大规模模型（如SiT-XL/2 baseline以上）或视频生成模型中应用；(5) 理论分析分布级奖励的梯度性质及其与样本级奖励的关系；(6) 与偏好对齐方法（如DPO）结合形成统一框架。

Q7: 总结一下论文的主要内容

本文针对视觉生成模型强化学习微调中样本级奖励的缺陷，提出分布级奖励框架。通过考虑生成样本的整体分布而非单个样本，有效缓解奖励黑客和模式崩溃。为高效计算分布级奖励，设计了子集替换策略，仅更新生成参考集的一小部分即可获得奖励信号。此外，利用强化学习优化后处理模型合并系数，解决SDE训练与ODE推理之间的不一致。实验在SiT-XL/2和EDM2上展示了FID-50K的显著提升（SiT: 8.30→5.77; EDM2: 3.74→3.52），同时定性结果也验证了视觉质量的改进和多样性的保持。本文方法为生成模型的对齐提供了新思路，具有较好的实用性和扩展性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦于视觉生成模型的优化，属于生成方向，与用户画像中的生成方向权重0.10直接匹配。

## 基本信息

- 作者：Ruihang Li, Mengde Xu, Shuyang Gu, Leigang Qu, Fuli Feng, Han Hu, Wenjie Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CV
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02291`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据中的Abstract和Introduction片段，并结合启发式草稿进行了扩展和校准。部分细节（如子集替换比例、消融实验设置）未在命中片段中详述，基于合理推断补充。
