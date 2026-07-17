---
user_id: "cheng tan"
paper_id: 4073
arxiv_id: "2607.13389"
title: "Where Should RL Post-Training Compute Go? Model Size, Search, Learning, and Feedback"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.13389.pdf"
pdf_url: "https://arxiv.org/pdf/2607.13389"
abs_url: "https://arxiv.org/abs/2607.13389"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-17T01:03:27"
---
# Where Should RL Post-Training Compute Go? Model Size, Search, Learning, and Feedback

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning post-training · compute allocation · search-learning frontier · grpo

## 一句话总结

本文通过IsoFLOP实验框架，系统研究了强化学习后训练（RL post-training）中计算资源在搜索（search）、学习（learning）及不同反馈机制之间的最优分配，揭示了在固定预算下存在结构化分配行为，并比较了结果奖励模型、过程奖励模型和共同评审三种反馈方式的效果。

## 摘要

> 论文以数学推理为代理任务，使用GRPO和LoRA对Qwen2.5指令模型进行后训练，通过三个实验阶段探索计算分配。主要发现：搜索与学习之间的分配存在最优前沿；不同奖励模型在低搜索预算下差异不大，但高搜索预算下结果奖励模型优于过程奖励模型；共同评审的偏好可能与传统奖励不一致。研究为RL后训练的计算资源分配提供了系统性的实践指导。

Q1: 这篇论文试图解决什么问题？

在强化学习后训练（RL post-training）中，计算预算固定时，如何分配资源给模型规模、搜索（推理时的采样次数）、学习（参数更新）和反馈（奖励模型的计算成本）是一个未被系统研究的问题。当前实践往往凭经验分配，缺乏理论指导。论文旨在通过系统实验揭示这种分配的结构化行为，并提供最优分配策略的启示。

Q2: 有哪些相关研究？

相关研究包括预训练的缩放定律（scaling laws）、RLHF中的奖励模型设计、推理时搜索计算（如chain-of-thought self-consistency）、以及过程奖励模型（PRM）的比较。例如，Bai等人（2022）研究了RLHF的计算分配，但未考虑与搜索的联合优化。论文还引用了Gemini团队等的大规模RL后训练工作。本文的独特贡献是在固定预算下系统性地联合优化搜索、学习和反馈分配。

Q3: 论文如何解决这个问题？

论文设计了IsoFLOP（固定FLOP）实验框架，将总计算预算固定，然后变化搜索步数（K，即每个输入采样生成的完成数）和训练步数（update steps），同时保持模型大小和LoRA秩等常数。使用组相对策略优化（GRPO）训练Qwen2.5模型，在Polaris-53K数学推理数据集上评估。实验分三个阶段：Stage A研究搜索-学习分配前沿（使用结果奖励模型ORM）；Stage B比较不同奖励系统（ORM vs PRM vs Common Judge）；Stage D扩展到不同模型规模和评价指标。通过扫描分配空间识别最优区域。

Q4: 论文做了哪些实验？

所有实验使用GRPO和LoRA适配Qwen2.5指令模型（3B或7B），K=2（除非说明），最大生成长度L=2048。Stage A：固定总FLOPs，变化K和更新步数，在Polaris-53K上训练，用多个评判器（BPRM、Qwen2.5-Math-72B等）评估数学推理准确率。Stage B：在相同总FLOPs预算下，使用不同奖励模型（ORM、PRM、以及基于一个共享过程质量评估器的共同评审Common Judge），比较其性能和偏好一致性。Stage D：扩展实验包括不同的模型大小（如7B）和额外的评价基准。所有实验重复多次以确保可靠性。

Q5: 发现了什么实验现象？

Stage A显示存在一个结构化的分配平面：当搜索分配过高（即K大量增加但训练步数很少）时，模型未能充分学习，性能显著下降；反之，当搜索分配过低（K=1），模型不能利用多采样带来的改进。存在一个最优分配区域，在该区域内性能基本平坦。Stage B观察到：在低搜索预算下，ORM和PRM性能相似；但高搜索预算下，ORM明显优于PRM，表明ORM更能有效利用多次采样。Common Judge的偏好有时与任务性能不一致，说明其可能引入偏移。此外，不同评判器之间的排名可能不一致，提示单一评价指标存在局限性。

Q6: 有什么可以进一步探索的点？

1) 将研究扩展到更真实的任务场景如机器人操作和规划，直接验证结论的迁移性。2) 考虑动态预算分配，即根据任务难度在线调整搜索-学习分配。3) 联合优化模型规模、搜索、学习和反馈，构建更全面的缩放定律。4) 探索其他RL算法如PPO与GRPO的比较。5) 研究更复杂的反馈机制如集成奖励模型或人类反馈的在线收集。6) 分析分配对鲁棒性、安全性等方面的影响。7) 扩展到不同的基础模型和领域（代码、科学等）。

Q7: 总结一下论文的主要内容

本文针对强化学习后训练（RL post-training）中的计算资源分配问题，提出并系统研究了一个核心问题：在固定计算预算下，如何分配资源给搜索（每个输入的采样数）、学习（训练步数）以及反馈（奖励模型的计算成本）以最大化模型在下游任务上的性能。通过将问题类比于预训练的缩放定律研究，作者采用IsoFLOP实验框架，以数学推理作为代理任务，使用GRPO和LoRA对Qwen2.5模型进行后训练。实验分为三个阶段：第一阶段（Stage A）绘制搜索-学习分配的前沿，发现存在清晰的最优区域，搜索过多或过少都会降低性能；第二阶段（Stage B）比较结果奖励模型（ORM）、过程奖励模型（PRM）和共同评审（Common Judge）三种反馈机制，发现ORM在高搜索预算下显著优于PRM，而共同评审的偏好不一定与任务性能一致；第三阶段（Stage D）扩展到更大模型和额外评估基准，验证结论的鲁棒性。论文的主要贡献在于揭示了RL后训练中计算分配的结构化行为，提供了多种反馈机制的对比，并为实际系统优化（如机器人学习中的模块改进）提供了指导。局限性包括使用数学推理作为代理任务，可能不能完全代表机器人后训练场景；以及实验仅探索了有限的计算预算和算法设置。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本研究探讨的RL后训练计算分配问题直接关联智能体系统的推理和规划模块优化

## 基本信息

- 作者：Patrick Wilhelm, Odej Kao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.13389`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据，主要依据实验设计和结果片段。
