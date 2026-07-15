---
user_id: "cheng tan"
paper_id: 3783
arxiv_id: "2607.11444v1"
title: "UMoE:Unlocking Every Expert in Domain-Specific Training"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11444v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11444v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:48:20"
---
# UMoE:Unlocking Every Expert in Domain-Specific Training

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：mixture of experts · domain-specific training · expert pruning · expert regrowing

## 一句话总结

UMoE 是一种预算不变的流水线，通过在领域 SFT 前剪枝低显著性专家并扰动扩展来重新对齐专家池，从而持续提升领域性能。

## 摘要

> Mixture-of-Experts (MoE) models scale capacity without proportional compute cost and have become a key architecture for frontier large language models (LLMs). Yet domain-specific post-training inherits an expert pool shaped by mixed-domain pre-training: a substantial subset of experts contributes little on the target domain, and standard supervised fine-tuning (SFT) leaves the composition of this pool unchanged. We propose a simple, budget-preserving pipeline that realigns the expert pool to the target domain before fine-tuning. Given a target domain, we (1) prune the experts with lowest domain-aligned saliency, (2) regrow the expert pool to its original size through perturbation-based expert expansion, and (3) apply standard SFT. The resulting model preserves the original expert count, parameter count, and inference cost. With a single frozen recipe and no per-domain hyperparameter tuning, UMoE consistently improves over Direct SFT across two MoE architectures (Qwen3-30B-A3B and Qwen3.5-35B-A3B), five domains (math, code, science, tool-use, and agentic coding), and 12 benchmarks. Representative improvements are 3.4 points in math average accuracy, 6.0 points on SWE-bench Verified. On a strong in-house math corpus, Direct SFT already surpasses Qwen3-30B-A3B-Thinking (82.81 vs. 81.06), yet UMoE further raises the average to 84.17, an additional 1.36 points, demonstrating robustness to a substantially stronger SFT regime. Data-scaling experiments further show that the gain persists as training data grows. Analysis reveals that the direct-SFT model allocates substantial routed-expert compute to a low-saliency subset that can be removed post hoc with little average degradation; UMoE turns this redundant capacity into useful domain capacity and achieves lower training loss, with gains spanning all difficulty levels in downstream evaluation.
> ![](images/ad695b2edbb5b9e0f350556c8076726d07c11ea9c640485e05d9c4fc7ddbd384.jpg)
> Figure 1: Overview of UMoE. Direct SFT fine-tunes the pretrained MoE as is. UMoE first prunes the least domain-salient experts and regrows the pool from the retained experts, restoring the original size and inference cost before SFT. The same domain data D is used for calibration and training.

Q1: 这篇论文试图解决什么问题？

MoE 模型通过路由机制在不同任务间分配计算，预训练时专家学习不同的功能专化，但领域后训练（domain-specific post-training）继承了一个由混合领域预训练塑造的专家池，其中大量专家在目标领域贡献很小。标准 SFT 只更新模型参数，不改变专家组成，导致低显著性专家持续获得路由概率但仍提供边缘增益。这种冗余计算限制了领域性能上限。UMoE 旨在通过重新对齐专家池，在不增加预算的前提下释放这些冗余专家的潜力，从而提升 SFT 效果。

Q2: 有哪些相关研究？

现有工作包括：(1) Expert SFT (ESFT) 只更新与目标领域最相关的专家，冻结其余参数，但未改变专家池组成；(2) 从稠密模型转换 MoE 的方法，如 LLaMA-MoE，通过专家复制、分区和持续训练，但 UMoE 直接从已经预训练的 MoE 开始；(3) 专家剪枝和合并方法也尝试优化模型效率，但未在领域 SFT 场景中重新生长专家。UMoE 的创新在于利用扰动扩展（expert upcycling 精神）实现剪枝后恢复，从而保持原架构和计算预算。

Q3: 论文如何解决这个问题？

UMoE 包含三个步骤，仅需一次全模型前向传播的 saliency 估计：(1) 剪枝：基于领域对齐的显著性指标，剪除一半低显著性的路由专家。Saliency 度量每个专家对目标领域 token 的归一化路由概率或贡献。(2) 重扩（regrow）：对每个保留的父专家应用独立扰动（perturbation），生成新专家，使专家数恢复至原始数量。这一过程受 expert upcycling 启发，确保新初始化与父专家有区分性。(3) 标准 SFT：在重扩后的模型上进行监督微调。整个过程不改变总参数量和推理计算量（专家数不变），也不需领域的超参数调整，单一冻结配方适用所有域。

Q4: 论文做了哪些实验？

实验在两个 MoE 架构上进行：Qwen3-30B-A3B（约 30B 参数，3B 激活）和 Qwen3.5-35B-A3B（约 35B 参数，3B 激活）。覆盖五个领域：数学（benchmarks: MATH-500, GSM8K, AIME2024 等）、代码（HumanEval, MBPP, SWE-bench Verified）、科学（MMLU, GPQA）、工具使用（BFCL, API-Bank）和智能体编码（SWE-bench Agentic, InterCode）。总共 12 个基准。对比基线是直接 SFT（Direct SFT）。主要结果：在数学域，UMoE 平均超越 Direct SFT 3.4 分；在 SWE-bench Verified 上绝对提升 6.0 分。在一个内部强数学语料上，Direct SFT 已超过 Qwen3-30B-A3B-Thinking（82.81 vs 81.06），但 UMoE 进一步将平均提升至 84.17，增加 1.36 分，说明 UMoE 在强基线之上仍有效。数据规模实验（scaling experiment）显示，当训练数据从 1x 增加到 4x 时，UMoE 的增益持续存在甚至扩大。

Q5: 发现了什么实验现象？

分析发现：Direct SFT 模型将大量路由专家计算分配给了低显著性专家子集，但后验移除这些专家后平均性能仅小幅下降，表明存在冗余容量。UMoE 通过剪枝和重扩将这些冗余容量转化为有用的领域容量，从而在 SFT 过程中实现更低的训练损失。下游评估中，UMoE 的提升涵盖所有难度级别（从易到难）。此外，固着配方（frozen recipe）无需针对每个域调整超参数，即使在不同数据规模下增益仍稳健。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：(1) 更精确的剪枝标准，例如基于路由负载均衡或专家间的协作程度；(2) 自适应剪枝比例，而非固定一半；(3) 将 UMoE 扩展到其他 MoE 变体（如 DeepSeek-MoE, X-MoE）；(4) 结合强化学习或偏好优化进行后训练；(5) 探索多领域场景下如何动态调整专家池；(6) 跨领域迁移时 UMoE 策略是否可持续。

Q7: 总结一下论文的主要内容

本文提出 UMoE，一种简单的预算不变流水线，旨在解决 MoE 大模型领域后训练中专家池与目标领域不匹配的问题。UMoE 分三步：（1）基于领域显著性剪枝一半路由专家；（2）通过扰动扩展从保留专家重扩回原规模；（3）应用标准 SFT。该方法不改变专家数、参数量和推理成本，且无领域超参数调整。在 Qwen3-30B-A3B 和 Qwen3.5-35B-A3B 上，覆盖数学、代码、科学、工具使用和智能体编码五个领域共 12 个基准，UMoE 一致优于直接 SFT，典型提升包括数学平均 3.4 分，SWE-bench Verified 6.0 分。数据扩展实验显示增益持续。分析揭示直接 SFT 存在专家分配冗余，UMoE 通过重新对齐将冗余容量转化为有效领域容量。本文方法简单高效，为 MoE 模型领域适应提供了新思路。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文提出领域特异性专家池重排方法，直接适用于智能体（Agent）和AI for Science等领域特定场景。

## 基本信息

- 作者：Xuefeng Li, Pengfei Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11444v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本字段构造时优先参考了PDF语义检索命中的证据片段，并结合heuristic_draft进行润色和补全。
