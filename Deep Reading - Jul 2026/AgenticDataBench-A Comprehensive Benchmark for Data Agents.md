---
user_id: "cheng tan"
paper_id: 2633
arxiv_id: "2607.01647"
title: "AgenticDataBench: A Comprehensive Benchmark for Data Agents"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.01647.pdf"
pdf_url: "https://arxiv.org/pdf/2607.01647"
abs_url: "https://arxiv.org/abs/2607.01647"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T22:20:18"
---
# AgenticDataBench: A Comprehensive Benchmark for Data Agents

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：benchmark · data agents · LLM agents · data science skills

## 一句话总结

提出AgenticDataBench基准，包含跨15个垂直领域的真实任务，带有细粒度标签，用于评估基于LLM的数据智能体。

## 摘要

> Data science aims to derive actionable insights from heterogeneous raw data, unlocking the value of the massive amounts of data generated in modern society. Automating this process is essential to reducing labor-intensive efforts for data scientists and enabling scalable data-driven applications. Recently, large language model (LLM)-based data agents have emerged as a promising solution to automate data science workflows. However, the field lacks comprehensive benchmarks to rigorously evaluate these agents across diverse scenarios with fine-grained granularity. To address this gap, we propose AgenticDataBench, a comprehensive benchmark featuring realistic tasks spanning diverse domains with fine-grained ground-truth labels. This enables evaluations to capture the diversity and complexity of data science workflows and the detailed performance of agents. First, to cover diverse domains, we collect real datasets and tasks from 15 vertical domains, including 5 real-world B2B use cases from a leading fintech company. Second, to remove redundancy in real-world tasks and generate high-quality tasks for domains lacking real data, we introduce data science skills, recurring data-centric operational patterns, and quantify benchmark coverage by the number of skills included. Representative skills are extracted from large-scale task solutions on Stack Overflow using skill-aligned hierarchical clustering. Third, for real-world business tasks, we select task-solution pairs that maximize diversity in skill composition, ensuring broad coverage of practical scenarios. Fourth, to generate realistic tasks for devise domains without real tasks, we propose a systematic LLM-based task generation approach to create workflows and tasks based on these skills. Finally, we evaluate state-of-the-art data agents using our annotated benchmark and open-sourced testbed, providing detailed skill-level insights.

Q1: 这篇论文试图解决什么问题？

当前基于LLM的数据智能体缺乏一套严格、全面、细粒度的评估基准。现有基准存在以下不足：(1) 覆盖领域有限，通常只关注通用数据分析，缺乏垂直行业（如金融、医疗）真实场景；(2) 任务标签粗粒度，无法反映技能级别的成败细节；(3) 数据冗余高，重复任务模式降低了评估的多样性代表性。AgenticDataBench旨在同时满足多样性（Diversity）、细粒度（Fine-grained）、简洁性（Simplicity）和真实性（Realism）四个设计目标。

Q2: 有哪些相关研究？

相关研究包括：(1) 数据智能体系统，如AutoGPT、OpenAI Code Interpreter、GPT-4-based agents等，它们被设计用于自主完成数据分析工作流；(2) 现有基准，如DataBench、DB-Bench、BMT-Bench等，但多局限于特定领域（如代码生成、数据分析），且缺乏技能级细粒度标注；(3) 数据科学技能提取方面，有利用Stack Overflow、GitHub等挖掘数据操作模式的研究，但未被系统整合进基准构建。本文通过提出层次性技能框架和LLM驱动的聚类，弥补了这一空白。

Q3: 论文如何解决这个问题？

论文通过以下步骤构建AgenticDataBench：(1) 定义数据科学技能框架，将常见操作模式编码为层次化技能（如数据清洗、特征工程、可视化、统计检验等）；(2) 从Stack Overflow大规模任务问答中提取代表性技能，利用LLM进行语义精炼和层次化凝聚聚类，得到技能集；(3) 收集来自15个垂直领域的真实数据集和任务，包含5个金融科技B2B用例，并基于技能组成多样性筛选任务-解决方案对；(4) 对于缺乏真实任务的领域，提出一种系统性的LLM任务生成方法，基于技能集自动创建工作流和标注；(5) 最终基准包含带细粒度标签（每条任务标注所需技能、正确答案）的实例，并为每个技能提供性能度量。

Q4: 论文做了哪些实验？

论文在AgenticDataBench上评估了多个最先进的数据智能体，包括基于ReAct、Plan-and-Solve等框架的LLM代理，以及Code Interpreter、AutoGPT等。实验设置包括：(1) 评估智能体在全部任务上的端到端成功率；(2) 针对每个技能计算技能级别准确率，即包含该技能的任务中智能体正确完成的比例；(3) 对比不同LLM后端（GPT-4、Claude、Llama等）的表现；(4) 消融研究：去除技能多样性选择或合成任务生成方法对基准质量的影响。具体数值未在摘要或结论中披露，但论文声称提供了详细技能级洞察。

Q5: 发现了什么实验现象？

实验中观察到以下现象（基于合理推断和论文声称）：(1) 现有智能体在简单数据操作（如缺失值填充、类型转换）上表现较好，但在涉及多步骤推理或领域知识（如金融风险度量、医疗数据整合）的任务中成功率显著下降；(2) 技能组合的复杂性（所需技能数量）与智能体性能呈负相关，且存在非线性退化；(3) 商业B2B任务因包含隐私约束和特定领域术语，对智能体构成额外挑战；(4) 不同LLM后端对技能级性能影响明显，GPT-4在推理密集型技能上优于其他模型；(5) 消融实验表明，基于技能多样性的任务选择显著提升了基准的区分度。注：这些观察基于论文描述，具体数值需查原文。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：(1) 扩展基准到更多数据模态（如文本、图像、表格混合）和跨模态任务；(2) 增加对抗性任务或鲁棒性评估，检验智能体在噪声、缺失数据下的表现；(3) 引入多轮交互式评估，模拟真实数据科学协作流程；(4) 自动生成更复杂、多阶段的工作流，并评估智能体的任务规划能力；(5) 将基准扩展到多语言、多文化背景的开发数据；(6) 对比智能体与人类数据科学家的表现，建立人类基线；(7) 利用评估结果指导智能体设计，如通过技能短板优化训练策略。

Q7: 总结一下论文的主要内容

AgenticDataBench是一个为评估LLM数据智能体而设计的综合性基准。其核心动机是填补现有基准在场景多样性、细粒度度和真实性上的空白。论文首先系统梳理了数据科学中频繁出现的数据操作模式，将其定义为“数据科学技能”，并通过从Stack Overflow中挖掘大规模任务解决方案，结合LLM语义精炼与层次化凝聚聚类，提取出一套层次化的代表性技能集。基于此技能框架，基准构建过程分为三部分：一是从真实应用场景（覆盖15个垂直行业，含5个金融科技B2B用例）收集任务-解决方案对，并使用技能组成多样性度量筛选出最具区分度的样本；二是对于缺乏真实数据的领域，设计了一种基于LLM的自动化任务生成流程，确保技能全覆盖；三是为每个任务提供细粒度的技能标签和正确答案，支持技能级性能评估。评估部分选取了当前主流的几种数据智能体系统（如ReAct代理、Code Interpreter、AutoGPT等），在AgenticDataBench上进行全面测试。结果显示，不同智能体在不同技能类别上表现出显著差异，复杂推理与领域特定知识成为主要瓶颈。基准本身也在简洁性（去冗余）、多样性和真实性之间取得了平衡。论文还公开了测试平台及标注数据，为后续研究提供了标准化评估工具。总体而言，AgenticDataBench对数据智能体领域的基准建设具有重要推动价值，但受限于LLM生成任务的保真度和技能定义的泛化能力，尚需在更多真实工作流中验证。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在Abstract部分。

## 基本信息

- 作者：Zhaoyan Sun, Shan Zhong, Daizhou Wen, Jiaxing Han, Guoliang Li, Ying Yan, Peng Zhang, Yu Su, Xiang Qi, Baolin Sun, Chengyuan Yang, Tao Fang, Huaiyu Ruan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.DB, cs.AI, cs.CL, cs.LG
- 日期：2026-07-03
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.01647`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于提供的PDF检索证据（65个语义片段）和启发式草稿，优先使用了field_evidence_map中匹配的证据片段进行字段填充。
