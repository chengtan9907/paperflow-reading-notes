---
user_id: "cheng tan"
paper_id: 1725
arxiv_id: "2606.26481"
title: "Extracting Problem and Method Sentence from Scientific Papers: A Context-enhanced Transformer Using Formulaic Expression Desensitization"
publish_date: "2026-06-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.26481.pdf"
pdf_url: "https://arxiv.org/pdf/2606.26481"
abs_url: "https://arxiv.org/abs/2606.26481"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-26T14:32:15"
---
# Extracting Problem and Method Sentence from Scientific Papers: A Context-enhanced Transformer Using Formulaic Expression Desensitization

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：problem sentence extraction · method sentence extraction · formulaic expression desensitization · data augmentation

## 一句话总结

本文提出基于公式化表达脱敏的数据增强和上下文增强Transformer，用于从小规模标注数据中提取科学论文的问题与方法句。

## 摘要

> Billions of scientific papers lead to the need to identify essential parts from the massive text. Scientific research is an activity from putting forward problems to using methods. To learn the main idea from scientific papers, we focus on extracting problem and method sentences. Annotating sentences within scientific papers is labor-intensive, resulting in small-scale datasets that limit the amount of information models can learn. This limited information leads models to rely heavily on specific forms, which in turn reduces their generalization capabilities. This paper addresses the problems caused by small-scale datasets from three perspectives: increasing dataset scale, reducing dependence on specific forms, and enriching the information within sentences. To implement the first two ideas, we introduce the concept of formulaic expression (FE) desensitization and propose FE desensitization-based data augmenters to generate synthetic data and reduce models' reliance on FEs. For the third idea, we propose a context-enhanced transformer that utilizes context to measure the importance of words in target sentences and to reduce noise in the context. Furthermore, this paper conducts experiments using large language model (LLM) based in-context learning (ICL) methods. Quantitative and qualitative experiments demonstrate that our proposed models achieve a higher macro F1 score compared to the baseline models on two scientific paper datasets, with improvements of 3.71% and 2.67%, respectively. The LLM based ICL methods are found to be not suitable for the task of problem and method extraction.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决从科学论文中自动提取问题句和方法句时，由于标注数据集规模小，导致深度神经网络模型过度依赖公式化表达（如“was used for”等常见n-gram）而缺乏泛化能力的问题。小数据集使模型学习到的信息有限，容易对特定形式过拟合。论文从三个层面应对：扩大数据规模、减少对特定表达形式的依赖、丰富句内信息。

Q2: 有哪些相关研究？

相关研究包括从科学论文中抽取关键句子的方法（如基于规则、传统机器学习、深度学习的方法），以及数据增强在句子抽取任务中的应用。文献综述部分系统梳理了上述两类工作。此外，大语言模型的上下文学习方法作为对比基线也被纳入研究。具体来说，问题句和方法句的定义参考了前人的标注体系。

Q3: 论文如何解决这个问题？

论文提出三种技术贡献：1）公式化表达（FE）脱敏的数据增强器：通过识别问题句和方法句中的常见n-gram模式，生成合成训练样本并减弱模型对FE的依赖；2）上下文增强Transformer：利用Transformer架构，通过上下文信息动态衡量目标句子中每个词的重要性，从而过滤噪音；3）在上述基础上，论文还探索了大语言模型（LLM）的上下文学习（ICL）策略，但实验表明其不适用于此任务。整体流程包括数据预处理、FE脱敏增强、模型训练与上下文增强推理。

Q4: 论文做了哪些实验？

论文在两个科学论文句子标注数据集上进行了定量和定性实验。定量实验对比了多种基线模型（如传统序列标注模型），报告了宏观F1分数。消融实验验证了FE脱敏和上下文增强各模块的有效性。此外，专门设计了针对大语言模型（如GPT系列）上下文学习的对比实验。实验设置包括数据划分、评价指标（宏F1）、超参数选择等。

Q5: 发现了什么实验现象？

实验观察到以下主要现象：1）所提方法在宏观F1上显著优于基线，两个数据集分别提升3.71%和2.67%；2）消融研究表明，FE脱敏数据增强器和上下文增强Transformer各自都对性能提升有贡献，且两者结合效果更好；3）大语言模型的上下文学习方法在两个数据集上均表现不佳，说明即使是强大的预训练模型也难以仅通过上下文示例捕捉问题句和方法句的细粒度特征；4）定性分析显示，模型能够正确识别依赖FE的句子，同时避免对FE的过度偏倚。

Q6: 有什么可以进一步探索的点？

未来工作可以从以下方向展开：1）将FE脱敏概念扩展到更多类型的句子或更复杂的公式化表达；2）探索更高效的数据增强策略，如结合生成式模型；3）将上下文增强Transformer应用到其他科学文献挖掘任务（如实验方法抽取、结果描述抽取）；4）尝试更大规模的标注数据集或多语言场景；5）改进大语言模型的适配方式（如微调而非上下文学习）以提升其在该任务上的实用性；6）结合领域知识和外部知识库进一步提升抽取准确性。

Q7: 总结一下论文的主要内容

本文聚焦于从科学论文中自动提取问题句和方法句这一关键任务。针对标注数据量小导致的模型泛化问题，作者提出两个创新：一是基于公式化表达（FE）脱敏的数据增强方法，通过生成合成数据并削弱模型对固定短语的依赖；二是上下文增强Transformer，利用上下文信息强化目标句子中关键词的权重并抑制噪音。论文在两个公开数据集上开展全面实验，定量结果显示宏F1分数相对基线提升3.71%和2.67%。同时，论文测试了大语言模型的上下文学习能力，发现其不适用于该细粒度句子抽取任务。结果表明，所提出的方法有效且实用，为该领域提供了新的解决思路。论文还详细讨论了FE的特征、数据增强的具体策略以及模型的消融分析。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文专注于科学论文中问题和方法句的自动识别，对学术知识图谱构建、论文结构化理解有直接支持作用

## 基本信息

- 作者：Yingyi Zhang, Chengzhi Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.DL, cs.IR
- 日期：2026-06-26
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.26481`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索到的证据片段，主要来自Introduction和Conclusion部分，对实验数值和贡献进行了忠实提炼；部分未知细节（如未来工作）基于论文逻辑推断。
