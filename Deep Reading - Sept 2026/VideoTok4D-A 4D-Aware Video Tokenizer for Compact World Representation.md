---
user_id: "cheng tan"
paper_id: 11386
arxiv_id: "2609.12874"
title: "VideoTok4D: A 4D-Aware Video Tokenizer for Compact World Representation"
institution: "中国科学技术大学 (University of Science and Technology of China)"
publish_date: "2026-09-14"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.12874.pdf"
pdf_url: "https://arxiv.org/pdf/2609.12874"
abs_url: "https://arxiv.org/abs/2609.12874"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-14T10:21:38"
---
# VideoTok4D: A 4D-Aware Video Tokenizer for Compact World Representation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：video tokenization · d scene representation · spatiotemporal disentanglement · novel view synthesis

## 一句话总结

VIDEOTok4D 提出了一种 4D 感知的视频 Tokenizer，通过时空解耦和轨迹感知注意力机制，将视频转化为紧凑的场景表示，在实现高质量动态新视角合成的同时，将存储需求降低了四个数量级。

## 摘要

> Video tokenizers have emerged as a cornerstone of modern video modeling, underpinning progress in compression, reconstruction and generation by mapping high-dimensional visual signals into compact latent spaces. However, despite this progress, current tokenization paradigms largely remain within the 2D visual domain, treating videos as image sequences rather than observations of an underlying dynamic 3D world. Consequently, the learned tokens inherit this observation-centric bias, limiting their capacity to compactly represent real-world 4D scenes. To mitigate this issue, we propose VIDEOTok4D, a novel 4D-aware video tokenizer for compact world representation. Specifically, our approach comprises three key designs: 1) a spatiotemporal disentanglement strategy that factorizes videos into static and dynamic tokens for holistic world modeling; 2) a track-aware dynamic attention mechanism that aggregates trajectory-aligned cues to promote cross-view motion consistency; and 3) Co4DGEN, a diffusion prior learned over the resulting VIDEOTok4D token space for efficient 4D scene generation. Extensive experiments have demonstrated that our proposed method achieves state-of-the-art performance while requiring up to 4 orders of magnitude less storage than dense 4D representations. Moreover, the compact token space substantially shortens diffusion sequences, enabling efficient generation.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：以观测为中心（Observation-centric）的局限性
当前的视频 Tokenizer（如 Video-VAE/VQ-VAE 类方法）主要将视频视为在时间轴上排列的 2D 图像网格。这种范式存在以下深层问题：
1. **视角绑定（View-bound）而非视角不变（View-invariant）**：Token 被锚定在特定的图像坐标和时间点上。当相机轨迹发生变化时，这种表示难以维持几何和外观的一致性，导致在处理动态新视角合成（NVS）时表现不佳。
2. **冗余性与缩放问题**：密集网格结构导致 Token 预算随空间分辨率和时间长度线性甚至超线性增长。对于长视频或高分辨率场景，计算和存储开销巨大。
3. **缺乏 3D 世界理解**：现有的方法忽略了视频本质上是底层动态 3D 世界的投影。缺乏对场景静态背景与动态物体演化的显式区分，使得模型难以捕捉复杂的 4D 物理规律。

### 任务目标
本文旨在构建一种“以场景为中心”（Scene-centric）的 Tokenizer，能够从多视角视频中提取出紧凑的、具备 4D 感知能力的表征，既能支持高效的压缩与重建，又能作为生成模型的强大潜空间。

Q2: 有哪些相关研究？

### 视频 Tokenization 的演进
1. **2D/剪辑级 Tokenizer**：如 Zhao 等人的工作，侧重于高保真重建，但将视频视为 2D 测量序列。这类方法在生成任务中表现良好，但在处理 3D 空间一致性时存在天然缺陷。
2. **3D 感知 Tokenizer**：近期研究（如 Scenetok）开始尝试将视频解释为 3D 场景的观测。它们通过聚合跨视角信息来形成紧凑表示，但大多侧重于静态场景，对动态物体的处理仍显不足。

### 动态场景表示（4D Representations）
1. **密集 4D 表示**：如动态 NeRF 或 4D Gaussian Splatting。虽然重建质量极高，但存储开销巨大（通常每秒视频需要数百 MB），且难以直接用于生成模型的潜空间，因为其参数量级与生成效率不匹配。
2. **生成式先验**：现有的 4D 生成模型往往需要在推理时进行昂贵的逐场景优化，或者依赖于多视图扩散模型，缺乏一个统一且高效的 Token 空间来连接重建与生成。

Q3: 论文如何解决这个问题？

### 1. 时空解耦策略（Spatiotemporal Disentanglement）
VIDEOTok4D 将场景内容分解为两个分支：
- **空间分支（Spatial Branch）**：利用静态 Token 聚合场景中持久存在的背景信息。通过跨视角注意力机制，将不同视角下的静态特征融合到统一的参考空间中。
- **时间分支（Temporal Branch）**：利用动态 Token 捕捉随时间变化的物体演化。这种解耦允许模型分别优化静态几何和动态运动的建模效率。

### 2. 轨迹感知动态注意力（Track-Aware Dynamic Attention）
为了在改变视角时保持物体运动的一致性，本文设计了轨迹感知机制：
- **运动补偿**：通过估计物体的运动轨迹，补偿由于相机移动引起的位移。
- **特征聚合**：沿着估计的轨迹聚合动态特征。这确保了即使在剧烈的相机运动下，动态 Token 也能准确记录物体的物理运动轨迹，而非仅仅是像素级的变化。

### 3. Co4DGEN 扩散先验
在 VIDEOTok4D 压缩后的紧凑 Token 空间上，作者训练了一个扩散模型：
- **高效采样**：由于 Token 空间极度压缩，扩散模型的序列长度大大缩短，显著提升了生成速度。
- **联合状态采样**：只需采样一次联合世界状态，即可渲染出多个同步的目标视角视频，无需为每个视角重复运行 Token 先验。

Q4: 论文做了哪些实验？

### 实验设置
- **数据集**：在多个主流的动态多视角数据集上进行评估，包括具有复杂运动和相机轨迹的场景。
- **基准对比**：对比了 SOTA 的视频 Tokenizer（如 VTok, Vidtok）以及传统的密集 4D 表示方法。
- **评估维度**：
 1. **重建保真度**：使用 PSNR, SSIM, LPIPS 等指标衡量动态新视角合成的质量。
 2. **存储效率**：对比表示单个场景所需的字节数。
 3. **生成质量**：评估 Co4DGEN 在条件生成任务下的 FVD 和视角一致性指标。

### 硬件与实现
实验涵盖了从 Tokenizer 训练到下游扩散模型微调的全过程，验证了 Token 空间在不同任务下的通用性。

Q5: 发现了什么实验现象？

### 关键发现
1. **极高的压缩比**：VIDEOTok4D 在保持与密集 4D 表示相当的重建质量时，存储空间需求降低了 **4 个数量级**。这证明了 4D 感知 Token 空间的极度紧凑性。
2. **跨视角一致性**：得益于轨迹感知注意力，模型在处理快速移动物体和大幅度相机旋转时，表现出比传统 2D Tokenizer 强得多的几何一致性。
3. **生成效率提升**：在 Co4DGEN 实验中，由于 Token 数量减少，推理时间显著缩短，同时生成的视频在多视角同步性上表现优异。
4. **消融实验趋势**：
 - 移除时空解耦会导致静态背景出现“漂移”现象。
 - 缺乏轨迹感知会导致动态物体在视角切换时出现撕裂或模糊。
5. **负结果/挑战**：在极度遮挡或拓扑结构剧烈变化的场景中，轨迹估计的准确性会影响动态 Token 的建模质量，这是目前的一个性能瓶颈。

Q6: 有什么可以进一步探索的点？

1. **轨迹估计的鲁棒性**：目前依赖于外部轨迹估计工具，未来可以探索将轨迹学习直接集成到 Tokenizer 的端到端训练中。
2. **大规模预训练**：将 VIDEOTok4D 扩展到更大规模的无监督视频数据集，以学习更通用的 4D 世界先验。
3. **实时渲染集成**：探索将该 Token 空间与实时渲染引擎（如 4D GS）更紧密地结合，实现交互式的 4D 内容生成。
4. **长视频扩展**：研究如何通过滑动窗口或分层 Token 结构处理分钟级的动态 4D 场景。

Q7: 总结一下论文的主要内容

本文提出了 VIDEOTok4D，这是一种旨在解决视频建模中“以观测为中心”偏差的创新 4D 感知视频 Tokenizer。作者指出，传统的视频 Tokenizer 将视频视为 2D 图像序列，忽略了其背后的 3D 物理世界，导致在处理动态新视角合成和高效存储时存在局限。VIDEOTok4D 的核心贡献在于其时空解耦架构，通过空间分支提取静态背景 Token，通过时间分支提取动态物体 Token。为了解决动态场景中的视角一致性问题，引入了轨迹感知动态注意力机制，利用运动轨迹对齐特征，补偿相机运动带来的干扰。此外，作者在这一紧凑的 Token 空间上构建了 Co4DGEN 扩散模型，实现了高效的 4D 场景生成。实验结果令人印象深刻：在动态新视角合成任务中，VIDEOTok4D 不仅达到了 SOTA 水平，还将存储开销从数百 MB 降低到了 KB 级别（压缩比提升 4 个数量级）。这一工作为构建高效、具备物理常识的 4D 世界模型奠定了基础，展示了从 2D 像素建模向 4D 场景建模转变的巨大潜力。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该研究直接关联生成模型（Generation）方向，特别是 4D 内容生成。

## 基本信息

- 作者：Xinyi Chen, Hanxin Zhu, Xijun Wang, Xingrui Wang, Sen Liang, Xin Li, Zhibo Chen
- 机构：中国科学技术大学 (University of Science and Technology of China)
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-14
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.12874`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点提取了 Introduction 和 Conclusion 中的核心架构设计与实验结论。
