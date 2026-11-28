---
user_id: "cheng tan"
paper_id: 22710
arxiv_id: "2611.30190v1"
title: "Same Executions, Different Gaps: Judgment Rules in Web-Agent Evaluation"
institution: "加州大学圣地亚哥分校（Zelong Wang）；哈佛大学（Weiyue Li）；独立研究者（Fei Xue、Junru Zhu，Seattle）。"
publish_date: "2026-11-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.30190v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.30190v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-28T01:37:11"
---
# Same Executions, Different Gaps: Judgment Rules in Web-Agent Evaluation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：web agent evaluation · llm-as-a-judge · agentrewardbench · paired comparison

## 一句话总结

论文在固定的 web-agent 执行记录上把评估比较拆成成功定义、证据支持与差异估计三个环节，证明把 judge 从“预测可能成功”改为“要求记录支持成功”可以在排名不变的情况下把 Claude 相对 Qwen 的领先从 8.33 个百分点压到 1.33 个百分点，并指出单 agent 分数更准确未必让 agent 间差距更准确，进而提出结合 judge 预测与抽样参考标签的差异估计思路。

## 摘要

> 论文关注 web-agent 评估中的一个被忽视问题：成功率增益被用来指导研发，但“谁排第一”并不等于“领先幅度有多可信”。作者把比较本身作为研究对象，区分三个环节：如何定义成功、什么证据支持 judge 的判定、以及如何估计两个 agent 的差异。研究在 AgentRewardBench 的固定执行记录上进行，保持 execution 与 record 不变，只改变 judge 的问题：让 judge 预测任务是否可能成功，或要求其判断提供的记录是否支持完成。结果显示，在后一种认证式判定下，Claude 相对 Qwen 的领先从 8.33 个百分点降到 1.33 个百分点，但点估计排名不变；显式允许 unresolved verdict 的新比较仍保留这一缩减，说明效应不是简单把未解决完成归为失败。控制性 source addition 实验进一步表明，任务要求、对象与观察之间缺失的关系可以掩盖真实差异，也可以制造表面优势。论文还报告一个反直觉现象：让两个 agent 各自的分数估计更准确，可能让它们的差异估计更不准确，因为原始误差部分抵消。为估计选定的结果差异，作者提出结合 judge 预测与抽样 task pair 的参考标签；额外 judge 与执行系统显示出一系列异质响应。

Q1: 这篇论文试图解决什么问题？

论文要解决的不是常规的“哪个 web agent 更强”，而是评估比较本身的测量可靠性：在固定执行记录与固定判定输入下，两个 agent 的成功率差距是否真的可识别、可复核、可解释。背景张力在于，success-rate gains 直接指导 planning、perception、tool use 等改动是否值得继续，而模型 judge 让大规模比较变得便宜；但 judge 对“成功”的定义、对证据的要求、以及对差异的汇总方式，都会成为测量仪器的一部分。论文的核心观察是：两个分数的差可以表现得不同于任一分数本身。如果一条评估规则同时降低两个 agent 的分数但降幅不等，排名可能保持不变，而报告的优势幅度却大幅消失；反过来，让每个 agent 的分数估计更准确，也可能让二者之差更不准确，因为原始误差可能部分抵消，改进单 agent 准确率会破坏这种抵消。由此，论文把比较拆成三个子问题：第一，什么算成功，是“任务可能完成”还是“记录支持完成”，未解决完成是否等同于失败；第二，什么证据支持判定，执行记录是否包含任务要求、对象与观察之间的必要关系；第三，如何估计差距，是用点估计、抽样参考标签，还是需要把方差与偏差分开建模。论文挑战的隐含假设包括：排名第一足以传递优势幅度；judge 的预测式判断可以替代证据认证；单 agent 准确率是差异估计的充分统计量；未解决完成可以简单归为失败；执行记录完整呈现了判定所需的关系。论文的贡献属于评估元研究或测量学，而非新 agent 能力；其边界是 AgentRewardBench 的固定 executions、有限 agent 对与抽样参考标签，因此结论应理解为“判定规则可以显著改变报告差距”的机制证明，而不是对所有 web-agent 排行榜的普适校准。

Q2: 有哪些相关研究？

与本文最直接相关的是 web-agent 评估与模型 judge 可靠性两条线。AgentRewardBench [11] 将模型 judge 的判断与专家评估对照，并报告不同评估者下 agent 分数与排名的变化，这为本文提供了固定 executions 与参考标签的基础；本文与其互补：AgentRewardBench 更关注 judge 与专家是否一致、排名是否稳定，本文进一步追问在排名不变时报告差距对判定规则有多敏感。WebJudge [13] 通过识别任务要求与相关截图来改进评估，说明“证据选择”本身会改变 judge 输入；本文把这一点推进为受控的 source addition 实验，考察任务要求、对象与观察之间缺失的关系如何掩盖差异或制造表面优势。Gaming the Judge 表明改变 agent 的陈述或呈现方式可以影响 judge 判断（片段在此处截断，具体机制需回原文确认），这与本文“认证式判定”的动机一致：要求记录支持而非预测可能成功，可能降低对表面陈述的敏感性。更广的相关工作包括 LLM-as-a-judge 的偏差与提示敏感性问题、配对比较与方差缩减、半监督/控制变量估计、以及 benchmark 排行榜的可靠性与可复现性。论文还隐含连接到测量不变性（measurement invariance）与差异中的差异（difference-in-differences）思路：两个群体的分数差在测量规则变化下是否稳定，不能由每个群体单独的测量精度推出。现有工作多数检查 judge 与专家的一致性、排名稳定性或单 agent 分数，较少系统研究“判定规则对差距幅度”的非对称影响与误差抵消；本文的定位正是补这一缺口。以上除 [11][13] 与 Gaming the Judge 外，部分相邻文献关系为合理推断，需回原文的 Related Work 与引用列表确认具体出处。

Q3: 论文如何解决这个问题？

论文的方法不是训练新 agent，而是构造一套评估比较的诊断协议。首先，固定 AgentRewardBench 中的 web-agent executions 与可用记录，选取配对的 web-agent tasks（片段显示 300 对任务，其中 50 对抽取参考标签，对应 100 条 execution），使不同判定规则在同一批执行上可比。其次，操作化两种 judge 问题：prediction 问“任务是否可能成功”，certification 问“提供的记录是否支持完成”；认证式判定允许 endorsed、unresolved、not endorsed 等 verdict（图中出现 1、0.5、0 的编码，具体语义需回原文确认），从而把“未解决完成”与“有支持的失败”区分开。再次，做 controlled source additions：在原始记录上加入 saved layout records 等来源，恢复任务要求、对象与观察之间的关系；用位置约束、对象/时间混淆等案例观察比较如何被改变。第四，估计差距：对抽样的 task pairs 获取参考标签，把 judge predictions 与 reference labels 结合，估计选定的结果差异；比较样本 judge gap 与全队列差距，并用 MSE 分解为 variance 与 squared bias 来评估估计误差，配合 calibration 与 decompletion controls。第五，做多 judge、多 execution system 的对照，包括 Astra/final、Sonnet 4.5、Kimi K2.5 等（具体角色需查原文），检查效应是否只是单一 judge 的伪影。最后，评价对象不是单 agent 准确率，而是排名、差距幅度与差异估计误差三者的关系。整体上，这是一套“改变判定规则—观察差距—分解误差—用少量参考标签校准”的评估方法学。

Q4: 论文做了哪些实验？

论文的实验围绕固定 executions 上的判定规则对照展开。数据与任务：使用 AgentRewardBench 的 web-agent 执行记录，片段显示 300 对配对任务，其中 50 对任务抽取参考标签（100 条 execution），主要比较对象为 Claude 与 Qwen，并补充额外 judge 与执行系统。实验一，prediction vs certification：同一批 execution 与 record 不变，只把 judge 问题从“可能成功”改为“记录支持完成”；图示/片段显示 prediction 下 Claude 26.67%、Qwen 18.33%（差 8.33 pp），certification 下 Claude 14.50%、Qwen 13.17%（差 1.33 pp），排名仍为 Claude 在前（具体数值对应关系需以原文图 1 为准）。实验二，unresolved verdict：构造显式允许未解决判定的新比较，检查缩减是否只是把未解决完成算作失败；片段表明缩减保留。实验三，controlled source additions：在原始记录上加入 saved layout records，构造位置约束、对象/时间混淆等案例，观察缺失的任务要求—对象—观察关系如何掩盖差异或制造表面优势；片段报告一个比较中揭示了遗漏的区分，另一个比较中移除了表面优势。实验四，抽样估计与误差分解：用 50 对参考标签估计差距，比较 sample judge gap 与 full cohort；用 MSE 同时计入 variance 与 squared bias，并做 calibration 与 decompletion controls；fresh prediction 与 binary certification controls 在同一 batch 中使用 Astra/final。实验五，多 judge/多系统：Astra/final、Sonnet 4.5、Kimi K2.5 等显示出一系列响应，检验效应异质性。实验六，单 agent 准确率 vs 差异准确率：比较让各 agent 分数更准确时，二者差距是否同步更准确，从而检验误差抵消。评价指标包括成功率、百分点差、排名、MSE/方差/偏差与校准；论文的重点不是单点 SOTA，而是同执行下不同判定规则的差距稳定性。

Q5: 发现了什么实验现象？

最醒目的现象是排名与幅度的解耦。按片段数值，预测式判定下 Claude 26.67%、Qwen 18.33%，差 8.33 pp；认证式判定下 Claude 14.50%、Qwen 13.17%，差 1.33 pp，Claude 仍排第一。两个分数都下降，但 Claude 降幅约 12.17 pp、Qwen 约 5.16 pp（按片段数值计算），降幅不对称，因此领先被压缩而非整体平移。这直接说明“排名不变”不能推出“优势幅度不变”。第二，允许 unresolved verdict 后缩减仍保留，排除了“认证只是把未解决完成都算失败”的简单解释；未解决完成与有支持的失败是不同测量对象。第三，source addition 出现方向相反的结果：缺失的任务要求—对象—观察关系在一个比较中掩盖了真实差异，在另一个比较中制造了表面优势；加入 saved layout records 后，一个比较揭示了遗漏的区分，另一个比较移除了表面优势。这说明证据完整性不是单向偏置，既可能低估也可能高估差距。第四，反直觉的误差抵消：让两个 agent 各自的分数估计更准确，可能让它们的差异估计更不准确；差异误差取决于两个 judge 误差的协方差，而不只是两个边际准确率。第五，MSE 分解显示方差与平方偏差都重要；50 对参考标签对应 100 条 execution，样本量本身会限制差距估计的稳定性，sample judge gap 与 full cohort 的对比正说明抽样波动。第六，多 judge 与多 execution system 呈现范围响应：效应并非单一 judge 的孤立伪影，但也不是完全同质，具体方向和幅度依 judge/系统而变。第七，指标间张力明显：单 agent 准确率、排名、差距幅度、差距误差、校准可以给出不同结论；认证式判定更严格不等于更接近真实能力，它可能改变的是测量对象。第八，片段提到 supported success 的小差距不一定意味着执行相同；小差距需要结合证据支持与未解决判定解释，不能直接读作能力接近。

Q6: 有什么可以进一步探索的点？

可探索方向包括：第一，把差距的不确定性作为一等报告对象，报告标准误、置信区间与配对设计的统计功效，而不是只报排名或点估计。第二，系统化 verdict schema：endorsed、unresolved、not endorsed 的语义、校准与任务依赖性；研究未解决判定的成本与收益。第三，建立差异估计器的理论：显式建模两个 judge 误差的相关性，推导单 agent 准确率与差异准确率的关系，给出误差抵消发生条件的诊断。第四，改进参考标签的采样：主动采样、分层抽样、半监督/控制变量估计，在有限标注预算下最小化差距估计方差；扩展到多 agent、多任务、多 judge 的比较。第五，自动抽取任务要求—对象—观察关系，把 source addition 做成 stress test 或反事实记录增强工具，检测记录缺失导致的假优势与假阴性。第六，跨 benchmark、跨 execution system、跨任务类型验证；当前证据集中在 AgentRewardBench 与有限 judge，需确认对动态网页、非确定性执行、多轮交互的适用性。第七，leaderboard 治理：当判定规则改变时，同时报告排名与差距变化，避免“排名稳定”掩盖“优势消失”；对动态基准做版本化与审计。第八，迁移到相邻评估场景：AI-for-science 工作流中的证据认证、生成系统的 judge 可靠性、过程奖励与可验证评估；这些场景同样面临“预测成功”与“记录支持”的区分。第九，失败模式研究：哪些任务类型最容易受缺失关系影响，哪些 judge 最容易被表面陈述诱导，哪些差距在统计上不可识别。整体上，论文打开的是一个评估测量学议程，而不是一个单点方法改进。

Q7: 总结一下论文的主要内容

论文的论证主线可以概括为：web-agent 研究习惯用成功率增益来判断一个改动是否值得继续，模型 judge 又让这种比较可以大规模进行；但“一个 agent 排第一”并不等于“它的领先幅度可信”。论文把评估比较本身作为对象，指出两个分数的差可以表现得不同于任一分数：如果一条判定规则同时压低两个 agent 的分数但幅度不等，排名可能不变，而报告的优势会大幅缩水；更反直觉的是，让每个 agent 的分数更准确，可能让二者之差更不准确，因为原来的误差可能部分抵消。由此，论文将比较拆成三个环节：成功如何定义、什么证据支持 judge 的判定、差距如何估计。第一个环节区分“任务可能成功”与“记录支持完成”，并把未解决完成与有支持的失败分开；第二个环节检查执行记录是否包含任务要求、对象与观察之间的必要关系；第三个环节则用点估计、抽样参考标签以及方差/偏差分解来估计差距。
技术主线是固定 AgentRewardBench 的 executions 与 records，只改变 judge 问题：prediction 式问“可能成功吗”，certification 式问“记录支持完成吗”。图示/片段显示，预测式下 Claude 26.67%、Qwen 18.33%，差 8.33 pp；认证式下 Claude 14.50%、Qwen 13.17%，差 1.33 pp，排名仍是 Claude 在前。两个分数都下降，但 Claude 降幅更大，因而领先被压缩。论文进一步构造显式允许 unresolved verdict 的比较，发现缩减仍然保留，说明效应不是简单把未解决完成归为失败。控制性 source addition 实验在原始记录上加入 saved layout records 等来源，恢复位置约束、对象/时间等关系；结果显示，缺失关系既可能掩盖真实差异，也可能制造表面优势，方向取决于具体任务与证据缺口。
实验主线还包括估计与误差分析：从 300 对配对任务中抽取 50 对（100 条 execution）获取参考标签，把 judge predictions 与 reference labels 结合来估计选定的结果差异；比较样本 judge gap 与全队列差距；用 MSE 同时计入 variance 与 squared bias，并做 calibration 与 decompletion controls。论文报告，单 agent 分数更准确并不保证差异更准确，差异误差取决于两个 judge 误差的相关结构。多 judge、多 execution system 的对照（包括 Astra/final、Sonnet 4.5、Kimi K2.5 等，具体角色需查原文）显示出一系列异质响应：效应不是单一 judge 的孤立伪影，但方向和幅度会随 judge/系统变化。
贡献上，论文不是提出新的 web agent 或更强 judge，而是提供一套评估测量学：把“排名—幅度—差距误差”分开，把成功定义与证据要求作为实验变量，把 source addition 作为诊断工具，并把参考标签用于差异估计而非仅用于单 agent 校准。其实践含义是，排行榜或论文比较在报告“A 比 B 高 X 个百分点”时，需要同时说明判定规则、证据标准、未解决判定的处理方式以及差距的不确定性；否则排名稳定可能掩盖优势消失。边界也很明显：研究集中在 AgentRewardBench 的固定 executions、有限 agent 对与 50 对参考标签，额外 judge/系统数量有限；认证式判定可能改变测量对象而非单纯提高严格性；差异估计器依赖参考标签，成本与可扩展性仍需权衡。总体而言，论文的核心信息是“同样的执行，可以得出不同的差距”，因此 claimed advantage 需要自己的检查，而不能从排名或单 agent 准确率自动推出。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你的 agent 方向直接重合：论文讨论 web-agent 评估、LLM-as-a-judge、排行榜与配对比较，适合作为评估协议设计参考。

## 基本信息

- 作者：Zelong Wang, Weiyue Li, Fei Xue, Junru Zhu
- 机构：加州大学圣地亚哥分校（Zelong Wang）；哈佛大学（Weiyue Li）；独立研究者（Fei Xue、Junru Zhu，Seattle）。
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-11-23
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.30190v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要、Introduction 与 Results 片段，并对启发式草稿中的噪声、错位与重复内容做了校正；方法细节处已用“合理推断/需回原文确认”标注证据强度。
