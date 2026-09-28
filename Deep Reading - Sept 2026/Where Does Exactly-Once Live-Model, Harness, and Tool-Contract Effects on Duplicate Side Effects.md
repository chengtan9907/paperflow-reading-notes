---
user_id: "cheng tan"
paper_id: 13505
arxiv_id: "2609.29095v1"
title: "Where Does Exactly-Once Live? Model, Harness, and Tool-Contract Effects on Duplicate Side Effects in LLM Agents"
publish_date: "2026-09-24"
pdf_url: "https://arxiv.org/pdf/2609.29095v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-26T01:50:25"
---
# Where Does Exactly-Once Live? Model, Harness, and Tool-Contract Effects on Duplicate Side Effects in LLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：exactly-once semantics · llm agents · idempotency keys · fault injection

## 一句话总结

论文用确定性沙盒 LIMBO（六个服务、十二种服务边界故障、25,930 个 episode，覆盖 9 个模型 × 3 个生产级 agent harness × 2 种契约变体 × 15 种恢复条件）系统追问「exactly-once（恰好一次）语义应该落在模型、agent harness 还是工具契约哪一层」，结论是责任归属随故障类型切换：当可以立刻读回真相时由模型决定（前沿模型重复率仅 0.5%，模型解释 53% 的可解释方差），当读回不可能（请求仍在飞行中、或传输层重复投递）时由契约决定（同一批前沿模型重复率升至 56% 与 74%，契约解释 81%），而 harness 几乎不起作用。

## 摘要

> When a tool-using agent's write times out or returns a server error, the action may already have taken effect. Retrying blindly duplicates it -- a second charge, a second announcement, a second deployment -- while giving up skips required work. We ask where exactly-once behaviour should be enforced: in the model, in the agent harness, or in the tool contract. We introduce LIMBO, a deterministic sandbox of six services with realistic contracts (optional idempotency keys, eventually consistent and missing read paths) and twelve fault modes injected at the service boundary, including late commits, redelivery and partial batches; every episode is graded against a ledger of committed effects. Across 25,930 episodes spanning nine recent models, three production agent harnesses, two contract variants and fifteen recovery conditions, the answer depends on the fault. When an immediate read-back can reveal what happened, the model decides: frontier models instructed to act exactly once almost never duplicate a write whose acknowledgement was lost (0.5%), weaker models often do, and the model explains 53% of the explained variance. When it cannot -- the request is still in flight, or the transport delivered it twice -- the same frontier models duplicate in 56% and 74% of episodes, and the contract explains 81%. We prove that no verification-only policy is exactly-once under late commits without a bound on in-flight time. Waiting works when such a bound is short and known, but with heavy-tailed in-flight delays even an hour of waiting per episode falls short of offering an idempotency key on every write, which lowers the duplicate rate from 28% to 4% because agents use keys when they exist. The harness barely matters, a guard that attaches keys transfers across harnesses unchanged, and agents reported success in 90% of the episodes in which they had duplicated an effect.

Q1: 这篇论文试图解决什么问题？

1) 问题本体：工具型 LLM agent 在写操作（扣款、发布、部署）上遭遇超时或 5xx 时，进入典型的「结果未知态」（in-doubt）：请求可能已经提交，也可能从未到达。此时重试造成重复副作用，放弃造成任务漏做。这是一个至少可追溯到分布式系统的经典二难，但论文把它重新表述为一个 agent 系统的责任分配问题。
2) 论文真正要回答的不是「如何实现 exactly-once」，而是「exactly-once 应该在哪一层被强制」。候选层有三个：(a) 模型层——由模型推理决定是否重试、是否先验证、是否带 key 重发；(b) harness 层——由 agent 编排框架的 retry/guard 逻辑统一负责；(c) 工具契约层——由服务端语义决定（是否接受 idempotency key、是否提供可读回路径、read-after-write 是否一致）。这个三分法是论文的核心分析框架，也是它区别于一般「agent 可靠性」工作的关键。
3) 为什么这个问题在 agent 场景下比在传统服务端更棘手：agent 的决策输入是自然语言观察而非事务上下文；一次「工具调用」在语义上可能对应多个后端副作用（例如 partial batch 只提交了一部分）；真实世界服务的契约参差不齐（key 是可选而非强制、读路径最终一致、某些服务干脆没有读路径），因此无法简单假定「服务端会替我们处理」。
4) 评价学缺口：既有 agent 评测几乎都以任务成功率为主指标，无法度量「任务看似成功但副作用被重复执行」。要度量这一点，必须以「已提交效果账本」这类外部地面真值判分，而不是以 agent 的自报结果或最终文本判分——论文的 90% 自报成功数据正说明后者不可靠。
5) 隐含假设与边界（据摘要推断，需回原文确认）：评测基于单一 agent 对单一服务的同步调用；故障由实验者可控地注入在服务边界；服务是沙盒内的合成服务。多 agent 并发、跨服务事务、人工审批链、真实第三方 API 的非确定性行为等是否覆盖，摘要未给出，属于需要在原文核对的边界条件。
6) 该问题的研究价值在于它把一个工程直觉（「加个 idempotency key 就好了」或「换个更强的模型就好了」）转化为可证伪的经验命题，并给出一个理论不可能性结果作为上界约束。

Q2: 有哪些相关研究？

注：本次未取得 PDF 正文，以下为按论文问题框架推断的相关研究脉络，具体引用条目需回原文 Related Work 核对，标注为「合理推断」。
1) 分布式系统语义层：at-most-once / at-least-once / exactly-once 语义划分、幂等请求与 idempotency key 的工业实践、两阶段提交、transactional outbox、saga 与补偿事务、去重窗口（dedup window）、重试与超时语义在不可靠网络下的不可判定性。论文的理论结果（verification-only 在 late commit 且 in-flight 无界时不可能 exactly-once）属于这一传统的直接延伸，可视为把经典结论搬到 agent-工具边界。
2) Agent 与工具使用：ReAct 式推理-行动循环、function calling 接口、工具/API 调用类基准（多以任务完成度为主指标）。这类工作通常把工具报错当作噪声或可重试事件，很少区分「重试安全」与「重试危险」，也不把副作用重复作为一等评测对象——论文的批评点正在此。
3) Agent 鲁棒性与故障注入：prompt 扰动、工具中断、环境噪声下的 agent 稳定性研究，以及分布式系统的确定性模拟与混沌工程（可复现故障注入、账本式一致性检查）。LIMBO 的设计（确定性 + 服务边界注入 + 账本判分）在方法论上更接近后者，而非传统的自然语言鲁棒性评测。
4) LLM 自我验证与不确定性：自一致性、自我反思、验证器/批评者模型、以及「先读回再决定」的策略族。论文用证明说明这一整族 verification-only 策略在特定故障下存在原理性缺口，这构成对该类方法的一个边界性结论。
5) 责任分层与系统设计：agent harness 的工程实践（重试策略、工具包装、guard/中间件）与工具契约设计（幂等端点、客户端生成 key、服务端去重表）。论文的「harness 几乎无关、而 guard 可跨 harness 移植」属于对这一工程直觉的反驳与修正。

Q3: 论文如何解决这个问题？

1) 核心方法不是新算法，而是「可判分的确定性实验台 + 受控因子设计 + 归因分析」。LIMBO 沙盒提供六个服务；契约故意做成不完美：idempotency key 可选而非强制、读路径最终一致、部分服务缺失读路径。这使「模型能否自己判断」与「契约是否提供信息」两个变量可以分离。
2) 故障注入在服务边界完成，共十二种故障模式，摘要明确列举的子集包括 late commit（提交发生但在超时之后才生效）、redelivery（传输层重复投递同一请求）、partial batch（批次只提交了一部分）。其余故障模式的枚举需回原文确认。
3) 判分机制：每个 episode 结束后与「已提交效果账本」比对，统计真实副作用是否被重复、是否被漏做。账本作为外部真值，绕开了 agent 自报成功这一不可靠信号。
4) episode 结构（据摘要推断）：agent 发起一次或多次写调用→注入故障→agent 选择恢复动作（重试、先读回、等待、带 key 重发、放弃等十五种恢复条件之一）→按账本判分。
5) 因子设计：9 个模型 × 3 个生产级 agent harness × 2 种契约变体 × 15 种恢复条件，共 25,930 个 episode。确定性执行保证可复现与可归因。
6) 归因方法：报告「模型解释了 53% 的可解释方差」（读回可行条件下）与「契约解释了 81%」（读回不可行条件下），说明作者做了方差分解/效应量分析。摘要中的 53%/81% 精确口径（是否基于 ANOVA、混合效应模型或条件熵分解）需回原文确认。
7) 理论部分：证明在 late commit 且 in-flight 时间无上界时，不存在 verification-only 策略能保证 exactly-once。该证明为实证结果提供了上界解释：为什么模型再强也无法在不可读回条件下自证。
8) 干预对比实验：一条路线是「等待」——在 in-flight 延迟有短且已知上界时有效；另一条路线是「每条写都携带 idempotency key」；并在重尾延迟下把等待预算拉到每 episode 一小时做压力对比。另有一个「guard 自动附加 key」的实现，用于检验同一机制跨 harness 的可移植性。
9) 需回原文确认的实现细节：六个服务的具体身份与语义、key 的服务端去重语义与窗口、十五种恢复条件的完整枚举、九模型与三 harness 的名单及版本、等待实验中重尾分布的具体参数、以及是否开源。

Q4: 论文做了哪些实验？

1) 规模与因子：25,930 个 episode；自变量包括模型（9 个近期模型）、agent harness（3 个生产级框架）、工具契约（2 种变体）、恢复条件（15 种）。因变量是重复副作用发生率、漏做率，以及方差归因中模型/契约的贡献份额。
2) 条件分层（摘要中最清晰的一条实验切分）：第一类条件允许「立即读回」以揭示真实结果；第二类条件不允许，具体细分为请求仍在飞行中（in-flight）与传输层重复投递（redelivery）两种情形。这一分层是主结论的来源。
3) 模型能力对比：前沿模型在被告知必须以 exactly-once 方式行动时，对「确认丢失的写」的重复率仅 0.5%；弱模型经常重复。这构成「读回可行时由模型决定」的证据。
4) 不可读回条件下的同批模型对照：前沿模型在 in-flight 与重复投递两类 episode 中分别有 56% 与 74% 重复，契约解释 81% 方差。这是在相同模型、相同 harness 下仅改变「信息是否可得」的对照，因此能支撑「责任层随故障类型切换」的论断。
5) 等待策略实验：在 in-flight 延迟有短且已知上界时等待有效；在重尾延迟下，即使每个 episode 等待一小时，仍不如「每次写都提供 idempotency key」。
6) key 覆盖实验：把「每次写都提供 key」与基线对比，重复率从 28% 降到 4%，机制解释是「key 存在时 agent 会使用它」。
7) harness 对照实验：三种生产级 harness 之间结果差异很小；同时测了一个自动附加 key 的 guard，其效果跨 harness 保持不变。
8) 自报一致性测量：在 agent 已经造成重复副作用的 episode 中，90% 的 agent 仍报告成功——这是一个关于评测信号有效性的独立测量。
9) 统计口径细节（置信区间、每个条件单元的样本量、方差分解的具体方法、多次运行的方差）摘要未给出，需回原文确认；因此上述百分比应视为点估计而非带区间的结论。

Q5: 发现了什么实验现象？

1) 主现象：责任归属不是单一答案，而是随故障类型发生相变（phase transition）。同一批模型在「可读回」条件下近乎完美（0.5% 重复），在「不可读回」条件下崩到 56%/74%。这是论文最有信息量的发现：它把「模型能力不足」和「信息在原理上不可得」两种失败机制区分开。
2) 方差归因的反转：读回可行时模型解释 53% 的可解释方差，读回不可行时契约解释 81%。说明干预手段应当随条件切换，而不是「一直换更强的模型」或「一直加 key」。
3) 负结果一（harness 几乎无关）：三个生产级 harness 之间差异很小。对投入大量工程在 harness 层重试/编排逻辑的团队来说，这是一个不利证据。
4) 正结果一（可移植性）：自动附加 idempotency key 的 guard 跨 harness 原样生效，说明契约类干预比 harness 类干预具有更好的迁移性质。
5) 负结果二（等待在重尾下失效）：等待策略依赖「in-flight 时间上界短且已知」这一前提；重尾延迟下每 episode 等一小时仍不达标。这直接反驳「加长超时/多等一会就好了」这一常见工程直觉。
6) 正结果二（key 覆盖是主杠杆）：重复率 28%→4%，且机制是 agent 愿意在 key 存在时使用它——也就是说瓶颈在契约供给，而非 agent 的配合意愿。
7) 测量层面的反直觉：agent 在 90% 已造成重复的 episode 中自报成功。这意味着以 agent 自述或最终答案正确性为指标的评测会系统性漏掉这类故障，也意味着「成功信号」不能用作恢复决策的依据。
8) 理论-实证呼应：verification-only 在 late commit 且 in-flight 无界下不可能 exactly-once 的证明，与 56%/74% 的实证结果方向一致——不是模型不够聪明，而是信息不可得。
9) 指标之间的张力：等待会拉高延迟成本、key 覆盖要求服务端改造、模型侧谨慎在不可读回时无效；三者之间存在成本/可行性上的取舍，论文用 28%→4% 对比 1 小时等待来说明哪一边的更优。

Q6: 有什么可以进一步探索的点？

1) 契约侧的推广与成本：把 idempotency key 从「可选」变成「默认/强制」的可行路径、服务端去重窗口（dedup window）与 TTL 的设计、以及无 key 能力服务的替代方案（补偿事务、可撤销写）。摘要指出 key 覆盖将重复率降到 4% 而非 0%，那剩余的 4% 来自哪里是明确的后续问题。
2) in-flight 时间上界的获取方式：论文的等待策略需要「上界短且已知」；在真实系统中如何为每个服务测量或推断该上界（尾延迟建模、服务端暴露 in-flight 状态、客户端保留请求指纹并轮询）是工程上未解决的问题。
3) 责任归属的自动发现：给定故障类型与契约能力，能否自动决定由模型、harness 还是契约负责？这可以形式化为一个策略选择问题，并与论文的理论结果结合，给出「哪些组合在原理上不可达」的可判定条件。
4) 多 agent 并发与跨服务原子性：摘要未显示覆盖多 agent 同时写同一资源、跨服务事务、以及人工审批链路的情形；这些场景中重复副作用的语义和检测方式都不同。
5) agent 自报成功与真实效果的脱钩（90%）：如何训练或提示模型识别「结果未知态」并主动表达不确定、以及如何在评测中把自报信号替换为账本信号，是一个独立且高价值的方向。
6) 恢复策略的学习化：把「重试/读回/等待/带 key 重发/放弃」当作一个决策问题，用强化学习或策略蒸馏学习按故障类型自适应的恢复策略，并以 LIMBO 作为可判分的训练环境。
7) 评测基础设施的扩展：把 LIMBO 类确定性沙盒推广到更多服务语义（最终一致读、部分批次、乱序提交、多写入者）、更多模型版本与更长时间跨度，以观察该结论是否随模型能力演进而漂移。
8) 与科学/工程工作流的对接：科学计算与实验自动化中的作业提交、仪器控制、数据写入同样是非幂等写操作，适用于同一分析框架，且常缺少可读回路径。
9) 形式化与验证：把「在给定故障模型与契约能力下，某恢复策略是否 exactly-once」做成可机器检验的性质，而不是仅靠实证统计。

Q7: 总结一下论文的主要内容

论文研究的问题是：当工具型 LLM agent 的写操作超时或返回服务端错误时，动作可能已经生效，此时重试会重复副作用（二次扣款、二次公告、二次部署），放弃则会漏做必要工作。作者把这个问题重新表述为一个责任分配问题——exactly-once 行为应当强制在模型（model）、agent harness，还是在工具契约（tool contract）层面？

为回答这一问题，作者构建了 LIMBO：一个确定性沙盒，包含六个服务，契约贴近现实（idempotency key 可选、读路径最终一致甚至缺失），在服务边界注入十二种故障模式，包括 late commit、redelivery、partial batch 等。每个 episode 的判定不依赖 agent 自报结果，而是与「已提交效果账本」比对，从而把真实副作用是否重复、是否漏做变成可测量量。

实验规模为 25,930 个 episode，覆盖九个近期模型、三个生产级 agent harness、两种契约变体、十五种恢复条件。主结论是条件依赖的：当立即读回可以揭示真实结果时，「模型说了算」——被告知必须以 exactly-once 行动的前沿模型几乎不会重复一个确认丢失的写（0.5%），弱模型则经常重复，模型解释了 53% 的可解释方差；当读回不可能时——请求仍在飞行中，或传输层重复投递——同一批前沿模型在 56% 与 74% 的 episode 中重复，此时契约解释 81%。作者用理论补上这一实证图景：证明在 late commit 且 in-flight 时间无上界的情况下，任何只做验证（verification-only）的策略都无法保证 exactly-once，因此这些重复不是「模型不够聪明」而是「信息在原理上不可得」。

在干预层面，论文对比两条路线。其一是等待：当 in-flight 时间有短且已知的上界时等待有效；但在重尾 in-flight 延迟下，即便每个 episode 等一小时，也不如「每次写都提供 idempotency key」。其二是契约改造：当 key 覆盖率提升后，重复率从 28% 降到 4%，机制是只要 key 存在，agent 就会去使用它——瓶颈在供给侧而非 agent 的配合意愿。

论文还给出两个带反直觉色彩的附带发现：一是 agent harness 几乎不影响结果，说明投入在编排层的重试逻辑收益有限；二是「自动附加 key」的 guard 可以跨 harness 原样迁移，说明契约类干预比 harness 类干预具有更好的可移植性。最后，在 agent 已经造成重复副作用的 episode 中，有 90% 的 agent 仍然报告成功，这既说明 agent 自身无法可靠感知副作用重复，也说明以自报成功为指标的评测会系统性漏检这类故障。

总体的论证主线是：把分布式系统的 exactly-once 问题搬到 agent-工具边界，用确定性沙盒与账本判分把「谁该负责」变成可测量命题；得到的答案不是一个统一结论，而是「责任层随故障类型切换」的条件性答案，并配以一个理论不可能性结果和一个工程上更优的契约侧干预。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与画像中的 agent 方向直接重合（权重 0.10）：论文研究对象就是工具型 agent 的写操作恢复行为，属于 agent 系统可靠性这一核心子问题。

## 基本信息

- 作者：Jiapeng Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.SE
- 日期：2026-09-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.29095v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未使用 PDF 检索证据（retrieved_evidence 与 field_evidence_map 均为空，sections 亦为空字符串），全部内容基于标题与摘要重构，方法实现、统计口径与模型/harness 名单等细节均已在对应字段内标注为需回原文确认；此外论文元数据中的 arXiv ID 与发布日期属未来日期，建议一并核实。
