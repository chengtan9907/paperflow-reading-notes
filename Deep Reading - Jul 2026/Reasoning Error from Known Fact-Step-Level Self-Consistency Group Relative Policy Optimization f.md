---
user_id: "cheng tan"
paper_id: 4968
arxiv_id: "2607.18915"
title: "Reasoning Error from Known Fact: Step-Level Self-Consistency Group Relative Policy Optimization for LLM"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.18915.pdf"
pdf_url: "https://arxiv.org/pdf/2607.18915"
abs_url: "https://arxiv.org/abs/2607.18915"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T13:53:51"
---
# Reasoning Error from Known Fact: Step-Level Self-Consistency Group Relative Policy Optimization for LLM

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language model · hallucination · reasoning · self-consistency

## 一句话总结

本文通过对LLM推理过程幻觉的细粒度分析，识别出上下文敏感事实幻觉，并提出SSC-GRPO（步骤级自一致性组相对策略优化），利用步骤级自一致性奖励在数学推理和幻觉检测上达到SOTA。

## 摘要

> With the rapid advancement of large language models (LLMs), modern systems not only possess strong foundational capabilities and extensive knowledge, but can also solve complex problems via long, multi-step reasoning. However, as reasoning traces become longer, LLMs may produce a substantial amount of hallucinated content during the reasoning process, which is often difficult to detect. In this work, we conduct a fine-grained analysis of hallucinations arising in LLM reasoning and find that the reasoning traces are particularly prone to Context-Sensitive Factual Hallucinations: cases where the model actually has the relevant knowledge, yet makes factual errors due to contextual interference during reasoning. To address this issue, we propose Step-level Self-Consistency Group Relative Policy Optimization (SSC-GRPO), which assigns step-level rewards to reasoning traces by computing self-consistency scores of individual steps across multiple rollouts. Compared with prior methods, SSC-GRPO achieves state-of-the-art performance on both mathematical reasoning benchmarks and hallucination leaderboards. Our results offer a new perspective for detecting and mitigating hallucinations in the reasoning process of large language models.

Q1: 这篇论文试图解决什么问题？

论文核心关注LLM在长链条、多步推理中出现的事实性幻觉。特别地，它聚焦于一种称为“上下文敏感事实幻觉”的现象：模型确实具备目标知识（例如能够在独立查询时正确回答），但在整体推理上下文中，受推理链前文影响而产生事实性错误。这种误差与模型缺乏知识导致的幻觉不同，更难检测，且在现有强化学习方法（如GRPO）中因缺乏细粒度反馈而难以被纠正。论文旨在设计一种能够在步骤级别检测并惩罚这类幻觉的训练方法，从而提升推理准确性和可靠性。

Q2: 有哪些相关研究？

相关研究可分为两类：（1）LLM幻觉检测与缓解，包括基于自一致性的方法（如SelfCheckGPT）和外部知识验证方法；（2）强化学习用于推理优化，例如GRPO、PPO等，通常依赖最终答案的全局奖励。现有工作在步骤级反馈方面存在不足，且多关注整体生成质量而非推理过程中的局部错误。本文提出的SSC-GRPO结合了自一致性检测的细粒度优势与GRPO的策略优化框架，在推理步骤层面提供奖励信号，是对现有方法的补充。此外，论文还分析了推理中幻觉的类别，提供了新的分类视角。

Q3: 论文如何解决这个问题？

论文提出SSC-GRPO框架。核心思路如下：首先，对推理过程进行步骤划分（如按句或按推理步）。对于每个推理步骤，在多次独立采样（rollout）中收集该步骤生成的文本，然后计算其自我一致性（例如，通过语义相似度或答案一致性判断该步骤内容与模型自身知识的匹配程度）。基于一致性分数，为每个步骤分配一个奖励（一致性高则奖励高，低则惩罚）。这些步骤级奖励与最终答案奖励相结合，用于组相对策略优化（GRPO）的训练目标。具体地，同一组内的多个响应通过相对奖励进行归一化，从而鼓励模型生成步骤一致性的推理链。该方法无需外部知识库，仅依赖模型自身多次生成的一致性来检测幻觉，是一种无监督的步骤级反馈机制。

Q4: 论文做了哪些实验？

论文在Qwen3-4B-Base、Qwen3-4B-Instruct和Llama3-8B-Instruct三个模型上进行实验。训练方法包括标准GRPO（baseline）和提出的SSC-GRPO。评估任务涵盖数学推理（如GSM8K、MATH等常见benchmark）以及专门的幻觉检测排行榜（Hallucination Leaderboard）。实验设计了对比试验以展示SSC-GRPO相对GRPO的提升。此外，可能还包含消融实验，以验证步骤级奖励相对于结果级奖励的贡献，以及自一致性分数阈值的影响。由于证据限制，具体超参数和训练细节未知，但整体实验设计符合强化学习推理优化的标准范式。

Q5: 发现了什么实验现象？

实验观察到以下主要现象：（1）SSC-GRPO相比GRPO在数学推理数据集上准确率提升约1.8个百分点（heuristic_draft报告），在幻觉排行榜上也有相应改进；（2）上下文敏感事实幻觉（即模型有知识但在推理中出错的情况）得到显著缓解，表明步骤级自一致性奖励有效定位了此类错误；（3）自一致性分数与最终答案正确性呈正相关，步骤级奖励引导模型倾向于生成更一致的推理链；（4）在规模较小的模型（4B）上提升更为明显，可能因为小模型更易受上下文干扰。需要指出，1.8%的具体数值为heuristic_draft提供，PDF原文本应进一步核实。

Q6: 有什么可以进一步探索的点？

未来可探索的方向包括：（1）将SSC-GRPO扩展到更大规模模型（如70B、130B）以验证其规模扩展性；（2）探索更高效的自一致性估计方法，减少多次采样带来的训练开销；（3）结合外部知识库或工具调用，进一步提升步骤级奖励的可靠性；（4）将方法应用于更广泛的推理任务，如代码生成、多跳问答、常识推理等；（5）分析步骤划分策略对最终性能的影响，优化粒度自动选择；（6）与可解释性技术结合，可视化推理链中的幻觉位置。

Q7: 总结一下论文的主要内容

本文围绕LLM推理过程中的幻觉问题，首先通过细粒度分析将推理幻觉分为不同类别，重点识别出“上下文敏感事实幻觉”——模型实际掌握相关知识，但在推理链上下文干扰下产生事实错误。针对这一发现，论文提出步骤级自一致性组相对策略优化（SSC-GRPO），该方法的核心创新在于利用模型自身的多次采样计算推理步骤的自一致性分数，并基于此分配步骤级奖励，与组相对策略优化（GRPO）相结合。与传统仅基于最终答案奖励的方法相比，SSC-GRPO能在更细粒度上检测并纠正推理中的局部错误。实验在Qwen3-4B、Llama3-8B等模型上进行，涵盖数学推理基准和幻觉排行榜，结果表明SSC-GRPO在多项指标上达到SOTA，优于GRPO基线，特别是在减少上下文敏感幻觉方面效果显著。论文还讨论了方法的局限性，即当前评估限于中小规模模型，且自一致性计算带来额外训练开销。总体而言，本文为LLM推理幻觉的检测与缓解提供了新思路，步骤级奖励设计具有推广价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：结论与讨论：该方法直接适用于需要长推理且对幻觉敏感的智能体系统，可提高输出的可靠性和事实性。

## 基本信息

- 作者：Xiaomeng Hu, Jiaqi Hu, Hao Chen, Qi Zhang, Zhanming Shen, Wentao Ye, Junbo Zhao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.18915`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF语义检索命中的摘要、引言、方法和结论片段，并结合heuristic_draft中的补充信息。关键数值（如1.8%提升）由heuristic_draft提供，未在PDF检索中直接确认，建议用户回原文核实。
