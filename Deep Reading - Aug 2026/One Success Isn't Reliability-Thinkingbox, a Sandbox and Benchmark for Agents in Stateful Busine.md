---
user_id: "cheng tan"
paper_id: 9098
arxiv_id: "2608.19741"
title: "One Success Isn't Reliability: Thinkingbox, a Sandbox and Benchmark for Agents in Stateful Business Workflows"
institution: "微软（Microsoft）团队（由开源仓库 https://github.com/microsoft/thinkingbox 合理推断；论文元数据未直接提供作者机构）。"
publish_date: "2026-08-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.19741.pdf"
pdf_url: "https://arxiv.org/pdf/2608.19741"
abs_url: "https://arxiv.org/abs/2608.19741"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-21T23:58:23"
---
# One Success Isn't Reliability: Thinkingbox, a Sandbox and Benchmark for Agents in Stateful Business Workflows

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent benchmark · tool use evaluation · stateful business workflows · sandbox

## 一句话总结

论文提出 Thinkingbox 沙箱与 Thinkingbox-bench 基准，用 507 个有状态业务工作流任务和基于终端后端状态的执行检查来评估智能体，发现最强模型 pass@1 为 65.36% 但 pass^20 仅为 25.25%，揭示“一次成功不等于可靠”的 discovery-reliability gap。

## 摘要

> Recent agent benchmarks increasingly ground evaluation in executable environments, from code repair to web navigation, app APIs, and function calling. Yet completing consequential work beyond code requires more than producing a plausible response or valid tool call: agents must gather missing information over multiple turns, follow domain policies, coordinate dependent tools, and realize the correct persistent state transition without collateral effects. In this paper, we introduce Thinkingbox, a sandbox for tool-agent-user interaction that provides isolated MCP-compatible tool sessions, complete execution traces, and outcome evaluation over terminal backend state. Built on this sandbox, Thinkingbox-bench contains 507 policy-conditioned workflows across numerous scenarios, including retail, hospitality, auto insurance, neobank internal IT, and consulting IT/HR support. Each attempt is evaluated by task-specific executable checks that accept valid trajectories while rejecting wrong, missing, or extra effects; designated tasks additionally check required properties of the final response. Across proprietary and open-weight models, the strongest achieves 65.36% pass@1, but only 25.25% pass^20. Moreover, many failed trials show clean termination and valid state-changing actions, showing that response or tool-call-level signals are not clear proxies for end-to-end task completion. Thinkingbox-bench reveals a large gap between occasionally finding a successful trajectory and reliably completing stateful business tasks. We release both Thinkingbox and Thinkingbox-Bench: https://github.com/microsoft/thinkingbox

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题可以拆成三层：
1. 现有智能体评估的信号错位：很多函数调用式工具评估主要看 API 选择、参数生成或可执行调用是否合法（例如 Li et al., 2023；Patil et al., 2024；Qin et al.），但这些信号不能衡量真实业务工作流中最重要的东西——最终系统状态是否正确。
2. 真实业务任务的多轮与状态依赖特性没有被现有基准充分覆盖：用户往往一开始就给出不完整信息，智能体需要主动追问；领域策略会限制智能体可做的操作；多个工具之间存在依赖关系；成功要求最终持久状态发生正确迁移，同时不能留下多余副作用。
3. 评估环境缺乏可验证性：如果只检查最终文本回复或单次工具调用，就很难区分“这次碰巧成功”和“稳定可靠地完成任务”。论文因此提出“一次成功不是可靠性”（One Success Isn't Reliability）这一核心命题，并尝试用沙箱加终端状态检查来度量这种可靠性差距。
简要来说，问题不是“智能体能不能调用工具”，而是“智能体能否在真实感的多轮、有状态、策略约束的业务流程中，可靠地产生正确的持久状态变化”。

Q2: 有哪些相关研究？

根据摘要和检索片段，论文把相关工作放在“可执行环境中的智能体评估”谱系中：
1. 代码修复类基准：把评估放到可执行环境，通过测试或程序状态判断修复是否成功。
2. 网页导航类基准：智能体在模拟浏览器中执行多步操作，评估目标页面或任务完成情况。
3. App API 与函数调用类基准：以 API 选择、参数生成或函数调用可执行性为核心。论文特别指出这类评估的局限：它们主要评估“调用是否正确”，而不是“调用之后世界状态是否正确”。
4. 多智能体/工具交互：论文把交互扩展为“工具-智能体-用户”三方，加入模拟用户、多轮信息收集和领域策略约束。
5. MCP（Model Context Protocol）兼容：沙箱提供 MCP 兼容工具会话，意味着与当前工具调用生态对接。
需要说明：检索证据只命中了少数引用名（Li et al., 2023；Patil et al., 2024；Qin et al.），这些工作具体对应哪些基准（如 ToolBench、API-Bank、τ-bench 等）并未在片段中展开。因此 related work 的细粒度对比需要回原文核对，当前归纳以摘要和引言片段为准。

Q3: 论文如何解决这个问题？

论文从三个层面解决问题：
1. 沙箱层（Thinkingbox）：构建一个可复用的“工具-智能体-用户”交互沙箱，核心设计包括：
 · 隔离的后端会话：每个智能体尝试运行在独立、隔离的 MCP 兼容工具会话中，避免状态污染，也保证可重复性。
 · 模拟用户：智能体不是直接面对工具，而是在对话中与一个模拟用户交互；用户可能初始提供不完整信息，需要智能体多轮追问。
 · 完整执行轨迹：记录所有工具调用、状态变更和交互消息，便于事后审计。
 · 副作用获取与结果评估：在任务结束后读取后端终态，而不是只看回复或工具调用本身。
2. 评估层（Thinkingbox-bench）：把沙箱实例化为 507 个可执行任务，覆盖五个领域（零售/电商、旅行与酒店、车险、新银行内部 IT、咨询 IT/HR 支持）。每个任务都是“策略条件化工作流”，即任务描述里包含领域策略约束。
3. 检查层（task-specific executable checks）：每个任务配有可执行检查脚本，对终态做三类判断：
 · 接受：正确、完整的持久状态转移；
 · 拒绝：状态转移错误、缺失或存在多余副作用；
 · 额外检查：部分任务还要求最终回复具备指定属性（例如必须包含某个确认信息或给出特定格式）。
 这种方法比“检查最终答案或单个工具调用”更严格，并且允许不同的合法工具调用路径得到分数，只要它们产生相同的正确世界状态；同时能捕捉附带副作用（例如误删数据、多扣款、重复下单）。
总体思路是：把“可靠完成任务”还原为“在隔离环境中产生正确的终态”，用可执行检查来裁决，而不是用自然语言或调用合法性来裁决。

Q4: 论文做了哪些实验？

根据摘要和检索到的结果片段，论文实验设计如下：
1. 基准规模：Thinkingbox-bench 共 507 个可执行的工具-智能体-用户任务。
2. 领域覆盖：五个实际业务领域——
 · 零售/电商（retail/e-commerce）
 · 旅行与酒店（travel and hospitality）
 · 车险（auto insurance）
 · 新银行内部 IT 支持（neobank internal IT support）
 · 咨询 IT/HR 支持（consulting IT/HR support）
3. 模型：覆盖闭源和开源权重模型（论文未在摘要中列出具体模型名，需原文确认）。
4. 评估指标：
 · pass@1：一次尝试即成功的比例；
 · pass^20（论文原文写法，推测为 20 次尝试内的通过率，具体采样协议需原文确认）；
 · 每次尝试由任务级可执行检查计分。
5. 失败分析：论文特别观察了失败尝试的模式，包括“干净终止但状态错误”和“无效状态变更但工具调用看起来合法”等。
需要指出：检索证据没有给出分领域、分模型的完整结果表，也没有给出消融实验细节。上面对实验设计的描述是基于摘要与结果片段的重构，完整实验矩阵需要查看原文 Experiments 章节。

Q5: 发现了什么实验现象？

论文最核心的实验现象是“发现-可靠性差距”（discovery-reliability gap）：
1. 最强模型 pass@1 为 65.36%，说明有相当一部分任务可以被“碰巧”完成；但 pass^20 只有 25.25%，意味着即使给 20 次机会，可靠性也明显下降。这一数字组合直接支持“一次成功不等于可靠性”的判断。
2. 很多失败尝试表现出“干净终止”（clean termination）和“有效状态变更动作”（valid state-changing actions）。也就是说，从表面信号看，模型正确结束了对话，也发出了合法的工具调用，但最终后端状态并不正确。这证明回复级或工具调用级信号不能作为端到端任务完成的代理。
3. 这种信号错位在真实业务场景中尤其危险：一个看似成功的客服会话可能实际没有完成退款、没有更新保单、没有创建工单，或产生了额外副作用。
4. 闭源与开源模型之间也存在性能差异，但摘要没有给出具体模型排名，论文只报告了“最强”模型的数字；跨模型分布和分领域差异需要原文补充。
可以合理推断，作者通过这些观察强调：未来智能体评估应该越来越依赖终态检查，而不是中间动作或最终文本。

Q6: 有什么可以进一步探索的点？

基于论文的定位和现有证据，可以提出以下可探索方向（含推测成分，需结合原文与后续工作）：
1. 训练侧：把 Thinkingbox 作为可执行环境引入强化学习或偏好优化，让模型直接优化“终态正确率”而非“回复似然”。
2. 可靠性与自验证：研究模型如何在执行过程中自我检查终态、检测副作用，并在失败时回滚或重试，从而缩小 pass@1 与 pass^20 之间的差距。
3. 评估方法论：把“基于终态可执行检查”的评估范式推广到更多领域（金融、医疗、法律、企业 ERP），并探索如何自动化生成检查器以降低人工成本。
4. 失败模式分析：深入分类“干净终止但状态错误”的具体原因（信息遗漏、策略误解、工具顺序错误、状态读取失败等），并针对每类提出可操作的模型能力要求。
5. 用户模拟改进：目前是模拟用户，未来可以引入更真实的用户模型（包括用户的错误输入、情绪、多意图、非规范表达），以及多用户/权限协作场景。
6. 安全与策略遵循：有状态业务任务中的“策略约束”是核心难点，未来可以专门研究策略推理、合规性检查与越权操作检测。
7. MCP 生态：Thinkingbox 兼容 MCP，未来可以接入更丰富的第三方 MCP 服务器，形成标准化工具评估协议。
8. 长尾可靠性：研究小模型、开源模型在同样基准上的失败模式，以及蒸馏、工具利用微调等干预手段带来的可靠提升。

Q7: 总结一下论文的主要内容

论文针对智能体评估中的一个关键盲区展开：现有基准越来越强调可执行环境，但大多仍把成功定义为“合理的回复”或“合法的工具调用”，而不是“系统终态正确”。作者指出，在代码修复、网页导航、API 调用等场景之外，真实业务工作流还要求智能体在多轮对话中主动收集缺失信息、遵循领域策略、协调多个依赖工具，并最终实现正确的持久状态迁移且不留副作用。为了给这类任务提供可验证的测试平台，论文提出 Thinkingbox 沙箱，支持隔离的 MCP 兼容工具会话、模拟用户对话、完整执行轨迹记录和基于后端终态的结果评估。评估不是看单步动作，而是用任务级可执行检查判断终态是否正确、缺失还是多余，部分任务还会额外检查最终回复的指定属性；这种设计允许不同工具调用路径获得分数，只要最终世界状态一致，同时能捕捉无关副作用。基于该沙箱，作者构建了 Thinkingbox-bench，共 507 个策略条件化工作流，覆盖零售/电商、旅行与酒店、车险、新银行内部 IT、咨询 IT/HR 支持五个领域，测试闭源与开源模型。核心结果是：最强模型 pass@1 为 65.36%，但 pass^20 仅为 25.25%；更重要的是，大量失败尝试表现为“干净终止+合法状态变更动作”，即表面信号正常而终态错误，说明回复级和工具调用级信号无法作为端到端任务完成的代理。这一“发现-可靠性差距”表明，当前模型更擅长偶尔找到一条成功轨迹，而非可靠地完成有状态业务任务。论文最后强调，Thinkingbox 与 Thinkingbox-bench 作为可复用沙箱和可执行基准开源，希望推动智能体评估从“输出正确”转向“状态正确、流程可靠”。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：这篇论文与你画像中的“agent”方向直接相关（权重 0.10），尤其是智能体评估、工具使用、多轮决策可靠性。

## 基本信息

- 作者：Zhuochun Li, Youngmin Ko, Ali Keramati, Nicola Ferri, Susana Palmaz Lopez Pelaez, Liang-Chun Tsai, Calvin Wang, Mirco Milletari, Tuhin Kundu, Vadim Smolyakov, Kjartan Olafsson, Tommy Guy
- 机构：微软（Microsoft）团队（由开源仓库 https://github.com/microsoft/thinkingbox 合理推断；论文元数据未直接提供作者机构）。
- 来源：arxiv
- 主题/分类：cs.CL, cs.DB
- 日期：2026-08-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.19741`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract、Introduction 和 Conclusion 片段；heuristic_draft 中的图片路径等噪声已忽略，部分细节（如具体模型名、完整结果表、Limitations 章节）因证据不足已在相应字段标注为推断或需原文确认。
