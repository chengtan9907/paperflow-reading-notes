---
user_id: "cheng tan"
paper_id: 6954
arxiv_id: "2608.06243v1"
title: "DASH: Divergence-Adaptive Supervision Horizons for On-Policy Self-Distillation of Reasoning Models"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.06243v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.06243v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T14:00:30"
---
# DASH: Divergence-Adaptive Supervision Horizons for On-Policy Self-Distillation of Reasoning Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · verifiable rewards · self-distillation · reasoning models

## 一句话总结

提出 DASH 方法，通过根据局部蒸馏信号与序列均值的偏差自适应调节反向多步聚合权重，解决 on-policy self-distillation 中时间结构未充分利用的问题，在三个数学推理基准上全面优于 vanilla OPSD。

## 摘要

> Reinforcement learning with verifiable rewards (RLVR) improves the reasoning capabilities of large language models using automatically verifiable outcome signals, but these signals are typically sparse and at the sequence-level. On-policy self-distillation (OPSD) mitigates this sparsity by querying a privileged teacher at student-visited prefixes and providing dense token-level distributional supervision. Although this dense supervision alleviates signal sparsity, we find that standard OPSD still underexploits the temporal structure of the rollout. It assigns every local divergence the same coefficient, regardless of its position or the divergence sequence in which it occurs. In on-policy autoregressive generation, the same divergence magnitude can follow different discrepancy histories, reflecting different evolutions of the mismatch between the teacher and student. Since the local scalar alone cannot distinguish these temporal contexts, standard OPSD cannot adapt its token-level weights to the realized discrepancy sequence. To address this limitation, we propose Divergence-Adaptive Supervision Horizons (DASH). DASH maps the gap between each local distillation signal and the sequence-level mean to an adaptive propagation gate and then uses these gates to control backward multi-step aggregation. By doing so, DASH adjusts token-level supervision weights according to how local divergences evolve during generation. Experiments on three mathematical reasoning benchmarks across three model scales show that DASH improves over our matched vanilla OPSD reruns on every benchmark at all three scales. DASH reuses the teacher and student distributions that OPSD already computes, so the gains require no additional teacher or student forward pass.
> Code: https://github.com/DBtxy/DASH-OPSD

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题如下：
1. RLVR（Reinforcement Learning with Verifiable Rewards）是当前提升大语言模型数学推理与代码生成能力的重要后训练范式，但其奖励信号通常是序列级且稀疏的，导致学习信号不足、优化效率低。
2. On-policy self-distillation（OPSD）通过查询特权教师模型（privileged teacher）在学生访问的每个前缀位置提供密集的 token 级分布监督，缓解了信号稀疏性。然而，标准 OPSD 在聚合这些局部蒸馏信号时，对所有 token 赋予相同权重，完全忽略了这些信号在生成过程中的时间演化结构。
3. 具体而言，在自回归生成中，同一个局部散度（local divergence）数值可能出现在不同的散度历史之后，反映教师与学生失配的不同演化路径。例如，散度可能从高到低变化，也可能从低到高变化，或者持续波动。标准 OPSD 仅依赖局部标量，无法区分这些时间上下文，因此无法根据实际发生的散度序列调整 token 级监督权重。
4. 作者将这一缺陷称为"时间系数分配差距"（temporal coefficient allocation gap），即监督权重分配未考虑路径依赖。
5. 因此，问题是如何设计一种序列感知的监督分配机制，使得 token 级权重能够自适应地响应散度的动态演化，从而更高效地利用 OPSD 中已经计算出的分布信息，且不增加额外计算开销。

Q2: 有哪些相关研究？

相关研究主要包括以下几个方向：
1. RLVR 方法：如 DAPO 等开源系统，使用可验证奖励进行强化学习，改进数学与代码推理（从参考文献片段可看出 DAPO 是该领域的重要工作，被作者引用）。
2. On-policy self-distillation（OPSD）：通过教师模型提供密集 token 级监督，相关方法试图改进监督来源或施加预定义的位置权重。例如，有些方法使用更细粒度的奖励或更精确的教师信号，另一些方法则根据 token 位置（如开头或结尾）预设不同权重。
3. 与 DASH 的定位差异：这些方法要么改进监督来源，要么使用预定义的位置加权，而 DASH 关注的是局部蒸馏信号在 rollout 中如何被聚合，即监督的"时间聚合方式"。DASH 利用序列相对散度差距构造自适应传播门，而不是固定位置权重。
4. 相关工作还包括对稀疏奖励和分布失配的研究，以及如何设计路径相关的目标函数。作者强调 vanilla OPSD 的权重是均匀的，而 DASH 引入了路径相关的序列级目标，其有效监督窗口会随实际散度序列自适应调整。

Q3: 论文如何解决这个问题？

DASH 的核心机制如下：
1. 整体框架：在 on-policy self-distillation 中，学生模型生成序列，教师模型为每个 token 提供分布监督。标准 OPSD 对所有 token 的局部蒸馏信号统一加权，而 DASH 将这些信号聚合方式改为序列感知的、路径依赖的自适应聚合。
2. 自适应传播门（Adaptive Propagation Gate）：DASH 将每个局部蒸馏信号与该序列的均值之间的差距（gap）映射为一个标量门（gate）。这个门的作用是控制反向多步聚合（backward multi-step aggregation）的传播强度。
3. 反向多步聚合：与仅使用单步局部信号不同，DASH 利用门控机制将多个时间步的监督信号按路径聚合，使得每个 token 的有效监督权重不仅取决于当前位置的散度，还取决于整个散度序列的演化轨迹。
4. 权重自适应：当某个局部散度明显高于或低于序列平均水平时，门会相应调整，从而在散度变化剧烈的阶段施加更大影响，在平稳阶段减弱影响。这样，监督权重不再一成不变，而是跟随散度演化的实际过程动态变化。
5. 计算效率：DASH 重用了 OPSD 已经计算出的教师和学生分布，只是改变了聚合方式，因此不引入额外的教师或学生前向传播，计算开销与 vanilla OPSD 基本持平。
6. 训练目标：最终形成一个路径相关的序列级目标函数，其"有效监督窗口"（effective supervision horizon）随实现的散度序列自适应调整。

Q4: 论文做了哪些实验？

实验设计如下：
1. 任务与数据集：在三个数学推理基准上评估（具体基准名称在检索证据中未明确，但摘要和实验设置片段提到三个 benchmark，合理推断为常见的数学推理数据集，如 GSM8K、MATH 等，但需要回原文确认）。
2. 模型规模：覆盖三个模型尺度（具体规模未在证据中给出，合理推断为小、中、大不同参数量）。
3. 评估指标：每个问题采样多个响应后取平均（片段提及 "responses per problem and then averages over all problems"），并额外报告三个基准的未加权平均值（unweighted mean）。
4. Baselines：总共对比七个基线和基础模型（Base 表示未经过额外训练的原始模型）。具体基线列表未在证据中完整呈现，但推测包括 vanilla OPSD、可能还有 RLVR 变体、以及其他自蒸馏方法。
5. 对照设置：为了公平比较，作者对 vanilla OPSD 使用“matched reruns”，即在与 DASH 相同的训练设置下重新运行 vanilla OPSD，以排除实现差异带来的影响。
6. 消融与鲁棒性：从摘要看，主要结果是在所有基准和所有规模上 DASH 均优于 matched vanilla OPSD reruns。

Q5: 发现了什么实验现象？

根据摘要和检索证据，主要实验现象包括：
1. 一致提升：DASH 在三个数学推理基准上、所有三个模型规模下，均比匹配的 vanilla OPSD 重运行表现更好，说明该方法具有一致且稳健的正向效果。
2. 无额外开销：由于 DASH 复用了已有的教师和学生分布，性能提升没有伴随额外的前向计算成本，这在实际部署中具有吸引力。
3. 未报告细节：摘要中没有给出具体提升幅度、消融实验或失败案例，因此无法判断提升大小是否显著、是否在所有难度或任务类型上都有增益，也无法了解在哪些情况下可能失效。
4. 路径敏感性的间接证据：作者发现标准 OPSD 分配均匀权重忽视了时间演化，DASH 的改进有效，这间接证明了路径依赖的监督对推理训练的重要性。
5. 注意：这些观察基于摘要和有限片段，具体数值和详细分析需要阅读原文。

Q6: 有什么可以进一步探索的点？

基于本文思路，可以进一步探索的方向包括：
1. 扩展到其他领域：将 DASH 应用到代码生成、数学竞赛、科学推理等更多可验证奖励的任务，检验其泛化性。
2. 结合更复杂的门控机制：目前的传播门是基于局部散度与序列均值的差距，未来可探索更丰富的时间上下文特征（如梯度方向、散度变化率、注意力模式）来设计门控。
3. 理论分析：深入分析路径依赖权重对收敛性、方差和信噪比的影响，建立理论保证。
4. 在更弱或更强的教师模型下的行为：教师质量如何影响 DASH 的效果，以及是否可以通过自适应方式选择教师。
5. 结合多步奖励或过程奖励：将 DASH 与过程奖励模型结合，实现更细粒度的监督。
6. 大规模系统验证：在更大规模模型和更长推理链上验证 DASH 的收益与计算开销。
7. 与其他 RLVR 方法（如 DAPO 等）的集成：探索 DASH 作为通用聚合模块在不同 RLVR 框架中的兼容性。

Q7: 总结一下论文的主要内容

本文针对可验证奖励强化学习（RLVR）中奖励信号稀疏的问题，研究 on-policy self-distillation（OPSD）中密集 token 级监督的有效分配方式。核心观察是标准 OPSD 对所有局部散度赋予相同系数，忽略了生成过程中散度演化的时间结构。由于相同的局部散度可能出现在不同的散度历史之后，反映教师与学生失配的不同演化路径，标准的单点标量无法区分这些时间上下文，导致监督权重无法自适应。为此，作者提出 Divergence-Adaptive Supervision Horizons（DASH），将每个局部蒸馏信号与序列级均值的差距映射为自适应传播门，并利用这些门控制反向多步聚合，使 token 级监督权重随局部散度的演化动态调整。这一方法路径依赖地调整了监督量，并形成序列级目标函数。在三个数学推理基准、三个模型规模上的实验表明，DASH 在每个基准和每个规模上都优于匹配的 vanilla OPSD 重运行，且不增加额外前向计算。本文的贡献包括：识别了标准 OPSD 中的时间系数分配差距；提出了序列感知的自适应监督聚合方法 DASH；通过多基准多规模的实验验证了其有效性。局限在于：实验仅限于数学推理任务，未提供理论分析，也没有报告详细的消融和失败案例。整体而言，这是一项针对自蒸馏训练监督分配的系统性改进工作，与生成、强化学习等方向相关。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文主题属于大语言模型生成与后训练，与用户画像中的生成方向（权重 0.1）直接相关。

## 基本信息

- 作者：ZhiYan Hou, Xinyu Tang, Hongyan An, Jianjin Zhang, Weizhen Wang, Yunyun Han, Gengsheng Li, Xiangzhao Hao, Haiyun Guo, Wenbin Hu, Jinqiao Wang, Yafeng Deng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.06243v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，主要基于摘要、引言、结论和实验设置片段进行归纳推断。
