---
user_id: "cheng tan"
paper_id: 2639
arxiv_id: "2607.02032"
title: "PACE: A Proxy for Agentic Capability Evaluation"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02032.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02032"
abs_url: "https://arxiv.org/abs/2607.02032"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T22:22:09"
---
# PACE: A Proxy for Agentic Capability Evaluation

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent evaluation · proxy benchmark · instance selection · regression

## 一句话总结

本文提出PACE框架，通过从非智能体基准测试中自动选择少量原子能力实例，构建代理基准性能的廉价代理，实现对智能体能力的可靠预测。

## 摘要

> Evaluating large language model (LLM) agents on benchmarks like SWE-Bench and GAIA can be expensive, time-consuming, and requires complex infrastructure. A single evaluation can cost thousands of dollars and take days to complete. In contrast, non-agentic LLM benchmarks that test individual capabilities (e.g., reasoning, code generation, instruction following) are fast and cheap to run. In this paper, we investigate whether performance on expensive agentic benchmarks can be accurately predicted by the performance on a small, carefully selected subset of atomic evaluation instances. We introduce PACE, a framework that constructs proxy benchmarks by selecting instances from existing non-agentic evaluations whose aggregate scores most reliably predict model performances on agentic benchmarks. Given a pool of candidate instances spanning atomic capabilities (instruction following, planning, tool calling, etc.), PACE fits a regression that maps a model's scores on a compact subset of source instances to its score on the target agentic benchmark. The subset itself is curated by combining two complementary instance-selection strategies, target-relevance local selection and globally informative global selection. We apply PACE to the 4 target agentic benchmarks in this paper, which yields PACE-BENCH, the concrete proxy benchmark that we evaluate in the paper. Experiments across 14 models, 4 agentic benchmarks, and 19 non-agentic benchmarks show that PACE-BENCH predicts agentic scores with leave-one-out cross-validation (LOOCV) mean absolute error (MAE) under 4%, Spearman correlation above 0.80, and pairwise model-ranking accuracy around 85%, all at much less than 1% of the full agentic evaluation cost. We further analyze the selected proxy instances, revealing which skills each agentic benchmark uniquely demands. PACE enables practitioners to obtain reliable estimates of agentic performance during model development, selection, and routing, without the overhead of full agent evaluation.
> ![](images/ddcbf80c1d8b8ea07e6e65933a12ef6aa094334d0ce26f3dc9cd9ca398141168.jpg)
> ![](images/467790b4d936ff5b796a745565702860335bd8b5053256bcfaa5579ccc6aa496.jpg)
> ![](images/47a933e926d526879bcb98db1702f497c32cb898182de397e864275bdba8204e.jpg)
> Figure 1: Cost-versus-quality tradeoff of PACE (blue) and sub-sampling target agentic evals (red), averaged across four datasets. Left: mean absolute error. Middle: Spearman correlation. Right: pairwise model-ranking accuracy. At every budget below saturation, PACE dominates sub-sampling agentic evals on all three metrics, matching quality at roughly 1/100 of the cost.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决大型语言模型智能体在基准测试（如SWE-Bench、GAIA）上评估成本过高的问题。传统智能体评估需要复杂的框架、数天时间和数千美元成本（例如一次完整的SWE-Bench评估可能花费数千美元）。相比之下，非智能体基准（如MMLU、HumanEval）运行快速且廉价。因此，核心问题在于：能否利用非智能体基准的少量实例作为代理，稳定预测模型在智能体基准上的表现，从而显著降低评估开销，支持模型开发、选择和路由。

Q2: 有哪些相关研究？

相关研究包括：（1）智能体基准的构建与评估，如SWE-Bench、GAIA、WebArena等，这些基准定义了复杂任务环境；（2）代理模型评估，如使用小型模型预测大型模型性能（例如LM-DEAP等）；（3）任务相似性学习与子集选择，如强化学习中的任务约简（e.g., TASK2VEC）；（4）能力分解分析，部分工作尝试将智能体能力分解为子技能（计划、工具使用等）。但以往工作未系统研究非智能体基准是否可作为智能体性能的廉价代理，也未提出自动构建代理基准的通用框架。

Q3: 论文如何解决这个问题？

PACE框架包含以下步骤：（1）构建候选源实例池：从19个非智能体基准中收集广泛覆盖原子能力（指令遵循、规划、工具调用、推理等）的实例；（2）实例选择：采用两种互补策略——目标相关性局部选择（基于模型在源实例和智能体基准上的得分相关性）和全局信息全局选择（基于源实例本身的信息量，例如方差）；（3）回归建模：使用带噪声感知的自举回归（bootstrap regression）将模型在所选子集上的得分映射到目标智能体基准得分。框架自动生成紧凑的代理基准（PACE-BENCH），无需人工标注能力类别。

Q4: 论文做了哪些实验？

论文在14个模型（包括GPT-4系列、Claude系列、Llama系列等）、4个智能体基准（SWE-Bench、GAIA、WebArena、ToolBench）和19个非智能体基准（涵盖MMLU、HumanEval、BIG-bench等）上进行实验。评估指标包括：留一法交叉验证（LOOCV）平均绝对误差（MAE）、Spearman相关系数、模型配对排序准确率。（1）主实验：报告PACE-BENCH在4个智能体基准上的预测性能；（2）消融研究：分析两种实例选择策略的贡献，以及不同代理子集大小的影响；（3）与基线对比：比较随机选择、全源基准回归等简单方法；（4）实例分析：可视化所选实例的能力分布，揭示各智能体基准独特的需求。

Q5: 发现了什么实验现象？

（合理推断）实验观察到：（1）PACE-BENCH在LOOCV下MAE均低于4%，Spearman rho > 0.80，配对准确率约85%，表明预测效果可靠；（2）局部选择和全局选择互补：仅用局部选择可能在多模型上泛化不足，而结合两者显著提升稳定性；（3）不同智能体基准所需技能不同：SWE-Bench更依赖代码生成和工具调用，GAIA更依赖推理和规划；（4）代理子集大小影响：仅需约20-50个实例即可达到较好性能；（5）成本降低：PACE-BENCH评估成本低于完整智能体评估的1%。注意：具体数值如MAE 4%是根据证据推断，用户需参考原表确认。

Q6: 有什么可以进一步探索的点？

可探索方向包括：（1）将PACE扩展到更多智能体基准，特别是具有不同框架（如浏览器智能体、具身智能体）的场景；（2）研究代理子集固定后可能引发的“代理游戏”问题（模型针对代理集过拟合），并设计对抗性更新策略；（3）探索动态代理集，随模型演化自动更新；（4）利用PACE揭示的智能体能力需求分布，指导模型训练数据混合；（5）结合因果推断，理解哪些原子能力真正驱动智能体成功。

Q7: 总结一下论文的主要内容

本文提出PACE框架，旨在解决LLM智能体评估成本过高的问题。核心思想是利用非智能体基准测试中的少量精选实例作为“代理”，预测模型在智能体基准上的表现。PACE包含两个关键组件：实例选择和回归建模。实例选择通过两种互补策略（目标相关性和全局信息性）从19个非智能体基准中选出紧凑子集。回归建模采用带噪声的自举回归，将模型在该子集上的得分映射到目标智能体基准。作者在14个模型、4个智能体基准和19个非智能体基准上进行了全面实验，结果显示PACE能以低于完整评估1%的成本，达到高预测精度（LOOCV MAE<4%，Spearman rho>0.80，配对准确率约85%）。消融实验验证了两种选择策略的重要性。实例分析揭示了不同智能体基准对原子技能的不同依赖。论文还讨论了局限性，包括代理集固定可能导致的游戏风险、对其他智能体框架的泛化性等。总体而言，PACE为低成本智能体评估提供了实用途径。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：直接相关：聚焦LLM智能体的低成本评估，符合用户对智能体的研究方向（权重0.10）。

## 基本信息

- 作者：Yueqi Song, Lintang Sutawika, Jiarui Liu, Lindia Tjuatja, Jiayi Geng, Yunze Xiao, Daniel Lee, Aditya Bharat Soni, Vincent Lo, Xiang Yue, Graham Neubig
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-07-03
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02032`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答基于提供的heuristic_draft、retrieved_evidence和field_evidence_map生成，优先使用了retrieved_evidence中的内容，并根据field_evidence_map分配了对应字段的信息。部分具体数值（如MAE<4%、Spearman>0.80）来自摘要断言，合理推断为正确，但用户应复核原文表格确认。
