---
user_id: "cheng tan"
paper_id: 18411
arxiv_id: "2610.35845"
title: "Work Agents: Systems, Capabilities, and Optimization"
publish_date: "2026-10-27"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.35845.pdf"
pdf_url: "https://arxiv.org/pdf/2610.35845v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-27T13:14:42"
---
# Work Agents: Systems, Capabilities, and Optimization

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：work agents · agent systems · runtime harness · agent evaluation

## 一句话总结

这篇综述/立场论文提出以「工作」为中心的系统级视角，把 work agent 定义为「基础模型决策核心 + runtime harness + 有状态数字工作区」的组合体，用工作持久性（work persistence）与工作开放性（work openness）两个维度刻画工作条件、把数字工作分为五种形态、提炼可靠执行所需的五类核心能力，并在模型优化、harness 优化与二者联合优化三个层面组织现有方法，最后以「工作产出」为轴重画评测方法与指标，指出持续委托、开放环境适应、执行—能力提升闭环三个未来方向。

## 摘要

> As foundation models evolve from information generation to digital work execution, agents are increasingly capable of completing complex tasks across tools, applications, and workflows. However, existing research lacks a unified perspective on how agents reliably accomplish delegated work. To address this gap, we present a work-centric, system-level perspective that views work agents as systems combining foundation models and runtime harnesses to execute tasks in stateful workspaces and deliver verifiable outcomes. We introduce two complementary dimensions, work persistence and work openness, to characterize working conditions, and classify digital work into five forms according to task scope and organization. Building on this framework, we identify five core capability families required for reliable work execution and examine approaches to model optimization, harness optimization, and their joint optimization. We further analyze existing evaluation methods and metrics from the perspective of work outcomes. Finally, we discuss future research directions, including sustained work delegation, adaptation to open environments, and the interaction between work execution and capability improvement. This paper provides a unified framework for understanding, optimizing, and evaluating work agents, supporting the development of systems capable of delivering reliable, verifiable, and practically valuable work outcomes.

Q1: 这篇论文试图解决什么问题？

【1）论文认定的问题不是某个任务上的性能缺陷，而是研究视角的缺失】摘要明确写道：随着基础模型从信息生成走向数字工作执行，agent 已能跨工具、应用与工作流完成复杂任务，但「现有研究缺乏关于 agent 如何可靠完成被委托工作的统一视角」。换言之，问题被定位在「概念与组织层面」——缺少一套能把任务条件、系统构成、能力需求、优化手段与评测口径串起来的语言。
【2）现实侧的压力来源（Introduction 证据）】人类已经在软件工程、办公工作、web 与 computer use、企业运营、科学研究等场景把复杂生产力任务委托给 agent；Codex、Claude Code 一类前沿 agent 已支持多日、跨会话的工作，并接管了原本由人承担的部分结果评估与反馈驱动修订。这带来一个结构性变化：工作的正确性不再由单次输出决定，而取决于「相关文件、记录、应用中的证据」以及「先前动作所建立的状态」。
【3）由此产生的四个具体缺口（由 Introduction 与 Abstract 合并推出）】(a) 任务条件无法被统一描述：短问答与多日跨会话委托被放在同一套评测语汇下比较，缺少刻画「工作有多持久」「工作在多大程度上是开放的」的坐标；(b) 系统构成被割裂研究：模型能力与 scaffolding/harness 常被分开讨论，缺少把两者视为一个交付单元的系统观；(c) 能力需求与优化手段之间没有映射：不清楚哪类能力该由训练解决、哪类该由 harness 解决、哪类必须联合优化；(d) 评测口径错位：既有评测多以「答案」为单位，而真实工作交付的是 artifact、状态变更与工作流结果，且 grader 与被测系统常常同步演化，导致可比性下降（结论段的「distinguishing the inspected result, the assessed properties…」与此一致）。
【4）一个隐含假设需要点明】论文假定「工作」可以被离散化为可枚举的五种形态，并假定「持久性 × 开放性」这两轴足以刻画工作条件。这两个假定在文中是分析装置而非被验证的命题：可见证据中没有给出形态划分的操作化判据，也没有做维度正交性检验。因此，读出这套框架时应把它当作「组织文献的坐标系」，而非「已被验证的分类学」（合理推断 + 证据缺口提示）。
【5）问题定义的边界】论文明确把对象限定为「数字工作」（digital work），即发生在有状态数字工作区中的、结果可验证的任务；物理世界操作、纯开放式艺术创作、无客观验收标准的工作基本不在框架核心。这一限定使「可验证产出」成为框架的阿基米德点——也意味着凡不可验证的工作，本框架的解释力会显著下降。
【6）与既有综述的差异（合理推断）】多数 agent 综述按「能力」或「应用域」切分；本文按「工作的组织形态 + 系统构成 + 优化杠杆」切分，因此更适合作为设计空间地图使用，而不适合作为单点方法的技术细节来源。

Q2: 有哪些相关研究？

论文的相关工作交织在两条线上：一条是「被综述的对象」，一条是「用于综述的组织工具」。以下按主题梳理可见证据能够支持的脉络。
【1）基础模型能力线：推理、规划与工具使用】Introduction 开篇即以基础模型在 reasoning、planning、tool-use 上的进展作为 agent 工作范围扩张的前提。这一条线在方法部分被进一步展开为训练阶段谱系：next-token prediction 的预训练建立语义、计算、结构与视觉知识；mid-training 注入领域材料、交互历史与长上下文使用能力；post-training 用示范、执行奖励、教师指导与累积反馈去监督「决策 / 轨迹 / 产物 / 早前尝试」四个不同粒度的对象（Figure 6 及其图注）。
【2）递归自我改进（recursive self-improvement）线】Introduction 明确点出 recursive self-improvement 把「能力提升本身」纳入 agent 可承担的工作范围，这是本文区别于一般 agent 综述的关键理论接口，也是结论中「执行—能力提升互动」方向的来源。
【3）Agent 系统与 harness 工程线】论文以 Codex、Claude Code 为前沿样本，说明多日、跨会话工作依赖 orchestration；方法部分进一步讨论了 skill 与 tool 的分工（稳定操作用工具实现，skill 提供「何时用、怎么用」的判断）、动作接口设计（agent 如何把决策翻译为动作）、长历史高效访问等 harness 议题。这条线在文献中通常被称为 scaffolding / agent framework / computer-use harness（术语归并为我的概括）。
【4）评测与 benchmark 线（Introduction 的图/表证据）】可见的 benchmark 名称包括：SWE-bench、SWE-agent、SWE-Lancer、AIA、PresentBench、MINT、ScienceAgentBench、Terminal-Bench、EntCollabBench、MCP-Universe、BankerToolBench、Mind2Web、AndroidWorld、MCP-Bench、CI-Work、TravelPlanner、PaperBench、SpreadsheetBench，并配有时间轴 2023—2026。由于 PDF 抽取把图内标签打散，无法可靠还原「哪个 benchmark 对应哪一年」，此处只记录集合而不做年份断言（证据限制提示）。
【5）训练与经验积累线】方法部分讨论了如何保留并利用历史经验（引用 [60, 260, 265, 369] 一类工作），以及「构造相关练习 → 提出变更 → 在独立工作环境中测试」的自改进回路；AutoDesign 被用作「渲染—校验—critic 反馈」闭环改进可复用工作流的案例。
【6）评测方法论线】结论段把评测拆为「被检查的结果 / 被评估的属性 / 判定准则」三层，并主张用「对共享任务要求的独立产出检查」使收益在 agent 与 grader 双变化下仍可比（引用 [383–385]）。这与 LLM-as-judge、可验证奖励、单元测试式验收等既有路线直接对话。
【7）定位差异】作者不做方法对标表，而是把上述各线统一到「工作条件—能力—优化杠杆—产出评测」的四段式框架中；因此本文的 related work 更像「文献的空间重排」，而非「方法的性能对比」。这一点决定了它作为索引的价值高于作为基线参考的价值。

Q3: 论文如何解决这个问题？

论文的「方法」是一套概念装置，而非一个新模型或新系统。可拆为五层。
【1）定义层：work agent 的三元构成】work agent = 以基础模型为决策核心 + 依赖 runtime harness 组织环境观察与动作执行 + 在有状态数字工作区中完成被委托任务。定义中三个要素缺一不可：模型负责决策，harness 负责「看见什么、能做什么」，工作区负责「状态存续」。这一定义的价值在于把「模型能力不足」与「harness 设计不足」在概念上分开，从而为后续的归因与优化分工留出位置。
【2）条件层：两个刻画维度】work persistence（工作持久性）：工作跨阶段、跨事件、跨会话延续的程度，责任需要被携带而非一次性释放；work openness（工作开放性）：工作目标与路径在多大程度上是事先给定的，还是在执行中逐步发现与界定的。结论段用一句话概括二者的分工：「Openness makes discovery part of this process; persistence carries responsibility across stages and events.」两轴组合形成一个二维工作条件空间，短问答落在低持久—低开放角，多日跨会话的开放式委托落在对角。
【3）形态层：五种数字工作形态】按「任务范围」与「组织方式」两个准则划分。结论段给出的演化序列（从返回答案、做局部改动，到协调工作流与相关工作区对象）与结论段并列的四类产出（answers、artifacts、state changes、workflow results）提示了形态谱系的大致骨架：回答问题 / 产出制品 / 施加状态变更 / 编排工作流 / 持续委托式工作（最后一项为合理推断，因为五种形态的确切命名在可见证据中未被完整列出，属需回原文核对的缺口）。
【4）能力层：五类核心能力族】论文声称识别出可靠执行所需的五类能力族。从可见的方法章节痕迹（2.3.3 Planning and Coordination、长历史访问、动作接口、skill/tool 区分、验证回路）可以推断其覆盖范围大致包括：需求理解与目标保持、证据获取与上下文组织、规划与多阶段协调、动作接口与工具/技能使用、状态维护与结果验证（该清单为基于章节结构的合理推断，非原文枚举）。
【5）优化层：三条杠杆】
 (a) 模型优化：沿 pre-training → mid-training → post-training 展开；post-training 的关键变量是「监督粒度」——监督决策、轨迹、产物还是早前尝试，四者所需信号（示范、执行奖励、教师指导、累积反馈）不同，且不预设共享数据集或通用训练配方。
 (b) harness 优化：包括工具与技能的表示（稳定操作→工具，判断与用法→技能）、动作接口设计、长历史与证据的高效访问、状态保留与重建、验证与反馈机制。论文对 harness 的评价标准是功能性的：接口是否在不耗尽上下文、不要求反复检查的前提下提供「下一步决策所需的证据」。
 (c) 联合优化：模型与工作同时变化时的协同更新；其闭环形态是「保留经验 → 构造相关练习 → 提出变更 → 在独立工作环境中测试 → 用后续任务表现衡量变更是否有效」。
【6）评测层：以工作产出为轴】论文主张区分「被检查的结果」「被评估的属性」「判定准则」，并以「对共享任务要求的独立产出检查」作为跨 agent 与跨 grader 的可比性基础。
【整体评价】该框架的解释力来自「分层解耦」：条件、能力、杠杆、评测四层各自可独立讨论又可互相映射。它的代价是每一层都停留在原则性描述，缺少可操作阈值——例如「持久性高到多少需要跨会话记忆」这类问题在可见证据中没有答案（合理推断）。

Q4: 论文做了哪些实验？

【0）性质判定】这是一篇综述/立场论文，没有自建的模型、系统或 benchmark，也没有报告任何由作者自己跑出的实验数值。因此本字段记录的是「论文所开展的实证性工作」——即它对评测版图的梳理、对训练配方的归纳与对案例的引用。
【1）评测版图的系统梳理（主要实证性劳动）】论文从「工作产出」视角重排既有评测方法：按产出类型（答案 / 制品 / 状态变更 / 工作流结果）与工作条件（持久性、开放性）定位各 benchmark；Introduction 中出现的时间轴图罗列了 SWE-bench、SWE-agent、SWE-Lancer、AIA、PresentBench、MINT、ScienceAgentBench、Terminal-Bench、EntCollabBench、MCP-Universe、BankerToolBench、Mind2Web、AndroidWorld、MCP-Bench、CI-Work、TravelPlanner、PaperBench、SpreadsheetBench，覆盖 2023—2026（年份与条目的对应关系因 PDF 抽取错乱而无法可靠还原）。
【2）训练阶段的「监督粒度」对照分析】Figure 6 与其图注构造了一组示意性样本，区分「可见上下文」与「预测目标 / 反馈」，并对比四类 post-training 监督对象：决策、轨迹、产物、早前尝试。这是一种「概念性消融」——不是跑实验，而是把不同训练路线按监督信号与监督对象两轴摆开，说明它们不可互相替代。
【3）引用型微观案例】
 (a) Kimi K2 的知识改写实验：对知识密集段落做「不同风格、不同视角、保持原意」的改写；早期 checkpoint 实验报告「十种不同改写各用一次」的 SimpleQA 准确率为 28.94%，对比「原文重复十次」的 23.76%。这是一个被引用的数值对比，用来说明表面形式多样性优于机械重复。
 (b) AutoDesign 的工作流改进回路：利用渲染、校验与 critic 反馈改进可复用的设计工作流，用于说明「单任务反馈如何沉淀为可复用工作流」。
【4）评测协议层面的设计讨论（非实验但带方法论语境）】论文讨论「独立产出检查 vs 与共享任务要求对齐的检查」，并指出收益需要在 agent 与其 grader 同时变更时仍可比（引用 [383–385]）；这属于评测设计原则的论证，而非对照实验。
【5）没有做的事（重要缺口）】可见证据中没有：跨 benchmark 的定量元分析（如按持久性分层的成功率曲线）、对「模型增益 vs harness 增益」的方差分解、对五形态/五能力族的标注一致性研究、对所述未来方向的试点实验。因此本文不能用于回答「某类工作该选哪条优化路线效果最好」这类量化问题。

Q5: 发现了什么实验现象？

【说明】以下「现象」均为论文在综述与论证中提炼或引用的观察，而非作者自跑实验的结果；标注为「引用数值」者为文中出现的二手数据。
【1）监督粒度决定能力形态，且四种粒度不可互换】Figure 6 的对照显示：监督「决策」培养的是单步判断，监督「轨迹」培养的是多步连贯性，监督「产物」把学习信号落在结果质量上，监督「早前尝试」则直接训练「从失败中修正」的能力。由此得出的现象是：同一个任务分布上，换监督对象等价于换能力目标；这解释了为何「用执行奖励做 RL」与「用示范做 SFT」在长程工作上表现不可比——它们优化的对象根本不同（合理推断，基于图注的对比结构）。
【2）形式多样性优于机械重复（引用数值）】Kimi K2 的早期 checkpoint 实验中，「十种不同改写各用一次」的 SimpleQA 准确率为 28.94%，「原文重复十次」为 23.76%，差值约 5 个百分点。论文用它论证：对 work agent 而言，把同一知识以多种表面形式暴露，比重复堆叠同一样本更能提升可用性。这是一个反直觉但可信度受限于「早期 checkpoint + 单一基准」的观察。
【3）接口设计直接改变证据获取成本】论文提出一个可操作判据：好的接口应当在不耗尽上下文、不要求反复检查的前提下提供「下一步决策所需的证据」。其反面现象是：当接口只返回原始、冗长或需二次解析的输出时，agent 会把预算消耗在「看」而不是「做」上——这是长程工作失败的一个常被忽视的来源（该表述为对论文论证的转述与合理外推）。
【4）工具与技能的分工是可观测的设计张力】稳定操作实现为工具、判断与用法实现为技能；二者混淆会导致两类失败：把需要判断的事固化成工具（失去适应性），或把稳定操作写成技能（增加不确定性）。论文强调这一区分对「需要判断何时用」的工作 agent 尤其重要。
【5）持久性上升会把失败模式从「不会做」转为「丢目标 / 状态漂移」】结论段指出：当工作从「返回答案或做局部改动」扩展到「协调工作流与相关工作区对象」时，agent 必须追踪更多依赖，并用上一步的结果指导下一步；此时主要风险不再是单步能力不足，而是依赖丢失、状态未重建、责任未跨阶段携带（这是论文的框架性论断，属作者主张而非实测结论）。
【6）开放性与持久性引入不同的失败面】开放性把「发现」纳入工作过程（要先弄清要做什么），持久性把「责任」跨阶段携带。二者叠加时会出现一类特殊失败：agent 在早期基于不完整证据锁定了一个错误的目标解释，并在后续所有阶段一致地执行它——一致性反而放大了错误（此为我基于两轴定义推出的失败模式，属推测，论文可见证据未直接展开）。
【7）grader 与被测系统共同演化会侵蚀可比性】结论段明确要求区分「被检查的结果 / 被评估的属性 / 判定准则」，并主张以对共享任务要求的独立产出检查来稳定比较。其背后的现象是：当评测器随 agent 一起迭代时，报告的提升可能部分来自评测口径漂移而非系统能力提升（该风险由论文的评测讨论直接支持，具体幅度未被量化）。
【8）benchmark 演化本身构成一条证据线】Introduction 的时间轴把 2023—2026 的评测从短任务推向多日、多工具、跨应用的工作（SWE 系、terminal 系、MCP 系、办公制品系、科研系并存），说明「工作条件」正在系统性变难；但条目与年份的对应因 PDF 抽取错乱无法核实，使用时需回原文图表确认（证据限制）。

Q6: 有什么可以进一步探索的点？

【论文明确提出的三个方向】
【1）Sustained work delegation（持续工作委托）】当工作跨越数天、多个会话与多次中断，需要解决的是「责任如何不丢」：目标与约束如何在长跨度上保持、上下文如何重建、交接（人↔agent、agent↔agent）如何协议化、中断后如何恢复、过程如何可审计。论文把持久性设为独立维度，等于宣告这一类问题不能被「更大会话窗口」自然消解。可探索点：跨会话状态表示与压缩、显式工作日志/承诺账本、恢复点与幂等动作设计、长任务的成功率随时间的衰减曲线测量（后者为我的延伸）。
【2）Adaptation to open environments（开放环境适应）】开放性要求 agent 在目标未完全给定的情况下先做发现：识别需求、探查环境、发现可用工具、处理环境漂移。可探索点：把「发现」当作可训练/可评测的一等能力、开放环境下的课程与自博弈、能力—环境不匹配的检测、以及在开放环境中定义「做完了」的判据（验收标准的自动构造）。
【3）Work execution ↔ capability improvement 的互动】执行产生观测、流程、纠正与反馈，可以反哺后续工作；训练可以沉淀可复用知识与动作模式；harness 改动可以改善证据可达性、执行控制与状态保留；模型—harness 需要协同更新，因为「模型在变，工作也在变」。可探索点：从工作痕迹自动挖掘训练信号（把 artifact、diff、critic 意见转成监督）、构造「相关练习」的自动课程、以独立工作环境验证变更（避免自评自测）、以及 recursive self-improvement 的安全边界。
【论文隐含但未展开的方向（我的延伸，标注为推断）】
【4）成本、时延与上下文经济学】论文用「不耗尽上下文」作为接口评价标准，但没有给出成本模型。可行方向：把 token/工具调用/人类介入折算为工作成本，绘制「成功率—成本」前沿，而非单点准确率。
【5）失败分类学与负结果库】框架给出了能力族却未给出失败模式枚举；建立跨 benchmark 的失败标签体系（丢目标、状态未重建、证据误读、过早收敛、验证缺失、权限越界）会直接提高框架的诊断价值。
【6）验证与 grader 的对抗鲁棒性】既然评测主张「独立产出检查」，就需要研究 grader 被 agent 针对性优化的风险（reward hacking）、以及可验证性分级（可自动验证 / 可半自动验证 / 仅可人审）。
【7）权限、安全与组织边界】真实数字工作涉及凭据、写操作、外部副作用；「状态变更」类工作的失败代价远高于「回答」类。需要研究最小权限执行、可回滚动作、以及副作用的事前审批接口。
【8）多 agent 组织形态】五形态中的「工作流编排/持续委托」天然是多角色场景；值得研究角色分工、责任归属、以及协作开销随持久性与开放性的 scaling 趋势。
【9）跨领域迁移】把工作条件两轴用于非数字工作（实验台操作、临床流程）会暴露「可验证性」假设的边界，这本身是检验框架普适性的好实验（与 AI-for-Science 方向直接相关）。

Q7: 总结一下论文的主要内容

【论证主线】论文从一个经验观察出发：基础模型的推理、规划与工具使用能力正在把 agent 的职责从「短、窄、一次性任务」推向「需要维持总体目标、协调多个阶段并把工作做到完成」的更大委托；Codex、Claude Code 一类系统已能支持多日、跨会话的工作，并接管了部分结果评估与反馈驱动修订。与此同时，recursive self-improvement 让「能力提升本身」也进入了 agent 可承担的工作范围。作者由此指出一个结构性错位：真实工作的正确性依赖「相关文件、记录、应用中的证据」与「先前动作建立的状态」，而现有研究缺少一套统一语言来描述「agent 如何可靠完成被委托的工作」。于是本文不做新方法，而是提供一套以工作为中心、系统级的分析装置，并把已有文献重排进这套装置。
【技术主线：四层框架】
第一层是定义：work agent = 基础模型（决策核心）+ runtime harness（组织观察与动作执行）+ 有状态数字工作区，交付可验证结果。这个三元定义把「模型不足」与「harness 不足」在概念上分离。
第二层是工作条件：work persistence（责任跨阶段、跨会话被携带的程度）与 work openness（目标是给定还是在执行中被发现）构成两维坐标；并据此按任务范围与组织方式把数字工作分为五种形态（从返回答案、产出制品、施加状态变更，到编排工作流与持续委托）。
第三层是能力：识别可靠执行所需的五类核心能力族（可见章节痕迹指向规划与协调、长历史与证据访问、动作接口与工具/技能使用、状态维护、验证与修正等；确切枚举需回原文核对）。
第四层是优化：模型优化沿 pre-training → mid-training → post-training 展开，其关键变量是监督粒度（监督决策 / 轨迹 / 产物 / 早前尝试，对应示范、执行奖励、教师指导、累积反馈四类信号）；harness 优化覆盖工具与技能表示、动作接口、证据可达性、状态保留与验证；二者还需联合优化，因为模型与工作同时变化。
【实验主线（本文为综述，无自建实验）】论文的实证性劳动体现在三处：一是对评测版图的系统梳理，把 SWE-bench、SWE-Lancer、Terminal-Bench、ScienceAgentBench、MINT、Mind2Web、AndroidWorld、MCP-Universe、MCP-Bench、TravelPlanner、PaperBench、SpreadsheetBench 等按「工作产出」重新定位，并用时间轴展示 2023—2026 任务条件的系统性变难；二是对训练配方的对照式归纳（Figure 6 的监督粒度示意）；三是对具体案例的引用，例如 Kimi K2 的改写实验（十种不同改写各一次 28.94% vs 原文重复十次 23.76% 的 SimpleQA 准确率）与 AutoDesign 的「渲染—校验—critic」反馈回路。评测层面，论文主张区分「被检查的结果」「被评估的属性」「判定准则」，并以对共享任务要求的独立产出检查保障跨 agent、跨 grader 的可比性。
【结论与边界】作者在结论中把整篇的论断收束为一句话：可靠的数字工作需要把任务要求、动作与产出在工作展开过程中持续连接起来；开放性让发现成为过程的一部分，持久性让责任跨阶段与事件被携带；五类核心能力提供了定位「哪里成功、哪里断裂」的坐标。同时，工作执行反过来提供观测、流程、纠正与反馈，可以改善后续工作与训练。
【证据质量与使用建议】本文是框架性综述，价值在于「坐标系与词汇表」，不在于「性能结论」；它对五形态与五能力族的划分在可见证据中缺少操作化判据与一致性检验，对成本、安全、权限等维度覆盖有限，也没有跨 benchmark 的定量元分析。因此最合适的使用方式是：把它当作设计空间地图与评测口径检查清单，遇到具体技术选择时再回到被引用的一手论文核对细节。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与你画像中的「agent」主方向（权重 0.10）直接重合：本文提供的不是某个 agent 方法，而是 agent 系统的设计空间地图（条件维度、能力族、优化杠杆、评测口径），适合作为选题定位与方案自检的参照系。

## 基本信息

- 作者：Zishan Xu, Enjun Du, Mingqi Gao, Cheng Yang, Junhao Shen, Zirong Chen, Zhiyuan Yao, Yifu Guo, Yuquan Lu, Jinyang Huang, Zhengxi Lu, Siyi Liu, Ziyu Zheng, Yongqi Zhang, Difan Zou, Weiwen Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-27
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.35845`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 语义检索命中的 Abstract、Introduction、Method 与 Conclusion 片段（含 Figure 6 图注、Kimi K2 改写数值、benchmark 时间轴标签与评测三层区分），并按 field_evidence_map 将方法类证据用于 solution_approach/core_method、结论类证据用于 limitations、方法与引言类证据用于 relevance/reading_focus；由于可见证据中不存在 results 章节与完整分类表，五形态与五能力族的确切枚举、以及作者机构均标注为证据缺口而未作编造。
