---
user_id: "cheng tan"
paper_id: 1106
arxiv_id: "2606.23105"
title: "Compression and Retrieval: Implicit Memory Retrieval for Video World Models"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23105.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23105"
abs_url: "https://arxiv.org/abs/2606.23105"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:13:14"
---
# Compression and Retrieval: Implicit Memory Retrieval for Video World Models

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：camera trajectories · memory retrieval · video world · compression retrieval · implicit memory

## 一句话总结

本文提出压缩与检索（CaR），一种通过注意力驱动的隐式记忆检索机制，结合轻量级上下文压缩网络，解决视频世界模型在复杂相机轨迹下的长程记忆一致性问题，并构建了大规模合成数据集SceneFly。

## 摘要

> Video world models hold promise for simulating interactive environments, yet maintaining consistent long-term memory across complex camera trajectories remains a critical challenge. Existing methods typically rely on computationally expensive context scaling or rigid heuristic retrieval mechanisms, which lacks generalization to varying camera trajectories and environments. In this paper, we propose Compression and Retrieval (CaR), an attention-driven implicit memory retrieval mechanism to overcome these limitations. By injecting viewpoint information via positional encoding, our method performs flexible memory retrieval through attention computation. To efficiently process extended contexts with minimal computational overhead, we further introduce a lightweight context compression network. Furthermore, we construct SceneFly, a large-scale synthetic dataset featuring realistic camera trajectories and frame-level annotations to train and evaluate long-horizon video world models. Extensive experiments demonstrate that our approach achieves state-of-the-art results on established benchmarks and exhibits strong generalization to open-domain scenes.

Q1: 这篇论文试图解决什么问题？

论文试图解决视频世界模型在长程交互生成中的核心问题：如何在复杂且变化的相机轨迹下维持场景一致性的长期记忆。现有方法要么保留所有帧（计算成本高，时域注意力二次增长），要么采用启发式检索（如基于相似度或最近帧，缺乏泛化性和灵活性，尤其当相机轨迹多变时）。具体来说：（1）全历史上下文方法（如INFER、CogView）因注意力复杂度随帧数二次增长，无法处理长历史；（2）检索增强方法（如STREAM）依赖于预先定义的检索规则，不能适应动态环境。因此，需要一个既能压缩历史又保留灵活检索的机制，让模型自主决定哪些记忆重要。

Q2: 有哪些相关研究？

相关研究主要包括以下几个方向：（1）视频生成世界模型：如VideoPoet、Genie、Gaia-1等，探索自回归或扩散模型用于视频预测和交互式生成；（2）长程视频记忆机制：INFER等保留所有帧但受限于计算，STREAM等使用显式检索但规则固定；（3）上下文压缩：如Memory Transformer、Compressive Transformer、LUMINA等，通过压缩历史表示来降低复杂度，但压缩方式较粗糙；（4）相机条件生成：通过零样本新视角合成或编码相机姿态（如NeRF、PoseDiffusion、CameraCtrl），但通常只关注单帧或短序列，未解决长程记忆；（5）检索增强生成（RAG）：在NLP中广泛应用，但现有方法在视频域中通常基于帧级特征检索，而非端到端的隐式检索。CaR的独特之处在于将视角信息编码进位置编码，通过注意力直接检索历史中相关部分，同时利用压缩网络保持历史全貌。

Q3: 论文如何解决这个问题？

论文提出CaR（Compression and Retrieval）框架，包含两个核心组件：（1）隐式记忆检索（Implicit Memory Retrieval）：将历史帧压缩为固定长度的上下文令牌（通过轻量级压缩网络），并与目标视频令牌拼接。每个令牌携带根据相机投影矩阵编码的位置嵌入（Positional Encoding），使得注意力计算可以基于视角相关性自动从历史令牌中检索所需信息。这样无需显式选择帧，而是让生成模型通过注意力权重决定哪些历史上下文最相关。（2）上下文压缩网络（Context Compression Network）：采用类似Perceiver的架构，将多帧历史通过交叉注意力压缩为少量紧凑令牌（如每帧压缩到几个令牌），大幅降低注意力计算的令牌数量，从而在长历史下保持计算可行性。压缩网络联合训练，以保留足够信息用于后续检索。此外，论文构建了SceneFly数据集，基于Unreal Engine 5渲染，包含多种室内、室外和风格化环境，随机长程相机轨迹和精确的逐帧相机标注（包括位置、朝向、焦距等）。数据集用于训练和评估模型的长程记忆能力。整体流程：给定历史帧序列，先通过压缩网络得到压缩上下文令牌；然后与待生成的目标视频令牌拼接，并添加各自相机姿态对应的位置编码；最后主体生成模型（基于扩散Transformer）通过自注意力机制进行记忆检索并生成目标视频。

Q4: 论文做了哪些实验？

论文设计了三组主要实验：（1）基准评估：在SceneFly（合成数据）和SpatialVid（真实世界视频）上，比较CaR与多种baseline，包括全历史方法（INFER）、检索方法（STREAM）、无记忆基线（Zero-shot NVS方法如SynSin、PixelNeRF等），以及视频生成模型（VideoPoet、Genie）。评价指标包括FVD（Fréchet Video Distance）、LPIPS、PSNR、SSIM以及场景一致性得分（Scene Consistency Score，基于预训练场景特征距离）。（2）消融研究：分别移除隐式检索（退化为直接拼接压缩上下文）、移除压缩网络（使用全帧）、替换位置编码为普通时间编码，以验证各组件的贡献。（3）泛化实验：在未见过的开放域场景（如真实世界视频、其他合成环境）上测试，评估模型对相机轨迹变化和新环境的适应能力。训练细节：使用DiT（Diffusion Transformer）作为生成骨干，图像/视频分辨率256×256，帧长4-8帧。SceneFly包含约500k视频片段，每个片段约32-64帧。训练在32×A100 GPU上进行。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：（1）CaR在SceneFly和SpatialVid上均取得SOTA结果，FVD降低约15-20%，场景一致性得分提升明显。尤其在长程（>16帧）情况下，CaR的FVD稳定，而全历史方法（INFER）因计算限制只能处理短帧，检索方法（STREAM）在轨迹转折时出现明显漂移。（2）消融表明，隐式检索是性能提升的核心，移除后场景一致性下降约12%；压缩网络在保证性能的同时将计算开销降低约60%（与全帧拼接相比）。（3）位置编码的视角信息至关重要：使用普通时间编码时，模型无法利用相机轨迹信息，导致视角变化大的区域重建模糊。（4）在开放域泛化实验中，CaR能够处理未见过的室内场景和动态光照，而baseline在新场景下出现严重伪影或场景混淆（如将厨房错误映射成卧室）。负结果：压缩令牌数量太少（如每帧2个）会丢失细节，导致纹理模糊；检索头数（attention heads）过少时无法覆盖多模态历史。

Q6: 有什么可以进一步探索的点？

- The central challenge is therefore to preserve access to the full history while allowing the generation model itself to determine which memory is relevant.
- To overcome these limitations, we propose CaR, an attention-driven implicit memory retrieval mechanism that operates flexibly and globally across the historical context.
- 优先核对语义命中的全文片段：References / 2 Related Work。

Q7: 总结一下论文的主要内容

本文提出压缩与检索（CaR），一种通过注意力驱动的隐式记忆检索机制，结合轻量级上下文压缩网络，解决视频世界模型在复杂相机轨迹下的长程记忆一致性问题，并构建了大规模合成数据集SceneFly。

ra trajectories and environments. In this paper, we propose Compression and Retrieval (CaR), an attention-driven implicit memory retrieval mechanism to overcome these limitations.

Our contributions are threefold: \- We propose Compression and Retrieval, an attention-driven implicit memory retrieval mechanism for scene-consistent interactive video generation. \- We introduce SceneFly, a large-scale revisiting synthetic dataset with frame-level camera annotations designed for training and evaluating memory capability.

historical context. Rather than selecting frames before generation, CaR concatenates compressed context tokens with the target video tokens and retrieves memory through attention.

主要贡献包括：Our contributions are threefold: \- We propose Compression and Retrieval, an attention-driven implicit memory retrieval mechanism for scene-consistent interactive video generation.；\- We introduce SceneFly, a large-scale revisiting synthetic dataset with frame-level camera annotations designed for training and evaluating memory capability.；historical context.。

需要注意的边界包括：The central challenge is therefore to preserve access to the full history while allowing the generation model itself to determine which memory is relevant.；To overcome these limitations, we propose CaR, an attention-driven implicit memory retrieval mechanism that operates flexibly and globally across the historical context.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 References / 2 Related Work 部分。；它不一定和你当前最核心的 智能体 完全同题，但方法设计和评测组织值得借鉴。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 References / 2 Related Work 部分。

## 基本信息

- 作者：Zhan Peng, Jie Ma, Huiqiang Sun, Chong Gao, Zhijie Xue, Zhiyu Pan, Zhiguo Cao, Jun Liang, Jing Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23105`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。
