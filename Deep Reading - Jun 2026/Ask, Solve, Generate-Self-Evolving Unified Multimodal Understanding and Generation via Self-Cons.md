---
user_id: "cheng tan"
paper_id: 1671
arxiv_id: "2606.27376"
title: "Ask, Solve, Generate: Self-Evolving Unified Multimodal Understanding and Generation via Self-Consistency Rewards"
publish_date: "2026-06-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.27376.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27376"
abs_url: "https://arxiv.org/abs/2606.27376"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-26T14:27:48"
---
# Ask, Solve, Generate: Self-Evolving Unified Multimodal Understanding and Generation via Self-Consistency Rewards

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：unified multimodal model · self-evolving training · self-consistency reward · visual understanding

## 一句话总结

提出一个自我进化的训练框架，使统一多模态大模型仅利用无标签图像即可自主提升视觉理解与图像生成能力，无需人工标注、偏好标签或外部奖励模型。

## 摘要

> Most existing unified large multimodal models (LMMs) that jointly perform visual understanding and image generation still rely heavily on curated supervision during post-training, typically requiring human annotations, preference labels, or external reward models. In this work, we ask: Can a unified LMM autonomously improve both capabilities using only unlabeled images? To answer this, we propose a self-evolving training framework built around three collaborating internal roles: a Proposer that generates visual questions, a Solver that answers and evaluates them, and a Generator that synthesizes images. Our training is driven by self-derived consistency signals, requiring no human annotations, preference labels, or task-trained external reward/judge model. To stabilize training, we introduce Solver Token Entropy (STE), a continuous difficulty signal derived from token-level prediction uncertainty that remains informative even when sample-level consistency collapses. For image generation, we further design a multi-scale internal evaluation scheme that combines question-answer fidelity scoring with cycle-consistent captioning. This creates a solver-mediated coupling: as visual understanding improves, the Solver provides more reliable generation-side assessment and stronger internal training signals. Our framework keeps the same role decomposition, reward logic, and training schedule across diverse unified model paradigms, including diffusion-based (BLIP3o), rectified-flow (BAGEL), and autoregressive (VARGPT-v1.1) architectures, requiring only each backbone's native prompting and generation interface. Across eight reported understanding metrics, our method consistently improves over the respective base unified models. In particular, when applied to BAGEL, our approach achieves an absolute gain of +3.5% on MMMU and improves image generation performance on GenEval from 82% to 85%. Code and models are publicly released.
> Github Code: https://github.com/mbzuai-oryx/Ask-Solve-Generate
> Project Page: https://mbzuai-oryx.github.io/Ask-Solve-Generate/
> Models: https://huggingface.co/collections/Ritesh-hf/ask-solve-generate-paper-models

Q1: 这篇论文试图解决什么问题？

统一多模态大模型（LMMs）虽然能同时进行视觉理解和图像生成，但后训练阶段通常需要大量人工标注、偏好标签或外部奖励模型，这限制了其规模化和自主进化能力。核心问题：能否设计一种训练框架，使统一LMM仅利用无标签图像数据，就能自主提升理解和生成能力，而无需任何外部监督信号？

Q2: 有哪些相关研究？

相关研究可分为两类：1）统一多模态模型：如BLIP3o（扩散）、BAGEL（整流流）、VARGPT（自回归），它们共享表示但后训练仍需监督。2）自我改进方法：如角色自我博弈、双重似然奖励、内部差距利用等，但仍依赖预定义奖励或外部反馈。本文区别于这些工作的是，完全依赖内部一致性信号，且适用于多种模型架构。

Q3: 论文如何解决这个问题？

提出一个自我进化训练框架，包含三个内部角色（LoRA适配器部署于冻结骨干网络）：1）提议者（Proposer）：基于无标签图像生成视觉问答对；2）求解者（Solver）：回答问题并评估答案质量，提供一致性信号；3）生成者（Generator）：根据图像生成文本描述或合成新图像。训练由自洽性奖励驱动，包括：对于理解任务，比较求解者在不同输入下的答案一致性；对于生成任务，结合问题-答案保真度评分和循环一致性描述。为稳定训练，引入求解者令牌熵（STE）作为连续难度信号。整个框架在同一角色分解、奖励逻辑和训练计划下适用于不同模型范式。

Q4: 论文做了哪些实验？

在三种统一模型上进行验证：BLIP3o-8B（扩散）、BAGEL-7B（整流流）和VARGPT-v1.1（自回归）。评估8项理解指标（如MMBench、TextVQA、MMMU等）和图像生成指标（GenEval）。实验包括：1）主实验对比基础模型与自我进化后模型的性能；2）消融研究：移除STE、移除多尺度评估等；3）分析训练过程中的稳定性。

Q5: 发现了什么实验现象？

1）三个骨干模型在理解指标上均取得一致提升，其中BAGEL在MMMU上绝对提升+3.5%，GenEval从82%提升至85%。2）STE有效防止训练崩溃，尤其在早期阶段。3）多尺度内部评估方案对生成质量改进至关重要。4）消融表明，移除任何角色或一致性信号都会导致性能下降。5）不同架构的增益幅度不同，但趋势一致。

Q6: 有什么可以进一步探索的点？

1）扩展到视频或3D等更多模态；2）探索更复杂的角色分解（如增加批评者角色）；3）结合少量人工标注进行半监督学习；4）将框架应用于更大规模模型和数据集；5）研究STE与其他难度度量（如梯度范数）的关系；6）分析生成样本的多样性和泛化能力；7）将自我进化应用于下游任务如视觉问答、图像编辑等。

Q7: 总结一下论文的主要内容

本文提出一个名为“Ask, Solve, Generate”的自我进化训练框架，旨在使统一多模态模型仅使用无标签图像即可提升视觉理解和图像生成能力。框架通过三个内部角色（提议者、求解者、生成者）协作，利用自洽性奖励信号驱动训练，无需任何外部监督。为稳定训练，引入求解者令牌熵（STE）。该方法在扩散、整流流和自回归三种主流统一模型上进行了验证，在多项理解和生成基准上取得一致提升。实验表明，该框架具有跨架构的通用性，为降低统一多模态模型对人工标注的依赖提供了新思路。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文与生成方向直接相关（权重0.10），提出自动化训练方法。

## 基本信息

- 作者：Ritesh Thawkar, Shravan Venkatraman, Omkar Thawakar, Abdelrahman Shaker, Fahad Khan, Hisham Cholakkal, Salman Khan, Rao Muhammad Anwer
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-06-26
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.27376`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 基于论文的PDF检索证据生成，主要参考了Abstract、Introduction、Conclusion和Results部分的内容。
