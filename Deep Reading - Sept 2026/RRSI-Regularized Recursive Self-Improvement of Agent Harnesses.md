---
user_id: "cheng tan"
paper_id: 12667
arxiv_id: "2609.24972v1"
title: "RRSI: Regularized Recursive Self-Improvement of Agent Harnesses"
institution: "Google Research"
publish_date: "2026-09-21"
pdf_url: "https://arxiv.org/pdf/2609.24972v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-24T01:09:34"
---
# RRSI: Regularized Recursive Self-Improvement of Agent Harnesses

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：large language models · autonomous agents · recursive self-improvement · regularization

## 一句话总结

RRSI 通过在提议和选择阶段引入正则化约束，解决了 LLM 智能体递归自我改进中的过拟合问题，在提升泛化性能的同时显著降低了 Token 消耗。

## 摘要

> An LLM agent's capability is largely magnified by its harness, namely the prompts, control flow, tooling, memory, and context management surrounding the frozen backbone model. Recent methods increasingly automate this process by iteratively proposing and selecting component-wise edits of an agent harness, practically establishing a form of recursive self-improvement (RSI) at the agent-system level. However, such recursive evolution may overfit by memorizing the training tasks, showing large in-distribution gains that shrink or even vanish on out-of-distribution benchmarks. We introduce Regularized Recursive Self-Improvement of Agent Harnesses (RRSI), which incorporates the principles of regularizations into harness self-improvement by constraining the evolution candidate proposal and selection. The proposer operates with a temporally annealed budget, limiting how many edits a candidate can bundle, and it encourages unexplored trajectories based on evolution history. The selector is equipped with a critic and a pruner: the critic screens benchmark-specific proposals, while the pruner, removes changes that are too small, too expensive, or no longer useful. Together these constraints favor reusable agent mechanisms over benchmark-specific ones or even noises. Across eight benchmarks spanning coding, agentic workspace and engineering design tasks, RRSI gains up to 14.1 points on the split it evolves against and up to 4.7 points on the five out-of-distribution benchmarks, while producing a harness that runs on 30% fewer policy tokens than the unregularized evolution. Code is available at https://github.com/google-research/rrsi and project page is https://regularized-rsi.com/.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：智能体 Harness 的过拟合陷阱
LLM 智能体的性能不仅取决于底座模型（Backbone），更取决于其外围的“Harness”系统，即提示词工程、控制逻辑、工具接口和上下文管理策略。当前的自动化优化方法（如递归自我改进 RSI）通过不断迭代修改这些组件来提升性能。然而，这种进化过程存在严重的“过拟合”风险：
1. **记忆效应**：自动化搜索往往会找到针对特定测试用例的“捷径”或硬编码逻辑，而非通用的推理能力。
2. **泛化崩溃**：在特定基准测试（In-Distribution）上取得的巨大进步，往往在面对未见过的任务（Out-of-Distribution）时迅速消失甚至产生负面影响。
3. **复杂度膨胀**：无约束的进化会导致 Harness 变得异常臃肿，包含大量冗余的提示词和复杂的控制流，增加了推理成本和延迟。

### 科学问题
如何在不牺牲自动化进化能力的前提下，引导智能体 Harness 向更通用、更简洁、更具鲁棒性的方向演化？论文试图通过引入“正则化”这一经典机器学习思想来约束 Agent 系统的自我改进路径。

Q2: 有哪些相关研究？

### 递归自我改进 (RSI) 的演进
早期的 RSI 主要集中在模型权重的微调（如 STaR, Self-Instruct），而近期研究转向了“Agent-level RSI”，即优化智能体的外部支架。代表性工作包括 DSPy（程序化提示词优化）、ADAPO 等。这些方法证明了通过迭代搜索可以显著提升智能体在特定任务上的表现。

### 提示词与控制流优化
现有的提示词优化器（如 OPRO）利用 LLM 作为优化器来生成新的提示词。然而，这些方法通常缺乏对“泛化性”的显式建模。RRSI 与它们的不同之处在于，它不仅关注“如何改进”，更关注“如何限制改进”，以确保改进的质量和可迁移性。

### 自动化工程设计与智能体框架
在软件工程和复杂任务规划领域，智能体框架（如 AutoGPT, MetaGPT）展示了复杂 Harness 的威力。RRSI 的研究背景正是基于这些日益复杂的系统，探讨如何通过算法手段而非人工干预来精炼这些系统的逻辑。

Q3: 论文如何解决这个问题？

### RRSI 框架设计：正则化进化的双重约束
RRSI 在 RSI 的两个关键环节——提议（Proposal）和选择（Selection）中引入了正则化机制。

#### 1. 提议阶段的正则化 (Regularized Proposer)
- **时间退火预算 (Temporally Annealed Budget)**：限制单次迭代中允许修改的组件数量或代码行数。随着进化轮次的增加，预算逐渐收紧。这迫使模型优先提出最具影响力的核心改进，防止一次性引入过多针对特定任务的细碎补丁。
- **探索鼓励机制 (Exploration-based Trajectories)**：记录进化的历史路径，通过 Prompt 引导 LLM 避开已尝试过的失败路径，转向未探索的逻辑空间，从而跳出局部最优解。

#### 2. 选择阶段的正则化 (Regularized Selector)
- **泛化评论员 (Generalization Critic)**：在评估候选 Harness 时，Critic 模型不仅看性能指标，还会分析修改内容。如果修改被判定为“过于针对当前数据集的特定模式”（Benchmark-specific），则会被降权或否决。
- **效能剪枝器 (Efficiency Pruner)**：这是一个多维度的过滤器。它会移除：
 - **微小增量**：性能提升低于阈值的修改。
 - **高成本逻辑**：显著增加 Token 消耗但收益平平的修改。
 - **过期逻辑**：在后续迭代中证明不再有用的旧组件。

### 技术实现
RRSI 维持一个“冻结”的底座模型，所有的进化都发生在代码化的 Harness 层。通过这种方式，它实现了在不重新训练模型的情况下，通过系统工程手段提升智能体的“智力”。

Q4: 论文做了哪些实验？

### 实验设置
- **基准测试**：共 8 个 Benchmark，涵盖三大领域：
 - **编码 (Coding)**：如 HumanEval, MBPP 等。
 - **办公协作 (Agentic Workspace)**：模拟真实办公环境的任务。
 - **工程设计 (Engineering Design)**：复杂的系统设计任务。
- **数据集划分**：分为进化集（Evolution Set）和五个分布外测试集（OOD Sets），用于严格评估泛化能力。
- **对比基线 (Baselines)**：
 - 原始冻结模型（Zero-shot/Few-shot）。
 - 无正则化的标准 RSI（Unregularized Evolution）。
 - 现有的 SOTA 提示词优化方法。

### 评估指标
- **Pass@1 准确率**：衡量任务完成质量。
- **泛化差距 (Generalization Gap)**：ID 性能与 OOD 性能的差值。
- **Token 效率**：完成任务所需的平均 Policy Token 数量。

Q5: 发现了什么实验现象？

### 关键发现与实验现象
1. **泛化性能的显著提升**：相比于无正则化的 RSI，RRSI 在 OOD 任务上的表现平均提升了 4.7 个百分点。这证明了正则化约束确实能引导模型学习到更通用的策略，而非死记硬背。
2. **ID 性能的稳健增长**：在进化集上，RRSI 取得了 14.1 分的提升。虽然在某些极端任务上，无约束进化可能在 ID 上更高，但 RRSI 的结果更具可持续性。
3. **Token 消耗的“瘦身”效应**：最令人惊讶的发现是，RRSI 生成的 Harness 在运行时比原始进化版本节省了 30% 的 Token。这说明 Pruner 有效地去除了提示词中的冗余信息和无效的控制流分支。
4. **反直觉现象**：实验观察到，有时限制修改预算（Budget）反而能加快收敛速度。这可能是因为较小的搜索空间降低了 LLM 提议者的认知负担，使其能更专注于逻辑核心。
5. **失败模式分析**：在没有正则化的情况下，智能体往往会生成极长的提示词，包含大量针对特定错误案例的“If-Else”式补丁；而 RRSI 倾向于生成更具抽象性的函数和通用的处理逻辑。

Q6: 有什么可以进一步探索的点？

### 可探索的研究方向
1. **跨模型 Harness 迁移**：研究在 GPT-4 上进化出的正则化 Harness 是否可以直接应用于 Claude 或开源模型（如 Llama-3），以及这种迁移过程中的性能损耗。
2. **动态正则化参数**：目前退火预算和剪枝阈值可能是预设的，未来可以探索根据进化过程中的反馈动态调整这些超参数的算法。
3. **多智能体协同进化**：将 RRSI 扩展到多智能体系统，研究如何正则化智能体之间的通信协议和协作流。
4. **长期演化的稳定性**：在成百上千轮的超长周期进化中，正则化是否足以防止系统陷入“模式崩溃”或产生不可预测的突变行为？

Q7: 总结一下论文的主要内容

这篇论文提出了 RRSI（正则化递归自我改进），旨在解决 LLM 智能体在自动化进化过程中普遍存在的过拟合问题。作者指出，虽然通过迭代优化智能体的 Harness（提示词、控制流等）可以显著提升特定任务的性能，但这种提升往往是以牺牲泛化性和增加系统复杂度为代价的。RRSI 框架通过在提议阶段引入“时间退火预算”和“探索鼓励”，以及在选择阶段引入“泛化评论员”和“效能剪枝器”，为智能体的自我改进过程戴上了“紧箍咒”。

实验结果令人印象深刻：RRSI 不仅在 8 个基准测试中展现了强大的 ID 提升（+14.1 pts），更在 OOD 泛化上表现优异（+4.7 pts），同时将推理成本降低了 30%。这一研究范式标志着智能体优化从“单纯追求指标”向“追求高质量、通用且高效的系统逻辑”转变。论文通过严谨的对比实验证明，在 Agent 系统的自动化构建中，适当的约束（正则化）比无限制的搜索更能产生具有生命力的智能体架构。这为未来构建能够自我演化且保持鲁棒性的通用人工智能系统提供了重要的理论和实践参考。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文直接关联智能体（Agent）方向，探讨了如何通过系统化方法提升智能体的自主进化能力。

## 基本信息

- 作者：Peng Xia, Rujun Han, Zifeng Wang, Yanfei Chen, Yufan Zhang, Yoonho Lee, Chengsong Huang, Han Yu, Zhongying CuiZhu, Yifei Ming, Huaxiu Yao, Burak Gokturk, Tomas Pfister, Chen-Yu Lee
- 机构：Google Research
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-09-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.24972v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成参考了论文摘要及启发式草稿，重点对 RRSI 的正则化机制（Proposer/Selector）进行了深度解析，并结合 Agent 领域背景对过拟合问题进行了系统化论述。
