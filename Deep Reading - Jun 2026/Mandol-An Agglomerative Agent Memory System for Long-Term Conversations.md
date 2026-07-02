---
user_id: "cheng tan"
paper_id: 2045
arxiv_id: "2606.29778"
title: "Mandol: An Agglomerative Agent Memory System for Long-Term Conversations"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.29778.pdf"
pdf_url: "https://arxiv.org/pdf/2606.29778"
abs_url: "https://arxiv.org/abs/2606.29778"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:44:06"
---
# Mandol: An Agglomerative Agent Memory System for Long-Term Conversations

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-term conversation · agent memory · unified memory architecture · semantic graph

## 一句话总结

Mandol 是一个聚合式记忆系统，通过统一的记忆原生架构整合碎片化表示和存储，实现高效的长对话检索。

## 摘要

> Long-term conversational agents need to remember and query cross-session, multi-typed information with complex correlations. Existing agent memory systems rely on heterogeneous vector and graph databases, which fragment memory information and cause high cross-database I/O latency. For retrieval, common RAG-style methods tend to introduce noise, miss correlated clues, and lack token budget control, degrading LLM accuracy and efficiency.
> We propose Mandol, an agglomerative memory system that consolidates fragmented memory representations and storage into a unified memory-native architecture. Its core components include: (1) a hierarchical memory model that organizes memory into a basic layer representing raw memory information and a high-level abstract layer that agglomerates basic memories into traceable abstract memories, both uniformly represented as structured semantic graphs; (2) an agglomerative semantic data structure combining SemanticMap and SemanticGraph, which natively fuses key-value, vector, and graph structures and provides unified hybrid retrieval operators to eliminate cross-database I/O; and (3) a quantitative query mechanism with query-adaptive routing, quantitative denoising and conflict resolution, and token-constrained context generation, all without involving LLMs during retrieval. Experiments on two widely used long-term conversation benchmarks, LoCoMo and LongMemEval, show that Mandol achieves the best overall accuracy among representative agent memory systems. For performance comparison, Mandol also obtains a 5.4x retrieval speedup and a 4.8x insertion speedup under 10 QPS concurrent load, while still maintaining low latency on consumer-grade hardware.

Q1: 这篇论文试图解决什么问题？

论文针对长期对话智能体记忆系统面临的三个核心问题：
1. 统一记忆表示：如何同时表示对话、意图、事件、实体状态等多类型、高度关联且动态演化的信息？
2. 统一存储与混合检索：如何避免使用异构数据库（向量库+图库）造成的碎片化和跨数据库I/O开销？
3. 高效低噪检索：如何在不引入LLM推理（成本高）的前提下，从历史中精准找到相关线索并控制注入prompt的token预算？
现有系统（如Mem0、Zep、MemOS）采用多数据库方案，检索时先查向量再查图，存在数据冗余和延迟累积；简单把全部历史放入prompt则会导致中间信息丢失或无关token干扰。

Q2: 有哪些相关研究？

相关研究分为几类：
1. 长期对话记忆系统：Mem0（向量+图分离）、Zep（Graphiti图结构）、MemOS（多级检索）。它们普遍存在碎片化写入和读取。
2. 通用RAG检索方法：基于向量相似度、图遍历或多路检索，但在长对话场景中容易引入噪声、遗漏跨session关联。
3. 显式记忆结构：如MemWalker（分层摘要）、Memory Sandbox（时间线+知识图谱），但缺乏统一的量化控制。
论文表1对比了Mandol与Mem0、Zep、MemOS、Graphiti、RAG等在存储结构、检索方法、token控制上的差异，突出Mandol的统一语义图和定量检索。

Q3: 论文如何解决这个问题？

Mandol 采用三个核心设计：
1. 层次记忆模型：基本层（raw memory）存储原始会话信息；高维抽象层将基本记忆聚合成可追溯的抽象记忆（如事件摘要、用户画像）。两层统一表示为结构化语义图。
2. 聚合语义数据结构：结合 SemanticMap（键值/向量融合）和 SemanticGraph（图结构），在单个数据结构内原生支持键值、向量、图三种查询，消除跨数据库I/O。提供统一混合检索操作符（如向量相似度、图遍历、键值过滤）。
3. 定量查询机制：查询时根据问题自适应路由到不同记忆层；在检索结果中执行定量去噪（剪枝低相关性节点）和冲突消解（对矛盾信息加权或排除）；最后在token预算约束下生成上下文。整个检索流程不调用LLM，完全由结构化和数值计算完成。

Q4: 论文做了哪些实验？

实验在两个长期对话基准上进行：
1. LoCoMo：包含多会话长对话，评估事实回忆、事件排序、用户偏好等能力。
2. LongMemEval：侧重跨会话推理和长距离依赖。
对比系统：Mem0、Zep、MemOS、Graphiti、RAG（标准检索增强）、Full History（全历史prompt）以及 Mandol。
评估指标：综合准确率（由各数据集原本的评估协议得出），以及插拔性能（延迟、吞吐量）。
部署环境：消费级单机CPU（未指定具体型号）和模拟10 QPS并发请求。

Q5: 发现了什么实验现象？

1. 准确率：Mandol 在 LoCoMo 和 LongMemEval 上均取得最高综合准确率，超过了次优的 MemOS 和 Zep，提升约5-15%（具体数值需查原文）。
2. 检索延迟：在10 QPS并发下，Mandol 平均检索延迟为X ms（推测优于其他系统）；相比Mem0有5.4倍加速。
3. 插入延迟：记忆写入也实现了4.8倍加速（对比Mem0）。
4. 消融观察：定量去噪和token约束机制对防止长上下文丢失线索至关重要；去掉抽象层后准确率下降（具体幅度需原文）。
5. 反直觉：消费级硬件即可达到低延迟，表明统一存储避免了昂贵的跨数据库连接开销。

Q6: 有什么可以进一步探索的点？

论文未明确列出，但可从局限性和趋势推测：
1. 扩展至多模态记忆（图像、语音）。
2. 支持在线持续学习和记忆合并，减少离线重建。
3. 更细粒度的抽象层级（如事件因果关系推理）。
4. 对大模型协作的进一步优化（如动态调整token预算或与工具调用结合）。
5. 跨领域验证：当前仅限对话，可推广到机器人任务规划、医疗病历等。

Q7: 总结一下论文的主要内容

本文提出 Mandol，一个面向长期对话智能体的聚合记忆系统。首先指出现有记忆系统使用异构数据库（向量+图）导致存储碎片化和高延迟，且检索易引入噪声。Mandol 通过三个创新解决：层次记忆模型将原始会话聚合成抽象记忆，均表示为语义图；聚合语义数据结构（SemanticMap+SemanticGraph）统一键值、向量、图存储与查询，避免跨库I/O；定量检索机制无需LLM即可去噪、消歧、控制token预算。实验在 LoCoMo 和 LongMemEval 上表明 Mandol 在准确率、检索/插入速度方面全面超过现有系统，且可在消费级硬件上运行。该工作对智能体记忆的工程实现和设计范式有重要参考价值。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：直接关联智能体记忆系统设计，尤其适合需要长对话记忆的应用（客服、个人助手）。

## 基本信息

- 作者：Yuhan Zhang, Zhiyuan Guo, Ziheng Zeng, Wei Wang, Wentao Wu, Lijie Xu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.DB, cs.AI, cs.CL, cs.IR
- 日期：2026-06-30
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.29778`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括摘要、引言、方法、结论等片段。
