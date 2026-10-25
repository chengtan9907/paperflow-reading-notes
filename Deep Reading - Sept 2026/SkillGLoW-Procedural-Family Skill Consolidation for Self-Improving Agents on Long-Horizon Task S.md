---
user_id: "cheng tan"
paper_id: 10450
arxiv_id: "2609.02217v1"
title: "SkillGLoW: Procedural-Family Skill Consolidation for Self-Improving Agents on Long-Horizon Task Streams"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.02217v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02217v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-09T01:18:36"
---
# SkillGLoW: Procedural-Family Skill Consolidation for Self-Improving Agents on Long-Horizon Task Streams

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：skill library · procedural family · self-improving agent · long-horizon tasks

## 一句话总结

本文提出 SkillGLoW（Global–Local Weave），以“程序族”（procedural families）为技能记忆的最小复用粒度，将任务执行中产生的局部技能聚合、压缩为去实例化的全局先验，并在每个任务上再生成实例细节，同时通过提交门控保证先验不损害已部署技能库；在数学推理、终端自动化、软件修复和具身控制四个长时程基准与三个模型上，该方法相对无技能基线平均提升 17.2 个硬指标点，所有 12 个持续改进轮次均为正增益。

## 摘要

> (a) Single Global Document (b) Flat Skill Library
> Granularity: Too Coarse (Over-compressed) Granularity: Too Fine (Instance-overfitted)
> LLM agents increasingly self-improve by writing and reusing Managment
> textual skills, kept either as one global document or as a flat Security Single
> Skill.md Retrieve
> pool of per-task entries, though most of the evidence comes
> Terminal Ops ... Score2026 from domains with structurally similar tasks. On long-horizon
> workloads where each task demands a different solution, the Software Build Tasks Retrieve Edit
> two forms fail in opposite ways: the document collapses into Top-K
> Data Analysis One entry per task, Delete
> generic discipline, while the pool inflates and its entries stay Grows with the stream ......Sep
> bound to the instance that wrote them. We argue the missing Distinct procedures into one document Unbounded growth, noisy retrieval
> 2 unit of reuse is the solving procedure shared by a cluster of
> related tasks, and build SkillGLoW (Global–Local Weave)
> around it: the local skills a task writes from its own execu- (c) GLoW：Global-Local Weave (Ours)
> The Right Granularity: Procedural Families
> tion are aggregated into procedural families and compressed
> into de-instantiated global priors, while the instance detail The Fix - Memory Granularity
> Too Coarse GLoW - The Right Grain Too Fine they hold is regenerated per task rather than stored; a commit One Procedure Many Small Global Document
> Per Family Task-Specific Entries gate admits a prior only when real execution shows it does One[cs.AI] not degrade the deployed library. Across four benchmarks
> (mathematical reasoning, terminal automation, software re- Best Task Family
> pair, and embodied control) and three models, the priors gain Skill.md
> Task Family Procedure
> 17.2 points (hard) over the no-skill baseline on average, with Procedures Averaged Balanced Point Unbounded Registry
> positive gains in all 12 continual-improvement runs, and 18.0 Coarse Fine
> with local regeneration, while the library holds one prior per
> Consolidation Fail
> procedural family, 3.6× more compact than the per-task pool. Group the per-task skills, Keep each family's shared procedure
> Under the same protocol GLoW leads a published single- L1L2L3 Consolidate： G1G2
> document optimizer on 15 of 21 cells. Unmodified, the li- Per-task keep the shared step, Family priors
> Local Skills Group by drop instance detail Commit Pass Global Skills brary lifts success on unseen ALFWorld tasks from 73.9% to procedure Gate
> 83.9%, evidence that what transfers is procedure rather than L1 ... Lk G1 Gk
> task memory.
> The reusable unit = the procedure shared by a family of tasks

Q1: 这篇论文试图解决什么问题？

这篇论文聚焦的核心问题是：LLM 智能体在长时程、异构任务流中进行持续自我改进时，应当以何种粒度、何种形式组织与维护不断生成的文本技能库。现有系统普遍依赖两种组织形式：单一全局文档（single global document）或扁平技能库（flat skill library）。前者的粒度太粗，把所有技能压缩成一套泛泛的规约或纪律，缺乏可操作的具体步骤，导致跨任务检索时无法提供有效指导；后者的粒度太细，每个任务独立写成一条记录，技能库随任务流无限膨胀，且条目与产生它的任务实例过拟合，检索时噪声大、复用率低。论文进一步指出，现有经验驱动方法的证据大多来自结构相似的任务，在异构长时程任务中，跨任务迁移几乎只发生在元层面，无法支撑细粒度技能复用。隐藏的设计假设是：任何组织形式都隐式地假设了一个任务分布——全局文档假设大多数任务共享同一种路径，扁平库假设每个任务相互独立。长时程工作负载两者都不满足。因此需要一种既能保持通用程序、又能容纳实例差异的中间粒度。论文试图回答：如何自动发现“程序族”（procedural family）这一共享单元，并将其压缩为先验，同时避免先验失效和库退化。

Q2: 有哪些相关研究？

相关工作可分为几个脉络。第一，长时程、复杂环境智能体基准与框架，如 Yao et al. 2023（ReAct）、Shinn et al. 2023（Reflexion）、Liu et al. 2024、Mialon et al. 2024（GAIA）、Jimenez et al. 2024（SWE-bench）、Merrill et al. 2026 等，这些工作推动了智能体从短时程封闭任务走向更开放、更持久的设定。第二，从自身执行轨迹中蒸馏可复用技能的系统，如 Zhou et al. 2026a、Lin et al. 2026、Ni et al. 2026，它们强调让模型从成功或失败的经验中写出技能，再以 prompt 注入方式复用（Kang et al. 2026; Yang et al. 2026a; Zhou et al. 2025）。第三，技能库的组织与优化研究：SkillsBench 提供了直接证据，人工策划的技能可将 pass rate 提升 16.2 个点，而模型仅凭任务描述自写的技能反而比无技能基线低 1.3 个点（Li et al. 2026a），说明技能生成必须扎根于真实执行轨迹。本文与其他方法（如 SkillOpt）的区别在于：多数方法优化“技能内容”或“技能选择”，而本文优化的是“记忆粒度”本身——即技能应该以多小的共性单元被存储和复用。论文提出程序族作为介于“全局文档”和“实例条目”之间的平衡粒度，并设计了端到端的凝聚（consolidation）流程。与“元技能”或“元策略”学习不同，本文强调程序族是任务簇共享的、可操作的过程描述，而非抽象的元规则。

Q3: 论文如何解决这个问题？

SkillGLoW（Global–Local Weave）的整体流程可分解为四个主要环节。第一，局部技能生成：每个任务在执行过程中（可能结合成功或失败轨迹）由模型自己写出针对该任务的局部技能（local skill），这些技能保留了完整的实例特化细节。第二，程序族聚合：将任务流中陆续出现的局部技能按照解决过程的相似性聚类，形成“程序族”（procedural family）。这里的关键观察是：一群相关任务共享同一种求解程序，只是输入输出和对象不同，因此可以抽取出共同过程。第三，压缩为去实例化的全局先验：对每个程序族，通过压缩和删除实例绑定（如具体文件名、坐标、API 参数等）生成一个全局先验（global prior），该先验用较少文字描述相同任务族的核心步骤。论文给出的例子是：一个实例技能需要七步、989 个词，并嵌入一张记忆位置表；压缩后的家族先验用四步、440 个词就完成了同一工作的过程描述。实例细节不进入库存储，而是在每个新任务到来时，由模型基于当前任务上下文“重新生成”（local regeneration），从而兼顾了通用性与实例适配。第四，提交门控：一个新生成的先验只有在真实执行中证明自己不会降低当前已部署技能库的表现时，才被允许写入库中；门控基于执行反馈而非文字表面质量。这保证了库的演化是单调或至少非破坏性的。在运行时，模型利用相似度检索（检索片段提到了 text-embedding-ada-002 嵌入和 top-k 设置为 3）来匹配任务与程序族，将家族先验注入 prompt，并让模型在局部再生成实例细节后执行。整个管线支持持续改进：每个任务完成后局部技能被更新，跨任务的程序族被重新聚合和压缩，门控再评估，形成一个可闭环的“写—聚—压—验”循环。

Q4: 论文做了哪些实验？

论文在四个长时程基准上评估 SkillGLoW，覆盖数学推理（mathematical reasoning）、终端自动化（terminal automation）、软件修复（software repair，如 SWE-bench 风格的训练集）和具身控制（embodied control，如 ALFWorld）。实验使用三个模型：从检索片段可见 GPT-5.4-mini、DeepSeek3、MiniMax2（可能是不同规模的模型）。每个模型在每个基准上构成一个独立的持续改进运行（continual run），共 12 个运行；每个运行从头开始，任务流逐步到来。对照条件包括：无技能基线（No-Skill）、SkillOpt†（一种技能优化方法）、GLoW 的单一全局文档变体（GLoW (Global)）、扁平技能库（Flat）、以及 GLoW 的不同家族/局部分支（如“both family arms”）。主表格（Table 1）每个单元格都是同一任务、同一 harness 下的配对测量（paired measurements）。论文还设置了 held-out 集合（Table A18），验证从训练流中凝练的先验是否能够泛化到未见的、不同来源的任务。评价指标采用 pass rate 或类似完成率，并区分硬（hard）指标（如二进制全对）与软指标。压缩率作为一个重要实验指标：比较库中条目数量和总词数，衡量“以小博大”的效益。另外还针对门控（Prior Repair）和局部再生成（local regeneration）做了消融，比较家族先验与实例技能在不同环节的贡献。

Q5: 发现了什么实验现象？

从摘要和检索片段中可以梳理出以下关键实验现象。第一，统计上的显著提升：GLoW 的先验相对无技能基线平均提升 17.2 个硬指标点，且所有 12 个持续改进运行均为正增益，没有出现负向破坏；当加上实例细节的局部再生成后，平均提升进一步达到 18.0 点。第二，压缩效率：在某个任务组上，GLoW 家族先验仅用 0.42 倍的条目数就达到了该组的最高分 0.467，性能匹配一个规模为其 2.4 倍的扁平技能库；另一个例子中，实例技能需要七步和 989 词并附带一张记忆位置表，而压缩后的家族先验只需四步和 440 词，说明压缩并未牺牲关键程序信息。第三，家族化相对于扁平库的系统性优势：论文指出两个家族分支（family arms）的表现都高于四个扁平库臂（flat arms）的中位数，这表明“程序族”粒度本身带来的增益，而不只是压缩的作用。第四，家族划分的性质：片段提到“程序共享是家族如何被划分的属性，而非基准的属性”（Procedure sharing is a property of how the family is drawn rather than of the benchmark），暗示它们的聚类机制能够跨不同基准一致地工作，且基准本身并未刻意设计成便于共享的。第五，Base-only 的效果被描述为“可靠地为正”，可能是说仅仅使用基础先验（不进行局部再生成）也已经能稳定提升，但搭配局部再生成会更好。第六，与 SkillOpt 的局部对比：在部分列上 SkillOpt 的增益不及 GLoW，甚至出现一个 13.2↓1.9 的负向列（可能的语义是某个子指标下降），而 GLoW 在大多数列上一致向上。检索片段也出现了 GPT-5.4-mini 的对比行：No-Skill 各列为 28.1, 75.6, 35.0, 37.4, 42.9, 60.9, 15.1, 30.3, 47.2，SkillOpt† 为 40.6↑12.5, 80.0↑4.4, 40.0↑5.0, 42.4↑5.0, 61.9↑19.0, 74.2↑13.3, 13.2↓1.9, 38.9↑8.7, 52.5↑5.2，而 GLoW (Global) 为 50.0↑21.9, 83.0↑7.4, 45.0↑10.0……（后续数字被截断，但整体趋势明显）。

Q6: 有什么可以进一步探索的点？

基于论文的思路和现有证据，至少可以沿以下方向进一步探索。一、更大规模、更长时间的任务流：当前 12 个运行可能只覆盖了有限的长期效应，是否会在数百个任务之后出现程序族分裂、合并或退化需要继续评测。二、动态家族树：允许程序族之间建立层级关系（如父子、兄弟），支持细粒度共享与粗粒度共享的混合。三、自动门控阈值优化：提交门控目前需要真实执行验证，成本较高，是否可以基于先验的离线评分或不确定性估计来预测其验证效果，从而减少在线验证次数。四、实例细节再生成的鲁棒性：当任务上下文不完整或分布漂移时，再生成是否可能失败；可以引入多候选再生成和选择机制。五、跨模型迁移：一个模型（如 GPT-5.4-mini）凝练出的程序族先验是否可以直接服务于更强或更弱的模型，或需要重新校准。六、与其他记忆系统的结合：如参数化记忆（LoRA、memory networks）、数据库检索增强（RAG）、图式记忆（schema memory）与文本技能库协同。七、与技能优化方法（如 SkillOpt）的兼容：是互为替代还是可以叠加；如何将内容优化与粒度优化解耦。八、程序族划分的可解释性：聚类过程是否能在人类可理解的层面上输出“为什么这些任务共享程序”，以支持审计。九、扩展到多模态和具身任务：当前具身控制（ALFWorld）比较简化，真实机器人上需要处理感知噪声和执行随机性。十、持续学习中避免灾难性遗忘：在压缩和聚合过程中，旧知识可能被新任务覆盖，需要显式的记忆保留机制。

Q7: 总结一下论文的主要内容

论文关注 LLM 智能体在长时程、异构任务流中的自我改进问题。随着模型和 agent harness 的成熟，任务从短时程封闭型转向长时程复杂环境，固定 prompt 和人工技能已不足以覆盖全部情况，因此近年工作开始让智能体从自身执行轨迹中提取可复用技能，并将成功或失败经验写成文本文档，再通过 prompt 注入复用。但“技能以什么形式持续组织”这一基础问题尚未被系统回答。现有实践分为两类：单一全局文档把所有经验压缩成一个长文档，描述过于粗粒度，退化为泛泛的纪律；扁平技能库每个任务一条记录，粒度太细、体积无限膨胀、检索噪声高，且条目与写入它的实例绑定，无法迁移。两种形式都隐式假设了任务分布：全局文档假设任务高度同质，扁平库假设任务完全独立。而长时程工作负载中，任务簇内部共享解决过程、簇间差异明显，因此需要一个新的记忆粒度。

论文提出“程序族”（procedural family）作为共享的求解程序单元，并围绕它构建 SkillGLoW（Global–Local Weave）系统。系统的工作流是：每个任务先写出自己的局部技能，保留完整实例细节；然后按过程相似性将局部技能聚类成程序族；每个程序族被压缩为去实例化的全局先验，只保留通用的过程步骤，删除具体对象、位置和记忆表等实例特化信息；实例细节不被存入库，而是在每个新任务上由模型重新生成。为了确保库的持续改进不发生倒退，系统还设置了一个提交门控：只有真实执行证明先验不降低已部署技能库性能时，才允许写入。运行时，通过嵌入检索相关程序族，将先验注入 prompt，并由模型补充实例细节后执行。

实验在数学推理、终端自动化、软件修复、具身控制四个基准上，使用三个模型进行 12 个独立的持续改进运行，并与无技能基线、SkillOpt、单文档技能库、扁平技能库等对比。主要结果：程序族先验平均带来 17.2 个硬指标点的提升，且 12 个运行全部为正；加入局部再生成后提升到 18.0 点。压缩效率显著，一个任务族用 0.42 倍的库条目数达到 0.467 的最高分，性能匹配一个 2.4 倍大小的扁平库；压缩后的先验用 440 词和四步取代了实例技能的 989 词和七步。家族臂的表现一致优于扁平臂的中位数，说明程序族粒度的优势不是压缩带来的偶然结果。此外，12 个运行均为独立随机种子或独立连续流，配对测量保证了统计可比性。总体而言，论文的核心贡献在于把“记忆粒度”本身作为设计对象，证明程序族是持续改进智能体在异构长时程任务流中一个自然且高效的复用单位，并提供了完整的自动发现、压缩、门控和再生成机制。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户画像中的“智能体”（agent，权重 0.10）方向直接重合，论文研究 LLM 智能体的持续自我改进与技能库管理。

## 基本信息

- 作者：Ao Yan, Xin Zhang, Jiawei Du, Joey Tianyi Zhou
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-09-02
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.02217v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索到的 Introduction 片段及摘要，但检索片段包含大量表格残片和公式噪声，部分细节（如具体聚类算法、门控实现方式）基于摘要与合理推断，建议以原文为准。
