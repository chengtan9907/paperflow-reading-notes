---
user_id: "cheng tan"
paper_id: 1428
arxiv_id: "2606.25530v1"
title: "Evaluating LLMs on Real-World Software Performance Optimization"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25530v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25530v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T18:08:03"
---
# Evaluating LLMs on Real-World Software Performance Optimization

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · code optimization · performance benchmark · runtime

## 一句话总结

当前大语言模型在真实软件性能优化上表现严重不足：运行时间改进微乎其微，内存优化几乎不存在，与专家实现（15.5×加速、171.3×内存降低）形成鲜明对比。

## 摘要

> Software performance optimization is a notoriously complex and manual task. Despite the growing use of Large Language Models (LLMs) for code refinement, we still lack benchmarks that capture how optimization actually happens in real-world codebases. Existing frameworks often oversimplify the problem by focusing on isolated functions or a single performance metric, missing the critical trade-offs between execution time and memory footprint, the inherent noise of the measurement environment, and the variability introduced by different input data and execution conditions. We address this by introducing SWE-Pro, a repository-level benchmark derived from 102 expert-written optimizations from open-source projects. Unlike previous benchmarks, SWE-Pro pairs each task with parameterized tests to evaluate runtime, peak memory, and Time-Weighted Memory Usage (TWMU) across varying input data and execution conditions under noise-aware measurement conditions. Our evaluation shows that current LLMs struggle significantly: runtime gains are negligible, and memory optimizations are nearly non-existent. This stands in sharp contrast to expert implementations, which achieve an aggregate speedup of 15.5x and peak memory reduction of 171.3x over benchmark tasks. Expert-written improvements are observed in 91.2% of tasks for runtime and 65.7% for peak memory. Our findings expose a substantial gap between current LLM capabilities and the demands of expert-level engineering.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决以下问题：现有软件性能优化基准过于简化，未能反映真实代码库中的优化复杂性和多维权衡，导致LLM优化能力的评估不准确。具体来说：1）现有基准通常基于孤立函数或单一性能指标（如仅运行时间），忽略了运行时与内存占用之间的权衡；2）测量环境的噪声（系统方差）未被有效处理，导致结果不可靠；3）输入数据与执行条件的变化被忽视，缺乏对优化鲁棒性的评估。因此，缺乏一个能够真实反映专家级优化实践、支持多指标评估且考虑噪声的基准，进而无法准确衡量LLM在真实世界性能优化上的能力。

Q2: 有哪些相关研究？

相关工作主要包括：1）SWE-Perf：一个仓库级性能优化基准，但主要关注程序正确性与效率的平衡，且未系统考虑内存优化和噪声处理。2）现有代码生成/优化评估框架（如HumanEval、MBPP）聚焦于功能正确性，而非性能优化。3）一些研究使用LLM进行代码优化，但多在孤立函数上评估，且指标单一。4）系统噪声处理方面，已有工作提出统计检验方法（如置信区间宽度）用于稳定测量，但未集成到优化基准中。本文SWE-Pro在以上基础上，综合了多指标评估、参数化测试及噪声感知测量，首次实现了仓库级的多维性能优化评估。

Q3: 论文如何解决这个问题？

论文提出SWE-Pro，一个仓库级性能优化基准，用于评估LLM在真实世界软件性能优化中的能力。具体方法包括：1）任务构建：从开源项目的专家编写优化中选取102个任务，每个任务包含原始代码、优化补丁及对应的参数化性能测试。2）多指标评估：同时测量运行时（runtime）、峰值内存（peak memory）和时间加权内存使用（TWMU），以捕捉效率与资源消耗的权衡。3）噪声感知测量：采用90%相对置信区间宽度（RCIW）收敛准则，有效过滤系统方差，能够在87%的专家级改进上复现优化效果。4）参数化测试：通过参数化输入数据与执行条件，评估优化在不同场景下的鲁棒性。5）评测协议：将LLM作为优化代理，给定仓库上下文，要求生成优化补丁，并与专家优化进行多指标对比。

Q4: 论文做了哪些实验？

论文在SWE-Pro基准上对多种当前最先进的大语言模型进行了系统实验：1）LLM选择：包括多个主流模型（如GPT-4、Claude、Llama系列等，具体列表需确认），但摘要未详细列出，从证据推断至少涵盖了常见商用和开源模型。2）评估设置：每个任务在多种输入参数与执行条件下运行，重复多次以获取稳定的性能数据。3）对比基线：包括原始未优化代码（baseline）和专家编写的优化补丁（expert）。4）指标：主要报告运行时加速比、峰值内存降低比和TWMU改进。5）额外分析：消融研究包括不同检索策略（如Oracle检索、无检索）对优化效果的影响。6）噪声过滤效果验证：通过RCIW阈值设置，验证了87%专家级改进的可复现性。

Q5: 发现了什么实验现象？

实验揭示以下关键现象：1）LLM在运行时优化上效果极差：生成的补丁在Oracle检索下平均运行时加速比仅为0.69×，意味着性能反而下降；TWMU也呈现类似下降趋势（0.85×）。2）内存优化几乎不存在：LLM生成的补丁几乎无法降低峰值内存，甚至经常增加内存使用。3）专家优化效果显著：在91.2%的任务上实现运行时改进，65.7%的任务上实现峰值内存改进；平均加速比达15.5×，内存降低达171.3×。4）噪声过滤方法有效：RCIW收敛准则能够复现87%的专家级改进，但LLM优化仍不稳定。5）检索策略影响：Oracle检索（给予正确文件上下文）相比无检索有轻微改善，但整体仍远落后于专家。6）负结果：LLM生成的补丁往往引入功能错误或降低性能，显示出当前模型缺乏对性能瓶颈的深入理解。

Q6: 有什么可以进一步探索的点？

1）多语言支持：当前SWE-Pro仅包含Python仓库，扩展到C/C++、Java等性能敏感语言可提升基准覆盖面。2）更多领域：当前聚焦数据密集型库，可扩展至系统软件、图形处理、数据库等。3）改进LLM优化能力：探索检索增强、规划策略、迭代优化、或结合静态分析工具的方法。4）噪声处理优化：更精细的测量协议或自适应RCIW阈值。5）扩展评估指标：增加能耗、I/O、并行效率等维度。6）自动化任务生成：减少人工专家编写的工作量，实现更大规模基准。7）研究LLM优化失败的原因：分析模型在优化推理中的具体短板（如缺乏性能模型、忽视内存布局等）。8）跨任务迁移：探索LLM在优化任务上的学习与泛化能力。

Q7: 总结一下论文的主要内容

本论文针对当前缺乏真实世界软件性能优化基准的问题，提出了SWE-Pro——一个仓库级基准，包含102个人工专家从开源项目中编写的优化任务。每个任务配以参数化性能测试，在噪声感知测量条件下综合评估运行时、峰值内存和TWMU。通过在多个SOTA LLM上的广泛实验，作者发现：1) LLM的运行时优化效果微乎其微，甚至在Oracle检索下性能平均下降0.69×；2) 内存优化几乎不存在，且经常导致内存增加；3) 专家优化平均实现15.5×加速和171.3×内存降低，在91.2%和65.7%的任务上有改进。论文还提出使用90% RCIW收敛准则过滤系统方差，成功复现87%的专家改进。这项工作首次系统性地揭示了当前LLM在仓库级多维性能优化上的巨大不足，为未来研究提供了扎实的评估平台与明确挑战。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：对于代码优化社区：提供了一个真实可靠的评估基准，可促进LLM优化能力的改进。

## 基本信息

- 作者：Ezgi Sarıkayak, Wenchao Gu, Hesham Ghonim, Chunyang Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SE, cs.AI, cs.CL
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25530v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据（包括摘要、引言、结论和限制部分）以及启发式草稿，信息综合自论文元数据与检索片段。
