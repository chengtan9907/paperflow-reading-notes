---
user_id: "cheng tan"
paper_id: 2151
arxiv_id: "2606.32026v1"
title: "AdaJEPA: An Adaptive Latent World Model"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.32026v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.32026v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T01:37:01"
---
# AdaJEPA: An Adaptive Latent World Model

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：test-time adaptation · world model · joint-embedding predictive architecture · model predictive control

## 一句话总结

AdaJEPA是一个在模型预测控制闭环中进行测试时自适应的潜世界模型，通过计划-执行-适应-重新规划循环持续校准世界模型。

## 摘要

> Latent world models enable planning from high-dimensional observations by predicting future states in a compact latent space. However, these models are typically kept frozen at test time: when their predictions become inaccurate, planning can fail, especially under test-time distribution shift. To address this, we propose AdaJEPA, an adaptive latent world model that performs test-time adaptation within the closed loop of model predictive control (MPC). After training, AdaJEPA plans and executes the first action chunk, uses the observed next-state transition as a self-supervised adaptation signal, and replans with the updated model. This closed-loop update continuously recalibrates the world model without additional expert demonstrations. Across a range of goal-reaching tasks, AdaJEPA substantially improves planning success with as few as one gradient step per MPC replanning step.

Q1: 这篇论文试图解决什么问题？

潜世界模型（如Joint-Embedding Predictive Architectures, JEPAs）在高维观测空间中学习紧凑的潜在动力学，用于规划。然而，现有方法在测试时保持模型参数冻结，当环境出现分布偏移（如新障碍物、不同的物理参数）时，模型的预测精度下降，导致规划失败。更根本的问题在于，规划和控制本质上是闭环过程，但世界模型却以开环方式使用。因此，需要在测试时在线自适应，但传统在线适应通常需要真实动作标签或专家演示，这在部署中难以获取。本文旨在解决：如何在不依赖外部信号的情况下，让潜世界模型在测试时利用自身预测与观测的差异进行自我校准，从而提升长期规划鲁棒性。

Q2: 有哪些相关研究？

潜世界模型领域的工作包括Dreamer、MuZero、Plan2Explore等方法，它们学习潜在状态转移模型以支持规划。JEPA系列（如VICReg、Self-Attention-based JEPA）通过联合嵌入预测架构学习抽象表示。然而，这些模型在测试时均冻结参数，不进行在线适应。测试时自适应（test-time adaptation）在图像分类（如Tent, SHOT）和强化学习中有应用，但通常需要损失函数或辅助任务。本文首次将测试时自适应引入潜世界模型规划中，利用MPC闭环中自然产生的下一状态观测作为自监督信号，无需专家数据。相关工作推测还包括元学习（MAML）和在线超参数调整，但论文未直接提及，建议读者参考原始论文Related Work部分。

Q3: 论文如何解决这个问题？

AdaJEPA的核心是“计划-执行-适应-重新规划”闭环。具体过程：1）使用当前世界模型（编码器+预测器）在潜在空间中规划（例如通过梯度下降或交叉熵方法CEM）得到动作块；2）在环境中执行第一个动作；3）观测环境返回的新状态（高维观测），将其编码为潜在表示；4）以自监督方式更新模型：最小化预测潜在状态与实际编码潜在状态之间的差异（如均方误差）。该步骤仅需少量梯度步（如1步），计算与样本高效。更新后的模型用于下一轮规划。这种设计利用了MPC天然的重规划结构，将适应融入控制循环。模型架构基于JEPA：编码器将观测映射到潜在表示，预测器在潜在空间预测未来状态。训练阶段使用标准的JEPA目标（如VICReg变体），测试时自适应则持续微调。

Q4: 论文做了哪些实验？

在PushT（推块）任务上评估，涉及将物体推至目标位置。使用Simulator和真实环境（模拟/实物）。比较的基线包括：冻结的JEPA世界模型（VJEPA）、随机动作、以及不同规划器（GD规划器、CEM规划器）。AdaJEPA的变体包括仅适应编码器、仅适应预测器或两者。主要指标：规划成功率（目标到达率）和达到目标所需的重新规划次数。在Table 2中报告了结果，显示AdaJEPA在所有配置下均优于冻结基线。消融实验：测试不同适应步数（1, 5, 10步）的效果，发现1步即可达到大部分性能增益；适应组件的影响（编码器 vs 预测器）；不同训练目标的影响（如对比损失 vs 均方误差）。附录A.1提供了详细实验设置，包括数据收集、超参数等。

Q5: 发现了什么实验现象？

1）AdaJEPA在PushT验证轨迹上显著提升了规划成功率，无论使用全局表示还是空间表示、不同JEPA架构、不同规划器（GD或CEM）以及不同训练目标，提升一致。2）自适应仅需1个梯度步即可带来大部分收益，更多的梯度步（如5、10）收益递减，表明适应高效且无需大量计算。3）适应编码器与适应预测器均有效，但同时适应两者效果最佳。4）相比基线，AdaJEPA在相等或更少的重新规划次数内达到目标，表明自适应提高了预测准确性和规划效率。5）可视化显示，自适应后潜在表示更准确地对齐了实际轨迹。无负结果或明显失败案例报告，但推测在快速变化的动力学或极少量观测下可能失效（原文未明确）。

Q6: 有什么可以进一步探索的点？

1）将AdaJEPA扩展到更复杂的操作任务（如多物体操控、变形物体）以及更长期的规划（超过10步）。2）探索自适应机制的理论性质，如收敛性、适应性如何影响规划偏差。3）结合其他测试时自适应方法（如同时优化规划器参数与模型参数）。4）在完全分布式或部分可观测环境下评估适应性。5）减少自适应对计算的需求，例如通过知识蒸馏或元初始化。6）将闭环适应思想扩展到其他潜世界模型（如Dreamer的变体）。7）研究自适应信号的质量：如果新观测本身是噪声或异常值，如何鲁棒地适应。

Q7: 总结一下论文的主要内容

本文提出AdaJEPA，一种基于联合嵌入预测架构（JEPA）的自适应潜世界模型，用于在模型预测控制闭环中进行测试时自适应。与其他测试时保持冻结的世界模型不同，AdaJEPA在执行动作后，利用新观测作为自监督信号，通过少量梯度更新来校准预测模型。该方法仅需极少的计算开销（1个梯度步），无需依赖专家演示或真实标签。实验在PushT任务上进行，涵盖多种JEPA变体和规划器，结果显示AdaJEPA持续提升规划成功率，且效率高。论文的主要贡献包括：1）提出并实现了测试时自适应潜世界模型的框架；2）在MPC闭环中自然融入自适应，形成plan–execute–adapt–replan循环；3）通过大量消融实验证明了自适应的有效性和高效性。局限方面，论文未讨论在极不稳定的环境或自适应信号噪声过大时的表现，且实验仅聚焦于单一任务（PushT）。潜在扩展包括更复杂的环境、更长规划 horizon 以及与其他在线学习方法的结合。总体而言，AdaJEPA为提升潜世界模型在部署时的鲁棒性提供了一个简单且有效的方案。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文方法对测试时自适应和潜世界模型的交叉领域具有直接参考价值。

## 基本信息

- 作者：Ying Wang, Oumayma Bounou, Yann LeCun, Mengye Ren
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.32026v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告主要参考了PDF检索证据（abstract、conclusion、results等片段）以及heuristic_draft，对方法进行详细阐述，但对related work和future directions部分因证据不足进行了合理推断和补充。
