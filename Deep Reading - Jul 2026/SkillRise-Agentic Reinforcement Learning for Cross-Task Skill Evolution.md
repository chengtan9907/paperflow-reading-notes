---
user_id: "cheng tan"
paper_id: 5984
arxiv_id: "2607.26784v1"
title: "SkillRise: Agentic Reinforcement Learning for Cross-Task Skill Evolution"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26784v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26784v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:14:07"
---
# SkillRise: Agentic Reinforcement Learning for Cross-Task Skill Evolution

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · skill learning · large language model agent · cross-task transfer

## 一句话总结

SkillRise是一个面向大型语言模型代理的统一强化学习框架，通过组织相关任务序列并交替执行任务解决与技能文档策展，实现跨任务技能学习与迁移。

## 摘要

> Large language model agents often encounter related yet distinct tasks that share reusable solution patterns. Yet standard agentic reinforcement learning treats tasks as independent episodes, while existing approaches to skill learning either focus on repeated attempts of one task or use pipelines with multiple stages that entangle extraction, retrieval, and execution. We introduce SkillRise, a unified reinforcement learning framework for learning skills across tasks. SkillRise organizes related instances into progressively challenging sequences and uses a single policy to alternate between task solving and curating an evolving skill document passed directly to the next task. Decoupled credit assignment across tasks supervises solving with the current task outcome and curation with discounted downstream outcomes. Experiments on ALFWorld, WebShop, and ScienceWorld show that SkillRise achieves the strongest Pass@1 performance among the compared methods, with gains over the strongest baseline ranging from 2.3 to 8.5 percentage points. Although trained across distinct tasks, its learned curation policy remains effective for repeated attempts on the same task. Further analysis reveals scaling at test time across tasks: performance improves with longer sequences of related tasks even when each task is attempted only once. This trend suggests that SkillRise reuses transferable skills across tasks rather than benefiting from repeated sampling of the same task. SkillRise further retains strong performance while substantially reducing the runtime overhead of skill learning pipelines with multiple stages. Together, these results provide a simple and efficient training paradigm for LLM agents to extract, refine, and reuse transferable skills across tasks. Our code is publicly available at https://github.com/Within-yao/SkillRise.

Q1: 这篇论文试图解决什么问题？

大型语言模型代理在执行相关但不同的任务时，往往存在可复用的解决方案模式。然而，当前面临两个主要缺陷：第一，标准智能体强化学习将每个任务视为独立情节，无法跨任务积累和复用技能；第二，现有技能学习方法要么局限于单任务的反复尝试（如通过重试机制），要么采用多阶段管道（先收集轨迹，再提取技能，最后存储并检索），这些方法不仅复杂，而且技能提取与执行耦合，难以归因技能本身的贡献，同时引入显著的计算开销。因此，需要一个统一、端到端且高效的框架，使得代理能够从一系列相关任务中持续提取、精炼并复用可迁移技能。

Q2: 有哪些相关研究？

相关研究分为几个方向：(1) 标准智能体强化学习方法（如PPO）将每个episode独立处理，未在任务间共享知识。(2) 单任务重复尝试方法：通过多次采样同一任务来提高成功概率，但无法将技能泛化到新任务。(3) 多阶段技能学习管道：典型做法包括先由代理收集交互轨迹，再使用专门的模块（如总结或聚类）提取技能，并存储在外部数据库中，后续任务检索应用。这类方法将技能提取、存储和检索解耦，但每个阶段各自独立训练，且技能质量与下游任务表现强耦合，导致端到端优化困难，同时附加阶段增加了推理时复杂度和延迟。SkillRise与所有这些不同，通过单一策略同时实现任务解决和技能策展，利用跨任务信用分配端到端优化，避免了外部技能管理设施。

Q3: 论文如何解决这个问题？

SkillRise框架包含三个核心组件：(1) 任务序列构建：将相关但不同的任务实例按难度递增排列成有序序列，序列的长度和内容在训练和测试时可变。(2) 单一策略双角色：一个策略网络交替扮演两个角色——解决者（Solver）根据当前任务指令和不断进化的技能文档产生动作，策展者（Curator）在完成一个任务后，将其交互轨迹总结成新的技能条目并追加到技能文档中。文档在整个序列中持续传递，无需外部数据库。(3) 解耦跨任务信用分配：解决者的训练信号来自当前任务的成败结果，策展者的训练信号来自折扣后的下游任务累积结果，以此激励策展者写出对未来任务有用的技能。优化采用角色感知组相对优化（Role-Aware Group-Relative Optimization，RAGRO），从同一组轨迹中为两个角色分别计算优势并更新。整个流程端到端训练，无需预定义技能模板或额外监督。

Q4: 论文做了哪些实验？

实验在三个典型指令遵循型环境中进行：ALFWorld（多房间物体操作）、WebShop（在线购物任务）和ScienceWorld（科学实验模拟）。对比方法包括：(1) 标准强化学习（以PPO-based方法为代表），(2) 单任务重复尝试（同一任务多次采样），(3) 多阶段技能管道（如SkillSelector等）。评估主要指标为Pass@k（k=1,2,3），即在前k次尝试中至少一次成功的概率。所有方法使用相同的LLM backbone（推测为开源模型如Llama系列，论文中具体型号待原文确认）。训练时，每个训练序列包含多个任务实例，按难度排序。测试时分别评估跨任务序列（每个任务仅尝试一次）和同任务重复（同一任务多次尝试）场景。运行时开销方面，比较了推理时间和整体计算成本。

Q5: 发现了什么实验现象？

关键实验现象包括：(1) 跨任务迁移优势：在所有三个基准上，SkillRise的Pass@1均显著优于所有基线，绝对提升在2.3到8.5个百分点之间，证明跨任务技能学习有效。(2) 多次尝试的稳定性：在Pass@2和Pass@3上也达到最高值，例如在ALFWorld中Pass@3达到92.2%，WebShop中96.1%，ScienceWorld中61.0%，表明技能不仅提升首次成功率，对后续尝试也有帮助。(3) 测试时跨任务缩放：增加序列中相关任务的数量（即提供更多利用技能的机会）时，即使每个任务只执行一次，Pass@1也随序列长度单调上升。这明确表明性能提升来自技能在不同任务间的转移复用，而非同一任务重复采样的获益。(4) 同任务泛化：即使仅跨不同任务训练，策展策略在同任务的多次尝试中依然保持高效，说明学到了可迁移的通用策略。(5) 效率优势：相比多阶段管道，单轮序列的运行时间显著减少，因为消除了外部检索和技能重建步骤，同时性能持平或更优。消融研究（需原文确认）可能进一步验证了策展信用分配权重的作用。

Q6: 有什么可以进一步探索的点？

未来可能探索的方向：(1) 扩展到更广泛、更多样的任务域，检验方法的泛化边界和对无关任务的鲁棒性。(2) 任务序列自动构建与自适应排序，以最大化技能积累效率。(3) 技能文档的显式分析：模型学到了何种形式的可迁移知识（如可重用的动作序列、子目标抽象、环境规律）。(4) 与其他强化学习技术结合，如基于模型的规划、分层强化学习，进一步提升长序能力。(5) 将SkillRise应用于交互式对话或具身推理等更开放场景，并研究多重技能文档并行维护。(6) 理论化分析信用分配的有效性和技能收敛条件。

Q7: 总结一下论文的主要内容

本文提出SkillRise，一个面向LLM代理的统一强化学习框架，旨在解决相关任务间的技能迁移学习问题。现有方法要么将任务独立处理，要么使用繁琐的多阶段管道。SkillRise通过三个关键设计实现端到端跨任务技能学习：(1) 将任务按递增难度组织为序列；(2) 单一策略交替扮演任务解决者和技能策展者，持续进化一个技能文档；(3) 解耦信用分配分别优化解决和策展行为。在ALFWorld、WebShop、ScienceWorld上的实验表明，SkillRise在Pass@1上超越最强基线2.3-8.5个百分点，并展示出跨任务测试时缩放特性，即增加相关任务数量提升单次尝试成功率。同时其运行效率显著优于多阶段基线，且学到的策展策略对同任务重复尝试有效。这些结果为LLM代理跨任务提取、精炼与复用可迁移技能提供了简单高效的训练范式。项目代码已开源。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接聚焦智能体强化学习，与用户研究方向中的agent高度相关（权重0.10）

## 基本信息

- 作者：Zhiyuan Yao, Yuxin Chen, Zhengxi Lu, Zishan Xu, Yueqing Sun, Yifu Guo, Yuquan Lu, Zhengzhou Cai, Kangning Zhang, Zhuowen Han, Zi-Han Wang, Ziang Ye, Qi Gu, Xunliang Cai, Weiwen Liu, Yongliang Shen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26784v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索到的evidence片段，并依据field_evidence_map对各字段进行了证据优先的组织与改写，确保信息准确且引用原文核心观察。
