---
user_id: "cheng tan"
paper_id: 10464
arxiv_id: "2609.01422v1"
title: "From Rollouts to Recipes: Self-Contained Post-Training for LLMs"
publish_date: "2026-09-01"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.01422v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.01422v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:36:13"
---
# From Rollouts to Recipes: Self-Contained Post-Training for LLMs

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：large language models · post-training · self-routing · mathematical reasoning

## 一句话总结

Self-Routing 是一种行为感知的后训练框架，通过模型自身的 Rollout 正确性和置信度，为每个样本动态分配最优训练策略（如 GRPO 或自蒸馏），从而显著提升数学推理性能。

## 摘要

> Post-training large language models usually applies a single training recipe to all samples, even though the model's own rollouts reveal different sample-level learning states. We propose Self-Routing, a behavior-conditioned post-training framework that uses rollout correctness and confidence to decide how each sample should be optimized. Depending on its behavior state, a sample is routed to GRPO, on-policy self-distillation, regularization, or skipping, allowing training to adapt without external teachers, extra annotations, or additional sampling. Experiments on mathematical reasoning across Qwen3 and Qwen3.5 backbones show that Self-Routing consistently improves over uniform GRPO, uniform OPSD, fixed mixtures, and simpler routing baselines. Further analyses show that the routing distribution changes over training and reduces unnecessary updates on low-signal or already stable samples.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决 LLM 后训练（Post-training）中的“全局配方（Global Recipes）”局限性问题。目前的后训练方法（如 SFT 或 RLHF）通常对整个数据集应用单一的优化目标，这种“一刀切”的做法忽略了模型在不同样本上的实际掌握程度。具体问题包括：
1. **学习状态的异质性**：模型在某些样本上可能已经非常稳定（高置信度且正确），在某些样本上处于学习边缘（部分正确），而在另一些样本上则完全迷茫（低置信度且错误）。
2. **无效更新与噪声干扰**：对已掌握的样本进行过度训练可能导致过拟合或能力退化；对模型完全无法理解的噪声样本进行训练，则会引入错误的梯度信号。
3. **外部依赖成本**：现有的自我改进方法往往依赖于更强大的外部教师模型（如 GPT-4）提供反馈，或者需要昂贵的人工标注来纠正 Rollout 轨迹。
4. **训练效率低下**：统一的优化目标无法根据样本的学习进度动态调整计算资源的分配，导致在低价值样本上浪费计算力。

Q2: 有哪些相关研究？

论文涉及的相关研究领域主要包括：
1. **强化学习后训练**：如 PPO 和 GRPO（Group Relative Policy Optimization），这些方法通过奖励信号优化模型，但通常对所有数据应用相同的 RL 目标。
2. **自蒸馏与自我改进（Self-Improvement）**：如 STaR、ReST 和在线自蒸馏（OPSD），利用模型生成的正确轨迹作为伪标签进行微调。本文将其作为路由的可选路径之一。
3. **课程学习（Curriculum Learning）**：根据样本难度调整训练顺序。Self-Routing 可以看作是一种动态的、基于模型实时表现的课程学习形式。
4. **混合专家模型（MoE）与路由机制**：虽然 MoE 在推理时进行路由，但本文借鉴了路由的思想，将其应用于训练阶段的优化目标选择。
5. **拒绝采样与过滤**：许多方法通过过滤掉错误样本来提升质量，而 Self-Routing 则更进一步，对不同状态的样本采取不同的积极策略，而非简单的丢弃。

Q3: 论文如何解决这个问题？

论文提出了 Self-Routing 框架，其核心在于“因材施教”的样本级优化：
1. **行为感知指标**：利用模型在训练过程中的 Rollout 表现，提取两个关键指标：正确性（Correctness，基于 Ground Truth 判定）和置信度（Confidence，基于模型输出的概率分布或一致性）。
2. **四种训练配方（Recipes）**：
 - **GRPO**：当模型表现出一定的不确定性但能生成部分正确答案时，通过组相对策略优化来强化正确逻辑。
 - **在线自蒸馏 (OPSD)**：当模型以高置信度生成正确答案时，将其作为高质量 SFT 信号进行监督学习，以稳定该能力。
 - **正则化 (Regularization)**：针对特定中间状态，仅保持策略的稳定性，防止偏离。
 - **跳过 (Skipping)**：对于已经完全掌握（无需再学）或完全错误且无改进信号（学不会）的样本，不进行参数更新，节省计算资源。
3. **动态路由逻辑**：在每个训练步，模型先对样本进行 Rollout，根据实时表现决定该样本进入哪个 Recipe。这意味着同一个样本在训练初期可能被路由到自蒸馏，而在后期可能被跳过。
4. **自包含性**：整个过程完全依赖模型自身的 Rollout，不需要外部模型干预，实现了闭环的自我进化。

Q4: 论文做了哪些实验？

论文设计了详尽的实验来验证 Self-Routing 的有效性：
1. **基座模型**：涵盖了 Qwen3-1.7B、Qwen3-4B 以及 Qwen3.5-0.8B、Qwen3.5-2B、Qwen3.5-4B 等多个代际和规模的模型，验证了方法的普适性。
2. **任务领域**：专注于数学推理（Mathematical Reasoning），这是评估 LLM 逻辑能力的核心场景。
3. **对比基线**：
 - **Base Model**：未经后训练的原始模型。
 - **Uniform GRPO**：对所有样本统一应用 GRPO 优化。
 - **Uniform OPSD**：对所有样本统一应用在线自蒸馏。
 - **Fixed Mixtures**：按固定比例混合不同优化目标的策略。
 - **Simple Routing**：基于简单规则（如仅看正确性）的路由基线。
4. **消融实验**：分析了正确性和置信度两个维度对路由决策的贡献，以及不同 Recipe 组合的影响。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象和趋势：
1. **性能一致性提升**：Self-Routing 在所有测试的 Qwen 模型上均显著优于 Uniform GRPO 和 Uniform OPSD。例如，在 Qwen3.5-4B 上，其准确率提升幅度明显超过了单一策略。
2. **路由分布的动态演进**：在训练初期，模型倾向于将更多样本路由到自蒸馏（OPSD），以快速建立正确的推理模式；随着训练进行，GRPO 的比例逐渐上升，用于精细化逻辑；在训练后期，大量样本被路由到“跳过”状态，表明模型已达到饱和或稳定。
3. **减少无效梯度**：分析发现，Self-Routing 成功识别并跳过了那些可能导致模型性能下降的“噪声 Rollout”，这解释了为什么它比 Uniform 策略更稳健。
4. **指标间的张力**：实验观察到，单纯追求正确性而不考虑置信度会导致模型在某些样本上过度拟合错误的推理路径，而 Self-Routing 通过置信度过滤缓解了这一问题。
5. **Scaling Trend**：随着模型规模从 0.8B 增加到 4B，Self-Routing 带来的增益依然存在，甚至在更大模型上表现出更强的逻辑一致性。

Q6: 有什么可以进一步探索的点？

尽管取得了显著成果，论文也指出了未来的探索方向：
1. **跨领域泛化**：验证 Self-Routing 在非结构化任务（如智能体规划 Agent Planning、创意写作或开放式对话）中的表现，这些领域的“正确性”判定更为复杂。
2. **路由维度的扩展**：除了正确性和置信度，是否可以引入推理链的逻辑一致性（Consistency）或步骤级（Step-wise）的反馈作为路由依据？
3. **采样效率优化**：虽然减少了参数更新次数，但 Rollout 采样本身仍占用大量计算资源。如何结合推测采样（Speculative Sampling）等技术降低采样开销是关键。
4. **多目标路由**：探索在更复杂的任务中，如何将样本路由到更多样化的优化目标（如对比学习、负采样强化等）。
5. **理论边界**：进一步研究路由阈值设定与模型收敛速度之间的理论关系。

Q7: 总结一下论文的主要内容

本文提出了 Self-Routing，这是一种创新的 LLM 后训练框架，旨在打破传统方法中“统一优化目标”的局限。该框架的核心逻辑是“行为感知”：它不预设固定的训练配方，而是根据模型在训练过程中对每个样本的实时 Rollout 表现（正确性与置信度）来动态决定优化策略。具体而言，样本会被智能地分配到强化学习（GRPO）、监督学习（自蒸馏）或直接跳过。在 Qwen3 和 Qwen3.5 系列模型上的数学推理实验证明，Self-Routing 在最终准确率和训练稳定性上均优于现有的统一策略基线。研究深入分析了训练过程中的路由动力学，发现这种动态分配机制能够有效过滤噪声信号并减少对已掌握知识的重复训练，从而实现更高效的自我进化。该方法无需外部教师模型或额外标注，为构建自包含、自适应的 LLM 后训练系统提供了有力的技术路径。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于研究 LLM 自我进化（Self-evolution）和强化学习后训练的科研人员具有重要参考价值。

## 基本信息

- 作者：Yifei Li, Lingling Zhang, Muye Huang, Zihan Ma, Jiashuai Liu, Jun Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-09-01
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.01422v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点提取了 Abstract、Conclusion 和 Experiments 部分的核心结论，并结合了 heuristic_draft 中的框架信息。
