---
user_id: "cheng tan"
paper_id: 5991
arxiv_id: "2607.26598v1"
title: "Living-Harness Is an Interactive-Agent Evolver"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26598v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26598v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:15:51"
---
# Living-Harness Is an Interactive-Agent Evolver

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：interactive agent · LLM agent · harness evolution · procedural repair

## 一句话总结

Living-Harness 是一个自演进 agent harness 框架，通过将交互失败转化为对持久化程序状态（情景记忆和状态图）的有限更新，使 agent 能够累积过程性修复，显著提升在 τ²-Bench 和 MultiWOZ-2.4 共八个交互环境中的任务成功率和跨模型复用能力。

## 摘要

> Large language model (LLM) agents may recover from a failure within an episode or after a retry, yet the same execution failure can recur in later tasks because post-episode feedback rarely revises the persistent harness that guides future interactions. Static harnesses improve reliability through fixed tools, context, memory, and workflow structures, but remain unchanged after deployment. We propose $\textbf{Living-Harness}$, a self-evolving agent harness that converts each completed trajectory and its evaluator signals into posterior evidence for bounded harness updates. Guided by a domain-level $\textbf{Evolution-SOP}$ ($\textbf{S}$tandard $\textbf{O}$perating $\textbf{P}$rocedure), Living-Harness extracts an episode abstraction and structured update evidence, and writes two complementary forms of procedural knowledge: episodic memory that records trigger conditions, failure patterns, and recovery actions, and a state graph that records state nodes, repair edges, and transition rules. The updated harness state is retrieved to guide future interactions, while tools and base context remain frozen, allowing procedural repairs to accumulate across evolution cycles. On eight interactive environments derived from $τ^2$-Bench and MultiWOZ-2.4, Living-Harness improves average Pass@1 over the strongest interactive baseline by 10.07 and 9.91 percentage points, respectively, and supports retrieval-only reuse of the evolved harness state across model backbones.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决 LLM 智能体在交互任务中持续重复相同执行失败的问题。现有 agent 系统虽能在单次交互内通过反馈或重试从失败中恢复，但这种纠正仅停留在响应级别，不会修改持久化引导后续交互的 harness（外部组织层，包括提示、工具、上下文、记忆、工作流等）。静态 harness 在部署后固定不变，无法纳入新观察到的失败模式和恢复动作，导致同样的执行失败在类似场景中反复出现。核心空白在于缺乏一种机制，将事后（post-episode）的失败证据转化为对持久化程序过程的修复。

Q2: 有哪些相关研究？

相关研究主要包括三类：第一，LLM agent 系统设计（Wang et al. 2024b；Xi et al. 2025；Yao et al. 2024），关注 agent 与环境的交互框架；第二，任务导向对话及工具使用（Budzianowski et al. 2018），强调多轮交互中的任务完成；第三，外部 harness 设计（Meng et al. 2026；Lee et al. 2026），通过固定规则、记忆和工作流提高可靠性，但部署后不再更新。此外，agent 自我改进工作（如反思、重试）关注单次交互内的纠正，而缺乏跨 episode 的持久化程序修复。Living-Harness 填补了这之间的空白，提出利用演化过程将 post-episode 失败反馈转化为对 harness 状态的结构化更新。

Q3: 论文如何解决这个问题？

Living-Harness 采用 rollout-evaluate-update 循环实现 harness 自演进。具体步骤：1）Rollout：agent 在当前 harness 状态（包含情景记忆和状态图）引导下与环境交互，使用固定工具和基础上下文；2）Evaluate：完成 episode 后，外部评估器给出成功/失败信号；3）Update：Evolution-SOP（领域级标准操作程序）将整个交互轨迹和评估信号转换为结构化更新证据，并执行两项更新——情景记忆（记录触发条件、失败模式、恢复动作）和状态图（记录状态节点、修复边、转换规则）。更新后的 harness 状态写回存储，供后续 episode 检索使用。工具和基础上下文在整个过程中固定不变，确保更新是有限的（bounded）且过程性修复随演化周期逐步累积。

Q4: 论文做了哪些实验？

论文实验基于两个标准交互数据集 τ²-Bench 和 MultiWOZ-2.4，从中共构建了八个交互环境。评估指标为 Pass@1（单次尝试成功率）。比较基线包括静态 harness 和强交互式 agent 基线（如带反思或重试的 agent）。实验设置细节原文未完全提供（如具体 baseline 列表、模型主干、评估样本数等），但摘要报告了总体提升：在 τ²-Bench 环境中平均 Pass@1 提升 10.07 个百分点，在 MultiWOZ-2.4 环境中提升 9.91 个百分点。此外，论文还进行了跨模型骨干复用实验，显示仅通过检索演进后的 harness 状态即可在不同 LLM 上获得性能收益。消融研究可能涉及情景记忆和状态图单独贡献，但原文未提供具体结果。

Q5: 发现了什么实验现象？

基于摘要和结论片段，观察到的关键现象包括：1）Living-Harness 持续提升平均 Pass@1，且提升幅度在多个环境一致（10.07 和 9.91 pp），说明方法的跨环境泛化性较好；2）性能随演化周期增长而提升，表明失败模式被有效捕获并转化为过程性修复，反馈积累有效；3）仅通过检索演进后的 harness 状态即可跨模型骨干复用，表明学到的是独立于模型参数的过程性知识，具有迁移性；4）更新是有限的（bounded）且工具/基础上下文冻结，避免了状态空间爆炸和灾难性遗忘。消融结果未明确给出，但推测两个组件（情景记忆、状态图）均有助于最终性能。实验部分缺乏对比不同演化策略或 SOP 设计的消融，也未报告失败 case 分析。

Q6: 有什么可以进一步探索的点？

可以从以下方向进一步探索：1）自动生成或学习 Evolution-SOP，减少领域专家设计成本；2）扩展到更开放、动态的环境，不局限于任务导向对话和工具使用；3）将更新范围从 harness 状态扩展到工具集合或基础上下文，允许更大灵活性但需约束 bounded 性；4）研究更新频率和状态空间增长的控制机制，避免无限膨胀；5）在安全关键或真人交互场景中验证框架的风险和可靠性；6）结合模型参数微调，实现双重演进；7）引入更丰富的记忆结构，如层次化状态图。

Q7: 总结一下论文的主要内容

本文针对 LLM agent 在交互任务中重复出现相同执行失败的问题，提出 Living-Harness——一种自演进的 agent harness 框架。核心思想是将每次交互完成后的反馈转化为对持久化程序状态的有限更新，从而让 agent 不仅能在当前 episode 中纠正错误，还能在后续任务中避免同类失败。Living-Harness 通过三个步骤的循环实现：Rollout（agent 在当前 harness 状态下与环境交互）、Evaluate（获取评估信号）、Update（利用 Evolution-SOP 从轨迹和信号中提取结构化证据，更新情景记忆和状态图）。情景记忆记录失败触发条件、故障模式和恢复动作；状态图记录状态节点、修复边和转换规则。工具和基础上下文固定不变，保证更新是受控的。在基于 τ²-Bench 和 MultiWOZ-2.4 构建的八个交互环境上，Living-Harness 在最强交互基线基础上将平均 Pass@1 分别提升了 10.07 和 9.91 个百分点，且演进后的 harness 状态可以仅通过检索方式跨不同 LLM 骨干复用，无需重新训练或调整。实验结果表明，可靠的 agent 不仅需要更强的单步生成能力或模型侧更新，还可以通过演进组织未来交互的外部 harness 来显著提升性能。本文的贡献包括：明确界定了持久过程性修复的问题；设计了完整的 harness 演进框架；在多个环境上验证了有效性和迁移性。工作局限性未在原文中明确讨论，但框架依赖于预定义的 Evolution-SOP，工具和基础上下文冻结可能限制适应范围，且尚未在极度开放或安全关键场景中验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接聚焦 LLM Agent 的可靠性提升，与用户关注方向（agent 权重 0.10）高度重合。

## 基本信息

- 作者：Yuetian Du, Yucheng Wang, He Xu, Jiexu Xu, Shanwen Tan, Bing Zhao, Boyu Yang, Zhijie Xu, Ming Kong, Hu Wei, Jie Liu, Qiang Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.MA, cs.AI, cs.CL
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26598v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（abstract、method、conclusion、introduction 片段），并优先采用这些证据补充和校准 heuristic_draft 及 field_evidence_map 指向的内容。
