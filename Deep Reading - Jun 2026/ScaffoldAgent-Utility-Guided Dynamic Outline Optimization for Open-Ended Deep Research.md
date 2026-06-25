---
user_id: "cheng tan"
paper_id: 807
arxiv_id: "2606.20122v1"
title: "ScaffoldAgent: Utility-Guided Dynamic Outline Optimization for Open-Ended Deep Research"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.20122v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.20122v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-20T01:17:23"
---
# ScaffoldAgent: Utility-Guided Dynamic Outline Optimization for Open-Ended Deep Research

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：open-ended deep research · outline optimization · utility-guided · agent

## 一句话总结

ScaffoldAgent 提出一种效用引导的动态大纲优化框架，用于解决开放式深度研究中的大纲漂移和反馈延迟问题。

## 摘要

> Open-ended deep research (OEDR) requires systems to acquire knowledge through multi-round retrieval and generate coherent long-form reports. The outline plays a central role as a structural scaffold that coordinates retrieval, evidence organization, and generation. However, existing methods either fix the outline before writing or refine it with local heuristics, leading to scaffold drift under continuous information accumulation and delayed feedback for evaluating outline modifications. We propose ScaffoldAgent, a utility-guided dynamic outline optimization framework for OEDR. ScaffoldAgent models outline evolution as a structured decision process with three operations: Expansion, Contraction, and Revision, enabling controlled updates to the report scaffold. It further introduces a utility-guided feedback mechanism that estimates the downstream value of each outline operation from retrieval gain, structural coherence, and trial-generation quality. The resulting utility signal guides node selection, operation scheduling, and termination during inference. Experiments on DeepResearch Bench and DeepResearch Gym show that ScaffoldAgent consistently improves long-form report generation and factual grounding over existing deep research agents.

Q1: 这篇论文试图解决什么问题？

开放式深度研究要求系统通过多轮检索获取知识并生成连贯的长篇报告。大纲作为结构化支架，协调检索、证据组织和生成。然而现有方法要么在写作前固定大纲，要么用局部启发式方法进行细化，导致两个关键问题：1) 支架漂移：随着持续信息积累，固定大纲无法适应新证据；2) 反馈延迟：评估大纲修改的效果是滞后的，缺乏统一优化目标同时考虑检索质量、结构合理性和生成质量。

Q2: 有哪些相关研究？

相关研究包括：1) 大纲生成方法：如固定预先写入的大纲或局部启发式更新；2) 检索增强生成中的迭代细化方法，如基于新检索文档更新大纲（Li et al., 2025）；3) 开放式推理和工具使用智能体，但缺乏统一效用目标。现有工作通常只考虑检索或结构的一个方面，未联合优化。

Q3: 论文如何解决这个问题？

ScaffoldAgent 提出效用引导的动态大纲优化框架。关键设计包括：1) 将大纲演化建模为结构化决策过程，包含三种操作：扩展（Expansion）、收缩（Contraction）和修订（Revision），实现受控的大纲更新；2) 效用引导的反馈机制：从检索增益、结构连贯性和试生成质量估计每个大纲操作的下游价值，生成的效用信号指导节点选择、操作调度和推理终止。推理时根据效用信号动态调整大纲演化。

Q4: 论文做了哪些实验？

实验在 DeepResearch Bench 和 DeepResearch Gym 两个基准上进行，评估长报告生成和事实基础。对比基线包括现有深度研究智能体。报告了多项指标，但检索证据中未提供具体数值。合理推断实验包括报告质量、事实准确性等自动和人工评估。

Q5: 发现了什么实验现象？

实验现象表明 ScaffoldAgent 一致性地改进了长报告生成和事实基础（据摘要描述）。具体消融趋势、负结果等未在检索证据中出现。推测大纲动态优化比静态或局部启发式方法更有效，效用信号对操作选择有积极作用。

Q6: 有什么可以进一步探索的点？

论文自身限制部分指出未来方向：1) 当前多轮评估仍是初步的，现有基准主要是单轮，未来需开发更长多轮轨迹和更全面的评估协议；2) 虽然当前设计轻量有效，但动态大纲优化天然是序列决策问题，未来可学习显式的大纲演化策略。其他可探索方向：扩展到交互式研究场景，联合优化检索和生成。

Q7: 总结一下论文的主要内容

本文提出 ScaffoldAgent，一个效用引导的动态大纲优化框架，用于开放式深度研究。该框架将大纲演化建模为包含扩展、收缩和修订三种操作的结构化决策过程，并引入效用引导的反馈机制，从检索增益、结构连贯性和试生成质量估计每个大纲操作的下游价值，从而指导节点选择、操作调度和推理终止。实验在 DeepResearch Bench 和 DeepResearch Gym 上验证，表明 ScaffoldAgent 一致性地改进了长报告生成和事实基础。论文贡献在于提出了一种统一效用目标下的动态大纲优化方法，解决了现有方法中的支架漂移和反馈延迟问题。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与智能体方向直接相关：ScaffoldAgent 是一个深度研究智能体框架。

## 基本信息

- 作者：Zhibang Yang, Xinke Jiang, Yuzhen Xiao, Ruizhe Zhang, Yue Fang, XinFei Wan, Zhengxing Song, Yuxuan Liu, Yuheng Huang, Xu Chu, Junfeng Zhao, Yasha Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.MA
- 日期：2026-06-18
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.20122v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于 heuristic_draft 和 retrieved_evidence（abstract、introduction、conclusion、limitations 片段），部分实验细节为推测，具体数值缺失。
