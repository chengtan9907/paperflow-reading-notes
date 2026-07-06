---
user_id: "cheng tan"
paper_id: 2491
arxiv_id: "2607.01763"
title: "Denser $\\neq$ Better: Limits of On-Policy Self-Distillation for Continual Post-Training"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.01763.pdf"
pdf_url: "https://arxiv.org/pdf/2607.01763"
abs_url: "https://arxiv.org/abs/2607.01763"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-06T12:44:58"
---
# Denser $\neq$ Better: Limits of On-Policy Self-Distillation for Continual Post-Training

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：continual post-training · on-policy self-distillation · self-distillation policy optimization · forgetting

## 一句话总结

本文通过自蒸馏策略优化（SDPO）深入评估on-policy自蒸馏在持续后训练中的效果，发现其仅在教师信号稳定时能加速领域内特化，但会加剧遗忘和崩溃，而GRPO等强化学习方法更稳定地保持已有能力。

## 摘要

> Continual post-training enables foundation models to acquire new knowledge while preserving existing capabilities. Recent work suggests that on-policy learning can mitigate forgetting, with on-policy self-distillation emerging as a particularly attractive approach. In this work, we revisit this optimistic view through self-distillation policy optimization (SDPO). Our experiments show that SDPO can accelerate in-domain specialization when teacher signals are stable and well aligned, but it struggles to generalize to out-of-distribution scenarios. In continual post-training, SDPO exhibits stronger forgetting and can even collapse, whereas on-policy reinforcement learning methods such as GRPO adapt more conservatively and better preserve prior capabilities. Further analyses reveal that denser self-distillation induces larger drift in both parameter space and response space, and can amplify high-frequency formatting artifacts through a self-reinforcing teacher--student loop. These findings suggest that on-policy data alone is insufficient for continual learning. Dense self-distillation can accelerate specialization when teacher targets are stable and token-level supervision is reliable, but it should not be treated as a default stabilizer for continual post-training. Our code is available at https://github.com/Moenupa/SDPO-CL.

Q1: 这篇论文试图解决什么问题？

本文旨在解答以下问题：在持续后训练场景下，on-policy自蒸馏（以SDPO为代表）是否真的如近期研究所暗示的那样能有效缓解灾难性遗忘？其在不同任务（领域内特化与分布外泛化）下的表现如何？密集的token级蒸馏监督会带来哪些副作用（如参数漂移、伪影放大）？为何on-policy数据本身不足以确保持续学习？通过系统实验和诊断工具，作者希望揭示on-policy自蒸馏的边界条件，为持续后训练算法设计提供谨慎的指导。

Q2: 有哪些相关研究？

相关研究主要集中在两个方向：持续学习中的遗忘缓解方法（特别是利用on-policy数据的方法）以及自蒸馏技术在模型训练中的应用。近期有工作主张on-policy学习能有效减少遗忘，因此on-policy自蒸馏被看作一种极具吸引力的技术路线。本文直接与这类乐观观点对话，并将SDPO与on-policy强化学习方法（如GRPO，Group Relative Policy Optimization）进行对比。此外，论文探讨了参数漂移和响应漂移等诊断概念，以及教师-学生循环可能导致的伪影放大。然而，由于原文未提供详细的文献综述，具体引用论文列表需参考原文的Related Work章节。

Q3: 论文如何解决这个问题？

本文设计了一种名为自蒸馏策略优化（SDPO）的研究框架。SDPO利用on-policy数据（即由当前策略模型生成的响应）进行逐token的自我蒸馏：学生模型计算其输出与教师模型（通常为前一版本或固定模型）输出的差异（如KL散度），以此为监督信号更新参数。该框架允许精细调节蒸馏密度（如蒸馏权重和教师更新频率）。为了全面评估，论文设置了单领域特化实验（教师信号稳定情况）和持续后训练实验（多阶段增量学习），并与off-policy蒸馏和on-policy RL方法（GRPO）横向比较。此外，作者引入了两种诊断指标：参数漂移（参数变化的L2范数）和响应漂移（输出分布变化），用于量化模型行为变化。方法的具体实现细节（如模型架构、超参数、数据集）需查阅原文。

Q4: 论文做了哪些实验？

论文进行了两类核心实验：单领域特化测试和持续后训练对比。单领域特化测试包括：在特定领域数据上训练，评估领域内性能（准确率等）和分布外泛化性能。持续后训练实验采用分阶段任务学习，比较SDPO、GRPO、off-policy蒸馏等方法在多个任务上的遗忘程度和整体性能。诊断实验包括改变蒸馏密度（如不同蒸馏权重、教师更新频率），记录参数漂移和响应漂移，并人工检查生成的文本发现格式伪影。实验可能涵盖了多个标准持续学习数据集和特定领域任务，但原文未明确列出具体数据集名称和数值结果，建议直接参考论文的实验部分。

Q5: 发现了什么实验现象？

主要观察到的实验现象包括：1. 在教师信号稳定且与目标对齐的情况下，SDPO能显著加速领域内特化，训练效率提升。2. 但在分布外（OOD）场景中，SDPO的泛化能力严重退化，性能下降明显。3. 在持续后训练设置下，SDPO导致旧任务能力大幅下降（遗忘更强），甚至出现模型完全崩溃（collapse）——模型输出随机或无意义内容。相反，GRPO方法虽然学习较慢，但能更稳定地保持先前能力。4. 更密集的自蒸馏（如增大蒸馏系数β或减少教师更新间隔）会带来更大的参数空间漂移和响应空间漂移。5. SDPO倾向于放大高频格式伪影，例如模型重复特定短语或格式化模式，形成自我强化的循环。6. 思维链（CoT）等情景下教师信号不稳定，SDPO表现更差，可能因为噪声被放大（合理推断）。7. 序列级监督（如GRPO）比token级密集监督更能抵御伪影放大。

Q6: 有什么可以进一步探索的点？

基于研究发现，未来可探索的方向包括：1. 设计稳定的教师更新机制，避免因教师快速变化引入噪声和伪影。2. 结合token级密集蒸馏和序列级RL信号，在加速特化与保持能力间取得平衡。3. 开发自适应蒸馏密度策略，根据任务难度或当前漂移水平动态调整。4. 深入理解高频伪影形成的理论原因，提出针对性正则化或去偏方法。5. 将研究扩展至更大规模模型（如数十亿参数）和更多样的持续学习场景（类别增量、任务增量等）。6. 探索在不牺牲遗忘缓解的情况下利用密集监督的其他方式，如使用收敛更快的价值函数。7. 研究是否可以借鉴因果推断，使蒸馏损失更接近真正成功的策略，而非对齐特权教师。

Q7: 总结一下论文的主要内容

本文主要研究on-policy自蒸馏在持续后训练中的实际效果和价值。作者首先指出持续后训练面临的知识获取与遗忘保持的矛盾，以及近期on-policy学习被认为能缓解遗忘的趋势。他们提出自蒸馏策略优化（SDPO）框架作为研究工具，在on-policy数据上实施密集的token级蒸馏训练。通过精心设计的单领域特化实验和多阶段持续后训练实验，论文获得了关键的对比结果：SDPO在教师信号稳定时能快速提升领域内任务表现，但无法推广到新分布且在持续学习中导致显著遗忘甚至崩溃；与之对比，on-policy RL方法GRPO收敛较慢但能可靠地保留旧知识。进一步分析发现，密集自蒸馏会增大参数和响应的漂移，并通过教师-学生循环放大高频格式伪影。基于这些现象，作者得出核心结论：on-policy数据本身不保证持续学习成功；密集自蒸馏的适用条件严格，不应作为默认稳定器。论文的贡献在于对乐观假设的实验性检验、开发了简单有效的诊断指标、并揭示了重要的失败模式，为后续研究提供了实证基础和警示。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对持续后训练中on-policy方法的有效性质疑具有重要启示。

## 基本信息

- 作者：Meng Wang, Haohan Zhao, Wenzhuo Liu, Lu Yang, Geng Liu, Haiyang Guo, Guo-Sen Xie, Gaofeng Meng, Hongbin Liu, Fei Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.01763`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了arXiv PDF的语义检索证据，并结合启发式草稿进行补全。
