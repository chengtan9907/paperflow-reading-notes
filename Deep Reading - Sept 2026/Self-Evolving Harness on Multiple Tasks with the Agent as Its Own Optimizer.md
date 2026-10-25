---
user_id: "cheng tan"
paper_id: 14393
arxiv_id: "2609.38372v1"
title: "Self-Evolving Harness on Multiple Tasks with the Agent as Its Own Optimizer"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.38372v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38372v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/reading_reports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-08T23:43:54"
---
# Self-Evolving Harness on Multiple Tasks with the Agent as Its Own Optimizer

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-evolving harness · language model agent · recursive self-improvement · multi-task evolution

## 一句话总结

本文提出一个接近递归自我改进的框架：同一个冻结模型在同一份 harness 上先作为 solver 求解多领域任务、再作为 proposer 读取完整运行记录并直接编辑自身 harness，通过多任务预训练与持续训练两个阶段的演化，使 49 行种子 harness 在分布内基准上平均提升 4.48 分、在五个未见过的分布外基准上平均提升 12.64 分，并超过或追平 Codex。

## 摘要

> A harness is the code around a language-model agent that organizes prompts, calls tools, manages context, and controls execution. As models grow stronger, recent work has begun to let agents improve their own harnesses, a line of work known as self-evolving harnesses. In most existing methods, a separate proposer running on a human-designed harness modifies the solver's harness, and a separate harness is evolved for each benchmark. Real-world tasks come from many domains, so both the evolution and the evaluation of a harness should cover a diverse range of tasks. We propose a framework close to recursive self-improvement: the same frozen model, on the same version of the harness, first solves tasks as the solver and then, as the proposer, reads the complete run records and directly edits the harness that runs it. Each evolution batch draws tasks from five benchmarks in different domains. To measure generalization, training and held-out tasks are strictly separated, and we additionally evaluate on five out-of-distribution benchmarks never used during evolution. We frame the evolution process as deep-learning training with two stages, multi-task pretraining and continual training. Starting from a 49-line seed harness, the harness obtained at the end of the first stage improves the average score by 4.48 points on the in-distribution benchmarks and by 12.64 points on the out-of-distribution benchmarks, surpassing Codex on the former and matching it on the latter. In the second stage, continued evolution on Claw-Eval, one of the out-of-distribution benchmarks, further raises the score on that benchmark from 66.17 to 68.06, exceeding Codex. We also provide an in-depth analysis of the mechanisms that emerged during evolution, including output truncation, history compaction, and independent review.

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：如何让语言模型智能体以最小的人类先验、可持续地改进自己外围的控制代码（harness），并让这种改进跨任务、跨领域泛化。具体拆解为三个子问题：（1）理想且实用的自我改进框架应该是什么样：现有方法大多把 proposer 设计为运行在固定、人工设计的 harness 上的独立组件，甚至外包给 Claude Code 等更强商用模型，并且外层循环还加入人工设计的轨迹整理步骤；作者认为这偏离了智能体自主改进的本意，人类先验仍然很多。（2）如何让改进泛化到未见任务：大多数现有方法为每个 benchmark 单独演化一个 harness，容易过拟合到单一 benchmark 的评分器、任务分布和交互风格；真实任务来自很多领域，演化任务应覆盖多领域，且评估必须包括从未参与演化的 benchmark。（3）演化过程是否可以看作模型训练：作者把多轮演化类比为深度学习的 multi-task pretraining 和 continual training，从而借用训练范式的概念来设计演化流程和度量演化行为。此外还有一层工程问题：proposer 读取完整运行记录并直接改动代码，如何保证修改后的 harness 还能通过工程检查、不影响评估公平性（例如评估文件只在 agent 完成后才放入环境，事后审计所有评估轨迹）。

Q2: 有哪些相关研究？

相关工作分三条线。（1）harness 设计对智能体性能的影响：ReAct、CodeAct、SWE-agent 等工作表明动作格式和工具接口的设计会显著改变智能体能力；在模型固定的情况下，仅改变 harness 就能造成很大的性能差异；前沿模型厂商为自己的模型开发专用 harness，如 Codex 和 Claude Code。（2）self-evolving harness 的已有方法：大多遵循 solver 产生轨迹、proposer 读取轨迹并修改 solver harness、循环重复的基本框架；例如 Meta-Harness 的 proposer 是 Claude Code 驱动的 Claude Opus 4.6，HarnessBank 用 Claude Opus 4.8 改进 Qwen3.6-27B 的 harness；外层循环常加入人工设计的步骤，如专门的 agent 组织轨迹。作者指出这些方法中 proposer 通常由更强的模型驱动、运行在固定人工 harness 上，与本文的 same frozen model、same harness 设定不同。（3）智能体自我改进与递归自我改进的相关思想：论文把演化过程类比为深度学习训练，提出 sub-batch 演化、checkpoint、评价分数作为 loss 等概念，与模型训练范式的类比相关。（4）评估与泛化：训练/留出任务严格分离、OOD benchmark 泛化评估的设计，与多任务学习和分布外泛化的评估传统相关。

Q3: 论文如何解决这个问题？

论文提出一个接近 recursive self-improvement 的框架，核心设计原则是：最小化人类先验、给模型最大自由修改 harness。具体做法如下。（1）同一个冻结模型 M 扮演两个角色：solver 在给定任务 x 上以当前 harness Ht 运行并产生完整轨迹 τ = AK(M, Ht, x)，proposer 读取更新任务 It（包含当前 harness 源码、演化笔记、上一批任务的完整运行记录）后直接编辑 harness，得到 Ht+1 = Extract(AK(M, Ht, It))；两者都使用同一模型、同一 harness 版本、各自在全新 session 中运行。（2）外层程序只负责分发任务和打包运行记录，没有专门 agent 整理轨迹或总结错误，从而减少人类设计的步骤。（3）更新任务对 proposer 的指令包括：修改后的 harness 将在未见任务和完全不同的 benchmark 上评估，因此改进必须泛化；允许添加或删除文件、引入任何组件；修改后的版本必须通过若干工程检查（如可运行性检查）。（4）演化按批次进行：每个演化批次从五个不同领域 benchmark 中抽取任务，构成 multi-task pretraining 阶段；持续训练阶段在某个 OOD benchmark 上继续演化，类比 continual training。（5）harness 状态 Ht = (Ct, Nt) 包括全部源码 Ct（系统提示、工具定义与实现、上下文处理、执行控制）和演化笔记 Nt；演化笔记随代码一起传给后续 proposer。（6）评估公平性设计：评估文件、参考答案和评分测试只在 agent 完成后才放入环境；事后用脚本审计所有评估轨迹，扫描命令与输出，防止信息泄露或 harness 修改评估逻辑。（7）演化过程使用 checkpoint 思维，评价分数作为 loss，解决演化中的 credit assignment 问题。

Q4: 论文做了哪些实验？

实验分两个阶段。（1）第一阶段（多任务预训练）：演化任务来自五个领域差异很大的 benchmark——Terminal-Bench 2.1（终端中的长程工程任务，如软件构建和系统配置）、GAIA2（模拟手机应用环境中处理异步请求）、OfficeQA Pro（在近一个世纪的美国财政部公报中检索证据并做数值推理）、τ3-Bench banking（与模拟用户进行多轮对话处理银行业务）、SWE-Bench Pro（修复大型代码仓库中的真实 issue）；共 7 次迭代，从 49 行种子 harness 开始。（2）分布外评估：用五个从未参与演化的 benchmark 测试泛化——SWE-bench Verified（Python 仓库 issue 修复）、DeepSWE（多语言仓库功能开发）、BrowseComp-Plus（在约 10 万网页固定语料上检索问答，语料作为文件放在任务环境中）、APEX-Agents（投行、咨询、法律等专业工作）、Claw-Eval（混合服务编排、多模态理解与多轮对话）。时间限制和评分遵循各 benchmark 官方设置。（3）第二阶段（持续训练）：在 Claw-Eval 上继续演化（Claw-Eval 是第一阶段的 OOD benchmark 之一），目标是从 66.17 提升到 68.06，超过 Codex。（4）机制分析：分析演化中出现的输出截断、历史压缩、独立审查、视觉检查预算、交付截止时间等机制及其对具体 task 的影响（如 M097 视频变速任务、34 个超时任务、1959 年列计数等案例）。（5）对照 baseline：种子 harness 本身（Seed）和 Codex harness。

Q5: 发现了什么实验现象？

实验现象和关键观察包括：（1）演化收益显著且 OOD 泛化强：第一阶段结束时 ID benchmark 平均提升 4.48 分、OOD benchmark 平均提升 12.64 分，OOD 收益大于 ID 收益，说明多领域演化任务带来的改进确实泛化到了未见 benchmark。（2）与 Codex 的比较：在 ID benchmark 上超过 Codex，在 OOD benchmark 上追平 Codex；排除 BrowseComp-Plus 后，Iteration 5 在其他四个 OOD benchmark 上的平均值（65.56）仍然超过 Seed（63.76）和 Codex（64.97），说明结果不依赖单一 benchmark。（3）第二阶段持续训练有效：Claw-Eval 从 66.17 提升到 68.06，超过 Codex，说明在 OOD benchmark 上继续演化（持续训练）能进一步适配。（4）harness 代码量快速膨胀：从 Seed 的 49 行增长到 Iteration 1 的 279 行、Iteration 2 的 343 行、Iteration 3 的 394 行、Iteration 4 的 414 行、Iteration 5 的 430 行，前几次迭代增长最快，之后趋于平缓。（5）出现的机制分五类：工具层（输出截断、命令时间限制、图像查看）、上下文管理（历史压缩、进度文件）、结束任务检查（从完成检查到全新上下文中的独立审查）、交互协议（决定对话何时结束）、任务特定规则（集合操作、截止时间、搜索停止条件）；第二阶段新增视觉检查预算和交付截止时间。（6）失败案例与修复：某个视频变速任务 M097 上 Iteration 5 请求了某些内容（推测与视频帧/速度处理相关）；34 个任务在早期迭代超时，Iteration 7 不再超时，多模态任务分数随之上升；Claw-Eval 上存在图像使用额度，额度用完后最多还允许 8 次模型调用（这可能是对多模态任务资源约束的 harness 级适配）。（7）OfficeQA Pro 上出现了针对任务特定规则的演化（例如从 1959 年开始计数的年份列、获得 0.88525 分数并通过评分），说明 harness 学到了 benchmark 特定的数值推理技巧——这同时提示任务特定规则可能包含过拟合风险，但作者用 OOD 评估来缓解。（8）评估审计机制：评估文件与参考答案在 agent 完成后才放入环境，事后扫描所有轨迹的命令与输出，说明作者把评估公平性作为实验设计的核心关切。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：（1）把演化过程更完整地类比为深度学习训练：例如把 sub-batch 演化当作 mini-batch、评价分数当作 loss、引入更系统的 credit assignment 机制，甚至研究演化中的过拟合、早停、正则化等训练概念。（2）扩大演化任务的领域覆盖和数量，研究任务多样性、任务数量与 OOD 泛化增益之间的 scaling 关系。（3）研究持续训练阶段的灾难性遗忘问题：在 Claw-Eval 上继续演化后，之前在 ID benchmark 上的收益是否保留；多阶段演化之间的迁移与遗忘需要系统测量。（4）把演化出的机制（输出截断、历史压缩、独立审查、进度文件）迁移到其他模型或固定 harness 上是否有收益，机制的可移植性值得研究。（5）演化效率问题：每个批次需要大量运行轨迹，如何减少模型调用成本、更智能地选择需要反馈的任务或轨迹（类似主动学习/课程学习）。（6）安全与评估公平性：proposer 直接编辑代码的框架下，如何防止 harness 通过修改评分器或评估环境作弊；本文用工程检查和事后审计缓解，但更 robust 的防护机制（如评估环境形式化隔离）值得探索。（7）把 self-evolving harness 与其他自我改进形式（如 self-reflection、self-training、测试时计算）结合，形成更完整的递归自我改进智能体。（8）用更弱的模型作为 solver 时演化是否仍然有效，以及演化出的 harness 在模型升级后是否继续保持有效（harness 与模型的 co-adaptation 问题）。（9）harness 代码膨胀问题：430 行以后如何控制复杂度、可维护性与可解释性，是否需要代码压缩或模块化机制。（10）探索 harness 演化与多智能体协作、工具学习等方向的交叉。

Q7: 总结一下论文的主要内容

论文围绕 self-evolving harness 提出一个接近递归自我改进的框架，核心主张是：同一个冻结模型应该在同一份 harness 上先作为 solver 求解任务、再作为 proposer 读取完整运行记录并直接编辑 harness，从而最小化人类先验、最大化模型修改自身控制代码的自由。论文从三个角度重新审视现有框架：理想自我改进框架应该具备什么结构；如何让改进泛化到未见任务；演化过程能否被看作模型训练。针对第一个问题，作者与 Meta-Harness（Claude Opus 4.6 驱动的 Claude Code 作为 proposer）和 HarnessBank（Claude Opus 4.8 改进 Qwen3.6-27B 的 harness）等现有方法区分开：这些方法的 proposer 由更强模型驱动、运行在固定人工 harness 上、外层循环还常有人工设计的轨迹整理步骤；本文的 proposer 与 solver 使用同一冻结模型、同一 harness 版本，外层程序只分发任务和打包运行记录，没有专门 agent 组织轨迹。针对第二个问题，作者让每个演化批次从五个领域差异很大的 benchmark（Terminal-Bench 2.1、GAIA2、OfficeQA Pro、τ3-Bench banking、SWE-Bench Pro）中抽取任务，并把任务分成训练任务和留出任务严格分离，再用五个从未参与演化的 OOD benchmark（SWE-bench Verified、DeepSWE、BrowseComp-Plus、APEX-Agents、Claw-Eval）评估泛化；更新任务明确告诉 proposer 修改后的 harness 会在未见任务和完全不同 benchmark 上评估，因此改进必须泛化。针对第三个问题，作者把演化类比为两阶段深度学习训练：第一阶段是多任务预训练，第二阶段是在 Claw-Eval 上的持续训练；期间使用评价分数作为 loss、子批次演化等概念。技术上，harness 状态 Ht=(Ct,Nt) 包括全部源码和演化笔记；proposer 可直接增删文件、引入任何组件，修改后必须通过工程检查；评估公平性通过延迟放置评估文件和事后审计轨迹来保证。实验上，从 49 行种子 harness 出发经 7 次迭代，ID benchmark 平均提升 4.48 分、OOD benchmark 平均提升 12.64 分，ID 上超过 Codex、OOD 上追平 Codex；第二阶段在 Claw-Eval 上继续演化把分数从 66.17 提到 68.06，超过 Codex。机制分析显示演化中出现了五类组件：工具层（输出截断、命令时间限制、图像查看）、上下文管理（历史压缩、进度文件）、结束任务检查（独立审查）、交互协议、任务特定规则；第二阶段加入视觉检查预算和交付截止时间。论文还报告了具体案例：视频变速任务 M097 在 Iteration 5 的行为变化、34 个超时任务在 Iteration 7 不再超时、OfficeQA Pro 上从 1959 年开始计数的年份列技巧等。总体而言，论文展示了一个以最小人类先验、跨领域多任务演化为特征的 harness 自我改进框架，并用严格的 ID/OOD 分离评估证明其泛化性，同时提供了丰富的机制演化分析。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）直接重合：论文主题是语言模型智能体的自我改进框架，核心对象是 agent 的外围控制代码（harness）。

## 基本信息

- 作者：Qiankai Xu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38372v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（58 个 chunk 中的 Introduction、Method、Results、Discussion 命中片段），并结合 heuristic_draft 与论文元数据进行了补全和校准；证据未覆盖的细节已明确标注为推测或需回原文确认。
