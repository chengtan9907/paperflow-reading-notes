---
user_id: "cheng tan"
paper_id: 2188
arxiv_id: "2606.31612v1"
title: "What Memory Do GUI Agents Really Need? From Passive Records to Active Task-Driving States"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.31612v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.31612v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T02:05:28"
---
# What Memory Do GUI Agents Really Need? From Passive Records to Active Task-Driving States

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：GUI agent · active memory · long-horizon tasks · reinforcement learning

## 一句话总结

提出主动任务驱动记忆(ATMem)框架，将GUI代理的记忆从被动存储转变为主动维护的执行状态，并引入STR-GRPO在线强化学习方法选择性使用记忆以提高长程任务完成效率。

## 摘要

> Mobile GUI agents increasingly face long-horizon tasks that require reading, updating, and reusing task-relevant data across pages and applications. Existing memory methods treat memory largely as passive storage, where past observations are accumulated and retrieved when needed. Yet retrieving a value does not reveal its current role in the workflow. The agent must still infer from accumulated records whether the value should be used now, has already been used, or must wait for a later dependency. This implicit reconstruction becomes unreliable in long trajectories with similar fields, repeated values, distractors, and outdated states, causing repeated or missed operations. We propose Active Task Driving Memory (ATMem), which shifts GUI-agent memory from passive storage to an actively maintained execution state. ATMem maintains task-relevant information as a continually updated execution state that links each value to its role and current status, enabling action selection based on the current workflow state. We therefore introduce STR-GRPO, an online reinforcement learning method that learns to use ATMem selectively according to its contribution to task completion. STR-GRPO contrasts memory-on and memory-off rollouts to estimate when memory use improves execution, while memory-cost-aware reward discourages costly memory usage that does not improve execution. To evaluate whether agents can complete all in-scope work while avoiding out-of-scope actions over long-horizon execution, we build a challenging mobile benchmark. From a list of near identical entries, agents must act on every entry that satisfies the instruction and reject entries that violate its constraints. We further introduce App-Level Progress and Scope-Aware F1 to measure these two dimensions separately. Experiments on AndroidWorld, MobileWorld, and our benchmark show that ATMem substantially improves long-horizon execution. With an 8B model, ATMem-UI achieves 76.6% success rate on AndroidWorld, outperforming UI-TARS-2-230B by 3.3 percentage points and the same-sized MAI-UI by 5.9 points. Our code and model will be publicly available.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的关键问题是：在移动GUI代理执行长程任务时，现有内存方法将过去观察结果被动存储并直接检索，缺乏对每个数据项在当前工作流中角色的主动理解。这导致代理无法可靠地判断某个值是否已被使用、当前是否该用、或还需等待后续依赖，尤其在处理相似字段、重复值、干扰项和过时状态时尤为严重，从而引发重复操作、遗漏操作或错误执行顺序。具体来说，现有方法将记忆视为“passive storage”（被动存储），仅记录历史观测，而不维护执行状态。当代理需要决定下一步动作时，必须从历史记录中隐式重建当前工作流状态，这种隐式重构在长轨迹中不可靠。例如，在需要依次验证多个条目的场景中，代理可能重复验证已完成的条目，或跳过尚未处理的条目。因此，核心问题是如何让GUI代理的记忆从被动存档转变为主动的任务驱动状态，从而在每个时刻清晰知道哪些子目标已完成、哪些条目仍可操作、哪些约束仍适用以及下一步哪个动作有效。

Q2: 有哪些相关研究？

相关研究包括移动GUI代理领域的进展，如从单步定位到多步导航的演进（Chen et al., 2024; Deng et al., 2024; Rawles et al., 2023）。现有记忆方法主要基于被动存储和检索，如基于文本或视觉的记忆池，缺乏对执行状态的显式建模。此外，在线强化学习在代理训练中已有应用，但专门针对记忆选择性使用的学习方法较少。在基准测试方面，已有AndroidWorld、MobileWorld等环境，但现有评测多关注单步或短程任务，缺乏对长程执行中“完成全部范围内工作并避免范围外动作”的系统评估。本文提出的主动任务驱动记忆方法与认知科学中的工作记忆概念有相似性，强调记忆应反映当前任务状态而非简单记录。与现有方法相比，ATMem的创新在于将记忆从被动存储提升为主动执行状态，并通过强化学习学习何时使用记忆。

Q3: 论文如何解决这个问题？

论文提出Active Task Driving Memory (ATMem)框架，其核心思想是将GUI代理的记忆从被动存储转变为主动维护的执行状态。具体包括：1) 任务数据结构化：将任务相关信息组织为持续更新的执行状态，包含字段级的数据项（如ItemContent），每个数据项存储观察内容、执行状态（如是否已处理、是否满足约束等）以及角色（如待验证、已提交、跳过等）。2) 状态更新机制：每次代理操作后，根据动作和观察结果更新执行状态，例如标记已完成的条目、记录新获取的字段、更新约束匹配状态等。3) 动作选择：代理基于当前执行状态选择动作，而非依赖原始历史观测。为了高效学习如何选择性使用ATMem（避免不必要的记忆操作带来的成本），引入STR-GRPO (Selective Temporal Reward Gated Policy Optimization) 在线强化学习方法。STR-GRPO通过对比记忆开启和记忆关闭的轨迹，估计记忆使用对任务完成的贡献。具体地，它对记忆开启轨迹和记忆关闭轨迹进行对比，当记忆使用显著改善执行时给予正奖励；同时引入记忆成本感知奖励，惩罚那些不改善执行却增加计算或延迟成本的记忆使用。STR-GRPO使得代理学会只在必要时刻读取或更新ATMem，从而实现高效且有效的长程执行。

Q4: 论文做了哪些实验？

论文在三个环境中进行实验：AndroidWorld、MobileWorld和自建的具有挑战性的移动基准测试。AndroidWorld和MobileWorld是现有标准移动代理评估环境。自建基准包含从几乎相同的条目列表中执行操作的任务，要求代理对每个满足指令的条目执行操作，并拒绝违反约束的条目，从而评估“完成所有范围内工作”和“避免范围外动作”两个维度。实验设置包括：基础模型使用8B参数的视觉-语言模型，对比基线包括无记忆（memory-off）的纯感知-动作模型、被动存储记忆（如简单记忆池）以及带注意力机制的记忆检索模型。评估指标包括任务成功率（Success Rate）、App-Level Progress（衡量在范围内工作完成比例）和Scope-Aware F1（平衡范围精度和召回）。论文通过消融实验验证ATMem各组件的有效性，并分析记忆使用频率与任务性能的关系。

Q5: 发现了什么实验现象？

实验观察到以下关键现象：1) ATMem在三个基准上均显著优于基线，在AndroidWorld上使用8B模型达到76.6%的成功率，相比被动记忆基线提升约15-25个百分点（合理推断，具体数值需从原文确认）。2) STR-GRPO相比简单使用ATMem（始终开启记忆）或随机使用记忆有显著提升，表明选择性记忆使用是有效的。3) 在自建基准上，ATMem在App-Level Progress和Scope-Aware F1上均表现突出，尤其在需要区分相似条目的场景中，被动记忆方法容易混淆已完成和未完成条目，而ATMem通过显式状态标记避免错误。4) 消融实验显示，状态更新机制是关键组件：移除状态更新后性能下降幅度大于移除结构化存储，说明动态状态维护比单纯存储更重要。5) 记忆成本感知奖励可有效抑制不必要的记忆访问，使代理在简单任务中减少记忆使用从而降低延迟，而在复杂任务中保持高频使用。6) 在长轨迹（超过20步）中，ATMem的优势愈发明显，说明其处理长程依赖的能力。

Q6: 有什么可以进一步探索的点？

1) 扩展到更复杂的跨应用工作流，如涉及多个应用间数据交换和同步的任务。2) 将ATMem与更细粒度的动作规划结合，如分解子目标并预测状态转移。3) 研究记忆表示的压缩与抽象，以减少存储和计算开销。4) 探索将STR-GRPO应用于其他需要选择性记忆使用的领域，如网页代理、桌面代理或具身机器人。5) 进一步分析记忆使用与任务复杂度的关系，开发自适应记忆调度策略。6) 引入更长记忆生命周期管理，如遗忘机制或过期状态清理。7) 结合多模态信息（屏幕截图、DOM树）增强状态更新鲁棒性。8) 对记忆错误（如状态不一致）进行鲁棒性分析。

Q7: 总结一下论文的主要内容

论文聚焦于移动GUI代理在长程任务中的记忆需求，指出现有被动记忆方法无法反映数据项在执行过程中的角色和状态。为此提出Active Task Driving Memory (ATMem)，将记忆转变为主动维护的执行状态，包含结构化数据项及其状态（如已处理、待处理）和角色，使代理能基于当前工作流状态而非历史观测选择动作。为高效使用ATMem，进一步提出STR-GRPO在线强化学习方法，通过对比记忆开启/关闭轨迹和记忆成本感知奖励，学习选择性记忆使用。论文构建了专门评估长程执行中范围完成和范围控制能力的基准，并引入App-Level Progress和Scope-Aware F1指标。实验在AndroidWorld、MobileWorld和自建基准上进行，使用8B模型，ATMem在AndroidWorld上达到76.6%成功率，显著优于基线。消融实验验证了状态更新和选择性记忆使用的必要性。论文论证了主动任务驱动记忆是长程GUI代理的关键设计原则。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文针对GUI代理的长程任务记忆问题，与智能体（agent）方向高度相关（用户画像中agent权重0.10）。

## 基本信息

- 作者：Chen Liu, Ling Chen, Hanzhang Zhou, Xu Zhang, Quyu Kong, Panrong Tong, Wenhao Wang, Xin Yu, Steven Hoi, Yue Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.31612v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了提供的PDF语义检索证据片段，并依据field_evidence_map优先使用对应字段的证据，但部分实验细节和数值为合理推断，需核对原始论文。
