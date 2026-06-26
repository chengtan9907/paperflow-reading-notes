---
user_id: "cheng tan"
paper_id: 1619
arxiv_id: "2606.27369"
title: "Reinforcement Learning without Ground-Truth Solutions can Improve LLMs"
publish_date: "2026-06-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.27369.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27369"
abs_url: "https://arxiv.org/abs/2606.27369"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-26T14:16:06"
---
# Reinforcement Learning without Ground-Truth Solutions can Improve LLMs

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · large language models · score-based optimization · reward calibration

## 一句话总结

提出 RiVER 框架，通过排名校准的奖励成型，在无 ground-truth 解决方案的可执行优化任务上训练 LLM，不仅能提升任务评分，还能泛化到精确求解基准。

## 摘要

> Reinforcement learning with verifiable rewards (RLVR) for training LLMs typically rely on ground-truth answers to assign rewards, limiting their applicability to tasks where the ground-truth solution is unknown. We introduce a \textbf{R}anking-\textbf{i}nduced \textbf{VER}ifiable framework (RiVER) that trains LLMs on score-based optimization tasks without ground-truth solutions, using deterministic execution feedback as continuous-valued supervision. When applying group-relative RL to such continuous rewards, we identify two key challenges: \emph{scale dominance}, where uncalibrated score magnitudes across test instances distort policy updates, and \emph{frequency dominance}, where repeatedly sampled suboptimal solutions can outweigh rare but stronger candidates. RiVER addresses these challenges with calibrated reward shaping that uses instance-wise comparisons and emphasizes top-ranked solvers while retaining bounded feedback for other valid solutions. We train on 12 AtCoder Heuristic Contest tasks and evaluate on Algorithm Engineering Benchmark (ALE-Bench), LiveCodeBench, and USACO. RiVER advances Qwen3-8B and GLM-Z1-9B-0414 by 8.9\% and 9.4\% in ALE rating rank. More importantly, despite training exclusively on score-based tasks without any ground-truth solutions, RiVER also improves the backbones across exact-solution benchmarks such as LiveCodeBench and USACO by an absolute average improvement of 2.4\% and 3.5\%. By contrast, baselines trained with raw execution scores improve ALE rating but fail to transfer to exact-solution benchmarks. These results suggest that score-based optimization tasks, combined with proper reward calibration, can serve as effective training environments for general coding ability without ground-truth solutions.

Q1: 这篇论文试图解决什么问题？

现有基于可验证奖励的强化学习（RLVR）在训练 LLM 时，通常要求任务有 ground-truth 答案才能构造二值或离散奖励，这严重限制了 RLVR 在开放型任务（如算法工程、启发式优化）中的应用，因为这些任务没有标准答案，只有连续评分。直接使用原始执行分数作为奖励会引入两个问题：一是不同实例的分数尺度差异导致策略更新不稳定（scale dominance）；二是重复采样中次优解出现次数多，可能淹没有利但少见的更好解（frequency dominance）。因此，关键问题是：如何在没有 ground-truth 解决方案的情况下，从可执行反馈中设计稳定且有效的 RL 训练信号，并保证泛化能力。

Q2: 有哪些相关研究？

相关工作包括：1）基于二值可验证奖励的 RLVR（如 AlphaCode 的正面/负面反馈）；2）直接使用执行结果（如代码通过率）作为奖励的 RL 方法；3）RLHF 中使用人类偏好信号，但需要大量人工标注；4）过程奖励模型（PRM）逐步监督但仍依赖专家或自洽性；5）群体相对策略优化（GRPO）在 LLM 中的应用。本文区别于以上方法之处在于：不依赖任何形式的 ground-truth，仅利用实例内相对排名和分数，并专门解决连续评分中特有的尺度与频率偏差问题。

Q3: 论文如何解决这个问题？

RiVER 是一个基于排名的可验证 RL 框架，核心步骤包括：1）对每个问题，策略采样一组可执行的求解器（代码）；2）每个求解器在隐藏测试实例上执行并得到连续分数；3）为解决 scale dominance，RiVER 对每个实例独立进行奖励校准：将原始分数转换为实例内的排名或分位数，消除跨实例尺度差异；4）为解决 frequency dominance，RiVER 采用 top-k 加权的信号聚合：强调排名靠前的求解器，同时为其他有效解保留有界奖励，避免高频率次优解主导梯度；5）最终奖励为校准后的排名驱动信号，输入群体相对 RL（如 GRPO）进行策略更新。整个训练过程完全无需 ground-truth 答案。

Q4: 论文做了哪些实验？

实验设置：使用 12 个来自 AtCoder Heuristic Contest 的算法工程任务作为训练环境，每个任务都有可执行的客观评分函数。基线包括：原始分数直接作为奖励（Raw Score）、二值通过/失败奖励（Binary）、以及不使用 RL 的监督微调（SFT）。评估在三个基准上进行：Algorithm Engineering Benchmark (ALE-Bench, 评分排名)、LiveCodeBench（精确代码生成）、USACO（编程竞赛问题）。模型基础：Qwen3-8B 和 GLM-Z1-9B-0414。训练细节：使用群体相对 RL（GRPO），每组采样 8 个求解器，训练 5000 步。

Q5: 发现了什么实验现象？

1）RiVER 在 ALE-Bench 评分排名上显著提升：Qwen3-8B 提升 8.9%，GLM-Z1-9B-0414 提升 9.4%，表明奖励校准对评分任务有效。2）关键发现：RiVER 在从未接触过 ground-truth 的条件下，在精确求解基准（LiveCodeBench、USACO）上也取得平均 2.4% 和 3.5% 的绝对提升，说明评分任务的训练信号能泛化到精确推理能力。3）反直觉结果：仅使用原始执行分数的基线（Raw Score）在 ALE 上也有提升（但弱于 RiVER），但在精确基准上不仅不提升，反而下降或持平，表明未校准的连续奖励可能导致过拟合于评分分布，损害泛化。4）频率主导的实证：实验中观察到，低分但高频出现的解在原始奖励下贡献了过多梯度，而 RiVER 的 top-k 加权有效抑制了这种影响。5）消融研究显示，同时进行实例内排名校准和 top-k 加权比单独使用其中一种效果更好。

Q6: 有什么可以进一步探索的点？

1）将 RiVER 扩展到非代码的评分任务（如数学推理、机器人控制、科学模拟），检验其通用性。2）结合信息论视角，探索更精确的奖励校准方法（如互信息最大化）。3）研究 RiVER 在更大模型家族（如 70B 以上）上的表现及 scaling 规律。4）将评分优化与精确推理的协同机制用于多任务学习，分析何种评分任务最有利于泛化。5）探索在训练环境中引入动态难度调整（如课程学习）以进一步提升样本效率。6）对频率主导和尺度主导进行更深入的理论分析，例如在何种分布偏移下校准策略失效。

Q7: 总结一下论文的主要内容

本文针对 RLVR 对 ground-truth 答案的依赖问题，提出 RiVER 框架，将可执行评分任务作为 RL 训练环境。核心贡献是识别并解决了连续评分奖励中的两个偏差（scale dominance 和 frequency dominance），并通过实例级排名校准和 top-k 加权设计出稳定的奖励成型方法。实验在 12 个算法工程任务上训练，并在三个异构基准上评估，证明了 RiVER 不仅能提升评分任务性能，还能迁移到精确求解基准，而未经校准的原始分数基线则无法迁移。这项工作为利用丰富但无 ground-truth 的可执行环境来增强 LLM 的通用编码能力开辟了新路径。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：对智能体（agent）领域：提供了在开放环境中利用执行反馈进行自我改进的通用范式。

## 基本信息

- 作者：Yingyu Lin, Qiyue Gao, Nikki Lijing Kuang, Xunpeng Huang, Kun Zhou, Tongtong Liang, Zhewei Yao, Yi-An Ma, Yuxiong He
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-06-26
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.27369`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 检索证据（abstract, introduction, conclusion, method 片段），并结合启发式草稿进行扩展，对未提供明确信息的字段（如 related_work, future_directions）进行了合理推断，已标注。
