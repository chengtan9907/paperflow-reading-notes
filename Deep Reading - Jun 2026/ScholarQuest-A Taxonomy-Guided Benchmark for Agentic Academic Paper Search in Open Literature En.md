---
user_id: "cheng tan"
paper_id: 817
arxiv_id: "2606.20235v1"
title: "ScholarQuest: A Taxonomy-Guided Benchmark for Agentic Academic Paper Search in Open Literature Environments"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.20235v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.20235v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-20T01:21:17"
---
# ScholarQuest: A Taxonomy-Guided Benchmark for Agentic Academic Paper Search in Open Literature Environments

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic search · benchmark · academic paper search · taxonomy-guided

## 一句话总结

提出 ScholarQuest，一个基于分类法的大规模基准，用于在开放文献环境中评估智能体学术论文搜索。

## 摘要

> Academic paper search is a core step in scientific research, and LLM-based search agents are emerging as a promising paradigm for iterative, intent-driven literature exploration. However, existing benchmarks are insufficient for systematically evaluating agentic academic search under realistic open literature environments. We propose ScholarQuest, a large-scale, taxonomy-guided benchmark for agentic academic paper search. ScholarQuest is constructed from over 1,000 computer science topics and four representative research intents, including method-oriented, setting-anchored, comparison-based, and scope-controlled queries. It further provides scalable answer construction and a shared retrieval backend ScholarBase for reproducible evaluation. Benchmarking results show that agentic methods outperform single-shot retrieval baselines, yet the best-performing agent only achieves 0.314 Recall@100 and 0.355 Recall@All, indicating substantial room for improvement. In addition, analyses of search efficiency, intent-level robustness, and failure cases further highlight the benchmark's ability to provide multi-dimensional evaluation signals for academic paper search agents. Our code and data are publicly available $^{1}$ .

Q1: 这篇论文试图解决什么问题？

现有学术论文搜索基准多依赖有限人工查询或论文衍生查询，无法覆盖多样化研究意图和真实搜索过程（如多轮迭代、意图保持），且缺乏在开放文献环境下系统评估智能体搜索代理的能力。论文旨在填补这一空白，提供一个大规模、可复现、分类法指导的基准。

Q2: 有哪些相关研究？

传统学术搜索依赖词汇或语义匹配（如 TF-IDF、BM25、密集检索），效率高但缺乏意图理解。近年 LLM 被用于生成查询或重排序，而智能体工作流方法（如 PaSa）将搜索建模为多步过程，包括搜索、检查、选择相关论文。现有基准如 BEIR、TREC 等主要评估一次性检索，未覆盖多步交互和意图多样性。

Q3: 论文如何解决这个问题？

提出 ScholarQuest 基准，从 1000+ 计算机科学主题中通过分类法引导生成四种研究意图的查询（方法导向、设置锚定、比较型、范围限定）。构建可扩展管道：整合多源检索（arXiv、Semantic Scholar）、引用扩展、相关性过滤和质量控制。提供共享检索后端 ScholarBase 确保评估可复现。答案集通过 arXiv ID 和元数据构建，排除主观性。

Q4: 论文做了哪些实验？

评估三种智能体搜索系统（PaSa 等）与单次检索基线（BM25、Contriever 等）在 ScholarQuest 上的表现。主要指标：Recall@100、Recall@All。分析不同意图类型下的搜索效率（步数、延迟）和失败案例。

Q5: 发现了什么实验现象？

智能体方法在 Recall@All 上优于单次检索基线，但最佳智能体仅达 0.355 Recall@All，性能差距显著。不同意图下表现不稳定：比较型查询召回最低，范围限定查询召回较高。搜索效率分析显示智能体多轮交互带来额外开销。失败案例包括意图漂移、过度扩展和遗漏关键论文。

Q6: 有什么可以进一步探索的点？

开发具有更强意图保持、约束感知过滤和逐轮证据级推理的智能体；扩展基准至更多学科和查询类型；引入用户满意度、效率等多维度指标；探索基于反馈的迭代优化策略。

Q7: 总结一下论文的主要内容

论文提出 ScholarQuest，一个基于分类法的大规模基准，旨在系统评估智能体学术论文搜索。基准覆盖 1000+ CS 主题和四种研究意图，通过可扩展管道构建答案集和共享检索后端确保可复现。实验对比了现有智能体方法与单次检索基线，发现智能体方法虽整体更优但绝对性能仍低，揭示意图保持、多轮推理等挑战。论文还分析了不同意图下的搜索效率和失败模式，为未来智能体搜索研究提供多维度评估信号和开放资源。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与智能体（agent）方向直接相关，提供标准化评估平台。

## 基本信息

- 作者：Tingyue Pan, Mingyue Cheng, Daoyu Wang, Yitong Zhou, Jie Ouyang, Qi Liu, Enhong Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.IR, cs.AI
- 日期：2026-06-18
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.20235v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告参考了PDF检索证据。
