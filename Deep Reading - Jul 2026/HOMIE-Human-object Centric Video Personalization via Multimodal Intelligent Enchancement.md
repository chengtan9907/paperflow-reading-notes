---
user_id: "cheng tan"
paper_id: 4714
arxiv_id: "2607.18217"
title: "HOMIE: Human-object Centric Video Personalization via Multimodal Intelligent Enchancement"
publish_date: "2026-07-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.18217.pdf"
pdf_url: "https://arxiv.org/pdf/2607.18217"
abs_url: "https://arxiv.org/abs/2607.18217"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T19:21:05"
---
# HOMIE: Human-object Centric Video Personalization via Multimodal Intelligent Enchancement

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：human-object centric video personalization · subject-driven video generation · multimodal large language model · diffusion model

## 一句话总结

提出HOMIE框架，统一处理inter-和intra-subject输入设置，通过全局多模态引导和模态参考嵌入提升人-物中心视频个性化性能。

## 摘要

> Human-object centric video personalization (HOCVP) is a core task within subject-driven video generation. However, existing methods suffer from two key limitations. First, most approaches focusing on inter-subject personalization still struggle to strike a balance between high subject fidelity and accurate interaction patterns between humans and diverse objects, especially when objects represent abstract concepts such as logos. Second, while intra-subject references (e.g., OCR maps, multi-view inputs) are expected to enhance subject fidelity, most existing works lack mechanisms to understand such latent correspondence. To address both challenges, we propose HOMIE, an HOCVP framework that tackles both inter- and intra-subject input settings in a unified manner. Compared to previous approaches, HOMIE proposes a better MLLM integration strategy to extract knowledge of reference-level relationships without compromising the controllability of text encoders or incurring costly re-alignment. Specifically, we introduce global multimodal guidance within self-attention to better align MLLM-derived semantic features with VAE tokens. Furthermore, we propose modality-reference embedding to differentiate tokens from MLLM features and VAE tokens and associate intra-subject reference image tokens. Extensive experiments validate that our method achieves state-of-the-art performance across various HOCVP tasks. Project Page: https://yiyangcai.github.io/homie-page.github.io/

Q1: 这篇论文试图解决什么问题？

现有主体驱动视频生成方法在人-物交互场景中面临两个关键挑战：1) 跨主体个性化难以在保持主体 fidelity 和准确建模交互模式之间取得平衡，尤其当物体为抽象概念（如标志）时；2) 缺乏机制利用帧内参考（如OCR图、多视角输入）增强主体一致性。当前方法要么完全替换文本编码器为MLLM导致重对齐成本高，要么无法有效处理多种输入类型。

Q2: 有哪些相关研究？

相关工作包括视频扩散模型（基于T2I扩散扩展至视频）、主体驱动生成（如DreamBooth、Custom Diffusion）以及MLLM集成策略（如替换文本编码器、通过隐藏状态连接）。HOMIE提出一种更好的MLLM集成方式，在不牺牲文本编码器可控性和避免重新对齐的前提下提取参考关系知识。

Q3: 论文如何解决这个问题？

HOMIE采用统一多模态输入处理，包含两个核心组件：1) 全局多模态引导（GMG），在自注意力层中将MLLM导出的语义特征注入视频token，增强语义推理和交互建模；2) 模态参考嵌入（Modality-Reference Embedding），区分MLLM特征和VAE token，并关联帧内参考图像token，提升主体 fidelity。该方法兼顾inter-和intra-subject场景。

Q4: 论文做了哪些实验？

实验包括定量比较（在HOCVP任务上对比多种baseline）、定性可视化、用户研究（40名参与者评估整体质量、文本跟随、主体一致性），以及可能的消融研究。项目页面提供视频结果。

Q5: 发现了什么实验现象？

HOMIE在多个HOCVP任务上达到SOTA；在定性比较中能忠实合成同一物体的不同视角并保持主体 fidelity；用户研究显示HOMIE在总体质量和一致性上优于Phantom、Kling、SkyReels-V3、UniVideo等基线。

Q6: 有什么可以进一步探索的点？

可探索方向包括：处理更复杂的人-物交互（如遮挡、动态变形）、扩展到任意数量对象、结合更精细的MLLM推理（如空间关系理解）、降低计算成本、在更多现实场景中验证泛化性。

Q7: 总结一下论文的主要内容

论文针对人-物中心视频个性化（HOCVP）任务，指给定人物和物体参考图像生成指定交互视频。现有方法在跨主体（inter-subject）场景中难以兼顾主体保真度和交互准确性，且缺乏处理帧内参考（intra-subject）机制。作者提出HOMIE，通过改进MLLM集成策略统一解决两类输入设置。具体技术包括：1) 全局多模态引导（GMG），在DiT自注意力中注入MLLM语义特征以增强交互推理；2) 模态参考嵌入，区分不同来源token并关联帧内参考图像。实验表明HOMIE在多项指标上达到SOTA，并在用户研究中获得更高偏好。论文贡献为统一框架、GMG模块和模态嵌入设计。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦于生成方向，与用户当前关注点（generation权重0.1）直接相关。

## 基本信息

- 作者：Yiyang Cai, Nan Chen, Rongchang Xie, Junwen Pan, Chunyang Jiang, Cheng Chen, Wen Zhou, Zhenbang Sun, Wei Xue, Wenhan Luo, Yike Guo
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.18217`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括摘要、引言、用户研究等片段。
