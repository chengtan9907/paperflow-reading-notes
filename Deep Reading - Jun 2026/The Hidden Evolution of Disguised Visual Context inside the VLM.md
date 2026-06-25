---
user_id: "cheng tan"
paper_id: 766
arxiv_id: "2606.20077v1"
title: "The Hidden Evolution of Disguised Visual Context inside the VLM"
institution: "University of Surrey"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.20077v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.20077v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-20T01:31:24"
---
# The Hidden Evolution of Disguised Visual Context inside the VLM

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：vision-language model · visual integration · in-context injection · layer-wise injection

## 一句话总结

本文系统对比了VLM中in-context和layer-wise两种视觉集成范式，发现in-context注入下视觉token在LLM内部经历从低频到高频的隐藏演化，从而影响多模态性能。

## 摘要

> Visual tokens enter Large Language Models (LLMs) as raw, foreign signals. How they are transformed into meaningful representations and interact with the language space depends entirely on the integration architecture. Whether by treating visual tokens as in-context prompts within the input sequence or injecting them directly into the LLM's intermediate layers. A controlled comparison and understanding of how these architectural choices affect visual information and its internal transformation to integrate with the LLM remains underexplored. We provide a fair comparison by evaluating in-context and layer-wise injection VLM integration paradigms under identical training conditions across single image, multi-image, and video benchmarks. In doing so, we uncover a hidden evolution where visual tokens enter the LLM as disguised visual context, raw representations lacking linguistic structure, but are progressively reshaped depending on the integration paradigm, each capturing fundamentally different frequency characteristics of the visual signal. We show that this evolution inside the LLM determines what visual features the VLM can utilize effectively, how visual representations align with the language space, and ultimately how each paradigm performs across different tasks. We further demonstrate that attention allocation alone is insufficient, and that performance is driven by the quality of visual representations at each layer.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决什么问题？当前多模态大语言模型（MLLM）中将视觉信息注入LLM的主要方式有两种：一是将视觉token作为上下文提示加入输入序列（in-context injection），二是直接注入LLM中间层（layer-wise injection）。然而，这两种架构选择如何影响视觉信息在LLM内部的转化过程，以及它们各自对模型性能的具体贡献，尚未得到公平的系统比较和理解。论文旨在揭示不同集成范式下视觉token在LLM内部的变化规律，并解释由此导致的性能差异。

Q2: 有哪些相关研究？

有哪些相关研究？相关工作包括两个方向：1）多模态大语言模型集成架构的设计，如CLIP、LLaVA、Flamingo等采用的in-context方式，以及BLIP-2、InstructBLIP、Qwen-VL等采用的adapter或layer-wise注入方式。2）MLLM内部机制分析，包括视觉表示与语言的关系、模态差距、中间层跨模态交互的重要性，以及视觉与语言约束在模型中的联合流动。已有工作发现视觉token在LLM中经历类似文本token的渐进细化，残差连接保证特征缓慢变化，且中间层对跨模态交互至关重要。本工作在这两个方向的基础上提供了公平的对照实验和深层分析。

Q3: 论文如何解决这个问题？

论文如何解决这个问题？论文采用控制变量法，设计公平的实验设定：在相同训练条件（包括相同视觉编码器、LLM骨干、训练数据、学习率和训练步数）下，比较三种集成变体：in-context injection (IN-CT，视觉token作为前缀与文本拼接)、layer-wise injection with gated cross-attention (LW-GC，通过门控交叉注意力将视觉特征注入LLM每一层)、以及layer-wise injection with additive tokens (LW-AT，将视觉token作为额外token添加到每层输入)。在单图（如OCR、VQA）、多图（如图片对比）、视频（如动作识别、时间推理）等基准上评估性能。当发现IN-CT显著优于LW方法后，进一步通过四类分析揭示内在原因：（1）语义演化分析，追踪视觉token在各层的语义变化；（2）频率分析，对视觉表示进行傅里叶变换以观察频率成分演化；（3）注意力分配分析，检查模型对视觉token的关注程度；（4）表示质量评估，测量每层视觉表示与语言空间的对齐程度。

Q4: 论文做了哪些实验？

论文做了哪些实验？实验部分包括：1）在单图像基准（如DocVQA、TextVQA、OCRBench、OKVQA、VizWiz等）上对比三种集成变体的性能。2）在多图像基准（如NLVR2、Mantis-Eval中需要多张图的任务）上对比。3）在视频基准（如Video-MME、EgoSchema、MVBench等）上对比。4）然后针对性能差异最大的OCR和视频任务，进行四组分析实验：语义聚类分析（t-SNE可视化）、频率分析（离散傅里叶变换）、注意力图分析、以及表示质量分析（中心核对齐）。实验使用LLaMA-3-8B作为语言骨干，SigLIP作为视觉编码器，所有变体在相同数据集上训练相同步数。

Q5: 发现了什么实验现象？

发现了什么实验现象？主要发现：1）在所有任务中，in-context注入（IN-CT）一致优于layer-wise注入（LW-GC和LW-AT），在OCR和视频任务上尤其显著。2）语义演化分析显示，IN-CT下视觉token在LLM浅层中处于离散、几乎不包含语义的状态（伪装上下文），但在深层逐渐获得语义一致性，形成聚类；而LW方法下视觉token变化较小。3）频率分析揭示了关键差异：IN-CT中视觉token从浅层的主导低频信号逐渐向深层发展出更强的高频成分，这种频率偏移与任务需求相关（OCR需要精细高频细节）；LW方法则保持相对稳定的频率分布。4）注意力分配分析表明，仅依靠注意力权重无法完全解释性能差异，因为LW方法有时甚至更关注视觉token，但在表示质量上仍较差。5）表示质量评估显示，IN-CT的高层视觉表示与语言空间的匹配度更高，而LW方法可能在浅层就过早融合，限制了后续调整空间。6）此外，实验中还观察到视觉token的演化与文本token的渐进细化类似，但起始状态更为原始。

Q6: 有什么可以进一步探索的点？

有什么可以进一步探索的点？1）探索其他集成范式，如混合方法（结合in-context和layer-wise）。2）研究不同LLM骨干（如更大模型、不同架构）下这些发现是否普适。3）将分析扩展到更多模态（如音频、点云），验证频率演化假设的跨模态普遍性。4）利用频率分析结果设计更优的视觉token表示方法，例如显式控制频率成分。5）探索在训练过程中动态调整集成方式以适配不同任务。6）研究视觉token数量对隐藏演化的影响。7）将分析扩展到更复杂的多模态推理场景，如视觉常识推理。8）利用这些发现指导更高效的微调策略，例如针对特定层进行针对性训练。

Q7: 总结一下论文的主要内容

这篇论文对VLM中视觉信息集成的两种主要范式——in-context注入（视觉token作为前缀与文本拼接）和layer-wise注入（通过门控交叉注意力或附加token将每层视觉特征注入LLM）——进行了公平、系统的比较。作者在相同训练条件下，在单图、多图和视频基准上评估了这三种变体，发现in-context注入在OCR和视频任务上显著优于layer-wise注入。为了理解性能差距，他们进行了四项深入分析：语义演化、频率特性、注意力分配和表示质量。关键发现是in-context注入下的视觉token在LLM内部经历了一种“隐藏演化”：从浅层几乎无语义的高频缺失状态，逐步发展为具有丰富语义和高频成分的表示；而layer-wise注入则缺乏这种渐进式演化，导致视觉表示与语言空间对齐较差。频率分析进一步揭示in-context注入允许视觉token从低频主导向高频增强转变，这恰好匹配了OCR等任务对细节的需求。论文还指出注意力分配本身不能解释性能差异，真正驱动性能的是每层视觉表示的质量。这些结果为理解VLM内部机制提供了新视角，并为设计更有效的多模态系统提供了指导。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：对于从事多模态大模型架构设计的研究者，本文提供了选择集成方式的实证依据和机制理解

## 基本信息

- 作者：Wish Suharitdamrong, Tony Alex, Muhammad Awais, Sara Atito
- 机构：University of Surrey
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-06-18
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.20077v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，特别是Introduction、Analysis和Conclusion部分的文本。
