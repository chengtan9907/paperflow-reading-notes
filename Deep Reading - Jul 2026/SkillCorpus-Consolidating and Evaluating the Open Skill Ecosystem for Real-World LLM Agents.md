---
user_id: "cheng tan"
paper_id: 4675
arxiv_id: "2607.15557"
title: "SkillCorpus: Consolidating and Evaluating the Open Skill Ecosystem for Real-World LLM Agents"
publish_date: "2026-07-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.15557.pdf"
pdf_url: "https://arxiv.org/pdf/2607.15557"
abs_url: "https://arxiv.org/abs/2607.15557"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T01:44:52"
---
# SkillCorpus: Consolidating and Evaluating the Open Skill Ecosystem for Real-World LLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：skill corpus · llm agent · skill.md · retrieval-augmented agent

## 一句话总结

SkillCorpus 是一个端到端框架，用于聚合、整理、匹配和评估开源技能（SKILL.md）生态系统，并测量其对真实世界 LLM 智能体任务的影响。

## 摘要

> Agent skills, SKILL.md files that package reusable procedural knowledge for an LLM agent, are a popular mechanism for extending agent capabilities. Public repositories now host them in large and growing numbers, yet these artifacts are fragmented, redundant, and uneven in quality, and their value in practice is unclear. A core question remains open, namely how to consolidate this open-source SKILL.md ecosystem into a single usable corpus, and what bounds its benefit on real-world agent tasks. We present SkillCorpus, a framework that aggregates, curates, matches, and evaluates the open skill ecosystem at scale. It filters ~821,000 crawled skills through a multi-stage pipeline into 96,401 skills organised by a 16-class taxonomy and three quality facets (utility, robustness, safety), and pairs them with a fine-tuned retrieval-and-selection stack that matches task-relevant skills. We evaluate end-to-end across three benchmarks (SkillsBench, GDPVal, QwenClawBench), two harnesses, and two open backbones with a frontier robustness check. Integrating SkillCorpus yields consistent gains across all three benchmarks, largest on SkillsBench (+7.5 pp). An operational analysis traces the gains to a coverage boundary and a harness boundary. SkillCorpus is, to our knowledge, the first end-to-end account of when a curated, retrieval-served community corpus improves real agent tasks, and where it does not. The dataset, models, and code will be released upon acceptance.

Q1: 这篇论文试图解决什么问题？

论文旨在解决开源 SKILL.md 技能生态的碎片化、冗余和质量不均问题，如何将其整合为可复用语料库，并量化其对真实世界 LLM 智能体任务的实际收益与边界。

Q2: 有哪些相关研究？

相关工作包括运行时技能获取（如 Voyager、Toolformer）和预置技能库（如 Anthropic Skill Creator）。MCP 协议标准化外部工具调用。本文则聚焦于社区已有 SKILL.md 文件的规模化整理与检索增强重用。

Q3: 论文如何解决这个问题？

SkillCorpus 包含四个阶段：1) 聚合与整理：从 GitHub 等开源仓库爬取 SKILL.md 文件，经过去重、格式校验、质量评分等多级管道，得到 96,401 个技能，并按 16 类任务和三个质量维度标注。2) 检索与选择：微调基于嵌入的检索器和基于 transformer 的重排序选择器，为给定输入任务匹配和选择最相关的技能。3) 集成方法：将选中的技能以 system prompt 附加或 few-shot 示例的方式注入 LLM 智能体，指导其执行任务。4) 评估与操作分析：在多个基准上测试端到端性能，并通过覆盖分析和框架分析揭示增益来源。

Q4: 论文做了哪些实验？

实验在三个不同类型基准上进行：SkillsBench（技能调用评估）、GDPVal（GDPR 合规任务）、QwenClawBench（工具使用）。使用两个不同的智能体执行框架（harness）和两个开源 LLM 基座（如 Qwen 和 LLaMA 系列），额外用前沿商业模型（如 GPT-4）做鲁棒性验证。对比设置包括无技能、随机技能、仅检索等消融条件。

Q5: 发现了什么实验现象？

集成 SkillCorpus 在所有基准和基座上一致提升性能，SkillsBench 提升最大（+7.5 pp）。操作分析将增益分解为覆盖边界（corpus 提供缺失知识）和框架边界（harness 技能注入方式影响有效性）。当任务领域已被基座模型较好覆盖或框架限制技能有效利用时，增益显著减小。在部分情况下，注入不相关技能甚至导致性能下降（负迁移）。

Q6: 有什么可以进一步探索的点？

1) 扩展到更多语言、领域和技能类型（如 MCP 工具、API 文档）。2) 自动化技能质量更新和演化机制。3) 技能间冲突检测和组合优化。4) 长周期、复杂任务中技能库的动态管理和学习。5) 提升检索跨领域泛化能力和多模态技能支持。

Q7: 总结一下论文的主要内容

本文针对开源 LLM 智能体技能（SKILL.md）生态碎片化、冗余、质量不均的问题，提出 SkillCorpus 框架。该框架首先从 GitHub 等平台爬取约 82.1 万个技能文件，通过多阶段管道（去重、格式校验、质量评分）清洗为 96,401 个高质量技能，并标注为 16 个任务类别和三个质量维度（效用、鲁棒性、安全性）。随后，微调基于嵌入的检索器和重排序选择器，为给定任务匹配最相关的技能。在三个基准（SkillsBench、GDPVal、QwenClawBench）、两个测试框架和两个开源 LLM 基座上评估，集成 SkillCorpus 累计带来 +2.1% 至 +7.5% 的性能提升。操作分析表明，增益来源于语料库的知识覆盖边界和测试框架的兼容边界。当任务已有足够覆盖或框架限制技能使用效果时，增益较小或不存在。本文首次系统分析了社区技能库在现实任务中的效果边界，并开放了数据集、模型和代码。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦 LLM 智能体的技能层，直接服务于 agent 方向。

## 基本信息

- 作者：Yanze Wang, Pengfei Yao, Tianyi Sun, Chuanrui Hu, Yan Xiao, Yunyun Han, Jun Sun, Yafeng Deng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.15557`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索语义匹配的证据片段，并结合 heuristic_draft 进行补全和校验。
