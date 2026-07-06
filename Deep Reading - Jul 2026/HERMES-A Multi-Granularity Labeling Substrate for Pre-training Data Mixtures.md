---
user_id: "cheng tan"
paper_id: 2542
arxiv_id: "2607.02266"
title: "HERMES: A Multi-Granularity Labeling Substrate for Pre-training Data Mixtures"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02266.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02266"
abs_url: "https://arxiv.org/abs/2607.02266"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-06T13:03:46"
---
# HERMES: A Multi-Granularity Labeling Substrate for Pre-training Data Mixtures

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：data mixing · pre-training · multi-granularity labeling · residual vector quantization

## 一句话总结

HERMES通过学习语义变换和3阶段残差向量量化构建多粒度标签基底，将数据混合设计从固定标签集转向可导航的粒度层次，并揭示了不同粒度下混合规则效果的显著差异。

## 摘要

> Most data-mixing methods assume the corpus has already been partitioned into groups, and the choice of those groups determines what a mixer can express. Existing labels, including provenance, topic or format taxonomies, and flat embedding clusters, commit to one semantic axis at one granularity; changing the resolution rebuilds the labels. We argue the bottleneck is the label system, not the mixer, and provide a hierarchical one. HERMES is a data-derived labeling substrate: a Learned Semantic Transform followed by 3-stage residual vector quantization annotates each document once into a coarse-to-fine code whose prefix length controls granularity up to approximately 130k cells. At coarse granularity HERMES sits at a plateau with KMeans-family methods on standard clustering metrics, so the contribution is the substrate, not the clusterer. On 1B-parameter, 25B-token pre-training, the hierarchy exposes an interaction fixed-granularity pipelines cannot test: at one prefix length, a combined Stage-2 rule contrast, equal-subbucket coverage versus size-proportional within-bucket quality top-30%, lifts a 16-task capability macro-average by +0.0253; at the next finer level, the same rule loses its measurable edge as candidate pools contract approximately 5x. HERMES reframes data mixture design from choosing among fixed label sets to navigating a reusable, data-derived granularity hierarchy.

Q1: 这篇论文试图解决什么问题？

论文试图解决现有数据混合标签系统的根本局限：当前标签（来源、主题/格式分类、平面嵌入聚类）只在一个语义轴和一个粒度上划分语料库，改变分辨率需要重建整个标签体系，无法灵活地支持不同粒度下的混合探索。这导致混合器的设计被标签系统的选择所约束，而真正的瓶颈被认为是标签系统本身。论文旨在提供一个可复用的、多粒度的标签基底，使得数据混合设计可以在一个连续的粒度层次上自由切换，无需重新构建标签。

Q2: 有哪些相关研究？

相关研究主要包括三个方面：(1) 数据混合方法：最近的数据筛选工作（如Penedo等2023、2024）显示数据质量对预训练至关重要，混合方法通常假定已划分好的组，并聚焦于采样器设计（如Group Distributionally Robust Optimization、Min-K%采样等）。(2) 标签系统：现有标签分为三类：来源标签（如网页域名、语料库来源）、主题/格式分类（如话题模型、格式分类器）和平面嵌入聚类（如基于BERT嵌入的KMeans聚类）。这些方法都只提供单一粒度的划分，且改变粒度需要重建。(3) 量化方法：残差向量量化（RVQ）在多模态和表示学习中广泛应用，HERMES将其应用于文档级标签生成，结合学习语义变换实现层次化标注。

Q3: 论文如何解决这个问题？

HERMES由两个主要组件组成：(1) 学习语义变换（Learned Semantic Transform, LST）：将原始文档映射到一个连续的语义嵌入空间。具体地，使用预训练语言模型的embedding加一个可训练的线性变换或小MLP，使得嵌入空间更好地适配后续量化任务。(2) 3阶段残差向量量化（3-stage Residual Vector Quantization, RVQ）：每个阶段使用一个独立的量化码本（codebook），第一阶段学习最粗粒度划分（约256个簇），第二阶段在当前残差上学习中等粒度（约256×256=65536个簇），第三阶段学习最细粒度（总约256^3≈16M个簇，但实际通过受限解码得到约130k个非空单元）。每个文档属于每个阶段的一个码，因此可以通过前缀长度（如L1、L2、L3）控制粒度。训练过程通过端到端的重建目标（如重建文档嵌入）和码本损失来学习LST和码本。注释过程一次完成，之后可通过截断最短前缀获得任意粗粒度标签。

Q4: 论文做了哪些实验？

论文在1B参数、25B token的预训练设置下进行实验，使用FineWeb数据集（约15T tokens的子集，但论文描述为25B tokens的实验）。主要实验包括：(1) 粗粒度（L1, 256簇）下比较不同分组方法：包括KMeans（随机初始化、统一初始化）、GMM、Agglomerative Clustering等，在聚类指标（NMI、ARI等）上HERMES处于平台期（plateau），与KMeans类方法相当。(2) 中粒度（L2, 约65536个候选簇）下对比不同混合规则：一种规则是等尺寸子桶覆盖（equal-subbucket coverage），即每个L1簇下采样固定数量的子桶以保证覆盖；另一种是子桶内按质量排序取前30%（size-proportional within-bucket quality top-30%）。结果显示后一种规则在16个任务（包括HellaSwag、ARC、MMLU等）的宏平均上提升+0.0253。(3) 细粒度（L3, 约130k个候选簇）下重复相同规则对比：发现优势消失，分析原因是候选池缩小约5倍（从L2的约65536到L3的约130k有效簇），导致每个簇内文档数太少，排名不稳定。(4) 消融实验：验证L1分组选择并非性能来源（通过改变分组方法观察后续性能差异不显著），证明贡献来自层次结构本身。

Q5: 发现了什么实验现象？

关键发现包括：(1) 在L1（256簇）粒度，各种聚类方法（KMeans、GMM、Agglomerative）在标准聚类指标上达到相似水平，HERMES并未在聚类质量上超越传统方法，但这是预期中的，因为其贡献在于层次结构而非单一聚类质量。(2) 在L2粒度，采用子桶内质量前30%的采样规则相比等覆盖规则在16任务宏平均上提升0.0253（约为1.5%相对提升），效果显著。(3) 在L3粒度，同一规则的优势消失，宏平均差异几乎为0。进一步分析发现L3的有效非空子桶数约为130k，而L2约为65536，实际候选池规模缩小约5倍（从约65536到约130k），但更关键的是中位子桶内的文档数从L2的约429降至L3的约81，导致基于质量排序的采样不稳定。(4) 论文还观察到一个反直觉现象：更细粒度并不必然带来更好的混合效果，因为细粒度下桶内文档稀疏性削弱了规则的有效性。(5) 消融实验表明，L1分组的具体选择（如KMeans vs. GMM）对后续性能影响很小，证实收益来自层次结构而非底层分组方法。

Q6: 有什么可以进一步探索的点？

论文提出了几个认可的局限和未来方向：(1) 两个旨在强化结论的控制实验尚未进行：“仅大子桶”（only-large-sub-buckets）和“合成缩小”（synthetic-shrinkage）变体，用于更直接地检验机制。(2) 机制证据强度：当前主动子桶缩小（收缩5倍）是直接数量测量，但更严格的因果验证需要上述控制实验。(3) 更深层次的前缀（如L4）可能带来进一步回报递减，且需要更大的语料库以维持桶内文档数。(4) 当数据分布与FineWeb-Edu的右尾分布差异较大时，结果可能有所不同，需要更多分布下的验证。(5) 可以将HERMES与其他标签系统（如主题分类、来源标签）结合，形成复合标签体系。(6) 探索更高效的量化训练或自监督方式，降低LST学习成本。(7) 将HERMES应用于更大规模模型（如7B、70B）和更多领域。

Q7: 总结一下论文的主要内容

本文提出HERMES，一种多粒度标签基底，用于改进预训练数据混合中的标签系统。当前数据混合方法通常假设语料已被划分为固定组，现有标签（来源、主题、格式、嵌入聚类）只在一个语义轴和粒度上操作，改变分辨率需要重建整个标签集。论文认为瓶颈在于标签系统，而非混合器，因此设计了一个数据驱动的层次化标签系统。HERMES包含两个核心组件：学习语义变换（LST）将文档映射到语义空间；3阶段残差向量量化（RVQ）将每个文档编码为一个粗到细的三层代码，通过截断前缀可以获得任意粗粒度的标签。具体地，L1约有256个粗类，L2约有65536个中类，L3有效约130k个细类。训练通过重建嵌入损失和码本损失联合优化。实验在1B参数、25B token的预训练上进行，使用FineWeb数据子集，评估16个自然语言理解任务。在粗粒度L1，HERMES的聚类指标与KMeans类方法相当，表明其优势不在单一聚类质量而在层次性。在L2粒度，对比两种混合规则：等子桶覆盖和按比例取子桶内质量前30%，后者在16任务宏平均上提升0.0253。但在L3粒度，相同规则优势消失，原因是候选子桶缩小约5倍，桶内文档数中位数从约429降至约81，导致基于排名的规则不稳定。消融实验确认L1分组选择不是性能来源。论文通过这些实验揭示了固定粒度管线无法发现的粒度-规则交互：收益依赖于粒度选择，粗粒度下分组方法差异小，中粒度下规则差异显现，细粒度下规则差异消失。HERMES将数据混合设计从选择固定标签集转换为在可复用的粒度层次中导航，为数据混合提供了新的研究范式。论文最后讨论了局限性，包括两个控制实验缺失、机制证据强度有待加强、更深前缀的适用性以及数据分布变化的影响。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文聚焦于预训练数据混合的标签系统设计，对任何涉及大规模预训练的智能体、科学应用等领域的数据筛选具有通用参考价值。

## 基本信息

- 作者：Ziyun Qiao, Yue Min, Ruining Chen, Yujun Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02266`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了PDF检索到的摘要、方法、结果和局限等证据片段，并结合heuristic_draft进行润色和补全。
