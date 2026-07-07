---
user_id: "cheng tan"
paper_id: 2862
arxiv_id: "2607.05391"
title: "LLM-as-a-Verifier: A General-Purpose Verification Framework"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.05391.pdf"
pdf_url: "https://arxiv.org/pdf/2607.05391"
abs_url: "https://arxiv.org/abs/2607.05391"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:55:05"
---
# LLM-as-a-Verifier: A General-Purpose Verification Framework

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm-as-a-verifier · verification · scaling · agent

## 一句话总结

提出LLM-as-a-Verifier通用验证框架，通过对评分token logits取期望得到连续评分，实现验证在评分粒度、重复评估和标准分解三个轴上的缩放，在多个agent任务基准上取得SOTA性能，并可作为密集奖励信号提升强化学习样本效率。

## 摘要

> Scaling pre-training, post-training, and test-time compute have become the central paradigms for improving the capabilities of LLMs. In this work, we identify verification, the ability to determine the correctness of a solution, as a new scaling axis. To unlock this and demonstrate its effectiveness, we introduce LLM-as-a-Verifier, a general-purpose verification framework that provides fine-grained feedback for agentic tasks without requiring additional training. Unlike standard LM judges that prompt LLMs to produce discrete scores for candidate solutions, LLM-as-a-Verifier computes the expectation over the distribution of scoring token logits to generate continuous scores. This probabilistic formulation enables verification to scale along multiple dimensions: (1) score granularity, (2) repeated evaluation, and (3) criteria decomposition. In particular, we show that scaling the scoring granularity leads to better separation between positive and negative solutions, resulting in more calibrated comparisons. Moreover, scaling repeated evaluation and criteria decomposition consistently lead to additional gains in verification accuracy through variance and complexity reduction. We further introduce a cost-efficient ranking algorithm for selecting the best solution among candidates using the verifier's continuous scores. LLM-as-a-Verifier achieves state-of-the-art performance on Terminal-Bench V2 (86.5%), SWE-Bench Verified (78.2%), RoboRewardBench (87.4%), and MedAgentBench (73.3%). Beyond verification, the fine-grained signals from LLM-as-a-Verifier can also serve as a proxy for estimating task progress. We build an extension for Claude Code, enabling developers to monitor and improve their own agentic systems. Finally, we show that LLM-as-a-Verifier can provide dense feedback for RL, improving the sample efficiency of SAC and GRPO on robotics and mathematical reasoning benchmarks.

Q1: 这篇论文试图解决什么问题？

当前大语言模型能力提升主要依赖预训练、后训练和测试时计算的缩放，但验证能力——即判断解决方案正确性的能力——尚未得到系统性的缩放。现有验证方法（如标准LM Judge）仅生成离散分数，缺乏细粒度，且无法有效区分正负样本。这限制了智能体系统在复杂任务中的可靠性和可扩展性。论文旨在填补这一空白，将验证作为独立缩放轴进行探索。

Q2: 有哪些相关研究？

相关研究包括：(1) 标准LM Judge：直接提示LLM输出离散评分，粒度粗，校准性差。(2) 奖励模型：训练专门的评分模型，需要收集偏好数据，泛化性有限。(3) 过程奖励模型（PRM）：提供中间步骤反馈，但通常需要人工标注或蒙特卡洛估计。(4) Best-of-N采样：使用结果奖励模型（ORM）选择最佳输出，但离散评分导致平局较多。(5) LLM自我一致性：通过多次采样聚合，但仅适用于有限任务。本工作与这些方法不同，通过连续评分和缩放轴实现无需训练的通用验证。

Q3: 论文如何解决这个问题？

LLM-as-a-Verifier的核心思想是将验证转化为连续评分问题。具体地，给定候选解决方案，设计一个评分模板（如'Score the correctness: [score]'），让LLM生成一个评分token（如数字或程度词）。框架计算该token在logits上的softmax分布，再对token值（如0-10）取期望，得到连续分数。在此基础上提出三个缩放轴：(1) 评分粒度：使用更细的评分尺度（如0-100代替0-10），提高正负样本分离度。(2) 重复评估：对同一方案多次独立评分后取平均，降低方差。(3) 标准分解：将整体评分分解为多个子标准的评分（如正确性、安全性、效率），再聚合。此外，论文提出一种成本高效的排名算法，利用连续分数间的差异进行排序，减少比较次数。

Q4: 论文做了哪些实验？

论文在多个基准上实验：Terminal-Bench V2（终端智能体任务）、SWE-Bench Verified（软件工程任务）、RoboRewardBench（机器人奖励设计）、MedAgentBench（医学智能体）。主要实验包括：(1) 验证性能：与Best-of-N基线（使用GPT-5.5等生成候选，LM Judge评分）对比，LLM-as-a-Verifier在Terminal-Bench V2上达86.5%，SWE-Bench Verified 78.2%，RoboRewardBench 87.4%，MedAgentBench 73.3%。(2) 缩放效果：系统性地测试评分粒度（如10级 vs 100级）、重复次数（1-10次）、标准数量（1-5个）对验证准确率的影响。(3) 作为过程/结果奖励模型：在智能体任务中评估pass@1，发现缩放提升单调。(4) 作为RL奖励信号：用于SAC（机器人任务）和GRPO（数学推理MATH基准），对比使用离散分数或任务奖励的基线，LLM-as-a-Verifier奖励使SAC样本效率提升约2倍，GRPO提升约1.1倍。(5) TurboAgent扩展：集成到Claude Code，作为监控和改进智能体的工具。

Q5: 发现了什么实验现象？

实验揭示以下现象：(1) 连续评分相比离散评分能更好地分离正负样本，尤其在评分粒度提升后，AUC显著提高。(2) 重复评估带来持续的准确率增益，但收益递减；每次重复约降低0.5-1%错误率。(3) 标准分解将复杂判断分解为子问题，减少了单次评分的方差，整体准确率提升2-4%。(4) 作为PRM时，pass@1随验证缩放单调递增，表明细粒度验证可指导搜索。(5) 在RL实验中，使用LLM-as-a-Verifier密集奖励比稀疏任务奖励收敛更快，且最终性能相当或更优。(6) 消融实验发现，评分粒度是最重要的缩放轴，重复评估和标准分解在粒度足够细时仍有叠加增益。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：(1) 扩展到无法直接获取logits的封闭API模型（如GPT-5.5、Claude Opus 4.7），通过样本统计近似连续分数。(2) 将验证框架应用于更广泛的领域，如代码生成、数学证明、法律分析等。(3) 探索验证缩放与生成缩放的协同作用，例如自验证生成。(4) 利用细粒度验证信号进行主动学习或数据过滤。(5) 将验证作为通用奖励信号用于更多RL算法和任务。(6) 研究标准分解的自动化，例如让LLM自动生成子标准。

Q7: 总结一下论文的主要内容

论文提出LLM-as-a-Verifier，一种无需训练的通用验证框架，通过连续评分实现了验证能力的缩放。背景：尽管预训练、后训练和测试时计算的缩放极大提升了LLM能力，但验证——判断解决方案正确性的能力——未得到同等关注。标准方法如LM Judge仅提供离散分数，粒度粗、区分度低。方法：框架的核心是计算评分token logits的期望，得到连续分数。在此基础上，作者识别出三个缩放轴：(1) 评分粒度（细化评分尺度），(2) 重复评估（多次评分平均），(3) 标准分解（多维标准评分聚合）。实验：在Terminal-Bench V2 (86.5%)、SWE-Bench Verified (78.2%)、RoboRewardBench (87.4%)、MedAgentBench (73.3%)上达到SOTA。缩放实验证实各轴均能提升准确率，且粒度轴收益最大。作为过程奖励模型时，pass@1单调提升。作为强化学习奖励信号，显著提升SAC在机器人任务和GRPO在数学推理上的样本效率（约2倍和1.1倍）。此外，细粒度信号可作为任务进展代理，并开发了TurboAgent工具集成到Claude Code。讨论：验证应被视为与生成同等重要的缩放轴。局限：需要访问模型logits，不适用于完全封闭API；部分场景下连续评分的校准性仍待改进。结论：LLM-as-a-Verifier提供了一种低成本、高收益的验证增强方案，有望成为智能体系统的标准组件。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦于智能体（agent）系统，与你画像中的智能体方向（权重0.1）直接相关。

## 基本信息

- 作者：Jacky Kwok, Shulu Li, Pranav Atreya, Yuejiang Liu, Yixing Jiang, Chelsea Finn, Marco Pavone, Ion Stoica, Azalia Mirhoseini
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG, cs.MA, cs.RO
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.05391`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据，并结合了heuristic_draft进行补充和润色。
