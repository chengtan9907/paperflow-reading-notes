---
user_id: "cheng tan"
paper_id: 12375
arxiv_id: "2609.21626"
title: "One Prompt Does Not Fit All: Self-Meta-Evolve for Personalized Information Extraction"
institution: "推测为微软亚洲研究院（Microsoft Research Asia, MSRA）——依据是作者名单中 Dongmei Zhang、Qingwei Lin、Zhitao Hou 等长期隶属 MSRA 的研究者，以及论文题域（企业级信息抽取、交互反馈、办公场景）与 MSRA 研究方向的一致性。但元数据中 institution 字段为空，且本次未获取 PDF 正文的首页与致谢，故此判断为推测，需以原文作者单位脚注为准，不应直接引用。"
publish_date: "2026-09-21"
pdf_url: "https://arxiv.org/pdf/2609.21626"
abs_url: "https://arxiv.org/abs/2609.21626"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-22T00:10:05"
---
# One Prompt Does Not Fit All: Self-Meta-Evolve for Personalized Information Extraction

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：personalized prompt optimization · information extraction · meta-prompt evolution · persona-conditioned feedback

## 一句话总结

论文把企业信息抽取重述为“每用户提示适配（per-user prompt adaptation under interaction feedback）”问题，提出层次化双循环框架 Self-Meta-Evolve：内循环依据 persona 条件化反馈编辑每个用户专属的结构化提示，外循环蒸馏成功的编辑模式来进化 meta-prompt 本身，从而在 292 个模拟企业用户基准上把成功率推到 74.58%（较最强提示优化基线高 13.56 绝对点），并在 20 位真实从业者的双盲成对比较中取得 71% 胜率。

## 摘要

> Large language models (LLMs) are increasingly deployed for enterprise information extraction (IE), where the same document must be reorganized differently for each user. Existing prompt optimization methods, however, rely on a single prompt optimized against a global objective, which is misaligned with the inherent user heterogeneity of real workplaces. We formulate enterprise IE as per-user prompt adaptation under interaction feedback and propose Self-Meta-Evolve, a hierarchical framework that maintains a dedicated prompt for each user and continuously refines it through a dual-loop process: an inner loop that edits structured prompts based on persona-conditioned feedback, and an outer loop that evolves the meta-prompt itself by distilling successful editing patterns. To enable scalable training and evaluation, we release a persona-driven IE benchmark of 292 simulated enterprise users, paired with a reproducible persona-generation pipeline grounded in O*NET occupational taxonomies. On this benchmark, Self-Meta-Evolve achieves a 74.58% success rate, outperforming the strongest prompt-optimization baseline by 13.56 absolute points, and reaches 52.54\% within only two iterations. A double-blind human study with twenty real professionals further confirms that prompts adapted by our framework win against static baselines in 71% of pairwise comparisons.

Q1: 这篇论文试图解决什么问题？

【论文靶心问题：企业 IE 中的“一稿多需”】
论文要解决的核心问题，是同一份企业文档在不同用户之间必须被重组为不同信息结构，而现有一阶的提示优化范式无法承载这种异质性。以会议纪要、项目周报、工单、客户邮件、合同或财报为例（具体文档类型属合理推断，摘要只给出“enterprise IE”这一粒度），销售关注客户意图与下一步动作，法务关注义务与风险条款，项目经理关注阻塞项与负责人，财务关注金额与口径。同一段原文，对这些角色的“正确抽取结果”在字段选择、粒度、排序、措辞上都不相同。

【三条错配，构成论文的问题分解】
1. 目标函数错配：现有提示优化（discrete prompt search、textual gradient、模块化编译等）通常优化单一提示以最大化一个全局目标（平均准确率/F1）。全局最优对个体用户往往不是最优，用户差异被当作噪声而非可系统性利用的信号。
2. 生命周期错配：真实偏好随任务、项目阶段、组织与人员变动而漂移，静态提示一旦离线优化完成即冻结，无法在部署中持续跟进。论文因此把问题放到 interaction feedback（交互反馈）下，要求提示具备在线、持续的适配能力。
3. 监督信号错配：企业环境难以获得高质量逐字段标注，但能低成本获得交互反馈（接受/拒绝/改写/再次追问）。这类反馈稀疏、隐式、且强烈依赖用户 persona——同一句反馈在不同角色语境下含义不同。传统优化方法并未针对这种信号结构设计。

【核心张力：个性化 vs 可扩展性】
若为每个用户从零做提示搜索，成本随用户数线性增长，而单用户可获得的反馈样本极少，冷启动几乎不可行。论文的解法取向是把跨用户共享的“如何编辑提示”的知识提升到 meta 层，把个性化留在 per-user 提示层，用层次结构分担学习代价——这也是它区别于普通 per-task prompting 或 prompt ensemble 的关键。

【评测缺口】
论文暗示（并在贡献中明确声称）现有工作缺少可规模化、可复现地评测 per-user IE 的基准，因此必须自建 persona 驱动的模拟用户集与生成流水线，并以 O*NET 职业分类法作为 persona 先验来源，这同时构成其问题设定的一部分：把“用户从哪来”也纳入方法可评测性的范围。

【问题设定成立的隐含前提（需回原文核对）】
(a) 用户偏好存在稳定且可被 persona 预测的结构，而不是纯个体随机噪声；(b) 交互反馈与真实用户满意度之间有足够相关性，可作为代理目标；(c) O*NET 的职业画像能代表企业内真实用户的需求差异。任何一条不成立，模拟基准上的收益都未必迁移到真实部署——这一点摘要未做讨论。

Q2: 有哪些相关研究？

【声明：本节为基于论文标题、摘要与问题定位推断出的邻近文献地图，本次未获取到 PDF 正文的 Related Work 章节，具体被引与对照关系必须回原文核对】

1. 自动提示优化（automatic prompt optimization）：一条主线是离散提示搜索，如 APE 式的指令生成与筛选、AutoPrompt 式的模板搜索、OPRO 用 LLM 作为优化器迭代改写提示、EvoPrompt/PromptBreeder 等进化式方法；另一条主线是连续/软提示（prompt tuning、P-tuning）与文本梯度方法（如 TextGrad 把自然语言反馈当作反向传播信号）；还有模块化编译路线（如 DSPy 把提示与 pipeline 一起编译）。这些方法的共同假设正是论文所攻击的：一个用户/任务共享一条提示，目标是一个全局标量。
2. 多提示与条件化提示：prompt ensembles、mixture-of-prompts、per-task/per-domain 提示库、retrieval-based prompt selection 等工作已经承认“提示不应唯一”，但仍以任务或数据分布为条件，而非以“个体用户”为条件，也缺少在线演化机制。
3. 反馈驱动的自我改进：Self-Refine、Reflexion 以及各类 self-evolving agent 工作让模型基于自身或环境反馈迭代修正输出/策略；RLHF/DPO 一系则从偏好比较中学习。论文与之的差别在于：反馈是 per-user 且以 persona 为条件，改进的对象不是输出或模型权重，而是用户专属的结构化提示，并且改进规则本身（meta-prompt）也在被改进。
4. Meta-prompt / 元学习：meta-prompting、self-referential prompt evolution（如让提示优化器优化自己的提示）与论文外循环的“蒸馏成功编辑模式来进化 meta-prompt”直接同源；更远的理论邻居是 MAML 式元学习与多任务学习中的共享/私有参数分解，论文的“共享 meta-prompt + 私有 per-user prompt”结构可看作该思想在提示空间中的对应物。
5. 个性化与 persona 建模：persona-conditioned LLM、个性化对话（PersonaChat）、用户画像提示、推荐系统中的用户建模、以及 Generative Agents 一类的 persona 模拟。O*NET 职业分类法在职业/任务建模与用户模拟文献中常被用作先验来源，论文的 persona 生成流水线明显继承了这一传统。
6. 信息抽取与评测：传统 IE 基准（ACE、CoNLL 系）、文档级 IE、schema-guided IE，以及 LLM-based IE 的评测协议。论文的差异点是引入“用户”这一维度，把评测从“对文档抽得准不准”扩展到“对不同用户抽得合不合意”。
7. 用户异质性与个性化评测：个性化评测在推荐与对话领域已有大量工作，但在结构化信息抽取上的 per-user 基准与协议基本空白——这是论文声称的定位。

Q3: 论文如何解决这个问题？

【总体架构】Self-Meta-Evolve 是一个层次化、持续演化的提示适配框架，结构上分离两件事：每个用户“当前该用什么提示”（per-user 层），以及“应该如何编辑提示”（meta 层）。两层通过双循环耦合。

1. 问题形式化：把企业 IE 写作“在交互反馈下的逐用户提示适配”。每个用户 u 持有专属提示 p_u，系统在交互中产出抽取结果并接收反馈，目标是最大化该用户累积的适配质量（具体目标函数形式与反馈如何聚合为奖励，摘要未给出，需回原文核对）。这与“优化单一全局提示”形成直接对立。

2. 结构化提示（structured prompts）：论文明确使用“结构化提示”这一表述，意味着提示被拆解为可定位、可局部编辑的组件（合理推断为角色设定、抽取 schema/字段定义、输出格式、粒度与风格约束、示例等槽位）。结构化表示是内循环能做“编辑”而非“重写”的前提，也让外循环能从编辑轨迹中抽象出模式。

3. 内循环（inner loop）：面向单个用户，基于 persona 条件化反馈对该用户的结构化提示执行编辑。“persona-conditioned”意味着反馈不是被当作裸信号，而是结合用户的职业/画像来解读——同一句“太啰嗦”或“漏了关键项”在不同 persona 下应触发不同的编辑动作。内循环因此是每用户私有的、需要高效样本利用的学习过程。

4. 外循环（outer loop）：跨用户汇总“哪些编辑带来了改进”，蒸馏出可复用的成功编辑模式，并用其进化 meta-prompt——也就是指导内循环如何编辑的元提示。这一步是论文最关键的设计：把个体经验沉淀为跨用户可迁移的编辑知识，从而让新用户/冷启动用户不必从零开始。

5. 双循环的耦合动力学：外循环提升内循环的编辑效率与方向感，内循环为外循环提供训练信号，构成一个 test-time 也持续运行的自我改进闭环，而非一次性的离线优化。这也解释了论文强调“两轮内达到 52.54%”——框架被设计为在极少交互轮次内就产生大部分收益。

6. 评测基础设施：292 个模拟企业用户 + 基于 O*NET 职业分类法的可复现 persona 生成流水线。把 persona 生成做成“流水线”而非一次性数据集，意味着评测集在方法论上可重生成、可扩展、可控覆盖度。

7. 验证策略：先在模拟基准上与提示优化基线对比，再用 20 位真实从业者的双盲成对比较做外部效度验证——即同时回答“比现有自动化方法好”与“对真人也有用”两个问题。

Q4: 论文做了哪些实验？

【实验一：模拟用户主基准】在作者自建的 persona 驱动 IE 基准（292 个模拟企业用户）上，Self-Meta-Evolve 取得 74.58% 成功率，比“最强提示优化基线”高 13.56 绝对点。摘要未点名具体基线集合，也未给出基线各自的绝对值，因此无法判断 13.56 点是相对哪一个方法、以及领先幅度分布是否均匀（合理推断基线包含全局提示优化方法与可能的 per-user 非层次化变体，需回原文核对）。

【实验二：迭代效率】论文报告仅在两轮迭代内就达到 52.54% 成功率。这构成一条“few-shot/few-iteration 效率”曲线上的关键点：框架在极少交互轮次内已越过半数成功率。摘要未给出完整轮次-成功率曲线，也未说明该曲线是否饱和、饱和点在哪、是否需要早停。

【实验三：人类研究】20 位真实专业人士参与的双盲（double-blind）研究，采用成对比较（pairwise comparison）协议：由本框架适配出的提示 与 静态基线提示 的对决中，前者在 71% 的比较中胜出。双盲意味着评估者不知道哪一侧来自本框架；“prompts adapted by our framework”暗示呈现给人类的是提示/提示产出的抽取结果（具体呈现形态、评估维度、一致性指标——如评分者间一致性 κ——摘要均未给出）。

【实验四（推断存在但摘要未提）】典型的消融应包含：去掉外循环（仅内循环 per-user 编辑）、去掉 persona 条件化（用无 persona 的反馈编辑）、不同用户数/scaling 行为、meta-prompt 的迁移性（把在 A 组用户上进化出的 meta-prompt 用于新用户组）。此外，成本维度（token 消耗、每用户编辑轮次、延迟）在摘要中完全缺失。以上均为合理推断，必须以原文为准。

Q5: 发现了什么实验现象？

【可确证的观察】
1. 全局最优 ≠ 个体最优，且差距可观：13.56 绝对点的领先说明用户异质性不是评测噪声，而是可被系统性利用的结构化信号。这是论文最有力的经验论断。
2. 早期收益陡峭：两轮即达 52.54%（相对 74.58% 的终值约七成），说明大部分收益来自“粗粒度适配”——即在 persona 层面就能捕获的需求差异；后续轮次可能对应更细粒度的个体偏好精修。这是合理推断，原文的完整曲线可验证。
3. 模拟与真人之间存在的落差：模拟基准上相对基线的优势（13.56 绝对点）与真人双盲的胜率（71%）不在同一量纲上，但 71% 明显未饱和——仍有约 29% 的成对比较未能取胜。这提示 persona 模拟与真实职业人偏好之间存在 sim-to-real gap，且个性化收益并非对所有人都成立，可能在部分 persona/文档类型上无增益甚至负增益。这一“谁没赢”的分布是原文最值得挖的负结果来源。

【需回原文核对的趋势与张力（摘要未提供）】
- 成功率定义：是字段级匹配达到阈值、整体 schema 一致，还是由 LLM judge 打分？定义不同会显著改变 74.58% 的含义与可比性。
- meta-prompt 蒸馏是否会发生模式坍塌：外循环倾向高频 persona 的成功模式，可能系统性牺牲长尾 persona，表现为“平均提升、方差变大”或少数群体退化。
- 提示编辑是否震荡：内循环在多轮之间是否出现反复改写同一槽位、反馈相冲突导致的不收敛现象。
- 反馈稀疏度鲁棒性：当反馈量降到极少（冷启动用户）时性能如何衰减，是这类框架的典型失败模式。
- 成本-收益比：两轮 vs 更多轮的边际收益与 token/延迟开销的比值未被报告。
以上均属“未观测到”的评测维度缺口，而非已确认的负面结果，不可当作结论使用。

Q6: 有什么可以进一步探索的点？

1. 从模拟 persona 走向真实部署：把 71% 的离线双盲胜率延伸到在线 A/B 与长期部署，检验偏好漂移下的持续适配能力；量化并缩小 sim-to-real gap（例如以真人对模拟反馈分布做校准）。
2. 反馈信号的扩展与信用分配：把显式纠正之外的隐式信号（接受/忽略、二次追问、停留与修改幅度）纳入反馈建模；研究多信号融合与时序信用分配——长程结果归因到哪一次编辑仍是开放问题。
3. meta-prompt 进化的稳定性与公平性：防止蒸馏过程中的模式坍塌与对高频 persona 的过拟合；引入长尾保护、去偏约束或分层蒸馏；让 meta 层知识可解释、可审计。
4. 隐私与隔离：企业场景中 per-user 数据与跨用户共享的 meta 层之间需要明确的隐私边界，联邦式元学习、差分隐私蒸馏、以及“哪一级知识可以跨用户流动”的组织治理问题都值得研究。
5. 成本工程：把每用户提示的维护成本压到可部署水平——提示槽位复用、编辑操作缓存、按需触发外循环、失败检测后的惰性更新。
6. 评测科学：设计能主动暴露 persona 过拟合的基准；提供跨组织、跨语言、跨行业的迁移评测；给出 persona 覆盖度与真实用户分布的匹配度指标。
7. 与 agent 系统耦合：把 per-user 提示视作 agent 的长期记忆/策略层，与工具调用、多轮任务规划、检索策略共同演化——即从“个性化提示”扩展到“个性化 agent 行为策略”。
8. 理论刻画：把逐用户提示适配形式化为共享-私有多任务/元学习问题，分析其样本复杂度、泛化界以及用户数增长时的 scaling 行为，为“何时个性化优于全局”给出可检验的条件。

Q7: 总结一下论文的主要内容

【一句话定位】这篇论文主张企业信息抽取的正确抽象不是“为任务优化一条提示”，而是“在交互反馈下为每个用户维护并进化一条提示”，并给出一个双层循环框架、一套 persona 驱动基准与一组含真人双盲对比的实证。

【问题主线】LLM 正在被广泛用于企业级信息抽取，而企业场景的独特性在于：同一份文档对不同的使用者必须被组织成不同的信息结构——角色不同，值得抽的字段、粒度、次序、措辞都不同。现有提示优化方法却遵循“单一提示 + 全局目标”的范式，其隐含假设是用户需求同质。论文指出这造成系统性错配：全局最优提示对个体用户并非最优；静态提示无法跟上真实偏好的漂移；企业环境里可得的是稀疏、隐式、且强烈依赖用户身份的交互反馈，而现有方法并非为此设计。若反过来为每个用户单独搜索提示，成本随用户数线性增长且单用户样本极少——个性化与可扩展性构成核心张力。据此论文把任务形式化为“交互反馈下的逐用户提示适配（per-user prompt adaptation）”。

【技术主线】Self-Meta-Evolve 用层次结构化解上述张力。每用户持有自己的结构化提示（可分解、可局部编辑，而非整段重写）。内循环面向单用户，依据 persona 条件化反馈编辑其结构化提示——反馈不是裸信号，而要结合用户角色画像来解释，因为同一句反馈在不同 persona 下应当触发不同的修改。外循环跨用户汇聚编辑轨迹，蒸馏出“哪些编辑有效”的成功模式，用来进化 meta-prompt 本身，即指导内循环如何编辑的元提示。这样，个体经验被沉淀为可迁移的跨用户编辑知识，新用户与冷启动用户不必从零开始；两层互相供给信号，使系统在推理时也持续自我改进，而不是一次性的离线优化。

【实验主线】为让训练与评测可规模化、可复现，论文发布了一个 persona 驱动的 IE 基准，包含 292 个模拟企业用户，并配套一条基于 O*NET 职业分类体系的可复现 persona 生成流水线——把“用户从哪来”也纳入方法论范围。在该基准上，框架达到 74.58% 成功率，比最强提示优化基线高 13.56 绝对点，并且仅两轮迭代就达到 52.54%。为检验外部效度，论文组织了一项 20 位真实专业人士参与的双盲人类研究，用成对比较判定，框架适配的提示在 71% 的对局中击败静态基线。

【论证结构的强度与薄弱点】论证链条完整：动机（用户异质性）→ 形式化（per-user adaptation under feedback）→ 方法（双循环 + 结构化提示）→ 基础设施（基准 + persona 流水线）→ 模拟验证 + 真人验证。最有力的部分是“全局最优 ≠ 个体最优”的可测化：13.56 绝对点把异质性从直觉变成了可复现的经验事实。最薄弱、也最需要回原文核对的部分是评测口径与边界：success rate 的具体定义、基线集合与最强基线的身份、消融证据是否支撑“外循环确实在贡献”、以及 74.58% 与 71% 之间是否存在用模拟用户自评自家框架的循环性风险。此外摘要未报告任何成本维度（token、轮次、延迟），也未讨论 per-user 数据与跨用户 meta 知识流动带来的隐私问题，这在企业场景中并非次要问题。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你的 agent 方向（权重 0.10）直接相关：论文的“私有 per-user 提示 + 共享 meta-prompt 自我进化”结构，本质是 agent 长期记忆/策略自适应的一种实现模板，可迁移到工具调用策略、检索策略或多轮任务规划的个人化。

## 基本信息

- 作者：Hongliang Li, Lu Wang, Yong Xu, Hanyang Chen, Zhitao Hou, Xiaoting Qin, Song Ge, Qingwei Lin, Dongmei Zhang
- 机构：推测为微软亚洲研究院（Microsoft Research Asia, MSRA）——依据是作者名单中 Dongmei Zhang、Qingwei Lin、Zhitao Hou 等长期隶属 MSRA 的研究者，以及论文题域（企业级信息抽取、交互反馈、办公场景）与 MSRA 研究方向的一致性。但元数据中 institution 字段为空，且本次未获取 PDF 正文的首页与致谢，故此判断为推测，需以原文作者单位脚注为准，不应直接引用。
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-09-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.21626`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未获取到任何 PDF 检索证据（retrieved_evidence 与 field_evidence_map 均为空），全文依据论文标题、作者、摘要与元数据撰写；heuristic_draft 中的字段基本是摘要片段的截断与错位（例如把“双盲人类研究 71% 胜率”这一实验结果误列为 main_contributions，第三项贡献更是被截断的半句），本报告已按正确的证据层级重写，并对所有摘要未覆盖的方法细节、消融、评测口径与成本维度明确标注为推断或缺口，需回原文核对。
