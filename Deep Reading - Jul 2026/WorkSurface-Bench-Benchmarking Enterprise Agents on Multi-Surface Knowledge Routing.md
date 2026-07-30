---
user_id: "cheng tan"
paper_id: 5719
arxiv_id: "2607.25765v1"
title: "WorkSurface-Bench: Benchmarking Enterprise Agents on Multi-Surface Knowledge Routing"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25765v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25765v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:53:32"
---
# WorkSurface-Bench: Benchmarking Enterprise Agents on Multi-Surface Knowledge Routing

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：enterprise agents · multi-surface knowledge routing · benchmark · document

## 一句话总结

WorkSurface-Bench 是一个用于评估企业智能体在文档、表格和依赖图等多表面对知识路由能力的基准，包含1151个可审计任务，实验发现正确路由是任务完成的必要但非充分条件，从而填补了当前基准无法区分表面选择与回答能力的缺口。

## 摘要

> Enterprise agents often need to integrate heterogeneous knowledge sources: documents for narrative facts, tables for computation, and dependency graphs for file relationships. Existing benchmarks typically evaluate retrieval or tool use without distinguishing whether an agent first selects the appropriate knowledge sources. We introduce WorkSurface-Bench, a benchmark for evaluating this capability as surface routing. It contains 1,151 atomic tasks derived from persona-scoped Workspace-Bench-Lite workspaces, spanning document, table, graph, and cross-surface questions. Its reference answers are auditable: table answers are reproduced through executed DuckDB queries, document answers are grounded in verified text spans, and graph answers are traced to source dependency annotations. We evaluate four model backbones across six controlled agent settings, yielding 27,624 protocol-error-free trajectories. Under gold-constrained tool access, agents achieve 98.7-99.8 Route F1, while Answer remains only 56.1-75.3 percent, showing that correct surface selection is necessary but insufficient for task completion. Matched interventions further show that surface hints improve Answer for three of four models, whereas removing irrelevant tools primarily improves routing and efficiency. In an independent three-annotator audit, all 200 sampled tasks pass all six quality criteria by majority vote, with 192 receiving unanimous judgments on every criterion. We release the dataset, construction pipeline, scoring code, and agent harness at https://github.com/haolpku/WorkSurface-Bench.

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：当前缺乏一个能够区分企业智能体在异构知识源之间进行表面选择（routing）与具体回答（answering）能力的基准。具体而言，现有基准如 AgentBench、ToolBench 主要评估智能体对 API 的调用能力，或者像 RGB、mmRAG 等聚焦于单一或多知识源的检索增强生成，但都没有设计来检验智能体是否意识到不同的问题需要访问不同类型的知识表面（如文档、表格、依赖图）。这种表面选择在真实企业场景中至关重要，因为同一份信息可能同时存在于文档和表格中，但检索方式截然不同。如果智能体选错表面，即使检索和工具调用能力再强也无法完成任务。因此，论文致力于创建一个专门评估路由能力的基准，并从中分离出路由与回答两个子能力，以揭示智能体失败的根本原因。

Q2: 有哪些相关研究？

相关工作包括三个主要分支：（1）通用智能体基准，如 AgentBench、API-Bank、ToolBench，侧重于评估智能体对 API 的规划和调用能力，但未涉及表面选择。（2）检索增强生成（RAG）基准，如 RGB、CRAG、KILT，主要集中在单一文本模态，最近也有工作如 mmRAG 开始组合文本、表格和知识图谱，但它们将所有表面作为一个整体，未分离路由步骤。（3）企业特定评估，如 Workspace-Bench 本身评估工作空间任务，但未专门隔离路由能力。WorkSurface-Bench 在这些基础上，通过任务设计强制要求智能体先判断所需表面，并分别测量路由和回答性能，从而提供了新的评估维度。

Q3: 论文如何解决这个问题？

论文通过以下步骤构建 WorkSurface-Bench：（1）源数据：基于 Workspace-Bench-Lite 的英文子集，该数据集包含由人工撰写并经专家验证的任务场景、依赖图和工作空间文件，来自 Lark/字节跳动的实际工作流程。（2）任务转换：将100个源任务中的1,850个评估规则转换为1,151个原子任务，每个任务有清晰的表面要求。转换采用确定性生成器和证明携带生成器，确保高质量。（3）可审计答案：对于表格问题，答案通过执行 DuckDB SQL 查询复现，并对应到具体单元格；对于文档问题，答案基于验证的文本跨度；对于图问题，答案追踪到依赖注释中的源路径。这种设计使失败可解释：低路由分表示选了错误表面，低证据分表示表面选对但提取答案失败。（4）评估协议：定义 Route F1（基于选择的工具类型与要求的工具类型），Answer Accuracy（答案字符串精确匹配或语义等价），Evidence F1（基于所需证据跨度的召回率）。在六个受控设置下（如无限制工具、黄金约束工具、仅表面提示、移除不相关工具等）评估智能体，以隔离不同因素对路由和回答的影响。

Q4: 论文做了哪些实验？

论文进行了一系列实验：（1）主要评估：使用四个主流 LLM 骨干（模型包括 GPT-4o, Claude 3.5 Sonnet, Llama 3 70B, Mistral Large 等），在六个智能体设置下运行，共收集27,624条无协议错误轨迹。设置包括：基线（所有工具可用）、黄金约束（只允许要求的表面工具）、表面提示（在系统提示中提供表面提示）、移除不相关工具（移除任务不需要的工具）、以及组合。（2）路由与回答解耦分析：计算每种设置下的 Route F1 和 Answer，并统计两者的相关性。（3）匹配干预实验：在相同任务上比较表面提示与无提示，以及有无不相关工具的差异，以量化不同干预的效果。（4）人工审核：随机抽取200个任务，由三位标注员依据六个质量标准（如问题明确、答案正确、证据充分等）进行独立审核，最终通过多数投票确定质量，并计算一致性。

Q5: 发现了什么实验现象？

实验揭示的主要现象包括：（1）在黄金约束工具设置（即只提供任务所需表面工具）下，智能体路由 F1 高达98.7-99.8%，几乎完美选择正确表面；但回答准确率仅56.1-75.3%，显示即使选对表面，智能体仍无法可靠得出正确答案。（2）路由与回答之间的斯皮尔曼相关系数仅为0.62，表明两者中度相关，进一步说明路由好不等于回答好。（3）表面提示（在提示中明确指出问题涉及哪些表面）能提升三个模型（GPT-4o、Claude、Llama）的回答准确率，但对其中一个模型无改善甚至略有损害。（4）移除不相关工具主要提升路由效率（减少工具调用数）并略微改善路由准确率，但对回答的影响不一。（5）人工审核显示所有200个任务至少在三个标注员中的多数票下通过所有质量标准，其中192项获得全一致意见，证明任务质量可靠。

Q6: 有什么可以进一步探索的点？

未来可探索的方向包括：（1）扩展到更真实的动态企业环境，其中知识表面可能发生变化或新增。（2）引入多轮交互，评估智能体在对话中逐渐调整路由策略的能力。（3）将表面路由与更复杂的推理链（如多步SQL、文档跨段推理）结合。（4）考虑成本与延迟优化，因为不必要的表面调用会增加开销。（5）增加多语言任务，扩大表面类型（如数据库、API、黑板）。（6）更细粒度的错误分析，区分路由错误是由于表面识别失败还是工具选择错误。

Q7: 总结一下论文的主要内容

论文 WorkSurface-Bench 针对企业智能体在多表面知识源中的路由能力评估问题提出了一个专门的基准。作者首先指出现有评估的不足之处：它们要么考察检索（RAG），要么考察工具调用，但未能判断智能体是否具备根据问题选择合适知识表面（路由）的能力。在企业应用中，知识通常分布在文档、电子表格和依赖图等不同表面，选择错误表面将导致后续所有操作失效。为填补这一空白，论文从 Workspace-Bench-Lite 数据集中通过半自动转换得到1,151个原子任务，覆盖五个角色场景，并确保每个任务有明确的表面标签和可审计的参考答案。参考答案的可审计性是一大特色：表格答案通过执行 DuckDB SQL 查询复现，确保数值计算正确；文档答案通过已验证的文本跨度定位；图答案通过依赖关系源注释追溯。这使得路由与回答可以分别测量，从而清晰定位失败原因。在实验设计上，论文采用四种代表性 LLM 骨干和六种不同受控设置，收集大量轨迹数据。黄金约束工具设置下，路由 F1 接近完美（98.7-99.8%），但回答准确率仅56.1-75.3%，这一显著差距表明路由是必要但非充分条件。通过匹配干预实验进一步发现，表面提示能改善回答（特别对三个模型有效），而移除不相关工具有助于提升路由效率和准确率。路由与回答之间的相关系数仅为0.62，印证了分离评估的重要性。此外，论文还进行了严格的人工审核，确保基准质量。最终，论文提出了一个标准化的评估流程，并开放了所有资源。整体上，WorkSurface-Bench 提供了一个新颖且必要的评估维度，为未来企业智能体研究提供了新的测试平台。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文直接针对企业智能体（agent）方向，与用户关注方向高度重合

## 基本信息

- 作者：Hao Liang, Meiyi Qiang, Sizhe Qiu, Linzhuang Sun, Wentao Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.DB
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25765v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence），并优先使用了各字段对应的field_evidence_map中的证据锚点。
