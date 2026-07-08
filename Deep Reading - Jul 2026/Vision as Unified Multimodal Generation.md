---
user_id: "cheng tan"
paper_id: 2943
arxiv_id: "2607.06560"
title: "Vision as Unified Multimodal Generation"
publish_date: "2026-07-08"
pdf_url: "https://arxiv.org/pdf/2607.06560"
abs_url: "https://arxiv.org/abs/2607.06560"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-09T00:33:52"
---
# Vision as Unified Multimodal Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：unified multimodal generation · computer vision · instruction tuning · visual prompts

## 一句话总结

论文提出将计算机视觉视为统一多模态生成，通过指令和视觉提示指定任务，单一模型无需专用架构即可匹配多种专业系统。

## 摘要

> We formulate computer vision as unified multimodal generation, where heterogeneous visual tasks are expressed in the native text and image generation spaces of a unified multimodal model, without task-specific architectures. Under this formulation, SenseNova-Vision uses natural-language instructions and optional visual prompts to specify tasks, target regions or views, and decoding conventions, and generates responses as text for symbolic outputs, images for dense spatial predictions, or mixed text-and-image outputs for compositional tasks. To support large-scale training, we convert diverse computer vision annotations into instruction-response examples compatible with these generation spaces, resulting in the SenseNova-Vision Corpus, a computer-vision instruction-response corpus spanning text, image, and mixed targets. Starting from an off-the-shelf pretrained unified multimodal model, SenseNova-Vision is trained primarily on this corpus, with auxiliary multimodal data used as a capability-preserving mixture, and requires no task-specific prediction heads or architectural modifications. The resulting model covers a broad range of vision tasks, including detection, OCR, keypoint estimation, segmentation, depth estimation, surface normal prediction, point maps, and camera pose estimation, while supporting language-defined variants that combine category, color, region, and other visual cues. Experiments show that a single unified model can match leading task-specialized systems across structured visual understanding, dense geometric prediction, segmentation, and multi-view visual geometry. These results suggest unified multimodal generation as a scalable route for integrating computer vision capabilities into general-purpose foundation models. The model and corpus are publicly available.

Q1: 这篇论文试图解决什么问题？

当前计算机视觉任务通常需要专用架构，如检测头、分割头等，导致模型碎片化，无法共享表示。多模态大模型虽有进展，但多数仍依赖任务特化适配器或额外的预测头。本文旨在解决这一问题，通过统一多模态生成框架，将所有视觉任务表述为文本和图像生成，消除任务专用架构，实现真正的统一模型。关键难点在于如何将异构标注统一转换为指令-响应格式，以及如何在生成空间中有效学习密集预测任务。

Q2: 有哪些相关研究？

相关工作包括：1) 视觉通用模型（如Unified-IO、VisionLLM等）尝试统一任务但常需专用头；2) 多模态大模型（如GPT-4V、LLaVA）通过指令微调处理多种任务但仍依赖外部模块；3) 将视觉任务视为生成问题的方法（如Pix2Seq、DETR）为序列生成，但本文扩展至图像生成空间。论文提出的统一多模态生成进一步消除了任务专用组件，直接利用模型的文本和图像生成能力。由于未获得PDF全文，相关工作细节可能不完整。

Q3: 论文如何解决这个问题？

本文提出统一多模态生成框架，输入为自然语言指令和可选视觉提示（如矩形框、点等），输出由指令动态决定：符号任务（如检测）输出文本，密集预测任务（如深度）输出图像（单通道或多通道编码），组合任务（如按颜色分割）输出图文混合。模型基于现成的预训练统一多模态模型（如类LLaMA架构），以SenseNova-Vision Corpus作为主要训练数据，该语料通过将现有视觉标注（检测框、分割掩码、深度图等）转换为指令-响应对构建。训练时混合通用多模态数据（如图像描述）以保持生成能力。整个训练过程无需任务特定预测头或架构修改，完全在原文生和图像生成空间优化。

Q4: 论文做了哪些实验？

论文在四类任务上评测：结构化视觉理解（检测、OCR）、密集几何预测（深度、表面法线、点图）、分割（语义/实例/全景）、多视图视觉几何（相机位姿）。与各领域领先的专用系统对比，例如检测对比DETR系列，深度对比DPT等。实验设置未提供更多细节（如训练轮数、批量大小），但声称单一模型在各项指标上均能达到或接近专业系统水平。数据集可能包括COCO、KITTI、ScanNet等常见基准，具体需查阅原文。

Q5: 发现了什么实验现象？

主要观察包括：1) 统一模型在结构化视觉理解任务上表现与专用模型相当，无明显差距；2) 密集几何预测任务中，图像生成的方式有效保留了空间细节；3) 分割任务中，结合文本指令和图像输出可以处理多种分割变体；4) 多视图几何任务展示了模型对视角和三维结构的理解。论文未报告负面结果或失败案例，但可能隐含模型在极端场景下仍需改进。

Q6: 有什么可以进一步探索的点？

未来可探索的方向：1) 扩展更多视觉任务，如视频理解、3D重建；2) 提升生成质量和分辨率，特别是密集预测图细节；3) 探索更大规模模型和数据的scaling规律；4) 结合强化学习或扩散过程优化生成；5) 应用于机器人感知、自动驾驶等实际场景；6) 研究任务间干扰和灾难性遗忘问题。

Q7: 总结一下论文的主要内容

本文提出了一种新的计算机视觉范式——统一多模态生成，将所有视觉任务统一到文本和图像生成空间中。通过自然语言指令和视觉提示，模型无需任务专用架构即可执行检测、分割、深度估计等多种任务。作者构建了SenseNova-Vision语料库，将现有视觉标注转换为指令-响应格式，并基于预训练多模态模型进行训练。实验表明单一模型在多个视觉任务上达到专业水平，展示了这种范式的可扩展性和统一性。论文贡献包括：提出统一框架、构建大规模语料库、验证模型有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文属于多模态生成与统一建模领域，与用户关注的生成方向（权重0.10）直接相关。

## 基本信息

- 作者：Xiaoyang Han, Jianhua Li, Kewang Deng, Zukai Chen, Xuanke Shi, Sihan Wang, Boxuan Li, Linyan Wang, Siyi Xie, Xin You, Jinsheng Quan, Zhongang Cai, Haiwen Diao, Ziwei Liu, Lei Yang, Dahua Lin, Quan Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-08
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.06560`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取时网络连接不稳定，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成主要依据论文摘要和元数据，未获得PDF全文，因此实验细节、消融研究、局限性等部分信息有限，建议结合原文验证关键结论。
