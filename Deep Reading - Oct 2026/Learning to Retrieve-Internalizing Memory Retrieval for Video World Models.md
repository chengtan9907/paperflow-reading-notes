---
user_id: "cheng tan"
paper_id: 14968
arxiv_id: "2610.11444v1"
title: "Learning to Retrieve: Internalizing Memory Retrieval for Video World Models"
publish_date: "2026-10-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.11444v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.11444v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T12:40:09"
---
# Learning to Retrieve: Internalizing Memory Retrieval for Video World Models

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video world model · memory retrieval · scene consistency · camera-conditioned generation

## 一句话总结

本文提出 Learning-to-Retrieve（L2R），将外部记忆检索机制内化为视频世界模型自身的状态更新行为——用相机姿态引导的检索门控决定“检索什么”、3D 重可见性信号监督的检索触发器决定“何时检索”，从而在不依赖外部记忆库和显式 3D 条件下提升长时程场景一致性。

## 摘要

> Video world models aim to generate explorable, 3D-consistent scene videos con-
> ditioned on camera trajectories. Existing approaches often rely on external mem-
> ory systems that explicitly retrieve previously observed content to mitigate scene
> drift during long-horizon generation. However, these auxiliary memory pathways
> operate outside the model’s internal generative dynamics, preventing the model
> from intrinsically learning when and what historical information should be re-
> trieved. We propose to internalize memory retrieval into the generation process,
> allowing retrieval to emerge as an intrinsic behavior of the video world modelarXiv:2610.11444v1
> rather than relying on an external memory system. Based on this principle, we
> introduce Learning-to-Retrieve (L2R), which repurposes the model’s persistent
> internal state as a memory for historical context. A camera-conditioned retrieval
> gate selectively accesses relevant historical information from this state, determin-
> ing what to retrieve, while a retrieval trigger determines when to retrieve. We fur-
> ther supervise the trigger with a 3D re-visibility signal, activating retrieval when
> previously observed content re-enters the current view while otherwise preserving
> the existing context. Together, these components enable the model to intrinsically
> acquire memory retrieval behavior and incorporate relevant historical observations
> into generation without a separate retrieval pathway. Across multiple base models
> and camera-revisit benchmarks, L2R improves long-term scene consistency while
> eliminating the need for an external memory bank or 3D conditions.
> ∗Equal contribution.
> †Project leader.
> ‡Corresponding author.
> 1
> Learning to Retrieve

Q1: 这篇论文试图解决什么问题？

1. 核心问题：视频世界模型按相机轨迹生成可探索场景视频时，当相机重新访问（revisit）之前观察过的区域，生成内容必须与之前的生成保持一致，即场景级一致性。长时程生成中容易发生场景漂移，导致重访内容不一致。
2. 范式一（条件生成/重建路线）：Yu et al. (2025c)、Huang et al. (2025)、Hu et al. (2026a)、Zheng et al. (2026)、Yang et al. (2026) 等工作用重建模型（Wang et al., 2025; 2026b）生成条件注入生成器，以提升视频保真与连贯性。论文指出该路线有效性受重建精度制约，重建误差会传播到下游生成，损害长时程质量。
3. 范式二（外部记忆检索路线）：把长期记忆作为辅助上下文模块，当过去帧与目标视角空间相关时检索出来增强默认短时滑动窗口上下文，代表工作包括 Li et al. (2025)、Yu et al. (2025a)、Xiao et al.、Yu et al. (2026b)、Wang et al. (2026c)。方法差异主要在检索策略（如相机重叠度选择）。关键缺陷：检索决策（是否检索、检索什么）由外部记忆通道完成，发生在核心生成动力学之外，模型自身无法内在地习得检索行为。
4. 论文要解决的缺口：能否将记忆检索内化为生成模型自身动力学的一部分——让模型在内部状态更新过程中自发决定何时、从何处、取哪些历史信息，从而不需要外部记忆银行与显式 3D 条件。
5. 隐含的两个子问题：(a) 内部化后的“何时检索/检索什么”决策如何形式化；(b) 触发器用什么信号监督才能学到与“重访场景”对应的检索时机。

Q2: 有哪些相关研究？

1. 视频世界模型：以参考帧或少量参考帧为条件、按用户指定相机轨迹生成探索 3D 场景的视频（Yu et al., 2025a; Xiao et al.; Zhu et al., 2026）。要求超越感知真实感与相机控制，满足场景级一致性。
2. 条件生成/重建约束路线：用重建模型生成条件注入生成器以提升保真与连贯（Yu et al., 2025c; Huang et al., 2025; Hu et al., 2026a; Zheng et al., 2026; Yang et al., 2026；重建模型：Wang et al., 2025; 2026b）。问题：重建误差传播限制长时程质量。
3. 外部记忆检索路线：长期记忆作为辅助上下文模块，当历史帧与目标视角空间相关时检索增强默认短时窗口（Li et al., 2025 即 VMem；Yu et al., 2025a; Xiao et al.; Yu et al., 2026b; Wang et al., 2026c）。早期方法按相机重叠度选择历史上下文。
4. 评测基线与基础设施：沿用 Echo-Memory（King et al., 2026）的相机条件方法、静态场景探索数据集（Yu et al., 2025a）与三支评测协议；对比 VMem、Context as Memory（K=20）、Echo-Memory、VideoSSM、Context-as-Prompt、No Memory 等。
5. 状态空间模型联系：本文在结论中提出“外部上下文记忆检索机制与状态更新动力学”的类比，并将内部检索与多种状态更新规则（GLA、GDN；VideoSSM 家族）结合，说明其思想与 state-space video world models 相关。
6. 定位：与外部记忆检索范式直接竞争——不是更换检索策略，而是取消外部通路，使检索成为状态更新的一部分；与重建条件范式相比，不引入重建误差，也不需要显式 3D 对应。
7. 说明：检索证据仅覆盖 Introduction/Method/Results/Conclusion 片段，个别引用工作（如各方法具体机制）为合理推断，精确对应关系建议回原文 Related Work 核对。

Q3: 论文如何解决这个问题？

1. 总体原则：内部化记忆检索（internalize memory retrieval），让检索作为视频世界模型内在状态更新动力学的一部分自然涌现，而非外部辅助模块。
2. 记忆载体：将模型持久内部状态 st 重新定义为历史上下文记忆，存储先前观察到的内容。
3. 两个核心决策组件：
 - 检索门控 γt（camera-guided retrieval gate）：决定“检索什么”。以相机位姿 pt 为条件，从内部状态 st−1 中选择相关历史信息，因为“当前视角是否重访历史内容”可由目标与上下文视角的几何重叠判断。
 - 检索触发器 rt（retrieval trigger）：决定“何时检索”。对当前 token xt 判定是否激活记忆检索；模仿理想世界模型行为——当目标视角与上下文视角几何重叠足够高时启动检索。
4. 状态更新形式化（Eq. 1 与 Eq. 3）：rt=1 时，ct=st=φ(ct,pt)，从状态检索得到的上下文与当前相机位姿共同送入状态更新规则 φ，生成新视频帧；rt=0 时保持 st−1，沿用现有上下文。
5. 触发器监督：用 3D 重可见性信号（3D re-visibility signal）监督触发器，当先前观察到内容重新进入当前视野时置为激活，否则保留上下文。该信号由 3D 几何目标计算，使检索时机具有几何可解释性。
6. 训练管线：将 L2R 集成进预训练相机可控视频世界模型训练。训练语料包含无相机回环的 5 秒片段（用于隔离记忆模块对长时程一致性的贡献、检验是否牺牲视觉保真度）与需要重访/回环的轨迹（用于学习触发器）。训练与推理设置与基线完全一致以保证公平（细节见 Appendix A）。
7. 部署特性：推理时无需外部记忆库、无需显式 3D 对应、无需显式 3D 条件注入；检索完全由模型内部状态与相机轨迹驱动。
8. 可实现性：与 state-update rule 解耦，实验验证了 GLA 与 GDN 两种状态更新规则（L2R-GLA、L2R-GDN），只需替换内部状态转移模块即可接入预训练模型。

Q4: 论文做了哪些实验？

1. 评价维度：沿记忆容量（memory capacity，长时程一致性）与视频质量（visual fidelity 与相机可控性）两个维度评价 L2R-GLA 与 L2R-GDN。
2. 基座与设置：以 Wan2.1-T2V-1.3B（Wan et al., 2025）为基座模型，沿用 Echo-Memory（King et al., 2026）的相机条件化方法与静态场景探索数据集（Yu et al., 2025a）；所有方法使用完全相同的训练与推理设置（Appendix A）。
3. 记忆容量三支评测协议：
 - Replay：按给定相机轨迹生成视频，用帧对齐 PSNR、SSIM、LPIPS 量化记忆容量。
 - In-domain loop closure：对匹配相机位姿的 outgoing 与 returning 帧用相同图像质量指标比较，衡量重访一致性。
 - Open domain return (O-V)：在数据集发布的 first-frame probes 上，用视觉语言 judge 评估场景级一致性与物体身份保持（object-identity preservation），检验对未见轨迹与未见视角重访的泛化能力。
4. 对比基线：VMem（Li et al., 2025）、Context as Memory（K=20）、Echo-Memory、VideoSSM、Context-as-Prompt、No Memory 等。
5. 训练语料隔离实验：额外准备无相机回环的 5 秒片段，用于验证记忆模块对长时程一致性的提升不是以牺牲感知保真度为代价。
6. 定性可视化：NYC Street 城市场景第一人称路线（穿过街道并返回同一排树街区），比较 No Memory、Context-as-Prompt 与 L2R-GLA 的重访一致性。
7. 说明：检索证据未包含 Replay/In-domain loop closure 的完整数值与消融表，具体指标数值、显著性检验与完整对比需对照原文表 1-3 核对。

Q5: 发现了什么实验现象？

1. 开放域返回（O-V）核心结果：L2R-GLA 达到 72.57，比 VMem 高 7.84 分，比 Context as Memory K=20 高 13.94 分。该协议使用 held-out first-frame probes，说明内部化检索对未见轨迹与视角重访的泛化能力强于外部记忆方法。
2. 提升幅度与基线强弱的规律（合理推断）：相对较强基线 VMem 提升约 7.84、相对较弱基线 Context as Memory 提升约 13.94，提升幅度大致与基线强弱成反比，且均为可观绝对提升；但缺乏误差条与显著性信息。
3. 内部化检索的有效性：在 O-V 这一开放域协议上大幅领先，说明“内部状态即记忆 + 几何监督触发器”的设计不只是记忆容量的增量改进，而是泛化能力的实质提升。
4. 质量-一致性权衡的检验：训练语料特意包含无回环 5 秒片段做隔离分析，说明作者主动验证了记忆模块不损害感知保真度与相机控制；这是对“一致性提升以画质下降为代价”潜在负结果的直接回应。
5. 定性现象：NYC Street 演示中，L2R-GLA 在往返轨迹上保持街道与树木的几何/纹理一致性；对比的 No Memory 与 Context-as-Prompt 出现场景漂移或内容不一致。
6. 跨状态更新规则的普适性：L2R-GLA 与 L2R-GDN 均有结果，显示该机制不绑定单一 state-update rule，可迁移到不同内部动力学。
7. 证据缺口：Replay 与 In-domain loop closure 的具体 PSNR/SSIM/LPIPS 数值、VideoSSM/Echo-Memory 的完整数值对比、消融（门控 vs 触发器单独贡献）未出现在检索片段中，需回原文确认。

Q6: 有什么可以进一步探索的点？

1. 更鲁棒的 3D 重可见性监督：当前用几何重叠/重可见性信号监督触发器，可探索用深度、点云、语义锚点等更稳的可见性估计处理遮挡、光照剧变与动态物体。
2. 动态场景扩展：从静态场景探索扩展到含移动物体、光照/天气变化的动态世界模型，重可见性信号需扩展为物体级身份追踪与遮挡推理。
3. 更长时程与记忆压缩：研究内部状态容量上限、状态遗忘与压缩机制；比较 GLA、GDN 等不同 state-update rule 的记忆容量边界，寻找更适合内部化检索的状态转移族。
4. 触发器自监督化：探索不依赖几何信号、直接从数据中涌现“何时检索”的自监督目标，或融合文本、深度、语义轨迹等多模态线索。
5. 内外部记忆混合架构：内部检索与外部记忆库并非互斥，可研究何时内部检索足够、何时必须外部检索，量化互补性。
6. 更大基座与 scaling：Wan2.1-T2V-1.3B 之外的更大模型、多模态基座；检索行为是否随模型规模涌现出类似 scaling law 的规律。
7. 更客观评测：开发 3D 几何对齐度量、跨视角像素级度量和人工评测，细化“记忆容量”与“生成质量”两维度的正交性验证。
8. 理论分析：把“外部检索 ↔ 状态更新”类比严格化，联系 SSM 线性注意力/指数衰减记忆的容量理论，为内部化检索提供理论边界。
9. 应用扩展：用于具身智能/agent 在线场景探索与规划（世界模型作为想象环境），研究在线生成的记忆管理与推理效率；以及 AI-for-Science 场景（内窥镜、遥感巡游、建筑考古重建）中按轨迹重访保持一致的可控生成。

Q7: 总结一下论文的主要内容

论文主线：视频世界模型要求在给定参考帧与相机轨迹时生成可探索、3D 一致的场景视频，重访已观察区域时保持场景级一致是核心要求。已有两条路线各自受限：重建条件生成路线受重建误差传播影响；外部记忆检索路线将“是否检索/检索什么”的决策放在模型生成动力学之外，模型无法内在地习得检索行为。
技术主线：作者提出 Learning-to-Retrieve（L2R），将记忆检索内化为状态更新动力学的一部分。模型持久内部状态被重新用作历史记忆；相机引导的检索门控 γt 以相机位姿为条件决定从状态中检索哪些历史信息（检索什么）；检索触发器 rt 决定何时激活检索，并通过 3D 重可见性信号监督——先前观察内容重新进入当前视野时激活、否则保留现有上下文。检索到的上下文与相机轨迹一起送入状态更新规则 φ 参与生成。L2R 可插入预训练相机可控视频世界模型，无需外部记忆库、无需显式 3D 对应或 3D 条件注入；支持 GLA 与 GDN 两种状态更新规则。
实验主线：以 Wan2.1-T2V-1.3B 为基座、沿用 Echo-Memory 相机条件设置与静态场景探索数据集，采用 Replay（帧对齐 PSNR/SSIM/LPIPS）、In-domain loop closure、Open domain return (O-V，VL judge 评估) 三支协议评测记忆容量，并单独评测视频质量以确认记忆机制不损害保真度与相机控制。对比 VMem、Context as Memory (K=20)、Echo-Memory、VideoSSM、Context-as-Prompt、No Memory 等基线。关键结果：L2R-GLA 开放域返回 O-V 达 72.57，超过 VMem 7.84 分、超过 Context as Memory K=20 13.94 分；NYC Street 定性实验显示重访时场景一致性优于无记忆与 prompt 上下文方法。训练语料包含无回环 5 秒片段以隔离验证记忆模块不以牺牲视觉保真度为代价。结论：L2R 在消除外部记忆检索器与显式 3D 条件的同时显著改善长程场景一致性，并建立了外部记忆检索与状态更新动力学之间的类比，为视频世界模型的记忆设计提供了内部化范式。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与生成方向直接相关（user profile 权重 0.1）：论文属视频生成/视频世界模型前沿，核心创新是把外部记忆检索内化为生成模型内部动力学行为。

## 基本信息

- 作者：JiaKui Hu, Tailai Chen, Yuqi Pan, Xuerui Qiu, Jialun Liu, Xiao Cao, Zhenxin Zhu, Guang Chen, Hangjun Ye, Bing Wang, Yanye Lu
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-08
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.11444v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（introduction/method/results/conclusion 共 8 条匹配片段），并结合论文摘要与章节文本进行重构；检索证据未覆盖的数值与消融细节已标注为需回原文核对。
