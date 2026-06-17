---
user_id: "cheng tan"
paper_id: 302
arxiv_id: "2606.17053v1"
title: "Context-Aware RL for Agentic and Multimodal LLMs"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.17053v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.17053v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:06:26"
---
# Context-Aware RL for Agentic and Multimodal LLMs

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：context-aware reinforcement learning · large language model · long-horizon reasoning · multimodal reasoning

## 一句话总结

提出ContextRL，通过在基于结果的强化学习中增加上下文选择辅助目标，提升LLM在长时推理和多模态任务中的上下文感知能力，在17个基准上实现一致改进。

## 摘要

> Large language models (LLMs) often fail when answering requires identifying a small but decisive piece of evidence within a long or complex context, such as a single line in a tool trace or a subtle detail in an image. We propose ContextRL, a context-aware reinforcement learning (RL) method that improves long-horizon reasoning and multimodal performance through an \emph{indirect} auxiliary objective. Instead of supervising only the final answer, ContextRL presents the model with a query, an answer, and two highly similar contexts, and rewards it for selecting the context that supports the query--answer pair, thereby encouraging fine-grained grounding. We construct contrastive context data in two domains: for coding agents, trajectories serve as contexts, yielding 1k pairs built via condition filtering; for multimodal reasoning, images serve as contexts, yielding 7K pairs built via generative editing and similarity search. ContextRL achieves average gains of +2.2% over standard GRPO on 5 long-horizon benchmarks, and +1.8% across 12 diverse visual question answering benchmarks. To disentangle the effect of the proposed objective from that of additional data, we compare against data-augmentation baselines that repurpose the same contrastive contexts as standard query--context--answer examples. These baselines provide little to no improvement, showing that the gains arise from the proposed context-selection objective rather than from the contrastive data alone.

Q1: 这篇论文试图解决什么问题？

LLMs在处理长或复杂上下文时，常忽略微小但关键的证据（如工具跟踪中的一行代码、密集图像中的细节），导致局部合理但上下文不一致的决定。现有结果级RL后训练仅关注最终答案正确性，未直接培养上下文感知能力。

Q2: 有哪些相关研究？

相关研究包括：（1）基于RL的LLM后训练，如GRPO、PPO等，但通常只对最终答案奖励；（2）上下文学习（ICL）和检索增强生成（RAG），但依赖外部检索或示例排序；（3）多模态RL，如RLHF-V等，但未显式训练上下文选择性；（4）对比学习方法在表示学习中的应用，但未直接用于LLM后训练。本文首次将上下文选择作为辅助RL目标。

Q3: 论文如何解决这个问题？

ContextRL在GRPO框架上增加一个轻量级的上下文选择目标。具体步骤：（1）构造对比上下文对，对每个查询-答案对，构建两个高度相似的上下文（一个正确一个错误）；（2）训练时，模型看到查询、答案和两个上下文，需输出正确上下文的选择（通过奖励正确选择实现）；（3）该目标与原始结果奖励联合优化。数据构建：智能体领域利用条件过滤从轨迹中提取对比对（1k）；多模态领域通过生成式编辑和相似度搜索构建图像对比对（7k）。

Q4: 论文做了哪些实验？

在两个场景共17个基准上评估。（1）长时智能体任务：5个基准（如AgentBench系列），对比基线包括GRPO、数据增强SFT/RL等。（2）多模态VQA：12个基准（如MME-RealWorld、OlympiadBench等），模型为Klear-AgentForge-8B和Qwen3-VL-8B。消融实验包括：与纯结果RL对比、与相同数据不同目标（SFT或结果RL）对比、以及分析不同模型规模。

Q5: 发现了什么实验现象？

（1）ContextRL在5个智能体基准上平均超越GRPO +2.2%，在12个VQA基准上平均+1.8%。（2）数据增强基线：使用相同对比数据进行SFT或结果RL训练，几乎无提升或反而下降，证明提升来自上下文选择目标而非额外数据。（3）在部分基准上（如OlympiadBench Physics），提升尤为显著（+1.5~1.8）。（4）从Klear-AgentForge-8B基座开始训练，ContextRL达到与更大参考模型竞争的水平。

Q6: 有什么可以进一步探索的点？

（1）将ContextRL扩展到更多领域，如代码理解、数学推理；（2）结合搜索或回溯机制处理更复杂的上下文结构；（3）探索更高效的对比数据生成方法，减少人工构造；（4）研究上下文选择目标对更长上下文（如100k+ tokens）的scaling性质；（5）与过程奖励模型结合，同时捕捉中间步骤和上下文线索。

Q7: 总结一下论文的主要内容

论文提出ContextRL，一种上下文感知的强化学习方法，旨在解决LLMs在长上下文和多模态任务中忽略关键证据的问题。核心思想是在标准结果奖励基础上，增加一个间接的上下文选择目标：给定查询、答案和两个相似上下文，模型需选择正确的上下文，从而迫使模型关注细粒度证据。数据构建针对两个领域：智能体轨迹（通过条件过滤生成1k对比对）和多模态图像（通过生成式编辑和相似性搜索生成7k对比对）。实验在5个长时智能体基准和12个VQA基准上进行，以GRPO为基线，ContextRL平均提升+2.2%和+1.8%。关键消融实验表明，相同对比数据如果用SFT或结果RL训练，增益消失，证明改进来自目标本身而非数据。论文讨论了计算开销（仅增加一个轻量头）和与更大模型的竞争力。结论认为上下文选择是简单且有效的辅助信号。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：直接针对智能体系统中上下文利用的关键问题，与agent方向高度相关。

## 基本信息

- 作者：Peiyang Xu, Bangzheng Li, Sijia Liu, Karthik R. Narasimhan, Pramod Viswanath, Prateek Mittal, Xingyu Fu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.CV
- 日期：2026-06-15
- 推荐级别：**值得快速浏览**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.17053v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答主要参考了PDF检索证据中的Abstract、Introduction和Conclusion片段，并基于这些片段进行合理推断与组织。
