---
user_id: "cheng tan"
paper_id: 13886
arxiv_id: "2609.30906"
title: "ToolSearcher: Optimizing Tool Selection at Scale via Reinforcement Learning"
publish_date: "2026-09-28"
pdf_url: "https://arxiv.org/pdf/2609.30906"
abs_url: "https://arxiv.org/abs/2609.30906"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-29T00:32:00"
---
# ToolSearcher: Optimizing Tool Selection at Scale via Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：tool selection · agentic reinforcement learning · large-scale tool retrieval · multi-turn search

## 一句话总结

ToolSearcher 把「大规模工具选择」定义为 agentic RL 的一个新问题，并用类别约束的工具区分、事件级搜索建模、轨迹对齐的信用分配三项设计，让 LLM agent 在需要多轮迭代搜索与复杂工具组合的场景下更准确地检索并选出目标工具。

## 摘要

> Large language models (LLMs) excel at natural language processing but struggle to interact with external environments. Tool learning provides a promising way to extend LLMs into actionable agents, where tool selection is a critical prerequisite for successful tool use. Existing work often assumes a small or predefined set of tools, leaving large-scale tool selection underexplored. Real-world repositories contain a vast and diverse array of tools, making it difficult for LLMs to effectively search, distinguish, and compose tools under context-length constraints. We identify large-scale tool selection as a new challenge for agentic reinforcement learning, highlighting that existing RL methods for knowledge-based question answering are inadequate for selecting tools while considering compatibility. To address this challenge, we propose ToolSearcher, a novel RL framework for effective multi-turn search and fine-grained optimization in large-scale tool selection. Specifically, we introduce category-constrained tool discrimination to improve the model's ability to distinguish functionally similar tools, event-level search modeling to explicitly optimize the discovery of target tools during multi-turn search, and trajectory-aligned credit allocation to provide fine-grained reward signals for different stages of the search-selection process. Extensive experiments on large-scale tool selection benchmarks demonstrate that ToolSearcher consistently outperforms a set of strong baselines in challenging settings involving iterative search and complex tool composition.

Q1: 这篇论文试图解决什么问题？

1）论文试图解决的问题可拆成三层。第一层是任务设定的缺口：LLM 要成为可执行 agent，必须调用外部工具，而「选哪个工具」是工具使用链条上的前置环节；已有研究普遍在「工具集很小」或「工具集预先给定、只需在候选内排序」的假设下做工具选择，这与真实工具仓库的规模与异质性严重不匹配。第二层是规模带来的操作性困难：工具仓库巨大且多样，模型在上下文长度约束下既无法一次性读入全部工具描述，也难以在候选池中快速定位、区分并组合工具。第三层是方法论缺口：作者认为大规模工具选择对 agentic RL 构成了「新挑战」，面向知识库问答（KBQA）的既有 RL 方法（即把检索当作动作、以最终答案奖励驱动检索行为的范式）不足以迁移过来，因为工具选择额外要求考虑工具之间的兼容性（compatibility）——仅仅检索到「相关」工具并不等于选出的工具组合是可执行、可协同的。

2）由此可以还原出论文隐含的三类子问题：搜索问题（在超大规模工具空间中做多轮、带上下文约束的检索）、区分问题（功能高度相似的工具之间需要细粒度判别，否则检索到「一类」却选错「一个」）、组合问题（多工具协作时需满足参数、能力与流程上的兼容约束）。论文把前两者与「搜索—选择」过程的奖励分配问题合并处理，第三点则以「兼容性」的形式进入 RL 优化目标。

3）论文的隐含假设（需回原文确认）：工具以文本形式描述并可被检索接口访问；工具存在可用的类别标签或类别层次结构（否则 category-constrained 设计无从落地）；兼容性可以被定义或至少可以被评测；多轮搜索的交互成本可接受。这些假设构成了方法的适用边界：若工具描述质量差、类别体系混乱或类别标注缺失，类别约束带来的收益可能反转。

4）失败模式与边界条件（推断）：上下文窗口受限时长轨迹的搜索容易产生累积误差；类别体系与真实功能划分不一致时，类别约束可能把真正正确的工具排除在候选之外；以奖励驱动的搜索策略存在 reward hacking 风险（例如反复触发搜索以刷取事件级奖励）。这些都需要原文的消融与案例分析来验证，摘要未提供。

Q2: 有哪些相关研究？

论文摘要只点名了两条脉络——工具学习/工具选择，以及面向 KBQA 的 RL 检索方法——并未列出具体引用，因此下面按研究线索组织，具体对话对象需回原文的 Related Work 与实验表格核对。

1）工具学习与 LLM agent 的工具使用：这一线索关注如何让 LLM 调用 API、函数或外部服务完成可执行任务，通常把流程拆成工具检索（retrieval）、工具选择（selection）、参数填充（grounding）与执行反馈（execution）。论文的定位是：该线索下的工具选择研究往往在小型或预定义工具集上评估，规模维度被回避了。

2）大规模工具检索与「工具海」问题：真实平台（如各类 API 市场、插件库）工具数量可达数千到数万，常见做法是先做稠密/稀疏检索召回候选子集，再由模型选择。论文的批评点在于：这类流水线把检索与选择割裂，且不显式建模「选出的工具之间是否兼容」。

3）面向知识库问答/开放域问答的 RL 检索（如搜索增强型 RL 范式）：以最终答案正确性为奖励，训练模型自主发起搜索、改写查询、整合证据。论文明确指出其不足：这类方法的奖励信号只对齐「答案对不对」，而工具选择需要对齐「工具能不能用、能不能一起用」，即兼容性维度在 KBQA 范式里没有对应物。

4）多轮交互中的信用分配与过程奖励：长轨迹上稀疏的终局奖励难以训练，常见解法包括轮级/步级奖励、过程奖励模型、层次化 RL、以及对轨迹做优势分解。论文的 event-level search modeling 与 trajectory-aligned credit allocation 正落在这条脉络上，属于把「搜索事件」而不是「整条轨迹」作为优化单元的思路。

5）对比学习/负样本构造用于细粒度区分：类别约束的工具区分在机制上接近「同类难负样本」的判别训练，与该方向以及度量学习相关。

6）局限性提示：以上为基于摘要的重建，论文实际引用的 baseline 列表、是否引用了 AnyTool/ToolBench 类工作与 Search-R1 类 RL 工作，均需以原文为准；不要据此认为论文已经覆盖了这些具体系统。

Q3: 论文如何解决这个问题？

ToolSearcher 是一个 RL 框架，方法论上由三个互相补位的组件构成，摘要给出的机制描述有限，以下在标注处区分「摘要明确支持」与「合理推断」。

1）类别约束的工具区分（category-constrained tool discrimination）——摘要明确支持的目标是「提升模型区分功能相似工具的能力」。合理推断的实现路径是：在同一类别内部构造难负例（把功能相近的工具放在一起让模型判别），使策略在「选对类别」之外还要「选对类别内的具体工具」；「category-constrained」这一命名暗示训练或奖励被限制/组织在类别粒度上，例如先在类别上约束候选范围，再在类内做细粒度判别。若如此，类别既是搜索空间的剪枝结构，也是负样本采样的组织方式。该设计的隐含前提是类别体系可得且与真实功能划分大致一致。

2）事件级搜索建模（event-level search modeling）——摘要明确支持的目标是「在多轮搜索中显式优化目标工具的发现」。合理推断：把「某一步搜索命中了目标工具」视为一个可被单独识别的离散事件（event），在事件层面给予奖励/优势估计，而不是把多轮搜索的成败只归因于轨迹末端的整体奖励。这样做的意义在于：即便最终选择错误，只要搜索阶段确实发现了正确的目标工具，搜索行为仍能获得正向信号，从而缓解长轨迹下的信号稀释。

3）轨迹对齐的信用分配（trajectory-aligned credit allocation）——摘要明确支持的是「为搜索—选择过程的不同阶段提供细粒度奖励信号」。合理推断：把一条完整轨迹切分为「搜索阶段」与「选择/组合阶段」，按阶段分配奖励权重，使不同阶段各自收到与其贡献对齐的信号；「trajectory-aligned」可能还意味着奖励需要与轨迹的实际展开方式（轮数、每轮动作）绑定，避免出现同一结果对应不同行为却拿到相同奖励的错配。

4）三者形成一条完整逻辑：类别约束解决「区分不像」，事件建模解决「搜不到」，信用分配解决「学不动」。共同的前提是存在一个可被多轮调用的搜索接口，且工具有结构化的类别信息。技术上的主要权衡包括：类别约束越强，判别越细但越容易被错误的类别划分所害；事件级奖励越重，搜索行为越积极但越可能诱发无意义的过度搜索；阶段级信用分配需要额外的归因设计，可能引入超参与训练不稳定性。这些权衡在摘要中均未披露，需以原文的消融实验为准。

Q4: 论文做了哪些实验？

摘要只提供了实验的定性描述，没有给出任何数据集名、工具数量级、模型规模、baseline 名称与数值，因此以下区分「摘要支持」与「信息缺口」。

1）实验对象（摘要明确支持）：大规模工具选择基准（large-scale tool selection benchmarks，具体名称未知），评测设置包含两类困难场景——迭代搜索（iterative search）与复杂工具组合（complex tool composition）。

2）对比对象（摘要明确支持）：一组强基线；具体是哪些检索/RL/agent baseline 未知，需回原文核对，重点看是否包含稠密检索＋LLM 选择的流水线式方法、以及 KBQA 风格的检索 RL 方法（后者正是论文批评的对象，若缺失会削弱论点）。

3）结论形态（摘要明确支持）：在困难设置下持续优于强基线；「consistently」暗示跨多个设定或多种规模均有增益，但没有给出任何数值、方差或显著性信息。

4）信息缺口清单（需回原文补齐）：训练使用的 RL 算法（PPO/GRPO/其他）、模型底座与参数量、工具库规模与类别数量、训练/测试工具集的重叠情况、搜索轮数上限、上下文长度设置、评价指标（工具选择准确率、任务成功率、组合成功率、检索召回率等）、是否有执行真实 API 的端到端评估、算力开销与推理延迟。

5）合理推断需要重点核查的实验设计：是否做了工具数量规模的 scaling 实验（如千级/万级工具库下性能曲线）；是否做了三组件的逐项消融；是否有对「兼容性」的单独度量（否则论文的核心论点之一无法被验证）；是否有类别体系错误时的鲁棒性测试。这些若缺失，会显著影响对论文贡献强度的判断。

Q5: 发现了什么实验现象？

由于未获取到实验结果文本与图表，本字段以「摘要支持的结论 + 需在原文核验的典型现象 + 基于方法的合理推测」三层组织，凡属推测均显式标注。

1）摘要明确支持的观察：在涉及迭代搜索与复杂工具组合的困难设置下，ToolSearcher 持续优于强基线。这是唯一的定性结论，未附带任何现象层面的解释，也没有给出反直觉结果或负结果。

2）合理推断的性能趋势：工具库规模越大、工具间功能越相似，基线（尤其是把检索与选择割裂的流水线方法）退化应越明显，而 ToolSearcher 的增益应随规模扩大而拉开；可检验的痕迹是原文是否给出随工具数量变化的性能曲线。若曲线平坦，说明方法受益主要来自多轮搜索而非规模本身。

3）合理推断的消融趋势：三组件大概率是互补关系——去掉类别约束，错误应集中在同类工具之间的混淆；去掉事件级搜索建模，失败应集中在「需要多轮才找到目标工具」的样本上；去掉信用分配，训练应在长轨迹样本上更不稳定或收敛更慢。原文的消融若显示某一项增益接近零，则该组件的必要性需重新评估。

4）推测中的指标间张力值得重点核对：搜索召回率提升不必然带来最终选择准确率提升（搜到了但选错），组合成功率又可能低于单工具选择准确率（选对单个工具但组合不兼容）。若论文只报告单一指标，会掩盖这一张力；这是阅读原文时最值得找的一张表。

5）推测中的失败模式：类别体系划分与真实功能不一致时，类别约束可能把正确工具排除，产生「越约束越差」的反例；事件级奖励可能诱发无意义的多轮搜索（刷事件奖励而不到达正确选择）；这些负结果若原文未报告，应视为该工作的证据缺口而非结论。

6）信息缺口：无任何数值、方差、显著性、成本数据；无法判断增益幅度是「稳健但小幅」还是「显著跃升」，也无法判断推理开销是否可接受。

Q6: 有什么可以进一步探索的点？

1）规模继续扩展与检索结构：从当前基准的规模推向百万级/持续增长的开放工具生态，需要研究分层检索（类别→子类→工具）、可学习的类别体系（而非依赖人工或平台给定标签）、以及增量索引更新下的策略稳定性。

2）类别体系本身的可靠性：category-constrained 设计的成立依赖类别划分正确。可探索的方向包括类别噪声鲁棒训练、类别不确定时的软约束（用分布而非硬过滤）、以及让模型自己归纳功能性类别（功能等价类而非平台元数据类别）。

3）兼容性建模的形式化：论文把「兼容性」作为与 KBQA 范式的关键差异点，但摘要未给出其定义方式。可进一步探索的是：把兼容性建成可学习的结构（工具依赖图、参数/类型约束求解、前置条件—后置条件匹配），以及在 RL 奖励中显式加入兼容性惩罚或约束优化的做法。

4）信用分配的进一步细化：事件级＋阶段级的分配之外，可研究方向包括搜索动作的因果归因（哪一次查询真正贡献了发现）、反事实奖励、以及过程奖励模型在工具选择任务上的迁移，尤其是如何避免事件奖励被刷。

5）动态与有状态环境：真实工具会失效、改版、限流、返回错误，当前静态基准难以覆盖。可探索在带噪声执行反馈、工具下线与版本漂移下的持续学习与遗忘抑制。

6）评测范式：从静态选择准确率走向真实执行闭环（端到端任务成功率、成本与延迟约束下的效用）、多轮人机交互、以及对抗场景（工具描述中含 prompt injection、恶意工具混入）的安全性评估。

7）跨域与个性化迁移：把在大规模通用工具库上学到的搜索策略迁移到垂直领域（如生物信息学工具链、科学计算工作流），这类场景工具描述专业性强、组合约束更硬，是检验方法泛化性的好试验场，也更贴近系统化、可复用的工具编排需求。

8）与多 agent 协作的结合：当多个 agent 各自持有一部分工具视图时，工具选择变成分布式决策问题，可探索共享工具空间下的协同检索与冲突消解。

Q7: 总结一下论文的主要内容

一、问题定位。论文从一个能力落差出发：LLM 在自然语言处理上表现优异，却难以与外部环境交互；工具学习是把 LLM 拓展为可执行 agent 的路径，而工具选择是工具使用成功的前置条件。已有工作通常假设工具集较小或预先定义，因此大规模工具选择长期未被充分研究。真实工具仓库的工具数量庞大且类型多样，在上下文长度约束下，模型很难有效完成搜索、区分与组合三件事。作者据此把大规模工具选择界定为 agentic RL 的一个新挑战，并给出一个关键判断：面向知识库问答（KBQA）的现有 RL 方法不足以胜任，因为这些方法的奖励只对齐「答案正确」，而工具选择还必须考虑工具之间的兼容性。

二、方法主线。论文提出 ToolSearcher，一个用于有效多轮搜索与细粒度优化的 RL 框架，包含三项设计。其一是类别约束的工具区分（category-constrained tool discrimination），目标是提升模型区分功能相似工具的能力——合理推断其做法是在类别粒度上组织候选与难负例，使模型不仅要选对功能类别，还要在类别内部选对具体工具。其二是事件级搜索建模（event-level search modeling），在多轮搜索过程中显式优化对目标工具的发现，把「某一步搜索命中目标工具」当作可单独奖励的事件，而非只依赖轨迹末端的整体成败。其三是轨迹对齐的信用分配（trajectory-aligned credit allocation），为「搜索—选择」过程的不同阶段提供细粒度奖励信号，缓解长轨迹下的奖励稀疏与归因错配。三者的分工可概括为：类别约束解决「区分不开」，事件建模解决「搜不到」，信用分配解决「学不动」。共同前提是存在可多轮调用的搜索接口，且工具具备可用的类别信息。

三、实验主线。论文在大规模工具选择基准上做了实验，评测设置包含迭代搜索与复杂工具组合两类困难场景，对比对象是一组强基线，结论是 ToolSearcher 持续优于这些基线。摘要未披露数据集名称、工具库规模、baseline 列表、指标定义与任何数值，也未提到消融规模、训练算法与模型底座，因此实验的强度与可复现性无法从现有信息判断。

四、论证结构与其薄弱处。论文的论证链条是「规模假设缺口 → 现有 RL（KBQA 范式）不适配 → 三个针对性组件 → 困难设定下的一致增益」。其中「兼容性是关键差异」这一论断最具争议性，也最需要在原文中确认是否有独立的兼容性度量与 ablation 支持；若缺失，则方法贡献主要落在搜索与信用分配上，与既有检索 RL 工作的区分度会减弱。另外，「持续优于强基线」缺少数值，读者无法判断增益幅度与代价（推理轮数、延迟、训练成本）。

五、阅读时需要核对的清单：三组件的逐项消融；工具数量规模的 scaling 曲线；搜索轮数与性能的关系（是否存在过度搜索）；同类工具混淆的案例分析；类别体系错误时的鲁棒性；是否在真实 API 执行闭环上评测；以及基线中是否包含论文所批评的 KBQA 风格 RL 方法，否则核心动机缺乏对照。

六、需要提醒的元数据异常。该条目的 publish_date 为 2026-09-28，arXiv 编号 2609.30906 同样指向 2026 年 9 月，属于未来时间戳；结合本次未获取到 PDF 正文与任何检索证据，建议在使用本笔记前先核验论文真实性与版本。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）直接重合：研究的是 LLM agent 的工具检索与选择，属于 agent 能力栈中的关键环节。

## 基本信息

- 作者：Zhenlong Dai, Xujie Song, Zitong Wang, Tong Niu, Jian liu, Weiqiang Wang, Xiu Tang, Sai Wu, Chang Yao, Jingyuan Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-09-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.30906`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未参考任何 PDF 语义检索证据（retrieved_evidence 与 field_evidence_map 均为空，sections 亦为空），全部内容仅基于标题与摘要推演，方法机制与实验细节均标注为推断或明确缺口；另需注意 publish_date 与 arXiv 编号均指向 2026 年 9 月的未来时间戳，论文真实性待核验。
