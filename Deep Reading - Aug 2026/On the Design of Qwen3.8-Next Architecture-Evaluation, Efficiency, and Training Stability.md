---
user_id: "cheng tan"
paper_id: 10062
arxiv_id: "2608.30320v1"
title: "On the Design of Qwen3.8-Next Architecture: Evaluation, Efficiency, and Training Stability"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30320v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30320v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:48:00"
---
# On the Design of Qwen3.8-Next Architecture: Evaluation, Efficiency, and Training Stability

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：mixture-of-experts · sparse attention · gated delta network · n-gram embedding

## 一句话总结

本文描述了 Qwen3.8-Flash-Next 稀疏 MoE 模型的架构设计与消融实验：在约 1/9 训练 FLOPs、1/3 激活参数和 1/3 训练 token 的条件下，借助 Gated DeltaNet 与全局注意力混合、QSA 稀疏注意力、Gated Residual 残差门控、主机 n-gram 嵌入表以及 Muon 优化器，在 14 个预训练基准上追平前代 397B-A17B 旗舰模型，并同时提升训练稳定性、放宽最优超参数的要求。

## 摘要

> We describe the architecture and ablations of Qwen3.8-Flash-Next, a sparse mixture-of-experts model with 125B parameters, 6B activated per token, and additional 51B parameters of n-gram embedding tables held off the accelerator. On fourteen pre-training benchmarks the model leads the 397B-A17B predecessor on eight and trails it on the rest by at most 2.6 points, at 1/3 the activated parameters, 1/3 the training tokens, and roughly 1/9 the training FLOPs. Token mixing uses a layer-wise hybrid of Gated DeltaNet (GDN) and global attention, with one full-attention layer in every four; at continued-pretraining time those full-attention layers are replaced by Qwen Sparse Attention (QSA), which scores context at micro-block granularity with a compressed lightweight indexer. The residual stream is widened to four branches and read through an elementwise gate, a design we call the Gated Residual (GR). Capacity is added outside the backbone by a single n-gram embedding layer whose tables are prefetched from host memory. We evaluate every candidate change along three axes: loss together with downstream benchmarks; the cost of the change in training, prefill and decode; and its effect on the optimal hyperparameters and training stability. Loss and downstream accuracy do not always move together: enlarging the n-gram vocabulary lowers loss monotonically while downstream accuracy saturates. The architecture and the Muon optimizer together shift the optimal learning rate and batch size upwards, render batch-size warmup unnecessary, and substantially improve stability under stress tests. Loss, benchmarks, efficiency and stability form one design problem. Solved jointly, they yield a recipe that is simultaneously more efficient, more capable and more stable.

Q1: 这篇论文试图解决什么问题？

本文试图解决的核心问题是如何在保持大语言模型能力的同时大幅降低训练与推理成本，并确保大规模训练稳定。具体而言，前代旗舰 Qwen3.8 系列的 397B-A17B 模型虽然质量高，但激活参数多、训练计算量大，部署成本高昂。论文的目标是设计一个新一代架构，使得模型在质量上不显著落后于该旗舰，但激活参数、训练数据和训练计算量都大幅缩减。

这一问题的难点在于多个设计目标相互耦合。第一，当改变架构（如引入新的注意力机制、残差结构、额外嵌入表）时，最优学习率、批量大小和 warmup 策略都会发生变化，单独的调参难以公平比较。第二，预训练 loss 与下游 benchmark 并不总是一致：某些改动（如扩大 n-gram 词汇表）可以持续降低 loss，但下游准确率却出现饱和，因此不能只看 loss 或只看下游。第三，在模型规模达到万亿参数、训练数据达到数十万亿 token 时，会出现小规模实验完全看不到的稳定性问题，例如梯度爆炸、loss spike 等，需要专门的稳定性应对手段。第四，效率评估必须覆盖训练、prefill 和 decode 多个阶段，否则某个看似高效的改动可能只是把成本转移到推理阶段。

因此，论文把“loss + 下游基准”、“效率成本”和“超参数与稳定性”三条轴统一纳入架构选择的决策框架，希望在联合优化的意义上找到比前代更优的帕累托点。这个问题的解决不仅产生一个具体的模型，更重要的是建立了一种可复用的架构设计与评估方法论。

Q2: 有哪些相关研究？

从摘要和检索到的片段来看，相关工作主要涉及以下几个方向（由于本文的 related work 部分未在检索证据中完整出现，以下基于常识与碎片信息归纳）：

1. 稀疏专家混合（MoE）模型：如 Qwen3 系列、前代 397B-A17B 等，通过只激活部分参数来节省计算。本文在 MoE 主干之外进一步增加 n-gram 嵌入表，相当于把一部分模型容量放到加速器之外。

2. 线性注意力与门控线性注意力：Gated DeltaNet（GDN）属于线性注意力的一种，旨在以更低的解码成本替代全注意力。本文采用 GDN 与全局注意力的混合排列，类似其他混合注意力模型（如 Jamba、Samba 等）的思路。

3. 稀疏注意力：Qwen Sparse Attention（QSA）在微块粒度上对上下文打分，并使用压缩的轻量级索引器，属于高效 Transformer 推理方向，可能与 StreamingLLM、Quest、MInference 等有交集。

4. 输入表征增强：n-gram 嵌入表提供了类似 n-gram 语言模型的额外特征，可视为对 token embedding 的一种补充，相关思想可追溯到 n-gram 统计语言模型与现代 NN 模型的结合（如 word n-gram embedding）。

5. 优化器：Muon 优化器（论文中提及）是一类利用矩阵正交化更新规则的优化器，与 AdamW 等主流优化器不同，它可能影响梯度更新的方向性和稳定性。

6. 大规模训练稳定性：检索片段引用 Chowdhery et al. 等人强调在万亿参数规模下会出现小规模实验不存在的不稳定性，相关技术包括渐变裁剪、warmup、loss spike 修复等。

7. 评估协议：片段显示评估使用了 WE-bench、MGSM、MMMLU、INCLUDE 等多语言与代码基准，体现了对多语言能力的重视。

由于缺乏完整的相关工作列表，以上仅为方向性概述，具体对比与分析需查阅论文原文。

Q3: 论文如何解决这个问题？

论文提出的解决方案由几个核心部分组成：

1. 稀疏 MoE 主干：整体为包含 125B 总参数的 MoE 模型，每 token 激活 6B 参数。相比前代 397B-A17B，激活参数减少为 1/3，训练 token 减少为 1/3，训练 FLOPs 约为 1/9。

2. Gated DeltaNet（GDN）与全局注意力混合：token mixing 采用逐层混合的方式，每四层放置一个全局注意力层，其余层使用 GDN。GDN 是一种线性注意力变体，能在解码时实现 O(1) 复杂度，但精度低于全注意力，因此用混合保证整体质量。在持续预训练阶段，这些全注意力层被 Qwen Sparse Attention（QSA）替换，QSA 在 micro-block 粒度上对上下文打分，并使用一个压缩的轻量级索引器快速识别相关块，从而降低长上下文下的注意力计算和显存占用。

3. Gated Residual（GR）：残差流被加宽为四个分支，通过一个逐元素门控（elementwise gate）将各分支加权合并。这种设计增加了信息通路，可能有助于更稳定地传播梯度，并且与门控 DeltaNet 的距离门控思路相兼容。

4. 外部 n-gram 嵌入层：在主干之外增加一个 n-gram 嵌入表（51B 参数），这些参数不放在加速器内，而是在需要时从主机内存预取。该设计允许在不增加加速器显存和计算负担的情况下扩大模型容量。

5. Muon 优化器：训练采用 Muon 优化器，它与架构设计共同改变了最优超参数的位置——最优学习率和批量大小都上移，批量大小 warmup 不再必要。这暗示 Muon 的自适应更新机制可能对梯度尺度和噪声更稳健。

6. 三轴评估框架：对每个候选改动，同时评估 (a) loss 与下游基准，(b) 训练/prefill/decode 中的成本，(c) 对最优超参数和训练稳定性的影响。通过这三轴联合消融，最终选出同时满足质量、效率和稳定性的配置。

整个设计强调联合优化，而不是单独调优某一环节。例如，架构改变会改变 loss landscape，进而影响最优优化器超参数，因此通过调整超参数来适配架构比固定超参数更公平也更实用。

Q4: 论文做了哪些实验？

依据摘要和检索到的评估片段，论文进行了以下实验：

1. 基础模型对比评估：在 14 个预训练基准上评估 Qwen3.8-Flash-Next-Base，并将其与前一世代旗舰进行对比。摘要明确提到在 14 个基准上，模型领先 8 个，其余基准最多落后 2.6 分。

2. 候选改动消融：对每个架构或训练配方改动（例如是否加入 n-gram 嵌入、是否替换注意力、GR 的分支数量、Muon 优化器是否使用等）进行消融，并沿三条轴记录效果：loss 与下游基准、训练/prefill/decode 成本、超参数与稳定性。这些消融是架构选择的核心依据。

3. 稳定性压力测试（Section 3.3）：专门设计了压力测试以观察训练稳定性。检索片段提到当模型达到万亿参数和数十万亿 token 时会出现独特的不稳定问题，图表中展示了训练 loss、预裁剪（pre-clip）梯度范数和滑动窗口标准差等指标。这说明实验关注了 loss spike、梯度爆炸等情况。

4. 基准集细目：检索到的评估片段列出了部分任务，包括 WE-bench（Jimenez et al., 2024）、多语言任务 MGSM（8-shot CoT）、MMMLU（5-shot）、INCLUDE（5-shot）等，并提及“Tab. 11 compares Qwen3.8-Flash-Next-Base with two strong baselines”。这表明评估覆盖了编码、推理、数学、多语言等多个能力维度。

5. 超参数敏感性实验：论文观察了最优学习率和批量大小在架构与 Muon 优化器组合下的变化，结果显示最优值上移且批量大小 warmup 不再必要。这类实验通常需要通过网格扫描或搜索完成，但具体搜索范围未在摘要中给出。

6. 效率测量：对训练、prefill 和 decode 三个阶段的成本进行了量化，用以比较不同改动带来的效率影响。具体数值未在摘要中披露，需要查阅正文中的图表。

由于检索证据有限，具体的实验设置、超参数范围和额外对比模型（如 Qwen3.8-27B-Base）未能从 PDF 片段中确认，建议阅读原始论文的第 3、4 节以获取完整细节。

Q5: 发现了什么实验现象？

从摘要和检索片段中可归纳出以下几类实验现象：

1. 质量与计算效率的权衡：在 14 个预训练基准上，Qwen3.8-Flash-Next 相比 397B-A17B 前代在 8 个基准上取得更好或相当的成绩，其余基准的降幅在 2.6 分以内，而激活参数、训练 token 和训练 FLOPs 分别大幅降低。这暗示架构改进带来的效率红利没有以同等比例牺牲质量。

2. loss 与下游准确率的非同步性：摘要明确指出“扩大 n-gram 词汇表会让 loss 单调下降，但下游准确率趋于饱和”。这是一个反直觉现象：训练目标（loss）与任务表现（benchmark）之间的相关性在某些干预下变弱，说明以 loss 为唯一早停或模型选择标准可能产生误导。

3. Muon 优化器与架构对超参数的影响：架构和 Muon 优化器联合作用后，最优学习率与批量大小均上移，且批量大小 warmup 不再必要。这说明传统上认为“批量大小必须从小 warmup 到大”的直觉在某些架构下不成立，可能归因于 Muon 更稳健的更新规则与 GR 等结构的梯度流改善。

4. 稳定性提升：在压力测试下，训练稳定显著改善。检索到的图表标题提到“Pre-clip gradient norm and sliding window std.”，表明模型通过预裁剪梯度范数控制和滑动窗口方差监测来防止梯度爆炸。具体数值未在片段中给出，但从摘要可知稳定性是明确提升的。

5. 注意力的质量与成本平衡：在持续预训练阶段用 QSA 替换全注意力后，模型仍保持了与前代相当的质量。这证实了在长上下文场景下，微块粒度的稀疏注意力可以在几乎不掉点的情况下大幅降低计算。

6. 基准内的表现差异：模型在 8 个基准上领先，其余落后最多 2.6 分，说明不同能力维度对架构改动的敏感度不同，先验上可能数学/代码任务对注意力精度更敏感，而多语言任务对 n-gram 嵌入更受益。但该推断仅基于摘要，需要查看分任务的详细结果。

需要说明的是，heuristic draft 中提到的“Qwen3.8-Flash-Next-Base 全面优于 Qwen3.8-27B-Base”以及“优于 Qwen3.7-Plus-Base 8/14 基准”等说法未在检索证据中直接出现，因此本文档不将其视为已确认的实验观察。

Q6: 有什么可以进一步探索的点？

基于论文的方法和发现，未来可在以下方向进一步探索：

1. n-gram 词汇量与下游表现的权衡：论文观察到 loss 随 n-gram 词汇量单调下降但下游饱和，可以进一步研究饱和点的成因，以及是否可以通过更好的 n-gram 表示（例如哈希、子词组合）突破饱和。

2. Gated Residual 的泛化性：GR 在 Qwen3.8-Flash-Next 中表现良好，可在其他主干（如纯 Transformer、卷积模型）中验证其效果，并探索分支数量和门控形式的最优设计。

3. 注意力替换模式的演进：当前每四层一个全局注意力层，持续预训练时替换为 QSA。未来可以测试其他替换比例、多层混合或动态选择策略，以实现更好的质量-成本曲线。

4. Muon 优化器与更大规模的结合：Muon 在 125B 规模上显著改变超参行为，值得在更大参数规模（如万亿参数）和更多任务上测试其稳定性和收敛性。

5. n-gram 嵌入表的主机-加速器协同优化：预取策略目前是简单的主存读取，未来可与更智能的缓存调度、异步预取、压缩存储结合，减少带宽压力和 I/O 延迟。

6. 长上下文与推理效率：QSA 的索引器设计可以进一步优化，例如学习式的微块重要性预测，或在不同层使用不同稀疏度，以降低长上下文解码时的内存瓶颈。

7. 联合评估框架的推广：论文的三轴评估方法可推广到其他架构设计任务，作为系统化比较的模板。

8. 多模态与指令微调：论文只报告了预训练结果，架构在后续 SFT、RLHF 和视觉/语音模态上的表现尚待验证。

Q7: 总结一下论文的主要内容

本文是 Qwen 团队对新一代架构 Qwen3.8-Flash-Next 的设计与消融报告，核心主张是：在模型质量、训练/推理效率和训练稳定性三者之间存在耦合关系，只有将它们作为联合设计问题求解，才能得到整体更优的方案。

一、研究背景与动机
前代旗舰 Qwen3.8 的 397B-A17B 模型拥有极高能力，但激活参数多、训练 token 和 FLOPs 需求大，部署成本和训练成本都较高。论文希望设计一个新架构，在维持相近能力的同时大幅降低资源消耗，并保证在大规模训练中的稳定性。作者指出，当模型规模达到万亿参数、数据量达到数十万亿 token 时，会出现小规模实验不存在的不稳定现象，例如梯度尖峰、损失发散等，因此稳定性是架构设计的必要维度。

二、架构设计
Qwen3.8-Flash-Next 是一个稀疏 MoE 模型，总参数 125B，每 token 激活 6B，另有 51B 参数的 n-gram 嵌入表存放在加速器之外的主机内存，使用时预取。token mixing 采用逐层混合结构：每四层包含一个全局注意力层，其余层为 Gated DeltaNet（GDN）。GDN 是一种门控线性注意力，解码复杂度低，但表达能力有限，因而与全局注意力混合以兼顾质量。在持续预训练阶段，全注意力层被 Qwen Sparse Attention（QSA）取代，QSA 将上下文切割为微块，用压缩轻量索引器评分，从而显著降低长上下文下的计算量和内存占用。残差流被加宽为四个分支，并通过逐元素门控（Gated Residual, GR）来融合信息，增强了梯度流和容量。模型外部另设单一的 n-gram 嵌入层，将跨 token 的 n-gram 统计信息注入模型，这是在不增加主干计算的情况下扩充容量的手段。训练使用 Muon 优化器，它与架构设计协同，改变了最优超参数的位置。

三、评估与消融框架
论文提出沿三个轴评估每个候选变更：第一轴是预训练 loss 和下游 benchmark；第二轴是训练、prefill 和 decode 阶段的效率成本；第三轴是对最优超参数和训练稳定性的影响。这一框架避免只追求 loss 下降而牺牲下游性能，也防止只顾训练效率而拖慢推理。所有候选改动都在这三个轴上同时测量，最终选出的方案是三者折中的结果。

四、主要实验结果
在 14 个预训练基准上，新模型相比 397B-A17B 前代在 8 个基准上领先或持平，其余基准最多落后 2.6 分，但只用了 1/3 激活参数、1/3 训练 token 和约 1/9 训练 FLOPs。评估包含多种任务，如代码任务 WE-bench、多语言任务 MGSM/MMMLU/INCLUDE 等。论文观察到 loss 与下游准确率并不总是同步：扩大 n-gram 词汇表使 loss 单调下降，而下游准确率趋于饱和，说明不能以 loss 作为唯一设计指标。架构和 Muon 优化器联合导致最优学习率和批量大小上移，批量大小 warmup 不再必要，并且压力测试中稳定性显著提升。这些结果证明了联合设计的有效性。

五、结论与意义
论文将 loss、benchmark、效率和稳定性视为一个统一的设计问题，并给出了一套同时更高效、更强大、更稳定的具体实现。其方法论意义在于：大模型架构研究不能孤立地评估单一指标，必须考虑架构、优化器和超参数之间的相互作用。该工作为后续高效大模型设计提供了新的基准和可复用的评估范式。

（注：本总结基于摘要和检索到的引言、结论、评估与稳定性片段，部分细节如具体消融表格、超参数扫描范围需要查阅原文。）

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent 方向：高效且稳定的基础模型可降低 agent 系统的推理成本和训练成本，特别是长上下文场景下 QSA 与混合注意力能改善响应延迟。

## 基本信息

- 作者：Zihan Qiu, Zekun Wang, Xiao Li, Yanpeng Li, Yang Xu, Yixuan Wang, Huaqing Zhang, Rui Men, Bochao Mao, Chengruidong Zhang, Fan Zhou, Hao Luo, Haofeng Huang, Haoran Lian, Haoyan Huang, Hongqing Chen, Jianwei Zhang, Jing Xu, Junjie Wang, Langshi Chen, Liangyu Wang, Linlang Jiang, Man Yuan, Minmin Sun, Peng Jin, Siqi Zhang, Siyu Wang, Xingzhang Ren, Yakai Wang, Yi Zhang, Yiming Dong, Yizhong Cao, Yubo Ma, Yunfei Mao, Bo Zheng, Dayiheng Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30320v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了提供的 PDF 语义检索证据（摘要、引言、结论、评估与稳定性测试片段），并结合元数据和摘要信息；部分推测已在文中标注，heuristic draft 中未获证据支持的对比未纳入结论。
