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
- 方法：ai-for-science, explanation
- 论文/报告：2 篇
- AIriskEval-edu: New Dataset for Risk Assessment in AI-mediated K-12 Educational Explanations
- MoWorld: A Flash World Model
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

# Language Models

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Language Models
- 方法：agent, ai-for-science, language, reasoning, science-discovery, reinforcement-learning, optimization, retrieval
- 论文/报告：12 篇
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

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, ai-for-science, generation, language, reasoning, science-discovery, reinforcement-learning, optimization
- 论文/报告：30 篇
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
- 方法：generation, language, vision-language-model, vision, reinforcement-learning, multimodal-learning, stat-ml, vision-language
- 论文/报告：17 篇
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

# Machine Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Machine Learning
- 方法：deep-learning
- 论文/报告：2 篇
- Denser $\neq$ Better: Limits of On-Policy Self-Distillation for Continual Post-Training
- The Optimal Sample Complexity of Learning Autoregressive Chain-of-Thought
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
