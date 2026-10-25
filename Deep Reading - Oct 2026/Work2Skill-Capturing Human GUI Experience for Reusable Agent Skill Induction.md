---
user_id: "cheng tan"
paper_id: 15808
arxiv_id: "2610.12984"
title: "Work2Skill: Capturing Human GUI Experience for Reusable Agent Skill Induction"
publish_date: "2026-10-12"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.12984.pdf"
pdf_url: "https://arxiv.org/pdf/2610.12984"
abs_url: "https://arxiv.org/abs/2610.12984"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T13:13:00"
---
# Work2Skill: Capturing Human GUI Experience for Reusable Agent Skill Induction

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：computer-use agent · GUI skill induction · human demonstration · multimodal dataset

## 一句话总结

Work2Skill 提出一个端到端框架：用 WorkRecorder 采集人对桌面 GUI 操作的多模态对齐演示，构建 WorkDemo-350 数据集，再通过按需检索证据并迭代撰写/修订的智能体技能归纳方法，把人类工作流转化为可复用的文本技能，使四个执行模型在 MyPCBench Bounded Action 和 OSWorld-Verified 子集上均取得最高分和最高成功率。

## 摘要

> Computer-use agents can perform increasingly complex GUI tasks, yet unfamiliar software and workflows requiring human domain knowledge remain challenging. Human demonstrations convey procedural knowledge through the same GUI interaction space, but reuse requires interpreting actions in context and separating workflow rules from task-specific details. We thus introduce Work2Skill, a framework that captures human GUI experience for reusable agent skill induction. WorkRecorder provides the framework's infrastructure for collecting this experience, aligning screen recordings, mouse and keyboard events, and available accessibility-tree information. Using WorkRecorder, we construct WorkDemo-350, the first synchronized multimodal dataset of human desktop GUI demonstrations for reusable agent skill induction and downstream evaluation. Its 350 demonstrations are screened for completeness and task success. We then propose an agentic skill-induction method that inspects evidence on demand to produce reusable skills capturing workflow rules, step dependencies, and completion checks. When the same four execution models are equipped with skills induced by each method under shared execution settings, Work2Skill achieves the highest scores and success rates. Mean score improvements over the corresponding No Skill baseline are 4.67--10.54 points on MyPCBench Bounded Action and 3.11--7.50 points on a 286-task subset of OSWorld-Verified. Our code and data are available at https://github.com/china-qijizhifeng/Work2Skill.

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：计算机使用智能体虽然 GUI grounding 和规划能力在提升，但面对不熟悉的软件和隐含领域知识的工作流时仍然失败，因为用户请求（如“提交我的报销单”）中并没有写明应该用哪个内部门户、附哪些票据、提交前检查什么。这类程序性知识是用户或组织特有的，智能体无法从通用训练中获得。因此需要一种机制把人类 GUI 演示中的程序性知识转化为智能体可复用的技能。转化的难点在于：人类动作必须结合上下文理解（例如点击哪个单元格、输入什么值都依赖当时界面状态），并且要把工作流规则与任务特定细节分离（“这个报销流程怎么走” vs “这次报销的金额是 450”）。现有工作有几种路线：把演示转成专家指令和语义动作轨迹、多模态教程或可复用技能；OpenAI Record & Replay 和 Claude Record a skill 等用户侧工具也已出现，但演示采集往往依赖平台特性或缺少统一的多模态对齐数据，技能归纳也缺少按需、可修订的证据检查机制。

Q2: 有哪些相关研究？

相关工作背景包括：计算机使用智能体的进展（Qin et al., 2025; Agashe et al., 2025; Wang et al., 2025a），它们依赖 GUI grounding 和规划能力；智能体在需要领域知识的工作流上失败的现象（Zhang et al., 2026e; Jang et al., 2026）。数据收集、grounding、模型训练（Wang et al., 2025a; Nayak et al., 2025; Feizi et al., 2026）以及智能体效率评估（Abhyankar et al., 2026）都依赖演示数据；演示结构会影响任务执行（Xing et al., 2026）。程序性知识表示方面有专家指令（Li et al., 2025）、任务图（Yin et al., 2025）等。把演示转成可复用技能的工作包括 multimodal tutorials（Bai et al., 2026）、reusable skills（Yang et al., 2026a; Xiao et al., 2026）；OpenAI Record & Replay（OpenAI, 2026b）和 Claude Record a skill（Anthropic, 2026b）已把这类方法做成面向用户的工具。执行时选择性检查帧（Zhang et al., 2026b）也与本文的按需证据检查相关。论文的直接基线是 No Skill、ShowUI-Aloha 和 Demo2Tutorial。注意：这些引用的具体内容和数值在检索片段中只出现作者-年份形式，无法核实细节。

Q3: 论文如何解决这个问题？

Work2Skill 的解决方案分三层。第一层是 WorkRecorder：采集人类在桌面 GUI 上的操作经验，把屏幕录制、鼠标键盘事件和可用的 accessibility tree 信息在时间上对齐，从而保留动作及其视觉上下文和界面语义。第二层是 WorkDemo-350：用 WorkRecorder 收集并筛选出 350 个完整且任务成功的人类桌面 GUI 演示，作为技能归纳和下游评估的数据基础；筛选既检查完整性也检查任务是否成功。第三层是智能体式技能归纳（Agentic Skill Induction）：由归纳智能体按需检查证据（截图、动作、accessibility 信息），而不是一次性固定处理全部证据；它迭代地撰写和修订技能文本，最终产出的技能捕获工作流规则、步骤依赖和完成检查。产出技能以纯文本形式通过共享文本接口注入执行模型（用 <task skills> 包裹并追加到 base system prompt 末尾），执行时不需要访问原始录制或其他演示证据。论文按四个研究问题组织实验：RQ1 归纳技能能否提升演示任务的执行；RQ2 和 RQ3 分别检验两个复用维度（同一应用内任务需求变化、模型间迁移）；RQ4 检验多模态证据与自适应探索的价值（合理推断，基于摘要中 RQ1-RQ3 的实验组织描述和结果部分提到的 key analyses）。

Q4: 论文做了哪些实验？

实验设置：在 MyPCBench Bounded Action（N=64）和 OSWorld-Verified 的 286 任务子集上进行评估；对 OSWorld-Verified 先排除无法完成有效人类演示的不可行任务。使用官方评估器，所有文本指导方法共用同一文本接口，用清晰分隔的块追加到执行模型 base system prompt 末尾，所有文本指导方法都用相同的 <task skills> 包装（模板和注入内容见 Appendix D.5）。对比方法包括 No Skill、ShowUI-Aloha 和 Demo2Tutorial 的文本适配版本，以及 Work2Skill 归纳的技能。执行模型有四个：GPT-5.5、第二个执行模型（表中分数 81.53→86.20，名称未在检索片段中出现）、ShowUI-Aloha 和第四个执行模型（对应分数 38.38→44.56 和 24.27→34.11 的两个较弱执行器）。对每个执行模型×每个技能归纳方法组合，报告 Score、成功率（SR %）、Steps 和 Token 消耗（千 token）；归纳方法还报告归纳阶段 token（Tok.）。实验围绕 RQ1（演示任务上的执行提升）、RQ2/RQ3（复用：任务需求变化和模型间迁移）和 RQ4（证据与探索机制分析）组织。关键分析包括：多模态证据提升技能质量；自适应探索优于固定证据处理；归纳技能跨模型跨任务泛化。

Q5: 发现了什么实验现象？

实验现象：1) 在四个执行模型上，Work2Skill 归纳的技能均取得最高 Score 和 SR。MyPCBench 上相对 No Skill 平均分提升 4.67–10.54 分、成功率提升 6.25–25.00 个百分点；OSWorld-Verified 子集上提升 3.11–7.50 分和 1.40–7.69 个百分点。2) 相对演示基线（ShowUI-Aloha、Demo2Tutorial 文本适配版），Work2Skill 在 MyPCBench 上提升 3.95–7.79 分，在 OSWorld-Verified 子集上提升 1.65–6.96 分。3) GPT-5.5 在 MyPCBench 上 SR 达到 48.44%，而 Aloha 和 D2T 都是 37.50%，说明技能对较强的执行模型收益更明显。4) 效率收益依赖模型：Work2Skill 相比 No Skill 在某些模型上使用更少执行步骤（如第二个执行模型 36.03 vs 40.58 步；但 GPT-5.5 反而从 40.89 增至 47.38 步，说明技能文本更长会带来额外 token 开销，Step/Tok 之间存在张力）。5) 归纳阶段 token 消耗较高（GPT-5.5 上 334.52k tokens，对比 Aloha 170.65k、D2T 177.32k），说明按需证据检查+迭代修订的归纳成本显著高于固定流程。6) OSWorld 上 GPT-5.5 的 Steps 从 19.05 增至 25.78，但 Score 仍提升，说明步骤增多不总是坏事。7) 技能在归纳模型和执行模型不同的情况下仍可迁移，但更强的归纳并不能弥合执行器之间的性能差距——即执行器本身的能力上限依然存在。8) 失败案例显示 Work2Skill 技能也没有完全覆盖所有任务（如 The Dundies 示例中仍出现 missing step，ShowUI-Aloha 得分 55/100 的例子里配置主打开放应用失败，具体属于哪个方法的失败模式无法从片段确定——提示证据显示该例是为说明步骤缺失而给出的）。9) 消融/分析确认：多模态证据（视觉截图+动作+accessibility）比单一模态产出更好的技能；自适应探索（按需检查证据）优于固定证据处理。

Q6: 有什么可以进一步探索的点？

进一步探索点包括：1) 把技能归纳扩展到更复杂的跨应用工作流和需要人机交互确认的任务；2) 研究归纳技能的结构化表示（如任务图、状态机）与纯文本提示的优劣，减少执行时的 token 开销；3) 降低归纳成本（当前 GPT-5.5 上归纳消耗 334.52k tokens），例如用更小的归纳模型、证据预筛选或增量技能更新；4) 处理技能归纳中的任务特定细节与工作流规则分离问题，研究如何显式参数化技能（如“报销金额 450”这类实例）；5) 把 WorkRecorder 扩展到 Web、移动端或无障碍信息不完整的场景，研究缺失 accessibility tree 时的技能质量；6) 探索归纳技能的安全性和隐私边界：人类演示可能包含敏感信息，技能文本可能泄露工作流细节；7) 技能的生命周期管理：当软件 UI 更新或工作流变化时如何检测技能失效并自动修订；8) 研究执行模型的差异对技能利用方式的影响——论文观察到更强的归纳不能弥合执行器差距，可进一步分析技能是以指令遵循还是记忆方式被使用；9) 把技能归纳与在线学习结合，让智能体在失败时主动请求新演示。

Q7: 总结一下论文的主要内容

论文的论证主线：计算机使用智能体能力提升但仍缺程序性知识 → 人类演示与智能体共享同一 GUI 交互空间，是程序性知识的直接来源 → 但演示的复用需要结合上下文解释动作、分离工作流规则与任务细节 → 因此需要采集+归纳的完整框架。技术主线：WorkRecorder（对齐屏幕录制、鼠标键盘事件、accessibility tree）→ WorkDemo-350（350 个筛选后的多模态演示）→ Agentic Skill Induction（按需检查证据+迭代撰写修订技能文本）。实验主线：四个执行模型 × 三种技能来源（No Skill / ShowUI-Aloha / Demo2Tutorial / Work2Skill）在 MyPCBench（64 任务）和 OSWorld-Verified（286 任务子集）上比较 Score、SR、Steps、Token。主要结果：Work2Skill 全面最高；MyPCBench 相对 No Skill 提升 4.67–10.54 分和 6.25–25.00 个百分点 SR；OSWorld 子集提升 3.11–7.50 分和 1.40–7.69 个百分点；相对两个演示基线分别提升 3.95–7.79 和 1.65–6.96 分。关键分析发现多模态证据和自适应探索的贡献，以及技能跨模型、跨任务变体的可迁移性；也发现归纳模型能力影响技能质量，但更强归纳不弥合执行器差距。结论强调：对齐的人类演示+灵活证据探索能产生可复用的程序性指导，执行时通过共享文本接口注入，无需访问原始录制。注意：论文部分引用和附录细节（如 Appendix D.5 模板）在检索证据中只部分可见，具体归纳 prompt 完整内容、数据集任务清单、部分执行模型名称需要回原文确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向（你的画像中权重最高方向）直接相关：Work2Skill 是计算机使用智能体的技能归纳与程序性知识获取工作。

## 基本信息

- 作者：Yuqi Huang, Hui Li, Yifei Cao, Zikun Cai, Tianci Xie, Yangfan Gao, Pinxue Guo, Chaoyun Zhang, Bingxuan Li, Rui Zheng, Hang Yan, Ning Liao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.HC
- 日期：2026-10-12
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.12984`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（introduction/method/results/conclusion 的命中片段及 field_evidence_map 对应的证据锚点），并结合摘要与论文元数据进行补全；部分细节（如部分执行模型名称、消融具体设计、附录模板）在证据中缺失，已在相应字段标注需回原文确认。
