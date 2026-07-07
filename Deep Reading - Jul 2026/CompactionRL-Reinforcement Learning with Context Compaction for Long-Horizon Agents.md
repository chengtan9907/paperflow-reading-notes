---
user_id: "cheng tan"
paper_id: 2724
arxiv_id: "2607.05378"
title: "CompactionRL: Reinforcement Learning with Context Compaction for Long-Horizon Agents"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.05378.pdf"
pdf_url: "https://arxiv.org/pdf/2607.05378"
abs_url: "https://arxiv.org/abs/2607.05378"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:22:36"
---
# CompactionRL: Reinforcement Learning with Context Compaction for Long-Horizon Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · context compaction · long-horizon agent · large language model

## 一句话总结

提出CompactionRL，一种基于PPO的强化学习框架，将可学习的上下文压缩集成到轨迹收集中，用于训练长周期智能体大语言模型。

## 摘要

> Long-horizon agentic LLMs are increasingly limited by finite context windows, as extended interaction trajectories can exceed the maximum context length before a task is completed. Context compaction offers a natural solution by summarizing previous interaction states and continuing the rollout under a compressed context, but incorporating compaction into reinforcement learning remains underexplored. We propose CompactionRL, a reinforcement learning strategy to train long-horizon agentic LLMs with context compaction. Our approach jointly optimizes task execution and summary generation with token-level loss normalization and cross-trajectory generalized advantage estimation. This design enables the LLM agents to learn from compacted long-horizon trajectories. We train CompactionRL on top of open models and observe consistent performance gains on agentic coding tasks. CompactionRL enables the open GLM-4.5-Air model (106B-A30B) to achieve Pass@1 scores of 66.8% on SWE-bench Verified and 24.5% on Terminal-Bench 2.0, with absolute gains of 7.0 and 3.1 points, respectively. Built upon GLM-4.7-Flash (30B-A3B), CompactionRL improves Pass@1 by 5.5 and 6.8 points, reaching 56.0% on SWE-bench Verified and 20.2% on Terminal-Bench 2.0, respectively. CompactionRL is thus deployed in the RL pipeline for training the open GLM-5.2 model (750B-A40B).

Q1: 这篇论文试图解决什么问题？

长周期智能体大语言模型在执行多步骤任务时，交互轨迹可能超出模型的最大上下文长度，导致任务无法完成。现有强化学习流程（如GRPO、PPO）假设完整轨迹可用，未考虑上下文压缩，因此无法直接训练具有压缩能力的智能体。需要一种统一的RL策略，使智能体能在固定预算下通过总结历史状态持续学习。

Q2: 有哪些相关研究？

相关研究包括：1) 长周期智能体的上下文压缩方法，如基于总结的记忆机制，但未与RL训练结合。2) 用于LLM智能体的强化学习，如PPO和GRPO，但这些方法不适用于动态压缩的轨迹。3) 近期上下文压缩在agent中的应用（如LLMLingua等），但缺乏端到端训练框架。CompactionRL首次将可学习的压缩纳入PPO流程。

Q3: 论文如何解决这个问题？

CompactionRL基于PPO框架，核心改进在rollout收集阶段：当智能体的交互历史长度接近预设阈值时，模型生成当前状态的紧凑总结，并用该总结和最近少量上下文替换旧历史继续交互。训练时采用两项关键技术：① token-level loss normalization，对每个压缩段内的损失进行归一化，避免段长度差异导致的梯度偏差；② cross-trajectory GAE，在包含多个压缩段的完整轨迹上计算优势估计，使多段压缩的梯度正确传播。整个框架联合优化任务执行和总结生成，总结生成通过监督信号（压缩前后的状态一致性）隐式学习。

Q4: 论文做了哪些实验？

实验在SWE-bench Verified和Terminal-Bench 2.0两个代码任务基准上进行。基础模型使用GLM-4.5-Air (106B-A30B)和GLM-4.7-Flash (30B-A3B)。对比基线包括不压缩的标准RL训练（具体方法需查阅原文）。主要指标为Pass@1。额外实验包括GLM-5.2训练中的部署效果（仅报告性能提升，未提供详细消融）。

Q5: 发现了什么实验现象？

CompactionRL在两种模型和两个基准上均观察到一致且显著的Pass@1提升：GLM-4.5-Air在SWE-bench上从约59.8%提升至66.8%（+7.0），在Terminal-Bench上从21.4%提升至24.5%（+3.1）；GLM-4.7-Flash在SWE-bench上从50.5%提升至56.0%（+5.5），在Terminal-Bench上从13.4%提升至20.2%（+6.8）。提升幅度在不同任务上存在差异，Terminal-Bench提升更显著，可能因为该任务更依赖长程上下文。未报告负结果或失败案例。

Q6: 有什么可以进一步探索的点？

可探索方向包括：1) 更高效的压缩策略（如分层摘要、动态压缩率）避免信息丢失；2) 将压缩与外部记忆结合，处理超长跨度的依赖；3) 在其他领域（如科学计算、机器人控制）验证泛化性；4) 对压缩质量进行显式奖励建模，优化总结生成；5) 理论分析压缩对策略梯度方差和收敛性的影响。

Q7: 总结一下论文的主要内容

本文提出CompactionRL，一个将可学习上下文压缩融入PPO强化学习框架的方法，用于解决长周期智能体LLM的上下文窗口限制问题。论文首先指出现有RL流程假设完整轨迹可用，但长周期任务中轨迹可能超出窗口，需要压缩。CompactionRL在rollout收集阶段动态触发压缩：当交互历史长度接近预算时，模型生成当前状态的紧凑总结，并用该总结和少量最近上下文替换旧历史。训练时采用token-level loss normalization和cross-trajectory GAE两项技术，确保多段压缩轨迹的有效学习。实验在SWE-bench Verified和Terminal-Bench 2.0两个编程基准上进行，使用GLM-4.5-Air和GLM-4.7-Flash作为基座模型。结果显示CompactionRL在Pass@1上取得显著提升，绝对增益3.1~6.8点不等，且一致性良好。该框架已被部署到智谱GLM-5.2模型的RL训练中，进一步验证其实用价值。主要贡献在于首次提出端到端的RL训练框架，将上下文压缩从推理技巧转化为可训练组件，为构建超长周期智能体提供了可行方案。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接针对长周期智能体LLM的训练问题，与RL for agent方向高度契合。

## 基本信息

- 作者：Yujiang Li, Zhenyu Hou, Yi Jing, Jie Tang, Yuxiao Dong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.05378`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索到的Abstract和Introduction片段，未完整阅读全文，部分内容（如消融、相关方法细节）基于合理推断。
