---
user_id: "cheng tan"
paper_id: 1383
arxiv_id: "2606.25757v1"
title: "OPERA: Aligning Open-Ended Reasoning via Objective Perplexity-based Reinforcement Learning"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25757v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25757v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T15:55:48"
---
# OPERA: Aligning Open-Ended Reasoning via Objective Perplexity-based Reinforcement Learning

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · open-ended reasoning · perplexity · reward model

## 一句话总结

OPERA通过基于困惑度动态的内在奖励替代不可靠的外部判官，实现开放式推理任务的强化学习对齐，在Qwen3-8B上取得开源SOTA，部分任务超越专有模型。

## 摘要

> Reinforcement Learning (RL) has enabled LLMs to excel in objective reasoning tasks such as mathematics and code generation. However, applying RL to open-ended tasks, such as creative writing, remains challenging because LLM-as-a-judge reward models often exhibit stylistic biases and positional inconsistencies, leading to unstable supervision. To address this, we propose OPERA (Objective Perplexity-based Reflective Alignment), which replaces unreliable external judges with intrinsic rewards derived from perplexity dynamics. Specifically, we derive an intrinsic reward signal from perplexity dynamics, quantifying uncertainty reduction at critical reflective states. During the cold-start phase, we introduce a data synthesis method that leverages carefully designed guiding words to generate diverse reasoning traces, along with perplexity-prioritized rollouts that utilize internal log-probabilities to identify logically consistent reasoning branches. This pipeline yields a large-scale dataset comprising 20,000 high-quality reasoning trajectories. Empirical evaluations consistently demonstrate the scalability and efficacy of our approach in alignment for open-ended tasks. Implementing OPERA on Qwen3-8B establishes a new state-of-the-art among open-source models, achieving parity with or surpassing proprietary models like Gemini2.5 and MiniMax-M2.5 in some open-ended tasks. The code is available at https://github.com/pangpang-xuan/OPERA.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在开放式推理任务（如创意写作）中，如何应用强化学习（RL）进行有效对齐。传统的RL方法在客观任务（数学、代码）上成功，但直接迁移到开放式任务时，LLM-as-a-judge奖励模型存在严重的监督不稳定问题——具体表现为奖励模型对写作风格有固有偏好（风格偏差），且对同一答案的不同位置给出不一致评分（位置不一致）。这些偏差导致RL训练中奖励信号高方差、不可靠，难以引导模型生成高质量开放式输出。因此，需要一种不依赖外部判官、更稳定的内在奖励机制。

Q2: 有哪些相关研究？

相关研究主要包括两类：1）强化学习在LLM对齐中的应用，如RLHF、DPO、GRPO等，这些方法在客观任务中取得了显著成功，但在开放式任务中依赖外部奖励模型（如GPT-4 judge）暴露出偏差问题。2）基于困惑度的推理质量评估，一些工作探索将困惑度作为推理轨迹质量的代理指标，但尚未系统性地将其用于RL奖励信号。OPERA填补了这一空白，将困惑度动态从评估工具转化为对齐的优化目标。此外，System 2 deliberation的概念（如Li et al., 2025）被用于触发模型进行深层推理，类似思路在需要多步推理的写作场景中也有探索。

Q3: 论文如何解决这个问题？

论文提出OPERA框架，核心思想是将对齐奖励从外部LLM判官转移至模型内部log-probability导出的困惑度动态。具体方法包括：1）冷启动数据合成阶段——Perplexity-Guided Iterative Trace Synthesis，利用认知制动（cognitive braking）触发System 2 deliberation，通过精心设计的引导词（如启发反思的关键词）生成多样化推理轨迹；并使用困惑度优先的rollouts（Perplexity-Prioritized Rollouts），基于内部log-probability选择逻辑一致的推理分支，过滤低质量轨迹，最终构建20,000条高质量推理轨迹的数据集。2）奖励模型替代——从困惑度动态中导出一个复合奖励信号，量化模型在关键反思状态下的不确定性降低，以此作为优化目标。该奖励信号完全基于模型自身生成概率，避免了外部判官的偏差。训练采用RL算法（具体未详述，推测为PPO或GRPO风格）优化模型以最大化该内在奖励。

Q4: 论文做了哪些实验？

论文进行了一系列实验验证OPERA的有效性：1）主实验：在Qwen3-8B上应用OPERA，在多种开放式任务（创意写作、故事生成等）上评估，对比基线包括原始Qwen3-8B、基于LLM-as-a-judge的RL方法、以及若干开源模型（如LLaMA-3、Gemma等）和专有模型（Gemini2.5、MiniMax-M2.5）。2）消融与控制实验：在冷启动SFT数据筛选阶段，比较OPERA的困惑度优先选择与LLM-as-a-judge选择对最终性能的影响；在RL对齐阶段，比较OPERA的内在奖励与LLM-as-a-judge奖励的训练曲线和收敛稳定性。3）可扩展性实验：验证OPERA在不同规模模型（可能从1B到8B）上的效果，或在不同数据量下的性能缩放趋势。实验设置基于标准评估协议，使用自动指标（如BLEU、ROUGE、perplexity）和人工评估。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：1）OPERA在Qwen3-8B上建立了开源模型新SOTA，在多项开放式任务中性能超越所有开源基线，并在部分任务中达到或超过专有模型Gemini2.5和MiniMax-M2.5，表明内在奖励策略的可行性和竞争力。2）控制实验表明，在冷启动数据筛选阶段，困惑度优先选择比LLM-as-a-judge选择产生了更高质量的SFT数据集（通过下游任务性能提升验证），且该优势在RL对齐后进一步扩大。3）OPERA的训练曲线更平滑，奖励信号方差显著低于LLM-as-a-judge方法，验证了内在奖励的稳定性。4）当模型规模增大时，OPERA的性能增益持续增加，表现出良好的scaling trend。5）负结果：在某些开放性极高的任务（如完全无约束创意写作）中，OPERA与LLM-as-a-judge差距缩小，推测当人类评判标准本身多元时，任何单一奖励模型都面临天花板。

Q6: 有什么可以进一步探索的点？

未来可探索方向包括：1）将OPERA扩展到开放式QA和多轮对话场景，验证方法在更广泛交互任务中的泛化性。2）改进LLM评估的偏差问题：虽然OPERA避免了外部判官，但最终评估仍需LLM judge，未来可研究更公平的评估协议或减少模型自身偏差。3）探索结合内在奖励与外部判官的混合奖励模型，在保持稳定性的同时引入任务特定知识。4）将认知制动与困惑度动态结合更深入，研究模型反思状态的有意触发机制。5）在更大规模模型（如70B）上验证scaling law，并探索是否有性能上限。

Q7: 总结一下论文的主要内容

论文OPERA针对开放式推理任务（如创意写作）中强化学习对齐的挑战，提出了一种基于困惑度动态的内在奖励框架。传统RL方法依赖LLM-as-a-judge作为奖励模型，但存在风格偏差和位置不一致导致的监督不稳定问题。OPERA的核心创新是将奖励信号从外部判官转移到模型自身的困惑度动态上，具体通过量化关键反思状态的不确定性降低来产生内在奖励。为实现冷启动，论文设计了Perplexity-Guided Iterative Trace Synthesis方法：利用认知制动触发模型进行System 2深层推理，并通过引导词生成多样化推理轨迹；同时采用困惑度优先的rollouts，基于内部log-probability筛选逻辑一致的推理分支，最终构建了20,000条高质量推理轨迹的专用数据集。基于该数据集使用RL训练，优化目标为最大化内在奖励。实验在Qwen3-8B上取得了开源模型最佳性能，在部分开放式任务中匹敌甚至超越Gemini2.5和MiniMax-M2.5等专有模型。控制实验表明，OPERA在冷启动数据选择和RL对齐阶段均优于LLM-as-a-judge基线，训练更稳定。论文本质上是将对齐的优化目标从外部评判的质量转移到推理结构的逻辑一致性，为开放式任务的RL对齐提供了新范式。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文聚焦生成任务（creative writing），与用户画像中 generation 方向（权重0.1）直接相关。

## 基本信息

- 作者：Wenxuan Jiang, Zining Fan, Zijian Zhang, Xuecheng Wu, Hongming Tan, Haoyang Dai, Xiaoyu Li, Xuezhi Cao, Ninghao Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25757v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（摘要、引言、结论、限制等字段的语义匹配片段）。
