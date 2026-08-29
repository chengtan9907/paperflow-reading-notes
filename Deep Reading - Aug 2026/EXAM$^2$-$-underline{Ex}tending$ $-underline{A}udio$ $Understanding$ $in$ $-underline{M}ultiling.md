---
user_id: "cheng tan"
paper_id: 9185
arxiv_id: "2608.23758v2"
title: "EXAM$^2$: $\\underline{Ex}tending$ $\\underline{A}udio$ $Understanding$ $in$ $\\underline{M}ultilingual$ $and$ $\\underline{M}ultimodal$ $Analysis$"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23758v2.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23758v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:12:57"
---
# EXAM$^2$: $\underline{Ex}tending$ $\underline{A}udio$ $Understanding$ $in$ $\underline{M}ultilingual$ $and$ $\underline{M}ultimodal$ $Analysis$

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multilingual audio understanding · multimodal benchmark · large audio language models · multiple-choice question answering

## 一句话总结

EXAM^2是一个覆盖六种语言、融合语音/声音/音乐/混合音频与视觉图像的多模态音频理解基准，用于揭示大音频语言模型在多语言和跨模态场景下的性能差距，并验证轻量融合模型Gemma3n-EXAM^2的显著提升。

## 摘要

> Recent large audio language models (LALMs) have achieved impressive progress in audio understanding. However, existing evaluations remain largely constrained to English and narrow audio domains. Prior benchmarks typically focus on a single audio modality, i.e., speech, sound, or music, limiting the systematic investigation into how these models generalize across diverse visual scenarios. In this paper, we introduce EXAM$^2$, a benchmark for multilingual and multimodal audio understanding spanning six languages and multiple modalities, including speech, sound, music, mixed-audio settings, and visual images. By incorporating visual information alongside heterogeneous audio inputs, EXAM$^2$ enables more realistic evaluation of scene-aware audio reasoning and cross-modal comprehension. EXAM$^2$ comprises $5,667$ multiple-choice questions, $22,614$ image instances, and $135,684$ multilingual translations. We evaluate state-of-the-art open-source and proprietary LALMs as well as multimodal LLMs, revealing substantial performance gaps in multilingual and cross-modal understanding. Furthermore, we propose Gemma3n-EXAM$^2$, a lightweight fusion-model fine-tuned on EXAM$^2$-train, achieves up to $12.4\%$ improvement in multilingual settings and $21.7\%$ gains in multimodal evaluation over a strong baseline. Empirical results establish EXAM$^2$ as a challenging benchmark and pioneer future multilingual and multimodal audio intelligence research.

Q1: 这篇论文试图解决什么问题？

论文指出当前LALMs评估的核心问题有三：（1）语言覆盖不均——大多评估以英语为中心，多语言音频理解能力缺乏系统考察；（2）模态单一——已有基准往往只关注语音、声音或音乐中的一种，无法反映真实场景中音频混杂、视觉信息并存的复杂情况；（3）缺乏统一的场景感知评估框架——音频理解往往与视觉场景相关联，现有基准难以衡量模型在跨模态推理上的表现。这些问题导致研究者无法准确判断模型在真实多语言、多模态环境中的泛化能力，也阻碍了该领域的发展。因此需要构建一个覆盖多语言、多模态、多种音频类型和视觉场景的统一基准，以系统揭示现有模型的短板。

Q2: 有哪些相关研究？

相关研究可大致分为三类。一类是音频理解基准，例如语音识别、音频字幕、声音事件分类等任务集，但通常只覆盖英语且面向单一模态。另一类是多模态大语言模型（MLLM）及其评估基准，它们将图像与文本结合，但音频模态的加入较少。还有一类是近期的大音频语言模型（LALM）工作，它们通过指令微调实现通用音频问答，但评估仍以英语和单模态为主。本文在总结这些工作的基础上，指出缺乏一个同时涵盖多语言、混合音频和视觉信息的综合评估平台。合理推断：相关工作还涉及多语言语音翻译、跨模态表示学习等，但具体文献未在摘要中列出。

Q3: 论文如何解决这个问题？

为了解决上述问题，论文提出EXAM^2——一个多语言多模态音频理解基准。其关键设计包括：（1）统一的多项选择问答（MCQ）评估框架，使跨模型比较更标准化；（2）数据覆盖六种语言（英语等，具体语言列表需查正文）和多类音频输入，包括语音、声音、音乐、混合音频，并配以视觉图像，模拟真实场景；（3）构建了包含5,667道题目、22,614个图像实例、135,684个多语言翻译的大规模数据集。在此基础上，作者进一步提出Gemma3n-EXAM^2，一种轻量级融合模型，基于Gemma3n进行多模态特征融合和指令微调，以适配EXAM^2的多语言多模态任务。该模型在多个语言和跨模态设置上相较强基线获得显著提升。

Q4: 论文做了哪些实验？

论文在EXAM^2上评估了多个当前先进的闭源和开源LALMs以及多模态LLMs，包括类似Phi-4-multimodal-instruct的模型（证据中提及，具体清单需查原文）。评估协议使用多项选择题，涵盖六种语言的单模态和跨模态任务。此外，作者将Gemma3n-EXAM^2在EXAM^2训练集上微调，并与强基线比较，衡量多语言和多模态场景下的性能提升。实验还采用可复现的代码库和评测脚本。具体实验设置（如训练/测试划分、指标等）在摘要中未完全展开，但可预期包含准确率等指标。

Q5: 发现了什么实验现象？

实验揭示了几个重要现象：（1）现有模型在多语言和跨模态理解上存在显著性能差距，表明当前LALMs在多语言音频理解上远未饱和；（2）Gemma3n-EXAM^2在多语言设置上较基线最高提升12.4%，在多模态评估中提升21.7%，证明针对多语言多模态数据的融合微调能带来明显收益；（3）证据片段中另出现“+21.65% average multilingual improvement”的表述，可能指平均多语言改进，与摘要中的12.4%可能在统计口径上不同（例如最大提升vs平均值），需要查看原文确认；（4）从局限性中可知，当前评测偏向准确性而非效率，意味着推理速度未纳入主要考量，未来模型在效率上或还面临挑战。

Q6: 有什么可以进一步探索的点？

未来方向可从多个角度展开：（1）将完整EXAM^2数据集投入训练，并探索更高效的训练方法以充分利用数据；（2）在评估中纳入推理速度和部署约束，平衡准确性与效率；（3）扩展语言数量与音频类型，引入更多真实场景噪声和口音；（4）改进跨模态推理的机制，例如联合音频-图像编码器设计；（5）将基准拓展到其他任务形式（如生成式问答、音频字幕）；（6）结合智能体（agent）系统，使音频理解驱动多模态交互决策；（7）在科学应用（如生物声学分析）中测试多语言多模态音频理解的价值。

Q7: 总结一下论文的主要内容

本文针对大音频语言模型（LALMs）评估中存在的英语中心、单模态和场景缺失问题，构建了多语言多模态音频理解基准EXAM^2。该基准统一使用多项选择问答（MCQ）框架，覆盖六种语言，包含语音、声音、音乐、混合音频以及视觉图像，共5,667道问题、22,614个图像实例和135,684个多语言翻译，能更真实地评估场景感知音频推理和跨模态理解。论文在EXAM^2上评测了多种闭源/开源LALMs和多模态LLMs，揭示了多语言和跨模态理解中的显著性能差距。为了缩小这一差距，作者提出轻量级融合模型Gemma3n-EXAM^2，利用Gemma3n架构进行微调，在多语言设置上最高提升12.4%，在多模态评估中提升21.7%。实验结果表明EXAM^2是一个有挑战性的基准，为未来多语言多模态音频智能研究开辟了道路。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对智能体（agent）研究：EXAM^2提供了多语言多模态音频理解的评测范式，可帮助构建具备场景感知能力的语音交互代理。

## 基本信息

- 作者：Jiawen Wang, Xiaoxue Gao, Zi Haur Pang, Nancy F. Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SD, cs.AI
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23758v2`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据（检索片段），并结合摘要与元数据信息整理，部分细节基于合理推断。
