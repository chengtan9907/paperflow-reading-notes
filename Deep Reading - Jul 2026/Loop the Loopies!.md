---
user_id: "cheng tan"
paper_id: 4593
arxiv_id: "2607.16051"
title: "Loop the Loopies!"
institution: "IQuest Research"
publish_date: "2026-07-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.16051.pdf"
pdf_url: "https://arxiv.org/pdf/2607.16051"
abs_url: "https://arxiv.org/abs/2607.16051"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T01:44:20"
---
# Loop the Loopies!

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：looped transformer · mixture of experts · reasoning · ipho

## 一句话总结

Loopie 系列循环 MoE Transformer 在相同计算预算下超越标准 Transformer，并在 IMO 和 IPhO 获得金牌。

## 摘要

> Loopie 系列包括两个 MoE 模型：20B 参数（2B 活跃）和 6B（0.6B 活跃）。它解决了循环 Transformer 在固定预训练计算下增加参数优于循环的挑战。后训练管道赋予其强大推理能力，在 2025 年 IMO 和 IPhO 获得金牌，无需工具。

Q1: 这篇论文试图解决什么问题？

循环 Transformer 面临核心挑战：给定预训练计算固定时，增加参数数量通常比循环使用模型多次表现更好。本文旨在使循环 Transformer 在相同计算预算下匹配甚至超越标准（非循环）Transformer，从而释放循环架构的潜力。

Q2: 有哪些相关研究？

相关研究包括循环 Transformer 架构（如 ALBERT、Universal Transformer）、Mixture-of-Experts（MoE）模型（如 Qwen3-MoE、Mixtral），以及计算匹配的扩展方法。本文以 Qwen3-MoE 为强基线，对比训练相同预算的 30B-A3B 标准 MoE 模型。

Q3: 论文如何解决这个问题？

提出 Loopie Recipe，一种计算匹配的扩展方法，设计两个循环 MoE 模型 Loopie-20B-A2B 和 Loopie-6B-A0.6B，均训练两个循环步。关键包括：1) 使用 MoE 架构减少活跃参数；2) 后训练流水线，包含新颖的监督预训练阶段，增强推理能力。

Q4: 论文做了哪些实验？

在 800B tokens 上预训练，与 30B-A3B 标准 MoE Transformer 进行消融比较。评估基准包括 AMC、OlympiadBench、IFEval、ARC-Challenge、MMLU-Redux 以及 2025 年 IMO 和 IPhO 竞赛。通过对比消融验证 Loopie Recipe 的有效性。

Q5: 发现了什么实验现象？

循环模型 Loopie 在多个基准上优于标准 Transformer 基线，尤其在推理密集型任务（IMO/IPhO）上达到金牌水平，无需外部工具。消融表明计算匹配的扩展方法有效克服了循环 Transformer 的先前劣势。未报告反直觉趋势或失败案例，但可能训练规模限制了步数探索。

Q6: 有什么可以进一步探索的点？

1) 探索更多循环步数（>2）及其与参数扩展的权衡；2) 将 Loopie Recipe 应用于更大规模模型或更多领域；3) 研究循环步数与推理深度之间的关系；4) 将循环 Transformer 应用于在线学习或持续学习场景；5) 在更广泛的任务（如多轮对话、规划）中验证泛化能力。

Q7: 总结一下论文的主要内容

论文提出 Loopie，两个循环 MoE Transformer 模型（20B/2B 活跃与 6B/0.6B 活跃），通过 Loopie Recipe 解决固定计算下循环 Transformer 的扩展挑战。在多个学术基准和 IMO/IPhO 竞赛中，Loopie 在相同计算预算下超越标准 Qwen3-MoE 基线，并通过后训练流水线获得强大推理能力。主要贡献包括：设计计算匹配的扩展方法、验证循环架构的潜力、实现竞赛级推理性能。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：循环 Transformer 与智能体中的多步推理有潜在联系，但本文未直接涉及 agent 场景。

## 基本信息

- 作者：Zitian Gao, Yilong Chen, Yihao Xiao, Xinyu Yang, Ran Tao, Joey Zhou, Bryan Dai
- 机构：IQuest Research
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.16051`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（关键片段来自 Abstract、Can Looped Transformers...、Architecture 等）。
