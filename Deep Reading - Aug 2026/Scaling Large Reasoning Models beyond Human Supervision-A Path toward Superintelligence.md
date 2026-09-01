---
user_id: "cheng tan"
paper_id: 9994
arxiv_id: "2608.31075v1"
title: "Scaling Large Reasoning Models beyond Human Supervision: A Path toward Superintelligence"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.31075v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.31075v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:41:28"
---
# Scaling Large Reasoning Models beyond Human Supervision: A Path toward Superintelligence

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large reasoning models · reinforcement learning with verifiable rewards · reward modeling · self-improving agents

## 一句话总结

本文把大型推理模型（LRM）从 RLVR 走向无人类监督的扩展问题拆解为奖励轴与经验轴，提出 L0–L4 五级监督退场阶梯，并系统梳理奖励黑客、反馈漂移、课程崩溃、环境错误等耦合风险及策略能力、反馈保真度、经验质量三类评估对象，为面向超智能的自持续学习系统提供概念框架与路线图。

## 摘要

> Recent advances in large reasoning models (LRMs) have shown that reinforcement learning with verifiable rewards (RLVR) can substantially improve reasoning in mathematics and code, where outcomes can be checked automatically. Extending this progress to open-ended and agentic tasks remains difficult because reliable rewards are harder to obtain and direct human supervision cannot keep pace with the scale and complexity of model-generated experience. This paper studies how LRMs can continue to improve as human supervision gradually recedes from the learning loop. We examine two connected dimensions of this problem. The reward axis traces the development from per-instance human judgments to reusable verifiers and rewards that operate even without human feedback. The experience axis examines how learning can progress from human-curated tasks and environments toward self-generated curricula, constructed environments, and autonomous co-evolution. We connect these dimensions through a five-level ladder from L0 to L4 that identifies which parts of the learning process remain under continued human control. Our analysis further highlights the risks introduced by increasingly autonomous rewards and experience generation, including reward hacking, feedback drift, curriculum collapse, and environment errors. Consequently, we also provide the evaluation around three complementary objects: policy capability, feedback fidelity, and experience quality. This analysis provides a structured account of current approaches to scaling LRMs beyond human supervision and the open problems involved in developing self-sustaining learning systems toward superintelligence. Furthermore, we maintain a continuously updated \href{https://github.com/visitworld123/Awesome-Scaling-LRM-Beyond-Human-Supervision}{GitHub repository} to track the latest advances.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：大型推理模型（LRM）在数学与代码任务上通过 RLVR 取得成功之后，如何继续扩展到开放域与 agentic 任务，并最终在人类监督逐渐退出的条件下实现持续自我改进。具体障碍有三层：第一，可靠奖励稀缺。数学和代码有可自动验证的结果，但开放域任务往往没有客观答案，逐实例的人类判断成本高、速度慢，无法规模化。第二，经验供给瓶颈。即使奖励问题部分解决，模型还需要大量高质量任务与环境；人工设计任务和环境的产量远低于模型自身生成经验的速度，长尾任务覆盖不足，难以支撑持续学习。第三，监督退场后的系统稳定性问题。随着奖励生成器和任务生成器都走向自主，策略、生成器、评估器三者可能相互适应，产生奖励黑客、反馈漂移、课程崩溃、环境错误等紧耦合故障，导致模型在自身体系内表现良好但在独立标准下停滞甚至退化。论文因此提出一个统一框架，把“人类控制退出的程度”从二值问题变成可分级的问题，并配套给出风险清单与评估体系。其深层动机是把去人类监督视作通往超智能的关键路径：只有让学习系统能够自我产生奖励、任务与环境，才能在人类无法逐例参与的条件下无限扩展能力。该问题兼具科学性与安全性，既涉及如何训练，也涉及如何评价和治理自主进化系统。

Q2: 有哪些相关研究？

论文是综述/立场文体，其“相关研究”即正文归纳的对象。从摘要与语义检索命中的参考文献片段看，它覆盖的工作包括：1) LRM 基础模型：Hui et al. (2024)、Yang et al. (2025a)、Li et al. (2025k)、Chen et al. (2025e)、Achiam et al. (2023，OpenAI o1 相关)、Comanici et al. (2025)、Sun et al. (2025b) 等；2) RLVR 与推理强化学习：在数学与代码任务上利用可验证奖励训练推理模型的系列工作；3) 自举式推理：STaR（Bootstrapping reasoning with reasoning），用推理输出来训练推理能力；4) 无监督推理激励：如 Zhang et al. (2025f) “Right question is already half the answer: Fully unsupervised LLM reasoning incentivization”，探索完全没有人类标注的推理激励；5) 自进化推理模型：R-Zero（Self-evolving reasoning LLM from zero data），从零数据开始自进化；6) 过程奖励模型：VeraPRM（Multi-domain process reward model via synthetic data），通过合成数据构造跨领域过程奖励；7) 开放式进化与自改进智能体：Darwin Gödel Machine（Open-ended evolution of self-improving agents），把 Gödel Machine 思想与开放进化结合；8) 通用 self-play、自博弈、课程学习、LLM-as-judge 与可复用评估器方向。论文将这些分散工作重新定位在奖励轴与经验轴组成的坐标系中，突出“人类控制逐步退出”这条共同线索。

Q3: 论文如何解决这个问题？

论文的解决方案是概念性框架而非具体算法。核心构件包括：1) 奖励轴：从 per-instance human judgments（逐实例人类判断）发展到 reusable verifiers（可复用验证器），再到无需人类反馈的奖励；2) 经验轴：从 human-curated tasks and environments（人工设计的任务与环境）发展到 self-generated curricula（自生成课程）、constructed environments（构造环境）、autonomous co-evolution（自主共同进化）；3) 用 L0–L4 五级阶梯（图 1）刻画学习过程中哪些部分仍处于人类控制之下，把“有无监督”变成渐进量级；4) 提出四类风险作为设计约束：reward hacking（奖励黑客）、feedback drift（反馈漂移）、curriculum collapse（课程崩溃）、environment errors（环境错误）；5) 提出三个互补评估对象：policy capability（策略能力）、feedback fidelity（反馈保真度）、experience quality（经验质量）；6) 强调紧耦合失败模式：策略、生成器、评估器可能相互适应，因此评估必须引入独立于自生成循环的标准；7) 维护 GitHub 仓库持续追踪最新进展。整体方法论的特色在于用双轴加阶梯把“人类监督退场”结构化，从而把零散方法放在同一张地图上比较。

Q4: 论文做了哪些实验？

论文没有报告传统意义上的实验：没有数据集、没有 baseline、没有训练配置、没有定量结果表。证据片段中未出现实验章节的量化内容。它的“实验”实质是文献归纳与框架推演：一是通过整理已有方法在奖励轴与经验轴上的位置来论证 L0–L4 划分的合理性；二是通过失败模式分析提出 policy capability / feedback fidelity / experience quality 三类评估对象。文末维护的 GitHub 仓库承担了持续收集实证进展的功能。因此，应把本文当作概念综述/立场文章来读，而不是实证论文。

Q5: 发现了什么实验现象？

论文没有给出数值实验现象，但提出了若干框架级观察：1) 自主奖励与自生成经验会引入奖励黑客、反馈漂移、课程崩溃与环境错误等风险；2) 当策略、生成器、评估器相互适应时，失败模式是紧耦合的：系统可能在自生成体系内表现良好，而在独立标准下性能停滞或下降；3) 由此，任何单一指标（如模型在自建任务上的准确率）都可能产生误导，评价必须同时关注策略能力、反馈保真度与经验质量三个互补对象。这些属于理论观察与风险预测，而非实验发现。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：1) 将 L0–L4 阶梯操作化：为每一级给出可判定的准则与升级条件，让研究者能定位具体系统处于哪一级；2) 无人类奖励设计：除可验证奖励外，如何为开放域与 agentic 任务构造稳健奖励（过程奖励、世界模型、多智能体互评等）；3) 自生成课程的质量控制：如何避免课程崩溃，维持难度曲线、覆盖度与分布多样性；4) 反馈漂移的监测与校准：评估器漂移时，如何用少量人类锚点或独立基准进行周期性校准；5) 环境错误的检测与修复：自构造环境正确性验证、逃生机制与纠错回路；6) 三对象评估体系落地：为 policy capability、feedback fidelity、experience quality 设计具体指标、基准与协议；7) 紧耦合失败模式的实证研究：测量策略-生成器-评估器共适应程度，观察独立准则下模型退化的时间动力学；8) 超级智能对齐治理：奖励黑客升级为“评估器被策略反制”时的安全约束与不可逆部署问题；9) AI-for-science 场景：自生成实验、自建科学环境在科学发现循环中的适用边界与可靠性保障；10) 从数学/代码到一般长程 agentic 任务的跨域迁移，以及推理能力在不同任务族之间的正负迁移规律。

Q7: 总结一下论文的主要内容

这是一篇面向大型推理模型（LRM）脱离人类监督持续扩展的综述/立场论文。论证主线是：RLVR 已在数学、代码任务上证明基于可验证奖励的强化学习能显著提升推理能力，但这类成功依赖结果可自动校验；转向开放域与 agentic 任务时，可靠奖励稀缺，且人类直接监督的产量无法跟上模型生成经验的规模与复杂度。由此，作者把问题拆成两个相互关联的维度：奖励轴描述从逐实例人类判断到可复用验证器、再到无需人类反馈的奖励的发展路径；经验轴描述从人工任务与环境到自生成课程、构造环境、自主共同进化的路径。两条轴线通过 L0–L4 五级阶梯联系起来，标识学习流程中哪些部分仍处于人类控制之下。论文进一步指出，当奖励与经验来源越来越自主时，会出现四类风险：奖励黑客、反馈漂移、课程崩溃、环境错误；而且由于策略、生成器与评估器相互适应，失败模式会紧耦合——系统可能在自生成体系内表现良好，但在独立准则下性能停滞或下降。为此作者提出围绕策略能力、反馈保真度、经验质量三个互补对象进行评估。整篇论文以概念框架、组织性分析和风险清单为主，没有提供新算法或实验数据；其核心贡献在于为“人类监督如何逐步退场”提供统一坐标系，并把 RLVR、自举推理、自进化模型、开放式进化、过程奖励模型等代表工作重新定位。论文还维护一个持续更新的 GitHub 仓库（Awesome-Scaling-LRM-Beyond-Human-Supervision）追踪最新进展。对后续研究的主要启示是：去人类监督不是二值状态，而是可分级、可评估、可治理的过程；评估必须独立于自生成循环，否则难以发现共适应导致的隐性退化。局限方面，框架的操作性（如何判定某一系统处于哪一级、如何量化反馈漂移等）尚未细化，也未深入讨论超级智能对齐的治理议题。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向直接相关：论文明确覆盖 agentic 任务、自生成课程、构造环境与自主共同进化

## 基本信息

- 作者：Zhiqin Yang, Jingwen Fu, Yuhan Liu, Hengyu Liu, Yonggang Zhang, Kainan Cao, Zizhuo Zhang, Chenxin Li, Ruibin Yuan, Jiahao Pan, Jiankai Sun, Zhenyuan Zhang, Yibo Li, Yunlong Lin, Jing Xiong, Sida Lin, Bo Han, Wei Xue, Yike Guo
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.31075v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于论文摘要与语义检索命中的 Introduction、Challenges and Future Directions 及 References 片段，未获取完整 PDF 正文；其中 L0–L4 阶梯的细节、奖励轴/经验轴的展开等属于基于摘要与现有证据的合理推断或推测。
