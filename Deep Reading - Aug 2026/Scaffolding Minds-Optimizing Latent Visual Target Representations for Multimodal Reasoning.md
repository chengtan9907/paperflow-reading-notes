---
user_id: "cheng tan"
paper_id: 8932
arxiv_id: "2608.19669"
title: "Scaffolding Minds: Optimizing Latent Visual Target Representations for Multimodal Reasoning"
institution: "Google Research"
publish_date: "2026-08-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.19669.pdf"
pdf_url: "https://arxiv.org/pdf/2608.19669"
abs_url: "https://arxiv.org/abs/2608.19669"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-21T23:50:20"
---
# Scaffolding Minds: Optimizing Latent Visual Target Representations for Multimodal Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：multimodal reasoning · latent visual tokens · reinforcement learning · scaffolding encoder

## 一句话总结

本文提出 Scaffolding Minds 框架，通过引入可学习的脚手架编码器和随机 RL 采样器，解决了潜变量视觉推理中目标表征亚优及探索受限的核心瓶颈。

## 摘要

> Latent reasoning has advanced multimodal reasoning through a two-stage training paradigm: (1) a helper image is encoded into latent tokens to teach visual chain-of-thought during a supervised fine-tuning (SFT) stage, and (2) these latent tokens are further refined with reward feedback during a reinforcement learning (RL) stage. In this paper, we identify two key limitations of this framework, one in each stage. First, the SFT stage typically relies on an off-the-shelf vision encoder to encode the helper image, yielding suboptimal latent representations that may not be well aligned with the downstream reasoning task. Second, existing RL methods treat the latent component only through deterministic regularization, which constrains policy drift but does not create alternative latent trajectories for exploration. To address these limitations, we propose Scaffolding Minds. Our approach learns a dedicated scaffolding encoder that provides an optimized target in latent space, and learns both the mean and variance of the RL sampler. We further show that these two improvements are complementary, together yielding substantial gains over strong baselines. Empirically, our method improves over the strongest latent-reasoning baseline by +9.5% on FrozenLake spatial planning, with the gain widening to +19% at 32x32 grid map, and by +5.2% on average across nine visual-centric reasoning benchmarks.

Q1: 这篇论文试图解决什么问题？

本文深入探讨了潜变量视觉推理（Latent Visual Reasoning）在当前多模态大模型（VLM）训练范式中的核心瓶颈。作者指出，现有的两阶段训练框架（SFT + RL）在每一阶段都存在显著的效率低下。首先，在监督微调（SFT）阶段，目前的通行做法是使用一个预训练且冻结的通用视觉编码器（如 CLIP 或 DINO）来将“辅助图像”（helper image）编码为潜变量 token。这种做法存在一个隐含假设，即通用的视觉特征足以支撑复杂的下游推理任务。然而，作者通过实验发现，这些通用编码器往往无法捕捉到推理敏感的细微特征，导致生成的潜变量目标（latent target）对于模型学习“视觉思维”而言是亚优的。其次，在强化学习（RL）阶段，现有的方法通常将潜变量组件视为一种确定性的正则化约束。这意味着模型在 RL 过程中只是被动地防止潜变量偏离 SFT 阶段设定的参考值，而没有主动在潜变量空间内进行探索。由于缺乏对潜变量动作（latent actions）的随机采样机制，模型无法在推理过程中尝试不同的“视觉想象”路径，这极大地限制了模型在面对复杂、非确定性任务时的信用分配能力和策略优化空间。这种“目标不准”与“探索不足”的组合，使得现有的潜变量推理模型在处理高难度空间规划或细粒度视觉辨析任务时，性能提升遭遇瓶颈。

Q2: 有哪些相关研究？

论文将 Scaffolding Minds 置于多模态推理和强化学习的交汇点进行讨论。相关研究主要包括：1. 潜变量视觉推理：如 Hao et al. (2025b) 提出的 Machine Mental Imagery，通过在推理链中插入潜变量 token 来模拟人类的视觉想象，但其依赖于静态的视觉编码器。2. 特权信息学习（LUPI）：该框架在概念上呼应了 Vapnik 的理论，即在训练时利用辅助信息（脚手架）来引导模型，但在推理时撤去这些辅助。3. 认知科学中的脚手架理论：借鉴了维果茨基（Vygotsky）关于教学支架的观点，即通过外部支持引导学习者进入“最近发展区”。4. 强化学习中的探索机制：对比了传统的确定性策略梯度与本文提出的随机潜变量采样，强调了在隐空间进行探索对复杂决策的重要性。论文指出，Scaffolding Minds 是首个同时优化潜变量目标表征并引入随机潜变量采样的多模态推理框架。

Q3: 论文如何解决这个问题？

为了克服上述局限，作者提出了名为“Scaffolding Minds”的创新框架，其核心思想是为模型构建一个可优化的、具有探索能力的“认知脚手架”。在第一阶段（优化潜变量目标），Scaffolding Minds 放弃了冻结的通用编码器，转而引入了一个专门的“脚手架编码器”（Scaffolding Encoder）。该编码器采用交叉注意力池化（Cross-attention Pooling）机制，能够根据当前的推理上下文动态地从辅助图像中提取关键特征。更重要的是，这个编码器是可学习的，它直接针对下游任务的损失函数进行端到端优化。通过这种方式，SFT 阶段产生的潜变量目标不再是通用的视觉描述，而是高度任务相关的“推理导向”表征。在第二阶段（增强 RL 探索），该框架引入了一个随机 RL 采样器。不同于以往的确定性映射，Scaffolding Minds 要求模型同时学习潜变量分布的均值（mu）和方差（sigma）。在 RL 训练过程中，模型会从这个分布中采样出不同的潜变量轨迹。这种随机性允许模型在“视觉想象”的维度上进行尝试和错误（trial-and-error），从而发现那些能够导致更优推理结果的潜在视觉路径。此外，作者还设计了一套协同训练协议，确保脚手架编码器提供的稳定目标与 RL 采样器的探索过程能够有机结合。这种设计不仅提升了模型在特定任务上的表现，还增强了其在未见过的视觉场景下的泛化能力。

Q4: 论文做了哪些实验？

论文设计了详尽的实验来验证 Scaffolding Minds 的有效性。主要实验设置包括：1. 空间规划任务：使用 FrozenLake 环境，设置了从 8x8 到 32x32 不同难度的网格地图，旨在测试模型在长程规划和空间推理中的表现。2. 视觉中心推理基准：在 9 个建立已久的基准测试上进行评估，包括 V*（细粒度属性和空间推理）、BLINK（核心视觉感知能力）等，以验证通用推理能力。3. 基准对比：对比了包括标准 VLM、带有 Visual CoT 的模型以及最强的潜变量推理基线（如 Machine Mental Imagery）。4. 消融实验：分别验证了脚手架编码器（可学习性、交叉注意力池化）和随机 RL 采样器（均值与方差学习）对最终性能的贡献。5. 缩放分析：研究了随着任务复杂度（地图尺寸）增加，该方法带来的增益变化趋势。

Q5: 发现了什么实验现象？

实验揭示了几个关键现象：1. 显著的缩放效应：在 FrozenLake 任务中，Scaffolding Minds 的优势随难度增加而扩大。在 8x8 地图上领先 9.5%，而在 32x32 地图上领先优势扩大到 19%，说明该方法在复杂决策场景下更具韧性。2. 组件的互补性：消融实验显示，仅将 L2 回归替换为下游任务损失配合交叉注意力池化可提升 4.3% 的准确率，而进一步微调视觉编码器则带来了额外的 10.0% 提升。两者结合构成了相对于基线的完整增益。3. 随机采样的价值：引入随机 RL 采样器后，模型在 RL 阶段的收敛速度更快且最终性能更高，证明了在潜变量空间进行探索能有效缓解信用分配问题。4. 跨领域泛化：在 9 个视觉推理基准上平均 5.2% 的提升表明，该方法不仅适用于合成的空间规划任务，在真实的细粒度视觉辨析任务中同样表现优异。5. 失败模式分析：在极少数情况下，如果辅助图像本身包含严重误导信息，脚手架编码器可能会产生错误的引导，但其随机采样机制在一定程度上缓解了这种确定性错误的影响。

Q6: 有什么可以进一步探索的点？

尽管取得了显著进展，作者也指出了未来的探索方向：1. 潜变量结构的进一步优化：探索除了 token 序列之外的其他潜变量表征形式，如分层潜变量或图结构表征。2. 无辅助图场景的扩展：研究如何在没有显式辅助图像的情况下，通过模型内部生成的“心理图像”来构建脚手架。3. 具身智能应用：将 Scaffolding Minds 应用于更复杂的具身智能体任务，如机器人在动态环境中的长程导航和操作。4. 计算效率优化：目前脚手架编码器的训练增加了计算开销，未来可以探索更轻量化的架构或更高效的训练协议。5. 认知对齐：进一步研究潜变量推理过程与人类认知过程（如心理旋转、空间工作记忆）的相似性，以提升模型的可解释性。

Q7: 总结一下论文的主要内容

《Scaffolding Minds: Optimizing Latent Visual Target Representations for Multimodal Reasoning》这篇论文针对多模态大模型在潜变量推理领域的关键缺陷提出了系统性的解决方案。论文首先指出，现有的潜变量推理范式在 SFT 阶段依赖冻结的通用编码器，在 RL 阶段缺乏探索机制，这限制了模型处理复杂视觉推理任务的能力。为了解决这些问题，作者提出了 Scaffolding Minds 框架。该框架的技术主线围绕“可学习的脚手架”展开：在 SFT 阶段，通过引入可微调的脚手架编码器和交叉注意力机制，实现了潜变量目标的动态优化，使其更贴合下游推理任务；在 RL 阶段，通过构建随机采样机制，允许模型在潜变量空间进行“试错”和探索，从而发现更优的推理路径。实验主线涵盖了从合成的空间规划任务到多样化的真实视觉推理基准。结果显示，Scaffolding Minds 在 FrozenLake 任务中取得了高达 19% 的性能提升，并在 9 个通用视觉推理基准上稳健地超越了现有最强模型。消融实验进一步证实了优化目标表征与增强 RL 探索之间的协同效应。总的来说，这篇论文不仅在技术上实现了突破，更在方法论上为多模态推理提供了一个更具动态性和探索性的新范式，对于未来开发具备更强“思维能力”的视觉智能体具有重要的参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对多模态智能体（Agent）决策过程感兴趣的研究者

## 基本信息

- 作者：Haoqiang Kang, Yinpeng Chen, Luyang Liu, Jesper Sparre Andersen, Abhijit Ogale, Baochen Sun, Lichan Hong, Ed H. Chi
- 机构：Google Research
- 来源：arxiv
- 主题/分类：cs.CV, cs.LG
- 日期：2026-08-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.19669`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于 SFT 和 RL 两个阶段局限性的分析以及实验数据的具体增幅。
