---
user_id: "cheng tan"
paper_id: 3166
arxiv_id: "2607.08766"
title: "OPSD-V: On-Policy Self-Distillation for Post-Training Few-Step Autoregressive Video Generators"
publish_date: "2026-07-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.08766.pdf"
pdf_url: "https://arxiv.org/pdf/2607.08766"
abs_url: "https://arxiv.org/abs/2607.08766"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-11T00:12:41"
---
# OPSD-V: On-Policy Self-Distillation for Post-Training Few-Step Autoregressive Video Generators

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：few-step video generation · autoregressive model · on-policy self-distillation · cache-aware distillation

## 一句话总结

提出OPSD-V，一种用于后训练少步自回归视频生成器的在策略自蒸馏方法，通过引入真实长视频上下文提供密集轨迹级监督，缓解长时生成中的误差累积和运动退化问题。

## 摘要

> We propose OPSD-V, an on-policy self-distillation paradigm for post-training few-step autoregressive (AR) video diffusion models. Existing few-step AR video generators can produce long videos with low latency, but still suffer from error accumulation and weakened motion dynamics during long autoregressive rollout. OPSD-V reduces long-horizon degradation while preserving the original few-step inference path. The key idea is to introduce real long-video data as temporal context during training and use it to provide dense trajectory-level supervision. Specifically, the student follows the exact inference-time rollout, generating each chunk conditioned on its own previously generated KV cache. In parallel, the teacher is evaluated at the same student-visited denoising states, but uses a cleaner AR-consistent temporal cache in which older history can be replaced by real-video context. This provides dense denoising-level corrective targets under on-policy AR cache dynamics, without changing the sampler, number of denoising steps, or inference-time cache mechanism. We apply OPSD-V to representative few-step AR video models, including Self-Forcing and LongLive. Experiments show consistent improvements in visual quality, motion dynamics, and VBenchLong scores. A user study with 10 participants comparing 20 video pairs shows that OPSD-V is preferred over the base models in 66.0% of overall-preference judgments (82.5% excluding ties).

Q1: 这篇论文试图解决什么问题？

现有少步自回归视频生成器（如通过DMD风格蒸馏获得的模型）能够以低延迟生成长视频，但在长自回归展开中仍存在严重的误差累积和运动动态减弱问题。具体表现为：随着生成块增多，早期误差被放大，导致视觉质量下降、动作僵化甚至内容重复。此外，现有训练范式通常使用短视频片段或教师强迫策略，与推理时的自回归缓存状态不匹配，导致训练-测试不一致。OPSD-V旨在解决这些长期退化问题，同时不改变推理时的采样器、步数和缓存机制。

Q2: 有哪些相关研究？

相关工作包括：1）少步自回归视频生成：Self-Forcing采用KV缓存和引导采样实现实时生成长视频；LongLive开发了块级自回归生成和流式长调优。2）蒸馏方法：DMD-style蒸馏将多步扩散模型压缩为少步模型，但忽略了自回归缓存动态。3）在策略自蒸馏（OPSD）：最近被用于轨迹级蒸馏（如Fang等2026），OPSD-V借鉴并扩展了上下文增强自蒸馏。4）缓存感知训练：部分工作利用KV缓存优化但未解决误差积累。OPSD-V的独特之处在于结合了真实长视频上下文和自回归缓存状态匹配的密集监督。

Q3: 论文如何解决这个问题？

OPSD-V框架包含学生和教师分支，两者共享相同模型权重。具体流程：1）将长训练视频划分为潜在块x1:N，使用第一个块x1作为前缀初始化KV缓存并锚定场景，但不参与损失计算。2）学生以自回归方式生成后续块，每个块的条件是之前生成的KV缓存（即推理路径）。3）教师在同一去噪状态下评估，但使用更清洁的缓存：对于块i，教师的历史缓存中，早期块（如x1...xi-1）被替换为对应的真实视频块的实际潜在表示。这使得教师能够提供密集的逐去噪步骤正确目标，而学生则从教师那里蒸馏。4）损失函数基于轨迹级蒸馏（类似OPD），计算学生与教师输出之间的差异。训练后，推理时无需任何修改，保持原有的少步采样和缓存机制。关键优势：在策略性（学生自己的缓存状态）和自回归一致性（教师使用真实上下文）之间取得平衡。

Q4: 论文做了哪些实验？

实验设置：使用自定义长视频数据集，包含3,800个约一分钟长度的视频，480p分辨率，覆盖多种场景。基模型选择Self-Forcing和LongLive，分别代表不同的少步自回归架构。评估指标包括VBenchLong（长视频质量基准）、CLIP相似度、用户偏好等。训练采用OPSD-V后训练。用户研究：10名参与者对20对视频进行偏好判断。结果：OPSD-V在视觉质量、运动动态和VBenchLong分数上取得一致性改进。用户偏好率为66.0%（整体），排除平局后为82.5%。此外，还进行了消融实验验证各个组件的作用（证据不足具体数值）。

Q5: 发现了什么实验现象？

从论文片段和摘要推断的主要实验现象：1）视觉质量提升显著，尤其在长时间生成中保持细节和连续性。2）运动动态更自然，减少了重复或停滞模式。3）VBenchLong得分提升，表明长视频生成能力增强。4）用户研究表明OPSD-V生成结果明显更受欢迎。5）消融实验（合理推断）显示：教师使用真实上下文至关重要；在策略训练（学生自己的缓存状态）优于离线策略。6）可能存在的负结果或边界：当训练数据不足或与目标领域差距大时，改进可能减弱。7）计算开销：训练时额外教师评估带来一定成本，但推理不变。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向：1）扩展到更长视频（超过一分钟），验证OPSD-V在极长时间场景的泛化。2）结合其他蒸馏方法（如对抗蒸馏）进一步提升少步生成质量。3）探索更大的训练数据集和更丰富的领域，减少对自定义数据集的依赖。4）将OPSD-V应用于其他自回归视频模型（如VideoLDM、CogVideo等）。5）分析教师缓存替换策略的影响，如部分替换或自适应替换。6）结合在线增强，如动态选择替换块。7）在保持推理效率的同时，进一步压缩模型步数。8）理论分析误差积累和自回归一致性的关系。

Q7: 总结一下论文的主要内容

本文提出OPSD-V，一种后训练自蒸馏方法，专门针对少步自回归视频生成器的长时退化问题。现有少步AR模型虽然在延迟上优势，但误差积累和运动弱化限制了实际应用。OPSD-V通过引入真实长视频作为时间上下文，并设计缓存感知的师生框架，让学生在其推理路径上获得密集的轨迹级监督。具体地，学生按常规自回归方式生成，教师则使用更清洁的缓存（替换旧历史为真实帧）提供正确信号。训练仅在自定义的3,800个一分钟长视频上进行，不改变推理配置。实验证明该方法能够显著提升视觉质量、运动动态和长视频基准分数，用户偏好率高达82.5%（排除平局）。论文还分析了相关工作和方法的局限性，指出未来可以扩展数据规模和模型范围。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文提供了少步自回归视频生成中误差积累问题的解决方案，与需要长时生成的应用场景高度相关。

## 基本信息

- 作者：Hongyu Liu, Chun Wang, Feng Gao, Xuanhua He, Yue Ma, Ziyu Wan, Yong Zhang, Xiaoming Wei, Qifeng Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.08766`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于论文摘要、启发式草稿以及PDF语义检索证据（52个片段），优先采用了检索命中的证据锚点进行填充。
