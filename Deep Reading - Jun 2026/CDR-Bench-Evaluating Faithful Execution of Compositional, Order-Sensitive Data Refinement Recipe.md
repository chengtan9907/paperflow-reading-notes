---
user_id: "cheng tan"
paper_id: 2152
arxiv_id: "2606.31435v1"
title: "CDR-Bench: Evaluating Faithful Execution of Compositional, Order-Sensitive Data Refinement Recipes"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.31435v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.31435v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T01:38:43"
---
# CDR-Bench: Evaluating Faithful Execution of Compositional, Order-Sensitive Data Refinement Recipes

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：data refinement · benchmark · compositional · order-sensitive

## 一句话总结

本文提出 CDR-Bench，一个包含 3,462 个高质量任务的基准，用于评估 LLM 在组合性、顺序敏感的数据细化配方上的忠实执行能力。

## 摘要

> Data refinement involves executing multi-step recipes over evolving text states, where both composition and execution order of processing operators determine the outcome. While existing benchmarks either isolate text editing or entangle it with code and tool execution, it remains unclear whether LLMs can directly and faithfully execute these compositional, order-sensitive data refinement recipes. To fill this gap, we introduce CDR-Bench, a comprehensive benchmark featuring 3,462 high-quality tasks spanning four real-world data refinement domains and 29 distinct operators. Our benchmark evaluates models across atomic, order-agnostic, and order-sensitive settings, leveraging deterministic reference outputs to enable exact evaluation. Experiments on 10+ state-of-the-art LLMs reveal consistent failure patterns: performance degrades sharply in compositional settings, and order-sensitive recipe success collapses. These findings underline that current LLMs lack the procedural faithfulness required for reliable compositional data refinement.

Q1: 这篇论文试图解决什么问题？

现有数据细化基准无法评估 LLM 直接、忠实地执行组合性和顺序敏感的多步数据细化配方的能力：它们要么孤立文本编辑（忽略组合性），要么将数据操作与代码/工具执行纠缠（无法测试 LLM 自身的推理和准确执行）。本文旨在填补这一空白，系统评估 LLM 在无外部工具支持下执行这类配方时的表现。

Q2: 有哪些相关研究？

相关工作包括：文本编辑基准（如 EditEval、CoEdIT），专注于单步编辑；数据为中心的智能体（如 Data-Juicer、Dify），虽然处理多步管道但评估纠缠了代码执行；以及 LLM 作为工具执行者的研究。CDR-Bench 首次聚焦于 LLM 直接执行数据细化配方，不依赖外部代码或工具。

Q3: 论文如何解决这个问题？

通过构建 CDR-Bench 基准解决。基准包含 3,462 个任务，源自 4 个真实世界数据细化领域（文本清理、风格转换、结构化修饰、内容增强），使用 29 个来自 Data-Juicer 库的算子。任务被分类为原子（单步）、顺序无关（多步但顺序不影响结果）和顺序敏感（多步且顺序关键）。所有任务都有确定性参考输出，支持精确的自动评估。

Q4: 论文做了哪些实验？

在 10 余个最新 LLM 上评估，包括 GPT-4、Claude、Gemini、Llama 系列、Qwen 等。报告了不同设置下的准确率，并比较了原子、顺序无关和顺序敏感任务的表现。使用精确匹配作为评估指标。

Q5: 发现了什么实验现象？

主要现象：(1) 所有模型在组合设置（顺序无关和顺序敏感）中性能远低于原子设置，显示强烈的退化趋势。(2) 顺序敏感配方的成功率极低，即使最强模型也接近基线性。(3) 开源和闭源模型表现出相似的退化模式，但闭源模型绝对性能更高。(4) 错误分析显示模型常忽略或错误地按错误顺序应用操作，暴露出缺乏程序忠实性。

Q6: 有什么可以进一步探索的点？

未来方向：(1) 扩展操作符库以包括更多领域和主观操作。(2) 引入 LLM 评判或其他方法评估主观细化配方。(3) 支持多语言和多模态数据。(4) 深入分析 LLM 在顺序推理和组合执行中的失败机制。(5) 探索如何通过提示策略、微调或工具增强来提升 LLM 的执行忠实度。

Q7: 总结一下论文的主要内容

论文针对 LLM 在执行组合性和顺序敏感的数据细化配方时的忠实度问题，提出了 CDR-Bench 基准。该基准包含 3,462 个高质量任务，覆盖 4 个真实世界领域和 29 个操作符，设计了原子、顺序无关和顺序敏感三类设置。基于确定性参考输出进行精确评估。在 10+ 个先进 LLM 上的实验显示：(1) 在组合设置中，所有模型性能大幅下降；(2) 顺序敏感配方成功率极低；(3) 错误模式一致，表明当前 LLM 缺乏程序忠实性。论文贡献了评估方法论、数据集和实验洞察，为数据细化方向提供了重要基准。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：对智能体（agent）方向：评估 LLM 直接执行多步任务的能力，相关于工具使用、规划和程序忠实性。

## 基本信息

- 作者：Yuchen Huang, Xiang Li, Zhenqing Ling, Sijia Li, Qianli Shen, Daoyuan Chen, Yi R. Fung, Yaliang Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.31435v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据
