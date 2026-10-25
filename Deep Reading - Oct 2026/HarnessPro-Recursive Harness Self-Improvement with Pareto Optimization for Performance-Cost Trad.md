---
user_id: "cheng tan"
paper_id: 16463
arxiv_id: "2610.26770v1"
title: "HarnessPro: Recursive Harness Self-Improvement with Pareto Optimization for Performance-Cost Trade-offs"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.26770v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.26770v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:03:09"
---
# HarnessPro: Recursive Harness Self-Improvement with Pareto Optimization for Performance-Cost Trade-offs

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：recursive self-improvement · agent harness optimization · performance-cost trade-off · pareto optimization

## 一句话总结

HarnessPro 提出一种无标注的递归 Harness 自改进方法，通过多智能体协作将执行轨迹反馈转化为考虑性能与成本双重目标的 Harness 更新，并用归一化加权平方距离选择输出 lite、medium、heavy 三种部署档位，在三个基准上实现平均 2.0 个百分点的性能提升和 26.1% 的单任务成本下降。

## 摘要

> Harness development plays a crucial role in building capable AI agents. Recent approaches improve harnesses through recursive self-improvement (RSI), but their performance gains often come with higher execution costs. We introduce HarnessPro, a label-free method for evolving harnesses to improve task performance while reducing execution cost. In this setting, candidate harnesses are evaluated using LLM-generated tests without access to ground-truth solutions or official tests. Specialized agents diagnose execution trajectories, plan updates with different performance-cost priorities, and implement them as candidate harnesses. Rather than returning a single harness, HarnessPro uses normalized weighted squared-distance selection to provide cost-focused (lite), balanced (medium), and performance-focused (heavy) harnesses for different deployment budgets. Selected harnesses enter the next iteration, where execution feedback guides further updates. Our experiments cover Terminal-Bench 2, SWE-Bench Pro, and ProgramBench, using GPT-5.5 with Codex and GLM-5.2 with Claude Code. Across these benchmarks using GPT-5.5 with Codex, the heavy mode of HarnessPro achieves an average pass-rate gain of 2.0 percentage points and an average per-task cost reduction of 26.1% relative to RHO. This advantage also holds when using GLM-5.2 with Claude Code. Code is available at https://github.com/pzs19/HarnessPro

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的问题是：递归 Harness 自改进（RSI）在提升智能体任务性能时，往往以更高的执行成本（token 消耗、延迟）为代价，而实际部署中执行成本与延迟同样重要。具体而言，论文指出近期 RSI 方法（如通过更充分的探索和更强验证来提升性能）虽然提高了 pass rate，但 token 消耗与延迟也随之上升，导致性能-成本权衡不理想。HarnessPro 的核心研究问题为：递归 Harness 自改进能否在不依赖人工标注（无 ground-truth 解法、无官方测试、无标签）的前提下，同时提升任务性能并降低执行成本？\n\n该问题包含两个子挑战：（1）Harness 搜索空间巨大。一个 Harness 由指令、工具、验证器、策略、记忆等组件构成，组合空间难以穷举；（2）性能与成本之间的权衡需要被显式建模，而不是仅追求性能单一目标，因为不同部署场景对成本、延迟和性能的偏好不同。此外，论文还隐含处理了候选 Harness 评估的标签缺失问题：在没有 ground-truth 或官方测试的情况下，如何可靠地比较候选 Harness 的优劣。\n\n从研究范式角度看，该工作把 Harness 优化从单一性能目标转化为多目标 Pareto 优化问题，并挑战了"性能提升必然带来成本上升"的隐含假设，试图找到 Pareto 前沿上的多个操作点而非单个最优解。这与传统 RSI 工作（只返回一个改进后的 Harness）形成对比。

Q2: 有哪些相关研究？

相关研究可归为几条主线：\n\n1. 递归自改进（RSI）与 Harness 优化：Zhang et al. (2026)、Suzgun et al. (2026)、Lin et al. (2026)、Pan et al. (2026a) 等近期工作利用过去执行轨迹诊断问题并指导 Harness 更新。其中 RHO（Pan et al., 2026a）通过轨迹分析和组件修订改进 Harness，是本文的主要 baseline；RHO+Iteration 是 RHO 的迭代变体。这类工作的共同特点是追求性能提升，但往往通过更广泛的探索和更强的验证实现，导致 token 消耗与延迟增加。\n\n2. 可复用策略与记忆方法：Dynamic Cheatsheet（Suzgun et al., 2026）维护可复用的策略和代码片段；ReasoningBank（Ouyang et al., 2026）检索从过去轨迹中蒸馏出的策略；Sleep-time Compute（Lin et al., 2025）在任务前准备可复用的记忆。这些方法通过在推理时注入外部记忆或策略来提升性能，本质上也是性能导向，成本问题未被系统处理。\n\n3. 多智能体协作与轨迹诊断：本文采用多智能体框架（trajectory analyzer、planner、editor、test-synthesis agent），与 agent 领域常见的方法设计模式相关，但区别在于这些智能体的产出是 Harness 代码本身而非任务答案。\n\n4. 无标签评估与 LLM 生成测试：HarnessPro 使用 LLM 生成的测试评估候选 Harness，这与无参考评估、LLM-as-a-judge、测试生成用于程序评估等方向相关，且与 SWE-bench 类基准的官方测试不可见设定兼容。\n\n5. 多目标优化与 Pareto 前沿：归一化加权平方距离选择属于多目标优化中选择 Pareto 代表性解的思路，与经典多目标优化（如 Tchebycheff 标量化、weighted distance）相关。\n\n6. 核集选择：训练集选取使用 DPP-based coreset selection（Kulesza & Taskar, 2012），属于数据子集选择与多样性采样方向。\n\n需要说明：摘要与元数据中的 GPT-5.5、GLM-5.2、Codex 0.147.0、Claude Code 2.1.231 等模型版本名称较新，发表时间为 2026 年，属于未来标注；若这些模型名在读者时间线中不存在或未发布，可能需要回原文确认其具体指代。

Q3: 论文如何解决这个问题？

HarnessPro 的解决方案是一个无标签、多智能体、递归迭代的 Harness 进化框架，核心思想是把性能与成本同时纳入更新与选择目标。整体流程按 epoch 迭代，每轮保留至多三个 Harness（lite、medium、heavy）。\n\n1. 多智能体编排：\n- Trajectory Analyzer（轨迹分析器）：对每个训练任务执行 G=3 次独立 rollout，记录轨迹 τ 与成本指标 m（如 token、延迟），分析器基于重复 rollouts 诊断失败原因与可避免的开销（avoidable overhead）。\n- Planner（规划器）：接收诊断结果，在不同性能-成本优先级下制定 Harness 改进计划；不同优先级对应 lite（成本优先）、medium（均衡）、heavy（性能优先）。\n- Editor（编辑器）：将改进计划实现为可执行的候选 Harness 代码。\n- Test-synthesis Agent（测试合成智能体）：在无 ground-truth 与官方测试的情况下，为候选 Harness 生成固定测试集，用于评估候选。\n\n2. 无标签评估：候选 Harness 用 LLM 生成的测试评估，不使用 ground-truth 解法或官方测试，这使方法可迁移到标签稀缺的新任务域。\n\n3. Pareto 选择机制：通过归一化加权平方距离选择（normalized weighted squared-distance selection）从候选集合中选出代表解。算法维护帕累托前沿点，按公式 J_m(h)（m ∈ {lite, medium, heavy}）选择代表：h_{t+1}^m ← argmin_{h∈候选} J_m(h)，候选集合每轮更新为 {h_lite, h_medium, h_heavy}。不同档位使用不同 rubric 评估性能-成本权衡。\n\n4. 递归迭代：入选的 Harness 进入下一轮迭代，新的执行反馈继续引导更新，直到达到预设 epoch 数（Terminal-Bench 2 与 SWE-Bench Pro 三 epoch，ProgramBench 四 epoch）。\n\n5. 训练集构建：从各基准剩余任务中用 DPP-based coreset selection（Kulesza & Taskar, 2012）选出 10 个任务作为训练集 D_train，其余作为验证集，降低进化所需的任务级监督成本。\n\n6. 成本导向的机制演化：进化得到的 Harness 包含动态机制（如根据任务描述选择执行 profile，包含推理努力级别与工具调用预算；压缩后的 reminder 追加到工具结果末尾，保留 prompt 前缀以利于缓存复用）和静态上下文冗余削减（如 Terminal-Bench 2 上的工具输出压缩），从而同时改善性能与成本。方法不依赖人工反馈，属于 label-free 设定。

Q4: 论文做了哪些实验？

论文在三个基准上评估 HarnessPro：Terminal-Bench 2（终端任务）、SWE-Bench Pro（仓库级 issue 解决）、ProgramBench（程序重建）。测试集分别留出 59、100、100 个任务，遵循 RHO 的数据划分协议；从剩余任务中用 DPP-based coreset 选取 10 任务作为训练集，其余为验证集。模型组合为 GPT-5.5×Codex 0.147.0 与 GLM-5.2×Claude Code 2.1.231；每个候选 Harness 每 epoch 每训练任务产生 G=3 次 rollout；Terminal-Bench 2 与 SWE-Bench Pro 进化 3 轮，ProgramBench 进化 4 轮，每轮后保留至多 3 个 Harness。\n\nBaseline 包括：Vanilla（原始 Codex 或 Claude Code Harness，即优化起点）、Dynamic Cheatsheet（维护可复用策略与代码片段）、ReasoningBank（检索从过去轨迹蒸馏的策略）、Sleep-time Compute（任务前准备可复用内存）、RHO（轨迹分析并修订 Harness 组件）、RHO+Iteration（RHO 的迭代版本）以及 HarnessPro 自带的三档输出（Lite、Medium、Heavy）。所有方法使用相同数据划分与评分设置。\n\n评估指标为 P(%)（pass rate）、C($)（单位任务成本美元）、T(s)（延迟秒）。论文同时报告了 GPT-5.5×Codex 和 GLM-5.2×Claude Code 两组实验，表格中给出了均值±标准差。

Q5: 发现了什么实验现象？

实验观察到的主要现象如下：\n\n1. 性能与成本可以同时改善（打破零和假设）：在 GPT-5.5×Codex 下，Heavy 模式相对 RHO 在三个基准上平均 pass-rate 提升 2.0 个百分点，同时每任务成本下降 26.1%。具体数值显示：Terminal-Bench 2 上 HarnessPro-Heavy 达到 80.8%（RHO 76.3%），成本 0.72（RHO 1.09，下降约 34%）；SWE-Bench Pro 上 Heavy 80.7%（RHO 80.0%），成本 1.97（RHO 2.68，下降约 26%）；ProgramBench 上 Heavy 69.6%（RHO 68.8%），成本 2.95（RHO 3.59，下降约 18%）。\n\n2. 三档模式的性能-成本梯度与非平凡行为：Lite 模式成本极低（Terminal-Bench 2 成本 0.43，RHO 的约三分之一），性能 72.9% 高于 Vanilla 的 69.5% 但低于 RHO；Medium 模式在 Terminal-Bench 2 上追平 RHO（76.3%）但成本减半（0.56 vs 1.09）。意外之处在于：SWE-Bench Pro 上 Lite 的 pass rate（72.3%）显著高于 Vanilla（59.0%）和多数 baseline，说明纯成本优先的进化也能带来性能增益；Terminal-Bench 2 与 SWE-Bench Pro 上 Medium 分别以 79.0 与 67.9 略低于或接近 RHO 的 80.0/68.8，但成本大幅下降（2.08→1.26；3.13→2.90，其中 SWE-Bench Pro 下降明显）。\n\n3. 延迟收益与反常情况：多数模式下延迟低于 RHO（如 Terminal-Bench 2 Heavy 310s vs RHO 354s；SWE-Bench Pro Medium 326s vs 437s；ProgramBench Medium 745s vs 928s），但 ProgramBench 的 Lite 延迟 985s 反而高于 RHO 的 928s 且高于 medium/heavy，而该行成本（2.18）却最低，说明 Lite 在 ProgramBench 上以延迟换取成本，三指标不是单调一致，存在指标间张力。GLM-5.2×Claude Code 下 ProgramBench 延迟普遍很高（数千秒），其中 Medium 5893s、Heavy 5925s 均低于 RHO 6434s，但高于 Lite 3907s。\n\n4. 跨模型组合的稳健性：GLM-5.2×Claude Code 下同样成立，Heavy 在 Terminal-Bench 2 上 81.4%（RHO 78.0%）、SWE-Bench Pro 77.3%（RHO 75.0%）、ProgramBench 64.4%（RHO 63.3%），成本分别从 1.00→0.62、2.17→1.01、17.00→14.95，说明方法不依赖特定模型对。\n\n5. 演化机制分析：在 Terminal-Bench 2 与 SWE-Bench Pro 上，进化出的 Harness 会从任务描述中挑选执行 profile（含推理努力级别、工具调用预算），lite/medium/heavy 使用不同 rubric；压缩后的 reminder 追加到工具结果末尾以保持 prompt 前缀不变从而复用缓存，同时削减静态上下文冗余（如工具输出压缩）。这些机制共同贡献增益（论文归因于多机制协同），而非单一机制。\n\n6. 递归迭代的持续收益：RHO+Iteration 相对单轮 RHO 在部分指标上提升（如 Terminal-Bench 2 成本 0.82 vs 0.82/1.09、SWE-Bench Pro 成本 1.90 vs 2.68），HarnessPro 的多轮进化进一步放大了这一趋势，说明递归自改进可持续进步；但需要指出，RHO+Iteration 在 Terminal-Bench 2 上性能未超过 RHO（76.3 vs 76.3），成本略降 0.82 vs 1.09，增益主要体现在成本侧。\n\n7. 负结果与反直觉点：Terminal-Bench 2 的 Lite 性能（72.9）虽高于 Vanilla 但低于所有基线的最高值，且成本波动较大（0.43±0.01）；SWE-Bench Pro 的 RHO 性能（80.0）高于 HarnessPro-Medium（79.0），说明平衡模式在极端性能优先场景未完全占优；ProgramBench 的 RHO 延迟 928s 低于 Heavy 的 888±32 的区间上限相近但 Lite 985±70 更高，说明成本优化不等于延迟优化（likely 因压缩与重试机制引入额外计算）。

Q6: 有什么可以进一步探索的点？

1. 更大规模的 Harness 搜索空间与自动化组件搜索：当前每轮仅由 planner 制定改进计划，搜索受限于 LLM 规划的先验；可探索程序化搜索（如蒙特卡洛树搜索、进化算法）与 LLM 结合，覆盖指令、工具、验证器、策略、记忆等更多组件组合。\n2. 训练集规模与核集选择影响：当前只用 10 个训练任务的 DPP coreset；可研究训练任务数量、多样性对进化质量和泛化边界的影响，以及核集选择标准与下游性能之间的关系。\n3. 更多档位与连续 Pareto 前沿输出：目前输出三个代表性档位；可研究用更细粒度（或连续）的 Pareto 前沿近似，让部署方可按预算任意插值选择操作点。\n4. 评估可靠性：LLM 生成的测试替代 ground-truth 的可靠性边界需进一步研究，包括测试生成的假阳性/假阴性率、测试污染风险，以及用验证集校准评估信号的方法。\n5. 延迟与成本的显式分离建模：实验显示 ProgramBench 的 Lite 出现成本低但延迟高的情况，说明成本和延迟并非完全耦合；可把延迟作为第三个优化目标或约束，探索三目标 Pareto 优化。\n6. 跨领域与跨 agent 框架泛化：本文验证了 Codex 与 Claude Code 两套 harness 栈；可扩展到更多 agent 框架（如通用 ReAct、多智能体协作框架、浏览器/具身智能体）和更多任务类型（如数据分析、科学计算）。\n7. 安全性与鲁棒性：递归自我修改 Harness 可能引入不可预期的行为退化、测试过拟合或安全风险；需要研究进化过程中的回退机制、稳定性保证与安全约束。\n8. 缓存与上下文工程的理论化：reminder 追加与前缀缓存复用目前是启发式机制；可研究 prompt 结构对 KV cache 命中率的影响，或把缓存命中率作为可优化指标。\n9. 与在线学习结合：当前进化在固定训练集上进行；可探索部署后利用真实用户轨迹持续微调与安全限制，形成闭环 self-improvement。\n10. 科学应用落地：结合用户画像中对 AI-for-Science 的兴趣，可将 HarnessPro 应用于科研 agent（如文献分析、实验设计、代码重建类任务），利用其 label-free 特性适应科研任务中缺少标准测试集的情况。

Q7: 总结一下论文的主要内容

HarnessPro 研究的问题是：递归 Harness 自改进虽然能提升智能体性能，但往往以更高的执行成本为代价；论文试图回答能否同时提升性能并降低成本。论证主线：首先指出现有 RSI 方法（Dynamic Cheatsheet、ReasoningBank、Sleep-time Compute、RHO 等）在性能提升的同时伴随 token 消耗与延迟增加；其次提出性能与成本的权衡应被显式建模，并认为找到好的操作点面临两大挑战：巨大 Harness 搜索空间和不同部署预算下的多目标需求；最后通过实验证明性能与成本可以同时改善。\n\n技术主线：HarnessPro 采用多智能体递归进化框架。Trajectory Analyzer 对每个训练任务执行多次 rollout，诊断失败与可避免开销；Planner 在不同性能-成本优先级下制定改进计划；Editor 将计划实现为候选 Harness；Test-synthesis Agent 在无 ground-truth 与官方测试条件下生成评估用测试。候选 Harness 通过归一化加权平方距离选择，在 Pareto 前沿上输出 lite（成本优先）、medium（均衡）、heavy（性能优先）三个档位，并递归迭代。训练集用 DPP coreset 从基准数据中选出 10 个任务，降低监督成本。在实现层面，进化出的 Harness 会从任务描述选择执行 profile（推理努力与工具预算）、压缩工具输出、把 reminder 追加到工具结果并复用 prompt 前缀缓存、削减静态上下文冗余等。\n\n实验主线：在 Terminal-Bench 2（终端任务）、SWE-Bench Pro（仓库级 issue 解决）、ProgramBench（程序重建）上，用 GPT-5.5×Codex 与 GLM-5.2×Claude Code 两套组合评估，对比 Vanilla、Dynamic Cheatsheet、ReasoningBank、Sleep-time Compute、RHO、RHO+Iteration 及自身三档。主要结果：GPT-5.5×Codex 下 Heavy 平均相对 RHO 获得 2.0 个百分点 pass-rate 增益，每任务成本降 26.1%；该优势在 GLM-5.2×Claude Code 同样成立；三档模式覆盖不同性能-成本操作点，多数配置下延迟也低于 RHO。机制分析显示提升来自多种演化机制的协同（动态 profile 选择、工具输出压缩、reminder 追加与缓存复用、静态上下文削减），且递归迭代带来持续进步。\n\n结论：HarnessPro 表明性能与效率可以共同提升，成本感知的进化是实用化递归自改进的有前景方向。论文提供开源代码。整体属于系统型工作，强调框架设计与实验协议完整性的对照，而非单一算法组件的理论突破。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向直接相关（权重 0.10）：论文主题是智能体 harness 的自动优化，属于 agent 基础设施层的核心问题，而非具体任务策略。

## 基本信息

- 作者：Zhuoshi Pan, ZhiYuan Feng, Yukuo Cen, Honglin Lin, Junru Lu, Zhongxia Chen, Shuo Yang, H. Vicky Zhao, Jie Tang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.26770v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 retrieved_evidence 中 Introduction、Method、Results、Conclusion 的命中片段，并结合 heuristic_draft 与论文元数据进行补全；部分机制细节（如归一化加权平方距离公式、测试合成流程）因全文片段不完整而标注为需要回原文确认。
