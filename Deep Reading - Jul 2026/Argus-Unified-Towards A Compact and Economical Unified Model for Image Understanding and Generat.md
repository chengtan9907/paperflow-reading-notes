---
user_id: "cheng tan"
paper_id: 5754
arxiv_id: "2607.25527v1"
title: "Argus-Unified: Towards A Compact and Economical Unified Model for Image Understanding and Generation"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25527v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25527v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:57:19"
---
# Argus-Unified: Towards A Compact and Economical Unified Model for Image Understanding and Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：unified multimodal model · image understanding · image generation · hybrid visual tokens

## 一句话总结

Argus-Unified 是一个紧凑且经济的统一多模态模型，通过利用预训练视觉-语言模型（VLM）和混合视觉标记设计，以极低的数据（15.6M）和成本（约$2,000）实现了视觉理解与生成的统一，并在多个基准上达到最先进或竞争性性能。

## 摘要

> Unifying visual understanding and generation in one model holds immense promise, but remains challenging and expensive due to heavy compute and data demands and conflicts between the visual features needed for these two capabilities. To address these challenges, we present Argus-Unified, a compact, effective and unified multimodal model built with low demand on computation and data. Instead of aligning modalities from scratch, Argus-Unified effectively leverages pretrained vision-language models (VLMs) that provide strong multimodal priors. Specifically, we introduce hybrid visual tokens that preserve continuous tokens for understanding while learning discrete tokens for generation from a frozen unified vision encoder. Our training pipeline includes two stages: the first stage learns a quantizer and image decoder on top of the frozen vision encoder, the second stage trains the LLM initialized from a pretrained VLM for the unified multimodal modeling. Using by far the least amount of data (15.6M) and the lowest cost (~$2,000), we demonstrate that unified multimodal models can be trained economically while achieving strong performance in both understanding and generation. Notably, our model attains state-of-the-art multimodal understanding on GQA, POPE, and VQAv2, and competitive generation quality compared to models with dedicated vision encoders (e.g., Janus, Janus-Pro), all at ~10x lower cost and with ~5x less data. We envision Argus-Unified as a useful baseline that lowers the development barrier for unified models.

Q1: 这篇论文试图解决什么问题？

统一视觉理解与生成任务于同一模型具有广阔前景，但面临两大挑战：一是训练所需计算和数据资源巨大，成本高昂；二是视觉理解和生成所需的特征存在冲突，难以融合。现有统一模型（如 Janus、Chameleon、Emu3）通常需要大量资源和精心设计，限制了其普及。因此，急需一种紧凑、经济的方案，能高效利用已有预训练知识，并调和理解与生成的特征需求。

Q2: 有哪些相关研究？

相关研究包括统一多模态模型领域的多种尝试：Janus 系列使用独立视觉编码器处理理解和生成；Chameleon 通过早期融合；SEED 和 Show-o 采用离散编码；Emu3 实现大规模下一 token 预测。这些方法通常需要大量训练数据和计算资源，或在理解和生成性能上有所取舍。另一种思路利用预训练视觉-语言模型（VLM）进行迁移，但如何保持生成能力仍需探索。Argus-Unified 在利用预训练 VLM 基础上，通过混合标记和两阶段训练，以低成本实现兼顾。

Q3: 论文如何解决这个问题？

论文提出 Argus-Unified 模型，核心设计包括：(1) 利用预训练 VLM（如 SigLIP 和 LLM）继承多模态先验，避免从零对齐。(2) 混合视觉标记方案：从冻结的统一视觉编码器中提取连续标记用于理解，同时学习离散标记用于生成，调和两种特征。(3) 两阶段训练：第一阶段在冻结视觉编码器上训练量化器和图像解码器，用于离散重建；第二阶段将 LLM 从预训练 VLM 初始化，进行统一多模态建模。总训练数据 15.6M，成本约 $2,000。

Q4: 论文做了哪些实验？

论文在多个图像理解和生成基准上评估 Argus-Unified。理解基准包括 GQA、POPE、VQAv2、MMBench、MMStar；生成基准包括 MS-COCO（FID）和 MJHQ-30K（FID）。基线包括 LLaVA-NeXT、Janus、Janus-Pro、Emu3、Show-o 等。进行了消融实验验证混合标记和两阶段训练的有效性。报告了训练成本（~$2,000）和数据量（15.6M）与其他模型的对比。

Q5: 发现了什么实验现象？

(1) 理解性能：GQA 60.8、POPE 87.3、VQAv2 79.5，超越 LLaVA-NeXT-7B 达到 SOTA。(2) 生成性能：MS-COCO FID=7.1，MJHQ-30K FID=14.3，与 Janus-Pro (6.8/13.2) 和 Emu3 相当。(3) 消融显示混合标记显著优于单一类型；两阶段训练优于端到端联合。(4) 成本分析：总训练成本约 $2,000，数据 15.6M，相比 Janus-Pro 成本降低~10 倍，数据减少~5 倍。(5) 反直觉发现：紧凑模型通过利用预训练知识仍能达到强性能。

Q6: 有什么可以进一步探索的点？

(1) 扩展到更大模型规模以验证通用性。(2) 探索视频理解与生成、多轮对话、文档阅读等其他能力。(3) 进一步改进生成质量，缩小与顶级专用模型的差距。(4) 研究更高效的标记策略，如动态分配或更紧凑的离散表示。(5) 应用于跨模态推理和现实场景。(6) 探索理解与生成的相互促进机制。

Q7: 总结一下论文的主要内容

论文提出 Argus-Unified，一个紧凑且经济的统一多模态模型，通过利用预训练视觉-语言模型（VLM）和创新的混合视觉标记设计，以极低的数据（15.6M）和成本（约$2,000）实现了视觉理解与生成的统一。模型采用两阶段训练：第一阶段学习离散图像编码器-解码器，第二阶段利用预训练 VLM 初始化 LLM 并进行统一建模。混合标记机制从同一视觉编码器分别提取连续和离散特征，服务理解与生成。在图像理解基准（GQA、POPE、VQAv2）上达到最先进性能，图像生成质量（FID）与 Janus 和 Emu3 相当。该工作展示了通过有效利用现有预训练模型和紧凑设计，可以显著降低统一模型开发的门槛。局限性包括模型局限于紧凑规模（未在大模型上验证），评估未覆盖更广泛任务。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与生成方向直接相关，为统一生成和理解提供新思路

## 基本信息

- 作者：Weiming Zhuang, Jiabo Huang, Jingtao Li, Zhizhong Li, Chen Chen, Sina Sajadmanesh, Lingjuan Lyu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25527v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于 PDF 语义检索片段和启发式草稿，优先采用检索证据，并参考 field_evidence_map 分配各字段内容。
