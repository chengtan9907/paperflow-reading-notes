---
user_id: "cheng tan"
paper_id: 1454
arxiv_id: "2606.25320v1"
title: "Data-Driven Evolution of Library and Information Science Research Methods (1990-2022): A Perspective Based on Fine-grained Method Entities"
institution: "南京理工大学信息管理系"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25320v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25320v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T18:12:14"
---
# Data-Driven Evolution of Library and Information Science Research Methods (1990-2022): A Perspective Based on Fine-grained Method Entities

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：library and information science · research methods · data-driven · fine-grained method entities

## 一句话总结

本研究利用定量方法，从细粒度方法实体的视角分析并总结了1990-2022年图情领域数据驱动研究方法的演化模式，揭示了数据资源是方法论演化的核心驱动力，并呈现‘出现-稳定/实际应用’的循环模式。

## 摘要

> Since the 1990s, advancements in big data and information technology have increasingly driven data-centric research in the field of Library and Information Science (LIS). To assess the influence of this data-driven research paradigm on the LIS discipline, this study conducts a fine-grained analysis to uncover the evolutionary trends of research methods within the domain. Using academic papers from LIS published between 1990 and 2022, four key categories of data-driven method entities are automatically extracted: algorithms and models, data resources, software and tools, and metrics. Based on these entities, the study examines the evolution of LIS research methods from three dimensions: the characteristics of research method entities over time, their evolution within different research topics, and the evolutionary features of research method entities across various research methods. The findings highlight data resources as a pivotal driver of methodological evolution in LIS, revealing a cyclical pattern of "emergence-stability/practical application" in the development of research methods within the field.

Q1: 这篇论文试图解决什么问题？

评估数据驱动研究范式对图情学科的影响，通过细粒度分析方法揭示该领域研究方法在1990-2022年间的演化趋势。具体包括：研究方法随时间如何变化？不同研究主题下方法实体有何差异？不同研究方法（如实验、理论分析等）下方法实体如何分布？

Q2: 有哪些相关研究？

相关研究包括对图情领域研究方法的定性综述、数据驱动范式的讨论（如Tang et al., 2021），以及基于引用或主题建模的方法演化分析。本研究通过自动提取细粒度方法实体，补充了传统定性综述的不足，提供了定量、客观的演化视角。参考文献包括JASIST、IPM等期刊的相关工作。

Q3: 论文如何解决这个问题？

1) 构建图情领域1990-2022年学术论文全文语料（假设方法章节中的方法更核心）；2) 自动提取四类方法实体：算法与模型、数据资源、软件与工具、指标；3) 从三个维度分析演化：时间维度（年度分布、相似度）、研究主题维度（六类主题）、研究方法维度（五种常用方法）。4) 通过分布差异、Jaccard相似度、可视化等揭示模式。

Q4: 论文做了哪些实验？

1) 四类方法实体在1990-2022年间的年度分布统计；2) 相邻年份实体集合的Jaccard相似度计算（衡量稳定性）；3) 六类研究主题（如信息组织、信息检索、用户行为等）下的方法实体分布变化；4) 五种常用研究方法（实验、理论方法、内容分析、问卷、文献计量）下的方法实体分布变化；5) 四类实体在时间窗口内的出现、稳定、衰退模式分析。

Q5: 发现了什么实验现象？

1) 数据资源实体在2000年后显著增长，成为方法演化的核心驱动力；2) 方法实体相似度总体上升，但2011-2013年出现下降，可能因神经网络兴起引入新算法；3) 不同主题下实体分布差异明显，如信息检索主题中算法实体比例高，用户研究主题中指标实体多；4) 五种研究方法中实验法使用最多且实体多样性最高；5) 方法演化呈现‘出现-稳定/实际应用’的循环模式，新兴方法先快速出现后进入稳定应用阶段。

Q6: 有什么可以进一步探索的点？

1) 扩展到更多学科领域验证演化模式；2) 细化方法实体分类（如区分经典算法与前沿算法）；3) 结合引用网络和合作网络分析方法流动；4) 利用大语言模型提高实体抽取准确性和细粒度；5) 引入时间序列预测模型预测未来方法趋势；6) 分析方法实体与论文影响力之间的关系。

Q7: 总结一下论文的主要内容

本研究针对图情领域自1990年代以来数据驱动研究范式的影响，通过自动提取细粒度方法实体（算法与模型、数据资源、软件与工具、指标），系统分析了1990-2022年间研究方法的演化特征。论文首先构建了包含图情领域代表性期刊全文的语料库，从方法章节中提取四类实体，然后从三个维度（时间、研究主题、研究方法）进行量化分析。主要发现包括：数据资源是方法论演化的核心驱动力；方法实体呈现出‘出现-稳定/实际应用’的循环模式，即新方法先快速涌现，随后进入稳定或实际应用阶段；不同研究主题和方法类型下的实体分布存在显著差异；2011-2013年间实体相似度下降可能与神经网络技术的兴起有关。研究为理解图情领域方法论的演化提供了客观、定量的视角，有助于指导未来方法选择和研究规划。论文还讨论了方法实体抽取的局限性（依赖方法章节假设）、语料覆盖范围有限等，并指出未来可向多学科扩展、结合引用分析等方向。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在5.2 Implications / References部分

## 基本信息

- 作者：Chengzhi Zhang, Yi Mao, Shuyu Peng
- 机构：南京理工大学信息管理系
- 来源：arxiv
- 主题/分类：cs.DL, cs.CL, cs.CY, cs.IR
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25320v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括摘要、引言、方法、结果、结论及参考文献片段。
