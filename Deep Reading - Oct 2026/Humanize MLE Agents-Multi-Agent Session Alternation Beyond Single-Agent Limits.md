---
user_id: "cheng tan"
paper_id: 15433
arxiv_id: "2610.13494"
title: "Humanize MLE Agents: Multi-Agent Session Alternation Beyond Single-Agent Limits"
publish_date: "2026-10-12"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.13494.pdf"
pdf_url: "https://arxiv.org/pdf/2610.13494v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T13:12:20"
---
# Humanize MLE Agents: Multi-Agent Session Alternation Beyond Single-Agent Limits

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：mle agent · multi-agent workflow · session alternation · context reset

## 一句话总结

本文提出Humanize MLE Agents（HMA），一种不修改任何原生模型-智能体对、仅在共享工作区上交替两个智能体并在每次交接时重置上下文的工作流级方法；通过将其建模为上下文重置半马尔可夫决策过程（CR-SMDP），理论推导了交替超越单智能体参考极限的条件与实验预算，并在MLE-bench的75个Kaggle任务上以四分之一时间达到78.2%的any-medal率，同时展现出跨异构智能体对的通用性。

## 摘要

> Existing LLM-based agent approaches for long-horizon machine learning engineering (MLE) often use context engineering, implemented through custom harnesses that manage tools, skills, and accumulated experience. However, we find that as single-session execution lengthens, the agent's reasoning intensity declines alongside slowing progress, summarized as long-horizon deceleration, and the benefits of these custom harnesses vanish as the underlying model improves, summarized as harness staleness. These findings suggest that their benefits may not persist across versions, which motivates a workflow-level approach in which agents build on one another's work across sessions. We model these alternating agent sessions as a context-reset semi-Markov decision process (CR-SMDP) and establish sufficient conditions for alternation to exceed single-agent reference limits, derive an explicit experiment-budget bound, and characterize when session caps preserve this advantage. Guided by this analysis, we introduce Humanize MLE Agents (HMA), which alternates two agents on a shared workspace, limits each session to at most k experiments and resets the context between sessions, without additional task-specific harness or prior knowledge supplements. Across 75 Kaggle tasks in MLE-bench, HMA reaches a state-of-the-art any-medal rate of 78.2%, exceeding the strongest open-source baseline by 8.0 points with a quarter of its time budget. Moreover, among six configurations spanning five heterogeneous agent pairs, HMA improves any-medal rate over the constituent baselines by 6.4 points on average, demonstrating cross-pair generality. A supplementary case on kernel optimization reaches 878 cycles, within 1.6% of the community leaderboard leader, demonstrating applicability beyond MLE.

Q1: 这篇论文试图解决什么问题？

本论文聚焦于长期机器学习工程（MLE）任务中的自主智能体设计，其核心问题是：现有依赖上下文工程（context engineering）和自定义harness的LLM智能体方法，虽然能在单次会话内提升性能，但存在两个随时间和模型演化而出现的失效模式，导致其收益不可持续。

第一个失效模式是长时程减速（long-horizon deceleration）：当单一会话不断延长，智能体的推理强度（以每次响应的平均输出token等指标衡量）逐渐下降，任务进度也变慢。这说明让一个智能体在同一个上下文窗口中长时间连续运行，并不能保持稳定的探索和优化能力。第二个失效模式是harness陈旧性（harness staleness）：当底层模型版本提升后，定制harness带来的相对收益会显著缩小甚至消失。也就是说，这些harness往往针对特定模型行为手工设计，无法跨版本迁移，随着模型能力的自然演进，其附加价值被稀释。

这两个发现共同指向一个结论：在单个agent内部堆砌上下文工程和工具管理，可能是在错误层面解决问题。作者因此将视角从“如何改进一个agent在一个会话内的行为”转向“如何组织多个完整agent的会话序列”，即在不受单会话长度限制的工作流层面寻求增益。

具体而言，论文尝试回答以下设计问题：
1) 两个不同（或相同）的、保持原生模型-harness对的智能体，在共享工作区上交替工作，每次交接时重置上下文，能否在给定实验预算下超过任意一个agent单独运行所能达到的性能？
2) 如果能够超过，其充分条件是什么？是否存在明确的实验预算界限？
3) 当每个会话被限制为最多k个实验（即固定上限中断）时，交替的优势是否仍然保持？若保持，k应如何选取？
4) 这种工作流级的协作是否不依赖任务特定harness或先验知识，并能跨不同模型组合、跨任务类型泛化？

现有方法的共同缺陷在于它们修改的是单个agent在其harness内的行动方式，无论搜索、反思、记忆、分支重用、可恢复工作空间，还是在任务条件接口中调整工具和技能管理，都没有改变“一个连续会话”的根本结构。因此，这些方法本质上都受限于单一会话的上下文演化。此外，之前的多agent工作流研究，如经验复用、反射、树搜索、工作流搜索、通信、角色分配、检查点或独立评审，大多关注如何在同一上下文内或通过外部记忆维持连续性，而最近的handoff研究则关注候选转移、控制交换以及延续另一模型轨迹的质量代价，但很少采用“固定上限会话+强制上下文重置”这种简单而可分析的机制。本文的独特之处在于不修改任何原生模型-harness对，仅通过工作流策略协调两个完整agent，从而使得任何模型进步都可以直接转化为HMA的优势，而不需要重新设计harness。

论文以数学建模（上下文重置半马尔可夫决策过程）和大量实验（MLE-bench 75个Kaggle任务）来回答这些问题，同时通过补充案例（AOPT内核优化）验证其适用范围不限于MLE-bench。

Q2: 有哪些相关研究？

论文涉及三个相关研究脉络。

第一，MLE智能体与harness级系统。MLE-bench（Chan et al., 2025）以Kaggle竞赛为场景评测自主智能体。近期大量系统通过搜索、反思、记忆、分支重用、可恢复工作空间或任务条件接口来增强单个agent（Toledo et al. 2025；Chen et al. 2026；Du et al. 2026；Zhu et al. 2026；Zhao et al. 2026；Yu et al. 2026；Zhang et al. 2026；Ye et al. 2026）。这些方法共同的特点是修改一个模型在其harness内部的行为方式，属于harness级改进。HMA与它们互补：HMA保持每个原生模型-harness对不变，只在工作流层协调完整agent。

第二，多智能体与长时程工作流。已有工作通过可复用经验、反思（如Reflexion，Shinn et al. 2023）、树搜索、工作流搜索、通信、角色分配、检查点或独立评审来维持长时程执行（Zhao et al. 2024；Zhou et al. 2024；Zhang et al. 2025；Wu et al. 2024；Qian et al. 2024；Li et al. 2025）。近年来的handoff研究考察候选迁移、学习控制交接和继续另一模型轨迹的质量成本（Luo et al. 2026；Li et al. 2026；Ganz et al. 2026）。HMA在该脉络中的定位是：对两个完整的原生agent在同一个共享工作区上进行固定上限的交替，每次交接都重置上下文。这与依赖外部记忆或跨轨迹延续的方法不同，因为它明确丢弃了上一会话的对话上下文，只保留工作区和已积累的结果。

第三，选项与半马尔可夫决策过程。半马尔可夫决策过程（SMDP）允许动作具有可变持续时间（Bradtke & Duff 1994；Dietterich 2000），选项（option）将此类动作定义为内部策略+终止规则（Sutton et al. 1999）。中断定理刻画了提前终止选项何时保持或提升价值。本文的CR-SMDP将每个agent会话建模为一个选项，并以固定上限k作为中断规则，同时加入上下文重置这一特殊状态转移结构，从而推导交替优越性的充分条件和预算界限。该理论贡献将工作流设计与经典序贯决策理论联系起来。

Q3: 论文如何解决这个问题？

HMA的核心思想是：不改变任何单个agent的内部设计，而是在工作流层面将两个完整的native agent（A和B）交替安排在同一个共享工作区上。每个agent会话被限定为最多k个实验，会话结束时，当前agent的上下文被完全重置，但共享工作区W和累计结果E保持不变；随后另一个agent从该状态继续。

1) CR-SMDP形式化
作者将整个工作流建模为上下文重置半马尔可夫决策过程（CR-SMDP）。工作流由一个固定策略μ控制，该策略在所有会话边界处选择下一个选项。状态为x=(W,E,c(u))，其中W是共享工作区，E是保留的结果，c(u)是当前活动agent u∈{A,B}的上下文。第i个会话由agent u执行，建模为选项o(u)_i=(I(u)_i, π(u)_i, β(u)_i)，其中I为发起集，π为内部策略，β为终止规则。终止产生选项边界，μ在此选择下一个选项。每个原始步骤完成一个实验并更新(W,E)。初始状态x0=(W0,E0,∅)来自任务状态分布ν0，E0为空，上下文为空。质量q(x)=q(E)∈[0,1]，定义期望质量Perf_μ(N)=E[q(E_N)]和目标成功概率p_μ(N;τ)=Pr(q(E_N)≥τ)。

2) 交替优势的条件与实验预算
论文推导了交替超过单agent参考极限的充分条件。检索到的Assumption 3.2（Cooperative access and use）指出：固定μ_k和τ∈(U*,1]时，在循环m中，agents u和ū分别从x_{2m}和x_{2m+1}出发，存在常数α,β∈(0,1]使得……（细节见原文）。其含义是，每个agent能够在会话中有效地利用前一个agent留下的工作区状态，并且这种协作式访问和利用的强度可以用α、β量化。在这些条件下，可以得出交替策略的期望质量或成功概率超过任何单一agent参考上限的充分条件，并给出显式的实验预算界（即需要多少实验预算才能保证超越）。

3) 固定k中断与会话上限选择
因为实际应用中会设置每个会话最多k个实验（固定上限中断），作者分析了在固定k中断下交替优势是否仍然保持。中断定理指出提前终止选项有时能保持甚至提升性能；本文将该定理推广到CR-SMDP，刻画了保证优势成立的条件。k的选择被视为一种经验超参数，论文通过实验分析其影响，并在附录中提供更多细节。

4) 完整工作流
HMA的流程如下：
- 在共享工作区初始化任务（如Kaggle竞赛的环境、数据、代码、README等）。
- Agent A开始会话1，在上下文c(A)中运行，执行最多k个实验；每次实验后更新工作区和结果E。
- 当达到k个实验或某个终止条件时，会话结束；此时保存工作区和E，丢弃上下文c(A)。
- Agent B在新的环境中开始会话2，读取共享工作区（包括A留下的所有文件、结果、错误日志等），但上下文从头开始。
- 如此交替，直到总实验预算N耗尽或达到目标质量。
该方法不添加任务特定harness，也不注入先验知识；两个agent可以来自不同模型家族（如GPT和Opus），也可以相同。所有代码、数据、中间结果都在共享文件系统中保留，而对话历史强制清空。

为什么有效？作者认为，上下文重置可以缓解长时程执行中上下文膨胀和注意力稀释带来的推理强度下降；每个会话开始时模型处于“清醒”状态，能够重新审视工作区中的客观证据；而共享工作区则保证信息在会话间无损传递，任务状态不会因上下文重置而丢失。交替还带来“双视角”效果：不同模型的偏好和错误模式不同，后一个agent可能发现前一个agent的盲点。

Q4: 论文做了哪些实验？

实验部分在MLE-bench上进行，覆盖75个Kaggle竞赛任务。MLE-bench是当前评估自主机器学习工程智能体的标准基准。作者测试了六种配置，这些配置由五种异构agent对构成，即使用不同的底层模型组合（例如GPT系列和Opus系列，从图注可见“Blue and purple regions identify GPT and Opus”）。

主要实验内容：
1) 主对比：将HMA与最强开源基线进行比较。在同等或更少预算下，HMA的any-medal率达到78.2%，超过最强开源基线8.0个百分点，并且只使用该基线四分之一的时间预算。这里“any-medal率”指在竞赛中获得任意奖牌（金/银/铜）的任务比例。
2) 跨配置通用性：在六种配置中，HMA相对于其构成基线（即单独运行每个agent）的any-medal率平均提升6.4个百分点，证明交替并不依赖于特定模型组合。
3) 案例分析：对INGV火山喷发预测任务进行深入分析。数据显示，第一个Opus会话结束时MAE为41,214；经过后续交替执行，MAE降至34,234，相对改进16.9%，并且比六个构成agent单独运行（goal runs）中的最佳MAE（63,991）低46.5%。这表明交替不仅超越了单个agent，还超越了所有构成agent独立完成最好成绩的聚合。
4) 补充案例：AOPT内核优化任务（超越MLE-bench）中，HMA在48小时运行中达到878 cycles，与社区排行榜领先者的差距在1.6%以内，说明该方法适用于非Kaggle特性的迭代优化问题。
5) 附带的图（Figure）展示了响应输出token均值随实验进程变化的轨迹，显示交接点（dotted lines mark handoffs）后token数回升，反映上下文重置恢复了模型输出强度。

论文还包含额外的分析，如聚合规则、响应与性能轨迹、匹配的构成权重、任务级比较，以及起始顺序效应、模型对效应等，详见附录D/E/F。但本次检索证据未提供这些附录的具体数值，因此这里不展开。

Q5: 发现了什么实验现象？

实验中揭示的核心现象如下。

1) 长时程减速的直接证据：在单个agent连续会话中，每次响应的平均输出token数量随实验进行而下降，同时任务进度放缓。图中蓝色和紫色区域分别标识GPT和Opus类模型，交接点用虚线标出，从曲线可以看出，在每次handoff之后，输出token数有回升迹象。这说明上下文重置能够恢复agent的“输出强度”，而持续在同一上下文中运行会导致生成越来越简短、甚至退化。

2) harness陈旧性的证据：作者指出，随着基础模型变强，定制harness的相对收益消失。这意味着harness级方法的效果与特定模型版本绑定；而HMA不依赖任何harness，因此其收益来自工作流结构，不易被模型升级侵蚀。

3) 交替超越单agent的定量现象：在INGV案例中，第一个Opus会话结束时MAE为41,214，看似已经较好；但交替后进一步降至34,234（改进16.9%），并且该最终MAE比六个构成模型单独运行的最佳值63,991低46.5%。尤其值得注意的是，六个构成goal runs的最佳MAE远高于第一个会话的MAE，这暗示单agent长时间执行不仅不能继续优化，反而可能陷入较差状态；而重置上下文并让另一个agent接手，能够跳出局部停滞。

4) 时间预算效率：HMA达到78.2% any-medal率所花时间仅为最强开源基线（达到70.2%左右）的四分之一。也就是说，不仅最终分数更高，而且收敛速度更快。这消除了“多agent交替只是消耗更多算力”的疑虑。

5) 跨配置稳健性：六种配置、五种异构agent对均显示HMA优于各自的构成基线，平均提升6.4个百分点，说明该现象不是某个模型组合的特例。

6) 跨任务泛化：AOPT内核优化案例达到878 cycles，距离社区排行榜第一仅1.6%，证明交替机制在标准的模型选择/超参优化之外，对非MLE的迭代优化任务也有效。

7) 需要注意的是，检索证据中没有提供消融实验的完整数值，例如k的选择如何影响性能、不同起始顺序的影响等。这些在附录中有分析，但本报告未能提取具体趋势。根据理论部分，可以合理推断（合理推断）存在一个最优k范围：k过小会导致频繁交接带来启动开销，k过大会退化为单agent长会话。但该推断需要原文附录验证。

Q6: 有什么可以进一步探索的点？

1) 超越MLE-bench的验证：作者在结论中明确表示，在MLE-bench之外建立比较性优势是未来工作。目前只有AOPT内核优化一个补充案例，需要更多类型任务（如科学计算、数据清洗、AutoML、LLM训练）来证实HMA的普遍性。

2) 理论条件的放松与深化：CR-SMDP中的充分条件（如协作访问和使用假设）可能较严格。未来可以探索更一般的条件，例如当两个agent能力不对称、任务状态部分可观测、或工作区存在冲突时，交替的次优性边界。

3) 自适应会话上限k：当前k是固定值或经验选择。可以研究根据任务难度、模型对和进度动态调整k的策略，例如利用强化学习学习一个中断策略，使得每个agent在收益递减时及时退出。

4) 多于两个agent的调度：HMA只考虑两个agent交替。未来可以扩展到3个或更多agent，研究更复杂的轮转策略，例如基于贝叶斯优化的使用次数分配。

5) 与harness级方法的结合：尽管HMA强调无需定制harness，但可以探索将HMA与现有harness（如搜索、反思、记忆）结合，看是否能叠加收益。也可能设计轻量的“工作流记忆”，在不破坏上下文重置的前提下保留跨会话的抽象经验。

6) 模型版本漂移下的鲁棒性：harness陈旧性问题是本文动机之一，未来应系统评估当模型升级后HMA的优势是否稳定，以及是否需要调整工作流参数。

7) 可解释性与诊断：分析每次交接后agent的行为变化，理解为什么交替有效：是上下文重置、双模型互补、还是工作区证据积累？可以做因果干预实验。

8) 应用到AI-for-Science：用户画像中的ai-for-science方向与HMA的迭代优化场景很契合，可尝试将HMA用于科学实验设计、分子优化、超算资源调度等长期优化任务。

9) 成本-性能联合建模：论文使用时间预算比较，但没有详细分析API成本。未来可将CR-SMDP扩展为带成本约束的优化问题，寻找预算分配的最优切换时间。

Q7: 总结一下论文的主要内容

本文的核心论点是：在长期MLE任务中，与其花费精力为单个agent设计越来越复杂的上下文工程和harness，不如将协作放在工作流层面，让多个完整的原生agent通过会话交替和上下文重置互相接力。作者首先通过实证观察发现了两个促使他们转向工作流层的问题：(a) 长时程减速——单会话持续时间越长，agent的推理强度（如平均输出token）下降，任务进展变慢；(b) harness陈旧性——随着底层模型进步，定制harness的附加收益逐渐消失。这两个观察表明，harness级改进是一种“易腐化”的投资，而模型自身能力的提升才是长期可依赖的。

为了给工作流级方案提供理论基础，作者提出了上下文重置半马尔可夫决策过程（CR-SMDP）。在CR-SMDP中，工作流由固定策略μ控制，状态由共享工作区W、保留结果E和活动agent的上下文c(u)组成；每个agent会话被建模为SMDP中的选项，会话终止即选项边界，μ在此选择下一个agent的会话。作者在此框架下推导了交替会话超过单agent参考极限的充分条件，给出了一个显式的实验预算界限，并分析了当会话被限制为最多k个实验（固定k中断）时该优势是否保持。相关假设（如Assumption 3.2“协作访问和使用”）刻画了agent从对方遗留的工作区状态中获益的能力，用α、β等常数量化。

基于此分析，作者设计了HMA工作流：两个agent（A和B）在共享工作区上交替工作；每个会话最多k个实验；会话结束后保留工作区和结果，但完全重置上下文；不添加任务特定harness，也不注入先验知识。该流程的核心机制是：工作区提供了会话间的“长期记忆”，而上下文重置解决了上下文膨胀带来的推理退化，使得每个agent都能以“清醒”状态重新审视前人的工作。

实验在MLE-bench的75个Kaggle任务上进行。HMA达到了78.2%的any-medal率，超过最强开源基线8.0个百分点，并且只花费对方四分之一的时间预算。更令人信服的是，在跨越五种异构agent对的六种配置中，HMA相对于其构成基线平均提升6.4个百分点；这说明无论agent强弱、是否同源，交替策略都能带来增益。在INGV火山喷发预测案例中，第一个Opus会话的MAE为41,214，交替后降至34,234（改善16.9%），且比六个构成agent单独运行获得的最佳MAE（63,991）低46.5%；这个结果突显了单agent长时间运行可能收敛到次优解，而交替接力可以突破瓶颈。作者还在AOPT内核优化任务上进行了48小时补充实验，达到878 cycles，离社区排行榜领先者仅1.6%，证明方法不限于MLE-bench。

最终结论认为，HMA的成功支持了工作流级协调在迭代优化任务中的价值，并呼吁进一步在更广泛任务中验证。论文也承认，当前比较优势的建立范围仅限MLE-bench，其他任务上的推广仍需未来工作。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与‘agent’方向高度相关，尤其适合关注多智能体协作、工作流设计、长期任务规划的读者。

## 基本信息

- 作者：Changye Li, Zijian Zhang, Muheng Li, Yiduo Pang, Tingjun Wu, Menghan Li, Yitong Liu, Hongkai Guo, Mengchi Chen, Chenyu He, Kaisheng Ma, Wei Xu, Song Han, Sihao Liu, Ligeng Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-12
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.13494`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要、引言、方法、结论以及field_evidence_map对应的检索证据片段，并对启发式草稿进行了清洗与补全；理论细节和附录数值如未出现在检索证据中，已在相应位置标注推断或证据缺口。
