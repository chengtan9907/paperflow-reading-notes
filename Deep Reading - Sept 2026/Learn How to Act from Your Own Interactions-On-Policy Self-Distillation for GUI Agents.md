---
user_id: "cheng tan"
paper_id: 13086
arxiv_id: "2609.27307"
title: "Learn How to Act from Your Own Interactions: On-Policy Self-Distillation for GUI Agents"
publish_date: "2026-09-24"
pdf_url: "https://arxiv.org/pdf/2609.27307"
abs_url: "https://arxiv.org/abs/2609.27307"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-24T12:09:16"
---
# Learn How to Act from Your Own Interactions: On-Policy Self-Distillation for GUI Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gui agent · on-policy self-distillation · multi-turn interaction · privileged information

## 一句话总结

GUI-SD-v2 提出一个两阶段训练框架，把 on-policy self-distillation（OPSD）从单步 GUI grounding 扩展到多轮 GUI 交互：第一阶段在同一 GUI 状态下对「有特权指导」与「无特权指导」的 rollout 做联合优化以强化特权跟随能力，第二阶段用 privilege-conditioned self-teacher 对 step-specific reasoning 与 memory guidance 做选择性蒸馏，从而在 AndroidWorld 与 MobileWorld 上同时改善 Pass@1 与 Pass@3 成功率。

## 摘要

> Graphical User Interface (GUI) agents enable the fulfillment of complex user instructions through multi-turn interactions with software environments, requiring step-wise reasoning and long-horizon memory to guide actions and retain task-relevant information, respectively. Recent on-policy self-distillation (OPSD) methods have achieved strong performance on GUI grounding, a foundational subtask for GUI agents, owing to dense token-level supervision from privilege-conditioned self-teachers. However, extending existing OPSD methods to multi-turn GUI agents is hindered by self-teachers' limited privilege-following ability and insufficient privileged guidance. In this paper, we introduce GUI-SD-v2, the next version of GUI-SD, which extends OPSD from GUI grounding to multi-turn GUI interaction and addresses key limitations through a two-stage training framework. Specifically, GUI-SD-v2 first strengthens privilege following by jointly optimizing rollouts with and without privileged guidance from the same GUI states. Furthermore, it selectively distills step-specific reasoning and memory guidance through a privilege-conditioned self-teacher, supporting action decisions and the retention of task-relevant information for subsequent interactions. Extensive experiments on two representative GUI agent benchmarks, AndroidWorld and MobileWorld, show that GUI-SD-v2 compares favorably with existing OPSD baselines while consistently outperforming the evaluated state-of-the-art methods in both Pass@1 and Pass@3 success rates. Code and training data will be publicly released.

Q1: 这篇论文试图解决什么问题？

【问题定位】论文要处理的是「多轮 GUI 交互」这一层级的学习问题，而不是单步 GUI grounding。摘要给出的因果链是：GUI agent 完成复杂指令需要 (a) step-wise reasoning 来产生每一步动作，(b) long-horizon memory 来在长轨迹中保留任务相关信息；GUI grounding 是这一体系的基础子任务；OPSD 已在 grounding 上被验证有效；但把 OPSD 迁移到多轮交互时失效或收益下降。

【瓶颈一：self-teacher 的 privilege-following 能力不足】OPSD 的监督信号来自 privilege-conditioned self-teacher——即同一模型在额外「特权信息」条件下的输出。若教师本身不能稳定、正确地利用这些特权信息，蒸馏出来的 token 级目标就会退化甚至带偏。论文把「特权跟随能力」本身当作一个需要训练的能力来对待（第一阶段），这实际上是在质疑 OPSD 的一个隐含前提：教师天然比学生更会使用特权信息。在多轮场景下这个前提更脆弱，因为特权信息要跨越多个时间步、多个界面状态被持续消费，误差会在轨迹上累积。

【瓶颈二：特权指导不足】第二个瓶颈指向特权信号的供给端：不仅教师要会用，还需要有足够数量、足够粒度、与多轮结构对齐的特权指导。单步 grounding 的特权信号（例如准确的目标元素/坐标）相对容易构造；多轮交互中「正确的下一步动作」往往不唯一，且推理与记忆是隐变量而非可观测标注，因此特权指导可能稀疏且覆盖不到推理链路与记忆维护环节。摘要中的「selectively distills step-specific reasoning and memory guidance」正对应这一缺口：需要把特权指导拆成「推理指导」和「记忆指导」两类，并且按步骤选择性地施加。

【为什么多轮场景让两个瓶颈更致命】合理推断：多轮任务中监督具有 token 级稠密性与轨迹级稀疏性的双重结构，教师的一个错误 token 级偏好可能被后续若干步放大；同时记忆类能力（保留先前界面/约束/中间结果）几乎没有直接的 token 级标签，只能通过特权上下文间接蒸馏。此外，若教师与学生共享同一状态分布，则学生一旦偏离教师「假设的状态」，特权信息与当前状态的对齐关系就会失效——这正是 on-policy 设定中 privilege 与分布漂移的经典张力。

【隐含假设】1）同一模型在特权条件下的表现可以作为可靠的上界参考，即特权教师的错误不会系统性污染训练（论文以第一阶段来缓解，但摘要未说明是否验证过这一点）；2）推理与记忆可以被显式划分为两类可分别蒸馏的指导信号；3）Pass@1 与 Pass@3 的提升能代表多轮能力的整体提升，而非仅采样多样性改善（推测，摘要未给出方差或显著性信息）。

【问题定义层面的待确认项】摘要没有说明：任务成功的判定标准、最大交互步数、特权信息的具体形态（人工标注 / 环境真值 / 更强模型的解释）、以及「privileged guidance 不足」是被诊断为覆盖不足、粒度不足还是质量不足。这些细节需要回到原文 Method 与实验设置部分核对；本次未获取 PDF 正文，故上述归因的精确措辞应以原文为准。

Q2: 有哪些相关研究？

【说明】本次未获取论文 PDF 正文，因此下文的既有研究脉络是按摘要关键词与领域通行图景整理，属于背景性梳理与合理推断，不代表论文 Related Work 的实际引用结构，具体引用关系与对比对象需回原文核对。

【GUI agent 与多轮交互任务】GUI agent 的典型设定是：给定自然语言指令，agent 在软件环境（移动端或桌面端）中读取界面（截图、accessibility tree 或 view hierarchy），输出点击、输入、滑动等动作，并在有限步数内完成任务。AndroidWorld 与 MobileWorld 是摘要明确点出的两个代表性基准，二者都把「最终是否完成任务」作为主要评测信号，并支持多次采样（对应 Pass@k）。

【GUI grounding 作为基础子任务】grounding 指把指令中的指代映射到界面上的具体元素或坐标，是动作执行的前置条件。它天然是单步、可标注的任务，因此成为各类监督方法（含 self-distillation）最先落地的位置。论文的叙事即以此为起点：先在 grounding 上成功，再向多轮交互迁移。

【on-policy self-distillation 与特权信息蒸馏】OPSD 的范式是：学生在自身策略分布上采样轨迹，教师（通常是与学生同源的模型，但额外看到 privileged information）在这些轨迹上提供 token 级监督。这与经典 offline KD（在固定数据集上模仿教师输出）不同，也与纯 RL（仅用稀疏结果奖励做信用分配）不同——它把稠密的、分布内（on-policy）的 token 级信号注入训练。这一思路与以下邻近方向存在概念交叠：asymmetric actor-critic 中给 critic 更多信息、teacher-student 特权信息蒸馏、以及近期把推理过程（rationale）纳入蒸馏目标的工作。摘要中「privilege-conditioned self-teacher」的表述提示教师与学生同源、差别在条件输入。

【长时程记忆与逐步推理】多轮 GUI 任务的另一条研究线是 memory 与 reasoning 的显式建模：把历史轨迹压缩为摘要、维护任务状态笔记、或让模型在动作前生成 subgoal。论文把 memory guidance 与 reasoning guidance 并列为两类可蒸馏信号，说明它倾向于把二者作为可干预的训练目标而非仅靠上下文长度自然获得。

【范式竞争关系】可辨识的竞争范式至少有三类：(1) 纯监督微调/轨迹模仿——数据成本高、易受分布漂移影响；(2) 纯 RL（结果奖励）——奖励稀疏、多轮信用分配困难；(3) OPSD——试图以特权教师的稠密监督绕开稀疏奖励，但受教师能力上限约束。GUI-SD-v2 属于在第 (3) 类内部做修补与扩展（先修教师，再修监督供给），而非提出全新范式；这一判断是基于摘要措辞的合理推断。

Q3: 论文如何解决这个问题？

【总体框架】GUI-SD-v2 是 GUI-SD 的下一代版本，目标是把 OPSD 的应用范围从 GUI grounding 扩展到多轮 GUI 交互。整体是一个两阶段训练框架，两个阶段分别对应摘要中诊断出的两个瓶颈：阶段一解决教师「不会用特权」，阶段二解决「特权指导不够用/不够准」。

【阶段一：用同一状态下的双路 rollout 强化特权跟随】论文明确提出「jointly optimizing rollouts with and without privileged guidance from the same GUI states」。可读出两个设计要点：1）数据来自同一 GUI 状态的两条路径——一条带特权指导、一条不带；2）两条路径被联合优化，而非只训练带特权的那条。合理推断：这种配对设计的目的是让模型在同一状态上把「有特权条件」与「无特权条件」的行为对齐，从而提升教师/模型对特权信息的利用效率，同时抑制把特权当作捷径（shortcut）的倾向。这一步训练的产物应是一个「更会跟随特权」的模型，使其可以作为后续阶段可靠的 self-teacher。

【阶段二：选择性蒸馏 step-specific reasoning 与 memory guidance】阶段二是 OPSD 主体：由 privilege-conditioned self-teacher 产生监督，学生在其自身策略采样得到的轨迹上学习。与原始 OPSD 的差别在于监督被「选择性地」拆分为两类——step-specific reasoning guidance 支撑当前步的动作决策，memory guidance 支撑任务相关信息在后续交互中的保留。「选择性」的具体含义（按 token 位置选择、按步骤类型选择、还是按信号置信度选择）摘要未说明，属需要回原文核对的关键细节。合理推断：若无选择性筛选，推理与记忆两类信号会在同一批 token 上互相干扰，或把教师用来解释特权信息的措辞泄漏成学生可直接利用的提示。

【与 v1 的差异（摘要可见部分）】GUI-SD（v1）聚焦 grounding；v2 把问题域扩到多轮交互，并在 OPSD 之前插入了「先训练特权跟随」的预热阶段，同时把蒸馏目标细化为推理与记忆两条流。

【关键设计权衡与待确认细节】1）计算成本：双路 rollout 意味着同一状态至少生成两条轨迹，训练开销与数据效率需要权衡；2）特权信息的构造方式与成本，是决定这套方法能否规模化复现的核心变量；3）学生-教师容量/条件差距过大时，蒸馏目标是否退化为噪声；4）两阶段之间是否冻结、是否迭代、阶段一是否可能损害通用能力（摘要完全未涉及，均属推测性风险点）；5）损失组合权重、蒸馏温度、轨迹采样步数等超参数在摘要中没有任何信息。

Q4: 论文做了哪些实验？

【基准】论文在两个「代表性」GUI agent 基准上做评测：AndroidWorld 与 MobileWorld。二者均为多轮移动端 GUI 交互类评测，具体任务数量、最大步数、环境版本在摘要中未给出。

【对比对象】摘要提到两类比较：1）existing OPSD baselines（即此前基于 on-policy self-distillation 的方法，可能包括 GUI-SD v1 一脉的方法，具体清单需回原文核对）；2）the evaluated state-of-the-art methods（措辞为「所评估的 SOTA」，限定语说明这是作者所选定的对比集合，而非全领域穷举）。

【报告指标】Pass@1 与 Pass@3 成功率。论文声称在这两个指标上「consistently outperforming」所评估的 SOTA，并「compares favorably with existing OPSD baselines」。注意「compares favorably」是相对模糊的表述，不像「outperforms」那样明确，因此与 OPSD baseline 的差距幅度与显著性需要以原文表格为准。

【资源承诺】代码与训练数据将公开（摘要原文为 will be publicly released），这对复现与后续研究是正向信号，但当前无法核验。

【缺失的实验信息（重要缺口）】摘要层面完全没有提供：base model 的规模与来源、训练数据规模与来源、特权信息的具体构造流程、训练算力与时长、消融实验（阶段一是否必要、memory guidance 与 reasoning guidance 各自贡献、选择性蒸馏准则的影响）、失败案例与错误类型分布、跨基准的迁移结果、以及任何具体数值与置信区间。因此本字段只能确认「实验做了什么」，无法确认「效果有多好」。

Q5: 发现了什么实验现象？

【可确认的结论性观察】1）在两阶段框架下，GUI-SD-v2 在 AndroidWorld 与 MobileWorld 上的 Pass@1 与 Pass@3 均被报告为优于所评估的 SOTA 方法；2）相比既有 OPSD baseline，论文使用「compares favorably」这一较弱表述，暗示优势可能存在但幅度或在部分设置下并不悬殊（这是从措辞做的合理推断，非论文陈述）；3）结论覆盖两个不同基准，说明提升至少在两个评测环境下可重复出现。

【指标之间的张力与可读出的隐含趋势】Pass@1 与 Pass@3 被同时报告且同时被声称提升，这一组合本身有信息量：如果提升只出现在 Pass@3，通常意味着能力没变而采样多样性/探索变好；如果两者同升，通常意味着策略本身变强（合理推断）。但摘要没有给出 Pass@3 与 Pass@1 的差距变化，因此无法判断提升是「整体平移」还是「分布尾部变化」，也无法判断是否存在 Pass@1 提升但 Pass@3 饱和或反降的情形。

【论文叙事中的因果假设】摘要的叙事隐含一个因果论断：把 OPSD 推广到多轮时的失败源于 (a) 教师特权跟随弱、(b) 特权指导不足；两阶段设计分别针对这两点，并因此带来提升。这属于机制性解释，摘要中并未给出对应消融证据（例如只做阶段一、只做阶段二、去掉 memory guidance 的对照结果）。若原文没有这类消融，则该因果归因应被视为待验证假设，而非已证结论。

【未报告的观察维度】失败案例与失败模式（如长步数任务是否更容易失败）、随交互步数的成功率衰减曲线、推理与记忆能力的分别评测、跨应用/跨平台泛化、特权信息泄漏或 reward hacking 的检查、以及与 base model 规模的关系（scaling trend）——这些在摘要中均无信息，需要回原文确认是否存在相应分析。

Q6: 有什么可以进一步探索的点？

【基于论文自身缺口的延伸】1）特权信号的自动化构造：如果多轮场景中「特权指导不足」是核心瓶颈，那么如何低成本、可扩展地生成可靠的 step-specific reasoning 与 memory 级特权信号，是决定该方法能否规模化的关键；摘要未说明当前构造方式，这本身就是一个明确的研究接口。2）选择性蒸馏准则的显式化：什么 token、什么步骤该被蒸馏、置信度阈值如何设定，可直接转化为可研究的方法学问题（例如按不确定性、按教师-学生分歧度、按步骤类型路由）。3）教师能力上限的刻画：把 privilege-following 当作可训练能力之后，自然的问题是学生能超过教师吗？是否存在蒸馏天花板？需要设计实验把「教师质量」与「学生容量」解耦。

【评测层面的延伸】4）过程级评测：当前只有 Pass@1/Pass@3 这类结果指标，缺少步骤级正确率、记忆保真度、子目标完成度等过程指标；若 memory guidance 确实生效，应可在中间状态的可诊断指标上观察到（合理推断）。5）长尾与失败分析：长步数任务、需要跨应用切换的任务、需要中止/回溯的任务是否受益，摘要未涉及。6）统计严谨性：多次采样下的方差、置信区间与显著性检验是这类小样本基准的必要补充。

【范式层面的延伸】7）与 RL 的结合：OPSD 用稠密特权监督替代稀疏奖励，一个自然的对照是「OPSD + RL 混合」，以及能否用自蒸馏信号做过程奖励模型的替代。8）跨域迁移：同一「先对齐特权条件、再选择性蒸馏推理/记忆」的配方是否能迁移到 web agent、OS agent、工具调用 agent，推而广之到长时程科学工作流 agent（该迁移属推测性建议，需自行评估任务结构差异）。9）成本与效率：双路 rollout 与两阶段训练的开销、推理期是否需要特权（应当不需要，但需确认）以及能否用小教师/大教师组合降低成本。

【风险与边界】10）安全性：特权信息若来自环境真值，训练期可见性是否会在部署期造成不一致行为；以及蒸馏是否会把教师的错误系统性固化（这是所有 self-distillation 方法的共同风险，值得单独检验）。

Q7: 总结一下论文的主要内容

【论证主线】论文的起点是一个已被验证有效的技术：on-policy self-distillation（OPSD）在 GUI grounding——GUI agent 的基础子任务——上表现良好，其成功机制被归因于 privilege-conditioned self-teacher 提供的稠密 token 级监督。接着论文提出一个观察性论断：把 OPSD 从 grounding 扩展到多轮 GUI 交互并不顺利，原因有两处——self-teacher 的 privilege-following 能力有限，以及特权指导（privileged guidance）供给不足。由此推出方法动机：要先把「教师是否会用特权」这件事解决，再把「怎么把特权转化为推理与记忆指导」这件事解决。这条论证链是摘要中明确给出的；其中对失败的归因属于作者的解释性论断，摘要层面未附消融证据。

【技术主线】GUI-SD-v2 的解法是两阶段训练框架，整体定位为 GUI-SD 的下一代版本。阶段一针对特权跟随能力：在同一 GUI 状态下，联合优化「带特权指导」与「不带特权指导」的两条 rollout，让模型学会在特权条件下与无特权条件下都正常行动，从而得到一个更可靠的特权教师。阶段二针对监督供给：由 privilege-conditioned self-teacher 选择性地蒸馏两类指导——step-specific reasoning guidance 用于支撑当前步的动作决策，memory guidance 用于在后续交互中保留任务相关信息。与 v1 相比，覆盖范围从单步 grounding 扩到多轮交互，并在 OPSD 之前增加了特权对齐的预热阶段，同时把蒸馏目标拆成推理与记忆两条流。摘要未披露损失函数、选择准则、超参数与特权信息的构造方式，这些是回原文时最需要确认的技术细节。

【实验主线】评测在两个 GUI agent 基准上进行：AndroidWorld 与 MobileWorld。对比对象包括既有 OPSD baseline 与作者所评估的 state-of-the-art 方法。报告指标为 Pass@1 与 Pass@3 成功率。结论是：相对 OPSD baseline 表现更优（摘要用词为 compares favorably），并在两个指标上一致超过所评估的 SOTA 方法。代码与训练数据承诺公开。摘要没有给出任何具体数值、模型规模、训练数据规模或消融结果，因此本总结能确认的是实验的骨架而非实验的强度。

【可读出的方法论立场】这篇工作不是在提出新范式，而是在既有 OPSD 范式内部做两项修补：把「教师质量」当作可训练变量，把「监督信号供给」当作可设计变量。这种把瓶颈从算法搬到「教师-数据」两端的问题重构，是它相对同类扩展工作的主要差异点（此判断基于摘要措辞的合理推断）。同时，其成立前提是：同一模型在特权条件下确实能提供比自身无特权状态更优的目标分布——这一前提在长轨迹、多步误差累积的场景下是否稳健，是阅读与复现时最值得质疑的地方。

【本总结的证据边界】以上全部内容来自论文标题、摘要与启发式草稿；本次未获取 PDF 正文，sections 为空、无 PDF 语义检索命中，因此方法细节、实验数值、局限性讨论均无法核验，任何涉及具体数字与配方的描述都属于缺口而非省略。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你画像中权重最高的 agent 方向直接重合：主题是多轮 GUI agent 的训练方法，而非单步感知任务。

## 基本信息

- 作者：Yan Zhang, Daiqing Wu, Huawen Shen, Liang Li, Gang Cao, Zhi Gong, Wei Dai, Xiaode Zhang, Can Ma, Yu Zhou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-09-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.27307`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次未获取 PDF 正文与任何 PDF 语义检索证据（retrieved_evidence 与 field_evidence_map 均为空），全部内容基于论文标题、摘要与启发式草稿整理，方法细节与实验数值均未经原文核验，且论文元数据（arXiv 编号与发布日期）建议回原站核对。
