---
user_id: "cheng tan"
paper_id: 478
arxiv_id: "2606.27999"
title: "HumanMoveVQA: Can Video MLLMs reason about human movement in videos?"
institution: "University of Surrey (Surrey Institute for People-Centred AI / CVSSP)"
publish_date: "2026-06-29"
pdf_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.27999.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27999"
abs_url: "https://arxiv.org/abs/2606.27999"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-29T15:04:17"
---
# HumanMoveVQA: Can Video MLLMs reason about human movement in videos?

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：video mllm · human movement · trajectory reasoning · orientation reasoning

## 一句话总结

HumanMoveVQA 是首个专门评估视频多模态大模型（MLLM）对人体全局轨迹和朝向推理能力的综合基准，包含 10K+ 结构化问答对。

## 摘要

> Despite the rapid advance of Multimodal Large Language Models (MLLMs) in
> high-level video understanding, a fundamental bottleneck remains: these models
> collapse complex human motion into coarse semantic labels. Existing bench-arXiv:2606.27999v1
> marks mostly focus on scene-centric events or local joint articulations, failing to
> probe global human motion in space over time (trajectory and orientation changes).
> We introduce HumanMoveVQA, the first comprehensive benchmark designed
> to evaluate global trajectory and orientation reasoning from an exocentric per-
> spective. Our benchmark utilizes a first-frame anchored world coordinate system,
> preserving translation and rotation relative to a fixed starting point. We propose a
> scalable, multi-stage pipeline that lifts 2D video observations into world-consistent
> 3D motion tracks to generate over 10K structured question-answer pairs across
> seven reasoning categories, including motion aggregation, sequential ordering, and
> trajectory-level inference. Our extensive evaluation reveals a critical capability gap
> in state-of-the-art proprietary models on deep human motion understanding. How-
> ever, we demonstrate that this is a learnable problem; by fine-tuning an open-source
> baseline with our targeted, world-consistent supervision, we achieve a significant
> improvement. HumanMoveVQA establishes a rigorous geometric foundation for
> developing next-generation, movement-aware video understanding models.
> Preprint.

Q1: 这篇论文试图解决什么问题？

### 核心挑战与瓶颈
1. **语义坍缩问题**：当前的视频理解模型通常将复杂的物理动作简化为“打网球”或“跑步”等高层标签，忽略了动作背后的物理本质，如移动速度、轨迹演变和朝向变化。
2. **空间推理缺失**：现有的视频语言数据集（如 Video-Text pairs）主要提供事件描述，缺乏关于人体在空间中“去往何处”以及“如何移动”的底层物理组件信息。
3. **基准测试局限性**：
 - **局部性**：现有基准（如 MotionLLM）多关注局部关节的细微动作，而非人体在环境中的全局位移。
 - **短时性**：多数测试集仅包含短视频片段，无法评估长时程（Long-horizon）的轨迹演化。
 - **坐标系不一致**：缺乏统一的世界坐标系参考，导致模型难以进行跨帧的几何一致性推理。

### 研究动机
这种信息瓶颈严重阻碍了 MLLM 在体育分析（如分析网球运动员的跑位）、自动驾驶（预测行人轨迹）和工业安全（监控危险区域侵入）等对物理运动敏感领域的应用。

Q2: 有哪些相关研究？

### 相关研究领域对比
1. **高层视频理解模型**：如 Gemini、GPT-4o 和 InternVL 等，虽然在视频问答和长视频理解上表现优异，但其训练数据多为高层描述，缺乏底层运动细节。
2. **细粒度动作识别**：近期研究开始关注关节级的运动（Pose-level），但这些研究往往局限于人体自身的姿态变化，忽略了人体作为整体在空间中的平移（Translation）。
3. **空间与几何推理**：虽然 3D 场景理解已有进展，但将动态人体运动与 3D 空间几何相结合的推理仍是空白。
4. **现有数据集**：如 EMDB、RICH 和 Egobody 等提供了高质量的 3D 人体运动捕捉数据，但缺乏与之配套的、用于评估语言模型推理能力的结构化问答对。

Q3: 论文如何解决这个问题？

### HumanMoveVQA 框架设计
1. **世界坐标系构建**：采用以视频第一帧为锚点的世界坐标系。这种设计确保了所有运动参数（位置、旋转）在整个序列中具有几何一致性，解决了相机运动带来的干扰。
2. **多阶段数据流水线**：
 - **3D 提升（Lifting）**：利用先进的单目 3D 人体姿态估计技术，将 2D 视频观测转化为 3D 运动轨迹。
 - **轨迹结构化**：提取人体的根节点位移（Root Translation）和全局朝向（Global Orientation）。
 - **自动 QA 生成**：基于提取的结构化运动数据，通过预定义的逻辑模板生成大规模问答对，避免了人工标注的主观性和不可扩展性。

### 推理类别划分（7 大维度）
- **存在性 (Existence)**：判断特定运动或朝向是否发生。
- **比较 (Comparative)**：对比不同时间段或不同维度的运动属性（如哪段位移更长）。
- **主导性 (Dominant)**：识别序列中最主要的运动方向或状态。
- **数值 (Numerical)**：对特定运动事件进行计数（如转身次数）。
- **排序 (Ordering)**：排列运动事件发生的先后顺序。
- **时序 (Temporal)**：定位运动发生的具体时间区间。
- **轨迹可达性 (Trajectory Affordance)**：推理人体在空间中的最终位置或潜在路径。

Q4: 论文做了哪些实验？

### 实验设置
- **评估模型**：包括闭源 SOTA 模型（GPT-4o, Gemini-1.5-Pro, Gemini-1.5-Flash）和开源模型（Qwen3-VL 8B, Video-LLaVA, InternVL 3.5, LLaVA-Video-NeXT 等）。
- **数据集来源**：主要基于 EMDB（包含 20-60 秒的长视频），并扩展至 RICH 和 Egobody 数据集以增强多样性。
- **评估指标**：采用分类准确率（Accuracy）和均值机会归一化得分（Score）。Score 考虑了不同类别选项数量（2 到 4 个）带来的随机猜测概率差异。

### 监督微调 (SFT)
- **基座模型**：Qwen3-VL 8B。
- **训练数据**：使用 HumanMoveVQA 生成的结构化运动监督数据进行微调，旨在验证运动推理能力是否可以通过学习获得。

Q5: 发现了什么实验现象？

### 关键实验发现
1. **SOTA 模型的集体失效**：即使是 GPT-4o 和 Gemini-1.5-Pro，在全局运动推理上的表现也远未达到理想水平。Gemini-1.5-Flash 虽然在“存在性”上表现尚可（80.4%），但在“比较”任务中甚至低于随机水平（30.0%），说明其无法有效聚合长时程信息。
2. **长时跟踪瓶颈**：GPT-4o 在“轨迹可达性”类别中得分极低（14.0），表明其在处理长视频中的人体持续跟踪和空间定位方面存在严重缺陷。
3. **语言偏见现象**：纯文本基线模型在某些类别（如“主导性”）上的表现竟然超过了 GPT-4o，这暗示现有模型在回答运动问题时可能更多依赖于语言先验（如“人通常向前走”）而非视觉证据。
4. **关节级监督的局限性**：实验对比显示，仅使用关节级（Joint-level）数据训练的模型（如 MotionLLM 方案）在全局轨迹推理上表现不佳，证明了全局位移监督的必要性。
5. **SFT 的显著增益**：经过针对性 SFT 后，Qwen3-VL 8B 的总分从 12.8 飙升至 37.9，提升了近三倍。特别是在“数值计数”（+40.9%）和“轨迹可达性”（+22.6%）方面进步巨大，证明运动推理是一项可学习的技能。

Q6: 有什么可以进一步探索的点？

### 未来研究方向
1. **增强长时程空间记忆**：开发能够维持长时几何一致性的模型架构，以解决轨迹丢失问题。
2. **多模态几何对齐**：探索如何将显式的 3D 坐标信息更有效地注入到 MLLM 的 Token 空间中，而非仅仅依赖视觉特征。
3. **复杂交互推理**：将基准扩展到多人交互、人机交互以及人体与复杂障碍物之间的空间关系推理。
4. **鲁棒性提升**：研究模型在极端光照、遮挡和剧烈相机运动下的运动推理稳定性。
5. **端到端运动生成**：利用 HumanMoveVQA 的发现，改进文本驱动的 3D 人体运动生成模型，使其具备更好的空间逻辑。

Q7: 总结一下论文的主要内容

本文提出了 HumanMoveVQA，这是一个旨在填补视频 MLLM 在人体全局运动推理领域空白的综合性基准。研究指出，当前的视频理解模型虽然擅长识别“发生了什么事件”，但在理解“人体如何在空间中移动”这一底层几何问题上表现极差。作者通过构建一个基于世界坐标系的自动化流水线，将 3D 运动捕捉数据转化为 10,000 多个涵盖七大推理维度的问答对。实验结果显示，包括 GPT-4o 在内的顶级模型在处理长时程轨迹聚合、运动计数和空间排序时存在显著缺陷，经常表现出严重的语言偏见或推理崩溃。然而，通过在 HumanMoveVQA 数据集上对开源模型 Qwen3-VL 进行监督微调，研究者成功将模型性能提升了近三倍，证明了全局运动推理能力可以通过高质量的几何监督数据习得。该工作为未来开发具备物理常识和空间感知能力的视频 AI 系统提供了重要的数据支持和评估框架。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：关注视频生成或理解中的物理一致性与几何推理

## 基本信息

- 作者：Pulkit Gera, Faegheh Sardari, Asmar Nadeem, Valentina Bono, Padraig Boulton, Adrian Hilton, Armin Mustafa
- 机构：University of Surrey (Surrey Institute for People-Centred AI / CVSSP)
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-06-29
- 推荐级别：**值得快速浏览**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.27999`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点提取了实验结果、方法论细节以及对 SOTA 模型缺陷的深入分析。
