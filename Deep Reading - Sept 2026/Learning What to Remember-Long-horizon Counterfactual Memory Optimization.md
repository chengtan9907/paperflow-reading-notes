---
user_id: "cheng tan"
paper_id: 14528
arxiv_id: "2609.37930v1"
title: "Learning What to Remember: Long-horizon Counterfactual Memory Optimization"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.37930v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37930v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-12T13:05:33"
---
# Learning What to Remember: Long-horizon Counterfactual Memory Optimization

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：persistent memory · credit assignment · counterfactual reward · policy optimization

## 一句话总结

MGPO 通过对相邻记忆状态做反事实对比，把每次记忆改写对未来下游效用的边际贡献转化为直接可优化的学习信号，在文档级信息抽取上提升性能并把平均记忆长度压缩近 80%，且学到的记忆策略可零训练跨领域、跨下游模型迁移。

## 摘要

> Persistent textual memory allows language models to carry information across long interactions, but learning what to remember is fundamentally a credit-assignment problem. A memory rewrite may only become useful many steps later, while much of the observed utility may be inherited from information already stored before the rewrite. We introduce Memory Gain Policy Optimization (MGPO), which isolates the incremental value of each memory rewrite by crediting it for its marginal contribution to current and future downstream utility. This turns delayed memory utility into a direct learning signal for optimizing what information should persist. We study MGPO on document-level information extraction, where structured supervision makes the effects of individual memory updates directly measurable. MGPO improves extraction while reducing average memory length by nearly 80% relative to the initial memory policy before optimization. The learned memory policy also supports reuse and transfer across domains, downstream models without further training. These results show that effective memory learning depends not only on preserving useful information, but on identifying which memory updates create lasting incremental value.

Q1: 这篇论文试图解决什么问题？

持久化记忆是语言模型在长交互中保持信息连续性的基本机制，但记忆容量有界，系统必须学会保留、修订还是丢弃已有内容。本文指出，学习这类更新决策本质上是一个信用分配（credit assignment）问题，其困难来自两个相互叠加的因素。第一是延迟性：一条记忆信息在写入时可能看似无关，却要在很多步之后才被下游任务使用；第二是继承性：即使一条记忆最终被证明有用，其效用也可能大部分来自改写前记忆状态中已存储的信息，而不是最近这次改写本身的贡献。因此，记忆状态效用（state utility）不能揭示最新一次更新到底贡献了多少，现有方法若直接以"最终记忆是否有用"评判改写策略，会把继承效用错误地归因给最近改写，扭曲学习信号。

现有记忆系统主要通过三类手段实现选择性保留：检索（retrieval）、显式记忆管理（explicit memory management）、学习式记忆构建与修订策略（learned policies）。但本文认为这些工作都绕开了核心学习问题——一次记忆更新是否真的创造了下游价值，没有被直接度量。作者主张把"记忆状态的价值"和"记忆改写的增量价值"分开，用反事实对照隔离后者：比较相邻记忆状态下同一 reader 在下游目标上的效用差。

为了让这种反事实度量在实验上可行，论文选择文档级信息抽取（document-level IE）作为测试平台：该场景有结构化监督，单个记忆更新的效果可以直接测量。同时采用"解耦写读"（decoupled writing）设定，记忆 writer 是唯一可训练组件、下游 reader 固定，从而把"学习该记住什么"从"学习怎么回答"中分离出来，使信用可以干净地归因到改写动作上。

核心研究问题可以归纳为：如何把一次记忆改写在当前与未来下游目标上的边际贡献转化为稠密、可估计的学习信号，同时不扭曲原始策略目标？MGPO 的回答是：对每个改写，在其整个剩余复用视界（remaining reuse horizon）上累计它与前一记忆状态在各下游目标上的效用差。

Q2: 有哪些相关研究？

相关工作可分为四块。

持久记忆与选择性保留：Toshniwal et al. (2020)、Wang et al. (2023)、Yu et al. (2026a)、Zhou et al. (2026) 等研究了语言模型如何在有限容量下选择性记忆，奠定了"记忆需要更新决策"的问题背景；本文的增量价值视角是对这类工作的深化。

延迟奖励的信用分配：Arjona-Medina et al. (2019)、Han et al. (2019)、Hung et al. (2019) 处理"行为很久之后才产生回报"的信用分配难题。本文的记忆场景与之同构——改写发生在 t，效用却可能在 j>t 才显现，因此直接借用了延迟回报下的边际贡献思想。

效用归因与 potential shaping：Setlur et al. (2025) 讨论观测效用如何在多个因素之间归因，本文明确指出"最终记忆的效用并不能揭示最新更新贡献多少"正来自这类归因模糊性。MGPO 的信用被描述为"exactly potential-shaped"，即保持事实策略目标不变、只重新分配学习信号；这符合强化学习中 potential-based reward shaping 保持最优策略不变的思想（合理推断，检索片段未显示是否直接引用 Ng et al. 的 shaping 理论）。

记忆系统三大路线：检索式记忆（Guo et al., 2025）、显式记忆管理（Chhikara et al., 2025; Xu et al., 2026; Yu et al., 2026b）、学习式记忆 writer（Yu et al., 2026a; Yan et al., 2026a; Zhou et al., 2026）。本文与第三类最接近，但指出它们没有直接度量改写的增量下游价值，这是论文定位的主要空白。

任务与数据集层面：文档级 IE 上，SciREX（Jain et al., 2020）提供 Method/Task/Material/Metric 实体及其文档级结构标注，是主实验平台；AIPAN-10K（Tang et al., 2026）是网站隐私政策数据实践标注语料，与 SciREX 在领域、抽取 schema、文档长度、标注密度上差异显著，被用作跨域迁移测试。Baseline 方面，检索证据确认 direct readout 类方法包括 Direct Readout 与 R1-RE（Dai et al.），后者合理推断为带推理/自证（reasoning/self-verification）风格的关系抽取方法；其余两组 baseline 的具体构成证据不足，需回原文核对。

Q3: 论文如何解决这个问题？

MGPO 的解决方案分三层。

第一层：形式化流式持久记忆。输入 c1,...,cN 顺序到达，只有有界持久记忆被前向携带；在每一步 t，可学习的记忆 writer πθ 处理上一记忆状态 mt−1 和当前输入 ct，生成更新后的记忆状态 mt∼πθ(·|mt−1,ct)，m0=∅，|mt|≤B。更新后的记忆文本在固定预算 B 下替换旧记忆，因此优化 writer 就是学习信息如何被保留、丢弃或重组。

第二层：解耦写读与状态效用定义。writer πθ 是唯一可训练组件，固定 reader E 处理记忆状态 mt 与下游输入 cj 生成答案 ŷt,j(ξ)=E(mt,cj;ξ)，其中 ξ 表示 reader 的随机性。记忆状态 mt 对目标 j 的效用定义为 Ft,j=Eξ[U(ŷt,j(ξ),yj)]。这里的期望覆盖 reader 的固定采样分布，使效用估计可重复。

第三层：反事实边际信用分配。为隔离第 t 次改写的增量贡献，对每个下游目标 j≥t 计算相邻记忆状态间的效用差 Δt,j=Ft,j−Ft−1,j，并把同一改写在整个剩余复用视界上的所有 Δ 累加为 Memory Gain：MGt=Σ_{j=t}^N Δt,j。Ft,j 构成上三角效用矩阵；论文明确指出 MGt 与 Gt 的区别：MGt 针对单个改写、跨下游目标聚合，而 Gt 聚合当前及后续改写的 Memory Gain（即 Gt=Σ_{k=t} MGk，合理推断），从而既包含当前也包含未来改写的回报。关键理论性质是：该信用是精确 potential-shaped 的，保持事实策略目标不变，只是把学习信号重新分配给真正产生下游效用的改写——这正是它区别于朴素"最终记忆效用"监督的地方。

训练实现：在文档级 IE 上，目标效用被构造成文档级 micro-F1 的加性一阶 surrogate；gold label 被分配到最早可观察的 chunk，使 chunk 级效用可加并与文档级目标对齐。在 held-out 轨迹上，surrogate 与精确文档级 F1 变化高度一致（Spearman ρ=0.988）。由于 reader 固定，反事实效用差可通过在相邻记忆状态下重复采样同一 reader 来估计，从而把延迟效用转化为每步可得的稠密监督。

Q4: 论文做了哪些实验？

训练与评估协议：主实验在 SciREX 上进行，采用训练/开发/测试三划分，开发集选模型、测试集报结果。优化两个任务：salient clustering（对参与报告结果的共指提及按实体身份聚类）和二元关系抽取（salient 实体之间）。文档按 1,024-token 分块，持久记忆上限 256 tokens；除另有说明外，所有学习式记忆策略均使用该配置。评估指标为每种抽取类型的 micro-pooled precision、recall 和 F1。

Baseline 设置：论文对比三组 baseline。检索证据只完整覆盖第一组——direct readout 类方法，包括 Direct Readout 和 R1-RE（Dai et al.）。第二、三组的具体构成在本次检索证据中缺失，合理推断包含非反事实信用分配的记忆策略对照组（例如以最终记忆效用为监督的 writer）和/或检索式记忆 baseline；这两组的构成与公平性需要回原文核对。

跨域迁移实验：AIPAN-10K 作为零训练迁移测试平台——所有记忆策略只在 SciREX 上训练，直接迁移到 AIPAN-10K，不做任何进一步训练或适配。该语料是网站隐私政策的数据实践标注，与 SciREX 在领域（隐私政策 vs 科学论文）、抽取 schema（隐私数据与实践类别 vs 科学实体）、文档长度（更长）与标注密度（更密）上均有显著差异，构成较强的迁移压力测试。

验证设计：surrogate 保真度在 held-out 轨迹上与精确文档级 F1 变化做对比（Spearman ρ=0.988），这是对"用 surrogate 训练不会偏离真实目标"的关键验证。论文还报告了记忆长度（相对初始策略压缩近 80%）以及 writer 向未见 reader、领域和下游任务的迁移情况；但具体数值表、消融矩阵和模型规模等细节在本次检索片段中未完整呈现，需回原文补充。

Q5: 发现了什么实验现象？

实验现象可归纳为五点。

压缩与性能双赢：MGPO 在提升抽取性能的同时，把平均记忆长度相对初始策略压缩近 80%。这是一个反直觉的核心现象——更紧凑的记忆反而带来更好的下游表现，说明选择性遗忘比堆砌信息更有效，增量价值信号引导 writer 只保留"不能从先前状态继承"的关键信息。

增量价值而非总效用驱动：这个结果直接支持论文的核心论点——决定记忆价值的不是最终是否被用到，而是改写创造了多少超出先前状态的边际效用；否则没有理由在压缩 80% 体积的同时还改善提取。

转移能力：学到的记忆策略无需进一步训练即可迁移到未见过的 reader、新领域（SciREX→AIPAN-10K）和下游任务。这合理推断说明 MGPO 学到的是关于"什么信息值得记住"的相对领域无关原则，而非对特定 reader/任务的过拟合；具体迁移收益数值在检索证据中缺失。

长距离证据整合：论文指出 MGPO 学会的记忆支持跨远距离段落（distant passages）的证据整合，与"延迟效用"动机一致——记忆把早期信息带到后期目标，使相隔很远的证据能被联合使用。

Surrogate 保真度：ρ=0.988 表明一阶加性 surrogate 在本文设定下几乎不偏离真实文档级 F1，为训练信号可靠性提供直接证据；但这也意味着该保真度只在本文效用设定下被验证。

负结果与失败案例：检索证据中没有出现失败案例、消融退化或指标间张力的描述；precision/recall 之间是否存在权衡、压缩是否在长文档或稀疏标注文档上损害 recall 等问题证据不足，需回原文确认。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括八点。

任务泛化：把 MGPO 从文档级 IE 推广到对话、指令跟随、agent 长期轨迹等更开放的长视界场景；这些场景的效用更稀疏、更主观，需要重新设计可加性 surrogate 或效用模型。

联合优化：当前 writer 是唯一可训练组件、reader 固定；未来可探索 writer-reader 联合训练，研究信用分配在双方都可变时是否仍保持 potential-shaped 性质，或在何种条件下近似成立。

反事实估计效率：当前反事实比较依赖对同一 reader 的重复采样；对更长生成、更高随机性的 reader，可研究低方差估计器、控制变量或学习式效用模型来降低估计成本。

预算与视界扩展：256-token 记忆与 1,024-token 分块只是单一配置；当记忆预算增大、视界 N 变长时，MGt 累计方差、计算成本与信用稀释如何扩展，值得系统研究。

与检索结合：MGPO 是纯写入端优化，可与检索式记忆叠加，研究"写入什么"与"检索什么"的联合最优策略。

理论分析：potential-shaped 信用的策略不变性在连续记忆状态、非平稳 reader 或近似效用模型下是否依然成立，是一个可做的理论课题。

在线与持续学习：跨领域零训练迁移暗示了持续学习潜力，可在不断到来的新领域上研究记忆重用的长期行为与灾难性遗忘。

评估协议：引入更长交互的人工评测或真实用户研究，验证记忆压缩在端到端任务体验中的影响，而不仅是离线 F1。

Q7: 总结一下论文的主要内容

论文研究的核心问题是：语言模型在有界持久记忆下如何学习"该记住什么"。作者把这个问题重新表述为信用分配问题——记忆改写的价值可能延迟出现，且最终效用可能大部分继承自已有的记忆状态，因此"记忆状态是否有用"回答不了"这次改写贡献了多少"。现有记忆系统（检索式、显式管理、学习式策略）都没有直接度量改写的增量下游价值，这是论文要填补的空白。

论证主线：在流式输入设定下，可学习 writer πθ 在每个时间步用新输入和旧记忆生成新记忆（|mt|≤B），固定 reader E 用记忆回答下游查询。定义状态效用 Ft,j=Eξ[U(ŷt,j,yj)] 后，论文指出该量混淆继承与贡献，因此对每个改写定义 Memory Gain：MGt=Σ_{j=t}^N (Ft,j−Ft−1,j)，把同一改写相对前一记忆状态在所有剩余下游目标上的效用差累计起来。这个信用被证明是精确 potential-shaped 的：它保持事实策略目标不变，只把学习信号重新分配到产生下游效用的改写上。作者据此把"学习什么该记住"操作化为"优化每个改写的边际贡献"。

技术主线：为了让延迟效用变成可计算的稠密监督，论文选择文档级信息抽取作为测试平台，把文档级 micro-F1 构造成加性一阶 surrogate，gold label 分配到最早可观察 chunk，使反事实效用差逐 chunk 可算；在 held-out 轨迹上 surrogate 与精确 F1 变化高度一致（Spearman ρ=0.988），验证了替代目标的保真度。reader 固定使反事实比较只需对相邻记忆状态重复采样同一 reader，从而把"如果没做这次改写会怎样"的对照变成可操作的估计。

实验主线：在 SciREX 上训练两个任务（salient clustering 与二元关系抽取），文档按 1,024-token 分块、记忆上限 256 tokens，与 direct readout 类 baseline（Direct Readout、R1-RE Dai et al.）等三组方法对比；在 AIPAN-10K 隐私政策语料上做零训练跨域迁移。结果上，MGPO 在提升抽取性能的同时把平均记忆长度压缩近 80%；学到的记忆策略可迁移到未见 reader、领域与下游任务，并支持跨远距离段落的证据整合。

结论：有效记忆学习不仅在于保留有用信息，更在于识别哪些记忆更新创造了持久的增量价值。论文指出，这种长视界边际信用产生更选择性、更紧凑的记忆，并把"记住什么"从启发式问题变成可优化的学习目标。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户画像中 generation（权重 0.10）方向直接相关：持久记忆是长文本生成与交互系统的核心组件

## 基本信息

- 作者：Jiaming Tang, Mingyan Liu, Armin Sarabi
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-09-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37930v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（66 个 chunk 中的 6 处命中，主要覆盖 Abstract、Introduction、Method、Results、Conclusion），并结合 heuristic_draft 与论文元数据补全；baseline 后两组构成、消融矩阵、具体数值表等细节证据不足，已在相应字段标注需回原文核对。
