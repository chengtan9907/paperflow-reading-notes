---
user_id: "cheng tan"
paper_id: 12268
arxiv_id: "2609.22068"
title: "CodeMidas: Scaling Agentic Coding RL Environments from Code Itself"
institution: "推测为小米（MiMo 团队）：依据是论文使用 MiMo-V2.5 作为基座模型、作者名单中包含 MiMo 相关研究者；元数据 institution 字段为空，且本次未获取 PDF 首页，机构归属需以原文作者单位标注为准。"
publish_date: "2026-09-21"
pdf_url: "https://arxiv.org/pdf/2609.22068"
abs_url: "https://arxiv.org/abs/2609.22068"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-22T00:07:31"
---
# CodeMidas: Scaling Agentic Coding RL Environments from Code Itself

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · coding agent · rl environment · agentic pipeline

## 一句话总结

CodeMidas 提出一条以源码为唯一任务特定输入的智能体流水线，把开源代码库中「已经实现的功能」反向转化为可执行、可自动验证的强化学习环境（产出 5,545 个训练任务、覆盖 3,185 个仓库、23 种语言、15 个技术领域），并用 GRPO 训练 MiMo-V2.5，在五个 coding agent 基准上全面提升（DeepSWE +11.7%、ProgramBench +17%、Terminal-Bench v2.1 +8.5%）。

## 摘要

> Training capable coding agents via reinforcement learning (RL) requires diverse tasks with reliable verifiers. Open-source codebases offer a rich source of such tasks, while existing methods typically rely on development artifacts such as issues and commits, limiting the range of tasks that can be extracted. To better scale RL environments, we present CodeMidas, an agentic pipeline that turns implemented functionality in existing codebases into executable RL environments using source code as its only task-specific input. CodeMidas allocates agentic compute to every stage of environment construction: agents explore implemented functionality to formulate behavioral specifications, construct tests grounded in execution of the original code, and validate and filter candidate tasks through execution checks and repeated solution rollouts. The resulting dataset has 5,545 training tasks from 3,185 open-source codebases spanning 23 programming languages and 15 technical domains. Training MiMo-V2.5 on these tasks with GRPO improves performance on all five diverse benchmarks, covering issue repair (DeepSWE + 11.7%), whole-program construction (ProgramBench +17%), and terminal work (Terminal-Bench v2.1 +8.5%). Ablations show that increasing the number of high-quality training tasks improves performance. Trajectory analysis shows the RL-trained agent demonstrates better behaviors like increasing codebase exploration and more diverse self-verification. These results establish source code as a scalable foundation for constructing RL environments that improve coding agents across diverse software tasks.

Q1: 这篇论文试图解决什么问题？

【1｜核心问题】论文要解决的不是「RL 算法不够好」，而是「RL 环境供给不足」：训练 coding agent 需要大量任务 + 可靠 verifier，而现有环境构造高度依赖开发过程副产物。

【2｜现有范式的具体瓶颈】以 issue/PR/commit 为任务源（如从 issue 描述 + 修复补丁构造任务）的范式，其可用任务集合等于「被显式记录过的开发事件集合」。由此产生的缺口（合理推断）：
- 覆盖偏斜：大量已实现、稳定、从未开过 issue 的功能完全无法出题；活跃讨论、维护良好的仓库被过度采样，长尾库被遗漏。
- 语言/领域偏斜：issue 驱动的生态以主流语言与 Web/后端项目为主，23 种语言、15 个领域这样的广度难以达到。
- 任务类型偏斜：issue 天然偏向 bug 修复（局部编辑），对「整程序构建」「终端操作」这类需要长程探索与自验证的任务供给不足。
- 数据时效与依赖：需要 issue 文本质量、补丁可复现性、以及历史环境可重建性。

【3｜第二个难点：验证器可靠性】把源码本身当任务源，意味着要自己造 oracle。合成测试若与真实语义不符，RL 会朝错误方向优化（reward hacking）；测试若只编码了实现细节，又会惩罚等价但不同的正确实现。论文的对策（摘要层面）是双重把关：测试必须 grounded 在原始代码的执行上，候选任务再经执行检查 + 重复解 rollout 过滤。

【4｜第三个难点：难度可训练性】GRPO 类方法依赖组内奖励方差：任务过易（全对）或过难（全错）都不产生有效梯度。因此「验证与筛选」不只是去伪，也是在做难度整形——这一点摘要未明说，属于合理推断。

【5｜隐含假设与边界】
- 假设 A：源码自身足以恢复可验证的行为规范，无需自然语言规格或人工标注。
- 假设 B：「已实现的功能」是有效监督信号——即当前实现即正确语义。这对存在已知 bug 或已废弃行为的代码不成立，属于该范式的结构性边界。
- 假设 C：以原始执行为 oracle 的测试，其判别力可以迁移到「修改后代码」的评估场景。
- 边界张力：训练任务来自「已实现功能的规格化」，但评测基准之一是 issue 修复（DeepSWE），二者分布不同；跨分布仍提升这一点本身就是论文最值得检验的论断（详见 experimental_observations）。

Q2: 有哪些相关研究？

【说明】本次未检索到 PDF 正文，无法确认论文实际引用的文献清单；以下为基于摘要所指问题域整理的相关研究脉络，均为合理推断/推测，需回原文核对 Related Work 章节。

【1｜issue/commit 驱动的 SWE 任务构建】SWE-bench 及其衍生（SWE-bench Verified、SWE-bench-extra、SWE-Gym、R2E-Gym 等）从真实 issue + PR 构造可执行评测/训练集；SWE-smith 一类工作通过合成 bug 扩充任务量；Commit0 则从「从零实现一个库」的角度构造任务。CodeMidas 的定位与这些工作形成正面对照：同属「从开源仓库造可执行任务」，但把任务特定输入从 issue/commit 收窄到「只有源码」。

【2｜测试生成与作为 oracle 的测试】自动化测试生成（如基于 LLM 的单测生成、覆盖率驱动的补充测试、变异测试驱动的测试增强）是 CodeMidas 的关键上游技术；两者交集在于「用执行而非用文本判断测试是否有效」。差分/变异测试思想（测试应在缺陷版本上失败、在原版上通过）很可能被用于候选任务筛选，但摘要未直接提及，属推测。

【3｜代码上的 RL 与可验证奖励】从 CodeRL、StepCoder 一类的执行反馈 RL，到更近的 SWE-RL、以及基于单元测试通过率做奖励的 RLVR 范式；GRPO（Group Relative Policy Optimization）作为本文使用的优化器，其组内相对优势设计天然要求同一任务可多次采样且有区分度，这直接解释了 CodeMidas 为何强调「重复解 rollout」过滤。

【4｜Agent RL 环境与终端/长程任务】Terminal-Bench 一类终端 agent 基准、以及各类可验证 sandbox 环境（Web/OS/工具使用）构成「agentic RL environment」这一更宽的研究方向。CodeMidas 的贡献落点是把「环境构造」本身做成 agentic pipeline，与自动环境合成/任务合成这一类工作同属一条路线（推测论文会在此处做 positioning）。

【5｜评测基准侧的邻居】DeepSWE（issue 修复）、ProgramBench（整程序构建）、Terminal-Bench v2.1（终端任务）是本文选用的三个具名评测；另外两个「diverse benchmarks」摘要未点名，是阅读时需补充的信息缺口。

【6｜与本文最相关的差异点（推断）】相较依赖开发副产物的路线，CodeMidas 的价值主张是「任务供给面从『被记录的事件』扩展为『被实现的行为』」，理论上界更大；代价是失去 issue 文本提供的自然语言任务描述与人工验证过的修复补丁，必须自建 spec 与 verifier——这也正是其错误模式的主要来源。

Q3: 论文如何解决这个问题？

【总览】CodeMidas 是一条 agentic 流水线，输入是代码库源码，输出是可执行 RL 环境（任务 + 验证器）。核心设计原则有两条：(a) 任务特定输入只用源码，从而把可用仓库集合最大化；(b) 把 agentic compute（即让 agent 反复探索、执行、迭代的算力预算）分配到环境构建的每一个阶段，而不是只在某个单点用 LLM 生成一次。

【阶段 1｜功能探索与行为规范化】（摘要明确：agents explore implemented functionality to formulate behavioral specifications）agent 在仓库中探索代码，识别「已实现的功能单元」，并将其写成行为规范：该功能应当做什么、输入输出契约、边界与异常行为。这一步等价于从实现反向工程出「可检验的意图」，是本方法区别于「直接抄 issue 文本」的关键。

【阶段 2｜以执行为依据的测试构造】（摘要明确：construct tests grounded in execution of the original code）测试不是凭空生成后信任，而是通过真实运行原始代码来锚定：测试必须在原仓库环境下真实执行并得到预期行为。合理推断：这一步会包含「生成 → 运行 → 修正」的闭环，失败则回到规范或测试重新生成；这既提升测试正确率，也天然产生了一批「在原始代码上通过」的用例。

【阶段 3｜验证与筛选】（摘要明确：validate and filter candidate tasks through execution checks and repeated solution rollouts）两道关卡：
- 执行检查：验证任务环境可复现、测试可运行、判定逻辑自洽（合理推断：会检查测试在原始代码上通过、在受扰动/残缺代码上失败，以获得判别力）。
- 重复解 rollout：对同一任务多次采样求解，用通过率刻画难度与可解性；过高（无区分度）或过低（不可解/验证器有误）的任务会被过滤。这一步同时服务于「验证器可信度」与「GRPO 可训练性」。

【阶段 4｜环境打包与 RL 训练】通过筛选的候选任务固化为 RL 环境；使用 GRPO 在 MiMo-V2.5 上训练，得到最终 agent。

【设计取舍与代价】
- 用源码替代 issue：扩大供给面，但失去了人工验证过的任务描述与修复补丁，必须自建 spec/verifier，错误面转移到合成环节。
- 用原始执行做 oracle：降低对自然语言规范的依赖，但引入了「实现即规范」的偏置——测试可能固化实现细节，惩罚等价正确解（合理推断的风险，需原文验证其缓解手段）。
- 全阶段投入 agentic compute：质量换成本，论文披露了任务规模但未在摘要披露各阶段算力预算与单位任务成本（信息缺口）。

Q4: 论文做了哪些实验？

【1｜数据集构建实验/产物】训练数据集：5,545 个训练任务，来自 3,185 个开源代码库，覆盖 23 种编程语言、15 个技术领域。该规模与广度本身是流水线有效性的间接证据（对比 issue 驱动范式在语言/领域上的可达上限）。构建过程中的过滤比例、各阶段成功率、agentic compute 消耗，摘要未披露。

【2｜RL 训练设置】基座模型 MiMo-V2.5 + GRPO，在 CodeMidas 任务上训练。学习率、rollout 数、batch、上下文长度、是否含 SFT/拒绝采样冷启动等超参与配方，摘要未披露。

【3｜主评测（五个基准）】摘要明确点名的三个：
- DeepSWE（issue 修复）：+11.7%；
- ProgramBench（整程序构建）：+17%；
- Terminal-Bench v2.1（终端工作）：+8.5%。
另外两个基准未点名，仅表述为「五个多样化基准全部提升」，属信息缺口。评测协议、是否存在 pass@k 与多次运行方差、baseline 对照（未训练版本 / 其他数据配方）等细节均需回原文。

【4｜消融实验】摘要给出结论：增加「高质量训练任务」的数量会提升性能。这暗示存在质量门槛（先筛选，再扩量），但质量判据、数量-性能曲线形状、饱和点、以及不同来源/语言/领域的组成消融未在摘要披露。

【5｜轨迹分析】对 RL 前后 agent 行为做轨迹层面的比较，观察到两类变化：代码库探索增加、自验证方式更多样。这是行为指标而非仅分数指标，是本文相对纯 benchmark 论文的加分项；但摘要未给出量化定义（如何度量「探索」「自验证多样性」）与统计显著性。

【6｜可能的缺失实验（需核对）】验证器可靠性验证（人工抽检通过率、测试对变异版本/错误补丁的判别率）、训练任务与评测基准的去污染检查、单位性能提升的算力/成本对照、失败案例与负结果分析。以上均为本次证据缺失项，不代表论文未做。

Q5: 发现了什么实验现象？

【1｜跨任务类型迁移，且提升幅度不均】训练任务全部来自「已实现功能的行为规范化」，但评测覆盖三类差异很大的任务，说明增益不是同分布复制，而更像通用软件工程行为的改善（合理推断）。幅度排序值得注意：整程序构建（ProgramBench +17%）> issue 修复（DeepSWE +11.7%）> 终端工作（Terminal-Bench v2.1 +8.5%）。推测原因：ProgramBench 类任务最依赖「从零探索 + 自验证」，与 CodeMidas 注入的行为模式（探索、造测试、跑验证）最同构；终端任务还额外依赖与环境/命令行的交互能力，训练信号覆盖较少，故增益最小。该解释为推断，需原文的细粒度分析验证。

【2｜行为层变化先于/伴随分数变化】轨迹分析显示 RL 后 agent 探索更多代码、自验证更多样。这提示提升部分来自「策略行为改变」而非单纯知识注入——与 CodeMidas 的训练任务要求「读懂实现 → 写规范 → 写测试」相呼应。反过来说，也提出一个可证伪问题：如果只测量行为指标而奖励里没有显式鼓励探索，探索增加是任务的隐式要求所致还是泛化行为？

【3｜数量-质量 scaling 趋势为正】消融显示高质量任务越多越好。但摘要未给出曲线的形状与饱和行为，也未说明「高质量」的判定阈值——这决定了结论是「继续堆量仍有收益」还是「已接近收益递减」，是复现与落地决策的关键缺口。

【4｜潜在张力（需重点核对的负结果方向）】
- 实现细节过拟合：以原始执行为 oracle 的测试可能编码实现细节，理论上会惩罚等价但不同写法的正确解；若如此，出现「训练分数升但 issue 修复/多样性下降」的风险。摘要报告的是全基准上升，因此该风险至少在聚合指标上未显现，但细粒度指标（如不同解法的通过率）未被摘要覆盖。
- 「已实现即正确」假设的反例：包含已知 bug、废弃路径或平台特定行为的代码会产出错误规范，形成噪声任务。论文摘要未披露这类任务的占比与过滤效果。
- 分布错配：训练任务无 issue 文本，而 DeepSWE 的输入含 issue 描述，跨格式输入仍提升 +11.7%，说明增益可能主要来自「代码库交互与自验证」这类与输入模态无关的能力，但也可能被掩盖的是「读 issue 定位问题」这一子能力未提升（推断，未见证据）。

【5｜证据缺口清单（本次未获取 PDF，无法确认）】过滤掉多少候选任务、各阶段 agent 成功率、平均每个任务的构建成本、消融的完整表格、五个基准中未点名的两个、去污染措施、以及任何失败案例报告。

Q6: 有什么可以进一步探索的点？

【1｜质量-数量权衡的定量刻画】把「高质量任务数」作为自变量画完整曲线：拐点、饱和点、以及不同质量阈值下的边际收益。这直接决定该范式是「越多越好」还是需要更精细的课程设计。

【2｜验证器的鲁棒性研究】①变异测试式打分：测试是否能杀死语义等价的错误实现，又不误杀等价正确解；②多 oracle 交叉验证（执行行为 + 类型/契约检查 + 静态不变量）；③对抗性 rollout 主动搜索 reward hacking 解。这是该范式最脆弱的一环。

【3｜突破「已实现即正确」假设】把任务源扩展到未实现/待修正功能（feature request、TODO、废弃 API 的迁移），或引入多版本差分（同一仓库不同版本的行为变化作为规范信号）。这能在不依赖 issue 文本的前提下覆盖「新增能力」类任务。

【4｜难度课程与自适应采样】把 rollout 通过率作为难度信号，做在线课程（先易后难或保持组内方差最大），并研究其对 GRPO 稳定性与最终上限的影响。

【5｜成本模型与预算分配】各阶段 agentic compute 的边际收益如何？是「探索阶段更深」更值，还是「过滤阶段更多 rollout」更值？给出单位任务的 token/时间成本，才能与 issue 驱动范式做公平比较。

【6｜覆盖度与偏斜分析】23 种语言、15 个领域的任务分布是否长尾？低资源语言与非 Web 领域（编译器、数据库、科学计算、GPU kernel）的任务可得性如何？

【7｜数据配方与混训】CodeMidas 任务与 issue 型任务、通用代码 SFT 数据的最优配比；以及是否能叠加终端/工具使用专用环境以获得 Terminal-Bench 上的更大增益。

【8｜行为指标的因果化】把「探索度」「自验证多样性」做成可度量的过程奖励或筛选信号，验证行为改善是否是分数提升的中介变量（mediation），而非相关现象。

【9｜去污染与泛化边界】训练仓库与评测基准是否重叠、如何保证仓库级切分；跨语言零样本迁移（训练无该语言任务却评测该语言）的表现。

【10｜与用户关注方向的连接（推断性迁移）】该流水线范式原则上可迁移到科学计算与生信代码库（如分析管线、数值库）：把「已实现的分析流程」变成带可执行验证的 agent 训练环境，验证信号可来自数值一致性、重跑可复现性、结果范围检查等——这是把 agentic RL 环境构造思路带入 AI4Science 的一条可行路径，但论文本身未涉及该方向。

Q7: 总结一下论文的主要内容

【论证主线】论文的出发点是一个供给侧判断：训练有能力的 coding agent 靠的不只是 RL 算法，而是「多样任务 + 可靠验证器」的环境供给。开源代码库是这类环境的富矿，但主流做法只用开发过程副产物——issue 与 commit——来抽取任务，这等价于把任务集合限定在「被显式记录过的开发事件」之内。论文主张这一限制是当前可扩展性的主要瓶颈，并提出替代方案：既然绝大多数功能从未被 issue 记录，那就直接从「已实现的功能」出题，令源码成为唯一的任务特定输入。

【技术主线】CodeMidas 是一条 agentic 流水线，其组织原则是把 agentic compute 分配到环境构建的每个阶段，而非在单点做一次性生成：
- 探索与规范：agent 探索代码库中已实现的功能，将其形式化为行为规范（该功能应有什么行为、边界条件如何）。这一步把「实现」反向工程成「可检验的意图」，是替代 issue 文本的关键。
- 测试构造：在原始代码上真实执行以锚定测试，避免纯文本生成的规范幻觉。合理推断这是「生成-执行-修正」的闭环。
- 验证与筛选：既做执行检查（环境可复现、判定自洽），也做重复解 rollout（用多次采样的通过率判断任务是否可解、是否有区分度）。前者保证验证器可信，后者同时服务 GRPO 的可训练性——组内无方差的任务不产生梯度。
- 训练：以 GRPO 在 MiMo-V2.5 上做 RL。

【实验主线】产物层面：5,545 个训练任务、3,185 个开源仓库、23 种语言、15 个技术领域——这个广度本身就是「源码而非 issue」这一选择的直接体现。训练层面：在 GRPO 下，模型在五个多样化基准上全部提升，摘要点名三个方向作为代表：issue 修复（DeepSWE +11.7%）、整程序构建（ProgramBench +17%）、终端工作（Terminal-Bench v2.1 +8.5%）。消融层面：高质量训练任务数量增加带来性能提升，支持「该数据源可扩展」的核心主张。分析层面：轨迹对比显示 RL 后的 agent 探索代码库更多、自验证方式更多样，说明收益部分来自行为模式的改变，而非仅仅是知识补全。

【结论与其强度】论文的结论是「源码可以作为构建 RL 环境的可扩展基础」。支持强度上，论点较有说服力之处在于：①任务源与评测基准分布不同（无 issue 文本训练却提升 issue 修复），表明学到的是可迁移的工程行为而非数据泄漏式记忆（此判断为合理推断，去污染措施未见披露）；②广度指标（语言/领域）是硬性可核查事实；③行为层面的证据与分数提升方向一致，形成多角度互证。

【需要保留怀疑的地方】第一，摘要是本次唯一证据源，未获取 PDF 全文，所有流程细节、阈值、超参、过滤比例、成本、未点名的两个基准均无从核实。第二，以原始执行为 oracle 的测试存在「实现即规范」的偏置，可能固化实现细节并惩罚等价正确解，摘要未给出缓解证据。第三，scaling 消融只给出单调方向，未给出饱和点，因此「继续扩量仍有收益」尚不能从摘要推出。第四，跨分布提升的机制未被拆解——是探索能力、自验证能力，还是单纯更多同族代码的 exposure，需要原文的分项分析才能判断。

【阅读定位】这是一篇系统/基础设施型工作：贡献重心在数据与环境构造 pipeline，而非新 RL 算法。对关注 agentic RL 环境供给、代码 agent 训练数据构造、以及「用可执行验证替代人工标注」这一范式的读者，属于应读原文的方法论参考；对其结论的强度判断，应重点阅读过滤标准、验证器效度验证与消融表。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）直接相关：主题是 agentic RL 的环境/数据供给，而非单一模型改进，属于该方向的基础设施层工作

## 基本信息

- 作者：Bowen Ye, Lei Li, Shicheng Li, Zihao Yue, Linghao Zhang, Hanglong Lv, Yuanxin Liu, Wenhan Ma, Hao Tian, Rang Li, Jinhao Dong, Yikai Zhao, Xiangwei Deng, Hailin Zhang, Liang Zhao, Qi Liu, Lingpeng Kong, Tong Yang, Fuli Luo
- 机构：推测为小米（MiMo 团队）：依据是论文使用 MiMo-V2.5 作为基座模型、作者名单中包含 MiMo 相关研究者；元数据 institution 字段为空，且本次未获取 PDF 首页，机构归属需以原文作者单位标注为准。
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-09-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.22068`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次未检索到 PDF 语义证据（retrieved_evidence 与 field_evidence_map 均为空，sections 亦为空），全部内容基于标题、摘要与元数据展开；凡超出摘要的流程细节、风险与迁移路径均已标注为合理推断或推测，具体数值、超参、未点名基准与实验细节需回原文核对。
