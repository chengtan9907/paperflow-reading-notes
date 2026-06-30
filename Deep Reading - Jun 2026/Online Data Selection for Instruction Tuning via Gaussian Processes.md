---
user_id: "cheng tan"
paper_id: 1926
arxiv_id: "2606.30077"
title: "Online Data Selection for Instruction Tuning via Gaussian Processes"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.30077.pdf"
pdf_url: "https://arxiv.org/pdf/2606.30077"
abs_url: "https://arxiv.org/abs/2606.30077"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:10:36"
---
# Online Data Selection for Instruction Tuning via Gaussian Processes

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：instruction tuning · data selection · gaussian process · online learning

## 一句话总结

提出GAIA框架，利用高斯过程回归进行全局数据质量估计，克服在线数据选择中批次约束导致的局部优化局限，实现自适应指令微调数据选择。

## 摘要

> With Large Language Model (LLM) pre-training and fine-tuning shifting its focus from data volume to data quality, quality data selection has emerged as a critical research topic. Existing online data selection methods for LLM training are typically “batch-constrained”, limiting optimization to local utility within random batches. To overcome this, we propose GAIA (Global Adaptive Instruction tuning via GAussian processes), a framework that formulates data valuation as a global estimation process. GAIA employs Gaussian Process regression to model continuous utility manifolds across the semantic space, utilizing an adaptive strategy fusion mechanism to dynamically prioritize high-utility samples. By casting the strategy-posterior update as an instance of the classical fixed-share Hedge framework for tracking the best expert, we inherit a dynamic-regret guarantee that characterizes GAIA’s robustness under non-stationary quality scores during training. Empirical evaluations on three datasets demonstrate that GAIA significantly outperforms state-of-the-art baselines like GREATS, establishing our method as a scalable and robust solution for efficient instruction tuning.

Q1: 这篇论文试图解决什么问题？

现有在线数据选择方法（如GREATS）在训练过程中动态选择数据，但受限于“批次约束”——每次随机采样的候选批次限制了选择范围，优化被限定在局部。这种随机性使得选择质量上限受候选池本身质量的限制，无法全局考虑数据的效用分布。本文旨在解决如何超越批次约束，实现全局自适应数据选择。

Q2: 有哪些相关研究？

相关研究包括基于梯度的在线数据选择方法（如GREATS）、基于损失或不确定性的主动学习方法。GREATS采用每次迭代的梯度信息作为选择依据，但仅在当前随机批次内最优。其他如MaxLoss、GradNorm等方法也被用于数据筛选，但缺乏全局视角。高斯过程在贝叶斯优化中常用于建模黑箱函数，本文将其引入数据选择，形成与现有方法不同的全局建模路径。

Q3: 论文如何解决这个问题？

GAIA框架的核心包括：1）使用高斯过程回归在语义空间中建模数据点的连续效用流形，将数据选择转化为全局优化问题；2）设计自适应策略融合机制，动态调整多个基础评分函数（如损失、梯度范数）的权重，通过固定份额Hedge算法更新策略后验，保证在数据质量分布非平稳时的动态遗憾界。从而实现在整个数据空间而非仅随机批次内的效用最大化。

Q4: 论文做了哪些实验？

论文在三个指令微调数据集上进行实验，对比基线包括GREATS、MaxLoss、GradNorm等随机选择方法。训练过程中跟踪选择数据的效用变化和下游任务性能。具体实验设置（数据集名称、模型架构、评估指标）因证据不足未完全呈现，但提到了GAIA在早期阶段快速识别高价值数据点，加速学习过程。

Q5: 发现了什么实验现象？

实验观察包括：GAIA在训练初始阶段便能有效选择高价值数据，相比GREATS（基于梯度）更早开始学习加速；下游任务性能上GAIA持续优于所有基线；消融研究（合理推断）验证了高斯过程建模与策略融合各自的作用。但具体数值（如准确率提升百分比）有待原文确认。

Q6: 有什么可以进一步探索的点？

未来方向可包括：1）改进基础评分函数，降低GAIA对其效能的依赖；2）将GAIA扩展到多任务或持续学习场景；3）探索更高效的GP近似以支持大规模数据；4）理论分析动态遗憾界在实际训练中的紧致性；5）结合数据多样性约束进一步优化选择策略。

Q7: 总结一下论文的主要内容

本文聚焦于大语言模型指令微调中的在线数据选择问题。现有方法受限于批次约束，仅能在随机采样的小批量中寻找最优，无法实现全局效用优化。作者提出GAIA（Global Adaptive Instruction tuning via GAussian processes）框架，首次将高斯过程回归应用于在线数据选择，通过对语义空间进行连续建模，将数据估值从局部批次扩展到全局。GAIA引入自适应策略融合机制，动态结合多个基础评分函数（如损失、梯度范数），并通过固定份额Hedge算法更新策略权重，提供理论上的动态遗憾保证，确保在训练过程中数据质量分布变化时仍能稳健选择。实验在三个指令微调数据集上展开，与GREATS、MaxLoss、GradNorm等基线对比，结果显示GAIA在数据选择效率（早期更快识别高价值数据）和下游任务性能上均显著领先。消融实验（推测）验证了核心组件的有效性。论文同时指出其局限：性能强依赖于基础评分函数的质量，且GP计算成本可能随数据量增长。总体而言，该工作为在线数据选择提供了一种全局优化的新范式，兼具理论保障与实证优势。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文方法虽聚焦指令微调，但全局数据选择策略可推广至其他LLM训练阶段。

## 基本信息

- 作者：Jun Wang, Quoc Phong Nguyen, Julien Monteil, Vu Nguyen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.30077`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索片段（Abstract、Introduction、Conclusion等），并结合启发式草稿进行合理推断，具体数值和未出现部分待原文验证。
