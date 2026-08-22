---
user_id: "cheng tan"
paper_id: 9078
arxiv_id: "2608.20274"
title: "Break It Down, Pass It On: Cross-Task Skill Transfer in LLM Agents"
publish_date: "2026-08-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.20274.pdf"
pdf_url: "https://arxiv.org/pdf/2608.20274"
abs_url: "https://arxiv.org/abs/2608.20274"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-21T23:56:34"
---
# Break It Down, Pass It On: Cross-Task Skill Transfer in LLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agents · skill induction · cross-task transfer · subtask decomposition

## 一句话总结

该论文系统研究了LLM智能体中技能归纳方式对跨任务迁移的影响，发现任务级技能大多损害性能而子任务级技能平均提升性能、文本技能比代码技能迁移更好，并提出无需任务执行即可计算的技能效用分数（skill utility score）作为技能记忆实用诊断。

## 摘要

> Large language model (LLM) agents can induce skills from completed tasks and reuse them later to grow more capable with experience. In practice, induced skills may transfer unreliably and can even harm the agent that retrieves them. When agent-induced skills transfer reliably across tasks remains an open question. We conduct a comprehensive and controlled study of how the way skills are induced shapes their transfer across tasks. Specifically, we compare task-level with subtask-level skill induction and text with code skill formats, the two axes along which existing methods differ. Task-level skills mostly reduce the agent's performance below its no-memory baseline while subtask-level skills raise it above on average, and text skills transfer better than code skills. To further understand our findings, we examine two complementary properties of the induced skills: specificity, which measures how closely a skill matches real tasks, and abstractness, which measures how evenly its relevance spreads across tasks. Neither property alone predicts task success, but their combined effect does, which we propose as a skill utility score. The score correlates consistently with task success when skills are transferred, and subtask-level and text skills score higher. Computing skill utility only needs the skills and task descriptions but not any task execution, so our score serves as a practical diagnostic of a skill memory before any new task runs.

Q1: 这篇论文试图解决什么问题？

1. **核心问题**：LLM智能体能否通过从已解决任务中归纳技能并在后续任务中复用而随经验增长？技能迁移在何种条件下可靠？
2. **具体难题**：现有实践表明，归纳出的技能可能迁移不可靠，甚至对检索到它的智能体产生负面作用（即‘有害技能’）。这种不稳定性的来源尚不明确。
3. **研究缺口**：不同方法在技能归纳层面（任务级 vs 子任务级）和技能编码格式（文本 vs 代码）上存在差异，但缺乏对这些因素如何影响跨任务迁移的系统性、受控研究。
4. **更深层问题**：即使发现某些归纳方式更优，也缺乏可解释的、可预先计算的指标来预测一组技能对任务是否有效。技能的具体性、抽象性等属性单独不能解释任务成功，需要一个能结合多属性的效用度量。
5. **方法论挑战**：现有评估往往在单一任务或非流式设置下进行，难以揭示技能随任务序列动态迁移的效果；本研究需要建立形式化的跨任务迁移框架来隔离技能归纳层面的因果影响。

Q2: 有哪些相关研究？

由于检索到的证据主要来自摘要、方法节和结论节，相关工作部分信息有限，以下为基于论文语境和领域常识的合理推断（标注‘合理推断’），供参考：
1. **LLM智能体与经验积累**（合理推断）：已有工作如Voyager、BabyAGI等探索LLM智能体从环境中学习技能库并复用的范式；本论文在受控基准上比较不同归纳粒度，是对这一方向的系统化。
2. **记忆增强与检索增强生成**（合理推断）：技能记忆本质上是外部记忆的一种，与RAG、memGPT等记忆机制相关；本文关注的是‘经验归纳’而非‘原始片段存储’。
3. **提示工程与自动提示优化**（合理推断）：技能以文本或代码形式呈现，涉及如何用自然语言或程序化描述封装可复用行为；类似APO、ProTeGi等自动提示搜索工作。
4. **子任务分解（task decomposition）**（合理推断）：任务级vs子任务级归纳与层次化任务规划、LLM中的Plan-and-Solve、Chain-of-Thought等分解思想联系密切，但本文聚焦于分解后的技能归纳而非推理时分解。
5. **技能可迁移性研究**（合理推断）：跨任务迁移在强化学习、元学习中有大量研究；本文从LLM智能体视角给出特定于语言/代码格式的经验规律。
6. **值得注意的是**（证据缺口）：论文没有提供具体的相关文献列表，上述工作仅作为背景推测，不能确认被引情况；如需准确文献脉络，需查阅论文完整References。

Q3: 论文如何解决这个问题？

1. **形式化跨任务技能迁移**：论文构建了一个形式化设置，LLM智能体顺序解决任务流 T_1,...,T_n，并共享一个技能记忆（sec. 3.1-3.2）。智能体在每完成一个任务或子任务后向记忆写入一个技能，在每开始新任务前读取相关技能。这样，较早任务上归纳的技能可以影响后续任务解决方式，从而实现跨任务迁移的量化测量。
2. **两个agent实例对应两种归纳层面**：论文设计了两个agent，分别对应任务级技能归纳（将整个任务作为技能总结）和子任务级技能归纳（将任务分解为子任务，并为每个子任务归纳技能）。这是核心比较轴之一。
3. **两种技能格式**：每个agent可以将技能编码为文本（自然语言指令）或代码（可执行程序/伪代码），构成第二个比较轴。
4. **技能属性刻画**：提出两个互补属性——specificity（具体性）：衡量技能与真实任务匹配的紧密程度；abstractness（抽象性）：衡量技能的相关性在各任务间的分布均匀程度。二者分别从不同角度刻画了技能泛化的本质。
5. **技能效用分数（Skill Utility Score）**：将两个属性组合为一个分数，作为技能对任务成功可能性的联合预测器。分数计算仅用技能描述和任务描述，不需要执行任何任务，因此可以在新任务运行前作为诊断指标。
6. **评估协议**：在三个标准基准上评估，覆盖多应用工具使用、办公文档工作流和数据科学pipeline，使用官方评估器对每个任务打分；并对比无记忆基线（baseline）来衡量技能迁移的净效应。

Q4: 论文做了哪些实验？

1. **基准与任务范围**：使用三个标准基准，共同覆盖多应用工具使用（multi-app tool use）、办公文档工作流（office-document workflows）和数据科学pipeline（data-science pipelines）。每个任务由官方评估器评分。
2. **模型集合**：在11个LLM上评估，具体模型列表未在检索片段中给出（证据缺口）。
3. **条件设计**：
 - 技能归纳层面：任务级（task-level）vs 子任务级（subtask-level）。
 - 技能格式：文本（text） vs 代码（code）。
 - 对照条件：无记忆基线（no-memory baseline），即不使用技能记忆的agent。
 - 组合起来形成多因素实验矩阵。
4. **技能效用分数验证**：计算每个技能记忆的specificity和abstractness，组合成skill utility score，检验该分数与各条件下实际任务成功率的相关性。
5. **实验流程**：agent在任务流上运行，按顺序解决任务，写入和读取技能；最终比较不同条件下的累积成功率或平均任务分数。
6. **额外约束**（从limitations推断）：整个实验依赖Docker沙盒环境；视觉类任务因需要沙盒文件系统而受限；每个任务以最终环境状态评分（而非过程）。

Q5: 发现了什么实验现象？

1. **任务级技能显著有害**：任务级技能的平均效果将智能体性能降至无记忆基线以下，说明将整个任务经验压缩成一个技能时，产生的技能往往是噪声化的、上下文纠缠的，迁移到新任务时实际上干扰了agent的决策。
2. **子任务级技能平均有益**：子任务级技能将性能提升到无记忆基线之上，表明细粒度的技能归纳能捕捉更可复用的原子操作，相比之下更容易跨任务泛化。
3. **文本技能优于代码技能**：在相同归纳层面下，文本格式的技能迁移效果更好。可能原因：代码技能过于具体化（绑定特定API、变量名或执行环境），而自然语言描述保留了任务意图的抽象性，更容易被LLM在上下文中灵活解释（合理推断，论文未给出机制解释）。
4. **单一属性不预测成功**：specificity或abstractness单独均不能预测任务成功；这解释了为什么直觉上‘更具体’或‘更抽象’都不总是好——二者存在权衡，只有平衡得当才有利于迁移。
5. **组合属性的一致相关性**：技能效用分数（specificity与abstractness的结合）与实际任务成功率在技能迁移场景中持续稳定相关，说明这是一个可靠的联合诊断量。
6. **分数与归纳方式的对应**：子任务级技能和文本技能的skill utility score更高，与它们实际提升性能的经验结果一致，从而为‘哪些技能更易迁移’提供了先验度量。
7. **基线比较的意义**：实验揭示了‘有记忆未必优于无记忆’的反直觉现象——不当的归纳方式会把记忆变成负资产。

Q6: 有什么可以进一步探索的点？

1. **扩展任务域**：当前结论基于三个基准（多应用工具、办公文档、数据科学），可扩展到更丰富的领域如网页导航、代码仓库维护、机器人控制等，验证技能效用分数的通用性。
2. **动态与演化的技能记忆**：论文限制中提到研究的是静态或固定结构的技能记忆，未来可探索技能随任务执行不断更新、合并、遗忘的演化式记忆，以及对应的效用诊断方法。
3. **视觉与多模态技能**：视觉任务需要沙盒文件系统而难以在当前Docker约束中研究，未来可设计支持视觉反馈的沙盒环境，将技能归纳推广到多模态agent。
4. **更细粒度的技能设计**：除任务级/子任务级之外，可考虑层次化多粒度技能，或让agent自动选择归纳粒度（自适应分解），并结合效用分数作为指导信号。
5. **技能检索与排序优化**：当前实验关注技能写入质量，但读取和检索策略同样重要；可利用skill utility score在不同候选技能间做优先级排序，或动态过滤低效用技能。
6. **理论化具体性与抽象性**：论文给出操作性定义，但缺乏理论解释为何二者组合能预测成功；未来可尝试建立信息论或决策论框架来形式化理解。
7. **跨模型泛化机制**：11个模型上的发现是否适用于更大的模型或专门推理模型？模型规模、指令遵循能力如何调节技能归纳效果，值得探索。
8. **主动技能获取**：将技能效用分数嵌入agent决策，使其在遇到新任务时主动选择‘可学习’的任务部分来生成高效用技能，形成元学习闭环。

Q7: 总结一下论文的主要内容

论文《Break It Down, Pass It On: Cross-Task Skill Transfer in LLM Agents》针对LLM智能体经验复用中的重要问题——技能迁移何时可靠——开展了系统受控研究。研究主线沿两个现有方法差异展开：技能归纳层面（任务级 vs 子任务级）和技能表示格式（文本 vs 代码）。作者在三个代表性基准（多应用工具使用、办公文档工作流、数据科学pipeline）上，基于11个LLM构建agent进行顺序任务流实验，以无记忆基线为对照。核心经验发现是：任务级技能大多是有害的，平均将性能压至基线以下；子任务级技能则平均带来正收益；文本技能比代码技能迁移效果更好。

为了解释这些现象并找到事前诊断工具，论文提出并测量了技能的两个互补属性——specificity（具体性：技能与真实任务的匹配紧密度）和abstractness（抽象性：技能相关性跨任务的均匀程度）。单独两者都不能预测任务成功率，但两者的联合效应能够稳定预测，作者将其形式化为skill utility score。该分数只需技能文本和任务描述即可计算，不需要执行任何任务，因此可以作为任务运行前诊断技能记忆质量的实用指标。此外，子任务级技能和文本技能在该分数上得分更高，进一步支持了分数作为因果中介的合理性。

论文的贡献包括：形式化了跨任务技能迁移的流程；构建了两个agent版本对应不同归纳层面；提供了大规模多模型的经验证据；提出基于组合属性的技能效用分数；并给出了可行的实践建议（优先子任务级归纳和文本格式）。局限性在于基准和任务类型有限，Docker沙盒约束阻碍了视觉任务，且未处理动态演进记忆，任务评分仅基于最终状态。整体上，该工作为LLM agent如何构建可迁移技能记忆提供了重要的实证基础和可操作诊断工具。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你的智能体（agent）方向直接相关，权重0.10，核心关注点正是LLM智能体经验积累与技能复用。

## 基本信息

- 作者：Yiyang Feng, Biddut Sarker Bijoy, Niranjan Balasubramanian, Jiawei Zhou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-08-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.20274`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据（abstract、method、conclusion、introduction、limitations等片段），并结合元数据与启发式草稿补全；部分相关工作和机制解释为合理推断并已标注，具体实验数值和统计量因证据不足未予编造。
