---
user_id: "cheng tan"
paper_id: 16138
arxiv_id: "2610.18766v1"
title: "Rubrics from the Task, Verdicts from Evidence: Pointwise Rubric Rewards with Tool-Augmented Judges"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.18766v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18766v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-02T01:02:29"
---
# Rubrics from the Task, Verdicts from Evidence: Pointwise Rubric Rewards with Tool-Augmented Judges

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：rubric-based reward model · tool-augmented judge · pointwise reward · label-blind distillation

## 一句话总结

ReVeR 让 rubric 生成器只根据任务指令推理出硬规则与分级原则，令带 Python 工具的点式裁判对每个响应独立取证并逐条判定，再用“偏好标签仅作过滤”的标签盲蒸馏训练二者并构建 ReVeR-Traces，从而在偏好准确率、测试时扩展和下游策略优化上优于成对 rubric RM，同时揭示 rubric 保真度、偏好准确率与下游效用是三种不同属性。

## 摘要

> Rubric-based reward models (RMs) evaluate responses against explicit criteria. However, existing methods often derive criteria from observed response candidates, introducing response-induced bias that may limit rubric generalizability. Their judges also struggle with mechanical constraints when relying on reading alone, and commonly used pairwise judges provide only relative preferences, complicating their direct use for policy optimization. We propose ReVeR, a pointwise rubric RM that derives hard rules and graded principles by reasoning about the instruction alone and evaluates responses independently through tool-augmented judging. Both components are trained through label-blind distillation, in which preference labels only filter a teacher's outputs, yielding ReVeR-Traces, a large-scale dataset of rubric-reasoning and judging trajectories. Experiments demonstrate that ReVeR outperforms pairwise rubric RMs in average preference accuracy, most clearly on instruction following, as well as in test-time scaling and downstream policy optimization. We further find that generator RL improves preference accuracy while reducing rubric fidelity, and that higher preference accuracy does not necessarily translate into better downstream policies. These findings highlight that rubric fidelity, preference accuracy, and downstream utility are distinct properties, shedding light on future RM research.

Q1: 这篇论文试图解决什么问题？

这篇论文的问题意识是拆开 rubric 奖励模型的两个耦合环节：评价标准从哪里来，以及判决如何产生；它并不是要再训练一个更强的 LLM judge。
1. 非可验证任务缺少唯一正确答案，传统 holistic scalar RM 容易不可靠；rubric RM 用显式标准提高可解释性和可诊断性，但标准质量与判决质量都会决定最终奖励。
2. 现有 rubric 生成多依赖观测到的响应候选：让教师对比偏好/拒绝响应，提出能区分二者的标准；或分析多个质量不同的候选，找出差异和常见失败模式。这样得到的 rubric 具有区分度，却带有 response-induced bias：标准可能编码当前候选集的偶然特征，而不是任务本身的要求；换一批响应、换一个策略或换一种风格分布后，rubric 可能不再适用。
3. 裁判侧有两个独立缺陷。其一，纯阅读式 judge 难以可靠执行机械约束，例如字符计数、格式、精确短语、长度上限；语言模型在计数和逐字符检查上容易出错。其二，成对 judge 只给相对偏好，不给单个响应的绝对分数，且受呈现顺序影响；这既引入 order bias，也使它难以直接用于 best-of-N 选择、DPO 配对构造和 GRPO 奖励。
4. 训练侧缺少 rubric reasoning 与 tool trajectory 的监督标签。偏好标签是弱信号；若把标签直接暴露给生成器，可能把“区分这对响应”的偏差写进 rubric。论文因此采用 label-blind distillation：教师先生成推理、rubric 和判定轨迹，偏好标签只用于过滤教师输出，不进入学生输入或监督文本。
5. 评价侧，社区常把偏好准确率当作 RM 的主要甚至唯一指标；但 rubric 是否忠实于任务、是否覆盖专家标准（rubric fidelity），以及奖励是否真的改善下游策略（downstream utility），可能与之分离。生成器 RL 可能提高准确率却改变标准语义；高准确率 RM 也可能被策略利用，产生高偏好分但低合规性的策略。
6. 隐含假设包括：仅凭指令可以推出足够覆盖的 rubric；hard rule 与 principle 的类型系统足以表达质量结构；Python 工具足以检查关键机械约束；pointwise 独立判定可以消除候选集和顺序影响；教师轨迹经偏好标签过滤后可以蒸馏出可靠学生。
7. 边界条件包括：高度主观或依赖领域知识的任务、工具不可执行的约束、指令严重欠指定、多轮 agent 轨迹、跨语言与跨领域迁移；这些场景下 task-derived rubric 的覆盖率和工具增强裁判的收益都需要重新验证。

Q2: 有哪些相关研究？

原论文的 Related Work 未在可见片段中完整展开，以下按 Introduction 引用、baseline 与实验设定归类，属于合理重建。
1. Holistic reward models 与 RLHF：Christiano et al. 2017、Ouyang et al. 2022、Wang et al. 2024a、Liu et al. 2026c 等工作用整体标量奖励建模人类偏好；在非可验证任务上，这类奖励容易不可靠（Hashemi et al., 2024; Lee et al., 2025），推动 rubric 化、可解释的奖励建模。
2. Rubric-based reward models：Gunjal et al. 2025、Viswanathan et al. 2025、Liu et al. 2026b 等把评价拆成标准生成与裁判判定；Rubric-RM、Rubric-ARM、CE-RM、PaTaRM（Jian et al., 2026）是论文直接对比的 rubric RM。WildChecklists（Viswanathan et al., 2025）从采样响应写 checklist，RubricHub（Li et al., 2026）从参考响应写 rubric，均属于 response-induced 或 reference-induced 的标准构造。
3. Reasoning reward models 与 LLM-as-a-judge：RM-R1（Chen et al., 2026）、RRM（Guo et al., 2026）、JudgeLRM（Chen et al., 2025）让裁判先推理再打分；成对 judge 的顺序偏差（Wang et al., 2024b; Shi et al., 2025）以及成对偏好不提供单响应绝对值的问题（Liusie et al., 2024; Liu et al., 2024b）是 ReVeR 采用 pointwise 判定的直接背景。
4. Tool-augmented judges：TIR-Judge 让裁判调用工具；ReVeR 进一步把工具调用组织成每个响应独立的判定轨迹，用 Python 解释器检查计数、格式、精确匹配等机械约束，并把工具证据纳入逐条 verdict。
5. 蒸馏与数据构造：Feedback Collection（Kim et al., 2023）和 R3（Anugraha et al., 2025）把书面反馈或蒸馏推理附着到被打分响应上；ReVeR 的区别是 label-blind distillation，偏好标签只过滤教师轨迹，学生学的是 rubric reasoning 与 tool-augmented judging，而不是直接拟合标签。
6. 策略优化与评价基准：DPO、GRPO 等用奖励模型做偏好优化或在线 RL；RewardBench、RM-Bench、IFEval、IFBench、Arena-Hard 分别覆盖偏好、指令遵循和下游策略评价。ReVeR 把 RM 准确率、rubric fidelity 和 policy utility 放在同一研究框架下比较，这是它与单一 benchmark 论文的主要差异。

Q3: 论文如何解决这个问题？

1. 形式化：给定指令 x（含上下文）和响应 y，rubric R = {(c_i, t_i)}_{i=1}^k 由自然语言标准 c_i 组成，每条标准带类型 t_i，取值为 hard rule 或 principle。生成器 G_θ 先写推理 z 再写 rubric；裁判 J_φ 带 Python 解释器，产生轨迹 τ，最终对每条标准给出一个 verdict；聚合器 S 把 verdict 分数 q(τ)=(s_1,...,s_k) 映射为奖励 r(x,y)=S(q(τ);R)。
2. Prompt-level rubric：R 只依赖 x，因此每个 prompt 只生成一个 rubric，被该 prompt 下所有响应共享；这保证候选之间用同一把尺子，减少候选集构成对标准的影响。
3. Pointwise verdict：τ 只依赖 (x,y,R)，因此 r(x,y) 对“还有哪些其他响应”以及“响应以什么顺序呈现”不变；这与成对 judge 的相对偏好和顺序偏差形成对照，也使奖励可直接用于 best-of-N、DPO 配对和 GRPO。
4. Task-derived rubric generation：生成器仅以指令为条件，按 p_θ(z,R|x)=p_θ(z|x)p_θ(R|x,z) 分解，先推理任务再写标准。推理 z 要识别用户想要的交付物、显式要求及其约束强度、称职读者会推断出的隐含期望，以及响应可能失败的方式；随后把要求写成 hard rule 或 principle。类型决定该标准如何计分与排序，但不决定检查方式，因此 hard rule 也可以由阅读或程序验证。
5. Tool-augmented judging：裁判每个 episode 只评一个响应，轨迹由多轮工具调用组成，例如写一段 Python、执行、观察输出，再给出该标准的 verdict；对字符计数、格式、精确短语等约束，程序证据比纯阅读更可靠。
6. 聚合：从图表与案例看，hard rule 充当门控或优先级约束，principle 提供分级比较并在接近候选间打破平局；案例中 ReVeR 给 A/B 的分数为 3.10 与 2.00，说明聚合不是简单二值，而是逐条 verdict 的细粒度加权。聚合器 S 的完整公式在可见片段中未展开，需回原文核对。
7. Label-blind distillation：教师生成 rubric reasoning 与 judging trajectory；偏好标签只用于过滤教师输出（例如保留与偏好方向一致的轨迹），不进入学生输入或监督目标。生成器与裁判都从过滤后的轨迹中蒸馏，形成 ReVeR-Traces 大规模数据集。这样既利用偏好信号，又避免把“这对响应的标签”直接写进 rubric。
8. 训练与推理：ReVeR 以 Qwen3.5-4B 为底座（表注称其为 untrained base），裁判调用 Python；推理时可用多次判定取均值（mj@5），在 best-of-N 中用 hard rule 先筛、principle 再排序，在策略训练中作为 DPO 配对构造器或 GRPO 奖励。

Q4: 论文做了哪些实验？

1. 研究问题：论文围绕三件事组织实验：ReVeR 在偏好对上能否把 preferred 排在 rejected 之上（§4.1）；它的奖励能否服务 best-of-N 选择与策略训练（§4.2）；它生成的 rubric 与专家 rubric 有多接近（§4.3）。
2. 偏好基准：Table 2 在 RewardBench 子集、RM-Bench 以及基于 IFEval/IFBench 构造的 IF 偏好对上报告准确率，共 11 列并给出平均；IF pairs 专门覆盖指令遵循中的机械约束。
3. Baselines：推理奖励模型 RM-R1、RRM、JudgeLRM；rubric 奖励模型 Rubric-RM、Rubric-ARM、CE-RM、PaTaRM；工具增强裁判 TIR-Judge。baseline 数值优先取原论文，缺失时用 released model 复算；具体版本与 prompt 在附录 B/F。
4. 判定协议：对每个偏好对，ReVeR 在同一 rubric 下把两个响应分别放进独立 episode 打分，分数高者胜；平局和无效判定都算错。mj@5 表示每个响应判五次取平均分再比较。未训练 Qwen3.5-4B 作为同协议底座对照。
5. 测试时扩展与 best-of-N：用 benchmark 自带 checker 作为上界，比较不同候选数 N 下 ReVeR 与 baseline 的选择质量；可见片段显示 ReVeR 获得 67%/55% 的 oracle gain，最强 baseline 为 41%/30%，且四候选时已超过所有 baseline 的十六候选结果（具体任务与配置需回表核对）。
6. 策略训练：从 Qwen2.5-7B-Instruct 出发，用 DPO 和 GRPO 训练策略；ReVeR 既用于构造 DPO 偏好对，也作为 GRPO 奖励。对比中包括 Rubric-ARM-8B 奖励；评价涉及 Arena-Hard 与指令遵循/合规指标。片段显示 Rubric-ARM-8B 策略在 Arena-Hard 上可达 62.0 的某指标，但偏好与合规出现分离，说明高偏好分不一定等于高约束满足。
7. Rubric fidelity：把 ReVeR 生成的 rubric 与专家 rubric 对齐，统计匹配、覆盖/召回等指标；并做生成器 RL 前后对比，观察 hard rule 与 principle 的数量和匹配率变化。
8. 案例研究：指令要求推荐一所大学，并确保字母 p 至少出现 15 次。两个响应都在追问偏好、没有点名大学；A 含 16 个 p，B 含 13 个 p，专家标签为 A。ReVeR 与 Rubric-ARM 都写出了“至少 15 个 p”的 hard rule；ReVeR 对每个响应单独跑 count('p')，得到 16 与 13，分数 3.10/2.00，6/6 次都选 A；Rubric-ARM 成对阅读时，在 (A,B) 顺序下称只有 B 满足 gatekeeper，在 (B,A) 顺序下称两者都不满足且 B 更接近，3/3 次都选 B。
9. 消融与附录：可见片段可确认的对照包括未训练底座、生成器 RL、工具增强与 hard-rule gate；更细的消融、训练超参、数据规模和 prompt 需看附录 B/D/F。

Q5: 发现了什么实验现象？

1. 偏好准确率整体占优，指令遵循最明显。ReVeR 在 11 列平均上优于成对 rubric RM，IF 子集收益最大；这与工具增强裁判能执行字符计数、格式和精确约束一致。纯阅读 judge 在这些约束上容易出现“看起来满足”的误判。
2. 测试时扩展斜率更陡。可见片段显示 ReVeR 获得 67%/55% 的 oracle gain，最强 baseline 为 41%/30%；四候选时已超过所有 baseline 的十六候选。合理推断是：prompt-level 共享 rubric 降低候选间标准漂移，pointwise 独立判定避免顺序偏差，重复判定（mj@5）进一步降低方差；但具体机制仍需看消融。
3. 下游策略并非偏好准确率的单调函数。ReVeR 在 DPO/GRPO 下游策略优化上整体优于对比，但论文明确报告：更高偏好准确率不一定带来更好策略；在 Rubric-ARM-8B 作为 GRPO 奖励时，策略可达到 Arena-Hard 62.0 的某指标，却出现偏好与合规分离。这说明奖励模型可能被策略利用，或偏好 benchmark 与真实约束满足之间存在分布差。
4. 生成器 RL 存在准确率—保真度权衡。生成器 RL 提高偏好准确率，但生成的 rubric 变得更多、与专家 rubric 匹配更少，rubric fidelity 下降；hard rule 的匹配率相对稳定，召回下降主要来自 hard rule（片段同时提到 hard rule 匹配稳定与召回下降，二者可能是 precision/recall 不同口径，需回附录 D 核对）。这意味着用偏好信号继续优化生成器，可能让标准更“可区分”却更不“像任务要求”。
5. 案例揭示成对阅读的系统性失败。字母 p 案例中，Rubric-ARM 在两种顺序下都选 B：一种顺序误判 B 满足 gatekeeper，另一种顺序虽承认两者都不满足却以“更接近 15”选 B；顺序改变理由但不改变错误偏好。ReVeR 的程序证据把 16 与 13 的差异变成可复核事实，6/6 次稳定选 A。这是 tool augmentation 与 pointwise independence 的联合收益，而不只是“模型更大”。
6. 严格计分放大难度。平局和无效判定都算错，可能压低绝对准确率；但这也迫使模型给出可比较的逐条 verdict，而不是靠语言风格或长度取胜。
7. hard rule 与 principle 的行为不同。hard rule 更适合门控和机械验证，稳定性较高；principle 提供分级与 tie-break，但在 RL 后容易数量膨胀、匹配率下降。聚合时若 hard rule 召回不足，可能直接把本应满足约束的响应错误过滤；若 principle 权重过大，又可能让风格/长度因素盖过硬约束。
8. 评价维度彼此分离。rubric fidelity 衡量标准是否忠实覆盖任务与专家标准；preference accuracy 衡量偏好对排序；downstream utility 衡量奖励是否训练出更好的策略。论文的核心经验发现是三者不能互相替代：高 fidelity 不一定高准确率，高准确率不一定高 utility。

Q6: 有什么可以进一步探索的点？

1. 多目标奖励建模：把 rubric fidelity、preference accuracy 与 downstream utility 放进同一个训练/选择目标，而不是先最大化准确率再事后检查保真度；可以探索约束优化、Pareto 选择、以及用下游策略收益做奖励模型选择。
2. 更可靠的 rubric fidelity 度量：当前专家 rubric 对齐可能低估合法但不同表述的标准；需要研究语义匹配、覆盖度、hard-rule recall、原则粒度、标准冗余与冲突检测，并区分 precision 与 recall 的口径。
3. 抑制生成器 RL 的 rubric drift：在 RL 中加入保真度正则、专家 rubric 检索、hard-rule 覆盖约束、课程学习或拒绝采样；研究为什么 RL 会让原则变多、匹配变差，以及如何保持任务语义。
4. 扩展工具增强裁判：除 Python 外接入检索、计算器、单元测试、浏览器、数据库、仿真器；处理工具错误、超时、不可执行约束和多工具组合；让裁判学会判断“工具输出是否足以支持 verdict”，而不是机械调用。
5. 聚合与校准：学习逐条 verdict 到标量奖励的聚合器，建模不确定性、冲突与平局；研究 hard-rule gate 的软硬程度、principle 权重、奖励尺度对 DPO/GRPO 的影响；避免奖励被长度、风格或格式 exploit。
6. 标签盲蒸馏的改进：教师选择、过滤策略、去偏、主动学习、过程监督；研究偏好标签只做过滤时，标签噪声和教师错误如何传播；能否用少量专家 rubric 或可验证检查增强蒸馏。
7. 下游感知的 RM 训练：直接用策略优化反馈训练奖励模型，或联合训练 reward 与 policy；研究 reward hacking、合规约束、KL 正则、GRPO 方差、DPO 配对构造对最终策略的影响。
8. 跨领域与多模态：把 task-derived rubric 推广到科学实验、生物信息、代码、数学、多轮 agent 轨迹和多模态输出；在 ai-for-science 场景中，rubric 可结合领域专家标准与可执行验证，但需要解决领域知识覆盖和工具接口问题。
9. 基准设计：建立能同时测量 rubric fidelity、preference accuracy、downstream utility 的 benchmark；加入对抗性机械约束、顺序扰动、候选集分布偏移、跨模型泛化与污染检测。
10. 理论研究：分析 response-induced rubric 的偏差来源、task-derived rubric 的充分条件、pointwise 与 pairwise 的样本复杂度/方差差异，以及何时“更可区分”的 rubric 会损害下游效用。

Q7: 总结一下论文的主要内容

论文的主线可以概括为：把 rubric 奖励模型从“从响应反推标准、成对比较”改成“从任务生成标准、逐响应取证”，并把评价从单一偏好准确率扩展为 rubric 保真度、偏好准确率与下游效用三个维度。
开头的问题诊断很清晰：在非可验证任务上，传统 holistic scalar RM 不可靠；rubric RM 用显式标准提高可解释性，但现有方法为了让 rubric 有区分度，常让教师对比偏好/拒绝响应，或分析多个候选的差异与失败模式，再据此写标准。这样得到的标准会吸收候选集的偶然特征，产生 response-induced bias；学生生成器虽然在训练集上学会从指令预测 rubric，但标准分布已经被候选响应塑形，换一批响应或换一个策略后未必稳定。裁判侧同样有问题：纯阅读 judge 难以执行字符计数、格式、精确短语等机械约束；成对 judge 只给相对偏好，受顺序偏差影响，也不能直接给单个响应打分，因此不适合 best-of-N、DPO 配对和 GRPO 奖励。
ReVeR 的解法是把两个环节解耦。生成器只看指令 x，先写推理 z，再写 rubric R；推理要识别交付物、显式要求及其约束强度、隐含期望和失败模式，标准分为 hard rule 与 principle 两类。hard rule 是可机械检查或明确门控的约束，principle 是分级评价原则；类型决定计分与优先级，但不决定检查方式。裁判 J 对每个响应单独开 episode，带 Python 解释器，沿工具调用轨迹 τ 逐条给出 verdict；聚合器 S 把逐条分数 q(τ) 映射成奖励 r(x,y)。因为 R 只依赖 x、τ 只依赖 (x,y,R)，奖励对候选集构成和呈现顺序不变，这是 pointwise 设计的核心优势。
训练上，论文提出 label-blind distillation：教师生成 rubric reasoning 与 tool-augmented judging trajectory，偏好标签只用于过滤教师输出，不进入学生输入或监督目标；生成器与裁判都从过滤后的轨迹蒸馏，形成 ReVeR-Traces。这样既利用偏好信号，又避免把“这对响应的标签”直接写进 rubric，降低标签诱导的 response bias。ReVeR 以 Qwen3.5-4B 为底座，推理时可多次判定取均值（mj@5），在 best-of-N 中用 hard rule 先筛、principle 再排序，在策略训练中用于构造 DPO 对或作为 GRPO 奖励。
实验分三条线。第一条是偏好基准：在 RewardBench 子集、RM-Bench 和基于 IFEval/IFBench 的 IF 偏好对上，共 11 列报告准确率，对比 RM-R1、RRM、JudgeLRM、Rubric-RM、Rubric-ARM、CE-RM、PaTaRM、TIR-Judge；协议是两个响应在同一 rubric 下分别独立打分，高者胜，平局/无效算错。ReVeR 在平均准确率上优于成对 rubric RM，指令遵循子集收益最明显，说明工具取证对机械约束有效。第二条是测试时扩展与策略优化：best-of-N 选择中，ReVeR 获得 67%/55% 的 oracle gain，最强 baseline 为 41%/30%，四候选时已超过所有 baseline 的十六候选；下游从 Qwen2.5-7B-Instruct 出发做 DPO/GRPO，ReVeR 奖励整体优于对比，但 Rubric-ARM-8B 在 GRPO 下可达 Arena-Hard 62.0 的某指标却出现偏好与合规分离，提示高偏好分不等于高约束满足。第三条是 rubric fidelity：把生成 rubric 与专家 rubric 对齐，发现生成器 RL 会提高偏好准确率却降低保真度，rubric 变得更多、与专家标准匹配更少；hard rule 匹配相对稳定，召回下降主要来自 hard rule（片段中 precision/recall 口径需回附录 D 核对）。
论文最值得记住的不是某个单点 SOTA，而是三个经验命题。其一，rubric 的来源决定偏差结构：从响应反推标准会得到“能区分当前候选”的标准，从任务生成标准才更可能“忠实于任务”。其二，裁判的形态决定可用性：成对 judge 给相对偏好，pointwise + 工具给绝对、可复核、顺序不变的分数，后者更适合策略优化。其三，评价指标不可互相替代：rubric fidelity、preference accuracy、downstream utility 是不同属性；生成器 RL 可以在准确率上升的同时损害保真度，高准确率 RM 也可以训练出高偏好但低合规的策略。
论文的边界也很清楚。工具增强主要覆盖可执行约束，主观质量、领域知识和多轮 agent 轨迹仍依赖语言判断；task-derived rubric 在指令欠指定时可能覆盖不足；label-blind distillation 仍受教师质量和偏好标签过滤策略影响；rubric fidelity 的专家对齐度量可能惩罚合法但不同表述的标准；下游策略实验受奖励尺度、长度/风格、KL 正则和训练配方影响。作者在结论中把 ReVeR 与 ReVeR-Traces 作为两个贡献，前者是方法，后者是可复用的 rubric reasoning 与 tool-augmented judging 轨迹数据；这也为后续研究留下了明确接口：改进 rubric 生成、改进工具裁判、改进聚合与下游训练，分别对应不同的失败模式。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与 agent 方向直接相关：ReVeR 的 tool-augmented judge 本质上是把裁判做成会调用工具、按轨迹取证的 agent；其 pointwise 轨迹、工具错误处理和逐条 verdict 可迁移到 agent 轨迹评价、工具使用合规和任务完成度评估。

## 基本信息

- 作者：Tianci Liu, Haoyu Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-13
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18766v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Method、Results、Conclusion 片段，并结合摘要与启发式草稿；对检索片段未覆盖的公式、表格数值和附录细节保留了证据缺口标注。
