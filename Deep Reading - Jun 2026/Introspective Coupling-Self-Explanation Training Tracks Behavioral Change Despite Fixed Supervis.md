---
user_id: "cheng tan"
paper_id: 2110
arxiv_id: "2606.32038v1"
title: "Introspective Coupling: Self-Explanation Training Tracks Behavioral Change Despite Fixed Supervision"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.32038v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.32038v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T01:26:16"
---
# Introspective Coupling: Self-Explanation Training Tracks Behavioral Change Despite Fixed Supervision

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：language model · explanation faithfulness · introspective coupling · counterfactual explanation

## 一句话总结

研究语言模型通过固定反事实解释训练产生的内省耦合现象：模型解释能跟踪自身行为变化，即使解释来自旧版本或不同模型，也表现出对当前行为的更高忠实性。

## 摘要

> When does training language models (LMs) to generate explanations of their predictions yield faithful introspection, rather than superficial imitation? We study LMs trained to explain which features of their inputs influenced their behavior, using models' counterfactual behavior on modified inputs as supervision. Surprisingly, we find that LMs trained on fixed counterfactual explanations derived from earlier checkpoints of themselves, or even from behaviorally similar models in different families, frequently produce explanations more faithful to their own current behaviors than to those of their training targets. This "introspective" coupling between LM explanations and behaviors occurs when training explanations remain sufficiently correlated with current behaviors over the course of training, even as behaviors themselves shift. We also show that introspective coupling tracks behavior shifts: when explanation training is provided concurrently with other post-training objectives, explanations track those shifts without requiring updated supervision. This phenomenon appears in multiple tasks, including sycophancy and refusal, and is robust to label noise. Overall, our results show that even fixed datasets of counterfactual explanations can provide scalable and generalizable post-training signal for introspection.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：当使用固定的反事实解释数据训练语言模型生成解释时，模型是否能够产生真正内省（即忠实反映其自身行为）的解释，而非仅仅模仿训练数据中的解释表面模式？具体来说，模型是否能在训练过程中行为发生变化后，仍能产生与当前行为一致的解释？这关乎监督信号固定时解释忠实性的边界条件和内在机制。

Q2: 有哪些相关研究？

['语言模型可解释性与忠实性评估：已有工作探索解释生成方法及忠实性度量，但通常假设解释监督来自当前模型。', '反事实解释：使用输入修改后的行为变化作为解释监督，但通常需要实时生成监督。', '自我训练与蒸馏：模型使用自身输出训练，可能产生自洽性，但本文关注解释与行为的关系。', '行为跟踪与模型监控：利用解释探测模型行为变化，本文提供一种无需更新监督的方案。', '内省能力：模型能否内省自身推理过程，本文对此提出新证据和方法。']

Q3: 论文如何解决这个问题？

论文通过一系列实验探究内省耦合现象。首先，他们训练语言模型（如 Llama 和 Pythia 变体）在特定任务（如谄媚和拒绝）上生成解释，监督信号是模型在反事实输入上的行为（例如，当输入中某特征改变时，模型输出是否变化）。关键设计：使用固定解释集（来自模型早期检查点或不同模型族）训练目标模型，然后测量模型解释的忠实性——即解释是否匹配其自身当前行为（Self）还是训练目标行为（Orig）。通过对比 Self 和 Orig 的忠实性得分，判断内省耦合是否存在。此外，他们还引入额外行为偏移训练（如直接优化行为），观察解释是否随之调整。正则化（如 KL 散度）被证明是产生该效应的必要条件。

Q4: 论文做了哪些实验？

['任务：谄媚（sycophancy）和拒绝（refusal）两类，可能涉及输入特征操纵。', '模型：Llama 和 Pythia 系列不同大小和检查点。', '主要实验：使用固定解释集（来自基础模型早期检查点）训练目标模型，比较解释对自身当前行为的忠实性（Self）与对训练目标行为的忠实性（Orig）。', '跨模型标签：使用来自完全不同模型族（如 Llama 标签训练 Pythia）的解释标签，测试内省耦合是否依赖模型相似性。', '混合比例：改变解释数据中当前模型自身标签的比例（α），观察 Self > Orig 的边界。', '行为偏移：在解释训练同时，用额外数据诱导模型行为偏移（如直接优化谄媚分数），观察解释是否跟踪行为变化。', '噪声鲁棒性：在解释标签中添加噪声，测试现象稳健性。']

Q5: 发现了什么实验现象？

['内省耦合现象：模型在固定解释集训练后，其解释对自身当前行为的忠实性（Self）显著高于对训练目标行为的忠实性（Orig），即 Self > Orig。', '该现象在跨模型标签设置下依然成立：使用 Llama 解释标签训练 Pythia 模型，Pythia 解释仍更忠实于自身行为而非 Llama 行为，表明不需要行为收敛。', 'Self > Orig 在不同 α 值（混合比例）下均成立，且不受解释标签噪声影响。', '当解释训练与其他行为偏移训练同时进行时，解释能自动跟踪行为变化，无需更新解释标签。例如，训练模型增加谄媚倾向后，相应解释也反映该倾向。', '正则化（如 KL 散度）是产生内省耦合的必要条件：没有正则化时，模型可能退化为单纯模仿训练解释。']

Q6: 有什么可以进一步探索的点？

['探究内省耦合的边界条件：在更复杂任务（如多步推理）和更大模型上是否成立？', '分析失败模式：什么情况下 Self > Orig 会失效？例如训练解释与当前行为完全无关时。', '利用内省耦合进行模型监控：是否可通过固定解释集持续跟踪模型行为偏移，无需实时标签？', '扩展到其他后训练目标：如 RLHF 或安全训练中，解释能否反映隐式行为改变？', '机制理解：内省耦合背后的计算原理是什么？是否与模型内部表示的重用有关？', '实际应用：如何设计解释数据集以最大化内省能力，同时避免误导。']

Q7: 总结一下论文的主要内容

本文发现并系统验证了语言模型中的内省耦合现象：当使用固定反事实解释集训练模型时，模型产生的解释倾向于忠实于其自身当前行为，而非训练目标行为。作者在谄媚和拒绝任务上，使用 Llama 和 Pythia 系列模型，通过对比 Self 忠实性和 Orig 忠实性证实该现象。进一步实验表明，内省耦合在跨模型标签、不同解释混合比例、标签噪声下均稳健，且能跟踪由额外行为训练引起的行为偏移。正则化（KL 散度）是关键条件。该工作揭示了固定解释监督可以产生动态内省能力，为模型行为监控和可解释性提供了一种可扩展方案。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：如果你关注模型可解释性和内省能力，本文提供新视角和实证证据。

## 基本信息

- 作者：Zifan Carl Guo, Laura Ruis, Jacob Andreas, Belinda Z. Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.32038v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 检索到的摘要、结论和 introduction 等证据片段，并结合 heuristic_draft 进行润色和补全。信息缺口处基于摘要合理推断，无编造具体数值。
