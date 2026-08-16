---
user_id: "cheng tan"
paper_id: 7459
arxiv_id: "2608.09826v1"
title: "Distill Skills into Weights, Not Prompts: Abstract Skills as Privileged Signals for On-Policy Self-Distillation"
institution: "未明确提供具体机构，根据作者姓名推测为中文学术机构或企业实验室。"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.09826v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.09826v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-12T01:29:14"
---
# Distill Skills into Weights, Not Prompts: Abstract Skills as Privileged Signals for On-Policy Self-Distillation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：self-distillation · reinforcement learning · mathematical reasoning · privileged context

## 一句话总结

SKALD 提出了一种在线自蒸馏框架，通过将抽象“技能卡”作为特权上下文引入教师分支，解决了强化学习在全对或全错样本组中梯度消失的问题，显著提升了模型在数学推理任务中的表现。

## 摘要

> Reinforcement learning with verifiable rewards yields no group-relative signal when rollout groups are uniformly correct or uniformly wrong, which account for 63.0-68.0% of groups in our experiments. We propose SKALD (Skill-Anchored Latent Distillation), an on-policy self-distillation framework that uses two context views of the same Qwen3-Base model: a question-only student and a teacher conditioned on an abstract, explicit-answer-filtered skill card. The student is trained on its own prefixes, transferring the skill-induced advantage into shared parameters without privileged input at test time. To stabilize context-induced distribution mismatch, SKALD employs an annealed exponentially tilted objective that downweights teacher-preferred tokens with very low student likelihood; as the tilt vanishes, it converges to teacher cross-entropy and recovers the forward-KL student gradient. An empirical gate activates distillation only when verified rollouts estimate a positive teacher advantage. Across five held-out mathematics benchmarks, SKALD improves overall avg@8 over GRPO by +2.46, +4.85, and +12.01 at 0.6B, 1.7B, and 4B, respectively. At 1.7B, zero-variance-only distillation recovers 84.7% of the full gain, while SKALD remains +4.06 above FLOP-matched GRPO and exceeds contextual skill exposure by +3.77. These results show that abstract skills provide dense supervision where group-relative rewards become uninformative.

Q1: 这篇论文试图解决什么问题？

### 1. RLVR 的信号稀疏性与代数盲点
在具有可验证奖励的强化学习（RLVR）中，如 GRPO 等算法依赖于组内相对奖励来计算优势（Advantage）。当一个 rollout 组内的所有样本全部正确或全部错误时，组内归一化后的优势为零。这意味着这些题目在当前策略下无法提供任何奖励梯度信号。实验发现，这种“零方差”组在训练中占比高达 63%-68%，严重限制了学习效率，尤其是在极难（全错）或已掌握（全对）的题目上。

### 2. 现有蒸馏方案的局限
传统的在线蒸馏通常需要一个更大、更强的外部教师模型，这增加了训练和部署的计算负担。而现有的自蒸馏方法往往直接使用完整参考答案作为特权信息，这容易导致模型产生对答案的过拟合或简单的模式记忆，而非真正的推理能力提升。

### 3. 核心挑战：如何定义有效的特权信息
论文试图解决的核心问题是：在不引入外部大模型且不泄露最终答案的前提下，如何为模型提供密集的、 token 级别的监督信号，以填补 RLVR 在零方差区域的真空？作者认为，抽象的“技能（Skill）”是关键，它能引导模型走向正确的推理路径，同时保持泛化性。

Q2: 有哪些相关研究？

### 1. 强化学习与数学推理
近期研究（如 Chen et al. 2026; Shao et al. 2024）强调了 RLVR 在提升 LLM 推理能力中的作用，但其结果监督的稀疏性一直是瓶颈。SKALD 旨在作为 RLVR 的补充，在奖励无法区分样本时提供监督。

### 2. 在线蒸馏与自蒸馏
在线蒸馏允许学生模型采样自己的轨迹，而教师提供 token 级的指导。SKALD 属于自蒸馏范畴，通过改变上下文视角（Privileged Context）来构建教师，这与 Li et al. 2026 和 Yang et al. 2026 的思路一致，但 SKALD 强调的是“抽象技能”而非具体答案。

### 3. 分布对齐与散度优化
在处理教师与学生分布失配时，SKALD 引入了 Rényi 类型的倾斜目标函数，这与传统的 Forward-KL 蒸馏有所不同，旨在处理教师分布中可能存在的低质量或学生难以理解的 token。

Q3: 论文如何解决这个问题？

### 1. 双上下文视角（Two-Context Setup）
SKALD 在训练期间维护同一个模型的两个逻辑分支：
- **学生分支**：仅输入问题 $Q$。
- **教师分支**：输入问题 $Q$ 加上一个“技能卡（Skill Card）” $S$。技能卡是经过过滤的、不含显式答案的抽象原理或解题技巧。

### 2. 退火指数倾斜目标函数（Annealed Exponentially Tilted Objective）
为了解决教师（有特权信息）和学生（无特权信息）之间的分布失配，SKALD 使用了一种 Rényi 类型的交叉熵损失：
- **权重调整**：通过一个倾斜参数 $\tau$ 降低那些学生模型预测概率极低的教师 token 的权重，防止学生被教师中过于“跳跃”或不符合学生当前能力的分布误导。
- **退火机制**：随着训练进行，$\tau$ 逐渐趋于 0，使目标函数收敛于标准的教师交叉熵，从而恢复 Forward-KL 梯度。

### 3. 经验门控机制（Empirical Gate）
并非所有的技能卡都能提供正向引导。SKALD 设计了一个门控，只有当验证后的 rollout 估计出教师分支具有正向优势（即教师引导下的回答正确率高于学生）时，才激活蒸馏。这确保了只有高质量的教师信号被传递给学生。

### 4. 在线自蒸馏流程
学生模型采样自己的 rollout 轨迹，教师分支对这些轨迹进行评分并提供 token 级的概率分布，通过上述目标函数将技能诱导的优势转移到共享的参数权重中。

Q4: 论文做了哪些实验？

### 1. 实验设置
- **基础模型**：Qwen3-Base 系列（0.6B, 1.7B, 4B）。
- **基准测试**：五个预留的数学基准测试（Held-out Mathematics Benchmarks）。
- **对比基线**：GRPO（Group Relative Policy Optimization）以及 FLOP 匹配的 GRPO 变体。

### 2. 训练细节
- 采用在线采样方式，学生生成 prefix，教师在对应位置提供监督。
- 针对 63%-68% 的零方差组进行了专门的消融实验，验证蒸馏在这些区域的有效性。
- 进行了严格的去重和防泄露处理，包括问题、解法和技能卡的去重。

Q5: 发现了什么实验现象？

### 1. 性能提升与规模效应
- 在所有规模上均显著优于 GRPO：0.6B 提升 +2.46，1.7B 提升 +4.85，4B 提升 +12.01。显示出模型规模越大，SKALD 带来的增益越明显。
- 在 4B 规模下，SKALD 比经过调优的等 FLOP GRPO 高出 +11.19。

### 2. 零方差组的贡献
- 实验证实，仅在 RLVR 无法提供信号的“零方差组”上进行蒸馏，就能恢复 1.7B 模型总增益的 84.7%。这有力证明了 SKALD 填补了 RL 的关键信息缺口。

### 3. 教师优势与门控
- 经验门控有效地过滤了无效的技能引导。在 4B 模型中，技能诱导的优势、倾斜参数和门控激活率均保持在正向区间，确保了蒸馏的稳定性。

### 4. 防泄露验证
- 通过人工审计和自动化清理，确认了增益并非来自技能卡中的答案泄露，而是来自于对解题技能的内化。

Q6: 有什么可以进一步探索的点？

### 1. 技能卡的自动化生成与优化
目前技能卡依赖于预定义的抽象，未来可以探索如何让模型在训练过程中自动发现和提炼有效的技能抽象。

### 2. 跨领域迁移
将 SKALD 框架从数学推理扩展到代码生成、逻辑证明或科学发现等其他具有可验证奖励的领域。

### 3. 动态退火策略
探索更复杂的退火策略，根据学生模型的实时表现动态调整倾斜参数 $\tau$，以实现更平滑的知识转移。

### 4. 与多模态推理结合
在多模态场景下，特权信息可以是图像的文本描述或中间视觉特征，利用 SKALD 进行跨模态自蒸馏。

Q7: 总结一下论文的主要内容

本文提出了 SKALD 框架，旨在解决强化学习在数学推理任务中因奖励信号稀疏（尤其是组内全对或全错导致的零方差问题）而产生的训练效率低下。SKALD 的核心思想是“将技能内化到权重中，而非依赖提示词”。它通过在训练时为同一模型提供一个带有抽象“技能卡”的教师视角，引导模型在 token 级别进行学习。为了克服教师与学生之间的分布差异，作者引入了带退火机制的指数倾斜目标函数和经验门控，确保只有高质量、可吸收的知识被蒸馏。实验结果令人印象深刻，尤其是在 4B 规模模型上取得了显著的性能突破，且证明了该方法能极好地利用 RLVR 无法处理的 60% 以上的训练数据。SKALD 为提升轻量级模型的推理能力提供了一种高效、不依赖外部大模型且计算友好的新范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于关注 LLM 推理能力提升、强化学习优化以及模型压缩/蒸馏的研究者具有极高参考价值。

## 基本信息

- 作者：Yubo Jiang, Fengying Xie, Zhiguo Jiang, Haopeng Zhang
- 机构：未明确提供具体机构，根据作者姓名推测为中文学术机构或企业实验室。
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.09826v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索到的 Introduction, Method, Results 和 Conclusion 部分的证据，重点分析了 SKALD 对 RLVR 信号稀疏问题的改进机制。
