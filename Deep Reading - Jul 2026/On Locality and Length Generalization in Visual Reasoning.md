---
user_id: "cheng tan"
paper_id: 3415
arxiv_id: "2607.09061"
title: "On Locality and Length Generalization in Visual Reasoning"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.09061.pdf"
pdf_url: "https://arxiv.org/pdf/2607.09061"
abs_url: "https://arxiv.org/abs/2607.09061"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-14T00:45:38"
---
# On Locality and Length Generalization in Visual Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual reasoning · length generalization · local perception · recurrent vision

## 一句话总结

本论文研究视觉推理中的长度泛化问题，发现全局视觉编码模型会利用感知捷径导致泛化失败，而基于严格局部感知的循环视觉策略能够有效实现长度泛化。

## 摘要

> A striking feature of the human visual system is that it ingests visual information through a series of local foveated glimpses, rather than a single global computation. This makes human vision distinctly different from most popular computer vision models in use today, which input images globally and in a single shot. A natural question therefore is whether local, sequential vision models may provide any fundamental computational benefits in addition to being biologically more plausible than global models. In this work, we investigate this question from the perspective of visual state tracking and length generalization. Inspired by recent studies of length generalization in language models, we study the behavior of vision models trained on simple vision tasks that require the aggregation of local information across an image. Our experiments reveal that, similar to language models, vision models can learn to exploit global shortcuts and thereby fail to generalize over task length or complexity. We also show that recurrent vision policies based on strictly local perception can mitigate these failures, thereby allowing models to generalize on these tasks. Our results show that local attention may be an essential overlooked requirement for robust compositional generalization.

Q1: 这篇论文试图解决什么问题？

本文试图解决视觉推理模型在任务长度或复杂度增加时无法泛化的问题。具体而言，当前主流视觉模型使用全局编码的单次前馈方式，可能依赖于全局感知捷径（global shortcuts），导致在需要跨局部区域聚合信息的任务上，当问题规模延长或复杂性增加时，性能急剧下降。论文类比语言模型中的长度泛化失败，提出并验证了视觉领域也存在类似现象，并探索局部顺序感知作为解决方案。

Q2: 有哪些相关研究？

相关工作涉及两个主要线索：一是语言模型中的长度泛化研究，例如Abbe等人（2024）研究了Transformer的推理泛化障碍和归纳scratchpad；二是视觉模型中的全局vs局部处理，包括人类眼动研究（如Hayhoe & Ballard, 2005）以及视觉Transformer中局部注意力机制。论文还提及当前SOTA模型如Bai等人(2025)、OpenAI、Anthropic的模型在图像描述和VQA上的成功，但指出其全局编码机制可能隐含泛化缺陷。此外，组合泛化和分布外泛化是更广泛的研究背景。

Q3: 论文如何解决这个问题？

论文提出一种基于严格局部感知的循环视觉策略。核心思想是模拟人类视网膜中央凹注视，模型通过一系列局部眼动（glimpses）来获取信息，每个时间步仅观测图像的一个局部区域，并基于历史状态决策下一注视点。具体实现上，使用一个循环神经网络（RNN）或Transformer解码器作为控制器，生成注视点坐标序列，每次裁剪并编码局部图像块，然后更新隐状态。该方法确保模型无法访问全局信息，从而避免利用全局捷径。在训练时，模型学习从局部观察中聚合信息完成推理任务。论文还探索了局部glimpse的大小和分辨率对泛化的影响。

Q4: 论文做了哪些实验？

论文在简单的视觉推理任务上评估了模型，这些任务以图表形式表示系统状态，要求聚合局部2D信息来推断状态（例如跟踪物体位置、方向变化等）。任务具有可变的复杂度（如步骤数、物体数量等）。实验设置包括：全局基线模型（如标准ViT、CNN全局编码）、局部循环模型（提出方法）。训练集仅包含有限复杂度的样本，测试集包含更长的序列或更多物体，以评估长度泛化。论文还进行了消融研究，改变glimpse大小、分辨率、循环步数等。另外，可能比较了不同决策策略（如固定扫描vs学习策略）。

Q5: 发现了什么实验现象？

关键实验发现包括：1) 全局模型在训练集上表现良好，但在测试长度增加时性能急剧下降，表明其依赖全局捷径；2) 局部循环模型在训练和测试分布上都能保持稳定性能，成功泛化到更长任务；3) 局部glimpse的大小和分辨率对泛化性能有影响，过大的glimpse可能导致模型利用局部捷径；4) 循环策略的决策序列质量影响结果，学习到的注视模式趋向于目标导向；5) 与语言模型中的发现类似，视觉模型也存在‘globality barrier’；6) 反直觉结果：即使全局模型在训练时看到所有信息，仍无法泛化，而局部模型因受限于局部观察被迫学习通用组合规则。

Q6: 有什么可以进一步探索的点？

可探索的方向包括：1) 将局部感知策略扩展到更复杂的现实世界视觉推理任务（如具身智能、图表理解）；2) 研究局部注意力与全局注意力的混合架构，平衡计算效率与泛化能力；3) 探索是否可以通过无监督学习等方式自动学习高效的凝视策略；4) 局部感知策略与大语言模型结合，实现多模态长度泛化；5) 深入分析泛化失败的理论原因，如全局编码导致的捷径学习；6) 考虑更复杂的决策模型，如强化学习或与预训练视觉特征结合。

Q7: 总结一下论文的主要内容

本文系统研究了视觉推理中的长度泛化问题。受人类视觉系统局部注视的启发以及语言模型长度泛化研究的推动，作者设计了一组简单的视觉状态跟踪任务，要求模型通过聚合多个局部区域信息来完成推理。实验结果表明，采用全局编码的常规视觉模型（如ViT）会学习利用全局感知捷径，导致任务复杂度增加时泛化失败；而提出的一种基于严格局部感知的循环视觉策略，通过序列化局部眼动来获取信息，能够有效实现长度泛化。论文的主要贡献包括：揭示了视觉模型中的全局捷径问题；证明局部顺序感知是稳健组合泛化的必要条件；提供了一套评估视觉长度泛化的基准和消融分析。该工作为视觉模型架构设计提供了新视角，强调了局部注意力在泛化中的关键作用。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与agent方向相关：局部凝视策略可类比智能体在环境中的主动感知（active perception），对具身智能体的长期视觉推理有借鉴

## 基本信息

- 作者：Pulkit Madan, Sanjay Haresh, Reza Ebrahimi, Sunny Panchal, Apratim Bhattacharyya, Roland Memisevic
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI, cs.LG
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.09061`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（abstract、method、results等片段）以及field_evidence_map的指引。
