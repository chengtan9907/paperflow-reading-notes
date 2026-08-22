---
user_id: "cheng tan"
paper_id: 9081
arxiv_id: "2608.19854"
title: "Repo0: Design-Driven Zero-to-All Code Generation"
institution: "Shanghai Jiao Tong University"
publish_date: "2026-08-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.19854.pdf"
pdf_url: "https://arxiv.org/pdf/2608.19854"
abs_url: "https://arxiv.org/abs/2608.19854"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-21T23:58:01"
---
# Repo0: Design-Driven Zero-to-All Code Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：code generation · software architecture · large language models · autonomous agents

## 一句话总结

Repo0 是一个基于设计驱动的持续结构演化框架，通过双向有向无环图（Dual-DAG）和模块化度量引导，实现了从自然语言需求到完整软件仓库的“零到全”代码生成。

## 摘要

> Large language model agents have made substantial progress in code generation, yet most existing systems assume a predefined repository architecture. This assumption does not hold in zero-to-all code generation, where an agent must construct an entire software project directly from natural-language requirements while maintaining a modular repository architecture throughout development. We present Repo0, a continuous structural evolution framework for zero-to-all code generation. Repo0 maintains an explicit architectural state instantiated as a Dual-Directed-Acyclic-Graph (Dual-DAG), consisting of a requirement-level DAG, a component-level DAG, and their alignment relation. Starting from natural-language requirements, it iteratively evolves component boundaries through structural actions guided by modularity metrics until structural convergence, after which the converged architecture guides test-driven development code generation. We evaluate Repo0 on six real-world repositories from RepoCraft using GPT-5 mini and DeepSeek V3.2. Repo0 achieves the highest Functionality Coverage and Pass Rate across all settings. Compared with RPG, the strongest repository-planning baseline, Repo0 improves Functionality Coverage by up to 20.08 percentage points and Pass Rate by up to 29.74 percentage points. Ablation and structural-evolution analyses further demonstrate the importance of the Dual-DAG architectural state, modularity-guided structural evolution, and explicit structural convergence.

Q1: 这篇论文试图解决什么问题？

### 1. 核心挑战：从零到全的架构真空
“从零到全”（Zero-to-All）的代码生成要求智能体仅凭模糊的自然语言需求，构建出包含多个模块、复杂依赖和完整测试体系的软件仓库。这与传统的单函数或单文件生成有本质区别：
- **架构缺失**：初始状态下不存在任何文件结构，智能体必须在编写代码前自行设计合理的模块划分。
- **动态演化需求**：软件设计是一个迭代过程，一次性规划往往难以预见实现过程中的逻辑冲突或冗余。
- **模块化维持**：在长序列生成中，如何保持代码的内聚性和低耦合，防止生成“面条式”代码，是现有 LLM 智能体的短板。

### 2. 现有方案的局限性
- **预定义架构假设**：许多系统（如 SWE-bench 相关的智能体）假设仓库结构已存在，仅负责修复或添加功能。
- **静态规划缺陷**：如 RPG 等前作虽然引入了规划图，但将其视为一次性生成的产物。一旦初始规划存在缺陷，后续的代码填充将步履维艰，缺乏纠偏机制。
- **缺乏显式状态维护**：大多数智能体依赖对话上下文来记忆结构，缺乏对仓库架构的显式、结构化表示，导致在大规模项目中容易丢失全局视角。

### 3. 研究目标
本文旨在建立一种“设计驱动”的生成范式，将架构设计从代码实现中解耦，并通过持续的结构演化（Structural Evolution）来逼近最优的软件设计方案，从而提升生成代码的质量和可维护性。

Q2: 有哪些相关研究？

### 1. LLM 代码生成智能体
早期的研究集中在单片段生成（如 HumanEval）。随着模型能力的提升，出现了 Devin、OpenDevin 和 SWE-agent 等仓库级智能体。这些系统通常采用“感知-规划-执行”循环，但在处理从零开始的大规模项目构建时，往往缺乏对软件工程原则（如模块化）的显式建模。

### 2. 仓库级规划与生成
RPG (Repository Planning Graph) 是该领域的重要基线，它通过构建需求与文件之间的映射图来指导生成。然而，RPG 的规划是静态的。Repo0 在此基础上引入了“持续演化”的概念，将规划图升级为动态演化的 Dual-DAG。

### 3. 软件工程中的模块化度量
传统的软件工程研究（如 Parnas 的信息隐藏原则）强调模块化。Repo0 将这些经典的度量指标（如耦合度、内聚度、扇入扇出等）转化为 LLM 可理解的反馈信号，作为架构演化的适应度函数（Fitness Function）。

### 4. 测试驱动开发 (TDD)
TDD 在自动化编程中被广泛用于质量保证。Repo0 在架构收敛后的代码填充阶段，深度集成了 TDD 流程，利用测试失败的反馈进行局部代码修复，确保最终生成的仓库不仅结构合理，而且功能正确。

Q3: 论文如何解决这个问题？

### 1. Dual-DAG 架构表示
Repo0 的核心是显式的架构状态表示，称为 Dual-DAG：
- **需求层 DAG ($G^R$)**：将原始自然语言需求分解为原子需求节点，并根据逻辑依赖关系建立有向边。
- **组件层 DAG ($G^C$)**：表示物理文件、类或核心函数的组织结构及其调用依赖。
- **对齐映射 ($\mathcal{A}$)**：建立需求节点与组件节点之间的多对多映射，确保每个功能点都有对应的实现载体，且每个组件都有明确的设计意图。

### 2. 持续结构演化循环 (Phase I & II)
- **初始化**：从需求分解开始，初步构建 Dual-DAG。
- **结构动作空间**：定义了一组原子操作，包括 `CreateComponent`（创建）、`MergeComponents`（合并）、`SplitComponent`（拆分）、`AddDependency`（添加依赖）等。
- **模块化引导演化**：在每一步演化中，系统计算当前架构的模块化得分（基于内聚性和耦合性），并将其作为 Prompt 的一部分输入给 LLM。LLM 根据得分和需求覆盖情况决定下一步动作。
- **结构收敛判定**：引入收敛监测机制。当连续若干步动作不再显著改变架构拓扑，且所有需求节点均已对齐到组件时，判定为“结构收敛”。

### 3. 测试驱动的代码生成 (Phase III)
- **骨架生成**：根据收敛的 $G^C$ 生成文件树和类/函数签名。
- **迭代填充与修复**：按照依赖顺序（从底向上）填充代码实现。每完成一个模块，立即生成并运行单元测试。若测试失败，则进入局部修复循环，利用错误堆栈信息修正代码，而不改变已确定的架构结构。

Q4: 论文做了哪些实验？

### 1. 实验设置
- **数据集**：使用 RepoCraft 基准，包含 6 个真实的 Python 开源仓库（requests, django, statsmodels, flask, fabric, pandas 的简化/改写版本）。为了防止模型记忆，对仓库名和变量名进行了同义词替换。
- **模型**：选用 GPT-5 mini 和 DeepSeek V3.2 作为底层推理引擎。
- **基线对比**：主要对比 RPG (Repository Planning Graph) 以及标准的 ReAct 模式智能体。

### 2. 评价指标
- **功能覆盖率 (Functionality Coverage)**：生成的代码实现了多少比例的原始需求。
- **单元测试通过率 (Pass Rate)**：通过预设测试用例的比例。
- **架构质量得分**：使用软件工程工具评估生成仓库的模块化程度。

### 3. 消融实验设计
- **w/o Dual-DAG**：移除显式的图结构，仅靠文本描述架构。
- **w/o Modularity Guidance**：移除模块化指标反馈，让 LLM 盲目演化。
- **w/o Convergence**：设定固定步数而非根据收敛判定停止。

Q5: 发现了什么实验现象？

### 1. 性能飞跃
- **整体表现**：Repo0 在所有 6 个仓库上均刷新了 SOTA。在 GPT-5 mini 驱动下，Repo0 在 `requests` 仓库上实现了 100% 的功能覆盖率，在 `django` 和 `statsmodels` 上分别达到 80.50% 和 80.68%。
- **对比 RPG**：在 `statsmodels` 任务中，Repo0 的通过率比 RPG 高出 29.74 个百分点，显示出在处理高度复杂数学/统计逻辑仓库时的卓越架构把控力。

### 2. 演化过程的动态特性
- **收敛趋势**：实验观察到，架构在演化初期（前 5-10 步）会经历剧烈的组件拆分与合并，随后模块化得分稳步上升。一旦进入收敛期，架构拓扑几乎不再变动，这证明了“先设计后实现”的有效性。
- **反直觉发现**：在某些情况下，LLM 会主动拆分已经写好的“大类”，将其重构为多个小组件以降低耦合。这种“自我重构”行为在没有模块化引导的基线中从未出现。

### 3. 失败模式分析
- **需求歧义**：当原始自然语言需求存在严重歧义时，Repo0 可能会在错误的架构方向上收敛，导致后期 TDD 无法修复逻辑错误。
- **长程依赖丢失**：在极少数情况下，跨越多个深层模块的全局状态管理在演化中可能被忽视，导致集成测试失败。
- **指标张力**：高内聚和低耦合之间有时存在张力，LLM 在极端情况下会过度拆分组件，导致调用链过长，增加了理解成本。

Q6: 有什么可以进一步探索的点？

### 1. 跨语言与多范式支持
目前 Repo0 主要在 Python 环境下验证。未来可以探索其在 Java（强类型、重设计模式）或 Rust（严格的所有权架构）中的表现，研究不同语言特性对架构演化的影响。

### 2. 增量式架构演化
研究当用户在开发中途修改或增加需求时，Repo0 如何在现有 Dual-DAG 基础上进行增量演化，而非从头开始，这更符合真实的敏捷开发场景。

### 3. 引入人类在环 (Human-in-the-loop)
开发交互式界面，允许人类架构师在演化循环的特定节点（如收敛前）介入，对 Dual-DAG 进行微调，实现 AI 的生成能力与人类设计经验的结合。

### 4. 强化学习优化演化策略
目前演化动作由 LLM 基于启发式规则选择。未来可以利用强化学习（RL），将模块化得分作为奖励函数，训练专门的架构演化策略模型。

Q7: 总结一下论文的主要内容

本文提出了 Repo0，这是一个针对“从零到全”代码生成任务的持续结构演化框架。该研究的核心贡献在于打破了传统代码生成中“一次性规划”的局限，引入了软件工程中的设计原则来驱动生成过程。Repo0 通过维护一个包含需求层和组件层的双向有向无环图（Dual-DAG），为智能体提供了一个显式的、可操作的架构状态空间。在生成过程中，Repo0 并不急于编写具体代码，而是先在模块化指标的引导下，通过迭代执行结构化动作（如合并、拆分组件）来优化仓库架构。只有当架构达到“收敛”状态——即结构稳定且完全覆盖需求时，才进入基于测试驱动开发（TDD）的代码填充阶段。实验结果表明，Repo0 在 RepoCraft 基准测试中表现优异，显著提升了生成仓库的功能完整性和代码质量。这一工作证明了显式架构建模和持续演化机制是实现复杂软件系统自动化构建的关键，为未来自主编程智能体的发展提供了新的范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文直接关联 AI Agent 在复杂软件工程任务中的应用，是当前智能体研究的热点。

## 基本信息

- 作者：Silin Chen, Haoyi Teng, Xiaodong Gu, Yuling Shi, Jiale Huang, Yongpan Wang, Hongyu Zhang, Haibing Guan
- 机构：Shanghai Jiao Tong University
- 来源：arxiv
- 主题/分类：cs.SE, cs.AI
- 日期：2026-08-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.19854`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于 Repo0 的 Dual-DAG 架构设计、模块化引导演化流程以及在 RepoCraft 基准上的详细实验数据。
