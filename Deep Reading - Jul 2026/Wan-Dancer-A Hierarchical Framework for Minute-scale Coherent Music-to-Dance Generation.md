---
user_id: "cheng tan"
paper_id: 3577
arxiv_id: "2607.09581"
title: "Wan-Dancer: A Hierarchical Framework for Minute-scale Coherent Music-to-Dance Generation"
institution: "Alibaba Group, Tongyi Lab (阿里巴巴通义实验室)"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.09581.pdf"
pdf_url: "https://arxiv.org/pdf/2607.09581"
abs_url: "https://arxiv.org/abs/2607.09581"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-14T01:02:14"
---
# Wan-Dancer: A Hierarchical Framework for Minute-scale Coherent Music-to-Dance Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：music-to-dance generation · long-duration video synthesis · hierarchical framework · diffusion models

## 一句话总结

Wan-Dancer 提出了一个分层框架，通过全局关键帧规划与局部时间细化，实现了分钟级、高清晰度且节奏同步的连贯音乐舞蹈视频生成。

## 摘要

> Generating long-duration, high-definition, and rhythmically synchronized dance videos directly from music remains a significant challenge, primarily due to the temporal constraints of current diffusion models, which typically fail beyond 20 seconds. Existing approaches, whether they rely on intermediate 3D skeletons or on end-to-end video synthesis, suffer from temporal drift, identity inconsistency, and repetitive motion patterns when extended to longer horizons. To address these limitations, we propose a novel hierarchical framework for minute-scale coherent music-to-dance generation. Our method decouples the process into global keyframe planning and local temporal refinement, leveraging full-track musical context to ensure long-range coherence. Key innovations include dynamic frame rate adaptation via time-mapped RoPE embeddings for precise alignment, an optical-flow-based loss function to enhance motion continuity, and motion-speed control to preserve high-fidelity details during rapid movements. Extensive experiments demonstrate that our framework surpasses the conventional duration barrier, generating stable, 720p/30fps videos exceeding one minute with superior temporal stability. Furthermore, the model exhibits robust versatility across five distinct dance genres, conditioned on both audio and textual prompts, establishing a new state-of-the-art in coherent, long-form dance video synthesis.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：分钟级生成的“时长壁垒”
目前的视频扩散模型（如 Wan, HunyuanVideo, Seedance）在生成短视频（5-15秒）方面表现出色，但在扩展到分钟级长视频时面临以下核心瓶颈：
1. **时间漂移与身份不一致**：随着生成时长的增加，模型难以维持人物身份（Identity）的稳定性，背景和人物特征往往会发生不可逆的扭曲。
2. **计算复杂度限制**：自注意力机制（Self-attention）在处理长序列时具有平方级的计算复杂度，直接生成长视频会导致显存溢出或推理时间不可接受。
3. **动作重复与节奏失调**：现有方法在长程生成中容易陷入循环动作模式，且难以在长达数分钟的音频中保持精确的音画同步（Beat Alignment）。
4. **运动模糊与伪影**：在快速舞蹈动作中，传统模型常出现肢体断裂或面部扭曲，尤其是在高分辨率（720p）下，细节丢失严重。

### 任务定义与边界
论文旨在解决从原始音频直接生成高保真舞蹈视频的任务，要求在长达一分钟以上的跨度内，同时满足视觉质量、动作多样性、节奏对齐和时空连贯性四个维度的要求。

Q2: 有哪些相关研究？

### 现有范式对比
1. **两阶段法（基于 3D 骨架）**：先生成 3D 姿态序列，再进行渲染。虽然动作控制力强，但渲染过程往往导致视觉质量下降，且难以捕捉复杂的服装和环境交互。
2. **端到端生成法**：直接从音频/文本生成视频。虽然视觉效果好，但受限于扩散模型的上下文窗口，难以处理长序列。

### 关键技术背景
- **扩散模型（Diffusion Models）**：如 Stable Video Diffusion 等，提供了强大的生成底座，但缺乏长程规划能力。
- **RoPE（旋转位置嵌入）**：在 LLM 中用于处理长文本，本文将其引入视频领域以解决时间对齐问题。
- **LoRA（低秩自适应）**：用于在特定舞种（如 K-pop, Breaking）上进行微调，增强风格化表现。

Q3: 论文如何解决这个问题？

### 分层生成架构（Hierarchical Framework）
Wan-Dancer 将任务解耦为两个核心阶段：
1. **全局关键帧规划（Global Keyframe Planning）**：
 - 利用全长音乐的上下文信息，在稀疏的时间点上规划关键动作帧。
 - 这种“大局观”确保了舞蹈编排在整分钟内不会出现单调重复，且能根据音乐的高潮和节奏变化调整动作强度。
2. **局部时间细化（Local Temporal Refinement）**：
 - 在关键帧之间进行插值和细节填充，确保动作的平滑过渡。

### 核心技术创新
- **动态帧率适配与 Time-mapped RoPE**：
 - 引入时间映射的旋转位置嵌入（RoPE），允许模型在不同帧率下灵活对齐音频特征，解决了长序列中位置编码失效的问题。
- **基于光流的损失函数（Optical-flow-based Loss）**：
 - 在训练中引入光流约束，强制模型学习相邻帧之间的运动一致性，显著减少了肢体闪烁和背景抖动。
- **运动速度控制（Motion-speed Control）**：
 - 针对舞蹈中的爆发性动作，通过专门的控制模块调整生成策略，防止在高速运动时出现画面崩坏，保留高频视觉细节。
- **多模态条件注入**：
 - 同时接受音频（Audio）和文本提示（Textual Prompts），允许用户通过文字指定舞种（如“爵士舞”、“街舞”），并由音频驱动具体节奏。

Q4: 论文做了哪些实验？

### 实验设置
- **数据集**：使用了包含多种舞种（K-pop, Ballet, Jazz, Hip-hop, Breaking）的大规模高质量舞蹈视频数据集。
- **硬件环境**：基于大规模 GPU 集群进行分布式训练。
- **分辨率与帧率**：目标输出为 720p 分辨率，30fps 帧率，时长超过 60 秒。

### 基线对比（Baselines）
- **X-Dancer**：对比其在长视频生成中的结构稳定性。
- **MusicInfuser**：对比其在风格一致性和节奏对齐上的表现。
- **原生 Wan/HunyuanVideo**：对比其在突破 20 秒限制后的崩溃情况。

### 评估指标
- **视觉质量**：FVD (Fréchet Video Distance)。
- **节奏同步性**：Beat Alignment Score。
- **身份保持**：ID Consistency Score。
- **运动平滑度**：基于光流的连续性评分。

Q5: 发现了什么实验现象？

### 关键发现与现象
1. **结构完整性优势**：在对比实验中，X-Dancer 在生成长视频时经常出现面部和手部的结构性伪影（如手指数量错误或面部扭曲），而 Wan-Dancer 能够保持解剖学上的完整性。
2. **风格一致性**：MusicInfuser 在处理特定舞种时容易产生泛化的、平庸的动作，而 Wan-Dancer 通过 LoRA 能够精确还原不同舞种的标志性动作（如 Breaking 的地板动作）。
3. **消融实验结果**：
 - **RoPE 的作用**：移除 Time-mapped RoPE 后，视频在 30 秒后会出现明显的音画不同步。
 - **光流损失的影响**：缺乏光流损失会导致快速旋转动作中出现严重的“重影”现象。
4. **负结果/局限模式**：在极度复杂的背景（如人群密集的舞台）下，模型偶尔会将背景元素误认为舞者的一部分，导致背景局部扭曲。
5. **Scaling Trend**：随着全局规划窗口的增大，舞蹈的叙事性和编排感显著增强，不再是简单的动作堆砌。

Q6: 有什么可以进一步探索的点？

1. **实时生成优化**：目前分钟级视频的推理成本仍然较高，未来可探索模型蒸馏或更高效的采样算法以实现近实时生成。
2. **多舞者交互**：当前的框架主要针对单人舞蹈，如何处理双人或群舞中的物理碰撞和空间协调是下一个挑战。
3. **更精细的物理模拟**：进一步增强服装（如长裙、飘带）在舞蹈中的物理真实感。
4. **交互式编舞**：允许用户在生成过程中实时干预关键帧，实现人机协同编舞。

Q7: 总结一下论文的主要内容

这篇论文介绍了 Wan-Dancer，一个专门为分钟级、高保真音乐驱动舞蹈视频生成设计的层次化扩散框架。针对现有模型在长视频生成中面临的计算复杂度高、身份不一致和节奏漂移等痛点，作者提出了一套系统性的解决方案。技术上，Wan-Dancer 通过全局关键帧规划确保了长程的编排连贯性，并利用局部细化模块保证了视觉细节。其核心贡献在于引入了时间映射的 RoPE 嵌入以实现精确的音画对齐，以及基于光流的损失函数来提升运动平滑度。实验结果令人印象深刻：模型能够稳定生成超过 60 秒、720p/30fps 的高质量视频，且在多种舞蹈风格上展现了极强的泛化能力。相比于之前的 SOTA 模型，Wan-Dancer 在保持人体结构完整性和动作节奏感方面取得了显著突破，为长格式多模态视频合成树立了新的标杆。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该研究直接关联生成式 AI（Generation）方向，特别是多模态长视频合成。

## 基本信息

- 作者：Mingyang Huang, Peng Zhang, Li Hu, Guangyuan Wang, Bang Zhang
- 机构：Alibaba Group, Tongyi Lab (阿里巴巴通义实验室)
- 来源：arxiv
- 主题/分类：cs.CV, cs.SD
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2607.09581`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于分层架构、RoPE 创新及实验对比的具体细节。
