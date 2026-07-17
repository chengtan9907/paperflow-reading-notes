---
user_id: "cheng tan"
paper_id: 3897
arxiv_id: "2607.12893v1"
title: "MemOps: Benchmarking Lifecycle Memory Operations in Long-Horizon Conversations"
publish_date: "2026-07-14"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.12893v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.12893v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-16T01:59:20"
---
# MemOps: Benchmarking Lifecycle Memory Operations in Long-Horizon Conversations

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-term memory · conversational agents · benchmark · lifecycle operations

## 一句话总结

MemOps将长期对话记忆评估重构为一系列显式的生命周期操作（记住、忘记、更新、反思等），通过操作级探针实现可解释的诊断，揭示了当前系统在有序记忆状态重建等能力上的不足。

## 摘要

> Long-term memory has become a foundational capability for LLM-based agents that accompany users across extended, multi-session interactions. Existing benchmarks, however, evaluate such memory almost exclusively through downstream question answering, scoring only the correctness of a final answer. This black-box formulation conflates the heterogeneous causes of memory failure, such as missing the introduction of a relevant fact, binding an operation to the wrong target, or relying on stale values after a correction. As a result, it can credit correct answers despite their reliance on inconsistent or unsafe memory states. In this paper, we argue that, in dynamic long-horizon interactions, memory is not a static collection of facts but a lifecycle of explicit operations, including remembering, forgetting, updating, reflecting, and their compositions. We introduce MemOps, a benchmark that reformulates conversational memory as a sequence of lifecycle operations and represents each memory event with a structured trace specifying its trigger, target, scope, state transition, and supporting evidence. A controllable generation pipeline embeds these operations into long, task-oriented conversations and produces gold operation traces together with six categories of operation-level probes, evaluated under both adjacent-evidence and long-context settings. Across long-context, retrieval-based, parametric and managed-memory systems, MemOps disentangles failure modes that final-answer accuracy alone conceals, revealing that current systems remain far from uniformly reliable. For instance, session-level retrieval outperforms turn-level retrieval, and long-context models remain notably weak at reconstructing ordered memory-state trajectories. These results move long-term memory evaluation from final-answer scoring toward interpretable, operation-level diagnosis.
> Date: July 15, 2026
> Author Legend: $\dagger$ Corresponding authors (lizy@memtensor.cn, yuxliang@outlook.com)
> Github: https://github.com/MemTensor/MemOps

Q1: 这篇论文试图解决什么问题？

论文试图解决现有长期对话记忆评估的黑盒化问题。现有基准（如MLVU、LongBench、L-Cite等）通常通过在一个长历史后提出一个问题，仅评分最终答案的正确性。这种范式将不同的记忆失败原因混为一谈：例如，模型可能正确回答了问题，但却是基于不正确的记忆状态（如使用了过时的事实）。此外，这些基准没有显式评估记忆的生命周期操作，如记住、更新、忘记、反思等，而这些操作在动态交互中至关重要。因此，需要一个可解释的、操作级的评估框架来诊断记忆系统的真实行为。

Q2: 有哪些相关研究？

相关研究包括长期对话基准（如MLVU、LongBench、L-Cite、Loogle等）和记忆增强系统（如检索增强、参数记忆、托管记忆等）。这些基准虽然覆盖了长上下文，但评估范式仍然是最终的问答评分。部分基准虽然命名了特定操作（如更新），但在评估时仍将其转化为单轮问答，没有追踪记忆状态的变化。在记忆系统方面，有基于检索的（如RAG）、参数记忆（如通过微调）、托管记忆（如外部记忆模块）等方法。MemOps超越了这些现有工作，通过结构化的操作跟踪和操作级探针提供细粒度的诊断。

Q3: 论文如何解决这个问题？

MemOps的解决方案包括三个关键组件：1）定义了一组生命周期操作：记住（remember）、忘记（forget）、更新（update）、反思（reflect）及其组合。每个操作由触发条件、目标、范围、状态转换和支持证据定义。2）构建了一个可控的生成管道，能够将操作嵌入到长的任务导向对话中，自动生成gold操作轨迹。3）设计了六类操作级探针，分别在邻接证据（短上下文）和长上下文设置下评估。探针包括操作识别、状态追踪、一致性检查等。评估时，使用一个LLM裁判（如DeepSeek）对系统输出的记忆状态进行诊断，计算精确率和召回率等指标。

Q4: 论文做了哪些实验？

论文在四种类型的记忆系统上进行了实验：1）长上下文模型（如DeepSeek-V2-0516、Llama-3.1-8B、Qwen2.5-72B等）；2）检索增强系统（session-level和turn-level检索组合不同的编码器和索引）；3）参数化记忆系统（如通过近端策略优化微调的模型）；4）托管记忆系统（如MemWalker、MemorySandbox等）。实验在MemOps生成的合成对话上进行，对话长度覆盖数十轮到数百轮。评估包括操作级指标（如操作检测的精确率/召回率、状态追踪的准确率）和最终问答准确率作为对比基线。

Q5: 发现了什么实验现象？

实验揭示了几个关键现象：1）会话级检索（将整个会话作为检索单元）在所有操作类型上优于轮次级检索（仅当前轮次或相邻轮次），尤其在忘记和更新操作上提升显著。2）长上下文模型在长上下文设置下表现不佳，尤其是在需要重建有序记忆状态轨迹时（例如更新序列），而邻接证据设置下相对较好，表明长上下文模型对顺序记忆状态的保持能力有限。3）托管记忆系统在操作级指标上表现参差不齐，某些系统在反思操作上较好但忘记操作很差。4）最终答案准确率与操作级指标之间存在显著脱节：一些系统虽然最终答案正确，但操作级指标显示其记忆状态不一致。5）LLM裁判本身存在一定不稳定性，引入了评估噪声。6）在邻接证据设置中，过时的值有时被错误地保留，导致更新操作失败。

Q6: 有什么可以进一步探索的点？

未来工作可以探索的方向：1）扩展操作类型，加入更复杂的组合操作或跨会话操作。2）改进评估方法，减少LLM裁判的不稳定性，例如通过更多的样本或更细化的评分规则。3）将MemOps应用于更多类型的记忆系统，包括知识图谱和多模态记忆。4）研究如何利用MemOps的诊断结果来改进记忆系统，例如设计面向操作级优化的训练目标。5）在真实对话数据上验证MemOps的迁移性，因为当前合成数据可能无法完全覆盖真实场景的多样性。6）探索记忆状态轨迹的可视化和解释工具，帮助开发者调试记忆错误。

Q7: 总结一下论文的主要内容

本论文提出了MemOps，一个用于长期对话中记忆生命周期操作的基准。针对现有基准仅评估最终答案准确率而无法诊断记忆状态一致性的问题，MemOps将记忆操作显式化，定义了记住（Remember）、忘记（Forget）、更新（Update）、反思（Reflect）等基本操作及其结构化表示，每个操作包含触发条件、目标、范围、状态转换和支持证据。通过可控生成管道，可以自动生成带有gold操作轨迹的任务导向对话数据。设计了六类操作级探针（操作检测、状态一致性、溯源、范围判断、轨迹重建等），分别在邻接证据（短上下文）和长上下文设置下评估。实验覆盖了四大类记忆系统：长上下文模型、检索增强系统、参数化记忆系统和托管记忆系统。主要发现包括：会话级检索在所有操作类型上优于轮次级检索；长上下文模型在长上下文设置下对有序状态重建任务表现薄弱；最终答案准确率与操作级指标相关性低，无法反映真实记忆状态。这些结果验证了操作级评估的必要性，为长期记忆研究提供了可诊断的评估工具。论文的主要贡献在于：1）提出记忆生命周期操作框架，实现从最终答案评分到操作级诊断的转变；2）构建可控生成管道和操作级探针，支撑细粒度评估；3）通过多系统实验揭示关键记忆失败模式；4）推动记忆评估向可解释性发展。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向高度相关，因为长期记忆是智能体持续交互的基础。

## 基本信息

- 作者：Xixuan Hao, Zeyu Zhang, Zehao Lin, Yihang Sun, Ziliang Guo, Xichong Zhang, Yuxuan Liang, Feiyu Xiong, Zhiyu Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-07-14
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.12893v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据中的摘要和片段，并结合论文标题、作者信息进行合理推断，未编造具体数值。
