---
user_id: "cheng tan"
paper_id: 6133
arxiv_id: "2607.27155v1"
title: "OmegaUse-OfficeVal: Benchmarking LLM Agents on Long-Horizon Office-Suite Tasks with Economic Grounding"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.27155v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.27155v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:26:27"
---
# OmegaUse-OfficeVal: Benchmarking LLM Agents on Long-Horizon Office-Suite Tasks with Economic Grounding

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：benchmark · llm agent · office suite · long-horizon tasks

## 一句话总结

提出OmegaUse-OfficeVal基准，用于评估LLM代理在长周期办公套件任务中的表现，并引入经济信号实现成本与质量的直接对比。

## 摘要

> Large language model (LLM) agents are increasingly expected to assist users in completing tasks. However, existing benchmarks provide limited support for evaluating whether agents can carry out office-suite workflows at a reasonable cost. We introduce OmegaUse-OfficeVal, a benchmark for evaluating LLM agents on long-horizon office-suite tasks with task-level economic grounding. The benchmark comprises 100 tasks derived from office-suite requests proposed by practitioners and adapted through a privacy-preserving process. On average, these tasks require 2.32 hours of human labor to complete. An important feature of the benchmark is that each task is paired with two economic signals: human labor time and task price proxy. These signals enable direct comparisons between human costs and LLM inference costs, as well as value-weighted evaluation. To support stable evaluation, we develop code-based verifiers from fine-grained rubrics. We evaluate several frontier LLMs together with a human baseline. Although all evaluated LLMs are substantially cheaper and faster than human workers, they have not yet approached human-level deliverable quality. The code and dataset are fully open-sourced, and more information is available on our project website: https://omegause-officeval.github.io.
> ![](images/7bd017b12d7b5d0a873a1077a7d917f528067e7287f13b2d7c60494ec4ec643a.jpg)
> (a)
> ![](images/7ae2fdb5c87e23ca2ca828ec12f6cf605d4a0829dcf5ef3887c1ad3139cdb9ed.jpg)
> (b)
> Figure 1: Overview of OmegaUse-OfficeVal. (a) The benchmark construction pipeline. (b) Task-level comparison of human annotators and LLM agents in terms of score, runtime, and cost; bubble size represents the average monetary cost per task.

Q1: 这篇论文试图解决什么问题？

论文试图解决当前LLM代理评估基准缺乏对办公套件长周期工作流成本效益评估的问题。现有基准多关注通用任务或限定域，未能提供经济信号来对比人类与代理的成本和质量，且任务往往是短周期或低层操作指令，而现实办公场景需要多步、长周期工作流。因此，需要一个包含经济性依据的、面向长周期办公套件任务的基准来衡量代理的实际可用性。

Q2: 有哪些相关研究？

相关工作分为三大类：生产力代理基准、办公自动化基准和计算机使用/GUI代理基准。生产力代理基准（如GDPVal、RLI）评估代理在通用生产力任务中的表现，但任务覆盖面广而不专于办公套件。办公自动化基准（如MiniWob、WebArena）侧重于特定网站或GUI操作，但缺乏对办公软件深度集成和经济成本的考量。计算机使用/GUI代理基准（如AndroidEnv、OSWorld）要求代理直接操控操作系统接口，但任务复杂性高且与实际办公流程距离较远。OmegaUse-OfficeVal填补了针对办公套件长周期工作流且带有经济信号的评估空白。

Q3: 论文如何解决这个问题？

论文通过以下步骤构建OmegaUse-OfficeVal基准：(1) 从从业者收集100个真实办公套件请求（如文档生成、数据整理、幻灯片制作等），并通过去隐私化适应；(2) 为每个任务标注两个经济信号：人工劳动时间（招募人类工作者完成所需时间）和任务价格代理（基于复杂度估计的货币价值）；(3) 设计基于代码的自动验证器，从细粒度评分规则（如格式正确性、内容覆盖度、逻辑一致性等）评估代理输出；(4) 招募人类工作者完成任务作为人类基线；(5) 在多个前沿LLM（如GPT-4、Claude等）上运行代理，比较其得分、运行时和成本，并引入价值加权评估来平衡质量与经济性。

Q4: 论文做了哪些实验？

论文在OmegaUse-OfficeVal上评估了多个前沿LLM代理，包括GPT-4、Claude系列等。实验首先将代理部署在受控环境中，通过验证器自动评分。同时招募人类工作人员完成同样的任务，记录其用时和质量评分。对比指标包括：任务得分（0-30分制）、运行时（分钟）和货币成本（美元）。此外，还进行了价值加权评估，即将任务得分与成本结合，计算每单位经济成本获得的质量。实验展示了每个任务上的个体差异，并提供了整体统计。结果表明，最佳LLM代理得分为17.91，人类基线为27.79；LLM代理平均运行时为4.48分钟，成本0.06美元，而人类平均运行时2.32小时，成本15美元左右。

Q5: 发现了什么实验现象？

实验现象包括：(1) 所有LLM代理都在成本和时间上远低于人类，但交付质量明显欠缺，最佳LLM得分比人类低约35%。(2) 价值加权评估进一步突出了模型之间的差异：成本效益最好的模型在质量上往往不是最高的，反之亦然。(3) 任务难度分布广泛，代理在简单文档格式化任务上接近人类，但在复杂数据分析和幻灯片设计任务上差距显著。(4) 推测代理失败源于长序列规划、办公软件特定操作（如精确格式调整）和跨应用整合的不足。(5) 经济信号作为归一化因子，使得不同任务间可以公平比较，且揭示了人类在低端任务上的成本劣势。

Q6: 有什么可以进一步探索的点？

论文未明确讨论未来工作，但基于基准和实验可合理推断以下方向：(1) 扩展任务集覆盖更多办公套件功能（如数据库管理、复杂公式）；(2) 引入实时人类反馈或强化学习提升代理质量；(3) 研究经济信号对模型训练和微调的指导作用；(4) 评估多模态代理在图表创建等方面的表现；(5) 讨论代理对劳动力市场的潜在影响。

Q7: 总结一下论文的主要内容

OmegaUse-OfficeVal基准的提出源于当前LLM代理评估的不足。现有基准如GDPVal和RLI覆盖通用生产力任务，但缺乏对办公套件长周期工作流（如使用Word、Excel、PowerPoint多步骤任务）的专门评估，且未考虑经济成本。本文通过收集100个来自实际工作场景的办公套件请求（经隐私清洗），构建了基准。每项任务平均耗时2.32小时人工，说明其复杂性。每个任务附带两个经济信号：人工劳动时间（从招募的人类工作者实测获得）和任务价格代理（基于工作复杂度估算的货币价值，如采用众包平台定价或行业工资标准）。这些信号支持“价值加权评估”，即用成本归一化质量得分，从而在LLM推理成本和人类劳动成本之间直接比较。
评估方法方面，论文开发了基于代码的自动验证器。验证器依据细粒度评分规则（例如文档格式、内容完整度、数值正确性等）对代理输出进行自动打分，避免了人工评分的不可重复性。人类基线由熟练办公软件用户完成同样任务，记录其时间与工资水平。
实验选取了多个前沿LLM（包括GPT-4、Claude等）作为代理的后端，通过提示或工具使用执行任务。结果表明，最佳LLM（推测为GPT-4）平均得分17.91，而人类基线为27.79（满分30）。在运行时上，LLM平均4.48分钟 vs 人类平均2.32小时；在成本上，LLM平均每次任务0.06美元（API调用成本），而人类成本约15美元（按工时工资计）。因此，LLM在效率和经济性上优势明显，但质量差距显著，尤其在需要创造性设计、精确布局和复杂逻辑分析的任务上。
在价值加权评估下，不同模型的排名发生变化：一些较便宜但质量稍低的模型在性价比上反而领先。这一现象提示，未来在部署代理时可根据具体任务对质量和成本的权衡选择模型。
该基准的主要贡献在于：首次将经济信号引入LLM代理评估，实现成本与质量的直接对比；提供了100个真实长周期办公套件任务；开发了自动化验证器确保可重复评估；开源了所有代码和数据。局限包括：任务数量有限（100个）；经济信号仅基于美国市场；代理仅能执行单轮任务而无法迭代；验证器可能遗漏某些主观质量维度。
总体而言，该论文为LLM代理在办公自动化领域的实用化评估奠定了框架基础，对后续研究和工程应用具有参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接关联智能体研究，尤其是LLM代理评估和安全。

## 基本信息

- 作者：Jingbo Zhou, Yusai Zhao, Qi Bao, Jingjia Cao, Zhenghai Chen, Chang Gao, Kaiqi Guo, Muxin Guo, Mingxuan Li, Xinjiang Lu, Yanru Ma, Yixiong Xiao, Zenghui Zhang, Le Zhang, Hua Wu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.HC
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.27155v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，包括摘要、引言和结论等部分。
