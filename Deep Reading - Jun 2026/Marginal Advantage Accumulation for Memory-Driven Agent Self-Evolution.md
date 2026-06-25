---
user_id: "cheng tan"
paper_id: 865
arxiv_id: "2606.20475v1"
title: "Marginal Advantage Accumulation for Memory-Driven Agent Self-Evolution"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.20475v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.20475v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-20T01:28:07"
---
# Marginal Advantage Accumulation for Memory-Driven Agent Self-Evolution

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：marginal advantage accumulation · operation-level evidence accumulation · trace distillation · agent self-evolution

## 一句话总结

本文提出边际优势累积（MAA）方法，通过差分信号和EMA累积跨批次操作级证据，在16个设置中的14个取得最佳结果，并减少75% token消耗。

## 摘要

> In batch-style trace distillation, the same memory operation may receive contradictory feedback across different batches. Existing methods lack a cross-batch, operation-level evidence accumulation mechanism, making it impossible to distinguish stably effective operations from accidental hits. This paper formalizes the requirement as two structural conditions, alignability and comparability, and proposes Marginal Advantage Accumulation (MAA). MAA constructs differential signals to make them comparable across batches, accumulates signed evidence per operation via EMA, and ensures cross-batch traceability through semantic identity merging. As a post-processing architecture, MAA achieves the best results in 14 out of 16 settings across 4 benchmarks and 4 target models, consistently outperforming existing batch-level distillation baselines and matching or surpassing online alternatives in most settings, while reducing optimization-phase token consumption by approximately 75%.
> Keywords: Marginal advantage accumulation, operation-level evidence accumulation, trace distillation, agent self-evolution, offline memory optimization

Q1: 这篇论文试图解决什么问题？

在批量式痕迹蒸馏（batch-style trace distillation）中，用于记忆优化的同一操作在不同批次可能获得矛盾的反馈信号。现有方法缺乏跨批次、操作级的证据累积机制，导致无法区分稳定有效的操作（consistently effective operations）与偶然命中（accidental hits）。论文形式化了两个必要条件：可对齐性（alignability，同一操作在不同批次间可稳定追踪）和可比性（comparability，不同批次的差分信号可比）。

Q2: 有哪些相关研究？

相关工作包括：（1）在线记忆优化方法（如ReACT），每次迭代使用当前模型生成反馈，但token消耗高；（2）批量式痕迹蒸馏方法（如SkillOpt），在离线阶段对静态轨迹批量优化，但跨批次操作级证据无法累积；（3）基于agent的记忆进化方法，如使用LLM作为优化器，但难以保持跨批次一致性。本文的MAA作为后处理架构，填补了操作级证据累积的空白。

Q3: 论文如何解决这个问题？

MAA以句子对格式的轨迹（traces）和操作列表（propose edit operations）为输入。核心方法：（1）差分信号构建（differential construction）：对每个操作计算跨批次的信号差值，使其可比；（2）EMA累积：对有符号差分信号进行指数移动平均（EMA），累积每个操作的历史证据；（3）语义身份合并（semantic identity merging）：通过语义相似度将不同批次中语义相同的操作合并为同一累积单元，确保跨批次追踪。MAA作为后处理模块，不依赖模型在线推理，可附加在任意批量式蒸馏流程之后。

Q4: 论文做了哪些实验？

论文在4个基准测试（HotpotQA, ALFWorld, SpreadsheetBench, ScienceAgentBench）和4个目标模型（GPT-5.4, Qwen3.7-Max, GPT-4.1, Qwen3-235B）上进行实验。设置了三个主要实验：（1）主实验结果（RQ1）：MAA在16个设置中14个取得最佳，在Qwen3.7-Max和GPT-5.4上超越SkillOpt所有数据集（GPT-5.4: ScienceAgentBench +0.9pp, ALFWorld +0.9pp, SpreadsheetBench +1.7pp）；（2）设计选择消融（RQ2）：验证差分构造的必要性和连续幅度的额外收益，abs-score EMA仅作为目标消融（表现劣于完整MAA）；（3）学习曲线与案例研究（RQ3）：MAA在epoch 3-4后与Reactive方法拉开差距，收敛速度因任务复杂度而异（HotpotQA 6 epoch达到测试性能，ALFWorld和SpreadsheetBench需要更多epoch）。

Q5: 发现了什么实验现象？

（1）MAA与Reactive方法对比：在全部四个数据集上，MAA从epoch 3-4开始拉开差距，但收敛速度与任务复杂度显著相关：HotpotQA（简单多跳QA）约6 epoch达到测试级性能；ALFWorld和SpreadsheetBench因步骤级决策复杂需要更多epoch。（2）证据轨迹案例：稳定有效操作（如通用导航策略“如果未找到目标则切换容器”）的差分信号持续为正（+6, +12...），EMA置信度高；而偶然命中操作（如特定位置无关的容器检查）的信号符号交替，EMA置信度低。（3）方向一致时EMA快速收敛，但部分操作因方向不一致导致EMA退化（如信号正负交替，累积值接近0）。

Q6: 有什么可以进一步探索的点？

（1）探索更复杂的操作类型（如条件分支、循环）的累积机制；（2）将MAA扩展到在线混合设置，结合在线反馈与离线累积；（3）研究操作语义身份合并的鲁棒性，处理同义但表达不同的操作；（4）在更大型的agent记忆库或多模态场景中验证MAA的扩展性；（5）降低MAA本身的token开销（当前已减少75%，但仍有优化空间）。

Q7: 总结一下论文的主要内容

本文针对批量式痕迹蒸馏中同一操作跨批次信号矛盾的问题，提出边际优势累积（MAA）方法。MAA形式化了可对齐性和可比性两个结构条件，通过差分信号构建、EMA有符号证据累积和语义身份合并实现跨批次操作级证据的稳定积累。作为后处理架构，MAA不修改训练流程，可灵活集成。在4个benchmark和4个模型上的16个设置中，MAA取得14个最佳结果，显著优于现有批量式基线（如SkillOpt），并在大多数设置中匹配或超越在线方法（如ReACT），同时优化阶段token消耗减少约75%。消融实验证实差分构造和连续幅度的重要性。学习曲线分析显示MAA在epoch 3-4后开始领先，收敛速度受任务复杂度影响。案例研究表明MAA能可靠区分稳定有效操作（长期正信号）和偶然命中（交替信号）。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本文直接针对智能体记忆优化中的证据累积问题，与用户画像中的agent方向（权重0.10）高度相关。

## 基本信息

- 作者：Mingyu Yang, Keye Zheng, Congchao Cheng, Yujie Liu, Xingkang Lu, Fan Jiang, Yefei Zheng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-06-18
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.20475v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本文档基于PDF检索证据（哈希768，60个chunk）和heuristic_draft综合生成；核心信息来自检索到的Abstract、Propose Channel、Main Experimental Results等片段，推断部分已标注。
