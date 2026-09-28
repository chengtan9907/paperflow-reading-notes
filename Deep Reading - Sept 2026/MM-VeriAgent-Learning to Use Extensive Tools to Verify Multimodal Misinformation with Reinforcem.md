---
user_id: "cheng tan"
paper_id: 13861
arxiv_id: "2609.30698"
title: "MM-VeriAgent: Learning to Use Extensive Tools to Verify Multimodal Misinformation with Reinforcement Learning"
publish_date: "2026-09-28"
pdf_url: "https://arxiv.org/pdf/2609.30698"
abs_url: "https://arxiv.org/abs/2609.30698"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-29T00:30:43"
---
# MM-VeriAgent: Learning to Use Extensive Tools to Verify Multimodal Misinformation with Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal misinformation detection · tool-augmented agent · reinforcement learning · large vision-language model

## 一句话总结

论文提出 MM-VeriAgent：先在文本、视觉、跨模态三类伪造分析子任务上做候选模型基准评测，择优封装成统一接口的可调用工具集 MM-VeriTools，再用强化学习训练 LVLM agent 学会针对混合来源多模态虚假信息自适应调用工具，并以 Tool-Execution Cache 预执行并复用工具输出以压低 RL 训练中的在线工具执行开销，最终在 MMFakeBench 上相对基座模型取得明显准确率提升，且推理阶段无需显式工具搜索。

## 摘要

> Real-world multimodal misinformation often involves mixed forgery sources, requiring sample-specific detection strategies. Existing tool-augmented methods rely on predefined workflows or inference-time planning, limiting adaptability or increasing inference cost. To address this issue, we introduce \textbf{MM-VeriAgent}, which learns to verify mixed-source multimodal misinformation with tools. We first build \textbf{MM-VeriTools}, a specialized toolkit for misinformation detection agents. By benchmarking various candidate models and methods on the sub-tasks required by mixed-source detection, we select the strongest for textual, visual, and cross-modal forgery analysis and encapsulate them as callable tools with a unified interface. On top of this toolkit, we train the LVLM agent with reinforcement learning to teach it how to use these tools to better solve mixed-source detection. Since many of the tools are specialized models whose online execution at every rollout severely limits RL efficiency, we further introduce \textbf{Tool-Execution Cache}, which pre-executes candidate tool calls and reuses their cached outputs during training. This preserves multi-step rollouts while reducing online tool execution, largely improving the training efficiency.Experiments on MMFakeBench demonstrate substantial accuracy gains over the base model without explicit tool search at inference time. Ablation and efficiency analyses further validate the learned tool-use policy and show that Tool-Execution Cache reduces online tool executions during training.

Q1: 这篇论文试图解决什么问题？

1. 任务层面的问题：真实场景中的多模态虚假信息不是单源伪造，而是混合来源（mixed forgery sources）——同一条图文样本可能同时叠加文本侧编造、图像侧篡改/生成、图文之间的跨模态语义错配等多种伪造痕迹。论文据此推出一个关键判断：不存在对所有样本都最优的固定检测流程，检测策略必须是 sample-specific 的。这个判断是全文的立论前提，也决定了「工具选择本身」成为需要被学习/被决策的对象，而不是被预先写死的常量。

2. 现有方法路线的两难（作者明确指出的研究缺口）：
（a）预定义工作流路线（predefined workflows）：把若干工具串成静态流水线，所有样本走同一条路径。优点是推理成本可预测；缺点是缺乏自适应性——当样本的伪造来源组合与流水线设计假设不匹配时，多余的调用浪费成本、缺失的调用直接导致漏检。
（b）推理时规划路线（inference-time planning）：让 LLM/LVLM 在测试阶段临场决定调用哪些工具、按什么顺序调用。优点是自适应；缺点是增加推理成本（每一步规划都要额外的前向计算与工具往返），且规划质量完全依赖基座模型的即兴决策能力，缺少针对该任务分布的监督或优化信号。

3. 由此抽象出的核心研究问题：能否把「如何用工具」从推理时的即时决策，转化为训练时习得的策略（learned tool-use policy），从而同时拿到「自适应性」与「推理时零规划开销」两端的好处？这正是论文标题 Learning to Use Extensive Tools 的落点。

4. 第二个、工程性质但同样是瓶颈的问题：工具集里大多是专用模型（文本/视觉/跨模态伪造分析模型），每次 RL rollout 若要在线执行这些模型，多步 rollout × 大规模采样会让训练在算力/时延上不可行。因此训练效率不是附带优化，而是让「工具增强 RL」这条路走通的前提条件。

5. 隐含假设与需要回原文核实的点（本次仅有摘要，无法确认）：
（a）「按子任务选最优单模型，再组合成工具集」是否等价于「组合后的系统在混合来源场景下最优」——子任务级最优与端到端最优之间存在组合误差，论文摘要未给出端到端工具集消融细节。
（b）Tool-Execution Cache 的有效性依赖工具输出的可复用性：对确定性工具成立，若工具含随机采样或依赖外部实时状态（如在线检索），缓存与在线执行结果可能不一致，摘要未说明这一点如何处理。
（c）缓存需要预先枚举候选工具调用，候选调用空间的规模、缓存命中率与内存开销在摘要中均未量化。
（d）评测仅提到 MMFakeBench，该基准对真实世界混合来源分布的覆盖度、以及是否含时序漂移的新式伪造手法，需回原文与基准论文核对。

Q2: 有哪些相关研究？

说明：本次未获得 PDF 正文，无法读到论文的 Related Work 章节；以下按「摘要明确点名的对照」与「从选题可推断的研究脉络」两级组织，后者已标注为推断，请以原文为准。

1. 摘要明确对照的既有路线（论文自述）：
（a）预定义工作流的工具增强方法（predefined workflows）：把检测工具编排成固定流程，牺牲样本级自适应性。
（b）推理时规划的工具增强方法（inference-time planning）：在测试时动态决定工具调用，增加推理成本。
论文的定位是同时绕开这两类限制。

2. 评测基准：MMFakeBench，被用作本文的主实验平台。该基准的定位（依据名称与论文语境推断）是面向 LVLM 的混合来源多模态虚假信息检测基准，这也解释了为什么论文强调 mixed-source 与 sample-specific。

3. 多模态虚假信息检测的研究脉络（外部背景，非论文原文）：通常包含图文不一致检测、图像篡改定位/合成图像检测、文本谣言与事实核查、跨模态证据对齐等子问题；本文的 MM-VeriTools 正是把这些子能力抽象成「文本伪造分析 / 视觉伪造分析 / 跨模态伪造分析」三类工具（推断其子任务划分与论文的 benchmark 表格一一对应）。

4. 工具增强的语言/视觉语言 agent 脉络（外部背景，合理推断）：从 ReAct 式「推理—行动」循环，到多 agent 编排与固定 pipeline，再到让模型自行规划工具序列；本文属于「把工具使用变成可训练策略」的这一支。

5. 用 RL 训练 LLM/LVLM 使用工具的脉络（合理推断）：可验证奖励强化学习（RLVR）、面向搜索/代码执行等外部环境的 agentic RL 工作。论文把「工具调用」视为可优化行为，且用最终检测正确性作为（推测）奖励信号，与这一脉一致。

6. RL 训练中的环境/工具执行效率脉络（推断）：异步 rollout、模拟环境、环境缓存、离线预执行等。Tool-Execution Cache 明显属于这一支，其差异点在于缓存对象是「专用检测模型的输出」而非通用 API 响应。

7. 本文与上述脉络的差异点总结：相对预定义 workflow，本文让策略可学习；相对 inference-time planning，本文把规划前移到训练阶段，换取推理时的低开销；相对通用 agentic RL，本文处理的是「工具本身是重量级专用模型、且工具输出具有可缓存性」这一特殊设定，并为此专门设计了训练期加速机制。

Q3: 论文如何解决这个问题？

论文的技术方案由三个耦合组件构成，摘要给出了每个组件的功能定位，但内部细节（模型清单、RL 算法、奖励设计、缓存实现）需回原文确认。

1. MM-VeriTools（工具集构建，面向「工具从哪来」）：
（a）拆解子任务：先界定混合来源检测所需的能力维度——文本伪造分析、视觉伪造分析、跨模态伪造分析（这是摘要明确列出的三类）。
（b）基准评测候选方案：在每一类子任务上对多个候选模型与方法做 benchmarking，而非直接采用现成模型。这是一个「先评测、再入选」的工程化选型流程，其价值在于把工具集的质量问题转化为可复核的对比实验。
（c）统一接口封装：把每个子任务的胜出方案封装为具备统一调用接口的工具，使上层 agent 无需关心底层模型异构性。这一层抽象的代价是：统一接口会掩盖各工具的输入输出格式差异与不确定性差异，具体如何处理（如是否统一为文本描述、是否返回结构化置信度）摘要未说明。

2. MM-VeriAgent（策略学习，面向「怎么用工具」）：
（a）在 MM-VeriTools 之上，用强化学习训练一个 LVLM agent，目标是学会在混合来源检测任务中调用哪些工具、以何种顺序调用。
（b）与 inference-time planning 的关键区别：规划能力被内化进策略参数，因此推理阶段无需显式的工具搜索（without explicit tool search at inference time），避免测试时额外的规划开销。
（c）摘要未给出 RL 算法（PPO/GRPO 等）、奖励构成（是否含工具调用格式奖励、调用次数惩罚、最终判断正确性奖励）、rollout 步数上限与训练数据规模，这些是复现与判断方法强度的关键，必须回原文核实。

3. Tool-Execution Cache（训练效率，面向「训练跑不跑得动」）：
（a）动机：工具多为专用模型，若每个 rollout 每一步都在线执行，训练成本不可接受。
（b）机制：预先执行候选工具调用并缓存其输出，训练时直接复用缓存，从而在保留多步 rollout 结构的前提下减少在线工具执行次数。摘要强调「largely improving the training efficiency（大幅提升训练效率）」。
（c）需要核实的设计点：缓存键如何定义（输入样本 + 工具 + 参数？）、预执行的候选集合如何枚举（是否穷举、是否按启发式剪枝）、缓存命中率与内存占用、工具输出具随机性时缓存是否会引入偏差、以及缓存策略是否会使学到的策略偏向高频缓存的调用路径（可能形成隐性偏差）。这些权衡摘要完全未涉及，属于阅读时的重点追问项。

Q4: 论文做了哪些实验？

摘要给出的实验信息有限，仅能确认以下骨架，具体数值、基线清单与实现细节均无法从现有证据确认。

1. 主实验平台：MMFakeBench（混合来源多模态虚假信息检测基准）。

2. 主结果声明：相对基座模型（base model）取得大幅准确率提升（substantial accuracy gains），且这一提升是在推理阶段不做显式工具搜索（without explicit tool search at inference time）的条件下取得的。这一点是实验设计上最关键的对齐——它把「策略是否真的内化了工具使用」变成可被结果直接支持的主张。

3. 消融实验（ablation）：用于验证学到的工具使用策略（learned tool-use policy）确实在起作用，而不只是工具集本身带来增益。通常这类消融会对比：无工具、随机/固定工具调用、训练后的策略调用，以及去掉 RL 只用基座模型等设置；论文摘要只说明「验证了学到的工具使用策略」，具体消融维度需查原文。

4. 效率分析（efficiency analysis）：用于证明 Tool-Execution Cache 减少了训练期的在线工具执行次数。摘要给出的结论方向是「reduces online tool executions during training」，但没有给出加速比、执行次数下降幅度、是否影响最终精度（即缓存引入是否带来精度损失）等量化信息。

5. 摘要未提及但读者需要核对的实验要素：基座 LVLM 的型号与规模、是否与闭源模型对比、MM-VeriTools 中三个子任务各自入选的具体模型、评测指标是否只有准确率（是否有 F1、按伪造来源类型的分组指标）、训练/测试是否同分布、是否存在 benchmark 训练集泄漏风险、以及推理时延与调用工具数的对比。

Q5: 发现了什么实验现象？

以下为摘要可支持的实验现象，以及需要进一步核实的趋势点：

1. 主要现象：加入「学到的工具使用策略」后，在 MMFakeBench 上相对基座模型出现显著准确率提升，且这一提升不依赖推理阶段的显式工具搜索。这暗示一个值得注意的趋势：工具使用的收益可以被「训练进参数」，而不必以推理时算力为代价换取——对 agent 类系统而言这是重要的成本结构证据（合理推断，具体幅度未给出）。

2. 消融揭示的因果归属：消融实验被用来验证「学习到的工具使用策略」的有效性。这意味着作者预期（并有实验支持）增益不是单纯来自工具集本身的接入，而是来自策略学会了何时调用哪个工具；如果原文消融包含「工具集 + 随机/固定调用」的对照，那么该对照与完整策略之间的差距就是策略学习的净收益，这是阅读时最值得抓取的数字。

3. 效率现象：Tool-Execution Cache 减少了训练期间的在线工具执行次数。这是一个训练侧现象，而非推理侧现象；摘要措辞明确限定在 during training，读者不宜把它误读为推理加速。同时需要追问：减少执行次数是否伴随精度下降（负结果风险），以及缓存是否对某些工具（随机性工具）不适用。

4. 指标间的张力（需回原文确认是否被讨论）：自适应性通常以更多工具调用为代价，而本文主张在推理时不做显式搜索即可保持自适应性；因此「准确率提升」与「推理调用次数/时延」之间的权衡曲线，是判断该主张强度的关键证据，摘要未提供该曲线。

5. 失败模式与反例：摘要未报告任何失败案例、误检类型分析或跨来源类型的误差分布。混合来源场景下，最可能的失败模式是「多来源叠加时策略只触发单一工具」「缓存导致调用路径同质化」以及「工具相互矛盾时无仲裁机制」；这些属于合理推断的待验证风险，不能当作论文已证实的结论。

Q6: 有什么可以进一步探索的点？

基于摘要可识别的延伸空间（部分为合理推断，需结合原文确认作者是否已讨论）：

1. 工具集的动态扩展：当前 MM-VeriTools 是离线选优后固定的工具集，未来可研究工具的动态发现与即插即用（新出现伪造手法时如何增量纳入新工具而不重训整个策略）。
2. 工具输出的仲裁与验证：多个专用工具给出冲突判断时，agent 是否需要额外的 verifier 或证据加权机制；这在混合来源样本上尤其关键。
3. 缓存机制的理论与边界：把 Tool-Execution Cache 推广到随机性工具、带时间戳的在线检索工具，研究缓存一致性、命中率—精度权衡，以及缓存引入的策略偏差（策略可能偏向被缓存覆盖的调用路径）。
4. RL 算法与奖励设计：摘要未说明算法与奖励，后续可比较不同 RL 算法、不同奖励塑形（工具调用数惩罚、格式奖励、过程奖励 vs 结果奖励）对策略效率与鲁棒性的影响。
5. 推理成本的显式优化：在「无需显式工具搜索」基础上进一步研究推理时的工具调用预算约束（budget-aware policy）、提前退出与难度自适应调用。
6. 跨基准与跨域泛化：从 MMFakeBench 扩展到其他多模态虚假信息基准、跨语言（中文/多语种）与真实社交平台数据，检验策略的分布外迁移能力。
7. 对抗鲁棒性：研究针对该 agent 的自适应攻击——攻击者若知道工具集构成，可构造同时规避文本、视觉、跨模态三类工具的样本；这是工具增强检测系统的结构性弱点，值得专门研究。
8. 策略可解释性与证据产出：让 agent 不仅给出真假判断，还输出可核验的证据链与工具调用轨迹，服务于人工复核与事实核查工作流。
9. 能力蒸馏与轻量化：把学到的工具使用策略蒸馏进更小的 LVLM，或研究无工具环境下的策略迁移，以降低部署成本。
10. 与生成方向的交叉：伪造生成与伪造检测存在共演化关系，可研究用生成模型做红队数据合成来持续训练检测 agent（与用户画像中的 generation 方向存在潜在交集，属推断）。

Q7: 总结一下论文的主要内容

本文的论证主线可拆为四段。

（1）问题设定。真实世界的多模态虚假信息往往不是单一伪造来源，而是混合来源：同一条图文样本可能同时叠加文本侧编造、图像侧篡改或生成、以及图文之间的跨模态不一致。作者由此提出核心判断——不存在对所有样本都最优的固定检测流程，检测策略必须是随样本而变的（sample-specific）。

（2）对现有路线的批评，也是本文的研究缺口。工具增强的检测方法大致分两类：一类依赖预定义工作流，所有样本走同一条固定流水线，鲁棒但缺乏自适应性；另一类在推理时做规划，由模型临场决定调用哪些工具，自适应但引入额外推理成本，且规划质量完全依赖基座模型的即时决策。论文要同时避开这两端。

（3）技术主线，由三个组件构成。第一，MM-VeriTools：作者先把混合来源检测拆成文本伪造分析、视觉伪造分析、跨模态伪造分析三类子任务，在每个子任务上对多个候选模型与方法做基准评测，选出最强方案，并封装为具有统一接口的可调用工具。这一步把「工具从哪来、凭什么选它」从直觉决策变成有评测依据的工程流程。第二，MM-VeriAgent：在工具集之上用强化学习训练一个 LVLM agent，让它学会在求解混合来源检测时调用哪些工具、以什么顺序调用，从而把原本发生在推理时的规划前移为训练时习得的策略（learned tool-use policy）。第三，Tool-Execution Cache：由于工具多为专用模型，若每次 rollout 都在线执行会严重限制 RL 效率；作者提出预先执行候选工具调用并缓存输出、训练时直接复用，从而在保留多步 rollout 结构的同时减少在线工具执行，显著提升训练效率。

（4）实验主线。在 MMFakeBench 上评测，相对基座模型取得大幅准确率提升，并且该提升是在推理阶段不做显式工具搜索的条件下取得的——这是「策略内化工具使用」这一核心主张最直接的结果证据。消融实验用于验证学到的工具使用策略确实在起作用（即增益来自策略学习而非工具集本身），效率分析用于验证 Tool-Execution Cache 确实减少了训练期的在线工具执行次数。

需要明确指出的信息缺口：本次仅获得摘要，无 PDF 正文。因此 RL 算法的具体选择（PPO/GRPO 或其他）、奖励函数构成、基座 LVLM 的型号与规模、MM-VeriTools 三个子任务各自入选的模型、缓存键定义与命中率、训练数据规模与 rollout 步数、MMFakeBench 上的具体数值、对比基线清单、推理时延对比，均无法确认，必须回原文核对。此外，元数据中的发表日期（2026-09-28）与 arXiv 编号（2609.30698）指向一个相对当前时点偏未来的时间，是否为预印本占位或元数据错误需要核实；作者机构在元数据中为空，正文亦未获取，故本报告不给出机构推断。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）直接重合：论文处理的是「LVLM agent 如何学习使用工具」这一核心 agent 问题，且提供了训练侧（RL）与效率侧（缓存）的完整设计。

## 基本信息

- 作者：Peipei Li, Shuhan Xia, Shengyang Liu, Zekun Li, Ran He
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.30698`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未检索到任何 PDF 语义证据（retrieved_evidence 与 field_evidence_map 均为空），全部内容基于标题、摘要与元数据推演，并已在各字段内逐项标注「论文明确陈述」「合理推断」与「推测」；heuristic_draft 的多个字段含 LaTeX 残留或直接截取自摘要、分析价值有限，已由本次输出替换并补全。
