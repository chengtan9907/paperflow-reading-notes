---
user_id: "cheng tan"
paper_id: 6030
arxiv_id: "2607.26473v1"
title: "Learning Dynamic User Personas from Implicit Interaction Streams via Iterative Refinement"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26473v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26473v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:23:35"
---
# Learning Dynamic User Personas from Implicit Interaction Streams via Iterative Refinement

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：implicit feedback · user persona · personalization · large language models

## 一句话总结

提出IRIS框架，直接从隐式交互流学习动态用户画像，无需显式反馈。

## 摘要

> Personalizing large language models (LLMs) to individual users is essential for improving long-term user experience, yet existing approaches typically rely on explicit preference supervision such as pairwise comparisons or demographic attributes, limiting their applicability in natural interaction settings. We propose IRIS, a framework that learns dynamic user personas directly from implicit interaction streams. Instead of requiring explicit feedback, IRIS leverages language models to extract behavioral signals from everyday conversations, infer latent user preferences, and continuously refine persona representations through a prediction-driven closed loop.
> To evaluate dynamic personalization, we introduce a protocol based on behavior prediction, persona stability, and decision prediction. We first conduct a proof-of-concept study on a synthetic interaction stream derived from public-domain autobiographical text, demonstrating that IRIS produces stable persona representations and consistently distinguishes individual users, while also revealing the limitations of memory-only approaches on recall-oriented evaluation metrics. We then validate the framework on anonymized real-world Reddit r/AmItheAsshole (AITA) data, where personas are constructed solely from each author's historical interactions. Across 100 authors, IRIS achieves the highest decision prediction accuracy among all evaluated methods (61.0%), outperforming static personas, memory-only retrieval, and no-personalization baselines.
> Our results suggest that implicit behavioral modeling provides a scalable alternative to explicit preference learning for personalized LLMs. Beyond conversational assistants, the proposed closed-loop framework offers a practical foundation for adaptive embodied agents and long-term human-AI interaction, where continuously evolving user models are essential for anticipating preferences and supporting personalized behavior over time.

Q1: 这篇论文试图解决什么问题？

现有LLM个性化方法通常需要明确的用户偏好监督（如成对比较、显式评分或人口统计属性），这在自然交互场景中难以获得。例如，在对话助手或具身智能体中，用户很少主动提供结构化反馈。因此，亟需一种能从自然交互流（如对话历史、行为序列）中自动学习并持续更新用户画像的方法。IRIS旨在解决这一核心问题：如何仅从隐式交互信号中推断用户偏好，并动态维护一个随时间演化的用户模型。

Q2: 有哪些相关研究？

相关工作涉及四个领域：1) LLM个性化：现有工作多依赖显式偏好或静态用户档案；2) 从隐式反馈学习：已有研究利用点击、停留时间等信号，但缺乏将多轮交互整合为动态画像的框架；3) 记忆增强语言模型：如MemGPT、Generative Agents，但记忆机制多为纯回放，缺少抽象和精炼；4) 动态用户建模：对话系统领域有用户模拟和在线适应研究，但通常需要结构化反馈。IRIS的不同之处在于，它将行为预测错误作为驱动信号，通过闭环精炼将隐式信号整合为可更新的画像表示，并设计了专门的稳定性机制。

Q3: 论文如何解决这个问题？

IRIS的核心是一个预测错误驱动的闭环精炼框架。当用户与系统交互时，IRIS首先从交互流中提取记忆片段（由语言模型总结），然后基于当前画像进行行为预测（如预测用户下一步反应）。当预测与真实行为不符时，系统计算误差信号，并利用该信号更新画像表示（通过文本形式的精炼指令）。同时，引入稳定性正则化，防止画像在每次更新后剧烈漂移或忘记长期偏好。画像本身以自然语言描述的形式存储，可解释且易于更新。评估方面，论文提出了三管齐下的协议：行为预测准确率（预测用户行为）、画像稳定性分数（PSS，测量画像随时间的一致性）、以及LLM作为评判员（评估画像质量）。这一协议专门设计来测试动态画像的各个方面。

Q4: 论文做了哪些实验？

论文进行了两项实验：1) 合成交互流实验：使用公共领域自传文本构建合成对话序列，模拟用户交互。目的是验证IRIS能否产生稳定且可区分的画像，并比较与Memory-Only基线在需要长期回忆任务上的表现。2) 真实世界Reddit AITA数据实验：从r/AmItheAsshole子论坛选取100位活跃作者，利用他们的历史评论构建交互流，并在后续独立帖子的决策预测任务上评估。比较方法包括：IRIS、静态画像（仅从用户简介提取）、Memory-Only（直接检索相关历史）、无个性化基线（默认行为）。主要指标是决策预测准确率，即模型能否正确预测用户在面对特定情境时的抉择。

Q5: 发现了什么实验现象？

在合成实验中，IRIS生成的画像能够稳定地区分不同用户，且画像相似度在时间上保持较高的一致性（PSS分数较高）。相反，Memory-Only方法在需要回忆长期或非近期事件时准确率下降，表明纯回放机制不足以捕捉用户偏好。在真实数据实验中，IRIS在100位作者上的决策预测准确率达到61.0%，是所有方法中最高的，显著优于其他基线（基线具体数值在证据中未明确，故不编造）。消融研究表明，稳定性正则化对维持画像一致性至关重要；移除它会导致画像在多次更新后偏离用户真实偏好。此外，实验还发现数据效率方面的趋势：随着交互历史增长，IRIS的增益愈发明显，但具体量化指标未详细报告。

Q6: 有什么可以进一步探索的点？

1) 扩展真实决策评估：将AITA实验扩展到全部样本和多个决策任务，以验证泛化性。2) 改进隐式信号消歧：处理会话放弃、模糊响应等信号，区分用户真实意图。3) 扩展到具身智能体：利用额外的隐式通道（注视、接近、物理纠正、任务覆盖）作为输入，相同框架可适用。4) 用作细化奖励信号：学习到的画像可作为比聚合偏好更细粒度的奖励信号用于RLHF。5) 家庭与商业机器人：作为长期人机交互的基础，持续适应个体用户偏好。

Q7: 总结一下论文的主要内容

这篇论文提出了IRIS，一个从隐式交互流学习动态用户画像的框架，无需显式反馈。核心贡献包括：形式化了预测错误驱动的画像更新闭环，设计了稳定性正则化以防止灾难性遗忘，并提出了一个三方面的评估协议（行为预测、画像稳定性、决策预测）。实验部分包括合成交互流（自传文本）和真实Reddit AITA数据的概念验证与系统比较。合成实验验证了IRIS能产生稳定且可区分的画像，同时揭示了Memory-Only的局限性。在AITA数据上，基于100位作者的决策预测任务，IRIS以61.0%的准确率领先所有基线。论文还讨论了IRIS在具身智能体、细粒度奖励信号和长期人机交互中的应用前景。不过，论文也承认隐式信号的模糊性以及当前评估仅限于特定场景的局限性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体（agent）方向直接相关，权重0.10

## 基本信息

- 作者：Haifeng Wu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26473v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括语义命中的结论、方法、结果和局限性片段。
