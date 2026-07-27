---
user_id: "cheng tan"
paper_id: 5475
arxiv_id: "2607.21051"
title: "Sample-Efficient Learning from Agent Experience"
publish_date: "2026-07-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.21051.pdf"
pdf_url: "https://arxiv.org/pdf/2607.21051"
abs_url: "https://arxiv.org/abs/2607.21051"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-24T13:15:35"
---
# Sample-Efficient Learning from Agent Experience

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：experience distillation · context distillation · in-context learning · sample efficiency

## 一句话总结

提出 Experience Distillation 方法，将智能体交互历史中的上下文学习增益蒸馏到模型权重，在 749 个软件工程任务和 6 个文字冒险游戏中保留至少 64.8% 的上下文学习提升，同时达到比强化学习基线样本效率高一个数量级的性能。

## 摘要

> Real-world agent learning is often constrained by costly environment interactions, such as running time-consuming experiments or obtaining human feedback. In-context learning offers a highly sample-efficient way for agents to learn from their own interaction histories, but its gains disappear once that experience is removed from the context. Separately, context distillation provides a mechanism for internalizing contextual information into model weights. However, applying it to agents' interaction histories without sacrificing environment sample efficiency remains underexplored. We term this problem Experience Distillation and develop an implementation that requires no further environment interaction beyond the collected experience. Experiments on 749 curated software-engineering tasks and six text-adventure games show that it retains at least 64.8\% of the gains from in-context learning across both domains, whereas direct supervised fine-tuning on the collected experience recovers only 3.8\%. Compared with classical reinforcement-learning baselines, in-context learning from trial-and-error experience followed by Experience Distillation matches their performance with at least \(9.6\times\) fewer environment samples.

Q1: 这篇论文试图解决什么问题？

论文试图解决两个关键问题：(1) 智能体在真实世界中学习时，环境交互通常代价高昂（如运行耗时的实验或获取人工反馈），因此需要一种样本高效的机制来从有限的交互经验中学习。(2) 上下文学习（ICL）虽然可以利用智能体自身的交互历史作为上下文来进行决策提升，但这种提升严重依赖上下文的存在：一旦交互历史从上下文中移除，模型就无法保留这些改进能力。上下文蒸馏虽然可以将上下文信息内化到模型参数中，但现有方法要么需要额外的环境交互（如在线蒸馏），要么不是针对智能体轨迹设计。因此，本文研究的是如何在无需额外环境交互的前提下，将智能体从自身交互历史中通过 ICL 获得的经验蒸馏到模型权重中，使得模型在独立运行时仍能保持这些经验带来的性能提升。核心问题是：如何定义和实现 Experience Distillation，并验证其在样本效率上的优势。

Q2: 有哪些相关研究？

相关工作包括：(1) 上下文学习（In-Context Learning, ICL）：在大量语言模型研究中被提出，利用输入上下文中的示例来引导模型输出，但在智能体环境中其增益依赖于上下文中提供的轨迹。(2) 上下文蒸馏（Context Distillation）：将上下文中的行为模式转移到模型权重中，如通过模拟教师-学生范式。本文引用了策略上下文蒸馏和 Penaloza et al. (2026) 在多轮智能体环境中的特权信息蒸馏。(3) 直接监督微调（Supervised Fine-tuning, SFT）：直接在收集的轨迹上微调模型，但实验表明该方案仅恢复 3.8% 的 ICL 增益，效果很差。(4) 强化学习（RL）基线：如 PPO，在 SWE 任务上达到 17.7% pass@1，但需要大量环境交互（比本文方法多 9.6 倍）。(5) 样本高效智能体学习：其他分支如使用世界模型、分支 rollout（Janner et al., 2019）等，但本文的 Experience Distillation 是在不额外交互的情况下进行蒸馏，与现有方法有本质区别。(6) 蒸馏在智能体中的应用：如模仿学习、行为克隆等，但本文强调 ICL 作为教师信号的蒸馏。

Q3: 论文如何解决这个问题？

为解决 Experience Distillation 问题，本文提出了一种基于教师-学生框架的蒸馏实现，不依赖额外环境交互。具体步骤如下：(1) 数据收集：使用一个基础策略（Base Policy）与环境交互，收集一组轨迹（经验）。每个轨迹包含一系列状态-动作对（或观察-行动）。(2) 教师目标生成：对于轨迹中的每个时间步，给定历史上下文（即从开始到当前步的部分轨迹），使用一个预训练或基础模型作为教师，以该上下文作为输入（通过 ICL）来预测当前步的决策（可能是动作或下一步计划）。教师模型能够利用上下文信息来做出比基础策略更好的决策。关键点在于：教师模型在预测当前步时，只依赖已有的轨迹历史，不需要重新与环境交互（resample in-history）。(3) 蒸馏训练：学生模型（与教师同架构或共享参数）在无上下文条件下进行训练，目标是最小化其预测与教师输出（即 ICL 上下文下的改进决策）之间的差异，通常使用 KL 散度或交叉熵损失。通过这种方式，学生模型尝试将 ICL 的增益内化到自身的参数中。(4) 蒸馏后的模型可以直接用于环境交互，无需提供上下文或额外蒸馏。本文强调，所有过程仅在已收集的经验上进行，不进行新的 rollout 或环境交互。与上下文蒸馏不同，这里教师使用的上下文是智能体的交互历史，而不是提示词或系统提示。

Q4: 论文做了哪些实验？

论文在两种不同的智能体任务上进行了实验：(1) Curated SWE（Software Engineering）：包含 749 个软件工程任务（可能是代码修复、开发等），每个任务需要智能体在模拟环境中执行一系列操作（如编辑文件、运行命令等）。模型需要与终端交互。(2) Tale-Suite：包含 6 个文字冒险游戏（文本交互环境），智能体通过自然语言指令与环境交互。实验设置：所有实验使用特定的基础模型（in-house base models），教师模型与基础模型相同或共享参数。主要比较方法：(a) Base（无上下文）；(b) ICL（提供历史上下文）；(c) Experience Distillation（蒸馏后的无上下文模型）；(d) SFT（直接监督微调）；(e) PPO（经典强化学习）。评估指标：pass@1（首次尝试的成功率）。环境样本效率：统计完成任务所需的轨迹步数（或 episode 数）。论文还评估了 ICL 增益的保留率。消融实验可能涉及：蒸馏数据量、教师信号种类等。样本效率比较：ICL+Experience Distillation 与 PPO 比较，在 SWE 上达到 51.4% vs 17.7% 的同时，样本效率提升 9.6 倍。

Q5: 发现了什么实验现象？

主要实验发现：(1) ICL 大幅提升性能：在两种任务上，提供完整轨迹上下文的 ICL 相比基准（无上下文）有显著的性能提升，证实智能体可以从自身经验中进行样本高效学习。(2) 直接监督微调（SFT）几乎无法内化 ICL 增益：SFT 在经验上微调后，性能提升仅为 3.8%（相对于 ICL 增益），说明简单的行为克隆无法捕捉上下文中的动态决策模式。(3) Experience Distillation 有效内化 ICL 增益：蒸馏后的模型在无上下文条件下保留了至少 64.8% 的 ICL 性能提升，远超 SFT。(4) 样本效率优势：与 PPO 相比，ICL+Experience Distillation 在 SWE 上达到 51.4% pass@1，而 PPO 仅 17.7%，同时使用的环境交互样本数仅为 PPO 的 1/9.6（9.6× fewer）。在 Tale-Suite 上类似趋势。(5) 可能存在的失败模式或边界：如果教师使用的上下文过长或过短，蒸馏效果可能下降；蒸馏假设教师能提供高质量目标，如果教师模型本身能力有限，增益可能受限；任务复杂度过高时，蒸馏可能无法完全内化所有模式。(6) 消融趋势（推测）：随着蒸馏数据量增加，性能逐渐提升但可能饱和；不同的教师上下文长度或数据来源可能影响蒸馏效果。

Q6: 有什么可以进一步探索的点？

基于本文工作，未来可以探索的方向包括：(1) 扩展到更复杂的智能体任务：如机器人操控、多智能体协作、长周期任务（如软件工程中超长交互序列）。(2) 在线蒸馏与持续学习：结合环境交互，在蒸馏过程中逐步更新教师或学生，实现持续改进。(3) 多种教师信号的融合：除了 ICL 教师，还可以引入世界模型、价值函数或人类反馈作为教师目标。(4) 跨任务泛化：研究蒸馏得到的参数是否能在未见任务上保留部分 ICL 能力。(5) 理论分析：分析 Experience Distillation 成功的原因，例如蒸馏过程中知识压缩的损失界。(6) 蒸馏与其他策略优化方法结合：如将蒸馏作为 RL 的预训练阶段，或者与 PPO 等结合进一步提升样本效率。(7) 处理上下文窗口限制：当轨迹过长超过上下文长度时，如何分块蒸馏。(8) 在更广的模型规模上研究尺度规律：蒸馏效果是否随模型增大而提升？(9) 探索蒸馏数据的选择策略：并非所有轨迹或时间点对蒸馏同等重要，采样策略可以优化。

Q7: 总结一下论文的主要内容

本文提出并研究了 Experience Distillation 问题——如何将智能体通过上下文学习（ICL）从其自身交互历史中获得的能力蒸馏到模型权重中，从而在无需上下文的情况下保留这些能力，同时保持高样本效率（即不额外消耗环境交互样本）。作者设计了一种无需额外环境交互的实现：利用收集的轨迹作为数据，使用一个教师模型（在上下文中观察轨迹历史）生成改进的决策目标，然后训练一个学生模型在无上下文条件下模仿这些目标。在 749 个 curated 软件工程任务和 6 个文字冒险游戏上的实验表明，该方法成功保留了至少 64.8% 的 ICL 性能提升，而直接监督微调仅保留 3.8%。与经典强化学习 PPO 相比，ICL+Experience Distillation 在达到更高性能的同时，使用更少的环境样本（9.6 倍减少）。本文是首次系统研究 Experience Distillation，并为样本高效的智能体学习提供了新范式。论文还讨论了相关工作，包括上下文蒸馏、特权信息蒸馏、强化学习基线等。实验结果展示了 ICL 作为一种高效但上下文依赖的策略提升方式，可以通过蒸馏被有效内化，从而在真实世界应用中有巨大潜力。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文属于智能体（Agent）学习方向，与用户画像中的 agent 方向直接相关（权重 0.10）。

## 基本信息

- 作者：Chenhui Gou, Haoqin Tu, Yunhao Fang, Jianfei Cai, Hamid Rezatofighi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.21051`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（retrieved_evidence）和 field_evidence_map，优先使用了抽象、结论和实验部分的语义匹配片段；heuristic_draft 部分内容被保留或修正。
