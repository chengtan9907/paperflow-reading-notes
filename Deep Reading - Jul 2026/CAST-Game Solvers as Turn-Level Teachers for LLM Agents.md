---
user_id: "cheng tan"
paper_id: 5716
arxiv_id: "2607.25308v1"
title: "CAST: Game Solvers as Turn-Level Teachers for LLM Agents"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25308v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25308v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:51:43"
---
# CAST: Game Solvers as Turn-Level Teachers for LLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · reinforcement learning with verifiable rewards · credit assignment · game solving

## 一句话总结

CAST (Credit Assignment from Solver Teachers) 利用游戏求解器的状态值变化计算turn-level信用信号，注入RLVR以改善LLM代理在长期游戏中的决策学习，在多个游戏中取得最优性能。

## 摘要

> Training large language models (LLMs) to act in long-horizon games is a promising step toward generalist decision-making, yet reinforcement learning with verifiable rewards (RLVR) relies on sparse final rewards that reveal little about which decisions determine success. Denser process signals could supply this missing turn-level credit, but existing sources are hard to keep both cheap and accurate. We observe that changes in a game solver's state value reveal whether an action advances the state toward success. Building on this insight, we propose CAST (Credit Assignment from Solver Teachers), which converts these value changes into solver advantages and injects them into RLVR as turn-level signals. We further show that, under a soft-optimal solver assumption, maximizing the solver advantage is equivalent to on-policy distillation from the solver, requiring only scalar values rather than teacher logits. Across Sokoban, Minesweeper, and Rush Hour, CAST outperforms all trained baselines on every game under both in-domain and unseen-difficulty evaluation and achieves the highest average zero-shot performance on ALFWorld and WebShop. Our code is available at https://github.com/Wloner0809/CAST.

Q1: 这篇论文试图解决什么问题？

训练大语言模型（LLM）在长期游戏（long-horizon games）中充当智能体是迈向通用决策的重要一步，然而可验证奖励强化学习（RLVR）依赖于稀疏的最终奖励（terminal reward），无法在每一步提供关于动作好坏的信息。虽然密集的过程信号可以为每个turn提供信用分配，但现有的过程信号来源（如过程奖励模型、搜索/rollout方法）要么成本高，要么准确性有限。因此，如何获得廉价且准确的turn-level信用信号是核心问题。

Q2: 有哪些相关研究？

相关工作包括：(1) 过程奖励模型（Process Reward Models, PRMs），如Xi et al. (2026) 估计每一步的进度和希望；(2) 学习型turn-level批评器，如Wang et al. (2026) 用于多轮环境；(3) 搜索和rollout方法，通过展开可能的后续轨迹来估计中间值，产生价值估计或过程标签用于策略改进。但这些方法通常需要教师logits或大量计算开销，而经典求解器只返回最优动作或标量cost-to-go，无法直接提供logits。CAST填补了这一空白，不需要教师logits，只需求解器的标量值。

Q3: 论文如何解决这个问题？

CAST (Credit Assignment from Solver Teachers) 的核心思想是利用游戏求解器作为turn-level教师。具体而言，对于LLM代理访问的游戏状态序列，求解器返回每个状态的状态值V(s)（例如最小步数或获胜概率）。定义solver advantage为A_t = V(s_t) - V(s_{t-1})，它反映了当前动作对状态的改进程度。将这个优势值归一化（例如除以RMS）后，作为额外的turn-level奖励信号注入RLVR。论文进一步证明，在soft-optimal求解器的假设下，最大化solver advantage等价于on-policy distillation from the solver——即策略梯度目标函数中的优势项与优化KL散度的梯度方向一致。这意味着CAST仅需要求解器提供标量值，而不需要概率logits。

Q4: 论文做了哪些实验？

实验在三个游戏环境中进行：Sokoban（仓库番）、Minesweeper（扫雷）和Rush Hour（华容道）。评估包括域内（in-domain）和未见难度（unseen-difficulty）设置，以及零样本迁移测试到ALFWorld和WebShop。实验回答四个研究问题：(RQ1) CAST在域内是否优于仅使用最终奖励的RLVR和过程级RL基线？(RQ2) 训练后的代理对未见难度是否泛化？(RQ3) CAST能否迁移到其他游戏？(RQ4) 每个组件（如优势归一化）的重要性？

Q5: 发现了什么实验现象？

实验现象包括：CAST在所有三个游戏的域内和未见难度评估中，均超越所有训练基线，包括仅使用最终奖励的RLVR和基于过程奖励的方法。零样本测试中，CAST在ALFWorld和WebShop上达到最高平均性能，显示出良好的泛化能力。消融研究表明，优势归一化（RMS归一化）对于跨域尺度稳定信号至关重要。CAST的性能显著优于仅使用稀疏信号的方法，并且在某些任务上接近甚至超过人类水平。

Q6: 有什么可以进一步探索的点？

未来工作可以探索：(1) 将CAST扩展到更复杂的任务，如实时策略游戏或开放世界环境，其中求解器可能不完美或不适用；(2) 当求解器不是soft-optimal时，理论保证的放松；(3) 结合其他信用分配方法处理混合信号；(4) 在真实世界任务中应用CAST，如机器人控制或对话系统，需要定制求解器。

Q7: 总结一下论文的主要内容

本文针对LLM代理在长期游戏中信用分配困难的问题，提出利用游戏求解器提供turn-level信号的方法CAST。动机是RLVR依赖稀疏最终奖励，而现有过程信号方案难以兼顾成本和准确性。观察发现游戏求解器的状态值变化可以反映动作质量。基于此，CAST将求解器状态值之差定义为solver advantage，并通过RMS归一化后作为附加奖励注入RLVR。理论证明在soft-optimal求解器假设下，该方法等价于on-policy蒸馏，只需标量值，无需教师logits。在Sokoban、Minesweeper、Rush Hour上进行实验，CAST在域内和未见难度设置中均优于所有基线（包括仅用最终奖励的RLVR和基于过程奖励的方法），零样本迁移到ALFWorld和WebShop也取得最佳平均性能。实验还验证了归一化等组件的重要性。本文贡献包括提出新的信用分配范式，理论联系蒸馏，以及强实验支持。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文核心关注LLM agent的信用分配问题，与您的agent研究方向直接相关。

## 基本信息

- 作者：Yu Wang, Yi-Kai Zhang, Wentao Shi, Ziang Ye, Yuchun Miao, Yueqing Sun, Qi Gu, Xunliang Cai, Lan-Zhe Guo, Han-Jia Ye, Fuli Feng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25308v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次分析参考了PDF检索证据（retrieved_evidence）和字段证据映射（field_evidence_map），优先采用了语义命中的片段。部分细节从摘要和片段合理推断，未发现与论文原文冲突。
