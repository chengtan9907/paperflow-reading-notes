---
user_id: "cheng tan"
paper_id: 3623
arxiv_id: "2607.11562v1"
title: "MonkeyOCRv2: A Visual-Text Foundation Model for Document AI"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11562v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11562v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:38:27"
---
# MonkeyOCRv2: A Visual-Text Foundation Model for Document AI

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual foundation model · document ai · document pretraining · multilingual

## 一句话总结

MonkeyOCRv2是一个面向文档AI的视觉-文本预训练基础模型，通过大规模多语言预训练和联合生成-重建策略在多个文档任务上取得显著提升。

## 摘要

> Mainstream visual encoders are pretrained on natural images and cannot be effectively applied to document images without document-oriented adaptation, as dense text and fine-grained character strokes demand character-level visual perception. We present MonkeyOCRv2, a visual-text pretrained model for document AI. First, we construct MonkeyDoc v2, to our knowledge the largest document-image pretraining corpus, comprising 113 million images spanning 17 languages. Second, we propose a pretraining strategy that jointly learns image-to-text generation and pixel-level document reconstruction: the former aligns visual representations with textual content, while the latter preserves character strokes and layout details. Extensive experiments are conducted on five representative document analysis tasks, including text recognition, formula recognition, text detection, document tampering detection, and overlapping text segmentation. Replacing the original encoders with MonkeyOCRv2 consistently improves performance across all five tasks, raising the overall recognition accuracy of CRNN from 58.7% to 67.3% and enabling the 110M UniMERNet-T to outperform the 325M UniMERNet-B. Finally, we validate its effectiveness as the vision encoder of multimodal large language models on the more challenging tasks of document parsing and document understanding. Kept frozen and paired with a lightweight language model, it yields a 0.7B document parsing model that sets a new open-source state-of-the-art on MDPBench, a recent benchmark spanning digital-born and photographed documents across 17 languages, surpassing the previous best 3B dots.mocr by 2.8% absolute with a vision encoder roughly 11× smaller; on OmniDocBench, it further outperforms much larger general-purpose VLMs such as Qwen3-VL-235B and GPT-5.2. The frozen encoder also powers a document understanding model that outperforms counterparts built on CLIP, DINO, and SAM across eight benchmarks under identical training settings. These results suggest that document-oriented visual pretraining can serve as a foundation for document intelligence in its own right. Code and data will be released at https://github.com/Yuliang-Liu/MonkeyOCRv2.

Q1: 这篇论文试图解决什么问题？

主流视觉编码器（如CLIP、DINO）在自然图像上预训练，无法直接适应文档图像，因为文档图像包含密集文本和细粒度字符笔画，需要字符级视觉感知。现有文档AI方法通常使用这些通用编码器，缺乏专门针对文档的视觉预训练，限制了性能。此外，用于文档预训练的大规模高质量数据集稀缺。

Q2: 有哪些相关研究？

相关工作包括：(1)通用视觉预训练模型如CLIP、DINO、SAM，在自然图像上优异但文档任务效果不佳。(2)文档AI专用方法如CRNN、UniMERNet，它们使用通用编码器或基于规则的特征。(3)多模态大语言模型中的视觉编码器，如PaddleOCR-VL系列使用0.6B编码器，MinerU2.5系列使用675M编码器，但这些编码器仍非文档专用。本文工作填补了文档专用视觉编码器的空白。

Q3: 论文如何解决这个问题？

论文提出MonkeyOCRv2，包含两大核心贡献：(1)构建大规模多语言文档预训练语料库MonkeyDoc v2，包含1.13亿张图像，覆盖17种语言，保证多样性和字符覆盖。(2)提出联合预训练策略：图像到文本生成和像素级文档重建。生成任务对齐视觉表示与文本内容，重建任务保留字符笔画和布局细节。这种设计使得编码器既能理解文本内容，又能保持精细视觉结构。

Q4: 论文做了哪些实验？

论文进行了大量实验。首先在五个代表性的文档分析任务上评估：(1)文本识别（CRNN基准，从58.7%提升至67.3%）；(2)公式识别（UniMERNet-T 110M超越UniMERNet-B 325M）；(3)文本检测；(4)文档篡改检测；(5)重叠文本分割。所有任务上替换原始编码器为MonkeyOCRv2均有一致提升。其次，将MonkeyOCRv2作为多模态大语言模型的视觉编码器，在文档解析任务上（MDPBench，涵盖17种语言），冻结编码器配合0.7B语言模型，以约11倍小的视觉编码器超越了此前最好的3B dots.mocr模型，绝对提升2.8%。在OmniDocBench上也优于更大通用VLM如Qwen3-VL-235B和GPT-5.2。在文档理解上，冻结的编码器在八个基准上优于基于CLIP、DINO和SAM的模型。

Q5: 发现了什么实验现象？

实验揭示以下现象：(1)文档视觉预训练带来的提升是普遍且一致的，在五个不同任务上均有效，说明模型学到了可迁移的文档特征。(2)替换编码器后，较小模型（UniMERNet-T 110M）能够超越更大模型（UniMERNet-B 325M），表明编码器质量比模型规模更重要。(3)在多模态大语言模型设置中，冻结的MonkeyOCRv2作为视觉编码器表现出色，即使语言模型很小（0.7B），也能达到甚至超越更大VLM的结果，说明文档专用视觉编码器可以降低对语言模型规模的依赖。(4)在文档理解上，相比通用编码器（CLIP、DINO、SAM），MonkeyOCRv2在所有八个基准上表现更好，进一步证明文档导向预训练的必要性。

Q6: 有什么可以进一步探索的点？

基于本文工作，未来可探索的方向包括：(1)将MonkeyOCRv2扩展到更多文档任务，如表格识别、版面分析等。(2)研究更高效的预训练方法，例如利用弱监督或自监督信号进一步降低数据需求。(3)探索将MonkeyOCRv2与其他模态（如语言、布局）联合训练的方式。(4)针对特定语言或领域（如历史文献、手写文档）进行领域适应。(5)研究编码器在更复杂多模态任务中的泛化能力。

Q7: 总结一下论文的主要内容

本文由来自华中科技大学等机构的研究者提出MonkeyOCRv2，一个面向文档智能的视觉-文本预训练基础模型。论文的背景是主流视觉编码器（如CLIP、DINO）在自然图像上预训练，缺乏对文档图像中密集文本和字符级细节的感知能力。为了解决这一问题，作者首先构建了迄今为止最大的文档预训练语料库MonkeyDoc v2，包含1.13亿张图像，覆盖17种语言，确保了数据的多样性和字符覆盖率。其次，提出了一种联合预训练策略：图像到文本生成（image-to-text generation）和像素级文档重建（pixel-level document reconstruction）。前者通过将图像特征解码为文本序列来对齐视觉和文本表示，后者通过重构图像像素来保留字符笔画和布局结构。这种双目标设计使得编码器能够同时理解文本内容并保持视觉细节。在实验部分，论文首先在五个经典文档分析任务上验证了MonkeyOCRv2的有效性：文本识别（CRNN从58.7%提升至67.3%），公式识别（110M的UniMERNet-T超越325M的UniMERNet-B），文本检测，文档篡改检测和重叠文本分割。所有任务中，简单替换原始视觉编码器为MonkeyOCRv2都带来了一致的性能提升，说明其特征具有良好的泛化性。更进一步，论文将MonkeyOCRv2作为多模态大语言模型（MLLM）的视觉编码器，在更复杂的文档解析和文档理解任务上测试。在文档解析方面，冻结的MonkeyOCRv2配合一个0.7B的轻量语言模型，在MDPBench基准上超越了之前最好的3B dots.mocr模型，绝对提升2.8%，而视觉编码器大小仅为后者的约1/11。在OmniDocBench上，该模型甚至超越了Qwen3-VL-235B和GPT-5.2等大型通用VLM。在文档理解方面，冻结的MonkeyOCRv2在八个基准上全面优于基于CLIP、DINO和SAM的对应模型。这些结果有力地证明了文档导向的视觉预训练可以成为文档智能领域的基础设施。论文还讨论了方法的局限性，如预训练计算开销，并计划开源代码和数据。总之，MonkeyOCRv2通过大规模数据、专门预训练和广泛验证，为文档AI提供了一种强大的视觉编码器方案。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文涉及文档图像生成和理解，与用户画像中的生成方向直接相关（权重0.1）

## 基本信息

- 作者：Yuliang Liu, Zhang Li, Ziyang Zhang, Shuo Zhang, Qiang Liu, Jiajun Song, Zidun Guo, Xinhan Wang, Handong Zheng, Yang Liu, Dongliang Luo, Zhiyin Ma, Jiarui Zhang, Xiang Bai
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11562v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据片段，并优先使用了field_evidence_map中的对应证据。
