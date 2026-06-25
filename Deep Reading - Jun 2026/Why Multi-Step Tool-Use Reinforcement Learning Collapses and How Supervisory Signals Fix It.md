---
user_id: "cheng tan"
paper_id: 1385
arxiv_id: "2606.26027v1"
title: "Why Multi-Step Tool-Use Reinforcement Learning Collapses and How Supervisory Signals Fix It"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.26027v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.26027v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T16:00:44"
---
# Why Multi-Step Tool-Use Reinforcement Learning Collapses and How Supervisory Signals Fix It

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · tool use · large language models · agentic RL

## 一句话总结

本文揭示了多步工具使用强化学习中单独使用RL会导致训练崩溃或性能停滞，其根源在于控制token的概率尖峰，并证明了多种监督信号可有效稳定训练。

## 摘要

> Tool use enables large language models (LLMs) to perform complex tasks, and recent agentic reinforcement learning (RL) methods show promise for enhancing model capabilities. However, RL alone often leads to instability or limited gains in tool-use tasks. In our experiments, some models exhibit catastrophic collapse, where performance abruptly drops and tool-invocation structures fail. The analysis reveals that these failures stem from unexpected probability spikes in specific control tokens, disrupting structured execution, yet the underlying tool-use capability remains intact, merely obscured by specific formats. To address this, we systematically investigate a diverse set of supervisory signals, including off-policy supervision, hint-based guidance, erroneous example supervision, and others, applied under both synchronous and interleaved training schemes. We find that interleaving supervised fine-tuning (SFT) with RL substantially improves stability, but exhibits degraded performance under format and content out-of-distribution (OOD) evaluation. We also analyze the impact of learning rates and generalization across settings. These results highlight the importance of understanding RL failures and demonstrate how diverse supervisory signals can guide exploratory learning, enabling robust training of LLMs for complex, multi-step tool-use tasks. Our Code is available at https://github.com/hypasd-art/Tool-RL-Box.

Q1: 这篇论文试图解决什么问题？

多步工具使用任务中，单独使用强化学习训练大语言模型时出现的训练不稳定、性能停滞甚至灾难性崩溃问题。具体表现为模型在训练中突然性能骤降，工具调用结构（如控制token）失效，严重影响可靠性和实用性。现有智能体RL方法虽在复杂任务上表现良好，但稳定性不足，缺乏对失败机制的深入理解。

Q2: 有哪些相关研究？

相关研究包括：1）大语言模型工具使用扩展（Hao et al., 2025a; Prabhakar et al., 2025a; Qin et al., 2024），使模型能调用外部API和环境。2）智能体强化学习方法（Zhang et al., 2025b; Wu et al., 2025），在多步任务中展示潜力但常不稳定。3）监督微调与RL结合的一般做法，但缺乏针对工具使用场景失败机制的系统分析。本工作填补了理解RL训练崩溃原因及监督信号干预效果的空白。

Q3: 论文如何解决这个问题？

论文首先通过实验观察并分析了RL训练崩溃的机制：特定控制token（如格式标记）在训练中出现概率尖峰，破坏结构化执行，而模型工具使用能力并未丧失。然后系统比较了多种监督信号作为干预手段：离线监督（off-policy supervision）、提示引导（hint-based guidance）、错误示例监督（erroneous example supervision）等，并在两种训练方案（同步SFT+RL和交错SFT+RL）下评估效果。特别发现交错训练（interleaving SFT with RL）能显著提升训练稳定性，但分布外泛化（OOD）性能下降。还分析了学习率对稳定性的影响，指出过高学习率易导致崩溃。通过多样化监督信号引导探索性学习，实现鲁棒训练。

Q4: 论文做了哪些实验？

论文在多种工具使用任务上进行了强化学习训练实验，具体任务类型可能涉及API调用。实验对比了：1）纯RL基线；2）RL结合不同监督信号（离线监督、提示引导、错误示例监督等）；3）两种训练方案（同步SFT+RL与交错SFT+RL）。评估指标包括训练稳定性（是否出现性能崩溃）、最终任务性能、格式和内容分布外泛化表现以及学习率敏感性。实验设置考虑了跨设置泛化，但具体数值未在摘要证据中提供。

Q5: 发现了什么实验现象？

关键实验现象：1）单用RL时，部分模型在训练过程中突然崩溃，性能骤降且工具调用结构失效，分析发现控制token概率出现尖峰；2）崩溃后模型底层工具使用能力并未丢失，仅被错误格式掩盖；3）交错SFT+RL显著提升稳定性，几乎消除崩溃现象，但代价是格式和内容分布外泛化（OOD）性能下降；4）学习率过高加剧不稳定，但适当学习率下监督信号有效；5）不同监督信号效果差异：离线监督和错误示例监督各有优劣，提示引导可能限制探索。这些发现揭示了稳定性与泛化之间的权衡。

Q6: 有什么可以进一步探索的点？

1）扩展训练数据规模，探索数据量对稳定性和泛化的影响；2）更深入分析控制token概率动态与崩溃的因果机制，设计针对性正则化方法；3）开发兼顾稳定性和OOD泛化的新型监督信号或混合训练策略；4）研究自适应学习率调度以避免手动调整；5）在更复杂、真实的工具环境（如多API组合任务）中验证发现；6）探索在其他智能体RL场景（如网页导航、代码生成）中的适用性。

Q7: 总结一下论文的主要内容

本文系统研究了多步工具使用任务中强化学习训练的不稳定性问题，并探索了多种监督信号作为解决方案。首先，论文通过实验揭示了单独使用RL会导致模型在训练中突然崩溃，性能急剧下降，工具调用结构（例如控制token）失效。深入分析表明，崩溃根源在于特定控制token（如格式标记）的概率出现尖峰，破坏了结构化执行顺序，但模型的底层工具使用能力并未丧失，只是被误格式掩盖。这一发现挑战了“RL失败意味着能力不足”的直觉。

为了解决该问题，论文提出了一个统一的实验框架，系统比较了四种监督信号：离线监督、提示引导、错误示例监督以及组合信号，并在两种训练方案（同步SFT+RL和交错SFT+RL）下评估。主要发现包括：交错SFT+RL能显著提高训练稳定性，几乎避免崩溃现象，但导致格式和内容分布外泛化性能下降；同步方案稳定性较差但泛化稍好。学习率分析显示过高的学习率会加剧不稳定性和崩溃风险。此外，论文还跨设置测试了泛化能力，表明不同任务间存在迁移差异。

实验结果表明，理解RL失败的原因比简单调整超参数更重要。多样化的监督信号能引导模型在探索中保持结构完整性，但存在稳定性-泛化权衡。论文局限性包括训练数据规模有限、工具环境开源受限，以及OOD评估仅覆盖格式和内容变化。整体上，本文为智能体RL训练提供了重要见解和实用指导。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文直接研究智能体中的工具使用强化学习，与你关注的agent方向高度相关。

## 基本信息

- 作者：Yupu Hao, Zhuoran Jin, Huanxuan Liao, Kang Liu, Jun Zhao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.26027v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据
