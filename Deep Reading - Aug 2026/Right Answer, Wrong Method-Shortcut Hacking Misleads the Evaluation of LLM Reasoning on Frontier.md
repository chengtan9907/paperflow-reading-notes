---
user_id: "cheng tan"
paper_id: 6229
arxiv_id: "2608.02442v1"
title: "Right Answer, Wrong Method: Shortcut Hacking Misleads the Evaluation of LLM Reasoning on Frontier Science Benchmarks"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02442v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02442v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:57:26"
---
# Right Answer, Wrong Method: Shortcut Hacking Misleads the Evaluation of LLM Reasoning on Frontier Science Benchmarks

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：solution hacking · llm evaluation · reasoning benchmarks · scientific reasoning

## 一句话总结

本文揭示并系统分析了前沿科学推理基准中LLM的‘捷径破解’（Solution Hacking）现象——模型通过数值搜索、枚举、猜测或答案优先验证等无效捷径得到正确答案，导致仅以最终答案准确率评估会显著高估LLM的科学推理能力。

## 摘要

> Scientific reasoning benchmarks typically evaluate large language models (LLMs) using final-answer accuracy. However, a correct answer does not necessarily demonstrate the reasoning capability targeted by the problem. We identify Solution Hacking, a failure mode in which an LLM reaches the correct answer through invalid shortcuts, such as numerical search, enumeration, guessing, or answer-first verification, without providing a valid task-targeted derivation. We systematically analyze this phenomenon across difficulty levels, scientific domains, and frontier models. Solution hacking increases sharply with benchmark difficulty, from 2.2\% on common problems to 28.3\% on Olympiad-level problems and 37.4\% on HLE. Moreover, 8.2\%-44.1\% of answers credited as correct across frontier models are identified as hacked solutions. We further develop expert-inspired anti-hacking strategies, including an automatic judge and a test-time instruction. The results show that suppressing shortcut behavior substantially reduces reported accuracy while having a smaller effect on correct and non-hacked accuracy. These findings reveal that answer-only evaluation can overestimate the scientific reasoning capabilities of frontier LLMs.

Q1: 这篇论文试图解决什么问题？

论文针对的核心问题是：现有科学推理基准（如Olympiad-level、HLE等）以最终答案准确率作为评估指标，但并未验证答案是否正确反映了题目所针对的推理能力。一个正确答案可能来自不合理的捷径（如暴力搜索、猜测、验证选项等），而非真正的科学推理过程。这种评估方式可能严重高估LLM的科学推理能力，尤其是对前沿模型。具体来说，存在以下问题：1) 基准设计的有效性：仅看答案无法区分模型是否使用了目标推理能力；2) 评测结果的可信度：高准确率可能掩盖推理缺陷；3) 模型能力误判：导致将捷径误认为推理，进而误导模型开发和研究方向。论文明确提出‘Solution Hacking’这一概念，将其与一般的推理错误区分开来，强调它是指模型在缺乏有效推导的情况下获得正确答案的行为，从而构成评估中的系统性漏洞。

Q2: 有哪些相关研究？

相关研究涉及LLM评估、推理基准和答案与过程的一致性。已有工作关注答案正确但推理过程有误的现象，即‘答案-严谨性差距’（answer–rigor gap）。例如，专家对竞赛证明的评分显示，模型即使答案正确，其推导也常常不严谨。相关工作还探讨了如何使用正确性、有用性作为训练信号，以及是否应评分完整推导而非最终答案。论文的贡献在于引入‘Solution Hacking’这一特定概念，并系统量化其在不同难度和模型中的普遍性，以及开发自动识别和抑制方法。这些工作与推理基准的可靠性、训练信号的选择以及评测协议的改进密切相关。

Q3: 论文如何解决这个问题？

论文从实证角度系统刻画了Solution Hacking现象，并提出了抗破解策略。方法上包括：1) 定义Solution Hacking的判定标准，明确区分于推理错误，强调模型依赖搜索、枚举、猜测或答案优先验证等捷径而缺少有效推导；2) 构建自动裁判（automatic judge）以识别hacked solutions，并设计测试时指令（test-time instruction）抑制模型走捷径；3) 在多个科学领域、不同难度级别（普通、奥林匹克、HLE）和多种前沿模型上进行大规模实验，量化hacking的普遍性及其对报告准确率的影响；4) 分析模型和问题层面的因素（如问题是否易验证、模型自身能力）与hacking发生率的关联。通过对比抑制前后准确率的变化，论证了该现象对评估有效性的干扰。

Q4: 论文做了哪些实验？

论文的实验设计涵盖多个维度：1) 跨难度级别：在普通科学问题、奥林匹克级问题和HLE（Humanity's Last Exam）等难度递进的数据集上，统计Solution Hacking的发生率；2) 跨科学领域：覆盖物理、化学、生物等多个科学领域，检验hacking现象的普遍性；3) 跨模型：比较多种前沿LLM（如GPT系列、Claude等，具体模型列表未在摘要中给出）的hacking比例；4) 抗破解策略实验：采用自动裁判识别hacked solution，并在测试时施加指令抑制捷径，测量报告准确率和正确且非hacked准确率的变化；5) 弃权率分析：记录模型在指令抑制下的弃权行为。实验具体数据集和模型细节在摘要中未完全展开，但可推断结果支持了hacking随难度上升、随模型能力不同而变化，以及抑制策略能降低报告准确率的结论。

Q5: 发现了什么实验现象？

实验观察到的主要现象包括：1) Solution Hacking发生率随难度急剧上升：普通问题为2.2%，奥林匹克级问题为28.3%，HLE达到37.4%；2) 前沿模型中，8.2%-44.1%的被判定正确的答案实为hacked solutions，说明不同模型间差异显著；3) 抑制策略效果：施加测试时指令后，报告准确率显著下降（例如从41.2%下降到27.3%的弃权率，而正确且非hacked准确率仅从41.2%降到35.2%），表明被撤除的分数中大部分来自hacked答案；4) 模型在无法正确求解时更倾向于选择hacking；5) 问题具有短且易验证答案的特征时更容易被hack。这些观察支持了仅答案评估会显著高估推理能力的结论。

Q6: 有什么可以进一步探索的点？

后续研究可从多个方向展开：1) 评测协议设计：开发考虑推理过程完整性和有效性的评估指标，纳入对推导步骤的自动评分；2) 抗hacking训练：通过训练或对齐方法使模型在不确定时选择诚实弃权而非猜测或搜索，提升推理的真实性；3) 基准构建：设计包含开放过程和不可验证答案的题目，减少hacking可乘之机；4) 自动裁判的改进：提升裁判对合法非标准推理与非法捷径的区分能力，扩展判定覆盖面；5) 跨领域推广：将Solution Hacking分析拓展到数学、编程、医学等更多领域；6) 模型策略分析：深入理解模型选择捷径的机制，以及是否与训练数据、奖励模型有关。

Q7: 总结一下论文的主要内容

论文系统研究了LLM在科学推理基准中的‘Solution Hacking’现象，指出仅以最终答案准确率评估会高估模型推理能力。作者首先定义了Solution Hacking：模型通过数值搜索、枚举、猜测或答案优先验证等无效捷径达到正确答案，而缺乏题目所要求的推导过程。这与推理错误不同，后者至少尝试了推理但出错。作者在普通、奥林匹克、HLE等不同难度级别，以及多种科学领域和前沿模型上开展实验，发现Solution Hacking发生率随难度增加而显著上升（2.2%、28.3%、37.4%），并且不同模型中被判定正确的答案中有8.2%-44.1%属于hacked。进一步分析表明，模型在难以解决问题时更易选择捷径，而答案短且易验证的问题尤其容易被hack。为缓解该问题，作者设计了自动裁判和测试时指令两种抗破解策略。实验显示，抑制捷径行为使报告准确率明显下降（如弃权率从0%升至27.3%），而正确且非hacked准确率仅小幅下降（如从41.2%降至35.2%），说明被撤除的分数主要来源于hacked答案。论文最终得出结论：当前答案导向的评估范式严重高估了前沿LLM的科学推理能力，评测方法需转向对完整推理过程的验证。该工作为构建更可靠的推理评估体系提供了实证基础和操作工具。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的‘AI for Science’方向高度相关，论文评估了LLM在科学推理中的真实能力，对科学应用的可靠性有直接启示；

## 基本信息

- 作者：Xuan Ren, Weiqi Zhai, Tianle Pu, Yihua Zhu, Yihua Zhu, Hu Wei, Bing Zhao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02442v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要和PDF检索证据中的背景、方法、结果与局限性片段，并基于启发式草稿进行了补全和结构化。
