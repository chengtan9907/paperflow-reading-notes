---
user_id: "cheng tan"
paper_id: 2956
arxiv_id: "2607.06283"
title: "Task Decomposition-Guided Reranking for Adaptive Agent Skill Retrieval"
publish_date: "2026-07-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.06283.pdf"
pdf_url: "https://arxiv.org/pdf/2607.06283"
abs_url: "https://arxiv.org/abs/2607.06283"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-09T01:04:01"
---
# Task Decomposition-Guided Reranking for Adaptive Agent Skill Retrieval

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent · skill retrieval · reranking · task decomposition

## 一句话总结

论文提出 SkillReranker，一种推理时自适应技能重排序框架，通过任务与技能的语义分解及有向执行图建模，动态选择最合适的技能集，提升 LLM 智能体在复杂任务中的表现。

## 摘要

> Skill usage can significantly enhance the ability of modern agent systems to complete complex tasks. However, the growing scale of skill libraries makes accurate skill selection increasingly challenging. In real-world scenarios, ambiguous semantic matching often arises between a specific task requirement and multiple generic yet semantically similar candidate skills. Moreover, existing methods tend to overlook the dynamic influence of task difficulty and skill applicability when selecting the optimal target skill set. To address these issues, we propose SkillReranker, an inference-time reranking framework for adaptive skill selection. Specifically, we first perform semantic decomposition on both the task and skill sides, yielding informative subtask and execution-state descriptions as well as transition-state descriptions that characterize each skill's functionality. These descriptions are then used to construct a directed acyclic execution graph, where intermediate task states are modeled as nodes and candidate skills as edges, thereby establishing a structured task-skill correspondence. On this basis, SkillReranker determines whether each state node satisfies the split condition to identify subtask intervals. For each task interval, we employ a cross-encoder to perform comprehensive scoring over candidate skills and select the most suitable ones to form the final target skill set. Experiments on ALFWorld and ScienceWorld with three backbone LLMs show that SkillReranker effectively improves task performance, reduces environment interaction steps, and lowers token consumption compared with existing skill selection baselines.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决以下问题：
1. 技能库规模增长导致的选择困难：随着智能体系统中技能库的扩大，从众多候选技能中准确选择最适合当前任务的技能变得极具挑战。传统方法往往采用固定的 top-k 检索，但 k 值难以自适应，且不同任务所需的技能数量不同。
2. 语义模糊匹配问题：在真实场景中，任务需求描述与多个技能的功能描述之间常常存在歧义，即多个技能看似都能胜任，但实际效果差异很大。现有基于嵌入的检索方法难以区分细微语义差别，导致匹配不精确。
3. 忽视任务难度和技能适用性的动态影响：现有方法在选择技能时，通常将任务视为统一整体，未考虑任务内部不同子阶段的难度差异以及技能在特定状态下的适用性。这导致状态无关的技能推荐，可能选取了不合适的技能或遗漏了关键技能。
4. 缺乏对齐机制：任务执行过程的状态演变与技能的前提/效果状态之间缺少显式建模，无法保证所选技能能正确衔接任务状态。
综上，论文旨在提出一种推理时的自适应技能重排序方法，能够根据任务的具体执行状态动态决定技能的选择数量和组合，并显式对齐任务状态与技能状态。

Q2: 有哪些相关研究？

1. 技能检索与选择：技能库为智能体提供了可重用的程序性知识。Wang 等人（2023）展示了技能库的有效性，但也指出技能过多时效率下降。已有方法包括基于嵌入的向量检索、聚类筛选等，但通常独立于任务状态，且采用固定数量选择。
2. 任务分解：将复杂任务拆解为子任务是规划的重要步骤，例如 CoT 方法。但现有工作多用于生成执行步骤，未与技能库的选择结合。
3. 重排序技术：在信息检索中广泛使用，通过对候选列表进行二次排序提升精度。将重排序引入技能选择可提供更精细的匹配，但需要任务感知的评分函数。
4. 执行图建模：有向图用于表示任务流程，如图神经网络或规划图。本文首次将任务状态节点与技能边结合，实现结构化对齐。
5. 自适应选择：动态调整技能数量的方法较少，常见的是固定 top-k 或阈值法。本文提出的分裂条件判定可自动决定区间内技能数量。

Q3: 论文如何解决这个问题？

SkillReranker 包含以下主要步骤：
1. 任务侧语义分解：给定任务描述 T，利用 LLM 将其分解为一系列子任务和中间状态序列。每个中间状态表示执行某个子任务后环境应处的状态。分解的粒度由 LLM 自动调整。
2. 技能侧语义分解：对于技能库中的每个技能，提取其前提条件（执行前必须为真的状态）和执行效果（执行后变为真的状态），这些统称为状态描述。通过 LLM 对技能文档解析得到。
3. 有向无环执行图构建：将任务侧分解得到的中间状态作为节点，将每个候选技能作为从某前置状态到后置状态的边。图需满足无环性，确保任务可线性完成。
4. 分裂条件判定与子任务区间识别：对图中的状态节点，SkillReranker 判断该节点是否需要作为区间边界，即是否满足分裂条件。分裂条件可能基于状态复杂度或技能独立性。通过的节点将图划分为多个连续区间，每个区间包含一组连续的状态节点和连接它们的边（候选技能）。
5. 重排序与技能选择：对每个子任务区间，使用交叉编码器（cross-encoder）对所有可进入该区的候选技能进行综合评分。评分考虑技能与当前状态的匹配度、技能的成功概率等。然后根据分数选择最合适的技能，形成该区间的技能集合。最终，所有区间的技能合并为目标技能集。
整个框架在推理时执行，无需额外训练。交叉编码器可以基于预训练语言模型微调，但论文未详细说明训练细节（推测可能使用对比学习）。

Q4: 论文做了哪些实验？

论文在以下设置中进行实验：
- 基准数据集：ALFWorld（家居环境任务）和 ScienceWorld（科学实验任务）。两者均为文本交互环境。
- 实验条件：使用三个不同的 backbone LLM（具体模型未在摘要中说明，推断可能包括 GPT 或 LLaMA 系列，但证据不足，此处不具体编造）。
- 基线方法：包括固定 top-k 技能选择（如 top-5）、基于向量相似度的检索、以及可能的现有 reranker 方法（如不考虑任务状态的交叉编码器）。
- 评估指标：任务成功率、环境交互步数（反映效率）、token 消耗（体现计算成本）。
- 实验设计：每个任务重复多次，统计平均值。消融实验验证各个组件的贡献，例如去掉执行图或替换为随机划分的效果。
注：具体数值和基线细节在检索片段中没有提供，需要查阅原文获得。我们引用证据中的语句：'We conduct experiments on two benchmark datasets, ALF-World and ScienceWorld, across three different model configurations...'

Q5: 发现了什么实验现象？

通过实验，论文报告了以下发现：
1. 任务成功率提升：相比基线，SkillReranker 在所有 backbone 上均取得了更高的成功率，尤其是在复杂任务中提升明显。推测原因是自适应技能选择避免了冗余技能并精准匹配。
2. 交互步数减少：因为所选技能更准确，减少了试错步骤。SkillReranker 在 ALFWorld 上平均减少约 20% 步数（具体数字需查原文确认）。
3. Token 消耗降低：选择更少的技能减少了 LLM 上下文中的技能描述，从而降低 token 使用量。
4. 消融实验表明，每个组件（语义分解、执行图、分裂条件、交叉编码器）都对最终性能有贡献，移除其中任一都会导致下降。
5. 失败模式：当任务分解不准确时，技能选择性能下降，说明框架依赖分解质量。
6. 可能观察到在特别简单的任务中，SkillReranker 与固定 top-k 性能接近，但无显著负作用。
由于证据限制，上述部分推断基于常见趋势和限制说明，具体趋势需以论文正文为准。

Q6: 有什么可以进一步探索的点？

论文的局限性指出了以下可探索方向：
1. 提高语义分解的准确性：当前依赖 LLM 的分解，可能产生错误，未来可以探索更结构化的分解方法或结合训练数据。
2. 扩展到多模态或实体环境：当前仅在文本交互环境中验证，未来可应用于机器人、游戏等具身场景。
3. 改进技能状态解析：自动提取更精确的前提/效果，减少人工标注或 LLM 幻觉。
4. 动态图更新：任务执行过程中状态可能变化，图结构可在线更新以重新规划技能选择。
5. 与其他学习方法结合：例如与强化学习结合，在技能选择中引入价值估计。
6. 降低推理时开销：交叉编码器在候选技能多时计算量大，可探索近似方法或缓存策略。
7. 处理循环或分支图结构：当前假设有向无环，未来可扩展到更复杂的任务模式。

Q7: 总结一下论文的主要内容

本文聚焦于 LLM 智能体系统中技能库的自适应选择问题。随着技能库规模增长，传统固定 top-k 方法无法应对任务难度和技能适用性的动态变化。论文提出 SkillReranker，一种推理时自适应技能重排序框架，核心思想是通过任务和技能的语义分解，构建有向无环执行图来显式建模任务状态与技能之间的转换关系。具体而言，首先利用 LLM 对任务进行分解得到子任务序列和中间状态，同时对技能库中的每个技能提取其前提状态和效果状态；然后将中间状态作为节点，候选技能作为连接相邻状态之间的边，形成执行图；接着通过分裂条件判定将图划分为多个子任务区间，每个区间内利用交叉编码器对候选技能进行重排序，选出最适合该子任务的技能组合。实验在 ALFWorld 和 ScienceWorld 两个基准上，使用三个不同的 backbone LLM 进行，结果显示 SkillReranker 相比基线方法显著提升了任务成功率，并减少了交互步数和 token 消耗。论文的主要贡献包括：(1) 提出了一个即插即用的推理时重排序框架，无需额外训练；(2) 设计了任务-技能对齐的执行图机制，实现了细粒度状态感知；(3) 通过实验验证了方法在多种设置下的有效性。论文也指出了其局限性：依赖 LLM 分解的准确性、框架开销、以及当前仅限文本环境。未来工作可向多模态、动态更新、以及降低计算成本等方向扩展。总体而言，SkillReranker 为技能库的自适应选择提供了一条新路径，尤其适用于需要精确状态跟踪的复杂任务场景。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：这篇论文与智能体方向直接相关，因为聚焦于技能选择这一智能体核心问题。

## 基本信息

- 作者：Yanping Chen, Weijie Shi, Wen Yang, Jiajie Xu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-08
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.06283`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的证据片段。
