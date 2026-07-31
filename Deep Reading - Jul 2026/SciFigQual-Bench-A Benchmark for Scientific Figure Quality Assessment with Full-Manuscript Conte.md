---
user_id: "cheng tan"
paper_id: 6012
arxiv_id: "2607.27084v1"
title: "SciFigQual-Bench: A Benchmark for Scientific Figure Quality Assessment with Full-Manuscript Context"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.27084v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.27084v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:21:10"
---
# SciFigQual-Bench: A Benchmark for Scientific Figure Quality Assessment with Full-Manuscript Context

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：scientific figure quality assessment · benchmark · full-manuscript context · multi-modal evaluation

## 一句话总结

本文提出 SciFigQual-Bench，一个利用全文本语境评估科学图像质量（清晰度、布局、图注匹配、上下文相关性、误导风险）的基准，并设计了 SFQ-Agent 分阶段跨模态自动化评估框架，实验表明其优于直接评估等方法。

## 摘要

> Scientific images are the core elements of presenting experimental conclusions, elaborating system architecture, and supporting comparative arguments in scientific papers. However, existing image quality assessment (IQA) are predominantly designed for natural photographs or AI-generated content, which cannot be directly applied to scientific papers. The few existing studies on scholarly charts remain confined to visual-surface comparisons, failing to verify caption alignment, citation relevance, or visual misleadingness. To address this, we propose SciFigQual-Bench, a full-text contextual benchmark that evaluates scientific image across five dimensions (clarity, layout, caption fit, context relevance, and misleading risk). The data covers the top computer-science conferences from 2020 to 2025, 6,308 images were independently scored by multiple domain experts in five dimensions and aggregated into the gold-standard annotations. Unlike previous scientific figure benchmarks, our dataset binds each image to its caption, citing sentence, and manuscript context. To enable automated evaluation on this benchmark, we designed a staged cross-modal evaluation SFQ-Agent to achieve auditable and refined scoring through the collection and fusion of modal evidence. Multiple mainstream large models were evaluated on the test subset eval1200, and the SFQ-Agent (F3) equipped with GPT-5.6-Sol achieved the lowest overall average absolute error (0.418) and the highest ±1-point consistency rate (93.4%), consistently outperforming both direct evaluation and auxiliary (Sidecar) visual language model evaluation schemes. Project page: https://frankdengai.github.io/SciFigQual-Bench/ (code: https://github.com/FrankDengAI/SciFigQual-Bench).

Q1: 这篇论文试图解决什么问题？

现有图像质量评估（IQA）主要针对自然照片和 AI 生成内容，未能考虑科学图像的特定需求。科学图像在论文中用于呈现实验结果、阐述系统架构和支持比较论证，其质量需要从多个维度全面评估，包括显示清晰度、图注与图像匹配度、图像与论文上下文相关性、以及潜在的视觉误导风险。已有少数研究关注学术图表评估，但大多停留在视觉层面的表面比较，无法验证图注对齐、引用准确性或视觉误导性。此外，现有研究缺乏结合全文语境（标题、引用句、段落上下文）的评估基准，导致无法发现图像与文本之间的不一致。因此，需要构建一个科学图像质量评估基准，将图像绑定到其图注、引用句和完整稿件上下文，以实现更全面、可审计的评估。

Q2: 有哪些相关研究？

相关研究包括自然图像质量评估（如 NIQE, BRISQUE, 深度学习 IQA 模型）、AI 生成内容质量评估（如 AIGC Image Quality Assessment）、学术图表评估（如 ChartQA, FigureQA, 但侧重于图面理解而非质量评判）、以及多模态评估方法（如使用 VLM 进行图文匹配评估）。此外，证据驱动的 NLP 领域提出了一些关于合理但不忠实推理的警告。本工作填补了科学图像全面质量评估的空白，特别是结合全文语境。

Q3: 论文如何解决这个问题？

论文提出了 SciFigQual-Bench 基准和 SFQ-Agent 评估框架。SciFigQual-Bench 包含 6308 张图像，均来自 2020-2025 年计算机科学顶级会议论文，每张图像都绑定其原始图注、引用句和全文上下文。由 5+ 名领域专家在五个维度（清晰度、布局、图注匹配、上下文相关性和误导风险）上独立评分，并基于评分聚合得到标准标注（可能是平均值或经过一致性质量控制）。SFQ-Agent 是一个分阶段跨模态评估框架：第一阶段是证据收集，从图像（视觉）、图注（文字）、引用句和相关上下文（文本）中提取不同模态的证据，可能包括使用视觉编码器、文本编码器或预训练模型生成描述和特征；第二阶段是证据融合与评分，将这些模态证据整合到一个推理过程中，生成五个维度的分数。该框架强调可审计性和精细化，因为每个评分都有明确的证据支撑。作者构建了 eval1200 测试子集，包含 1200 张图像，用于评估各种模型（如 GPT-5.6-Sol）以及不同的评估方案（直接评估、使用侧车 VLM 辅助、SFQ-Agent 等）。

Q4: 论文做了哪些实验？

论文进行了以下实验：1) 构建 eval1200 测试子集，包含 1200 张具有标准标注的图像。2) 评估多个主流大模型（包括 GPT-5.6-Sol 等）在直接评估、使用侧车 VLM（Sidecar VLM）评估以及 SFQ-Agent 评估下的性能。3) 对比指标包括总体平均绝对误差（Overall MAE）和 ±1 点一致性率（±1-point consistency rate）。4) 分维度分析各个维度的 MAE 和系统偏差。5) 可能还分析了不同模型和方案在误导风险评估等维度的表现。

Q5: 发现了什么实验现象？

实验结果表明：1) 配备 GPT-5.6-Sol 的 SFQ-Agent (F3) 实现了最低的总体 MAE（0.418）和最高的 ±1 点一致性率（93.4%），显著优于直接评估和使用侧车 VLM 的评估方案。2) 直接评估往往在大模型上表现不佳，说明单纯依靠大模型进行视觉质量评估容易出错。3) 使用侧车 VLM 可以改善一些维度但仍有差距。4) SFQ-Agent 的分阶段证据融合方法有效提升了评估的准确性和可解释性。5) 某些模型存在系统性偏见，例如某个模型在某个维度上始终偏低（偏差区间 -0.120 至 -0.097），表明训练中内在的严苛性。6) 分维度 MAE 揭示了不同模型在不同维度上的优劣势。7) 误导风险维度评估难度更大，所有模型一致性较低。

Q6: 有什么可以进一步探索的点？

未来可以探索的方向包括：1) 将基准扩展到更多学科领域（如医学、生物学、工程等）。2) 增加更多评估维度，如颜色使用、可重复性、格式合规等。3) 改进 SFQ-Agent 框架，引入更多模态（如表格、图例）和更高级的证据推理机制。4) 开发自动误导检测和高亮不一致机制。5) 探索将基准用于同行评审自动辅助系统。6) 研究跨文化/跨会议图质量标准的差异。7) 降低评估成本，使框架适用于大规模数据集。

Q7: 总结一下论文的主要内容

论文研究科学图像质量评估问题。现有 IQA 方法主要针对自然照片或 AI 生成内容，缺乏针对科学论文中图像的全面评估。作者提出 SciFigQual-Bench，一个基于全文本语境的科学图像质量评估基准。从 2020-2025 年计算机科学顶级会议论文中收集 6308 张图像，每张图像绑定其图注、引用句和完整稿件上下文。由多位领域专家在清晰度、布局、图注匹配、上下文相关性和误导风险五个维度独立评分，聚合得到标准标注。同时设计 SFQ-Agent，一个分阶段跨模态自动化评估框架，通过收集和融合多方面证据（视觉、图注、引用句、上下文）实现可审计的精细化评分。在 eval1200 子集上评估多种主流大模型和评估方案，结果表明 SFQ-Agent 配合 GPT-5.6-Sol 取得最佳性能（总体 MAE 0.418，±1 一致性率 93.4%）。还分析了系统偏差和维度差异，展示了基准的诊断能力和价值。代码和数据已公开。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作属于 AI for Science 中科学论文质量自动评估方向，与智能体（agent）和 AI for Science 相关。

## 基本信息

- 作者：Zihan Deng, Chuanzhi Xu, Huiqi Liang, Haoyang Li, Xiaozhen Zhong, Lequan Yu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.27084v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，主要基于 Abstract、Introduction、Related Work、Conclusion 和实验分析部分。
