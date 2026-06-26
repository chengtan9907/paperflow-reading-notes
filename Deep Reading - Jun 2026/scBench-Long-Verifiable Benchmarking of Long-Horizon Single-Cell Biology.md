---
user_id: "cheng tan"
paper_id: 1833
arxiv_id: "2606.26563"
title: "scBench-Long: Verifiable Benchmarking of Long-Horizon Single-Cell Biology"
publish_date: "2026-06-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.26563.pdf"
pdf_url: "https://arxiv.org/pdf/2606.26563"
abs_url: "https://arxiv.org/abs/2606.26563"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-26T14:41:58"
---
# scBench-Long: Verifiable Benchmarking of Long-Horizon Single-Cell Biology

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：single-cell biology · benchmark · long-horizon reasoning · AI agent

## 一句话总结

本文提出 scBench-Long，一个用于长视界单细胞生物学的可验证基准，要求 AI 智能体从原始或近原始数据中恢复科学结论，无需预设方法，并报告了最强智能体-工具组合仅 25.4% 的通过率。

## 摘要

> Single-cell studies require analysts to convert raw measurements into specific biological claims through multi-step workflows and integration of metadata, assay context, and auxiliary evidence. Existing AI-biology benchmarks largely measure broad knowledge, executable workflows, or local analysis steps. We introduce scBench-Long, a benchmark for long-horizon single-cell biology in which agents must recover scientific conclusions from raw or near-raw data without prescribed methods. The benchmark contains 21 evaluations spanning melanoma CD8 T-cell reactivity, CD8 RNA+ATAC regulatory inference, human--monkey chimera development, KRAS-driven lung tumor aging, and lethal COVID-19 lung pathology. Tasks cover paired scRNA/TCR sequencing, RNA and chromatin profiling, cross-species transcriptomics, combinatorial scRNA-seq, single-nucleus RNA-seq, immune repertoires, ortholog maps, ligand--receptor resources, and validation evidence. Candidate claims are reproduced, reviewed, and converted into controlled answer vocabularies with deterministic grading and trajectory rubrics. Across 1,068 completed trajectories, the strongest model--harness pair passes 16/63 runs (25.4\%). scBench-Long evaluates whether agents can move beyond local analysis steps and make complex scientific claims that are supported by single-cell data.

Q1: 这篇论文试图解决什么问题？

现有 AI-生物学基准要么测试广泛的生物知识，要么仅评估局部的单细胞分析步骤（如降维、聚类、差异表达），无法衡量 AI 代理执行完整长视界科学推理的能力。单细胞研究需要多步骤工作流、多模态数据整合以及关于实验设计的背景知识，最终结论往往难以确定性评分（同一数据可支持多个合理声明）。本文旨在填补这一空白，提供一个可验证、端到端的长期单细胞分析基准。

Q2: 有哪些相关研究？

相关研究包括：(1) 通用 AI-biology 基准如 BioBench、GeneTasks、MedQA，主要测试知识回忆或简单推理；(2) 单细胞特定基准如 scIB、scEval、scGeneRANK，聚焦局部步骤如批次校正、聚类、差异表达，不评估完整流程；(3) 长视界代理基准如 SWE-bench、AgentBench，但非生物领域；(4) 可验证基准设计如 [28, 29, 36]，采用端点评分和受控词汇表。本文延续了后者的设计范式，但专门针对单细胞场景的多种数据类型和科学声明。

Q3: 论文如何解决这个问题？

scBench-Long 围绕五个研究系统（黑素瘤 CD8 T 细胞反应性、CD8 RNA+ATAC 调控推断、人-猴嵌合体发育、KRAS 驱动肺肿瘤衰老、COVID-19 肺病理）构建 21 个评估。每个评估提供原始或近原始单细胞数据（如 scRNA-seq、scATAC-seq、TCR-seq 等）、实验背景、相关元数据和辅助证据（如直系同源图谱、配体-受体资源）。代理必须执行完整的分析流程（包括质量控制、标准化、降维、聚类、差异表达、通路富集、细胞注释、跨模态整合、统计检验等），最终输出结构化答案（如二进制标签、分类、排序或数值）。答案通过预定义的受控词汇表进行确定性评分（pass/fail），避免歧义。为诊断分析瓶颈，每个评估还配有关键路径规则（rubric），追踪代理是否通过必要的分析节点。评分遵循端点评分策略，只有在最终声明正确时才通过。

Q4: 论文做了哪些实验？

论文构建了 scBench-Long 基准，并通过多个智能体模型和工具组合进行测试。具体实验包括：(1) 智能体-工具组合：Claude 4.8（推测为 Opus 4.8）+ Claude Code、Gemini 3.5 Flash + PI、GPT-4o + 类似工具等；每个评估运行 3 个重复（轨迹），共 21×3=63 条轨迹，总计 1068 条已完成轨迹（推测来自多个模型或多次重复）。(2) 评估协议：代理被提供数据目录和文件，要求根据研究问题执行分析并输出答案。所有运行在受控环境中进行，记录完整工作流和最终结果。(3) 评分：基于终点二进制（通过/失败），并辅以轨迹规则分析关键路径进展。

Q5: 发现了什么实验现象？

1. 整体性能低：最强组合（Claude Opus 4.8 + Claude Code）仅通过 16/63 条轨迹（25.4%），次优（Gemini 3.5 Flash + PI）通过 14/63（22.2%）。没有模型在所有 21 个评估中都通过至少一个重复；只有 8/21 个评估存在至少一个通过重复，仅 2/21 个评估三个重复全部通过。2. 代理普遍能够执行局部分析步骤（如聚类、差异表达），但无法整合所有结果得出正确科学结论。3. 常见失败模式：(a) 用文献先验替代从数据中学习（例如直接报告已知标志物而不验证）；(b) 误将大数值（如高表达计数）等同于生物学重要性；(c) 推断因果关系时忽视混杂因素或无对照设计；(d) 遗漏关键分析步骤（如跨样本整合、批次校正、交叉验证）；(e) 对正交验证证据（如 ATAC-seq 验证基因调控）利用不充分。

Q6: 有什么可以进一步探索的点？

1. 扩展基准覆盖范围：增加空间生物学、表观基因组学（如 scCUT&Tag 等）、治疗学（药物结合），以及更多疾病和治疗场景。2. 引入开放角色评估：除了 binary pass/fail，设计部分成功度量或置信度评分。3. 改进可验证性：允许多个有效答案路径，同时保持评分确定性（如通过更细粒度的受控词汇）。4. 开发反馈机制：向代理提供错误中间结果或提示，模拟迭代科学过程。5. 跨模态和跨物种整合：当前已包含部分，可进一步扩展复杂层级。6. 训练代理利用正交证据（如实验室验证实验）增强结论可靠性。

Q7: 总结一下论文的主要内容

本文提出 scBench-Long，一个专注于长视界单细胞生物学的可验证基准。研究动机在于现有基准无法评估 AI 代理完成多步骤科学推理的能力。scBench-Long 包含 21 个评估，覆盖五个研究系统（黑色素瘤免疫反应、RNA+ATAC 调控、人-猴嵌合体、肺癌衰老、COVID-19 病理），每个任务要求代理从原始或近原始单细胞数据出发，整合多种数据类型和背景知识，最终输出符合受控词汇的科学声明。基准设计强调可验证性，通过预定义的答案词汇和确定性评分（pass/fail）确保评估客观，同时辅以轨迹规则追踪关键分析步骤。实验表明，最强模型-工具组合通过率仅 25.4%，代理普遍在执行完整推理和避免已知失败模式（如依赖文献先验、混淆数值大小与重要性）上存在不足。作者将 scBench-Long 定位为更广泛可验证基准家族的一部分，并讨论未来扩展方向。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：直接面向 AI for Science 领域，特别是单细胞数据分析自动化。

## 基本信息

- 作者：Ian Diks, Zhen Yang, Arjun Banerjee, Tim Proctor, Kenny Workman
- 机构：未提供
- 来源：arxiv
- 主题/分类：q-bio.GN, cs.AI
- 日期：2026-06-26
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.26563`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 检索证据及摘要/Introduction/Discussion 片段，其中 Discussion 片段补充了核心方法和失败模式。
