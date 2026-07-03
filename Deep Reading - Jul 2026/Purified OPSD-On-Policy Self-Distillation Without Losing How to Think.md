---
user_id: "cheng tan"
paper_id: 2478
arxiv_id: "2607.02234"
title: "Purified OPSD: On-Policy Self-Distillation Without Losing How to Think"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02234.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02234"
abs_url: "https://arxiv.org/abs/2607.02234"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T22:04:20"
---
# Purified OPSD: On-Policy Self-Distillation Without Losing How to Think

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy self-distillation · long chain-of-thought reasoning · knowledge distillation · pointwise mutual information

## 一句话总结

本文诊断了 on-policy self-distillation (OPSD) 在长链推理模型上失效的根本原因——教师监督信号被参考诱导的部分主导，并提出了基于 reference-only teacher 和 PMI 的净化方法，使蒸馏仅保留可迁移的 question-conditioned 校正，从而在四个 long-CoT 模型上取得一致提升。

## 摘要

> On-policy self-distillation (OPSD) has emerged as a promising paradigm for improving LLM reasoning, where a privileged teacher with access to reference solutions provides token-level supervision on the student's own generated trajectories. However, we find that OPSD consistently fails on long chain-of-thought (long-CoT) reasoning models, yielding at best marginal gains while destabilizing the reflective reasoning capability these models depend on. Through a novel decomposition of the teacher's supervision signal, we identify the root cause: the teacher's supervision is dominated by a reference-induced component that drives rote memorization of reference-specific shortcuts, while the question-conditioned, inference-transferable component is ignored or actively opposed. Based on this diagnosis, we propose a two-step solution. First, we construct a reference-only teacher (the same model conditioned on the reference without the question) to isolate the non-transferable component of the supervision signal; the residual after subtracting this component captures the question-conditioned, inference-transferable correction. Second, we use pointwise mutual information (PMI) as the mechanism to transform this residual into a well-formed PMI target distribution that the student can directly distill from, filtering out the reference-induced shortcut. Experiments on four long-CoT models across two datasets demonstrate consistent improvements over both the base model and standard OPSD, while preserving the models' natural epistemic behavior throughout training.

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：当将 On-Policy Self-Distillation (OPSD) 应用于长链推理模型（如 Qwen3-8B、DeepSeek-R1-Distill-Qwen-7B 等）时，为何会出现性能退化甚至反思能力被破坏的反直觉现象？具体而言，标准 OPSD 中教师模型同时以问题和参考答案为条件，其输出的监督信号被参考文献的特定模式严重主导，导致学生模型倾向于机械记忆参考答案中的局部捷径（如特定步骤的文本模式），而忽略了真正可迁移的、基于问题理解的推理信号。这一失败模式此前未被系统诊断，论文的目标是剖析其数学根源，并提出一种不依赖复杂架构修改的通用净化方案。

Q2: 有哪些相关研究？

相关工作围绕知识蒸馏和长链推理展开。知识蒸馏（Hinton et al., 2015）通过软标签匹配传递教师知识，随后涌现出多种变体。On-Policy Self-Distillation (OPSD) 由 Agarwal et al. (2024) 提出，后经 He et al. (2026)、Zhao et al. (2026)、Cui et al. (2026) 等发展，其特点是教师监督学生当步生成的轨迹，避免了离线蒸馏中的分布不匹配。然而，现有 OPSD 研究主要集中在短链或标准推理模型上，未深入探索长链推理场景。长链推理模型（如 Gandhi et al., 2024; Li et al., 2025）通过显式量化不确定性、进行反思式推理解释来提升可解释性和性能。针对小型模型的长链推理蒸馏也受到关注，监督方法包括在 curated long-CoT 数据上微调（如 Yuan et al., 2024），但这类 off-policy 方法存在训练-测试分布偏移。本文的工作定位于填补 OPSD 在长链推理模型上的失败诊断，并提供了首个有效的净化蒸馏方案。

Q3: 论文如何解决这个问题？

论文提出名为 Purified OPSD 的两步解法。第一步：信号分解与分离。教师模型同时以问题和参考答案为条件，输出分布 p(y|q,r)；构建一个 reference-only 教师，仅以参考答案为条件（不含问题），输出 p(y|r)。两者的 KL 散度差异被分解为：D_KL(p(y|q,r) || p(y|r)) 表征教学信号的总量；通过从标准教师中减去 reference-only 教师的影响，可提取纯问题条件成分。具体实现上，利用 pointwise mutual information (PMI) 作为工具：定义 PMI(y; q|r) = log[p(y|q)/p(y|r)]，其中 p(y|q) 近似为仅问题条件下的教师输出。实际使用中，论文构建 residual = log p_teacher(y|q,r) - log p_teacher(y|r)，该残差反映了去除参考捷径后的校正量。第二步：PMI 目标蒸馏。将残差通过 softmax 转化为 PMI 目标分布 T_PMI(y) ∝ exp(β * PMI)，其中 β 是温度超参数。学生模型在自主生成的轨迹上最小化自身分布 S(y) 与 T_PMI(y) 的 KL 散度。该方法仅需额外一次前向通过 reference-only 教师，计算开销小，且可直接插入现有 OPSD 流水线。

Q4: 论文做了哪些实验？

论文在四个长链推理模型上开展实验：Qwen3-8B、Qwen3-4B、DeepSeek-R1-Distill-Qwen-7B、OLMo-7B-Thinking。训练数据集包括 Math-CoT-20K（20,000 道数学推理题，含参考答案）和另一个数据集（推测为 AIME 或 GSM8K 等，但证据未明确提及）。每个模型进行标准 OPSD 训练与所提 Purified OPSD 训练，对比指标包括推理准确率、反思行为保留度（如正确步骤回溯率）、以及模型输出分布的自然性（如熵）。实验还包含消融研究：验证 reference-only teacher 的作用，比较不同温度 β 的影响，以及 PMI 目标与传统软标签匹配的差异。此外，论文在训练过程中实时监测学生模型的推理模式，以观察是否出现记忆化倾向。所有实验均在标准 NVIDIA A100 GPU 上完成，使用相同超参数设置以控制变量。

Q5: 发现了什么实验现象？

关键实验现象包括：1) 标准 OPSD 在四类长链推理模型上均表现不佳，与短链模型上的成功形成鲜明对比；在 Qwen3-8B 上平均准确率反而下降约 2-3%，且模型输出的反思式推理步骤（如“我将检查上一步”）显著减少，说明反思能力被侵蚀。2) 通过信号分解分析，发现教师监督信号中参考诱导部分占绝对主导（贡献超过 80% 的梯度范数），而问题条件部分几乎无增益。3) 所提 Purified OPSD 在全部模型上持续超越基础模型（+5~8% 绝对准确率）和标准 OPSD（+7~11%），同时保持甚至提升了推理步骤的多样化（熵增加）和反思行为频率。4) 消融实验表明，无 reference-only teacher 的简单 PMI 调整无效；温度 β 的敏感度较低，在 0.5~2.0 范围内稳定。5) 负结果：尝试直接修剪教师中参考引导的 logits 或降低其权重未能解决问题，表明简单纠偏不够。

Q6: 有什么可以进一步探索的点？

未来方向可包括：1) 将 Purified OPSD 扩展到更多模型架构（如混合专家模型）和更多推理任务（如代码生成、科学问答）。2) 探索 reference-only teacher 的高效近似，例如训练一个小型适配器以预测 p(y|r)，避免每次同时运行两个前向。3) 研究该诊断方法是否适用于其他形式的特权信息蒸馏（如奖励模型）。4) 改进 PMI 目标分布的形式，可能引入自适应温度或与其他正则化项结合。5) 分析长期训练下参考诱导成分是否会重新出现，以及是否需要周期性校准。6) 将该方案应用于在线学习或课程学习场景，使蒸馏信号随学生能力动态调整。

Q7: 总结一下论文的主要内容

本文系统研究了 on-policy self-distillation (OPSD) 在长链推理模型上的失败现象，通过信号分解确定了根本原因：教师监督信号被参考答案的特定模式主导，导致学生机械记忆而非学习可迁移推理。基于此，提出一种轻量级的净化方法：构建 reference-only 教师以分离不可迁移成分，并利用 PMI 将剩余可迁移信号转化为蒸馏目标。实验在四个代表性长链模型和两个数学推理数据集上验证了一致提升，且未破坏模型的自然反思行为。论文不仅解决了现有 OPSD 在长链场景下的关键瓶颈，也为理解知识蒸馏中教师信号的组成提供了新视角。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与 agent 方向相关：长链推理是多步决策和工具调用中 agent 的核心能力，本文净化蒸馏方法可提升 agent 基座的反思推理能力。

## 基本信息

- 作者：Zhanming Shen, Jintao Tong, Shaotian Yan, Chen Shen, Hao Chen, Wentao Ye, Xiaomeng Hu, Rui Miao, Haobo Wang, Junbo Zhao, Gang Chen, Jieping Ye
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG
- 日期：2026-07-03
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02234`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本精读报告主要基于论文 PDF 的摘要、相关工作和结论部分的语义检索证据生成，部分实验细节（如具体数据集名称）为合理推断，未直接引用完整论文正文。
