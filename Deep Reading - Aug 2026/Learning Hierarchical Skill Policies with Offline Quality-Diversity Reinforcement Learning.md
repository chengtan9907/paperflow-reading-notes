---
user_id: "cheng tan"
paper_id: 8947
arxiv_id: "2608.19684"
title: "Learning Hierarchical Skill Policies with Offline Quality-Diversity Reinforcement Learning"
institution: "东京大学（The University of Tokyo）；RIKEN 先进智能研究中心（RIKEN Center for Advanced Intelligence Project, AIP）"
publish_date: "2026-08-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.19684.pdf"
pdf_url: "https://arxiv.org/pdf/2608.19684"
abs_url: "https://arxiv.org/abs/2608.19684"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-21T23:51:35"
---
# Learning Hierarchical Skill Policies with Offline Quality-Diversity Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：offline-to-online reinforcement learning · hierarchical skill policies · quality-diversity · advantage weighting

## 一句话总结

QDOS 提出一种基于优势加权品质-多样性（Quality-Diversity）预训练的双层技能策略框架，从混合质量的离线数据中提取多样且高价值的底层技能，并通过离线数据双重复用（双数据集重用）显著加速稀疏奖励任务的在线强化学习。

## 摘要

> Recent studies investigate how to leverage pre-collected datasets to improve the policy performance and sample efficiency of RL. One promising approach to achieve this goal is to employ a two-stage strategy: In the first stage, diverse skills are extracted as a low-level policy from a given dataset, and a high-level policy is trained to solve a specific task in the second stage. Typically, extraction of the low-level policy is performed based on unsupervised learning such as trajectory VAE. However, a limitation of this approach is that the quality of the low-level policy highly depends on the quality of the dataset. To address this issue, we introduce QDOS (Quality-Diversity Offline Skill learning), a unified pipeline for robust offline-to-online learning. Our approach incorporates an Advantage-Weighted Quality-Diversity pretraining objective, which weights the skill extraction and diversity objectives by the estimated advantage of each trajectory segment. This approach allows the model to extract diverse and high-value skills. By providing robust and task-relevant skill representations, QDOS significantly improves the quality of the embedded skill space used by the low-level policy. We further integrate this with a dual dataset reuse strategy, where offline data is used both for skill pretraining and for populating the online replay buffer via pseudo-labeling. Experiments demonstrate that QDOS significantly outperforms strong baselines in structured manipulation tasks and unstructured locomotion tasks, confirming its ability to accelerate exploration and improve final returns in challenging sparse-reward domains.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：当离线数据质量良莠不齐时，如何可靠地从数据中提取有用的低层技能，并利用这些技能加速后续在线强化学习。具体拆解如下：
1. 现有两阶段层级技能学习依赖无监督轨迹 VAE 提取低层技能，但轨迹 VAE 对所有数据片段一视同仁地最小化重构误差；当离线数据中混有次优或无关行为时，重建目标会把噪声也编码进技能空间，导致低层策略包含大量无价值、任务不相关的行为。
2. 离线到在线（offline-to-online）学习中，离线策略初始化和在线微调之间的分布不匹配、以及稀疏奖励下的探索困难，使得仅靠“先预训练再微调”不够鲁棒；高层策略需要从一开始就能利用离线数据中的丰富信息。
3. 关键挑战是“如何识别和提取有用行为”：在包含最优与次优动作混合的数据集中，技能提取不应只追求重构所有数据，而应优先从高价值、高回报的行为片段中学习，同时保持行为多样性，以便高层策略在不同任务情境下有足够的选择空间。
4. 此外，论文还关注如何让离线数据在在线阶段继续发挥作用，而不是只在预训练阶段使用一次；这引出了“双数据集复用”问题，即离线数据既要用于技能预训练，也要通过伪标签重放参与在线学习。
综上，QDOS 把“技能提取的质量-多样性权衡”和“离线数据全流程复用”统一到一个框架中，目标是在混合质量数据、稀疏奖励、长视野任务条件下实现更鲁棒和更高效的离线到在线强化学习。

Q2: 有哪些相关研究？

与本文相关的研究方向主要有四条线索：
1. 离线到在线强化学习（Offline-to-Online RL）：常见做法是先用离线数据预训练一个策略，再在线微调（如 RLPD、SAC 等 off-policy 算法）。这类方法通常受限于预训练策略质量与在线微调阶段的分布偏移。论文提到的 RLPD [12] 和 SAC [23] 可作为高层策略的 off-policy 学习器，属于该方向的代表性算法。
2. 层级技能与时间抽象（Hierarchical Skills / Temporal Abstraction）：通过将任务分解为低层技能序列，降低高层策略的有效视野长度，从而简化长视野问题。本文的低层技能可看作时间抽象的一种实现，高层策略在技能空间做决策。
3. 无监督技能提取：传统方法用轨迹 VAE 等生成模型对轨迹段进行压缩和重建，从而得到潜在技能表示；这类方法不区分数据片段质量，容易在噪声数据上学到无关行为。论文明确将这一点作为现有方法的局限。
4. Quality-Diversity 与离线技能发现：本文方法自称受 DiveOff [19] 启发。DiveOff 在静态数据集上引入 Quality-Diversity 目标，目的是发现多个多样化的可行解；QDOS 则进一步引入优势加权，使技能发现过程偏向高价值行为，同时保留多样性。
5. 论文的主要基线是 SUPE [6]，一种使用轨迹技能且不进行优势加权的 SOTA offline-to-online 方法；从摘要看，QDOS 与 SUPE 的关键差异在于预训练目标中加入了优势加权和 Quality-Diversity 机制。
需要说明的是，检索到的相关工作中关于 DiveOff、SUPE、RLPD、SAC 的描述来自摘要和片段证据，更细的对比设置（如是否还有更多基线）需回原文确认。

Q3: 论文如何解决这个问题？

QDOS 的核心思路是把“技能预训练”从纯无监督重构改成“优势加权的质量-多样性优化”，并让离线数据在在线阶段继续复用。具体方法可拆为以下部分：
1. 底层技能提取：给定离线数据集，将轨迹切分为片段，用一个低层策略（skill policy）对轨迹片段进行编码和生成。与标准轨迹 VAE 不同，QDOS 不把每个片段等同看待；它结合 Quality-Diversity 目标，既要覆盖多样化的行为模式，又要保证行为质量。
2. Advantage-Weighted Quality-Diversity Pre-training：论文的核心创新是对技能提取目标和多样性目标分别施加“估计优势”加权。即对每个轨迹片段，先估计其相对于平均水平的好坏（advantage），然后高优势片段在技能提取和多样性目标中拥有更高权重。这样低层策略会更关注高回报、任务相关的行为，同时仍保留行为多样性，避免把次优或无关行为当作主要技能。
3. 高层策略学习：在低层技能空间上训练高层策略，利用时间抽象降低任务视野长度，使长视野、稀疏奖励问题更容易学习。高层策略可以使用 off-policy 算法（如 RLPD 或 SAC）进行学习。
4. 双数据集复用（Dual Dataset Reuse）：离线数据在两条路径中被重复使用——其一，用于技能预训练；其二，通过伪标签（pseudo-labeling）方式为离线数据生成高层动作或技能标签，填充在线阶段的回放缓冲区。这样在线学习一开始就能从丰富的离线数据中采样，执行 off-policy 更新，减少在线探索负担，并缓解冷启动问题。
5. 整体流程形成 unified pipeline：离线阶段完成低层技能提取和初步高层策略训练；在线阶段依靠高质量、优势加权的技能空间和充满离线-在线混合数据的回放缓冲区，快速探索并收敛到更优策略。
需要注意的是，由于 PDF 检索片段只覆盖了方法的核心思想，关于伪标签的具体生成方式、劣势估计器的实现细节、网络结构和训练超参数在证据中未完整展开，这些部分属于“合理推断”或需回原文核实。

Q4: 论文做了哪些实验？

论文在四个具有挑战性的稀疏奖励领域上评估 QDOS，这些领域要求长视野规划与精确控制：
1. antmaze：四足蚂蚁在迷宫中导航到达目标，使用大型迷宫布局；属于非结构化运动任务。
2. antsoccer：从检索到的引言片段看，属于包含蚂蚁智能体的任务，可能是多技能组合场景；具体设定需回原文确认。
3. kitchen：典型的结构化机械臂操作任务，要求完成厨房场景中的多步操作；属于结构化操作任务。
4. humanoidmaze：人形智能体在迷宫中运动/导航，属于高维非结构化运动任务。
检索到的实验设置片段明确列出了 antmaze，并在引言中提到了 antsoccer、kitchen、humanoidmaze；因此“四个领域为 antmaze/antsoccer/kitchen/humanoidmaze”是合理推断，最终以原文实验列表为准。
基线方面，论文至少与 SUPE（其 primary baseline，一种不使用优势加权的最先进 offline-to-online 轨迹技能方法）进行对比；其余强基线的名字和具体版本在检索片段中未展开。
评估指标主要关注在线学习阶段最终回报和探索加速效果；摘要称 QDOS 在结构化操作任务和非结构化运动任务上都显著优于强基线。但检索片段未包含定量表格、学习曲线、方差区间和消融结果，因此无法在这里转述具体数值。

Q5: 发现了什么实验现象？

根据摘要和正文检索片段，可以归纳出以下实验现象：
1. QDOS 在结构化操作任务（如 kitchen）与非结构化运动任务（如 antmaze、humanoidmaze 等）上都显著超过强基线，说明优势加权 QD 技能预训练不是只在某一类任务上有效，而是具有跨任务类别的泛化能力。
2. 在稀疏奖励领域，QDOS 能够加速在线探索并提升最终回报；这暗示优势加权技能空间为高层策略提供了更易用的行为先验，降低了在线探索初期需要大量随机尝试的压力。
3. 作者指出标准轨迹 VAE 在处理噪声/混合质量数据时会把无关行为也学进来；QDOS 通过优势加权改变了技能提取的“关注点”，这可以解释为何它在质量参差的离线数据上表现更好。
4. 从片段证据看，QDOS 与 DiveOff 的差异在于后者主要面向静态数据集中的多解发现，而 QDOS 面向 offline-to-online 场景；这个差异可能体现为在线阶段回报提升更明显，但检索片段未给出对应的对比实验图表。
5. 目前检索到的证据中没有负结果、失败案例或消融趋势的展开；关于 QD 多样性目标与优势加权之间的平衡如何随数据集噪声水平变化、伪标签重放何时失效等问题，现有摘要和片段都没有提供信息，需要回到论文实验章节确认。

Q6: 有什么可以进一步探索的点？

基于 QDOS 的方法思想和当前证据，可以进一步探索以下方向：
1. 在线阶段对低层技能进行持续微调：当前方法主要在离线阶段预训练低层策略，在线阶段更多是高层策略学习；是否可以允许低层技能随任务分布偏移而更新，形成闭环的层级自适应？
2. 优势估计器的改进：QDOS 依赖轨迹片段的优势估计来加权 QD 目标，优势估计的偏差和噪声会直接影响技能质量；可以用更鲁棒的离线 critic、ensemble Q 或保守估计来提升加权可靠性。
3. 多样性度量与任务相关性的权衡：Quality-Diversity 目标鼓励行为覆盖，但并非所有多样性都有用；如何让多样性目标自适应地聚焦到与下游任务相关的行为空间，是值得探索的问题。
4. 伪标签重放的细节：双数据集复用中的伪标签生成方式（例如用哪个策略给离线轨迹打标签、标签置信度如何校准）对在线学习影响很大，可以系统研究不同伪标签策略。
5. 扩展到真实机器人：当前实验看起来以仿真任务为主；把 QDOS 迁移到真实操作/移动场景时，需要考虑低层技能的可执行性、数据质量噪声和安全约束。
6. 理论分析：可以为优势加权 QD 目标提供收敛性、样本复杂度或偏差-方差分析，说明为什么加权后的技能空间有助于在线探索。
7. 消融与边界条件：研究数据集质量（噪声比例、覆盖度）、任务视野长度、奖励稀疏程度对 QDOS 优势的影响，找出其适用边界和失效模式。

Q7: 总结一下论文的主要内容

这篇论文关注的是离线到在线强化学习中的层级技能策略学习问题。传统两阶段方案先离线提取低层技能，再在线训练高层策略；其中低层技能通常由轨迹 VAE 等无监督模型从离线数据中学习。QDOS 针对该范式的核心缺陷展开：无监督技能提取对数据质量敏感，一旦离线数据中混有大量次优或无关行为，技能空间就会受到污染，后续高层策略也难以学到好行为。
为解决此问题，论文提出 QDOS 统一框架。其关键技术包括：一是 Advantage-Weighted Quality-Diversity 预训练目标，在技能提取和多样性目标中引入每个轨迹片段的估计优势作为权重，使模型优先从高价值片段学习，同时保持行为多样性；二是双数据集复用策略，让离线数据既参与技能预训练，又通过伪标签填充在线回放缓冲区，使在线阶段从一开始就能利用丰富的离线经验做 off-policy 学习。整体架构上，QDOS 形成“低层多样化技能 + 高层任务决策”的层级策略，利用时间抽象降低任务视野长度，缓解稀疏奖励下的长视野规划困难。
实验方面，论文在四个挑战性稀疏奖励领域评估 QDOS，包括 antmaze（四足蚂蚁迷宫导航）、antsoccer、kitchen（厨房操作）和 humanoidmaze（人形迷宫运动）。这些任务覆盖结构化操作和非结构化运动两类场景，具备长视野规划和精确控制需求。结果表明 QDOS 显著优于强基线，尤其是以 SUPE 为代表的基于轨迹技能但不做优势加权的方法，能够加速探索并提高最终回报。
总体来看，QDOS 的贡献是把 Quality-Diversity 思想从静态离线发现扩展到 offline-to-online 训练，并通过优势加权解决了“数据质量参差时该学什么技能”的关键问题。论文的实践意义在于：即使离线数据集不是精心筛选的高质量数据，也能自动提取多样且高价值的技能，从而提升后续在线学习的效率。
需要指出的是，由于本次检索到的 PDF 语义片段主要覆盖摘要、引言、结论和部分方法/实验设置，未包含完整实验数值表格、学习曲线、消融结果和超参数分析，因此上述总结中的定量结论主要转述论文自我报告，细节需结合原文验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对“智能体/agent”方向：本文是离线到在线层级技能学习的直接案例，展示了如何从混合质量离线数据中构造可复用的技能库，适合作为分层决策和技能发现类研究的参照。

## 基本信息

- 作者：Tanachai Anakewat, Takayuki Osa, Tatsuya Harada
- 机构：东京大学（The University of Tokyo）；RIKEN 先进智能研究中心（RIKEN Center for Advanced Intelligence Project, AIP）
- 来源：arxiv
- 主题/分类：cs.AI, cs.LG, cs.RO
- 日期：2026-08-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.19684`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（30 个片段，命中 Introduction、Conclusion、Related Work、Experimental Setup 等）并结合摘要与启发式草稿；具体实验数值、完整基线表和消融细节因检索证据不足未展开。
