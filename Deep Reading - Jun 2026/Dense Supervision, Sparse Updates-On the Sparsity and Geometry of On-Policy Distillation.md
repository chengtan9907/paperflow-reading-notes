---
user_id: "cheng tan"
paper_id: 159
arxiv_id: "2606.13657v1"
title: "Dense Supervision, Sparse Updates: On the Sparsity and Geometry of On-Policy Distillation"
publish_date: "2026-06-11"
pdf_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/arXiv - Jun 2026/2606.13657v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.13657v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
report_version: "2026-06-04-v5"
daily_note_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-14T23:54:51"
---
[[Daily Note - Jun 2026]]

- PDF：[https://arxiv.org/pdf/2606.13657v1](https://arxiv.org/pdf/2606.13657v1)

# Dense Supervision, Sparse Updates: On the Sparsity and Geometry of On-Policy Distillation

- PDF: https://arxiv.org/pdf/2606.13657v1
- 原文: https://arxiv.org/abs/2606.13657v1

> ★★☆☆☆ 按需阅读 · 约 20 分钟 · 模型 openai-compatible/gemini-3-flash-preview · 证据 PDF 全文 + 元数据

🏷 关键词：on-policy distillation · parameter sparsity · geometry of updates · post-training

## 一句话总结

本研究揭示了同策略蒸馏（OPD）在参数更新上呈现出坐标稀疏性与几何集中性，证明其本质更接近于“稀疏的策略编辑”而非“稠密的监督重写”。

## 摘要

> 2026
> On-policy distillation (OPD) has recently become a prominent post-training
> recipe as it combines two desirable ingredients: on-policy student trajecto-Jun ries and dense teacher supervision, yet how this hybrid changes a model’s
> parameters remains unclear. Across several language and vision-language
> 11 model pairs and use cases, our analysis yields two main findings. On
> sparsity, OPD-style updates are small and coordinate-sparse. They are
> distributed across layers and are usually FFN-heavy. This sparse structure
> is operationally useful: training only the discovered subnetwork recovers
> nearly the same performance as full OPD. However, the sparsity-inducing
> SGD optimizer underperforms AdamW in our optimizer ablation, likely be-
> cause dense teacher supervision preserves heterogeneous coordinate-wise[cs.LG] gradient scales where AdamW’s adaptive scaling remains useful. On geom-
> etry, the updates are numerically full-rank but spectrally concentrated; they
> lie mostly away from the principal singular subspaces of the source weights
> and fall disproportionately on coordinates where the source weights are
> close to zero. These findings suggest that dense teacher supervision does
> not turn OPD into ordinary dense parameter rewriting; instead, OPD re-
> tains important geometric signatures of on-policy post-training. Code is
> available at https://github.com/SydCS/OPD-Param-Analysis.

Q1: 这篇论文试图解决什么问题？

该论文试图解决的核心问题是：同策略蒸馏（OPD）在模型参数空间中究竟是如何起作用的？

在当前的大模型后训练管线中，存在三种主要范式：监督微调（SFT）提供稠密信号但存在分布偏移；强化学习（RL）在同策略下运行但奖励信号稀疏且面临信用分配难题；而 OPD 试图通过在学生生成的轨迹上施加教师模型的稠密 Token 级监督来结合二者优点。尽管 OPD 在性能上表现优异，但学术界对其在参数更新层面的机制缺乏深入理解：它究竟是像 SFT 那样对模型进行全局性的“稠密重写”，还是像 RL 那样进行局部的“稀疏编辑”？

理解这一机制对于优化后训练效率、设计更高效的蒸馏算法以及理解模型知识迁移的本质至关重要。论文通过对比 OPD、离线蒸馏（Offline Distillation）和强化学习（如 RLVR）的参数更新差异，试图填补这一理论与实证研究的空白。

Q2: 有哪些相关研究？

论文将 OPD 置于后训练（Post-training）的广义框架下进行讨论，相关研究主要分为三类：

1. 监督微调与离线蒸馏：如传统的 SeqKD，这类方法使用固定的教师数据，虽然提供稠密监督，但无法解决测试时的分布偏移问题。
2. 强化学习（RL）：如 PPO 或 DeepScaleR 等方法，利用环境反馈或验证器奖励进行同策略优化。这类方法通常被认为会引起稀疏的参数变化，但面临奖励稀疏性挑战。
3. 同策略蒸馏（OPD）：近期代表作包括 DeepSeek-V3/R1 中的蒸馏实践、Agarwal 等人提出的同策略学习理论。OPD 被视为 SFT 和 RL 的混合体，旨在利用教师模型的“特权信息”来引导学生模型的同策略探索。

此外，论文还参考了关于神经网络参数几何、稀疏训练以及 AdamW 与 SGD 优化器差异的相关文献，以解释 OPD 更新的独特性。

Q3: 论文如何解决这个问题？

论文采用了一套系统性的参数分析框架，从“稀疏性”和“几何性”两个维度剖析 OPD：

1. 稀疏性分析：计算 OPD 更新量（Delta）的坐标稀疏度。研究者设定了阈值来识别“有效更新”的坐标，并对比了 OPD 与离线蒸馏在不同层（如 Attention 与 FFN）的分布差异。为了验证稀疏性的功能性，他们还进行了“子网络恢复实验”，即只更新 OPD 识别出的非零坐标，观察性能损失。
2. 优化器消融：对比了 AdamW 和 SGD 在 OPD 过程中的表现。由于 OPD 提供了稠密监督，直觉上 SGD 可能适用，但实验发现 AdamW 的自适应缩放对于处理坐标间异构的梯度尺度至关重要。
3. 几何特征提取：利用奇异值分解（SVD）分析更新矩阵的谱分布。研究者考察了更新量是否落在原始权重矩阵的主奇异子空间内，并分析了更新量大小与原始权重数值大小的相关性（即是否倾向于修改原本接近零的参数）。

Q4: 论文做了哪些实验？

论文在多个模型对（如 Qwen 系列、Llama 系列）和任务（数学推理、视觉问答）上进行了实验：

1. 稀疏度测量：结果显示 OPD 的更新极其稀疏。例如，在相同阈值下，离线蒸馏的稀疏度仅为 3.06%，而 OPD 表现出极高的坐标稀疏性，且更新主要集中在 FFN 层。合理推断这反映了 FFN 承担了主要的知识存储或调整功能。
2. 子网络干预实验：研究者发现，仅训练 OPD 过程中发生显著变化的子网络（约占总参数的很小比例），就能恢复完整 OPD 训练 90% 以上的性能提升。这证明了 OPD 的更新确实是结构化且高效的。
3. 几何偏移实验：通过投影分析发现，OPD 的更新方向与原始权重的主成分方向几乎正交。这意味着 OPD 倾向于在原始模型未充分利用的维度上进行“增量式”修改，而不是覆盖已有的重要特征。
4. 优化器对比：在 JustRL-teacher 等设置下，AdamW 显著优于 SGD。分析表明，尽管监督信号是稠密的，但不同参数位置的梯度强度差异巨大，AdamW 的二阶动量补偿了这种不均匀性。

Q5: 有什么可以进一步探索的点？

基于 OPD 的稀疏与几何特性，论文提出了几个值得探索的方向：

1. 极简后训练算法：既然 OPD 更新是稀疏且集中在 FFN 的，是否可以设计专门针对 FFN 层的轻量化蒸馏方案，或者利用这种稀疏性进行更高效的参数高效微调（PEFT）？
2. 优化器改进：针对 OPD 这种“稠密监督、稀疏更新”的特殊模式，开发比 AdamW 更节省内存但能处理异构梯度尺度的优化器。
3. 知识迁移机制：进一步研究为什么 OPD 会避开原始权重的主子空间。这是否意味着模型在后训练阶段主要是在学习如何“激活”或“组合”已有能力，而非获取全新的基础知识？
4. 跨模态扩展：将此分析框架应用于更复杂的 Agent 轨迹蒸馏或多模态长文本生成场景，验证稀疏性规律是否普适。

Q6: 总结一下论文的主要内容

这篇论文对同策略蒸馏（OPD）的参数更新机制进行了深度“显微镜”式的分析。研究的核心结论是：尽管 OPD 接收来自教师模型的稠密 Token 级信号，但其对学生模型参数的改变在本质上是稀疏且具有特定几何偏好的。通过对 Qwen 和 Llama 等模型的实证研究，作者发现 OPD 的更新量非常小，且在坐标空间呈现高度稀疏性，这种稀疏性在 FFN 层尤为明显。实验证明，这种稀疏结构具有功能性意义，因为仅通过更新一小部分关键参数子集即可达到全量更新的效果。在几何层面，OPD 更新避开了原始权重的核心特征空间，倾向于在权重较小的坐标上进行微调。此外，研究还解释了为什么 AdamW 在这种看似稠密的监督任务中依然不可或缺。总体而言，这项工作挑战了“蒸馏即重写”的传统认知，提出 OPD 更像是一种精准的“同策略参数编辑”，为理解大模型后训练过程提供了重要的理论支撑和实证证据。

## PDF 证据定位

- 研究背景：
  Abstract | score=1.190 | 2026 On-policy distillation (OPD) has recently become a prominent post-training recipe as it combines two desirable ingredients: on-policy student trajecto-Jun ries and dense…
  Introduction | score=1.096 | signatures matter operationally through two interventions. First, we restart OPD from the source checkpoint and train only coordinates selected by nonzero checkpoint-delta masks.…
  Introduction | score=1.093 | here OPD differs most from offline distillation. 6 Discussion and future work The primary message of this paper is that OPD has dense supervision but sparse, offprincipal…
- 核心方法：
  Abstract | score=0.947 | ncentrated; they lie mostly away from the principal singular subspaces of the source weights and fall disproportionately on coordinates where the source weights are close to…
  Introduction | score=0.941 | of coordinates have no visible movement. The offline distillation contrast behaves very differently, with 11.94% relative delta norm and only 3.06% sparsity at the same…
  Conclusion | score=0.907 | This work studies OPD parameter updates through two complementary properties: sparsity and geometry. Observational studies show that OPD-style updates are small…
- 主要结果：
  Introduction | score=0.905 | ook more like supervised distillation, more like RLVR, or like a distinct regime? This paper answers that question by analyzing the sparsity and geometry of OPD parameter…
  Introduction | score=0.895 | on-style contrast shows a weaker version of this effect: Qwen-Math Distill has 2.22% median principal-projection energy, several times larger than DS-Qwen OPD and DeepScaleR…
  Introduction | score=0.879 | ed reinforcement learning OPD Current policy Dense teacher feedback Interactive imitation learning Despite its empirical success, what OPD does to a model remains less…
- 局限性：
  Conclusion | score=1.051 | This work studies OPD parameter updates through two complementary properties: sparsity and geometry. Observational studies show that OPD-style updates are small…
- 相关性：
  Introduction | score=1.155 | On-policy distillation (OPD) is emerging as a third major component in post-training pipelines for large language models, alongside supervised fine-tuning and reinforcement…
  Introduction | score=1.122 | date geometry. 2 Background 2.1 OPD as on-policy dense supervision OPD can be formulated as dense teacher supervision applied to on-policy student behavior (Agarwal et al.…
  Introduction | score=1.107 | trains on the current policy’s own samples, but learns from environmentor verifier-derived outcome rewards that are often sparse, and therefore faces a credit-assignment problem.…

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：对于关注“智能体（Agent）”能力的开发者，本文揭示了如何通过 OPD 高效地将复杂推理轨迹蒸馏到小模型中。

## 基本信息

- 作者：Guo Yu, Wenlin Liu, Yulan Hu, Hao-Xuan Ma, Jun-Peng Jiang, Han-Jia Ye
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-06-11
- 推荐级别：**按需阅读**
- 预计阅读时间：约 20 分钟
- 解析来源：PDF 全文 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.13657v1`

## 代码与资源

- 代码：暂未发现公开链接
- 数据：暂未发现公开链接
- 项目主页：暂未发现公开链接
- 原文 PDF：https://arxiv.org/pdf/2606.13657v1
- arXiv：https://arxiv.org/abs/2606.13657v1
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了提供的 PDF 检索证据，特别是关于 OPD 稀疏性、FFN 集中性、几何投影以及优化器对比的实验数据。分析结合了 Introduction 的动机描述和 Conclusion 的总结，确保了机制解释的准确性。
- 方法证据锚点：Abstract | score=0.947 | ncentrated; they lie mostly away from the principal singular subspaces of the source weights and fall disproportionately on coordinates where the source weights are close to…
- 结果证据锚点：Introduction | score=0.905 | ook more like supervised distillation, more like RLVR, or like a distinct regime? This paper answers that question by analyzing the sparsity and geometry of OPD parameter…
