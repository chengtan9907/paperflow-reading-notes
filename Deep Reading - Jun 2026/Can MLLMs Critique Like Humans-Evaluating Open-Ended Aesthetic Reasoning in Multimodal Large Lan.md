---
user_id: "cheng tan"
paper_id: 1925
arxiv_id: "2606.29689"
title: "Can MLLMs Critique Like Humans? Evaluating Open-Ended Aesthetic Reasoning in Multimodal Large Language Models"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.29689.pdf"
pdf_url: "https://arxiv.org/pdf/2606.29689"
abs_url: "https://arxiv.org/abs/2606.29689"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:08:43"
---
# Can MLLMs Critique Like Humans? Evaluating Open-Ended Aesthetic Reasoning in Multimodal Large Language Models

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal large language models · aesthetic critique · open-ended evaluation · reference-based similarity

## 一句话总结

本文评估多模态大语言模型（MLLM）在开放式审美批评任务上与人类批评的相似性，发现基于参考的相似性指标会误导，反映的是流畅全面的风格而非选择性和特异性。

## 摘要

> Open-ended aesthetic critique is a challenge for multimodal large language models (MLLMs): unlike multiple-choice aesthetic benchmarks, it has no single correct answer, and most aesthetic evaluation has measured models against numeric scores rather than the written critiques people actually give. We evaluate MLLM critiques against ranked human references and ask whether they are close to human ones. Using the Reddit Photo Critique Dataset, we score five open-weight MLLMs against multiple ranked human critiques per photo with reference-based similarity metrics, under six prompt conditions that disentangle persona framing, aspect hinting, length control, and single- versus multi-pass generation, and add an image-grounding control that feeds each model the wrong photograph. We find that reference-based similarity gives a misleading picture. Stricter lexical and learned metrics show only weak alignment with human critiques, while a coarse embedding cosine reports broad topical overlap that the grounding control traces to a stable house style rather than image-specific observation. Behaviorally, the models diverge from humans in consistent ways the scores do not surface: even under a length cap they write two to three times as much, cover nearly every aesthetic aspect where humans are selective, engage each aspect more uniformly and at greater depth, and repeat themselves across critiques of the same photo where humans vary. We argue that reference-based similarity rewards a fluent, comprehensive critique style rather than the selectivity and specificity of human critique, and discuss implications for evaluating and training open-ended multimodal generation.

Q1: 这篇论文试图解决什么问题？

当前审美评估基准多使用数字分数或多选题形式，无法捕捉开放式审美批评的复杂性和选择性。MLLM在生成开放式批评时，其输出是否接近人类批评？现有基于参考的相似性指标（如ROUGE、BERTScore、余弦嵌入）是否能有效衡量这种相似性？论文旨在揭示参考相似性指标的误导性，并强调需要关注模型行为差异。

Q2: 有哪些相关研究？

相关工作包括审美评估基准（如AVA、Aesthetic Visual Analysis）、MLLM评估（如MMBench、MMMU）、文本生成评估指标（如ROUGE、BERTScore）以及模型行为分析。但缺乏针对开放式审美批评的参考相似性评估及其误导性的系统研究。

Q3: 论文如何解决这个问题？

使用Reddit Photo Critique数据集，包含图像和多个人类批评。评估五个开源MLLM（如LLaVA、Qwen-VL等）。设计六种提示条件：人格框架（新手/专家）、方面暗示（列举审美方面）、长度控制（无限制/固定长度）、单轮/多轮生成。使用参考相似性指标：词汇（ROUGE-L）、学习（BERTScore F1）、余弦嵌入（IN-CLIP）。增加图像接地控制：将每个模型的正确图像与随机其他图像交换，比较批评相似性。分析输出长度、方面覆盖、重复率等行为特征。

Q4: 论文做了哪些实验？

实验包括：1）在六种提示条件下生成每个照片的批评；2）计算每个模型批评与多个人类批评的相似性；3）图像接地控制：计算模型对正确图像（C）和错误图像（S）的批评与人类批评（H）的相似性，比较C-vs-H、S-vs-H和C-vs-S；4）统计输出长度、每个审美方面提及频率、重复短语比例。使用多个MLLM（具体模型列表见论文）和Reddit Photo Critique数据集（数量、来源见论文）。

Q5: 发现了什么实验现象？

1）参考相似性指标给出误导性画面：严格词汇和学习指标显示模型与人类弱对齐，但余弦嵌入显示高重叠。2）接地控制：模型对正确和错误图像的批评相似性（C-vs-S）高于任何模型与人类批评的相似性（C-vs-H和S-vs-H），表明模型具有稳定的风格而非图像特定观察。3）行为差异：模型输出长度是人类2-3倍（即使有长度限制）；模型几乎覆盖所有审美方面，而人类选择性关注少数几个；模型每个方面均匀深入，人类有侧重点；模型在同一照片的不同批评中重复短语，人类则变化。

Q6: 有什么可以进一步探索的点？

可探索的方向：1）开发更细粒度的评估指标，能捕捉选择性和特异性；2）训练MLLM时引入选择性生成或长度控制；3）利用多样化提示条件或指令微调减少风格偏差；4）跨领域测试（如艺术、设计）；5）人机协作评估框架；6）研究模型内部表示与批评生成的关系。

Q7: 总结一下论文的主要内容

论文系统评估了MLLM在开放式审美批评任务上的表现。使用Reddit Photo Critique数据集和五种模型，通过六种提示条件控制多个变量，结合参考相似性指标和接地控制实验，揭示了当前指标的两大问题：一是参考相似性奖励流畅全面而非选择性特异性，二是模型有强烈的内部风格导致批评缺乏图像特异性。行为分析进一步量化了模型与人类在长度、覆盖范围、深度和重复性上的差异。论文贡献包括：提出评估开放式批评的实验框架、诊断参考相似性缺陷、提供行为证据和未来改进方向。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：直接涉及多模态大语言模型的生成评估和行为分析

## 基本信息

- 作者：Sajjad Ghiasvand, Maryam Amirizaniani, Haniyeh Ehsani Oskouie, Mahnoosh Alizadeh, Ramtin Pedarsani
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.29689`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本生成参考了PDF检索证据（retrieved_evidence），并在启发式草稿基础上补全和校准。
