---
user_id: "cheng tan"
paper_id: 7414
arxiv_id: "2608.09044v1"
title: "Tree-of-Experience: Hierarchical Experience Management for Self-Evolving Agents"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.09044v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.09044v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-12T01:23:47"
---
# Tree-of-Experience: Hierarchical Experience Management for Self-Evolving Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agent · experience management · hierarchical reasoning · self-evolving agents

## 一句话总结

本文提出 Tree-of-Experience (ToE) 框架，将 LLM 智能体的经验组织为与分析视角和推理路径对应的共享树结构，并通过环境结果反馈校准节点可靠性，从而在复杂推理任务中实现细粒度归因、跨任务迁移、系统性更新与高效检索，显著提升问题求解性能与效率。

## 摘要

> Continual self-evolution requires LLM agents to transform environmental interactions into reliable and reusable experience. Existing methods typically refine individual trajectories or abstract shared knowledge from related trajectories, but their experience representations are often disconnected from the underlying reasoning process. This limits feedback attribution, cross-task transfer, and update and retrieval efficiency, particularly in complex reasoning tasks with outcome-level feedback. To overcome this limitation, we propose \textbf{T}ree-\textbf{o}f-\textbf{E}xperience (ToE), a structured experience-management framework that aligns experience organization with the hierarchical reasoning process of LLM agents. Specifically, ToE organizes the experience into a shared tree of analytical perspectives and reasoning paths, whose reliability is calibrated through environmental outcomes to support systematic updating, transfer, and efficient retrieval. The experimental results on \textsc{Game of 24} and \textsc{FinEvolveBench} show that ToE substantially improves both problem-solving performance and efficiency. On \textsc{Game of 24}, ToE achieves a 31.4\% relative improvement in accuracy over the experience-free ToT baseline. On \textsc{FinEvolveBench}, ToE improves tsIC by an average of 41.24\% over the experience-free pipeline across 12 evaluation settings, whereas conventional experience-management methods often underperform experience-free baselines.

Q1: 这篇论文试图解决什么问题？

这篇论文针对的核心问题是：LLM 智能体如何从与环境的持续交互中真正‘获得经验’，而不仅仅记录轨迹。作者指出，交互历史不会自动变成经验——经验是对‘什么重要、为何重要、何时可能再次重要’的结构化解释。因此，智能体需要通过反思过滤偶然细节、提取可迁移的经验，才能用过往结果更有效地解决相关问题。

现有经验抽象机制分为两类：
1. 轨迹内转换（intra-trajectory transformation）：例如 Reflexion、MemRL、ReMe，将每个轨迹精炼为仍绑定在原始任务上下文中的经验单元，推理时检索语义相似任务来辅助。这类方法虽然减少了原始轨迹中的噪声与幻觉，但记忆常常碎片化、上下文依赖强，导致可迁移性受限，同时增加了检索成本与推理开销。
2. 轨迹间归纳（inter-trajectory induction）：例如 FLEX，从相关轨迹中抽象共享知识。这类方法在任务重复度较高时能压缩经验，但在复杂推理任务中，因为结果级反馈（只有最终对错或分数）难以准确归因到具体推理步骤，归纳出的经验可能被错误强化，甚至导致性能不如无经验基线。

论文进一步指出，现有经验表示与底层推理过程‘脱节’是根本障碍。由于缺乏对推理过程的细粒度对应，智能体很难说清是哪一步导致了失败或成功，跨任务迁移时无法找到可复用的推理模式，更新时也难以精准修正错误节点。尤其在复杂推理（如多步数学推理、金融决策）中，结果级反馈只给出最终成败，没有中间监督，传统经验管理会出现‘整体表示无法定位错误’的问题。

因此，论文要解决的科学问题可以拆解为三个子问题：
- 推理粒度：经验应该挂在哪个粒度的推理单元上（单步、路径、视角？）；
- 可靠性校准：如何利用环境结果反馈来区分哪些经验可靠、哪些不可靠；
- 更新与检索效率：如何在不重放全部历史的前提下，系统性地添加新视角、修正旧路径、合并冗余节点，并在推理时快速找到最相关的经验。

Q2: 有哪些相关研究？

相关研究围绕 LLM 智能体的记忆与经验管理展开，大致分几个脉络：

1. 轨迹内经验精炼（Intra-trajectory Transformation）：
 - Reflexion (Shinn et al. 2023)：让智能体用语言反思自己的失败，将反思结果作为记忆用于后续尝试。它给出的经验是自然语言的自我批评，不显式建模推理步骤间的因果结构。
 - MemRL (Zhang et al. 2026b) 和 ReMe (Cao et al. 2026) 等后续工作进一步把轨迹精炼成更紧凑的经验单元，并引入记忆更新机制，但这些经验仍然与具体任务上下文绑定，迁移时需要语义匹配，跨任务泛化能力有限。

2. 轨迹间知识归纳（Inter-trajectory Induction）：
 - FLEX (Cai et al.…) 等从多条相关轨迹中提取共享的‘经验模板’或规则，以压缩存储、提高复用。这类方法在任务重复度较高时有效，但当反馈是结果级的、任务重复度低时，容易出现归因错误——把偶然因素当成规律，从而强化了不可靠经验。

3. 树状推理与规划：
 - Tree-of-Thoughts (ToT, Yao et al. 2023) 是本文在 Game of 24 上对比的推理基线。ToT 将推理过程组织为树，但没有专门的经验积累机制，每次新问题都要从头探索，缺乏跨问题复用。

4. 经验管理与自进化智能体：
 - 更广义的 self-evolving agent 框架涉及长期记忆、工具使用、策略学习。本文的独特之处在于把‘经验’组织成解析视角与推理路径的共享树，用环境反馈校准可靠性，使得经验与推理过程同构。

总体来看，已有方法要么在轨迹级别做‘事后总结’，要么在多轨迹层面做‘共性抽取’，但都没有把经验表示嵌入推理过程的中间结构，导致归因粒度粗糙。ToE 的定位是填补这一空缺，用树结构把‘分析视角’（高层的策略或认知模式）和‘推理路径’（具体的多步推理链）统一管理。

Q3: 论文如何解决这个问题？

ToE 的核心思想是让经验组织与智能体的层级推理过程对齐。具体而言，ToE 把经验表示为一棵共享的树：树节点对应‘分析视角’（analytical perspectives）和‘推理路径’（reasoning paths），根节点代表任务级抽象，中间层是更细的视角，叶子层是具体的推理步骤或完整路径。经验不是与单条轨迹绑定，而是沉淀为这棵树的对应节点。

方法要解决三个关键问题：
1. 推理粒度（reasoning granularity）：经验应该挂在哪个层级？作者选择把‘分析视角’作为中层节点，把具体的‘推理路径’作为低层节点。分析视角是指一种通用的解题方向或策略，例如‘先尝试将较大数分解’或‘用目标数反推’；推理路径则是该视角下实际执行的具体步骤序列。这样的分层让经验既能泛化（视角级迁移），又能具体指导（路径级回放）。
2. 可靠性校准（reliability calibration）：环境反馈（如最终答案是否正确、金融收益指标）被用来评估每个节点的可靠程度。论文采用‘通过环境结果校准’的方式——例如统计某视角在不同问题上的成功率，或某路径在类似场景中的胜率，据此给节点打分。这样既保留了成功经验，也标注了失败经验，并避免把不可靠的偶然因素当作普适规律。校准后的可靠性信息支持系统性更新：当新结果到来，更新对应节点的统计量；当冗余节点出现，合并；当新视角被证明有效，加入树中。
3. 更新与检索效率：ToE 支持增量扩展，不需要重训或重放全部历史。推理时，智能体根据当前问题状态，从树中检索高可靠性且相关的视角与路径，把它们作为上下文提示，引导新问题的推理。检索效率比逐个轨迹比对更高，因为树的层级结构能快速剪枝。

实现细节（合理推断）：假设每个问题先用 ToT 或类似多路径探索生成多个推理轨迹，然后对轨迹进行结构化分析，抽取共有的分析视角，把相同视角下的路径归为一组，形成树的中间节点。再用环境结果（正确/错误）来更新节点的可靠性计数。之后新问题到来时，从树中选出可靠性阈值之上的视角，指导新的生成，生成的新路径再回流更新树。这种‘探索-归纳-校准-检索-再探索’的闭环是 ToE 的总体流程。

论文强调这种表示与推理过程同构，因此获得三点好处：
- 细粒度结果归因：能把最终结果归因到具体的视角或路径，而不是整条轨迹；
- 跨任务迁移：视角这种相对抽象的单元能在不同任务间复用，路径则作为适配性强的示例；
- 更新与检索的高效性：树结构天然支持局部更新和剪枝检索。

Q4: 论文做了哪些实验？

论文在两个互补的基准上评测 ToE：
1. Game of 24：经典的数学推理挑战，给定四个数字用四则运算得到 24。任务需要多步搜索，反馈是最终表达式的正确性（结果级反馈）。论文对比的无经验基线是 Tree-of-Thoughts (ToT)（Yao et al. 2023）。结果：ToE 相比经验-free 的 ToT 基线在准确率上相对提升 31.4%，并且显著减少了 LLM 调用次数（结论中明确提到‘substantially reducing LLM calls’）。这意味着 ToE 在提升性能的同时还带来了效率收益。
2. FinEvolveBench：论文引用的另一个基准（Deng et al. 2026）。从名称推测，这是一个金融决策演化基准，评估智能体在动态金融环境中持续决策的能力。其特征是任务重复度低、结果反馈延迟且隐含（结论片段提到‘task repetition is low and outcome feedback is delayed and implicit’）。论文报告 tsIC 指标（financial/economic context 中可能指 time-series IC 或 trimmed IC，具体含义摘要未给，合理推断为排序相关性或决策质量指标）在 12 个评估设置上平均相对提升 41.24%（相比 experience-free 的流水线）。

实验设计要点：
- 对比对象包括：无经验基线（ToT / pipeline）、传统经验管理方法（如轨迹内精炼、轨迹间归纳的代表）。论文特别指出传统方法在这些设置上常常不如无经验基线，这说明经验管理方法在复杂推理任务中容易翻车。
- 跨模型验证：ToE 在不同 backbone LLM 上保持有效性（结论最后一句‘remains effective across different backbone LLMs’），说明方法泛化性较好。
- 评估设置共有 12 个，可能包括不同的任务子集、模型或反馈形式（论文摘要未展开具体设置）。

从检索到的实验片段看，论文主要在 Results 部分展示数字，但没有给出详细消融表格。因此无法从已有证据中得知树深度、节点数量、可靠性阈值等超参数的影响。

Q5: 发现了什么实验现象？

从摘要、结论和实验片段中，我们可以归纳出以下实验现象：
1. ToE 在复杂推理任务中明显优于无经验基线：Game of 24 准确率相对提升 31.4%，说明经验管理确实起作用，而且‘结构化树’这种组织方式比重新探索更高效。
2. 传统经验管理方法在 FinEvolveBench 这类任务上会失效甚至拖后腿：任务重复度低、反馈延迟且隐含时，从单条轨迹精炼或从相似轨迹归纳出的经验质量无法保证，反而引入噪声。这是反直觉的——通常认为经验总比没有强，但这里显示错误归因的经验会害了智能体。
3. ToE 在低重复度、延迟反馈场景下依然有效：FinEvolveBench 平均 tsIC 提升 41.24%，说明树结构 + 可靠性校准能克服负迁移，这是方法最亮眼的点。
4. 效率提升：在 Game of 24 上显著减少 LLM 调用次数，说明检索到的经验能引导搜索走正确方向，减少了盲目试探。
5. 跨模型稳定性：多个 backbone LLM 上都有效，说明 ToE 不是对特定模型的过拟合技巧。

尚未确认的细节：
- 论文没有给出两个基准上具体 baseline 方法的名称和完整对比表（可能摘要未摘录）；
- 没有消融研究（例如去掉可靠性校准、去掉视角层、不同树结构）的定量结果；
- 没有展示树增长的复杂度或存储开销；
- tsIC 的具体定义、计算方式以及 12 个设置的具体组合不可得。

Q6: 有什么可以进一步探索的点？

基于论文的问题和方法，可以进一步探索的方向包括：
1. 更丰富的反馈信号：目前主要依赖结果级反馈，未来可探索过程级反馈（如中间步骤验证）、多源反馈（人类判断、环境奖励）融合进来校准节点可靠性。
2. 自动树结构演化：当前树的分析视角可能需要人工先验或由 LLM 归纳，能否用算法自动决定树的深度、分支数和节点粒度，甚至让树结构随任务分布动态生长或修剪。
3. 跨任务与跨领域的迁移：ToE 在 Game of 24 和 FinEvolveBench 上的成功是否能扩展到代码生成、数学证明、科学研究等更广泛的任务？特别是‘分析视角’这种抽象单元是否具有通用性。
4. 与其他记忆机制的结合：例如把 ToE 的长时经验与工作记忆、情景记忆融合，与工具使用和反思机制结合，构建更完整的自进化智能体架构。
5. 可靠性校准的理论分析：为什么结果级反馈下，树形结构能避免负迁移？能否从贝叶斯或因果角度给出误差界的分析？
6. 效率与扩展性优化：随着应用时间增长，树不断变大，如何控制检索延迟和存储成本？可能需要近似检索、层级缓存或遗忘机制。
7. 处理反馈噪声和延迟：FinEvolveBench 已隐含延迟反馈，但如果反馈是稀疏的、有噪声的，校准策略需要更鲁棒，例如引入不确定性估计。
8. 与在线强化学习的结合：树结构中的可靠性类似价值估计，可以尝试用强化学习策略更新树的路径选择，而不仅仅是检索固定的高可靠路径。

Q7: 总结一下论文的主要内容

论文《Tree-of-Experience: Hierarchical Experience Management for Self-Evolving Agents》围绕‘LLM 智能体如何自我进化’这一核心议题展开。它的出发点是：智能体必须能从反复试错中积累经验，但‘交互历史’与‘经验’之间存在本质鸿沟——经验需要被结构化解释为‘什么重要、为何重要、何时再起作用’。现有经验管理方法分为两条路线：轨迹内精炼（Reflexion、MemRL、ReMe）和轨迹间归纳（FLEX）。轨迹内精炼得到的是绑定在特定任务上下文中的记忆碎片，迁移性差、检索开销大；轨迹间归纳从多条轨迹抽取共性，但在复杂推理任务中难以把结果级反馈准确归因到具体推理步骤，导致经验被错误强化，性能反而不如无经验基线。

为了克服上述困难，作者提出 ToE，其核心主张是‘经验的组织应与智能体的层级推理过程对齐’。ToE 将经验表示为一棵共享树，树的节点分为两级：分析视角和推理路径。分析视角对应一种通用的解题策略或认知模式，推理路径则是该视角下的具体多步推理序列。每个节点的可靠性通过环境结果的反馈（例如最终对错或金融指标）来标定。这样构建的经验树具有三个特性：一是支持细粒度结果归因——能定位到具体视角或路径，而不是整条轨迹；二是支持视角级跨任务迁移——抽象视角可在不同任务间复用，具体路径提供可参照的示例；三是支持系统更新和高效检索——新经验增量加入，冗余节点可合并，推理时能按可靠性剪枝检索。

在实验部分，作者选择了两个互补的任务：Game of 24（数学推理、结果反馈直接）和 FinEvolveBench（金融决策、重复度低、反馈延迟且隐含）。结果显示：在 Game of 24 上，ToE 相比无经验 ToT 基线准确率相对提升 31.4%，并显著减少 LLM 调用次数；在 FinEvolveBench 上，ToE 在 12 个评估设置中平均提升 tsIC 41.24%，而传统经验管理方法往往低于无经验基线。此外，ToE 在多个 backbone LLM 上保持有效性，说明其泛化性较好。论文的结论强调，在任务重复度低、反馈延迟隐含的现实场景中，现有记忆机制经常失效甚至退步，而 ToE 通过结构化经验表示和可靠性校准获得了一致的性能提升。

整体上，论文的贡献在于提出了一个将经验表示与推理过程同构的框架，把‘经验管理’从扁平记忆或轨迹总结推进到层级结构的精细化操作，并且用实验证据表明这种结构在复杂的、反馈稀疏的环境中能带来实质收益。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文主题与‘agent’方向直接重合，并涉及’ai-for-science‘中常见的复杂推理与决策场景，与你当前画像中的 top direction（agent 0.10）高度相关。

## 基本信息

- 作者：Zihao Deng, Yining Zhu, Leiming Wang, Jingfei Lu, Junbo Wang, Chuncheng Ran, Yu Yang, Dixuan Yang, Jikun Shen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.09044v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要、引言、方法、结论和实验片段，并结合启发式草稿进行了补全与修正。
