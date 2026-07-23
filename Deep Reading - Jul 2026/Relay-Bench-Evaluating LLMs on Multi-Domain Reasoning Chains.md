---
user_id: "cheng tan"
paper_id: 5172
arxiv_id: "2607.18438"
title: "Relay-Bench: Evaluating LLMs on Multi-Domain Reasoning Chains"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.18438.pdf"
pdf_url: "https://arxiv.org/pdf/2607.18438"
abs_url: "https://arxiv.org/abs/2607.18438"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T14:01:06"
---
# Relay-Bench: Evaluating LLMs on Multi-Domain Reasoning Chains

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-domain reasoning · benchmark · large language model · composite problems

## 一句话总结

Relay-Bench是一个纯文本基准，用于评估LLM在单个提示中完成跨领域多步推理链的能力，结果显示当前最佳模型GPT-5.5仅达到43.3%的准确率。

## 摘要

> Introducing Relay-Bench, an unsaturated, holistic, text-only benchmark that measures LLMs' ability to complete an assortment of tasks from distinct domains in a single prompt. The leading model, GPT-5.5 (xHigh), scores 43.3%. The test set entirely consists of composite problems: groups of single-domain subproblems that are strung together into challenges that require reasoning across multiple domains in combination. Many of these problems then have layers of complexity added through prompt encoding and deliberate context bloat. Domains tested include visual reasoning, coding, math, information extraction (with a focus on web search), problem-solving, general knowledge, and data analysis. No restrictions are imposed outside of the model harness, and models are explicitly encouraged to leverage code-execution, web searches, and all available tools. All problems are composed of two to thirteen subproblems and do not require multi-modal input or output.

Q1: 这篇论文试图解决什么问题？

当前大型语言模型评估基准通常聚焦于单领域任务（如数学、编码、问答），或简单组合，无法全面反映模型在实际应用中可能遇到的跨领域、多步骤推理能力。随着模型在现有基准上达到饱和（如GPQA、MATH-500等得分超过90%），亟需一种更具挑战性的评估方法。Relay-Bench旨在填补这一空白，通过复合问题设计，要求模型在同一提示中完成多个不同领域的子问题，从而测试其综合推理能力和可靠性。该基准强调未饱和性，以区分不同模型在复杂推理链上的表现差异。

Q2: 有哪些相关研究？

现有的LLM评估基准包括单领域问答类（如GPQA、MATH-500、AIME 2025、Humanity's Last Exam），这些基准在各自领域取得了高分，但未能模拟需要交叉多个领域的复合任务。一些基准如AgentBench或SWE-bench涉及多步任务，但通常限定在编码或软件工程领域。多模态基准如MMMU评估跨模态推理，但Relay-Bench专注于纯文本的跨领域推理。已有工作也研究了工具使用和代理工作流，但缺乏标准化、未饱和的复合推理评估。Relay-Bench通过构建由多个不同领域子问题串联而成的复合问题，提供了独特的评估维度，同时保持纯文本格式以兼容各类模型。

Q3: 论文如何解决这个问题？

论文通过以下步骤构建基准：(1) 问题生成：人工或自动生成复合问题，每个问题由2-13个不同领域的子问题组成。子问题不仅包括专家级任务（如编码、数学），也包含通用任务（如翻译、字符串操作）。初始问题集经预评估以筛选出能区分模型性能的问题，对于未导致模型失败的问题，进一步增加子问题或合并到更大的复合问题中，以提高难度。(2) 复杂度增加：通过提示编码（如将指令隐藏在代码块或Base64中）和上下文膨胀（在提示中加入冗余信息）来增加推理难度，迫使模型在长上下文中保持专注。(3) 评估框架：采用统一的模型框架，不限制工具使用，明确允许代码执行和网络搜索。设置超时和工具调用限制，记录模型终止和失败情况。评分基于最终答案的准确性。(4) 兼容性：基准为纯文本格式，不依赖多模态输入或专有API特性，便于新模型的评估。

Q4: 论文做了哪些实验？

论文使用Relay-Bench评估了多种前沿LLM，包括专有和开源模型。实验采用统一的模型框架，允许模型使用Python代码执行、网络搜索等工具。主要测量指标为整体准确率。实验发现，最佳模型GPT-5.5 (xHigh) 得分43.3%，远低于现有基准的饱和水平（如>90%）。许多模型，即使推理努力设置为最大，也经常在完成前终止或达到工具调用次数限制，表明基准的高度挑战性。论文还报告了模型在不同复合问题复杂度下的表现，以及在不同领域子问题上的成功率和失败模式。不过，由于资源限制，基准规模有限，仅包含一组复合问题，结果可能受特定问题选择影响。

Q5: 发现了什么实验现象？

实验揭示以下现象：(1) 基准显著区分了不同模型，最好和次好之间可能存在较大差距。(2) 模型在面对复合问题时，经常因工具调用超限或自我终止而失败，说明模型在长期多步任务中缺乏鲁棒性。(3) 提示编码和上下文膨胀有效增加了难度，模型在处理隐藏指令和干扰信息时表现下降。(4) 不同子问题领域的组合可能引发新的错误模式，例如模型在数学子问题成功后，在后续常识子问题上出错。(5) 工具使用虽被鼓励，但部分模型可能因错误调用或无限循环而失败。(6) 模型在单一领域上的表现与其在复合问题中的表现并非强相关，提示需要专门的复合推理评估。

Q6: 有什么可以进一步探索的点？

未来工作可从以下方面展开：(1) 扩大基准规模，增加更多复合问题和覆盖更多领域。(2) 引入更复杂的复合结构，如树状或图状依赖。(3) 对工具使用进行沙盒化或规范化，提高评估的可重复性。(4) 研究模型在复合问题上的失败原因，改进推理策略。(5) 扩展至多模态输入输出，模拟更复杂的应用场景。(6) 将基准与代理工作流评估结合，分析模型在自主任务执行中的表现。(7) 探索自动问题生成方法，减少人工成本。

Q7: 总结一下论文的主要内容

本文提出Relay-Bench，一个用于评估大型语言模型跨领域多步推理能力的纯文本基准。针对现有评估体系在复合推理测量上的缺失，Relay-Bench通过由多个不同领域子问题串联而成的复合问题，模拟需要跨域知识整合的复杂任务。每个复合问题包含2至13个子问题，涵盖视觉推理、编码、数学、信息提取、问题解决、常识知识和数据分析等领域。许多问题还采用提示编码（如隐藏指令）和上下文膨胀（添加无关信息）增加实际难度，迫使模型在长上下文中维持注意力和任务连贯性。基准设计强调未饱和性、可重复性和兼容性，不限制工具使用，允许模型调用代码执行和网络搜索，以反映真实应用中的条件。问题生成过程遵循迭代淘汰策略：先构建一组候选复合问题，通过预评估筛选出至少导致部分模型失败的问题，然后对未失败的问题进行增强（增加子问题或合并），直到所有问题都具有区分度。评估结果表明，即使是最先进的模型，在Relay-Bench上也仅达到43.3%的准确率（GPT-5.5 xHigh），远低于现有单领域基准的饱和水平。许多模型在推理过程中因工具调用限制或自我终止而提前结束，凸显了当前模型在长时间、多步推理中的鲁棒性不足。论文还分析了模型在不同子问题组合上的表现，指出复合问题的失败往往源于个别子问题的错误，但错误模式具有一定的随机性。总的来说，Relay-Bench填补了LLM评估在跨领域复合推理方面的空白，为研究模型综合能力提供了有价值的工具。但其规模有限，且评估依赖于特定模型框架，未来需进一步扩展和标准化。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：作为LLM复合推理评估基准，与代理、多步推理研究高度相关

## 基本信息

- 作者：Liam Swayne
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.18438`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据片段（包括引言、方法、结果、限制等），并结合启发式草稿进行补充和修正。
