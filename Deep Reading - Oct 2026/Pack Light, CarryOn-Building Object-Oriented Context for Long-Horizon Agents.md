---
user_id: "cheng tan"
paper_id: 16401
arxiv_id: "2610.18784v2"
title: "Pack Light, CarryOn: Building Object-Oriented Context for Long-Horizon Agents"
institution: "合理推断：加州大学圣地亚哥分校（UCSD，作者 Julian McAuley 的长期所属）与南京大学（项目页域名 ethannju.github.io 中的「nju」线索）；论文元数据 institution 字段为空、PDF 证据未含作者单位段落，上述归属需以原文首页核对，不应作为确定事实引用。"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.18784v2.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18784v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-02T01:09:27"
---
# Pack Light, CarryOn: Building Object-Oriented Context for Long-Horizon Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-horizon agents · context management · agent memory · object-oriented context

## 一句话总结

CarryOn 把长程智能体的上下文从「交互历史的压缩产物」改写为「可复用任务对象的视图」：以 continuation carriers 承载已确立的进展、以 execution context 承载未完成的约束与目标、以 live tail 保留最近交互，并用 Tracker–Grounder–Compiler 在每次 fold 时只对受影响的记录做增量修订；在 OSWorld 2.0 与 BrowseComp-Plus 上分别取得 +5.8% 与 +7.0% 的 partial/accuracy 提升，同时总 token 用量下降 51.9% 与 22.9%。

## 摘要

> Long-horizon agents accumulate progress in persistent task state, yet repeatedly process interaction histories describing how that progress was achieved. This raises a natural question: can long-horizon context instead be organized around reusable progress rather than the history that produced it? In this paper, we introduce CarryOn, an object-oriented context runtime that organizes continuation around reusable task objects. Rather than treating compressed histories or reconstructed accounts of progress as the primary basis for continuation, CarryOn builds a context view from continuation carriers, complementary execution context, and a live tail of recent interactions. A core challenge is keeping this reusable basis valid after its surrounding history is retired: how can CarryOn update the basis without reconstructing the full representation at every fold? CarryOn maintains carrier records across folds, revising affected records against bound evidence and applicable environment checks while carrying forward unaffected records and jointly updating execution context. Experiments on OSWorld 2.0 and BrowseComp-Plus show improved gains of 5.8% and 7.0% while reducing total token usage by 51.9% and 22.9%, respectively. Project page: https://ethannju.github.io/carryon

Q1: 这篇论文试图解决什么问题？

一、问题起点：长程智能体的「进度双写」
1. 论文把长程任务的进展存放在两个地方：持久化任务状态（what has been achieved）与交互历史（how it was achieved）。
2. 一旦图片已经插入幻灯片、测量值已经写入报告，任务状态本身就已经承载了该进度，后续步骤可以直接检视并修改它；此时再去读「我是怎么把它插进去的」在信息上是冗余的。
3. 这种冗余随执行推进而累积：智能体每一轮都要为「已经被状态编码过的信息」再支付一次 token 与注意力成本，而且历史越长，越容易出现与真实状态不一致的叙述。

二、要解决的核心矛盾：续跑依据应该是「结果」还是「结果的叙述」
1. 现有做法（摘要、检索、剪枝，以及把历史翻译成显式 progress records 与结构化任务状态）确实抑制了上下文增长。
2. 论文的批评是：当这些「重构出来的叙述」替代了可复用的任务结果时，续跑就依赖于一个有损、选择性、且可能与智能体直接检视并修改的环境状态发生漂移的第二手账本。
3. 论文的主张是把上下文组织在可复用任务对象周围——工作产物、来源可溯的发现、在显式条件下仍然可用的应用状态——让智能体通过直接访问对象来恢复信息，而不是读一段关于对象的描述。

三、由此派生的技术难题（论文自述的核心挑战）
1. 折叠后的有效性维护：一旦被覆盖的交互历史被退休，可复用基底如何仍然保持有效？
2. 不能每次 fold 都全量重建表示——那等于把压缩收益又还回去；需要「只修订受影响的记录、其余原样结转」的增量机制。
3. 修订必须有依据：对着 bound evidence 与 applicable environment checks 做，否则载体只是换了一种形式的幻觉来源。
4. 载体与执行上下文的耦合约束：载体已捕获的进度不应在执行上下文里重复，而未解决的目标、约束、未完成工作又必须在 fold 之后仍然显式存在——这是一条「既不重复也不丢失」的切分线，也是方法设计中最容易被做错的地方。

四、隐含假设与适用边界（我的判断）
1. 假设任务进度可以被「外化」为可重新打开、可检视的对象或可写入的记录；对纯推理、纯对话、或以「操作过程本身」为产出的任务，这条假设较弱（合理推断）。
2. 假设证据可绑定、环境检查可执行；Grounder 的检查覆盖率因此成为单点风险，检查不到的地方就是载体悄悄过期的地方。
3. 假设存在合理的 fold 时机与粒度；短任务上运行时的固定开销可能不划算——论文自己的 cost 曲线显示早期是净亏，交叉点在 200 turn 之前（见 Method 图 3(b) 描述）。
4. 这里的隐含立场是「结构化复用优于原始历史、原始历史优于摘要替代」；论文用 CGS 的对照（保留更多 post-fold 文本却更低分）间接支持该立场，但这是单一对照，不足以证明该序关系普遍成立。

五、与「更长窗口」路线的竞争
1. 给定证据中没有出现「直接加长上下文窗口 / 不做压缩」的主对照，这是一个需要回原文确认的评价缺口。
2. 从机制上看，CarryOn 的赌注并非「省 token」本身，而是「续跑所依赖的表示应当与环境中可检视的状态同构」；token 节省只是该同构关系的副产品。

Q2: 有哪些相关研究？

一、交互历史的压缩与替代（summarize / compress）
1. 递归摘要、滚动摘要，以及 CGS 这类「每次 fold 生成完整替换摘要」的做法。论文直接以 CGS 作为主要对照，并报告在共享子集上 CarryOn 的 partial score 高出 5.00%，同时 post-fold 文本还短约 10%。
2. 论文对该线的批评：摘要是选择性的、有损的第二手叙述，会随时间与真实状态漂移。

二、外部历史 / 检索式记忆（external-history methods）
1. 保持活动上下文有界，把早期交互留在 prompt 之外、按需取回（原文引用 Packer et al., 2024；Li et al., 2026）。
2. 与本文差别：检索取回的仍然是「叙述」，而 CarryOn 取回的是「对象/记录 + 使用条件」，并且附带了有效性校验。

三、剪枝与截断（pruning / truncation）
1. 按相关性、新旧或重要性丢弃历史片段（原文引用 Xiao et al., 2026；Chen et al., 2026b；Kang et al., 2025；Xu et al., 2025 等）。
2. 与本文差别：剪枝是不可逆的信息丢弃；CarryOn 试图先把「仍然有效的信息」提升为可验证的载体，再退休历史，理论上把丢弃变成「有损但有据的抽象」。

四、显式进度记录 / 结构化任务状态（最接近的一条线）
1. 把历史翻译成 progress records 或结构化 task state（原文引用 Wu et al., 2026a；Chen et al., 2026a；Wang et al., 2024）。
2. 与本文差别：本文加入对象化、稳定身份、复用条件、后继版本与增量修订协议，并明确区分载体与执行上下文的分工。

五、压缩结果的有效性校验
1. 已有工作对压缩后的表示做校验（原文提及 Sun et al., 2025 一类做法）。
2. CarryOn 把校验内嵌进 fold 流程：Tracker 绑定证据、Grounder 做环境检查、通过验证后才替换覆盖的历史。

六、任务与评测语境
1. OSWorld 2.0（GUI/OS 操作类长程任务）与 BrowseComp-Plus（深度检索/浏览类任务），都属于「环境中存在可检视状态」的长程设定——这既是方法适用面，也是评测选择带来的外部效度限制。
2. 思想上与 object-centric / state-centric agent、记忆分层（working memory vs long-term memory）、上下文工程（context engineering）同源，但本文把「对象」落到了运行时的数据结构与更新协议上，而不只是提示词层面的隐喻。

七、证据缺口说明
1. 给定证据中的 Related Work 与参考文献被截断，大量条目是 2026 年的引用，我无法逐一核实每个系统的具体机制与差异。
2. 上述归类以原文行内引用的位置为准（哪一句引了哪一类工作），具体逐篇对照需回原文 Related Work 章节。

Q3: 论文如何解决这个问题？

一、总体框架（Figure 2 的运行时循环）
1. 与智能体自身的交互循环并行运行三个角色：Tracker 识别候选的可复用进度，并将其绑定到支持证据；Grounder 在适用时对该进度做环境校验；Compiler 联合更新 continuation carriers 与 execution context。
2. 通过验证后，一次 fold 用更新后的上下文视图替换被覆盖的交互历史；此后的新交互构成新的 live tail。

二、上下文视图三分（3.2 节的核心抽象）
1. Continuation carriers：暴露可复用进度给后续执行。每条载体具有稳定 identity，记录「可复用内容或其位置」，并附带复用条件（conditions for reuse）。修订时在同一 identity 下产生后继版本（successor versions），未受影响的载体保留原记录。
2. Execution context：保存载体未捕获的续跑信息——目标、约束、未完成事项等。
3. Live tail：保留最近一次 fold 之后的原始交互，避免「刚发生的事」也要经过抽象才能被看见。

三、载体的三种形态
1. External objects：独立存在于环境中、可被重新打开或修改的工作产物（代码、文档、表格、演示文稿等）；载体记录的是「内容或其位置 + 使用条件」，底层对象仍留在环境里。
2. Information products：以来源为根据的发现（source-grounded findings），直接存进载体记录本身。
3. Application states：在显式条件下仍然可用的应用/界面状态。
（校准：external objects 与 information products 在证据中有直接文本支持；application states 来自 Introduction 的表述，三者并列是我的合理归并，原文是否严格三分需回 3.2 节确认。）

四、增量更新协议（论文强调的核心机制）
1. 只修订「受影响的记录」，依据 bound evidence 与 applicable environment checks 执行；其余记录结转（carry forward），从而避免每次 fold 全量重建表示。
2. 修订产生同一 identity 下的后继版本，保持「同一条进度线索」的可追溯性，而不是让同一条线索在不同 fold 里变成多条互不相认的记录。
3. 联合更新执行上下文：载体已捕获的进度不重复写入执行上下文；未解决的目标、约束与未完成工作必须在 fold 之后仍然显式存在（原文明确把这条称为二者「耦合」的关系）。
4. Compiler 接收一个「准备好的输入包 U_t」（证据片段出现该符号），其具体字段在给定证据中未展开。
5. 验证纪律：对真实冲突、缺失的必需信息、未知结果保留有针对性的检查，不因为要省成本就关闭检查——证据中这段以规则式语句出现，属于对 Compiler/Grounder 行为的约束，具体归属需回原文确认。

五、一个重要的设计对照：原则 vs 运行时
1. 论文实现了一个只保留「围绕可复用进度组织上下文」这一原则、但用重复摘要方式实现的对照。
2. 结果：该原则本身已优于通用压缩，而完整 CarryOn 运行时又进一步优于该对照——说明增量载体维护这套机制有独立贡献，不是「换个说法讲摘要」。

六、证据缺口
1. fold 的触发条件（token 阈值、轮数、事件驱动还是任务阶段）、Grounder 的检查库如何生成、载体 schema 的具体字段、底层模型与提示词格式，给定证据均未包含。
2. 这些细节直接决定方法的可复现性与迁移成本，属于阅读时必须回原文/附录确认的项。

Q4: 论文做了哪些实验？

一、评测对象与场景
1. OSWorld 2.0：GUI/OS 操作类长程任务，典型特征是环境中存在可重新打开、可检视的外部对象（文件、应用状态），理论上最契合本文的对象化假设。
2. BrowseComp-Plus：深度检索/浏览类任务，产物更多是「信息性发现」而非外部对象，是本文方法的另一个压力测试面。

二、主结果（相对 baseline）
1. OSWorld 2.0：partial score 43.9% → 49.7%（+5.8），总 token 用量下降 51.9%，含运行时开销的 nominal model cost 下降 35.9%。
2. BrowseComp-Plus：accuracy 49% → 56%（+7.0），总 token 用量下降 22.9%，成本同步下降。
3. 摘要层面的表述是「improved gains of 5.8% and 7.0%」，结合结论段可判断分别是 partial score 与 accuracy 的绝对百分点提升（合理推断）。

三、消融实验（共享的 40 任务 OSWorld 2.0 子集，Table 5）
1. 隐藏 execution context：binary 22.50% → 17.50%，partial 55.93% → 44.00%，agent 调用次数变化不大。
2. 隐藏 continuation carriers：partial 降到 44.62%，同时 agent 调用次数 +23.8%、成本 +22.3%。
3. 注意该子集上 CarryOn 的 partial 55.93%，与全量集 43.9%→49.7% 的数值不一致，说明子集难度或任务构成与全集不同——跨表比较数值时必须谨慎（合理推断）。

四、与 CGS 的对照（Table 4）
1. CarryOn 相对 CGS 的 partial score 高出 5.00%（55.93% vs 50.93%），同时 post-fold 文本还短约 10%。
2. 这是论文最关键的一组「反直觉」对照：保留更多文本的摘要式方法反而得分更低。

五、成本与轮次分析（Figure 3(b)）
1. CarryOn 在早期因 context-management 调用而额外增加成本，但随着后续 agent 调用在更小上下文上运行，开销被摊薄；累积成本曲线在 200 turn 之前出现交叉。
2. 这意味着收益依赖任务足够长：短程任务上该运行时可能净亏。

六、载体演化分析
1. 论文比较每次 committed fold 前后载体记录的变化，用以观察载体是如何随执行演进的（具体统计量在给定证据中被截断）。
2. 结合 Discussion 的表述可推断：大部分载体是被结转/小幅修订而非每次重建（合理推断，需回原文表格确认比例）。

七、轨迹级案例
1. 附录 C.2 提供了轨迹级示例，用于展示「没有载体时 agent 如何重做/复现先前结果」。

八、评价设计的缺口
1. 给定证据未说明随机种子与重复次数、方差或置信区间、底层模型身份、OSWorld 2.0 的任务总数与任务类型分布。
2. 未见「不做任何压缩、直接长窗口」的对照，也未见到与检索式记忆在等 token 预算下的受控比较。
3. 主指标以 partial score 为主，binary/all-or-nothing 成功率的提升在证据中不明显（见 experimental_observations）。

Q5: 发现了什么实验现象？

一、最重要的一条：两个组件不可互相替代，且分工不对称
1. 去掉 execution context：binary 从 22.50% 掉到 17.50%（相对下降约 22%），partial 从 55.93% 掉到 44.00%（相对下降约 21%），但 agent 调用次数几乎不变。
2. 去掉 continuation carriers：binary 基本守住，partial 降到 44.62%，但 agent 调用 +23.8%、成本 +22.3%。
3. 解读：载体买到的是效率（不必重做已完成的步骤），执行上下文买到的是「还差什么」的正确性（否则 agent 会稳定地做出一个不完整的结果且不自知）。这一非对称性是本文最值得记住的实验现象：上下文管理失效的两种模式——「做得快但做不完」与「知道要做什么但反复重做」——需要不同的修复手段。

二、反直觉结果：保留更多文本 ≠ 更好
1. CGS 在每次 fold 生成完整替换摘要，post-fold 文本比 CarryOn 长约 10%，但 partial 反而低 5.00 个百分点（50.93% vs 55.93%）。
2. 这说明中介变量不是「残留文本量」，而是「残留信息是否以可复用、可验证的形式存在」。对「压缩率—性能」的直觉曲线是一个反例。

三、原则本身有增益，但运行时机制另有独立增益
1. 仅保留「围绕可复用进度组织上下文」的原则、用重复摘要实现，就已经优于通用压缩。
2. 完整 CarryOn 又优于该原则级对照，说明增量修订与载体身份维护不是装饰性工程。

四、token 与 cost 的降幅不对称
1. OSWorld 上 token 降 51.9%，但含运行时开销的 nominal cost 只降 35.9%，中间约 16 个百分点的差额被 context-management 调用吃掉。
2. 这与 Figure 3(b) 的早期净亏、200 turn 前交叉一致：CarryOn 的经济性是「规模经济」式的，需要在长任务上摊薄固定开销。

五、两个 benchmark 的节省幅度不同（−51.9% vs −22.9%），而准确率提升相近（+5.8 vs +7.0）
1. 合理推断：GUI/OS 任务中大量进度可外化为外部对象（指针式记录，卸载率高）；浏览/检索任务的产物多为文本发现，压缩空间更小。
2. 这提示方法收益与「任务状态可外化程度」强相关，是一个可检验的边界条件。

六、指标之间的张力：增益主要落在 partial credit
1. 在 40 任务子集上，CarryOn 的 binary 为 22.50%，与 baseline 持平（证据原文表述为 matching the baseline binary score of 22.50%），而 partial 提升到 55.93%。
2. 也就是说，本文的收益更多是「把任务推进得更远」，而不是「把任务真正做完」。这是一条需要在原文表格中确认、但对结论影响很大的观察；若成立，则「长程智能体的完成率瓶颈可能不在上下文管理，而在任务最后阶段的可验证性/收尾能力」。

七、载体演化的定性趋势
1. 论文跟踪每次 committed fold 前后的载体记录，用以说明「多数记录被结转、少数被修订」（合理推断，具体比例证据中缺失）。
2. 若该趋势成立，它支持了「增量维护可行性」这一核心工程假设——即不必全量重建。

八、失败模式（来自案例与机制推断）
1. 无载体时的典型失败：agent 重新打开文件、重新读取、重新生成已经存在的产物（对应调用次数与成本上升）。
2. 无执行上下文时的典型失败：产物看似完整但遗漏约束或未完成项，且二进制判定被卡住。
3. 潜在的第三种失败（论文证据未覆盖，属推测）：载体过期但仍被信任，导致自信地基于旧条件继续执行。

Q6: 有什么可以进一步探索的点？

一、fold 策略的学习与理论化
1. 何时 fold、fold 掉什么、保留多长的 live tail，目前更像工程超参；可以学习一个策略来权衡「上下文成本」与「程序性知识的丢失风险」。
2. 值得研究的反例：某些历史的价值恰恰在「怎么做的」而非「做出了什么」——例如一次调试成功后，后续需要复现同样的操作序列、或需要知道哪个参数组合失败过。折叠这类历史是高风险的，可以显式建模为「程序性载体」。

二、载体形态的扩展
1. 除外部对象、信息性发现、应用状态之外，可考虑：失败与负结果（「不要重试 X」）、工具可用性知识、用户偏好、假设与待验证命题。
2. 每类载体的「复用条件」语义不同，需要一个可组合的 schema 与版本语义。

三、有效性维护的形式化
1. 载体之间的依赖图、证据的新鲜度（staleness）、环境漂移检测、冲突消解策略，都是本文只给出工程解的部分。
2. 与数据库的增量视图维护（incremental view maintenance）、缓存一致性协议、MVCC 的思想高度同构：可以借用「视图—基表」的一致性模型来定义「上下文视图相对环境状态的一致性」。

四、Grounder 的成本—覆盖权衡
1. 环境检查的自动生成、检查选择的成本感知调度、false-accept 与 false-reject 的非对称代价，都是可做的。
2. 一个具体问题：当检查不可执行（无 API、无 UI 探针）时，载体应当降级为「待验证」还是直接不写入？

五、评测指标的细化
1. 建议加入：每 token 进度、冗余动作率、fold 后的恢复成本、载体过期率、首次错误发生的 turn。
2. 需要设计「状态不可外化」的任务族（纯推理、长对话、过程中间即产物）来测边界。
3. 需要对抗性设定：环境在 agent 背后被修改（drift），观察载体是否会导致自信的错误续跑。

六、跨会话与多智能体
1. 载体天然适合跨会话持久化与在多个 agent 之间共享，但随之而来的是权限、冲突合并与来源可信度问题。
2. 与多智能体协作结合时，「谁有权修订哪条载体」是一个新的协议设计问题。

七、与学习方法的结合
1. 把「写载体」当作可训练技能（RL 或蒸馏），可能比提示词规则更稳。
2. 反过来，载体可以作为训练数据的结构化标注，用于教模型区分「结果」与「过程叙述」。

八、对 AI-for-science 的迁移（与用户画像相关）
1. 长程实验流水线（数据准备—运行—分析—写作）中，产物天然是文件与数据集，契合 external objects 形态。
2. 但科学工作的关键中间产物往往包含「否定性结论」与「不可复现的偶然」，这类信息在本文的载体分类中位置不明，是一个明确的可探索缺口。

Q7: 总结一下论文的主要内容

一、论文要解决的问题
长程智能体把进展存放在两处：持久任务状态（已经达成了什么）与交互历史（是怎么达成的）。论文指出，一旦任务状态已经承载了某项进展（图片已插入幻灯片、测量值已写入报告），再让智能体反复处理「这项进展是怎么做出来的」在信息上就是冗余的，而且这种冗余随步数增长。现有方法通过摘要、检索、剪枝来抑制历史增长，或者把历史翻译成显式进度记录与结构化任务状态；这些做法确实压缩了上下文，但当它们「替代」了可复用的任务结果时，续跑就建立在一个有损、选择性、可能与真实环境状态漂移的第二手叙述之上。论文的核心提问是：长程上下文能否围绕可复用的进展、而不是产生进展的历史来组织？

二、方法主线
CarryOn 是一个面向对象的上下文运行时。它把 agent 的活动上下文拆成三部分：continuation carriers（承载可复用任务对象，具有稳定 identity、记录内容或其位置、附带复用条件）、execution context（承载载体未覆盖的补充续跑信息，如目标、约束、未完成事项）、live tail（最近一次 fold 之后的原始交互）。运行时与 agent 的交互循环并行：Tracker 识别候选的可复用进度并绑定支持证据；Grounder 在适用时对进度做环境校验；Compiler 联合更新载体与执行上下文；验证通过后，一次 fold 用更新后的视图替换被覆盖的交互历史，后续交互构成新的 live tail。
方法的真正难点不在抽象本身，而在「折叠之后如何维持有效性」：论文明确拒绝每次 fold 全量重建表示，改为跨 fold 维护载体记录——只对受影响的记录依据 bound evidence 与 applicable environment checks 做修订，未受影响的记录结转，修订在同一 identity 下产生后继版本；同时联合更新执行上下文，保证「载体已捕获的进度不重复」「未解决的目标与约束仍然显式」。论文还内置了一条验证纪律：对真实冲突、缺失必需信息、未知结果保留有针对性的检查，不因为省成本就关闭检查。

三、实验主线
评测选在两个长程场景：OSWorld 2.0（GUI/OS 操作，环境中有可检视的外部对象）与 BrowseComp-Plus（深度检索/浏览，产物多为信息性发现）。主结果：OSWorld 2.0 上 partial score 43.9%→49.7%（+5.8），总 token −51.9%，含运行时开销的 nominal cost −35.9%；BrowseComp-Plus 上 accuracy 49%→56%（+7.0），token −22.9%。
消融在共享的 40 任务 OSWorld 2.0 子集上进行：隐藏 execution context 使 binary 22.50%→17.50%、partial 55.93%→44.00%，而调用次数几乎不变；隐藏 continuation carriers 使 partial 降到 44.62%，同时调用次数 +23.8%、成本 +22.3%。与 CGS 的对照显示，CarryOn 的 partial 高出 5.00%（55.93% vs 50.93%），而 post-fold 文本还短约 10%。论文还比较了一个「只保留原则、用重复摘要实现」的变体，发现原则本身优于通用压缩、完整运行时又优于该变体。成本分析（Figure 3(b)）显示 CarryOn 早期因管理调用而净亏，累积成本曲线在 200 turn 前交叉。

四、结论与作者自述的贡献
作者把贡献落在三点：以可复用任务对象而非历史压缩物组织长程上下文；给出载体与执行上下文联合、增量、带证据与环境校验的 fold 机制；在两个长程 benchmark 上同时取得更好的任务指标与更低的 token/成本。结论段强调「任务对象承载已确立的进展，显式上下文随仍然需要的东西演化」。

五、我的判断与证据校准
1. 论文最有价值的洞察不是压缩率，而是「上下文的表示应当与环境状态同构」，以及由此派生的「载体买效率、执行上下文买完成度」的非对称消融结论。
2. 需要警惕的一处指标张力：在 40 任务子集上，CarryOn 的 binary 与 baseline 同为 22.50%（证据原文为 matching the baseline binary score），提升主要体现在 partial credit——即「推进得更远」而非「真正做完」。这一点若在原文表格中成立，会显著改变对方法实际价值的判断。
3. token 与 cost 降幅不对称（−51.9% vs −35.9%）说明运行时自身开销可观，方法的适用面被「任务足够长」这一条件约束。
4. 给定证据缺少随机种子、方差、底层模型、任务总数与「不压缩的长窗口」对照，重复性与外部效度无法从现有材料判断。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：直接命中用户主方向 agent：本文是 agent 长期运行时的上下文/记忆基础设施，可迁移到任何数十至数百轮的工具调用型 agent。

## 基本信息

- 作者：Yuchen Lu, Julian McAuley, Lei Zhang
- 机构：合理推断：加州大学圣地亚哥分校（UCSD，作者 Julian McAuley 的长期所属）与南京大学（项目页域名 ethannju.github.io 中的「nju」线索）；论文元数据 institution 字段为空、PDF 证据未含作者单位段落，上述归属需以原文首页核对，不应作为确定事实引用。
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-13
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18784v2`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成以 PDF 语义检索命中的 Introduction / Method / Discussion 片段为主要依据，并结合 heuristic_draft 做了补全；需注意 heuristic_draft 存在字段错位（key_results 落入参考文献条目、main_contributions 混入实验结论），已按 field_evidence_map 的证据锚点纠正，未在证据中出现的数值、模型与超参一律标注为缺口。
