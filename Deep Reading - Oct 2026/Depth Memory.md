---
user_id: "cheng tan"
paper_id: 16103
arxiv_id: "2610.18809v1"
title: "Depth Memory"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18809v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18809v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:07:56"
---
# Depth Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：depth mixing · linear associative memory · recursive least squares · transformer architecture

## 一句话总结

本文提出 Depth Memory (DM) 统一框架，将 Transformer 沿深度方向的信息传播建模为跨层的存储与检索问题，并据此提出 Linear Depth Memory (LDM) 架构——用递归最小二乘线性回归实现深度方向的信息检索，在 400M 和 1.5B 规模上一致取得最低困惑度。

## 摘要

> Residual connections propagate information through a network by additively ac-
> cumulating layer outputs. Recently proposed alternatives, such as Manifold-
> Constrained Hyper-Connections (mHC) and Attention Residuals (AttnRes), in-2026 stead use more expressive depth aggregation mechanisms that improve language
> modeling performance at scale. In this work, we consider the mixing of repre-
> sentations over depth as a problem of storing and retrieving information across
> layers. Formalizing this perspective, we introduce Depth Memory (DM), a uni-Oct
> fied framework that describes the standard pre-layer-norm (PreLN) transformer,
> mHC, and AttnRes in terms of streams that carry representations through the net-13 work. One such stream, for example, could be an explicit memory stream that
> is queried to enrich the input to every layer, and whose output is subsequently
> written back into the memory for use by later layers. For AttnRes, this memory
> stream is a growing cache of explicit layer representations. For mHC, it is an
> expanded residual stream. Building on this view, we propose Linear Depth Mem-
> ory (LDM), which formulates information retrieval across depth as a regression[cs.LG] problem solved using recursive least squares. The resulting architecture propa-
> gates three streams across depth: the standard residual stream, a linear associative
> memory stream, and an inverse covariance stream. For high-throughput training
> and inference, we propose a hardware-aware implementation of LDM that reduces
> peak activation memory and memory traffic. LDM consistently achieves the low-
> est perplexities among PreLN, mHC and AttnRes baselines at the 400M and 1.5B
> parameter scales, alongside competitive zero-shot accuracy.
> Code Checkpoints

Q1: 这篇论文试图解决什么问题？

本文针对的核心问题是：Transformer 沿深度方向的信息传播机制长期未变，标准做法是 ResNet 式残差连接，即把每层输出按固定权重加法累积到残差流中。这种逐层等权加法的深度混合机制在表达力上有根本限制：对于序列维度（token mixing）和通道维度（channel mixing）已有大量创新（softmax/linear attention、SSM、gated MLP、MoE），但深度维度（layer mixing）的机制创新相对滞后。近期工作（mHC、AttnRes）虽提出更丰富的深度聚合方式并验证了规模化收益，但缺少统一理论框架来解释它们为什么有效、彼此之间是什么关系。本文尝试回答：(1) 能否把深度方向信息传播统一建模为存储与检索（memory read/write）问题？(2) 哪些现有架构（PreLN、mHC、AttnRes）是这个框架的特例？(3) 在此框架下能否设计出兼有表达力、训练稳定性和硬件效率的新深度混合机制？AttnRes 存在 cache 无限增长和计算开销问题，mHC 使用扩展残差流而缺乏按需检索的机制；此外，具有记忆机制的深度混合在 400M/1.5B 规模、50B/100B token 预算下是否依然优于简单残差加法和近期 baseline，是本文用实验检验的核心问题。

Q2: 有哪些相关研究？

相关工作沿三个维度展开。第一，深度混合机制：ResNet 的 additively residual connection 是基线；Hyper-Connections (HC, Zhu et al. 2025) 与 Manifold-Constrained Hyper-Connections (mHC, Xie et al. 2026) 维护扩展残差流（expanded residual stream），用超连接系数混合层输入输出；Attention Residuals (AttnRes, Kimi Team et al. 2026b) 显式让每一层注意力 attend 到之前层的输出，per-layer 可学习加权，分 Full AttnRes 与 Block AttnRes 两种变体，Block 版本用约八个 depth block 聚合深度表示以控制成本；Kimi Team et al. 2026a 与 DeepSeek-AI et al. 2026 在大规模验证了残差替代方案。第二，记忆与线性联想记忆：Schlag et al. (2021) 的线性注意力 / 联想记忆工作为 LDM 提供理论基础，快速权重（fast weights）与 Hebbian 式写读操作是 LDM 的建模来源；递归最小二乘（RLS）在信号处理与在线学习中有长期历史，这里用于求解跨层线性回归检索问题。第三，序列与通道混合：softmax attention (Vaswani et al. 2023)、linear attention (Katharopoulos et al. 2020)、state-space models (Gu et al. 2022)、gated MLP (Shazeer 2020)、MoE (Fedus et al. 2022) 构成 Transformer 架构搜索的两个主要轴，本文把 depth mixing 提为与 sequence/channel mixing 并列的第三轴并以此定位自己的工作。Training 方面采用 AdamW、cosine decay 等标准配方，评测用 lm-eval-harness (Gao et al. 2024) 和 FineWeb-Edu (Penedo et al. 2024)。

Q3: 论文如何解决这个问题？

论文分两层提出方案。第一层是统一框架 Depth Memory (DM)：把跨层表示传播建模为带显式记忆的流系统。状态 S_l = (x_l, M_l)，其中 x_l 是标准 PreLN residual stream，M_l 是矩阵值隐藏状态（记忆）。每层执行 Read 与 Write：Read((x_{l-1}, M_{l-1}), q_{l-1}) 用当前层 query 从记忆 M_{l-1} 检索历史层表示，检索结果与残差流共同构成层输入；Write 把层输出（或经变换后的表示）写回记忆 M_l 供后续层使用。在此框架下：PreLN 是记忆为空的特例；AttnRes 的记忆流是显式层表示 cache（key/value 为历史层输出，query 为当前层表示，attention 选择性加权历史层），Full 版每层 attend 所有历史层，Block 版按 depth block 聚合；mHC 的记忆流是 expanded residual stream，通过超连接系数进行读写混合。第二层是新架构 Linear Depth Memory (LDM)：把 Read 操作具体化为线性回归问题——用递归最小二乘（RLS）求解从记忆表示到目标表示的线性映射，使当前层能从压缩记忆中检索与 query 最匹配的历史信息。LDM 传播三条流：标准残差流、线性联想记忆流（matrix-valued hidden state M_l）和逆协方差流（RLS 所需的 inverse covariance matrix）。实现上：query 和 key 从隐藏表示生成，维度 H=32（400M）/H=64（1.5B），per-group value 维度等于注意力头维度，G=16 组；使用 state unwinding 与 windowed（窗口化）机制控制记忆规模与计算成本；作者发现固定 α=1 优于可学习 α，因此主配置固定 α=1。作者还提出硬件感知实现，通过降低峰值激活内存和内存流量支持高吞吐训练推理。

Q4: 论文做了哪些实验？

实验设计分三部分。主实验：在 FineWeb-Edu 上预训练两个规模模型——400M Llama-like 在 50B token 上、1.5B Qwen-like 在 100B token 上；baseline 为 PreLN 标准 Transformer、mHC、AttnRes（400M 训练 Full 与 Block 两种，1.5B 由于 Full AttnRes 训练成本高只训练 Block AttnRes，Block 深度块数 400M 用 8、1.5B 用 7）；所有模型使用 AdamW（weight decay 0.1）、线性 warmup 加 cosine decay 到峰值学习率的 10%；LDM 参数开销比 PreLN 多约 1%，与 mHC 相当；主配置 D_k=8，另在 400M 评估 D_k=4。评测指标为验证困惑度（Val，pretraining validation perplexity）和 lm-eval-harness 零样本基准：ARC-E、ARC-C、HellaSwag、PIQA、WinoGrande、OBQA、Lambada 及平均准确率。消融实验：在 400M 规模、15B FineWeb-Edu token、峰值学习率 1e-3 下（其余设置同主训练配方）消融 LDM 的主要设计选择。扩展实验：Appendix D.3 在 400M 上做极端长宽比（aspect ratio）扫描，进一步检验 LDM 对较浅配置的偏好；困惑度单调下降。

Q5: 发现了什么实验现象？

主结果表显示：在 400M/50B 设置下，LDM 验证困惑度 12.08，优于 PreLN 12.73、AttnRes Full 12.35、AttnRes Block 12.12、mHC 12.23 和 LDM D_k=4 的 12.14（此处 LDM 12.08 与 12.14 均为表中值，注意表中 LDM D_k=4 与 LDM Ours 均为 12 水平，12.08 为最低）。零样本平均准确率 LDM 为 50.46，与 AttnRes Block 50.61 接近，高于 PreLN 49.05、mHC 49.55。在 1.5B/100B 设置下，LDM 验证困惑度 9.15，低于 PreLN 9.48、AttnRes Block 9.26、mHC 9.25；零样本平均 59.71，低于 PreLN 58.41 和 mHC 59.68、AttnRes Block 59.42。总体趋势：困惑度收益一致且随规模增大保持，但零样本精度提升相对温和，个别任务（如 400M 下 ARC-C）出现 LDM 低于部分 baseline 的情况，说明困惑度与下游准确率不完全同步。消融实验揭示：固定 α=1 优于可学习 α，这是作者在主配置中固定 α 的原因；困惑度随配置变化单调下降，LDM 在 400M 上偏好较浅配置（aspect ratio 扫描支持该结论）。D_k=8 优于 D_k=4，说明记忆宽度/检索容量增加带来困惑度改善。LDM 参数开销约 1%，与 mHC 相当，低于具有完整 cache 的 Full AttnRes 的后续成本，验证了硬件感知实现的价值。文献中未明确披露的数值缺口：单任务层面个别准确率与困惑度之间存在张力，例如 ARC-C 上 400M LDM 40 分左右水平有波动，需回原文核对。

Q6: 有什么可以进一步探索的点？

可从以下方向进一步探索：(1) 更大规模验证：本文最高到 1.5B/100B token，LDM 在 7B/70B 乃至 MoE 架构上的 scaling 行为未知；Kimi/DeepSeek 已把深度混合推到更大规模，LDM 能否保持优势需要实验。(2) 记忆机制的理论分析：RLS 记忆的容量、遗忘机制与层数/序列长度的关系，windowed 机制对长上下文记忆覆盖范围的影响值得深究。(3) 与序列混合的交互：LDM 的记忆流与 attention/SSM 的时间维度记忆如何交互，能否统一 depth 与 sequence 两个方向的记忆。(4) 硬件协同设计：state unwinding 与 windowed 实现可以进一步针对长序列、流水线并行和专家并行优化；峰值激活内存与内存流量的更细粒度 profiling 与 kernel 融合。(5) 多模态与科学应用：用户关注 agent 与 AI-for-science，可探索 LDM 深度记忆作为 agent 长期记忆接口（把跨层联想记忆用作可检索的模型内记忆），以及在科学数据（如分子、蛋白质序列）上代替标准深度混合是否带来收益。(6) 训练动态与初始化：α=1 固定优于可学习的反直觉现象值得理论解释；RLS 逆协方差流初始化、数值稳定性与精度（fp32/fp8）对训练稳定性的影响。(7) 与其他深度混合机制组合：DM 框架预测 mHC 与 AttnRes 是不同记忆流实现，二者与 LDM 是否可混合形成多流架构。(8) 下游能力：当前零样本提升温和，需评估指令微调、代码、数学与长上下文任务上 LDM 的真实收益边界。

Q7: 总结一下论文的主要内容

本文围绕一个相对被忽视的架构轴——深度方向的表示混合（depth mixing）——展开。开头指出 Transformer 架构创新集中在序列混合（attention/linear attention/SSM）与通道混合（gated MLP/MoE），而深度传播一直停留在 ResNet 式加法残差。近期 mHC 与 AttnRes 在规模化语言建模中证明更丰富的深度聚合有效，但缺乏统一理解。作者提出 Depth Memory (DM) 框架：把跨层传播看成流系统，其中可以存在标准残差流之外的显式记忆流；每层用 query 从记忆读取历史表示来丰富输入，并把当前层表示写回记忆。这个框架把 PreLN（无记忆）、AttnRes（记忆是增长中的显式层表示缓存，可被 attention 选择性读取）、mHC（记忆是扩展残差流，按超连接系数读写）统一为不同记忆实现。基于此视角，作者设计 Linear Depth Memory (LDM)：将深度方向的读取形式化为线性联想记忆上的 RLS 回归检索，沿深度维护三条流——残差流、矩阵值联想记忆流、逆协方差流。相比 AttnRes 的显式缓存，LDM 的记忆是压缩的线性映射；相比 mHC，LDM 的读取是 query 驱动的按需检索而非固定系数混合。作者还提出 state unwinding 与 windowed 等硬件感知实现降低峰值激活内存与流量。实验在 400M Llama-like/50B token 与 1.5B Qwen-like/100B token 上对比 PreLN、mHC、AttnRes(Full/Block) 与 LDM(D_k=8/D_k=4)。LDM 在验证困惑度上两个规模均最优（400M: 12.08 vs 最优 baseline 12.12；1.5B: 9.15 vs 最优 baseline 9.25），零样本平均准确率有竞争力（400M 50.46、1.5B 59.71，即在 1.5B 平均略高）。消融与附录的 aspect ratio 扫描支持设计选择（固定 α=1、D_k=8、浅层偏好）。总体论证逻辑是：先统一解释既有方法，再推出新机制，再用匹配配置的预训练实验验证困惑度与零样本能力，最后给出硬件效率实现。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：如果把 agent 视为需要长期记忆与检索的序列决策系统，LDM 的深度联想记忆（可查询、可写回、压缩的层表示记忆）提供了一种模型内部记忆接口的思路，可与外部检索/记忆系统对照。

## 基本信息

- 作者：David González-Martínez, Philipp Nazari, T. Konstantin Rusch, Shiwei Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18809v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Introduction/Method/Results 证据片段，并结合启发式草稿和检索片段做了交叉验证；表格精确数值存在排版歧义，已在相应字段标注需回原文核对。
