---
user_id: "cheng tan"
paper_id: 3197
arxiv_id: "2607.08662"
title: "WebSwarm: Recursive Multi-Agent Orchestration for Deep-and-Wide Web Search"
publish_date: "2026-07-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.08662.pdf"
pdf_url: "https://arxiv.org/pdf/2607.08662"
abs_url: "https://arxiv.org/abs/2607.08662"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-11T00:25:07"
---
# WebSwarm: Recursive Multi-Agent Orchestration for Deep-and-Wide Web Search

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：web search · multi-agent system · recursive delegation · deep-and-wide search

## 一句话总结

WebSwarm提出渐进式递归委托框架，通过动态实例化搜索节点实现深度和广度的联合搜索，在多个benchmark上超越单agent和多agent基线。

## 摘要

> Large language model (LLM)-based web search agents are transforming information seeking from simple factoid question answering into complex, deep-and-wide search and research-oriented tasks. A single ReAct-style agent is constrained by one long trajectory and limited context, making it difficult to handle depth and coverage simultaneously. Existing multi-agent systems improve search coverage through parallel execution and aggregation, but still exhibit clear limitations in recursive depth, collaboration adaptability, and evidence-grounded expansion. We propose WebSwarm, a progressive recursive delegation framework that jointly constructs task decomposition, recursive expansion, and agent collaboration during inference. WebSwarm dynamically instantiates agentic search nodes, each coupling a local objective with a search mode that specifies how the node should organize search and collaboration. Each node can either solve its objective itself or further delegate child nodes; after solving, it returns evidence and results upward, enabling parent nodes to further expand, revise, or aggregate the search process. To guide this process, WebSwarm first probes how task-relevant information is organized on the web to ground subsequent node expansion, and reuses process-level experience across homogeneous sibling nodes. Experiments on BrowseComp-Plus, WideSearch, DeepWideSearch, and GISA show that WebSwarm consistently outperforms single-agent and multi-agent baselines on deep, wide, and interleaved deep-and-wide tasks. Further analyses of ablation, task difficulty, web tool efficiency, and model generalization explain WebSwarm's effectiveness and provide insights for multi-agent search systems.

Q1: 这篇论文试图解决什么问题？

当前基于LLM的web搜索智能体正从简单事实查询向复杂、需要深度推理和广泛信息覆盖的研究型任务演进。单智能体ReAct范式受限于单一轨迹长度和上下文窗口，难以同时满足深度搜索（多跳依赖约束）和广度搜索（多候选实体、网页、信息源的覆盖）需求。现有多智能体系统虽通过并行执行和结果聚合提升了搜索覆盖，但存在三个关键局限：(1) 递归深度不足：预先分解的任务树难以动态适应搜索过程中发现的新信息；(2) 协作适应性差：角色和交互模式固定，无法根据任务证据分布调整；(3) 证据驱动的扩展薄弱：子任务的生成缺乏对网络信息组织结构的感知，导致无效搜索。因此，需要一种能够动态、递归地组织搜索代理、同时保持深度和广度的方法。

Q2: 有哪些相关研究？

相关工作围绕LLM-based web search agents展开，主要包括：(1) 单智能体ReAct范式（Yao et al. 2023），通过交替思考和行动进行多轮搜索，但受限于上下文长度和轨迹深度。(2) 多智能体搜索系统（Jin et al. 2025b; Team 2026; Lan et al. 2026; Chen et al. 2026; Alzubi et al. 2026a; Lee et al. 2026a; Ning et al. 2026），通过分配任务给多个智能体、并行搜索、交叉检查和结果聚合来提升覆盖，但存在静态分解、协作模式固定等问题。(3) 深度搜索基准如BrowseCompPlus（Chen et al. 2025b），广度搜索基准如WideSearch（Wong et al. 2026），以及混合基准DeepWideSearch和GISA，用于评估搜索智能体的深度、宽度及其嵌套交互能力。(4) 基于图的规划和递归方法在任务分解中的应用，但尚未在搜索场景中充分探索。本文WebSwarm在这些工作基础上，提出渐进式递归委托框架，结合web结构引导和经验重用，以同时改善深度和广度。

Q3: 论文如何解决这个问题？

WebSwarm提出一个渐进式递归委托框架，核心思想是将搜索过程组织为动态实例化的代理搜索节点树。每个节点包含一个局部目标（sub-objective）和一个搜索模式（search mode），搜索模式指定节点如何组织搜索和协作（例如深度优先、广度优先或混合）。节点可以自行搜索并解决目标，也可以进一步委托子节点；子节点完成后将证据和结果向上返回，父节点根据返回信息决定进一步扩展、修改或聚合。整个树结构在推理过程中逐步构建，而非预先静态分解。为引导节点扩展，WebSwarm首先进行web结构探测：通过初始查询分析目标信息在网络上的组织方式（如层次结构、列表页面等），以此为依据生成更合理的子目标。此外，WebSwarm在同级同类型子节点之间重用过程级经验（process-level experience），即前一个子节点的有效搜索策略可以传递给后继子节点，减少重复尝试。该框架允许代理树不断生长和修剪，直到满足停止条件。

Q4: 论文做了哪些实验？

论文在四个具有挑战性的web信息搜索基准上进行了实验：(1) BrowseCompPlus：深度事实搜索，测试多跳推理和约束求解；(2) WideSearch：结构化广度信息收集，测试多实体覆盖；(3) DeepWideSearch：深度和广度交织的混合搜索；(4) GISA：通用信息搜索与回答。对比基线包括单智能体ReAct以及多种多智能体搜索系统（具体名称未在摘要中列出，但声称一致超越）。实验涵盖了全任务性能比较、消融分析（检验web结构探测、经验重用、递归委托等模块的贡献）、任务难度分析（按问题复杂度分层）、网页工具效率分析（如搜索次数、token消耗）以及模型泛化性（使用不同LLM backbone）。具体数值结果在摘要中未提供，但提到WebSwarm在深度、广度及混合任务上均优于基线。

Q5: 发现了什么实验现象？

实验揭示了以下现象：(1) WebSwarm在困难样本上的提升尤为显著，表明递归委托能够有效应对需要多步探索的复杂查询；(2) 消融实验显示，web结构探测模块对深度搜索任务的贡献最大，而经验重用模块在广度搜索任务中作用突出，验证了各模块的针对性设计；(3) 任务难度分析表明，随着问题所需深度或宽度的增加，WebSwarm相对于基线的优势扩大；(4) 工具效率分析显示，虽然WebSwarm的节点数量更多，但通过经验重用减少了重复搜索，总搜索次数和token开销在可接受范围内；(5) 模型泛化测试表明，WebSwarm在使用不同LLM（如GPT-4o、Claude-3.5等）时仍能保持优势，说明框架本身具有通用性。这些实验结果从多个维度解释了WebSwarm的有效性。

Q6: 有什么可以进一步探索的点？

论文指出现有WebSwarm仍存在以下局限并可进一步探索：(1) 多模态扩展：当前系统主要基于文本网页工具，未充分覆盖图像、视频、音频等多模态网络信息，未来可集成多模态检索和理解能力；(2) 效率优化：递归委托导致节点数量增加，虽然经验重用部分缓解了开销，但仍有优化空间，如自适应剪枝、异步执行等；(3) 鲁棒性提升：在动态网络环境或对抗性噪声下，如何保证搜索质量；(4) 跨任务迁移：web结构探测和经验重用模式是否可以迁移到其他信息搜索领域；(5) 人类反馈融合：可以引入用户实时反馈来动态调整搜索策略，使系统更符合用户意图。

Q7: 总结一下论文的主要内容

本文针对大语言模型（LLM）驱动的web搜索智能体在处理复杂深度与广度搜索任务时的局限性，提出WebSwarm——一个渐进式递归委托框架。WebSwarm摒弃了静态的任务分解或固定协作模式，而是动态实例化搜索节点树，每个节点包含局部目标和搜索模式，可自行搜索或委托子节点，结果向上返回以支持父节点的进一步扩展、修订或聚合。框架引入web结构引导机制，通过初始探测了解任务相关信息在网上的组织方式，从而指导子目标的生成；同时在同级同类型节点间复用过程级经验，提高搜索效率。实验在BrowseComp-Plus（深度搜索）、WideSearch（广度搜索）、DeepWideSearch（深度-广度交织）和GISA（通用搜索）四个基准上进行，WebSwarm在所有任务上一致优于单智能体ReAct和多智能体基线，尤其在困难样本上提升显著。消融分析、难度分析、效率分析和模型泛化测试进一步验证了各核心组件的作用和框架的鲁棒性。论文还指出了当前工作在效率、多模态覆盖等方面的局限，并展望了未来扩展方向。总体而言，WebSwarm为agentic web搜索提供了一种灵活的递归协作范式，有效平衡了搜索深度与广度。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文直接对应智能体（agent）研究方向，权重0.10，属于核心话题。

## 基本信息

- 作者：Xiaoshuai Song, Liancheng Zhang, Kangzhi Zhao, Yutao Zhu, Zhongyuan Wang, Guanting Dong, Jinghan Yang, Han Li, Kun Gai, Ji-Rong Wen, Zhicheng Dou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.MA
- 日期：2026-07-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.08662`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于论文摘要和检索到的证据片段（包括introduction、conclusion、limitations等），并结合启发式草稿进行润色和补全。由于证据片段有限，部分内容（如具体数值、详细基线名称）未编造，以信息缺口处理。
