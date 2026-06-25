---
user_id: "cheng tan"
paper_id: 1168
arxiv_id: "2606.20814"
title: "What Shapes Emergent Misalignment? Insights from Training Dynamics, Model Priors, and Data"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.20814.pdf"
pdf_url: "https://arxiv.org/pdf/2606.20814"
abs_url: "https://arxiv.org/abs/2606.20814"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T01:49:45"
---
# What Shapes Emergent Misalignment? Insights from Training Dynamics, Model Priors, and Data

> ★★★☆☆ 值得快速浏览 · 模型 heuristic/PaperFlow template

🏷 关键词：narrow fine-tuning · alignment scores · after narrow · evaluation prompts · training evaluation · activations · overlap · misalignment

## 一句话总结

We study EM and its variability directly through the components of fine-tuning: training dynamics, model priors, and data.

## 摘要

> Emergent misalignment (EM) is a phenomenon in which models generalize with narrow fine-tuning, leading to broad (yet uneven) misalignment across evaluation questions. We study EM and its variability directly through the components of fine-tuning: training dynamics, model priors, and data. (1) We first explored how in-domain training loss relates to out-of-domain alignment scores across datasets and model families. Then, we tried to induce potential alternative local minima through different learning schedules for one narrow fine-tuning, but did not find strong runs with better broad alignment scores conditioned on similar or lower training loss. (2) We found that although the mean and standard deviations of the misaligned model scores are usually statistically different from those of the pre-trained model, there are some potential signals on overall positive correlation. The evaluation prompt-only activations from both the pre-trained and the original instruct models (prior to narrow fine-tuning) could predict fine-grained alignment scores after narrow fine-tuning. (3) Finally, we compared activation deltas before and after narrow fine-tuning and found moderate-to-high subspace overlap and similarity between the resulting activation shifts for training and evaluation prompts. Subspace overlaps between training and evaluation prompt activations correlate with their shifts' similarities when measuring with the last prompt-token activations. The train-evaluation data prompt overlap is controlled against overlap computed from random vectors and evaluation prompts activations. Code is available at https://github.com/aisa-group/em-generalization.

Q1: 这篇论文试图解决什么问题？

Emergent misalignment (EM) is a phenomenon in which models generalize with narrow fine-tuning, leading to broad (yet uneven) misalignment across evaluation questions. We study EM and its variability directly through the components of fine-tuning: training dynamics, model priors, and data.

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：directly: (1) training dynamics, (2) model “priors”, and (3) data.；We explain each angle in more detail below, connecting related works and our proposals.；ing learning schedules for one narrow fine-tuning, we did not find meaningful local minima that yielded significantly better alignment scores at comparable or lower training loss.。

Q3: 论文如何解决这个问题？

directly: (1) training dynamics, (2) model “priors”, and (3) data. We explain each angle in more detail below, connecting related works and our proposals.

Q4: 论文做了哪些实验？

ing learning schedules for one narrow fine-tuning, we did not find meaningful local minima that yielded significantly better alignment scores at comparable or lower training loss. \- Model “priors”: Our comparisons found that while most misaligned models have statistically different mean and standard deviations from the pre-trained model, there may be some…

Q5: 发现了什么实验现象？

ing learning schedules for one narrow fine-tuning, we did not find meaningful local minima that yielded significantly better alignment scores at comparable or lower training loss. \- Model “priors”: Our comparisons found that while most misaligned models have statistically different mean and standard deviations from the pre-trained model, there may be some…

Q6: 有什么可以进一步探索的点？

- To this end, rather than focusing on a particular human-defined notion of harmfulness, we aim to study how narrow fine-tuning induces overall shifts in model behavior.
- We compare the train and the evaluation activation deltas of the prompts activations before and after narrow fine-tuning.
- 优先核对语义命中的全文片段：Abstract。

Q7: 总结一下论文的主要内容

We study EM and its variability directly through the components of fine-tuning: training dynamics, model priors, and data.

Emergent misalignment (EM) is a phenomenon in which models generalize with narrow fine-tuning, leading to broad (yet uneven) misalignment across evaluation questions. We study EM and its variability directly through the components of fine-tuning: training dynamics, model priors, and data.

directly: (1) training dynamics, (2) model “priors”, and (3) data. We explain each angle in more detail below, connecting related works and our proposals.

ing learning schedules for one narrow fine-tuning, we did not find meaningful local minima that yielded significantly better alignment scores at comparable or lower training loss. \- Model “priors”: Our comparisons found that while most misaligned models have statistically different mean and standard deviations from the pre-trained model, there may be some…

主要贡献包括：directly: (1) training dynamics, (2) model “priors”, and (3) data.；We explain each angle in more detail below, connecting related works and our proposals.；ing learning schedules for one narrow fine-tuning, we did not find meaningful local minima that yielded significantly better alignment scores at comparable or lower training loss.。

需要注意的边界包括：To this end, rather than focusing on a particular human-defined notion of harmfulness, we aim to study how narrow fine-tuning induces overall shifts in model behavior.；We compare the train and the evaluation activation deltas of the prompts activations before and after narrow fine-tuning.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Abstract 部分。；它不一定和你当前最核心的 智能体 完全同题，但方法设计和评测组织值得借鉴。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Abstract 部分。

## 基本信息

- 作者：Yuchen Zhang, Anietta Weckauff, Diego Garcia-Olano, Maksym Andriushchenko
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG, stat.ML
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2606.20814`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
