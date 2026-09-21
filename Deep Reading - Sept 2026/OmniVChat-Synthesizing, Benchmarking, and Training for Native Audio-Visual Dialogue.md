---
user_id: "cheng tan"
paper_id: 12298
arxiv_id: "2609.21465"
title: "OmniVChat: Synthesizing, Benchmarking, and Training for Native Audio-Visual Dialogue"
publish_date: "2026-09-21"
pdf_url: "https://arxiv.org/pdf/2609.21465"
abs_url: "https://arxiv.org/abs/2609.21465"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-22T00:09:47"
---
# OmniVChat: Synthesizing, Benchmarking, and Training for Native Audio-Visual Dialogue

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：omni-modal dialogue · audio-visual dialogue · synthetic data generation · multi-agent data engine

## 一句话总结

论文把「用户以音视频原生输入、omni 模型直接输出文本」形式化为 OmniVChat 任务，并给出配套三件套——多智能体合成数据引擎 OmniVChat-Studio、覆盖五类基础对话能力的评测基准 OmniVChat-Bench（含人类录制版 OmniVChat-Bench-Human）以及联合优化回复正确性、效率、风格的 RL 奖励设计 OmniVChat-RL，通过在 Qwen3-Omni-Instruct 上使用合成对话做 RL 训练，同时提升其在合成基准与人类录制基准上的表现，以证明奖励设计有效且能向真实对话迁移。

## 摘要

> We define OmniVChat (Omni Video Chat) as the task of native audio-visual dialogue between a user and an omni model. In OmniVChat, omni models directly and simultaneously receive audio and video from a user and return text. The user's query is embedded in the audio and video, without a separate text question, external captioning, or speech recognition. Direct audio-visual input reduces external latency and computation while preserving perceptual cues. However, research on OmniVChat faces two constraints: data availability and evaluation. Recordings of people using their own devices are scarce. Furthermore, a good reply often needs to account for the user's surroundings, facial expressions, and nearby objects, and such responses can be expressed in many different ways, making keyword matching unreliable for evaluating reply quality. Recent progress in agent systems and video generation makes generation for comprehension viable, which means using synthesized dialogues for training and evaluation. Therefore, we present OmniVChat-Studio, a multi-agent data engine for synthesizing single- and multi-turn audio-visual dialogues. We use synthesized dialogues to build OmniVChat-Bench, an evaluation benchmark that evaluates omni models' basic dialogue abilities across five ability categories. We also present OmniVChat-RL, a reinforcement learning reward design that jointly targets reply correctness, efficiency, and style in OmniVChat. Training Qwen3-Omni-Instruct with OmniVChat-RL on synthesized dialogues improves its performance on both OmniVChat-Bench and the human-recorded OmniVChat-Bench-Human. These gains validate the reward design and show transfer to real-world dialogues in training and evaluation.

Q1: 这篇论文试图解决什么问题？

【1. 任务层面要解决什么】
论文要解决的核心问题不是某个具体模块的性能，而是「原生音视频对话」这一交互范式的可行化。传统多模态对话通常走级联路线：ASR 把语音转文字、captioning/视觉编码器把画面转成描述或特征，再把拼接后的文本送进 LLM。论文把这种级联视为需要被替代的对象，理由是：(a) 外部模块引入额外延迟与算力；(b) 转写与描述过程会丢失感知线索（环境、面部表情、附近物体）。OmniVChat 要求模型直接吃用户同时提供的音频与视频、输出文本，提问本身存在于音视频中，没有独立文字问题——这意味着模型必须自己完成「听清问题 + 看懂场景 + 决定回答」的联合决策（合理推断）。

【2. 约束一：数据稀缺】
论文明确指出「人们在自有设备上使用模型的录制数据非常稀缺」。这不是简单的数据量问题，而是采集范式问题：真实 OmniVChat 数据要求第一人称/近距离视角的音频（用户说话）与视频（用户所处环境、表情、手持物体）同步，并且带有用户真实意图。这种数据天然涉及隐私、场景不可复现、意图标注昂贵，难以规模化。因此论文把「合成数据」从补充手段提升为主路径。

【3. 约束二：评测不可靠】
论文对评测难题的论证有两个关键点，值得单独拎出来：
- 语义依赖外部上下文：好回复需要同时考虑用户周围环境、面部表情、附近物体，也就是说「正确」不是一个仅由文本问题决定的函数，而是由音视频上下文决定的函数。
- 表达多解性：同一语义的正确回复可以有大量不同措辞，因此关键词匹配、n-gram 重叠等文本匹配指标会系统性低估正确回答、误判正确回答。这直接否定了把传统 NLG 自动指标直接搬到 OmniVChat 的做法。

【4. 论文的隐含假设】
- 假设一（generation for comprehension）：由多智能体+生成模型合成的音视频对话，其分布足够接近真实用户对话，可用于训练与评测。这是全文最关键的假设，若合成数据与真实数据存在系统性的场景/口音/设备/语言风格偏移，则基准分数与训练收益都可能被高估。
- 假设二：合成基准上测得的提升与人类录制基准上的提升方向一致（论文用 OmniVChat-Bench-Human 作为该假设的部分检验手段）。
- 假设三（推测）：「效率」与「风格」可以被设计成可优化的奖励信号且不与正确性冲突；论文只声称三者被联合优化，未在摘要层面给出权衡关系。

【5. 边界条件与尚未被回答的问题】
- 该任务限定为文本输出：模型听得见、看得见，但不说、不看图生成。因此它并不覆盖「原生音视频对话」的完整双向形态（例如语音回复、实时打断、副语言反馈）。
- 评测对象限定为「基础对话能力」五类，摘要未说明是否覆盖长时记忆、多轮指代、时间推理、安全对齐等更难的维度。
- 数据由合成而来，因此「真实设备录制数据稀缺」这一根本瓶颈被绕开而非被解决；论文的迁移验证（Human benchmark）是缓解而非消除该问题。
- 论文元数据（arXiv ID 2609.21465、发布日期 2026-09-21）呈现为未来时间戳，本次生成未获得 PDF 正文或参考文献，上述边界条件中关于能力类别、baseline、数据规模的具体内容均需回原文核对。

Q2: 有哪些相关研究？

本次检索没有提供 PDF 正文、参考文献列表或 related work 章节，因此以下是对论文所处研究脉络的重建，属于「合理推断」，引用关系需回原文核对。

【1. 级联式多模态对话系统】
即 ASR + 视觉描述/captioning + LLM 的流水线。论文的动机陈述（减少外部延迟与算力、保留感知线索）实际上是在与这一整条路线对话：级联方案模块化、可解释、可复用，但误差会逐级累积，并且转录与描述是对连续感知的有损压缩。

【2. omni 模型 / 全模态 LLM】
论文直接以 Qwen3-Omni-Instruct 为训练对象，说明其工作位置在「已有的 omni 基座之上」，而非从零训练。这类模型的共同特征是原生接受音频+视频/图像输入。与之相关的还有以语音为输出、以视频为输入的对话模型（推测），以及各类 audio-visual LLM 的对话能力评测工作。

【3. 多模态对话基准与「以生成促理解」】
- 视频/视频问答与视频对话基准：这类工作通常用人工标注或影视素材构建，评测多依赖多选题或文本相似度。论文提出的反驳点（关键词匹配不可靠）正是对这类评测范式的批评。
- 合成数据用于评测：近年来出现用强生成模型合成测试样本以缓解标注瓶颈的做法。论文提出的 generation for comprehension 是这一思路在音视频对话场景的延伸，并把「视频生成」与「智能体系统」同时作为使能技术。

【4. 智能体系统作为数据引擎】
用多智能体流水线（角色扮演、场景生成、对话编排、质量过滤等）自动生产对话数据，与大模型自我博弈/自我指令（self-instruct 类）与角色扮演数据合成路线同源。OmniVChat-Studio 在这一支脉上的差异点在于：合成对象是「音视频+内嵌语音提问」而非纯文本对话，因此需要同时调度视频/音频生成与对话逻辑（合理推断）。

【5. 多模态 RLHF / RLVR 与奖励设计】
OmniVChat-RL 属于「为多模态对话设计复合奖励」的一类工作，把正确性之外的维度（效率、风格）显式写进奖励，这与把帮助性/无害性拆成多目标的 RLHF 传统、以及可验证奖励（RLVR）的路线都有交集。其独特之处在于奖励是面向「音视频条件下的开放文本回复」而非数学/代码等可验证任务——这类任务的正确性奖励本身如何获得，是值得回原文查验的关键技术点（推测其依赖模型裁判或合成数据自带的参考答案）。

Q3: 论文如何解决这个问题？

论文的解法可以概括为「一个任务定义 + 三个组件」，三者构成闭环：合成数据既用于训练也用于评测，RL 在其中承担把「可合成」转化为「可优化」的桥梁。

【1. 任务定义：OmniVChat】
把「用户同时输入音频与视频、模型直接输出文本」定义为任务。关键约束有三：查询内嵌于音视频；不使用外部分词/识别/captioning；模型需要同时利用听觉与视觉上下文生成回复。这一形式化同时定义了后续数据格式（音视频对 + 内嵌语音提问 + 文本回复）与评测输入形态。

【2. OmniVChat-Studio：多智能体数据引擎】
- 形态：多智能体（multi-agent）数据引擎，生成单轮与多轮两类音视频对话。
- 角色分工：摘要未展开具体 agent 名称与流程，推测至少包含场景/情境构造、对话意图与脚本生成、音视频渲染（语音合成 + 视频生成）、以及质量校验/过滤环节。（推测，需回原文核实）
- 设计含义：把「数据采集」换成「数据合成」，从而绕开隐私、场景不可复现与标注昂贵的问题；同时因为生成过程可控，可以为特定能力维度定向造数据，这也是它能支撑五类能力基准与 RL 训练的前提。

【3. OmniVChat-Bench：五类基础对话能力基准】
- 由合成对话构建，用于评测 omni 模型的基础对话能力，划分为五个能力类别。
- 摘要未给出五个类别的具体名称；从论文对「好回复需要顾及环境、表情、附近物体」的论述推测，类别可能覆盖环境/物体感知、面部表情与社会线索理解、时空关系、指令遵循与多轮一致性等方向（推测，需回原文核实）。
- 关键设计取舍：作者明确否定了关键词匹配式评测，这暗示基准采用某种语义级/模型裁判式或人工核验式评分，但具体评分协议在摘要中未披露（信息缺口）。

【4. OmniVChat-Bench-Human：人类录制版本】
作为合成基准的外部效度检验，用于回答「合成数据训练与评测的收益是否只是合成分布的产物」。它是论文论证「迁移到真实世界对话」的核心证据载体。其规模、采集协议、与合成基准的难度对齐方式均为信息缺口。

【5. OmniVChat-RL：面向 OmniVChat 的奖励设计】
奖励被显式拆为三个目标：
- 正确性（reply correctness）：回复是否真正回应了音视频中的查询与情境；
- 效率（efficiency）：推测指向回复的简洁度/冗余度/延迟或长度控制（具体定义需回原文核实）；
- 风格（style）：推测指向表达的自然度、语气与用户情境的匹配度。
这种「正确性之外的显式目标」设计使奖励不再是单一准确率，代价是引入了多目标加权与潜在冲突，摘要未披露权重与消融结果（信息缺口）。

【6. 训练对象与闭环】
以 Qwen3-Omni-Instruct 为训练基座，用 OmniVChat-RL 在合成对话上做强化学习。评测同时落在 OmniVChat-Bench（合成）与 OmniVChat-Bench-Human（真实录制）上，形成「合成数据 → RL 训练 → 双基准验证」的整体技术路线。

Q4: 论文做了哪些实验？

【摘要层面可确认的实验内容】
1. 训练实验：以 Qwen3-Omni-Instruct 为基座，在 OmniVChat-Studio 合成的音视频对话上使用 OmniVChat-RL 进行强化学习训练。
2. 评测实验（合成域）：训练后的模型在 OmniVChat-Bench 上进行评测，覆盖五个基础对话能力类别。
3. 评测实验（真实域）：同一模型在人类录制的 OmniVChat-Bench-Human 上评测，用于检验合成数据训练与评测是否迁移到真实对话。
4. 奖励设计验证：通过上述两组评测结果来论证 OmniVChat-RL 的「正确性+效率+风格」联合奖励是有效的。

【论文实验设计中明显存在的（需回原文确认的）环节】
- baseline 集合：摘要未提及对比了哪些模型（其他 omni 模型、同基座的 SFT 版本、无 RL 版本等）。判断训练收益是否可归因于 RL 而非数据本身，无 RL / SFT-only 对照是必需品。
- 消融：三项奖励的逐项消融、权重敏感性、单轮 vs 多轮、合成数据规模 scaling 曲线，摘要均未提及。
- 评测协议：OmniVChat-Bench 如何打分（模型裁判？人工？参考答案从何而来？）未披露——这直接决定了结论强度，因为该基准本身就是论文提出的。
- 人类基准的规模与统计：OmniVChat-Bench-Human 的样本量、采集人群、设备与场景分布未披露，无法判断提升是否具有统计显著性与人口覆盖度。
- 是否报告了效率类指标（token 数、延迟）的实测值：文中把「效率」作为奖励目标，但摘要未给出任何延迟/长度数字，需回原文核对是否存在 latency 或输出长度的实测对比。

【行内校准】
以上「可确认」部分全部来自摘要的明确陈述；「需确认」部分是本次未获得 PDF 正文导致的信息缺口，不应据此推断论文未做相关工作，也不应假定其做过。

Q5: 发现了什么实验现象？

【可确认的现象】
1. 方向一致的迁移现象：在合成对话上用 OmniVChat-RL 训练后，模型在合成基准 OmniVChat-Bench 与人类录制基准 OmniVChat-Bench-Human 上均出现提升。这是摘要中唯一被明确报告的现象，也是全文论证的支点。
2. 训练信号可由合成数据提供：结果支持「合成对话可以充当 RL 的有效训练分布」这一判断，即合成数据不仅可用于评测，也可用于优化。
3. 复合奖励可优化：正确性、效率、风格三项被联合优化后没有出现「奖励不可优化」的情况（至少从最终指标看是有效的）。

【反直觉之处与值得追问的张力】
- 最值得注意的张力是：训练数据完全合成、评测之一也是合成的，却仍在人类录制集上提升。这一现象有至少两种解释——(a) 合成数据确实覆盖了可迁移的能力（真实收益）；(b) 人类基准与合成基准被刻意对齐了场景/任务分布，使得「人类录制」更多体现在采集方式而非难度分布上（评测构造带来的收益）。二者无法从摘要中区分，需回原文查看两套基准的能力类别是否一一对应、样本是否同源意图。
- 效率与正确性的潜在冲突：把「效率」写进奖励通常会在某一点之后压低信息量。摘要未报告效率与正确性之间的此消彼长曲线，也未给出任何长度/延迟指标，因此无法判断效率奖励是否以牺牲正确性为代价（推测该张力存在，需回原文核对）。
- 风格奖励的可度量性：风格是最容易被奖励模型「钻空子」的维度，摘要未披露风格奖励是模型裁判、规则还是人工，因此无法判断是否存在 reward hacking 风险（信息缺口）。

【负结果与失败模式】
摘要未报告任何负结果、失败案例或反例（例如某能力类别未提升、某场景类型退化）。对一篇同时提出数据、基准和训练方法的工作而言，缺少失败模式披露会削弱「迁移」这一结论的可信度，建议在精读时优先查找逐能力类别的分数表，观察是否存在类别间不均衡（合理推断该风险存在）。

Q6: 有什么可以进一步探索的点？

【1. 从「文本回复」推进到真正的原生输出】
论文的 OmniVChat 只要求输出文本。最自然的延伸是让模型同时输出语音（带副语言：语气、停顿、打断、回应音），形成双向原生音视频对话。这会重新引入延迟与流式交互的约束，也把「效率」奖励从文本长度层面扩展到端到端响应时延。

【2. 合成数据的真实性校准】
当前核心假设是合成数据可代表真实用户对话。可探索的方向包括：合成器与真实分布的对齐度量（场景、口音、设备、光照、语言风格）、在合成数据上显式注入域随机化/难度分层、以及用少量真实数据做校准（如重要性加权、混合训练）。论文的 OmniVChat-Bench-Human 只是粗粒度检验，缺少「何种合成属性导致真实域收益」的因果分析。

【3. 评测范式的进一步形式化】
论文批评关键词匹配不可靠，但自身基准的评分协议未在摘要中披露。可继续推进：将「回复质量」分解为可核查的子维度（情境引用是否正确、物体/表情识别是否正确、是否回答了隐含意图），并研究模型裁判与人工评判的一致性、以及基准被训练数据污染（contamination）后的失效模式。

【4. 奖励设计的深层问题】
- 正确性奖励在开放音视频对话中如何自动获得（模型裁判？合成数据自带真值？多候选投票？）是关键可扩展性问题。
- 效率与风格奖励的权重、冲突与 reward hacking 边界。
- 把奖励从「文本」扩展到「多轮行为」（是否在正确时机追问、是否保持人格一致）。

【5. 多智能体的可扩展性与数据飞轮】
OmniVChat-Studio 作为多智能体引擎，可以探索自演进闭环：用当前模型的失败案例反哺合成器生成更难样本（困难样本挖掘 / 对抗式数据生成），并研究合成数据规模与能力提升之间的 scaling 关系。这一方向与用户关注的 agent 方向直接相关。

【6. 跨领域迁移】
该「多智能体生成数据 → 用于训练与评测 → RL 优化复合奖励」的方法论并不绑定于日常对话场景。可迁移到需要主观判断且人工标注昂贵的领域（如科学观察记录、实验操作指导、医疗/教学场景的第一人称音视频问答）。注意：这类迁移会引入安全与合规约束，且对「正确性」的定义更依赖专家，属于推测性延伸。

【7. 边界与安全】
始终开启的音视频输入带来的隐私、旁观者同意、被动录制等问题，在「用户使用自有设备」这一数据设定下尤为关键，是可预期的后续研究议题。

Q7: 总结一下论文的主要内容

论文围绕一个被作者新定义的任务 OmniVChat（Omni Video Chat）展开：omni 模型同时直接接收用户提供的音频与视频，并输出文本；用户的提问内嵌在音视频之中，不提供单独的文字问题，也不借助外部的语音识别或画面描述模块。作者给出的动机有两条：一是减少外部模块带来的延迟与算力开销，二是避免级联式转写/描述对感知线索（用户所处环境、面部表情、身边物体）的有损压缩。

论文认为推进这一方向卡在两个瓶颈上。其一是数据：真实世界中「人们用自己设备与模型自然对话」的录制数据非常稀缺，这类数据涉及隐私、场景不可复现与意图标注困难，难以规模化。其二是评测：一条好回复往往需要同时考虑用户周围环境、表情和附近物体，而且同一语义可以用大量不同方式表达，因此关键词匹配一类指标不可靠，会系统性误判。

作者的破题思路是一个范式判断——「generation for comprehension」：近期智能体系统与视频生成的进展，使得用合成对话来训练和评测理解能力变得可行。围绕这一判断，论文交付三件套：

第一，OmniVChat-Studio，一个多智能体数据引擎，用来合成单轮与多轮音视频对话。它同时承担训练数据来源与基准构建来源两种角色。摘要未展开各个 agent 的具体分工与视频/语音生成细节（推测包含情境构造、对话编排、音视频渲染与质量过滤等环节）。

第二，OmniVChat-Bench，基于合成对话构建的评测基准，从五个能力类别考察 omni 模型的基础对话能力；此外还有人类录制版本 OmniVChat-Bench-Human，用于检验结论是否能离开合成分布。两套基准的存在方式说明作者的论证策略是「合成训练 + 合成评测 + 真实评测」三点定位，把人类录制集当作外部效度锚点。

第三，OmniVChat-RL，一套面向 OmniVChat 的强化学习奖励设计，显式地把回复正确性、效率与风格三个目标写进奖励。这使优化目标不再是单一准确率，代价是多目标之间的潜在冲突与权重问题——摘要未披露权重与逐项消融。

技术主线由此闭合：多智能体合成数据 → 复合奖励的 RL 训练 → 在合成基准与人类录制基准上双重评测。实验上，作者用 OmniVChat-RL 在合成对话上训练 Qwen3-Omni-Instruct，报告其在 OmniVChat-Bench 与 OmniVChat-Bench-Human 上均有提升，并据此认为奖励设计得到验证、且收益可迁移到真实世界对话。

论文的贡献在结构上是「任务定义 + 数据引擎 + 评测基准 + 训练方法」的组合型工作，符合系统性研究而非单点改进的形态。需要保留的怀疑点有三：其一，合成数据代表真实分布这一前提没有被摘要层面的证据充分支撑，人类基准的提升也可能部分来自两套基准的分布对齐设计；其二，基准由论文自身提出并用于验证论文自身的方法，评测协议（打分方式、参考答案来源）未在摘要中披露，结论强度取决于此；其三，效率与风格奖励是否与正确性发生权衡、是否存在 reward hacking，摘要未报告任何消融或失败案例。本次生成未获得 PDF 正文与参考文献，上述所有具体机制、数值、baseline 与能力类别名称均需回原文核实。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向直接相关（权重 0.10）：OmniVChat-Studio 是一个多智能体数据引擎，其核心问题——多个智能体如何协同合成可控、可验证、可定向造难的数据——是 agent 系统中数据生产链条的典型形态，可迁移到其他主观性强、标注昂贵的数据域。

## 基本信息

- 作者：Haolin He, Yunfei Chu, Qi Chen, Wen Huang, Yuan Feng, Muzhi Zhu, Zheqi Dai, Haoning Xu, Dongchao Yang, Chunyat Wu, Zining Liang, Zhengxi Liu, Xiquan Li, Xie Chen, Xize Cheng, Qize Yang, Jin Xu, Qiuqiang Kong
- 机构：未提供
- 来源：arxiv
- 主题/分类：eess.AS, cs.AI, eess.IV
- 日期：2026-09-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.21465`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次未获得任何 PDF 检索证据（retrieved_evidence 与 field_evidence_map 均为空，正文章节文本也为空），全部内容基于标题、作者列表与摘要撰写，涉及方法细节、能力类别、评分协议与实验数值的部分均标注为推断或信息缺口，需回原文核实；另注意论文元数据（arXiv ID 2609.21465、2026-09-21）呈未来时间戳，建议先确认出处。
