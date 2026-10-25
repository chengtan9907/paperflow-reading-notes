---
user_id: "cheng tan"
paper_id: 1413
arxiv_id: "2606.25832v1"
title: "MiniOpt: Reasoning to Model and Solve General Optimization Problems with Limited Resources"
institution: "根据论文首页脚注，作者分属华东师范大学（East China Normal University，上海）、蚂蚁集团（AntGroup，杭州）和南方科技大学（Southern University of Science and Technology，深圳）；其中 Ke Zhao、Zixiang Di、Yaolin Wen、Bingdong Li、Xiangfeng Wang、Hong Qian 属华东师范大学，Xiang Shu、Qitao Shi、Xingyu Lu、Jun Zhou 属蚂蚁集团，Ke Tang 属南方科技大学；通讯作者为 Hong Qian。"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Jun 2026/2606.25832v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25832v1.pdf"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-10-12T13:29:24"
---
# MiniOpt: Reasoning to Model and Solve General Optimization Problems with Limited Resources

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · optimization generalization · reinforcement learning · reasoning-to-model-and-solve

## 一句话总结

MiniOpt 提出了一套面向优化问题的强化学习（RLVR）框架，通过“先结构化建模、再生成可执行求解器”的 reasoning-to-model-and-solve 范式，配合分级奖励 OptReward 和面向优化的策略优化算法，使 3B 级小型 LLM 在有限训练资源下获得较强的优化泛化建模与求解能力。

## 摘要

> 论文摘要（重组版）：传统优化求解器虽高效，但其使用依赖专家将自然语言描述的问题手工转换为精确数学模型与可执行代码，费时且难以泛化。LLM 为此提供了新路径，代表性工作如 LLMOPT、BPP-Search、Text2Zinc 通过将自然语言解析为结构化 formulation 并生成求解器代码，显著推进了该方向。然而，现有 LLM 方案通常依赖大规模监督数据集、昂贵的推理标注和中间步骤验证，训练开销大，难以在小规模模型和有限资源下部署。为应对这一挑战，论文提出 MiniOpt：一个基于 RLVR（可验证奖励的强化学习）的框架，采用 reasoning-to-model-and-solve 范式，将优化推理分解为结构化优化建模与可执行求解器生成两部分；提出 OptReward，一种具有分级评分结构的奖励函数，联合评估建模（formulation）与求解（solution）质量，从而无需专家示范即可学习策略；并设计面向优化的策略优化策略，提升紧凑模型的探索效率与 RL 训练稳定性。实验表明 MiniOpt-3B 在有限资源下展现出较强的优化泛化性能。

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：如何在有限的训练数据、标注和算力条件下，让小型 LLM 获得跨问题类型与跨场景的“优化泛化”能力。具体可以从以下几个层面理解：
1. 专家瓶颈：传统优化求解器本身高效，但把现实问题用自然语言描述后，通常需要专家将其手工转化为数学规划形式并编写可执行代码。这一过程耗时、易错，且换个场景就要重新建模，不具备通用性。
2. LLM 自动化路线的资源矛盾：LLM 使“自然语言 → 结构化优化建模 → 求解器代码”的自动化成为可能，已有 LLMOPT、BPP-Search、Text2Zinc 等工作。但这些方法普遍依赖大规模监督数据、昂贵的推理标注和中间步骤验证，训练成本很高，限制了其在中小企业和移动设备等资源受限场景中的部署。
3. 优化泛化尚未解决：论文沿用 LLMOPT 对“优化泛化”的定义，即模型能否对未见过的优化问题，在不同优化类型和不同场景下完成 formulation 与求解。现有方法更多是针对特定类型或特定数据分布的拟合，跨类型、跨场景的泛化仍不充分。
4. 小型模型 + 有限资源的双重约束：MiniOpt 的目标不是继续堆数据和算力，而是在参数规模小（如 3B）、训练数据有限、没有专家示范的条件下，仍能稳定学出可泛化的优化建模与求解策略。
5. RL 自身难点：优化任务的解空间大、奖励稀疏，模型输出又必须同时满足“数学上正确”和“代码可执行”两个条件。现有 RL 方法往往需要过程验证或密集标注；MiniOpt 需要回答的是如何设计可自动计算、可分级、且能引导紧凑模型稳定探索的奖励与策略更新方式。

Q2: 有哪些相关研究？

从摘要、引言片段和实验对比信息可以重构出以下相关研究脉络；由于检索证据主要来自 Abstract 和 Introduction，Related Work 原文并未完整进入证据，下面的分类属于基于摘要、片段和领域惯例的合理推断：
1. 自然语言到优化建模（NL2Opt）：代表性工作包括 LLMOPT、BPP-Search、Text2Zinc。它们的共同路线是把自然语言问题描述解析为结构化 formulation，再生成求解器代码。MiniOpt 明确延续 LLMOPT 的“优化泛化”定义，并以其为方法对照。
2. 通用 LLM 作为优化器/代码生成器：实验中出现的基线包括 Qwen2.5、DeepSeek-V3 以及“通用推理模型”（general thinking models）。这暗示相关工作还包括直接用通用大模型做数学推理或代码生成，而不是专门面向优化问题训练。
3. 基于可验证奖励的强化学习（RLVR）：MiniOpt 的方法背景是 RLVR 和 GRPO 一类轻量策略优化方法。论文提出的 OptaGRPO 属于对 GRPO 的优化任务定制改造；相关领域还包括过程奖励、结果奖励、格式奖励和可执行代码验证等。
4. 资源高效与小模型训练：论文强调“有限训练资源”和“紧凑模型”，相关工作可延伸到数据蒸馏、合成数据、领域自适应训练、课程学习和小模型推理增强等方向。引言片段还提到“通过针对性训练数据强化领域特定技能”，说明作者关注领域数据的作用。
5. Agentic 应用与统一表示：检索到的引言片段提到 LLM 的“理解、多模态和推理泛化能力”为 agentic 应用打开新路径，并且出现“five-element formulation”的统一表示。合理推断该工作与“LLM 作为 agent 自动完成建模-求解闭环”这一更广的研究线相关，即把优化求解器作为工具，把 formulation 作为中间动作。

Q3: 论文如何解决这个问题？

MiniOpt 的解决方案可以拆成“任务分解、奖励设计、策略优化、训练数据与两阶段训练”四个部分：
1. Reasoning-to-Model-and-Solve 范式：核心思想是不让模型直接输出最终答案，而是把优化推理显式分解为两个阶段：先进行结构化优化建模（structured optimization modeling），再生成可执行求解器（executable solver generation）。这种分解把“理解问题”和“编写可运行代码”解耦，使中间过程可以被检查、被奖励、被学习。
2. 统一建模表示：检索片段中出现“five-element formulation”，合理推断论文设计了一种五要素的统一 formulation 表示，作为连接自然语言问题与求解器代码的中间结构。具体五要素内容未在检索到的截断文本中给出（推测与决策变量、目标函数、约束、参数、求解指令等高度相关），需要在原文 Method 部分确认。
3. OptReward 分级奖励：论文提出 OptReward，具有层次化评分结构，联合评估 formulation 质量与 solution 质量，核心思路是“让中间过程易于验证”。检索片段显示其包含格式奖励：要求模型按正确顺序输出 <answer> ... </answer> 等结构，所有条件满足则格式分 Sfmt=+1，否则 Sfmt=-1。此外，摘要明确说明它不需要专家示范，各评分组件来自确定性的解析或执行结果，从而规避昂贵的中间步骤人工标注。
4. 优化导向的策略优化策略：论文提出面向优化的策略优化方法。检索片段中出现“OptaGRPO”字样，合理推断这是作者对 GRPO 的定制变体，用于提升探索效率并稳定小模型的 RL 训练。消融片段显示“将 OptaGRPO 还原为原始 GRPO”会导致性能下降，说明该组件是方法的关键部分。
5. 两阶段训练与数据构建：检索片段提到“范式习得（paradigm acquisition）与优化泛化（optimization generalization）两个阶段”，两者共享“解析、执行”等 pipeline；推测第一阶段先让模型稳定学会输出符合格式的 formulation 和 solver，第二阶段再用 RL 提升跨问题泛化。数据方面，论文按场景频率匹配完整数据集的分布，并对样本不足的类型补充 MILP 问题，使各优化类型达到期望的均衡分布；这些数据作为 Stage-2 RL 训练数据使用。

Q4: 论文做了哪些实验？

受检索证据所限，实验细节并不完整；根据 Abstract、Introduction 命中片段和实验对比信息，可以确认论文做了以下实验：
1. 主模型与基线对比：以 MiniOpt-3B 为主模型，与通用 LLM（Qwen2.5、DeepSeek-V3）、通用推理模型（general thinking models）以及既有优化 LLM 方法进行对比。检索片段提到“与 3B 模型相比在 LP、IP、MILP formulation 上保持 10% 到 13% 的绝对提升”，说明实验覆盖线性规划（LP）、整数规划（IP）和混合整数线性规划（MILP）。
2. 指标：片段中出现 SA 与 ER 两个指标。合理推断 SA 为求解正确率/求解精度（solve accuracy），ER 为可执行率/执行成功率（execution rate）；其精确定义与评测协议需要到原文实验部分确认。
3. 参数规模实验：论文重点验证“即使在更小参数规模下仍保持优越性能”，推测有 MiniOpt-3B 与更大模型（如 7B 或更大通用模型）的跨规模对比，以及参数规模缩放趋势。
4. 消融实验：至少包括策略优化组件消融，证据显示“将 OptaGRPO 还原为原始 GRPO”后性能下降；此外还有数据配比/数据补充相关的设置（如用 MILP 问题补足类型分布），推测也有对应消融。
5. 训练资源与数据效率分析：由于论文问题定义是“limited resources”，预计实验包含训练数据量、标注成本或训练步数等资源维度分析；但该部分未在检索证据中明确出现，需要按需阅读原文确认。

Q5: 发现了什么实验现象？

根据检索到的片段，论文实验中最值得注意的现象包括：
1. 小模型的优化泛化优势：MiniOpt 的核心卖点是“在减小参数规模后仍能保持优越性能”。这与常见的“越大越好” scaling 直觉形成对比，说明专门的任务分解、奖励和训练策略可以在一定程度上弥补模型容量不足。
2. 对 3B 级模型的显著提升：在 LP、IP、MILP 三类 formulation 上，MiniOpt 相对 3B 通用模型保持 10% 到 13% 的绝对提升。这意味着提升主要不是靠模型变大，而是靠训练目标与推理结构的优化。
3. 策略优化组件的必要性：将 OptaGRPO 还原为原始 GRPO 后出现明显性能回落。检索片段中出现“SA 55.07%、平均 ER 82.73%，ΔSA=-4.58%、ΔER=-5.19%”等数字，合理推断这是某组设置下还原/移除 OptaGRPO 后的指标变化：SA 下降约 4.58 个百分点，ER 下降约 5.19 个百分点。具体对应哪组对比仍需原文确认。
4. 执行率与求解正确率之间的张力：若上述 SA 与 ER 来自同一测试场景，则 ER（约 82.73%）明显高于 SA（约 55.07%），说明模型生成的代码“能跑起来”并不等于“能解对”；瓶颈更多在 formulation 与求解逻辑的正确性，而不是代码语法/可执行性。这是一个重要的指标间张力，值得在原文中核实。
5. 数据配比的价值：论文刻意让各优化类型在训练数据中达到均衡分布，并用 MILP 问题补足短缺类型。合理推断这能避免模型偏向数据占比高的类型，对跨类型泛化有正面作用；但该效果的具体消融数字未在检索证据中给出。

Q6: 有什么可以进一步探索的点？

结合论文定位和现有证据，可以列出以下可进一步探索的方向：
1. 规模效应：MiniOpt-3B 之后，可以系统研究 1B/3B/7B/14B 等不同规模下的优化泛化 scaling 曲线，以及“任务分解 + 奖励设计”能在多大程度上替代参数增长。
2. 更广的优化问题类型：目前证据主要集中在 LP、IP、MILP；可扩展到非线性规划、随机优化、鲁棒优化、组合优化（路径规划、调度、装箱）以及带隐式约束或黑盒目标的问题。
3. 求解器协同与自我修正：当前范式是生成可执行求解器；下一步可以让模型与外部求解器、执行报错信息、数值检查器形成闭环，在失败后自动修正 formulation 或代码，从而提升 ER 向 SA 的转化率。
4. 奖励设计的深化：OptReward 的分级结构可以进一步引入学习到的软奖励、过程奖励、反例引导奖励，或对 Sfmt 这种硬性 +1/-1 格式奖励做平滑化，以缓解奖励稀疏问题。
5. 数据与课程学习：可探索自动生成更难优化问题的课程策略、领域自适应数据合成，以及如何在极少样本下快速适配新优化类型。
6. 部署导向优化：针对移动端和中小企业场景，可研究量化、蒸馏、推理加速，以及“小模型 + 外部工具/求解器”的混合架构。
7. 理论分析：对 OptaGRPO 的探索效率与稳定性、分级奖励的 credit assignment、以及优化泛化与数据分布之间的关系，都缺少理论刻画。
8. Agentic 与多学科应用：自然语言到优化求解的范式可嵌入更广泛的 agent 工作流，例如供应链决策、交通调度、实验设计、科学计算中的自动建模（AI for Science），以及多目标决策支持系统。

Q7: 总结一下论文的主要内容

这篇论文围绕“让小型 LLM 在有限资源下学会求解多种未见过的优化问题”这一目标，提出了一个完整的强化学习方案 MiniOpt。
从研究动机看，传统优化求解器虽然计算高效，但使用门槛高：需要专家把自然语言描述的现实问题转化为数学规划和可执行代码，过程不通用且耗时。LLM 的出现使自动化建模与求解成为可能，但已有方法依赖大规模监督数据、昂贵推理标注和中间步骤验证，训练开销大，难以部署到资源受限的中小企业或移动设备。论文因此把问题定义为“优化泛化”，即模型能否对未见过的优化问题跨类型、跨场景地完成建模与求解。
在技术路线上，MiniOpt 的核心是“reasoning-to-model-and-solve”范式：把优化推理显式拆分为结构化优化建模与可执行求解器生成。两步分解使模型先学会把自然语言问题转成统一的形式化表示（检索片段称为“five-element formulation”，具体五要素需原文确认），再基于该表示生成可运行代码。为了让训练不依赖专家示范和人工过程标注，论文提出 OptReward：一个层级化评分奖励函数，同时评估 formulation 与 solution 质量；其评分组件来自确定性的解析或执行结果。检索片段显示格式层有明确的 Sfmt=+1/-1 条件，要求模型按正确顺序输出 <answer> ... </answer> 结构，体现了“让中间过程易于验证”的设计思想。在策略优化上，论文提出面向优化的策略优化方法，检索片段中出现的 OptaGRPO 合理推断为对 GRPO 的优化定制变体，目标是提升探索效率并稳定紧凑模型的 RL 训练；消融片段显示一旦退回原始 GRPO，性能就会下降，说明该组件不可或缺。训练流程上，论文分为“范式习得”和“优化泛化”两个阶段，两阶段共享解析、执行等 pipeline；训练数据按场景频率匹配完整数据集分布，并用 MILP 问题补足样本不足的类型，以达到类型均衡。
在实验上，MiniOpt-3B 与 Qwen2.5、DeepSeek-V3 等通用 LLM 以及通用推理模型对比，覆盖 LP、IP、MILP 等 formulation 类型；结果显示 MiniOpt 在参数规模更小的情况下仍保持优势，在 LP/IP/MILP 上相对 3B 模型有 10% 到 13% 的绝对提升。检索到的指标数字（如 SA 55.07%、平均 ER 82.73%）暗示执行成功率高于求解正确率，生成代码可执行不一定意味着求解正确，问题瓶颈更可能在建模与求解逻辑层面。消融实验表明 OptaGRPO 和均衡数据配比对最终性能有实质贡献。
总体而言，MiniOpt 的价值在于把“任务分解、奖励设计、策略优化、数据构造”作为一个整体系统来设计，而不是简单地在更大模型上堆数据；这使其成为资源受限条件下推进 LLM 自动优化建模与求解的代表性工作。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与 Agent 方向直接相关：论文把优化建模与求解器生成视为可验证的 agentic 推理过程，使用统一 formulation 作为中间动作，属于“LLM 作为 agent 调用工具/求解器”的研究线。

## 基本信息

- 作者：Ke Zhao, Zixiang Di, Hong Qian, Xiang Shu, Yaolin Wen, Qitao Shi, Bingdong Li, Xingyu Lu, Xiangfeng Wang, Jun Zhou, Ke Tang, Yang Yu
- 机构：根据论文首页脚注，作者分属华东师范大学（East China Normal University，上海）、蚂蚁集团（AntGroup，杭州）和南方科技大学（Southern University of Science and Technology，深圳）；其中 Ke Zhao、Zixiang Di、Yaolin Wen、Bingdong Li、Xiangfeng Wang、Hong Qian 属华东师范大学，Xiang Shu、Qitao Shi、Xingyu Lu、Jun Zhou 属蚂蚁集团，Ke Tang 属南方科技大学；通讯作者为 Hong Qian。
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-06-24
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25832v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 语义检索命中的 Introduction/Abstract 片段与论文元数据；部分细节（如 SA/ER 指标定义、OptaGRPO 名称、具体消融设置、five-element formulation 内容）因原文截断或证据不足，已在对应字段标注为合理推断或推测。
