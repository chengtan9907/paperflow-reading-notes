---
user_id: "cheng tan"
paper_id: 5465
arxiv_id: "2607.21419"
title: "PATS: Policy-Aware Training Scaffolding for Agentic Reinforcement Learning"
publish_date: "2026-07-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.21419.pdf"
pdf_url: "https://arxiv.org/pdf/2607.21419"
abs_url: "https://arxiv.org/abs/2607.21419"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-27T10:52:48"
---
# PATS: Policy-Aware Training Scaffolding for Agentic Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · LLM agent · training scaffold · adaptive context

## 一句话总结

PATS是一种策略感知训练支架框架，通过将rollout组转化为证据卡片并自适应调整上下文，在长期智能体强化学习中提升策略性能并减少提示token使用。

## 摘要

> In long-horizon LLM agent reinforcement learning, weak policies often repeat similar failures, producing uninformative rollout trajectories and limiting effective policy optimization. Existing skill-centric methods improve exploration by optimizing, filtering, or internalizing reusable skills. However, they remain centered on the skills themselves rather than being designed as adaptive training-time support for the evolving policy. To address this, we propose a policy-centric training paradigm that reframes skills as a dynamic training scaffold. Our framework, PATs, converts rollout groups from the latest policy into evidence cards and uses task-specific evaluation to adjust the context used in subsequent rollouts. Concrete guidance helps weak policies to complete challenging tasks. As policy improves, redundant context is revised or removed to reduce reliance on explicit guidance while preserving useful rollout variation. The policy is optimized with environmental rewards using standard RLVR, and the training scaffold is discarded at deployment. On ALFWorld and WebShop, PATs improves over strong baselines by up to 18.6%. Across seven search-augmented QA benchmarks, it remains competitive while using 32.1% fewer prompt tokens than the baseline.
> ![](images/1835c7fe3eb0ea61f3773fab9d97a66684708beb3f765b0f852c8d77259c1644.jpg)
> Figure 1: Seed-0 training dynamics on 1.5B ALFWorld under the shared 150-step RL budget. Left: validation success rate. Right: mean prompt tokens per policy call; faint traces are raw logs and bold traces are seven-step moving averages. Pats expands its training context early and later contracts it as validation improves, whereas SkillRL's context grows and SKILL0 follows staged withdrawal.

Q1: 这篇论文试图解决什么问题？

长期LLM智能体强化学习中，策略容易陷入重复失败模式，产生低质量rollout轨迹，导致策略更新效率低下。现有方法聚焦于技能的提取与复用，但缺乏对当前策略演化状态的适应性：技能库要么静态，要么随策略改进而膨胀，未能动态调整指导粒度。PATS旨在解决如何在训练过程中为策略提供自适应、可收缩的训练脚手架，在初期提供具体指导，在策略成熟后减少冗余提示，从而提升样本效率和最终性能。

Q2: 有哪些相关研究？

相关研究包括三方面：1）技能中心方法（SkillRL、SKILL0等），通过优化、过滤或内化技能改善探索；2）基于上下文的RL，利用提示或演示提供任务指导；3）LLM智能体的RL微调（如RLVR、PPO等）。PATS区别于上述方法在于其以策略为中心的训练支架，可根据策略性能动态调整上下文，避免了技能库的单向增长或不适应性。

Q3: 论文如何解决这个问题？

PATS的核心是将rollout组转化为‘证据卡片’（evidence cards），这些卡片记录成功或失败的轨迹片段。在每个训练轮次，基于任务特定评估指标（如验证成功率），PATS从当前rollout池中选择相关卡片组成上下文，注入后续策略rollout的提示中。随着策略提升，评估指标改善，PATS逐步移除冗余卡片，减少不必要的指导。整个过程使用标准RLVR（如GRPO）优化策略，训练完成后丢弃整个支架。方法包括证据卡片生成、上下文自适应调度和RLVR优化三个关键模块。

Q4: 论文做了哪些实验？

实验在三个场景进行：1）ALFWorld（1.5B模型），训练150步RL，比较基线包括SkillRL、SKILL0、PPO等；2）WebShop（7B模型），类似比较；3）七个搜索增强QA基准（7B模型），评估零样本和少样本性能。指标包括成功率、WebShop得分、提示token数。消融研究包括移除证据卡片、固定上下文、随机卡片选择等。

Q5: 发现了什么实验现象？

实验现象：PATS在训练初期上下文长度迅速扩展，随验证成功率提升后逐渐收缩，而SkillRL的上下文持续增长，SKILL0呈现阶段式退出。PATS在ALFWorld上相比最强基线提升18.6%，在WebShop上提升显著且提示token减少32.1%。在搜索QA基准上，PATS在保持竞争性能的同时使用更少token。消融显示自适应卡片选择对性能至关重要。

Q6: 有什么可以进一步探索的点？

未来方向包括：1）更复杂的证据卡片选择机制，如学习型调度或图结构；2）扩展到更广泛的长期任务（机器人操作、工具使用）；3）与更高效RL算法（如PPO、IMPALA）结合；4）理论分析支架收敛性与最优性；5）跨任务迁移学习或元学习以加速新任务适应。

Q7: 总结一下论文的主要内容

本文针对长期LLM智能体强化学习中策略重复失败的问题，提出以策略为中心的训练范式PATS。该框架将rollout组转化为可动态调整的证据卡片，作为训练支架提供自适应指导。PATS通过任务特定评估调整上下文内容：在策略较弱时提供具体成功/失败案例，策略改进后移除冗余提示以保持简洁。策略使用标准RLVR优化，支架在部署时完全丢弃。在ALFWorld和WebShop上，PATS显著提升成功率并减少提示token消耗；在搜索增强QA基准上，保持竞争力同时降低32.1% tokens。实验证明自适应上下文调度优于固定或单调增长的技能库方法。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接涉及LLM智能体强化学习，与用户智能体方向高度相关。

## 基本信息

- 作者：Yipeng Shi, Zhipeng Ma, Yue Wang, Qitai Tan, Yang Li, Peng Chen, Zhengzhou Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.21419`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括abstract、conclusion和main results片段，并结合heuristic_draft进行润色补全。
