---
user_id: "cheng tan"
paper_id: 22255
arxiv_id: "2611.22989v1"
title: "How Web Research Agents Interpret User Priorities"
publish_date: "2026-11-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.22989v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.22989v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-21T15:08:17"
---
# How Web Research Agents Interpret User Priorities

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：web research agents · user priorities · assessment criteria · llm agent evaluation

## 一句话总结

论文通过 36 次让两个分别基于 OpenAI 与 Anthropic 模型的 web research agents 在 growth、resilience 或无 priority 条件下比较 Microsoft 与 Alphabet，发现多数报告会聚焦用户声明的 priority，但会自行采用不同的判断 criteria，导致同一 priority 下出现不同甚至相反的公司排序，因而主张 agent 评估与界面应显式检查和暴露 priority 被操作化为哪些 criteria。

## 摘要

> Keywords
> Web research agents gather information online and write cited Web Research Agents, User Priorities, Assessment Criteria, Evalua-
> reports, including financial analysis. Users can state a priority to tion of LLM Agents, Financial Research, Human-AI Interaction
> guide this research, but a priority may not say how to judge the
> options, leaving the agent to choose the criteria. We examine how
> two such agents, built on OpenAI and Anthropic models, interpret
> a stated priority. In 36 runs, the agents compared Microsoft and Al- 1 Introduction
> phabet as businesses. Prompts asked for special attention to growth Large language models (LLMs) can struggle with questions that
> or resilience, or gave no priority. We analyzed reports, searches, require up-to-date information [36]. Web search gives them access
> returned sources and cited sources. Of the 24 reports given a pri- to external sources [25]. Agent-based systems go further and let thearXiv:2611.22989v1
> ority, 21 focused on it, but their criteria varied. Under the growth model decide what to search for, which pages to inspect and when
> priority, OpenAI reports that ranked the companies accounted for it has enough information to answer [27]. We call such systems
> risk or predictability and favored Microsoft, while the Anthropic web research agents when they search the web and produce cited
> ones judged growth by its pace and favored Alphabet. Under the reports [3].
> resilience priority, reports used partly different criteria, and most Two users can give a web research agent the same task and still
> named Microsoft as the more resilient company. Of the search and need different reports, because they have different goals [19, 38]. A
> source measures tested, only the Anthropic agent’s search wording user can tell the agent what matters by stating a priority, asking it to
> differed detectably by condition. These findings suggest evaluations give special attention to one aspect of the task. A stated priority can
> should examine the criteria behind a report’s judgements, not only tell the agent what to pay attention to while leaving open how to
> its focus or accuracy. Interfaces could let users see and adjust those judge the options. Decision analysis treats these as separate choices,
> criteria. choosing an objective and deciding how to measure performance
> on it [15]. We call the grounds a report gives for comparing options
> on a priority its assessment criteria. When the user does not specify
> Sharma and Suchdev

Q1: 这篇论文试图解决什么问题？

1. 核心问题不是检索能力，而是 priority 的解释鸿沟。用户说“特别关注 growth”或“特别关注 resilience”，在自然语言上很清楚，但在决策上并不完整：它指定了注意力对象，没有指定评价属性、度量方式、时间窗口、风险调整方式或权衡规则。以 growth 为例，可以选择营收增速、EPS 增速、云业务增速、用户增长、前瞻指引、股价回报、风险调整后增长等；以 resilience 为例，可以选择资产负债表稳健性、收入周期性、业务多元化、竞争壁垒、衰退期回撤恢复等。不同 criteria 都可能被合理地称为 growth 或 resilience，却可能支持不同公司。论文的核心概念区分是：priority 属于“关注什么”，criteria 属于“如何判断”，二者在决策分析中本是分离的选择；web research agent 把这个原本应由用户或协议明确的选择留给了模型。

2. 这个问题在金融分析中尤其突出。Microsoft 与 Alphabet 都是业务结构复杂的公司，growth 和 resilience 没有单一、公认的度量。agent 若把 growth 操作化为“增长拐点”或“增长速度”，可能偏向 Alphabet；若操作化为“风险调整后增长”或“可预测性”，可能偏向 Microsoft。两种解释都能写得有理有据，却对应不同的投资判断。用户看到的是一份“响应了 growth priority”的报告，实际得到的可能是 agent 自行替换过的决策框架。

3. 现有评估容易漏掉这一层。常见的 agent 评估看任务完成、事实准确性、引用质量、指令遵循或最终答案；这些指标可以全部表现良好，而 criteria 已经分叉。论文因此把评估对象从“报告是否提到 priority”推进到“报告用什么 criteria 来兑现 priority”。这也是它对 LLM agent evaluation 的实质贡献：把解释过程变成可检查的评估面。

4. 由摘要可以合理推断，论文关心四类问题：报告是否聚焦 priority；不同 agent 在相同 priority 下使用哪些 criteria；criteria 是否导致公司排序差异；搜索与来源行为是否随 priority 改变。摘要没有逐条列出 research questions，以上是根据研究设计和发现反推的问题结构。

5. 关键张力是“合规”与“可审计”不是一回事。21/24 的报告聚焦了 priority，说明表面遵循率不低；但 criteria 变化说明解释层并不稳定。另一个张力是“合理解释”与“标准漂移”难以区分：OpenAI 在 growth 下引入风险或可预测性，可能是成熟的风险调整后增长判断，也可能是把 growth 偷换成了更宽泛的 quality 判断。论文没有 ground truth，无法裁决哪种解释正确，这正是它主张把 criteria 显式化的原因。

6. 边界条件也需要明确：论文研究的是两个特定 agent、一个公司比较任务、一个金融分析场景和 36 次运行；它不是在估计所有 web research agents 的普遍行为，也不是在证明某个模型家族更会理解 priority。作者能支持的是“在这组条件下，priority 的解释存在可观察差异，且评估应检查 criteria”，不能支持“某 agent 整体更优”或“某 criteria 是唯一正确标准”。

Q2: 有哪些相关研究？

由于提供的 Related Work 正文为空，以下只能根据摘要引用编号、关键词和结论片段做主题重建，不能视为论文完整文献综述。

1. LLM 的时效性限制与 web search。摘要以 [36] 引出 LLM 对需要最新信息的问题可能吃力，又以 [25] 引出 web search 为模型提供外部来源。这条线索把论文放在“检索增强语言模型”传统中：模型内部知识不足时，通过外部网页补充证据。

2. Web research agents。摘要用 [27] 描述 agent-based systems 让模型决定搜什么、看哪些页面、何时信息足够；又用 [3] 把“搜索网页并生成带引用报告”的系统定义为 web research agents。与此相关的还有工具使用、多步检索、浏览器操作、报告生成和引用归因等研究方向。论文不提出新 agent 架构，而是审计这类 agent 在用户意图解释上的行为。

3. 用户目标、个性化与同一任务的不同报告。摘要以 [19, 38] 指出两个用户给同一任务仍可能需要不同报告，因为目标不同。这连接了个性化、用户建模、偏好 elicitation 和 goal-conditioned generation。论文的切入点更窄：用户已经给出一个 priority，agent 仍要决定如何把 priority 变成判断标准。

4. 决策分析中的“关注对象”与“判断标准”分离。摘要明确提到 decision analysis treats these as separate choices，即决策分析传统会把“关注什么”和“如何判断选项”区分开。论文把这一概念用于 agent：priority 是注意力信号，criteria 是评价规则；前者不能自动决定后者。这是论文最重要的理论支点之一。

5. Agent evaluation 与 instruction following。检索片段出现 Instruction-Following Evaluation for Large Language Models 之类的参考文献残片，说明论文至少触及指令遵循评估传统；但无法确认它在正文中的具体位置。更一般地，相关工作会包括任务成功率、事实性、引用忠实度、工具使用效率、报告质量和人类偏好评估。论文的差异是主张增加 criteria-level evaluation。

6. 人机交互与金融研究。关键词包括 Financial Research 和 Human-AI Interaction；结论片段提到 MyScholarQA [2] 的 approval step，以及用户可 revisiting criteria [29]。这说明论文还把“让用户查看、批准、修改 criteria”视为交互设计问题，而不只是模型评估问题。

7. 研究缺口。综合这些线索，论文瞄准的缺口是：现有工作关注 agent 能否检索、能否写报告、能否遵循指令，但较少系统检查 agent 在 priority 欠指定时选择了哪些 criteria、这些 criteria 是否跨运行/跨 agent 稳定、以及 criteria 如何影响最终判断。论文用一个小规模审计实验填补这一缺口。

Q3: 论文如何解决这个问题？

论文的“解决方案”不是提出新模型或新 agent 架构，而是设计一个审计式实证研究，把 priority 的解释过程拆开观察，并在结论中提出界面层面的改进主张。

1. 固定任务，操纵 priority。所有运行使用同一比较任务：把 Microsoft 与 Alphabet 作为企业进行比较。提示分三种条件：特别关注 growth、特别关注 resilience、不给出 priority。这样可以在任务内容不变的情况下观察 priority 对报告和搜索行为的影响。

2. 两个 agent，重复采样。作者使用两个分别基于 OpenAI 与 Anthropic 模型的 web research agent；每个 agent 在每个条件下运行 6 次，合计 36 次运行。重复采样的作用是暴露同一条件下的随机变异，而不是把单次报告当成模型属性。

3. 多层数据采集。分析对象包括最终报告、搜索查询、返回来源和引用来源。这个设计把“报告层解释”与“搜索层行为”“来源层证据”分开，便于判断差异发生在检索阶段还是综合/判断阶段。

4. 报告级编码。作者检查报告是否聚焦 priority，并提取报告用于比较公司的 criteria、公司排序和理由。检索片段中的表格按 Neutral、Growth、Resilience 编号，列出 winner 与 criterion，说明作者做了逐报告编码；例如 growth 下出现“风险调整后五年增长轮廓”一类理由，resilience 下出现“广告业务周期性”一类理由。

5. 统计检验。结论片段显示作者使用条件标签置换检验，进行了 5,000 次 shuffles；两条件比较涉及 12 次运行。摘要只报告“只有 Anthropic agent 的搜索措辞出现可检测差异”，没有在可见片段中给出完整效应量和校正细节。

6. 分析逻辑。若 priority 被稳定执行，报告应聚焦 priority 且 criteria 应可复现地贴近 priority；若差异主要来自检索，搜索查询和来源指标应随条件变化；若差异主要来自解释与综合，则报告层会分叉而搜索层可能保持稳定。论文的发现更支持后一种解释，但受样本量和测量粒度限制。

7. 设计主张。结论片段提出：让用户看到 agent 准备采用的 criteria，并允许选择或修改；报告中每个判断标注所用 criterion，并说明该 criterion 是用户选择还是 agent 提议；在可验证时链接支持比较的引用证据和对应查询；用户读完后可重新审视 criteria；还可把 criteria 保存下来用于未来相似任务。这是把“解释审计”产品化的思路。

8. 方法缺口。可见证据没有提供完整 prompt、模型版本、agent scaffold、搜索 API、criteria 编码手册、编码者间一致性或预注册信息。因此这套方案在概念上清楚，但可复现性细节仍需回原文确认。

Q4: 论文做了哪些实验？

实验矩阵（据摘要与结论片段重建）：
- Agent：OpenAI-based agent；Anthropic-based agent。
- Priority 条件：growth；resilience；none（neutral）。
- 重复：每个 agent 每个条件 6 次；每个 agent 18 次；总计 36 次；其中带 priority 的报告 24 份，neutral 报告 12 份。
- 任务：把 Microsoft 与 Alphabet 作为企业进行比较。
- 采集数据：最终报告、搜索查询、返回来源、引用来源。
- 统计：条件标签置换 5,000 次；两条件比较涉及 12 次运行。

测量层次可以分成三类。

1. 报告层：报告是否聚焦指定 priority；报告使用哪些 criteria 来比较两家公司；报告最终偏向哪家公司；报告给出什么理由和引用证据。摘要明确报告了 24 份 priority 报告中有 21 份聚焦 priority，以及 growth/resilience 条件下 criteria 与公司排序的差异。

2. 搜索层：搜索查询的数量与措辞是否随条件变化。摘要的结论是，在所测试的搜索与来源指标中，只有 Anthropic agent 的搜索措辞出现可检测的条件差异。片段还给出每个 agent 18 次运行中的查询总量：OpenAI agent 247 条，Anthropic agent 203 条；这是描述性计数，不能直接解释为效率或质量差异。

3. 来源层：返回的 URL 与最终引用的来源是否随条件变化。摘要说其他搜索与来源指标没有可检测差异；片段中的统计表出现 Returned URLs、All 等列名以及 .52、.0064 等数值，但列名和表头被截断，不能可靠判断这些数值对应效应量、p 值还是校正后 q 值，因此不应把具体数字写成确定结论。

实验设计上，三个 priority 条件相当于对“用户 priority”这一因素的准消融：growth 与 resilience 是两种不同关注点，neutral 是无 priority 基线。但这不是组件消融，也没有模型规模、训练算力或检索深度的 scaling 实验。两个 agent 的对比是产品/模型家族层面的对比，且与 scaffold、工具链、系统提示等因素混淆，不能归因于底层模型本身。

论文没有报告人类评估、决策正确性 ground truth、报告质量评分、引用忠实度人工核查或用户信任实验。它也没有在可见证据中说明 21/24 的“聚焦”如何判定、criteria 如何归类、多个编码者是否一致。这些是理解实验强度的关键缺口。

Q5: 发现了什么实验现象？

1. 优先级采纳率高，但操作化一致性低。24 份带 priority 的报告中有 21 份聚焦该 priority，说明 agent 大多没有忽略用户要求；但报告使用的 criteria 并不一致。这个组合是论文最重要的现象：表面遵循率不能证明 agent 理解了 priority 的判断含义。剩余 3 份未聚焦 priority 的报告是 under-interpretation 的候选失败案例，但摘要没有展开它们的具体表现。

2. Growth 条件下，criteria 选择直接翻转公司排序。OpenAI 报告在给公司排序时把风险或可预测性纳入 growth 判断，最终偏向 Microsoft；Anthropic 报告以增长速度或增长拐点判断 growth，最终偏向 Alphabet。表格片段中的理由包括“Microsoft 具有更强的风险调整后五年增长轮廓”和“Alphabet 当前显示出更强的增长拐点”。这说明同一个 growth priority 可以被操作化为“风险调整后增长”或“绝对增长速度”，两者都可能有道理，但决策含义不同。合理推断：这更像 criterion substitution，而不是简单的事实错误；论文没有 ground truth，无法裁决哪种解释更正确。

3. Resilience 条件下出现“同果异因”。多数报告认为 Microsoft 更有韧性，但使用的 criteria 只是部分不同。表格片段中的理由包括广告业务在衰退中通常比企业订阅业务承压更大、Microsoft 的收入结构更偏企业订阅等。只评估最终 winner 会把这种一致性当成“agent 都理解了 resilience”，但理由层面的差异可能影响可迁移性和用户信任。

4. 搜索行为与报告解释出现脱钩。摘要明确说，在所测试的搜索与来源指标中，只有 Anthropic agent 的搜索措辞随条件出现可检测差异；其他指标没有可检测差异。这是一个重要的 negative result：如果检索量和来源选择大体稳定，那么报告层的 criteria 分叉更可能发生在信息综合与判断阶段，而不是“搜到了不同东西”。但这也可能是统计功效不足、测量粒度过粗或指标选择未覆盖关键差异导致的，不能把 null result 解读为“检索完全不受 priority 影响”。

5. 查询数量差异是描述性的，不是条件效应。OpenAI agent 在 18 次运行中共发出 247 条查询，Anthropic agent 发出 203 条；这可以说明两者行为风格不同，但不能说明谁更高效、谁搜索更深入，也不能说明 priority 条件导致了差异。来源质量、来源多样性、引用忠实度和信息覆盖率都没有在摘要中报告。

6. 统计设计存在张力。每个条件只有 6 次运行，两条件比较只有 12 次运行；作者用 5,000 次标签置换来估计随机性，但同时测试了多个搜索/来源指标。只发现一个可检测差异，可能是真实的选择性效应，也可能是低功效下的假阴性。片段中出现的 .85、.0022、.09 等数值因表头截断无法可靠解释，不应写成确定的 p 值或效应量。

7. 没有 scaling trend，也没有组件消融。论文比较的是两个现成 agent，而不是同一模型在不同规模、不同检索深度或不同 scaffold 下的表现；因此不能从结果推断“更大的模型更会解释 priority”或“某个模型家族更尊重用户意图”。模型家族与 agent 实现混淆是这类产品级比较的固有局限。

8. 指标之间存在张力。Priority focus 很高，但公司排序可以相反；最终答案可以一致，但理由结构不同；查询数量可以不同，但来源质量未知；引用数量可以统计，但是否真正支持 criterion 未知。这些张力说明评估应同时看过程指标与结果指标，而不能只取一个“合规率”或“胜率”。

9. 报告中的 criteria 可能是事后合理化。论文从最终报告提取 criteria，这能反映报告如何论证，但不能证明 agent 在搜索前就确定了该 criterion。若要区分“先定标准再找证据”和“先有结论再编标准”，需要过程日志、中间决策点或干预实验；可见证据没有做到这一层。

Q6: 有什么可以进一步探索的点？

论文明确提出的方向（来自结论片段）：
1. 在任务开始前让用户看到 agent 准备采用的 criteria，并允许用户选择或修改；这被描述为类似 MyScholarQA [2] 的 approval step。
2. 在报告中让每个判断标注所用 criterion，并说明该 criterion 是用户选择的还是 agent 提议的。
3. 在可验证时，把判断链接到支持比较的引用证据，并展示针对该 criterion 的搜索查询。
4. 允许用户读完报告后重新审视 criteria [29]，形成“先批准、后复盘”的闭环。
5. 允许用户保存 criteria，用于未来相似任务，把一次解释固化为可复用模板。
6. 评估层面，主张检查 agent 如何把 priority 操作化为 criteria，而不只是检查报告是否提到 priority 或最终答案是否正确。

可进一步探索的方向（基于论文缺口的研究者推断）：
1. 建立 criteria taxonomy 与编码手册，覆盖 growth、resilience 等 priority 的常见操作化方式，并报告编码者间一致性。
2. 做更大的 factorial 实验：更多模型、更多 agent scaffold、更多任务领域、更多重复次数，并预注册主分析与多重比较校正。
3. 引入 ground truth 或专家评审，区分“合理的风险调整后增长”与“标准漂移”，评估 criteria 的规范合理性，而不只是描述差异。
4. 做 mediation analysis：criteria 选择在多大程度上中介了 priority 对最终排序的影响；搜索行为又是否中介了 criteria 选择。
5. 研究交互式澄清：agent 在遇到欠指定 priority 时主动追问，或提出多个 criteria 让用户选择，是否比一次性报告更符合用户意图。
6. 做人类用户研究：criteria 展示/批准机制是否提升信任、决策质量和可审计性，是否增加认知负担或自动化偏差。
7. 研究 criteria 的稳定性与可迁移性：同一用户跨任务是否偏好一致 criteria；保存的 criteria 在新任务、新时间点是否仍然适用。
8. 扩展到更多高风险领域：科学文献调研、政策分析、医疗/法律信息检索、竞品分析等，检验 priority→criteria 鸿沟是否普遍。
9. 研究来源选择与 criteria 的耦合：agent 是否倾向于选择更容易支持某个 criterion 的来源；引用证据是否真正覆盖 criterion，而不是装饰性引用。
10. 研究多语言、多文化用户表达 priority 的差异，以及模型是否系统性地把某些 priority 解释成更窄或更宽的标准。
11. 研究成本、延迟与解释透明度之间的权衡：展示 criteria、等待批准、追溯查询都会增加交互成本，何时值得。
12. 研究安全与操纵风险：agent 提议的 criteria 可能引导用户得出特定结论；需要防止 criterion 选择成为隐性说服或利益冲突的入口。

Q7: 总结一下论文的主要内容

这篇论文是一篇审计式实证研究，而不是提出新 agent 架构的系统论文。它的中心论点是：web research agent 收到用户 priority 后，真正的困难不在于“是否提到这个 priority”，而在于“把 priority 操作化成什么判断标准”。用户说“特别关注 growth”或“特别关注 resilience”，只指定了注意力对象，没有指定度量、时间窗口、风险调整和权衡规则；agent 必须自行补全这些 criteria，而不同补全方式可能支持不同结论。论文借用决策分析中“关注什么”与“如何判断”的区分，把这一解释鸿沟作为研究对象。

技术主线是“固定任务 + 操纵 priority + 重复采样 + 多层日志 + 报告编码 + 统计检验”。作者让两个分别基于 OpenAI 与 Anthropic 模型的 web research agent 完成同一任务：把 Microsoft 与 Alphabet 作为企业进行比较。提示分 growth、resilience、无 priority 三组；每个 agent 每个条件运行 6 次，共 36 次，其中 24 份报告带 priority。分析材料覆盖最终报告、搜索查询、返回来源和引用来源。作者检查报告是否聚焦 priority、使用哪些 criteria、最终偏向哪家公司，并用条件标签置换检验比较搜索与来源指标；两条件比较涉及 12 次运行，置换 5,000 次。

实验主线可以概括为“报告层分叉、行为层相对稳定”。报告层最醒目的结果是：24 份 priority 报告中有 21 份聚焦了 priority，但 criteria 并不一致。growth 条件下，OpenAI 报告把风险或可预测性纳入公司排序，偏向 Microsoft；Anthropic 报告以增长速度或增长拐点判断 growth，偏向 Alphabet。也就是说，同一个 priority 下，两个 agent 不仅理由不同，最终公司排序也相反。resilience 条件下，多数报告认为 Microsoft 更有韧性，但 criteria 只是部分不同；最终答案一致掩盖了理由结构的差异。搜索与来源层的结果则更“冷”：在所测试的指标中，只有 Anthropic agent 的搜索措辞随条件出现可检测差异；其他搜索与来源指标没有可检测差异。OpenAI agent 在 18 次运行中共发出 247 条查询，Anthropic agent 发出 203 条，但这是描述性计数，不能直接解释为条件效应或效率差异。

论证主线由此展开：如果评估只看报告是否“响应了 priority”，21/24 的高聚焦率会让人以为 agent 理解了用户；如果只看最终答案，resilience 条件下的一致性又会掩盖 criteria 差异；但一旦检查 criteria，就会发现同一 priority 可以被解释成不同的决策规则，甚至导致相反结论。因此，论文主张评估应增加 criteria-level 的检查：报告是否说明所用标准、标准从何而来、标准是否与用户 priority 一致、标准是否被证据支持。结论部分进一步提出交互设计：让用户查看和批准 criteria；在报告中标注每个判断使用的 criterion 及其来源（用户选择或 agent 提议）；链接支持该判断的引用证据和对应查询；允许用户读完后重新审视 criteria；并保存 criteria 以供未来相似任务复用。

贡献与边界需要同时看。贡献在于把“用户 priority 的解释”从一个抽象的人机交互问题变成可编码、可比较的实验对象，并给出了一个反直觉但重要的组合：高 priority 聚焦率不等于高解释一致性；搜索行为稳定也不等于报告判断稳定。边界在于样本小、任务单一、领域集中在金融分析、两个 agent 的模型与 scaffold 混淆、没有公司排序的 ground truth、没有人类用户评估，且可见证据缺少完整 prompt、模型版本、criteria 编码手册和编码者间一致性。因此，论文更适合被读作“提出评估维度并给出存在性证据”的研究，而不是“估计普遍行为分布”或“判定某个 agent 更优”的研究。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：论文把 web research agent 的评估从任务完成/引用质量推进到用户 priority 的解释与 criteria 选择，适合关注 agent 可靠性、可审计性和人机协作的研究者。

## 基本信息

- 作者：Robin Sharma, Parisa Suchdev
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.HC, cs.AI
- 日期：2026-11-17
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.22989v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract 与 Conclusion 片段，并对缺失的 Method/Results 细节、统计表头和机构信息做了显式缺口标注。
