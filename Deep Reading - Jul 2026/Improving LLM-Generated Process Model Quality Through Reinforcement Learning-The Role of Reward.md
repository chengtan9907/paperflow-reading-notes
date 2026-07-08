---
user_id: "cheng tan"
paper_id: 2937
arxiv_id: "2607.06175"
title: "Improving LLM-Generated Process Model Quality Through Reinforcement Learning: The Role of Reward Function Design"
publish_date: "2026-07-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.06175.pdf"
pdf_url: "https://arxiv.org/pdf/2607.06175"
abs_url: "https://arxiv.org/abs/2607.06175"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-09T00:24:17"
---
# Improving LLM-Generated Process Model Quality Through Reinforcement Learning: The Role of Reward Function Design

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · reward function design · process model generation · BPMN

## 一句话总结

系统性研究了奖励函数设计对强化学习优化LLM生成BPMN过程模型质量的影响，发现等权重奖励普遍优于针对特定维度的加权，且设计选择与模型架构存在显著交互。

## 摘要

> Large language models (LLMs) can generate BPMN process models from natural-language descriptions, yet supervised fine-tuning (SFT) limits their output quality to the patterns present in the training data. Reinforcement learning (RL) can optimize beyond this ceiling using external quality measures, but how the reward function should be designed when quality is multi-dimensional remains unexplored. We present a systematic investigation of reward function design for RL-based process model generation, training two LLM families (Llama~3.1 8B, Qwen~2.5 14B) under 48 configurations using Group Sequence Policy Optimization with rewards derived from an automated evaluation framework comprising 38 metrics across syntactic, pragmatic, and semantic quality. Three findings emerge. First, RL significantly improves pragmatic and syntactic quality while preserving semantic fidelity, reducing output variability by more than sixfold. Second, equal reward weighting consistently outperforms targeted weighting: emphasizing a specific dimension fails to improve it and can collapse the model into a low-quality mode. Third, design choices interact with model architecture in non-trivial ways: the invalidity penalty is essential for one model but irrelevant for the other, and SFT initialization is indispensable for one architecture but counterproductive for another. These results demonstrate that reward composition is a primary determinant of optimization outcomes, with effects as large as the decision to apply RL itself. The findings generalize to any structured generation task where quality is assessed along multiple automated dimensions. We release our implementation and experimental code at https://github.com/chlauer99/RL_for_process_modeling.

Q1: 这篇论文试图解决什么问题？

LLM能够根据自然语言描述生成BPMN过程模型，但监督微调（SFT）只能将输出质量限制在训练数据中的模式内。强化学习（RL）可以利用外部质量度量优化超越此上限。然而，当质量是多维度时（例如语法、实用、语义），如何设计奖励函数尚未被探索。现有工作通常使用单一奖励或简单组合，缺乏对不同加权策略和组件效果的系统的理解。本文旨在填补这一空白，通过大量实验揭示奖励函数设计中的权衡和交互。

Q2: 有哪些相关研究？

相关工作包括：（1）利用LLM生成过程模型的研究，如使用SFT微调预训练语言模型生成BPMN模型；（2）将RL应用于结构化生成任务，例如代码生成、分子生成等；（3）多维度质量评估框架，如BEF4LLM，该框架包含38个指标覆盖语法、实用和语义三个维度。本文与这些工作的区别在于系统性地专注于奖励函数设计本身，而非仅将RL作为黑盒优化工具，并且通过控制实验分离了不同设计选择的影响。

Q3: 论文如何解决这个问题？

论文提出了一套完整的训练框架用于将GSPO应用于过程模型生成。具体包括：（1）将输入自然语言描述转换为BPMN模型的任务形式化；（2）使用BEF4LLM框架的38个指标对生成的模型进行自动评估，从中衍生出奖励信号；（3）采用Group Sequence Policy Optimization（GSPO）作为RL算法，比较不同的奖励加权策略（包括等权重、针对语法/实用/语义的加权等），以及不同的初始化方式（从SFT checkpoint或从基础模型直接RL）；（4）对Llama 3.1 8B和Qwen 2.5 14B两个模型家族进行实验，每种模型在24种配置下训练，总共48个实验。还引入无效惩罚机制以处理生成非法BPMN的情况。实验设计系统覆盖了奖励组成、模型架构和初始化策略三个因素。

Q4: 论文做了哪些实验？

论文进行了大规模实验，包括：（1）两个模型家族：Llama 3.1 8B 和 Qwen 2.5 14B；（2）两种初始化策略：从SFT checkpoint开始RL，或从基础模型直接RL；（3）多种奖励加权方案：等权重、针对语法维度加权、针对实用维度加权、针对语义维度加权、以及额外引入无效惩罚；（4）总共48个实验配置。评估使用BEF4LLM框架的38个指标，覆盖语法、实用和语义质量。还分析了输出方差、不同维度间的权衡等。实验包括了与SFT基线的比较，以及消融研究。

Q5: 发现了什么实验现象？

实验揭示的主要现象包括：（1）RL能显著提升语法和实用质量，同时保持语义保真度，输出变异性降低超过六倍；（2）等权重奖励（即所有维度权重相同）一致优于任何针对特定维度加权的策略。当试图通过加权强调某一维度时，该维度并未改善，反而可能导致模型在其他维度崩溃，陷入低质量模式；（3）设计选择与模型架构存在重要交互：无效惩罚在Llama模型上至关重要，对避免产生非法模型非常有效，但在Qwen模型上影响微弱；SFT初始化对于Llama模型是必要的，但对于Qwen模型反而有负面效果；（4）奖励函数组成的改变产生的效果与是否使用RL本身一样大，强调了设计细节的重要性。另外还发现，某些配置下模型可能收敛到奖励欺骗但实际质量低的局部最优。

Q6: 有什么可以进一步探索的点？

基于本文发现，未来可探索的方向包括：（1）研究更复杂的奖励组合策略，如动态调整权重或使用权重网络进行自动学习；（2）将研究扩展到更大的模型家族（如70B参数）和更多样化的结构化生成任务（如代码、SQL、分子图等）；（3）探索使用过程奖励或迭代奖励更新以替代单一的最终奖励；（4）结合人类反馈（RLHF）或AI反馈（RLAIF）的奖励设计；（5）分析奖励函数与模型架构的相互作用机制，为不同模型提供设计指南；（6）研究在等权重基础上加入目标约束时的帕累托最优前沿。

Q7: 总结一下论文的主要内容

本文系统研究了在多维度质量评估下，如何设计强化学习奖励函数以优化LLM生成的BPMN过程模型质量。作者采用Group Sequence Policy Optimization算法，基于BEF4LLM自动评估框架的38个指标构建奖励信号，在Llama 3.1 8B和Qwen 2.5 14B两个模型家族上进行了48种不同配置的实验。研究发现：第一，强化学习能显著提升模型的实用和语法质量，同时保持语义一致性，并将输出变异性降低六倍以上；第二，等权重奖励策略在所有实验设置中均优于针对特定维度加权的策略，表明强调某一维度不仅无法提升该维度，反而可能导致整体质量崩溃；第三，奖励函数设计的影响与模型架构选择之间存在重要交互，例如无效惩罚对Llama模型关键但对Qwen模型无关，SFT初始化对Llama必不可少但对Qwen反而有害。这些发现表明，奖励函数的具体组成是实现优化效果的关键决定因素，其影响程度与是否使用强化学习本身相当。论文还为结构化生成任务中的RL应用提供了实用指南，并指出了未来研究方向。论文代码已开源。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：涉及使用强化学习优化大模型生成结构输出，与生成任务直接相关

## 基本信息

- 作者：Alexander Rombach, Chantale Lauer, Nijat Mehdiyev
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-07-08
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.06175`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据片段，包括abstract、methodology、results等，并优先采用了field_evidence_map中对应的证据锚点。
