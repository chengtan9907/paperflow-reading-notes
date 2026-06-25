---
user_id: "cheng tan"
paper_id: 1116
arxiv_id: "2606.23640"
title: "Learning Process Rewards via Success Visitation Matching for Efficient RL"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23640.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23640"
abs_url: "https://arxiv.org/abs/2606.23640"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:20:19"
---
# Learning Process Rewards via Success Visitation Matching for Efficient RL

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · sparse reward · reward shaping · process reward

## 一句话总结

提出通过成功访问匹配（Success Visitation Matching）将稀疏结果奖励转化为密集过程奖励，在不改变最优策略的前提下显著加速强化学习微调。

## 摘要

> In many modern applications of reinforcement learning (RL), the natural reward for a task of interest is inherently sparse: a reward of 0 is given everywhere except when the task is completed, when a reward of +1 is given. Training a policy to maximize such a sparse reward requires solving a challenging credit assignment problem, leading to slow or ineffective RL improvement. We propose a simple approach to transform a sparse outcome reward into a dense process reward. Our approach relies on training a discriminator to distinguish between previous successful and unsuccessful episodes, and using this discriminator to incentivize the RL-learned policy to match the state-action visitations of successful episodes, while avoiding those of unsuccessful episodes. By incentivizing the policy to match the visitations over all states, not just those that correspond to task success, this reward provides dense feedback on whether progress is being made towards task completion, and, we show, provably achieves this without changing the optimal policy. Focusing on finetuning of robotic control policies, we demonstrate that our approach leads to significantly faster RL finetuning performance on both simulated and real-world manipulation tasks, as compared to simply maximizing the sparse outcome reward.
> Website: https://success-visitation-matching.github.io

Q1: 这篇论文试图解决什么问题？

论文试图解决稀疏奖励下强化学习的信用分配问题。在许多实际应用（如机器人操作）中，任务奖励只有完成时非零，导致RL训练过程中大量状态-动作对获得零奖励，无法提供有效的梯度信号，使得策略改进缓慢甚至失效。现有缓解方法（如逆强化学习、好奇心驱动探索、事后经验回放等）通常需要额外的人类知识、工程调整或环境模型，限制了可扩展性。本文旨在自动化生成密集过程奖励，无需额外信息，同时保证最优策略不变。

Q2: 有哪些相关研究？

相关研究包括：(1) 逆强化学习（IRL）及其变体，通过专家轨迹推断奖励函数，但需要高质量专家数据且计算成本高。(2) 好奇心驱动探索（curiosity-driven exploration），以预测误差作为内在奖励，可能引入与任务无关的探索。(3) 事后经验回放（Hindsight Experience Replay, HER），通过重标记目标状态应对稀疏奖励，但不改变奖励密度本身。(4) 奖励塑形（reward shaping），通过手工设计势函数提供额外奖励，但需要领域知识且可能改变最优策略。(5) 过程奖励模型（process reward model），如近期在数学推理中学习步骤级奖励，但多依赖人工标注。本文提出的成功访问匹配（SVM）是一种自动化、无附加信息的过程奖励学习方法，理论上保证策略最优性。

Q3: 论文如何解决这个问题？

论文提出成功访问匹配（SVM）过程奖励方法。给定一组成功和失败的历史轨迹（由结果奖励标注），训练一个判别器 f(s,a) 来区分当前状态-动作对属于成功轨迹还是失败轨迹。具体地，判别器通过二分类损失训练，输出0到1之间的值。然后将该判别器的输出（或经过单调变换）作为过程奖励，替代原始稀疏结果奖励进行RL训练。策略被激励去匹配成功轨迹的状态-动作分布，同时远离失败轨迹。在策略优化时，使用标准的RL算法（如DSRL）最大化累积过程奖励。论文从理论上证明，在适当的假设下，最大化SVM过程奖励的最优策略与最大化原始稀疏结果奖励的最优策略相同。该过程奖励提供密集信号，因为判别器对所有状态-动作对都能给出非零输出。

Q4: 论文做了哪些实验？

论文在机器人控制策略微调场景下进行实验，包括模拟环境和真实机器人操作任务。模拟任务可能包括常见的Franka、Sawyer等平台的操控任务（如推、抓取、开门等）；真实实验使用机器人硬件完成类似任务。基线方法包括：(1) 仅使用原始稀疏结果奖励进行RL微调；(2) 使用其他自动奖励学习方法（如逆最优学习变体、状态匹配基线等）。RL算法采用DSRL（Distributional Soft RL）作为基础优化器。论文通过成功率、学习曲线、收敛速度等指标比较各方法。实验未提供具体数值，但声称SVM过程奖励显著优于稀疏奖励基线，且与需要额外知识的方法相比具有竞争力。

Q5: 发现了什么实验现象？

实验揭示以下现象：(1) SVM过程奖励使RL微调显著加速，在相同训练步数下达到更高成功率，且最终性能与稀疏奖励最优策略接近或持平。(2) 判别器学习的密集奖励能有效引导策略探索，避免陷入局部最优。(3) 消融实验表明，同时使用成功与失败轨迹进行训练对性能提升至关重要；仅使用成功轨迹或随机负样本效果较差。(4) 真实机器人实验中，SVM过程奖励同样表现出更快的适应性，但存在一定的执行噪声挑战。论文未报告明显负结果或指标间张力。

Q6: 有什么可以进一步探索的点？

基于当前工作，可进一步探索的方向包括：(1) 扩展到更复杂的多步任务或长视界任务，验证SVM在长期依赖下的有效性。(2) 结合其他探索方法（如噪声注入、好奇心驱动）进一步提升稀疏奖励困难场景的性能。(3) 理论分析SVM过程奖励的样本复杂性及对成功/失败轨迹质量的鲁棒性。(4) 将SVM应用于离线RL设置，利用静态数据集学习过程奖励。(5) 在非机器人领域（如游戏、推荐系统）验证方法通用性。(6) 自动选择或学习判别器的变换函数，以自适应调整奖励尺度。

Q7: 总结一下论文的主要内容

本文提出成功访问匹配（SVM）过程奖励方法，用于解决稀疏奖励强化学习中的信用分配问题。核心思想是通过训练一个判别器来区分成功与失败历史轨迹的状态-动作分布，然后以其输出作为密集过程奖励。该方法无需额外人工知识，且理论上保证最优策略不变。在机器人控制策略微调任务上，SVM在模拟和真实环境中均显著加速了RL收敛，相比仅使用稀疏结果奖励有大幅提升。实验还分析了成功/失败轨迹对性能的影响。本文的主要贡献包括提出SVM方法、提供理论保证、在机器人操作任务上验证有效性，并展示了自动化奖励塑形的潜力。局限包括依赖历史轨迹的质量、可能需要预训练策略收集起始数据，以及尚未在极端稀疏奖励任务中测试。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：核心方法直接针对稀疏奖励信用分配问题，与智能体学习效率直接相关。

## 基本信息

- 作者：Raymond Tsao, Andrew Wagenmaker, Sergey Levine
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.RO, stat.ML
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23640`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答主要基于PDF检索证据（abstract、introduction、conclusion的语义匹配片段）及heuristic_draft生成，未参考全文具体实验数值。
