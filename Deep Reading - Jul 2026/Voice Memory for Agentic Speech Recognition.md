---
user_id: "cheng tan"
paper_id: 6024
arxiv_id: "2607.26410v1"
title: "Voice Memory for Agentic Speech Recognition"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26410v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26410v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:22:23"
---
# Voice Memory for Agentic Speech Recognition

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic speech recognition · voice agents · memory modeling · on-device

## 一句话总结

提出VOICE MEMORY，一种推理时自适应记忆方案，使冻结的生成式错误纠正器在流式语音识别中通过读取可编辑记忆文件实现可控修正，将有害编辑率从64%降至35%，并在10个域上使加权词错误率从8.36%降至7.52%。

## 摘要

> We present VOICE MEMORY, a inference-only scheme for agentic speech recognition: at stream time, a frozen corrector reads a single per-domain memory.md and decides per utterance whether to act on the hypothesis or abstain and keep the 1-best. Asynchronously, a score-gated optimizer revises that file through bounded edits, accepting an edit only when it strictly improves a held-out score. Extended from classical ASR-LM framework, we refer this split the listener-thinker architecture; the two roles are coupled only through the memory, so no weights change and the learned skill stays auditable and portable. Restraint turns out to be the operative skill this loop discovers: unconstrained generative error correction (GER) over-corrects, breaking correct tokens on up to 64% of its edits on financial news, and VOICE MEMORY reduces this rate to 35%. Across ten HyPoradise domains with an open corrector, VOICE MEMORY lowers weighted word error rate from 8.36% to 7.52% (7.47% with three added in-context examples) without regressing any dataset below its 1-best baseline; gains concentrate where recoverable headroom is largest, including air-travel commands (8.40% to 3.40%) and noisy far-field speech (CHiME-4, 12.69% to 10.46%). The memory transfers across corrector families and adds zero parameters to the inference path. A demo and example code are provided for future studies.
> Keywords: voice agents, memory modeling, on-device, language models, test-time adaptation

Q1: 这篇论文试图解决什么问题？

论文试图解决生成式错误纠正（GER）在流式语音识别中的过度修正问题，以及如何实现无需参数更新的领域自适应。具体来说，现有GER系统在修正时往往破坏原本正确的词，且每次推理消耗大量计算。同时，语音识别系统需要在不同领域快速适应，但微调或上下文学习成本高且不可审计。论文提出了智能体语音识别的概念：系统应根据条件选择动作（保持或修正），并通过记忆机制持续适应，满足可审计、可移植、零参数增加的要求。

Q2: 有哪些相关研究？

相关研究包括三方面：1）生成式错误纠正（GER）：利用语言模型对ASR结果进行后处理，但存在过度修正和计算成本高的问题。2）记忆增强方法：如episodic memory和memory networks，但通常需要重新训练或特定记忆结构。3）测试时适应：如prompt-based methods和few-shot learning，但每次请求重复计算。VOICE MEMORY结合了这些思路，但创新地使用可编辑的纯文本记忆文件，通过离线优化器更新，实现无权重变化的持续适应。

Q3: 论文如何解决这个问题？

论文提出VOICE MEMORY，包括两个组件：1) 推理时：一个冻结的生成式纠正器（thinker）读取域记忆文件（memory.md），该记忆以对话形式存储之前的正确修正范例。纠正器对每条话语输出修正假设或放弃（保持1最佳）。2) 异步优化器：在推理间隙，评分门控优化器分析之前的假设和修正，尝试有限编辑记忆文件（如添加、删除、更新条目），只有当编辑使保留数据的评分（如词错误率）严格改善时才接受。这样，记忆文件逐步积累有用修正，而纠正器权重不变。架构称为listener-thinker，listener为标准ASR，thinker为纠正器加记忆。记忆文件可移植到其他纠正器模型，无需额外参数。

Q4: 论文做了哪些实验？

论文在HyPoradise基准的10个领域上进行评估，包括：航空旅行命令、CHiME-4（噪声远场）、金融新闻、医学转录、会议等。主要使用MiniMax-M3作为纠正器（428B参数，23B激活），并在附录中使用Qwen3-30B-A3B复制。实验设置包括：1-best基线、常规GER、VOICE MEMORY、VOICE MEMORY + 3个上下文示例。还进行了语音翻译（GenTranslate）的迁移测试。指标包括加权词错误率（WWER）、有害编辑率（HER）、可恢复信息比率。结果验证了所有领域的一致改善。

Q5: 发现了什么实验现象？

关键实验观察：1) 无约束GER在金融新闻上64%的编辑是有害的（破坏正确token），VOICE MEMORY将此率降至35%。2) 在所有10个域上，VOICE MEMORY的WWER均不差于1-best，且平均从8.36%降至7.52%（+0.3示例后7.47%）。3) 增益分布不均：航空旅行命令从8.40%降至3.40%（相对改善59.5%），CHiME-4从12.69%降至10.46%（相对改善17.6%），而某些域如LibriSpeech改善较小（约0.1%）。4) 记忆可跨纠正器家族：Qwen3-30B上观察到类似改善趋势。5) 在语音翻译任务上，VOICE MEMORY也降低了翻译错误率，表明机制可迁移。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：1) 验证agentic memory机制是否适用于其他n-best语言任务如文本纠错、机器翻译；2) 扩展到更丰富的记忆形式（如嵌入向量、结构化知识）；3) 研究记忆文件的自动构建策略，减少人工干预；4) 在端到端ASR系统上的应用；5) 探索更多上下文示例对性能的影响；6) 分析记忆与纠正器行为的交互机制。

Q7: 总结一下论文的主要内容

本文提出VOICE MEMORY，一种用于智能体语音识别的推理时记忆驱动自适应方案。该方案不改变任何模型权重，通过一个可编辑的纯文本记忆文件和评分门控优化器，使冻结的生成式错误纠正器持续改进。在10个域上的实验显示有害编辑率大幅下降（64%→35%），加权词错误率从8.36%降至7.52%，且所有域均未退步。增益集中在可恢复信息最大的域。记忆可跨纠正器迁移，推理路径零参数增加。论文还定义了智能体语音识别的条件，并引入了可恢复信息比率和有害编辑率两个新指标。未来工作可扩展到其他序列任务。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体（agent）方向直接相关，提出了agentic ASR定义和实现

## 基本信息

- 作者：Chao-Han Huck Yang, Zih-Ching Chen, Piotr Zelasko, Zhehuai Chen, Jagadeesh Balam, Boris Ginsburg
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.SD, eess.AS
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26410v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了论文PDF检索证据（retrieved_evidence），并结合heuristic_draft和field_evidence_map完成内容重构。所有字段已根据证据锚点进行对齐。
