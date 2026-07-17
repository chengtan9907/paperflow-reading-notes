---
user_id: "cheng tan"
paper_id: 4337
arxiv_id: "2607.14935v1"
title: "VideoChat3: Fully Open Video MLLM for Efficient and Generalist Video Understanding"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.14935v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.14935v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-18T00:57:30"
---
# VideoChat3: Fully Open Video MLLM for Efficient and Generalist Video Understanding

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video understanding · multimodal large language model · efficient video tokenization · streaming video perception

## 一句话总结

VideoChat3 是一个完全开放、高效且通用的视频多模态大语言模型，通过 I3D-ViT 和自适应帧分辨率实现效率，并通过大规模多样化数据集提升泛化能力。

## 摘要

> Recent advances in video understanding have spanned motion, long video, and streaming interaction, driving this field toward real-world applications. Despite this progress, current open-source models remain limited in several ways. They often struggle to generalize across diverse video types, making them effective only in specific domains. High computational demands further restrict their efficiency and scalability. Moreover, most models are only partially open, with key components such as training code, strategy, or datasets unavailable, which hinders reproducibility and slows community-driven development. To address these issues, we introduce VideoChat3, a fully open, efficient, and generalist video-centric MLLM. VideoChat3 advances video understanding through two complementary designs. For efficiency, we introduce Inflated 3D Vision Transformer (I3D-ViT) and Adaptive Frame Resolution for Streaming Video Perception, which enables efficient spatiotemporal representation and reduces the cost of processing video inputs during training and inference. For effectiveness, we develop a scalable video data synthesis pipeline that curates three diverse, high-quality training datasets: VideoChat3-Academic2M, VideoChat3-LV116K, and VideoChat3-OL617K, covering general, long-form, and streaming video scenarios, improving the model's generalization across domains. By integrating these designs, VideoChat3 achieves a rare balance of broad generalization and computational efficiency. Experiments across general, long-form, and streaming benchmarks demonstrate that VideoChat3 surpasses prior open-source models with equal or larger parameter counts with only 4B parameters and higher efficiency. By releasing model weights, training code, training strategy, and complete training datasets, we aim to provide a fully reproducible foundation and help the open-source community bridge gaps in data access and training resources, fostering broader development of real-world video understanding systems.

Q1: 这篇论文试图解决什么问题？

当前视频理解领域的开源多模态大语言模型存在三个主要问题：1）泛化能力弱，只能在特定领域有效；2）高计算需求限制效率与可扩展性；3）开放程度低，训练代码、策略和数据集未公开，阻碍可复现性和社区发展。VideoChat3 旨在同时解决这些问题，实现高效、通用且完全开放的视频理解。

Q2: 有哪些相关研究？

1）视频多模态大语言模型：如 VideoChat、Video-LLaMA 等，多数未完全开放且效率有改进空间。2）高效视频编码方法：通过帧采样或时间聚合减少计算量，但常损失细节。3）流式视频理解：已有在线视频问答工作，但未与高效训练结合。本文的 I3D-ViT 和自适应帧分辨率借鉴了 3D 卷积和视觉变换器优势；数据合成管道受 ShareGemini 等启发。Qwen3-VL 等工作强调开放数据重要性，VideoChat3 更进一步完全开放所有组件。

Q3: 论文如何解决这个问题？

VideoChat3 通过两个互补设计：1）效率设计：I3D-ViT（预训练 2D ViT 膨胀为 3D，引入时间维建模）；自适应帧分辨率（根据视频内容动态调整分辨率，降低计算成本）；分块时间池化（Chunk-Wise Temporal Pooling）进一步压缩时序信息。2）数据设计：可扩展视频数据合成管道，产出三个数据集：VideoChat3-Academic2M（通用视频问答重标注）、VideoChat3-LV116K（长视频理解）、VideoChat3-OL617K（流式视频）。训练采用多阶段策略。

Q4: 论文做了哪些实验？

在通用视频理解、长视频理解和流式视频理解三大基准上评估。基线包括同等和更大参数规模的开源 MLLM。评估指标包括问答准确率等。VideoChat3 以 4B 参数在多个基准上超越先前模型。消融实验验证 I3D-ViT、自适应帧分辨率和各数据集的有效性。（具体数值和数据集名称基于摘要推断）

Q5: 发现了什么实验现象？

1）VideoChat3 以 4B 参数在通用、长视频、流式基准上超越更大开源模型，展示效率-性能平衡。2）自适应帧分辨率在流式场景显著降低计算量，保持准确率。3）各数据集对相应场景贡献明显，如 OL617K 提升流式问答。4）线索验证机制减少噪声定位，提高答案可靠性。5）可能的反直觉结果：小参数模型在长视频任务中仍能处理复杂时序依赖，得益于 I3D-ViT。6）消融显示分块时间池化在不损失精度下减少 token 数。

Q6: 有什么可以进一步探索的点？

1）扩展数据合成至更多领域（如 embodied、交互）。2）探索更大参数下的效率-性能权衡。3）应用 I3D-ViT 和自适应帧分辨率于视频生成等任务。4）优化流式处理实时性和延迟。5）结合强化学习或 RLHF 提升指令遵循。6）在监控、自动驾驶等实际场景验证。7）与智能体系统结合，驱动决策。

Q7: 总结一下论文的主要内容

本文提出 VideoChat3，完全开放、高效且通用的视频 MLLM。针对开源模型泛化、效率和开放性问题，引入 I3D-ViT 和自适应帧分辨率实现高效视频编码，并构建可扩展数据合成管道生成 Academic2M、LV116K、OL617K 数据集。仅 4B 参数即在多个基准上超越更大模型，实现泛化与效率平衡。作者开源权重、代码、策略和完整数据集，提供可复现基础。主要贡献：高效编码方案、大规模多样化数据集、完全开源。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本工作与智能体方向相关：流式视频感知能力可增强智能体对环境实时理解，支持在线决策。

## 基本信息

- 作者：Xinhao Li, Yuhan Zhu, Xiangyu Zeng, Yuhao Dong, Haoning Wu, Zhiqiu Zhang, Yuandong Yang, Changlian Ma, Qingyu Zhang, Yansong Shi, Xinyu Chen, Haoran Chen, Zizheng Huang, Jun Zhang, Kun Ouyang, Lin Sui, Ziang Yan, Yicheng Xu, Chenting Wang, Yinan He, Hongjie Zhang, Yi Wang, Yu Qiao, Yali Wang, Ziwei Liu, Kai Chen, Limin Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.14935v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（包括 abstract、method、results、limitations 相关片段）以及 field_evidence_map。
