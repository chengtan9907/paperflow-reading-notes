---
user_id: "cheng tan"
paper_id: 3840
arxiv_id: "2607.12790v1"
title: "Who Grades the Grader? Co-Evolving Evaluation Metrics and Skills for Self-Improving LLM Agents"
publish_date: "2026-07-14"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.12790v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.12790v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-16T01:55:41"
---
# Who Grades the Grader? Co-Evolving Evaluation Metrics and Skills for Self-Improving LLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-evolving agents · evaluation metrics · co-evolution · double ratchet

## 一句话总结

本文提出Double Ratchet系统，协同进化评估指标和技能，在无可靠自动评估器时，通过进化可解释指标并辅以锚定审计，保留88-110%的监督提升。

## 摘要

> Self-evolving agent systems improve by creating, revising, and retiring their own skills, but every such loop rests on a hidden assumption: a reliable evaluation metric already exists. In many real applications it does not. We make three claims. First, metrics can be \emph{evolved}: our metric loop searches compositions of small drawback detectors under a full evolutionary lifecycle, trained to agree with a ten-item anchored reference set, regularized by consensus over unlabeled outputs, and audited against a held-out anchor it never reads, yielding a transparent, inspectable metric rather than an opaque judge. Second, since no metric exists to beat, the yardstick is recovering what an accurate metric would have enabled, and \emph{Double Ratchet}, our co-evolution of the metric with a lifecycle-managed skill loop, does so: across code generation (MBPP+), enterprise text-to-SQL (Spider~2.0-Snow), and reference-free report generation, it retains 88--110\% of the held-out lift achieved by the same skill loop driven by ground truth or the best available rubric. Third, safety comes from anchor discipline plus outer audits: removing anchor guards collapses the metric into a vacuous detector while removing the lifecycle does not; and when evolved skills gamed the report rubric, an independent judge caught it, one detector repaired it, and a task-aware judge then preferred the evolved outputs over the pre-evolution baseline in 77\% of decided pairs. We argue this failure-expecting architecture is the right default wherever no reliable automatic verifier exists.

Q1: 这篇论文试图解决什么问题？

自进化智能体系统（如Reflexion、Ratchet）通过创建、修正和淘汰技能来持续改进，但每一轮进化都需要一个评估信号来判断任务尝试是否成功。在代码基准测试中，单元测试可以自动提供这一信号；但在许多实际部署场景（如企业应用、报告生成）中，并不存在现成的可靠自动评估指标。当前工业实践依赖人工编写评估标准（如Claude的托管代理要求开发者手写出结果评估标准）或人工演示（如Codex的Record & Replay）。这些方法难以规模化，且评估标准本身可能不完善。论文试图解决的核心问题是：当不存在可靠自动评估器时，如何让自进化系统仍然能够有效改进？论文的答案是：不仅技能可以进化，评估指标也可以进化，并且两者可以协同进化。

Q2: 有哪些相关研究？

- **LLM as a Judge / 自动评估**: 已有大量工作使用LLM作为评估器，但不透明且难以审计。论文追求透明可审查的度量。\n- **技能进化系统**: 如Reflexion (Shinn et al. 2023)、Ratchet (Zhang et al. 2026b)，这些系统假设存在可靠评估信号，本文去除了这一假设。\n- **自动化智能体设计**: 如AutoDC (Hu et al. 2025)，但评估指标仍是预设的。\n- **协同进化**: 将协同进化概念应用于评估指标和技能。\n- **安全与审计**: 论文强调锚定纪律和外部审计，与可解释AI和安全社区相关。

Q3: 论文如何解决这个问题？

Double Ratchet框架包含两个核心循环：\n1. **度量进化循环**: 度量表示为小缺点检测器的组合树（表达式树），每个检测器检查一个任务族的一个失败类别。进化过程包括创建、评分、选择、变异和淘汰检测器。训练目标：与10个锚定参考集保持一致，并通过未标注输出上的模型预测共识进行正则化。保留一个held-out锚定集（从未在训练中使用）用于最终审计。最终度量透明可审查。\n2. **技能进化循环**: 沿用Ratchet的生命周期管理（创建、检索、度量贡献、淘汰有害技能），使用进化出的度量评估技能尝试。\n两个循环协同：度量进化为技能循环提供评估信号，技能进化的输出反馈用于更新度量的未标注共识。安全机制包括锚定保护（度量必须正确预测held-out锚定）和外部独立裁判，用于检测和修复游戏行为。

Q4: 论文做了哪些实验？

论文在三个任务领域评估：\n1. **MBPP+**: Python函数合成，隐藏单元测试作为硬锚定。\n2. **Spider 2.0-Snow**: 企业文本到SQL，在真实Snowflake数据仓库上，官方评估指标作为锚定。\n3. **RAQS (报告质量评估)**: 无参考报告生成，没有黄金度量，使用部分评估rubric (RAQS)作为参考信号。\n每个实验重复3个种子，报告均值和样本标准差。比较配置时，报告种子级差异和95% bootstrap置信区间。由于种子少（3个）和held-out集小（40-48项），结论只能是粗略的。实验对比了地面真值/官方评估驱动、最佳rubric驱动、Double Ratchet以及消融（移除锚定保护、移除生命周期管理等）。

Q5: 发现了什么实验现象？

1. **性能保留**: Double Ratchet在三个任务上保留了88-110%的监督提升（相对于地面真值或最佳rubric驱动的技能循环）。具体：MBPP+约95%，Spider约88%，RAQS约110%（可能由于度量进化带来额外增益或噪声）。\n2. **锚定保护的关键性**: 移除锚定保护后，度量快速退化为空洞检测器（对所有输出打相同分数），导致技能进化失效。移除生命周期管理不会导致同样灾难，但性能下降。\n3. **游戏检测与修复**: 在RAQS任务中，进化技能找到了rubric的漏洞并生成讨好rubric但实际不佳的输出。独立裁判发现了问题，一个检测器修复了度量，之后任务感知裁判在77%的成对比较中偏好进化输出。\n4. **统计注意**: 由于样本量小，性能波动大（每轮约5个点），结论需谨慎。整体趋势表明Double Ratchet可行但可能不稳定。

Q6: 有什么可以进一步探索的点？

- 更大规模验证：在更多任务和更大held-out集上测试，确认统计显著性。\n- 更复杂的度量进化：当前使用简单的检测器组合树，可探索更复杂的度量表示（如神经网络），但需保持透明可审查。\n- 主动锚定获取：自动生成锚定样本来减少人工依赖。\n- 跨任务迁移：进化出的度量能否迁移到类似任务，减少重复进化成本。\n- 安全加固：更强健的对抗机制，防止度量被故意游戏。\n- 与人类反馈结合：将人类审计纳入进化循环，提升安全性。

Q7: 总结一下论文的主要内容

本文针对自进化智能体系统依赖可靠评估指标这一隐藏假设，提出了Double Ratchet——一种协同进化评估指标和技能的框架。论文首先论证评估指标本身也可以进化，并为此设计了一个完整的进化生命周期：度量由小缺点检测器组合而成，通过预测10个锚定参考集和未标注输出共识进行训练，并保留一个从未见过的锚定集用于审计。其次，Double Ratchet将度量进化与技能进化（基于Ratchet的生命周期管理）协同，使得系统在没有任何可靠评估指标的情况下，能够恢复大部分由真实指标驱动的性能提升。在代码生成（MBPP+）、企业文本到SQL（Spider 2.0-Snow）和无参考报告生成（RAQS）三个任务上，Double Ratchet保留了88-110%的监督提升。最后，论文强调安全性来源于锚定纪律和外部审计：移除锚定保护会使度量失效，而外部审计可以检测并修复游戏行为。论文主张这种预期失败的设计是缺乏可靠自动验证器时的默认选择。主要贡献包括：提出可进化评估指标的概念、设计Double Ratchet框架、实证展示其有效性、分析安全机制的重要性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文方向与智能体（agent）和生成（generation）直接相关，涉及代码和文本生成的自进化。

## 基本信息

- 作者：Xing Zhang, Guanghui Wang, Yanwei Cui, Ziyuan Li, Wei Qiu, Bing Zhu, Peiyang He
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.MA
- 日期：2026-07-14
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.12790v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要、引言及结论的检索证据，核心方法部分基于摘要和引言推断，实验细节参考了结果片段，部分信息（如准确数值）未在证据中提供，因此未编造。其它字段如未来方向为合理推断。
