---
user_id: "cheng tan"
paper_id: 3138
arxiv_id: "2607.07702"
title: "From Noisy Traces to Root Causes: Structural Trajectory Analysis and Causal Extraction for Agent Optimization"
publish_date: "2026-07-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.07702.pdf"
pdf_url: "https://arxiv.org/pdf/2607.07702"
abs_url: "https://arxiv.org/abs/2607.07702"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-10T11:14:17"
---
# From Noisy Traces to Root Causes: Structural Trajectory Analysis and Causal Extraction for Agent Optimization

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent optimization · llm reflection · causal extraction · trajectory filtering

## 一句话总结

STRACE通过批次级故障过滤和轨迹内因果定位，从冗余异质的长周期智能体执行轨迹中提取紧凑因果切片，构建高信噪比优化上下文，显著提升基于LLM反思的优化效果。

## 摘要

> The optimization of long-horizon agents increasingly relies on reflection-based mechanisms, where a large language model (LLM) acts as an optimizer to diagnose agent failures and improve agent policies. However, real execution traces are difficult to use directly for optimization: large trace collections are often redundant and heterogeneous, making optimization inefficient and prone to overfitting to low-value failures; meanwhile, each individual trajectory also contains many irrelevant steps, while naive context reduction methods such as truncation or sliding windows can discard causally important evidence and produce misleading optimization signals. To resolve this dilemma, we introduce STRACE (Structural Trajectory Analysis and Causal Extraction), a framework that constructs high signal-noise optimization contexts for more precise and effective optimization. At the batch level, STRACE mines failure patterns to filter redundant traces and retain representative failures; within each selected trace, it performs causal localization over a textual dependency graph to remove non-causal steps and identify the true root-cause module for optimization. Empirical results demonstrate that STRACE significantly outperforms standard context-filtering baselines. Notably, on a challenging formal verification task (VeruSAGE-Bench), it successfully optimizes human-expert designed agents, delivering 1.4× success-rate improvement (42.5% to 58.5%). The code is available at https://github.com/moomight/STRACE.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决长周期智能体优化中执行轨迹的信噪比问题。现有基于LLM反思的优化 (如Reflexion) 直接使用大量冗余且异质的执行轨迹，导致优化器效率低下且容易过拟合低价值失败。同时，单条轨迹中大量非关键步骤会稀释因果信号，而简单的上下文缩减方法 (如截断、滑动窗口) 可能丢弃因果上关键的证据，产生误导性优化信号。因此，亟需一种结构化方法从噪声轨迹中提取紧凑、因果相关的优化上下文。

Q2: 有哪些相关研究？

相关研究主要包括两个方向：(1) 基于反思的智能体优化 (如Reflexion, MAML启发方法)，利用LLM作为优化器从执行错误中学习；(2) 因果根因定位 (如Zhang et al., 2025)，旨在从轨迹中识别导致失败的真正步骤。然而，现有工作要么直接处理原始轨迹导致噪声问题，要么聚焦于单步因果分析，缺乏在批次和轨迹两个层面联合进行冗余过滤与因果紧缩的系统方案。STRACE填补了这一空白。

Q3: 论文如何解决这个问题？

STRACE包含两个核心阶段：(1) 批次级故障模式挖掘：将一批轨迹汇总为统计和结构化诊断，利用故障重发频率和严重性信号过滤冗余或低价值失败，保留代表性痕迹；(2) 轨迹内因果定位：基于代码库或任务规定的先验依赖关系构建文本依赖图，在选定轨迹中通过后向推理去除因果无关步骤，识别根因模块。优化器仅针对根因模块的指令注入预防性改进语句，避免对其他模块产生过度影响。整个框架旨在最大化传递给LLM优化器的上下文信噪比。

Q4: 论文做了哪些实验？

论文在三个基准上评估STRACE：HotpotQA（多跳问答）、WebArena（通用智能体任务）和VeruSAGE-Bench（形式化验证综合任务）。与标准上下文过滤基线（如直接使用原始轨迹、随机截断、滑动窗口）以及朴素反思方法进行比较。实验覆盖不同任务类型和复杂度，并进行了消融研究以分离轨迹过滤和因果定位各自的贡献。

Q5: 发现了什么实验现象？

STRACE在所有基准上一致优于基线，尤其在信噪比要求高的任务上优势明显。在VeruSAGE-Bench上，将人工专家设计智能体的成功率从42.5%提升至58.5%，绝对提升16.0%，相当于1.4倍改进。消融实验（推测）表明，轨迹过滤和因果定位均对提升有正向贡献，且两者联合效果最佳。此外，因果定位步骤有效降低了优化上下文的长度，同时保留了关键因果链，减少了优化器的处理成本。

Q6: 有什么可以进一步探索的点？

当前STRACE假设对智能体系统有充分可见性（依赖代码或任务定义的先验知识），因此一个直接方向是扩展到纯黑盒或仅能观测轨迹的场景。此外，可探索更鲁棒的因果定位方法（如基于因果推断或反事实推理）；结合主动学习选择更有价值的轨迹；以及将框架推广到多智能体协作或在线持续优化设定中。

Q7: 总结一下论文的主要内容

本文提出STRACE（Structural Trajectory Analysis and Causal Extraction），一个用于长周期智能体优化的端到端框架。动机在于：现有基于LLM反思的优化器（如Reflexion）直接处理原始执行轨迹，面临批次级冗余异质和轨迹内因果信号稀疏的双重问题，导致优化低效甚至过度拟合。STRACE通过两个创新模块解决此困境：批次级故障模式挖掘（基于统计和结构化诊断过滤冗余痕迹）和轨迹内因果定位（在文本依赖图上后向裁剪出因果紧凑的子轨迹，并定位根因模块）。优化器仅对根因模块的指令进行预防性更新，实现了安全、经济且高效的改进。实验在HotpotQA、WebArena和VeruSAGE-Bench上进行，结果显示STRACE显著优于标准上下文过滤基线，特别是在严格的形式化验证任务VeruSAGE-Bench上将成功率从42.5%提升至58.5%（绝对提升16%）。消融研究验证了两个关键组件的必要性。论文贡献包括：(1) 提出将执行日志视为结构化因果证据的优化视角；(2) 引入联合轨迹过滤与因果定位机制；(3) 实现模块级别精确的指令注入。这些贡献为长周期智能体优化提供了新的方法论。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦长周期智能体优化，与用户画像中的‘agent’方向直接契合。

## 基本信息

- 作者：Ying Chang, Jiahang Xu, Xuan Feng, Chenyuan Yang, Peng Cheng, Yuqing Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-09
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.07702`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于论文摘要、引言、系统概述及局限性部分的检索证据，并参考了启发式草稿中的框架描述与实验结果。
