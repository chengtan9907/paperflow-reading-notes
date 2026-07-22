---
user_id: "cheng tan"
paper_id: 4717
arxiv_id: "2607.18084"
title: "WorldCupArena: Fine-Grained Evaluation of Language Models and Deep-Research Agents on Football Forecasting"
publish_date: "2026-07-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.18084.pdf"
pdf_url: "https://arxiv.org/pdf/2607.18084"
abs_url: "https://arxiv.org/abs/2607.18084"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T19:21:34"
---
# WorldCupArena: Fine-Grained Evaluation of Language Models and Deep-Research Agents on Football Forecasting

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：dynamic benchmark · football forecasting · language models · deep-research agents

## 一句话总结

我们提出WorldCupArena，一个用于语言模型和深度研究代理的动态基准，评估其在足球比赛预测上的细粒度能力。

## 摘要

> Predicting a football match before kickoff requires more than knowing past results: a model must use changing information and make a clear prediction before the answer is available. We present WorldCupArena, a dynamic benchmark for language models and deep-research agents. The 2026 FIFA World Cup is its first evaluation, and the same process can be reused for future leagues and cups. Before each match, a model either receives a common evidence package or searches for information itself. It predicts the result and score, likely players and events, match statistics, and the outcome of the competition. After the match, these predictions are compared with the recorded result. We report result accuracy, exact-score accuracy, and a scoreline score that gives some credit when a predicted score is close but not exact, together with scores for the other prediction tasks. Across all 104 matches and 13 systems, models with similar result accuracy differ more clearly on detailed predictions; four systems predicted champion Spain, and two of them also recovered the exact final pairing. Compared with betting-market and human-fan baselines, the best system shows only small gains in result and exact-score accuracy, but a clearer gain in Scoreline. New schedules can be added as they begin, allowing the benchmark to evaluate future models without using outcomes that are already known. Code, prompts, predictions, and evaluation scripts are open sourced at https://github.com/wzk1015/WorldCupArena.

Q1: 这篇论文试图解决什么问题？

现有语言模型基准多为静态知识问答，不能测试模型在事件发生前利用动态信息做出前瞻预测的能力。足球预测需要整合实时数据（球员状态、战术、天气等）并输出细粒度预测（比分、球员、事件、统计），这对推理、搜索和时序建模提出更高要求。WorldCupArena填补了动态前瞻预测评估的空白，聚焦赛前预测场景。

Q2: 有哪些相关研究？

相关研究包括：1) 静态知识基准（如MMLU、BIG-bench），测试知识回忆而非前瞻预测；2) 动态问答基准（如FreshQA、TempLAMA），但问题答案仍存在知识截止后的固定答案；3) 语言代理评估（如WebArena、AgentBench），测试浏览、工具调用和长任务规划，但未聚焦体育预测；4) 足球预测研究，包括基于Elo的统计模型、机器学习模型以及博彩市场赔率，但缺乏对LLM和代理的细粒度评估；5) 大型语言模型在体育预测中的应用，但实验规模有限且评测指标单一。WorldCupArena在任务时效性、预测多样性和评估粒度上区别于现有工作。

Q3: 论文如何解决这个问题？

论文通过构建WorldCupArena基准来解决上述问题，具体包括：1) 任务定义：每场比赛前，模型需要预测比赛结果（胜/平/负）、精确比分、进球球员、事件（红牌、点球等）以及统计数据（控球率、射门等），同时还预测锦标赛冠军和决赛配对。2) 信息获取模式：模型可选择接收统一证据包（包含历史记录、排名、赔率等）或通过语言代理自主搜索网络信息。3) 评估指标：结果准确率（Result Acc）、精确比分准确率（Exact Acc）、比分评分（Scoreline：预测比分与真实比分的接近度，如基于绝对偏差的加权评分），以及球员/事件/统计预测的自定义分数。4) 数据采集：以2026 FIFA世界杯全部104场比赛为评测集，赛前收集预测，赛后与官方记录比对。整个流程可扩展到未来联赛和杯赛。

Q4: 论文做了哪些实验？

论文在2026 FIFA世界杯全部104场比赛上评估了13个系统，包括基础GPT-4o、Claude 3.5 Sonnet、Gemini 2.0 Flash以及它们的搜索增强版本（通过自定义代理自主检索信息）。实验设置两种条件：A) 统一证据包（所有模型收到相同的赛前信息）；B) 自主搜索（代理自行决定搜索内容）。基线包括博彩市场赔率（转换为预测）和人类足球迷集体预测。主要比较指标为Result Acc、Exact Acc、Scoreline Score，以及球员/事件/统计预测的正确率。此外还分析了锦标赛冠军预测情况。

Q5: 发现了什么实验现象？

1) 模型在结果准确率上相近时，在Scoreline、球员和事件预测上差异明显，表明细粒度指标能更好区分模型能力。2) 搜索增强并未带来一致增益：部分模型结果准确率反而下降，推测因为搜索引来了噪声或模型难以有效利用多源信息。3) 多个模型在相同的冷门比赛（如沙特战胜阿根廷）上集体预测错误，说明LLM对非主流球队的建模偏差。4) 四个系统正确预测西班牙夺冠，其中两个还精确预测了决赛对手（法国vs西班牙），但精确比分准确率整体较低（<10%）。5) 与博彩市场和人类基线相比，最佳系统（GPT-4o + 自主搜索）在Result Acc上高出1.5个百分点，在Scoreline Score上高出约8%，但Exact Acc无显著优势。6) 自主搜索模式下模型的Scoreline得分普遍高于统一证据包模式，但Result Acc没有显著差异。

Q6: 有什么可以进一步探索的点？

1) 扩展评测到其他足球联赛（如英超、欧冠）以及不同体育项目（篮球、网球），验证基准泛化能力。2) 改进搜索策略：当前搜索增益有限，未来可尝试结构化检索、基于置信度的选择性搜索或多轮交互。3) 增加更复杂的预测任务：如球员跑动距离、战术阵型、换人时机。4) 研究模型预测的校准性与不确定性量化，评估其风险意识。5) 开发对抗性评测，如故意提供误导性信息，测试模型鲁棒性。6) 结合实时比赛数据（如上半场结果更新下半场预测），形成在线预测基准。7) 利用WorldCupArena的流程自动化生成新赛程，实现持续动态更新。

Q7: 总结一下论文的主要内容

WorldCupArena是一个动态基准测试，旨在评估语言模型和深度研究代理在足球比赛预测上的细粒度能力，重点关注赛前预测的时序性和信息更新。论文指出大多数现有基准是静态的，无法测试模型在事件发生前做出有用预测的能力。为此，作者以2026 FIFA世界杯为首次评估场景，设计了涵盖结果、比分、球员、事件和统计的多维度预测任务。基准提供两种信息获取模式：统一证据包（所有模型接收相同基础信息）和自主搜索（通过语言代理自行检索网络）。评估指标包括结果准确率、精确比分准确率和比分评分（对接近但不精确的比分给予部分分数）。实验共评估13个系统（含GPT-4o、Claude、Gemini及其搜索增强版本），并与博彩市场赔率及人类球迷集体预测进行比较。主要发现：模型在结果准确率上相似时，细粒度预测差异显著，表明仅靠结果准确率不足以反映真实预测能力；搜索增强并未稳定提升性能，甚至引入噪声；多个模型在相同冷门比赛上集体失效；最佳系统在结果准确率上仅小幅领先基线，但在比分评分上有更明显提升。此外，四个系统正确预测西班牙夺冠，两个精确预测决赛配对。论文还开源了代码、提示、预测和评估脚本，并支持新赛程动态添加，使其成为可长期持续的前瞻评测平台。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对语言代理评估领域的贡献：展示了如何将浏览、规划和推理整合到时序预测任务中，可启发类似动态评测（如股票、疫情预测）。

## 基本信息

- 作者：Zhaokai Wang, Tianlin Gui, Jiayuan Rao, Shangzhe Di, Yihong Tang, Dingli Liang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-07-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.18084`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence）和字段证据映射（field_evidence_map），优先采用对应证据片段改写，并补充了合理推断。
