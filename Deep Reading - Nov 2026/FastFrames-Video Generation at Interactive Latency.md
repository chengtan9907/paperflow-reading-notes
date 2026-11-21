---
user_id: "cheng tan"
paper_id: 22385
arxiv_id: "2611.23226v1"
title: "FastFrames: Video Generation at Interactive Latency"
publish_date: "2026-11-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.23226v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.23226v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-21T15:29:30"
---
# FastFrames: Video Generation at Interactive Latency

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：autoregressive video diffusion · interactive latency · few-step distillation · kv cache

## 一句话总结

FastFrames 通过端到端剖析自回归视频扩散的推理管线，识别并替换占过半延迟的 VAE 解码与 KV cache 写入，在 Wan 2.1 上实现 1.3B 模型单张 B200 36 ms/帧（28 RPS）和 14B 模型 8×B200 45 ms/帧（22 RPS）的交互级延迟，同时保持或提升生成质量。

## 摘要

> Autoregressive video diffusion models generate frames based on new observations and inputs, making them a promising medium for interactive visual experiences. However, existing models are orders of magnitude too slow for fluid interaction. This paper introduces FastFrames, a family of fast autoregressive video diffusion models that achieves interactive latency without compromising quality. We profile the inference pipeline of state-of-the-art models and find that operations often treated as fixed implementation details, such as VAE decoding and KV cache updates, in fact account for over half of end-to-end inference latency. We challenge the assumption that these components are indispensable and replace them with lightweight alternatives. Our 1.3B variant runs at 36 ms per frame, or 28 responses per second (RPS), on a single B200 GPU while matching the generation quality of a state-of-the-art model that is nearly 8x slower. Our 14B variant runs at 45 ms per frame, or 22 RPS, on an 8x B200 GPU node and yields significantly better visual quality and realism. We show that FastFrames enables real-time interaction in applications such as camera-conditioned and physics-conditioned video generation.

Q1: 这篇论文试图解决什么问题？

一、交互式视频的真实约束
论文要解决的是自回归视频扩散模型在交互应用中的输入到响应延迟，重点不在离线视频生成的吞吐。作者指出 FPS 会误导：把 30 帧作为一个 batch 在 1 秒内生成，系统可以报告 30 FPS，但用户仍要等 1 秒才看到第一帧响应；交互系统研究（Miller, 1968）和 HCI 的延迟检测阈值强调低延迟，因此核心指标应是每次自回归响应的时间，不应以每秒帧数为主。这个区分把问题从“模型能否快”推进到“系统能否在用户操作后及时反馈”。
二、AR 扩散的串行结构
自回归模型把视频分成 latent chunk，按 p(z1:N)=∏ p(zi|z<i) 逐块生成；每个 chunk 由 DiT 在 KV cache 条件下迭代去噪。串行依赖意味着批量吞吐优化不能直接转化为单流延迟，且每帧都要重复采样、缓存更新和像素解码。论文选择每个 AR step 只生成一个 latent frame（T=1），以降低单次响应时间；这与 Self-Forcing 等一次生成三个 latent frame 的设计形成权衡：大 chunk 更接近双向模型、可能更好优化，但单次延迟更高。
三、被误当作固定成本的两个环节
论文的关键诊断是：现有加速工作主要减少 DiT 采样步数，但把 VAE 解码和 KV cache 更新当作不可动的实现细节。对 CF++ 的端到端剖析显示，这两项合计超过一半延迟。尤其是标准 AR 管线为了把新帧写入 KV cache，需要在 t=0 对预测的 clean latent 再跑一次 DiT，然后丢弃这次前向的输出；这相当于为缓存写入支付一次完整 DiT 计算。VAE 解码则是每帧从 latent 到 RGB 的重型上采样。两者都是架构选择，并非生成质量的理论必需。
四、质量与延迟的两难
少步蒸馏可以把 DiT 前向压到两步，但剩余开销让端到端延迟仍远离交互级；直接减小模型或降低分辨率又会损失视觉质量。论文试图在不牺牲质量的前提下，把非 DiT 环节换成轻量学习组件，并用同 backbone 的基线做质量对照。这里隐含一个判断：DiT 特征已经足够丰富，像素重建和缓存写入可以由更便宜的网络近似，而不必每次都走完整模块。
五、评价与部署边界
论文用 RPS（每秒响应数），而非 FPS，作为主指标，并明确 AR 模型的一次 response 是一个自回归生成单元，双向模型的一次 response 是整个 clip；因此 RPS 不会为“一个 response 内生成多帧”额外记分。这个口径更贴近交互，但也带来跨模型比较的张力。实验边界是 480p、文本到视频、B200 高端 GPU、满 KV cache 的稳态单流延迟；网络、输入处理、多用户调度和端侧部署不在其中。
六、核心研究问题
论文试图回答三个问题：在自回归视频扩散的串行推理中，哪些环节真正支配端到端延迟；能否用轻量替代物去掉这些环节而不损质量；以及如何用面向交互的指标，而非 FPS，来评价实时视频生成。

Q2: 有哪些相关研究？

一、视频扩散与可控生成
论文建立在视频扩散模型的主线之上：Ho et al. 将扩散用于视频，Singer et al.、Blattmann et al. 推进了文本到视频与开放域视频合成，近期 Seedance、Black Forest Labs、MiniMax 等系统进一步提升了真实感与场景多样性。可控生成方面，Wang et al. 与 He et al. 等工作让模型响应相机、动作等条件，这为交互式应用提供了控制接口。FastFrames 不改变这条生成主线，而是把“条件输入后多快能看到画面”作为系统目标。
二、自回归视频扩散
Jin et al.、Deng et al.、Chen et al.、Teng et al. 等将视频生成建模为逐块自回归过程，使模型能持续接收新观测与新控制。Self-Forcing 以三个 latent frame 为一个 chunk，偏向更接近双向模型的生成质量；Causal Forcing++（CF++）以单个 latent frame 为 chunk，偏向更低延迟，是 FastFrames 的直接起点。Krea-Realtime、LingBot 等则代表同一 Wan 2.1 生态中的实时或低延迟基线。论文的差异在于：它不只改 DiT 采样器，而是把 AR 管线中的缓存写入和像素解码也作为一等优化对象。
三、少步蒸馏与加速采样
Yin et al. 的早期工作、Yin et al. 2025 的分布匹配蒸馏、Huang et al. 2025 与 Zhu et al. 2026 的少步 AR 方案，已经把 DiT 的采样步数压到很少步。FastFrames 延续这条路线，使用两步采样，但指出仅优化 DiT 步数会留下大量非 DiT 开销；因此它与少步蒸馏是互补关系，而不是替代关系。
四、像素扩散与解码器
近期像素扩散模型 JiT（Li & He, 2026）表明，不依赖传统 VAE 解码器也能生成锐利图像。受此启发，FastFrames 质疑：当 DiT 已经输出富含空间上下文的特征时，是否仍需要重型 VAE 解码器。它选择训练轻量 pixel decoder，把解码成本从固定大模块转移到可定制的小网络。这一思路与“latent 生成 + 重型解码”的主流范式形成竞争。
五、交互系统、世界模型与机器人
交互系统研究（Miller, 1968）早已强调响应延迟；世界模拟、游戏生成、机器人预测模型等工作（Chou et al., Seo et al., Yu et al., Robbyant Team, Ye et al., NVIDIA）都需要模型在闭环中及时响应。FastFrames 的相机条件与物理条件演示正是面向这类场景。不过，这些应用通常还要求长时一致性、物理合理性和控制精度，论文只覆盖了延迟这一维度。
六、系统级推理优化
KV cache 管理、分布式注意力和推理调度是视频扩散系统的常见优化点；Ulysses 等分布式方案被用于多卡推理。现有工作多把这些当作工程细节，FastFrames 则通过剖析证明它们可能主导端到端时间，并用学习组件替换其中的冗余计算。这使它同时属于生成模型、系统优化和交互式媒体三条研究线的交叉。
七、研究缺口
综合来看，先前工作要么报告 FPS/吞吐，要么只减少 DiT 步数，缺少对“输入到响应”延迟的端到端拆解；也较少质疑 VAE 解码和 KV cache 写入是否必须按标准方式执行。FastFrames 的贡献在于把这两个被忽视的环节显式建模，并用同 backbone 的质量对照验证替代方案的可行性。

Q3: 论文如何解决这个问题？

一、总体建模
FastFrames 把视频表示为 latent 序列 z1:N=(z1,…,zN)，每个 chunk zi∈R^{C×T×H×W}，按 p(z1:N)=∏ p(zi|z<i) 自回归分解；每个 latent frame 对应 K 个 RGB 帧。与 Self-Forcing 的 T=3 不同，FastFrames 取 T=1，每个 AR step 只生成一个 latent frame，以缩短单次响应。每个条件分布由 DiT 在 KV cache 上迭代去噪，采用两步扩散采样。
二、剖析驱动的瓶颈定位
论文先对当时最快的少步 AR 模型 CF++ 做端到端剖析，分别计量扩散采样、KV cache 更新、VAE 解码等环节。结果显示，VAE 解码与 KV cache 更新合计超过一半延迟。这个发现把优化目标从“再减一步采样”转向“去掉非生成必需的重型实现”。
三、用 pixel decoder 替代 VAE 解码器
标准管线在 DiT 输出 latent 后用 VAE decoder 解码为 RGB。FastFrames 认为 DiT 特征已经足够丰富，因而训练一个轻量 pixel decoder，从 latent/DiT 特征直接重建像素。训练时冻结 DiT，只更新 decoder，以像素重建为目标；论文提到需要对梯度尺度做相对重建梯度的归一化以稳定优化，并尝试过低学习率微调 DiT，但观察到训练不稳定。图 3 的定性结果显示 pixel decoder 能重建细薄结构；具体网络结构和多尺度连接在提供的片段中未展开。
四、用 KV refinement network 替代 KV 写入前向
标准 AR 管线为了把新生成的 chunk 写入 KV cache，会以预测的 clean latent 在 t=0 再跑一次 DiT，取其 keys/values 写入缓存，然后丢弃这次前向的输出。FastFrames 训练一个 KV refinement network，直接预测新帧应写入的 K/V，从而省掉这次完整 DiT 前向。从片段可合理推断，其监督目标是标准 DiT 前向产生的 K/V；原文命中片段未展开损失细节。在两步扩散设置中，这一替换能显著降低每帧的固定开销，但本质上是学习近似，可能引入缓存误差。
五、训练与模型族
FastFrames-S（1.3B）从两步 CF++ checkpoint 初始化，冻结 DiT，只训练 pixel decoder 与 KV refinement network。FastFrames-L（14B）无法直接从现成 AR checkpoint 初始化，而是从 Wan 2.1 14B 双向基座开始，按 CF++ 的配方转为自回归模型，再做少步蒸馏与 DMD 训练。两个变体都执行两步扩散、每个 AR step 生成一个 latent frame。
六、延迟测量与系统实现
论文在 NVIDIA B200 上以 832×480 分辨率测量稳态延迟，使用满 KV cache，并在官方支持时启用 Ulysses 等分布式优化。计时覆盖扩散采样、KV cache 更新和 pixel decoding，而不是只计 DiT 前向。RPS 定义为端到端延迟的倒数；对 AR 模型，一次 response 是一个自回归生成单元，因此不会因为一个单元内含多帧而获得额外分数。
七、设计取舍
FastFrames 的核心取舍是：用 T=1 降低单次延迟，用学习组件替代精确但昂贵的固定模块，用冻结 DiT 保持 S 变体的质量，用完整重训换取 L 变体的质量上限。论文的目标是证明在给定质量目标下，这些环节可以被更轻的近似替代，并未试图证明 VAE 解码或 KV 写入在理论上可省。

Q4: 论文做了哪些实验？

一、模型与基线
FastFrames 基于 Wan 2.1，包含 FastFrames-S（1.3B）与 FastFrames-L（14B）。S 从两步 CF++ checkpoint 初始化并冻结 DiT；L 从 Wan 2.1 14B 双向基座转为 AR、蒸馏并做 DMD。对照包括同 backbone 的 CF++（对应 S）与 Krea-Realtime（对应 L），以及双向视频生成模型作为质量与延迟的宽参考；片段还提到 LingBot 等实时基线。
二、指标与测量协议
论文报告模型大小、端到端延迟、RPS 与 VBench 分数。所有模型使用相同 VBench prompts 与 seeds。延迟在 NVIDIA B200、832×480、满 KV cache 的稳态下测量，包含扩散采样、KV cache 更新和 pixel decoding；官方支持时启用 Ulysses 等分布式优化。RPS 是延迟的倒数；AR 模型一次 response 是一个自回归生成单元，双向模型一次 response 是整个 clip，因此 RPS 不奖励一个 response 内生成多帧。
三、主结果
FastFrames-S 在单张 B200 上达到 36 ms/帧、28 RPS，质量与 CF++ 匹配；论文片段指出 CF++ 约慢 7.6 倍，摘要概括为“近 8 倍慢的 SOTA”。FastFrames-L 在 8×B200 节点上达到 45 ms/帧、22 RPS，质量与 Krea-Realtime 匹配，并报告更好的视觉质量与真实感。两个变体都保持两步采样、每个 AR step 一个 latent frame。
四、应用展示
论文展示 camera-conditioned 与 physics-conditioned 视频生成的实时交互。前者让用户/系统改变相机轨迹后快速看到新视角；后者在物理条件变化下实时生成响应。这些演示用于说明低延迟使闭环交互成为可能，而不是只做离线指标比较。
五、分析与消融
剖析实验是论文的核心分析：VAE 解码与 KV cache 更新合计超过一半延迟，说明只优化 DiT 步数不够。替换实验显示 pixel decoder 能重建细薄结构，KV refinement 能替代 t=0 的 DiT 写入前向；训练上，低学习率微调 DiT 出现不稳定，而冻结 DiT、训练轻量组件更可行。片段未给出逐组件的完整消融表。
六、证据缺口
提供的片段没有给出 VBench 的具体分数、每个基线的精确延迟、pixel decoder 与 KV refinement 的单独消融数值，也没有用户研究或长时一致性曲线。因此本报告只确认“质量匹配/更好”的定性结论与已给出的延迟数字，不补造具体分数。实验边界是 480p 文本到视频、B200 高端 GPU、稳态单流；更高分辨率、端侧硬件、多用户和网络延迟未评估。

Q5: 发现了什么实验现象？

一、非 DiT 环节主导延迟：这是论文最反直觉的观察。通常加速工作盯着扩散采样步数，但端到端剖析显示 VAE 解码与 KV cache 更新合计超过一半时间。也就是说，即使把 DiT 压到两步，系统仍可能被“生成之后”的解码和“写缓存”吃掉大半延迟。
二、FPS 与交互延迟脱钩：论文用 30 帧 batch 在 1 秒内生成的例子说明，高 FPS 可以是高吞吐，而不是低延迟。对交互而言，用户关心的是操作后第一响应的时间；批量生成多帧不会缩短这个等待。RPS 指标因此强调一次自回归响应的完成时间。
三、去掉 VAE 解码器不必然损质量：pixel decoder 在冻结 DiT 的条件下训练，片段提到图 3 能忠实重建细薄结构。这说明重型 VAE 解码器并非质量的唯一来源，DiT 特征本身可能已经包含足够的空间信息。不过，这只给出定性证据，缺少高分辨率、细纹理和长视频的定量对比。
四、KV 写入可以用学习网络近似：标准管线在 t=0 对 clean latent 再跑一次 DiT 只为写 K/V，然后丢弃输出；FastFrames 用 KV refinement network 直接预测缓存。这个替换的收益来自省掉一次完整 DiT 前向，但代价是缓存不再精确。合理推断，误差会在长时自回归中累积，可能导致画面漂移或细节退化；提供的片段没有给出长时误差曲线。
五、模型规模与硬件之间存在张力：1.3B 变体在单张 B200 上 36 ms，14B 变体在 8×B200 节点上 45 ms，响应延迟接近，但后者使用的 GPU 数量约为前者的 8 倍。交互级延迟并非“免费”，14B 的质量提升以显著更高的硬件成本换取。论文没有给出单位算力或能耗口径的公平比较。
六、训练稳定性揭示了一个工程约束：低学习率微调 DiT 会出现不稳定，而冻结 DiT、只训练 pixel decoder 与 KV refinement 更可行。这说明在保持生成质量的同时做管线替换，训练策略本身是难点；S 变体依赖 CF++ checkpoint，L 变体则必须重走双向到 AR 再到少步蒸馏的流程。
七、RPS 指标带来跨模型比较的张力：AR 模型的一次 response 是一个生成单元，双向模型的一次 response 是整个 clip；RPS 刻意不奖励一个 response 内生成多帧。这个口径对交互公平，但会让“单次生成整段视频”的双向模型在数字上显得吃亏，也让不同 AR chunk 大小的模型难以直接比较。
八、未验证的失败模式：片段没有覆盖高分辨率、快速相机运动、复杂物理交互、长时一致性、多用户并发和端侧部署。pixel decoder 是否会在高频纹理上失败、KV 近似是否会在数百帧后漂移、控制条件改变时缓存是否仍然有效，都是需要原论文实验或后续工作确认的问题。

Q6: 有什么可以进一步探索的点？

一、控制条件扩展
论文 conclusion 明确把“加入动作条件而不增加延迟”列为未来方向。相机条件与物理条件演示只是起点；真正交互式世界模型还需要文本、动作、物理参数、用户编辑等多模态控制。可探索的方向包括：把控制信号编码进 KV cache 或条件 token，设计控制感知的缓存更新，避免每次控制变化都重跑完整 DiT。
二、分辨率与硬件
当前评估是 480p、B200 高端 GPU。更高分辨率会同时放大 DiT、pixel decoder 和 KV cache 的成本；端侧或轻量硬件则要求量化、剪枝、蒸馏和内存带宽优化。可探索：pixel decoder 的多尺度结构、分块解码、低精度 KV cache、在移动 GPU 上的延迟测量，以及分辨率与延迟的 scaling law。
三、长时一致性与物理合理性
论文承认长时一致性和物理可信度仍是开放问题。KV refinement 的近似误差可能随帧数累积；pixel decoder 可能在高频细节上漂移；物理条件视频需要满足刚体、碰撞、光照等约束。可探索：长时误差校正、周期性全量 DiT 重算、物理约束损失、记忆机制和场景状态缓存。
四、KV 近似的误差控制
KV refinement network 目前是学习近似。可探索：自适应刷新策略（误差大时回退到精确 DiT 写入）、缓存压缩与淘汰、误差可观测性、不确定性估计、分段一致性约束。也可以研究 KV cache 是否可以被更低秩或事件驱动的方式表示，从而在保持长时一致性的同时进一步降延迟。
五、解码器与生成器联合优化
FastFrames-S 冻结 DiT、只训练 pixel decoder；这是一种解耦策略。可探索：解码器与 DiT 的联合微调、感知损失与对抗损失、扩散解码器与 pixel decoder 的混合、在训练中直接优化端到端延迟。需要小心的是，联合训练可能破坏少步蒸馏得到的生成质量，论文已经观察到低学习率微调 DiT 的不稳定。
六、训练目标与推理成本感知
当前训练目标主要是重建与蒸馏，推理延迟是事后测量。可探索：把延迟、缓存写入成本、解码成本纳入训练目标，做成本感知的蒸馏；研究不同 chunk 大小、采样步数、缓存策略的联合搜索；把“质量-延迟”帕累托前沿显式建模，而不是先训练再测。
七、评价体系
论文用 RPS 替代 FPS 是重要一步，但仍缺少交互质量的系统评价。可探索：用户研究中的感知延迟、抖动、控制响应时间、长时一致性、物理合理性、分布外输入下的失败率；区分首帧延迟、稳态延迟和尾延迟；报告多用户并发下的 p95/p99 延迟。VBench 分数不足以覆盖这些维度。
八、系统与多用户部署
当前测量是单流、满 KV cache、稳态。真实服务需要批处理、调度、显存管理、请求抢占和多用户隔离。可探索：连续批处理下的延迟-吞吐权衡、KV cache 的分页与共享、 speculative decoding、分布式推理的通信开销、在 8×B200 节点上进一步压缩跨卡同步。
九、跨领域迁移
FastFrames 的思路可迁移到音频生成、机器人视频预测、科学模拟和游戏引擎。特别是 agent 与 world model 场景，输入到响应的低延迟直接决定闭环控制质量；但這些场景还要求动作条件、长时记忆和可解释的物理状态。可探索把 pixel decoder 与 KV refinement 的设计推广到其他自回归扩散模态。

Q7: 总结一下论文的主要内容

一、论文主线
FastFrames 是一篇系统导向的自回归视频扩散工作，目标是把输入到画面的响应延迟压到交互级，重点不在提高离线视频生成的吞吐。作者认为，现有模型在蒸馏和少步采样上已经很快，但用 FPS 衡量会掩盖真实交互延迟：一个模型可以把 30 帧打包在 1 秒内生成，报告 30 FPS，用户仍要等待 1 秒才看到响应。因此论文以每次自回归响应的时间为核心指标，并用 RPS 表示每秒可完成的响应数。
二、问题诊断
论文选择当时最快的少步 AR 模型 CF++ 做端到端剖析，发现扩散采样之外的 VAE 解码与 KV cache 更新合计超过一半延迟。尤其标准 AR 管线为了写入新帧的 K/V，需要在 t=0 对预测的 clean latent 再跑一次 DiT，然后丢弃这次前向的输出；这相当于为缓存写入支付一次完整 DiT 计算。VAE 解码则是每帧从 latent 到 RGB 的重型上采样。作者挑战“这些环节不可省”的假设，把它们视为可替换的实现细节。
三、方法
FastFrames 基于 Wan 2.1，采用单 latent frame per AR step 的分解 p(z1:N)=∏ p(zi|z<i)，每个 chunk 由 DiT 在 KV cache 上做两步去噪。它用轻量 pixel decoder 替代 VAE decoder，冻结 DiT，仅以像素重建为目标训练解码器；用 KV refinement network 直接预测新帧的 K/V，替代 t=0 的额外 DiT 前向。FastFrames-S（1.3B）从两步 CF++ checkpoint 初始化，只训练上述轻量组件；FastFrames-L（14B）从 Wan 2.1 14B 双向基座转为 AR，再按 CF++ 配方做少步蒸馏与 DMD。
四、实验
论文在 B200、832×480、满 KV cache 的稳态下测量端到端延迟，计时包含扩散采样、KV cache 更新与 pixel decoding，并在官方支持时启用 Ulysses 等分布式优化。对照包括同 backbone 的 CF++ 与 Krea-Realtime，以及双向视频生成模型；所有模型使用相同 VBench prompts 与 seeds。RPS 的定义刻意不奖励一个 response 内生成多帧：AR 模型的一次 response 是一个自回归生成单元，双向模型的一次 response 是整个 clip。
五、关键结果
FastFrames-S 在单张 B200 上达到 36 ms/帧、28 RPS，质量与 CF++ 匹配；论文片段指出 CF++ 约慢 7.6 倍，摘要概括为“近 8 倍慢的 SOTA”。FastFrames-L 在 8×B200 节点上达到 45 ms/帧、22 RPS，质量与 Krea-Realtime 匹配，并报告更好的视觉质量与真实感。论文还展示 camera-conditioned 与 physics-conditioned 视频生成的实时交互，说明低延迟使闭环控制成为可能。
六、关键发现
最反直觉的发现是：端到端延迟的大头不在 DiT 采样，而在被当作固定实现细节的 VAE 解码与 KV 写入；因此只做少步蒸馏不够，必须做管线级替换。第二个发现是 VAE 解码器并非质量必需，轻量 pixel decoder 可以重建细薄结构；第三个发现是 KV 写入可以用学习网络近似，但这也引入缓存误差与长时漂移的风险。训练上，低学习率微调 DiT 不稳定，冻结 DiT、训练轻量组件更可行。
七、边界与意义
实验边界是 480p 文本到视频、B200 高端 GPU、稳态单流；更高分辨率、端侧硬件、多用户、网络延迟、动作条件与长时一致性仍未覆盖。14B 变体需要 8×B200 节点，说明交互级延迟仍以高硬件成本为代价。总体而言，FastFrames 的价值在于把交互式视频生成从“报告 FPS”推进到“优化输入到响应延迟”，并证明 VAE 解码与 KV cache 写入可以作为一等系统变量来重新设计。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向直接相关：论文讨论自回归视频扩散的实时化，核心是延迟而非离线质量，适合关注交互式视频生成、世界模型和实时渲染的研究者。

## 基本信息

- 作者：Gene Chou, Guandao Yang, Fujun Luan, Lars Mescheder, Wei Dong, Stephan Richter, Vladlen Koltun
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-11-17
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.23226v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Method、Results 与 Conclusion 片段；对片段未覆盖的 VBench 具体分数、完整消融、用户研究和作者机构信息，报告中已明确标注为证据缺口。
