---
user_id: "cheng tan"
paper_id: 9992
arxiv_id: "2608.31106v1"
title: "DreamX-Creator: Democratizing Native Audio-Video Generation at 2K Resolution"
institution: "Alibaba Group（阿里巴巴集团）"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.31106v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.31106v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:40:34"
---
# DreamX-Creator: Democratizing Native Audio-Video Generation at 2K Resolution

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：audio-video generation · joint generation · flow matching · cross-modal attention

## 一句话总结

DreamX-Creator 1.0 是一个以 7B 参数为核心、面向 2K 分辨率的原生联合音频-视频生成系统，通过门控跨模态注意力、渐进式联合训练与自回归一步细化实现高质量同步生成。

## 摘要

> Recent video generators often omit audio or synthesize it in a separate stage, limiting reciprocal modeling of visual dynamics and acoustic events. We present DreamX-Creator 1.0, a compact native joint audio-video generation system centered on a 7B generator. Conditioned on a first frame and a text prompt, the generator jointly denoises modality-specialized audio and video streams. The streams are processed independently in the first half of the network and coupled in the latter half through Gated Cross-Modal Attention, whose token- and head-wise output gates modulate each active cross-modal attention-head output. A unified Audio-Video Data System constructs and filters temporally coherent clips, produces structured multimodal annotations, and organizes clips into capability-oriented data pools. Progressive Joint Training comprises two audio-video pre-training stages followed by High-Quality Finetuning. Audio-Video Reinforcement Learning further post-trains the generator with Modality-Aware Multimodal Feedback that routes video-, audio-, and cross-modal feedback to the corresponding streams. For high-resolution output, our Autoregressive 1-Step 2K Refinement pipeline adapts a bidirectional multi-step teacher into an autoregressive multi-step refiner and distills it into a student requiring one denoising evaluation per temporal chunk. Overall, DreamX-Creator 1.0 achieves native, synchronized audio-video generation with performance competitive with state-of-the-art open-source systems. By releasing our compact 7B generator and 2K Refiner, we seek to democratize native audio-video generation and provide an accessible foundation for future research in unified audio-video generative modeling.

Q1: 这篇论文试图解决什么问题？

这篇论文针对原生音频-视频联合生成中的几个核心问题展开。第一，现有视频生成器普遍忽略音频，或采用独立的音频合成阶段（如视频到音频的管道式方法），这割裂了视觉动态与声学事件之间的双向依赖关系，导致生成结果在时间同步、语义一致性和物理因果性上存在明显缺陷。第二，当前能够实现联合建模的系统往往规模庞大（数十亿甚至上百亿参数），且多提供托管 API 而不开放权重，或者复现条件苛刻，这构成了研究社区深入探索原生音频-视频生成的实际障碍。第三，跨模态交互的引入存在两难：交互强度需要按层、按注意力头精细调节，过强会压倒模态特有的表示，过弱则无法建立有效关联；同时，背景音频可能引入虚假的跨模态相关性，破坏学习的质量。第四，音频-视频样本的评估涉及多个维度（视觉质量、音频质量、语义一致性、时间同步等），这些指标往往不能同时提升，存在相互制约的关系，需要设计能够平衡的训练和后训练策略。

Q2: 有哪些相关研究？

相关研究可以划分为几个脉络。一是纯视频生成方向，许多近期工作（如 Team et al., 2026a,b）在视觉保真度、运动质量和时长上进步显著，但音频常被省略或在后期单独添加。二是视频到音频合成方向，Diff-Foley（Luo et al., 2024）等工作实现了同步的视频到音频合成，但它们是单向依赖，无法让音频反过来调节视觉生成。三是联合音频-视频生成方向，如 Ovi（2025）采用双主干跨模态融合，另有研究提出原生视听对齐（Native audio-visual alignment, 2026），以及 Kling AI 3.0 等商业系统宣称支持原生多模态视频生成。但在开源生态中，紧凑、可复现且达到 2K 分辨率的联合生成系统仍然稀缺。本工作与这些研究的差异在于：一是采用 7B 的紧凑规模；二是设计了显式的门控跨模态注意力机制，而不是简单的拼接或交叉注意力；三是构建了系统化的数据构建和渐进式训练流程；四是提出了针对高分辨率的自回归一步细化方案。

Q3: 论文如何解决这个问题？

DreamX-Creator 1.0 的解决方案围绕四大部分。

1) 联合生成架构：以首帧和文本提示为条件，生成器同时去噪视频隐流和音频隐流。每个流拥有独立的 token 率、位置编码和 Transformer 骨干，并通过共享文本编码器接收条件。网络前半部分两个流各自处理，后半部分引入门控跨模态注意力（Gated Cross-Modal Attention）进行双向耦合。每个激活的跨模态注意力头输出都经过 token 级和 head 级的 sigmoid 门控，门控信号同时依赖于目标隐藏状态和跨模态注意力输出，并采用每样本的方向掩码来选择活动路径（例如某些层视频到音频，某些层音频到视频，或双向）。这种设计能够在不过度干扰模态特有表示的前提下，按需调节模态间信息流动。

2) 统一音频-视频数据系统：构建大规模、时间上一致的音视频片段，设计过滤流程去除不同步、噪声大或语义不匹配的样本，生成结构化的多模态标注（如场景描述、声源、事件时间戳等），并按能力维度（如语音、音乐、环境声、人物交互等）组织成数据池，以支持分阶段训练。

3) 渐进式联合训练：训练分为两个音频-视频预训练阶段和高质量微调阶段。预训练阶段使用大量低质量但覆盖广的数据，让模型建立基本的联合分布；后一个预训练阶段可能采用更高质量或更难样本；高质量微调使用精标数据提升视觉和音频质量、语义一致性及时间同步性，同时保持基础模型的泛化能力。训练目标使用音频和视频的流匹配（flow-matching）目标。

4) 音频-视频强化学习后训练：利用模态感知的多模态反馈（Modality-Aware Multimodal Feedback）进行强化学习，将视频质量反馈、音频质量反馈和跨模态一致性反馈分别路由到对应的流或交互模块，解决多指标不可同增的问题。

5) 自回归 1 步 2K 细化流水线：为了扩展到 2K 分辨率，先将双向多步教师模型改造为自回归多步细化器（沿时间块顺序细化），再蒸馏为一步学生模型，使每个时间块只需一次去噪评估，兼顾质量与效率。

Q4: 论文做了哪些实验？

论文中实验部分的完整细节在摘要和检索片段中未被完全呈现，以下基于现有材料进行合理推断。作者应当进行了以下几类实验。

1) 联合生成质量评估：在开源或内部数据集上评估生成视频和音频的质量，可能包括 FVD（Fréchet Video Distance）等视频指标、音频指标如 FAD（Fréchet Audio Distance）或主观 MOS 评分，以及语义一致性指标（如 CLIP 文本-视频对齐、文本-音频对齐）和时间同步指标（如 AV-alignment）。

2) 与 SOTA 开源系统的对比：与当前领先的开源视频生成和音频-视频生成系统对比，验证 7B 参数下的性能竞争力。

3) 消融研究：分别去掉门控跨模态注意力、数据池构建、渐进式训练阶段或强化学习后训练，评估其对最终质量的影响。此外还应对门控机制的门控类型（token 级 vs head 级）和方向掩码策略进行消融。

4) 2K 细化实验：对比直接生成 2K 与基座模型+细化的效果，验证蒸馏学生模型的保真度和计算效率。

5) 训练稳定性分析：观测预训练各阶段的损失变化和生成样本质量变化，验证渐进式训练的有效性。

由于具体的评估协议和数值未在检索片段中给出，上述内容为合理推断，需参考论文原文的实验章节确认。

Q5: 发现了什么实验现象？

根据摘要和检索片段，可以归纳以下实验观察：

1) 高质量微调阶段可以同时改善视觉质量、音频质量、语义一致性和时间同步性，同时保持基础模型的生成能力与多样性——这表明精心设计的微调并不必然导致灾难性遗忘或能力退化。

2) 训练中面临的核心挑战是：音频-视频样本的多个评估维度往往不能同时提升。这意味着在强化学习后训练中，针对单指标的优化可能损害其他指标，因此需要模态感知的多模态反馈来区分优化信号。

3) 门控跨模态注意力的设计动机来自观察：跨模态交互的所需强度随层和注意力头不同而变化，过强的交互会压倒模态特有表示。因此采用条件于隐藏状态和注意输出的 sigmoid 门控是合理有效的策略。

4) 数据构建中，背景音频可能引入虚假跨模态相关性，因此需要专门的过滤机制来消除这类噪声。

由于缺乏具体实验图表数据，上述观察主要基于论文文本描述，更细节的现象（如趋势、失败案例）需查阅原文实验部分。

Q6: 有什么可以进一步探索的点？

进一步探索的方向包括：

1) 方法层面：改进门控跨模态注意力的表达能力，如引入自适应层数选择或可学习的路由机制；探索更高效的多模态融合方式，减少注意力开销。

2) 效率层面：进一步压缩模型规模，例如使用蒸馏或量化，使得 7B 模型可以在消费级 GPU 上运行；同时优化 2K 细化器的推理速度，实现实时或近实时的生成长视频。

3) 训练层面：设计更精细的数据池调度，引入课程学习或数据难度感知采样；扩展强化学习反馈的来源，结合人类偏好或更丰富的自动评估器。

4) 扩展能力：将模型扩展到更长时长（如分钟级）和更高分辨率（如 4K）；同时支持更多条件形式（如音频引导视频、视频引导音频、语音与嘴型同步等）。

5) 鲁棒性与泛化：测试模型在不同语言、文化场景和极端光照/噪声条件下的表现；研究如何避免背景音频引入的虚假跨模态相关。

6) 跨学科应用：将原生音视频生成应用于虚拟现实/增强现实、影视前期制作、辅助创作和科学可视化等，评估其实际落地价值。

7) 开源生态建设：围绕 7B 生成器和 2K 细化器构建社区，建立标准化的评估基准和数据集，促进这一方向的快速发展。

Q7: 总结一下论文的主要内容

DreamX-Creator 1.0 旨在打破原生音频-视频联合生成中的研究障碍。核心论点在于：只有将音频和视频作为平等的、同步的生成目标进行联合建模，才能充分利用视觉动态与声学事件间的双向依赖；而以往管道式方法忽略了这种交互。现有联合系统又因规模过大、不开放权重或难以复现而无法普及。为此，论文提出了一套完整的约 7B 参数的开源系统，从数据、架构、训练到高分辨率细化都进行了专门设计。

技术主线上，作者首先构建了一个原生联合生成器：以首帧和文本为条件，分别维护音频和视频两个隐流，前半段独立处理以保留模态特有表示，后半段通过门控跨模态注意力耦合。该门控机制对每个激活的注意力头进行 token 级和 head 级 sigmoid 输出门控，并根据目标隐藏状态和跨模态输出动态调节，配合方向掩码控制信息流向，从而实现层与头级别的自适应融合。数据方面，统一音频-视频数据系统解决了配对数据稀缺和噪声大的问题，通过过滤时间不一致的片段、生成结构化标注、划分能力池来支撑训练。训练策略采用渐进式联合训练，两个预训练阶段逐渐提升难度和质量，并结合高质量微调来优化最终质量。针对多目标冲突，引入模态感知的强化学习反馈，分别对视频、音频和跨模态一致性进行优化，避免相互干扰。最后，为生成 2K 分辨率视频，作者将双向多步教师模型改造为自回归多步细化器，再蒸馏成每时间块只需一次去噪评估的学生模型，有效平衡质量和计算成本。

实验主线上，论文虽然未在检索片段中给出具体评测数值，但概要指出系统性能能与开源 SOTA 系统竞争，且高质量微调和强化学习后训练能同时提升视觉质量、音频质量、语义一致性和时间同步性。消融和训练策略的调整是论文评估的重点。论文的最大贡献在于以紧凑 7B 规模实现了原生联合生成，并开源生成器和 2K 细化器，为后续研究提供了可复现的基础。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与生成方向直接相关，提供了一种原生音视频联合生成的系统化方法。

## 基本信息

- 作者：Jiashu Zhu, Yanhao Zheng, Ruitian Tian, Rujing Dang, Shen Zhang, Bingze Song, Jiachen Lei, Ruimin Lin, Jiahong Wu, Xiangxiang Chu
- 机构：Alibaba Group（阿里巴巴集团）
- 来源：arxiv
- 主题/分类：cs.CV, cs.SD
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.31106v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成综合参考了论文摘要、元数据及检索到的 PDF 片段（引言、架构概述、方法等），部分实验细节基于摘要合理推断，需要回原文确认。
