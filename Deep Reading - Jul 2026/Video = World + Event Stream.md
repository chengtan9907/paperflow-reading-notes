---
user_id: "cheng tan"
paper_id: 4364
arxiv_id: "2607.15038v1"
title: "Video = World + Event Stream"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.15038v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.15038v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-18T00:58:02"
---
# Video = World + Event Stream

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video generation · real-time interaction · multimodal understanding · world model

## 一句话总结

Wan-Streamer v0.3 将视频建模为“世界+事件流”，并用于实时全双工音视频交互。

## 摘要

> We present Wan-Streamer v0.3, which reframes our native-streaming interaction model under a single organizing view: a video is a world plus an event stream. The world is the persistent context in which a video unfolds, including the environment, scene, subjects, ambient acoustic conditions, voice characteristics, and other relatively stable conditions. The event stream is everything that changes over time within that world, including scene or environmental changes, subject behavior, speech, and other sounds. This yields a general-purpose pretraining task over large amounts of real video: given a world and incoming input, predict how the world moves, changes, and responds in real time. The resulting competence can be specialized to a broad family of real-time downstream tasks. We instantiate it on real-time full-duplex audio-visual interaction, where the event stream is the agent's speech together with free-form behavior. Functionally, the model's multimodal understanding process is vision-language-action-like: it maps multimodal user input to language-form speech and behavior actions. Wan-Streamer v0.3 preserves the v0.2 operating point: 640x368 video at 25 FPS, a 160 ms streaming unit, approximately 200 ms model-side response latency, and approximately 550 ms total interaction latency under a 350 ms bidirectional network budget.

Q1: 这篇论文试图解决什么问题？

论文试图解决实时全双工音视频交互中的一个核心问题：如何高效地建模视频中持久上下文与动态变化，以支持低延迟的流式生成。现有方法通常将视频视为连续帧序列，缺乏对持久世界的明确显式建模，导致在多轮交互中难以保持上下文一致性。本文提出世界+事件流分解，将稳定场景与变化事件分离，从而使得模型可以专注于预测事件而对世界保持恒定记忆，降低冗余计算并提高交互连贯性。该问题在实时交互、数字人、具身智能等领域具有广泛意义。

Q2: 有哪些相关研究？

相关工作包括实时视频生成模型（如Gen-3, Kling）、多模态交互系统（如GPT-4o, Gemini Live）、流式音频-视觉模型（如Streaming Transformer）。本文工作与前述不同之处在于：明确将视频分解为世界和事件流，并以此作为预训练目标，从而统一多种实时下游任务。此外，论文介绍了Wan-Streamer系列从v0.2到v0.3的演进，其中v0.3新增自由形式行为流。但目前缺乏与同类型系统的直接对比实验。

Q3: 论文如何解决这个问题？

论文的核心方法是世界-事件流分解（World-Event Stream Factorization）。具体而言，定义世界（World）W为视频中随时间保持相对稳定的因素，包括场景、环境、主体外貌、环境声音、语音特征等；事件流（Event Stream）E为随时间变化的一切，包括场景变化、主体行为、语音内容等。基于此，提出通用视频预训练任务：给定世界W和输入事件流E_in（用户输入），预测响应事件流E_out（模型输出，包括语音和行为）。模型通过流式架构持续处理多模态输入（文本、音频、视频），输出交织的语音和行为标记（interleaved speech-and-behavior token stream），并以此条件生成联合的音频-视频潜在表示。关键设计是：模型的生成直接在事件流上进行，而不对世界进行重复解码，从而保持低延迟。该预训练任务可迁移到多种实时下游任务，本文实例化为实时全双工音视频交互。

Q4: 论文做了哪些实验？

论文未提供详细的实验验证部分。主要呈现了系统在实时交互场景下的运作方式，并报告了运营点指标：视频分辨率640×368，帧率25FPS，流单元大小160ms，模型端响应延迟约200ms，在350ms双向网络预算下总交互延迟约550ms。论文提到从v0.2到v0.3的关键变化是事件流现在携带自由形式行为与语音。但缺乏与其他基线的定量对比、用户研究或消融实验。因此，无法根据本文内容评估该方法的实际性能提升程度。

Q5: 发现了什么实验现象？

由于论文未提供实验数据，未观察到具体的实验现象。仅从描述可知，系统能实现约550ms总交互延迟，且模型响应用户的流式文本、音频和视频输入。事件的分解使得模型能够生成与用户输入同步的语音和行为。但无负结果或失败案例报告。

Q6: 有什么可以进一步探索的点？

根据论文结论，该分解支持通用视频预训练并可迁移到多种实时下游任务（roaming downstream tasks）。未来可探索的方向包括：1）将世界模型扩展到更长时间跨度，处理世界自身的缓慢变化；2）探索在其他实时任务中的应用，如机器人操控、远程监控、虚拟现实等；3）改进事件流的建模，支持更复杂的用户行为和多智能体交互；4）充分利用大规模真实视频进行预训练以提升世界理解的泛化性；5）降低网络延迟和计算开销以支持更低的交互延迟。此外，该分解是否适用于不同类型的视频（如快速变化的电影或体育）需要进一步验证。

Q7: 总结一下论文的主要内容

本论文介绍Wan-Streamer v0.3，提出将视频视为世界加事件流的组织视图。世界包含持久上下文（场景、环境、主体、声音特征等），事件流包含随时间的变化（行为、语音、场景变化等）。基于此，设计通用预训练目标：给定世界和输入，预测世界的实时响应。该预训练能力可迁移至实时全双工音视频交互等下游任务。模型采用视觉-语言-动作式的多模态理解过程，将用户输入映射为语言形式的语音和行为动作，形成交织的事件流，直接条件生成音频-视频。系统规格保持与v0.2一致：640×368@25FPS，160ms流单元，约200ms模型响应延迟，550ms总交互延迟。论文主要贡献包括：提出world+event stream分解，给出通用预训练任务，并实例化实时交互系统。但未提供定量实验验证。整体上，该工作为实时流式多模态交互提供了一种概念框架。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作属于实时多模态交互与生成领域，与智能体（agent）方向有直接重合

## 基本信息

- 作者：Lianghua Huang, Zhi-Fan Wu, Yupeng Shi, Wei Wang, Mengyang Feng, Cheng Yu, Chen Liang, Junjie He, Chen-Wei Xie, Yu Liu, Jingren Zhou, Ang Wang, Bang Zhang, Baole Ai, Chongyang Zhong, Jinwei Qi, Kai Zhu, Pandeng Li, Peng Zhang, Wenyuan Zhang, Xinhua Cheng, Yitong Huang, Yun Zheng, Yuxiang Bao, Yuzheng Wang, Zhiwei Lin, Zoubin Bi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.15038v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据，包括摘要、引言和结论片段。
