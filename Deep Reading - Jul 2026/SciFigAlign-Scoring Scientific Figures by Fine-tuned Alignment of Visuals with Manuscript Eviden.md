---
user_id: "cheng tan"
paper_id: 6052
arxiv_id: "2607.27066v1"
title: "SciFigAlign: Scoring Scientific Figures by Fine-tuned Alignment of Visuals with Manuscript Evidence"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.27066v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.27066v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:25:21"
---
# SciFigAlign: Scoring Scientific Figures by Fine-tuned Alignment of Visuals with Manuscript Evidence

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：scientific figure quality · multimodal assessment · CLIP fine-tuning · SciBERT

## 一句话总结

本文提出 SciFigAlign，一种基于手稿证据微调的多模态评分器，用于科学插图质量评估，在自建数据集上显著优于传统方法和 LLM 评判。

## 摘要

> Scientific figure assessment in peer review differs fundamentally from general image quality evaluation: a figure must be visually legible, faithfully support the manuscript's claims, and communicate evidence with a clear visual hierarchy. However, if we apply traditional image assessment methods to scientific figure quality assessment, limitations emerge: classic IQA models capture perceptual quality or aesthetics but cannot judge whether a figure serves the paper's scientific argument; CLIP-based methods assess generic image-text correspondence, yet lack understanding of manuscript context; and zero-shot LLM/VLM judges, when repurposed for figure scoring, often yield overly concentrated scores with limited fusion of visual and textual evidence. We introduce an annotated dataset of 3,857 scientific figures from peer-reviewed conference papers, each rated along four peer-review-oriented dimensions: Clarity, Relevance, Informativeness, and Structure. We propose SciFigAlign, a fine-tuned multimodal scorer that grounds figure quality assessment in manuscript evidence. Given a figure crop, caption, citing paragraphs, and light paper context, SciFigAlign fine-tunes CLIP and SciBERT end-to-end with per-modality cross-attention and CubeMLP fusion, jointly optimizing SmoothL1 regression with a within-paper ranking hinge loss. Under paper-level splits, SciFigAlign achieves a macro MAE of 0.3524 and a within-paper pairwise accuracy of 81.64% on the test set with n = 396, a 59% relative error reduction over the best LLM-as-judge baseline with MAE 0.864. Ablations confirm that manuscript-grounded inputs, citing-context denoising, and ranking supervision are all critical, showing that scientific figure assessment requires learned alignment between visual content and manuscript evidence rather than prompting alone, even with state-of-the-art VLMs.

Q1: 这篇论文试图解决什么问题？

同行评审中对科学插图的评估需要同时考虑视觉可读性、证据支持性和沟通层次，而现有图像质量评估方法主要针对自然图像，缺乏对科学声明一致性的判断。CLIP 方法评估图像-文本对应但缺少手稿上下文；零样本 LLM/VLM 作为评分器时输出分数集中，无法有效区分同一论文中插图的优劣。此外，缺乏标注数据集限制了该领域研究。本文旨在解决科学插图质量评估中如何利用手稿证据进行细粒度、可区分评分的问题。

Q2: 有哪些相关研究？

相关研究包括传统图像质量评估（IQA），如基于自然图像统计的 NR-IQA 方法（Mittal 等）；多模态嵌入方法，如 CLIP 及其变体用于图像-文本匹配；零样本 LLM/VLM 作为评判者，如使用 GPT-4 或 Qwen2-VL 直接打分；科学图像理解领域如图表问答（ChartQA）、论文图理解（PaperFig）等。本文特别指出，现有方法均无法有效评估科学插图与手稿声明的对齐程度，尤其是需要区分同一论文内不同插图的相对质量。

Q3: 论文如何解决这个问题？

SciFigAlign 是一个多模态评分框架，输入包括插图裁剪、标题、引用段落和轻量论文上下文。视觉编码使用预训练 CLIP ViT-L/14，文本编码使用 SciBERT，然后通过逐模态跨模态注意力（类似于 Q-Former 的交叉注意力）对齐视觉和文本特征，最后使用 CubeMLP 融合输出四个维度的分数（清晰度、相关性、信息量、结构）。训练采用多任务损失：SmoothL1 回归损失逼近人工评分，以及页内排序 Hinge 损失确保同一论文中质量高的插图获得更高总分。引用上下文通过去重和相关性过滤进行降噪。模型在页级划分的数据集上训练，防止信息泄露。

Q4: 论文做了哪些实验？

实验基于自建数据集，包含 3,857 张来自开放获取会议论文的插图（其中 1,982 张经过人工评分），每张插图在四个维度上由 3 名标注者打分。数据集按论文划分为训练/验证/测试（66/28/145 篇论文，测试集 n=396）。对比基线包括零样本 LLM（GPT-4o、Qwen2-VL-7B 等）、线性探针（SigLIP2-Ridge）、微调 VLM（BLIP2-LoRA、LLaVA-Med-LoRA）。评估指标为宏 MAE、Spearman 秩相关系数（SRCC）和页内配对准确率（PA）。消融实验移除了不同输入组件（标题、引用、上下文）、不同编码器、以及排序损失，分析各组件贡献。

Q5: 发现了什么实验现象？

SciFigAlign 在宏 MAE 上达 0.3524，PA 达 81.64%，比最佳 LLM 基线（MAE 0.864）降低 59% 误差，比 SigLIP2-Ridge（MAE 0.877）降低 60%。但 SRCC 仅为 0.3088，表明跨论文的排序一致性一般。消融显示去除引用上下文后 MAE 上升至 0.394，去除排序损失后 PA 下降至 74.5%。失败模式分析表明：拥挤的多面板定性网格图和 OCR 密集的表格图容易误判；信息量方面存在分歧（视觉稀疏但标题声称强结果）；结构错误常与不规则面板布局有关。LLM 基线评分分布集中，区分度不足。

Q6: 有什么可以进一步探索的点？

可探索方向包括：扩展到医学、生物等更多学科；引入全文上下文和引用关系以增强对齐；细粒度评价（每个维度独立优化或生成解释性评语）；结合生成模型自动修复低质量插图；改进跨领域泛化能力；探索更高效的编码器或轻量化模型用于实时评估。对于数据集，可以扩充更多样化的图像类型和更多标注者以降低主观性。此外，可将评分框架用于辅助同行评审系统或自动论文质量筛查。

Q7: 总结一下论文的主要内容

本文针对科学插图质量评估这一同行评审中的关键任务，指出现有通用方法（IQA、CLIP、LLM）均无法有效捕捉插图与手稿声明的对齐关系。作者首先构建了一个包含 3,857 张来自同行评审会议论文插图的标注数据集，每张插图在清晰度、相关性、信息量、结构四个维度上由人工评分，并按论文划分以防止数据泄露。然后提出 SciFigAlign 框架，采用 CLIP 和 SciBERT 作为多模态编码器，通过逐模态跨模态注意力对齐视觉与文本表示，使用 CubeMLP 融合输出四维分数。训练联合使用 SmoothL1 回归损失和页内排序 Hinge 损失，后者强制同一论文内高质量插图排序更高。输入中包含手稿上下文（标题、引用段落、论文摘要/引言），引用段落通过去噪提高信噪比。实验结果显示，SciFigAlign 在测试集上达到宏 MAE 0.3524、页内配对准确率 81.64%，比最佳 LLM 评判基线误差降低 59%，比线性探针降低 60%。消融实验证实每个输入组件和损失函数均贡献明显。错误分析揭示了模型在密集多面板图、OCR 表格和视觉稀疏图上的不足。本文的主要贡献包括：新标注数据集、端到端多模态评分框架、与手稿证据对齐的评分方法、以及显著优于现有方法的实证结果。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文专注于 ai-for-science 领域中的科学插图质量自动评估，可辅助同行评审系统。

## 基本信息

- 作者：Chuanzhi Xu, Zihan Deng, Huiqi Liang, Chengkun Yue, Zhanlin Cui, Pengfei Ye, Weidong Cai
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.27066v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，优先采用了 retrieved_evidence 中的片段，并结合 heuristic_draft 进行补充和润色。
