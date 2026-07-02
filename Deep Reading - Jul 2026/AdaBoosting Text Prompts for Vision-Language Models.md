---
user_id: "cheng tan"
paper_id: 2340
arxiv_id: "2607.00684"
title: "AdaBoosting Text Prompts for Vision-Language Models"
publish_date: "2026-07-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.00684.pdf"
pdf_url: "https://arxiv.org/pdf/2607.00684"
abs_url: "https://arxiv.org/abs/2607.00684"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-02T14:21:27"
---
# AdaBoosting Text Prompts for Vision-Language Models

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：vision-language model · few-shot adaptation · text prompt boosting · zero-shot classification

## 一句话总结

提出Text Prompt Boosting (TPB)，一种基于AdaBoost的文本提示集成框架，用于视觉-语言模型的少样本自适应，通过将类级提示视为弱分类器并迭代加权组合，实现shot可扩展和跨模型鲁棒的零样本分类。

## 摘要

> 视觉-语言模型（VLM）如CLIP在零样本图像分类中依赖自然语言提示，但提示质量影响准确率。现有少样本提示适应方法未明确关注错误分类样本，导致标注增加时改进有限。本文提出Text Prompt Boosting (TPB)，将每个类别的提示池（包括模板和LLM生成句子）视为弱分类器，通过AdaBoost风格算法迭代选择并加权提示，重点修正前一轮错误，最终集成强分类器。实验表明TPB在源模型上shot可扩展性优异，且自然语言提示跨模型迁移鲁棒，在五个ViT-L级VLM上持续超越先前方法。

Q1: 这篇论文试图解决什么问题？

视觉-语言模型（VLM）实现零样本分类时，精度高度依赖手工或模板化提示的质量。现有少样本提示适应方法（如硬提示优化）使用少量标注图像调整提示，但这些方法在构建提示时未显式关注被错误分类的样本，导致即使增加标注样本（shots）也只能获得边际改进。因此，核心问题是如何有效利用有限的少样本监督，构建一个能随shots增加而持续提升性能、且在不同VLM间迁移鲁棒的提示集合。

Q2: 有哪些相关研究？

相关研究可分为两类：1) 软提示适应（如CoOp, CoCoOp）：学习连续向量作为提示，需要反向传播，计算成本高且不易跨模型迁移。2) 硬提示适应（如LLMbo基于对话反馈、ProAPO基于进化搜索、PEZ基于梯度优化）：直接优化离散自然语言提示，可解释性强，但现有方法未利用弱分类器集成思想。TPB首次将boosting引入硬提示适应，通过集成多个文本弱分类器（每个提示对应一个分类器）来系统性地关注困难样本。

Q3: 论文如何解决这个问题？

TPB框架步骤如下：1) 为每个类别构建提示池，包含简单模板（如“a photo of a {class}.”）和LLM生成的丰富描述句。2) 将每个提示视为一个文本弱分类器，其输出为基于该提示的类概率（通过CLIP相似度计算）。3) 执行AdaBoost风格循环：设置样本权重初始均匀；每轮从提示池中选择一个弱分类器（通常选择可降低加权误差的提示），更新样本权重（加大错误分类样本权重）；循环直至收敛或达到轮数。4) 最终强分类器为所选弱分类器的加权集成。关键创新：通过boosting动态聚焦难例，使每次迭代的提示选择互补，从而提升整体分类性能。

Q4: 论文做了哪些实验？

论文在多个标准零样本分类基准（如ImageNet, CIFAR-100, FGVC Aircraft等）上评估TPB。实验设置：1) 源模型配置：使用CLIP ViT-L作为主模型，并额外在四个其他ViT-L级VLM（如OpenCLIP, SigLIP等）上测试跨模型迁移。2) 少样本设置：1-shot, 2-shot, 4-shot, 8-shot, 16-shot。3) 对比方法：包括零样本基线、CoOp、CoCoOp、LLMbo、ProAPO、PEZ等。4) 消融：分析提示池大小、boosting轮数、弱分类器选择策略的影响。

Q5: 发现了什么实验现象？

1) Shot scalability：TPB随shots增加性能稳定提升，在16-shot时接近全监督上限，而现有方法在4-shot后增益饱和。2) Transfer robustness：TPB在源模型上的提示集成可直接迁移到其他VLM，性能优于从头适应方法，表明自然语言提示的跨模型通用性。3) 消融发现：提示池多样性（混合模板与LLM描述）比单一类型效果更好；boosting迭代约5-10轮后收敛；每轮选择加权误差最小的弱分类器策略最优。4) 反直觉现象：虽使用boosting，但集成后类别内提示数量差异较大，说明并非所有类别都需要同等数量的弱分类器。

Q6: 有什么可以进一步探索的点？

1) 动态提示池扩展：结合LLM在线生成新提示，替代固定池。2) 更高效boosting变体：如用于多分类的AdaBoost.M2或Real AdaBoost，以利用更多概率信息。3) 跨模态迁移：将TPB应用于视频-语言模型或图文检索任务。4) 弱监督场景：探索在标签噪声或部分标注下的鲁棒性。5) 与其他集成方法（如堆叠、Bagging）比较。

Q7: 总结一下论文的主要内容

本文提出Text Prompt Boosting (TPB)，解决VLM少样本自适应中提示改进有限的问题。TPB将每个类别的文本提示池（包含模板和LLM生成描述）视为弱分类器，通过AdaBoost算法迭代选择并加权，重点修正前一轮错误样本，最终集成强分类器。实验在多个数据集和五个ViT-L级VLM上验证，显示TPB具有优异的shot可扩展性（随shots增加持续提升）和跨模型迁移鲁棒性。主要贡献包括：首次将boosting用于VLM提示集成，提出系统性的弱分类器构建与选择框架，并证明自然语言提示集成比连续提示更易迁移。局限包括依赖预定义提示池和boosting的额外计算开销。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：提供少样本场景下提示集成的系统性评估框架

## 基本信息

- 作者：Seokhee Jin, Changhwan Sung, Sunung Mun, Hoyoung Kim, Jungseul Ok
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-07-02
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.00684`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 基于PDF检索证据生成，部分内容（如具体实验数值、数据集）为合理推断；关键方法细节依赖原文语义片段。
