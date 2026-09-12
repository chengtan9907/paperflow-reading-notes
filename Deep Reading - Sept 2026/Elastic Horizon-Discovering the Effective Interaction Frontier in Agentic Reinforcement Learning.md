---
user_id: "cheng tan"
paper_id: 10892
arxiv_id: "2609.07247v1"
title: "Elastic Horizon: Discovering the Effective Interaction Frontier in Agentic Reinforcement Learning"
publish_date: "2026-09-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.07247v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.07247v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-12T13:22:34"
---
# Elastic Horizon: Discovering the Effective Interaction Frontier in Agentic Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic reinforcement learning · interaction horizon · closed-loop control · curriculum learning

## 一句话总结

论文提出「有效交互前沿（effective interaction frontier）」假设，并给出 Elastic Horizon——一个用成功轨迹长度的 90 分位数动态估计 H*、经 EMA 平滑后闭环调节 agentic RL 交互预算的控制器，在 AppWorld 与 BFCL 上使 7B/14B 智能体的 horizon 从欠配与过配初值两端收敛到饱和带内，取得最佳成功率并最多节省 25% 的每步轨迹 token。

## 摘要

> Scaling the interaction horizon-the maximum number of environment interactions per episode-improves LLM agents on long-horizon tasks, and curriculum-based methods that progressively expand the horizon outperform fixed-horizon alternatives. However, existing schedules are open-loop: they monotonically increase the horizon until a manually specified maximum, with no mechanism to detect when further expansion stops helping. We propose the effective interaction frontier hypothesis: a dynamic boundary beyond which additional interactions yield diminishing returns while cost grows linearly. We then introduce Elastic Horizon, a closed-loop controller that tracks this boundary via the 90th percentile of successful trajectory lengths. On AppWorld and BFCL, fixed-horizon sweeps reveal clear saturation plateaus; Elastic Horizon stabilizes the horizon inside the saturation band from both under- and over-capacity initializations, attains the best success rates across 7B and 14B backbones, and saves up to 25% of per-step trajectory tokens. Our work shifts the paradigm from how to scale interaction horizons to when to stop scaling.

Q1: 这篇论文试图解决什么问题？

1) 论文瞄准的核心问题：agentic RL 中「每个 episode 允许多少次环境交互」这一预算参数（interaction horizon）被当成超参数来人工设定，而现有 curriculum 式调度是开环的。开环的含义是：调度器只按预设规则单调地把 horizon 往上推，直到某个人工设定的上限；它没有任何来自智能体自身行为的反馈信号，因此无法判断「继续加预算还有没有用」。
2) 这一问题的代价是双向的。若上限设得过高，多余交互不产生能力增益，但每一步都要付出 token 与推理成本，成本随 horizon 线性增长；若上限设得过低，能力被预算饿死（under-budgeted），智能体无法完成本可完成的长程任务。
3) 论文引用的既有观察为问题提供了经验基础：据 Introduction 片段，Liu et al. (2025) 发现 ReAct 智能体「在 100 次工具调用预算处饱和」，「无法利用额外的工具预算，触及性能天花板」；Shen et al. (2025) 观察到固定长 horizon（h = 30）会导致……（该片段在检索处被截断，具体结论需回原文核对）。这两条证据共同说明：饱和是真实存在的，而固定值或盲目递增都不是正确的应对方式。
4) 论文对问题的重新表述是其关键动作：把「饱和」从一个静态常数重新概念化为一个随训练动态移动的边界，即「有效交互前沿 H*」。这一重述直接决定了解决路径——既然边界是动态的，就必须在线估计而不是离线设定。
5) 隐含假设（合理推断）：成功轨迹的长度分布携带了「任务实际需要多少交互」的信息；且训练过程中策略能力变化会体现在该分布上，因此可以用它做反馈信号。这个假设是整篇工作的方法论支点，也是后续所有批评的落点。
6) 需求侧动机（基于摘要的推断）：在长程 agent 训练成本高企的背景下（每步都是 LLM 前向 + 环境调用），把预算从超参数降级为被控变量，对算力预算敏感的训练方有直接价值。
7) 问题的边界条件：论文关注的是「单次 episode 内交互次数上限」这一种预算，而非上下文长度、工具种类数、重试次数等其他预算；后者的可迁移性作者只在结论中作为展望提及。

Q2: 有哪些相关研究？

1) 论文的 Related Work 章节原文未在本次检索证据中出现，因此以下梳理主要来自 Abstract、Introduction 片段与 Conclusion 中显式引用的工作，以及对研究脉络的合理推断；细节需回原文核对。
2) 固定 horizon 的 agentic RL：该脉络把交互预算设为常数，简单但在长程任务上要么不足要么浪费。Shen et al. (2025) 的观察（固定长 horizon h = 30 会导致……，片段截断）属于这一支的负面证据。
3) Curriculum 式 horizon 扩张：论文指出渐进扩大 horizon 的方法优于固定 horizon 的做法。这是与本文最直接竞争/继承的一支——本文承认其有效性，但批评其调度是开环、单调、且终点靠人工指定。
4) 饱和现象的经验研究：Liu et al. (2025) 报告 ReAct 智能体在 100 次工具调用预算处饱和、无法利用更多预算。这一支提供了「收益递减确实存在」的实证前提，但止步于观察，未给出在线控制机制。
5) 预算/算力分配的一般思路（合理推断的相邻脉络）：训练中动态分配 compute、context 或采样预算的工作，以及 curriculum learning 中按能力推进难度的经典范式。本文可被视为把「按能力推进」的思想从任务难度迁移到交互预算维度上。
6) 与本文最接近的差异化定位：既有工作回答「怎么扩大」，本文回答「何时停止扩大」；既有工作输出静态 schedule，本文输出闭环控制律。
7) 对照组与基线（合理推断）：论文的对照主要是固定 horizon 扫描（fixed-horizon sweeps）以及开环 curriculum 调度，而非其他自适应预算方法——这一点在证据中只以「fixed-horizon alternatives」的形式出现，论文是否与其他自适应方法做过对比，证据不足以判断。
8) 评测环境脉络：AppWorld（交互式 app/API 任务）与 BFCL（工具调用/函数调用基准）是 agentic 工具使用评测的两条主线，前者偏长程多步任务、后者偏函数调用正确性，二者互补。
9) 信息缺口提示：论文引用的具体文献列表、是否讨论了 horizon 与上下文窗口的关系、是否与前缀式自适应计算（adaptive computation）工作对话，均在本次证据之外。

Q3: 论文如何解决这个问题？

1) 总体思路：把交互预算从「超参数」改写为「被控变量」，用智能体自身的行为证据在线估计有效交互前沿 H*，并让训练用的 horizon 跟随该估计双向移动。
2) 第一步是定义前沿的可观测代理量。Elastic Horizon 采用的是「成功轨迹长度的第 90 百分位数」（P90）：只有成功的轨迹才被用来做能力估计，取其长度的 P90 作为 H* 的估计——含义是，让预算刚好覆盖绝大多数成功轨迹所需要的交互步数，而不是被少数极长轨迹或大量失败轨迹拉偏。
3) 第二步是阻尼与稳定。检索证据明确指出该方法「通过基于百分位的边界估计并结合 EMA 平滑来自适应，且不需要人工 schedule（Section 4）」。EMA 的存在意味着单批次的噪声不会直接传导为 horizon 的抖动，这是闭环控制能「稳定在饱和带内」而非振荡的关键（合理推断）。
4) 第三步是双向调节语义。Conclusion 中的表述是：控制器从智能体「已展示的行为」中设定交互预算，并把 frontier 同视为一个区间而非单向边界——Limitations 片段提到模型在训练中可能会出现 under-budgeted 的情况，对应的解决方向是「从已展示能力出发、在双向进行分配」。这与开环 curriculum 的单调递增形成鲜明对比：Elastic Horizon 在证据显示能力已足够时可以主动收缩预算。
5) 无人工 schedule：Introduction 明确该方法「requiring no manual schedule」，即不需要预先指定最大 horizon、步长或扩张时刻表；这是与 curriculum 方法最操作层面的差别。
6) 机制验证（mechanism validation）：Introduction 的贡献列表中有一项专门是机制验证，其片段显示「Elastic Horizon stabilizes the horizon inside the…」（后续被截断，从摘要可知是 inside the saturation band）。这说明论文不只报告端到端指标，还单独检查控制器是否真的在做它声称的事——即 horizon 轨迹是否落在饱和平台内部。
7) 算法细节缺口：本节涉及的控制器更新频率（每多少 step 更新一次 H*）、初始 horizon 的设定方式、P90 的样本窗口大小、EMA 系数、horizon 是否被离散/截断、与 RL 算法（PPO/GRPO 等）的耦合方式，在现有证据中均未见，属于需要回原文 Section 4 核对的关键实现细节。

Q4: 论文做了哪些实验？

1) 评测环境：AppWorld 与 BFCL。二者分别覆盖交互式长程 app/API 任务与工具（函数）调用能力，构成互补的评测面。
2) 主干模型：7B 与 14B 两档 LLM 主干，用来检验方法是否随规模稳定（论文声称在两档主干上都取得最佳成功率）。
3) 固定 horizon 扫描（fixed-horizon sweeps）：这是全文最基础、也是最关键的一组实验——通过扫描不同固定 horizon 值，画出「horizon–性能」曲线，并从中识别出饱和平台（saturation plateau）。它既是问题存在性的证据，也是后续判定 Elastic Horizon 是否「落在饱和带内」的参照系。
4) 初始化敏感性实验：从欠容量（under-capacity，初始 horizon 偏低）与过容量（over-capacity，初始 horizon 偏高）两种初值出发，检验控制器能否从两端都收敛到饱和带。这是闭环控制器最核心的鲁棒性测试，也是开环单调 curriculum 做不到的方向（它无法从高初值主动降下来）。
5) 机制验证实验：单独检验 horizon 的演化轨迹是否稳定在饱和带内，而不仅是看最终成功率。
6) 成本计量：以「每步轨迹 token」（per-step trajectory tokens）作为效率指标，报告最多 25% 的节省。这一口径意味着效率收益来自每一步交互更短/更少，而非来自少跑训练步。
7) 对照设置：主要对照是固定 horizon 方案（及其不同取值）与开环 curriculum 式扩张。是否与「随机 horizon」「线性 schedule」「早期停止膨胀」等朴素启发式做对照，证据中未见。
8) 消融（合理推断但证据薄弱）：Limitations 提到 P90 的选择是「经验驱动的」，暗示论文可能对百分位取值做过敏感性分析；但具体消融表（不同 percentile、有无 EMA、不同更新频率）在现有证据中不可见，需回原文核对。
9) 未覆盖的维度：是否在多任务/多环境混合、不同 RL 算法、更长 horizon 场景（如 h 上百）、以及训练后期能力回落的情形下测试，证据不足。
10) 数据规模与训练步数、超参、随机种子数等信息在现有证据中完全缺失。

Q5: 发现了什么实验现象？

1) 饱和平台是真实存在且可观测的：固定 horizon 扫描在 AppWorld 与 BFCL 上都暴露出清晰的饱和平台——超过某个 horizon 区间后，性能不再提升（甚至持平），而成本仍在增长。这是全文最基础的经验事实，也直接支撑了「有效交互前沿」假设。
2) 收益递减与成本线性增长的剪刀差：摘要明确把 frontier 刻画为「额外交互收益递减、成本线性增长」的分界。这意味着最优预算不是「越大越好」，而是存在一个收益/成本意义上的合理区间。
3) 双向收敛现象：Elastic Horizon 从欠容量初值出发会向上进入饱和带，从过容量初值出发会向下进入饱和带，最终都稳定在带内。这比「从低处爬到高处」的单向 curriculum 更难，也更能说明闭环信号的有效性。
4) 控制器行为与成功统计量绑定：由于 H* 取自成功轨迹长度的 P90，可以观察到 horizon 会随成功轨迹长度分布的变化而移动——训练早期成功稀少/轨迹短时预算偏低，能力提升后成功轨迹变长，预算随之抬升（合理推断，需回原文的 horizon 轨迹图核对）。
5) 成功率与效率同时改善，而非权衡：论文报告在两档主干、两个基准上取得最佳成功率，同时最多节省 25% 的每步轨迹 token。这一「同向改善」是本文最强的卖点，也意味着对照的固定 horizon 基线处在饱和带之外（过高或过低）而非恰好最优。
6) 冷启动期的失败模式（论文自陈）：Limitations 指出 Elastic Horizon 依赖成功轨迹统计来做能力估计，因此在训练早期或成功稀疏的场景下，估计信号质量差——这是一个结构性弱点：控制信号只在「已经有成功」之后才可靠。
7) 启发式带来的张力：P90 的选择被作者自己称为「经验驱动」。作为纯人工先验，它与论文批评开环 schedule 「靠人工指定」的立论存在一定张力（这是评审很可能追问的点，也属于本文的自洽性风险）。
8) 反事实/负结果提示（来自 Limitations 的展望）：实例级或难度条件下的 horizon 分配可以进一步提升效率，但会带来额外复杂度——作者把它留给未来，等价于承认当前实现在实例粒度上是「一刀切」的。
9) 指标间的张力：per-step token 节省与成功率同时改善，说明效率收益并非来自「少做事」，但论文未（在现有证据中）报告 episode 级总成本、wall-clock 或训练总步数，因此「节省」的成本口径是否等价于端到端收益仍需核实。
10) 缺失的现象证据：不同主干规模（7B vs 14B）的饱和带位置是否随规模移动、两个基准的饱和带形状是否一致、控制器超参（EMA 系数、窗口）变化时是否出现振荡，这些在现有证据中无法判断。

Q6: 有什么可以进一步探索的点？

1) 冷启动与稀疏成功问题：Limitations 明确指出 Elastic Horizon 依赖成功轨迹统计；在训练早期或难任务上成功稀疏时，估计不可靠。可行的方向包括引入不依赖成功标签的替代信号，或对估计做显式的置信度加权与保守回退。
2) 替代控制信号：论文在 Limitations 中点名了两条路径——使用部分进展指标（partial progress indicators，如子目标完成度、里程碑达成）与使用全部 rollout（而非仅成功 rollout）的轨迹长度分布。后者直观上样本效率更高，但会引入失败轨迹极长/极短的偏置，需要新的稳健统计量。
3) P90 的启发式性质：作者承认百分位选择是经验驱动的。进一步方向是把百分位本身变成可学习/可自适应的参数（例如按成功率反馈调节分位数），或给出 frontier 估计的偏差–方差分析，把启发式升级为有原则的估计器。
4) 粒度细化：Limitations 提到实例级或难度条件下的 horizon 分配，可以进一步提效，代价是额外复杂度。这实际上是把单条全局预算曲线换成一族条件化预算（难度分桶、任务类型、子任务阶段）。
5) 迁移到其他预算维度：Conclusion 明确提出——同样的「按已展示能力在双向分配」的处理，有望适用于 agent 训练中除交互 horizon 之外的其它预算（例如上下文长度、工具调用次数、重试预算、搜索宽度），这是本文最自然的外延。
6) 理论化：目前「有效交互前沿」是经验假设。可探索的方向包括在简化的决策/采样模型下刻画 frontier 的存在性与形状，给出饱和带宽度与任务结构的关系。
7) 与 curriculum 的组合：把闭环控制器叠加在难度 curriculum 之上（难度与 horizon 两个维度同时自适应），可能比单独调节 horizon 更强，但两个闭环的相互干扰需要研究。
8) 稳定性与超参鲁棒性：EMA 平滑系数、更新频率、估计窗口对收敛与振荡的影响，值得系统化的敏感性研究，并探索用控制论工具（阻尼、迟滞、死区）显式保证稳定。
9) 评测外推：当前证据只覆盖 AppWorld 与 BFCL，且 horizon 规模有限。向更长程环境、多智能体协作、真实生产工具链推广时的表现，以及是否出现「任务本身变难导致 frontier 移动」的情形，都值得检验。
10) 成本口径的完整化：从 per-step trajectory token 扩展到端到端训练成本、wall-clock 与能耗，验证效率声明在系统层面是否成立。

Q7: 总结一下论文的主要内容

1) 问题与背景：agentic RL 中，每个 episode 能进行多少次环境交互（interaction horizon）是决定 LLM 智能体在长程任务上表现的关键预算。扩大 horizon 有收益，因此主流做法采用 curriculum 式调度，逐步把 horizon 推大；这类方法确实优于固定 horizon。但作者指出，所有现有调度都是开环的：它们单调递增直到一个手工指定的上限，没有任何机制判断继续扩张是否仍然有效。代价是双向的——上限过高时额外交互只带来成本（成本随 horizon 线性增长）而无收益；上限过低时能力被预算压制。既有的经验证据（Liu et al. 2025 报告 ReAct 智能体在 100 次工具调用预算处饱和、无法利用更多预算；Shen et al. 2025 观察到固定长 horizon h = 30 的负面现象，片段截断）说明饱和真实存在，但止步于观察。
2) 核心假设：作者提出「有效交互前沿（effective interaction frontier）」假设——存在一个动态边界，越过它之后额外交互收益递减，而成本线性增长。与把饱和当作静态常数的做法不同，这里 frontier 是随训练动态移动的，因此必须在线估计。
3) 方法：Elastic Horizon 是一个闭环控制器，用智能体自身展示的行为来设定交互预算。具体地，它用成功轨迹长度的第 90 百分位数估计有效交互前沿 H*，并通过 EMA 平滑后驱动 horizon 的更新，无需任何人工 schedule。由于信号来自成功轨迹，控制器可以在两个方向上调节：能力提升、成功轨迹变长时扩张预算；证据显示预算已超出所需时收缩预算（对应 Limitations 中提到的 under-budgeted 修正与「双向分配」表述）。
4) 实验设计：在 AppWorld 与 BFCL 两个基准上，用 7B 与 14B 两档主干模型做验证。关键的第一组实验是固定 horizon 扫描，用来暴露饱和平台并定义「饱和带」这一参照系；随后从欠容量与过容量两种初始化出发检验闭环收敛性；另设机制验证，检查 horizon 是否真的稳定在饱和带内；效率指标采用每步轨迹 token。
5) 主要结果：固定 horizon 扫描在两个基准上都显示清晰的饱和平台；Elastic Horizon 从欠配与过配两端初始化都能把 horizon 稳定到饱和带内；在 7B 与 14B 主干上均取得最佳成功率；同时最多节省 25% 的每步轨迹 token。即成功率与效率同向改善，而非此消彼长。
6) 论证主线与范式主张：论文的论证链条是「饱和存在（经验）→ 饱和是动态前沿（假设）→ 用成功轨迹 P90 可在线估计（机制）→ 闭环控制落在饱和带内且更省（验证）」。作者据此主张把研究范式从「如何扩大交互 horizon」转为「何时停止扩大」，并进一步认为交互预算不必是超参数。
7) 局限（论文自陈）：对成功轨迹统计的依赖导致冷启动/稀疏成功时估计不可靠；P90 是经验驱动的启发式，与论文反对「人工指定」的立论存在张力；实例级或难度条件化的预算分配可进一步提效但更复杂，留作未来工作；未来信号可转向部分进展指标或全 rollout 长度分布。
8) 证据边界说明：本次精读的 PDF 检索证据集中在 Abstract、Introduction、Conclusion 与 Limitations 片段，Method（Section 4）与 Experiments 的原文细节、具体成功率数值、消融表、控制器超参与 RL 算法配置均未出现在证据中，相关处已逐条标注为推断或缺口，需回原文核对。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与该用户画像中的「agent」方向直接重合（权重 0.10）：主题是 agentic RL 的交互预算控制，属于智能体训练的核心工程与算法问题。

## 基本信息

- 作者：Gangyi Zhang, Junjie Meng, Letian Zhang, Wei Wu, Yang Zheng, Dong Wang, Yang Liu, Guanjun Jiang, Chongming Gao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-09-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.07247v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract、Introduction、Conclusion、Limitations 片段，并据此补全 heuristic_draft；Method 与 Experiments 章节原文及具体数值未出现在证据中，相关细节已逐处标注为推断或待核实缺口。
