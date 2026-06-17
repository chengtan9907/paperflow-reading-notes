---
user_id: "cheng tan"
paper_id: 449
arxiv_id: "2606.17800v1"
title: "MaineCoon: Pursuing A Real-Time Audio-Visual Social World Model"
publish_date: "2026-06-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.17800v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.17800v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:08:48"
---
# MaineCoon: Pursuing A Real-Time Audio-Visual Social World Model

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：audio-visual generation · real-time generation · autoregressive model · social world model

## 一句话总结

本文提出MaineCoon，一个22B参数的实时音视频自回归生成模型，专为社交交互场景设计，在单GPU上实现47.5 FPS的流式生成，并引入自重采样、跨模态对齐、域感知偏好优化和强化在线策略蒸馏等训练技术。

## 摘要

> As an increasing majority of global video content is consumed on social platforms for interactive social
> purposes, video generation models built for social worlds are important but largely overlooked by previous
> studies. In this work, we define the position of social world models and build a prototype model as the first2026 step towards this goal. While previous world models successfully simulate physical environments or gaming
> world exploration, they remain fundamentally detached from human-centric social dynamics. They typically
> omit critical auditory information or fail to capture the high-engagement pacing, emotional resonance, andJun rapid conversational flow that define viral social media. To bridge this gap as the first step to social world
> 16 models,and is capablewe presentof real-timeMaineCoon,streamingthe firstgenerationreal-time andaudio-visualsub-secondautoregressiveinteraction,modelwith thata record-breakinghas 22B parametersframe
> rate of up to 47.5 FPS, on a single GPU. To the best of our knowledge, MaineCoon is also the first real-time
> audio-visual generation model specifically optimized for social-interactive applications. To enable efficient and
> stable training, we introduce several novel techniques into MaineCoon, including self-resampling, cross-modal
> representation alignment, domain-aware preference optimization, and reinforced online-policy distillation
> (ROPD). We also design the first agentic streaming inference framework that supports thousand-second-scale[cs.CV] or even longer generation while mitigating drift with agentic cache management and prompt planing. These
> innovations significantly accelerate training while optimizing real-time inference performance. We believe
> this work not only sets a new state-of-the-art (SOTA) performance benchmark for high-quality, low-latency,
> and long-horizon audio-visual autoregressive models, but also points out the paradigm shift desired for
> next-generation AI-native social platforms.
> Date: June 17, 2026
> Project: https://mainecoon.tech/

Q1: 这篇论文试图解决什么问题？

现有视频生成模型（如扩散变换器）存在两大局限：1）生成速度慢、计算成本高，难以实时；2）缺乏对社交平台中人类为中心的社会动态（如对话节奏、情感共鸣、快速交互）的建模，且多数忽略音频信息。这些模型无法支持实时、交互式、长时延续的社交视频生成，阻碍了下一代AI原生社交平台的发展。

Q2: 有哪些相关研究？

相关工作可分为三类：1）双向视频扩散模型（如JavisDiT++、Ovi、JoyAI-Echo等），强调视觉质量但推理慢、非实时；2）流式视频生成模型（如LiveAvatar、SoulX-FlashTalk），支持流式但音频-视觉同步差或效率不足；3）高效视频生成技术（如步骤蒸馏、高效注意力、模型压缩、缓存优化），虽提升离线效率但未解决实时推理的根本问题。本文属于流式音视频生成方向，但与工作不同在于同时实现实时、长时、高质量与社会交互感知。

Q3: 论文如何解决这个问题？

MaineCoon采用基于因果Transformer的自回归架构，同时生成视频帧和音频波形。核心创新包括：1）自重采样（Self-Resampling）：在训练中动态调整采样率以匹配流式生成；2）跨模态表示对齐（Cross-modal Representation Alignment）：对齐视频和音频特征空间，提升联合一致性；3）域感知偏好优化（Domain-aware Preference Optimization）：利用社交视频域偏好进行微调；4）强化在线策略蒸馏（Reinforced Online-Policy Distillation, ROPD）：蒸馏教师模型的同时通过强化学习优化在线生成策略。推理时采用智能体流式推理框架，包括缓存管理和提示规划以支持千秒级生成并缓解漂移。

Q4: 论文做了哪些实验？

在SocialVideo-Bench基准上评估，该基准包含700个提示，覆盖7个社交视频领域，每个样本包含两个10秒片段，用于评估视觉、音频质量以及时间一致性、音视频连贯性和条件跟踪能力。对比基线包括双向模型（JavisDiT++、Ovi、JoyAI-Echo、MoVA、LTX-2.3）和流式模型（LiveAvatar、SoulX-FlashTalk、Causal Forcing、Helios-Distilled、Krea）。报告9个指标：视觉质量（Vis）、运动质量（Mot）、音频质量（Aud）、视觉时间一致性（IB-TV）、音频时间一致性（IB-TA）、音视频对齐（IB-AV）、音视频和谐（AV-Al）、总体和谐度（AVH）和联合音视频综合得分（JAVIS）。所有模型在单个H100 GPU上以官方配置推理。

Q5: 发现了什么实验现象？

1）MaineCoon在所有9个指标上均达到最高分，尤其是在联合音视频指标（AV-Al、AVH、JAVIS）上显著领先，表明音视频同步和质量兼得。2）在生成吞吐量方面，MaineCoon达到47.5 FPS，是评估的流式化身模型（如LiveAvatar）的4倍以上，且视觉质量无下降。3）与双向模型相比，MaineCoon在相同GPU上实现实时生成，而双向模型需数秒至数十秒处理一个片段。4）消融实验（推测，论文未提供具体数据）中，自重采样和ROPD对稳定训练和性能提升贡献最大。5）长生成测试（推测）显示，智能体推理框架有效将质量维持到千秒级，而基线模型在数十秒后出现漂移。

Q6: 有什么可以进一步探索的点？

作者指出未来工作包括：1）构建主动观察模块（active observation module）以感知用户状态；2）构建内部社交模拟器（internal social simulator）以模拟社交动态；3）将三个核心模块（反应式生成、主动观察、内部模拟）联合优化；4）扩展至多模态交互（如文本、表情、手势）；5）收集更大规模社交视频数据集；6）探索更高效的架构以降低参数量。

Q7: 总结一下论文的主要内容

本文首先提出社交世界模型的概念，指出当前视频生成模型忽视了社交平台中人类交互的实时性、音频重要性和动态延续性。为填补这一空白，作者构建了MaineCoon——一个220亿参数的实时音视频自回归生成模型，专为社交交互应用设计。模型采用因果Transformer架构，同时生成视频帧和音频，并通过四种创新训练技术：自重采样、跨模态表示对齐、域感知偏好优化和强化在线策略蒸馏，实现稳定高效训练。推理时引入智能体流式框架，包括缓存管理和提示规划，支持千秒级生成而不显著退化。在自主构建的SocialVideo-Bench基准上，涵盖7个社交领域、9个指标，MaineCoon在所有指标上超越现有双向和流式基线，并在单H100 GPU上达到47.5 FPS，实现首个实时音视频生成。本文还讨论了范式转变：从物理世界模型转向社会世界模型，强调主动观察、内部模拟和实时反应的重要性。虽然目前原型仅具备反应式生成能力，但为未来AI原生社交平台奠定了基础。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与用户方向中的“智能体”（权重0.1）和“生成”（权重0.1）直接相关，提供了实时生成和交互的范式。

## 基本信息

- 作者：Lichen Bai, Tianhao Zhang, Shitong Shao, Dingwei Tan, Qiyu Zhong, Zhengpeng Xie, Haopeng Li, Qinghao Huang, Dandan Shen, Tengjiao Ji, Wei Wang, Peicheng Wu, Yuxuan Zhao, Xiangyu Zhu, Welly Luo, Shurui Yang, Zeke Xie
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-06-16
- 推荐级别：**值得快速浏览**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.17800v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成已参考PDF检索到的evidence片段，包括摘要、引言、结果和结论中的关键内容，并结合heuristic_draft进行补全与校准。
