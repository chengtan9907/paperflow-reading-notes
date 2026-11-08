---
user_id: "cheng tan"
paper_id: 20224
arxiv_id: "2611.03247v1"
title: "R$^2$-Omni: Reducing Structural and Precision Redundancy in Omni-LLMs for Long-Form Generation"
publish_date: "2026-11-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.03247v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.03247v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-08T02:04:54"
---
# R$^2$-Omni: Reducing Structural and Precision Redundancy in Omni-LLMs for Long-Form Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：omni-modal large language models · kv cache compression · token consolidation · long-form generation

## 一句话总结

R²-Omni 是一个免训练框架，联合压缩 Omni-LLM 音视频 KV 缓存中的结构冗余与精度冗余，在约 5× 内存压缩下保持约 97.98% 的长文本生成质量，并取得 1.48× 端到端加速。

## 摘要

> Omni-modal large language models (Omni-LLMs) typically process large numbers of multimodal tokens from audio-visual inputs, incurring substantial computational costs and a large memory footprint for the key-value (KV) cache. Aggressive token pruning can alleviate this problem while maintaining competitive performance on multiple-choice tasks, which require only a single option as output. However, we observe substantial quality degradation when generating long-form, open-ended responses. We attribute this gap to query-guided selection, which can retain sufficient evidence for answer selection while discarding details needed for comprehensive generation. This motivates reducing overlapping information across tokens, termed structural redundancy, while preserving diverse content. However, the retained representations may still contain precision redundancy due to unnecessarily high numerical precision in their cached values. To address these two levels of redundancy in token representations, we introduce a training-free framework named R$^2$-Omni. First, Structure-Aware Token Consolidation (STC) consolidates redundant representations using spatiotemporal similarity in video and local continuity in audio. Then, Semantic-Sensitivity Guided Precision Allocation (SSPA) allocates value precision to the retained representations according to the estimated reduction in attention-output error under quantized keys, with query attention and audio-visual alignment providing a semantic prior. Extensive experiments on long-form generation benchmarks demonstrate an average 5.0$\times$ reduction in the memory footprint while preserving 97.98% of baseline generation quality and achieving a 1.48$\times$ end-to-end speedup, highlighting the benefits of jointly reducing structural and precision redundancy.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：论文要解决的是 Omni-LLM 在长音视频输入下 KV cache 过大、解码成本过高的问题，但它的切入点是把压缩目标从“尽量少保留 token”推进到“在长文本开放式生成中尽量不丢失信息”。已有 token pruning 在多项选择任务上表现尚可，因为任务只需要选出正确选项，保留少量高相关性证据就可能足够；长文本生成则要求覆盖更多事件、属性、时间细节和跨模态线索，单靠 query attention 选出的少量 token 很容易造成细节缺失。

2. 现有方法的错位：论文把这一差距归因于 query-guided token selection。该方法用当前问题对 token 的注意力判断重要性，并丢弃低分 token。对于“单选”任务，这相当于保留决策所需的关键证据；对于“生成”任务，初始 query 未必能预判哪些细节会在后续段落中被需要，因此高压缩率下生成质量下降更快。论文的 Fig. 1(a) 观察到 dense-captioning 质量随剪枝率上升而下降的速度明显快于多项选择性能，这是其问题成立的关键经验依据。

3. 两层冗余的拆解：论文不是把问题简化为单一的 token 数压缩，而是区分结构冗余与精度冗余。结构冗余指不同音视频 token 之间存在大量重叠信息，可以通过合并而不是硬删除来减少 token 数；精度冗余指即使完成了 token 合并，保留表示的缓存数值仍可能使用不必要的高精度，因而可以在数值位宽上继续压缩。合理推断，这一拆解对应 KV cache 成本的两个乘数：保留 token 数与每个表示的平均位宽。

4. 训练-free 约束：论文明确采用免训练框架，意味着它不重新训练 Omni-LLM，也不依赖大规模标注来学习压缩策略，而是利用输入本身的结构相似性、注意力输出误差估计和语义先验做在线或推理时决策。这有利于部署和与现有模型组合，但也带来风险：相似度、误差代理和语义先验都是启发式信号，未必在所有模型、层、头和任务上都可靠。

5. 隐含假设与评价问题：STC 假设视频时空相似与音频局部连续性能够标识可合并的冗余表示；SSPA 假设在键量化条件下估计出的注意力输出误差削减量能够预测生成质量。这些假设在场景切换、重叠语音、快速运动、细粒度 OCR、罕见事件或强事实性要求下可能失效。论文主要在三个长文本音视频基准上验证，平均质量保持率很高，但不同子指标可能出现方向不一致，说明“平均质量”会掩盖局部能力变化；这部分需要结合消融和附录进一步核对。

Q2: 有哪些相关研究？

1. Omni-LLM 与长音视频理解：论文的相关背景是同时处理文本、视频和音频的 Omni-LLM，例如 Qwen2.5-Omni 系列与 MiniCPM-o-4.5。这类模型支持音视频推理、实时多模态助手和复杂视频理解，但长输入会带来大量多模态 token，增加 prefill 计算与自回归解码的 KV cache 压力。论文的实验模型正是这三个代表性 backbone，说明其定位是模型无关的推理侧压缩框架。

2. 多模态 token pruning：论文将 OmniZip、OmniSelect、SEATS 作为 token pruning 基线。这一路线通过删除冗余 token 降低 token 数，常在多项选择基准上以较高压缩率维持性能。论文的批评点是：这类方法多以 query-guided 重要性为依据，适合“选答案”，不天然适合“写长文”；当任务需要更广的信息覆盖时，低分 token 可能承载后续生成所需的时间、属性或事件细节。R²-Omni 的 STC 因而改为依据音视频内在结构做 consolidation，与 query-guided top-k 保留形成对照。

3. KV cache 量化：论文将 KIVI、OTT、OScaR 作为 KV-cache quantization 基线，代表通过低比特缓存降低内存占用的路线。与 token pruning 不同，量化不直接删除 token，因此在长文本生成上可能更稳；但固定或粗粒度的位宽分配仍可能把高精度浪费在不敏感表示上，或对敏感表示压缩过度。SSPA 的差异在于按注意力输出误差估计分配值精度，并把 query attention 与音视频对齐作为语义先验。

4. 长文本音视频生成基准：论文使用 video-SALMONN-2 testset、LongVALE 和 OmniDCBench，覆盖 detailed video captioning、dense time-aware captioning、fine-grained temporal event understanding 等场景。这些基准的共同点是要求模型输出长文本并覆盖时间与事件细节，正好对应 token pruning 容易失败的开放式生成条件。

5. 与相邻路线的区别：更广义的相邻研究还包括 KV cache eviction、token merging、attention sparsity、低比特量化和多模态 token 压缩。论文的相对位置是：不训练新模型，不单独优化 token 数或位宽，而是把“结构冗余”和“精度冗余”放进同一个压缩框架，并用 3 个模型 × 3 个长文本基准以及 naive composed baselines 做系统比较。其贡献更像是一个推理侧系统框架和问题重述，而不是单一新网络结构。

Q3: 论文如何解决这个问题？

1. 总体框架：R²-Omni 是 training-free 的推理侧压缩框架，输入是 Omni-LLM 对音视频输入编码后形成的多模态 KV 表示，输出是压缩后的缓存表示。它按顺序做两件事：先用 STC 减少保留表示的数量，再用 SSPA 减少每个保留表示的数值精度。这样内存收益来自 token 数与位宽两个方向的乘积，而不是只压一个轴。

2. Structure-Aware Token Consolidation（STC）：STC 的核心不是按当前 query 的注意力分数做 top-k 保留，而是利用输入本身的结构冗余。视频侧使用 spatiotemporal similarity，即帧内与跨帧的时空相似性来识别重叠表示；音频侧使用 local continuity，即时间局部连续性来识别可合并片段。从“consolidation”的命名和摘要看，合理推断它更接近带权合并或聚类式合并，而不是简单硬删除；被合并的表示仍以某种聚合形式保留多样内容。其设计目标是让长文本生成所需的广泛证据不被初始 query 提前裁掉。

3. Semantic-Sensitivity Guided Precision Allocation（SSPA）：SSPA 在键已被量化的条件下，估计每个保留表示采用不同值精度时对注意力输出误差的削减量，并把更高位宽分配给“减少误差收益更大”的表示。这里的语义先验来自 query attention 与 audio-visual alignment：前者指示当前查询相关的表示，后者指示跨模态对齐重要的表示。于是精度分配不是均匀量化，也不是只按数值幅度，而是按对注意力输出和语义重要性的敏感度分配。摘要没有展开逐层、逐头还是逐 token 分配，具体粒度和搜索过程需要回原文核对。

4. 联合优化逻辑：STC 先降低 token 数，SSPA 再在剩余表示上做非均匀位宽分配。两者共同作用于 KV cache 的“数量 × 精度”成本。与“token pruning + 统一量化”的 naive 组合相比，R²-Omni 的区别在于：结构合并保留多样内容，精度分配避免对敏感表示过度压缩。论文的 Table 2 也把 naive composed token-pruning 与 KV-cache quantization 基线放在一起比较，以突出联合建模的收益。

5. 设计假设与风险：STC 假设时空/局部连续性能够刻画语义冗余，但快速剪辑、重叠说话人、细粒度文字或罕见事件可能打破这一假设；SSPA 假设注意力输出误差的估计能预测生成质量，但量化误差在多层、多头上会累积，语义先验也可能偏向当前 query 而忽略后续长文本需求。论文的消融表明 STC 与 SSPA 都有贡献，但具体误差界和失效模式仍需进一步分析。

Q4: 论文做了哪些实验？

1. 模型与硬件：实验覆盖 Qwen2.5-Omni-3B、Qwen2.5-Omni-7B 和 MiniCPM-o-4.5 三个 Omni-LLM，均在 NVIDIA H800 80GB GPU 上运行，使用 greedy decoding。论文强调同一模型内所有方法共享相同输入、prompt 和生成设置，以控制比较条件。

2. 基准：使用 video-SALMONN-2 testset、LongVALE 和 OmniDCBench 三个长文本音视频生成基准，覆盖 detailed video captioning、dense time-aware captioning 和 fine-grained temporal event understanding。视频采样方面，Qwen 在 video-SALMONN-2 上均匀采样 128 帧，MiniCPM 使用 native A/V-aligned sampling 且最多 100 帧；LongVALE 使用最多 100 帧的均匀采样。

3. 对比方法：token pruning 基线包括 OmniZip、OmniSelect、SEATS；KV-cache quantization 基线包括 KIVI、OTT、OScaR。论文还在 Table 2 中比较 naive composed token-pruning 与 KV-cache quantization 组合，以检验“分别压缩 token 数和位宽”是否足够。所有方法尽量对齐到可比的压缩预算。

4. 指标：表中报告质量相关指标、Correct、EER、压缩比、SODA-C、SODA-M 以及相对基线质量保持率。不同基准的指标侧重不同，既有事件/时间覆盖类指标，也有长文本匹配类指标；因此论文同时报告多个维度，而不是只看单一分数。

5. 消融与效率：消融覆盖 STC、SSPA 以及语义信号的作用；从可见片段看，去掉任一模块都会使质量-压缩折中变差。效率方面，论文报告平均 5.0× 内存压缩、97.98% 的基线质量保持和 1.48× 端到端加速，并说明加速来自长上下文效率评估，而非单纯理论内存下降。

6. 比较公平性：论文声称在相同 backbone、输入、prompt 和解码配置下比较，并对齐压缩预算；但 Table 2 中不同方法的实际压缩比仍有轻微差异，例如约 4.8× 到 5.4×，因此读表时需要注意“同等预算”是近似对齐，而非逐位完全相同。

Q5: 发现了什么实验现象？

1. 任务维度上最重要的现象是：token pruning 在多项选择任务上可以较激进，但在长文本生成上掉点更快。论文用 Fig. 1(a) 展示 dense-captioning 质量随剪枝率上升而下降的斜率明显大于多项选择性能，这支持了“query-guided 选择适合选答案、不适合写长文”的解释。

2. Table 2 显示 token pruning 基线在约 5× 压缩下对长文本质量保持较弱。Qwen2.5-Omni-3B 上，OmniSelect、OmniZip、SEATS 的质量保持率分别约为 81.53%、89.89%、90.28%，而 R²-Omni 约为 99.99%；Qwen2.5-Omni-7B 上分别为 75.03%、80.43%、91.26%，R²-Omni 约为 100.11%；MiniCPM-o-4.5 上分别为 44.98%、46.88%、45.31%，R²-Omni 约为 93.86%。MiniCPM 的 token pruning 崩塌尤其明显，说明 aggressive pruning 在开放式生成上可能造成灾难性细节丢失。

3. KV-cache quantization 基线整体比 token pruning 更稳，但仍未达到 R²-Omni 的水平。3B 上 KIVI、OScaR、OTT 分别约为 87.10%、88.81%、94.12%；7B 上约为 96.45%、98.45%、99.19%；MiniCPM 上约为 91.27%、93.03%、91.46%。这组结果说明“少保留 token”与“低比特缓存”不是等价操作：量化保留了完整 token 集，因而在长文本上通常更安全；但在 3B 和 MiniCPM 上仍有明显质量缺口。

4. 模型规模与架构会影响压缩鲁棒性。Qwen2.5-Omni-7B 在量化基线上已经接近基线质量，R²-Omni 的优势空间较小；Qwen2.5-Omni-3B 和 MiniCPM-o-4.5 的 token pruning 退化更严重，R²-Omni 的相对收益更明显。合理推断，较小模型或不同视觉/音频编码器结构对 token 冗余的利用方式不同，压缩策略需要模型自适应。

5. 指标之间存在张力：以 Table 2 的 SODA-C 与 SODA-M 为例，3B 上 R²-Omni 的 SODA-C 为 2.23、高于基线 2.06，但 SODA-M 为 4.78、略低于基线 5.01；7B 上 SODA-C 从 1.63 升至 1.71，SODA-M 从 7.32 降至 7.09；MiniCPM 上 SODA-C 从 2.72 降至 2.33，但 SODA-M 从 17.14 升至 17.60。这说明平均质量保持率接近 100% 并不代表所有子能力都无损，不同长文本指标对压缩的敏感方向可能不同。

6. 消融结果支持 STC 与 SSPA 的互补性。可见片段中 w/o SSPA 与 w/o STC 都会改变质量、压缩比和 SODA 指标的折中；合理推断，去掉 STC 会削弱结构冗余消除能力，去掉 SSPA 会削弱非均匀精度分配带来的误差控制。论文还检查了语义信号在精度分配中的作用，说明 query attention 与音视频对齐不是装饰性先验。

7. 内存压缩与端到端加速之间存在明显差距：平均 5.0× 内存压缩只带来 1.48× 端到端加速，说明实际延迟还受 kernel、数据搬运、注意力计算和压缩开销影响。这是部署时的重要观察：KV cache 内存下降不等比例转化为吞吐提升。

8. 从负结果角度看，R²-Omni 并非在每个子指标上都优于所有基线，尤其 Qwen2.5-Omni-7B 上量化基线已很强，R²-Omni 的优势主要体现在“结构压缩与精度压缩联合”以及较小模型/较难模型上的稳定性；这也提示后续工作需要更细的 per-task、per-metric 分析，而不能只看平均质量保持率。

Q6: 有什么可以进一步探索的点？

1. 自适应精度分配：论文明确指出压缩效果部分取决于精度分配策略，且内存效率与生成质量的平衡可能随模型和应用变化。后续可以研究按模型、层、头、任务动态调整位宽预算，而不是依赖固定超参数。

2. 更可靠的敏感度估计：SSPA 目前依赖注意力输出误差的估计和语义先验。可以进一步分析量化误差在多层的传播、键量化与值量化的交互，并引入更接近生成质量的可微或可校准代理，甚至给出误差上界。

3. 结构冗余的鲁棒建模：STC 依赖视频时空相似与音频局部连续性，未来可以针对快速场景切换、重叠语音、多人对话、细粒度 OCR、罕见事件和长镜头等失效场景设计更稳健的相似度或分割策略。

4. 与 KV cache eviction、attention sparsity、prefill 加速结合：R²-Omni 目前主要减少 decode 阶段的 KV cache 成本，后续可与 token eviction、稀疏注意力、chunked prefill 和硬件 kernel 协同，形成端到端长音视频推理系统。

5. 训练感知或轻量校准：论文坚持 training-free，但未来可以探索轻量校准、测试时自适应或少量可学习参数，在保持 plug-in 特性的同时提升压缩策略的模型适应性。

6. 更全面的评测：需要在更多模型架构、更多语言、更多模态、流式输入、多轮对话和 agentic 长程任务上验证；同时应加入人工评测、事实性、时间顺序一致性、幻觉率和安全性指标，避免自动长文本指标掩盖关键错误。

7. 理论分析：可以把结构冗余与精度冗余放入率失真或信息瓶颈框架，分析在给定内存预算下 token 合并与位宽分配的联合最优性，解释为什么 query-guided 选择在长文本生成中次优。

8. 系统与部署：需要测量不同 batch size、序列长度、GPU 架构下的真实吞吐、首 token 延迟、内存带宽和压缩开销，探索与 paged attention、连续批处理和服务化推理框架的集成。

9. 扩展到 agentic systems：论文结论明确提到未来将扩展到更广泛的模型架构和具有更多样生成需求的 agentic 系统。对于长程多模态智能体，KV cache 压缩可能直接影响记忆保持、工具调用上下文和历史视听证据的保留，是值得继续探索的方向。

Q7: 总结一下论文的主要内容

R²-Omni 是一篇面向 Omni-LLM 长文本生成的推理效率论文。它的起点不是“如何再设计一个更强的多模态模型”，而是“如何在不大幅损失开放式生成质量的前提下，压缩音视频输入带来的巨大 KV cache”。论文首先指出，现有 token pruning 在多项选择任务上可以在很高压缩率下维持性能，但长文本生成会明显退化；作者将这一差距归因于 query-guided token selection 的偏差：它倾向于保留回答当前问题所需的少量证据，却丢弃全面描述所需的事件、属性、时间和跨模态细节。

围绕这一诊断，论文把冗余拆成两层。第一层是结构冗余：音视频 token 之间存在大量重叠信息，视频表现为时空相似，音频表现为局部连续性，因此可以通过 Structure-Aware Token Consolidation（STC）合并表示，而不是按 query 注意力硬删 token。第二层是精度冗余：即使 token 数已经减少，保留表示的缓存值仍可能使用过高数值精度，因此可以用 Semantic-Sensitivity Guided Precision Allocation（SSPA）按注意力输出误差的估计削减量分配值精度，并用 query attention 与音视频对齐作为语义先验。两者结合形成 training-free 的 R²-Omni 框架，分别压缩 token 数与位宽。

实验主线覆盖三个 Omni-LLM（Qwen2.5-Omni-3B、Qwen2.5-Omni-7B、MiniCPM-o-4.5）和三个长文本音视频基准（video-SALMONN-2、LongVALE、OmniDCBench），对比 token pruning 基线 OmniZip、OmniSelect、SEATS，KV-cache quantization 基线 KIVI、OTT、OScaR，以及 naive composed 组合。论文报告平均 5.0× 内存压缩、97.98% 的基线生成质量保持和 1.48× 端到端加速。Table 2 的结果显示，token pruning 在长文本上退化最明显，量化基线更稳但仍有缺口，R²-Omni 在多数设置下取得更好的质量-压缩折中；模型规模与架构会显著影响压缩鲁棒性，MiniCPM 上的 token pruning 甚至出现接近崩塌的质量保持率。

论文的论证主线可以概括为：先观察任务差异，再归因于 query-guided 选择的覆盖不足，然后把问题重述为结构与精度两层冗余，接着用 STC 与 SSPA 分别处理两层冗余，最后用多模型、多基准、消融和效率实验支撑“联合压缩优于单轴压缩”的结论。其贡献更偏系统框架与问题重述，而不是提出新的预训练目标；优势在于 training-free、可插拔、对长文本生成友好，局限在于精度分配策略依赖模型与应用，结构相似性和误差代理仍是启发式信号，且 5× 内存压缩只转化为 1.48× 端到端加速。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向直接相关：论文讨论的是长文本开放式生成中的信息覆盖问题，而不是只优化多项选择准确率，适合关注长输出质量的读者。

## 基本信息

- 作者：Bolong Feng, Pengfei Zhou, Zhenglin Wan, Wei Wang, Wangbo Zhao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-11-02
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.03247v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 检索命中的 Abstract、Introduction、Method、Results 与 Conclusion 片段，未在片段中展开的公式和实现细节已在相应位置标注为合理推断或待核对。
