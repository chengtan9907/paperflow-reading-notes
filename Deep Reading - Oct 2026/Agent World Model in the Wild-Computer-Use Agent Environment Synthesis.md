---
user_id: "cheng tan"
paper_id: 16502
arxiv_id: "2610.26828v1"
title: "Agent World Model in the Wild: Computer-Use Agent Environment Synthesis"
institution: "合理推断：以 Microsoft Research（微软研究院）为主——代码仓库托管于 MSR-Orchard 组织，作者群包含多位微软研究院学者（如 Jianfeng Gao、Baolin Peng、Hao Cheng、Canwen Xu、Wenlin Yao 等）；部分作者（如 Huaxiu Yao）可能隶属高校。论文 JSON 元数据未提供机构字段，具体单位请以 arXiv 首页/论文首页标注为准。"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.26828v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.26828v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:07:15"
---
# Agent World Model in the Wild: Computer-Use Agent Environment Synthesis

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：environment synthesis · computer-use agent · agent world model · quality gate

## 一句话总结

WildAWM 提出一种完全由开源模型驱动的计算机使用智能体（CUA）环境合成流水线：构建同时支持 GUI、MCP、CLI 三种交互接口的可执行个人计算机世界与跨接口长 horizon 任务，并用“异构 agent 执行证据 + 跨 judge 一致性 + 反事实探测”的执行式质量门控保证任务可解、够难、verifier 可靠；仅在这些合成世界中训练的 Qwen-4B/35B 模型在 8 个未见过的 web/claw/computer-use benchmark 上取得一致提升，而 GPT-6-Astra 在合成任务集上也只解出约 60%。

## 摘要

> 背景与动机：agent 学习正从“从人类数据学习”转向“通过与世界交互学习”，但直接在真实计算机系统中规模化这种学习非常困难，且会带来显著安全风险。方法：提出 WildAWM——一套可扩展的、用于训练通用 CUA 的可控环境与任务合成流水线。与现有工作的关键差异：现存方法通常聚焦单一 agent 交互接口（web、工具、终端、软件工程各自为政）；WildAWM 构造同时支持 GUI、MCP 服务、CLI 的可执行环境，让 agent 能灵活组合视觉交互、工具调用与代码执行。全开源：整个流水线完全由开源模型驱动，论文认为这证明了高质量计算机使用环境不一定依赖专有前沿模型。任务合成的三大挑战：确保可解性、维持足够难度、产出可靠 verifier；对策是执行式质量门控——结合异构 agent rollouts、跨 judge 一致性检查、判断鲁棒性的反事实探测。产出与验证：合成 341 个长 horizon 环境-任务对，部分任务需要超过 100 步 agent 动作，即使 GPT-6-Astra 也只能解决其中约 60%；仅用这些合成世界训练两个不同规模的 Qwen 模型，二者在覆盖 web、claw、computer-use 的 8 个 OOD benchmark 上均取得显著提升，说明在 WildAWM 合成世界中学习可泛化到未见过的计算机使用任务。代码已公开：https://github.com/MSR-Orchard/WildAWM。

Q1: 这篇论文试图解决什么问题？

1. 范式背景：LLM agent 学习正从“模仿人类数据”转向“与世界交互学习”。后者在真实计算机系统上规模化有三重障碍——真实环境不可重置、不可完全控制；交互成本高、并行度低；真实操作（文件修改、网络请求、系统配置）可能造成实际损害并引入安全风险。因此必须有可控、可重置、可判分的“合成世界”作为训练场。
2. 核心问题：如何自动、可扩展地合成高质量的可执行计算机使用环境与任务，使通用 CUA（general computer-use agent）能在其中学习并获得可迁移的能力？
3. 缺口一：单一接口局限。现有环境合成与 CUA 研究通常锁定单一模态（GUI web 导航、桌面自动化、终端、MCP 工具、软件工程），但真实个人电脑任务往往是跨接口复合的：看界面、点按钮、调工具、写命令可能出现在同一任务中。缺少同时支持 GUI/MCP/CLI 的“通用个人电脑”环境。
4. 缺口二：任务状态分布失真。主流 benchmark 使用任务专用、注入式的初始状态，任务之间彼此孤立；真实使用中 agent 面对的是某一人物长期使用电脑后形成的累积状态（文件、配置、历史、应用数据）。WildAWM 用 persona 跨多日累积世界状态 W(c,d) 逼近这种分布。
5. 缺口三：合成任务的三难问题。可解性——LLM 生成的文本任务常“看似可解、执行上无解”或只有靠欺骗 verifier 的伪解；难度——太简单会沦为又一个 toy benchmark，难以训练长 horizon 能力；verifier 可靠性——自动判定任务是否完成本身不可信，LLM judge 会被 agent 自述或表面迹象欺骗。
6. 缺口四：对专有前沿模型的依赖。高质量数据/环境生成通常被视为只有 GPT/Claude 级专有模型才能完成；论文要回答“开源模型能否闭环自举出高质量环境”，这直接关系到可复现性与可审核性。
7. 隐含假设与风险：核心假设是“合成世界与真实世界分布足够接近，以至于在合成世界学到的能力能 OOD 泛化到真实未见任务”。该假设不 trivial，论文用 8 个外部 benchmark 检验，但真实桌面域（OSWorld 系）上的迁移幅度仍有限。
8. 术语提醒：论文标题中的“Agent World Model”不是传统意义的状态转移预测模型，而是“可执行世界/环境生成器”，阅读时需注意这一术语差异。

Q2: 有哪些相关研究？

1. Computer-use agents 谱系：（a）GUI/视觉驱动 agent——web 导航（He et al. 2024; Zheng et al. 2024; Yang et al. 2026a）、桌面自动化（Agashe et al. 2025; Zhang et al. 2024; 2025a; Wang et al. 2026b）、多应用工作流（Tan et al. 2024; Qin et al. 2025; OpenAI 2025b; Yang et al. 2026b）；（b）行动空间扩展——CLI（Zhou et al. 2026）、coding（Song et al. 2026）、MCP 工具（Jia et al. 2025）；（c）harness/claw 类框架——通过代码与 claw-agent 框架暴露异构工具（Yu et al. 2026; OpenClaw contributors 2026; Anthropic 2025; anomalyco 2025）。论文明确采纳“通用 CUA = GUI + CLI + MCP 按需联合使用”的广义视角，而非绑定单一交互模态。
2. Environment synthesis（可执行环境自动构建）近年兴起：web 交互环境合成（Zhou 2026; Wu et al. 2026）、工具使用环境合成（Wang et al. 2026d; Du et al. 2026; Dong et al. 2026）、终端交互环境合成（Zhu et al. 2026; Ivison et al. 2026; Li et al. 2026）、软件工程环境合成（Jain et al. 2025; Yang et al. 2025; Chen et al.）。
3. 与这些工作的区别：（a）单接口 vs 跨接口——现有合成通常围绕单一应用或单一接口，WildAWM 构建覆盖 GUI/MCP/CLI 的完整个人电脑环境，且任务生成要求轨迹实际使用所声明的全部接口，作为跨接口工作流的执行证据；（b）状态分布——多数合成环境用任务专用注入状态，WildAWM 使用 persona 跨多天活动累积的状态（中位 52.4K tokens）；（c）质量控制——把可解性、难度、verifier 可靠性显式化为执行式质量门，而不是只依赖 prompt 质量；（d）全开源闭环——GLM-5.2 担任生成器与 agent-as-a-judge，不依赖专有前沿模型。
4. 评测/训练参照：OOD 评测覆盖 WebArena、Online-Mind2Web、QwenClawBench、ClawEval、CUA-Gym、OSWorld-Verified、OSWorld 2.0、Agents' Last Exam 三类任务；与 OpenForgeRL（Yu et al. 2026）、CUA-Gym 的已报道结果作参考对比（脚注说明：因其 checkpoint 不可得或使用传统 GUI harness，仅作参考）。
5. 事实校准：论文为 arXiv 2026-10 版本，引用了大量 2026 年文献与前沿模型名（GPT-6-Astra、Claude Opus 5/4.8、GLM-5.2 等）；本报告按论文自述转述这些条目，不对其现实可核验性作判断。

Q3: 论文如何解决这个问题？

1. 整体框架：每个合成实例是一个三元组——从世界状态 W 初始化的个人计算机环境、任务 τ、以及评估任务是否完成的 verifier V。任务与“当前世界状态”绑定，而不是凭空生成。
2. 世界合成（World Synthesis）：世界状态定义为 persona c 在日期 d 的累积计算机状态 W(c,d)，模拟一个“数字人物”连续多天使用电脑留下的文件、配置、应用状态与活动痕迹；环境状态中位规模 52.4K tokens，远大于 CUA-Gym 的任务级注入状态。这样任务可以依赖“历史遗留”，逼近真实个人电脑的分布。
3. 任务生成（Task Generation）：给定初始世界状态 W0=W(c,d)，生成与该状态有根（grounded）的任务；生成时必须声明任务预期使用的接口组合（GUI/MCP/CLI）；验证时要求成功轨迹实际使用所有声明的接口，提供“跨接口工作流确实被执行”的轨迹级证据。
4. 多 agent 验证循环与五个质量门：异构开源 agent 逐个尝试任务，五个质量门用执行证据决定接受、修订或丢弃任务。
（1）可解性门：以异构 agent rollouts 的成功作为任务可解的执行证据，避免“文本可解、执行无解”。
（2）初始不可满足预筛（Init. Unsat.）：在 rollouts 前过滤任务形式/状态自相矛盾的情况；消融显示移除它使平均分从 31.86 降至 24.08（Table 7）。
（3）难度门：估计任务难度（步骤数与成功率的组合），保证足够难又不至于不可解；合成任务中位数可达 100+ 步，GPT-6-Astra 约 60% 成功率。
（4）跨 judge 一致性检查（Judge Cons.）：多个 judge（或 majority vote）对同一轨迹判定的一致性；报告 inter-judge Cohen's κ=0.90，消融显示移除后平均分从 31.86 降至 22.56，是损失最大的门。
（5）反事实探测（CF Probe）：构造“看似接近完成、实际未完成”的反事实轨迹，检验 judge 是否被表面迹象欺骗；移除后平均分降至 27.91。
5. verifier 证据规则（关键设计）：agent 的笔记（note）只能作为“交付物本身”的证据，永远不能作为外部事实的证明——例如“agent 声称选中了某个网页元素”不证明它真的选中了；必须交叉核对独立来源（查询相关系统状态、应用日志等）。这实质是把“自我陈述证据”与“执行证据”分离，防止 judge 被 agent 自述带偏。
6. 训练配方：先对 Qwen base 模型做 SFT 3 epochs 热身，再 RL（最多 64 训练步，batch size 8 个任务，每任务 G=4 次 rollout）。RL 中按任务维护已完成 rollout 池（池大小 K），µτ 与 στ 为该任务池内奖励的均值与标准差，轨迹优势定义为 (rj−µτ)/στ，并按 reward 上/下半池分别统计以鼓励奖励多样性（该细节来自检索到的方法片段，公式上下文不完整，确切语义需回原文确认）。
7. 全开源基建：8 张 NVIDIA B200 GPU 完成合成与训练；GLM-5.2 兼任任务生成器和 agent-as-a-judge；OpenCode harness（anomalyco 2025）驱动执行；评估时 OpenCode 配 GUI（click、type）与 MCP（Playwright）工具并开启高推理强度。

Q4: 论文做了哪些实验？

1. 数据与划分：WildAWM 合成 341 个任务，随机分为 100 个 SFT、208 个 RL、33 个域内评测（检索片段记为“341 XAI tasks”，可能是命名或转录噪声，按 341 个任务理解）。
2. OOD 评测集：8 个外部 benchmark 分 3 类——web agent：WebArena（WA）、Online-Mind2Web（O2W）；claw agent：QwenClawBench（QCB）、ClawEval（CLE）；computer-use agent：CUA-Gym（CUG）、OSWorld-Verified（OSV）、OSWorld 2.0（OS-2）、Agents' Last Exam（ALE）。指标：O2W、WA、CLE 报 pass rate，其余报 mean score。
3. 模型与 baselines：训练用 Qwen3.6-35B-A3B 与 Qwen3.5-4B 两个规模；对比前沿模型包括 GPT-6-Astra、GPT-5.6-Sol、GPT-5.5、Claude Opus 5、Claude Opus 4.8、GLM-5.2，统一在 OpenCode harness 下评测；CUA-Gym 与 OpenForgeRL 的已报道数字仅作参考（脚注：因其 checkpoint 不可得或使用传统 GUI harness 评测）。
4. 训练设置：SFT 3 epochs 热身，RL 最多 64 训练步、batch size 8 任务、G=4 rollouts/任务；合成与训练共用 8 张 NVIDIA B200。
5. 域内验证：GPT-6-Astra 在合成任务上仅约 60% 成功率，说明任务对前沿模型仍困难（域内评测上 WildAWM 训练模型的具体分数未在检索证据中出现，标注为信息缺口）。
6. 消融实验一（质量门，Table 7）：在 SFT 设置下对 Qwen3.5-4B 逐个移除门，报告 4 列分项 + 平均：完整 WM+SFT = 38.49 / 50.17 / 17.17 / 21.59 / 31.86；w/o Init. Unsat. = 33.21 / 42.36 / 17.71 / 3.05 / 24.08；w/o Judge Cons. = 29.10 / 37.72 / 19.62 / 3.79 / 22.56；w/o CF Probe = 31.37 / 42.55 / 24.09 / 13.64 / 27.91。各列对应的具体 benchmark 映射未在证据中完整给出，需回原文确认。
7. 消融实验二（格式惩罚，Table 8）：移除格式惩罚（format penalty）导致 WA 与 OSV 显著下降；CLE pass rate 反而升高但 mean score 未同步升高（69.89 vs 完整版 69.45），说明更多任务只是“刚好越过 pass 阈值”。
8. verifier 可靠性测量：Judge majority 在 5 个判据上与单个 judge（Claude Opus 5）的一致性为 0.84–0.95 区间（如 Opus-5 0.89/1.00/0.80/0.89/0.79，Judge majority 0.95/0.98/0.93/0.95/0.90），inter-judge Cohen's κ=0.90。
9. 环境多样性测量：随任务池增长，逐对任务嵌入的余弦距离均值保持稳定，说明覆盖度增加并非来自相似任务的重复堆叠。

Q5: 发现了什么实验现象？

1. 主结果（Table 5，4B 级可见部分）：Qwen3.5-4B base 平均 21.83（O2W 26.39 / WA 30.09 / QCB 44.06 / CLE 26.67 / CUG 21.39 / OSV 18.18 / OS-2 0.87 / ALE 7.00）；WildAWM 训练后 O2W 45.65 / WA 39.82 / QCB 48.37 / CLE 36.67 / CUG 37.35 / OSV 22.48，其余列在检索证据中被截断。web 类提升幅度最大（O2W +19.3 点），OSWorld 系（OSV）提升较小（+4.3 点），提示合成世界到真实桌面 OS 任务的迁移仍是短板。
2. 合成任务确实难：GPT-6-Astra 在 8 个 OOD benchmark 上平均 73.11，但在 WildAWM 合成任务上只有约 60% 成功率；任务中位数 100+ agent 步骤，与常见短任务 benchmark 分布不同——长 horizon 是刻意制造的难度轴。
3. 反直觉点：4B 级模型仅靠纯合成数据训练，OOD 平均从 21.83 大幅上升（可见部分平均已接近 38），说明在 CUA 能力上“环境质量 + 任务质量”能部分补偿模型容量；但 35B 训练行未出现在检索证据中，无法在此确认规模效应（信息缺口）。
4. 消融趋势：移除 judge 一致性损失最大（31.86→22.56），verifier 链条是当前质量瓶颈；移除 Init. Unsat. 使某一列从 21.59 崩到 3.05（可能是某个 OOD 域），说明“初始不可满足”任务若不预筛会系统性污染训练信号。
5. 反事实探测的代价：移除 CF Probe 后某一列（13.64）反而优于完整版对应列（21.59 所在列），说明反事实探测可能过滤掉部分“边界可解”任务、牺牲个别域分数换取整体可靠性（列映射需回原文确认）。
6. pass rate 指标的可投机性：移除格式惩罚后 CLE pass rate 上升但 mean score 几乎不变（69.89 vs 69.45），典型“阈值投机”——更多轨迹刚好擦线通过而非做得更好；这说明 CUA 评测中 pass@k 类指标易被格式/边界行为污染，verifier 需要显式惩罚这类行为。
7. verifier 可靠性测量：inter-judge Cohen's κ=0.90，majority voting 与单个强 judge 的一致性在 0.84–0.95 区间，说明跨 judge 投票能稳定判定；但 κ 是在自然变化范围内测量的，反事实扰动下可靠性会下降（推测）。
8. 环境多样性：任务池增长时逐对余弦距离均值保持稳定，新增覆盖不是同质任务堆叠，给“继续扩数据规模”提供了基本前提。
9. 负结果与边界：4B base 在 OS-2 上仅 0.87、ALE 上仅 7.00，训练后仍远低于前沿模型（60–80 档）；真实桌面（OSWorld 2.0）与“考试型”综合任务（ALE）是合成训练最难泛化的两个方向，说明当前合成世界与真实桌面/综合考试分布仍有明显 gap。

Q6: 有什么可以进一步探索的点？

1. 合成到真实的系统性迁移研究：OSWorld 2.0 类真实桌面任务提升最小，值得拆解根因——是合成环境的视觉保真度不足、任务粒度不匹配、还是状态分布差异？可做分层诊断实验。
2. 世界状态的规模与长期性：中位 52.4K tokens 只是“多日”级别；可扩展到更长时间跨度、多用户/多 persona 状态，甚至多 agent 共享世界，产生更丰富的 grounded 任务空间。
3. verifier 的进阶：更难察觉的反事实扰动（部分完成、顺序错误、副作用未清理）、可证明的判定一致性、把系统日志/应用状态/网络流量自动转成证据链，减少对 LLM judge 主观判断的依赖。
4. 难度自适应与课程学习：根据当前训练 agent 的能力动态调节任务难度，让世界生成与训练循环耦合，形成 self-play 式自举。
5. 接口覆盖扩展：文件系统权限与安全边界、多应用并行与多窗口协作、剪贴板与后台进程、真实浏览器网络交互、移动端环境。
6. 安全与对齐：在合成世界中预演权限越界、有害操作、prompt 注入，把安全行为作为默认训练信号嵌入任务合成。
7. 完全自举：目前 GLM-5.2 仍是生成器与 judge；可探索用训练中的 Qwen 自身迭代生成下一批世界（弱监督自举），并研究 judge 偏差如何传导进合成任务分布。
8. 评测标准化：研究 pass rate 与 mean score 的阈值投机问题（本文 format penalty 消融已间接证明其存在）；对齐合成环境与 OSWorld 2.0 / Agents' Last Exam 的评分协议，建立统一的 CUA 难度标尺。
9. 跨领域迁移（推测）：同样的“环境合成 + 执行式质量门”方法论可迁移到科学计算环境（模拟器、仪器 API、数据管道），为科学 agent 提供可控训练场，这与科学应用方向可能形成交集。

Q7: 总结一下论文的主要内容

论证主线：论文从范式转移出发——agent 学习正从“从人类数据学习”转向“通过与世界交互学习”，但真实计算机系统不适合作为规模化训练场（不可控、难重置、有真实损害与安全风险），因此核心命题是：能否用合成世界替代真实世界作为通用 CUA 的训练环境，并保证合成世界足够真实、足够难、足够可验证。论文对“通用 CUA”采取广义定义：agent 不应被绑定在单一交互模态上，而应在 GUI、MCP、CLI 之间按需切换。现有环境合成工作按接口分裂（web/工具/终端/软件工程各自为政），且任务状态多为注入式、彼此孤立，与真实个人电脑“某人物长期使用后的累积状态”分布不一致。
技术主线：WildAWM 把合成实例定义为三元组（世界状态 W、任务 τ、verifier V）。世界合成把状态建模为 persona 在多日使用后的累积结果 W(c,d)，环境状态中位 52.4K tokens；任务生成要求任务 grounded 在累积状态上并声明预期接口组合；随后进入多 agent 验证循环——异构开源 agent 执行任务，五个执行式质量门审查执行证据决定接受、修订或丢弃：可解性门（异构 rollout 成功证据）、初始不可满足预筛、难度门（足够难但可解）、跨 judge 一致性检查、反事实探测（judge 是否被“看似完成实则未完成”的轨迹欺骗）。verifier 采用严格证据规则：agent 自述笔记只能作为交付物的证据、不能作为外部事实的证明，必须交叉核对独立系统状态。整个管线只用开源模型：GLM-5.2 兼任生成器与 agent-as-a-judge，OpenCode harness 执行，8×B200 GPU 完成合成与训练。训练配方为 SFT 3 epochs 热身 + RL（≤64 步、batch 8、G=4），RL 用按任务归一化的轨迹优势 (rj−µτ)/στ 并区分 reward 上下半池以鼓励奖励多样性。
实验主线：合成 341 个长 horizon 任务（100/208/33 划分），GPT-6-Astra 在其上仅约 60% 成功率，证明难度充足；用 Qwen3.5-4B 与 Qwen3.6-35B-A3B 在纯合成世界中训练，并在 8 个 OOD benchmark（WebArena、Online-Mind2Web、QwenClawBench、ClawEval、CUA-Gym、OSWorld-Verified、OSWorld 2.0、Agents' Last Exam）上与 6 个前沿模型对比。可见证据显示：4B base 平均 21.83，WildAWM 训练后在多个 benchmark 大幅提升（O2W 26.39→45.65、WA 30.09→39.82、CUG 21.39→37.35、OSV 18.18→22.48），证明合成世界学习能跨 web/claw/computer-use 三类任务泛化。消融证明每个质量门都有贡献：移除 judge 一致性损失最大（31.86→22.56），移除初始不可满足预筛在某个 OOD 域近乎崩盘（21.59→3.05）；格式惩罚消融揭示 pass rate 可被阈值投机污染（CLE pass 升而 mean score 不升）。verifier 的 inter-judge Cohen's κ=0.90，任务池余弦距离稳定说明可扩展性有保障。结论主张：开源模型可以自举生成可扩展的合成世界，并在其中训练出向未见任务泛化的通用 CUA。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与“智能体（agent）”方向直接重合：论文解决的是通用计算机使用 agent 的训练环境问题，是本方向的 infrastructure 级工作。

## 基本信息

- 作者：Zhaoyang Wang, Qianhui Wu, Haorui Wang, Simon Yu, Baolin Peng, Canwen Xu, Hao Cheng, Wenlin Yao, Boyi Liu, Yite Wang, Jianfeng Gao, Huaxiu Yao
- 机构：合理推断：以 Microsoft Research（微软研究院）为主——代码仓库托管于 MSR-Orchard 组织，作者群包含多位微软研究院学者（如 Jianfeng Gao、Baolin Peng、Hao Cheng、Canwen Xu、Wenlin Yao 等）；部分作者（如 Huaxiu Yao）可能隶属高校。论文 JSON 元数据未提供机构字段，具体单位请以 arXiv 首页/论文首页标注为准。
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.26828v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（130 个 chunk）与 field_evidence_map，并结合摘要、Introduction、Results、Discussion/Conclusion 归纳；heuristic_draft 中混入的表格残片与页眉噪声（如孤立数字、'Preprint'、'is admissible ONLY...' 被误拼入贡献列表）已忽略并改用干净文本改写，部分缺失数字（Table 5 的 35B 行、Table 7 列名映射、域内评测具体分数）已在相应字段标注为信息缺口。
