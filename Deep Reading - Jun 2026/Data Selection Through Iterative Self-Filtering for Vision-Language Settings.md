---
user_id: "cheng tan"
paper_id: 1117
arxiv_id: "2606.23611"
title: "Data Selection Through Iterative Self-Filtering for Vision-Language Settings"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23611.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23611"
abs_url: "https://arxiv.org/abs/2606.23611"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:22:13"
---
# Data Selection Through Iterative Self-Filtering for Vision-Language Settings

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：data selection · self-filtering · vision-language · CLIP

## 一句话总结

提出一种自举式自筛选方法，通过迭代训练和选择数据子集来改进CLIP模型训练，无需预训练模型或额外数据。

## 摘要

> The availability of large amounts of clean data is paramount to training neural networks. However, at large scales, manual oversight is impractical, resulting in sizeable datasets that can be very noisy. Attempts to mitigate this obstacle to producing performant vision-language models have so far involved heuristics, curated reference datasets, and using pre-trained models. Here we propose a novel, bootstrapped method in which a CLIP model is trained on an evolving, self-selected dataset. This evolving dataset constitutes a balance of filtered, highly probable clean samples as well as diverse samples from the entire distribution. Our proposed Self-Filtering method iterates between training the model and selecting a subsequently improved data mixture. Training on vision-language datasets filtered by the proposed approach improves downstream performance without the need for additional data or pre-trained models.

Q1: 这篇论文试图解决什么问题？

大规模视觉语言数据集（如LAION）通常包含大量噪声（文本-图像不匹配、低质量内容），严重影响下游模型性能。现有数据过滤方法要么依赖手工规则和启发式，要么需要精心策划的参考数据集，要么依赖预训练模型（如OpenAI CLIP）作为过滤器。这些方法都需要外部资源或人工干预，且无法随着数据集分布变化自适应。本文试图回答：能否仅通过训练过程本身，利用模型对自身数据的记忆和泛化特征，自举地逐步筛选出更干净、更有效的训练数据？

Q2: 有哪些相关研究？

1. **课程学习与数据选择**：模型先学习简单样本再记忆难样本（Arpit et al., 2017），因此可基于训练动态选择样本。MentorNet (Jiang et al., 2018) 是一种过滤模型，可根据样本损失进行权重分配。
2. **视觉语言数据过滤**：Xu et al. (2023) 指出平衡数据（包含多样性和高质量）可提升CLIP训练效果。其他工作使用预训练的CLIP模型作为过滤器（如OpenAI CLIP）来筛选文本-图像对。
3. **自举方法**：类似工作在小规模或传统模型中出现过（如广义线性模型中的自筛选），但本文首次将自举数据选择应用于大规模视觉语言模型（CLIP）的预训练场景。

Q3: 论文如何解决这个问题？

提出Self-Filtering方法：
1. **初始训练**：使用完整（含噪声）数据集训练一个CLIP模型。
2. **数据选择**：基于当前模型对每个样本的预测置信度（如对齐分数）或记忆化特征，选出高质量子集（高置信度样本）。同时保留一部分低置信度但多样性的样本，避免只保留简单样本导致表示塌缩。
3. **迭代**：用选出的混合数据重新训练模型，然后再次进行数据选择。重复若干轮，每次数据集逐渐变干净且更具代表性。
4. **最终训练**：使用最后一轮选出的固定数据集（或最后一轮模型）进行下游评估。
方法无需外部预训练模型或额外数据，完全基于训练自举。

Q4: 论文做了哪些实验？

实验设置信息不足（需原文补充），但从检索证据可知：
1. **主要对比**：将Self-Filtering选出的固定数据集与OpenAI的CLIP模型筛选的数据集进行对比，两者在下游任务上性能相似（Figure 4）。
2. **混合训练**：使用完整数据集+自选数据集的混合训练，结果优于仅用全数据集的baseline，且与使用OpenAI CLIP筛选的混合训练性能相当（Figure 5）。
3. **评估任务**：推测包括常见的视觉语言下游任务（如ImageNet zero-shot分类、检索等），但具体细节缺失。

Q5: 发现了什么实验现象？

1. **自筛选数据集的质量**：仅使用模型自身筛选的数据集训练，能达到与使用预训练CLIP（如OpenAI的ViT-L/14）所筛选数据集相当的下游性能，证明自举的有效性。
2. **混合训练优势**：将全数据集与自选数据集混合训练，比单纯使用全数据集表现更好，说明自选数据提供了更有效的训练信号。
3. **多样性保留**：自选数据集并非只保留高置信度样本，而是保留部分低置信度样本以维持多样性，避免过拟合到简单模式。
4. **迭代收敛**：随迭代轮次增加，所选数据集趋于稳定，性能提升有限。

Q6: 有什么可以进一步探索的点？

1. **更精细的选择策略**：当前基于置信度的选择可能不是最优，未来可探索基于损失、梯度或影响函数的选择标准。
2. **扩展到其他模态和架构**：Self-Filtering是否适用于扩散模型、语言模型或其他多模态模型？
3. **动态混合比例**：在迭代过程中如何自适应调整干净样本与多样样本的比例？
4. **理论分析**：理解自举选择过程如何保证收敛到干净数据集，提供更严格的收敛性证明。
5. **缓解计算开销**：迭代训练需要多次训练完整模型，训练成本高；研究更高效的自举策略（如只用子模型预测）。

Q7: 总结一下论文的主要内容

本文提出Self-Filtering，一种用于视觉语言设置的自举数据选择方法。核心思想是：无需使用预训练模型或外部数据，仅通过迭代训练CLIP模型并利用模型自身预测来逐步筛选出更干净、更具代表性的训练子集。该方法在每一轮中训练模型，然后基于置信度选择高概率干净样本，同时保留部分低置信度样本以维持多样性，形成改进的数据混合，用于下一轮训练。实验表明，使用Self-Filtering选出的固定数据集训练，其下游性能与使用OpenAI的预训练CLIP模型筛选的数据集训练的性能相当，且在混合训练中优于直接在完整噪声数据集上训练。这项工作证明了自举数据选择在大规模视觉语言预训练中的可行性，减少了对外部资源的依赖。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：针对视觉语言模型训练中的噪声数据问题，提出自举式过滤方案。

## 基本信息

- 作者：Andrei Liviu Nicolicioiu, Sarvjeet Singh Ghotra, Morgane M. Moss, Aaron Courville
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI, cs.LG
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23611`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（Abstract、Contributions、Introduction、5.1节等片段），并结合heuristic_draft进行润色和补全。
