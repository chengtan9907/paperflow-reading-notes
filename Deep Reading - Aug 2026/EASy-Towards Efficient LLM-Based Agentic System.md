---
user_id: "cheng tan"
paper_id: 6710
arxiv_id: "2608.04588"
title: "EASy: Towards Efficient LLM-Based Agentic System"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.04588.pdf"
pdf_url: "https://arxiv.org/pdf/2608.04588"
abs_url: "https://arxiv.org/abs/2608.04588"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-06T11:18:36"
---
# EASy: Towards Efficient LLM-Based Agentic System

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic system · reinforcement learning · llm orchestration · efficiency optimization

## 一句话总结

EASY 是一个通过强化学习联合优化任务性能与计算效率的可训练 agentic 框架，让 orchestrator 感知执行器能力-成本画像并采用里程碑-计划-执行工作流，在多个基准上实现更强的性能-效率权衡。

## 摘要

> Agentic systems have emerged as a promising paradigm for solving complex tasks by coordinating specialized LLM-based agents. However, most existing systems primarily optimize task success while giving limited consideration to execution efficiency under practical constraints such as executor capability and computational cost. Existing router-based methods have limited ability to reason over rich, evolving task contexts, multi-step dependencies, and intermediate execution feedback, and often generalize poorly to unseen executors. We propose EASY, a trainable agentic framework that jointly optimizes task performance and computational efficiency through reinforcement learning. EASY equips an LLM-based orchestrator with explicit knowledge of the capability and cost profiles of heterogeneous executors, enabling context-sensitive coordination beyond performance-only routing. It further introduces a milestone-plan-act workflow that decomposes complex tasks into manageable milestones, constructs dependency-aware execution graphs, assigns suitable executors, and parallelizes independent steps while adapting subsequent decisions to intermediate outcomes. To train the orchestrator, we develop a tree-structured rollout procedure that explores alternative milestone decompositions and execution plans, together with multi-component rewards that capture task correctness, execution efficiency, and trajectory completeness. Extensive experiments on mathematical reasoning, embodied decision-making, and deep research benchmarks show that EASY consistently achieves stronger performance-efficiency trade-offs than strong agentic baselines.

Q1: 这篇论文试图解决什么问题？

本论文要解决的问题是：在 agentic 系统中，如何让一个协调器（orchestrator）在真实约束下同时优化任务成功率和执行效率。具体拆分为两个子问题：
1. 性能与效率的失衡：现有 agentic 系统（多个专用 LLM agent 协作）几乎只以任务成功率作为优化目标，忽视了实际部署中执行器能力差异和计算成本（如 API 价格、延迟、token 消耗）。这导致系统在简单任务上可能使用昂贵大模型，或把需要专业能力的子任务分配给弱执行器，造成资源浪费或性能不足。
2. Router 方法的表达力局限：常见的 router-based 方法用轻量预测模型在候选 agent 之间做选择。这种模型容量低，无法对丰富的、动态演化的任务上下文进行推理；也难以建模多步依赖（如后续子任务依赖前面输出）和利用中间执行反馈；当执行器集合变化（出现未见过的执行器）时泛化能力差。
3. 优化目标与训练信号问题（合理推断）：要从经验中学习协调策略，需要合适的可扩展训练信号。单纯的任务级奖励稀疏且不反映效率，需要设计能权衡正确性和效率的奖励，以及能探索不同规划和分解的采样过程。
因此，论文目标是构建一个可训练的框架，使 orchestrator 学习如何根据任务上下文、执行器能力-成本画像和中间反馈，动态分解任务、构建依赖图、分配执行器并决定并行度，在任务性能与计算成本之间取得更好权衡。

Q2: 有哪些相关研究？

由于没有检索到完整的 Related Work 章节，以下基于摘要和常识进行合理推断，具体文献请以论文原文为准：
1. Agentic 系统 / 多智能体协作：大量研究让多个 LLM agent 分工协作（如规划、工具调用、多轮交互）来解决复杂任务；典型思路包括角色分工、动态任务分配和反思循环。EASY 属于这一类，但强调协调层的可训练性。
2. Router-based 模型选择：现有方法利用 router 或 gating 网络在多个模型/agent 间选择（例如模型级联、成本感知路由），通常使用轻量分类器或嵌入匹配。论文明确指出这些方法对上下文推理和多步依赖建模不足，是 EASY 的主要对比对象。
3. 任务分解与规划：Plan-and-Execute、least-to-most、hierarchical planning 等将任务分解为子任务并按依赖图执行。EASY 的 milestone-plan-act 工作流与这些方法相关，但将分解和分配统一到一个可训练的 orchestrator 中。
4. 强化学习与语言智能体：RLHF、过程奖励模型（PRM）、tool-use agent 的训练、可微搜索等被用于优化 agent 策略。EASY 使用树结构 rollout 和多组件奖励，属于 RL 训练 agentic 系统这一线。
5. 性能-成本权衡：模型级联（cascade）、投机执行、early exit 等关注推理成本与质量；EASY 将此问题提升到任务规划层面，通过选择执行器和并行度来控制成本。

Q3: 论文如何解决这个问题？

EASY 的解决方案由三个主要组件构成：
1. 能力-成本感知的 orchestrator：用一个 LLM 作为 orchestrator（协调器），并显式地给它提供异构执行器的能力画像（capability profile）和成本画像（cost profile）。这意味着 orchestrator 不再只是按任务类型路由，而是能根据当前子任务难度、执行器擅长领域和调用成本做出上下文敏感的分配决策。
2. 里程碑-计划-执行（Milestone-Plan-Act）工作流：
 - 里程碑分解：将复杂任务分解为可管理的里程碑（milestones），而不是细粒度的原子步骤；这可以在保持语义完整性的同时减少规划开销（合理推断）。
 - 依赖感知执行图：为里程碑构建依赖关系图（DAG），从而识别哪些步骤可以并行执行。
 - 执行器分配：根据里程碑特性和执行器画像分配合适的执行器。
 - 并行执行：对图中无依赖的独立步骤并行执行，缩短端到端时间。
 - 自适应调整：根据中间执行反馈/结果，动态修改后续里程碑或执行计划，而不是固定计划执行到底。
3. 训练策略：
 - 树结构 rollout（tree-structured rollout）：在训练时从当前状态展开一棵决策树，探索不同的里程碑分解和执行计划（类似 MCTS 的思路），为 orchestrator 提供多样的轨迹样本，从而提高策略的鲁棒性和可靠性。
 - 多组件奖励：奖励函数包含三部分——任务正确性（最终答案质量）、执行效率（如成本、延迟、步数）和轨迹完整性（如是否完成所有里程碑、是否出现无效循环）。通过加权组合，引导策略同时优化性能与效率。
整体上，EASY 将 orchestrator 作为一个可训练的强化学习策略，利用成本感知的树搜索和奖励信号来学习跨任务泛化的协调行为。

Q4: 论文做了哪些实验？

根据摘要和检索片段，论文实验设置包括：
1. 基准任务：数学推理（mathematical reasoning）、具身决策（embodied decision-making）、深度研究（deep research）三个 benchmark，覆盖了不同类型的复杂任务和多步规划场景。
2. 对比基线：与“强 agentic 基线”比较，具体包括哪些方法未在摘要中出现；从上下文推断可能包含通用 agent 框架、router-based 方法等。
3. 评估指标：性能-效率权衡，另有单独的“效率得分”（efficiency score）指标（如 Figure 4 所示），可能综合了任务成功率和计算成本。
4. 消融研究（来自启发式草稿）：验证了里程碑-计划-执行工作流、树结构 rollout、奖励设计和泛化能力等组件对整体结果的贡献。
5. 执行器规模实验（检索片段 4_2_main_results）：将执行器从 Qwen2.5-7B-Instruct 替换为 GPT-5-mini，观察所有方法的绝对性能提升，并验证 EASY 是否保持领先。
6. 跨分支和跨任务的泛化实验（检索片段“oss branches and tasks”）：评估训练的 orchestrator 在新分支/新任务上的表现。
注意：具体数据集名称、基线的精确配置和数值结果在提供的材料中未出现，需要查看原文 Table/Figure。

Q5: 发现了什么实验现象？

从检索到的结果片段可以归纳出以下实验现象：
1. 一致的效率优势：Figure 4 显示 EASY 在每一个评估 benchmark 上都取得比所有基线更高的效率得分，说明其学习到的协调策略能显著降低执行成本或提升资源利用，而不是以牺牲性能为代价。
2. 随更强执行器扩展：将执行器从 Qwen2.5-7B-Instruct 替换为 GPT-5-mini 后，所有方法的绝对性能都提升，但 EASY 仍然保持最佳结果。这表明 EASY 的 orchestrator 不是针对特定弱执行器过拟合，而是能从更强执行器中获益，具有可扩展性。
3. 消融贡献（来源于启发式草稿，属于论文报告）：各组件（milestone-plan-act 工作流、树结构 rollout、奖励设计、泛化机制）都对最终结果有实质贡献，暗示这些设计在协同工作。
4. 泛化性：在跨分支和跨任务设置下 EASY 表现出稳健性（依据检索片段推测），但缺少细节。
需要说明：我们无法从检索片段中获得具体数值（如提升百分比、成本降低倍数），以上观察为定性结论。

Q6: 有什么可以进一步探索的点？

基于论文提出的框架和问题，可以进一步探索的方向包括：
1. 更丰富的执行器画像：引入延迟、显存、能耗等多维成本，以及执行器能力的不确定性（如置信度），增强 orchestrator 的决策依据。
2. 动态执行器环境：在训练和执行时执行器集合动态变化（如新增 API 模型、模型下线），研究 orchestrator 如何快速适应未见过的执行器，进一步泛化。
3. 更复杂的任务依赖：处理非 DAG 的依赖（如循环、条件分支、需要回滚的执行路径），或将里程碑粒度自适应调整。
4. 奖励设计优化：研究如何更细粒度地估计中间步骤的正确性和效率，例如使用过程奖励模型来避免追踪不完全奖励；或引入多目标约束优化（指定成本预算下最大化性能）。
5. 扩展到科学应用（与用户画像中的 ai-for-science 相关）：将 EASY 应用于科学发现中的多 agent 协作（如实验设计、文献综述、数据管道），评估其在长周期、多模态、高不确定性任务上的表现。
6. 与外部规划器和工具集成：将里程碑分解与现有 planner（如 PDDL 导致）或 Toolformer 类方法结合，进一步减少规划错误。
7. 离线到在线训练：研究在真实部署中持续学习或在线更新 orchestrator，利用更多真实反馈数据改进策略。

Q7: 总结一下论文的主要内容

本文提出 EASY（Efficient Agentic SYstem），一个可训练的 agentic 框架，用于在现实约束下联合优化任务性能和计算效率。论文的论证主线是：现有 agentic 系统主要追求任务成功率，而真实部署中执行器能力差异和计算成本不可忽略；同时，基于路由的轻量方法无法对丰富演化上下文、多步依赖和中间反馈进行推理，且对未知执行器泛化差。因此需要一种可训练的、能力-成本感知的 orchestrator。技术主线上，EASY 将 orchestrator 作为一个 LLM 策略，显式输入执行器的能力画像和成本画像；并设计了里程碑-计划-执行工作流：先解耦里程碑，再构建依赖感知执行图，分配执行器，并行执行独立步骤，并在中间反馈的基础上动态调整后续计划。训练时，作者采用树结构 rollout 来探索不同的里程碑分解和执行计划，配合由任务正确性、执行效率和轨迹完整性组成的三组件奖励。这个训练框架提供了可扩展的监督，学习可靠且低成本的协调策略。实验主线上，论文在数学推理、具身决策和深度研究三个 benchmark 上与强 agentic 基线比较，报告 EASY 在性能-效率权衡上持续领先；进一步分析和消融验证了各个组件的贡献，以及 orchestrator 对更强执行器（Qwen2.5-7B-Instruct 到 GPT-5-mini）的适应性和跨分支/跨任务的泛化性。整体上，EASY 展示了通过强化学习自动学习 agent orchestration 的可能性，为构建性能-成本双优的 agentic 系统提供了一个有前景的范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的智能体（agent）方向直接重合：论文正是关于 LLM 基 agentic 系统的协调与效率优化。

## 基本信息

- 作者：Junnan Liu, Linhao Luo, Thuy-Trang Vu, Gholamreza Haffari
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.04588`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的证据片段（Abstract、Introduction、Conclusion、Main Results），并结合启发式草稿进行补全；部分内容为基于摘要的合理推断。
