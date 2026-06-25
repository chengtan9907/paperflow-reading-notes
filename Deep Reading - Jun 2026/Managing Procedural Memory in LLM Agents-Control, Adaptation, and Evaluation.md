---
user_id: "cheng tan"
paper_id: 1237
arxiv_id: "2606.23127"
title: "Managing Procedural Memory in LLM Agents: Control, Adaptation, and Evaluation"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23127.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23127"
abs_url: "https://arxiv.org/abs/2606.23127"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:13:44"
---
# Managing Procedural Memory in LLM Agents: Control, Adaptation, and Evaluation

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：procedural memory · llm agents · skill transfer · benchmark

## 一句话总结

本文引入AFTER基准，评估LLM代理中程序性记忆的技能迁移，发现单次精炼带来3.7-6.7点提升，跨模型准确率达73.1%，技能泛化程度因角色而异。

## 摘要

> Procedural memory is increasingly used to improve LLM agents on recurring workplace tasks, yet its ability to produce reusable skills remains poorly understood. We introduce AFTER, a benchmark of 382 realistic enterprise tasks spanning six professional roles and 22 procedural skills, designed to evaluate how skills transfer across tasks, roles, and model backbones. The benchmark includes controlled evaluation settings for local improvement, cross-task transfer, cross-role transfer, and cross-model generalization. Experiments show that procedural memory delivers consistent gains in industrial workflows: a single refinement round improves aggregate performance by 3.7-6.7 points, while skills evolved from diverse multi-model execution traces achieve 73.1% cross-model test accuracy, outperforming all single-model trace sources. We further find that some skills generalize broadly across tasks and models, whereas others become specialized to role-specific workflows and lose effectiveness under transfer. These results provide practical guidance for building, evaluating, and deploying procedural memory systems in production agent platforms.

Q1: 这篇论文试图解决什么问题？

现有LLM代理在重复性工作任务中利用程序性记忆来积累经验，但缺乏标准化的基准来系统评估这些程序技能的可迁移性。已有工作通常评估固定、专家策划的技能在固定任务集上的表现，将技能视为静态制品而非从经验中演化的结构。此外，通用代理基准虽然包含真实任务，但未针对程序性记忆的迁移进行专门设计。因此，论文致力于解决以下问题：（1）如何构建一个包含多角色、多任务的真实企业基准以评估程序性记忆？（2）程序技能如何在任务、角色和模型之间迁移？（3）不同源轨迹（单一模型 vs. 多模型）对技能泛化能力的影响如何？

Q2: 有哪些相关研究？

相关工作分为两大部分：一是LLM代理的经验提升方法，早期工作探索了口头自我反思（Shinn et al., 2023）和跨任务技能提取；二是代理基准测试，包括通用代理基准（如GAIA、AgentBench）和专门针对技能迁移的基准（如Li et al., 2026; Liu et al., 2026）。这些工作要么将技能视为静态制品，要么未涉及跨模型迁移。本文提出的AFTER基准填补了这些空白，专注于程序性记忆在多个迁移场景下的评估。

Q3: 论文如何解决这个问题？

论文提出AFTER（Agent Framework for Transfer Evaluation of pRocedural memory）基准，包含382个真实企业任务，覆盖6个角色（如工程师、数据分析师等）和22个程序技能。基准设计四种受控评估设置：（1）局部改进：同一任务同一技能上精炼；（2）跨任务迁移：同一角色不同任务间技能迁移；（3）跨角色迁移：不同角色间技能迁移；（4）跨模型泛化：在未见过的模型上测试技能。技能从代理执行轨迹中提取，格式化为结构化指南。基准包括固定任务集和评估指标（如全程准确率、部分准确率）。

Q4: 论文做了哪些实验？

论文在AFTER基准上进行了一系列实验：（1）局部改进实验：单次精炼前后性能对比，使用GPT-4作为基座模型；（2）跨任务迁移实验：将同一技能应用至同一角色的不同任务；（3）跨角色迁移实验：将技能从一个角色转移至另一个角色；（4）跨模型泛化实验：在不同模型骨干（如Llama、Claude）上测试由多种模型轨迹演化出的技能。实验涵盖6个角色、22个技能和382个任务。对比基线包括无程序性记忆的代理（零样本）、单一模型轨迹源、多模型轨迹源等。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：（1）单次精炼轮次使整体性能提升3.7-6.7个百分点，表明程序性记忆能快速带来增益；（2）跨任务和跨角色迁移时，技能泛化能力差异显著：一些技能（如数据清洗）能很好泛化，另一些（如财务报告）则高度专用化；（3）跨模型泛化实验中，由多模型轨迹演化出的技能达到73.1%的测试准确率，显著优于任何单一模型轨迹源（最佳单一源约65%），说明多样化的轨迹有助于学习更通用的技能；（4）技能专用化程度与角色任务一致性相关，角色内任务相似的技能更易迁移。

Q6: 有什么可以进一步探索的点？

论文指出以下未来方向：（1）扩大基准覆盖领域，包括医疗、法律、科学研究等非技术行业；（2）研究轨迹数量与迁移质量的关系，因为实际部署中轨迹池可能远大于实验规模；（3）探索程序性记忆的持续学习设置，随着新任务和角色出现动态演化技能；（4）结合其他记忆形式（如情景记忆、语义记忆）以增强泛化；（5）开发自动化的技能提取和精炼流程，减少人工干预。

Q7: 总结一下论文的主要内容

本文针对LLM代理中程序性记忆的可迁移性评估问题，提出了AFTER基准。该基准包含382个真实企业任务、6个专业角色和22个程序技能，并通过局部改进、跨任务、跨角色和跨模型四种设置系统评估技能迁移性能。实验发现：单次精炼带来3.7-6.7点提升；多模型轨迹源在跨模型测试中达到73.1%准确率，优于单模型源；技能泛化能力呈现分化——部分技能广泛适用，部分高度专用化。这些发现为构建、评估和部署程序性记忆系统提供了实践指南。论文的贡献在于提供了一个标准化的评估框架、揭示了技能迁移的关键特性和影响因素。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文直接针对LLM代理中程序性记忆的评估，与智能体（agent）研究方向高度相关（权重0.10）。

## 基本信息

- 作者：Julia Belikova, Rauf Parchiev, Evgeny Egorov, Grigorii Davydenko, Gleb Gusev, Andrey Savchenko, Maksim Makarenko
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.SE
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23127`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（abstract、introduction、related work、limitations等片段），并结合heuristic_draft进行分析和补全。
