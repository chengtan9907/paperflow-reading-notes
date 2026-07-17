---
user_id: "cheng tan"
paper_id: 4296
arxiv_id: "2607.14749v1"
title: "WanSong v1.0 Technical Report"
institution: "Alibaba Group (Wan Team)"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.14749v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.14749v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-18T00:54:34"
---
# WanSong v1.0 Technical Report

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：song generation · diffusion model · long-form audio · dual-stem generation

## 一句话总结

WanSong 是一个纯扩散模型，能够直接生成长达5分钟的高保真多语言歌曲，并同时输出人声和背景音乐双轨，通过步骤蒸馏实现快速推理和下游编辑微调。

## 摘要

> Music generation foundation models have recently attracted significant industry attention. However, achieving efficient generation and high-fidelity long-form audio while supporting controllability remains challenging. To address these needs, we present \textbf{WanSong}, a simple yet powerful approach for long-form, commercial-grade song generation. Unlike autoregressive (AR) and cascaded multi-stage pipelines (\eg, AR followed by diffusion), \textbf{WanSong} is a pure diffusion-based model that directly generates high-fidelity, multilingual songs up to 5 minutes and outputs dual stems (vocals and background music) in a single run. In addition, our diffusion framework enables faster inference through step-distillation, and offers an efficient pathway for fine-tuning and customization to support downstream editing tasks.

Q1: 这篇论文试图解决什么问题？

现有音乐生成模型（如自回归或多阶段流水线）在生成高效、高保真的长音频方面存在局限，且难以同时支持可控性。商业系统如 Suno 和 Mureka 虽然质量高，但多基于多阶段方法，导致推理慢、复杂度高。WanSong 试图解决这些问题，提出一个单阶段、纯扩散的框架，直接生成长歌曲和双轨，并通过步骤蒸馏加速。

Q2: 有哪些相关研究？

相关研究包括商业系统 Suno (2026) 和 Mureka (2026)，它们展现了高保真音乐生成能力，但可能采用自回归或多阶段架构。MusicLM (Agostinelli et al. 2023) 使用多级语义和声学令牌生成。DiffRhythm 2 (Yao et al. 2025) 采用块匹配流匹配。WanSong 区别于这些工作，采用纯扩散、单阶段流水线，直接处理连续音频表示，并同时生成人声和背景音乐。

Q3: 论文如何解决这个问题？

WanSong 采用纯扩散框架，将音频编码为连续潜在序列。首先，一个高压缩率 VAE（可调整 patch 大小）将音频映射到潜在空间。然后，基于扩散的骨干网络在潜在空间生成样本。通过步骤蒸馏显著减少采样步数。模型直接输出两个通道：人声和背景音乐。此外，通过微调和下游编辑（如 melody 改编）支持定制化。训练数据经精心筛选和标注，结合音频类别和质量平衡。后训练阶段可能采用 DPO 和 ReFL 强化学习微调。

Q4: 论文做了哪些实验？

论文进行了 VAE 重建 fidelity 评估，在两个基准（音乐和语音）上比较不同压缩比（2048 vs 1024）的影响，使用 PER 和感知质量评分。完整模型在内部 WanSong 基准和标准测试集上与 Stable Audio 2 比较，指标包括 STFT 距离和 SI-SDR。数据筛选方面，构建了高质量、平衡的子数据集。强化学习微调部分使用了 reward 模型和 DPO/ReFL 方法。

Q5: 发现了什么实验现象？

实验观察到：VAE 压缩比显著影响感知质量，实际压缩比 1024（patch size=1）优于 2048（PER 15% vs 19.2%，Quality 更高）。在完整模型对比中，WanSong 在相同压缩比下在音乐和语音基准上全面优于 Stable Audio 2，STFT 距离降低 19-22%，SI-SDR 提高 0.28-1.72 dB。步骤蒸馏在保持质量的同时实现了更快推理。强化学习微调可能进一步改善主观质量。

Q6: 有什么可以进一步探索的点？

证据中未明确提及。可能的方向包括：进一步优化蒸馏策略以降低计算成本；扩展到更多音乐风格和语言；增强可控性维度（如歌词、风格、乐器）；探索更高效的自监督训练；将模型应用于下游任务如音乐编辑和混音。强化学习微调部分值得更深入的研究。

Q7: 总结一下论文的主要内容

本技术报告介绍了 WanSong v1.0，一个纯扩散的歌曲生成基础模型。它直接生成最长5分钟的多语言高保真歌曲，并同时输出人声和背景音乐双轨。采用单阶段流水线，避免多阶段级联的复杂性。通过步骤蒸馏实现快速推理，并支持微调和下游编辑。实验系统分析了 VAE 压缩比对重建质量的影响，并在客观指标上验证了模型优于类似压缩比的 Stable Audio 2。论文还讨论了数据筛选和强化学习微调（DPO/ReFL）。WanSong 旨在为商业级歌曲生成提供简单而强大的基线。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向（权重0.10）直接相关，适合关注生成模型的读者。

## 基本信息

- 作者：Binghui Chen, Pandeng Li, Yu Liu, Jingren Zhou
- 机构：Alibaba Group (Wan Team)
- 来源：arxiv
- 主题/分类：eess.AS, cs.CV
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.14749v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据并优先采用了field_evidence_map指示的证据片段。
