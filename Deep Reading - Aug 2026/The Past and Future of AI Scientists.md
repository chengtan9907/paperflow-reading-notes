---
user_id: "cheng tan"
paper_id: 7982
arxiv_id: "2608.14407"
title: "The Past and Future of AI Scientists"
publish_date: "2026-08-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.14407.pdf"
pdf_url: "https://arxiv.org/pdf/2608.14407"
abs_url: "https://arxiv.org/abs/2608.14407"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-18T02:15:18"
---
# The Past and Future of AI Scientists

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：ai scientist · self-driving laboratory · scientific discovery · scientific agent

## 一句话总结

本文对“AI 科学家”（能够自动化科学发现的机器）的过去与未来进行综述性考察，梳理了从 Adam、Eve 到基础模型时代的技术演进，提出集成式 AI 科学家的核心挑战、架构需求与社会治理问题。

## 摘要

> We present a survey of the past and future of AI Scientists: machines capable of automating science. AI Scientists can originate hypotheses, deduce their consequences, design and execute experiments, interpret their results, and revise their beliefs. Such systems are not enlarged prediction programs or conversational assistants. They are integrated scientific agents, connected to the literature, formal knowledge, mathematical models, simulations, data-analysis systems and physical laboratories.
> Adam was the first machine to make novel scientific discoveries through cycles of hypothesis formation and physical experimentation. Eve established the architecture of the modern self-driving laboratory. Foundation models, autonomous agents and laboratory robotics now make it possible to build systems far more general than either Adam or Eve.
> The central problem is no longer whether individual components of science can be automated. They can. The problem is integration. AI Scientists must combine neural learning with logic, probability, mathematics, causal reasoning, simulation, experimental design, robotics and formal scientific records.
> AI Scientists have the potential to transform science: to make science faster, cheaper, more systematic and more reproducible. AI Scientists could investigate systems too complicated for unaided human science, and enable thousands of AI scientists to work together on single problems. They could also concentrate scientific power, automate error and connect scientific reasoning directly to dangerous physical actions. Their development must therefore include rigorous evaluation, complete provenance, accountability, safety and governance.
> The Nobel Turing Challenge sets the goal of developing by 2050 AI systems capable of automating Nobel-quality discoveries. Progress is ahead of schedule. When we succeed it will create a new form of science and transform the world.

Q1: 这篇论文试图解决什么问题？

论文围绕的核心问题是：如何构建真正意义上的“AI 科学家”，而不只是科学任务中的自动化工具。具体而言，作者从以下层面进行问题分析：

1. **定义性困难**：什么才算 AI 科学家？论文明确反对将科学预测模型、能写论文的大语言模型或实验室自动化系统直接等同于 AI 科学家。这些工具可以在某个环节产生科学价值，但不具备形成、修正和验证科学信念的闭环能力。AI 科学家必须是一个“整合的科学智能体”，能主动提出假说、推导结果、设计并执行实验、解释数据和修正信念。这个定义本身就构成了研究的问题边界。

2. **集成问题（核心瓶颈）**：论文认为，科学流程中的每个组件（如感知、推理、实验设计、物理操作）都已分别实现自动化，但把它们连接成整体并让系统能自主循环运行，是当前最困难的问题。这种集成不仅是工程接口层面的拼接，还涉及不同知识表示（神经网络的隐式知识、逻辑规则、概率模型、因果图、模拟器）之间的协调与转换。论文指出“AI 科学家必须结合神经网络学习与逻辑、概率、数学、因果推理、模拟、实验设计、机器人和形式化科学记录”，这实际上把集成提升为最重要的方法论命题。

3. **能力定位问题**：论文认为科学能力是一个连续谱，从日常的重复性科研工作，到极其罕见的能力——改变整个领域。AI 科学家将占据这个连续谱的各个位置，但如何评价一个 AI 系统究竟站在哪里，一直没有解决。作者批判了两种虚假评价标准：生成流畅的科学文本不等于发现；重新发现训练数据中已存在的答案也不算真正的发现。AI 科学家必须依据其“前瞻性发现、发现可靠性和节省人类科研投入的程度”来判断。

4. **风险与治理问题**：自动化科学可能让科学变得更快、更便宜、更系统、更可重复，但也可能集中科学权力、自动化传播错误，甚至将科学推理直接连接到危险的物理行动。因此，AI 科学家的开发必须配套严格的评估体系、完整的溯源记录、责任归属、安全机制和治理框架。这一问题在论文中被提升到与集成问题同等重要的高度。

5. **时间表问题**：论文将诺贝尔图灵挑战（Nobel Turing Challenge）作为可检验的目标——到 2050 年构建能够自动化诺贝尔级发现的人工智能系统，并认为目前进展“提前于时间表”。这一挑战同时提出了科学可行性、策略路径和社会接受度的多重问题。

Q2: 有哪些相关研究？

由于本文是全景式综述，相关工作按照历史和技术路线呈现：

1. **早期符号式科学发现系统**：在机器学习普及之前，AI 与科学结合主要依靠符号逻辑、知识图谱和手工规则，这些系统能进行一定程度的假说演绎，但难以感知复杂数据或与物理实验互操作（合理推断，论文中未显式列举；摘要中强调逻辑是 AI 科学家所需的关键组件之一）。

2. **机器人科学家与自驱动实验室**：论文重点回顾两个历史代表：Adam，被描述为第一个以假说形成和物理实验循环做出新颖科学发现的机器；Eve，被描述为“现代自驱动实验室”的架构奠基者。Eve 之后的“自动驾驶实验室”（self-driving laboratory）通常是在闭环中自动选择实验并优化目标函数，不一定生成或评估明确科学假说。论文专门强调“AI 科学家比自驱动实验室更通用”——如果实验室只是按照目标函数优化，而不产生假说，那它只是自动化优化器，不是完整的 AI 科学家。

3. **基础模型与自主智能体**：论文指出，当前基础模型（foundation models）、自主 agent 和实验室机器人技术的进步，已经使构建比 Adam、Eve 更通用的系统成为可能。这些技术在自然语言理解、代码生成、跨域知识检索和工具调用方面提供了前所未有的灵活性，是 AI 科学家由专门系统走向通用系统的核心推力。

4. **科学知识的形式化与综合**：相关工作还包括将科学领域知识编码为形式化逻辑、概率模型、因果模型和数学方程等方向。这些知识表示是 AI 科学家进行推演和解释的理论底座。论文要求“神经学习 + 逻辑 + 概率 + 数学 + 因果推理 + 模拟 + 实验设计 + 机器人 + 正式科学记录”的融合，正是对这些既有研究路线的一种兼容和升华。

5. **评估基准与挑战赛**：与 AI 科学家评估直接相关的相关工作包括：针对科学发现的 benchmark、自动科研竞赛、以及像“人工智能下一次图灵测试”一类的长期挑战。论文对“流畅散文 = 发现”和“重新发现 = 发现”这两种范式提出了批判，这说明评估方法论本身在领域中处于尚不成熟状态。

6. **AI for Science 大规模协作与治理**：相关研究还包括多智能体协作、分布式科学平台、区块链式可溯源科研记录、AI 安全研究。论文提醒 AI 科学家可连接危险物理行动，并呼吁发展与安全并行的治理设计。这呼应了 AI 安全与负责任人工智能的文献脉络。

Q3: 论文如何解决这个问题？

该论文并非提出一种具体算法，而是一套概念框架、历史判断和路线图，核心是如何设计未来 AI 科学家：

1. **概念界定**：将 AI 科学家定义为“集成的科学智能体”，其完整工作循环为：提出假说 → 推导可检验后果 → 设计实验 → 执行实验（可通过机器人）→ 解释结果 → 修订信念 → 再循环。论文将 AI 科学家与单纯预测系统、对话式论文生成器、普通自驱动实验室区分开，认为真正的 AI 科学家必须具备对假说的显式生成与评估能力。

2. **集成架构提案**：从语义检索命中的“架构和路线图”片段可以看到，作者给出了集成式 AI 科学家的组件清单（推测是推荐架构，而非强制标准）：
 - 关于背景知识的解读/心理模块；（原文片段“tation modules”可能是“interpretation modules”的截断）
 - 对抗式批评者和独立验证 agent（adversarial critics and independent verification agents）；
 - 安全与权限管理器（a safety and permissions governor）；
 - 完整溯源系统（a provenance system recording every material decision and action）；
 - 供人类科学家设定目标/干预的接口（interfaces through which human scientists set objectives and monitor）。
 这种架构将“集成”从抽象概念落实为可操作模块设计，强调保障可追溯性和人类监督。

3. **历史演进策略**：论文通过 Adam 和 Eve 的案例，说明从“单一发现机器”到“现代自驱动实验室架构”，再到未来通用系统的递进路径。作者主张，未来系统应继承 Adam 的假说形成—物理实验闭合循环，也要吸收 Eve 的高通量自驱动优化能力，并利用基础模型增强泛化和知识吸收。

4. **评估方法**：论文提出必须以前瞻性发现（prospective discovery）为准则：系统是否做出了以前不知道、且不在训练数据中的发现；发现的可靠性如何；相对于人类科学家的时间/成本投入，AI 科学家的效率增益如何。这类标准同时排除“文笔流畅”的伪发现和“复现已知答案”的假阳性。

5. **嵌入安全与治理**：解决方案中专门列出安全、溯源和人类接口，暗示 AI 科学家开发应从一开始就将治理机制作为技术组成部分内嵌，而不是事后附加。

6. **里程碑目标**：将诺贝尔图灵挑战作为到 2050 年需要达成的宏观目标，并指出“当前进度提前于时间表”——既是对可行性的判断，也是为后续研究设定衡量标准。

Q4: 论文做了哪些实验？

本文为一篇综述/立场论文，并未报告新的算法实验、数据集或基线结果。不过，论文在论证中回顾并分析了两个历史“实验系统”：

1. **Adam 系统**：根据摘要，Adam 是第一个以“假说形成—物理实验”的循环进行新颖科学发现的机器。它能在特定生物学领域（检索片段的“auxotrophic-growth experiments”表明它可在营养缺陷型生长实验中做选择）自动设计实验并利用实验数据修正假说。这是“AI 科学家”概念的实证起点。

2. **Eve 系统**：论文称 Eve 奠定了“现代自驱动实验室”的架构。Eve 的重点不是单一假说验证，而是将实验流程自动化、系统化和高通量化；它能选择化合物和化验条件（evidence from limitation/retrieval snippet），在药物筛选等场景中自主决策下一步实验。严格说 Eve 很可能包含优化目标函数的过程。

3. **非正式的“实验证据”**：论文还通过一系列反例和思维实验来论证评估标准：例如“产生流畅科学散文的 LLM”和“在训练集中重新发现答案”的系统，都不是 AI 科学发现的有效证明。这种思想实验虽然没有物理实验数据，但对定义和评估框架起到关键作用。

4. **综述的“观察性实验”**：论文将过去几十年的 AI+科学研究作为一个自然实验序列来观察：从逻辑系统到机器人科学家，再到基础模型驱动的自治 agent，系统能力逐步提升而集成问题日益突出。该观察支撑了“集成是当前核心问题”的结论。

（注意：由于我们只有摘要和部分检索片段，文中讨论的具体实验细节、数据集、性能数字均不充分；此处为根据检索证据和领域共识的合理转述。）

Q5: 发现了什么实验现象？

从论文的论证和历史回顾中，可以提取以下观察结果/现象（这些是论文据以立论的核心“事实”）：

1. **单个组件已经可行**：科学的各个环节——数据处理、假说搜索、实验设计、实验室操作——已经分别实现自动化，问题不在能不能自动化，而在如何连贯整合。这是作者对当前领域状态的一个核心判断。

2. **Adam 证明了闭环发现的可行性**：自动假说形成 + 物理实验循环可以产生真正新的科学知识。虽然 Adam 的动作词汇有限（检索片段显示其只能选择营养缺陷型生长实验一类的小词汇表），但其闭环机制仍然有意义。

3. **Eve 带来了架构范式的转变**：Adam 之后，Eve 以更完整的实验室自动化架构提升了实验通量，使“自驱动实验室”成为现代主流技术路线。该范式的代价是，自驱动实验室通常是在优化一个目标函数，而未必生成显式假说、也未必能对假说进行科学解释；因此它只是 AI 科学家的一个组件，而非 AI 科学家本身。

4. **能力连续谱的现象**：论文指出科学能力存在于一个连续谱上——从日常常规科研到罕见的重塑领域能力——而 AI 科学家未来也会落在该光谱的某个位置。这暗示 AI 科学家不会一夜之间全面取代人类科学，而是从低端科研工作开始逐步拓展。

5. **“流畅散文”和“重新发现”都不是发现**：这是论文对当前评估失效现象的两个观察：a) LLM 可以生成语言流畅的论文，但这不经检验便不能成为科学发现；b) 从训练数据中重新输出已有答案的价值很低，不能作为真正自主智能的证据。这两个观察帮助排除大量“伪进步”。

6. **当前系统的操作词汇仍然有限**：检索片段指出，AI 科学家能执行的实验种类通常来自“小而刻板、由人类定义”的动作空间。这解释了为什么通用性仍是尚未突破的瓶颈。

7. **风险与收益并存**：预期收益（加速、降本、系统化、提高复现性、可探索人类难以处理的复杂系统、多个 AI 科学家协作）与潜在危害（权力集中、错误自动化、危险物理操作的直接关联）同时上升，使得治理机制成为不可见但必需的维度。

Q6: 有什么可以进一步探索的点？

基于论文的论断和展望，可以进一步探索的方向包括：

1. **解决“集成问题”**：将神经学习、逻辑、概率、因果推理、数学建模与模拟、实验设计、机器人学和正式科学记录统一在同一个系统中，是论文提出的首要未来方向。可以探索新的模型架构、知识表示对齐方法、跨组件接口协议，以及全自动实验闭环的工程难题。

2. **通用型 AI 科学家的架构实验**：论文给出的架构模块（如解释模块、对抗性批评者与独立验证 agent、安全权限管理器、完整溯源记录、人类接口）可以作为具体系统设计的蓝图，研究者可以尝试实现这些模块并测试各模块之间的信息流效率。

3. **建立可信的评估基准**：基于“前瞻性发现、可靠性、节省人类投入”的评估原则，未来可构建新的 benchmark——排除训练数据泄漏、排除预测常见模式、允许开放式实验设计。这个评估体系本身是重大科研挑战。

4. **探索多 AI 科学家协作**：论文提到未来可“让数千个 AI 科学家在单一问题上共同工作”。由此衍生的问题包括：多个 agent 之间的任务分配、知识共享、结果仲裁、科学假说的冲突解决，以及多智能体场景下的完整溯源。

5. **研究超人类复杂系统**：人类科学家直觉与形式化方法难以处理的系统（如复杂的基因调控、多体物理、社会系统）可能成为 AI 科学家独特的应用领域，需要研究 AI 科学家的决策可解释性和可靠性。

6. **安全与治理机制**：设计“权限管理器”和安全护栏，确保 AI 科学家不能自主执行危险实验；建立分布式溯源系统，用区块链或审计日志记录“每个物质决策和动作”。

7. **科学发现的哲学和认知科学问题**：论文使用“科学能力连续谱”挑战了“机器做科学”的简单二元论，未来可进一步探讨机器信念修订的认知模型，以及 AI 发现如何被容纳进人类科学知识体系。

8. **诺贝尔图灵挑战的路线图更新**：论文认为进展“提前于时间表”，未来需要更有力的里程碑：例如规定中期目标（在若干领域达到人类博士水平的独立科研能力）并公开评估。

Q7: 总结一下论文的主要内容

《AI 科学家的过去与未来》是一篇由 Ross D. King 撰写的综述与立场论文，系统定义并讨论了能够自动化科学发现的机器——AI 科学家。论文的主要主张是，科学自动化的真正瓶颈不再是个体组件，而在于组件的深度集成。作者将人工智能科学家界定为一条完整闭环：提出假说、推导后果、设计实验、执行实验（包括机器人操作）、解释结果、修正信念，并再次循环。它们必须联结文献、形式化知识、数学模型、模拟、数据分析和物理实验室，因而“不只是膨胀的预测程序或对话助手”。

论文通过两个历史案例说明这一概念的可行性。Adam 作为第一台以假说形成与真实物理实验闭环做出新颖科学发现的机器，证明了机器驱动的发现过程是可行的。Eve 则构建了现代自驱动实验室的架构，极大地提高了实验自动化和迭代效率。不过，论文特别指出，AI 科学家比自驱动实验室更一般化；只优化目标函数的实验室未必生成或评估显式科学假说，因而不应被视为完整的 AI 科学家。

在“当前焦点”部分，作者论证：既然各组件均可自动化，核心挑战就在集成。未来的 AI 科学家必须把神经学习与逻辑、概率、数学、因果推理、模拟、实验设计、机器人技术和正式科学记录协同融合。就架构而言，检索片段显示论文提出了一个带有解释模块、对抗性批评者与独立验证 agent、安全权限管理员、完整 provenance 系统以及人类可交互接口的体系，对“集成”提供了工程层面的回应。

在“收益和风险”部分，论文提出 AI 科学家可以加速科研、降低成本、提高系统性和可复现性，并具备探索人类难以单独处理的复杂系统、开展大规模多智能体协作的潜力；但也会带来科学权力集中、错误传播被自动化、科学推理直接连接危险物理行动的风险，因此必须建设严格评估、完整溯源、问责、安全与治理框架。在“评估与基准”部分，论文明确批评了两种伪发现：流畅的科学写作和重新发现训练集中已有答案；而应以“前瞻性发现”“发现可靠性”“节省人类科学工作”作为评判标准。

论文最后引入“诺贝尔图灵挑战”——目标是到 2050 年实现自动化、诺贝尔奖级别的科学发现；并认为当前发展节奏已超前于预期。整篇论文在性质上更接近纲领性综述，没有给出新的实验结果，而是通过定义、历史案例、架构草案、评估原则和风险分析，为 AI for Science 的未来研究提供了高层次的路线图。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与当前关注方向“agent”和“ai-for-science”直接相关：它把 AI 科学家定义为一种自主式科学研究 agent，强调闭环决策和工具/实验室交互。

## 基本信息

- 作者：Ross D. King
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-17
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.14407`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了启发式草稿和 PDF 语义检索命中的摘要及若干章节片段（如 Integrated AI Scientists、Evaluation、Nobel Turing Challenge 等），所有非直接引述的细节均标注为合理推断；由于论文本体未提供完整正文，部分架构和局限描述依赖检索片段重构。
