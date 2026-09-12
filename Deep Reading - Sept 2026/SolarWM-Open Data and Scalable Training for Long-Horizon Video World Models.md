---
user_id: "cheng tan"
paper_id: 10429
arxiv_id: "2609.02886v1"
title: "SolarWM: Open Data and Scalable Training for Long-Horizon Video World Models"
institution: "Show Lab (National University of Singapore), Hong Kong University of Science and Technology, and other collaborating institutions."
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02886v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02886v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:33:40"
---
# SolarWM: Open Data and Scalable Training for Long-Horizon Video World Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：video world models · interactive video generation · long-horizon inference · data engine

## 一句话总结

SolarWM 是一个全开源的交互式视频世界模型基础框架，通过统一的数据引擎和骨干网络自适应方案，实现了从 5 秒短序列训练到长达数小时的长程推理。

## 摘要

> We introduce SolarWM, a fully open foundation for building interactive video world models from data preparation through long-horizon inference. Training across heterogeneous data sources and video backbones is challenging: datasets differ in temporal scale, camera geometry, visual quality, motion, and captioning styles, while video generators use distinct representations and architectures. Naive data mixing and model-specific implementations therefore produce inconsistent supervision and make results difficult to reproduce and compare. SolarWM addresses this coupling with a reconfigurable multi-source data engine and a backbone-native adaptation framework. The engine converts 1.43 million canonical clips from 10 datasets into a unified, frame-aligned contract covering visual observations, metric camera geometry, captions, quality metadata, selection decisions, and provenance, while decoupling source processing from mixture construction. Under shared camera-conditioning, training, and inference interfaces, we instantiate four 5B--33B models based on Wan2.2, LTX-2.5, and MiniMax-H3 while preserving their native representations and objectives. A unified three-stage recipe combines bidirectional adaptation, teacher-forced autoregressive initialization, and distribution matching distillation. The resulting causal models enable real-time interaction over rollouts ranging from minutes to hours after being trained on only 5s sequences. By releasing the resulting data, pipeline, recipes, weights, and framework, SolarWM provides a reproducible and extensible foundation for interactive world-model research.

Q1: 这篇论文试图解决什么问题？

### 核心挑战与痛点
1. **数据异构性 (Data Heterogeneity)**：现有的视频数据集在多个维度上存在严重不一致，包括时间尺度（帧率与时长）、相机几何（内参及运动轨迹）、视觉质量（分辨率与噪声）、运动模式以及文本描述风格。这种异构性导致朴素的数据混合无法提供一致的监督信号。
2. **模型架构耦合 (Model-Backbone Coupling)**：不同的视频生成模型（如 Wan2.2, LTX-2.5, MiniMax-H3）采用不同的潜在空间表示、预测目标和架构设计。现有的世界模型研究往往针对特定骨干网络进行硬编码，缺乏通用性。
3. **复现与比较困难**：由于缺乏统一的数据处理标准和训练协议，不同研究之间的结果难以直接对比，且闭源趋势阻碍了社区的协作进步。
4. **长程推理的训练成本**：在长序列上直接进行自回归训练计算成本极高，如何在短序列训练的基础上实现稳定、长程（分钟级甚至小时级）的视频生成是一个关键技术瓶颈。

Q2: 有哪些相关研究？

### 相关研究领域
1. **交互式视频世界模型 (Interactive Video World Models)**：如 BiWM、Lingbot-world 等，旨在通过动作或相机控制生成连续的视频序列。SolarWM 在此基础上强调了框架的通用性和开源性。
2. **训练框架与开源堆栈 (Training Frameworks & Open Stacks)**：论文对比了现有的视频生成训练流程，指出 SolarWM 提供了从底层数据清洗到高层模型蒸馏的完整链路。
3. **长程视频生成 (Long-horizon Video Generation)**：涉及自回归生成、状态空间模型以及扩散模型的长序列扩展技术。SolarWM 通过三阶段训练方案优化了这一过程。

Q3: 论文如何解决这个问题？

### 技术路线与架构设计
1. **可重构多源数据引擎 (Reconfigurable Multi-source Data Engine)**：
 - **规范合同 (Canonical Contract)**：将 10 个原始数据集的 143 万个剪辑转化为统一格式，包含视觉观测、度量相机几何、语言监督、质量元数据及来源追踪。
 - **解耦处理**：将源数据处理与混合构建分离，允许灵活调整数据配比而不必重新处理原始视频。
2. **骨干网络原生适配框架 (Backbone-native Adaptation)**：
 - 支持多种主流骨干网络（Wan2.2, LTX-2.5, MiniMax-H3），保留其原有的潜在表示和优化目标，通过共享的相机调节接口实现交互控制。
3. **统一三阶段训练配方 (Three-stage Training Recipe)**：
 - **阶段一：双向适配 (Bidirectional Adaptation)**：在双向注意力机制下进行预训练，捕捉全局时空特征，这是优化量最大的阶段。
 - **阶段二：教师强制自回归初始化 (Teacher-forced AR Initialization)**：将模型转换为因果（Causal）模式，通过教师强制快速收敛自回归预测能力。
 - **阶段三：分布匹配蒸馏 (DMD)**：通过分布匹配进一步提升生成质量，减少自回归过程中的误差累积，且该阶段所需的优化步数极少。

Q4: 论文做了哪些实验？

### 实验设计与设置
1. **模型规模**：涵盖了从 5B 到 33B 参数量的四个模型实例，验证了框架的可扩展性。
2. **训练数据**：使用处理后的 1.43M 规范剪辑，训练序列长度仅为 5 秒。
3. **评估场景**：
 - **分布外 (OOD) 生成**：使用 GPT Image 2 或 Krea 生成的图像作为初始帧，测试模型在未见场景下的 10 秒生成能力。
 - **长程回放 (Long-horizon Rollouts)**：测试模型在持续交互下的稳定性，时长从分钟级延伸至小时级。
4. **消融实验**：评估了三阶段训练中各阶段对最终性能的贡献，特别是双向预训练对后续自回归任务的促进作用。

Q5: 发现了什么实验现象？

### 关键发现与现象
1. **训练阶段效能**：实验观察到，大部分模型优化实际上在第一阶段（双向训练）就已经完成。第二阶段（AR 适配）收敛速度极快，而第三阶段（DMD）仅需极少步数即可显著提升视觉保真度。
2. **短训长推 (Short-to-Long)**：尽管训练时仅使用 5 秒的短视频片段，但通过合理的相机几何调节和自回归推理，模型能够生成长达数小时且逻辑连贯的视频流，表现出极强的时空外推能力。
3. **跨骨干网络一致性**：无论底层是哪种视频生成架构，统一的训练配方均能产生高质量的交互式世界模型，证明了该框架的通用性。
4. **指标张力**：在长程生成中，模型在保持相机轨迹一致性与维持视觉细节丰富度之间存在一定的权衡，DMD 阶段有助于缓解这种张力。

Q6: 有什么可以进一步探索的点？

### 可探索方向
1. **更大规模的数据扩展**：将数据引擎扩展到千万级甚至亿级剪辑，探索 Scaling Law 在交互式世界模型中的表现。
2. **多模态交互增强**：除了相机控制，引入更复杂的动作指令（如机械臂抓取、物体交互）和音频反馈。
3. **实时性优化**：进一步降低 33B 等大模型的推理延迟，以支持更流畅的实时交互应用。
4. **失败模式分析**：深入研究在极端运动或复杂遮挡场景下模型失效的原因，并开发相应的鲁棒性增强技术。

Q7: 总结一下论文的主要内容

SolarWM 论文提出了一套完整的开源交互式视频世界模型构建方案。其核心贡献在于打破了数据与模型之间的紧耦合：通过“数据引擎”实现了 10 个异构数据集的标准化，构建了包含 143 万剪辑的统一语料库；通过“适配框架”使得 Wan2.2、LTX-2.5 等不同架构的视频生成器能够共享同一套训练逻辑。论文详细阐述了一个高效的三阶段训练流程：首先通过双向训练打下坚实的特征基础，随后通过快速的自回归适配和分布匹配蒸馏，将预训练模型转化为具备长程生成能力的交互式世界模型。实验证明，该方法不仅效率高（仅需 5s 训练数据），而且泛化能力强，支持小时级的视频生成。通过开源数据、代码和 5B-33B 的模型权重，SolarWM 为社区提供了一个透明、可复现且易于扩展的研究基石，有望加速自动驾驶、机器人仿真及虚拟现实等领域的发展。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于关注智能体（Agent）在虚拟环境中学习的用户，该模型提供了高质量的物理模拟环境。

## 基本信息

- 作者：Junchao Huang, Guian Fang, Shengju Qian, Xianghao Kong, Zhuoran Zhao, Wei Huang, Yihua Du, Zixin Zhang, Justin Cui, Yuchao Gu, Yukang Chen, Xinting Hu, Tianyu He, Shaoshuai Shi, Zhuotao Tian, Xin Wang, Mike Zheng Shou, Li Jiang
- 机构：Show Lab (National University of Singapore), Hong Kong University of Science and Technology, and other collaborating institutions.
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.02886v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是关于数据引擎的 1.43M 剪辑规模、三阶段训练配方的具体步骤以及模型家族的构成（Wan2.2, LTX-2.5, MiniMax-H3）等关键细节。
