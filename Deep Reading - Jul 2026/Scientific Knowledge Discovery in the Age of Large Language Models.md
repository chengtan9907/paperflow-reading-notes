---
user_id: "cheng tan"
paper_id: 6042
arxiv_id: "2607.26670v1"
title: "Scientific Knowledge Discovery in the Age of Large Language Models"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26670v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26670v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:24:43"
---
# Scientific Knowledge Discovery in the Age of Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · scientific knowledge discovery · literature retrieval · literature screening

## 一句话总结

本篇综述系统梳理了34篇关于生成式大语言模型应用于科学文献检索和筛选的研究，分析了其技术特征和评估方法。

## 摘要

> The rapid growth of scholarly literature has made identifying relevant publications increasingly difficult, and conventional search systems still depend heavily on manually formulated queries and effortful manual inspection. Generative large language models (LLMs) offer a more flexible alternative, supporting literature retrieval and the screening of candidate studies against eligibility criteria. This chapter surveys 34 peer-reviewed papers applying generative LLMs to these two tasks, identified via a Boolean search over the OpenAIRE Graph (1,589 records screened to 34 inclusions). Reviewed studies are characterised by LLMs employed, model access and adaptation, prompting and architectural techniques, ground-truth sources, and evaluation metrics.

Q1: 这篇论文试图解决什么问题？

随着科学出版的指数增长，研究人员和从业者面临着前所未有的文献筛选负担。传统的信息检索系统主要依赖关键词匹配和布尔逻辑，要求用户精确构建查询，但领域术语的快速演变使得查询维护成本高昂；基于统计的检索排序（如TF-IDF、BM25）难以捕捉深层语义关系。此外，对于系统综述等任务，人工筛选候选文献是大规模耗时环节。虽然已有基于机器学习的分类器尝试自动化筛选，但它们通常需要大量标注数据且在领域迁移中表现不一。生成式LLMs的兴起提供了新的交互范式：用户可以用自然语言描述信息需求，模型能理解上下文、产生推理并评估文献相关性。然而，这一领域的应用尚处于早期，缺乏系统的技术总结和评估框架。因此，本文旨在回答以下问题：2024-2026年间，生成式LLMs在文献检索和筛选中的具体应用方式是什么？使用了哪些模型和技术？如何评估性能？现有研究的优势和不足是什么？通过系统综述，本文为后续研究提供了结构化的参考。

Q2: 有哪些相关研究？

相关研究包括传统的文献检索方法（如向量空间模型、概率检索模型、学习排序）、文献推荐系统（协同过滤、基于内容、图方法）以及自然语言处理在学术检索中的应用（如Sentence-BERT嵌入检索、基于Transformer的查询重写）。在自动化文献筛选方面，早期工作采用朴素贝叶斯、支持向量机等分类器。最近，大语言模型（如GPT、BERT）被用于筛选系统综述中的参考文献。然而，本文是首批专门针对生成式LLMs（而非判别式或编码器模型）在文献检索和筛选两个任务上的系统综述之一。其他综述可能更广泛地覆盖NLP在学术检索中的应用，或集中于特定领域（如生物医学），而本文在搜索策略、纳入标准和分析维度上具有特定的聚焦。

Q3: 论文如何解决这个问题？

本文遵循系统综述和元分析（PRISMA）原则。首先，在OpenAIRE Graph（一个大型学术出版平台）上进行布尔搜索，查询串包含'large language model','LLM','agentic'，以及'literature exploration','scientific knowledge discovery','academic search','paper recommendation','systematic review screening'等术语的组合。检索得到1589条记录。经过标题和摘要筛选、全文筛选，最终纳入34篇出版于2024-2026年的同行评审论文。数据提取阶段，从每篇纳入论文中提取以下特征：(a)使用的LLM（如GPT-4、Llama 3、Gemini等）；(b)模型访问模式（API、开源模型本地部署、微调）；(c)提示工程策略（零样本、少样本、思维链、动态prompt等）；(d)系统架构（单步、多步、RAG、多智能体）；(e)真实数据源（PubMed、OpenAlex、CORD-19等）；(f)评估指标（精确率、召回率、F1、NDCG、人工评估等）。这些特征被汇总对比，以识别常见实践和创新趋势。

Q4: 论文做了哪些实验？

该论文为系统综述，未进行新的实验。它通过预先定义的搜索和筛选流程收集已有研究的实验数据，进行综合描述，而不是开展自己的实证研究。

Q5: 发现了什么实验现象？

由于本文是综述，没有直接的实验观测结果。它汇总了纳入论文中的实验发现，例如：多数文献检索研究使用top-k精确率或召回率作为主要指标；文献筛选任务中F1和AUC是最常见的评估；零样本LLM方法通常能匹配或超过轻量级监督基线。但具体对比和分析需要参考原始纳入的34篇论文。

Q6: 有什么可以进一步探索的点？

基于综述发现，未来研究可考虑：（1）开发统一、多领域的文献检索与筛选基准，包含更多真实用户查询；（2）探索多模态（表格、图表）的理解与检索；（3）结合知识图谱增强LLM的事实性和可解释性；（4）优化LLM推理效率以支持大规模实时检索；（5）评估LLM方法在新颖性识别和综合综述生成中的价值；（6）纳入更多非英语文献以拓宽覆盖范围。同时，提示工程和检索增强生成的系统研究也有待深入。

Q7: 总结一下论文的主要内容

本文系统综述了2024-2026年间34篇应用生成式大语言模型进行科学文献检索和筛选的同行评审研究。研究背景是学术文献爆炸性增长和传统检索方法的固有局限。作者在OpenAIRE Graph数据库上执行布尔搜索，筛选得到34篇论文并进行详细特征提取。分析维度包括所使用的LLM类型（以GPT-4为主）、模型访问方式（API为主）、提示策略（零样本最常见）、系统架构（涵盖RAG、多智能体等）、数据来源（如PubMed）和评估指标（准确率、F1等）。结果显示，生成式LLMs在文献检索和筛选任务中展现出灵活性，能够支持自然语言交互，减少人工工作量。然而，研究存在异质性大、评估标准不统一、计算成本高、领域通用性不足等问题。未来需要更标准化的基准和更系统的评估。本文为该领域提供了清晰的技术图谱和未来研究方向指南。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：作为系统综述，为关注AI for Science的研究者提供了快速了解领域全貌的入口

## 基本信息

- 作者：Eleni Adamidi, Serafeim Chatzopoulos, Thanasis Vergoulis
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.DL, cs.AI, cs.CL, cs.IR
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26670v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据（摘要、结论和方法片段）以及元数据信息。
