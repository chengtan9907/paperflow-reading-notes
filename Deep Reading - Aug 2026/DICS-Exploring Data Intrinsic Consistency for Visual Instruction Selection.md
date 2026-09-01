---
user_id: "cheng tan"
paper_id: 9997
arxiv_id: "2608.30209v1"
title: "DICS: Exploring Data Intrinsic Consistency for Visual Instruction Selection"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30209v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30209v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:41:59"
---
# DICS: Exploring Data Intrinsic Consistency for Visual Instruction Selection

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual instruction tuning · data selection · multimodal learning · self-scoring metric

## 一句话总结

本文提出数据内在一致性（DIC）自评分指标及其自适应选择方法DICS，通过量化视觉指令样本内部成分间的一致性，在多种数据规模和模型架构上实现了超越全量微调的数据选择效果。

## 摘要

> Visual instruction tuning is crucial for advancing the vision-language alignment and instruction-following capabilities of Vision-Language Models (VLMs). However, identifying optimal subsets under a fixed ratio constraint from rapidly expanding datasets remains a significant bottleneck. While existing methods largely depend on distribution diversity or heuristic filtering, they often overlook the internal coherence within individual samples. To bridge this gap, we propose Data Intrinsic Consistency (DIC), a self-scoring metric designed to quantify the sample-level inter-component consistency. DIC consists of two modules: Visual Information Consistency (VIC), evaluating the alignment between visual content and instructions, and Response Information Consistency (RIC), assessing response coherence relative to the instruction. Building upon DIC, we introduce Data Intrinsic Consistency Selection (DICS), an adaptive data selection method that optimizes the trade-off between high intra-sample consistency and global distributional diversity under varying data budgets. Extensive experiments demonstrate that DICS consistently outperforms state-of-the-art methods across diverse dataset scales and model architectures, surpassing full-dataset fine-tuning while using only 25% of the LLaVA-1.5-665K data. We further curate DICS-6M, a 6M-sample multi-modal instruction corpus that enables the largest-scale visual instruction selection study to date; remarkably, DICS reaches 94.52\% of the official InternVL3-8B-Instruct performance using less than 25\% of its reported training data. Code can be seen at https://github.com/cqu-student/DICS

Q1: 这篇论文试图解决什么问题？

本文聚焦视觉指令数据选择问题。具体而言，视觉指令微调需要从大规模、快速膨胀的多模态指令数据集中，在固定的数据预算（如数据量比例）约束下挑选出最优子集，以训练出性能尽可能高的视觉语言模型（VLM）。该问题的核心挑战在于：1）数据规模巨大，如何高效且有效地评估每个样本的质量；2）现有方法大多基于分布多样性（如聚类、核心集选择）或启发式过滤（如基于规则、外部模型打分），但这些方法忽略了样本内部结构的一致性——即图像、指令、响应三者之间的内在对齐质量。作者认为，高质量视觉指令数据必须满足样本内部各组件间的高一致性。但这一性质在以往工作中未被显式建模。此外，已有针对纯语言数据的数据选择方法（如Li et al., 2024b）难以直接迁移到多模态场景，因为它们缺乏评估视觉信息对质量影响的机制。因此，本文试图填补这一空白，提出一种能捕捉样本内在一致性的自评分指标，并基于该指标设计自适应数据选择算法，在固定预算下同时保证样本质量和整体多样性。

Q2: 有哪些相关研究？

本文相关研究主要涵盖以下几类：1）视觉指令微调及其数据重要性：视觉指令微调（如Ouyang et al., 2022; Cui et al., 2023; Chiang et al., 2023）已成为VLM训练的关键环节，通过大规模指令数据增强视觉-语言对齐和指令跟随能力。已有工作表明数据质量对微调效果影响显著，但如何系统性选择高质量子集仍是开放问题。2）数据选择方法：主流方法包括基于多样性（如核心集选择、聚类、图覆盖）、基于启发式规则（如长度、复杂度过滤）以及基于外部模型打分的样本级过滤。这些方法或只考虑全局分布，或只考虑单方向模态对齐，难以在百万级数据上扩展，且普遍忽略样本内部多组件间的一致性。3）语言模型中的数据选择：在纯语言设置中已有一些工作尝试通过内部一致性或loss差异来筛选数据，但正如本文指出，这些方法由于缺乏视觉信息评估机制，不适用于多模态场景。4）VLM中的幻觉与质量问题：低一致性样本（如指令与图像不匹配）容易诱导模型产生幻觉，这也从侧面说明了样本内部一致性的重要性。本文工作区别于上述各方向，首次将样本内部一致性作为数据选择的核心准则，并与多样性有机结合。

Q3: 论文如何解决这个问题？

本文方法分为两部分：1）定义数据内在一致性（DIC）指标。DIC建立在视觉指令数据样本 (I, x, y) 三个组件（图像I、指令x、响应y）之上，通过自评分框架统一视觉信息一致性（VIC）和响应信息一致性（RIC）。VIC评估视觉内容与指令之间的对齐程度；RIC评估响应相对指令的连贯性。核心思想是：通过对比在部分指令与完整指令场景下的训练loss差异，来衡量每个组件对整体一致性的贡献，从而得到无需外部监督的样本级分数。直觉上，低VIC样本表现出指令与图像弱对齐，容易导致模型幻觉；低RIC样本则提供与指令明显不匹配的响应。2）提出数据内在一致性选择（DICS）方法。DICS在给定数据预算下，自适应地结合高样本内一致性（即DIC分数高）与全局分布多样性。它根据不同数据规模动态调节两者权重，在数据预算较小时更依赖多样性以保证覆盖，在预算较大时更依赖一致性以去噪。最终通过一种高效的选择策略，从大规模数据集中选出子集，实现性能最大化。该框架无需外部LLM或额外标注，能够扩展到百万级甚至更大规模数据。

Q4: 论文做了哪些实验？

论文报告了多项实验，涵盖不同的数据集规模和模型架构。具体包括：1）在LLaVA-1.5-665K数据集上进行子集选择实验，固定数据比例（如25%），对比全量微调与DICS选择子集的微调效果；结果显示DICS在仅用25%数据时即可超越全量微调性能。2）在多个数据集规模和多种模型架构（如不同VLM骨干）上对比DICS与现有最先进(SOTA)的数据选择方法，验证一致性和稳健性。3）构建DICS-6M，一个包含600万样本的多模态指令语料库，并使用DICS进行数据选择，训练InternVL3-8B-Instruct，与官方报告的训练结果对比。结果表明，DICS使用不到官方25%的训练数据，即可达到94.52%的官方性能，这是迄今最大规模的视觉指令选择研究。实验设计还包含对不同选择预算、不同方法组件（如VIC和RIC各自的作用）的系统分析（合理推断，因摘要和片段未详细展开）。总体而言，实验覆盖了从纯语言到多模态、从数十万到数百万数据规模、从通用VLM到最新8B量级模型的广泛场景。

Q5: 发现了什么实验现象？

论文观察到以下关键现象：1）低VIC样本的特征：弱对齐的指令与图像会诱导VLM产生幻觉，说明视觉-指令一致性对抑制幻觉至关重要。2）低RIC样本的特征：响应与指令明显不匹配的样本会干扰模型学习正确指令跟随，降低微调质量。3）数据选择规模与性能的关系：在固定预算下，优先选择高DIC样本能有效提升微调效果，且DICS在25%数据下即可超越全量，说明数据中存在大量冗余或低质量样本，删除它们不仅不损害性能反而提升泛化。4）随着数据规模从665K扩展到6M，DICS依然保持有效，表明方法具有良好扩展性。5）该方法在不同模型架构上表现一致，说明样本内在一致性是一种模型无关的数据质量信号。6）与仅使用多样性或仅使用一致性的方法相比，DICS通过自适应平衡两者取得了更优结果（合理推断，基于摘要中“优化权衡”的表述）。这些观察揭示了视觉指令数据质量中“内部一致性”的重要性，也说明大规模指令数据中噪声和冗余比例较高。

Q6: 有什么可以进一步探索的点？

基于本文工作，未来可在以下方向深入探索：1）将DIC扩展到其他多模态数据形式，如视频、音频指令数据，验证其通用性；2）研究DIC与更多下游任务（如具身智能、智能体规划）的关联，提升数据选择在复杂任务中的效益；3）将DIC与其他数据质量维度（如信息量、难度、多样性）结合，构建更全面的多目标数据选择框架；4）探索DIC的在线版本，在训练过程中动态更新数据选择策略；5）减少DIC计算开销，例如通过代理模型或采样估计，使其能应用于更大规模（如数十亿）的数据集；6）从理论上分析内部一致性与模型泛化误差之间的关系，为数据选择提供更坚实的理论指导。这些方向在论文中未明确列出，属于合理推测。

Q7: 总结一下论文的主要内容

本文针对视觉指令数据选择问题，提出了数据内在一致性（DIC）这一样本级自评分指标，以及相应的自适应选择方法DICS。首先，作者指出现有数据选择方法（基于多样性或启发式过滤）大多忽略样本内部组件间的连贯性，而高质量指令数据应具有图像-指令-响应之间的一致关系。因此，DIC包含两个模块：视觉信息一致性（VIC）和响应信息一致性（RIC），通过对比部分与完整指令下的训练损失差异，对每个样本进行自评分，无需外部监督。然后，DICS依据DIC分数并自适应结合全局分布多样性，在不同的数据预算下进行选择。大量实验表明，DICS在多种数据集规模和模型架构上优于现有SOTA，仅用25%的LLaVA-1.5-665K数据即可超过全量微调性能。此外，作者构建了DICS-6M百万级语料库，并使用DICS在其中选择数据训练InternVL3-8B-Instruct，取得了接近官方全量训练94.52%的性能，验证了方法在超大规模数据上的有效性。主要贡献包括：提出DIC自评分指标；提出DICS数据选择方法；构建DICS-6M大型视觉指令语料库；以及在多个基准上的全面实验验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：这篇论文属于数据选择与数据质量评估方向，与智能体训练中数据筛选、指令数据优化有直接关联。

## 基本信息

- 作者：Yuyang Hong, Jinhui Guo, Jiaqi Gu, Lubin Fan, Ruixiang Wang, Kun Ding, Yue Wu, Shiming Xiang, Jieping Ye
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30209v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的摘要和少量正文片段，并结合论文元数据与heuristic_draft进行推断，部分细节基于合理推断。
