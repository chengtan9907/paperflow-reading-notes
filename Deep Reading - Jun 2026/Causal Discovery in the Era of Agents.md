---
user_id: "cheng tan"
paper_id: 1272
arxiv_id: "2606.23608"
title: "Causal Discovery in the Era of Agents"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23608.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23608"
abs_url: "https://arxiv.org/abs/2606.23608"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:36:49"
---
# Causal Discovery in the Era of Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：causal discovery · large language model · agentic AI · causal learning

## 一句话总结

本文提出智能体应辅助因果发现流程而非替代，因果结论必须基于数据、假设和算法，并通过causal-learn+平台实例化该原则。

## 摘要

> Recent attempts to combine large language models (LLMs) with causal discovery ask models to infer pairwise directions, propose graph structures, or inject language-model outputs as priors and constraints. These approaches promise faster analysis, but they also obscure whether a causal evidence is supported by data and assumptions or by textual associations, prompt artifacts and hallucinated mechanisms. We argue for a different role for agents in causal discovery. Agents should inspect data, retrieve context, explain method assumptions and clarify graph outputs, but they should not supply edges, orientations, priors, constraints or causal conclusions. We propose the principle that agents assist the workflow, while causal claims remain grounded in data, explicit assumptions, formal algorithms, diagnostics and user or domain-expert decisions. We instantiate this principle in causal-learn+, an online platform that coordinates data analysis, preprocessing, method recommendation, expert-knowledge incorporation, formal discovery and interpretation around the algorithmic ecosystem of causal-learn. A case study on Big Five personality data illustrates agent-assisted pipeline of causal discovery without turning language-model unreliability into causal evidence. The platform is available at causallearn.com.
> Keywords: causal discovery, agentic AI, causal learning, reliable AI

Q1: 这篇论文试图解决什么问题？

当前将LLM与因果发现结合的方法（如直接推断方向、生成图结构、注入语言模型输出作为先验）存在根本问题：因果发现需要关于因果关系的证据，而语言模型仅学习文本模式。这些方法使得因果证据的可信度被污染，难以区分结论是来自数据与假设还是来自文本关联、提示伪影或幻觉机制。论文试图重新定义智能体在因果发现中的角色，以避免LLM不可靠性干扰因果结论。

Q2: 有哪些相关研究？

1. LLM直接参与因果发现：包括让LLM推断变量间方向（如Pairwise方向预测）、提出完整图结构（如LLM作为图生成器）、将LLM输出作为先验知识或约束注入标准因果发现算法。这些方法声称加速分析，但未解决证据来源混淆问题。
2. 传统因果发现方法：如基于条件独立性检验（PC算法）、基于评分（GES）、基于函数因果模型（LiNGAM）等，依赖数据分布假设和统计检验。
3. 智能体辅助科学发现：将智能体用于数据科学工作流（如自动机器学习、数据清洗），但较少聚焦因果发现领域中智能体角色的明确界定。
本文批评现有LLM+因果发现工作忽视了因果可靠性的基础，提出更保守的原则。

Q3: 论文如何解决这个问题？

论文提出核心原则：智能体辅助工作流，因果结论必须基于数据、显式假设、形式化算法、诊断结果和用户/专家决策。智能体的职责被明确定义为正面的：
1. 数据检查与预处理：自动检测缺失值、异常值、数据类型，建议预处理步骤。
2. 方法推荐：根据数据特征（样本数、变量类型、分布假设）推荐合适的因果发现算法。
3. 专家知识融入：通过自然语言或结构化输入引导用户表达领域知识，转换为算法约束（如背景知识图、强制边/禁止边）。
4. 形式化发现执行：调用causal-learn中的算法运行发现，不替换搜索模块。
5. 结果解释与通信：解释图符号、总结假设、生成诊断报告。
论文实例化为causal-learn+平台，一个在线agentic系统，围绕开源causal-learn库构建，提供Web界面让用户通过智能体助手完成完整因果发现流程。

Q4: 论文做了哪些实验？

论文未进行传统对比实验，而是提供了一个案例研究：使用Big Five人格数据集（公开心理测量数据）演示causal-learn+平台的工作流。案例展示了用户如何通过智能体助手交互，完成数据上传、缺失值处理（建议插补方法）、方法选择（智能体推荐基于连续线性假设的PC算法）、知识注入（用户指定某些变量间的冗余关系）以及结果解释（智能体解释显著边和条件独立性检验结果）。案例强调智能体不参与因果边决定，而是辅助用户做出明智决策。

Q5: 发现了什么实验现象？

案例研究显示：
1. 智能体成功引导用户完成因果发现流程，降低了使用causal-learn的技术门槛。
2. 用户可以在智能体协助下正确注入领域知识（如剔除冗余测量变量），改善图质量。
3. 智能体能够提供算法假设和诊断信息的自然语言解释，帮助非专家用户理解结果。
4. 未发现智能体输出因果边或方向，验证了原则的可行性。
没有定量指标（如结构汉明距离、F1分数）报告，因为论文重点是原则验证而非性能对比。

Q6: 有什么可以进一步探索的点？

1. 更全面的评估：设计用户研究比较有/无智能体辅助的因果发现效率、可复现性和最终图质量。
2. 扩展到更多数据集和领域：如生物医学、社会科学、经济学等，测试智能体适应不同数据特征和领域知识的能力。
3. 增强智能体交互：支持多轮对话、上下文记忆，以及基于用户反馈的动态调整方法推荐。
4. 智能体诊断可追溯性：记录每一步决策原因，便于审计和复现。
5. 处理更复杂场景：如时间序列因果发现、隐藏变量、混合数据类型等。
6. 探索智能体在因果发现伦理和公平性方面的角色：如检测偏见数据、建议平衡采样。

Q7: 总结一下论文的主要内容

本文是一篇立场论文，核心论点是：在大模型时代，智能体不应直接参与因果发现的图构建环节（如推断边方向、提供先验），而应作为辅助工具支持数据检查、方法推荐、知识注入和结果解释。这一原则旨在保证因果结论的可靠性，使其仍然基于数据、假设和形式化算法。论文通过causal-learn+平台实例化该原则，该平台围绕开源因果发现库causal-learn构建，提供在线代理式工作流。案例研究使用Big Five人格数据展示了智能体如何在不提供因果证据的情况下协助用户完成发现过程。论文强调，智能体的到来不应改变因果证据的标准，而应通过更好的辅助提升因果发现的透明度、可复现性和用户理解。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：直接涉及智能体（Agent）在科学发现中的应用，与用户画像中agent方向（权重0.10）高度相关。

## 基本信息

- 作者：Yujia Zheng, Vishal Verma, Mantej Gill, Haoyue Dai, Peter Spirtes, Kun Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG, cs.SE, stat.AP
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23608`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence）中的语义分块，并整合了heuristic_draft字段信息。
