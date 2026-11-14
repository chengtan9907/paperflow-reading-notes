---
user_id: "cheng tan"
paper_id: 21500
arxiv_id: "2611.14364v1"
title: "ARE-bench: Benchmarking Agents' Research Ability in Real-World Researcher Sessions and Workspaces"
publish_date: "2026-11-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.14364v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.14364v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-14T17:43:31"
---
# ARE-bench: Benchmarking Agents' Research Ability in Real-World Researcher Sessions and Workspaces

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：research agents · agent benchmark · workspace state · session context

## 一句话总结

论文提出 ARE-bench，用 75 个来自真实研究者项目中途的请求、可重放的真实工作区和 1,173 条二元 checklist，评估研究型 agent 能否在尊重既往会话决策与工作区现状的前提下推进项目，并报告最强模型仅 66.2% 任务全项通过、去掉历史与材料/工作区要求会把成绩高估到约 69%。

## 摘要

> Research agents achieve increasingly strong benchmark scores, yet these scores do not necessarily translate into reliable delivery in ongoing research projects. Many research-agent benchmarks emphasize standalone tasks and relatively self-contained workflows in clean or curated workspaces. However, researchers ask agents to complete one step within an ongoing project, where requests depend on earlier session decisions and workspaces contain unfinished files, outdated versions, and failed attempts. We introduce ARE-bench to evaluate whether research agents advance ongoing projects while respecting prior decisions and existing workspace state, grounded in real session context and in-progress workspaces. ARE-bench includes 75 tasks built on agent sessions and full workspaces from recruited researchers' own projects, spanning seven research stages, 33 requirements, and 150 sub-requirements, with 1,173 binary checklist items in total. Each task is anchored at a real user request, preserves its requirements, and summarizes the earlier requests still in effect as background. The complete workspace is replayed to that boundary without cleanup, preserving artifacts from earlier turns. Each checklist is compiled from an intent-aligned rubric and credits any valid solution. Across seven frontier models, the strongest passes every checklist item on only 66.2% of tasks, and Design and Execution rank among the hardest stages for every model. Three models pass about 69% of tasks when only delivery contracts and general norms are checked, but just 28.4% to 40.0% once historical and material or workspace requirements are also checked. Evaluating delivery without these contextual requirements can therefore overestimate research agents and mask differences between them.

Q1: 这篇论文试图解决什么问题？

1. 论文试图解决的问题可以概括为：研究 agent 的基准分数与真实科研项目中的可靠交付之间存在构念错位。现有基准大多把研究能力拆成独立任务，例如 Kaggle 竞赛、研究工程任务或整篇论文复现；这些任务通常有相对自足的规格、干净或精选的起始工作区，成功标准也偏向最终产物。真实研究者更常见的请求是项目中的一步：修一个失败运行后的脚本、给论文加一张表、调整一个实验配置。这个请求是否做对，不只取决于当前指令，还取决于此前会话中已经做出的决策，以及工作区中未完成文件、过时版本、失败尝试和依赖关系。

2. 因此，论文把“研究能力”重新定义为在 session 连续性和 workspace 状态约束下的下一步交付能力。关键约束有三类：其一是历史要求，即早期请求中仍然生效的决策；其二是材料/工作区要求，即现有文件、版本、失败记录和依赖所规定的边界；其三是当前交付契约，即本次请求要求的具体产物和验收条件。只满足第三类，可能得到“看起来能跑”但违反项目约束的结果。

3. 论文要回答的研究问题包括：前沿模型能否完成这种中途请求；哪些研究阶段最难；当评价从“交付契约与一般规范”扩展到“历史与材料/工作区要求”时，模型表现如何变化；成本与上下文规模是否带来收益。这些问题共同指向一个评价设计问题：如果 benchmark 不显式检查上下文要求，分数会不会系统性高估 agent，并掩盖模型之间的真实差异。

4. 论文的隐含假设也值得注意：它假设单个 cut-point 请求能够代表项目中的下一步；假设工作区重放和依赖恢复足以还原原始上下文；假设 intent-aligned rubric 能稳定编译成二元 checklist；假设任何有效解都可以被 evaluator 识别。这些假设任何一个不成立，都会影响构念效度。尤其是 replay 和 anonymization，可能丢失原始研究者认为理所当然的线索；rubric 和 checklist evaluator 也可能漏证或对“看似合理”的声明过度给分。

5. 从失败模式看，论文关心的不是单纯的代码错误，而是“局部正确、全局违规”：agent 可能让脚本重新运行，却改变坐标处理、重叠标签或匹配损失，从而不再符合工作区中参考论文的要求；也可能忽略失败尝试中已经排除的路线，或覆盖过时但仍被引用的文件。这类失败在只检查最终答案的 benchmark 中很难暴露。

6. 最后，论文的问题设定具有评价范式竞争意味：是继续用独立任务和最终产物衡量研究 agent，还是转向状态化、上下文敏感、可审计的交付评估。ARE-bench 选择后者，并用二元 checklist 和 ISR/CSR 把“严格完成”与“部分合规”分开。其贡献不只是一个新榜单，更是把“项目中途状态”作为一等评价对象。

Q2: 有哪些相关研究？

1. 论文引言明确把既有研究 agent 基准分为几类：以 Kaggle 竞赛为代表的独立数据科学任务（Chan et al., 2024，通常指 MLE-bench 一类）；以研究工程为代表的基准（Wijk et al., 2024，RE-Bench 一类）；以及以整篇论文复现为代表的基准（Starace et al., 2025，PaperBench 一类）。这些基准的共同点是目标相对 standalone，起始状态相对干净或经过整理，成功标准偏向最终结果或复现程度。

2. 论文特别指出 PaperBench Code-Dev 的边界：它评估代码开发，但不检查执行，也不检查论文实证结果是否被复现；因此即使分数接近上限，仍有重要研究交付维度未被测试。这一观察是 ARE-bench 的问题起点：高分不等于真实项目中的可靠交付。

3. 与软件工程 agent 基准的关系：SWE-bench 一类工作评估仓库级 issue 修复，强调代码、测试和仓库状态；这与 ARE-bench 的 workspace 约束有相似之处，但 ARE-bench 的任务来自研究项目会话，要求同时理解研究意图、历史决策和材料约束，而不只是修一个 issue。此段为合理推断，论文可见片段未逐一展开 SWE-bench。

4. 与长时程/状态化 agent 基准的关系：OSWorld、WebArena、WorkArena 一类环境评估 agent 在真实软件或网页中的多步操作，强调状态转移和工具使用；ARE-bench 的差异在于任务锚定研究者的真实请求，评价的是研究交付而不是通用电脑操作。此段为相邻工作定位，不是论文原文的完整 related work。

5. 与 rubric/checklist 评价的关系：PaperBench 使用 rubric 评估复现，ARE-bench 把 intent-aligned rubric 编译成二元 checklist，并允许任何有效解，不把原始轨迹当作 gold answer。这使它更接近“契约合规”评估，而不是轨迹匹配。相关范式还包括 LLM-as-a-judge、agent-as-a-judge 和过程级验证，但这些在可见片段中没有被系统比较。

6. 与 AI-for-Science agent 的关系：自动机器学习、数据科学 agent、实验设计 agent 都试图让模型参与科研流程；ARE-bench 把研究流程拆成 Survey、Design、Data & Env.、Code、Execution、Analysis、Writing 七个阶段，并用 33 项需求、150 项子需求刻画任务。这比“能否复现一篇论文”更细，也更贴近项目中的单步协作。

7. 研究空白可以概括为：既有基准缺少 session 连续性、真实 in-progress workspace、历史/材料要求这三者的联合建模。ARE-bench 的定位不是替代所有研究基准，而是补上“项目中途下一步”这一层。论文的 related work 在可见片段中不完整，以上分类结合了引言点名、基准设计与相邻领域知识；具体引用清单需回原文核对。

Q3: 论文如何解决这个问题？

1. 总体设计：ARE-bench 的核心单元不是一篇论文或一个独立竞赛，而是一个真实研究会话中的 cut-point 请求。作者招募研究者，使用其自己项目中的 agent 会话和完整工作区，抽取 75 个任务。每个任务锚定一个真实用户请求，保留该请求的需求，并把仍然生效的早期请求概括为背景。

2. 任务 brief 的结构：可见证据显示任务包含 Background、Requirements、Environment 三部分。Background 概括早期仍然生效的用户请求；Requirements 说明当前交付物与验收条件；Environment 说明可用资源与限制。这个结构把“历史上下文”和“当前契约”分开，使 actor 得到自足且可审计的规格，同时不直接给出原始轨迹。

3. 工作区重放：论文把完整工作区重放到 cut point，且不做清理，保留此前轮次产生的未完成文件、过时版本和失败尝试。这样 actor 面对的是 in-progress research state，而不是 curated starting state。论文还进行 replay checks 和 dependency recovery，以尽量恢复依赖；但重放和恢复可能丢失原始研究者可用的上下文，这是设计上的固有权衡。

4. 任务分类体系：75 个任务覆盖 7 个研究阶段：Survey、Design、Data & Env.、Code、Execution、Analysis、Writing；共 33 项需求、150 项子需求。每条 checklist item 还带一个子需求标签，任务的需求画像由这些 item 的父需求并集构成。这个 taxonomy 既用于覆盖度控制，也用于分阶段报告。

5. Checklist 构建：每条 checklist 由 intent-aligned rubric 编译而来，共 1,173 条二元项。评分原则是“任何有效解都给分”，不要求与原始轨迹逐字一致；这避免了把原始答案当作唯一 gold，但也把很大责任放在 rubric 和 evaluator 上。论文还保留 rubric 的原始措辞和 provenance package，以支持审计。

6. 评价协议：actor 收到 instruction 和 restored workspace，交付物按 task-specific binary checklist 评分。ISR 用于任务级严格完成（通过全部 checklist 项），CSR 用于 checklist 级部分合规；二者配合可以区分“全对”与“做了大部分”。论文另设成本指标，并在三轮运行中报告样本标准差。

7. 控制与发布：论文使用 replay checks 和匿名化，避免把原始轨迹当作 gold answer；发布协议支持在模型运行可用时填充结果 schema，同时保持 benchmark 定义固定。每个模型记录 archived model identifier 和 actor harness；主实验中 GPT-6.1-Sol 使用 Codex，其余模型使用 Claude Code。

8. 设计权衡：ARE-bench 用二元 checklist 换取可审计性和跨模型可比性，但牺牲了对质量、优雅性和科学洞察的细粒度区分；用 cut-point 请求换取真实性和可评分性，但牺牲了对完整多轮项目的覆盖；用工作区重放换取状态真实性，但引入上下文丢失和依赖恢复误差。这些权衡是阅读时需要重点核对的地方。

Q4: 论文做了哪些实验？

1. 实验设置：主实验包含 75 个任务，其中 67 个 CPU 任务、8 个 GPU 任务；1,173 条 checklist 项覆盖 7 个研究阶段。任务资源按任务配置，CPU 任务运行在 E2B sandboxes 或 Docker containers，GPU 与研究任务使用 Docker；表中记录 2–16 个 CPU、内存和 GPU 配置，以及 actor time limits。GPU/research batches 使用 Codex 0.118.0 或 Claude Code 2.1.283。

2. 被评估模型：七个前沿模型，包括 GPT-6.1-Sol、Claude Opus 5.5、DeepSeek-V4.1-Flash、Qwen3.8-Max、GLM-5.3-Flash、Kimi-K3、Qwen3.8-27B。GPT-6.1-Sol 使用 Codex harness，其余六个使用 Claude Code harness。这个安排在解释模型差异时是一个潜在混杂因素：模型能力与 scaffold 效应没有完全分离。

3. 评分流程：actor 收到 instruction 和 restored workspace；交付物按 task-specific binary checklists 评分。论文报告 ISR（任务级严格完成，需通过全部 checklist 项）和 CSR（checklist 级部分合规），并报告成本指标。主结果平均 over three rounds，图 1 给出 sample standard deviation across rounds 的误差棒。

4. 分析维度：整体 ISR；按研究阶段拆分（Survey、Design、Data & Env.、Code、Execution、Analysis、Writing）；上下文要求消融——只检查 delivery contracts 和 general norms，对比同时检查 historical 和 material/workspace requirements；成本分析 RQ4，考察更大上下文和更高成本是否带来更好表现。可见片段还展示了具体失败案例，例如仅恢复执行但改变坐标处理、重叠标签和匹配损失，导致 checklist 只通过 0/20 或 16/20 之类的结果。

5. 基线与对照：主实验是七个前沿模型之间的比较，没有在可见证据中看到人类研究者基线、随机 baseline 或非 agent 的强规则 baseline。Qwen3.8-27B 可以视为较小模型参照，但模型规模、harness 和训练数据都未控制。论文的对照主要是评价条件之间的对照（是否检查上下文要求），而不是方法模块的消融。

6. 统计与可复现性：三轮运行提供方差信息，但可见片段没有报告显著性检验、评分者间一致性或 evaluator 的校准结果。任务数 75，GPU 任务仅 8 个，阶段级细分后每格样本可能很小。论文提供 archived model identifiers、harness 版本和 release protocol，但模型版本与 2026 年日期属于归档设定，当前公开资料难以核验。

7. 实验设计的关键张力：为了让任何有效解都能得分，评分依赖 rubric 和 evaluator 的判断；为了保持真实性，工作区不做清理，但这会让任务之间共享大量历史 artifact，增加泄漏和混淆风险。论文用 replay checks 和 anonymization 控制这些风险，但无法完全消除。整体上，实验更像“真实条件下的系统评测”，而不是控制变量严格的机制实验。

Q5: 发现了什么实验现象？

1. 总体严格完成率不高。论文报告最强模型也只在 66.2% 的任务上通过全部 checklist 项；可见图 1 还出现约 25.8% 的低值，七个模型的总体 ISR 有明显跨度。由于图注重排，不能可靠地把每个数值绑定到具体模型，但“最强也未接近满分”这一结论是明确的。这说明真实项目中途请求对前沿 agent 仍有相当难度。

2. 阶段难度高度不均。论文明确指出 Design 和 Execution 对每个模型都是最难阶段之一；Survey 可能相对容易，Writing、Analysis、Data & Env.、Code 位于中间。这个现象符合直觉：设计和执行需要同时理解研究意图、历史决策和材料约束，而 survey 或写作更接近文本生成。不过阶段级数值受任务数和任务难度分布影响，不能直接解释为能力排序。

3. 最重要的现象是上下文要求带来的巨大评分落差。三个模型在只检查 delivery contracts 和 general norms 时约有 69% 的任务通过，但一旦同时检查 historical 和 material/workspace requirements，就降到 28.4%–40.0%。这意味着模型往往能完成“显式合同”，却无法遵守“项目隐性契约”：早期决策、文件版本、失败尝试和依赖关系。只看交付契约的评测会系统性高估研究 agent，并压缩模型之间的差异。

4. 严格完成与部分合规之间存在张力。ISR 要求全部 checklist 项通过，因而对“接近完成”很严厉；CSR 则显示部分合规。论文把二者并列，说明仅看平均 checklist 通过率可能掩盖“最后几项关键要求没做”的失败模式。真实科研中，最后一两项往往正是最关键的约束，例如不能改变某个坐标处理或损失函数。

5. 定性案例揭示“局部正确、全局违规”。可见片段提到，仅恢复执行并不等于成功：某个交付改变了坐标处理、重叠标签和匹配损失，而这些是工作区中参考论文所要求的；结果 checklist 通过数很低。另一个案例涉及 training、delivery、archived review、checkpoints、validation 和 report recovery，说明 agent 需要区分哪些 artifact 仍然有效、哪些是过时版本。这类失败不是代码语法错误，而是项目状态理解错误。

6. 模型排名可能不稳定。论文说去掉上下文要求会“mask differences between them”，暗示在窄口径下多个模型都接近 69%，看起来差不多；加入上下文要求后差距才显现。与此同时，GPT-6.1-Sol 使用 Codex、其余模型使用 Claude Code，harness 差异可能放大或缩小模型差距。因此，模型排序应谨慎解读，最好等待同 harness 的对照实验。

7. 成本与上下文的关系尚不能下结论。论文设有成本分析和 RQ4，问题是更大上下文和更高成本是否带来更好表现；但可见证据只显示存在该分析，没有给出趋势。不能据此断言“上下文越长越好”或“成本越高越好”。这是一个需要回原文 4.5 节核对的关键点，也可能是负结果或非单调趋势所在。

8. 与既有高分基准的对比构成一个反直觉现象：Qwen3.8-Max 在 PaperBench Code-Dev 上报告 93.0%，但在 ARE-bench 上并非同样接近满分。论文用这个例子说明，不检查执行和复现的 code-dev 评分会留下重要维度未测。这个对比不是严格的可比实验，但有效地揭示了 benchmark 构念差异。

9. 方差与显著性信息不足。三轮运行和误差棒表明作者重视随机性，但可见片段没有给出标准差数值、置信区间或显著性检验。对于 75 个任务、8 个 GPU 任务的规模，阶段级差异和模型间小差距可能不稳定。阅读时应优先看整体趋势和上下文消融的巨大落差，而不是过度解读小数差。

10. 负面/失败模式的价值大于平均分。论文的核心贡献不是证明某个模型更强，而是暴露“交付契约合规”与“项目上下文合规”之间的鸿沟。这个鸿沟对 agent 设计有直接含义：需要项目记忆、决策日志、工作区状态建模、回归检查和显式的历史要求验证。

Q6: 有什么可以进一步探索的点？

1. 扩展任务覆盖与纵向研究。当前 75 个任务、7 个阶段、8 个 GPU 任务，规模仍有限；未来可以覆盖更多学科、更多实验室文化、更多工具链和更多语言，并把 cut-point 任务扩展成真正的多轮纵向项目，观察 agent 的下一步行动是否最终转化为项目成功。

2. 提升 replay 保真度与上下文恢复。工作区重放和依赖恢复可能丢失原始研究者可用的上下文，匿名化也可能移除重要线索。未来可以研究哪些上下文对任务成功最关键，建立 replay 完整性的度量，做“完整重放 vs 精简重放”的消融，并记录恢复失败对评分的因果影响。

3. 改进 rubric 与 evaluator 的可靠性。rubric 写作仍是人工判断，checklist evaluator 可能漏证或过度给分。未来可以报告评分者间一致性、evaluator 校准、对抗性 checklist、证据引用要求和不确定性估计；也可以比较 LLM judge、规则 verifier 和人类专家评分的差异。

4. 分离模型能力与 scaffold 效应。当前 GPT-6.1-Sol 使用 Codex，其余模型使用 Claude Code，模型与 harness 混杂。未来应在同一 harness 下交叉评估所有模型，或做 model × harness 的因子实验，量化 scaffold、工具权限、上下文管理策略各自的贡献。

5. 建立上下文要求的分类学与 agent 记忆机制。历史要求、材料要求、工作区要求、交付契约可以进一步细分为可操作的类别；对应地，agent 需要项目记忆、决策日志、版本图、失败尝试索引和回归检查。未来可以研究“显式契约检查”“变更影响分析”“历史决策检索”等模块的增益。

6. 从 checklist 奖励到训练信号。二元 checklist 可以转化为 RL 奖励、过程监督或 verifier，但需要防止 reward hacking：agent 可能学会迎合 checklist 而不是真正推进项目。未来可以研究多轮验证、对抗性 rubric、留出 checklist 项、以及训练后在新项目上的迁移。

7. 成本、上下文与 scaling 规律。论文设置了成本分析和 RQ4，但可见证据没有给出趋势。未来可以系统研究上下文长度、token 预算、工具调用次数、wall-clock 时间与成功率之间的关系，寻找 Pareto 前沿，并区分“更多上下文有帮助”与“更多上下文引入噪声”的边界。

8. 人类基线与专家研究。当前可见证据没有报告人类研究者基线。未来可以招募专家在同样任务上操作，测量人类的时间、成本和成功率，建立难度校准；也可以做专家反思访谈，提炼 agent 失败模式与人类策略的差异。

9. 动态与私有 held-out 评测。静态公开 benchmark 存在污染风险；任务和 rubric 公开后，模型可能被针对性优化。未来可以维护私有 held-out 集、滚动更新任务、使用实时项目会话，并研究公开集与私有集上的排名一致性。

10. 多轮交互与澄清。当前任务以单个 cut-point 请求为单位，接近“给指令后一次交付”；真实项目允许提问、澄清和迭代。未来可以评估 agent 在不确定时是否主动询问、如何更新计划、如何在多轮中保持决策一致性。

11. 从下一步成功到项目结局。ARE-bench 测的是局部下一步，不是项目最终成功。未来可以追踪任务之后的项目演化，研究局部合规是否累积为可复现、可发表的研究成果，以及哪些早期决策对长期结果影响最大。

12. 跨领域与跨任务迁移。研究 agent 的能力是否可迁移到软件维护、数据科学、实验科学、社会科学等不同工作流，仍待验证。未来可以做跨领域任务套件，比较同一 agent 在代码密集、数据密集、写作密集任务上的差异。

13. 安全、隐私与数据治理。真实工作区可能含未发表数据、凭证、个人身份信息。未来需要研究匿名化与可用性的权衡、权限最小化、审计日志、以及研究者 consent 和数据撤回机制。

14. 评估理论本身。ARE-bench 提出了“交付契约 vs 上下文要求”的区分，未来可以进一步形式化构念效度、生态效度和 consequential validity，比较 outcome-based、process-based、contract-based 三种评估范式的优劣，并建立多指标报告规范。

Q7: 总结一下论文的主要内容

1. 论文的主线可以概括为“从独立任务分数转向项目中途交付”。作者首先指出，研究 agent 在 PaperBench 等基准上的高分并不能说明它们能在真实研究项目中可靠工作：现有基准多为 standalone 目标，工作区干净或经过整理，成功标准偏向最终产物；真实研究者却常常只要求 agent 完成项目中的一步，而这一步依赖此前会话决策和工作区中未完成、过时或失败的 artifact。论文用 Qwen3.8-Max 在 PaperBench Code-Dev 上 93.0% 的例子说明，即使接近上限的 code-dev 分数，也可能没有测试执行与实证复现等关键维度。

2. 为此，作者构建 ARE-bench：从招募研究者的真实 agent 会话和完整工作区中抽取 75 个任务，覆盖 Survey、Design、Data & Env.、Code、Execution、Analysis、Writing 七个阶段，共 33 项需求、150 项子需求和 1,173 条二元 checklist。每个任务锚定一个真实用户请求，任务 brief 包含 Background、Requirements、Environment：Background 概括仍然生效的早期请求，Requirements 给出当前交付物与验收条件，Environment 说明资源与限制。

3. 技术上的关键设计是工作区重放。论文把完整工作区恢复到 cut point，且不做清理，保留未完成文件、过时版本和失败尝试，使 actor 面对真实的 in-progress research state。每条 checklist 由 intent-aligned rubric 编译而来，评分允许任何有效解，不把原始轨迹当作唯一 gold answer；同时用 replay checks 和匿名化控制泄漏与隐私。评价协议让 actor 收到 instruction 和 restored workspace，交付物按 task-specific binary checklist 评分，ISR 衡量任务级严格完成，CSR 衡量 checklist 级部分合规，并配套成本指标。

4. 实验评估七个前沿模型：GPT-6.1-Sol、Claude Opus 5.5、DeepSeek-V4.1-Flash、Qwen3.8-Max、GLM-5.3-Flash、Kimi-K3、Qwen3.8-27B。GPT-6.1-Sol 使用 Codex，其余使用 Claude Code。主实验 75 个任务中 67 个 CPU 任务、8 个 GPU 任务；CPU 任务运行在 E2B 或 Docker，GPU/研究任务使用 Docker；结果取三轮平均并报告样本标准差。

5. 主要发现是：最强模型也只在 66.2% 的任务上通过全部 checklist 项；Design 和 Execution 对每个模型都是最难阶段之一；三个模型在只检查交付契约与一般规范时约有 69% 任务通过，但加入历史与材料/工作区要求后降至 28.4%–40.0%。这说明模型能完成显式合同，却常常违反项目隐性契约；窄口径评测会高估 agent 并掩盖模型差异。定性案例进一步显示，仅让代码重新运行并不等于成功，改变坐标处理、重叠标签或匹配损失都会造成“局部正确、全局违规”。

6. 论文的贡献不只是新榜单，而是把“项目中途状态”作为一等评价对象：它区分交付契约与上下文要求，区分严格完成与部分合规，并用可重放工作区、intent-aligned rubric 和 provenance package 支持审计。其局限也很明确：来源会话可能过度代表某些项目、工具和研究者群体；cut-point sampling 不保证覆盖所有科研流程；replay 和匿名化可能丢失上下文；rubric 仍是人工判断，checklist evaluator 可能漏证或过度给分；75 个任务、8 个 GPU 任务的规模有限；模型与 harness 混杂；可见证据未报告人类基线和显著性检验。

7. 总体而言，论文的论证主线是“真实项目中的下一步请求是研究 agent 的关键测试场景”，技术主线是“cut-point 任务 + 结构化 brief + 工作区重放 + 二元 checklist”，实验主线是“七模型总体与分阶段评测 + 上下文要求消融 + 成本分析”。结论是：脱离历史与工作区上下文评估交付，会系统性高估研究 agent；要让 agent 真正推进项目，需要项目记忆、状态建模、历史决策遵守和回归检查，而不仅是更高的 standalone benchmark 分数。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent 评测直接相关：它把 session context 和 workspace state 纳入评测，补足静态、单轮 benchmark 的生态效度缺口。

## 基本信息

- 作者：Jiahang Lin, Wanying Ren, Wanghan Xu, Huayu Sha, Yang Shu, Zhonghang Lu, Chengjun Pan, Yi Dong, Shichun Liu, Yuyang Hu, Shihan Dou, Honglin Guo, Huijie Lv, Pujun Zheng, Zhe Sun, Yuankai Ying, Yiding Wang, Jiahao Chen, Zhixiong Yang, Dingbo Yuan, Yicheng Zou, Zhecan Wang, Guoxiu He, Xiaoyu Xing, Qi Zhang, Tao Gui, Xuanjing Huang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-11-10
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.14364v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Results、Conclusion 片段，并据此补全；heuristic_draft 中 research_background、core_method 等字段明显是图注或表格残片拼接，未作为事实依据；论文的 2026 年日期与模型版本属于其归档设定，具体指标定义和数值仍需回原文核验。
