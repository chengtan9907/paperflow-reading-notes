---
user_id: "cheng tan"
paper_id: 9581
arxiv_id: "2608.23552v1"
title: "Prime Agent: A Self-Improving RLM Harness"
institution: "Prime Intellect（依据 GitHub 仓库 PrimeIntellect-ai 推断；论文作者列表包含 Elie Bakouch、Johannes Hagemann 等与 Prime Intellect 相关的成员）。"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23552v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23552v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:24:03"
---
# Prime Agent: A Self-Improving RLM Harness

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent harness · recursive language model · test-time compute · persistent REPL

## 一句话总结

Prime Agent 提出一种以持久 IPython REPL 与 Recursive Language Model（RLM）抽象为核心的开源 agent harness，把执行、恢复、验证与资源核算标准化，从而将 ARC-AGI-3 RHAE Best@1 从 30% 提升至 95.5%，并在长上下文编码、GPU-kernel 生成、模拟器构建、nanoGPT speedrun 与 Factorio 持续学习等长时程任务上匹配或超越原生及主流 harness。

## 摘要

> Language models are sequential processors, but long-horizon agency requires external information and computation beyond model weights and active context. Prime Agent is an open-source harness for long-horizon evaluation and coding-agent workflows. A persistent IPython REPL follows the Recursive Language Model abstraction for programmatic context processing and test-time compute, while Continual Harness preserves histories, memories, skills, prompts, and subagent specifications across trajectories. Recursive subagents coordinate through direct agent-to-agent communication, and the Agents View lets humans inspect and manage daemon-backed sessions. Prime Agent standardizes execution, recovery, verification, and resource accounting while leaving strategy construction to the model. This low-friction, expressive membrane prevents harness failures from becoming model failures and pushes measurement toward the model's true maximal underlying capability. Prime Agent raises ARC-AGI-3 RHAE Best@1 from 30% to 95.5% and matches or exceeds native and popular harnesses across long-context coding, GPU-kernel generation, emulator construction, and autonomous nanoGPT speedruns. On Factorio, we find refinement allows for continuous technology progression and dedicated subagents enable parallelized work. Code is available at https://github.com/PrimeIntellect-ai/prime-agent.

Q1: 这篇论文试图解决什么问题？

论文解决的核心问题是：语言模型是顺序处理器，而长时程（long-horizon）智能体任务要求模型在远超单次上下文窗口的时间尺度上持续获取外部信息、执行计算并维持状态，这超出了模型权重与活动上下文所能承载的范畴。具体而言存在以下几层问题：
1. **状态持久性缺失**：现有 agent 框架通常在单次轨迹内有效，但跨轨迹的历史、记忆、技能与提示词难以保存和复用，导致模型无法在长期任务中持续积累经验。
2. **计算与推理的耦合**：模型推理、Python 执行与工具调用在测试时混在一起，缺少清晰的资源核算与程序化上下文处理机制，难以评估真正的模型能力。
3. **子代理协调成本高**：现有 multi-agent 系统多依赖中心化调度或自然语言消息传递，缺少直接的 agent-to-agent 通信与递归子代理机制，并行化与分工受限。
4. **Harness 失败与模型失败混淆**：评测中基础设施的故障（如执行环境崩溃、恢复失败、验证不一致）常被误认为是模型能力不足，导致测量偏差。
5. **评测基础设施不统一**：不同 harness 对执行、恢复、验证和资源计账的协议各异，难以进行公平对比，也阻碍了评测结果的可重复性。
论文主张通过一个“低摩擦、高表达力”的 harness 膜来隔离基础设施问题，让策略构造完全由模型负责，从而推动评测向模型真实的最大底层能力靠拢。

Q2: 有哪些相关研究？

根据摘要与检索证据，论文的相关研究主要围绕以下几条线索：
1. **Agent harness 与评测框架**：包括原生（native）harness 与社区流行的开源 harness。论文在长上下文编码、GPU-kernel 生成、模拟器构建与 nanoGPT speedrun 等任务上与它们进行对比，表明 Prime Agent 匹配或超越这些基线。合理推断这些基线包括 OpenAI 系 agent 评测框架、Aider、OpenHands 等 coding-agent 工具，以及 SWE-bench 类长时程评测环境。
2. **测试时计算（test-time compute）扩展**：论文将测试时计算定义为模型推理、Python 执行与工具调用的组合，并分别报告 tokens、时间与成本。这与 o1 类推理模型、Best-of-N 采样、Tree-of-Thoughts 等测试时扩展工作相关，但 Prime Agent 更强调程序化计算（REPL）而非纯推理链扩展。
3. **Recursive Language Model（RLM）抽象**：RLM 是近期出现的 agent 设计范式，强调模型可以递归地调用自身（或子代理）来分解任务并处理上下文。Prime Agent 将 RLM 抽象落地为持久 IPython REPL 与递归子代理机制。
4. **持续学习与记忆系统**：Continual Harness 保存历史、记忆、技能、提示词与子代理规格，这与 MemGPT、Voyager 等具身/文本智能体的记忆与技能库工作相关，但 Prime Agent 更强调跨轨迹的通用 harness 层。
5. **人类-Agent 交互界面**：Agents View 提供可视化界面，允许人类检查和管理 daemon 支撑的持久会话，这与 OpenHands、LangGraph Studio 等可视化 agent 调试工具形成对照。
注意到检索证据中 references 标记 [1]、[32]、[44] 存在，但具体文献内容未在证据中展开，因此上述相关工作的具体映射属于合理推断而非论文明文。

Q3: 论文如何解决这个问题？

论文提出 Prime Agent，一个开源的长时程 agent harness，其设计围绕以下几个关键技术决策：
1. **持久 IPython REPL 作为程序化上下文处理核心**：每个会话拥有一个持久 IPython Read-Eval-Print Loop。测试时计算由三部分组成——模型推理、Python 执行、工具调用；评估时分别报告 tokens、时间与成本。已安装的工具以模块形式导入 REPL，使模型可以程序化地扩展自身能力。REPL 的持久性意味着中间变量、函数定义与计算结果可以在多次推理调用之间保留，从而把模型的活动上下文扩展到程序状态中。
2. **Recursive Language Model（RLM）抽象**：REPL 遵循 RLM 抽象，即模型可以把上下文处理与计算委托给程序化环境，递归地处理长上下文与复杂任务。这与传统 agent 将所有信息塞入上下文窗口的做法形成对比。
3. **Continual Harness 跨轨迹状态保全**：在轨迹之间保存历史、记忆、技能、提示词与子代理规格。这意味着模型可以在多次会话中复用学到的技能与策略，实现自我改进（self-improving）。
4. **递归子代理与直接 agent-to-agent 通信**：子代理可以递归地创建，并通过直接通信协调工作。检索证据显示计算管理会将测试时计算分配给程序、工具调用、可复用技能与并行递归子代理。这种设计支持任务并行分解，例如 Factorio 中多个子代理同时负责不同的生产链。
5. **Agents View 可视化控制台**：提供可视化界面，让人类检查、附加（attach）与管理由 daemon 支撑的持久会话。这解决了长时程任务的监督与干预问题。
6. **标准化执行、恢复、验证与资源核算**：harness 提供统一的执行协议、故障恢复机制、输出验证流程与资源计账方案，但刻意不干预策略构造——策略完全由模型决定。作者把这称为“低摩擦、高表达力的膜”，其目的是让 harness 失败（如执行环境崩溃）不会变成模型失败，从而更准确地测量模型真实能力。

Q4: 论文做了哪些实验？

根据摘要与检索证据，论文的实验覆盖了五个任务族：
1. **交互式推理（Interactive Reasoning）**：以 ARC-AGI-3 为测试平台。ARC-AGI-3 需要模型在动作限制下学习每个游戏的规则，建立临时的世界模型（ad-hoc world model）。Prime Agent 将 RHAE Best@1 从 30% 提升至 95.5%。具体实验协议（如动作限制数量、评测回合数、使用的底层模型）在检索证据中未展开。
2. **长上下文编码（Long-context Coding）**：与原生及流行 harness 对比，Prime Agent 匹配或超过基线。具体基准（如 SWE-bench、RepoBench 或内部长上下文任务）未在证据中给出。
3. **GPU-kernel 生成**：评测模型编写 GPU kernel 的能力，Prime Agent 表现匹配或超过基线。
4. **模拟器构建（Emulator Construction）**：要求模型从零构建模拟器，Prime Agent 表现匹配或超过基线。
5. **自主 nanoGPT speedrun**：在给定时间/资源约束下自主完成 nanoGPT 的训练或推理任务，Prime Agent 匹配或超过原生与流行 harness。
6. **持久环境（Persistent Environments）**：以 Factorio 游戏为测试环境，发现 refinement 机制支持持续技术进展（continuous technology progression），专用子代理实现并行化工作。
实验设计与基线细节、消融实验、资源消耗表等在检索证据中未出现，需要回原文确认。

Q5: 发现了什么实验现象？

检索证据支持的实验观察包括：
1. **ARC-AGI-3 的显著提升**：RHAE Best@1 从 30% 提升到 95.5%，提升幅度超过 3 倍。这是一个强结果，表明持久 REPL 与递归子代理带来的程序化计算能力对交互式推理任务有决定性帮助。合理推断：REPL 允许模型在推理之外执行代码来验证模式假设，从而大幅减少纯文本推理的错误。
2. **跨任务匹配或超越基线**：在长上下文编码、GPU-kernel 生成、模拟器构建、nanoGPT speedrun 上，Prime Agent 匹配或超过原生与主流 harness。这说明 Prime Agent 的收益不局限于特定任务类型，但“匹配或超越”的表述也暗示在某些任务上并非全面碾压，而是与强基线持平。
3. **Factorio 中的持续技术进展**：refinement（精炼/反思）机制允许模型在持续运行中不断推进技术树，说明跨轨迹状态保存对长期目标追求有实际价值。
4. **专用子代理实现并行化**：在 Factorio 中，专用子代理可以并行工作，表明直接 agent-to-agent 通信与递归子代理机制在真实持久环境中有效。
推测性观察：论文提到“refinement allows for continuous technology progression”，可能意味着模型通过 REPL 中保存的技能和记忆来迭代改进策略，而非每次从头开始。此外，检索证据提到“Results across interactive reasoning, long-context tasks, autonomous research, systems construction, and persistent environments”，其中“autonomous research”和“systems construction”可能对应模拟器构建与 nanoGPT speedrun，但“autonomous research”具体任务未在摘要中出现。指标间的张力（如 Best@1 与成本、tokens 消耗的关系）在证据中未提及。

Q6: 有什么可以进一步探索的点？

基于论文主张与检索证据，可以勾勒以下可进一步探索的方向：
1. **RLM 抽象的规模化**：论文将 RLM 抽象落地为 REPL 与递归子代理，但 RLMs 的递归深度、分支因子与上下文共享机制仍可进一步优化，例如探索更高效的子代理通信协议。
2. **自我改进的自动化闭环**：Continual Harness 保存技能与记忆，但技能如何被自动验证、去重、更新与淘汰？如何避免记忆污染与技能退化？
3. **评测协议的标准化**：Prime Agent 标准化了执行、恢复、验证与资源核算，可进一步推动社区采纳统一的长时程评测协议，尤其是 tokens/时间/成本的三维报告标准。
4. **ARC-AGI-3 之外的推理基准**：95.5% Best@1 的结果在 ARC-AGI-3 上取得，进一步可验证其在 ARC-AGI-2、ARC-AGI-1 私有集及其他交互式推理基准上的泛化。
5. **持久环境中的长期学习**：Factorio 实验展示了持续技术进展，可扩展至更复杂的长期任务（如 Minecraft、软件维护、科研智能体），研究子代理分工的长期稳定性。
6. **人类-Agent 协作界面**：Agents View 提供可视化管理，但如何设计更高效的 human-in-the-loop 干预机制（如中断、重定向、技能注入）仍有很大空间。
7. **成本-性能权衡**：论文报告 tokens、时间与成本，可进一步分析 REPL 持久状态 vs 上下文扩展在不同任务中的成本效益，以及何时应该用工具调用而非模型推理。
8. **Harness 失败与模型失败的解耦度量**：论文的核心主张之一是“防止 harness 失败变成模型失败”，可以设计专门的诊断协议来量化两类失败的比例，验证这一主张。

Q7: 总结一下论文的主要内容

《Prime Agent: A Self-Improving RLM Harness》由 Prime Intellect 团队（含 Seth Karten、Alex L. Zhang、Kevin Thomas、Sebastian Müller、Elie Bakouch 等）撰写，于 2026 年 8 月提交至 arXiv。论文从“语言模型是顺序处理器，但长时程智能体需要外部信息与计算”这一观察出发，提出一个开源 harness 来弥合模型权重/上下文与长期任务需求之间的鸿沟。

论证主线：论文认为，长时程智能体的能力瓶颈往往不在模型本身，而在 harness——即模型与外部环境之间的接口层。大多数现有 harness 将模型的活动上下文视为计算的主要载体，导致长上下文任务成本高、状态易丢失、恢复困难，且基础设施故障常被误判为模型能力不足。Prime Agent 的核心理念是建立一个“低摩擦、高表达力的膜”，把执行、恢复、验证与资源核算标准化，而把策略构造完全留给模型。

技术主线：Prime Agent 由五个相互咬合的组件构成。第一，持久 IPython REPL 遵循 Recursive Language Model（RLM）抽象，使模型可以通过程序化方式处理上下文与计算；测试时计算分为模型推理、Python 执行与工具调用三类，并分别核算 tokens、时间与成本。第二，Continual Harness 在轨迹之间保存历史、记忆、技能、提示词与子代理规格，使模型具备跨会话的持续学习能力，这也是“Self-Improving”一词的载体。第三，递归子代理机制允许模型创建子代理并通过直接 agent-to-agent 通信协调工作，计算管理可将测试时计算分配给程序、工具调用、可复用技能与并行递归子代理。第四，Agents View 提供可视化界面，人类可以检查、附加与管理 daemon 支撑的持久会话。第五，标准化的执行、恢复、验证与资源核算协议确保基础设施层的一致性。

实验主线：论文在五个任务族上验证了 Prime Agent。交互式推理方面，在 ARC-AGI-3 RHAE 上 Best@1 从 30% 提升至 95.5%，是最突出的结果。长上下文编码、GPU-kernel 生成、模拟器构建与自主 nanoGPT speedrun 上，Prime Agent 匹配或超越原生与主流 harness。在持久环境 Factorio 中，refinement 机制支持技术的持续进展，专用子代理实现并行化工作。结论部分总结道：持久执行、递归会话、自主控制、记录历史与 Continual Harness 共同构成一个支持长时程工作的统一基底，并展示了该基底在不同任务形式上的通用性。

论文的局限与信息缺口：检索证据未包含实验基线名称、具体模型配置、消融实验、资源消耗表与失败案例分析，因此无法在摘要级别验证“95.5%”结果的可重复性细节；Factorio 实验的量化指标（如科技树推进速度、子代理数量）也未在证据中给出。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文与用户的“agent”方向直接相关，提供了一个具体的、开源的 harness 设计实例，可作为长时程智能体系统的参考实现。

## 基本信息

- 作者：Seth Karten, Alex L. Zhang, Kevin Thomas, Sebastian Müller, Elie Bakouch, Daniel Auras, Mika Senghaas, Fares Obeid, Konstantin Dunas, Johannes Hagemann, Sami Jaghouar
- 机构：Prime Intellect（依据 GitHub 仓库 PrimeIntellect-ai 推断；论文作者列表包含 Elie Bakouch、Johannes Hagemann 等与 Prime Intellect 相关的成员）。
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.SE
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23552v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索证据（摘要、引言、架构、结论片段），并结合 heuristic_draft 进行补全；对于检索证据未覆盖的实验细节与基线信息，已在对应字段中明确标注为推断或信息缺口。
