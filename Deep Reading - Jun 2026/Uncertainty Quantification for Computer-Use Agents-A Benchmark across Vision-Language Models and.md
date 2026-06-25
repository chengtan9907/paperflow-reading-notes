---
user_id: "cheng tan"
paper_id: 1391
arxiv_id: "2606.25760v1"
title: "Uncertainty Quantification for Computer-Use Agents: A Benchmark across Vision-Language Models and GUI Grounding Datasets"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25760v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25760v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T16:09:53"
---
# Uncertainty Quantification for Computer-Use Agents: A Benchmark across Vision-Language Models and GUI Grounding Datasets

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：uncertainty quantification · computer-use agent · GUI grounding · vision-language model

## 一句话总结

提出ARGUS基准，系统评估跨不同模型、数据集和接口的后验不确定性量化方法在GUI点击任务中的性能，揭示UQ方法排名的选择性迁移规律。

## 摘要

> Computer-use agents turn vision-language model (VLM) predictions into executable GUI clicks, so reliable uncertainty estimates are essential for rejection, calibration, miss-severity ranking, and spatial safety regions. Yet evidence on post-hoc uncertainty quantification (UQ) for these agents is fragmented across isolated model and dataset pairs, leaving it unclear whether UQ method rankings stay stable when the agent, benchmark, or observable interface changes. We present ARGUS, a cross-regime benchmark for post-hoc UQ in single-step executable GUI grounding, covering a 27-method, seven-family open-weight matrix over 4 GUI-grounding VLM agents and 4 datasets, plus an 8-method API-compatible closed-source matrix across 3 frontier vendors where logits, hidden states, and attention maps are unavailable. The evaluated methods span logit-based scores, sampling and consistency measures such as semantic entropy and self-consistency, hidden-state and density estimators such as Mahalanobis and SAPLMA, attention-based scores, P(True) and verbalised-confidence prompting, and split-conformal prediction. The main finding is selective transfer: UQ rankings are stable across datasets for a fixed model, but degrade across model classes and observable interfaces. Hidden-state and density methods form the most stable open-weight family, while CoCoA-1MCA, Focus, sampling-based scores, and verbalised self-assessment win in specific regimes. Ranking transfer is strongest within a fixed model across datasets, reaching Spearman $\rho = 0.969$ and averaging $\rho = 0.705$ over 120 open-weight pairs. In contrast, cross-tier transfer to closed-source vendors is much weaker: Spearman $\rho$ averages only $+0.08$ over 12 vendor×dataset pairs on the shared 8-method intersection, so closed-source UQ recommendations should be reranked on the target rather than extrapolated. Model-class transitions further reshape UQ preferences: attention, verbalised, and VLM-native families lose AUROC on every dataset, while density methods remain stable; a scale-only baseline shows that logit-family degradation on SCREENSPOT-PRO is partly confounded by scale rather than grounding fine-tuning alone. Finally, conformal click regions show that score-level discrimination is not enough for deployment: locally weighted disks can shrink radii by $40 - 60\%$ when the plug-in UQ is calibrated, but coverage can degrade under calibration-test or interface mismatch. We release per-item records, calibration/test splits, UQ scores, and analysis scripts as a reproducible basis for regime-aware UQ selection in GUI agents.

Q1: 这篇论文试图解决什么问题？

计算机使用代理将VLM预测转化为GUI点击，需要可靠的不确定性估计以支持拒绝、校准、错误严重性排序和空间安全。现有后验UQ证据碎片化，每个研究仅在孤立模型-数据集对上评估少量方法，不清楚UQ方法排名在更改代理模型、基准数据集或可观测接口时是否稳定。缺乏一个统一的、跨体制的基准来系统比较不同UQ方法在不同条件下的泛化能力。

Q2: 有哪些相关研究？

相关工作包括：(1) 计算机使用代理和GUI定位：SCREENSPOT-PRO、SCREENSPOT-V2、OSWORLD-G/Jedi等基准用于单步GUI定位；(2) 不确定性量化：包括基于logits的分数（如最大softmax概率、熵）、采样一致性（如语义熵、自一致性）、隐藏状态密度（如马氏距离、SAPLMA）、注意力分数、verbalized confidence提示（如P(True)）、保形预测以及CoCoA、Focus等专门方法；(3) 选择性预测：研究工作在分类和生成任务中定义UQ信号，但未系统比较在不同GUI代理、数据集和接口下的排名泛化。

Q3: 论文如何解决这个问题？

本文构建了跨体制的后验UQ基准ARGUS。一个体制由（数据集、模型类、可观测接口）三元组定义。开放权重面板评估27种UQ方法（7个家族），覆盖4个GUI定位VLM代理（包括基于不同后端的模型）和4个数据集。闭源面板评估8种UQ方法（与开放权重面板共享部分方法），覆盖3个前沿供应商的API，其中logits、隐藏状态和注意力图不可用，因此仅使用基于采样的分数、verbalized confidence和保形预测。方法包括：logit-based分数（MSP、熵）、采样一致性（语义熵、自一致性）、隐藏状态密度（马氏距离、SAPLMA）、注意力分数（如平均注意力、attention entropy）、verbalized prompts（P(True)、direct confidence）、保形预测以及CoCoA-1MCA、Focus和VLM-native UQ家族。评估指标包括AUROC、Spearman相关系数等，用于测量排名一致性。

Q4: 论文做了哪些实验？

开放权重面板：在4个VLM代理（如ScreenSpot-V2、OSWorld-G等使用的模型）和4个数据集上评估27种UQ方法。闭源面板：在3个闭源供应商（如GPT-4V、Claude、Gemini等）和4个数据集上评估8种共享方法。每个体制中，计算每种UQ方法的性能，并计算不同体制间的Spearman秩相关系数以评估排名迁移情况。总共生成120个开放权重配对和12个闭源配对。

Q5: 发现了什么实验现象？

主要发现：(1) 选择性迁移：UQ排名在固定模型下跨数据集高度稳定（Spearman ρ=0.969，平均ρ=0.705），但跨模型类或跨接口时稳定性下降。(2) 隐藏状态和密度方法（如马氏距离、SAPLMA）在开放权重面板中是最稳定的UQ家族。(3) CoCoA-1MCA、Focus、基于采样的分数和verbalized自评估在特定领域（如某些模型-数据集组合）表现最佳。(4) 跨层级迁移到闭源供应商很弱：在共享的8种方法上，12个供应商×数据集对的平均Spearman ρ仅为+0.08，因此闭源UQ建议应在目标上重新排序而非外推。(5) 模型类转变重塑UQ偏好：注意力、verbalized和VLM-native家族在模型类转变时AUROC下降显著。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：(1) 将单步UQ扩展到多步交互式计算机使用任务；(2) 在线自适应UQ方法，适应不断变化的界面分布；(3) 对隐藏状态密度方法进行理论分析，解释其迁移鲁棒性；(4) 探索闭源模型的UQ信号，如基于对话的verbalized confidence和思维链提示；(5) 结合保形预测提供校准的预测集合；(6) 将UQ用于主动学习和故障检测的闭环系统。

Q7: 总结一下论文的主要内容

本文针对计算机使用代理中VLM执行GUI点击任务的不确定性量化问题，提出了一个名为ARGUS的跨体制基准。该基准系统比较了27种开放权重和8种闭源的后验UQ方法在4个VLM代理和4个GUI定位数据集上的性能。主要贡献包括：(1) 构建了第一个跨体制UQ评估框架，定义了体制由数据集、模型类和可观测接口构成；(2) 全面评估了包括logit-based、采样一致性、隐藏状态密度、注意力、verbalized和保形预测在内的多种UQ方法族；(3) 揭示UQ排名选择性迁移的规律：在固定模型下跨数据集高度稳定，但跨模型类和跨接口时显著下降；(4) 为闭源模型提供UQ方法选择建议，强调需要重新排序而非外推。实验表明，隐藏状态和密度方法在开放权重模型中泛化最好，但闭源场景下verbalized自评估和保形预测更实用。该工作为构建可靠、可迁移的UQ系统提供了重要指导。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与计算机使用代理和GUI定位任务高度相关，直接服务于VLM部署中的不确定性管理和安全控制

## 基本信息

- 作者：Divake Kumar, Sina Tayebati, Devashri Naik, Amanda Sofie Rios, Nilesh Ahuja, Omesh Tickoo, Ranganath Krishnan, Amit Ranjan Trivedi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL, cs.CV
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25760v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据中的摘要、引言、相关工作和基准描述部分。
