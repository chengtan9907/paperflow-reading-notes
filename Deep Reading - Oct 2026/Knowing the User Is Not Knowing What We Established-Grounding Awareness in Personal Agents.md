---
user_id: "cheng tan"
paper_id: 15926
arxiv_id: "2610.18684v1"
title: "Knowing the User Is Not Knowing What We Established: Grounding Awareness in Personal Agents"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18684v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18684v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:02:09"
---
# Knowing the User Is Not Knowing What We Established: Grounding Awareness in Personal Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：grounding awareness · personal agents · conversational memory · user authorization

## 一句话总结

本文提出“Grounding Awareness（接地意识）”这一概念，指个人智能体必须区分用户对提议的四种互动状态——已接受、未确认、已修订、限定范围——并构建了受控基准与 Grounding Layer（GL）机制，系统验证了“记住用户不等于记住双方已约定内容”这一核心论点，并发现更强的记忆系统、更完整的用户信息甚至更高模型能力都可能导致更多未经授权的执行。

## 摘要

> 论文以冷推销场景为例：助手提议未来拒绝销售电话，一种交互中用户明确接受，另一种中用户未回应；数周后同一推销电话再次到来，助手只在第一种情况下应直接拒绝，而第二种情况不应把未确认的提议当作已授权指示。然而现有的个人智能体记忆研究大多聚焦于学习用户知道什么、偏好什么、想要什么，记忆系统会把已记录的内容当作已确立的内容，丢失“提议-接受-修订-范围”这一交互状态的关键区分。为此论文提出 Grounding Awareness 能力定义，构建受控基准测试，系统测试智能体能否区分已接受与未接受的提议，以及能否只在合适范围内应用已接受的安排。实验跨多个智能体和记忆系统发现：强记忆性能并不保证 Grounding Awareness；更完整的用户信息反而可能增加对未确认提议的执行；持久记忆可能把未确认提议当作已授权指示，导致智能体在可发消息、可购物、可改日程的场景下产生未经授权的后果性行动。

Q1: 这篇论文试图解决什么问题？

1. 核心问题：现有个人智能体记忆系统普遍把“记住用户”当作“记住双方已经约定的内容”，两者之间存在根本差距。记忆系统记录一段对话历史 H 后写入持久状态 M，并在后续请求 q 时检索上下文；但个人化研究主要建模用户事实、偏好及其演化（如 Zhao et al., 2025; Li et al., 2026），而偏好不等于授权：用户不喜欢销售电话，并不意味着助手可以自行拒绝销售电话。
2. 被忽视的维度：一个提议（proposition）的互动状态（interactional status）由四要素决定——谁提出、是否被接受、是否被修订、适用范围在哪里；这与“用户私下被假设的偏好”无关，而是对话中共同确立（grounding）的结果。
3. 关键失败模式：记忆系统把一条单一未确认提议记录为持久偏好或指示后，会在未来所有会话中复用，等于把未授权内容变成指令。当智能体具备发消息、购物、改日程等执行能力时，这种错误会造成后果性危害。
4. 任务表述：论文将 Grounding Awareness 形式化为个人智能体的独立需求，并需要回答三个子问题：是否知道提议状态（Sensitivity）、是否只对已接受提议行动（Selectivity）、是否遵守已接受安排的适用范围（scope 约束）。
5. 隐藏更反直觉的问题：更强的记忆性能和更完整的用户信息是否必然带来更好的接地意识？论文发现否定的答案，甚至出现反向效应，这说明接地意识不是记忆能力的副产品，而是一个独立、需要专门设计的维度。

Q2: 有哪些相关研究？

1. 个人化记忆与用户建模：Zhao et al., 2025; Li et al., 2026 等方向建模用户事实、偏好与演化；记忆系统（Xu et al., 2025; Li et al., 2025b）跨会话组织、更新用户信息；但这些工作聚焦于检索和更新事实，不区分“讨论过的内容”与“达成一致的内容”。
2. 对话中的 grounding 与 common ground 理论：Clark and Schaefer, 1989; Clark and Brennan, 1991; Traum, 1991 奠定 grounding 理论，区分共同背景与互动中逐步确立的内容；近期工作（Cho and May, 2020; Shaikh et al., 2024; Mohapatra et al., 2024b; Poelitz et al., 2026; Jokinnen et al., 2024）考察语言模型如何在交互中建立 common ground。
3. 信息溯源（information provenance）：Wu et al., 2025 提出的“更新后回答最新值”等记忆更新问题；Yang et al., 2026 提出“根据提问者不同而给出不同回答”，涉及信息来源/说话者追踪；还有跟踪缺席参与者信息的访问控制问题。这些相关能力（信息溯源、信息来源、信息访问）与 Grounding Awareness 相邻但不相同：它们回答“信息从哪来”“该不该给谁”，而接地意识回答“该提议是否被共同确立为授权”。
4. 个人智能体的执行与记忆风险：论文以 OpenJarvis 默认系统提示（缺少当前日期会影响时间相关决策）等具体配置缺陷作为辅助例证，表明除了接地意识本身，工程配置也会造成边界故障。
5. 基准测试与评估方法：论文提出的基准采用确定性约束构建受控候选提议（candidate），只有满足确定性规则才被接受，配合人工标注（行动 gold 88% 一致率，接地状态标注 κ）验证标注质量；这与 MemOps（Guo et al., 2026）等长期对话生命周期记忆操作基准相关。

Q3: 论文如何解决这个问题？

1. 概念定义：提出 Grounding Awareness，定义为智能体区分已接受（accepted）、未确认（unacknowledged）、已修订（revised）和限定范围（scoped）安排的能力，并在选择行动时只对已接受的安排生效、只在适用范围内生效。
2. 任务形式化：给定完整交互历史 H，记忆方法写入持久状态 M，并在后续请求 q 时检索上下文 M；目标是在后续决策中基于提议的互动状态而非用户私下偏好做判断。
3. 受控基准构建：设计一个可控的基准测试，生成包含同一提议与后续请求、但用户是否授权不同的成对历史（例如 Figure 1 的冷推销场景：用户“接受”vs“未回应”），从而在控制变量的前提下测试智能体区分能力。
4. Grounding Layer（GL）机制：在评估的多个模型/记忆配置之上叠加一个“接地层”，用预定义规则治理后果性行动，从互动状态（status）、范围（scope）、修订（revision）和行动风险（action stakes）出发决定是否执行，而不是从用户私下假定的偏好出发；证据显示 GL 在评估的全部配置中一致提升 Grounding Awareness。
5. 评测维度：论文同时测量 Sensitivity（对提议状态变化的敏感度）、Selectivity（只执行已接受提议的选择性）、Overgeneration（过度生成/越权行动）、Overconservatism（过度保守/漏执行）、ΔExec I−N（已接受与未确认两种历史之间的执行差异，即区分度指标）。
6. 稳健性：设计 robustness test（附录 B.3），并在附录中补充跨 grounding 状态、跨 backbone、跨记忆系统的性能分析（G.5 Performance Across Grounding…）；对时间相关决策，还指出 OpenJarvis 默认系统提示缺少当前日期属于工程故障，需单独修复。

Q4: 论文做了哪些实验？

1. 实验配置：跨多个 agent 框架/backbone（包括 Hermes full 配置、DeepSeek、OpenJarvis 等）与多种记忆系统（含持久化记忆写入与检索机制）进行系统比较；backbone 配置中记录了 Sensitivity、Selectivity、Overgeneration、Overconservatism、ΔExec I−N 等指标。
2. 成对历史受控测试：对同一提议（如冷推销拒绝提议、Kevin 生日聚会邀请等）构造用户接受与未确认两种历史，数周后同一请求到达，观察智能体是否只在已接受历史中执行。
3. 标注质量验证：两名人工标注者验证行动 gold（88% 一致率）与 grounding-condition 标签（计算 inter-annotator κ），说明基准标签具有可复用性。
4. 记忆系统对比：比较“无记忆”“基本记忆”与持久化记忆写入/检索在接地意识上的表现，验证“强记忆性能≠强接地意识”；包含记忆保存 scoped 提议范围的专项测量（如 Jarvis 对已存 scoped 提议只保留 4–11% 的范围约束）。
5. 信息完整性对照：在部分配置中提供更完整的用户信息（更多偏好、事实上下文），测量其对未确认提议执行率的影响，检验“更多信息是否帮助”的反直觉假设。
6. GL 干预实验：在所有配置上叠加 Grounding Layer，报告对 Sensitivity、Selectivity、Overgen.、Overcons.、ΔExec I−N 的改善；包括 Hermes full 配置的详细数值（Sens. 0.21、Sel. 0.23、Overgen. 0.28、Overcons. 0.37、ΔExec I−N +0.24）。
7. 错误归因分析：当完整交互信息可用时，71% 的错误仍发生在存在可获取证据的情况下，说明错误不是单纯的信息缺失问题；还包括附录 B.3 的 robustness test 与 G.5 的跨 grounding 状态性能分析。
8. 工程边界检查：检查 OpenJarvis 默认系统提示中缺少当前日期等配置问题如何影响时间依赖决策，将其与接地意识缺陷区分开。

Q5: 发现了什么实验现象？

1. 强记忆性能不保证接地意识：在多个代理与记忆系统的组合上，记忆能力强的系统在 Sensitivity、Selectivity 上仍可能很低，说明接地意识是独立能力维度。
2. 更多用户信息反而有害：提供更完整的用户信息会增加对未确认提议的执行（例如把“用户可能不喜欢推销”这一偏好当作执行依据），直接反驳“信息越全越好”的直觉；这是论文最关键的反直觉发现。
3. 持久记忆放大错误：持久记忆会把一次未确认的助手提议固化为长期指示，跨会话复用，导致未授权执行；71% 的错误在完整交互信息可用时仍然出现，说明错误不是证据缺失而是状态判定错误。
4. 具体模型表现：Hermes full 配置下 Sens. 0.21、Sel. 0.23、Overgen. 0.28、Overcons. 0.37，ΔExec I−N 为 +0.24——即虽然区分度为正，但仅约两成左右的敏感度和选择性，且存在明显的过度生成和过度保守双重错误。
5. DeepSeek 的 Sensitivity 从 0.41 降到 0.37、Contextual Selectivity 从 0.42 降到 0.33：在某些条件下（如更保守的系统设定、或减少 false common ground）模型反而更不接地，说明“减少虚假共同背景”并不自动转化为“更好的接地执行”。
6. 范围约束丢失严重：Jarvis 对已存储的 scoped 提议只保留了 4–11% 的范围限制，说明即使记住提议本身，其适用边界也极易在记忆写入/检索中丢失。
7. GL 干预一致有效：在所有评估配置上 Grounding Layer 均提升接地意识，说明通过显式规则治理行动决策可以部分弥补模型和记忆系统的缺陷。
8. 标注可靠性：行动 gold 人工标注一致率 88%，接地状态标注 κ 可用，说明基准的标签并非噪声主导。

Q6: 有什么可以进一步探索的点？

1. 长期跨度记忆生命周期：论文限定在单次或短程会话，未来可研究长期多会话中记忆的写入、更新、遗忘、修订对接地状态的影响（与 MemOps 等生命周期基准结合）。
2. 让智能体主动确认而非被动记忆：研究 agent 在不确定授权时主动发起 grounding/确认行为的策略，把接地意识从“判别”扩展为“互动策略”。
3. Grounding Layer 的规则泛化：当前 GL 用预定义规则治理后果性行动，未来可探索学习型策略、风险分级授权、可解释规则库，以及如何在规则冲突时仲裁。
4. 跨语言、跨文化、跨任务类型的转移：销售电话、日程安排、购物等场景之外，测试医疗、金融、法律等高风险领域中“授权状态”的判定与表示。
5. 指标体系的深化：Sensitivity/Selectivity/Overgen./Overcons./ΔExec 之外，引入代价敏感评估（不执行 vs 错误执行的代价不对称）、范围遵守率的精细测量，以及长期行为审计。
6. 多模态与多参与者 grounding：跟踪“缺席参与者”信息、群体对话中谁提议谁接受的归属问题；这是与信息访问（Yang et al., 2026）、信息溯源相邻的开放问题。
7. 记忆表示层的接地状态编码：探索显式结构化记忆（把 proposition 的 status/scope/revision 作为一等公民）而非隐式文本记忆；这可能是根治“记忆丢失范围约束”的路线。
8. 用户模型与授权模型解耦：把“用户偏好画像”与“交互许可状态”分开存储和更新，从架构层面防止偏好被误用为授权。

Q7: 总结一下论文的主要内容

论文的核心论证是：个人智能体记忆系统长期以来以“知道用户”为目标——建模用户的已知、偏好和需求并跨会话更新——但“知道用户”不等于“知道用户与智能体共同确立了什么”。一个助手提议在未经用户确认时被记住并复用，就会变成一条未经授权的长期指示；已接受的安排也不必依赖一般偏好即可约束未来行动。论文由此提出 Grounding Awareness，把它定义为个人智能体的一项独立需求：能够区分已接受（accepted）、未确认（unacknowledged）、已修订（revised）、限定范围（scoped）的提议，并在行动选择中只执行恰当的授权。技术路线上，论文没有直接提出新的模型架构，而是先构建受控基准：以 Figure 1 的冷推销场景为原型，构造同一提议、同一后续请求但用户授权状态不同的成对历史，用确定性约束生成候选提议并配合人工标注（行动 gold 88% 一致率、接地标注 κ）保证质量；随后在多个 agent backbone（包括 Hermes、DeepSeek、OpenJarvis）与多种记忆系统组合上测量 Sensitivity、Selectivity、Overgeneration、Overconservatism 与 ΔExec I−N。结果呈现三个层次的发现：一是强记忆性能不代表接地意识；二是更完整的用户信息会显著提高对未确认提议的执行率；三是持久记忆会把未确认提议固化为长期指令，且 71% 的错误发生在完整交互信息可用的情况下。与此同时，Jarvis 对已存储 scoped 提议仅保留 4–11% 的范围约束，说明范围信息是记忆链路中最脆弱的成分。作者进一步提出 Grounding Layer（GL），用预定义规则在行动决策层治理授权状态、范围、修订与行动风险，并在全部评估配置中一致提升接地意识（如 Hermes full 下 Sens. 从 0.21 提升、ΔExec I−N +0.24 等）。论文的工程性发现还包括 OpenJarvis 默认系统提示缺少当前日期会干扰时间相关决策这类配置故障；Limitations 中承认 GL 目前依赖预定义规则治理后果性行动，未来需要研究长期跨度下的记忆生命周期操作以及智能体如何主动学习接地。总体而言，论文用受控实验把“接地状态”从对话理论概念转变成可评测、可干预的个人智能体设计约束，其主张是：记忆系统与用户画像系统不应把偏好与授权混为一谈，接地意识应作为独立能力被显式建模与评测。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你画像中的 agent 方向（权重 0.10）直接重合，论文研究对象正是 LLM-based personal agents 的行动安全与记忆设计

## 基本信息

- 作者：Jiajun Chen, Xinyue Chen, Yicheng Jiang, Hua Shen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.HC, cs.LG
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18684v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要与 Introduction 证据片段；检索证据多为截断文本，部分实验数值与配置细节已标注需回原文核对。
