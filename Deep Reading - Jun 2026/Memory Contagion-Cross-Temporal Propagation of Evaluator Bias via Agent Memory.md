---
user_id: "cheng tan"
paper_id: 1274
arxiv_id: "2606.23195"
title: "Memory Contagion: Cross-Temporal Propagation of Evaluator Bias via Agent Memory"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23195.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23195"
abs_url: "https://arxiv.org/abs/2606.23195"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:38:22"
---
# Memory Contagion: Cross-Temporal Propagation of Evaluator Bias via Agent Memory

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：memory contagion · evaluator bias · agent memory · llm agent

## 一句话总结

本文形式化并实证了Memory Contagion现象：有偏评估者的偏差通过智能体记忆系统跨时间传播，即使在完美记忆整合（oracle）条件下也会发生，且整合对不同类型偏差具有相反效应（衰减长度偏好、放大权威偏差）。

## 摘要

> Large Language Model (LLM) agents increasingly rely on memory systems to maintain long-term coherence. Recent work Zhang et al. [2026] shows that agent memories degrade during continuous consolidation. However, existing research assumes memories are derived from unbiased experiences. In this work, we identify and formalize a novel phenomenon: Memory Contagion—the cross-temporal propagation of evaluator bias through agent memory. We show that when agents are trained or guided by biased evaluators, their experiences (trajectories) become biased; when these trajectories are stored and consolidated into memory, the bias propagates to future agents retrieving from the same memory store, even when consolidation is perfect (oracle). Across two bias types (length preference, authority bias) and four experimental phases, we demonstrate: (1) Memory Contagion occurs even with perfect consolidation (oracle condition, $\Gamma_{A} = 13.18$ for length, 11.45 for authority $^{*}$ ), proving that biased input is a sufficient cause of contagion; (2) Consolidation has bias-type-dependent effects: robustly attenuating length bias ( $\Gamma_{B} = 2.03$ ) while preliminarily amplifying authority bias ( $\Gamma_{B} = 16.80$ ; \*single-run estimate), revealing a previously unknown interaction; (3) No observed safe threshold: bias propagation is detected at contamination rates as low as p = 0.2. Our findings expose a critical vulnerability in current agent memory designs and provide formal tools for measuring cross-temporal bias propagation.

Q1: 这篇论文试图解决什么问题？

现有LLM智能体记忆系统通常假设记忆源自无偏经验，但实际中智能体可能由有偏评估者（如人类或自动评价器）训练或指导，导致其轨迹（经验）含有偏差。当这些有偏轨迹被存储并整合到共享记忆库中时，可能会影响后续智能体的行为，即使这些智能体自身评估器是无偏的。这个问题尚未被系统研究。本文旨在回答：评估者偏差能否通过记忆系统跨时间传播？其传播机制是什么？记忆整合（consolidation）会缓解还是加剧这种传播？是否存在安全的污染阈值？

Q2: 有哪些相关研究？

本文相关研究主要涵盖以下几个方面：
1. **LLM智能体记忆系统**：Zhang等人[2026]研究了记忆整合过程中的退化问题，但假设经验无偏。其他工作如RAG（Retrieval-Augmented Generation）和记忆网络探讨了外部记忆对LLM的帮助，但未关注偏差传播。
2. **评估者偏差**：在RLHF（Reinforcement Learning from Human Feedback）和偏好学习中，人类评估者的偏差（如长度偏好、权威偏差）已被广泛研究，但这些工作通常关注单轮或静态偏好，未考虑通过记忆的跨时间传播。
3. **偏差放大与传播**：在AI对齐和安全性研究中，已知训练过程中的偏差可能被放大（如sycophancy、reward hacking），但大多数研究聚焦于模型参数更新中的反馈循环，而非外部记忆系统作为传播媒介。
4. **因果形式化**：本文引用Sharma等人[2024]关于谄媚行为的研究，以及Dubois等人[2023]关于模型编写的评估，作为偏差来源的背景。但本文的独特贡献在于将记忆系统视为偏差传播的通道。

Q3: 论文如何解决这个问题？

本文采用形式化定义与实验验证相结合的方法。首先，给出Memory Contagion的数学定义：令M为记忆库，E为评估器，偏差传播度量Γ衡量目标智能体A'在从M检索记忆后行为偏差的增加。通过4个实验阶段系统分解传播机制：
- **阶段1：有偏记忆存储（Biased Storage）**：让源智能体A在有偏评估器下生成轨迹，构建有偏记忆库。
- **阶段2：整合效应分析**：比较oracle整合（完美压缩）与noisy整合（退化）对传播的影响。
- **阶段3：机制分解**：区分内容传播（检索到有偏记忆）与评估器偏移（目标智能体自身的评估器被污染）。
- **阶段4：剂量响应分析**：改变污染比例p（0.2、0.4、0.6、0.8），测量Γ(p)以寻找安全阈值。
实验包含两种偏差类型：长度偏好（较短回复被偏好）和权威偏差（来自权威来源的回复被偏好）。评估指标包括Γ_A（相对于无偏基线的绝对传播）、Γ_B（整合前后的传播变化）等。

Q4: 论文做了哪些实验？

论文在两个偏差类型上进行了四个实验阶段：
1. **阶段1：有偏记忆存储**：使用有偏评估器训练源智能体A，使其产生有偏轨迹，然后存储到记忆库。测量目标智能体A'（使用无偏评估器）从该记忆库检索后的行为偏差。结果：oracle条件下长度偏差Γ_A=13.18，权威偏差Γ_A=11.45，证明即使完美整合也存在显著传播。
2. **阶段2：整合效应分析**：比较oracle整合与noisy整合（模拟记忆退化）。发现长度偏差在整合后显著减小（Γ_B=2.03，即整合衰减了偏差），而权威偏差在整合后增大（Γ_B=16.80，单次运行估计）。
3. **阶段3：机制分解**：通过分析A'实际检索到的记忆内容，发现当记忆库有偏时，A'在78%（长度）和82%（权威）的情况下检索到有偏记忆，确认内容传播是主要原因。同时测量A'自身评估器的偏移，发现权威偏差条件下评估器也发生偏移。
4. **阶段4：剂量响应分析**：改变污染比例p从0.2到0.8，测量Γ(p)。结果表明，在p=0.2时已检测到非零传播，无安全阈值。传播强度随p增加而增加，但非线性。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：
1. **Memory Contagion在oracle条件下发生**：即使记忆整合完全保真（无信息丢失），偏差仍从有偏源智能体传播到使用无偏评估器的目标智能体。这表明偏差输入本身就是传播的充分条件，无需整合缺陷。
2. **整合效应依赖偏差类型**：长度偏差在整合后被稳健衰减（Γ_B=2.03），而权威偏差在整合后反而被放大（Γ_B=16.80，但为单次运行估计，需谨慎）。这首次揭示整合对偏差类型的非单调影响，暗示不同偏差可能通过不同机制传播。
3. **内容传播占主导**：在机制分解中，目标智能体检索有偏记忆的比例很高（78%和82%），说明主要是记忆内容本身的偏差导致行为偏移，而非智能体评估器状态变化。但权威偏差下评估器也出现轻微偏移，提示可能存在双重通道。
4. **无安全阈值**：在p=0.2（仅20%记忆有偏）时已观察到显著传播，表明即使少量污染也能导致可检测的偏差传播。传播强度随污染比例增加，但并非线性。
5. **权威偏差的放大效应**：与直觉相反，记忆整合（本应去噪）却放大了权威偏差，可能因为权威偏差与记忆的“重要性”标记耦合，导致整合算法优先保留此类信息。

Q6: 有什么可以进一步探索的点？

基于本文发现，以下方向值得进一步探索：
1. **更多偏差类型**：仅测试了长度偏好和权威偏差，其他常见偏差如社会偏见、风格偏好、相关性偏好等可能展现不同传播特性。
2. **质量正交的偏差注入方法**：当前偏差注入可能耦合了任务质量（如较短回复可能质量更低），未来需设计与质量无关的偏差标记（如中性标记符）以分离因果效应。
3. **任务场景泛化**：实验基于特定生成任务，需验证在对话、推理、代码生成等不同场景下的传播模式。
4. **整合算法设计**：如何设计偏差感知的记忆整合算法，既能保持信息完整性又能抑制偏差传播？是否需要显式的偏差检测模块？
5. **多轮传播与雪崩效应**：如果记忆库被多代智能体持续污染，偏差是否加速放大？是否存在类似于“回音室”的自我强化循环？
6. **交互与动态记忆**：当智能体动态更新记忆库时，偏差传播的长期动力学如何？是否有记忆衰减或冲突解决机制可缓解？
7. **可解释性工具**：开发形式化度量（如Γ）的应用扩展，用于监控和审计实际部署的智能体系统中的偏差传播。

Q7: 总结一下论文的主要内容

本文首次提出并系统研究了LLM智能体记忆系统中的“Memory Contagion”现象——即有偏评估者的偏差通过记忆库跨时间传播到后续智能体。通过形式化定义和四阶段实验，作者证明即使在完美记忆整合（oracle）条件下，传播仍然显著发生（长度偏好Γ_A=13.18，权威偏差Γ_A=11.45）。记忆整合对不同类型偏差具有相反效应：它衰减长度偏好（Γ_B=2.03）但放大权威偏差（Γ_B=16.80，单次运行）。机制分析表明，传播主要通过内容通道实现（目标智能体检索有偏记忆的比例高达78-82%），而评估器状态偏移是次要途径。剂量响应实验发现，即使在污染率低至20%时，仍能检测到偏差传播，不存在安全阈值。这些发现揭示了当前智能体记忆系统的一个关键漏洞：记忆被设计为忠实地存储经验，却不区分经验是否被有偏评估器污染，从而成为偏差在时间维度上传播的通道。论文提供了量化传播的度量工具，为未来设计更鲁棒的智能体记忆系统奠定了基础。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：直接研究LLM智能体记忆系统的安全问题，与用户画像中的agent方向高度相关（权重0.10）。

## 基本信息

- 作者：Zewen Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23195`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 基于PDF语义检索证据撰写
