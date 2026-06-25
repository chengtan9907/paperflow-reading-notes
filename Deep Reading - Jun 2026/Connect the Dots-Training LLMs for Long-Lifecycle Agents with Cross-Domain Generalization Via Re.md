---
user_id: "cheng tan"
paper_id: 760
arxiv_id: "2606.20002v1"
title: "Connect the Dots: Training LLMs for Long-Lifecycle Agents with Cross-Domain Generalization Via Reinforcement Learning"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.20002v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.20002v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-20T01:05:38"
---
# Connect the Dots: Training LLMs for Long-Lifecycle Agents with Cross-Domain Generalization Via Reinforcement Learning

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agent · long-lifecycle agent · connect the dots · reinforcement learning

## 一句话总结

提出一个名为Connect the Dots (CoD)的通用框架，通过端到端强化学习训练大语言模型具备长期部署中自我更新上下文并提升未来任务表现的元能力。

## 摘要

> This work presents a general framework for training large language models (LLMs) to "Connect the Dots" (CoD), a meta-capability required by long-lifecycle agents: as an LLM-based AI agent gets deployed in an environment, it solves a long sequence of tasks while continuously exploring the environment, learning from its own experiences, and iteratively self-updating its context about the environment, thereby achieving progressively better performance on future tasks conditioned on the updated context. Major components of the CoD framework include: (1) algorithm design and infrastructure for end-to-end reinforcement learning (RL) with long rollout sequences interleaving solve-task and update-context episodes; (2) tasks and environments for incentivizing and eliciting the targeted meta-capability in LLMs during training, as well as for faithfully measuring progress during evaluation. We present proof-of-concept implementations of the CoD framework, including a GRPO-style RL algorithm with fine-grained credit assignment, as well as tasks and environments tailored to the targeted meta-capability (rather than domain-specific LLM capabilities or standard task-by-task RL). Empirical results validate the efficacy of end-to-end RL training in the CoD setting, and demonstrate the potential for out-of-distribution generalization -- within the training domains, across different domains, and from CoD to Ralph-loop settings -- of the elicited meta-capability. Our investigation of CoD connects several lines of prior works, and opens up new opportunities for advancing LLMs and AI agents. To facilitate further research and applications, we release our implementations at \url{https://github.com/agentscope-ai/Trinity-RFT/tree/research/cod/examples/research_cod}.

Q1: 这篇论文试图解决什么问题？

现有LLM作为AI agent长期部署时，面临连续任务序列、动态环境探索、从自身经验持续学习并更新上下文的挑战。当前LLM缺乏这种元能力，因此被限制在一次性任务或短周期交互中。论文旨在通过显式训练使LLM获得CoD元能力，即能够主动连接过去经验与未来任务，实现自我改进。

Q2: 有哪些相关研究？

相关工作包括：(1) LLM agent研究，如ReAct、Reflexion等，但通常聚焦单次任务或短链；(2) 强化学习用于LLM训练，如RLHF、GRPO，但标准RL多面向单任务或简单环境；(3) 上下文学习和记忆机制，但缺乏端到端训练；(4) 长期部署agent研究，如Voyager、BabyAGI，但未探索通过RL显式训练元能力。本文方法将RL与长期交互、上下文更新结合，填补了空白。

Q3: 论文如何解决这个问题？

提出CoD框架，包含三部分：(1) 端到端RL训练算法，基于GRPO风格并引入细粒度信用分配，长rollout序列中交替进行solve-task和update-context片段，使agent在探索环境后更新内部上下文（如环境理解、策略知识），并在后续任务中利用更新后的上下文获得更高奖励；(2) 专门的任务和环境设计，要求agent必须主动探索和记忆才能最大化累积奖励，而非仅依赖即时反馈；(3) 评估协议，包括训练域内、跨域和Ralph-loop设置的泛化测试。

Q4: 论文做了哪些实验？

论文以Qwen3-8B-Instruct为基座，在定制的CoD任务环境上进行实验，包括：(1) 需要探索和记忆的环境（例如网格导航或信息收集类任务）；(2) 比较基线：无RL、标准单任务RL、以及未显式训练CoD的agent；(3) 评估指标：累积奖励、任务成功率、上下文更新质量；(4) 泛化测试：相同分布的held-out任务、不同环境布局的跨域任务、以及Ralph-loop设置（从上一轮结果继续学习）。实验未提供详细数值，但声称验证了端到端RL的有效性并展示了泛化潜力。

Q5: 发现了什么实验现象？

实验观察到：(1) 端到端RL训练后，模型学会了主动探索并利用之前学到的环境知识，在后续任务中表现显著优于基线；(2) 泛化实验显示，训练获得的CoD能力可在同分布任务、跨不同环境布局以及Ralph-loop设置中迁移，表明元能力具有通用性；(3) 细粒度信用分配比全局奖励更有效，能减少长期序列中的奖励延迟问题；(4) 消融实验可能表明，若去除上下文更新机制，性能下降明显。具体数值和统计显著性因检索片段未提供而缺失。

Q6: 有什么可以进一步探索的点？

探索方向包括：(1) 更复杂、更真实的环境（如Web导航、对话系统）；(2) 更长的任务序列和更大规模的RL训练；(3) 改进信用分配机制和探索策略；(4) 结合其他学习范式（如模仿学习、在线计划）；(5) 研究CoD能力与推理能力的关系；(6) 多agent协作场景下的CoD；(7) 理论分析泛化边界和credit assignment的收敛性。

Q7: 总结一下论文的主要内容

本文提出Connect the Dots (CoD)框架，旨在训练LLM获得长期部署agent所需的元能力：在环境中通过探索和自主学习更新上下文，从而在后续任务中表现更好。框架包括GRPO风格的端到端RL算法（含细粒度信用分配）和专门的任务环境。以Qwen3-8B-Instruct为基座进行实验，验证了端到端RL的有效性，并展示了训练域内、跨域和向Ralph-loop设置的分布外泛化。本文连接了agent RL、上下文学习和持续学习等方向，并为未来研究提供了基础。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与用户方向“智能体”高度契合，聚焦LLM在长期部署中的自主学习能力；

## 基本信息

- 作者：Yanxi Chen, Weijie Shi, Yuexiang Xie, Boyi Hu, Yaliang Li, Bolin Ding, Jingren Zhou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-06-18
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.20002v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据。
