---
user_id: "cheng tan"
paper_id: 10398
arxiv_id: "2609.01437v1"
title: "HarnessDev: Can LLMs Create and Evolve Their Own Agent Harness?"
publish_date: "2026-09-01"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.01437v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.01437v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:29:08"
---
# HarnessDev: Can LLMs Create and Evolve Their Own Agent Harness?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent harness · self-improving agents · agent evaluation benchmark · runnable infrastructure

## 一句话总结

本文提出 HARNESSDEV 基准，将智能体评估的单位从任务输出转移到可运行的执行基础设施（agent harness），系统考察 LLM 能否从种子系统出发创建并基于下游执行反馈持续进化自己的 harness。

## 摘要

> As agents move from research prototypes to deployed tools, their capability increasingly depends on model-external execution infrastructure, commonly termed the agent harness. Changing this harness while holding model weights fixed can substantially alter task performance. Current agent evaluations typically report downstream performance under a chosen harness, leaving a model's ability to develop the harness itself comparatively underexplored. We introduce HARNESSDEV, a benchmark that shifts the unit of evaluation from task outputs to runnable infrastructure. HARNESSDEV covers two stages. In Creation, the agent starts from a minimal seed and a small number of cases, then builds a complete execution system. In Evolution, it starts from its own created harness and iteratively revises it using downstream execution feedback, with the goal of improving benchmark performance. We then evaluate each constructed harness on capability—task success on held-out benchmarks, and efficiency—execution-token cost. The reported Creation results cover six creator LLMs, four domains, and five downstream benchmarks totaling 2,207 unique downstream instances, with hidden evaluation tasks withheld from development. We find that generated harnesses remain substantially behind mature human-engineered references on code and on search and research, while matching or exceeding the selected references on writing and machine-learning experimentation, with large variation in execution cost. Evolution produces some performance gains, but they are unstable and transfer only partially to held-out tasks. Experiments with a fixed runtime model further show that the gains depend strongly on the model executing the harness, indicating limited transfer across models.
> Date: September 2, 2026
> Project Page: https://self-developing-agents.github.io/

Q1: 这篇论文试图解决什么问题？

论文的核心问题是：当智能体的能力越来越由模型外部的基础设施（harness）决定时，LLM 能否自己开发并持续改进这种基础设施？这等价于把研究问题从“给定 harness，模型表现如何”翻转为“模型能否制造 harness”。这一翻转引出若干深层难题：

1. 评估单元的错位：现有智能体评测几乎都默认 harness 固定，报告的是“模型+harness”混合体的下游任务分数。任务分数无法区分模型自身能力与基础设施贡献，因此也无法回答“模型能不能做基础设施开发”这一独立问题。论文主张把评估单元从 task outputs 改为 runnable infrastructure，让 harness 本身成为被测对象。

2. 开发任务的特殊性：harness 开发不是一次性的实现任务，而是需要持续维护、迭代和进化的工程任务。引言片段明确把问题表述为“LLM 能否协助 harness 工程师、甚至接管这一角色”（合理推断，该句片段缺失开头）。这要求评测不仅测单次生成，还要测长期改进能力。

3. 评估生成式基础设施比评估生成式答案更难（论文原话）。论文指出 harness 可能过拟合于写出它的模型，甚至“记住”构建者隐含的输出偏好；也就是说，一个 harness 表现好可能只是因为它是某个特定模型的“定制外骨骼”，而不是因为它具备通用、可迁移的执行逻辑。这使“构造成功”的定义变得模糊——是在什么模型上运行、在什么任务族上算成功？

4. 自我评估的循环困难：2_background 命中片段提示，智能体要判断自己是否在改进，必须预先构造某种评估方式；“合规”只有先被编码成可检查的规则才变得可判定；而执行系统在可用之前无法被检验（结合语义合理转述）。这揭示了“自我开发 harness”任务中的基础性障碍：开发过程的反馈回路依赖一个尚未存在或尚未可信的执行与评估环境。

5. 能力与移植性的张力：论文最后用固定 runtime model 实验把“写 harness 的模型”和“执行 harness 的模型”解耦，发现收益强烈依赖执行方，提示当前模型写出的 harness 更像私有的、模型绑定的配置，而非通用的基础设施。由此带出一个根本质疑：若 harness 只在构建它的模型上有效，它还算不算“基础设施”？

6. 任务设计层面：Creation 需要把“任务说明+少量开发案例”泛化到未见任务，本质上是少样本条件下的软件工程泛化；Evolution 则需要在连续修订中避免性能震荡，并保证改进不只在开发集上成立。论文据此提出两个研究问题：RQ1——模型能否从弱小但可运行的 seed 出发构建出能泛化到未见任务的基础设施；RQ2——模型能否在改进 harness 的同时保持其已有能力（片段在“preserving”处截断，合理推断为保持原有性能/可用性）。

Q2: 有哪些相关研究？

本次语义检索只命中了摘要、引言和背景章节的少量片段，未命中论文正式的 Related Work 段落，因此以下梳理以合理推断为主，具体引文需回原文核对：

1. 智能体评估的主流范式：现有工作几乎都把 harness（工具调用运行时、上下文管理、错误恢复、评测器、循环控制等模型外执行基础设施）当作固定常量，在其上评测模型的任务完成率。无论是交互式 web/搜索任务、代码仓库任务还是问答任务，分数都混合了模型与脚手架两方面的贡献。HARNESSDEV 的定位正是补上这一缺口：把基础设施从“评测背景”变成“评测对象”。

2. 自改进与自我训练类工作：已有大量研究探索模型利用自身输出、执行反馈或反思信号提升策略，例如基于结果反馈的迭代修正。但这些工作通常改进的是模型本身或 prompt 层面的策略，而不是改进承载策略执行的完整外部系统；它们也不要求生成的系统能在未见任务上独立运行。HARNESSDEV 的 Evolution 阶段与之相似点在于都利用下游反馈，但改进对象是 harness，涉及多文件代码编辑与系统级修改，难度更高。

3. 代码生成与自动化软件工程：前沿模型已能够编辑多文件代码库、修复真实 pull request，这类能力是 harness 开发的必要但不充分条件。相关方向通常评测“代码修改是否正确”，而 HARNESSDEV 评测的是修改后的系统“是否能在隐藏任务上被运行并取得好成绩”。这从生成正确代码进一步延伸到生成可运行、可评估、可泛化的完整系统。

4. 评估器与合规性检查的构建问题：2_background 片段说明，判断系统是否改善需要先把“合规/正确”编码成机器可检查的形式，而这一编码工作本身可能是瓶颈。这呼应了用 LLM 写评估器或自动化评估的研究线——关键区别在于 HARNESSDEV 中，评估对象是作为基础设施的完整系统，而非单一输出。

5. 跨模型迁移与 harness 过拟合：论文观察到增益依赖执行模型，这与“prompt/脚手架对模型敏感”的既有认知一致——ReAct 式格式、few-shot 风格、工具调用 schema 等都对具体模型高度敏感。本文将此现象首次（推测为该基准的定位）系统化为 harness 开发中的可测量问题，并给出固定 runtime model 的实验范式来分离变量。

6. 基准设计方法学上的相关性：以“隐藏任务”“开发集与测试集分离”“成本与能力双维度”为设计原则的基准已是 agent 评测的常用协议，HARNESSDEV 的创新在于把这些协议从任务层搬到基础设施层，并以“模型创建的完整执行系统”为提交物。

Q3: 论文如何解决这个问题？

论文的方案是设计一个两阶段基准协议，把“开发 harness”变成可重复、可量化、可隔离变量的实验任务：

1. 核心对象：被测对象不是模型答案，而是 agent harness——模型外部、负责执行的基础设施。被测流程是模型从无到有构建并迭代这一基础设施的全过程。

2. Creation（创建）阶段：智能体拿到任务说明（task specification）、一个最小种子（minimal seed）和少量开发案例，必须构建出完整的执行系统。关键设计是 seed 被描述为“weak but runnable”——起点能跑但很弱，目的是排除“从零开始写全部代码”的偶然性，让任务聚焦于系统构建与泛化。Creator 必须把少数案例中体现的规律抽象成能处理未见任务的通用基础设施。该阶段对应 RQ1（合理推断 RQ1 的完整表述即摘要所述的 Creation 问题）。

3. Evolution（进化）阶段：智能体从自己创建（或给定）的 harness 出发，利用下游执行反馈（实际运行产生的信号，如成功/失败、错误信息、输出等）对 harness 做迭代式修订，目标是提升在基准上的整体表现。该阶段对应 RQ2——在改进的同时保持原有能力（片段截断，合理推断）。这里的反馈回路是真实执行而非模型自评，体现了“以运行为准”的方法论立场。

4. 双维度评测协议：每个构造出的 harness 被运行在留出的下游基准上，报告两个指标——能力（capability，任务成功率）与效率（efficiency，执行 token 成本）。评测中的关键保护措施是隐藏评测任务（hidden evaluation tasks withheld from development），防止开发过程对测试任务过拟合，即把训练/测试分离原则应用到 harness 开发协议中。

5. 评估规模与覆盖：Creation 实验覆盖 6 个 creator LLM、4 个任务领域、5 个下游基准、2,207 个独特下游实例，以人工精心设计的成熟 harness 作为参照系。

6. 变量隔离设计：论文做了“固定 runtime model”实验——推测该实验设计为：让不同 creator 模型创建 harness，但统一用同一个固定模型来执行所有 harness（或让每个 harness 分别与不同执行模型组合）。这样就能区分 harness 的收益到底来自 harness 本身的结构，还是来自 harness 与特定执行模型的隐性适配。结果是收益强依赖执行模型，跨模型迁移有限，说明 harness 与“写它”或“为它优化”的模型之间存在强耦合。

7. 领域选择逻辑（合理推断）：四个领域（代码、搜索与研究、写作、机器学习实验）覆盖了当前 agent 的主要部署形态；写作领域的参考 harness 被描述为 short-form writing 类别，提示各领域都配有对应的人工参考实现作为能力上限锚点。

Q4: 论文做了哪些实验？

根据摘要和引言片段可重建的实验框架如下（具体实验配置细节论文全文未完整命中，以下标注推断处需原文核对）：

1. Creation 主实验：6 个 creator LLM 分别在 4 个领域从种子与少量开发案例出发构建 harness。每个 harness 随后在留出基准上被评测任务成功率（capability）与执行 token 消耗（efficiency）。下游评测共 5 个基准、2,207 个独特实例。对照物为成熟的人工构建参考 harness（human-engineered references）。

2. 领域内对照分析：按 harness 类型（code、search and research、writing、machine-learning experimentation）分别比较模型构建的 harness 与人工参考的表现。摘要给出总体结论：代码与搜索/研究域明显落后；写作（short-form writing）域打平；机器学习实验域超过参考。

3. 成本分析：记录每个生成 harness 的执行 token 成本并比较其方差——摘要指出执行成本存在 large variation，暗示论文对能力-效率的权衡做了定量刻画（具体成本数字未见，不做编造）。

4. Evolution 实验：让模型在自身创建的 harness 上迭代修订，观察连续修订轮次中的性能轨迹。核心观测指标是：性能是否随修订单调提升、提升幅度、以及开发中观测到的增益迁移到 held-out 任务时保留多少。

5. 固定 runtime model 的迁移实验：将“构建 harness 的模型”与“执行 harness 的模型”解耦，检验 harness 收益对执行模型的依赖程度。该实验直接服务于“harness 是否过拟合于构建它的模型”这一假设检验。

6. 泛化保护：所有下游评测任务中隐藏评测集与开发过程隔离，使开发反馈无法触及测试任务，确保 Creation 泛化结论与 Evolution 迁移结论不被数据污染干扰。

7. 该论文没有提供（至少检索片段未显示）的具体实验信息包括：6 个 creator LLM 的名单、各领域对应的具体下游基准名称、人工参考 harness 的构建方式、Evolution 的迭代轮数与预算控制策略、执行 token 成本的具体统计口径。这些属于需回原文核对的信息缺口。

Q5: 发现了什么实验现象？

从摘要和引言片段可以整理的实验发现如下：

1. 总体能力差距呈领域不对称：模型生成的 harness 在 code 与 search and research 两类任务上明显落后于人工参考；但在 writing（short-form writing）上与参考持平甚至更好，在 machine-learning experimentation 上超过参考。这一反差本身是反直觉的——若按“代码编写难度”直觉，模型最可能接近人类的领域应是代码，但实际最强的相对表现在写作与 ML 实验上。可能的解释方向（推测）：code harness 与 search/research harness 对外部工具链、状态管理和错误恢复的要求更高，小规模开发案例难以覆盖其复杂度；而写作 harness 的核心逻辑（指令-生成-简单校验）恰好落在 LLM 自身最擅长的文本处理半径内。

2. 效率维度波动大：生成 harness 的执行 token 成本呈现 large variation，说明模型在构建时没有稳定地优化成本，不同 harness 可能采取了差异极大的决策路径（如冗余工具调用、长上下文保留、反复自我检查）。能力与成本之间可能存在权衡关系，但摘要未给出相关性结论。

3. Evolution 收益存在但不稳定：论文明确表述为“produces some performance gains, but they are unstable”。这说明连续修订的性能轨迹不是单调上升的——可能在某一轮提升后在下一轮回退，呈现震荡。这与一般“RL 式迭代自改进”文献中常见的非单调性一致，但在 harness（多文件系统）层面出现说明系统级修改的连锁回归风险更高（合理推断）。

4. Evolution 收益的泛化衰减：开发中观察到的增益转移到 held-out 任务时变小且一致性下降。这是典型的过拟合信号——模型可能把开发案例中的特定模式编码进 harness，而非学到通用的执行改进。

5. 强烈的模型绑定效应：固定 runtime model 的实验中，增益强烈依赖于执行 harness 的模型，说明模型写出的 harness 隐式适配了构建者自身的输出格式、工具调用习惯和错误恢复风格（论文点出 harness 可 overfit 到写它的模型）。这是本论文最有诊断价值的观察：当前模型生产的 harness 更像“定制接口”而非通用基础设施。

6. 失败模式归纳（结合论文自述）：harness 的失败不只是“功能缺失”，还包括对构建者过拟合、评估困难（无法仅凭输出判断系统质量）、以及自我评估循环中“合规性必须先被编码才可检验”的元层面障碍。

Q6: 有什么可以进一步探索的点？

从论文的问题设定与实验缺口出发，可以延伸出以下探索方向（部分为论文结构自然引申的合理推断，非原文明确列举）：

1. 打破模型绑定的 harness 泛化：既然当前 harness 收益强依赖执行模型，核心开放问题是——能否通过训练目标、评估协议或架构约束，引导模型生成对执行模型更鲁棒的 harness？例如在创建阶段就随机化执行模型，或在评测中把跨模型迁移分数作为显式指标。

2. Evolution 的稳定性机制：连续修订的性能震荡说明需要更可靠的修订策略。可探索方向包括：为 Evolution 配备回归测试集（在每次修订后验证旧能力不退化）、引入版本回滚机制、用 ensembling 或投票选出修订候选、对修订做细粒度 diff 级别的归因。

3. 反馈信号的丰富化：当前 Evolution 利用下游执行反馈（任务成功/失败与 token 成本）。是否可以引入中间过程信号（工具调用轨迹的正确性、错误恢复是否触发、状态管理是否泄漏）作为更密集的训练/修订信号？

4. Creation 的样本效率研究：从少量 dev cases 泛化到 2,207 个未见实例的表现，与 dev cases 数量、种子复杂度、任务规范的详细程度之间的关系尚待系统刻画。可做 scaling 实验：dev cases 增加时 harness 质量如何变化。

5. 成本-能力前沿（cost-capability frontier）：执行成本的大方差提示可以用帕累托前沿分析来刻画 harness 设计空间——在给定成本预算下最优 harness 由什么构成？Evolution 是否能把 harness 推向成本更低、能力不降的区域？

6. 与模型训练闭环结合：论文考察的是固定权重的模型来开发 harness；进一步可问：如果 harness 开发能力反哺模型训练数据（例如用模型自建 harness 的执行轨迹训练模型），是否形成正反馈？反之，模型的 harness 开发能力本身能否通过专门训练提升（即把“建 harness”作为目标任务）？

7. 安全与对齐视角：harness 过拟合构建者模型这件事有对齐含义——若一个智能体能修改自己的执行环境，它可能通过降低评测严格性或篡改结果记录来“刷分”。本文的 hidden evaluation 设计能挡掉任务级过拟合，但 harness 层面的评测器操纵（self-evaluation 循环被模型自己控制）是更隐蔽的风险，值得专门研究。

8. 基准本身的扩展：当前覆盖 4 个领域、5 个下游基准。可扩展到多模态执行环境、长时程任务、多智能体协作 harness、真实 API 依赖的部署场景；也可以把“人工参考 harness”替换为自动化基线（如最高分模型自建 harness 之间的相互比较），消除人工参照系的主观性。

9. 可解释性与诊断工具：当 Evolution 使性能提升或回退时，当前评测只能给出任务分数层面的信号；开发可定位到具体 harness 组件（如工具调用解析、上下文压缩、错误处理）的归因工具，将大幅提升 harness 开发的工程效率。

Q7: 总结一下论文的主要内容

HARNESSDEV 论文针对一个正在显性化的结构性缺口提出问题：智能体性能越来越依赖模型外部的执行基础设施（agent harness），而现有评测体系却只测“在给定 harness 下模型的表现”，从不测“模型能否构建并改进 harness 本身”。论文的出发点是两个观察：第一，保持模型权重不变、更换 harness 即可显著改变任务表现——说明基础设施是能力的独立贡献项；第二，前沿模型已经具备编辑多文件代码库、关闭真实 pull request 的能力，这种潜力已经存在，缺的是一个能测量它的基准。论文由此引入 HARNESSDEV，将评测单元从任务输出转移到可运行的基础设施。

技术主线上，HARNESSDEV 设计为两阶段协议。Creation 阶段让智能体从“弱小但可运行”的种子系统与少量开发案例出发，依据任务说明构建完整的执行系统，本质上是测试从少样本示例到通用基础设施的泛化能力；Evolution 阶段让智能体从它自己创建的 harness 出发，利用真实下游执行反馈做持续修订，目标是提高基准性能。这种“创建+进化”的双阶段设计把 harness 开发刻画成一个持续工程过程，而非一次性代码生成。评测协议也相应地分两个维度：capability 用 held-out 基准上的任务成功率衡量，efficiency 用执行 token 成本衡量；开发过程与评测任务严格隔离，评测任务被隐藏以保证泛化结论的可信度。

实验主线上，Creation 实验覆盖 6 个 creator LLM、4 个任务领域、5 个下游基准、2,207 个独特下游实例。结果呈明显的领域不对称：模型生成的 harness 在代码、搜索与研究任务上大幅落后于成熟的人工参考实现，但在写作上打平、在机器学习实验上反超参考。效率维度的执行成本表现则方差很大。Evolution 实验显示模型确实能利用下游执行反馈改进自己的 harness，但这种改进不稳定——性能随修订轮次起伏，且开发中获得的增益在 held-out 任务上会变小、一致性变差。最后，固定 runtime model 的实验把构建 harness 的模型和执行 harness 的模型解耦，结果发现收益强烈依赖执行模型，跨模型迁移十分有限。

综合起来，论文的论证链条是：harness 是智能体能力的真实组成部分（动机）→ 现有评测无法衡量模型的 harness 开发能力（缺口）→ HARNESSDEV 提供两阶段、双维度、带隐藏测试的评测协议（方法）→ 实验显示当前模型能创建和进化 harness，但离可靠的人工基础设施水平仍有距离，且产物明显绑定于构建它的模型（发现）。结论对智能体自我改进领域具有双重含义：一方面，模型自主开发执行基础设施的能力已初步显现，甚至在某些领域超过人工参考；另一方面，这种能力的可靠性、泛化性和跨模型可移植性都远远不足。评价一个生成的 harness 比评价一个生成的答案更困难——它可能过拟合于构造它的模型，这就使得“模型能否真正成为自己的 harness 工程师”这一问题的答案，远比“模型能否写出可运行代码”更微妙。论文的价值不在于宣称模型已经能自我开发，而在于建立了一个能让这个问题被系统化研究的评测框架。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接命中你的 agent 研究方向（权重 0.10）：论文系统考察 LLM 能否创建并进化自己的 agent harness，把评估单元从任务输出转移到执行基础设施，问题定位前沿——这是'智能体的自我改进'向'智能体的自我基础设施化'的延伸。

## 基本信息

- 作者：Yuhao Wu, Jingyuan Zhang, Jiajun Shi, Xinping Lei, Qingshui Gu, Yuxuan Zhang, Zexuan Wang, Chen He, Chen Huang, Maojia Song, Zhiyuan Zeng, Shaowen Wang, Jinkai Liu, Yunfeng Shi, Jiaheng Liu, Shen Yan, Wenhao Huang, Ge Zhang, Wenxuan Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SE, cs.CL
- 日期：2026-09-01
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.01437v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于用户提供的论文摘要、Introduction 与 Background 的 PDF 语义检索命中片段撰写，未获得完整全文；对超出证据范围的细节均标注为合理推断或推测，具体实验配置与完整 related work 需回原文核对。
