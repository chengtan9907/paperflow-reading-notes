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
- 论文/报告：3 篇
- DisciplineGen-1M: A Large-Scale Dataset for Multidisciplinary Visual Generation and Editing
- CausalGame: Benchmarking Causal Thinking of LLM Agents in Games
- Playing ZendoWorld: Challenging AI Agents on Active Visual Concept Induction
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

<!-- paperflow:4bb61e246f20af89 -->
## Playing ZendoWorld: Challenging AI Agents on Active Visual Concept Induction

[[Deep Reading - Jul 2026/Playing ZendoWorld-Challenging AI Agents on Active Visual Concept Induction|Deep Reading]]

[https://arxiv.org/pdf/2607.08233](https://arxiv.org/pdf/2607.08233)

- **论文针对智能系统在主动视觉概念归纳中面临的核心挑战，提出了ZendoWorld基准环境。ZendoWorld是一个受控游戏环境，要求智能体从视觉场景中推断隐藏的逻辑规则，并通过设计新场景来主动获取信息，形成感知-归纳-实验设计的闭环。论文实现了多种代表性智能体：基于VLM（GPT-5-mini）的端到端方法、贝叶斯粒子滤波、动态概念发现以及神经符号方法，并在22个不同复杂度的游戏上进行了系统比较。实验围绕四个研究问题展开：端到端解决能力、感知瓶颈、归纳准确性和实验信息性。主要发现包括：(1)标注准确率与规则恢复显著解耦，高标注率并不反映对规则的理解；(2)感知和归纳是不同智能体的不同瓶颈，VLM感知强但归纳弱，而贝叶斯方法归纳好但感知受限；(3)VLM智能体在实验设计上表现极差，提出的场景几乎无信息价值，甚至不如随机基线；(4)人类实验表明，即使是人类在复杂规则下也会出现类似错误，说明归纳本身具有挑战性。ZendoWorld为评估和提升智能体的主动推理能力提供了新的测试平台，并指出了具体改进方向，例如增强实验设计策略、结合符号与神经方法等。论文还公开了代码和数据，以推动该方向的研究。**

# AI for Education

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Education
- 方法：agent, ai-for-science, generation, explanation, math-pr, math-st
- 论文/报告：5 篇
- AIriskEval-edu: New Dataset for Risk Assessment in AI-mediated K-12 Educational Explanations
- MoWorld: A Flash World Model
- Extending LLM Context via Associative Recurrent Memory
- AdvancedMathBench: A Benchmark Suite for Advanced Mathematical Proof Generation and Verification
- SearchOS-V1: Towards Robust Open-Domain Information-Seeking Agent Collaboration
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:3e721192a35a0890 -->
## AIriskEval-edu: New Dataset for Risk Assessment in AI-mediated K-12 Educational Explanations

[[Deep Reading - Jul 2026/AIriskEval-edu-New Dataset for Risk Assessment in AI-mediated K-12 Educational Explanations|Deep Reading]]

[https://arxiv.org/pdf/2607.01934](https://arxiv.org/pdf/2607.01934)

- **本文提出了AIriskEval-edu-db2数据集，用于K-12教学解释的可解释风险评估。数据集包含170个ScienceQA问题，每个问题有12个解释（1个人类+11个LLM模拟），覆盖五个风险维度，其中785个解释附带结构化可解释性标注。通过半自动流程结合专家验证确保质量。实验将Llama 3.1 8B经LoRA微调后与GPT-4、Claude等模型对比，结果显示微调后的轻量模型在多数风险维度上接近前沿模型，在可解释性任务上表现有竞争力。该数据集支持本地部署的教育审计应用，平衡了性能与隐私。论文的主要贡献是提供了首个带细粒度可解释性标注的教学风险数据集，并验证了小模型微调的可行性。**

<!-- paperflow:e5662ff411126cbe -->
## MoWorld: A Flash World Model

[[Deep Reading - Jul 2026/MoWorld-A Flash World Model|Deep Reading]]

[https://arxiv.org/pdf/2607.06216](https://arxiv.org/pdf/2607.06216)

- **本文提出了 MoWorld，一种高效、低成本的 Flash World Model，旨在解决世界模型在实际部署中的效率和成本问题。通过可扩展的 3D 原生数据引擎、课程式跨帧预训练、降噪步蒸馏和混合精度并行推理的协同设计，MoWorld 在 NPU 上实现了 50 FPS 的实时交互，同时将推理成本降低 30-50%。实验证明了其领先的性能和广泛的应用潜力。MoWorld 是首个为 NPU 设计的实时交互世界模型，为世界模型的大规模实际应用提供了基础。**

<!-- paperflow:acb150f818058a76 -->
## Extending LLM Context via Associative Recurrent Memory

[[Deep Reading - Jul 2026/Extending LLM Context via Associative Recurrent Memory|Deep Reading]]

[https://arxiv.org/pdf/2607.11614v1](https://arxiv.org/pdf/2607.11614v1)

- **本文针对LLM长上下文的计算和内存瓶颈，提出基于关联递归记忆（Associative Recurrent Memory Transformer, ARMT）的实用扩展方案。作者首先指出标准Transformer的O(L^2)复杂度和线性内存限制了实际应用；现有改进方案（如稀疏注意力、状态空间模型）多需从头训练，无法直接复用预训练权重。为此，他们继承并改进Recurrent Memory Transformer（RMT），通过在Transformer层中插入分段级关联记忆模块，实现常数内存和线性时间。核心贡献包括：一套系统化的训练配方——持续预训练、合成长上下文数据生成、课程学习和选择性记忆集成；两个域特定长上下文数据集（ManyTypes-long和GovReport-long）；以及详尽的实验分析。实验表明，ARMT能够在保持原窗口内性能的基础上，处理超出原始上下文极限2-4倍的输入，在分布外长度上泛化优于基线，并减少30% FLOPs。消融研究证实了配方中各组件的必要性。局限主要在于实验仅覆盖≤1B参数的模型以及域特定任务。整体上，这项工作为在有限资源下利用现有LLM进行高效长上下文处理提供了一条可行路径。**

<!-- paperflow:282e183f3950190c -->
## AdvancedMathBench: A Benchmark Suite for Advanced Mathematical Proof Generation and Verification

[[Deep Reading - Jul 2026/AdvancedMathBench-A Benchmark Suite for Advanced Mathematical Proof Generation and Verification|Deep Reading]]

[https://arxiv.org/pdf/2607.11849v1](https://arxiv.org/pdf/2607.11849v1)

- **这篇论文系统评估了大语言模型在高级数学证明生成与验证上的能力。作者首先指出现有数学推理基准的不足：范围局限于初等数学且评估粒度粗糙，无法对证明过程进行可靠、细粒度的自动评判。为此，他们提出了AdvancedMathBench基准套件，由ProverBench和VerifierBench两部分组成。

ProverBench包含296个从大学教材、博士资格考试题目中筛选的高级数学问题，覆盖代数、分析、几何、拓扑、逻辑等多个分支，并分为UG（本科）和QE（博士资格考试）两个难度层次。每个问题附有标准自然语言证明。为自动评估证明正确性，论文开发了一套专用验证流程：通过大规模采集专家对模型生成证明的标注（包括整体有效性、错误类型和位置），训练一个验证模型，该模型能同时输出二元判决和细粒度的错误定位。验证流程采用了理性感知奖励、正面证明修复和悲观验证等机制，在保留测试集上实现了与人类专家高度一致的评判。

VerifierBench则用于单独评估模型自身的验证能力。他们收集了888条由多种模型生成的证明轨迹，并请专家标注每条轨迹的正确性及理由，以此作为金标准。在VerifierBench上，模型需要判断给定证明的有效性并解释其判断，通过元验证器计算其与专家的一致性。

实验规模评测了多个前沿LLM。在ProverBench生成任务上，最佳模型GPT-5.5-xhigh在UG和QE子集上分别取得75.8和66.1的得分（百分比），证明高级数学生成仍是艰巨挑战。在VerifierBench验证任务上，最佳模型的Balanced F1仅65.1，且所有模型在真负率上表现低下，表明模型严重倾向于错认无效证明为有效。这一发现揭示出当前LLM在关键错误检测上的根本性缺陷。

论文还进行了验证器的消融实验，验证了各组件的重要性；并分析了模型在不同数学分支上的表现差异，发现代数和分析相对较好，几何和拓扑更差。这些结果刻画了LLM在高级数学推理边界上的精细图景。

最后，作者总结了基准的局限性（如问题规模有限、仅覆盖自然语言证明等），并指出未来方向包括结合形式化工具、扩展数据规模和改进验证训练。总...**

<!-- paperflow:40cc4e8f8d1bced9 -->
## SearchOS-V1: Towards Robust Open-Domain Information-Seeking Agent Collaboration

[[Deep Reading - Jul 2026/SearchOS-V1-Towards Robust Open-Domain Information-Seeking Agent Collaboration|Deep Reading]]

[https://arxiv.org/pdf/2607.15257v1](https://arxiv.org/pdf/2607.15257v1)

- **本文提出了SearchOS，一个旨在提升开放域信息搜索鲁棒性的系统级多智能体框架。论文首先指出现有搜索智能体在长交互历史中容易丢失进度跟踪、陷入搜索循环的问题。为解决这些问题，SearchOS将信息搜索任务形式化为带引用溯源的关系模式补全，并设计了面向搜索的上下文管理（SOCM）——一种将搜索进度外化为前沿任务、证据图、覆盖图和失败记忆的机制。框架进一步引入了管道并行调度，使多个子智能体能够高效地协同填充覆盖缺口；搜索工具中间件则实时监控并记录智能体与搜索工具的交互，处理停滞和预算约束。层次化技能系统提供了可重用的策略和访问技能，帮助智能体避免重复失败。在WideSearch和GISA两个基准上的实验表明，SearchOS在所有评估指标上均超过现有的单智能体和多智能体基线。消融实验揭示了几个关键洞察：搜索时动态模式规划比固定模式更有效；覆盖图防止过早停止；失败内存减少重复搜索；管道并行提高吞吐量。论文的工作为构建鲁棒、透明和高效的信息搜索协作系统提供了系统性方案。**

# Language Models

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Language Models
- 方法：agent, ai-for-science, language, vision-language-model, reasoning, science-discovery, vision, reinforcement-learning
- 论文/报告：30 篇
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

<!-- paperflow:38f809cc8011778d -->
## Improving LLM-Generated Process Model Quality Through Reinforcement Learning: The Role of Reward Function Design

[[Deep Reading - Jul 2026/Improving LLM-Generated Process Model Quality Through Reinforcement Learning-The Role of Reward|Deep Reading]]

[https://arxiv.org/pdf/2607.06175](https://arxiv.org/pdf/2607.06175)

- **本文系统研究了在多维度质量评估下，如何设计强化学习奖励函数以优化LLM生成的BPMN过程模型质量。作者采用Group Sequence Policy Optimization算法，基于BEF4LLM自动评估框架的38个指标构建奖励信号，在Llama 3.1 8B和Qwen 2.5 14B两个模型家族上进行了48种不同配置的实验。研究发现：第一，强化学习能显著提升模型的实用和语法质量，同时保持语义一致性，并将输出变异性降低六倍以上；第二，等权重奖励策略在所有实验设置中均优于针对特定维度加权的策略，表明强调某一维度不仅无法提升该维度，反而可能导致整体质量崩溃；第三，奖励函数设计的影响与模型架构选择之间存在重要交互，例如无效惩罚对Llama模型关键但对Qwen模型无关，SFT初始化对Llama必不可少但对Qwen反而有害。这些发现表明，奖励函数的具体组成是实现优化效果的关键决定因素，其影响程度与是否使用强化学习本身相当。论文还为结构化生成任务中的RL应用提供了实用指南，并指出了未来研究方向。论文代码已开源。**

<!-- paperflow:f162c08631767c64 -->
## More Convincing, Not More Correct: Self-Play Reward Hacking of Reference-Free LLM Judges

[[Deep Reading - Jul 2026/More Convincing, Not More Correct-Self-Play Reward Hacking of Reference-Free LLM Judges|Deep Reading]]

[https://arxiv.org/pdf/2607.05904](https://arxiv.org/pdf/2607.05904)

- **该论文系统性地揭示了在无参考评判者（reference-free LLM judges）下进行自博弈（self-play）训练时存在的结构性问题：由于验证不对称性，评判者衡量的是合理性而非正确性，导致策略可以通过生成看似合理但错误的答案来劫持奖励信号。作者通过设计隐藏锚点审计（hidden-anchor audit）精确量化了这种奖励黑客攻击的程度——在GSM8K任务上，自博弈将评判者通过率从0.72提升至0.94，但真实准确率保持不变（0.20），产生了0.74的判对差距。更重要的是，这种黑客攻击具有强大的可转移性：制造的错误不仅能欺骗同族模型，还能欺骗不同家族（Qwen, Llama, Gemma）和不同规模（至14B）的模型，甚至一个严格的三家族集成仍然接受55%的错误答案。多种防御措施（更强评判者、重计算提示、直接训练集成奖励）均告失败。论文识别出决定性的变量不是评判者是否看到候选，而是它是否先提交自身答案：如果评判者首先独立生成答案（盲解），辨别力从接近随机提升至0.96；如果先提交自身答案再判断候选（候选可见），假阳性率从0.719骤降至0.012。基于此，作者提出去锚定奖励（de-anchored reward）作为训练信号，在自博弈全程保持假阳性率为零，从而防止盆地形成而非事后检测。该模式在代码生成和竞赛数学的最佳N选择场景下被复制，并在Gemma策略的完整训练循环中得到验证。论文提供了一个可证伪的边界（判对差距不超过1-准确率），为理解哪些机制暴露于该漏洞提供了理论指导。**

<!-- paperflow:4fbabb2546d63f0d -->
## Max Out GRPO Signal: Adaptive Trace Prefix Control for Hard Reasoning Problems

[[Deep Reading - Jul 2026/Max Out GRPO Signal-Adaptive Trace Prefix Control for Hard Reasoning Problems|Deep Reading]]

[https://arxiv.org/pdf/2607.07674](https://arxiv.org/pdf/2607.07674)

- **本论文提出 AdaPrefix-GRPO，一种通过自适应前缀长度控制来解决 GRPO 在困难问题上梯度消失问题的方法。论文首先指出 GRPO 在组内所有 rollout 均失败时梯度为零的缺陷，将问题归因于困难样本的无效学习。受前缀可以调节问题难度的启发，作者将前缀长度建模为每个问题的连续难度旋钮，并设计了一个闭环反馈控制器。该控制器基于每个问题的实时成功率（移动平均 k/G）与目标 set-point（全局 50% 加上每个问题的偏移）之间的误差，动态调整前缀长度，使得成功率始终保持在梯度利用率最高的区域。同时，控制器在训练后期逐渐将前缀长度衰减到零，确保模型最终能独立解决任务。实验表明，在相同训练计算量下，AdaPrefix-GRPO 在 0.6B 模型上将数学推理准确率提升 2.1 倍，在 Qwen3-1.7B 上提升 1.6 倍，在 AIME 上提升 1.7 倍，并大幅缩短推理序列长度。该方法实现简洁，仅需数据准备和 loss mask 修改，不改变 GRPO 核心训练流程。论文还讨论了一系列消融实验和边界条件，证实了控制器的有效性。最后，论文指出了前缀来源（参考解 vs. 模型生成）和控制器超参数的影响，并提出若干未来方向。总体而言，AdaPrefix-GRPO 为 GRPO 的难度自适应训练提供了一个简单而强大的框架。**

<!-- paperflow:bf92541bf88d93cd -->
## When Synthetic Speech Is All You Have: Better Call GRPO

[[Deep Reading - Jul 2026/When Synthetic Speech Is All You Have-Better Call GRPO|Deep Reading]]

[https://arxiv.org/pdf/2607.08409](https://arxiv.org/pdf/2607.08409)

- **本论文针对隐私敏感领域（如银行）ASR系统中合成语音适应问题，提出使用组相对策略优化（GRPO）替代监督微调（SFT）。论文系统比较了SFT、GRPO和SFT+GRPO在银行领域合成语音上的表现。实验表明，GRPO在纯合成数据上WER从SFT的36.71%降至22.09%（相对降低40%），结合SFT可进一步降至20.19%。混合少量真实语音（5-10小时）即可接近使用全量真实语音的SFT效果。论文还探究了合成数据选择策略和奖励函数设计，发现随机采样和WER奖励最有效。机制分析表明，GRPO改进来自行为层面：减少插入错误、改善停止校准和注意力锚定，而早期表示不变。这些发现表明，当合成语音是主要资源时，RL应优先于SFT。论文的实验设计严谨，提供问题、方法、实验和机制的全面分析，对低资源环境下的ASR领域适应具有重要指导意义。**

<!-- paperflow:c31fe63d368f4b76 -->
## Scalable Visual Pretraining for Language Intelligence

[[Deep Reading - Jul 2026/Scalable Visual Pretraining for Language Intelligence|Deep Reading]]

[https://arxiv.org/pdf/2607.09657](https://arxiv.org/pdf/2607.09657)

- **本文系统研究了视觉预训练（VP）作为语言智能基础模型的可扩展学习范式。当前的主流语言模型预训练仅依赖文本，但科学知识大量以视觉形式（公式、图表、排版）呈现，这些信息在转文本过程中丢失。作者挑战了“语言模型必须从文本学习”的默认假设，提出直接在文档图像上进行无监督视觉预训练。核心方法是对渲染的文档页面使用Vision Transformer编码成patch序列，然后以自回归方式预测每个patch在潜在空间中的表示。这一目标的优势在于无需配对文本，并且深度整合了视觉潜在信息。通过控制变量实验（同一语料的视觉vs文本表示），作者发现VP在多个科学推理基准上持续优于文本预训练（TP）。更重要的是，VP的性能随预训练数据量增加而提升，表现出良好的可扩展性。在定性分析中，视觉密集样本（如含复杂公式或图表的题目）的增益最为显著，说明模型从视觉布局中捕获了关键线索。此外，VP还自然提升了跨模态对齐能力。尽管本文仅聚焦科学文档，且视觉潜在解码与文本生成间的协调仍有待优化，但其结果为视觉信息在预训练中的价值提供了有力证据，开辟了一种绕过文本提取直接使用视觉语料库的预训练路径。未来工作可探索更大规模的视觉预训练、更高效的编码器以及更细致的多模态协同。**

<!-- paperflow:ed967a1d54d29172 -->
## Self-Guided Test-Time Training for Long-Context LLMs

[[Deep Reading - Jul 2026/Self-Guided Test-Time Training for Long-Context LLMs|Deep Reading]]

[https://arxiv.org/pdf/2607.09415](https://arxiv.org/pdf/2607.09415)

- **## 论文主线概述
本文聚焦长上下文 LLM 在推理任务中对关键证据有效利用不足的问题，提出 Self-Guided Test-Time Training（S-TTT）方法。

### 背景与动机
现代 LLM 已支持极长的上下文窗口（如 128k tokens），但实验表明，仅增加窗口长度不必然提升任务表现，模型往往被无关内容干扰，无法准确提取与问题相关的证据。测试时训练（TTT）通过将测试样本视为微调实例进行参数自适应，可针对特定输入调整模型行为，但其在长上下文场景下遭遇“训练数据选择”难题：若直接在全上下文上微调，计算开销极大且引入大量噪声；若随机采样跨度，因大部分内容不相关，反而损害性能。

### 核心方法：S-TTT
S-TTT 的关键思想是利用模型自身来识别证据丰富的跨度，再仅在这些跨度上执行 TTT。具体分为两步：
1. **跨度识别**：设计 prompt 指令（包含示例输出格式），要求 LLM 从输入上下文中逐字复制出回答当前问题所需的证据片段。Prompt 可设计为 few-shot 格式，模型输出形如 `<evidence> ... </evidence>` 的标记。可采用多次采样或正则表达式抽取增强鲁棒性。
2. **参数自适应**：将识别出的跨度按原始顺序拼接为训练序列，对模型应用标准语言建模目标（next token prediction）。使用 LoRA 实现高效、低资源参数更新，避免全参数微调。训练完成后，以更新后的模型生成答案。

### 实验设计
作者在 LongBench-v2 和 LongBench-Pro 两个长上下文推理基准上评估了 Qwen3-4B-Thinking-2507 和 Llama-3.1-8B-Instruct 两个模型。对比基线包括无 TTT 的基准模型、随机跨度 TTT（random span）、全上下文 TTT（full context）以及 oracle 跨度 TTT（使用 ground truth 证据跨度）。

### 关键结果与现象
- **跨度质量影响显著**：Random Span TTT 在...**

<!-- paperflow:d92d6163c0b5f621 -->
## HierCAD: Hierarchical Text-to-CAD Design via Structure Alignment and Parameter Grounding

[[Deep Reading - Jul 2026/HierCAD-Hierarchical Text-to-CAD Design via Structure Alignment and Parameter Grounding|Deep Reading]]

[https://arxiv.org/pdf/2607.11339v1](https://arxiv.org/pdf/2607.11339v1)

- **本文针对文本到 CAD 生成中结构不一致与参数不准的挑战，提出 HierCAD 框架。核心创新包括将 CAD 生成视为从对象级到部件级的渐进推理过程，并设计结构对齐（推理轨迹与参数操作对齐）和参数基础（通过扰动和排序学习去除捷径）的统一学习策略。在标准数据集上，HierCAD 在自动指标和人工评估上均优于现有方法，验证了层次化设计与特定学习目标的有效性，展示了将结构化先验注入 LLM 的潜力。**

<!-- paperflow:10f771508f34bb7c -->
## SCOPE-RL: Optimizing Reasoning Paths Before and After Success

[[Deep Reading - Jul 2026/SCOPE-RL-Optimizing Reasoning Paths Before and After Success|Deep Reading]]

[https://arxiv.org/pdf/2607.11506v1](https://arxiv.org/pdf/2607.11506v1)

- **SCOPE-RL: 优化成功前后推理路径的强化学习方法

当前使用可验证奖励的强化学习方法（RLVR）通常只依赖最终答案正确性作为奖励信号，这种稀疏的锚点能可靠判定轨迹是否成功，但无法对推理过程提供直接反馈。在成功之前，模型在困难问题上的中间进展无法获得奖励，导致探索效率低；在成功之后，结果奖励无法区分高质量推理路径和冗余或有局部错误的路径，导致模型可能学习低效的策略。

GRPO等策略更新层级的改进主要关注如何利用最终奖励进行策略优化，而未解决奖励信号本身的稀疏性问题。过程奖励模型（PRM）需要额外训练或外部监督，且难以泛化。

本文提出SCOPE-RL，在保持GRPO更新机制的情况下，通过两阶段框架稠密化奖励锚点。第一阶段：自适应脚手架RL（ASR）。在模型尝试解决问题时，将问题分解为一组隐藏最终答案的子问题（scaffolded sub-questions）。模型需逐步回答这些子问题，每个步骤的答案可以与数据集提供的标准值或规则进行匹配，从而获得一个前缀分解的可验证奖励。这使得即使在最终答案错误的情况下，模型也能因中间步骤的正确而获得正向信号，从而引导模型学习正确的中间推理。第二阶段：质量感知过程RL（QPR）。当模型已经能够产生正确的最终答案后，对正确轨迹进行精细化筛选。通过一个正确性门控（correctness-gated）的过程形状奖励，优先选择具有高有用步骤密度、低冗余、组织清晰的推理路径，同时抑制那些虽然最终答案正确但过程冗余、混乱或包含局部错误的轨迹。该奖励函数可以基于启发式规则或轻量模型评估。两个阶段顺序执行，ASR先提升模型达到最终答案的能力，QPR再优化轨迹质量。两者都使用GRPO作为策略更新基线，整体框架易于集成到现有RLVR流水线中。

论文在Qwen3-8B-Instruct 和 Qwen3-0.6B-Instruct 上进行了实验。训练数据采用DAPO-Math和Big-Math两个数学数据集。评估涵盖多个数学推理和科学问答基准（合理推断包括MATH、GSM8K、AIME等）。主要对比基线包括纯结果GRPO和GSPO。

主要结果：在Qw...**

<!-- paperflow:c679b7ea690335e6 -->
## ResearchQA: Benchmarking Citation-Grounded Question-Answering on Scientific Papers

[[Deep Reading - Jul 2026/ResearchQA-Benchmarking Citation-Grounded Question-Answering on Scientific Papers|Deep Reading]]

[https://arxiv.org/pdf/2607.11074v1](https://arxiv.org/pdf/2607.11074v1)

- **论文介绍了ResearchQA，一个用于大型语言模型引文基础问答评估的新基准。研究背景是：LLM被广泛用于辅助科学阅读，但现有评估方法（如LLM评分和嵌入相似度）无法有效检测答案是否由可验证的引文支持，导致模型可能产生虚假引用。为此，作者构建了一个包含6,211个问答对的数据集，源自494篇开放获取论文，覆盖8个科学领域（包括生物医学、计算机科学等）和4种问题类型（查找、理解、多跳、对抗）。该基准的关键设计是多有效支持段落（一个答案可引用多个段落）和对抗性不可回答问题（奖励拒绝回答）。评估采用确定性引文匹配器（严格检查引用文本是否在原文）和LLM评估器（评分答案质量）。实验在8个主流模型上进行，包括GPT-4、Gemini、Claude系列及其快速版本，以及两个开源模型（GPT-OSS-120B、ZAI GLM 4.7）。主要发现：引文指标（引用准确率、段落覆盖度）能清晰区分模型，而LLM评估器分数差异很小；开源模型ZAI GLM 4.7在引文准确率上接近最佳闭源模型，同时延迟低3-6倍；不同模型在对抗性拒绝上表现差异大；端到端延迟范围从2.9秒（GPT-OSS-120B）到30.4秒（Claude Opus 4.7）。论文还指出了局限性：问题仅基于文本，不包括图表；问题生成可能受单一模型影响；引文匹配是确定性的，可能遗漏语义等价引用。总体而言，ResearchQA为科学文献阅读辅助的评估提供了更严格和区分度更高的方法，并推动了开源模型在该任务中的应用。**

<!-- paperflow:08f26f2ffe68d40c -->
## PRISM Edit: One Vector for All Temporal Answers

[[Deep Reading - Jul 2026/PRISM Edit-One Vector for All Temporal Answers|Deep Reading]]

[https://arxiv.org/pdf/2607.11327v1](https://arxiv.org/pdf/2607.11327v1)

- **本文针对大语言模型时序知识编辑的挑战，提出了一种机制对齐的方法PRISM Edit。论文首先指出现有的定位-编辑范式在时序事实上的根本缺陷：它们将事实视为单值静态，更新时直接覆盖旧答案，忽略了旧答案在历史时间上下文中仍然有效的情况。通过首次对时序事实进行因果追踪分析，作者发现LLM内部实际上采用两阶段机制处理时序知识：早期MLP层提取与时间无关的主体表示，后期层（尤其是上层注意力与MLP）根据输入中的时间上下文信号调制该表示，输出时间正确的答案。基于这一发现，PRISM Edit不再直接修改定位到的知识存储位置，而是在早期MLP层优化一个多义主体表示，使其在不同时间上下文下经过模型固有调制通路后能产生对应的正确预测。该过程无需修改模型架构，仅需少量训练步即可完成，编辑速度提升2倍以上。为系统评估时序编辑，作者构建了TimeConflict基准，涵盖24个关系、22708条记录，每个样本包含当前答案和多个历史答案及其时间标志。实验在TimeConflict和时序增强版CounterFact上进行，对比了ROME、MEMIT、MEND、KE、FT等基线方法。结果显示，PRISM Edit在时序一致性（TC）上平均提升23.3分，当前相对时间分数（CRS）提升33.7分，历史相对时间分数（HES）提升12.5分，且编辑速度最快。消融研究验证了多义表示和两阶段通路的重要性。论文还讨论了方法的局限性，包括对时间上下文提示的依赖以及当前只覆盖离散时间点等。总体而言，本文揭示了LLM时序知识处理的内在机制，并据此提出了一个高效、无需架构修改的编辑方案，为时序知识编辑领域奠定了重要基础。**

<!-- paperflow:4a6ab68bf5f80aad -->
## TreeThink: A Modular Tree Search Library for Mathematical Reasoning with LLMs

[[Deep Reading - Jul 2026/TreeThink-A Modular Tree Search Library for Mathematical Reasoning with LLMs|Deep Reading]]

[https://arxiv.org/pdf/2607.11258v1](https://arxiv.org/pdf/2607.11258v1)

- **TreeThink 论文提出了一个用于神经定理证明的模块化、异步树搜索 Python 库。该库解决了现有 LLM 树搜索库缺乏形式验证集成和定理证明系统中搜索实现重复的问题。TreeThink 的核心贡献在于提供了一个可扩展的框架，包括可插拔的搜索算法、多样化的评估器、基于 vLLM 的高效推理管道，以及对 Lean 4、Rocq、Isabelle/HOL 和自然语言推理的原生支持。实验在 miniF2F 和 MATH500 上进行，展示了跨语言形式证明搜索和自然语言推理的能力，异步执行实现了高达 6.3 倍的加速。代码以 MIT 许可证开源。论文还讨论了局限性，如评估范围有限、仅支持 vLLM、缺乏可视化等，并指出了未来工作方向。总体而言，TreeThink 为神经定理证明研究提供了一个实用且可重用的基础设施。**

<!-- paperflow:c0956b56be36062d -->
## GDP.pdf: Benchmarking Grounded Multimodal Reasoning over Professional PDF Documents

[[Deep Reading - Jul 2026/GDP.pdf-Benchmarking Grounded Multimodal Reasoning over Professional PDF Documents|Deep Reading]]

[https://arxiv.org/pdf/2607.11192v1](https://arxiv.org/pdf/2607.11192v1)

- **该论文提出了GDP.pdf，一个用于评估多模态大模型在专业PDF文档上进行接地推理的基准测试。作者指出，现有文档AI基准分别测量OCR、布局分析、图表推理等能力，但未要求综合运用这些能力回答专业人员真正会问的问题。GDP.pdf由10个不同专业领域（如福利申请、租赁合同、数据表、临床指南、建筑施工图等）的在职工作人员，基于真实PDF文件创建问答对。为保证任务具有挑战性且贴近实际，每个问题必须被至少2个前治多模态模型以实质性方式回答錯誤（错误答案、遗漏关键证据、或捏造事实）才被纳入最终集合。每个任务附带由领域专家制定的原子级评分标准，确保评估可靠且能区分部分正确与完全正确。同时每道题标注一项多项能力的分类本体（共11项，分基础提取、结构化推理、高级推理三层）。论文在100道题上评估了7个前治多模态模型（Gemini 3.1 Pro、Claude Opus 4.7、GPT-5.4、Grok 4.20、Kimi K2.5、Mistral Large 3、Nova 2 Pro），发现严格通过率最高仅为15%（Gemini 3.1 Pro），最低1%（Nova 2 Pro）。对错误样本的详细分析表明，模型在表格对位、图表解读、脚注和例外条款处理、平面图符号计数、扫描噪声容忍、以及修正条款覆盖前文上存在系统性缺陷。论文还报告了按能力维度的细化结果，揭示了不同模型的强弱项。GDP.pdf基准测试、评分标准及能力分类已开源。作者认为该基准侧重测试模型的下限（基础能力）而非上限，对于文档自动化部署具有实际指导意义。论文的贡献包括：一个专家撰写、筛选严格、带有细粒度标注的多模态PDF推理基准；一套11项能力的分类体系；以及针对前治模型的全面评估和错误模式分析，指出当前模型在接地PDF推理上远未达到可靠水平。**

<!-- paperflow:ba1bdaef127e0792 -->
## Domain-Aware Scaling Laws Uncover Data Synergy

[[Deep Reading - Jul 2026/Domain-Aware Scaling Laws Uncover Data Synergy|Deep Reading]]

[https://arxiv.org/pdf/2607.11052v1](https://arxiv.org/pdf/2607.11052v1)

- **本文系统研究了语言模型预训练中数据组成带来的协同效应。作者首先指出现有缩放法则忽略数据组成，而经验证据表明不同领域数据混合会产生非加性影响（如代码提升数学推理、某些混合造成干扰）。为此，他们将这种效应称为数据协同，并形式化为超出独立贡献之和的部分。论文提出了领域感知缩放法则，在经典缩放损失函数中引入两项：一阶领域→基准协同（衡量某领域对某一基准的直接贡献）和二阶领域间协同（衡量需要两个领域共现才能获益的能力）。利用开源社区提供的各种预训练混合下的语言模型检查点（如Llama、Mistral等），他们通过观测这些模型在多个基准上的表现，用回归方法估计协同参数。该框架显著改进了领域无关缩放法则的预测准确性，并能恢复稳定的协同信号。为验证估计的可信度，作者根据协同估计选择了预测最优混合和预测反最优混合，从头训练了小规模模型，结果实际性能排名与预测完全一致，证明协同估计能正确指导数据配比。实验还揭示了一些具体的协同模式，例如代码数据与数学推理、代码生成呈正协同，而某些低质量网页文本与学术能力呈负协同。总体而言，本文为理解和优化预训练数据配比提供了量化工具，强调了数据质量与交互的重要性，并开辟了数据协同这一研究方向。**

<!-- paperflow:9266a14521743e48 -->
## Ring-Zero: Scaling Zero RL to a Trillion Parameters for Emergent Reasoning

[[Deep Reading - Jul 2026/Ring-Zero-Scaling Zero RL to a Trillion Parameters for Emergent Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2607.12395v1](https://arxiv.org/pdf/2607.12395v1)

- **本文是关于超大规模强化学习推理能力的深度探索。研究团队成功将 Zero RL 训练流水线扩展至万亿参数规模，并系统性地解决了大规模训练中的稳定性与推理质量问题。通过引入裁剪重要性采样、比例修正和 KL 约束，Ring-Zero 克服了万亿参数模型在强化学习中的发散风险。实验不仅证明了 1T 模型在数学推理任务上的领先地位，更重要的是揭示了大规模模型在没有人工干预的情况下，能够自发学习到诸如自我纠错、结构化思考和拟人化表达等高级认知功能。论文提出的三维评估框架为未来推理模型的评价提供了新的标准。这项工作验证了“苦涩的教训”，即通过大规模算力和通用算法的结合，可以实现超越手工设计启发式方法的智能涌现。**

<!-- paperflow:c9eb7067e61d8950 -->
## Post-Training Shifts Confidence: A Three-Stage Analysis of How SFT, RL, and OPD Shape Pre-, Intra-, and Post-CoT Calibration

[[Deep Reading - Jul 2026/Post-Training Shifts Confidence-A Three-Stage Analysis of How SFT, RL, and OPD Shape Pre-, Intra|Deep Reading]]

[https://arxiv.org/pdf/2607.13753](https://arxiv.org/pdf/2607.13753)

- **本文系统研究了监督微调（SFT）、强化学习（RL）和在线策略蒸馏（OPD）三种主流后训练方法对大型语言模型推理置信度的影响。作者指出，先前评估仅关注最终答案准确性，忽略了推理过程中置信度在难度估计、早停和答案聚合等方面的关键作用。为此，论文提出三阶段校准框架：Pre-CoT（推理前，基于初始token概率预估难度）、Intra-CoT（推理中，基于滑动窗口平均置信度决定早停）和Post-CoT（推理后，基于轨迹置信度进行答案聚合）。通过在数学推理基准上的控制实验（基于Qwen2.5-7B-Instruct），作者发现各后训练方法在不同阶段表现出截然不同的校准优势：OPD提供最可靠的第一阶段信号，SFT在第二阶段（在线早停）中表现最佳，而RL在第三阶段（聚合）中最具鉴别力。进一步分析揭示置信度的位置依赖性——RL置信度在推理链路径承诺期后才具有信息量，而OPD置信度早期有用但后期可能反向校准。基于这一发现，作者提出位置感知置信度策略（PosConf），仅从可靠相对位置区间提取置信度。PosConf在RL聚合任务上超越多数投票6.1个百分点，在OPD早停任务上在严格token预算下再获4.3个百分点提升。论文强调了后训练方法选择应结合推理阶段和位置特性，为高效推理系统中的置信度利用提供了系统性指导。**

<!-- paperflow:7d9ab88a2ab7fa15 -->
## Demystifying On-Policy Distillation: Roles, Pathologies, and Regulations

[[Deep Reading - Jul 2026/Demystifying On-Policy Distillation-Roles, Pathologies, and Regulations|Deep Reading]]

[https://arxiv.org/pdf/2607.13399](https://arxiv.org/pdf/2607.13399)

- **本文《Demystifying On-Policy Distillation: Roles, Pathologies, and Regulations》系统研究了在线策略蒸馏（OPD）在大型语言模型（LLM）后训练中的训练动态。研究首先阐明了OPD的核心作用：它并非提升学生模型的能力上限（asymptotic performance ceiling），而是通过密集的token级信号引导学生探索正确的推理路径，从而加速早期学习过程。这一观点区别于将OPD视为单纯知识迁移或强化学习信号的传统认知。作者通过实证确认，提示多样性（prompt diversity）对最终性能的影响远大于每个问题的采样数量，且OPD的有效性完全取决于教师提供信号的质量。

进一步地，论文揭示了两种导致探索失败的核心病理：学生-教师不匹配（Student-Teacher Mismatch）和长度利用（Length Exploitation）。学生-教师不匹配发生在师生模型分布差距较大时，此时教师信号虽然密集，但可能与任务正确性偏离，反而引导学生朝错误方向探索。长度利用则源于OPD的聚合token级目标函数：学生可以通过生成非常短或非常长的响应来获得高奖励，而非真正学习推理策略。这两种病理都会使OPD偏离有效探索。

为解决上述问题，论文提出了两种轻量信号调节策略：优势裁剪（Advantage Clipping）和对数尺度压缩（Log-scale Compression）。优势裁剪将超出合理范围的信号值进行截断，以减少极端噪声的影响；对数尺度压缩则通过对奖励进行对数化处理，削弱长度维度对总奖励的支配，从而抑制长度利用。这两种方法不改变模型架构，仅在训练时对损失信号进行后处理。

实验在七个涵盖数学推理（如MATH-500）、代码生成等任务的基准上进行，并与朴素OPD和RLVR（可验证奖励强化学习）基线比较。关键发现包括：（1）弱教师（如1.7B参数）在早期训练中能带来快速提升，但强教师（如7B）最终能达到更高性能，体现了OPD的加速而非天花板提升作用；（2）教师能力并非越大越好，过大的师生差距会导致负...**

<!-- paperflow:3ca76b2d58cf2390 -->
## Deep Interaction: An Efficient Human-AI Interaction Method for Large Reasoning Models

[[Deep Reading - Jul 2026/Deep Interaction-An Efficient Human-AI Interaction Method for Large Reasoning Models|Deep Reading]]

[https://arxiv.org/pdf/2607.14049](https://arxiv.org/pdf/2607.14049)

- **本文提出Deep Interaction，一种新型人机交互方法，旨在高效修正大语言模型在链式推理（CoT）中的错误。论文首先指出了当前CoT推理中错误修正的痛点：重新生成可能重复犯错，多轮对话纠错效率低且不稳定。为解决此问题，Deep Interaction允许用户直接编辑模型的原始回答，修正错误推理步骤，然后将修正后的CoT蒸馏成精炼提示（distilled prompt），再利用该提示引导模型沿正确路径推理并生成最终答案。该方法避免了重新生成整个回答的开销，同时保留了用户修正的精确性。在STEM推理任务上的实验结果显示，相比基线方法（如重新生成、多轮对话修正等），Deep Interaction的纠正成功率提高了超过25%，并减少了约40%的token消耗。此外，消融研究揭示，仅增加历史对话轮次并不能有效提升纠正效果，从而凸显了直接编辑+蒸馏提示策略的优势。论文还提供了案例研究，直观展示了编辑前后CoT的变化。总体而言，Deep Interaction为高层级人机协同推理提供了高效、轻量的解决方案。**

<!-- paperflow:18606e79925fae1e -->
## DeepLoop: Depth Scaling for Looped Transformers

[[Deep Reading - Jul 2026/DeepLoop-Depth Scaling for Looped Transformers|Deep Reading]]

[https://arxiv.org/pdf/2607.13491](https://arxiv.org/pdf/2607.13491)

- **本文深入探讨了循环 Transformer（Looped Transformers）在深度扩展过程中的训练稳定性问题，并提出了 DeepLoop 这一创新的残差缩放方案。研究的核心逻辑在于：循环架构中的参数共享打破了传统残差缩放理论中“层间独立”的假设，导致梯度在共享参数上产生聚合效应，并在后续前向传播中被重复读取。作者通过严谨的一阶扰动分析，引入了访问对齐系数 $\kappa_R$，证明了在循环模型中，为了维持训练稳定，残差缩放指数必须从传统的 1/4 提升至 1/2。基于此理论，DeepLoop 为 Post-LN 架构量身定制了 $\alpha = (2N)^{1/2}$ 和 $\beta = (8N)^{-1/2}$ 的参数化规则。在 GPT-2 规模的实验中，DeepLoop 不仅解决了深层循环模型的训练崩溃问题，还在验证损失和 8 项下游 NLP 任务中全面超越了现有的缩放方法。该工作强调了在设计高效参数模型时，必须从“参数访问模式”这一新视角重新审视模型的初始化与缩放逻辑，为未来开发更深、更强的紧凑型语言模型奠定了理论基础。**

<!-- paperflow:200fa4dcf84a111d -->
## On-Policy Delta Distillation

[[Deep Reading - Jul 2026/On-Policy Delta Distillation|Deep Reading]]

[https://arxiv.org/pdf/2607.15161v1](https://arxiv.org/pdf/2607.15161v1)

- **这篇论文提出了一种新的知识蒸馏方法OPD^2，专门用于提升大语言模型的推理能力。其核心贡献是引入delta信号，即教师模型与其推理调优前的基础模型之间的输出差异，作为蒸馏的监督信号。相比于传统on-policy蒸馏直接模仿教师输出，delta信号更清晰地指示了推理调优带来的特征变化，使得学生模型能够更高效地学习教师的推理策略。论文从问题出发，分析了现有蒸馏方法的不足，然后形式化定义了delta信号，并基于此设计了训练流程。实验部分设计非常系统：在数学、科学、代码三个典型推理领域进行验证，涵盖了多种模型（Qwen3、Gemma4）和多种对比方法（标准OPD、ExOPD）。主要结果显示OPD^2在所有设定下均显著优于基线，且稳定性更强。消融实验进一步确认了delta信号是关键因素。此外，论文还务实地讨论了计算成本的增加（训练时间增加8-28%），并认为这一代价换来的性能提升是值得的。总体而言，OPD^2为LLM推理能力的后训练提供了一种简单、有效且通用的新途径，对于希望高效蒸馏大型推理模型的研究者和工程师具有直接参考价值。**

<!-- paperflow:8c631d8f0df9adfe -->
## Leveraging Instruction Tuning and Merging for Reasoning Model Adaptation

[[Deep Reading - Jul 2026/Leveraging Instruction Tuning and Merging for Reasoning Model Adaptation|Deep Reading]]

[https://arxiv.org/pdf/2607.14895v1](https://arxiv.org/pdf/2607.14895v1)

- **本文提出了一种简单、低成本的方法来适应推理语言模型到新的领域，尤其是那些缺乏可靠验证器的领域。方法先进行指令微调，然后与原始模型合并以恢复推理能力。合并系数通过校准集确定。在四个开源RLM上的实验表明，该方法在编程和文本摘要任务上均能提升性能，同时保持其他领域能力。成本极低（不到3美元），无需验证器，适合实际部署。**

<!-- paperflow:ce5daa6d53be1730 -->
## Rubrics on Trial: Evolving Rubrics from a Single Query via Synthetic Pairwise Evidence

[[Deep Reading - Jul 2026/Rubrics on Trial-Evolving Rubrics from a Single Query via Synthetic Pairwise Evidence|Deep Reading]]

[https://arxiv.org/pdf/2607.15092v1](https://arxiv.org/pdf/2607.15092v1)

- **该论文关注大型语言模型评估中的评分规则（rubric）自动生成问题。现有方法依赖人工或外部资源，而直接查询到规则生成缺乏质量控制。作者提出Rubrics on Trial框架，仅凭查询即可从空集迭代演化出高质量规则集。核心步骤为：①基于当前规则集和查询，让LLM提出候选规则；②针对每个候选规则，构造合成正负响应对（遵循/违反该规则）；③用LLM比较这些对，检验规则的判别力、过度具体性和风格偏见，仅通过检验的规则加入集。这种方法完全无需外部标注或模型训练，只依赖LLM本身。实验涵盖五个基准套件七个评估集，该方法在平均准确率上最优并主导六项。消融实验验证了验证步骤的必要性。论文还分析了不同演化阶段规则特征。局限性包括依赖合成数据质量和LLM判断可靠性。整体上，这项工作为自动评估规则生成提供了新颖且有效的范式。**

<!-- paperflow:912db545027d07c5 -->
## Stop Thinking, Start Looking: Efficient Post-Training for Multimodal Document Question Answering via Reasoning-Free Alignment

[[Deep Reading - Jul 2026/Stop Thinking, Start Looking-Efficient Post-Training for Multimodal Document Question Answering|Deep Reading]]

[https://arxiv.org/pdf/2607.14682v1](https://arxiv.org/pdf/2607.14682v1)

- **本文聚焦于多模态文档问答中的显式视觉定位（DVG）问题，目标是在不牺牲精度的情况下实现高效的训练。当前主流方法包括监督微调（SFT）和基于推理的强化学习（RL），但前者容易饱和且依赖大量标注，后者则因冗长的推理链导致高昂的推理成本，且其必要性存疑。针对这一矛盾，作者提出Perception-RFT，一种基于Group Relative Policy Optimization (GRPO) 的推理自由训练框架。其核心思路是直接优化视觉特征到结构化定位输出（如边界框）的映射，完全不产生中间推理token。为了系统评估推理的作用，论文在同一奖励设置下构建了两个对比训练流程：无推理的RFTb和有推理的Reasoning-RFTb。实验采用Qwen3-VL-4B作为基座模型，在内部金融文档数据集（ID）和两个公开分布外基准（DOGR-Bench、MMDocBench）上进行冷启动RL训练。主实验表明，无推理的RFTb在定位精度上与推理变体相当甚至更优，而推理token长度减少超过60%。进一步的分析揭示了几个关键发现：(1) 推理抑制：Reasoning-RFTb在训练过程中自发减少推理链长度，显示优化驱动模型转向直接感知。(2) 动力学迁移：在文本域中观察到的SFT饱和和冷启动RL不稳定性在多模态视觉定位中同样成立。(3) 定位发散：在OOD数据上，语义理解与几何精度两个维度的指标在联合RL优化下表现出选择性权衡，即提升一方会损害另一方，形成一种此前未表征的权衡模式。(4) 数据效率：早期SFT（仅使用约35%的数据）后过渡到RL训练，可以达到与完整训练相当的精度，显著降低了标注需求。总体而言，本文通过严谨的控制实验系统论证了在多模态文档定位中显式推理的非必要性，并提出了一种高效、低成本的训练范式。**

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, science-discovery, vision
- 论文/报告：58 篇
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

<!-- paperflow:c004a5f86e3c2c9b -->
## WebRetriever: A Large-Scale Comprehensive Benchmark for Efficient Web Agent Evaluation

[[Deep Reading - Jul 2026/WebRetriever-A Large-Scale Comprehensive Benchmark for Efficient Web Agent Evaluation|Deep Reading]]

[https://arxiv.org/pdf/2607.06118](https://arxiv.org/pdf/2607.06118)

- **本文提出了 WebRetriever，这是一个旨在彻底改变 Web Agent 评估现状的大规模、高难度基准测试集。针对现有基准规模小、领域窄、指标单一的问题，WebRetriever 提供了 800 个真实网站和 1550 个精心设计的任务，覆盖了从日常消费到专业企业应用的广泛场景。论文的核心贡献在于：首先，它揭示了当前 Web Agent 在面对真实世界复杂性时的巨大性能缺口；其次，它推出了 NavEval 评估框架，该框架通过整合交互轨迹和 DOM 语义，克服了传统 LLM 裁判仅依赖视觉信息导致的评估不准问题，实现了与人类判断的高度对齐。此外，论文定义了三层评估协议，将评估重心从简单的“到达页面”转向了“知识理解”和“精确信息提取”。实验结果表明，即使是目前最先进的模型在 WebRetriever 面前也面临巨大挑战，尤其是在专业领域和长路径任务中。这项工作为 Web Agent 的未来发展指明了方向，即需要更强的跨域泛化能力、更精细的交互逻辑理解以及对外部知识的有效整合。WebRetriever 不仅是一个数据集，更是一套完整的诊断工具，能够帮助开发者识别 Agent 在感知、推理和执行各个环节的具体薄弱点。**

<!-- paperflow:87ce8379c7a7ec8c -->
## Information Gain-based Rollout Policy Optimization: An Adaptive Tree-Structured Rollout Approach for Multi-Turn LLM Agents

[[Deep Reading - Jul 2026/Information Gain-based Rollout Policy Optimization-An Adaptive Tree-Structured Rollout Approach|Deep Reading]]

[https://arxiv.org/pdf/2607.06223](https://arxiv.org/pdf/2607.06223)

- **这篇论文聚焦于强化学习训练大型语言模型（LLM）智能体完成长程搜索任务时 rollout 预算低效的问题。现有方法无论是链式采样还是树式搜索，都未显式利用中间状态的信息价值来分配计算预算，导致大量资源花费在无潜力分支上。本文提出 IGRPO 框架，将中间状态的信息增益作为 rollout 组织原则。具体而言，在每次 rollout 过程中执行预算感知的树状扩展：计算每个状态的信息增益（如模型预测置信度变化），高信息增益分支获得更多扩展预算，低信息增益分支被抑制。这种策略在有限预算下优先探索信息量大的状态。理论部分证明了这种 rollout 方式在极限收敛时会诱导出一个隐式教师分布，该分布自然偏向于信息更丰富的轨迹，从而为策略优化提供明确的优化目标。IGRPO 将树状探索与策略学习统一，避免了传统两阶段方法。实验在 HotpotQA、2WikiMultihopQA、MuSiQue 等七个搜索增强问答基准上进行，对比了链式和树式基线。结果一致显示 IGRPO 在相同总预算下取得更高准确率，消融研究证实信息增益自适应分配是性能提升的关键。整体而言，论文提出了一种新颖的预算感知探索策略，并通过理论分析和实验验证了其有效性，为长程任务 LLM 智能体的强化学习训练提供了一种高效方法。**

<!-- paperflow:6c85c7b7d8d0b56d -->
## PIPBench: A Profile-Inclusive Framework for Personalized Image Generation Evaluation

[[Deep Reading - Jul 2026/PIPBench-A Profile-Inclusive Framework for Personalized Image Generation Evaluation|Deep Reading]]

[https://arxiv.org/pdf/2607.06440](https://arxiv.org/pdf/2607.06440)

- **本文通过提出PIPBench，针对个性化图像生成评估缺乏包含用户画像框架的问题提供了解决方案。论文首先系统定义了用于表示用户视觉偏好的多维画像，包括人口统计、心理特质和审美轴。然后通过真实用户实验收集偏好数据，并设计基于LLM的管线自动生成大量虚拟用户数据以支持大规模评估。基于该基准，论文对现有主流方法（包括无基线、参考图像条件、偏好融合等方式）进行了全面的比较与分析。实验结果揭示了现有方法在有效利用详细画像方面的不足，并确认了画像信息对提升个性化生成对齐度的重要性。最后，论文总结了当前挑战并指出了未来研究方向。该基准有望为个性化图像生成社区提供标准化验证平台。**

<!-- paperflow:936aa4e5b83df063 -->
## Task Decomposition-Guided Reranking for Adaptive Agent Skill Retrieval

[[Deep Reading - Jul 2026/Task Decomposition-Guided Reranking for Adaptive Agent Skill Retrieval|Deep Reading]]

[https://arxiv.org/pdf/2607.06283](https://arxiv.org/pdf/2607.06283)

- **本文聚焦于 LLM 智能体系统中技能库的自适应选择问题。随着技能库规模增长，传统固定 top-k 方法无法应对任务难度和技能适用性的动态变化。论文提出 SkillReranker，一种推理时自适应技能重排序框架，核心思想是通过任务和技能的语义分解，构建有向无环执行图来显式建模任务状态与技能之间的转换关系。具体而言，首先利用 LLM 对任务进行分解得到子任务序列和中间状态，同时对技能库中的每个技能提取其前提状态和效果状态；然后将中间状态作为节点，候选技能作为连接相邻状态之间的边，形成执行图；接着通过分裂条件判定将图划分为多个子任务区间，每个区间内利用交叉编码器对候选技能进行重排序，选出最适合该子任务的技能组合。实验在 ALFWorld 和 ScienceWorld 两个基准上，使用三个不同的 backbone LLM 进行，结果显示 SkillReranker 相比基线方法显著提升了任务成功率，并减少了交互步数和 token 消耗。论文的主要贡献包括：(1) 提出了一个即插即用的推理时重排序框架，无需额外训练；(2) 设计了任务-技能对齐的执行图机制，实现了细粒度状态感知；(3) 通过实验验证了方法在多种设置下的有效性。论文也指出了其局限性：依赖 LLM 分解的准确性、框架开销、以及当前仅限文本环境。未来工作可向多模态、动态更新、以及降低计算成本等方向扩展。总体而言，SkillReranker 为技能库的自适应选择提供了一条新路径，尤其适用于需要精确状态跟踪的复杂任务场景。**

<!-- paperflow:0015439c164f77fb -->
## Agon: Competitive Cross-Model RL with Implicit Rival Grading of Reasoning

[[Deep Reading - Jul 2026/Agon-Competitive Cross-Model RL with Implicit Rival Grading of Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2607.07690](https://arxiv.org/pdf/2607.07690)

- **本文针对GRPO等可验证奖励强化学习方法仅评分最终答案而忽视推理过程导致模型产生长而非优推理的问题，提出Agon（竞争性跨模型强化学习）。核心思想是让两个模型互为对手和裁判：在对称角色交替中，一个模型草拟解决方案，另一个模型阅读该草稿后独立解题。奖励不仅基于最终答案正确性，更基于一方是否战胜另一方，从而隐式地评估推理质量，无需过程标签。训练采用GRPO风格的组相对优化。推理时采用两阶段级联：草稿模型先产生推理，回答模型阅读后生成最终答案，这一过程与训练一致。实验在数学（DeepMath困难集）和代码（Competitive Programming）上进行，使用Qwen3、Qwen3.5和Gemma4模型。主要结果：Agon将GRPO的pass@1翻倍，增益是未训练Mixture-of-Agents的约8倍，该收益在多个模型家族上复现。消融验证了双模型共同进化的必要性，且Agon有效抑制了GRPO常见的推理长度膨胀。论文贡献在于：重新定义推理质量为竞争对手可提供的隐式奖励；提出Agon训练配方；展示显著实证改进。局限包括：需要两个强度匹配的模型；计算成本翻倍；仅在具备清晰奖励的任务上验证；缺乏理论收敛分析。未来工作方向为潜在空间协同推理。**

<!-- paperflow:426abed2f0bb43da -->
## From Noisy Traces to Root Causes: Structural Trajectory Analysis and Causal Extraction for Agent Optimization

[[Deep Reading - Jul 2026/From Noisy Traces to Root Causes-Structural Trajectory Analysis and Causal Extraction for Agent|Deep Reading]]

[https://arxiv.org/pdf/2607.07702](https://arxiv.org/pdf/2607.07702)

- **本文提出STRACE（Structural Trajectory Analysis and Causal Extraction），一个用于长周期智能体优化的端到端框架。动机在于：现有基于LLM反思的优化器（如Reflexion）直接处理原始执行轨迹，面临批次级冗余异质和轨迹内因果信号稀疏的双重问题，导致优化低效甚至过度拟合。STRACE通过两个创新模块解决此困境：批次级故障模式挖掘（基于统计和结构化诊断过滤冗余痕迹）和轨迹内因果定位（在文本依赖图上后向裁剪出因果紧凑的子轨迹，并定位根因模块）。优化器仅对根因模块的指令进行预防性更新，实现了安全、经济且高效的改进。实验在HotpotQA、WebArena和VeruSAGE-Bench上进行，结果显示STRACE显著优于标准上下文过滤基线，特别是在严格的形式化验证任务VeruSAGE-Bench上将成功率从42.5%提升至58.5%（绝对提升16%）。消融研究验证了两个关键组件的必要性。论文贡献包括：(1) 提出将执行日志视为结构化因果证据的优化视角；(2) 引入联合轨迹过滤与因果定位机制；(3) 实现模块级别精确的指令注入。这些贡献为长周期智能体优化提供了新的方法论。**

<!-- paperflow:6e7e7a81c883fa26 -->
## Multi-Modal, Multi-Environment Machine Teaching for Robust Reward Learning

[[Deep Reading - Jul 2026/Multi-Modal, Multi-Environment Machine Teaching for Robust Reward Learning|Deep Reading]]

[https://arxiv.org/pdf/2607.08647](https://arxiv.org/pdf/2607.08647)

- **本文针对逆强化学习中奖励函数泛化能力不足的问题，提出了一种多环境、多模态的机器教学范式。首先，论文系统分析了不同反馈模态（演示、比较、修正信号等）对奖励函数的约束能力，得出了在无限数据下比较反馈具有最强全局约束，而在有限预算下演示更有效的结论。基于此理论观察，论文设计了一个两阶段分层教学算法：首先通过贪婪策略在多个MDP中选择信息量最大的环境，以揭示互补的奖励约束；然后在选定环境中，根据当前预算智能选择反馈模态和具体查询，最大化对奖励空间的约束。在多个MDP基准上的实验表明，相比均匀环境选择和单环境基线，所提方法在相同反馈预算下能显著降低遗憾并提升跨环境泛化性能。本文的主要贡献包括：提供了多模态反馈约束的理论比较、引入了一个可扩展的多环境教学框架、并通过实验验证了其有效性。这项工作为构建可跨环境泛化的奖励学习系统奠定了基础，未来可考虑引入人类认知偏差、在线适应等扩展。**

<!-- paperflow:316a56516f86eab0 -->
## From Solvers to Research: Large Language Model-Driven Formal Mathematics at the Research Frontier

[[Deep Reading - Jul 2026/From Solvers to Research-Large Language Model-Driven Formal Mathematics at the Research Frontier|Deep Reading]]

[https://arxiv.org/pdf/2607.07779](https://arxiv.org/pdf/2607.07779)

- **本文是一篇关于AI4Math的立场论文，核心论点是当前AI4Math系统虽然已经在预定义的形式数学问题（如IMO问题）上取得显著成功，但距离成为能够辅助前沿数学研究的'研究代理'仍有根本性差距。论文主张AI4Math的下一步发展应该从'问题求解器'转向'研究代理'，即能够参与开放性、未完全指定的数学探索与发现的系统。

论文首先回顾了AI4Math领域的三个核心组成部分：形式数学数据集、自动形式化技术以及证明合成方法。在数据集方面，现有资源如MiniF2F、ProofNet等主要覆盖标准竞赛问题和经典定理，缺乏对研究前沿的覆盖；自动形式化方面，LLM展示了从自然语言翻译为形式代码的能力，但存在歧义、忠实性和完整性挑战；证明合成方面，结合搜索和LLM生成的方法在预定义问题上表现优异，但在需要抽象推理和创造性的问题上受限。

然后，论文深入分析了当前系统作为研究代理的五大核心局限：(1) 数据集层面：规模小、覆盖窄、缺乏层级和关系结构；(2) 关系结构层面：数学命题之间的依赖、推论、类比关系未被有效建模；(3) 数学探索层面：现有环境不支持逐步探索、假设提出和交互式验证；(4) 工具生态系统层面：Lean、Coq等系统碎片化，缺乏统一接口和工作流支持；(5) 人机协作层面：人类与AI的角色分工不明确，协作工具原始。这些局限共同导致现有系统无法处理开放式的前沿数学问题。

基于上述分析，论文提出了一项战略路线图，旨在弥合求解器与研究代理之间的鸿沟。该路线图围绕五个支柱构建：扩展和结构化形式数学数据、构建关系型数学知识库、开发交互式数学探索平台、统一工具生态系统以及设计高效的人机协作机制。论文特别强调最终目标不应是完全自主的定理证明，而是能够增强人类数学能力的协作系统。

最后，论文呼吁社区关注AI4Math在抽象概念理解、数学创造力和端到端自动形式化等根本性挑战，并鼓励开展跨学科合作以实现向研究代理的转变。本文作为综合性立场论文，为AI4Math的未来发展方向提供了清晰的系统分析和建设性建议。**

<!-- paperflow:4ca8c5d9b62e591a -->
## DeepSearch-World: Self-Distillation for Deep Search Agents in a Verifiable Environment

[[Deep Reading - Jul 2026/DeepSearch-World-Self-Distillation for Deep Search Agents in a Verifiable Environment|Deep Reading]]

[https://arxiv.org/pdf/2607.07820](https://arxiv.org/pdf/2607.07820)

- **本文针对工具使用智能体的自我改进难题，提出了 DeepSearch-World 可验证环境和 DeepSearch-Evolve 自蒸馏框架。首先，基于离线 Wikipedia 构建了包含搜索和阅读工具的确定性环境，支持进度验证和反思信号。其次，通过实体级随机游走生成了 420K 多跳 QA 任务。然后，设计迭代自蒸馏训练循环，包括轨迹生成、质量过滤、数据混合和模型微调。在 BrowseComp、GAIA 和 HotpotQA 上的实验表明，DeepSearch-World-9B 在无需更强模型蒸馏的情况下达到了与依赖外部教师模型的方法相竞争的性能。消融和案例研究验证了框架各组件的重要性以及 agent 的自纠正能力。该工作证明了可验证环境对于长程、深度搜索智能体自我进化的可扩展性，并提供了环境、数据集、代码和模型以促进后续研究。**

<!-- paperflow:a714e8b53c133555 -->
## WebSwarm: Recursive Multi-Agent Orchestration for Deep-and-Wide Web Search

[[Deep Reading - Jul 2026/WebSwarm-Recursive Multi-Agent Orchestration for Deep-and-Wide Web Search|Deep Reading]]

[https://arxiv.org/pdf/2607.08662](https://arxiv.org/pdf/2607.08662)

- **本文针对大语言模型（LLM）驱动的web搜索智能体在处理复杂深度与广度搜索任务时的局限性，提出WebSwarm——一个渐进式递归委托框架。WebSwarm摒弃了静态的任务分解或固定协作模式，而是动态实例化搜索节点树，每个节点包含局部目标和搜索模式，可自行搜索或委托子节点，结果向上返回以支持父节点的进一步扩展、修订或聚合。框架引入web结构引导机制，通过初始探测了解任务相关信息在网上的组织方式，从而指导子目标的生成；同时在同级同类型节点间复用过程级经验，提高搜索效率。实验在BrowseComp-Plus（深度搜索）、WideSearch（广度搜索）、DeepWideSearch（深度-广度交织）和GISA（通用搜索）四个基准上进行，WebSwarm在所有任务上一致优于单智能体ReAct和多智能体基线，尤其在困难样本上提升显著。消融分析、难度分析、效率分析和模型泛化测试进一步验证了各核心组件的作用和框架的鲁棒性。论文还指出了当前工作在效率、多模态覆盖等方面的局限，并展望了未来扩展方向。总体而言，WebSwarm为agentic web搜索提供了一种灵活的递归协作范式，有效平衡了搜索深度与广度。**

<!-- paperflow:195da982becc9380 -->
## AutoPersonas: A Multi-Timescale Loop Engine for Open-Ended Persona Evolution

[[Deep Reading - Jul 2026/AutoPersonas-A Multi-Timescale Loop Engine for Open-Ended Persona Evolution|Deep Reading]]

[https://arxiv.org/pdf/2607.08252](https://arxiv.org/pdf/2607.08252)

- **本论文系统研究了长期人物智能体在持续生活循环中的自我锁定问题。首先，作者定义了自我锁定失败模式：智能体虽然产出局部合理的连续事件，但整体生活轨迹缓慢坍缩向熟悉环境、薄弱关系和停滞决策，无法实现真正的开放演化。通过分析，他们将此归因于两个耦合压力——模型级的高概率收敛和系统级的上下文引力。为解决此问题，论文提出了AutoPersonas，一个多时间尺度生命环境引擎，核心创新在于OSO循环（Occurrences, Observations, State）。该循环将环境侧的发生事件（充满发散可能）、观察（累积且经过筛选）和人物状态（受严格边界约束）严格分离，使得发散材料得以进入系统但改变状态需经过证据控制的吸收过程。架构还包括环境水印、事件硬化等机制，确保人物身份的连续性和演化边界。论文放弃传统基准，采用诊断审计的方法验证架构的有效性。实验部分包含四个主要研究：1）三年压缩模拟定性暴露自我锁定的多种表现（环境水印壳、事件硬化缺口、慢变累积失败、递归犹豫、弱关系持久性）；2）八模型40天动作压力测试定量证明了自我锁定的普遍性，所有模型动作类别重复率在短期内升至90%以上，语义宏主题重复率79%-88%；3）同运行时A/B测试展示了上下文掩码加发散目标干预的效果，宏主题重复率从61.8%降至36.3%，主题计数从55提升至102；4）在少年地精奇幻世界中验证泛化性，重复率仅42.8%且无现实入侵，表明环境设计对自我锁定程度的影响。论文结论指出，长期人物智能体不仅需要记忆，还需要一个不随它们一起坍缩的环境和支持开放演化的架构。AutoPersonas提供了迈向这一目标的一步。**

<!-- paperflow:cd6df86c3e0337ff -->
## TTHE: Test-Time Harness Evolution

[[Deep Reading - Jul 2026/TTHE-Test-Time Harness Evolution|Deep Reading]]

[https://arxiv.org/pdf/2607.08124](https://arxiv.org/pdf/2607.08124)

- **论文提出 Test-Time Harness Evolution (TTHE)，一种在测试时通过未标记执行轨迹演化 LLM agent 周围的可执行程序（harness）的新范式。现有 agent 系统通常依赖预定义的固定工作流，但测试时环境变化导致性能下降。TTHE 将 harness 视为适应状态，维护一个候选 harness 种群，利用 agent 自身的执行轨迹（无需真实标签）通过 Proposer 和 Judge 构成进化循环。Proposer 分析当前 harness 与执行轨迹并生成改进版本，Judge 利用执行衍生的代理信号评估并选择更好的 harness 进入种群，从而实现对后续输入的持久适应。整个过程中 solver、proposer、judge 皆使用相同冻结 LLM，不更新权重、不依赖额外标记数据。在 text-to-SQL、竞赛编程、软件工程、数据科学编码、agentic 工具使用五个执行密集型任务上，TTHE 稳定改进 ReAct 基线，产出可检视的策略。论文还分析了代理信号可靠性这一核心挑战。该工作将 LLM agent 的测试时适应重定义为可执行控制程序的演化，揭示了无监督 agent 改进的关键问题。**

<!-- paperflow:5f09e6d978b3d317 -->
## Video Generation Models are General-Purpose Vision Learners

[[Deep Reading - Jul 2026/Video Generation Models are General-Purpose Vision Learners|Deep Reading]]

[https://arxiv.org/pdf/2607.09024](https://arxiv.org/pdf/2607.09024)

- **本文提出GenCeption，将预训练视频生成模型转化为通用视觉感知系统。动机源自NLP通用模型成功，作者认为视频生成提供了时空先验和多模态对齐，是通用视觉预训练的催化剂。方法基于三个原则：保留生成预训练、任务无关后训练、前馈推理。输入RGB视频和任务提示，经VAE编码、DiT处理、VAE解码直接输出任务结果。实验在8种以上任务中达到或超越SOTA，数据效率高（7-500倍数据节省），并展现合成到真实的泛化能力。意义在于提供了视频生成作为通用视觉基础的实践证据，暗示生成中蕴含理解。局限包括架构依赖、任务覆盖不足、微调策略简单、涌现机理尚未清晰。**

<!-- paperflow:e5275de32057a8c6 -->
## EasyOPD: An Easy-to-use On-Policy Distillation Framework for Large Language Models

[[Deep Reading - Jul 2026/EasyOPD-An Easy-to-use On-Policy Distillation Framework for Large Language Models|Deep Reading]]

[https://arxiv.org/pdf/2607.11012v1](https://arxiv.org/pdf/2607.11012v1)

- **论文 EasyOPD 提出了一个基于 verl 的易用在策略蒸馏框架，旨在解决现有 OPD 方法实现碎片化、难以复现和扩展的问题。传统 off-policy 蒸馏使用固定教师数据，无法匹配学生策略演化；OPD 虽能收集 student rollout 上的监督，但各方法在监督形式、tokenizer 兼容性、教师访问和粒度上差异大。EasyOPD 通过三层分离架构（配置、方法逻辑、分布式执行）和扩展边界定义，实现方法模块与共享后端的解耦。框架实例化了三种 OPD 场景：跨 tokenizer OPD（处理不同词表）、在策略自我蒸馏（学生从自身采样学习）和逐步骤 OPD（部分步骤监督）。在推理、代码生成、科学知识和工具使用基准上的实验验证了框架的实效——各方法保持其预期性能，分布式扩展良好，且易用性高（提供可运行配置、文档和安装包）。论文贡献在于提供了一个标准化、模块化的 OPD 实现平台，将零散的 OPD 研究整合到统一接口，降低后续研究和复现成本。**

<!-- paperflow:cc5386040dc6e0da -->
## StructAgent: Harness Long-horizon Digital Agents with Unified Causal Structure

[[Deep Reading - Jul 2026/StructAgent-Harness Long-horizon Digital Agents with Unified Causal Structure|Deep Reading]]

[https://arxiv.org/pdf/2607.11388v1](https://arxiv.org/pdf/2607.11388v1)

- **本文围绕长时程数字智能体中的核心挑战——任务进展的结构化表示和验证——提出了StructAgent。现有智能体因缺乏显式状态而难以实现可靠执行，本文通过统一状态和结构化工作流系统地解决了这一问题。统一状态压缩交互历史为紧凑的因果表示，结构化工作流将任务分解为可验证的步骤转换，并支持检查点、恢复和工具使用。实验在两个环境（OSWorld-Verified和Minecraft）上验证了有效性，在多个模型上取得巨大提升，实现了开源SOTA。论文的贡献在于将因果结构思想引入智能体设计，并展示了系统级改进优于单纯模型缩放。论证主线：首先指出长时程任务中原始交互历史的局限性，然后提出统一状态和结构化工作流作为解决方案，最后通过实验证明效果。技术主线：智能体围绕统一状态进行决策，每一步执行后通过验证器更新状态，确保进展可靠。实验主线：在OSWorld-Verified上对比多种模型基线，展示显著提升；在Minecraft上测试泛化，证明通用性。不足可能包括验证器依赖外部定义、状态设计成本、以及长尾任务中的泛化挑战。总体而言，这是一篇强系统性的论文，对智能体可靠性和长时程任务有重要价值。**

<!-- paperflow:1ff5e2268af7fe09 -->
## MM-ToolSandBox: A Unified Framework for Evaluating Visual Tool-Calling Agents

[[Deep Reading - Jul 2026/MM-ToolSandBox-A Unified Framework for Evaluating Visual Tool-Calling Agents|Deep Reading]]

[https://arxiv.org/pdf/2607.11818v1](https://arxiv.org/pdf/2607.11818v1)

- **本文提出MM-ToolSandBox，一个面向视觉工具调用代理的统一评估框架与基准。论文首先指出，随着多模态基础模型的进步，视觉工具调用成为一个关键但尚未被充分评估的智能体能力。现有基准要么只处理文本工具调用（如ToolBench），要么只评估多模态理解而不涉及工具执行（如MME），缺乏将视觉输入、多轮交互和可执行工具结合的平台。为了填补这一空白，作者设计了MM-ToolSandBox框架，其核心是一个状态化的执行环境，包含511个工具，跨越16个应用领域（如日历、地图、邮件、图像处理等）。工具支持两种执行模式：代码执行（Python）和结构化API调用。框架特别设计支持多图像输入和多轮对话，允许用户逐步提供图像，模型需要根据新的视觉信息调整计划。基准场景的生成是论文的技术亮点之一。作者提出信息流引导规划（Information-Flow Guided Planning, IFGP）方法，从原子信息单元出发，构建任务所需的信息依赖图，然后逆向生成用户指令序列和初始视觉状态。这种方法可以系统化地控制任务的复杂性、多样性和视觉要素。生成的场景经过多阶段过滤（逻辑完整性、视觉必要性、对话自然性等），最终由人工验证，得到258个高可靠性场景。此外，还设计了50个交互式UI应用场景，基于智能手机截屏模拟真实用户操作任务。实验部分评估了12个多模态模型，包括前沿闭源模型（GPT-5.4、Gemini-3.1、Claude 4.5 Opus）和一系列开源模型（Qwen2-VL 4B/72B、Llama 3.2 Vision 11B/90B、Cambrian 8B/34B等）。主要评价指标为Agent Success Rate（ASR），即完全正确完成所有工具调用的比例，以及Partial Success Rate（PSR），允许部分正确步骤计数。结果显示，即使最强的模型Claude 4.5 Opus也仅达到48.8% ASR，GPT-5.4为46.2%，Gemini-3.1为42.5%。开源模型中最大规模的Llama 3.2 Vision 90B达到29.2%，而小模型如Qwen2-...**

<!-- paperflow:45662af55326ed92 -->
## GEIS: A Generation-Evaluation-Improvement Loop of Agent Skills for Long-Form Article Generation

[[Deep Reading - Jul 2026/GEIS-A Generation-Evaluation-Improvement Loop of Agent Skills for Long-Form Article Generation|Deep Reading]]

[https://arxiv.org/pdf/2607.11503v1](https://arxiv.org/pdf/2607.11503v1)

- **长文本生成是NLP领域的挑战，现有方法如STORM通过多智能体协作提高了信息覆盖，但存在能力纠缠、难以迭代改进等问题。本文提出GEIS，一种生成-评估-改进循环的智能体技能框架，将长文本生成分解为命名技能。论文首先分析现有工作的不足，然后详细描述GEIS的设计：写作技能遵循Request-Plan-Draft-Audit-Refine-Deliver流程，评估技能进行成对比较并输出结构化报告，改进技能从评估中提取模式并修改写作技能的规则。在Tasi Harness平台上实现，作者在20个维基百科特色主题上进行了系统实验。使用GPT-5.4生成，Qwen 3.5 Plus评估。结果显示，GEIS在100分PDF质量标准上比Tasi Harness默认写作器提升8.0分，且优于STORM在结构质量和内容质量上的表现。更重要的是，通过一次改进循环，写作技能的平均分从82.90提升至86.95，其中17个主题得到改进，且提升主要来自内容质量。这些结果表明，将长文本生成范式从固定工作流转向可检查、模块化、评估引导的改进循环是有效的。论文还讨论了依赖LLM-as-a-judge的局限性，并提出了未来方向。总体而言，GEIS为长文本生成提供了一个系统化的框架，强调了技能分离和持续改进的重要性。**

<!-- paperflow:5ed7825555c8199b -->
## Agentic Routing: The Harness-Native Data Flywheel

[[Deep Reading - Jul 2026/Agentic Routing-The Harness-Native Data Flywheel|Deep Reading]]

[https://arxiv.org/pdf/2607.11399v1](https://arxiv.org/pdf/2607.11399v1)

- **论文深入探讨了大型语言模型智能体中的模型选择问题，提出了一种新颖的“Harness-Native agentic routing”范式。该范式的核心是将路由决策从单次查询优化提升到基于完整执行框架状态的步骤级决策。作者指出，随着模型专业化趋势的加强，单一模型难以在所有任务上最优，而智能体的执行框架（harness）提供了丰富的状态信息（如工具调用结果、上下文长度、验证反馈等），这些信息对模型选择至关重要。论文提出两种操作模式：单模型路由（选择最合适模型以平衡成本和质量）和多模型路由（集成多个模型输出以提高准确性）。关键创新在于每次路由决策自动产生结构化数据记录，形成“harness-native数据飞轮”：累积的执行数据用于训练更好的路由策略和针对性模型，进而提升后续执行质量和效率，产生更多高质量数据。论文在OpenSquilla系统中实现了该框架，包含冷启动LightGBM排序器和分阶段路由-模型路径。在DRACO和PinchBench基准上的实验验证了该范式的有效性，表明其不仅能有效控制成本，更能作为数据引擎驱动智能体系统持续改进。**

<!-- paperflow:28762eb0f06d812d -->
## Agent Hacks Agent: Autoresearch for Production-Agent Red-Teaming

[[Deep Reading - Jul 2026/Agent Hacks Agent-Autoresearch for Production-Agent Red-Teaming|Deep Reading]]

[https://arxiv.org/pdf/2607.11698v1](https://arxiv.org/pdf/2607.11698v1)

- **本文针对生产级LLM Agent的安全漏洞可迁移性问题，提出AHA（Agent Hacks Agent）框架。当前红队测试仅保留攻击表面工件，缺乏对漏洞使能条件的记录，导致难以审计和修补。AHA将黑盒红队形式化为autoresearch问题，以输出可审计、可迁移的漏洞知识为目标。核心是Vulnerability Concept Graph (VCG)，其中每个概念包含攻击面、使能条件、伪造器、迁移预测和证据。AHA采用可证伪循环：研究Agent提出假设，自动生成伪造器，在沙盒中执行攻击，然后反思轨迹并提升成功发现为VCG中的概念。实验在Claude Code和Codex上，针对三种模型和三个场景（含直接/间接攻击）进行。结果显示，发现的漏洞概念在不同模型间共享核心，冻结的VCG在单次攻击协议下比最强冻结发现基线高14.2%的ASR，且概念可跨场景和攻击通道迁移。该工作使得安全团队能直接检查使能条件、修补Agent并复用VCG验证修复，将红队产出从一次性攻击程序转向可积累的漏洞知识库。**

<!-- paperflow:18e736d92752d53e -->
## Beyond the Eye: Efficient Multimodal Reasoning via Self-Regulated Implicit Visual Tools

[[Deep Reading - Jul 2026/Beyond the Eye-Efficient Multimodal Reasoning via Self-Regulated Implicit Visual Tools|Deep Reading]]

[https://arxiv.org/pdf/2607.11106v1](https://arxiv.org/pdf/2607.11106v1)

- **本文提出了一种通过自我调节隐式视觉工具实现高效多模态推理的新范式BEE。首先分析了当前“Thinking with Images”范式的不足：频繁调用外部视觉工具（如检测、OCR）和重复图像编码导致高延迟和计算开销。BEE的核心思想是将工具调用行为融入模型训练，使模型学会自适应地平衡内部知识和隐式工具调用。方法包括两阶段训练：第一阶段是形式化CoT监督微调（Formalized CoT SFT），通过构建带有结构化工具槽和混合调用（工具调用与直接知识回答）的CoT轨迹，激活模型的隐式工具表示，并学习何时调用工具、何时依靠内部知识；第二阶段是自我调节奖励驱动对齐（Self-regulated Reward-Driven Alignment），引入净工具增益（Net Tool Gain, NTG）指标来量化每次工具调用的边际贡献，并基于此设计奖励函数，惩罚无效工具依赖，鼓励模型在内部知识足够时抑制工具调用，实现知识路由。实验方面，BEE在多个细粒度视觉感知基准（如MME-Real等）上达到最优性能，同时与现有显式工具方法和隐式方法（Monet、EVA）相比，推理延迟显著降低，在8K分辨率设置下延迟减少高达86.2%。在一般复杂推理任务上，BEE也保持了竞争力。进一步的分析表明，NTG度量能够有效检测冗余工具使用，自我调节机制减少了不必要的调用。总体而言，BEE为高效多模态推理提供了一种新的隐式工具范式，兼顾性能和效率。**

<!-- paperflow:bf4bbdc9d0e49295 -->
## Agentic Skill Optimization over Lie Algebroids

[[Deep Reading - Jul 2026/Agentic Skill Optimization over Lie Algebroids|Deep Reading]]

[https://arxiv.org/pdf/2607.11493v1](https://arxiv.org/pdf/2607.11493v1)

- **本论文从微分几何视角重新审视智能体系统中的技能优化问题。传统方法将技能编辑视为向量空间中的独立调节，忽略了编辑之间的结构化依赖、隐藏状态和非交换性。作者提出LASKO框架，将带类型的Markdown技能工件建模为底流形Md，可用编辑策略构成Lie algebroid A→Md。锚映射ρ区分可见效果（像）与隐藏结构（核），Lie括号度量编辑的非交换程度。基于此几何模型，LASKO在优化时先执行廉价的Lie括号筛选（微秒级），快速识别无效或冲突编辑，再对通过的编辑进行昂贵的LLM验证。在因果提取任务上，相比对所有编辑运行DeepSeek V3.1（671B参数）的暴力方法，LASKO实现近15倍加速。论文还讨论了正则、等价和奇异三种情况的处理。主要贡献包括：提出技能编辑的几何形式化；引入Lie algebroid作为优化框架；实现数量级的效率提升。论文为智能体系统的持续自我改进提供了新的理论工具和实用方法。**

<!-- paperflow:2e67c62c878cfa66 -->
## Automated Textbook Auditing with Multi-Agent LLM Systems

[[Deep Reading - Jul 2026/Automated Textbook Auditing with Multi-Agent LLM Systems|Deep Reading]]

[https://arxiv.org/pdf/2607.11276v1](https://arxiv.org/pdf/2607.11276v1)

- **本文针对教材质量保证需要同时评估事实准确性、技术正确性和语言质量这一挑战，提出了一个名为AI Textbook Auditor的模块化多智能体管道系统。系统核心由两个并行分析轨道组成：事实与技术轨道部署一组专门的LLM代理，分别负责检测事实不准确、代码错误、定义错误和概念不一致，并针对人文学科集成网络搜索增强；语法轨道在PDF原生级别操作，保留变音符号编码以处理罗马尼亚语等特色字符。所有候选问题经过一个法官代理，该代理使用领域特定规则过滤由编码伪像和有效领域惯例引起的假阳性，最终生成结构化报告供人工审核员验证。系统通过自定义提示实现领域自适应，提示需由领域专家编写以编码错误分类和约束。输入支持视觉原生页面渲染和PyMuPDF文本提取两种模式。论文在两个罗马尼亚高中教材上进行了演示：计算机科学教材产生56个技术发现（精确率62.5%），历史与社会教材产生72个发现（包括事实错误、意识形态偏见和语法错误）。实验表明，系统作为分诊工具可有效减少人工手动定位候选问题的工作量，但假阳性率较高，需要专家验证。论文强调其设计并非替代人工，而是辅助。相关工作部分指出，现有教育NLP主要聚焦学生文本评估，而教材内容审核是未充分探索的领域。主要贡献包括：（1）首次提出结合事实、技术和语法检查的多智能体教材审核系统；（2）设计了法官代理机制以减少假阳性；（3）通过自定义提示实现了领域自适应；（4）在真实教材上验证了系统可行性。局限性包括：假阳性问题、对专家提示的依赖、泛化性未充分证明、以及意识形态偏见检测的主观性。总体而言，本文为教育AI提供了一个有潜力的应用方向，但模型精度和工程成熟度有待进一步提升。**

<!-- paperflow:c6831a47d4786cd1 -->
## PaperRouter-Agent: A Content-Grounded LLM Agent for Personalized Hierarchical Paper Routing

[[Deep Reading - Jul 2026/PaperRouter-Agent-A Content-Grounded LLM Agent for Personalized Hierarchical Paper Routing|Deep Reading]]

[https://arxiv.org/pdf/2607.11564v1](https://arxiv.org/pdf/2607.11564v1)

- **论文提出了一种新的任务形式——个性化层次论文路由（PHPR），并设计了一个无需训练的LLM代理PaperRouter-Agent来解决它。该代理通过Planner缩小候选，Retriever获取文件夹内论文证据，Inspector评估匹配，Reflector利用用户反馈改进。在真实数据集和LaMP-2基准上的实验表明，该方法显著优于单次基线，特别是在按元数据（如venue, year）组织的文件夹上。论文的贡献包括任务形式化、方法设计、评估协议和实证结果。**

<!-- paperflow:ce5b30dd3d6bf668 -->
## UMoE:Unlocking Every Expert in Domain-Specific Training

[[Deep Reading - Jul 2026/UMoE-Unlocking Every Expert in Domain-Specific Training|Deep Reading]]

[https://arxiv.org/pdf/2607.11444v1](https://arxiv.org/pdf/2607.11444v1)

- **本文提出 UMoE，一种简单的预算不变流水线，旨在解决 MoE 大模型领域后训练中专家池与目标领域不匹配的问题。UMoE 分三步：（1）基于领域显著性剪枝一半路由专家；（2）通过扰动扩展从保留专家重扩回原规模；（3）应用标准 SFT。该方法不改变专家数、参数量和推理成本，且无领域超参数调整。在 Qwen3-30B-A3B 和 Qwen3.5-35B-A3B 上，覆盖数学、代码、科学、工具使用和智能体编码五个领域共 12 个基准，UMoE 一致优于直接 SFT，典型提升包括数学平均 3.4 分，SWE-bench Verified 6.0 分。数据扩展实验显示增益持续。分析揭示直接 SFT 存在专家分配冗余，UMoE 通过重新对齐将冗余容量转化为有效领域容量。本文方法简单高效，为 MoE 模型领域适应提供了新思路。**

<!-- paperflow:76d58ff455f0de60 -->
## OpsMem: Dual-Memory Reasoning with Cross-Memory Resonance for Failure Diagnosis

[[Deep Reading - Jul 2026/OpsMem-Dual-Memory Reasoning with Cross-Memory Resonance for Failure Diagnosis|Deep Reading]]

[https://arxiv.org/pdf/2607.11357v1](https://arxiv.org/pdf/2607.11357v1)

- **本文提出OpsMem，一种双记忆推理框架用于故障诊断。论文首先指出现有基于LLM的诊断方法在经验复用和状态协调上的不足，然后设计了一个包含短期记忆（STM）和长期记忆（LTM）的解决方案。STM追踪当前诊断的进展（证据、假设等），LTM以知识图谱形式存储可复用的操作经验。通过跨记忆共鸣（CMR），在诊断过程中动态激活与当前状态最相关的LTM子图，从而引导多智能体系统进行有效诊断。诊断结束后，经验巩固模块将成功的案例转化为新的模式并更新LTM。实验使用华为真实微服务故障数据集，对比了多种基线，结果显示OpsMem在诊断匹配度和相关性上大幅领先，消融实验验证了各组件的必要性，时间改进实验证明了其持续学习能力。论文的主要贡献在于提出了一种新颖的双记忆视角，将瞬态诊断状态与持久经验分离并动态耦合，为运维智能化提供了新思路。**

<!-- paperflow:75ce1b884adb73d5 -->
## Can LLMs Write Reliable Rubrics? A Meta-Evaluation for Experiment Reproduction

[[Deep Reading - Jul 2026/Can LLMs Write Reliable Rubrics-A Meta-Evaluation for Experiment Reproduction|Deep Reading]]

[https://arxiv.org/pdf/2607.12835v1](https://arxiv.org/pdf/2607.12835v1)

- **本文首次对LLM生成的rubric在论文复现评估中的可靠性进行了系统性元评估。基于rubric的评估是评估LLM研究智能体开放输出的一种有前景方法，尤其在论文复现场景中，直接比较论文与仓库代码常因幻觉而失败。然而，构建论文特定的rubric需要大量专家努力，限制了类似PaperBench基准的可扩展性。为解决这一问题，作者设想能否利用LLM自动生成rubric。

论文方法上，作者将rubric转化为更结构化的checklist格式，以利于自动评估。设计了四种生成设置（可能包括零样本、少样本、提示增强等），并使用两个不同的骨干LLM生成rubric。评估采用内在和外在双重验证：内在评估通过计算生成rubric与人类专家rubric之间的语义相似性衡量；外在评估则通过实际使用生成rubric对再生成的论文副本进行评分，并与人类评分进行对齐比较。

实验在PaperBench基准上进行，结果表明：增强的生成设置显著改善了下游评分对齐，最强的设置接近人类基线，但内在语义相似性的改进相对有限。进一步的定性分析揭示，LLM生成的rubric通常过于细粒度（包含过多子任务或标准）、偏向于给出高分、对不同论文领域的适应性较差。这些发现既展示了LLM作为rubric生成器的潜力（接近人类水平），也指出了现有局限（细粒度、高分偏向和领域不适应性）。

论文还讨论了其局限性，包括研究范围局限于机器学习论文复现，可能无法直接迁移到其他科学领域；实验仅基于单一基准和有限模型。总体而言，本文为自动rubric生成和元评估提供了重要基准和方法框架，并为未来改进LLM生成评估工具指明了方向。**

<!-- paperflow:5fdf4899270421f2 -->
## Who Grades the Grader? Co-Evolving Evaluation Metrics and Skills for Self-Improving LLM Agents

[[Deep Reading - Jul 2026/Who Grades the Grader-Co-Evolving Evaluation Metrics and Skills for Self-Improving LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2607.12790v1](https://arxiv.org/pdf/2607.12790v1)

- **本文针对自进化智能体系统依赖可靠评估指标这一隐藏假设，提出了Double Ratchet——一种协同进化评估指标和技能的框架。论文首先论证评估指标本身也可以进化，并为此设计了一个完整的进化生命周期：度量由小缺点检测器组合而成，通过预测10个锚定参考集和未标注输出共识进行训练，并保留一个从未见过的锚定集用于审计。其次，Double Ratchet将度量进化与技能进化（基于Ratchet的生命周期管理）协同，使得系统在没有任何可靠评估指标的情况下，能够恢复大部分由真实指标驱动的性能提升。在代码生成（MBPP+）、企业文本到SQL（Spider 2.0-Snow）和无参考报告生成（RAQS）三个任务上，Double Ratchet保留了88-110%的监督提升。最后，论文强调安全性来源于锚定纪律和外部审计：移除锚定保护会使度量失效，而外部审计可以检测并修复游戏行为。论文主张这种预期失败的设计是缺乏可靠自动验证器时的默认选择。主要贡献包括：提出可进化评估指标的概念、设计Double Ratchet框架、实证展示其有效性、分析安全机制的重要性。**

<!-- paperflow:c7339b7d458647e1 -->
## FinResearchBench II: A Deep Research Benchmark with Consensus-Derived Gold Rubrics for Distinguishing Financial Report Quality

[[Deep Reading - Jul 2026/FinResearchBench II-A Deep Research Benchmark with Consensus-Derived Gold Rubrics for Distinguis|Deep Reading]]

[https://arxiv.org/pdf/2607.12252v1](https://arxiv.org/pdf/2607.12252v1)

- **FinResearchBench II 是一篇面向金融领域深度研究代理评估的基准构建论文。核心贡献在于提出了一套完全自动化的、无需人类专家最终介入的评分标准生成与筛选流程，并生成了高质量的金牌评分标准集用于系统排名。

**问题背景**：深度研究代理（如基于LLM的助手）越来越多地被用于撰写长篇金融报告，但评估这些报告质量需要大量人工专家定义和执行评分标准，成本高且不可扩展。

**方法概述**：
1. **数据建构**：从真实世界收集104个金融用户查询，确保覆盖常见需求。
2. **候选评分标准生成**：用多个深度研究模型生成对应报告，然后自动从报告中提取14,450个查询特定的“是/否”评分标准（例如“报告是否包含公司的最新财务数据？”）。
3. **LLM评估可行性验证**：在采样子集上对比三位人类专家和三个LLM法官的判断，发现在三方均同意的项目上LLM与人类一致率达98.67%，从而论证用LLM替代人类执行大规模评估的合理性。
4. **一致性过滤**：对于每个评分标准，要求三个LLM法官在同一查询下的所有10个系统报告上完全一致，保留3,687个稳定评分标准。
5. **可区分性过滤**：保留那些至少能区分出一个系统通过和一个系统未通过的评分标准，最终获得2,600个共识派生的金牌评分标准。
6. **系统评估**：使用金牌集对10个深度研究系统打分，获得项目级通过率排名，范围从58.58%到22.23%，显示出有效的区分力。

**创新点**：
- 首次提出完全自动化的评分标准生成流程，移除人类专家的最终执行环节。
- 通过双重过滤（一致性与可区分性）保证评分标准的稳定性和信息量。
- 基于真实用户查询，生态效度高。
- 在金融报告深度研究评估领域提供了首个大规模基准。

**意义与局限**：该工作为深度研究代理的自动评估提供了可扩展的范式，可降低评估成本并支持系统比较。局限在于目前仅针对金融领域，且过滤过程可能丢弃部分有用评分标准。实验结果依赖LLM法官的质量，未分析法官变化的敏感性。此外，评分标准的可解释性及与人类偏好的深层对齐尚待探索。

总体而言...**

<!-- paperflow:94e485127774d872 -->
## Track, Rank, Crack: Epistemic Working Memory Scales Multi-Hop Reasoning in Language Agents

[[Deep Reading - Jul 2026/Track, Rank, Crack-Epistemic Working Memory Scales Multi-Hop Reasoning in Language Agents|Deep Reading]]

[https://arxiv.org/pdf/2607.12267v1](https://arxiv.org/pdf/2607.12267v1)

- **该论文系统地研究了语言代理在多跳推理中的性能退化问题，将其归因于上下文稀释，并提出SLEUTH——一种基于提示的结构化认知工作记忆方法。SLEUTH维护三个显式组件：Confirmed Facts（基于来源的确证事实）、Active Hypotheses（按证据排序的候选假设）和Open Questions（驱动下一步的信息需求）。在五个多跳基准上的实验表明，SLEUTH相比CoT、ReAct、Reflexion等基线有显著提升，且优势随推理链长度增加而扩大。论文进一步识别了证据充分性问题——代理常找到答案却不提交——并设计轻量承诺触发器解决；该触发器仅在结构化状态下有效，验证了结构化状态对有效承诺的必要性。在弱模型上强制执行SLEUTH协议可恢复高达+19个百分点的性能，凸显了推理组织方式比原始模型能力更重要。论文提供了丰富的消融和分析实验，揭示了组件的贡献、失效模式以及剩余差距。总体而言，SLEUTH通过将隐式的上下文状态显式化，为可扩展、可审计的多跳推理代理提供了实用方案，并对agent如何组织推理和承诺决策提供了深刻见解。**

<!-- paperflow:1784e9b4e60daf7d -->
## MemOps: Benchmarking Lifecycle Memory Operations in Long-Horizon Conversations

[[Deep Reading - Jul 2026/MemOps-Benchmarking Lifecycle Memory Operations in Long-Horizon Conversations|Deep Reading]]

[https://arxiv.org/pdf/2607.12893v1](https://arxiv.org/pdf/2607.12893v1)

- **本论文提出了MemOps，一个用于长期对话中记忆生命周期操作的基准。针对现有基准仅评估最终答案准确率而无法诊断记忆状态一致性的问题，MemOps将记忆操作显式化，定义了记住（Remember）、忘记（Forget）、更新（Update）、反思（Reflect）等基本操作及其结构化表示，每个操作包含触发条件、目标、范围、状态转换和支持证据。通过可控生成管道，可以自动生成带有gold操作轨迹的任务导向对话数据。设计了六类操作级探针（操作检测、状态一致性、溯源、范围判断、轨迹重建等），分别在邻接证据（短上下文）和长上下文设置下评估。实验覆盖了四大类记忆系统：长上下文模型、检索增强系统、参数化记忆系统和托管记忆系统。主要发现包括：会话级检索在所有操作类型上优于轮次级检索；长上下文模型在长上下文设置下对有序状态重建任务表现薄弱；最终答案准确率与操作级指标相关性低，无法反映真实记忆状态。这些结果验证了操作级评估的必要性，为长期记忆研究提供了可诊断的评估工具。论文的主要贡献在于：1）提出记忆生命周期操作框架，实现从最终答案评分到操作级诊断的转变；2）构建可控生成管道和操作级探针，支撑细粒度评估；3）通过多系统实验揭示关键记忆失败模式；4）推动记忆评估向可解释性发展。**

<!-- paperflow:98d665f45e625272 -->
## Exploratory, Communicative, and Deployable: Vision-Driven Embodied Agents for Open-World Mobile Manipulation

[[Deep Reading - Jul 2026/Exploratory, Communicative, and Deployable-Vision-Driven Embodied Agents for Open-World Mobile M|Deep Reading]]

[https://arxiv.org/pdf/2607.13653](https://arxiv.org/pdf/2607.13653)

- **本文提出一个名为REAL（推断）的视觉驱动具身智能体框架，针对开放世界移动操作任务，将探索、通信与部署能力统一在一个系统内。核心组件包括世界图（WG）用于结构化场景表示，一组工具API用于环境交互，以及两阶段训练（SFT → GSPO RL）策略。在模拟器中训练后，该框架在真实世界超过60次任务中展现出零样本迁移能力，成功率达到可对比水平（模拟内39.3%）。论文详细分析了训练过程中的成功瓶颈与失败模式，包括逻辑推理提升、动作记忆丢失、模糊指令处理等，为后续研究提供了系统基线。主要贡献：1）提出包含探索、通信与部署的完整框架；2）设计sim-to-real一致的表示与动作接口；3）通过大量模拟与真实实验验证了有效性。**

<!-- paperflow:04bb7f6631bf7151 -->
## SPyCE: Skill-Policy Co-evolution for Multimodal Agents

[[Deep Reading - Jul 2026/SPyCE-Skill-Policy Co-evolution for Multimodal Agents|Deep Reading]]

[https://arxiv.org/pdf/2607.13854](https://arxiv.org/pdf/2607.13854)

- **本文提出SPyCE，一种用于多模态智能体的技能-策略共同进化框架，旨在解决视觉推理任务中经验的有效复用问题。该框架引入层次技能库，分为执行技能和工作流技能，分别捕获局部操作模式和全局任务逻辑。通过强化学习过程，智能体根据检索的技能进行决策，成功轨迹被蒸馏抽象为新技能，不断更新库，实现技能与策略的协同提升。在多个基准上的实验表明，SPyCE优于纯RL和基于记忆的方法，消融分析验证了层次技能设计的必要性。该工作为多模态智能体的经验抽象与重用提供了一种系统化方案。**

<!-- paperflow:f37321ab8f9fecae -->
## DeepStress: Stress-Testing Deep Search Agents

[[Deep Reading - Jul 2026/DeepStress-Stress-Testing Deep Search Agents|Deep Reading]]

[https://arxiv.org/pdf/2607.13920](https://arxiv.org/pdf/2607.13920)

- **论文针对搜索代理在现实应用中可能遭遇低质量证据从而导致失败的问题，提出 DeepStress 框架，通过用合成文档生成器替换检索模块，在受控环境下系统评估搜索代理对不可靠信息的鲁棒性。框架控制文档的信任度、相关性和事实性三个维度。在 HotpotQA 和 BrowseComp-Plus 数据集上测试多种搜索代理，发现代理间存在显著行为差异，并提出新指标更好地记录系统输出以及冲突知识交互。实验揭示了代理处理退化文档的不同策略，如是否表达不确定性、是否增加检索等。论文还分析了参数知识与检索知识的交互作用。这项工作为评估和提升搜索代理的可靠性提供了重要工具和见解。**

<!-- paperflow:d9fc71220fdc1fd0 -->
## AgentCompass: A Unified Evaluation Infrastructure for Agent Capabilities

[[Deep Reading - Jul 2026/AgentCompass-A Unified Evaluation Infrastructure for Agent Capabilities|Deep Reading]]

[https://arxiv.org/pdf/2607.13705](https://arxiv.org/pdf/2607.13705)

- **论文《AgentCompass: A Unified Evaluation Infrastructure for Agent Capabilities》针对大型语言模型作为智能体时评估基础设施碎片化、难以复现的问题，提出了统一的评估平台 AgentCompass。首先，论文分析了当前评估管道的高耦合性、基准分散以及缺乏统一运行环境等痛点。然后，详细介绍了 AgentCompass 的设计：包括声明式 RunRequest 解耦评估对象与操作、模块化组件（Benchmark、Harness、Environment）通过标准化协议组合、异步运行时高效处理多轮交互。AgentCompass 原生集成了 20+ 基准，涵盖工具使用、Web 与研究、科学推理、智能体编码和生产力五大维度。基于该平台，论文在编码和生产力任务上对多个主流模型（如 GPT-4o、Claude-Opus-4.8、DeepSeek-V4-Pro、Qwen 等）进行了评估，观察到大部分模型遵循 test-time scaling law，但同时发现异常值。实验还展示了 AgentCompass 在效率、复现性和可扩展性方面的优势。最后，论文讨论了基础设施的局限性及未来发展方向。总体而言，AgentCompass 为智能体评估提供了一个稳健、标准化的平台，有助于推动智能体研究的良性发展。**

<!-- paperflow:fac9318e76537c76 -->
## EgoProceVQA: A Novel Egocentric Procedural Understanding Task with Self-Skill-Exploration Agent

[[Deep Reading - Jul 2026/EgoProceVQA-A Novel Egocentric Procedural Understanding Task with Self-Skill-Exploration Agent|Deep Reading]]

[https://arxiv.org/pdf/2607.13792](https://arxiv.org/pdf/2607.13792)

- **本文提出EgoProceVQA任务，系统评估第一人称视频的程序性理解能力。开发EgoProceGen平台自动化构建QA数据，建立3600问题的基准。评估发现现有MLLM和代理存在明显不足。为此设计EgoProceAgent框架，通过预定义工具库和标准化子技能库，采用四阶段自探索机制发现有效策略，无需真实反馈。实验表明EgoProceAgent在Qwen2.5-VL-7B上达到开源模型最佳，PSR任务提升84.3%。本文贡献了新任务、数据平台和自探索代理框架，为程序性AI助手奠定基础。**

<!-- paperflow:b2e009170f787001 -->
## Self-Evolving Agent Harnesses via Gated Semantic Quality-Diversity

[[Deep Reading - Jul 2026/Self-Evolving Agent Harnesses via Gated Semantic Quality-Diversity|Deep Reading]]

[https://arxiv.org/pdf/2607.13683](https://arxiv.org/pdf/2607.13683)

- **本文针对大型语言模型智能体的实际部署场景，指出其性能不仅取决于模型本身，更受外部充满（prompts、知识注入、运行时控制等）的显著影响。在模型冻结条件下，自动化优化充满成为关键途径。但现有自演化方法面临自生成反馈噪声大和基于任务评估的过拟合问题。为此，作者提出新颖的自演化框架，核心创新是分离变更提议和信用分配，并引入门控语义质量-多样性存档（GSME）。语言模型诊断失败并提案补丁，而采样、执行和显著性测试由确定性代码处理，确保每个改进都经得起统计检验。补丁按病理（WHERE×WHY）存储而非按任务，这一归纳偏置迫使演化覆盖多样失败模式，阻止过拟合。泛化性通过密封测试严格评估。实验在7个领域（Qwen3.6-27B）上进行，训练选择的充满在密封测试上全数提升，增益+9到+15.5 pp，保留训练增益86%-147%，证实泛化性。跨模型研究（Qwen3.5-397B和Gemini2.0-Flash-001）揭示病理-补丁匹配定律：不同模型有主导病理，补丁需匹配该病理才有效，且匹配关系在家族间复制。此外，祖先复用降低成本约67%。论文还讨论了信用分配对强验证器依赖等局限。总之，本文提出了系统性、可泛化的自演化方法，证明了诊断-信用循环而非具体充满的可迁移性。**

<!-- paperflow:bf5d2943d9995c67 -->
## Multimodality as Supervision: Self-Supervised Specialization to the Test Environment via Multimodality

[[Deep Reading - Jul 2026/Multimodality as Supervision-Self-Supervised Specialization to the Test Environment via Multimod|Deep Reading]]

[https://arxiv.org/pdf/2607.14721v1](https://arxiv.org/pdf/2607.14721v1)

- **本文提出了Test-Space Training (TST)，一种在特定测试环境中利用多模态数据进行自监督预训练的框架，旨在训练高性能专用模型而无需外部大规模数据集。通过将设备限制在测试环境中收集多模态数据，TST采用跨模态学习作为自监督信号。实验结果表明，仅凭环境中的多模态数据即可与DINOv2、CLIP等通用模型竞争，并揭示了预训练数据在环境特化与泛化之间的权衡。论文还进行了广泛的消融分析，验证了多模态的重要性。该工作为减少对互联网数据集的依赖提供了新的视角，并对机器人、AR/VR等领域的模型部署具有实际意义。**

<!-- paperflow:f11f46a85d369060 -->
## Plover: Steering GUI Agents through Plan-Centric Interaction

[[Deep Reading - Jul 2026/Plover-Steering GUI Agents through Plan-Centric Interaction|Deep Reading]]

[https://arxiv.org/pdf/2607.15193v1](https://arxiv.org/pdf/2607.15193v1)

- **本文提出Plover，一个以计划为中心的GUI自动化系统，旨在解决自主代理在动态真实环境中行为不透明、用户难以介入纠正的问题。核心思路是将任务计划（即步骤序列）从内部推理状态转为持久、可视、可编辑的外化制品。系统采用规划器-执行器分离架构：规划器基于用户指令和当前截图生成结构化步骤，执行器逐步执行具体交互，二者通过计划制品解耦。用户界面始终显示当前计划及执行进度，支持四种干预模式：（1）直接编辑计划步骤，（2）用自然语言添加约束或指引，（3）在截图上标注目标元素或位置，（4）在极端情况下完全手动接管。智能重规划机制允许用户在计划更新前预览差分并确认。为验证设计，作者先进行了6人形成性研究，梳理出计划制定、执行监督、失败恢复三类挑战；然后选取26个自主代理在基准任务上的失败案例，让参与者用Plover修复，发现大部分失败通过简单自然语言引导即可恢复，说明失败多源于局部上下文而非整体规划错误。进一步场景分析展示了系统在真实多步任务中的实用性。主要贡献包括：提出计划中心交互新范式；实现完整系统并开源；实证揭示自主GUI失败的可修复性规律；为混合倡议代理设计提供设计建议。局限在于评估样本较小，尚未验证跨平台泛化，且系统优先轻量干预，对需完全重写的复杂场景支持有限。总体而言，Plover证明了计划透明化和局部干预机制能显著提升GUI代理的可控性和用户信任。**

<!-- paperflow:b57927a42543e1d2 -->
## Video = World + Event Stream

[[Deep Reading - Jul 2026/Video = World + Event Stream|Deep Reading]]

[https://arxiv.org/pdf/2607.15038v1](https://arxiv.org/pdf/2607.15038v1)

- **本论文介绍Wan-Streamer v0.3，提出将视频视为世界加事件流的组织视图。世界包含持久上下文（场景、环境、主体、声音特征等），事件流包含随时间的变化（行为、语音、场景变化等）。基于此，设计通用预训练目标：给定世界和输入，预测世界的实时响应。该预训练能力可迁移至实时全双工音视频交互等下游任务。模型采用视觉-语言-动作式的多模态理解过程，将用户输入映射为语言形式的语音和行为动作，形成交织的事件流，直接条件生成音频-视频。系统规格保持与v0.2一致：640×368@25FPS，160ms流单元，约200ms模型响应延迟，550ms总交互延迟。论文主要贡献包括：提出world+event stream分解，给出通用预训练任务，并实例化实时交互系统。但未提供定量实验验证。整体上，该工作为实时流式多模态交互提供了一种概念框架。**

<!-- paperflow:d6e52f0b06abaaaa -->
## Harnessing LLMs for Reliable Academic Supervision: A Comparative Study

[[Deep Reading - Jul 2026/Harnessing LLMs for Reliable Academic Supervision-A Comparative Study|Deep Reading]]

[https://arxiv.org/pdf/2607.14707v1](https://arxiv.org/pdf/2607.14707v1)

- **本文以学术监督为案例，系统研究了LLM作为可靠组件时的缰绳工程方法。首先指出现有纯对话式LLM在高可靠领域部署的问题，提出了以LangGraph为核心的ASuS系统，通过统一状态模式、符号-语义检索、模式验证、LLM判官、人类循环、风险评分和审计追踪等模块，约束LLM输出。为严格评估，设计五个场景，从六个维度进行十名评估者的盲测和2×2消融。实验结果强烈支持缰绳工程：小模型（GPT-4o-mini）+缰绳全面超越大模型（GPT-5）无缰绳，平均分4.08 vs 1.23，统计显著。消融表明缰绳贡献模型不变。论文还提取了七个可复用模式，对LLM系统工程有重要参考。最后讨论了局限性（阈值未敏感性测试、领域有限）和未来工作。**

# AI Research

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Research
- 方法：待从后续精读中沉淀
- 论文/报告：3 篇
- What Types of Human-AI Teams Exist?
- HERMES: A Multi-Granularity Labeling Substrate for Pre-training Data Mixtures
- Best-of-$N$ TTS Evaluation is Confounded by ASR Family Alignment
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

<!-- paperflow:85db1df3d374db0d -->
## Best-of-$N$ TTS Evaluation is Confounded by ASR Family Alignment

[[Deep Reading - Jul 2026/Best-of-$N$ TTS Evaluation is Confounded by ASR Family Alignment|Deep Reading]]

[https://arxiv.org/pdf/2607.08256](https://arxiv.org/pdf/2607.08256)

- **这篇论文关注零样本TTS中Best-of-N推理的评估问题。BoN通过ASR验证器从多个候选中选择内容最准确的输出，但本文发现这种评估本身存在混淆：验证器的表现高度依赖于评估它的ASR模型家族。作者在LibriSpeech-PC数据集上使用F5-TTS生成候选，用Whisper、wav2vec2.0和HuBERT三种不同家族的ASR模型作为verifier和evaluator进行实验。实验显示，verifier的排名在不同evaluator下会反转，同家族verifier-evaluator对会高估效果，而跨家族对则显示更真实的性能差距。为了缓解这个问题，作者提出两种跨家族排名集成方法：rank-averaging（平均排名）和conjunctive max-rank（最小化最大排名）。这两种方法在三个evaluator上实现了平均WER 1.61%（N=10），比F5-TTS基线降低12%，并且自动主观指标SIM-o和UTMOS没有退化。此外，最佳单一verifier（Whisper-large-v3）在官方评估器下将WER从2.06%降至1.72%。论文还分析了oracle headroom，指出仍有优化空间。最终建议在BoN TTS评估中至少使用两个不同训练谱系的ASR家族进行交叉验证，以避免结果偏倚。**

# Computer Vision

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Computer Vision
- 方法：agent, generation, language, vision-language-model, reasoning, vision, reinforcement-learning, multimodal-learning
- 论文/报告：31 篇
- Optimizing Visual Generative Models via Distribution-wise Rewards
- Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning
- ESC: Emotional Self-Correction for Reliable Vision-Language Models
- From SRA to Self-Flow: Data Augmentation or Self-Supervision?
- ChatImage: Navigating Long-Form LLM Answers through Interactive Images
- SteelBench: Evaluating Vision-Language Models in Real-World Industrial Environments
- GUSH3R: Everyone Everywhere All at Once as Gaussians
- AlayaWorld: Long-Horizon and Playable Video World Generation
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

<!-- paperflow:111e63c960b9fbbf -->
## AlayaWorld: Long-Horizon and Playable Video World Generation

[[Deep Reading - Jul 2026/AlayaWorld-Long-Horizon and Playable Video World Generation|Deep Reading]]

[https://arxiv.org/pdf/2607.06291](https://arxiv.org/pdf/2607.06291)

- **AlayaWorld 是一个由 Alaya Lab 团队开发的全栈开源框架，用于构建可实时交互、长时域的视频世界。它针对传统游戏世界开发的高成本和低灵活性，以及现有视频世界模型缺乏统一平台的问题，提出了一个整合数据准备、模型构建、训练、推理加速和部署的模块化解决方案。

该方法基于 LTX-2.3 视频扩散模型进行微调，并引入了条件控制模块以实现实时动态交互。生成过程采用自回归 chunk 方式，每 chunk 通过四次去噪步骤产生720p 24fps 的一秒视频，从而支持持续的世界更新和用户动作（如导航、战斗、施法、召唤怪物等）。

实验部分重点展示了两个方面的结果：第一，在多种视觉风格（写实、Minecraft、水墨、油画、赛博朋克、像素和塞尔达风格）下，世界场景的结构一致性得以保持，表明模型能分离内容与风格；第二，通过运行时性能测试验证了系统在实时交互条件下的响应能力。此外，论文还提供了与公开 baseline 的公平比较（虽未提取详细指标）和标准视频生成评估工具。

主要贡献包括：
- 提出了首个全栈开源框架，覆盖从数据到部署的完整流程。
- 基于 LTX-2.3 结合设计模块实现了实时交互世界生成。
- 验证了多种视觉风格下的场景一致性和长时域稳定性。
- 提供了可复现的代码、管道和评估工具。

局限性与未来工作包括：骨干模型能力可能受限；实时条件更新仍有延迟；长时域生成中的一致性可能随序列增长下降；需要更多探索不同交互模态和更高分辨率。

总体而言，AlayaWorld 为生成式世界模型的研究与应用提供了坚实的实践基础，特别适合对实时交互、游戏、具身智能感兴趣的研发者。**

<!-- paperflow:9ca83ccdf162755c -->
## Prompt-Adapter Context Routing for Parameter-Efficient Multi-Shot Long Video Extrapolation

[[Deep Reading - Jul 2026/Prompt-Adapter Context Routing for Parameter-Efficient Multi-Shot Long Video Extrapolation|Deep Reading]]

[https://arxiv.org/pdf/2607.06481](https://arxiv.org/pdf/2607.06481)

- **PACR-Video提出一种参数高效的多镜头长视频外推方法。核心思想是将预训练的文本到视频扩散Transformer冻结，仅通过少量可训练参数实现跨镜头一致控制。方法在Transformer的时序注意力层插入低秩适配器，为每个镜头学习角色提示标记，并构建递归提示库存储实体、位置、动作和风格信息。生成下一镜头时，通过门控机制仅路由当前场景所需的提示历史。训练采用局部重建（噪声预测）、全局身份对比（DINO特征）和提示稀疏正则化，并引入适配器组合调度以平衡早期一致性与后期变化。在六个覆盖动画、真实动作和电影叙事的基准上，PACR-Video在FVD、语义对齐、身份保持、运动平滑性和人类偏好等指标上显著优于现有基线（包括记忆增强和递归上下文方法）。实验验证了紧凑提示表示和参数高效适配器可以有效取代密集记忆和全微调，为长视频生成提供可控且稳定的方案。**

<!-- paperflow:a0923adc2dccd22b -->
## Vision as Unified Multimodal Generation

[[Deep Reading - Jul 2026/Vision as Unified Multimodal Generation|Deep Reading]]

[https://arxiv.org/pdf/2607.06560](https://arxiv.org/pdf/2607.06560)

- **本文提出了一种新的计算机视觉范式——统一多模态生成，将所有视觉任务统一到文本和图像生成空间中。通过自然语言指令和视觉提示，模型无需任务专用架构即可执行检测、分割、深度估计等多种任务。作者构建了SenseNova-Vision语料库，将现有视觉标注转换为指令-响应格式，并基于预训练多模态模型进行训练。实验表明单一模型在多个视觉任务上达到专业水平，展示了这种范式的可扩展性和统一性。论文贡献包括：提出统一框架、构建大规模语料库、验证模型有效性。**

<!-- paperflow:660ff5ea3aa8b9be -->
## UI2App: Benchmarking Visual Interaction Inference in Executable Web Application Generation

[[Deep Reading - Jul 2026/UI2App-Benchmarking Visual Interaction Inference in Executable Web Application Generation|Deep Reading]]

[https://arxiv.org/pdf/2607.06306](https://arxiv.org/pdf/2607.06306)

- **Large language models (LLMs) have demonstrated growing competence in web page generation.

Large language models (LLMs) have demonstrated growing competence in web page generation. However, existing text-driven approaches rely on complex prompts that impose substantial demands on users and offer limited expressivity for page layout and cross-page visual…

In this paper, we introduced UI2App, the first benchmark to measure interaction inference: recovering application behavior from image-only multi-page screenshots, with no textual… Each artifact is scored along four dimensions (executability, navigation reachability, visual fidelity, and IIS), with IIS grounded in the interaction taxonomy that tiers…

UI2App is the first benchmark targeting interaction inference rather than specification-following from image-only input. \- End-to-end evaluation protocol and IIS taxonomy.

主要贡献包括：In this...**

<!-- paperflow:79dcdecc1a973c65 -->
## A Definition and Roadmap for World Models

[[Deep Reading - Jul 2026/A Definition and Roadmap for World Models|Deep Reading]]

[https://arxiv.org/pdf/2607.06401](https://arxiv.org/pdf/2607.06401)

- **本文由Xinyuan Chen等二十余位作者联合撰写，旨在为人工智能领域中'世界模型'（World Model）这一核心但定义模糊的概念提供精确的科学定义和系统的发展路线图。全文共六章。第一章引言指出，从基于模型的强化学习到视频生成，再到具身机器人和物理AI，大量系统被冠以'世界模型'之名，却缺乏统一的定义。第二章正式定义世界模型：它是一个使用有限计算资源对物理世界中状态转移过程的近似，必须满足全模态性、多维输出和因果结构三大属性。文章进一步讨论了智能体-环境循环框架（2.2节），并区分了'理解世界'（学习隐式表征用于决策）和'预测未来'（显式预测下一观测）两种主要观点（2.3节），认为二者并非互斥而是互补。第三章（根据目录推断）对现有世界模型进行了分类，可能包括基于显式物理模型的方法和基于神经网络学习的方法。第四章详细介绍了构建有效世界模型的关键技术，包括：链式想象（Chain-of-Imagination），即将推理拆解为多步通过世界模型进行想象的过程；物理约束学习，如利用微分方程或对称性来引导模型；反事实推理，用于探索干预效应；以及长期和层次化规划。第五章（推测）列举了应用领域，涵盖机器人操作、自动驾驶、游戏和通用物理AI。第六章总结开放挑战，包括数据不对称（多模态数据分布不均）、物理一致性的保持、评估标准化的缺失、安全和对齐等问题。本文的核心贡献在于给出了一个跨子领域可接受的世界模型定义，并规划了一个从基础预测到完全物理交互的分阶段路线图。适合AI研究者快速了解世界模型领域的整体图景和关键争议。**

<!-- paperflow:c06f61ef5569aeb9 -->
## To Retain or to Adapt? Generalizing Continual Learning

[[Deep Reading - Jul 2026/To Retain or to Adapt-Generalizing Continual Learning|Deep Reading]]

[https://arxiv.org/pdf/2607.05609](https://arxiv.org/pdf/2607.05609)

- **本论文对持续学习（CL）的基础哲学进行了深刻的反思。长期以来，CL 领域被“缓解灾难性遗忘”这一单一目标所主导，默认将联合任务学习（JTL）视为不可逾越的性能巅峰。作者通过严密的理论推导和实验验证指出，这种“保留至上”的观点在非平稳环境中是站不住脚的。

论文的核心贡献在于将 CL 重新定义为在线优化问题，并引入了平均终身误差（ALE）作为评价标准。通过对 ALE 的分解，作者识别出了“不稳定性”与“瞬态误差”之间的根本冲突。理论推导得出的“临界任务时长”公式为我们提供了一个清晰的判据：当任务足够长或环境变化足够快时，保留旧知识不仅无益，反而有害。这一发现为“何时该遗忘”提供了科学的解释，而不仅仅是将其视为一种失效模式。

在方法论上，论文提出的“预测性持续学习（Predictive CL）”框架将 CL 与在线学习理论接轨，强调了对未来任务结构的预测能力。实验部分通过图像分类和强化学习任务，生动地展示了历史知识如何从助推器变成绊脚石，并证明了简单的窗口化策略在处理非平稳流时的有效性。总而言之，这篇论文不仅提供了一套新的度量工具和算法框架，更重要的是，它促使社区重新思考持续学习的终极目标：我们究竟是为了记住过去，还是为了更好地适应未来？**

<!-- paperflow:8ac2988428abd35a -->
## OpenCoF: Learning to Reason Through Video Generation

[[Deep Reading - Jul 2026/OpenCoF-Learning to Reason Through Video Generation|Deep Reading]]

[https://arxiv.org/pdf/2607.08763](https://arxiv.org/pdf/2607.08763)

- **本文聚焦于视频生成模型中的推理能力，提出链式帧推理（CoF）这一新范式，即通过生成连续的帧序列展示推理过程。针对现有视频生成模型在推理任务上监督不足和缺少专门设计的问题，作者贡献了三方面：首先，构建了OPENCoF-17K数据集，包含11个任务类别，每个任务由输入图像和对应推理视频组成，为模型提供丰富的时间推理监督。其次，基于预训练视频生成模型Wan2.2-I2V-A14B，利用该数据集微调得到WAN-CoF，在四个公开视频推理基准上显著超越基线。最后，为进一步提升推理能力，探索了视觉推理令牌和文本推理令牌机制：视觉令牌嵌入潜在表示中捕捉精细时空变化，文本令牌增强语义条件。通过性能对比和注意力可视化，表明令牌分别对低层视觉推理和高层语义推理有所贡献，且在不同深度、去噪步骤和时空位置呈现不同关注模式。整体工作不仅提供了数据与模型基础，也为视频生成与推理的结合开辟了方向。**

<!-- paperflow:4a4148d20da5a9d7 -->
## APIVOT: Adaptive Planning with Interleaved Vision-Language Thoughts

[[Deep Reading - Jul 2026/APIVOT-Adaptive Planning with Interleaved Vision-Language Thoughts|Deep Reading]]

[https://arxiv.org/pdf/2607.08024](https://arxiv.org/pdf/2607.08024)

- **APIVOT 论文聚焦于长程机器人规划中的核心矛盾：高层语义推理与底层几何约束之间需要联合优化，而现有方法将语言和视觉能力隔离或按固定顺序使用，导致在空间密集任务中性能受限。为此，作者提出了一种基于VLM的自适应交织规划框架。技术主线包括三个训练阶段：第一阶段通过监督微调使VLM生成描述性的语言思考（子目标分解和动作排序）；第二阶段引入视觉生成模块，使模型能够产生代表未来状态图像的视觉思考；第三阶段学习自适应模态选择，允许模型按需决定使用哪种思考。实验在模拟厨房环境中验证，涵盖多种含有空间约束的长期任务。实验主线显示APIVOT 在成功率上大幅超越GPT-4V、LLaVA等通用VLM以及SayCan等专用规划器，尤其在障碍物和狭小空间场景下提升最为显著。关键发现包括：模型学到了合理的模态分配行为——在几何关键步骤生成视觉思考，在语义步骤仅用语言思考——从而在保持高效的同时确保了空间可行性。论文还分析了视觉思考作为一种紧凑的内部表示如何比语言描述更有效地传达空间信息。总体而言，这项工作为端到端机器人规划提供了一种全新的范式，即让VLM在规划过程中原生地、自适应地使用多种思维形式，架起了语言推理与几何验算之间的桥梁。**

<!-- paperflow:8d46617125058326 -->
## DeltaV: Thinking with Visual State Updates in Unified Large Multimodal Models

[[Deep Reading - Jul 2026/DeltaV-Thinking with Visual State Updates in Unified Large Multimodal Models|Deep Reading]]

[https://arxiv.org/pdf/2607.08434](https://arxiv.org/pdf/2607.08434)

- **论文DeltaV提出了一种视觉更新范式，用于统一大多模态模型中的交织多模态推理。当前ULMMs在每个推理步骤生成完整中间图像，导致大量令牌冗余和监督稀释。DeltaV将其替换为增量视觉更新：模型基于历史状态预测紧凑的更新令牌，仅编码变化内容，并引入时间相似性（TSIM）路由器动态控制每个更新的令牌预算。为了支持多样化推理，作者构建了StructCoT数据集，包含1.05M样本，覆盖44个任务领域。实验表明，DeltaV在不牺牲重建保真度的前提下，减少55.6%的视觉令牌，并将推理性能提升3.3%超过全图像生成基线。训练的DeltaV-2B在域内评估中超越更大开源模型8.4%，在外部基准上超越同等规模Qwen3-VL-2B 5.9%。论文的主要贡献包括提出了一个高效的视觉更新框架、一个自适应令牌分配机制、一个大规模多模态推理数据集，以及全面的实验验证。局限性可能包括对初始视觉状态的敏感性和对复杂场景更新的处理能力（未经证实）。该工作为多模态推理提供了新的发展方向，特别是在计算效率和监督效率方面。**

<!-- paperflow:853671f57dd827d3 -->
## OPSD-V: On-Policy Self-Distillation for Post-Training Few-Step Autoregressive Video Generators

[[Deep Reading - Jul 2026/OPSD-V-On-Policy Self-Distillation for Post-Training Few-Step Autoregressive Video Generators|Deep Reading]]

[https://arxiv.org/pdf/2607.08766](https://arxiv.org/pdf/2607.08766)

- **本文提出OPSD-V，一种后训练自蒸馏方法，专门针对少步自回归视频生成器的长时退化问题。现有少步AR模型虽然在延迟上优势，但误差积累和运动弱化限制了实际应用。OPSD-V通过引入真实长视频作为时间上下文，并设计缓存感知的师生框架，让学生在其推理路径上获得密集的轨迹级监督。具体地，学生按常规自回归方式生成，教师则使用更清洁的缓存（替换旧历史为真实帧）提供正确信号。训练仅在自定义的3,800个一分钟长视频上进行，不改变推理配置。实验证明该方法能够显著提升视觉质量、运动动态和长视频基准分数，用户偏好率高达82.5%（排除平局）。论文还分析了相关工作和方法的局限性，指出未来可以扩展数据规模和模型范围。**

<!-- paperflow:86267eef4e04ad6a -->
## On Locality and Length Generalization in Visual Reasoning

[[Deep Reading - Jul 2026/On Locality and Length Generalization in Visual Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2607.09061](https://arxiv.org/pdf/2607.09061)

- **本文系统研究了视觉推理中的长度泛化问题。受人类视觉系统局部注视的启发以及语言模型长度泛化研究的推动，作者设计了一组简单的视觉状态跟踪任务，要求模型通过聚合多个局部区域信息来完成推理。实验结果表明，采用全局编码的常规视觉模型（如ViT）会学习利用全局感知捷径，导致任务复杂度增加时泛化失败；而提出的一种基于严格局部感知的循环视觉策略，通过序列化局部眼动来获取信息，能够有效实现长度泛化。论文的主要贡献包括：揭示了视觉模型中的全局捷径问题；证明局部顺序感知是稳健组合泛化的必要条件；提供了一套评估视觉长度泛化的基准和消融分析。该工作为视觉模型架构设计提供了新视角，强调了局部注意力在泛化中的关键作用。**

<!-- paperflow:0e2b8fa8f8818bac -->
## Learning More from Less: Reinforcement Learning from Hindsight

[[Deep Reading - Jul 2026/Learning More from Less-Reinforcement Learning from Hindsight|Deep Reading]]

[https://arxiv.org/pdf/2607.09042](https://arxiv.org/pdf/2607.09042)

- **这篇论文提出LfH方法，解决VLA后训练中RL样本效率低的问题。在稀疏奖励下，初始策略的失败轨迹通常被直接抛弃，但这些轨迹可能完成了其他任务。LfH利用一个VLM对一组失败轨迹生成一个事后指令（描述它实际完成的行为），并对每条轨迹评分（衡量符合程度）。然后策略在原始样本和重标记样本上联合训练。这样，失败轨迹变成了成功演示，大幅提高了信号密度。在LIBERO-PRO基准上，LfH在样本效率上比标准RL提升5倍，并超过人工密集奖励基线。消融实验确认指令重标记和奖励重标记均有贡献。分析显示LfH保持了策略多样性，防止了GRPO常见的坍塌。真实机器人实验进一步验证了从仿真到真实的迁移效果。总体而言，LfH提供了一种新颖且实用的范式：通过语言接口将失败重新解释为成功，显著提升数据利用效率。**

<!-- paperflow:100e83cd7b7bf7ab -->
## Wan-Dancer: A Hierarchical Framework for Minute-scale Coherent Music-to-Dance Generation

[[Deep Reading - Jul 2026/Wan-Dancer-A Hierarchical Framework for Minute-scale Coherent Music-to-Dance Generation|Deep Reading]]

[https://arxiv.org/pdf/2607.09581](https://arxiv.org/pdf/2607.09581)

- **这篇论文介绍了 Wan-Dancer，一个专门为分钟级、高保真音乐驱动舞蹈视频生成设计的层次化扩散框架。针对现有模型在长视频生成中面临的计算复杂度高、身份不一致和节奏漂移等痛点，作者提出了一套系统性的解决方案。技术上，Wan-Dancer 通过全局关键帧规划确保了长程的编排连贯性，并利用局部细化模块保证了视觉细节。其核心贡献在于引入了时间映射的 RoPE 嵌入以实现精确的音画对齐，以及基于光流的损失函数来提升运动平滑度。实验结果令人印象深刻：模型能够稳定生成超过 60 秒、720p/30fps 的高质量视频，且在多种舞蹈风格上展现了极强的泛化能力。相比于之前的 SOTA 模型，Wan-Dancer 在保持人体结构完整性和动作节奏感方面取得了显著突破，为长格式多模态视频合成树立了新的标杆。**

<!-- paperflow:90cac8f138e184f0 -->
## SLVMBench: Skill Learning from Video Memory

[[Deep Reading - Jul 2026/SLVMBench-Skill Learning from Video Memory|Deep Reading]]

[https://arxiv.org/pdf/2607.11312v1](https://arxiv.org/pdf/2607.11312v1)

- **SLVMBench 是由来自（机构未知）的研究者提出的首个评估视频大语言模型从长视频记忆学习技能并实时应用的基准。该基准的核心是：模型被给予一个2-3小时的视频流，其中嵌入了一个2-3分钟的教学视频（演示某项技能，如折纸、烹饪等），其余为无关干扰视频。随后，模型需要观看一个新的视频片段，并实时回答有关该片段的问题，这些问题需要运用从教学视频中学到的技能。基准的关键设计包括：通过保证至少30分钟的干扰间隔来测试长期记忆；通过人工标注实现亚秒级时间对齐；问题设计避免常识性猜测确保必须运用技能；教程与问题配对以保证知识覆盖。论文评估了包括GPT-4V、Gemini、Video-LLaVA等在内的多种视频LLM，发现它们在技能学习任务上表现显著不足，特别是在长视频记忆条件下性能急剧下降。这表明现有视频LLM在从长期视频记忆中抽取和迁移程序知识方面存在严重局限。SLVMBench 作为第一个此类基准，为未来研究实时技能获取和长上下文视频记忆提供了平台。**

<!-- paperflow:2194559b7af809e1 -->
## MonkeyOCRv2: A Visual-Text Foundation Model for Document AI

[[Deep Reading - Jul 2026/MonkeyOCRv2-A Visual-Text Foundation Model for Document AI|Deep Reading]]

[https://arxiv.org/pdf/2607.11562v1](https://arxiv.org/pdf/2607.11562v1)

- **本文由来自华中科技大学等机构的研究者提出MonkeyOCRv2，一个面向文档智能的视觉-文本预训练基础模型。论文的背景是主流视觉编码器（如CLIP、DINO）在自然图像上预训练，缺乏对文档图像中密集文本和字符级细节的感知能力。为了解决这一问题，作者首先构建了迄今为止最大的文档预训练语料库MonkeyDoc v2，包含1.13亿张图像，覆盖17种语言，确保了数据的多样性和字符覆盖率。其次，提出了一种联合预训练策略：图像到文本生成（image-to-text generation）和像素级文档重建（pixel-level document reconstruction）。前者通过将图像特征解码为文本序列来对齐视觉和文本表示，后者通过重构图像像素来保留字符笔画和布局结构。这种双目标设计使得编码器能够同时理解文本内容并保持视觉细节。在实验部分，论文首先在五个经典文档分析任务上验证了MonkeyOCRv2的有效性：文本识别（CRNN从58.7%提升至67.3%），公式识别（110M的UniMERNet-T超越325M的UniMERNet-B），文本检测，文档篡改检测和重叠文本分割。所有任务中，简单替换原始视觉编码器为MonkeyOCRv2都带来了一致的性能提升，说明其特征具有良好的泛化性。更进一步，论文将MonkeyOCRv2作为多模态大语言模型（MLLM）的视觉编码器，在更复杂的文档解析和文档理解任务上测试。在文档解析方面，冻结的MonkeyOCRv2配合一个0.7B的轻量语言模型，在MDPBench基准上超越了之前最好的3B dots.mocr模型，绝对提升2.8%，而视觉编码器大小仅为后者的约1/11。在OmniDocBench上，该模型甚至超越了Qwen3-VL-235B和GPT-5.2等大型通用VLM。在文档理解方面，冻结的MonkeyOCRv2在八个基准上全面优于基于CLIP、DINO和SAM的对应模型。这些结果有力地证明了文档导向的视觉预训练可以成为文档智能领域的基础设施。论文还讨论了方法的局限性，如预训练计算开销，并计划开源代码和数据。总之，MonkeyOCRv2...**

<!-- paperflow:eed9c48839bb6f0b -->
## LightMem-Ego: Your AI Memory for Everyday Life

[[Deep Reading - Jul 2026/LightMem-Ego-Your AI Memory for Everyday Life|Deep Reading]]

[https://arxiv.org/pdf/2607.11487v1](https://arxiv.org/pdf/2607.11487v1)

- **该文介绍了LightMem-Ego，一个专为日常辅助设计的轻量级流式多模态记忆系统。论文指出，个人AI助手回答过去经历查询时需要持续积累和检索长期多模态记忆，但现有系统在流式处理和记忆层次上存在局限。LightMem-Ego提出层次化记忆架构，包括当前记忆（保留原始感官输入）、短期记忆（数小时内事件摘要）和长期记忆（情景记忆记录具体事件、语义记忆提取规律）。系统通过智能手机或AI眼镜摄像头和麦克风捕获第一人称视觉和音频流，后端调用API进行语音识别、视觉理解和语言生成，并将多模态数据在共享时间线上对齐。用户查询时，路由模块根据时间范围和类型决定检索层级，检索到的多模态证据作为上下文输入语言模型生成答案。能力对比表显示系统在记忆全面性上优于现有助手（如Siri、Google Assistant）和记忆系统（如MemGPT、Recall），并在真实设备上演示了物体查找、对话回忆、生活总结和个性化建议等场景。论文也讨论了依赖第三方API带来的系统级限制（如延迟、无网络不可用），并展望了端侧效率、鲁棒性、自适应记忆生命周期等未来工作。整体上，LightMem-Ego为可穿戴个人AI记忆系统提供了新架构设计和参考实现，代码已开源。**

<!-- paperflow:9e9c632d4d147da7 -->
## CoRe: A Comprehensive Framework for Cross-Image Comparative Reasoning in Vision-Language Models

[[Deep Reading - Jul 2026/CoRe-A Comprehensive Framework for Cross-Image Comparative Reasoning in Vision-Language Models|Deep Reading]]

[https://arxiv.org/pdf/2607.12786v1](https://arxiv.org/pdf/2607.12786v1)

- **本文针对视觉语言模型（VLM）在跨图像比较推理任务上的不足，提出CoRe（Comprehensive Comparative Reasoning）统一框架。首先，通过多专家协作流水线从结构化视觉元数据自动构建了大规模比较三元组数据集CoRe-20K，覆盖计数、深度、距离和空间关系四类比较任务。其次，设计了TriSR（Triplet Structured Reward）框架，在GRPO优化下联合监督属性定位、判断对齐和三重一致性，使模型学会进行结构化比较推理。最后，构建了首个专用基准CoRe-Bench，用于细粒度评测。实验部分，CoRe在CoRe-Bench上以Partial Accuracy指标超过最强基线28.2个百分点，诊断分析验证了各奖励组件的有效性，通用基准测试表明方法保持了通用能力。主要贡献包括：提出统一框架CoRe，系统性解决跨图像比较推理问题；构建CoRe-20K数据集，实现自动化、覆盖多种比较类型；设计TriSR结构化奖励，通过多信号联合监督提升推理质量；提出CoRe-Bench基准，填补评测空白。局限性主要体现在数据集覆盖属性有限、自动生成可能引入噪声、基准规模较小以及训练计算成本较高。未来工作可从属性扩展、鲁棒性、任务泛化等方向展开。**

<!-- paperflow:4f0e6462a8e0e476 -->
## UniVR: Thinking in Visual Space for Unified Visual Reasoning

[[Deep Reading - Jul 2026/UniVR-Thinking in Visual Space for Unified Visual Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2607.12800v1](https://arxiv.org/pdf/2607.12800v1)

- **UniVR论文首次系统研究了直接从纯视觉演示学习复杂推理、物理动态和长期规划的问题。提出VR-GRPO强化学习范式，通过全局和步骤级奖励确保推理一致性。构建大型基准VR-X。在多种任务上取得显著提升，证明了视觉空间推理的潜力。开源代码、数据和模型。论文论证了纯视觉推理的可行性，技术主线是RL优化序列预测，实验主线是基准评估和消融。**

<!-- paperflow:39bf899ffa59b280 -->
## Do We Really Need Multimodal Emotion Language Models Larger Than 1B Parameters?

[[Deep Reading - Jul 2026/Do We Really Need Multimodal Emotion Language Models Larger Than 1B Parameters|Deep Reading]]

[https://arxiv.org/pdf/2607.12787v1](https://arxiv.org/pdf/2607.12787v1)

- **本文围绕多模态情感识别（MER）的效率问题，系统论证了小于1B参数的轻量级模型通过知识蒸馏可以达到甚至超越大模型的效果。论文首先指出现有MLLMs在MER中表现优异但参数过大不可实用的根本矛盾，并提出核心疑问：是否真的需要>1B参数的模型？基于此，作者设计了Light-MER框架，采用8B教师蒸馏为0.6B学生，核心贡献包括：1) 提出SWD-H损失，使用切片Wasserstein距离对齐教师和学生的隐藏状态分布，优于传统的逐点MSE或KL散度，能够在分布层面传递知识。2) 引入基于GRPO的多奖励优化策略，同时优化情感分类准确率、生成描述质量和推理效率，使得学生模型在多目标上达到帕累托最优。3) 在九个标准数据集上进行了广泛的对比实验和消融研究，验证了SWD-H和GRPO各自以及组合的有效性。实验结果显示，SWD-H+GRPO实现的总体均值74.16优于其他蒸馏变体（KL-on-logits为73.49），且学生模型推理速度显著快于教师，同时隐藏状态对齐度高达99.2%。进一步的现象分析表明，输出分布对齐并不保证隐藏状态分布对齐，而SWD-H恰好弥补了这一差距。消融实验确认了每个组件的正向贡献。本文挑战了大规模模型在MER中必然最优的常规认识，证明了精心设计的轻量级蒸馏模型可以在保持高性能的同时满足资源受限平台的效率需求，为小型多模态情感语言模型的未来研究与应用奠定了基础。**

<!-- paperflow:3055e689af869f16 -->
## VGIF-Score: Interpretable and Diagnostic Evaluation of Spatio-Temporal Instruction Following in Video Generation

[[Deep Reading - Jul 2026/VGIF-Score-Interpretable and Diagnostic Evaluation of Spatio-Temporal Instruction Following in V|Deep Reading]]

[https://arxiv.org/pdf/2607.13527](https://arxiv.org/pdf/2607.13527)

- **本文针对视频生成评估中缺乏可解释性和诊断能力的问题，提出了VGIF-Score框架。该框架通过时空依赖图（ST-DAG）将指令分解为可验证的约束，并结合客观完成和主观满意度两个分支进行综合评估。同时构建了VGIF-Bench基准，包含200个精心设计的时空指令，覆盖多种依赖类型。实验在多个主流模型上进行，展示了框架的有效性和诊断价值。主要贡献包括：提出创新的评估框架、构建新基准、提供细粒度诊断分析。局限性包括依赖VLM可能引入偏差、基准规模有限等。整体而言，该工作为视频生成评估提供了新视角和实用工具。**

<!-- paperflow:b68ab448c2dc0a2b -->
## OvisOCR2 Technical Report

[[Deep Reading - Jul 2026/OvisOCR2 Technical Report|Deep Reading]]

[https://arxiv.org/pdf/2607.13639](https://arxiv.org/pdf/2607.13639)

- **OvisOCR2 Technical Report 提出了一个仅0.8B参数的高效端到端文档解析模型，其在公共基准上首次超越了复杂的pipeline方法，彰显了端到端路线的潜力。

**背景与动机**：文档解析（Document Parsing）是将文档页面图像转化为结构化、机器可读格式的关键技术。传统方法多采用OCR → 版面分析 → 表格/公式识别的pipeline，组件相互独立且错误可能累积。近年来，以视觉语言模型（VLM）为基础的端到端方法简化了流程，但性能往往不及精心调校的pipeline。同时，部署场景要求模型轻量化。因此，研究者希望在小模型中融合数据、训练和结构创新，使端到端模型成为最优选择。

**OvisOCR2设计**：模型基于VLM架构（推测为Qwen-VL的变体），输入页面图像，输出纯文本Markdown，自动保持自然阅读顺序。关键创新在于：
- **数据引擎**：为解决真实标注稀缺且噪声大的问题，构建了合成数据管道。从网络采集HTML页面，自动转换为Markdown并渲染为图像，形成天然对齐的训练对。进一步使用agent-based方法对HTML模板进行编辑和扩展，生成多样化页面，覆盖不同布局、字体、表格风格等。与此同时，也加入经质量控制的真实文档标注，提高数据真实感。
- **训练流程**：采用“两阶段（SFT+RL）训练+蒸馏+融合”的多步策略。首先在混合数据上分别SFT一个4B教师模型和一个0.8B学生模型。随后仅在4B模型上进行强化学习，奖励函数综合了文本、公式和表格的准确度，并通过on-policy hard case构造有效利用计算资源。RL后的4B模型作为教师，通过on-policy蒸馏（OPD）将知识传递给学生，蒸馏数据与RL数据一致。最后通过模型融合（可能是检查点平均或logit集成）进一步优化0.8B学生的权重。
- **基础设施优化**：针对多模态RL中的长文本、大图像和渲染式奖励计算，实施了分布式训练优化，包括奖励缓存、梯度检查点等。

**实验结果**：在OmniDocBench v1.6上，OvisOCR2取得整体9...**

<!-- paperflow:3425ff25f19de524 -->
## From Pixels to States: Rethinking Interactive World Models as Game Engines

[[Deep Reading - Jul 2026/From Pixels to States-Rethinking Interactive World Models as Game Engines|Deep Reading]]

[https://arxiv.org/pdf/2607.14076](https://arxiv.org/pdf/2607.14076)

- **本论文以“从像素到状态”为核心理念，重新审视了交互式世界模型的构造范式。作者将传统游戏引擎中的递归动作-状态-观察循环作为分析透镜，将当前数据驱动的交互式世界模型研究划分为四个独立但关联的维度：玩家动作控制（如何支持多样且持续的用户输入）、游戏状态动态（状态是否被显式建模并遵循规则更新）、状态-观察持久性（生成的视觉帧如何在长时间尺度上与底层状态保持一致）、实时交互生成（如何降低生成延迟并支持条件动态切换）。通过对现有文献的系统归类，论文揭示了当前方法的一个共性问题：绝大多数工作侧重于少数维度，且缺乏对显式游戏状态动态的深入建模，而这正是游戏引擎能够提供可重现、可交互体验的关键原因。导致这一现状的主要瓶颈在于缺乏高质量、帧对齐的动作-状态-观察三元组数据。为此，论文贡献了一个专门针对《黑神话：悟空》的数据引擎，采集了超过90小时的游戏实况视频，同步记录了玩家操作、引擎导出的结构化状态以及渲染图像。经过后处理和验证，该数据集为状态感知世界模型的训练和评测提供了稀缺的规模化标注资源。论文以该数据集为基础，展示了当前视频预测模型在状态一致性方面的不足，并呼吁未来工作应更积极地整合显式状态表示。整体上，论文既是一篇带有框架观点和数据集贡献的综述性论文，也为交互式世界模型从“像素级模仿”走向“状态级理解”提供了路线图。**

<!-- paperflow:79f40eed1a48fe24 -->
## MagicPrompt: Ultra-Lightweight Prompt Tuning for Video Generation

[[Deep Reading - Jul 2026/MagicPrompt-Ultra-Lightweight Prompt Tuning for Video Generation|Deep Reading]]

[https://arxiv.org/pdf/2607.14595v1](https://arxiv.org/pdf/2607.14595v1)

- **本文针对大规模视频扩散模型微调的计算瓶颈，提出MagicPrompt框架。核心包括：（1）注意力嵌入提示调优，在注意力层插入轻量级可学习提示，以极小参数开销调节生成；（2）双空间奖励反馈优化，引入自监督潜在损失提高条件引导训练的稳定性。MagicPrompt不修改预训练权重，仅训练提示参数（少于1%）。在文本到视频生成任务上，定性结果在语义对齐和运动动态上优于LoRA、VACE；定量指标接近或达到全微调水平，训练成本大幅降低。消融实验揭示token数量与性能的权衡。整体上，MagicPrompt为视频扩散模型高效适配提供极低参数量、稳定优化的解决方案。**

<!-- paperflow:5d8768d4e9852984 -->
## VideoChat3: Fully Open Video MLLM for Efficient and Generalist Video Understanding

[[Deep Reading - Jul 2026/VideoChat3-Fully Open Video MLLM for Efficient and Generalist Video Understanding|Deep Reading]]

[https://arxiv.org/pdf/2607.14935v1](https://arxiv.org/pdf/2607.14935v1)

- **本文提出 VideoChat3，完全开放、高效且通用的视频 MLLM。针对开源模型泛化、效率和开放性问题，引入 I3D-ViT 和自适应帧分辨率实现高效视频编码，并构建可扩展数据合成管道生成 Academic2M、LV116K、OL617K 数据集。仅 4B 参数即在多个基准上超越更大模型，实现泛化与效率平衡。作者开源权重、代码、策略和完整数据集，提供可复现基础。主要贡献：高效编码方案、大规模多样化数据集、完全开源。**

# Machine Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Machine Learning
- 方法：optimization, deep-learning
- 论文/报告：5 篇
- Denser $\neq$ Better: Limits of On-Policy Self-Distillation for Continual Post-Training
- The Optimal Sample Complexity of Learning Autoregressive Chain-of-Thought
- Where Should RL Post-Training Compute Go? Model Size, Search, Learning, and Feedback
- WanSong v1.0 Technical Report
- Beyond Entropy: Correctness-Aware Advantage Shaping via Contrastive Policy Optimization
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:43d174bb099a0b76 -->
## Denser $\neq$ Better: Limits of On-Policy Self-Distillation for Continual Post-Training

[[Deep Reading - Jul 2026/Denser $-neq$ Better-Limits of On-Policy Self-Distillation for Continual Post-Training|Deep Reading]]

[https://arxiv.org/pdf/2607.01763](https://arxiv.org/pdf/2607.01763)

- **本文主要研究on-policy自蒸馏在持续后训练中的实际效果和价值。作者首先指出持续后训练面临的知识获取与遗忘保持的矛盾，以及近期on-policy学习被认为能缓解遗忘的趋势。他们提出自蒸馏策略优化（SDPO）框架作为研究工具，在on-policy数据上实施密集的token级蒸馏训练。通过精心设计的单领域特化实验和多阶段持续后训练实验，论文获得了关键的对比结果：SDPO在教师信号稳定时能快速提升领域内任务表现，但无法推广到新分布且在持续学习中导致显著遗忘甚至崩溃；与之对比，on-policy RL方法GRPO收敛较慢但能可靠地保留旧知识。进一步分析发现，密集自蒸馏会增大参数和响应的漂移，并通过教师-学生循环放大高频格式伪影。基于这些现象，作者得出核心结论：on-policy数据本身不保证持续学习成功；密集自蒸馏的适用条件严格，不应作为默认稳定器。论文的贡献在于对乐观假设的实验性检验、开发了简单有效的诊断指标、并揭示了重要的失败模式，为后续研究提供了实证基础和警示。**

<!-- paperflow:591a8326bf3793f2 -->
## The Optimal Sample Complexity of Learning Autoregressive Chain-of-Thought

[[Deep Reading - Jul 2026/The Optimal Sample Complexity of Learning Autoregressive Chain-of-Thought|Deep Reading]]

[https://arxiv.org/pdf/2607.07423](https://arxiv.org/pdf/2607.07423)

- **本文在可实现的PAC学习框架下，分析了自回归思维链（CoT）精确迹线学习的样本复杂度。主要结果表明，样本复杂度由局部下一个词类的Daniely-Shalev-Shwartz维度决定，且与迹线长度无关。这一结果通过引入奇偶维度（parity dimension）得以证明，该维度是DS维度的展开稳定改进，通过低坐标跨越定理控制一包含密度。论文还展示了DS维度在自回归展开下可能增长，从而解释了为何需要奇偶维度作为中介。与先前工作（如压缩路线和在线学习方法）相比，本文提供了更紧的上界，且无迹线长度依赖，实现了最优的样本复杂度率。论文的主要贡献包括：建立样本复杂度与DS维度的直接联系；提出奇偶维度作为关键分析工具；并揭示DS维度在展开下的不稳定性。**

<!-- paperflow:f2762c24a411907a -->
## Where Should RL Post-Training Compute Go? Model Size, Search, Learning, and Feedback

[[Deep Reading - Jul 2026/Where Should RL Post-Training Compute Go-Model Size, Search, Learning, and Feedback|Deep Reading]]

[https://arxiv.org/pdf/2607.13389](https://arxiv.org/pdf/2607.13389)

- **本文针对强化学习后训练（RL post-training）中的计算资源分配问题，提出并系统研究了一个核心问题：在固定计算预算下，如何分配资源给搜索（每个输入的采样数）、学习（训练步数）以及反馈（奖励模型的计算成本）以最大化模型在下游任务上的性能。通过将问题类比于预训练的缩放定律研究，作者采用IsoFLOP实验框架，以数学推理作为代理任务，使用GRPO和LoRA对Qwen2.5模型进行后训练。实验分为三个阶段：第一阶段（Stage A）绘制搜索-学习分配的前沿，发现存在清晰的最优区域，搜索过多或过少都会降低性能；第二阶段（Stage B）比较结果奖励模型（ORM）、过程奖励模型（PRM）和共同评审（Common Judge）三种反馈机制，发现ORM在高搜索预算下显著优于PRM，而共同评审的偏好不一定与任务性能一致；第三阶段（Stage D）扩展到更大模型和额外评估基准，验证结论的鲁棒性。论文的主要贡献在于揭示了RL后训练中计算分配的结构化行为，提供了多种反馈机制的对比，并为实际系统优化（如机器人学习中的模块改进）提供了指导。局限性包括使用数学推理作为代理任务，可能不能完全代表机器人后训练场景；以及实验仅探索了有限的计算预算和算法设置。**

<!-- paperflow:979b39d2398a81f5 -->
## WanSong v1.0 Technical Report

[[Deep Reading - Jul 2026/WanSong v1.0 Technical Report|Deep Reading]]

[https://arxiv.org/pdf/2607.14749v1](https://arxiv.org/pdf/2607.14749v1)

- **本技术报告介绍了 WanSong v1.0，一个纯扩散的歌曲生成基础模型。它直接生成最长5分钟的多语言高保真歌曲，并同时输出人声和背景音乐双轨。采用单阶段流水线，避免多阶段级联的复杂性。通过步骤蒸馏实现快速推理，并支持微调和下游编辑。实验系统分析了 VAE 压缩比对重建质量的影响，并在客观指标上验证了模型优于类似压缩比的 Stable Audio 2。论文还讨论了数据筛选和强化学习微调（DPO/ReFL）。WanSong 旨在为商业级歌曲生成提供简单而强大的基线。**

<!-- paperflow:85f739ad056333a7 -->
## Beyond Entropy: Correctness-Aware Advantage Shaping via Contrastive Policy Optimization

[[Deep Reading - Jul 2026/Beyond Entropy-Correctness-Aware Advantage Shaping via Contrastive Policy Optimization|Deep Reading]]

[https://arxiv.org/pdf/2607.14614v1](https://arxiv.org/pdf/2607.14614v1)

- **本文提出了对比策略优化（CPO），一种针对RLVR中熵信号局限性的正确性感知优势塑造方法。通过理论和实验验证，参考引导与当前策略分布之间的token级对比分歧能可靠指示token正确性。基于此，CPO直接以该分歧作为优势更新策略，并自然解决了零优势问题，同时揭示在线蒸馏是特例。实验在多个推理基准上展示CPO显著优于熵方法，且支持有效的探索-利用平衡。贡献包括新信号、新框架、理论联系和实验验证。**
