---
user_id: "cheng tan"
paper_id: 13504
arxiv_id: "2609.29892v1"
title: "Qwen-Planner-Agent: A Closed-Loop AI-for-AI Framework for Real-World Mobile Planner Agents"
publish_date: "2026-09-24"
pdf_url: "https://arxiv.org/pdf/2609.29892v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-26T01:50:04"
---
# Qwen-Planner-Agent: A Closed-Loop AI-for-AI Framework for Real-World Mobile Planner Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：mobile agent · agentic reinforcement learning · ai-for-ai · data flywheel

## 一句话总结

该论文提出 Qwen-Planner-Agent：一个以「行动—反馈—验证」共享契约为接口的闭环 AI-for-AI 框架，把 agentic 数据飞轮、监督冷启动加混合环境在线智能体强化学习（含 CARE 奖励/优势工程）、以及模型与 harness 的协同演化三条链路打通，在 MobilePA-Bench 上取得所有被评估模型与系统中的最佳综合表现，并在非移动 agentic 基准上出现正向迁移且基本保持通用能力。

## 摘要

> The rapid progression of large language models is extending AI from passive content generation into the active workflows of engineering and scientific discovery. This shift raises a compelling question: can AI be both the object of development and an active participant in building next-generation AI systems? We explore this question by building Qwen-Planner-Agent within a closed-loop AI-for-AI framework for scalable development and iterative improvement. Mobile planning offers a demanding test of this approach: complex, long-horizon tasks challenge agent reliability, while costly real-device interaction limits development scalability. The framework connects data production, model training, and deployment through a shared action-feedback-verification contract. (i) AI for Data builds a human-gated agentic data flywheel in which specialized agents construct tasks, collect interaction trajectories, curate and balance training data, and use training feedback to guide subsequent data generation. (ii) AI for Training combines a supervised planning cold start with hybrid-environment online agentic reinforcement learning, where we introduce Competence-Aware Reward-and-Advantage Engineering (CARE) to reduce reasoning and tool-use costs while preserving task performance. (iii) AI drives model--harness co-evolution through an execution-evidence-driven loop that orchestrates memory, skills, and tools at runtime and feeds structured action feedback and preserved failure traces back into coordinated model and harness adaptation. Qwen-Planner-Agent achieves the best overall performance among all evaluated models and systems on MobilePA-Bench, improving over its base model across tool use, memory, skills, and sub-agent coordination. Further evaluations of our model show improvements across non-mobile agentic benchmarks while largely preserving general capabilities.

Q1: 这篇论文试图解决什么问题？

1）元层面的问题：AI 能否既是「被开发对象」又是「开发参与者」。论文把 AI-for-AI 从概念讨论落到一个可端到端验证的具体领域：LLM 已从被动内容生成转向工程与科学发现的主动工作流，那么是否可以让 AI 参与到构建下一代 AI 系统本身？这实际上是一个 agent 自我改进（self-improvement）的工程化命题，作者选择用闭环框架来回答，而不是提出单点模型技巧。合理推断：作者真正关心的是「闭环能否真正闭合」——即部署端产生的证据能否稳定、低成本地回流到数据与训练环节。

2）领域选择造成的双重约束。移动端 planner agent 被选为试验台，原因写得很明确：一方面任务复杂、长程，直接挑战 agent 的可靠性；另一方面真机交互代价高，使得「多试、快试」的常规研究迭代方式不可扩展。这两条约束并非移动域独有：任何依赖真实设备、真实 GUI 或真实环境的 embodied / GUI agent 研究都面临同一矛盾——环境成本拖慢数据与策略的迭代速度。因此论文的问题设定天然指向「基础设施 + 方法论」而非「模型结构」层面的解法。需要指出：摘要并未论证「移动规划足以代表 AI-for-AI 的一般闭环」，这是一个隐含假设，属于可质问点（推测其合理性来自移动任务的可验证性与规模）。

3）三段式割裂的具体问题（由摘要的三条支柱反推）。
（a）数据侧：任务构造、轨迹采集、数据筛选与配比通常由人工或固定脚本的静态流水线完成，训练反馈无法回流到数据生成过程，导致数据分布与模型「当前能力」错配——太简单无法提供梯度信号，太难则近似无信号；同时缺乏人工闸门时存在质量与安全风险。这正是 AI for Data 要解的：用专门化 agent 承担构造、采集、筛选、配平，并把训练反馈作为下一轮生成的指导信号；human gate 则作为质量/风险控制点被显式保留。
（b）训练侧：监督式规划冷启动能提供格式与行为先验，但很难优化长程决策；在线智能体强化学习更贴近目标，但需要环境交互。混合环境（hybrid-environment）的提法暗示作者在真机之外引入了替代性交互环境以摊薄成本（合理推断：模拟器/回放/离线轨迹的混合，具体构成需回原文核对）。同时存在一个常在 agentic RL 中被忽略的张力：推理 token 与工具调用本身都有成本，若只优化任务成功率，策略会倾向「过度思考」和「滥用工具」；CARE（Competence-Aware Reward-and-Advantage Engineering）正是为压制这种倾向同时保住性能而设计的，其「competence-aware」的命名暗示奖励/优势估计会依据模型当前能力水平做自适应调节（合理推断）。
（c）部署/推理侧：agent 的最终行为不只由权重决定，还由 harness（运行时对记忆、技能、工具的编排）决定。模型更新后 harness 往往需要手工重写，二者演化不同步，导致模型侧收益被 harness 侧瓶颈吞掉。论文要求 harness 也是闭环中的可学习/可适配对象，并把结构化行动反馈与失败轨迹作为两类适配信号。

4）评测问题：需要一个能同时覆盖工具使用、记忆、技能、子代理协调四类能力的移动 planner 基准（MobilePA-Bench），并且还要检验收益是否只局限在移动域——因此论文额外在非移动 agentic 基准上评估，并检查通用能力是否被破坏（灾难性遗忘风险）。

5）隐含假设与证据缺口：摘要未说明成本指标的具体度量方式（推理 token 数、工具调用次数、墙钟延迟还是真机交互步数），也未给出 human gate 的介入比例、人工瓶颈如何随规模变化、失败轨迹回流的隐私与安全边界。这些都是阅读原文时需要重点确认的边界条件。

Q2: 有哪些相关研究？

说明：本次未获取到 PDF 正文与参考文献列表，以下按领域谱系给出「论文大概率需要对话/对标的邻近工作类型」，具体被引文献、baseline 名称与对比数字必须回原文核对；凡属于推断的部分均已标注。

1）GUI / 移动端 agent 与基准。移动 UI 自动化与手机操作 agent 的代表性路线包括基于截图—动作循环的 GUI agent、以及围绕 Android 环境构建的可执行评测平台（如 AndroidWorld、Mobile-Agent 系列、AppAgent 一类工作，属领域常识性举例，非论文实际引用确认）。这些工作的共性是：把手机操作形式化为可执行动作序列，并以任务完成率为主指标。Qwen-Planner-Agent 与之的差异点（合理推断）在于：它不把模型当作唯一变量，而是把数据飞轮、训练目标与 runtime harness 一起纳入被优化对象，且引入 MobilePA-Bench 作为覆盖工具/记忆/技能/子代理的评测面。

2）Agentic RL 与 LLM 强化学习。从 RLHF/PPO 到近期的可验证奖励 RL（GRPO 一类）构成了 Qwen-Planner-Agent 训练支柱的直接背景；差异在于本工作面向多轮、带工具调用、长程的交互式任务，而非单轮可验证问答，因此需要处理稀疏且延迟的奖励、以及环境成本问题。CARE 落在「奖励塑形 + 优势估计工程」这一支线上：与长度惩罚、工具调用惩罚、预算感知奖励、过程奖励模型等方法同属一类思路，但摘要强调它是「competence-aware」的——即随模型当前能力水平自适应，而不是固定系数的惩罚项（合理推断，需核对公式）。

3）数据飞轮与合成/自举数据。self-instruct 式自举、agent 轨迹蒸馏、课程学习与数据配比研究是 AI for Data 的邻域。本工作的主张是：数据生成环节本身也由 agent 承担，并以训练反馈作为下一轮数据生成的控制器；同时保留 human gate 作为质量控制，这在「全自动合成」与「纯人工标注」之间构成一个中间设计点，与纯合成数据路线的核心区别在于反馈闭环的显式接入。

4）Harness / scaffolding 与 agent 记忆、技能、工具。ReAct 式推理—行动交错、反思类方法、技能库（如以可复用技能累积为代表的工作）、长期记忆系统、以及多子代理编排框架，构成了「harness」这一概念的技术来源。本工作的差异主张是把 harness 从手工工程对象变成闭环中的演化对象，并由「执行证据」驱动模型与 harness 的协同适配。

5）AI-for-AI / 自我改进。用 AI 加速 AI 研发（自动搜索算法、自动做实验、自我奖励等）是论文自我定位的元问题来源。Qwen-Planner-Agent 与之的关系（推测）是把自我改进限定在「数据生产—训练目标—运行时编排」这条工程链上，而不主张开放式自我改写。

6）通用能力回归评测。论文声称在非移动 agentic 基准上提升、并大体保持通用能力，这属于能力迁移与灾难性遗忘的经典议题，对应评估通常包括通用知识与推理基准。摘要未列出具体基准名，这是需要原文确认的缺口。

Q3: 论文如何解决这个问题？

论文的解法是一个三段闭环框架，三段共享同一份「行动—反馈—验证」契约（action-feedback-verification contract）。该契约是理解全文的关键：它意味着每一条数据、每一次训练样本、每一次运行时执行都携带同一套结构化字段——执行了什么动作、得到什么反馈、如何判定是否成功。因为契约统一，下游训练与运行时编排都能消费上游数据（合理推断）。

支柱一：AI for Data —— 带人工闸门的智能体化数据飞轮。
流程包含四个角色化步骤：
1. 任务构造（task construction）：由专门化 agent 生成任务，而非人工枚举；
2. 交互轨迹采集（trajectory collection）：在环境中执行并记录交互；
3. 数据筛选与配平（curate and balance）：对采集数据做质量筛选与难度/类型配平；
4. 反馈驱动再生成：用训练反馈（例如哪些数据带来的收益低）反向指导下一轮数据生成。
关键设计点是 human-gated：人工不是全流程标注者，而是守门人。合理推断，闸门用于拦截低质量或高风险任务/轨迹，同时把人工成本限制在可扩展的规模上。这套飞轮的直接动机是：数据分布需要跟随模型当前能力动态移动，否则会退化成「过易无信号、过难无梯度」。

支柱二：AI for Training —— 冷启动 + 混合环境在线智能体 RL + CARE。
1. 监督式规划冷启动（supervised planning cold start）：用高质量的规划轨迹先建立基本的行为先验与输出格式，为后续 RL 提供稳定起点。
2. 混合环境在线 agentic RL（hybrid-environment online RL）：在多种环境混合的条件下做在线强化学习；混合环境的作用是摊薄真机交互成本，使迭代在经济学上可行（合理推断：真机 + 轻量模拟/回放环境的组合，具体构成未在摘要中说明）。
3. CARE（Competence-Aware Reward-and-Advantage Engineering）：论文明确给出的目标函数权衡是「在保住任务表现的前提下降低推理与工具使用成本」。这意味着奖励或优势估计不仅要编码任务成功，还要编码过程代价，并且以某种「能力感知」的方式调节——推测其含义是：当模型在该类任务上尚不熟练时，应避免过早施加强成本惩罚（否则会诱发保守、少探索的退化解），而当模型已具备能力时，再收紧成本约束。这是对常见的固定系数长度/工具惩罚的一个改进思路，属于全文最值得细读公式与消融的部分。

支柱三：AI 驱动的 model–harness 协同演化。
1. 运行时编排：harness 在推理时编排记忆（memory）、技能（skills）与工具（tools），并可协调子代理（sub-agent coordination）。
2. 执行证据驱动：把结构化的行动反馈与「被保留下来的失败轨迹」作为演化信号。把失败轨迹显式保留并回流，是这一支柱的一个特征性设计——失败样本不被丢弃，而是作为训练与 harness 适配的素材。
3. 协同适配：模型侧与 harness 侧的更新被放在同一个循环里协调进行，而不是模型升级后人工重写 prompt/工具/记忆策略。

整体上，该框架的研究承诺是「闭环性」：数据生产、训练目标、运行时编排三者以同一契约耦合，使任一环节产生的信号都能被其他环节消费。评价这条承诺是否兑现，需在原文中核对闭环的延迟（一轮迭代需要多久）、人类介入比例，以及 harness 演化是否真的由自动化证据驱动还是依赖人工规则。

Q4: 论文做了哪些实验？

受限于本次仅有摘要与元数据（未获取 PDF 正文、无检索证据命中），以下区分「摘要明确陈述的实验」与「按框架逻辑合理推断应存在、但需回原文确认的实验」。

A. 摘要明确陈述的评估：
1. 主评测：MobilePA-Bench。论文声称 Qwen-Planner-Agent 在所有被评估的模型与系统中取得最佳综合表现（best overall performance）。摘要未给出任务数、任务类别划分、成功判定方式、对比系统清单与具体分数，均为需原文补全的关键信息。
2. 相对基座模型的四维提升：工具使用（tool use）、记忆（memory）、技能（skills）、子代理协调（sub-agent coordination）。这四项恰好对应 harness 的四个运行时组件，说明评测设计刻意与框架支柱三对齐——但这同时意味着主评测可能偏向框架自身的能力切分，是否公平对比较难从摘要判断。
3. 跨域迁移评估：在非移动类 agentic 基准上显示提升（结论为「improvements across non-mobile agentic benchmarks」，未给具体基准名与幅度）。
4. 通用能力保持：明确表述为 largely preserving general capabilities。「largely」这一限定词暗示可能存在小幅回退，需核对具体基准与差值。

B. 合理推断应存在、但摘要未证实的实验（阅读时必须核对）：
1. CARE 的消融：固定系数成本惩罚 vs competence-aware 调节、不同的成本权重、只惩罚推理长度 / 只惩罚工具调用 / 两者都罚，以及「性能—成本」帕累托曲线的呈现方式。这是验证 CARE 主张的核心实验。
2. 冷启动 vs 冷启动+RL 的对照，以及混合环境中各环境类型的贡献拆分。
3. 数据飞轮的增益验证：单轮数据 vs 多轮飞轮迭代、有无 human gate、有无训练反馈驱动的再生成、数据配平策略的影响。
4. harness 组件消融：记忆、技能、工具、子代理逐个关闭的表现变化。
5. 失败轨迹回流的消融：保留 vs 丢弃失败轨迹对最终性能的影响。
6. 成本度量与效率实验：token 消耗、工具调用次数、交互步数、延迟或真机交互成本的量化对比。
7. 与外部强 baseline（商业 agent 系统或其他移动 agent）的对比细节与统计显著性。

Q5: 发现了什么实验现象？

同样基于摘要与元数据，无法取得数值级现象。以下区分「摘要可直接读出的事实性观察」与「由措辞推断出的趋势/张力」，后者均标注为推断。

1）事实性观察：在 MobilePA-Bench 上，Qwen-Planner-Agent 的综合表现优于所有被评估的模型与系统；相对自身基座模型，在工具使用、记忆、技能、子代理协调四个维度上均有提升。这说明从基座到最终系统的增益不是单点收益，而是横跨 harness 多个组件——合理推断：训练侧的改进与运行时编排的改进之间存在互补，单靠模型权重或单靠 harness 工程都难以复现同等提升（后者为推断，需消融验证）。

2）性能—成本的权衡现象（全文最值得关注的一条）。CARE 的设计目标被表述为「降低推理与工具使用成本的同时保持任务表现」。这类表述在文献中通常对应一个帕累托改进的观察，但摘要用词是「while preserving task performance」而非「improving」，提示成本下降可能是主要收益，性能是「不掉」而不是「提升」（合理推断，需核对消融表）。若原文确实呈现为成本下降、性能持平，那么这里的科学问题是：成本下降来自行为层面的改变（更少冗余推理、更少无效工具调用）还是来自奖励塑形导致的策略保守化，二者在长期任务上的后果不同，前者是纯收益，后者可能在更复杂任务上转化为成功率下降。这一点在摘要层面无法判定。

3）泛化与遗忘的张力。论文同时声称「非移动 agentic 基准上有提升」与「大体保持通用能力」。前者是正向迁移，后者用的是保留性措辞。合理推断：存在一种可能——agentic 训练带来的收益主要集中在工具调用与多轮交互这类可迁移能力上，而对通用知识与推理的影响是中性的甚至轻微负向；「largely preserving」是作者对轻微回退的诚实限定。这是阅读时需要对着具体数字看的地方。

4）失败轨迹的价值假设。框架明确保留失败轨迹并回流。这隐含一个经验性主张：失败样本比成功样本包含更多可学习的边界信息（推测其依据来自 agent 训练中稀疏成功率的现实——长程任务中成功轨迹稀少，若只学成功轨迹则数据量不足）。若原文展示了失败轨迹回流的增益消融，则这条假设被直接支持；若没有，则属未验证的设计选择。

5）数据飞轮的自洽性现象。摘要提到「用训练反馈指导后续数据生成」，这暗示作者观察到数据分布会随模型能力漂移而失效（否则不需要反馈控制）。这是一个值得关注的负反馈机制设计，但摘要未给出漂移的定量证据。

6）证据缺口清单：无任何具体数值、无 baseline 名称、无成本度量定义、无显著性检验信息、无失败案例分析、无 human gate 介入比例、无迭代轮次与单轮成本。这些缺口使「最佳综合表现」这一结论目前无法独立复核。

Q6: 有什么可以进一步探索的点？

1）闭环的真实闭合度与迭代经济学。论文主张闭环，但未说明一轮「数据生成—训练—部署—反馈」的周期成本与延迟。可进一步研究：闭环中哪些环节是真正的自动反馈（而非人工规则），人类闸门在规模扩大后是否成为主导瓶颈，以及是否存在可以取消或分层的中间环节。

2）CARE 的机制拆解与理论化。「competence-aware」的判定依据是什么（在线成功率、熵、优势方差、还是显式的难度估计）？能力估计错误时会发生什么（例如模型被低估导致成本惩罚过晚生效，或在困难任务上过早施加惩罚引发探索塌缩）？可进一步做：把能力感知换成固定调度、按任务难度分层的固定权重，比较二者的成本—性能前沿；研究成本惩罚在长程任务上的时间尺度敏感性（早惩罚 vs 晚惩罚）。

3）成本指标的定义与多目标优化。推理 token、工具调用、真机步数、延迟、货币成本之间可能不可互换。可进一步研究多目标帕累托前沿的显式刻画，以及是否应把成本作为约束（constrained RL）而非奖励项。

4）model–harness 协同演化的稳定性。模型与 harness 同时变化会引入非平稳性：harness 变更可能让此前训练的模型行为失配。可探索交替更新 vs 联合更新、harness 变更的版本兼容性、以及是否会出现「harness 过拟合当前模型」的退化。这部分在摘要中完全未涉及，是明显的开放问题。

5）失败轨迹的利用方式。保留失败轨迹之后如何用（SFT 负例、偏好对、过程奖励建模、还是仅作为 harness 规则修正证据）会显著影响效果。可进一步研究失败类型分类学、失败到 harness 修改的自动映射，以及失败轨迹回流带来的分布偏移风险。

6）human gate 的设计空间。把关强度、把关者专业度、闸门放在飞轮的哪一环节（任务构造 vs 轨迹筛选 vs 发布前验证）对最终质量与成本的影响，是一个偏 HCI/流程设计的可研究方向。

7）跨域泛化的边界。论文声称非移动 agentic 基准有提升。可进一步研究：迁移的载体是工具调用格式、记忆策略还是子代理编排协议；在需要专业领域知识（例如生物/科学实验流程）的任务上，这种迁移是否仍然成立。

8）与 AI-for-science 的连接。若把「移动规划」替换为科研工作流（实验设计、仪器操作、数据整理），该框架的哪些组件可以直接复用、哪些依赖手机 UI 的特定性，是一个自然的后续方向；生物/科学应用落地的关键可能在于验证契约如何定义（真值来源、可重复性）。

9）安全性、隐私与审计。真机交互意味着真实账号与真实数据；失败轨迹保留与回流涉及隐私与合规。可探索脱敏后的证据契约、可审计的决策链，以及自动化 harness 修改的权限边界。

Q7: 总结一下论文的主要内容

论文的核心命题是 AI-for-AI：随着大语言模型从被动内容生成走向工程与科学发现的主动工作流，一个自然的问题是——AI 能否既作为被开发的对象，又作为构建下一代 AI 系统的主动参与者。作者没有停留在概念层面，而是选定「真实世界移动端 planner agent」作为试验台，用端到端闭环系统来回答这个问题。选择移动规划的理由是它同时具备两个难点：任务复杂且长程，直接考验 agent 的可靠性；真机交互代价高昂，使得开发迭代无法像纯软件任务那样快速扩展。这两个约束决定了论文的解法必须是「基础设施 + 方法论」层面的。

据此，作者构建了 Qwen-Planner-Agent，并把整个开发过程组织为一个闭环 AI-for-AI 框架。框架的耦合机制是一份共享的「行动—反馈—验证」契约（action-feedback-verification contract），它把数据生产、模型训练与部署三个环节串成一条链：因为三个环节使用同一套结构化字段描述动作、反馈与验证结果，任一环节产生的信号都能被其他环节消费。这一契约是理解全文结构的关键，也是作者主张「闭环」而非「流水线」的技术依据。

框架的第一条支柱是 AI for Data：一个带人工闸门的智能体化数据飞轮。由专门化 agent 承担任务构造、交互轨迹采集、训练数据的筛选与配平，并把训练反馈用于指导下一轮的数据生成。其设计动机是让数据分布随模型当前能力动态漂移——避免出现「太简单没有梯度信号、太难近似无信号」的错配；human gate 则把人工成本压缩到守门而非全量标注的规模，同时保留质量与风险控制点。

第二条支柱是 AI for Training：先做监督式规划冷启动，建立基本行为先验与输出格式；再进行混合环境下的在线智能体强化学习，以摊薄真机交互成本（混合环境的具体构成在摘要中未展开，需回原文核对）。训练侧的关键创新是 Competence-Aware Reward-and-Advantage Engineering（CARE），其明确目标是在保住任务表现的前提下降低推理与工具使用成本。这直接针对 agentic RL 中一个常见失衡：若只优化任务成功率，策略会倾向于过度推理和滥用工具；而固定系数的成本惩罚又可能在模型尚不熟练时抑制必要的探索。CARE 用「能力感知」的方式调节奖励与优势估计，试图绕开这一两难（具体公式与消融需核对原文）。

第三条支柱是 AI 驱动 model–harness 协同演化：通过执行证据驱动的循环，在运行时编排记忆、技能与工具，并把结构化的行动反馈与被显式保留的失败轨迹回灌到模型与 harness 的协调适配中。这条支柱的设计立场是：agent 的最终表现不单由权重决定，runtime harness（记忆、技能、工具、子代理编排）同样是决定性变量；若模型更新后 harness 仍靠人工重写，模型侧收益会被 harness 侧瓶颈吞掉。把失败轨迹保留下来并作为演化信号，也隐含了一个经验性主张——在长程任务中成功轨迹稀少，失败样本承载更多边界信息。

实验层面，论文的主评测是 MobilePA-Bench，结论是 Qwen-Planner-Agent 在所有被评估的模型与系统中取得最佳综合表现，并在工具使用、记忆、技能、子代理协调四个维度上均优于其基座模型。这四个维度与第三条支柱的 harness 组件一一对应，说明评测设计刻意与框架结构对齐。此外，论文报告了跨域证据：在非移动类 agentic 基准上也观察到提升，同时「大体保持」通用能力。这一「largely」的限定词提示可能存在轻微回退，是需要对着具体数字确认的地方；而正向迁移则支持「工具调用与多轮交互能力具备跨域可迁移性」这一解读（合理推断）。

总体而言，这篇论文的贡献结构是系统性的而非单点的：把数据飞轮、成本感知的 agentic RL、以及模型—harness 协同演化组织在同一个契约下，并以移动规划作为压力测试。其最值得细读的部分有三处：CARE 的能力感知机制与消融（性能—成本帕累托到底如何移动）、数据飞轮中训练反馈闭环的实际作用与 human gate 的成本占比、以及 harness 演化是否真由执行证据自动驱动。需要提醒的是，本次生成仅基于摘要与元数据，未获取 PDF 全文与检索证据，因此所有具体数值、baseline 名称、成本指标定义、统计显著性与失败案例分析均无法核实；「最佳综合表现」「大幅/轻微提升」这类结论性表述目前只能作为作者主张记录，不能作为已复核事实。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与当前画像中的 agent 方向（权重 0.10）直接重合：论文主体是移动端 planner agent 的完整训练、数据与运行时编排体系。

## 基本信息

- 作者：Tingyu Qu, Weigao Sun, Yuecheng Liu, Yucheng Zhao, Yi Zhu, Yifeng Ding, Qiyi Wang, Sihan Cao, Pengkun Jiao, Hanlei Xie, Xiongwei Wu, Qichao Wang, Haodong Zhang, Jiajun Liu, Yuhao Wang, Yuqing Xie, Junpeng Zhao, Long Chen, Ming Ma, Sihan Yang, Ziwang Zhao, Yanhao Jia, Liangquan Gong, Feida Zhu, Yiran Zhong, Steven Hoi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-09-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.29892v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未获取 PDF 全文，retrieved_evidence 与 field_evidence_map 均为空，所有内容仅基于标题、作者列表、摘要与元数据，并以「合理推断/推测」显式标注了推断部分；各字段的证据缺口已在对应位置逐条列出，机构字段因元数据缺失且无正文可查而返回空字符串。
