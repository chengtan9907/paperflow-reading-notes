---
user_id: "cheng tan"
paper_id: 14361
arxiv_id: "2609.37119v1"
title: "Unlocking the Critic: Reward-Free Policy Optimization for LLM Post-Training"
institution: "University of Luxembourg; Seafill Open-Source Community; Université Paris-Saclay"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.37119v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37119v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/reading_reports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-08T23:45:15"
---
# Unlocking the Critic: Reward-Free Policy Optimization for LLM Post-Training

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reward-free policy optimization · critic as reward · LLM post-training · stability

## 一句话总结

该论文重新审视了 LLM 后训练中“去 critic”的潮流，证明 critic 的不稳定主要是优化产物而非网络本身的问题，并提出 Reward-Free Policy Optimization (RFPO)：用一个冻结的预训练 critic 同时充当 rollout 级奖励、GAE 基线和对未完成前缀的成功预测器，在不使用任何外部标签的情况下匹配监督 PPO 的性能，同时大幅降低算力和内存开销。

## 摘要

> Recent approaches to reinforcement learning (RL) post-training for large language
> models increasingly remove the critic to reduce training instability and memory
> overhead. Even where a critic is trained, it is discarded once training ends, al-2026 though it has learned to predict outcomes. We revisit this trend and show that a
> pretrained critic’s ability to predict future outcomes can make it a valuable asset
> for efficient long-horizon reasoning. First, we find that instability in critic-basedSep RL for long chain-of-thought reasoning is largely an optimization artifact: keeping
> policy updates small and low in variance restores stable convergence. Second, a
> 29 well-pretrained critic estimates the posterior probability of eventual success from
> later trajectory states and unfinished prefixes. Its predictions therefore provide
> outcome-derived, dense, per-prefix learning signals that, during policy optimiza-
> tion, require neither completed rollouts, step-level annotations, nor external re-
> ward labels. Building on this insight, we introduce Reward-Free Policy Optimiza-
> tion (RFPO), which repurposes a single calibrated, frozen critic as a rollout-level
> reward, a value baseline for generalized advantage estimation, and a success fore-[cs.LG] caster for unfinished prefixes. We further show that binarizing the debiased score
> stops the policy from exploiting the critic’s length bias. Binarized, RFPO matches
> supervised PPO without a single label in the training loop, while substantially cut-
> ting compute and memory overhead. This makes RFPO particularly well suited
> to long-horizon reasoning tasks, where outcomes arrive late and generation domi-
> nates cost: because rollouts can be rewarded before they finish, training no longer
> has to pay for waiting on every trajectory to complete. Our findings challenge the
> prevailing critic-free paradigm and establish critic-based, reward-free optimiza-
> tion as a scalable and computationally efficient path for LLM post-training.

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题：主流的 LLM 后训练 RL 范式（以 PPO 为代表）在长链思维推理任务中普遍移除 critic（value network），其理由是 critic 训练和持有成本高、估计不可靠，且即便训练了 critic 也会在训练后被丢弃。作者质疑这一范式，并指出 critic 的价值可能被低估：一个训练良好的 critic 不仅能作为方差缩减基线，还能预测未来结果（即估计最终成功的后验概率）。由此引出三个具体问题：(1) critic 在长链推理中表现不稳定是否真的是 critic 本身的问题，还是训练更新规则的优化伪影；(2) 一个冻结的、预训练良好的 critic 能否被重新用作奖励模型（reward model），为策略提供稠密、逐前缀的信用分配信号；(3) 在奖励函数固定且可被策略利用的情况下，如何保证稳定性并匹配监督 PPO 的性能。在此基础上，论文提出奖励函数可以从 critic 的预测中内生获得，而不需要依赖外部 verifier 或人工标注。

Q2: 有哪些相关研究？

论文的相关工作围绕两条线索展开：一是 LLM RL 后训练中 critic 的存废之争，二是 reward-free 或弱监督信号下的策略优化。具体包括：(1) 当前主流作业：用单 prompt 多采样构建 baseline 代替 value network（如 Shao et al., 2024; Ahmadian et al., 2024; Yu et al., 2025; Hu, 2025; Liu et al., 2025; Zheng et al., 2025），理由包括 critic 昂贵且不可靠（Kazemnejad et al., 2025）；(2) 完全无价值网络的 reasoning/agentic 训练系统（Guo et al., 2025; Kimi Team 2025a/b; Yang et al., 2025; MiniMax, 2025）；(3) 反向潮流：不是移除 critic 而是修复 critic，例如预训练 value head、用生成式 critic、按 critic 解释回报的比例进行加权等（Yuan et al., 2025; Yue et al., 2025; ByteDance Seed, 2025; Shan et al., 2026; Pan et al., 2026），但所有这些工作都保留 critic 的原有角色（降低策略梯度方差），奖励仍来自外部；论文则完全不同：把 critic 从“baseline”的角色转化为“奖励来源”本身；(4) PPO 的价值网络起源（Schulman et al., 2017）作为背景。论文引用这些工作构成两个对照阵营：remove critic 与 repair critic，而 RFPO 似乎属于第三种立场：repurpose critic。

Q3: 论文如何解决这个问题？

RFPO 的核心思路是把预训练 critic 的功能从“策略梯度的基线”扩展为“三重角色”：(1) 作为 rollout 级奖励：critic 对最终成功的后验概率估计本身就是一条基于结果的稠密学习信号；(2) 作为 GAE 的价值基线：继续充当方差缩减器；(3) 作为未完成前缀的成功预测器：对尚未结束的 rollout，critic 仍能基于前缀打分，从而在 rollout 完成之前就提供奖励。技术上，论文强调两件事：第一，稳定性来自更新规则而非 critic 本身——保持策略更新小且低方差（单次更新规则，single-update rule，见 §4）可恢复稳定收敛；第二，对去偏（debiased）分数进行二值化（binarizing）以消除 critic 对长度的偏好，防止策略通过拉长或缩短响应来利用奖励中的长度偏差。据此，冻结的校准 critic 既提供奖励，又作为 GAE 基线，还提前为未完成前缀打分，训练不再需要等待每条轨迹完整结束。

Q4: 论文做了哪些实验？

论文用四个问题组织实验：(1) binarization 是否以稳定换天花板——连续奖励版本在 AIME 2026 pass@1 上达到 29.2，超过监督 PPO 的 25.0，但所有连续奖励实验后期都会衰退，响应长度随去偏强度漂移；二值化后稳定性恢复；(2) 训练是否稳定——零标签实验在 310–378 步内没有一个退化，平滑响应长度保持在初始值的 ±13% 以内，策略熵从 0.32 逐渐下降到 0.21–0.22（监督 PPO 为 0.25），未见持出集准确率崩溃；(3) RFPO 是否达到监督 PPO——跨四个套件，零标签 checkpoint 平均 pass@1 为 41.1%，监督 PPO 为 41.8%，半标签臂为 41.3%，共享初始……（原文截断）；(4) 奖励能否在 rollout 结束前分配——超过四分之一的 batch 可以在没有完整答案的情况下获得奖励；在生成上限为 5,120、8,192、12,288 tokens 时，……（原文截断）。此外还有关于策略更新次数与稳定性的实验：在 8 次更新 × 256 rollouts 的条件下，15 次运行中有 12 次发散；而单次更新条件下 45 次运行中有 8 次失败。

Q5: 发现了什么实验现象？

实验现象中最重要的几个：(1) 连续 critic 分数作为奖励时天花板更高但不可持续——AIME 2026 pass@1 达到 29.2 vs 监督 PPO 25.0，是全文最高 AIME 分数，但每个连续奖励臂后期都会衰退，响应长度随去偏强度缩短或拉长，说明策略在利用 critic 残余的长度偏好；(2) 二值化可以抵消长度偏好，但代价是放弃更高的天花板；(3) 稳定性与更新规则强相关：8 次更新 × 256 rollouts 时 15 次运行 12 次发散，单次更新时 45 次运行 8 次失败，失败模式的梯度范数中位数在 0.27 到 0.89 之间，说明发散具有组间差异；(4) 健康臂上 AUC 斜率在 −0.006 到 +0.002 每 100 步之间，|t| 均小于 0.8，说明 310 步内没有显著退化，唯一显著下降是连续奖励臂（−0.010 每 100 步，t=−2.89）；(5) 策略熵缓慢下降（0.32→0.21–0.22），与监督 PPO 的 0.25 相当，未出现崩溃或上漂；(6) 未完成前缀的评分精度与完整 rollout 相当，因此训练无需等待每条轨迹结束；约四分之一 batch 在无完整答案时即可获得奖励。

Q6: 有什么可以进一步探索的点？

论文结论指向长程推理与 agentic 场景：随着推理轨迹变长、agentic episode 变长，等待最终结果的时间与算力成本会越来越高，价值网络的预测能力与提前评分能力会越来越重要。可进一步探索的方向包括：(1) 将 RFPO 扩展到 agentic 场景，其中 episode 很长且结果稀疏；(2) 设计更好的去偏方法，在不牺牲连续奖励天花板的前提下消除长度偏差；(3) 研究 critic 在不同策略分布下的校准迁移能力，即冻结 critic 在策略演化过程中的可靠性边界；(4) 将 critic 预测与可验证奖励（verifier）混合，探索半标签或混合奖励的配置空间；(5) 研究连续奖励臂后期衰退的机制，尤其是去偏强度与响应长度漂移之间的定量关系；(6) 探索 critic 本身在预训练阶段的训练方法，使价值头更 robust 地支持 rollout 级奖励。

Q7: 总结一下论文的主要内容

论文从根本上挑战了“critic-free”的 LLM 后训练范式。主流做法（如 GRPO 类方法）用单 prompt 多采样 baseline 代替价值网络，理由包括内存和训练成本以及 critic 不可靠；论文反而主张 critic 并不只是 baseline，而是一个包含大量结果预测信息的模型，其不稳定主要源于更新规则而非网络本身。作者提出 RFPO 协议：将单个冻结、校准的预训练 critic 同时作为 rollout 级奖励、GAE 基线和未完成前缀预测器。关键工程决策是对去偏分数做二值化，以阻止策略利用 critic 的长度偏差。实验覆盖四套评测，零标签 RFPO 平均 pass@1 41.1%，基本匹配监督 PPO 的 41.8%，连续奖励版本在 AIME 2026 达到 29.2 的最高分但后期不稳定。训练稳定性数据显示，单次更新规则下零标签运行在 310–378 步内无退化，响应长度保持稳定，熵逐步下降；相比之下，多更新设置出现发散率升高。一个重要实验现象是，critic 能对未完成前缀以接近完整 rollout 的精度预测最终成功，因此约四分之一以上的 batch 可以在答案完成前获得奖励，极大减少长轨迹训练中的等待成本。论文的定位是“critic-based, reward-free optimization”，试图建立一条不同于 GRPO 和纯监督 PPO 的可扩展路径，特别适合长程推理与 agentic 任务。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent（权重 0.10）直接相关：论文结论明确指向 agentic episodes 变长时价值网络预测能力的价值

## 基本信息

- 作者：Hongyang Li, Xiao Li, Caesar Wu, Said Mammar, Grégoire Danoy, Pascal Bouvry
- 机构：University of Luxembourg; Seafill Open-Source Community; Université Paris-Saclay
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37119v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次精读参考了 arXiv 2609.37119v1 的 PDF 语义检索证据（93 chunks，命中 introduction/results/conclusion/references），所有数值与结论均以检索到的证据片段为准，部分截断信息（如四套件评测与生成上限实验细节）已明确标注为需回原文确认。
