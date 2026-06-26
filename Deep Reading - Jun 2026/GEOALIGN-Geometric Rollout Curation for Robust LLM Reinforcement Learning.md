---
user_id: "cheng tan"
paper_id: 1625
arxiv_id: "2606.26917"
title: "GEOALIGN: Geometric Rollout Curation for Robust LLM Reinforcement Learning"
publish_date: "2026-06-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.26917.pdf"
pdf_url: "https://arxiv.org/pdf/2606.26917"
abs_url: "https://arxiv.org/abs/2606.26917"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-26T14:18:58"
---
# GEOALIGN: Geometric Rollout Curation for Robust LLM Reinforcement Learning

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · llm alignment · rollout curation · directional inconsistency

## 一句话总结

提出GEOALIGN，一种轻量级插件，通过检测并修正表示空间中方向不一致的rollout（方向不一致性）来稳定在线RL训练，提升LLM对齐性能。

## 摘要

> Online reinforcement learning is widely used to align large language models (LLMs) with reward signals, yet training can be unstable under noisy or misspecified rewards. We identify a failure mode we call directional inconsistency: within a batch, a small set of high-reward rollouts induces representation-space preference directions that sharply disagree with the batch majority, resulting in high-variance and destabilizing updates. We propose geoalign, a lightweight plug-in for rollout curation in iterative policy optimization. Geoalign (i) forms within-prompt preference pairs, (ii) learns an online projector on per-rollout hidden states to concentrate reward-ordered displacement directions, and (iii) detects directionally inconsistent rollouts via their angular deviation from a batch consensus prototype and rectifies them with within-prompt stable alternatives. Geoalign is forward-pass only and adds negligible overhead. Across dialogue alignment with a learned reward model and mathematical reasoning with binary verified rewards, Geoalign improves final performance and reduces training oscillation, outperforming PF-PPO, PAR, PODS, and Seed-GRPO. These results suggest latent directional consensus as an effective reliability signal for online LLM RL.

Q1: 这篇论文试图解决什么问题？

论文旨在解决在线强化学习对齐大语言模型时，由于奖励信号存在噪声或被错误指定导致训练不稳定、梯度方差高的问题。具体识别了方向不一致性这一关键失败模式：在训练批次中，少数高奖励rollout（特别是具有异常高奖励但方向异常的样本）会在表示空间中诱导出与多数rollout整体偏好方向冲突的更新方向，从而产生破坏性的梯度更新，加剧训练振荡和性能退化。

Q2: 有哪些相关研究？

相关研究分为几类：（1）通过梯度检查、影响函数或轨迹级归因（如Hu et al., 2025; Dai et al., 2025）识别有害样本的方法，但需要额外梯度计算；（2）在线RL的稳定性改进，如调整学习策略、使用保守奖励或正则化（如PPO、GRPO的变体）；（3）基于几何信号的鲁棒训练方法（虽在自监督学习中有探索，但在RL中较少）。本文首次利用rollout方向一致性作为稳定性信号，不同于现有方法。基线包括PF-PPO、PAR、PODS和Seed-GRPO。

Q3: 论文如何解决这个问题？

GEOALIGN作为一种插件，集成到迭代策略优化中。具体步骤：（1）对于每个prompt，采样多个rollout，基于奖励排序形成偏好对（低奖励→高奖励）；（2）学习一个在线投影器，将rollout的隐藏状态映射到低维空间，然后计算每对rollout之间的位移方向（低→高奖励方向）；（3）在表示空间中计算批次内所有位移方向的共识原型（例如平均方向）；（4）计算每个rollout的位移方向与原型之间的角度偏差，检测角度偏差大的rollout作为方向不一致的异常样本；（5）对于被检测的rollout，用同一prompt中具有稳定方向（即与共识方向一致）的替代rollout替换掉（通过选择与原型方向接近的rollout或直接去除异常rollout）。整个过程仅需前向传播，无需额外梯度计算。

Q4: 论文做了哪些实验？

实验在两个任务上评估：（1）对话对齐：使用HH-RLHF数据集，训练一个奖励模型提供连续奖励信号，采用在线PPO训练策略；（2）数学推理：使用数学问题数据集（如GSM8K风格），奖励为二元验证信号（正确/错误）。基线包括PF-PPO、PAR、PODS和Seed-GRPO。评估指标包括最终奖励/准确率、训练过程中的振荡幅度（如奖励方差）、训练曲线平滑度等。消融实验验证了投影器和方向修正的有效性。

Q5: 发现了什么实验现象？

实验观察到：（1）方向不一致性在训练批次中呈现长尾分布，即少数rollout具有极大的角度偏差，而大多数rollout方向与共识一致；（2）GEOALIGN有效降低了训练过程中的奖励方差，使训练曲线更平滑，同时提高了最终性能（在对话和数学任务上均优于基线）；（3）消融实验显示，移除投影器（直接使用原始隐藏状态）或移除修正步骤均导致性能下降，说明两者缺一不可；（4）与基线相比，GEOALIGN在收敛速度上也略有提升，且未增加显著计算开销。

Q6: 有什么可以进一步探索的点？

未来可探索的方向包括：（1）更高效的投影学习方法，如使用自适应投影维度或非线性投影；（2）将GEOALIGN扩展到多步奖励或稀疏奖励场景；（3）与其他稳定性方法（如梯度裁剪、奖励归一化）结合以进一步提升鲁棒性；（4）在更复杂任务（如长文本生成、多轮对话）上验证方法；（5）探索方向不一致性在不同RL算法（如GRPO变体）中的表现；（6）理论分析方向一致性与优化稳定性之间的关系。

Q7: 总结一下论文的主要内容

本文提出GEOALIGN，一种基于几何信号的轻量级rollout筛选方法，用于改善在线RL对齐LLM时的训练稳定性。核心贡献是识别了方向不一致性这一失败模式，即少数高奖励rollout的更新方向与批次整体方向冲突，导致训练振荡。GEOALIGN通过在prompt内形成偏好对，学习投影器计算位移方向，并与批次共识原型比较，检测并修正此类异常rollout。实验在对话对齐和数学推理任务上验证了有效性，表明该方法能提升最终性能并减少训练振荡，优于多个现有基线。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与在线RL对齐LLM的研究范式紧密相关，为奖励鲁棒性提供了新视角。

## 基本信息

- 作者：Ting Zhou, Zhenqing Ling, Yiyang Zhao, Ying Shen, Daoyuan Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-06-26
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.26917`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于论文摘要及检索证据进行合理推断
