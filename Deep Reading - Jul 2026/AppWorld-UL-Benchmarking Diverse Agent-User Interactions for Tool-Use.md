---
user_id: "cheng tan"
paper_id: 5640
arxiv_id: "2607.20536"
title: "AppWorld-UL: Benchmarking Diverse Agent-User Interactions for Tool-Use"
publish_date: "2026-07-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.20536.pdf"
pdf_url: "https://arxiv.org/pdf/2607.20536"
abs_url: "https://arxiv.org/abs/2607.20536"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-24T13:17:47"
---
# AppWorld-UL: Benchmarking Diverse Agent-User Interactions for Tool-Use

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：tool-use agents · agent-user interaction · benchmark · underspecification

## 一句话总结

为了提升工具使用代理与用户交互的评估多样性，我们提出AppWorld-UL，一个包含516个任务的基准，通过系统扰动涵盖不明确、不可行和需批准三种交互现象，并采用受知识边界约束的LLM用户模拟，实验表明最先进的Claude Opus 4.7仅达48.6%成功率，突出交互能力的关键瓶颈。

## 摘要

> Tool-use agents that address day-to-day digital tasks such as ordering groceries must not only operate applications, but also interact with the user, e.g., to ask clarification questions, prompt for confirmation, and inform the user when the instruction is infeasible. However, current benchmarks for evaluating agent-user interactions do not capture the diversity of such interactions. Further, they operate in small environments with few, often non-state-changing, APIs. To address this gap, we introduce AppWorld-UL, a “user-in-the-loop” benchmark of 516 challenging tasks requiring diverse agent-user interactions. Building upon the AppWorld framework with 9 popular simulated apps like Amazon and Spotify, we systematically modify original tasks to introduce ambiguities and constraints that necessitate various types of agent-user interaction. User behavior is simulated by an LLM prompted to respond with carefully designed knowledge boundaries, offering more reliable simulation than the unconstrained or overly rigid alternatives used in prior work. Our evaluation reveals that a state-of-the-art LLM, Claude Opus 4.7, achieves only 48.6% success on AppWorld-UL, and only 35.7% on the harder, compositional subset. On the stricter, scenario-level metric, compositional task performance drops to only 21.3%. Our analysis reveals that correct user-interaction is crucial for success. This demonstrates the benchmark’s difficulty and its potential to advance research on user-in-the-loop tool-use agents. $^{1}$
> ![](images/85a8d779d387a4dedb89b72e75428b446ca7fac4983728d5aee1b9179dd9ecca.jpg)
> Figure 1. Three phenomena that necessitate agent-user interaction in AppWorld-UL. Underspecification: clarifying which path to take when multiple paths are available. Infeasibility: communicating infeasibility when it arises. Need for Approval: Seeking explicit confirmation before executing actions with high cost. Agent-Environment interactions are hidden.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决工具使用代理在真实部署中需要与用户交互，但现有基准无法充分评估这种交互能力的问题。具体来说：(1) 当前基准(如WebArena、OSWorld、AppWorld)主要面向自主任务，假设代理可独立执行，很少或只以简化方式包含用户交互；(2) 即使有交互，其类型也局限于一两种简单场景，缺乏对常见交互现象如任务不明确(underspecification)、不可行(infeasibility)和需批准(need for approval)的覆盖；(3) 已有的用户模拟方法要么过于刚性(规则驱动)无法捕捉真实用户行为变化，要么过于自由(无约束LLM)导致结果不稳定且难以复现；(4) 缺少一个系统化方法来从现有自主任务基准创建交互任务，以及一个能可靠评估交互质量的评估框架。因此，论文旨在构建一个包含多样化、可控交互任务的基准，配套可靠的用户模拟和程序化评估，以准确衡量现有LLM agent在需要用户交互的设定下的表现。

Q2: 有哪些相关研究？

相关工作包括四个方向：(1) 工具使用代理基准：如WebArena、OSWorld、AndroidWorld和AppWorld，它们评估代理在模拟环境中通过API执行任务的能力，但主要面向自主执行，很少涉及用户交互；(2) 交互式基准：如有些工作引入澄清或确认步骤，但交互类型有限，且环境规模小；(3) 用户模拟方法：一些早期工作使用规则或简单模板，缺乏灵活性；近期工作使用LLM模拟用户，但无知识约束导致代理可能收到不一致或超出预期的回复；(4) 人机交互与任务导向对话：研究如何生成澄清问题、处理歧义等，但通常不完全集成到工具使用的代理评估中。AppWorld-UL的独特贡献在于利用扰动方法系统化引入三类交互现象，并通过知识边界约束LLM用户提高可靠性，填补了现有基准的空白。

Q3: 论文如何解决这个问题？

论文提出了一种系统化方法来构建包含交互任务的基准：(1) 基础环境：采用AppWorld框架，提供9个模拟流行应用(如Amazon、Spotify、餐厅预订等)的状态化环境和丰富API；已有AppWorld任务主要为自主任务(代理无需用户交互可完成)；(2) 任务扰动方法：通过一系列精心设计的扰动(perturbation)算子，将原始自主任务转化为需要用户交互的任务。扰动引入三种常见交互需求：不明确(underspecification)使任务描述存在多义性，需要代理向用户澄清；不可行(infeasibility)使任务在环境中无法执行，代理需告知用户并提出替代方案(合理推断)；需批准(need for approval)使某些操作代价较高(如确认下单)，代理必须征得用户明确同意；(3) 用户模拟：使用LLM模拟用户，但引入知识边界(即只提供完成任务所需的部分知识，且不会主动超出该范围)以确保模拟一致性和可靠性，避免无约束LLM的随意性；(4) 评估框架：任务级指标(整体任务是否成功)、场景级指标(交互过程是否正确，如是否及时澄清)以及组合指标；通过状态追踪和程序化验证自动评分。整个过程可扩展应用于其他自主任务基准。

Q4: 论文做了哪些实验？

论文在AppWorld-UL基准上进行了多组实验：(1) 评估了多种前沿LLM作为代理(包括Claude Opus 4.7等，具体列表需参考原论文)在516个任务上的整体性能，报告任务级和场景级成功率；(2) 按交互类型分解：分别测试三类交互(不明确、不可行、需批准)及组合任务的性能，以识别不同交互场景的难度；(3) 对比自主版本：将交互任务还原为无交互需求的自主任务(即去除扰动)，比较代理在同一任务骨架下有无交互需求时的成功率差异，量化交互带来的额外难度；(4) 场景级指标评估：除二元任务成功外，还评估交互行为正确性(如是否在恰当时机澄清、是否错误地假设用户偏好、是否无需批准时错误请求等)，提供更细粒度分析(合理推断)；(5) 可能还进行了用户模拟可靠性验证(如对比不同LLM用户或规则用户的影响)。具体模型和结果详见原论文。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：(1) 性能瓶颈：即使最先进的Claude Opus 4.7在AppWorld-UL整体任务上仅达48.6%成功率，在组合子集上降至35.7%，在更严格的情景级指标上组合任务仅21.3%，表明当前代理在用户交互场景下表现严重不足；(2) 交互是主要挑战：将交互任务还原为自主版本后，平均成功率提升至78.1%(推测来源：原文'to 78.1%')，表明大部分困难源于交互需求而非任务本身复杂性；(3) 组合交互大幅增加难度：需要同时处理不明确和需批准等的任务比单一交互类型任务困难得多，场景级指标尤其低，说明代理在多类型交互循环中容易出错；(4) 错误模式特点：代理常犯的错误包括未能提出澄清问题、过早假设不完整信息、未意识到操作代价需要确认、以及未能有效传达不可行性；不同交互类型的失败模式存在差异(合理推断)；(5) 用户模拟稳定性：受知识边界约束的LLM用户相比无约束版本产生更一致和可预测的回复，有助于公平比较(推测)。

Q6: 有什么可以进一步探索的点？

基于现有工作局限，未来可探索：(1) 扩展至更多真实应用和平台(如本地桌面、手机OS)以增加环境多样性；(2) 细化用户模拟：引入个性化知识边界、情绪、偏好不确定性，使其更贴近真实用户；(3) 多轮交互与动态任务：任务可能会随交互推进而演化，需要代理进行多轮澄清或协商；(4) 将交互能力作为训练目标：在强化学习或示范微调中引入交互奖励，提升代理主动交互能力；(5) 自适应交互策略：代理应能判断何时需交互、选择最优交互类型(如澄清vs.假设)；(6) 自动化任务扰动生成：利用LLM自动生成高质量的交互任务，扩大基准规模；(7) 交互质量的非功能指标：如交互效率、自然度、用户满意度等。

Q7: 总结一下论文的主要内容

这篇论文聚焦于工具使用LLM代理在与用户交互场景下的评估问题。真实代理部署中，代理不仅需操作应用API，还需与用户交互——例如任务阐述不明确时请求澄清、操作代价高昂时需确认、任务不可行时通知用户。然而现有基准(如WebArena、AppWorld)严重缺乏对此类交互多样性的覆盖，且用户模拟方法不稳定或过于刚性。为弥补这一缺口，论文提出AppWorld-UL，一种系统化构建的“用户参与”基准。

构建方法：基于AppWorld框架(9个模拟应用、丰富API体系)，采用扰动算子(perturbation operators)将原有自主任务改造为需用户交互的任务。扰动引入三种典型交互现象：不明确(underspecification)——任务描述存歧义，需代理向用户澄清意图；不可行(infeasibility)——目标在当前状态无法达成，需代理主动告知用户并给出替代方案；需批准(need for approval)——操作涉及费用、隐私等代价，需用户明确同意。这些现象可单独或组合出现，共生成516个任务，其中306个为单一类型、126个为组合类型。

用户模拟：为可靠仿真用户行为，论文设计了知识边界(knowledge boundaries)机制，指导LLM仅基于任务相关的部分知识回复，避免过度配合或超出范围的信息，在刚性与自由度间取得平衡。评估采用任务级(整体成功)和场景级(交互过程正确性)双重指标，并通过状态环境程序化验证。

实验用多个前沿LLM作为代理，在AppWorld-UL上进行全面评估。最佳模型Claude Opus 4.7整体成功率仅48.6%，组合子集35.7%，场景级组合任务仅21.3%。消融实验移除交互需求后，同一任务骨架成功率升至78.1%，清晰表明交互能力是主要瓶颈。不同交互类型难度不等，组合场景尤为严峻。错误分析提示代理常未能及时澄清、误解用户意图、忽略确认需求或传达不可行性不充分。

贡献包括：(1) AppWorld-UL基准，系统涵盖三类交互现象，共516个任务；(2) 可复现的扰动生成方法，适用于其他自主基准；(3) 可靠的LLM用户模拟方法(知识边界约束)；(4) 详实实验揭示当前模型的交互能力缺陷，证明交互是关键挑战。

局限：基准基于模拟应用，尚未涵盖真实设备交互；用户模拟仅是近似，可能缺乏真实交互的微妙性；任务扰动范围有限；指标专注功能正确性，未评估交互自然度等。未来可向多应用、多模态、多轮交互、交互策略学习方向扩展。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文核心关注工具使用智能体的用户交互评估，与智能体研究方向（权重0.10）高度吻合。

## 基本信息

- 作者：Junzhi Chen, Harsh Trivedi, Jane Pan, Michael JQ Zhang, Tejas Srinivasan, Niranjan Balasubramanian, Ashish Sabharwal
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG, cs.MA
- 日期：2026-07-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.20536`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence）及field_evidence_map，并基于heuristic_draft进行了润色和补全，推断内容已标注合理推断或推测。
