---
user_id: "cheng tan"
paper_id: 14943
arxiv_id: "2610.12002v1"
title: "Agentic-TTT: Training test-time policy for test-time training"
publish_date: "2026-10-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.12002v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.12002v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T12:47:27"
---
# Agentic-TTT: Training test-time policy for test-time training

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：test-time training · test-time policy · self-improvement · llm agent

## 一句话总结

本文提出 Agentic-TTT：学习一个测试时策略（test-time policy）来管理何时调用何种 TTT 方法、是否构建或复用参数空间技能，把 TTT 从固定适应过程重构为可学习的决策过程，在基准上使效用相对直接推理几乎翻倍，并能权衡计算与效用、泛化到训练未见领域。

## 摘要

> Test-time training (TTT) adapts an LLM's parameters using signals derived from test inputs, and can make striking improvements in pre-specified settings such as IMO competitions or designated open problems. By turning deployment experience into parameter updates, TTT provides a direct mechanism for model-level self-improvement. Yet TTT is not universally beneficial: each TTT algorithm works in different settings, and applying an ill-suited method could waste test-time compute or even damage model performance. Therefore, such parameter-level self-improvement requires agency: the model must decide when TTT is warranted, which algorithm to invoke, and whether an existing skill can be reused. To fill this gap, we introduce Agentic-TTT, which learns a test-time policy to govern those decisions. Agentic-TTT turns TTT procedures into callable tools, treats accumulated skills as an evolving deployment environment, and trains its policy using the observed utility gains from its decisions. On our benchmark, Agentic-TTT nearly doubles the utility over the backbone model, learns to trade off utility against compute, and generalizes to domains unseen during training. Together, these results point toward autonomous self-improvement: models that can decide how to learn from their own deployment experience.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在开放式 LLM 部署中，模型如何自主决定是否以及如何进行参数级自我改进（test-time training, TTT）。具体而言，TTT 通过测试输入信号更新模型参数，把部署经验转化为参数更新，在预设场景（如 IMO 竞赛、指定开放问题）中能带来显著提升。然而 TTT 并非普遍有益：不同 TTT 算法适用于不同设置，选错方法会浪费测试时计算甚至损害模型性能。因此，参数级自我改进需要 agency——模型必须自己判断：当前查询是否需要 TTT？如果需要，该调用哪个 TTT 算法？是否已有可复用的技能？是否应该构建新技能？以及如何在效用与计算成本之间取舍。

论文进一步指出，现有 self-improving agents 的改进大多停留在 scaffold 层面：通过 reflection 和 memory 把历史交互蒸馏为文本经验，通过 prompt 演化、agentic workflow 搜索、可执行技能积累来提升系统整体能力，但模型参数本身保持不变。TTT 方法（如 POT、TTT-FS、TTRL、SEAL、TLM）虽然能更新参数，但每个方法都是固定的适应过程，缺乏高层决策机制。作者认为，模型级 agency over TTT——即由模型自己决定如何从部署经验中学习——在开放式 LLM 部署中尚未被探索。

这个问题的难点在于：(1) 决策空间大且异构：方法选择、技能构建/复用、直接回答等多种动作类型混合，且动作效果高度依赖查询和领域；(2) 训练信号稀疏且延迟：效用增益只有在完整执行 TTT 并验证答案后才能获得，难以直接监督每一步决策；(3) 计算-效用权衡：TTT 消耗测试时计算，策略需要学会在有限预算下分配资源；(4) 泛化要求高：策略必须在训练未见的领域和模型上仍然有效；(5) 技能库是动态演化的环境：技能构建成本需要跨查询摊销，策略需要感知技能库状态并决定何时加载、何时新建。

Agentic-TTT 的解决思路是：把 TTT 过程封装为可调用工具，把累积的参数空间技能视为演化的部署环境，训练一个测试时策略来管理这些决策，并用决策带来的效用增益作为训练信号。这一设定把 TTT 从“固定适应过程”重构为“可学习的决策过程”，是本文的核心问题定位。

Q2: 有哪些相关研究？

相关研究可以从三条线索梳理。

第一条线索是“自我修改与自我改进”的思想史。论文开篇引用 von Neumann 1945 年提出的存储程序架构，使程序能够基于运行时数据改写自身指令；Turing 进一步指出“让机器修改自身指令提供了从经验中学习的机制”。这一思想在当代神经网络中并未完全实现：模型部署后参数通常冻结，部署数据只能影响计算而不能改变参数化能力。现代 self-improving agents 在系统层面延续了这一愿景：Reflexion（Shinn et al., 2023）和 Zhao et al. (2024) 通过反思与记忆把历史交互蒸馏为文本经验；Fernando et al. (2024) 演化 prompt；Zhang et al. (2025a) 搜索 agentic workflow；Wang et al. (2024) 积累可执行技能。这些工作的共同点是：改进发生在围绕模型的 scaffold（上下文、控制流、外部动作库）中，模型参数本身不变。Agentic-TTT 则把自我改进推进到参数层面，同时引入高层决策。

第二条线索是 test-time training（TTT）方法本身。论文实验涉及五种代表性方法：POT（Jiao et al., 2026）、TTT-FS（Akyürek et al., 2025）、TTRL（Zuo et al., 2025）、SEAL（Zweiger et al., 2025）和 TLM（Hu et al., 2025a）。从证据片段看，TTT-FS 从 few-shot demonstrations 适应，TTRL 则跨多个同域查询聚合 self-consistency 信号，属于多查询方法；其他方法的具体机制在可见文本中未展开。这些方法各有适用场景，但没有一个在所有设置下普遍有效——这正是 Agentic-TTT 要解决的痛点。值得注意的是，多查询方法（如 TTRL）需要环境维护跨查询的状态，这促使作者把技能库设计为演化的部署环境。

第三条线索是训练测试时策略所用的强化学习方法。论文采用 Dr. GRPO（Liu et al., 2025b）风格的 advantage 估计（mean-centered advantage，省略组标准差归一化），并设置调用惩罚系数 λinv=0.15。这与近期 LLM 推理时 RL 训练（如 GRPO 系列）一脉相承，但应用对象从“生成答案”变为“管理 TTT 决策”。

此外，相关工作还隐含地与 meta-learning（学习如何学习）、test-time compute scaling（测试时计算分配）和 tool use（把 TTT 过程工具化）交叉。Agentic-TTT 的独特之处在于把“选择学习算法”本身作为学习目标，而不是在固定 TTT 流程内优化超参数。

Q3: 论文如何解决这个问题？

Agentic-TTT 的核心是一个测试时策略（test-time policy），它管理三类决策：是否调用 TTT、调用哪个 TTT 方法、是否构建或复用参数空间技能。整体架构由策略、harness、技能库和 TTT 工具组成。

工具化 TTT：论文把五种 TTT 过程（POT、TTT-FS、TTRL、SEAL、TLM）封装为可调用工具。策略通过结构化动作（ADAPT action block）发出调用请求，harness 解析动作块、检索所需内容并执行 TTT。策略可以为每个方法提供 method-specific payloads，即方法相关的参数或上下文。这种设计使策略的动作空间统一为“工具调用”，与 LLM agent 的 tool use 范式一致。

技能库作为演化部署环境：累积的参数空间技能（以 LoRA 形式）构成一个动态环境。技能构建需要额外的训练计算，但加载已有技能不需要额外适应，因此构建成本可以跨后续查询摊销。策略需要感知技能库状态：如果某个技能已在当前轨迹中构建，可以直接加载；如果尚未构建，则需要权衡“现在构建”的额外训练计算与预期收益。证据片段显示，当技能未构建且预期收益有限时，策略的目标是直接用基础模型回答（Direct Answer），而不是盲目构建技能。为了支持多查询 TTT 方法（如 TTRL 聚合 self-consistency 信号），环境还维护跨同域查询的状态。

策略训练：训练分为两个阶段。第一阶段是 guidance distillation：使用 2,072 个 turn-level 示例（来自 177 条轨迹）来蒸馏初始策略行为，让策略学会基本的调用和技能管理模式。第二阶段是强化学习：使用 187 个训练输入组成的流，以决策带来的效用增益为奖励训练策略。RL 采用 Dr. GRPO 风格的 mean-centered advantage，并设置 λinv=0.15 作为调用惩罚（合理推断：用于抑制不必要的 TTT 调用），省略组标准差归一化以保留信号尺度。此外，实验通过 λcomp 控制计算成本惩罚，使策略能在效用与计算之间权衡。

推理与解耦：评估时，策略和 TTT 技能使用独立的 LoRA；最终答案解码时禁用策略 LoRA，只接收原始查询，确保答案生成不被策略参数污染。策略和答案解码都使用 greedy 解码。这种解耦设计让策略只负责“如何学习”，而答案生成仍由 backbone 完成。

整体上，Agentic-TTT 把 TTT 从“固定适应过程”重构为“可学习的决策过程”：策略在部署时观察查询和技能库状态，决定是否以及如何更新参数，然后执行 TTT 或直接回答，并根据结果效用更新自身。

Q4: 论文做了哪些实验？

实验设计围绕三个问题：Agentic-TTT 能否通过 learned agency 实现自我改进？能否控制推理时计算？策略能否迁移到训练分布之外？

数据与评估：数据集覆盖 8 个域，来自 11 个 benchmark 家族。Guidance distillation 使用 2,072 个 turn-level 示例（来自 177 条轨迹）；强化学习使用 187 个训练输入组成的流。评估采用固定的顺序流，包含 176 个 held-out 计分查询，从空技能库开始；技能库状态在训练和评估运行之间不共享。总体效用（utility）是任务特定自动验证器给出的平均二值分数。附录 B 详述数据来源、划分、流构造和评分规则。

实现：默认 backbone 为 Qwen2.5-7B-Instruct，集成五种 TTT 方法：POT、TTT-FS、TTRL、SEAL、TLM。策略和 TTT 技能使用独立的 LoRA；最终答案解码禁用策略 LoRA，只接收原始查询。策略和答案解码在评估时均为 greedy。训练策略报告三次独立训练运行的均值和样本标准差，每个 checkpoint 评估一次。Harness 和优化细节在附录 C 和 D。

主实验（3.1）：在相同 backbone 下比较所有方法。核心指标是总体效用。摘要和结论声称 Agentic-TTT 相对直接推理几乎翻倍效用，且没有固定 TTT 方法普遍有效。具体对比包括：直接回答（Direct Answer）、每种 TTT 方法单独使用、以及 Agentic-TTT 的 learned policy。

计算-效用权衡：通过改变计算惩罚系数 λcomp（证据中出现 0、0.10、0.20、0.40 等值）绘制 cost-gain profile，观察策略如何在效用与 TTT 调用次数之间取舍。同时统计各方法的调用次数（method invocations），例如 TTRL 的调用次数从 21.3 降至 11.7，TLM 略有增加。

跨模型泛化：将训练好的策略迁移到其他 backbone（Qwen-7B、Llama-8B、GLM-9B），对比“仅 prompting”与 Agentic-TTT。证据显示效用分别提升 0.254、0.136、0.142，显著优于仅 prompting，但增益是模型相关的：Llama 在部分指标上退化（原文截断，具体退化维度需查原文）。

技能操作分析：评估中记录方法路由和技能操作（如 Direct Answer、Math 技能加载/构建）。证据显示加载技能无需额外适应，构建成本可跨查询摊销；大多数技能相对 Direct Answer 无害，仅有一个技能有害（原文表述为“harm only one relative to Direct Answer”，合理推断为多数技能不损害、个别技能有负面效果）。

未见领域泛化：评估流包含训练未见的域，Agentic-TTT 在这些域上仍能保持效用提升，说明策略学到的是跨域的决策原则而非过拟合特定域。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：

1. 没有固定 TTT 方法普遍有效。这是 Agentic-TTT 成立的前提：不同 TTT 方法在不同设置下各有优劣，单一方法无法在所有查询上稳定超越直接回答。这一观察直接支持“需要 agency 来选择方法”的论点。

2. Agentic-TTT 学会收益感知的方法分配，而非统一抑制 TTT。证据显示，策略选择性地减少高成本、低预期收益的方法：TTRL 调用次数近乎减半（从 21.3 降至 11.7），而 TLM 调用次数略有增加。这说明策略不是简单地少用 TTT，而是根据方法特性和查询情境重新分配计算资源。

3. 技能加载零额外适应成本，构建成本可摊销。加载已有技能不需要额外的 TTT 适应，因此技能构建成本可以在后续查询中分摊。这解释了为什么策略愿意在早期构建技能：虽然单次构建代价高，但多次复用后净收益为正。

4. 跨模型迁移有效但增益模型相关。在 Qwen-7B 上效用提升 0.254，GLM-9B 上 0.142，Llama-8B 上 0.136，均显著优于仅 prompting。但 Llama 在部分指标上出现退化（原文截断，具体退化维度需查原文），说明 learned TTT agency 的迁移效果与 backbone 的 TTT 兼容性有关。

5. 计算-效用权衡可被显式控制。通过调整 λcomp，策略在效用与 TTT 调用成本之间产生清晰的 cost-gain profile：λcomp 越大，策略越倾向于少调用 TTT、多用直接回答，效用相应下降但计算开销减少。这表明策略学到的不是单一行为，而是可调节的权衡曲面。

6. 未见领域泛化成立。在训练未见的域上，Agentic-TTT 仍能保持效用提升，说明策略学到的是“何时学习、如何学习”的通用原则，而非对特定 benchmark 的过拟合。

7. 技能操作中 Direct Answer 仍是重要基线。技能操作分析显示 Direct Answer 与 Math 等技能并存，且大多数技能相对 Direct Answer 无害、仅个别有害。这提示策略学会了保守的技能使用策略：只在预期收益明确时才偏离直接回答。

8. 反直觉点：多查询方法（TTRL）被大幅削减。TTRL 聚合 self-consistency 信号理论上更稳健，但其多查询开销使其在计算预算约束下成为首要削减对象，说明“更稳健”不等于“更划算”。

Q6: 有什么可以进一步探索的点？

基于本文的设定和结果，可以进一步探索的方向包括：

1. 更大规模与更广方法覆盖：当前实验限于 7B-9B 级模型和五种 TTT 方法。扩展到更大模型（如 70B 级）和更多 TTT 变体（在线 RL、记忆增强、全参数微调等）可以检验 learned agency 的规模扩展性。

2. 策略自身的在线持续学习：当前策略在训练后固定，技能库在部署中演化。一个自然延伸是让策略本身也随部署经验持续更新，形成“策略-技能”双层演化，但需要解决稳定性与灾难性遗忘问题。

3. 更丰富的技能表示与组合：技能目前以 LoRA 形式存在。未来可探索模块化技能组合（如多个 LoRA 的加权融合）、技能继承与分化、以及技能间的冲突消解，使技能库真正成为可演化的知识资产。

4. 细粒度计算预算控制：λcomp 目前是全局标量。可探索按查询难度、领域先验或用户预算动态调节计算分配，甚至让策略输出连续的计算预算而非离散的方法选择。

5. 训练信号的改进：当前依赖任务特定自动验证器提供二值效用。在开放任务中可引入 LLM-as-judge、人类反馈或可学习奖励模型，并研究稀疏延迟奖励下的 credit assignment。

6. 安全性与对齐：模型自主决定如何更新自身参数带来新的安全风险——策略可能学会利用验证器漏洞或产生不可逆的破坏性更新。需要研究约束下的 TTT agency、可回滚的参数更新、以及策略行为的可解释性。

7. 理论分析：何时 TTT 有益？效用增益的可识别性如何？能否给出 TTT 方法选择的信息论或决策论刻画？这些理论问题有助于指导策略架构设计。

8. 与 scaffold 级自我改进的联合：将参数级 agency（本文）与系统级 scaffold（reflection、memory、workflow 搜索）结合，形成完整的自主自我改进循环，并研究两者之间的协同与冲突。

9. 技能库的长期管理：随着部署时间增长，技能库会膨胀、过时、冲突。需要研究技能遗忘、去重、质量评估和自动淘汰机制，以及技能库状态的高效表征。

10. 跨领域迁移的机制分析：本文观察到未见域泛化，但未解释其机制。未来可分析策略到底依赖哪些特征做决策（查询文本、技能库状态、历史效用），从而设计更可解释、更鲁棒的决策策略。

Q7: 总结一下论文的主要内容

本文提出 Agentic-TTT，目标是让 LLM 在部署后能够自主决定如何从自身经验中学习，即实现参数级自我改进的 agency。论文从思想史切入：von Neumann 的存储程序架构和 Turing 关于“机器修改自身指令以从经验中学习”的论述，为自我改进提供了早期蓝图；但当代神经网络部署后参数冻结，部署数据只能影响计算而不能改变参数化能力。现代 self-improving agents 通过 reflection、memory、prompt 演化、workflow 搜索和技能积累在 scaffold 层面改进系统，但模型参数本身不变。

TTT 提供了一条参数级路径：用测试输入信号更新参数，把部署经验转化为参数更新。TTT 在预设场景（如 IMO 竞赛）中效果显著，但并非普遍有益：不同 TTT 算法适用于不同设置，选错方法会浪费测试时计算甚至损害性能。因此，作者提出需要模型级 agency：模型必须决定何时 TTT 值得、调用哪个算法、是否复用已有技能。这一空白此前未被探索。

技术路线上，Agentic-TTT 把五种 TTT 方法（POT、TTT-FS、TTRL、SEAL、TLM）封装为可调用工具，把累积的参数空间技能（LoRA）视为演化的部署环境，并训练一个测试时策略来管理决策。策略通过结构化动作（ADAPT action block）调用 TTT，harness 负责解析和执行；技能库支持跨查询复用，构建成本可摊销。训练分两阶段：先通过 guidance distillation（2,072 个 turn-level 示例、177 条轨迹）初始化策略，再用强化学习（187 个训练输入）以效用增益为奖励优化策略，采用 Dr. GRPO 风格的 mean-centered advantage，并设置调用惩罚 λinv=0.15。推理时策略 LoRA 与答案解码解耦，最终答案只由 backbone 生成。

实验覆盖 8 个域、11 个 benchmark 家族，评估流包含 176 个 held-out 查询，从空技能库开始。主实验表明：没有固定 TTT 方法普遍有效，而 Agentic-TTT 相对直接推理几乎翻倍效用；策略学会收益感知的方法分配（TTRL 调用从 21.3 降至 11.7，TLM 略增）；通过 λcomp 可显式控制计算-效用权衡；策略迁移到 Qwen-7B、Llama-8B、GLM-9B 时分别提升 0.254、0.136、0.142，优于仅 prompting，但增益模型相关（Llama 有退化）；在训练未见领域上仍保持效用提升。技能操作分析显示加载技能无需额外适应，构建成本可跨查询摊销，且大多数技能相对 Direct Answer 无害。

结论上，本文把 TTT 从固定适应过程重构为可学习的决策过程，论证了“模型能够决定如何从自身部署经验中学习”这一自主自我改进愿景的可行性。局限在于：实验规模有限（7B-9B 模型、五种 TTT 方法）、效用依赖自动验证器、跨模型增益不一致、以及自主参数更新的安全性未充分讨论。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）直接重合：本文研究模型级自我改进、测试时决策和工具化 TTT，属于 agent 自主性的前沿问题。

## 基本信息

- 作者：Jiahao Lu, Mohan Kankanhalli
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-08
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.12002v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要、Introduction/Results/Conclusion 片段及 PDF 语义检索命中的证据；部分方法细节和实验设置（如 λinv 作用、Llama 退化维度）因原文缺失属于合理推断，需回原文确认。
