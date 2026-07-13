---
user_id: "cheng tan"
paper_id: 3414
arxiv_id: "2607.09657"
title: "Scalable Visual Pretraining for Language Intelligence"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.09657.pdf"
pdf_url: "https://arxiv.org/pdf/2607.09657"
abs_url: "https://arxiv.org/abs/2607.09657"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-14T00:27:37"
---
# Scalable Visual Pretraining for Language Intelligence

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual pretraining · language intelligence · scientific documents · autoregressive latent prediction

## 一句话总结

本文挑战语言模型必须仅从文本预训练的假设，提出视觉预训练（VP）框架，直接在科学文档图像上进行自回归潜在预测预训练，实验证明其在科学推理上超越文本预训练，且增益随数据量增加而扩展。

## 摘要

> The rapid progress of large foundation models has been driven predominantly by pretraining on large-scale text corpora. However, many forms of knowledge are conveyed through visual representations, where figures, typeset equations, and page layouts carry rich information that cannot be faithfully or completely captured by text alone. Yet current pretraining approaches discard these visual cues by converting visually rich sources, such as documents and web pages, into plain text for learning language intelligence. This paper challenges the default assumption that language models must be trained on text-only representations and shows that Visual Pretraining is a scalable learner for foundation model intelligence. To this end, we conduct a systematic study of unsupervised visual pretraining paradigms that directly leverage visual documents without text extraction. Across multiple backbones and benchmarks, visual pretraining on the same underlying corpora consistently outperforms text-only pretraining, offering an efficient pathway to scalable language intelligence.

Q1: 这篇论文试图解决什么问题？

当前语言模型预训练仅依赖文本，忽略了科学文档中丰富的视觉信息（如公式、图表、排版）。这些信息在转文本时丢失，导致模型无法捕捉原本以视觉形式表达的知识。论文核心问题是：能否通过视觉预训练直接利用视觉文档来提升语言智能，并验证其可扩展性。

Q2: 有哪些相关研究？

相关研究可分为三类：\n- **多模态预训练**（如CLIP、DALL·E）需要大量图像-文本配对数据，侧重于视觉-语言对齐。\n- **文档理解模型**（如LayoutLM、Pix2Struct）直接处理文档图像，但优化目标多为解析或生成文本，而非通用语言智能。\n- **视觉语言模型**（如Flamingo、BLIP-2）融合视觉编码器与语言模型，但依赖配对数据且通常滞后于纯文本模型。\n本文不同：提出无监督自回归视觉潜在预测，仅使用图像，不需要配对文本，目标是通过视觉学习增强语言能力。

Q3: 论文如何解决这个问题？

论文提出Visual Pretraining (VP) 框架，包括：\n1. **输入**：将科学文档页面渲染为图像，不提取文本。\n2. **视觉编码**：使用Vision Transformer (ViT) 将图像分割为固定大小patches并编码为潜在表示。\n3. **预训练目标**：自回归预测每个patch在潜在空间中的表示，即根据已见patches预测下一个patch的潜在特征。\n4. **模型架构**：基于Transformer（推测为decoder-only），在潜在序列上进行因果预测。\n5. **训练**：在大规模科学文档图像集上预训练，控制与文本预训练相同的底层语料。\n6. **下游使用**：预训练后的模型可直接用于科学推理任务（如问答、数学解题），或通过轻量微调适配。

Q4: 论文做了哪些实验？

论文设计了对比实验，比较视觉预训练（VP）与文本预训练（TP）在科学推理上的表现。\n- **语料**：使用同一科学文档语料库（如arXiv论文），分别以图像和纯文本形式预训练。\n- **骨干**：不同规模的Transformer模型（推测为LLaMA风格）。\n- **基准**：多个科学推理评测（如科学问答、数学解题、图表理解），具体名称未在检索证据中给出。\n- **结果**：1) VP在大部分任务上优于TP。2) 随预训练数据量增加，VP的收益保持甚至扩大。3) 在视觉信息密集的样本（含图表、公式）上，VP增益最大。4) 跨模态对齐能力提升，无需配对监督。\n- **消融**：虽未详细提及，但有分析不同预测目标的影响（如潜在空间vs像素空间）。

Q5: 发现了什么实验现象？

实验揭示以下现象：\n- VP在科学推理任务上显著优于同等语料的TP。\n- 预训练数据量扩展时，VP性能持续提升，未出现饱和。\n- 视觉密集样本（如表格、复杂公式）的增益更大，说明VP能捕获文本被丢弃的信息。\n- 跨模态对齐（如将图像内容与文本概念对应）在无配对监督下自然改善。\n- 推测：视觉预训练可能提供了更强的正则化或更丰富的梯度信号。

Q6: 有什么可以进一步探索的点？

论文指出多个开放方向：\n1. **协调视觉与文本解码**：进一步研究如何联合优化视觉潜在预测与文本生成，使两者互补。\n2. **数据压缩与扩展**：视觉语料可压缩性更高，探索基于高度压缩视觉语料库的超大规模预训练。\n3. **多样视觉来源**：除科学文档外，推广到图表、手写、示意图等。\n4. **更优编码器**：设计针对文档影像的视觉编码器，提升效率。\n5. **多任务学习**：将VP融入多任务框架，同时学习语言和视觉表示。

Q7: 总结一下论文的主要内容

本文系统研究了视觉预训练（VP）作为语言智能基础模型的可扩展学习范式。当前的主流语言模型预训练仅依赖文本，但科学知识大量以视觉形式（公式、图表、排版）呈现，这些信息在转文本过程中丢失。作者挑战了“语言模型必须从文本学习”的默认假设，提出直接在文档图像上进行无监督视觉预训练。核心方法是对渲染的文档页面使用Vision Transformer编码成patch序列，然后以自回归方式预测每个patch在潜在空间中的表示。这一目标的优势在于无需配对文本，并且深度整合了视觉潜在信息。通过控制变量实验（同一语料的视觉vs文本表示），作者发现VP在多个科学推理基准上持续优于文本预训练（TP）。更重要的是，VP的性能随预训练数据量增加而提升，表现出良好的可扩展性。在定性分析中，视觉密集样本（如含复杂公式或图表的题目）的增益最为显著，说明模型从视觉布局中捕获了关键线索。此外，VP还自然提升了跨模态对齐能力。尽管本文仅聚焦科学文档，且视觉潜在解码与文本生成间的协调仍有待优化，但其结果为视觉信息在预训练中的价值提供了有力证据，开辟了一种绕过文本提取直接使用视觉语料库的预训练路径。未来工作可探索更大规模的视觉预训练、更高效的编码器以及更细致的多模态协同。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接挑战当前主流预训练范式，对多模态学习和文档理解具有启发意义。

## 基本信息

- 作者：Yiming Zhang, Zhonghan Zhao, Wenwei Zhang, Haiteng Zhao, Tianyang Lin, Yunhua Zhou, Demin Song, Kuikun Liu, Haochen Ye, Haian Huang, Yuzhe Gu, Haijun Lv, Qipeng Guo, Bin Liu, Gaoang Wang, Kai Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI, cs.MM
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.09657`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成已参考PDF语义检索命中的证据片段，并结合摘要、引言、实验结果和讨论部分信息。
