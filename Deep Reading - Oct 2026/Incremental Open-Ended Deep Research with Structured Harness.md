---
user_id: "cheng tan"
paper_id: 14938
arxiv_id: "2610.11566v1"
title: "Incremental Open-Ended Deep Research with Structured Harness"
institution: "论文未在元数据中明确标注机构；根据通讯邮箱（chenmeilin2@xiaohongshu.com、merlinis@zju.edu.cn）合理推断作者可能来自小红书（xiaohongshu.com）和浙江大学（zju.edu.cn），但此推断未获论文正文直接确认。"
publish_date: "2026-10-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.11566v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.11566v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-11T17:13:37"
---
# Incremental Open-Ended Deep Research with Structured Harness

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：open-ended deep research · incremental update · structured harness · report generation

## 一句话总结

本文提出Incremental-OEDR设置与Structured Harness框架，将深度研究报告视为随时间演化的结构化状态，通过证据池复用与选择性章节更新实现增量式维护，在保持报告质量的同时显著提升报告连续性与降低研究成本。

## 摘要

> 论文针对现有Open-Ended Deep Research (OEDR)系统从零生成报告、难以持续维护的问题，提出一个新的研究设定Incremental-OEDR，以及配套的Structured Harness框架。具体内容包括：(1) 将报告表示为大纲、章节和支持证据组成的结构化集合；(2) 提供结构化检索、持久化结构化证据池和结构化生成机制，支持选择性地更新过时内容、保留有效知识并融入新信息；(3) 建立跨越十年的时间评估框架，包含Single-Step Task (SST)和Long-Chain Task (LCT)两个任务；(4) 在DeepResearch Bench和DeepConsult上，分别使用开源配置(OC)和专有配置(PC)进行实验，结果显示Incremental-OEDR在保持报告质量的同时，大幅提升报告连续性并降低研究成本，例如在DeepResearch Bench上内容级ROUGE-L F1提升0.51、大纲级EM F1提升0.63、token消耗降低33%、搜索调用减少61%。项目页面：https://ioedr-project.github.io/。

Q1: 这篇论文试图解决什么问题？

1. 现有OEDR系统的一次性研究范式：目前主流的OEDR系统（无论是预定义流程的workflow还是动态规划的agent）都围绕单次查询从零开始生成报告，每次都需要重新规划、搜索、验证和综合，无法利用历史研究状态。
2. 实际场景中报告需要持续演化：在许多真实应用中，研究主题随时间变化，新信息不断涌现，报告不是静态产物，而是需要被定期维护和更新的知识载体。从零重生成的方式导致大量重复计算，并且会丢失之前积累的证据和结构，破坏报告的连续性和可追溯性。
3. 缺乏增量维护机制：现有系统没有显式建模“报告作为演化状态”的概念，也没有针对报告更新的结构化操作，例如如何判定哪些章节已经过时、哪些证据可以复用、如何在不破坏整体结构的前提下融入新发现。
4. 评估体系的缺失：已有基准（如DeepResearch Bench、DeepConsult）通常只评估单次生成质量，缺乏对时间维度上报告连续性和更新成本的度量，无法体现增量式研究相对于一次性研究的优势。
5. 证据管理困难：随着检索证据数量增长，一次性系统难以有效组织历史证据，导致检索噪声增加、生成策略僵化，限制了其适应中间发现的能力（论文中明确指出rigid pipelines的适应性有限）。
6. 技术空白：论文提出Incremental-OEDR作为新的研究问题，并强调现有系统的差异：它们无法处理“旧报告需要被修订而非重写”的场景。

Q2: 有哪些相关研究？

1. OEDR工作流（Workflow）：论文引用Han et al. (2025)、Felovic、Shi et al. (2026)等，这类系统遵循预定义的研究流水线，先由初始outline指导证据检索，再生成报告。优点是可预测、简单，但管线固定，难以根据中间证据动态调整。
2. OEDR智能体（Agent）：引用Shao et al. (2024)、Li et al. (2026)、Patel et al. (2025)等，让LLM智能体动态规划、搜索并基于中间发现迭代细化研究过程，灵活性更高，适合复杂开放问题，但仍是一次性生成。
3. 报告质量评估体系：DeepResearch Bench使用RACE指标，DeepConsult使用GPT-4.1评分，这些基准原本针对单次生成质量设计。
4. 时间敏感检索：论文通过Tavily API的end_date参数控制可见信息的时间范围，这一机制与时间敏感信息检索相关，但论文侧重在评估框架中使用。
5. 增量学习与持续更新：虽然论文没有直接引用增量学习或持续学习文献，但其思想与持续学习、流式数据上的知识维护有关。我们合理推断该方向与RAG系统中的增量索引更新也有交集，但论文未展开。
6. 结构化文档生成：Structured Harness中大纲-章节-证据的表示与文档规划、分层生成相关，但论文将结构化表示用于增量更新，而非一次性生成。
7. 证据池与记忆机制：持久化证据池与智能体长期记忆、工具使用（如evidence_store write_note）有联系；检索证据中也提到工具使用时需要task_id关联证据，反映其与智能体工具调用的工程结合。

Q3: 论文如何解决这个问题？

论文的核心解决方案是Structured Harness，配合Incremental-OEDR设定，具体分四个层面：
1. 结构化表示（Structured Representation）：将报告从无结构的文本流转换为结构化集合，包含大纲（outline）、章节（section）和支撑证据（evidence）。每个章节与若干证据关联，形成可独立操作的最小单元。这为后续选择性更新提供细粒度操作基础。
2. 结构化检索（Structured Retrieval）：在发起新的外部搜索之前，先基于原有报告结构对内部状态进行检索，给智能体提供“现有研究状态的定向视图”，从而决定哪些部分需要补充信息、哪些部分可以跳过。这避免了盲目全量搜索。
3. 持久化结构化证据池（Persistent Structured Evidence Pool）：在周期性更新过程中，底层证据被持续保存和积累，支持证据复用。智能体可以通过证据池访问历史证据，避免重复检索，同时保持证据来源的可追溯性。检索片段中提到工具使用时需要task_id来关联证据，说明证据池与对话/任务状态绑定。
4. 结构化生成（Structured Generation）：生成新报告时不是从头开始，而是在原报告基础上进行“选择性更新”：保留有效知识，修订过时或不完整的内容，并纳入新获取的信息。论文中暗示生成过程会遵循章节级操作，可能包括删除、重写、新增章节，而不是整体重写。检索片段中也有提示“保留已有分析章节（包括比较/trade-off/决策框架），不要删除”，说明框架对结构延续性有硬性约束。
5. 两种任务设置：Single-Step Task（单次增量更新）和Long-Chain Task（长期连续更新链），分别测试单步更新能力和多步积累的稳定性。
6. 训练/推理配置：支持开源配置（OC，例如Qwen3.5-397B-A17B作为Researcher agent）和专有配置（PC，模型未具体说明），表明框架与后端LLM解耦。
7. 工程实现：从检索证据可见，系统通过Tavily搜索API控制时间范围，使用evidence_store工具记录证据，并可能使用judge model（如GPT-4.1）进行质量评判。

Q4: 论文做了哪些实验？

1. 时间评估框架：跨越十年（2016-2025），每个时间戳固定在当年12月31日。使用Tavily搜索API的end_date参数，限制检索结果只能访问不晚于目标时间t的信息，以此模拟不同时期可用的信息环境。
2. 任务设计：
 - Single-Step Task (SST)：给定前一时间戳的OEDR初始化报告R_{t-1}，让Incremental-OEDR生成下一时间戳的报告R_t，并与独立从原始查询生成的OEDR报告对比。
 - Long-Chain Task (LCT)：从初始报告R_0开始，基于前一次生成的报告依次更新，形成长链，测试多步维护能力。
3. 模型配置：
 - Open-source Configuration (OC)：使用Qwen3.5-397B-A17B作为Researcher agent（其他角色可能还包括planner/writer等，具体结构在论文中较详细，但此处仅有片段）。
 - Proprietary Configuration (PC)：使用专有模型（具体未在可见片段中说明）。
4. 基准数据集：DeepResearch Bench（采用RACE作为官方质量指标）和DeepConsult（采用GPT-4.1平均质量评分）。
5. 评估视角：
 - Report Quality（报告质量）：沿用各benchmark官方指标。
 - Report Continuity（报告连续性）：对比内容级ROUGE-L F1和大纲级EM F1，衡量更新后报告与原报告的结构和内容保持程度。
 - Research Cost（研究成本）：统计每个报告的token消耗和搜索调用次数。
6. 对比设置：以OEDR（一次性生成）为baseline，对比Incremental-OEDR在SST和LCT下的表现。
7. 其他实验可能包括：不同时间点的性能曲线、不同任务的连续性与质量权衡等，但可见材料有限。

Q5: 发现了什么实验现象？

1. 质量与信息可用性的耦合：在DeepResearch Bench上，MS-Agent(OC)的RACE分数从2016年的47.57上升到2025年的49.28；在DeepConsult上从6.31上升到7.27。这表明OEDR系统的表现强烈依赖于当时可获取的信息丰富度，因此在评估持续研究系统时必须考虑时间维度——仅用最新信息评估会高估报告质量。
2. 连续性提升：Incremental-OEDR相比OEDR在内容级ROUGE-L F1上最高提升0.51，在大纲级EM F1上提升0.63，说明结构化更新机制能够很好地保留报告原有结构和内容，避免从零生成带来的“记忆丢失”。
3. 成本大幅降低：token消耗降低33%，搜索调用减少61%，体现了证据池复用和结构化检索带来的直接效率收益。
4. 长链稳定性：从检索到的结果表格片段看，IOEDR(OC)在DeepConsult上的质量分从2016年的6.32逐步上升到2025年的7.46（LCT）；IOEDR(PC)从6.55升至7.50。这表明长链更新并不导致质量崩溃，反而能随时间积累逐渐提高。不过该片段缺乏完整上下文，具体指标名称（可能是RACE或平均分）需在原文中确认。
5. 配置鲁棒性：在OC和PC下均观察到收益，说明Structured Harness不依赖特定基础模型。
6. 需要进一步确认的细节：论文中“↓22% RACE”的标注（图1中）与摘要中“33% token reduction”存在表观不一致，推测可能指不同配置下的变异或不同的成本度量，建议阅读原文图表说明。

Q6: 有什么可以进一步探索的点？

1. 扩展到其他报告类型：当前评估集中在DeepResearch Bench和DeepConsult，可推广到医疗、金融、法律等专业领域的持续研究报告中。
2. 更长时间跨度与细粒度时间步：十年年度粒度可以扩展为季度/月度更新，或跨越二十年以上的长尾信息环境，研究知识过时与更新的动力学。
3. 证据冲突处理：当新增证据与已有证据矛盾时，如何自动决定保留哪方或生成争议分析，是一个关键挑战。
4. 误差累积问题：尽管LCT实验表明质量未崩溃，但长链更新的误差漂移、信息失真仍需系统化研究，可结合一致性校验机制。
5. 选择性更新的粒度控制：目前以章节为单位，未来可探索段落级、句子级甚至事实级更新，在保存结构的同时提高更新精度。
6. 证据池的遗忘与归档：持久化证据池可能无限增长，需要设计证据重要性衰减、去重、压缩或归档策略，控制长期成本。
7. 用户交互与反馈：将用户对旧报告的修改作为增量信号，支持人机共同维护报告，或根据用户兴趣调整更新优先级。
8. 多语种与多模态：扩展到多语言报告、跨语言检索，以及包含图表、表格、图像证据的富媒体报告更新。
9. 评估体系完善：目前的连续性指标（ROUGE-L、EM）可能无法捕捉事实级延续，需要设计更细粒度的“知识保持率”或“事实翻新率”指标。

Q7: 总结一下论文的主要内容

论文针对深度研究报告的长期维护问题，提出Incremental-OEDR设定与Structured Harness框架。论证主线：现有OEDR系统一次性从零生成报告，缺乏对报告随时间演化的支持，导致重复劳动和连续性的缺失。技术主线：通过将报告表示为大纲-章节-证据的结构化集合，配合结构化检索、持久化证据池和结构化生成，系统能够在旧报告基础上进行选择性更新；这样的设计使智能体可以精准定位过时内容、复用有效证据、按需获取新信息，而不是每次重新开始。实验主线：构建了2016-2025十年时间评估框架，利用Tavily的end_date控制信息可见范围，设计了SST和LCT两种任务分别测试单步更新和长链维护；在DeepResearch Bench和DeepConsult上，使用开源配置（Qwen3.5-397B-A17B）和专有配置进行验证，从报告质量、报告连续性和研究成本三个视角评估。核心结果包括：相比一次性OEDR，Incremental-OEDR在内容级ROUGE-L F1提升0.51、大纲级EM F1提升0.63、token消耗降低33%、搜索调用减少61%，同时质量基本持平；时间维度分析显示OEDR性能随信息可用性增长而提升，说明时间因素对评估持续研究系统至关重要。论文还公开了项目页面，便于复现和扩展。整体上，这项工作将“研究报告维护”这一实际需求形式化为可评估的研究问题，并提供了有效的技术方案和评估框架。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中agent（权重0.1）方向直接相关：论文研究的是LLM智能体驱动的深度研究系统，并设计了复杂工具调用（evidence_store、Tavily）和持久记忆机制。

## 基本信息

- 作者：Meilin Chen, Hongyuan Bao
- 机构：论文未在元数据中明确标注机构；根据通讯邮箱（chenmeilin2@xiaohongshu.com、merlinis@zju.edu.cn）合理推断作者可能来自小红书（xiaohongshu.com）和浙江大学（zju.edu.cn），但此推断未获论文正文直接确认。
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-10-08
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.11566v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据（hash:hash:768，共97 chunks）及论文摘要与可见节选，结合heuristic_draft完成；部分未在证据中出现的内容基于论文上下文合理推断并已标注。
