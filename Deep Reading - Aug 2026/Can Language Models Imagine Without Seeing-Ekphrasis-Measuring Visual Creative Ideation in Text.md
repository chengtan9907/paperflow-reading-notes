---
user_id: "cheng tan"
paper_id: 7174
arxiv_id: "2608.06967"
title: "Can Language Models Imagine Without Seeing? Ekphrasis: Measuring Visual Creative Ideation in Text-Only LLMs"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.06967.pdf"
pdf_url: "https://arxiv.org/pdf/2608.06967"
abs_url: "https://arxiv.org/abs/2608.06967"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-11T01:11:01"
---
# Can Language Models Imagine Without Seeing? Ekphrasis: Measuring Visual Creative Ideation in Text-Only LLMs

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual creative ideation · language models · benchmark · novelty evaluation

## 一句话总结

本文提出视觉创造性思维（VCI）的概念并构建Ekphrasis基准，通过400个任务和14个语言模型的评估，发现实用性、表达性和新颖性三个维度可分离，且文本级VCI排序在跨模态渲染后仍能保持，从而为文本-only语言模型的视觉意念提供了一种可测量的操作性定义。

## 摘要

> Current evaluations do not isolate whether text-only language models can originate visual concepts before image generation. Fluent visual prose can hide visual-plan failures: an answer may appear creative while repeating familiar visual clichés or failing to specify a renderable scene. We define Visual Creative Ideation (VCI) as the ability to produce textual visual plans that are useful, expressive, and population-novel, and introduce Ekphrasis, a 400-task benchmark spanning Abstraction, Combination, Transformation, and Adaptation. Ekphrasis scores anonymized pairwise comparisons with dimension-specific checklists, aggregates preferences with Bradley-Terry models, and uses Typed Idea Graphs to convert task-specific population clichés into novelty references. Across 14 language models, VCI separates usefulness, expressiveness, and novelty rather than reducing to fluency: strong models achieve similar overall scores through different profiles, and useful plans can remain visually clichéd. A cross-modal grounding study further shows that text-level VCI ordering largely survives faithful rendering and blind image-level preference judgment, supporting Ekphrasis as a measure of visual ideation beyond prose quality.

Q1: 这篇论文试图解决什么问题？

该论文主要针对以下问题：（1）当前评估未能隔离文本-only语言模型在图像生成之前是否能产生真正的视觉概念；现有评估可能将语言流畅性误认为视觉想象力。（2）流畅的视觉散文可能隐藏视觉计划失败：模型可能输出表面合理但重复常见视觉陈词滥调，或忽略关键细节导致场景不可渲染。（3）缺乏在文本侧测量视觉创造性思维（VCI）的专门基准，尤其是在新颖性上缺乏基于群体的客观参考。因此，作者试图将VCI从一般文本创造力评估中剥离，并建立可操作、可验证的评估框架。

Q2: 有哪些相关研究？

相关研究可分为几类：一、语言模型创造力评估：已有基准大多评估语言产物、过程发明或联想距离，如发散思维任务（Alternative Uses、Divergent Association）探测原创性（Guilford, 1967; Olson等）。这些方法侧重于语言层面的发散性，而不涉及视觉概念的新颖性和可渲染性。二、视觉创意评估：图像生成或文本到图像模型通常关注生成质量或文本-图像对齐，但没有在生成前对文本计划进行视觉意念隔离。三、Ekphrasis在表1中占据视觉意念单元，输出是文本但目标构念是图像生成前的视觉概念创造。先前的基准覆盖相邻单元，但没有专门隔离文本侧视觉意念，且没有基于群体样本的新颖性锚定和跨模态效度证据。从检索片段看，相关工作还涉及程序发明和认知评估，但强调这些并不能直接回答文本-only模型是否具有视觉意念。

Q3: 论文如何解决这个问题？

论文的方法论包括四个核心部分：（1）定义VCI：将视觉创造性思维操作化为产生的文本视觉计划满足三个条件——对给定任务有用（useful）、表达力足以支撑具体图像（expressive）、相对任务特定语言模型群体的常见模式具有新颖性（novel）。（2）构建Ekphrasis基准：包含400个任务，分为抽象（Abstraction）、组合（Combination）、变换（Transformation）和适应（Adaptation）四类，覆盖不同类型的视觉意念挑战。（3）评分机制：采用匿名成对比较，结合维度特定的检查表（针对有用性、表达性和新颖性分别设计），通过Bradley-Terry模型聚合成对偏好，得到整体分数。（4）新颖性参考：利用类型化思想图（Typed Idea Graphs）将任务特定群体中的常见观点（即clichés）形式化，为计算新颖性提供参照。此外，论文还设计跨模态基础研究，将模型生成的文本计划进行忠实渲染，再让盲评者进行图像级偏好判断，以验证文本级VCI排序在视觉呈现后是否依然成立。

Q4: 论文做了哪些实验？

论文进行了两类主要实验：（1）主基准评估：在14个不同规模的语言模型上运行Ekphrasis的400个任务，使用匿名成对比较和维度特定检查表收集偏好，并通过Bradley-Terry模型计算每个模型在有用性、表达性、新颖性及整体VCI上的得分。（2）跨模态基础研究：选取部分模型的输出，通过忠实渲染生成对应图像，然后让评估者在图像级别进行盲偏好判断，检验文本级VCI排序是否能迁移到视觉呈现层面。摘要中还提到评分采用匿名化以减轻偏差，而检查表有助于维度分离。具体模型列表、渲染方法和评估细节未在检索片段中给出，需要参考原文。

Q5: 发现了什么实验现象？

主要实验现象包括：（1）VCI三个亚维度（有用性、表达性、新颖性）相互分离，而不是简化为单一流畅性因子；强模型可以在整体分数相近的情况下，呈现出不同的画像组合。（2）一个突出的反直觉发现是：有用的视觉计划仍可能高度视觉陈词滥调，说明有用性与新颖性并不必然耦合。（3）跨模态研究中，文本级VCI排序在忠实渲染和盲图像偏好判断中大体保持，说明该测量捕捉到了至少在某种程度上可转移到图像感知的成分，而非仅仅反映文笔质量。这些结果支持Ekphrasis作为一种独立测量视觉意念的工具。

Q6: 有什么可以进一步探索的点？

进一步探索的方向包括：（1）扩展任务分布与语言文化覆盖面，检验VCI定义的跨文化稳定性；（2）将Ekphrasis用于多模态模型，比较文本-only与图文模型的视觉意念差异；（3）改进Typed Idea Graphs以更细粒度建模群体cliché，或探索动态更新方法；（4）结合人类创造力研究，验证VCI与人类评分的相关性；（5）分析位置效应与长度效应的产生机制，并发展更稳健的评判协议；（6）将VCI与下游图像生成质量对接，研究视觉意念对生成结果的影响；（7）探索VCI对模型训练或提示设计的指导意义。

Q7: 总结一下论文的主要内容

该论文针对文本-only语言模型是否具备真正的视觉创造性思维这一核心问题展开研究。作者首先指出现有评估的缺陷：流畅的视觉散文可能掩盖视觉计划的失败，模型可能重复陈词滥调或生成不可渲染的场景。为此，他们提出了视觉创造性思维（VCI）的定义，强调有用性、表达性和群体新颖性三个维度，并基于创造力研究中的“原创性+有用性”标准。然后，他们构建了Ekphrasis基准，含400个任务，分属抽象、组合、变换、适应四个类别，采用匿名成对比较、维度特定检查表、Bradley-Terry偏好聚合和Typed Idea Graphs来评分和计算新颖性。在14个语言模型上的实验表明，VCI的三个维度是可分离的，强模型可以通过不同画像达到相似整体分数，且有用计划可能缺乏新颖性。进一步的跨模态基础研究通过对文本计划进行忠实渲染并进行图像级盲评，显示文本级VCI排序在跨模态后仍大体保持，从而提供了测量有效性的证据。论文的结论是Ekphrasis能够超越散文质量，测量视觉意念本身。该工作为评估语言模型的视觉想象能力提供了新工具，并揭示了文本流畅性与视觉创造性之间的解耦。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作直接属于生成与评测交叉方向，与用户画像中的“生成”方向相关。

## 基本信息

- 作者：Hongyu Luo, He Wang, Huihao Jing, Hong Ting Tsang, Yuxuan Liu, Wuganjing Song, Yauwai Yim, Chunyang Li, Yangqiu Song
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.06967`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告基于论文摘要及检索到的章节片段生成，未获得完整PDF正文，部分实验细节为合理推断或推测。
