---
user_id: "cheng tan"
paper_id: 15814
arxiv_id: "2610.12789"
title: "EvoBelief: Falsifiable World Models for Self-Improving LLM Agents"
institution: "根据论文通讯作者邮箱（swang47@tulane.edu 与 gujiang@amazon.com）可推断作者单位涉及 Tulane University（图兰大学）与 Amazon（亚马逊）；其余作者的单位信息在提供的正文片段中未出现，完整机构列表请核对论文首页脚注。"
publish_date: "2026-10-12"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.12789.pdf"
pdf_url: "https://arxiv.org/pdf/2610.12789v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T13:21:56"
---
# EvoBelief: Falsifiable World Models for Self-Improving LLM Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world model · self-improving agent · falsifiable hypothesis · belief evolution

## 一句话总结

EvoBelief 提出在 LLM 智能体部署期间进化其环境信念：把环境知识表示成可证伪假设（C∧A⇒Y），经意外触发、被动/主动证据验证、阈值提交、反例修订或退役，在语言模型参数完全冻结的条件下实现跨任务复用的自我改进，并在 ALFWorld、MiniGrid、DiscoveryWorld 等基准上显著超过对比方法。

## 摘要

> Self-evolving LLM agents typically improve how they act through accumulated experience, reflection, or skills. Yet even procedurally capable agents can fail when their beliefs about environment dynamics are incomplete, incorrect, or outdated. We study a complementary axis of self-evolution: evolving those beliefs during deployment. Such beliefs can support multiple tasks, but incorrect ones can also propagate across them. We introduce EvoBelief, a self-evolving world model that represents environment knowledge as falsifiable hypotheses. Unexpected outcomes and decision-relevant unknowns trigger hypothesis formation. Later observations and controlled experiments provide independent validation before a rule can guide predictions or plans within its supported scope. Counterexamples narrow or retire rules as the environment changes. The language model remains frozen, and each rule remains linked to its evidence and the decisions it changes. Across four benchmarks spanning static dynamics, hidden drift, and scientific discovery, EvoBelief improves task performance and reuses learned rules across tasks. On ALFWorld without actor-side transition memory, it improves success over the matched Frozen World Model by 23.9% and 31.3% with Claude Haiku 4.5 and GPT-5.4, respectively. On MiniGrid, it revises stale rules and recovers after unannounced dynamics changes. In DiscoveryWorld's Reactor Lab, it discovers and validates quantitative laws, completing 83.3% of Normal-tier and 58.3% of Challenge-tier instances, versus 0% for each evaluated baseline. These results demonstrate self-improvement through testable, revisable environment knowledge rather than model-parameter updates.

Q1: 这篇论文试图解决什么问题？

论文要解决的问题是：现有自进化 LLM 智能体只进化“如何行动”，而不进化“对环境的信念”，导致在新环境或动态变化环境中即使行动能力完备仍会失败，且错误信念会跨任务传播。

1) 现有自进化机制的盲区：
- 主流机制（经验轨迹学习、反思、长期记忆、技能蒸馏）针对的是策略/程序层面的失败，即 agent 知道环境如何运转但不知道该怎么行动；
- 但 agent 的行动还依赖其关于环境动态的先验信念，而这些先验来自预训练数据，在新环境/局部环境中可能不完整、不正确或过时；
- 论文用“合成化合物 X”的例子说明：智能体已经掌握加热、纯化、分析等操作能力，却因先验错误地认为 X 在 120°C 以下稳定（实际 100°C 即分解）而失败。

2) 任务级修正为何不够：
- “改用更低温度”这样的任务特定修正只解决眼前失败，无法沉淀为可复用的环境属性知识；
- 环境信念是跨任务资产：关于 X 热稳定性的知识不只服务于合成，还能指导运输、存储、分析等相邻任务；
- 反之，错误信念也会跨任务传播，造成更广的失败面。

3) 部署期信念进化这个轴尚未被系统研究：
- 需要一个持久化的世界模型 Wt，它随部署证据更新，又不受 episode 重置影响；
- 需要把知识表示成可检验形式，避免把未经证实的先验或偶然相关性当作规则；
- 需要让规则可修订、可退役，以应对环境动态漂移；
- 需要在不更新模型参数的前提下完成这一切，从而保留预训练通用能力。

4) 形式化难点：
- 环境存在与冻结 LLM 先验不一致的局部动态，且可能在部署中途改变；
- 状态 s 是隐式的，只能通过观测 o 与动作 a 的互动间接推断；
- 需要区分“当前情境是什么”（episode 级）与“环境如何响应动作”（持久级）两类知识，二者更新节奏不同。

Q2: 有哪些相关研究？

论文的相关工作可从五条线梳理：

1) 行为层面的自我进化 LLM 智能体（论文引言直接引用的主线）：
- 从历史交互轨迹中学习策略；
- 通过反思修正行为（对应 Shinn et al. 2023 一类 Reflexion 风格工作）；
- 积累长期记忆与角色化经验（对应 Park et al. 2023 一类生成式智能体工作）；
- 把经验蒸馏为可复用技能库（对应 Wang et al. 2023 一类 Voyager 风格工作）；
- 共同点：改进对象是 π(a|s,g) 而不是对环境本身的信念，都假设环境动态已知或可由行动经验直接补偿。

2) 世界模型与测试时环境适应：
- 论文将 EvoBelief 放在“世界模型随部署进化”这一更小分支中；
- 对比方法 WorldEvolver 代表世界模型进化的既有方案，EvoBelief 在同样设定下与之对比；
- 区别在于 EvoBelief 把世界知识写成可证伪、带证据链、可退役的规则，而非隐式参数化记忆（合理推断 WorldEvolver 采用某种参数化或隐式世界模型更新，具体机制需查原文，因为检索到的片段只出现其作为 baseline 的名字）。

3) 推理-行动框架：
- ReAct 作为基础 agent 范式被用作 baseline，说明 EvoBelief 不是替换推理循环，而是在其上加装独立的信念管理系统；
- Frozen World Model（Frozen WM）是作者自建的匹配对照：用与 EvoBelief 相同的 W-blind actor 与 predictor，但不允许任何学习规则参与预测/规划，以隔离“规则知识”本身的贡献。

4) 科学发现与环境交互基准：
- ScienceWorld：文本交互式科学实验环境（静态、先验大体一致）；
- ALFWorld：家务操作环境（静态、先验大体一致）；
- MiniGrid：迷宫导航，含有未经宣布的隐藏动态漂移；
- DiscoveryWorld：科学发现任务套件，其中 Reactor Lab 要求智能体从测量中归纳并验证定量/分类规律。

5) 可证伪性/科学方法启发的 AI 学习（从方法设计合理推断的相关脉络）：
- 论文把 Popper 式可证伪原则工程化为假设提交阈值（独立验证次数 τtest、绑定次数 τbind、证据次数 τep）；
- 与主动学习、实验设计、贝叶斯假设检验存在概念交集，但论文强调用 LLM 生成假设、用环境执行测试、用规则库沉淀结论；
- 语言模型在这里的角色是“使假设变便宜”：生成候选规则的成本远低于人工假设（该判断来自检索片段“language models make hypotheses cheap to…”）。

注意：第 5 条部分属于对方法设计的合理推断；论文正文引用列表在提供的片段中不完整，若要写正式综述需对照 Introduction/Related Work 原始引文。

Q3: 论文如何解决这个问题？

EvoBelief 的解决方案是把“环境信念”外部化为一个可证伪假设库，并设计完整的假设生命周期。

1) 问题形式化与双状态设计：
- 环境：隐状态 st∈S、观测 ot∈O、动作 at∈A、目标 g、交互历史 ht；
- 冻结参数 θ 下，agent 维护外部化状态 bt=(zt, Wt)：
 - zt：情境状态，记录当前局面中被认为为真的实体、关系、数量与任务进度，每个 episode 边界重置；
 - Wt：持久世界知识，记录环境如何响应动作，跨 episode 保持，仅在新证据下更新；
- 冻结 LLM 提供通用先验，Wt 存储部署期习得的环境特定知识。

2) 知识单元：可证伪假设：
- 假设形式 H := (C ∧ A ⇒ Y)：条件 C 成立时执行动作/多步流程/程序 A，应产生可观察后果 Y；
- 每条假设额外存储：受支持的范围（scope）、测试配方（recipe for testing）、证据记录（evidence record）；
- 该形式使假设直接可测：建立 C → 执行 A → 检查 Y。

3) 假设生命周期：
- 形成：由两类事件触发——预期之外的结果、决策相关的未知；假设从 agent 自身交互经验中归纳生成；
- 测试（两种证据通道）：
 - 被动证据：普通任务转换恰好检验某条假设（例如后续任务恰好在加热化合物 X 时记录 X 是否保持完整）；
 - 主动证据：智能体主动执行的受控实验（论文在“e an experiment”处讨论受控实验细节，对应 Appendix B）；
 - 不确定测试：所需条件未被见证、或结果缺失/模糊时，测试既不算支持也不算反证，假设保持 pending 状态；
- 验证与提交：仅当满足 Commit(H) ⇔ Nep(S)≥τep ∧ Nbind(S)≥τbind ∧ Ntest(S)≥τtest 时，假设以“已验证规则”身份进入 Wt；三个阈值在 benchmark 运行前设定（片段明确“thresholds are set before any benchmark run”）；
- 绑定与泛化：规则先绑定到具体实体（例如“80°C 以上会分解”先只适用于化合物 X），覆盖其他化合物需要来自它们的直接证据（片段明确“covers other compounds only after evidence from them”），从而防止过早泛化；
- 应用：只有已验证规则能在其支持范围内指导预测与规划；未验证假设不参与决策；
- 修订与退役：新反例使规则范围缩小；环境发生动态变化时陈旧规则被退役，从而“撤回权威”，避免累积知识变成负担。

4) 与 LLM 的接口：
- ALFWorld/MiniGrid/ScienceWorld 中，行为-效应规则从 typed transitions 归纳，直接用于状态变化预测、规划与否决（veto），不需要规则条件化模型调用；
- DiscoveryWorld 中，定量/分类规律拟合到测量与观测，并由干预前冻结的 held-out 预测进行验证；只有 DiscoveryWorld 的冻结预测引擎（与 Frozen WM 共享的固定预测组件）做规则条件模型调用；
- 每条规则保留“它改变了哪些决策”的追溯链接，使知识更新可问责。

5) 设计原则：
- 用外部结构化知识代替参数内隐记忆，使学习可解释、可回滚、可迁移；
- 验证先于信任，反例先于淘汰，独立测试先于提交，避免冷启动期被偶然证据误导。

Q4: 论文做了哪些实验？

1) 基准与部署场景：
- ScienceWorld（静态、先验基本一致）：文本交互式科学实验；
- ALFWorld（静态、先验基本一致）：家务操作；
- MiniGrid（隐藏动态漂移）：迷宫任务，环境中存在未经宣布的动力变化；
- DiscoveryWorld（科学发现）：其中 Reactor Lab 要求智能体从测量中发现并验证定量规律。

2) 对比方法：
- ReAct：基础推理-行动基线；
- WorldEvolver：世界模型进化基线；
- Frozen WM：作者构造的匹配对照，与 EvoBelief 使用相同的 W-blind actor 和 predictor，但不允许任何学习规则指导预测或规划；该对照用于评估完整 EvoBelief 系统相对纯先验驱动控制的增益。

3) 控制条件：
- 所有方法共享 benchmark 接口、任务顺序、随机种子与预算；每个任务只尝试一次；
- ALFWorld 与 ScienceWorld 在 actor 侧迁移记忆的有/无两种条件下评估，记忆对各方法等量提供；
- MiniGrid 与 DiscoveryWorld 上 EvoBelief 不加 actor 侧迁移记忆运行；
- 主干模型使用 Claude Haiku 4.5 与 GPT-5.4 两种。

4) 实现细节：
- ALFWorld/MiniGrid/ScienceWorld：行为-效应规则从 typed transitions 归纳，直接预测状态变化供规划与否决使用，无需规则条件化模型调用；
- DiscoveryWorld：定量与分类规律拟合到测量值/观测值，并由干预前冻结的 held-out 预测验证；只有 DiscoveryWorld 的冻结预测引擎（与 Frozen WM 共享的固定预测组件）会做规则条件模型调用；
- 假设身份与可采证据的细节在 Appendix B；阈值设置（τep、τbind、τtest）在 benchmark 前固定，各组配置见 Appendix E。

5) 主要报告指标：成功率（ALFWorld/MiniGrid）、任务完成率（DiscoveryWorld 分 Normal/Challenge 两档）、规则复用指标（134 个 unseen 任务上的行为曲线）、规则接地性（置换颜色/房间名后的成功率表，Table 10）、机制消融（Table 11）。

注意：因检索片段限制，ScienceWorld 的具体成功数字、WorldEvolver 各基准的精确表现、Appendix E 的预算细节未拿到，需查原文；片段中可见的图表文字（如“six task types”分块曲线、No mem/Mem 条件、Haiku/GPT 双主干）表明 ALFWorld 有按任务类型的长时间部署曲线。

Q5: 发现了什么实验现象？

1) 对比 Frozen WM 的增益主要来自规则知识而非行动记忆：
- ALFWorld 在无 actor 侧迁移记忆时，EvoBelief 相对 Frozen WM 提升 23.9%（Claude Haiku 4.5）与 31.3%（GPT-5.4）；
- Frozen WM 使用同一 W-blind actor 与 predictor、只是禁用规则，说明增益可归因于被验证规则的指导（预测/否决/规划），而非 agent 推理能力的差异。

2) DiscoveryWorld 上的从 0 到 1 跨越：
- Reactor Lab 中 EvoBelief 完成 Normal-tier 83.3%、Challenge-tier 58.3%，而每个被评估的基线都是 0%；
- 这说明纯先验/纯 ReAct 式智能体在该环境无法自发形成并验证定量规律；EvoBelief 的假设形成 + 主动实验 + held-out 验证链路是完成此类任务的关键（成功率来自摘要，链路机制来自方法片段）。

3) 规则接地性与名称不变性：
- 检索片段显示在“每个颜色和房间名都被置换”的条件下，EvoBelief 达到 80.8%，而 Frozen WM 只有 7.1%（另一处片段提及 1.3% 的 Frozen WM 对比值，可能对应不同置换条件，需查原文 Table 10 确认两个数值分别对应哪种置换程度）；
- 这表明习得规则锚定在观察到的效应上，而不是实体名称的表面共现；名称置换会击垮依赖表面记忆的方法，但对效应验证过的规则影响小得多。

4) 跨任务规则复用：
- 检索片段提到“134 unseen tasks (Haiku 4.5, no transition memory)”并展示按六类任务簇分块的部署期曲线，说明 EvoBelief 在未见过的任务上能套用已提交规则（合理推断曲线显示成功率随部署 episode 提升，具体趋势需看对应 Figure）。

5) 陈旧规则退役的价值：
- 片段明确指出“退役陈旧规则与学习新规则互补，因为撤回权威的能力让累积知识在环境变化时是资产而非负担”；
- MiniGrid 的未宣布动态变化实验直接检验这一点：规则先被提交、后被反例推翻，agent 能恢复性能（恢复的具体数字未在检索片段中提供）。

6) 机制消融：
- 表 11 的机制消融“显示两个方向（both sides of the…）”在检索片段处截断，可能是“假设形成”与“规则退役”两个机制都各自贡献；建议回原文核对具体消融项与数值，不要据不完整片段断言。

7) 被动证据的局限（合理推断）：
- 方法章节强调被动证据来自“恰好检验假设的任务转换”，这意味着被动证据的覆盖率依赖任务序列的运气；EvoBelief 因此保留主动实验路径作为补充。

8) 语言模型使假设变便宜：
- 片段提到“language models make hypotheses cheap to…”，即 LLM 生成候选假设的边际成本极低，这使假设库可以覆盖更广的条件空间，这是 LLM 先验与外部证伪机制协同的机制性解释。

注意：ScienceWorld 上的绝对数字、WorldEvolver 的逐项对比结果，以及任何 scaling trend / 失败案例（例如 Challenge-tier 未完成的 41.7% 是哪些任务类型）均不在检索证据内，需要回原文核对。

Q6: 有什么可以进一步探索的点？

1) 阈值与生命周期的自适应：τep、τbind、τtest 目前是运行前人工设定；可探索根据任务风险、环境变化频率、证据质量动态调整阈值，或在贝叶斯框架下把规则提交概率作为隐变量学习。
2) 更丰富的假设语言：C∧A⇒Y 的确定性规则形式适合离散因果，但真实环境常有概率性、连续量、组合爆炸的条件空间；可扩展为概率规则、不等式/函数型规律、结构因果模型，同时保持可证伪与可退役性质。
3) 主动实验策略的优化：主动证据比被动证据可靠但成本高；可设计信息增益驱动的实验选择（选择哪些假设、在什么条件下测），把实验预算当作决策问题；DiscoveryWorld 的定量规律拟合已隐含此方向。
4) 与行为层自进化的整合：EvoBelief 负责“信念进化”，反思/技能库负责“行为进化”；两者共享证据与决策追踪时可能产生协同（例如技能失败反哺假设形成，新规则催生新技能）。
5) 多智能体与群体知识库：多个部署实例可以共享/仲裁规则库；规则绑定、证据链、退役机制天然适合联邦式知识积累；需要解决跨实例证据的独立性与权威冲突。
6) 真实世界与 AI for Science 落地：从模拟基准走向实验室自动化、机器人操作、科学仪器控制；本论文在 DiscoveryWorld 上的定量规律发现已验证科学发现场景，下一步可对接真实反应数据库与主动实验平台。
7) 规则冲突与优先级：当多条已验证规则在同一个 (C, A) 上给出矛盾预测时如何处理（时间戳、证据强度、范围粒度）；论文未展开，但从退役机制可自然延伸出规则仲裁层。
8) 与参数更新的混合：论文刻意保持 LLM 冻结以隔离外部知识的贡献；实际系统中可探索“外部规则为主、参数微调为辅”，或把退役规则蒸馏回提示/记忆。
9) 安全、可问责与可解释：每条规则带证据链和决策影响链，为审计与安全评估提供接口；可研究如何用规则库检测并主动纠正 LLM 对环境的先验幻觉。
10) 理论刻画：可证伪假设库的收敛性、样本复杂度、阈值选择与泛化误差的关系目前是经验性的；把规则提交建模为多重假设检验并控制伪发现率是有价值的理论方向。

以上 2、3、5、7、10 属于基于论文机制的合理推断/延伸，1、4、6、8、9 也在论文设计边界内自然成立；均有待后续工作验证。

Q7: 总结一下论文的主要内容

一、论证主线
论文从“自进化 LLM 智能体为什么还会失败”这一问题切入。现有自进化机制（经验轨迹学习、反思、记忆、技能蒸馏）改进的是策略层 π(a|s,g)：智能体学会更好地行动；但行动还建立在“环境如何响应动作”的信念之上，而这类信念在部署前来自预训练先验，在陌生环境里可能是错的。论文用化合物 X 的合成例子说明：智能体具备加热、纯化、分析等程序能力，却因为“X 在 120°C 内稳定”这一错误先验而在 100°C 分解点失败；任务级修补（“用更低温度”）无法沉淀出“X 的热稳定性”这一可复用于运输、存储、分析的属性知识。更重要的是，错误信念会跨任务传播，使失败从单任务扩散到多任务。因此论文提出把“信念进化”作为与“行为进化”互补的自我进化轴，并把它形式化为部署期持续更新的世界模型。

二、技术主线
EvoBelief 将环境知识外部化为可证伪假设库。环境被形式化为隐状态-观测-动作-目标过程；智能体维护两个外部化状态：情境状态 zt（描述当前局面，episode 级重置）与世界知识 Wt（描述环境对动作的响应，跨 episode 持久）。世界知识的基本单元是假设 H := (C ∧ A ⇒ Y)——“条件 C 下执行 A 应产生 Y”，该形式使假设直接可测。假设生命周期由四个环节构成：
1) 形成：意外结果与决策相关未知触发新假设；
2) 测试：被动证据（普通任务转换恰好检验某假设）与主动证据（受控实验）双通道；测试若未见证条件或结果模糊则判为不确定，不算支持也不算反证；
3) 提交：仅当独立证据次数、绑定次数、测试次数同时超过预设阈值（τep、τbind、τtest）才成为已验证规则，进入 Wt 并可指导预测、规划与否决；规则先绑定具体实体，泛化到其他实体必须有它们自己的证据；
4) 修订/退役：反例缩小规则范围，环境漂移使规则陈旧时将其退役，从而“撤回权威”。
全程保持 LLM 参数冻结：预训练模型供给通用先验，Wt 沉淀环境特定知识；每条规则保留证据链与其改变的决策的记录，使学习可审计。工程实现上，ALFWorld/MiniGrid/ScienceWorld 的规则从 typed transitions 归纳并直接用于状态预测与规划，无需规则条件化模型调用；DiscoveryWorld 使用拟合定量/分类规律 + 干预前冻结的 held-out 预测验证。

三、实验主线
论文在四个基准、三个部署场景下评估：ScienceWorld 与 ALFWorld（静态、先验对齐）、MiniGrid（隐藏动态漂移）、DiscoveryWorld（科学发现）。基线含 ReAct、WorldEvolver 与作者自建的 Frozen WM（与 EvoBelief 同 actor、同 predictor，但禁用一切学习规则，用来隔离规则知识的边际贡献）。所有方法共享接口、任务顺序、种子与预算，每任务只尝试一次；主干为 Claude Haiku 4.5 与 GPT-5.4 两种。
关键结果：
- ALFWorld（无 actor 侧迁移记忆）相对 Frozen WM 成功率提升 23.9%（Haiku 4.5）与 31.3%（GPT-5.4）；
- MiniGrid 上 EvoBelief 能修订陈旧规则并在未经宣布的动态变化后恢复（恢复幅度未在检索片段中给出具体数字）；
- DiscoveryWorld Reactor Lab 完成 Normal-tier 83.3%、Challenge-tier 58.3%，各基线均为 0%；
- 名称置换测试（颜色/房间名全部置换）下 EvoBelief 达 80.8% 而 Frozen WM 仅 7.1%（另一片段提及 1.3% 的对照值，可能对应不同置换条件），说明规则接地于观察效应而非名称；
- 134 个 unseen 任务上展示跨任务规则复用（Haiku 4.5、无迁移记忆）；表 11 机制消融显示假设形成与规则退役两端都有贡献（片段截断，具体项需核对）。

四、结论与意义
论文证明了一种新的自我改进路径：把环境知识当作可测试、可修订的外部规则来进化，可以在完全不更新模型参数的情况下提升部署期表现，并使知识跨任务复用。相比参数内隐更新的世界模型，EvoBelief 的可证伪性与证据链带来可解释、可回滚、可迁移的性质；相比纯行为层自进化，它补上了“信念”这一维度。局限在于规则语言的表达力、阈值的先验设定、基准仍以模拟环境为主，以及 Challenge-tier 尚未完全解决（41.7% 未完成）。整体而言，这是“世界模型 + 科学方法 + LLM agent”交叉的一项系统性工作，对 agent 部署期适应与 AI for Science 两个方向都有参考价值。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户画像中的 agent 方向直接相关：论文提出智能体自我进化的新维度（信念进化），是 agent 部署期适应研究的系统性工作。

## 基本信息

- 作者：Janet Wang, Jiang Guo, Yi Xiang, Jiaqing Yuan, Mingwen Dong, Hideo Kobayashi, Shuaichen Chang, Chandan K. Reddy, Jiarong Jiang
- 机构：根据论文通讯作者邮箱（swang47@tulane.edu 与 gujiang@amazon.com）可推断作者单位涉及 Tulane University（图兰大学）与 Amazon（亚马逊）；其余作者的单位信息在提供的正文片段中未出现，完整机构列表请核对论文首页脚注。
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-12
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.12789`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（retrieved_evidence，102 个 chunk 的匹配结果）并按 field_evidence_map 将 Introduction/Method/Results 证据分别锚定到 background、method、results 相关字段；检索片段不完整处（如 Table 11 消融项、ScienceWorld 绝对数字、WorldEvolver 精确表现）已在对应字段标注为需回原文核对。
