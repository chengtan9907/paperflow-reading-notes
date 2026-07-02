---
user_id: "cheng tan"
paper_id: 2332
arxiv_id: "2607.01211"
title: "Are Performance-Optimization Benchmarks Reliably Measuring Coding Agents?"
institution: "Singapore Management University; Shanghai Jiao Tong University"
publish_date: "2026-07-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.01211.pdf"
pdf_url: "https://arxiv.org/pdf/2607.01211"
abs_url: "https://arxiv.org/abs/2607.01211"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-02T14:18:12"
---
# Are Performance-Optimization Benchmarks Reliably Measuring Coding Agents?

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：performance optimization benchmark · coding agent · benchmark reliability · runtime measurement

## 一句话总结

系统审计了GSO、SWE-Perf和SWE-fficiency三个仓库级性能优化基准对编码智能体评估的可靠性，发现跨机器不稳定、评分规则导致排名不一致以及大多数任务已被公开提交解决等问题。

## 摘要

> Repository-level performance-optimization benchmarks such as GSO, SWE-Perf and SWE-fficiency evaluate coding agents by applying patches to real repositories and comparing runtime against unoptimized baselines and official reference patches. Their leaderboard scores are increasingly used as evidence of coding-agent progress, but those scores can conflate runtime instability, benchmark-specific scoring rules, and how many tasks are already solved by at least one public submission. We audit these issues across the three benchmarks. First, we replay the official reference patches for 740 code optimization tasks across four common types of Google Cloud machines. Most benchmark tasks can be replayed, but their reference patches satisfy the original benchmark validity rules in every cross-machine replay for only 39/102 GSO tasks, 11/140 SWE-Perf tasks, and 411/498 SWE-fficiency tasks; SWE-Perf is especially fragile because many reference patches produce close-to-zero runtime changes. Second, we show that public submission rankings depend strongly on the benchmark scoring rule. Among eight public submissions shared by GSO and SWE-fficiency, the official rankings disagree on 9 of 28 pairwise submission comparisons, and SWE-fficiency's leaderboard scoring rule assigns the worst ten tasks overly high score weights of 58.5%-82.8%. Third, looking across 10 public submissions for each task, we find that at least one submission matches or beats the reference patch on 85.3% (384/450) of replay-valid GSO and SWE-fficiency tasks, and beats the unoptimized base code on 99.8% (449/450). Our study complements leaderboard scores by identifying tasks with more reliable performance signals, quantifying per-task score contributions, and exposing the remaining performance gaps that are hidden by aggregate rankings.

Q1: 这篇论文试图解决什么问题？

当前仓库级性能优化基准（GSO、SWE-Perf、SWE-fficiency）的排行榜分数被用作编码智能体进展的证据，但这些分数可能因运行时测量不稳定性、基准特定的评分规则以及大量任务已被公开提交解决而产生误导。论文旨在系统审计这些因素如何影响基准的可靠性，并提供更细致的评估方法。

Q2: 有哪些相关研究？

相关研究包括仓库级性能优化基准的设计（如GSO、SWE-Perf、SWE-fficiency）、编码智能体的评估方法（如SWE-bench系列）、运行时测量的可重复性问题（如硬件波动、操作系统差异）以及基准评分规则的影响（如聚合函数选择、任务权重偏差）。论文的参考文献中还包含对智能体软件工程领域的综述（如Wang等, Agents in Software Engineering）和基准设计方法论。

Q3: 论文如何解决这个问题？

通过三个研究问题（RQ1: 参考补丁的跨机器可复现性；RQ2: 评分规则对排名的影响；RQ3: 公开提交是否已解决大多数任务），对三个基准的740个任务进行系统审计。具体包括：在四种Google Cloud机器类型（如n2-standard-4、n2d-standard-4等）上重放官方参考补丁，验证其有效性规则；比较8个公开提交在官方评分规则与替代规则（如边界惩罚）下的排名差异；统计450个重放有效任务中公开提交的表现。

Q4: 论文做了哪些实验？

实验包括三部分：（1）在四种Google Cloud机器类型上重放GSO、SWE-Perf和SWE-fficiency的740个官方参考补丁，检查每个补丁是否在所有机器上满足原始基准有效性规则；（2）使用八种公开提交，比较GSO和SWE-fficiency的官方排名与两种替代评分规则（边界惩罚、中位数速度提升）下的排名差异，分析最差十任务的分数贡献；（3）对450个重放有效任务（GSO和SWE-fficiency），统计每个任务是否有至少一个公开提交匹配或超越参考补丁。

Q5: 发现了什么实验现象？

实验观察：（1）跨机器可复现性差：GSO仅39/102任务（38%）、SWE-Perf仅11/140任务（8%）、SWE-fficiency仅411/498任务（83%）的参考补丁在所有机器上有效；SWE-Perf中许多补丁的运行时增益接近零，导致对机器变化极度敏感。（2）评分规则敏感：SWE-fficiency官方评分规则中，最差十任务贡献了58.5%-82.8%的分数；改用边界惩罚后，8个提交中有6个排名改变，8/28的两两比较结果翻转。（3）任务饱和：在450个重放有效任务中，384个（85.3%）已被至少一个公开提交匹配或超越参考补丁，449个（99.8%）超越了未经优化的基线；剩余未解决任务并非主要是基础正确性失败或策略选择问题。

Q6: 有什么可以进一步探索的点？

未来方向包括：（1）改进基准的运行时测量稳定性，例如标准化机器配置、多次运行取中位数、使用更精细的计时方法；（2）设计更鲁棒的评分规则，减少对少数困难任务的过度依赖，如采用边界惩罚、中位数速度提升或基于任务难度的自适应权重；（3）探索如何将公开提交的解作为更强的基线，推动未解决任务的研究；（4）跨更多软件环境、机器类型和编译器优化级别扩展审计；（5）研究运行时测量本身的可重复性问题（如CPU频率缩放、内存干扰）。

Q7: 总结一下论文的主要内容

论文系统审计了三个仓库级性能优化基准（GSO、SWE-Perf、SWE-fficiency）对编码智能体评估的可靠性。通过重放740个官方参考补丁、分析评分规则和公开提交表现，揭示了排行榜分数背后的三个混杂因素：（1）跨机器可复现性差，仅少数任务（GSO 38%，SWE-Perf 8%，SWE-fficiency 83%）的参考补丁在所有机器上有效，SWE-Perf尤为脆弱；（2）评分规则高度影响排名，SWE-fficiency中最差十任务贡献了58.5%-82.8%的分数，导致排名对规则选择敏感；（3）大多数任务（85.3%）已被至少一个公开提交解决，使得排行榜分数难以反映智能体间的真实能力差距。论文提出了替代评分规则（如边界惩罚）和任务级可靠性标签，为基准用户和设计者提供了更稳定的评估信号。研究还发现剩余未解决任务并非简单的正确性失败或策略问题，值得进一步探索。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：对编码智能体评估基准的设计和使用有直接指导意义，尤其适合基准开发者和用户

## 基本信息

- 作者：Zhi Chen, Zhensu Sun, Yuling Shi, David Lo, Lingxiao Jiang
- 机构：Singapore Management University; Shanghai Jiao Tong University
- 来源：arxiv
- 主题/分类：cs.SE, cs.AI
- 日期：2026-07-02
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.01211`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段。
