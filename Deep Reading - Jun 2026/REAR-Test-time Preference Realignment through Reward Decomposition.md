---
user_id: "cheng tan"
paper_id: 1871
arxiv_id: "2606.30339"
title: "REAR: Test-time Preference Realignment through Reward Decomposition"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.30339.pdf"
pdf_url: "https://arxiv.org/pdf/2606.30339"
abs_url: "https://arxiv.org/abs/2606.30339"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T15:57:35"
---
# REAR: Test-time Preference Realignment through Reward Decomposition

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：test-time scaling · preference alignment · reward decomposition · large language models

## 一句话总结

提出 REAR 框架，通过将奖励分解为问题相关和偏好相关两部分，实现在测试时无需训练即可对大型语言模型进行偏好对齐，并兼容 best-of-N 采样和树搜索等测试时扩展算法。

## 摘要

> Aligning large language models (LLMs) with diverse user preferences is a critical yet challenging task. While post-training methods can adapt models to specific needs, they often require costly data curation and additional training. Test-time scaling (TTS) presents an efficient, training-free alternative, but its application has been largely limited to verifiable domains like mathematics and coding, where response correctness is easily judged. To extend TTS to preference alignment, we introduce a novel framework that models the task as a realignment problem, since the base model often fails to sufficiently align with the stated preference. Our key insight is to decompose the underlying reward function into two components: one related to the question and the other to preference information. This allows us to derive a REAlignment Reward (REAR) that selectively rescales the proportions of these two reward terms. We then show that REAR can be formulated as a linear combination of token-level policy log-probabilities, making it computationally efficient and easy to integrate with various TTS algorithms such as best-of-$N$ sampling and tree search. Experiments show that compared to other test-time baselines, REAR not only enables scalable test-time realignment for preference alignment tasks under diverse user requirements, but also generalizes to mathematical and visual tasks under appropriate preference settings.

Q1: 这篇论文试图解决什么问题？

大型语言模型（LLM）与多样用户偏好的对齐是一个关键挑战。后训练方法（如 RLHF）需要昂贵的数据收集和额外训练。测试时扩展（TTS）是一种高效的免训练替代方案，但此前主要局限于数学和编程等可验证领域，其中响应正确性易于判断。本文试图将 TTS 扩展到偏好对齐任务，该任务中响应质量是整体的，难以简化为简单可验证答案。具体问题是如何有效引导测试时算法为复杂偏好对齐任务评分并选择更优响应。

Q2: 有哪些相关研究？

相关工作包括：（1）后训练偏好对齐方法，如 RLHF、DPO 及其变体，需要额外训练。（2）测试时扩展（TTS）方法，如最佳-N 采样、树搜索，在推理阶段提升性能，但主要应用于可验证任务。（3）奖励建模，学习奖励函数以对齐偏好。（4）测试时偏好对齐的前期尝试，如通过提示工程或重排序。本文区别于上述工作，提出无需训练的测试时偏好对齐框架，并通过奖励分解实现计算高效的重新对齐。

Q3: 论文如何解决这个问题？

本文将测试时偏好对齐建模为重新对齐问题：基础模型在生成时未能充分对齐所陈述的偏好。关键洞察是将潜在奖励函数分解为两部分：与问题相关的奖励和与偏好信息相关的奖励。由此推导出重新对齐奖励（REAR），它选择性地重新调整这两部分奖励的比例。REAR 可表示为 token 级策略对数概率的线性组合，因此计算高效，易于集成到各种 TTS 算法中，如 best-of-N 采样和树搜索。具体地，开发了两种可扩展的测试时算法：REAR 指导的 best-of-N 采样和 DVTS（推测为树搜索变体），可在不同采样预算下提升生成性能。

Q4: 论文做了哪些实验？

论文在偏好对齐任务上评估 REAR 指导的测试时扩展效果。实验设置包括：（1）对比基线：与贪婪解码、无指导的 best-of-N 等测试时方法比较。（2）数据集：使用标准偏好对齐基准。（3）评测指标：对齐评分或偏好符合度。此外，还通过指定适当偏好文本，将方法泛化到数学（MATH500）和视觉任务。在 MATH500 上进行了偏好文本敏感性分析，比较三种不同相关程度的偏好文本（详细相关、一般相关、无关），并与贪婪解码（准确率 74.6）对比。

Q5: 发现了什么实验现象？

实验观察到：（1）REAR 指导的测试时扩展在偏好对齐任务上显著优于无指导的基线，且随采样预算增加性能持续提升。（2）在 MATH500 上，相关偏好文本（详细 description）显著提升准确率（例如 78.2），而无关偏好文本（74.8）与贪婪解码（74.6）在统计上无差异，表明 REAR 不会盲目提升任意文本的分数，而是奖励匹配偏好文本的生成轨迹。（3）方法泛化到数学和视觉任务，表明通过指定适当偏好文本即可适配不同场景。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：（1）自动生成更高质量和多样性的偏好文本，减少人工设计。（2）将 REAR 应用于更复杂的多步骤任务，如对话和 agent 推理。（3）探索与其他测试时技术（如思考链、自我批评）的结合。（4）研究偏好文本的表示学习，提高泛化性。（5）将框架扩展到多偏好融合场景。（6）在更大模型和更广泛任务（如科学推理）上验证可扩展性。

Q7: 总结一下论文的主要内容

本文提出 REAR，一种测试时偏好重新对齐框架，通过奖励分解实现免训练、计算高效的偏好对齐。论文首先指出测试时扩展在可验证任务上的成功及其在偏好对齐任务上应用的挑战，然后形式化测试时偏好对齐为重新对齐问题，并将奖励分解为问题相关和偏好相关两部分。推导出 REAR 作为 token 级策略对数概率的线性组合，从而可无缝集成到 best-of-N 采样和树搜索等 TTS 算法中。实验证明 REAR 在偏好对齐任务上优于其他测试时基线，且通过调节偏好文本可泛化到数学和视觉任务。此外，敏感性分析表明 REAR 对偏好文本的选择敏感，但不会对无关文本产生虚假提升。本文的主要贡献包括：（1）提出测试时偏好对齐的重新对齐形式化。（2）推导出计算高效的 REAR 奖励。（3）开发两种可扩展测试时算法。（4）实验验证有效性和泛化性。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文提出的测试时偏好对齐方法可直接应用于需要动态调整偏好的智能体系统。

## 基本信息

- 作者：Fuxiang Zhang, Pengcheng Wang, Chenran Li, Yi-Chen Li, Yuxin Chen, Lang Feng, Chenfeng Xu, Masayoshi Tomizuka, Bo An
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.30339`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 检索证据片段，包括摘要、引言、方法和实验部分。
