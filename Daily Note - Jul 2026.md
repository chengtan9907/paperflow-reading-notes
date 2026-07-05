# On-Policy Distillation & Post-Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：On-Policy Distillation & Post-Training
- 方法：待从后续精读中沉淀
- 论文/报告：1 篇
- Purified OPSD: On-Policy Self-Distillation Without Losing How to Think
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:50e0973de38a0dfd -->
## Purified OPSD: On-Policy Self-Distillation Without Losing How to Think

[[Deep Reading - Jul 2026/Purified OPSD-On-Policy Self-Distillation Without Losing How to Think|Deep Reading]]

[https://arxiv.org/pdf/2607.02234](https://arxiv.org/pdf/2607.02234)

- **本文系统研究了 on-policy self-distillation (OPSD) 在长链推理模型上的失败现象，通过信号分解确定了根本原因：教师监督信号被参考答案的特定模式主导，导致学生机械记忆而非学习可迁移推理。基于此，提出一种轻量级的净化方法：构建 reference-only 教师以分离不可迁移成分，并利用 PMI 将剩余可迁移信号转化为蒸馏目标。实验在四个代表性长链模型和两个数学推理数据集上验证了一致提升，且未破坏模型的自然反思行为。论文不仅解决了现有 OPSD 在长链场景下的关键瓶颈，也为理解知识蒸馏中教师信号的组成提供了新视角。**

# Multimodal Models & Visual Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Multimodal Models & Visual Reasoning
- 方法：language, vision-language-model, vision, reinforcement-learning, deep-learning, vision-language
- 论文/报告：3 篇
- AdaBoosting Text Prompts for Vision-Language Models
- Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning
- Show Me Examples: Inferring Visual Concepts from Image Sets
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:a3c8bea8301ea2f3 -->
## AdaBoosting Text Prompts for Vision-Language Models

[[Deep Reading - Jul 2026/AdaBoosting Text Prompts for Vision-Language Models|Deep Reading]]

[https://arxiv.org/pdf/2607.00684](https://arxiv.org/pdf/2607.00684)

- **本文提出Text Prompt Boosting (TPB)，解决VLM少样本自适应中提示改进有限的问题。TPB将每个类别的文本提示池（包含模板和LLM生成描述）视为弱分类器，通过AdaBoost算法迭代选择并加权，重点修正前一轮错误样本，最终集成强分类器。实验在多个数据集和五个ViT-L级VLM上验证，显示TPB具有优异的shot可扩展性（随shots增加持续提升）和跨模型迁移鲁棒性。主要贡献包括：首次将boosting用于VLM提示集成，提出系统性的弱分类器构建与选择框架，并证明自然语言提示集成比连续提示更易迁移。局限包括依赖预定义提示池和boosting的额外计算开销。**

<!-- paperflow:303e62d796883732 -->
## Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning

[[Deep Reading - Jul 2026/Visually Grounded Self-Reflection for Vision-Language Models via Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2607.02490](https://arxiv.org/pdf/2607.02490)

- **本文针对视觉语言模型在思考链推理中自我反思缺乏视觉接地的问题，提出VRRL强化学习训练框架。方法通过随机轨迹掩码和缓冲回滚注入两个组件，迫使模型在训练中学会基于视觉反馈纠正错误。实验覆盖表格图表定位和空间导航任务，在分布外条件下取得显著提升。论文还分析了模型失效案例和组件贡献，展示了视觉接地反思的有效性。**

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
- 论文/报告：1 篇
- Optimizing Visual Generative Models via Distribution-wise Rewards
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:a12b6e3a0256529f -->
## Optimizing Visual Generative Models via Distribution-wise Rewards

[[Deep Reading - Jul 2026/Optimizing Visual Generative Models via Distribution-wise Rewards|Deep Reading]]

[https://arxiv.org/pdf/2607.02291](https://arxiv.org/pdf/2607.02291)

- **本文针对视觉生成模型RL微调中样本级奖励导致的奖励破解和模式坍塌问题，提出分布级奖励框架。首先指出样本级奖励使得所有生成样本独立优化，容易陷入局部最优并丧失多样性。为此，作者设计分布级奖励，将生成样本的整体分布与真实分布对比（如FID）作为奖励信号，从而鼓励多样性。由于直接计算完整分布的奖励成本过高，引入子集替换策略：维护一个固定大小的参考集，每次迭代仅用新生成的样本替换其中一部分，再基于该子集计算分布奖励，大幅降低计算量。此外，针对常规RL中SDE训练与ODE推理的不一致性，作者利用RL优化后验模型合并系数，避免性能差距。实验在SiT和EDM2两个扩散模型上验证，FID-50K获得显著改善，定性结果也展示了更好的感知质量，同时未牺牲多样性。论文还展示了后验合并优化的有效性。该工作为生成模型的对齐提供了一种稳健有效的奖励设计思路。**

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
- 方法：ai-for-science, generation
- 论文/报告：1 篇
- DisciplineGen-1M: A Large-Scale Dataset for Multidisciplinary Visual Generation and Editing
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:e464bf17ed8b5c23 -->
## DisciplineGen-1M: A Large-Scale Dataset for Multidisciplinary Visual Generation and Editing

[[Deep Reading - Jul 2026/DisciplineGen-1M-A Large-Scale Dataset for Multidisciplinary Visual Generation and Editing|Deep Reading]]

[https://arxiv.org/pdf/2607.02290](https://arxiv.org/pdf/2607.02290)

- **本文提出了 DisciplineGen-1M，这是目前规模最大、学科覆盖最广的学术视觉生成与编辑数据集。研究的核心逻辑在于：当前的生成模型缺乏对“结构化知识”的理解，导致在专业领域应用受限。为了打破这一瓶颈，作者设计了一套包含矢量渲染和程序化合成的自动化流水线，生产了 120 万个高质量样本。这些样本不仅包含精美的视觉图像，还附带了丰富的结构化标注和编辑指令。通过在这些数据上训练，模型在多个学科基准测试中刷新了纪录，并证明了学术数据对提升通用视觉推理能力的价值。该工作为 AI for Science 领域的视觉表达奠定了重要的数据基础，并为未来开发具备“专家级”绘图能力的 AI 助手指明了方向。**

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
- 方法：reasoning, retrieval
- 论文/报告：3 篇
- CheckRLM: Effective Knowledge-Thought Coherence Checking in Retrieval-Augmented Reasoning
- Rethinking Speech-LLM Integration for ASR: Effective Joint Speech-Text Training by Interleaving
- Measuring the Gap Between Human and LLM Research Ideas
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

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent
- 论文/报告：3 篇
- AgenticSTS: A Bounded-Memory Testbed for Long-Horizon LLM Agents
- PACE: A Proxy for Agentic Capability Evaluation
- SkillFuzz: Fuzzing Skill Composition for Implicit Intents Discovery in Open Skill Marketplaces
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

# AI Research

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Research
- 方法：待从后续精读中沉淀
- 论文/报告：1 篇
- What Types of Human-AI Teams Exist?
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:045cc92ab4e1cd65 -->
## What Types of Human-AI Teams Exist?

[[Deep Reading - Jul 2026/What Types of Human-AI Teams Exist|Deep Reading]]

[https://arxiv.org/pdf/2607.02198](https://arxiv.org/pdf/2607.02198)

- **本论文是一篇系统文献综述，旨在厘清当前人机团队研究中存在的不同类型及其特征。作者分析了53篇相关论文，从心理学团队分类视角出发，归纳出5个独特的团队集群：AI Assistant（AI作为辅助工具）、Ad-hoc Dependency（临时依赖）、Ad-hoc Forced Dependency（强制临时依赖）、Paired Equanimity（对等配对）和Group Equanimity（对等群体）。每个集群在团队规模、成员关系、任务依赖性、领导结构和沟通模式等维度上各有特征。研究发现，即使团队规模相同（如一对一），不同集群的特征组合也可能差异显著，这说明“人机团队”这一术语掩盖了重要的内部异质性，可能阻碍研究间的知识迁移。为此，论文提出了一套报告检查清单，要求研究者明确团队的关键特征，并呼吁采用更精确的分类语言。论文最后讨论了如何在更广范围内整合人机团队研究，包括跨领域比较和方法论改进。**
