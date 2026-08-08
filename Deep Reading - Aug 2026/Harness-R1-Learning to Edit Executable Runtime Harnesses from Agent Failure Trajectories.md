---
user_id: "cheng tan"
paper_id: 6208
arxiv_id: "2608.02276v1"
title: "Harness-R1: Learning to Edit Executable Runtime Harnesses from Agent Failure Trajectories"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02276v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02276v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:53:50"
---
# Harness-R1: Learning to Edit Executable Runtime Harnesses from Agent Failure Trajectories

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent harness · reinforcement learning · GRPO · failure trajectories

## 一句话总结

本文提出 Harness-R1——首个将智能体运行时 harness 的编辑本身训练成一种可学习能力的方法，用独立 9B 工程师模型在冻结目标智能体的失败轨迹上通过冷启动 SFT 和在线 GRPO 学习输出可执行补丁，在 WebShop、ALFWorld、DBBench 上将 Qwen3.5-9B 的成功率从 44.3% 提升到 53.6%。

## 摘要

> Agents built around large language models continually accumulate interaction trajectories during deployment, yet their behavior typically remains fixed. Beyond updating model weights, these trajectories can improve the agent harness that constructs context, mediates tools, validates actions, and recovers execution. We introduce Harness-R1, the first method, to our knowledge, that makes failure-conditioned, lifecycle-wide editing of an existing executable runtime a learned capability. It post-trains a dedicated harness engineer with online reinforcement learning so that its edits are optimized for the realized task success they produce, rather than proposed by a fixed editor. A separate 9B engineer converts batches of target-agent failures into validated executable patches; fresh same-batch reruns of the frozen target provide outcome rewards, so training updates only the engineer. Cold-start supervised fine-tuning initializes this editing policy, which is then trained online with group-relative policy optimization. Across WebShop, ALFWorld, and DBBench, Harness-R1 raises vanilla Qwen3.5-9B success from 44.3% to 53.6% (+9.3 percentage points). After direct target-agent fine-tuning, a target-specific engineer raises the average further from 59.2% to 64.2% (+5.0 points); because these gains hold both before and after fine-tuning the target, Harness-R1 points toward co-evolving the harness engineer and the target agent.

Q1: 这篇论文试图解决什么问题？

论文聚焦于以下问题：LLM 智能体在部署中持续产生交互轨迹，但这些轨迹通常只被用来更新模型权重，智能体周围的运行时体系（harness）——包括上下文构建、工具调用中介、动作验证、错误恢复等组件——却保持不变。现有工作或者训练任务决策模型本身（actor），或者使用固定的编辑器来优化 harness，但后者不会根据实际结果更新编辑器的参数。因此论文提出的核心问题是：能否把“根据失败轨迹编辑可执行运行时 harness”本身变成一个通过强化学习学习到的能力？即让一个独立的“harness 工程师”模型学习在给定一批失败轨迹的条件下，产出能实际提高任务成功率的补丁。论文还指出了直接训练 harness 工程师的两大挑战：(1) 可编辑运行时涉及相互依赖的执行组件（上下文、工具、验证、恢复），修改一处可能影响其他部分；(2) 编辑的效果只有在真实执行后才知道，需要反馈回路。此外，现有 harness 优化方法通常由固定编辑器提出补丁，再用结果选择或迭代改进，但编辑器参数本身不更新，限制了改进上限。Harness-R1 针对这些问题，采用“冻结目标 agent + 训练独立工程师”的分离式设计，用同批次重跑获得结果奖励，从而让工程师的编辑策略向真实任务成功率的提升优化。

Q2: 有哪些相关研究？

相关研究可分为两条主线：一条是直接改进任务决策模型（actor），通过监督微调、强化学习或在线学习提升智能体本身（例如 Xia et al., 2026; Lu et al., 2026; Shi et al., 2026 等）；另一条是保持模型权重固定，优化智能体周围的结构，即 harness。在 harness 演化方面，近期工作把 harness 视为可执行的、多组件的优化对象：一类方法从执行轨迹、评估分数和奖励中搜索或合成整个 harness（例如 Meta-Harness）；另一类方法让固定的 proposer 提出补丁并用结果筛选或迭代改进（如 HarnessX 等工作用跨 harness 的 GRPO 训练任务模型）。然而，这些方法的 harness 提议者通常固定不变，结果只用于选择补丁而不直接更新提议者参数。另有一些更直接的相关工作后训练策略来编辑或控制模型外部结构，但要么限制编辑空间，要么把编辑与任务求解耦合在一起。Harness-R1 的关键区别在于：训练一个独立的、专门用于编辑 harness 的工程师模型，其更新仅由编辑后目标智能体的实际任务成功率驱动，并且编辑是针对失败轨迹批次进行的生命周期级修改。

Q3: 论文如何解决这个问题？

Harness-R1 采用两阶段训练流程，核心是训练一个独立的 9B 参数的 harness 工程师模型，同时保持目标智能体（target agent）完全冻结。
1. 任务设定：将 harness 编辑形式化为一个批次条件学习问题。输入是一批目标智能体的失败轨迹，输出是经过验证的可执行补丁（patch），这些补丁修改目标智能体的运行时 harness——包括上下文构建、工具中介、动作验证和执行恢复等组件。补丁必须可执行、可验证，并作用于同一批任务。
2. 冷启动监督微调（cold-start SFT）：先用已有的监督数据（例如由固定编辑器产生的补丁或人工构造的编辑示例）初始化编辑策略，让工程师学会基本的补丁格式和编辑行为。
3. 在线强化学习（GRPO）：随后使用 group-relative policy optimization 在真实结果奖励上继续训练。具体流程是：工程师对一批失败轨迹生成多个候选补丁（构成一个 group），将每个补丁应用到冻结的目标智能体上，让目标重新运行同一批任务；根据重跑的成功率变化计算奖励（例如前后成功率之差或归一化结果）。这个奖励只反向传播到工程师模型的参数，目标智能体始终冻结。GRPO 用组内相对优势来更新策略，使得能够生成带来更高任务成功率的补丁的工程师行为被加强。
4. 与目标微调的兼容：论文还测试了先对目标智能体进行直接微调（即利用同一批失败轨迹做 SFT 或 RL 提升目标自身）再训练的工程师，形成两阶段共同进化；由于工程师训练不触碰目标参数，两者可以交替进行。关键设计是“冻结目标 + 同批重跑”，这保证了奖励直接反映编辑的实际效用，避免固定编辑器或离线评估的偏差。

Q4: 论文做了哪些实验？

论文在三个交互式基准上评估 Harness-R1：WebShop（在线购物）、ALFWorld（家用机器人任务）、DBBench（数据库操作）。这些环境分别强调不同形式的智能体执行（如工具调用、多步骤规划、结构化查询等）。实验旨在回答四个问题：(1) 基于结果训练的 harness 编辑是否优于基于监督学习的编辑和强固定编辑器？(2) 在直接训练目标智能体之后，harness 编辑是否仍然有用？(3) 编辑能否迁移到未见过的任务？(4) 在直接目标微调前后，增益是否保持？评估指标为任务成功率。主实验使用基础模型 Qwen3.5-9B 作为目标智能体。Harness-R1 将其平均成功率从 44.3% 提升到 53.6%（+9.3 个百分点），在所有三个基准上都有提升，且最大绝对增益出现在某个基准（具体环境未在摘要中说明）。进一步，在对目标智能体进行直接微调之后，原本的工程师带来的增益仍然有效；接着用针对特定目标的工程师（在目标微调后重新训练的工程师）将平均成功率从 59.2% 进一步提升到 64.2%（+5.0 个百分点）。这些结果说明：即使目标已经过直接微调，harness 编辑仍然能带来额外收益，且工程师可以随目标共同进化。

Q5: 发现了什么实验现象？

实验揭示的关键现象包括：
- 结果训练的编辑优于监督基线：Harness-R1 的增益显著，说明用实际任务成功率作为奖励能有效引导补丁质量。
- 提升在所有三个基准上普遍出现，说明 harness 编辑不是只在单一环境有效，而是具有跨环境泛化能力；不同环境对 harness 不同组件的敏感性不同，但方法均能改进。
- 增益在目标直接微调后依然保持：即使目标模型已经通过 SFT/RL 吸收了失败轨迹信息，harness 编辑仍能带来额外的 +5.0 点提升，说明 harness 层面与模型权重层面的改进是互补的、非冗余的。
- 这指向共同进化：由于工程师训练不依赖目标内部表示，只依赖外部结果，因此可以在目标不同训练阶段反复使用；目标微调后再训练的工程师进一步提升，表明两者可以协同迭代。
- 推断：GRPO 的组内相对奖励有效避免了单个补丁的噪声，使得工程师能稳定学到编辑策略。但具体消融（如去掉 SFT、改变重跑次数、patch 空间限制等）论文中应有更细的分析，当前摘要未给出（推测，需原文确认）。

Q6: 有什么可以进一步探索的点？

基于论文的设定和结果，可以探索的方向包括：
1. 完整的共同进化协议：论文展示了工程师可以在目标微调前后都带来增益，但尚未实现真正的交替迭代；未来可以研究工程师与目标智能体多轮交替训练的动态，以及如何避免过拟合到当前目标版本。
2. 扩大编辑空间：当前 patch 主要针对上下文构建、工具中介、动作验证和恢复执行；可以扩展到更广泛的运行时组件，如记忆管理、任务分解策略、提示模板的动态选择等。
3. 规模效应：9B 工程师已经有效，更大或更小的工程师模型对编辑质量的影响、蒸馏到小模型的可能性、工程师本身是否需要与目标同规模等。
4. 跨环境迁移：当前在三个基准上分别训练和评估，是否可以训练一个跨环境的通用工程师，或者在一个环境编辑后迁移到另一个环境。
5. 奖励设计：当前用同批次重跑的成功率变化作为奖励，可以考虑更细粒度的过程奖励（如中间步数、工具调用正确性）、成本约束（重跑次数、token 开销）、以及多目标（成功率与稳定性）。
6. 失败轨迹的选取：当前用失败批次，失败类型多样性、批次大小、是否混合成功轨迹对编辑效果的影响值得研究。
7. 可解释性和安全性：可执行补丁可能引入风险；需要研究如何审计补丁、避免破坏原有能力、以及编辑器产生的故障模式。
8. 应用到科学领域：结合用户画像中的 ai-for-science 方向，这种 harness 编辑方法可用于科学工具链的自动修复，例如让智能体学会更好的调用实验工具、数据分析库等。

Q7: 总结一下论文的主要内容

论文围绕一个核心观察展开：LLM 智能体在部署中不断积累失败轨迹，但这些轨迹的用途通常局限于更新模型权重，而决定智能体如何与外部世界交互的执行时环境（harness）——包括上下文构建、工具中介、动作验证、错误恢复——往往保持不变。作者提出，失败轨迹同样可以用于改进这些可执行运行时组件，并引入 Harness-R1，据我们所知是第一个把“基于失败条件的生命周期级 harness 编辑”训练成一种可学习能力的方法。

方法上，Harness-R1 训练一个独立的 9B 参数的“harness 工程师”模型。训练分为两步：先用受监督的数据进行冷启动 SFT，让工程师学会生成可执行的补丁；再用在线强化学习（group-relative policy optimization，GRPO）优化编辑策略，奖励由冻结的目标智能体在重新运行同一批任务后的实际成功率变化给出。这个设计的关键在于：目标智能体完全冻结，奖励只更新工程师，因此编辑效果被直接归因于补丁的真实收益。

实验中，Harness-R1 在 WebShop、ALFWorld、DBBench 三个交互式基准上对基础 Qwen3.5-9B 智能体取得了平均成功率从 44.3% 到 53.6%（+9.3 个百分点）的提升。在直接对目标智能体进行微调后，论文还显示目标特定的工程师能将平均成功率从 59.2% 进一步推进到 64.2%（+5.0 个百分点）。由于这些增益在目标微调前后都存在，且目标微调后工程师仍能提供额外收益，论文认为 harness 工程师与目标智能体有希望实现共同进化（co-evolution）。

论文的主要贡献包括：首次将 harness 编辑形式化为一个可学习的、结果驱动的优化问题；提出了一套完整的训练协议（SFT 冷启动 + GRPO 在线训练 + 冻结目标重跑奖励）；在三个多样化的基准上验证了该方法优于基线，并展示了与目标微调的互补性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本论文与智能体研究（agent，权重 0.10）直接相关，关注 LLM 智能体的运行时优化与失败轨迹利用。

## 基本信息

- 作者：Shuai Shao, Kangning Zhang, Qingyao Li, Shijian Wang, Hao Wang, Wenxiang Jiao, Yuan Lu, Yi Guo, Weiwen Liu, Weinan Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02276v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了所提供的 arXiv 摘要和全文语义检索命中片段（主要来自 Introduction、Related Work、Method、Experiments 与 Main Results），并在此基础上进行了补全与合理推断。
