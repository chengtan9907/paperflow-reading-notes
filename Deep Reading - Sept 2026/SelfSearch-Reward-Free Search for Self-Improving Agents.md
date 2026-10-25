---
user_id: "cheng tan"
paper_id: 14352
arxiv_id: "2609.37968v2"
title: "SelfSearch: Reward-Free Search for Self-Improving Agents"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.37968v2.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37968v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/reading_reports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-08T23:44:37"
---
# SelfSearch: Reward-Free Search for Self-Improving Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-improving agents · reward-free search · self-modification · episode records

## 一句话总结

SelfSearch 提出一种完全无奖励信号的智能体自我改进搜索方法，利用历史自我改进剧情的记录（推理过程、工具动作、结果反馈）来引导后续修订，在无需下游评估信号的情况下在六个模型-基准组合上均提升种群平均成功率，并以仅 4.03 美元的搜索成本在 Terminal-Bench 2.1 上达到与顶级 harness Codex 持平的成绩。

## 摘要

> Advances in the coding capabilities of LLM agents allow them to inspect and modify their own instructions, tools, and execution procedures. Existing approaches use this ability to search for improved agents through repeated downstream evaluation, which incurs substantial costs and ties the search to the evaluated tasks. We introduce \textbf{SelfSearch}, a reward-free search procedure in which agents modify themselves using records of previous self-improvement episodes. These records capture the reasoning, tool actions, and outcomes of earlier modification attempts, providing concrete experience for improving both task solving and self-modification. Without downstream reward signals during search, SelfSearch improves population-mean success over the initial agent in all six model--benchmark settings, with individual agents gaining up to 11.2 percentage points on Terminal-Bench 2.1. On SWE-bench Multilingual, an agent improves success by \textbf{5.0} percentage points while reducing execution cost by \textbf{38.5}\% on tasks solved by both the initial and evolved agents. SelfSearch achieves competitive task success with evaluation-guided search baselines at lower search cost. With only \textbf{\$4.03} in search cost, it produces a harness that solves \textbf{82.0}\% of Terminal-Bench 2.1 tasks with DeepSeek V4 Flash under the settings of a public nine-harness comparison, matching the top-scoring harness, Codex. These results suggest that experience gained through self-modification can improve agents' downstream capabilities and efficiency.

Q1: 这篇论文试图解决什么问题？

这篇论文回答的核心问题是：智能体能否通过自我修改的“过程经验”而非“结果评分”来持续改进自身？具体而言：

1) 现有范式的问题：LLM 智能体现在可以检查并修改自己的指令、工具和执行流程，但绝大多数自我改进方法依赖下游评估（如开发集上跑任务）来指导搜索。这带来两个结构性缺陷：(a) 评估成本随着任务数量和候选版本数线性增长，每次修改都要重新执行下游任务；(b) 改进信号被绑定到评估任务分布，可能导致过拟合某一基准而损害跨任务泛化。

2) 未被利用的资源：论文指出现有方法只把一次修改当作产生“候选智能体”，却丢弃了“修改过程本身产生的记录”。这些记录包含智能体如何识别局限、构造修订、检查效果的信息——即元认知层次的经验。论文的动机是：自我改进的过程经验可以为未来的自我改进提供证据，从而形成“学会如何学习”的正循环。

3) 无奖励搜索的可行性与边界：去掉下游奖励信号后，搜索失去直接性能导向，需要依靠记录中的隐式反馈（如工具动作是否成功、推理是否受挫、结果是否失败）来引导。这引出一个关键问题：仅有过程记录而无结果奖励，搜索是否仍能稳定提升能力？这正是论文用六个模型-基准组合实验验证的核心。

4) 隐含假设与风险：论文隐含假设“过程记录中的失败模式和成功模式可迁移到新修改决策”，且“改进者自身也能通过历史改进经验进化”（即 adaptive lineage 的意义）。这些假设并非显然成立，因为记录可能过时、噪声大、或过度反映早期错误。论文通过消融实验部分验证了该假设，但泛化边界仍开放。

Q2: 有哪些相关研究？

论文直接定位在 LLM 智能体自我改进研究线上。根据摘要、引言和检索到的引用片段，相关研究可分为几层：

1) 基于任务经验的智能体改进：已有工作（如 Zhang et al. 2026a；Wang et al. 2026；Zhang et al. 2026b）让智能体从自身任务求解经验中学习，随时间提升能力。这类方法的核心是利用下游评估作为改进信号——即每次候选版本都要在目标任务分布上反复执行，这是 SelfSearch 要绕开的高成本环节。

2) 评估引导的智能体搜索：论文对比的基线就是在 SWE-bench 开发集上引导十次修订、选择开发分数最高的候选。这类方法类似于“黑盒优化 + 代理奖励”，搜索方向和选择标准都依赖下游评估。论文将其与 SelfSearch 对比，展示无奖励搜索在相似或更好的任务成功率下搜索成本更低。

3) 开放式自我改进与经验共享：检索到的引用“Open-ended self-improvement via experience sharing, 2026”（arXiv:2602.04837）表明存在将多个智能体的改进经验相互共享的工作，这与 SelfSearch 的“记录跨谱系传递”机制相关。另一种参考是 Live-SWE-agent（Chunqiu Steven Xia 等），关注软件工程智能体在任务执行中的持续性改进。

4) 元认知与自修改智能体：引言从 Flavell (1979) 的元认知概念出发，类比人类反思学习过程。SelfSearch 属于智能体自我修改（self-modification）一类，即智能体在代码层面改写自身实现，而非仅更新权重或提示。这一方向包括动态工具创建、策略路由修改、执行循环调整等，论文提到“对策略路由和执行循环的修改”。

5) 无奖励与开放式学习：SelfSearch 的“reward-free search”与无奖励强化学习、进化和探索式方法相关，但具体比较对象未在检索片段中出现，推测论文相关工作部分讨论了这类方法。需要回原文确认（证据缺口）。

总体而言，论文填补的空白不是“能否自我改进”，而是“能否只用自我修改的过程记录（而非任务结果评分）来引导搜索”，并同时提升“任务求解”和“自我修改”两种能力。

Q3: 论文如何解决这个问题？

SelfSearch 是一种双谱系（two-lineage）的奖励无关搜索过程，核心设计如下：

1) 初始智能体与谱系定义：给定初始智能体 B0，系统同时维护两条进化谱系——能力谱系（capability lineage，记为 c）和自适应谱系（adaptive lineage，记为 a）。两者从同一 B0 出发，但搜索方向 d 的定性指令不同：能力谱系重在改进任务求解能力，自适应谱系重在改进自我修改机制本身。最终分别产出最终能力智能体 Bc 和最终自适应智能体 Ba。

2) 修改 Episode 与记录生成：每一代 k，当前智能体 Bk 在隔离容器（无网络、支持并行工具调用）中运行一个自我修改 episode。运行中的智能体在 episode 期间保持不变，其编辑结果定义后继者 Bk+1。形式化地：(B_{k+1}, e_k) = SelfImprove(B_k; E_k, d)，其中 E_k 是之前 episode 的记录集合，d 是定性搜索方向。每个 episode 产生一条记录 e_k，包含推理（Thought）、工具动作（Action）、输出结果（Observation）以及修改前后的差异。记录是跨谱系共享的？论文提到“重复条目可能反映从另一谱系合并的更改”（表 5 注释），说明两条谱系会交换记录或修改。检索到的轨迹示例显示，记录包含如“Let’s inspect the previous trajectory”“The previous output was clipped”“parsing handles default values”“I should add a search tool”等内容，说明记录保存了失败原因分析和工具迭代过程。

3) 无奖励搜索：关键特点是在搜索过程中不使用任何下游任务评估作为奖励信号。智能体仅通过阅读先前修改记录来发现弱点、设计修订和验证效果。Search 方向是定性指令，不提供量化得分。每一步的选择也不基于 checkpoint 的下游表现——最终评估直接面向每个谱系最后一代的 Bc 和 Ba，而非择优挑选中间检查点。

4) 搜索循环与跨代累积：重复该 episode 十代（ten generations），所有检查点都被保留，记录集合 E_k 随代数累积。新修改可以复用旧工具，也可以进一步细化——论文观察到“直接复用工具”和“后续 episode 暴露局限后进一步细化”两种模式。

5) 执行基础设施：episode 在无网络隔离容器中执行，支持并行工具调用（Appendix A 描述执行基础设施）。最终代理与初始代理使用相同的下游执行模型和执行限制，仅在智能体实现（harness）上有差异，从而隔离自我改进带来的影响。

Q4: 论文做了哪些实验？

论文实验覆盖三个基准：SWE-bench Verified（120 个任务）、SWE-bench Multilingual（60 个任务，覆盖 8 种编程语言）、Terminal-Bench 2.1（全部 89 个任务）。模型配置两组：

1) GPT 配置：自我改进模型用 gpt-5.6-sol，下游执行模型用 gpt-5.6-luna，均为 medium reasoning effort。
2) DeepSeek 配置：自我改进模型用 deepseek-v4-pro-0813，下游执行模型用 deepseek-v4-flash-0731。

搜索配置：每个模型配置运行双谱系搜索共十代。Episode 在无网络的隔离容器中执行，支持并行工具调用。保留所有 checkpoint，最终评估各谱系最后一代的能力智能体 Bc 和自适应智能体 Ba，报告个体结果和种群均值（两个谱系成功率的均值），而不是基于下游性能挑选检查点。

成本指标：平均每个任务的执行成本 = 总模型执行成本 / 尝试任务数（包括失败尝试）。种群成功率为两个谱系成功率的平均。

基线比较：
- 评估引导搜索基线：在 SWE-bench 开发集上引导十次修订，对每个基线从开发集候选中选择开发分数最高的版本（评分相同则选最新修订）。比较时排除这十个开发任务本身（避免基线过拟合开发集带来的偏差）。
- 公共九-harness 比较：使用扩展执行限制的设置，将 SelfSearch 产物与包括 Codex 在内的九个公开 harness 对比。注意此比较使用与表 1 不同的执行限制，需看 Appendix D.3 确认细节。

消融实验：在 SWE-bench Verified 上，对每个模型设置比较：(a) 初始智能体 B0；(b) 移除 episode records 的变体（w/o episode records）；(c) 固定 improver 的变体（w/ fixed improver，即改进者自身不随谱系进化）；(d) 完整 SelfSearch。消融在世代-10 的智能体上评估。

机制分析：追踪十代中改动如何发展、如何在两谱系间传递，检查进化工具在下游任务上的使用情况（表 5 记录了每一代两谱系引入的修改类型，如精确文本替换、行范围文件查看、计划修订和失败恢复等）。

Q5: 发现了什么实验现象？

1) 主结果：在全部六个模型-基准组合（2 模型 × 3 基准）中，SelfSearch 最终种群的平均成功率均超过初始智能体。个体提升幅度：Terminal-Bench 2.1 最高达 11.2 个百分点、SWE-bench Multilingual 6.7 个百分点、SWE-bench Verified 5.0 个百分点。摘要明确提到“population-mean success over the initial agent in all six model–benchmark settings”。

2) 成本与效率：在 SWE-bench Multilingual 上，一个 SelfSearch 智能体提升 5.0 个百分点，同时在初始与进化智能体都解决的任务上执行成本降低 38.5%。这意味着改进不仅带来能力提升，还带来 token/计算效率改善（推测与工具复用和更精准的遍历有关）。另一条检索片段显示（具体设置不明）：能力智能体从 43.8% 提升到 55.1%，平均成本增加 3.1%；自适应智能体提升到 44.9%，平均成本降低 64%（片段截断为“64 (%) 6.2%”，可能指 64% 或 6.2%——建议对照原文确认）。这说明能力谱系和自适应谱系在成本-性能权衡上呈现不同方向。

3) 消融趋势：
- 去除 episode records：GPT 设置种群均值下降 2.1 个百分点，DeepSeek 设置下降 2.9 个百分点。
- 固定 improver：均值下降 1.7（GPT）和 2.9（DeepSeek）个百分点。
- 完整 SelfSearch 在两个模型设置下均取得最高均值（GPT: 76.2% vs 初始 75.0%；DeepSeek: 86.7% vs 初始 81.7%）。
值得注意的是，在 DeepSeek 设置下，w/o episode records 和 w/ fixed improver 的能力谱系成功率为 82.5%，自适应谱系 85.0%，均值 83.8%，已经高于初始 81.7%，但低于完整 SelfSearch 的 86.7%。说明即使去掉某些成分，仅靠无奖励的开放式修改仍有正收益，但记录和可进化的改进者各自都有增量贡献。

4) 跨代演变与谱系间传递：表 5 显示十代修改的主题变化，例如第 1 代出现精确文本替换、计划修订和失败恢复；第 2 代出现行范围文件查看、精确文本替换；后续代出现工具细化等。重复条目可能来自另一谱系的合并，说明两谱系之间存在信息传递，且修改会反复迭代（如工具先被直接复用，后来因暴露局限又被进一步细化）。

5) 负结果与反直觉点：尽管无奖励搜索整体有效，但存在方向性差异——能力谱系有时成本上升（+3.1%），自适应谱系成本大幅下降却成功率提升较小（43.8%→44.9%）。这说明“追求更高效”和“追求更高成功率”可能是两种不同的改进目标，自适应谱系的改进方向更多偏向成本优化。另一个反直觉点：消融中去除 episode records 时 DeepSeek 自适应谱系仍达 85.0%，高于 GPT 完整方法（76.2%），暗示模型自身能力（或自我改进基础能力）比特定搜索机制的影响更大。

6) 公共对比：在九-harness 比较中（使用扩展执行限制），SelfSearch 产物解决 Terminal-Bench 2.1 的 73/89（82.0%），与最高分 harness Codex 持平，搜索成本仅 4.03 美元。这显示极低搜索成本也能产出极具竞争力的智能体实现。

Q6: 有什么可以进一步探索的点？

论文展示的无奖励自我修改搜索打开多个后续研究点：

1) 记录表示与压缩：SelfSearch 依赖完整 episode 记录（推理、动作、结果），随着代际增加记录集合无限膨胀。未来可学习如何选择、摘要或压缩记录，保留高信息量失败/成功片段，提升搜索效率。这也可与检索增强结合。

2) 谱系间信息传递机制：论文观察到两谱系间修改相互融合，但未深入分析何种记录、何时传递最有效。可研究谱系间的记录路由策略、合并时机，以及是否应建立多条异质谱系（如同时优化速度、鲁棒性、工具多样性）来提升种群多样性。

3) 部分奖励信号的混合：论文严格限制为 reward-free，但实际场景中可能有便宜的部分反馈（如单元测试是否通过、成本阈值、用户点击）。研究“无奖励为主 + 极低频率奖励校准”的混合模式，可能在保持泛化的同时规避无效搜索分支。

4) 元认知层面的理论分析：为什么自我修改的经验能改善后续修改决策？能否形式化地刻画“改进策略的改进”？（例如把改进策略视为一条可学习的高层策略，用记录作为上下文）。目前缺乏理论模型解释无奖励搜索中的 credit assignment。

5) 跨模型与跨任务泛化：摘要提到“gains transferring across execution models”（改进收益跨执行模型转移），这意味着进化出的 harness 可能在更强的模型上依然有效。未来可系统测试进化结果在新模型版本、新语言、新任务类型上的迁移性，以及是否出现任务分布外的退化。

6) 安全性：让智能体自主修改自身实现引入了风险（如工具被错误修改、执行循环死循环、奖励黑客压力消失后仍有其他意外行为）。论文未讨论安全约束，后续可研究在无奖励搜索中加入安全性过滤器或可回滚机制。

7) 成本-性能帕累托前沿：实验已显示能力谱系和自适应谱系在成本和成功率上取向不同，可系统搜索两目标（成功率×执行成本）的帕累托前沿，并研究搜索方向和定性指令对最终位置的影响。

8) 更长时间的进化与规模化：十代是否足够？会不会发生过拟合或退化？更长世代序列、更大初始种群、不同模型家族（如开源模型）下的稳健性值得探索。注意论文题目为“Reward-Free Search”，未来也可与强化学习微调方式对比。

Q7: 总结一下论文的主要内容

论文围绕“智能体能否从自我改进的过程中学习更好的自我改进”这一元认知问题展开。出发点是人通过反思自身学习过程来提升学习能力，而具备编码能力的 LLM 智能体可以改写自身的指令、工具和执行逻辑，从而可以在实现层面尝试这种元认知。现有自我改进方法几乎都依赖下游评估作为搜索信号：对每个候选智能体在开发集上运行任务，按成功率迭代。这导致两个弊端：评估成本随任务数和迭代次数线性增长；改进方向被约束在评估任务分布内。SelfSearch 提出一种完全无奖励的替代搜索：不运行任何下游任务，只让智能体阅读此前自我修改 episode 的记录（包括推理轨迹、工具动作、输出结果），从中识别弱点、设计修改并验证。每条修改 episode 既产生新智能体，也产生一条新记录，从而形成“经验累积 → 更好修改”的正反馈。

方法上，SelfSearch 运行双谱系搜索：能力谱系聚焦任务求解能力，自适应谱系聚焦修改机制本身；两者从同一初始智能体出发，各自独立演化十代，但记录允许跨谱系渗透（表 5 中重复条目可反映另一谱系的并入）。搜索过程中没有任何下游评估信号，最终评估时不对途中 checkpoint 挑选，而是直接评估两条谱系各自的最终版本 Bc 和 Ba，报告个体成功率和种群均值。

实验使用 GPT（gpt-5.6-sol 改进 + gpt-5.6-luna 执行）和 DeepSeek（deepseek-v4-pro 改进 + deepseek-v4-flash 执行）两组模型配置，在 SWE-bench Verified（120 任务）、SWE-bench Multilingual（60 任务、8 语言）、Terminal-Bench 2.1（89 任务）上对比初始、能力进化、自适应进化智能体。结果在所有六个模型-基准组合中种群均值都超过初始，个体最高提升 11.2 个百分点（Terminal-Bench 2.1），并在 SWE-bench Multilingual 上出现 5.0 个百分点的提升加 38.5% 执行成本下降（对双方案均能解决的任务）。与评估引导基线相比，SelfSearch 在更低搜索成本下取得竞争性成功率；在公共九-harness 比较中，仅花 4.03 美元搜索成本产出的 DeepSeek V4 Flash harness 以 82.0% 的通过率与最高分 Codex 持平。

机制消融在 SWE-bench Verified 上表明：去掉 episode 记录使均值下降 2.1（GPT）和 2.9（DeepSeek）个百分点；固定改进者使其下降 1.7 和 2.9 个百分点；完整方法在两个设置上都取得最高均值。过程分析显示修改主题从文本操作、行范围查看扩展到计划修订、失败恢复和工具迭代，且工具会被先复用再因暴露局限而细化。结论认为：自我修改的过程记录确实携带可用于未来改进的、可迁移的经验，无奖励搜索是一种低成本且有效的智能体改进范式。论文还声明使用生成式 AI 工具辅助概念构建、实验设计、轨迹分析和文献整理，但未生成合成数据集。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你的 agent 方向（权重 0.10）直接相关：论文核心是智能体自我修改与自我改进，属于 LLM agent 自主性研究的关键一环。

## 基本信息

- 作者：Jungwoo Yang, Injin Kong, Yohan Jo
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37968v2`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 retrieved_evidence 中的 Introduction/Results/Discussion 语义检索片段，并结合论文摘要与 section 文本补全；部分推断已行内标注。
