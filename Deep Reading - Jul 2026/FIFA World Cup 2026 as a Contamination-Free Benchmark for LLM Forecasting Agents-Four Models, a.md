---
user_id: "cheng tan"
paper_id: 4946
arxiv_id: "2607.17765"
title: "FIFA World Cup 2026 as a Contamination-Free Benchmark for LLM Forecasting Agents: Four Models, a Bookmaker, and 104 Matches"
publish_date: "2026-07-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.17765.pdf"
pdf_url: "https://arxiv.org/pdf/2607.17765"
abs_url: "https://arxiv.org/abs/2607.17765"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T19:22:34"
---
# FIFA World Cup 2026 as a Contamination-Free Benchmark for LLM Forecasting Agents: Four Models, a Bookmaker, and 104 Matches

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm forecasting · benchmark · contamination-free · world cup 2026

## 一句话总结

提出了 WC2026-Agents 基准，利用2026年世界杯104场无污染比赛评估四个前沿 LLM 作为自主预测智能体，并与博彩市场对比，揭示原始准确率掩盖的校准、决策质量和自知差异。

## 摘要

> We introduce WC2026-Agents, a benchmark and dataset for evaluating large language models (LLMs) as autonomous forecasting agents on real, future events. For every one of the 104 matches of the 2026 FIFA World Cup, four frontier models -- Claude Opus 4.8, ChatGPT (GPT-5.5, high reasoning), Gemini 3.1 Pro, and Grok (Expert Mode) -- ran an identical search-act-reflect loop: gather evidence with a web tool, commit to a 1X2 (team-A win / draw / team-B win) distribution and a virtual 100-USD bet, and, after the match, reflect given only the final score. Because every match kicked off after the models' training cutoffs, the benchmark is contamination-free by construction. Crucially, we pair the four agents with a fifth competitor drawn from the same information environment -- the pre-match betting market -- collected as per-match 1X2 odds, giving an economically grounded baseline and letting us score not just what an agent predicts but what it does with money. The release contains 416 forecasts and 414 reflections with verbatim reasoning, ground truth (including penalty shootouts), odds, and a reproducible evaluation suite. A reference evaluation surfaces findings that raw accuracy hides: the four agents issue an identical top pick in 92% of matches and none beats the market's Brier score; indeed, a naive flat stake on the market favorite out-earns all four agents. Yet the agents diverge sharply as decision-makers: betting return-on-investment ranges from -18% to +10%, fading the market is unprofitable for all four, the share of forecasts that cite the market ranges from 12% to 100%, and self-reported error rates on wrong picks range from 36% to 86%. The benchmark thus measures calibration, decision quality, and self-knowledge -- axes on which frontier models differ even when their predictions do not. Data and code: https://github.com/graphuofm/FIFA2026LLM

Q1: 这篇论文试图解决什么问题？

论文试图解决如何系统评估 LLM 作为自主预测智能体在真实未来事件上的能力，特别是避免数据污染，并超越单一准确率指标引入经济决策和自知等维度。现有评估常使用历史事件导致数据泄露，或仅关注预测准确性而忽略智能体如何利用信息、执行决策和自我反思。该工作通过无污染的世界杯比赛任务结合市场基线，为评估校准、决策质量和自知提供了框架。

Q2: 有哪些相关研究？

相关研究包括 LLM 预测能力评估、预测市场与群体智慧、智能体反思与校准。已有工作探索 LLM 在政治、体育等领域的预测但常受数据污染困扰；预测市场研究显示赔率聚合信息有效；智能体反思方面自我批评与元认知受关注。该基准创新结合无污染赛场、市场基线和反射性评估。由于证据有限，关联具体文献需原文确认。

Q3: 论文如何解决这个问题？

具体方案分任务构建、智能体协议和评估。任务：选择2026年世界杯104场比赛，每场赛前预测保证无污染。智能体协议：四个 LLM 使用相同搜索-行动-反思循环：先网络搜索收集信息，再提交 1X2 概率分布及基于该分布下注100虚拟美元（推测为按比例分配），赛后仅根据比分反思并评价自身错误。评估指标包括 Brier 分数、投注回报率、市场引用率（推理中提及赔率比例）、自报错误率。同时收集市场赔率作为基线。

Q4: 论文做了哪些实验？

实验包括运行四个 LLM 智能体对全部104场比赛进行预测和反思，并收集市场赔率。计算各项指标：Brier 分数、准确率、假设按预测分布下注的回报率、市场引用率、错误回忆率等。对比四个智能体之间及 vs 市场。具体实验设置如搜索工具、提示模板等未在摘要细述。

Q5: 发现了什么实验现象？

主要现象：(1) 预测趋同：智能体在92%比赛中 top pick 相同，表明事实推断高度一致。(2) 市场优势：无智能体 Brier 分数胜市场，平注市场最爱回报超过所有智能体。(3) 决策分歧：回报率-18%到+10%，表明决策策略差异大。(4) 信息使用差异：市场引用率12%到100%，反映对市场信息依赖不同。(5) 自知差异：自报错误率36%到86%，显示自我认知能力悬殊。这些说明仅顶选准确率会掩盖重要差异。

Q6: 有什么可以进一步探索的点？

未来方向包括：(i) 校准与不确定性：利用416个概率预测分析校准曲线。(ii) 决策优化：测试不同投注策略对回报的影响。(iii) 自我反思：深入分析反思语言模式。(iv) 跨领域迁移：应用于其他体育或事件预测。(v) 动态预测：考虑赛中实时调整。(vi) 市场博弈：利用市场信号改进预测。(vii) 代理改进：更好的搜索、记忆或自我校验。论文提供标准化数据便于此类研究。

Q7: 总结一下论文的主要内容

本文针对 LLM 自主预测智能体评估，提出了 WC2026-Agents 基准。利用2026年世界杯104场比赛，所有事件在模型训练截止后发生，从根本上避免数据污染。四个前沿模型在统一协议下运行：赛前搜索→输出 1X2 概率和虚拟赌注→赛后反思自我错误。同时收集博彩市场赔率作为经济基线。发布416条预测、414条反思、结果、赔率及评估代码。参考评估揭示三大发现：(1) 顶选预测上模型趋同（92%一致）且均不如市场 Brier 分数；(2) 决策回报差异显著（-18%到+10%），平注市场最爱即可胜过所有模型；(3) 自知能力差距悬殊（36%到86%），市场引用率12%到100%，反映信息融合策略截然不同。这些论证了单一准确率指标的局限性，展示校准、决策质量和自知作为独立维度的必要性。主要贡献在于提供可复现、无污染的多维 LLM 预测智能体评估平台。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本论文与智能体评估方向高度相关，提供预测代理的详细任务设计和评估框架

## 基本信息

- 作者：Jiacheng Ding, Cong Guo, Jason Xu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.DB
- 日期：2026-07-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.17765`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答优先参考了PDF语义检索命中的证据片段（Abstract、Introduction、Conclusion等）并结合heuristic_draft完成，其中对方法细节和局限性部分涉及合理推断。
