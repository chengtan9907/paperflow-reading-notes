---
user_id: "cheng tan"
paper_id: 1868
arxiv_id: "2606.29672"
title: "How LLMs See Creativity: Zero-Shot Scoring of Visual Creativity with Interpretable Reasoning"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.29672.pdf"
pdf_url: "https://arxiv.org/pdf/2606.29672"
abs_url: "https://arxiv.org/abs/2606.29672"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T15:53:49"
---
# How LLMs See Creativity: Zero-Shot Scoring of Visual Creativity with Interpretable Reasoning

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：automated creativity assessment · chain-of-thought reasoning · large language models · llm-as-a-judge

## 一句话总结

多模态大语言模型可以在零样本条件下匹配人类对视觉创造力的评价，其推理链揭示了评估过程但并未提高与人类评分的一致性。

## 摘要

> Evaluating the originality of visual images poses enduring challenges for creativity assessment. Automated scoring using AI models has proven effective in the verbal domain, yet key questions remain about evaluating visual creativity and understanding how models arrive at their ratings. The present research asks whether multimodal large language models (LLMs) can serve as judges of visual creativity zero-shot—without any fine-tuning or examples of human ratings—and whether their “reasoning” output offers an interpretable window into their evaluation process. We tested six multimodal LLMs (Gemini 3 Flash, Gemma 4 31B IT, GPT-5.4 Mini, GLM-5v Turbo, Kimi K2.5, and Qwen 3.6 Plus) on 992 AI-generated images (based on human-written prompts) and 1,500 hand-drawn sketches scored for creativity by human raters. In Study 1, all models showed substantial alignment with human creativity ratings on both datasets (r = .57–.68 on AI-generated images; r = .29–.68 on sketches). In Study 2, we analyzed the step-by-step reasoning processes of three LLMs evaluating the same images and drawings. Although reasoning made model evaluations interpretable—showing what they attend to, how they balance originality vs. quality, and how they justify their ratings—reasoning did not improve alignment with human ratings. In sum, our findings indicate that multimodal LLMs can match human judgments of visual creativity without any additional training, and that their reasoning reveals how AI models evaluate creativity. An open scoring app implementing this pipeline is available at https://review-visual-eval-scoring.hf.space.
> Keywords: automated creativity assessment; chain-of-thought reasoning; large language models; LLM-as-a-judge; visual creativity

Q1: 这篇论文试图解决什么问题？

视觉创造力评估面临两大难题：一是人工评分费时费力、难以规模化；二是缺乏对自动评分模型评估过程的理解。现有自动评分主要集中于语言领域，视觉领域的零样本评估及其可解释性尚未充分探索。本文试图回答：通用多模态大语言模型能否在没有微调或示例的情况下作为视觉创造力的评判者？模型的推理输出能否揭示其评估机制？

Q2: 有哪些相关研究？

相关工作主要分为两条主线：一是传统的创造力评估方法，如共识评估技术（Amabile, 1982）和发散思维中的主观评分（Silvia et al., 2008），这些方法可靠但难以扩展。二是基于AI的自动评分探索，在语言领域已有成功应用（如自动评分言语创造力），但在视觉领域进展有限。近期研究尝试用深度学习模型分类高创造性绘画（例如Cropley等人2025年的大规模开源流水线），但缺乏对通用LLM零样本评分的系统性检验。本文填补了这一空白。

Q3: 论文如何解决这个问题？

论文采用双研究设计。研究1：直接请求6个多模态LLM对图像（AI生成图像和手绘草图）进行1-5分创造力评分，计算与人类评分的斯皮尔曼相关性，并通过控制边缘密度等低级视觉特征进行偏相关分析。研究2：收集其中3个具备推理能力的模型（GLM-5v Turbo、Kimi K2.5、Qwen 3.6 Plus）在评分时生成的推理链，采用两类分析：(a) 用另一个纯文本模型（GPT-5.4 Nano）从链中预测原始评分（移除数字后），以检验链是否包含真实评分信号；(b) 将链分解为句子，按感知、原创性、质量、理由四个类别编码，揭示推理结构。

Q4: 论文做了哪些实验？

研究1：使用两个数据集——992个AI生成图像（基于人类提示生成，已有创造力标准评分）和1500个手绘草图（来自人类参与者，经多名评分者打分）。六种LLM在零样本设置下对每个图像评分，计算与人类平均评分的Spearman相关系数。数据集平衡了来源和风格。研究2：重新运行三个推理模型，收集推理链，并用文本模型预测评分以验证链的有效性。还对推理链句子进行分类，统计各维度出现频率。

Q5: 发现了什么实验现象？

1. 所有六种模型与人类评分呈中度以上正相关（AI图像上r=0.57-0.68，手绘草图上r=0.29-0.68），其中Gemini 3 Flash表现最强（r=0.68），GPT-5.4 Mini在草图上最弱（r=0.29）。2. 模型普遍存在宽松偏差：人类评分偏低（均值2.39），而模型评分偏高（均值2.67-3.63），几乎不使用最低分。3. 偏相关分析控制边缘密度后相关性几乎不变，表明对齐不是源于对低级视觉复杂性的共同敏感。4. 推理链分析显示文本模型能从链中高度准确地重建原始LLM评分（即使移除了数字），证明链包含真实评分信号。5. 推理链句子分类揭示了跨模型一致的“感知→原创性→质量→理由”四阶段结构，但推理链的添加并不提高与人类评分的一致性。6. 不同模型在关注维度上存在差异，例如有的更侧重原创性，有的更侧重质量（如Kimi K2.5偏重质量，Qwen 3.6 Plus偏重原创性）。

Q6: 有什么可以进一步探索的点？

1. 扩展至其他视觉创造力类型（如产品设计、艺术作品、复合创意产品）以检验泛化能力。2. 研究模型推理链的准确性：尽管链包含评分信号，但可能包含幻觉或错误归因，需设计机制检测。3. 探索如何通过提示工程或微调改善模型的宽松偏差和评分分辨率。4. 引入人类专家评分作为更严格的基准，测试模型在专业领域的表现。5. 研究文化或教育背景对LLM评分的影响，以及模型是否携带训练数据中的偏见。6. 结合因果推理方法，区分模型是真正理解创造力还是仅仅模仿表面特征。

Q7: 总结一下论文的主要内容

本论文系统研究了多模态大语言模型在视觉创造力评估中的零样本能力，及其推理过程的可解释性。作者指出，随着生成式AI产生大量开放输出，评估成为瓶颈，尤其创造力评估因其主观性和多维度而充满挑战。尽管自动评分在语言领域取得进展，视觉领域的通用模型评估仍未充分探索。

研究分为两个子研究。研究1测试了六种最新多模态LLM（Gemini 3 Flash、Gemma 4 31B IT、GPT-5.4 Mini、GLM-5v Turbo、Kimi K2.5、Qwen 3.6 Plus）在两类图像（992个AI生成图像和1500个手绘草图）上的零样本创造力评分。所有模型均采用相同提示（无示例或无微调），计算评分与人类平均评分的Spearman相关性。结果表明，所有模型均与人类评分有中度以上正相关（AI图像上r=0.57-0.68，草图上r=0.29-0.68），其中Gemini 3 Flash在两个数据集上均表现最佳（r=0.68），而GPT-5.4 Mini在草图上表现最弱（r=0.29）。偏相关分析控制边缘密度后相关性几乎不变，证明对齐并非源于低级视觉特征。模型普遍存在宽松偏差，偏好较高分数。

研究2聚焦于三个具备显著推理能力的模型（GLM-5v Turbo、Kimi K2.5、Qwen 3.6 Plus），收集其在相同图像上的推理链。首先，利用一个独立的文本模型（GPT-5.4 Nano）仅从推理链（移除数字后）预测原始评分，发现预测评分与原始LLM评分高度一致，证明推理链包含真实的评分信号，而非事后合理化。其次，将推理链句子按感知、原创性、质量、理由四类编码，揭示跨模型一致的评估结构：模型首先描述图像内容，然后分维度评估，最后给出理由。不同模型各维度侧重不同。

关键词发现：推理链虽然提供了可解释性，但并未提升模型评分与人类的一致性；即，推理的存在与否不影响评分质量。这一结果对LLM激励机制设计有启示：推理过程可能主要服务于事后解释，而非决策本身。

论文的贡献在于：（1）首次证明通用多模态LLM可以零样本匹配人类视觉创造力评分，无需领域微调；（2）揭示了LLM评估视觉创造力的内部推理结构；（3）提供了一个开源评分应用，推动后续研究。局限性包括：模型存在宽松偏差、数据集覆盖有限、未包含人类专家对比、以及推理链可能是生成式而非因果性的。未来工作可探索模型在不同创意领域、文化背景下的表现，以及如何进一步提升评分分辨率和推理链的真诚性。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：直接相关到生成方向（权重0.10），尤其适用于评估生成内容（如AI图像）的创造力。

## 基本信息

- 作者：William Orwig, Roger E. Beaty
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.29672`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence），并按照field_evidence_map优先使用对应字段的证据锚点。
