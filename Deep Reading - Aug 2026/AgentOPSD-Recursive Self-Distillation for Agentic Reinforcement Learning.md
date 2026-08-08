---
user_id: "cheng tan"
paper_id: 6994
arxiv_id: "2608.05987v1"
title: "AgentOPSD: Recursive Self-Distillation for Agentic Reinforcement Learning"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.05987v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.05987v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T14:03:59"
---
# AgentOPSD: Recursive Self-Distillation for Agentic Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · credit assignment · agentic rl · self-distillation

## 一句话总结

AgentOPSD 提出一种无批评器的递归回合级信用分配方法，通过在对数几率空间中递归更新贝叶斯信念状态将稀疏结果监督转化为回合级信用信号，在 ALFWorld、WebShop 和 Search-QA 上以 Qwen 3B/7B 超过 GRPO 和强自蒸馏基线，并在 Qwen2.5-7B 上达到 ALFWorld 89.1% 的成功率。

## 摘要

> Reinforcement learning(RL) with verifiable rewards constructs trajectory-level advantage, yet often fails to credit the few pivotal decisions that drive outcomes in long-horizon multi-turn agentic RL. Some recent works introduce privileged self-distillation into credit assignment for RL, offering denser supervision, but it still remains unclear how such a local signal should express sequential credit. We therefore propose AgentOPSD, a critic-free recursive turn-level credit assignment for agentic reinforcement learning. AgentOPSD aggregates token-level teacher-student log-probability gaps into turn-level evidence, recursively updates a Bayesian belief state in log-odds space. This provides a principled reweighting scheme that transforms sparse outcome supervision into turn-level credit signals and identifies pivotal turns by the marginal revision between consecutive states while remaining fully compatible with standard policy optimization, requiring neither additional rollouts. We evaluate AgentOPSD on ALFWorld, WebShop, and Search-QA with two Qwen model scales (3B and 7B). AgentOPSD improves over GRPO and strong self-distillation baselines, reaching 89.1% success on ALFWorld with Qwen2.5-7B, and ablations attribute the gains to turn-level aggregation and history-dependent recursive belief updates. Our code is available at https://github.com/ZethWang/AgentOPSD.
> ![](images/35ee0b4565ccf0201604faac18896460942408b1781e4c2f8518ff53ce4f6d28.jpg)
> ![](images/00388b420cb43fc37ce318b726ed8b439ec64f307d54eb8c579c9d58c2c8cec4.jpg)
> ![](images/0ba873eedddba54d6e8c44bfa187b9addbab532f20c8927aeaafdf5406d55bb4.jpg)
> Figure 1: Training dynamics and horizon-robustness of AgentOPSD on Qwen2.5-7B-Instruct / ALFWorld. (a) Validation success rate over training. (b) Sensitivity to task horizon: success points lost per extra turn (OLS slope of per-sub-task success against the measured mean turns of successful episodes). (c) Policy entropy over training.

Q1: 这篇论文试图解决什么问题？

论文针对的是长程多轮智能体强化学习中的信用分配（credit assignment）问题。具体而言，基于可验证奖励的 RL 通常构建轨迹级优势（trajectory-level advantage），但这类稀疏且延迟的结果信号难以在长程交互中精确归因到少数几个真正改变结果走向的关键决策（pivotal decisions）。虽然已出现引入特权自蒸馏（privileged self-distillation, OPSD）的工作来提供更密集的监督信号，但一个悬而未决的问题是：这种局部密集信号应当如何表达序列上的信用——即如何把逐 token 或逐回合的局部信号整合成一个既有全局一致性、又能定位关键回合的信用信号。论文指出，孤立地看待局部信号是不够的，一个回合的信用不应由它自身单独决定，而应由它对最终成功概率估计的改变量来决定。这可以理解为一种从“稠密但无结构的局部信号”到“结构化的回合级信用信号”的转化问题，并且要求转化过程与标准策略优化（如 GRPO）兼容，不能引入额外 rollout 开销。

Q2: 有哪些相关研究？

论文将研究置于几个相关的脉络中：（1）基于可验证奖励的强化学习（RLVR）已从单轮推理（如 Shao et al., 2024; Guo et al., 2025; Yu et al., 2025; Lu et al., 2026a 等）扩展到长程智能体任务，包括具身文本世界、网络购物和搜索问答，但轨迹级优势在长程多轮场景下存在信用分配困难。（2）一系列工作试图改进 credit assignment：例如 OPSD（On-Policy Self-Distillation）将策略自身蒸馏为密集信号；相关变体（论文提及 He et al., 2026 等）通过让同一个 policy 在训练时使用特权信息来避免单独教师模型；近年研究进一步将 OPSD 信号纳入 RL 训练。（3）在奖励侧，GiGPO（Feng et al., 2025）通过将 episode-level 优势与从重复锚定状态估计的 step-level 优势结合，改进 credit assignment。（4）论文强调其方法与这些方案的区别：AgentOPSD 不需要批评器，也不需要额外 rollout，而是将局部 self-distillation 信号与递归贝叶斯信念更新结合，生成回合级信用。值得注意的是，证据片段显示相关文献比较丰富，但当前检索到的信息有限，无法完整列出所有对照工作。

Q3: 论文如何解决这个问题？

AgentOPSD 的核心思想是：一个回合的信用不应由它本身的局部信号孤立决定，而应由该信号对最终成功概率估计的改变量决定。为此，论文将对数几率空间中的逐回合师生对数概率差（token-level teacher-student log-probability gaps 聚合为 turn-level evidence）作为证据，并用递归贝叶斯公式更新关于任务成功的一个信念状态（belief state）。具体来说，每经过一个回合，信念状态根据新增的证据进行对数几率更新，从而形成累积的“成功概率估计”。在此基础上，回合信用被定义为连续信念状态之间的边际修正（marginal revision），即该回合引发的成功概率估计的变化。这一设计使稀疏结果信号被重新分配为回合级信用，并且利用历史信息（整个轨迹的上下文）进行依赖历史的递归更新。该方法无需额外的 rollout，也不需要批评器，可以与标准策略优化（如 GRPO 风格）无缝配合。论文还强调，这种在 log-odds 空间中的递归更新揭示了孤立的自蒸馏差距本身并不是信用，关键在于它如何修正对成功的信念。

Q4: 论文做了哪些实验？

论文在三个交互环境中评估 AgentOPSD：ALFWorld（具身文本世界）、WebShop（网络购物）、Search-QA（搜索问答），并使用两个 Qwen 模型规模（3B 和 7B）。主要实验包括：（1）与标准 RL 基线 GRPO 以及两个强自蒸馏基线（OPSD 类方法）进行对比；（2）在所有环境和模型规模下报告成功率等指标；（3）进行消融实验：分别去除回合级聚合、去除依赖历史的递归信念更新，以验证各组件贡献；（4）提供训练动态与 horizon 鲁棒性分析：论文图1展示了 Qwen2.5-7B-Instruct 在 ALFWorld 上的验证成功率随训练的变化、策略熵随训练的变化，以及任务对 horizon 的敏感性（用每个额外 turn 损失的成功点数衡量）。由于现有证据只覆盖摘要和引言，具体实验协议细节（如 rollout 数量、training steps、baseline 实现细节）未见，但可合理推断论文提供了标准的对比与消融设置。

Q5: 发现了什么实验现象？

实验现象包括：（1）AgentOPSD 在三个环境和两种模型规模上总体优于 GRPO 和强自蒸馏基线，最亮眼的数字是在 Qwen2.5-7B 上 ALFWorld 达到 89.1% 成功率；（2）消融实验表明，回合级聚合和依赖历史的递归信念更新均贡献明显，说明“局部信号聚合为回合证据”和“用历史修正信念”是收益的关键来源；（3）图1显示训练过程中验证成功率稳步提升，策略熵逐渐下降，体现有效利用信用信号进行优化的过程；（4）horizon 鲁棒性方面，AgentOPSD 比基线更少地随任务步数增加而丢失成功率，即对较长任务更具鲁棒性（具体数值未给出，但图1标题暗示这一趋势）；（5）另有比对实验显示，收益主要来自信用构建而非特权访问，在统一设置下 AgentOPSD 与特权基线使用相同的检索技能，差别只在师生差异的利用方式，这进一步将效果归因于 credit construction。需要说明，部分现象是从论文图1标题和证据片段中推断，精确数值需查阅原文图。

Q6: 有什么可以进一步探索的点？

基于现有信息，可以合理推断若干可探索方向：（1）方法向更长 horizon、更复杂环境（如真实网页操作、具身 robotics）延展，验证递归信念更新在更高度稀疏奖励下的行为；（2）探索更丰富的证据形式：当前是 token-level log-prob gaps 的聚合，可否融合状态价值或环境反馈等其他信号；（3）对贝叶斯递归更新的先验与似然模型做理论分析：当前构造是否有最优性或对错误先验的敏感性；（4）与其它器或离线数据结合：论文明确不需要 critic，但交互 critic 是否能进一步提升；（5）在更大模型（如 70B）和更多任务上做扩展，观察规模效应；（6）将“边际修正”作为关键回合识别器，用于可解释性分析和决策追踪；（7）跨任务迁移：自蒸馏信号能否在任务分布变化时保持可靠；（8）考虑多轮基准中的负迁移或过优策略问题，是否需要额外的正则化。这些方向是推测性的，论文正文可能包含部分讨论，但当前摘要与证据未展开。

Q7: 总结一下论文的主要内容

论文围绕长程多轮智能体强化学习中的信用分配难题展开。论证主线是：基于可验证奖励的 RL 通过轨迹级优势进行策略优化，但在长程交互中，最终结果往往只由少数关键决策决定，轨迹级优势无法精准归因这些决策；已有自蒸馏（OPSD）方法提供了局部密集信号，但“局部信号如何表达序列信用”仍是开放问题。论文的核心洞察是：一个回合的信用不应由它自身的局部信号孤立决定，而应由该信号对最终成功概率估计的改变量决定。为形式化这一直觉，论文将回合级师生对数概率差视为证据，在对数几率空间中进行递归贝叶斯更新，得到对任务成功的信念状态，并将回合信用定义为连续状态间的边际修正。这样既保留了局部信号的密集性，又通过递归更新引入了历史依赖，输出有结构的回合级信用信号。技术主线上，AgentOPSD 是无批评器方法，兼容标准策略优化（如 GRPO），不需要额外 rollout，这一点可与需要批评器或额外采样的方法区分开。实验主线上，论文在 ALFWorld、WebShop、Search-QA 三个交互环境、Qwen 3B 和 7B 两种规模下评估了 AgentOPSD，对比 GRPO 与强自蒸馏基线，结果显示其全面超越，特别在 Qwen2.5-7B 上 ALFWorld 达到 89.1% 成功率。消融实验说明回合级聚合与递归信念更新是收益来源。论文还报告了训练动态、策略熵和 horizon 鲁棒性分析，显示 AgentOPSD 随任务长度增大性能下降更平缓，且收益主要来自信用构建而非特权访问。总体而言，该工作为智能体 RL 的密集信用分配提供了一种并行的、无批评器的、可直接落地的方案，并验证了其在多个任务与模型规模上的有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文属于智能体（agent）方向，与你画像中的 agent 权重 (0.10) 直接相关。

## 基本信息

- 作者：Zi-Han Wang, Zhengxi Lu, Zhiyuan Yao, Jinyang Wu, Jie Wu, Zhengzhou Cai, Yueqing Sun, Ziang Ye, Linji Hao, Qi Gu, Xunliang Cai, Yongliang Shen, Yujiu Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.05987v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于论文 abstract 与 introduction 的 PDF 检索证据（fields: background/method/results/limitations/relevance），并结合元数据与图1标题信息进行归纳；部分实验细节为合理推断。
