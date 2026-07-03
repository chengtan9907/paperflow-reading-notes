---
user_id: "cheng tan"
paper_id: 2679
arxiv_id: "2607.01733"
title: "Rethinking Speech-LLM Integration for ASR: Effective Joint Speech-Text Training by Interleaving"
institution: "Microsoft, USA"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.01733.pdf"
pdf_url: "https://arxiv.org/pdf/2607.01733"
abs_url: "https://arxiv.org/abs/2607.01733"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T22:26:13"
---
# Rethinking Speech-LLM Integration for ASR: Effective Joint Speech-Text Training by Interleaving

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：speech-llm integration · joint speech-text pre-training · interleaved sequences · automatic speech recognition

## 一句话总结

该论文提出Joint Speech-Text Interleaved Pre-Training (JSTIP)，通过交错语音-文本序列进行预训练，解决decoder-only speech-LLM模型过度专精于语音条件解码而无法有效利用文本先验知识的问题。

## 摘要

> 论文发现传统decoder-only语音-LLM集成在ASR中不能从文本先验知识中持续获益，提出JSTIP预训练策略：将语音和文本从词级或片段级对齐后交错形成统一序列，使用下一个token预测目标联合训练（包括纯语音、交错、纯文本），防止模型过度专精。使用400M参数的Conformer编码器和38k小时内部英语数据进行训练，在多个ASR基准上取得改进，并在实体识别任务中简化领域适应，达到与合成语音-文本对相当的性能。

Q1: 这篇论文试图解决什么问题？

传统decoder-only speech-LLM模型在ASR训练中倾向于过度专精于语音条件解码，导致LLM的文本先验知识未能充分利用。尤其在训练数据增多时，ASR性能提升趋缓，LLM先验贡献减弱。该论文从预训练视角重新审视语音-LLM集成，识别出这一关键限制。

Q2: 有哪些相关研究？

相关研究包括Speech-LLM集成方法（如基于encoder-decoder或decoder-only架构），以及多模态预训练（如SpeechCLIP、HuBERT+GPT等）。论文指出，先前工作多直接微调LLM或添加适配器，但未充分解决模态间学习失衡问题。JSTIP通过交错序列训练，强制模型在语音和文本间交替预测，更有效地注入文本知识。

Q3: 论文如何解决这个问题？

提出JSTIP预训练策略：（1）将语音与文本对齐，获得词级或片段级对齐；（2）将语音和文本段交错形成交替序列（如语音段-文本段-语音段...），词级交错中跳过特殊token以增强耦合；（3）使用统一的下一个token预测目标，对纯语音、交错、纯文本三种格式联合训练，防止模型只学习语音条件解码；（4）训练时对非预测部分进行loss masking。该设计使模型在预训练阶段即接触多模态度量分布，从而保持利用文本知识的能力。

Q4: 论文做了哪些实验？

使用38k小时的匿名内部英语ASR数据（约2.3B tokens，token率12.5Hz）。模型为decoder-only speech-LLM，语音编码器为400M Conformer（时间下采样8倍）。输入80维log Mel滤波器组。在多个内部测试集评估WER。对比基线包括传统ASR训练、speech-LLM微调等。此外，在实体识别任务上进行领域适应实验，使用纯领域转录文本与合成语音-文本对比较。

Q5: 发现了什么实验现象？

实验观察到：（1）随着ASR训练数据增加，传统speech-LLM方法的ASR准确度提升不如预期，LLM先验贡献减弱；（2）JSTIP在不同数据规模下均优于传统方法，尤其在低资源场景下表现显著；（3）词级交错比片段级交错带来更紧的跨模态耦合，但在连续语音对齐上更具挑战性；（4）在实体识别领域适应中，JSTIP仅使用领域转录文本即可达到与使用合成语音-文本对相当的性能，简化了适应流程。

Q6: 有什么可以进一步探索的点？

未来工作可探索：1）基于词类型的模态选择（如仅将特定词类转换为文本）；2）更细粒度的交错策略（如音素级）；3）将JSTIP扩展到多语言或多任务场景；4）在更大规模数据和模型下的扩展性分析；5）与其他预训练目标（如对比学习）的结合。

Q7: 总结一下论文的主要内容

本文重新审视面向ASR的语音-LLM集成问题，指出现有decoder-only模型在常规训练下会过度专精于语音条件解码，无法充分利用LLM的文本先验知识。为此，作者提出Joint Speech-Text Interleaved Pre-Training (JSTIP)，一种面向ASR的预训练策略，通过构建交错语音-文本序列（词级或片段级对齐），并使用统一的下一个token预测目标对纯语音、交错和纯文本数据联合训练，防止模型遗忘文本建模能力。实验使用38k小时内部英语数据训练的400M参数Conformer+LLM模型，在多个ASR测试集上取得改进，尤其在小数据量条件下。此外，在实体识别领域适应任务中，JSTIP仅需领域转录文本即可达到与合成语音-文本对相当的结果，简化了领域迁移过程。消融研究比较了词级和片段级交错的效果，显示词级交错能更紧密耦合模态但对齐更困难。论文最后指出词类型引导的模态选择和扩展性研究是未来方向。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：详细分析了词级和片段级交错对ASR性能的影响，为多模态预训练中序列设计提供指导

## 基本信息

- 作者：Ruchao Fan, Yiming Wang, Rui Zhao, Liliang Ren, Keqi Deng, Xiaoyang Chen, Ali Zare, Bo Ren, Yuxuan Hu, Junkun Chen, Yan Huang, Yelong Shen, Jinyu Li
- 机构：Microsoft, USA
- 来源：arxiv
- 主题/分类：cs.CL, eess.AS
- 日期：2026-07-03
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.01733`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了PDF检索证据（来自Qwen3嵌入检索），结合启发式草案进行综合润色和补全；对未有直接证据的细节基于合理推断。
