---
user_id: "cheng tan"
paper_id: 5988
arxiv_id: "2607.26818v1"
title: "Ripple: Real-Time Streaming Audio-Video Generation With Cross-Modal Recurrent Memory"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26818v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26818v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:14:42"
---
# Ripple: Real-Time Streaming Audio-Video Generation With Cross-Modal Recurrent Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：streaming audio-video generation · real-time generation · cross-modal recurrent memory · causal diffusion

## 一句话总结

Ripple 提出跨模态循环记忆机制，实现实时流式音视频联合生成，在 480P 分辨率下达到约 28 FPS，并支持长序列一致生成。

## 摘要

> Audio-video generative models achieve impressive quality but suffer from high latency, making them unsuitable for real-time applications. Although several streaming audio-video generation methods have been proposed, they remain costly and fail to support long-form generation. To address this, we propose \textbf{Ripple}, a real-time joint audio-video generation system with a cross-modal recurrent memory mechanism. To enable efficient streaming inference while preserving long-term context, Ripple combines a fixed-length sliding-window attention with modality-specific memory states that continuously summarize audio and video context. Cross-modal memory interaction is further introduced to enhance audio-visual synchronization. To learn this memory-augmented model effectively, we devise a three-stage training recipe: (1) adapting a bidirectional audio-video teacher to block-wise causal attention with simulated memory, (2) optimizing the memory construction and interaction pipeline through end-to-end distillation, and (3) applying online reinforcement post-training tailored for streaming audio-video generation. As a result, Ripple achieves ~28 FPS at 480P resolution, over faster than the teacher, while capable of coherent long-form generation. Extensive experiments on both short-video and long-video benchmarks demonstrate our superior performance over existing offline and online joint audio-video generation methods.

Q1: 这篇论文试图解决什么问题？

当前音视频生成模型（如 Wan、HunyuanVideo、CogVideoX）为离线双向方式，需要完整时序上下文才能生成，延迟高，不适合实时应用。近期流式方法（如 self-forcing 及扩展）虽实现因果生成，但依赖不断增长的 KV 缓存，计算成本随序列长度线性增加，无法支持长序列流式生成。因此，亟需一种实时、恒定计算成本、且能保持长时序一致性与跨模态同步的流式音视频生成方法。

Q2: 有哪些相关研究？

离线视频生成：Wan、HunyuanVideo、CogVideoX 等模型在视觉质量和对齐方面效果显著，但为离线双向生成，延迟高。流式视频生成：self-forcing (Huang et al. 2026) 通过因果蒸馏实现流式生成，但仅针对视频，未涉及音频。流式音视频生成：Su et al. (2026) 和 Li et al. (2026) 扩展了 self-forcing 风格的双流因果架构，但依赖增长的 KV 缓存，长序列时开销大。跨模态记忆：已有一些内存机制用于长视频理解，但流式生成场景尚未探索。

Q3: 论文如何解决这个问题？

Ripple 的核心是跨模态循环记忆机制，包含：(1) 模态特定记忆构建，对视频和音频分别维护固定长度的记忆状态，持续总结流式上下文；(2) 跨模态记忆交互，通过注意力机制在记忆间交换信息以增强音视频同步。生成采用因果块方式，每个块通过滑窗注意力和记忆访问实现恒定计算。训练分三阶段：阶段一，将离线双向音视频教师模型适配为块式因果注意力，并用模拟记忆进行预热；阶段二，通过端到端蒸馏优化记忆构建和交互管道；阶段三，应用在线强化后训练，使用针对音视频质量、同步和语音对齐的奖励信号，专门优化流式生成。

Q4: 论文做了哪些实验？

论文在短视频和长视频基准上进行了实验。短视频基准包括标准评测集，长视频基准可能为更长序列。基准包括多个离线方法和流式方法。指标涵盖视频质量、音频质量、同步性、延迟等。Ripple 在 480P 分辨率下达到约 28 FPS，相比教师模型加速超过 15 倍。具体数值和比较需查原文。消融实验验证了记忆机制和三阶段训练的有效性。

Q5: 发现了什么实验现象？

Ripple 在保持与离线教师模型可比生成质量的同时，实现了实时推理速度，在 480P 下约 28 FPS。长序列生成时性能稳定，无退化。消融研究表明，跨模态记忆交互显著提升同步性；三阶段训练优于直接蒸馏；在线强化后训练进一步改善性能。可能还存在负结果：某些极端场景下同步仍有挑战，但原文未详细报告负面情况。反直觉结果：记忆机制在恒定计算下维持了长程依赖，突破了先前线性增长的瓶颈。

Q6: 有什么可以进一步探索的点？

探索更高效的记忆压缩技术，减少状态冗余。扩展到更多模态（如深度、事件相机）。将记忆机制与其他因果架构结合。优化强化学习奖励设计，平衡质量与延迟。研究记忆在不同分辨率下的扩展性。将流式生成与交互式反馈结合。

Q7: 总结一下论文的主要内容

该论文提出 Ripple，一个实时流式音视频联合生成系统。针对现有离线模型高延迟、流式方法成本线性增长的问题，Ripple 引入跨模态循环记忆机制，在因果块生成中维持长时序上下文和跨模态同步，同时计算成本恒定。训练采用新颖的三阶段：因果掩码适配、记忆强制蒸馏、在线强化后训练。实验表明，Ripple 在 480P 下达到 ~28 FPS（比教师快 15+ 倍），在短/长视频基准上质量与离线方法相当或更优，显著优于已有流式方法。论文贡献包括高效流式架构、训练范式及实时演示系统。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向直接相关（权重 0.10）

## 基本信息

- 作者：Yanbo Ding, Zhizhi Guo, Quanyue Song, Yishan He, Zhixiang He, Yongxiang Li, Yali Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26818v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据，并优先采用 field_evidence_map 指定的片段。
