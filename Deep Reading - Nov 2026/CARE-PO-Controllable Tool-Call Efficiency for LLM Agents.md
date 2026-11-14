---
user_id: "cheng tan"
paper_id: 21007
arxiv_id: "2611.12125v1"
title: "CARE-PO: Controllable Tool-Call Efficiency for LLM Agents"
publish_date: "2026-11-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.12125v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.12125v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-11T02:14:00"
---
# CARE-PO: Controllable Tool-Call Efficiency for LLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：tool-call efficiency · llm agents · grpo · reward shaping

## 一句话总结

CARE-PO 通过在 GRPO 中引入按任务难度锚定、训练前固定的参考预算，并用成功源轨迹注入与覆盖成本抑制“零工具调用”坍缩，再用提示中的 effort 标签条件化策略，使 LLM agent 在单次训练后即可在成功率与工具调用数之间选择不同操作点。

## 摘要

> External tool calls are a major running cost of an LLM agent, while its training objectives align almost exclusively with task completion. In a step-level diagnosis of Qwen3.5 trajectories at two model sizes on GAIA, a long-horizon tool-use benchmark, we find that most contain removable calls, at a similar rate at both sizes. Some methods add an efficiency reward to the objective to reduce the call count, but introduce problems of their own: the anchor they price calls against is computed from the policy's own rollouts (e.g., the group's shortest correct rollout) and so moves with the policy; training can drift toward a collapsed policy that issues no tool call at all, a degeneracy seen in published reports and in our 27B baseline; and a training run delivers a single operating point, so the user cannot trade efficiency against capability. We present CARE-PO (Controllable Anchored Relative Efficiency Policy Optimization) to mitigate these problems. The efficiency reward prices calls against a reference budget anchored to the task's own difficulty and fixed before training. Injecting a successful trajectory from the training data into each online GRPO rollout group and charging for evidence a trajectory never gathers ease the drift toward a collapsed call count. Shaping the cost curve by an effort tag in the prompt yields an efficiency and a capability operating point from a single training run. On GAIA, five of the six operating points across three model sizes improve on their base model in success rate and call count at once. On four held-out multi-hop benchmarks the result is size-dependent: at 27B and 4B success rate is held or raised at a lower call count, while at 9B both operating points trade success rate for fewer calls. The call count remains well above zero throughout training at all three sizes.

Q1: 这篇论文试图解决什么问题？

1. 核心成本错配。论文的出发点是：LLM agent 的外部工具调用是主要运行成本，但训练目标几乎只对齐任务完成，不惩罚多余调用。于是模型有动机通过“多查几次、早试错、重复验证”换取成功率，形成成功率高但调用数高的策略。
2. 诊断粒度缺口。已有工作记录了 tool overuse，但论文指出多数是轨迹级计数，缺少在单条轨迹内部定位冗余步骤的判据；作者因此在 GAIA 上对两个规模 Qwen3.5 做步骤级诊断，发现大多数轨迹含可删除调用，且两个规模比例相近。这说明冗余不是小模型独有，也不能只靠“模型变大”自然消失。
3. 现有在线 RL 效率奖励的三个结构性问题。其一，锚点内生：OTC-PO 用组内正确轨迹的最短调用数作为价格锚，锚随策略更新而移动，奖励目标本身非平稳；策略越省，锚越低，继续省调用越来越难，训练可能走向极端。其二，零调用坍缩：效率项可能把“完全不调用工具”变成最优退化策略；论文称已发表报告和作者 27B baseline 都出现该现象。其三，单操作点：一次训练只得到一个成功率—调用数权衡点，用户想换效率档位需重新训练。
4. GRPO 层面的额外困难。效率奖励若让同一 rollout 组的分数趋同，组内优势消失，GRPO 得不到有效梯度（论文引用 He et al., 2026）。这意味着“加一个效率项”不仅是奖励设计问题，还会改变优化信号的可辨识性。
5. 因此论文的问题定义不是单纯“把调用数降下来”，而是在成功率不降或提升的约束下，让调用效率相对任务级、训练前固定的基准来度量，并让同一策略可在多个效率—能力操作点间切换，同时避免零调用退化。
6. 隐含假设与边界。方法默认任务难度可预先锚定、调用数能代表成本、成功判定可靠；若工具异质（搜索 vs 代码执行 vs 付费 API）、任务分布漂移或存在合法无需工具的问题，固定参考预算与覆盖计费的合理性需要重新检验。上述“零调用是退化”的批评在工具密集任务中成立，但对本来无需工具的任务可能是误伤，这是阅读时要特别区分的边界。

Q2: 有哪些相关研究？

论文可见的 Related Work 信息主要来自 Introduction 的分类，完整综述在片段中未展开，以下按论文自身的脉络组织。
1. 推理效率与 early exit。论文把“减少工具调用”与已有的 overthinking、推理 token 早停研究区分开：后者研究推理 token 的过度思考与提前退出（Cuadron et al., 2025; Wei et al., 2026），工具调用这一外部动作研究得少得多。
2. 工具过度使用。Qian et al. (2025) 记录了 tool overuse，但论文认为多数工作停留在轨迹级调用计数，缺少步骤级冗余定位；CARE-PO 的第 3.1 节补这一诊断粒度。
3. 推理时约束。第一类干预不改权重，在解码/执行时把预算作为外部约束施加（Liu et al., 2026b; Chen et al., 2026b; Fang et al., 2026）。优点在于即插即用，缺点是不会把效率内化到策略里，且通常只给出一个受约束的运行点。
4. 离线训练。第二类在预先收集的信号上改权重（Chen et al., 2026a; Wang et al., 2026），不能根据在线 rollout 自适应；与本文的 online GRPO 路线形成对照。
5. 在线 RL。第三类用 GRPO（Shao et al., 2024）把效率需求放进奖励，代表工作是 OTC-PO（Wang et al., 2025）与 TEPO（Li et al., 2026）。论文的关键批评是：OTC-PO 的锚是组内正确轨迹的最小调用数，会随策略漂移；TEPO 用“降低熵的调用比例”度量效率，没有任务级参考预算；两者都只产出一个操作点。
6. 优化层面的相关工作。He et al. (2026) 指出效率奖励可能把 rollout 组分数抹平，使 GRPO 无梯度；这构成 CARE-PO 设计反坍缩与组内比较机制的直接动机。
7. 与本文的定位差异。CARE-PO 不是另加一个更强的推理时约束，也不是换一个离线信号，而是把价格锚改成任务级、训练前固定的参考预算；用源成功轨迹注入与未收集证据计费保持组内可比较；用 effort tag 条件化策略，让一次训练覆盖多个操作点。片段还提到 search agents 的相关报告，但文本截断，无法确认具体比较对象。

Q3: 论文如何解决这个问题？

一、总体形式。CARE-PO 学一个条件策略 πθ(·|x,e)：x 是任务，e 是提示中的 effort 标签。奖励由“交付价值” V_e 与“效率因子” U_e 组成，U_e 对成本 u 单调递减；片段中出现 Eq. (1)，但公式不完整，不能据此推导具体归一化。训练使用 online GRPO，把效率需求放进奖励而不是外挂推理时约束。
二、固定参考预算，替代内生锚点。每个任务有一个按自身难度锚定、训练前固定的 reference budget B_ref；调用成本相对 B_ref 计价，而不是相对当前策略组内最短正确轨迹。这样价格不随策略变省而移动，避免“越省越难再省”的非平稳奖励。论文的“relative efficiency”可理解为：同样调用数，在更难、预算更大的任务上相对便宜；在同一任务内，超过预算的调用被更重地计费。具体 B_ref 是由标注、源轨迹还是难度分箱构造，证据不足，需回 Section 3.3。
三、反坍缩：源轨迹注入。每个在线 GRPO rollout 组本来有 G 条策略样本；作者把训练数据中该任务的一条成功源轨迹作为第 G+1 个成员加入组内，并用同一奖励式打分。这样组内始终有一个“成功但调用结构不同”的参照，降低所有策略样本一起退化为零调用的概率，也为 GRPO 保留组内分数差异。
四、反坍缩：覆盖计费。奖励对“轨迹没有收集到的证据”计费，形成 coverage 项。其效果从片段可见：一条完全不调用工具、即使答案正确的轨迹，也要按一个 reference budget 计费，和“花完整预算交付”的轨迹相同，而不是零成本。这样零调用不再是免费捷径；但也会误伤合法无需工具的任务，这是方法的明确边界。
五、可控操作点。effort tag e 塑造成本曲线：低 effort 让调用更贵、鼓励少调用；高 effort 让调用相对便宜、鼓励多验证。一个条件策略在一次训练后同时覆盖效率点与能力点，用户在推理时用标签选择，而不必重新训练。片段中的 low/high 两个点即这一机制的体现；是否支持连续档位、标签如何校准，证据不足。
六、组件耦合与代价。固定 B_ref 解决锚点漂移，源轨迹注入与 coverage 解决坍缩，effort 条件化解决单操作点；三者分别对应论文归纳的三类问题。代价是奖励设计更复杂：B_ref 需要可靠的任务难度先验，coverage 需要可操作的“证据”定义，源轨迹可能把策略拉向特定解题风格。论文片段没有给出这些实现细节，复现时需重点核对。

Q4: 论文做了哪些实验？

1. 诊断实验。作者在 GAIA 上对两个模型规模的 Qwen3.5 轨迹做步骤级诊断，统计含可删除调用的轨迹比例；摘要称大多数轨迹含冗余调用，且两个规模比例相近。具体判据（人工标注、规则匹配还是模型判定）在片段中未给出，是阅读 Section 3.1 的重点。
2. 主训练与评测。主实验覆盖三个模型规模（4B、9B、27B，合理推断为同一 Qwen3.5 家族），在 GAIA 与四个 held-out 多跳基准上评估。训练和评测都使用 live、非平稳的 web search；GAIA 报告三次独立运行的均值与样本标准差，多跳基准使用 greedy decoding。至少一个 held-out 基准是 Bamboogle（125 题，Press et al., 2023），另外三个名称未在证据片段中完整出现。
3. 基线与操作点。比较对象包括 base model、OTC-PO、TEPO；OTC-PO 与 TEPO 由作者重新实现。CARE-PO 至少报告 low 与 high 两个 effort 操作点。片段明确提到 Tables 1 和 2 因 27B OTC-PO 出现零调用坍缩，改为评估其较早 checkpoint；这提醒读者核对 checkpoint 选择规则是否对所有基线一致。
4. 指标。摘要明确使用 success rate 与 call count；检索表片段还出现 SR、AC、HV 三列。按上下文推测 SR=success rate、AC=average call count、HV=综合成功率与调用数的 hypervolume/Pareto 指标；HV 的精确定义未在证据中出现，不能把片段数值当作最终排名。
5. 消融。片段提到对 effort conditioning 与 coverage term 做消融，并与 OTC-PO 的坍缩运行对比；还出现“trained at t=1, low tag”一类配置描述，但表头不完整，无法还原全部消融网格。
6. 分析实验。作者选取 base model 与效率点在每次运行都答对的 37 道题，发现调用数仍下降 57.5%；并用逐题调用数对另一来源做回归，声称压缩幅度随任务难度变化。这个分析用于排除“效率提升只是因为放弃了难题”的替代解释。
7. 证据缺口。片段没有给出四个多跳基准的完整名称与规模、工具集合、prompt、训练超参、计算预算、显著性检验和完整表格；因此无法独立判断 5/6 双赢的统计稳健性，也无法复现 B_ref 与 coverage 的具体实现。

Q5: 发现了什么实验现象？

1. 冗余是普遍现象，而非小模型专属。步骤级诊断显示大多数 GAIA 轨迹含可删除调用，两个模型规模比例相近。这个现象本身支持“效率应作为训练目标”而不是只靠 scale 解决；但论文没有在片段中给出冗余步骤的类型分布，无法判断是重复搜索、无效验证还是错误重试。
2. GAIA 上出现少见的“双赢”。三个规模、两个 effort 点共六个操作点中，五个同时在成功率与调用数上优于 base model。这比常见的“降调用必降成功率”更强；但六个里仍有一个未双赢，且片段没有说明是哪一个规模/档位，阅读时应定位失败点，避免把 5/6 误读为普遍规律。
3. held-out 多跳基准呈非单调的规模依赖。27B 与 4B 在更低调用数下保持或提升成功率；9B 的两个操作点都用成功率换调用数。也就是说，中间规模反而出现负迁移。合理推断这与 9B 的优化稳定性、容量阈值或 effort 条件化校准有关，但论文片段未给出机制解释；这是最重要的负结果之一。
4. 反坍缩有效，但“调用数大于零”只是最低标准。27B OTC-PO baseline 被报告坍缩到零调用，CARE-PO 在 4B/9B/27B 训练全程调用数都远高于零（Appendix C）。这支持源轨迹注入与 coverage 计费的作用；但调用数不归零不等于策略健康，仍需看成功率、调用结构和是否只是被 coverage 项强制产生低价值调用。
5. 效率收益不只是“放弃难题”。在 base 与效率点每次都答对的 37 道题上，调用数仍下降 57.5%。这是一个较强的机制证据：同一批可解题上，策略确实减少了冗余步骤，而不是靠漏答换低调用。不过 37 题是子集，不能外推到全部 GAIA。
6. 压缩幅度与任务难度相关。片段称用逐题调用数对另一来源回归，压缩随任务难度 scaling；具体方向和效应量未给出。若难任务压缩更多，说明冗余主要集中在长尾复杂任务；若相反，则效率增益可能集中在简单任务。阅读时需核对回归自变量与系数。
7. effort 标签确实形成不同操作点，但权衡方向需看完整表。片段中可见 CARE-PO (low) 为 66.8/1.99/0.601、CARE-PO (high) 为 75.4/2.80/0.649（按 SR/AC/HV 顺序的推测），高 effort 点成功率与调用数同时更高，HV 也更高；低 effort 点更省调用但成功率低。由于列映射和模型/数据集归属不清，这只能作为“存在可调权衡”的旁证，不能据此排名。
8. 成本模型有一个反直觉边界。完全不调用工具的正确轨迹也按一个 reference budget 计费，和花完整预算交付相同；这避免了零调用免费，但也可能惩罚合法无需工具的任务。该设计在 GAIA 这种工具密集基准上合理，迁移到混合任务时需重新评估。
9. 非平稳 web 环境带来方差。训练与评测都用 live web search，GAIA 只报三次运行；多跳基准 greedy decoding。检索结果随时间和地域变化，复现难度高；片段没有给出置信区间或配对检验，5/6 双赢的稳健性需看完整表。
10. 基线比较的公平性需核对。27B OTC-PO 在较早 checkpoint 评估，作者解释是坍缩；这合理，但也意味着基线不是最终策略。读者应检查 checkpoint 选择是否预先规定、是否对所有基线一致，以及是否给出坍缩前后的完整曲线。

Q6: 有什么可以进一步探索的点？

1. 把调用数升级为真实成本模型。当前以 call count 计价，无法区分搜索、代码执行、付费 API、长延迟工具。后续可研究异质工具价格、token/延迟/金额/风险的联合成本，以及成本感知的 Pareto 前沿。
2. 自动化与校准 reference budget。B_ref 目前依赖任务难度锚定且训练前固定；如何从少量标注、源轨迹或难度预测器自动估计，如何在分布漂移下更新而不破坏锚的稳定性，是核心问题。可探索“固定锚 + 在线监控”的两层机制。
3. 连续 effort 控制。论文至少展示 low/high 两个点；后续可研究连续 effort 标签、用户偏好条件、推理时预算约束与策略条件化的组合，并校准标签语义，使“低/高”在不同任务上可比较。
4. 理论分析。需要解释固定锚为何比内生锚更稳定、coverage 项在什么条件下消除零调用退化、源轨迹注入如何影响 GRPO 梯度方差，以及效率奖励抹平组内分数（He et al., 2026）的充分条件。
5. 跨规模与跨模型族验证。9B 的负迁移说明方法不是尺度不变；后续应在更多模型族、更多中间规模上做 scaling 分析，定位容量阈值与优化不稳定性，并设计规模自适应的 effort 校准。
6. 非平稳工具环境下的鲁棒性。live web search 的索引、延迟和结果质量会漂移；可研究在线重估 B_ref、缓存/验证调用、以及在工具失败或返回空结果时的回退策略，避免把环境噪声误判为冗余。
7. 与推理时约束结合。CARE-PO 是训练时内化效率；推理时预算、早停、缓存、计划器仍可叠加。后续可比较“训练内化 + 推理外约束”的组合是否优于单独使用，并研究二者冲突时的优先级。
8. 扩展到更多 agent 场景。除 GAIA/多跳问答外，应覆盖代码执行、企业 API、科学工作流、多工具协作和长程任务；这些场景的工具成本结构、证据需求和成功判定都不同，coverage 的定义需要重做。
9. 安全与可靠性。少调用可能意味着少验证、更多幻觉或过早承诺；后续应研究不确定性感知的调用策略、关键证据的最低覆盖约束，以及效率优化对错误类型分布的影响。
10. 评测协议。需要报告配对显著性、置信区间、完整 Pareto 前沿、不同 web 快照下的复现结果，以及基线 checkpoint 选择规则；否则“双赢”难以跨团队比较。

Q7: 总结一下论文的主要内容

一、论证主线。论文指出 LLM agent 的外部工具调用是主要运行成本，但训练目标几乎只奖励任务完成，导致策略倾向于用更多调用换成功率。作者没有直接假设“调用越少越好”，而是先在 GAIA 上做步骤级诊断：对两个规模 Qwen3.5 的轨迹逐步骤检查，发现大多数轨迹含可删除调用，且两个规模比例相近。由此论文主张，工具冗余是训练目标与策略行为共同造成的普遍问题，不能只靠模型变大解决。
二、对现有方法的批评。作者把干预分为推理时约束、离线训练和在线 RL 三类，重点批评在线 RL 中的效率奖励：OTC-PO 用组内正确轨迹的最短调用数作锚，锚随策略移动；TEPO 用熵降低型调用比例，没有任务级参考；两者都只产出一个操作点。更严重的是，效率奖励可能把策略推向完全不调用工具的坍缩解，作者在 27B baseline 观察到该退化；同时若效率项抹平组内分数，GRPO 会失去梯度。
三、技术主线。CARE-PO 的三个组件逐一对应上述问题。第一，效率奖励不再相对策略自身 rollout 计价，而是相对每个任务按难度锚定、训练前固定的 reference budget；价格外生且稳定。第二，在每个 online GRPO rollout 组中加入训练数据里的成功源轨迹作为第 G+1 个成员，并对轨迹未收集到的证据计费（coverage）；零调用正确轨迹也要按一个 reference budget 计费，从而不再是免费捷径。第三，用 prompt 中的 effort tag 条件化成本曲线，学一个 πθ(·|x,e)，让单次训练覆盖低 effort 的效率点与高 effort 的能力点。
四、实验主线。诊断之外，主实验覆盖 4B/9B/27B 三个规模，在 GAIA 与四个 held-out 多跳基准上比较 base、OTC-PO、TEPO 与 CARE-PO 的 low/high 操作点；训练和评测都用 live web search，GAIA 三次运行报均值与标准差，多跳基准 greedy。GAIA 上六个操作点中五个同时在成功率和调用数上优于 base；held-out 结果依赖规模：27B/4B 在更低调用数下保持或提升成功率，9B 两个操作点都以成功率换调用数；三个规模训练中调用数始终远高于零。作者还用 37 道 base 与效率点每次都答对的题做子集分析，发现调用数仍下降 57.5%，并报告压缩幅度与任务难度相关。
五、结论与边界。论文的贡献不只是“省调用”，而是把效率锚外生化、把反坍缩机制显式化、把单操作点变成可条件化的多操作点；其最强证据是 GAIA 的 5/6 双赢与同对子集上的大幅压缩，最强反例是 9B 的负迁移。边界也很清楚：调用数只是成本代理，B_ref 的构造与校准未在证据中展开，coverage 计费可能误伤合法无需工具的任务，live web 与非平稳性带来复现方差，27B OTC-PO 的较早 checkpoint 也需要核对公平性。整体上，这是一篇以诊断驱动方法设计、并用多规模与负结果展示边界的 agent 效率训练论文。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与画像中的 agent 方向直接重合（权重 0.10）：主题是 LLM agent 工具调用效率与训练。

## 基本信息

- 作者：Xiaoyang Ye, Xiao Yao, Hairui Zhu, Yiying Yang, Bei Gan, Jinliang Lu, Tengjin Weng, Yuan Yao, Wenhao Jiang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-11-09
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.12125v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Introduction 与 References 片段，并对片段中列名、数值映射和公式不完整之处作了显式不确定性标注。
