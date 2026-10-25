---
user_id: "cheng tan"
paper_id: 17417
arxiv_id: "2610.29784v2"
title: "Evolving with Open Worlds: Towards Recursive Self-Improvement for Future Forecasting"
institution: "中国人民大学（合理推断，基于 Ji-Rong Wen、Wayne Xin Zhao 等作者的常见所属机构；论文提供的元数据未给出完整机构列表，其他作者单位需回原文核对）"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.29784v2.pdf"
pdf_url: "https://arxiv.org/pdf/2610.29784v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-25T14:53:02"
---
# Evolving with Open Worlds: Towards Recursive Self-Improvement for Future Forecasting

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：recursive self-improvement · event forecasting · agentic forecasting · reinforcement learning with verifiable rewards

## 一句话总结

论文提出 EvoForesight：以真实世界事件预测作为持续更新、结果可验证的“活的”环境，用 PROMETHEUS 合成任务、BRANCHUS 搜索—预测智能体和 ATHENA 递归进化引擎，把每次预测轨迹转化为参数更新、可复用技能与更难任务，从而在 FutureX 周更榜单连续三周排名第一，并在四个基座模型的合成评测中进入前三。

## 摘要

> Most contemporary artificial intelligence (AI) systems are released with frozen parameters and improve through external updates, whereas an ideal system would keep improving from its own experience, a capacity we call recursive self-improvement (RSI). Yet studying RSI requires an environment that is continuously changing, objectively verifiable, and naturally grounded, posing a fundamental challenge for agent evolution. Here we introduce EvoForesight, a sustainable evolution agent system grounded in future event prediction, in which agents continuously interact with a dynamically evolving environment and improve by internalizing and externalizing the experience gained through these interactions. A living environment synthesizes verifiable tasks, and a recursive-evolution engine turns the reasoning trajectory behind each prediction back into capability: decoupling search from prediction adapts parameter-level evolution to the contamination-prone live environment, while distilling reusable skills lets the agent evolve from the limited live trajectories such an environment affords. On the external FutureX leaderboard, which evaluates predictions against real-world outcomes and is updated weekly, our self-improved forecaster ranked first across all systems over three consecutive weeks, achieving up to a 195% improvement in rating. On our synthesized evaluation, it ranked among the top three across four base LLMs, outperforming the strongest baseline by up to 10.7 Bradley--Terry points. More broadly, these results suggest that a system built for self-improvement is best understood as an agent that co-evolves with a genuinely living, open-ended environment, pointing to a shift in AI from training static models on fixed data toward cultivating agents that grow stronger from their own experience, with such an open world as the indispensable basis of that growth.

Q1: 这篇论文试图解决什么问题？

一、论文试图解决的总问题
论文真正要解决的不是“把事件预测准确率再提高一点”，而是一个更基础的问题：一个 AI 系统能否依靠自身与环境交互产生的经验，递归地变得更强。论文把 RSI 描述为能力随时间累积、错误被纠正、收益被复利的过程，并把评价标准从“某一时刻会什么”转向“能力如何形成和增长”。这一目标对方法要求高，对研究环境的要求更高：环境必须持续出题、自动判分、且不能与训练数据混在一起。

二、为什么固定数据集不够
1. 固定数据集是惰性的：题目不更新，挑战不会随 agent 变强而变难，容易饱和，无法支撑多轮自我改进。
2. 固定数据集不能自动判断进步：它只提供静态答案键，不能像真实世界那样在事件发生后给出时间上自然的 reward。
3. 真实开放世界是非平稳的：新闻、事件、工具和知识都在变化，静态训练分布与部署分布持续偏离。
4. 自我改进需要客观 reward：若由模型自评，容易 reward hacking；若靠人评，成本高且不可持续。
5. 动态环境有污染风险：未来事件一旦发生，结果会进入网络文本；若训练或检索混入结果，reward 会失真。
6. 实时轨迹稀缺：每周真正可验证的 live 样本有限，参数级更新样本效率低，难以直接做大规模 RL。

三、为什么选择未来事件预测
论文把未来事件预测视为天然 testbed：任务面向未来，结果在事件发生后可客观核验，与真实新闻和世界状态自然 grounding，且可以每周再生、按难度分层（FutureX 的 L1–L4）。这使“环境出题—agent 预测—世界揭晓真值—reward—进化”的闭环可行。预测还要求 agent 在不确定性下收集证据、权衡假设、输出概率，因此一条轨迹同时包含搜索行为、推理过程和校准信息，可被多轴复用。

四、核心技术矛盾
1. 探索与预测耦合：搜索策略若与预测策略一起更新，容易把噪声、缓存或泄漏吸收进参数；论文因此强调 search 与 predict 解耦。
2. 稀疏延迟奖励：结果要等事件发生，credit assignment 难，奖励方差大。
3. 技能与参数如何互补：纯 prompt/技能快但上限有限；纯参数深但样本效率低、易遗忘。
4. 环境自进化可能作弊：任务合成器若只追求难，可能生成不可验证、歧义或已经泄漏的题。
5. 评价分布漂移：周更榜单的题目分布、难度和新闻周期不断变化，历史提升不一定可复现。

五、隐含假设与边界条件
论文的路线隐含假设：世界事件有足够可预测结构；结果可被及时、无歧义地判定；任务合成质量可控；技能可跨题迁移；基座模型已有足够先验；搜索工具能覆盖关键证据；预测不会反过来影响事件本身（self-fulfilling prophecy）。这些条件在金融、政治、公共卫生等高风险领域尤其需要单独审计。

六、对 heuristic_draft 的校准
现有检索片段主要来自 Method/Results，不能单独支撑完整动机；上述动机主要依据摘要与 Introduction 的明确表述，技术矛盾部分含合理推断，具体实验边界需回原文核对。

Q2: 有哪些相关研究？

一、递归自我改进与自举学习
与本文相邻的研究包括 self-training、self-critique、STaR 类 rationale bootstrapping、RLAIF/RLHF、constitutional AI、self-play、open-ended learning 与 continual learning。它们共同关心“模型能否用自己产生的数据改进自己”。论文的差异在于：把 RSI 放到非平稳、可验证、自然 grounding 的真实预测环境中，而不是在固定 benchmark 上反复采样；同时更新参数、技能和环境，形成三轴闭环。

二、LLM 事件预测与 agentic forecasting
直接相关的是 FutureX 这类周更事件预测 benchmark；其他路线包括多 agent debate、检索增强预测、superforecaster 风格工作流、prediction market 聚合、概率校准与 Brier/log loss 评估。论文的差异是：不是一次性预测 pipeline，而是把预测轨迹作为训练材料，并让环境根据 agent 的失败变难。

三、RLVR 与可验证奖励
RLVR 已在数学、代码等可自动判分任务上流行；预测任务的特点是 reward 延迟、概率化、由外部世界判定。论文把事件 resolved outcome 作为 reward，并用 search/predict 解耦控制污染。具体 RL 算法、reward shaping、方差控制与更新频率未在提供片段中给出，需回原文核对。

四、技能库、记忆与工具使用
与 Voyager 式 skill library、procedural memory、tool-augmented agents、hierarchical planning 相邻。论文的技能是预测程序性知识，分任务特定手工种子技能与从轨迹蒸馏的可复用技能，按角色检索，并从抽象原则逐层 specialization 到具体来源。差异在于：技能蒸馏与参数 RL、环境进化处于同一闭环，而不是独立模块。

五、动态 benchmark 与污染控制
相关主题包括 live benchmark、temporal split、contamination audit、weekly refresh。论文用“未来事件真值尚未揭晓”来降低泄漏，但仍依赖搜索工具与模型先验；reward 动态只能作为间接证据，不能替代严格的数据 lineage 审计。

六、开放世界、课程学习与环境生成
相关路线包括 self-play、curriculum learning、adversarial task generation、environment synthesis。论文的 PROMETHEUS 用失败分析生成更难任务，属于 agent-environment co-evolution；其风险是任务合成器与预测器形成“军备竞赛”，难度上升但可验证性下降。

七、研究空白
多数工作只改 agent 或只改环境；较少工作把参数、技能、任务难度三条反馈轴闭合，并在真实周更榜单上做纵向验证。论文声称填补这一空白，但完整 Related Work 引用列表未在提供文本中给出，以上为按主题归纳，具体引用需回原文核对。

Q3: 论文如何解决这个问题？

一、总体架构：闭环而非流水线
EvoForesight 由三个模块构成一个闭环：PROMETHEUS 出题并在事件发生后揭晓真值；BRANCHUS 产出概率预测与完整推理轨迹；ATHENA 把轨迹转化为能力；更新后的 BRANCHUS 再面对更难的 PROMETHEUS。论文强调这不是从输入到预测的 feed-forward pipeline，而是 agent 与环境互相改进的持续过程。

二、PROMETHEUS：活的出题环境
1. 输入：每日新闻与真实世界事件，形成非平稳、可验证的世界。
2. 任务来源：FutureX 外部周更 benchmark，加上 agent 驱动的合成题；前者提供外部公信力，后者提供针对弱点的训练信号。
3. 生成流程：模板与 agent discovery 结合，随后做时间验证（确保是未来事件）、去重、冲突检测和多样性选择。
4. 真值：只有事件发生后由世界揭晓，不是固定答案键；这使 reward 具有时间上的自然性。
5. 难度：FutureX 任务分 L1–L4，可支撑分层评估与课程式训练。

三、BRANCHUS：搜索—预测分离的 agentic forecaster
1. 策略形式：πθ(·|q,S) 输出概率分布 p-hat 与轨迹 τ；τ 记录收集到的证据、考虑过的假设和中间决策。
2. 双团队：search team πs 负责收集与审查证据；predict team πp 负责汇总推理并给出概率。分工依据是“工作是否依赖时间”：搜索需要及时性，预测需要在证据基础上做稳定判断。
3. 技能库：任务特定技能由人工编写作为种子；可复用技能从预测轨迹中蒸馏；技能按抽象到具体分层（s1 一般原则到 s4 具体程序或来源），并按 agent 角色检索。
4. 输出可审计：不仅给答案，还给证据链和推理轨迹，使后续参数更新、技能蒸馏和失败分析都有材料。

四、ATHENA：三轴递归进化引擎
1. 参数轴：用 resolved outcome 做 RLVR，更新 BRANCHUS 的参数。论文强调把 search 与 predict 解耦，使参数级进化适应容易被污染、且样本有限的 live 环境；具体更新哪些参数、如何分配 credit 需回原文核对。
2. 技能轴：把成功与失败轨迹蒸馏为可复用技能，写入 repository；下一轮按角色检索复用。技能提供快速、可检查、可解释的改进通道。
3. 环境轴：分析失败模式，生成 weakness-targeted 的 task-synthesis 技能，让 PROMETHEUS 出更难的题；形成对抗式 curriculum，使环境与 agent 同步进化。

五、为什么这样设计
- 解决 live 轨迹少：技能蒸馏从少量轨迹提取可复用经验，放大每条轨迹的价值。
- 解决污染：未来真值、时间验证、search/predict 解耦共同降低泄漏进入参数的风险。
- 解决深浅互补：技能带来快速可检查的增益，参数更新带来更深层的能力改变。
- 解决环境停滞：失败驱动任务合成，使挑战随能力上升，而不是在固定题集上饱和。

六、代价与风险
该方案需要持续新闻、工具与验证基础设施；多轮 RL 计算成本高；技能库会膨胀、冲突或过时；任务合成可能产生不可验证题；search/predict 解耦可能牺牲联合优化的上限。这些是论文工程化与推广时需要重点检查的地方。

Q4: 论文做了哪些实验？

一、实验目标
实验不只是测静态预测准确率，而是分别检验：整体预测强度、参数进化、技能进化、环境进化、污染控制与跨模型泛化。论文在 Results 开头明确说，先建立系统整体强度，再逐一考察三条轴。

二、评测环境
1. FutureX：外部周更 benchmark，要求模型预测未来一周的事件，任务分四个难度 L1–L4；预测与真实世界结果对照，排行榜每周更新。
2. 合成评测：PROMETHEUS 合成任务，在四个基座 LLM 上比较，使用 Bradley–Terry 点衡量相对强度。

三、baseline
论文称覆盖不同模型家族与领域的 diverse agent baselines，并以最强 baseline 作为参照。具体 baseline 名单、模型规模、工具权限、检索预算、是否共享相同工具未在提供片段中给出，需回原文核对。

四、指标
- FutureX rating：排行榜评分，论文称最高提升 195%，但未在摘要中说明参照点与统计口径。
- Bradley–Terry 点：合成评测中的相对偏好强度，最高领先最强 baseline 10.7 点。
- calibration：Discussion 明确提到校准有清晰增益。
- 可能还报告 Brier、log loss、accuracy 等预测指标，但提供证据不完整，不能编造具体数值。

五、实验轴
1. 整体系统 vs 多种 agent baselines。
2. 参数进化：RLVR 前后对比。
3. 技能进化：每轮把 live 轨迹加入 pool，观察技能蒸馏带来的逐步提升。
4. 环境进化：失败驱动任务合成后，下一轮是否带来更大增益。
5. 消融：参数-only、技能-only、环境-only、组合；论文 Discussion 暗示组合优于任一单轴，但具体消融数值未在证据中。
6. 污染检查：若答案泄漏 reward 会崩塌；不同指标响应速度不同。
7. 跨基座：四个 LLM 上重复评测。

六、报告的主要结果
FutureX 连续三周在所有系统中排名第一，rating 最高提升 195%；合成评测在四个基座 LLM 上均进入前三，相对最强 baseline 最高提升 10.7 Bradley–Terry 点；校准有明确增益；技能进化呈逐轮累积。

七、证据缺口
缺少置信区间、随机种子方差、每模型明细、baseline 公平性审计、算力/时延/成本、长期漂移、人工预测者或预测市场对比。三周榜单成绩也可能受新闻周期与题目难度分布影响。

Q5: 发现了什么实验现象？

一、自我改进呈逐轮累积，而非一次性跳变
论文对技能进化的描述最清楚：每轮把新的 live 轨迹加入 pool，蒸馏出的技能使预测质量逐步上升，而不是一次 prompt 改动带来突变。这说明收益来自“预测—轨迹—蒸馏—复用”的数据飞轮，技能库在时间上产生了复利效应。

二、参数与技能两条通道互补
论文主张参数级 RL 带来深层改进，技能蒸馏带来快速、可检查的改进；两者结合超过任一单独轴（具体数值未在提供证据中）。这一现象揭示：RSI 不一定要把所有经验压进参数，外显技能库可以承担部分持续学习功能，并让改进更可审计。

三、环境变难与 agent 变强同时发生
ATHENA 用失败分析生成 weakness-targeted 题，使 PROMETHEUS 下一轮更难；论文称这带来更大增益。反直觉点在于：更难环境没有压垮 agent，反而提供了更有效的训练信号，形成 agent-environment co-evolution。

四、排行榜成绩与校准同时改善
FutureX 三周第一、rating 最高 195%；Discussion 还称 calibration 有清晰增益。这说明系统不是只追排行榜名次的过拟合，概率质量也有改善。但不同指标响应速度不同，ranking 与 calibration 可能出现阶段性错位，单一指标会误导。

五、污染控制的间接证据
论文论证：若答案泄漏，reward 早应崩塌；实际 reward 能持续变尖锐，说明训练信号可能未被污染。这是合理但非决定性的证据；模型预训练数据、搜索缓存、新闻更新和工具返回仍可能造成部分泄漏，需要更严格的数据 lineage 审计。

六、跨基座泛化
四个基座 LLM 上均进入前三，说明方法不依赖单一模型；但“前三”不是全部第一，且基座规模、家族、工具权限未给出，不能推断普遍增益。

七、指标间张力
片段提到“两类指标响应速度不同”：某些指标先升，某些后升；可能存在 rank 提升但校准滞后，或校准提升但 BT 点不明显。论文未在提供片段中给出完整 per-metric 曲线与显著性检验。

八、失败模式与负结果
- live 轨迹有限，技能池增长可能饱和；
- 任务合成可能产生歧义题，噪声 reward 会污染参数；
- 周更榜单样本小，三周成绩受新闻周期影响；
- 搜索工具成本/延迟可能限制规模；
- 长期 RL 可能遗忘或漂移；
- 预测可能自我实现或影响事件。
这些在提供证据中未系统量化，属于合理推断与需核对项。

Q6: 有什么可以进一步探索的点？

一、把 RSI 从三周扩展到长周期
需要数月甚至数年的连续运行，观察能力曲线、遗忘、漂移与饱和；建立 retention、transfer、experience efficiency 等指标，区分“短期榜单提升”与“长期可累积能力”。

二、理论条件与不可能性边界
什么环境支持 RSI？可验证性、非平稳性、任务难度可调、反馈延迟、噪声水平、探索覆盖如何共同决定样本复杂度？需要形式化“经验→能力”的转化率，并给出失败条件。

三、verifier 与 reward 设计
延迟结果、模糊事件、多结果分布、概率评分（Brier/log loss）、抗 reward hacking；多验证者、因果审计、外部 ground truth；对高风险领域引入 human-in-the-loop。

四、任务合成与课程学习
难度校准、可验证性过滤、冲突检测、多样性；避免 adversarial collapse；用 agent 失败模式生成可学习课程；自动发现新任务类型，而不是只把旧题变难。

五、技能库治理
技能版本化、冲突消解、遗忘机制、组合与跨领域迁移；技能与参数的路由；技能蒸馏保真度；可解释性与安全审查。技能库过大时如何检索、合并、淘汰，是长期运行的关键工程问题。

六、search/predict 解耦的理论与替代
联合优化与解耦的 tradeoff；credit assignment；工具策略学习；检索预算与延迟；多 agent 协作；是否可以让搜索策略也安全地学习，而非永久冻结或隔离。

七、跨领域迁移
AI for Science：实验设计、假设生成、文献预测；金融、政策、医疗预测；软件工程；任何 grounded、verifiable、non-stationary 的领域。需要领域特定 verifier 与安全约束，不能直接照搬排行榜目标。

八、多 agent 与开放世界
agent-agent co-evolution、self-play、环境生成器与预测器的军备竞赛；防止共谋、欺骗、reward hacking；研究开放世界的任务生态与激励机制。

九、评价范式
动态 leaderboard、时间外推、污染审计、预注册、人工专家/预测市场基线、决策效用而非仅 accuracy；报告算力与成本；建立可复现的 RSI 基准。

十、部署与治理
预测的市场影响、自我实现、隐私、法律责任；高风险领域的 human-in-the-loop；对“系统自己变强”带来的失控风险做持续监测。

Q7: 总结一下论文的主要内容

一、核心主张：从“此刻会什么”转向“能否从经验中变强”
论文开篇提出，智能通常被衡量为某一时刻的能力，但更重要的问题可能是系统能否通过自身经验变得更有能力。自然科学与社会科学中最强大的系统——生物演化、科学方法、人类专长——都依赖累积自我改进：纠正过去错误，复利过去收益。作者由此主张，理想 AI 系统应按能力如何形成和增长来评价，而 RSI 是自主 agent 与科学发现工具的前提。

二、问题：RSI 的瓶颈是环境
研究 RSI 不只是设计算法，还要有合适环境。agent 只有持续面对新挑战，且环境能无人工干预地判断是否进步，才能递归改进。固定数据集是惰性的：不更新挑战、不自动判分、容易饱和，无法支撑多轮自我改进。真实世界开放、非平稳、带噪声，但难以控制。未来事件预测因此成为天然 testbed：面向未来、结果事后可客观核验、与新闻自然 grounding、可每周再生、可按难度分层。

三、系统：三模块闭环
EvoForesight 由 PROMETHEUS、BRANCHUS、ATHENA 构成。PROMETHEUS 是 living data environment，持续吸收真实新闻，结合 FutureX 周更 benchmark 与 agent 驱动合成，生成可验证预测任务；任务经过时间验证、去重、冲突检测和多样性选择，真值只在事件发生后揭晓。BRANCHUS 是 agentic forecaster，策略 πθ(·|q,S) 输出概率预测 p-hat 与推理轨迹 τ；它把策略分解为 search team 与 predict team，前者收集和审查证据，后者汇总推理并给概率；技能库包含人工种子技能与从轨迹蒸馏的可复用技能，按角色和抽象层级检索。ATHENA 是 recursive-evolution engine，沿三条轴把轨迹转为能力：用 resolved outcome 做 RLVR 参数更新；把轨迹蒸馏为技能；分析失败并生成 weakness-targeted 的 task-synthesis 技能，使 PROMETHEUS 出更难的题。

四、闭环如何运转
每一轮：PROMETHEUS 出题；BRANCHUS 搜索证据、形成假设、输出概率与轨迹；事件发生后世界揭晓真值；ATHENA 用结果更新参数、蒸馏技能、分析失败并改造任务生成器；更强的 BRANCHUS 下一轮面对更难的 PROMETHEUS。论文强调 search 与 predict 解耦，以适应容易被污染、且 live 轨迹有限的动态环境；技能蒸馏则从少量轨迹中提取可复用经验，弥补参数更新样本效率低的问题。

五、实验主线
论文先在 FutureX 外部周更榜单上评估整体系统，再分别考察参数进化、技能进化与环境进化。FutureX 任务预测未来一周事件，分 L1–L4 难度，结果与真实世界对照；合成评测在四个基座 LLM 上使用 Bradley–Terry 点比较。论文报告：FutureX 连续三周在所有系统中排名第一，rating 最高提升 195%；合成评测在四个基座 LLM 上均进入前三，相对最强 baseline 最高提升 10.7 Bradley–Terry 点；校准也有清晰增益；技能进化呈逐轮累积，而非一次性跳变；失败驱动任务合成带来更大后续增益；reward 动态被用作未发生严重泄漏的间接证据。

六、解释与贡献
论文把这些结果解释为：自我改进系统应被理解为与真正开放、活的环境共同进化的 agent，而不是在固定数据上训练的静态模型。其主要贡献包括：把 RSI 落到真实、动态、可验证的预测环境；构建三模块闭环；提出 search/predict 解耦与技能蒸馏来应对污染与样本稀缺；在周更外部榜单与多基座合成评测上给出实证；提出可迁移到任何 grounded、verifiable、non-stationary 领域的方法论。

七、边界与阅读判断
论文的“开放世界”仍由 benchmark、合成器和验证器中介，不是完全无约束的真实世界；三周榜单只能证明短期有效性，不能证明长期 RSI；195% 与 10.7 点均为最高值，缺少平均值、方差与 per-model 明细；baseline 公平性、算力成本、长期遗忘、任务合成质量、污染审计均需回原文核对。尽管如此，论文把“环境设计”提升为 RSI 的核心变量，并给出参数、技能、环境三轴联动的完整工程路线，对 agent 自我进化、动态 benchmark 和 AI for Science 都有较高参考价值。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与 agent 方向直接相关：论文给出搜索团队、预测团队、工具调用、技能库、失败驱动课程和参数更新的完整 agent 自进化闭环，可作为设计自我改进 agent 的架构参考。

## 基本信息

- 作者：Zhipeng Chen, Yunsen Liang, Yanxi Chen, Hongkang Zhao, Lingyun Feng, Ruiyang Ren, Guoxin Chen, Jia Deng, Feng Chang, Chuan Hao, Ran Tao, Jian Yang, Ji-Rong Wen, Bryan Dai, Wayne Xin Zhao
- 机构：中国人民大学（合理推断，基于 Ji-Rong Wen、Wayne Xin Zhao 等作者的常见所属机构；论文提供的元数据未给出完整机构列表，其他作者单位需回原文核对）
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-21
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.29784v2`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先使用了 PDF 语义检索命中的 Method、Results、Discussion 证据，并用摘要与 Introduction 补足动机；片段未覆盖的 baseline、指标定义和消融数值均明确标注为需回原文核对。
