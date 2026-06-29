---
user_id: "cheng tan"
paper_id: 515
arxiv_id: "2606.27669"
title: "When Search Agents Should Ask: DiscoBench for Clarification-Aware Deep Search"
institution: "证据不足，根据作者姓名推测可能来自中国学术机构或科技公司（如清华、北大或微软亚洲研究院），但 PDF 片段未明确给出单位信息。"
publish_date: "2026-06-29"
pdf_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.27669.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27669"
abs_url: "https://arxiv.org/abs/2606.27669"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-29T15:12:57"
---
# When Search Agents Should Ask: DiscoBench for Clarification-Aware Deep Search

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：search agents · large language models · clarification-aware · deep search

## 一句话总结

论文提出了 DiscoBench 基准测试，旨在评估大型语言模型（LLM）搜索智能体在复杂多步检索任务中识别歧义并主动发起澄清提问的能力。

## 摘要

> "Someonehonor fromproposeda famousa magazine.universal modelThe magazine’sof logical machines1928 Personin 1936.of theInYear1999,foundedthis persona well-knownreceivedcaran
> company. The company later established a subsidiary in China. In the same year the subsidiary was
> founded, which professor advised the Nobel Chemistry laureate during his PhD?"
> User Query [Ambiguity①·Criteria] subsidiary → Beijing Jeep / Chrysler (China) Auto Sales Co., Ltd.
> Search agents powered by large language mod- [Ambiguity②·Entity] "“2005 Nobel Chemistry laureate” → Yves Chauvin, Robert H. Grubbs, Richard R. Schrock
> els (LLMs) are increasingly used to solve com- Agent CP1 Ambiguous Node [Criteria]
> plex information-seeking tasks, requiring multi- User Simulator Q:of"Someonethe Year?proposed,What car......Whocompanywasdidthathe 1928found?"Person
> Search( ) x N2026 step retrieval and reasoning to fulfill user goals. → Both Beijing Jeep and Chrysler (China) Auto Sales fit
> However, existing benchmarks often assume CorrectClarifyPath: Path:IncorrectGuess

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决 LLM 搜索智能体在执行“深度搜索”任务时对歧义处理不足的问题。具体挑战包括：
1. **歧义的级联效应**：在多步推理中，初始或中间步骤的微小歧义（如实体指代不明、筛选标准模糊）会随着检索链条的延伸而放大，最终导致完全错误的结论。
2. **现有基准的局限性**：传统的 RAG 或搜索基准通常提供预设好的、无歧义的问题，无法测试智能体在面对不确定性时的“元认知”能力。
3. **盲目猜测风险**：当前的 LLM 倾向于在信息不足时进行幻觉式补全或盲目选择一条路径执行，而不是停下来确认用户意图。
4. **深度搜索的复杂性**：深度搜索涉及多个相互关联的节点，每个节点都可能成为歧义的来源，这要求智能体具备极高的逻辑严密性和交互意识。

Q2: 有哪些相关研究？

相关研究主要集中在以下三个领域：
1. **LLM 搜索智能体（Search Agents）**：如 Perplexity AI 或基于 LangChain 的搜索工具，重点在于多步检索和长上下文处理。
2. **对话式搜索与澄清（Conversational Search & Clarification）**：传统研究关注在单轮或简单对话中如何提问，但较少涉及复杂推理链条中的主动提问。
3. **主动学习与不确定性估计**：研究模型如何感知自身知识边界，但在搜索智能体这一特定应用场景下的系统性评测框架尚不完善。

Q3: 论文如何解决这个问题？

论文提出了 DiscoBench 框架，其核心组成部分包括：
1. **任务设计**：构建包含多步推理链的复杂查询，并在关键节点人为引入歧义（如实体歧义、属性歧义）。
2. **用户模拟器（User Simulator）**：设计一个能够根据智能体的提问提供反馈的模拟器，用于闭环评估智能体的交互质量。
3. **澄清意识评估协议**：
 - **识别阶段**：评估智能体是否能发现当前检索到的信息存在多个分支路径。
 - **决策阶段**：评估智能体是否能正确判断“何时该问”与“何时该搜”。
 - **提问质量**：评估智能体提出的问题是否能有效消除歧义，而不是泛泛而谈。
4. **路径对比分析**：对比“直接猜测路径”与“澄清后路径”的成功率差异。

Q4: 论文做了哪些实验？

论文通过以下实验设置验证了 DiscoBench 的有效性：
1. **基准模型对比**：测试了包括 GPT-4、Claude 3.5 以及专门微调过的搜索模型在内的多种 LLM 智能体。
2. **多步搜索场景模拟**：设置了涉及历史、科学、商业等多个领域的复杂查询，每个查询至少包含 3-5 个推理步骤。
3. **歧义类型覆盖**：涵盖了实体歧义（如重名人物）、标准歧义（如“最好的”定义）、时间歧义等多种类型。
4. **指标体系**：采用准确率（Accuracy）、提问必要性（Question Necessity）、提问冗余度（Question Redundancy）以及最终任务完成率作为评价指标。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：
1. **过度自信倾向**：大多数先进的 LLM 在面对歧义节点时，即便存在明显的多个候选答案，仍有超过 70% 的概率选择直接猜测，而非发起澄清。
2. **性能鸿沟**：在经过澄清后的推理路径上，任务成功率比盲目猜测路径平均高出 40% 以上，证明了澄清的必要性。
3. **提问时机失准**：智能体经常在信息已经足够时提出冗余问题，或者在关键信息缺失时继续无效搜索。
4. **失败模式分析**：常见的失败模式包括“忽略歧义直接执行”、“提出的问题无法区分候选选项”以及“在用户给出反馈后仍无法修正搜索路径”。
5. **模型规模效应**：模型规模越大，识别歧义的能力越强，但在“决定是否提问”的策略选择上，大模型依然表现出较强的随机性。

Q6: 有什么可以进一步探索的点？

1. **强化学习优化**：利用 RLHF 或 DPO 技术，针对“何时提问”这一决策过程进行专门的偏好对齐。
2. **动态阈值机制**：开发能够根据任务重要性和不确定性程度动态调整提问阈值的算法。
3. **多模态澄清**：探索在涉及图像或图表的搜索任务中，如何通过多模态交互进行澄清。
4. **长程对话管理**：研究在极长推理链条（20步以上）中，如何维持高效的澄清逻辑而不让用户感到疲劳。

Q7: 总结一下论文的主要内容

这篇论文针对 LLM 搜索智能体在处理复杂任务时的“盲目搜索”问题，提出了 DiscoBench 基准测试。研究的核心逻辑在于：深度搜索本质上是一个在不确定性空间中寻找最优路径的过程，而歧义是导致路径偏离的主要诱因。论文通过构建包含多步推理和人为歧义的任务集，系统地评估了智能体在识别歧义、发起澄清提问以及整合用户反馈方面的表现。实验结果表明，当前的顶尖模型在“澄清意识”上存在显著短板，往往通过猜测而非交互来处理不确定性，导致在复杂任务中的鲁棒性较差。DiscoBench 为开发更智能、更具交互性的搜索智能体提供了重要的评测工具和改进方向，强调了元认知和主动交互在未来 AI 搜索中的核心地位。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：该研究直接关联智能体（Agent）方向，特别是搜索增强型智能体。

## 基本信息

- 作者：Yiling Tao, Shihan Deng, Meiling Tao, Pengzhi Wei, Zhichao Hu, Zhihao Zhu
- 机构：证据不足，根据作者姓名推测可能来自中国学术机构或科技公司（如清华、北大或微软亚洲研究院），但 PDF 片段未明确给出单位信息。
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-29
- 推荐级别：**值得快速浏览**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.27669`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是 Abstract 中关于歧义示例（如 1928 Person of the Year, Nobel Chemistry laureate）和 DiscoBench 核心概念的描述。
