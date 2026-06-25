---
user_id: "cheng tan"
paper_id: 1236
arxiv_id: "2606.23525"
title: "Self-Compacting Language Model Agents"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23525.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23525"
abs_url: "https://arxiv.org/abs/2606.23525"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:11:37"
---
# Self-Compacting Language Model Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-compaction · language model agents · context window · summarization

## 一句话总结

提出SelfCompact，一种让语言模型自身决定何时及如何压缩智能体轨迹的脚手架，无需微调或外部监督，在数学推理和搜索任务上匹配或超越固定间隔压缩且成本更低。

## 摘要

> Long agent traces composed of chains of thought and tool calls accumulate stale content that anchor subsequent generations, and eventually outgrow the context window. Existing scaffolds mitigate it with fixed-interval compaction triggered at a token threshold. Such triggers pay no heed to trajectory structure, risking discard of partial results mid-derivation or mid-search. We propose SelfCompact, a scaffold that allows the model itself to decide when and how to compact. Specifically, it pairs two inference-time elements: (i) a compaction tool the model invokes to summarize the accumulated context, and (ii) a lightweight rubric specifying when to fire (a sub-task has resolved, or the trajectory is converging) and when to suppress (mid-derivation, or when stuck). Both are needed. The tool alone is unevenly used across open-weight models, often invoked at unhelpful moments or not at all; the rubric alone cannot act. Together, they elicit effective adaptive compaction without any fine-tuning or external supervision. We present empirical results on six benchmarks (competitive math and agentic search) and seven models. Our results show that SelfCompact matches or exceeds fixed-interval summarization at a fraction of the token cost, improving over a no-summarization baseline by up to 18.1 points on math and 5-9 points on agentic search at 30-70% lower per-question cost. Our results expose a meta-cognitive gap: although unprompted models cannot reliably tell when their own context is rotting, a lightweight rubric closes this gap, reframing when to compact as a capability that scaffolds can supply without training.

Q1: 这篇论文试图解决什么问题？

语言模型智能体在执行长链推理或工具调用时，产生大量中间内容（思维链、工具输出），这些内容逐渐变得过时并锚定后续生成，最终超出上下文窗口限制。现有解决方案采用固定的令牌阈值触发压缩，但这种策略不考虑轨迹结构，可能在推导或搜索中间阶段丢弃部分结果，导致性能下降。核心问题是如何在不进行训练或外部监督的情况下，让模型自适应地决定何时以及如何压缩上下文。

Q2: 有哪些相关研究？

相关工作包括：（1）上下文压缩：在API基础模型中已成为标准内置功能，但通常依赖固定阈值或外部信号。（2）工具使用模型：近期进展使模型能调用工具，但压缩工具的使用行为不均匀。（3）自我认知：研究表明模型难以可靠地判断自身上下文是否过时，存在“元认知差距”。SelfCompact通过轻量级规则弥补这一差距，无需训练。

Q3: 论文如何解决这个问题？

SelfCompact包含两个推理时组件：（1）内联压缩工具（compaction tool）：模型可自行调用的工具，用于总结累积的上下文。（2）轻量级规则（rubric）：指定何时触发压缩（子任务解决或轨迹收敛）和何时抑制压缩（推导中间或卡住时）。规则以自然语言描述，不涉及训练。两者结合实现自适应压缩：工具提供机制，规则确保在正确时机使用。

Q4: 论文做了哪些实验？

实验在6个基准上进行：竞赛数学（4个推理模型）和智能体搜索（3个部署代理）。比较基线包括无压缩、固定间隔压缩等。评估指标包括准确率和每次查询的令牌成本。详细实验设置和成本核算参见附录C和D。

Q5: 发现了什么实验现象？

实验现象：（1）SelfCompact在数学和搜索任务上匹配或超越固定间隔压缩，同时成本降低30-70%。（2）消融实验表明，仅提供压缩工具会导致模型在不当时机调用或不调用，而加入规则后性能显著提升。（3）未加提示的模型无法可靠识别自身上下文是否过时，轻量级规则有效弥补这种元认知差距。

Q6: 有什么可以进一步探索的点？

未来方向包括：（1）将SelfCompact扩展到更长轨迹和多步骤任务。（2）研究规则设计的自动生成或自适应调整。（3）探索在训练中融入压缩决策的可能性。（4）评估在其他模型架构（如Mamba）上的效果。（5）考虑多智能体场景下的分布式压缩。

Q7: 总结一下论文的主要内容

本文提出SelfCompact，一种让语言模型智能体自主决定何时及如何压缩轨迹的脚手架。它由内联压缩工具和轻量级规则组成，无需微调或外部监督。在6个基准和7个模型上的实验表明，SelfCompact匹配或超越固定间隔压缩，同时大大降低令牌成本。消融实验揭示了元认知差距：模型自身难以判断上下文是否过时，而规则有效弥补了这一差距。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该论文直接涉及智能体轨迹压缩问题，与智能体方向高度相关（权重0.10）。

## 基本信息

- 作者：Tianjian Li, Jingyu Zhang, William Jurayj, Xi Wang, Chuanyang Jin, Mehrdad Farajtabar, Eric Nalisnick, Daniel Khashabi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23525`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括Abstract、结论和结果片段。
