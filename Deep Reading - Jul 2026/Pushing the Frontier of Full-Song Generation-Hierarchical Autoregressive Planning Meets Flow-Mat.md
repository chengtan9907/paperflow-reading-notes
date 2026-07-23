---
user_id: "cheng tan"
paper_id: 5430
arxiv_id: "2607.20253"
title: "Pushing the Frontier of Full-Song Generation: Hierarchical Autoregressive Planning Meets Flow-Matching Rendering"
publish_date: "2026-07-23"
pdf_url: "https://arxiv.org/pdf/2607.20253"
abs_url: "https://arxiv.org/abs/2607.20253"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-23T17:20:37"
---
# Pushing the Frontier of Full-Song Generation: Hierarchical Autoregressive Planning Meets Flow-Matching Rendering

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：song generation · lyrics-to-song · flow matching · autoregressive model

## 一句话总结

本报告提出了一个统一的歌曲生成框架，能够从歌词、文本描述和音乐属性生成高质量全长音乐，并支持歌词转歌曲、器乐生成和翻唱生成三项任务。

## 摘要

> In this report, we present a unified song generation framework capable of producing high-quality full-length music from lyrics, text descriptions, and musical attributes. The proposed framework supports three tasks: Lyrics-to-Song Generation, which generates complete songs from text descriptions, lyrics, and musical attributes; Instrumental Music Generation, which creates music without vocals; and Cover Song Generation, which reinterprets existing songs with different styles while preserving their melodic content. Architecturally, our system consists of four main components: a semantic-aware tokenizer, hybird-LM, FullDiT, and a two-level melody module. The tokenizer encodes audio into 8-codebook RVQ tokens for efficient discrete music representation. Based on these tokens, hybird-LM performs hierarchical autoregressive audio-token modeling for full-song generation. To improve audio fidelity, FullDiT performs full-song flow matching in a continuous VAE latent space conditioned on codec tokens, lyrics, and text captions. For cover song generation, the melody module extracts and discretizes melody cues from reference audio to guide generation while preserving the original melodic content. Finally, we investigate DPO, GRPO, and OPD as reward-based post-training strategies for hybird-LM and apply flow-based GRPO to FullDiT to improve musicality and rendering quality. Experimental results on a multilingual automatic benchmark, complemented by the Artificial Analysis Music with Vocals leaderboard, show that the proposed framework achieves competitive performance in the evaluated settings.

Q1: 这篇论文试图解决什么问题？

全歌生成面临的主要挑战包括：生成工具长时长的音乐结构（如主歌、副歌）、保持歌词与旋律的对齐、在长序列中维持音频质量、以及支持多种任务（从歌词生成、纯器乐生成和翻唱生成）。现有方法通常针对短音频片段或单一声部，难以处理全长歌曲的复杂性和多样性。本文旨在解决这些问题，构建一个统一的框架来生成高质量的全长音乐。

Q2: 有哪些相关研究？

相关研究包括音频标记化（如RVQ）、自回归音乐生成模型（如MusicGen、AudioLM）、流匹配生成模型（如Matcha-TTS、Stable Audio）、以及基于奖励的优化方法（如DPO、GRPO）。在歌曲生成领域，先前工作如Jukebox、Riffusion等探索了歌词到音乐的生成，但在长时连贯性和音质方面存在局限。本文通过结合层次化自回归规划和流匹配渲染，试图推进全歌生成的前沿。

Q3: 论文如何解决这个问题？

框架由四个主要组件构成：1）语义感知分词器（AudioTokenizer），将音频信号编码为8码本残差向量量化（RVQ）标记，实现高效的离散音乐表示；2）hybird-LM，一种混合语言模型，基于这些标记进行层次化自回归音频标记建模，用于全长歌曲生成；3）FullDiT，一个全扩散Transformer，在连续VAE潜在空间中执行流匹配，条件于编解码器标记、歌词和文本描述，以提高音频保真度；4）两级旋律模块，用于翻唱生成，从参考音频中提取和离散化旋律线索，指导生成保留原旋律。此外，对hybird-LM研究了DPO、GRPO和OPD等基于奖励的后训练策略，并对FullDiT应用了基于流的GRPO，以改善音乐性和渲染质量。

Q4: 论文做了哪些实验？

论文在包含多语言的自动基准上评估框架，包括Artificial Analysis Music with Vocals排行榜。实验涉及三项任务：歌词转歌曲生成、器乐音乐生成和翻唱生成。比较了不同方法在音频质量和任务特定指标上的表现。由于摘要未提供具体数值，需要查阅原文了解详细设置和结果。

Q5: 发现了什么实验现象？

摘要未明确报告实验现象或具体结果。合理推断：hybird-LM结合FullDiT可能生成了更长的连贯音频，后训练策略（特别是GRPO）可能改善了音乐性；语义感知分词器可能更好地保持了歌词与旋律的对齐；旋律模块可能成功传递了参考旋律。具体消融趋势、失败案例和负结果需待原文确认。

Q6: 有什么可以进一步探索的点？

进一步探索的方向包括：1）扩展框架以处理更多音乐风格和语言；2）改进分词器以捕捉更细粒度的音乐结构；3）优化流匹配模型的效率以支持实时生成；4）探索更复杂的奖励建模策略，如结合人类反馈的强化学习；5）研究翻唱生成中的旋律保持与风格转换的平衡；6）将该框架应用于交互式音乐创作和音乐教育场景。

Q7: 总结一下论文的主要内容

本文提出一个统一的歌曲生成框架，旨在推进全长歌曲生成的前沿。框架支持三项核心任务：从歌词、文本描述和音乐属性生成完整歌曲（Lyrics-to-Song Generation）；无演唱的纯器乐生成（Instrumental Music Generation）；以及以不同风格重新演绎现有歌曲同时保留旋律内容的翻唱生成（Cover Song Generation）。

架构上，系统由四个主要模块组成。首先，语义感知分词器（semantic-aware tokenizer）将音频编码为8码本残差向量量化（RVQ）标记，以实现高效的离散音乐表示。其次，hybird-LM执行层次化自回归音频标记建模，利用这些离散标记生成全长歌曲结构。第三，FullDiT（全扩散Transformer）在连续VAE潜在空间中进行流匹配，条件于编解码器标记、歌词和文本描述，从而显著提升音频渲染的保真度。最后，两级旋律模块专为翻唱任务设计，从参考音频中提取并离散化旋律线索，指导生成过程以保持原旋律。

为了进一步提升音乐性和渲染质量，论文还探索了基于奖励的后训练策略。对于hybird-LM，研究了直接偏好优化（DPO）、群组相对政策优化（GRPO）和在线偏好蒸馏（OPD）。对于FullDiT，采用了基于流的GRPO。这些策略旨在通过奖励信号微调模型，使其输出更符合人类审美。

实验方面，论文在多语言自动基准上进行了评估，并参考了Artificial Analysis Music with Vocals排行榜，显示了所提框架在各项任务中具有竞争性的表现。然而，摘要中未提供具体的数值结果和与基线的详细比较，需要阅读原文获取更多细节。

总体而言，该工作提出了一个综合性的全歌生成解决方案，结合了层次化自回归规划和流匹配的优势，并通过奖励优化进一步提升质量，代表了该领域的前沿尝试。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦于生成任务（用户画像中生成方向权重0.10），直接相关。

## 基本信息

- 作者：Junyu Dai, Xinyue Fan, Weiqin Li, Xiangang Li, Yunjia Li, Bin Ma, Yukun Ma, Chongjia Ni, Yufei Shi, Haoxu Wang, Menglin Wu, Jianwei Yu, Huaicheng Zhang, Han Zhao, Shengkui Zhao, Haina Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SD, cs.AI, eess.AS
- 日期：2026-07-23
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.20253`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未参考PDF检索证据，仅基于摘要和启发式草稿。字段内容可能因信息不足而不完整，建议结合原文验证。
