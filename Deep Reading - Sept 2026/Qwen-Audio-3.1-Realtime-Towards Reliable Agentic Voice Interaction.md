---
user_id: "cheng tan"
paper_id: 12650
arxiv_id: "2609.25176v1"
title: "Qwen-Audio-3.1-Realtime: Towards Reliable Agentic Voice Interaction"
institution: "推测为阿里巴巴通义千问（Qwen）团队——依据是标题中的 Qwen-Audio 命名与作者署名中出现的 Qwen 系列语音团队成员；但提供的元数据中 institution 字段为 null，本次也未获得 PDF 正文（sections 全空），因此该推断未经原文机构信息确认，需回原文首页核对。"
publish_date: "2026-09-21"
pdf_url: "https://arxiv.org/pdf/2609.25176v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-24T01:08:15"
---
# Qwen-Audio-3.1-Realtime: Towards Reliable Agentic Voice Interaction

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：real-time voice assistant · speech language model · agentic tool use · on-policy distillation

## 一句话总结

Qwen-Audio-3.1-Realtime 用 Think（Core-Cocktail 监督微调 + 多模态多教师在线策略蒸馏 M²-OPD）、Act（自演化可执行环境 + 多粒度 rollout 的 GRPO）与 Speak and Coordinate（对齐说话与否、何时说、怎么说）三层机制，把实时语音助手从“能听会说”推进到“能推理、能调工具、懂对话规则”的可靠语音智能体；相对 Qwen-Audio-3.0-Realtime，它在 τ-Voice 半双工语音改造版上把整体任务成功率从 78.4% 提到 82.0%，在语音到语音的 Full-Duplex-Bench v1.5 上把对背景语音的应答率从 73.0% 压到 13.0%，并附赠一个以 3.0 为前台、通过前景-背景协调与记忆支持持久任务的 Voice Harness 原型。

## 摘要

> Real-time voice assistants must reason over evolving requests, execute actions, and follow conversational rules. Qwen-Audio-3.1-Realtime brings these requirements together through Think, Act, and Speak and Coordinate. Think combines Core-Cocktail supervised fine-tuning with Multimodality and Multi-Teacher On-Policy Distillation (M$^{2}$-OPD) to transfer language capabilities and develop native audio skills. Act uses self-evolving executable environments and multi-granularity rollouts for Group Relative Policy Optimization (GRPO), teaching the model to use tools, interpret feedback, and complete tasks. Speak and Coordinate aligns how, when, and whether the assistant speaks or acts. We evaluate audio reasoning, multilingual understanding, tool use, conversational behavior, full-duplex interaction, and safety. Compared with Qwen-Audio-3.0-Realtime, 3.1 raises overall task success from 78.4% to 82.0% on our half-duplex speech-to-text adaptation of $τ$-Voice. On speech-to-speech Full-Duplex-Bench v1.5, the response rate to background speech falls from 73.0% to 13.0%. We also present a separate Voice Harness prototype, using Qwen-Audio-3.0-Realtime as its foreground, that extends spoken interaction to persistent tasks through foreground--background coordination and memory.

Q1: 这篇论文试图解决什么问题？

一、论文自我声明的问题（摘要直接支持）
实时语音助手（real-time voice assistant）被同时施加三项要求：1）对不断演变的请求做推理（reason over evolving requests）；2）执行动作（execute actions）；3）遵守对话规则（follow conversational rules）。论文的立论是：现有系统无法把这三者“bring together”，因此需要一个把三者统一在同一模型里的方案。注意这是一个能力集成型问题陈述，而非单点算法缺陷——它默认三项目标之间存在工程与训练目标上的相互挤压。

二、三项目标之间的内在张力（基于摘要的合理推断）
1. 语言能力 vs 原生音频能力：Think 的表述是“transfer language capabilities and develop native audio skills”，暗示基线语音模型的语言/推理能力弱于其文本主干，而直接改成端到端音频又会损失语言侧的先验；因此需要在“蒸馏迁移”与“原生音频技能习得”之间做配比。
2. 会说话 vs 会行动：Act 要求模型在语音流中调用工具、读回反馈、继续推进任务，这要求语音模型具备多轮状态跟踪与函数调用格式的稳定性；而语音交互的时间尺度（用户边说边改口）比文本更苛刻。
3. 该说 vs 不该说：Speak and Coordinate 处理“how, when, and whether”三问，对应“怎么说、何时插话、以及是否应该改为执行动作而非说话”。这与全双工场景中的 barge-in、背景人声、旁路对话直接冲突：一个“总是响应”的模型在自由对话里显得灵敏，但在背景语音存在时表现为严重误触发。摘要给出的 73.0% → 13.0% 数字正说明这一冲突在 3.0 上并未被解决。

三、评测层面的问题
论文必须同时评估音频推理、多语言理解、工具使用、对话行为、全双工交互、安全六类能力，说明作者认为单一基准无法刻画“可靠”语音智能体；这也意味着论文需要自建或改造基准（把 τ-Voice 从文本工具调用对话改造成半双工语音版即为一例，摘要明确写了 “our half-duplex speech-to-text adaptation of τ-Voice”）。

四、隐含假设与未明说边界
1. “可靠性”被操作化为任务成功率 + 行为合规率（如背景语音应答率），但摘要未给出延迟、成本、打断恢复等系统指标的约束。
2. Voice Harness 用 Qwen-Audio-3.0-Realtime 作前台，说明持久任务能力被放在模型之外的 harness 层解决，3.1 本身是否内建该能力在摘要中无法判断（证据不足，需回原文确认）。
3. 半双工 vs 全双工两条评测线并行，暗示模型能力在不同交互模式下可能并不等价，摘要没有给出两者之间的性能关系。

Q2: 有哪些相关研究？

重要声明：本次提供的材料只有论文标题、作者、摘要与元数据，sections 中 introduction/method/results/discussion/conclusion 全为空，retrieved_evidence 与 field_evidence_map 也是空的。因此以下不是对该论文 Related Work 章节的复述，而是基于摘要关键词与领域常识画出的“相邻研究地图”，用于你回原文时逐条核对作者实际引用了哪些工作。凡涉及具体文献名的判断均为推测。

一、级联式语音助手 vs 端到端语音语言模型
传统方案是 ASR → LLM → TTS 的级联管线，优点是各模块可独立优化、文本侧推理能力强，缺点是延迟累积且丢失副语言信息（语气、情绪、重叠语音）。端到端 speech-to-speech 模型则以牺牲部分语言能力换取低延迟与原生音频技能。本文的 Think 模块（语言能力迁移 + 原生音频技能）正是对这一取舍的直接回应：用蒸馏把文本侧能力搬到音频侧，而不是在两者之间二选一。

二、语音-文本联合训练与多模态对齐
让语音与文本共享解码空间、或以文本为“语义锚点”训练音频编码器，是近年语音 LLM 的主流做法。论文将其称为 Core-Cocktail 监督微调（推测指混合多种数据/任务的“鸡尾酒式”SFT 配方），具体配方在摘要中未披露。

三、在线策略蒸馏与多教师蒸馏
On-Policy Distillation 的关键是让学生模型在自己产生的轨迹分布上接受教师信号，而非仅在教师数据上做离线模仿；多教师版本（M²-OPD）则暗示教师按模态/能力分工（多模态 + 多教师），用于把不同来源的能力合并进同一策略。这与“多能力集成”的问题设定高度契合，但教师数量、配比、损失形式摘要均未给出。

四、RLVR / GRPO 与可执行环境
GRPO（Group Relative Policy Optimization）是 DeepSeek 系工作中广泛使用的无 critic 组相对策略优化方法，在可验证奖励场景（数学、代码、agent 任务）中常与大规模 rollout 结合。本文的 Act 模块把这条路线搬到语音智能体：自演化可执行环境 + 多粒度 rollout。相关研究方向包括工具调用 RL、合成可执行环境、环境难度自适应（推测），以及与 τ-bench 系列对话式工具调用基准的互动。

五、对话式工具调用基准：τ-bench 与 τ-Voice
τ-Voice 属于 τ-bench 家族面向语音的延伸，评测的是在带规则约束的领域对话中（如客服）通过工具调用完成任务。论文明确说自己用的是 “our half-duplex speech-to-text adaptation”，即他们自己改造的半双工、语音输入-文本任务侧版本，因此与其他论文报告的 τ-Voice 数字不可直接横向比较。

六、全双工交互及其评测
Full-Duplex-Bench v1.5 关注语音到语音的低延迟全双工行为，典型考核点包括 turn-taking、barge-in、背景语音处理等。背景语音应答率正是这一类指标的典型代表。本文把它作为第二根主评测轴。

七、语音安全
摘要把 safety 列为六大评测维度之一，说明作者把安全视为“可靠”定义的组成部分；但具体的安全分类（有害内容、越权工具调用、隐私/录音等）摘要未展开。

八、本文的相对位置（推测）
作者试图主张的差异化是：多数语音工作只解决“听得懂/说得好”或只解决“会调工具”，而本文把 SFT + 蒸馏 + RL + 对话行为对齐串成一条流水线，并用覆盖六类能力的评测体系证明整体提升；Voice Harness 则进一步把重心从“单轮实时对话”推向“持久任务”。

Q3: 论文如何解决这个问题？

摘要给出的方案是三层结构加一个外围原型，本节按“每一层要解决什么、机制名称暗示了什么、哪些细节仍缺失”展开。凡摘要未明写的实现细节均标注为推断。

一、Think：把语言能力搬进音频，同时长出自己的音频技能
1. 组件一：Core-Cocktail 监督微调。名称暗示是混合多来源、多任务数据的 SFT 配方（推测包含文本指令、语音理解、语音生成、多模态数据等）。摘要只说它承担“监督微调”角色，未给出数据配比、训练阶段划分或是否分阶段解冻。
2. 组件二：M²-OPD（Multimodality and Multi-Teacher On-Policy Distillation）。要点有三：a) on-policy——学生在自己采样出的轨迹上接受蒸馏信号，缓解离线模仿的分布偏移；b) multi-teacher——多个教师分别贡献监督，暗示文本教师负责语言/推理、音频教师负责语音侧行为；c) multimodality——蒸馏信号跨模态。摘要明确其目标为“transfer language capabilities and develop native audio skills”，即把文本侧能力迁移过来并同时培育原生音频技能。
3. 该层的隐含设计取舍：若只做蒸馏，学生可能退化为文本教师的语音复读机；若只做音频 SFT，则语言能力不足。Core-Cocktail + M²-OPD 的组合是这两端的折中。

二、Act：用 RL 教会模型“用工具—读反馈—完成任务”
1. 自演化可执行环境（self-evolving executable environments）：环境本身会更新/加难，用于持续供给有区分度的任务，避免固定环境下的策略过拟合。具体演化机制（任务生成、难度爬升、奖励校验）摘要未披露。
2. 多粒度 rollout（multi-granularity rollouts）：暗示在多个层级上采样轨迹——可能是单步工具调用粒度、完整任务回合粒度，以及介于两者之间的中间粒度（推测，需回原文确认）。多粒度 rollout 的动机通常是解决长程 agent 任务中奖励稀疏、信用分配困难的问题。
3. GRPO：以组内相对优势替代 value critic，适合 rollout 成本高但可批量采样的 agent 训练。该层的学习目标是“使用工具、解读反馈、完成任务”三件事，注意“解读反馈”被单独点名，说明作者认为工具返回值的语义解析（而非仅格式正确）是难点。

三、Speak and Coordinate：把“要不要说话”变成可优化的决策
该层显式对齐三件事：how（怎么表达）、when（何时插话/何时停顿）、whether（是说话还是改为执行动作）。这实际上是把“对话行为”从被动的解码结果，升级为需要与任务目标共同优化的策略维度。它与 Act 存在耦合：一次工具调用可能比一句解释更合适，反之亦然。摘要未说明这一层是用 RL、偏好优化还是规则约束实现（证据不足）。

四、Voice Harness 原型（外围系统，非 3.1 模型本体）
1. 关键事实（摘要直接支持）：它使用 Qwen-Audio-3.0-Realtime 作为前台（foreground），而不是 3.1。
2. 功能：通过前景-背景协调（foreground-background coordination）与记忆（memory），把口语交互扩展到持久任务（persistent tasks）——即用户打断、暂停、切换话题后，任务状态仍被保留与续接。
3. 系统含义（合理推断）：持久任务需要与实时对话解耦，因此采用前台负责即时交互、后台负责长任务推进的双通道结构；记忆是连接二者的状态载体。3.1 是否把该能力内化到模型层，摘要未说明。

五、方案的整体逻辑
Think 负责“有东西可推理、有音频能力可用”，Act 负责“把推理落到动作”，Speak and Coordinate 负责“把动作与话语放在正确的时间点上”，Voice Harness 负责“把时间尺度从一轮对话拉长到跨会话任务”。评测的六个维度正好覆盖这条链路的不同环节。

Q4: 论文做了哪些实验？

声明：论文的 Experiments 章节正文未提供，以下仅基于摘要列出的评测范围与两处具体数字整理，不含表号、超参、数据规模等信息。

一、评测覆盖的六个维度（摘要直接列出）
1. 音频推理（audio reasoning）；2. 多语言理解（multilingual understanding）；3. 工具使用（tool use）；4. 对话行为（conversational behavior）；5. 全双工交互（full-duplex interaction）；6. 安全（safety）。
这六项说明作者把“可靠性”拆成能力侧（推理、多语言、工具）与行为侧（对话行为、全双工、安全）两块来度量。

二、可提取的具体实验对象与数字
1. τ-Voice 半双工语音到文本改造版：由论文自行适配（“our half-duplex speech-to-text adaptation of τ-Voice”），因此这是作者自建评测设置，非原版 τ-Voice 标准协议。指标为“overall task success”。结果：Qwen-Audio-3.0-Realtime 78.4% → Qwen-Audio-3.1-Realtime 82.0%，绝对提升 3.6 个百分点，相对提升约 4.6%。
2. Full-Duplex-Bench v1.5（语音到语音）：指标为“response rate to background speech”（对背景语音的应答率）。结果：73.0% → 13.0%，绝对下降 60 个百分点，相对下降约 82%。

三、对比基线与消融（缺失项，需回原文确认）
摘要未说明：1）除了 3.0 版本外是否对比了其他语音助手/端到端语音模型；2）Thinking / Act / Speak and Coordinate 三块各自的消融贡献；3）M²-OPD 的教师组合与有无蒸馏的对照；4）GRPO 是否有非 RL 基线；5）Voice Harness 是否做了单独评测（摘要只把它作为原型呈现，未给数字）。

四、Voice Harness 的设计性实验（属于系统原型，非受控实验）
以 Qwen-Audio-3.0-Realtime 为前台，通过前景-背景协调与记忆把交互扩展到持久任务。摘要未提供任务成功率、记忆命中率、长期一致性等量化指标。

Q5: 发现了什么实验现象？

一、两个数字讲出的核心现象
1. 能力向上、打扰向下。3.1 同时在两个方向上都优于 3.0：任务成功率上升（78.4% → 82.0%），对背景语音的应答率大幅下降（73.0% → 13.0%）。这两个指标方向相反却同为“改善”，说明它们度量的是两种不同的失败模式——前者是“做不成事”，后者是“不合时宜地说话”。论文主张的“可靠”实际上是同时压低这两类失败（作者用 “reliable agentic voice interaction” 表述，合理推断）。
2. 背景语音应答率 73.0% → 13.0% 是一个量级性的行为翻转，而不是渐进优化。73% 意味着在 3.0 上模型对背景人声几乎“照单全收”，这在真实环境（电视声、旁人对话、办公室噪音）中会导致会话被频繁劫持。降到 13% 说明 3.1 学到了“背景语音不是对我说的”这一判断，而这类判断恰恰属于 Speak and Coordinate 的 “whether” 维度，与摘要中“aligns how, when, and whether the assistant speaks or acts”的定位一致。这是本摘要中最有信息量的因果暗示（合理推断，原文是否有归因消融需核对）。
3. 代价未披露：13% 不是 0%。残留的 13% 应答可能来自真实插话与背景语音的边界模糊（推测）。同时摘要未给出“该响应时是否也变迟钝”的对称指标（如 barge-in 响应延迟、漏响应率）。任何“更少应答”的改进都必须检查是否伴随漏响应上升——这是该结果最需要回原文核对的张力点。

二、任务成功率侧的观察
1. 3.6 个百分点的提升放在半双工语音改造的 τ-Voice 上，属于中等幅度改进。考虑到这是“语音到文本”的适配（语音输入、文本侧任务评测），它主要检验的是语音理解 + 工具调用链路，而非 TTS 质量或全双工行为。
2. 由于这是作者自建适配版本，78.4%/82.0% 不可与其他论文报告的 τ-Voice 数字横向比较（方法论提示，非论文结论）。

三、评估结构本身的观察
1. 六个评测维度并存，说明作者承认单指标提升可能掩盖行为侧退化——这正是 3.0 的教训（能力可用但打扰严重）。
2. 安全被列入指标清单但摘要未给数字，属于证据缺口。
3. Voice Harness 以 3.0 为前台而非 3.1，是一个明显的实验设计留白：持久任务能力与 3.1 的新能力没有在摘要层面被联合评估。这既可能是工程进度原因，也可能是有意让两个贡献相互独立（推测）。

Q6: 有什么可以进一步探索的点？

以下方向中，凡摘要未提及的均为基于本文结构的延伸建议，不是作者已声明的 future work（作者声明部分在摘要中不存在）。

一、能力集成层的进一步问题
1. M²-OPD 的教师组合、数量、权重与模态分工未公开；可探索的题目包括：教师能力重叠时如何防止冲突信号、多教师蒸馏在音频生成侧是否同样有效、蒸馏与 RL 阶段能否合并为单阶段训练。
2. Core-Cocktail 的数据配方（文本/语音/多模态比例）与“原生音频技能”的度量方式，是复现与改进的关键缺口。
3. 语言能力迁移是否带来“文本强、语音弱”或反向的不对称退化，需要分模态的能力对照实验。

二、Agent 训练层
1. 自演化可执行环境的演化策略（任务难度爬升、任务多样性、奖励可验证性）若失控可能导致环境崩塌（任务同质化）或奖励黑客；可探索环境演化的稳定性判据与防刷机制。
2. 多粒度 rollout 的具体粒度划分、信用分配方式与算力开销的权衡值得进一步系统研究。
3. 面向语音的 agent 任务奖励如何设计：当前多半来自文本侧任务完成度，语音特有失败（听错、被打断、时序错位）是否被纳入奖励，是明显空白。

三、对话行为与全双工
1. 背景语音应答率降到 13% 后，需要对称地考察“必要的响应是否被漏掉”“插话延迟是否增加”。理想目标是同时报告误触发率与漏触发率的联合曲线（DET 式分析）。
2. 说话/行动的联合决策（whether to speak or act）目前只是摘要中的一句定位，缺少可解释的决策边界刻画；可探索显式的“沉默策略”与任务收益之间的权衡。
3. 全双工 + 多语言 + 安全的交叉场景（例如嘈杂的非母语环境下）是六维度评测尚未展示的组合。

四、Voice Harness 与持久任务
1. 论文原型的前台是 3.0 而非 3.1，最直接的后续是做 3.1 × Harness 的联合版本，并检验 3.1 的行为对齐是否让前台-后台协调更稳。
2. 记忆机制的具体形态（上下文压缩、外部存储、检索策略）、记忆冲突与遗忘策略、隐私边界，都是可持续研究方向。
3. 持久任务的评测基准缺失：如何定义“跨天/跨会话的任务续接成功率”，目前没有公开的稳定协议（推测）。

五、评测与部署
1. τ-Voice 的半双工语音改造版若能公开，将有助于横向比较；否则各家的语音 agent 数字仍不可比。
2. 端到端延迟、打断恢复时间、单位交互成本等系统指标应与成功率并列报告。
3. 安全侧：工具调用越权、录音/隐私、语音伪造辨识在语音 agent 场景中有独特风险面，摘要未给数字，值得优先补足。
4. 级联式（ASR→LLM→TTS）与端到端语音 agent 在同一声学条件下的受控对比仍稀缺（推测为领域空白）。

Q7: 总结一下论文的主要内容

这篇论文要解决的问题可以概括为一句话：实时语音助手必须同时满足三项能力要求——在持续演变的请求上做推理、执行动作、并遵守对话规则——而现有系统通常只能覆盖其中一到两项。Qwen-Audio-3.1-Realtime 的主张就是把三者统一进同一模型，并用一套覆盖六类能力的评测来证明“可靠（reliable）”是可度量、可改进的（问题陈述部分由摘要直接支持；作者对现有系统缺陷的具体归因需回原文确认）。

技术路线由三个命名模块构成，命名本身透露了作者的能力分解方式。
第一是 Think，负责“想”。它由 Core-Cocktail 监督微调与 Multimodality and Multi-Teacher On-Policy Distillation（M²-OPD）组成，目标有两个且被并列陈述：transfer language capabilities（把语言能力迁移进来）与 develop native audio skills（发展原生音频技能）。这一并列本身就说明作者拒绝在“文本侧推理强”和“音频侧原生能力好”之间二选一，而是用蒸馏把两端拉到一起。M²-OPD 的三个关键词分别指向：on-policy（学生在自己采样出的轨迹上接受教师信号，缓解分布偏移）、multi-teacher（多个教师分工提供监督，推测为文本教师与音频教师）、multimodality（蒸馏跨模态进行）。摘要未披露教师数量、权重、损失形式与训练阶段编排。
第二是 Act，负责“做”。论文使用自演化可执行环境（self-evolving executable environments）与多粒度 rollout（multi-granularity rollouts）来运行 Group Relative Policy Optimization（GRPO）。学习目标被写成三件事：使用工具、解读反馈、完成任务。把“解读反馈”单列出来值得注意——它暗示作者的难点认知不止在“会不会调用工具”，而在于工具返回值往往是非结构化、部分失败或语义含糊的，模型必须据此修正后续动作。自演化环境解决的是任务供给与难度爬升，多粒度 rollout 解决的是长程任务的采样与信用分配（后一句为合理推断，具体粒度定义摘要未给）。
第三是 Speak and Coordinate，负责“说与不说的调度”。它的对齐目标是 how（以什么方式说）、when（何时说）、whether（是说，还是改为执行动作）。这一层实际上把对话行为从“解码时的自然产物”提升为“需要与任务目标联合优化的显式策略维度”，也是全文与纯语音识别/合成工作最本质的分野。

在主模型之外，论文还给出一个独立的 Voice Harness 原型。它的关键设计是双通道：Qwen-Audio-3.0-Realtime 充当前台（foreground），负责即时口语交互；harness 层通过前景-背景协调与记忆把交互扩展到持久任务（persistent tasks）。需要特别注意的是，该原型使用的是 3.0 而不是 3.1（摘要明确），这意味着持久任务能力在当前版本中被放在模型之外的系统层解决，两个贡献彼此独立呈现。

评测设计覆盖六个维度：音频推理、多语言理解、工具使用、对话行为、全双工交互、安全。这种划分等于承认“可靠”不能由单一指标刻画，能力侧与行为侧必须并测。摘要给出的两处具体结果分别落在能力侧与行为侧：在论文自建的半双工、语音转文本版 τ-Voice 上，整体任务成功率从 Qwen-Audio-3.0-Realtime 的 78.4% 提升到 3.1 的 82.0%；在语音到语音的 Full-Duplex-Bench v1.5 上，对背景语音的应答率从 73.0% 降到 13.0%。

这两组数字应当被一起读。前者是“把事做成”的能力，提升幅度中等（绝对 3.6 个百分点），且由于 τ-Voice 版本是作者自行改造的，不能与其他论文报告的 τ-Voice 数字直接比较。后者是“不该说时别说”的行为，变化是量级性的：从 73% 降到 13% 意味着 3.0 在背景语音存在时几乎会持续误响应，而 3.1 基本学会了区分“这段语音不是对我说的”。这一翻转恰好落在 Speak and Coordinate 的 whether 维度上，构成摘要中最强的一条因果暗示（归因是否经消融验证，需回原文核对）。同时要注意其代价面未被披露：13% 不是 0%，而“更少应答”是否伴随“该应答时漏应答”或“插话延迟增加”，摘要完全没有对称指标。安全维度被列入评测清单却没有给出任何数字，Voice Harness 也没有量化结果。这些都是阅读原文时需要优先补齐的缺口。

总体而言，这是一篇系统集成型技术报告：它的贡献不在于提出某个全新算法，而在于把监督微调、多教师在线蒸馏、可验证奖励的 RL、对话行为对齐四条既有技术线组织成一条流水线，并把“语音智能体的可靠性”拆成可测的六个维度，用两组方向相反的数字来同时支撑能力提升与行为收敛的双重主张。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你画像中的 agent 方向直接重合（权重 0.10）：本文的 Act 模块（自演化可执行环境 + 多粒度 rollout + GRPO）是 agentic RL 在语音模态上的一次系统化落地，可与你关注的文本 agent 训练范式做对照。

## 基本信息

- 作者：Lujia Bao, Qian Chen, Luyao Cheng, Chong Deng, Yuxiang Kong, Xiangang Li, Xu Li, Jiaqing Liu, Chao-Hong Tan, Haoyu Wang, Wen Wang, Xilou Wang, Junhao Xu, Liang Yi, Binbin Zhang, Qinglin Zhang, Qiquan Zhang
- 机构：推测为阿里巴巴通义千问（Qwen）团队——依据是标题中的 Qwen-Audio 命名与作者署名中出现的 Qwen 系列语音团队成员；但提供的元数据中 institution 字段为 null，本次也未获得 PDF 正文（sections 全空），因此该推断未经原文机构信息确认，需回原文首页核对。
- 来源：arxiv
- 主题/分类：eess.AS, cs.AI, cs.CL, cs.SD
- 日期：2026-09-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.25176v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未获得任何 PDF 检索证据（retrieved_evidence 与 field_evidence_map 均为空，sections 各章正文为空），全部内容基于论文摘要与元数据推演：模块名称与两处数值来自摘要原文，方法与实验细节凡超出摘要范围者均已标注为推断或证据缺口，并与 heuristic_draft 的粗略线索做了合并补全。
