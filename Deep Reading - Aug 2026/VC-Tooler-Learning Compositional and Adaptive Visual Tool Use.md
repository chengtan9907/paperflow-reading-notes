---
user_id: "cheng tan"
paper_id: 6199
arxiv_id: "2608.02217v1"
title: "VC-Tooler: Learning Compositional and Adaptive Visual Tool Use"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02217v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02217v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:50:47"
---
# VC-Tooler: Learning Compositional and Adaptive Visual Tool Use

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual tool use · agentic multimodal reasoning · vision-language model · reinforcement learning

## 一句话总结

VC-Tooler 通过分层轨迹合成与两阶段训练（监督冷启动 + 强化学习），将视觉工具使用学习为组合式且自适应的能力，在通用与智能体基准上达到开源模型最优。

## 摘要

> Agentic multimodal reasoning extends passive image understanding by allowing VLMs to actively acquire and refine visual evidence through visual tool interactions. Effective visual tool use requires three capabilities: grounding tool calls in visual context, composing tools across multiple steps, and adapting reasoning to tool-returned observations. However, existing approaches largely focus on grounding within fixed tool spaces and rigid invocation patterns, leaving composition and adaptation insufficiently addressed. We present VC-Tooler, which learns visual tool use as a compositional and adaptive capability. To this end, we first build a trajectory bank through a hierarchical synthesis pipeline covering three capability levels: single-tool grounding, multi-tool composition, and diverse tool contexts and interfaces. We then train the model in two stages: a supervised cold start that establishes these capabilities, followed by reinforcement learning that encourages accurate, efficient, and context-aware visual tool use. VC-Tooler achieves state-of-the-art performance among open-source models on both general-purpose and agentic benchmarks, including $95.8\%$ on V* and $35.3\%$ on VTC-Bench, and shows promising transfer under richer tool settings at inference time. Project page: https://w1zheng.github.io/VC-Tooler

Q1: 这篇论文试图解决什么问题？

论文试图解决的问题是：现有视觉语言模型（VLM）的智能体式多模态推理中，视觉工具使用（visual tool use）能力不足，尤其是组合与适应方面。有效的视觉工具使用需要三种能力：(1) 在视觉上下文中 grounding 工具调用，即知道何时、对图像哪个区域调用什么工具；(2) 跨多个步骤组合工具，即能够编排多个工具的调用序列来完成复杂任务；(3) 根据工具返回的观察结果自适应地调整推理和后续行动。然而，现有方法大多在固定工具空间和僵化调用模式下做 grounding，导致模型只能记忆固定的调用范式，而无法泛化到新的工具组合和新的接口形式。作者进一步指出，当监督信号仅覆盖少数工具时，模型容易记住固定调用模式，而不是习得可迁移的工具使用技能。因此，核心问题是如何让模型在开放、多样、组合性的工具使用场景中具备通用且可迁移的能力。

Q2: 有哪些相关研究？

相关研究主要包括几个脉络：(1) VLM 从被动图像理解向主动智能体推理的演进，即模型主动使用外部工具支持多步问题求解；(2) 视觉工具的具体形式，如 zoom（缩放）、crop（裁剪）、OCR 等，用于获取或细化视觉证据；(3) 视觉 grounding 研究，关注将语言指令与图像区域对齐；(4) 智能体式基准和训练方法，如 V*、HRBench、CharXiv、MME-RealWorld 等通用基准，以及 VTC-Bench 这样的智能体工具使用基准。由于本文的证据片段有限，具体参考文献细节不够完整，但可以推断作者讨论了固定工具空间方法的局限，并强调需要更灵活的组合与适应机制。

Q3: 论文如何解决这个问题？

VC-Tooler 的解决方案分为两个核心部分。第一部分是构建轨迹库（trajectory bank），采用分层合成（hierarchical synthesis）流水线，覆盖三个能力级别：单工具 grounding（single-tool grounding）、多工具组合（multi-tool composition）、多样工具上下文与接口（diverse tool contexts and interfaces）。这个流水线旨在生成一个大规模语料，包含数百种视觉工具接口和上下文变体，从而鼓励模型超越固定模式（schemas）和调用形式进行泛化。第二部分是两阶段训练：第一阶段是监督冷启动（supervised cold start），在轨迹库上进行监督学习，建立工具使用的 grounding、组合和适应能力；第二阶段是强化学习（reinforcement learning），通过奖励信号鼓励模型产生准确、高效、上下文感知的工具调用。整体上，该方法将工具使用视为一种可学习的组合式能力，而非对固定调用模式的记忆。

Q4: 论文做了哪些实验？

论文在两类基准上评估 VC-Tooler：通用基准和智能体基准。通用基准包括 V* (Wu and Xie 2024)、HRBench-4K/8K (Wang et al. 2025d)、CharXiv (Wang et al. 2024) 和 MME-RealWorld (Zhang et al. 2025a)，用于测量广泛的视觉能力。智能体基准包括 VTC-Bench，用于测量工具使用和智能体行为。从检索到的结果片段看，VC-Tooler-RL 在 V* 上达到 95.8，在另一项（可能是 HRBench 或其它）上达到 83.8，在 VTC-Bench 上达到 35.3。由于片段不完整，具体各基准的完整结果和对比 baseline 需要查阅原文。此外，论文还验证了在推理时向更丰富工具设置迁移的能力。

Q5: 发现了什么实验现象？

根据现有证据，VC-Tooler 在多个基准上取得强性能，特别是在 V* 上达到 95.8%，在 VTC-Bench 上达到 35.3%，表明组合式工具使用训练能够提升模型在更困难、需要多步工具调用的任务上的表现。检索片段提到“当监督信号局限于少量工具时，模型倾向于记忆固定调用模式而非获得可迁移的技能”，这暗示消融或对比实验可能显示了训练数据多样性对泛化的重要性。另外，RL 阶段相对 SFT 冷启动有进一步收益（因为最佳结果标注为 VC-Tooler-RL）。这些观察是合理的推断，具体消融趋势、失败案例和指标间张力需查阅原文实验部分。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：(1) 扩展到更多样化和更复杂的工具集，如可执行代码、网页操作、物理机器人工具等；(2) 研究更高效的轨迹合成方法，减少人工标注或合成成本；(3) 探索在线适应或持续学习，让模型在使用中持续改进工具使用策略；(4) 将 VC-Tooler 的方法迁移到其他模态（如音频、视频）或跨模态工具使用；(5) 深入分析 RL 奖励设计对工具调用效率与准确率之间的权衡；(6) 在更真实的智能体任务（如网页导航、GUI 操作）上进行端到端评估；(7) 研究模型对工具返回错误的鲁棒性，以及如何从失败中恢复。

Q7: 总结一下论文的主要内容

VC-Tooler 针对 VLM 视觉工具使用中组合与适应能力不足的问题，提出了一种将工具使用学习为组合式自适应能力的完整方案。论文首先指出有效工具使用需要三种能力：工具调用的视觉 grounding、多步工具组合、以及对工具返回观察的自适应调整。现有方法局限于固定工具空间和僵化调用模式，容易造成对固定范式的记忆而非通用技能。为此，作者设计了分层合成流水线，构建覆盖单工具 grounding、多工具组合、多样工具上下文与接口的轨迹库，规模达到数百种工具接口，并以此进行监督冷启动训练，随后引入强化学习以鼓励准确、高效、上下文感知的工具调用。实验在通用基准（V*、HRBench-4K/8K、CharXiv、MME-RealWorld）和智能体基准（VTC-Bench）上评估，VC-Tooler-RL 达到开源模型最优性能，例如 V* 95.8%、VTC-Bench 35.3%，并展示了向更丰富工具设置迁移的潜力。这项工作为 VLM 从被动感知走向主动智能体推理提供了系统性的训练方法和数据策略，其轨迹合成与 RL 训练框架具有可推广性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）直接相关，讨论智能体式 VLM 的工具使用能力。

## 基本信息

- 作者：Yizheng Wu, Jiashen Hua, Bing Deng, Jieping Ye
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02217v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的证据片段，包括摘要、引言、结论和实验结果片段；部分细节基于片段合理推断，具体数值和消融需查阅原文确认。
