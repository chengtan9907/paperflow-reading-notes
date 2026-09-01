---
user_id: "cheng tan"
paper_id: 10122
arxiv_id: "2608.30181v1"
title: "A.X K2 Technical Report"
institution: "韩国主权 AI 相关机构 (根据作者名及“national Sovereign AI effort”合理推断)"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30181v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30181v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:50:42"
---
# A.X K2 Technical Report

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：mixture-of-experts · agentic applications · sparse gated attention · long context

## 一句话总结

A.X K2 是一个拥有 688B 参数的混合专家（MoE）语言模型，通过 8.5T 高质量数据训练，引入了稀疏门控注意力（SGA）和门控归一化（GN）技术，旨在为智能体应用提供高性能的长文本处理与推理能力。

## 摘要

> We introduce A.X K2, a 688B-parameter Mixture-of-Experts (MoE) language model trained from scratch as a high-performance foundation for \emph{agentic} applications. Trained on approximately 8.5T tokens---fewer than its predecessor, A.X K1---on a smaller but higher-quality mixture with substantially expanded agentic and software-engineering data, it nonetheless improves over A.X K1 across the board, by over 30 percentage points on some benchmarks, reflecting large gains in token efficiency. To support long contexts efficiently, we introduce Sparse Gated Attention (SGA), which combines sparse attention with gated attention, and adopt Gated Norm (GN) to stabilize large-scale training. SGA is trained natively at 128K through a \emph{sparse} indexer warmup that optimizes the indexer against its own sparse top-$k$ selection rather than the dense attention distribution, making adaptation markedly cheaper: each query reads only 2,048 positions, yet long-context quality is unchanged and A.X K2 scores 94.6 on RULER out to 256K. The outlier suppression of GN in turn keeps 4-bit NVFP4 serving within one point of FP8 accuracy. A simple yet effective Think-Fusion recipe further lets users switch between thinking and non-thinking modes within a single unified model. Extensive evaluations show that A.X K2 performs competitively against strong open-weight baselines, matching or exceeding them on math and Korean-language benchmarks.

Q1: 这篇论文试图解决什么问题？

### 1. 核心挑战：智能体能力的规模化与效率平衡
当前的 LLM 前沿正从通用的文本生成转向“智能体能力”（Agentic Competence），即多步推理、工具调用和长程规划。然而，在构建此类模型时面临以下核心矛盾：
- **长文本推理的计算开销**：传统的密集注意力机制在处理超长上下文（如 128K 以上）时，KV 缓存和计算量呈平方级增长，难以在实际智能体场景中落地。
- **大规模 MoE 训练的稳定性**：688B 规模的 MoE 模型在训练过程中极易出现激活值离群（Outliers），导致训练崩溃或量化精度大幅下降。
- **推理成本与质量的权衡**：如何在保持高性能推理（如数学、代码）的同时，降低部署成本（如使用 4-bit 量化）而不损失精度。
- **数据效率问题**：如何在 token 数量有限（8.5T）的情况下，通过优化数据配比（Data Mixture）实现比更大规模预训练更好的效果。

### 2. 针对性问题定义
- **长文本效率**：如何在不牺牲质量的前提下，让模型在 256K 长度下每条查询仅读取极少量的位置（如 2,048 个）？
- **量化友好性**：如何通过架构设计主动抑制离群值，从而支持 NVFP4 等极低比特推理格式？
- **模式切换**：如何在一个模型中融合“深度思考”与“快速响应”两种截然不同的推理范式？

Q2: 有哪些相关研究？

### 1. 混合专家模型 (MoE) 架构
论文参考了 DeepSeek-V3/V2、GLM-4.5 等近期高性能 MoE 系统的设计经验。A.X K2 采用了多头潜在注意力（MLA）作为骨干网络，以优化推理时的 KV 缓存占用。

### 2. 长文本与稀疏注意力
- **Gated Attention**：参考了 Qiu et al. (2025b) 的工作，用于增强模型对上下文信息的门控筛选能力。
- **稀疏机制**：与传统的全量注意力不同，SGA 借鉴了稀疏索引的思想，但通过“稀疏索引器预热”（Sparse Indexer Warmup）解决了索引器与注意力分布不一致的冷启动问题。

### 3. 训练稳定性与量化
- **归一化技术**：针对大规模模型中的离群值问题，论文对比了传统的 LayerNorm/RMSNorm，并引入了 Gated Norm (GN) 作为改进方案。
- **量化推理**：关注 NVFP4 等新兴硬件加速格式，旨在实现 Sovereign AI（主权 AI）的高效部署。

Q3: 论文如何解决这个问题？

### 1. 架构创新：Sparse Gated Attention (SGA)
- **机制组合**：将门控注意力（Gated Attention）与稀疏选择机制结合。在 128K 原生训练中，通过 SGA 让每个 Query 仅与 Top-K（如 2,048）个 Key-Value 对交互。
- **稀疏索引器预热**：这是 SGA 的关键。模型不是针对密集的注意力分布进行训练，而是直接针对其自身的稀疏 Top-K 选择进行优化，显著降低了长文本适配的计算成本。

### 2. 稳定性增强：Gated Norm (GN)
- **离群值抑制**：GN 通过门控机制动态调整归一化尺度，有效抑制了 Transformer 深层中常见的激活值爆炸现象。
- **量化支持**：由于 GN 减少了激活值的动态范围，使得模型在 4-bit NVFP4 格式下的精度损失控制在 FP8 的 1 个百分点以内，极大提升了部署效率。

### 3. 数据与训练策略
- **高质量数据混合**：虽然 token 总量减少至 8.5T，但大幅增加了智能体任务、软件工程和复杂推理数据的比例。
- **Think-Fusion**：开发了一种融合配方，通过特定的训练目标，使模型能够根据指令或任务需求，在“思考模式”（长链推理）和“标准模式”（直接输出）之间无缝切换。

### 4. 规模化原则
- 遵循 MoE Scaling Laws，在固定的计算预算下，通过计算预算规划（Compute-budget-based planning）在模型规模与训练时长之间选择了最优平衡点（688B 参数）。

Q4: 论文做了哪些实验？

### 1. 实验设置
- **模型规模**：688B 总参数，MoE 架构。
- **预训练数据**：8.5T tokens，包含大量代码、数学和智能体交互数据。
- **上下文长度**：原生支持 128K，通过 SGA 扩展至 256K。

### 2. 评测基准
- **通用能力**：MMLU, GSM8K, HumanEval 等。
- **长文本能力**：RULER 基准（测试范围覆盖 4K 至 256K）。
- **智能体能力**：专门的 Agentic Benchmarks（涉及多步工具调用）。
- **特定语言**：韩语能力测试（作为主权 AI 的核心指标）。

### 3. 消融实验与对比
- **K1 vs K2**：对比前作 A.X K1，验证数据质量提升带来的 token 效率。
- **SGA 效率测试**：测量在不同上下文长度下的推理延迟与显存占用。
- **量化对比**：对比 FP8、NVFP4 在不同任务上的准确率保持情况。

Q5: 发现了什么实验现象？

### 1. 性能飞跃
- **Token 效率**：K2 在使用更少数据的情况下，性能全面超越 K1。在某些特定基准上，得分提升了 30 个百分点以上，证明了数据质量和架构优化的巨大潜力。
- **长文本表现**：在 RULER 基准测试中，A.X K2 在 256K 长度下获得了 94.6 的高分。尽管每个 Query 只读取 2,048 个位置，但其长文本质量与全量注意力几乎无异。

### 2. 稳定性与量化现象
- **离群值控制**：引入 Gated Norm 后，训练过程表现出极高的稳定性。在 NVFP4 4-bit 部署环境下，模型精度与 FP8 相比仅有微小波动（<1%），这在超大规模模型中非常罕见。

### 3. 任务表现
- **数学与韩语**：模型在数学推理和韩语理解方面表现出极强的竞争力，匹配甚至超过了许多知名的开源权重基准模型。
- **模式切换**：Think-Fusion 成功实现了单一模型内的双模式运行，用户可以根据实时响应需求选择是否开启“思考”过程。

Q6: 有什么可以进一步探索的点？

### 1. 智能体能力的进一步深化
- 探索更复杂的自主规划与错误自我修正机制。
- 增强模型在多模态智能体场景下的表现（如视觉-语言导航）。

### 2. 推理架构优化
- 进一步优化 SGA 的硬件实现，以实现更低的推理延迟。
- 探索更低比特（如 2-bit 或 3-bit）量化在 MoE 模型上的可行性。

### 3. 主权 AI 的扩展
- 针对特定国家/地区的文化和法律环境进行更深度的对齐训练。
- 提升模型在低资源语言上的迁移学习能力。

Q7: 总结一下论文的主要内容

本技术报告介绍了 A.X K2，这是一个旨在成为智能体应用高性能基础的 688B 参数 MoE 模型。该研究的核心贡献在于证明了通过“高质量数据+架构创新”可以显著提升大模型的 token 效率和部署可行性。K2 在 8.5T tokens 上训练，虽然规模巨大，但通过引入稀疏门控注意力（SGA），成功解决了长文本推理的计算瓶颈，实现了 256K 上下文的高效处理。同时，门控归一化（GN）的引入不仅稳定了训练，还为 4-bit 极低比特推理扫清了障碍。实验结果表明，K2 在数学、代码及韩语任务上表现卓越，且具备独特的“思维融合”能力，允许在推理时灵活切换模式。作为主权 AI 努力的一部分，A.X K2 展示了在特定领域和语言环境下构建顶尖性能模型的路径，为未来智能体驱动的应用提供了坚实的底座。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文与智能体（Agent）方向高度相关，提供了从底层架构到数据配比的完整方案。

## 基本信息

- 作者：Cheolseung Baek, Dhammiko Arya, Eunki Kim, Gun Song, Gyoungeun Han, Hyunho Yang, Hyunjun Eun, Jin Kim, Junyoung Park, Juyun Wee, Minki Hong, Minkyung Park, Minsang Kim, Minsoo Kang, SaeRom Kim, Sangjin Kim, Sangyeol Lee, Seojin Lee, Seokhwan Jo, Seokyoung Hong, Seongho Choi, Seonghye Cho, Seongmin Ok, Sereimony Sek, Seungmo Cho, Seungsik Kim, Singon Kim, Sohee Park, Sooyeon Park, Subin Yi, Sungbin Yoon, Sungeun Lee, Sung Jun Cheon, Sungwan Kim, Sunwoo Lee, Tae Yoon Kim, Wonbeom Jang, Yohan Ra, Yong-jin Han, Youngjin Kim, Youngrang Kim, Yujin Kang, Yujin Lee
- 机构：韩国主权 AI 相关机构 (根据作者名及“national Sovereign AI effort”合理推断)
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.30181v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是关于 SGA、GN 技术细节以及 RULER 实验结果的部分。
