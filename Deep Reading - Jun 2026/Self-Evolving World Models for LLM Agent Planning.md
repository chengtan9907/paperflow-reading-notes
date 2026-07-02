---
user_id: "cheng tan"
paper_id: 2035
arxiv_id: "2606.30639"
title: "Self-Evolving World Models for LLM Agent Planning"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.30639.pdf"
pdf_url: "https://arxiv.org/pdf/2606.30639"
abs_url: "https://arxiv.org/abs/2606.30639"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:39:50"
---
# Self-Evolving World Models for LLM Agent Planning

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world model · LLM agent · memory · planning

## 一句话总结

提出 WorldEvolver，一种无需训练的自演化世界模型框架，通过测试时记忆修订增强 LLM agent 的预测准确性和规划成功率。

## 摘要

> World models offer a principled way to equip long-horizon LLM agents with foresight: predictions of action consequences before execution. However, unreliable foresight can be ignored, misused, or even degrade downstream decision-making. In this paper, we introduce WorldEvolver, a self-evolving world model framework that revises its deployment-time context while keeping the downstream agent and all model parameters frozen. WorldEvolver integrates three modules: (i) Episodic Memory, which exploits real action transitions through retrieval-based simulation; (ii) Semantic Memory, which extracts persistent heuristic rules from prediction-observation mismatches; and (iii) Selective Foresight, which filters low-confidence predictions before integrating them into agent reasoning context. We evaluate WorldEvolver on ALFWorld and ScienceWorld, measuring world model prediction accuracy on Word2World and downstream agent success rate on AgentBoard. Extensive experiments show that WorldEvolver achieves the highest prediction accuracy across three backbones and leads other world model baselines on downstream agent success rate, demonstrating that test-time memory revision enhances both predictive fidelity and planning performance.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决长程 LLM agent 规划中世界模型预测不可靠的问题。世界模型通过预测行动后果（预见）来辅助决策，但不可靠的预测可能被忽略、误用甚至恶化决策。现有方法通常需要在线更新模型参数或修改 agent 行为，成本高且不稳定。作者希望设计一种无需训练、保持 agent 和模型参数冻结的机制，通过测试时记忆修订提升预测保真度和规划性能。

Q2: 有哪些相关研究？

相关工作包括：1) 世界模型在 LLM agent 中的应用，延续了基于模型的强化学习（如 Ha & Schmidhuber, 2018; Hafner et al., 2025）的思路，现有系统使用 LLM 同时作为世界模型和 agent。2) 记忆增强的 agent，利用外部记忆存储经验或规则。3) 选择性预见，即对预测进行置信度过滤。本文结合这三者并提出自演化框架。

Q3: 论文如何解决这个问题？

WorldEvolver 包含三个模块：1) 情景记忆（Episodic Memory）：通过基于检索的模拟利用真实行动转移，存储和重复使用过去经验。2) 语义记忆（Semantic Memory）：从预测与观察的不匹配中提取持久启发式规则，作为提示级证据。3) 选择性预见（Selective Foresight）：在将预测集成到 agent 推理上下文之前过滤低置信度预测。整个框架无需训练，仅修订世界模型上下文。

Q4: 论文做了哪些实验？

在 ALFWorld 和 ScienceWorld 两个长程文本环境上评估。使用 Word2World 基准衡量世界模型预测准确性，使用 AgentBoard 衡量下游 agent 成功率。使用了三个不同的 backbone LLM。与多种世界模型基线对比。

Q5: 发现了什么实验现象？

WorldEvolver 在三个 backbone 上均达到最高预测准确性，并在下游 agent 成功率上领先基线。表明测试时记忆修订同时提升了预测保真度和规划性能。具体消融和负结果未在摘要中提供，但推测情景记忆和语义记忆各自贡献显著。

Q6: 有什么可以进一步探索的点？

1) 扩展到更多样化和真实环境（当前限于文本环境）。2) 更鲁棒的置信度估计和自适应过滤机制。3) 探索与其他 agent 框架的结合。4) 考虑稀疏步骤级反馈的更有效集成。

Q7: 总结一下论文的主要内容

本文提出 WorldEvolver，一种针对 LLM agent 规划的自演化世界模型框架。核心思想是在不更新模型参数或修改 agent 的情况下，通过测试时修订世界模型上下文来提升预测质量。框架包含三个互补模块：情景记忆（检索和重用真实转移）、语义记忆（从错误中提取规则）和选择性预见（过滤低置信度预测）。在 ALFWorld 和 ScienceWorld 上的实验表明，WorldEvolver 在预测准确性和下游 agent 成功率上均优于基线。主要贡献是提出了一个训练免费、模块化的记忆中心框架，并能处理稀疏反馈。局限在于仅测试了文本环境，且置信度估计简单。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本文与智能体方向直接相关

## 基本信息

- 作者：Xuan Zhang, Wenxuan Zhang, See-Kiong Ng, Yang Deng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-06-30
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.30639`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了检索到的摘要和结论片段，未使用完整 PDF 文本。
