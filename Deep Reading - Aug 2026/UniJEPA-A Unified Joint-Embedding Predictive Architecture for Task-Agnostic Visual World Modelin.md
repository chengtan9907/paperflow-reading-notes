---
user_id: "cheng tan"
paper_id: 7181
arxiv_id: "2608.07409"
title: "UniJEPA: A Unified Joint-Embedding Predictive Architecture for Task-Agnostic Visual World Modeling"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.07409.pdf"
pdf_url: "https://arxiv.org/pdf/2608.07409"
abs_url: "https://arxiv.org/abs/2608.07409"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-11T01:13:59"
---
# UniJEPA: A Unified Joint-Embedding Predictive Architecture for Task-Agnostic Visual World Modeling

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：joint-embedding predictive architecture · world model · self-supervised learning · visual representation learning

## 一句话总结

UniJEPA 提出一个统一的联合嵌入预测架构，在共享潜空间中同时学习图像级光度预测与视频级时序预测，通过单一防坍缩目标实现无需 EMA、stop-gradient 的自监督视觉世界模型，并支持零样本规划。

## 摘要

> Joint-Embedding Predictive Architectures (JEPAs) have emerged as a principled framework for self-supervised learning of world models in compact latent spaces, yet existing methods are fragmented: some predict masked parts of a single image in latent space (I-JEPA), others learn to predict global photometric transformations (Image World Models), while video-scale JEPAs predict future temporal states and are post-trained for action-conditioned planning (V-JEPA 2, DINO-World, DINO-WM). These objectives are treated as distinct recipes with separate encoders, predictors, and anti-collapse regularizers, hindering a single model from unifying image-level and video-level world modeling. We present UniJEPA, a unified JEPA that jointly learns photometric prediction (image-level transformations) and temporal prediction (video-level next-state dynamics) in one shared latent space. A single end-to-end objective, composed of a next-embedding prediction loss and a Gaussian regularizer, yields a provably anti-collapse encoder-predictor pair trainable from raw pixels without EMA, stop-gradient, or pre-trained encoders. We show that the same latent space supports controllable abstraction: photometric prediction learns invariant structure while temporal prediction learns equivariant dynamics. After action-conditioned post-training on offline trajectories, UniJEPA enables zero-shot planning by treating goal features as prediction targets. On image, video, and control benchmarks, UniJEPA matches or surpasses task-specific JEPAs while requiring a single loss hyperparameter, and plans up to tens of times faster than generative world models at comparable accuracy.

Q1: 这篇论文试图解决什么问题？

1) 问题背景：联合嵌入预测架构（JEPA）是自监督学习世界模型的一种有原则的框架，它通过在紧凑的潜空间中进行预测来避免像素级重建，从而提升计算效率和表征质量。然而，现有的 JEPA 族方法各自为政，针对不同层次和任务采用不同的设计。
2) 碎片化现状：I-JEPA 预测单幅图像中掩码的潜表示块，聚焦于空间结构；Image World Models 学习全局光度变换的不变性；视频级 JEPA（如 V-JEPA 2、DINO-World、DINO-WM）预测未来时间状态，并通常在动作条件下进行后训练以支持规划。这些方法使用独立的编码器、预测器和防坍缩正则器，导致一种方法难以同时处理图像级和视频级的世界建模。
3) 核心矛盾：是否存在一个统一的目标和单一共享潜空间，使得模型能同时学习图像级的光度不变性、视频级的时序等变性以及动作条件下的规划能力，而无需为每个任务定制架构和损失？
4) 技术挑战：在联合学习多个预测任务时，如何防止表征坍缩（即所有输入映射到常量）？现有防坍缩机制（EMA、stop-gradient、对比损失）往往复杂且需要额外超参数。此外，不同任务可能对表征的抽象层级有不同要求——光度预测希望不变性（去除光照、颜色变化），时序预测希望等变性（保留运动状态），如何在一个潜空间内协调这两种需求？
5) 空缺点：现有 JEPA 框架尽管在各自任务上有效，但缺乏一个真正统一、端到端可训练、防坍缩保证清晰、且能展示可控不变性-等变性谱的视觉世界模型。

Q2: 有哪些相关研究？

1) JEPA 的起源与动机：LeCun（2022）提出 JEPA 作为通往自主机器智能的路径，强调在抽象表征空间进行预测而非在像素空间重建。
2) 图像级 JEPA：I-JEPA（Assran et al., 2023）学习预测图像中缺失块的潜表示，以掩码建模方式捕获空间结构；该类方法证明在潜空间中的预测可以产生高质量视觉表征，且无需数据增强。
3) 视频级 JEPA：V-JEPA（Bardes et al., 2024）等扩展到视频时序预测，学习未来的潜表示；V-JEPA 2、DINO-World、DINO-WM 进一步将动作条件纳入后训练，实现基于潜空间预测的规划器。
4) 图像世界模型：Image World Models 将 JEPA 应用于学习全局光度变换（如颜色、亮度变化）下的不变表征，类似于计算机视觉中的不变性学习，但缺少时序维度。
5) 防坍缩机制：现有方法往往依赖 EMA 教师、stop-gradient 或对比损失来避免表征坍缩。LeWorldModel（Maes et al., 2026）表明单一损失超参数即可有效训练 JEPA，UniJEPA 参考了这种简约目标的设计。
6) 与生成式世界模型的对比：生成式世界模型（如基于像素重建或扩散模型）虽然在生成任务上表现直观，但计算开销大，规划速度慢；JEPA 类方法因在紧凑潜空间预测而获得效率优势，UniJEPA 进一步量化了这一优势。
7) 统一化尝试的缺失：尽管已有各种改进，鲜有工作真正将光度预测（不变性）与时序预测（等变性）在一个共享潜空间和一个防坍缩目标中联合训练，并严格证明其抗坍缩性质。

Q3: 论文如何解决这个问题？

UniJEPA 的核心思路是构建一个统一的联合嵌入预测架构，将两种互补的预测任务整合到一个共享潜空间中，并用单一正则化机制保证训练稳定性。
1) 统一潜空间：使用一个共享编码器（Vision Transformer 骨干）将图像和视频帧映射到紧凑的潜表征；一个共享预测器在不同时间步、不同输入变换之间进行预测。
2) 双重预测目标：
 - 光度预测：对图像施加全局光度变换（如亮度、对比度、偏色），编码器提取不变结构，预测器从一种变换的表征预测另一种变换的表征，从而学习对光照变化不敏感的高层语义。
 - 时序预测：给定视频时间序列，预测器依据当前时刻的潜表征预测下一时刻的潜表征，学习动态演变和物理状态转移的等变表征。
3) 单一防坍缩目标：总损失由 next-embedding 预测损失（例如余弦相似度或平滑 L1）和高斯正则器组成。高斯正则器鼓励编码器输出的潜表征服从各向同性的高斯先验，从而在理论上防止所有样本产生相同表征的坍缩。作者给出定理证明该正则器足以保证端到端训练下编码器-预测器不坍缩，无需 EMA、stop-gradient 或预训练编码器。
4) 可控不变-等变谱：通过调整光度预测和时序预测在总损失中的相对权重，可以控制最终表征偏向不变性（抗干扰）或等变性（保动态）。两个任务共享同一潜空间，因此模型无需重新训练就能在抽象层级上连续调节。
5) 动作条件后训练与规划：在预训练后，利用离线轨迹（状态-动作-下一状态）对预测器进行轻量后训练，使其接受动作作为条件输入来预测未来潜状态。规划时，将目标图像编码为潜特征，作为预测目标，通过搜索或优化动作序列使预测的潜状态逼近目标特征，实现零样本规划。
6) 简化的训练超参数：整个预训练阶段仅需一个损失超参数（用于平衡预测损失与正则项的系数），大大降低调参成本。

Q4: 论文做了哪些实验？

根据摘要与检索到的片段，UniJEPA 的实验覆盖图像、视频和控制三大类基准，但检索到的文本未提供具体数据集名称、指标数值和完整设置。以下信息均来自摘要和结论中的明确陈述：
1) 图像基准：评估光度预测学习的表征质量，与任务专用 JEPA（如 I-JEPA）比较，UniJEPA 达到或超越后者。
2) 视频基准：评估时序预测的动态建模能力，与视频级 JEPA（如 V-JEPA）比较，性能匹配或更优。
3) 控制基准：在动作条件后训练后，测试零样本规划性能（例如将目标特征作为预测目标），对比生成式世界模型和专用规划器。
4) 效率实验：训练 15M 参数量变体在单 GPU 上只需几小时，与 LeWorldModel 报告的训练效率一致；规划速度比生成式世界模型快数十倍（在相同精度水平下）。
5) 消融与可控性分析（合理推断）：可能包括改变光度与时序损失的权重观察表征在不变-等变谱上的移动，以及验证单一高斯正则器在不同任务组合下均能防坍缩；但具体消融实验细节并未出现在检索片段中。
6) 注意：由于检索证据仅有摘要、结论和部分相关工作，上述实验设置可能不完整，无法提供更详细的基准列表、训练数据规模、超参数选择或对比方法的完整名称。

Q5: 发现了什么实验现象？

1) 统一目标不损性能：UniJEPA 将光度与时序预测联合训练，在图像、视频和控制任务上均能达到甚至超过各自领域任务专用 JEPA 的效果（摘要明确陈述）。这表明不同预测任务在共享潜空间中不仅没有冲突，反而可能互补。
2) 防坍缩性质的理论保证转化为实际稳定性：作者证明高斯正则器能防止坍缩，实验中模型从原始像素端到端训练即可收敛，无需 EMA 等额外机制，说明理论保证与实际训练稳定性一致。
3) 规划效率的显著优势：在相近精度下，规划速度比生成式世界模型快数十倍，合理推断这是因为潜空间预测避免了每步像素级生成的开销，且预测器为轻量模块。这一优势对实时控制应用意义重大。
4) 可控不变-等变谱的灵活性（推测）：调整光度与时序损失权重，表征行为会在“忽略光照变化”和“保留运动状态”之间连续变化，这在实际部署中可用于匹配不同任务所需的抽象层级。
5) 负结果或局限的缺失：检索片段中未提及任何失败案例、性能退化或指标间的冲突，因此无法判断是否存在对某些复杂动态场景失效的情况。
6) 训练效率的观察：15M 参数模型单 GPU 数小时完成训练，表明该方法对计算资源要求较低，使得大规模实验成本可控。

Q6: 有什么可以进一步探索的点？

1) 扩展到更大规模与更多模态：将 UniJEPA 应用于更大规模视频数据集或融合音频、文本等多模态信息，检验统一潜空间的扩展性。
2) 更深度的规划与强化学习结合：当前规划是零样本目标引导，未来可以研究如何将 UniJEPA 的潜表征嵌入到强化学习策略中，用于长时程决策，或结合蒙特卡洛树搜索等规划算法。
3) 研究不变-等变谱的理论边界：虽然论文证明了防坍缩，但尚未完全刻画两个任务在共享潜空间中的相互影响；可以形式化分析损失权重如何影响表征的几何结构，以及是否存在最佳平衡点。
4) 改进正则器：高斯正则器是可防坍缩的充分条件，但未必是最优；未来可探索其他正则化（如熵最大化、非对比互信息估计）在统一多任务中的适用性。
5) 跨任务泛化测试：在更多下游任务上评估（如动作识别、视觉问答、机器人抓取），验证统一潜表征的多功能性。
6) 实时系统部署：利用其规划速度优势，在机器人或自动驾驶平台上进行闭环实验，明确在物理系统中的鲁棒性。
7) 与生成式世界模型结合：探索潜空间预测与像素级细化的混合模型，兼顾效率与精细重建。

Q7: 总结一下论文的主要内容

UniJEPA 旨在解决现有 JEPA 族世界模型碎片化的问题。论文指出现有方法分别针对图像掩码预测（I-JEPA）、光度变换（Image World Models）、视频时序预测（V-JEPA 2 等），各自使用独立编码器、预测器和防坍缩正则器，缺乏统一的视觉世界模型。UniJEPA 提出在一个共享潜空间中联合学习两种互补的预测目标：光度预测（图像级全局变换的不变性）与时序预测（视频级下一状态的等变性）。其训练目标只包含一个 next-embedding 预测损失和一个高斯正则器，端到端从原始像素训练，理论上可证明防坍缩，无需 EMA、stop-gradient 或预训练编码器。论文进一步表明，通过调节两个任务的损失权重，可以控制在共享潜空间表征中不变性和等变性的相对程度，形成可控的抽象谱。在预训练之后，UniJEPA 通过动作条件下在离线轨迹上进行后训练，可将目标特征作为预测目标进行零样本规划。实验部分（摘要所述）覆盖图像、视频和控制基准，性能匹敌或超越任务专用 JEPA；训练效率方面，15M 参数模型单 GPU 数小时完成训练；规划速度比生成式世界模型快数十倍。论文结论强调统一目标、防坍缩保证、可控不变-等变谱以及高效规划是其主要贡献。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对智能体（agent）方向：UniJEPA 展示了如何将世界模型用于零样本规划，其动作条件后训练和潜空间预测为模型预测控制或强化学习提供了一种高效备选方案。

## 基本信息

- 作者：An Lanji, Dawei Liu, Jin Li, Haoran Xu, Mei Chen, Yu Tian
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.07409`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（摘要、结论、相关工作和扩展性片段），但证据覆盖有限，部分内容基于启发式草稿和合理推断，未能核实具体实验数值。
