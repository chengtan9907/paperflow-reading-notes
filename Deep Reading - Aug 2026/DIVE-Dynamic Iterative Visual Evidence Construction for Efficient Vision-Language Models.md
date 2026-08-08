---
user_id: "cheng tan"
paper_id: 6704
arxiv_id: "2608.04496"
title: "DIVE: Dynamic Iterative Visual Evidence Construction for Efficient Vision-Language Models"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.04496.pdf"
pdf_url: "https://arxiv.org/pdf/2608.04496"
abs_url: "https://arxiv.org/abs/2608.04496"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-06T11:15:46"
---
# DIVE: Dynamic Iterative Visual Evidence Construction for Efficient Vision-Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：vision-language model · visual token pruning · efficient inference · token compression

## 一句话总结

DIVE 提出一种免训练的视觉 token 动态迭代证据构建框架，把传统的静态 top-k 视觉 token 剪枝转化为 select-update-re-evaluate 的迭代过程，通过残差条件化打分与耦合单侧更新来抑制已覆盖证据，从而在极高压缩率下（88.9% 视觉 token 减少）保持 VLM 平均性能的 98.2%。

## 摘要

> Visual inputs in vision-language models (VLMs) are often encoded into substantially longer token sequences than text, making visual tokens a major bottleneck for efficient inference. Abundant recent methods address this bottleneck by scoring token importance and pruning low-scoring tokens in a single pass. However, one-shot scoring is insufficient because a token's prompt-relevant usefulness depends on the evidence already retained. Motivated by this insight, we introduce DIVE (Dynamic Iterative Visual Evidence Construction), a training-free framework that recasts visual-token pruning as dynamic evidence construction. DIVE repeatedly selects the remaining token with the highest residual-conditioned score, updates the visual and prompt residuals to discount the evidence already explained, and re-evaluates the remaining tokens. This select-update-re-evaluate process builds a retained set of complementary, prompt-relevant evidence. Experiments across eight image-understanding benchmarks show that DIVE consistently preserves performance across token budgets. With an 88.9% reduction in visual tokens, DIVE retains 98.2% of the uncompressed model's average performance. Code is available at https://github.com/Zhong-Chenchen/DIVE.git.

Q1: 这篇论文试图解决什么问题？

DIVE 要解决的核心问题是：视觉 token 是 VLM 推理效率的主要瓶颈，而现有视觉 token 剪枝方法的单次（one-shot）重要性打分存在根本性缺陷——一个 token 的价值不是静态属性，而是相对于当前已保留证据而言的条件属性。具体来说：(1) 视觉输入（如图像 patch 编码后的 token）在数量上远超文本 token，导致自注意力计算量随序列长度平方增长，推理延迟和显存占用显著增大；(2) 已有方法通常一次性计算所有 token 的重要性分数，然后直接保留 top-k 个 token，这种贪婪的静态选择忽略了 token 之间的冗余性和互补性——两个各自高分但语义重叠的 token 同时保留时，边际信息增益远低于一个高分 token 加一个低分但覆盖新区域的 token；(3) 单次打分也无法利用提示（prompt）与图像之间的交互信息在剪枝过程中的动态变化，因为一旦某些证据被确认保留，剩余 token 的“有用性”定义就应该随之改变。论文的出发点是：更优的剪枝应该是一个迭代的证据构建过程，每一步的选择都会改变后续 token 的价值评估，因此需要 select-update-re-evaluate 的循环，而不是一次性的静态筛选。

Q2: 有哪些相关研究？

相关研究主要分为几个方向：(1) 基于注意力权重的视觉 token 剪枝——利用 VLM 中视觉 token 与文本 token 或 [CLS] token 的注意力分数作为重要性信号，剪掉低注意力 token，如 FastV 等；(2) 基于提示相关性（prompt relevance）的打分——直接度量视觉 token 与文本提示的语义相关性，保留与问题最相关的视觉区域；(3) 基于视觉冗余（visual redundancy）的方法——利用视觉特征之间的相似度或聚类结构去除冗余 token，例如相似度聚合、token merging 等；(4) 重要性-多样性（importance-diversity）混合准则——在打分之外额外引入多样性约束，避免选出的 token 集中于某一图像区域。论文在实验部分将 DIVE 与基于注意力、提示相关性、视觉冗余和重要性-多样性准则的主流方法进行了比较，并额外考虑了 MMTok、CDPruner 和 SCOPE 等方法（这些方法的具体机制在检索证据中未展开，合理推断它们分别代表多模态 token 剪枝、基于 CDP 的剪枝和某种范围选择式剪枝）。与此前一次性打分方法不同，DIVE 属于迭代/贪心子集选择范式，与贪心子集选择方法（greedy subset selection）有方法上的亲缘关系，但通过残差更新机制将“已选证据的边际贡献”显式纳入后续选择。

Q3: 论文如何解决这个问题？

DIVE 的核心是一个免训练（training-free）的迭代框架，不需要任何参数更新或微调，直接在推理时对视觉 token 进行动态筛选。具体流程为：(1) 初始化：将所有视觉 token 视为候选集，设定 token 预算（即要保留的 token 数量）；(2) 选择（select）：在每一轮迭代中，从剩余 token 中选出“残差条件化得分”最高的 token，该得分不仅考虑 token 自身的视觉显著性和与提示的相关性，还考虑该 token 相对于当前已选证据集的边际新信息量；(3) 更新（update）：将被选中的 token 所覆盖的视觉-文本证据从残差中扣除，即对视觉残差和提示残差做耦合的单侧更新（coupled one-sided updates），使得已经解释过的信息不再重复贡献分数；(4) 重新评估（re-evaluate）：在更新后的残差上重新计算剩余 token 的得分，因为一个 token 的价值在证据被部分解释后已经改变；(5) 重复 select-update-re-evaluate 直到达到预算。这一过程使得最终保留的 token 集合具备两个关键性质：互补性（complementary）——不同 token 覆盖不同的视觉证据区域，避免冗余；提示相关性（prompt-relevant）——每一步都结合提示残差进行条件化，确保选出的证据与当前问题对齐。论文特别指出，现有的单次打分方法等价于 DIVE 只执行一轮选择而不做残差更新的特例；DIVE 的关键贡献在于把“价值”从静态属性变成动态条件属性，并通过抑制已覆盖证据引导后续选择走向未覆盖区域、次要物体或互补上下文细节。

Q4: 论文做了哪些实验？

论文在 8 个图像理解基准上进行了零样本（zero-shot）评估，每个基准遵循官方评测划分和协议，默认设置来自 lmms-eval。主实验的受控设置使用 LLaVA-1.5-7B，每张图像编码为 576 个视觉 token，在该设置下系统地变化 token 预算以评测不同压缩率下的性能保持情况。对照方法覆盖四类主流视觉 token 剪枝准则：基于注意力（attention）、提示相关性（prompt relevance）、视觉冗余（visual redundancy）和重要性-多样性（importance-diversity）；此外还额外对比了 MMTok、CDPruner 和 SCOPE 等较新的子集选择方法。实验报告了 DIVE 在 88.9% 视觉 token 减少（即保留约 64/576 token）时保留未压缩模型平均性能的 98.2%，以及在多个预算档位（如保留 100.3%、99.9%、98.2% 等）上的表现。除主比较外，论文还包含对贪心子集选择方法的额外比较（C.1 节）以及消融/分析实验（检索证据未覆盖细节，推测包括对更新策略、残差定义、迭代轮数等的分析），以验证 DIVE 在精度与效率之间的平衡。

Q5: 发现了什么实验现象？

实验观察到的核心现象是：在极端的 token 预算下（如减少 88.9% 的视觉 token），DIVE 仍然能保持未压缩模型平均性能的 98.2%，而静态单次打分方法在同等压缩率下通常会出现明显性能下降（对照方法的具体数值在检索证据中未给出，合理推断其性能下降幅度大于 DIVE）。在 LLaVA-1.5-7B 的 576 token 设置下，DIVE 在不同预算档位分别保留了未压缩模型平均性能的 100.3%、99.9%、98.2%，这说明在温和压缩下 DIVE 甚至可能带来轻微的性能提升（100.3% 可能源于剪枝带来的隐式去噪或注意力聚焦效应，属于推测），而在极端压缩下性能退化是平滑而非急剧的。另一个现象是“token 价值随证据更新而改变”这一设计动机得到验证：与一次性打分相比，迭代残差更新能持续将后续选择引导至未覆盖图像区域、次要物体和互补上下文细节，说明视觉 token 剪枝中的冗余主要来自证据重叠而非单个 token 的绝对重要性不足。由于检索证据未覆盖完整实验表格和负结果，无法确认 DIVE 在哪些基准上提升最显著、在哪些基准上仍有性能缺口，也无法确认与 MMTok/CDPruner/SCOPE 的对比中是否存在失败案例，这是信息缺口。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：(1) 将 DIVE 的迭代残差框架扩展到其他模态（如视频、音频）的 token 剪枝，验证 select-update-re-evaluate 范式是否在时序模态中同样有效；(2) 与训练类方法结合，例如把 DIVE 选出的 token 作为可微分的硬选择模块接入端到端训练，或学习残差更新函数以替代手工设计的耦合更新规则；(3) 探索更高效的残差更新实现，当前迭代过程在每一轮都需要重新计算剩余 token 的得分，计算开销随迭代轮数线性增长，可以通过近似、缓存或分层选择来加速；(4) 将 DIVE 与 KV cache 压缩、投机解码等推理加速技术组合，在系统层面获得更大吞吐收益；(5) 在更大规模 VLM（如 13B/34B 甚至 MoE 模型）和多图/高分辨率输入设置下验证 DIVE 的泛化性和最优预算调度策略；(6) 将证据构建的解释性用于 VLM 可解释性分析，例如用 DIVE 的残差轨迹来可视化模型每轮决策依据；(7) 研究 token 预算的动态分配——目前预算在推理前固定，可以探索根据输入复杂度和提示难度自适应决定预算；(8) 分析 DIVE 在对抗样本、分布外图像、细粒度计数/空间关系任务上的失败模式，因为“覆盖互补区域”的偏向可能牺牲某些需要全局上下文的细粒度任务。

Q7: 总结一下论文的主要内容

DIVE 论文的论证主线是：VLM 推理效率的瓶颈在于视觉 token 数量远大于文本 token，而现有的剪枝方法采用单次打分+静态 top-k 选择，这种范式忽略了 token 价值的条件性——一个 token 是否值得保留取决于已经保留了哪些证据。由此提出关键洞见：视觉 token 的价值不是独立静态属性，而是相对于当前已构建证据集的边际贡献。技术主线上，DIVE 将剪枝重新定义为动态证据构建，采用 select-update-re-evaluate 三阶段循环：每轮选出残差条件化得分最高的剩余 token，然后对视觉与提示残差做耦合单侧更新以折扣已解释证据，再在更新后的残差上重新评估剩余 token，重复直到填满预算。这种设计使得最终保留的 token 集合既互补又提示相关，避免了重复覆盖同一图像区域。实验主线上，论文在 8 个图像理解基准上用 lmms-eval 官方协议做零样本评估，主受控实验使用 LLaVA-1.5-7B（每图 576 token），对比了基于注意力、提示相关性、视觉冗余和重要性-多样性的主流剪枝方法，并额外纳入 MMTok、CDPruner、SCOPE 等贪心子集选择方法。核心定量结果是：在减少 88.9% 视觉 token 的条件下，DIVE 保留未压缩模型平均性能的 98.2%；在温和预算下甚至达到 100.3% 的相对性能。综合来看，DIVE 的主要贡献是一个免训练、即插即用的迭代剪枝框架，以少量额外迭代计算换取极端压缩率下的性能保持，为 VLM 高效推理提供了不同于静态打分的新范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 VLM 高效推理研究：DIVE 提供了不同于静态 top-k 剪枝的新范式，其残差条件化思想可迁移到其他 token 压缩场景。

## 基本信息

- 作者：Chen Zhong, Xiao An, Zijie Wang, Jiepan Li, Guangyi Yang, Wei He
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.LG
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.04496`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据，主要命中 Abstract、Introduction、Conclusion、Evaluation Protocol 和 Main Comparisons 相关片段；由于检索证据未覆盖完整方法细节和全部实验表格，部分分析标注为合理推断或推测。
