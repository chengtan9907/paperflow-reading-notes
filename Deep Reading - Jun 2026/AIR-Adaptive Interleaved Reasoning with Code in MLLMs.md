---
user_id: "cheng tan"
paper_id: 1238
arxiv_id: "2606.23678"
title: "AIR: Adaptive Interleaved Reasoning with Code in MLLMs"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23678.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23678"
abs_url: "https://arxiv.org/abs/2606.23678"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:15:11"
---
# AIR: Adaptive Interleaved Reasoning with Code in MLLMs

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal large language model · interleaved reasoning · reinforcement learning · code generation

## 一句话总结

本文提出AIR框架，通过强化学习训练赋予多模态大语言模型自适应交错推理能力，以代码增强的方式解决复杂数值计算问题。

## 摘要

> Following the paradigm shift initiated by OpenAI o3, interleaved reasoning with code to enhance multimodal large language models (MLLMs) has become a pivotal research frontier. The existing literature focuses primarily on tool-use within vision-perception tasks. However, such approaches typically rely on predefined heuristics for visual manipulation and are inherently incapable of addressing numerical computation problems due to their exclusive focus on visual operations. This paper empowers MLLMs with adaptive interleaved reasoning capabilities through extended reinforcement learning training on code-augmented complex numerical computation tasks. To this end, we propose a comprehensive three-component solution consisting of: a two-stage cold-start data construction pipeline, data filtering strategies for RL dataset curation, and an adaptive tool-invocation strategy leveraging a group-constrained reward function for interleaved reasoning trajectories. Extensive experiments demonstrate that after Reinforcement Learning training with the group-constrained reward function, performance improves by an average of 6.1 percentage points (pp) on evaluation benchmarks. Specifically, the accuracy for interleaved reasoning samples increases by 9.9 pp, and the overall success rate of tool-use exceeds 95%. Our data and code are available at: https://github.com/CongHan0808/AIR.git.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决多模态大语言模型在复杂数值计算任务中缺乏自主交错推理能力的问题。现有工作只关注视觉感知中的工具使用，无法处理需要数值计算的问题。

Q2: 有哪些相关研究？

相关研究包括OpenAI o3/o4-mini展示的交错推理范式，DeepSeek-R1和Kimi-K1.5等应用可验证奖励强化学习的工作。现有文献主要聚焦于视觉感知任务中的工具使用。

Q3: 论文如何解决这个问题？

论文提出AIR框架，包含三部分：1）两阶段冷启动数据构建流程，生成初始推理数据；2）强化学习数据集的数据过滤策略；3）自适应工具调用策略，使用组约束奖励函数引导交错推理轨迹。训练分为SFT冷启动和RL增强两个阶段。

Q4: 论文做了哪些实验？

论文进行了两阶段训练：先使用冷启动数据进行SFT，再在RL阶段使用组约束奖励函数优化。在多个评估基准上进行了测试，测量了平均性能、交错推理准确率和工具使用成功率。

Q5: 发现了什么实验现象？

实验观察到：组约束奖励函数显著提升性能，平均提升6.1pp；交错推理样本准确率提升9.9pp；工具使用成功率超过95%。案例研究显示，模型能正确调用代码进行复杂数据计算，但也存在失败案例（失败模式未详细列出）。

Q6: 有什么可以进一步探索的点？

论文未明确讨论未来方向，但可探索的点包括：扩展到更长的推理链条、集成更多工具类型、应用于科学计算领域、提升失败案例的鲁棒性、探索更高效的数据构建方法。

Q7: 总结一下论文的主要内容

本文提出AIR（Adaptive Interleaved Reasoning）框架，旨在增强多模态大语言模型的自主交错推理能力，特别是针对复杂数值计算任务。受OpenAI o3范式启发，现有工作仅关注视觉感知中的工具使用，无法处理数值计算。AIR通过强化学习训练实现自适应代码调用。方法包含三个组件：冷启动数据构建管线（两阶段生成初始推理数据）、RL数据集的过滤策略、以及利用组约束奖励函数的自适应工具调用策略。训练采用两阶段范式：先SFT冷启动，再RL强化。在多个基准上评估，组约束奖励函数使性能平均提升6.1pp，交错推理样本准确率提升9.9pp，工具使用成功率超95%。案例分析展示了成功和失败实例，表明模型在复杂计算中能有效调用代码，但仍存在挑战。论文贡献包括：提出AIR框架、实现自适应交错推理、设计组约束奖励、提供数据构建和过滤方法。限制在于：失败案例未深入分析，无跨领域验证。这项工作为MLLM的数值计算推理提供了系统方案。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本文聚焦的数值计算推理对智能体（agent）的数学和逻辑能力提升有借鉴意义

## 基本信息

- 作者：Cong Han, Xiaohan Lan, Haibo Qiu, Yujie Zhong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23678`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（abstract、method、results片段），并基于heuristic_draft和字段证据映射进行润色和补全。
