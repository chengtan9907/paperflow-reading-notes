---
user_id: "cheng tan"
paper_id: 6479
arxiv_id: "2608.03223v1"
title: "Agentic Reinforcement Learning with Self-Distilled Reward Shaping"
publish_date: "2026-08-04"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.03223v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.03223v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-06T01:47:13"
---
# Agentic Reinforcement Learning with Self-Distilled Reward Shaping

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic reinforcement learning · reward shaping · temporal credit assignment · self-distillation

## 一句话总结

本文提出 ADRS（Agentic Reinforcement Learning with Self-Distilled Reward Shaping），一种利用训练期特权技能通过自蒸馏生成返回关联的 token 级奖励塑形信号、并整合进原生 RL 优势构造的框架，用于改善多轮语言智能体在稀疏轨迹奖励下的时序信用分配，在多个交互基准上显著提升长时任务表现。

## 摘要

> Agentic reinforcement learning enables LLM agents to learn through interaction, but sparse trajectory-level rewards reveal success without identifying which intermediate decisions deserve credit. Training-only privileged skills can provide denser supervision by allowing the same frozen policy snapshot to rescore fixed tokens from skill-free trajectories while conditioned on task-matched procedural skills. Existing methods, however, do not jointly calibrate teacher scores across interaction steps, relate teacher confidence to realized returns, and integrate the resulting signal into native reward-to-advantage construction. We introduce Agentic Reinforcement Learning with Self-Distilled Reward Shaping (ADRS), a framework for constructing return-associated token-level credit for multi-turn language agents. ADRS centers and normalizes privileged token scores within each step, modulates them with a return-associated Teacher Value Advantage (TVA) gate based on within-group confidence--return association, and incorporates the gated token signal into native RL credit construction. Together, these components determine what the teacher prefers, when that preference is return-relevant, and how it enters the native reinforcement-learning credit path, while keeping rollouts and inference skill-free. Finally, experiments across three interactive benchmarks show that ADRS consistently improves performance on long-horizon tasks, with gains persisting across RL backbones, reduced-data settings, unseen tasks, and extended training. For anonymous review, our code is available at the following the link: https://github.com/gitrxh/ADRS-arxiv

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：多轮语言智能体在强化学习训练中，轨迹级稀疏奖励只能提供整体成功与否的信息，无法对不同中间决策进行时序信用分配。具体来说：

1. 稀疏奖励导致信用分配困难：长时程交互中，只有最终结果（如任务成功或失败）作为奖励信号，中间步骤对结果的实际贡献没有显式的、可学习的监督，模型难以知道哪一步是关键。

2. 密集监督信号的成本与可靠性问题：人为标注过程奖励或训练独立的奖励模型成本高昂且可能引入偏差；已有过程奖励模型多数只按状态/动作质量打分，未必与真实回报对齐。

3. 特权技能作为教师信号的潜力与缺陷：训练阶段可以使用任务匹配的程序化技能（privileged procedural skills），让冻结的策略快照在技能条件下重新打分无技能轨迹中的 token，从而形成密集教师信号。但论文识别出现有做法的三个关键缺口：
 - 跨步骤不可比：token 的 log 概率受局部上下文、策略不确定性、响应组成（长度、格式、语义）影响，其偏移和尺度在不同交互步骤之间并不直接可比，直接使用会产生误导性的信用信号。
 - 教师自信不等于有用：一个教师可以对某些 token 很有信心，但这种信心未必与该步决策对最终回报的贡献相关，甚至可能强化与任务无关的格式偏好。需要显式判断教师偏好在什么时候与回报相关。
 - 信号未融入原生 RL 信用路径：已有方法多把教师分数当作独立奖励或约束，没有将 token 级信号接入 reward-to-advantage 的构造过程，导致学生策略优化时信用信息与策略梯度之间的接口不够顺畅。

4. 推理时必须无特权：特权技能只能在训练时出现；如果推理时依赖技能，模型部署成本高且可能不具泛化性，因此需要一种训练期塑形、推理期无成本的方案。

5. 需要一个统一框架，回答三个问题：教师偏好什么（token 级信号）、教师何时偏好与回报相关（TVA 门控）、以及这种偏好如何进入原生 RL 信用路径（预优势集成）。

Q2: 有哪些相关研究？

论文所涉相关研究方向可从检索片段与领域背景归纳如下：

1. 奖励塑形（Reward Shaping）：这是补足稀疏反馈的经典思路。检索片段显示论文引用了基于势的塑形（potential-based shaping），其关键性质是在固定马尔可夫势函数下不改变最优策略（[19]）；RUDDER（[2]）则通过将延迟回报重新分配给与结果相关的早期决策来处理信用分配。这些方法为 ADRS 提供了理论背景和对照基线。

2. 时序信用分配（Temporal Credit Assignment）：除 RUDDER 外，还包括 eligibility trace、return decomposition、事后重新标注等。论文的核心贡献正落在这一方向，尤其是多轮语言交互环境中的 token 级信用分配。

3. LLM 智能体的强化学习微调：近年来流行的方法包括 RLHF（基于人类反馈的强化学习）、PPO/GRPO 等在线策略优化、以及各类用于 agent 轨迹优化的方法。ADRS 强调其信号可以嵌入不同 RL 主干，说明它面向这一更广泛的 agentic RL 生态。n
4. 过程奖励模型（Process Reward Models, PRM）：与 ADRS 的 token 级密集奖励目标类似，但 PRM 通常需要人工标注中间步骤或训练单独模型；ADRS 则利用特权技能以自我蒸馏方式产生信号，无需额外标注器。

5. 特权信息与教师-学生蒸馏：训练阶段使用特权信息（如模拟器状态、完美规划器或程序技能），推理阶段不依赖，是模仿学习中的常见范式。ADRS 的特殊之处在于教师就是策略自身的冻结快照，属于“自身蒸馏”（self-distillation），从而避免训练一个独立教师模型带来的分布漂移问题。

6. 程序化技能与任务分解：任务匹配的程序化技能（procedural skills）可视为一种显式先验或规划提示，用于在训练时引导策略的 token 级决策。相关工作涉及提示工程、planning 与 hierarchical RL 的结合。

7. 相关基准：论文在三个交互式基准上实验，已知至少包含 ALFWorld（基于文本的家居导航/操作任务）；另外两个基准名称未在检索片段中出现，需查阅原文。ALFWorld 一度广泛用于研究语言智能体的规划、常识推理和信用分配。

Q3: 论文如何解决这个问题？

论文提出 ADRS 框架，整体流程可以概括为“生成特权 token 分数 → 步骤内校准 → 回报关联门控 → 预优势集成”。具体组件如下：

1. 特权分数生成（Privileged Score Generation）：训练时，策略与环境交互产生“无技能轨迹”（skill-free trajectories）。同时，冻结的策略快照（即训练过程中的某个历史策略，frozen policy snapshot）在给定任务匹配的程序化技能（procedural skill）的条件下，对这段轨迹中的固定 token 重新打分。由于条件中包含技能信息，而 rollout 中没有技能，二者的概率差异就反映了技能带来的“偏好”或“知识”，可作为 token 级奖励信号。利用同一策略的冻结版本做教师，避免了训练独立教师模型，属于自蒸馏（self-distillation）机制。

2. 步骤内校准（Within-Step Calibration）：不同交互步骤的 token log 概率绝对水平受局部语境、策略不确定性、响应长度和格式影响，因此直接跨步骤比较教师分数会引入偏差。ADRS 对每个交互步骤内部的 token 分数进行中心化（减去该步均值）和归一化（除以该步标准差或其等价尺度），使每个步骤的教师偏好信号具有可比性。这解决的是“教师偏好什么”在尺度上的可比性。

3. Teacher Value Advantage（TVA）门控：校准后的 token 分数并不会被无条件使用。论文引入基于“组内置信度-回报关联”的门控机制：按某种分组（例如相同任务类型、相同示范轨迹组、或相同决策点类）统计教师置信度与实际回报之间的关联，当关联为正且显著时，该组信号才被视为 return-relevant；否则门控关闭或衰减。TVA 门控的值来自这种置信度-回报关联，用来调制 token 信号强度，从而回答“教师何时该被信任”。这解决了“自信教师不一定有用”的问题，避免模型被与回报无关的偏好干扰。

4. 预优势集成（Pre-Advantage Integration）：经过校准和门控后的 token 级信号被接入原生 RL 的 reward-to-advantage 构造流程，也就是说它是在计算优势函数之前参与奖励或价值目标构建的，而非在 loss 上简单加权。这样 token 级信用就直接进入策略梯度更新路径，并与普通轨迹奖励协同工作。

5. 推理时无特权：整个训练过程仅将技能用作条件输入来生成教师分数；所有 rollout 与最终推理均保持 skill-free，因此部署时不需要程序技能，也没有额外推理开销。

6. 与 RL 主干的兼容性：框架声称它与不同 RL 主干兼容，实验结果也显示在多种 RL 主干上都有一致提升，说明该方法是作为信用分配层插入不同优化器的通用模块。

Q4: 论文做了哪些实验？

根据检索片段，论文的实验可归纳如下：

1. 基准与任务：在三个交互式基准上进行评估；检索片段中明确出现的基准是 ALFWorld，具体任务包括 heat、pick-and-place、cool、look-at-object-in-light 等操作类轨迹。另外两个基准的名称和任务类型在提供的片段中没有出现，需查原文。所有任务均为长时多轮语言交互任务，以稀疏成功/失败信号为最终奖励。

2. 总体性能比较：论文报告了相对最强基线的改进为 0.7 和 7.0 个点（分数类型/指标未知，可能与不同基准或不同设置对应）。这些提升表明 ADRS 在不同任务难度和稀疏度下都能带来增益，但提升幅度差异大（0.7 vs 7.0），暗示任务间异质性。

3. RL 主干变化：实验结果展示了“跨 RL 主干”的一致性提升，即替换底层 RL 优化算法后 ADRS 仍然有效，说明其信号构造方式与具体优化器解耦。

4. 模型家族与参数规模：论文提到效果跨“不同模型家族与参数规模”持续，可能用多个 LLM（如不同规模的开源模型）验证，但具体模型清单未知。

5. 数据缩减：在减少训练数据量的条件下，ADRS 仍然保持提升，说明其信用信号帮助缓解数据短缺问题。

6. 未见任务与扩展训练：在未见过的任务上亦有泛化收益；在延长训练时间后提升不会衰减。

7. 消融与机制诊断：为了考察 token 级优势调制如何支持长时性能，论文设计了动作-对象机制诊断，对四条固定的 ALFWorld 成功轨迹（heat、pick-and-place、cool、look-at-object-in-light）进行剖析，这些轨迹包含 32 个动作-对象 token。诊断关注 token 级优势在各动作类别上的分布及其与关键步骤的对应关系。具体观察结果（例如哪些 token 获得最大优势）未在检索片段中呈现，需查阅原文。

8. 代码资源：提供了匿名评审代码仓库（https://github.com/gitrxh/ADRS-arxiv），便于复现。

Q5: 发现了什么实验现象？

从检索证据中可以提炼出以下实验发现：

1. 一致性能增益：ADRS 在长时任务上相对最强基线获得改进，且增益跨 RL 主干、跨模型家族与参数规模、在数据缩减、未见任务和扩展训练设置下均保持。这表明框架的收益不是偶然调参的结果，而具有结构性稳定性。

2. 改进幅度的异质性：片段中出现 0.7 分和 7.0 分两种提升幅度，说明 ADRS 在不同场景下的效果差异很大；可能 0.7 分对应的任务本身已被较强基线解决或信号较为饱和，而 7.0 分的任务则信用分配困难更大。这一张力提示用户需关注不同任务的收益边界。

3. 教师自信不一定等于回报相关：论文动机明确指出“a confident teacher is not necessarily a useful …”，TVA 门控因此引入。实验中该门控的作用（如开/关对比）未在片段中展示，但合理推断存在消融实验验证门控必要性。

4. 跨步骤校准的必要性：由于 token log 概率受局部上下文和响应组成影响，跨步骤直接比较会导致噪声信用；步骤内校准使信号可比，这也应该体现为消融实验的负向变化（推测）。

5. Token 级优势调制的稀疏性：机制诊断中，四条轨迹只包含 32 个动作-对象 token，说明语言智能体轨迹的 token 序列较短。诊断结果可能显示优势集中在少数关键动作 token 上（如冷却、拾放），而其他 token 的调制较弱——这是推测，但符合信用分配的目标。

6. 性能提升不依赖推理特权：检索片段提到“performance gains do not rely on privileged…”（截断），合理推断其含义是提升不依赖推理时特权信息，与框架设计一致。

7. 未发现负结果：提供的片段中没有明确记录失败案例或负结果；这可能是因为论文未报告，也可能因为片段截取有限。用户在阅读时应注意这一点。

8. 指标间张力：0.7 与 7.0 的差距大，说明不同基准对 ADRS 的敏感度不同；若论文同时报告成功率与平均奖励等指标，可能存在指标间提升不均衡，需查阅原文确认。

Q6: 有什么可以进一步探索的点？

从论文的问题设定和框架设计出发，可以提出如下可进一步探索的方向：

1. 自动技能发现与动态技能库：目前依赖任务匹配的程序化技能作为教师条件；未来可以研究自动从示例或知识库中生成技能，或构建可更新的技能库，减少人工设计技能的成本。

2. 更细粒度的信用分配：当前是 token 级，未来可扩展到实体级、步骤级、工具调用级或计划级信用，并结合可解释性分析。

3. TVA 门控的泛化：可以探索其他置信度-回报关联度量，如互信息、因果效应估计、学习型门控网络，甚至在线自适应门控。

4. 自蒸馏快照的更新策略：冻结策略快照的选择频率、更新时机以及快照与当前策略的分布漂移如何影响塑形信号质量，值得系统研究。

5. 理论分析：能否在多轮语言 MDP 条件下证明类似势函数塑形的最优性保持性质？在什么条件下 TVA 门控不会改变最优策略？

6. 扩展到更广泛模态与真实世界智能体：ALFWorld 等文本环境相对封闭；可尝试用于带视觉输入的具身智能体、网页操作智能体或代码生成智能体。

7. 多任务与元学习：将 ADRS 应用于多任务 RL，利用共享技能进行跨任务信用分配，可能提高样本效率。

8. 与过程奖励模型结合：将 ADRS token 信号作为 PRM 的辅助特征，或反过来用 PRM 取代技能条件，探索混合监督方式。

9. 负向信号与失败模式利用：论文主要利用成功轨迹的教师偏好；未来可研究失败轨迹中哪些 token 应被抑制，以强化负面信用。

10. 多智能体协作：当多个语言智能体协作时，信用分配需要跨智能体对齐，ADRS 是否可扩展为团队级 Teacher Value Advantage 是开放问题。

11. 对超参数的敏感性：步骤内归一化方式、组内分组粒度、门控阈值等超参数的影响需要系统消融与调参指南。

12. 真实部署中的成本与收益权衡：虽然推理无特权，但训练期需要额外技能推理和重新打分，其训练开销的测量与优化也值得研究。

Q7: 总结一下论文的主要内容

本文聚焦智能体强化学习（Agentic RL）中的一个关键瓶颈：LLM 智能体在多轮交互中学习时，只能获得轨迹级的稀疏成功/失败奖励，缺乏对中间决策的信用分配。这种稀疏信号使模型难以判断哪些 token 级动作真正推动了任务成功，尤其当任务需要长序列推理和操作时，梯度信号极弱。

论文的主要观察是，训练阶段可以利用特权信息（privileged skills）来补偿这种稀疏性。具体而言，程序化技能（procedural skills）是与任务匹配的、对成功步骤有先验描述的信息，例如“先加热再放入物品”“先拿起物体再移到目标位置”等。训练时，可以将这些技能作为条件输入，使用同一个策略的冻结快照重新评分无技能轨迹中的每个 token。因为教师是策略自身的旧版本，所以这种机制被称为“自我蒸馏”（self-distillation）。

然而，作者指出三个朴素使用这种教师信号的问题。第一，不同交互步骤的 token log 概率在量纲和偏移上不可比：token 的概率受到局部上下文、策略不确定性、响应长度与格式组成的影响；因此，一个步骤中的高置信度不能直接与另一个步骤中的高置信度相比。第二，教师的高置信度未必与其决策对最终回报的贡献有关：模型可能对低风险、格式化的 token 信心十足，但真正改变任务走向的却可能是一些低调但关键的 token；若不加区分，所有 token 信号都会被平均地注入 RL 优化，导致模型过度优化与回报无关的偏好。第三，即使生成了 token 级信号，也需要设计它与原生 RL 信用构造的接口，使其能有效地进入 reward-to-advantage 转换，而不是孤立地作为一种外部奖励。

为应对上述三个问题，论文提出 ADRS，其核心包含三个组件：

- 步骤内校准：在每个交互步骤内部对特权 token 分数进行中心化和归一化，去除跨步骤的尺度与偏移差异，使信号可比；
- TVA 门控（Teacher Value Advantage gate）：基于组内置信度-回报关联，把教师置信度与真实回报联系起来。如果一组样本中教师置信度与回报正相关，说明该组信号值得采信；否则门控会降低甚至关闭该信号。TVA 门控回答“何时教师偏好与回报相关”；
- 预优势集成：将门控后的 token 信号在求优势函数之前并入原生 RL 信用路径，从而直接参与策略梯度优化。

这三个组件共同完成了“教师偏好什么—何时有效—如何进入优化”的完整链路。重要的是，整个训练过程中，rollout 和推理都保持 skill-free；特权技能仅在离线重新打分时作为条件，所以推理时不会引入额外依赖或计算开销。

实验部分覆盖三类交互基准（其中可确认的是 ALFWorld，还包括两个未在片段中命名的基准），并系统评估了 ADRS 在多个 RL 主干、不同模型家族与参数规模、数据缩减、未见任务以及扩展训练时间下的表现。论文报告了相对最强基线 0.7 和 7.0 分的提升，说明 ADRS 在不同任务上均有收益，但幅度差异提示任务间的异质性。此外，论文通过动作-对象机制诊断（选取 heat、pick-and-place、cool、look-at-object-in-light 四条成功轨迹，共 32 个动作 token）展示了 token 级优势调制如何促进长时任务的信用分配。整体结果说明，ADRS 是一种相对通用的信用分配增强插件，可以嵌入不同的 RL 优化器。

论文的主要贡献可以概括为：提出 ADRS 统一框架，填补了特权教师分数在跨步骤校准、置信度-回报关联、以及与原生 RL 优势构造衔接上的三重空白；通过自蒸馏避免了独立教师模型带来的成本与分布不匹配；实验证明其跨主干、跨模型、跨数据规模、跨任务和跨训练时长的稳健提升；并提供可复现代码。

当然，受限于检索证据不完整，论文中未展示的方法细节（如归一化具体形式、TVA 门控的分组方式、优势集成的位置、超参数设置）以及另外两个基准的名称，都需要读者在原文中进一步确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你画像中的“agent”方向直接相关：ADRS 属于智能体强化学习中的信用分配与奖励设计，可以指导你构建更高效的 agent 训练方案。

## 基本信息

- 作者：Ranxu Zhang, Guinan Chen, Chenshaodong, Jinghao Lin, Xiaozhou Xu, Sunzhe, Yanyong Zhang, Chao Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-08-04
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.03223v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于提供的论文元数据、摘要与 24 个 PDF 语义检索片段完成，未获得完整原文；方法细节、特定实验数值与部分结论来自合理推断或明确标注的推测，建议结合原文验证。
