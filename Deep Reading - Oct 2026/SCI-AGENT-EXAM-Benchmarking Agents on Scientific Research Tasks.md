---
user_id: "cheng tan"
paper_id: 17962
arxiv_id: "2610.32441"
title: "SCI-AGENT-EXAM: Benchmarking Agents on Scientific Research Tasks"
publish_date: "2026-10-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.32441.pdf"
pdf_url: "https://arxiv.org/pdf/2610.32441"
abs_url: "https://arxiv.org/abs/2610.32441"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-26T12:16:32"
---
# SCI-AGENT-EXAM: Benchmarking Agents on Scientific Research Tasks

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：scientific agents · benchmark · workflow taxonomy · llm-as-a-judge

## 一句话总结

SCI-AGENT-EXAM 是一个面向真实科研工作流的智能体基准：它用 120 个覆盖 11 个科学领域、7 类科研任务的专家任务，同时要求可执行产物与文本产物，并用程序化验证、专家校准的 LLM judge 与执行轨迹分析来评估 10 个前沿智能体的能力、效率与稳定性。

## 摘要

> Large language model (LLM) agents are increasingly capable of supporting scientific workflows, but their capabilities across diverse research activities remain difficult to assess systematically. Existing benchmarks often focus on selected scientific capabilities or task types, limiting a comprehensive characterization of scientific agents' strengths and weaknesses in realistic research workflows. We introduce SCI-AGENT-EXAM, a workflow-oriented benchmark comprising 120 expert-curated tasks across 11 scientific fields and 7 research task types defined by the Scientific Workflow Task Taxonomy (SWTT). Each task requires both executable artifacts (e.g., code and scripts) and textual artifacts (e.g., reports and explanations). To evaluate the heterogeneous task outputs, we develop a hybrid evaluation protocol combining deterministic programmatic verification for executable artifacts with a calibrated, expert-validated LLM-based judge for textual artifacts. Beyond outcome assessment, we analyze progress and solution stability along execution trajectories to characterize how agents reach their final results. Experiments with 10 frontier LLM agents reveal substantial performance gaps: GPT-6 Astra achieves the highest overall score of 80.9, yet only 26.0% of its runs exceed the 0.95 success threshold. Further analyses uncover distinct capability profiles across scientific task types and substantial variation in execution efficiency and stability. Our code and data are available at https://github.com/SKYLENAGE-AI/Sci-Agent-Exam.

Q1: 这篇论文试图解决什么问题？

1）核心问题界定：论文试图解决的关键问题是“如何在接近真实科研工作流的条件下，系统、可复现、可解释地测量 LLM 智能体的科研能力”，并非某个科学任务的单点准确率。这要求基准同时覆盖多学科、多阶段任务，并要求智能体交付可执行产物与可解释文本，而非只回答选择题或写代码。2）现有评测的碎片化：摘要指出已有基准往往只覆盖选定的科学能力或任务类型，导致无法全面刻画智能体在真实工作流中的强弱。合理推断，这会造成三类盲区：科学知识问答强不等于会做数据分析；代码生成强不等于能保证科学正确性；单步工具调用强不等于能维持长程一致性。3）异构产物的评测困难：科研产出既有代码、脚本、表格、图等可执行或可检查产物，也有报告、解释、论证等开放式文本。单一精确匹配或单一 LLM judge 都不够：前者对文本过严，后者对可执行结果不够可靠。4）只看结果不够：同样的最终分数可能来自一次成功、反复试错、早期达到峰值后回退，或依赖外部提示。论文因此强调沿执行轨迹分析进度与解稳定性，试图区分“会做”与“稳定地做对”。5）真实工作流的约束：任务需要指定科学目标、执行环境、可见资源，并要求正确传播依赖、schema、单位和中间结果；这意味着评测不仅是生成，还包括环境理解、工具使用、数据管理和验证。6）抗捷径与构念效度：检索片段出现 Trap Injection、在修改数据上重跑 run_analysis.py 的 hold_for_redesign 等设计，说明论文关心智能体是否真正执行分析而非复制答案或记忆模式；具体机制需回原文确认。7）隐含假设：基准假设科研工作流可以被分解为 7 类任务并由专家在受控环境中复现；假设程序化检查与专家校准的 judge 能覆盖主要质量维度；假设 10 个前沿智能体的表现能代表当前能力边界。这些假设本身需要构念效度与外部效度检验。8）因此，论文要回答的实证问题是：前沿智能体在跨领域、双产物、长程科研任务上究竟强在哪、弱在哪，成绩是否稳定，执行效率如何，以及评测结论是否受执行框架影响。

Q2: 有哪些相关研究？

由于提供的检索片段没有完整 Related Work 段落，以下按摘要与引言线索归纳论文的相关研究定位，具体引用需回原文核对。1）科学知识与推理基准：一类工作用问答、多跳推理、图表理解或学科考试测量模型的科学知识；优点是可控、易自动评分，缺点是通常不要求真实执行代码或交付可复现 artifact，难以覆盖数据分析和实验设计。2）代码与数据科学智能体基准：另一类工作以可执行程序为中心，评估代码生成、notebook 分析、数据驱动发现或在计算环境中完成既定目标；这类基准更接近“做出来”，但往往局限于单一任务族，缺少跨学科工作流和文本论证维度。3）交互式环境与工具使用基准：还有研究在交互环境中评估搜索、浏览、软件操作和多步工具调用；它们强调 agentic 行为，但科学领域特有的依赖、单位、schema、统计推断与领域验证未必被系统覆盖。4）AI-for-Science 工作流系统：已有系统尝试自动做文献调研、假设生成、实验设计、数据分析和报告撰写；这些工作多聚焦系统能力展示，评测常是案例式的，缺少统一任务分类和横向可比协议。5）混合评测与 LLM-as-judge：确定性程序化验证适合代码和结构化输出，rubric-based LLM judge 适合开放式文本；相关研究关注 judge 与专家一致性、位置或长度偏差和校准。论文把两者结合，并用专家验证的 judge 处理文本产物。6）过程级或轨迹评测：传统基准多看最终答案，近年工作开始记录步骤、工具调用、成本和失败恢复；论文的差异化在于把 progress、stability、efficiency 与最终质量并列，针对科研工作流的长程性。7）差异化定位：SCI-AGENT-EXAM 的新意在于用 SWTT 把 11 个领域、7 类任务、双产物和轨迹分析组织成一个工作流导向的评测构念，而不只是扩大单一数据集规模；它要与上述各类基准竞争的是“覆盖面、真实性与可复现性”的三角权衡。

Q3: 论文如何解决这个问题？

1）SWTT 任务分类法：论文用 Scientific Workflow Task Taxonomy 把科研工作流拆成 7 类研究任务，作为跨领域可比的上层坐标；11 个科学领域提供领域异质性。摘要没有逐项列出 7 类名称，片段可见至少涉及 Code Generation、Hypothesis、End-to-End Analysis，以及比较或验证类任务，完整定义需查原文。2）任务实例：120 个任务由专家策划，每个任务规定科学目标、执行环境与可见资源；片段还提到任务要求正确传播依赖、schema、单位和中间结果，说明它要求带环境约束的 artifact 生产，并不仅限于纯文本问答。3）双产物要求：每个任务同时要求可执行产物（代码、脚本等）和文本产物（报告、解释等）。这使评分必须覆盖“做得对不对”和“讲得清不清、科学论证是否成立”两个通道。4）混合评测：对可执行产物用确定性程序化验证，例如运行脚本、检查输出、测试接口或结构化结果；对文本产物用校准过的、经专家验证的 LLM judge，按 rubric 打分。论文强调 calibrated 与 expert-validated，意味着 judge 不是直接拿来用，而是经过某种对齐或验证流程；具体校准集、一致性指标和权重需回原文。5）轨迹分析：除最终分数外，记录执行过程中的进度与解稳定性，刻画智能体是早期达到峰值、持续改进、停滞还是回退；这为“同样分数、不同过程”提供诊断。6）抗捷径设计：片段出现 Trap Injection 与在修改数据上重跑 run_analysis.py 的 hold_for_redesign 变体，合理推断用于检验智能体是否真正读取数据、执行分析并传播修改，而非复制答案键或依赖记忆；具体任务数量和判定逻辑需核对附录。7）评测对象：10 个前沿 LLM 智能体在统一任务上运行，比较总分、阈值成功率、任务类型或领域剖面、效率与稳定性；片段还显示不同 execution harness（codex 与 claude-code）会改变成绩，说明 scaffold 是方法的一部分，不能当作无关实现细节。

Q4: 论文做了哪些实验？

1）主实验：10 个 frontier LLM agents 在 120 个任务上评测，覆盖 11 个领域和 7 类任务；每个 agent 进行多次运行（摘要提到 runs），报告 overall score 与超过 0.95 成功阈值的运行比例。2）分层分析：按任务类型和科学领域拆分，比较不同智能体的 capability profile，而不是只给一个总排名。3）双通道评分：分别看可执行产物的程序化验证结果与文本产物的 judge 分数，分析两类信号是否一致。4）轨迹分析：沿执行轨迹计算 progress、early completion、dip、recovery、execution efficiency、solution stability 等指标，观察最终质量与过程质量的关系。5）评测协议诊断：对 LLM judge 做校准或专家验证；片段提到 paired clean-task sets、Trap Injection、在修改数据上 rerun run_analysis.py 的 hold_for_redesign，以及 answer keys 与 margin 检查，说明论文做了抗捷径和评分可靠性方面的对照。6）执行框架比较：片段显示同一模型在 codex 与 claude-code 下分数不同，GPT-5.6 Sol 在 codex 下高 14.3 分，说明论文比较了 harness 对结论的影响。7）失败案例分析：附录 H 被用来展示两类相反失败模式；片段还讨论 programmatic regressions 与 scientific correctness failures 的可恢复性差异。8）可复现性：代码与数据公开在 GitHub，便于复跑任务、检查评分与扩展基线。

Q5: 发现了什么实验现象？

1）高分不等于高可靠：摘要明确 GPT-6 Astra 总分最高 80.9，但只有 26.0% 的运行超过 0.95 成功阈值。这说明排行榜上的平均分可能被部分高分运行拉高，而“稳定达到接近满分”仍是稀缺能力；对科研场景，后者往往比均值更重要。2）能力剖面分化：不同任务类型和领域上，智能体强弱并不一致，呈现 distinct capability profiles。片段把 End-to-End Analysis 与最高的 early-completion rate（36.2%）联系起来，同时提到最高 dip，但因断行严重，具体归属与数值需回表核对；可确定的是，论文用轨迹指标揭示了“早期看起来很好、后续不稳定”的现象。3）执行效率与稳定性差异显著：摘要明确存在 substantial variation in execution efficiency and stability。合理推断，轨迹并非单调改进，常见模式包括早熟、停滞、回退后恢复、以及依赖反复试错；同样的最终分数可能对应完全不同的过程质量。4）错误可恢复性不对称：片段指出 programmatic regressions 通常比 scientific correctness failures 更容易恢复。也就是说，智能体较擅长修语法或运行错误，但一旦科学假设、统计口径、单位或因果解释错了，后续很难通过局部修补救回；这解释了为什么“代码能跑”不等于“科研做对”。片段还以 DeepSeek-V4-Pro 的“程序化满分但……”作为相反失败模式，提示单看可执行测试会高估科学正确性，完整结论需查 Appendix H。5）执行框架会改变结论：片段显示某模型在 codex 下比 claude-code 低若干分，而 GPT-5.6 Sol 相反，在 codex 下高 14.3 分。这说明模型排名对 scaffold、工具接口、预算和运行策略敏感；基准若只报告单一 harness，可能把“框架适配”误读为“模型能力”。6）抗捷径设计揭示真实执行的重要性：Trap Injection 与 modified-data rerun 的设计用于区分真正读取数据、执行分析并传播依赖的智能体，与复制答案键或记忆任务模式的智能体；片段提到检查 answer keys 与 selected-to-runner-up margin，但完整结果不充分。7）指标之间存在张力：总分、0.95 阈值成功率、early completion、efficiency、stability 可能给出不同排序；片段还出现 51.7% 与 47.8% 的高总体率以及 20–26% 的模型区间，但因文本断行无法可靠判断其具体指标与对象，建议回原表核对。8）总体现象：当前前沿智能体已能在部分科研任务上产出可用 artifact，但跨领域稳健性、长程一致性、科学正确性和执行效率仍是主要瓶颈；基准的价值在于把这些瓶颈从单一分数中拆出来。

Q6: 有什么可以进一步探索的点？

1）任务分类法的构念效度：对 SWTT 的 7 类任务做专家一致性、难度分层和因子分析，检验它们是否真的对应可分离的能力维度；扩展更多任务类型，例如文献综述、实验设计、同行评审、科研伦理等。2）动态与抗污染基准：用程序化任务生成、隐藏测试、修改数据、时间切分和红队陷阱构造变体，降低训练数据记忆；研究 trap 的难度自适应与可解释性。3）评测协议：系统比较 programmatic checks 与 LLM judge 的权重、置信区间和失效模式；报告 judge 与专家的一致性、跨领域校准、位置或长度或风格偏差；对开放式文本引入对抗评审和多评审聚合。4）轨迹指标：把 progress、early completion、dip、recovery、efficiency、stability 操作化为可复现定义，研究采样频率、预算和工具延迟对曲线的影响；用分层模型分解任务难度、模型能力、harness 和运行噪声。5）智能体能力：重点研究科学正确性错误的自检与恢复、假设检验、统计推断、单位或 schema 传播、长程记忆和计划重规划；比较单 agent 与多 agent 协作、human-in-the-loop 的收益。6）执行框架标准化：把 scaffold、工具集、token 或时间预算、API 非确定性纳入基准协议，报告模型与 harness 的交互效应，而不是只给单一榜单。7）向真实科研延伸：多模态图表或图像、仪器接口、湿实验闭环、长期项目协作、伦理与安全审查；这些都超出当前以代码或脚本为主的评测范围。8）成本感知评估：除准确率外报告 token、工具调用、wall-clock、失败重试成本，研究“更贵是否更稳”。9）开放科学：维护可复现环境、版本化任务、公开失败轨迹和 leaderboard，鼓励提交评分器与 judge 校准报告。10）跨领域迁移：分析在物理、化学、生物、社会科学之间能力是否迁移，识别领域特异性瓶颈。

Q7: 总结一下论文的主要内容

论文的主线可以概括为“从碎片化能力评测转向工作流评测”。作者首先指出，LLM 智能体已能参与文献调研、代码编写、数据分析、假设生成和报告撰写，但现有基准通常只覆盖选定的科学能力或任务类型，无法系统回答“智能体在真实科研工作流中到底强在哪、弱在哪、是否稳定”。为此，论文提出 SCI-AGENT-EXAM：一个 workflow-oriented benchmark，包含 120 个专家策划任务，覆盖 11 个科学领域和 Scientific Workflow Task Taxonomy（SWTT）定义的 7 类研究任务。每个任务都同时要求可执行产物（代码、脚本等）和文本产物（报告、解释等），并把科学目标、执行环境、可见资源以及依赖、schema、单位和中间结果的正确传播纳入任务设定。评测上，论文没有用单一精确匹配，而是设计 hybrid protocol：可执行产物用确定性程序化验证，文本产物用 calibrated、expert-validated 的 LLM-based judge；前者保证可运行、可检查，后者处理开放式论证。论文进一步把评测从最终结果扩展到执行过程，沿 trajectory 分析进度与解稳定性，并观察 early completion、dip、recovery、efficiency 等模式，从而区分“偶然做对”和“稳定做对”。实验对 10 个前沿 LLM agents 进行多轮运行。主结果中，GPT-6 Astra 总分 80.9 最高，但仅 26.0% 的运行超过 0.95 成功阈值；不同任务类型或领域的能力剖面不同，执行效率与稳定性差异显著。检索片段还显示 harness 敏感：同一模型在 codex 与 claude-code 下分数可差 14.3 分；程序化回归比科学正确性错误更易恢复；End-to-End Analysis 的 early-completion 为 36.2% 并与最高 dip 相关，但具体归属需核对原表。论文的贡献在于把“任务分类—双产物—混合评分—轨迹诊断”组织成一套可复用的评测范式，而不只是增加一个数据集；它揭示了平均分与高阈值成功率、可执行正确与科学正确、模型能力与 scaffold 适配之间的张力。边界也很明显：120 个任务相对科研空间仍是小样本；专家策划与 judge 校准有主观性和成本；程序化测试可能脆性；模型、harness 与预算存在混淆；0.95 阈值是人为设定；以代码或脚本为主的任务对湿实验和长期协作覆盖有限；需要更多 runs、统计检验和外部效度验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接重合：论文系统评估 10 个前沿 LLM 智能体在长程、多步、工具使用任务上的表现，并分析轨迹、效率与稳定性，适合关注 agent 评测与失败模式的研究者。

## 基本信息

- 作者：Tianyun Zhong, Wei Wang, Jialin Li, Jianhong Tu, Liyao Li, Chuqiao Kuang, Tianshi Zheng, Songchi Zhou, Weiqi Zhai, Qile Jin, Jiajia Li, Jiasheng Zheng, Boxi Cao, Yaojie Lu, Hongyu Lin, Shiwei Ye, Bing Zhao, Hu Wei, Lin Qu, Xianpei Han
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-10-26
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.32441`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考 PDF 语义检索片段并与摘要交叉核对；检索片段存在明显断行、串栏和表格残片，heuristic_draft 多处语义不完整，凡无法由干净证据支持的内容均标注为合理推断或需回原文核对。
