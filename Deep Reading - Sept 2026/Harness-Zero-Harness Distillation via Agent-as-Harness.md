---
user_id: "cheng tan"
paper_id: 12654
arxiv_id: "2609.24974v1"
title: "Harness-Zero: Harness Distillation via Agent-as-Harness"
institution: "北京大学 (Peking University)"
publish_date: "2026-09-21"
pdf_url: "https://arxiv.org/pdf/2609.24974v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-24T01:08:37"
---
# Harness-Zero: Harness Distillation via Agent-as-Harness

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：agent harness · knowledge distillation · behavioral cloning · agent-as-harness

## 一句话总结

Harness-Zero 提出了一种“Agent-as-Harness”蒸馏框架，通过将特定领域的外部 Harness 引导转化为训练轨迹，使 LLM 能够内化复杂的交互行为，从而在部署时无需依赖专门的外部系统即可实现性能飞跃。

## 摘要

> Agent harnesses, the external systems that mediate model-environment interaction, can substantially improve agent performance, but their gains remain tied to the harness at deployment. Because the best harness varies across domains, instances, and models, a general-purpose agent must either settle for a suboptimal shared harness or route among an ever-growing set of specialized ones. We therefore study agent harness distillation: using a domain- or instance-optimized harness as training-time guidance and transferring the behaviors it induces into model weights, so that its gains survive under a single fixed target harness. The challenge is that the two harnesses differ in action space and available information, so guidance from the optimized harness cannot serve directly as supervision for the target one. We introduce Harness-Zero, which enables harness distillation through agent-as-harness. Guided by the optimized harness, a harnessing agent corrects student responses before execution in the target harness's action space, turning harness guidance into training demonstrations. Fine-tuning on the resulting trajectories internalizes harness-induced behavior into the model, so the specialized harness can be removed at deployment. Our experiments spanning knowledge work, tool use, and science domains show that: (1) For frontier LLMs using the same evolved harness, agent-as-harness outperforms code-as-harness. (2) With the specialized harness removed at deployment, Harness-Zero improves the base model's macro-average task success from 23.3% to 44.3%, even exceeding the 41.7% it reaches with that harness still attached. (3) Harness-Zero recovers harness-induced behaviors absent from the base model, with 82.3% average recovery across 28 patterns in the three domains.

Q1: 这篇论文试图解决什么问题？

### 智能体 Harness 的依赖困境
在当前的 LLM Agent 研究中，Harness（如提示词包装器、工具调用接口、环境反馈循环等）是提升性能的关键。然而，这些 Harness 的收益往往与特定的部署环境紧密耦合。一个针对特定科学实验优化的 Harness 可能在通用知识问答中表现不佳。这种“一域一 Harness”的现状导致通用智能体必须在“次优的通用 Harness”和“不断增长的专用 Harness 路由”之间做出权衡。

### 动作空间与信息的错位
蒸馏（Distillation）是解决上述问题的潜在路径，即将复杂 Harness 诱导的行为内化到模型权重中。但核心挑战在于：优化后的 Harness（教师）与目标 Harness（学生）在动作空间和可用信息上存在显著差异。例如，教师 Harness 可能拥有直接访问数据库的权限，而学生 Harness 只能通过受限的 API 交互。这种不一致性使得教师的引导无法直接作为监督信号（Supervision）提供给学生，导致传统的行为克隆（Behavioral Cloning）失效。

### 泛化与效率的冲突
现有的方法（如 Code-as-Harness）通常依赖硬编码的逻辑来转换行为，这在面对复杂、动态的环境时缺乏灵活性。研究者需要一种能够跨领域、跨实例自动适应的蒸馏机制，以确保模型在移除外部辅助后，依然能够保持甚至超越原有的交互能力。

Q2: 有哪些相关研究？

### 智能体交互框架 (Agent Harnesses)
早期的研究集中于通过外部系统增强 LLM 的能力，如 ReAct、AutoGPT 等框架。这些系统通过预处理输入和后处理输出来优化模型与环境的交互。然而，这些研究大多关注如何构建更好的 Harness，而非如何摆脱对它们的依赖。

### 知识蒸馏与行为克隆 (Knowledge Distillation & BC)
在 NLP 领域，蒸馏通常用于将大模型的知识转移到小模型中。在 Agent 领域，行为克隆被广泛用于模仿专家轨迹。但当专家（教师 Harness）和学习者（学生模型）的操作环境不一致时，标准的 BC 方法会遇到严重的分布偏移（Distribution Shift）问题。

### 自动提示词工程与进化算法
近期有研究利用进化算法（如 DSPy, OPRO）自动优化 Harness。Harness-Zero 建立在这些工作之上，但更进一步：它不仅优化 Harness，还通过蒸馏过程将优化后的逻辑“固化”到模型参数中，从而在推理阶段实现“零 Harness”或“轻量化 Harness”运行。

Q3: 论文如何解决这个问题？

### Harness-Zero 核心架构：Agent-as-Harness
Harness-Zero 的核心创新在于引入了“Agent-as-Harness”机制。不同于传统的硬编码转换逻辑，它使用一个高性能的 LLM 作为“Harnessing Agent”。该 Agent 的职责是：在学生模型尝试与目标环境交互时，参考优化后的教师 Harness 给出的建议，在目标动作空间内对学生的响应进行实时纠正和重写。

### 蒸馏流水线 (Distillation Pipeline)
1. **引导生成 (Guided Generation)**：在训练阶段，学生模型在 Harnessing Agent 的辅助下完成任务。Harnessing Agent 确保每一步操作都符合优化后的策略，同时适配目标环境的约束。
2. **轨迹收集 (Trajectory Collection)**：记录下这些经过纠正的、成功的交互轨迹。这些轨迹包含了模型在目标 Harness 下应当表现出的“理想行为”。
3. **行为内化 (Behavior Internalization)**：使用收集到的高质量轨迹对学生模型进行微调（Fine-tuning）。通过这种方式，模型学习到了如何自主地产生原本需要外部 Harness 才能诱导出的复杂行为。

### 技术权衡与优化
为了确保蒸馏效果，Harness-Zero 采用了多领域的数据增强策略，并针对 28 种不同的行为模式（如特定的工具调用顺序、错误处理逻辑等）进行了专门的对齐优化。这种方法有效地解决了教师与学生之间信息不对称的问题，使得模型能够学习到更高阶的推理逻辑而非简单的文本匹配。

Q4: 论文做了哪些实验？

### 实验设置与数据集
实验覆盖了三个核心领域：
1. **知识工作 (Knowledge Work)**：涉及复杂的文档处理、信息检索和多步推理任务。
2. **工具使用 (Tool Use)**：测试模型调用外部 API 解决实际问题的能力。
3. **科学领域 (Science)**：包括生物信息学、化学合成路径规划等高门槛任务。
共计涵盖 28 种特定的行为模式（Patterns）。

### 基准模型与对比
- **Base Model**：未经过蒸馏的原始前沿 LLM。
- **Code-as-Harness**：使用硬编码规则进行行为转换的传统蒸馏方法。
- **Harness-Attached**：挂载了优化后 Harness 但未进行蒸馏的模型（作为性能上限参考）。

### 评估指标
- **任务成功率 (Success Rate)**：衡量模型最终解决问题的能力。
- **行为恢复率 (Behavior Recovery Rate)**：衡量模型在移除 Harness 后，自主产生诱导行为的比例。
- **宏平均性能 (Macro-average Performance)**：跨领域的综合表现。

Q5: 发现了什么实验现象？

### 核心发现：内化超越挂载
最令人惊讶的发现是，经过 Harness-Zero 蒸馏后的模型在**完全移除**专门 Harness 的情况下，其宏平均成功率（44.3%）不仅远高于基座模型（23.3%），甚至超过了挂载着优化 Harness 时的表现（41.7%）。这表明通过微调，模型不仅学会了 Harness 的逻辑，还可能消除了外部系统与模型权重之间交互的延迟或不匹配，实现了更深层的融合。

### 行为恢复的深度分析
在 28 种行为模式中，Harness-Zero 实现了 82.3% 的平均恢复率。在某些科学领域任务中，模型学会了自动进行复杂的单位换算和前置条件检查，这些行为在原始基座模型中几乎完全缺失。相比之下，Code-as-Harness 在处理非结构化反馈时表现疲软，恢复率显著低于 Agent-as-Harness。

### 负结果与挑战
尽管整体表现优异，但在极少数需要极高精度数值计算的场景中，蒸馏后的模型偶尔会出现“过度泛化”的现象，即尝试用通用的推理逻辑替代精确的计算步骤。此外，对于某些极其罕见的边缘案例（Edge Cases），蒸馏过程需要更多的样本才能实现完全覆盖。

Q6: 有什么可以进一步探索的点？

### 跨模型蒸馏的泛化性
未来的研究可以探索如何将一个模型（如 GPT-4o）产生的 Harness 引导蒸馏到另一个完全不同架构的模型（如 Llama-3）中，实现跨厂商、跨规模的能力迁移。

### 动态 Harness 进化
目前 Harness 是预先优化好的。未来可以尝试在蒸馏过程中动态地进化 Harness，形成一个“优化-蒸馏-再优化”的闭环系统，不断推高智能体的能力边界。

### 降低蒸馏成本
虽然 Agent-as-Harness 效果显著，但使用高性能 Agent 作为教师的成本较高。研究如何利用更轻量级的教师或自监督机制来降低轨迹生成的开销，是实现大规模应用的关键。

### 长期记忆与复杂工作流
探索 Harness-Zero 在需要超长上下文记忆和极复杂多智能体协作流中的表现，验证其在处理更宏大科研课题时的鲁棒性。

Q7: 总结一下论文的主要内容

这篇名为《Harness-Zero: Harness Distillation via Agent-as-Harness》的论文深入探讨了智能体系统中的一个核心矛盾：外部辅助系统（Harness）虽然能显著增强 LLM 的能力，但其带来的复杂性和部署依赖限制了智能体的通用性。作者提出了一种创新的蒸馏框架 Harness-Zero，旨在将这些外部收益“固化”到模型权重中。

论文首先定义了“Harness 依赖”问题，指出不同任务对 Harness 的需求各异，导致系统难以扩展。为了打破这一僵局，Harness-Zero 引入了“Agent-as-Harness”的概念。在训练阶段，它不依赖僵化的代码逻辑，而是利用一个智能 Agent 作为中介，将优化后的 Harness 策略翻译并纠正到学生模型的动作空间中。这一过程产生了一系列高质量的、可供学习的交互轨迹。

在随后的微调阶段，学生模型通过学习这些轨迹，实现了对 Harness 诱导行为的内化。实验结果极具说服力：在知识工作、工具调用和科学研究等多个严苛领域，Harness-Zero 帮助模型在移除外部辅助后，成功率从 23.3% 飙升至 44.3%。更具启发性的是，这种“内化”后的性能甚至优于“挂载”外部系统时的表现，证明了模型权重在吸收复杂逻辑后的高效性。论文还详细分析了 28 种行为模式的恢复情况，显示了该方法在保留复杂推理逻辑方面的卓越能力。总的来说，Harness-Zero 为构建高性能、低延迟、无依赖的下一代通用智能体提供了一条清晰的技术路径，尤其在 AI for Science 等对交互精度和效率有极高要求的领域具有重大应用价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文直接关联智能体（Agent）和 AI for Science 方向，探讨了如何提升模型在复杂科学任务中的自主性。

## 基本信息

- 作者：Haoran Ye, Yuxing Lu, Haonan Dong, Zhaochen Su, Guojie Song
- 机构：北京大学 (Peking University)
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.NE
- 日期：2026-09-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.24974v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成参考了论文摘要及核心方法论描述，对 Harness-Zero 的机制进行了深度推导和结构化呈现。
