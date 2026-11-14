---
user_id: "cheng tan"
paper_id: 20746
arxiv_id: "2611.12886v1"
title: "Reframe: Observation-Aligned Multi-Teacher On-Policy Distillation for Agentic Visual Reasoning"
publish_date: "2026-11-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.12886v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.12886v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-11T02:00:30"
---
# Reframe: Observation-Aligned Multi-Teacher On-Policy Distillation for Agentic Visual Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic visual reasoning · multi-teacher on-policy distillation · tool use · observation alignment

## 一句话总结

Reframe 提出一个以“观测状态”而非仅输出分布为对齐对象的智能体视觉推理后训练框架：先用 GD-GRPO 通过证据感知奖励和随机丢弃原始视觉输入训练空间/时间专家，再用 OG-MOPD 在 rollout 中修复缺陷工具动作并从修复后的观测继续多教师在线蒸馏，在 14 个图像/视频基准上优于 Mix-RL 与 vanilla MOPD。

## 摘要

> Agentic reinforcement learning (RL) and multi-teacher on-policy distillation (MOPD) offer complementary routes to post-training. For visual agents, however, tool use presents two related challenges. Models may overlook useful tool-returned observations, while poorly grounded actions can limit the evidence available for subsequent distillation. We introduce Reframe, a post-training framework that treats the observation state, rather than only the output distribution, as an object of alignment. First, Ground-Drop-GRPO (GD-GRPO) trains spatial and temporal specialists with evidence-aware rewards and stochastic removal of the original visual input, encouraging them to acquire grounded evidence and use the returned observations. Second, Observation-Gated-MOPD (OG-MOPD) replaces deficient tool actions with canonical ones mid-rollout and resumes distillation from the repaired observation. The two stages complement each other, as learning to rely on tool-returned observations can help the policy benefit from observation repair. Neither stage requires a task-specific SFT cold start. Extensive experiments over 14 image and video benchmarks demonstrate that Reframe outperforms both Mix-RL and vanilla MOPD. Ablation results show that GD-GRPO increases observation dependence and tool gain compared with outcome-based GRPO.

Q1: 这篇论文试图解决什么问题？

（1）问题场景：视觉智能体通过裁剪、缩放、选帧等工具，把不可直接观察的细节转化为可读观测；但后训练通常只奖励最终答案或只匹配教师输出分布，观测本身是否有效、是否被使用，未被显式优化。（2）失败模式一——观测忽视：模型可能形式上调用工具，却在生成答案时回到原始图像、原始视频或语言先验；outcome-based GRPO 无法区分“借助工具答对”和“忽略工具答对”，因此会把捷径行为与真正的证据使用一起强化。对 MOPD 而言，如果学生的 continuation 没有真正以工具观测为条件，即使教师提供的 token 分布再准，也只是在输出层模仿，不能保证证据链成立。（3）失败模式二——动作 grounding 不足：缺失、无效或偏离目标的工具动作会返回低信息甚至无关观测；后续 on-policy distillation 从不良观测出发，教师看到或利用的证据状态与学生实际获得的状态错位，蒸馏信号被污染。更严重的是，错误动作会改变 rollout 分布，使信用分配把失败归因于后续推理，而不是动作本身。（4）两种失败互相耦合：观测忽视会降低调用工具的意愿；不良动作又让即使愿意读取观测的模型拿不到好证据。只在输出分布上对齐，无法区分“观测无效”和“策略不会用观测”，也无法给动作质量提供细粒度学习信号。（5）现有路线不足：agentic RL 常用稀疏结果奖励，容易受捷径与高方差困扰；MOPD 假设学生 rollout 的观测状态可用，而且多教师互补并不自动保证证据有效性；任务特定的 SFT 冷启动成本高，还可能把工具使用格式固化。（6）论文的切入点：把 observation state 当作第一等对齐对象，同时优化“获取有效观测”和“依赖观测推理”。GD-GRPO 用证据感知奖励和随机移除原始视觉输入来制造信息瓶颈；OG-MOPD 在 rollout 中修复缺陷动作，并从修复后的观测恢复蒸馏。两者分别对应“愿不愿意用”和“有没有好东西可用”。（7）隐含假设与可检验点：需要存在可判定的 canonical action 或动作有效性检查；工具观测与答案之间存在可学习的因果联系；专家教师能覆盖学生所需的证据类型；随机丢弃原图不会造成不可恢复的信息损失。若这些假设在开放工具或长尾任务中不成立，方法收益的边界会收窄。

Q2: 有哪些相关研究？

（1）多模态智能体与工具增强推理：一条脉络研究如何让 MLLM 调用裁剪、缩放、帧选择、检索等工具，把视觉感知扩展到高分辨率、长视频和外部知识；核心难点是工具选择、参数 grounding、观测整合和多步信用分配。Reframe 位于该脉络，但把工具返回的观测状态而不仅是最终答案作为优化对象。（2）Agentic RL 与 GRPO：以 group relative policy optimization 为代表的结果奖励 RL 在推理任务中广泛使用；后续工作引入过程奖励、格式奖励、工具调用奖励来缓解稀疏性。论文指出 outcome-based GRPO 可能强化“调用工具但不用观测”的捷径，因此需要证据感知奖励。（3）On-policy distillation 与多教师蒸馏：on-policy distillation 在学生自己生成的 rollout 上由教师提供 token 级监督，能缓解 exposure bias；多教师版本用空间/时间等专家做互补。vanilla MOPD 的关键假设是学生 rollout 的观测状态有效，Reframe 通过动作修复和观测门控放宽该假设。（4）Evidence-aware 与 observation-aware training：相关思路包括信息瓶颈、反事实输入丢弃、检索证据强制使用、工具返回内容 masking 等，目标是让模型真正以外部证据为条件。GD-GRPO 的随机移除原始视觉输入属于这一类，但它把这一机制与 grounding reward 和多教师蒸馏结合。（5）Visual grounding 与过程奖励：IoU 匹配、格式奖励、互补覆盖奖励用于约束区域或帧选择；片段显示 Reframe 用 IoU grounding reward，并奖励互补覆盖、抑制重复定位同一目标。该设计把“动作是否提供新证据”纳入奖励，而不只是动作是否合法。（6）评测与基准：图像和视频推理基准、工具使用基准、V-STaR 一类时空指标。论文覆盖 14 个图像和视频基准，并用观测依赖性和工具增益做机制级消融。总体 gap：已有工作分别研究工具 RL、蒸馏或证据使用，较少把“观测有效性”作为蒸馏的前置条件联合优化；Reframe 的贡献在于把 observation state 提升为对齐对象，并用两阶段设计处理其获取与使用。以上为领域定位，论文 Related Work 原文未在检索片段中完整出现。

Q3: 论文如何解决这个问题？

（1）总体框架：Reframe 是两阶段后训练框架，直接在基础模型上训练，不依赖任务特定的 SFT 冷启动。第一阶段 GD-GRPO 训练空间与时间专家；第二阶段 OG-MOPD 把多教师在线蒸馏与观测修复结合，让学生整合跨图像和跨视频能力。（2）GD-GRPO 的专家训练：空间专家侧重图像中的区域定位与裁剪，时间专家侧重视频帧选择；两者都使用证据感知奖励。可见片段明确提到，以预测区域与真值的 IoU 作为 grounding reward，并奖励互补覆盖、防止对同一目标的重复定位，从而鼓励策略获取“有信息量”的观测。除 grounding 外，合理推断还有动作格式或有效性约束，但片段未完整展开。（3）随机移除原始视觉输入：训练时以 0.5 的概率丢弃原始图像或视频，工具返回的观测仍然可见，因此答案必须依赖工具已经检索到的证据。rollout 会按预先采样的 drop flag 分组，并在子组内归一化优势，避免把“有原图”和“无原图”的样本混在一起比较。这个信息瓶颈是 GD-GRPO 促使模型使用观测、而不是复制原图的关键机制。（4）OG-MOPD 的动作修复：在学生 rollout 中检测有缺陷的工具动作，包括缺失、无效或偏离目标的动作；如果存在有效的 canonical action，就用规范动作替换原动作，并从修复后的观测继续 rollout 与蒸馏。片段还提到 web-search 工具不可用、缺失或无效动作只有在 canonical action 有效时才可修复，说明修复有明确的可用性边界。（5）观测门控蒸馏：OG-MOPD 不再无条件匹配学生原始 rollout 的教师分布，而是把观测有效性作为门控，让教师监督发生在“证据状态可用”的 continuation 上；这样可以减少教师证据状态与学生实际观测错位造成的蒸馏噪声。多教师分别提供空间与时间专长，学生在统一策略中整合。（6）两阶段互补：GD-GRPO 提升学生依赖工具观测的倾向，使它更能利用 OG-MOPD 修复后的好观测；OG-MOPD 提升观测有效性，使 GD-GRPO 的依赖有可靠对象。二者的组合不是简单叠加，而是分别作用于“观测使用”与“观测获取”两个环节。（7）实现与动作格式：片段显示训练使用 32 卡、蒸馏使用 8 卡（具体对应关系因断句需回原文确认），学生 rollout 生成与评分规则同址部署；视频动作以 JSON 给出 target_frames 的 1-based 唯一帧索引，并且每次调用至少移除当前帧的 50%。图像侧动作空间和蒸馏损失权重在可见证据中未完整给出。

Q4: 论文做了哪些实验？

（1）评测范围：论文在 14 个图像与视频基准上评估，其中片段明确提到“七个图像基准”；视频侧可能使用 V-STaR 的空间和时间指标（片段出现该表述，但完整用法需核对）。这些基准覆盖图像空间推理与视频时间推理，用于检验跨模态整合效果。（2）对比基线：主要对比 Mix-RL 与 vanilla MOPD；片段显示 Mix-RL 沿用 GD-GRPO 的 batch 配置，vanilla MOPD 使用相同的教师与学生训练设置，说明比较尽量控制变量。另以 outcome-based GRPO 作为消融基线，用来隔离证据感知奖励与输入丢弃各自的作用。（3）消融设计：GD-GRPO 对 outcome-based GRPO，用于检验观测依赖性与工具增益；OG-MOPD 的观测修复用于检验修复观测是否带来额外收益；两阶段组合用于检验互补性。片段还出现 specialist gap、学生超过专家教师等分析，说明作者对多教师蒸馏的上限和差距做了机制讨论。（4）训练配置：片段提到 32 卡训练、8 卡蒸馏、学生 rollout 与评分规则同址部署；具体 GPU 型号、batch size、学习率、蒸馏温度、损失系数未在可见证据中给出。论文强调不需要任务特定的 SFT 冷启动，这本身就是与常见“先 SFT 再 RL 或蒸馏”流程的对照。（5）评价指标：基准准确率是主指标；机制指标包括 observation dependence（移除或破坏工具观测后的性能变化）与 tool gain（有工具与无工具的性能差）；grounding 侧用 IoU 及互补覆盖；视频侧可能用 V-STaR 时空指标。片段只给出“工具使 Reframe 在七个图像基准上从 64.25 提升”这一起点，后续数值被截断，不能编造完整增幅。（6）证据缺口：14 个基准的具体名称、逐基准分数、教师模型规模、学生初始化、训练步数、统计显著性、失败案例分析均未在检索片段中完整出现；因此本报告对实验结论的复述以“优于基线”和“消融方向”为主，不虚构具体表格数值。

Q5: 发现了什么实验现象？

（1）观测依赖性提升：消融显示，GD-GRPO 相比 outcome-based GRPO 提高了模型对工具返回观测的依赖，并提升了工具增益。这说明证据感知奖励与随机移除原始输入改变了策略的证据使用方式，而不只是最终准确率；一个可能的机制是信息瓶颈迫使模型把工具观测纳入决策，否则在丢弃原图的子组中无法恢复关键信息。（2）工具带来实质收益：片段提到在七个图像基准上，工具使 Reframe 从 64.25 提升（后续数值缺失）。这至少表明工具使用不是形式上的，且 Reframe 的工具增益可测量；具体增幅以及是否所有基准一致，仍需原文表格确认。（3）学生可超过专家教师：片段出现“pass its specialist teachers”的表述，暗示多教师蒸馏后的学生可能在整体或部分指标上超过单个专家；同时“specialist gap”又提示某些专家子项仍存在差距。合理解释是：多教师提供了互补证据，修复机制又扩大了学生可用的观测分布，因此学生不必被单个教师的上限锁死；但句子残缺，不能确定“超过”是整体还是局部。（4）观测修复带来额外增益：OG-MOPD 的结果显示，修复后继续蒸馏有进一步收益；片段强调 repair provides task-relevant evidence，说明修复不只是“让动作合法”，而是把 continuation 的条件建立在更有信息量的观测上，从而让教师监督更有效。修复与 GD-GRPO 的组合可能产生正向交互：学生越依赖观测，修复的价值越大。（5）跨模态整合：片段称 Reframe 主要通过 on-policy training 整合图像与视频推理，说明统一策略可以在两种模态上获得增益，而不是各自为政。该现象与多教师空间和时间专家的分工一致，但跨模态迁移的具体来源（共享表示、蒸馏信号还是数据混合）在可见证据中未展开。（6）张力与反直觉点：随机丢弃原图在训练时减少了可用信息，却提升了工具依赖；修复动作替换了学生自己采样的动作，引入非学生策略的动作，理论上会造成分布偏移，但实验显示蒸馏收益；学生超过专家教师则挑战“蒸馏只能逼近教师上限”的直觉。这些现象都值得在原文中核对具体数值和适用条件。（7）边界与失败模式：web-search 工具不可用、canonical action 无效时不能修复、专家子项仍可能存在 gap；观测依赖性指标可能受基准和工具类型影响，不能直接外推为因果结论。片段未给出负结果或失败案例的完整列表，这是复现和评估方法时需要注意的证据缺口。

Q6: 有什么可以进一步探索的点？

（1）观测状态的形式化：可以把 observation state 定义为动作、返回内容、置信度与证据覆盖的联合变量，研究其可识别性、因果效应和信息量；用反事实干预（移除、替换、加噪观测）估计 observation dependence，而不仅看性能差。（2）动作修复策略：当前修复依赖 canonical action 的有效性；未来可以学习 repair policy，在开放动作空间、部分可观测、无标准答案时生成或检索替代动作；也可以研究“修复到什么程度”最优，避免过度修复抹掉学生的探索。（3）工具集扩展：片段提到 web-search 不可用，说明工具覆盖有限；可以扩展到检索、代码执行、数据库查询、科学仪器控制等，并研究多工具组合下的观测有效性与信用分配。对 AI-for-Science 场景，显微镜、遥感、医学影像、实验视频都是自然延伸方向。（4）蒸馏机制：修复后的 rollout 混合了学生动作与规范动作，存在 off-policy 偏差；未来可研究重要性加权、教师置信度门控、token-level 观测门控、多教师权重自适应，以及蒸馏与 RL 目标的联合优化。（5）训练课程与超参数：原始输入丢弃概率 0.5 是固定值还是调度值、是否需要从易到难、推理时是否保持同样的信息瓶颈，都是可探索点；drop ratio、修复率、教师数量与收益之间的 scaling trend 值得系统研究。（6）奖励设计：把“证据是否被使用”做成过程奖励，而不只是动作 IoU；可以引入互补覆盖、信息增益、反事实证据充分性、可验证的观测引用等信号，降低对 grounding 标注的依赖。（7）评测与基准：建立专门衡量观测依赖、工具增益、修复鲁棒性的 benchmark；覆盖工具失败、观测噪声、长视频、跨模态、开放域任务；加入人类评估与安全评估，防止模型学会“看起来在用证据”。（8）效率与部署：32 卡训练、8 卡蒸馏的成本较高；可探索参数高效蒸馏、异步 rollout、教师缓存、早期退出；同时研究推理时工具延迟或失败下的退避策略。

Q7: 总结一下论文的主要内容

（1）研究背景：视觉智能体的后训练有两条互补路线——agentic RL 让模型在工具交互中试错，multi-teacher on-policy distillation（MOPD）让学生在自己的 rollout 上吸收多个专家教师的分布。对视觉智能体而言，工具调用会引入一个关键中间状态：工具返回的观测。论文指出，现有方法主要在输出层做对齐，默认学生已经获得并使用了有效观测；但在真实 rollout 中，这两个条件都可能不成立。（2）问题拆解：第一，模型可能忽略工具返回的有用观测，形式上调用工具，实际仍依赖原始图像、原始视频或语言先验；第二，工具动作 grounding 不佳（缺失、无效、偏离目标）会返回低信息观测，使后续蒸馏建立在不良证据上。两者耦合：不依赖观测会降低工具使用意愿，不良动作又让依赖观测的模型无好证据可用。因此，仅优化最终答案或匹配教师 token 分布，无法区分“观测无效”与“策略不会用观测”。（3）核心洞察：Reframe 把 observation state 作为对齐对象，同时优化“获取有效观测”和“使用观测推理”。这与只对齐输出分布的蒸馏形成对比：蒸馏的 continuation 必须建立在可用观测上，教师信号才有意义；RL 的奖励也必须区分“用证据答对”和“不用证据答对”。（4）第一阶段 GD-GRPO：训练空间与时间专家。空间专家处理图像区域定位和裁剪，时间专家处理视频帧选择；两者使用证据感知奖励，片段明确提到 IoU grounding reward 与互补覆盖奖励，防止重复定位同一目标。更关键的是随机移除原始视觉输入：以 0.5 概率丢弃原图或原视频，工具返回观测仍可见，答案必须依赖工具检索到的证据；rollout 按预采样 drop flag 分组并在子组内归一化优势。这个信息瓶颈迫使专家学习“调用工具—获得证据—基于证据推理”，而不是复制原始输入。该阶段不需要任务特定 SFT 冷启动。（5）第二阶段 OG-MOPD：在学生 rollout 中检测缺陷工具动作，若存在有效的 canonical action，则替换缺失、无效或偏离的动作，并从修复后的观测继续 rollout 与多教师蒸馏；观测有效性作为门控，避免教师在学生不良观测上提供错位监督。多教师分别提供空间与时间专长，学生在统一策略中整合。两阶段互补：GD-GRPO 提升观测依赖，使学生更能利用修复后的好观测；OG-MOPD 提升观测有效性，使依赖有可靠对象。（6）实验设计：论文在 14 个图像与视频基准上评估，对比 Mix-RL、vanilla MOPD，并以 outcome-based GRPO 做消融；片段显示训练使用 32 卡、蒸馏使用 8 卡，学生 rollout 与评分规则同址部署；视频动作以 JSON 指定 target_frames，每次调用至少移除当前帧的 50%。评测不仅看基准准确率，还看 observation dependence 与 tool gain，以检验机制，而不只看最终得分。（7）主要发现：Reframe 在 14 个基准上优于 Mix-RL 与 vanilla MOPD；GD-GRPO 相比 outcome-based GRPO 提升观测依赖与工具增益；在七个图像基准上工具使 Reframe 从 64.25 提升（后续数值缺失）；学生可超过专家教师，同时 specialist gap 提示局部差距；观测修复带来进一步增益；图像与视频推理主要通过 on-policy training 整合。整体上，论文把“观测状态”从隐含中间变量提升为可训练、可门控、可修复的对齐对象，这是其与输出层蒸馏和结果奖励 RL 的核心区别。（8）意义与边界：Reframe 提供了一条不需要任务特定 SFT 冷启动的视觉智能体后训练方案，并用多基准与机制消融支撑“观测对齐”的主张。但可见证据也留下边界：canonical action 的有效性是修复前提，工具集在片段中受限于 web-search 不可用等条件；随机丢弃原图的概率与推理时行为是否一致、修复引入的非学生动作是否造成分布偏移、以及观测依赖指标是否因果，都需要回原文核对。总体而言，这是一篇把 agentic RL 与多教师 on-policy distillation 在“观测”层面耦合的系统性工作，适合按“问题—两阶段方法—机制消融—边界”的顺序精读。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你的 agent 方向直接重合：论文讨论视觉智能体的工具调用、观测使用与多教师 on-policy 后训练，属于 agentic RL 与 distillation 的交叉。

## 基本信息

- 作者：Haozhi Yuan, Handong Li, Yifan Zhang, Qianshan Wei
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-11-09
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.12886v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的全文片段，但片段存在截断、断行和指代不清；已对不确定内容做行内校准，未编造具体实验数值、基准名称或作者动机。
