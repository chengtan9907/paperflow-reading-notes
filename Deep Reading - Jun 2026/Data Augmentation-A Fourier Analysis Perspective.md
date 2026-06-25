---
user_id: "cheng tan"
paper_id: 1501
arxiv_id: "2606.24418v1"
title: "Data Augmentation: A Fourier Analysis Perspective"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.24418v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.24418v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T18:27:31"
---
# Data Augmentation: A Fourier Analysis Perspective

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：data augmentation · invariance · symmetry · sample complexity

## 一句话总结

本文研究部分数据增强是否能在统计上达到与完整增强相同效益，并利用傅里叶分析和表示论证明随机采样子集增强可达到相同极小化最优速率，但精确不变性需要全群平均。

## 摘要

> Data augmentation is a simple and model-agnostic approach for exploiting known invariances in learning problems. Given a group acting on the input space, one augments the training set with transformed copies of each sample. Because it exploits symmetries without modifying the underlying learning algorithm, data augmentation can be applied broadly across learning methods. However, this universality comes at a computational cost: when the group is large, full group-sized augmentation quickly becomes computationally infeasible. This raises a fundamental question: Can partial data augmentation achieve the same statistical benefits as full augmentation in terms of generalization and sample complexity? We develop a general framework for investigating this question using Fourier analysis and the representation theory of finite groups. We show that, for a broad class of classical learning problems, partial data augmentation based on a randomly sampled subset of group elements achieves the same minimax rates as full augmentation, up to an approximation error that vanishes as the subset size increases. Our results provide a theoretical explanation for why partial augmentation can retain the statistical benefits of full augmentation despite enforcing symmetry only approximately, and shed light on a recently raised question in learning with symmetries (Díaz et al., 2025): whether statistically optimal learning under general group invariances can be achieved using computationally scalable methods. Moreover, we prove a complementary impossibility result: enforcing exact invariance via data augmentation requires averaging over the entire group, and cannot be achieved by any strict subset when the hypothesis space is sufficiently expressive. Together, these results provide a unified perspective on full and partial data augmentation, as well as exact and approximate symmetry enforcement.
> Keywords: data augmentation, invariance, symmetry, sample complexity, representation theory

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：在计算成本受限的情况下，部分数据增强（仅使用群元素的随机子集进行增强）能否在泛化性能和样本复杂度方面达到与完整数据增强（使用整个群）相同的统计效益？该问题源于实践中群的规模可能极大（如旋转群、排列群），导致完整增强计算不可行，但经验上部分增强往往仍有效。

Q2: 有哪些相关研究？

相关工作包括利用对称性的学习理论（如等变网络、群卷积）、数据增强的统计分析（如通过核方法或风险分解），以及Díaz等人（2025）提出的问题：是否可以用计算可扩展方法实现群不变性下的统计最优学习。本文基于表示论和傅里叶分析，对投影估计器（密度估计和回归）给出了肯定回答，并补充了精确不变性需要全群平均的边界结果。

Q3: 论文如何解决这个问题？

论文采用傅里叶分析和有限群表示论作为核心工具。考虑投影基估计器（如截断傅里叶级数），将数据增强视为对经验风险的群平均操作。通过将部分增强的估计误差分解为偏差和方差项，利用表示论将群平均转化为特征空间中的线性平均，证明随机采样子集的增强能够达到与完整增强相同的极小化最优速率，且误差随子集大小增大以O(1/|S|)速率消失（合理推断）。同时，通过反例证明当假设空间足够表达时，任何严格子集都无法实现精确不变性。

Q4: 论文做了哪些实验？

本文为纯理论工作，未设计实验验证。所有结论均通过数学定理证明。

Q5: 发现了什么实验现象？

无。

Q6: 有什么可以进一步探索的点？

未来可探索的方向包括：(1) 将分析推广到无限群或连续群（如旋转群、平移群）；(2) 研究深度神经网络等非参数模型在部分数据增强下的统计性质；(3) 将理论扩展到其他学习任务（如分类、决策）；(4) 探索近似不变性与精确不变性在其他约束条件下的权衡；(5) 结合计算可行性设计自适应子集选择策略。

Q7: 总结一下论文的主要内容

本文系统研究了有限群对称性下部分数据增强的统计性质。作者针对基于投影基的密度估计和回归问题，利用傅里叶分析和表示论建立了理论框架。主要发现：当使用随机采样的群元素子集进行数据增强时，估计器的极小化最优速率与使用整个群相同，近似误差随子集大小增大而消失。这一结果解释了实践中随机部分增强的广泛有效性。同时，论文证明了一个不可能性结果：若假设空间足够表达，精确不变性只能通过全群平均实现，任何部分增强都无法严格保证。这些结论统一了完全/部分数据增强、精确/近似对称性的理论理解，并为设计计算高效的对称性学习算法提供了指导。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：为数据增强的理论理解提供了新视角，解释了随机部分增强在实践中的有效性。

## 基本信息

- 作者：Behrooz Tahmasebi, Melanie Weber, Stefanie Jegelka
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, stat.ML
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.24418v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 已参考PDF检索证据，主要基于Abstract和结论部分生成，理论分析结论来自检索片段。
