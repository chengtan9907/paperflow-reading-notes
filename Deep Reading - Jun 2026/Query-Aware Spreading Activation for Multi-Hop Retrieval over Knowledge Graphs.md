---
user_id: "cheng tan"
paper_id: 1929
arxiv_id: "2606.30133"
title: "Query-Aware Spreading Activation for Multi-Hop Retrieval over Knowledge Graphs"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.30133.pdf"
pdf_url: "https://arxiv.org/pdf/2606.30133"
abs_url: "https://arxiv.org/abs/2606.30133"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:29:36"
---
# Query-Aware Spreading Activation for Multi-Hop Retrieval over Knowledge Graphs

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：graph rag · multi-hop retrieval · spreading activation · query-aware traversal

## 一句话总结

提出一种基于查询感知的激活扩散方法，用于知识图谱上的多跳检索，将整个检索过程实现为单个Cypher查询，无需将图加载到内存，在MuSiQue上达到与QAFD-RAG相当的精确匹配分数，并超越最强纯结构基线HippoRAG。

## 摘要

> Retrieval-augmented generation built on knowledge graphs (Graph RAG) outperforms flat passage retrieval on multi-hop question answering by leveraging graph structure. In most existing systems, however, the question only sets the seed nodes; the subsequent traversal becomes "query-blind", depending solely on the graph structure. The exception is QAFD-RAG, which implements query-aware traversal via a flow-diffusion solver with combined edge re-weighting. This architecture requires loading the full graph into Python memory and an iterative solver with a variable number of iterations complicating integration with the graph database. We propose a spreading-activation method that achieves the same query-aware traversal with a single per-step semantic gate: the step weight is the cosine similarity between the candidate entity's description and the question, and the number of iterations is fixed. The whole retrieval procedure - seed mapping, propagation, top-K selection and context assembly - is expressed as a single Cypher query executed in one round-trip to Neo4j; the graph never leaves the database. On MuSiQue our method matches QAFD-RAG by exact match (32.80 vs 33.50) and outperforms the strongest purely-structural baseline in our comparison, HippoRAG, by 5.3 EM and 3.4 F1; on 2WikiMultiHopQA HippoRAG and QAFD-RAG retain an advantage due to their phrase-node architectures. An ablation with the gate disabled confirms that the gate is the source of a simultaneous F1 gain of 3.6 to 7.4 points and a retrieval-latency reduction by a factor of 1.5 to 4.9.

Q1: 这篇论文试图解决什么问题？

现有基于知识图谱的检索增强生成（Graph RAG）系统在多跳问答中优于平面段落检索，但大多数系统仅在种子节点选择时使用问题，后续图遍历变得“查询盲目”，仅依赖图结构。例外是QAFD-RAG，它通过流扩散求解器实现查询感知遍历，但需要将全图加载到Python内存并使用迭代求解器，迭代次数可变，难以与图数据库集成。问题：如何实现既查询感知又高效、无需将图移出数据库的图遍历方法？

Q2: 有哪些相关研究？

相关研究按问题对检索的影响程度组织：
1. 仅用问题选择种子节点的方法（Section II-A）：如PageRank、贪心扩展等，遍历独立于查询。
2. 额外用问题指导图遍历的方法（Section II-B）：包括QAFD-RAG（流扩散求解器，结合边重加权），以及本文提出的激活扩散方法。
其他基线：HippoRAG（使用短语节点结构）、基于PageRank的方法等。本文方法属于第二类，但避免内存加载和可变迭代。

Q3: 论文如何解决这个问题？

提出一种查询感知的激活扩散（Spreading Activation）方法，实现为单个Cypher查询到Neo4j。算法从问题中提取种子实体，在知识图谱上传播激活，迭代次数固定为K。每一步，计算候选实体描述与问题的余弦相似度作为语义门控（step weight），乘以激活值进行传播。最终选择top-K激活实体作为检索结果，连同上下文组装为LLM输入。关键：所有操作在Neo4j内完成，一次往返，无需将图加载到Python内存。

Q4: 论文做了哪些实验？

在MuSiQue和2WikiMultiHopQA两个公共多跳问答基准上评估。对比基线包括：QAFD-RAG、HippoRAG、PageRank-based方法等。评估指标：精确匹配（EM）和F1分数。报告包括整体性能、消融研究（禁用语义门控）、延迟分析。

Q5: 发现了什么实验现象？

1. 在MuSiQue上，本文方法（EM 32.80）与QAFD-RAG（33.50）基本持平，显著优于HippoRAG（EM 27.5，F1 36.2）。2. 在2WikiMultiHopQA上，HippoRAG和QAFD-RAG因短语节点架构保持优势。3. 消融实验：禁用语义门控导致F1下降3.6-7.4，同时检索延迟增加1.5-4.9倍，证明门控的有效性和效率优势。4. 固定迭代次数避免了QAFD-RAG的可变迭代问题。

Q6: 有什么可以进一步探索的点？

1. 探索更复杂的语义门控形式，如学习注意力权重。2. 将方法扩展到更大规模的动态知识图谱。3. 研究不同传播策略（如衰减因子）的影响。4. 与更强大的LLM集成进行端到端评估。5. 在多语言或跨模态场景中的适用性。

Q7: 总结一下论文的主要内容

本文针对Graph RAG中图遍历查询盲目问题，提出一种查询感知的激活扩散方法。通过引入余弦相似度语义门控，使每一步传播都考虑查询，且迭代次数固定，便于图数据库集成。整个检索流程封装为单个Cypher查询，高效且无需将图移出数据库。在MuSiQue上达到与QAFD-RAG相当的性能，并超过纯结构基线。消融实验验证了语义门控的关键作用。局限：在2WikiMultiHopQA上不及短语节点架构方法，可能因缺乏实体短语级匹配。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：直接相关于多跳问答和图检索增强生成。

## 基本信息

- 作者：Illia Makarov, Mykola Glybovets
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.IR
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.30133`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，主要来自Abstract、Introduction、Method、Conclusion及相关chunks。
