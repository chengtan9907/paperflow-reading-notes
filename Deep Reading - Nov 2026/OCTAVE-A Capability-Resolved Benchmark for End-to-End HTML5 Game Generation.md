---
user_id: "cheng tan"
paper_id: 21782
arxiv_id: "2611.17681"
title: "OCTAVE: A Capability-Resolved Benchmark for End-to-End HTML5 Game Generation"
publish_date: "2026-11-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.17681.pdf"
pdf_url: "https://arxiv.org/pdf/2611.17681"
abs_url: "https://arxiv.org/abs/2611.17681"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-16T23:13:47"
---
# OCTAVE: A Capability-Resolved Benchmark for End-to-End HTML5 Game Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：html5 game generation · benchmark · agentic coding · capability dimensions

## 一句话总结

OCTAVE 是一个面向端到端 HTML5 游戏生成的能力分辨基准：它用 180 个任务、1,800 条需求与八个能力维度，把单一游戏级总分拆成 Completeness/Richness 及需求级证据，并在 11 个模型–harness 组合上揭示项目交付与需求实现的双重缺口。

## 摘要

> 完整游戏生成要求智能体在长周期开发中同时协调规则、交互、状态与呈现，因此只用游戏级聚合分数很难判断智能体究竟实现了哪些开发能力、哪些需求仍未满足。OCTAVE 以能力覆盖来组织任务，并通过需求级证据诊断实现差异。它从真实玩家请求、商业机制重建与人类专家设计三类来源构建 180 个任务、1,800 条评估需求，覆盖 PC Web、Mobile Web、2D 与 3D。需求被区分为评估公开明确核心实现的 Completeness 与评估隐藏深层系统关系的 Richness，随后用静态源码判定分析不同任务条件下的实现表现。对 11 个模型–harness 组合的全任务评估得到 Completeness 33.2%、Richness 32.5%，19.9% 的生成记录被标记为静态 gate 失败或缺失产物；排除同模型自评后，最佳组合在公开与隐藏需求上的通过率仍低于 50%。进一步分析显示，Richness 并非在所有任务条件下都比 Completeness 更难；系统在 Core Rules 与 Numerical Design 上相对较好，而 AI/NPC 与 Feedback and Game Feel 是更突出的能力缺口；需求难度标签只能提供粗粒度分层，且与实际通过率仅呈弱负相关。

Q1: 这篇论文试图解决什么问题？

这篇论文要解决的是“如何可诊断地评测端到端 HTML5 游戏生成”这一基准构造问题，而不是单纯提升某个游戏生成模型的分数。完整游戏生成要求智能体在长周期开发中同时协调游戏规则、交互、状态转换、渲染与场景呈现，涉及跨文件代码、运行时集成和多种资源；这类任务的失败可能来自需求理解、代码实现、项目交付、资源缺失或运行时逻辑，而传统单一游戏级聚合分数会把它们压缩成一个数，无法回答“哪些能力已经具备、哪些需求没有满足”。论文指出，仅报告 game-level aggregate score 会掩盖三类异质性：其一是项目交付失败与需求实现失败的异质性，例如静态 gate 失败或缺失产物会让分数归零，但这并不等同于所有需求都不会写；其二是能力维度的异质性，Core Rules、AI/NPC、Numerical Design、Feedback and Game Feel 等维度的难度和失败模式不同，平均分无法定位短板；其三是任务条件的异质性，真实玩家请求、商业机制重建、人类专家设计，以及 PC Web/Mobile Web、2D/3D 的通过率可能差异很大。为此，OCTAVE 把评测单位下沉到 requirement level，用 180 个任务、1,800 条评估需求、八个能力维度组织任务，并把需求区分为评估公开明确核心实现的 Completeness 与评估隐藏深层系统关系的 Richness，再以静态源码判定分析不同任务条件下的实现表现。论文进一步关心：当前系统整体通过率如何；Completeness 与 Richness 是否系统性不同；不同任务来源、平台和维度下表现如何；需求难度标签能否预测实际通过率；静态判定与人工复核是否一致。其隐含假设包括：复杂游戏需求可以被分解为可静态检查的原子需求；三类任务来源对“完整游戏生成”有代表性；需求通过率可以聚合为有意义的能力信号；隐藏需求确实测到了更深的系统关系而非更难的模式匹配。这些假设若被削弱，基准的结论边界也会收缩。

Q2: 有哪些相关研究？

论文的相关研究可以沿着三条线组织。第一条是 LLM 编程能力评测从函数级走向长周期软件工程。HumanEval 与 MBPP 以独立函数题和单元测试为主，抽象掉项目依赖；RepoBench 把评测推进到真实代码仓库中的跨文件补全；SWE-bench 用真实 GitHub Issue 和仓库测试评测智能体修改、修复既有项目的能力，SWE-agent、OpenHands 则代表面向这类任务的 agent 系统。第二条是从零构建完整软件的基准。Commit0 要求根据自然语言规格实现库；PaperBench 要求智能体复现论文的研究贡献，包括实现代码库并执行实验；NL2Repo-Bench 更进一步，要求从单份自然语言文档构建完整软件仓库、不提供项目脚手架。这条谱系逐步扩大任务所需的系统集成范围，从独立函数、跨文件补全、仓库修复，走到完整软件系统。第三条是游戏生成与游戏 AI 评测。论文把游戏开发视为更困难的设置，因为一个游戏同时涉及 gameplay logic、状态转换、用户交互、渲染和场景组织；检索片段还显示，相关工作涉及从高层设计直接生成、模板复用、调试、跨文件一致性、场景连接和运行时集成等游戏 AI 研究脉络。与这些工作相比，OCTAVE 的差异不在于提出新的游戏生成模型，而在于把“完整 HTML5 游戏生成”做成一个 capability-resolved benchmark：它不是只给一个游戏级分数，而是用八维能力标签、公开/隐藏需求、Completeness/Richness 和需求级静态判定来诊断实现差异；任务来源覆盖真实玩家请求、商业机制重建和专家设计，部署条件覆盖 PC Web、Mobile Web、2D 与 3D。评测方法上，论文还与静态代码审查、需求级 rubric、同模型自评偏差和有限人工复核等议题相关。需要说明的是，提供的检索片段只完整展开了编程基准谱系，游戏生成基准的具体对照名称和数量在片段中不完整，相关段落应回原文 Related Work 核对。

Q3: 论文如何解决这个问题？

OCTAVE 的方法可以拆成四层。第一层是任务集构造：共 180 个任务，来自三类来源——真实玩家请求、商业机制重建、人类专家设计；任务覆盖 PC Web、Mobile Web、2D 和 3D，并在构造阶段区分 PC Web 与 Mobile Web。真实玩家请求部分包含 extraction、cleaning 和 provenance 流程，把原始玩家请求转成可追踪的候选任务；商业机制重建从已有商业游戏的机制出发；专家设计则由人类专家编写。每个任务最终配有一份 Game Specification，智能体从空工作区开始，不获得任何预先存在的游戏逻辑或任务特定代码。第二层是需求体系：共 1,800 条 Evaluation Requirements，按八个 Capability Dimensions 组织并分配到任务；可确认的维度包括 C1 Core Rules、C2 AI/NPC、C3 Numerical Design 以及 Feedback and Game Feel，完整 C1–C8 名称需回原文核对。需求分为 Completeness 与 Richness：前者评估公开说明的核心实现，后者评估隐藏的、更深的系统关系；对应地也有 public requirements 与 hidden requirements 的区分。第三层是生成与评测协议：生成阶段只给最终 Game Specification，智能体自主开发；评测阶段检查最终 artifact，用 static source-code judgments 对照预构造的 Evaluation Requirements，并记录 static gate failure 或 missing artifact。第四层是实验分析：用 11 个 model–harness 组合跑全任务，按任务来源、能力维度、平台/2D-3D 条件分层统计 Completeness、Richness 和 gate-failure-or-missing-artifact rate；用 Spearman 相关分析 gate failure 与得分、需求难度与通过率的关系；排除 same-model self-evaluation 后重算最佳组合；再用有限样本的人工 cross-check 校验自动判定。方法设计的关键取舍是：以静态源码判定换取可扩展、可复现的低成本评测，但牺牲了对运行时行为、交互手感和实际可玩性的直接测量；以需求级分解换取诊断性，但依赖需求编写质量和静态可判定性。片段还提到 450 条 public requirements 与 300 条 hidden requirements，以及 full replacement、same-source revision、retention without change 的分层表；这些数字与 1,800 条总需求的口径关系在提供的证据中并不完整，不能直接等同，需回原文核对。

Q4: 论文做了哪些实验？

实验是 benchmark 评测而非模型训练。论文评估 11 个 model–harness 组合，agent harness 为 Claude Code 或 Codex CLI；模型包括 DeepSeek-V4.1-Flash、DeepSeek-V4-Flash-0731、DeepSeek-V4-Pro、Fable 5、Fable 5.1、Opus 5、GLM-5.2、GLM-5.3、GPT-5.5、GPT-5.6-sol 和 GPT-6-astra。所有组合使用同一任务集，并在各 track 内使用对应的 generation protocol；不同 track 的 evaluation-call 差异在 Appendix E.1，reasoning-effort、runtime 配置和预算在 Appendix D.1–D.3。生成阶段中，每个任务从独立空工作区开始，工作区没有预先存在的游戏逻辑或任务特定代码；智能体只收到最终 Game Specification，然后自主开发。生成结束后，评测系统检查最终 artifact，按照预构造的 Evaluation Requirements 评估实现情况。评价指标包括 Completeness（公开明确核心实现的通过率）、Richness（隐藏深层系统关系的通过率）、static gate failure 或 missing artifact 比例；因为模型和 harness 同时变化，论文在 combination 层面报告结果，而不是把分数归因于单一模型。分析维度包括：全任务总体；按任务来源（真实玩家请求、商业机制重建、专家设计）；按八个能力维度；按 PC Web/Mobile Web 与 2D/3D 条件；gate failure 与 Completeness/Richness 的 Spearman 相关；需求难度分层与实际通过率的相关；排除 same-model self-evaluation 后的最佳组合表现；以及有限样本的人类 cross-check。全任务共 11×180=1,980 条 generation records，表中报告 Overall 19.9% gate-failure-or-missing-artifact、33.2% Completeness、32.5% Richness。片段还显示某些条件行有 180 条记录、13.3% gate failure、57.1% Completeness、47.6% Richness，但该行对应的具体条件在截断文本中不完整，不能安全地映射到某个模型或任务来源。

Q5: 发现了什么实验现象？

（1）整体通过率低且未饱和。11 个组合的全任务平均 Completeness 33.2%、Richness 32.5%，说明当前系统在完整游戏生成上仍有大量需求未实现；排除同模型自评后，最佳组合在公开与隐藏需求上的通过率仍低于 50%，这不是单一评分口径造成的虚高。（2）项目交付是独立瓶颈。19.9% 的记录为 static gate failure 或 missing artifact，且 gate-failure rate 与 Completeness、Richness 呈强负相关（Spearman ρ≈−0.70 与 −0.72）。这意味着低分不只是“需求写不对”，还可能是“最终产物没有通过静态 gate 或缺失 artifact”；GPT-5.5 被作者称为例外，说明这种关系并非机械决定，具体机制需看原文。（3）Completeness 与 Richness 接近，但并非固定难度阶梯。总体两者几乎持平，且论文明确指出 Richness 不是在所有任务条件下都比 Completeness 更难；这与“隐藏需求一定更难”的直觉相反。合理推断是：隐藏需求可能更偏结构性、可用静态模式检查，而公开需求可能包含更多显式功能与集成细节，二者难度取决于任务条件与需求写法。（4）任务来源影响很大。真实玩家请求的 Completeness/Richness 为 28.4%/27.8%，论文还报告另一类来源达到 53.1%/47.4%，片段截断处未完整显示来源名称；按上下文推测可能为商业机制重建，但需回原文核对。真实玩家请求更杂乱、欠规范、跨功能，商业机制重建更接近有模板的机制复现，因此总体分数对任务来源构成敏感。（5）能力维度呈明显不均衡。系统在 Core Rules 与 Numerical Design 上相对较好，在 AI/NPC 与 Feedback and Game Feel 上缺口更突出；这说明“会写规则和数值”不等于“会做 autonomous behavior、状态机、反馈、打击感、动画时序和交互手感”。（6）需求难度标签的校准很弱。需求难度与实际通过率的 pooled Spearman 仅 −0.1201，且 pooled decline 对子集构成敏感、within-subset 关联很弱；难度分层只能算粗粒度组织工具，不能当作通过率的可靠预测器。（7）静态审查能发现一类运行时逻辑缺陷，但不等于动态测试。片段中的 3D puzzle game 例子显示，静态审查通过了 5/6 条 Completeness Requirements，但代码存在长帧间隔丢弃 elapsed time、使模拟进度依赖渲染条件的问题；这既说明静态审查可以读出时序/状态 bug，也说明它可能漏掉实际运行中的表现。（8）人工复核覆盖有限。论文做了 human cross-check，但片段只显示 limited sample 且关键结论被截断，无法确认一致性数值、争议裁决和假阳性/假阴性比例；这是评测效度上最需要回原文核对的证据之一。

Q6: 有什么可以进一步探索的点？

（1）静态与动态混合评测：用浏览器自动化、执行 trace、状态快照、截图/视频、输入回放和性能指标补充静态源码判定，直接测量运行时行为、帧率、交互响应和可玩性；尤其要覆盖 Feedback and Game Feel 这类静态代码难以判断的维度。（2）扩大人工复核与评分者间信度：对需求级判定做分层抽样、双人标注、争议裁决，报告一致性统计、假阳性/假阴性，并分析静态判定在哪些维度最不可靠。（3）分离 model 与 harness：用 factorial design 让同一模型跨 harness、同一 harness 跨模型，控制 reasoning effort、token/时间预算、工具权限和重试策略；当前 11 个组合的排名不能干净归因于模型能力。（4）需求难度校准：用项目反应理论或经验难度、区分度、猜测参数替代粗粒度难度标签；报告需求级方差和置信区间，避免把 pooled 相关当作因果。（5）抗污染与动态隐藏需求：生成式私有 holdout、同源修订追踪、full replacement/same-source revision/retention 分层，防止公开需求被模式匹配；同时保证 hidden requirements 真的测深层关系而不是更偏的检查项。（6）针对能力缺口的训练与课程：围绕 AI/NPC、状态机、路径规划、数值平衡、反馈/游戏感、长周期项目管理构造数据与奖励；把 gate failure 作为独立的工程可靠性目标。（7）跨平台与 3D 扩展：Mobile Web 的输入、屏幕、性能约束，2D/3D 的场景图、渲染管线、资源加载和物理一致性都值得单独分层；当前片段未充分展示这些条件的完整结果。（8）向其它交互式软件迁移：把游戏替换为科学模拟器、实验控制界面、可视化分析工具或 XR 场景，检验 capability-resolved evaluation 能否用于 AI-for-science 的交互式产物生成；论文本身不涉及生物医学，但评测框架可迁移。（9）质量维度扩展：除了需求通过率，加入 fun、balance、novelty、accessibility、代码可维护性、安全性和资源效率，避免“需求都过了但游戏不好玩”。（10）基准治理：版本化任务、公开 leaderboard、成本报告、重复运行方差、模型快照时间戳和评测脚本可复现性，都是长期维护必须解决的问题。

Q7: 总结一下论文的主要内容

一、问题定位。OCTAVE 关注的是端到端 HTML5 游戏生成的评测方法。完整游戏生成不是单函数补全，而是长周期、多能力耦合的系统工程：智能体要同时处理游戏规则、交互、状态转换、渲染、场景和资源，任何一个环节失败都可能让最终游戏不可玩。现有编程基准从 HumanEval/MBPP 的函数级测试，走到 RepoBench 的仓库级补全、SWE-bench 的 issue 修复，再到 Commit0、PaperBench、NL2Repo-Bench 的从零构建完整软件/仓库，系统集成范围不断扩大；但游戏生成还有实时交互、渲染和游戏感等额外维度。论文指出，若只报告一个游戏级 aggregate score，就无法判断智能体实现了哪些能力、哪些需求未满足，也无法区分项目交付失败与需求实现失败。因此它提出 capability-resolved benchmark，把评测单位下沉到需求级。
二、基准构造。OCTAVE 包含 180 个任务、1,800 条 Evaluation Requirements，任务来自三类来源：真实玩家请求、商业机制重建、人类专家设计；覆盖 PC Web、Mobile Web、2D 和 3D。真实玩家请求经过 extraction、cleaning 和 provenance 流程，从原始玩家 prompt 转成可追踪候选任务；商业机制重建围绕已有商业游戏机制；专家设计由人类专家编写。每个任务有一份 Game Specification，智能体从空工作区开始，不获得预置游戏逻辑或任务特定代码。需求按八个 Capability Dimensions 组织，可确认的维度包括 C1 Core Rules、C2 AI/NPC、C3 Numerical Design 与 Feedback and Game Feel；完整维度名称需回原文核对。需求分为 Completeness 与 Richness：Completeness 评估公开说明的核心实现，Richness 评估隐藏的、更深的系统关系；相应也有 public/hidden requirements 的区分。
三、评测协议。生成阶段只给最终 Game Specification，智能体在独立空工作区自主开发；评测阶段检查最终 artifact，用 static source-code judgments 对照预构造需求，并记录 static gate failure 或 missing artifact。指标包括 Completeness、Richness 和 gate-failure-or-missing-artifact rate。实验覆盖 11 个 model–harness 组合，harness 为 Claude Code 或 Codex CLI，模型包括 DeepSeek-V4.1-Flash、DeepSeek-V4-Flash-0731、DeepSeek-V4-Pro、Fable 5、Fable 5.1、Opus 5、GLM-5.2、GLM-5.3、GPT-5.5、GPT-5.6-sol、GPT-6-astra；所有组合使用同一任务集，track 内使用对应生成协议，预算和 reasoning-effort 设置在附录。因为模型与 harness 同时变化，论文在 combination 层面报告结果。
四、主要结果。全任务 1,980 条记录（11×180）上，Completeness 33.2%、Richness 32.5%、gate-failure-or-missing-artifact 19.9%。排除同模型自评后，最佳组合在公开与隐藏需求上的通过率仍低于 50%。gate-failure rate 与 Completeness、Richness 强负相关（ρ≈−0.70、−0.72），说明项目交付失败是低分的重要来源；GPT-5.5 是例外，具体模式需看原文。任务来源差异明显：真实玩家请求 28.4%/27.8%，论文还报告另一类来源 53.1%/47.4%，片段截断处未完整显示来源名称，按上下文推测可能为商业机制重建，需回原文核对。能力维度上，Core Rules 与 Numerical Design 相对较好，AI/NPC 与 Feedback and Game Feel 缺口更突出。
五、分析发现。Richness 并非在所有任务条件下都比 Completeness 更难，二者总体接近且条件依赖；这挑战了“隐藏需求一定更难”的直觉。需求难度标签与实际通过率仅弱负相关（pooled ρ=−0.1201），且 pooled decline 对子集构成敏感、within-subset 关联弱，说明难度分层只是粗粒度工具。静态审查可以发现时序/状态类缺陷，例如 3D puzzle game 中长帧间隔丢弃 elapsed time、模拟进度依赖渲染条件，但它不能替代动态测试和人工 playtest。人工 cross-check 只覆盖有限样本，片段未给出完整一致性统计，评测效度仍需扩大审计。
六、贡献与边界。OCTAVE 的主要贡献是把完整游戏生成拆成能力覆盖与需求级实现，提供三源任务、八维标签、Completeness/Richness、静态 gate 和 11 组合实证；它的边界是静态判定、模型–harness 混杂、任务来源构成、HTML5 单域和有限人工复核。整体上，这篇论文更像一份“评测基础设施+诊断报告”，其价值在于让游戏生成 agent 的失败可定位，而不是宣称某个模型已经解决游戏生成。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：OCTAVE 评测的是长周期、空工作区、工具链驱动的自主编码智能体，而不是单轮代码补全；对研究 agent 规划、调试、跨文件集成和项目交付有参考价值。

## 基本信息

- 作者：Kehang Li, Xinlan Xu, Yulin Jin, Heng Cao, Xuchen Li, Houxian Ren, Zihao Ji, Qiang Luo, Yadong Mao, Qilin He, Jian Zhang, Jian Zhao, Zihao Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.SE
- 日期：2026-11-16
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.17681`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Results 与 Conclusion 片段，并在任务数量、需求计数和分层数值等存在歧义处显式标注了需回原文核对。
