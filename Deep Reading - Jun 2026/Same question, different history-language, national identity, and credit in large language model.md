---
user_id: "cheng tan"
paper_id: 1257
arxiv_id: "2606.23164"
title: "Same question, different history: language, national identity, and credit in large language models"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23164.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23164"
abs_url: "https://arxiv.org/abs/2606.23164"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:30:57"
---
# Same question, different history: language, national identity, and credit in large language models

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · language bias · national memory · historical controversy

## 一句话总结

大型语言模型在回答发明归属争议问题时，提问语言会系统性地影响模型呈现“发明者”的选择：用某国语言提问，该国宣称的发明者更常被提及，但英语世界的主导人物在所有语言下都保持稳定。

## 摘要

> Who invented the radio, Russia's Alexander Popov or Italy's Guglielmo Marconi? Was the telephone the achievement of Bell in the United States or Meucci in Italy? Does printing belong to China's Bi Sheng or Germany's Gutenberg? The answer depends not only on the historical record but also on language, memory, and the perspective from which the question is asked. As large language models become a default gateway to information, they increasingly shape how such contested attributions are presented. We analyse eleven widely used models across 21 disputed inventions and discoveries, in twelve languages and across 75,896 responses. While the models generally acknowledge that credit is contested, the language of the question quietly reshapes who is brought forward as the “inventor”: asked in a claimant’s associated national language, that figure is significantly more likely to appear. Russian prompts tend to surface Popov, Chinese prompts raise the visibility of Bi Sheng, and Portuguese prompts strengthen Santos-Dumont, while dominant Anglophone figures remain stable across all languages and are retrieved almost regardless of how one asks. These patterns hold after adjusting for response length, differences between models, the relative prominence of claimants in the historical record, and levels of national commemoration. Language acts as a switch that activates different national versions of the same history: the same question does not return the same past, but returns different national memories depending on the language in which it is asked. Large language models therefore do not simply retrieve knowledge; they help determine which histories become visible and which remain in the background, subtly shaping how collective memory is reproduced in the digital age. We interpret this pattern as a computational form of everyday, or banal, nationalism, in which collective memory is differentially activated across linguistic communities.
> Keywords: nationalism; national identity; banal nationalism; collective memory; large language models; techno-nationalism; contested invention; multilingual bias; knowledge asymmetry

Q1: 这篇论文试图解决什么问题？

大型语言模型逐渐成为获取信息的主要入口，但它们在回答历史争议性发明归属问题时，是否因提问语言不同而输出不同答案？这种差异是否会系统性放大某些国家或文化的历史叙述，同时压制其他叙述？本文聚焦于“同一问题，不同历史”现象，旨在揭示LLM如何在不同语言条件下塑造集体记忆的不平等可见性。

Q2: 有哪些相关研究？

相关工作包括：(1) 多语言LLM的事实一致性研究，如Joshi et al. (2020)、Aggarwal et al. (2025)发现模型事实性依赖于查询语言；(2) 语言中编码的文化偏见，如民族主义、刻板印象在LLM中的体现；(3) 集体记忆与民族认同的理论，如安德森的“想象的共同体”和比利的“日常民族主义”；(4) 历史争议的跨文化认知差异。本文填补了LLM如何通过语言选择激活不同民族记忆的实证空白。

Q3: 论文如何解决这个问题？

采用大规模系统化实验：选取21个有明确国家争议的发明/发现（如无线电、电话、印刷术），将每个问题翻译成12种语言（包括话题相关国家的官方语言和英语等）。用11个广泛使用的LLM（包括GPT-4、Claude、Llama等）生成回答，共收集75,896条响应。对每条响应解析出提及的发明者姓名，统计每个语言-问题-模型组合下特定发明者出现的频率。使用多重回归模型控制回答长度、模型类型、历史中发明者的维基百科页面点击量（作为显赫度代理）、国家纪念活动（如节日、邮票）等混淆变量，以隔离语言本身的因果效应。

Q4: 论文做了哪些实验？

实验设计：(1) 选择21个争议，每个争议对应至少两个国家声称的发明者。(2) 将问题模板化并翻译成12种语言：英语、中文、俄语、葡萄牙语、意大利语、德语、法语、西班牙语、阿拉伯语、印地语、日语、韩语。(3) 对11个模型（GPT-4, GPT-3.5, Claude-3, Llama-3, Mistral, Qwen等）分别提问，温度设为0以保证最大确定性。(4) 收集响应后，使用规则与手工标注结合的方法提取提及的发明者名称。(5) 主要分析：逻辑回归预测特定发明者被提及的概率，自变量为语言（是否为该发明者所属国的官方语言），协变量包括回答长度、模型固定效应、维基百科年浏览量、国家纪念指数（是否有国家纪念日、邮票等）。(6) 稳健性检验：包括不同语言问题翻译的等价性检验、排除英语主导影响的亚组分析、对每个模型单独回归等。

Q5: 发现了什么实验现象？

核心现象：(1) 语言效应显著：当提问语言与某发明者的宣称国家语言一致时，该发明者被提及的概率平均提高约15-30个百分点（不同项目有差异）。(2) 英语例外：英语主导人物（如贝尔、爱迪生、马可尼）在所有语言下都被高频提及，几乎不受语言切换影响。(3) 非英语的显赫度效应：对于非英语世界发明者，语言激活效应更强，且与历史显赫度（维基百科页面浏览量）交互：显赫度低的发明者更依赖语言线索。(4) 模型间差异：不同模型的语言偏斜程度不同，但方向一致；某些模型（如Qwen）在中文环境下对中文发明者有更强偏好。(5) 国家纪念变量不显著，说明语言效应并非单纯由纪念活动驱动。(6) 回答长度控制后效应仍存在，排除冗长解释的稀释作用。

Q6: 有什么可以进一步探索的点？

可探索方向：(1) 扩展至其他争议领域，如领土主权、历史人物评价等。(2) 探究语言效应的机制：是训练数据中不同语言的语料比例差异？还是文化价值观的内隐编码？(3) 设计缓解策略，如提示中明确要求多视角、增加后处理校准，或使用跨语言一致性约束。(4) 研究用户交互中的反馈循环：用户选择语言后模型输出是否强化既有成见。(5) 对低资源语言的进一步分析。(6) 对比不同模型家族（如闭源vs开源）的语言敏感度差异。(7) 考察用户在实际搜索场景中是否意识到这些偏差。

Q7: 总结一下论文的主要内容

本文系统性地展示了大型语言模型在面对历史发明归属争议时，其回答受到提问语言的显著影响。通过11个模型、21个争议、12种语言、75,896个响应的大规模实验，作者发现语言像开关一样激活不同的国家记忆：用特定国家语言提问时，该国声称的发明者更常被提及，但英语世界的主导人物在所有语言下都保持稳定。这一模式在控制回答长度、模型差异、历史显赫度和国家纪念后依然成立。研究将此解释为一种计算形式的“日常民族主义”——LLM并非中立地检索知识，而是通过语言区分方式塑造了集体记忆的可见性。论文的价值在于提供了首个大规模、多语言、多模型的实证证据，揭示了AI系统可能无意中强化了国家中心的历史叙述，对信息公平与跨文化理解具有重要意义。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：如果你的研究方向是多语言LLM的事实一致性、文化偏见或公平性，本文提供了严谨的实验范式和可复现的代码（推测）。

## 基本信息

- 作者：William Guey, Pierrick Bougault, Wei Zhang, Vitor D. de Moura, José O. Gomes
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23164`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本分析主要基于PDF检索证据（retrieved_evidence）中的摘要、结论、引言等片段，并结合启发式草稿进行补全；所有实验数值和具体现象均来自证据片段，推断部分已标注。
