---
user_id: "cheng tan"
paper_id: 2418
arxiv_id: "2607.01084"
title: "Can Agents Generalize to the Open World? Unveiling the Fragility of Static Training in Tool Use"
institution: "南京大学 LAMDA 实验室 (LAMDA-NeSy)"
publish_date: "2026-07-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.01084.pdf"
pdf_url: "https://arxiv.org/pdf/2607.01084"
abs_url: "https://arxiv.org/abs/2607.01084"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-02T14:26:55"
---
# Can Agents Generalize to the Open World? Unveiling the Fragility of Static Training in Tool Use

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：llm agents · tool use · open-world generalization · distributional shift

## 一句话总结

本文揭示了当前大语言模型智能体在静态基准测试中的性能繁荣与其在开放世界动态环境下的脆弱性之间的巨大鸿沟，并提出了扰动增强微调（PAFT）策略以提升其鲁棒性。

## 摘要

> While Large Language Model (LLM) agents demonstrate proficiency in static benchmarks, their deployment in real-world scenarios is hindered by the dynamic nature of user queries, tool sets, and interaction dynamics. To address this generalization gap, we formalize OpenAgent (Tool-Use Agent in Open-World), a problem setting characterized by distributional shifts across query, action, observation, and domain dimensions. To systematically diagnose its impact, we construct a controlled sandbox environment where we define fine-grained environmental shifts across a four-tier hierarchy, Perception, Interaction, Reasoning, and Internalization, and conduct a comprehensive series of experiments. Our analysis yields a series of key insights, demonstrating that agents trained via both Supervised Fine-Tuning(SFT) and Reinforcement Learning suffer from varying degrees of performance degradation when confronting open environmental shifts. Building on these insights, we propose Perturbation-Augmented Fine-Tuning, a disturbance-based intervention strategy for SFT that lays the foundation for enhancing agent robustness and utility in realistic environments. Our code will be released at: https://github. com/LAMDA-NeSy/OpenAgent.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：静态训练与动态现实的脱节
当前的 LLM 智能体研究大多集中在封闭、静态的基准测试中，这些测试通常假设测试环境与训练环境分布一致。然而，在真实部署中，智能体面临着高度非平稳的环境：
1. **查询偏移（Query Shift）**：用户表达方式的多样性和模糊性。
2. **动作偏移（Action Shift）**：工具接口的更新、参数格式的微调或新工具的加入。
3. **观测偏移（Observation Shift）**：外部环境反馈的噪声、延迟或格式变化。
4. **领域偏移（Domain Shift）**：从熟悉的任务领域迁移到完全陌生的应用场景。

### 现有研究的局限性
现有的工具学习基准（如 ToolBench 等）虽然提供了大量工具，但其评估协议往往是静态的。智能体在这些基准上通过不断的 SFT 或 RL 训练，往往会陷入“轨迹过拟合”（Trajectory Overfitting）的陷阱，即它们记住了特定的调用序列，而非理解了工具背后的语义逻辑。一旦环境发生细微扰动，这些智能体就会表现出极大的脆弱性，导致任务失败。

Q2: 有哪些相关研究？

### 工具学习基准的演进
早期的工具学习研究主要关注本地工具调用（如 Wang et al., 2024a）和远程协议（如 Li et al., 2023）。虽然这些工作推动了智能体能力的发展，但它们大多在受控的、分布一致的环境中进行评估。

### 鲁棒性与泛化性研究
近期有一些工作开始关注智能体的鲁棒性，例如针对工具描述的扰动或错误处理。然而，这些研究往往缺乏一个系统性的框架来量化不同维度的环境偏移。本文通过引入 Model Context Protocol (MCP) 等现代协议视角，将工具使用提升到了一个更具动态性的“开放世界”维度，填补了系统性诊断智能体在复杂偏移下表现的空白。

Q3: 论文如何解决这个问题？

### OpenAgent 形式化定义
本文将开放世界下的智能体决策过程建模为一个受分布偏移影响的马尔可夫决策过程（MDP）。通过定义四个关键维度的偏移（查询、动作、观测、领域），为评估智能体的泛化能力提供了数学基础。

### 四层级受控沙盒环境
为了隔离不同因素的影响，研究者构建了一个分层诊断沙盒：
1. **感知层（Perception）**：测试智能体对输入提示词和工具描述变化的敏感度。
2. **交互层（Interaction）**：模拟工具调用过程中的协议变化和反馈噪声。
3. **推理层（Reasoning）**：评估智能体在多步规划中应对中间状态偏移的能力。
4. **内化层（Internalization）**：考察智能体是否真正掌握了工具的底层逻辑，而非仅仅是符号锚定。

### 扰动增强微调（PAFT）
PAFT 是一种基于干扰的干预策略。在 SFT 阶段，PAFT 不仅仅提供标准的专家轨迹，还会主动引入受控的扰动（如改写查询、模拟工具报错、修改参数顺序等），并要求智能体在这些扰动下依然能恢复到正确的决策路径。这种方法旨在打破智能体对特定符号序列的依赖，增强其语义理解能力。

Q4: 论文做了哪些实验？

### 实验设置
- **基座模型**：采用了多种主流 LLM（如 Llama-3, Qwen 系列）作为智能体骨干。
- **训练范式**：对比了标准的 SFT 和基于 PPO/DPO 的强化学习方法。
- **评估指标**：除了传统的成功率（SR），还引入了鲁棒性得分（Robustness Score）和恢复率（Recovery Rate）。

### 实验场景
实验涵盖了从简单的 API 调用到复杂的多步任务规划，利用沙盒环境生成了数千个具有不同偏移程度的测试用例。特别关注了在训练集中未见过的工具组合和领域场景。

Q5: 发现了什么实验现象？

### 关键发现与反直觉结果
1. **SFT 的脆弱性与过拟合**：SFT 训练的智能体极易产生“符号锚定”现象。即使是微小的工具名称更改，也会导致其调用失败，这表明它们更多是在进行模式匹配而非逻辑推理。
2. **RL 的语义接地能力**：相比之下，RL 训练的智能体在语义理解上表现更好，能够处理一定程度的描述偏移。然而，它们在面对“边界条件”（如极端的观测噪声）时依然表现出不稳定性。
3. **性能退化的非线性**：环境偏移对性能的影响并非线性的。在感知层和交互层的复合偏移下，智能体的成功率会出现断崖式下跌，显示出不同维度的偏移具有累积效应。
4. **PAFT 的有效性**：实验证明，经过 PAFT 处理的模型在未见过的偏移场景下，其性能保持率比标准 SFT 模型高出 20%-35%，且在标准基准测试中没有明显的性能损失。

Q6: 有什么可以进一步探索的点？

### 可探索的方向
1. **动态在线适应**：研究智能体如何在部署过程中通过少量的交互反馈，实时调整其策略以适应环境偏移。
2. **跨协议泛化**：随着 MCP 等协议的普及，如何让智能体具备在不同通信协议之间无缝切换的能力是一个重要课题。
3. **自动扰动生成**：利用 LLM 自动生成更具挑战性、更符合真实世界分布的扰动数据，以进一步强化 PAFT 的效果。
4. **长程任务中的偏移累积**：深入研究在超长步骤任务中，微小偏移如何演变成灾难性失败，并开发相应的纠错机制。

Q7: 总结一下论文的主要内容

本文针对 LLM 智能体在真实世界部署中面临的泛化性难题，开展了系统性的研究。作者首先指出，现有的静态基准测试掩盖了智能体在面对动态环境时的脆弱性。为此，论文提出了 OpenAgent 框架，将开放世界中的环境变化形式化为查询、动作、观测和领域四个维度的分布偏移。通过构建一个包含感知、交互、推理和内化四个层级的受控沙盒环境，作者对 SFT 和 RL 训练的智能体进行了深度诊断，揭示了 SFT 容易陷入轨迹过拟合而 RL 在边界条件下鲁棒性不足的问题。为了解决这些问题，论文提出了扰动增强微调（PAFT）策略，通过在训练中引入受控干扰，迫使智能体学习更本质的语义逻辑而非表面的符号关联。实验结果表明，PAFT 显著提升了智能体在开放世界场景下的适应能力和鲁棒性。这项工作为构建更可靠、可部署的智能体系统提供了重要的理论基础和实践指导。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：该研究直接关联智能体（Agent）的鲁棒性与泛化性，是当前 Agent 落地应用的关键瓶颈。

## 基本信息

- 作者：Song-Lin Lv, Weiming Wu, Rui Zhu, Zi-Jian Cheng, Lan-Zhe Guo
- 机构：南京大学 LAMDA 实验室 (LAMDA-NeSy)
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-02
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2607.01084`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是关于 OpenAgent 形式化定义、沙盒层级设计以及 PAFT 策略的详细描述。
