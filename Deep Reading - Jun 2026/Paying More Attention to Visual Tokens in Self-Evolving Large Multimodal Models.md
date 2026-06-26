---
user_id: "cheng tan"
paper_id: 1634
arxiv_id: "2606.27373"
title: "Paying More Attention to Visual Tokens in Self-Evolving Large Multimodal Models"
publish_date: "2026-06-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.27373.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27373"
abs_url: "https://arxiv.org/abs/2606.27373"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-26T14:26:04"
---
# Paying More Attention to Visual Tokens in Self-Evolving Large Multimodal Models

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-evolving large multimodal models · visual under-conditioning · visual invariance · unsupervised learning

## 一句话总结

VISE是一种无监督自演进框架，通过几何和语义不变性奖励直接正则化多模态模型的视觉注意力，显著提升图像描述和VQA性能。

## 摘要

> Recently, self-evolving large multimodal models (LMMs) have received attention for improving visual reasoning in a purely unsupervised setting. However, multi-role self-play and self-consistency reward schemes in existing self-evolving LMMs optimize answer agreement without ensuring the decoder attends to visual content, relying instead on statistical language priors to produce self-consistent outputs. This leads to a persistent failure mode we term visual under-conditioning, where the decoder relies on language priors rather than the image during generation, manifesting as insufficient attention to visual tokens. As a result, current self-evolving LMMs struggle on vision-language understanding tasks such as image captioning and visual question answering. To address this, we propose VISE (Visual Invariance Self-Evolution), a purely unsupervised self-evolving framework that directly regularizes the model's visual conditioning policy through two complementary invariance-based rewards: a geometric invariance reward that enforces spatial consistency under known transformations, and a semantic invariance reward that penalizes evidence-agnostic generation by requiring the model to recognize the absence of evidence when predicted regions are perturbed. VISE operates within a single model without specialist roles, external reward models, or annotations, and is trained on raw unlabeled images. Experiments on 18 benchmarks demonstrate the efficacy of our approach. Using Qwen3-VL-2B as the base model, VISE achieves gains of +16.85 CIDEr on COCO and +19.66 CIDEr on TextCaps, reduces object hallucination by 5.0 Chair-I points, and generalizes across multiple model families and scales.
> Performance delta relative to the base model across captioning and reasoning benchmarks (Qwen3-VL-2B)
> ![](images/54ee446fae70a652d34f73116c364f112458be657547cb80b2bd881f5d1790fe.jpg)
> ![](images/fa93d7c7ca92bad1d0abbef730122383a82a4e9c21330fb1472cbecaa0eb92b9.jpg)
> Project Page : mbzuai-oryx.github.io/VISE/
> GitHub Code : github.com/mbzuai-oryx/VISE
> HuggingFace Model : huggingface.co/shravvvv/VISE

Q1: 这篇论文试图解决什么问题？

最近，自演进大型多模态模型（LMMs）在无监督环境下改善视觉推理方面受到关注。然而，现有方法（如多角色自博弈和自一致性奖励机制）优化的是答案一致性，并未确保解码器真正关注视觉内容，导致模型依赖统计语言先验生成自一致输出，而非依据图像信息。这种“视觉欠条件化”现象表现为解码器对视觉token的注意力不足。因此，当前自演进LMMs在需要精细视觉条件的任务（如图像描述、视觉问答）上表现不佳，特别是在区域级描述和物体细节捕获方面。论文通过实验验证了这一假说：即使模型在推理基准上有所提升，但在描述任务上差强人意。

Q2: 有哪些相关研究？

自演进LMMs领域已有若干工作：①多角色自博弈方法（如proposer-solver、questioner-reasoner角色），通过角色间交互提升推理一致性；②自一致性奖励机制，优化输出与自身或其他角色的吻合度；③基于强化学习的无监督微调方法。但这些方法均未显式调节视觉注意力。此外，物体幻觉减轻方法（如Chair-I指标优化）常依赖外部奖励或标注数据。基线方法VisionZero-RW通过奖励微调降低了物体幻觉（Chair-I -2.99），但牺牲了其他能力。VISE与之不同，直接针对视觉条件化进行正则化，无需额外角色或奖励模型。

Q3: 论文如何解决这个问题？

VISE（Visual Invariance Self-Evolution）是一个完全无监督的自演进框架，在单一模型内通过两个互补的不变性奖励直接正则化解码器的视觉条件化策略：①**几何不变性奖励**：在空间变换（如平移、缩放）前后，要求视觉特征保持一致性，确保解码器关注稳定的空间视觉信息；②**语义不变性奖励**：随机扰动图像中的预测区域（如遮挡部分），要求解码器识别“证据缺失”，从而惩罚仅依赖语言先验的无证据生成。VISE在原始未标注图像上训练，无需专门角色、外部奖励模型或人类标注。训练过程使用同一个模型，交替执行视觉编码和文本解码，通过奖励信号更新参数。该方法可扩展至不同模型家族和规模。

Q4: 论文做了哪些实验？

论文在18个基准上进行了全面评估，包括COCO Caption、TextCaps、NoCaps、Flickr30k等描述任务，以及VQAv2、OK-VQA、ScienceQA等视觉问答和推理任务。基础模型为Qwen3-VL-2B。对比基线包括基础模型（untuned）、其他自演进方法（如VisionZero-RW）以及监督微调方法。评估指标包括CIDEr、SPICE（描述质量）、Chair-I（物体幻觉）以及准确率等。此外，还在不同模型家族（如Qwen系列、其他架构）上测试泛化能力。实验设置了消融研究验证每个不变性奖励的贡献。

Q5: 发现了什么实验现象？

①在描述任务上，VISE显著提升：COCO CIDEr +16.85，TextCaps CIDEr +19.66，NoCaps上也取得增益。②物体幻觉方面，Chair-I降低5.0点，表明句子级幻觉减少。但物体存在二元可靠性减弱（即判断物体是否存在的准确性下降），提示改善不一致。③推理基准上也有提升，但增益小于描述任务。④消融实验显示，两个奖励联合使用效果优于单独使用，语义不变性奖励对减少幻觉贡献更大。⑤泛化实验表明VISE适用于不同规模和家族的模型（如Qwen3-VL-2B、7B等），且无需重新设计。⑥与基线VisionZero-RW相比，VISE在描述和幻觉平衡上更优（VisionZero-RW虽然Chair-I -2.99但描述CIDEr可能下降）。

Q6: 有什么可以进一步探索的点？

①解决物体存在可靠性下降的问题，可能通过联合优化二元存在检测或引入互补约束。②探索更丰富的几何变换（如透视、仿射）和语义扰动策略。③将VISE扩展到视频理解、指代表达等更复杂视觉任务。④研究视觉条件化正则化与大规模预训练的协同作用。⑤分析自演进过程中视觉注意力分布的动态变化。⑥结合其他模态（如音频、文本）进行多模态统一正则化。⑦在更大规模模型和数据集上验证。

Q7: 总结一下论文的主要内容

论文聚焦自演进大型多模态模型（LMMs）中的视觉欠条件化问题，指出现有方法优化答案一致性但忽略视觉注意力，导致模型依赖语言先验。为此，作者提出VISE——一种完全无监督的自演进框架，通过两个不变性奖励直接正则化视觉条件化：几何不变性奖励保证空间一致性，语义不变性奖励惩罚无证据生成。VISE在单一模型内训练，无需外部奖励或标注。在18个基准上的实验表明，以Qwen3-VL-2B为基础，VISE在COCO Caption上CIDEr提升16.85，TextCaps上提升19.66，物体幻觉（Chair-I）降低5.0，并在多个模型家族中展现泛化性。但局限性在于物体存在可靠性有所下降。论文论证了直接增加视觉token注意力对自演进LMMs的必要性和充分性。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：该论文与用户的生成方向直接相关（权重0.10），特别是图像描述生成任务。

## 基本信息

- 作者：Shravan Venkatraman, Ritesh Thawkar, Omkar Thawakar, Rao Muhammad Anwer, Hisham Cholakkal, Salman Khan, Fahad Khan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-06-26
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.27373`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（Abstract、Introduction、Experiments、Conclusion片段），并基于启发式草稿进行润色和补全。
