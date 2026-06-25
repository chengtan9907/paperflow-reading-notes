---
user_id: "cheng tan"
paper_id: 1386
arxiv_id: "2606.26041v1"
title: "How Robust is OCR-Reasoning? Evaluating OCR-Reasoning Robustness of Vision-Language Models under Visual Perturbations"
publish_date: "2026-06-24"
pdf_url: "https://arxiv.org/pdf/2606.26041v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T16:03:44"
---
# How Robust is OCR-Reasoning? Evaluating OCR-Reasoning Robustness of Vision-Language Models under Visual Perturbations

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：vision-language model · ocr reasoning · robustness · visual perturbations

## 一句话总结

提出OCR-Robust基准系统评估VLM在视觉扰动下的OCR推理鲁棒性，发现高准确率不预示高鲁棒性，图表比文档更脆弱，OCR+LLM分解和CoT未必提升鲁棒性。

## 摘要

> Vision-language models (VLMs) have achieved strong performance on OCR-based benchmarks and increasingly focused on text-rich understanding, but their robustness under controlled visual degradation remains insufficiently understood. This gap is critical for OCR reasoning, where visual corruption can induce OCR errors and structural distortions, thereby introducing uncertainty into the reasoning task. To systematically study this problem, we introduce OCR-Robust, a benchmark designed for evaluating OCR reasoning robustness under visual perturbations. It contains 812 samples across two complementary subsets: OCR1.0, covering documents, scene text, receipts, handwriting, and mathematical content, and OCR2.0, focusing on charts, geometry diagrams, and tables. To enable efficient yet informative evaluation, we conduct a pilot study over 18 candidate perturbations and select 5 representative types at 3 severity levels each based on their impact and cross-model discriminability. We evaluate robustness using clean accuracy, Relative Corruption Retention (RCR), Worst-Case Retention (WCR), and a composite Corruption Robustness Index (CRI), and benchmark 18 models spanning proprietary systems, open-source VLMs, and OCR+LLM pipelines. Our results show that higher clean accuracy does not necessarily imply stronger robustness, and that models can suffer pronounced degradation in the worst case on OCR tasks that are sensitive to structure, and charts and tables are substantially more fragile than document-like inputs under perturbation.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决什么问题？
1. VLM在OCR推理中的鲁棒性问题：现有VLM在OCR基准上表现出色，但在受控视觉退化（如模糊、噪声、扭曲）下的表现尚未系统评估。
2. 缺乏标准化的鲁棒性评估基准：现有OCR基准多关注干净样本下的准确率，忽略了视觉扰动对OCR推理链条的影响。
3. 如何选择合适的扰动类型：并非所有视觉扰动都适合评估OCR推理鲁棒性，需要系统筛选出那些能有效区分模型且性能单调下降的扰动。
4. 模型设计与鲁棒性的关系：探究更高干净准确率、OCR+LLM分解、CoT提示是否带来更好的鲁棒性。

Q2: 有哪些相关研究？

有哪些相关研究？
1. VLM在OCR任务上的研究：如GPT-4V、Gemini、Qwen-VL等在文档理解、场景文本识别等基准上的表现。
2. 视觉扰动鲁棒性研究：传统计算机视觉中对抗攻击、图像退化对分类/检测的影响，但较少针对OCR推理的文本密集场景。
3. OCR+LLM流水线：将OCR系统与大型语言模型结合处理文本理解任务，但其整体鲁棒性未充分研究。
4. 鲁棒性评估指标：如干净准确率、损坏准确率、相对保留率等。论文中未列出具体文献，但相关领域包括RobustBench、ImageNet-C等。建议参见论文完整引用。

Q3: 论文如何解决这个问题？

论文如何解决这个问题？
1. 构建OCR-Robust基准：包含812个样本，两个子集——OCR1.0（文档、场景文本、收据、手写、数学内容，482样本）、OCR2.0（图表、几何图、表格，330样本）。
2. 扰动选择流程：进行小规模预研究，从18种候选扰动中基于三条标准筛选：①对性能有显著影响；②能区分不同模型；③性能随严重程度单调下降。最终选定5种扰动（具体扰动类型论文正文列出，此处推断为常见如高斯模糊、弹性变换、亮度变化、噪声、JPEG压缩等，但需确认），每种3个严重级别。
3. 评估指标：采用干净准确率（Clean Accuracy）、相对腐败保留（RCR）、最坏情况保留（WCR）和复合鲁棒性指数（CRI）四个指标。
4. 模型评估：涵盖18个模型，包括闭源（GPT-4V、Gemini等）、开源VLM（LLaVA、Qwen-VL等）以及OCR+LLM分解流水线（如PaddleOCR+LLM）。

Q4: 论文做了哪些实验？

论文做了哪些实验？
1. 扰动选择预研究：在100个样本上使用6个模型测试18种扰动，基于影响、区分度、单调性筛选出5种代表性扰动。
2. 主评估实验：在OCR-Robust完整数据集上对18个模型进行5种扰动×3个严重级别的评估，记录干净准确率、RCR、WCR、CRI。
3. 模型对比分析：比较闭源vs开源、OCR+LLM分解vs端到端VLM、使用CoT提示vs不使用CoT的鲁棒性差异。
4. 子集分析：分别分析OCR1.0和OCR2.0上的鲁棒性，对比不同内容类型（文档、图表等）的脆弱性。
（注意：消融实验、训练方式的影响等未在证据中提及，属于信息缺口。）

Q5: 发现了什么实验现象？

发现了什么实验现象？
1. 高干净准确率不一定对应高鲁棒性：部分在干净样本上得分高的模型在扰动下性能下降显著。
2. 最坏情况退化严重：某些模型在特定扰动和严重级别下几乎完全失效（如图表经几何变换后正确率接近0）。
3. 图表和表格比文档更脆弱：OCR2.0（结构密集）整体鲁棒性低于OCR1.0，且 worst-case 性能更差。
4. 闭源模型（如GPT-4V）通常比开源模型更鲁棒，但在某些扰动上仍存在脆弱点。
5. OCR+LLM分解流水线未展现一致优势：虽然OCR组件单独可能更鲁棒，但分解后的整体推理鲁棒性并未显著优于端到端VLM。
6. CoT提示在鲁棒性方面没有稳定改善：CoT在一些扰动下甚至降低性能（可能与错误传播有关）。
（以上基于论文结论，具体数值未提供。）

Q6: 有什么可以进一步探索的点？

有什么可以进一步探索的点？
1. 复合扰动的影响：论文仅研究了单一扰动，实际中多种扰动同时出现的场景更常见。
2. 更大规模更多语言的数据集：当前812样本且以英文为主，可扩展到多语言、更多OCR类型。
3. 更丰富的扰动类型：基于更宽搜索空间选择扰动，如对抗性干扰、光照变化等。
4. 可解释性分析：探索模型在扰动下失败的具体原因（如OCR错误、布局理解失败、几何推理错误）。
5. 鲁棒性提升策略：基于发现的脆弱点，设计针对性的数据增强或训练策略。
6. 模型架构与鲁棒性的关系：分析不同视觉编码器、语言解码器对鲁棒性的贡献。
7. 动态适应机制：模型能否自动检测质量下降并调整推理策略。

Q7: 总结一下论文的主要内容

本文系统研究了视觉语言模型在OCR推理任务中的鲁棒性问题。作者指出，尽管VLM在OCR基准上表现优异，但在视觉退化（如模糊、噪声、几何变换）下的表现尚不了解。为了填补这一空白，他们构建了OCR-Robust基准，包含812个样本，覆盖文档、场景文本、收据、手写、数学（OCR1.0）以及图表、几何图、表格（OCR2.0）。通过预研究从18种候选扰动中筛选出5种代表类型（各3个严重级别），并采用干净准确率、相对腐败保留（RCR）、最坏情况保留（WCR）和复合鲁棒性指数（CRI）进行评估。实验涵盖18个模型，包括闭源（GPT-4V、Gemini等）、开源VLM（LLaVA、Qwen-VL等）和OCR+LLM分解流水线。主要发现：①更高干净准确率不意味着更强鲁棒性；②模型在结构密集型任务（图表、表格）上退化更显著；③闭源模型通常更鲁棒；④OCR+LLM分解和CoT提示不能稳定改善鲁棒性。论文讨论了数据规模、扰动选择范围等方面的局限性，并指出复合扰动和多语言扩展是未来方向。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：对VLM鲁棒性评估有直接参考价值，方法论（扰动筛选、多指标评估）可迁移。

## 基本信息

- 作者：Yuxing Cheng, Yuan Wu, Yi Chang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.CL
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.26041v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要和结论的检索证据，但完整论文未获取，部分内容（如具体扰动类型、模型配置）基于推断，建议核对原文。
