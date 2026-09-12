---
user_id: "cheng tan"
paper_id: 10431
arxiv_id: "2609.02367v1"
title: "The Missing Temporal Link: Temporal Context Routing for Script-Driven Audio-Video Generation"
institution: "北京大学 (Peking University, DAGroup-PKU)"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02367v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02367v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:33:47"
---
# The Missing Temporal Link: Temporal Context Routing for Script-Driven Audio-Video Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：audio-video generation · script-driven generation · temporal context routing · shot transition control

## 一句话总结

本文提出了时间上下文路由（TCR）机制，通过将脚本指定的时间戳映射到音视频生成的共享时间轴上，实现了对剧本驱动生成中镜头切换和对话时机的精确控制。

## 摘要

> Joint audio-video generation models have made substantial progress in visual quality and audio-visual synchronization. However, they still provide limited control over when shot transitions occur and dialogue is spoken. This limitation constrains their application in script-driven content creation, where timing errors can undermine narrative coherence and the viewing experience. Current joint generators align video and audio representations on a shared temporal axis, yet the precise timing of shots and dialogue specified in a structured prompt is encoded only in the prompt's text representation and remains unaligned with the temporal coordinates of either modality. Consequently, video and audio may remain synchronized with each other while both fail to follow the script timeline. This mismatch motivates us to extend temporal alignment beyond video and audio to include the structured script. We therefore introduce Temporal Context Routing (TCR), which maps the script timing onto the shared temporal axis of video and audio generation and routes each prompt's guidance to the corresponding positions in both modalities. Compared with the baseline on 200 test scripts, TCR reduces Shot Boundary MAE by 96%, from 1.11 s to 0.042 s, and raises Dialogue Acc@0.5 s from 28.3% to 84.1%. TCR achieves these improvements while maintaining visual quality and audio-visual synchronization comparable to those of the baselines. A user study further shows that participants prefer TCR on all five evaluated dimensions.
> Project page: https://github.com/DAGroup-PKU/Temporal-Context-Routing

Q1: 这篇论文试图解决什么问题？

### 核心挑战：缺失的时间链接
剧本驱动的音视频生成要求模型不仅要生成高质量的内容，还要严格遵循剧本中定义的时间线（Timeline）。然而，现有模型面临以下核心问题：
1. **时间坐标失调**：虽然视频和音频在内部是同步的，但它们与剧本中指定的“第几秒发生什么”这一外部指令缺乏硬性的时间对齐。剧本中的时间信息被淹没在文本嵌入（Text Embeddings）中，模型难以将其精确映射到生成的帧或音频采样点上。
2. **多任务重叠与独立性**：镜头提示（Shot Prompts）和对话提示（Dialogue Prompts）在时间上可能重叠、包含或部分交错。例如，一段对话可能跨越多个镜头，或者一个镜头内包含多句对话。现有的全局注意力机制难以处理这种复杂的、具有特定时间跨度的局部引导。
3. **控制精度不足**：在长视频生成中，微小的时钟漂移会随时间累积，导致剧本与画面的严重脱节，破坏叙事逻辑。

### 任务定义
给定一个结构化剧本，其中包含多个带有起始和结束时间戳的镜头描述和对话文本，目标是生成一段音视频，使得视觉上的镜头切换和听觉上的对话起始点与剧本标注高度一致。

Q2: 有哪些相关研究？

### 联合音视频生成
近期研究（如 Ruan et al., 2023; Kondratyuk et al., 2024）利用扩散模型（Diffusion Models）在共享的潜在空间中同时生成视频和音频，解决了跨模态同步问题。然而，这些模型通常采用全局文本提示，缺乏对特定时间段内容的精细控制。

### 剧本驱动的生成
早期的剧本驱动研究主要关注视频序列的逻辑连贯性，但往往忽略了音频的精确配合。现有的可控生成技术（如 ControlNet 或 T2I-Adapter）多关注空间控制（如边缘、姿态），而在时间维度的精确控制（尤其是秒级以下的精度）仍是研究空白。

### 时间对齐技术
传统方法依赖于交叉注意力机制（Cross-Attention），但这种机制在处理长序列和复杂时间约束时，容易产生注意力弥散，无法保证特定时间点触发特定事件。

Q3: 论文如何解决这个问题？

### 时间上下文路由 (Temporal Context Routing, TCR)
TCR 的核心思想是将“何时生成什么”这一逻辑从模糊的语义理解转变为明确的路由机制：

1. **结构化脚本模式 (Structured Script Schema)**：
 - 将原始剧本转换为结构化表示，每个元素（镜头或对话）都带有明确的 $[t_{start}, t_{end}]$ 时间区间。
 - 使用检测到的视觉切片（Visual Cuts）和词级语音对齐（Word-level Speech Alignment）对初始时间戳进行精细化处理，网格精度达到 0.1 秒。

2. **时间映射与路由 (Temporal Mapping & Routing)**：
 - **共享时间轴映射**：将视频帧和音频潜在表示映射到统一的时间坐标系中。
 - **路由分数计算**：为每个提示词计算一个持续时间归一化的路由分数。该分数决定了在特定时间步长下，哪些提示词的引导信息应该被注入到生成过程中。
 - **局部引导注入**：通过修改注意力机制，使得在时间 $t$ 的生成仅受覆盖该时间点的剧本片段引导，从而实现“独立且重叠”的控制。

3. **多模态协同优化**：
 - TCR 同时作用于视频扩散路径和音频扩散路径，确保两者的引导源在时间上是完全一致的，从而在根源上消除了音画不同步以及与脚本脱节的可能性。

Q4: 论文做了哪些实验？

### 实验设置
- **数据集**：使用包含 200 个测试脚本的基准测试集，涵盖复杂的镜头切换和密集的对话场景。
- **基线模型 (Baselines)**：与当前最先进的联合音视频生成模型（未配备 TCR）进行对比。
- **评估指标**：
 - **Shot Boundary MAE (平均绝对误差)**：衡量生成的镜头切换点与剧本指定点的偏差。
 - **Dialogue Acc@0.5s**：对话起始时间误差在 0.5 秒以内的比例。
 - **音视频同步性 (AV-Sync)**：使用 SyncNet 等指标评估。
 - **视觉质量**：使用 FVD (Fréchet Video Distance) 等指标。

### 用户研究
邀请参与者在五个维度（视觉质量、音频质量、音画同步、剧本遵循度、整体偏好）上对 TCR 和基线进行盲测评分。

Q5: 发现了什么实验现象？

### 关键发现
1. **精度飞跃**：TCR 将镜头边界的 MAE 从 1.11 秒大幅降低至 0.042 秒，实现了近乎完美的镜头控制。对话准确率从 28.3% 提升至 84.1%，解决了长视频中对话“抢跑”或“滞后”的顽疾。
2. **质量保持**：在引入强时间约束的同时，视频的 FVD 指标和音频的质量指标并未下降，证明 TCR 是一种非侵入性的增强机制，不会破坏预训练模型的生成能力。
3. **鲁棒性**：即使在剧本中存在密集的短镜头（如 0.5 秒的快速剪辑）或重叠的背景音/对话时，TCR 依然能保持稳定的路由效果。
4. **反直觉现象**：实验发现，简单的文本拼接（将时间写在 Prompt 里）几乎无法提供任何有效的时间控制，这证实了模型在处理长文本时对数字/时间信息的忽略倾向，凸显了 TCR 这种结构化路由的必要性。

Q6: 有什么可以进一步探索的点？

1. **动态脚本调整**：目前 TCR 依赖于预定义的静态时间戳，未来可以探索如何让模型根据生成的视觉节奏动态微调脚本时间。
2. **更复杂的交互控制**：扩展 TCR 以支持更细粒度的控制，如特定动作的起始、特效的触发等。
3. **实时生成优化**：研究如何降低 TCR 在推理过程中的计算开销，以支持实时或交互式的剧本创作工具。
4. **跨长序列的一致性**：在极长视频（数分钟以上）中，如何结合 TCR 和长程记忆机制保持角色和环境的一致性。

Q7: 总结一下论文的主要内容

这篇论文针对剧本驱动的音视频生成中“时间失控”的核心痛点，提出了时间上下文路由（TCR）技术。作者指出，尽管现有的联合生成模型能产生高质量的音画，但它们无法精确遵循剧本中规定的镜头切换和对话时间，这是因为时间信息在文本编码中被模糊化了。TCR 通过在生成过程中引入显式的时间路由机制，将剧本中的时间区间直接映射到音视频的共享时间轴上，确保每个时间点的生成都受到正确剧本片段的引导。实验结果令人印象深刻：在镜头切换精度上实现了 96% 的误差缩减，并在对话对齐准确率上实现了数倍的提升。该研究不仅为专业内容创作提供了实用的工具，也为多模态生成中的精确时间控制提供了新的范式。通过 TCR，AI 导演能够像人类导演一样，精确地指挥每一帧画面和每一句台词的出现时机。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于关注 AI 视频生成和多模态对齐的研究者具有极高参考价值。

## 基本信息

- 作者：Yichen Liu, Quanwei Zhang, Haozhe Wang, Donghao Zhou, Xiaojie Li, Yang Shi, Jiaming Liu, Ruihua Huang, Yingtian Zou, Daquan Zhou
- 机构：北京大学 (Peking University, DAGroup-PKU)
- 来源：arxiv
- 主题/分类：cs.MM, cs.CV
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.02367v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是关于 TCR 的技术实现细节、实验数据（MAE 从 1.11s 降至 0.042s）以及机构信息（DAGroup-PKU）。
