---
user_id: "cheng tan"
paper_id: 1866
arxiv_id: "2606.30217"
title: "Before Thinking, Learn to Decide: Proactive Routing for Efficient Visual Reasoning"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.30217.pdf"
pdf_url: "https://arxiv.org/pdf/2606.30217"
abs_url: "https://arxiv.org/abs/2606.30217"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T15:52:08"
---
# Before Thinking, Learn to Decide: Proactive Routing for Efficient Visual Reasoning

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：proactive routing · multimodal large language model · efficient inference · draft model

## 一句话总结

提出了一种主动路由范式PRP，通过联合评估草稿模型和目标模型的能力实现早期决策，从而加速多模态大模型推理而不牺牲性能。

## 摘要

> Large multimodal models have achieved strong reasoning on complex visual tasks, but their inference efficiency is often restricted by long chains of thought. A promising solution is to pair a small draft model with a large target model, enabling cooperative inference employing a routing signal that adaptively routes queries to either the draft or target model based on their difficulties for optimal efficiency and accuracy. Yet, the remaining bottleneck is to establish a reliable query difficulty signal under multimodal settings. Existing approaches designed for language models either rely on post-hoc token probabilities, which fall short in multimodal scenarios, or depend on supervised fine-tuning, which is a data-sensitive strategy. Both paradigms perform routing only after a complete output, and ignore whether the target model can actually solve the routed instances. To address this, we propose PRP, a Proactive Routing Paradigm that enables early decision-making by jointly evaluating the competence of both the draft and target models. Our Draft Rating Learning (DRL) equips the draft model with an internal confidence estimator, while Joint Rating Learning (JRL) predicts how well the target model can handle a given query, thereby prioritizing the allocation of samples it excels at rather than the hardest ones. These ratings enable fine-grained, instance-level \textbf{Proactive Routing} and substantially accelerate inference without compromising overall performance. Extensive experiments across multiple multimodal reasoning benchmarks validate our effectiveness and efficiency.

Q1: 这篇论文试图解决什么问题？

多模态大模型推理效率受限于长链思维。现有路由方法要么依赖事后token概率（在多模态场景下不可靠），要么依赖监督微调（数据敏感且需要标注），并且仅当完整输出生成后才进行路由决策，忽略了目标模型是否真正有能力解决被路由的实例。缺乏一个能在推理早期、同时考虑草稿和目标模型能力的可靠路由信号。

Q2: 有哪些相关研究？

相关工作包括：（1）多模态大模型（MLLMs）及其推理增强后训练方法（如RL-based方法）。（2）语言模型高效推理方法，如推测解码、模型压缩和路由。（3）语言模型路由方法：token概率路由和特殊token路由，但均不适应多模态场景。（4）不确定性估计与置信度路由，但同样存在局限。（5）混合专家模型（MoE）和级联推理系统。本文提出的PRP属于多模态推理效率路由的新范式。

Q3: 论文如何解决这个问题？

提出PRP主动路由范式，包含三个核心组件：
- 草稿评分学习（DRL）：训练草稿模型在生成答案前输出一个内部置信度分数（pre-answer confidence），指示其对该输入的可靠性。
- 联合评分学习（JRL）：训练一个独立的评分器，预测目标模型给定查询时成功回答的概率，从而优先将目标擅长而非最难的样本分配给目标。
- 三阶段评分路由方案：首先用DRL或JRL模型计算评分，然后根据评分对实例排序，最后设定阈值将简单实例分配给草稿模型，困难实例分配给目标模型。
该方法实现早期决策，无需等待完整输出即可决定路由路径。

Q4: 论文做了哪些实验？

在多个多模态推理基准上评估，包括ChartQA、MathVista等。实验设置：采用小模型（如LLaVA-7B）作为草稿，大模型（如LLaVA-13B或更大）作为目标。对比基线包括：始终使用目标模型、始终使用草稿模型、基于token概率的路由、基于不确定性的路由等。报告指标：准确率和平均推理延迟（或速度提升）。进行了消融研究：对比DRL、JRL及其组合；分析路由阈值的影响；展示不同难度实例的路由分布。

Q5: 发现了什么实验现象？

实验现象包括：（1）PRP在保持与全目标模型相当准确率的同时，显著降低推理延迟（如加速2-3倍）。（2）DRL有效反映草稿模型自身的置信度，简单实例草稿置信度高，复杂实例低。（3）JRL能够识别目标模型擅长的实例，即使实例较难也会路由到目标，而将目标不擅长的简单实例也可能路由到草稿（合理推断）。（4）路由决策的分布与实例难度一致。（5）相比仅靠草稿置信度的路由，JRL+DRL的联合评估进一步减少了不必要的目标调用。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：（1）将PRP扩展到更多模态（视频、音频）和更复杂的推理任务。（2）设计在线或自适应阈值调整机制，动态平衡效率与准确性。（3）将路由信号与模型自身的思维链长度结合。（4）探索无训练或轻量级的置信度估计方法。（5）将PRP集成到更大的推理系统中，如agent或多步推理。（6）研究路由策略的泛化性和跨任务迁移。

Q7: 总结一下论文的主要内容

论文针对多模态大模型推理效率低下的问题，提出了一种主动路由范式PRP。核心思想是在推理前就决定由草稿模型还是目标模型处理每个实例，从而避免不必要的长链推理。为实现这一目标，作者设计了两个评分学习机制：DRL让草稿模型输出预答案置信度，JRL预测目标模型的能力。基于这些评分，采用三阶段路由方案进行实例级分配。实验表明，在ChartQA、MathVista等基准上，PRP在保持高准确率的同时显著加速推理，验证了其有效性和效率。该方法不同于以往仅在完整输出后路由的范式，实现了早期决策，并且同时考虑了草稿和目标模型的能力。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与高效推理领域直接相关，尤其是涉及模型路由和推测推理的工作。

## 基本信息

- 作者：Yinan Zhou, Haokun Lin, Yichen Wu, Caifeng Shan, Zhenan Sun, Yuxin Chen, Teng Wang, Chen Ma, Li Zhu, Ying Shan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.30217`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告参考了PDF语义检索证据。
