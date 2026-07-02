---
user_id: "cheng tan"
paper_id: 2156
arxiv_id: "2606.32014v1"
title: "Scalable Behaviour Cloning on Browser Using via Skill Distillation"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.32014v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.32014v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T01:41:40"
---
# Scalable Behaviour Cloning on Browser Using via Skill Distillation

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：behaviour cloning · skill distillation · browser agent · web automation

## 一句话总结

通过将人类浏览器交互轨迹蒸馏为可检索、可组合的自然语言技能，并组织成技能图，浏览器智能体可借助集体人类经验实现显著的行为克隆性能提升，在 WebArena-Hard 上将总体任务成功率从 60.5% 提高到 81.4%（+20.9 个绝对百分点）。

## 摘要

> Internet users collectively perform an enormous range of skilled work through web browsers, from software development and document editing to search, forms, and enterprise workflows, making human browsing a highly scalable but under-exploited source of reusable browser skills. We argue that the bottleneck for browser agents is decision-making under incomplete information rather than low-level operation, and that the priors agents lack are already implicit in human interaction traces. We therefore study scalable behavior cloning for browser agents via skill distillation, converting user interaction trajectories into compact natural-language skills that agents can read, retrieve, reuse, and compose directly. We further organize the distilled skills into a skill graph so that growth proceeds through consolidation rather than unbounded accumulation. This suggests that the scalability of browser agents may come less from manually designed tasks and more from the collective skills already expressed by internet users. Our project is available at: https://lab.einsia.ai/browserbc/.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：如何让浏览器智能体获得可迁移、可复用的决策先验，从而在复杂、多步骤的网页任务中做出有效决策。当前浏览器智能体（如基于多模态大模型的 agent）在低级操作（如点击、输入）方面已较为成熟，但在需要规划、验证进度、识别任务完成、从错误中恢复等方面表现不佳，根本原因是缺乏关于“如何做”的高层任务先验。这些先验其实大量存在于互联网用户日常的浏览轨迹中，但原始轨迹是脆弱的、页面特定的动作序列，直接用于行为克隆会学到表层模式而非可迁移能力。因此需要一种表示方法将人类轨迹抽象为可泛化的技能，同时技能库必须可扩展，避免无限膨胀。

Q2: 有哪些相关研究？

相关工作涵盖三个主要方向：1) 浏览器智能体与网页自动化：以往工作如 WebArena、Mind2Web、WebAgent（Gur et al., 2024）等尝试用大语言模型进行网页任务，但通常依赖手工设计的任务或奖励，扩展性受限；OSWorld 将问题泛化到操作系统层面。2) 行为克隆与模仿学习：传统行为克隆直接在低级动作空间学习，需要大量高质量演示，且面临分布偏移和泛化问题；最近工作尝试将演示抽象为更高层表示，但往往需要手动定义。3) 技能蒸馏与技能库：在机器人领域，技能蒸馏已用于从轨迹中提取可重用动作片段，但浏览器环境中技能表示通常依赖结构化编程而非自然语言；论文借鉴了将经验表示为自然语言的想法（如 Reflexion、SayCan），但强调技能的生产者与执行者解耦，并引入技能图管理。证据还提及 Zheng et al. (2024) 将 grounding 视为视觉语言智能体的关键瓶颈，以及 Xu et al. (2025) 利用可扩展数据的工作。

Q3: 论文如何解决这个问题？

论文提出 BROWSERBC 框架，以技能为中心的模仿学习方法。具体流程：1) 收集人类浏览器交互轨迹（原始动作序列，含点击、输入、导航等）。2) 将每条轨迹蒸馏为一个自然语言技能：技能编码何时适用、如何导航和验证进度、何时终止、如何从错误恢复，形成自包含的描述。蒸馏过程可能涉及 LLM 总结轨迹中的模式。3) 自然语言表示的关键优势：技能的生产者（人类或模型）可以与执行者（智能体）解耦，一个实体完成一次任务后即可蒸馏出技能供其他智能体使用。4) 将蒸馏出的技能组织成技能图（Skill Graph），技能通过相似性连接，新技能可以与已有技能合并或巩固，避免无限制积累；增长通过巩固而非堆积实现。在执行时，智能体根据当前任务和页面状态检索相关技能，读取并组合执行。整体上，该方法将人类集体经验转化为可检索、可组合的技能库，智能体在决策时获得先验指导。

Q4: 论文做了哪些实验？

实验在 WebArena-Hard 基准上进行，该基准包含多个任务组（如购物、多站点、内容管理等），共 258 个子任务。论文比较了基线（推测为没有技能蒸馏的直推动作克隆或原始 VLM 智能体，具体基线名称在证据中未明确）和 BROWSERBC。主要结果：基线总体成功率 60.5%（156/258），BROWSERBC 达到 81.4%（210/258），绝对提升 20.9 个百分点。提升最大的任务组是 Multi-site 和 Shopping，这些任务需要过程性导航、多步搜索和结果解释，对决策先验需求最高。论文还进行了消融分析（证据片段提到“4 3 Analysis And Ablations”），可能包括移除技能图、不同技能表示方式、技能数量影响等，但具体数值未在检索证据中提供。此外，实验可能评估了技能库的规模增长趋势和检索效率。

Q5: 发现了什么实验现象？

从实验结果可观察到以下现象：1) BROWSERBC 带来的提升具有任务依赖性，在需要多步推理和页面间跳转的任务上效果最明显，提示技能蒸馏提供了过程性知识而不仅仅是动作复制。2) 提升幅度较大（+20.9%），表明从人类轨迹中提取的先验对当前 VLM 智能体有实质性帮助。3) 推测实验可能发现随着技能数量增加，已有技能图能够通过合并相似技能维持可控规模，避免检索退化。4) 消融实验可能显示纯行为克隆（不蒸馏或仅 flat action imitation）难以泛化到新页面变体，而技能蒸馏生成的自然语言描述更具鲁棒性。5) 可能存在负结果：对于某些高度特定于站点的任务（如独特 UI 组件），蒸馏出的技能可能覆盖不足。

Q6: 有什么可以进一步探索的点？

论文指出，浏览器智能体的可扩展性可能更多来自集体人类技能而非手工任务设计。基于此，未来可探索：1) 更大规模的技能库构建，利用互联网上海量的匿名浏览轨迹自动生成技能。2) 技能质量评估与验证机制，确保蒸馏出的技能正确且无有害行为。3) 技能图的自适应更新，包括技能合并、遗忘和重写。4) 跨任务泛化，将习得技能迁移到未见过的网站类型或语言环境。5) 结合强化学习，在技能执行后进行反馈学习以微调技能。6) 隐私保护，确保人类轨迹中的敏感信息不会泄露到技能描述中。7) 将技能蒸馏框架推广到其他 GUI 环境（如移动应用、桌面操作系统），作为通用交互技能库。

Q7: 总结一下论文的主要内容

这篇论文提出 BROWSERBC，一个可扩展的浏览器行为克隆框架，核心创新在于利用技能蒸馏将人类浏览器交互轨迹转化为自然语言技能并组织成技能图。论文首先论证了当前浏览器智能体的主要瓶颈是不完全信息下的决策，而非低级操作能力；而人类日常浏览行为 implicitly 包含了所需的决策先验。基于此，BROWSERBC 收集人类轨迹（推测来自志愿者或日志数据），通过 LLM 将每条轨迹总结为结构化的自然语言技能，包含适用条件、导航步骤、验证点、终止条件和恢复策略。自然语言表示使得技能的生产者（例如一个完成任务的用户或专家模型）与执行者（任意智能体）解耦，技能可以被其他智能体直接读取和重用。为了管理不断增长的技能库，论文引入技能图：技能作为节点，语义相似性作为边，新技能通过巩固（consolidation）融入已有节点，避免无限膨胀。在 WebArena-Hard 基准上的实验表明，BROWSERBC 将总体任务成功率从 60.5% 提升至 81.4%，尤其是在过程性强的多站点和购物任务上提升最显著。论文还包含消融分析（具体内容未在检索证据中详细呈现），可能研究技能蒸馏 vs. 原生动作克隆、技能图 vs. 平面技能库、技能数量扩展等维度。论文最后指出，浏览器智能体的可扩展性源自互联网用户已经表达的集体技能，未来可通过更大规模数据采集和技能自动化生成进一步解锁。该工作提供了从人类行为中汲取先验的新范式，区别于传统的手工规则或任务生成方法。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文直接相关于智能体方向（用户权重 0.10），提供了一种利用人类轨迹进行行为克隆的新范式。

## 基本信息

- 作者：Kaisen Yang, Zheng Jiang, Yuzhao Peng, Houde Qian, Boshi Zhang, Youjie Zheng, Shijin Hong, Qingle Liu, Ruoyu Han, Bohan Lyu, Bingxiang He, Eren Cai, Calvin Xiao, Qinhuai Na
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.32014v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（chunk_count=55，包含背景、方法、结果、局限性和相关工作的命中文档），并结合 heuristic_draft 中的信息进行润色和补全，证据不足处已标注“推测”或明确指出信息缺口。
