---
user_id: "cheng tan"
paper_id: 21792
arxiv_id: "2611.18533"
title: "ModHarness-RL: Recasting Harnesses as Learnable and Evolving Programs for Agentic RL"
institution: "The Pennsylvania State University；LinkedIn；University of California, Santa Barbara"
publish_date: "2026-11-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.18533.pdf"
pdf_url: "https://arxiv.org/pdf/2611.18533"
abs_url: "https://arxiv.org/abs/2611.18533"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-16T23:18:11"
---
# ModHarness-RL: Recasting Harnesses as Learnable and Evolving Programs for Agentic RL

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic reinforcement learning · harness learning · context compression · behavior steering

## 一句话总结

论文提出 MODHARNESS-RL，把智能体强化学习中的 harness 重构为可由数据学习、并随策略训练而演化的程序，并借助结构化状态上的压缩程序和引导程序，在多数设置下提升 held-out 表现并降低上下文开销。

## 摘要

> 论文关注 agentic RL 中的 harness：它是模型与外部环境之间的工程层，决定模型看到哪些交互历史、收到什么行为引导。现有方法通常在整个训练过程中使用手工编写且固定的 harness，但有效 harness 依赖任务、数据、模型与上下文预算，并会随模型训练而变化。MODHARNESS-RL 将交互记录为结构化 STATE（一组带类型的块），并把 harness 的两个核心部件写成作用在这些块上的程序：COMPRESSION 负责把累积历史组装成模型上下文，STEERING 在行为偏离时提供引导。训练中，系统从近期轨迹提出候选程序，通过执行验证，并在过去轨迹上回放，使 harness 跟踪模型行为变化。作者在 CodeSearch 上做深入实验，并在 ALFWorld 与 BIRD SQL 上分析，模型规模覆盖 1.5B 到 8B；两个机制在多数设置下优于固定 harness 基线。结论是 harness 可以从任务数据中学习，并随模型演化。

Q1: 这篇论文试图解决什么问题？

一、核心问题：论文要把 agentic RL 中的 harness 从“手工编写、训练全程固定”的工程层，改造成“可从数据学习、并随策略演化”的程序对象。harness 在这里指模型与外部环境之间的脚手架，决定模型在每个回合看到哪些交互历史、以什么形式看到，以及在行为偏离时收到什么引导。论文的判断是：agentic RL 的瓶颈不只在 policy，也在 policy 周围的观察与引导方式。
二、缺口一：有效 harness 依赖任务、数据、模型和上下文预算，手工为每个配置调 harness 不可扩展。论文在 Introduction 中明确说，现有 agentic RL 通常在特定任务上手写 harness 并保持固定；但同一个 harness 很难同时适配不同领域、不同模型规模与不同 context budget。于是论文主张 harness 应从数据中推导，而不是靠人工试错。
三、缺口二：有效 harness 会随模型训练而变化。RL 会自行解决一部分失败，如果 harness 预先写死，就可能继续对已经消失的失败施加引导，或者无法响应训练中出现的新失败。论文由此提出一个重要的设计原则：新的 harness 组件只应针对“持续存在”的失败加入。这个原则把 harness 演化从一次性设计变成训练过程中的在线调整。
四、论文的具体提问：如何让 harness 可从数据优化，并与模型一起优化？这要求 harness 不只是自然语言 prompt，而是可执行、可验证、可比较的对象；否则无法在训练循环中自动提出候选、检查候选、评估候选。论文的答案是结构化 STATE 加两个程序：COMPRESSION 负责组装上下文，STEERING 负责行为引导。
五、隐含假设与可能反例：第一，假设失败可以归因于 harness；若失败主要来自策略能力不足或环境不可解，steering 可能无效甚至干扰。第二，假设过去轨迹回放能预测未来行为；但策略在训练中变化，回放分布会偏移。第三，假设执行验证能保证候选程序有效；执行成功只说明程序能跑通，不说明它压缩掉了关键信息或引导语义正确。第四，假设程序化 harness 的收益大于搜索与验证成本；论文用“回放不需要模型调用”降低这部分成本，但候选提出与验证仍可能有开销。第五，假设压缩的收益主要在预算受限时；若上下文充足，压缩可能只节省 token，甚至因信息损失略降分数。
六、评价难点：这类工作的核心不是单一分数，而是 score、prefill token、调用次数、单轮时延之间的多目标权衡。固定 harness baseline 是否充分调优、不同基线的搜索预算是否对齐、公共基准是否被预训练污染、held-out 任务是否足以测出泛化，都会影响结论。论文用三个环境、两种预算和两个模型规模建立信号，但 CodeSearch 研究最深，ALFWorld 与 BIRD SQL 主要验证单个程序的泛化，这是理解其结论边界的关键。

Q2: 有哪些相关研究？

一、Agentic RL 与多轮工具使用。论文把 LLM agent 视为通过多轮交互与环境求解任务的策略，agentic RL 直接用可验证的环境奖励训练这类策略（Yao et al., 2023; Liu et al., 2024b; Jin et al., 2025; Wei et al., 2025; Da et al., 2025）。片段还提到 Wang et al., 2025 与 Feng et al., 2025 这一多轮工具策略/可验证结果的研究群，以及 Toolformer、ToolLLM、ToolRL 这类工具获取工作。论文与它们的区别不在于提出新的 RL 损失，而在于把训练时围绕模型的 harness 也变成优化对象。
二、上下文压缩、记忆与摘要。一条相关线把不断增长的交互 transcript 压缩：memory、summarization、pruning（Packer et al., 2024; Wu et al., 2025; Zhang et al., 2025; Bashir et al., 2026; Shao et al., 2026）。这些方法通常把历史当自由文本处理，目标是“少放一点”或“摘要一下”。MODHARNESS-RL 的 COMPRESSION 则把历史记录为带类型的 STATE 块，再写程序去选择、过滤、重排和组装；论文强调确定性程序还能保留 prefix-cache 复用，因此效率评估不能只数 prompt token。
三、行为引导、接口与 guardrails。另一条线通过额外指令、接口设计或 guardrails 引导 agent（Yang et al., 2024; Xia et al., 2025）。这类工作多是静态的、任务手写的约束。MODHARNESS-RL 的 STEERING 把引导写成作用在 STATE 上的程序，并让它在训练中随策略演化；触发条件是“行为偏离”，而不是每轮固定注入提示。
四、可学习的 harness、prompt、skill 与 memory 基线。论文把 memory、skills、prompts 和 harness 本身各取一个代表，作为能与 policy 一起学习的基线；其中 EvolveR（Wu et al., 2026）是 harness 演化方向的代表。论文的差异点有三层：把 harness 表示成程序而非自由文本；把 harness 拆成 COMPRESSION 与 STEERING 两个可分别消融的部件；用“候选提出—执行验证—历史回放”的循环让 harness 跟踪策略变化。
五、相邻但片段未展开的研究线。按方法形态，这项工作还与程序合成、神经符号方法、自我修改 agent、AutoML/超参搜索、编译器优化有相似性：都是在一个可执行空间中搜索、验证、回放和选择。但提供的片段没有把论文与这些线做系统对照，因此这里只能作为合理推断的邻近关系，不能当作论文明确的相关工作。
六、定位小结：与 memory 方法相比，它同时管理上下文与行为引导；与 prompt optimization 相比，它优化的是可执行程序与结构化状态；与 harness evolution 基线相比，它强调程序化表示、执行验证和历史回放。论文的新颖性主要在这个问题表述与循环设计，而不是单个压缩或提示技巧。

Q3: 论文如何解决这个问题？

一、总体框架。MODHARNESS-RL 把一次多轮交互记录为结构化 STATE：一组带类型的块，而不是一整段自由文本 transcript。harness 的两个核心部件都是作用在这些块上的程序：COMPRESSION 把累积历史组装成模型当前上下文；STEERING 在检测到行为偏离时提供引导。训练时，系统从近期轨迹提出候选程序，先做执行验证，再在过去轨迹上回放，以估计候选是否值得保留。于是 harness 不是训练前写死的，而是随策略行为变化而更新。
二、STATE 的作用。类型化块让程序可以按类型查询、选择、过滤、重排、截断或摘要，而不是在字符串上做不可验证的编辑。这为“程序化 harness”提供了接口：程序看到的是结构化记录，输出的是模型可读的上下文或引导。论文片段没有给出完整的块类型表，但可以合理推断至少包含观察、动作、工具输出、错误、计划等交互要素；具体 schema 需回原文确认。
三、COMPRESSION。它负责 context assembly：从不断增长的 STATE 中选择哪些块进入模型上下文、以什么顺序和粒度呈现。论文说 COMPRESSION 是“learned once”，即只学一次，不随 policy 每步演化。一个合理的设计解释是：上下文组装策略相对稳定，而 steering 更依赖策略当前行为；但论文片段没有展开这个设计理由。COMPRESSION 的确定性很重要：确定性程序在历史重写时更容易保留前缀稳定，从而利用 prefix cache；这也是论文用 prefill token 而非 prompt token 衡量效率的原因。
四、STEERING。它负责在行为偏离时提供 guidance。与每轮固定加提示不同，STEERING 是程序：它读取 STATE，判断当前行为是否偏离，再决定注入什么引导。论文说 STEERING 随 policy 演化，因为策略变化会改变失败模式；只对持续失败加组件，可以避免 harness 越来越臃肿。片段提到论文对每个 STEERING 组件做消融，说明 steering 内部可能由多个可拆分的引导部件组成；具体部件名称与触发逻辑未在片段中完整出现。
五、候选程序—执行验证—历史回放。候选程序从近期轨迹中提出；提出后先在记录的状态上执行，检查是否可运行、输出是否合法；再在过去轨迹上回放，估计它对这些历史失败的影响。回放不需要模型调用，因此即使 reservoir 变大，计算上也相对便宜；论文用回放曲线观察每个失败在训练步上的出现频率。这个循环的关键是：它把“harness 是否有效”变成可离线比较的信号，而不是每改一次 harness 就重新跑完整 RL。
六、与 RL 的结合。检索片段显示论文用 GRPO（Shao et al., 2024）优化目标；但片段没有给出完整目标函数、奖励设计、harness 演化与策略更新的交替频率、以及候选程序的信用分配。可以确认的是，policy 在环境中用可验证奖励训练，harness 的候选则用执行与回放筛选；两者共享同一套交互记录。
七、设计权衡。程序表达力越强，搜索空间越大、验证越难；压缩越激进，token 越省但信息损失风险越高；steering 越强，越能纠正失败但越可能干扰已学会的行为；演化越频繁，越能跟踪策略，但越容易过拟合近期轨迹。论文的“多数设置提升”说明这些权衡在其实验范围内可控，但没有给出理论保证。
八、证据缺口。提供的片段没有展示程序语言、候选生成器（是否由 LLM 提出）、验证器的具体检查、候选选择准则、回放重采样方式、以及 COMPRESSION 只学一次的实现细节。这些是精读时应优先回原文 Method 与 Appendix 核对的部分。

Q4: 论文做了哪些实验？

一、环境与数据。论文在三个 agentic 环境上评估。CodeSearch（Sutawika et al., 2026）：agent 在真实代码库中定位 bug，输出要修改的 file、module、entity；训练用 SWE-Smith，评估用 SWE-Bench Lite、Verified、Pro，指标是三个名称的平均 F1，范围 [0,1]。BIRD SQL（Li et al., 2023）：agent 查询真实数据库回答自然语言问题；训练用 BIRD train split，评估用 dev split。ALFWorld（Feng et al., 2025）：文本家务环境，agent 通过动作序列完成任务；训练用 training games，评估 held-out seen 与 unseen games。三个环境都强调评估任务未在训练中出现；论文还从训练 split 中留出 D_holdtrain，并保证它与所有评估集不相交。
二、模型与预算。模型覆盖 1.5B 到 8B：片段中出现 Qwen3-4B、Qwen3-8B、Qwen2.5-1.5B、Qwen2.5-7B。每个环境在两种 context budget 下评估：limited（上下文预算会在 episode 中途耗尽）与 sufficient（预算很少被触及）。这两个预算对应论文的核心权衡：上下文稀缺时看分数，上下文充足时看 token 与效率。
三、实验臂。所有方法共享环境、policy 与训练配置，只改变模型周围的东西。Base harness 是手写脚手架，训练全程不变；+COMPRESSION 只加一个学到的压缩程序；+STEERING 只加一个学到的引导程序；MODHARNESS-RL 同时运行两者，其中 COMPRESSION 学一次，STEERING 随 policy 演化。基线按“可与 policy 一起学习的组件”各取代表：memory、skills、prompts 和 harness 本身；EvolveR（Wu et al., 2026）是 harness 演化代表。效率对照还使用 GPT-based compaction，即用前沿模型做上下文压缩。
四、指标。性能用各 benchmark 的 score；效率用 prefill tokens、调用次数、单轮毫秒数。prefill token 的定义很关键：不是简单数 prompt token，而是 prompt token 乘以 prefix cache miss 的比例，再在 episode 上求和，并通过回放记录 episode 来测量。论文明确指出，若只数 prompt token，会偏向任何重写历史的方法，因为重写会破坏 prefix-cache 复用；因此必须把缓存未命中算进去。
五、研究问题。片段说论文用三个 research questions 研究两个 harness 模块；从 Discussion 可确认至少包括：COMPRESSION 带来的性能与效率增益；STEERING 及其各组件的消融；以及两个模块在不同环境、预算、模型规模下的行为差异。具体 RQ 措辞未在片段中完整给出。
六、证据缺口。提供的 Results 片段没有完整表格，且 Table 2 的 OCR 数值有明显错位（例如 calls、tokens、ms/turn 的列对齐异常），因此不宜引用精确数值。片段的结论性表述是：两个机制在多数设置下优于固定 harness；CodeSearch 上 full method 超过 memory、skill、prompt、harness evolution 基线；ALFWorld 与 BIRD SQL 用来测试各程序能否泛化。完整基线调参预算、随机种子、显著性检验、训练算力等信息需要回原文确认。

Q5: 发现了什么实验现象？

一、总体效果是“多数设置提升”，不是一致提升。摘要与结论都说 COMPRESSION 和 STEERING 在多数设置下超过固定 harness baseline；这暗示存在未提升甚至回退的设置。可辨识的表格片段也显示，某些 context-sufficient 设置中，COMPRESSION 的分数略低于 GPT compaction（例如 CodeSearch 8B 与 ALFWorld 7B 的片段数值），但在 context-limited 设置中分数更高或接近。这个模式与论文的定位一致：压缩的首要收益在预算紧张时体现为分数，在预算充足时体现为成本。
二、预算依赖性是最重要的实验现象。Context-limited 时，历史会中途溢出，COMPRESSION 通过程序化选择与压缩把关键信息留在上下文里，因此提升 held-out score；context-sufficient 时，历史很少触顶，压缩对分数的边际收益下降，价值转向减少 prefill token。结论段明确概括为“raising the score when context is scarce and reducing prefill tokens when it is not”。这句话是理解整篇论文实验设计的钥匙。
三、效率差异非常大。Discussion 比较 COMPRESSION 与 GPT-based compaction，并用 prefill token 而非 prompt token 计量。片段显示 COMPRESSION 的调用次数、token 与单轮耗时都大幅低于 GPT compaction，部分指标达到数量级差异；但 Table 2 的 OCR 错位使具体数值不可靠。可引用的文字结论是：在 limited budget 下两臂在最后一次模型调用上的差异在 3% 以内（具体指标需回原文核对）；在 sufficient budget 下，COMPRESSION 在 ALFWorld 上少用约 74% token，在 CodeSearch 上少用约 10% token。这个差异说明域结构影响压缩收益：ALFWorld 的长交互历史可能更容易程序化压缩，CodeSearch 的上下文可能已经较紧凑。
四、CodeSearch 上的基线比较是最强的证据。片段说 full method 在 file、module、entity 三个层级上都优于基线（Table 5），并且超过 zero-shot GPT-5-mini——也就是“它的 harness 的创建者”——在每一个设置中，因此增益不能解释为从创建者模型蒸馏。这个论断很重要：如果 harness 由强模型生成，学生模型可能只是模仿创建者的输出；论文用 zero-shot 对照削弱了这种解释。不过，具体 baseline 列表、调参公平性和统计显著性仍需看表。
五、跨环境泛化是“单程序泛化”而非 full method 全面对比。ALFWorld 与 BIRD SQL 用来测试 COMPRESSION/STEERING 能否迁移到其他环境，覆盖两种预算与两个模型规模；但结论的 Limitations 明确说，理想情况下 full method 与 baseline 也应覆盖 ALFWorld 等环境，而当前没有。因此这两个环境支持“各程序有泛化信号”，不足以支持“full method 在所有环境全面优于所有基线”。
六、消融与失败曲线。片段提到对每个 STEERING 组件做消融，说明 steering 不是单一提示，而是可拆分的多个引导部件；不同域上两个程序的作用不同，CodeSearch 上 STEERING 的具体效果被截断，需回原文。另一个有意思的现象是 failure replay：系统记录每个失败在训练步上的出现频率，回放不需要模型调用，因此可以廉价地观察失败是否持续。这个曲线既是分析工具，也是“只对持续失败加组件”这一原则的操作化证据。
七、指标间张力。Score、调用次数、prefill token、ms/turn 不是同向指标：GPT compaction 可能在某些设置分数更高，但调用与 token 成本高得多；COMPRESSION 可能分数略低，但成本低一个数量级。论文没有把多目标合成单一分数，而是分预算讨论，这是合理的，但也意味着“哪个方法更好”取决于部署约束。另一个张力是压缩率与信息损失：压缩越强，token 越少，但关键实体/错误信息可能被丢掉，CodeSearch 的 entity-level F1 正是检验这种损失的敏感指标。
八、证据可靠性。提供的 Results/Discussion 片段有表格 OCR 错位、列名与数值混杂、句子截断等问题；因此本笔记只保留可被文字结论支持的观察，不把表格里的具体数字当作精确事实。需要回原文核对：哪些设置提升、哪些回退、提升幅度、显著性、以及 GPT-5-mini 对照的具体协议。

Q6: 有什么可以进一步探索的点？

一、把 COMPRESSION 也做成持续演化，而不是只学一次。当前设计是 COMPRESSION learned once、STEERING evolving；一个自然问题是：当策略变化导致历史分布变化时，压缩程序是否也需要更新？可以研究压缩程序的周期性重学、分层压缩（turn-level 与 episode-level）、以及压缩率随预算动态调整。
二、联合优化 policy 与 harness，而不是交替启发式。论文的循环是候选提出—验证—回放，但信用分配仍不清楚：一个更好的分数应该归功于 policy 还是 harness？可以引入反事实回放、shapley 式归因、或把 harness 选择纳入 RL 目标，研究联合优化的稳定性与收敛性。
三、更丰富的程序表示与验证。当前程序作用在 typed STATE 上；可以探索层级程序、带类型的 DSL、可组合的压缩/引导算子、以及从自然语言规范合成程序。验证器也应从“可执行”升级为“语义约束”：例如必须保留错误行、必须保留实体名、引导不能泄露答案。可以引入静态分析、单元测试、形式化不变量与沙箱执行。
四、扩展到更多环境与任务形态。论文覆盖 CodeSearch、ALFWorld、BIRD SQL，模型 1.5B–8B；可以扩展到多模态 agent、多智能体协作、长程科研工作流、开放域网页任务、具身环境、以及不可验证或延迟奖励任务。连续 context budget、不同缓存策略、不同推理后端也值得系统研究。
五、AI-for-science 的应用。科学 agent 的 harness 往往包含领域协议、数据 schema、工具调用规范与安全约束；可以把 COMPRESSION 用于实验记录/文献证据的压缩，把 STEERING 用于偏离实验协议时的纠偏。需要验证的是：程序化 harness 能否学会领域约定，而不是过拟合到特定 benchmark；以及错误压缩是否会导致不可追溯的科学结论。这是方法迁移的合理推断，论文尚未验证。
六、系统与安全。生成程序在交互状态上执行，需要沙箱、权限最小化、审计日志与回滚机制；还应研究候选程序被奖励黑客利用的风险，例如 steering 程序学会注入答案或绕过工具限制。可以把程序验证与安全策略结合，形成“可执行但不可越权”的 harness。
七、评估与理论。需要更多负结果、失败案例与跨域迁移实验；需要报告程序搜索成本、候选数量、验证失败率、回放偏差、以及不同 proposer 模型的影响。理论上可以研究 harness 演化的单调改进条件、泛化界、以及 harness 与 policy 的 co-evolution 稳定性。
八、跨领域类比。程序化 harness 与编译器优化、AutoML、程序合成、控制律设计有相似结构：在可执行空间中搜索、验证、回放、选择。可以把这些领域的经验（例如代价模型、搜索空间剪枝、分层搜索）引入 agentic RL 的 harness 学习。

Q7: 总结一下论文的主要内容

一、论文定位与问题。MODHARNESS-RL 研究的是 agentic RL 中常被当作工程细节的 harness：模型与外部环境之间的脚手架，决定模型看到哪些交互历史、以什么形式看到、在行为偏离时收到什么引导。论文指出，现有 agentic RL 通常在特定任务上手写 harness，并在训练全程固定；但有效 harness 依赖任务、数据、模型和上下文预算，而且会随模型训练而变化。于是论文提出两个问题：harness 能否从数据中学习？能否与模型一起演化？
二、方法主线。论文把一次交互记录为结构化 STATE，一组带类型的块；把 harness 的两个核心功能写成作用在这些块上的程序。COMPRESSION 负责把累积历史组装成模型上下文；STEERING 在行为偏离时提供引导。训练时，系统从近期轨迹提出候选程序，先执行验证，再在过去轨迹上回放，以估计候选对失败的影响；回放不需要模型调用，因此可以廉价地跟踪失败随训练步的变化。COMPRESSION 只学一次，STEERING 随 policy 演化；论文用 GRPO 优化策略（检索片段提到），但完整目标与调度需回原文。
三、实验主线。论文在三个环境上评估：CodeSearch（SWE-Smith 训练，SWE-Bench Lite/Verified/Pro 评估，file/module/entity 平均 F1）、BIRD SQL（BIRD train 训练，dev 评估）、ALFWorld（training games 训练，seen/unseen 评估）。模型覆盖 Qwen3-4B、Qwen3-8B、Qwen2.5-1.5B、Qwen2.5-7B，每个环境在 context-limited 与 context-sufficient 两种预算下测试。实验臂包括 Base harness、+COMPRESSION、+STEERING、full MODHARNESS-RL；基线覆盖 memory、skills、prompts、harness evolution（EvolveR）以及 GPT-based compaction。效率指标用 prefill token（计入 prefix cache miss）、调用次数与单轮时延。
四、主要发现。两个机制在多数设置下优于固定 harness；收益呈预算依赖：上下文稀缺时提升分数，上下文充足时主要减少 prefill token。COMPRESSION 相对 GPT compaction 大幅降低调用与 token 成本；文字结论提到 limited budget 下两臂在最后一次模型调用上的差异在 3% 以内，sufficient budget 下 ALFWorld 少用约 74% token、CodeSearch 少用约 10% token。CodeSearch 上 full method 超过 memory、skill、prompt、harness evolution 基线，并超过作为 harness 创建者的 zero-shot GPT-5-mini，说明增益不是简单蒸馏。ALFWorld 与 BIRD SQL 提供单程序泛化信号，但没有同等覆盖 full method 与全部基线。
五、贡献与边界。论文的主要贡献是问题表述与循环设计：把 harness 从手工固定层变成可学习、可验证、可回放、可演化的程序对象，并给出 STATE + COMPRESSION + STEERING 的分解。边界也很清楚：三个环境、1.5B–8B 模型、两种预算；CodeSearch 最深，其他环境较浅；执行验证不等于语义正确；历史回放有分布偏移；程序搜索与沙箱安全未充分展开；没有理论保证。
六、阅读定位。这不是一篇提出新 RL 损失的工作，而是一篇把 harness engineering 转成 harness learning 的系统性方法论文。它的价值在于把“模型看到什么、如何被引导”变成可优化对象，并用 score–token–时延的多目标视角评估；它的风险在于程序化 harness 的搜索空间、验证可靠性与跨域泛化仍需更多证据。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：把 harness 作为 agentic RL 的可学习组件，适合关注多轮工具使用、上下文工程与训练-推理协同的研究者。

## 基本信息

- 作者：Zelin He, Siyu Zhu, Jacob Helwig, Yujian Liu, Li An, Rohit Jain, Shiyu Chang, Alborz Geramifard
- 机构：The Pennsylvania State University；LinkedIn；University of California, Santa Barbara
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-11-16
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.18533`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的摘要、引言、结果、讨论与结论片段，并对方法细节和表格数值的证据缺口作了显式标注。
