---
user_id: "cheng tan"
paper_id: 20833
arxiv_id: "2611.12552v1"
title: "SciMARS: Multi-Agent Recursive Self-Improvement for Closed-Loop Scientific Discovery"
publish_date: "2026-11-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.12552v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.12552v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-11T02:08:06"
---
# SciMARS: Multi-Agent Recursive Self-Improvement for Closed-Loop Scientific Discovery

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-agent systems · closed-loop scientific discovery · recursive self-improvement · shared memory

## 一句话总结

论文提出 SciMARS：在闭环科学发现任务中，用共享证据图保存跨智能体观察，并用可分离的角色、协调与记忆编辑器递归改写多智能体 harness，使团队能在任务内根据实验证据调整分工、交接与记忆策略。

## 摘要

> We propose SciMARS, a framework for recursive task-local adaptation of multi-agent harnesses in closed-loop scientific discovery. Multi-agent systems coordinate analysis, experimental design, and verification as scientific evidence evolves. However, two problems limit their effectiveness: (i) without shared memory, agents lose observations and repeat work; (ii) fixed role procedures, handoffs, and memory policies perpetuate mistakes despite retained evidence. We address these problems with a shared evidence graph and separate component editors. SciMARS adapts roles and coordination; SciMARS + Memory adds a third editor for how agents record, retrieve, and receive evidence. Candidate probes guide composition and selection of the next harness. We evaluate scientific discovery through experimental-data replay and a digital twin. SciMARS + Memory resolves 12/18 GPT-6-Astra and 2/18 GLM-5.3 attempts on Autonomous Science Bench, compared with 8/18 and 1/18 for the base multi-agent team. On ScienceAgentBench and AppWorld, it solves 14/38 and 11/20 tasks, compared with 11/38 and 5/20 for the base team. These results support combining shared memory with harness adaptation across scientific and interactive tasks.

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是闭环科学发现中多智能体团队如何随证据演化而自我修正的组织问题，并非单点算法精度。科学发现被描述为假设生成、实验设计、分析、验证的循环；闭环系统把计算决策与实验反馈连接起来，使新观察能够影响下一轮实验。多智能体系统把工作分给规划者、编码者、验证者等专门角色，理论上可以并行处理分析、设计与验证。但论文指出两个结构性缺陷。第一，证据隔离。一个智能体产生的观察、失败检查或约束，可能只留在自己的对话历史中，无法到达依赖它的另一个智能体。结果是规划者重复一个已被拒绝的实验，编码者遗漏验证者已经发现的约束。这本质上是跨角色证据的路由与持久化问题，不能简单归结为上下文长度问题：证据需要被记录、带来源地保存，并在后续决策点被检索或推送。第二，固定执行 harness。即使证据被保留，角色流程、交接规则与记忆策略仍然固定，规划者未必据此改变计划，编码者未必改变实现，验证者未必重新检索相关证据。论文强调“保留证据”不等于“使用证据”；失败检查被存下来，并不自动改变下一次行动。这个区分是论文立论的关键：记忆与程序适应是两个问题，不能互相替代。由此，论文需要同时解决记忆与适应，并且适应必须是任务局部的、递归的。所谓任务局部，是不同科学 campaign 的实验预算、约束与验证标准不同，全局统一的 workflow 难以适配；所谓递归，是每一轮尝试产生的证据与结果应反馈到下一版 harness，而非一次性设计后固定。另一个隐含约束是自修改的风险：如果编辑器可以任意重写角色与协调，可能引入不稳定或退化，因此需要候选探针、受测试的选项以及证据不足时保留原 harness 的机制。论文的评估设计也对应这一双重目标：既要看共享记忆，也要看 harness 适应，且要在科学发现、科学编程与交互任务上验证。

Q2: 有哪些相关研究？

论文的相关研究可以沿四条线索组织。第一是多智能体框架与角色分工。Introduction 引用 AutoGen（Wu et al., 2023）与 Hong et al.（2024）等工作，说明多智能体系统通过专门角色与显式交互模式组织协作；AutoGen 以对话或交接协调 agent，角色框架则把工作拆成规划、编码、验证等环节。论文的出发点在于指出固定角色流程在证据变化时会失效，并非否定角色分工。第二是 agent harness 与 workflow 的自动改进。Introduction 提到近期方法从执行反馈、工具使用等信号改进 agent harness 与 workflow（Lee et al., 2026; Nam et al., 2026）。这类工作通常搜索提示、工具调用序列或工作流结构，追求跨任务可复用的更优流程。SciMARS 的差异在于把改进限制在任务局部，并让角色、协调与记忆策略分别可编辑；它在任务内根据证据递归调整 harness，而非只搜索一个全局 workflow。论文还把 integrated workflow search 作为 baseline，使用 MASS-RHI 组件与 benchmark adapter，说明 workflow 搜索是直接的竞争路线。第三是共享记忆与上下文图。论文明确受 Mycelium 的 active context graphs（Choudhury et al., 2026）启发，提出带来源标注的共享证据图，记录观察、发现、计划与未解决问题，并通过检索与订阅路由给角色。与普通对话历史或向量检索相比，这条线索强调证据的结构、来源与跨角色可达性；与 blackboard 式共享工作区相近，但论文的重点是证据图如何支撑后续 harness 编辑，而不只是检索增强。第四是闭环科学发现与评测。论文使用 Autonomous Science Bench（Wen and Liu, 2026）、ScienceAgentBench（Chen et al., 2025）与 AppWorld（Trivedi et al., 2024）。前者覆盖生物学、化学与材料科学的六个 campaign，要求 discovery 与 held-out learning 双 gate；ScienceAgentBench 评估数据驱动科学发现中的代码与分析任务；AppWorld 评估模拟应用状态下的交互程序。相关研究还包括 autonomous lab、实验设计优化与 AI scientist 系统，但给定片段没有展开这些引用，因此这里只能作为领域背景，不能归为论文明确讨论的 baseline。论文声称的空白是：现有工作或者提供记忆但不改变角色、交接与记忆策略，或者搜索 workflow 但不把共享证据与任务局部 harness 适应结合起来。这个 gap 的成立依赖于 fixed memory 与 integrated workflow search 两个 baseline 的完整对比；可见 Results 片段只明确给出 base 与 SciMARS + Memory 的数字，完整证据仍需回原文核对。

Q3: 论文如何解决这个问题？

SciMARS 的技术路线可以概括为共享证据图、分离式组件编辑器与候选探针约束的 harness 组合。它是在任务内递归修改多智能体 harness，并非训练基座模型，因为论文强调 task-local adaptation 与候选探针，未提参数更新，这属于合理推断。第一，共享证据图。系统把观察、发现、计划与未解决问题记录为图节点，并保留来源 provenance。与只保留对话历史不同，证据图支持检索与订阅：记录可以跨越当前 handoff，被后续依赖它的角色读取或推送。这样，规划者可以看到验证者拒绝过的实验，编码者可以看到约束，验证者可以回查早期观察。论文受 Mycelium 的 active context graphs 启发，但把它用于闭环科学发现中的跨角色证据路由。第二，分离式组件编辑器。SciMARS 把角色与协调的修改拆成独立编辑器，避免一次重写整个 harness。角色编辑器修改角色程序与职责，协调编辑器修改交接与协作方式；论文提到 endpoint guidance belongs to the contract，说明角色契约中包含端点与交付约束，编辑器不能任意越界。证据不足时，编辑器可以选择保留原 harness，而不是强制改动；它也可以保留原 harness、采纳或组合候选修改，或改写角色程序。这一点是稳定性的关键：并非每轮都必须自我修改。第三，记忆编辑器。SciMARS + Memory 增加第三个编辑器，专门修改智能体如何记录、检索与接收证据。论文提到该设置会触发独立的 memory-editor LLM 调用。也就是说，记忆是可与角色、协调一样被证据驱动调整的对象。论文把“保留证据”与“调整团队如何使用证据”分开：共享证据图解决前者，记忆编辑器解决后者。第四，候选探针与组合。候选探针用于指导下一版 harness 的组合与选择。论文给出形如 HSt = Compose(Ht, {Hjt, ojt}j∈A, o0t, Lt) 的组合式更新，把当前 harness、角色级候选与输出、任务级信号等组合成下一版 harness；具体符号定义需回原文 Method 核对。关键约束是 Coordination must come from a tested option：协调修改必须来自被测试过的选项，不能凭空拼接；启用记忆时，同样的约束也适用于记忆修改。这相当于在自修改外面加了一层验证门。第五，闭环运行。系统把 in silico 分析与模拟实验验证连接起来，评估使用 experimental-data replay 与 digital twin。每轮产生证据、失败检查与结果，证据图保存它们，编辑器据此提出或保留 harness，候选探针测试后再组合成下一版。这个过程在任务内递归，并非跨任务训练一个全局策略。

Q4: 论文做了哪些实验？

论文的实验覆盖三类任务：闭环科学发现、科学编程与交互式应用。评估对象包括 Base Multi-Agent、Fixed Memory、Integrated Workflow Search、SciMARS 与 SciMARS + Memory。Results 片段明确说比较这些方法；Integrated Workflow Search 使用 MASS-RHI 组件与作者实现的 benchmark adapter。第一，Autonomous Science Bench。包含六个 campaign：生物学中的 nuclease 与 biosensor engineering，化学中的 propylene 与 Suzuki reaction，材料科学中的 oxygen-evolution 与 coercivity。实验预算依次为四批 96、三批 10、三批 20、四批 48、四批 48、四批 16 个设计。成功需同时通过 discovery 与 held-out learning 两个 gate，并非只看是否找到候选。论文报告两个模型 cohort：GPT-6-Astra 与 GLM-5.3，每个 cohort 18 次尝试。评估通过 experimental-data replay 与 digital twin 进行，属于模拟实验验证，并非湿实验。第二，ScienceAgentBench。使用的快照包含 102 个已验证任务；论文只纳入 38 个 machine-graded 任务，排除 64 个 visualization 任务，并明确不对完整 benchmark 作声明。所有方法都收到任务指令、要求的输出路径与相同的 public files；都不获得 organizer 提供的 domain knowledge、previews 或 folder trees。通过标准是最终程序满足官方 evaluator；success 是任务通过百分比。这个设置控制了信息访问，但也意味着结论只适用于 38 个机器评分任务。第三，AppWorld。用模拟应用状态上的测试评估交互程序；使用 package version 0.1.3.post1 与 data version 0.1.0，共 20 个任务。通过数按最终程序是否满足测试计算。这个 benchmark 更接近交互式工具使用与状态管理，与科学发现的任务结构不同。第四，指标与协议。论文以通过任务数与成功率为主；ScienceAgentBench 的成功是官方 evaluator 通过率；Autonomous Science Bench 需要双 gate；AppWorld 通过模拟状态测试。可见片段没有给出置信区间、重复种子、统计检验、token 与成本或延迟数据，也没有给出所有 baseline 在每个 benchmark 上的完整数值。

Q5: 发现了什么实验现象？

可见结果支持一个总体趋势：SciMARS + Memory 在三个 benchmark 上都优于 Base Multi-Agent，但增益幅度与任务类型、基座模型强弱明显相关。第一，Autonomous Science Bench。GPT-6-Astra cohort 从 8/18 提升到 12/18，绝对增加 4 次，按通过率约从 44.4% 到 66.7%；GLM-5.3 cohort 从 1/18 提升到 2/18，绝对只增加 1 次，按通过率约从 5.6% 到 11.1%。前者说明在较强模型上，共享记忆与 harness 适应可以带来实质性提升；后者说明较弱模型上提升存在但绝对值很低，harness 适应不能弥补基座能力不足。由于 ASB 要求 discovery 与 held-out learning 双 gate，提升可能来自更好的证据保留与跨轮约束传递，但可见材料没有拆分两个 gate 的通过情况，不能确定。第二，ScienceAgentBench。Base 11/38，SciMARS + Memory 14/38，绝对增加 3 个任务，约从 28.9% 到 36.8%。相对提升约 7.9 个百分点，低于 AppWorld 的 30 个百分点；绝对通过数增加 3，也少于 AppWorld 的 6。这可能因为科学编程任务更依赖单次代码正确性、库与数据理解、输出 schema，而不是多轮证据协调；也可能是 38 个机器评分任务的难度分布所致。这个解释是推测，需要 per-task 分析。第三，AppWorld。Base 5/20，SciMARS + Memory 11/20，绝对增加 6 个任务，约从 25% 到 55%，是报告中最明显的提升。交互式应用状态测试需要记住先前操作、约束与失败信息，共享证据图与记忆编辑器理论上更直接地对症；但同样没有消融证据证明增益来自记忆而非 harness 适应。第四，关键张力：论文的设计主张是“保留证据”与“适应团队如何使用证据”要分开，但可见 Results 只明确给出 Base 与 SciMARS + Memory 的对比。Fixed Memory、SciMARS 单独、Integrated Workflow Search 的完整数值未在片段中出现。因此无法判断：增益是来自共享记忆、来自 harness 适应，还是两者交互；也无法判断候选探针带来的收益是否被其额外编辑成本抵消。若 Fixed Memory 已经接近 SciMARS + Memory，则“联合适应”的必要性会被削弱；若 SciMARS 单独就很强，则记忆编辑器的边际贡献需要重新论证。这是阅读全文时应优先核对的消融。第五，失败模式与边界。GLM-5.3 的 1/18 到 2/18 表明方法并非普遍有效；在弱模型或高难度任务上，编辑器可能提出无效修改，或证据图不足以支撑正确决策。论文设计“证据不足时保留 harness”与“协调必须来自被测试选项”来降低风险，但可见材料没有报告负结果、回滚率、编辑失败率或震荡情况。AppWorld 的大幅提升也可能部分来自任务可交互修复，而非科学发现能力。

Q6: 有什么可以进一步探索的点？

第一，完整 factorial 消融。最紧迫的是报告 Base、Fixed Memory、SciMARS、SciMARS + Memory、Integrated Workflow Search 在每个 benchmark 上的完整数值，并分别拆出角色编辑器、协调编辑器、记忆编辑器的边际贡献。只有这样才能验证论文的核心主张：共享记忆与 harness 适应是互补的，而不是重叠或相互替代。第二，统计可靠性。补充多随机种子、置信区间、配对检验与效应量；当前 18 次或 38 次任务的通过数差异可能受采样波动影响，尤其是 GLM-5.3 的 1/18 与 2/18，绝对差异只有一次尝试。第三，成本与延迟。编辑器调用、候选探针测试、证据图检索与订阅都会增加 token 与 wall-clock 成本。需要报告每任务成本、编辑次数、回滚率、候选探针数量与收益曲线；否则无法判断在真实实验预算下是否划算。第四，记忆图的质量与治理。研究证据冲突如何解决、过期证据如何淘汰、来源可信度如何加权、检索精度与召回如何影响决策、图规模增大后的压缩与检索效率。还要看订阅机制是否造成信息过载，以及错误证据是否会被跨角色放大。第五，递归自修改的稳定性。分析多轮编辑后的漂移、震荡、过拟合与退化；编辑器何时选择“保留原 harness”最有效；候选探针的生成策略与选择偏差；是否需要理论上的单调改进保证或安全约束。第六，从模拟走向真实。当前评估是 experimental-data replay 与 digital twin，需要真实湿实验或更高保真度的数字孪生验证；同时要区分“通过 evaluator”与“科学正确”。论文自己的验证提示也指出，满足输出要求并不建立科学正确性，需要检查方法、输入处理、join、单位、输出 schema、泄漏与不支持的捷径。第七，跨任务与跨领域迁移。任务局部适应是否能在新 campaign、新学科、新工具链上快速迁移；能否把学到的编辑经验蒸馏为可复用先验，同时避免负迁移；与 continual learning、meta-learning 的关系。第八，人机协作与安全。科学发现涉及高风险实验与伦理约束，需要人类审批、可审计的编辑日志、危险操作拦截与责任追踪；多智能体自修改系统尤其需要可解释的变更理由与回滚机制。第九，benchmark 设计。ScienceAgentBench 只用了 38/102 个机器评分任务，排除 visualization；需要完整 benchmark 与更细的任务分层。Autonomous Science Bench 的双 gate 设计很好，但应报告每个 gate 的通过率与失败原因。AppWorld 的交互成功是否迁移到科学工作流，也需要跨 benchmark 的相关性分析。

Q7: 总结一下论文的主要内容

一、研究定位。论文面向闭环科学发现中的多智能体系统，主张 agent 的改进不应只是一次性的 prompt 或 workflow 搜索，而应是在任务内、随实验证据递归进行的 harness 适应。所谓 harness，在论文语境中包括角色职责、角色间交接与协调方式以及记忆策略；SciMARS 的目标是让这些组件在每一轮证据到来后被编辑、测试与组合，而不是固定执行。二、问题诊断。作者把现有系统的失败归为两类。第一是证据隔离：观察、失败检查与约束留在单个 agent 的历史里，无法到达依赖它的另一个 agent，导致重复被拒绝的实验或遗漏验证者发现的约束。第二是固定执行规则：即使证据被保留，角色流程、handoff 与记忆策略不变，规划者、编码者、验证者仍可能重复错误。论文特别强调“保留证据”不等于“使用证据”，因此共享记忆与程序适应必须分开处理、联合设计。另一个背景是科学发现的实验预算有限，ASB 的 campaign 只允许若干批设计，系统必须在少量轮次内利用证据，而不是无限试错。三、方法。SciMARS 由三部分组成。其一是共享证据图：记录观察、发现、计划与未解决问题，带 provenance，并通过检索与订阅把记录路由到当前 handoff 之外的角色。其二是分离式组件编辑器：角色与协调分别编辑，避免单体式重写；SciMARS + Memory 增加第三个记忆编辑器，专门调整 agent 如何记录、检索与接收证据。编辑器可以选择不改，证据不足时保留原 harness；角色契约中的 endpoint guidance 约束编辑边界。其三是候选探针：生成并测试候选修改，下一版 harness 由当前 harness、角色级候选与输出、任务级信号组合而成，论文给出 HSt = Compose(Ht, {Hjt, ojt}j∈A, o0t, Lt) 形式的更新；协调修改必须来自被测试的选项，启用记忆时同样约束记忆修改。这个“测试后再组合”的机制是自修改的安全阀。四、实验。论文在三个 benchmark 上评估：Autonomous Science Bench 的六个 campaign，包括 nuclease、biosensor、propylene、Suzuki reaction、oxygen-evolution、coercivity，实验预算从四批 96 到三批 10 不等，成功需通过 discovery 与 held-out learning 双 gate，使用 experimental-data replay 与 digital twin；ScienceAgentBench 只取 38 个 machine-graded 任务，排除 64 个 visualization 任务，所有方法获得相同 public files、任务指令与输出路径，不获得 organizer domain knowledge；AppWorld 用模拟应用状态测试 20 个交互任务。Baseline 包括 Base Multi-Agent、Fixed Memory、Integrated Workflow Search（MASS-RHI + adapter）、SciMARS 与 SciMARS + Memory。五、发现。SciMARS + Memory 相对 base 在报告的所有 cohort 上提升：ASB GPT-6-Astra 8/18 到 12/18，GLM-5.3 1/18 到 2/18；ScienceAgentBench 11/38 到 14/38；AppWorld 5/20 到 11/20。AppWorld 的绝对提升最大，ASB 强模型次之，ScienceAgentBench 较小，GLM-5.3 的绝对水平仍很低。作者据此认为共享记忆与 harness 适应应联合使用，并强调该设计把“保留证据”与“调整团队如何使用证据”分开。六、边界与待核对。可见材料没有给出 Fixed Memory、SciMARS 单独与 Integrated Workflow Search 的完整数值，因此无法完成归因消融；没有统计检验、重复种子、成本与延迟数据；ScienceAgentBench 只覆盖 38/102 个任务，不能外推到完整 benchmark；科学发现评估是模拟环境，不是湿实验；GLM-5.3 的结果说明方法对基座能力敏感。论文还提示，最终程序满足输出要求并不等于科学正确，验证需要检查方法、输入处理、join、单位、schema、泄漏与不支持的捷径。总体而言，这是一篇把共享证据图与任务局部 harness 编辑结合起来的系统型工作，核心价值在问题分解与架构设计，实证强度取决于全文消融与真实场景验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你的 agent 方向直接相关：论文把多智能体 harness 的角色、协调与记忆策略作为可编辑对象，并提供候选探针约束的递归改进框架。

## 基本信息

- 作者：Weimin Wu, Han Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-11-09
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.12552v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Results 与 References 片段，并用摘要补足；Method、Discussion 与 Conclusion 在给定材料中为空，因此涉及编辑器内部实现、完整消融与结论性判断的地方已标注为推断或证据缺口。
