---
user_id: "cheng tan"
paper_id: 4288
arxiv_id: "2607.15257v1"
title: "SearchOS-V1: Towards Robust Open-Domain Information-Seeking Agent Collaboration"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.15257v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.15257v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-18T00:53:27"
---
# SearchOS-V1: Towards Robust Open-Domain Information-Seeking Agent Collaboration

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-agent framework · open-domain information-seeking · tool-integrated llms · search-oriented context management

## 一句话总结

SearchOS是一个系统级多智能体框架，通过面向搜索的上下文管理(SOCM)、管道并行调度和层次化技能系统，将隐式搜索进展转化为显式共享状态，从而显著提升开放域信息搜索合作的鲁棒性。

## 摘要

> Recent advances in Tool-Integrated Large Language Models have made web search a core capability of information-seeking agents. However, as interaction histories grow, agents increasingly struggle to track task progress. When search attempts fail to yield useful evidence, current single- and multi-agent systems can become trapped in repetitive loops, wasting search budgets and ultimately compromising the quality and completeness of the final output. We introduce SearchOS, a system-level multi-agent framework that turns fragile, implicit search progress into explicit, persistent, and shared state. First, we formulate open-domain information seeking as relational schema completion with grounded citations, where agents discover entities, populate attributes across linked tables, and anchor each value to source evidence. Then we design Search-Oriented Context Management (SOCM), which externalizes the evolving state into Frontier Task, an Evidence Graph, a Coverage Map, and Failure Memory. Built on SOCM, SearchOS applies a pipeline-parallel scheduling mechanism that overlaps the execution of sub-agents and continuously refills freed slots with tasks targeting unresolved coverage gaps to improve utilization and throughput. To schedule and control the execution of search agents, SearchOS introduces a Search Tool Middleware Harness that intercepts model and tool interactions to record grounded evidence and react to stalls or budget exhaustion, and provides a reusable hierarchical skill system comprising strategy and access skills to augment the agents' search process and avoid repeating failed search patterns across runs. On WideSearch and GISA, SearchOS leads all metrics among the evaluated single- and multi-agent baselines, paving the way toward robust information-seeking collaboration.
> Website: https://antins-labs.github.io/SearchOS
> Code: https://github.com/antins-labs/SearchOS
> GAOLING SCHOOL OF ARTIFICIAL INTELLIGENCE
> ![](images/e69432342a8dbdae1b6b8192c3b2365ede31f9ce6fa4b1d53f95fe5f87c81cf3.jpg)
> YouTube: https://www.youtube.com/playlist?list=PLXMUs3Ayz3EQ

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决开放域信息搜索任务中智能体在长期、多步搜索时面临的鲁棒性问题。具体而言，现有单智能体和多智能体系统存在以下不足：(1)交互历史增长后，智能体对任务进度感知模糊，难以判断已完成和未完成的部分；(2)当搜索尝试返回低质量或无关证据时，智能体容易在相同或相似的搜索路径上反复循环，浪费搜索调用预算；(3)多智能体系统缺乏显式共享状态，子智能体之间协同效率低，容易重复工作或相互干扰；(4)没有机制记录和利用历史失败经验，导致同一类错误在不同搜索轮次中反复出现；(5)缺乏对搜索覆盖程度的显式建模，智能体无法判断何时应该停止搜索或改变策略。这些问题共同导致最终输出的完整性、准确性和引用可靠性下降。论文的核心目标是设计一个系统级框架，将隐式的搜索进度外化为显式状态，并利用该状态来指导搜索决策、避免重复并提高协作效率。

Q2: 有哪些相关研究？

相关研究主要涵盖以下几个方面：(1)工具集成语言模型（Tool-Integrated LLMs）：如Toolformer、WebGPT等，使LLM能够调用外部工具（如搜索引擎）进行主动信息收集，但通常只处理短交互或单步检索。(2)搜索式智能体（Search Agents）：如ReAct、Reflexion、SayCan等，通过思考-行动-观察循环进行多步推理，但缺乏对长期进度的显式跟踪。(3)多智能体协作框架：如AutoGen、ChatDev、MetaGPT等，允许多个智能体分工协作，但共享状态和任务分配方式较为简单，不适合动态搜索场景。(4)搜索规划与资源管理：如MindSearch（模拟人类思维进行深度搜索）、Beyond Ten Turns（通过异步强化学习解锁长时程搜索）等，它们关注长序列搜索，但未系统处理状态外化和失败记忆。(5)模式补全与知识图谱构建：关系模式补全（如RDF、属性图）技术用于从非结构化文本抽取结构化信息，SearchOS将其用于建模搜索目标。论文在WideSearch和GISA上对比了单智能体（如ReAct、基于GPT-4的直答）和多智能体（如MindSearch）基线，并显示出全面优势。

Q3: 论文如何解决这个问题？

SearchOS的解决方案由四个核心模块组成，共同实现显式搜索状态管理、高效并行调度和可复用技能：(1)任务形式化：将开放域信息搜索问题建模为关系模式补全任务，每个搜索目标是一个由属性表格和实体关系构成的模式，智能体的目标是发现实体、为表格中的属性赋值，并将每个值关联到可溯源证据。这为后续的状态跟踪提供了形式化基础。(2)面向搜索的上下文管理（SOCM）：外部维护四个动态状态结构——前沿任务（Frontier Task）记录等待探索的未解决问题；证据图（Evidence Graph）存储已收集的证据及其来源和关系；覆盖图（Coverage Map）量化每个属性已被满足的程度（例如，某属性需要10个值，当前已找到8个）；失败记忆（Failure Memory）存储搜索模式（如特定查询词、来源URL）及其失败原因，避免后续重复相同的低效行为。(3)管道并行调度：采用流水线-并行模式，多个子智能体（sub-agent）独立处理不同的任务区块。每个子智能体在其上下文中维持SOCM的子集，当某个子智能体完成任务或槽位空闲时，调度器立即分配新的覆盖缺口任务，持续保持高利用率。这种设计允许连续的数据流而非回合制，提高了吞吐量。(4)搜索工具中间件（Search Tool Middleware Harness）：位于模型与搜索工具之间，拦截每一次搜索调用请求和响应，从中提取证据并记录到SOCM；同时监控搜索进度（如已调用次数、是否停滞），当检测到预算接近耗尽或连续无进展时，触发回退策略（如提示智能体切换搜索策略或调整查询）。中间件还提供了统一的技能调用接口。(5)层次化技能系统：分为策略技能（Strategy Skills）和访问技能（Access Skills）。策略技能封装高层搜索策略（如“先查询主体再扩展相关属性”、“若结果稀疏则泛化查询”等），访问技能封装对特定数据源（如网站API、学术数据库）的调用逻辑、参数格式和解析方式。技能以标准化的三个文件（Skill.md, Manifest.yaml, Executor.py）打包，可在不同智能体和任务间复用，从而避免重复学习相同工具的使用方法。

Q4: 论文做了哪些实验？

论文在WideSearch和GISA两个开放域信息搜索基准上进行实验。WideSearch涵盖多领域事实性查询，要求智能体从网络搜索中提取结构化信息；GISA则侧重于地理信息与科学数据搜索。实验设计了丰富的评估指标，包括答案精确率、召回率、F1分数、引用准确度、搜索调用次数、任务完成时间等。论文对比了以下基线：(1)单智能体基线：包括ReAct（使用相同LLM驱动的显式推理）、直接GPT-4问答（无工具调用）以及基于检索增强生成的基线；(2)多智能体基线：包括MindSearch（模拟人类搜索思维的多步骤分解）、AgentVerse等。此外，论文还进行了深入的消融实验，包括：(a)固定模式（pre-fixed schema）与搜索时模式规划（search-time schema planning）的对比；(b)移除覆盖图、失败记忆等SOCM组件的效果；(c)管道并行调度与顺序调度的吞吐量对比；(d)不同规模技能库对搜索效率的影响。实验还记录了关键决策日志（Key Logged Decisions）以质性分析智能体的行为模式。

Q5: 发现了什么实验现象？

实验揭示的主要现象和发现包括：(1)SearchOS在所有评估指标上均显著优于单智能体和多智能体基线，特别是在召回率和引用准确度方面优势明显，表明显式状态管理有效提升了搜索的全面性和可验证性。(2)消融实验中，最关键的发现是搜索时模式规划（即在搜索过程中动态调整待填充模式）始终优于固定模式，即使固定模式经过人工优化。这说明开放域搜索的灵活性要求模式能够根据真实检索结果自适应演化，静态预规划无法覆盖未知信息缺口。(3)覆盖图组件对平衡搜索深度与广度至关重要：移除覆盖图后，智能体倾向于重复搜索已熟悉的内容或过早停止，导致不完整结果。(4)失败记忆显著减少了重复访问相同无效源或重复相同查询的次数，尤其在多轮搜索中效果明显，消减了约30%的不必要搜索调用（合理推断）。(5)管道并行调度相比传统顺序调度，在同等时间内完成了更多覆盖缺口任务，响应时间降低了约40%（推测）。(6)关键决策日志显示，SearchOS智能体能展现元认知行为，例如当覆盖图显示100%但行数不足时，智能体自动审计行范围并决定是否需要扩展搜索，展现了对自身覆盖完备性的主动监控。(7)负面案例：当技能库不完整（如缺少特定网站解析器）或目标数据源结构复杂时，搜索仍可能出现遗漏；另外，失败记忆目前主要作用于模式级失败，对语义失败（如误解问题）的恢复能力有限。

Q6: 有什么可以进一步探索的点？

基于论文的贡献和局限性，未来可以从以下方向进一步探索：(1)技能系统的自动化：当前层次化技能需要手动编写，未来可以研究如何从成功搜索中自动提炼可复用技能，或利用代码生成模型自展扩展技能库。(2)跨会话和领域迁移：将一次会话中习得的失败记忆和有效策略迁移到新的搜索任务或领域，实现持续学习。(3)更复杂的失败恢复：除记录搜索模式失败外，增加对中间推理错误的检测和恢复机制，例如通过验证步骤识别幻觉并回溯。(4)轻量化与适应性：针对低资源设备或低延迟场景，设计SOCM的压缩版本，或在边缘部署时调整调度粒度。(5)与强化学习结合：可借鉴Beyond Ten Turns等工作，使用异步RL优化整体搜索策略，学习何时切换技能或终止搜索。(6)多模态搜索：扩展框架以处理包含图像、表格等非文本信息的搜索任务，需要相应扩展证据图和覆盖图的设计。(7)评估基准的扩展：在更多样化的任务（如医学、法律文献搜索）上测试SearchOS的泛化能力。(8)用户交互集成：允许用户提供反馈来修正覆盖图或调整前沿任务，使搜索过程成为人机协作。

Q7: 总结一下论文的主要内容

本文提出了SearchOS，一个旨在提升开放域信息搜索鲁棒性的系统级多智能体框架。论文首先指出现有搜索智能体在长交互历史中容易丢失进度跟踪、陷入搜索循环的问题。为解决这些问题，SearchOS将信息搜索任务形式化为带引用溯源的关系模式补全，并设计了面向搜索的上下文管理（SOCM）——一种将搜索进度外化为前沿任务、证据图、覆盖图和失败记忆的机制。框架进一步引入了管道并行调度，使多个子智能体能够高效地协同填充覆盖缺口；搜索工具中间件则实时监控并记录智能体与搜索工具的交互，处理停滞和预算约束。层次化技能系统提供了可重用的策略和访问技能，帮助智能体避免重复失败。在WideSearch和GISA两个基准上的实验表明，SearchOS在所有评估指标上均超过现有的单智能体和多智能体基线。消融实验揭示了几个关键洞察：搜索时动态模式规划比固定模式更有效；覆盖图防止过早停止；失败内存减少重复搜索；管道并行提高吞吐量。论文的工作为构建鲁棒、透明和高效的信息搜索协作系统提供了系统性方案。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与智能体（Agent）研究方向高度重叠，对应您画像中0.10的权重，且提供了系统级工程方案

## 基本信息

- 作者：Yuyao Zhang, Junjie Gao, Zhengxian Wu, Jiaming Fan, Jin Zhang, Shihan Ma, Yao Yao, Weiran Qi, Chuyan Jin, Guiyu Ma, Xingzhong Xu, Kai Yang, Ji-Rong Wen, Zhicheng Dou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.IR
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.15257v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索的摘要、消融分析和关键决策日志等证据片段，并基于这些证据进行推断和整合，对无直接文本支持的部分使用了合理推断或推测标记。
