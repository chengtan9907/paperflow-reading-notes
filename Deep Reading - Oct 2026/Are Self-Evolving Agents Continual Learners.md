---
user_id: "cheng tan"
paper_id: 16447
arxiv_id: "2610.25858v1"
title: "Are Self-Evolving Agents Continual Learners?"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.25858v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.25858v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:02:27"
---
# Are Self-Evolving Agents Continual Learners?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：continual learning · self-evolving agents · nonparametric adaptation · plasticity stability tradeoff

## 一句话总结

本文系统研究非参数自进化 LLM 智能体在主题、工作流、领域及复合漂移四种任务流中的持续学习能力，发现三类方法普遍陷入可塑性-稳定性权衡：自我修改方法全局覆盖旧知识，检索增强方法在未见任务上检索到外来记忆并发生检索碰撞，从而证明当前自进化智能体还不是完整的持续学习者。

## 摘要

> Large language model agents are often deployed in environments where new knowledge, tools, and workflows keep arriving. Such an agent is therefore expected to adapt continually, learning new tasks as they arrive without forgetting earlier ones. This resembles the continual learning problem, but at a pace that conventional gradient updates cannot sustain. Nonparametric self-evolving agents are a natural alternative: they adapt by rewriting their own harness, such as prompts, memory, and skills, from task feedback while the model weights stay frozen. We study whether such agents are effective continual learners across four drifting task streams that shift in topic, workflow, or domain (built from BIRD, BFCL, and ScienceWorld). Evaluating three representative methods spanning experience retrieval and self-modification (ReasoningBank, GEPA, and Meta-Harness), we find that although self-evolution improves aggregate performance on most streams, higher plasticity consistently comes at the cost of stability. Specifically, self-modifying methods apply global edits to a shared prompt or workspace, interfering with unseen tasks and overwriting earlier knowledge as the stream shifts. Conversely, similarity-based memory retrieval localizes updates across distant domains, but still retrieves foreign memories on unseen tasks and suffers from retrieval collision when similar queries require conflicting workflows. These findings show that current self-evolving agents remain incomplete continual learners.

Q1: 这篇论文试图解决什么问题？

**核心问题**：论文研究的是非参数自进化 LLM 智能体（self-evolving agents）在动态部署环境中能否成为有效的持续学习者（continual learners）。具体而言，LLM agent 在实际使用时面对的是不断到达的新任务、新工具和新工作流，要求系统在服务用户的过程中持续学习，同时不遗忘早期知识。

**为什么梯度更新不适用**：传统持续学习依赖梯度更新，但 agent 的适应必须发生在交互时间尺度上——每次环境交互都可能产生对后续会话有用的知识，且需要在下一次会话开始时立即生效。如果每轮会话后重新训练 LLM，成本高到不可接受；如果只在小批量最近交互上微调，又会侵蚀已有能力（摘要和引言均指出这一点）。因此需要一种权重冻结、仅修改 prompt/记忆/技能等“harness”的非参数适应方式。

**什么是非参数自进化**：论文给出形式化描述：输入 x 在任务环境中 rollout 得到 episode y∼π_θ(·|x,c)，即 episode 级策略 π_{θ,c}；evolver 可以选择更新模型权重 θ 或 harness c。非参数情形下 θ 固定（权重冻结），evolver 只重写 c，例如 prompt、memory、skills。这种方式避免了梯度更新的计算开销和灾难性遗忘风险，但它的持续学习性质尚未被系统检验。

**研究缺口**：已有的自进化方法（如 ReasoningBank、GEPA、Meta-Harness）往往在单一任务或静态基准上验证，缺乏对连续任务流、特别是任务组之间发生分布漂移时的系统评估。作者指出需要同时考虑两个因素：evolver f 本身的设计，以及任务流的权重调度 w（即不同任务组随时间出现的比例和顺序）。

**任务流中的漂移类型**：论文将漂移划分为四种：(1) 主题漂移（topic drift）：环境和工作流不变，底层主题变化，如不同数据库 schema 和局部约定；(2) 工作流漂移（workflow drift）：环境和工具目录不变，但正确响应所需的工作流改变，如 BFCL-Miss 要求询问缺失参数而 BFCL-Base 禁止多余澄清；(3) 领域漂移（domain drift）：环境本身和动作空间变化；(4) 复合漂移（compound drift）：以上多种漂移组合。任务流采用块结构：每个块内所有更新都由一个活动任务组驱动，块边界切换活动组，且 agent 不知道组标签和块边界，必须在无任务监督下自行推断结构，这对应 task-free continual learning。

**根本挑战**：论文的核心发现是当前自进化方法陷入可塑性与稳定性的两难。自我修改类方法（GEPA、Meta-Harness）通过对共享 prompt 或工作区做全局编辑获得高可塑性，但编辑会全局影响后续所有 rollout，导致新任务的规则干扰尚未见到的任务，并在流转移时压缩或覆盖早期知识。检索增强类方法（ReasoningBank）用相似性检索把更新局部化到相关查询，理论上避免覆盖，但在未见任务上仍会检索到外来记忆，且当不同工作流在 embedding 空间中邻近时发生检索碰撞。这些机制层面的失败模式说明自进化智能体还不是完整的持续学习者。

Q2: 有哪些相关研究？

**经典持续学习**：持续学习研究如何让模型在连续任务上训练而不遗忘旧任务，代表性综述包括 De Lange et al. 2022、Parisi et al. 2019、van de Ven and Tolias 2019。这些工作主要面向分类/监督学习，强调灾难性遗忘的缓解。

**LLM 持续学习**：近年来持续学习成为 LLM 的核心挑战，代表工作包括 Jang et al. 2022、Ke et al. 2023、Luo et al. 2023、Shi et al. 2024、Wu et al. 2024、Shenfeld et al. 2026（引用见论文引言）。这些工作大多仍以梯度更新为主，与本文的非参数 harness 更新路线不同。

**Agentic 环境与基准**：论文涉及的 agent 环境包括：BIRD（Li et al. 2023，文本到 SQL，关系数据库查询）、BFCL（Patil et al. 2025，多轮用户请求经有状态 API 服务）、ScienceWorld（Wang et al. 2022，文本环境中交互式科学实验）。此外还提到操作软件工具和系统的 agent 工作（Yao et al. 2023; Zhang et al. 2025b）以及数据库查询/写代码的用户代理工作（Wang et al. 2025）。

**非参数自进化方法**：论文评估三种代表性方法：ReasoningBank（基于查询相似性的经验检索增强，通过注入记忆更新行为）、GEPA 和 Meta-Harness（均为自我修改方法，重写全局 prompt 或 harness）。这些方法分别代表“经验检索”和“自我修改”两类范式，正好覆盖当前主流自进化路线。

**Task-free continual learning**：论文的任务流设定不允许 agent 知道组标签和块边界，agent 必须自己推断任务结构，这与 task-free continual learning 设定一致（Aljundi et al. 2019; Buzzega et al. 2020; De Lange et al. 2022）。

**与既有研究的区别**：已有自进化 agent 研究多在静态任务集上评估，缺少对漂移任务流的系统性压力测试。本文贡献在于把持续学习评估协议扩展到 agentic 非参数适应场景，并揭示全局编辑与检索碰撞这两类具体失败机制。

Q3: 论文如何解决这个问题？

论文采用“提出评估框架 + 对照实验”的方式研究问题，而非提出新方法。核心要素如下：

**1. 形式化**：定义 episode 级策略 π_{θ,c}，其中 θ 为模型权重，c 为 harness（prompt、记忆、技能等外部配置）。给定输入 x，在任务环境中 rollout 得到 episode y∼π_{θ,c}(·|x)。evolver f 可以更新 θ 或 c；本文研究非参数情形，即 θ 冻结，只更新 c。

**2. 任务流建模**：研究两个因素：evolver f 和任务流的权重调度 w。目标函数记为 J_w(f)（Eq. 6），评估标准是 evolver 相对 seed harness 在该目标上的改进。任务流被实例化为漂移流：权重 w_t 是 one-hot 且分段常数，即任务流由连续块组成，每个块内所有任务来自同一任务组，块边界切换活动组；块内每个更新由活动组驱动，之前组不再收到任务和反馈，后续组尚未出现。

**3. 四种漂移流配置**：主题漂移（T=150, K=3，三个 BIRD 数据库顺序出现）、工作流漂移（T=100, K=2，BFCL-Miss 后接 BFCL-Base）、领域漂移（T=150, K=3，环境变化，证据片段未完整给出其具体构成）、复合漂移（组合多种漂移，证据片段显示包含 BFCL-Miss（domain）→BFCL-Base（workflow）的转换）。每个流始终保持一个共享 harness，不泄露组标签和块边界，agent 不知道自己在漂移流中，也不被要求平衡各组性能。

**4. 评估协议**：对每个任务族 k，在 disjoint hold-out 集 Deval_k 上计算冻结 harness c_t 的准确率 A[t,k] = (1/|Deval_k|) Σ_{x∈Deval_k} E_{y∼π_{θ,c_t}(·|x)} R_k(x,y)（Eq. 7）；seed harness c_0 作为基线。这种协议可以同时观测可塑性（新任务组性能提升）和稳定性（旧任务组性能保持）。

**5. 方法分类与变量控制**：将 ReasoningBank 归为经验检索/增强类，GEPA 和 Meta-Harness 归为自我修改类。所有方法共享同一个冻结基座模型、相同的任务流和共享 harness 维护方式，以此隔离 harness 适应机制的影响。

**6. 诊断分析**：论文不仅报告平均性能，还通过检索记忆来源分析（如 compound drift 中检索到外部任务组的比例）和具体失败案例（如 BFCL-Miss→BFCL-Base 转换中旧策略被覆盖）来揭示失败机制。

Q4: 论文做了哪些实验？

**基准与数据**：实验基于三个 agentic benchmark 构建任务流：BIRD（text-to-SQL，关系数据库查询）、BFCL（多轮函数调用，有状态 API）、ScienceWorld（文本交互式科学实验环境）。每个任务流的每个任务组包含 50 个训练任务，评估集为每组 50 个任务（ScienceWorld 为 90 个）。

**四个漂移流**：
1. 主题漂移（T=150, K=3）：BIRD-F1（formula_1）→ BIRD-Code（codebase_community）→ BIRD-TP（thrombosis_prediction）。环境和工作流固定，底层主题/数据库 schema 变化，需要学习各数据库 schema 和局部约定，避免把旧假设迁移到下一个。
2. 工作流漂移（T=100, K=2）：BFCL-Miss → BFCL-Base。API 目录相同，但 BFCL-Miss 要求 agent 主动询问用户澄清缺失参数，而 BFCL-Base 提供全部所需参数并惩罚不必要澄清，因此正确工作流发生改变。
3. 领域漂移（T=150, K=3）：环境本身及动作空间变化（具体构成从证据片段看未完整给出，合理推断是跨 benchmark 或跨环境的任务组，需原文确认）。
4. 复合漂移：组合多种漂移，证据片段显示包含 BFCL-Miss（domain）→ BFCL-Base（workflow）的转移，要求 evolver 同时处理跨域转换和域内冲突工作流。

**方法与控制**：评估 ReasoningBank、GEPA、Meta-Harness 三种方法，基座模型在所有方法和所有流上保持不变（据 Results 片段“We hold the base model fixed across methods and streams”），以隔离 harness 适应机制的影响。每个流中方法维护一个共享 harness，持续整个时间范围。

**评估指标**：对每个任务族在 disjoint hold-out 集上计算准确率 A[t,k]（Eq. 7），对比 seed harness c_0，观察跨时间点的性能曲线。论文额外诊断检索类方法的记忆来源（foreign task group 比例）和自修改方法的指令覆盖情况。

**统计与可视化**：结果包括随时间变化的性能曲线、复合漂移中检索记忆来自外部任务组的占比热图（对应 Figure 3 相关子图），以及 BFCL-Miss→BFCL-Base 转换中的具体行为分析（例如 seed agent 调用工具的触发时机改变）。具体数值需查阅原文。

Q5: 发现了什么实验现象？

**1. 自进化整体有效但不稳定**：在大多数任务流上，自进化方法相对 seed harness 提升了综合性能，说明非参数适应确实能带来收益；但没有任何方法能在强可塑性同时不损害其他任务组性能，可塑性-稳定性权衡是跨所有方法的一致现象。

**2. 自我修改方法的高可塑性来自全局编辑**：GEPA 和 Meta-Harness 通过重写共享 prompt 获得高可塑性，但该编辑会施加于后续所有 rollout。因此某一任务块学到的规则会全局传播，干扰尚未见到的任务；当流转移到下一个块时，新指令会压缩或覆盖早期知识。最严重的情形发生在连续块共享同一领域但需要冲突工作流时——模型在表面域和工具目录不变的情况下无法识别漂移，会把前一个策略重写成相反策略（对应 Figure 3 相关子图及其周围分析）。

**3. 检索增强方法局部化但受制于相似性**：ReasoningBank 通过查询相似性检索记忆，更新被局部化到相似的未来查询，因此不会覆盖共享 prompt，在远距离领域之间能防止遗忘。但它在任务组首次出现之前就会注入外来记忆（foreign memory）：在未见任务上检索到的记忆有相当比例来自外部任务组（compound drift 的诊断热图显示这一比例，具体数值需查原文），这会造成误导。

**4. 检索碰撞**：当不同任务组要求冲突的工作流，但它们的查询在 embedding 空间中相近时，相似性检索会命中错误记忆，导致行为冲突。这是检索增强路线独有的失败模式，在复合漂移中尤为突出。

**5. 领域漂移与工作流漂移的交互**：在 BFCL-Miss（domain）→BFCL-Base（workflow）转换中，由于表面领域和工具目录没有改变，模型未能识别配置转变，将早期策略直接改写为相反版本；这说明领域连续性和工作流冲突的组合对自修改方法尤其危险。

**6. 两类干扰的时间方向相反**：检索方法的问题不是遗忘，而是“预干扰”——在正确任务组到来之前就把错误记忆注入上下文；自我修改方法的问题则是“后干扰”——前期规则残留影响后期未见任务。两种方向相反的干扰机制共同构成了不完整持续学习的证据。

注：上述第 3、4 点的具体数值论文未在摘要中给出，图表细节需要查阅原文确认；机制判断基于摘要和结论段落的明确陈述。

Q6: 有什么可以进一步探索的点？

基于论文揭示的失败机制，可以探索以下方向：

**1. 局部化自我修改**：设计只影响相关任务子集的 harness 编辑机制，如按任务组/工作流分槽的 prompt 结构、条件化激活的模块、或带版本控制的 prompt 栈，减少全局编辑的干扰。

**2. 漂移检测与块边界推断**：当前 agent 被禁止得知块边界，但若要避免覆盖，必须自动检测任务结构变化。可在 harness 中维护不确定性估计或任务分布签名，在检测到变化时触发保护机制。

**3. 检索去碰撞**：对记忆添加工作流标签或冲突检测，在检索时排除与当前行为冲突的记忆；或使用工作流感知的嵌入空间，拉开冲突工作流的表示距离。

**4. 混合机制**：将检索增强的局部性与自我修改的表达能力结合，例如用检索激活局部规则、用全局 prompt 存储跨任务共享知识，并显式管理两者之间的优先级。

**5. 更丰富的任务流调度**：除 one-hot 分段常数块外，可研究任务组重叠、渐进式漂移、循环出现等更真实的时间结构，以及非平稳权重调度对可塑性-稳定性边界的影响。

**6. 理论刻画**：为可塑性-稳定性权衡建立形式化度量，分析不同 harness 更新机制（全局重写 vs 相似性检索）的容量和干扰上界。

**7. 扩展到其他 agentic 领域**：本文使用 BIRD、BFCL、ScienceWorld 三个基准；可扩展到更多真实部署场景（如代码生成、网络操作、具身控制），并验证结论的泛化性。

**8. 灾难性遗忘缓解技术的移植**：将传统持续学习中的 rehearsal、regularization、dynamic architecture 思想移植到非参数 harness 更新中，例如定期压缩记忆、为旧任务保留固定 prompt 槽等。

**9. 可解释诊断**：开发工具追踪每条规则/记忆的来源与作用范围，帮助定位干扰来源，并自动生成用户可理解的适应日志。

Q7: 总结一下论文的主要内容

论文围绕一个核心问题：非参数自进化 LLM 智能体是否真正具备持续学习能力。作者观察到 LLM agent 的实际部署环境具有动态性——新任务、新工具、新工作流持续出现，而模型权重在训练后冻结；传统持续学习通过梯度更新实现，但 agent 必须在交互时间尺度上适应，无法承担每会话重训的成本，小批量微调又会侵蚀已有知识。因此，权重冻结、仅重写 prompt/记忆/技能等 harness 的非参数自进化成为自然替代。然而这类方法是否能在连续漂移任务流中同时保持可塑性和稳定性，此前缺少系统验证。

论文首先给出形式化框架。给定输入 x，agent 在任务环境中 rollout 得到 episode y∼π_θ(·|x,c)，其中 θ 为模型权重、c 为 harness，由此定义 episode 级策略 π_{θ,c}。evolver 可以选择更新 θ 或 c；非参数情形下 θ 固定，只更新 c。作者认为评估一个 evolver 需要考虑两个因素：evolver 自身 f 和任务流的权重调度 w。任务流被实例化为漂移流：权重 w_t 是 one-hot 且分段常数，即任务流由连续块构成，每个块内所有更新由单一任务组驱动，块边界切换活动组；之前的组不再获得反馈，之后的组尚未出现。这种设定对应 task-free continual learning 的经典场景：agent 不知道组标签和块边界，必须自行推断任务结构并编码进共享 harness。

基于此框架，作者从三个 agentic benchmark 构造四个漂移流，隔离三种基本漂移及组合：主题漂移（BIRD 三个数据库顺序出现，环境与工作流不变，但 schema 和局部约定变化）、工作流漂移（BFCL-Miss 后接 BFCL-Base，API 相同但正确工作流从主动澄清变为禁止多余澄清）、领域漂移（环境与动作空间变化）、复合漂移（组合多种漂移，证据显示含 BFCL-Miss（domain）→BFCL-Base（workflow）的跨域+冲突工作流转换）。每个任务组有 50 个训练任务（ScienceWorld 评估集 90 个），每个流中方法维护一个共享 harness，保持基座模型固定。评估指标是各任务族在 disjoint hold-out 集上的准确率 A[t,k]（Eq. 7），相对于 seed harness 的表现定义为改进目标 J_w(f)（Eq. 6）。

实验比较三类代表性方法：ReasoningBank（经验检索增强）、GEPA 和 Meta-Harness（自我修改）。核心发现可以归纳为三个层面。第一，自进化在多数旅程上提升综合性能，但没有任何方法能实现强可塑性而不损失其他任务组，可塑性-稳定性权衡是跨方法普遍存在的。第二，自我修改方法的高可塑性来自对共享 prompt/工作区的全局编辑；因为编辑对所有后续 rollout 全局有效，某块学到的任务特定规则会干扰未见任务，并在流转移时被后续块压缩或覆盖。论文特别指出，最严重的失败发生在连续块共享同一领域但要求冲突工作流的场景：由于表面领域和工具目录不变，模型无法识别漂移，把旧策略重写成相反版本（对应 BFCL-Miss 到 BFCL-Base 的转换）。第三，检索增强方法通过查询相似性局部化更新，能避免覆盖共享 prompt 并防止远域遗忘，但它在任务组首次出现之前就会把外来记忆注入上下文——在未见任务上检索到的记忆有相当比例来自外部任务组（compound drift 的诊断热图量化了这一比例）。更本质的是检索碰撞：当不同任务组需要冲突工作流且查询在 embedding 空间中接近时，相似性检索会命中错误记忆。两种机制方向相反：自我修改是“全局编辑的前向干扰+旧知识覆盖”，检索增强是“外来记忆的前置注入+相似性碰撞”，但都使方法无法成为完整的持续学习者。

最后论文给出结论：当前非参数自进化 agent 仍是“不完整的持续学习者”。该结论具有方法学价值：它把持续学习的可塑性-稳定性框架成功迁移到 agentic 非参数适应场景，并提供了可操作诊断工具（记忆来源分析、行为案例分析），为后续设计局部化更新、漂移感知和检索去碰撞机制提供了明确靶点。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文直接涉及 agent 研究方向（用户画像权重 0.10）：其核心对象是 LLM agent 的非参数持续适应机制，与 agent 开发与评估高度相关。

## 基本信息

- 作者：Xiaoyin Chen, Yizheng Huang, Yinglun Xu, Ziyun Zhao, Emmett McQuinn, Julie Liu, Hope Yao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.25858v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 7 处证据片段（Introduction、Method、Results、Conclusion），结合 arXiv 元数据与摘要进行归纳；图表具体数值、领域漂移流的完整构成等证据不足处已在相应字段标注为待原文确认。
