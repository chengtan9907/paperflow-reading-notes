---
user_id: "cheng tan"
paper_id: 5715
arxiv_id: "2607.25853v1"
title: "HiSkill: Empowering LLM Agents with Hierarchical Skill Graphs"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25853v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25853v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:51:10"
---
# HiSkill: Empowering LLM Agents with Hierarchical Skill Graphs

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：hierarchical skill graph · llm agent · skill reuse · graph-guided execution

## 一句话总结

提出 HiSkill，一种层次化技能图框架，通过构建包含技能节点和 AtomicOp 节点的有向图来组织 LLM 智能体的交互轨迹，并在推理时检索任务相关子图以指导技能切换与动作执行。

## 摘要

> Skills have become an important abstraction for enabling large language model (LLM) agents to reuse past experience in long-horizon interactive tasks. However, existing trajectory-to-skill methods often produce flat collections of high-level textual skills that are stored and retrieved independently, leaving skill relations underutilized and maintaining a gap between high-level skills and executable actions. In this paper, we propose HiSkill, a hierarchical skill graph framework that organizes interaction trajectories into a directed graph with skill nodes, AtomicOp nodes, and typed edges. Specifically, the graph connects reusable high-level skills with executable action templates, while also capturing decomposition, temporal transition, compatibility, support, and recovery relations among them. At inference time, HiSkill retrieves a compact task-relevant subgraph and performs subgraph-guided task execution, where a symbolic task state, an active skill, and the retrieved subgraph guide the LLM agent to switch skills, select AtomicOps, and ground executable actions iteratively. Experiments on three interactive environments show that HiSkill outperforms state-of-the-art baselines while reducing inference token consumption, demonstrating the effectiveness of bridging high-level skills and executable action grounding through a hierarchical skill graph. Our data and code is available at https://github.com/BUPT-GAMMA/HiSkill.

Q1: 这篇论文试图解决什么问题？

现有 LLM 智能体技能重用方法存在两个核心问题：1）生成的技能库为扁平集合，技能间关系（如分解、时序、兼容等）未被利用，导致检索低效且缺乏上下文指导；2）高层技能与可执行动作之间存在执行鸿沟——技能描述是文本级抽象，需进一步转换为具体动作序列，现有方法缺乏桥接机制。HiSkill 旨在通过层次化技能图同时建模技能关系并提供原子操作节点，弥合抽象到执行的差距。

Q2: 有哪些相关研究？

相关研究包括技能库方法（Vector Skills、SkillNet、GoS）和技能图方法（SkillX、Xia et al. 2026b）。Vector Skills 等采用扁平语义检索，未建模关系；SkillNet 进行技能库选择但忽略关系；GoS 在技能级补全依赖但未下探到动作级。Xia 等构建技能级图但关系类型单一。HiSkill 的创新在于构建 AtomicOp 节点和五种类型边（分解、时序、兼容、支持、恢复），实现从高层技能到可执行动作的多层次绑定。此外，论文对比了 ReAct、SayCan 等隐式技能方法。

Q3: 论文如何解决这个问题？

HiSkill 由三个组件构成：1）层次化技能图构建：从历史交互轨迹中提取技能节点（基于相似度聚类）和 AtomicOp 节点（可执行动作模板），并标注五种边——分解（技能→子技能/AtomicOp）、时序（AtomicOp 间顺序）、兼容（技能/AtomicOp 可组合）、支持（技能前提）、恢复（失败恢复）。2）任务相关子图检索：根据任务描述进行语义匹配和词汇匹配，从全局图中检索包含相关技能和 AtomicOp 的紧凑子图。3）子图引导任务执行：维护符号任务状态，初始化激活技能，迭代执行：由 LLM 根据当前状态、激活技能和子图选择切换技能或执行 AtomicOp，并通过 LLM 生成具体动作。该流程将高层规划与底层执行紧密结合。

Q4: 论文做了哪些实验？

论文在三个交互式任务环境（具体名称未在检索证据中详细列出）上评估 HiSkill，对比 ReAct、SayCan、Vector Skills、SkillNet、GoS 等基线。实验设置包括：1）多轮成功率（SR）；2）推理 token 消耗（效率）；3）消融研究（移除关系边、跳过子图检索等）。论文还报告了不同类型边对性能的贡献，以及子图检索的压缩比。

Q5: 发现了什么实验现象？

根据摘要，HiSkill 在三个环境上均优于所有基线，在成功率上提升显著，同时降低推理 token 消耗（表明子图检索有效过滤了无关技能）。消融实验显示：1）移除关系边（特别是时序和分解边）导致性能下降，验证了关系的重要性；2）子图检索比全图检索更高效且性能相近。具体数值和失败模式未在检索证据中体现，需回原文确认。

Q6: 有什么可以进一步探索的点？

1）将 HiSkill 扩展到更复杂的开放域任务（如机器人操控、软件工程），验证图的可迁移性。2）自动发现和更新关系类型（而非预设五种），支持动态图演化。3）结合在线学习，从执行反馈中增量修正图结构。4）降低图构建成本（目前依赖历史轨迹聚类和手工标注？）。5）探索跨任务技能学习，减少对新任务轨迹的依赖。——证据中未提及，基于推测。

Q7: 总结一下论文的主要内容

论文针对 LLM 智能体在长程交互任务中技能重用效率低、高层技能与可执行动作脱节的问题，提出 HiSkill——一种层次化技能图框架。HiSkill 的核心是将历史交互轨迹组织为有向图，包含两层节点：高层技能节点（文本级抽象，如“获取木材”）和 AtomicOp 节点（可执行动作模板，如“打开箱子”），并定义五种类型边（分解、时序、兼容、支持、恢复）以丰富关系建模。图构建后，推理时根据任务描述检索相关子图，然后由 LLM 智能体在子图引导下迭代执行：当前符号状态和激活技能决定下一步（切换技能或执行 AtomicOp），LLM 将 AtomicOp 具体化为环境可执行动作。该流程使得技能选择与动作执行基于结构化的上下文，而非扁平检索。实验在三个交互环境上进行，与 ReAct、SayCan、Vector Skills、SkillNet、GoS 等对比，HiSkill 在成功率上取得最佳，且推理 token 消耗量最低（相比不使用子图的变体）。消融研究证实了五种边和子图检索的必要性。论文的主要贡献包括：1）首次在 AtomicOp 级别构建层次技能图，桥接高层规划与低层执行；2）引入多种关系边丰富图结构；3）提出子图检索与引导执行机制，实现高效且决策透明的技能调用。局限在于图构建依赖历史数据质量，且关系类型预设可能不覆盖所有场景。总体而言，HiSkill 为 LLM 智能体技能组织与复用提供了一种结构化的新范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦于 LLM 智能体的技能组织与重用，与您关注的 agent 方向直接相关。

## 基本信息

- 作者：Yu Hao, Jinxuan Cai, Qi Zhang, Yawen Li, Zhiqiang Zhang, Chuan Shi, Cheng Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25853v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（retrieved_evidence），并依据字段证据映射对各字段进行优先填充；部分内容（如实验环境名称、具体数值）因证据缺失而保持开放，已标注推测范围。
