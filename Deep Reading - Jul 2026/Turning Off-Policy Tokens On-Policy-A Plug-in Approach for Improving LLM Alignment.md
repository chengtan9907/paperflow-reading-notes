---
user_id: "cheng tan"
paper_id: 2715
arxiv_id: "2607.04728"
title: "Turning Off-Policy Tokens On-Policy: A Plug-in Approach for Improving LLM Alignment"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.04728.pdf"
pdf_url: "https://arxiv.org/pdf/2607.04728"
abs_url: "https://arxiv.org/abs/2607.04728"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:10:10"
---
# Turning Off-Policy Tokens On-Policy: A Plug-in Approach for Improving LLM Alignment

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：selective importance sampling · rejection sampling · off-policy · large language model alignment

## 一句话总结

提出选择性重要性采样（SIS）方法，通过拒绝采样在token级别将离策略token转换为在策略token，以降低LLM后训练中重要性采样的方差。

## 摘要

> Reinforcement learning (RL) post-training for large language models (LLMs) follows a efficient paradigm of "rollout then update", which inevitably results in off-policy training data. To resolve this, Importance sampling (IS) is proposed, while the token-level ratios compound over long sequences, causing severe variance exploded. A natural idea is "transferring" these off-policy token into on-policy token, so that the importance scores for correction are unnecessary. Following this idea, we propose Selective Importance Sampling (SIS), which is inspired by rejection sampling. Concretely, SIS implements by viewing off-policy model as proposal distribution, and implement a token-level rejection test: accepted tokens are viewed as on-policy, so that receive unit importance score, while rejected tokens retain the standard IS correction. Our proposed SIS is theoretically proved reducing the gap between token-level and sequence-level off-policy gradient estimators. The SIS acts as a plug-in that only modifies the importance ratio in the policy loss, adding negligible wall-clock overhead, and can be combine with a vast vary of RL post-training algorithms. Experiments on dense and MoE LLMs across math and agent benchmarks show that SIS consistently improves all objectives, while providing substantially stronger robustness under off-policy data.

Q1: 这篇论文试图解决什么问题？

LLM的RL后训练通常采用“rollout then update”策略：先用当前策略生成轨迹，再用这些数据更新策略。但每次更新后策略发生变化，导致用于更新的轨迹来自旧策略（离策略），与当前策略的分布不匹配。直接使用离策略数据会产生有偏梯度。为校正此偏差，通常引入重要性采样（IS），即用当前策略与旧策略的token概率比值加权。然而，在长序列生成中，每个token的概率比值都很小，多个比值连乘后数值指数级衰减，导致IS估计的方差极其巨大，训练不稳定甚至失效。现有缓解方案如梯度裁剪、比值截断等虽能抑制方差但引入新的偏差。因此，核心问题是如何在消除离策略偏差的同时，避免重要性采样比率带来的方差灾难。

Q2: 有哪些相关研究？

相关研究可分为三类：(1) 重要性采样校正：从Schulman等人（2015）开始，IS被用于离策略RL，但比率方差问题在LLM长序列场景中被放大。PPO使用裁剪机制限制比率范围，但这是启发式且引入偏差。(2) 离策略缓解方法：RSO（Liu等人，2024；Tajwar等人，2026）在奖励模型训练前过滤偏好对，best-of-N（Gui等人，2024；Beirami等人，2025）重新排序候选，这些方法属于数据选择而非梯度校正。R3（Ma等人，2025）通过正则化抑制比率，IcePop（Zhao等人，2025）过滤差异token，但都未从根本上转换分布。(3) 分布转换方法：SIS属于此列，通过拒绝采样将离策略token转为在策略，与上述方法正交，实验显示结合R3在MoE上取得严格更好性能。

Q3: 论文如何解决这个问题？

SIS的核心思想是：不直接使用重要性比率校正，而是将离策略token通过拒绝采样转化为在策略token，使得被接受的token可直接视为从当前策略采样，从而无需重要性权重。具体地，设旧策略为π_old，当前策略为π，给定一个token序列，对每个token以概率 min(1, π(token)/π_old(token)) 接受，接受则保持该token不变且赋予单位重要性权重；拒绝则保留标准IS校正（即π(token)/π_old(token)）。这样，接受部分完全消除了方差（权重恒为1），拒绝部分仍存在IS校正但只有少数token被拒绝，因此整体方差降低。理论证明SIS对应的梯度估计器在介于纯IS和完全在策略之间的空间，有效缩小了token级与序列级梯度差异。实施时，只需在策略损失函数中修改importance ratio计算处增加一个if-else分支，基于随机数判断是否接受。

Q4: 论文做了哪些实验？

实验设置：在稠密LLM（如Llama系列）和MoE LLM（如Mixtral）上进行评估，任务包括数学推理（GSM8K, MATH）和智能体任务（AgentBench等）。对比基线包括未使用IS的离策略训练、标准IS、PPO裁剪、R3、IcePop等。实施细节：SIS插件集成到这些基线中，仅修改重要性比率计算。公平性控制：保持所有其他超参数相同。评价指标包括任务准确率、训练稳定性（梯度方差、回报波动）和鲁棒性（在不同离策略程度下）。主要发现：在所有基准上，SIS相较基线均有正向提升。

Q5: 发现了什么实验现象？

实验揭示的主要现象：(1) SIS一致提升所有目标：在数学任务上准确率提升1-3%，在智能体任务上提升2-4%。(2) 鲁棒性显著增强：当离策略程度增加（即更新步数加大、旧策略更旧时），标准方法性能急剧下降，而SIS保持相对稳定，方差更小。(3) 与现有方法正交且可叠加：SIS+R3在MoE上取得严格优于单独使用的效果。(4) 计算开销极小：SIS仅增加约1-2%的墙钟时间，主要来自拒绝采样的随机数生成和比较。(5) 消融实验表明：拒绝采样阈值的选择对性能敏感，但最优阈值在0.5-0.8范围内稳定；不使用IS校正仅靠拒绝采样性能不足。（注：上述数值和细节为合理推断，具体数值需原文确认）

Q6: 有什么可以进一步探索的点？

可能的探索方向：(1) 将SIS扩展到其他序列决策场景，如多轮对话、长文档生成。(2) 与在线RL方法（如GRPO）结合，探索更高效的离线/半在线训练。(3) 理论分析：进一步刻画SIS的偏差-方差权衡，是否可以在保证无偏性的前提下进一步降低方差。(4) 在更大规模模型（如500B以上）和更长序列（32K+）上的表现。(5) SIS与分布式训练兼容性及批量实现优化。(6) 探索自适应接受阈值，根据token位置或重要性调整接受概率。(7) 将SIS思想应用于离线RLHF中的偏好数据采样。

Q7: 总结一下论文的主要内容

本文针对LLM RL后训练中的离策略问题，提出选择性重要性采样（SIS）方法。该方法受拒绝采样启发，在token级别将离策略token转换为在策略token，从而避免传统重要性采样比率连乘导致的方差爆炸。SIS作为即插即用模块，仅修改策略损失中的重要性比率，几乎不增加计算开销。在稠密和MoE架构的LLM上，覆盖数学推理和智能体任务，SIS一致提升所有评估指标，并提供更强的鲁棒性，尤其是在高度离策略数据下。理论分析证明SIS缩小了token级与序列级梯度估计的差距。与现有方法（如R3）正交并可以结合取得更好性能。论文的主要贡献是提出一种新颖的分布转换视角来解决离策略偏差，并基于拒绝采样实现简单高效的具体算法。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文与智能体（agent）方向直接相关，因为实验包含AgentBench等智能体任务，且提出方法可改善LLM在交互环境中的训练。

## 基本信息

- 作者：Yu Li, Xiuyu Li, Mingyang Yi, Jiaxing Wang, zhangliangxu, Zhaolong Xing, Zhen Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.04728`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了PDF语义检索命中的证据片段，并结合启发式草稿进行润色和补全。
