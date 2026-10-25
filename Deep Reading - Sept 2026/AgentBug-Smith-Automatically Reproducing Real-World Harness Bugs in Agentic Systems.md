---
user_id: "cheng tan"
paper_id: 14828
arxiv_id: "2609.37864v1"
title: "AgentBug-Smith: Automatically Reproducing Real-World Harness Bugs in Agentic Systems"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.37864v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37864v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-12T12:57:02"
---
# AgentBug-Smith: Automatically Reproducing Real-World Harness Bugs in Agentic Systems

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：harness bug · agentic system · bug reproduction · benchmark construction

## 一句话总结

本文提出 AGENTBUG-SMITH，一个面向智能体系统 harness（外围支撑层）缺陷的自动化发现与复现方法，并基于它构建了包含 200 个可复现真实 harness 缺陷的活体基准 LIVE-HARNESS-BENCH，用于持续评估和提升软件智能体对 harness 缺陷的修复能力。

## 摘要

> 1. 动机：Agent harness bug（智能体外围基础设施缺陷）具有独特性质，现有最先进的软件智能体难以修复；已有基准只包含少量、固定的可执行 harness bug，且人工构建成本高达数百小时。
> 2. 方法：提出 AGENTBUG-SMITH，一个自动化 harness bug 复现方法，能够从开源 agentic systems 中持续发现并复现真实世界的 harness bugs。
> 3. 效果：在多种 backbone LLM 上，AGENTBUG-SMITH 一致优于面向通用软件设计的现有 bug 复现技术，harness bug 复现成功率高出 10.67%–27.56%。
> 4. 基准：将 AGENTBUG-SMITH 应用于开源 agentic systems，自动构建了 LIVE-HARNESS-BENCH，目前包含 200 个可复现的 harness bugs，且可随新出现的 bug 持续扩展。
> 5. 下游应用一：作为评测基准，系统评估了现有最先进软件智能体，揭示其修复真实 harness bugs 的能力有限。
> 6. 下游应用二：作为真实修复知识库，从中蒸馏可复用的修复技能，将软件智能体的 harness bug 修复率提升 6.32%。
> 7. 总体目标：建立可扩展的评估与改进基础，把真实 agent 失败转化为可执行的评测实例和可复用的 harness 改进知识，服务于递归自我改进智能体这一长期目标。

Q1: 这篇论文试图解决什么问题？

这篇论文针对的核心问题是：在 agentic systems 快速普及、harness 日益复杂的背景下，如何系统化地发现、复现并修复真实世界的 harness bugs。

具体问题可拆解为以下几点：
1. Harness bug 的重要性与被忽视：智能体系统通常由 backbone LLM 和其外围 harness 组成，harness 承担上下文管理、安全护栏、编排、工具集成等功能，代码规模可达 20 万行（如 OpenClaw）。近年研究已表明，仅改进 harness 即可显著提升 agent 效果，甚至无需更换 backbone LLM；self-evolving harness 和递归自我改进等概念进一步把 harness 从静态支撑层变成动态演化组件。然而，harness 实现复杂度的增长也使其成为 agentic systems 中故障的重要来源。
2. Harness bug 的特殊性使通用 bug 复现技术失效：harness bugs 的触发往往依赖外部 LLM 调用、工具交互、上下文状态、消息结构与序列化、内存/依赖管理、token 计费与配额等 agent 特有元素。通用软件缺陷复现技术没有为这些 agent 特有机制建模，难以构造出能够稳定复现失败的环境与测试。
3. 现有基准的瓶颈：已有 benchmark 只包含少量、固定的可执行 harness bugs，构建它们需要数百小时人工劳动，无法跟上真实世界中 harness bugs 持续涌现的速度；因此，评估数据陈旧、覆盖有限，也难以为修复智能体提供足够多的训练/蒸馏样例。
4. 修复能力的评估缺口：即便有了 bug 列表，也需要知道现有 SOTA 软件智能体到底能不能修复真实 harness bugs。由于缺少规模化、可复现的基准，这种评估一直难以进行，或者说现有评估的结论缺乏代表性。
5. 持续改进的闭环缺失：要让 agent 具备递归自我改进能力，需要不断把真实失败转化为可复现的评测实例和可复用的修复知识，这要求 bug 发现、复现、评估、知识沉淀形成一个自动化的持续闭环；当前没有这样的基础设施。

因此，论文不只是提出一个新的 benchmark，而是试图解决“如何可持续地从开源生态中自动挖掘真实 harness bugs，并让这些 bug 变成可执行评测和可复用修复知识”这一系统性缺口。根据摘要和检索片段，该问题在现有文献中尚缺少专门针对 harness bug 的自动复现手段，这也是本工作的核心切入点。

Q2: 有哪些相关研究？

论文涉及的相关研究方向可以按以下线索组织：

1. LLM Agent 与 Agentic Systems：引言引用了一系列关于 LLM agents 大规模应用的工作（如 Yang et al., 2024; Kannan et al., 2024; Zeng et al., 2023; Gottweis et al., 2026; Xiao et al., 2025），说明 agentic systems 正在成为新的软件范式。这些工作为本文提供了“agent 已广泛部署，组件可靠性重要”的背景。

2. Harness Engineering：论文重点关注 agent 的 harness（外围支撑层）。引言提到 Li et al. (2026) 等关于 harness engineering 的研究，表明“仅改进 harness 就能提升 agent 效果”已成为一个新兴研究方向；Lee et al. (2026) 的 self-evolving harnesses 和 Liu et al. (2026) 的 recursive self-improvement 则把 harness 视为可持续演化的组件，这正是本文把 harness bugs 作为核心对象的重要动机。

3. Agent/Harness Bug 研究：论文引用了 Rahardja et al. (2025)、Zhang et al. (2026b)、Chen et al. (2026) 等近期工作，显示现代 agent 应用和框架中存在多种实现缺陷；这些工作支撑了“harness bugs 是实际且严重的问题”的判断。由于正文缺失，这里只能基于引用出现的位置做合理推断：它们可能涉及 agent bugs 的实证研究或基准构建。

4. 通用软件缺陷复现技术：论文对比的 baseline 是“为通用软件设计的 bug reproduction 技术”，其中包括 SWE-Factory、SWE-bench-Live 等（检索片段中出现）。这类技术通常面向传统代码仓库中的缺陷，通过构造测试和环境来复现失败。本文认为它们没有针对 harness 的独特性建模，因此复现成功率较低。

5. 软件缺陷修复评测基准：SWE-bench 系列（如 SWE-bench Verified，SWE-bench-Live）是当前软件智能体修复评测的主流基准；本文构建的 LIVE-HARNESS-BENCH 可以被视为把 SWE-bench 思路扩展到 harness bugs，并引入“活体/持续扩展”特性。

6. 修复技能蒸馏与自我改进：下游应用中，论文从真实 harness bug 修复中蒸馏可复用修复技能，用于增强现有软件智能体的修复能力。这与 LLM 智能体自我改进、从经验中学习、技能库构建等方向相关。

7. 多智能体系统与自动化流水线：AGENTBUG-SMITH 本身是一个多智能体系统，把识别、环境构建、测试生成、联合优化等步骤流水线化，这与自动化软件工程中的多智能体协作、自主程序修复等方向相关。

总体来看，本文站在“通用 bug 复现”和“agent harness engineering”的交叉点，试图把通用软件工程中的缺陷复现技术适配到 agentic systems 特有的 harness bugs 上，并形成一个可持续扩展的评估与知识反馈闭环。由于我没有获取完整的 Related Work 章节，上述分类主要依据引言中的引用和检索到的方法/结果片段推断，具体引文细节需要以原文为准。

Q3: 论文如何解决这个问题？

论文提出的解决方案是 AGENTBUG-SMITH，一个面向 harness bug 的自动化发现与复现方法。根据摘要、引言片段和检索证据，其技术路线可归纳如下：

1. 整体架构：多智能体系统。AGENTBUG-SMITH 被构建为 multi-agent system，目标是在无需人工干预的情况下流水线化完成“发现问题—复现问题—产出可执行测试”的过程。这样做的好处是每个子任务可以由专门的 LLM agent 承担，既便于分工，也便于对每个环节进行独立评估和替换。

2. 阶段一：持续 harness bugs 识别。从开源 agentic systems 的 GitHub issues 等渠道持续搜集大量 issue，并自动判断哪些 repository 是 agent repository、哪些 issue 与 harness bug 相关。该阶段强调成本可控，论文在实现中选择 GPT-4.1-mini 这类性价比高的模型，因为该阶段需要处理大规模、连续的 issue 流。

3. 阶段二：自动化环境构建。对识别出的 harness bug 相关 issue，自动构建能够运行被测系统的执行环境。检索证据特别强调“functions under test cannot be mocked”（被测函数不可被 mock），即必须使用真实实现，确保复现出的失败确实源于有缺陷的实现本身，而不是测试替身或环境假设带来的假阳性。

4. 阶段三：失败复现测试生成与联合优化。系统生成能够触发 bug 的测试或执行脚本，并且将测试与环境的构造作为联合优化问题处理，而不是分别独立生成。这种“Joint Optimization of Test and Environment”的思路意味着测试所需的依赖、配置、mock 边界、资源等与环境一起调整，以提高最终复现成功率。检索片段中还可看到，系统可能从问题描述中提取关键约束，并采用迭代生成/验证循环（合理推断）。

5. Bug 分类学支撑（部分可见）：检索到的参考文献片段中出现类似“Content errors bad message attributes, serialization, non-primitive types”“Memory broken internal or external memory-stack modules dependencies”“LLM operation issues: model access, token usage, output handlers”的分类信息。这说明论文可能构建了一套 harness bug 分类体系，用于指导识别、复现和修复。该分类覆盖消息内容/序列化错误、内存与依赖问题、LLM 接入与 token 使用、输出处理等 agent 特有方面。由于该片段来自 References/附录区域，具体分类如何融入方法需以原文为准。

6. 可扩展性与活体机制：AGENTBUG-SMITH 被设计为持续运行：不断从开源生态获取新 issue，自动复现，并把成功复现的 bug 加入 LIVE-HARNESS-BENCH。由此，基准不再是一个静态快照，而是可以随着真实世界中的新缺陷出现而自动扩展。

7. 下游知识复用：论文进一步把 LIVE-HARNESS-BENCH 中的真实修复案例作为知识库，从中蒸馏可复用的修复技能，注入现有软件智能体，以提升其 harness bug 修复能力。这一步使系统从“复现问题”走向“辅助修复”，形成发现问题到改进修复能力的闭环。

总体而言，AGENTBUG-SMITH 的核心创新不是单一组件，而是把“持续海量 issue 筛选 + agent 特有的环境/测试联合优化复现 + 活体基准构建 + 修复技能蒸馏”整合为一个自动化流水线。其中“被测函数不可 mock”和“测试与环境联合优化”是检索证据中明确点出的关键设计原则，体现了对复现真实性的重视。

Q4: 论文做了哪些实验？

由于完整论文正文未全部提供，以下实验信息主要来自摘要、Results 片段和检索证据；具体协议细节需以原文为准。

1. Harness Bug 识别效果评估（4.1 节）：
- 设置：在 AGENTBUG-SMITH 的连续识别阶段使用成本较低的 GPT-4.1-mini 作为 backbone；将 vanilla LLM 直接调用作为 baseline（因为没有专门针对 harness bug 识别的现有技术）。
- 采样：从方法识别出的 225 个 GitHub issues 中随机抽取 132 个（对应 95% 置信水平、5% 误差范围），由两名标注者独立判断“所选仓库是否为 agent 仓库”以及“所选 issue 是否与 harness bug 相关”。
- 一致性：标注结果 Cohen’s kappa 为 0.88，表明高度一致。
- 稳定性：由于 LLM 推理具有随机性，作者对每个识别结果独立重复分类 5 次，并报告集合一致性 Sset 和归一化熵 Sentropy（公式在附录 H）。
- 结果：仓库级识别准确率达到 95%（另见 key_results 中的更多细节）。

2. Harness Bug 复现效果评估：
- 对比对象：面向通用软件的 bug 复现技术，检索片段中至少出现 SWE-Factory、SWE-bench-Live 等 baseline；同时在不同 backbone LLM（如 GPT-4.1-mini、DeepSeek-v3.2）上评估。
- 评价指标：复现成功率；检索片段还显示有“plausibly resolved”列，指生成的 patch 能通过失败复现测试的情况（这更接近修复评估而非复现评估）。
- 检索片段数值：部分对比中，AGENTBUG-SMITH 在 225 个目标 issue 上复现成功 46/225（20.44%）到 63/225（28.00%），而通用基线只复现 1–6 个（0.44%–2.67%）。需要说明，这些数字可能来自不同 backbone/不同阶段，具体对应关系需查询原文表格。
- 摘要给出的综合结论：与通用 bug 复现技术相比，AGENTBUG-SMITH 的 harness bug 复现成功率提升 10.67%–27.56%；另一处检索证据则提到环境构造成功率提升 10.67%–35.11%。这两个指标不同，分别对应最终复现成功率和中间环境构建成功率。

3. LIVE-HARNESS-BENCH 构建：
- 将 AGENTBUG-SMITH 应用于多个开源 agentic systems，持续自动复现，最终得到包含 200 个可复现 harness bugs 的活体基准；基准可随着新 bug 的出现继续扩展。

4. 下游应用一：用 LIVE-HARNESS-BENCH 评测 SOTA 软件智能体：
- 将基准作为评测集，让现有最先进软件智能体尝试修复其中的 harness bugs；实验揭示这些智能体在真实 harness bugs 上的修复能力有限，但具体修复率数字在现有材料中未看到，需查阅原文结果部分。

5. 下游应用二：修复技能蒸馏：
- 把 LIVE-HARNESS-BENCH 中真实修复案例作为知识库，从中蒸馏可复用修复技能，再用于增强现有软件智能体；实验显示 harness bug 修复率提升 6.32%。同样，具体实验设计（如何蒸馏、注入哪个 agent）在现有材料中未展开，需参考原文。

6. 稳定性与消融（部分缺失）：文中至少在识别阶段做了 5 次独立分类的稳定性评估；是否还有端到端消融、成本分析、失败案例分析，从现有材料无法确认。

Q5: 发现了什么实验现象？

根据摘要、Results 片段和检索证据，可以提炼出以下实验现象：

1. 识别阶段表现强且稳定：AGENTBUG-SMITH 的 harness bug 识别达到 95% 的仓库级准确率，且两位标注者的 Cohen’s kappa 为 0.88，说明自动识别结果与人类判断高度一致。5 次重复分类的集合一致性和熵指标说明该方法对 LLM 采样随机性具有较好的稳定性（具体数值需查附录 H）。

2. 通用 bug 复现技术显著失效：在与通用软件 bug 复现技术的对比中，基线在真实 harness bug 上复现成功率很低（检索片段显示在 225 个 issue 上仅 1–6 个成功，约 0.44%–2.67%），而 AGENTBUG-SMITH 可达到 20.44%–28.00%。这直观反映了 harness bug 的独特性：现有面向普通代码缺陷的复现流程没有考虑 LLM 调用、工具交互、harness 状态等要素，因此大面积失败。

3. 不同 backbone 下的一致性优势：AGENTBUG-SMITH 在多个 backbone LLM 上都一致优于基线，说明其效果不是某个特定模型带来的偶然优势；提升幅度在不同模型上有差异（环境构建成功率提升 10.67%–35.11%，最终复现成功率提升 10.67%–27.56%）。

4. 最终复现成功率仍然不高：即使 AGENTBUG-SMITH 显著超过基线，其绝对复现成功率也只有约 20%–28%。这说明真实世界 harness bugs 的复现本身非常困难，仍有大量 bug 无法自动复现；这也构成未来工作的重要空间。

5. “被测函数不可 mock”可能对复现真实性有影响：检索证据特别提到被测函数不能被 mock，以保证失败确实来自实际有缺陷的实现。这可能意味着 AGENTBUG-SMITH 采用了更严格的复现标准，因此其成功率数字比“宽松复现”更可信，但也可能因此降低了可复现比例。

6. 现有 SOTA 智能体修复真实 harness bugs 的能力有限：使用 LIVE-HARNESS-BENCH 评测后，作者得到的结论是 SOTA 软件智能体难以修复这类 bug。这与引言中“harness bugs 对最先进 agent 也具有挑战性”的判断一致，说明问题不是单纯因为复现工具弱，而是修复能力本身存在缺口。

7. 修复知识蒸馏带来可测量的提升：将基准中的真实修复作为知识源进行技能蒸馏后，现有智能体的 harness bug 修复率提升 6.32%。这表明“真实失败案例→可复用修复知识→修复能力提升”的闭环是可行且有效的，尽管提升幅度有限。

8. 指标之间的张力：论文同时报告“复现成功”“plausibly resolved patch 通过测试”“修复率”等不同指标。检索片段显示 “plausibly resolved” 指生成的 patch 能通过失败复现测试，这意味着“通过测试”并不等于“真实修复”，可能存在测试覆盖不足或过拟合测试的虚假修复风险；这也是评估设计中的一个需要读者注意的张力。

Q6: 有什么可以进一步探索的点？

基于论文的定位和已有结果，可以延伸出如下可进一步探索的方向：

1. 提升复现绝对成功率：当前 AGENTBUG-SMITH 达到约 20%–28% 的复现成功率，仍有大量 harness bugs 无法自动复现。未来可以研究更复杂的动态环境构造、更细粒度的 harness 状态建模、利用日志/堆栈信息引导复现、以及跨仓库迁移复现经验等方法。

2. 扩展 harness bug 分类体系：检索证据中出现的分类（内容/序列化错误、内存与依赖问题、LLM 接入与 token 使用、输出处理等）很可能还不完整；可以结合更多真实缺陷数据扩充分类学，并为每一类设计专门的复现与修复策略。

3. 更广泛的 agent 框架覆盖：目前基准来自开源 agentic systems，未来可以覆盖更多流行框架、商业系统的开源部分、不同编程语言生态，以提升基准的代表性和通用性。

4. 自动复现与自动修复的联合优化：论文目前先把复现做好，再用知识蒸馏辅助修复；未来可以探索“复现—修复—再验证”的端到端闭环，让同一个多智能体系统在发现缺陷后直接尝试修复并验证。

5. 更严格的修复验证：现有 “plausibly resolved” 指标只要求 patch 通过失败复现测试，仍可能被针对性 hack 或过拟合。未来可以引入回归测试套件、对抗性验证、人工/模型裁判等机制，防止虚假修复。

6. 修复技能蒸馏的机制深化：6.32% 的提升说明蒸馏有效但幅度有限；可以研究如何蒸馏更结构化、可组合的修复技能（如工具使用模式、修复模板、错误模式库），如何选择注入时机，以及如何在 agent 的自我改进循环中持续更新技能库。

7. 递归自我改进的实证研究：论文声称目标是递归自我改进智能体；未来可以设计长期运行实验，观察 agent 在每次从 LIVE-HARNESS-BENCH 获取新知识后是否在后续任务中表现持续提升，以及是否会出现遗忘、技能冲突或恶性循环。

8. 成本与效率建模：持续识别大量 issue 需要成本可控的模型；可以进一步研究识别阶段的主动采样策略、级联模型选择、缓存和增量学习，以降低持续运行的推理成本。

9. 安全性与社会维度：harness bug 中包括安全护栏缺陷、权限管理错误等；未来可以专门研究这类高危缺陷的复现与修复优先级，并讨论自动修复可能引入的新风险。

10. 与 AI-for-Science 等垂直领域结合：若科学 agent 广泛依赖 harness 进行工具调用、数据流管理和实验编排，本文的方法可作为底层基础设施，用于保障科学 agent 的可靠性；但目前论文并未涉及该场景，这属于合理推断与应用迁移方向。

Q7: 总结一下论文的主要内容

这篇论文围绕“agentic systems 中的 harness bug”展开，提出了一套自动化发现、复现、基准化和知识复用的完整方案。

一、论证主线
论文首先指出，LLM agents 正在成为新的软件范式，而 agentic systems 通常由 backbone LLM 和外围 harness 组成。harness 承担上下文管理、安全护栏、编排、工具集成等关键职能，代码规模可达 20 万行，其复杂度已经使其成为系统失败的重要来源。同时，近年 harness engineering、self-evolving harnesses 和 recursive self-improvement 等研究方向表明，harness 本身可以从静态支撑层演化为决定 agent 能力的关键组件，因此 harness 的质量和缺陷修复能力至关重要。

然而，harness bugs 与普通软件缺陷不同，它们涉及 LLM 调用、工具交互、状态恢复、token 计费、消息序列化等 agent 特有机制，导致现有面向通用软件设计的 bug 复现技术难以奏效；SOTA 软件智能体在修复这类缺陷时也表现有限。更现实的问题是，现有 benchmark 中可执行的 harness bugs 数量少且固定，构建它们需要数百小时的人工劳动，无法跟上真实 bug 持续涌现的速度。因此，论文认为需要一种自动化的、可持续的机制，把真实世界中的 agent 失败转化为可复现的评测实例和可复用的修复知识。

二、技术主线
为填补这一缺口，论文提出 AGENTBUG-SMITH。它是一个多智能体系统，旨在无需人工干预的情况下完成 harness bug 的持续发现与复现。其流水线包含三个主要环节：
1. 持续识别：从开源 agentic systems 的 GitHub issues 等来源自动筛选与 harness bug 相关的 issue，使用成本较低的模型（如 GPT-4.1-mini）以支持大规模持续运行。
2. 环境构造：针对识别出的 issue 自动构建执行环境，并且强调被测函数不能被 mock，确保复现出的失败确实来自真实的有缺陷实现。
3. 失败复现：生成能够触发 bug 的测试，并将测试生成与环境污染构造作为联合优化问题，以提高最终复现成功率。
论文还建立了一套 harness bug 分类体系（检索片段中出现内容错误、内存与依赖问题、LLM 操作问题等类别），用于支撑识别和复现流程。

基于 AGENTBUG-SMITH，论文构建了 LIVE-HARNESS-BENCH，一个“活体”且可扩展的基准，当前包含 200 个可复现的真实 harness bugs，并可通过持续运行不断加入新 bug。

三、实验主线
论文的实验设计覆盖了三个层面：
1. 组件有效性：对 harness bug 识别阶段，论文从 225 个自动识别的 issues 中随机抽样 132 个（95% 置信度、5% 误差），由两名标注者独立评估仓库是否为 agent 仓库、issue 是否与 harness bug 相关，得到 Cohen’s kappa 0.88，仓库级准确率 95%；同时通过 5 次重复分类验证识别稳定性。
2. 复现效果：AGENTBUG-SMITH 与面向通用软件的 bug 复现技术（如 SWE-Factory、SWE-bench-Live 等）在多个 backbone LLM 上对比。结果表明 AGENTBUG-SMITH 显著优于基线：harness bug 复现成功率提升 10.67%–27.56%，环境构建成功率提升 10.67%–35.11%；部分对比中 AGENTBUG-SMITH 在 225 个 issue 上复现 46–63 个（约 20.44%–28.00%），而基线仅 1–6 个（0.44%–2.67%）。这既体现了方法优势，也说明绝对成功率仍较低，任务难度大。
3. 下游应用：第一，使用 LIVE-HARNESS-BENCH 作为评测基准，系统评估 SOTA 软件智能体的 harness bug 修复能力，结论是它们的能力有限；第二，把 LIVE-HARNESS-BENCH 作为真实修复案例知识库，从中蒸馏可复用修复技能，使现有智能体的 harness bug 修复率提升 6.32%。

四、总结与意义
论文将 AGENTBUG-SMITH 和 LIVE-HARNESS-BENCH 作为一个整体基础设施，目标是持续将真实世界的 agent 失败转化为可执行评测和可复用知识，从而支持对软件智能体的持续评估和改进，最终服务于递归自我改进智能体。该工作把“bug 复现”从一次性任务变为持续运行的自动化流程，把“基准”从静态集合变为可扩展的活体资源，并在修复技能蒸馏上验证了知识反馈的可行性。整体上，这是一项系统性较强的工作，介于 software engineering for AI agents 和 agent self-improvement 两个方向之间。

需要注意的是，由于我只获取了摘要、引言片段、部分 Results 和 Conclusion，文中具体的方法细节（如多智能体协作方式、联合优化算法、蒸馏的具体形式）和完整实验表格尚未展开，上述总结在个别环节包含合理推断，精确实现请以全文为准。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文聚焦 agentic systems 的 harness 工程与缺陷修复，属于用户画像中“agent”方向的直接相关研究，可用于理解 agent 可靠性基础设施。

## 基本信息

- 作者：Yiming Cheng, Alfin Wijaya Rahardja, Mengshi Zhang, Zihao Chen, Zhenpeng Chen, Yiling Lou
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-09-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37864v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（Introduction、Results、Conclusion、References 等片段），并结合论文摘要与启发式草稿；部分方法细节和实验条件因原文未完整提供而标注为合理推断或推测。
