---
user_id: "cheng tan"
paper_id: 14416
arxiv_id: "2609.38025v1"
title: "Dr. OPD: Learning What to Follow for Optimal On-Policy Distillation of Large Language Models"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.38025v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38025v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/reading_reports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-08T23:41:12"
---
# Dr. OPD: Learning What to Follow for Optimal On-Policy Distillation of Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · token-level supervision · bilevel optimization · large language models

## 一句话总结

提出 Dr. OPD，把大模型 on-policy 蒸馏建模为双层优化：下层学生用带 token 权重的教师监督训练，上层根据学生最终奖励选择权重，并通过每轮闭式更新权重加一步加权 OPD 梯度更新的交替求解器实现，使强到弱与同尺寸蒸馏在数学、代码任务上一致超过 vanilla OPD，强到弱设置下还能让学生反超教师。

## 摘要

> On-policy distillation (OPD) trains a student on its own generated responses using dense, token-level supervision from a stronger teacher. Vanilla OPD treats all teacher signals equally, assuming that the teacher's supervision is equally important for every token. However, teacher signals at different tokens may have very different effects on the student's performance: some correct important reasoning errors, while others have little effect on the final answer. Motivated by this observation, we introduce Dr. OPD (OPD Done Right), which defines the optimal weighted OPD to maximize the student's performance. We formulate Dr. OPD as a bilevel optimization problem in which the student learns from weighted teacher supervision, while the weights are selected to maximize the expected reward of the resulting student. To solve Dr. OPD, we develop an efficient iterative solver that updates the token weights and student policy alternatively. At each round, it updates weights in closed form and then takes one gradient step on the resulting weighted OPD objective. Under regularity conditions, we show that this weighted update achieves a higher expected reward than a vanilla OPD update. Empirically, across strong-to-weak and same-size distillation on math and code, Dr. OPD consistently outperforms all evaluated baselines. In particular, in the strong-to-weak distillation setting, Dr. OPD improves average math performance by $9.7$ points over vanilla OPD, and enables the smaller student to surpass its larger teacher.

Q1: 这篇论文试图解决什么问题？

这篇论文要解决的核心问题是：on-policy distillation 中教师在不同 token 上的监督信号价值不均，而 vanilla OPD 默认所有 teacher signal 同等重要，导致蒸馏目标与最终任务表现之间出现错配。具体来说，OPD 是当前大模型后训练里重要的能力迁移手段：学生模型在自己生成的响应上，逐 token 与更强的教师模型做分布对齐，因此每个生成位置都能获得学习信号，而不是像 RLOO/GRPO 那样只在整条轨迹结束后收到一个标量奖励。vanilla OPD 使用等权重，隐含的假设是教师对每个 token 的指导都一样有用。但作者用数学题的例子说明：同一个学生响应里，教师可能在某一步纠正了关键推理错误，也可能只是换了一种措辞但数学含义相同；前一种信号对最终答案影响很大，后一种几乎不影响最终结果，二者理应被赋予不同权重。由此引出两个层次的问题。第一层是目标设计问题：既然不同 token 的教师信号效用不同，应该如何重新定义加权 OPD 目标，使蒸馏优化直接服务于任务奖励最大化。第二层是求解问题：权重函数定义在所有可能的 context-token 对上，且不预设参数形式，难以直接做梯度优化；同时学生策略在训练中不断变化，某一时刻最优的权重可能很快失效，所以需要高效的迭代求解方案，并需要说明这种加权更新为什么比 vanilla OPD 更好。论文还隐含回应了一个更普遍的方法论问题：后训练阶段模仿教师与优化任务奖励之间并不天然一致，适当地选择‘模仿什么、不模仿什么’可能是比简单蒸馏更本质的机制。解决上述问题后，论文在数学推理与代码生成两个代表性任务上检验方法，目标是在强到弱迁移场景中让小模型追平甚至反超教师，在同尺寸场景中让蒸馏收益进一步逼近饱和上限。

Q2: 有哪些相关研究？

相关工作可以从四条线索来组织。第一是 on-policy distillation 本身。论文将 OPD 定位为后训练阶段从强教师向更便宜、或训练更少的学生迁移能力的重要方法，并引用了 Agarwal et al., 2024、Xu et al., 2026、Team et al., 2026 等工作；与它的对照是 outcome-reward 强化学习方法，如 Ahmadian et al., 2024 的 RLOO 和 Shao et al., 2024 的 GRPO，这些方法只使用整条轨迹的标量奖励，而 OPD 使用教师提供的逐 token 稠密监督。第二是 token 级蒸馏与分布对齐训练，代表性引用为 Gu et al., 2024，核心思想是在学生访问到的每个 token 上最小化学生与教师下一 token 分布之间的差异；本文在其基础上前进一步，认为每个 token 的蒸馏项应被重新加权。第三是非均匀教师信号/样本加权研究，论文提到 Armandpour et al., 2026、Xie et al., 2026、Xing et al. 等近期工作，它们也从不同角度指出并非教师的每个信号对学生同等有用，不过本文与它们的主要区别是把 token 权重的选择显式建模为以学生最终奖励为目标的双层优化，而不是依赖启发式或辅助模型。第四是双层优化在 LLM 对齐、数据加权和元学习中的应用：上层目标选权重、下层目标训练学生/模型，这与 reward-weighted regression、offline RL 中的 advantage weighting 在思想上有联系，但 Dr. OPD 的权重没有参数化形式，需要专门的闭式更新。实验相关数据方面，论文使用了 He et al., 2026 的 DeepMath-103K（带难度标签）和 Cui et al. 的 Eurus-RL-Code 作为训练数据，说明工作落在数学推理与代码生成的蒸馏评测生态中。需要指出，由于当前检索到的 PDF 片段有限，除上述明确出现的文献外，更完整的 baseline 列表和对比方法细节需要回到原论文的 Related Work 与实验表格确认。

Q3: 论文如何解决这个问题？

Dr. OPD 的解法分为三个层面：问题重新建模、交替求解器设计、理论保证。

1. 双层优化建模
论文把“最优加权 OPD”定义为一个 bilevel optimization。下层问题：在给定 token 权重函数 w 的条件下，学生通过最小化带权的 OPD 损失来更新自身策略，即对每个学生访问到的 context-token 对，用 w 乘以学生与教师的下一 token 分布差异，再做梯度下降。上层问题：选择权重函数 w，使得经过下层训练得到的学生 θ*(w) 在任务奖励上的期望值最大。这个公式把“模仿教师”和“提升任务表现”分离成两个目标，权重成为连接二者的控制变量。

2. 交替迭代求解器
由于权重函数定义在所有可能的 context-token 对上、且不预设参数形式，不能直接做像式 (7) 那样对权重参数的梯度更新；论文改为构造一个可处理的下界近似。求解器每轮做两步：
- 对当前学生策略 θ̄，以闭式更新 token 权重 w。从检索到的附录片段看，这一步与当前学生策略处的方向导数和奖励信号有关，例如片段中出现 bct = dt ⟨∇θ log pθ(yt | st)|θ=θ̄, br⟩ = dt d/dϵ log p_{θ̄+ϵ br}(yt|st)|ϵ=0 这类方向导数恒等式，说明权重更新本质上是在评估“如果学生沿某个更新方向移动，该 token 上的对数概率变化多少”，并据此决定该 token 的蒸馏权重。
- 固定权重后，在加权 OPD 目标上做一步标准梯度更新，得到 θ+(w)。这样避免了在非参数权重空间上做高维优化，把上层问题的求解压缩为每轮闭式权重计算。

3. 理论保证
论文在“improvement guarantee”这一节给出加权更新优于 vanilla OPD 的条件。附录 C.1 的正则条件包括：有限词表、学生和教师概率严格为正、可行权重函数是某个集合 W 中的可测函数。另一个片段给出步长相关的约束：∥θ+(w) − θ̄∥ ≤ η w_max sqrt(E[T]) M，即足够小的学生步长能保证所有响应停留在所需光滑性成立的邻域内；条件 ληLRM ≤ 1 等价于 λ ≤ 1/(ηLRM)，说明上层目标的步长与下层学生步长、奖励函数 Lipschitz 常数、模型规模等参数之间存在耦合约束。在这些条件下，论文证明加权 OPD 更新得到的期望奖励不低于 vanilla OPD 更新。

整体上，Dr. OPD 不是简单地在蒸馏损失上乘以某个注意力权重，而是把权重选择本身变成一个针对最终奖励的可证明有效的优化问题。与端到端元学习相比，它的关键简化在于每轮只用一步学生更新、权重闭式求解，因此计算开销可控。需要说明的是，由于检索到的 method 正文有限，上述关于式 (7)、方向导数和闭式权重的细节属于对附录片段与摘要的一致性重构，完整证明与算法伪代码应以原文 Method 和 Appendix 为准。

Q4: 论文做了哪些实验？

论文实验覆盖数学推理与代码生成两类任务，并设置强到弱蒸馏（strong-to-weak）与同尺寸蒸馏（same-size）两种迁移场景，所有设置均与 vanilla OPD 和“所有评估基线”对比。

已确认的实验设计细节如下：
- 数学任务使用 DeepMath-103K（He et al., 2026），该数据集提供难度标签。论文按难度分带训练学生：Qwen3-1.7B-Base 学生在 [3, 4) 难度带训练，其他学生在 [6, 10] 难度带训练。
- 代码生成任务使用 Eurus-RL-Code（Cui et al.）训练。
- 学生模型涉及 Qwen3-1.7B-Base 等；教师为学生更强或同尺寸的模型，具体规模和对应关系在检索片段中未完整展开。
- 强到弱设置中，Dr. OPD 在平均数学成绩上比 vanilla OPD 高 9.7 分，并能让学生超过其更大/更强的教师。
- 同尺寸设置中，vanilla OPD 已经能把学生带到接近甚至超过教师的水平，留给蒸馏的边际空间有限；但 Dr. OPD 仍是两种设置下平均表现最好的方法，相对 vanilla OPD 分别提高 2.3 分和 3.0 分。
- 另一个已确认的细粒度结果：对 1.7B-Base 学生，Dr. OPD 在数学上比 vanilla OPD 提高 1.8 分，在代码上提高 6.4 分，代码上的增益显著更大。

由于论文元数据中 method 与 results 正文为空，检索到的片段主要来自引言、结论和参考文献，因此完整的 baseline 名单、训练超参、奖励模型、评估指标和表格编号需要回到原文 Table 1/Table 2 等确认。目前只能确认至少包含 vanilla OPD 作为核心对照，以及论文声称的一致超越。

Q5: 发现了什么实验现象？

从已检索到的证据可以归纳出以下几类实验现象：

1. 强到弱蒸馏中方法收益最大。平均数学成绩相对 vanilla OPD 提升 9.7 分，且学生能够反超教师。这说明当学生能力明显弱于教师、模仿空间较大时，选择性加权教师信号能显著改善蒸馏效率；这可能是因为等权重蒸馏会把学生的优化预算浪费在无关紧要的措辞对齐上，而 Dr. OPD 把预算集中到与奖励相关的关键 token。

2. 代码任务上的增益比数学任务更大。对 1.7B-Base 学生，Dr. OPD 在代码上比 vanilla OPD 高 6.4 分，而在数学上只高 1.8 分。合理推断是代码生成中语法正确性、API 调用、结构约束等 token 的重要性差异更大，因此加权带来的收益也更明显；数学的奖励信号可能更加集中在少数推理步骤上，等权重蒸馏的差距已经被部分缓解。

3. 同尺寸蒸馏面临饱和效应。vanilla OPD 已经让学生接近甚至超过教师，留给进一步模仿的增益空间有限，此时 Dr. OPD 仍能带来 2.3 分和 3.0 分的平均提升。这个现象说明即使学生与教师能力相近，不同 token 监督的异质性依然存在，但边际收益比强到弱场景小。

4. 加权更新的作用方向与奖励一致。理论片段表明权重更新与当前学生策略处沿更新方向的奖励相关方向导数挂钩，实验上的稳定提升支持“优先学习对奖励影响更大的 token”这一机制解释。

5. 论文没有在已检索片段中报告明显的负结果或失败案例；在各设置下 Dr. OPD 均保持最优或最优之一。需要注意的是，基于现有片段我们还无法判断 Dr. OPD 是否在某些指标、某些难度带或某些模型上出现退化，或是否与具体奖励模型选择强相关，这些需要通过完整实验表格和消融来确认。

Q6: 有什么可以进一步探索的点？

基于论文的问题设定和现有证据，可以进一步探索的方向包括：

1. 放宽理论条件的扩展。目前 improvement guarantee 依赖有限词表、学生/教师概率严格为正、可测权重函数、足够小步长等正则条件；未来可以研究连续/开放词表、概率退化或更大步长下的理论性质，以及全局收敛性而非单步改进保证。

2. 权重函数的参数化与可迁移。论文刻意不限制权重函数形式，以换取闭式更新；如果改为学习一个轻量的权重预测器（例如以状态和 token 输入、以奖励信号为标签），可能实现跨任务/跨学生复用，减少每轮闭式计算的成本。

3. 与其他后训练目标混合。Dr. OPD 的加权蒸馏可以与 GRPO/RLOO 等 outcome-reward 强化学习结合，形成“关键 token 模仿教师、非关键 token 自由探索”的混合目标；也可以与 process reward model、步级验证器结合，把权重直接建立在过程奖励上。

4. 扩展任务与模态。论文只验证了数学与代码生成；自然语言对话、工具调用/智能体轨迹蒸馏、科学文献推理、蛋白质/分子生成等任务中，token 级重要性分布可能更不均匀，值得验证。尤其是 agent 场景下，一条轨迹包含规划、工具调用、结果解析等多类 token，教师信号的异质性可能更强。

5. 权重的可解释性分析。闭式权重本身可以作为诊断工具：分析模型在哪些 token 上被赋予高权重，能揭示学生错误模式、教师关键修正点和奖励模型偏好，帮助理解蒸馏过程中能力的来源。

6. 与 curriculum/难度分带的交互。论文已利用 DeepMath-103K 的难度标签分带训练学生；未来可以研究 token 权重是否与问题难度、学生熟练度存在自适应关系，形成更自动的课程蒸馏。

7. 扩展到多教师蒸馏。如果存在多个强弱不同的教师，Dr. OPD 的权重选择框架可以推广为同时选择“跟哪个教师学”和“学该教师哪些 token”，但需要新的双层形式与求解器。

8. 计算开销的系统测量。当前摘要未给出 Dr. OPD 相对 vanilla OPD 的额外时间/显存开销；实际部署前需要评估每轮闭式权重计算、额外采样和奖励评估的成本。

Q7: 总结一下论文的主要内容

一、问题背景与动机
On-policy distillation（OPD）是大模型后训练中重要的能力迁移方式：学生模型在自己生成的响应上，逐 token 接受教师的稠密监督，因此每个生成位置都有学习信号，比 RLOO/GRPO 这类仅在轨迹末端的标量奖励方法更细粒度。vanilla OPD 对所有 token 的教师信号等权对待，默认每个 token 的监督对学生同样重要。作者指出这一假设并不成立：在长响应中，教师修正关键推理步骤的信号与仅仅偏好不同措辞的信号，对最终答案的影响完全不同；如果蒸馏损失不区分它们，学生就会把优化预算浪费在低价值对齐上，甚至可能被无关的教师偏好带偏。

二、方法主线
论文提出 Dr. OPD（OPD Done Right），把最优加权 OPD 形式化为双层优化。下层问题是在给定权重函数 w 的条件下，学生通过最小化加权 OPD 损失更新策略；上层问题选择 w，使训练后学生的期望任务奖励最大化。由于权重函数定义在所有 context-token 对上且不预设参数形式，直接对权重做梯度更新不可行，论文采用交替迭代求解：每轮先根据当前学生策略闭式更新 token 权重，然后在加权 OPD 目标上做一步梯度更新。理论部分在有限词表、严格正概率、可测权重函数、足够小步长等正则条件下，证明这种加权更新能比 vanilla OPD 更新带来更高的期望奖励；附录 C 给出了记号、正则条件和步长约束（如 ∥θ+(w)−θ̄∥ ≤ η w_max sqrt(E[T]) M、ληLRM ≤ 1 等）。

三、实验主线
实验在数学与代码两类任务、强到弱与同尺寸两种蒸馏设置下展开。数学使用带难度标签的 DeepMath-103K，Qwen3-1.7B-Base 学生在 [3,4) 难度带训练，其他学生在 [6,10] 难度带训练；代码使用 Eurus-RL-Code。主要结果包括：强到弱设置下平均数学成绩相对 vanilla OPD 提升 9.7 分，学生能超越更强教师；对 1.7B-Base 学生，数学提升 1.8 分、代码提升 6.4 分；同尺寸设置下 vanilla OPD 已将学生带到接近或超过教师的水平，但 Dr. OPD 仍取得最好平均表现，相对 vanilla OPD 提升 2.3 分与 3.0 分。论文声称在所有评估设置下一致优于基线。

四、贡献与意义
Dr. OPD 的主要贡献在于把“教师信号的价值不对等”从经验观察变成一个可优化的双层问题，并给出了计算可行、有理论保证的交替求解器。它提示后训练阶段不应盲目模仿教师，而应学会“跟谁学、学哪些 token、学到什么程度”；在强到弱迁移时收益尤其明显。

五、需要回原文确认的部分
由于本次可用的 PDF 语义检索主要覆盖引言、结论和部分附录片段，完整的 Related Work、Method 公式推导、全部 baseline 名称、模型规模表、训练超参、奖励模型选择、消融实验和表格编号仍需要回到原文阅读。当前关于同一尺寸设置中 2.3/3.0 分对应的具体任务、1.7B-Base 与 1.7B 学生的训练条件、以及闭式权重更新的完整推导，只能依据摘要和片段做合理推断，不应视为精确复述。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：如果你的核心关注是生成模型后训练，本文的 token 级加权思路可以迁移到可控文本生成、代码生成和长链推理蒸馏流程。

## 基本信息

- 作者：Zhenyu Wang, Tianze Wang, Linjun Zhang, Yifan Hu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL, math.OC
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38025v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据、论文摘要/引言/结论与启发式草稿；启发式草稿较碎片化，已在字段中结合检索证据重构，证据不足处均标注为合理推断或推测。
