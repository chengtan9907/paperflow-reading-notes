---
user_id: "cheng tan"
paper_id: 1467
arxiv_id: "2606.25432v1"
title: "Brevity is the Soul of Inference Efficiency: Inducing Concision in VLMs via Data Curation"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25432v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25432v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T18:23:06"
---
# Brevity is the Soul of Inference Efficiency: Inducing Concision in VLMs via Data Curation

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：inference efficiency · vision-language models · data curation · brevity

## 一句话总结

通过预训练数据筛选诱导VLM输出简洁性，在保持精度相近的前提下实现35倍推理成本优势，同时揭示了冗长token的收益随模型规模缩小的规律。

## 摘要

> Inference efficiency is typically pursued by shrinking the model: distillation, pruning, quantization, and sparse routing each lower per-token cost while treating token count as fixed. But output length has been inflating, and it is precisely the component the standard toolkit leaves untouched. Here, we argue that brevity is the missing inference-efficiency lever, and that pretraining data curation is a practical way to pull it: a model trained on concise, correct data learns to answer in fewer tokens; i.e. it has a lower Cost-of-Pass. We apply our VLM curation pipeline to the MAmmoTH-VL single-image subset, and compare models trained on our curated data, the standard MAmmoTH-VL data, and external open-weight frontier VLMs. On a controlled 20-evaluation set and 14 VLMs at 1B-4B activated parameters, we hold output length fixed with a per-model regression, separating brevity from quality, and price models in FLOPs per correct answer. Curation buys a 35x Cost-of-Pass advantage over the most verbose 4B comparator (Qwen3.5-4B) within $\sim$1 pp of accuracy (0.41 vs 14.58 TFLOPs per correct answer; 0.691 vs 0.704 mean accuracy). Curation also buys a +17.55-percentage-point matched-length accuracy gain over the uncurated baseline that grows with model scale (from +16.7 pp at 1B to +21.2 pp at 4B). This brevity improvement concedes no quality: generic verbosity buys no accuracy at any capability or scale, and the window where reasoning-structured verbosity still earns its tokens shrinks from 4 of 8 capability groups at 2B to 1 of 8 at 4B. Per example, the concise model even reaches correct answers the verbose reasoning model misses, marking reasoning as a distinct curation target rather than something brevity gives up. Inference efficiency in this regime is a tokens-per-correct problem, and brevity is the lever that targets it directly.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决视觉语言模型（VLM）推理效率中的一个被忽视的问题：输出长度持续膨胀，而现有的效率优化手段（蒸馏、剪枝、量化等）仅降低每token成本，未触及输出长度本身。因此，即使模型缩小，冗长的输出仍会导致高昂推理成本。论文旨在通过预训练数据筛选诱导模型输出简洁性，从而在不牺牲质量的前提下大幅降低推理成本，并量化简洁性的收益。

Q2: 有哪些相关研究？

相关工作涵盖五个线索：（1）输出长度膨胀的趋势与成本；（2）推理效率的传统方法：模型压缩（蒸馏、量化、稀疏路由）；（3）控制输出长度的尝试：RL侧长度塑造（Singhal et al., 2024; Park et al., 2024）、后处理摘要或两阶段生成；（4）VLM中的冗长问题：衡量与缓解；（5）数据筛选对模型行为的影响。然而，这些研究分散于纯文本对话、推理模型、VLM等领域，缺乏统一的简洁性视角。本文首次系统研究VLM预训练数据筛选对输出简洁性和推理效率的影响。

Q3: 论文如何解决这个问题？

论文设计了一套VLM预训练数据筛选流程，以MAmmoTH-VL单图像子集为起点。具体方法包括：针对每个训练样本（图像+指令+答案），使用质量评估模型（可能是基于正确性和简洁性的评分）筛选出答案正确且表达简洁的样本。筛选标准可能同时考虑答案长度和任务正确性，剔除冗长或多轮重复的样本。仅使用筛选后的子集训练模型，保持其他超参数不变。同时，论文还对比了未筛选基线（原始MAmmoTH-VL数据）以及多个开源VLM（Qwen、InternVL、Isaac系列）。为公平比较简洁性，论文采用线性回归方法控制输出长度，分离出长度对精度的影响。

Q4: 论文做了哪些实验？

论文在20个评测任务组成的评测集上进行了系统实验，涉及14个1B-4B参数的VLM模型，包括：自己训练的筛选模型（不同规模1B/2B/4B）、未筛选基线模型、以及Qwen3.5-4B、InternVL2-2B/4B、Isaac-2B/4B等开源模型。主要实验指标为每次正确回答的FLOPs（Cost-of-Pass）和匹配长度下的精度。通过回归方法固定输出长度，从而单独衡量简洁性带来的精度变化。此外，论文还按能力组（如图表理解、指代定位、字幕生成等8组）分析冗长token的收益，观察不同规模下推理式冗长的有效性。所有模型在相同评测集上评估，使用统一提示格式。

Q5: 发现了什么实验现象？

实验观察到以下关键现象：（1）筛选模型在Cost-of-Pass上具有压倒性优势：以4B规模为例，筛选模型每次正确回答仅需0.41 TFLOPs，而最冗长的Qwen3.5-4B需14.58 TFLOPs（35倍差距），但两者平均精度分别为0.691和0.704，仅差1.3个百分点。（2）匹配长度下，筛选模型精度始终优于未筛选基线，且增益随模型规模增大而增长（1B时+16.7pp，2B时+18.9pp，4B时+21.2pp）。（3）通用冗长（非推理结构）在任何能力和规模下均不提升精度，甚至会下降（如指代定位任务中下降高达54.9pp）。（4）推理式冗长（如链式思考）仅在较小模型的部分能力组中有正收益：在2B规模下，8个能力组中有4组收益为正；在4B规模下仅剩1组（可能为复杂推理）。这暗示推理式冗长的收益随模型能力增强而萎缩。（5）有趣的是，筛选模型在某些个别样本上能够正确回答，而冗长的推理模型却失败，表明简洁性并非牺牲推理能力，而是避免不必要的逻辑噪声。

Q6: 有什么可以进一步探索的点？

论文开辟了多个可探索的方向：（1）将简洁性筛选扩展到其他模态（如视频、纯文本）或混合模态场景；（2）研究更精细的筛选策略，例如基于任务类型动态调整简洁性阈值，或结合强化学习在线优化长度；（3）将简洁性与推理能力深度融合，探索如何在不增加冗长的情况下提升复杂推理能力；（4）研究不同参数量下简洁性筛选的边界效应，尤其是更大模型（>7B）是否仍能从简洁性中获益；（5）开发专门的简洁性评估基准，以更全面衡量模型输出的简洁性和信息密度；（6）将Cost-of-Pass指标纳入模型选择标准，推动实际部署中的效率优化。

Q7: 总结一下论文的主要内容

本文提出通过预训练数据筛选来诱导VLM的简洁性输出，从而提升推理效率。作者首先指出当前效率优化忽略输出长度膨胀的问题，然后设计了一套数据筛选流程，选择简洁且正确的训练样本。在1B-4B参数规模的14个VLM对比中，筛选模型在精度几乎持平的前提下，每次正确回答的FLOPs降低最多35倍。匹配长度下，筛选模型精度相比未筛选基线大幅提升（最高+21.2pp），且增益随规模增大。进一步分析表明，通用冗长无益，推理式冗长仅在有限条件下有效且窗口缩。实验结论支持“推理效率应聚焦于每次正确回答的token成本”的观点，并为通过数据治理实现简洁性提供了实证基础。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：对智能体（agent）场景中推理成本控制有直接参考：简洁性可大幅降低每次决策的token消耗

## 基本信息

- 作者：DatologyAI, :, Matthew L. Leavitt, Siddharth Joshi, Haoli Yin, Rishabh Adiga, Haakon Mongstad, Alvin Deng, David Schwab, Bogdan Gaza, Ari Morcos
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CV
- 日期：2026-06-24
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25432v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本文基于PDF检索证据（retrieved_evidence）生成，优先参考了原文片段，尤其第3.1节和第3.4节的具体数据与分析。
