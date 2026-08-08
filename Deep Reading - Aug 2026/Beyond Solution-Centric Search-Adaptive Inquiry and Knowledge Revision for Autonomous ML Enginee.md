---
user_id: "cheng tan"
paper_id: 6226
arxiv_id: "2608.02143v1"
title: "Beyond Solution-Centric Search: Adaptive Inquiry and Knowledge Revision for Autonomous ML Engineering"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02143v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02143v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:56:49"
---
# Beyond Solution-Centric Search: Adaptive Inquiry and Knowledge Revision for Autonomous ML Engineering

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：autonomous agents · machine learning engineering · information paradigm · knowledge revision

## 一句话总结

Iris 提出“信息范式”替代传统的“以方案为中心的搜索”，通过询问-修正循环（Inquiry-Revision Loop）实现自主机器学习工程中的自适应信息获取与知识演化。

## 摘要

> Long-horizon autonomous research tasks such as machine learning engineering require systems to make interdependent decisions under a limited budget. Existing LLM-based agents typically organize candidate-solution improvement through tree, graph, or chain structures, meaning that the search process determines how information is acquired and managed. We call this design solution-centric search and propose instead the information paradigm, in which an evolving information state represents the system's understanding of the task and guides solution improvement. We instantiate this paradigm in Iris, an inquiry-revision loop. For information acquisition, Iris generates local action plans from the current information state and uses epistemic actions to probe decision-critical unknowns without modifying the retained solution. For information management, Iris synthesizes observations across experiments into task knowledge composed of revisable claims with explicit scope and status. It updates this knowledge as new evidence arrives and constructs each decision context from raw evidence, structured summaries, or task knowledge at the required level of detail. On MLE-Bench, Iris attains a 64.9% any-medal rate under a 12-hour budget, the highest among compared systems. Across four tasks spanning harness engineering and model post-training, Iris also demonstrates cross-domain generalization.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：长程自主研究的复杂性
机器学习工程（MLE）是长程自主研究的一个典型且极具挑战性的实例。它不仅要求系统具备代码编写能力，更要求在数据准备、模型设计、实验实现和结果评估之间做出复杂的、相互依赖的决策。所有这些操作都必须在固定的时间或计算预算内完成。

### 现有范式的局限性：以方案为中心的搜索（Solution-Centric Search）
目前的 LLM 智能体（如基于树搜索、图搜索或多分支搜索的系统）普遍遵循“以方案为中心”的设计。在这种范式下：
1. **信息获取受限：** 预定义的搜索拓扑（如树或图）限制了系统如何从当前状态选择下一步行动。信息获取被动地服务于方案的生成和修改。
2. **缺乏认识性行动（Epistemic Actions）：** 现有系统的行动大多是干预性的（Interventional），即直接修改或评估候选方案。缺乏专门用于探测、澄清或验证关键未知信息的手段，导致在不确定性较高时盲目试错。
3. **知识管理薄弱：** 系统对任务的理解往往是碎片化的。在长程研究中，早期的结论可能被后期的证据推翻，但现有系统缺乏有效的机制来跨实验综合信息并修正已有的知识主张。

### 核心痛点：预算与理解的张力
在有限预算下，盲目的试错成本极高。如果系统不能随着实验的进行不断深化对任务的理解（即演化其“信息状态”），就无法在后续决策中选择更有效的方向。因此，如何将“信息”而非“方案”置于研究过程的中心，是提升自主研究系统效能的关键。

Q2: 有哪些相关研究？

### 自主科学研究系统
通用自主科学系统（如 Lu et al. 2024; Schmidgall et al. 2025）涵盖了从研究构思、实验执行到分析报告的全流程。Iris 属于这一大类，但更专注于 MLE 这一特定且高难度的工程领域。

### MLE 智能体与搜索协议
在机器学习工程领域，现有的 LLM 智能体主要通过不同的搜索和精炼协议来优化可执行方案：
- **树搜索（Tree Search）：** 如 Jiang et al. 2025 和 Zhu et al. 2026，通过分支和回溯寻找最优解。
- **图搜索（Graph Search）：** 如 Toledo et al. 2025，允许更复杂的方案演化路径。
- **目标组件精炼：** 如 Nam et al. 2025，专注于对方案的特定部分进行迭代。
- **预算感知搜索：** 如 Chen et al. 2026，在多分支搜索中考虑计算预算。

### 范式对比
尽管上述方法在实现细节上各异，但它们都将“候选方案”作为组织原则。Iris 的不同之处在于提出了“信息范式”，强调研究的进步本质上是系统“信息状态”的改变，这一观点在自主智能体领域具有前瞻性。

Q3: 论文如何解决这个问题？

### 信息范式（Information Paradigm）
Iris 的核心思想是将研究过程建模为“信息状态”的不断演化。系统不再仅仅是为了寻找一个更好的代码实现，而是为了构建一个关于任务的完整知识体系，并利用该体系来指导方案的改进。

### 询问-修正循环（Inquiry-Revision Loop）
Iris 通过一个闭环流程来实现信息范式，主要包含两个核心阶段：

#### 1. 信息获取（Inquiry）：自适应与探测
- **自适应行动计划：** Iris 不遵循固定的搜索树，而是根据当前的信息状态动态生成局部行动计划。这意味着行动的拓扑结构是随研究进程自适应调整的。
- **认识性行动（Epistemic Actions）：** 系统可以主动执行不修改保留方案的行动，专门用于探测决策关键的未知量（例如：运行一个小规模实验来验证某个超参数的影响，或检查数据集的分布特征）。这使得系统在做出重大决策前能先消除不确定性。

#### 2. 信息管理（Revision）：综合与多粒度访问
- **跨实验知识修正：** Iris 将所有实验观察综合为“任务知识”。这些知识由一系列“主张”（Claims）组成，每个主张都有明确的适用范围（Scope）和状态（Status）。当新证据出现时，系统会主动修正、更新或废弃旧的主张。
- **多粒度上下文构建：** 在进行下一次规划时，Iris 能够根据需要从不同粒度提取信息：
 - **原始证据：** 具体的日志、输出或代码片段。
 - **结构化摘要：** 对单个实验的总结。
 - **任务知识：** 经过综合和修正的高层级理解。
这种机制确保了决策上下文既包含必要的细节，又具备全局的视野。

Q4: 论文做了哪些实验？

### 实验设置
- **基准测试：** 使用完整的 MLE-Bench，这是一个衡量 LLM 智能体在真实机器学习任务中表现的权威基准。
- **预算限制：** 设定了 12 小时的固定时间预算，模拟真实的工程压力。
- **跨领域验证：** 除了 MLE 任务，还在四个跨领域任务上进行了测试，包括测试框架工程（Harness Engineering）和模型后训练（Model Post-training），以验证系统的通用性。

### 对比实验与消融研究
- **基准对比：** 将 Iris 与现有的 SOTA MLE 智能体进行对比。
- **组件消融：** 分别验证了认识性行动、知识修正机制以及不同信息粒度对最终性能的贡献。
- **骨干模型研究：** 测试了 Iris 在不同底层 LLM（如 GPT-4o, Claude 3.5 Sonnet 等）上的表现稳定性。
- **轨迹分析：** 对询问和知识修正的过程进行了定性分析，展示了系统如何通过修正错误认知来最终达成目标。

Q5: 发现了什么实验现象？

### 核心发现
- **SOTA 性能：** 在 MLE-Bench 上，Iris 达到了 64.9% 的“任意奖牌”（any-medal）率，这是目前所有对比系统中的最高分，证明了信息范式在处理复杂工程任务时的优越性。
- **跨领域泛化：** Iris 在非 MLE 任务中同样表现出色，说明其询问-修正循环是一种通用的科学研究范式，不局限于特定的代码生成任务。
- **知识修正的价值：** 轨迹分析显示，Iris 能够识别并纠正早期实验中产生的误导性结论。这种“自我纠错”能力是其在长程任务中保持正确方向的关键。
- **认识性行动的效率：** 引入认识性行动后，系统在盲目修改方案上的尝试显著减少。通过先“询问”再“行动”，Iris 能够更高效地利用有限的计算预算。
- **指标间的张力：** 实验观察到，在极高复杂度的任务中，维护复杂的知识状态会消耗一定的上下文窗口，这在底层模型能力受限时可能导致推理负担增加，但在强模型（如 Claude 3.5）支持下，这种权衡是高度正向的。

Q6: 有什么可以进一步探索的点？

### 潜在研究方向
- **更高级的知识表示：** 目前的任务知识主要基于文本主张，未来可以探索引入形式化逻辑、概率图模型或更复杂的知识图谱来增强系统对因果关系的理解。
- **长期记忆与跨任务学习：** 研究如何让 Iris 在多个不同的研究任务之间迁移知识，实现真正的“经验累积”。
- **人机协同研究：** 探索人类专家如何通过干预 Iris 的信息状态（例如提供先验知识或修正错误主张）来加速研究进程。
- **计算效率优化：** 随着研究周期的延长，信息状态会变得非常庞大。如何高效地检索、压缩和更新这些信息，同时保持决策的准确性，是一个重要的工程挑战。
- **多模态信息整合：** 在某些科学领域，实验结果可能包含图像、波形等非文本数据，扩展 Iris 以处理多模态证据将是必要的。

Q7: 总结一下论文的主要内容

这篇论文提出了“信息范式”（Information Paradigm），旨在解决自主机器学习工程（MLE）等长程研究任务中，现有智能体因过度关注“方案搜索”而忽视“信息积累”的问题。作者认为，自主研究的本质应该是系统“信息状态”的不断演化和深化。

基于这一范式，作者开发了名为 Iris 的系统。Iris 的核心是一个“询问-修正”（Inquiry-Revision）循环。在“询问”阶段，Iris 突破了传统的固定搜索结构，根据当前对任务的理解动态生成局部计划，并引入了“认识性行动”，允许系统在不改变现有方案的情况下，通过探测性实验来消除关键的不确定性。在“修正”阶段，Iris 展现了强大的知识管理能力，它能将跨多个实验的观察结果综合成一套动态更新的“任务知识”。这套知识由具备明确适用范围的主张组成，能够随着新证据的出现而自动修正，从而避免了系统被早期错误结论误导。

实验结果令人印象深刻：在权威的 MLE-Bench 基准测试中，Iris 在 12 小时的预算限制下取得了 64.9% 的奖牌率，刷新了行业纪录。此外，Iris 在测试框架工程和模型后训练等跨领域任务中也表现出了极强的泛化能力。消融实验进一步证实，认识性行动和知识修正机制是提升系统性能的关键因素。总的来说，Iris 的成功证明了将信息获取与修正作为自主研究系统核心设计目标的正确性，为未来开发更智能、更具自我演化能力的科学研究智能体指明了方向。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接关联智能体（Agent）架构设计和 AI for Science 方向。

## 基本信息

- 作者：Shaokang Fu, Yulong Tao, Linbo Jin, Jiarong Zhao, Qiming Shi, Tianjun Pan, Haonan Li, Chengyu Wang, Jia Wu, Chengfu Huo
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.02143v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点结合了 Introduction 和 Conclusion 的核心论点，并对 Iris 的询问-修正循环进行了深度解析。
