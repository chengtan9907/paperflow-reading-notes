---
user_id: "cheng tan"
paper_id: 430
arxiv_id: "2606.16576v1"
title: "Can LLM Agents Infer World Models? Evidence from Agentic Automata Learning"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16576v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16576v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:07:22"
---
# Can LLM Agents Infer World Models? Evidence from Agentic Automata Learning

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agents · automata learning · world models · interactive learning

## 一句话总结

我们提出智能体自动机学习（agentic automata learning）以评估工具调用型LLM智能体通过交互揭示隐藏环境的能力。

## 摘要

> 本工作引入智能体自动机学习框架，通过隐藏确定性有限自动机（DFA）来评估LLM智能体的交互式发现能力。智能体需通过成员查询（MP）和等价查询（EQ）与oracle交互以揭示隐藏DFA。该框架提供可控的任务复杂度、可衡量的交互效率以及经典自动机学习算法作为强基线。实验表明，最先进的LLM在DFA规模增大时性能急剧下降；推理模型（如GPT-5.4 reasoning）明显优于非推理模型，但轨迹分析揭示其在查询规划、证据整合和假设构建中反复出现失败。总体而言，当前LLM智能体有时能完成非平凡交互式发现，但远不如经典算法稳健高效。

Q1: 这篇论文试图解决什么问题？

现有LLM智能体基准评估（如网页导航、软件工程、工具使用）通常建立在复杂端到端任务上，难以隔离诸如自适应信息收集、记忆组织、基于反馈的推理等核心能力。需要一种可控的、具备正式复杂性度量的任务来评估智能体在交互中推断隐藏结构的能力。同时，已有世界模型学习工作主要关注训练阶段而非推理时的交互学习。因此，本文提出自动机学习作为探针，以系统衡量LLM智能体的发现效率与失败模式。

Q2: 有哪些相关研究？

相关工作分为三类：1) LLM智能体基准：如WebArena、SWE-bench、ToolBench等，侧重于端任务成功率，但环境复杂难以归因。2) 世界模型学习：Kuo等人（2023）和Vafa等人（2024）用语言模型从数据中学习世界模型，但专注训练而非交互推断。3) 主动自动机学习：由Rivest和Schapire（1993）提出并由Isberner（2015）扩展的L*算法等，能够在多项式查询内学习DFA。本文将这些经典算法作为强基线，并设计智能体版自动机学习来桥接LLM智能体与形式化学习之间的鸿沟。

Q3: 论文如何解决这个问题？

提出智能体自动机学习（agentic automata learning）框架：1) 用隐藏DFA A*初始化oracle，智能体通过成员查询（MP：问字符串是否属于语言）和等价查询（EQ：提交假设DFA并获取反例）与oracle交互。2) 智能体配备专用工具（如send_mp、send_eq）来发起查询，并接收结果。3) 任务复杂度由A*的状态数控制，评估指标为成功标识A*所需的查询次数（交互效率）。4) 使用经典L*算法作为参考基线，其查询复杂度为O(kn^3)，其中k为字母表大小，n为状态数。

Q4: 论文做了哪些实验？

实验设置：1) 评估模型：GPT-4o、GPT-5.4（含reasoning变体）、Gemini 3.1 Flash Lite、Llama-3.3-70B、Llama-4。2) 隐藏DFA：随机生成2至20个状态的DFA，字母表大小固定为2。3) 每个配置运行多次，记录成功率、平均查询次数、工具调用正确性等。4) 对成功轨迹进行手动分析，标注失败类型。5) 消融实验：对比被动学习（仅给示例但无查询交互）与主动学习。

Q5: 发现了什么实验现象？

1) 成功率随DFA状态数急剧下降：2状态时多数模型成功；6状态时除GPT-5.4 reasoning外大部分失败；20状态时仅GPT-5.4 reasoning有少量成功，GPT-4o及以下为0%。2) 推理模型显著更强：GPT-5.4 reasoning在10状态时成功率达40%，非推理版本（GPT-5.4 without reasoning）为0%。3) 经典L*算法在所有规模下均100%成功，且查询次数远低于模型。4) 轨迹分析揭示三大失败模式：查询规划差（如重复相同MP、早发EQ）、证据整合失败（忽略反例关键信息、遗忘早期结果）、假设构建错误（无法从观察中归纳出正确DFA）。5) 工具使用问题：部分模型未正确调用查询工具，或输错了参数。6) 成功率与模型参数量并非严格正相关（如Llama-3.3-70B仅次于GPT-4o）。

Q6: 有什么可以进一步探索的点？

1) 扩展至非确定性自动机（NFA）或有随机性的环境。2) 放松完美oracle假设：引入噪声、部分反馈、延迟信号或由LLM替代oracle。3) 用更弱的反馈替代等价查询（如仅提供反例但不要求完整DFA）。4) 将框架扩展到其他形式结构（如上下文无关语法、图结构）。5) 研究被动学习（仅从样本推断）与主动学习的差距，以分离交互能力与归纳能力。6) 探索通过少量微调或提示增强提升智能体交互学习效果。

Q7: 总结一下论文的主要内容

本文提出智能体自动机学习（agentic automata learning）作为评估LLM智能体交互式发现能力的正式框架。通过将隐藏环境建模为确定性有限自动机（DFA），智能体通过成员查询和等价查询逐步揭示目标。该框架提供可控复杂度、可量化效率以及经典算法（如L*）作为基准。实验评估了多个前沿LLM，发现性能随自动机规模增加而急剧下降：2状态时多数成功，20状态时仅推理模型GPT-5.4 reasoning有少量成功。推理模型整体优于非推理模型，但轨迹分析揭示其在查询规划（如重复提问、过早发起等价查询）、证据整合（忽略反例、遗忘早期信息）和假设构建（归纳错误）上存在系统性缺陷。经典L*算法在所有条件下表现出色，凸显了当前LLM在形式化发现任务上的不足。论文贡献包括：形式化新框架、系统评估多个LLM、揭示具体失败模式、为未来智能体诊断提供可扩展测试床。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本文与您的智能体（agent）方向直接相关，研究了LLM智能体通过交互发现隐藏结构的能力。

## 基本信息

- 作者：Reef Menaged, Gili Lior, Shauli Ravfogel, Roee Aharoni, Gabriel Stanovsky
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16576v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本输出基于PDF语义检索的文本片段生成，主要参考了Abstract、Introduction及部分方法、结果段落的证据，对缺失信息做了合理推断。
