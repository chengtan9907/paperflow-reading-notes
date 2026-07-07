---
user_id: "cheng tan"
paper_id: 2716
arxiv_id: "2607.04963"
title: "STAPO: Selective Trajectory-Aware Policy Optimization for LLM Agent Training"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.04963.pdf"
pdf_url: "https://arxiv.org/pdf/2607.04963"
abs_url: "https://arxiv.org/abs/2607.04963"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:13:24"
---
# STAPO: Selective Trajectory-Aware Policy Optimization for LLM Agent Training

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · large language model · agent training · trajectory neglect

## 一句话总结

提出归一化熵（normalized entropy）以定位轨迹忽视步骤，并在此基础上构建STAPO框架，通过层次化分组RL结合轨迹感知奖励与独立惩罚，在ALFWorld、WebShop及搜索增强问答等长程任务上取得SOTA性能。

## 摘要

> Reinforcement Learning (RL) is the dominant paradigm for training Large Language Model (LLM) agents on long-horizon tasks. However, sparse and delayed rewards often lead to trajectory neglect, in which agents lose focus on the task goal and interaction history at intermediate steps. Prior work has explored step-level supervision using Shannon-entropy-based uncertainty signals, which conflate inherent state complexity with agent confidence and therefore provide unreliable estimates of decision reliability. To address this issue, we propose normalized entropy, which measures confidence deviations relative to an agent's average behavior under a given state, thereby strengthening the association between low-quality actions and trajectory neglect. Building on this insight, we introduce Selective Trajectory-Aware Policy Optimization (STAPO), a hierarchical group-based RL framework. STAPO leverages normalized entropy to locate outlier steps associated with trajectory neglect and optimizes them via a joint mechanism of trajectory-aware reward and trajectory-independent penalty, enhancing trajectory awareness while preserving training stability. Extensive experiments on ALFWorld, WebShop, and Search-Augmented QA demonstrate that STAPO achieves state-of-the-art performance while substantially alleviating trajectory neglect, validating its effectiveness and robustness for agentic tasks.

Q1: 这篇论文试图解决什么问题？

论文试图解决以下问题：1）在长程任务中，稀疏和延迟的奖励导致智能体在中间步骤出现轨迹忽视，即智能体逐渐偏离任务目标或遗忘交互历史，从而产生低质量动作。2）现有基于香农熵的步骤级不确定性监督方法误将状态复杂度当作智能体不确定性，无法可靠地标识低质量决策步骤。因此，核心科学问题是如何精确识别并纠正LLM agent在长程交互中的轨迹忽视行为。

Q2: 有哪些相关研究？

相关研究主要包括：1）基于强化学习的LLM agent训练方法，如使用PPO、Reinforce等算法优化agent在环境中的累积奖励；2）针对稀疏奖励的探索策略，如内在奖励、课程学习等；3）步骤级监督信号的研究，特别是利用熵或不确定性来指导智能体关注关键步骤；4）长程任务中的轨迹建模，如轨迹级别信用分配。本文特别指出，先前基于香农熵的方法无法区分状态随机性与智能体置信度，导致监督不可靠。此外，ALFWorld、WebShop和搜索增强QA等环境是典型的agent任务基准。STAPO的层次化分组RL框架还与分组策略优化方法（如GRPO）相关，但本文着重于解决轨迹忽视的特定挑战。

Q3: 论文如何解决这个问题？

论文提出STAPO框架，包含两个核心组件：1）归一化熵：对于给定状态，计算智能体动作分布的熵，并通过与历史或平均行为下的熵进行比较，得到归一化指标，从而反映智能体在该状态下的决策可靠性偏差。归一化熵能更严格地关联低质量动作与轨迹忽视，避免状态复杂度带来的混淆。2）层次化分组RL框架：首先通过归一化熵统计定位轨迹中的异常步骤（Outlier Localization），这些步骤很可能是轨迹忽视的体现；然后对这些异常步骤实施选择性优化（Selective Optimization），同时施加轨迹感知奖励和轨迹无关惩罚。轨迹感知奖励鼓励智能体在整个轨迹中保持连贯的行为，而轨迹无关惩罚作为正则化项提供稳定性。该框架在组级别进行策略更新，能够有效地重新聚焦于被忽视的步骤，同时不干扰正常表现良好的步骤。

Q4: 论文做了哪些实验？

论文在三个典型长程智能体任务上进行实验：1）ALFWorld：一个文本交互的具身任务环境；2）WebShop：基于网页浏览的电商购物任务；3）Search-Augmented QA：结合搜索的问答任务。实验设置中，STAPO与多种基线方法对比，包括经典RL方法（如PPO）、现有步骤级方法（如基于Shannon熵的方法）以及直接使用环境奖励的基线。主要评估指标为任务成功率和轨迹质量指标（如完成步数等）。进行了消融研究以验证归一化熵和STAPO各组件的作用，并分析了计算开销（附录J显示额外约5.7%的代价）。在多个随机种子下报告统计结果。

Q5: 发现了什么实验现象？

从检索证据中未获得详细的实验观察数据。根据论文描述，主要观察包括：1）STAPO在所有三个环境中取得最优成功率，显著优于基于Shannon熵的步骤级方法，验证了归一化熵的优势；2）归一化熵能更精确地定位导致轨迹忽视的异常步骤，消融实验表明移除该组件导致性能下降；3）轨迹感知奖励与独立惩罚的共同作用既能提升轨迹连贯性，又不会牺牲策略稳定性；4）在视觉-语言agent的初步实验（附录L）中显示有潜力，但尚未全面探索；5）计算开销可控，与扩大组大小相比更经济。可能还存在一些负结果，但检索证据未提及。

Q6: 有什么可以进一步探索的点？

以下方向值得进一步探索：1）多模态扩展：将STAPO应用于更广泛的视觉-语言智能体设置，特别是在具有不同视觉界面的任务中验证归一化熵和异常定位的有效性。2）更复杂的credit assignment：结合更细粒度的信用分配机制，除了步骤级，还可以考虑子轨迹级。3）跨任务泛化：研究归一化熵是否能在不同任务类型间通用，是否需要任务特定的标准化。4）与搜索/规划结合：如何将轨迹感知奖励与显式规划结合。5）扩展到在线学习场景：当前框架假设离线或固定轨迹，在线场景下需要适应。6）理论基础：进一步分析归一化熵的理论性质，如与最优策略的关系。

Q7: 总结一下论文的主要内容

本文从强化学习中稀疏奖励导致LLM agent轨迹忽视的问题出发，指出已有基于Shannon熵的步骤级不确定性监督不准确的原因——混淆了状态随机性和智能体置信度。作者提出归一化熵（normalized entropy），通过将当前动作分布的熵与智能体在该状态下的平均熵比较，消除状态复杂度的影响，从而可靠地识别因轨迹忽视产生的低质量动作。基于此，设计了STAPO（Selective Trajectory-Aware Policy Optimization），一个层次化分组RL框架，包含两阶段操作：异常定位阶段，利用归一化熵的统计特征动态定位轨迹中的异常步骤；选择性优化阶段，对这些异常步骤联合采用轨迹感知奖励（鼓励轨迹一致性）和轨迹无关惩罚（维持训练稳定性），而其他步骤保持正常策略更新。在ALFWorld、WebShop和搜索增强问答三个代表性的长程智能体任务上，STAPO均取得最优成功率，且消融实验证实归一化熵和联合优化机制的关键作用。分析表明，计算开销增加约5.7%，但相比扩大组大小更经济。作者还给出了在视觉-语言agent上的初步结果，但指出更广泛的多模态设置仍需探索。论文的主要贡献在于：1）定义轨迹忽视问题并分析现有熵方法的局限；2）提出归一化熵以精准定位异常步骤；3）构建实用的STAPO框架；4）在多个任务上验证有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文直接针对LLM智能体在长程任务中的轨迹忽视问题，与智能体研究方向高度重合（权重0.10）。

## 基本信息

- 作者：Qiuyi Qi, Tian Liang, Mutian Bao, Jinjian Zhang, Dongnan Liu, Wei Zhou, Linjian Mo, Ming Kong, Jie Liu, Feng Zhang, Qiang Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.04963`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答基于PDF检索证据（retrieved_evidence）和启发式草稿生成；部分字段（如实验观察、消融细节）因证据不足未做详细展开，建议回读原文确认具体数值和图表。
