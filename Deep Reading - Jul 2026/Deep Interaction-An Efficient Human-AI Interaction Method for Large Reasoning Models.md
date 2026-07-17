---
user_id: "cheng tan"
paper_id: 4071
arxiv_id: "2607.14049"
title: "Deep Interaction: An Efficient Human-AI Interaction Method for Large Reasoning Models"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.14049.pdf"
pdf_url: "https://arxiv.org/pdf/2607.14049"
abs_url: "https://arxiv.org/abs/2607.14049"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-17T01:02:50"
---
# Deep Interaction: An Efficient Human-AI Interaction Method for Large Reasoning Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：deep interaction · human-ai interaction · chain-of-thought reasoning · error correction

## 一句话总结

提出Deep Interaction方法，允许用户直接编辑LLM的CoT推理错误，通过蒸馏修正后的推理路径引导模型，在STEM任务上校正成功率提升25%以上，Token消耗降低约40%。

## 摘要

> The emergence of Chain-of-Thought (CoT) reasoning has significantly enhanced the ability of large language models (LLMs) to tackle complex, multi-step tasks. However, when errors occur, current interaction approaches typically involve re-generating another response that may make mistakes again, or users laboriously flag the faulty step in follow-up turns that may get responses You are right, I made a mistake here followed by similar errors recurring. To address this issue, we propose an efficient human intervention mechanism for precisely correcting reasoning errors in LLMs, termed Deep Interaction. Our approach enables direct editing of the original response, allowing erroneous parts to be corrected while preserving accurate reasoning steps. We refine the edited CoT into a distilled prompt, which then steers the LLM along the corrected reasoning path. Experimental results show that our method achieves over a 25% improvement in correction success rate and reduces token usage by approximately 40% on STEM tasks reasoning compared to baseline approaches.

Q1: 这篇论文试图解决什么问题？

当前人机交互中，当LLM的CoT推理出现错误时，常见做法是让模型重新生成完整回答（可能再次犯错），或由用户在多轮对话中逐步指出错误步骤，但模型常会承认错误后仍重复类似错误。缺乏对推理链中错误步骤的精确、高效的干预手段，导致用户修正负担重且修正不稳定。

Q2: 有哪些相关研究？

相关研究包括：(1) 基于CoT的推理增强方法，如Few-shot CoT、Self-Consistency等，但均未涉及交互式修正；(2) 人机交互式纠错，如对话式修正、用户标记错误位置，但多轮对话效率低且模型易反复出错；(3) 提示工程方法，如通过提示引导模型修正，但难以定位具体错误步骤；(4) 模型编辑方法，如直接修改模型参数，但成本高且可能影响模型通用能力。

Q3: 论文如何解决这个问题？

Deep Interaction包含两个核心步骤：(1) 直接编辑：用户直接在大语言模型生成的原始CoT回答上修改错误部分（如替换或删除错误推理步骤），保持正确步骤不变；(2) 蒸馏提示：将编辑后的修正CoT提炼为简洁的distilled prompt，该提示保留了修正后的推理链关键信息，然后用于引导LLM重新推理生成最终答案。该方法可视为一种在推理时进行的人机协同微调。

Q4: 论文做了哪些实验？

论文在STEM任务（可能包括数学、物理、编程等需要多步推理的问题）上评估Deep Interaction。基线方法包括直接重新生成、多轮对话纠错等。衡量指标包括纠正成功率（correct success rate）和token使用量。实验设置可能涉及不同难度级别的题目，以及不同回合数历史的消融。

Q5: 发现了什么实验现象？

(1) Deep Interaction的纠正成功率比基线提升超过25%；(2) token使用量降低约40%；(3) 消融实验表明，简单地提供多轮历史对话（如3轮）并不足以有效纠正错误，反而可能因上下文干扰而降低效果；(4) 案例研究展示了编辑前后CoT的变化，蒸馏提示能有效保留修正逻辑。

Q6: 有什么可以进一步探索的点？

未来可探索的方向包括：(1) 将方法扩展到更多类型任务（如开放域生成、长文本推理等）；(2) 开发智能辅助编辑界面，降低用户编辑门槛；(3) 研究自动检测错误步骤并建议编辑；(4) 结合强化学习从用户编辑中学习，进一步减少专家干预；(5) 在不同规模模型和推理架构（如树搜索）上验证方法的泛化性。

Q7: 总结一下论文的主要内容

本文提出Deep Interaction，一种新型人机交互方法，旨在高效修正大语言模型在链式推理（CoT）中的错误。论文首先指出了当前CoT推理中错误修正的痛点：重新生成可能重复犯错，多轮对话纠错效率低且不稳定。为解决此问题，Deep Interaction允许用户直接编辑模型的原始回答，修正错误推理步骤，然后将修正后的CoT蒸馏成精炼提示（distilled prompt），再利用该提示引导模型沿正确路径推理并生成最终答案。该方法避免了重新生成整个回答的开销，同时保留了用户修正的精确性。在STEM推理任务上的实验结果显示，相比基线方法（如重新生成、多轮对话修正等），Deep Interaction的纠正成功率提高了超过25%，并减少了约40%的token消耗。此外，消融研究揭示，仅增加历史对话轮次并不能有效提升纠正效果，从而凸显了直接编辑+蒸馏提示策略的优势。论文还提供了案例研究，直观展示了编辑前后CoT的变化。总体而言，Deep Interaction为高层级人机协同推理提供了高效、轻量的解决方案。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该方法与智能体（agent）系统中人机协作推理高度相关，可用于构建可交互纠错的推理管道

## 基本信息

- 作者：Hefeng Zhou, Jinxuan Zhang, Jiong Lou, Yuxin Liu, Chaochao Lu, Jingjing Qu, Jie Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.14049`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成综合参考了PDF检索证据（摘要、引言、消融细节、案例研究、结论）以及摘要信息，对缺失部分进行了合理推断。
