---
user_id: "cheng tan"
paper_id: 18885
arxiv_id: "2610.36925"
title: "Vision-Language Agents Can Learn in Their Own Latent Worlds"
publish_date: "2026-10-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.36925.pdf"
pdf_url: "https://arxiv.org/pdf/2610.36925"
abs_url: "https://arxiv.org/abs/2610.36925"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-28T11:41:56"
---
# Vision-Language Agents Can Learn in Their Own Latent Worlds

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：vision-language agents · model-based reinforcement learning · world models · latent imagination

## 一句话总结

DWELL 把预训练 VLM 重构为以自身潜表示为空间的循环潜在转移算子，让同一个模型兼任世界模型与策略，并通过真实交互与潜空间想象的交替学习，在有限环境经验下提升视觉决策性能、显著降低想象计算量。

## 摘要

> 2026
> Learning from experience is essential for building the next generation of agents that
> continually expand their capabilities. Pretrained vision-language models (VLMs)Oct already internalize rich world knowledge, yet this knowledge is rarely leveraged
> 27 toefficientlydrive policyremainsimprovementchallenging,throughas policyonlineupdatesinteraction.shift the agent’sRealizinginteractionthis potentialdistri-
> bution, requiring the world model to adapt accordingly, while continually updating
> large pretrained models is computationally expensive. To address this challenge,
> we introduce DWELL, a model-based reinforcement learning (RL) framework that
> reframes a VLM as a recurrent latent transition operator, endowing it with the
> ability to predict, adjust, and act in latent space. Leveraging the VLM’s own[cs.AI] representations and its dual role as both world model and policy, the agent con-
> tinually learns from real interactions to anticipate and evaluate action outcomes,
> and from latent imagination to improve its behavior. Across five interactive visual
> environments, DWELL achieves the strongest overall performance among train-
> able methods while substantially reducing the compute required for imagination,
> showing that effective world models for VLM agents need not reconstruct the
> future—they need to predict futures that are useful for action. Our code is publicly
> available at https://github.com/Tom111-lhx/Dwell.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：论文把矛盾定位为“经验学习的重要性”与“真实交互昂贵性”之间的冲突。智能体要从自身经验持续扩展能力，但模型无关 RL 通常需要大量环境交互，经验收集成为扩展瓶颈；因此关键不只是要不要学，而是如何用有限真实经验换取更大的策略改进。
2. VLM 知识利用不足：预训练 VLM 已内化丰富的世界知识，但主要是静态感知器；面对动态环境、部分可观测性和交互分布偏移时表现脆弱。持续更新大模型计算昂贵，而策略更新又会改变智能体的交互分布，要求世界模型同步适应，形成“策略非平稳性—世界模型滞后”的耦合难题。
3. 世界模型路线的两难：生成式世界模型逐步重建未来观测，计算开销大，而且像素级保真不一定等于控制相关；紧凑潜空间世界模型效率更高，但若预测出的潜状态与 VLM 策略的表示接口不匹配，预测再准也难以直接用于动作。论文试图同时回避“重建昂贵”和“潜空间不可用”这两个问题。
4. 论文的解决切口：不另建一个外部模拟器，而是把 VLM 自身潜表示当作世界模型空间；让世界模型、价值估计和策略共享表示接口；以“对动作有用”而非“观测保真”作为世界模型的核心目标。这一思路把世界模型从“模拟环境”改写为“策略可读的未来预测器”。
5. 隐含假设：其一，VLM 的潜空间包含足够支撑决策的状态信息；其二，有限真实交互足以学得可靠的潜动力学；其三，潜空间想象带来的模型偏差可以被策略学习容忍；其四，环境奖励或可学奖励模型能够提供有效监督；其五，共享表示不会导致表示坍缩、任务干扰或 shortcut learning。这些假设在可见片段中未被逐条验证。
6. 可检验点：论文的主张需要在逐环境性能、长时程任务、稀疏奖励、交互分布偏移和计算预算约束下检验。若 DWELL 只在平均成功率上占优，而部分环境失败或方差较大，则“潜空间世界模型普遍优于生成式世界模型”的结论需要收窄为“在特定任务族和特定接口设计下更优”。

Q2: 有哪些相关研究？

由于提供的文本没有完整 Related Work 章节，以下依据 Introduction、Method 和 Experiments 中出现的引用与基线重建，完整性需回原文核对。
1. VLM 智能体与智能体强化学习：论文把当前 agent 能力归因于大规模预训练（Brown et al., 2020; Hoffmann et al., 2022）以及人类示范/偏好反馈后训练（Ziegler et al., 2019; Ouyang et al., 2022），并引用近期工作（Wang et al., 2026; Li et al., 2026）说明用 RL 改进多步决策的趋势。DWELL 与这些工作的区别在于强调从自身交互经验中持续学习，而不是只做静态任务微调。
2. 世界模型与基于模型的 RL：论文引用 Hafner et al. (2019; 2023) 的潜空间想象路线，以及 Janner et al. (2019)、Chandra et al. (2025) 关于环境交互瓶颈的讨论。其核心对话对象是“在紧凑潜空间中进行想象、避免每步重建观测”的模型式 RL 传统。
3. 面向 VLM 智能体的生成式世界模型：Zhu et al. (2026) 与 Fang et al. (2025) 被用来代表“合成观测作为训练环境”的路线，覆盖具身与 Web 任务。DWELL 明确把自己与这类方法区分开：不生成未来观测，而是在 VLM 自身潜空间中预测未来。
4. 预测性表示学习：Method 片段提到 joint embedding predictive architecture 的概念脉络，并强调后验与先验两路更新所利用的信息不同，且两者都是确定性潜表示而非概率分布。这与“重建像素”的生成式世界模型形成方法论竞争。
5. POMDP 与信念状态：论文用 POMDP 形式化在线交互，引用 Åström (1965) 与 Kaelbling et al. (1998)。智能体基于历史形成信念，再以信念为条件选择动作，这为“递归潜在转移算子”提供了问题结构。
6. 持续学习与自生成经验：Silver & Sutton (2025) 被用来支撑“从自身经验持续学习”的长期方向；Villalobos et al. (2024) 被用来说明人类数据终将受限的“数据墙”问题。DWELL 可视为在这一议程下，用世界模型降低真实经验需求的一次尝试。
7. 评测基准谱系：SOKOBAN、FROZENLAKE、NAVIGATION、PRIMITIVESKILL 和 EB-ALFRED 分别来自离散视觉规划、第一人称导航、视觉操作和长时程组合任务等 benchmark 传统。论文用它们覆盖不同的部分可观测性、动作后果预测和长时程决策难度。

Q3: 论文如何解决这个问题？

总体思路：DWELL 不训练一个独立的像素级模拟器，而是把 VLM 改造成递归潜在转移算子，使其在自身潜空间中完成“预测—调整—行动”的闭环。
1. POMDP 形式化：在线交互被建模为部分可观测马尔可夫决策过程，包含状态、动作、观测、转移、奖励、观测函数和折扣因子。智能体维护信念 b_t，它是历史 h_t 的充分统计量，并据此选择动作。
2. 信念递归：给定观测与动作流，模型在 VLM 潜空间中递归更新信念；论文把这一步描述为推断“most explainable states”，从上下文看应指对决策最有用、最可解释的潜状态，精确含义需核对原文公式。
3. 潜动力学学习：方法片段明确区分后验与先验两路更新，前者利用当前可用信息推断状态，后者在没有显式反馈时向前预测；两者都是确定性潜表示，而非概率分布。这使世界模型可以在不生成观测的情况下进行多步预测。
4. 双角色共享接口：同一个 VLM 既作为世界模型，又作为策略；论文强调使用共享表示接口同时服务于预测、价值估计和动作。这一设计是 DWELL 与“独立世界模型 + 独立策略”路线的关键区别。
5. 真实交互学习：从在线交互收集的观测—动作—奖励流被用于更新世界模型、奖励模型以及策略/价值网络；Introduction 明确提到真实交互会持续精炼 world and reward models。
6. 潜空间想象：在真实反馈缺失时，智能体在潜空间中进行 imagined rollouts，用预测的未来状态评估动作后果并改进策略；由于不解码未来观测，想象的计算成本显著降低。
7. 规划与执行：潜空间预测可用于 model-based planning；策略在信念上执行动作，形成“真实交互—潜空间想象—策略改进—再交互”的循环。
8. 计算设计：DWELL 的核心效率主张来自“不重建未来观测”。但可见片段没有给出完整的损失函数、梯度是否穿过想象 rollout、世界模型与策略的更新频率、是否使用 stop-gradient 或目标网络等关键实现细节，这些需要回原文核对。

Q4: 论文做了哪些实验？

实验围绕两个问题展开：潜空间想象是否能在受控交互预算下提升任务性能与学习效率；其计算开销是否实际可承受。
1. 环境：五个交互式视觉环境覆盖互补的决策机制。SOKOBAN 与 FROZENLAKE 测试离散视觉环境中的空间推理和多步规划；NAVIGATION 测试第一人称观测下的具身导航；PRIMITIVESKILL 测试视觉 grounding 的操作技能；EB-ALFRED 把评测扩展到长时程组合式具身任务。五者共同覆盖不同程度的部分可观测性、动作后果预测和长时程决策。
2. 统一骨干与公平性控制：所有可训练 agent 方法统一使用 Qwen2.5-VL-3B 作为 VLM backbone，并在相同环境反馈和交互预算下训练。这一设置旨在把差异归因于学习框架，而不是模型规模或数据量。
3. 指标与统计：主指标为任务成功率，默认报告三个独立训练种子的均值和标准差。
4. 基线：至少包括模型无关在线 RL 的 PPO；结果片段还出现 LE World Model，以及摘要/结论中与生成式 world model 的对比；Table 1 还评估了 off-the-shelf VLMs。完整基线列表与各自调参细节需看原文。
5. 效率实验：统计在线训练期间的总 GPU hours，包括 world-model updates、imagined rollouts 和 policy/value optimization；所有方法在相同环境训练计划和硬件配置下比较。Figure 3 用于展示这一计算对比。
6. 组件分析：结果片段提到不仅考察潜动力学学习，还考察“让预测状态直接被 VLM 策略使用”的作用，说明论文对共享表示接口做了消融或分析；但具体消融变体、每项增益和显著性检验未在可见片段中给出。
7. 学习效率：实验设置强调在相同交互预算下比较，暗示论文关注样本效率而不仅是最终性能；相关学习曲线和跨环境趋势需要回原文图表确认。

Q5: 发现了什么实验现象？

1. 整体性能：摘要与结果片段明确称 DWELL 在可训练方法中取得最强整体性能。需要注意“overall”是跨五个环境的总体结论，不等同于每个环境都第一；逐环境胜负、方差和失败案例必须看 Table 1。
2. 计算效率：DWELL 相比生成式世界模型显著降低了想象阶段的计算开销，这是论文最核心的效率现象。片段同时指出 LE World Model 因紧凑潜动力学成为成本最低的方法；DWELL 的成本应高于 LE World Model，但仍低于需要生成观测的世界模型。由于结果片段在“DWELL consistently requires substantially...”处截断，精确对比对象和差距需回 Figure 3 核对。
3. 表示接口比“学动力学”更关键：结果片段强调，增益不仅来自学习潜动力学，还来自让预测状态直接被 VLM 策略使用。这揭示了一个重要现象：世界模型的预测损失下降不一定转化为策略改进，潜状态的可用性、对齐方式和接口设计可能才是性能差异的主要来源。
4. 有限经验下的样本效率：所有可训练方法在相同环境反馈和交互预算下比较，DWELL 的整体优势说明潜空间想象可能把有限真实经验放大了；但片段没有展示学习曲线，无法判断优势来自早期探索、后期收敛还是方差降低。
5. 任务异质性与潜在张力：五个环境覆盖离散规划、导航、操作和长时程组合任务，平均最强可能掩盖某些任务族的退步。尤其是 EB-ALFRED 这类长时程任务，潜空间多步 rollout 的误差累积可能更严重；片段未报告逐任务失败模式。
6. 计算—性能权衡：LE World Model 更便宜，DWELL 更强但更贵，说明“最紧凑”不等于“最有效”。论文的立场不是追求最低 FLOPs，而是在可接受计算下换取更强的策略改进；这一权衡的 Pareto 前沿仍需完整数据支撑。
7. 尚未确认的现象：潜空间模型偏差利用、想象地平线延长后的性能下降、稀疏奖励下的失败、跨环境负迁移等，都是合理推断出的风险，但可见片段没有把它们作为实验现象报告。三个种子的标准差和显著性检验也未展示，不能据此判断结果稳健性。

Q6: 有什么可以进一步探索的点？

1. 不确定性感知的潜动力学：当前片段强调确定性潜表示，未来可以引入概率头、ensemble 或校准机制，让智能体区分“可预测的未来”和“高不确定性的未来”，避免在想象的错误区域过度优化。
2. 多步 rollout 的误差控制：研究想象地平线、课程式延长、潜空间一致性约束、时序差分预测和闭环校正，缓解长时程任务中的误差累积与模型偏差利用。
3. 混合式世界模型：在潜空间预测为主的同时，保留按需解码能力；当任务需要精细视觉验证或人类可解释性时再生成观测，形成“潜空间快思考 + 生成式慢验证”的混合架构。
4. 奖励与价值学习：论文依赖环境奖励或奖励模型，未来可研究稀疏/延迟奖励下的奖励建模、偏好反馈、内在动机和探索 bonus，减少对密集环境信号的依赖。
5. 持续与非平稳学习：策略更新会改变交互分布，世界模型需要同步适应；可探索 replay、正则化、世界模型多时间尺度更新、灾难性遗忘抑制和在线模型选择。
6. 规模化与跨骨干迁移：当前实验统一使用 Qwen2.5-VL-3B，尚未验证更大 VLM、不同视觉编码器、不同动作空间和真实机器人/真实 Web 环境中的 scaling 行为。
7. 评测体系：除了成功率，还应报告样本效率、计算 Pareto、鲁棒性、长尾任务、失败类型学、跨环境迁移和统计显著性；尤其需要逐环境结果，避免“平均最强”掩盖任务特异性退化。
8. 理论与机制解释：可研究潜状态是否构成决策充分统计量、世界模型偏差如何影响策略改进、共享表示何时导致干扰或坍缩，以及潜空间想象与真实交互的最优配比。
9. 应用迁移：DWELL 的“潜空间预测 + 共享策略接口”可迁移到机器人操作、GUI/Web 智能体、科学实验闭环和交互式科学发现等场景，但这些场景的观测噪声、奖励稀疏性和安全约束会带来新的失败模式。

Q7: 总结一下论文的主要内容

论文的主论证可以概括为一个反问：如果 VLM 已经内化了大量世界知识，为什么还要把它当作静态感知器，另建一个昂贵的生成式模拟器？DWELL 的回答是，把 VLM 本身改造成潜空间中的递归转移算子，让它在自己的表示空间里预测动作后果，并让同一套表示同时服务于世界模型、价值估计和策略。
一、问题主线。论文从“智能体需要从自身经验持续学习”出发，指出模型无关 RL 的环境交互成本是扩展瓶颈；同时，预训练 VLM 虽有丰富世界知识，却难以在动态、部分可观测环境中直接转化为持续改进的策略。生成式世界模型可以生成想象轨迹，但逐步重建观测计算昂贵，且像素级保真未必对控制有用。论文因此提出：世界模型不应追求重建未来，而应预测对动作有用的未来。
二、技术主线。DWELL 把在线交互建模为 POMDP，智能体基于历史维护信念并选择动作。模型在 VLM 潜空间中递归更新信念，区分利用当前信息的后验推断与无反馈时的先验预测；两者都是确定性潜表示，避免显式观测重建。同一个 VLM 以共享表示接口同时承担世界模型、价值估计和策略，形成“预测—评估—行动”的闭环。训练时，真实交互用于精炼世界模型、奖励模型和策略；当反馈缺失时，智能体在潜空间中进行想象 rollout，用预测的未来状态评估动作并改进策略。论文强调，DWELL 的关键不只是学到一个潜动力学模型，而是让预测出的潜状态能够被 VLM 策略直接使用。
三、实验主线。论文在 SOKOBAN、FROZENLAKE、NAVIGATION、PRIMITIVESKILL 和 EB-ALFRED 五个交互式视觉环境上评测，覆盖离散规划、第一人称导航、视觉操作和长时程组合任务。所有可训练方法统一使用 Qwen2.5-VL-3B，并在相同环境反馈和交互预算下比较；主指标为成功率，报告三个种子的均值和标准差。基线包括 PPO、LE World Model、生成式世界模型以及 off-the-shelf VLMs。效率方面统计 world-model updates、imagined rollouts 和 policy/value optimization 的 GPU hours。
四、结论与边界。论文报告 DWELL 在可训练方法中取得最强整体性能，并显著降低想象阶段的计算开销；LE World Model 因紧凑潜动力学成为成本最低的方法，而 DWELL 在性能与成本之间取得不同权衡。论文的结论是：有效的世界模型不必复现世界本身，而应预测智能体能够直接行动和学习的未来。需要注意，可见片段没有给出具体数值、逐环境胜负、完整消融和失败案例；“整体最强”不能外推为每个环境都最优，潜空间多步预测的误差累积和模型偏差利用仍是待检验的边界条件。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent/agentic RL 方向直接相关：论文讨论 VLM 智能体如何从自身交互经验持续学习，而不是仅依赖静态示范数据。

## 基本信息

- 作者：Haoxi Li, Sikai Bai, Jianfei Ma, Qinglin Hou, Xiaocheng Lu, Tao Han, Jie Zhang, Song Guo
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-10-28
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.36925`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Abstract、Introduction、Method、Results 与 Conclusion 片段，并对片段截断处的具体数值、完整消融和机构信息保留了证据缺口。
