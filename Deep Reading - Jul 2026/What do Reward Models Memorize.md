---
user_id: "cheng tan"
paper_id: 5753
arxiv_id: "2607.24484v1"
title: "What do Reward Models Memorize?"
publish_date: "2026-07-27"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.24484v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.24484v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:56:41"
---
# What do Reward Models Memorize?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reward model · counterfactual memorization · rlhf · preference dataset

## 一句话总结

本文通过反事实记忆方法量化和分析了判别训练奖励模型在人类偏好数据上的记忆行为。

## 摘要

> This paper studies what discriminatively trained reward models (RMs) memorize by measuring counterfactual memorization on two human preference datasets. We show that RMs 1) misallocate memorization to easy, high margin preference pairs, 2) memorize dataset-specific shortcuts (e.g., model identity, user sampling strategy), and 3) overgeneralize simple heuristic correlates of human preference (e.g., length, compliance) when confronted with unseen preference pairs. Overall, our findings indicate that discriminative training of RMs from human preference data results in biased RMs not yet capable of judging response quality in context-dependent scenarios.
> https://github.com/ioverho/rm-shortcuts

Q1: 这篇论文试图解决什么问题？

当前奖励模型（RM）在RLHF中被用于拟合人类偏好，但经常表现出reward hacking和分布外泛化问题。先前研究已知RM可能依赖简单特征（如回复长度），但缺乏对RM到底记忆了什么内容的系统理解。本文旨在量化RM在训练过程中记忆哪些数据特征，特别是记忆资源是否被错误分配到不重要的模式，从而影响其泛化能力。

Q2: 有哪些相关研究？

本文涉及多个相关领域：1) 反事实记忆（Counterfactual Memorization）最初用于研究模型记忆训练样本的程度，本文将其应用于RM；2) 偏好数据集中的偏差（如长度偏好、模型身份偏好），先前工作已观察但未从记忆角度分析；3) RLHF中的reward hacking以及降低reward hacking的方法；4) 机器学习中的捷径学习（shortcut learning）。本文通过记忆视角统一分析这些现象。

Q3: 论文如何解决这个问题？

本文提出了一套量化RM记忆的方法框架：首先，使用标准LM→SFT→RM流程（backbone为Llama-3.2-1B）在两个人类偏好数据集（PRISM等）上训练判别型RM。然后，通过构造反事实样本（如交换偏好标签、扰动属性）并计算模型在反事实样本上的表现变化，定义记忆度量。同时，利用SHAP值分析不同特征（如回复长度、生成模型身份、用户评分等）对RM输出决策的贡献，从而识别RM过度依赖的特征。最终通过分类和归因分析揭示RM的记忆模式。

Q4: 论文做了哪些实验？

实验包括：1) 在PRISM和另一个数据集上训练RM，测量标准测试集和反事实测试集上的准确率；2) 按照偏好间隔（margin）将测试样本分组，观察不同间隔下反事实记忆程度；3) 使用SHAP计算各特征对RM决策的重要性，并分析高重要性特征的类型；4) 测试RM在未见过的偏好对（如来自不同模型对或评分者）上的泛化能力；5) 消融研究：移除高间隔样本或特定特征对RM性能的影响。实验代码已开源。

Q5: 发现了什么实验现象？

主要实验现象包括：1) 记忆错配（Misallocation）: RM在容易、高间隔的偏好对上表现出极高反事实记忆（即强烈依赖这些模式），而在困难、低间隔样本上记忆较少。这意味着RM将有限的记忆容量浪费在已经可区分的模式上，而非学习更具泛化性的偏好表示。2) 数据集伪影记忆：RM记忆了与具体数据集构建方式相关的特征。例如，它学会了偏好来自特定响应模型的答案（模型身份），并记住了用户评分者的采样策略（如评分者ID）。3) 启发式过度泛化：当面对训练中未出现的偏好对比时，RM倾向于依赖长度、顺从性等简单启发式特征，而不是上下文质量。例如，RM将更长的回答自动赋予更高奖励，即使它在训练数据中并不总是成立。这些现象共同表明RM存在系统性偏差。

Q6: 有什么可以进一步探索的点？

未来工作可以从以下几个方面展开：1) 设计减轻RM记忆错配的训练策略，例如通过重加权困难样本或引入对抗训练；2) 探索多维度或层级化的RM架构，以分解对启发式特征的依赖；3) 开发更好的反事实记忆度量，区分有害记忆与有用知识；4) 研究记忆模式在不同模型规模、不同数据集规模下的scaling趋势；5) 将记忆分析扩展到其他RLHF组件（如策略模型）或不同的偏好学习方法（如DPO）；6) 在实际RLHF部署中评估本文发现的记忆偏差对最终对齐效果的影响。

Q7: 总结一下论文的主要内容

本文首次系统性地研究了判别训练奖励模型（RM）在人类偏好数据上的反事实记忆问题。通过两个标准人类偏好数据集（PRISM等）和Llama-3.2-1B backbone，论文构建了完整的SFT→RM流程，并采用反事实记忆度量（通过交换偏好标签或扰动属性）和SHAP归因分析，从记忆分配、数据集伪影依赖、启发式特征泛化三个层面揭示了RM的记忆模式。主要发现包括：(1) RM将记忆容量错误集中于高间隔的“简单”偏好对，而非对泛化更有价值的困难样本；(2) RM记住了数据集中特定的伪影特征，如产生回复的大语言模型身份和用户评分者的采样策略；(3) 当面对分布外偏好对比时，RM无法基于上下文质量进行判断，转而过度依赖长度、顺从性等简单启发式相关特征，导致泛化失败。这些发现表明，当前从人类偏好数据判别训练出的RM尚未学会真正的判断能力，而是记忆了数据集的浅层模式和捷径。论文讨论了这些记忆偏差对RLHF可靠性、reward hacking风险以及偏好数据集设计的影响，并开源了实验代码。总体而言，该工作为理解RLHF中奖励模型的内在行为提供了重要视角，并对对齐研究中的数据集构造和模型训练实践具有警示意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于RLHF和对齐研究，本文提供理解reward hacking根源的视角

## 基本信息

- 作者：Ivo Verhoeven, Pushkar Mishra, Ekaterina Shutova
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-07-27
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.24484v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次精读报告基于PDF检索证据和论文元数据生成，部分内容参考了检索片段的语义信息。
