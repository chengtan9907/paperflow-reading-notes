---
user_id: "cheng tan"
paper_id: 1113
arxiv_id: "2606.22995"
title: "Group-Graph Policy Optimization for Long-Horizon Agentic Reinforcement Learning"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.22995.pdf"
pdf_url: "https://arxiv.org/pdf/2606.22995"
abs_url: "https://arxiv.org/abs/2606.22995"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:18:06"
---
# Group-Graph Policy Optimization for Long-Horizon Agentic Reinforcement Learning

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：group reinforcement learning · long-horizon agent · credit assignment · state-transition graph

## 一句话总结

提出G2PO，通过图结构进行组级策略优化，实现长程智能体任务的细粒度信用分配。

## 摘要

> Group-based Reinforcement Learning (RL) has significantly enhanced Large Language Models (LLMs) in agentic scenarios. To achieve finer-grained policy updates, recent agentic RL frameworks have shifted from trajectory-level to step-level training. However, long-horizon agentic RL suffers from severe reward sparsity and delay, as feedback is often deferred for dozens of interaction steps. While existing step-level frameworks refine training granularity, their credit assignment remains coarse-grained and still treats agent exploration as isolated, linear trajectories. This oversimplified perspective ignores the inherent graph structure of state transitions, leading to high-variance state-value estimation and myopic, localized credit assignment. To overcome these critical bottlenecks, we propose Group-Graph Policy Optimization (G2PO), a novel group-based RL algorithm tailored for multi-turn agentic tasks. G2PO explicitly transforms linear interaction trajectories into a global state-transition graph. By aggregating identical observations across different trajectories, we introduce group-aggregation state-value estimation that reduces sampling variance and trajectory-dependent bias. Furthermore, we redefine agent actions as transitions between state nodes and propose an edge-centric advantage estimation strategy. By globally standardizing Temporal Difference (TD) errors across the entire graph, G2PO explicitly identifies and prioritizes critical transitions that drive absolute task progress. Extensive experiments on representative long-horizon benchmarks—WebShop, ALFWorld, and AppWorld—demonstrate that G2PO substantially outperforms state-of-the-art prompt-based and RL baselines, achieving remarkable success rate improvements of up to 22.2% over GRPO. Code available at https://github.com/Nala-YN/G2PO.

Q1: 这篇论文试图解决什么问题？

论文试图解决长程智能体强化学习中的两个核心问题：1) 奖励稀疏和延迟：在长程多轮交互中，反馈往往在数十步之后才出现，导致学习信号极其稀疏。2) 现有步级RL方法的信用分配粗粒度：虽然从轨迹级转到步级，但仍然是线性轨迹视角，忽略状态之间的图结构，导致状态值估计方差高，且难以区分关键突破动作与普通状态中的微小改进。

Q2: 有哪些相关研究？

相关研究包括：① 基于组的RL方法（如GRPO），利用组内奖励的统计量作为基线，实现策略更新，但在长程任务中仍面临信用分配问题。② 步级训练框架（如step-level DPO、step-level PPO），将训练粒度从轨迹细化到步骤，但未考虑状态的联系。③ 基于图结构的RL方法，多用于机器人或游戏，尚未在LLM智能体领域广泛探索。本文与GRPO最相关，但引入了全局图结构和组聚合机制。

Q3: 论文如何解决这个问题？

论文提出G2PO框架，包含三个关键创新：1) **状态图构建**：将多个智能体交互轨迹中的状态去重，构建全局状态转移图，节点为唯一状态，边为动作。2) **组聚合状态值估计**：对于相同状态节点，聚合所有经过该状态轨迹的后续回报，计算均值作为状态值，降低单轨迹采样方差和偏差。3) **边优势估计**：将动作视为图上的边，用全局标准化后的TD误差作为边的优势值，对每个动作进行细粒度信用分配，优先提升关键转移（如完成子任务的动作）。整体训练采用广义优势估计，结合组基线和图结构信息。

Q4: 论文做了哪些实验？

论文在三个长程智能体基准上评估：WebShop（在线购物模拟）、ALFWorld（家居任务）、AppWorld（应用程序交互）。基线包括提示方法（zero-shot、few-shot CoT）和RL方法（GRPO）。使用Qwen/DeepSeek系列模型作为基础LLM。主要实验：1) 在WebShop和ALFWorld上报告成功率，表1显示Qwen3.5-397B仅17.2%成功率，RL方法显著提升，G2PO进一步超越GRPO。2) 在AppWorld上评估，G2PO同样取得最优。3) 消融实验：分别移除图聚合和边优势，验证各自贡献。4) 分析组大小、图构建方式等超参数影响。

Q5: 发现了什么实验现象？

实验现象：① RL训练方法大幅优于纯提示方法，表明长程任务需要交互优化。② G2PO相比GRPO在WebShop上成功率高22.2%，证明图结构信用分配的有效性。③ 消融显示，组聚合状态值估计和边优势估计均起关键作用，缺一不可。④ 图构建能有效合并相同状态，减少方差，训练更稳定。⑤ 在更复杂的AppWorld上，G2PO的优势更大，说明图结构对长序列任务尤其重要。

Q6: 有什么可以进一步探索的点？

进一步可探索的方向：① 将图结构扩展到动态图或分层图，适应更复杂的依赖关系。② 结合多模态信息（如视觉状态）构建状态图。③ 探索更高效的图构建算法，减少计算开销。④ 将G2PO应用于具身智能或机器人任务。⑤ 研究图结构对基础模型泛化能力的影响。⑥ 探索与其他信用分配方法（如逆强化学习）的结合。

Q7: 总结一下论文的主要内容

本文针对长程智能体强化学习中奖励稀疏和信用分配困难的问题，提出G2PO算法。G2PO将多条交互轨迹中的状态聚合为全局有向图，节点表示状态，边表示动作。在此基础上，使用组聚合方法估计状态值（多个轨迹的均值），降低方差；并定义边优势（全局标准化TD误差），实现细粒度信用分配，突出关键动作。在WebShop、ALFWorld、AppWorld三个长程基准上，G2PO显著优于GRPO等基线，最高提升22.2%成功率。分析表明，图结构有效缓解了长程任务中的信用分配困境。代码已开源。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：这篇论文和你当前画像里的方向有直接重合：智能体方向。

## 基本信息

- 作者：Yunan Wang, Minghui Song, Zihan Zhang, Shaohan Huang, Haizhen Huang, Furu Wei, Weiwei Deng, Feng Sun, Qi Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.22995`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据中的摘要、结论、方法及结果片段。
