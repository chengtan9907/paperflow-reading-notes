---
user_id: "cheng tan"
paper_id: 5176
arxiv_id: "2607.19104"
title: "SciCodePile: A 128GB Corpus and Executable Benchmark for Challenging Scientific Code Generation"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.19104.pdf"
pdf_url: "https://arxiv.org/pdf/2607.19104"
abs_url: "https://arxiv.org/abs/2607.19104"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T14:01:42"
---
# SciCodePile: A 128GB Corpus and Executable Benchmark for Challenging Scientific Code Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：scientific code generation · large language models · code corpus · executable benchmark

## 一句话总结

本文提出 SciCodePile，目前最大规模的科学代码语料库（128GB/37k 仓库），并构建 200 个可执行评测任务；评估 15 个 LLM 发现科学代码生成极具挑战（Pass@1 最高仅 12.30%），但继续预训练和指令微调可带来数倍性能提升。

## 摘要

> Large language models (LLMs) excel at general-purpose code generation, yet how well they handle scientific code remains an open question. Existing datasets and benchmarks are limited in scale, domain coverage, or executable verification, leaving the true gap between current LLMs and reliable scientific code generators inadequately assessed. To address these limitations, we present SciCodePile, the largest scientific code corpus to date, constructed from 37,737 public repositories and collectively comprising 128GB of code that spans multiple computational science disciplines. From this corpus, we further curate an executable benchmark of 200 tasks, each equipped with a sandboxed execution environment and an automated test harness for functional verification. We evaluate 15 LLMs from both open-source and closed-source families on three tasks: prefix-to-suffix completion, fill-in-the-middle infilling, and executable code generation. Results show that scientific code generation remains highly challenging: The best CodeBLEU reaches only 38.13 and 38.37 on the two completion tasks, while the strongest model achieves just 12.30\% Pass@1 on the executable benchmark, underscoring how far current models remain from reliable scientific code generation. To demonstrate the training utility of SciCodePile, we further show that continued pretraining on our corpus improves CodeBLEU by $\times$2.84 on scientific code completion, and instruction tuning on our data improves Pass@1 by $\times$4.79 on the executable benchmark. All code and data are available at https://huggingface.co/SciCodePile.

Q1: 这篇论文试图解决什么问题？

当前缺乏一个大规模、跨领域、支持可执行验证的科学代码生成专用语料库和评测基准。现有通用代码生成基准（如 HumanEval、MBPP）不包含科学领域知识，而已有科学代码数据集（如 SciCode、BioCoder）规模有限（通常数百例），且多仅提供静态验证或仅覆盖特定子领域，无法全面评估和针对性优化 LLM 在科学代码生成上的能力。因此，本文旨在填补这一空白，构建高质量、多领域、可执行的资源并据此揭示 LLM 的真实水平。

Q2: 有哪些相关研究？

相关工作分为两类：通用代码生成基准和科学代码数据集。通用基准注重功能正确性但领域知识覆盖不足；科学数据集多为特定领域（如生物信息学、化学），规模小且缺少可执行验证。本文的 SciCodePile 在规模（128GB）和领域覆盖上显著超越现有资源，并率先引入自动执行测试体系。此外，现有代码预训练语料（如 The Stack）虽体量大，但缺乏针对科学领域的过滤和格式化。

Q3: 论文如何解决这个问题？

论文提出端到端构建框架：(1) 仓库检索与筛选：通过关键词从 GitHub 爬取 37,737 个科学计算仓库，经语言、可读性过滤得 128GB 代码；(2) 多格式数据构造：从每个仓库抽取四种互补格式——代码文件、README 摘要、函数指令和函数实现，形成仓库级到指令级的监督信号；(3) 可执行基准任务构建：从语料中选取 200 个科学计算任务，每个包含自然语言描述、代码框架（或空）和 pytest 单元测试，封装在 Docker 沙箱中。评估三种模式：后缀补全、中间填充、指令到可执行代码。

Q4: 论文做了哪些实验？

实验包括：(1) 基准评测：15 个 LLM（闭源如 GPT-4、Claude，开源如 CodeLlama、DeepSeek-Coder），在后缀补全和中间填充上用 CodeBLEU，可执行任务上用 Pass@1/Pass@5；(2) 训练效用验证：在 SciCodePile 子集（约 0.5B token）上进行继续预训练和指令微调，对比模型提升。

Q5: 发现了什么实验现象？

1) 科学代码生成极具挑战：最佳模型 CodeBLEU 仅 38.13（后缀补全）和 38.37（中间填充），Pass@1 仅 12.30%；2) 模型间差距显著，闭源模型整体优于开源，但远未可靠；3) 训练效用显著：继续预训练使 CodeBLEU 提升约 2.84 倍，指令微调使 Pass@1 从 1.90% 升至 9.10%（提升 4.79 倍），但仍低于未训练的最佳模型（12.30%），说明模型原始能力差异大。

Q6: 有什么可以进一步探索的点？

1) 扩展领域覆盖到更多子领域及非 Python 语言；2) 增加任务数量和复杂度，覆盖完整科学工作流；3) 改进验证机制（如浮点容差、统计检验）；4) 探索更大规模模型上的训练效果；5) 研究代码生成对科学可重复性的影响；6) 拓展到代码检索、修复等任务。

Q7: 总结一下论文的主要内容

本文围绕科学代码生成，针对现有资源规模小、领域窄、缺乏可执行验证的不足，提出 SciCodePile。技术主线上：从 GitHub 爬取 37k+ 仓库，经多格式抽取构建 128GB 语料库，并精选 200 个可执行任务（Docker 沙箱 + pytest）。实验主线上：系统评估 15 个 LLM 在三种范式下的表现，发现最优 Pass@1 仅 12.30%；随后通过继续预训练和指令微调，分别带来 2.84 倍 CodeBLEU 提升和 4.79 倍 Pass@1 提升，验证了语料库的训练价值。论证主线上：从“通用代码生成优秀但科学代码生成未知”出发，论证现有资源不足，介绍构建方法，用评测暴露现状差距，再用训练实验证明资源效用，形成完整闭环。主要贡献是提供了领域内最大规模、可执行的科学代码数据集与基准。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接服务 AI for Science 中的科学代码生成，提供大规模、可执行资源，适合作为领域基准。

## 基本信息

- 作者：Weifeng Sun, Ye Fan, Yuchen Chen, Gou Tan, Jieke Shi, Yuan Yidi, Swee Liang Wong, Jonathan Pan, David Lo
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SE, cs.AI
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.19104`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于论文摘要、检索到的语义片段（intro/conclusion/limitations）及 heuristic_draft 综合完成，未直接访问 PDF 全文，主要依据所提供证据，部分细节为合理推断，需核对原文。
