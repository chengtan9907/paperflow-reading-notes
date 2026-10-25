---
user_id: "cheng tan"
paper_id: 16192
arxiv_id: "2610.18638v1"
title: "To Align or Not To Align: Check Your COMPASS Before You Train"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18638v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18638v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:12:00"
---
# To Align or Not To Align: Check Your COMPASS Before You Train

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：model compatibility · representation alignment · model merging · k-nn overlap

## 一句话总结

提出COMPASS（COMPatibility ASsessment Score），一个理论驱动的诊断工具，通过k-NN重叠和训练范式上限两个轴，在alignment或merging之前估计模型对的兼容性，并在72个跨模态对齐和392个模型合并实验中验证其预测与下游成功强相关。

## 摘要

> Combining pretrained models is a common strategy for building AI systems, whether by aligning a vision encoder with a language model or merging fine-tuned checkpoints. However, finding compatible pairs typically requires costly empirical search, as compatibility is known only after alignment or merging. We propose COMPASS (COMPatibility ASsessment Score), a theoretically motivated diagnostic that estimates whether a model pair will be compatible prior to alignment or merging. Building on the observation that representational convergence is most robust at the level of local neighborhoods, COMPASS decomposes model compatibility into two cheap-to-compute axes: a pair-specific ease, defined as the $k$-NN overlap between the two encoders, and a paradigm-level ceiling, capturing the maximum overlap their training paradigms allow. Together, these two axes provide a ranking of candidate model pairs and prescribe how to act on each pair: (i) prioritize pairs that already agree and whose paradigms are compatible, (ii) invest additional compute on pairs with compatible paradigms but low initial agreement, (iii) switch the pretraining paradigm when it cannot support sufficient agreement, or (iv) deprioritize pairs that score poorly on both axes. We apply COMPASS to $72$ cross-modal alignment experiments and $392$ task-arithmetic model merges and show that compatibility estimates by COMPASS strongly correlate with downstream success, achieving average Pearson correlation of $0.91$ on cross-modal alignment tasks and $0.70$ on model merging.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决什么问题？在现代AI系统中，组合预训练模型是核心操作，例如将视觉编码器与语言模型对齐（如CLIP风格）或合并微调检查点（如task arithmetic）。无论哪种情况，最关键的决策是选择哪对模型进行组合。然而，现有方法通常需要昂贵的经验搜索，因为模型对的兼容性只有在对齐或合并之后才能知道。虽然已有针对单个预训练模型微调的迁移性度量（transferability metrics），但模型对兼容性的类似问题通常只能通过昂贵的实验回答。本文提出一个诊断工具，能够从小的探针集排名候选对，每个模型评分一次并跨候选对复用，从而将评估成本从随对数缩放降低到随模型数缩放。具体来说，论文试图回答：在训练之前，如何估计一对预训练模型是否会在对齐或合并后成功？这涉及两个子问题：一是如何定义和度量模型对的兼容性；二是如何将兼容性估计转化为可操作的建议，以指导用户选择、投资或切换训练范式。

Q2: 有哪些相关研究？

相关研究主要围绕几个方向。首先，表示假说方面：Platonic Representation Hypothesis（柏拉图表示假说）认为足够表达力的模型趋向于收敛到共享的全局表示；而Aristotelian Representation Hypothesis（亚里士多德表示假说）则表明一致性主要是局部的——不同目标和数据训练的模型在“谁与谁接近”上达成一致，即使全局几何不同。后者为本文的局部邻域方法提供了理论基础。其次，迁移性度量方面：已有工作提出针对单个预训练模型选择微调的度量（如[12-14]），但模型对兼容性的问题通常被忽略。第三，模型对齐和合并方面：视觉-语言对齐（如[1-6]）和任务算术合并（如[7-10]）是两种典型的组合操作，但选择哪对模型通常依赖经验。本文的方法与这些工作互补，提供训练前的诊断。此外，k-NN重叠作为表示相似性的度量在先前工作中已有使用（如Alshammari等人[23]），本文将其用于模型对兼容性。

Q3: 论文如何解决这个问题？

论文如何解决这个问题？核心是提出COMPASS，一个理论驱动的诊断工具。它基于两个关键观察：一是表示收敛在局部邻域层面最稳健，二是模型对的兼容性可以分解为两个独立的轴。具体而言，COMPASS定义了两个度量：
1. Ease（对特定易用性）：衡量特定模型对在未对齐或合并前是否已经接近，通过两个编码器在相同样本上的k-NN重叠来度量。高ease意味着两个模型已经对局部邻域有相似看法。
2. Ceiling（范式级上限）：衡量两个训练范式允许的最大重叠，即范式本身是否支持强一致性。高ceiling意味着范式有足够空间让一致性涌现。
这两个轴结合形成一个相图，将候选对分为四类：值得立即使用（ease高、ceiling高）、预算受限（ease低、ceiling高，需要额外计算）、上限受限（ease高、ceiling低，范式不支持）、不太可能成功（两者都低）。理论方面，论文建立了每样本起始条件重叠与有限时间收敛之间的联系（定理4.1），证明起始条件重叠越高，在固定训练预算内越容易使对兼容。实践上，COMPASS只需要一个小型探针集，每个模型评分一次，即可跨候选对复用，大幅降低评估成本。

Q4: 论文做了哪些实验？

论文做了哪些实验？作者将COMPASS应用于两个主要场景：
1. 跨模态对齐：共72个视觉-语言对实验。具体任务包括COCO图像到文本检索（I2T R@1）等。评估COMPASS的兼容性估计与下游对齐成功之间的相关性。
2. 任务算术模型合并：共392个模型合并实验。评估平均保留率（mean retention）作为下游成功指标。
实验展示了COMPASS估计与下游成功之间的Pearson相关性，平均在跨模态对齐任务上为0.91，在模型合并上为0.70。此外，论文还通过相图可视化不同对的行为，并分析了不同训练范式（如对比视觉语言模型、判别模型、自蒸馏模型）在不同应用中的表现差异。实验还涉及消融研究，例如排除某些家族或范式后重新计算COMPASS，相关性保持稳定（对齐0.85-0.93，合并0.60-0.72）。

Q5: 发现了什么实验现象？

实验揭示了以下现象：
1. COMPASS的兼容性估计与下游成功强相关，验证了局部邻域一致性作为兼容性信号的有效性。
2. 相图能够区分不同行动类别：有些对立即可用，有些需要更多计算，有些受范式限制，有些则不太可能成功。
3. 同一模型家族在不同应用中表现不同：对比视觉语言模型在跨模态对齐中有利（因为其预训练目标接近下游目标），但在任务算术合并中不利。判别和自蒸馏模型则表现出相反的模式。
4. Figure 2显示每样本起始条件重叠与端点对应，表明起始条件重叠越高，在固定训练预算内越容易使对兼容，这支持了理论保证。
5. 消融实验表明COMPASS的预测对排除某些家族或范式具有鲁棒性，相关性保持较高。
6. 随机顺序的基线（如Tables 8和9）显示COMPASS优于随机选择，但具体数值未在摘要中给出。

Q6: 有什么可以进一步探索的点？

有什么可以进一步探索的点？
1. 将COMPASS扩展到其他模态和任务，例如音频、视频、多模态组合，以及更广泛的迁移学习场景。
2. 更细粒度的连续分数替代相图，以提供更精确的排名和行动建议。
3. 改进理论保证，例如放宽定理4.1中的假设，或推导更紧的有限时间收敛界。
4. 研究不同训练范式对ceiling的影响机制，从而指导预训练范式的选择或设计。
5. 探索更高效的探针集设计，减少计算成本，同时保持估计准确性。
6. 将COMPASS应用于实际系统，例如自动选择模型对进行集成或持续学习。
7. 结合其他兼容性信号（如全局几何、功能相似性）以增强鲁棒性。
8. 在模型合并中，研究不同合并算法（如task arithmetic变体）与COMPASS的交互。

Q7: 总结一下论文的主要内容

本文提出COMPASS，一个在训练前估计模型对兼容性的诊断工具。论证主线：作者从表示收敛的局部性出发，认为模型对是否兼容可以通过局部邻域一致性来预测，而不必等到昂贵的对齐或合并之后。技术主线：COMPASS将兼容性分解为两个轴——ease（特定对的k-NN重叠）和ceiling（训练范式允许的最大重叠），并基于理论分析（定理4.1）证明起始条件重叠控制有限时间收敛。实验主线：在72个跨模态对齐和392个任务算术合并实验中，COMPASS的估计与下游成功强相关（平均Pearson 0.91和0.70），相图提供了可操作的建议。论文还讨论了不同训练范式在不同应用中的适用性，以及相图作为可解释摘要的局限性。总体而言，COMPASS提供了一种低成本、理论驱动的模型对选择方法，有望减少组合预训练模型时的经验搜索成本。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文的方法论设计（任务设定、实验协议、消融分析）值得借鉴，适合系统性工作。

## 基本信息

- 作者：Fabian Gröger, Petar Damjanović, Maria Brbić
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18638v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据片段。
