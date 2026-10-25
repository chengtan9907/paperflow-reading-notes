---
user_id: "cheng tan"
paper_id: 17786
arxiv_id: "2610.30989v1"
title: "ColaBench: Evaluating Memory-Guided Knowledge Acquisition in Personal Agents"
publish_date: "2026-10-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.30989v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.30989v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-25T15:16:52"
---
# ColaBench: Evaluating Memory-Guided Knowledge Acquisition in Personal Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：personal agents · long-term memory · knowledge acquisition · benchmark

## 一句话总结

论文提出 ColaBench——一个含 349 个任务、10 个用户 Profile 与 13,033 篇文档的健康/日常生活合成基准，用 Evidence Graph 追踪个人条件、历史披露与答案证据，并配套 MemKB 以状态摘要和记忆—文档链接引导检索与规则应用，取得 86.25% Accuracy、平均 4.07 轮交互。

## 摘要

> and an evaluation framework for studying how memory guides
> Personal agents need to draw on long-term interaction history to knowledge acquisition and rule application.
> retrieve and apply external knowledge suited to a user’s current
> request. Even an answer grounded in relevant documents can be CCS Concepts
> unsuitable when it overlooks the user’s circumstances. This creates • Information systems →Personalization; • Computing
> two connected challenges: using historical conditions to identify methodologies →Intelligent agents.
> the knowledge needed for a new request, and applying that knowl-
> edge to the user’s situation. Existing benchmarks advance memory Keywords
> recall, personalized retrieval, and memory-guided action, while the
> agents, long-term memory, knowledge acquisition, data synthesis, joint evaluation of these two challenges remains underexplored. We
> benchmarks introduce ColaBench, a synthetic benchmark in health and daily
> living, with 349 tasks across 10 Profiles and a knowledge base of
> 13,033 documents. Its Evidence Graph connects personal condi-
> 1 Introduction
> tions to their disclosures across public interaction sessions, records
> Personal agents must draw on long-term interaction history to prior document exposure, and links answer requirements to per-arXiv:2610.30989v1 handle users’ evolving requests [10, 19]. Users often leave prior sonal and newly acquired document evidence. We also introduce
> experiences, preferences, and constraints unstated [6], while the MemKB, which uses state summaries to inform queries and rule
> current task may require new external knowledge [8]. Consider a application, and memory–document links to retrieve knowledge
> associated with the user’s history. MemKB achieves 86.25% Accu- user asking whether they qualify for a caregiver respite grant after
> mentioning, in separate conversations, that they care for a relative racy with an average of 4.07 interaction rounds per task. Task-level
> with Alzheimer’s disease and live alone. The agent must find the analysis finds larger gains on tasks integrating information across
> grant’s eligibility rules and connect the co-residence requirement multiple historical Facts; error analysis further identifies unsup-
> to the earlier disclosure of living alone (Table 1). Overlooking that ported personal assumptions and rule misapplication as obstacles
> connection can turn relevant information into unsuitable advice. to using retrieved evidence. ColaBench provides traceable tasks
> Reliable personalization therefore depends on how history guides
> ∗Primary contact: Xuhui Jiang. knowledge acquisition and how the acquired knowledge is applied
> to the user.
> WWW ’27, Dublin, Ireland These tasks pose two connected challenges. Knowledge acqui-
> 2027. sition requires recovering the historical conditions that determine
> WWW ’27, May 10–14, 2027, Dublin, Ireland Zhang et al.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的问题可以分三层。
1. 表层任务失败：个人智能体回答用户当前请求时，常常需要外部知识（政策、资格规则、健康建议等），但仅靠检索相关性不够。论文开篇的照护者 respite grant 例子说明：用户曾在不同会话提到照顾阿尔茨海默病亲属、独居；当前问自己是否符合补助资格。智能体必须找到补助的居住/同住规则，并把“独居”这一历史披露与“照护对象须与申请人同住”的规则连起来。若忽略这层连接，相关文档会被误用成不适合的建议。
2. 机制层缺口：论文把问题拆成两个耦合挑战：(a) 用历史条件识别新请求需要什么知识——记忆影响查询构造与检索目标；(b) 把获取到的知识应用到用户情境——记忆影响规则匹配、条件核验与最终判断。二者不是先后独立的流水线：检索什么取决于对用户状态的理解，而应用又取决于检索到的规则与个人事实是否对齐。
3. 评估层缺口：现有基准分别覆盖记忆召回、个性化检索、记忆引导行动，但缺少对“知识获取”与“规则应用”的联合评估。若只测记忆召回，无法判断智能体是否会把召回的个人条件用于检索；若只测个性化检索，无法判断检索到的规则是否被正确套用到用户身上；若只测行动，又难以区分失败来自记忆、检索还是规则推理。论文因此需要可追踪的中间证据：个人条件从哪次披露来、文档何时被暴露、答案要求对应哪些个人/文档证据。
4. 隐含假设与边界：基准是合成环境，健康与日常生活领域，10 个 Profile、349 个任务，KB 冻结为 13,033 篇文档；这提供了可控性与可追踪性，但也意味着真实用户的噪声披露、矛盾陈述、规则时效性、跨领域迁移和隐私约束尚未被覆盖。论文用“同证据”“新文档”“隐藏历史”“无历史答案”等门控来分离记忆贡献与检索贡献，这一设计本身回应了“相关但不合适”的评估难题。
5. 可反驳点：如果任务答案主要靠新文档即可得出，记忆就是装饰；如果给同一文档后无历史模型也能答对，则联合评估不成立。论文的 Novelty/Final audit gate 和 Same evidence/New document 条件正是在试图排除这类伪任务。以上对门控语义的概括来自片段中的图式标签，具体实现需回原文核对。

Q2: 有哪些相关研究？

论文的相关工作至少可放在四条脉络中；由于 PDF 片段只保留了部分 Related Work 信息，以下对具体文献的归属做“合理推断”，并以论文明确提到的线索为锚。
1. 会话式/个性化检索与知识助手：片段明确提到 TREC iKAT 2023（Conversational and Interactive Knowledge Assistants）与 LaMP（Language Model Personalization），以及 Personalized Conversational Search。这一脉络关注在多轮会话中理解用户意图、做个性化检索与答案生成；其评价通常围绕检索相关性、答案质量或个性化匹配。ColaBench 的差异在于把“个人条件”作为检索与规则应用的前置变量，并用 Evidence Graph 追踪条件披露与文档证据，而不只评价最终答案。
2. 智能体长期记忆：论文指出已有工作推进 memory recall、memory-guided action。记忆研究通常问“能否召回过去信息”，行动研究问“记忆能否改进任务执行”。ColaBench 追问的是中间环节：召回的记忆是否被用于构造查询、是否被用于判断外部规则能否套用到用户。也就是说，记忆不是上下文里的装饰性片段，而是知识获取与应用的控制变量。
3. RAG/工具使用与知识获取：个人智能体需要检索外部知识库，可能涉及多轮搜索、文档阅读、证据筛选。论文的 MemKB 使用混合检索（语义+关键词+记忆—文档链接，合理推断三通道等权融合），并通过状态摘要把分散的个人条件带入查询。与一般 RAG 基准相比，ColaBench 的文档层是冻结的、可追踪的，且任务要求区分“个人证据”与“新获取文档证据”。
4. 个性化评估与合成基准：健康与日常生活是个人化规则密集的领域，适合构造“资格/建议”类任务。合成基准的优势是可控、可审计、可覆盖多 Fact 组合；风险是合成痕迹、语言/文化单一、规则简化。论文用 10 个 Profile、349 个任务、13,033 篇文档搭建测试床，并用审计门控（Blind solver、Final audit gate、Novelty 等）过滤不满足条件的任务。
5. 与本文最直接的区别：现有基准“分别”推进记忆召回、个性化检索、记忆引导行动，ColaBench 把三者放进同一个任务级评价：给定公开交互历史与知识库，智能体需要先找到规则，再把个人条件与规则条件对齐，最后给出可追溯答案。论文的贡献不只是数据集，还包括 Evidence Graph 这一结构化评估中介和 MemKB 这一可消融的方法基线。

Q3: 论文如何解决这个问题？

论文的解决方案由“基准”与“方法”两部分组成。
一、ColaBench 的构造
1. 领域与规模：健康与日常生活；349 个任务、10 个 Profile、13,033 篇文档的知识库。任务围绕用户的个人条件与外部规则/知识展开，要求智能体在长期交互历史中找条件、在 KB 中找规则，并完成判断。
2. Evidence Graph：这是基准的核心数据结构。它把个人条件连接到其在公开交互会话中的披露（即某条件在哪次对话被说出），记录用户此前对哪些文档有曝光，并把答案要求（answer requirements）连接到个人证据与新获取的文档证据。这样，任务不是只有输入输出，而有可追踪的证据链：答案需要哪些条件、这些条件来自哪次披露、需要哪篇文档、文档是否在历史中出现过。
3. 构造阶段：片段显示至少有 Stage 0: Building the Document Layer（收集公开文档并构建文档层），后续阶段应包含 Profile/任务生成、证据图构建、审计与过滤（具体阶段编号需回原文）。网页在交互过程中被发现时也会被加入 Document nodes，说明文档层与交互历史是耦合的。
4. 评估门控：片段中出现 Blind solver、Final audit gate、Same evidence、New document、Search/read documents、History hidden、Answer、Recompose、Rebuild rule、Reference answer、Grounding conditions、application + requirements、No-history answer、Novelty Fail 等标签。可合理推断：基准用“盲解器”检查任务是否可解；用“最终审计门”检查证据与答案一致性；用“同证据”条件检验给相同文档时记忆是否仍必要；用“新文档”条件检验任务是否真的需要新知识；用“隐藏历史”和“无历史答案”构造反事实基线；用“Novelty Fail”拒绝那些无需记忆或无需新文档就能答对的伪任务。
二、MemKB 方法
1. 状态摘要：MemKB 用状态摘要（state summary）把分散在多次会话中的个人条件组织起来，带入查询构造与后续判断。片段明确说 user state summary brings dispersed personal conditions into query formulation and subsequent judgment。
2. 记忆—文档链接：MemKB 建立记忆与文档之间的链接，用这些链接检索与用户历史相关的知识；这不是单纯按当前 query 做语义检索，而是把“用户过去暴露过的文档/与条件相关的文档”作为检索信号。
3. 混合检索：片段显示 MemKB 通过语义通道与关键词通道直接检索 KB，并以三通道等权融合（第三通道很可能是记忆—文档链接通道，需回原文确认）；每篇文档在每个通道最多贡献一次，融合后进入阅读/判断。
4. 规则应用：MemKB 不仅检索，还把答案要求拆成条件（grounding conditions / application + requirements），用个人证据核验条件，再套用规则。片段中的 Rule: The care recipient must live at home with the applicant. Judgment: Living alone does not satisfy the co-residence requirement 正是这种条件化判断的示例。
5. 交互式执行：MemKB 平均 4.07 轮完成一个任务，说明它不是单轮 RAG，而是多轮搜索/阅读/判断的智能体流程；具体轮次控制、停止条件和重试机制需回原文。
三、设计权衡
- 用合成基准换取证据可控与隐私安全，但牺牲真实噪声；
- 用结构化 Evidence Graph 换取可追踪性，但构造成本高；
- 用状态摘要压缩历史，可能损失细节，但能缓解长上下文与分散条件的问题；
- 用记忆—文档链接增强个性化检索，但可能引入历史曝光偏差（过去看过的文档不一定当前相关），论文用检索融合与消融来检验其贡献。

Q4: 论文做了哪些实验？

论文的实验围绕三个问题展开：(1) MemKB 在 ColaBench 上整体表现如何；(2) 性能增益来自哪些组件，尤其在单 Fact 与多 Fact 任务上是否不同；(3) 失败案例暴露了哪些使用证据的障碍。由于片段未给出完整 baseline 列表，以下只写可确认的设置与结果，具体 baseline 名称需回原文核对。
1. 主实验：在 349 个任务、10 个 Profile、13,033 篇文档的 ColaBench 上评测。MemKB 取得 86.25% Accuracy，平均每任务 4.07 个交互轮次。片段未给出其他 baseline 的具体分数，因此不能在这里比较排名；但论文显然设置了无历史/同证据/新文档等条件来分离记忆与检索的贡献。
2. 任务分层：按任务需要整合的历史 Fact 数量分层。多 Fact 任务需要把多条个人条件组合起来才能匹配规则；单 Fact 任务只需一条条件。论文报告在 275 个任务上，单 Fact 与多 Fact 任务的增益分别为 5.86 与 12.24 个百分点；多 Fact 与单 Fact 的未调整增益差为 4.39 个百分点。后续用分层任务数作权重（stratum weights）做加权分析，两组使用相同权重，以控制任务构成差异。
3. 消融实验：111 个任务的消融支持两个组件——组织化的个人条件（state summaries）与记忆链接知识（document links）。片段明确说 ablations support the contributions of state summaries and document links。其中 6.5 节专门分析 State Summaries 与 Document Links；有文档链接时，MemKB 在更多任务上阅读参考文档，并在更多任务上正确完成。
4. 检索行为分析：片段提到 divergent trends distinguish search exposure from subsequent reading——搜索曝光与后续阅读呈现不同趋势；仅让文档出现在检索结果里不够，智能体是否真正阅读并用于判断才是关键。这解释了为什么 document links 的增益不只是召回率提升，而是阅读/应用行为的改变。
5. 错误分析：对 48 个 MemKB 失败答案，用 GLM-5.3 Flash（low）作为评判/标注模型，结合完整评估参考与真实答题轨迹做错误归类；Figure 4 把每个答案的主要错误类别与其关键证据的可见性关联。论文指出两类主要障碍：无依据的个人假设（unsupported personal assumptions）与规则误用（rule misapplication）。
6. 评估协议实验：片段中的 Same evidence、New document、History hidden、No-history answer、Novelty Fail 等条件应属于实验设计的一部分，用于检验任务是否真正需要记忆与新文档。具体每个条件的样本量和对比结果在片段中不完整，需回原文确认。

Q5: 发现了什么实验现象？

从片段可提炼出若干值得注意的实验现象与张力。
1. 整体准确率与交互成本：MemKB 达到 86.25% Accuracy，平均 4.07 轮/任务。这说明多轮记忆引导的检索与规则应用是可行的，但也留下约 13.75% 的失败空间；论文没有报告延迟/token 成本，因此不能判断 4.07 轮是否经济。
2. 多 Fact 任务增益更大：多 Fact 任务的增益（12.24 个百分点）高于单 Fact 任务（5.86 个百分点），未调整差异 4.39 个百分点。这是一个反直觉但合理的现象：多 Fact 任务本来更难，记忆与结构化状态摘要的价值更大；单 Fact 任务可能靠局部检索就能解决，记忆的边际收益较小。它提示评估个人智能体时，应把“需要跨会话组合条件”的任务单独分层，否则整体准确率会掩盖方法差异。
3. 状态摘要与文档链接的贡献不是简单叠加：消融支持两者都重要，但片段没有给出完整的 2×2 交互结果。可合理推断：状态摘要主要改善查询构造与条件覆盖，文档链接主要改善证据获取与阅读；若只有摘要没有链接，智能体可能知道该找什么但找不到；若只有链接没有摘要，可能找到文档但无法把个人条件对齐规则。
4. 搜索曝光≠阅读使用：片段明确说 divergent trends distinguish search exposure from subsequent reading。有文档链接时，MemKB 在更多任务上阅读参考文档，并在更多任务上答对。这意味着检索指标（文档是否被召回）与最终正确率之间存在断裂；仅优化召回可能不转化为答案质量。对 RAG/agent 评估的启示是，需要记录“文档是否被打开/阅读/引用”，而不能只看检索命中。
5. 错误结构：48 个失败答案的错误分析把主要问题归为无依据的个人假设与规则误用。前者是“记忆不足却自信补全”——智能体假设用户有某条件，但 Evidence Graph 中没有披露；后者是“有规则但套错”——例如把同住要求与独居事实错误匹配。这两种错误对应问题的两端：知识获取（该找的证据没找到/没用到）与知识应用（找到了但判断错）。
6. 证据可见性与错误类别相关：Figure 4 把错误类别与关键证据可见性关联，暗示部分失败是因为关键证据在交互中不可见或未被暴露；这为“何时应该追问/澄清”提供了线索。
7. 审计与失败任务：Novelty Fail 等标签说明合成任务并非全部可用，部分任务因无需记忆或无需新文档被拒。这本身是一个重要的“负结果”：构造一个真正需要记忆与新知识的任务比构造一个看起来个性化的任务更难；基准质量依赖审计门控。
8. 边界：以上现象都来自合成健康/日常生活任务，不能直接外推到真实临床或政策场景；真实场景中规则有时效、例外和模糊性，用户披露也可能矛盾。论文的错误类别是一个起点，不是完整分类学。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向可分为数据、方法、评估与应用四层。
一、数据与基准
1. 从合成走向真实：在隐私合规前提下引入真实用户交互（脱敏、授权、联邦式构造），检验合成任务上的结论是否成立；真实数据会带来噪声披露、矛盾陈述、遗漏条件与口语化指代。
2. 领域扩展：从健康/日常生活扩展到金融、法律、教育、政务、企业 IT 等规则密集领域；不同领域的规则复杂度、时效性与风险等级不同，可比较方法迁移性。
3. 时间维度：当前 KB 是冻结的，规则可能更新；可构造“规则版本随时间变化”的任务，要求智能体区分历史有效规则与当前规则，并处理用户条件的变化。
4. 多语言/多文化：资格规则与家庭结构在不同文化下差异很大；跨语言迁移与本地化评估是空白。
5. 对抗与隐私：构造隐私攻击任务（智能体是否泄露历史披露）、记忆投毒任务（错误历史如何污染判断）、以及“该忘就忘”的遗忘评估。
二、方法与架构
6. 记忆表示：比较结构化状态摘要、原始对话检索、知识图谱、事件时间线等表示；研究压缩与保真度的权衡，以及摘要丢失关键条件时的回退机制。
7. 检索—应用联合优化：当前 MemKB 把检索与规则应用分成多轮；可研究端到端训练、检索决策与澄清提问的联合策略，让智能体在证据不足时主动问用户，而不是做无依据假设。
8. 证据核验与不确定性：为每个答案条件输出“支持/反驳/未知”三值判断，显式建模未知条件；在规则存在例外时，学习例外检测与优先级。
9. 多跳规则推理：多 Fact 任务已显示增益，但规则可能有多个前提、否定条件与例外；可引入程序化规则执行、符号推理与 LLM 结合。
10. 记忆冲突解决：当用户在不同时间披露矛盾信息（如居住情况变化），智能体应识别时间戳与可信度，而不是简单取最新或取摘要。
三、评估方法
11. 更细的过程指标：除 Accuracy 与轮次外，记录证据召回、证据阅读、条件覆盖、规则匹配、引用忠实度、澄清质量、成本与延迟；建立“检索曝光→阅读→应用”的漏斗指标。
12. Judge 可靠性：论文用 GLM-5.3 Flash 做错误分析；可研究 LLM-as-judge 在个性化规则判断上的偏差，加入人工复核与多 judge 一致性。
13. 反事实评估：Same evidence / New document / History hidden 等条件可进一步发展成因果拆解，估计记忆、检索、规则推理各自的因果效应，而不只是消融。
14. 任务难度标定：按 Fact 数量、规则复杂度、证据分散度、干扰文档相似度等维度做难度分层，避免整体分数掩盖长尾失败。
四、应用与交互
15. 主动澄清：当关键条件缺失时，智能体应提问而不是假设；可评估提问的信息增益与用户负担。
16. 可解释与可审计：把 Evidence Graph 暴露给用户，生成“因为你在某次对话提到 X，且规则要求 Y，所以结论 Z”的解释；评估解释是否提升信任与纠错。
17. 安全与责任：健康/补助建议涉及高风险，需研究拒答边界、专业转介、错误后果分级；不能把基准准确率等同于部署安全性。
18. 与工具生态结合：个人智能体可能调用表单、计算器、政策 API；可研究记忆引导的工具选择与参数填充。

Q7: 总结一下论文的主要内容

论文的主线可以概括为：个人智能体的个性化不是“把历史塞进上下文”那么简单，而是要解决记忆如何引导外部知识获取、以及获取到的知识如何应用到用户情境这两个耦合问题。作者用 ColaBench 把这条主线变成可追踪、可消融的实验。
1. 动机与问题。个人智能体要处理用户不断变化的请求；用户往往不会一次性说出所有经验、偏好与约束，而当前任务又可能需要新的外部知识。论文举了一个典型例子：用户问自己是否符合照护者 respite grant 的资格，曾在不同会话提到照顾阿尔茨海默病亲属并独居。智能体必须找到补助规则，并把“照护对象须与申请人同住”与“用户独居”这两条信息连起来；否则，即使检索到相关文档，也会给出不合适建议。由此，论文把问题拆成两个挑战：用历史条件识别所需知识，以及把知识应用到用户情境。现有基准分别推进记忆召回、个性化检索与记忆引导行动，但缺少联合评估。
2. 基准。ColaBench 是健康与日常生活领域的合成基准，含 349 个任务、10 个 Profile、13,033 篇文档。它的核心是 Evidence Graph：把个人条件连接到其在公开交互会话中的披露，记录用户此前对文档的曝光，并把答案要求连接到个人证据与新获取的文档证据。这样，每个任务都有可追踪的证据链，而不是只有问题与答案。构造上，论文先构建文档层（Stage 0），再从公开交互中抽取/组织个人条件与任务，并用审计门控过滤不满足条件的样本。片段中的 Blind solver、Final audit gate、Same evidence、New document、History hidden、No-history answer、Novelty Fail 等标签，说明基准试图保证任务确实需要记忆与新文档，而不是靠常识或单轮检索就能答对。
3. 方法。MemKB 是论文提出的基线方法。它用状态摘要把分散在多次会话中的个人条件组织起来，带入查询构造与规则应用；用记忆—文档链接检索与用户历史相关的知识；检索上采用语义、关键词与（合理推断）记忆链接三通道等权融合，每篇文档每通道最多贡献一次。MemKB 不是单轮 RAG，而是多轮搜索/阅读/判断的智能体流程，平均 4.07 轮完成一个任务。它把答案要求拆成条件，用个人证据核验，再套用规则；论文表格中的例子是“照护对象须与申请人同住”与“独居不满足同住要求”的判断。
4. 实验与结果。主实验在 349 个任务上报告 MemKB 86.25% Accuracy、平均 4.07 轮。任务级分析发现，需要整合多条历史 Fact 的任务增益更大：在 275 个任务上，单 Fact 与多 Fact 任务的增益分别为 5.86 与 12.24 个百分点，未调整差异 4.39 个百分点；随后用分层任务数作权重做加权分析。111 个任务的消融支持状态摘要与文档链接的贡献；有文档链接时，MemKB 在更多任务上阅读参考文档，并在更多任务上正确完成。论文还观察到“搜索曝光”与“后续阅读”的分离趋势：文档被检索到不等于被阅读和使用，这解释了为什么仅优化召回不一定提升答案正确率。
5. 错误分析。论文分析 48 个失败答案，使用 GLM-5.3 Flash（low）结合评估参考与答题轨迹做归类；Figure 4 关联错误类别与关键证据可见性。主要障碍是两类：无依据的个人假设（智能体假设了未被披露的条件）与规则误用（找到规则但条件匹配/判断错误）。这两类错误正好对应知识获取与应用两端。
6. 结论与定位。论文主张 ColaBench 通过 Evidence Graph 把答案要求与历史/文档证据连接起来，MemKB 以更少交互轮次提升准确率，消融支持状态摘要与记忆链接知识的作用；二者共同把记忆评估推进到“智能体获取了什么知识、对用户做了什么判断”。需要注意，这是一个合成基准，规模为 10 个 Profile、349 个任务，KB 冻结；其结论能否外推到真实高风险场景、动态规则与噪声披露，仍需进一步验证。片段未给出完整 baseline 列表与所有实验数值，部分方法细节（如三通道的具体构成、审计门控的实现）需回原文核对。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与你的智能体方向直接相关：论文讨论长期记忆、检索、规则应用与多轮执行，属于 agent memory / personalized agent 的系统性工作。

## 基本信息

- 作者：Wenjie Zhang, Zhichao Shi, Xuhui Jiang, Xiaojun Wu, Cehao Yang, Chengjin Xu, Xiaoning Song
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-22
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.30989v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 55 个片段，主要依据摘要、Introduction、Method 片段与 Conclusion；由于原文存在双栏交错与 OCR 噪声，部分方法/实验细节标注为需回原文核对。
