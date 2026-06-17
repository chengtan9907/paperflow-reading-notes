---
user_id: "cheng tan"
paper_id: 114
arxiv_id: "2606.13598v1"
title: "Reward Modeling for Multi-Agent Orchestration"
publish_date: "2026-06-11"
pdf_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/arXiv - Jun 2026/2606.13598v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.13598v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
report_version: "2026-06-04-v5"
daily_note_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-14T23:55:58"
---
[[Daily Note - Jun 2026]]

- PDF：[https://arxiv.org/pdf/2606.13598v1](https://arxiv.org/pdf/2606.13598v1)

# Reward Modeling for Multi-Agent Orchestration

- PDF: https://arxiv.org/pdf/2606.13598v1
- 原文: https://arxiv.org/abs/2606.13598v1

> ★★☆☆☆ 按需阅读 · 约 20 分钟 · 模型 openai-compatible/gemini-3-flash-preview · 证据 PDF 全文 + 元数据

🏷 关键词：multi-agent systems · reward modeling · orchestration · test-time scaling

## 一句话总结

本研究提出 OrchRM，一种无需人工标注的自监督编排奖励模型框架，通过在编排层面构建胜负对，显著提升了多智能体系统（MAS）的训练效率与推理性能。

## 摘要

> Multi-Agent Systems (MAS) built on Large Language Models (LLMs) require effective orchestration to coordinate specialized agents, yet training such orchestrators is hindered by limited supervision and high computational cost. We propose Orchestration Reward Modeling (OrchRM), a self-supervised framework for evaluating orchestration quality without human annotations. OrchRM leverages intermediate artifacts from multi-agent executions to construct win-lose pairs for Bradley-Terry reward model training. Unlike existing MAS test-time scaling and orchestrator training frameworks that rely on costly sub-agent rollouts, OrchRM operates directly at the orchestration level, enabling efficient and high-performing reward-guided orchestrator training and MAS test-time scaling. OrchRM improves training efficiency by up to 10x in token usage while improving MAS test-time scaling performance by up to 8% in accuracy. These gains consistently transfer across multiple domains, including mathematical reasoning, web-based question answering, and multi-hop reasoning, demonstrating orchestration-level reward modeling as a scalable direction for robust multi-agent orchestration. Code will be available at https://github.com/Wang-ML-Lab/OrchRM.

Q1: 这篇论文试图解决什么问题？

该论文试图解决多智能体系统（MAS）中“编排器”（Orchestrator）难以高效训练和评估的问题。目前 MAS 编排面临三大挑战：首先，编排策略、任务类型与子智能体能力之间存在强耦合，导致高质量的人工标注极难获取；其次，多智能体工作流极其复杂，全量执行子智能体任务会产生巨大的计算开销和时间成本（例如 MAS-Orchestra 框架在 100 步训练中消耗超过 10 亿 Token）；最后，现有的测试时缩放方法通常需要对完整执行路径进行验证，这在多智能体场景下效率极低。论文的核心目标是寻找一种能够在“编排层面”而非“执行层面”进行评估的奖励信号，以实现低成本、高效率的编排优化。

Q2: 有哪些相关研究？

相关研究主要集中在三个方向：1. **多智能体系统（MAS）**：从手动设计工作流转向由 LLM 驱动的自动化编排（如 MAS-Orchestra、DyLAN 等），这些系统通过动态规划和智能体交互解决复杂任务。2. **奖励建模（Reward Modeling）**：在 RLHF 框架下，通过人类偏好训练奖励模型（RM）来对齐 LLM，但在 MAS 编排领域的应用尚处于起步阶段。3. **测试时缩放（Test-time Scaling）**：如 Best-of-N 采样和自我验证机制，旨在通过推理时的计算换取性能提升，但现有方法在 MAS 中往往需要昂贵的子智能体全量回测。

Q3: 论文如何解决这个问题？

OrchRM 框架的核心在于将奖励建模从“最终答案”前移至“编排计划”层面。具体步骤包括：
1. **自监督数据构建**：利用 MAS 执行过程中产生的中间产物（如编排计划、子智能体响应、最终答案），根据最终答案的正确性自动标注胜负对。如果一个编排计划导向了正确答案而另一个没有，则构成一对训练数据。
2. **奖励模型训练**：基于 Bradley-Terry 模型，在构建的偏好对上微调预训练的奖励模型（如 Skywork-Reward-LLaMA-3.1-8B），使其能够直接对编排计划（Plan）打分。
3. **测试时缩放应用**：在推理阶段，编排器生成多个候选计划，OrchRM 对这些计划进行评分，并选择得分最高的计划进行后续的子智能体调用，避免了对所有候选计划进行全量执行。
4. **编排器训练应用**：将 OrchRM 作为奖励信号，通过强化学习（如 PPO 或 GRPO）或拒绝采样微调来优化编排器策略，显著降低了对子智能体反馈的依赖。

Q4: 论文做了哪些实验？

论文在多个具有挑战性的基准测试上进行了实验：
1. **数据集**：包括 AIME 24&25（数学推理）、BrowseComp（Web 问答）、HotpotQA（多步推理）以及用于 OOD 测试的 GPQA（科学推理）。
2. **模型配置**：使用 Qwen2.5-7B-Instruct 作为编排策略模型，GPT-OSS-120B 作为子智能体骨干模型，奖励模型初始化自 Skywork-Reward-8B。
3. **测试时缩放实验**：对比了 Best-of-N (BoN) 采样下，OrchRM 与 LLM-as-a-judge、多数投票（Majority Voting）的性能。结果显示 OrchRM 在 N=8 时准确率提升高达 8%。
4. **训练效率实验**：在编排器微调中，OrchRM 相比于需要全量回测的基线方法，节省了约 90% 的 Token 消耗（10x 效率提升），且性能更优。
5. **消融实验**：研究了不同数据混合比例（如 3:1 的偏好对构建）对奖励模型准确性的影响。

Q5: 有什么可以进一步探索的点？

未来的研究方向包括：
1. **跨领域泛化性**：目前 OrchRM 在特定领域表现优异，如何构建更大规模、更多样化的通用编排数据集以提升其在完全未知任务上的表现仍是挑战。
2. **多步编排优化**：当前的 OrchRM 主要针对单次规划的编排，未来可以探索在多轮交互、动态修正的复杂编排流中进行实时奖励建模。
3. **端到端联合训练**：探索编排器与奖励模型的协同进化，通过在线学习不断优化奖励信号的准确度。

Q6: 总结一下论文的主要内容

本文提出了 OrchRM，一种针对多智能体系统（MAS）编排优化的奖励建模框架。针对 MAS 训练成本高、标注难的痛点，OrchRM 创新性地提出在“编排计划”层面进行自监督学习。它通过分析 MAS 执行轨迹的中间产物，自动构建偏好对来训练奖励模型。该模型不仅能作为推理时的过滤器（Test-time Scaling），在不增加子智能体调用成本的前提下提升系统准确率，还能作为高效的强化学习奖励源，将编排器的训练效率提升 10 倍。实验证明，OrchRM 在数学、Web 搜索和多步推理等多个领域均表现出极强的鲁棒性和迁移能力，为构建高效、可扩展的自动化多智能体系统提供了新的技术路径。

## PDF 证据定位

- 研究背景：
  Introduction | score=1.168 | the orchestration level (Sec. 4.2 and 4.3). We evaluate our approach across three domains, including mathematical reasoning, web-based question answering, and multi-hop…
  Introduction | score=1.157 | t learning objective, our goal is to investigate the feasibility of leveraging reward models to improve both training and test-time performance in orchestration-based MAS…
  Introduction | score=1.156 | reusable supervision for automated multi-agent orchestration. Our main contributions are: • We developed Orch-RM, a reward modeling framework for training LLM-based…
- 核心方法：
  Method | score=1.150 | on combines the diversity of multiple samples with the discriminative power of the reward model, often yielding more robust performance than both majority voting and standard…
  Method | score=1.149 | sive and poor Expensive 𝑦' 𝑦' Orch RM MAS Orchestration Training 𝑦) ... 𝑦) 𝑦* Scalar head Orchestrator 𝑦* 𝑦+ 𝑟! 𝑟" 𝑟#$%& 𝑧' 𝑧) ... 𝑧("' 𝑧( 𝑎'! ... 𝑎(" 𝑟! ≻𝑟" ≻𝑟#$%& 𝑟' 𝑟) ...…
  Method | score=1.139 | h our trained orchestration reward model rϕ: bz = arg maxz∈Z {rϕ(x, z)} . (7) The selected best orchestration is then used for the full MAS trajectory generation Y ∼πΘ(· | x…
- 主要结果：
  Results | score=0.848 | .2-3B, V2-Qwen3-4B, and V2-Qwen3-8B) [12]; (iii) without verification as a lower bound (LB), and an oracle reward model that always selects the best answer as an upper bound…
- 局限性：
  Conclusion | score=1.195 | In this work, we propose Orch-RM, an orchestration reward modeling framework for multi-agent LLM systems. By learning orchestration-level reward signals from accessible training…
  Conclusion | score=1.043 | e diversity and quality of available orchestrator models and sampled trajectories. Constructing largerscale and more diverse orchestration datasets remains an important…
- 相关性：
  Method | score=1.207 | entirely from accessible rollout artifacts without additional annotation. We combine these two sources of pairwise data using a fixed mixing ratio (3:1) to train the reward…
  Method | score=1.197 | = (x, z, Y , a). Each trajectory is assigned an outcome label based on the correctness of a, which is later used to construct reward model training data. Motivation and Overview.…
  Introduction | score=1.190 | zed domains [9, 16, 32], developing a capable LLM-based orchestrator has become increasingly important, yet remains highly non-trivial. This difficulty stems from several…

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该论文直接切中智能体（Agent）编排的效率瓶颈，与用户关注的智能体方向高度契合。

## 基本信息

- 作者：King Yeung Tsang, Zihao Zhao, Vishal Venkataramani, Haizhou Shi, Zixuan Ke, Semih Yavuz, Shafiq Joty, Hao Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG, cs.MA
- 日期：2026-06-11
- 推荐级别：**按需阅读**
- 预计阅读时间：约 20 分钟
- 解析来源：PDF 全文 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.13598v1`

## 代码与资源

- 代码：暂未发现公开链接
- 数据：暂未发现公开链接
- 项目主页：暂未发现公开链接
- 原文 PDF：https://arxiv.org/pdf/2606.13598v1
- arXiv：https://arxiv.org/abs/2606.13598v1
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于 OrchRM 的数据构建逻辑、实验数值（10x 效率、8% 提升）以及具体的基准测试集信息。
- 方法证据锚点：Method | score=1.150 | on combines the diversity of multiple samples with the discriminative power of the reward model, often yielding more robust performance than both majority voting and standard…
- 结果证据锚点：Results | score=0.848 | .2-3B, V2-Qwen3-4B, and V2-Qwen3-8B) [12]; (iii) without verification as a lower bound (LB), and an oracle reward model that always selects the best answer as an upper bound…
