---
user_id: "cheng tan"
paper_id: 22290
arxiv_id: "2611.23588v1"
title: "ARISE: Practical Self-Evolving Memory for Accurate and Cost-Efficient Enterprise Agents"
publish_date: "2026-11-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.23588v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.23588v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-21T15:17:23"
---
# ARISE: Practical Self-Evolving Memory for Accurate and Cost-Efficient Enterprise Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：enterprise agents · agent memory · self-evolving agents · failure attribution

## 一句话总结

ARISE 提出一种不更新模型权重、工具或工作流逻辑的企业智能体自演化记忆架构：用部署模型自身或教师模型对失败做归因并生成修正，将修正保存为可检索的微观记忆、 consolidate 为宏观规则，且仅在留出集准确率提升时接纳规则更新，在 14 个企业流程域上以约持平成本取得显著准确率提升并优于 GEPA。

## 摘要

> Enterprise LLM agents operate in dynamic environments where foundation models evolve rapidly, workflows update frequently, and deployments need to satisfy accuracy and cost requirements. We propose ARISE (Agent Refinement through Iterative Self-Evaluation), a practical memory architecture in which the agent's memory evolves from its own failures without updating model weights, tools, or workflow logic. ARISE uses the model itself or a teacher model to attribute the failures and propose corrections. Then, ARISE keeps the corrections as retrievable micro memory, consolidates them into macro rules, and accepts a rule update only if held-out accuracy rises. Across 14 enterprise-workflow domains from SOP-Bench and $τ^2$-bench using 5 models, ARISE-Teacher, which uses Claude Opus 4.6 for attribution and consolidation, gains 10.9 pp over the baseline on SOP-Bench and 5.8 pp on $τ^2$-bench. ARISE-Teacher also outperforms GEPA by 4.5 pp on $τ^2$-bench and 1.4 pp on SOP-Bench. With the deployment model as its own attributor, ARISE-Self still gains 10.0 pp on SOP-Bench. ARISE-Teacher uses 4.0% fewer model calls and 5.2% fewer output tokens at comparable per-task cost (-1.0%).

Q1: 这篇论文试图解决什么问题？

一、论文试图解决的核心问题
企业 LLM 智能体不是一次性模型调用，而是要在动态环境中持续执行多步工作流：基础模型会升级，业务流程会修改，工具接口和策略会变化，同时部署方还要求准确率和单任务成本可控。论文的出发点是，企业往往无法频繁重训模型或重写工具/工作流，因此需要一种“部署后可演化”的机制，让智能体从自己的失败中学习，而不是依赖权重更新。

二、为什么这不是简单的提示优化
论文要解决的不是单轮问答的提示词好坏，而是多步智能体在真实工作流中的失败修正：失败可能来自错误规划、错误工具调用、遗漏检查、错误参数、对业务规则理解不足，或环境本身变化。要把一次失败转化为未来可用的知识，必须解决三个环节：（1）失败归因——在长轨迹中判断“错在哪一步、为什么错”；（2）修正复用——把具体案例中的教训抽象成可迁移的规则，而不是记住某个题目的答案；（3）安全接纳——避免一条错误或过拟合的修正污染整个记忆系统。

三、记忆层级与验证门控的张力
论文采用 micro memory 与 macro rules 两层：微观记忆保留具体失败案例的修正，检索时更贴近情境但容易碎片化、增加检索成本；宏观规则更紧凑、可审查、可跨任务复用，但可能过度泛化或与旧规则冲突。核心难点是何时把微观修正提升为宏观规则，以及如何防止错误规则进入部署。ARISE 的答案是“留出集准确率门控”：只有候选规则使 held-out accuracy 上升才接纳。这个设计把记忆更新从“生成即写入”变成“生成—验证—接纳”的闭环。

四、成本约束下的自演化
企业部署不仅看准确率，还看模型调用次数、输出 token 和每任务成本。用强教师模型做归因/ consolidation 可能提高准确率，但会引入额外调用；用部署模型自归因更便宜，但归因质量可能不足。论文因此同时评估 ARISE-Teacher 与 ARISE-Self，并把 ∆Calls、∆Tok、∆Cost 作为一等指标。这一点使问题从“能不能提升准确率”扩展为“在什么成本下提升、对哪些模型和任务类型提升”。

五、隐含假设与边界
论文的 formulation 隐含假设包括：失败轨迹可以被归因；修正可以从训练任务迁移到留出任务；留出集能代表部署分布；宏观规则之间可以组合而不产生严重冲突；部署模型或教师模型具备足够的诊断能力；记忆内容不会受到对抗性污染。若工具本身缺失、工作流设计错误或失败不可归因，单纯记忆演化无法修复。论文明确不更新工具与工作流逻辑，这既是其轻量部署优势，也是其能力边界。

六、与现有范式的差异
与权重微调相比，ARISE 不改变模型参数，部署成本低、可审计、模型无关；与一次性提示优化相比，它把失败作为持续信号；与普通反思记忆相比，它增加了微观/宏观两层结构和留出验证门控。其真正要回答的是：在企业约束下，能否把“失败经验”变成可验证、可维护、成本可接受的部署资产。

Q2: 有哪些相关研究？

一、自我反思与经验驱动学习
与 ARISE 最直接相关的是让智能体从自身轨迹中学习的路线：Reflexion 类方法把失败反馈转化为语言反思并用于后续尝试；ExpeL 类方法从成功/失败轨迹中抽取经验或 insight；Self-Refine 类方法让模型迭代修改自己的输出。ARISE 与它们的共同点是“不更新权重、用自然语言经验改进行为”，差异在于它把修正分成可检索的 micro memory 与可审查的 macro rules，并引入留出集准确率门控来决定是否接纳规则。表格把 ExpeL 与 Reflexion 作为对照，说明论文将自身定位为这条经验学习路线的企业化、系统化版本。

二、提示与智能体流程的自动优化
GEPA 是论文最重要的强基线之一。它代表一类通过反思、变异或进化搜索来优化提示/智能体流程的方法；在 SOP-Bench 上 GEPA 平均达到 86.0%，说明它并不是弱基线。ARISE 与 GEPA 的差异在于优化对象：GEPA 更偏向搜索一套更好的提示或流程配置，ARISE 则把失败修正沉淀为可检索记忆和规则，并在部署时动态检索；ARISE 还强调不改变工作流逻辑。论文报告 ARISE-Teacher 在 τ2-bench 上比 GEPA 高 4.5 pp、在 SOP-Bench 上高 1.4 pp，但同时要看到 GEPA 在 SOP-Bench 的强表现——这说明 ARISE 的优势具有任务类型依赖性。

三、智能体记忆系统
Mem0 等工作关注生产级智能体的可扩展长期记忆；生成式智能体路线关注情景记忆、反思与计划；检索增强生成路线关注外部知识检索。ARISE 的记忆不是事实性知识库，而是“失败修正”的程序性记忆：micro memory 保存具体失败的归因与修正，macro rules 保存跨案例的规则。它与传统 RAG 的差别在于写入信号来自智能体自身失败，且写入前有验证门控；与纯反思日志的差别在于有 consolidation 和两层结构。

四、企业工具使用与工作流基准
SOP-Bench 与 τ2-bench 属于企业/工具使用智能体评测：前者覆盖 11 个企业流程域，后者覆盖 3 个多轮领域。相关研究通常关注工具调用准确率、任务完成率、多轮交互和成本。ARISE 在这些基准上评估 5 个模型（Qwen3-32B、DeepSeek v3.2、Haiku 4.5、Sonnet 4.6、Opus 4.6），并把成本变化纳入报告，这使其比只报告准确率的智能体论文更接近部署决策。

五、失败归因与信用分配
ARISE 的 attributor 需要判断失败根因并提出修正，这与智能体调试、轨迹诊断、信用分配和教师模型蒸馏相关。论文用 Claude Opus 4.6 作为教师做归因与 consolidation，实际上把“强模型诊断弱模型失败”作为一种知识迁移机制；ARISE-Self 则测试部署模型能否自我诊断。相关研究还包括过程奖励、执行反馈、工具错误检测等，但 ARISE 的独特之处是把诊断结果写成可检索、可合并、可门控的记忆，而不是直接用于训练。

六、与持续学习/测试时适应的关系
ARISE 不更新权重，因此更接近记忆增强的测试时适应和持续学习。它与在线学习一样面对非平稳分布、灾难性遗忘和负迁移，但没有梯度更新，所有适应都发生在记忆层。相关方法还包括参数高效微调、LoRA、提示微调等；ARISE 的卖点是模型无关、部署轻、可审计，代价是记忆容量、检索成本和规则维护。

七、定位总结
总体看，ARISE 处在“经验学习 + 智能体记忆 + 提示/流程优化”的交汇处。它不主张取代权重更新或工具重写，而是主张在企业部署约束下，用可验证的记忆演化获得大部分收益。其相关工作的关键对比轴是：是否更新权重、是否改变工作流、是否使用教师、是否有验证门控、是否区分微观/宏观记忆。

（注：以上部分文献关系依据论文摘要、Table 2 对照方法与检索片段中的 Mem0、AutoGuide、ExpeL、Reflexion、GEPA 等线索归纳；具体引用版本与作者年份需回原文核对。）

Q3: 论文如何解决这个问题？

一、总体循环
ARISE 的目标不是训练一个新模型，而是维护一套随失败演化的记忆。其闭环可以概括为：在训练任务上运行当前智能体 → 收集失败轨迹 → 由 attributor 归因并提出修正 → 将修正写入 micro memory → 将一批微观修正 consolidate 为 macro rules → 在留出集上验证候选规则 → 只有准确率上升才接纳 → 部署更新后的记忆。整个过程不更新模型权重、工具定义或工作流逻辑。

二、失败归因
方法使用部署模型自身或教师模型作为 attributor。检索片段提到 Figure 4 给出 attributor prompt，说明归因是通过结构化提示完成的：输入包括失败轨迹、任务目标、工具调用与执行结果，输出应包含失败原因和修正建议。教师设置 ARISE-Teacher 使用 Claude Opus 4.6 做归因与 consolidation；自归因设置 ARISE-Self 使用部署模型本身。这一设计把“诊断”和“执行”解耦：执行模型可以较小，诊断模型可以更强；但教师会带来额外调用成本。

三、micro memory
归因产生的修正先以微观记忆保存，粒度接近“某个失败案例的教训”。微观记忆是可检索的：推理时根据当前任务检索相关修正，作为上下文提示。检索策略由 (k, θ) 描述（推测 k 为检索条数、θ 为相关性阈值；具体定义需回原文确认）。微观记忆的优点是具体、贴近情境；缺点是数量增长快、检索成本高、可能包含噪声或过拟合个例。

四、macro rules consolidation
ARISE 不满足于把微观修正一直堆在检索库里，而是定期把一批修正 consolidate 成宏观规则。检索片段明确说 consolidation 接收当前 batch 的 corrections，说明它是批处理式的：一批失败修正被归纳为更紧凑的规则集。宏观规则的好处是可直接审查、跨任务复用、减少检索负担；风险是过度抽象、规则冲突、旧规则僵化。论文把宏观规则视为可被操作员 review 的部署资产，这是企业场景的重要设计。

五、留出集验证门控
候选规则不会直接上线。ARISE 在 held-out 数据上评估更新后的规则集，只有准确率上升才接纳；否则保留当前规则。这个门控是方法的核心安全机制：它把“记忆写入”变成“假设检验”，降低错误归因或过拟合规则对部署的伤害。检索片段中“Validation behavior. The gate keeps the current...”也印证了这一点。门控的代价是需要额外的验证调用和留出数据；如果留出集太小或分布偏移，门控可能失灵。

六、部署与推理
部署时，智能体在执行任务前/中检索宏观规则和/或微观记忆，把它们注入上下文；工具、工作流和模型权重保持不变。因此 ARISE 是模型无关的，可以随基础模型升级而迁移；记忆层也可以版本化、审计和回滚。论文在 5 个模型上验证，说明它不是绑定某个模型。

七、Self 与 Teacher 的取舍
ARISE-Self 用部署模型自己做归因和 consolidation，成本低、隐私好、无需外部强模型；ARISE-Teacher 用 Opus 4.6 做教师，归因质量更高，尤其在多轮任务上收益更大。论文的对比显示：在 SOP-Bench 上 Self 已经很强（平均 86.5），Teacher 只再高 0.9 pp；在 τ2-bench 上 Teacher 平均 62.3，Self 59.1，差距 3.2 pp，说明多轮、长轨迹任务的归因更依赖强模型。这个“教师帮助集中在多轮任务”的观察是方法设计的重要边界。

八、超参数与消融
检索片段提到检索策略 (k, θ) 在低/默认/高设置下准确率为 85.6/87.4/88.2，批大小 B=10/30/50 为 86.3/87.4/87.6（具体对应模型与基准需核对），说明方法对超参不极端敏感，默认设置接近最佳。片段还显示存在“不存储/不检索 micro memory”的消融设置，用于分离宏观规则与微观记忆的贡献；但具体数值在现有证据中不完整。另有 20% label noise 下仍保留约三分之二增益的线索，说明验证门控对噪声有一定鲁棒性，但需要原文确认。

九、设计权衡
ARISE 的主要权衡是：教师质量 vs 调用成本；微观记忆的具体性 vs 检索开销；宏观规则的复用性 vs 过拟合/僵化；验证门控的安全性 vs 验证成本与延迟；不更新权重带来的部署便利 vs 无法修复模型能力上限。论文的贡献不在于发明某个单点模块，而在于把这些权衡组织成一个可运行的闭环。

Q4: 论文做了哪些实验？

一、基准与任务域
论文在两类企业智能体基准上评估：SOP-Bench 覆盖 11 个企业流程域，τ2-bench 覆盖 3 个多轮领域，合计 14 个领域。Overall 指标按 11:3 对两个基准加权，且使用 equal-domain averages，避免大领域样本数淹没小领域。τ2-bench 的 accuracy 被描述为 mean trial success（pass^1），并报告 pass^4（四次试验均成功）的比例；SOP-Bench 的任务完成判定以智能体是否解决任务为准。具体每个领域的任务数、工具集和评分脚本在可见片段中不完整。

二、模型与对照
5 个模型：Qwen3-32B、DeepSeek v3.2、Claude Haiku 4.5、Sonnet 4.6、Opus 4.6，均通过 Amazon Bedrock 调用（检索片段）。对照包括：Base（未加记忆的基线）、ExpeL、Reflexion（表中 Refl.）、GEPA、ARISE-Self、ARISE-Teacher。ARISE 数值为三次运行平均；Opus 4.6 上 Self 与 Teacher 是同一个模型，因此两列数值重复。

三、指标
主指标是 accuracy（%）。同时报告相对 baseline 的 ∆Acc（pp）、∆Tok、∆Calls、∆Cost。成本按 API list price 计算，Opus 5.50/27.50、Sonnet 3.30/16.50、Haiku 1.10/5.50、DeepSeek 0.62/1.85、Qwen 0.15/0.60（每 1M input/output token，美元；来自检索片段）。∆Tok/∆Calls/∆Cost 的负值表示比 baseline 更省。统计上使用分层配对 bootstrap（按测试任务和训练运行）来估计不确定性（Appendix B.1）。

四、评估协议的关键细节
Table 2 caption 明确：SOP-Bench 的每个训练配置只做一次测试 pass；GEPA 在 SOP-Bench 上采用 per-task re-evaluation；τ2-bench 的 evaluation trials 在 §4.1 描述（可见片段未给出细节）。这意味着不同方法的评估次数/重评估方式并不完全一致，比较时需要看 §4.1 和附录。ARISE 的三次运行平均与 GEPA 的逐任务重评估是否等价，是阅读时最需要核对的公平性问题之一。

五、主要结果（Table 2 平均值）
SOP-Bench 平均：Base 76.5；ExpeL 75.7；Reflexion 77.1；GEPA 86.0；ARISE-Self 86.5；ARISE-Teacher 87.4。ARISE-Teacher 相对 Base +10.9 pp，相对 GEPA +1.4 pp；ARISE-Self 相对 Base +10.0 pp。
τ2-bench 平均：Base 56.5；ExpeL 55.5；Reflexion 55.9；GEPA 57.8；ARISE-Self 59.1；ARISE-Teacher 62.3。ARISE-Teacher 相对 Base +5.8 pp，相对 GEPA +4.5 pp；ARISE-Self 相对 Base +2.6 pp。
Overall（14 域，11:3 加权）：Base 72.2；ExpeL 71.4；Reflexion 72.5；GEPA 80.0；ARISE-Self 80.6；ARISE-Teacher 82.0。ARISE-Teacher 相对 Base +9.8 pp，相对 GEPA +2.0 pp；ARISE-Self 相对 Base +8.4 pp。

六、逐模型亮点
Qwen3-32B：SOP-Bench 从 65.9 到 Self 78.4/Teacher 78.1；τ2 从 23.3 到 Self 27.2/Teacher 29.3，绝对水平仍低。
DeepSeek v3.2：SOP-Bench GEPA 89.2 高于 ARISE-Self 84.3 与 Teacher 88.6；τ2 Teacher 59.2 高于 GEPA 56.5。
Haiku 4.5：SOP-Bench Teacher 88.0，与 GEPA 88.3 接近；τ2 Teacher 64.6，远高于 Self 56.7 与 GEPA 55.0。
Sonnet 4.6：SOP-Bench Teacher 89.2，τ2 Teacher 77.3；检索片段称其两个基准上超过 Opus 4.6 baseline，且每任务成本低 33%/39%。
Opus 4.6：SOP-Bench 93.1，τ2 80.9；Self=Teacher，相对 baseline +11.4/+4.4 pp。

七、超参数与消融
检索策略 (k, θ) 低/默认/高：85.6/87.4/88.2；批大小 B=10/30/50：86.3/87.4/87.6（片段所示，具体基准/模型待核对）。这说明默认设置接近最优且曲线较平。检索片段还提到：去掉 micro memory 的存储/检索后仍有 ARISE consolidation 与 validation 的收益（具体下降多少未完整给出）；20% label noise 下增益保留约三分之二；缺失分数超过两个时标签变为 Unable to Decide。这些属于鲁棒性/消融证据，但片段断行严重，需回原文确认。

八、成本实验
摘要报告 ARISE-Teacher 平均每任务成本 -1.0%、模型调用 -4.0%、输出 token -5.2%。Table 2 的 Overall 行显示 ∆Calls -4.0、∆Cost -1.0，但 ∆Tok 仅 -0.4，与摘要的 -5.2% 不一致，可能对应不同聚合口径（例如 SOP-Bench vs Overall、总量 vs 输出量），需要核对。逐模型成本收益也不均匀：DeepSeek SOP-Bench ∆Cost +6.4%，Sonnet τ2 +2.8%，Qwen τ2 -11.9%。因此“成本中性”是平均值结论，不是每个配置都成立。

Q5: 发现了什么实验现象？

一、准确率主效应：记忆演化确实有效，但幅度强烈依赖任务类型
ARISE-Teacher 在 SOP-Bench +10.9 pp、τ2-bench +5.8 pp，Overall +9.8 pp；ARISE-Self 在 SOP-Bench +10.0 pp，但 τ2 只有 +2.6 pp。最直接的解读是：单轮/流程化企业任务中，失败修正更容易被归因和复用，部署模型自归因已能捕获大部分收益；多轮工具使用任务中，长轨迹、延迟反馈和信用分配更难，强教师带来的额外诊断能力更重要。

二、教师 vs 自归因：教师帮助集中在多轮任务
SOP-Bench 平均 Self 86.5、Teacher 87.4，只差 0.9 pp；τ2-bench 平均 Self 59.1、Teacher 62.3，差 3.2 pp。最醒目的单元格是 Haiku 4.5 的 τ2：Self 56.7、Teacher 64.6，教师带来 +11.1 pp（相对 baseline +3.1 vs +11.1）。这说明“用强模型做失败归因”不是均匀增益，而是对弱模型、多轮任务的补偿。反过来，Qwen3-32B 的 SOP-Bench 出现 Teacher 78.1 略低于 Self 78.4 的情况，提示教师也可能引入错误归因或过拟合规则。

三、GEPA 是强基线，ARISE 的优势不是全局碾压
GEPA 在 SOP-Bench 平均 86.0，仅比 ARISE-Self 低 0.5 pp、比 Teacher 低 1.4 pp；在 DeepSeek SOP-Bench 上 GEPA 89.2 甚至高于 ARISE-Teacher 88.6。ARISE 的真正优势在 τ2-bench：GEPA 57.8，ARISE-Teacher 62.3，+4.5 pp。这与“多轮任务更需要过程性规则/记忆”的解释一致，也说明论文的贡献应表述为“在特定任务结构上超过强提示优化基线”，而不是“普遍优于 GEPA”。

四、模型异质性：强模型上限更高，弱模型绝对水平仍低
Opus 4.6 baseline 已是 SOP 81.7/τ2 76.5，ARISE 仍能 +11.4/+4.4；Qwen3-32B τ2 从 23.3 到 29.3，提升比例不小但绝对成功率仍低。方法不能修复基础模型在工具使用/多轮规划上的能力上限；它更像“把已有能力组织得更好”，而不是“教会模型新能力”。

五、成本与调用：平均中性，局部有增有减
Overall Teacher ∆Cost -1.0%、∆Calls -4.0%，但逐单元格差异明显：DeepSeek SOP-Bench +6.4% 成本换 +12.0 pp；Sonnet τ2 +2.8% 成本换 +2.7 pp；Qwen τ2 -11.9% 成本且 +6.0 pp。另有 Sonnet 4.6 的 ARISE 配置在两个基准上超过 Opus 4.6 baseline，且每任务成本低 33%/39%，这是“用小模型+记忆”替代“大模型”的最有力部署论据。但平均 -1.0% 不能解释为所有场景都省钱。

六、验证门控：防止退化，但可能保守
检索片段中“Validation behavior. The gate keeps the current...”表明门控在候选规则不提升时保留旧规则，这能解释为什么 ARISE 很少出现大幅下降（表中大多数 ∆Acc 为正）。代价是：如果留出集噪声大、任务分布变化，门控可能拒绝真正有用的规则，或接纳偶然提升的规则。20% label noise 下仍保留约三分之二增益的片段，说明门控有一定鲁棒性，但也暗示噪声会侵蚀收益。

七、超参稳定性：默认值接近最优
检索策略 (k, θ) 从低到高：85.6 → 87.4 → 88.2；批大小 10/30/50：86.3 → 87.4 → 87.6。曲线较平，说明方法不依赖精细调参；这对企业部署是加分项，但也意味着“检索更多/批更大”带来的边际收益有限。

八、负结果与失败模式
可见的负结果包括：Qwen SOP-Bench Teacher 略低于 Self；DeepSeek SOP-Bench GEPA 高于 ARISE；Sonnet τ2 仅 +2.7 pp（Teacher）且成本 +2.8%；Opus τ2 仅 +4.4 pp。失败模式可能包括：错误归因被写入记忆、规则过度泛化、微观记忆检索到不相关案例、教师与学生模型风格不匹配、验证集不足导致门控失灵。论文的 Case 2（红绿标注、逐字摘录）显然用于展示归因/修正的正反例，值得精读。

九、评估公平性张力
SOP-Bench 上 ARISE 每个训练配置只做一次测试 pass，而 GEPA 采用 per-task re-evaluation；τ2 的 trials 在 §4.1 描述但可见片段缺失。若不同方法的重评估次数、试验次数或任务采样不一致，准确率和成本比较可能有偏差。阅读时应先确认：ARISE 的三次运行平均是否与 GEPA 的重评估在同一测试集、同一预算下比较；成本是否计入教师调用和验证调用。

十、总体判断
实验揭示的核心现象不是“记忆一定有用”，而是“记忆的收益取决于失败可归因性、任务多轮程度和教师质量”。在流程化 SOP 任务上，GEPA 已经很强，ARISE 的优势有限；在多轮 τ2 任务上，教师归因 + 规则记忆带来更明显的提升。这个任务类型交互是论文最值得带走的实证结论。

Q6: 有什么可以进一步探索的点？

1. 归因质量与不确定性：研究如何让 attributor 输出置信度、定位到具体步骤/工具调用，并在低置信度时拒绝写入记忆；结合执行轨迹、工具返回值和过程奖励做信用分配。
2. 降低教师依赖：用教师-学生蒸馏、路由（只对困难/多轮失败调用教师）、主动学习选择最有信息量的失败案例；探索小 verifier 替代大教师做规则验证。
3. 记忆治理：规则冲突检测、过期规则淘汰、版本化与回滚、访问控制、隐私脱敏、防提示注入/记忆投毒；企业部署需要“记忆审计日志”。
4. 验证协议：在线 A/B、延迟标签、非平稳分布下的滚动验证；用置信区间和效应量决定接纳，而不是单点准确率；研究验证集污染与泄漏。
5. 跨域/跨模型迁移：宏观规则能否从 SOP 域迁移到 τ2 域、从弱模型迁移到强模型；模型升级后旧规则是否仍然有效；负迁移的检测与缓解。
6. 微观/宏观记忆的最优配比：学习检索策略 (k, θ)、动态决定何时检索 micro vs macro；记忆压缩与摘要；层次化记忆的容量管理。
7. 人机协同：让操作员 review 宏观规则，提供自然语言解释和影响分析；研究人工反馈的成本与收益。
8. 扩展到工具/工作流演化：论文刻意冻结工具与工作流；未来可在安全约束下让记忆触发工具选择或流程微调，但要防止自动修改带来的风险。
9. 评测扩展：更多企业域、更多多轮任务、pass^k、成本感知指标、对抗性失败、长尾任务；公开可复现的失败轨迹数据集。
10. 理论理解：什么条件下教师归因优于自归因？规则记忆的样本复杂度、泛化界与遗忘机制；多轮任务中过程性记忆的作用机制。
11. 安全与合规：企业数据不能外传给教师时的本地化方案；记忆内容可能包含敏感信息，需要脱敏和保留策略。
12. 与权重更新/参数高效微调的结合：记忆负责快速适应，权重更新负责长期能力提升；研究两者的互补与冲突。

Q7: 总结一下论文的主要内容

一、论文定位
ARISE 是一篇面向企业部署的智能体记忆架构论文，核心主张是：在不更新模型权重、工具或工作流逻辑的前提下，智能体可以通过“失败归因—微观记忆—宏观规则—留出验证”的闭环实现自我演化，从而同时满足准确率和成本约束。论文不是提出新的基础模型，也不是提出新的工具调用算法，而是提出一套可运行、可审计、模型无关的记忆更新流程。

二、问题背景
企业 LLM 智能体处于动态环境：基础模型快速迭代，业务流程频繁更新，部署方要求准确率和成本可控。传统做法要么重训/微调模型（成本高、周期长），要么重写提示/工作流（需要工程投入），要么依赖静态记忆（无法从失败中学习）。ARISE 试图填补的空白是：能否让智能体在部署后从自己的失败中学习，并把学习结果沉淀为可检索、可验证、可维护的记忆资产。

三、方法主线
ARISE 的循环包括：（1）在训练任务上执行智能体，收集失败轨迹；（2）用部署模型自身或教师模型做失败归因，提出修正；（3）把修正保存为可检索的 micro memory；（4）将一批微观修正 consolidate 为 macro rules；（5）在留出集上验证候选规则，只有准确率上升才接纳；（6）部署时检索规则/记忆，不改权重、工具或工作流。教师设置使用 Claude Opus 4.6 做归因与 consolidation；自归因设置使用部署模型本身。方法的关键不是“反思”本身，而是把反思结果分层、批处理、验证门控，避免噪声记忆直接污染部署。

四、实验主线
论文在 SOP-Bench（11 个企业流程域）与 τ2-bench（3 个多轮域）上评估 5 个模型：Qwen3-32B、DeepSeek v3.2、Haiku 4.5、Sonnet 4.6、Opus 4.6。对照包括 Base、ExpeL、Reflexion、GEPA、ARISE-Self、ARISE-Teacher。指标包括 accuracy、∆Tok、∆Calls、∆Cost；Overall 按 11:3 加权，使用 equal-domain averages，ARISE 数值为三次运行平均。评估协议中，SOP-Bench 每个训练配置只做一次测试 pass，GEPA 采用 per-task re-evaluation，τ2 的 trials 在 §4.1 描述——这是比较公平性的关键细节。

五、关键发现
（1）ARISE-Teacher 在 SOP-Bench 平均 87.4，比 Base 76.5 高 10.9 pp；在 τ2 平均 62.3，比 Base 56.5 高 5.8 pp；Overall 82.0，比 Base 72.2 高 9.8 pp。
（2）ARISE-Self 在 SOP-Bench 86.5，+10.0 pp；τ2 59.1，+2.6 pp。自归因在流程化任务上已能捕获大部分收益，多轮任务仍需要教师。
（3）与 GEPA 相比：SOP-Bench 87.4 vs 86.0（+1.4 pp），τ2 62.3 vs 57.8（+4.5 pp），Overall 82.0 vs 80.0（+2.0 pp）。GEPA 在 SOP-Bench 非常强，甚至在 DeepSeek 上高于 ARISE；ARISE 的优势主要集中在多轮 τ2。
（4）教师帮助集中在多轮任务：SOP-Bench Self 86.5 vs Teacher 87.4（+0.9），τ2 59.1 vs 62.3（+3.2）；Haiku 4.5 的 τ2 从 56.7 到 64.6（+11.1）。
（5）成本：Overall Teacher ∆Cost -1.0%、∆Calls -4.0%，但逐模型异质；Sonnet 4.6 的 ARISE 配置在两个基准上超过 Opus 4.6 baseline，且每任务成本低 33%/39%。这说明“较小模型 + 记忆”可能替代“更大模型”。
（6）超参较稳：检索策略低/默认/高 85.6/87.4/88.2，批大小 10/30/50 86.3/87.4/87.6。

六、边界与局限
论文不更新工具/工作流，因此无法修复工具缺失或流程设计错误；强依赖失败可归因；教师调用有成本；平均成本中性掩盖局部成本上升；记忆可能被错误归因、噪声或对抗性内容污染；留出验证门控可能保守或受分布偏移影响；评估协议（单次测试 pass vs per-task re-evaluation、τ2 trials）需要仔细核对；摘要与 Table 2 在输出 token 变化上存在 5.2% vs -0.4 的不一致，需要回原文确认。

七、可迁移价值
ARISE 的可复用设计模式是“失败归因 + 双层记忆 + 验证门控 + 成本核算”。对 agent memory、self-improving agents、企业工具使用、成本敏感部署都有直接参考价值；对 AI-for-science 智能体，可把“实验失败归因—规则固化—留出验证”迁移到实验流程，但需要领域专家定义验证集和安全边界。

八、总体判断
论文的实证贡献是扎实的：14 个域、5 个模型、多个强基线、成本列和超参分析。但它不是“全面碾压 GEPA”的故事；更准确的结论是：在流程化 SOP 任务上，GEPA 已是强基线，ARISE 仅小幅领先；在多轮工具任务上，教师归因 + 规则记忆带来更明显收益。方法的价值在于把企业部署约束（不改权重、可审计、成本可控）转化为可运行的记忆闭环。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：企业智能体、工具使用、多轮任务、记忆驱动自演化，都是当前 agent 研究的核心议题。

## 基本信息

- 作者：Jiangyun Zhang, Zhou Tang, Kristen Surrao, Zhiyu Chen, Albert Guan, Tor Nitayanont, Yupei Zhang, Roopali Singh, Yunkai Xiao, Shreya Pandit, Julia Huang, Shayan Ali Akbar, Erwin Cornejo, Yuan Li, Yi Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.MA
- 日期：2026-11-17
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.23588v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Method/结果片段，并用摘要与 Table 2 校准关键数字；检索片段存在断行、表格残片和个别数字不一致（如输出 token 变化），无法确认的细节已标注“需核对/推测”，未编造实验设置或机构信息。
