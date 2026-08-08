---
user_id: "cheng tan"
paper_id: 6461
arxiv_id: "2608.03573v1"
title: "SFT Conflicts, RL Coexists: A Theoretical and Empirical Analysis of Multi-Task Learning for LLMs"
publish_date: "2026-08-04"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.03573v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.03573v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-06T01:44:02"
---
# SFT Conflicts, RL Coexists: A Theoretical and Empirical Analysis of Multi-Task Learning for LLMs

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：supervised fine-tuning · reinforcement learning · multi-task learning · gradient interference

## 一句话总结

本文通过多任务训练实验发现SFT在多阶段训练中遭受严重任务冲突而RL能够实现稳定共存，并从参数级稀疏性与近似正交更新、梯度干扰的范数受限与方差受限机制给出理论与实证解释，进而提出Parallel-RL解耦多任务训练范式。

## 摘要

> Supervised Fine-Tuning (SFT) and Reinforcement Learning (RL) exhibit fundamentally different behaviors in enhancing multi-task reasoning for large language models (LLMs). Our preliminary experiments revealed a phenomenon: SFT suffers from severe task conflicts under multi-stage training, whereas RL enables stable coexistence across diverse tasks. Empirically, we trace this to the parameter level, observing that RL induces sparse and approximately orthogonal updates across tasks. We provide a theoretical explanation for this mechanism by analyzing multi-task gradient interference. Our results reveal a distinction: interference in SFT is norm-limited, scaling with the absolute gradient magnitude, whereas interference in RL is variance-limited, bounded by the gradient variance induced by advantage normalization and on-policy optimization. This small variance bound yields near-orthogonal optimization directions across tasks. Leveraging this insight, we propose Parallel-RL, a paradigm that decouples multi-task training, significantly improving efficiency and flexibility.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在LLM的后训练阶段，为什么SFT与RL在多任务学习（特别是多阶段训练）场景下表现出截然不同的行为——SFT出现严重的任务冲突，而RL则能实现任务共存？具体来说，问题可分为三个层次：
1. 现象层：当使用混合数据或多阶段策略进行多任务训练时，SFT与RL的性能变化差异巨大，SFT在优化某一能力时会严重损害其他已有能力，而RL能够在学习新任务的同时保留旧任务性能。
2. 机制层：这种差异背后的参数更新与梯度干扰机制是什么？作者观察到RL的跨任务参数更新具有稀疏性和近似正交性，而SFT则不然，需要从理论上解释这两种训练范式下梯度干扰的根本区别。
3. 方法层：能否利用RL的这种共存特性，设计一种更高效、更灵活的多任务训练范式，从而克服传统多任务SFT中的冲突与低效问题。

Q2: 有哪些相关研究？

根据检索到的片段，论文的相关研究背景覆盖三大部分：
1. 后训练范式（Post-Training for Large Language Models）：主流方法包括SFT与RL（如Jaech et al., 2024; Guo et al., 2025），近期研究聚焦如何通过后训练增强推理能力。
2. SFT与RL的对比分析：已有工作比较两种范式在数据效率、泛化、对齐等方面的差异，但较少从多任务干扰的角度进行系统性理论分析。
3. 多任务学习（Multi-Task Learning）：传统多任务学习面临梯度冲突、灾难性遗忘等问题，但该论文揭示RL在多阶段训练下能规避这类冲突，与经典结论形成对照。
此外，论文还涉及参数更新稀疏性、梯度正交性等优化理论相关概念。由于检索证据仅覆盖综述性内容，具体引用细节需要回原文确认。

Q3: 论文如何解决这个问题？

论文采用“经验现象发现→参数级观测→理论解释→新方法设计”的研究路径：
1. 首先通过初步实验对比SFT与RL在混合数据和多阶段训练下的性能，明确“SFT冲突、RL共存”的现象。
2. 然后在参数层面分析RL与SFT的更新模式，发现RL产生稀疏且近似正交的跨任务更新，而SFT则存在显著梯度干扰。
3. 提出理论解释：将多任务梯度干扰区分为“范数受限”（norm-limited）与“方差受限”（variance-limited）。SFT的干扰由绝对梯度幅值决定，因此任务间梯度大时冲突严重；RL经过优势归一化和在策略优化后，梯度方差被限制在较小范围内，从而跨任务更新方向近似正交，实现共存。
4. 基于该理论，提出Parallel-RL范式：通过解耦多任务训练，使各任务可以并行或模块化训练，无需混合数据或复杂的任务调度，显著提升训练效率与灵活性，并以最小适配获得更优性能。

Q4: 论文做了哪些实验？

根据摘要和检索片段，论文的实验设计包括：
1. 多任务训练策略对比：测试SFT与RL在“混合数据”（mixed-data）和“多阶段”（multi-stage）两种策略下的性能差异，结果见表1，显示SFT在两种策略下均出现严重冲突，而RL保持稳定。
2. 参数级分析：观察训练过程中参数更新的稀疏性和正交性，对比两种范式的跨任务更新轨迹。
3. 理论验证：通过梯度干扰的数学分析，证明SFT干扰的范数受限性与RL干扰的方差受限性，并验证该理论对现象的解释力。
4. 方法验证：对提出的Parallel-RL进行实证测试，评估其在多任务推理上的性能、效率与灵活性（具体实验设置、数据集与baseline在检索证据中未详细呈现，需参考原文）。

Q5: 发现了什么实验现象？

论文揭示了以下关键实验现象：
1. SFT任务冲突：在多阶段训练中，SFT优化新任务会显著损害已训练任务的能力，且混合数据也无法避免冲突；这种冲突随训练任务增多而加剧（合理推断）。
2. RL任务共存：RL在多阶段训练中表现出鲁棒性提升，检索片段提到“RL demonstrates robustness improvement (with gains of 24.9%) in multi-stage settings”，即多阶段RL相较单一阶段有24.9%的性能增益，说明RL不仅不遗忘旧任务，还能在持续学习中获益。
3. 参数更新差异：RL的跨任务参数更新稀疏且近似正交，而SFT的更新密集且方向相关，这从参数层面解释了为什么RL能减少干扰。
4. 干扰来源不同：梯度干扰在SFT中受绝对梯度范数控制，在RL中受优势归一化与在策略优化引入的方差控制，且该方差上界很小，使得RL在多任务中的优化方向近似正交。
5. 理论预测与实验一致：文中理论与实证结果相互印证，支撑“SFT Conflicts, RL Coexists”的结论。

Q6: 有什么可以进一步探索的点？

基于本文工作，可以进一步探索的方向包括：
1. 扩展任务类型：当前研究聚焦推理任务，可验证在其他类型任务（如生成、分类、代码）中SFT冲突与RL共存的现象是否成立。
2. 理论深化：当前理论基于梯度干扰的范数/方差界定，可以进一步推导更精确的冲突上界，或考虑二阶优化、自适应优化器的影响。
3. Parallel-RL的扩展：探索Parallel-RL在不同模型规模（如超过千亿参数）、不同RL算法（如PPO变体、DPO等）下的适用性，并研究任务划分与并行调度的最优策略。
4. 与多任务SFT改良结合：将RL共存特性的理论洞见迁移到SFT，例如通过梯度投影或归一化来模拟方差受限效果，从而缓解SFT冲突。
5. 灾难性遗忘的更系统研究：借助本文框架，对持续学习中的遗忘与增益现象进行更细粒度的分析。
6. 实际应用：将Parallel-RL应用于智能体训练或多智能体协作场景，验证其在复杂实际任务中的效率与稳定性提升。

Q7: 总结一下论文的主要内容

本文系统分析了SFT与RL在多任务训练中的根本差异。作者首先通过初步实验发现一个鲜明现象：当LLM接受多阶段训练（依次学习不同任务）时，SFT会引发严重的任务冲突——优化新能力会极大破坏已有能力；而RL则展现稳定的任务共存——学习新任务的同时能保留甚至提升旧任务性能。为理解这一差异，作者从参数层面展开分析，观察到RL跨任务产生的参数更新是稀疏且近似正交的，而SFT的更新则存在明显的梯度干扰。为解释上述机制，作者对多任务梯度干扰进行理论分析，提出关键区分：SFT的干扰是范数受限的，其强度随绝对梯度幅值增长，因此任务间梯度大时冲突不可避免；RL的干扰则是方差受限的，借助优势归一化和在策略优化，RL的梯度方差被限制在很小的范围内，这导致不同任务的优化方向近似正交，从而天然避免冲突。这一理论不仅解释了实证观察，也为设计新方法奠定了基础。基于该洞见，作者提出Parallel-RL——一种解耦多任务训练的范式。与传统混合数据或多阶段SFT不同，Parallel-RL允许各任务独立或并行训练，再以模块化方式组合，极大提升了训练效率和灵活性，并以最少的适配获得甚至超越传统方法的性能。实验显示RL在多阶段训练中能带来24.9%的鲁棒性提升，进一步验证了共存特性的实际价值。总体而言，本文在现象、机制、理论和方法四个层面系统论证了“SFT冲突、RL共存”这一核心结论，为LLM多任务后训练提供了新的理论视角与实用方案。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文对LLM后训练范式（SFT vs RL）的对比分析，对智能体训练中多技能整合具有参考价值。

## 基本信息

- 作者：Kejian Zhu, Zhuoran Jin, Shangqing Tu, Hongbang Yuan, Yushi Bai, Kang Liu, Juanzi Li, Jun Zhao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-08-04
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.03573v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的Abstract、Introduction、Conclusion及Related Work等片段，并结合论文元数据进行了补全；部分详细实验数值与设置因证据不足未编造。
