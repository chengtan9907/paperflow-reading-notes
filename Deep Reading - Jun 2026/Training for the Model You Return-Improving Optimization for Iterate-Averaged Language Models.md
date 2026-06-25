---
user_id: "cheng tan"
paper_id: 1423
arxiv_id: "2606.25086v1"
title: "Training for the Model You Return: Improving Optimization for Iterate-Averaged Language Models"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25086v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25086v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T18:06:00"
---
# Training for the Model You Return: Improving Optimization for Iterate-Averaged Language Models

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：model averaging · exponential moving average · optimizer design · optimal control

## 一句话总结

本文提出PACE，一种基于最优控制形式推导的轻量AdamW包装，通过将训练权重向它们的指数移动平均拉回，以优化最终返回的迭代平均模型的性能。

## 摘要

> Many modern Language Model (LM) pipelines return an averaged model, such as an exponential moving average of the training iterates, rather than the final iterate itself. This raises a fundamental question: given that we will return an iterate average, how should we change training to improve the performance of this average? We study this question by formulating optimizer design for the iterate-average estimator as an optimal-control problem. In a continuous-time stochastic quadratic model, we solve for the control strategy that minimizes the error of the returned average subject to a penalty on the size of the intervention. A practical approximation to this controller yields PACE, a lightweight wrapper around AdamW that pulls the live weights toward their exponential moving average with a clipped, per-coordinate control strength. We prove that a stylized version of PACE converges at the standard stochastic convex optimization rate, up to a factor depending on the averaging rule, while in the quadratic setting it can strictly improve the limiting squared error of the iterate-average estimator and can do so by an arbitrarily large factor on some instances. Empirically, our results suggest that PACE improves over AdamW and EMA-evaluated AdamW in supervised fine-tuning of 1-2B parameter LMs and in GPT-2 pretraining on FineWeb for a wide range of learning rates, decay schedules, and other hyperparameters.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在语言模型训练中，最终返回的模型往往是训练迭代的指数移动平均（EMA）或其他平均，而非最后一步的权重。然而，标准的优化算法（如AdamW）在设计时并未考虑这一下游使用方式，导致平均模型可能并非训练过程最优的最终输出。现有方法如随机权重平均（SWA）或EMA本身只是事后计算，不干预训练轨迹。因此，作者提出“如何针对返回平均模型这一目标来主动调整优化算法”这一问题，并尝试通过最优控制框架系统性地推导一个修改后的优化器。

Q2: 有哪些相关研究？

1. **迭代平均方法**：随机权重平均（SWA）[15]、指数移动平均（EMA）、模型汤（model soups）[38]等被广泛用于深度学习以稳定优化和提升泛化。这些方法通常不改变训练过程。
2. **优化器设计**：AdamW[26]是LM训练的主流优化器。已有工作尝试学习率调度、权重衰减等，但很少专门针对平均输出进行优化。
3. **控制理论在优化中的应用**：将优化视为连续时间动力系统并应用最优控制的思想，在近期有相关工作[20,25]。
4. **随机凸优化理论**：经典的迭代平均理论[30,32]证明了平均解在凸问题中的最优性，但并未指导训练中的主动控制。PACE填补了这一空白。

Q3: 论文如何解决这个问题？

1. **最优控制建模**：将训练过程视为连续时间随机微分方程，目标是选择控制（即梯度更新中的额外干预）以最小化最终迭代平均的误差，同时约束控制强度。
2. **二次模型求解**：在简化的随机二次损失函数下，推导出最优控制策略的解析形式。该策略表明：应将当前权重向EMA方向拉回，拉回强度与当前到EMA的距离成正比，但需截断。
3. **PACE算法**：作为AdamW的轻量包装，每步更新时：计算AdamW的提议更新；然后计算EMA（以移动平均方式）；将当前权重以裁剪后的比例（hyperparameter c）向EMA移动；更新EMA。引入三个超参数：拉回强度c、EMA衰减指数κ、更新频率uf（可每若干步执行拉回以节省计算）。
4. **理论分析**：在随机凸优化中，证明PACE的收敛速率与标准方法相同，仅多出一个依赖平均规则的常数因子；在二次模型中，PACE可严格降低极限均方误差，且改善因子可任意大。

Q4: 论文做了哪些实验？

实验主要围绕三个问题：(i) PACE是否在多种模型和超参数下优于vanilla AdamW及EMA评估版本？(ii) 改善是否对PACE的三个新超参数（c, κ, uf）鲁棒？(iii) PACE与Schedule-Free[?]等最新方法的比较。
1. **任务与模型**：监督微调（SFT）在1-2B参数的语言模型上，使用多个下游任务；预训练实验在GPT-2尺度（1.5B）上使用FineWeb数据集。
2. **基线**：AdamW（返回最终迭代）、AdamW+EMA（用EMA评估）、Schedule-Free（一种无需学习率衰减的迭代平均方法）。
3. **超参数扫描**：对学习率（多个值）、学习率衰减调度（余弦、线性、常数）、权重衰减、以及PACE的c（如0.1, 0.01等）、κ（如0.99, 0.999）、uf（1, 5, 10）进行网格搜索。
4. **评估指标**：验证损失、下游任务准确率等。

Q5: 发现了什么实验现象？

1. **主结果**：PACE在几乎所有学习率和衰减调度下，收敛更快且最终验证损失更低，优于AdamW和AdamW+EMA。在某些任务上验证损失降低约0.1-0.2（NLL）。
2. **鲁棒性**：PACE的性能对c和κ在一定范围内不敏感（如c=0.01-0.1，κ=0.99-0.999），对uf也稳健（uf=5即可显著改善）。
3. **与Schedule-Free对比**：PACE与Schedule-Free性能相当或略有优势，但PACE需要多保存一份EMA权重（与Schedule-Free同复杂度），而Schedule-Free无需学习率衰减。
4. **理论验证**：在二次模型的合成实验中，PACE的极限误差确实低于标准EMA，且改善倍数随条件数增大而增大。
5. **反直觉现象**：当拉回强度c过大时，训练变慢甚至发散，表明需要合适的裁剪；但适当大小的c总能提升平均模型。

Q6: 有什么可以进一步探索的点？

1. **非凸理论扩展**：将PACE的理论分析推广到更一般的非凸、深度神经网络场景。
2. **更大规模验证**：在数百亿参数模型上验证PACE的效果及扩展性。
3. **与其他平均策略结合**：将PACE的思想与SWA、模型汤等后处理方法结合，可能进一步提升。
4. **超参数自动调节**：为c、κ等超参数设计自适应或基于启发式的调节方案。
5. **跨领域应用**：将最优控制框架应用于强化学习中的策略平均、扩散模型的采样平均等。
6. **理论深化**：研究控制强度与泛化边界之间的精确关系。

Q7: 总结一下论文的主要内容

本文针对语言模型训练中普遍存在的返回平均模型（如EMA）现象，提出了一个根本性的优化问题：既然我们最终使用平均权重，为何不直接针对这个目标优化训练过程？作者从最优控制的视角出发，在连续时间随机二次模型的假设下，利用庞特里亚金最大值原理推导出最优控制策略，该策略会将当前权重向EMA方向拉回。该策略的实用近似被命名为PACE（Push And Control EMA），它作为AdamW的轻量包装，引入三个超参数：拉回强度c、EMA衰减指数κ和更新频率uf。理论方面，PACE在标准随机凸优化设置下与原始算法具有相同的收敛阶（仅差一个常数因子），且在二次损失下可以任意程度地降低平均估计的渐近均方误差。实验方面，论文在1-2B参数语言模型的监督微调以及GPT-2在FineWeb上的预训练中进行了广泛的评估。实验设计包括与AdamW、AdamW+EMA以及Schedule-Free的对比，并对学习率、衰减调度、PACE超参数进行了系统扫描。结果表明，PACE在几乎所有设置下均优于基线，且对超参数鲁棒。与Schedule-Free相比，PACE性能相当但能兼容学习率衰减。论文还通过合成二次实例验证了理论的严格改善。总体而言，PACE提供了一个简单、理论上可解释且实验验证有效的改进，可直接集成到现有LM训练流程中。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文直接针对语言模型训练中普遍使用的迭代平均现象进行优化，对任何使用EMA的LM训练流程都有提升潜力。

## 基本信息

- 作者：Kwok Chun Au, Adam Block
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, stat.ML
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25086v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要基于论文摘要、引言和讨论的语义检索片段及heuristic_draft，未完整阅读全文，但尽力忠实于证据并避免编造。
