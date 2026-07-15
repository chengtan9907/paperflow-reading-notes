---
user_id: "cheng tan"
paper_id: 3644
arxiv_id: "2607.11074v1"
title: "ResearchQA: Benchmarking Citation-Grounded Question-Answering on Scientific Papers"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11074v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11074v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:42:09"
---
# ResearchQA: Benchmarking Citation-Grounded Question-Answering on Scientific Papers

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：citation-grounded question answering · scientific paper benchmark · large language model evaluation · citation accuracy

## 一句话总结

ResearchQA是一个包含6,211个问答对的引文基础问答基准，覆盖494篇开放获取论文、8个科学领域和4种问题类型（查找、理解、多跳、对抗），旨在评估大型语言模型在科学论文阅读中生成可验证引文支持答案的能力。

## 摘要

> Large language models are increasingly used to assist scientific reading, but existing evaluation methods often fail to detect whether answers are supported by verifiable citations. We introduce ResearchQA, a benchmark of 6,211 single-paper question-answer pairs from 494 open-access papers spanning eight domains and four question types: lookup, comprehension, multi-hop, and adversarial. ResearchQA is designed for citation-grounded evaluation: it permits multiple valid supporting passages for a claim and rewards grounded refusal when the source paper does not support an answer. We evaluate eight leading closed- and open-weight models in a citation-grounded chat-with-paper setting using a deterministic citation matcher and an LLM-based rubric evaluator. Citation-based metrics separate systems more clearly than LLM-evaluator scores: section coverage and citation accuracy vary substantially across models, while evaluator scores remain tightly compressed. We further find that open-weight models approach the best closed-model citation accuracy while achieving 3 to 6 times lower per-example latency. We release the benchmark, evaluation harness, and evaluator prompt.

Q1: 这篇论文试图解决什么问题？

论文试图解决当前大型语言模型在科学论文阅读辅助中缺乏引文可信度评估的问题。具体来说，现有的评估方法（如LLM评分、嵌入相似度）无法区分答案是否由可验证的引文支持，导致模型可能生成流畅但虚假的引用，从而传播错误信息。虽然存在一些问答基准（如QASPER、HotPotQA），但它们要么局限于Wikipedia领域从而缺乏科学论文的密集技术内容，要么依赖人工标注导致规模受限且引文验证不够严格。此外，这些基准不支持多有效支持段落，也未考虑模型应该因无法找到支持而拒绝回答的情况。因此，ResearchQA旨在提供一个可直接测量引文基础问答质量的基准，通过确定性引文匹配确保每个引用都真实存在于原文中，并允许模型在无支持时优雅拒绝，从而推动模型在科学文献阅读中的可信赖性。

Q2: 有哪些相关研究？

相关工作主要涵盖三个方面：科学论文问答、引文基础语言模型和端到端任务性能基准。在科学论文问答方面，QASPER是源自NLP论文的人工标注数据集，但规模有限且仅覆盖NLP领域；HotPotQA基于Wikipedia，缺乏科学论文的统计、方法和表格等内容。其他工作如MultiHopQA扩展了多跳推理，但引文验证不够严格。在引文基础生成方面，有工作通过自动指标（如cite-worthiness）或人工评估验证引用质量，但未嵌入基准。ResearchQA结合了这些方向的特点，通过四个关键创新脱颖而出：1) 支持多有效支持段落，承认答案可能源于多个位置；2) 包含对抗性不可回答问题，测试模型在无支持时拒绝的能力；3) 使用确定性引文匹配器进行客观验证，取代软匹配或人工判断；4) 覆盖8个领域（如生物医学、物理学、计算机科学等），比现有基准更广泛。

Q3: 论文如何解决这个问题？

论文通过以下步骤构建解决方案：首先，从开放获取论文中提取全文文本，并将其分割为段落或章节。然后，由作者编写6,211个问答对，确保问题涵盖四种类型：lookup（直接查找）、comprehension（理解）、multi-hop（多跳推理）和adversarial（对抗性，即论文不支持答案的问题）。每个问题对应一个或多个支持段落，这些段落被标注为答案的来源。在评估时，采用Chat-with-Paper范式：模型输入论文全文（或相关章节）和问题，输出答案文本并引用支持段落。评估包含两个组件：确定性引文匹配器，检查引用的段落是否真实存在于文本中并正确引用；LLM评估器，评价答案的事实准确性、完整性、拒绝合理性等。通过综合引文指标（引用精度、段落覆盖度、引文准确率、拒绝率）和答案质量指标，得到最终评分。

Q4: 论文做了哪些实验？

评估了八个大型语言模型，包括闭源模型（Gemini 3.1 Pro, Gemini 3 Flash, GPT-5.4, GPT-4.1, Claude Opus 4.7, Claude Haiku 4.5）和开源模型（GPT-OSS-120B, ZAI GLM 4.7）。每个模型在100行均匀采样的子集上进行测试，输入为论文全文和问题，输出答案及引用。使用的评估指标包括：引文精度（Cite. Precision）、段落覆盖度（Sect. Coverage）、引文准确率（Cite. Accuracy）、拒绝率（Refusal）、事实准确性（Factual）、完整性（Completeness）以及端到端延迟（Latency）。确定性引文匹配器严格检查引用文本是否出现在原文，LLM评估器基于提示对答案质量进行评分。实验记录了每个模型在各项指标上的得分，并比较了不同模型家族及快速/智能配置的性能差异。

Q5: 发现了什么实验现象？

实验现象包括：1) 引文指标（如段落覆盖度和引文准确率）对模型性能的区分度远高于LLM评估器分数，后者所有模型分数压缩在4.5-5.0之间；2) 开源模型（ZAI GLM 4.7）在引文准确率上与最佳闭源模型（GPT-4.1）相当，但延迟显著更低（约1/3）；3) 不同模型在拒绝率上差异明显，一些模型在对抗性问题上拒绝不足或过度拒绝；4) 端到端延迟差异达一个数量级，GPT-OSS-120B最快（2.9s），Claude Opus 4.7最慢（30.4s）；5) 模型快速版本与智能版本在引文指标上差距较小，但在LLM评分上更接近；6) 在对抗样本上，模型的事实准确性得分较高（4.478-5.000），表明模型倾向于给出流畅但不一定正确的回答。

Q6: 有什么可以进一步探索的点？

未来方向包括：1) 构建多模态科学理解基准，将问题扩展到图表、表格、公式等视觉内容，并实现相应的引文基础验证；2) 使用更多样化的模型家族（而非单一模型）生成问题，以减少风格、难度分布和类别边界上的偏差；3) 扩展到多论文跨文档问答场景，评估模型在整合多源信息时的引文基础能力；4) 改进引文匹配器，从确定性文本匹配提升到语义等价匹配，允许同义改写或摘要片段作为有效支持；5) 探索更细粒度的支持证据类型（如数值结果、算法步骤），并设计对应的评估协议。

Q7: 总结一下论文的主要内容

论文介绍了ResearchQA，一个用于大型语言模型引文基础问答评估的新基准。研究背景是：LLM被广泛用于辅助科学阅读，但现有评估方法（如LLM评分和嵌入相似度）无法有效检测答案是否由可验证的引文支持，导致模型可能产生虚假引用。为此，作者构建了一个包含6,211个问答对的数据集，源自494篇开放获取论文，覆盖8个科学领域（包括生物医学、计算机科学等）和4种问题类型（查找、理解、多跳、对抗）。该基准的关键设计是多有效支持段落（一个答案可引用多个段落）和对抗性不可回答问题（奖励拒绝回答）。评估采用确定性引文匹配器（严格检查引用文本是否在原文）和LLM评估器（评分答案质量）。实验在8个主流模型上进行，包括GPT-4、Gemini、Claude系列及其快速版本，以及两个开源模型（GPT-OSS-120B、ZAI GLM 4.7）。主要发现：引文指标（引用准确率、段落覆盖度）能清晰区分模型，而LLM评估器分数差异很小；开源模型ZAI GLM 4.7在引文准确率上接近最佳闭源模型，同时延迟低3-6倍；不同模型在对抗性拒绝上表现差异大；端到端延迟范围从2.9秒（GPT-OSS-120B）到30.4秒（Claude Opus 4.7）。论文还指出了局限性：问题仅基于文本，不包括图表；问题生成可能受单一模型影响；引文匹配是确定性的，可能遗漏语义等价引用。总体而言，ResearchQA为科学文献阅读辅助的评估提供了更严格和区分度更高的方法，并推动了开源模型在该任务中的应用。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对LLM科学文献评估方法具有直接参考价值，特别是引文可靠性指标的设计。

## 基本信息

- 作者：Saba Imran, Debanjum Singh Solanky
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11074v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence）和字段证据映射（field_evidence_map），优先采用了原始论文摘要和相关片段的语义检索结果。
