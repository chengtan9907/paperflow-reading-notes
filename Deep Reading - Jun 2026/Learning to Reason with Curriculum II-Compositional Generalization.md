---
user_id: "cheng tan"
arxiv_id: "2606.27721"
title: "Learning to Reason with Curriculum II: Compositional Generalization"
publish_date: "2026-06-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Jun 2026/2606.27721.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27721"
abs_url: "https://arxiv.org/abs/2606.27721"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-10-12T13:29:28"
---
# Learning to Reason with Curriculum II: Compositional Generalization

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：compositional generalization · curriculum learning · chain-of-thought reasoning · semiautomaton simulation

## 一句话总结

本文通过半自动机（semiautomaton）模拟这一规范问题，证明基于自课程（autocurriculum）的递归分解-组合学习在监督微调（SFT）与可验证奖励强化学习（RLVR）两种设定下，能分别以亚多项式监督复杂度和指数级更弱的覆盖条件学会长度 T 的序列计算，从而首次从理论上刻画了组合泛化为何以及何时比端到端直接学习更高效。

## 摘要

> Compositional generalization, the ability to solve complex problems by combining solutions to simpler sub-problems, is a fundamental capability of both natural and artificial intelligence, and a key mechanism underlying chain-of-thought reasoning. However, the theoretical underpinnings of compositional generalization remain poorly understood: when and why does decomposing a problem into parts yield more efficient learning than solving it directly? We study this question through the canonical problem of learning to simulate semiautomata (predicting the outcome of $T$ steps of sequential computation), a model that captures state tracking, regular language recognition, and modular arithmetic. We show that an autocurriculum-based approach building on Part I of this series, recursively decomposing longer sequences into shorter sub-problems, learning to solve them, and composing the solutions, achieves dramatically better statistical complexity than direct methods. (i) For a setting inspired by supervised fine-tuning (SFT) where the learner receives interactive feedback on intermediate states of the computation, curriculum facilitates learning from only $2^{\mathcal{O}(\sqrt{\log T})}$ tokens of supervision; i.e., subpolynomial in the sequence length $T$, overcoming the $Ω(T)$ token barrier required by direct simulation. (ii) For a setting inspired by reinforcement learning with verifiable rewards (RLVR), where the learner improves a pre-trained reference model using an outcome verifier, we show that curriculum reduces the requirement on the reference model from coverage at the full sequence length $T$ to coverage at a shorter block length $B \ll T$, an exponentially weaker condition.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：组合泛化（compositional generalization）在经验上被广泛认为是思维链推理和推理模型 scaling 的关键机制，但其理论基础薄弱——当且仅当满足什么条件时，把问题分解为子问题再组合，能比端到端直接学习更高效？这种效率提升的幅度有多大？
2. 具体学术空白：已有工作大量强调 curriculum training 和 autocurriculum（模型自己生成课程）在实践中的价值，但基本没有严格理论回答：(a) autocurriculum 何时有效、为何有效、能降低多少学习代价；(b) 课程设计应遵循什么原则；(c) 在 SFT 和 RLVR 两种主流后训练范式下，组合课程分别能带来什么样的统计复杂度收益。
3. 本文选择的研究问题载体：学习模拟半自动机（semiautomaton），即给定初始状态和长度为 T 的输入序列，预测 T 步顺序计算后的状态。这个任务模型化了一类顺序计算，能编码状态追踪（state tracking）、正则语言识别和模运算，且具有马尔可夫结构，便于形式化分解；同时由于半自动机可编码分支程序（Barrington, 1986），在标准密码学假设下难以学习，为下界论证提供了基础。
4. 两个具体设定下的问题：(i) interactive SFT（iSFT）设定：学习者可以从计算中间状态获得交互式反馈，直接模拟需要 Ω(T) 个监督 token，问题在于课程分解能否打破这个线性屏障；(ii) RLVR 设定：学习者只有一个预训练参考模型 π_ref 和结果验证器，没有中间状态标签，问题在于课程能否把对参考模型覆盖质量的要求从全长 T 尺度降到更短的块长 B 尺度。
5. 隐含的范式问题：论文实际上在追问“推理时额外计算（CoT token）与训练时课程分解之间的分工”——即使每一步推理仍需线性查询，训练阶段对中间状态的监督需求能否被压缩到亚多项式；以及“预训练模型需要多好，后训练课程才能成功”——覆盖条件从 T 降到 B 意味着预训练负担指数级减轻。

Q2: 有哪些相关研究？

1. 组合泛化与思维链的经验证据：Lake and Baroni 2018、Saxton et al. 2019 等早期工作指出系统组合性是智能的核心；Press et al. 2023、Dziri et al. 2023、Zhao et al. 2024 等近期工作提供证据表明组合泛化是推理模型 scaling 到困难任务的关键机制。这些工作主要是经验性的，缺少理论刻画。
2. 课程学习：Bengio et al. 2009 提出 curriculum learning 的经典框架；Zeng et al. 2025、Motwani et al. 2025、Team et al. 2025 等近期工作强调课程式训练对推理模型组合泛化的重要性，即按难度递增的问题教会学习者越来越复杂的低层技能组合。
3. 自课程（autocurriculum）：Chen et al. 2025b、Shi et al. 2025、Yu et al. 2025、Zhao et al. 2025 等工作展示了课程可以由学习者自己诱导产生，避免手工设计难度概念。本文的理论框架正是建立在这一方向上。
4. 本系列 Part I（Rajaraman et al. 2026）：Part I 提出了受 boosting 启发的 autocurriculum 形式——用模型自身表现从大规模问题池中选择重点问题——并建立了初步理论。本文是 Part II，把框架推广到 SFT 和 RLVR 两种后训练设定，并聚焦于半自动机模拟的组合分解。
5. 学习理论中的相关下界：Kearns and Valiant 1994 的经典假设（如 RSA 等密码学假设）下难以学习某些概念类；Barrington 1986 证明半自动机可编码分支程序，使得本文能用标准密码学假设为直接学习方法给出下界，从而说明课程带来的复杂度降低是本质性的。
6. 覆盖原理（The Coverage Principle）：Foster 等人 2025 的预训练-后训练覆盖理论（coverage principle）是本论文 RLVR 部分的理论背景——后训练能否成功取决于预训练模型对目标分布的覆盖程度；本文的结果相当于把覆盖条件从全长 T 降到块长 B。检索证据中 references 片段出现该文献，说明作者直接把本文结果与覆盖原理文献对话。
7. RLVR 与可验证奖励：用结果验证器（outcome verifier）改进预训练模型是当前 LLM 后训练（如 RLHF/V/RLEM 变体）的重要实践方向，本文为其提供了第一个（据论文声称）关于课程分解收益的严格理论。
8. 局限与相邻文献：论文讨论部分提到 mistake bound 可学习性假设强于 i.i.d. 可学习性（VC 维度有限），这一区分连接了在线学习理论（mistake bound）与统计学习理论（i.i.d. 可学习性 / VC 维），说明本文框架可放在更广的学习理论谱系中理解。

Q3: 论文如何解决这个问题？

1. 总体框架：直接承接 Part I 的 autocurriculum 方法，核心思想是递归分解（recursive decomposition）——把长度为 T 的序列计算分解成更短的子问题（块长 B 尺度），先学习求解短块，再把短块解组合成全长解。组合方式是学习半自动机的转移结构：用长度 B 的块模型模拟每一步块转移，迭代组合得到 T 步结果。
2. 设定 I：interactive SFT（iSFT）。学习者可以查询交互式思维链 oracle（iCoT oracle），获得中间计算状态的反馈（即每一步的状态标签）。直接方法需要 Ω(T) 个监督 token。课程方法把长度 T 的问题递归分解为短块，对短块做监督学习，再组合。关键结果：仅需 2^{O(√log T)} 个监督 token，即亚多项式（subpolynomial in T）。注意检索证据中明确说明：即使使用课程方法，每次思维链推理仍需要对 iCoT oracle 做 Ω(T) 次查询，因此查询复杂度仍是线性的；被压缩的是训练所需的监督 token 数（样本/标签复杂度），不是推理时的查询数。
3. 设定 II：RLVR。学习者只有弱参考模型 π_ref 和结果验证器（outcome verifier），没有中间状态标签。方法要点（据 Introduction 检索片段）：把长度 B 尺度学到的模型合并为全长模型时，通过 guess-and-check 模拟中间状态标签——即用当前模型猜测中间状态，再用结果验证器检查最终结果是否正确，从而筛选出正确的中间状态标签用于进一步训练。训练总代价（training cost）定义为训练过程中从 π_ref 或 Π 中任何模型生成的状态数。
4. 理论工具：概率过滤/拒绝采样框架（检索到的证明片段显示，方法使用接受-拒绝过程从分布 μ 中采样块 X，并按权重 w(X_i) 进行重要性加权，保证每个块 i 被选中的概率 ≤ w(X_i)/(L·‖w‖_∞) 形式的界，这是课程分布构造的核心引理）；KL 散度链式法则用于分析组合误差传播（讨论部分 Proposition 6.1 显示，在 forward KL 目标下组合思想仍然成立）。
5. 下界论证：利用半自动机可编码分支程序（Barrington 1986）以及标准密码学假设（Kearns and Valiant 1994），证明直接学习方法存在本质的学习障碍，从而说明课程带来的指数级/亚多项式收益不是人为设定的产物。
6. 两个设定共享的机制：马尔可夫结构是分解的关键——半自动机模拟具有马尔可夫性，使长度为 T 的问题可以无损地分解为块转移的组合；模型也被要求是马尔可夫的（这是局限，见 limitations）。课程的核心作用是把“学习长程依赖”转化为“学习短程转移 + 组合规则”，从而把监督复杂度和覆盖条件的瓶颈从 T 移到 B。

Q4: 论文做了哪些实验？

1. 论文性质说明：从 arXiv 元数据和检索证据（references、discussion 片段）看，本文是理论（learning theory）论文，没有传统意义上的实验章节——sections 中 method、results、conclusion 基本为空，检索命中的内容全部来自 Introduction 和 Discussion 以及 References/附录证明片段。因此以下按理论论文的“实验等价物”（定理、引理、下界、命题）来组织。
2. 设定 I（iSFT）的理论结果：课程方法只需 2^{O(√log T)} 个监督 token 即可学会半自动机模拟，而直接模拟有 Ω(T) token 下界——这是本文的核心定量结果，对应 Theorem 3.4 附近的讨论（Discussion 6.2.1 专门讨论该定理中 T 依赖的最优性问题，说明作者自己也在追问 √log T 指数是否还能改进）。
3. 设定 II（RLVR）的理论结果：课程方法把参考模型 π_ref 的覆盖要求从全长 T 降到块长 B ≪ T，并在 Section 3.4 给出了正式的下界和与直接方法的详细比较（检索证据明确提到 Section 3.4 包含 formal lower bounds and a detailed comparison）。
4. 下界与假设分析：在标准密码学假设（Kearns and Valiant 1994 风格）下，由于半自动机可编码分支程序（Barrington 1986），直接学习存在固有困难；论文在 Section 3.4 给出正式下界。
5. 理论性质的验证性命题：讨论部分给出 Proposition 6.1——当学习目标是 forward KL 时，组合思想依然成立（KL 链式法则），说明主要定理不依赖特定的目标函数形式；这是一个“概念验证”性质的理论结果，相当于消融了目标函数这个变量。
6. 假设对比分析：i.i.d. 可学习性 vs mistake bound 可学习性的对比（limitations 检索片段）——当状态集 S={0,1} 且输入字母表 Σ 任意时，i.i.d. 可学习性等价于假设类 Π 的 VC 维有限；而有限 mistake bound 是比 i.i.d. 可学习性更强的假设。这个对比是论文对自身假设强度的“敏感性分析”。

Q5: 发现了什么实验现象？

1. 核心现象：分解带来复杂度相变。直接模拟的长度依赖是线性的 Ω(T)，课程分解后监督复杂度降到 2^{O(√log T)}——从线性到亚多项式，属于复杂度相变而非常数因子改进。这是本文最核心的“实验现象”（理论意义上）。
2. RLVR 设定下的现象：覆盖条件从 T 降到 B 是指数级弱化。因为块长 B ≪ T，对参考模型的覆盖要求在长度维度上从指数规模（2^T 级别的状态路径空间）降到 2^B 级别，这是质的改变。
3. 反直觉点：即使训练侧监督复杂度被压缩到亚多项式，推理（inference）侧的查询复杂度仍然是 Ω(T)——每次思维链仍需要线性次 oracle 查询。也就是说课程压缩的是训练成本而非推理成本，训练与推理的成本结构不对称。这个点在检索证据（Introduction 片段）中被明确陈述，属于论文自己承认的边界。
4. 猜测-检查（guess-and-check）现象：在没有中间状态标签的 RLVR 设定中，中间状态标签可以靠“生成-筛选”获得——用当前模型猜测中间状态，再用结果验证器验证最终输出，被验证器接受的轨迹作为伪标签。这说明结果级验证信号可以蒸馏出过程级监督信号，但代价是训练成本（生成状态数）增加。
5. 目标函数敏感性：forward KL 目标下组合性质依然成立（Proposition 6.1），说明主要结论对损失函数选择有一定稳健性；但论文没有声称对所有目标函数（如 reverse KL）都成立，这是一个边界条件。
6. 假设强度权衡：mistake bound 假设比 i.i.d. 可学习性强；在 S={0,1} 的特例下 i.i.d. 可学习性等价于 VC 维有限。说明论文的主要结果建立在比经典 PAC 框架更强的在线学习假设上，这是结果适用性的重要边界。
7. 负结果/开放张力：Discussion 6.2.1 明确追问 Theorem 3.4 的 T 依赖是否为最优（√log T 指数是否可改进），说明作者认为当前 T 依赖可能不是紧的，存在改进空间；这是论文自己标注的未解决张力。

Q6: 有什么可以进一步探索的点？

1. 最优性追问：Theorem 3.4 中监督复杂度的 T 依赖（2^{O(√log T)}）是否最优？能否改进为 poly(log T) 或更紧的界？Discussion 6.2.1 明确把这个问题列为开放问题。
2. 去除马尔可夫假设：现有结果依赖半自动机模拟任务的马尔可夫结构，且要求训练模型本身是马尔可夫的；这排除了能关注完整历史的 Transformer 等非马尔可夫模型类。如何把课程分解理论推广到非马尔可夫架构（如带注意力的模型）是重要方向。
3. 放松 RLVR 覆盖假设：作者在 Discussion 中明确提到“进一步放松 RLVR 结果中的覆盖假设”是后续方向。
4. 更一般的组合机制：当前利用的是半自动机模拟的马尔可夫性；对不具备马尔可夫结构的任务（如需要全局依赖的算法推理），是否存在其他可用的分解原则？
5. KL 目标下的扩展：Proposition 6.1 显示 forward KL 下组合成立；可以进一步研究 reverse KL、总变差距离等目标下组合课程的性质，以及这些目标对错误传播（error propagation）的影响。
6. 推理-训练成本权衡的系统化：论文发现课程压缩训练监督但不压缩推理查询（仍为 Ω(T)）。可以探索是否有一种课程设计能同时压缩训练与推理成本，或证明二者存在不可兼得的权衡。
7. 猜测-检查机制的全面分析：RLVR 设定中 guess-and-check 产生伪标签的代价（生成状态数）未被完全刻画；分析其样本效率、对验证器错误率的敏感度，以及如何与主动学习/探索策略结合，是自然延伸。
8. 从半自动机到真实推理任务：把结果推广到更接近真实 LLM 推理的任务（算术、代码、多步规划），需要处理连续状态空间、随机性、非确定性转移等；论文自己在 Discussion 6.1 承认问题设定相对语言模型推理做了若干简化。
9. 与覆盖原理（coverage principle）更深层的对话：把本文的块长覆盖条件纳入 Foster et al. 2025 的预训练-后训练统一理论，研究课程分解与预训练数据分布的联合设计。
10. 实际算法落地：把理论课程（基于权重 w(X_i) 的拒绝采样构造）转化为可扩展的课程调度算法，并与实践中流行的自课程方法（如基于正确率的难度选择）建立对应关系。

Q7: 总结一下论文的主要内容

本文是 Rajaraman 等人系列工作的第二部分（Part II），目标是为组合泛化——即通过组合简单子问题的解来解决复杂问题——建立严格的学习理论。论文以学习模拟半自动机（semiautomaton simulation）为规范测试平台：给定初始状态和长度 T 的输入序列，预测 T 步顺序计算后的状态；这个任务能捕捉状态追踪、正则语言识别和模运算等顺序计算，并因半自动机可编码分支程序而具备良好的下界基础。

论证主线：论文首先指出，实践经验表明思维链和课程学习（特别是自课程）对推理模型的组合泛化至关重要，但理论上不知道课程何时、为何、以多大程度降低学习成本。为回答这个问题，论文提出基于递归分解的 autocurriculum 方法：把长序列分解成短块，先学习短块转移模型，再把块模型组合成全长模型，从而把学习瓶颈从全长 T 转移到块长 B。

技术主线：论文在两个后训练设定下分析该方法。设定 I（interactive SFT, iSFT）中，学习者有交互式 oracle 提供中间状态反馈；课程方法把监督复杂度降到 2^{O(√log T)}（亚多项式），突破直接模拟的 Ω(T) token 下界。设定 II（RLVR）中，学习者只有预训练参考模型 π_ref 和结果验证器，没有中间状态标签；课程方法通过 guess-and-check（用当前模型猜测中间状态、用验证器筛选）生成伪标签，把对 π_ref 的覆盖要求从全长 T 降到块长 B ≪ T，指数级弱化预训练负担。下界部分利用 Barrington 定理（半自动机编码分支程序）和 Kearns-Valiant 密码学假设论证直接方法的内在困难。

实验/证明主线：论文以定理、下界和命题的形式给出结果，包括 Section 3.4 的正式下界与直接方法对比、Theorem 3.4 的 T 依赖上界（其最优性在 Discussion 6.2.1 被追问）、以及 Proposition 6.1（forward KL 目标下组合依然成立，用 KL 链式法则证明）。讨论部分还系统列出了简化假设（马尔可夫性、mistake bound 假设强于 i.i.d. 可学习性、非马尔可夫模型类被排除）和开放问题。

总体结论：组合结合课程可以在 SFT 和 RLVR 两种范式下相对非课程基线实现监督复杂度的超多项式（superpolynomial）降低，从而为“思维链+课程”这一实践信念提供了第一个严格的理论支撑；但同时承认推理侧查询复杂度仍为线性，训练模型须为马尔可夫，且 T 依赖的最优性未知。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：对智能体（agent）方向：论文关于“验证器筛选猜测轨迹”（guess-and-check）的机制与智能体领域常用的过程奖励/结果奖励、轨迹筛选、自我对弈式数据生成直接相关，可作为奖励设计理论参考

## 基本信息

- 作者：Nived Rajaraman, Audrey Huang, Miroslav Dudik, Robert Schapire, Dylan Foster, Akshay Krishnamurthy
- 机构：未提供
- 来源：未知
- 主题/分类：未知
- 日期：2026-06-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.27721`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Discussion、References 片段，并结合 arXiv 元数据与 heuristic_draft 交叉补充；heuristic_draft 中 main_contributions/experimental_observations 等字段混入了参考文献残片和公式噪声，已在生成时用检索证据与论文结构推断剔除。
