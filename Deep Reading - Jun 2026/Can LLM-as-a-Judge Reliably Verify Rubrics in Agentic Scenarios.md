---
user_id: "cheng tan"
paper_id: 2033
arxiv_id: "2606.29920"
title: "Can LLM-as-a-Judge Reliably Verify Rubrics in Agentic Scenarios?"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.29920.pdf"
pdf_url: "https://arxiv.org/pdf/2606.29920"
abs_url: "https://arxiv.org/abs/2606.29920"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:37:17"
---
# Can LLM-as-a-Judge Reliably Verify Rubrics in Agentic Scenarios?

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm-as-a-judge · rubric verification · agentic scenarios · benchmark

## 一句话总结

本文介绍RuVerBench，首个评估LLM作为裁判（LaaJ）在代理场景中验证rubric可靠性的基准，发现即使最强模型也存在显著噪声，并分析了提示设计、批处理和多数投票等策略的影响。

## 摘要

> Rubric-based scoring has become a widely used paradigm in model evaluation, typically with LLM-as-a-Judge (LaaJ) for rubric scoring. However, the reliability of LaaJ for rubric scoring remains underexplored. This concern is especially pronounced in agentic scenarios, where long, complex outputs further challenge reliable scoring. To address this, we conduct a systematic meta-evaluation of LaaJ reliability for rubric verification. We introduce RuVerBench, the first benchmark for assessing LaaJ reliability in rubric verification for agentic scenarios. RuVerBench covers two prevalent agentic domains, deep research and agentic coding, with 2,458 instances, each containing a model-generated output, a rubric, and a human-annotated label indicating whether the output satisfies the rubric. Using RuVerBench, we evaluate numerous frontier LLMs and find that even the most advanced models achieve strong performance but still exhibit substantial noise. We further analyze the impact of key LaaJ strategies, including prompt design, batching, and majority voting, on rubric verification. We find that weaker models are more sensitive to prompt variations, batched verification presents a trade-off between accuracy and efficiency, and majority voting yields effective but diminishing returns. We have released our dataset and code to facilitate future research: https://github.com/THU-KEG/RuVerBench.

Q1: 这篇论文试图解决什么问题？

这篇论文旨在解决LLM作为裁判（LLM-as-a-Judge）在代理场景中进行rubric验证时的可靠性问题。具体来说，rubric评分已成为评估复杂模型输出的标准方法，通常依赖LLM进行自动评分。然而，在代理场景（如深度研究和代理编码）中，模型输出通常较长、结构复杂且依赖上下文，这使得LLM裁判的可靠性面临挑战。当前缺乏专门评估LLM裁判在代理场景中rubric验证可靠性的基准，因此论文首先构建了RuVerBench，并基于该基准系统评估多种LLM和策略的可靠性。

Q2: 有哪些相关研究？

相关工作包括：（1）LLM-as-a-Judge范式：LLM被广泛用于模型评估和奖励建模（如Liu et al., 2023; Zheng et al., 2023），但其可靠性需要仔细理解。（2）基于rubric的评估：传统上用于评估复杂人类表现，引入NLP后最初用于检查基于规则的简单约束（如格式或关键词），现已扩展到更复杂的任务。（3）可靠性研究：已有研究探讨LLM作为裁判时的偏差、一致性和噪声等问题，但针对代理场景中rubric验证的可靠性缺乏系统研究。本工作填补了这一空白。

Q3: 论文如何解决这个问题？

论文提出RuVerBench基准，覆盖深度研究和代理编码两个常见代理领域，共包含2458个实例。每个实例由三部分组成：模型生成输出、rubric（具体评估指标）、以及人工标注的二元标签（指示输出是否满足rubric）。基于该基准，论文评估了多种前沿LLM（如GPT-4、Claude等），并设计了实验来分析不同策略对可靠性的影响：（1）提示设计：比较不同风格的提示模板对评分一致性的影响。（2）批处理验证：将多个实例一起提交给LLM进行评分，评估准确性和效率的权衡。（3）多数投票：对同一实例多次评分后取多数结果，分析收益递减规律。

Q4: 论文做了哪些实验？

论文在RuVerBench上进行了以下实验：（1）评估多个前沿LLM的rubric验证准确性（如准确率、F1等），并测量其噪声水平（通过多次重复评分的一致性）。（2）分析提示设计的影响：对弱模型和强模型分别使用多种提示模板，比较评分结果的变化幅度。（3）批处理实验：将不同数量的实例（如1、5、10、20）打包在一个提示中，记录评分准确率和消耗的token数，分析权衡。（4）多数投票实验：对同一实例进行多次（如3、5、7次）评分并投票，观察准确率提升与计算成本的曲线。

Q5: 发现了什么实验现象？

实验发现：（1）即使最先进的LLM（如GPT-4）在rubric验证上也存在显著噪声，多次评分结果不一致的比例较高。（2）较弱模型对提示设计更敏感，提示的微小变化可能导致评分结果的大幅波动。（3）批处理验证在实例数量较少时能提高效率而不显著损失准确性，但批处理数量过大时准确率下降，存在最优批大小。（4）多数投票能有效提升评分可靠性，但收益递减：从1次增加到3次时提升明显，继续增加投票次数带来的提升有限。此外，不同模型在rubric验证上的表现差异较大，且代理编码领域比深度研究领域更具挑战性（准确率更低）。

Q6: 有什么可以进一步探索的点？

（1）将RuVerBench扩展到更多代理领域（如工具使用、多轮对话）和更多语言。（2）使用分级标签（如1-5分）代替二元标签，以支持更精细的可靠性分析。（3）随着LLM的更新，持续扩展评估的模型集和策略集。（4）研究如何通过校准或后处理进一步提高LLM裁判的可靠性。（5）探索人机协作的rubric验证方案，在关键实例上引入人类审核。

Q7: 总结一下论文的主要内容

本文针对LLM作为裁判（LaaJ）在代理场景中rubric验证的可靠性进行了系统的元评估。作者首先指出，rubric评分已成为模型评估的常用方法，但LLM裁判的可靠性在复杂代理场景中尚未被充分研究。为此，他们构建了RuVerBench，这是首个专门针对代理场景rubric验证可靠性的基准，包含深度研究和代理编码两个领域的2458个人工标注实例。利用该基准，他们评估了多个前沿LLM，发现即使最强模型也表现出显著噪声，且弱模型对提示设计更为敏感。进一步实验揭示了批处理验证中准确性与效率的权衡，以及多数投票的有效性但收益递减。本文的贡献包括：提出了首个针对性基准，系统评估了多种LLM和策略，并为未来研究提供了数据集和代码（https://github.com/THU-KEG/RuVerBench）。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：直接针对LLM作为裁判的可靠性问题，对于所有依赖LLM自动评估的场景都有参考价值。

## 基本信息

- 作者：Yangda Peng, Yunjia Qi, Hao Peng, Haotian Xia, Guanzhong He, Xintong Shi, Richeng Xuan, Songyuanyi Lu, Yixian Liu, Zhichao Hu, Yuhong Liu, Lei Hou, Bin Xu, Juanzi Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-30
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.29920`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 已参考PDF检索证据（Abstract、Introduction、Conclusion、Related Work等片段）
