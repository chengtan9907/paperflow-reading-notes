---
user_id: "cheng tan"
paper_id: 14344
arxiv_id: "2609.37923v1"
title: "EpiCon: Collective Agent Learning through Co-Evolving Multimodal Memory"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.37923v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37923v1.pdf"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-09T12:57:26"
---
# EpiCon: Collective Agent Learning through Co-Evolving Multimodal Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-agent systems · multimodal memory · collective learning · experience reuse

## 一句话总结

提出 EpiCon，一种通过共享多模态外部记忆实现智能体集体学习的框架：两个独立训练的 2B 模型（记忆控制器和树状自组织器）分别负责问题内文本/视觉记忆的协同精炼与跨问题/跨系统的经验库维护，在不更新宿主模型参数的前提下提升多种 MAS harness 与 backbone 的表现。

## 摘要

> Agents can learn from past executions, but enabling different agents to reuse and build on one another's experience remains challenging. We introduce EpiCon, a shared multimodal memory framework for agent collective learning without updating host model parameters. EpiCon links question-level memory evolution to a persistent experience bank through two independently trained 2B models: a memory controller and a tree self-organizer. The controller jointly refines textual guidance and visual evidence across attempts and selectively includes visual memory. The self-organizer consolidates lessons hierarchically and retrieves experience and rules for new problems. We evaluate EpiCon on eleven benchmarks spanning four multimodal task domains, using two harnesses and multiple backbones. A frozen bank improves other systems even with a single solving attempt. A second harness raises the original system's macro-average score by 2.6 points across eleven benchmarks. Across four host configurations, EpiCon improves macro-average scores by 1.7 to 4.9 points over No Memory and reduces memory-operation time by 67\% to 74\% relative to backbone-sized memory models.

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：多智能体系统（MAS）在执行过程中会产生大量关于解题步骤、失败模式和证据选择的经验，但这些经验通常封闭在单次运行或单一系统内部，难以被其他智能体系统复用，也难以在同一系统的后续任务中持续积累。现有外部记忆可以在不更新宿主参数的情况下提供经验，但大多停留在文本层面或单一任务的笔记结构；多模态（尤其视觉）证据与文本指导常常需要跨尝试协同修订，而累积下来的经验还需要层次化整合，才能被后续检索。

具体而言，论文试图回答：（1）如何在单道题内同时更新文本指导与视觉证据，而不是只更新文本笔记；（2）何时应该把视觉记忆纳入经验库，避免噪声或无关图像区域干扰；（3）如何在跨问题尺度上把分散的教训整理成可检索的层次结构；（4）如何保证共享经验库不绑定某一套 harness 或 backbone，从而支持不同系统之间互相借鉴、共同演进。EpiCon 把“反馈驱动的多模态记忆精炼”和“可被不同 agent 系统复用并继续更新的共享经验库”连接起来，这是论文认定的空白点。

Q2: 有哪些相关研究？

论文的相关工作可分为三层。一是语言模型驱动的多智能体系统（MAS）：多个 agent 通过推理、工具使用与协作完成复杂任务，执行过程本身产生可被外部记忆利用的经验（Wu et al., 2023; Fourney et al., 2024; Hua et al., 2024b; Zeng et al., 2026a; Lin et al., 2026）。二是外部记忆/经验复用：在不更新宿主模型参数的情况下，把历史执行经验注入后续推理（Wang et al., 2024b; Ouyang et al., 2026）；更近期的工作通过链接笔记、层次化经验结构或共享记忆库让经验在不同模型/框架间复用（Xu et al., 2026; Zhang et al., 2026b; Tang et al., 2025; Chang et al., 2026）。三是多模态记忆：在文本指导之外保留视觉证据（Zeng et al., 2026c）。

EpiCon 的评测直接与四个外部记忆系统对比：Mem0、Cognee、A-Mem、Agent-KB。按照论文引言的自述，现有工作尚未把“反馈驱动的多模态记忆精炼”与“可持续被不同 agent 系统复用和更新的共享经验库”系统地连接起来；EpiCon 正是补这个缺口。相关引用编号以原文为准，检索片段只提供了这些工作的名称和大致方向。

Q3: 论文如何解决这个问题？

EpiCon 的核心是“专用记忆 harness + 两个独立训练的 2B 模型”。Memory Controller Cθ 负责问题内的记忆演进：在多次尝试中联合更新文本指导与视觉证据，并决定哪些视觉记忆值得保留。Tree Self-Organizer Sϕ 负责维护持久经验库 B：把累积的 lessons/revisions 进行层次化组织、去冗余和合并，并在新问题到来时检索相关经验与规则。两个模型独立训练，不改变宿主 MAS 的编排或 backbone 参数。harness 负责协调记忆更新、检索、视觉裁剪和存储。

更新过程可形式化为 B(k+1) = Sϕ(B(k), Uk)，其中 k 是维护步骤而非单个问题/求解尝试；Uk 是第 k 批被接受的 lessons 与 revisions。这样，经验可以跨任务逐步积累。训练监督来自 MAS 轨迹，例如用 CAMEL 采集轨迹，构造 memory update、lesson 和 tree operation 的监督信号。

EpiCon 的“共享”体现在两个层面：一是同一 MAS 内部，历史 bank 可以被后续问题检索；二是不同 MAS 之间，一套系统可以使用另一套系统留下的 bank，也可以继续向其中贡献新经验，而宿主模型参数始终不变。实验设置还区分了“冻结 bank（仅复用历史经验）”和“启用问题级更新（允许当前问题修改记忆）”，用于分离历史经验复用与在线记忆演进的贡献。

Q4: 论文做了哪些实验？

实验覆盖 11 个基准、4 类多模态任务域：文档理解（DocVQA2026、MP-DocVQA、ParseBench）、视觉到代码（Vision2Code、Omni-I2C、ChartMimic）、视觉数学（MATH-Vision、WeMath、WorldBench）以及通用视觉语言推理（ReasonMap、BabyVision）。评测总量为 4,538 道题。

实验配置包括两套 MAS harness：Codex 和 DeepSeek-Harness；backbone 包括 Qwen3.8-27B 和 Gemma4-31B，另外用 Codex+GPT-5.6-Luna 做“更强求解器”泛化测试。Baselines 包括 No Memory、Mem0、Cognee、A-Mem、Agent-KB。外部记忆系统使用对应 backbone 零样本执行记忆操作；EpiCon 有两种变体：backbone-sized 变体用同一 backbone 做记忆控制与树组织，2B 变体用两个独立训练的 2B 模型。

实验协议上，No Memory 在每道题上只做一次求解尝试；主比较中，每个基于记忆的方法用同一批基准中与评估问题不相交的 1,000 个 construction questions 构建自己的 bank。被评估的 MAS 在冻结 bank 下执行一次 solve，并禁止问题级更新，以检验历史经验复用（对应表 2）。另有共享至多 5 次尝试预算的设置，视觉记忆可冻结或演进。论文还把 MAS 执行时间和 token 用量与记忆操作分开统计，以刻画记忆模块本身的成本。更强求解器泛化实验对应表 7，显示在四个基准上均有提升。

Q5: 发现了什么实验现象？

实验揭示的主要现象包括：
1. 冻结 bank 的可迁移性：即使关闭问题级记忆更新、只让被评估 MAS 单次求解，来自其他系统构建的历史 bank 也能带来提升，说明跨系统经验复用不依赖在线更新。
2. 集体学习的互惠性：另一套 harness 使用原始系统留下的 bank 后，又能贡献新经验反过来提高原始系统，形成循环改进（结论段明确支持）。
3. 轻量记忆模型的性价比：2B 控制器/组织器相对 backbone 规模记忆模型将记忆操作时间降低 67%–74%，同时宏观分数仍获提升，说明记忆模块可以在小模型上承担，而不必动用完整 backbone。
4. 更强求解器仍有收益：Codex+GPT-5.6-Luna 上四个基准全部提升，检索片段提到 ParseBench 从 63.8 提升到 66.5；该数值来自启发式草稿而非直接检索到的表格正文，建议回原文核对表 7。
5. 指标分离的张力：论文刻意把记忆操作时间/token 与 MAS 执行分开统计，说明记忆系统的工程成本是重要评价维度；不同设置共享至多 5 次尝试预算，而 No Memory 单次求解作为对照，比较时需注意预算不对等带来的影响。
6. 未看到显式负结果，但跨域提升幅度不一（合理推断），提示 bank 的收益可能依赖任务分布与历史经验的相似性。

Q6: 有什么可以进一步探索的点？

论文没有系统讨论未来工作，但结合方法和结果可以提出多条可探索方向：
1. 跨领域/跨分布迁移：当前 bank 由同一批基准的 construction questions 构建，可进一步测试从不同任务域或不同语言数据构建的 bank 能否迁移。
2. 长期持续学习：把维护步骤 k 扩展到超长时间跨度，研究经验库的容量控制、遗忘、冲突消解和陈旧经验失效问题。
3. 记忆训练与宿主训练的联合：当前不更新宿主参数，可考虑在保持共享 bank 的同时，用 bank 反哺宿主指令微调或强化学习。
4. 视觉记忆的选择与裁剪策略：记忆控制器“选择性纳入视觉记忆”的具体准则值得深入分析，例如如何避免无关图像区域污染经验库。
5. 多智能体博弈场景下的安全与隐私：共享 bank 可能被某套系统注入误导经验，需要考虑可信度评估、来源追踪和对抗鲁棒性。
6. 理论与实践：树自组织器的层次化整合目标函数、检索与更新的权衡目前偏工程，可建立更形式化的集体学习理论。
7. 科学应用：将共享多模态记忆用于科学工作流（如实验记录、图表理解、多步骤建模）时，需要验证本文结论是否迁移到科学数据分布。

Q7: 总结一下论文的主要内容

背景与问题：多智能体系统（MAS）在解决复杂任务时会产生大量执行经验，包括解题步骤、失败模式和关键证据。EpiCon 的目标是让这些经验不仅能服务当前系统，还能被其他 harness/backbone 组合复用，并在复用过程中继续积累，从而实现“集体学习”，同时完全不更新宿主模型参数。

方法：EpiCon 由专用记忆 harness、一个 Memory Controller Cθ 和一个 Tree Self-Organizer Sϕ 组成，后两者都是独立训练的 2B 模型。控制器在单道题内部联合更新文本指导与视觉证据，并选择性保留视觉记忆；组织器维护一个跨问题、跨系统的持久经验库 B，把被接受的 lessons 和 revisions 层次化整合，并在新问题上检索经验与规则。维护过程写成 B(k+1) = Sϕ(B(k), Uk)。训练监督从 MAS 轨迹构造，例如使用 CAMEL 采集轨迹生成更新、教训和树操作监督。宿主 MAS 的编排和 backbone 参数保持不动。

实验：在 11 个基准、4 类多模态任务域上，用两套 MAS harness（Codex、DeepSeek-Harness）和多组 backbone（Qwen3.8-27B、Gemma4-31B、GPT-5.6-Luna）评测。Baselines 包括 No Memory、Mem0、Cognee、A-Mem、Agent-KB。主比较中，各记忆方法使用 1,000 道与评估集不相交的 construction questions 构建自己的 bank；评估时被测试 MAS 单次求解并使用冻结 bank，以检验历史经验复用。另有共享至多 5 次尝试预算、视觉记忆冻结/演进的设置。实验还单独统计记忆操作的时间与 token 成本。

关键结果：冻结 bank 即使只配合单次求解也能提升其他系统；另一套 harness 使用原始系统的 bank 后又能反向贡献，把原始系统的宏观平均分提高 2.6 分；在 4 种宿主配置下，EpiCon 相对 No Memory 的宏观平均分提升为 1.7–4.9 分；相对 backbone 规模记忆模型，记忆操作时间降低 67%–74%。在更强求解器 GPT-5.6-Luna 上，四个基准均有提升（如检索草稿提到的 ParseBench 63.8→66.5，待核对表 7）。

结论：EpiCon 验证了“经验可以超越生成它的系统被其他系统使用，并且其他系统还能贡献新经验反过来改进原系统”的集体学习路径，证明共享多模态外部记忆是让多个 agent 系统共同演进的有效机制。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户当前“agent”方向直接相关（画像权重 0.10），聚焦多智能体记忆、经验复用与集体学习。

## 基本信息

- 作者：Ziyun Zeng, Hang Hua, Shaden Alshammari, Rogerio Feris, William T. Freeman, Jiebo Luo
- 机构：未提供
- 来源：arxiv
- 主题/分类：high_relevant
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37923v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 method、results、conclusion 片段，并忽略了背景检索中的 OCR 噪声；heuristic_draft 中未经检索片段直接确认的数值（如 ParseBench 63.8→66.5）已在对应字段标注为待核对。
