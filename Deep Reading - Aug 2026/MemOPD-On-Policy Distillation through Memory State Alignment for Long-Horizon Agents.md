---
user_id: "cheng tan"
paper_id: 7189
arxiv_id: "2608.07068"
title: "MemOPD: On-Policy Distillation through Memory State Alignment for Long-Horizon Agents"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.07068.pdf"
pdf_url: "https://arxiv.org/pdf/2608.07068"
abs_url: "https://arxiv.org/abs/2608.07068"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-11T01:15:07"
---
# MemOPD: On-Policy Distillation through Memory State Alignment for Long-Horizon Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-horizon agent · compact memory · on-policy distillation · state alignment

## 一句话总结

MemOPD 通过记录并重建每次模型调用的原始输入状态，解决长程智能体压缩记忆时上下文改写导致的教师评分状态错配问题，实现状态对齐的在线策略蒸馏，并在实验中显著提升 F1、加速训练。

## 摘要

> Long-horizon agents accumulate growing contexts during interaction, impairing performance and stability. Compact memory mitigates this problem by compressing and rewriting the history retained between model invocations. Learning what to retain typically relies on proximal policy optimization (PPO) with final task rewards, but sparse rewards provide little guidance for individual memory updates. This limitation motivates on-policy distillation (OPD), which supplies dense teacher supervision on student rollouts. For such supervision to be valid, the teacher must evaluate each sampled action under the same state in which it was generated. However, the context rewriting performed during memory compression can break this alignment. When sampled responses are retained and re-encoded for later invocations, flattening the interaction into a persistent history may cause the teacher to score the action under a state that the student never visited during rollout. The action therefore remains on-policy by provenance, but not necessarily by state. We therefore propose Memory-Aligned On-Policy Distillation (MemOPD). MemOPD records the inputs and sampled outputs of each model invocation, restores its original token positions and causal visibility, and packs the reconstructed invocations for efficient teacher scoring. The teacher provides full-vocabulary supervision at the sampled action positions, while PPO preserves the final task objective. Experiments verify state alignment across several context updates and show that it improves F1 by 7.0% over persistent-history teacher scoring in a matched control. Overall, MemOPD-3B improves F1 over PPO by up to 416.2%, while packing yields up to a 1.63× speedup in actor computation during training. The code for this work is publicly available at: https://github.com/TPssp/MemOPD.

Q1: 这篇论文试图解决什么问题？

这篇论文面向长程智能体（long-horizon agents）在持续交互中面临的一个核心矛盾：为保证行为和稳定性，模型需要保留不断增长的历史上下文，但上下文越长，推理成本和性能衰减越严重。主流缓解手段是紧凑记忆（compact memory），即在两次模型调用之间压缩并重写历史，使下一次调用只看到一个更小的状态。问题是，如何学习“保留什么”这一策略？

1. 直接使用 PPO 等强化学习算法配合最终任务奖励：这在原则上可行，但长程任务的奖励往往稀疏、延迟，对每一次记忆更新（即每一步上下文压缩）几乎没有可用的梯度信号，导致记忆策略难以学习。
2. 另一种思路是教师蒸馏（teacher distillation），用更大的教师模型对学生 rollout 中的动作给出密集的逐 token 监督，相当于用教师的知识填充稀疏奖励的空隙。但这种监督有前提：教师必须对“学生实际经历的状态”进行打分，即动作与状态必须来自同一次采样。
3. 然而紧凑记忆的上下文重写机制破坏了这一对齐：学生在某次调用时基于当时的上下文采样了动作，但训练时这个上下文可能已被压缩、删除或重编码，学生看到的是展平后的持久历史（persistent history），而不是采样动作时的原始状态。于是，按来源（provenance）动作确实是学生生成的，但按状态（state）它从未在评分状态下被访问过。
4. 论文揭示了一个此前未被清晰表述的失败模式：如果把这样的 rollout 直接交给教师打分，教师实际是在“另一个状态”下评价动作，这会污染密度监督信号，甚至可能错误触发 PPO 的 clipping 机制。

因此，问题核心不是简单的“蒸馏好不好”，而是：在带记忆压缩的智能体训练中，如何保证蒸馏监督的状态有效性？论文将动作来源与状态有效性解耦，提出需要一种能够重建每次调用原始状态的训练机制。

Q2: 有哪些相关研究？

论文的 related work 横跨记忆机制、上下文压缩、强化学习与蒸馏四个方向：

1. 外部记忆与检索：从最早将外部知识库或记忆库与模型结合的思路，如 Lewis et al. (2020) 的检索增强生成、Karpukhin et al. (2020) 的 DPR，到 Borgeaud et al. (2022) 的 RETRO 在生成模型中引入大规模检索。这类工作通常把记忆当作静态知识源，而非可学习的压缩策略。
2. 上下文压缩与记忆管理：Park et al. (2023) 等尝试让智能体维护显式记忆库，Zhong et al. (2023)、Packer et al. (2024) 等研究如何从长上下文中提取或合成紧凑记忆。这些工作主要关注压缩方法的本身质量，很少讨论压缩后的记忆如何与强化学习训练流程的采样状态对齐。
3. 基于强化学习的记忆学习：Schulman et al. (2017) 的 PPO 成为主流，Jin et al. (2025)、Zhou et al. (2025) 等后续工作尝试在 agent 场景中应用 PPO 系列算法，但共同的痛点是任务奖励稀疏、延迟，难以给记忆更新提供细粒度信号。
4. 知识蒸馏与 on-policy 蒸馏：教师模型提供密集 token 级监督，已被广泛用于压缩模型能力。将蒸馏用于 RL 训练时，需要保证教师监督的分布与学生的采样分布一致，即 on-policy 性质；但现有蒸馏工作大多假设训练状态与采样状态一致，没有考虑上下文重写可能造成的状态漂移。

论文的定位恰好填补了上述交叉点：既要学习紧凑记忆，又要用蒸馏提供密集监督，同时还要处理压缩造成的状态不一致。相比之下，已有工作要么只研究记忆压缩本身，要么只研究 RL 蒸馏，但都没有显式处理“压缩重写使监督状态失效”的问题。

Q3: 论文如何解决这个问题？

MemOPD 的总体思路是：在训练时，先把学生 rollout 中每次模型调用的原始输入和采样输出完整记录下来，然后在评分阶段重建出与调用时完全一致的 token 序列和因果可见性，再把这些重建后的调用打包成若干批次交给教师模型打分。具体流程可分解为：

1. 记录每次调用：在 rollout 过程中，记录每个时间步模型的输入上下文、采样得到的动作 token、以及对应的因果掩码和 token 位置信息。这些信息是采样当时唯一真实的状态。
2. 重建调用状态：训练时，不直接使用压缩后的持久历史作为教师输入，而是根据记录的信息恢复原始调用。这包括恢复每个 token 的绝对位置，并重新构造因果掩码，保证教师评分时能看到的 token 与学生采样时完全一致。
3. 区分动作与上下文副本：由于压缩记忆可能把某个动作的 token 又作为后续调用的上下文出现，重建时需要确定哪些位置是“学生真正采样的动作位置”，哪些只是后来被重复使用的上下文副本。MemOPD 显式区分这两者，只对采样动作位置进行教师监督。
4. 打包高效评分：将多个重建调用按长度打包在一起，共用一次前向传播，减小训练时的计算开销，从而在保证状态对齐的同时获得 1.63× 的 actor 计算加速。
5. 与 PPO 结合：教师在有监督的动作位置提供全词表（full-vocabulary）蒸馏损失，作为稠密辅助信号；PPO 继续在整个 rollout 上按策略梯度方式优化最终任务奖励。这样既保留了任务目标的引导，又让每个记忆更新步骤都能获得来自教师的即时反馈。
6. 验证机制 RCE：为验证“重建确实恢复了正确状态”，论文提出 RCE 准则（从 heuristic 信息推断，RCE 可能指 reconstruction correctness/alignment criterion，具体全称未在摘要中给出），用于衡量重建计算与原始调用的等价程度。论文声称在不同上下文更新策略下，MemOPD 都能精确恢复原有的前向计算，这为状态对齐提供了可验证的判据。

这种设计的核心假设是：只要把每次调用时的原始上下文和因果可见性恢复出来，教师就可以精确地按学生采样时的状态打分，从根本上消除持久历史打分带来的状态漂移。

Q4: 论文做了哪些实验？

论文的实验设计围绕三个问题展开：状态错配是否会真实发生、MemOPD 的状态对齐是否有效、以及对齐后的蒸馏相比既有基线有多大提升。可归纳为以下几个部分（具体数据来源主要以摘要和检索片段为准，部分细节需视为合理推断）：

1. 状态错配的直接测量：论文先对学生 rollout 进行持久历史打分，计算其与重建调用打分的差异。结果显示，持久历史打分与原始调用的 p99 log 概率误差达到 1.774，在 651 个采样动作位置上改变了 top-1 预测，并有 13.29% 的动作错误触发 PPO clipping。这组数据直接证明：上下文改写会显著破坏状态一致性。
2. 匹配对照（matched control）实验：在控制其他变量不变的情况下，对比“持久历史教师打分”和“MemOPD 重建状态打分”的蒸馏效果。结果显示，MemOPD 相比持久历史打分在 F1 上提升 7.0%。同时，摘要中也提到持久历史教师在提升 F1 上比纯 PPO 高 5.6%，说明教师指导即使存在状态错配也有一定价值，但状态对齐后价值更大。这里 5.6% 与 7.0% 的具体换算在摘要中未完全说明，合理推断是：MemOPD 是在持久历史打分基础上额外获得 7.0% 的提升，而持久历史本身相对 PPO 提升 5.6%。
3. 整体性能：MemOPD-3B（可能是 3B 学生模型）相比 PPO 基线的 F1 提升最高达 416.2%，说明在稀疏奖励的长程任务中，纯 PPO 可能非常困难，而状态对齐的蒸馏提供了巨大增益。
4. 效率验证：打包重建调用后，训练时 actor 计算最高提速 1.63×，表明状态对齐并不以牺牲效率为代价。
5. 上下文更新策略的鲁棒性：论文声称 RCE 验证在不同上下文更新策略（可能包括不同压缩率、不同重写方式）下都能精确恢复计算，侧面验证状态对齐的通用性。

由于论文未披露具体任务环境（如是否为 Web 导航、游戏、多轮对话等），数据集和 baseline 细节无法从摘要确认。

Q5: 发现了什么实验现象？

论文的实验现象揭示了一个长期被隐含跳过的问题：压缩记忆训练中的“状态漂移”是真实且量级可观的。关键观察包括：

1. 状态漂移的度量：持久历史解码与原始调用解码的 p99 log 概率误差为 1.774，这已经足以改变 651 个动作采样位置的 top-1 预测，并导致 13.29% 的动作误触发 PPO 的 clipping 机制。这意味着哪怕学生生成的 token 不变，一旦上下文被重写，教师或 PPO 对它的评价就可能完全不同，训练信号失真严重。
2. 蒸馏的有益性与对齐的增益具有可叠加关系：持久历史教师相比纯 PPO 提升 F1 5.6%，说明教师信号本身有价值；MemOPD 在匹配对照中又比持久历史教师提升 7.0%，说明状态对齐能进一步释放蒸馏潜力。这组数字揭示了“有监督 vs 有正确状态监督”的差距。
3. 巨大相对提升背后的稀疏奖励挑战：MemOPD-3B 相比 PPO 的 F1 提升最高达 416.2%，这个数字本身需要谨慎解读——极有可能是 PPO 在冷启动或长程任务中几乎无法学到有效策略（例如 F1 接近 0），因此蒸馏带来的绝对收益看起来被放大。摘要没有给出绝对 F1 值，读者应避免把 416.2% 直接解读为“模型能力提升 4 倍”。
4. 效率与准确性的双赢：打包重建将 actor 训练计算加速 1.63×，同时没有损害状态对齐的准确性，这说明方法的额外记录与重建开销可以通过打包得到补偿。
5. 指标间张力：论文同时报告准确性和加速，但 1.63× 是 actor 计算的加速，不包括教师评分或打包开销的整体端到端加速；实际训练总耗时改进可能更小，这一点也需要读者注意。
6. 反直觉发现：即使状态错配持续存在，简单的持久历史教师打分仍能带来 5.6% 的 F1 提升，这说明“错误状态上的监督”不全是噪声，可能仍带有一定的任务相关信号——但状态对齐后收益更高。

Q6: 有什么可以进一步探索的点？

基于论文的发现，后续可以探索的方向包括：

1. 更复杂的记忆更新策略：论文验证了 RCE 在多种上下文更新策略下可行，但未覆盖所有可能的压缩/重写算子（如抽象摘要式重写、外部向量检索叠加）。测试 RCE 在更激进重写下的失效边界，有助于确定 MemOPD 的适用范围。
2. 与在线学习结合的扩展：MemOPD 目前将重建状态用于教师蒸馏，但同样的问题也可能影响 PPO 的 advantage 估计。将状态对齐推广到 value 网络和 advantage 计算，可能进一步提升 RL 训练的稳定性。
3. 降低重建成本：记录每次调用所有输入和因果掩码会显著增加存储开销。研究更紧凑的记录格式、或利用可逆计算近似重建状态，是实用化的重要方向。
4. 扩展到更大模型和多模态：MemOPD-3B 已经验证了有效性，是否可以扩展到 7B/70B 规模的 student/teacher，以及跨模态（如图文交互）的长程 agent，还需要进一步实验。
5. 与在线策略蒸馏结合：论文的 OPD 是纯在线蒸馏，可以尝试将状态对齐的思想引入离线 ORPO、DPO 等偏好优化方法，也可能缓解 RLHF 中类似的状态错配问题。
6. 理论分析：从理论上刻画状态错配上界与蒸馏误差的关系，推导何时重建是必要的、何时可以近似，这将为框架提供更坚实的保障。
7. 更多下游任务验证：当前只报告 F1 指标，扩展到成功率、人类评测、多任务泛化等维度，可以更全面评估 MemOPD 的收益与局限。

Q7: 总结一下论文的主要内容

这篇论文提出并解决了长程智能体训练中一个根本性的状态对齐问题。背景是：长程 agent 需要依赖紧凑记忆来避免上下文无限增长，而紧凑记忆通常由 RL 学习如何压缩。但 RL 的最终任务奖励稀疏，导致记忆策略难以学习；蒸馏可以提供密集监督，但前提是教师必须按学生采样时的状态来打分。论文通过分析发现，记忆压缩过程中的上下文重写会破坏这一前提：学生采样动作时的原始上下文与训练时供教师评分的持久历史不一致，导致动作虽然是学生 on-policy 生成的，却在一个学生从未访问过的状态下被评分。

论文首先对这个状态错配问题做了定量刻画：持久历史打分与原始调用打分之间的 p99 log 概率误差达到 1.774，改变了 651 个动作位置的 top-1 预测，并造成 13.29% 的动作误触发 PPO clipping。这些数据说明状态错配不是理论上的吹毛求疵，而是在实际训练中会产生显著负面影响的真实缺陷。

为解决该问题，论文提出 MemOPD（Memory-Aligned On-Policy Distillation）。其核心思想是记录并重建每次调用的原始输入与因果可见性，使教师能精确地对学生采样动作时的状态进行打分。具体流程包括：在 rollout 时保存每个调用的输入、采样输出和位置信息；训练时重建原始 token 位置和因果掩码；区分真正的采样动作位置与后来成为上下文的副本位置；将重建后的调用打包以高效利用算力；在采样动作位置施加教师的全词表蒸馏损失，同时保留 PPO 的最终任务奖励。论文还提出了 RCE 验证准则，用于判断重建计算与原始调用是否完全一致，并声称该准则在不同上下文更新策略下均能精确恢复计算。

实验方面，论文通过匹配对照验证了状态对齐的价值：MemOPD 相比持久历史教师打分在 F1 上提升 7.0%；持久历史教师相对 PPO 提升 5.6%，意味着状态对齐是在已有蒸馏收益之上的额外提升。整体上，MemOPD-3B 相比 PPO 的 F1 提升最高达 416.2%，同时由于打包机制，训练时 actor 计算获得最高 1.63× 加速。这些结果支持论文的核心论断：状态对齐不仅可行，而且能显著提升蒸馏训练的效果。

论文的贡献点可以归纳为四点：一是识别并形式化了上下文重写带来的状态错配问题；二是提出了 MemOPD 这一集记录、重建、区分、打包于一体的训练框架；三是引入了 RCE 作为验证状态对齐的准则；四是通过实验证明了方法的有效性和效率。讨论部分进一步区分了动作来源（provenance）与状态有效性（validity），指出仅保证动作是学生生成不足以构成 on-policy 训练。

工作的局限性包括：实验场景和基准未在摘要中公开，无法判断方法的推广范围；416.2% 的相对提升可能受 PPO 低基线影响；1.63× 加速仅指 actor 计算，不包含教师和整体训练开销；重建机制可能引入额外的存储与计算成本。未来研究可围绕更复杂的记忆更新策略、更高效的记录方式、更大规模的模型和更多任务来展开。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文直接面向长程智能体（agent）的训练问题，与用户画像中的 agent 方向高度相关。

## 基本信息

- 作者：Zhiyuan Liu, Tinghong Ye, Chenghao Liu, Yizhuo Li, Songfang Huang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.07068`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要及 PDF 全文语义检索命中的证据片段（背景、方法、结果、讨论），并结合启发式草稿进行润色与补全；部分细节（如 RCE 具体定义、实验任务细节）因可见证据不足已做出明确标注或合理推断。
