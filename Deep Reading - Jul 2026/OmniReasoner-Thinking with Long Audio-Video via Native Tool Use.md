---
user_id: "cheng tan"
paper_id: 4972
arxiv_id: "2607.19339"
title: "OmniReasoner: Thinking with Long Audio-Video via Native Tool Use"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.19339.pdf"
pdf_url: "https://arxiv.org/pdf/2607.19339"
abs_url: "https://arxiv.org/abs/2607.19339"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T13:55:24"
---
# OmniReasoner: Thinking with Long Audio-Video via Native Tool Use

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long audio-video reasoning · tool-use · post-training · reinforcement learning

## 一句话总结

OmniReasoner是一个用于长音频视频推理的工具使用后训练框架，通过监督微调和强化学习让全模态LLM学会自适应调用缩放工具，在需要时获取高保真局部音频视频信息，从而提升推理准确性和时间定位能力。

## 摘要

> Long audio-video reasoning is difficult for omnimodal LLMs because the decisive evidence is often sparse, cross-modal, and too expensive to preserve with uniformly high-fidelity inputs. We introduce OmniReasoner, a tool-use post-training framework for Thinking with Long Audio-Video: omni-modal LLMs learn, via supervised fine-tuning and reinforcement learning, to decide whether and where to call a zoom-in tool before answering. OmniReasoner first builds a low-cost global preview of the full stream and then, when needed, calls the zoom-in tool with a requested temporal interval for higher-fidelity visual and audio inspection before answering. Because the model observes different sampling granularities before and after this call -- a sparse global preview and a denser local clip -- we introduce TimeAnchor, which keeps the tool's temporal argument valid and round-trip-consistent across these granularities, rather than tied to frame indices from a particular sampling rate. To make this tool-use behavior trainable without expensive manual interval annotation, we build a Temporal Augmented Data Engine that synthesizes tool-use post-training trajectories by video editing and composition. Experiments across omnimodal and video benchmarks show that OmniReasoner improves both answer accuracy and temporal grounding while concentrating high-fidelity computation on informative regions. Code is available at https://github.com/RockyChen0205/OmniReasoner.

Q1: 这篇论文试图解决什么问题？

全模态LLM在长音频视频推理中面临两个主要困难：1）关键证据稀疏且跨模态分布，模型难以有效感知；2）对所有输入保持统一高保真采样导致计算冗余。现有方法或均匀编码、或依赖启发式采样，但无法自适应关注需要更精细检查的时空区域。OmniReasoner针对这些问题，提出让模型学会主动使用工具，在推理时按需获取高保真信息，实现计算资源的自适应分配。

Q2: 有哪些相关研究？

相关研究包括三类：1）稠密帧编码方法，对所有帧进行均匀高保真处理，计算量大；2）稀疏帧采样方法，如随机或均匀采样，可能错过关键信息；3）交互式或工具使用方法，如VideoAgent等，允许模型搜索相关片段。OmniReasoner属于第三类，但创新点在于后训练框架和自动合成训练数据，同时引入时间锚点解决跨粒度一致性问题。（合理推断）

Q3: 论文如何解决这个问题？

OmniReasoner的推理流程包含两步：首先，模型对长音频视频进行稀疏全局预览（如低帧率采样），然后模型判断是否需要调用缩放工具，并指定一个时间区间（例如“inspect 32–38 seconds”）。工具调用后，模型获得该区间的高保真视觉和音频片段，再根据这些信息生成答案。为了在不同的采样粒度下保持时间参数有效，引入了时间锚点（TimeAnchor），工具参数基于时间锚点而非帧索引，从而保证跨粒度一致性。训练阶段，使用监督微调（SFT）让模型学会产生正确的工具调用和推理链，再通过强化学习（RL）优化决策策略。为了自动生成训练数据，构建了时间增强数据引擎（Temporal Augmented Data Engine），通过视频编辑和组合合成带工具调用标注的训练轨迹，避免了昂贵的人工标注。

Q4: 论文做了哪些实验？

论文在全模态和视频理解基准上进行了实验，并比较了基础模型（Qwen-Omni-2.8B）和其他交互式方法。报告的主要结果包括：在Daily-Omni基准上，准确率从62.1提升至64.2；在WorldSense基准上，准确率从45.4提升至46.7。此外还进行了消融研究，分离SFT和RL的贡献，并验证时间锚点和数据引擎的效果。（基于片段推断，具体消融细节未充分体现）

Q5: 发现了什么实验现象？

实验观察到：1）在需要跨模态检索的基准上（如Daily-Omni）提升显著，在简单任务上提升温和，符合预期；2）工具使用后训练不会损害模型在其他任务上的性能；3）OmniReasoner能够主动调用缩放工具获取更多帧，将高保真计算集中在信息区域；4）时间锚点机制有效保证了工具调用的一致性；5）数据引擎合成的训练数据质量足以支持有效的工具使用行为学习。（推测部分来自消融，但整体趋势由证据支持）

Q6: 有什么可以进一步探索的点？

未来方向包括：1）扩展到多步工具使用，实现更复杂的信息查找；2）在更大更强的基座模型（如Qwen-Omni-3）上验证；3）改进多轮强化学习基础设施，支持更多交互；4）探索工具感知的预训练，使基础模型更适应工具使用范式；5）将时间锚点扩展到其他模态（如深度图、传感器数据）。

Q7: 总结一下论文的主要内容

本文提出OmniReasoner，一个针对长音频视频推理的工具使用后训练框架。核心动机是：长音频视频中的决定性证据稀疏、跨模态且保留统一高保真成本过高。OmniReasoner模型首先通过稀疏采样构建全局预览，然后学习决定是否需要以及在哪一区间调用缩放工具，以获取高保真局部信息后再回答。这一过程通过监督微调（SFT）和强化学习（RL）训练实现。为支持训练，设计了时间锚点（TimeAnchor）机制，保证工具调用时间参数在不同采样粒度下有效；并构建了时间增强数据引擎（TADE），通过视频编辑合成带工具调用的训练轨迹。实验在多项全模态和视频基准上进行，结果显示OmniReasoner在准确性和时间定位上均有所提升（如Daily-Omni从62.1到64.2，WorldSense从45.4到46.7），同时没有牺牲其他任务性能，并将计算集中在信息区域。主要贡献包括：提出工具使用后训练框架、时间锚点机制、合成数据引擎，以及实验验证。局限性在于基础设施仅支持两步工具使用、未在更大模型上实验、未探索工具感知预训练等。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文的智能体式工具使用范式和后训练RL方法对多模态智能体设计有直接参考价值

## 基本信息

- 作者：Yu Chen, Caorui Li, Ziyu Xiong, Yidong Wang, Mingqi Gao, Shuman Liu, Biao Liu, Chunfeng Yang, Anxiang Zeng, Haibo Zhang, Chaofan Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.19339`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，主要从Abstract、Introduction、Conclusion和Results部分提取信息。
