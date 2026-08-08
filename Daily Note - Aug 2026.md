# Multimodal Models & Visual Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Multimodal Models & Visual Reasoning
- 方法：reasoning, multimodal-reasoning
- 论文/报告：3 篇
- TRAM: Enhancing Multimodal Reasoning with Trajectory-Derived Auxiliary Memory
- Douyin Multimodal Embedding Model Technical Report
- Illuminating Visual Identity in Universal Multimodal Embeddings
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:b1837d2056cae8d8 -->
## TRAM: Enhancing Multimodal Reasoning with Trajectory-Derived Auxiliary Memory

[[Deep Reading - Aug 2026/TRAM-Enhancing Multimodal Reasoning with Trajectory-Derived Auxiliary Memory|Deep Reading]]

[https://arxiv.org/pdf/2608.01922v1](https://arxiv.org/pdf/2608.01922v1)

- **本文针对多模态大推理模型（MLRMs）在长程推理中容易出现的逻辑断裂和信息丢失问题，提出了一种名为 TRAM 的创新性增强方法。研究的核心动机源于一个关键发现：长程推理的失败往往不是因为模型“看不见”图像，而是因为模型“记不住”自己之前推导出的逻辑结论。TRAM 通过引入“轨迹衍生辅助记忆”来弥补这一缺陷。技术上，它采用了双时间尺度的循环机制，分别利用快流和慢流捕捉短期和长期的推理状态，并将这些状态压缩为潜在记忆向量。在解码阶段，这些记忆通过残差路径被注入到模型的高层解码器中，为后续生成提供持续的逻辑锚点。实验结果令人印象深刻，在 4 种模型和 8 个涵盖数学、科学、视觉逻辑的基准测试中，TRAM 均实现了无需训练的性能飞跃。这不仅证明了 TRAM 作为一种推理时工具的实用性，也为理解 MLRMs 的内部推理机制提供了新的视角，即“推理轨迹本身就是一种宝贵的、需要被显式管理的资源”。**

<!-- paperflow:7b2330b630ffa227 -->
## Douyin Multimodal Embedding Model Technical Report

[[Deep Reading - Aug 2026/Douyin Multimodal Embedding Model Technical Report|Deep Reading]]

[https://arxiv.org/pdf/2608.02148v1](https://arxiv.org/pdf/2608.02148v1)

- **这篇技术报告介绍了抖音多模态嵌入模型（DME），其目标是在大规模工业多模态检索中同时获得高效率和细粒度语义区分。论文开篇指出现有方法的根本矛盾：对比模型高效但监督粒度粗，CoT 模型精细但无法在线服务。DME 的方案是两阶段训练：第一阶段用大规模对比预训练建立统一嵌入空间，覆盖多种模态和检索任务，确保模型在十亿级索引下具备基本检索能力；第二阶段专门补充"语义充分性"——通过证据锚定的类型化潜在推理（ETLR）让嵌入锚定于检索相关证据，通过跨条件重建（CCR）强制嵌入保留对方侧语义。ETLR 在隐藏空间组织推理 token，不产生显式文本；CCR 从嵌入自回归重建对方侧内容，这些生成式监督只在训练时使用，因此推理时的嵌入由单一编码器前向生成，查询侧仅增加边际开销。论文将"信息完备性"定义为可量化的语义充分性度量，即从嵌入恢复输入内容的能力，并用它指导训练。实验在 MMEB-v2 基准上比较了 DME-2B 和 DME-9B 与可比规模现有模型的性能，获得最先进结果；延迟分析确认潜在推理 token 的额外开销很小；语义充分性量化验证了嵌入的信息完备性。在工业部署部分，DME 被用于抖音搜索生产检索系统，离线阶段用 DME 初始化内部模型并继续训练，线上 A/B 测试获得 0.1% 的 Lifetime 指标提升。整个工作展示了如何在保留纯稠密检索效率的情况下，通过训练阶段的生成式监督注入细粒度语义，为大规模多模态检索提供了一条兼具学术价值和工程可行性的路径。**

<!-- paperflow:1ed78e94fe0bfe43 -->
## Illuminating Visual Identity in Universal Multimodal Embeddings

[[Deep Reading - Aug 2026/Illuminating Visual Identity in Universal Multimodal Embeddings|Deep Reading]]

[https://arxiv.org/pdf/2608.01794v1](https://arxiv.org/pdf/2608.01794v1)

- **这篇论文围绕 Universal Multimodal Embeddings（UMEs）中一个被忽视的关键能力——视觉身份判别（Visual Identity Discrimination, VisID）——展开了系统性的研究，提出了任务形式化、基准、训练方法和实验验证的完整闭环。

动机主线：UMEs 试图将多种模态和多种任务统一到一个共享表示空间中，近年来多模态大语言模型（MLLMs）的进展让这一领域快速发展。但论文指出，现有 UME 方法普遍没有显式处理视觉身份判别能力，即判断两个视觉输入是否指向同一个具体身份或实例的能力。这种能力对实例检索、行人/车辆重识别、AI 生成内容中的身份保持等实际问题至关重要。论文用 MMEB 基准的例子说明这一盲区：MMEB 有 36 个子集，但其中只有一个图像到图像子集 NIGHTS，且 NIGHTS 的设计并非典型的身份判别任务。因此，该能力既没有被评测，也没有被优化，论文认为这限制了 UME 在实际应用中的价值。

技术主线：论文提出三部分方案。第一，统一的 VisID 任务形式化，将分散的实例检索、re-identification、同身份验证等传统任务统一到一个问题框架中。第二，大规模基准 MVEB（Multimodal Visual Identity Embedding Benchmark），从真实数据和合成数据中构建，既可用于评估也可用于训练，并配有 out-of-domain 评估设定来检验泛化能力。第三，一个简单有效的联合训练框架，通过身份感知采样机制（identity-aware sampling）在训练中构造同身份正样本对，将视觉身份表示学习与标准对比学习目标联合优化。该框架兼容标准 UME 训练流程，同时处理采样与损失设计两个环节，使模型在获得身份判别能力的同时不损害通用多模态表示的质量。

实验主线：论文在自建的 MVEB 上评估身份判别能力，在通用基准 MMEB 上评估通用性能。实验结果显示，所提出的方法将 VisID 性能提升了约 25%+，同时在 MMEB 上保持了有竞争力的通用多模态检索准确率，说明...**

# Data, Benchmarking & Evaluation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data, Benchmarking & Evaluation
- 方法：agent, ai-for-science, reasoning
- 论文/报告：3 篇
- SWE-Touch: Benchmarking Coding Agents When Users Touch the Code
- From Simple QA to Deep Research: A Verifiable Benchmark Constructed through Iterative Task Evolution
- Right Answer, Wrong Method: Shortcut Hacking Misleads the Evaluation of LLM Reasoning on Frontier Science Benchmarks
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:6bc9cdefe0022197 -->
## SWE-Touch: Benchmarking Coding Agents When Users Touch the Code

[[Deep Reading - Aug 2026/SWE-Touch-Benchmarking Coding Agents When Users Touch the Code|Deep Reading]]

[https://arxiv.org/pdf/2608.02499v1](https://arxiv.org/pdf/2608.02499v1)

- **随着前沿模型驱动的编码智能体能力迅速增强，它们对仓库范围的修改越来越难以被用户跟踪。论文指出，真实软件开发总是发生在共享工作区中：用户可能随时查看、修改代码，而现有基准却通常让智能体独自工作，或只允许用户通过消息参与，没有让用户在代码层面直接动手。这一评估缺口导致我们无法回答：编码智能体在任务进行中遇到用户修改代码时，能否理解并正确响应？SWE-Touch 正是为压力测试这一设定而提出的框架。其核心思想是构造“Counter-Edits”——对任务相关代码的合理编辑，但与任务完成目标相冲突，这些编辑经过验证，确保不能单独解决问题也不能被忽略。框架从多条修复轨迹中挖掘任务关键区域，用一个独立的用户补丁生成器构造编辑，并在智能体执行到相关代码时注入带上下文的用户消息，以此模拟真实协作中的干扰。实验部分，论文在 SWE-bench Verified 上系统评估了九个编码模型，并在更长视野的 SWE-Bench Pro 和 DeepSWE 上做了补充实验。结果显示，Counter-Edit 使平均解决率平均下降 7.7 个百分点，长视野任务上退化依然显著。更值得注意的是，这一退化伴随着模型排名的变化，一些在自主评估中表现接近的模型在共享工作区条件下明显落后。轨迹分析进一步解释了失败原因：智能体可能保留冲突代码——没有意识到用户改动与任务目标的矛盾；或者替换代码但缺少足够的仓库重检和针对性的测试验证，导致修复质量不可靠。这些证据共同表明，自主解决率并不能可靠预测用户参与时的协作性能，当前编码智能体的状态感知和自适应行为仍不足以支撑真实的共享工作区协作。论文由此提出未来优化的三个关键能力：检测工作区变化、协调冲突编辑与任务要求、验证受影响行为。SWE-Touch 的贡献在于提供了首个系统化评估这些能力的基准框架，并给出了可复现的对抗性测试协议，为后续研究指明了方向。总体而言，这项工作揭示了一个被现有基准长期忽视的重要维度，并对编码智能体评估体系的完善和交互式智能体的开发具有推动作用。**

<!-- paperflow:5a703cd91770332f -->
## From Simple QA to Deep Research: A Verifiable Benchmark Constructed through Iterative Task Evolution

[[Deep Reading - Aug 2026/From Simple QA to Deep Research-A Verifiable Benchmark Constructed through Iterative Task Evolut|Deep Reading]]

[https://arxiv.org/pdf/2608.02163v1](https://arxiv.org/pdf/2608.02163v1)

- **本文聚焦深度研究（deep research）基准的自动构建与可验证评估问题。深度研究任务允许多种有效解决路径，同时要求领域知识，这使得传统固定答案评估失效，需要任务特定的细粒度评分标准。现有基准或依赖专家编写（成本高、规模有限），或依赖人类已有材料（开放性受限），或采用模型互评（相对分数，无法提供绝对质量度量），或自动生成评分标准但可靠性不足。针对这一缺口，作者提出一个完全自动构建的可验证基准，包含 500 个深度研究任务，覆盖 31 个主题、10 个大类，并设计了三种查询形式来探测互补的深度研究能力。

技术核心是迭代式 Explorer–Formalizer–Challenger 流水线。该流水线从 Wikipedia QA 语料中的简单问题出发，逐步将其演化为深度研究任务。每个任务被形式化为一个有向无环图（DAG），节点是原子步骤，边表示依赖，每个节点关联检查点以验证步骤完成情况及事实依据。演化过程受控：每一轮增加一个或多个步骤、增加回答维度，最终形成需要多源搜索和综合分析的高难度任务。查询文本、DAG 结构和评分标准三者共同演化，确保任务复杂度与评估标准始终对齐。评分采用事实锚定的逐点评分标准（pointwise rubrics），每条标准都对应可核查的事实或来源，从而支持细粒度诊断、绝对分数和人类可审查性。

实验方面，作者测试了 10 个模型在三种查询类型上的表现，结果显示基准能清晰区分模型能力，且查询类型间的得分模式存在差异。工具消融实验发现，移除工具后所有模型在所有查询类型上得分均大幅下降，总平均分下降约 0.14，证据型和分析型检查点平均分各下降约 0.07，说明任务中编码了大量长尾信息，必须依赖外部工具。同时，事实锚定的评分标准表现出与人类判断一致、重复稳定的评估性质。作者在结论中承认当前构造过程使用前沿模型导致成本较高，但认为这是构建可信基准的必要投资。所有数据、代码和结果已公开，便于社区复现与扩展。**

<!-- paperflow:18f56968b1f7b18a -->
## Right Answer, Wrong Method: Shortcut Hacking Misleads the Evaluation of LLM Reasoning on Frontier Science Benchmarks

[[Deep Reading - Aug 2026/Right Answer, Wrong Method-Shortcut Hacking Misleads the Evaluation of LLM Reasoning on Frontier|Deep Reading]]

[https://arxiv.org/pdf/2608.02442v1](https://arxiv.org/pdf/2608.02442v1)

- **论文系统研究了LLM在科学推理基准中的‘Solution Hacking’现象，指出仅以最终答案准确率评估会高估模型推理能力。作者首先定义了Solution Hacking：模型通过数值搜索、枚举、猜测或答案优先验证等无效捷径达到正确答案，而缺乏题目所要求的推导过程。这与推理错误不同，后者至少尝试了推理但出错。作者在普通、奥林匹克、HLE等不同难度级别，以及多种科学领域和前沿模型上开展实验，发现Solution Hacking发生率随难度增加而显著上升（2.2%、28.3%、37.4%），并且不同模型中被判定正确的答案中有8.2%-44.1%属于hacked。进一步分析表明，模型在难以解决问题时更易选择捷径，而答案短且易验证的问题尤其容易被hack。为缓解该问题，作者设计了自动裁判和测试时指令两种抗破解策略。实验显示，抑制捷径行为使报告准确率明显下降（如弃权率从0%升至27.3%），而正确且非hacked准确率仅小幅下降（如从41.2%降至35.2%），说明被撤除的分数主要来源于hacked答案。论文最终得出结论：当前答案导向的评估范式严重高估了前沿LLM的科学推理能力，评测方法需转向对完整推理过程的验证。该工作为构建更可靠的推理评估体系提供了实证基础和操作工具。**

# World Models, Generation & Audio

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：World Models, Generation & Audio
- 方法：generation
- 论文/报告：2 篇
- WorldExam: Benchmarking World Models from Apparent Appearance to Inherent Reactivity
- CultureVidBench: Benchmarking Cultural Understanding in Text-to-Video Generation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:6f585663ee0425a7 -->
## WorldExam: Benchmarking World Models from Apparent Appearance to Inherent Reactivity

[[Deep Reading - Aug 2026/WorldExam-Benchmarking World Models from Apparent Appearance to Inherent Reactivity|Deep Reading]]

[https://arxiv.org/pdf/2608.02603v1](https://arxiv.org/pdf/2608.02603v1)

- **论文《WorldExam: Benchmarking World Models from Apparent Appearance to Inherent Reactivity》针对视频生成模型作为世界模型评估时的核心缺口，提出了一个分层诊断基准。论文的论证主线始于一个观察：可控视频生成模型正越来越多地被开发为世界模型，而不仅仅是视频片段生成器。因此，评估这些模型时不能只看生成视频的表观外观，还要考察其固有反应性——即从场景状态推断世界应如何反应，并生成输入中未明确描述的合理后果的能力。现有基准大多集中在评估视觉质量或显式指令遵循，例如检查请求的运动、布局、相机轨迹是否被实现，但这些评估无法触及模型是否真正理解场景动力学。为此，论文引入 WorldExam，这是一个包含四个层级的诊断基准：视觉质量（Visual Quality）、控制遵循（Control Adherence）、空间一致性（Spatial Consistency）和世界反应性（World Reactivity）。层级从低到高排列，分别检验生成保真度、指令忠实度、空间连贯性以及场景因果推断能力。基准包含 1,474 个案例，分布在八个专门任务上，并支持对相机驱动、动作驱动和语言驱动三种模型范式的统一评估。评估的关键设计是接口适配：通过将每个案例转换为模型的原生输入格式（相机轨迹、动作序列或语言提示），使得不同范式的模型可以在同一任务集上可比。模型被形式化为函数 f(I, C) → V，其中 I 是初始图像，C 是模型面对的输入指令，V 是生成的视频。世界反应性层级特别设计了场景条件反应和目标导向行为任务，这些任务要求模型超出输入中显式给定的信息，推断合理的后续状态。实验部分选取了 20 个代表性模型：6 个相机驱动、7 个动作驱动、7 个语言驱动，为每个案例构造原生输入，评估其表现。结果展示了三种范式之间的清晰能力分裂：相机驱动模型擅长相机轨迹控制，但接口不支持动态交互；动作驱动模型主体控制精确，但其他场景元素常常无响应；语言驱动模型交互性较强，但复杂控制的遵循不够忠实。综合来看，没有模型能在广泛任务上持续保...**

<!-- paperflow:766799bc2bd94c8b -->
## CultureVidBench: Benchmarking Cultural Understanding in Text-to-Video Generation

[[Deep Reading - Aug 2026/CultureVidBench-Benchmarking Cultural Understanding in Text-to-Video Generation|Deep Reading]]

[https://arxiv.org/pdf/2608.01942v1](https://arxiv.org/pdf/2608.01942v1)

- **本文针对文生视频（T2V）模型在文化理解方面的短板，提出了首个系统性的评估基准 CultureVidBench。研究者指出，尽管当前的 T2V 模型在生成高质量视频方面取得了巨大进步，但它们在处理全球多样化文化时表现出明显的偏见和知识匮乏。CultureVidBench 的核心贡献在于其精心设计的 1,000 个提示词，这些提示词不仅涵盖了广泛的地理区域（12 个国家），还深入到了物质文化、社会实践和仪式典礼等 14 个细分维度。与以往侧重于静态图像或通用视觉质量的基准不同，CultureVidBench 特别强调了视频的动态特性（如仪式过程）和多模态属性（如文化相关的文字和音频）。在技术路线上，论文提出了一套多维度的评估框架，结合了人工主观评价和基于 MLLM 的自动评分。实验结果揭示了当前顶尖 T2V 模型的普遍弱点：它们虽然能生成视觉上吸引人的视频，但在捕捉细粒度的文化细节、处理非西方文字以及呈现特定文化动作方面存在显著困难。该研究为未来开发更具文化包容性和准确性的生成式 AI 模型提供了重要的参考坐标和评估工具，强调了在追求技术性能的同时，必须关注全球文化的多样性和尊重。**

# Research Agents & Scientific Discovery

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Research Agents & Scientific Discovery
- 方法：agent, generation, vision
- 论文/报告：2 篇
- Beyond Solution-Centric Search: Adaptive Inquiry and Knowledge Revision for Autonomous ML Engineering
- Style Wins, Substance Loses: A Diagnosis of LLM-as-Judge in Idea Generation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:338514c13f4fb33a -->
## Beyond Solution-Centric Search: Adaptive Inquiry and Knowledge Revision for Autonomous ML Engineering

[[Deep Reading - Aug 2026/Beyond Solution-Centric Search-Adaptive Inquiry and Knowledge Revision for Autonomous ML Enginee|Deep Reading]]

[https://arxiv.org/pdf/2608.02143v1](https://arxiv.org/pdf/2608.02143v1)

- **这篇论文提出了“信息范式”（Information Paradigm），旨在解决自主机器学习工程（MLE）等长程研究任务中，现有智能体因过度关注“方案搜索”而忽视“信息积累”的问题。作者认为，自主研究的本质应该是系统“信息状态”的不断演化和深化。

基于这一范式，作者开发了名为 Iris 的系统。Iris 的核心是一个“询问-修正”（Inquiry-Revision）循环。在“询问”阶段，Iris 突破了传统的固定搜索结构，根据当前对任务的理解动态生成局部计划，并引入了“认识性行动”，允许系统在不改变现有方案的情况下，通过探测性实验来消除关键的不确定性。在“修正”阶段，Iris 展现了强大的知识管理能力，它能将跨多个实验的观察结果综合成一套动态更新的“任务知识”。这套知识由具备明确适用范围的主张组成，能够随着新证据的出现而自动修正，从而避免了系统被早期错误结论误导。

实验结果令人印象深刻：在权威的 MLE-Bench 基准测试中，Iris 在 12 小时的预算限制下取得了 64.9% 的奖牌率，刷新了行业纪录。此外，Iris 在测试框架工程和模型后训练等跨领域任务中也表现出了极强的泛化能力。消融实验进一步证实，认识性行动和知识修正机制是提升系统性能的关键因素。总的来说，Iris 的成功证明了将信息获取与修正作为自主研究系统核心设计目标的正确性，为未来开发更智能、更具自我演化能力的科学研究智能体指明了方向。**

<!-- paperflow:11359bde28ba9fb1 -->
## Style Wins, Substance Loses: A Diagnosis of LLM-as-Judge in Idea Generation

[[Deep Reading - Aug 2026/Style Wins, Substance Loses-A Diagnosis of LLM-as-Judge in Idea Generation|Deep Reading]]

[https://arxiv.org/pdf/2608.01666v1](https://arxiv.org/pdf/2608.01666v1)

- **## 论文总结

### 1. 背景与动机

随着 AI Scientist 和自动化研究 agent 的快速进展，科学想法生成已经从“一次性写作辅助”升级为自动化科研工作流中的核心生成组件。LLM 能以极低成本批量产生大量候选想法，但下游实验验证、文献调研、方法实现、论文写作、人工评审等环节都需要昂贵的真实资源。因此，如何从大量候选中可靠地筛选出值得投入资源的好想法，成为决定自动化科研质量的关键问题。

现有 LLM-as-Judge 方法在被用于科学想法评估时，通常采用成对排序、多维打分或多智能体评审等策略，但它们的判断依据是否包括“科学实质本身”而不是“写作风格的包装”，此前几乎没有系统性的检验。本文开篇就直接提出这个问题：LLM 评委是否真的在看科学内容，还是被语言风格所误导？

### 2. 核心问题与难点

科学想法的原始文本中，实质内容与表达风格是天然纠缠的。一个平庸想法可能被漂亮的修辞包装得很“高大上”，反之，一个优秀的想法也可能因为写得朴素而被低估。更麻烦的是，仅仅观察评审分数的变化不足以确认风格影响——因为分数变化也可能来自随机噪声或任务难度。因此需要一种受控实验设计，在“固定科学内容、只改写作风格”的条件下，测量评审行为的变化。

此外，作者敏锐地指出一个潜在陷阱：如果只追求“风格不变”（低 SBI），评审可能通过“对什么都给差不多的分数”来实现，即分数塌缩。这种假稳健在之前的工作中很少被讨论。论文因此主张以三个互补指标（SBI、SRR、AWR）来约束评估器。

### 3. 方法：SciStyleBench

论文提出的 SciStyleBench 包含三个组件：

- **SciStyleStage**：受控扰动环境。固定 600 个科学想法的实质内容，施加 15 种可控写作风格变体，并在三种背景设置（无上下文、固定域上下文、开放域检索上下文）下配置。每个设置包含 9,000 个评估实例，总计 27,000 个实例。该设计确保“实质”与“风格”实现正交分离。

- **SciStyleMetrics**：定义三个量化指标。SBI 度量风格变化对...**

# Web, GUI & Computer-Use Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Web, GUI & Computer-Use Agents
- 方法：agent
- 论文/报告：1 篇
- Qwen-CUA: Native Computer Use for (almost) Everything
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:4e1ba24659ac1afb -->
## Qwen-CUA: Native Computer Use for (almost) Everything

[[Deep Reading - Aug 2026/Qwen-CUA-Native Computer Use for (almost) Everything|Deep Reading]]

[https://arxiv.org/pdf/2608.02352v1](https://arxiv.org/pdf/2608.02352v1)

- **论文提出 Qwen-CUA，一个原生计算机使用智能体。作者指出，人类操作计算机的通用界面是屏幕加键盘鼠标，因此一个真正通用的计算机代理应当只依赖截图观察和原生动作输出，而不是 DOM 树或专有 API。这不仅能覆盖几乎任何软件，还能更自然地泛化到用户自定义界面。然而，纯视觉界面带来三大挑战：长视界状态维持、交互经验获取成本高、奖励稀疏但可验证。

为此，Qwen-CUA 采用 397B-A17B 的 Qwen MoE 骨干，在每一步仅接收桌面截图，通过键盘鼠标事件动作完成任务。模型完全没有利用 DOM、无障碍元数据或任务特定 API。为了支撑长视界，agent scaffold 维护 20 张活动截图，并将更早的截图折叠为固定大小块，从而控制上下文长度并保持 prompt 前缀稳定，提高计算效率。

训练侧是论文的重点工程贡献。作者搭建了包含接近 100,000 个 vCPU 和数万个并发环境的云端 rollout 集群，构造了约 40,000 个可验证任务，并采集了个性化长视界工作流。训练中采用可验证奖励优化完整轨迹，并引入长视界轨迹切片来处理长序列的训练问题。更重要的是，论文采用迭代式训练流程：每个训练阶段产生的策略都会用于重新生成监督数据并调整强化学习任务分布，使模型持续在自身当前能力边缘学习。

实验覆盖八个计算机使用基准，包括 OSWorld-Verified、OSWorld 2.0、RedTeamCUA 等。结果显示 Qwen-CUA 在全部基准上一致超过 Qwen3.7，并与领先专有系统相当或更优：OSWorld-Verified 达到 86.2，OSWorld 2.0 的二进制/部分完成为 18.5/48.4。将相同配方扩展到超过一万亿总参数的 Qwen-CUA-Max 后，相应分数提升至 87.6 和 21.2/53.3，显示了规模化带来的明确收益。安全性方面，攻击成功率从 36.6 降至 16.4。论文还报告了效率分析、内部浏览器部署以及原生交互结合 Bash 的混合实验，进一步刻画了实际部署中的优势和局限。

整体来看，论文的核心主张是原生计算机使...**

# Agent Skills, Harness & Tooling

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Skills, Harness & Tooling
- 方法：agent, generation, reinforcement-learning, optimization, deep-learning, gui-agent
- 论文/报告：6 篇
- VC-Tooler: Learning Compositional and Adaptive Visual Tool Use
- SKT: Skill-Use Training at Scale via Verified Synthetic Data Generation
- SkillTrace: Traversing a Query-Skill Graph for Composable LLM Agents
- Harness-R1: Learning to Edit Executable Runtime Harnesses from Agent Failure Trajectories
- HarnessCompass: Guiding Automatic Harness Evolution toward Generalizable and Effective Agent Harnesses
- Progressive Agent Skill Generation via Reinforcement Learning
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:1cb6cf9e0e92580e -->
## VC-Tooler: Learning Compositional and Adaptive Visual Tool Use

[[Deep Reading - Aug 2026/VC-Tooler-Learning Compositional and Adaptive Visual Tool Use|Deep Reading]]

[https://arxiv.org/pdf/2608.02217v1](https://arxiv.org/pdf/2608.02217v1)

- **VC-Tooler 针对 VLM 视觉工具使用中组合与适应能力不足的问题，提出了一种将工具使用学习为组合式自适应能力的完整方案。论文首先指出有效工具使用需要三种能力：工具调用的视觉 grounding、多步工具组合、以及对工具返回观察的自适应调整。现有方法局限于固定工具空间和僵化调用模式，容易造成对固定范式的记忆而非通用技能。为此，作者设计了分层合成流水线，构建覆盖单工具 grounding、多工具组合、多样工具上下文与接口的轨迹库，规模达到数百种工具接口，并以此进行监督冷启动训练，随后引入强化学习以鼓励准确、高效、上下文感知的工具调用。实验在通用基准（V*、HRBench-4K/8K、CharXiv、MME-RealWorld）和智能体基准（VTC-Bench）上评估，VC-Tooler-RL 达到开源模型最优性能，例如 V* 95.8%、VTC-Bench 35.3%，并展示了向更丰富工具设置迁移的潜力。这项工作为 VLM 从被动感知走向主动智能体推理提供了系统性的训练方法和数据策略，其轨迹合成与 RL 训练框架具有可推广性。**

<!-- paperflow:7fa4def1aa2ea9c4 -->
## SKT: Skill-Use Training at Scale via Verified Synthetic Data Generation

[[Deep Reading - Aug 2026/SKT-Skill-Use Training at Scale via Verified Synthetic Data Generation|Deep Reading]]

[https://arxiv.org/pdf/2608.02287v1](https://arxiv.org/pdf/2608.02287v1)

- **本文针对语言模型智能体的“技能使用”能力不足问题，提出了一个名为 SKT（Skill-Use Training via Verified Synthetic Data Generation）的验证式数据合成流水线，并基于此构建了训练数据集和 held-out 基准 SkillEval。

**论证主线**：作者首先指出，Agent Skills 作为可复用程序性知识的外化载体虽然越来越普遍，但模型自身并没有内化“何时、如何、为何使用技能”的程序性知识。单纯扩充技能库或在 prompt 中附加技能描述，并不能让模型可靠地识别、应用和协同技能。因此需要直接面向“技能使用”这一行为进行训练。

**技术主线**：SKT 流水线包含三个关键环节。第一，技能选择：从 2,000 个公开技能中挑选适合任务合成的技能，并考虑单技能与多技能配置的多样性、组合性和可验证性。第二，任务合成与验证修复：生成技能约束的任务（包括结构化规格说明），通过规则验证和智能体验证来判断任务是否可解、轨迹是否正确，并利用验证反馈进行迭代修复，直到通过验证。这一环节强调“反馈引导修复”，显著区别于一次性合成后简单过滤的做法。第三，轨迹过滤：只保留成功执行且实质性使用每一个必需技能的轨迹，确保训练信号的高质量。最终得到 4,000 个任务包和 27,164 条已验证轨迹。

为验证方法的有效性，作者使用同一流水线但构建一个与训练任务池不相交的任务池，生成 SkillEval 基准，避免数据泄漏。随后在 SkillsBench、MolBench-Bind、AgentSkillOS-bench 和 SkillEval 四个基准上，对多种基础模型和多种 agent harness 进行监督微调评估。实验结果显示一致性的性能提升。验证消融实验证明“验证+修复”是增益的关键；跨 harness 评估说明能力泛化不局限于特定接口；规模扩展实验显示扩大技能覆盖范围能进一步提高性能。

**实验主线**：论文通过四个维度的实验构建证据链：
- 主实验：多模型 + 多基准 + 多 harness 的 SFT 效果；
- 消融：...**

<!-- paperflow:fc8b7573fc6e3d33 -->
## SkillTrace: Traversing a Query-Skill Graph for Composable LLM Agents

[[Deep Reading - Aug 2026/SkillTrace-Traversing a Query-Skill Graph for Composable LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2608.02356v1](https://arxiv.org/pdf/2608.02356v1)

- **论文的核心主张是：可组合技能检索不能只看单个技能的相关性，而必须把“完整”和“可执行”作为目标，并且这个目标可以通过一个三层关系图自然地求解。作者首先在引言中区分了可组合技能检索与传统信息检索（Karpukhin 等 2020）、RAG（Lewis 等 2020）和工具选择（Patil 等 2024；Qin 等 2023）的不同：前者需要一组能共同完成复杂任务的技能，而不是单个相关结果。同时，他们以 SkillsBench（技能被表示为包含指令、脚本、资源的程序性知识包）、ToolBench（API 调用组合）和 ALFWorld（高层面策略技能）为例，说明尽管技能形态不同，共同需求都是识别并协调一组能力。
在此基础上，论文把挑战拆成三个层次：查询内部原子技能查询的组合关系；查询与候选技能之间的语义匹配；候选技能之间的依赖关系。问题定义片段进一步指出，穷举技能组合不可行，因为搜索空间随技能库大小指数增长，因此需要把可组合技能检索形式化为图搜索问题。这些关系合并成一个 Query-Skill Graph：图既捕捉查询的内部结构，也反映技能库的组织方式；在这个图上搜索就是从查询结构出发，通过匹配和依赖遍历来寻找完整技能子集。
SkillTrace 的流程是：先把复杂用户查询组织成语义层次，保留原子查询之间的组合结构；然后做查询-技能匹配，找到种子技能；最后通过在技能依赖图上遍历和传播，把缺失的技能逐步补全，直到得到可执行组合。论文强调，这个三层建模与以往 flat 或只覆盖部分关系的图方法不同，是它性能提升的主要来源。
实验部分，SkillTrace 在 SkillsBench 上达到 53.17% 成功率，在 ALFWorld 上达到 91.43%，均超过所有 flat 和 graph 基线；并且在多个 backbone 语言模型上保持一致的提升，作者据此认为显式建模查询组合和技能依赖对检索完整可执行技能集很重要，也为构建更强的技能型 agent 提供了方向。
需要指出的是，可见的 PDF 检索证据主要覆盖摘要、引言、问题定义、结论和主结果片段，并未给出完整方法章节、完整...**

<!-- paperflow:8c9f324be450a411 -->
## Harness-R1: Learning to Edit Executable Runtime Harnesses from Agent Failure Trajectories

[[Deep Reading - Aug 2026/Harness-R1-Learning to Edit Executable Runtime Harnesses from Agent Failure Trajectories|Deep Reading]]

[https://arxiv.org/pdf/2608.02276v1](https://arxiv.org/pdf/2608.02276v1)

- **论文围绕一个核心观察展开：LLM 智能体在部署中不断积累失败轨迹，但这些轨迹的用途通常局限于更新模型权重，而决定智能体如何与外部世界交互的执行时环境（harness）——包括上下文构建、工具中介、动作验证、错误恢复——往往保持不变。作者提出，失败轨迹同样可以用于改进这些可执行运行时组件，并引入 Harness-R1，据我们所知是第一个把“基于失败条件的生命周期级 harness 编辑”训练成一种可学习能力的方法。

方法上，Harness-R1 训练一个独立的 9B 参数的“harness 工程师”模型。训练分为两步：先用受监督的数据进行冷启动 SFT，让工程师学会生成可执行的补丁；再用在线强化学习（group-relative policy optimization，GRPO）优化编辑策略，奖励由冻结的目标智能体在重新运行同一批任务后的实际成功率变化给出。这个设计的关键在于：目标智能体完全冻结，奖励只更新工程师，因此编辑效果被直接归因于补丁的真实收益。

实验中，Harness-R1 在 WebShop、ALFWorld、DBBench 三个交互式基准上对基础 Qwen3.5-9B 智能体取得了平均成功率从 44.3% 到 53.6%（+9.3 个百分点）的提升。在直接对目标智能体进行微调后，论文还显示目标特定的工程师能将平均成功率从 59.2% 进一步推进到 64.2%（+5.0 个百分点）。由于这些增益在目标微调前后都存在，且目标微调后工程师仍能提供额外收益，论文认为 harness 工程师与目标智能体有希望实现共同进化（co-evolution）。

论文的主要贡献包括：首次将 harness 编辑形式化为一个可学习的、结果驱动的优化问题；提出了一套完整的训练协议（SFT 冷启动 + GRPO 在线训练 + 冻结目标重跑奖励）；在三个多样化的基准上验证了该方法优于基线，并展示了与目标微调的互补性。**

<!-- paperflow:556dd3cfc818744a -->
## HarnessCompass: Guiding Automatic Harness Evolution toward Generalizable and Effective Agent Harnesses

[[Deep Reading - Aug 2026/HarnessCompass-Guiding Automatic Harness Evolution toward Generalizable and Effective Agent Harn|Deep Reading]]

[https://arxiv.org/pdf/2608.01918v1](https://arxiv.org/pdf/2608.01918v1)

- **这篇论文介绍了 HarnessCompass，一个旨在引导自动 Harness 演化向通用且高效方向发展的框架。Harness 作为 LLM 智能体与环境交互的桥梁，其设计优劣直接决定了智能体的上限。作者指出，现有的自动演化方法（AHE）虽然减少了人工干预，但存在严重的过拟合问题，且由于信号单一和组件干扰，演化效率低下。

HarnessCompass 提出了三项核心原则：首先是**约束演化**，通过施加全局约束，迫使 Meta-Agent 产生任务无关的修改，从而确保 Harness 在未见任务上的泛化能力；其次是**主动反馈**，通过收集智能体在执行过程中的第一人称感受，为演化提供了比单纯轨迹更丰富的诊断信息；最后是**分组件优化**，通过解耦优化路径，解决了组件间相互抵消的问题。

实验结果令人印象深刻：在 SWE-bench Verified 基准上，配合 GPT-5.4 模型，该框架在短短 5 次迭代内将 Pass@1 成功率从 54% 提升至 66%。更重要的是，该方法演化出的 Harness 展现了极强的迁移能力，无论是在新任务还是新模型上都表现优异。这项工作强调了“有纪律的演化”与搜索算法本身同等重要，为构建高性能、高可靠性的 LLM 智能体系统提供了新的范式。**

<!-- paperflow:1b879f859412eae5 -->
## Progressive Agent Skill Generation via Reinforcement Learning

[[Deep Reading - Aug 2026/Progressive Agent Skill Generation via Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2608.01678v1](https://arxiv.org/pdf/2608.01678v1)

- **本文提出了 Skill-α，这是一个旨在解决 LLM 智能体技能自动生成难题的强化学习框架。研究背景在于，尽管技能模块化能显著提升智能体处理复杂任务的能力，但如何从杂乱的文档或失败的经验中提炼出高质量、可执行的技能仍然是一个开放挑战。现有的方法大多依赖于不可扩展的启发式规则。Skill-α 的核心创新在于将技能生成重新定义为一个渐进式的序列编辑任务。在这个框架下，生成器不是一次性输出完整技能，而是通过一系列微小的编辑动作来逐步完善技能。为了解决缺乏直接监督信号的问题，作者引入了“回滚奖励”机制：每一步编辑都会在特定的锚定任务上进行实测，通过对比编辑前后的性能差异来提供即时反馈。如果某次编辑导致性能下降，系统会自动撤销该操作，从而确保技能进化的方向始终是正向的。实验结果在 CL-Bench 和 tau2-bench 两个具有代表性的基准上验证了该方法的优越性，特别是在处理长文档和复杂交互轨迹时，Skill-α 生成的技能显著提高了下游智能体的任务成功率。消融实验进一步证实了渐进式策略和回滚机制在处理信用分配问题上的关键作用。该研究为构建能够自我进化、不断积累知识的自主智能体提供了一条可行的技术路径。**

# Memory, Personalization & Long-Horizon Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Memory, Personalization & Long-Horizon Agents
- 方法：agent, reasoning, retrieval, stat-ml, stat-me
- 论文/报告：3 篇
- GROVE: Growing and Reasoning over Temporally Stratified Memory from Streaming Video Experience
- From Profiling to Synthesis: Benchmarking Implicit Behavioral Alignment in Personalized LLM Agents
- MemArbiter: Decision-Time Memory Arbitration for Long-Horizon LLM Agents
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:df8faa42ee81cec0 -->
## GROVE: Growing and Reasoning over Temporally Stratified Memory from Streaming Video Experience

[[Deep Reading - Aug 2026/GROVE-Growing and Reasoning over Temporally Stratified Memory from Streaming Video Experience|Deep Reading]]

[https://arxiv.org/pdf/2608.02392v1](https://arxiv.org/pdf/2608.02392v1)

- **GROVE 提出了一种无需训练的流式视频记忆框架，通过将连续视频流因果地组织为感知证据、时刻、情节和跨天模式四个时间分层，并用尺度原生检索技能统一服务反应式问答与主动式辅助。

可穿戴助手应从用户视角持续观察世界数小时、数天甚至数周，并在此基础上提供两种互补的帮助形式：反应式问答（如“我昨天把钥匙放哪了？”）和主动式辅助（如用户即将重复犯错时主动提醒）。两种行为依赖同一个底层能力：把开放式的视频流变成随时间保持忠实、可搜索、可行动的记忆。

现有视频记忆系统（Xie et al. 2026、Long et al. 2025、Li, Numan, and Steed 2026、Chen et al. 2026a、Li et al. 2026、Liang et al. 2026 等）已能构建在线事件记忆、层次化组织经验、用迭代/智能体式检索定位稀疏证据，但绝大多数保留“查询优先”契约——必须由显式请求决定检索什么。主动助手（Zhang et al. 2025b、Gong et al. 2026）则用当前情境决定是否干预，但记忆和控制机制主要为服务触发设计，与开放式回忆接口分离。

因此研究空白是：缺乏一个单一记忆流，既能被用户查询访问，也能被当前情境访问。GROVE 以时间尺度为组织原则，提出训练免费框架，用同一记忆同时服务反应式问答与主动式辅助。

GROVE 的核心方法分为两个部分：时间分层记忆结构与尺度原生检索接口。

1. 时间分层记忆：从流式视频中因果地逐层构建四层结构：
 - 感知证据层：保留细粒度逐帧感知证据，是基础层。
 - 时刻层：增量整合为带时间戳的时刻。
 - 情节层：进一步整合为连贯活动片段。
 - 模式层：归纳跨天重复的行为模式。
 - 更新是因果的（只依赖当前和过去的信息），且高层记忆在情节关闭时才更新。

2. 尺度原生检索技能：每一层配一种专精技能——定位观察、回放活动、遍历长期规律。不同时间尺度的问题使用对应层的技能，而不是用单一检索过程处理所有尺度。

3. 统一访问：反应式 QA 和主动式辅助共享记忆和技能库。两种策略（查询发起 vs...**

<!-- paperflow:717a4c1c138a916b -->
## From Profiling to Synthesis: Benchmarking Implicit Behavioral Alignment in Personalized LLM Agents

[[Deep Reading - Aug 2026/From Profiling to Synthesis-Benchmarking Implicit Behavioral Alignment in Personalized LLM Agent|Deep Reading]]

[https://arxiv.org/pdf/2608.02171v1](https://arxiv.org/pdf/2608.02171v1)

- **本论文从“个性化从知识到行动的差距”这一观察出发，对 LLM 智能体个性化评测与建模进行了系统性反思。作者指出现有基准普遍依赖静态偏好快照、固定交互日志或预定义用户档案的问答，这些设定把个性化简化为显式知识的获取与匹配，而忽略了一个关键事实：真实个性化需要在任务执行过程中，动态综合来自纵向交互历史的隐含、噪声、矛盾、时变的用户约束。为了让个性化评测更贴近现实，论文做了三项工作：
1. 提出 IBA-Bench：一个面向隐性行为对齐的基准。其数据层构造了纵向交互历史，内含隐含偏好线索、动态属性、以及长期特质与短期状态的冲突；任务层则给智能体提供完整交互记录与用户请求，要求其在不依赖显式偏好表的情况下完成任务并满足推断出的约束。基准覆盖九个应用领域，突出“执行中的对齐”而非“知识问答”。
2. 提出 IBA-Agent：一个 LLM 驱动的个性化智能体框架，用于弥合历史偏好与当前任务执行之间的鸿沟。框架的核心是两个组件——广泛检索（从长历史中收集分散证据，而不是只看最近片段）与轨迹级对齐（在生成任务执行轨迹时统一考虑推断出的用户约束）。它被设计为能够调和冲突优先级（如长期偏好与临时状态之间的冲突），以模型无关的方式集成到不同 LLM backbone 上。
3. 实验验证：在 IBA-Bench 上对包括强基座 LLM（GPT、Qwen 等方向的模型）和强通用 agent（Claude Code、Hermes Agent）进行系统评测。结果显示，即使是当前最先进模型，在合成密集的执行任务上依然表现挣扎，实证验证了知识到行动差距的存在；而 IBA-Agent 在九个领域上相对于既有方法显著提升了复杂场景下的行为对齐。总体而言，论文既是一个新基准的提案，也是一个新 agent 方法的研究；它把“个性化能力评测”从偏好预测/QA 导向推向“偏好条件化任务执行”导向，对后续个性化智能体的设计、评测与训练都有推进意义。限于可获取的检索证据，论文的具体数据构造细节、指标定义、消融与误差分析、以及 IBA-Agent 的完整算法流程在摘要中未完全展开，需查阅全文确认。**

<!-- paperflow:34b23a16dc4549c4 -->
## MemArbiter: Decision-Time Memory Arbitration for Long-Horizon LLM Agents

[[Deep Reading - Aug 2026/MemArbiter-Decision-Time Memory Arbitration for Long-Horizon LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2608.02113v1](https://arxiv.org/pdf/2608.02113v1)

- **论文以“长程 LLM 智能体必须保留并使用跨步信息才能连贯行动”为出发点，指出现有记忆方法的主要努力方向是提高可访问性，但动作相关信息仍然可能因为形成不充分、组织不佳、优先级不当或呈现不当而无法影响当前决策。作者把这种访问后的失败称为 Memory-Action Gap，并明确提出：缩小这一 gap 需要的不只是更强的检索，而是决策时的记忆仲裁。

技术主线上，MemArbiter 是一个外部记忆模块。它首先把交互历史分解成原子记忆项，避免一条原始观察同时携带状态变化、动作结果、约束条件和参考事实时被整体存储而无法按需取用；然后按功能把这些原子项组织到五个 Memory Bank 中。在每步决策开始时，state-parsing 处理任务目标与当前状态；仲裁层结合 bank-level demand 和 item-level relevance 来估计哪些记忆需要被强调。MemArbiter 将记忆内容与 prompt 呈现分离，为每个记忆项提供两种表示：focal 表示保留完整内容以直接支持动作选择，ambient 表示采用确定性生成、提供背景但不抢占预算；再通过 temporal presentation gate 控制记忆在时间上的呈现，从而在统一 per-step token 预算内动态调节记忆显著性。整体来看，该方法把记忆管理从“选哪些文本”升级为“以什么功能角色、什么表示粒度、什么时间位置呈现”。

实验主线上，论文在 ALFWorld 的 134 个未见任务上，与 Flat Retrieval 和 Flat Recency 两个基线在统一每步记忆预算下比较。使用开权重动作生成模型时，MemArbiter 在 500 token 预算下达到 82.8% 成功率，比最强基线高 20.9 个百分点；在 750 token 预算下达到 92.5%，高 25.4 个百分点。论文还报告了失败后单步恢复率的提升，以及失败动作重复和状态-动作循环的减少；开权重与专有模型上都观察到改进。这些结果共同支持核心主张：功能感知的记忆仲裁能让已可访问的信息更有效地引导动作。

论文...**

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, generation, language, reasoning, reinforcement-learning, optimization, multimodal-learning, multimodal-reasoning
- 论文/报告：22 篇
- Global Optimization and Inference-Time Region Grafting for Agentic Workflows
- Agentic Commerce World: An Auditable and Verifiable Environment for Vibe Commerce
- CRISP: Critical Step Perception for Training Efficient Deep Search Agents
- Diagnosing Search Behavior and Failure Modes in Long-Horizon Search Agents
- KC-Agent: A Dual-Process Cognitive Architecture for Efficient ML Model Improvement
- Token-Native Storage: Read and Write in your Agent's Language
- TurnSight: Turn-Level Hindsight Self-Distillation for Tool-Integrated Reasoning
- Video-DeepResearch: Towards the Next-Generation Multimodal Deepresearch Agent
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:e0fa482bfc863ba8 -->
## Global Optimization and Inference-Time Region Grafting for Agentic Workflows

[[Deep Reading - Aug 2026/Global Optimization and Inference-Time Region Grafting for Agentic Workflows|Deep Reading]]

[https://arxiv.org/pdf/2608.02353v1](https://arxiv.org/pdf/2608.02353v1)

- **这篇论文提出并验证了 GRAFT，一种用于智能体工作流的推理时区域嫁接方法。论文的核心论证起点是：现有的智能体工作流优化方法，如基于任务特定搜索的 AFlow 和输入条件化架构选择的 MaAS，虽然能在离线阶段找到较优的工作流设计，但它们本质上是静态的——工作流在执行前被确定，之后无法利用执行过程中产生的无标签质量信号来调整失败区域。这种静态性限制了工作流在真实场景中的泛化能力，因为不同输入可能需要不同的推理路径、子任务分配或工具组合。
直接让工作流在推理时自适应是困难的：最朴素的重优化方案需要对整个工作流进行重新搜索，这在计算上不可行。同时，仅依赖 LLM 自我反思也不可靠，没有外部反馈时，LLM 评估和修正自身推理可能会降低性能。此外，多智能体系统中的区域间干扰使得局部修改可能带来不可预知的副作用。
针对这些挑战，GRAFT 提出了一个优雅的思路：保留全局优化好的工作流，将其作为种子，然后在推理时针对每个输入局部地替换有问题的区域。整个过程无需训练参数。具体而言，GRAFT 利用执行时收集的无标签质量信号来评估区域级的替代方案，并且只接受那些既能提升局部质量又能保持工作流级一致性的替换。这个接受准则非常关键，它既保证了局部改进的有效性，又避免了局部修改对全局信息流和任务衔接的破坏。通过这种方式，GRAFT 实现了实例级的自适应，而无需对整个工作流进行重新优化。
在方法验证方面，论文设计了系统的实验。实验覆盖了数学推理、代码生成、多跳问答和知识密集型问答五类基准任务。在匹配优化器和执行器的公平设置下，GRAFT 的平均得分达到 87.44，比最强的先前工作流优化方法 MaAS 高 3.85 分。这直接回答了 RQ1：推理时区域嫁接确实能改进离线优化的工作流。RQ2 则关注收益的来源，推测通过消融实验验证了收益来自局部嫁接机制本身而非额外计算量或自我修正。更进一步的实验显示，仅将执行器替换为更强的模型，无需重新优化全局工作流，也能获得额外性能提升。这一结果具有很强的启示意义：优化后的工作流并非一个僵化的静态产物，而是一个可以随推理时反馈和更强执行器而演化的可适应执行策略...**

<!-- paperflow:9c310b8febc3b731 -->
## Agentic Commerce World: An Auditable and Verifiable Environment for Vibe Commerce

[[Deep Reading - Aug 2026/Agentic Commerce World-An Auditable and Verifiable Environment for Vibe Commerce|Deep Reading]]

[https://arxiv.org/pdf/2608.02441v1](https://arxiv.org/pdf/2608.02441v1)

- **论文从 vibe coding（用自然语言描述软件、由 AI 负责实现）出发，类比提出 vibe commerce：用户用自然语言表达购买或出售目标，并委托 AI agent 完成对应商业任务。但商业场景与写代码有一个关键差异：买卖天然涉及两个相互独立且有不同权威的控制者——Buyer 和 Merchant，他们共享一个市场状态，却各自持有私有目标（例如买方价格上限、卖方底价）。因此，纯粹的“单智能体任务完成”评估框架不足以支撑 vibe commerce，需要一个新的可审计、可验证环境。

为此，论文引入 Agentic Commerce World（ACWORLD）。ACWORLD 的核心是 Vibe Commerce Protocol（VCP）：每个 agent 提议的动作必须先绑定到具体的 Buyer 或 Merchant，再由 Commerce Intelligence Platform 校验，只有通过校验的“授权效果”才会被提交到共享世界状态。所有交互都被记录，因此 agent 行为可审计、评估可复现。Figure 1 展示了典型谈判：买方私有 $100 上限、卖方私有 $90 底价，双方以 $95 成交，且私有界限始终未泄露。

ACWORLD 是一个有状态的、多对多的商业环境，同一架构既支持紧凑受控的小世界，也支持大规模目录。基准包含两个互补的 track：capability-coverage track 包含 200 个任务，覆盖 10 个商业类别和 80 种能力，用于检验广泛的商业行为；large-catalog track 包含 60 个任务，在 785,022 条可交易 listing 中进行搜索和交易，用于暴露规模压力。两个 track 使用完全相同的执行与打分通路，保证跨任务、跨模型的可比性。

实验部分评测了十个模型。Capability-coverage track 的平均得分范围为 65.9%–85.6%，large-catalog track 为 56.1%–91.4%。更重要的是，论文对“如何评估”本身做了分析，得到三个关键结论：...**

<!-- paperflow:68b2a71af1861d30 -->
## CRISP: Critical Step Perception for Training Efficient Deep Search Agents

[[Deep Reading - Aug 2026/CRISP-Critical Step Perception for Training Efficient Deep Search Agents|Deep Reading]]

[https://arxiv.org/pdf/2608.01867v1](https://arxiv.org/pdf/2608.01867v1)

- **CRISP 是一篇面向 LLM 深度搜索智能体效率优化的工作。论文的出发点是：虽然 LLM 智能体能够通过多步工具交互解决复杂问题，但现有智能体经常产生冗长、低效的轨迹，包含重复查询、弱线索和未使用的观察，导致计算与交互成本高昂。已有的效率方法通常直接减少工具使用频率，但论文指出这种方式过于粗糙——它无法区分“收集必要证据的步骤”和“冗余步骤”，统一惩罚可能损伤答案质量。

为此，论文提出证据中心的关键步骤（evidence-critical steps）概念，定义为“提供或保留支撑最终答案证据的工具交互步骤”。在此基础上，CRISP 框架包含三大模块：

1. 反向证据归纳（Backward Evidence Induction）：用强模型从最终答案出发，沿完整搜索轨迹反向遍历，判断每个工具交互步骤是否贡献关键证据，从而自动生成步骤级标注。

2. 关键步骤识别器蒸馏：将强模型的步骤级判断蒸馏到较小模型中，实现单次遍历全轨迹分析，降低训练与推理开销。

3. 效率感知奖励：仅在成功 rollout 上施加奖励，保留关键步骤、剪除冗余步骤，从而在不牺牲正确性的前提下提升交互效率。

实验在 BrowseComp 和 HLE-Verified 两个基准上进行，结果显示：CRISP 保持具有竞争力的最终答案准确率，同时平均交互轮次分别减少 15.1% 和 33.2%。这表明长轨迹中大量冗余步骤是可控的，且证据关键性判断能够有效指导正则化。

论文的主要贡献也可归纳为：提出一种证据中心的效率优化视角；设计自动化的反向证据归纳标注方法；构建轻量级识别器；并在真实困难搜索基准上验证了效率-准确率权衡的改善。整体上，CRISP 的价值在于用细粒度的步骤级信号替代粗粒度的工具惩罚，为深度搜索智能体的训练提供了更精准的效率控制途径。

（注：由于当前仅能访问摘要和部分 Introduction/Conclusion 片段，具体方法细节、Baseline 设置和消融实验需要阅读原文进一步确认。）**

<!-- paperflow:099249f41bbeea2f -->
## Diagnosing Search Behavior and Failure Modes in Long-Horizon Search Agents

[[Deep Reading - Aug 2026/Diagnosing Search Behavior and Failure Modes in Long-Horizon Search Agents|Deep Reading]]

[https://arxiv.org/pdf/2608.01913v1](https://arxiv.org/pdf/2608.01913v1)

- **本文针对长程搜索智能体（Long-Horizon Search Agents）的性能瓶颈进行了系统性的轨迹级诊断。研究的核心发现是：搜索的“量”（步数、Token消耗）并不等同于“质”（答案准确率）。作者通过引入人工标注的相关性判断，将智能体的失败解构为“检索差距”和“利用差距”。实验证明，检索到的证据质量（尤其是累积召回率）是决定成败的关键，而许多智能体在已经获取足够证据后仍盲目搜索，导致效率低下且引入噪声。研究揭示了搜索过程中的早期收益递减现象，并指出表现强劲的智能体具有更高的“查询纪律”，能有效避免冗余。最后，本文为构建更高效的深度研究系统提供了明确方向，包括优化查询构造、实施证据驱动的停止准则以及更精细的上下文管理策略。这一工作为智能体从“盲目搜索”向“精准研究”的转变提供了理论支撑和工程指导。**

<!-- paperflow:d2255c80a36f3ca3 -->
## KC-Agent: A Dual-Process Cognitive Architecture for Efficient ML Model Improvement

[[Deep Reading - Aug 2026/KC-Agent-A Dual-Process Cognitive Architecture for Efficient ML Model Improvement|Deep Reading]]

[https://arxiv.org/pdf/2608.02351v1](https://arxiv.org/pdf/2608.02351v1)

- **KC-Agent（Kahneman-Clear Agent）针对生产环境中机器学习系统面临的数据漂移问题，提出了一种受心理学双过程理论启发的自动化模型改进架构。论文的核心动机是：ML 模型在部署后，其输入数据分布会随时间变化（数据漂移），导致性能下降，需要不断更新。然而，传统手动更新和既有自动化方法面临效率、可靠性、知识积累之间的权衡。作者借鉴卡尼曼（Kahneman）在《思考，快与慢》中提出的双系统理论，将模型改进过程分为 System 1（快速、直觉式、低计算成本的模式识别）和 System 2（缓慢、深思熟虑、系统性的推理与增量更新）。KC-Agent 的关键想法是：System 2 通过深入探索发现成功的解决方案，将其沉淀到结构化记忆中；System 1 在遇到类似情境时直接调用这些记忆，从而实现快速响应而无需重新计算。这种记忆复用机制使得 KC-Agent 能够跨会话累积领域专长，与 CodeAct 等每次从零开始的方法形成鲜明对比。此外，为了满足生产环境对安全性和可验证性的要求，论文引入了原子变更原则——即每次更新作为一个不可分割的整体，要么完全成功要么完全不生效——并提供回滚能力，确保任何失败修改都能恢复到先前状态。实验部分，论文在五个数据集上进行评估：包括具有真实时间退化模式的 NASA 涡扇数据（FD001 等）和带受控漂移的合成数据。基线包括 CodeAct、Tree of Thoughts、ReAct、Reflexion 和 Plan-Execute。量化结果表明 KC-Agent 在准确率（76.8%）和执行时间（13.2秒）上同时达到最佳，相对提升分别为 CodeAct +2.4%、ToT +3.6%、ReAct +8.0%、Reflexion +8.9%。在最具挑战性的 NASA-FD001 上，KC-Agent 达到 61.3% 准确率，优于基线 49.3%（相对提升 24%）。除数值指标外，论文还采用由多个 SOTA LLM 组成的专家组进行共识评估，KC-Agent 在 Smartness（8.33/10）上显著领先，而保守方法（如 C...**

<!-- paperflow:1a1916877926afe4 -->
## Token-Native Storage: Read and Write in your Agent's Language

[[Deep Reading - Aug 2026/Token-Native Storage-Read and Write in your Agent's Language|Deep Reading]]

[https://arxiv.org/pdf/2608.02376v1](https://arxiv.org/pdf/2608.02376v1)

- **论文从基础设施层面提出一个范式转变：把存储的物理格式从‘人类可读的 UTF-8’改为‘模型可读的 BPE token ID’。论证主线是：现在真正的读写方已经不是人，而是 embedder、reranker 和 agent，它们每次访问文本都要执行 UTF-8→token ID 的转换，写时还要反向转换；如果存储本身就用 token ID，这些转换可以省掉，同时因为 token 序列比字符序列更紧凑，还能获得压缩收益。技术主线是压缩和延迟两方面的实证：作者用 r50k 词表的 uint16 打包就比 UTF-8 小 2.25 倍，熵编码可达 3.30 倍；在六种 tokenizer 和英文、代码、印地语三种语料上，token-ID 压缩匹配或超过所有字节 codec，包括语料训练的 zstd 字典。一个关键发现是 BPE 按合并顺序编号而非频率，导致 ID 熵偏高；仅按频率重排 ID，streamvbyte 这类普通整数 codec 就能恢复熵编码的大部分压缩率，解码快约 7 倍，这是一行代码的改进。另一个关键结果是读路径延迟：因为模型直接消费 ID，无需重新 tokenize，延迟可减少约 10-600 倍；在 agent 工作负载中，一个 RAG 回答读数百 chunk，节省会复合放大。论文承认的障碍是 tokenizer 不统一，所以最后呼吁标准化：发布共享词表，类比 ASCII/UTF-8。全文的贡献是：提出 token-native storage 概念、给出压缩与延迟的系统性证据、指出 BPE 频率重排的一行改进、以及提出行业标准化倡议。该论文更像观点+初步实验的 position paper，而非完整数据库系统实现。**

<!-- paperflow:720d0fcba267462a -->
## TurnSight: Turn-Level Hindsight Self-Distillation for Tool-Integrated Reasoning

[[Deep Reading - Aug 2026/TurnSight-Turn-Level Hindsight Self-Distillation for Tool-Integrated Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2608.04007v1](https://arxiv.org/pdf/2608.04007v1)

- **### 论证主线
论文针对工具集成推理（TIR）中长视界信用分配问题，指出轨迹级 RL 将单一优势均匀传播给所有动作，无法区分不同工具的因果贡献。现有密集监督方法（如在线策略自蒸馏）虽提供更丰富信号，但存在两个缺点：1) 利用标准答案或检索技能作为特权上下文，与智能体实际访问的执行状态不一致；2) 采用 token 级碎片化监督，忽略轮级交互结构。基于此，论文形式化两个信用分配要求：与在线执行状态对齐、交互级连贯。

### 技术主线
作者提出 TurnSight，一个轮级后见自蒸馏框架，其核心创新是使用工具执行结果本身作为后见信号，完全在线，无需外部知识与状态假设。具体流程：
- 从当前策略生成的轨迹中提取每轮工具执行结果作为后见信号；
- 构造多个不同前瞻视野的后见视图，评估每个轮次的短期与长期影响；
- 通过跨视野方向一致性筛选可靠信号，过滤冲突与噪声；
- 在兄弟轨迹间归一化，使信号具有可比性；
- 将归一化后的信号用于自适应调制 RL 优势，同时保留优势的符号（即优化方向），从而保留策略梯度探索能力。

### 实验主线
论文在三个基准上验证有效性，使用 FTRL 数据集（超 2000 个自动生成问题，可执行工具环境与程序化验证）进行训练，采用 8B 模型，对比轨迹级 RL（如 GRPO）和既有自蒸馏方法。结果表明 TurnSight 一致超越所有基线，在 8B 模型上建立新 SOTA，同时保持完全在线策略。实验覆盖域内与域外任务，展示鲁棒性与泛化能力。

### 总结
TurnSight 提供了一种在不改变 RL 优化方向的前提下注入细粒度轮级信用的新途径，其执行条件化后见信号的设计大幅提升了状态对齐性，多视野一致性过滤机制保证了信号可靠性，兄弟归一化增强了训练稳定性。该工作为 TIR 和其他长视界智能体任务的信用分配提供了新范式。

### 局限性（隐含）
论文依赖可验证反馈，工具执行结果的质量直接影响后见信号；跨视野一致性可能过滤掉一些非共识但有效的信号；兄弟归一化需要同问题多轨迹采样，增加推理开销。这些未在摘要中显式讨论，但可从方法逻辑中推断。**

<!-- paperflow:28200da15c280fdc -->
## Video-DeepResearch: Towards the Next-Generation Multimodal Deepresearch Agent

[[Deep Reading - Aug 2026/Video-DeepResearch-Towards the Next-Generation Multimodal Deepresearch Agent|Deep Reading]]

[https://arxiv.org/pdf/2608.03979v1](https://arxiv.org/pdf/2608.03979v1)

- **这篇论文提出了 Video-DeepResearch (Video-DR)，这是一个旨在解决多模态智能体在处理连续视频流时面临的深度研究难题的系统性框架。研究的核心动机在于发现当前最先进的模型（如 GPT-5, Claude-4.5）在处理视频任务时存在严重的“模态偏见”和“参数知识泄露”，即它们更喜欢搜网页而不是看视频，且容易受预训练记忆干扰。为了打破这一僵局，Video-DR 创新性地提出了感知与探索解耦的流水线，通过阶段性工具解锁机制，强制智能体在获取外部信息前必须完成深度的视觉定位。在技术实现上，Video-DR 采用了 SFT 结合 GRPO 的两阶段训练范式，利用强化学习优化智能体的工具调用逻辑和推理路径。为了验证该框架，作者还推出了 Video-DR-Bench，这是一个包含 200 个高难度、多跳推理任务的基准。实验数据表明，Video-DR-35B-A3B 模型在性能上全面超越了现有的顶级商业模型，平均准确率达到 64.0%。该工作不仅填补了视频深度研究智能体的空白，还为多模态强化学习提供了一个极具参考价值的范式。论文通过详尽的消融实验和工具使用分析，揭示了智能体在跨模态任务中如何通过结构化的流程设计和策略优化来克服固有的行为偏差，为下一代多模态智能体的发展指明了方向。**

<!-- paperflow:a546b760d9bab76c -->
## PAST-Bench: Benchmarking the Foundations of Recursive Self-Improvement in Personal Agents

[[Deep Reading - Aug 2026/PAST-Bench-Benchmarking the Foundations of Recursive Self-Improvement in Personal Agents|Deep Reading]]

[https://arxiv.org/pdf/2608.04003v1](https://arxiv.org/pdf/2608.04003v1)

- **这篇论文针对个人 AI 智能体是否真正从保留经验中受益这一递归自我改进的基础问题，构建了 PAST-Bench 基准和 Hermes+ 干预框架。论证主线是：递归自我改进（RSI）要求智能体把自身运行产生的经验转化为未来更好的行为，但现有系统虽然保留偏好、任务历史、工具例程和习得技能，却缺乏系统性测试来确认这些保留经验是否真的带来改进。技术主线上，作者设计了一种性能归因协议：在 26 个场景、204 个 episode 中，任务按任务族有序组成 fresh-session 序列，每个任务族内配对执行‘持久化开启’和‘持久化关闭’两种条件，控制基础模型、运行时等混淆因素；评估单元是智能体穿越任务族的完整轨迹，报告后期任务增益，并额外检查增益是否符合预期的 save-retrieve-update 机制路径。实验主线覆盖 7 个基础模型和 4 个智能体框架，发现改进是真实但不均匀的，且相同总体增益的智能体在机制路径证据上差异显著。基于诊断结果，作者提出 Hermes+，在智能体循环各阶段加入五项针对性干预，总体上提升平均增益并让路径证据更清晰，其中最强效果出现在需要替换过时状态的任务上，但效果依赖于具体能力和模型。结论上，论文强调评估 RSI 需要同时报告任务分数和机制证据，PAST-Bench 与 Hermes+ 共同构成‘从保留经验到系统性改进’的评估-诊断基础，并在未来工作中提出拓宽生态效度、时间跨度、探索参数级 RSI、细化路径失败模式等方向。**

<!-- paperflow:999167d58ce4df29 -->
## Agentic Reinforcement Learning with Self-Distilled Reward Shaping

[[Deep Reading - Aug 2026/Agentic Reinforcement Learning with Self-Distilled Reward Shaping|Deep Reading]]

[https://arxiv.org/pdf/2608.03223v1](https://arxiv.org/pdf/2608.03223v1)

- **本文聚焦智能体强化学习（Agentic RL）中的一个关键瓶颈：LLM 智能体在多轮交互中学习时，只能获得轨迹级的稀疏成功/失败奖励，缺乏对中间决策的信用分配。这种稀疏信号使模型难以判断哪些 token 级动作真正推动了任务成功，尤其当任务需要长序列推理和操作时，梯度信号极弱。

论文的主要观察是，训练阶段可以利用特权信息（privileged skills）来补偿这种稀疏性。具体而言，程序化技能（procedural skills）是与任务匹配的、对成功步骤有先验描述的信息，例如“先加热再放入物品”“先拿起物体再移到目标位置”等。训练时，可以将这些技能作为条件输入，使用同一个策略的冻结快照重新评分无技能轨迹中的每个 token。因为教师是策略自身的旧版本，所以这种机制被称为“自我蒸馏”（self-distillation）。

然而，作者指出三个朴素使用这种教师信号的问题。第一，不同交互步骤的 token log 概率在量纲和偏移上不可比：token 的概率受到局部上下文、策略不确定性、响应长度与格式组成的影响；因此，一个步骤中的高置信度不能直接与另一个步骤中的高置信度相比。第二，教师的高置信度未必与其决策对最终回报的贡献有关：模型可能对低风险、格式化的 token 信心十足，但真正改变任务走向的却可能是一些低调但关键的 token；若不加区分，所有 token 信号都会被平均地注入 RL 优化，导致模型过度优化与回报无关的偏好。第三，即使生成了 token 级信号，也需要设计它与原生 RL 信用构造的接口，使其能有效地进入 reward-to-advantage 转换，而不是孤立地作为一种外部奖励。

为应对上述三个问题，论文提出 ADRS，其核心包含三个组件：

- 步骤内校准：在每个交互步骤内部对特权 token 分数进行中心化和归一化，去除跨步骤的尺度与偏移差异，使信号可比；
- TVA 门控（Teacher Value Advantage gate）：基于组内置信度-回报关联，把教师置信度与真实回报联系起来。如果一组样本中教师置信度与回报正相关，说明该组信号...**

<!-- paperflow:668a41eb64631070 -->
## EASy: Towards Efficient LLM-Based Agentic System

[[Deep Reading - Aug 2026/EASy-Towards Efficient LLM-Based Agentic System|Deep Reading]]

[https://arxiv.org/pdf/2608.04588](https://arxiv.org/pdf/2608.04588)

- **本文提出 EASY（Efficient Agentic SYstem），一个可训练的 agentic 框架，用于在现实约束下联合优化任务性能和计算效率。论文的论证主线是：现有 agentic 系统主要追求任务成功率，而真实部署中执行器能力差异和计算成本不可忽略；同时，基于路由的轻量方法无法对丰富演化上下文、多步依赖和中间反馈进行推理，且对未知执行器泛化差。因此需要一种可训练的、能力-成本感知的 orchestrator。技术主线上，EASY 将 orchestrator 作为一个 LLM 策略，显式输入执行器的能力画像和成本画像；并设计了里程碑-计划-执行工作流：先解耦里程碑，再构建依赖感知执行图，分配执行器，并行执行独立步骤，并在中间反馈的基础上动态调整后续计划。训练时，作者采用树结构 rollout 来探索不同的里程碑分解和执行计划，配合由任务正确性、执行效率和轨迹完整性组成的三组件奖励。这个训练框架提供了可扩展的监督，学习可靠且低成本的协调策略。实验主线上，论文在数学推理、具身决策和深度研究三个 benchmark 上与强 agentic 基线比较，报告 EASY 在性能-效率权衡上持续领先；进一步分析和消融验证了各个组件的贡献，以及 orchestrator 对更强执行器（Qwen2.5-7B-Instruct 到 GPT-5-mini）的适应性和跨分支/跨任务的泛化性。整体上，EASY 展示了通过强化学习自动学习 agent orchestration 的可能性，为构建性能-成本双优的 agentic 系统提供了一个有前景的范式。**

<!-- paperflow:d5c2ad9b7a1acefa -->
## Skill-Use: Can LLMs Actually Use Skills in Agentic Harnesses?

[[Deep Reading - Aug 2026/Skill-Use-Can LLMs Actually Use Skills in Agentic Harnesses|Deep Reading]]

[https://arxiv.org/pdf/2608.04828](https://arxiv.org/pdf/2608.04828)

- ****论证主线：** 论文从一个现实观察出发：LLM 智能体越来越多地依赖“技能”这种结构化文档，但现有评估只关注技能自身质量或任务最终成功，对智能体“会不会用技能”这个中间环节几乎没有刻画。作者认为，技能使用可分解为三个可独立测量的方面：触发、遵循、边界。如果这三个方面都能被可靠评估，我们才能真正回答“模型是否掌握了技能”的问题。

**技术主线：** 为此，作者构建了 SKILL-USE 基准。其核心设计是渐进披露：智能体只看到技能名称和一句描述，必须先自行检索完整技能文档才能执行。基准用 79 个真实技能配对 177 个真实文件任务，覆盖 9 个领域，所有任务运行在 Docker 沙箱中，通过轨迹级 rubric 评分。SU 分数把三维度整合，触发是执行计分的前提。构建过程中采用 masked-skill screening 过滤通用性任务，并对每个技能-任务-rubric 三元组做端到端人工验证，尽量保证任务必须依赖技能才能完成。

**实验主线：** 论文选取 8 个 LLM 和 2 种 agent harness，逐一跑完 177 个任务，记录触发、遵循、边界得分与 SU。结果显示最强配置（GPT-5.5 × CC）SU 只有 0.613，说明可靠技能使用尚未实现。更关键的是，触发与遵循是彼此独立的瓶颈：识别出技能不等于能按流程执行，能执行也不等于会主动触发。而且同模型在不同 harness 上的分数和排名变化显著，说明技能使用不是模型固有属性，而是模型与 harness 交互出来的系统能力。

**整体评价：** 这项工作把“技能使用”从技能质量和任务成功中独立出来，给出了一个可操作、可分维度的评估框架，并且第一次系统暴露了 harness 对技能使用的影响。它为后续提升 LLM 智能体的技能运用能力提供了清晰的度量基准和失败模式分析。**

<!-- paperflow:7436369214269b8a -->
## Argus: A General-Purpose Agentic Runtime for Long-Horizon Reasoning

[[Deep Reading - Aug 2026/Argus-A General-Purpose Agentic Runtime for Long-Horizon Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2608.05144](https://arxiv.org/pdf/2608.05144)

- **Argus 是一篇提出通用智能体运行时（agentic runtime）的论文，其核心观点是：长时程推理的成功不在于一个方向跑多远，而在于运行时能持久地根据证据前进、也能在必要时转向。论文将这一观点具体化为一个持久、可自我进化的系统。

**论证主线**：论文首先指出现有长时程智能体的关键缺陷——连续性的丧失（transcript 被压缩或截断导致状态丢失），以及缺乏将"转向"与"漂移"分开的机制。为了达到任何可认证的长期行为，运行时必须满足某些属性，而这些属性在现有长时运行智能体中均不成立。Argus 因此被设计为一种基础设施，使持久性和转向成为系统级属性，而不是模型参数属性。

**技术主线**：Argus 由四个模型驱动角色（Manager、Planner、Engineer、Reviewer）和三个系统平面（控制平面、执行平面、持久状态平面）构成。控制平面锚定整个战役并调度工作；执行平面负责在真实工具和工件上执行有界任务；持久状态平面（推断）保存项目状态、记忆、技能和程序。

**进化机制**：系统的自我进化发生在固定模型权重的前提下，通过运行时状态的改变实现。任何新增记忆、技能、程序、验证器、路由决策或拒绝路线，都必须经过角色拥有的审查和（在可用时的）任务原生验证。"验证门控"确保了只有经过验证的经验才能被积累，从而避免污染持久状态。

**实验设计**：论文围绕两个研究问题（RQ1 基准性能、RQ2 固定模型自我进化）展开，覆盖七个 GPT-5.5 基准竞技场，包括 SWE-Bench Pro、AARRI-Bench、数学数据合成、GPU 内核优化、语言模型训练、软件修复和研究任务。此外还报告了三个生产垂直案例：RWKV6 内核合并上游、多日数学战役、六个论文流水线。

**关键结果**：
- SWE-Bench Pro 78% vs Direct Copilot 59%（token 1.41 倍）；
- AARRI-Bench 76.8%；
- 数学数据合成 28.0 分优势；
- 成熟波比启动波少用 21% 的解决输入 token 和 15% 的活动工作流...**

<!-- paperflow:0ef6aa4ad26c70f6 -->
## OneDayAgent: Towards a Long-Horizon Harness for Autonomous Agents

[[Deep Reading - Aug 2026/OneDayAgent-Towards a Long-Horizon Harness for Autonomous Agents|Deep Reading]]

[https://arxiv.org/pdf/2608.05013](https://arxiv.org/pdf/2608.05013)

- **随着大语言模型（LLM）智能体被越来越多地应用于开放式日常请求，这类请求往往跨越工作、学习和生活等多个领域，具有长时程、跨环境、多模态的典型特征。智能体需要在一个较长的执行过程中持续保持用户的目标和约束，并操作各种异构工具和附件。然而，长时程决策面临的挑战是独特的：随着任务从分钟延伸到小时，上下文持续累积，多步决策容易受到目标漂移、状态丢失和上下文溢出等问题的困扰。已有研究分别处理了这些失败模式，但缺乏一个统一的框架来同时管理它们，并且这样的框架还需要在多个后端模型上保持有效。\n\n本文提出 OneDayAgent，一个长时程执行框架（harness），旨在将开放式请求转化为受管理的执行过程。该框架基于三个核心能力：（1）任务分解，将过载的请求分解为一系列有界的子任务，使长任务变得可操作；（2）执行记忆，在上下文压力下维护关键的执行状态，防止目标漂移和状态丢失；（3）交付物验证，验证并修复最终交付物，确保符合用户请求。此外，框架保持工具和环境接口轻量，通过统一的动作接口暴露异构环境，使得框架本身与具体工具解耦。\n\n在实验方面，作者使用 AgentIF-OneDay 基准进行了评估，该基准包含 104 个开放式日常任务。以 GLM-5.2 作为后端时，OneDayAgent 取得了总分 0.821，达到新的最先进水平。更重要的是，同一 harness 无需任何调优即可在来自三个模型族的五个不同后端 LLM 上运行，展示了良好的跨后端泛化能力。论文还观察到，不同后端模型在同一工作流下会展现出不同的执行风格，这进一步说明框架能够容纳后端差异。\n\n本文的主要贡献在于首次提出了一个能够联合处理任务分解、执行记忆和交付物验证的长时程智能体框架，并通过实验证明了其在单一基准上的优越性和跨后端的泛化性。这项工作为长时程智能体的系统设计提供了一个新的视角，即通过外部 harness 而非修改模型来提升整体能力。\n\n需要注意的是，由于目前只能获取摘要和部分片段，本文的具体实现细节、实验对比、消融分析等信息尚不明确。例如，任务分解的具体策略、执行记忆的存储机制、交付物验证的工作...**

<!-- paperflow:cb3c319a2b26e991 -->
## EvolveNet: Collaborative Harness Evolution for Agent Self-Improvement

[[Deep Reading - Aug 2026/EvolveNet-Collaborative Harness Evolution for Agent Self-Improvement|Deep Reading]]

[https://arxiv.org/pdf/2608.04968](https://arxiv.org/pdf/2608.04968)

- **EvolveNet 提出了“协作式 harness 演化”这一新范式，用于解决多代理环境中共享经验无法集中时的代理自我改进问题。论文首先指出，LLM 代理的能力不只取决于模型权重，还取决于 harness——那个负责构造上下文、调用工具、校验结果、失败恢复的可执行程序。近期研究已经证明，通过演化 harness 可以在不更新模型权重的情况下获得持续改进，但这些工作大多假设可以将所有 workload、执行轨迹和行为反馈集中到一个优化器，沿一条串行轨迹演化单一 harness。这一假设在真实世界中不成立：用户、组织和环境产生的是互相隔离的经验流，无法集中池化；而最值得学习的经验，恰恰是那些无法被直接集中的经验。

为了突破这一限制，EvolveNet 把经验提取的动作从中心下放到数据本地。具体地，一个共享 harness 被广播给多个数据本地的代理部署；每个部署在自己私有的 workload 上独立演化 harness。跨越数据边界的不是原始数据或执行轨迹，而是程序文本、相对公共基线的改动 delta、以及逐项行为判定。这些信息被送回聚合端，组合成一个更新后的共享 harness，再重新分发，使每个参与代理都能继承其他代理发现的操作经验。通过这种方式，EvolveNet 将聚合边界从“原始工作负载”上移到“学到的改编”，让 workload 保持在本地，并允许多个演化搜索并发推进，从而降低串行深度。

程序不是参数，无法求平均，独立修改的程序在组合时可能冲突。为此 EvolveNet 引入了带作用域类型、由证据引导的程序聚合（scope-typed, evidence-guided program aggregation）。聚合依据每个改编的作用域和逐项行为判定，决定如何安全地组合来自不同代理的修改；消融实验进一步表明，改进的收益来自“组合”而非“选择”——即把不同代理的改编融合起来，而不是从它们中挑一个最佳版本。

实验覆盖五个差异很大的场景：BIRD（文本到 SQL）、DS-1000（数据科学编码）、LiveCodeBench（竞赛编程）、SWE-bench（软件工程...**

<!-- paperflow:302bd48495ff2f51 -->
## HarnessOpt-Bench: Evaluating LLMs at Harness Optimization

[[Deep Reading - Aug 2026/HarnessOpt-Bench-Evaluating LLMs at Harness Optimization|Deep Reading]]

[https://arxiv.org/pdf/2608.06301v1](https://arxiv.org/pdf/2608.06301v1)

- **本论文提出并标准化了“自动 harness 优化”的研究问题，并给出了第一个系统性基准 HarnessOpt-Bench。作者认为，在 LLM 被广泛部署于 agentic 系统的背景下，系统能力不仅仅来自模型权重，还来自环绕模型的 harness（prompts、工具、控制流、记忆、编排代码）。自动 harness 优化——即 AI 系统依据评估反馈迭代改进自身 harness——既是提升系统性能的现实路径，也是对 AI 系统自身提出的高难度挑战。然而领域内缺少统一的测量协议，无法回答“哪个模型更擅长优化 harness”这一问题。

为了填补空白，论文设计了一个端到端评测流程：优化器（LLM + 编码 harness）以目标智能体的种子 harness、分级评估反馈和固定预算为输入，输出经过修改的最终候选。候选的得分基于其在保留测试分区上的归一化增益，该测试分区在搜索全程不可访问，从而避免优化器通过拟合可见分数取得虚假收益。为了保障协议的完整性，作者实现了可信执行环境，用于强制评估边界、计量目标智能体的资源使用、并保存每个候选版本供审计。设计上还采用了配对实验：每个优化器模型在共享编码 harness 和原生 harness 两种条件下运行，以便分离模型能力与框架效应。

实验部分选取了来自 3 家开发者的 5 个前沿模型（claude-opus-5、claude-sonnet-5、gpt-5.6-sol、gpt-5.6-terra、kimi-k3），在 4 个下游任务上进行了共 111 次评分运行。结果表明，优化器模型之间的差异平均大于共享/原生 harness 之间的差异，说明模型本身是主导因素；原生 harness 并不保证更好，说明该能力是可迁移的；增益在不同任务和种子 regime 间波动明显。基于这些结果，作者将 harness 优化确立为一种可测量、可区分、且仍有大幅改进空间的能力。

论文还坦率地声明了局限性：基准是“防作弊倾向”而非“防作弊牢不可破”，优化器可能通过反复的开发-验证反馈间接得到测试信息；候选代码限于 Python；每个任务只固定一个...**

<!-- paperflow:38a2ce7ea8281e8c -->
## The Illusion of Visual Tool-Use: A Causal Audit of Thinking with Images

[[Deep Reading - Aug 2026/The Illusion of Visual Tool-Use-A Causal Audit of Thinking with Images|Deep Reading]]

[https://arxiv.org/pdf/2608.06270v1](https://arxiv.org/pdf/2608.06270v1)

- **论文围绕“thinking-with-images”这一范式展开因果审计。所谓 thinking-with-images，是指让多模态大模型在推理时主动调用 crop-and-zoom 等视觉操作，以获取局部高分辨率证据。该范式在直觉上能提升细粒度感知，但已有观察显示：工具使用往往只带来边际甚至负向的准确率变化，却显著增加 token 消耗；模型可能反复裁剪无关区域，并在直接推理本可答对的样本上失败。论文由此提出核心质疑：返回的视觉证据是否真的因果地影响了答案？

为了回答这个问题，作者将视觉工具使用建模为因果图，把每条轨迹的路径分为“观测中介路径”（observation-mediated paths）与“动作诱导捷径”（action-induced shortcuts），并设计了三个层级的干预：policy 层比较工具使用与直接推理；trajectory 层在 rollout 中破坏所有观测；step 层在固定前缀下反事实替换单个观测。step 层对应的估计量 Visual Evidence Gain（VEG）用于隔离每条观测的因果贡献。

在六个代表性模型和五个细粒度感知基准上，论文发现策略失校准是核心瓶颈，具体表现为两种失败模式：Calling Without Looking（观测无因果效应）与 Looking Without Planning（观测有信息但调用规划不连贯）。trajectory 层诊断进一步揭示，policy 层的总体准确率增益是真实的，但集中于少数已校准样本；在更广泛的 rollout 中，视觉工具使用并不因果有效。作者将这种聚合增益与 rollout 级无效性之间的错位称为“视觉工具使用的幻觉”（the illusion of visual tool-use）。

论文的贡献包括：首次将视觉工具使用置于因果审计框架下；提出三层干预协议与 VEG 估计量；识别出两类可操作的失败模式；给出一个 trajectory 级诊断来分解 policy 级增益；并开源代码（https://github.com/OpenCausaLab/CauAudit）...**

<!-- paperflow:9f06493f5e8f4d38 -->
## When History Lies: Evaluating and Improving Tool Use under Misleading Multi-Turn Histories

[[Deep Reading - Aug 2026/When History Lies-Evaluating and Improving Tool Use under Misleading Multi-Turn Histories|Deep Reading]]

[https://arxiv.org/pdf/2608.06057v1](https://arxiv.org/pdf/2608.06057v1)

- **本文探讨了多轮交互中工具调用智能体的一个被忽视的失败模式：历史痕迹虽然结构有效、语义合理，却已不再对当前请求具有权威性。作者称之为“历史污染”（history pollution），并证明这种污染能够劫持模型已经具备的正确决策策略，导致模型误用被污染的实体或接口约定。在Qwen3-1.7B上的实验显示，污染使32.1%的原本正确决策发生翻转，凸显了问题的严重性。

为了系统研究并解决这一问题，作者提出了两大部分贡献。第一是基准ContextPollute-Bench。该基准以配对方式构造，对每个任务提供三种同步视图：Original（原始、干净的历史）、Polluted（注入污染的历史）和Oracle State（当前真实状态的权威视图）。三者共享相同的系统策略、当前工具、最新请求和黄金下一动作，从而确保性能差异完全由历史状态造成，而不是任务本身的变化。基准还设计了十一种“黄金保持式”（gold-preserving）干预方法，从决策状态、实体绑定和接口执行等不同层面注入污染，覆盖完整工具调用和非调用决策两种场景。这一设计允许研究者精细地定位模型在哪一类状态误导下最脆弱。

第二是训练方法ORACLE-OPD（Oracle条件策略蒸馏）。核心思想是利用在Oracle状态下可用的教师策略，来指导仅能观察污染历史的学生模型。具体而言，学生先基于污染历史生成自己的前缀（而不是逐token模仿教师的完整输出），然后教师根据Oracle状态和学生的前缀提供软监督信号。这种基于学生生成前缀的软监督避免了将教师期望的轨迹直接强加给学生，因为学生和教师所处的状态不同。实验表明，在Qwen3-1.7B上，ORACLE-OPD达到87.0%的平衡工具使用准确率（BTA），大幅优于Gold-SFT（66.3%）、Oracle序列蒸馏（82.3%）和离策略token蒸馏（85.0%）。方法还表现出良好的规模扩展性：使用8B教师可以将相同的1.7B学生提升至91.9%，而8B学生本身可达到93.0%。

更重要的是，作者验证了训练得到的策略具有跨环境迁移能力。在冻结训练后检查点的情况下，模型能够...**

<!-- paperflow:ab8f36f77a8d0589 -->
## AppDeltaWorld: Transition-Grounded Delta Code World Model for Mobile GUI Agents

[[Deep Reading - Aug 2026/AppDeltaWorld-Transition-Grounded Delta Code World Model for Mobile GUI Agents|Deep Reading]]

[https://arxiv.org/pdf/2608.05891v1](https://arxiv.org/pdf/2608.05891v1)

- **AppDeltaWorld 是一篇面向移动 GUI Agent 的世界模型论文，核心主张是：将下一 GUI 状态建模为“可达的代码更新”，而不是无约束的图像或文本生成，可以在保真度、模态覆盖和动作转移一致性上同时取得改善。

论文从两个领域的痛点出发：一方面，真实移动交互轨迹在敏感应用和隐私关键操作中难以获取；另一方面，模拟环境搭建和维护成本高，现有 GUI 世界模型又存在生成不稳定、模态单一、转移逻辑不一致等问题。为此，作者提出一个四阶段流程：首先在动作转移约束下检索应用特定的 Level-1 HTML 参考；然后基于当前屏幕、动作、预测文本和检索结构生成 Level-2 可执行 HTML；再合成视觉资产并插入图像槽；最后通过浏览器渲染得到下一屏。整个过程是转移锚定的，即模型需要判断动作是否有效，对无效或低置信度转移予以拒绝，而非幻觉式生成。

作为世界模型，AppDeltaWorld 在 CMGUIBench-500 上以 Code2World 协议评估取得最高保真度，关键提升表现在结构布局和 UI 元素重建，验证了代码生成与视觉插入互补的有效性。作为训练环境，AppDeltaWorld 生成过滤后的闭环 SFT 数据，与公开监督结合训练 AppDeltaAgent，在 AndroidLens 上达到 SOTA，并在 MobileGym 和 MobileWorld 上一致提升。此外，论文展示了测试时强化学习能力：仅通过与世界模型交互即可进一步改善策略，无需接触真实应用。

论文的论证主线是“数据稀缺 -> 现有解决方案不足 -> 以转移约束的增量代码世界模型提供可扩展且高保真的替代 -> 通过世界模型与下游 Agent 的双重评估证明其价值”。技术主线是“层级 HTML 检索 + 条件可执行 HTML 生成 + 视觉资产插入 + 世界模型在环数据构建”。实验主线则分世界模型保真度和 Agent 能力两条线，并通过测试时 RL 进一步验证世界模型的可优化闭环性质。

整体而言，AppDeltaWorld 为移动 GUI 领域提供了一个具有实用潜力的合成数据引擎，既缓解...**

<!-- paperflow:ded294a2579c0570 -->
## Unified Agent: Managing Interactions across Devices

[[Deep Reading - Aug 2026/Unified Agent-Managing Interactions across Devices|Deep Reading]]

[https://arxiv.org/pdf/2608.05729v1](https://arxiv.org/pdf/2608.05729v1)

- **本论文针对 AI 智能体在“跨设备、跨时间”执行用户任务时的核心问题展开研究。作者指出，随着模型能力的提升，智能体应用场景正从单应用扩展到用户的多个设备，但现有系统在此类场景下性能不佳。问题的根源在于：交互观察（例如用户在哪台设备上说了什么、做了什么）散落在不同设备和不同时刻，而主流智能体系统并非围绕这一事实而设计。单智能体方法把设备视为工具，却缺乏对所有设备跨时间的统一状态管理；多智能体系统虽能协调多个代理，却也没有维护一个紧凑的“携带状态”来表达请求的全局背景。因此，智能体在需要决策时常常面临“证据不可见”的困境。

针对这一困境，论文提出核心原则：智能体应当维护一个紧凑、行动就绪的状态，该状态需要整合三类信息——交互证据（engagement evidence）、用户陈述事实（stated facts）和长期请求（standing request）。状态设计本身是决定系统成功与否的关键变量，而非增加系统复杂度或依赖更大模型。作者将这个原则具体实现为 Unified Agent，一个有状态智能体，它会随交互更新状态，并在每个时间点将状态与当前观察结合，产生行动。

为了验证状态设计的重要性，作者构建了专门的基准（benchmark），模拟跨设备、跨时间的用户-智能体交互任务，并用该基准对比了 Unified Agent 与四种已发表设计的改编版本。实验结果表明，在默认设置下 Unified Agent 的整体性能显著优于所有对比系统。随后，作者进行了一系列鲁棒性测试，更换底层多模态大语言模型（MLLM）家族、调整模型能力配置和推理努力（reasoning effort），结果 Unified Agent 始终保持领先地位，说明状态设计的优势不依赖于特定底层模型，具有很强的泛化能力。

论文的主要贡献包括：第一，形式化了跨设备、跨时间的用户-智能体交互问题，明确了状态设计在其中的核心地位；第二，提出了一个统一的智能体框架，通过紧凑状态组织交互证据、事实和请求；第三，构建了富有挑战性的基准，为后续研究提供公共评测工具；第四，通过多组实验验证了状态设计在不同 MLLM 环...**

<!-- paperflow:a756e325f55e5487 -->
## SkillZip: Contract-Preserving Graph Compression for Scalable Agent Skill Libraries

[[Deep Reading - Aug 2026/SkillZip-Contract-Preserving Graph Compression for Scalable Agent Skill Libraries|Deep Reading]]

[https://arxiv.org/pdf/2608.05604v1](https://arxiv.org/pdf/2608.05604v1)

- **本文提出了 SkillZip，这是一个旨在解决大规模智能体技能库管理难题的系统框架。针对现有方法在压缩过程中破坏执行契约、无法细粒度复用等问题，SkillZip 创新性地引入了“执行感知过程抽象”概念。它将技能转化为段落级过程图，通过识别跨技能的重复逻辑基元并将其重写为可逆的移植宏，实现了高倍率（3.46x）且保真的压缩。该框架不仅关注压缩率，更通过严格的依赖闭包和验证器可达性约束，确保了压缩后的技能在 LLM 上下文中依然可执行。配套的 ReZip 机制则通过执行反馈实现了技能库的动态维护。实验结果表明，SkillZip 在处理超大规模技能库（达 10 万级）时具有显著的性能优势，为构建高效、可靠、可扩展的智能体系统奠定了技术基础。**

<!-- paperflow:cd7313512bec9310 -->
## From Economic Agents to Agentic Economies: A Systems Blueprint for Economic World Models

[[Deep Reading - Aug 2026/From Economic Agents to Agentic Economies-A Systems Blueprint for Economic World Models|Deep Reading]]

[https://arxiv.org/pdf/2608.06020v1](https://arxiv.org/pdf/2608.06020v1)

- **本论文是一篇arXiv预印本，标题为“From Economic Agents to Agentic Economies: A Systems Blueprint for Economic World Models”，作者包括Jiale Han、Xiang Li、Jing Qian、Wenyuan Gu、Pin Gao、Ye Luo、Hongyuan Zha、Dacheng Tao、Benyou Wang和Lin William Cong。论文的核心是把经济学中新兴的“经济世界模型”（EWM）概念从一个理论愿景发展为可执行的技术路径。

论证主线始于一个信念：“要理解一个经济，我们不仅应该观察它的结果，而且应该构建一个生成这些结果的世界模拟器。”传统经济研究往往把模型当作解释或者预测的工具——要么通过计量方法拟合历史数据，要么在固定假设下推导未来。EWM则把模型变成实验引擎：用户和智能体可以对虚拟经济进行干预，观察经济系统如何从内部演化出来。为此，论文提出了一个系统的能力阶梯，把EWM的发展分为六个层次：
- 第一层：固定规则智能体世界（对应经典ABM），智能体行为由固定规则驱动；
- 第二层：自适应智能体世界，智能体具备一定的学习和适应能力；
- 第三层：基于LLM的智能体世界，用语言模型作为智能体大脑，产生更丰富的决策行为；
- 第四层：自演化智能体世界，智能体能够在仿真过程中演化自身的信念、目标和策略；
- 第五层：内生制度演化世界，市场规则、制度、机制等由智能体互动内生生成并演化；
- 第六层：仿真-现实经济孪生，系统与真实经济数据持续对齐，形成可信的双向映射。

技术主线是CS/AI系统的设计视角。论文在Cong (2025)的经济学框架基础上，将EWM视为生成式、交互式环境，研究“经济状态如何从异质智能体的决策、互动和适应中涌现”。这意味着需要把经济学概念（信念、行动、市场、制度）工程化为可计算模块，并保证这些模块可以组合成自洽的仿真系统。论文特别区分了“孤立的Economic Agent”与“完整的Agentic Economy”：单个经济智能体只在智能体...**

# Language Models

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Language Models
- 方法：language, vision-language-model, reasoning, vision, reinforcement-learning, optimization, multimodal-learning, safety
- 论文/报告：11 篇
- Self-Improving Large Language Models via Progressive Experience Evolution
- UEmbed: Unified Sparse and Dense Multimodal Embeddings
- SFT Conflicts, RL Coexists: A Theoretical and Empirical Analysis of Multi-Task Learning for LLMs
- Test-Time Scaling in Reasoning LLMs: Inference Regimes, Evaluation, and Reproducibility
- Hi-TTRL: Regulating Consensus with Hints for Test-Time Reinforcement Learning
- Benchmarking the Benchmarks: Testing the Predictive Validity of Commonsense Benchmarks
- PaDoc: Layout-Grounded Parallel Decoding for Document Parsing
- DASH: Divergence-Adaptive Supervision Horizons for On-Policy Self-Distillation of Reasoning Models
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:7c57531c056c9a45 -->
## Self-Improving Large Language Models via Progressive Experience Evolution

[[Deep Reading - Aug 2026/Self-Improving Large Language Models via Progressive Experience Evolution|Deep Reading]]

[https://arxiv.org/pdf/2608.02139v1](https://arxiv.org/pdf/2608.02139v1)

- **论文围绕“LLM 自改进的本质是什么”这一问题展开。作者提出，自改进不应只被看作策略优化问题，而应被看作“把瞬态交互信号转化为可复用、持久模型能力”的问题。他们引入“经验”概念：从交互轨迹中提炼出的紧凑、可迁移知识，包括可复用推理策略、任务不变约束和反复出现的失败模式。基于此，作者指出现有测试时方法和训练时方法各有短板：前者能显式使用经验但不能参数化内化，后者能参数化更新但缺乏可迁移经验的显式积累机制；两者之间缺少“经验蒸馏”这一中间阶段。为填补这一空白，论文提出 SPEE（Self-Progressive Experience Evolution），一个统一的后训练框架。SPEE 先执行显式经验演化：从多次交互收集的成功与失败轨迹中反思、提取并验证经验，通过持续演化的全局经验池过滤低效用经验、缓解事后合理化，然后通过特权引导的在线策略自蒸馏（OPSD）把经验内化进策略参数。接着执行隐式策略优化：用奖励驱动的强化学习在内化先验的基础上探索新策略，产生更丰富的轨迹，从而闭环推进自我改进。实验在五个数学推理基准、三个模型规模上显示 SPEE 一致超过测试时与训练时自进化基线；训练动态分析还表明经验蒸馏在提高奖励的同时避免策略崩溃、保留响应多样性，并可能带来数据效率优势。论文的贡献在于提出了一个经验中心的自进化视角、一个完整的经验蒸馏-策略优化闭环，以及一个可直接使用的开源实现。**

<!-- paperflow:eb6c1b7b5569b9ef -->
## UEmbed: Unified Sparse and Dense Multimodal Embeddings

[[Deep Reading - Aug 2026/UEmbed-Unified Sparse and Dense Multimodal Embeddings|Deep Reading]]

[https://arxiv.org/pdf/2608.02583v1](https://arxiv.org/pdf/2608.02583v1)

- **UEmbed 论文提出了一个统一的稠密与稀疏多模态嵌入模型，旨在解决现有学习型稀疏检索（LSR）局限于编码器架构、多模态扩展依赖辅助模块、以及稠密与稀疏表示分离这三个主要问题。论文的核心创新在于仅解码器的架构设计和词表划分策略：在输入序列后附加 N 个可学习特殊 token，并将词表分为 N 个不相交子集，每个 token 在因果前向中只预测自己子集上的稀疏权重，最后拼接为完整稀疏向量，同时从同一隐状态获得稠密表示。这种设计避免了全词表预测的高计算开销，并使得单一模型即可输出两种检索表示。模型在公开数据上训练，发布了 2B、4B、9B 三个规模。
实验方面，UEmbed 在 MMEB-v2 多模态基准上，UEmbed-9B 取得稠密 71.8、稀疏 71.0 的成绩，超越了基于公开数据训练的多模态嵌入模型（如 RzenEmbed）；在 BEIR 文本基准上，稠密设置的 nDCG@10 平均分达到 56.3（9B），与强基线竞争。论文还通过实验展示了三个实用优势：一是混合评分一致提升文本和视觉文档检索效果；二是设计兼容现有检索基础设施；三是在智能体应用中具有实际价值。
论文同时承认了一些局限：训练语料以英文和中文为主，存在语言与文化偏差；视频领域稀疏与稠密表示性能差距稍大，归因于高信息密度和时序复杂性；模型参数规模相对较大，可能带来部署成本。整体而言，UEmbed 提供了一种新的范式：在单一多模态骨干中统一稠密和稀疏嵌入，并使稀疏检索能够直接处理文本和视觉输入，为多模态检索系统的简化与扩展提供了新思路。**

<!-- paperflow:8a0c5e4831868c1f -->
## SFT Conflicts, RL Coexists: A Theoretical and Empirical Analysis of Multi-Task Learning for LLMs

[[Deep Reading - Aug 2026/SFT Conflicts, RL Coexists-A Theoretical and Empirical Analysis of Multi-Task Learning for LLMs|Deep Reading]]

[https://arxiv.org/pdf/2608.03573v1](https://arxiv.org/pdf/2608.03573v1)

- **本文系统分析了SFT与RL在多任务训练中的根本差异。作者首先通过初步实验发现一个鲜明现象：当LLM接受多阶段训练（依次学习不同任务）时，SFT会引发严重的任务冲突——优化新能力会极大破坏已有能力；而RL则展现稳定的任务共存——学习新任务的同时能保留甚至提升旧任务性能。为理解这一差异，作者从参数层面展开分析，观察到RL跨任务产生的参数更新是稀疏且近似正交的，而SFT的更新则存在明显的梯度干扰。为解释上述机制，作者对多任务梯度干扰进行理论分析，提出关键区分：SFT的干扰是范数受限的，其强度随绝对梯度幅值增长，因此任务间梯度大时冲突不可避免；RL的干扰则是方差受限的，借助优势归一化和在策略优化，RL的梯度方差被限制在很小的范围内，这导致不同任务的优化方向近似正交，从而天然避免冲突。这一理论不仅解释了实证观察，也为设计新方法奠定了基础。基于该洞见，作者提出Parallel-RL——一种解耦多任务训练的范式。与传统混合数据或多阶段SFT不同，Parallel-RL允许各任务独立或并行训练，再以模块化方式组合，极大提升了训练效率和灵活性，并以最少的适配获得甚至超越传统方法的性能。实验显示RL在多阶段训练中能带来24.9%的鲁棒性提升，进一步验证了共存特性的实际价值。总体而言，本文在现象、机制、理论和方法四个层面系统论证了“SFT冲突、RL共存”这一核心结论，为LLM多任务后训练提供了新的理论视角与实用方案。**

<!-- paperflow:d6dd9e9bf31a7aec -->
## Test-Time Scaling in Reasoning LLMs: Inference Regimes, Evaluation, and Reproducibility

[[Deep Reading - Aug 2026/Test-Time Scaling in Reasoning LLMs-Inference Regimes, Evaluation, and Reproducibility|Deep Reading]]

[https://arxiv.org/pdf/2608.04001v1](https://arxiv.org/pdf/2608.04001v1)

- **本文是一篇系统化视角的测试时扩展（test-time scaling）方法学论文，作者团队来自多个机构（具体机构未在证据中说明，但摘要页面字段 institution 为 null，且作者中有多位华人学生和教授，推测可能包含 Case Western Reserve University 等，但证据不足，不编造）。论文的核心主张是：当前对“测试时扩展”的理解过于松散，把多种本质不同的推理算法混为一谈，导致跨研究比较、评估和复现存在系统性困难。

论文首先提出统一形式化：将自回归 LM 视为隐式前缀树，测试时扩展是树上的预算受限推理。基于此，识别三种结构机制：（1）单轨迹顺序扩展（如长 thought chain）；（2）叶级扩展与终端归约（如 self-consistency、best-of-n）；（3）前缀级扩展（如搜索、MCTS）。三种机制在统计依赖性、计算核算和失败模式上不同，因此不能用一个标量预算统一衡量。

在评估层面，论文主张将“被测对象”设为整个推理系统而非单纯模型，并区分端到端系统性能和候选库诊断。作者设计了一个评估剖面（discovery–stability profile），其坐标和简单泛函能够恢复或界定常见重复采样指标（如 pass@k、majority@k、best-of-n 精度），并要求“协议匹配的报告”：即报告准确率时必须同时给出推理预算、采样协议、计算消耗和不确定性区间。

在可复现性层面，论文区分“精确回放”（exact replay，逐 token 复现）与“分布可复现性”（distributional reproducibility，精度分布可复现），并说明支撑每种层次所需的工件（种子、解码参数、环境、协议记录、重复运行次数等）。这为推理研究建立了一个更严格的报告标准。

论文还整理了开放权重推理生态（模型侧如思考模式与特殊令牌，接口侧如 API 参数与预算控制），并在广泛知识、符号推理和竞赛数学三类基准上应用上述原则，以实证展示系统级视角的实际后果。此外，作者构建了包含超过 20 亿条完整推理轨迹（token 级或轨迹级数量存在歧...**

<!-- paperflow:529006e353513503 -->
## Hi-TTRL: Regulating Consensus with Hints for Test-Time Reinforcement Learning

[[Deep Reading - Aug 2026/Hi-TTRL-Regulating Consensus with Hints for Test-Time Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2608.03545v1](https://arxiv.org/pdf/2608.03545v1)

- **本文提出了 Hi-TTRL（Hint-based Test-Time Reinforcement Learning with Consensus Regulation），用于解决测试时强化学习（TTRL）中多数投票伪标签的共识强度不稳定问题。

背景上，强化学习已被证明能有效增强大语言模型的推理能力，特别是 RLVR 范式使用可验证奖励取得了显著成果，但它依赖大量的带标注数据，难以规模化。TTRL 通过多数投票构造伪标签来摆脱外部标注，但其奖励信号对共识强度高度敏感：低共识会放大不可靠伪标签的优势，高共识又会压缩奖励对比度并导致梯度消失。

Hi-TTRL 的解决方案分为三步：首先，在完整 rollout 前从部分样本估计共识强度；其次，判断其是否落在预设目标区间内，若偏离则触发 MCMC hint 采样器；第三，采样器以幂变换前缀分布为目标，通过有限步采样生成前缀作为提示。幂指数 $\alpha>1$ 锐化分布促进收敛，$\alpha<1$ 平坦化分布促进探索，从而把最终 rollout 的共识强度拉回目标区间。这种方式从采样源头动态调节分布，既不强制模型输出，也不破坏策略的自反馈学习过程。

实验方面，论文在多个数据集和三个骨干模型上对比了 Hi-TTRL 与标准 TTRL，结果显示前者在所有骨干上均取得最佳平均性能，提升一致。消融实验验证了自适应调节相对固定策略和去除调节的优越性；共识调节分析展示了提示对 rollout 共识分布的影响；case study 和机制分析提供了更定性的理解。

总体而言，Hi-TTRL 将 TTRL 中“奖励设计”的矛盾转化为“采样分布控制”的问题，提出了一种新颖且有效的自适应共识调节机制。它既不是对奖励函数直接打补丁，也不是简单的采样温度调整，而是利用 MCMC 采样、以幂变换分布为桥梁，把估计-控制-生成连成闭环。

本文的主要贡献包括：明确刻画了共识强度在 TTRL 中的双重矛盾；首次提出在采样阶段直接调节共识强度的框架；设计了一种基于幂变换前缀分布的 MCMC hint 采样器；通过实验证明该框架在多种设置下的有效性。局限在...**

<!-- paperflow:99e24ed3da6b8198 -->
## Benchmarking the Benchmarks: Testing the Predictive Validity of Commonsense Benchmarks

[[Deep Reading - Aug 2026/Benchmarking the Benchmarks-Testing the Predictive Validity of Commonsense Benchmarks|Deep Reading]]

[https://arxiv.org/pdf/2608.03340v1](https://arxiv.org/pdf/2608.03340v1)

- **本文以“基准的基准”（Benchmarking the Benchmarks）为视角，检验标准化常识基准对下游任务的预测效度，是评估领域一项系统性的实证研究。论文的论证主线是：常识基准被广泛用作LLM能力的代理指标，但其对现实世界任务的预测能力未被充分量化；因此作者引入准则效度（criterion validity）的概念，将下游任务表现作为基准有效性的判据。技术主线上，论文构建了一个多任务、多模型的评估矩阵：23个模型来自六个家族，覆盖四个标准常识基准、四个重做变体、三个非常识控制任务和八个下游任务；统一采用对数似然多选评估以减少任务间噪声，并通过Spearman相关、偏相关和留一家族交叉验证刻画预测关系。实验主线揭示了三个关键结果：第一，重做基准基本保持原始模型排名，意味着修复数据集内部质量问题并不会实质改变模型相对能力判断；第二，常识基准的跨家族预测效度是任务依赖的，仅在少数下游任务上稳定成立，其余情况增益有限或仅体现在特定度量上；第三，基准标签所暗示的常识子领域（社会、物理、时间等）与下游任务实际所需推理类型经常错位，而事件/物理类基准对同轴任务预测力较强，说明概念对齐而非表面标签决定了预测效度。论文最终主张，标准化常识基准提供的证据是任务依赖的，而非普遍性的常识能力证明，这为基准设计者和模型评测者提了个醒：依赖单一或少数基准总分来衡量模型常识能力可能造成误导。总体而言，本文的方法论贡献在于提出了一个可复用的预测效度检验框架，其实证发现则推动了对静态基准价值的重新审视。**

<!-- paperflow:b9f617919d3ff5ae -->
## PaDoc: Layout-Grounded Parallel Decoding for Document Parsing

[[Deep Reading - Aug 2026/PaDoc-Layout-Grounded Parallel Decoding for Document Parsing|Deep Reading]]

[https://arxiv.org/pdf/2608.06146v1](https://arxiv.org/pdf/2608.06146v1)

- **PaDoc 论文针对端到端文档解析中的序列化解码效率问题，提出一种布局锚定的并行解码方法。文档解析任务需要从页面图像中提取布局区域、阅读顺序、文本、公式、表格和图形等结构化表示。现有端到端解析器将布局和内容序列化成扁平 token 序列，强制独立区域沿一条随总内容长度增长的解码路径顺序执行；两阶段裁剪方法虽然可以并行，却因重复视觉编码和上下文碎片化损失效率与全局信息。PaDoc 的核心洞察是，文档布局本身就提供了并行结构：在给定页面图像和区域布局条件下，不同区域的内容生成是近似独立的。为此，论文提出区域充分性假设，并推导出前缀条件分解，将联合概率分解为布局流和并行区域内容分支。训练阶段，PaDoc 在单个多模态大语言模型中引入打包变长祖先注意力，通过逻辑祖先掩码让每个区域分支只可见图像与对应布局祖先，而屏蔽兄弟区域内容，从而在标准 next-token 训练目标下实现并行分支训练，并且避免了密集注意力矩阵。推理阶段，PaDoc 使用掩码并行解码，将区域分支映射为 vLLM 后端上的并发请求，并利用缓存中的共享前缀复用，避免重复视觉预填充，将解码深度压缩到最长布局-内容路径。实验在 OmniDocBench Full 上进行质量评估，PaDoc 获得总体布局 F1 91.1，端到端总体 94.24，文本编辑距离 0.038，公式 CDM 95.59，均达到顶级水平；在 384 页子集与单张 A800 GPU 上，PaDoc 在五种并发级别下都是最快的端到端解析器，相比同骨干 Sequential SFT 基线吞吐提升 67.4%–118%，P95 延迟降低 39.2%–54.9%。此外，尽管只有 2.1B 参数，PaDoc 超过了 1.0B 的 HunyuanOCR-1.5，验证了布局锚定并行能显著降低服务成本。论文贡献可概括为：提出了基于布局分支的并行解码公式化分解；设计了一种无架构修改的单 MLLM 训练与推理方案；并通过全面的实验证实其在质量与速度上的优势。PaDoc 的主要局限可能在区域充分性假设对复杂跨区域版面的限制，以及当前评测集中度（单 GPU、单数据集子集...**

<!-- paperflow:272d19aebbdcf325 -->
## DASH: Divergence-Adaptive Supervision Horizons for On-Policy Self-Distillation of Reasoning Models

[[Deep Reading - Aug 2026/DASH-Divergence-Adaptive Supervision Horizons for On-Policy Self-Distillation of Reasoning Model|Deep Reading]]

[https://arxiv.org/pdf/2608.06243v1](https://arxiv.org/pdf/2608.06243v1)

- **本文针对可验证奖励强化学习（RLVR）中奖励信号稀疏的问题，研究 on-policy self-distillation（OPSD）中密集 token 级监督的有效分配方式。核心观察是标准 OPSD 对所有局部散度赋予相同系数，忽略了生成过程中散度演化的时间结构。由于相同的局部散度可能出现在不同的散度历史之后，反映教师与学生失配的不同演化路径，标准的单点标量无法区分这些时间上下文，导致监督权重无法自适应。为此，作者提出 Divergence-Adaptive Supervision Horizons（DASH），将每个局部蒸馏信号与序列级均值的差距映射为自适应传播门，并利用这些门控制反向多步聚合，使 token 级监督权重随局部散度的演化动态调整。这一方法路径依赖地调整了监督量，并形成序列级目标函数。在三个数学推理基准、三个模型规模上的实验表明，DASH 在每个基准和每个规模上都优于匹配的 vanilla OPSD 重运行，且不增加额外前向计算。本文的贡献包括：识别了标准 OPSD 中的时间系数分配差距；提出了序列感知的自适应监督聚合方法 DASH；通过多基准多规模的实验验证了其有效性。局限在于：实验仅限于数学推理任务，未提供理论分析，也没有报告详细的消融和失败案例。整体而言，这是一项针对自蒸馏训练监督分配的系统性改进工作，与生成、强化学习等方向相关。**

<!-- paperflow:ff32cadf5b949386 -->
## What Current AI Benchmarks Leave Unmeasured: Modality, Search, Citations, and Implications (for Safety Evaluations)

[[Deep Reading - Aug 2026/What Current AI Benchmarks Leave Unmeasured-Modality, Search, Citations, and Implications (for S|Deep Reading]]

[https://arxiv.org/pdf/2608.06202v1](https://arxiv.org/pdf/2608.06202v1)

- **这篇论文是一项针对 LLM 安全评估方法论的系统性审计研究，核心论点是：现有的基准评估框架在测量维度上存在结构性缺失，导致基于这些评估得出的安全结论并不可靠。

**研究背景**：LLM 评估结果经常被引用来证明模型“安全”“可靠”“可部署”，但标准评估流程固守在几个默认设置上——只用 API 访问、每个 prompt 只跑一次、只报准确率。现实部署中，用户通过聊天界面交互，模型可能带有 web search，且随机采样导致同一问题可能得到不同回答。这些被忽略的条件是否影响评测结论？作者决定对最广泛使用的模型进行严格审计。

**实验设计**：选取两个高引用安全/偏见基准——BBQ（偏见）与 SafetyBench（安全），分层抽取 401 条提示。每个提示在“ChatGPT 聊天界面 vs OpenAI API”以及“启用 vs 禁用 web search”组成的 4 种条件下各运行 3 次，共收集 4812 条响应。评估指标包括准确率、响应一致性、文本相似度、引用溯源和弃权行为。

**关键结果**：(1) 搜索关闭时，UI 在两个基准上的准确率均低于 API；(2) 启用 web search 使准确率最高下降 8 个百分点，并在一项基准上逆转了模态优劣方向；(3) 重复运行中高达 21% 的 prompt 产生不一致答案；(4) 两种模态给出了不同的引用，弃权行为也高度不一致。模态间准确率差异虽仅 2–3 个百分点且统计显著，但更重要的是这些行为变异在常规准确率报告中完全不可见。

**理论意义**：作者将 LLM 评估重新定义为“测量设计问题”，强调安全评估必须像科学测量一样明确条件变量（模态、搜索、重复次数、回应级属性），并指出单点准确率指标掩盖了模型行为中的关键变异性。他们的审计不仅揭示了方法论缺陷，也提供了可复现的审计协议（代码已开源）。

**实践价值**：对 AI 安全实践者、第三方审计机构和监管者而言，该工作提供了具体警示：任何声称“模型在基准上安全”的结论都应附带其测试条件；对高风险的部署决策，应进行多条件、多次运行的稳健性评估。

**局限**...**

<!-- paperflow:17c00761456edd11 -->
## On-Policy Self-Distillation without Any Supervision

[[Deep Reading - Aug 2026/On-Policy Self-Distillation without Any Supervision|Deep Reading]]

[https://arxiv.org/pdf/2608.06296v1](https://arxiv.org/pdf/2608.06296v1)

- **本论文《On-Policy Self-Distillation without Any Supervision》提出了一种完全无监督的 on-policy 自蒸馏范式，目标是用模型自身的内部一致性来替代 all 外部监督信号，重新定义“自蒸馏”的边界。

论证主线：作者首先指出现有 on-policy 自蒸馏（OPD/OPSD）虽然强调“self”，但实际仍依赖 ground-truth、环境反馈或更大教师模型，这些外部信号限制了方法的普适性与可持续性。他们进一步分析 on-policy 蒸馏的优势——在学生状态上提供密集 token 级监督并缓解 off-policy 的分布失配——但这种优势建立在可以获得正确参考解的前提之上。本文的核心论证是：多数投票共识（majority-vote consensus）可以充当这一参考，因为模型自身多次采样中一致出现的答案通常比单次采样更可靠，且只对模型“自信但错误”的轨迹进行定向蒸馏，可以规避自训练中的确认偏误。

技术主线：作者提出 U-OPSD 算法，包含四个关键环节：1）采样多个 rollout；2）通过 self-consistency 阈值下的多数投票构造伪解，并取最短伪解作为教师条件；3）识别与伪解不一致的最长错误完成序列；4）将该错误序列的前缀作为学生状态，以最短伪解为条件构造教师分布并蒸馏回模型。这一设计使得训练信号完全来自模型自身，无需任何外部标签。整个流程在数学上可以理解为：在模型对自己“很有把握但实际跑偏”的状态附近，用自身共识分布的‘重心’拉回生成过程。

实验主线：作者在五个数学推理基准（AIME24、AIME25、HMMT25、MATH500、AMC23）上，使用 Qwen3 4B 和 8B 模型，分别在非思考模式与思考模式下评估。结果分为三个层次：1）相对于基础模型，非思考模式下 4B 提升 8.5%、8B 提升 10.7%，说明无监督自蒸馏能带来显著增益；2）相对于使用 ground-truth 的有监督 OPSD，非思考模式下平均高出 3.2%（4B）与 2.3%（8B），思考模式下 4B 高...**

<!-- paperflow:daea09cb2be2cbc9 -->
## Learning When to Trust via Selective Context Preference Optimization

[[Deep Reading - Aug 2026/Learning When to Trust via Selective Context Preference Optimization|Deep Reading]]

[https://arxiv.org/pdf/2608.06377v1](https://arxiv.org/pdf/2608.06377v1)

- **论文针对语言模型在外部信号影响下回答易被误导的问题，提出应从“选择性信任”而非“单纯抗误导”的角度来评估和改进模型。作者指出，现有直观方法（训练模型抵抗误导信号）会导致模型全盘不信任上下文，在上下文有用时不再利用；在单条件评测中这种退化行为与理想的选择性信任行为无法区分。为此，论文构建了MIST基准：将每个推理题目人工标注为四种匹配条件（干净、误导、正确上下文、无关上下文），保证共享题目内容，只改变上下文角色。基于该基准提出SC2W配对指标，统计干净-正确回答被误导信号翻转的比例，直接衡量易感性。在多个开源模型上的研究表明，这种翻转现象普遍存在，甚至前沿模型也会被外表权威的错误提示欺骗。在此基础上提出SCOPE训练方法：挖掘clean-correct和misleading-wrong的失败配对作为偏好对，构造信号反事实样本，并在四种条件上均衡优化标准DPO目标，使模型学会根据上下文角色决定信任程度，而不是简单忽略上下文。实验显示SCOPE大幅降低SC2W，同时保持模型在干净、正确和无关上下文上的准确率，证明了选择性信任目标的可行性。最后，论文主张模型评测应综合“抗误导能力”和“利用有用上下文能力”，以选择性信任为标准。局限包括限定的文本诊断设置、模型家族覆盖有限、无法排除数据污染等。**

# Computer Vision

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Computer Vision
- 方法：generation, language, vision-language-model, vision, reinforcement-learning, vision-language, deep-learning
- 论文/报告：5 篇
- CAPEval: A Decoupled Caption Evaluation across Understanding and Generation
- DIVE: Dynamic Iterative Visual Evidence Construction for Efficient Vision-Language Models
- HelloWorld: Enabling Socially Interactive Characters in Video World Models
- WorldCycle: Self-Verifiable Reinforcement Learning for Long-Horizon Video World Models
- SkillMemo: Expert-guided Skill Memory Framework for Compositional Embodied Manipulation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:70528b9e211282c5 -->
## CAPEval: A Decoupled Caption Evaluation across Understanding and Generation

[[Deep Reading - Aug 2026/CAPEval-A Decoupled Caption Evaluation across Understanding and Generation|Deep Reading]]

[https://arxiv.org/pdf/2608.02589v1](https://arxiv.org/pdf/2608.02589v1)

- **本文针对图像描述评估中长期存在的“质量定义模糊”问题，提出了 CAPEval 框架。该框架的核心贡献在于将描述质量拆解为 Coverage（覆盖度）和 Precision（精确度）两个独立维度。通过构建包含人工验证原子项的基准数据集，作者对 10 种描述生成器进行了详尽评估。

实验部分是本文的重头戏。作者通过控制变量法，将不同质量特征的描述投入到多模态理解和文本生成图像的训练流程中。结果揭示了一个重要的规律：理解任务更看重“写得全”（Coverage），而生成任务更看重“写得对”（Precision）。

这一结论具有极强的实战意义。对于构建大规模多模态数据集的研究者来说，如果目标是训练类似 CLIP 的理解模型，可以适当容忍描述中的噪声以换取更丰富的语义覆盖；而如果是为了训练 Stable Diffusion 类的生成模型，则必须优先过滤掉含有幻觉的描述。CAPEval 不仅提供了一个评估工具，更揭示了多模态学习中数据质量与任务目标之间的深层联系。这种解耦的视角为未来多模态数据的精细化治理和模型性能的针对性提升指明了方向。**

<!-- paperflow:c7fb50ed3a08f593 -->
## DIVE: Dynamic Iterative Visual Evidence Construction for Efficient Vision-Language Models

[[Deep Reading - Aug 2026/DIVE-Dynamic Iterative Visual Evidence Construction for Efficient Vision-Language Models|Deep Reading]]

[https://arxiv.org/pdf/2608.04496](https://arxiv.org/pdf/2608.04496)

- **DIVE 论文的论证主线是：VLM 推理效率的瓶颈在于视觉 token 数量远大于文本 token，而现有的剪枝方法采用单次打分+静态 top-k 选择，这种范式忽略了 token 价值的条件性——一个 token 是否值得保留取决于已经保留了哪些证据。由此提出关键洞见：视觉 token 的价值不是独立静态属性，而是相对于当前已构建证据集的边际贡献。技术主线上，DIVE 将剪枝重新定义为动态证据构建，采用 select-update-re-evaluate 三阶段循环：每轮选出残差条件化得分最高的剩余 token，然后对视觉与提示残差做耦合单侧更新以折扣已解释证据，再在更新后的残差上重新评估剩余 token，重复直到填满预算。这种设计使得最终保留的 token 集合既互补又提示相关，避免了重复覆盖同一图像区域。实验主线上，论文在 8 个图像理解基准上用 lmms-eval 官方协议做零样本评估，主受控实验使用 LLaVA-1.5-7B（每图 576 token），对比了基于注意力、提示相关性、视觉冗余和重要性-多样性的主流剪枝方法，并额外纳入 MMTok、CDPruner、SCOPE 等贪心子集选择方法。核心定量结果是：在减少 88.9% 视觉 token 的条件下，DIVE 保留未压缩模型平均性能的 98.2%；在温和预算下甚至达到 100.3% 的相对性能。综合来看，DIVE 的主要贡献是一个免训练、即插即用的迭代剪枝框架，以少量额外迭代计算换取极端压缩率下的性能保持，为 VLM 高效推理提供了不同于静态打分的新范式。**

<!-- paperflow:14df2ae469ce44ea -->
## HelloWorld: Enabling Socially Interactive Characters in Video World Models

[[Deep Reading - Aug 2026/HelloWorld-Enabling Socially Interactive Characters in Video World Models|Deep Reading]]

[https://arxiv.org/pdf/2608.05070](https://arxiv.org/pdf/2608.05070)

- **本文提出 HelloWorld，一个能够在视频世界模型中生成面向观众的多模态社交互动的模型，并配套推出评测基准 HelloWorldBench。论文首先指出现有视频世界模型的两个核心缺陷：一是基准设计只关注场景保真和相机可控性，完全忽视角色存在；二是模型本身缺乏生成角色对观看者做出有意义社交行为的机制。为此，作者从模型与评测两方面入手。

在模型方面，HelloWorld 由两个主要组件构成：自蒸馏与相机姿态条件化。自蒸馏使用一个基础世界模型作为教师，在蒸馏过程中向学生模型注入交互能力，同时保持原有视频质量。相机姿态条件化允许模型在交互时依然响应用户的相机控制，避免以往交互模型常见的“冻结相机”或画质退化问题。在推理阶段，模型引入一个无需训练的交互触发模块，该模块与用户按下的交互按钮 F 绑定，当按钮被按下时，它会对生成过程中的交叉注意力进行调制，促使角色执行与文本提示一致的、面向观看者的动作、手势、表情和语音。

在评测方面，作者提出 HelloWorldBench，这是第一个专门评估世界模型中观看者导向社交互动的基准。该基准覆盖了多模态互动以及非人类角色（动物、机器人、玩具等），填补了现有基准只重视场景与相机的空白。

实验环节，论文在 HelloWorldBench 上与多个现有世界模型（如 WorldPlay、Matrix-Game 3.0）进行了对比。结果表明，在三个社交交互指标上，HelloWorld 以较大优势超越现有方法，而基线模型却倾向于生成静态场景，凸显出它们缺乏角色交互能力。同时，HelloWorld 的视频生成质量与最强基线相当或略高，证明了自蒸馏的有效性。为了评估交互的用户感知质量，论文还组织了 30 名评分者对 41 个样本进行成对比较的用户研究，进一步确认了 HelloWorld 在主观体验上的优势。

总体而言，HelloWorld 首次将多模态社交互动系统性地带入了视频世界模型，不仅提供了可行的技术方案，还为未来该方向的研究设立了基准。该工作有望推动 world model 从“场景模拟器”向“具有社交意识的交互式环境”演进，对于虚拟角...**

<!-- paperflow:a8568951fcd4804b -->
## WorldCycle: Self-Verifiable Reinforcement Learning for Long-Horizon Video World Models

[[Deep Reading - Aug 2026/WorldCycle-Self-Verifiable Reinforcement Learning for Long-Horizon Video World Models|Deep Reading]]

[https://arxiv.org/pdf/2608.04964](https://arxiv.org/pdf/2608.04964)

- **本文针对交互式视频世界模型（IWM）长时程生成中的误差累积问题，提出了一种名为 WorldCycle 的自验证强化学习框架。

**研究动机**：IWM 将生成式视频模型转化为交互式模拟器，对长时程规划与探索至关重要，但自回归方式导致误差累积。现有的 RL 后训练方法受限于验证瓶颈——没有真实未来状态来度量长期漂移，因此只能优化短时程视觉质量或单步动作对齐。

**核心洞察**：可逆动作循环能够提供自验证监督。一个动作序列与其逆动作序列构成的闭合路径，在解析上必须返回初始状态。这一性质无需标注轨迹，即可作为长时程正确性的监督信号。

**方法**：
1. 从普通动作序列构造闭合动作循环及其重复执行版本。
2. 设计两个奖励函数：空间闭合奖励（利用正向与反向片段的镜像结构，要求对应中间状态一致）和时间一致性奖励（要求多次循环执行中的状态对齐）。
3. 用 RL 对预训练 IWM 进行后训练，使动作被学习为一致的状态算子。
4. 该方法可直接优化基础模型不擅长的分布外复合动作循环。

**基准**：发布 CycleBench，这是第一个评估视频世界模型作为状态转移模拟器的诊断基准。

**结果**：WorldCycle 将状态回归漂移降低最多 44%，复合动作准确率提升近 4 倍。这些改进为物理 grounded 世界模型提供了基础。

**讨论**：作者强调该框架不是简单的结构正则化，而是基于解析已知闭合关系的 RL 后训练方法；未来可扩展到近似可逆场景和其他动作模态，如机器人操纵。**

<!-- paperflow:ea763f4639d9049a -->
## SkillMemo: Expert-guided Skill Memory Framework for Compositional Embodied Manipulation

[[Deep Reading - Aug 2026/SkillMemo-Expert-guided Skill Memory Framework for Compositional Embodied Manipulation|Deep Reading]]

[https://arxiv.org/pdf/2608.05970v1](https://arxiv.org/pdf/2608.05970v1)

- **SkillMemo 论文的核心目标是解决具身操作模型在数据受限条件下组合泛化能力不足的问题。文章首先指出，虽然 Diffusion Policy（DP）和 Vision-Language-Action（VLA）模型在机器人操作任务上取得了显著成功，但它们的高度数据依赖限制了其在分布外（OOD）场景下对可重用技能结构的捕捉，导致面对未见任务配置时性能急剧下降。

针对该问题，论文提出 Skill-Based Memory（SkillMemo）框架，其技术主线为“隐式技能分解 + 情景记忆检索”。具体地，SkillMemo 包含两个核心组件：
1. **专家引导轨迹分割模块**：基于 Mixture-of-Experts（MoE）架构，通过训练门控系数将长视界演示隐式地切分为多个潜在原子技能。该模块不依赖人工标注，能够自动发现技能的时序边界。
2. **技能级情景记忆库**：将每个技能表示为紧凑的键值对存储，键负责检索，值编码技能内容。该记忆库具有动态性，可在训练中积累新技能。
推理时，模型根据当前输入从记忆库检索最相关的技能原语，并将其与当前门控分布融合，从而在动作预测时注入技能级上下文先验。这种设计既保留了隐式技能发现的优点，又引入了显式记忆的可复用性。

实验方面，论文在模拟基准和真实世界 UR5e 机械臂上进行了全面评估。单任务评估（表 6）显示 SkillMemo 在全部五个接触丰富任务上均优于原始 DP，尤其在 Corn in Pot 任务上提升了 7.5%；组合泛化评估（表 7）采用有限源任务训练、未见任务对测试的协议，验证了技能记忆对组合重组的促进作用。此外，论文还实例化了 DP 和 VLA 两种骨干，证明 SkillMemo 的模型无关性。最终，SkillMemo 被评为达到最先进性能，并超越 π0.5。

论文的论证主线可概括为：数据稀缺 → 显式/隐式技能分解不足 → 提出 MoE 分割与记忆检索融合 → 实验验证组合泛化。主要贡献包括：第一，提出基于 MoE 的隐式轨迹分割方法，能自动发现原子技能；第二，设计技能级情景记忆库，以键值对形式存储和检索技...**

# AI for Education

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Education
- 方法：agent, multimodal-learning, deep-learning
- 论文/报告：2 篇
- Beyond Simply Environment Scaling: Designing Effective Environment Distributions for Multimodal Agent Learning
- M$^3$R-Bench: A Unified Benchmark for Evidence-Grounded Multimodal Metaphor Understanding
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:bf83492a0705e2d5 -->
## Beyond Simply Environment Scaling: Designing Effective Environment Distributions for Multimodal Agent Learning

[[Deep Reading - Aug 2026/Beyond Simply Environment Scaling-Designing Effective Environment Distributions for Multimodal A|Deep Reading]]

[https://arxiv.org/pdf/2608.03571v1](https://arxiv.org/pdf/2608.03571v1)

- ****1. 论证主线**
本文挑战了“环境数量越多越好”这一直觉。作者首先通过系列实验证实了单纯扩大环境池在训练多模态智能体时并不总带来增益，进而提出：环境分布的有效性取决于两个结构性因素——多样性和难度结构。这一论点将问题从“规模扩展”转向“分布设计”。

**2. 技术主线**
- 针对多样性，设计 **AES（Ability-aware Environment Selection）**，通过感知智能体能力来选择互补的环境子集，避免重复与冗余。
- 针对难度结构，设计 **HDC（Hierarchical Difficulty Curriculum）**，从“约束削弱”和“状态规模递进”两个层级组织课程，引导智能体逐步适应更复杂任务。
- 两个方法分别对应环境分布的两个设计维度，共同构成一个更系统的环境设计框架。

**3. 实验主线**
- 初步实验验证“Naive Scaling”的局限，证明数量增长的非单调性。
- 端到端实验对比 AES/HDC 与简单环境池的训练效果，证明新方法有效。
- 结论还指出有效性依赖于多样性与难度结构，并讨论了当前方法的条件与局限。

**4. 主要结论**
环境分布的设计比单纯扩大环境数量更重要；基于能力感知的多样性选择与层次化难度课程能显著改善多模态智能体训练。未来工作需在更大规模与更丰富环境上进一步验证。

**5. 信息缺口**
由于检索片段有限，论文中具体的实验数值、环境描述、基线细节、方法实现（如 AES 的算法步骤、HDC 的调度规则）均未获得，需要阅读原文补全。**

<!-- paperflow:5ef5e86cda848990 -->
## M$^3$R-Bench: A Unified Benchmark for Evidence-Grounded Multimodal Metaphor Understanding

[[Deep Reading - Aug 2026/M$^3$R-Bench-A Unified Benchmark for Evidence-Grounded Multimodal Metaphor Understanding|Deep Reading]]

[https://arxiv.org/pdf/2608.05817v1](https://arxiv.org/pdf/2608.05817v1)

- **本文围绕多模态隐喻理解,提出统一且以证据接地的基准M³R-Bench,并在此基础上开发了训练方法M³R-Reasoner,旨在解决现有评测碎片化、缺乏解释依据的核心问题。

**研究动机**:隐喻是跨域映射的语言现象,在多模态场景中,视觉和文本共同构建Target–Source映射,理解隐喻不仅需要识别概念对应,还需要恢复情感态度和交际意图。然而,已有基准(MultiMET、MultiCMET、MultiMM)各自独立评估部分子任务,不要求模型提供证据说明,导致无法判断模型是真正理解还是基于浅层线索。作者通过初步实验发现,即使是先进MLLM如GPT-5.5,在视觉证据利用和映射正确性上都很薄弱,具体表现为视觉证据得分29.66、映射正确性43.95,且常将映射替换为主题级概念。这些发现催生了统一、证据接地的评测需求。

**基准设计**:M³R-Bench包含1,000个图像-文本实例,所有标注经人工验证。注释基于概念隐喻理论和非字面语言理解理论,为每个实例提供四类联合标注:(1)隐喻出现与否;(2)Target–Source映射;(3)情感倾向;(4)分阶段解释,阶段路径为“证据识别→映射建立→情感推断”。这种设计不仅明确指出了应该是哪些视觉/文本证据支撑映射,还限定了推理过程的顺序,使得评测能够定位模型在哪个环节失败。

**模型方法**:针对评测暴露的跨模态证据-映射不匹配,论文提出M³R-Reasoner。该方法包含两个关键训练阶段:一是课程式推理监督,通过逐步递增难度的分割监督或示例,让模型学会产出结构化的证据-映射-情感推理链;二是任务感知强化学习,利用与任务紧密相关的奖励(如rubric评分、证据覆盖度、映射正确性)对输出进行优化,使模型输出与人类对证据忠实性的偏好对齐。仅使用8B参数量的骨干,方法在四个统一任务指标上超越更大专有模型。

**实验结果**:评测显示M³R-Reasoner在Visual Evidence和Sentiment Justification上相比GPT-5.5分别提升28.45和30.11分,平均rubric分数超越Claude...**

# AI for Science

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science
- 方法：agent, gui-agent
- 论文/报告：1 篇
- EviGraph: Evidence-Guided Autonomous Research Agents
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:623d33fa5ff4f2e3 -->
## EviGraph: Evidence-Guided Autonomous Research Agents

[[Deep Reading - Aug 2026/EviGraph-Evidence-Guided Autonomous Research Agents|Deep Reading]]

[https://arxiv.org/pdf/2608.04738](https://arxiv.org/pdf/2608.04738)

- **本文针对自主科研智能体在逻辑严密性和结论可靠性方面的核心痛点，提出了 EviGraph 框架。该框架摒弃了传统的线性流水线模式，转而采用“类型化证据图”作为科研过程的核心载体。通过将科研抽象为问题、缺口、假设、实验、发现和主张六个关键环节，并建立显式的依赖关系，EviGraph 实现了对科研逻辑的实时监控与动态修复。实验结果证明，这种以证据为中心的架构不仅能显著提升论文的质量，更重要的是，它为自主科研引入了急需的“诚实性”和“可解释性”。EviGraph 的成功表明，未来的 AI 科研助手不应仅仅是高效的文本生成器，而应是严谨的逻辑构建者和证据维护者。**

# Machine Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Machine Learning
- 方法：agent, reinforcement-learning, deep-learning
- 论文/报告：3 篇
- OPD-V: Visual On-Policy Self-Distillation with Modality Balance
- AgentOPSD: Recursive Self-Distillation for Agentic Reinforcement Learning
- ML-for-ML
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:e3cebc1c38921bde -->
## OPD-V: Visual On-Policy Self-Distillation with Modality Balance

[[Deep Reading - Aug 2026/OPD-V-Visual On-Policy Self-Distillation with Modality Balance|Deep Reading]]

[https://arxiv.org/pdf/2608.05131](https://arxiv.org/pdf/2608.05131)

- **本文提出了 OPD-V，一种旨在解决多模态大语言模型（MLLM）在同策略自蒸馏（OPSD）中模态失衡问题的创新框架。作者指出，MLLM 在后训练阶段往往会陷入过度依赖文本先验的陷阱，导致视觉信息的利用率不足。为了解决这一问题，OPD-V 引入了“模态平衡置信区域”的概念，通过构建正向教师（Zoom-In 图像）和负向教师（Mask 图像）来提供差异化的监督信号。通过测量模态平衡注意力比例，OPD-V 能够识别出模型推理中的失衡状态，并利用 Jensen-Shannon 蒸馏机制引导学生模型向更平衡、更依赖视觉证据的方向优化。在 6 个基准测试和 4 个模型主干上的广泛实验证明，OPD-V 不仅提升了模型的整体准确率，还增强了模型对视觉细节的感知能力。该研究为 MLLM 的后训练优化提供了一个全新的视角，即通过显式建模模态平衡来提升模型的鲁棒性和推理质量。**

<!-- paperflow:9afef00c5a489bc6 -->
## AgentOPSD: Recursive Self-Distillation for Agentic Reinforcement Learning

[[Deep Reading - Aug 2026/AgentOPSD-Recursive Self-Distillation for Agentic Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2608.05987v1](https://arxiv.org/pdf/2608.05987v1)

- **论文围绕长程多轮智能体强化学习中的信用分配难题展开。论证主线是：基于可验证奖励的 RL 通过轨迹级优势进行策略优化，但在长程交互中，最终结果往往只由少数关键决策决定，轨迹级优势无法精准归因这些决策；已有自蒸馏（OPSD）方法提供了局部密集信号，但“局部信号如何表达序列信用”仍是开放问题。论文的核心洞察是：一个回合的信用不应由它自身的局部信号孤立决定，而应由该信号对最终成功概率估计的改变量决定。为形式化这一直觉，论文将回合级师生对数概率差视为证据，在对数几率空间中进行递归贝叶斯更新，得到对任务成功的信念状态，并将回合信用定义为连续状态间的边际修正。这样既保留了局部信号的密集性，又通过递归更新引入了历史依赖，输出有结构的回合级信用信号。技术主线上，AgentOPSD 是无批评器方法，兼容标准策略优化（如 GRPO），不需要额外 rollout，这一点可与需要批评器或额外采样的方法区分开。实验主线上，论文在 ALFWorld、WebShop、Search-QA 三个交互环境、Qwen 3B 和 7B 两种规模下评估了 AgentOPSD，对比 GRPO 与强自蒸馏基线，结果显示其全面超越，特别在 Qwen2.5-7B 上 ALFWorld 达到 89.1% 成功率。消融实验说明回合级聚合与递归信念更新是收益来源。论文还报告了训练动态、策略熵和 horizon 鲁棒性分析，显示 AgentOPSD 随任务长度增大性能下降更平缓，且收益主要来自信用构建而非特权访问。总体而言，该工作为智能体 RL 的密集信用分配提供了一种并行的、无批评器的、可直接落地的方案，并验证了其在多个任务与模型规模上的有效性。**

<!-- paperflow:b4439ba54ab2b43e -->
## ML-for-ML

[[Deep Reading - Aug 2026/ML-for-ML|Deep Reading]]

[https://arxiv.org/pdf/2608.06046v1](https://arxiv.org/pdf/2608.06046v1)

- **论文正文围绕一个核心观点展开：网络侧与 ML 侧的控制决策不应被割裂优化，而应在统一的端到端目标下联合选择。
一、论证主线
作者从 AI 训练工作负载的成本问题切入，指出在共享云集群中，训练作业的通信行为与网络控制机制存在强耦合。当前实践中，网络机制（如拥塞控制、流量调度）单独决定字节如何传输，而 ML 系统（如批大小、梯度压缩、同步策略）单独决定通信的时机和规模。这种分离导致两者无法协同，浪费了潜在的性能提升。作者因此提出 ML-for-ML，强调跨层联合优化，并以 time-to-target-loss 作为共享优化目标，而非分别优化网络吞吐或单步训练时间。
二、技术主线
论文定义了控制器动作空间，由网络侧和 ML 侧旋钮组合构成（Table 1）。评估每个候选组合时，同时关注其如何改变前景任务可见的通信时间以及训练进度，从而将两端的影响统一到 loss 变化上。初步原型采用了某种优化算法（具体算法未在检索证据中给出）来搜索联合配置。
三、实验主线
实验在一个受控的共享集群争用场景中进行，对比独立优化与联合优化。核心发现是：独立处理旋钮无法可靠找到最优组合，最优组合随争用程度变化；联合优化可显著加速达到目标损失，最多提升 42%。
四、局限与展望
作者明确当前工作只是第一步，实验仅覆盖单一前景作业、有限控制空间和 loss 作为反馈信号的情况。讨论中提出了扩展方向，包括更丰富的动作空间、动态 SLO、多作业协调、对抗性流等。整体来看，这是一篇视角性/初步探索性论文，但提出的统一目标与评估思路具有启发价值。**
