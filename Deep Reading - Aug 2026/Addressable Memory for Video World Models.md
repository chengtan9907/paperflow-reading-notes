---
user_id: "cheng tan"
paper_id: 7183
arxiv_id: "2608.07408"
title: "Addressable Memory for Video World Models"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.07408.pdf"
pdf_url: "https://arxiv.org/pdf/2608.07408"
abs_url: "https://arxiv.org/abs/2608.07408"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-11T01:14:33"
---
# Addressable Memory for Video World Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world models · video generation · kv cache compression · rotary position embedding

## 一句话总结

交互式视频世界模型中，训练外的时间RoPE偏移导致KV缓存不可寻址，本文提出免训练的WORLDTRACE可寻址压缩记忆框架，以FIELD与LANDMARK两种变体分别提升时间一致性与情景回忆。

## 摘要

> We study visual persistence in interactive video world models. These models rely on a Key-Value (KV) cache as a growing visual memory to carry forward previously generated frames. However, we find that models can no longer reliably address stored content once rollouts extend beyond the training horizon, because temporal Rotary Positional Embeddings (RoPE) offsets then fall outside the range seen during training and the model struggles to retrieve the relevant visual information through attention. Moreover, naively compressing the cache in the RoPE-rotated space corrupt memory by averaging together incompatible positional phases. To address this, we propose WORLDTRACE, a training-free memory framework for long-horizon visual persistence. WORLDTRACE keeps compressed memory addressable by assigning each summary slot a distinct, in-distribution virtual position. Within this addressable cache, we study two memory compression approaches: WORLDTRACE-FIELD compresses history for temporal coherence, while WORLDTRACE-LANDMARK stores verbatim scene traces at detected transitions for episodic recall. We further introduce LOOPBENCH, a benchmark evaluating whether a compressed cache can reconstruct a previously visited scene after a long detour. WORLDTRACE-FIELD improves temporal consistency by +15.5%, and WORLDTRACE-LANDMARK improves episodic recall by +19.5% on LOOPBENCH, extending visually persistent generation without retraining.

Q1: 这篇论文试图解决什么问题？

本文针对的是交互式视频世界模型中的长期视觉持久性问题。具体来说：
1) 这类模型将自回归生成过程中的KV缓存当作累积的视觉记忆，需要把早期生成的帧内容延续到后续生成中。
2) 实践中rollout长度一旦超过训练时的序列长度，时间维度的RoPE偏移量会超出训练分布，导致模型的注意力机制无法正确寻址（匹配）旧记忆，仿佛“忘了”之前看到的内容。
3) 一个常见的缓解手段是压缩缓存以控制显存和上下文长度，但直接在RoPE旋转后的空间做压缩会破坏键的相位结构——平均值会混叠不同朝向的向量，从而产生无意义的记忆。
4) 这两个问题实际上是耦合的：记忆放在哪里（位置信息）与记忆存什么（内容压缩）共同决定了记忆是否可用；先前的许多方法只修复其中之一，比如重新参数化或限制偏移，或者只决定每个槽压缩何种信息，但忽视了地址可寻址性这一前置条件。

Q2: 有哪些相关研究？

相关工作可以从两条线来组织：
1) 记忆放置/寻址调整：通过修改位置编码或注意力机制来让模型在长序列中仍然能够访问旧内容。例如Block-relative（[107]）等方法，通过相对位置分块或重参数化来限制偏移量。其他常见思路包括RoPE外推/插值、滑动窗口、全局-局部注意力等，但这些方法主要解决位置泛化，不一定讨论压缩记忆的可寻址性。
2) 记忆内容压缩：决定KV缓存中存储何种信息、如何汇总历史内容，例如隐层状态压缩、token合并或记忆槽网络等。这类方法通常关注信息保留率，但在压缩过程中可能忽视位置编码的相位结构。
本文提出的WORLDTRACE同时考虑两个维度：在典范空间（canonical space）中重新分配虚拟位置，避免RoPE旋转压缩带来的混叠，同时保留两种不同粒度的记忆——FIELD用于保持时间一致性，LANDMARK用于保存突发转换处的原始痕迹。这两点分别回应了“位置”与“内容”两个挑战。

Q3: 论文如何解决这个问题？

WORLDTRACE是一个训练免的框架，用于自回归视频世界模型中的可寻址压缩记忆。其核心思想是：将远程历史压缩到固定数量的摘要槽（summary slots）中，同时保证每个槽在注意力机制中可被正确寻址。具体做法包括：
1) 将存储的键从原始RoPE旋转空间映射到一个“典范空间”（canonical-space）的虚拟位置，这些虚拟位置被设定为训练分布内（in-distribution）——这样即使实际时间步超出训练范围，模型仍能查询到这些槽。
2) 提出两种记忆内容压缩策略：WORLDTRACE-FIELD将历史的视觉信息按某种字段/场的方式压缩，目标是保持时间上的连贯性；WORLDTRACE-LANDMARK则在检测到场景转换（transitions）时，将这些时间点的原始场景轨迹逐字存储下来，用于后续的情景回忆。
由于不需要重新训练模型，该方法可以即插即用地应用于现有的自回归视频世界模型。

Q4: 论文做了哪些实验？

论文提出了新的评测基准LOOPBENCH，用于考察压缩缓存是否能在长时间绕行后重建先前访问过的场景。实验在LOOPBENCH上比较了WORLDTRACE的两种变体（FIELD和LANDMARK）与基线方法（可能是朴素缓存、直接压缩等方法，但具体名称未在摘要中给出）。主要指标包括：时间一致性（temporal consistency）和情景回忆（episodic recall）。结果显示：WORLDTRACE-FIELD将时间一致性提升15.5%；WORLDTRACE-LANDMARK将情景回忆提升19.5%。这些改进是在不重新训练模型的情况下取得的。注意：具体的实验设置、基线和消融内容由于我们仅获取摘要，无法在此处细述，需要阅读原文的实验部分。

Q5: 发现了什么实验现象？

从摘要和检索片段中可以提炼的实验现象包括：
1) 超出训练长度的rollout会导致KV缓存中的内容变得不可寻址，这是长视界生成的一个主要失败模式；
2) 在RoPE旋转空间中进行朴素压缩会破坏记忆的相位结构，导致记忆被“平均值”污染；
3) 提出的两种压缩变体分别在时间一致性和情景回忆上带来增益，说明不同的记忆内容组织方式服务于不同需求——FIELD偏重连续生成时的帧间协调，LANDMARK偏重离散事件后的恢复；
4) 这些成功说明，只要保证记忆可寻址，即使不训练也能显著延长视觉持久生成的时效。
不过，由于缺乏细节，我们无法得知具体的失败案例或scaling趋势。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：
1) 将WORLDTRACE与训练阶段结合，比如微调或加入位置编码适配，可能获得更大提升；
2) 研究更复杂的压缩策略，例如学习式的摘要槽分配或自适应槽数；
3) 将方法论迁移到其他带RoPE或类似位置编码的序列模型（如LLM、具身智能体）；
4) 扩展LOOPBENCH到更复杂的交互场景，涵盖任意长绕行和多样环境；
5) 分析虚拟位置分配的理论性质，例如最优虚拟位置规律；
6) 结合检索增强机制，使模型能在极长历史中精准找到特定时刻（event-based retrieval）。

Q7: 总结一下论文的主要内容

本文针对交互式视频世界模型的长期视觉记忆问题展开。首先指出现有模型依赖KV缓存作为累积视觉记忆，但当生成长度超出训练范围时，时间RoPE偏移导致注意力无法正确寻址旧记忆；同时，直接压缩RoPE旋转空间中的缓存会因相位混叠而破坏记忆。作者将这两个问题提炼为“记忆放在哪里”与“记忆存什么”的耦合挑战。为解决该问题，他们提出训练免的WORLDTRACE框架：通过将每个摘要槽分配到分布内的虚拟位置（典范空间键分配）来确保可寻址性；并设计了两种压缩变体——FIELD（压缩历史以维持时间一致性）和LANDMARK（在场景转换处保留逐字轨迹以支持情景回忆）。此外，他们提出了LOOPBENCH基准，模拟在长时间绕行后重建已访问场景的任务。实验证明，WORLDTRACE-FIELD将时间一致性提升15.5%，WORLDTRACE-LANDMARK将情景回忆提升19.5%。该方法无需任何训练即可即插即用，为长视界视频世界模型提供了一种轻量且有效的记忆增强手段。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与用户画像中的“生成”方向直接相关，并且是系统性工作（提出基准+方法）。

## 基本信息

- 作者：Xindi Wu, Sven Elflein, James Lucas, Olga Russakovsky, Laura Leal-Taixé, Despoina Paschalidou, Jonathan Lorraine, Aljoša Ošep
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.LG
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.07408`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的Abstract及关键片段（如贡献列表和方法概述），并结合启发式草稿进行了扩展，未获取到完整论文正文，因此部分细节为合理推测。
