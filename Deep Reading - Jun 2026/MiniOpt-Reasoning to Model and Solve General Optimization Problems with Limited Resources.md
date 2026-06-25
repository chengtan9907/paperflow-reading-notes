---
user_id: "cheng tan"
paper_id: 1413
arxiv_id: "2606.25832v1"
title: "MiniOpt: Reasoning to Model and Solve General Optimization Problems with Limited Resources"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25832v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25832v1"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T17:55:47"
---
# MiniOpt: Reasoning to Model and Solve General Optimization Problems with Limited Resources

> ★★★☆☆ 值得快速浏览 · 模型 heuristic/PaperFlow template

🏷 关键词：optimization generalization · optimization problems · reinforcement learning · strong optimization · generalization across · miniopt · optimization-oriented · reasoning

## 一句话总结

To address these challenges, we propose MiniOpt, a reinforcement learning framework that learns to solve optimization problems through an "reasoning-to-model-and-solve" paradigm.

## 摘要

> Achieving strong optimization generalization across diverse optimization problems while requiring limited training resources remains a challenging problem for optimization-oriented large language models (LLMs). Existing approaches typically rely on large-scale supervised datasets, costly reasoning annotations, and expensive intermediate step verification, resulting in substantial training overhead. To address these challenges, we propose MiniOpt, a reinforcement learning framework that learns to solve optimization problems through an "reasoning-to-model-and-solve" paradigm. MiniOpt decomposes optimization reasoning into structured optimization modeling and executable solver generation. Building upon this paradigm, we introduce OptReward, a reward function with hierarchical score structure that jointly evaluates formulation and solution, enabling effective policy learning without expert demonstrations. We further develop an optimization-oriented policy optimization strategy that improves exploration efficiency and stabilizes reinforcement learning for compact models. Extensive experiments show that MiniOpt-3B exhibits strong optimization generalization across various optimization types, problem scenarios, and task domains. For models with fewer than 10B parameters, MiniOpt series achieves the highest average solving accuracy (SA). For models with more than 10B parameters, MiniOpt still shows competitive performance. These results suggest that optimization-oriented reward design and reinforcement learning provide an effective pathway for developing compact optimization-specialized language models with strong optimization generalization capabilities. The code is available at https://github.com/Hsiang-1/MiniOpt.

Q1: 这篇论文试图解决什么问题？

Ke Zhao $^{*}$ , Zixiang Di $^{*}$ , Hong Qian, Xiang Shu, Yaolin Wen, Qitao Shi, Bingdong Li, Xingyu Lu, Xiangfeng Wang, Jun Zhou, Ke Tang, and Yang Yu Abstract—Achieving strong… Existing approaches typically rely on large-scale supervised datasets, costly reasoning annotations, and expensive intermediate step verification, resulting in substantial…

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：This paper proposes MiniOpt, a novel reasoning-to-model-and-solve paradigm and the corresponding small-scale models MiniOpt-3B and MiniOpt-7B.；The proposed models achieve competitive or even superior performance under limited computational resources.；We evaluate MiniOpt models on diverse optimization benchmarks spanning multiple types and scenarios to assess whether small-scale parameter LLMs (3B/7B) can achieve strong…。

Q3: 论文如何解决这个问题？

This paper proposes MiniOpt, a novel reasoning-to-model-and-solve paradigm and the corresponding small-scale models MiniOpt-3B and MiniOpt-7B. The proposed models achieve competitive or even superior performance under limited computational resources.

Q4: 论文做了哪些实验？

We evaluate MiniOpt models on diverse optimization benchmarks spanning multiple types and scenarios to assess whether small-scale parameter LLMs (3B/7B) can achieve strong… To what extent can MiniOpt at 3B/7B achieve high SA and ER across types and scenarios, and how does it compare with larger reasoning LLMs and prior learning-based approaches?

Q5: 发现了什么实验现象？

We evaluate MiniOpt models on diverse optimization benchmarks spanning multiple types and scenarios to assess whether small-scale parameter LLMs (3B/7B) can achieve strong… To what extent can MiniOpt at 3B/7B achieve high SA and ER across types and scenarios, and how does it compare with larger reasoning LLMs and prior learning-based approaches?

Q6: 有什么可以进一步探索的点？

- nd prior learning-based approaches?
- (Q2) Pareto Front of Performance vs.
- 优先核对语义命中的全文片段：References / V Discussions / C Training Pipeline Of Miniopt。

Q7: 总结一下论文的主要内容

To address these challenges, we propose MiniOpt, a reinforcement learning framework that learns to solve optimization problems through an "reasoning-to-model-and-solve" paradigm.

Ke Zhao $^{*}$ , Zixiang Di $^{*}$ , Hong Qian, Xiang Shu, Yaolin Wen, Qitao Shi, Bingdong Li, Xingyu Lu, Xiangfeng Wang, Jun Zhou, Ke Tang, and Yang Yu Abstract—Achieving strong… Existing approaches typically rely on large-scale supervised datasets, costly reasoning annotations, and expensive intermediate step verification, resulting in substantial…

This paper proposes MiniOpt, a novel reasoning-to-model-and-solve paradigm and the corresponding small-scale models MiniOpt-3B and MiniOpt-7B. The proposed models achieve competitive or even superior performance under limited computational resources.

We evaluate MiniOpt models on diverse optimization benchmarks spanning multiple types and scenarios to assess whether small-scale parameter LLMs (3B/7B) can achieve strong… To what extent can MiniOpt at 3B/7B achieve high SA and ER across types and scenarios, and how does it compare with larger reasoning LLMs and prior learning-based approaches?

主要贡献包括：This paper proposes MiniOpt, a novel reasoning-to-model-and-solve paradigm and the corresponding small-scale models MiniOpt-3B and MiniOpt-7B.；The proposed models achieve competitive or even superior performance under limited computational resources.；We evaluate MiniOpt models on diverse optimization benchmarks spanning multiple types and scenarios to assess whether small-scale parameter LLMs (3B/7B) can achieve strong…。

需要注意的边界包括：nd prior learning-based approaches?；(Q2) Pareto Front of Performance vs.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 References / V Discussions / C Training Pipeline Of Miniopt 部分。；这篇论文和你当前画像里的方向有直接重合：生成（权重 0.10）。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 References / V Discussions / C Training Pipeline Of Miniopt 部分。

## 基本信息

- 作者：Ke Zhao, Zixiang Di, Hong Qian, Xiang Shu, Yaolin Wen, Qitao Shi, Bingdong Li, Xingyu Lu, Xiangfeng Wang, Jun Zhou, Ke Tang, Yang Yu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2606.25832v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
