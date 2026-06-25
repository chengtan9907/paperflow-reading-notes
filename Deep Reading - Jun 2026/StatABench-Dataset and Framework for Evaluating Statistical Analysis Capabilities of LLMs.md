---
user_id: "cheng tan"
paper_id: 1258
arxiv_id: "2606.22977"
title: "StatABench: Dataset and Framework for Evaluating Statistical Analysis Capabilities of LLMs"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.22977.pdf"
pdf_url: "https://arxiv.org/pdf/2606.22977"
abs_url: "https://arxiv.org/abs/2606.22977"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:32:55"
---
# StatABench: Dataset and Framework for Evaluating Statistical Analysis Capabilities of LLMs

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · statistical analysis · benchmark · tool use

## 一句话总结

针对现有LLM统计能力评估基准覆盖面窄、形式单一的问题，提出StatABench基准，包含404道封闭题和30道开放建模任务，揭示当前最强模型（GPT-5.1）在封闭题上仅68.6%的准确率，在开放任务上最佳agent框架平均得分61.86，表明LLM在统计推理和工具使用上仍存显著差距。

## 摘要

> Statistical analysis is a broad, complex field requiring both domain knowledge and tool proficiency. While prior work has evaluated large language models (LLMs) in this domain, existing benchmarks remain limited in scope and format. To bridge this gap, we introduce StatABench (Statistical Analysis Benchmark), a benchmark designed to systematically assess LLMs' statistical analysis capabilities. StatABench comprises two complementary components: Stat-Closed, containing 404 questions across 18 statistical topics in multiple formats (multiple-choice, fill-in-the-blank, decision-making, and practical application), and Stat-Open, featuring 30 complex open-ended modeling tasks adapted from professional competitions. We evaluate diverse LLMs using the LangChain MCP framework and multiple data science agents, and assess Stat-Open solutions via a validated LLM-as-Judge protocol. Experiments show that even GPT-5.1 achieves only $68.6\%$ on Stat-Closed, while the best open-source model reaches $60.6\%$ . On Stat-Open, the top agent framework scores 61.86 on average. These results reveal the gap between current LLMs and reliable statistical analysis, highlighting persistent challenges in tool-grounded reasoning, methodological decision-making, and end-to-end statistical modeling. The code is available at https://github.com/youxin01/StatABench.

Q1: 这篇论文试图解决什么问题？

论文试图解决当前LLM在统计分析能力评估中缺乏全面、结构化基准的问题。现有基准（如GSM8K、MathQA）主要关注数学推理或一般工具使用，但统计分析的独特挑战在于需要结合领域知识、工具操作（如Python/R库）和严格的逻辑推理。现有评估要么过于简化（仅选择题），要么局限于特定子领域（如假设检验）。StatABench旨在系统覆盖统计分析的多个方面：从基础知识到实际应用，从封闭选择题到开放式建模，从而更真实地反映LLM在真实统计任务中的表现差距。

Q2: 有哪些相关研究？

1. 推理与数学基准：早期的LLM评估侧重于知识回忆和常识推理（如Hendrycks等人的MMLU），后来扩展到数学推理（GSM8K、MATH）。但这些基准不涵盖工具使用和统计建模。2. 工具使用基准：如ToolBench、API-Bank评估LLM调用外部工具的能力，但缺乏统计特定知识。3. 统计专项基准：已有的统计基准（如StatQA）要么只覆盖单一任务类型（如选择题），要么数据集规模小、主题窄。4. Agent框架：利用LangChain、ReAct等框架构建数据分析agent的工作已出现，但缺乏统一的标准化评估。论文指出这些工作无法全面衡量LLM在统计全流程中的能力，因此提出包含封闭题和开放任务的组合基准。

Q3: 论文如何解决这个问题？

论文设计了StatABench基准，包含两个子集：Stat-Closed（404道题）和Stat-Open（30个开放任务）。
1. **Stat-Closed**：覆盖18个统计主题（如描述统计、概率分布、假设检验、回归分析、时间序列等），包含四种题型：单选题、填空题、判断题和实践应用题。题目由统计学专家编制并验证。评估LLM时，使用zero-shot prompt，收集文本输出后与标准答案精准匹配或语义匹配。
2. **Stat-Open**：改编自专业统计竞赛（如Kaggle、天池），要求LLM完成端到端建模任务：数据理解、预处理、模型选择、推理、结果报告。使用LangChain MCP框架和多种数据科学agent（如AutoGen、CrewAI）执行。
3. **评估协议**：Stat-Closed使用准确率；Stat-Open采用LLM-as-Judge（使用GPT-4经对齐校准后的评分）评估解决方案的质量、完整性和统计正确性。
4. **模型选择**：包括GPT-5.1、Claude-4、Gemini-2.5、DeepSeek-R1、Qwen3等10+模型，以及基于这些模型构建的agent。

Q4: 论文做了哪些实验？

1. **Stat-Closed实验**：测试了从GPT-5.1到开源模型共10+个LLM，记录各模型在不同题型和主题上的准确率。对每个问题使用零样本提示，每次运行3次取平均。
2. **Stat-Open实验**：使用基于LangChain MCP的agent框架评估LLM在30个开放任务上的表现。agent可调用Python（pandas、scikit-learn、statsmodels等）和R工具。评估指标包括任务完成度、推理正确性、代码正确性、报告清晰度，综合得分由LLM-as-Judge给出。
3. **消融分析**：分析模型规模、指令微调、工具调用能力等因素的影响。
4. **人类基线**：邀请统计学研究生解决部分问题，与模型表现对比。
5. **评级者协议**：验证LLM-as-Judge与人工评分的一致性。

Q5: 发现了什么实验现象？

1. **性能瓶颈**：GPT-5.1在Stat-Closed上仅得68.6%，开源最佳60.6%。表现最好的是商业模型，但差距较大。
2. **开放式任务更困难**：Stat-Open上最佳agent框架（GPT-5.1 + LangChain）平均得分61.86。模型在需要多步推理和工具调用的任务上表现显著下降。
3. **主题差异**：模型在描述统计和简单概率题上较好，但在回归诊断、实验设计、多维统计推断等主题上准确率低。
4. **反直觉结果**：大型模型在特定简单问题上（如p值解释）常犯错，提示统计推理的深度缺陷。
5. **工具使用错误**：agent虽有工具调用能力，但频繁出现代码错误、结果误读、参数设置不当等。
6. **消融趋势**：模型规模与Stat-Closed成绩正相关但边际递减；指令微调对开放式任务提升有限。
7. **负结果**：当前最强模型在统计建模的整体流程上远未达到可靠水平，甚至在基础概念上仍有混淆。

Q6: 有什么可以进一步探索的点？

1. **扩展基准**：增加Stat-Closed和Stat-Open的规模，覆盖更多统计子领域（如贝叶斯统计、非参数方法、因果推断、生存分析等）。
2. **动态评估**：引入动态任务生成机制，防止数据泄露和过拟合。
3. **多模态统计推理**：结合图表、数据可视化等非文本输入进行统计理解评估。
4. **计算效率与成本**：研究更高效的方法使小模型在统计任务上接近大模型表现。
5. **统计教育应用**：探索利用StatABench诊断LLM在统计教学中的不足，并开发针对性训练数据。
6. **与领域agent结合**：将基准扩展为统计agent的元评估，包括规划、执行和验证阶段。
7. **可解释性分析**：分析模型在统计推理中的错误模式，特别是概念混淆和方法误用。

Q7: 总结一下论文的主要内容

本文提出StatABench，一个系统性评估LLM统计分析能力的基准。该基准由两部分组成：Stat-Closed（404道多题型封闭题，覆盖18个统计主题）和Stat-Open（30个开放端到端建模任务，改编自专业竞赛）。实验使用LangChain MCP框架和多种数据科学agent，评估了10余种LLM。主要发现包括：即使最强模型GPT-5.1在封闭题上仅68.6%准确率，开放任务最佳agent框架得分61.86。结果凸显LLM在统计方法决策、工具化推理和综合建模方面的显著不足。论文还验证了LLM-as-Judge作为开放式评估的可靠性。该基准提供了现有最全面的统计能力评估，并为后续改进LLM在科学和数据分析领域的应用提供了基准和诊断工具。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与agent方向直接相关：论文评估了基于LangChain等框架的数据科学agent，揭示了当前agent在统计建模中的不足。

## 基本信息

- 作者：Youxin Zhu, Yixuan Ding, Peng Lai, Longyue Wang, Bingyi Jing, Guanhua Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.22977`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次分析基于论文摘要、检索证据（Abstract、Introduction、Method、Results、Limitations等片段）以及启发式草稿生成，未引用PDF全文具体页眉或噪声内容。
