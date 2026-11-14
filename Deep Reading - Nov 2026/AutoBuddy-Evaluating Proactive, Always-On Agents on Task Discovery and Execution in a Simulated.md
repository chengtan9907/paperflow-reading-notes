---
user_id: "cheng tan"
paper_id: 21185
arxiv_id: "2611.12393v1"
title: "AutoBuddy: Evaluating Proactive, Always-On Agents on Task Discovery and Execution in a Simulated Workplace"
institution: "Microsoft（合理推断，待核实：作者名单中的 Chetan Bansal、Saravan Rajmohan、Victor Rühle、Fangkai Yang、Wei-Ning Chen、Dongge Han 等常见于 Microsoft 研究体系的论文署名；本次提供的元数据 institution 为 null，PDF 正文未取到作者单位段落，引用前请回原文首页核对）"
publish_date: "2026-11-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.12393v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.12393v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-11T02:18:13"
---
# AutoBuddy: Evaluating Proactive, Always-On Agents on Task Discovery and Execution in a Simulated Workplace

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：proactive agents · always-on assistant · task discovery · agent benchmark

## 一句话总结

论文提出 AutoBuddy / AutoBuddy-Bench：把一家模拟企业的员工、项目、业务系统与事件历史建成 temporal graph，再渲染成互相一致的邮件、聊天、日历、文档与业务记录，由“产生义务的事件”自动派生带标准答案的任务，要求 agent 先自己判断用户有哪些没说出口的待办、再到义务所在的系统里写操作完成，并在 120 个任务、17 个 backbone（三次重复取均值）上测得端到端分数大致落在 0.12–0.70，且给出“任务发现”与“任务执行”是两种可分离能力这一核心结论。

## 摘要

> （去除 PDF 页眉、图 1 轴标签与断行噪声后的摘要复述）
> 1. 背景：随着 LLM agent 自主性提高，它们被期待作为 always-on assistant 常驻在用户身边，主动识别需要做的工作并直接做完，而不是等指令。
> 2. 问题：现有 benchmark 通常把任务直接交给 agent；但在知识工作里，请求藏在用户的 workspace 状态中，agent 必须先识别存在哪些义务（obligations）。论文把这一设定称为“主动式任务发现与执行（proactive task discovery and execution）”：从 workspace 状态推断用户待办，并在工作真正发生的系统里完成。
> 3. 两个挑战：对 agent 而言，任务埋在体量巨大的 mail、chat、documents、records 里；对评测而言，需要一个“人、事件、文档互相一致”的模拟组织，任务才能自然涌现且正确答案可知。
> 4. 方法：提出 AutoBuddy-Bench 模拟器——把组织的 employees、projects、business systems、event history 建模为 temporal graph，渲染为 grounded、realistic 的 workspace artifacts，从“产生义务的事件”派生 ground-truth 任务，并用 world state 计算每个任务的结果。
> 5. 实验：在同一套 120 个任务上跑 17 个 backbone，三次运行取均值；只变 backbone，发现 discovery 与 execution 是两种不同的能力。

Q1: 这篇论文试图解决什么问题？

一、论文把问题重新定义在哪一层
1. 它翻转了 agent 的默认交互范式：从“请求—响应”变成“常驻—主动”。摘要原话是现有 benchmark 通常把任务给 agent，而知识工作中的请求“sit in the user's workspace”，agent 必须先识别“which obligations exist”。所以被形式化的对象不是任务求解，而是“发现 + 执行”的联合问题。
2. 被评测的能力因此有两段：(a) 从邮件、聊天、日历、文档、业务记录的状态里推断用户未说出口的待办；(b) 在任务真实发生的业务系统里做写操作，而不是在对话框里交一段文字答案。第二点尤其关键：它把“知道该做什么”与“能在系统里改对状态”绑成一个闭环。

二、为什么这是真问题而不是 benchmark 的自我发明（合理推断，但与摘要一致）
1. 知识工作里的任务本质上是“事件驱动的义务”：合同到期、审批卡在某人那里、客户问题无人回复、入职流程缺一步。这些义务不会以 prompt 出现，只以“某条记录处于某种状态”出现。
2. 一旦 agent 常驻，人机接口的成本结构就变了：用户不愿意、也不会为每件小事写清需求；系统的价值上限取决于它能否从噪声中自行打捞出这些义务。这也是为什么“任务发现”值得被单独评测。

三、论文显式给出的两个挑战
1. agent 侧：任务埋在体量巨大的 mail / chat / documents / records 中，信噪比极低。这不是普通检索题——需要判断“哪条消息对我构成义务”，而不是“哪条消息与我语义相似”。这两者在长上下文里会给出完全不同的召回。
2. 评测侧：要判定“该发现的都发现了吗”“做对了吗”，必须先有一个内部自洽的模拟组织（人、事件、文档互相一致），否则任务不是自然涌现的，标准答案也无从定义。摘要明确提出 benchmark 需要这样一个模拟组织，使任务“arise realistically”且“correct outcomes are known”。

四、它主张的现有工作缺口
1. 执行型 benchmark 把任务当作输入，只评 execution，绕过了 discovery。
2. 一些工作要求发现或澄清能力，但不要求跨系统闭环完成。
3. 摘要的判断是“Existing benchmarks evaluate important parts of this problem but not holistically”——缺口在于“不整体”，而非单点能力缺失。具体分类需要回 Section 2 的对照表确认。

五、隐含假设与可质疑之处（我的判断，非论文结论）
1. 可判定性假设：ground truth 由 world state 计算，意味着只有“状态可差分”的任务能被纳入。真实知识工作中大量任务（写一份好方案、说服同事、提前预警风险）没有唯一正确终态。这会系统性偏向流程型/事务型义务，可能高估“主动性”中可被评测的比例。
2. 可枚举假设：任务从“产生义务的事件”确定性派生，等价于假设义务集合是可枚举的。而真实的主动性还包括发现没人定义过的义务（改进机会、隐患），这类开放式主动性大概率在覆盖范围之外。
3. 保真度假设：合成组织的自洽性由构造保证，但“自洽”不等于“真实”。真实 workspace 的噪声是相关噪声（组织历史、人际、政治），合成噪声往往更接近独立噪声，可能让 discovery 的难度失真。
4. 一个方法学上的风险：如果“发现”与“执行”在评分上串联（漏发现即执行 0 分），端到端分数会被 discovery 召回主导，模型排名可能主要反映发现能力而非综合能力。论文用“discovery 与 execution 可分离”这一结论部分回应了这一点，但分解口径需回原文核对。

Q2: 有哪些相关研究？

一、论文自己划定的参照系（摘要片段可支持的部分）
1. 最接近的工作被点名为 PROBE [35]。摘要原句是“PROBE also requires agents to…”，句子在 PDF 抽取中被截断——它到底要求 agent 主动发现信息需求、还是主动澄清，必须回原文确认，不要按我的推测引用。
2. 第二类是 execution-based benchmarks：任务由指令给出，评的是完成质量。论文的批评点很集中——这类设定绕过了“任务从哪来”。
3. 摘要明确“Existing benchmarks evaluate important parts of this problem but not holistically (Section 2)”，说明作者把差异定位在“整体性/闭环”上，而不是声称前人方法有错。
4. CCS 概念里出现 benchmarks、test collections、simulation、evaluation，说明论文自我定位在评测与仿真这条线，而非新模型或新训练方法。

二、按领域展开的相关工作版图（用于定位；论文具体引用了哪些需回 Section 2 核对）
1. Web / OS / 软件工程类可执行环境：把浏览器、桌面、代码仓库做成可交互沙箱，用状态或测试判定成功。共同点是任务由指令给出；本文的差异是任务要先被找出来，且横跨多个办公系统。
2. 企业办公协作仿真：合成邮件、聊天、日历、会议纪要来构造组织记忆，多见于信息检索、问答、会议辅助。这类工作提供了 artifact 的真实感，但通常缺少“事件—义务”因果链，也没有写操作闭环。
3. 主动式/长程助手研究：关注 agent 何时该主动插话、何时该澄清、如何维护记忆与待办。这一支与本文的 discovery 直接同题，但多在对话内部评估，缺少 workspace 级的证据与副作用。
4. 合成数据与世界模型驱动的评测生成：用结构化世界（图、数据库）渲染成自然语言观测，使 ground truth 能从世界状态自动导出，避免人工标注。这是 AutoBuddy-Bench 的方法论近亲，也是我认为最值得借鉴的一支。
5. 长上下文与 needle-in-haystack 式检索：任务信号被稀释在大量文档里，失败模式同源；但本文要求的不只是“找到”，还要判断“它是否构成我的义务”并“在系统里改对状态”，多出两跳。
6. 主动性 / 澄清式交互：把“该不该问”当作策略问题的工作，与本文的 always-on 设定互补——本文提供的是“副作用真实发生”的评测场。

三、本文相对上述各支的差异化（我的归纳）
1. 相对执行型 benchmark：把“任务从哪来”也纳入评测，指标覆盖发现与执行两端。
2. 相对办公仿真：让同一个世界既产任务又判结果，评分与仿真耦合，不需要另建标注集。
3. 相对主动式助手研究：从对话级主动性推进到跨系统、有写入副作用的主动性。
4. 相对合成数据工作：目的不是造训练语料，而是造“可判分”的评测任务；但反过来也可用作训练环境（这是我的延伸判断）。

四、读 Related Work 时的具体注意点
1. Section 2 大概率会按“任务是否给定 / 是否需要写操作 / 单轮还是常驻”做一张对照表，那张表比正文更有信息量，应优先看。
2. 需要确认论文是否讨论了同期工作（concurrent proactive-agent benchmark）。这类工作的差异往往只在两点：是否闭环写操作、ground truth 从哪来。
3. 需要确认它对“simulation fidelity”这条线的引用态度：是把仿真当作必要之恶，还是主张仿真本身即可作为评测标准。这决定了本文结论的可外推程度。

Q3: 论文如何解决这个问题？

一、总体思路
把评测问题转化为“造一个可控世界，再从这个世界自动导出题目与答案”。一句话流水线：world → artifacts → obligations → tasks → execution → world-state diff。

二、五个组件（前四个由摘要明确支持）
1. 世界模型：把组织的 employees、projects、business systems、event history 组织成 temporal graph。时间是一等公民——义务是随时间产生的，任务是否合法取决于“某个事件是否已经发生”。
2. 渲染器：把图渲染成 grounded、realistic 的 workspace artifacts（邮件、聊天、日历、文档、业务记录）。“grounded”是关键约束：每条 artifact 都要能追溯到图里的实体与事件，从而保证跨文档一致性（同一项目名、同一日期、同一负责人、同一金额）。这正是“任务能自然涌现”的前提。
3. 任务派生：从“产生义务的事件”派生 ground-truth 任务。任务不是人写的，而是事件类型的函数。好处是可规模化、与 artifacts 天然一致；代价是任务分布被事件模式限制（这是我从方法推断出的边界）。
4. 评分：每个任务的 outcome 由 world state 计算，而不是文本匹配或 LLM-as-judge。这解决了开放式写操作的判分难题——不管 agent 怎么写邮件，只看“记录是否被正确更新、请求是否被正确满足”。
5. agent 交互协议：从噪声片段看，大意是 agent 必须识别“addressed to the user”的请求，并在独立的 session 里带写权限去执行（原文被 OCR 打碎，如“carries out each in a separate session with write access”，需回原文核对）。合理推断：agent 在没有用户实时指点的会话中获得 workspace 读权限，先自行列出待办，再逐个任务开子会话、拿写权限执行；任务之间相互隔离，避免错误级联污染。

三、AutoBuddy 与 AutoBuddy-Bench 的关系（需回原文确认，但可给出最合理读法）
1. 图 1 标题是“End-to-end score of AutoBuddy with 17 backbone models”，说明 AutoBuddy 同时还是一个可换 backbone 的 agent 系统 / harness。
2. 摘要说“We introduce AutoBuddy-Bench, a simulator”，说明 Bench 指环境 + 任务集 + 评分器。
3. 因此最合理读法是：AutoBuddy = 参考 agent（协议 + scaffold），AutoBuddy-Bench = 世界模拟器 + 任务 + 判分。读论文时必须分清“模型能力”与“harness 设计”各自的贡献边界，否则 17 backbone 的对比会被误读成纯模型排名。

四、方法上的关键权衡（我认为这是本文最可复用的部分）
1. 用“事件”而不是“文档”作为任务之源：文档可以有噪声、冗余、甚至矛盾（只要与图一致），但只要事件图正确，任务与答案就正确。这让“高噪声观测”与“精确 ground truth”可以共存——是解决开放式任务评测的通用配方。
2. 用 world-state diff 而不是 LLM-as-judge：可复现、便宜、无评委偏见；代价是只能覆盖“状态可判定”的任务，这是方法的硬边界，不是实现细节。
3. 一任务一会话 + 写权限：把发现与执行在协议层解耦，便于分别打分；也逼近真实助理“一条线一条线地办”的形态，同时降低跨任务的错误传播。
4. 只变 backbone：把“系统设计”固定为常量，换来干净的模型能力对比，但也把结论绑定在这套协议上（这是方法本身带来的外推限制）。

Q4: 论文做了哪些实验？

一、主实验（由摘要与图 1 支持）
1. 任务集：120 个任务，同一套任务对所有模型。
2. 模型：17 个 backbone，跨 Anthropic / OpenAI / DeepSeek / Others 四个家族（图 1 图例）。
3. 协议：每个配置跑三次取均值（“mean over three runs”），指标是 end-to-end score。
4. 控制变量：只换 backbone，scaffold 固定（“Varying only the backbone”）。

二、分数读数（基于图 1 的 OCR 残片，数值与模型名的对应顺序不可靠，只作量级参考）
1. 区间：约 0.12 到 0.70；中位数在 0.5 附近；最高一档约 0.70，最低一档约 0.12。
2. 家族分布：Anthropic 的 opus 档与 OpenAI 的 sol 档位于前列，DeepSeek 的 Pro 档紧随，Others（如 Kimi-K2.6）处于中段，haiku / mini / flash 类小模型明显掉队。
3. 重要提醒：PDF 抽取把 x 轴标签与柱值打散成“0.7 .70 .68 .65 .63 .63……”这种形态，一个数值序列比模型数多出一位，说明有轴标签混入。因此“哪个模型多少分”必须回原文看图，不要在引用中照抄。

三、论文明确提到的分解分析
1. 核心结论是“只变 backbone”条件下，discovery 与 execution 是两种可分离能力（摘要原句“the two are distinct capabilities”）。这意味着实验至少报告了两个可分离度量，以及它们与端到端分数的关系。
2. 具体口径未知：是发现阶段的召回/F1、还是分阶段通过率、还是条件通过率（在已发现任务上的执行成功率），摘要未说明，需回原文。

四、按常理应存在、但我无法从现有证据确认的实验（需回原文核对）
1. oracle-task 对照：直接把任务清单交给 agent，用来度量 discovery 造成的损失上限。这是该设定下最关键的一张表，也最能支撑“两种能力”的论断。
2. 错误分析：漏发现（false negative）与误发现（false positive）各自的占比，以及误发现是否带来负面分。
3. 交互消融：是否允许向用户澄清、是否提供回话通道、任务是否允许延后处理。
4. 规模消融：workspace 体量、事件历史深度、跨系统数量对分数的影响（用于判断难度来自“体量”还是“推理”）。
5. 成本维度：token、工具调用次数、时延与分数的权衡——always-on 场景下这条几乎必读。
6. 统计量：三次运行的方差、置信区间、模型间差异是否显著。120 个任务、3 次重复的设置下，0.02–0.05 的差距很可能不显著。
7. 人类基线或用户研究。
以上若原文缺失，就是明显的可补实验空间；若原文有，则本报告的这部分应被原文覆盖。

Q5: 发现了什么实验现象？

一、端到端上限明显未饱和
1. 最高约 0.70、最低约 0.12（图 1）。这意味着“常驻主动助理”在当时的强模型上仍是未解任务，且形态不是“少数模型已过线、其余追赶”——整体离饱和有距离。0.70 具体是任务通过率还是归一化分数，需回原文确认，不要按 pass@1 解读。

二、能力不是单峰的：家族交错、档位决定分层
1. 同一家族内部跨度极大（同族既有 0.6+ 也有 0.1–0.3 的档位），说明该任务对模型档位、规模、推理预算高度敏感，而不是靠家族特性取胜。
2. 0.43–0.59 一段非常密集，多个家族的模型咬在一起，说明中段模型的差距可能落在噪声量级内，需要统计检验才能排序。
3. 小/快模型出现“悬崖”而非“略差”：约 0.12–0.27 的几个点提示，当任务需要在大体量多系统 artifacts 中维持长程状态并做写操作时，小模型接近“不做”。这类悬崖式差异在长上下文与多步工具任务中很常见（合理推断）。

三、最重要的现象：discovery 与 execution 是两种能力
1. 论文在只变 backbone 的条件下得出二者可分离（“the two are distinct capabilities”，论文明确支持）。方法学含义很硬：只报端到端分数会把两种能力混成一个数，使“某模型更强”不可解释。
2. 我的解读（非论文原话）：execution 侧的进步（工具调用、指令跟随、格式遵循）不一定迁移到 discovery（判断“什么该做”）。这与“agent 能力 ≈ 工具调用能力”的流行叙事相冲突，是本文最值得带走的一条。
3. 证据强度提醒：摘要只给“distinct”这一质性表述，没有相关系数或排序不一致的具体程度。是否有 rank correlation、散点图或分档交叉表来支撑，需回原文看。若只有“两个指标排名不同”而没有量化，这条结论的说服力会打折。

四、级联失败结构（推断，需分解数据验证）
1. 评分结构上 discovery 是 execution 的前置门：漏掉一个任务，该任务的执行分直接为 0。因此端到端分数对发现召回极其敏感，可能主导模型间差距。
2. 验证方法很直接：若“给定任务后的执行分”显著高于端到端分，则瓶颈在发现侧；若两者接近，则瓶颈在执行侧或评分器。这是我读这篇论文时最想先找的一张表。

五、指标之间的张力（推测，论文未必讨论）
1. 误报与漏报的取舍：主动 agent 多做事会提高召回但增加误操作成本；若端到端分数不区分“做错”和“不做”，可能隐性奖励“乱做”。
2. 这与 always-on 场景的真实代价相反——真实部署中误操作（发错邮件、改错记录）比漏做更贵。若评分器不对此加惩罚，排行榜的最优策略可能偏离真实最优策略。

六、负结果层面的观察
1. 更强的通用推理能力未必带来更好的主动性：中段密集、家族交错是间接证据。
2. 目前没有看到“规模越大单调越好”的强证据（受限于 OCR 不可靠，这条只能作为待验证假设）。

Q6: 有什么可以进一步探索的点？

一、评测层面
1. 把 discovery 升为一等指标：分别报召回、精度、误报代价（做错比不做更糟）、以及“发现延迟”（义务产生到被发现的时间差）。当前端到端单指标无法区分“没看见”和“看见了做错”。
2. 纳入“不可判定任务”与开放式义务：现有 ground truth 依赖 world state 差分，天然排除无唯一正解的任务；如何评测“提了一个好建议”“提前预警了风险”是开放问题，可能需要过程评分或人类偏好。
3. 动态世界：事件持续生成、workspace 在线变化，评测 agent 在流式输入下的状态维护，而不是一次性快照。这会同时考验记忆与增量推理。
4. 人机协同指标：澄清次数、打断用户的次数、节省的分钟数——把技术分数换算成经济量，才好评判 always-on 是否值得。
5. 规模与统计：报告方差/置信区间，扩大任务数以支撑细粒度模型排序；120 任务、3 次重复不足以支撑“0.02 差距”的结论。
6. 对抗性 workspace：注入误导性记录（看似任务实则不是我的义务），专门测误报。

二、方法层面
1. 显式信念状态与待办账本：agent 应维护“我已知的义务集合”，做增量更新，而不是每轮重读上下文；这与长上下文检索的工程折中直接相关。
2. clarify-vs-act 策略：把“何时问、何时做”做成可学习策略；不可逆操作需要“先问再做”的触发条件。
3. 权限与审计：写权限最小化、可回滚、操作日志。这是 always-on agent 落地的硬约束，也是很好的评测维度（目前摘要层面看不到）。
4. 跨会话一致性：一任务一会话的协议下，如何避免重复执行、冲突修改、同一状态被两次更新。

三、训练层面
1. 用 simulator 造可验证 reward 做 RL/后训练：world-state diff 天然是 verifiable reward，适合 RLVR 式训练闭环。
2. 合成困难负例：让 workspace 里存在大量“看起来像任务但不是我的义务”的记录，训练判别而非语义匹配。
3. 课程学习：从单系统单义务 → 多系统交叉义务 → 长历史依赖，逐级加难；并观察 scaling trend 是平滑还是相变。

四、跨领域迁移（与当前画像相关）
1. 科研助理：实验室采购、合规、审稿截止、数据版本、仪器维护——同样是“事件产生义务”，可复用同一套 temporal graph + 渲染 + world-state 判分框架，且天然属于 ai-for-science 的落地场景。
2. 个人生活助理：义务密度低但后果重（账单、预约、续签），误报成本更高，是检验“判分是否惩罚误操作”的好场景。
3. 与生成方向结合：grounded rendering 本身是一个受控合成数据生成问题，一致性约束、噪声注入方式、可控难度都可独立研究。
4. 与组织行为/人因结合：用户对“被代劳”的接受度、审计需求、责任归属，这些是技术指标之外的真实边界。

Q7: 总结一下论文的主要内容

一、论文要解决什么
论文把 agent 的默认设定从“被动接单”推到“常驻在场”。摘要明确批评现有 benchmark“usually give the agent the task to perform”，而知识工作中的请求“sit in the user's workspace”，agent 必须先识别“which obligations exist”。因此它定义了一个新任务：主动式任务发现与执行——从邮件、聊天、日历、文档、业务记录的状态推断用户未说出口的待办，并在义务真正发生的系统里完成写操作。这一设定要求评测同时覆盖“发现”与“执行”两端，而现有工作只覆盖其中一端，论文的判断是“evaluate important parts of this problem but not holistically”。

二、怎么造这个世界（技术主线）
AutoBuddy-Bench 的构造是四段流水线，摘要层面可确认：
1. 世界建模：把组织的 employees、projects、business systems、event history 组织为 temporal graph。时间被显式建模，因为义务是随时间由事件触发的。
2. 渲染：把图渲染成 grounded、realistic 的 workspace artifacts（邮件/聊天/日历/文档/业务记录）。grounded 保证每条 artifact 可回溯到图实体，跨文档一致，从而使“任务自然涌现”成立。
3. 任务派生：从“产生义务的事件”派生 ground-truth 任务——题目不是人写的，是事件类型的函数。
4. 判分：每个任务的 outcome 由 world state 计算，而非文本匹配或模型评审。
这套设计的核心洞见是：把“任务与答案的正确性”锚在事件图上，把“观测的噪声与体量”留给渲染层，于是高噪声与精确 ground truth 可以同时成立。我认为这是本文最可复用的方法论贡献。

三、agent 侧（需回原文确认的边界）
从摘要与图 1 的命名可推断：AutoBuddy 既是论文给出的参考 agent / harness，也是一套可换 backbone 的系统；AutoBuddy-Bench 是环境 + 120 个任务 + 评分器。协议上，agent 需要识别“addressed to the user”的请求，并在独立 session 中带写权限执行（原文片段“carries out each in a separate session with write access”有 OCR 噪声）。这意味着发现与执行在协议层就被拆开，便于分别度量，也降低跨任务污染。

四、实验主线与关键发现
1. 规模：120 个任务，17 个 backbone，跨 Anthropic / OpenAI / DeepSeek / Others 四家族，每配置三次运行取均值，只变 backbone 以固定 scaffold。
2. 读数（图 1 OCR，顺序不可靠）：端到端分数约 0.12–0.70，中位数在 0.5 附近，头部约 0.70，尾部约 0.12；中段 0.43–0.59 很密集，家族交错。
3. 核心结论：在只变 backbone 的条件下，任务发现与任务执行是两种可分离的能力（“the two are distinct capabilities”，论文明确支持）。其方法学推论是评测必须把二者拆开报告，否则端到端单指标会把两种能力混成一个不可解释的数。
4. 由分数结构可做的合理推断：discovery 是执行的前置门，漏发现即该任务 0 分，因此端到端分数可能被发现召回主导；若“给定任务后的执行分”显著高于端到端分，即可确认瓶颈在发现侧。这一点需回原文的分解表验证。

五、论证链的强度评估
1. 强的地方：问题定义清晰且有现实依据；“事件图 + 渲染 + world-state 判分”的构造在逻辑上闭合，解决了开放式写操作难判分的痛点；17 backbone 的同条件对比提供了较宽的实证面。
2. 弱的地方：摘要只给“distinct”这一定性表述，缺相关系数或排序不一致的量化；ground truth 依赖 world state 差分，天然排除无唯一正解的任务，主动性被窄化为“可判定的流程型义务”；120 任务、3 次重复下，中段模型间 0.02–0.05 的差距是否显著未知；只变 backbone 虽控制了变量，却把结论绑定在 AutoBuddy 的协议上，换 harness 后排序是否稳定未知。
3. 需要回原文核对的关键点：Section 2 的对照表、temporal graph 的 schema、事件→义务的派生规则、部分完成的给分方式、是否有 oracle-task 对照、是否有成本与安全维度、是否有统计检验。

六、定位判断
这不是一篇提出新模型能力的论文，而是一篇定义评测范式的基础设施论文：它把“主动性”从对话层的插话行为，推进到跨系统、有写入副作用、可自动判分的闭环。对做 agent 系统的人来说，最可操作的两条结论是：把 discovery 与 execution 分开度量；以及用事件图而非文档做任务与答案的锚点。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与画像首要方向 agent（0.10）直接同题：这是“主动型 agent”的评测基建，而不是又一个工具调用或网页操作 benchmark，定位在范式层。

## 基本信息

- 作者：Shiqi Chen, Daniel Madrigal Diaz, Wei-Ning Chen, Gbola Afonja, Fangkai Yang, Rujia Wang, Chetan Bansal, Victor Rühle, Saravan Rajmohan, Dongge Han
- 机构：Microsoft（合理推断，待核实：作者名单中的 Chetan Bansal、Saravan Rajmohan、Victor Rühle、Fangkai Yang、Wei-Ning Chen、Dongge Han 等常见于 Microsoft 研究体系的论文署名；本次提供的元数据 institution 为 null，PDF 正文未取到作者单位段落，引用前请回原文首页核对）
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG
- 日期：2026-11-09
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.12393v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先采用了 6 条 PDF 语义检索片段（全部落在 Abstract，含图 1 的 OCR 残片），sections 字段全为空，因此方法细节、实验分解、模型名与分数的对应关系多为文内标注过的合理推断，引用具体数值前请回原文核对。
