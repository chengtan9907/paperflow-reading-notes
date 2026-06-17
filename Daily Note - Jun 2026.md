# Multimodal Models & Visual Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Multimodal Models & Visual Reasoning
- 方法：optimization
- 论文/报告：1 篇
- See First, Answer Later: Visual Evidence Pre-Alignment via Sufficiency-Driven RL
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:30806212efcd34f4 -->
## See First, Answer Later: Visual Evidence Pre-Alignment via Sufficiency-Driven RL

[[Deep Reading - Jun 2026/See First, Answer Later-Visual Evidence Pre-Alignment via Sufficiency-Driven RL|Deep Reading]]

[https://arxiv.org/pdf/2606.17678v1](https://arxiv.org/pdf/2606.17678v1)

- **论文提出了VEPA（Visual Evidence Pre-Alignment），一种插入在多模态大语言模型预训练与后训练之间的中间训练阶段，旨在解决标准训练范式中视觉对齐不足导致的“盲答”问题。VEPA的核心创新是使用充分性驱动的GRPO（Group Relative Policy Optimization），迫使MLLM生成问题条件化的视觉描述，并通过一个冻结的纯文本盲读器来验证该描述是否足以让推理模型恢复正确答案。这种“先见后答”的机制无需细粒度视觉标注，仅利用问答对隐式地监督视觉证据提取。实验在多个基准（ChartQA、GQA、A-OKVQA等）上显示VEPA能持续提升准确率，尤其在需要精确视觉理解的图表任务上改善显著（+7%）。反事实评估和问题仅分析证实VEPA模型确实增强了对视觉输入的依赖，减少了语言先验偏差。消融和鲁棒性实验进一步表明VEPA训练出的描述更简洁、信息密度更高。该工作为MLLMs的训练流水线提供了一个简单但有效的干预点，强调视觉感知基础对于后续推理的关键作用。**

# Reward Models & Reinforcement Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Models & Reinforcement Learning
- 方法：agent, reinforcement-learning, multimodal-learning
- 论文/报告：2 篇
- Context-Aware RL for Agentic and Multimodal LLMs
- A Unified Causal-Origin Taxonomy of Distributional Shifts in Reinforcement Learning
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:b45d05715ef85a7e -->
## Context-Aware RL for Agentic and Multimodal LLMs

[[Deep Reading - Jun 2026/Context-Aware RL for Agentic and Multimodal LLMs|Deep Reading]]

[https://arxiv.org/pdf/2606.17053v1](https://arxiv.org/pdf/2606.17053v1)

- **论文提出ContextRL，一种上下文感知的强化学习方法，旨在解决LLMs在长上下文和多模态任务中忽略关键证据的问题。核心思想是在标准结果奖励基础上，增加一个间接的上下文选择目标：给定查询、答案和两个相似上下文，模型需选择正确的上下文，从而迫使模型关注细粒度证据。数据构建针对两个领域：智能体轨迹（通过条件过滤生成1k对比对）和多模态图像（通过生成式编辑和相似性搜索生成7k对比对）。实验在5个长时智能体基准和12个VQA基准上进行，以GRPO为基线，ContextRL平均提升+2.2%和+1.8%。关键消融实验表明，相同对比数据如果用SFT或结果RL训练，增益消失，证明改进来自目标本身而非数据。论文讨论了计算开销（仅增加一个轻量头）和与更大模型的竞争力。结论认为上下文选择是简单且有效的辅助信号。**

<!-- paperflow:6459f9418c06cf56 -->
## A Unified Causal-Origin Taxonomy of Distributional Shifts in Reinforcement Learning

[[Deep Reading - Jun 2026/A Unified Causal-Origin Taxonomy of Distributional Shifts in Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2606.16933v1](https://arxiv.org/pdf/2606.16933v1)

- **该论文提出了一个统一的因果起源分类法，用于描述强化学习中的分布偏移。核心贡献包括：(1) 将数据集偏移原则从监督学习迁移到RL，基于POMDP分解交互过程；(2) 区分内部（智能体驱动的策略更新等）和外部（环境驱动的状态转移、奖励等）偏移；(3) 引入移时间边界概念，划分显式、隐式和混合偏移；(4) 统一了ID/OOD泛化与非平稳性视角，两者被重新解释为结构化过程变化。论文还提出了一个评估框架，通过性能下降和恢复指标衡量偏移影响。该分类法强调，不同因果来源的偏移可能需要不同的适应机制，因此区分来源对于设计鲁棒RL算法至关重要。论文的主要局限性在于对混合偏移的详细解释尚不充分，且未在真实RL任务中进行实证验证。**

# Data, Benchmarking & Evaluation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data, Benchmarking & Evaluation
- 方法：待从后续精读中沉淀
- 论文/报告：1 篇
- ATOM-Bench: A Real-World Benchmark for Atomic Skills and Compositional Generalization in Manipulation Policies
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:28a9d11dfcaab272 -->
## ATOM-Bench: A Real-World Benchmark for Atomic Skills and Compositional Generalization in Manipulation Policies

[[Deep Reading - Jun 2026/ATOM-Bench-A Real-World Benchmark for Atomic Skills and Compositional Generalization in Manipula|Deep Reading]]

[https://arxiv.org/pdf/2606.16826v1](https://arxiv.org/pdf/2606.16826v1)

- **论文提出ATOM-Bench，一个真实世界基准，旨在系统评估操作策略的原子技能获取和组合泛化能力。基准分解桌面操作为动作原子和指令原子，包含30个原子任务和24个保留组合任务，覆盖单臂和双臂场景。通过3000个人类演示和2700次物理部署，评估了五种代表性策略。实验发现当前策略在简单指令接地技能上表现良好，但在精细动作原子和需要对象选择/逻辑推理的指令上失败。更重要的是，强原子技能无法可靠迁移到组合任务，表明组合泛化是瓶颈。引入的Atomic Score和Compositional Failure Share指标有效诊断失败模式。该基准为理解操作策略泛化来源和指导改进提供了诊断工具。**

# World Models, Generation & Audio

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：World Models, Generation & Audio
- 方法：待从后续精读中沉淀
- 论文/报告：1 篇
- MaineCoon: Pursuing A Real-Time Audio-Visual Social World Model
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:aa81cf10f88b6b07 -->
## MaineCoon: Pursuing A Real-Time Audio-Visual Social World Model

[[Deep Reading - Jun 2026/MaineCoon-Pursuing A Real-Time Audio-Visual Social World Model|Deep Reading]]

[https://arxiv.org/pdf/2606.17800v1](https://arxiv.org/pdf/2606.17800v1)

- **本文首先提出社交世界模型的概念，指出当前视频生成模型忽视了社交平台中人类交互的实时性、音频重要性和动态延续性。为填补这一空白，作者构建了MaineCoon——一个220亿参数的实时音视频自回归生成模型，专为社交交互应用设计。模型采用因果Transformer架构，同时生成视频帧和音频，并通过四种创新训练技术：自重采样、跨模态表示对齐、域感知偏好优化和强化在线策略蒸馏，实现稳定高效训练。推理时引入智能体流式框架，包括缓存管理和提示规划，支持千秒级生成而不显著退化。在自主构建的SocialVideo-Bench基准上，涵盖7个社交领域、9个指标，MaineCoon在所有指标上超越现有双向和流式基线，并在单H100 GPU上达到47.5 FPS，实现首个实时音视频生成。本文还讨论了范式转变：从物理世界模型转向社会世界模型，强调主动观察、内部模拟和实时反应的重要性。虽然目前原型仅具备反应式生成能力，但为未来AI原生社交平台奠定了基础。**

# Research Agents & Scientific Discovery

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Research Agents & Scientific Discovery
- 方法：agent, gui-agent
- 论文/报告：1 篇
- LabOSBench: Benchmarking Computer Use Agents for Scientific Instrument Control
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:96382f1594cbeb23 -->
## LabOSBench: Benchmarking Computer Use Agents for Scientific Instrument Control

[[Deep Reading - Jun 2026/LabOSBench-Benchmarking Computer Use Agents for Scientific Instrument Control|Deep Reading]]

[https://arxiv.org/pdf/2606.16802v1](https://arxiv.org/pdf/2606.16802v1)

- **论文提出了 LabOSBench，一个基于 Web 的科学仪器模拟器基准，用于评估多模态 GUI 代理在科学仪器控制任务上的能力。基准包含 8 种常用仪器的模拟器，涵盖 96 个子任务，分为样品处理、对准、配置、执行和完成五大类别。任务设计保留了科学仪器操作的复杂性：密集界面、程序依赖、连续参数调节和视觉反馈闭环。评估了包括 GPT-5.5、Claude Opus-4.5、Seed-1.6、GTA1 等 11 个基线模型/框架。关键发现：现有代理在结构化、离散的 GUI 子任务上表现较好（如样品处理和配置中的简单步骤），但在需要科学状态理解、视觉反馈驱动调整和长程工作流的任务上显著失败。例如，SEM 聚焦调整需要迭代优化图像清晰度，只有少数模型能有效完成。端到端评估显示代理在简单工作流上可达 60-80% 成功率，但在复杂工作流上几乎完全失败。论文揭示了科学仪器控制作为计算机使用代理评估中一个具有挑战性且尚未充分探索的场景，并提供了可复现的评估平台。主要贡献：1）首个面向科学仪器 GUI 控制的可执行基准；2）覆盖 8 种仪器的任务套件；3）系统评估揭示当前代理的局限。**

# Web, GUI & Computer-Use Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Web, GUI & Computer-Use Agents
- 方法：agent
- 论文/报告：1 篇
- MyPCBench: A Benchmark for Personally Intelligent Computer-Use Agents
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:16f1a1df7aa5184e -->
## MyPCBench: A Benchmark for Personally Intelligent Computer-Use Agents

[[Deep Reading - Jun 2026/MyPCBench-A Benchmark for Personally Intelligent Computer-Use Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.16748v1](https://arxiv.org/pdf/2606.16748v1)

- **The paper identifies a gap in existing computer-use agent benchmarks: lack of personalization. To address this, the authors introduce MyPCBench, a benchmark comprising a reproducible Linux desktop environment with 17 simulated web applications, all seeded with data for a fixed persona (Michael Scott). 184 tasks are sourced from real user requests. They evaluate six models using a uniform computer+bash tool surface. Claude Opus 4.6 achieves the highest success rate (55.4%), with failures concentrated on cross-application and long-horizon tasks. Three family-shaped failure patterns are identified. The benchmark and associated resources are released to encourage research on personalized computer-use agents.**

# Agent Skills, Harness & Tooling

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Skills, Harness & Tooling
- 方法：agent, language, reinforcement-learning, optimization
- 论文/报告：4 篇
- daVinci-kernel: Co-Evolving Skill Selection, Summarization, and Utilization via RL for GPU Kernel Optimization
- OpenClaw-Skill: Collective Skill Tree Search for Agentic Large Language Models
- SkillWiki: A Living Knowledge Infrastructure for Agent Skills
- REFLEX: Reflective Evolution from LLM Experience
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:83ca97686712a303 -->
## daVinci-kernel: Co-Evolving Skill Selection, Summarization, and Utilization via RL for GPU Kernel Optimization

[[Deep Reading - Jun 2026/daVinci-kernel-Co-Evolving Skill Selection, Summarization, and Utilization via RL for GPU Kernel|Deep Reading]]

[https://arxiv.org/pdf/2606.16497v1](https://arxiv.org/pdf/2606.16497v1)

- **This paper introduces daVinci-kernel, a reinforcement learning framework for GPU kernel optimization that addresses the challenge of maintaining and evolving optimization knowledge as the model's capability improves. The key insight is that as the policy becomes more skilled, the nature of required optimizations shifts, making static skill libraries ineffective. daVinci-kernel co-evolves skill selection, summarization, and utilization through three jointly trained agents sharing an LLM backbone: a Skill Selection Agent that retrieves relevant techniques using BM25 and LLM reranking, a Policy Agent that generates multi-turn CUDA/Triton kernels conditioned on selected skills, and a Skill Summary Agent that distills successful rollouts into reusable skills. Skills are added only after execution-based verification ensures reproducible speedups. The agents are initialized via diverse SFT and...**

<!-- paperflow:c5df5afade7bc903 -->
## OpenClaw-Skill: Collective Skill Tree Search for Agentic Large Language Models

[[Deep Reading - Jun 2026/OpenClaw-Skill-Collective Skill Tree Search for Agentic Large Language Models|Deep Reading]]

[https://arxiv.org/pdf/2606.16774v1](https://arxiv.org/pdf/2606.16774v1)

- **本文针对LLM智能体在OpenClaw等现实系统中的技能构建难题，提出Collective Skill Tree Search (CSTS)框架。CSTS通过集体智能（多模型协作）自动构建结构化技能树：先分解任务为子任务，再迭代生成多样化候选技能（CSN-Gen）并评估质量与可迁移性（CSN-Assess）。基于该树构造SFT数据训练模型，进一步提出Collective Skill Reinforcement Learning (CSRL)，通过比较不同技能下的轨迹来拓宽解空间。实验在QWENCLAWBENCH和PINCHBENCH上表明，基于Qwen3.5-4B和9B的OpenClaw-Skill模型取得一致显著提升，在长程规划、工具使用和泛化上达到或超越强闭源模型。论文贡献包括：首次将树搜索引入技能构建，集体智能保证多样性与可迁移性，CSRL增强探索。**

<!-- paperflow:41681f737abee2e8 -->
## SkillWiki: A Living Knowledge Infrastructure for Agent Skills

[[Deep Reading - Jun 2026/SkillWiki-A Living Knowledge Infrastructure for Agent Skills|Deep Reading]]

[https://arxiv.org/pdf/2606.16523v1](https://arxiv.org/pdf/2606.16523v1)

- **论文提出SkillWiki，一个用于智能体技能的统一活知识基础设施。目标是解决当前智能体技能缺乏大规模生产、治理和演化基础设施的问题。SkillWiki将异构知识（轨迹、文档、API规范、脚本、历史技能）转化为可复用、可执行、可验证、可版本化的技能资产，并通过知识图谱关联技能与原始证据。它提供了一个完整的生命周期治理框架，覆盖从生产到归档的状态转换（S0-S7）。实验使用125个人工制品验证了知识到技能的生产可行性，并通过一个API文档案例演示了端到端生命周期管理。论文贡献包括：统一基础设施、知识接地技能构建、生命周期治理框架、开源实现。局限性包括：依赖LLM生成质量、技能质量评估尚未全面涉及、大规模部署验证不足。**

<!-- paperflow:5230cbc3b2c62d54 -->
## REFLEX: Reflective Evolution from LLM Experience

[[Deep Reading - Jun 2026/REFLEX-Reflective Evolution from LLM Experience|Deep Reading]]

[https://arxiv.org/pdf/2606.16496v1](https://arxiv.org/pdf/2606.16496v1)

- **REFLEX论文针对LLM引导进化搜索中诊断与修复耦合的问题，提出了一个解耦架构。核心贡献在于：1) 将视觉行为诊断（Critic）与代码生成（Actor）分离，使得每次变异的理由透明可审计；2) 引入Skill Memory，存储从搜索中提炼出的可复用控制哲学，支持跨运行知识迁移，显著提升样本效率。方法上，REFLEX是一个免训练进化框架（无需额外训练LLM），Critic接收当前策略的视觉行为证据（如状态轨迹图、奖励曲线），输出结构化的诊断文本（如“奖励过早停滞，需要更激进的探索”）；Actor基于诊断和Skill Memory中的代码片段生成子代策略。Skill Memory随着进化动态更新，自动积累高质量代码模式。实验在Lunar Lander、Acrobot、Pendulum控制基准和36维天线阵列合成任务上进行。结果显示：REFLEX在Acrobot和Pendulum上10次LLM调用内即找到可行策略，在Lunar Lander上达到1.092的归一化加权分数（接近MLES的1.098但收敛更快）；在天线任务中，REFLEX-best设计副瓣电平低于经典Dolph-Chebyshev和Taylor锥度。消融实验验证了Critic和Skill Memory各自的关键作用：去除Critic导致性能下降和审计缺失；去除Skill Memory则失去跨任务转移能力。热身启动实验表明，用先前运行积累的Skill Memory初始化新运行可大幅加速早期搜索（第2次评估收益Δ=+1.327）。此外，REFLEX还提供了完整的变异轨迹文本，增强了可解释性。总体而言，REFLEX以简单有效的解耦思路，在保持或提升最终性能的同时，大幅提高了进化搜索的透明度和样本效率，为可审计AI策略搜索提供了新范式。**

# Memory, Personalization & Long-Horizon Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Memory, Personalization & Long-Horizon Agents
- 方法：agent, generation, vision
- 论文/报告：1 篇
- MemSlides: A Hierarchical Memory Driven Agent Framework for Personalized Slide Generation with Multi-turn Local Revision
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:49078c443cf16e7a -->
## MemSlides: A Hierarchical Memory Driven Agent Framework for Personalized Slide Generation with Multi-turn Local Revision

[[Deep Reading - Jun 2026/MemSlides-A Hierarchical Memory Driven Agent Framework for Personalized Slide Generation with Mu|Deep Reading]]

[https://arxiv.org/pdf/2606.17162v1](https://arxiv.org/pdf/2606.17162v1)

- **This paper identifies the gap that current slide generation systems lack persistent personalization for multi-turn revision. MemSlides addresses this with a hierarchical memory framework: user profile memory (intent-conditioned profiles for round-0 alignment), working memory (active preferences and session constraints across turns), and tool memory (reusable execution knowledge for reliable local edits). The local revision mechanism modifies only the smallest affected slide region, avoiding full regeneration. Experiments on a controlled multi-persona profile bank show that user profile memory improves persona alignment across multiple LLM backends (GLM-5, Gemini 3.1 Pro, GPT-5) compared to SlideTailor and DeepPresenter baselines. Tool memory ablation on diagnostic modify pairs demonstrates higher completion and verification rates. Qualitative cases confirm working memory preserves user p...**

# Language Models

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Language Models
- 方法：language
- 论文/报告：2 篇
- Do Large Language Models Always Tell The Same Stories?
- Nothing from Something: Can a Language Model Discover 0?
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:f851abb0bab019b4 -->
## Do Large Language Models Always Tell The Same Stories?

[[Deep Reading - Jun 2026/Do Large Language Models Always Tell The Same Stories|Deep Reading]]

[https://arxiv.org/pdf/2606.17350v1](https://arxiv.org/pdf/2606.17350v1)

- **This paper addresses the contested question of whether LLMs generate diverse narratives by focusing on narrative similarity as a proxy. The authors collect human-written and LLM-generated stories from 10 models (including GPT-4, Gemini, Claude, Qwen, Llama, etc.) using prompts from r/WritingPrompts. A contrastive similarity annotation framework (Hatzel et al., 2026) is employed, where annotators judge which of two stories is more similar to a reference. Both human evaluations and three automatic embedding-based methods are used. The key finding is that LLM stories are significantly more similar to each other than human stories, with frontier models showing the highest homogeneity—effectively converging to a generic 'mean' narrative that approximates individual human writing but lacks collective diversity. Common mitigation strategies (negative prompting, temperature scaling) are shown to...**

<!-- paperflow:134298f1f091097c -->
## Nothing from Something: Can a Language Model Discover 0?

[[Deep Reading - Jun 2026/Nothing from Something-Can a Language Model Discover 0|Deep Reading]]

[https://arxiv.org/pdf/2606.17289v1](https://arxiv.org/pdf/2606.17289v1)

- **本论文以“零”的发现为案例，系统研究了语言模型在数学概念上的强外部泛化能力。论文首先指出，尽管当前AI在数学推理基准上表现出色，但尚未证明其能超越训练数据中的结构——即实现真正意义上的数学发现。受认知科学中Piaget和Fodor理论的启发，作者设定了一个严格控制的加法任务：训练数据中不出现数字0（除进位位外），测试模型在需要输出0或包含0的题目上的表现。实验采用GPT-2规模模型，并在多种预训练条件下进行对比。主要发现包括：零样本时所有模型完全无法泛化（准确率≈0%）；但通过加入少量（16-256个）包含零的示例进行微调，模型能够快速学习使用零，其中语言预训练模型（尤其是未过滤语料）表现出更强的学习效率。论文还在八进制下验证了结论的稳健性，并分析了数字表示相似性与进位操作对结果的影响。总体而言，这项工作为评估语言模型的概念发现能力提供了一个清晰的范式，并揭示了语言预训练在支持这种外推中的潜在作用。论文对未来研究如何实现真正的数学发现、以及如何将认知科学理论引入机器学习提出了重要问题。**

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, reasoning
- 论文/报告：3 篇
- VeriGraph: Towards Verifiable Data-Analytic Agents
- Can LLM Agents Infer World Models? Evidence from Agentic Automata Learning
- Agent trajectories as programs: fingerprinting and programming coding-agent behavior
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:19bec9a69602a0f0 -->
## VeriGraph: Towards Verifiable Data-Analytic Agents

[[Deep Reading - Jun 2026/VeriGraph-Towards Verifiable Data-Analytic Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.16603v1](https://arxiv.org/pdf/2606.16603v1)

- **该论文针对LLM数据分析智能体输出不可验证的问题，提出了VeriGraph框架。核心思想是将智能体的执行过程重构成一个增量构建的异构证据有向无环图（DAG），其中节点包括原始数据、变量、计算结果和自然语言声明，边通过三种原语操作（计算扩展、基础扩展、推导扩展）连接。这种结构化表示使得每项结论的溯源简化为图可达性检查，从而支持细粒度的审计。为了训练智能体构建高质量的证据图，论文设计了两阶段训练：首先通过原子SFT让模型学会执行基础的图原语操作，然后通过轨迹SFT学习完整的图构建过程，最后使用基于图的策略优化（复合奖励函数）进一步微调。复合奖励联合监督答案正确性（最终答案得分）、计算完整性（每一步代码执行是否正确）和推导一致性（每个推导步骤是否合理）。实验在四个涵盖QA、数据分析和多步研究的基准上进行。结果表明，VeriGraph-8B在总体得分上超越所有基线（包括CodeAct和ReAct强变体），同时其Grounding Rate高达87.61%，这意味着近九成的最终声明可以直接从证据图中恢复。消融研究揭示了各项训练组件的重要性，其中轨迹SFT对平衡准确性与可审计性最为关键。图分析显示，图拓扑反映了任务类型和难度，且图大小与任务难度正相关，验证了图作为内部难度信号的有效性。此外，框架在不同骨干模型上表现一致，表明其普遍适用性。论文的结论是，将证据结构作为第一类优化目标（而非后验解释）显著提升了智能体的可验证性。**

<!-- paperflow:bff93cb735811f6f -->
## Can LLM Agents Infer World Models? Evidence from Agentic Automata Learning

[[Deep Reading - Jun 2026/Can LLM Agents Infer World Models-Evidence from Agentic Automata Learning|Deep Reading]]

[https://arxiv.org/pdf/2606.16576v1](https://arxiv.org/pdf/2606.16576v1)

- **本文提出智能体自动机学习（agentic automata learning）作为评估LLM智能体交互式发现能力的正式框架。通过将隐藏环境建模为确定性有限自动机（DFA），智能体通过成员查询和等价查询逐步揭示目标。该框架提供可控复杂度、可量化效率以及经典算法（如L*）作为基准。实验评估了多个前沿LLM，发现性能随自动机规模增加而急剧下降：2状态时多数成功，20状态时仅推理模型GPT-5.4 reasoning有少量成功。推理模型整体优于非推理模型，但轨迹分析揭示其在查询规划（如重复提问、过早发起等价查询）、证据整合（忽略反例、遗忘早期信息）和假设构建（归纳错误）上存在系统性缺陷。经典L*算法在所有条件下表现出色，凸显了当前LLM在形式化发现任务上的不足。论文贡献包括：形式化新框架、系统评估多个LLM、揭示具体失败模式、为未来智能体诊断提供可扩展测试床。**

<!-- paperflow:4cc069999f25707a -->
## Agent trajectories as programs: fingerprinting and programming coding-agent behavior

[[Deep Reading - Jun 2026/Agent trajectories as programs-fingerprinting and programming coding-agent behavior|Deep Reading]]

[https://arxiv.org/pdf/2606.16988v1](https://arxiv.org/pdf/2606.16988v1)

- **本文以代码智能体为对象，系统性地提出了从过程层面理解和比较智能体行为的新范式。核心论点：传统以结果为中心的基准无法捕捉解决问题的风格差异，而轨迹中蕴含丰富的过程信息。作者首先定义了程序指纹（procedural fingerprint），即从智能体原始轨迹中通过涌现词汇归纳获得的高阶抽象表示，能够区分不同模型的行为习惯。通过对SWE-Bench上十种智能体的实验，验证了指纹的可归属性（85.7%准确率）、模型间相似性与发布时期和蒸馏关系的关联（如教师-学生对JS散度仅0.25），并识别出每种智能体的独特行为模式。进一步，作者开发了ProcGrep库，提供自顶向下的轨迹搜索能力，在情节搜索上超越LLM方法的效率和确定性。应用方面，展示了利用早期轨迹的部分指纹预测最终成功率，证明过程线索可以用于减少计算浪费。论文还讨论了该框架在任务路由、监控和成本分析等方面的潜力。总体而言，该工作为智能体行为的可解释性、可比较性和可编程性提供了工具基础和理论验证，弥补了从“智能体做了什么”到“如何做”之间的评估鸿沟。**
