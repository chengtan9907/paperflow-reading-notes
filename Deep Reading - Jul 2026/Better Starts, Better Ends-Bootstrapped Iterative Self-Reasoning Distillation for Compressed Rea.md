---
user_id: "cheng tan"
paper_id: 4678
arxiv_id: "2607.15736"
title: "Better Starts, Better Ends: Bootstrapped Iterative Self-Reasoning Distillation for Compressed Reasoning"
publish_date: "2026-07-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.15736.pdf"
pdf_url: "https://arxiv.org/pdf/2607.15736"
abs_url: "https://arxiv.org/abs/2607.15736"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T01:45:58"
---
# Better Starts, Better Ends: Bootstrapped Iterative Self-Reasoning Distillation for Compressed Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：chain-of-thought reasoning · self-distillation · on-policy distillation · reasoning compression

## 一句话总结

提出BIRD，一种两阶段自蒸馏方法，通过简洁指令生成正确轨迹并切换提示进行SFT热启，再经在线反向KL蒸馏，在维持或提升准确率的同时显著压缩推理链长度，实现更强准确-效率权衡。

## 摘要

> Large reasoning models often solve problems through long chain-of-thought (CoT) traces, yet much of this computation is spent on redundant derivations, repeated self-verification, and detours that do not improve the final answer. Existing on-policy self-distillation methods reduce this cost by matching a student model to a concise copy of itself on prefixes sampled from the student's own rollouts. We show that this objective has an initialization bottleneck. Since supervision is applied only to visited prefixes, training from a verbose base model places the KL loss on contexts that are often noisy, redundant, or already off track. In such regions, a concise teacher can provide only local corrections, while the student continues to explore trajectories that an efficient reasoner should avoid. In this paper, we propose BIRD(Bootstrapped Iterative Self-Reasoning Distillation), a two-stage self-reasoning distillation method that improves the rollout distribution before on-policy training. BIRD first samples concise solutions from the base model under a brevity instruction, keeps only answer-correct traces, and performs a lightweight prompt-switch SFT step. The traces are generated with the brevity instruction but learned under the original task prompt, turning instruction-induced conciseness into a default reasoning behavior. Starting from this warm model, BIRD then applies on-policy reverse-KL distillation with a concise self-teacher, now on cleaner and more informative prefixes. Across Qwen3 series models, BIRD achieves a stronger accuracy-efficiency trade-off than prompting and cold-start on-policy distillation on MATH-500 and AIME benchmarks. On Qwen3-8B, it improves MATH-500 accuracy from 86.2% to 92.0% while reducing the average response length from 3,099 to 1,115 tokens. These results highlight prefix support as a central factor in efficient reasoning distillation.

Q1: 这篇论文试图解决什么问题？

现有on-policy自蒸馏（OPSD）在压缩推理链时存在初始化瓶颈。训练时KL散度仅在学生当前rollout的前缀上计算，如果从冗长的基模型开始，这些前缀往往充满冗余、噪声甚至错误方向。简洁教师只能在这些区域内提供局部校正，而学生不会自发探索高效推理者应避免的轨迹，导致蒸馏效率低下。本质问题是前缀支持（prefix support）受限：教师无法在有效推理所需的分布上施加足够影响。

Q2: 有哪些相关研究？

相关工作包括：(1) 链式推理CoT（Wei et al., 2022）赋予模型中间步骤能力但增加推理长度；(2) 推理蒸馏方法如DistilBERT、长度惩罚训练等试图压缩推理序列；(3) On-policy Self-Distillation（OPSD）通过在线KL匹配自我精简，本文直接与之对比并揭示其冷启动缺陷；(4) 通过指令控制推理长度（如简洁提示）也被广泛使用，但仅推理时不改变模型默认行为；(5) 自我改进方法（如STaR、ReST）通过正确轨迹迭代训练，但往往需要大量数据。本文结合了指令微调、正确性筛选和在线蒸馏的思路。

Q3: 论文如何解决这个问题？

BIRD采用两阶段训练。Stage 1（热启）：对基模型使用简洁指令（如“用简洁步骤作答”）采样大量解答，仅保留答案正确的轨迹；然后使用原始任务提示对这些正确轨迹进行监督微调（SFT）。此处的提示切换（prompt-switch）使简洁成为模型的默认推理行为而非瞬时效应的伪影。Stage 2（在线蒸馏）：以Stage 1微调后的模型为学生，以基模型在简洁指令下的输出（或同一简洁教师）为教师，执行on-policy反向KL蒸馏。学生从原始提示采样完整rollout，在rollout前缀上计算KL散度，迫使学生对齐教师的简洁分布。由于Stage 1已改善前缀支持，第二阶段蒸馏效率更高。

Q4: 论文做了哪些实验？

实验在Qwen3系列模型（包括多个规模）上进行。基准采用MATH-500和AIME数学推理数据集。对比方法包括：(1) base model（原始推理）；(2) base model + brevity instruction（仅推理时加简洁提示）；(3) cold-start OPSD（从基模型直接进行on-policy蒸馏）；(4) BIRD（本文方法）。评估指标为准确率（Accuracy）和平均响应长度（Length, tokens）。消融研究分别验证Stage 1和Stage 2的贡献、提示切换的作用、以及正确性筛选的影响。还分析了模型规模缩放趋势。

Q5: 发现了什么实验现象？

主要现象：(1) BIRD在Qwen3-8B上MATH-500准确率从86.2%提升至92.0%，平均长度从3099降至1115 tokens，实现了准确率的增长与大幅压缩；(2) 仅使用简洁指令推理（不训练）相比基模型已有一定改善，但BIRD在此基础上进一步大幅提升压缩比且准确率更高；(3) 与cold-start OPSD相比，BIRD在同等长度下准确率显著更高，说明热启有效改善了前缀分布；(4) 模型规模越大，BIRD相对基线提升幅度越大，推测更大模型更可靠地遵循简洁指令从而产生更干净的初始数据；(5) BIRD的方法可跨模型系列迁移（例如从Qwen3迁移到其他家族），验证了其通用性。

Q6: 有什么可以进一步探索的点？

未来可探索方向：(1) 更精细的第一阶段数据筛选策略，例如纳入部分正确或结构合理的轨迹；(2) 多轮蒸馏或迭代式自改进，持续优化前缀支持；(3) 将BIRD扩展到更多领域如代码生成、常识推理、数学证明等；(4) 与其他推理加速技术结合（如speculative decoding、early exiting、自适应计算）；(5) 理论分析反向KL蒸馏在前缀分布下的收敛性与动态；(6) 探索更灵活的教师形式（如长度自适应教师）。

Q7: 总结一下论文的主要内容

本文聚焦于大型语言模型长思维链推理的效率问题。尽管CoT提升了复杂推理能力，但冗长的中间步骤导致推理成本高昂。现有on-policy自蒸馏（OPSD）通过让学生模型匹配自身rollout前缀上的简洁版本来压缩推理，但由于KL损失仅在学生已访问的前缀上计算，当从冗长基模型开始时，前缀包含大量噪声，蒸馏信号只能局部修正而无法引导模型进入高效推理区域，即存在冷启动问题。为解决这一瓶颈，本文提出BIRD（Bootstrapped Iterative Self-Reasoning Distillation），一种两阶段自蒸馏方法。第一阶段：利用简洁指令从基模型采样多条解答，过滤出答案正确的轨迹，然后使用原始任务提示（而非简洁指令）对这些轨迹进行监督微调。此步骤将指令诱导的简洁性内化为模型的默认推理行为，显著改善了后续蒸馏时学生rollout的前缀分布。第二阶段：以第一阶段微调后的模型为学生，以基模型在简洁指令下的输出为教师（或同一简洁模型），进行on-policy反向KL蒸馏。此时学生从原始提示采样，KL损失在前缀上迫使学生逼近教师的简洁分布，由于前缀已经较清晰，蒸馏效率大幅提升。实验在Qwen3系列模型（不同规模）上展开，在MATH-500和AME数学题集上进行评估。主要结果：Qwen3-8B上MATH-500准确率从86.2%提高到92.0%，平均响应长度从3099个token降至1115个；相比仅使用简洁提示推理（准确率88.1%，长度1924）和cold-start OPSD（准确率90.3%，长度1382），BIRD在压缩更严重的同时准确率更高。缩放实验显示模型越大BIRD优势越明显，且方法可跨模型系列迁移。消融实验验证了第一阶段热启、提示切换、正确性筛选等各组件均不可或缺。综上，BIRD强调前缀支持是高效推理蒸馏的核心因素，通过改善前缀分布实现了更强的准确-效率权衡。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与推理效率压缩研究方向高度相关，特别是长链推理的长度控制

## 基本信息

- 作者：Leichao Dong, Dongxu Zhang, Yiding Sun, Qirui Wang, Yuhan Wang, Lin Chen, Jihua Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.15736`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要基于PDF片段检索证据（retrieved_evidence）及heuristic_draft，未参考完整PDF原文。
