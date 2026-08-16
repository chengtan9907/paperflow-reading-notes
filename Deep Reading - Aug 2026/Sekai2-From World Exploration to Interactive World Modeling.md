---
user_id: "cheng tan"
paper_id: 7440
arxiv_id: "2608.09449v1"
title: "Sekai2: From World Exploration to Interactive World Modeling"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.09449v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.09449v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-12T01:27:59"
---
# Sekai2: From World Exploration to Interactive World Modeling

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world model · video dataset · camera trajectory · long-horizon video generation

## 一句话总结

Sekai2 是一个多源真实世界视频数据集，在长视频中同时提供相机轨迹与时间对齐的层次化语义标注，并引入带环路和重访的全景序列，旨在将世界探索数据推进到交互式世界建模，为长时程视频生成、相机可控合成和世界模型预训练提供可扩展的真实数据资源。

## 摘要

> Video world models must capture how scenes evolve over time and across viewpoints. Training them for long-horizon generation and camera control therefore benefits from long videos paired with camera trajectories and temporally grounded semantics. Existing corpora rarely offer the three together: large-scale web video provides broad visual diversity but no trajectories or time-aligned text, while pose-annotated datasets are typically short-range or reconstruction-oriented. We introduce Sekai2, a multi-source real-world video dataset that carries the world-exploration footage of Sekai toward interactive world modeling. The release contains 128,892 clips totaling 2,826 hours from 10,428 source videos across 113 countries or regions, and is deliberately weighted toward sustained observation: under a common 120-second decomposition, 43,594 segments reach the full two minutes and account for 51.4% of all footage. Every clip includes a released camera trajectory and hierarchical annotations disentangling subject motion, environment dynamics, static scene content, and camera behavior, resulting in 649,597 temporally grounded segments. Crucially, we further introduce 982 panoramic sequences captured along non-linear trajectories with loops and revisits. These revisits provide repeated observations of the same locations across time and viewpoints, offering essential supervision for learning persistent scene representations, long-term spatial memory, and geometrically consistent world models. Corpus-scale analyses demonstrate complete pose-and-caption coverage, broad geographic and semantic diversity, varied camera trajectories, and highly non-redundant temporal descriptions. Together, these properties make Sekai2 a scalable resource for long-horizon video generation, camera-controllable synthesis, and interactive world-model pre-training.

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：真实世界视频数据中，'长时间连续性''相机轨迹''时间上对齐的细粒度语义'这三类监督信号难以在同一数据集中同时获得，导致长时程生成和相机可控的世界模型训练缺乏合适的真实数据基础。具体痛点包括：(1) 大规模网络视频虽然场景多样，但通常没有显式相机位姿，也没有与时间对齐的文本描述，模型难以从中学到几何一致的世界动态；(2) 已有的位姿标注数据集（如重建导向的 SLAM/多视图数据集）通常时长很短或局限于合成环境，不支持长时间跨度的观察和记忆学习；(3) 语义标注往往是视频级或片段级的整体描述，未将主体运动、环境动态、静态内容和相机运动解耦，难以提供独立、可组合的世界建模监督；(4) 缺乏对同一场景在不同时间、不同视角下的重访观测，阻碍模型学习持久场景表征和长期空间记忆。论文试图以数据为中心，通过构建 Sekai2 这样的语料来同时补齐这些维度，从而把'世界探索'的提升推进到'交互式世界建模'。

Q2: 有哪些相关研究？

相关研究可归纳为几条线：(1) 大规模真实世界视频数据集，如常见web video语料，强调视觉多样性但缺轨迹和时间对齐文本；(2) 带相机位姿的空间视频数据集，例如 SpatialVID、OmniWorld 等，引入相机位姿、深度等空间监督，但主要聚焦短视频片段或合成环境（基于命中片段，论文在 4.1 节与这些数据集比较）；(3) 世界模型探索类数据集，Sekai 是前序工作，提供第一人称和航拍视频用于真实世界探索，Sekai2 是在其基础上的延伸，从探索走向交互式建模；(4) 长时程视频数据集，强调时间连续性，但未将相机轨迹和层次语义统一；(5) 全景/360°视频数据集，用于场景理解，但缺少非线性的重访轨迹和长时间跨度。总体来看，已有工作大多只强化单一监督维度，而 Sekai2 的核心定位是'在同一个数据框架里同时提供长时间连续性、相机轨迹、细粒度时间语义和重访结构'，这是与现有数据集的主要区别。

Q3: 论文如何解决这个问题？

论文的解决方案是一个数据构建与标注的系统化流程，而非单一模型方法。核心思路是'在单一数据框架中联合提供长时程连续性、相机轨迹、时间上细粒度的层次语义以及重访结构'。具体做法为：(1) 从 Sekai 已有的世界探索素材出发，扩展源视频规模，多源收集覆盖 113 个国家或地区的第一人称与航拍视频；(2) 用统一的 120 秒分解方式切分视频，使大量片段达到完整两分钟长度，从而保证长时间连续性；(3) 对每个片段解算并释放相机轨迹，使数据具备几何一致性；(4) 设计层次化标注方案，将主体运动（subject motion）、环境动态（environment dynamics）、静态场景内容（static scene content）和相机行为（camera behavior）四类信息解耦，并生成与时间对齐的分段标注，最终得到 649,597 个时间上对齐的片段；(5) 专门引入 982 个全景序列，其轨迹是非线性的，包含环路（loops）与重访（revisits），从而提供同一地点在不同时间和视角下的重复观测；(6) 通过数据分布协议和许可约束处理版权问题，公开版本目前仅包含约 20 小时的原始素材，但整体分析基于全量数据。这些步骤使 Sekai2 具备可扩展性，并且每个片段同时具有位姿、语义和长期连续性。

Q4: 论文做了哪些实验？

论文的实验主要是语料级别的分析，而非模型训练对比。根据 5 Experiments 章节证据，论文从三个维度评估 Sekai2：视觉观察质量、相机几何、语义标注。具体实验包括：(1) 与现有数据集在规模、时长、位姿覆盖、标注时效性等方面进行比较（4.1 节对比 SpatialVID、OmniWorld 等）；(2) 对位姿覆盖和轨迹多样性进行统计，展示完整的 pose-and-caption coverage；(3) 分析地理和语义多样性，包括国家/地区覆盖数量和语义类别分布；(4) 对时间描述的非冗余性进行量化分析，说明不同时间片段内容不重复；(5) 对全景序列的环路和重访结构进行统计分析，验证其对持久场景表征的潜在价值；(6) 可能包含数据质量验证（如标注一致性和轨迹精度检查）。需要注意的是，当前证据片段未提供具体数值表格或基准测试结果，因此实验更偏向数据质量审计与语料统计，而非端到端生成模型评测。

Q5: 发现了什么实验现象？

从已获得的检索证据中可观察到的实验现象和结论包括：(1) 语料刻意偏重持续观察，120 秒分解下 43,594 个片段达到完整两分钟，占总素材 51.4%，这表明长时间片段占比较高，而非平均分布；(2) 与专注于短视频或合成环境的 SpatialVID、OmniWorld 等相比，Sekai2 强调长时间连续性和真实世界场景；(3) 语料具备'完整的位姿与字幕覆盖'，表明几乎所有片段都有轨迹和时序描述；(4) 地理和语义多样性广泛，覆盖 113 个国家或地区；(5) 相机轨迹多样，且时间描述高度非冗余；(6) 全景序列的重访结构能提供同一地点的跨时间、跨视角观测。由于证据有限，没有看到具体的定量消融或负结果。一个潜在张力是：公开释放的素材因许可限制仅约 20 小时，而全量分析基于内部更大数据集，这可能导致外部使用者无法完全复现统计分析。

Q6: 有什么可以进一步探索的点？

基于论文定位，未来可探索的方向包括：(1) 将 Sekai2 用于训练长时程视频生成模型，检验其在多分钟连续生成上的增益；(2) 利用重访全景序列学习持久场景表示、空间记忆和几何一致的世界模型，可尝试显式神经场景表示或 3D 高斯泼溅等作为 backbone；(3) 将相机轨迹作为条件标签，发展相机可控的视频合成，并探索轨迹表示（如绝对位姿与相对位姿）对生成质量的影响；(4) 利用层次化解耦标注（主体/环境/静态/相机）设计分解式生成框架，分离动态和静态建模；(5) 将时间对齐的细粒度语义用于视频描述生成、密集事件标注和交互式 agent 训练；(6) 扩展到更多地域和长尾场景，缓解地理和语义偏差；(7) 研究快速、低成本的轨迹与语义自动标注方法（如基于大模型的自动标注），减少人工成本；(8) 统一现有的短时空间数据集与 Sekai2 的长时程数据，形成从短到长的多尺度训练协议；(9) 探索公开 20 小时受限数据下的低资源适应方法，研究数据许可对可复现性的影响；(10) 将 Sekai2 与生物/科学应用结合，如生态监测、场景重建、长期变化分析等，对接用户画像中的 ai-for-science 偏好。

Q7: 总结一下论文的主要内容

本文提出 Sekai2，一个面向交互式世界建模的多源真实世界视频数据集。论文的论证主线是：视频世界模型需要同时获得长时间演化、视角变化和语义演变的信息，而现有数据往往只能提供其中一两种监督信号。大规模网络视频有视觉多样性但缺轨迹和时间文本；位姿标注数据短程且面向重建；语义标注通常不区分主体、环境、静态场景和相机四类因素。Sekai2 从 Sekai 数据出发，通过数据工程将这三类监督统一到同一数据框架中。技术主线上，Sekai2 构建了 128,892 个片段、总计 2,826 小时、来自 10,428 个源视频和 113 个国家或地区的语料；每个片段包含相机轨迹、层次化时间对齐标注（分主体运动、环境动态、静态内容、相机行为），共 649,597 个时间对齐片段；特别引入 982 个带环路和重访的全景序列，以提供同一场景跨时间、跨视角的重复观测。实验主线是语料规模与质量分析，验证了位姿-字幕全覆盖、地理和语义多样性、轨迹多样性、时间描述非冗余，以及与现有空间数据集（如 SpatialVID、OmniWorld）相比的长时间连续性优势。由于数据许可限制，公开版本仅约 20 小时，其余用于内部完整分析。整体上，Sekai2 不是简单扩展数据量，而是在'长时间连续性、相机轨迹、细粒度时间语义、重访结构'四个维度上进行联合设计，为长时程视频生成、相机可控合成和交互式世界模型预训练提供可扩展的真实资源。论文贡献在于：将多源真实视频加工成同时具有轨迹和层次语义的长时程语料；引入重访结构以支持持久场景学习；通过大规模统计展示语料质量。局限方面，公开数据的许可受限可能影响外部研究和复现；轨迹与标注的自动处理流程细节在现有摘要中不完整；缺少下游生成模型的实证验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 6 Conclusion / 5 Experiments / 2 3 Camera And Temporally Grounded Video Supervision 部分

## 基本信息

- 作者：Kang He, Wenshuo Peng, Zihui Gao, Jiaming Tan, Kaipeng Zhang, Yongtao Ge
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.09449v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 检索证据（background/method/results/limitations/relevance 等命中的摘要与结论片段），并结合 arXiv 元数据中的摘要进行补全；由于 PDF 正文未完整提供，部分实验细节和流程为合理推断或推测。
