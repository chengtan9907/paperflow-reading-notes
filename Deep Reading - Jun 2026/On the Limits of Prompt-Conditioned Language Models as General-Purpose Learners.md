---
user_id: "cheng tan"
paper_id: 1229
arxiv_id: "2606.23668"
title: "On the Limits of Prompt-Conditioned Language Models as General-Purpose Learners"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23668.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23668"
abs_url: "https://arxiv.org/abs/2606.23668"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:05:50"
---
# On the Limits of Prompt-Conditioned Language Models as General-Purpose Learners

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · prompt conditioning · cheap-talk game · PAC-Bayes bounds

## 一句话总结

本文通过将用户-LLM交互建模为廉价谈话博弈，并应用PAC-Bayes理论，证明了在提示条件下语言模型存在两种不可约的误差下界：表达性下界（由语言信道容量限制导致）和目标错配下界（由对齐约束导致），从而表明提示条件语言模型并非通用问题求解器。

## 摘要

> Large Language Models (LLMs) are frequently portrayed as general-purpose solvers capable of solving arbitrary tasks. We argue that this view overlooks a fundamental constraint: language is a compressed and capacity-limited interface for conveying task information. Modelling User--System interaction as a bilevel \emph{cheap-talk} game, we analyse how latent tasks are encoded into prompts and reinterpreted under alignment and safety constraints. We introduce a conceptual decomposition separating task inference from execution and derive PAC-Bayes bounds that distinguish finite-sample estimation error from irreducible structural limitations. Our first main result establishes an \emph{expressivity floor}: language acts as a capacity-limited communication channel, and whenever the informational complexity of a task family exceeds the capacity of that channel, distinct tasks become unavoidably indistinguishable to the Solver, inducing a strictly positive error floor that cannot be eliminated by additional data, optimisation, or model scaling alone. We then establish an \emph{objective-misalignment floor}: when alignment constraints restrict the admissible output set, the User-ideal distribution may lie outside the feasible class, inducing an irreducible distortion. Together, these results yield a formal negative conclusion: prompt-conditioned LLMs are not universal problem solvers through prompting alone, as there exist task families for which correct behaviour is provably unattainable even in the infinite-data regime. More broadly, our analysis shows the limits of prompt-based generalisation arise from information-constrained communication and alignment-constrained objectives. This suggests that interfaces beyond natural language, including multimodal observations and, external memory, may reduce the inherent LLM limitations by increasing the task-relevant information available to the System.

Q1: 这篇论文试图解决什么问题？

大型语言模型（LLM）被广泛宣传为能够解决任意任务的通用求解器。然而，这一观点忽略了语言作为任务信息传输介质的根本限制：语言是压缩且容量有限的通信信道。本文试图回答：在提示条件下，语言模型是否真的能成为通用问题求解器？是否存在理论上的性能极限，使得即使拥有无限数据和无限模型容量，某些任务也无法通过提示正确解决？本文从信息论和博弈论角度，严格证明存在两种不可约的误差，表明提示条件LLM不是通用求解器。

Q2: 有哪些相关研究？

已有工作研究了自回归模型的计算表达力和复杂度理论限制（如LJL+20, FC20），但对提示条件语言模型的样本效率和泛化极限的理论表征很少。本文填补了这一空白，通过廉价谈话博弈和PAC-Bayes分析，提供了提示条件下泛化极限的理论下界。此外，关于对齐和安全的研究多聚焦于经验方法，本文从理论上揭示了目标错配带来的不可约误差。

Q3: 论文如何解决这个问题？

本文采用双层廉价谈话博弈（cheap-talk game）建模用户与LLM的交互：用户（Sender）观察潜在任务，通过自然语言提示（信号）传递给LLM（Receiver/ Solver），LLM生成响应。将任务推理与执行概念性分离：LLM首先从提示推断任务，然后基于推断执行任务。利用PAC-Bayes框架推导泛化界，区分两种误差来源：①有限样本估计误差（可通过数据减少）；②由通信信道容量限制导致的表达性误差下界（expressivity floor），当任务族的信息复杂度超过信道容量时，不同任务在提示中变得不可区分；③由对齐约束导致的目标错配误差下界（objective-misalignment floor），当对齐限制可行输出集时，用户理想分布可能落在可行类外。理论分析表明，后两种误差在无限数据、无限模型容量和最优优化下仍然存在，构成不可约的性能下限。

Q4: 论文做了哪些实验？

本文为纯理论分析，未设计实验验证。所有结论基于数学推导和PAC-Bayes界，不涉及具体数据集或模型对比。

Q5: 发现了什么实验现象？

无实验现象。理论推导得出关键结论：在完全揭示（fully revealing）情形下，通信导致的零样本和少样本适应障碍消失，但目标错配引起的下界仍然存在；当存在更大错配或信息瓶颈时，性能差距在渐近数据下也无法克服。

Q6: 有什么可以进一步探索的点？

1. 探索超越自然语言的接口，如多模态输入（图像、视频）、外部记忆和工具使用，以增加任务相关信息，可能缓解表达能力限制。2. 设计新的训练目标或对齐方法，减少目标错配误差下界。3. 将理论分析扩展到更复杂的交互场景（多轮对话、强化学习环境）。4. 在具体任务族（如数学推理、代码生成）中量化理论下界与实际性能之间的差距。5. 研究不同容量信道（如受限token预算）对表达性下界的具体影响。

Q7: 总结一下论文的主要内容

本文从理论上严格论证了提示条件语言模型并非通用问题求解器。作者将用户与LLM的交互建模为廉价谈话博弈，其中用户通过自然语言提示传递任务信息，LLM作为解答者进行推断和执行。通过引入概念分解，将任务推理与执行分离，并应用PAC-Bayes框架推导泛化界。论文的核心贡献在于建立了两个不可约误差下界：
1. 表达性误差下界（Expressivity Floor）：语言作为通信信道具有容量限制（比如固定token长度、离散符号集）。当任务族的信息复杂度（如任务分布的熵）超过信道容量时，不同任务会被映射到相同的提示分布，导致LLM无法区分它们。这种模糊性造成的误差下界是正数，且无法通过收集更多数据、更好的优化或扩大模型容量来消除——因为问题本身源自信息传输的瓶颈。
2. 目标错配误差下界（Objective-Misalignment Floor）：对齐训练（如RLHF、安全约束）会限制LLM生成的输出范围（例如避免有害内容）。然而，用户理想的任务求解分布可能恰好位于限制区域之外（例如某些合法但敏感的科学问题），导致LLM即使有能力正确回答，也被迫输出偏离最优的结果。这种误差下界同样不可约，因为它源自训练目标与真实目标之间的结构冲突。

论文进一步讨论了这些下界在零样本和少样本学习中的表现：当通信信道完全揭示任务（即提示足以唯一确定任务）时，表达性下界消失，但目标错配下界仍存；当存在信息瓶颈或目标错配时，随着数据增加，性能趋于一个大于零的常数，而非无限逼近最优。
论文最后指出，这些限制本质上源于语言本身的压缩特性和对齐约束，而非模型容量或数据量。因此，突破这些限制可能需要超越纯自然语言的接口，例如结合多模态感知、外部记忆或工具使用，以增加系统可用的任务相关信息。本文为理解LLM的能力边界提供了严谨的理论基础，对于AI安全评估、任务设计和接口设计具有重要指导意义。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：对于智能体（agent）研究方向，本文揭示了纯文本提示作为任务通信信道的根本限制，提示多模态感知和工具使用对于提升智能体性能的必要性。

## 基本信息

- 作者：David Mguni, Julian Ma, Jun Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23668`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告参考了PDF检索证据，主要依据Abstract和Main Results段落的语义命中信息，结合heuristic_draft和field_evidence_map生成。由于论文为纯理论工作且未提供实验细节，部分字段（如实验观察）直接指出无实验内容。
