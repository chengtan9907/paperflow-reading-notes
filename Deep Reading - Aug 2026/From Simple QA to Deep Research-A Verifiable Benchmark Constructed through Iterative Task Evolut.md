---
user_id: "cheng tan"
paper_id: 6209
arxiv_id: "2608.02163v1"
title: "From Simple QA to Deep Research: A Verifiable Benchmark Constructed through Iterative Task Evolution"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02163v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02163v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:54:07"
---
# From Simple QA to Deep Research: A Verifiable Benchmark Constructed through Iterative Task Evolution

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：deep research · benchmark construction · task evolution · directed acyclic graph

## 一句话总结

提出一个由简单问答迭代演化而来的可验证深度研究基准，包含 500 个任务、31 个主题、10 大类别和三种查询形式，用 DAG 组织原子步骤并配套事实锚定的逐点评分标准，实验证明该基准能清晰区分模型能力且评估细粒度、稳定、与人类对齐。

## 摘要

> Deep research benchmarks require expert-level tasks and reliable evaluation grounded in task-specific knowledge. Existing benchmarks rely heavily on expert authoring or pre-existing human-authored materials, while fully automatic construction struggles to ensure consistent and traceable verification. To address this gap, we introduce a verifiable benchmark of 500 deep research tasks spanning 31 topics and 10 major categories, with three query forms designed to probe complementary capabilities required for deep research. The benchmark is constructed automatically using an iterative Explorer–Formalizer–Challenger pipeline that progressively transforms simple questions into deep research tasks. Concretely, each task is represented as a directed acyclic graph (DAG) of atomic steps and associated checkpoints, enabling the query, DAG, and rubrics to evolve together in a controlled manner. Experiments demonstrate that the benchmark clearly discriminates among models and across query types, while its fact-grounded pointwise rubrics enable fine-grained, human-aligned, and stable evaluation. Our data, implementation, and results are publicly available at https://github.com/chr6192/TaskEvolving.git.

Q1: 这篇论文试图解决什么问题？

1. 深度研究任务的评估难点：深度研究允许多种有效路径和答案，同时高质量回答需要领域知识，这种开放性和专业性使得固定统一评估不可行，需要基于任务特定知识的细粒度评分标准。
2. 现有自动构建基准的困境：现有基准（如 AgentDisCo、QUEST、DR-Arena）依赖报告互评，得分与参与比较的模型集合相关，不能提供回答质量的绝对度量；DeepResearchEval 虽然用 LLM 生成任务特定评分标准，但生成标准的质量专业性和可靠性没有保证。
3. 核心缺口：如何在不依赖专家编写或预有人类材料的前提下，自动构建多样且专业的深度研究任务，并确保评估标准的可验证性、可追踪性以及与任务特定知识的一致性。
4. 构建过程的挑战：简单问题到深度研究任务的转化需要受控地增加复杂度（如多步骤、多角度、多源搜索），同时确保每步都可验证，这需要一个系统的任务演化与形式化机制。

Q2: 有哪些相关研究？

1. 深度研究评估基准：
 - AgentDisCo、QUEST、DR-Arena：通过让模型相互比较报告来评估，得分相对有效但依赖比较集合，无法给出绝对质量度量。
 - DeepResearchEval：认为通用评估维度不足，用 LLM 为每个查询生成 1-3 条任务特定评分标准，但生成标准的可靠性和专业质量未得到保证。
2. 基准构建范式对比：
 - 专家构建：可靠但成本高、规模受限，难以覆盖多样主题。
 - 基于已有材料（如人类撰写的报告/问答）：可追踪但任务受原有材料约束，难以生成真正开放的研究任务。
 - 全自动构建：本文所属路线，追求无需人工干预，但需解决验证一致性和评分可追踪性两大问题。
3. 任务复杂度演化：本文采用迭代式任务演化，从简单 QA 逐步增加步骤和回答维度，与课程学习、任务生成等思路相关（合理推断），但核心创新在于将演化过程形式化为 DAG + 检查点，使难度增长可控且可验证。
4. 评估指标形式：点状评分（pointwise rubrics）相比整体打分或两两比较，能提供细粒度诊断和绝对分数，本文进一步将评分标准锚定在事实和来源上，增强可核查性。

Q3: 论文如何解决这个问题？

1. 总体思路：利用 Wikipedia QA 语料作为起点，通过迭代式 Explorer–Formalizer–Challenger 流水线自动构建深度研究任务，无需专家编写或预有人类材料。
2. 任务表示：每个任务表示为一个有向无环图（DAG），节点为原子步骤，边定义步骤间的依赖；每个节点关联检查点（checkpoint），用于验证该步骤是否完成以及回答是否基于事实。DAG 作为核心结构载体，使任务可分解、可追踪、可验证。
3. 迭代演化机制：
 - 每一轮由 Explorer 提出新的问题变体，Formalizer 将其形式化为带 DAG 和检查点的结构化任务，Challenger 尝试攻击或检验任务的质量与验证性，三者协同使查询、图结构和评分标准共同演进。
 - 示例：Round 1 将简单查询 q1 变为需要额外一步才能解决的任务；Round 2 进一步变为具有多个回答方面的查询；经过 T 轮后得到需要多源搜索和综合分析的高级深度研究任务。
4. 三种查询形式：设计用于探测互补的深度研究能力，具体形式未在摘要中详述（合理推断为事实型、分析型、综合型等）。
5. 评分标准：采用事实锚定的逐点评分标准（fact-grounded pointwise rubrics），每条标准对应可核验的事实或来源，从而提供绝对质量度量、细粒度诊断以及人类可审查的一致性。

Q4: 论文做了哪些实验？

1. 基准规模：500 个深度研究任务，覆盖 31 个主题、10 个大类、三种查询形式（来源于摘要和引言）。
2. 评估模型：共测试 10 个模型（具体模型名称未在摘要中列出，来自检索片段），覆盖不同能力和规模。
3. 评估协议：使用事实锚定的逐点评分标准对模型输出进行评分；对比不同模型、不同查询类型的表现；可能采用 LLM-as-judge 或自动评分（合理推断）。
4. 消融实验：移除工具（可能指搜索/检索工具）后对比模型表现，检验任务对外部工具和长尾知识的需求。
5. 人类对齐与稳定性分析：验证自动评分与人类判断的一致性以及多次评估的稳定性（来自摘要中的“human-aligned, and stable evaluation”）。
6. 公开资源：提供了数据、代码和实验结果，便于复现和扩展。

Q5: 发现了什么实验现象？

1. 模型区分度：基准能清晰区分 10 个模型，说明任务难度梯度设计有效，模型能力差异被显著放大。
2. 查询类型差异：不同查询类型的得分模式不同，证实三种查询形式确实在探测互补的深度研究能力。
3. 工具消融的显著影响：移除工具导致所有 10 个模型在所有查询类型上的得分大幅下降，总平均分下降约 0.14；证据型（evidential）和分析型（analytical）检查点的平均分各下降约 0.07。这说明任务中编码了大量长尾信息，模型必须依赖外部工具才能获取，单凭参数化知识无法解答。
4. 评估性质：事实锚定的点状评分标准提供了细粒度反馈，自动评分与人类判断一致性高，且重复评估稳定。
5. 负结果（合理推断）：工具消融后仍有一定残余分数，表明模型具备一定内部知识或推理能力，但不足以支撑完整深度研究。

Q6: 有什么可以进一步探索的点？

1. 降低构造开销：当前 pipeline 在任务演化阶段依赖 frontier model，成本较高；可探索用更小模型蒸馏、主动采样、或混合人机协作来降低成本。
2. 扩展任务多样性：扩大主题范围（如科学、医学、法律）、增加语言覆盖，或将 Wikipedia 语料替换为更专业的语料以支持 AI for Science 等应用。
3. 自适应难度控制：利用 DAG 的拓扑结构动态调节原子步骤数量和检查点粒度，按目标模型能力生成合适难度的任务。
4. 过程监督训练：利用检查点和 DAG 结构为 agent 提供过程级奖励信号，用于训练更好的推理与规划策略（合理推断）。
5. 评分标准生成质量：探索自动生成 rubric 的专业性保障机制，例如引入领域知识库或人工审核环节，增强可信度。
6. 更丰富的查询形式：除三种之外，可加入多模态、交互式或时间敏感型查询，覆盖更多真实研究场景。
7. 考察评估稳定性边界：系统性地分析评分模型、温度参数、评分标准措辞等对结果稳定性的影响。

Q7: 总结一下论文的主要内容

本文聚焦深度研究（deep research）基准的自动构建与可验证评估问题。深度研究任务允许多种有效解决路径，同时要求领域知识，这使得传统固定答案评估失效，需要任务特定的细粒度评分标准。现有基准或依赖专家编写（成本高、规模有限），或依赖人类已有材料（开放性受限），或采用模型互评（相对分数，无法提供绝对质量度量），或自动生成评分标准但可靠性不足。针对这一缺口，作者提出一个完全自动构建的可验证基准，包含 500 个深度研究任务，覆盖 31 个主题、10 个大类，并设计了三种查询形式来探测互补的深度研究能力。

技术核心是迭代式 Explorer–Formalizer–Challenger 流水线。该流水线从 Wikipedia QA 语料中的简单问题出发，逐步将其演化为深度研究任务。每个任务被形式化为一个有向无环图（DAG），节点是原子步骤，边表示依赖，每个节点关联检查点以验证步骤完成情况及事实依据。演化过程受控：每一轮增加一个或多个步骤、增加回答维度，最终形成需要多源搜索和综合分析的高难度任务。查询文本、DAG 结构和评分标准三者共同演化，确保任务复杂度与评估标准始终对齐。评分采用事实锚定的逐点评分标准（pointwise rubrics），每条标准都对应可核查的事实或来源，从而支持细粒度诊断、绝对分数和人类可审查性。

实验方面，作者测试了 10 个模型在三种查询类型上的表现，结果显示基准能清晰区分模型能力，且查询类型间的得分模式存在差异。工具消融实验发现，移除工具后所有模型在所有查询类型上得分均大幅下降，总平均分下降约 0.14，证据型和分析型检查点平均分各下降约 0.07，说明任务中编码了大量长尾信息，必须依赖外部工具。同时，事实锚定的评分标准表现出与人类判断一致、重复稳定的评估性质。作者在结论中承认当前构造过程使用前沿模型导致成本较高，但认为这是构建可信基准的必要投资。所有数据、代码和结果已公开，便于社区复现与扩展。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向高度相关：深度研究是 tool-augmented agent 的核心应用，本文的基准可用于评估 agent 的规划、工具使用和综合分析能力。

## 基本信息

- 作者：Can Wang, Haoran Chen, Haowen Gao, Hao Ding, Zhaoyang Liu, Zhiying Tu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02163v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（retrieved_evidence 中的 Background/Method/Results/Limitations 片段），并结合摘要、引言和结论部分进行了归纳与合理推断。
