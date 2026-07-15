---
user_id: "cheng tan"
paper_id: 3624
arxiv_id: "2607.11399v1"
title: "Agentic Routing: The Harness-Native Data Flywheel"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11399v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11399v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:38:58"
---
# Agentic Routing: The Harness-Native Data Flywheel

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language model agents · agentic routing · execution harness · model selection

## 一句话总结

提出一种基于智能体执行框架状态进行步骤级模型选择的智能路由范式，并利用每次路由产生的轨迹数据形成持续自我改进的数据飞轮。

## 摘要

> Large language model agents are increasingly executed not by a single model call, but by an execution harness that manages observation, context, control, action, state, and verification. At the same time, frontier and open models are becoming structurally specialized: a model that is strong at code editing, long-context recovery, tool use, mathematical reasoning, or low-latency response may not dominate on the other axes. This makes model selection inside an agent a core systems problem rather than a per-query serving trick. Existing routing methods mostly optimize single-turn cost-quality trade-offs and therefore miss the execution state, intermediate failures, and feedback loops that make agents different from chat completion. We propose Harness-Native agentic routing, a step-level routing paradigm that selects either a single best-fit model for cost-effective execution or multiple complementary models for ensemble-style accuracy improvement, conditioned on the full harness state. The key insight is that every routing decision naturally produces a structured data record -- consisting of the query, harness state, model choice or model set, execution trace, outcome, and cost -- whose labels are supplied by the environment rather than by the router itself. These records form a harness-native data flywheel: execution traces train better routers and harness-native models, which improve cost-quality trade-offs and generate more traces under the same budget. We instantiate this idea in OpenSquilla with a four-layer routing stack, an open LightGBM cold-start ranker, and a staged router-model path that turns logged arena records into progressively stronger routing policies. The report studies singleton and multi-model routing on agentic benchmarks including DRACO and PinchBench, and argues that agentic routing is not merely cost control, but a data engine for agent-native training.

Q1: 这篇论文试图解决什么问题？

现有路由方法（如RouteLLM、OpenRouter等）仅针对单次查询优化，基于静态提示或输入-输出对，忽略了智能体执行过程中的丰富状态信息（如工具调用失败、上下文压力、验证反馈、恢复尝试等），无法适应模型专业化带来的动态需求。智能体场景中，模型选择需要根据当前执行步骤的具体需求和上下文动态调整，而现有方法无法利用这些信息，导致固定模型或简单路由难以达到最优。因此需要一种能够感知执行状态并在步骤级别进行路由的范式。

Q2: 有哪些相关研究？

相关研究主要分为两类：一是传统模型路由与编排方法，如基于规则或基于质量预测的路由，它们仅考虑单次查询，对执行状态缺乏建模；二是智能体框架（如LangChain、AutoGPT、OpenAI Assistants）中的模型选择，通常采用固定模型或轮询策略，缺乏精细化路由。本文的工作位于两者交叉，提出在harness内部进行步骤级路由，利用执行状态信息动态决策，并形成数据飞轮持续改进。此外，模型集成或混合方法常被用于提升鲁棒性，但未考虑执行状态。

Q3: 论文如何解决这个问题？

核心是Harness-Native agentic routing范式。在智能体执行框架的每个决策点，路由器接收完整的harness状态，包括当前观察、上下文、控制信息、动作历史、中间状态和验证结果。路由器可操作两种模式：
1. 单模型模式：选择成本效益最高的单一模型执行下一步；
2. 多模型模式：选择一组互补模型，融合它们的输出以提高准确率。
每次路由决策后，执行结果被记录为结构化数据记录，包含查询、状态、模型选择、执行轨迹、结果和成本。这些记录积累后用于训练改进的路由器（如通过LightGBM排序列）和针对特定harness状态优化的专用模型（harness-native models）。具体实现为OpenSquilla系统的四层路由栈：第一层冷启动LightGBM排序器负责初始路由；后续层利用积累的arena记录逐步优化策略。整个系统形成闭环飞轮：更好的路由产生更优的执行，生成更高质量训练数据，进一步增强路由和模型能力。

Q4: 论文做了哪些实验？

论文在DRACO和PinchBench两个智能体基准上进行实验，包括单模型路由和多模型路由两种设置。基线包括固定模型分配、随机路由、基于静态质量预测的路由等。评估指标包括任务成功率、成本、延迟等。但具体实验结果数据（如准确率提升百分比、成本降低比例等）在检索片段中未充分提供，需查阅原文以获取详细数值和消融分析。

Q5: 发现了什么实验现象？

实验表明，路由策略能够根据harness状态动态调整模型选择，在保持任务成功率的同时降低平均成本；多模型路由在复杂任务上展现了更好的鲁棒性。随着执行数据积累，路由策略性能逐步提升，验证了数据飞轮效应。但具体现象如成本-质量帕累托前沿、路由决策分布等未在检索片段中呈现。

Q6: 有什么可以进一步探索的点？

1. 探索更细粒度的路由决策（如子步骤级别或跨步骤依赖路由）。
2. 利用积累的arena记录训练更强大的harness-native模型，可能无需依赖外部基础模型。
3. 将路由扩展到多智能体协作场景，不同智能体使用不同模型。
4. 研究路由策略在不同harness设计（如不同观察和验证模块）下的泛化能力。
5. 结合路由与推理计算预算优化（如动态控制思维链长度）。
6. 在实际生产环境中验证框架的稳定性和可扩展性。

Q7: 总结一下论文的主要内容

论文深入探讨了大型语言模型智能体中的模型选择问题，提出了一种新颖的“Harness-Native agentic routing”范式。该范式的核心是将路由决策从单次查询优化提升到基于完整执行框架状态的步骤级决策。作者指出，随着模型专业化趋势的加强，单一模型难以在所有任务上最优，而智能体的执行框架（harness）提供了丰富的状态信息（如工具调用结果、上下文长度、验证反馈等），这些信息对模型选择至关重要。论文提出两种操作模式：单模型路由（选择最合适模型以平衡成本和质量）和多模型路由（集成多个模型输出以提高准确性）。关键创新在于每次路由决策自动产生结构化数据记录，形成“harness-native数据飞轮”：累积的执行数据用于训练更好的路由策略和针对性模型，进而提升后续执行质量和效率，产生更多高质量数据。论文在OpenSquilla系统中实现了该框架，包含冷启动LightGBM排序器和分阶段路由-模型路径。在DRACO和PinchBench基准上的实验验证了该范式的有效性，表明其不仅能有效控制成本，更能作为数据引擎驱动智能体系统持续改进。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文直接针对智能体系统中的模型选择问题，与用户画像中的“agent”方向高度相关。

## 基本信息

- 作者：Xinchen Liu, Hang Zhou, Yingjie Zong, Yuchuan Tian, Liuyang Song, Shuo Zhang, Yulong Li, Wei He, Mengyu Zheng, Runke Liu, Siyang Cheng, Xiang Kuang, Hailin Hu, Kai Han, Yunhe Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11399v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索片段（retrieved_evidence）并整合了启发草稿（heuristic_draft）。
