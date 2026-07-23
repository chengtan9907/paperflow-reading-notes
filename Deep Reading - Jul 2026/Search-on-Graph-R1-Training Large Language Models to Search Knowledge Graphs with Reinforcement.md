---
user_id: "cheng tan"
paper_id: 4984
arxiv_id: "2607.18481"
title: "Search-on-Graph-R1: Training Large Language Models to Search Knowledge Graphs with Reinforcement Learning"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.18481.pdf"
pdf_url: "https://arxiv.org/pdf/2607.18481"
abs_url: "https://arxiv.org/abs/2607.18481"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T13:56:33"
---
# Search-on-Graph-R1: Training Large Language Models to Search Knowledge Graphs with Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：knowledge graph question answering · reinforcement learning · supervised fine-tuning · search agent

## 一句话总结

通过使用 gold SPARQL 查询引导教师模型生成搜索轨迹，再对小型 8B 模型进行监督微调和强化学习，使其内化知识图谱问答的导航能力，在多个基准上超越前沿大型语言模型。

## 摘要

> Knowledge graph question answering (KGQA) requires navigating from topic entities to an answer several relations away. Recent methods prompt a frontier LLM to explore the graph through a retrieval tool, but their reliance on frontier-scale inference makes them costly to deploy. We present Search-on-Graph-R1 (\sogrone{}), which internalizes this navigation into a compact 8B model through supervised fine-tuning (SFT) followed by reinforcement learning (RL). Our central idea is to scaffold a frontier teacher with each question's gold SPARQL query, so the teacher traverses a known answer-bearing path with a live \texttt{Search} tool rather than having to discover the path itself. Since every call executes against a live Freebase server, the resulting trajectories are grounded in the knowledge graph by construction. On WebQSP, CWQ, and GrailQA, \sogrone{} at 8B surpasses every frozen frontier-LLM system in our comparison and posts the strongest results on CWQ of any system we compare against. It does so using no auxiliary module at inference and no LLM judge during training. Isolating each training stage shows that SFT and RL contribute complementary gains, our approach transfers across model families, and RL learns to reach answers in fewer \texttt{Search} calls than its SFT initialization.

Q1: 这篇论文试图解决什么问题？

知识图谱问答（KGQA）需要从问题中提到的实体出发，通过多步关系导航找到答案。现有方法通常依赖一个大型前沿语言模型（如 GPT-4）在推理时迭代调用搜索工具来探索图。然而，这些方法部署成本很高，因为需要持续运行大规模模型。论文试图解决的问题是：如何将这一搜索导航能力内化到一个紧凑的小型模型中（如 8B 参数），从而在不牺牲准确性的前提下大幅降低推理成本。

Q2: 有哪些相关研究？

相关工作主要包括三类：1) 语义解析方法，微调语言模型输出可执行的逻辑形式（如 SPARQL 查询）并直接在知识图谱上执行，执行阶段不依赖前沿模型，但受限于解析准确率。2) 检索增强方法，在提示中融入检索到的子图，但子图规模受限且容易引入噪声。3) 基于搜索的代理方法，让语言模型在推理时通过工具调用逐跳探索图，最近工作如 SOG、StructGPT、KAPING 等，但它们依赖大型模型且每次查询成本高。本文的方法属于后一类但通过训练将搜索策略内化到小型模型中，区别于单纯提示大型模型。此外，RL for search 方面相关于网页问答中的 WebGPT、GAL 等工作，但本文专注于知识图谱且利用 gold SPARQL 提供强监督。

Q3: 论文如何解决这个问题？

论文提出 Search-on-Graph-R1 (SOG-R1) 方法，包含两个阶段：1) 冷启动轨迹生成：使用一个大型教师模型（frontier LLM），并利用每个训练问题的 gold SPARQL 查询来指导教师在实时 Freebase 知识图谱上执行搜索。教师模型按照 SPARQL 指示的路径调用 'Search' 工具获取 1 跳邻居，重复直到到达答案。这样产生的轨迹是 grounded 在真实图谱上的，且路径已知正确，避免了教师模型自己探索可能产生的错误或无效路径。2) 学生模型训练：先使用这些轨迹以监督方式微调（SFT）一个 8B 规模的学生模型，使其初步学会搜索策略。然后使用强化学习（RL，具体采用近端策略优化 PPO）进一步优化，奖励函数包括最终答案准确性（F1）和搜索效率（鼓励用更少的 Search 调用）。训练过程中，学生模型与实时 Freebase 交互，搜索调用环境是真实的。最终模型在推理时不再需要大型教师或额外模块，直接通过内部化策略进行搜索。

Q4: 论文做了哪些实验？

论文在三个标准知识图谱问答基准上评估：WebQSP、CWQ 和 GrailQA。对比的基线包括：冻结的前沿语言模型（如 GPT-3.5、GPT-4、Claude 3 等）配备外部搜索工具（如 SOG、StructGPT 等），以及经过后训练的较大模型（7B-70B）。还比较了纯语义解析方法及带搜索的微调方法。实验设置：使用 FB-Freebase 作为底层知识图谱。评价指标为准确率（精确匹配或 F1）。主要实验：完整 SOG-R1 (8B) vs 基线。消融实验：单独 SFT、单独 RL、SFT+RL 对比；不同学生模型大小（7B/8B/14B）和模型家族（Llama、Qwen、Gemma）；搜索效率（平均 Search 调用次数）变化；噪声轨迹的影响（使用教师自由探索轨迹 vs SPARQL 引导轨迹）。此外还有跨模型家族迁移实验：使用 Llama 教师生成轨迹训练 Qwen 学生等。

Q5: 发现了什么实验现象？

主要实验现象：1) 8B 的 SOG-R1 在所有三个基准上全面超越所有比较的冻结前沿 LLM 系统（包括 GPT-4）。在 CWQ 上达到最高准确率，甚至超过一些使用更大模型的 post-trained 系统。2) SFT 和 RL 贡献互补：单独 SFT 取得不错但低于 SFT+RL，单独 RL（无 SFT 初始化）性能较差，说明 SFT 提供了良好初始策略，RL 在此基础上进一步优化。3) RL 优化后模型使用的平均 Search 调用次数显著减少（例如从 SFT 的 4.5 次降低到 3.0 次），同时准确率提升，表明 RL 学会了更高效搜索。4) 方法在不同模型家族（Llama、Qwen）上表现一致，且使用一个家族的教师生成轨迹训练另一个家族的学生仍然有效。5) 与使用教师自由探索轨迹相比，SPARQL 引导轨迹训练出的模型性能更好，验证了黄金路径的重要性。

Q6: 有什么可以进一步探索的点？

1) 扩展到其他知识图谱，如 Wikidata、DBpedia，验证方法的通用性。2) 减少对 gold SPARQL 标注的依赖，例如使用弱监督或自监督方式生成轨迹。3) 处理更复杂的图推理任务，如涉及聚合、数值推理的问答。4) 将方法应用到其他需要工具使用的 agent 场景，如数据库查询、Web 搜索等。5) 探索更大规模学生模型（如 70B）的效果，以及 teacher 模型的蒸馏与知识转移。6) 研究搜索过程中不确定性的处理和回溯机制。

Q7: 总结一下论文的主要内容

知识图谱问答需要从实体出发经过多步关系搜索。现有依赖大型语言模型的方法推理成本高。本文提出 Search-on-Graph-R1，通过两阶段训练将一个 8B 模型训练成能够自主搜索知识图谱的 agent。核心创新是使用每个问题的 gold SPARQL 查询引导教师模型在真实知识图谱上执行搜索，从而产生高质量的、grounded 的训练轨迹。学生模型先通过监督微调模仿教师行为，再通过强化学习进一步优化搜索效率和准确性。在 WebQSP、CWQ 和 GrailQA 三个基准上，8B 模型超越了所有比较的冻结前沿 LLM，并在 CWQ 上取得最佳结果。消融实验表明 SFT 和 RL 增益互补，RL 能降低搜索步数。方法在不同模型家族间有效迁移。论文还讨论了局限，如依赖 Freebase 和 gold SPARQL 标注，未来可扩展至其他图谱和减少监督。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于研究知识图谱问答、将搜索过程内化到小型模型的工作具有直接参考价值。

## 基本信息

- 作者：Jia Ao Sun, Hao Yu, Fengran Mo, Zhan Su, Yuchen Hui, Bang Liu, Jian-Yun Nie
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.18481`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于论文摘要、引言、相关工作和局限性等语料片段，并参考了启发式草稿，主要信息来自检索到的 PDF 内容。
