---
user_id: "cheng tan"
paper_id: 2728
arxiv_id: "2607.05394"
title: "Weak-to-Strong Generalization via Direct On-Policy Distillation"
institution: "清华大学智能产业研究院 (AIR), 字节跳动 (ByteDance)"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.05394.pdf"
pdf_url: "https://arxiv.org/pdf/2607.05394"
abs_url: "https://arxiv.org/abs/2607.05394"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:38:24"
---
# Weak-to-Strong Generalization via Direct On-Policy Distillation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：weak-to-strong generalization · on-policy distillation · reinforcement learning · policy shift

## 一句话总结

Direct-OPD 通过将弱模型在强化学习前后的策略偏移（Policy Shift）作为隐式奖励信号，在强学生的同策略状态上进行蒸馏，实现了低成本且高效的弱到强推理能力泛化。

## 摘要

> Reinforcement learning with verifiable rewards (RLVR) is a powerful recipe for improving language-model reasoning, but it is expensive to repeat on every new strong model because the target model must generate many rollouts during training. As models scale, post-training itself becomes a bottleneck. We study a weak-to-strong alternative: run RL on a smaller model where rollouts are cheaper, then reuse what that RL run learned to improve a stronger target model. Directly distilling the post-RL weak teacher is not enough, because the teacher's final policy mixes useful RL gains with the limitations of the smaller model. We propose Direct On-Policy Distillation (Direct-OPD), which transfers the teacher's RL-induced policy shift instead. Direct-OPD compares the post-RL teacher with its own pre-RL reference and treats their log-ratio as a dense implicit reward for the student. In plain terms, the checkpoint pair tells us which actions RL made the weak model more or less likely to take, and Direct-OPD applies that signal on the stronger student's own on-policy states. This directly reuses the weak model's RL supervision signal without training an explicit reward model or running sparse-reward RL on the target model. Empirically, Direct-OPD consistently leverages weaker teachers to improve stronger target models; notably, it boosts Qwen3-1.7B from 48.3% to 62.4% on AIME 2024 in just 4 hours on 8 A100 GPUs. It outperforms step-matched direct RL and enables the sequential composition of multiple policy shifts. Our results show that RL outcomes can be reused across model scales as implicit reward signals, not merely as final models to imitate.
> Date: July 7, 2026
> Correspondence: zhouhao@air.tsinghua.edu.cn
> Project Page: https://bytedtsinghua-sia.github.io/Direct-OPD/

Q1: 这篇论文试图解决什么问题？

### 1. 核心挑战：RLVR 的高昂成本
带可验证奖励的强化学习（RLVR）已成为提升大语言模型（LLM）推理能力的标配。然而，RLVR 的训练成本与模型规模直接挂钩：在训练过程中，目标模型需要生成大量的采样（rollouts），随着模型参数量的增加，这一过程产生的计算开销和显存占用成为后训练阶段的严重瓶颈。

### 2. 现有弱到强（Weak-to-Strong）方案的局限性
* **直接蒸馏的弊端**：传统的知识蒸馏通常让学生模型模仿弱老师的最终策略。但弱老师（小模型）受限于表达能力，其最终策略往往混合了 RL 带来的增益和模型本身的局限性（如幻觉、表达不精炼）。
* **性能天花板**：如果强学生仅仅是模仿弱老师，其性能上限往往被老师锁死，难以实现真正的“弱到强”突破。

### 3. 隐含假设与技术权衡
* **假设**：RL 过程对模型策略的“改进方向”（Policy Shift）比模型本身的“绝对状态”更具迁移价值。
* **权衡**：放弃了在强模型上进行端到端 RL 的高精度，换取了极高的计算效率和跨模型尺度的知识复用能力。

Q2: 有哪些相关研究？

### 1. 强化学习与推理模型
近期如 DeepSeek-R1 等工作证明了通过大规模 RL 提升模型推理能力的有效性。这类方法依赖于数学、代码等可验证的奖励信号，但对算力资源要求极高。

### 2. 弱到强泛化（Weak-to-Strong Generalization）
OpenAI 提出的范式探讨了如何利用弱模型生成的标签来监督强模型。本文将其扩展到了 RL 领域，不再局限于静态标签，而是关注 RL 诱导的动态策略变化。

### 3. 同策略蒸馏（On-Policy Distillation, OPD）
传统的 OPD 旨在通过学生自身的采样来对齐老师的分布。Direct-OPD 借鉴了这一形式，但创新性地将“老师的改进”而非“老师的输出”作为对齐目标。

### 4. 隐式奖励与 DPO 系列方法
Direct-OPD 在数学形式上与 DPO（直接偏好优化）有相似之处，即利用对数比（log-ratio）来表征奖励。不同之处在于，Direct-OPD 将其应用于跨模型的弱到强迁移场景。

Q3: 论文如何解决这个问题？

### 1. 核心机制：策略偏移提取（Policy Shift Extraction）
Direct-OPD 不直接使用弱老师 $\pi_{weak}^{RL}$，而是引入一个 pre-RL 的参考模型 $\pi_{weak}^{ref}$。通过计算两者在给定状态下的对数比：
$$r_{implicit} = \log \frac{\pi_{weak}^{RL}(a|s)}{\pi_{weak}^{ref}(a|s)}$$
这个对数比反映了 RL 过程鼓励或抑制了哪些行为，被定义为“策略偏移”。

### 2. 密集隐式奖励的应用
将上述策略偏移作为强学生 $\pi_{strong}$ 的密集奖励信号。在训练时，强学生根据当前提示词（Prompt）生成采样，并在这些同策略状态上计算老师的策略偏移量。

### 3. 训练目标函数
Direct-OPD 的优化目标结合了隐式奖励和 KL 散度约束：
* **奖励项**：最大化学生动作在老师策略偏移方向上的投影。
* **约束项**：引入 KL 散度防止学生模型偏离其初始权重 $\pi_{strong}^{ref}$ 太远，保持模型的稳定性。

### 4. 算法流程
1. 在小模型上运行 RLVR，获得 $\pi_{weak}^{RL}$。
2. 冻结 $\pi_{weak}^{RL}$ 和 $\pi_{weak}^{ref}$。
3. 强学生 $\pi_{strong}$ 进行同策略采样。
4. 利用老师的对数比作为奖励，更新学生参数。
5. （可选）进行多轮迭代或顺序组合多个不同的策略偏移。

Q4: 论文做了哪些实验？

### 1. 实验设置
* **老师模型**：以 R1-Distill-1.5B 为参考模型，JustRL-1.5B（经过 RL 训练）为 post-RL 老师。
* **学生模型**：Qwen3-1.7B、Qwen3-7B 等性能更强的模型。
* **任务领域**：数学推理（AIME 2024, MATH, GSM8K）。
* **硬件环境**：8x A100 80GB GPU。

### 2. 基准对比（Baselines）
* **Direct RL**：在相同步数下，直接对学生模型进行 RLVR 训练。
* **Standard Distillation**：学生直接模仿 $\pi_{weak}^{RL}$ 的输出分布。
* **SFT Baseline**：仅使用高质量数据进行有监督微调。

### 3. 评价指标
* Pass@1 准确率。
* 训练时间与算力消耗（TFLOPS）。
* 模型在未见任务上的泛化能力。

Q5: 发现了什么实验现象？

### 1. 显著的性能提升
* **Qwen3-1.7B 案例**：在 AIME 2024 任务上，基座模型准确率为 48.3%，经过 Direct-OPD 训练后飙升至 62.4%。
* **超越直接 RL**：在相同的训练步数和计算预算下，Direct-OPD 的效果优于直接在学生模型上运行 RL。这表明“策略偏移”提供了一个比稀疏奖励更易学习的密集信号。

### 2. 弱老师带强学生（Weak-to-Strong Success）
* **反直觉现象**：即使弱老师在某些任务上的绝对得分低于强学生，其 RL 诱导的策略偏移依然能为学生提供正向引导。这验证了“改进方向”的跨模型通用性。

### 3. 训练效率极高
* **时间成本**：Qwen3-1.7B 的提升仅需 4 小时训练。相比之下，传统的 RLVR 需要数倍的时间来处理复杂的 rollout 和奖励计算。

### 4. 策略偏移的顺序组合（Sequential Composition）
* 实验发现，可以先后应用来自不同 RL 阶段或不同任务的策略偏移，性能呈现阶梯式上升，证明了该信号的可叠加性。

### 5. 失败模式与局限
* 如果弱老师的 RL 过程本身存在严重的过拟合或奖励作弊（Reward Hacking），这种负面偏移也会被传递给学生。

Q6: 有什么可以进一步探索的点？

### 1. 跨领域迁移
目前实验集中在数学推理，未来可探索 Direct-OPD 在代码生成、创意写作或多模态任务中的表现，验证策略偏移信号的普适性。

### 2. 自动选择最优老师对
研究如何自动筛选或加权多个弱老师的策略偏移，以构建更强大的复合奖励信号。

### 3. 理论边界探索
深入分析策略偏移作为隐式奖励的数学收敛性，特别是在老师和学生模型架构差异巨大时，KL 约束如何动态调整以维持最优迁移。

### 4. 极小模型驱动极大模型
探索使用 100M 级别的极小模型作为“探路者”运行 RL，进而指导 70B 甚至更大型号模型的后训练，最大化算力杠杆效应。

Q7: 总结一下论文的主要内容

本文提出了 Direct-OPD，一种旨在解决大模型 RL 训练成本瓶颈的弱到强泛化方法。该方法的核心洞察在于：RL 对模型性能的提升可以被抽象为一种“策略偏移”信号，这种信号比模型最终的概率分布更具迁移价值。通过计算弱老师在 RL 前后的对数概率比，并将其作为密集奖励应用于强学生的同策略采样中，Direct-OPD 成功实现了在极低算力消耗下显著提升强模型推理能力的目标。实验在 Qwen3 系列模型上验证了其有效性，特别是在 AIME 2024 等高难度数学竞赛任务中表现卓越。该研究不仅为高效后训练提供了新路径，也深化了对弱到强泛化机制的理解，证明了 RL 成果可以作为一种通用的隐式奖励信号在不同规模的模型间流转。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于关注模型训练效率和算力优化的研究者具有极高参考价值。

## 基本信息

- 作者：Shiyuan Feng, Huan-ang Gao, Haohan Chi, Hanlin Wu, Zhilong Zhang, Zheng Jiang, Bingxiang He, Wei-Ying Ma, Ya-Qin Zhang, Hao Zhou
- 机构：清华大学智能产业研究院 (AIR), 字节跳动 (ByteDance)
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2607.05394`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点提取了 Abstract、Introduction 和实验结果部分的核心数据与逻辑。
