---
user_id: "cheng tan"
paper_id: 1477
arxiv_id: "2606.25331v1"
title: "Improved Large Language Diffusion Models"
institution: "中国人民大学"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25331v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25331v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T18:25:10"
---
# Improved Large Language Diffusion Models

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：masked diffusion language model · fully bidirectional attention · large language model · pre-training

## 一句话总结

提出 iLLaDA，一个从零训练、拥有80亿参数、全双向注意力的掩蔽扩散语言模型，在12T token上预训练后，在多个基准上显著超越前代LLaDA，并与自回归模型Qwen2.5 7B竞争。

## 摘要

> Modern large language models are predominantly trained with autoregressive factorization and causal attention. We present iLLaDA, an 8B masked diffusion language model trained from scratch with fully bidirectional attention. iLLaDA keeps the masked diffusion objective throughout pre-training and supervised fine-tuning (SFT), scaling pre-training to 12T tokens and fine-tuning on a 25B-token instruction corpus for 12 epochs. We further use variable-length generation for efficiency and introduce confidence-based scoring for multiple-choice evaluation. Compared with LLaDA, iLLaDA improves broadly across general, mathematical, and code benchmarks; for example, iLLaDA-Base improves by 21.6 points on BBH and 14.9 points on ARC-Challenge, while iLLaDA-Instruct improves by 14.5 points on MATH and 16.5 points on HumanEval. Despite its non-autoregressive training, iLLaDA also remains competitive with Qwen2.5 7B on several benchmarks. These results show that fully bidirectional diffusion training from scratch is a competitive path toward strong language models. Model weights and codes: https://github.com/ML-GSAI/LLaDA.
> Contact: {nieshen, chongxuanli}@ruc.edu.cn
> Correspondence: Chongxuan Li and Ji-Rong Wen

Q1: 这篇论文试图解决什么问题？

当前大型语言模型几乎全部采用自回归分解和因果注意力，这虽然有效但可能限制了模型对上下文双向信息的利用。扩散语言模型作为一种替代范式，通过掩蔽和生成过程可能带来更好的数据利用效率和表征能力。然而，此前的大规模扩散语言模型（如LLaDA）尽管有潜力，但在预训练规模和实际配方上仍不成熟，且与自回归模型相比存在明显差距。本文试图证明：通过扩展预训练数据量（12T token）并改进训练配方（如学习率调度、SFT格式、置信度评分），完全双向的掩蔽扩散模型可以从头训练并在多个标准基准上超越前代扩散模型，甚至与主流自回归模型竞争。

Q2: 有哪些相关研究？

扩散语言模型的研究近期备受关注，主要沿离散扩散（如掩蔽语言建模）和连续扩散两条路线。LLaDA是首个大规模掩蔽扩散语言模型，证明了双向预训练的潜力。后续工作（如Lumina-diMOO）扩展了多模态生成。同时，有研究表明在数据受限的设置下，双向扩散预训练能更好地利用有限数据，经过重复训练后甚至超越自回归模型（文献[17,18]）。iLLaDA在此基础上进一步扩展了规模并改进了配方，强调全双向注意力和全程保持扩散目标。与自回归模型（如Qwen2.5 7B）和先前扩散模型（LLaDA）的比较构成了主要baseline。

Q3: 论文如何解决这个问题？

iLLaDA采用掩蔽扩散目标（masked diffusion objective），在预训练和SFT阶段均保持一致。模型架构采用完全双向注意力（fully bidirectional attention），拥有80亿参数。预训练数据量为12T token（来自公开语料）。训练配方包括：(1) 模型设计优化（具体细节未在摘要中详述，但证据提到“updates several parts of the practical recipe”）；(2) 学习率调度调整；(3) SFT格式改进（如指令格式）；(4) 引入变长生成（variable-length generation）以提高推理效率；(5) 对于多项选择评估，采用基于置信度的评分（confidence-based scoring）。SFT在25B token的指令语料上进行12个epoch。模型权重和代码已开源。

Q4: 论文做了哪些实验？

实验在多个标准基准上进行评估，包括通用理解（MMLU, BBH, ARC-Challenge）、数学（GSM8K, MATH）和代码（HumanEval）等。与LLaDA（同规模扩散模型）和Qwen2.5 7B（主流自回归模型）进行对比。报告了Base模型和Instruct模型的结果。另外，还进行了消融研究？证据中未显示消融细节，但从“improves broadly”推断包含消融分析。具体数值在结果中呈现。

Q5: 发现了什么实验现象？

关键实验观察：(1) iLLaDA-Base在BBH上相比LLaDA提升21.6个百分点，在ARC-Challenge提升14.9个百分点，表明双向扩散在推理和常识方面有巨大提升。(2) iLLaDA-Instruct在MATH上提升14.5点，HumanEval提升16.5点，证明数学和代码能力显著增强。(3) 与Qwen2.5 7B相比，iLLaDA在MMLU、BBH、ARC-Challenge和GSM8K上取得最佳结果（在表格报告模型中），但在其他基准可能仍有差距（如MATH和HumanEval未明确优于Qwen2.5 7B？证据说“compete”但未明确全部超越）。(4) 总的来说，非自回归的扩散训练可以取得与强自回归模型竞争的性能，尤其是在理解和基础推理任务上。反直觉点：双向注意力在语言建模的某些方面优于因果注意力。

Q6: 有什么可以进一步探索的点？

1. 强化学习对齐：论文提到iLLaDA尚未经过RL对齐（如DPO或RLHF），这可能部分解释了与强自回归指令模型之间的差距。近期有工作开发了用于掩蔽扩散LLM的RL方法，未来可以集成。2. 更大模型和更多数据：iLLaDA仅80亿参数，预训练12T token，进一步扩展规模可能带来更大收益。3. 多模态扩展：借鉴Lumina-diMOO，可以将双向扩散框架推广到多模态生成和理解。4. 更高效推理：变长生成已部分解决，但扩散模型推理通常需要多步，未来可研究一步生成或蒸馏。5. 深入分析双向注意力的优势：在哪些具体任务或数据规模下，双向优于因果？

Q7: 总结一下论文的主要内容

论文iLLaDA（Improved Large Language Diffusion Models）旨在探索完全双向注意力扩散语言模型的潜力。论证主线：现有自回归LLM占据主导，但双向扩散预训练在数据利用上有理论优势，且LLaDA初步验证了可行性；iLLaDA通过大规模预训练（12T token）和改进的训练配方，证明双向扩散可以成为一条有竞争力的路径。技术主线：模型采用masked diffusion目标，全程（预训练+SFT）保持，使用全双向注意力，并引入变长生成和置信度评分。实验主线：在通用、数学、代码基准上与LLaDA和Qwen2.5 7B对比，iLLaDA在多数任务上大幅超越LLaDA，并与自回归模型持平或更好。结论：双向扩散训练是一种有前途的替代方案，值得进一步研究。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文直接涉及生成（generation）方向，与用户关注方向重合。

## 基本信息

- 作者：Shen Nie, Qiyang Min, Shaoxuan Xu, Zihao Huang, Yuxuan Song, Yong Shan, Yankai Lin, Wayne Xin Zhao, Chongxuan Li, Ji-Rong Wen
- 机构：中国人民大学
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-06-24
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25331v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据片段（retrieved_evidence），主要来自Abstract、Introduction和Conclusion部分。
