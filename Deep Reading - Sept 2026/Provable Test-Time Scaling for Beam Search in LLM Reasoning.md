---
user_id: "cheng tan"
paper_id: 14303
arxiv_id: "2609.38672v1"
title: "Provable Test-Time Scaling for Beam Search in LLM Reasoning"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.38672v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38672v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-07T01:26:54"
---
# Provable Test-Time Scaling for Beam Search in LLM Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：beam search · test-time compute · LLM reasoning · sample complexity

## 一句话总结

本文为beam search测试时缩放提供首个理论保证，提出置信过滤beam search（CF-Beam），将最优响应存活所需样本从二次覆盖系数降至近线性，并证明其regret上界优于随horizon指数增长的序列级方法，实验证实困难长程推理场景下更鲁棒。

## 摘要

> Beam-search-based test-time methods provide an effective way to improve large language model (LLM) performance on long-horizon generation by pruning invalid reasoning paths early, leading to significantly improved reasoning efficiency and more favorable test-time cost scaling. Despite strong empirical success, the theoretical understanding of beam search remains limited. In this paper, we study the test-time compute guarantee of the commonly used beam search framework that uses the model's internal log-likelihood for intermediate scoring, while relying on an external reward model only after a complete response is generated. We first establish a lower bound for vanilla beam search, showing that at least $Ω(C^\star(x)^2)$ samples are required for the optimal response to survive, where $C^\star(x)$ is the token-level coverage coefficient for prompt $x$. This motivates our modified confidence-filtered beam search (CF-Beam), which reduces the sufficient coverage dependence from quadratic to nearly linear under prefix competitiveness, for fixed horizon, gap, and target accuracy. We then show that the regret of CF-Beam is upper-bounded by the probability of rare failure events and the reward estimation error scaled by a path-level coverage coefficient, where the rare-failure term vanishes as per-step sampling increases. Our results highlight a fundamental advantage of beam search over sequence-level inference methods such as Best-of-N and Best-of-Majority. While the guarantees of these approaches typically involve coverage coefficients that grow exponentially with the horizon $L$, CF-Beam controls the dominant search-induced term through a token-level coverage coefficient that scales polynomially with $L$. Our numerical experiments further confirm that beam search is more robust on hard instances and under increasing reasoning horizons.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决一个双重问题：一是测试时计算方法在长程推理中的样本效率和理论理解缺失，二是beam search虽在工程上被广泛使用、能通过剪枝无效路径提升效率，但其测试时缩放行为没有任何可证明的保证。具体来说，现有主流测试时方法如Best-of-N（BoN）和多数投票在上下文变长时需要极快增长的采样数才能维持精度，因为需要覆盖整个序列级成功事件；而复杂任务中不完美验证器会进一步加剧选择错误，例如过度信任虚假候选或丢弃分数偏低但正确的解。这些方法本质上属于序列级推断：把整条轨迹作为一个单元采样，再用外部奖励或投票聚合选择，因此覆盖系数会随长度指数恶化。相比之下，beam search在生成中逐步剪枝，理论上应该能利用前缀结构降低覆盖代价，但缺乏严格分析。本文因此提出并回答以下具体问题：（1）普通beam search（用模型自身对数似然作为中间分数）究竟需要多少样本才能保证最优响应不被淘汰？（2）能否设计一个改进的beam search变体，把样本复杂度从二次依赖降为近线性？（3）在考虑外部奖励模型噪声的情况下，beam search的遗憾上界如何表达？与序列级方法相比，其搜索诱导的覆盖项能否避免指数增长？（4）理论预测是否在可控实验中得到验证，尤其是困难实例和长horizon场景。论文将这些问题形式化为token级覆盖系数C*(x)和路径级覆盖系数，通过下界定理、充分条件定理和遗憾分解给出系统回答。值得注意，本文的框架刻意选择只用模型自身对数似然做中间打分，而不是更强的过程奖励模型，这使得分析更贴近实际部署中不额外训练过程验证器的场景，但也意味着中间打分质量受限于生成模型自身置信度。

Q2: 有哪些相关研究？

相关研究主要分布在三条线。第一条是测试时计算方法：Best-of-N采样、多数投票、自一致性（self-consistency）、思维链（CoT）及其变体在实证上显著提升LLM推理能力，并被广泛部署于实际系统（如Zaharia等2024；Team等2023）。但这些工作大多关注经验性能，对样本复杂度的理论刻画较少，且已知在长序列时需要指数级样本。第二条是测试时缩放理论：例如Jinnai等2024、Ichihara等2025研究了BoN和投票的缩放行为；Huang等2025分析了不完美验证器对选择的影响；He等2025发表“Does thinking more always help? understanding test-time scaling in reasoning models”等，开始系统研究测试时计算与推理模型缩放的关系。第三条与beam search直接相关：有工作（如2025年某文）分析了beam search用于基于自一致性的不确定性量化（uncertainty quantification），但据本文作者所述，此前没有工作研究beam search的测试时缩放保证，尤其没有给出样本复杂度下界或遗憾上界。此外，强化学习领域关于beam search的变体（如best-of-majority，Di等2025把多数投票与best-of结合）也是本文基线的一部分。本文与现有工作的关键区别在于：它把beam search视为逐步过滤/搜索过程，引入token级覆盖系数和路径级覆盖系数的精细度量，从而揭示beam search相对序列级方法的理论优势。还值得注意的是，本文强调中间打分仅使用模型内部对数似然而非外部过程奖励模型，这与过程监督（process reward model）文献有所区别，更接近实际中无需额外训练验证器的部署方式。总体而言，本文填补了“beam search测试时缩放保证”这一空白，并将自身置于测试时计算理论化的前沿。

Q3: 论文如何解决这个问题？

论文采用理论分析驱动的算法设计路线。首先形式化框架：给定提示x，固定LLM策略π（记为π_ref），生成过程按步骤t=1,...,L进行；每个前缀s_{t-1}处，采样N个下一动作，计算该前缀下每个动作的对数似然打分Q(s_t)=Σ_{i=1}^t log π̂(a_i|s_{i-1},x)（即模型内部对数似然累积），然后保留得分最高的top-b轨迹进入下一轮；最终在完整响应集合B_L中，用外部奖励模型选择得分最高的y。为了衡量搜索难度，定义token级覆盖系数C*(x)，它刻画为保证最优轨迹s*不被剪枝，每个前缀处需要覆盖的候选动作的某种最坏情况数量（合理推断：与最优token概率成反比或与竞争token的比率相关）。论文的第一个结果为下界（Theorem 1）：存在LLM实例使得普通beam search需要Ω(C*(x)²)样本才能让最优响应以目标精度存活；该下界通过构造“最优轨迹以常数概率被丢弃”的硬实例证明（检索片段显示构造中覆盖系数满足50≤C*≤...，可能涉及某个区间）。这个二次依赖说明vanilla beam search虽然利用了前缀结构，但内部对数似然打分可能把中等概率的最优前缀挤出beam，导致需要二次覆盖。 为了改进，作者提出CF-Beam（置信过滤beam search）：在每个前缀处，先根据当前模型估计的下一token分布π̂(·|s_{t-1},x)计算每个采样动作的置信度，只保留满足π̂_t(a_t|s_{t-1},x)≥β_t的动作，然后再从过滤后的集合中取top-b。β_t可以是理论阈值（Oracle版本）或启发式选择如β_t(s)=γ max_a π̂(a|s,x)（Empirical版本，实验中γ=0.3）。这样的过滤使算法优先保留高置信动作，避免低概率无信息候选挤占beam。在“前缀竞争性”假设下（即最优轨迹的每一步前缀相对于其他动作具有足够的概率优势，具体定义未在摘要中给出，合理推断为最优token概率与次优token概率之比有下界），论文证明（Theorem 2/Corollary 1）：固定horizon、gap和目标精度时，CF-Beam的充分样本复杂度从二次降为近线性，即覆盖系数依赖从C*²降到约C*（多项式乘因子）。 最后，论文给出CF-Beam的遗憾上界：regret ≤ P(罕见失败事件) + O(奖励估计误差 × 路径级覆盖系数)。其中罕见失败事件指最优轨迹在置信过滤或top-b选择中被意外淘汰的事件，其概率随每步采样N增加而指数级或多项式消失；路径级覆盖系数衡量整条最优路径被外部奖励模型噪声影响的敏感度，该项由奖励模型本身误差（如ϵ_RM²）控制。与之对比，BoN和Best-of-Majority等序列级方法的覆盖系数通常随L指数增长（因为要求整条序列在一次采样中成功或至少多次近似），而CF-Beam的搜索诱导项由token级覆盖系数控制，随L多项式增长（因为每步只需覆盖当前前缀的局部选择）。理论部分还讨论C与C*max的关系：当实例变难（p变小），C远大于C*max，CF-Beam优势明显；当实例变简单，两者接近，优势缩小。

Q4: 论文做了哪些实验？

实验在RLVR（可能指reinforcement learning with verifiable rewards）设置下进行，使用一个合成LLM模拟器配对噪声奖励模型，噪声水平ϵ_RM²=0.01。目标任务是识别最优响应（即reward模型真实得分最高的完整回答）。评估了两种CF-Beam实现：Oracle CF-Beam使用Theorem 2给出的理论阈值β_t；Empirical CF-Beam采用启发式阈值β_t(s)=γ max_a π̂(a|s,x)，γ=0.3，无需知道最优token概率。对照基线包括：Vanilla beam search（普通beam search，无置信过滤）、Best-of-N（Huang等2025的BoN）、Majority Voting（多数投票）、Best-of-Majority（Di等2025）。实验参数化：three difficulty levels由每个rollout的成功概率p∈{0.01,0.05,0.3}控制，p越小越难。主要展示两个图：Figure 2比较在变化的总采样预算下各算法的识别准确率，三个子图分别对应p=0.01、0.05、0.3；Figure 3报告准确率随某个变量的变化（原文被截断，合理推断可能是推理horizon L或预算的进一步细分趋势）。实验协议、模拟器构建细节、参数设置放在附录H；附录H.2还报道了在预训练LLM上评估Empirical CF-Beam的结果。实验设计重点在于验证理论预测：当p=0.01时稀疏覆盖和尾部波动放大Theorem 1的失败模式，CF-Beam应取得最大优势；当p增大时优势缩小，Vanilla-Beam在足够预算下赶上。注意摘要提到数值实验“进一步确认beam search在困难实例和越来越长的推理horizon下更鲁棒”，所以可能还包含horizon变化的实验图（但未在可见片段中出现，属于合理推断）。

Q5: 发现了什么实验现象？

实验观察与理论预测高度一致。最显著的现象是：在最困难设定p=0.01（每rollout成功率仅1%）下，CF-Beam（尤其Oracle版本）相比所有基线取得最大准确率优势；这正是稀疏覆盖和尾部波动最严重的区域，也正好是Theorem 1中vanilla beam search二次覆盖失败模式被放大的机制——低成功概率意味着最优token在每一步出现频率低，普通beam search容易在早期前缀就将其剪掉，而CF-Beam通过置信过滤集中预算在高概率动作上，更可能保住最优路径。随着难度降低（p=0.05，p=0.3），各算法的差距系统性收窄，Vanilla-Beam在足够大的总预算下能够追赶甚至达到相近准确率。这面图反映了一个反直觉的结论：beam search的优势不是均匀的，而是在最困难、最需要测试时计算的实例上才突出；这意味着简单地把beam search当作通用增强手段可能低估其价值，而CF-Beam专门针对困难分布更有效。另一个观察是Empirical CF-Beam即使没有使用Oracle阈值（不知道最优token概率），仅凭γ=0.3的启发式置信过滤，就能在困难设定下明显超越Vanilla-Beam，说明方法对阈值选择有一定鲁棒性，但尚需观察其与Oracle版本的差距——在易设定下Empirical版本可能因为过滤过于激进或不足而表现不如Oracle（可见片段未明确给出差距，属合理猜测）。此外，论文提到的“beam search在越来越多推理horizon下更鲁棒”这一观察与本文理论一致：序列级方法的覆盖系数随L指数增长，而CF-Beam仅多项式增长，因此长序列时CF-Beam的劣势不扩大甚至相对改善。这些现象共同支持了“token级覆盖系数控制搜索诱导项”的理论主张，也揭示了指标间张力：准确率提升来源于牺牲部分预算在高置信过滤上，若预算极小，过滤可能无益甚至有害。

Q6: 有什么可以进一步探索的点？

本文为beam search测试时缩放打开了多个可进一步探索的方向。第一，放松前缀竞争性假设：当前充分性结果依赖该条件，若最优路径在某个前缀上概率优势微弱或存在多个竞争路径，CF-Beam是否还能保持近线性？这些问题需要更细粒度的覆盖系数设计（如路径依赖的自适应阈值）。第二，自适应阈值选择：Empirical CF-Beam使用固定γ=0.3，但最优γ很可能随难度、horizon和模型校准质量变化，因此可研究基于置信区间或在线学习的β_t选择策略，甚至把阈值与预算分配联合优化。第三，引入过程奖励模型：本文刻意使用内部对数似然作为中间分数，若将中间分数替换为轻量级过程奖励模型，理论框架能否扩展？此时“前缀竞争性”是否更容易满足，覆盖系数会如何变化，都值得分析。第四，验证器噪声的深入建模：当前遗憾项中奖励估计误差乘以路径级覆盖系数，但误差来源只给了二阶矩噪声水平，可以研究非均匀噪声、对抗性噪声或相关噪声对regret的影响。第五，扩展到更广搜索结构：如与蒙特卡洛树搜索（MCTS）或分层beam search结合，其token级覆盖与路径级覆盖的权衡需要重新推导。第六，在真实LLM上的大规模验证：附录H.2只给出初步预训练模型评估，未来需要覆盖多个模型规模、任务类型（数学、代码、agent决策），建立测试时计算-准确率的Pareto曲线，并比较CF-Beam与BoN在真实基准上的缩放RPM（每token查询数）成本。第七，理论下界的紧性：vanilla beam search的下界是否紧，CF-Beam的近线性上界能否改进为严格线性？是否可在非最坏情况下给出平均覆盖系数的更优刻画？第八，将覆盖率系数与可解释性挂钩：例如分析哪些token是“瓶颈前缀”，从而指导推理时资源分配。这些方向都有可能推进测试时计算理论的实用化。

Q7: 总结一下论文的主要内容

论文围绕“beam search测试时缩放”这个前沿问题，给出了首个系统性的理论分析，并据此设计了新算法。论证主线从经验现象出发：测试时计算在LLM推理中效果显著，但主流序列级方法（BoN、多数投票等）在长序列和困难问题上需要指数级增长的样本，且易受不完美验证器影响；而beam search通过逐步剪枝减轻了复合错误，在工程中表现出高效率，但缺乏理论支撑。技术主线分四步。第一步，建立通用框架：把beam search建模为逐步采样-打分-剪枝过程，中间分数采用模型内部对数似然累积，外部奖励模型只作用于最终候选集。引入token级覆盖系数C*(x)量化困难程度。第二步，给出负面结果（Theorem 1）：普通vanilla beam search需要Ω(C*(x)²)样本才能保证最优响应以固定精度存活，证明通过构造一个最优路径以常数概率被剪掉的硬实例完成。这个二次依赖揭示了内在的低效：当最优token在某个前缀出现概率不高时，它可能被其他高似然但次优的动作挤出beam，要抵消这种挤出需要二次覆盖。第三步，设计正面算法CF-Beam：在每一步先做置信过滤，仅保留概率不低于β_t的动作，再取top-b；置信过滤的作用是把预算集中在“有可能成为最优前缀”的动作上，减少无效候选的干扰。在前缀竞争性假设（即最优前缀具备足够概率优势）下，Theorem 2把充分覆盖依赖从C*²降为近线性（固定horizon、gap和目标精度）。第四步，导出遗憾上界：regret由罕见失败事件概率与奖励噪声×路径级覆盖系数构成；罕见失败项随逐步采样增加收敛到零，因此长期看主要误差来自外部奖励估计误差。这个分解清晰地分离了搜索误差与验证器噪声。与序列级方法对比是关键理论卖点：BoN和Best-of-Majority的覆盖系数随L指数增长，因为需要整条轨迹级别的成功；CF-Beam的搜索诱导项由token级覆盖系数控制，随L多项式增长，因此从理论上证明了beam search在长程推理上的基础优势。实验主线使用合成LLM模拟器和噪声奖励模型，将难度参数化为每rollout成功概率p∈{0.01,0.05,0.3}，比较Oracle CF-Beam、Empirical CF-Beam（γ=0.3）、Vanilla beam search、BoN、多数投票和Best-of-Majority。结果显示CF-Beam在p=0.01最困难时优势最大，随着p增大差距缩小，且趋势与C与C*max的关系一致；这验证了理论预测的机制。论文还在预训练LLM上做了初步验证，并声称首次为beam search的测试时缩放提供可证明保证。整体上论文构建了“下界→改进算法→上界→遗憾分解→实验验证”的完整理论链条，强调beam search相对序列级推断的维度优势，同时指出其优势集中在困难长程场景。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文属于生成与推理方向，与用户画像中“generation”方向权重直接相关，提供了测试时推理的理论纵深；

## 基本信息

- 作者：Qijia He, Yu Huang, Yuan Cheng, Yuxin Chen, Yingbin Liang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38672v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文元数据、摘要、部分引言/结果片段及语义检索证据；由于未提供完整PDF原文，涉及定理证明细节和部分实验数据的内容为基于证据的合理推断或推测，需以原文为准。
