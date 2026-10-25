---
user_id: "cheng tan"
paper_id: 13886
arxiv_id: "2609.30906"
title: "ToolSearcher: Optimizing Tool Selection at Scale via Reinforcement Learning"
publish_date: "2026-09-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.30906.pdf"
pdf_url: "https://arxiv.org/pdf/2609.30906.pdf"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-09T12:56:40"
---
# ToolSearcher: Optimizing Tool Selection at Scale via Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：tool selection · reinforcement learning · large-scale tool repository · multi-turn search

## 一句话总结

ToolSearcher 提出一个面向大规模工具库的强化学习框架，通过类别约束工具区分、事件级搜索建模和轨迹对齐信用分配，让 LLM 在多轮搜索与复杂工具组合场景中显著提升最终工具选择质量。

## 摘要

> Large language models (LLMs) excel at natural language processing but struggle
> to interact with external environments. Tool learning provides a promising way to
> extend LLMs into actionable agents, where tool selection is a critical prerequisite
> for successful tool use. Existing work often assumes a small or predefined set of
> tools, leaving large-scale tool selection underexplored. Real-world repositories
> contain a vast and diverse array of tools, making it difficult for LLMs to effec-
> tively search, distinguish, and compose tools under context-length constraints. We
> identify large-scale tool selection as a new challenge for agentic reinforcement
> learning, highlighting that existing RL methods for knowledge-based question
> answering are inadequate for selecting tools while considering compatibility. To
> address this challenge, we propose ToolSearcher, a novel RL framework for effec-
> tive multi-turn search and fine-grained optimization in large-scale tool selection.arXiv:2609.30906v1 Specifically, we introduce category-constrained tool discrimination to improve
> the model’s ability to distinguish functionally similar tools, event-level search
> modeling to explicitly optimize the discovery of target tools during multi-turn
> search, and trajectory-aligned credit allocation to provide fine-grained reward sig-
> nals for different stages of the search-selection process. Extensive experiments on
> large-scale tool selection benchmarks demonstrate that ToolSearcher consistently
> outperforms a set of strong baselines in challenging settings involving iterative
> search and complex tool composition. The code and the dataset are available at
> https://github.com/zhenlongDai/ToolSearcher.
> ∗Co-corresponding authors.
> 40th Conference on Neural Information Processing Systems (NeurIPS 2026).

Q1: 这篇论文试图解决什么问题？

这篇论文针对的核心问题是：在包含海量且功能高度多样化工具的大型工具库中，如何让 LLM 有效地搜索、区分并组合出满足用户需求的工具集合。具体分解如下：

1. 现有工具学习研究的主要空白：典型的工具学习流程包含工具选择、工具调用和响应生成三个阶段，但已有工作大多集中在工具调用（如参数规范、工具执行）上，工具选择这个前置关键环节被明显忽视。

2. 小工具集假设与现实脱节：现有研究常假设候选工具集是预定义的小集合，这与 PyPI、npm、HuggingFace 等生态中工具数量庞大、功能重叠的现实不符。上下文长度限制使 LLM 无法一次性读入所有工具文档，因此必须引入搜索环节。

3. 大规模工具选择带来的特有难点：
 - 搜索与决策需要协调：既要通过多轮搜索逐步缩小候选范围，又要在最终阶段从候选集中选出兼容性正确的工具组合，两类能力需要协同。
 - 功能相似工具难以区分：大规模仓库中大量工具功能近似，模型容易混淆。
 - 组合兼容性约束：最终选出的工具集必须满足接口约束（输入约束 I 和输出模式 O），工具之间需要相互兼容，而不仅仅是单个工具正确。
 - 多轮搜索的奖励稀疏：传统序列级奖励难以判断是哪一步搜索、哪一次选择导致了成功或失败。

4. 现有 RL 方法的不足：已有的基于 RL 的方法大多针对知识型问答，它们没有显式建模工具搜索过程，因此无法有效提升 LLM 对兼容工具组合的搜索能力。作者将大规模工具选择定位为“智能体强化学习的新挑战”，强调其与知识问答 RL 在目标函数和过程建模上有本质差异。

Q2: 有哪些相关研究？

根据摘要、引言和方法片段可以归纳的相关研究脉络如下：

1. 工具学习三阶段框架：论文引用已有工作 [29] 将工具学习分为工具选择、工具调用和响应生成三个阶段，并指出多数研究聚焦于工具调用阶段，例如函数参数生成、工具执行等；工具选择这个“关键前提”长期被低估。

2. 小候选集假设下的工具选择：近期研究 [27, 43, 36] 通常假设一组预定义或规模较小的候选工具，然后让模型从中挑选。这类设置回避了大规模检索和搜索问题，与真实世界差距明显。

3. 以搜索引擎作为外部组件的工具选择：论文指出当前研究 [32, 41] 已经开始利用搜索引擎作为工具选择流程的一部分，但现有 RL 方法并未显式建模工具搜索过程，限制了模型对兼容工具组合的搜索能力提升。

4. 大规模工具库的统计与生态：相关工作 [54, 10] 统计了主流软件和模型生态（PyPI、npm、HuggingFace）中已有工具的数量与功能多样性，从数据层面说明大规模工具选择的现实必要性。

5. 强化学习用于智能体任务：论文将工具选择与基于知识问答的 RL 方法进行比较，暗示已有 RL 方法（如面向问答的序列级奖励优化）主要优化最终答案的正确性，缺乏对搜索过程的细粒度建模。方法片段还提到 GSPO 这类可以按序列级似然进行优化的方法，说明作者关注如何在多轮 RL 中实现比序列级更精细的优势调整。

总体来看，相关研究脉络是从“小集合工具选择”走向“大规模搜索式工具选择”，从“工具调用优化”走向“工具选择与搜索过程的联合优化”，而 ToolSearcher 是第一个显式将大规模工具选择定义为独立 RL 挑战并设计专用过程奖励的工作（根据论文表述合理推断）。

Q3: 论文如何解决这个问题？

ToolSearcher 的解决方案围绕三个核心组件构建，并在一个统一的 RL 训练框架下协同工作：

1. 任务定义与符号约定（Section 3.1）：给定大型工具库 T 和用户需求 q，工具选择的目标是找到一组工具 Ts = {T1, T2, ..., Tn}，并满足条件概率最大化的形式化目标。每个工具带有接口规范，包括输入约束 I 和输出模式 O。模型 πθ 需要经过多轮搜索和最终决策来完成选择。

2. 类别约束工具区分（Category-Constrained Tool Discrimination，Section 3.3）：针对大规模工具库中功能相似工具难以区分的问题，设计类别约束机制。具体而言，在训练或推理中引入工具类别的信息作为约束，迫使模型在类别内做精细区分，而不是仅依赖表面描述。从 Figure 1(a) 可见，用户需求进入 LLM，LLM 与搜索引擎交互，工具文档从大规模工具库中被检索回来，类别约束作用于这一过程，帮助模型区分相似工具。

3. 基于 RL 的工具选择训练框架（Section 3.4）：
 - 事件级搜索建模（Event-Level Search Modeling，Section 3.4.1）：将多轮搜索过程拆分为离散的“搜索事件”，为每个事件定义是否首次检索到了目标工具等判定信号。通过事件级优势估计，模型可以显式学习“发现目标工具”这一行为，而不是只从最终结果获得奖励。Figure 1(b) 展示了搜索轨迹中奖励事件、首次搜索到的目标工具等概念。
 - 轨迹对齐信用分配（Trajectory-Aligned Credit Allocation，Section 3.4.2）：在搜索-选择全过程上对齐奖励与优化目标，使最终奖励的分配与每一步相对整体进度的贡献相匹配。论文强调在多轮 RL 中，比序列级更细粒度的优势调整是必要的；该机制为搜索阶段和最终选择阶段提供不同的细粒度奖励信号，避免将最终成功或失败归因到所有步骤。

4. 训练与推理细节（来自方法片段）：
 - 搜索阶段检索：采用 Qwen3-Embedding-0.6B 作为搜索引擎的检索器，该选择基于多轮方法的工具检索性能对比。
 - 每轮候选数量：多轮方法每轮检索结果限制为 5 条（top-5 per turn）。
 - 训练交互轮次：设置了最大交互轮次数（具体数值在证据片段中被截断，需查原文）。
 - 整体训练流程：LLM 与搜索引擎交互产生搜索轨迹，对轨迹进行事件级标注和奖励计算，然后通过 RL 更新策略。

该方案的核心思想是：把“搜索”和“选择”当作一个可端到端优化的决策过程，用事件级回报解决奖励稀疏问题，用类别约束解决相似工具混淆问题，用轨迹对齐解决过程信用分配问题。

Q4: 论文做了哪些实验？

根据摘要、方法片段和结论，可以确认的实验安排如下：

1. 基准数据集：
 - StableToolBench：大规模工具选择场景的主基准，包含多个测试场景。方法片段明确提到“StableToolBench 的三个场景”，并使用 SRecall 和 Recall 两个指标分别评估搜索阶段检索和最终阶段决策。
 - AppWorld：用于分布外（out-of-distribution）泛化评估，验证 ToolSearcher 在未见过的工具分布上的表现。

2. 评估指标：
 - SRecall：搜索阶段的检索成功率，用于衡量模型能否在搜索中召回目标工具。
 - Recall：最终工具选择的召回率，用于衡量最终决策是否正确覆盖了所需工具。
 - 论文通过这两个指标分离分析“搜索能力”和“选择能力”的平衡。

3. 基线对比：与多个 RL 基线方法比较，包括基于知识问答的 RL 方法和现有的工具选择方法。论文摘要称 ToolSearcher 在“迭代搜索和复杂工具组合”的挑战性设置中持续优于强基线。

4. 骨干模型变化：实验在多个不同 backbone LLM 上验证方法有效性（方法片段提到“不同 backbone LLM”），说明方法对模型规模的鲁棒性。

5. 检索器对比：在多轮方法中比较不同检索器，最终选择 Qwen3-Embedding-0.6B。每个多轮方法每轮检索 top-5。

6. 消融分析（合理推断）：论文设计了类别约束工具区分、事件级搜索建模、轨迹对齐信用分配三个组件，通常会有对应的消融实验来验证每个组件的贡献，但检索证据未直接给出消融表，需查原文确认。

7. 实验场景示例：方法片段特别提到场景 I3 上的结果，说明存在类似“I1/I2/I3”这样的难度分级场景，可能是按工具组合复杂度或搜索深度划分。具体场景定义未在摘要中给出，属于推断。

Q5: 发现了什么实验现象？

根据方法片段和启发式草稿中保留的证据，实验观察可以归纳为以下几点：

1. 搜索与选择的权衡现象：子分析显示，部分基线方法在提升搜索阶段 SRecall 的同时，反而损害了最终选择的 Recall。这揭示了一个重要的权衡：模型可能会“过度搜索”或把能力偏向于找到工具，但在最终组合与兼容性判断上变弱。

2. ToolSearcher 的平衡效果：实验表明 ToolSearcher 能有效平衡搜索与决策两种能力，在不同 backbone LLM 上都改善了最终工具选择性能。这说明事件级搜索建模和轨迹对齐信用分配共同作用，使得搜索能力的提升不会以牺牲最终选择为代价。

3. 场景 I3 的突出结果：在场景 I3 中，ToolSearcher 将最终选择 Recall 提升到了 26.91%，而对比方法在提升搜索表现的同时使基础模型的最终选择性能下降。这个观察表明，在更复杂的场景中，传统 RL 方法容易陷入“局部搜索成功”的陷阱，而 ToolSearcher 的细粒度奖励分配能防止这种偏科。

4. 多轮搜索的有效性：采用 top-5 检索的多轮交互设置下，ToolSearcher 表现出持续改进，说明显式优化搜索事件确实有利于在有限上下文条件下发现分散在不同类别中的工具。

5. 分布外泛化：在 AppWorld 上的结果证明了框架的泛化能力，说明学到的搜索策略和区分能力不是过拟合于 StableToolBench 的工具分布。（注释：这一条是依据结论中的陈述，具体数值未提供。）

6. 检索器选择的影响：实验对比后选用 Qwen3-Embedding-0.6B，说明检索器质量对多轮工具搜索有显著影响，但论文未展示具体对比数值。

7. 负结果/失败模式（推测）：由于工具文档噪声和功能重叠，模型可能在类别约束不充分时混淆相似工具，导致最终组合包含不兼容工具；论文未在可见片段中报告此类失败案例，需要查看实验部分确认。

Q6: 有什么可以进一步探索的点？

基于论文提出的问题框架和方案，可以进一步探索的方向包括：

1. 更大规模与动态工具库：将方法扩展到持续增长的真实生态（PyPI/npm/HuggingFace），研究工具新增、废弃、版本更新对搜索策略和 RL 训练的影响。

2. 更复杂的组合约束：当前目标是工具之间的输入输出兼容性；可以扩展到多工具工作流编排、并行调用、条件执行等更复杂的组合语义。

3. 检索器与策略的联合优化：当前使用固定的 Qwen3-Embedding 检索器；可以探索将检索器纳入 RL 训练，实现检索-选择的端到端联合学习，或者引入可学习的重排序模块。

4. 更细粒度的事件定义：事件级搜索建模中的“事件”目前大致对应一次搜索到目标工具；可以扩展为子目标分解、中间失败恢复、重复冗余搜索惩罚等更丰富的信号。

5. 多模态与跨语言工具：真实工具文档可能包含代码示例、多语言描述、表格等；研究如何让搜索与选择模型利用这些异质信息。

6. 推理时的自适应搜索预算：当前设置了最大交互轮次和每轮 top-k；可以学习动态决定何时停止搜索、何时转向最终选择，以平衡延迟与准确率。

7. 与其他 agent 能力的整合：将 ToolSearcher 与工具调用、参数生成、错误恢复、记忆管理等模块联合训练，形成完整 agent 系统，而不是孤立的工具选择组件。

8. 理论基础与信用分配的可解释性：轨迹对齐信用分配还比较启发式；可以研究基于因果推断或反事实分析的优势估计，给出更严谨的奖励分配理论。

9. 评测体系改进：现有 SRecall/Recall 二分指标可能掩盖部分匹配或兼容性程度；可设计考虑工具重要性、调用顺序、失败代价的细粒度评测指标。

10. 从工具选择推广到一般资源选择：该框架的思想可能适用于 API 选择、知识库检索、插件选择、数据集选择等更广义的“大规模候选集上的多轮搜索与决策”问题。

Q7: 总结一下论文的主要内容

这篇文章针对大规模工具库中的工具选择问题，提出并验证了一个名为 ToolSearcher 的强化学习框架。全文的论证主线、技术主线和实验主线可以概括如下：

一、论证主线：
论文从 LLM 无法直接与外部环境交互这一痛点出发，说明工具学习是让 LLM 成为 actionable agent 的关键路径。工具学习包含选择、调用、响应生成三个阶段，但已有研究过度聚焦于调用阶段，导致工具选择成为被忽视的瓶颈。现实中的工具仓库（PyPI、npm、HuggingFace）已经达到百万级且功能高度重叠，上下文长度限制决定了不可能把所有工具文档一次性喂给模型，搜索因此成为不可回避的前置操作。作者进一步指出，大规模工具选择不只是“检索+选择”的简单拼接，还需要考虑工具之间的兼容性（接口的输入约束和输出模式匹配），这使得它可以被看作一个全新的智能体强化学习挑战。已有的 RL 方法主要为知识问答设计，只优化最终答案，没有显式建模工具搜索过程，因而在不牺牲最终选择质量的前提下很难提升搜索能力。由此，论文提出需要专门的 RL 框架来联合优化搜索与选择。

二、技术主线：
ToolSearcher 由三个关键设计组成。第一是类别约束工具区分（Category-Constrained Tool Discrimination）：引入工具类别的约束信息，迫使 LLM 在功能相近的工具之间做精细差异判别，缓解大规模库中的混淆问题。第二是事件级搜索建模（Event-Level Search Modeling）：把多轮搜索轨迹切分为“搜索事件”，并为每个事件给出是否发现目标工具的奖励信号，从而显式地引导模型学习“如何找到目标工具”。第三是轨迹对齐信用分配（Trajectory-Aligned Credit Allocation）：在整条搜索-选择轨迹上对齐奖励与优化目标，为搜索阶段和最终决策阶段分配不同的细粒度优势估计，避免把最终结果的成功或失败粗糙地归因到所有步骤。在实现上，系统包含一个搜索引擎，选用了 Qwen3-Embedding-0.6B 作为检索器，多轮方法下每轮取回 top-5 个候选工具，并设置了最大交互轮次。训练过程让 LLM 在搜索轨迹上执行 RL，用事件级奖励更新策略。

三、实验主线：
论文在 StableToolBench 的多个场景和分布外基准 AppWorld 上进行实验。评估采用 SRecall 和 Recall 分别刻画搜索阶段的检索表现和最终阶段的决策表现，这一设计使得“搜索能力”和“选择能力”可以被分别观察。结果表明，ToolSearcher 在不同 backbone LLM 上都能有效平衡两种能力，并提升最终的工具选择 Recall。一个关键现象是：在某些对比方法中，搜索 SRecall 的提升伴随着最终 Recall 的下滑，说明这些方法把模型能力带偏了；而 ToolSearcher 通过事件级建模和轨迹对齐信用分配避免了这种偏移，在场景 I3 中将 Recall 提高到 26.91%。在分布外场景 AppWorld 上的表现进一步验证了方法的泛化性。综合来看，论文用清晰的组件划分和细粒度指标证明了：把大规模工具选择当作一个多轮搜索与决策联合的 RL 问题来建模，比沿用知识问答式 RL 或小集合选择方法更有效。

四、总体评价：
这是一项问题定义清晰、方案模块化、实验度量具有针对性的工作。它将长期被忽视的“工具选择”从工具学习流程中独立出来，并指出了搜索过程建模对最终组合质量的重要性。但可见的文本片段中没有给出消融数据、训练超参、计算开销等细节，因此对实现复现和进一步分析仍需要查阅论文全文。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向直接相关：论文把工具选择作为 LLM agent 的核心能力，并采用 RL 优化多轮搜索决策，适合关注 agent 训练方法的读者。

## 基本信息

- 作者：Zhenlong Dai, Xujie Song, Zitong Wang, Tong Niu, Jian liu, Weiqiang Wang, Xiu Tang, Sai Wu, Chang Yao, Jingyuan Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：high_relevant
- 日期：2026-09-28
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.30906`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 语义检索命中的 Introduction/Method/Conclusion 片段，并排除了 references 中无关的统计假设文本；部分实验细节和数值因证据不足以合理推断或缺口形式标注。
