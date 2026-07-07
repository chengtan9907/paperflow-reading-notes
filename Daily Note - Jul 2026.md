# On-Policy Distillation & Post-Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：On-Policy Distillation & Post-Training
- 方法：待从后续精读中沉淀
- 论文/报告：0 篇
- 暂无精读论文
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->



# Multimodal Models & Visual Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Multimodal Models & Visual Reasoning
- 方法：language, vision-language-model, vision, vision-language
- 论文/报告：2 篇
- AdaBoosting Text Prompts for Vision-Language Models
- Show Me Examples: Inferring Visual Concepts from Image Sets
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:a3c8bea8301ea2f3 -->
## AdaBoosting Text Prompts for Vision-Language Models

[[Deep Reading - Jul 2026/AdaBoosting Text Prompts for Vision-Language Models|Deep Reading]]

[https://arxiv.org/pdf/2607.00684](https://arxiv.org/pdf/2607.00684)

- **本文提出Text Prompt Boosting (TPB)，解决VLM少样本自适应中提示改进有限的问题。TPB将每个类别的文本提示池（包含模板和LLM生成描述）视为弱分类器，通过AdaBoost算法迭代选择并加权，重点修正前一轮错误样本，最终集成强分类器。实验在多个数据集和五个ViT-L级VLM上验证，显示TPB具有优异的shot可扩展性（随shots增加持续提升）和跨模型迁移鲁棒性。主要贡献包括：首次将boosting用于VLM提示集成，提出系统性的弱分类器构建与选择框架，并证明自然语言提示集成比连续提示更易迁移。局限包括依赖预定义提示池和boosting的额外计算开销。**

<!-- paperflow:0be63a66c83a9af6 -->
## Show Me Examples: Inferring Visual Concepts from Image Sets

[[Deep Reading - Jul 2026/Show Me Examples-Inferring Visual Concepts from Image Sets|Deep Reading]]

[https://arxiv.org/pdf/2607.02402](https://arxiv.org/pdf/2607.02402)

- **本文针对现有视觉语言模型在纯视觉上下文推理中的缺陷，提出了**视觉概念集推理（VICIS）**任务。VICIS要求模型从一个小图像集合中推断共享概念，并将其应用于一张查询图像生成新结果，从而评估模型的抽象视觉推理能力。

作者首先指出现有VLM（如Flamingo、GPT-4V）在此类任务上的失败：它们要么忽略上下文，要么复制图像，无法提取概念。为弥补该差距，他们设计了一个系统的任务框架：使用WordNet层次结构自动构建上下文-查询-目标三元组，确保概念一致。接着提出一个可训练的模型架构，该架构从上下文集中提取概念嵌入，并融合查询特征，通过条件生成（可能基于扩散模型）输出符合概念且与查询对齐的图像。

实验在合成数据集和ImageNet/WordNet真实数据集上进行，基准包括任务特定模型和通用VLM。结果显示，所提模型在生成准确性和多样性上显著优于基线，并能泛化到未见概念和草图模态。此外，该方法还展示了概念推理作为非语言规范表达方式的潜力，即通过示例而非文本传达意图。

研究贡献包括：（1）提出VICIS任务，填补视觉概念推理评估空白；（2）开发基于WordNet的数据构建方案；（3）设计有效的训练框架和架构；（4）在合成与真实数据上验证了方法的有效性及泛化能力。**

# Reward Models & Reinforcement Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Models & Reinforcement Learning
- 方法：math-pr, math-st
- 论文/报告：2 篇
- Is One Layer Enough? Training A Single Transformer Layer Can Match Full-Parameter RL Training
- GRPO, Dr. GRPO, and DAPO Are Three Operations on One Number: The Group-Standard-Deviation Identity
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:2a80d736a7d3e18e -->
## Is One Layer Enough? Training A Single Transformer Layer Can Match Full-Parameter RL Training

[[Deep Reading - Jul 2026/Is One Layer Enough-Training A Single Transformer Layer Can Match Full-Parameter RL Training|Deep Reading]]

[https://arxiv.org/pdf/2607.01232](https://arxiv.org/pdf/2607.01232)

- ****论证主线**：论文从常规的均匀参数更新做法出发，提出疑问：RL后训练的增益是否均匀分布在所有层？通过定义'层贡献'量化指标，设计逐层隔离训练实验，发现一个显著但未被认知的现象：训练单层即可恢复全参数RL大部分增益，且高贡献层集中于网络中部。该现象在多种模型、算法、任务上稳定重复，暗示RL适应具有内在结构倾向。基于此，论文提出层感知训练策略，仅训练关键层即可超越全参数训练，兼具效率与性能。

**技术主线**：1. 引入层贡献指标：$C_l = (P_{single l} - P_{pretrain})/(P_{full} - P_{pretrain})$。2. 实验协议：冻结除目标层外所有参数，使用相同RL配置训练。3. 统计分析：计算Spearman秩相关系数评估贡献排名一致性。4. 应用：设计显式层选择策略（启发式中间层法、自适应TOP-K法）。

**实验主线**：
- 设置：7个模型（Qwen3, Qwen2.5）、3种RL算法、4个任务基准（数学、代码、智能体）。
- 关键结果：
 - 单层贡献曲线呈现山峰形状，峰值在中间。
 - 训练顶部10个贡献层达到69.1%准确率 > 全参数66.4%。
 - 跨数据集排名相关系数0.76，跨任务0.59。
- 负结果：训练低贡献层无效甚至有害。
- 延伸：简单启发式（只训练中间1/3层）即可匹配全参数训练。

**结论**：RL后训练的结构属性是增益高度集中在中间层，这挑战了均匀更新的隐式假设，并为高效RL训练开辟新途径。**

<!-- paperflow:967bc031a8c534ee -->
## GRPO, Dr. GRPO, and DAPO Are Three Operations on One Number: The Group-Standard-Deviation Identity

[[Deep Reading - Jul 2026/GRPO, Dr. GRPO, and DAPO Are Three Operations on One Number-The Group-Standard-Deviation Identit|Deep Reading]]

[https://arxiv.org/pdf/2607.00152](https://arxiv.org/pdf/2607.00152)

- **本文从数学上统一了GRPO、Dr.GRPO和DAPO三种主流RLVR方法。通过提出group-standard-deviation恒等式，证明训练更新大小正比于同一prompt下回答的二值奖励标准差σ。GRPO除以σ实现方差稳定化，Dr.GRPO移除除法从而等权重处理所有问题，DAPO丢弃σ=0的无信息组。这一恒等式揭示了GRPO中的难度偏差根源——高分歧问题获得更大更新，而全体一致问题（无论正确与否）对训练无贡献。实验在Big-Math数据集上验证了σ的分布特性，并在训练中展示了不同方法在更新分配上的差异。论文的贡献在于：1）给出了三种方法的统一形式，消除表面差异；2）说明了标准偏差并不是无害的归一化步骤，而是决定学习发生位置和强度的核心参数；3）提供了实践指导：选择哪种方法取决于是否需要难度自适应。局限性包括：分析限于二值奖励、单prompt情形，真实多prompt训练中的互作用未被充分建模。**

# Data, Benchmarking & Evaluation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data, Benchmarking & Evaluation
- 方法：agent, optimization
- 论文/报告：3 篇
- Are Performance-Optimization Benchmarks Reliably Measuring Coding Agents?
- TestEvo-Bench: An Executable and Live Benchmark for Test and Code Co-Evolution
- AgenticDataBench: A Comprehensive Benchmark for Data Agents
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:d80c004187c6b5ef -->
## Are Performance-Optimization Benchmarks Reliably Measuring Coding Agents?

[[Deep Reading - Jul 2026/Are Performance-Optimization Benchmarks Reliably Measuring Coding Agents|Deep Reading]]

[https://arxiv.org/pdf/2607.01211](https://arxiv.org/pdf/2607.01211)

- **论文系统审计了三个仓库级性能优化基准（GSO、SWE-Perf、SWE-fficiency）对编码智能体评估的可靠性。通过重放740个官方参考补丁、分析评分规则和公开提交表现，揭示了排行榜分数背后的三个混杂因素：（1）跨机器可复现性差，仅少数任务（GSO 38%，SWE-Perf 8%，SWE-fficiency 83%）的参考补丁在所有机器上有效，SWE-Perf尤为脆弱；（2）评分规则高度影响排名，SWE-fficiency中最差十任务贡献了58.5%-82.8%的分数，导致排名对规则选择敏感；（3）大多数任务（85.3%）已被至少一个公开提交解决，使得排行榜分数难以反映智能体间的真实能力差距。论文提出了替代评分规则（如边界惩罚）和任务级可靠性标签，为基准用户和设计者提供了更稳定的评估信号。研究还发现剩余未解决任务并非简单的正确性失败或策略问题，值得进一步探索。**

<!-- paperflow:b25aa2fd3345dfa3 -->
## TestEvo-Bench: An Executable and Live Benchmark for Test and Code Co-Evolution

[[Deep Reading - Jul 2026/TestEvo-Bench-An Executable and Live Benchmark for Test and Code Co-Evolution|Deep Reading]]

[https://arxiv.org/pdf/2607.02469](https://arxiv.org/pdf/2607.02469)

- **TestEvo-Bench 是一个专注于测试与代码协同演化的可执行活基准，旨在解决现有基准过于静态、与代码变更脱节、且无法防止数据泄露的问题。论文首先指出现有测试生成和更新基准的不足：它们通常将测试视为独立于代码变更的任务，依赖静态指标（如覆盖率），不要求测试实际运行验证。此外，这些基准的测试用例可能已经存在于 LLM 预训练数据中，导致评估高估。为此，作者设计了 TestEvo-Bench，通过从真实 Git 提交历史中挖掘测试与代码同时变更的提交对，并经过执行验证（确保旧代码测试通过、新代码测试应通过或调整为失败），构建了 746 个测试生成任务和 509 个测试更新任务。每个任务包含完整的环境配置（如 Java 版本、Maven pom.xml、依赖），使得评估可以基于实际编译和测试执行结果。基准的“活”特性体现在：每项任务记录提交时间戳，且新任务会定期通过自动化流水线添加，评估者可以仅选择模型训练截止时间之后的任务，从而避免数据泄露。实验采用了四种先进的代码智能体：Claude Code（Claude Opus 4.7）、Gemini CLI（Gemini 3.1 Pro）、SWE-Agent 以及一个启发式基线。结果表明，这些智能体在无成本限制时能达到较高的成功率（测试生成 77.5%，测试更新 74.6%），但在较新的任务上成功率显著下降，且对成本非常敏感。论文还进行了消融分析，如不同基础模型组合、任务难度分布、以及测试质量指标（覆盖率、变异分数）与成功率的关联。结论部分重申了可执行活基准的重要性，并承诺将持续扩展基准。论文的附录提供了任务示例、流水线细节以及完整的实验结果表格。总体而言，TestEvo-Bench 为评估 LLM 在测试自动化领域的能力提供了一个更真实、更可靠的平台，并对未来研究方向给出了清晰的指引。**

<!-- paperflow:0ce0232718640676 -->
## AgenticDataBench: A Comprehensive Benchmark for Data Agents

[[Deep Reading - Jul 2026/AgenticDataBench-A Comprehensive Benchmark for Data Agents|Deep Reading]]

[https://arxiv.org/pdf/2607.01647](https://arxiv.org/pdf/2607.01647)

- **AgenticDataBench是一个为评估LLM数据智能体而设计的综合性基准。其核心动机是填补现有基准在场景多样性、细粒度度和真实性上的空白。论文首先系统梳理了数据科学中频繁出现的数据操作模式，将其定义为“数据科学技能”，并通过从Stack Overflow中挖掘大规模任务解决方案，结合LLM语义精炼与层次化凝聚聚类，提取出一套层次化的代表性技能集。基于此技能框架，基准构建过程分为三部分：一是从真实应用场景（覆盖15个垂直行业，含5个金融科技B2B用例）收集任务-解决方案对，并使用技能组成多样性度量筛选出最具区分度的样本；二是对于缺乏真实数据的领域，设计了一种基于LLM的自动化任务生成流程，确保技能全覆盖；三是为每个任务提供细粒度的技能标签和正确答案，支持技能级性能评估。评估部分选取了当前主流的几种数据智能体系统（如ReAct代理、Code Interpreter、AutoGPT等），在AgenticDataBench上进行全面测试。结果显示，不同智能体在不同技能类别上表现出显著差异，复杂推理与领域特定知识成为主要瓶颈。基准本身也在简洁性（去冗余）、多样性和真实性之间取得了平衡。论文还公开了测试平台及标注数据，为后续研究提供了标准化评估工具。总体而言，AgenticDataBench对数据智能体领域的基准建设具有重要推动价值，但受限于LLM生成任务的保真度和技能定义的泛化能力，尚需在更多真实工作流中验证。**

# World Models, Generation & Audio

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：World Models, Generation & Audio
- 方法：待从后续精读中沉淀
- 论文/报告：0 篇
- 暂无精读论文
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->



# Research Agents & Scientific Discovery

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Research Agents & Scientific Discovery
- 方法：待从后续精读中沉淀
- 论文/报告：1 篇
- Autonomous Scientific Discovery via Iterative Meta-Reflection
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:49814aad3f9b2f72 -->
## Autonomous Scientific Discovery via Iterative Meta-Reflection

[[Deep Reading - Jul 2026/Autonomous Scientific Discovery via Iterative Meta-Reflection|Deep Reading]]

[https://arxiv.org/pdf/2607.01131](https://arxiv.org/pdf/2607.01131)

- **论文提出DiscoPER，一种基于LLM的自主科学发现框架，旨在突破现有系统在开放式探究上的限制。该框架通过PROPOSE-EVALUATE-REFLECT循环实现无预设目标的假设生成与验证，其中二阶元反思机制将已有发现作为数据进行分析，生成指导以主动探索未知区域。系统还通过工具使用处理多模态数据（如图像），进一步扩展假设空间。在生态学领域的iNatDisco基准上，DiscoPER恢复8/9已知模式，支持率72.7%，显著优于经典因果发现（PC, LiNGAM）和无反思LLM基线。消融实验证实了元反思、图像工具和LLM规模的关键作用。论文还形式化了开放式科学发现的通用框架，指出先前系统是其受限实例。**

# Agent Skills, Harness & Tooling

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Skills, Harness & Tooling
- 方法：agent, stat-ml, stat-me
- 论文/报告：2 篇
- Can Agents Generalize to the Open World? Unveiling the Fragility of Static Training in Tool Use
- SkillCoach: Self-Evolving Rubrics for Evaluating and Enhancing Agentic Skill-Use
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:aedc55f770211154 -->
## Can Agents Generalize to the Open World? Unveiling the Fragility of Static Training in Tool Use

[[Deep Reading - Jul 2026/Can Agents Generalize to the Open World-Unveiling the Fragility of Static Training in Tool Use|Deep Reading]]

[https://arxiv.org/pdf/2607.01084](https://arxiv.org/pdf/2607.01084)

- **本文针对 LLM 智能体在真实世界部署中面临的泛化性难题，开展了系统性的研究。作者首先指出，现有的静态基准测试掩盖了智能体在面对动态环境时的脆弱性。为此，论文提出了 OpenAgent 框架，将开放世界中的环境变化形式化为查询、动作、观测和领域四个维度的分布偏移。通过构建一个包含感知、交互、推理和内化四个层级的受控沙盒环境，作者对 SFT 和 RL 训练的智能体进行了深度诊断，揭示了 SFT 容易陷入轨迹过拟合而 RL 在边界条件下鲁棒性不足的问题。为了解决这些问题，论文提出了扰动增强微调（PAFT）策略，通过在训练中引入受控干扰，迫使智能体学习更本质的语义逻辑而非表面的符号关联。实验结果表明，PAFT 显著提升了智能体在开放世界场景下的适应能力和鲁棒性。这项工作为构建更可靠、可部署的智能体系统提供了重要的理论基础和实践指导。**

<!-- paperflow:c067107fd4e7eb6c -->
## SkillCoach: Self-Evolving Rubrics for Evaluating and Enhancing Agentic Skill-Use

[[Deep Reading - Jul 2026/SkillCoach-Self-Evolving Rubrics for Evaluating and Enhancing Agentic Skill-Use|Deep Reading]]

[https://arxiv.org/pdf/2607.01874](https://arxiv.org/pdf/2607.01874)

- **论文提出了SKILLCOACH，一个自演化评分框架，旨在评估和改进LLM智能体的技能使用能力。核心洞察是：最终任务成功不足以衡量技能使用的质量，因为智能体可能通过试错或错误决策偶然成功。SKILLCOACH从真实轨迹中自动衍生出四个过程评分维度（技能选择、遵循、组合、反思），将过程质量与外部验证器的结果信号分离。该评分既可作为评估工具，也可用于筛选高质量训练轨迹。实验表明，演化的评分能有效暴露最终准确率隐藏的失败，并且作为训练监督信号优于单纯的结果过滤。论文贡献在于：首次利用技能结构定义过程监督，同时服务于评估和训练；提出了自演化机制，使评分无需手工设计；通过实验验证了评分在不同模型规模下的有效性。局限包括：当前评估限于模拟环境，技能库为静态，且评分演化依赖特定LLM能力。整体上，该工作为智能体技能使用的细粒度评估和训练提供了新范式。**

# Memory, Personalization & Long-Horizon Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Memory, Personalization & Long-Horizon Agents
- 方法：agent
- 论文/报告：1 篇
- MemSyco-Bench: Benchmarking Sycophancy in Agent Memory
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:aa83a8f29b79b973 -->
## MemSyco-Bench: Benchmarking Sycophancy in Agent Memory

[[Deep Reading - Jul 2026/MemSyco-Bench-Benchmarking Sycophancy in Agent Memory|Deep Reading]]

[https://arxiv.org/pdf/2607.01071](https://arxiv.org/pdf/2607.01071)

- **论文提出了MemSyco-Bench，一个专门评估LLM智能体中记忆诱导谄媚问题的基准。其动机是：长期记忆虽然增强了个性化和连续性，但也会导致智能体过度遵从用户历史信息，造成事实扭曲。现有基准忽视了这一风险。MemSyco-Bench包含五个任务——拒绝记忆证据、尊重适用范围、冲突消解、更新追踪、个性化利用，每个任务通过对比有无记忆时的输出计算准确率和谄媚率。实验评估了七个现有记忆系统，发现它们普遍存在谄媚问题，尤其在需要忽略记忆的场景中表现糟糕。论文揭示了记忆系统的核心矛盾：如何在不牺牲个性化好处的同时控制记忆的负面影响。MemSyco-Bench为未来记忆系统设计提供了评估工具和基准。**

# AI for Science & Biology

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science & Biology
- 方法：agent, generation
- 论文/报告：2 篇
- DisciplineGen-1M: A Large-Scale Dataset for Multidisciplinary Visual Generation and Editing
- CausalGame: Benchmarking Causal Thinking of LLM Agents in Games
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:e464bf17ed8b5c23 -->
## DisciplineGen-1M: A Large-Scale Dataset for Multidisciplinary Visual Generation and Editing

[[Deep Reading - Jul 2026/DisciplineGen-1M-A Large-Scale Dataset for Multidisciplinary Visual Generation and Editing|Deep Reading]]

[https://arxiv.org/pdf/2607.02290](https://arxiv.org/pdf/2607.02290)

- **本文提出DisciplineGen-1M，一个大规模多学科视觉数据集，旨在解决图像生成和编辑模型在知识密集学科图表场景下的不可靠问题。数据集包含120万样本覆盖10个学科，通过四种创新管道构建：矢量图形渲染确保结构精确性，OCR编辑提供真实编辑对，程序化合成扩展多样性，大规模过滤保证质量。每个样本包含图像、描述字幕和编辑指令，并附带结构化元数据。此外，论文训练了一个学科感知推理生成模型，采用扩散架构并融入学科嵌入和知识编码。在GenExam和GRADE基准上的实验表明，该模型大幅超越开源基线，并展现出对一般推理任务（WISE/RISE）的迁移能力。消融分析验证了每种数据管道的贡献，其中矢量渲染最为关键。该工作为学术视觉内容生成提供了关键数据基石，有望推动教育、科研出版等领域的应用。数据集、模型及代码将开源以促进可重复研究。**

<!-- paperflow:0195c5c9d80b565a -->
## CausalGame: Benchmarking Causal Thinking of LLM Agents in Games

[[Deep Reading - Jul 2026/CausalGame-Benchmarking Causal Thinking of LLM Agents in Games|Deep Reading]]

[https://arxiv.org/pdf/2607.04293](https://arxiv.org/pdf/2607.04293)

- **论文提出CausalGame，一个评估LLM代理因果思考能力的基准测试。通过14个模拟选择偏差、测量误差和隐藏混杂因素的游戏场景，要求代理主动设计实验、观察数据并报告因果关系。对30个前沿LLM的评估显示，所有模型均表现出严重的因果推理缺陷：最佳存活率68.0%（分析最优78-85%），因果推理评分通过率仅5-7%。实验揭示了代理容易受选择偏差影响、无法识别隐藏混杂等关键问题。论文还提供了详细的失败模式分析，并讨论了未来方向。CausalGame为AI科学家系统的因果思考评估提供了可扩展的平台。**

# AI for Education

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Education
- 方法：ai-for-science, explanation
- 论文/报告：1 篇
- AIriskEval-edu: New Dataset for Risk Assessment in AI-mediated K-12 Educational Explanations
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:3e721192a35a0890 -->
## AIriskEval-edu: New Dataset for Risk Assessment in AI-mediated K-12 Educational Explanations

[[Deep Reading - Jul 2026/AIriskEval-edu-New Dataset for Risk Assessment in AI-mediated K-12 Educational Explanations|Deep Reading]]

[https://arxiv.org/pdf/2607.01934](https://arxiv.org/pdf/2607.01934)

- **本文提出了AIriskEval-edu-db2数据集，用于K-12教学解释的可解释风险评估。数据集包含170个ScienceQA问题，每个问题有12个解释（1个人类+11个LLM模拟），覆盖五个风险维度，其中785个解释附带结构化可解释性标注。通过半自动流程结合专家验证确保质量。实验将Llama 3.1 8B经LoRA微调后与GPT-4、Claude等模型对比，结果显示微调后的轻量模型在多数风险维度上接近前沿模型，在可解释性任务上表现有竞争力。该数据集支持本地部署的教育审计应用，平衡了性能与隐私。论文的主要贡献是提供了首个带细粒度可解释性标注的教学风险数据集，并验证了小模型微调的可行性。**

# Language Models

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Language Models
- 方法：agent, ai-for-science, language, reasoning, science-discovery, optimization, retrieval, math-pr
- 论文/报告：8 篇
- CheckRLM: Effective Knowledge-Thought Coherence Checking in Retrieval-Augmented Reasoning
- Rethinking Speech-LLM Integration for ASR: Effective Joint Speech-Text Training by Interleaving
- Measuring the Gap Between Human and LLM Research Ideas
- Purified OPSD: On-Policy Self-Distillation Without Losing How to Think
- Can Language Models Actually Retrieve In-Context? Drowning in Documents at Million Token Scale
- Bringing Agentic Search to Earth Observation Data Discovery
- Weak-to-Strong Generalization via Direct On-Policy Distillation
- TREK: Distill to Explore, Reinforce to Refine
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:457adeec9337faad -->
## CheckRLM: Effective Knowledge-Thought Coherence Checking in Retrieval-Augmented Reasoning

[[Deep Reading - Jul 2026/CheckRLM-Effective Knowledge-Thought Coherence Checking in Retrieval-Augmented Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2607.02262](https://arxiv.org/pdf/2607.02262)

- **本文针对推理语言模型在知识密集型任务中易出现事实错误且错误累积的问题，提出 CheckRLM 框架。该框架在推理过程中实时提取事实声明，检索外部知识进行一致性校验，并对不一致进行最小成本局部修正。通过这种过程内干预，CheckRLM 有效抑制了错误传播，同时保持低推理成本。实验在多个多跳问答数据集上验证了其有效性，消融实验表明各组件均发挥作用。主要贡献包括提出过程级知识校验范式、实现高效局部修正、开源代码和数据。局限性在于当前仅支持纯文本知识源，未来可向多模态扩展。**

<!-- paperflow:134ce92ca4f3344c -->
## Rethinking Speech-LLM Integration for ASR: Effective Joint Speech-Text Training by Interleaving

[[Deep Reading - Jul 2026/Rethinking Speech-LLM Integration for ASR-Effective Joint Speech-Text Training by Interleaving|Deep Reading]]

[https://arxiv.org/pdf/2607.01733](https://arxiv.org/pdf/2607.01733)

- **本文重新审视面向ASR的语音-LLM集成问题，指出现有decoder-only模型在常规训练下会过度专精于语音条件解码，无法充分利用LLM的文本先验知识。为此，作者提出Joint Speech-Text Interleaved Pre-Training (JSTIP)，一种面向ASR的预训练策略，通过构建交错语音-文本序列（词级或片段级对齐），并使用统一的下一个token预测目标对纯语音、交错和纯文本数据联合训练，防止模型遗忘文本建模能力。实验使用38k小时内部英语数据训练的400M参数Conformer+LLM模型，在多个ASR测试集上取得改进，尤其在小数据量条件下。此外，在实体识别领域适应任务中，JSTIP仅需领域转录文本即可达到与合成语音-文本对相当的结果，简化了领域迁移过程。消融研究比较了词级和片段级交错的效果，显示词级交错能更紧密耦合模态但对齐更困难。论文最后指出词类型引导的模态选择和扩展性研究是未来方向。**

<!-- paperflow:d3dbfa747917cea3 -->
## Measuring the Gap Between Human and LLM Research Ideas

[[Deep Reading - Jul 2026/Measuring the Gap Between Human and LLM Research Ideas|Deep Reading]]

[https://arxiv.org/pdf/2607.01233v1](https://arxiv.org/pdf/2607.01233v1)

- **本文系统性地测量了LLM生成的研究想法与人类研究者实际想法之间的分布差距。作者构建了一个基于文献上下文的评估框架：从已发表论文中逆向工程出可能激发想法的少量prior works，并要求LLM基于这些文献生成新想法，同时以原始论文作为人类想法参照。核心创新是提出双轴研究品味分类法，从机会模式（how a proposal frames the research gap）和范式（how the contribution is constructed）两个维度刻画想法。通过大规模标注实验，作者发现LLM想法在机会模式上高度集中于'桥梁式'（衔接现有工作），在范式上偏向'综合方法'，而人类想法分布更广，包括填补空白、理论深化等不同方式。这一差距在多个LLM间一致存在，表明当前LLM在科研创意上具有系统性品味偏差，其生成范围比人类更窄。论文的实验设计严谨，包含消融研究和跨模型比较，并对局限性有清晰讨论，如领域偏斜和上下文简化。主要贡献在于提出了一种可操作的研究品味量化方法，并揭示了AI创意生成中的品味盲区。**

<!-- paperflow:50e0973de38a0dfd -->
## Purified OPSD: On-Policy Self-Distillation Without Losing How to Think

[[Deep Reading - Jul 2026/Purified OPSD-On-Policy Self-Distillation Without Losing How to Think|Deep Reading]]

[https://arxiv.org/pdf/2607.02234](https://arxiv.org/pdf/2607.02234)

- **本文系统研究了在线自蒸馏 (OPSD) 在长思维链推理模型上的失败现象及其深层原因，并提出了一种有效的净化方案。首先，论文通过大量实验证实标准 OPSD 在四个不同规模的长 CoT 模型（Qwen3-8B、Qwen3-4B、DeepSeek-R1-Distill-Qwen-7B 和 OLMo-7B-Thinking）上均未带来明显提升，反而损害了模型的反思推理能力，表现为自我纠正能力下降和过度自信。为了揭开这一反直觉现象，作者创新性地对教师的监督信号进行了分解，发现来自参考解的成分在监督中占据主导，它迫使模型记忆参考解中的局部模式（捷径），而衡量真正推理能力的“问题条件化”成分被压制。基于这个诊断，论文设计了净化 OPSD：构建一个“仅参考”教师，得到纯参考解驱动的输出，从原教师输出中减去该部分得到残差；然后利用点互信息 (PMI) 将残差转化为一个形式良好的概率目标，学生直接优化这个新目标从而避开参考捷径。实验在四个主流长 CoT 模型上，以 Math-CoT-20K 为训练集，验证了该方法的有效性。不仅在数学推理准确率上相比基线和标准 OPSD 有稳定提升，更重要的是恢复了模型的自我纠正倾向和置信度校准，表明其内在推理过程得以保留。消融实验表明，两个组件——参考教师减法和 PMI 蒸馏——缺一不可。此外，论文还展示了方法在训练过程中的认知行为更稳定，并且在分布外测试上具有更强的泛化能力。这项工作不仅解决了 OPSD 在长 CoT 场景下失效的实际问题，而且通过信号分解提供了一种诊断训练失败的一般性框架，对理解知识蒸馏中的信息干扰具有理论启示。整体而言，该论文问题明确、分析深入、方法精巧、实验充分，是知识蒸馏与推理能力提升领域的一项重要贡献。**

<!-- paperflow:d89df94987286355 -->
## Can Language Models Actually Retrieve In-Context? Drowning in Documents at Million Token Scale

[[Deep Reading - Jul 2026/Can Language Models Actually Retrieve In-Context-Drowning in Documents at Million Token Scale|Deep Reading]]

[https://arxiv.org/pdf/2607.01538](https://arxiv.org/pdf/2607.01538)

- **本论文系统研究了语言模型在上下文检索（ICR）任务中扩展到百万token规模的能力与局限性。作者首先指出，ICR通常被限制于小规模或重排序场景，而实际检索系统需要处理大规模语料库并泛化到超长输入。为填补这一空白，论文提出了BlockSearch，一个0.6B参数的自回归LM检索器，使用块稀疏注意力降低计算量，并通过训练时随机化块大小和序列长度来促进长度泛化（训练128K，测试1M）。然而，在极端长度外推下（如语料库包含大量无关文档时），BlockSearch的检索性能急剧下降。通过机制分析，论文揭示了“注意力稀释”效应：当上下文包含许多文档时，所有文档的注意力分数总和恒为1，无关文档大量挤占了softmax分母，导致金标文档的归一化权重降低，即使其原始logit仍较高。基于此诊断，作者提出了两种长度感知的改进：一是对softmax进行缩放（根据序列长度调整logit温度），二是引入文档级稀疏注意力（仅允许少量候选文档参与交互）。最终模型BlockSearch+在MS MARCO和NQ上匹配了强密集检索基线（Contriever），在LIMIT上取得3倍于密集检索的性能，同时比并发模型MSA小7倍。论文贡献包括：首次系统评估ICR的大规模能力；揭示注意力稀释机制；提出有效缓解方案；并在多个基准上验证了ICR作为检索范式的潜力。实验还指出当前长上下文基准（如Needle in a Haystack）过于合成，ICR任务能提供更语义化的挑战。局限在于大N下仍有明显退化，且未探索更大模型。整体上，该工作为长上下文检索和注意力控制提供了重要的实证与理论基础。**

<!-- paperflow:583e1cbc92abec77 -->
## Bringing Agentic Search to Earth Observation Data Discovery

[[Deep Reading - Jul 2026/Bringing Agentic Search to Earth Observation Data Discovery|Deep Reading]]

[https://arxiv.org/pdf/2607.02387](https://arxiv.org/pdf/2607.02387)

- **本篇论文聚焦于地球科学数据发现领域，提出了一种基于大语言模型的智能体搜索系统，旨在解决NASA地球观测数据集和工具查找困难的问题。论文的主要贡献包括：
1. 任务框架化：将地球科学数据发现重新定义为可信任、可验证的智能体搜索任务，不同于传统的单纯检索或问答。
2. NASA-EO-Bench基准：从NASA EO-KG知识图谱和同行评审论文中构建了一个包含约2.1万引用验证查询-数据集对的开放基准，用于训练和评估数据发现模型。
3. 两阶段搜索系统：第一阶段采用监督式检索（BM25+神经评分器分数融合），大幅超越传统基线（R@10和MRR提升5倍以上）；第二阶段引入零样本LLM智能体重排，进一步提升MRR 28%。
4. 实际部署：系统已作为公共服务面向地球科学社区开放，并集成了NASA现有工具（Harmony、SDE、WorldView等）。
实验验证了知识图谱与LLM结合的潜力，以及监督检索与智能体推理的互补性。论文的工作属于AI for Earth Science的前沿实践，对数据密集科学研究的数据发现提供了有效范式。**

<!-- paperflow:ae9a10563beb6952 -->
## Weak-to-Strong Generalization via Direct On-Policy Distillation

[[Deep Reading - Jul 2026/Weak-to-Strong Generalization via Direct On-Policy Distillation|Deep Reading]]

[https://arxiv.org/pdf/2607.05394](https://arxiv.org/pdf/2607.05394)

- **本文提出了 Direct-OPD，一种旨在解决大模型 RL 训练成本瓶颈的弱到强泛化方法。该方法的核心洞察在于：RL 对模型性能的提升可以被抽象为一种“策略偏移”信号，这种信号比模型最终的概率分布更具迁移价值。通过计算弱老师在 RL 前后的对数概率比，并将其作为密集奖励应用于强学生的同策略采样中，Direct-OPD 成功实现了在极低算力消耗下显著提升强模型推理能力的目标。实验在 Qwen3 系列模型上验证了其有效性，特别是在 AIME 2024 等高难度数学竞赛任务中表现卓越。该研究不仅为高效后训练提供了新路径，也深化了对弱到强泛化机制的理解，证明了 RL 成果可以作为一种通用的隐式奖励信号在不同规模的模型间流转。**

<!-- paperflow:431bf8bd4d2aa136 -->
## TREK: Distill to Explore, Reinforce to Refine

[[Deep Reading - Jul 2026/TREK-Distill to Explore, Reinforce to Refine|Deep Reading]]

[https://arxiv.org/pdf/2607.05339](https://arxiv.org/pdf/2607.05339)

- **这篇论文提出了TREK（Teacher-Routed Exploration via Forward KL），一种新颖的分阶段训练方法，用于改进Group Relative Policy Optimization（GRPO）在困难推理prompt上的探索能力。TREK的核心洞察是：GRPO在困难prompt上停滞的根本原因是正确的解模式不在当前学生策略的采样支持内，因此需要一种机制扩展该支持。与传统蒸馏不同，TREK利用蒸馏不是为了复制教师输出，而是为了将教师产生的已验证解模式融入学生的策略分布，从而扩大学生的探索空间。方法上，TREK首先通过pass@k识别学生正确率低的困难prompt；然后，从提议源（可以是外部教师如DeepSeek-V4，或是同一模型但使用额外推理时间）获得多个候选解，经验证器筛选正确解；接着，按这些解在当前学生策略下的似然排序，选择top-r个解；随后，进行短期的前向KL训练，使学生学会生成这些解（即覆盖目标分布），从而扩展其采样支持；最后，恢复标准的on-policy GRPO精炼。整个过程周期性地重复。实验在数学推理和智能体任务上验证了TREK的有效性。在AIME 2024/2025上，使用DeepSeek-V4教师可以在Qwen3各规模（1.7B至32B）上取得一致提升，例如Qwen3-8B的AIME 2025从36.9提升至40.3（avg@16）。自我上下文变体也实现了明显改进，证明方法不依赖于特定外部教师。在智能体任务ALFWorld和ScienceWorld上，TREK分别将成功率从75.8%提升至82.8%和从12.5%提升至26.7%，并且在训练早期展现出快速收敛的优势。TREK的主要贡献包括：（1）识别了GRPO在困难prompt上的探索支持缺失问题；（2）提出了基于前向KL的探查扩展方法，将蒸馏重新定义为支持扩展而非模仿；（3）展示了该方法在多种模型规模和任务上的普适性。局限性在于仍依赖外部或额外的提议源，且前向KL阶段涉及额外超参数调谐。总体而言，TREK提供了一种简单、通用且有效的补充训练协议，可即插即用于现有的GRP...**

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, generation, language, reinforcement-learning, optimization, multimodal-learning, gui-agent, deep-learning
- 论文/报告：18 篇
- AgenticSTS: A Bounded-Memory Testbed for Long-Horizon LLM Agents
- PACE: A Proxy for Agentic Capability Evaluation
- SkillFuzz: Fuzzing Skill Composition for Implicit Intents Discovery in Open Skill Marketplaces
- EvoPolicyGym: Evaluating Autonomous Policy Evolution in Interactive Environments
- PairCoder++: Pair Programming as a Universal Paradigm for Verified Code-Driven Multimodal and Structured-Artifact Generation
- WorldDirector: Building Controllable World Simulators with Persistent Dynamic Memory
- Program-as-Weights: A Programming Paradigm for Fuzzy Functions
- Search Beyond What Can Be Taught: Evolving the Knowledge Boundary in Agentic Visual Generation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:18248449f520261f -->
## AgenticSTS: A Bounded-Memory Testbed for Long-Horizon LLM Agents

[[Deep Reading - Jul 2026/AgenticSTS-A Bounded-Memory Testbed for Long-Horizon LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2607.02255](https://arxiv.org/pdf/2607.02255)

- **论文题为《AgenticSTS: A Bounded-Memory Testbed for Long-Horizon LLM Agents》。主要贡献是提出并实现了一种有界记忆合约（bounded memory contract），用于长程LLM智能体的记忆管理，并在Slay the Spire 2游戏中构建了一个可复现的测试床。

**论证主线**：当前智能体记忆机制（如简单历史追加）难以隔离和消融，导致记忆组件影响不可控。作者将记忆重新定义为“每个未来决策允许看到什么的合约”，并设计了一种替代方案：每个决策的输入由类型化检索组装，不保留跨决策的原始转录，从而保证提示大小有界且各层可独立消融。

**技术主线**：
- 定义五类知识层（L1-L5），涵盖角色模板、状态模式、状态记忆、跨决策记忆、触发技能。
- 在Slay the Spire 2中实现该合约：游戏具有封闭规则、可枚举实体（576卡牌、293遗物等），适合LLM决策。
- 释放完整数据包：298条轨迹、记忆/技能快照、提示记录和分析脚本，支持重现和扩展。

**实验主线**：
- 固定A0消融：无存储基线胜率3/10，加技能层6/10，但统计不显著（p≈0.37）。
- 公共基线：前沿LLM零胜，人类16%，验证任务难度。
- 补充跨骨干和累积上下文基线作为操作参考。

**局限**：
- 样本量小，统计推断弱。
- 仅限turn-based游戏，未涵盖连续控制、视觉输入。
- 无训练，依赖LLM固有能力。
- 合约设计针对特定任务结构，泛化性待验证。

总体而言，本文提供了一个精心设计的实验框架和生成资源，但核心发现仍处于方向性验证阶段，需要更大规模实验确认。**

<!-- paperflow:07ea2cc476984494 -->
## PACE: A Proxy for Agentic Capability Evaluation

[[Deep Reading - Jul 2026/PACE-A Proxy for Agentic Capability Evaluation|Deep Reading]]

[https://arxiv.org/pdf/2607.02032](https://arxiv.org/pdf/2607.02032)

- **本文提出PACE框架，旨在解决LLM智能体评估成本过高的问题。核心思想是利用非智能体基准测试中的少量精选实例作为“代理”，预测模型在智能体基准上的表现。PACE包含两个关键组件：实例选择和回归建模。实例选择通过两种互补策略（目标相关性和全局信息性）从19个非智能体基准中选出紧凑子集。回归建模采用带噪声的自举回归，将模型在该子集上的得分映射到目标智能体基准。作者在14个模型、4个智能体基准和19个非智能体基准上进行了全面实验，结果显示PACE能以低于完整评估1%的成本，达到高预测精度（LOOCV MAE<4%，Spearman rho>0.80，配对准确率约85%）。消融实验验证了两种选择策略的重要性。实例分析揭示了不同智能体基准对原子技能的不同依赖。论文还讨论了局限性，包括代理集固定可能导致的游戏风险、对其他智能体框架的泛化性等。总体而言，PACE为低成本智能体评估提供了实用途径。**

<!-- paperflow:5558b1208979ac8b -->
## SkillFuzz: Fuzzing Skill Composition for Implicit Intents Discovery in Open Skill Marketplaces

[[Deep Reading - Jul 2026/SkillFuzz-Fuzzing Skill Composition for Implicit Intents Discovery in Open Skill Marketplaces|Deep Reading]]

[https://arxiv.org/pdf/2607.02345](https://arxiv.org/pdf/2607.02345)

- **本文针对开放技能市场中技能组合产生的隐式意图发现问题，提出 SkillFuzz 方法。首先形式化了问题：将技能组合视为测试单元，利用规划产物在免执行条件下检测偏离无技能基线的行为。然后设计了技能契约提取和差异预言，并采用契约引导的蒙特卡洛树搜索高效搜索冲突组合。实验在 SkillsBench 上使用多种规划智能体验证，结果表明隐式意图普遍存在，SkillFuzz 能发现大量高风险意图，且搜索效率优于基线方法。主要贡献包括：(1) 首次将隐式意图发现形式化为模糊测试问题；(2) 提出免执行的测试框架 SkillFuzz；(3) 实验证明方法的有效性和高效性。论文为技能市场安全提供了新的测试范式。**

<!-- paperflow:d036a33cd230d15e -->
## EvoPolicyGym: Evaluating Autonomous Policy Evolution in Interactive Environments

[[Deep Reading - Jul 2026/EvoPolicyGym-Evaluating Autonomous Policy Evolution in Interactive Environments|Deep Reading]]

[https://arxiv.org/pdf/2607.02440](https://arxiv.org/pdf/2607.02440)

- **本文针对现有智能体评估无法独立衡量策略进化能力的问题，提出了自主策略进化（Autonomous Policy Evolution）这一受控评估设置，并构建了EvoPolicyGym基准。该基准由16个紧凑的交互式RL环境组成，每个环境规定了固定的交互预算，智能体作为编码器迭代修改可执行策略代码，仅通过环境反馈进行改进。实验评估了多个语言模型智能体，结果显示GPT-5.5在聚合排名和所有环境前两名的覆盖率上均表现最优。除了排行榜结果，EvoPolicyGym还提供了轨迹级诊断工具，揭示智能体在预算分配和反馈转化上的行为差异。分析表明，成功的策略进化不仅依赖于单次任务获胜，更依赖于发现任务适当机制并在有限反馈下持续调优。论文通过这一框架为未来智能体策略进化能力的评估和优化提供了标准化平台。**

<!-- paperflow:a862a9557d82cf28 -->
## PairCoder++: Pair Programming as a Universal Paradigm for Verified Code-Driven Multimodal and Structured-Artifact Generation

[[Deep Reading - Jul 2026/PairCoder++-Pair Programming as a Universal Paradigm for Verified Code-Driven Multimodal and Str|Deep Reading]]

[https://arxiv.org/pdf/2607.01883](https://arxiv.org/pdf/2607.01883)

- **本文提出PairCoder，一种用于可验证代码驱动生成的通用配对编程范式。核心观察是，LLM生成结构化工件（如图表、CAD模型、3D场景）通常依赖代码中介，但单次推理无法利用代码的工具链反馈，导致高结构错误率。PairCoder通过两个代理的配对编程来缓解：Driver编写代码，Navigator基于工具链验证证据（编译错误、执行结果、渲染对比）审查代码，当错误持续时交换角色。该框架无需额外训练，可即插即用于任何生成式LLM。实验在17个公开基准和7个模型（包括GPT、Qwen、Claude等）上进行，广泛覆盖了各类结构化工件生成以及经典代码合成。结果显示，PairCoder在几乎所有可验证基准的官方指标套件上取得提升，例如Blender场景可执行性从0.20增至0.78，TikZ编译率提升10-30%。代价是成本增加约7倍，但改进集中在工具链提供强oracle且基线有空间的任务中。论文还分析了角色交换策略的有效性和收益模式。总体而言，PairCoder展示了配对编程作为验证驱动生成的可扩展范式的潜力，同时揭示了其适用边界和成本挑战。**

<!-- paperflow:390f2fee9797c6bf -->
## WorldDirector: Building Controllable World Simulators with Persistent Dynamic Memory

[[Deep Reading - Jul 2026/WorldDirector-Building Controllable World Simulators with Persistent Dynamic Memory|Deep Reading]]

[https://arxiv.org/pdf/2607.02517](https://arxiv.org/pdf/2607.02517)

- **本文提出 WorldDirector，一种面向可控视频世界模型的新型框架，核心创新在于持久动态对象记忆和不受限制的视点探索。现有世界模型将物理动力学与像素渲染耦合，依赖连续视觉观测，导致对象在长时间遮挡后丢失身份。WorldDirector 通过两大支柱解决该问题：第一，将语义运动编排与视觉生成解耦，利用 LLM 规划 3D 轨迹和相机运动，作为高层控制信号；第二，在因果自回归视频生成架构中将轨迹条件化，确保生成视频严格遵循规划的物理逻辑。该方法能够在视频中保持动态对象的外观一致性，即使对象离开视野很长时间后重新出现也能保持精确的视觉身份。实验表明，该方法在复杂、长时间的事件合成上实现了前所未有的可控性和持久动态记忆。主要贡献包括：（1）提出解耦语义运动编排与视觉生成的范式；（2）实现持久动态对象记忆，超越被动视频生成；（3）支持用户通过文本自由定义对象和行为，实现高度可控性。局限可能包括：依赖 LLM 规划的质量，以及长时间生成的计算开销。这篇论文代表了从视频生成向持久世界模拟器的重要一步。**

<!-- paperflow:db5b67734544781b -->
## Program-as-Weights: A Programming Paradigm for Fuzzy Functions

[[Deep Reading - Jul 2026/Program-as-Weights-A Programming Paradigm for Fuzzy Functions|Deep Reading]]

[https://arxiv.org/pdf/2607.02512](https://arxiv.org/pdf/2607.02512)

- **本文提出Program-as-Weights（PAW），一种新的模糊函数编程范式。核心思想是将自然语言函数规范一次性编译为紧凑的神经网络权重（LoRA适配器），在固定的轻量级解释器（0.6B Qwen3）上本地、离线执行，从而替代对大型LLM API的依赖。为此，作者构建了FuzzyBench数据集（1000万示例），涵盖多种模糊任务（日志分类、JSON修复、排序等）。他们训练了一个4B参数的编译器，采用两阶段流程：首先通过任务改写模板将用户规范标准化，然后由LoRA编译器生成适配器。编译后的PAW程序在测试集上达到73.78%精确匹配，超过直接提示32B模型（68.70%），而推理内存仅约1/50，在MacBook M3上以30 token/s运行。此外，PAW支持函数组合和本地部署，为日常编程中的模糊任务提供了可重复、低成本、保护隐私的解决方案。论文还讨论了局限，如当前限于单步、未验证多步推理，以及编译器未显式优化组合性。整体上，PAW将基础模型从逐个问题解决者重新定位为工具构建者，对边缘计算、隐私敏感应用和降低AI成本具有重要意义。**

<!-- paperflow:f795f0dc84556434 -->
## Search Beyond What Can Be Taught: Evolving the Knowledge Boundary in Agentic Visual Generation

[[Deep Reading - Jul 2026/Search Beyond What Can Be Taught-Evolving the Knowledge Boundary in Agentic Visual Generation|Deep Reading]]

[https://arxiv.org/pdf/2607.05382](https://arxiv.org/pdf/2607.05382)

- **这篇论文系统研究了视觉生成模型的世界知识瓶颈问题。作者首先论证了用户请求的开放性和长尾性，指出生成器的训练数据是固定的，因此对于新实体、趋势事件等无法正确生成，且会自信地产生幻觉。现有基准（如T2I-CompBench、DPG-Bench）只评估常见概念，导致生成器得分虚高（近40分虚高）。为了正视这一问题，作者构建了SearchGen-20K数据集和SearchGen-Bench基准，包含20,839条提示，覆盖12种失败类别和22个领域，并提供了预执行的多模态搜索语料库以支持离线研究。在SearchGen-Bench上的评估显示，前沿开放生成器仅得21-28分，商业模型也仅得41-58分，远低于标准基准的分数。

基于此，作者探索了使用搜索工具进行知识增强的路径。然而，简单的检索-拼接策略（朴素搜索）会不加选择地注入信息，反而干扰生成器的正常输出。作者将原因归结为生成器存在一个特定的、演化的知识边界：某些知识可通过训练内化，某些必须保留在外部上下文中。这个边界难以预先指定，但可以通过teach-then-search协同训练框架发现。

该框架包括两个阶段：首先是“teach”阶段，评估生成器的当前知识边界；然后是“search”阶段，利用拒绝采样微调训练一个搜索推理器，使其只搜索生成器确实不知道的信息，从而避免噪声。经过单次迭代的协同训练后，搜索推理器能够实现生成器自适应搜索，在SearchGen-Bench上产生从无搜索到自适应搜索的单调性能提升。论文将这一框架定位为递归自我改进的基础，未来可以通过多轮迭代进一步优化。

实验部分详细对比了无搜索、朴素搜索和自适应搜索的设置，验证了自适应搜索的有效性。消融研究可能分析了搜索时机、检索数量等因素。总体而言，这篇论文对于推动世界知识引导的视觉生成具有重要价值，提供了基准、方法和开放资源。**

<!-- paperflow:0cbe438386eb6eec -->
## Turning Off-Policy Tokens On-Policy: A Plug-in Approach for Improving LLM Alignment

[[Deep Reading - Jul 2026/Turning Off-Policy Tokens On-Policy-A Plug-in Approach for Improving LLM Alignment|Deep Reading]]

[https://arxiv.org/pdf/2607.04728](https://arxiv.org/pdf/2607.04728)

- **本文针对LLM RL后训练中的离策略问题，提出选择性重要性采样（SIS）方法。该方法受拒绝采样启发，在token级别将离策略token转换为在策略token，从而避免传统重要性采样比率连乘导致的方差爆炸。SIS作为即插即用模块，仅修改策略损失中的重要性比率，几乎不增加计算开销。在稠密和MoE架构的LLM上，覆盖数学推理和智能体任务，SIS一致提升所有评估指标，并提供更强的鲁棒性，尤其是在高度离策略数据下。理论分析证明SIS缩小了token级与序列级梯度估计的差距。与现有方法（如R3）正交并可以结合取得更好性能。论文的主要贡献是提出一种新颖的分布转换视角来解决离策略偏差，并基于拒绝采样实现简单高效的具体算法。**

<!-- paperflow:653bf3db72a1f945 -->
## STAPO: Selective Trajectory-Aware Policy Optimization for LLM Agent Training

[[Deep Reading - Jul 2026/STAPO-Selective Trajectory-Aware Policy Optimization for LLM Agent Training|Deep Reading]]

[https://arxiv.org/pdf/2607.04963](https://arxiv.org/pdf/2607.04963)

- **本文从强化学习中稀疏奖励导致LLM agent轨迹忽视的问题出发，指出已有基于Shannon熵的步骤级不确定性监督不准确的原因——混淆了状态随机性和智能体置信度。作者提出归一化熵（normalized entropy），通过将当前动作分布的熵与智能体在该状态下的平均熵比较，消除状态复杂度的影响，从而可靠地识别因轨迹忽视产生的低质量动作。基于此，设计了STAPO（Selective Trajectory-Aware Policy Optimization），一个层次化分组RL框架，包含两阶段操作：异常定位阶段，利用归一化熵的统计特征动态定位轨迹中的异常步骤；选择性优化阶段，对这些异常步骤联合采用轨迹感知奖励（鼓励轨迹一致性）和轨迹无关惩罚（维持训练稳定性），而其他步骤保持正常策略更新。在ALFWorld、WebShop和搜索增强问答三个代表性的长程智能体任务上，STAPO均取得最优成功率，且消融实验证实归一化熵和联合优化机制的关键作用。分析表明，计算开销增加约5.7%，但相比扩大组大小更经济。作者还给出了在视觉-语言agent上的初步结果，但指出更广泛的多模态设置仍需探索。论文的主要贡献在于：1）定义轨迹忽视问题并分析现有熵方法的局限；2）提出归一化熵以精准定位异常步骤；3）构建实用的STAPO框架；4）在多个任务上验证有效性。**

<!-- paperflow:b3bc773ad440df03 -->
## Multi-Turn On-Policy Distillation with Prefix Replay

[[Deep Reading - Jul 2026/Multi-Turn On-Policy Distillation with Prefix Replay|Deep Reading]]

[https://arxiv.org/pdf/2607.04763](https://arxiv.org/pdf/2607.04763)

- **论文研究多轮在线蒸馏（OPD）在智能体任务中的高效实现。完全在线的 OPD 虽然效果好，但需要大量环境交互，成本高。作者发现多轮 OPD 存在前缀陷阱：使学生历史更 on-policy 虽然能提高相关性，但可能降低教师在这些历史上的目标可靠性。为解决这一问题，提出 ReOPD（Replayed-Prefix On-Policy Distillation），一种无需环境交互的离线蒸馏方法。ReOPD 利用预先收集的教师轨迹作为重放前缀，在选定步骤让学生行动并由教师提供监督。它通过步骤衰减采样策略优先使用早期前缀，从而在保持学生相关性同时确保教师可靠性。实验在数学推理和搜索任务上验证了 ReOPD 的有效性：准确率匹配或超越在线 OPD，训练速度至少快4倍，且无需在训练中调用工具。该方法将昂贵的智能体交互转化为可复用的离线资源，为大规模蒸馏提供了可扩展的范本。主要贡献在于：(1) 提出 ReOPD 算法；(2) 揭示前缀陷阱；(3) 将多轮 OPD 视为可靠性感知的前缀分布设计；(4) 实现高效训练，零环境交互。**

<!-- paperflow:2a81575bf04628a1 -->
## UI-MOPD: Multi-Platform On-Policy Distillation for Continual GUI Agent Learning

[[Deep Reading - Jul 2026/UI-MOPD-Multi-Platform On-Policy Distillation for Continual GUI Agent Learning|Deep Reading]]

[https://arxiv.org/pdf/2607.04425](https://arxiv.org/pdf/2607.04425)

- **本文针对多平台GUI代理构建中的数据稀缺和行为模式混淆问题，提出了UI-MOPD方法，首次将多教师在线策略蒸馏引入GUI代理的持续学习框架。

**动机与挑战**：随着多模态基础模型的发展，GUI代理从单平台向跨平台演进成为趋势。然而，跨平台数据难以获取，且不同平台交互惯例差异大，导致联合训练出现行为混合、平台能力退化及灾难性遗忘。现有工作多聚焦于统一建模，但未专门处理持续学习中的平台特定能力保持问题。

**方法**：
1. **Uni-GUI数据集**：收集桌面（OSWorld）和移动（MobileWorld）平台的高质量交互轨迹，确保数据多样性和可执行性。
2. **UI-MOPD框架**：包含多个平台特定教师策略（在每个平台单独训练）和一个平台条件共享学生策略。共享策略通过平台嵌入（platform embedding）条件化，使其能根据不同环境切换交互模式。
3. **在线策略蒸馏**：持续学习过程中，学生与环境交互产生新轨迹，并依据当前平台动态选择对应教师，通过蒸馏损失（如KL散度）将教师的知识迁移给学生，同时学生在当前任务上通过强化学习（如REINFORCE）优化任务成功。
4. **平台条件蒸馏**：蒸馏损失中显式包含平台标识，确保学生针对不同平台学习独立的行为分布，避免模式混叠。

**实验**：在OSWorld（678任务）和MobileWorld（200任务）上评估，将UI-MOPD与UI-TARS-2、GELab-Zero-4B、MobileAgent-v3.5等基线对比。结果表明：
- UI-MOPD在OSWorld上达到38.2%成功率，在MobileWorld上达到12.0%，性能具有竞争力且跨平台更均衡。
- 相比顺序微调，UI-MOPD显著缓解了灾难性遗忘，保持了先前平台的能力。
- 消融实验验证了多教师蒸馏和平台条件的必要性。

**贡献**：
1. 构建了Uni-GUI跨平台数据集。
2. 提出了UI-MOPD，首个用于GUI代理持续学习的多教师在线策略蒸馏方法。
3. 系统实验证明了方法在跨平台能力平衡和遗忘缓解上的有效性。

*...**

<!-- paperflow:cb27ab48c77e7b43 -->
## CompactionRL: Reinforcement Learning with Context Compaction for Long-Horizon Agents

[[Deep Reading - Jul 2026/CompactionRL-Reinforcement Learning with Context Compaction for Long-Horizon Agents|Deep Reading]]

[https://arxiv.org/pdf/2607.05378](https://arxiv.org/pdf/2607.05378)

- **本文提出CompactionRL，一个将可学习上下文压缩融入PPO强化学习框架的方法，用于解决长周期智能体LLM的上下文窗口限制问题。论文首先指出现有RL流程假设完整轨迹可用，但长周期任务中轨迹可能超出窗口，需要压缩。CompactionRL在rollout收集阶段动态触发压缩：当交互历史长度接近预算时，模型生成当前状态的紧凑总结，并用该总结和少量最近上下文替换旧历史。训练时采用token-level loss normalization和cross-trajectory GAE两项技术，确保多段压缩轨迹的有效学习。实验在SWE-bench Verified和Terminal-Bench 2.0两个编程基准上进行，使用GLM-4.5-Air和GLM-4.7-Flash作为基座模型。结果显示CompactionRL在Pass@1上取得显著提升，绝对增益3.1~6.8点不等，且一致性良好。该框架已被部署到智谱GLM-5.2模型的RL训练中，进一步验证其实用价值。主要贡献在于首次提出端到端的RL训练框架，将上下文压缩从推理技巧转化为可训练组件，为构建超长周期智能体提供了可行方案。**

<!-- paperflow:1cdfdf84ace37ece -->
## GaP: A Graph-as-Policy Multi-Agent Self-Learning Harness For Variational Automation Tasks

[[Deep Reading - Jul 2026/GaP-A Graph-as-Policy Multi-Agent Self-Learning Harness For Variational Automation Tasks|Deep Reading]]

[https://arxiv.org/pdf/2607.05369](https://arxiv.org/pdf/2607.05369)

- **本文提出Graph-as-Policy (GaP)，一种用于变分自动化（VA）任务的多智能体自学习框架。VA任务要求机器人在反复执行同类操作时应对对象几何和姿态的变化，而现有模型无关策略可靠性不足，LLM代码生成易幻觉。GaP结合TAMP的结构化可解释性与LLM的灵活性，将策略表示为有向计算图，节点来自模块化开放机器人技能库（MORSL）。多智能体系统（分解、编码、校验、优化）协作生成初始图，然后通过内部仿真排练和迭代优化（调整参数和拓扑）最大化成功率和吞吐量。论文贡献包括：定义VA任务类别、提出GaP框架、构建MORSL、开源8个VA基准（4仿真+4真实世界）。实验表明GaP在成功率和吞吐量上显著优于模型无关策略、TAMP和基于LLM的代码方法。消融实验证实迭代优化和多智能体协作是关键。尽管存在依赖LLM、几何推理有限、sim-to-real差距等局限性，GaP在可靠性和灵活性间取得了有效平衡，为工业机器人自动化提供了有前景的框架。**

<!-- paperflow:953802decf4970d6 -->
## OptiAgent: End-to-End Optimization Modeling via Multi-Agent Iterative Refinement

[[Deep Reading - Jul 2026/OptiAgent-End-to-End Optimization Modeling via Multi-Agent Iterative Refinement|Deep Reading]]

[https://arxiv.org/pdf/2607.05346](https://arxiv.org/pdf/2607.05346)

- **本文提出OptiAgent，一个端到端的多智能体优化建模框架。给定自然语言问题描述，框架自动生成数学公式和求解代码，通过四个专用智能体和四个反馈循环进行迭代精炼。主要贡献包括：1）优先数学建模的模块化架构；2）多循环验证机制，覆盖五种关键失败模式；3）在多个基准上达到SOTA性能。实验证明，该方法在LP、MILP和NLP任务上均优于先前工作，同时提高了建模过程的透明度和可审计性。**

<!-- paperflow:24411e96e1ccc129 -->
## HunyuanOCR-1.5: Making Lightweight OCR VLMs Faster and Better

[[Deep Reading - Jul 2026/HunyuanOCR-1.5-Making Lightweight OCR VLMs Faster and Better|Deep Reading]]

[https://arxiv.org/pdf/2607.04884](https://arxiv.org/pdf/2607.04884)

- **HunyuanOCR-1.5是轻量级端到端OCR专用视觉语言模型的升级版本，继承HunyuanOCR-1.0的骨干设计，重点提升推理速度和长尾OCR能力。论文的动机源于当前OCR VLM在文档表格等长结构文本上解码缓慢，以及在古籍、多图像理解等特殊场景下性能不足的局限性。为解决这些问题，作者引入了两个关键技术：DFlash推理加速技术和Agentic Data Flow智能体数据构建系统。DFlash基于推测性解码，在保持输出质量的前提下显著降低解码延迟，实验显示Transformer引擎下加速6.37倍，vLLM下加速2.14倍，达到轻量级OCR VLM中最快推理速度。Agentic Data Flow则构建了一个自动化流水线，将模型弱点转化为数据需求，自主搜索、验证并生成针对性训练数据，有效提升了模型在古籍OCR、图表表格解析、多图像问答等长尾领域的能力。同时，论文采用能力导向评估体系，在OmniDocBench v1.6上取得顶级端到端OCR性能，并在多个专门构建的基准上创下新纪录。模型还通过升级预训练（更高分辨率、更大数据量）和后训练（多任务微调、长上下文扩展）进一步扩展边界。关键贡献包括：(1) 首个将推测性解码适配到OCR结构化输出并获得显著加速的工作；(2) 提出基于智能体的数据构造框架弥补长尾数据短缺；(3) 在多项长尾任务上达到最优，且不牺牲核心OCR能力；(4) 提供开源模型和加速工具，推动研究传播。整体而言，HunyuanOCR-1.5在效率和质量上实现了平衡，成为轻量级端到端OCR VLM的卓越标杆。**

<!-- paperflow:814ffe7524521b11 -->
## AgentGym2: Benchmarking Large Language Model Agents in De-Idealized Real-World Environments

[[Deep Reading - Jul 2026/AgentGym2-Benchmarking Large Language Model Agents in De-Idealized Real-World Environments|Deep Reading]]

[https://arxiv.org/pdf/2607.05174](https://arxiv.org/pdf/2607.05174)

- **语言智能体正快速部署，但现有评估多基于理想化环境，无法反映真实世界中的不确定性、噪声和探索需求。本文提出AgentGym2，一个去理想化的评估框架，其任务实例直接源于真实端到端工作需求，除推理和规划外，重点测量端到端程序执行、工具发现、工具组合和鲁棒性四项能力。框架基于AgentGym扩展，增加了数字真实环境（如终端、浏览器），构建了可组合工具箱，并采用分层设计解耦智能体、工具与环境。实验评估了15个主流闭源和开源模型，发现包括GPT-5、Gemini在内的SOTA系统在AgentGym2上表现挣扎，尤其在工具发现和组合任务上远未达到可部署水平，凸显了当前智能体能力与现实应用之间的鸿沟。论文还分析了不同模型的强弱项，指出工具探索和适应新组合是主要瓶颈。AgentGym2为智能体研究提供了一个更贴近实际需求的测试平台，有望推动更具鲁棒性和泛化能力的智能体系统发展。**

<!-- paperflow:b8c018f5d8ea9304 -->
## LLM-as-a-Verifier: A General-Purpose Verification Framework

[[Deep Reading - Jul 2026/LLM-as-a-Verifier-A General-Purpose Verification Framework|Deep Reading]]

[https://arxiv.org/pdf/2607.05391](https://arxiv.org/pdf/2607.05391)

- **论文提出LLM-as-a-Verifier，一种无需训练的通用验证框架，通过连续评分实现了验证能力的缩放。背景：尽管预训练、后训练和测试时计算的缩放极大提升了LLM能力，但验证——判断解决方案正确性的能力——未得到同等关注。标准方法如LM Judge仅提供离散分数，粒度粗、区分度低。方法：框架的核心是计算评分token logits的期望，得到连续分数。在此基础上，作者识别出三个缩放轴：(1) 评分粒度（细化评分尺度），(2) 重复评估（多次评分平均），(3) 标准分解（多维标准评分聚合）。实验：在Terminal-Bench V2 (86.5%)、SWE-Bench Verified (78.2%)、RoboRewardBench (87.4%)、MedAgentBench (73.3%)上达到SOTA。缩放实验证实各轴均能提升准确率，且粒度轴收益最大。作为过程奖励模型时，pass@1单调提升。作为强化学习奖励信号，显著提升SAC在机器人任务和GRPO在数学推理上的样本效率（约2倍和1.1倍）。此外，细粒度信号可作为任务进展代理，并开发了TurboAgent工具集成到Claude Code。讨论：验证应被视为与生成同等重要的缩放轴。局限：需要访问模型logits，不适用于完全封闭API；部分场景下连续评分的校准性仍待改进。结论：LLM-as-a-Verifier提供了一种低成本、高收益的验证增强方案，有望成为智能体系统的标准组件。**

# AI Research

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Research
- 方法：待从后续精读中沉淀
- 论文/报告：2 篇
- What Types of Human-AI Teams Exist?
- HERMES: A Multi-Granularity Labeling Substrate for Pre-training Data Mixtures
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:045cc92ab4e1cd65 -->
## What Types of Human-AI Teams Exist?

[[Deep Reading - Jul 2026/What Types of Human-AI Teams Exist|Deep Reading]]

[https://arxiv.org/pdf/2607.02198](https://arxiv.org/pdf/2607.02198)

- **本论文是一篇系统文献综述，旨在厘清当前人机团队研究中存在的不同类型及其特征。作者分析了53篇相关论文，从心理学团队分类视角出发，归纳出5个独特的团队集群：AI Assistant（AI作为辅助工具）、Ad-hoc Dependency（临时依赖）、Ad-hoc Forced Dependency（强制临时依赖）、Paired Equanimity（对等配对）和Group Equanimity（对等群体）。每个集群在团队规模、成员关系、任务依赖性、领导结构和沟通模式等维度上各有特征。研究发现，即使团队规模相同（如一对一），不同集群的特征组合也可能差异显著，这说明“人机团队”这一术语掩盖了重要的内部异质性，可能阻碍研究间的知识迁移。为此，论文提出了一套报告检查清单，要求研究者明确团队的关键特征，并呼吁采用更精确的分类语言。论文最后讨论了如何在更广范围内整合人机团队研究，包括跨领域比较和方法论改进。**

<!-- paperflow:b1c739a2a0db65a4 -->
## HERMES: A Multi-Granularity Labeling Substrate for Pre-training Data Mixtures

[[Deep Reading - Jul 2026/HERMES-A Multi-Granularity Labeling Substrate for Pre-training Data Mixtures|Deep Reading]]

[https://arxiv.org/pdf/2607.02266](https://arxiv.org/pdf/2607.02266)

- **本文提出HERMES，一种多粒度标签基底，用于改进预训练数据混合中的标签系统。当前数据混合方法通常假设语料已被划分为固定组，现有标签（来源、主题、格式、嵌入聚类）只在一个语义轴和粒度上操作，改变分辨率需要重建整个标签集。论文认为瓶颈在于标签系统，而非混合器，因此设计了一个数据驱动的层次化标签系统。HERMES包含两个核心组件：学习语义变换（LST）将文档映射到语义空间；3阶段残差向量量化（RVQ）将每个文档编码为一个粗到细的三层代码，通过截断前缀可以获得任意粗粒度的标签。具体地，L1约有256个粗类，L2约有65536个中类，L3有效约130k个细类。训练通过重建嵌入损失和码本损失联合优化。实验在1B参数、25B token的预训练上进行，使用FineWeb数据子集，评估16个自然语言理解任务。在粗粒度L1，HERMES的聚类指标与KMeans类方法相当，表明其优势不在单一聚类质量而在层次性。在L2粒度，对比两种混合规则：等子桶覆盖和按比例取子桶内质量前30%，后者在16任务宏平均上提升0.0253。但在L3粒度，相同规则优势消失，原因是候选子桶缩小约5倍，桶内文档数中位数从约429降至约81，导致基于排名的规则不稳定。消融实验确认L1分组选择不是性能来源。论文通过这些实验揭示了固定粒度管线无法发现的粒度-规则交互：收益依赖于粒度选择，粗粒度下分组方法差异小，中粒度下规则差异显现，细粒度下规则差异消失。HERMES将数据混合设计从选择固定标签集转换为在可复用的粒度层次中导航，为数据混合提供了新的研究范式。论文最后讨论了局限性，包括两个控制实验缺失、机制证据强度有待加强、更深前缀的适用性以及数据分布变化的影响。**

# Computer Vision

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Computer Vision
- 方法：language, vision-language-model, reinforcement-learning, vision, vision-language, deep-learning
- 论文/报告：7 篇
- Optimizing Visual Generative Models via Distribution-wise Rewards
- Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning
- ESC: Emotional Self-Correction for Reliable Vision-Language Models
- From SRA to Self-Flow: Data Augmentation or Self-Supervision?
- ChatImage: Navigating Long-Form LLM Answers through Interactive Images
- SteelBench: Evaluating Vision-Language Models in Real-World Industrial Environments
- GUSH3R: Everyone Everywhere All at Once as Gaussians
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:a12b6e3a0256529f -->
## Optimizing Visual Generative Models via Distribution-wise Rewards

[[Deep Reading - Jul 2026/Optimizing Visual Generative Models via Distribution-wise Rewards|Deep Reading]]

[https://arxiv.org/pdf/2607.02291](https://arxiv.org/pdf/2607.02291)

- **本文针对视觉生成模型强化学习微调中样本级奖励的缺陷，提出分布级奖励框架。通过考虑生成样本的整体分布而非单个样本，有效缓解奖励黑客和模式崩溃。为高效计算分布级奖励，设计了子集替换策略，仅更新生成参考集的一小部分即可获得奖励信号。此外，利用强化学习优化后处理模型合并系数，解决SDE训练与ODE推理之间的不一致。实验在SiT-XL/2和EDM2上展示了FID-50K的显著提升（SiT: 8.30→5.77; EDM2: 3.74→3.52），同时定性结果也验证了视觉质量的改进和多样性的保持。本文方法为生成模型的对齐提供了新思路，具有较好的实用性和扩展性。**

<!-- paperflow:303e62d796883732 -->
## Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning

[[Deep Reading - Jul 2026/Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2607.02490](https://arxiv.org/pdf/2607.02490)

- **本文研究了视觉语言模型（LVLM）的自我反思能力，特别是如何通过强化学习训练使其进行视觉基础的自我反思。现有LVLM在生成思维链时虽然具备反思能力，但在分布外图像上经常忽略视觉输入，导致纠错失败。作者提出VRRL框架，包含两个核心组件：随机掩码（Random Turn Masking）和缓冲回滚注入（Buffered Roll-In）。随机掩码通过只对轨迹后缀进行更新，强迫模型从错误中间状态中恢复；缓冲回滚通过从经验池中采样历史失败状态，使模型暴露于多样化的失败情形。训练分两步：先SFT初始化，再RL训练，奖励来自环境根据视觉反馈给出的正确性信号。实验在表格理解、图表理解和空间导航任务上进行，评估分布外泛化能力。结果表明，VRRL显著优于标准RL和反思微调基线，尤其是分布外准确率提升明显。消融研究验证了两个组件的必要性。此外，分析显示VRRL模型确实进行多轮视觉基础的反思，纠正了初始错误，而基线模型则倾向于忽略视觉信息。本工作的主要贡献包括：首次通过RL专门训练视觉基础的自我反思；提出两个促进视觉基础反思的训练机制；在多个OOD场景下验证有效性。限制包括：需要环境提供视觉反馈，不适用于开放VQA；实验任务有限。未来可探索更广泛的应用。总体而言，本文为提升LVLM鲁棒性提供了一个有效且可解释的方向，通过强化学习实现了视觉基础的自我反思，对于智能体等需要多轮交互和纠错的应用有重要参考价值。**

<!-- paperflow:1e229e90231b6251 -->
## ESC: Emotional Self-Correction for Reliable Vision-Language Models

[[Deep Reading - Jul 2026/ESC-Emotional Self-Correction for Reliable Vision-Language Models|Deep Reading]]

[https://arxiv.org/pdf/2607.02089](https://arxiv.org/pdf/2607.02089)

- **本文提出了一种名为情感自我校正（Emotional Self-Correction, ESC）的无训练框架，旨在利用情感提示提升视觉语言模型（VLM）的推理可靠性。通过初步实验，作者发现情感信号（如鼓励谨慎思考的语句）可以有效唤醒 VLM 的自我纠错潜能，而无需进行额外的模型训练。基于这一发现，ESC 设计了包含外部验证器和情感反馈的三阶段流程：首先，VLM 对给定输入生成初始响应；其次，一个独立的验证器评估该响应的正确性，仅在判断为可能错误时才启动修订步骤；最后，向模型注入包含情感内容的提示，引导模型重新审视并生成修订回答。该设计避免了不必要的修订计算，同时利用情感信号引导模型进行更深度的思考。论文在安全性、幻觉、以视觉为中心的感知和多模态推理四大类基准上进行了广泛实验，结果表明 ESC 能一致提升模型在这些领域中的可靠性，并且不损害模型的原始效用。此外，论文还探讨了情感的双重角色——既作为能被模型识别的内容，也作为结构化的控制信号。通过定量分析和消融研究，作者验证了验证器、情感类型、注入时机等关键组件的作用。ESC 为轻量级、可扩展的 VLM 可靠性改进提供了新范式，并启发了将情感与人工智能更紧密结合的研究方向。**

<!-- paperflow:e4d3af3b9901e304 -->
## From SRA to Self-Flow: Data Augmentation or Self-Supervision?

[[Deep Reading - Jul 2026/From SRA to Self-Flow-Data Augmentation or Self-Supervision|Deep Reading]]

[https://arxiv.org/pdf/2607.02508](https://arxiv.org/pdf/2607.02508)

- **论文从质疑Self-Flow性能提升来源出发，设计Attention Separation实验，证明双时步调度的主要贡献是噪声维度的数据增强而非自对齐增强。进一步发现Attention Separation本身也可作为数据增强方法。最终构建了包含自对齐、双时步调度和注意分离增强的统一训练方案，并在ImageNet 256×256上验证了有效性。论证主线为：提出假说→设计诊断实验→验证假说→利用发现构建新方法。技术主线为：SRA自对齐目标 → DTS双时步输入 → AS注意分离掩码。实验主线为：对比SRA、Self-Flow、DTS+AS和最终方案，消融各组件贡献。论文的主要贡献在于澄清了自对齐方法的关键机制，并提供了诊断和增强工具。**

<!-- paperflow:d2fdf45dcb431da1 -->
## ChatImage: Navigating Long-Form LLM Answers through Interactive Images

[[Deep Reading - Jul 2026/ChatImage-Navigating Long-Form LLM Answers through Interactive Images|Deep Reading]]

[https://arxiv.org/pdf/2607.05290](https://arxiv.org/pdf/2607.05290)

- **ChatImage 论文提出了一种将长形式 LLM 答案转换为交互式视觉图像的系统。动机是解决当前 LLM 答案以线性文本呈现导致的细粒度检查困难。方法包括内容结构化、布局规划、图像生成和视觉接地，其中关键设计是'生成-然后-接地'，即先合成图像再通过视觉模型定位交互区域，以保证一致性。论文发布了参考实现和包含 30 个问题的基准，涵盖信息图、地图和场景。实验使用交互循环完成度、视觉对齐门控和掩码完整性指标进行评估，结果显示 ChatImage 能够有效生成可交互图像，且接地策略优于基线。论文还讨论了应用场景（如教育和技术文档）和局限性（最适合已有空间结构的答案）。总体而言，ChatImage 为 LLM 答案的可视化和交互提供了一种新颖的范式。**

<!-- paperflow:24835882d75343b0 -->
## SteelBench: Evaluating Vision-Language Models in Real-World Industrial Environments

[[Deep Reading - Jul 2026/SteelBench-Evaluating Vision-Language Models in Real-World Industrial Environments|Deep Reading]]

[https://arxiv.org/pdf/2607.05264](https://arxiv.org/pdf/2607.05264)

- **这篇论文引入SteelBench，一个旨在评估视觉-语言模型在真实工业环境（特别是钢铁厂CCTV监控）中能力的诊断基准。现有视频和VLM基准缺乏对工业CCTV特有的视觉退化（灰尘、低光、遮挡）和程序性推理（活动识别与安全规则判断）的测试。SteelBench包含从149小时工厂录像中精心挑选的1345个密集标注片段，每片段标注每个工人的动作、PPE状态、空间位置和关联安全规则。论文创新性地提出来源感知审计协议，以量化使用VLM辅助标注可能带来的自我验证偏见，发现未审计的VLM标签可使同族模型准确率高估17个百分点。作者在九个代表性VLMs（包括GPT-4o、Gemini、Qwen、LLaVA等）上进行了动作识别、安全推理、校准和诊断检查等多维度评估。结果显示最佳模型Qwen3.5-122B仅达42.6%动作准确率，远低于人类84.6%的基准。即使动作正确，模型也频繁在安全判断上出错。没有一个模型通过五项预设诊断检查中的两项以上。结果表明，在工业活动理解中，简单的准确率排行榜不足以衡量能力，需要关注失败模式、来源可靠性和推理一致性。SteelBench数据集公开，为未来研究提供了诊断工具和基准。**

<!-- paperflow:8f882c8a7bb1f77a -->
## GUSH3R: Everyone Everywhere All at Once as Gaussians

[[Deep Reading - Jul 2026/GUSH3R-Everyone Everywhere All at Once as Gaussians|Deep Reading]]

[https://arxiv.org/pdf/2607.05243](https://arxiv.org/pdf/2607.05243)

- **本文提出GUSH3R，一个用于从单目视频进行动态人体-场景重建的前馈框架。核心思想是结合几何先验（DUSt3R）和人体先验（SMPL），在单次前向传播中同时重建动态人体和静态场景为统一的3D高斯泼溅表示。该表示支持可微渲染，可直接进行高保真新视角合成。

方法上，GUSH3R分别预测场景高斯和人体高斯，利用先验参数化确保几何一致性，并合并为统一场景。在多个数据集上的实验表明，该方法在新视角合成质量上达到或接近现有优化方法，但推理速度大幅提升，展示了前馈方案在动态场景重建中的潜力。

局限性包括依赖先验质量、对严重遮挡和复杂交互效果有限、未利用时间信息等。本文工作为实时动态人体-场景重建提供了新思路，未来可向更鲁棒、更高效的方向发展。**

# Machine Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Machine Learning
- 方法：待从后续精读中沉淀
- 论文/报告：1 篇
- Denser $\neq$ Better: Limits of On-Policy Self-Distillation for Continual Post-Training
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:43d174bb099a0b76 -->
## Denser $\neq$ Better: Limits of On-Policy Self-Distillation for Continual Post-Training

[[Deep Reading - Jul 2026/Denser $-neq$ Better-Limits of On-Policy Self-Distillation for Continual Post-Training|Deep Reading]]

[https://arxiv.org/pdf/2607.01763](https://arxiv.org/pdf/2607.01763)

- **本文主要研究on-policy自蒸馏在持续后训练中的实际效果和价值。作者首先指出持续后训练面临的知识获取与遗忘保持的矛盾，以及近期on-policy学习被认为能缓解遗忘的趋势。他们提出自蒸馏策略优化（SDPO）框架作为研究工具，在on-policy数据上实施密集的token级蒸馏训练。通过精心设计的单领域特化实验和多阶段持续后训练实验，论文获得了关键的对比结果：SDPO在教师信号稳定时能快速提升领域内任务表现，但无法推广到新分布且在持续学习中导致显著遗忘甚至崩溃；与之对比，on-policy RL方法GRPO收敛较慢但能可靠地保留旧知识。进一步分析发现，密集自蒸馏会增大参数和响应的漂移，并通过教师-学生循环放大高频格式伪影。基于这些现象，作者得出核心结论：on-policy数据本身不保证持续学习成功；密集自蒸馏的适用条件严格，不应作为默认稳定器。论文的贡献在于对乐观假设的实验性检验、开发了简单有效的诊断指标、并揭示了重要的失败模式，为后续研究提供了实证基础和警示。**
