---
user_id: "cheng tan"
paper_id: 10105
arxiv_id: "2608.30294v1"
title: "Dynamic Hub-and-Spoke Memory for Streaming Video Understanding"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30294v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30294v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:50:05"
---
# Dynamic Hub-and-Spoke Memory for Streaming Video Understanding

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：streaming video understanding · hub-and-spoke memory · retrieval-augmented VLM · training-free framework

## 一句话总结

提出动态中心辐射记忆（D-HSM），一种无需训练的框架，通过实体中心的文本化记忆压缩长程历史，并结合近期视觉窗口动态检索问题相关证据，增强冻结 VLM 的流式视频理解能力。

## 摘要

> Streaming video understanding requires answering questions at arbitrary times over a continuously growing visual stream. The central challenge is to compactly remember long-range history while effectively retrieving question-relevant evidence. We propose Dynamic Hub-and-Spoke Memory (D-HSM), a training-free framework that represents distant history as structured textual memory while preserving the recent frames as visual tokens for fine-grained perception. Specifically, D-HSM turns selected historical video chunks into typed textual observations and stores them in an entity-centered hub-and-spoke memory, with entities as hubs and related evidence as spokes. When answering a question, D-HSM dynamically retrieves a compact question-aware memory subset, expands it through hub-and-spoke links, and combines it with the recent visual window for frozen-VLM answer prediction. Extensive experiments on both streaming and long video benchmarks show that D-HSM consistently and substantially improves VLM backbones and outperforms other state-of-the-art online and offline video understanding baselines.

Q1: 这篇论文试图解决什么问题？

流式视频理解面临双重挑战：(1) 长程历史记忆的紧凑表示：视频流无限增长，模型必须压缩历史信息，避免存储全部帧或冗长文本；(2) 问题相关证据的精准检索：不同问题需要不同时间粒度和语义范围的历史证据，例如关于当前时刻的问题需要细粒度视觉细节，而关于过去事件的问题则需要跨时间推理。检索片段中的 Introduction 明确提出了第一个子问题——‘流式模型如何将长程历史组织为结构化语义记忆？’，第二个子问题（可能对应‘②’）提及不同问题需要不同证据。因此，问题本质上是一个记忆组织与查询检索的联合优化问题，且要求无需重放原始视频、不需训练即可适配任意冻结 VLM。

Q2: 有哪些相关研究？

相关研究可归为以下几个方向（合理推断）：(1) 流式视频理解（Streaming Video Understanding）：代表性基准即文中提到的 StreamingBench (Lin et al., 2024b) 和 OVO-Bench (Li et al., 2025b)，强调在线实时问答；(2) 长视频语言建模：现有方法通常通过采样帧、视频摘要、或记忆网络（Memory Network）来处理长上下文，但往往牺牲细粒度感知或需要训练；(3) 检索增强 VLM（Retrieval-Augmented VLM）：通过相似度检索从视频库中提取相关片段，但流式场景下需在线索引和动态更新；(4) 视频问答的记忆机制：如分层记忆、外部存储、或基于 Transformer 的长短期记忆。D-HSM 的独特在于将历史以结构化文本形式组织成 hub-and-spoke 图，无需训练，直接赋能冻结 VLM。

Q3: 论文如何解决这个问题？

D-HSM 是一个训练无关（training-free）的框架，整体流程由三部分组成：(1) 历史编码：将选定的历史视频块转化为带类型（typed）的文本观察（如人物、物体、动作、事件等），这些文本观察作为记忆的内容；(2) 记忆组织：以实体（entity）为中心构建 hub-and-spoke 结构，实体充当 hub（中心节点），其相关证据作为 spoke（辐条）连接，形成可导航的记忆图；(3) 动态检索与预测：给定问题，首先动态检索一个紧凑的问题感知子集，然后通过 hub-and-spoke 链接扩展该子集（例如从实体扩展到相关证据），最后与近期视觉窗口（recent visual window）的 token 拼接，输入冻结 VLM 生成答案。近期视觉窗口保留细粒度视觉信息，弥补文本化历史损失的视觉细节。方法完全无需训练，只依赖已有 VLM 的零样本能力。

Q4: 论文做了哪些实验？

论文在以下维度进行了实验（基于摘要和检索片段归纳，无具体数值）：(1) 基准：流式基准（StreamingBench、OVO-Bench）以及离线长视频基准，覆盖实时感知、长时间推理和多跳问题；(2) 基线对比：与现有在线（online）和离线（offline）视频理解方法对比，显示 D-HSM 在流式和长视频任务上均优于基线；(3) 消融实验：包括对近期视觉窗口大小的研究（Table C1），增加近帧数一致提升 StreamingBench 性能；以及对比不同记忆组件（如只有近帧 vs 完整 D-HSM），发现仅近帧在实时感知任务上较强，但长时推理与上下文一致性受限；(4) 可视化分析（附录 D）：展示 hub-and-spoke 记忆的具体示例。

Q5: 发现了什么实验现象？

从检索到的消融片段可以直接观察到以下现象：(1) ‘s Only’ 基线（可能指只有 spoke 或只有近期帧的变体）在实时感知任务上表现强，但在需要较长时序推理和上下文一致性的任务上能力有限，说明单一视觉记忆不足以支撑长时理解；(2) 近期视觉窗口大小的增加持续改善 StreamingBench 性能，表明细粒度的当前感知对于流式问答至关重要；(3) 将近期帧观测与提出的记忆结构整合（即完整 D-HSM）后，模型能在实时感知和长时推理之间取得平衡，从而整体超过基线。这些现象反直觉地表明，即使 VLM 冻结，纯粹的外部结构化文本记忆也能有效补足其长程推理短板。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：(1) 可学习的记忆压缩：当前方法用规则或 VLM 生成文本观察，未来可训练轻量化的记忆编码器以压缩更丰富语义；(2) 更复杂的图结构：不只是 hub-and-spoke，可扩展到层级或多关系图，支持更灵活的推理路径；(3) 动态遗忘与更新：设计显式的遗忘机制，防止过期信息干扰，并适应概念漂移；(4) 与智能体结合：D-HSM 的记忆检索机制可直接用作 agent 的长期记忆模块，支持多轮交互中的上下文回忆；(5) 跨模态扩展：将文本观察替换为多模态摘要（音频、深度图等），适用于更普遍的流式感知；(6) 统一长视频与流式范式：探索从离线长视频到流式的迁移，以及联合训练 VLM 与记忆模块的潜力；(7) 真实场景测试：在监控、第一人称视频等实际流式场景中验证可扩展性。

Q7: 总结一下论文的主要内容

流式视频理解要求系统在任意时刻对持续增长的视频流进行问答，既要记住遥远过去的关键信息，又要感知当前时刻的细粒度细节。现有方法要么保留全部历史导致计算膨胀，要么仅用最近帧丢弃长程依赖，难以兼顾。D-HSM 提出一种无需训练的框架，将长程历史组织成结构化文本记忆，同时保留近期视觉窗口。具体而言，它把历史视频块转化为带类型的文本观察，并以实体为中心构建 hub-and-spoke 记忆结构：实体（人、物体、地点等）是中心节点，相关的证据文本作为辐条。当问题到来时，D-HSM 先动态检索出紧凑的问题感知子集，再沿 hub-and-spoke 链接扩展至相关证据，最后与近期视觉 token 一起输入冻结的 VLM 完成答案生成。这种设计让 VLM 无需重新观看视频即可'回忆'过去，同时不丢失当前的视觉细节。实验在 StreamingBench、OVO-Bench 等流式基准以及离线长视频基准上进行，显示 D-HSM 持续改善各种 VLM 骨干，并超越在线和离线基线。消融研究验证了组件有效性：近期视觉窗口增加改善实时感知，而仅有近帧的基线在长时推理上显著退化。可视化案例进一步展示了记忆结构如何支持不同时空粒度的问答。整体上，D-HSM 提供了一个简洁、即插即用的记忆增强方案，可作为流式视频理解系统的通用组件。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：D-HSM 的训练无关记忆与检索机制可直接应用于 agent 系统，作为长期记忆模块支持任意时刻的上下文问答。

## 基本信息

- 作者：Xinru Jiang, Lin Zhao, Xi Xiao, Yunbei Zhang, Janet Wang, Chenrui Ma, Haolin Li, Yanzhi Wang, Yifan Gong, Octavia Camps
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30294v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告基于摘要和部分 PDF 检索证据生成，未参考完整论文正文，部分内容为合理推断。
