---
user_id: "cheng tan"
paper_id: 2046
arxiv_id: "2606.29938"
title: "LatentRevise: Learning from Zero-Hit Reasoning"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.29938.pdf"
pdf_url: "https://arxiv.org/pdf/2606.29938"
abs_url: "https://arxiv.org/abs/2606.29938"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:45:18"
---
# LatentRevise: Learning from Zero-Hit Reasoning

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning with verifiable rewards · zero-hit prompts · latent revision · reasoning

## 一句话总结

提出 LatentRevise 方法，通过一阶潜在修订从零命中提示的失败轨迹中恢复训练信号，显著提升数学推理性能。

## 摘要

> Reinforcement learning with verifiable rewards (RLVR) is bottlenecked by hard prompts on which correct trajectories have low probability, so sampling misses them within a practical budget and leaves the policy update with little useful signal. We frame such zero-hit prompts as RLVR's sampling frontier, where new reasoning behavior is most valuable yet least likely to be sampled. Importantly, failed rollouts can be informative: they expose where the model's reasoning went wrong. We introduce LatentRevise, a first-order latent revision method that recovers training signal for this zero-hit regime. Given a failed rollout and the gold answer as an anchor, LatentRevise optimizes the input embeddings of its reasoning prefix under two complementary gradients, moving the prefix away from the failed continuation and toward the gold answer. The optimization is constrained to the convex hull of the model's vocabulary embeddings, so each update moves the latent toward a real token embedding rather than an arbitrary feature direction. We find that continuations from the revised prefix lengthen, exhibit self-reflection, and reach correct answers missed by the original rollouts. Used as training data, these trajectories improve SFT and RLVR on math benchmarks over standard baselines.

Q1: 这篇论文试图解决什么问题？

RLVR 在困难提示（hard prompts）上面临核心瓶颈：由于正确轨迹的生成概率极低，在有限采样预算（如 pass@N=0）下无法采样到任何正确轨迹，导致策略更新缺乏有效监督信号。现有方法如增加采样或使用外部指导（教师模型、过程监督）成本高昂或不可行。因此，如何从零命中提示中提取有用信号成为关键问题。

Q2: 有哪些相关研究？

相关工作包括：（1）支持约束下的 RLVR，如 STaR（Zelikman et al., 2022）和 RFT（Yuan et al., 2023）通过自训练利用模型自己的正确轨迹；Group-relative RLVR 方法（如 GRPO）利用组内相对优势，但仍依赖正确轨迹的存在。（2）直接偏好优化（DPO）等方法可从失败轨迹中学习，但需要正确与错误配对，而零命中提示下正确轨迹缺失。（3）推理时搜索策略（如 MCTS）通过增大计算开销提升成功率，但未解决训练信号缺失问题。本文是首个从零命中提示的失败轨迹中直接恢复训练信号的方法。

Q3: 论文如何解决这个问题？

LatentRevise 的核心思想是：给定一个失败 rollout（推理轨迹）和正确答案作为锚点，在连续潜在空间中对推理前缀的输入嵌入进行优化。优化包含两个互补梯度：一个将前缀表示推离失败续写方向，另一个将其拉向正确答案的表示。优化过程被约束在模型词汇嵌入的凸包内，确保每次更新后的潜在表示都对应真实词汇嵌入，而非任意特征方向。修订后的前缀然后用于继续生成，产生新的完整轨迹。这些新轨迹若通过验证器，则作为训练数据用于后续的 SFT 或 RLVR 训练。

Q4: 论文做了哪些实验？

论文在数学推理基准（如 GSM8K、MATH）上评估 LatentRevise。实验设置包括：（1）零命中提示设置：选择 pass@N=0 的提示作为测试集；（2）基线方法：标准采样（增加采样数）、STaR、RFT、以及直接使用失败轨迹的 DPO；（3）训练协议：先用 LatentRevise 产生正确轨迹，再用于 SFT 或 RLVR 微调。此外，还进行了消融实验评估两个梯度的贡献、凸包约束的影响，以及不同修订步数的效果。

Q5: 发现了什么实验现象？

实验观察到：修订后的前缀生成的轨迹显著变长，并表现出自我反思行为（如检查、纠正中间步骤）。修订后的轨迹在零命中提示上恢复了大量正确解，使 pass@k 显著提升。作为训练数据，这些轨迹使 SFT 和 RLVR 在困难提示上的准确率超过标准基线。凸包约束被证明是必要的：移除后修订方向不稳定，性能下降。两个梯度中，推离失败梯度和拉向答案梯度均不可或缺。

Q6: 有什么可以进一步探索的点？

可探索的方向包括：（1）将 LatentRevise 扩展到更复杂的多步推理任务，如科学推理或代码生成；（2）结合搜索策略（如 MCTS）进一步生成更高质量的轨迹；（3）研究如何自动设置优化步数和学习率等超参数；（4）将该方法应用于其他模态（如视觉推理）；（5）探索将失败信息用于过程监督信号的生成。

Q7: 总结一下论文的主要内容

论文提出 LatentRevise，一种针对 RLVR 零命中提示的潜在修订方法。该方法利用失败推理轨迹和正确答案，在词汇嵌入凸包约束下优化前缀嵌入，从而产生正确的续写轨迹。实验证明，该方法能从原本无法学习的提示中恢复训练信号，大幅提升数学推理性能，并在 SFT 和 RLVR 设置下显著优于基线。工作填补了 RLVR 在处理极端稀疏奖励时的空白，展示了从失败中学习的潜力。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：对于智能体路径规划或推理，零命中问题普遍存在，本文方法可借鉴用于训练信号恢复。

## 基本信息

- 作者：Yiqiu Guo, Xueting Han, Qi Jia, Guangtao Zhai, Jing Bai
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-30
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.29938`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 检索证据（摘要、引言、结论、相关工作等片段），对 heuristic_draft 进行了润色和补全，并依据证据映射分配字段内容。
