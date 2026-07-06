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
saved_at: "2026-07-06T12:37:50"
---
# Purified OPSD: On-Policy Self-Distillation Without Losing How to Think

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy self-distillation · long chain-of-thought reasoning · knowledge distillation · pointwise mutual information

## 一句话总结

本文深入诊断了在线自蒸馏 (OPSD) 在长思维链推理模型上失效的根本原因——教师监督信号中参考诱导成分主导导致死记硬背，并提出了净化监督信号的两步解决方案：构造参考教师隔离非可迁移成分，再利用点互信息 (PMI) 将残差转变为可蒸馏的目标分布，从而在多个模型上实现一致改进并保留反思推理能力。

## 摘要

> On-policy self-distillation (OPSD) has emerged as a promising paradigm for improving LLM reasoning, where a privileged teacher with access to reference solutions provides token-level supervision on the student's own generated trajectories. However, we find that OPSD consistently fails on long chain-of-thought (long-CoT) reasoning models, yielding at best marginal gains while destabilizing the reflective reasoning capability these models depend on. Through a novel decomposition of the teacher's supervision signal, we identify the root cause: the teacher's supervision is dominated by a reference-induced component that drives rote memorization of reference-specific shortcuts, while the question-conditioned, inference-transferable component is ignored or actively opposed. Based on this diagnosis, we propose a two-step solution. First, we construct a reference-only teacher (the same model conditioned on the reference without the question) to isolate the non-transferable component of the supervision signal; the residual after subtracting this component captures the question-conditioned, inference-transferable correction. Second, we use pointwise mutual information (PMI) as the mechanism to transform this residual into a well-formed PMI target distribution that the student can directly distill from, filtering out the reference-induced shortcut. Experiments on four long-CoT models across two datasets demonstrate consistent improvements over both the base model and standard OPSD, while preserving the models' natural epistemic behavior throughout training.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决标准在线自蒸馏 (OPSD) 在长思维链 (long-CoT) 推理模型上失败的问题。尽管 OPSD 在普通推理任务上有效，但在需要长时间反思推理的长 CoT 模型上，它要么只带来微弱的提升，要么显著破坏模型的反思推理能力。论文深入探究了失败机制，发现教师（基于参考解提供监督）的监督信号存在一个主导成分——参考诱导成分，该成分鼓励学生记忆参考解中的表面模式（捷径），而压制了真正对推理迁移有帮助的问题条件化信号。因此，需要一种新的蒸馏方法，能够过滤掉参考诱导成分，保留可迁移的推理信号，同时不损害模型的内部认知过程。

Q2: 有哪些相关研究？

相关工作包括三个主要方面：第一，知识蒸馏 (Knowledge Distillation, Hinton et al., 2015) 通过软标签匹配从教师向学生传递知识，在线自蒸馏 (OPSD) 是其变体，教师利用参考解提供 token 级监督 (Agarwal et al., 2024; He et al., 2026; Zhao et al., 2026; Cui et al., 2026)。第二，长思维链推理模型 (如 Gandhi et al., 2024; Li et al., 2025) 通过扩展推理步骤来量化不确定性并实现反思推理，将长 CoT 能力蒸馏到小模型引起了大量关注，现有方法包括在教师生成轨迹上的监督微调 (SFT) 和强化学习，但这些离策略方法存在训练-测试分布不匹配问题。第三，针对 OPSD 失败的分析，已有工作指出分布偏移或教师质量问题，但缺乏对长 CoT 模型特殊性的深入理解。本文通过信号分解诊断了失败原因，并提出了净化监督信号的方案，填补了该空白。

Q3: 论文如何解决这个问题？

论文提出净化 OPSD (Purified OPSD) 的两步方法。第一步：构造一个“仅参考教师” (reference-only teacher)，即与教师模型相同但不输入问题、仅输入参考解的变体，其输出捕获了监督信号中对参考解本身的过拟合部分（非可迁移成分）。从原始教师输出中减去该成分，得到残差，该残差代表了问题条件化、可推理迁移的修正信号。第二步：利用点互信息 (Pointwise Mutual Information, PMI) 将残差转换为一个具有良好概率性质的 PMI 目标分布。具体而言，PMI 量化了残差中每个 token 相对于参考解的条件对数几率，从而过滤掉参考捷径，使得学生可以直接蒸馏这个目标分布。整个框架与标准 OPSD 无缝集成，训练开销仅增加一个前向推理（参考教师）。

Q4: 论文做了哪些实验？

论文在四个代表性的长 CoT 推理模型上进行实验：Qwen3-8B、Qwen3-4B、DeepSeek-R1-Distill-Qwen-7B 和 OLMo-7B-Thinking。训练数据为 Math-CoT-20K，包含约 20K 数学推理问题和参考解。对比基线包括未经蒸馏的基模型和标准 OPSD。评估指标涵盖数学推理准确率、反映反思能力的自我纠正率以及表示认知行为稳定性的指标（如熵、置信度校准）。实验还设计了消融研究，验证参考教师和 PMI 目标组件的贡献，以及不同温度、损失权重等超参数的影响。训练采用标准 OPSD 框架，学生从自身生成轨迹学习，教师提供 token 级监督。

Q5: 发现了什么实验现象？

实验发现以下关键现象：(1) 标准 OPSD 在所有四个长 CoT 模型上均导致性能下降或仅有边际提升，同时模型的自我纠正能力（反思推理的标志）显著恶化，输出熵降低表明模型变得过于自信，损坏了自然认知行为。(2) 提出的 Purified OPSD 在所有设置下均显著优于基线和标准 OPSD，准确率有明显提升，并恢复了自我纠正率，使模型的置信度校准更接近原始模型。(3) 消融研究表明，仅使用参考教师减去或仅使用 PMI 重加权都不足以取得最佳效果，两者结合至关重要。(4) 训练过程中，标准 OPSD 早早地偏离了基线的行迹，而 Purified OPSD 始终保持在更接近基线的认知区域，表明其保留了模型的推理风格。(5) 在分布外 (OOD) 测试集上，Purified OPSD 的泛化鲁棒性也更强，显示出更好的迁移能力。

Q6: 有什么可以进一步探索的点？

基于当前工作，有多个可进一步探索的点：1) 将 Purified OPSD 扩展到更复杂的推理任务（如科学推理、多步规划），验证其泛化能力。2) 减少对参考解的依赖，探索弱监督或无参考解下的净化机制。3) 将净化思想与强化学习（如 RLHF/RLAIF）结合，可能进一步提升模型的推理熟练度。4) 深入分析 PMI 目标的理论性质，理解其为何能有效保留认知行为。5) 将诊断方法（信号分解）应用于其他蒸馏或训练不稳定场景，例如多模态模型或在线学习。6) 探索更高效的参考教师构造，降低额外计算开销。

Q7: 总结一下论文的主要内容

本文系统研究了在线自蒸馏 (OPSD) 在长思维链推理模型上的失败现象及其深层原因，并提出了一种有效的净化方案。首先，论文通过大量实验证实标准 OPSD 在四个不同规模的长 CoT 模型（Qwen3-8B、Qwen3-4B、DeepSeek-R1-Distill-Qwen-7B 和 OLMo-7B-Thinking）上均未带来明显提升，反而损害了模型的反思推理能力，表现为自我纠正能力下降和过度自信。为了揭开这一反直觉现象，作者创新性地对教师的监督信号进行了分解，发现来自参考解的成分在监督中占据主导，它迫使模型记忆参考解中的局部模式（捷径），而衡量真正推理能力的“问题条件化”成分被压制。基于这个诊断，论文设计了净化 OPSD：构建一个“仅参考”教师，得到纯参考解驱动的输出，从原教师输出中减去该部分得到残差；然后利用点互信息 (PMI) 将残差转化为一个形式良好的概率目标，学生直接优化这个新目标从而避开参考捷径。实验在四个主流长 CoT 模型上，以 Math-CoT-20K 为训练集，验证了该方法的有效性。不仅在数学推理准确率上相比基线和标准 OPSD 有稳定提升，更重要的是恢复了模型的自我纠正倾向和置信度校准，表明其内在推理过程得以保留。消融实验表明，两个组件——参考教师减法和 PMI 蒸馏——缺一不可。此外，论文还展示了方法在训练过程中的认知行为更稳定，并且在分布外测试上具有更强的泛化能力。这项工作不仅解决了 OPSD 在长 CoT 场景下失效的实际问题，而且通过信号分解提供了一种诊断训练失败的一般性框架，对理解知识蒸馏中的信息干扰具有理论启示。整体而言，该论文问题明确、分析深入、方法精巧、实验充分，是知识蒸馏与推理能力提升领域的一项重要贡献。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文对蒸馏失败原因的深入分析为理解训练不稳定提供了新视角，可迁移至其他模型的训练诊断。

## 基本信息

- 作者：Zhanming Shen, Jintao Tong, Shaotian Yan, Chen Shen, Hao Chen, Wentao Ye, Xiaomeng Hu, Rui Miao, Haobo Wang, Junbo Zhao, Gang Chen, Jieping Ye
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02234`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于 heuristic_draft 和 retrieved_evidence，优先参考了 field_evidence_map 中指定的证据锚点，内容均源自论文摘要、结论及相关节段。
