---
user_id: "cheng tan"
paper_id: 17927
arxiv_id: "2610.32026"
title: "RSI-Opt: Recursive Self-Improvement Optimizer Agents For Model Training"
publish_date: "2026-10-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.32026.pdf"
pdf_url: "https://arxiv.org/pdf/2610.32026"
abs_url: "https://arxiv.org/abs/2610.32026"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-26T12:12:22"
---
# RSI-Opt: Recursive Self-Improvement Optimizer Agents For Model Training

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：recursive self-improvement · optimizer agents · llm agents · policy evolution

## 一句话总结

RSI-Opt 让外层 meta-agent 在多次训练运行之间演化由 prompt、可复用流程手册、可执行优化器代码库和决策节奏组成的优化器智能体策略，并用档案库保留与选择策略，从而把单次运行内的在线调优转化为可累积、可迁移的递归自我改进能力。

## 摘要

> 论文关注传统神经网络训练中优化器与调度需预先设定、而固定策略的 LLM 优化器智能体无法跨运行积累经验的问题。RSI-Opt 把优化器智能体的策略显式拆成四部分：提示词、流程手册、经过测试的优化器代码库和决策节奏；外层 meta-agent 根据既往运行证据重写策略，档案库保存所有被评估策略供后续演化。内层 coding agent 在训练过程中按计划检查损失与诊断信息，编辑 updater 与 analyst 程序，并在短试运行失败时回滚；最终评估指标对运行内智能体不可见。作者在 5 代、每代 4 个子代、3 个训练任务上演化出 21 个有效策略，并在新鲜种子、留出任务和 Burgers PDE 上评估，报告了 archive z-score、分类准确率和相对 L2 误差的改进；跨模型迁移与 PDE 迁移有正结果，但随 LLM 和任务变化。

Q1: 这篇论文试图解决什么问题？

（1）核心问题不是单纯的“在线调学习率”，而是“如何让优化器智能体在多次训练运行之间递归地改进自己的策略，并把改进变成可复用、可验证、可迁移的知识”。传统训练先选优化器与 schedule，其效果依赖架构、数据与训练动力学，换配方往往要反复跑实验和人工干预；LLM coding agent 虽然能在单次训练中监控 loss、诊断报告并改写 updater，但固定 agent policy 不会把上一次运行的经验带入下一次运行，导致每次运行都从头摸索。
（2）论文把问题拆成几个子问题：第一，策略如何表示，才能同时承载自然语言经验、流程性知识、可执行代码和决策时机；第二，跨运行的信用分配如何做，即哪些改动真正带来了收益，如何避免把偶然波动写进 playbook；第三，探索与利用如何平衡，既要让 meta-agent 改写策略，又要用 archive 防止遗忘和退化；第四，如何验证生成代码的合法性、安全性和可回滚性；第五，如何区分“在演化任务上有效”与“在未见任务、未见模型上仍有效”。
（3）论文的隐含假设包括：训练套件能代表目标分布；meta-agent 的 CPU proxy 实验对真实训练有预测力；多个任务的指标可以被 raw-space summary z-score 合理汇总；固定 update budget 下 agent 的决策延迟只影响墙钟成本、不影响更新次数；代码编辑与短试运行足以过滤大部分失败。这些假设在论文片段中没有被逐条检验，尤其是 proxy 实验与真实 GPU 训练之间的保真度、z-score 的标准化细节，以及训练套件与留出任务之间的分布关系，仍需回原文确认。
（4）评价设计上存在一个关键张力：论文既想让策略在训练套件上被搜索出来，又想证明它能迁移到 held-out task 和不同 LLM。若训练套件指标参与搜索、甚至任务脚本把某个 split 命名为 test，而该指标又用于演化，就可能产生协议层面的乐观偏差；论文用新鲜种子和 held-out 任务缓解，但片段中“training-suite metrics guide the search even when a task script names its evaluation split test”这句话提示需要仔细核对每个任务的隔离方式。
（5）另一个被论文明确提出但未完全解决的张力是：在 seen task 上，初始决策似乎捕获了大部分收益；在 unseen task 上，持续 adaptation 才带来更好结果。这说明演化出的策略可能同时包含“可复用的先验”和“对训练任务分布的过拟合”，二者如何分解、如何控制，是理解 RSI-Opt 的关键。

Q2: 有哪些相关研究？

（1）学习型与搜索型优化器。论文把 learned optimizers 归为“跨任务 meta-train 一个 update rule”的路线，把 Lion 等归为“符号搜索发现 update algorithm”的路线；这些方法得到的更新程序通常在目标训练开始前就固定，虽然更新可以依赖当前状态，但不会在目标运行中由 agent 继续改写。RSI-Opt 的演化对象不是单个 update rule，而是“指导运行内编辑 update rule 的 optimizer-agent policy”，这是它与 learned optimizer 的核心区别。
（2）经典优化器作为可组合积木。论文明确把 Adam、AdamW、Shampoo、SOAP、Muon，以及面向持续学习的 spectral orthogonal gradient projection 等视为 agent 可以选择和组合的 building blocks。这个定位很重要：RSI-Opt 不要求从零发明优化器，而是让 agent 在已有算法族上做选择、组合、改写和调度。
（3）LLM agent 改进程序与策略。相关路线包括用 LLM 优化 prompt 与解、演化 prompt、改进代码生成 scaffold、设计 agentic system、以及针对 evaluator 演化算法。Darwin Gödel Machine 维护一个被修改过的 coding agents 的 archive，并从中采样父代继续探索，其思想源自 self-improving Gödel machine；RSI-Opt 的外层搜索明确采用这种 archive-based structure。论文还引用了 persistent natural-language lessons、skill libraries、以及从对比多模型轨迹中学习 self-evolving procedural memory 等工作，这些机制都在解决“如何把经验跨 episode 携带”。
（4）RSI-Opt 的定位差异。它把 procedures 与 executable tools 同时放进 optimizer-agent policy：playbook 记录自然语言层面的诊断与操作流程，library 保存经过测试的优化器代码，prompt 只负责把任务、接口和 playbook 入口组织起来，decision cadence 则决定 agent 何时介入。每个有效且被评估的 child 都进入 archive，分数用于后续父代选择和最终策略选择；片段在此处截断，未完整说明分数是否还承担其他角色。与只演化 prompt 或只演化代码的工作相比，这种表示更接近“可执行的研究笔记”。
（5）相邻但论文片段未充分展开的方向包括 hyperparameter optimization / AutoML、meta-learning、program synthesis、以及科学机器学习中的 neural PDE solver。论文把 Burgers PDE 作为 held-out 科学计算任务，说明其目标不只是图像分类，但相关工作的系统对比在提供的片段中有限，不能据此判断它相对 AutoML 或 learned optimizer 的全面优势。

Q3: 论文如何解决这个问题？

（1）策略表示。一个 policy π 由四部分组成：prompt（任务描述、协议、程序接口与 playbook 入口）、playbook（自然语言流程、诊断经验、注意事项）、library（经过测试的 optimizer code 模块）、decision cadence（运行内何时介入、介入多少次）。这个拆分让“知识”与“代码”分离：prompt 保持相对稳定，playbook 和 library 承载大部分可累积经验，cadence 控制 agent 的干预频率。
（2）外层递归演化。算法从 π0 开始：baseline prompt 加上空 playbook 和空 library；在训练套件 T 与种子集 S 上评估 π0，并初始化 archive A。每一代 g，meta-agent 从 archive 中采样父代，阅读既往运行的证据（分数、失败、诊断、代码），改写生成 K=4 个 child policies；meta-agent 还可以自己运行 CPU proxy 实验来比较候选想法。child 需通过模板字段、Python 解析、大小限制等 validation checks；失败者退回 meta-agent 修复，最多两次。每个有效 child 在 T×S 上评估后进入 archive，archive 分数指导后续父代采样和最终策略选择。论文运行 5 代、每代 4 个 child，加上初始策略共 21 个 policy。
（3）内层运行适配。训练脚本、模型架构、数据顺序、loss、batch size 和参数更新次数固定；在 D 个 scheduled decision points，in-run agent 收到自上次决策以来的 loss history、analysis program 的报告、错误与 timing 信息。agent 在 workspace 中可查看和编辑文件、运行 shell 命令，但不能访问网络或 GPU。它主要编辑两个程序：coder 在每个 update 接收梯度并原地修改参数；analyst 按配置频率计算只读诊断。新的 updater 先在短 trial window 上测试，若抛异常则回滚。训练在 agent 决策时暂停，因此 agent latency 影响墙钟成本，而不改变模型更新预算；对话历史在单次运行内跨决策保留。最终评估指标对 in-run agent 隐藏，由外层 evaluator 在运行结束后使用。
（4）选择与迁移。archive 中的分数不是最终论文指标，作者会对高分候选在额外种子上复评，例如 p016 在四种子均值 +14.55 下被选中；最终比较使用 fresh seeds。策略与具体 in-run LLM 解耦，因此可以把一个模型上演化出的 policy 迁移到另一个模型；论文报告了 Opus 5.5 到 GPT-5.5 的跨模型迁移，以及 Burgers PDE 上的迁移。
（5）设计 rationale。第一，archive 同时承担记忆、探索和防遗忘，避免每代从当前最优重新开始；第二，playbook 让 agent 记录“为什么”而不只是“怎么做”，library 让可执行知识可复用、可测试；第三，固定 update budget 与隐藏最终指标，试图把“策略搜索”与“目标任务过拟合”分开；第四，短试运行和回滚把代码编辑的风险限制在可恢复范围内。这些设计使 RSI-Opt 更接近“可审计的自我改进实验平台”，而不是一个黑箱AutoML 控制器。

Q4: 论文做了哪些实验？

（1）训练套件。论文称在 3 个训练任务上做 5 代演化。从方法与结果片段可辨认的任务包括：ResNet-18 / CIFAR-10 图像分类；WikiText-2 语言建模，使用 Adam 与 step decay；OPAL 的 noisy-label CIFAR-10 任务，带 10% 错误标签。第三个任务中 agent 能看到 development set，但看不到 test accuracy。训练套件指标用于演化搜索；最终论文表格在 fresh seeds 上报告，静态基线用 5 个种子，RSI-Opt 与 single-agent 大多用 3 个种子（mean ± std）。
（2）模型与基线。in-run 模型包括 Opus 5.5、GPT-5.5、GPT-6.1。基线至少包括：static recipe（固定优化器配方）、tuned LR（调过的学习率）、single-agent baseline（固定 prompt，无累积 playbook/library，每次运行同一模板）、first-change frozen（只在第一次决策改一次然后冻结）、以及 full within-run adaptation（持续 adaptation 的 fixed-prompt baseline）。论文还比较 archive score 与 fresh-seed 复评，避免只用搜索分数选策略。
（3）指标。分类任务用 accuracy（%）；Burgers PDE 用 mean relative L2 error；多任务汇总用 raw-space summary z-score。z-score 的具体标准化方式、方向与聚合权重在提供的片段中不完整，不能自行补公式。论文报告的是“representative raw-space summary z-score”，不是单一任务 accuracy 的直接平均。
（4）演化协议。5 代、每代 4 个 child，共 21 个 policy（含 π0）；所有有效 child 都进入 archive；validation 要求模板字段、Python 解析和大小限制，失败 child 最多修复两次；最终对高分候选在额外种子上复评。片段提到 seeds 200、201、202 不指导 evolution 或 final-policy selection，说明最终评估与搜索种子有隔离意图；但具体每个表用哪些种子、static 5 seeds 与 RSI 3 seeds 的配对方式，需要回原文核对。
（5）留出任务与 PDE。论文把训练套件之外的任务作为 held-out，包括 held-out ML 任务和 PDE 任务；PDE 部分涉及现有 neural solvers 及其默认配方，片段明确给出 Burgers PDE 的结果，其他 PDE 的逐项结果未在证据中展开。Burgers 上报告的是相对 fixed-prompt agent baseline 的 mean relative L2 error 改善倍数：Opus 5.5 约 2.7 倍，GPT-6.1 约 8.8 倍；论文同时明确“PDE transfer varies across LLMs and tasks”。
（6）受控比较。论文用 first-change frozen vs continued adaptation 分离“第一次决策的收益”与“持续适配的收益”；用 single-agent fixed prompt vs RSI-Opt 分离“跨运行累积策略”的贡献；用跨模型迁移检验策略是否绑定某个 LLM；用 static/tuned LR 检验传统固定配方是否足够。片段还提到一个 control：测试的默认配方在不同任务上不足，但这只能说明“这些默认值不够”，不能证明“所有精心选择的固定配方都不行”。

Q5: 发现了什么实验现象？

（1）演化出的知识主要沉淀在 procedures 与 executable tools，而不是 prompt。meta-agent 对 fixed prompt 的改动很小，child 只增加 0–23 行，且多是指向 playbook 的摘要；大部分累积知识出现在 playbook 和 library modules。这说明 RSI-Opt 的“经验”不是把越来越长的指令塞进 prompt，而是把诊断流程和可执行代码模块化。
（2）archive 分数有提升，但需要区分口径。Generation 0 加空 playbook/library 的引用后得 +10.30；最佳 archive score 是 generation 4 的 p016，+14.63；对三个最高分 eligible candidates 在额外两个种子上复评后选 p016，其四种子均值 +14.55，另外两个为 +14.42 与 +14.19。摘要则报告代表性 raw-space summary z-score 从 +10.48 升到 +14.33（Opus 5.5）。三组数字的起点/终点不完全一致，可能是候选集、种子或汇总方式不同；不应把 +10.30、+10.48、+14.33、+14.63 混成一条曲线。
（3）早期策略已经发现了一些可迁移的优化知识：在 transformer 矩阵上使用 Muon、把 learning rate 衰减到零、按参数角色做差异化处理；同时策略会警告“学习率下降后短暂的 loss drop 不等于持久进步”。这表明 agent 学到的不只是代码片段，还包括对训练动力学的诊断性启发式。
（4）seen task 与 unseen task 出现明显分离。论文的 controlled comparisons 表明：在 seen tasks 上，初始决策捕获了大部分观察到的收益；在 unseen tasks 上，continued adaptation 才进一步改善结果。换句话说，演化策略的“先验”在熟悉任务上很有效，但面对分布外任务时，仍需要运行内持续调整；这既是 RSI-Opt 的优势，也是其泛化边界的直接证据。
（5）跨模型迁移有正信号但不稳定。Opus 5.5 上演化的策略在 evolution suite 上从 +10.48 提升到 +14.33；通过 cross-model policy transfer 到 GPT-5.5 的 held-out tasks，从 −0.36 提升到 +3.11。提升方向是正的，但幅度远小于同模型 evolution suite，且论文明确 PDE transfer 随 LLM 和任务变化。不能把一次跨模型成功理解为普遍可迁移。
（6）PDE 上的相对提升很大，但任务间差异也大。Burgers PDE 上，相对 fixed-prompt agent baseline，Opus 5.5 将 mean relative L2 error 降低约 2.7 倍，GPT-6.1 降低约 8.8 倍；这说明演化策略可能带来实质性的科学计算收益，但“倍数”是相对 baseline 的比值，不是绝对误差，且其他 PDE 的结果在片段中不足以判断普遍性。
（7）在饱和的 CIFAR-10 任务上，收益很小。ResNet-18/CIFAR-10 的 static 为 94.60±0.16，tuned LR 为 94.62±0.23；RSI-Opt 在 Opus 5.5 下为 94.78±0.03，在 GPT-5.5 下为 94.77±0.17，single-agent 分别约 94.55 与 94.58。提升约 0.2 个百分点，且 tuned LR 已接近；GPT-6.1 行在片段中截断。这个结果提示：RSI-Opt 的收益高度依赖任务是否还有优化空间，不能只看相对提升。
（8）一个需要回原文核对的受控结果：片段称相对 first-change frozen，full baseline 在 held-out ML 上增加约 2.4 z，而 evolved agent 增加约 1.4 z。若按字面理解，evolved agent 的增量反而小于 full baseline，这与“演化策略更强”的主叙事存在张力；可能是比较对象、符号方向或指标口径不同。报告保留这个张力，不据此下结论。
（9）评估协议存在潜在乐观偏差。片段明确说 training-suite metrics 指导搜索，即使某个任务脚本把评估 split 命名为 “test”；作者用 fresh seeds 评估这些任务上的表现，并用 held-out task 做迁移检验，但这仍要求读者核对每个任务的 test 是否真正隔离。另一个边界是 agent 决策会暂停训练，wall-clock 成本没有被 update budget 完全捕获。

Q6: 有什么可以进一步探索的点？

（1）扩大任务与模型覆盖。当前证据主要是 3 个训练任务、5 代演化、少量 LLM 和 Burgers PDE；未来应在更多架构（Transformer、CNN、状态空间模型）、更多数据模态（语言、视觉、表格、科学观测）、更多优化器族上重复，检验 playbook 与 library 的可迁移性是否随任务多样性 scaling。
（2）做组件归因。需要系统 ablation：只演化 prompt、只演化 playbook、只演化 library、只演化 cadence，以及四者组合；区分“可复用先验”与“运行内 adaptation”的贡献；分析 archive 大小、父代采样策略、child 数量、代数对收益的影响。论文已经用 first-change frozen 做了初步分离，但更细的 factorial design 仍然缺失。
（3）提高 proxy 实验的保真度。meta-agent 自己跑 CPU proxy 实验是 RSI-Opt 的亮点，但 proxy 与真实 GPU 训练之间的 ranking 是否一致、proxy 是否会奖励“看起来合理但实际无效”的代码，仍需验证。未来可以研究 proxy 的置信区间、跨任务校准、以及把真实短跑结果反馈给 meta-agent 的混合方案。
（4）改进迁移与元学习。跨模型迁移目前有正信号但不稳定；未来可以学习“模型无关的 playbook”、按 in-run LLM 的能力做 policy adaptation、用多个模型的轨迹训练 meta-agent，甚至把 policy 分解为“任务无关诊断层”和“模型特定执行层”。同时要测试从 Opus 到 GPT、从大模型到小模型、从 ML 到 PDE 的双向迁移。
（5）强化评价方法。需要 pre-register 搜索空间与种子、严格隔离 test split、报告 compute-matched baseline、给出 wall-clock 与 agent latency、用多层 bootstrap 估计方差；z-score 的标准化与聚合方式应公开，避免不同任务尺度主导结论。还应报告失败任务与负迁移，而不仅是成功案例。
（6）安全与可靠性。可执行代码库带来明显的安全与稳健性风险：需要 sandbox、静态检查、资源限制、异常回滚、代码 diff 审计、以及防止 reward hacking 的 evaluator 设计。论文的 validation 只覆盖模板字段、Python 解析和大小限制，未来应加入数值稳定性检查、梯度异常检测、以及对训练脚本副作用的隔离。
（7）科学机器学习扩展。Burgers 只是起点；未来应在更多 PDE（Navier-Stokes、Darcy flow、波动方程、多物理耦合）、不同离散化、不同神经求解器、以及带物理约束的 loss 上测试。若迁移到生物医学或生物科学，还需要领域特定的数据约束、可解释性要求和安全审查；论文本身没有生物应用证据。
（8）理论与开放性问题。RSI-Opt 目前是经验系统，缺少对“递归自我改进何时收敛、何时过拟合、archive 如何影响探索”的理论刻画。可以借鉴 quality-diversity、open-ended learning、meta-learning 的泛化界，研究策略表示的可组合性、archive 的覆盖度与最终性能之间的关系，以及自我改进系统中的误差累积。
（9）与 AutoML/HPO 的融合。把 RSI-Opt 作为外层策略搜索器，与 Bayesian optimization、multi-fidelity HPO、learned optimizer 结合，可能比纯 LLM 改写更样本高效；但也要避免把 HPO 的搜索空间与 LLM 的开放式代码编辑混在一起导致不可比。

Q7: 总结一下论文的主要内容

1. 问题与动机。论文从传统训练的一个现实痛点出发：优化器与学习率调度通常在训练前选定，但其效果依赖架构、数据和训练动力学；换配方往往要反复实验和人工干预。LLM coding agent 提供了一种新的适应方式：它可以在训练中读取 loss 与诊断信息，编辑优化器程序。但固定 agent policy 不会跨运行积累经验，每次训练都从同一提示词开始，无法把“上次哪些改动有效、哪些是假象”带入下一次。RSI-Opt 要解决的就是这个“跨运行递归自我改进”的问题。
2. 方法主线。RSI-Opt 把 optimizer-agent policy 显式表示成四元组：prompt、playbook、library、decision cadence。外层 meta-agent 从 archive 中采样父代，根据既往运行证据重写策略，生成 4 个 child；child 通过模板、Python 解析和大小检查后，在训练套件与种子上评估，再进入 archive。archive 既保存历史策略，也为父代采样和最终选择提供分数。内层 in-run agent 在固定的模型、数据、loss、batch size 和 update budget 下，按计划在 20 个左右的决策点检查 loss history 与 analyst 报告，编辑 coder（接收梯度并原地更新参数）和 analyst（只读诊断）；新 updater 先短试运行，异常则回滚。最终评估指标对 in-run agent 隐藏，训练在 agent 决策时暂停，因此延迟影响墙钟但不影响更新次数。
3. 实验主线。作者运行 5 代、每代 4 个 child，共 21 个 policy，没有 invalid policy。训练套件包括 ResNet-18/CIFAR-10、WikiText-2 + Adam/step decay、OPAL noisy-label CIFAR-10（10% 错误标签）。评估使用 fresh seeds（最终表多为 3 seeds，static 为 5 seeds），并覆盖 held-out ML 任务与 PDE 任务。基线包括 static recipe、tuned LR、single-agent fixed prompt、first-change frozen 和 full within-run adaptation。跨模型迁移测试 Opus 5.5 演化策略到 GPT-5.5；PDE 重点报告 Burgers 的 mean relative L2 error。
4. 关键发现。第一，演化出的知识主要落在 playbook 和 library，而不是 prompt；child prompt 只增加 0–23 行，多是摘要和入口。第二，archive 分数从 Gen0 的 +10.30 提升到 p016 的 +14.63，p016 四种子均值 +14.55；摘要报告代表性 z-score 从 +10.48 到 +14.33，数字口径不完全一致，需要核对。第三，早期策略已经发现 Muon on transformer matrices、learning-rate decay to zero、parameter-role-specific treatment 等可复用知识，并学会警告“短暂 loss drop 不等于持久进步”。第四，seen task 上初始决策捕获大部分收益，unseen task 上 continued adaptation 才进一步改善，说明策略同时包含可复用先验与任务特定适配。第五，跨模型迁移有正信号：GPT-5.5 的 held-out 从 −0.36 到 +3.11；Burgers 上 Opus 5.5 约 2.7 倍、GPT-6.1 约 8.8 倍的相对 L2 改善，但 PDE transfer 随 LLM 与任务变化。第六，在饱和的 CIFAR-10 上提升只有约 0.2 个百分点，说明收益依赖任务是否还有优化空间。
5. 边界与意义。RSI-Opt 的意义在于把“优化器 agent”从单次运行的固定策略，推进为一个可跨运行积累 procedures 与 executable tools 的演化系统；archive-based outer loop 与可编辑 updater/analyst 的组合，使它同时具备开放探索和可回滚执行的特点。但证据仍有限：训练任务只有 3 个，跨模型与 PDE 迁移不稳定，z-score 聚合与摘要/结果数字存在口径差异，first-change frozen 的受控结果甚至出现“evolved agent 增量小于 full baseline”的待核对张力；此外，agent latency、代码安全、test split 隔离和 proxy 保真度都需要更严格的评估。总体而言，这是一篇方法概念清晰、实验设计有系统性问题意识的 agent-for-training 工作，适合作为“LLM 自我改进 + 优化器自动化 + 科学计算迁移”的参考，但不宜把摘要中的倍数直接外推为普遍结论。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent / self-improvement 方向高度相关：RSI-Opt 把 LLM agent 的策略作为演化单元，结合 archive、meta-agent、可执行工具和运行内 adaptation，是“agent 改进 agent”的一个具体范式。

## 基本信息

- 作者：Binghang Lu, Yige Sun, Guang Lin
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-10-26
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.32026`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的摘要、方法与结果片段，并纠正了 heuristic_draft 将训练任务列表误当作核心方法、以及把截断数字当作完整结论的问题；对摘要与结果中不一致的 z-score 口径保留了两套数字并标注需回原文核对。
