---
user_id: "cheng tan"
paper_id: 5999
arxiv_id: "2607.26637v1"
title: "Filesystem-Based Memory for LLM Agents: Organization, Evolution, and Sustainability"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26637v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26637v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:18:07"
---
# Filesystem-Based Memory for LLM Agents: Organization, Evolution, and Sustainability

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：filesystem-based memory · LLM agents · long-term memory · memory organization

## 一句话总结

本文首次系统性地探索了基于文件系统的LLM智能体长期记忆，揭示了组织能降低检索成本但当前智能体无法利用组织提升答案质量，且工具集对存储形状的影响与模型更换相当。

## 摘要

> Deployed LLM agents increasingly keep their long-term memory as a filesystem: a directory tree of markdown files that the agent itself reads, writes, and reorganizes through generic file tools. Yet research has largely passed over this medium: prior systems design bespoke memory representations and study retrieval over them, leaving the default's two working assumptions untested: that an agent can keep a growing store organized as memories accumulate, conflict, and go stale, and that this organization pays. We present the first systematic exploration of filesystem-based memory for LLM agents. We formalize the setting as three roles around one memory filesystem: a management agent integrates and organizes incoming content, a search agent answers queries with cited sources, and an execution agent supplies task trajectories that are distilled into skills, unifying declarative memory and skills in a single store. Across long-conversation benchmarks and embodied tasks, we vary memory shape (agent-organized hierarchy, verbatim dump, chunk retrieval), stream scale, tool harness (sandboxed shell, memory-tool-style functions, varied search tooling), and the strengths of the management and search agents, tracking answer quality, cost, and store health as memory grows. What organization reliably buys is search economy: organized stores roughly halve retrieval cost where material is large. Today's agents, however, fall short of the default's promise: in our growth study, organization erodes for all but the strongest management agent, and no agent we measure converts organization itself into better answers. And the model is not the only lever over a store's shape: changing the tool set alone reshapes the store as strongly as swapping the model. The study turns the filesystem default from an assumption into a design space for agent memory.

Q1: 这篇论文试图解决什么问题？

论文旨在验证当前LLM智能体长期记忆领域一个被广泛采纳但未经检验的实践——即由智能体自主读写和维护的文件系统（目录树+Markdown文件）是否具备可持续性与效用。具体解决两个关键问题：（1）随着记忆不断累积，智能体是否能够自动保持文件系统的有序性，避免冲突、重复和过时？（2）这种组织性是否能为下游任务（如问答、任务执行）带来实质性的收益（如更低的成本或更高的准确率）？现有研究工作主要集中设计专属的记忆表示（如向量数据库、缓存、知识图谱）并与检索机制耦合，缺少对文件系统这一“默认方案”的系统性剖析。本工作为这两个假设提供了首批实证证据。

Q2: 有哪些相关研究？

相关研究可分为三类：（1）定制化记忆系统：如MemGPT（带层次化记忆的对话智能体）、Generative Agents（存档型记忆）、Reflexion（反思+经验缓存）等，均设计了与任务耦合的记忆结构，并在特定场景中优化了检索或合并策略。（2）检索增强生成（RAG）：利用外部知识库辅助LLM生成，但通常记忆是静态或预先定义的，而非动态演化的。（3）智能体长期记忆的工程实践：工业界多采用文件系统直接存储任务日志、知识等，但缺乏对其组织与演化规律的理论分析。本文首次将文件系统作为独立研究主题，填补了定制系统与无结构存储之间的空白。

Q3: 论文如何解决这个问题？

论文采用系统性实验研究（empirical study）的方法论，而非提出新的算法或系统。核心步骤如下：（1）形式化三个智能体角色：管理智能体（负责接收新内容并整合到文件系统中，如创建、移动、删除文件）、搜索智能体（给定问题，检索文件系统并生成带引用的答案）、执行智能体（在技能设置中，执行任务并产生轨迹，这些轨迹被蒸馏为技能文件）。（2）定义两种任务范式：长对话任务（需维护历史信息）和具身/技能任务（需从成功轨迹中学习技能）。（3）设计实验条件：记忆形状（agent-organized hierarchy、verbatim dump、chunk retrieval）、流规模（逐步增加记忆量）、工具集（sandboxed shell、memory-tool-style functions、varied search tools）、智能体能力（不同LLM模型）。（4）观测指标：答案质量（准确率或任务成功率）、成本（交互轮次、token消耗、读取内容量）、存储健康（早期记忆存活率、更新正确性）。实验追踪存储随时间的生长曲线，而非仅比较端点结果。

Q4: 论文做了哪些实验？

论文实验覆盖两大设置：（1）长对话场景：使用多会话对话数据集（推测如Multi-Session Chat等），测试不同记忆组织方式下智能体在长期对话中维持历史一致性和准确回答的能力。（2）具身技能场景：使用类似ALFRED的任务，智能体需从之前的成功执行轨迹中提炼技能并存储为文件，在后续任务中检索并使用。具体变量包括：记忆形状（agent-maintained hierarchy vs. plain dump vs. chunk retrieval）、记忆规模（从小到大的生长过程）、工具集（完整Shell vs. 受控文件工具 vs. 不同搜索函数）、管理/搜索智能体的底层模型（不同能力的LLM）。每个条件重复多次以获取统计规律。测量指标包括：答案正确率（或任务成功率）、每次查询的成本（rounds、tokens、读取内容量）、存储健康指标（更新前后关键信息是否丢失、早期内容是否可检索）。实验重点收集生长曲线数据，展示不同条件下的动态表现。

Q5: 发现了什么实验现象？

实验中观察到的核心现象包括：（1）组织降低检索成本：层次化组织结构在记忆规模较大时，可将检索所需读取的内容量减少约一半，且成本优势随规模增加而扩大。（2）组织随记忆增长而侵蚀：除使用最强LLM作为管理智能体的设置外，其他智能体均出现目录结构混乱、文件内容重复和过期，组织度逐渐下降。（3）组织不改善答案质量：在对比实验中，有组织存储与无组织存储在问答准确率上无显著差异，即智能体未能利用组织带来的检索优势。（4）工具集的影响与模型同级：仅将工具接口从shell命令切换为受限的文件操作函数，即可引发存储结构的大幅变化（例如目录深度、文件大小分布），其影响程度堪比更换管理智能体的模型容量。（5）早期记忆存活：所有设置中，早期储存的内容倾向于在演化中存活，尤其在管理智能体较强者中存活率更高，但存在更新错误导致信息丢失的情况。（6）成本-质量解耦：降低成本的同时质量未提升，提示单纯的检索效率提升并不自动增强推理能力。（7）反直觉现象：尽管组织退化，搜索成本上升有限，因搜索工具本身具备一定的容错性。

Q6: 有什么可以进一步探索的点？

基于研究发现，未来工作可在以下方向深入：（1）强化管理智能体：设计专门的管理策略（如定期整理、去重、摘要）或训练专有管理模型。（2）组织感知的搜索：让搜索智能体更智能地利用文件层次和元数据，例如通过路径导航、摘要优先等。（3）自适应组织形式：根据任务类型和存储内容动态调整目录结构，而非固定树形。（4）多智能体共享存储：多个智能体协同读/写同一文件系统时的冲突解决和一致性维护。（5）跨任务迁移：探索文件系统记忆在不同领域（如代码、科研）的适用性。（6）工具影响机制分析：系统研究工具设计如何影响智能体的记忆操作行为。（7）与检索增强生成（RAG）的结合：将文件系统作为RAG的动态知识源。

Q7: 总结一下论文的主要内容

该论文是首个对基于文件系统的LLM智能体长期记忆进行系统性实证研究的工作。它首先指出，当前部署的LLM智能体普遍将长期记忆保存为文件系统（即由智能体自身读写的Markdown文件目录树），但学术界对此默认方案的两个核心假设（智能体可自动维护组织且组织有意义）从未加以验证。为此，论文形式化了基于文件系统的记忆框架，定义了三个角色：管理（管理内容整合与组织）、搜索（基于记忆回答问题）和执行（提供被蒸馏为技能的任务轨迹），从而将声明式记忆与技能统一在同一存储中。实验部分设计了两个任务范式：长对话基准与具身技能任务，并系统变化四个维度的变量：（i）记忆形状（智能体主动构建的层级结构 vs. 原始转储 vs. 分块检索），（ii）记忆流规模，（iii）工具接口（完整的沙盒shell vs. 类似记忆工具的函数 vs. 不同的搜索工具），（iv）管理/搜索智能体的模型能力。追踪指标包括答案质量、对话/任务成本（轮次、tokens、读取量）以及存储健康（早期记忆存活率、更新正确性），重点获取生长曲线而非端点结果。主要发现有三点：第一，组织确实能带来检索成本的显著降低（约减半），在大规模记忆下尤为明显；第二，除最强模型外，所有智能体的组织均随时间推移而退化，说明当前模型难以独立维持长期有序；第三，组织上的优势没有转化为答案质量的提升，意味着搜索智能体无法有效利用结构信息。此外，研究揭示了一个重要但以往被忽视的因素：工具接口的改变对存储结构的影响程度不亚于更换智能体模型。论文将文件系统记忆从默认假设转化为一个具有丰富设计空间的开放问题。在局限与未来方向上，文章指出需要更强大的管理智能体、更深度的组织利用技术等多条路径。总体而言，该工作为LLM智能体长期记忆的实用设计提供了结构性认知与经验基础。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文核心主题为LLM智能体的长期记忆，与用户画像中的agent方向权重0.1直接匹配。

## 基本信息

- 作者：Sizhe Zhou, Sheldon Yu, Hui Wei, Junda Wu, Siru Ouyang, Yizhu Jiao, Shijia Pan, Julian McAuley, Yu Zhang, Tong Yu, Jiawei Han
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26637v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本文生成优先参考了PDF检索证据（retrieved_evidence），结合heuristic_draft和field_evidence_map进行内容扩充与校准。
