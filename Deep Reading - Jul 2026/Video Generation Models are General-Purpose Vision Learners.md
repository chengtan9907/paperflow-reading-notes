---
user_id: "cheng tan"
paper_id: 3410
arxiv_id: "2607.09024"
title: "Video Generation Models are General-Purpose Vision Learners"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.09024.pdf"
pdf_url: "https://arxiv.org/pdf/2607.09024"
abs_url: "https://arxiv.org/abs/2607.09024"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-14T00:25:03"
---
# Video Generation Models are General-Purpose Vision Learners

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video generation · diffusion model · generalist vision model · perception

## 一句话总结

GenCeption 提出利用预训练的视频生成扩散模型作为通用的视觉表示学习器，通过前馈方式执行多种视觉任务，并展示了卓越的数据效率和泛化能力。

## 摘要

> Driven by next-token prediction, NLP shifted from task-specific models into powerful generalist foundation models. What, then, is the equivalent catalyst needed to achieve a general-purpose model in computer vision? In this paper, we contend that large-scale text-to-video generation serves as a strong pre-training paradigm for computer vision, providing the necessary spatiotemporal priors, vision-language alignment, and scalability required for general visual intelligence. We introduce GenCeption, which leverages a pre-trained video generative diffusion backbone to define a feed-forward perception model, capable of performing various vision tasks steered by text instructions. Empirical results demonstrate that GenCeption achieves state-of-the-art performance across a diverse suite of tasks, including depth, surface normal, and camera pose estimation, expression-referring segmentation, and 3D keypoint prediction, often matching or surpassing specialized models (e.g. DepthAnything3, SAM3, D4RT, VGGT-Omega, Sapiens, David, Genmo, and Lotus-2). Furthermore, the video generative pretrained backbone outperforms alternative pretraining paradigms (e.g., V-JEPA, and Video MAE) under comparable settings. Importantly, GenCeption exhibits preliminary data and model scaling properties along with exceptional data efficiency, where it achieves comparable performance with leading models like D4RT and VGGT-Omega with 7 to 500 less training data. Finally, GenCeption also exhibits intriguing emergent behaviors: a model trained exclusively on synthetic human videos generalizes to real-world footage and out-of-distribution object categories (e.g., animals and robots). These findings suggest that video generation is not merely a synthesis tool, but a foundational path toward generalist vision intelligence for the physical world. Project page: https://genception.github.io

Q1: 这篇论文试图解决什么问题？

当前计算机视觉领域仍以任务专用模型为主，缺乏类似NLP中next-token prediction的通用预训练范式。本文试图回答：如何利用已有的高质量视频生成模型作为通用视觉表示学习器，以统一方式处理多种视觉任务，从而推动通用视觉智能的发展。具体挑战包括：如何在不破坏生成预训练语义的前提下将其转化为感知模型，如何设计任务无关的统一架构，以及如何验证其数据效率和泛化能力。

Q2: 有哪些相关研究？

相关研究可分为以下几类：1. 视觉基础模型如DepthAnything、SAM、D4RT、VGGT-Omega等，针对特定任务设计，缺乏统一性。2. 视频预训练方法如Video MAE、V-JEPA，利用视频进行自监督学习，但未充分利用生成式目标。3. 生成式视觉模型如Genmo、Lotus-2探索了视频生成用于视觉理解，但未系统验证其作为通用主干的潜力。GenCeption 的关键区别在于：直接将预训练视频扩散模型作为通用特征提取器，通过保持生成优先的原则实现跨任务统一。

Q3: 论文如何解决这个问题？

GenCeption 基于三个设计原则：1. 多模态生成预训练作为表示学习：使用大规模文本-视频对预训练的扩散Transformer（DiT）作为骨干，保持VAE编码器、文本编码器和Transformer架构。2. 任务无关的后训练：通过文本提示指定输出模态，所有任务共享同一网络结构，输出编码为规范化的视频帧，方便添加新任务。3. 前馈重构：丢弃扩散迭代采样，将DiT隐层特征通过轻量输出头直接映射到输出潜在空间并解码，实现单步推理。微调时仅更新部分参数（如时序注意力层和输出头），以最大程度保留预训练知识。

Q4: 论文做了哪些实验？

实验覆盖四大类任务：密集预测（单目深度、表面法线、相机姿态、ray map）、密集语义预测（引用分割、人体语义解析）、稀疏预测（2D/3D关键点）以及生成验证。基准包括DepthAnything、SAM、D4RT、VGGT-Omega、Sapiens等。数据集包括MiDaS、NYUv2、ScanNet、Human3.6M、RefCOCO等。训练基于预训练视频扩散模型，在混合任务数据上微调。评估使用各任务标准指标。还进行对比预训练范式消融、数据规模缩放实验、以及从合成数据到真实世界的泛化实验。

Q5: 发现了什么实验现象？

1. 视频生成预训练显著优于其他视频预训练（V-JEPA、Video MAE），验证生成式目标的独特优势。2. 数据效率卓越：深度估计中仅用D4RT 1/500数据即达SOTA；姿态估计中用1/7数据超越VGGT-Omega。3. 涌现的合成到真实泛化：仅在合成人体视频上训练的模型可零样本泛化到真实场景和不同物体类别。4. 前馈推理稳定性高，避免扩散随机性。5. 缩放趋势提示更大规模模型仍有提升空间。6. 跨任务兼容性好，未出现明显负迁移。

Q6: 有什么可以进一步探索的点？

1. 扩展到更多任务（如3D重建、光流、目标检测）。2. 更大规模预训练与微调，探索scaling法则。3. 优化任务表示（如紧凑tokens）。4. 少样本与持续学习发挥数据效率优势。5. 生成与理解的融合。6. 深入分析涌现机制。7. 更高效的微调策略（如参数高效适配）。8. 下游应用如机器人视觉、自动驾驶、医疗成像。

Q7: 总结一下论文的主要内容

本文提出GenCeption，将预训练视频生成模型转化为通用视觉感知系统。动机源自NLP通用模型成功，作者认为视频生成提供了时空先验和多模态对齐，是通用视觉预训练的催化剂。方法基于三个原则：保留生成预训练、任务无关后训练、前馈推理。输入RGB视频和任务提示，经VAE编码、DiT处理、VAE解码直接输出任务结果。实验在8种以上任务中达到或超越SOTA，数据效率高（7-500倍数据节省），并展现合成到真实的泛化能力。意义在于提供了视频生成作为通用视觉基础的实践证据，暗示生成中蕴含理解。局限包括架构依赖、任务覆盖不足、微调策略简单、涌现机理尚未清晰。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文探索了利用视频生成实现通用视觉，与用户关注的生成方向高度相关。

## 基本信息

- 作者：Letian Wang, Chuhan Zhang, Rishabh Kabra, Jasper Uijlings, Steven Waslander, Andrew Zisserman, Joao Carreira, Kaiming He, Misha Andriluka, Eduard Gabriel Bazavan, Andrei Zanfir, Cristian Sminchisescu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.09024`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF语义检索证据（retrieved_evidence）及元数据Abstract，heuristic_draft提供了框架性信息但证据不足部分已结合检索进行补充和修订。
