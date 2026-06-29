---
user_id: "cheng tan"
paper_id: 553
arxiv_id: "2606.28279"
title: "Agentic Hardware Design as Repository-Level Code Evolution"
institution: "根据作者背景（Brucek Khailany, Nathaniel Pinckney 等），推断该研究主要由 NVIDIA（英伟达）研究团队主导。"
publish_date: "2026-06-29"
pdf_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.28279.pdf"
pdf_url: "https://arxiv.org/pdf/2606.28279"
abs_url: "https://arxiv.org/abs/2606.28279"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-29T15:23:35"
---
# Agentic Hardware Design as Repository-Level Code Evolution

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：hardware design · agentic framework · repository-level evolution · verilog generation

## 一句话总结

HORIZON 是一个将硬件设计视为仓库级代码演化的自演化智能体框架，通过 Markdown 治理工具和 Git 工作树管理，在多个 RTL 基准测试中实现了 100% 的全自动完成率。

## 摘要

> We present HORIZON, a self-evolving agent framework that treats hardware design as repository-level code evolution. A Markdown harness is compiled into a project pack containing domain knowledge, an executable evaluator, an acceptance predicate, and a git/runtime policy; a hands-free agent loop then evolves an isolated git worktree, using repository operations for state management, tracing, and replay. This extends prior works of repository-scale self-evolution from EDA software systems, to hardware-design artifacts themselves. We evaluate our approach on ChipBench, RTLLM, Verilog-Eval, and nine CVDP categories, achieving 100\% benchmark completion across all suites with a fully hands-free agentic loop. However, we do not claim that agentic AI for hardware design is solved: these benchmarks are controlled proxies for a much broader engineering problem in chip design. Section~\ref{sec:discuss} examines the limitations of the current study and highlights open research challenges.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：单次生成的局限性
在硬件设计领域，尤其是 RTL（寄存器传输级）设计中，单次（Single-turn）代码生成存在严重的局限性。硬件设计的正确性不仅取决于语法的正确，更取决于极其严格的周期级行为、复位和接口约定、位宽匹配以及仿真器的动态反馈。因此，生成“看起来合理”的 Verilog 代码对于实际工程而言是远远不够的。

### 现有研究的缺口
虽然先前的研究（如 AlphaEvolve、SATLUTION 和 ABCEvo）已经展示了 LLM 在仓库级代码自演化方面的潜力，但这些工作主要集中在优化 EDA 软件系统或算法内核上。换句话说，它们改变的是工程师用来设计芯片的“程序”，而不是工程师创造的“硬件设计”本身。如何将硬件设计过程本身转化为一种可管理、可演化且具备版本控制能力的仓库级演化过程，是当前领域内的一个重大技术空白。

### 任务复杂性与验证压力
硬件设计任务通常涉及复杂的依赖关系和多步骤的验证流程。一个有效的智能体必须能够：
1. 将候选工件放置在可运行的工作空间中；
2. 调用复杂的领域工具（如仿真器、Lint 工具、综合工具）；
3. 准确解释工具反馈的失败原因；
4. 根据反馈不断修正工件，直到满足显式的验收条件。这种对闭环反馈和状态管理的高要求，使得传统的生成式方法难以胜任。

Q2: 有哪些相关研究？

### 仓库级自演化系统
HORIZON 的设计灵感直接来源于一系列针对软件系统的自演化研究：
- **AlphaEvolve (2025)**：展示了 LLM 结合自动评估器可以改进算法内核。
- **SATLUTION (2025)**：将自演化理念扩展到了完整的 SAT 求解器仓库。
- **ABCEvo (2026)**：将该范式应用于 ABC 逻辑综合系统。这些工作共同确立了“在版本控制下演化软件工件”的范式，即只有在可执行证据支持的情况下才接受代码更改。

### 硬件生成与评估基准
论文引用并评估了当前主流的硬件设计基准测试，这些基准构成了 HORIZON 的实验基础：
- **ChipBench**：涵盖混合 RTL 生成任务。
- **RTLLM-2.0**：侧重于从自然语言描述生成 RTL 代码。
- **Verilog-Evalv2**：类似于 HDLBits 风格的 Verilog 生成任务。
- **CVDP (Code and Verification Generation Categories)**：这是一个全面的分类体系，涵盖了从 RTL 代码补全（CID 002）、规格说明转 RTL（CID 003）、代码修改（CID 004）、模块复用（CID 005）到 Linting/QoR 优化（CID 007），以及验证侧的激励生成（CID 012）、检查器生成（CID 013）、断言生成（CID 014）和调试修复（CID 016）。

Q3: 论文如何解决这个问题？

### 1. Markdown Harness 与项目包编译
HORIZON 引入了一种结构化的 Markdown Harness，作为人类与智能体之间的接口。Harness 定义了任务的目标、必要的领域知识、可执行的评估器脚本以及验收谓词。一个“引导智能体”（Bootstrap Agent）负责将此 Harness 编译成一个“项目包”（Project Pack），其中包含了 Git 策略和运行时环境配置。

### 2. 基于 Git 工作树的状态管理
HORIZON 将硬件设计问题转化为一个自包含的 Git 工作树。这种设计的巧妙之处在于：
- **隔离性**：每个演化尝试都在独立的工作树中进行，互不干扰。
- **可追溯性**：利用 Git 的提交记录进行状态管理、错误追踪和历史回放。
- **原子性**：只有通过验收谓词的代码更改才会被正式提交，确保了仓库始终处于“已知良好”的状态。

### 3. 全自动智能体循环（Hands-free Loop）
智能体循环是 HORIZON 的核心执行引擎，其流程如下：
- **编辑（Edit）**：智能体根据当前任务和之前的失败反馈修改代码。
- **评估（Evaluate）**：调用 EDA 工具链（如 Verilator, Vivado 或商业仿真器）运行测试套件。
- **反馈（Feedback）**：捕获工具输出的日志、错误信息或性能指标。
- **决策（Commit/Reject）**：如果满足验收谓词，则提交更改；否则，将错误信息反馈给智能体进行下一轮修正。

### 4. 领域工具集成
HORIZON 能够无缝集成开源（Open-source）和商业（Commercial）EDA 流程。对于需要商业仿真器的验证任务（如 CVDP CID 012-014），框架提供了相应的接口支持，确保了在复杂工业场景下的适用性。

Q4: 论文做了哪些实验？

### 实验设置
- **骨干模型**：所有实验均采用 GPT-5.3 作为智能体背后的决策大脑，且在实验过程中保持参数固定。
- **硬件环境**：运行在配备 AMD EPYC 9334 32 核处理器和 512 GB RAM 的服务器上。
- **软件环境**：集成了多种 EDA 工具，包括用于基础验证的开源工具和用于高级验证任务的商业仿真器。

### 评估基准与任务分类
实验覆盖了极其广泛的硬件设计维度：
1. **RTL 生成**：ChipBench, RTLLM-2.0, Verilog-Evalv2。
2. **CVDP 专项任务**：
 - **代码层面**：补全、规格转 RTL、修改、模块复用。
 - **质量层面**：Linting 和 QoR（结果质量）改进。
 - **验证层面**：激励（Stimulus）、检查器（Checker）和断言（Assertion）生成。
 - **维护层面**：调试与 Bug 修复。

### 评价指标
主要评价指标是 **Pass Rate（通过率）**，即智能体在无需人工干预的情况下完成任务的比例。此外，论文还记录了达到成功所需的**迭代次数（Iterations）**以及 **Token 消耗情况**。

Q5: 发现了什么实验现象？

### 1. 100% 的基准测试完成率
HORIZON 在所有评估的基准测试套件中均达到了 100% 的完成率。这是首个在无需人工干预的情况下，端到端完成所有这些 RTL 基准测试的智能体系统。

### 2. 迭代次数的显著差异
不同任务的难度通过迭代次数得到了直观体现：
- **简单任务**：如 RTLLM-2.0 和 Verilog-Evalv2，平均仅需 2 次迭代即可成功。
- **极难任务**：CVDP CID 002（RTL 代码补全）平均需要 **82 次迭代**，CID 004（代码修改）需要 36 次迭代。这表明对于复杂逻辑，智能体需要大量的尝试和错误反馈来对齐设计意图。
- **零起步任务**：在 CVDP CID 007（Linting/QoR 改进）中，初始迭代（Iter 0b）的成功率为 0%，但 HORIZON 最终通过 24 次迭代实现了 100% 修复，证明了闭环反馈的不可替代性。

### 3. Token 消耗与缓存效率
- **集中性**：Token 消耗高度集中在少数几个困难类别中。
- **缓存优势**：研究发现，约 **91% 的 Token 是缓存的输入 Token**。这意味着在长上下文的多轮对话中，利用 LLM 的缓存机制可以极大地降低推理成本。作者指出，未来的优化重点应放在 Token 效率而非单纯的正确率上，因为正确率在现有基准上已趋于饱和。

### 4. 失败模式分析
虽然最终达到了 100%，但过程中存在非通过的情况（如 ChipBench 中的一个任务），通常是由于环境配置或工具链限制而非智能体逻辑错误。这提示了环境鲁棒性在智能体设计中的重要性。

Q6: 有什么可以进一步探索的点？

### 1. 提升 Token 效率
鉴于 91% 的 Token 消耗来自缓存，如何更智能地管理上下文、减少冗余信息的输入，是降低智能体运行成本的关键方向。

### 2. 迈向更真实的工程场景
当前的基准测试虽然全面，但仍是真实芯片设计工程的“受控代理”。未来的研究需要挑战更复杂的问题，例如：
- **大规模 SoC 级集成**：处理具有数千个模块和复杂总线协议的系统。
- **物理设计交互**：将时序、面积和功耗（PPA）反馈引入演化循环。
- **微架构探索**：让智能体自主权衡不同的流水线深度或缓存策略。

### 3. 验证规划与形式化验证
目前的验证主要依赖仿真激励。未来可以探索如何让智能体自主制定验证计划，并结合形式化验证工具（Formal Verification）来提供更强的正确性保证。

### 4. 跨领域设计空间验证
将 HORIZON 的框架目标从单纯的 RTL 实例化扩展到更广泛的设计空间，验证其在不同硬件描述语言（如 Chisel, Bluespec）或异构计算架构设计中的通用性。

Q7: 总结一下论文的主要内容

本文介绍了 HORIZON，一个创新的自演化智能体框架，它重新定义了 AI 辅助硬件设计的方法论——将其视为仓库级的代码演化过程。HORIZON 的核心逻辑在于：硬件设计（尤其是 RTL）对正确性的要求极高，单次生成无法满足需求，必须通过“工具反馈-自主修正”的闭环来实现。该框架通过 Markdown Harness 定义任务，并将其编译为包含领域知识和验收谓词的项目包。智能体在隔离的 Git 工作树中运行，利用 Git 的版本控制能力进行状态管理和回溯。实验结果令人瞩目：在包括 ChipBench、RTLLM、Verilog-Eval 和 CVDP 在内的所有主流 RTL 基准测试中，HORIZON 均实现了 100% 的自动完成率，且完全无需人工干预。论文深入分析了实验中的迭代效率和 Token 消耗模式，指出虽然在现有基准上正确率已接近饱和，但如何提高 Token 效率以及如何应对更复杂的真实工程挑战（如微架构探索和物理设计约束）仍是未来的开放课题。HORIZON 不仅展示了 LLM 在硬件设计领域的强大潜力，也为构建工业级的自动芯片设计流水线提供了一个系统性的参考架构。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该论文与智能体（Agent）和生成式 AI（Generation）方向高度契合，展示了智能体在垂直工业领域的深度应用。

## 基本信息

- 作者：Cunxi Yu, Chenhui Deng, Nathaniel Pinckney, Brucek Khailany
- 机构：根据作者背景（Brucek Khailany, Nathaniel Pinckney 等），推断该研究主要由 NVIDIA（英伟达）研究团队主导。
- 来源：arxiv
- 主题/分类：cs.AR, cs.AI
- 日期：2026-06-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.28279`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于 HORIZON 框架的组成部分（Harness, Project Pack, Git Worktree）、实验结果（100% 完成率）以及关于 Token 缓存效率的发现。
