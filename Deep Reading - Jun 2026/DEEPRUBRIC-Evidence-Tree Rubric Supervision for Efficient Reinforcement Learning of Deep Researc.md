---
user_id: "cheng tan"
paper_id: 293
arxiv_id: "2606.17029v1"
title: "DEEPRUBRIC: Evidence-Tree Rubric Supervision for Efficient Reinforcement Learning of Deep Research Agents"
institution: "Shandong University, Qingdao, China; Zhongguancun Academy, Beijing, China; Fudan University, Shanghai, China"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/arXiv - Jun 2026/2606.17029v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.17029v1"
abs_url: "https://arxiv.org/abs/2606.17029v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-04-v5"
daily_note_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-16T21:37:12"
---
# DEEPRUBRIC: Evidence-Tree Rubric Supervision for Efficient Reinforcement Learning of Deep Research Agents

> ★★★☆☆ 值得快速浏览 · 约 20 分钟 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：deep research agents · reinforcement learning · rubric-based rewards · evidence tree

## 一句话总结

DeepRubric 通过“证据优先”的逆向构建方法生成证据树，从而合成高质量的查询-评分准则对，显著提升了深度研究智能体在强化学习中的训练效率。

## 摘要

> Deep research agents synthesize long-form reports by searching and reasoning over retrieved evidence. Reinforcement learning with rubric-based rewards improves these agents by optimizing them against checkable criteria that translate report quality into reward signals, but its efficiency depends on whether those criteria reliably capture the task scope and evidence needs. Most existing studies ask an LLM to generate rubrics for a given query, but when the model fails to infer the underlying information needs, the generated rubrics may be incomplete and reduce RL efficiency. To obtain more reliable query--rubric supervision, we introduce DeepRubric, a data construction framework that reverses this process: instead of inferring evaluation criteria for a given query, it first determines what an evidence-backed report should be evaluated on and then synthesizes aligned query--rubric pairs from those evaluation targets. Starting from a sampled seed topic, DeepRubric builds an evidence tree by recursively expanding evidence-backed sub-questions, whose leaves serve as atomic and verifiable evaluation targets. It then uses the evidence tree to synthesize the training query and rubrics, ensuring that the reward evaluates exactly the information requested by the query. Using DeepRubric, we construct 9K query--rubric supervision examples and train DeepRubric-8B with rubric-based GRPO, achieving comparable performance to prior open state-of-the-art deep research models across three benchmarks with roughly 13x fewer RL GPU-hours.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决深度研究智能体（Deep Research Agents）在强化学习（RL）优化过程中，奖励信号（Reward Signals）质量低下且不稳定的核心问题。在长文档合成任务中，智能体需要根据查询（Query）进行多步搜索和推理。目前的 RL 优化通常依赖于 LLM 生成的评分准则（Rubrics）来提供奖励，但这种“查询优先”（Query-First）的模式存在严重缺陷：如果生成准则的 LLM 无法准确预判完成该任务所需的全部信息细节，生成的准则就会残缺不全。这种不完整的监督信号会导致 RL 训练效率极低，智能体难以学习到全面的搜索策略，且往往需要耗费巨大的计算资源（如数千 GPU 小时）才能达到理想效果。因此，如何构建高质量、覆盖面广且具备事实依据的“查询-评分准则”监督数据，是提升研究智能体性能的关键瓶颈。

Q2: 有哪些相关研究？

相关研究主要集中在以下三个方面：首先是深度研究智能体（Deep Research Agents），如 OpenAI Deep Research 和各类基于 RAG 的长文档生成系统，它们强调多步检索与信息整合。其次是基于评分准则的强化学习（Rubric-based RL），这类方法利用 LLM 作为评判者，根据预设的 Checklist 对生成内容进行细粒度评估，是目前优化智能体行为的主流手段。最后是强化学习算法的改进，特别是 GRPO（Group Relative Policy Optimization）等算法，它们在处理大规模生成任务时表现出比传统 PPO 更高的效率。然而，现有工作大多忽略了 Rubric 本身的构建质量，往往直接依赖 LLM 对 Query 的直觉推断，缺乏系统性的数据工程来保证 Rubric 的完备性。

Q3: 论文如何解决这个问题？

DeepRubric 提出了一种创新的“证据优先”（Evidence-First）数据构建框架，其核心逻辑是“先有证据，后有题目”。首先，从一个种子话题出发，系统通过递归地提出并回答子问题来构建一棵“证据树”（Evidence Tree）。在构建过程中，每个子问题都必须通过实际的检索和推理得到证据支持，确保了树节点的真实性。接着，将证据树的叶子节点转化为原子化且可验证的评估目标（Atomic Evaluation Targets），这些目标构成了 Rubric 的核心。最后，基于整棵证据树所涵盖的信息，逆向合成一个能够覆盖这些知识点的综合性查询（Query）。通过这种方式，DeepRubric 确保了每一个评分准则都有据可查，且查询与准则之间达到了完美的对齐。利用该框架，研究者构建了 9,000 组高质量监督数据，并采用 GRPO 算法对 8B 规模的模型进行强化学习训练。

Q4: 论文做了哪些实验？

论文进行了详尽的实验验证：首先，利用 DeepRubric 框架合成了 9K 组查询-评分准则对，并训练了 DeepRubric-8B 模型。实验在三个主流的深度研究基准测试上展开，评估指标涵盖了报告的准确性、完整性和证据支持度。关键的对比实验显示，DeepRubric-8B 在性能上达到了与当前开源最强深度研究模型（通常规模更大或训练时间更长）相当的水平。最显著的实验结果在于效率对比：DeepRubric 仅消耗了约 1/13 的 RL GPU 小时（GPU-hours）就达到了同等甚至更优的性能。此外，消融实验验证了“证据优先”构建模式相比于传统“查询优先”模式在提升奖励信号质量方面的决定性作用。

Q5: 有什么可以进一步探索的点？

未来的研究方向包括：第一，扩展证据树的多模态能力，使其能够处理包含图表、图像和表格的复杂证据，从而生成更具专业深度的科学报告。第二，探索动态 Rubric 机制，即在智能体执行任务的过程中，根据实时检索到的新信息动态调整评估准则。第三，将 DeepRubric 框架推广到其他复杂的智能体任务中，如自动化软件工程或法律案例分析，验证其在不同垂直领域的泛化性。第四，进一步优化证据树的构建效率，降低合成高质量监督数据本身的计算开销。

Q6: 总结一下论文的主要内容

本文针对深度研究智能体在强化学习中面临的奖励信号不准确、训练效率低的问题，提出了 DeepRubric 框架。该框架摒弃了传统的“根据问题猜答案”的准则生成模式，转而采用“根据证据定问题”的逆向构建思路。通过递归构建证据树，DeepRubric 能够生成具有高度可验证性和覆盖面的原子化评分准则。实验表明，利用该框架生成的 9K 条数据，配合 GRPO 算法，仅需以往方法 1/13 的计算资源即可训练出性能卓越的 8B 规模研究智能体。这一工作为高效、可解释的智能体训练提供了新的范式，证明了高质量合成监督信号在复杂任务优化中的核心价值。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：该研究直接关联智能体（Agent）方向，特别是长文档生成和复杂推理场景。

## 基本信息

- 作者：Minghang Zhu, Chuyang Wei, Junhao Xu, Yilin Cheng, Zhumin Chen, Jiyan He
- 机构：Shandong University, Qingdao, China; Zhongguancun Academy, Beijing, China; Fudan University, Shanghai, China
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-15
- 推荐级别：**值得快速浏览**
- 预计阅读时间：约 20 分钟
- 解析来源：PDF 全文
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.17029v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要及检索到的核心片段，并结合了深度研究智能体领域的背景知识进行推导。
