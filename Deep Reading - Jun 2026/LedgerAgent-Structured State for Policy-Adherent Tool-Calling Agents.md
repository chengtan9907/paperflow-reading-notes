---
user_id: "cheng tan"
paper_id: 781
arxiv_id: "2606.20529v1"
title: "LedgerAgent: Structured State for Policy-Adherent Tool-Calling Agents"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.20529v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.20529v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-20T01:11:45"
---
# LedgerAgent: Structured State for Policy-Adherent Tool-Calling Agents

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：tool-calling agents · policy adherence · state management · inference-time method

## 一句话总结

提出LedgerAgent，一种推理时方法，通过单独的分账本维护任务状态并在提示中渲染，从而增强工具调用智能体的策略依从性。

## 摘要

> Policy-adherent tool-calling agents in customer-service domains must maintain task states across turns while calling tools and obeying domain policies. Task states consist of relevant facts, identifiers, constraints, and conditions observed through user interaction and tool calls. In standard agents, task states are not represented separately. Observations, tool returns, and policy instructions are placed in the prompt, leaving agents to reconstruct the relevant states from the prompt each time they decide what to do next. This design makes state management implicit, creating two common failure modes. An agent may retrieve the right facts but later ground its decision in stale, missing, or incorrect information; and a syntactically valid tool call may still violate a domain policy that depends on the current task state. We introduce \textsc{LedgerAgent}, an inference-time method for tool-calling agents that maintains observed task states in a separate ledger and renders the states into the prompt. The ledger is also used to check state-dependent policy constraints before environment-changing tool calls are executed, blocking policy violations. Across four customer-service domains and a mixed panel of open- and closed-weight models, \textsc{LedgerAgent} improves average pass\textasciicircum{}k over a standard prompt-based tool-calling approach, with the largest gains under stricter multi-trial consistency metrics.

Q1: 这篇论文试图解决什么问题？

这篇论文解决的核心问题是：在需要遵守领域策略的客服场景中，标准工具调用智能体（基于提示的LLM）的状态管理是隐式的。任务状态（如用户信息、订单状态、约束条件）散布在提示历史中，每次决策时智能体需要从提示中重建当前状态。这导致两个关键失败模式：1) 状态接地错误（state grounding）：智能体可能检索到正确的记录，但实际动作基于过时、缺失或错误重建的信息；2) 策略违规（policy violation）：即使工具调用语法正确，如果该调用违反依赖于当前状态的领域策略（例如，在订单已支付的情况下尝试退款），也无法被显式阻止。论文明确指出现有方法（如ReAct、反思等推理时支架）虽然改进了规划或反思，但大多没有将状态表示与策略检查分离，导致策略执行脆弱。

Q2: 有哪些相关研究？

相关工作涵盖三类：1) 工具调用智能体（tool-calling agents）：如ReAct（Yao et al., 2023a）、Toolformer（Schick et al., 2024）等，但它们的状态管理是隐式的，只依赖提示历史。2) 推理时支架（inference-time scaffolds）：如规划（Plan-and-Solve）、反思（Reflexion）、工作流约束（Yao et al., 2023b; Madaan et al., 2023; Shinn et al., 2023），这些方法可提升性能，但大多未显式分离状态表示。3) 策略执行（policy enforcement）：现有方法将策略规则置于提示中或依赖模型推理动作是否允许（Yao et al., 2024; Barres et al., 2025），但当规则适用性依赖于当前任务状态时容易失败。LedgerAgent与上述方法正交，通过添加确定性组件（分账本和策略门）来显式处理状态和策略检查。

Q3: 论文如何解决这个问题？

LedgerAgent是一种推理时方法，在标准工具调用智能体基础上增加两个确定性组件：1) 分账本（Ledger）：一个结构化存储，用于记录从成功工具返回中观测到的任务状态（如用户ID、订单号、退款金额限制等）。该分账本以预定义领域模式（domain schema）组织，支持字段更新。每次智能体决定调用工具前，分账本中的当前状态被渲染到提示中，使模型能访问最新、具象的状态信息。2) 策略门（Policy Gate）：一个确定性的规则引擎，在调用会改变环境状态的工具（如更新订单、退款等）之前，检查提议的工具调用是否满足领域策略。策略被表示为关于分账本字段的谓词（例如，“仅当订单状态为‘已支付’时才能调用refund_tool”）。如果违规，则阻止该调用并返回错误信息，让智能体重新规划或纠正。该方法不修改模型参数，仅改变提示内容和执行流程。

Q4: 论文做了哪些实验？

论文在四个客服领域（推测包括订单管理、用户支持等，具体领域名称未在检索片段中完全给出）进行了实验。使用了多种开源和闭源模型（包括GPT-4、Claude、Llama等混合面板）。评估指标为pass^k（其中k可能表示尝试次数或轨迹数）和更严格的多轮一致性指标（multi-trial consistency metrics），后者衡量在多次运行中是否保持一致的策略遵守。与标准基于提示的工具调用方法（可能为ReAct或简单提示基线）进行对比。实验设计可能包括对每个领域模拟多轮交互，记录工具调用正确性、策略违规次数、最终任务成功率等。论文报告了平均pass^k提升，且随着一致性指标严格化，优势更加明显。具体数值、基线细节、消融实验等未在检索证据中明确，需查阅原文。

Q5: 发现了什么实验现象？

检索证据未提供具体实验现象，但根据摘要和结论可推断：1) 主要观察：LedgerAgent在所有四个客服领域的平均pass^k均优于标准基于提示的方法。2) 趋势：在多轮一致性指标（即要求多次独立轨迹均成功或一致遵守策略）下，改进幅度更大，说明显式状态管理对稳定策略遵循至关重要。3) 未见负面结果或反直觉发现，但推测在状态变化复杂的长期场景中，分账本维护可能增加提示长度和计算开销。4) 未提及失败案例，但论文局限性指出该方法假设工具返回能映射到稳定领域模式，这限制了在非结构化或无工具返回的应用场景。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：1) 松弛对结构化领域模式的依赖，支持半结构化或自然语言状态提取。2) 将分账本与学习型组件结合，例如自动推断分账本字段或策略谓词。3) 扩展到多智能体协作场景，其中多个智能体共享或更新同一分账本。4) 探索动态策略更新，允许策略根据环境或用户反馈自适应演化。5) 对更复杂的策略（如时序约束）进行形式化验证。6) 将LedgerAgent迁移到机器人或具身智能领域，其中状态可能来自传感器而非工具返回。7) 减少提示长度增加带来的推理成本，例如通过蒸馏或缓存。

Q7: 总结一下论文的主要内容

论文针对客服领域策略依从工具调用智能体中的状态隐式管理问题，提出LedgerAgent。其核心思想是将任务状态显式存储于结构化分账本中，每次决策前渲染到提示，并通过策略门在执行环境改变操作前检查策略合规性。该方法不修改模型参数，属于推理时增强。在四个客服领域、多种开源/闭源模型上的实验显示，相比标准基于提示的方法，LedgerAgent在pass^k和更严格的多轮一致性指标上均有提升。论文的主要贡献在于指出了状态接地失败和策略违规两个关键问题，并提供了简单、确定性、与模型无关的解决方案。局限性包括：依赖结构化工具返回、需要预定义领域模式，以及增加提示长度带来的计算开销。这项工作与现有推理时支架方法正交，可结合使用。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与用户关注方向中的“agent”直接相关（权重0.10），提供了一种提升策略遵从性的实际方法。

## 基本信息

- 作者：Md Nayem Uddin, Amir Saeidi, Eduardo Blanco, Chitta Baral
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-06-18
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.20529v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（Abstract、Introduction、Method、Conclusion等片段），并结合heuristic_draft进行润色和补充，对缺失信息（如具体实验数值）明确标记为需查阅原文。
