---
user_id: "cheng tan"
paper_id: 18355
arxiv_id: "2610.34714"
title: "SearchHorizonBench: A Benchmark for Search Value in AI Agents"
publish_date: "2026-10-27"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.34714.pdf"
pdf_url: "https://arxiv.org/pdf/2610.34714"
abs_url: "https://arxiv.org/abs/2610.34714"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-27T12:44:27"
---
# SearchHorizonBench: A Benchmark for Search Value in AI Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：search value · web search agents · agentic search · counterfactual evaluation

## 一句话总结

SearchHorizonBench 是一个从执行经验中学习与评估“搜索价值”的基准：它把搜索价值定义为同一公共历史下继续搜索与直接回答的期望奖励差，并用 45,632 次配对/重复执行在 3,168 个问题上检验历史估计器能否把预测质量转化为成本敏感决策效用。

## 摘要

> Keywords
> AI agents must decide when another search is worth its cost. Web search agents, marginal value, counterfactual evaluation, prob-
> Answer-success probabilities capture question difficulty, while ability calibration, adaptive retrieval, execution experience, bench-
> this decision requires estimating the additional benefit of search. mark
> We introduce SearchHorizonBench, a benchmark for learning and
> evaluating search value from execution experience. Paired continu- 1 Introduction
> ations and repeated executions separate expected search gain from Large-scale agentic search logs document iterative query refor-
> generation noise and connect prediction quality to decision utility. mulation and lexical links to earlier retrieved context [25]. Each
> The resource contains 45,632 executions across 3,168 questions. additional search can resolve a missing relation or entity while con-
> On 1,632 held-out questions, historical calibration reduces Brier suming time and money. In Test-400, permitting search increases
> error by 16.2%, with the improvement concentrated in success-level reference-equivalent answer success from 69.38% to 73.56% and
> information. Search gains have a standard deviation of 21.4 per- mean actor tokens from 1,069 to 6,829. Search-value predictionarXiv:2610.34714v1
> centage points, and tested historical estimators explain less than 2% identifies which questions justify that additional information cost.
> of this variance in point estimates. Matched-configuration learning SearchHorizonBench defines search value as the expected reward
> curves probe this distinction across training scales. At the reference difference between continuing with search and answering from
> token price, free selection observations yield a 2.49-point utility the same public history. The actor completes a chosen continua-
> opportunity over the best constant action, driven primarily by gain tion; the forecaster estimates its outcome. Three measurements
> information. Reusable histories, frozen splits, strong baselines and describe their behavior. Action-success prediction measures cor-
> scoring tools provide a common foundation for developing and rectness under each continuation. Gain prediction measures their
> evaluating value-aware search agents. difference. Decision utility combines the selected reward with the
> cost of acquiring its information. Calibration captures question dif-
> CCS Concepts ficulty; paired gain and cost measurements identify the information
> • Information systems →Evaluation of retrieval results. relevant to choosing further search.
> The benchmark connects these measurements through native
> ∗Corresponding author.
> paired executions. Every saved state supports both continuations

Q1: 这篇论文试图解决什么问题？

1. 核心决策问题：
Web 搜索智能体在每个决策点都面临“继续搜索”与“基于当前公共历史直接回答”的选择。每次额外搜索可能补全缺失的实体或关系，但会消耗 token、时间与 API 成本；因此需要估计的是搜索的边际价值，而非搜索本身是否“相关”。

2. 现有评估的错位：
静态检索基准通常衡量检索结果与答案的相关性/召回率，或端到端 agent 的最终成功率；它们不能回答“如果在这个历史点再搜一次，期望奖励会提高多少”。一个检索片段可能静态相关，却对当前 actor 的最终正确率没有因果贡献；反之，一次看似不相关的搜索可能补全关键线索。论文引用的 Bridge Evidence 一类工作也指出静态检索效用不能预测因果效用（合理推断）。

3. 因果识别难题：
搜索价值是反事实量：同一问题、同一公共历史下，继续搜索的期望奖励与不搜索的期望奖励之差。直接比较“搜索组”和“不搜索组”会被问题难度、actor 能力、历史长度等混杂。即便配对，单次执行仍有生成噪声（采样温度、工具返回、答案抽取）。因此需要配对续写 + 重复执行，把期望增益与执行噪声分开。

4. 预测与决策的脱节：
已有校准研究多关注 answer-success 概率（问题难度），但“知道问题难”不等于“知道搜索能否解决它”。一个高难度问题可能搜了也没用，一个低难度问题可能只差一个实体。论文要检验：历史估计器能否预测 gain；这种预测能否在 token 成本下转化为决策效用；还是说 success 信息主导了校准指标，而 gain 信息才是效用瓶颈。

5. 基准缺口：
缺少一个公开的、带配对反事实续写、重复执行、可复用历史、冻结预测器与决策效用评估的资源。SearchHorizonBench 试图填补这一缺口，并用 45,632 次执行提供足够的方差估计。

6. 隐含假设（需批判性阅读）：
（a）搜索价值可以由公共历史条件下的期望奖励差定义，隐含“历史可复用、无隐藏状态”；（b）奖励以 reference-equivalent answer success 为主，可能忽略部分正确、引用质量与用户效用；（c）actor policy、搜索引擎与 token 价格固定，跨配置迁移性待验证；（d）二值动作（搜一次/不搜）简化了多步搜索、查询改写与源选择。上述为合理推断，具体假设需回原文核对。

Q2: 有哪些相关研究？

从摘要、Introduction 与参考文献片段可辨认出以下几条相关研究线：

1. Agentic search 与迭代检索：
大规模 agentic search 日志记录了迭代式 query reformulation、以及新查询与早期检索上下文之间的词汇链接（论文引用 [25]）。这类工作说明真实搜索是多轮、依赖历史的；但也正因如此，单轮检索指标难以刻画“再搜一次”的边际价值。SearchHorizonBench 把公共历史作为条件，评估下一步动作。

2. 静态检索效用 vs. 因果效用：
参考文献中出现 Bridge Evidence: Static Retrieval Utility Does Not Predict Causal Utility in Multi... 一类工作，核心论点与本文动机一致：检索片段的相关性分数不等于它对下游答案的因果贡献。本文进一步把因果效用操作化为配对续写下的期望奖励差。

3. Agentic search 基准与训练环境：
片段提到 SkillStep Agentic Search、LearnBench 等工作，说明近期已有多个面向 agentic search 的基准/训练资源。本文的差异在于：不只看最终成功率，而是提供反事实的 search/no-search 配对标签、重复执行噪声估计、以及 decision utility 评估。

4. 校准与答案成功预测：
Answer-success 概率、Brier 误差、历史校准是本文评估的核心工具。相关研究包括 LLM 置信度校准、选择性预测、以及基于历史执行数据的经验贝叶斯/等渗校准。本文的发现是：这些工具对 success 有效（Brier -16.2%），但对 gain 的解释力很弱（<2%），从而把“校准”与“搜索价值预测”区分开。

5. 自适应检索与停止决策：
自适应 RAG、search routing、learning-to-stop、metareasoning 等工作通常用置信度、熵、检索分数等启发式决定是否检索。本文提供的反事实标签可用于监督这类策略，并指出仅靠 success 置信度不够，需要 gain 信号。

6. 决策理论与价值 of information：
search value 的定义接近 value of information：在采取行动前，额外信息的期望收益减去成本。本文把这一概念落地为可计算的 benchmark，并用“最佳常数动作”作为 utility 下界，用自由选择观测衡量可实现的机会。

7. 与本文最直接的区别（合理推断）：
现有工作要么评估检索质量，要么评估端到端 agent 成功率，要么做校准；本文把三者串起来：反事实标签 → 预测器 → 成本敏感决策效用。其核心贡献不是某个新模型，而是一个可复用的评估范式和一组揭示“gain 不可预测”的负结果。

Q3: 论文如何解决这个问题？

1. 形式化定义：
对问题 q 与公共历史 h，定义两个续写动作：a_search（继续搜索后再回答）与 a_answer（直接基于 h 回答）。SearchHorizonBench 把 search value 定义为 Δ(q,h)=E[R(q,h,a_search)]−E[R(q,h,a_answer)]，其中 R 为 reference-equivalent answer success（可能再减去 token 成本）。actor 负责完成续写，forecaster 只根据 (q,h) 或公共信号估计各续写的结果；两者的职责分离，避免把 actor 的执行能力与预测能力混在一起。

2. 配对续写与重复执行：
对每个 (q,h) 采样多条 search 续写与 answer 续写，形成配对；重复执行用于估计每个续写的成功率，从而把期望增益与单次生成噪声分开。片段提到 paired continuations and repeated executions separate expected search gain from generation noise，这是识别策略的核心。

3. 资源规模与划分：
45,632 次执行、3,168 个问题；1,632 个 held-out 问题用于主评估。片段还提到 population 中 47 和 44 次执行 unresolved，原始 reward 记录在所有敏感性分析中保持固定。Test-400 是一个允许真实搜索的诊断面板，用于量化搜索的成功率收益与 token 成本。

4. 预测任务：
（a）Action-success prediction：给定续写，预测其正确概率；衡量指标包括 Brier 误差与校准。（b）Gain prediction：预测 search 续写与 answer 续写的成功率之差；这是决策真正需要的量。片段明确说 Action-success prediction measures correctness under each continuation. Gain prediction measures their ... evaluating value-aware...。

5. 估计器与特征：
片段提到 Text 模型与 public-signal 模型、Frozen MiniLM sentence embeddings、stratum controls。合理推断：文本模型用问题/历史的句向量做回归或分类；public-signal 模型加入检索分数、历史长度、查询日志等公共信号；strata 控制任务难度/领域差异。历史校准可能是在 held-out 之前用历史执行数据拟合校准器（如 Platt/isotonic），再在 held-out 上评估。

6. 评估协议：
- Brier 误差：衡量概率预测的校准与锐度；历史校准目标是把 Brier 降低。
- 方差解释：用 held-out 的 gain 方差作分母，看估计器点估计能解释多少（R² 类指标）；结果是 <2%。
- Decision utility：给定预测 gain 与 token 价格，选择 search 当且仅当预测净收益 > 0；与最佳常数动作（总是搜/总是不搜）比较。参考 token 价格下，自由选择观测的效用机会为 2.49 点。
- Matched-configuration learning curves：在相同配置下改变训练规模，观察 gain prediction 与 success prediction 的学习曲线差异，判断瓶颈是数据量还是信号本身。
- Reference-equivalence scoring：固定答案串 + 语义判断，空答案与显式区间有单独处理；评审面板一致性用于控制标注噪声。

7. 可复用性设计：
摘要末尾提到 Reusable histories, frozen…，合理推断是公开可复用的历史轨迹与冻结的预测器/评估配置，方便后续工作在同一反事实标签上比较不同 forecaster 与决策策略。

Q4: 论文做了哪些实验？

1. 主 held-out 评估（1,632 题）：
比较历史校准前后的 Brier 误差；报告 16.2% 的降低，并分解改善来源，发现主要集中在 success-level 信息，而非 gain 信息。这是论文最核心的定量结果之一。

2. 搜索增益的方差分解：
在 held-out 上估计每个问题的 search gain，得到标准差 21.4 个百分点；然后用受测历史估计器的点估计去解释这一方差，R² < 2%。该实验直接回答“gain 是否可预测”，结论是当前估计器几乎不能解释问题间的增益差异。

3. 匹配配置的学习曲线：
固定模型配置与特征集，改变训练规模，分别观察 success prediction 与 gain prediction 的学习曲线。目的是区分“数据不够”与“信号不可学”：如果 gain 曲线随数据量平坦，则说明瓶颈不是样本量。片段未给出具体曲线数值，需回原文核对。

4. 决策效用实验：
按参考 token 价格把预测转化为 search/no-search 决策，计算相对最佳常数动作的效用机会。结果显示自由选择观测有 2.49 个百分点的机会，且主要来自 gain 信息；这说明 success 校准的改善没有同等转化为决策收益。该实验还比较了不同决策规则/成本阈值（合理推断）。

5. Test-400 诊断面板：
允许真实搜索，测量 reference-equivalent 成功率从 69.38% 到 73.56%（+4.18 点），平均 actor token 从 1,069 到 6,829（约 6.4 倍）。该实验量化“搜索的平均收益与成本”，并作为 search value 异质性的宏观背景。

6. 消融/敏感性分析：
- Text-only vs. public-signal 模型：检验公共信号（检索分数、历史结构等）是否提升 gain 预测。
- Frozen MiniLM vs. 其他编码器：控制表示质量的影响。
- Stratum controls：按任务类型/难度分层，排除任务构成差异。
- Bootstrap：区分 execution noise 与 question-bootstrap variability；对 47/44 个 unresolved 执行做敏感性分析，原始 reward 固定。
- 评审一致性：fixed panels 使用同一 reviewer family，报告一致性（片段提到 +0.0153 的 source map 差异），说明语义评分噪声受控。

7. 模型配置诊断：
片段提到 provider 的 thinking mode 忽略 temperature、top-p 行为模型相关；这些不是核心实验，但影响复现与噪声估计，论文应做了相应控制或记录。

Q5: 发现了什么实验现象？

1. 搜索平均有效，但增益高度异质：
Test-400 中搜索把成功率从 69.38% 提到 73.56%，平均 +4.18 点；但 gain 的标准差达 21.4 点，说明有些问题搜索大幅提升，有些问题搜索几乎无益甚至有害。平均值掩盖了决策所需的条件异质性。

2. 成功预测 ≠ 增益预测：
历史校准把 Brier 误差降低 16.2%，但改善集中在 success-level 信息；也就是说，校准器更会判断“这题难不难/actor 会不会做”，而不太会判断“再搜一次能不能把不会变成会”。这是本文最重要的概念区分。

3. 当前历史估计器几乎解释不了 gain 方差：
<2% 的方差解释率是一个强负结果。它意味着，即使有 45,632 次执行和 3,168 个问题，现有特征/模型仍无法可靠排序哪些问题值得搜索。可能原因：gain 依赖查询改写、检索内容、actor 的即时状态等未观测变量；也可能是 gain 本身噪声太大，需要更强的因果表示。

4. 决策效用机会有限且来源集中：
相对最佳常数动作只有 2.49 点效用机会，且主要来自 gain 信息。这说明：如果只用 success 置信度做路由，收益有限；真正的瓶颈是 gain 信号。该结果也暗示“自由选择”在参考价格下并未大幅优于“总是搜/总是不搜”，成本吃掉了很多平均收益。

5. 学习曲线揭示瓶颈不是数据量（合理推断）：
匹配配置下，success prediction 随训练规模改善更明显，而 gain prediction 的曲线更平坦；这支持“gain 信号难学”而非“样本不够”。具体斜率需回原文核对。

6. 成本-收益张力：
搜索带来 +4.18 点成功率，但 token 从 1,069 增至 6,829；按参考价格，净效用取决于每个成功点值多少 token。论文用 decision utility 把这一张力显式化，而不是只看成功率。

7. 校准的分解效应：
Brier 改善集中在 success-level，说明校准可能主要修正了问题难度先验，而没有修正“搜索的因果效应”。这提示未来工作应分别校准 success 与 gain，而不是用一个统一校准器。

8. 执行噪声与问题抽样噪声：
重复执行与 bootstrap 分析显示，gain 估计的不确定性同时来自生成噪声与问题抽样；片段提到 variance estimate 把 sampled corrections 的不确定性与 question-bootstrap variability 分开。这意味着单次执行比较会高估 gain 的可靠性，配对+重复是必要的。

9. 失败模式：
（a）搜索可能引入干扰信息，降低原本可答对的题；（b）actor 可能过度搜索，token 成本压过收益；（c）forecaster 可能把“问题难”误判为“搜索有用”；（d）reference-equivalence 评分可能把语义等价的答案判错，增加标签噪声。上述部分为合理推断，需结合原文失败案例分析。

Q6: 有什么可以进一步探索的点？

1. 更强的 gain estimator：
当前历史估计器解释 <2% 方差，说明需要新的归纳偏置。可探索：（a）显式建模“缺失关系/实体”——把问题分解为子图，预测搜索能否补全；（b）用 actor 的中间状态（草稿答案、不确定性）作为特征；（c）因果表示学习，分离问题难度与搜索效应；（d）用 LLM 生成“搜索后可能获得的信息”并评估其价值。

2. 从二值动作到搜索策略：
基准目前主要比较“搜一次”与“不搜”。未来可扩展到多步搜索、查询改写、源选择、停止时机；把 search value 定义为动态规划中的 value of information，评估长视野策略。

3. 决策理论：
研究成本敏感阈值、风险厌恶、预算约束下的搜索分配；比较 expected utility、CVaR、机会约束等目标；把 token 价格的不确定性纳入决策。

4. 跨模型/跨检索后端泛化：
测试不同 actor（开源/闭源、不同规模）、不同搜索引擎/索引、不同 token 价格下的 gain 可迁移性；研究校准随分布偏移的退化。

5. 数据效率与主动学习：
45,632 次执行成本高；可探索主动采样哪些 (q,h) 最能降低 gain 估计方差；用合成反事实或离线策略评估减少真实搜索。

6. 奖励设计：
从 reference-equivalent success 扩展到部分正确、引用质量、时效性、用户偏好；研究不同 reward 下 search value 是否一致。

7. 与工具调用结合：
把 search value 框架迁移到代码执行、数据库查询、科学仪器调用等；不同工具的成本/收益结构不同，需要领域特定的基准。

8. 理论分析：
给出 gain 可识别性的条件；分析配对续写的偏差-方差权衡；推导需要多少重复执行才能达到给定决策精度。

9. 生产落地：
把 gain predictor 集成到 agent router；在线学习；监控搜索收益漂移；按问题类型设置不同搜索预算。

10. 负结果的价值：
本文的 <2% 解释率提醒社区：不要假设“校准 success 概率”等于“知道何时搜索”。未来应把 gain prediction 作为独立任务，建立专门的评价指标与基线。

Q7: 总结一下论文的主要内容

一、问题与动机：
论文关注 agentic search 中的停止/继续决策：智能体在每一轮都可以选择再搜一次，或基于当前公共历史直接回答。额外搜索可能补全缺失实体/关系，但消耗 token、时间与金钱。现有评估多衡量检索相关性或端到端成功率，无法回答“在这个历史点，再搜一次的期望奖励增量是多少”。答案成功概率只能刻画问题难度，不能替代搜索的边际价值。

二、基准设计：
SearchHorizonBench 把 search value 定义为同一公共历史下，继续搜索与直接回答的期望奖励差。数据包含 3,168 个问题、45,632 次执行。核心设计是配对续写与重复执行：对同一 (q,h)，分别采样 search 与 answer 两条续写，并重复多次，以分离期望搜索增益与生成噪声。actor 负责完成续写，forecaster 只根据公共历史/公共信号预测结果，从而把执行能力与预测能力解耦。资源还包含可复用历史、冻结预测器/评估配置、以及 Test-400 真实搜索诊断面板。

三、评估协议：
主评估在 1,632 个 held-out 问题上进行。预测任务分为 action-success prediction（每个续写的正确概率）与 gain prediction（两条续写的成功率之差）。指标包括 Brier 误差、校准、gain 方差解释率、decision utility、matched-configuration learning curves。决策效用按参考 token 价格计算：预测净增益为正则搜索，否则直接回答；与最佳常数动作（总是搜/总是不搜）比较。评分采用 reference-equivalence：固定答案串加语义判断，空答案与显式区间单独处理；评审面板一致性用于控制标注噪声。

四、主要发现：
1）历史校准把 Brier 误差降低 16.2%，但改善集中在 success-level 信息；2）search gain 的标准差为 21.4 个百分点，受测历史估计器只能解释 <2% 的 gain 方差；3）匹配配置学习曲线显示，gain prediction 的瓶颈不只是训练规模（合理推断）；4）按参考 token 价格，自由选择观测相对最佳常数动作只有 2.49 点效用机会，且主要来自 gain 信息；5）Test-400 中允许搜索把成功率从 69.38% 提到 73.56%（+4.18 点），但平均 token 从 1,069 增至 6,829。

五、解释与张力：
论文的核心概念区分是“成功预测”与“增益预测”：知道问题难，不等于知道搜索能否解决它。Brier 改善主要来自难度校准，而决策真正需要的是条件增益。gain 的高方差与低解释率说明当前估计器可能依赖了错误信号（如问题难度、历史长度），而忽略了查询改写、检索内容、actor 中间状态等因果变量。2.49 点效用机会进一步表明：在参考价格下，搜索的平均收益被成本大幅抵消，自由选择的净收益有限；只有准确识别高增益问题，才能把平均收益转化为决策优势。

六、局限与边界：
（1）奖励以 reference-equivalent success 为主，可能忽略部分正确与用户效用；（2）动作空间主要是二值（搜一次/不搜），多步搜索与查询改写未充分覆盖（推测）；（3）结果依赖固定 actor、搜索引擎、token 价格，跨配置迁移性待验证；（4）45,632 次执行成本高，扩展受限；（5）PDF 片段存在双栏错位，部分方法细节需回原文核对；（6）评审一致性虽受控，但语义等价判断仍有噪声。

七、定位：
这不是一篇提出新 agent 模型的论文，而是一篇基准/评估范式论文。它的贡献在于把“何时搜索”形式化为可计算的反事实估计问题，提供配对标签、重复执行噪声控制、决策效用评估，并用负结果指出 gain prediction 是开放难题。对 agent 系统、自适应检索、AI-for-Science 中的文献/数据库查询策略均有方法学迁移价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：论文研究智能体的搜索决策、工具调用成本、自适应检索与停止策略，属于 agent 系统的核心控制问题。

## 基本信息

- 作者：Jingjie Ning, Xueqi Li, Yibo Kong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG
- 日期：2026-10-27
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.34714`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要与片段，并对双栏抽取导致的错位文本做了甄别；方法细节与数值仍建议回原文核对。
