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
- 方法：待从后续精读中沉淀
- 论文/报告：0 篇
- 暂无精读论文
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->



# Reward Models & Reinforcement Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Models & Reinforcement Learning
- 方法：待从后续精读中沉淀
- 论文/报告：0 篇
- 暂无精读论文
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->



# Data, Benchmarking & Evaluation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data, Benchmarking & Evaluation
- 方法：待从后续精读中沉淀
- 论文/报告：0 篇
- 暂无精读论文
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->



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
- Incremental Open-Ended Deep Research with Structured Harness
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:de40151693bafbe0 -->
## Incremental Open-Ended Deep Research with Structured Harness

[[Deep Reading - Oct 2026/Incremental Open-Ended Deep Research with Structured Harness|Deep Reading]]

[https://arxiv.org/pdf/2610.11566v1](https://arxiv.org/pdf/2610.11566v1)

- **论文针对深度研究报告的长期维护问题，提出Incremental-OEDR设定与Structured Harness框架。论证主线：现有OEDR系统一次性从零生成报告，缺乏对报告随时间演化的支持，导致重复劳动和连续性的缺失。技术主线：通过将报告表示为大纲-章节-证据的结构化集合，配合结构化检索、持久化证据池和结构化生成，系统能够在旧报告基础上进行选择性更新；这样的设计使智能体可以精准定位过时内容、复用有效证据、按需获取新信息，而不是每次重新开始。实验主线：构建了2016-2025十年时间评估框架，利用Tavily的end_date控制信息可见范围，设计了SST和LCT两种任务分别测试单步更新和长链维护；在DeepResearch Bench和DeepConsult上，使用开源配置（Qwen3.5-397B-A17B）和专有配置进行验证，从报告质量、报告连续性和研究成本三个视角评估。核心结果包括：相比一次性OEDR，Incremental-OEDR在内容级ROUGE-L F1提升0.51、大纲级EM F1提升0.63、token消耗降低33%、搜索调用减少61%，同时质量基本持平；时间维度分析显示OEDR性能随信息可用性增长而提升，说明时间因素对评估持续研究系统至关重要。论文还公开了项目页面，便于复现和扩展。整体上，这项工作将“研究报告维护”这一实际需求形式化为可评估的研究问题，并提供了有效的技术方案和评估框架。**

# Agent Skills, Harness & Tooling

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Skills, Harness & Tooling
- 方法：agent
- 论文/报告：1 篇
- AgentEvolver: System-Wide Self-Evolution Through Task Execution
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:a75e557d9c02d711 -->
## AgentEvolver: System-Wide Self-Evolution Through Task Execution

[[Deep Reading - Oct 2026/AgentEvolver-System-Wide Self-Evolution Through Task Execution|Deep Reading]]

[https://arxiv.org/pdf/2610.11613v1](https://arxiv.org/pdf/2610.11613v1)

- **AgentEvolver 的出发点是智能体领域的一个基本矛盾：一个 agent 可以完成任务，却没有改进自己做事的方式。作者认为问题出在缺少一个把“改变”连接到“评估”和“后续使用”的机制。为此他们设计了一个保持基础模型固定的系统，把任务执行变成能力演化的过程。系统层面，八类实体族——包括操作、方法、智能体、控制流、接口和支持性状态等——都通过统一的版本化生命周期接受修订；共享 Runtime 协调正在进行的工作；持久化规划保存任务方向；可恢复上下文保存支撑证据。这样一来，任务执行中产生的每次组件改动都被版本化、可评估、可复用，而不是随任务结束而丢失。评估方面，论文用 SWE-bench Pro Public 衡量任务结果，用六个应用案例（网站、游戏、研究工作）考察能力变化。团队报告演化配置达到 82.08% 的解析率，高于不演化基线；案例显示保留的能力进入了后续不同域的任务，同时也有未完成目标和一次失败策略的记录。论文最重要的概念贡献是把组件改进与任务成功分开：能力积累不应当用单次任务成功率来代理。作者坦诚列出两个开放问题：跨独立任务的可迁移性和总开发成本。整体来看，这是一个系统基础设施性质的贡献，价值在于为研究能力积累提供了一个可观察、可追踪、可复现的平台，而不是一个特定的算法或模型。**

# Memory, Personalization & Long-Horizon Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Memory, Personalization & Long-Horizon Agents
- 方法：agent
- 论文/报告：1 篇
- Memory Type Varies: Empowering LLM Agents for Long-Term Memory with Diverse Strategies
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:03f1f23e3a4ec610 -->
## Memory Type Varies: Empowering LLM Agents for Long-Term Memory with Diverse Strategies

[[Deep Reading - Oct 2026/Memory Type Varies-Empowering LLM Agents for Long-Term Memory with Diverse Strategies|Deep Reading]]

[https://arxiv.org/pdf/2610.11573v1](https://arxiv.org/pdf/2610.11573v1)

- **1) 论文背景与问题：LLM在长期对话中面临上下文窗口限制导致的“遗忘”问题，RAG-based记忆方法通过外部记忆库与选择性检索来缓解，但现有方法对所有记忆采用统一策略，忽略了记忆的异质性。作者观察到LongMemEval-S的t-SNE分布中存在明显的“信息耦合”与“语义鸿沟”，暗示不同记忆聚成不同簇，需要区别对待。
2) 核心主张：记忆可以并且应该被分类；不同记忆类型需要定制化检索策略；类型路由可以在多类混合语料上突破统一策略的性能上界。
3) TriMEM数据集：为了支撑类型化记忆研究，论文构建了TriMEM多类记忆数据集，基于认知心理学将记忆分为情景记忆、个人语义记忆和通用语义记忆，并为多种场景提供精确标注。标注标准的可操作性（例如情景记忆必须包含具体实例细节，一般习惯或模糊知识不算）是TriMEM质量的关键。
4) MemoType框架：MemoType由记忆类型路由器、查询类型路由器以及类型感知检索策略组成。它以当前查询为输入，可能先生成假想记忆，再对查询和记忆进行类型判定；情景记忆采用事件元素增强，个人语义记忆侧重语义关联，通用语义记忆使用相应知识检索；最后通过自适应剪枝模块筛选出与查询真正相关的内容供LLM生成回答。路由机制不仅提升了检索精度，还通过将昂贵操作限定在特定类型子集内降低了计算开销。
5) 理论贡献：论文证明任何单一检索策略在多类语料上的期望检索精度存在基本上界，这意味着只要语料是异质的，统一策略就注定无法达到最佳可能精度。该定理为类型感知检索的必要性提供了数学基础。
6) 实验验证：三个数据集上的主实验显示，MemoType在MPNet、MiniLM、QAMiniLM三种编码器下均显著优于HyDE、Mill、Query2Doc、SeCom、A-Mem、MemoryOS等基线，Recall@1最高提升16.18%。路由器在未参与训练的PerLTQA上表现良好，消融实验证明分类策略和剪枝模块均有贡献。论文还通过数据泄漏检验排除了模型记忆原始benchmark内容的嫌疑，增强了结果说服力。
7) 讨论与边界：论文承认目前只做了实例级别的记忆类型...**

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, ai-for-science, generation, language, reasoning, science-discovery, reinforcement-learning, retrieval
- 论文/报告：52 篇
- Do LLMs Learn from Rewards in Context? : Rethinking the role of reward in In-Context Reinforcement Learning
- AgentGarten: Code Worlds for Evolving Agents
- MiMo-V2.6: Scaling Reinforcement Learning Towards Self-Improvement
- SAIL: Scientific Agentic Intelligence via a Science-Aware Loop
- Agentic-TTT: Training test-time policy for test-time training
- DataSense-Bench: The First Step Toward an AI Scientist
- ILM: An AI-Powered Storytelling Educational Tool
- OneSearch-VL: Unified Multimodal Deep Research Agent for Image and Video
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:3a59cd5724354a1b -->
## Do LLMs Learn from Rewards in Context? : Rethinking the role of reward in In-Context Reinforcement Learning

[[Deep Reading - Oct 2026/Do LLMs Learn from Rewards in Context-Rethinking the role of reward in In-Context Reinforcement|Deep Reading]]

[https://arxiv.org/pdf/2610.11152v1](https://arxiv.org/pdf/2610.11152v1)

- **论文的核心主张是：被广泛接受的‘上下文强化学习（ICRL）’这一说法在 Direct ICRL 这一最简设定下并不成立，其行为模式更接近 ICL 而非 RL。论证主线从 ICL 的已知机制出发：ICL 对输入分布和表面形式敏感、对标签内容不敏感、对否定语义难以处理，这些已知模式为后续实验预测提供了理论坐标。技术主线是构建 Direct ICRL 设定（只给原始轨迹-奖励对），并在记忆累积与序列决策两个 regime 下进行系统干预：翻转奖励、随机化奖励、删除奖励字段、打乱/破坏轨迹、显式元提示（explore/exploit/reason/anti）。实验主线覆盖四个基准、六个模型，测量通道包括 episode reward 曲线、目标输出的逐 token log-probability 与 Target Replay Rate（每条件 500 条采样）。关键发现可归纳为五条：(1) Direct ICRL 确实带来超过 Random Sampling 的改进，效应真实；(2) 奖励被读取但效应很小——翻转、随机化、删除奖励均不改变改进曲线形状，最强干预（完全反转）也只造成微小差距；(3) 显式元提示（包括 Anti Prompt）无法激活奖励利用或负向信用分配；(4) 轨迹驱动改进但非通过语义内容——打乱或破坏轨迹效果不变；(5) 失败轨迹的回避信号微弱到可忽略（τ− 复现率在各条件下仅相差 0.1-0.2 个百分点）。论文把这些模式与 Min et al. 的 ICL 标签随机化发现直接对齐，得出结论：Direct ICRL 不是无参数 RL 系统，而是带有奖励字段的 ICL；因此以 RL 视角研究 ICRL 的既有框架（filtering、self-bootstrapping、reward-based design）需要被重新审视，agent 记忆设计应优先关注 ICL 已识别的核心因素——输入分布与表面形式。讨论部分还指出该结论对任务密集型场景的实践含义：奖励标签无法轻易替代显式记忆设计与训练，这与 ICL 中‘否定难以处理’的既有发现一致，并对 ICRL 的适...**

<!-- paperflow:3883705ac81af279 -->
## AgentGarten: Code Worlds for Evolving Agents

[[Deep Reading - Oct 2026/AgentGarten-Code Worlds for Evolving Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.12374v1](https://arxiv.org/pdf/2610.12374v1)

- **论文的核心论点是：智能体的能力受限于其练习环境；理想的训练环境必须同时具备“忠实性”（状态、规则、动态一致且可检查）和“真实性”（视觉观测符合真实世界分布），但现有模拟器/游戏引擎与视频世界模型分别只能满足其一，存在根本性权衡。AgentGarten 提出的解决方案是把两者耦合：每个场景程序运行在模拟器/游戏引擎中，以代码形式定义持久状态和交互规则，并通过统一接口导出结构化几何条件；一个共享的神经渲染器实时生成视觉观测，为智能体提供逼真的“眼睛”。这样，环境的物理/逻辑层是显式可编程的，而视觉层由数据驱动的渲染器补全，从而绕过了传统 3D 资产生成的高成本。神经渲染器的训练采用论文提出的 Adversarial Forcing：在自回归自强制目标基础上，通过精确重放（exact replay）让历史预填充变得可微，使后面预测的损失能够反向更新先前观测的编码；再加入真实视频的对抗监督，改善长程 rollout 的视觉保真度。推理侧通过手写 kernel、CUDA graph 和微型 decoder 实现实时性能（480×832 在 H100 上超过 35 FPS）。在智能体学习侧，智能体通过视觉观测感知世界并实时交互，每轮经验被蒸馏成 playbook，由后续智能体继承和细化，大幅提升样本效率：约 4 轮即可完成传统强化学习需要数百万轮的任务。实验还展示了多类场景（导航、操控、工具使用、车辆、多智能体 hide-and-seek）以及自博弈 RL 对照，说明框架的通用性。论文最后讨论了当前几何条件接口的冗余与不完备问题，并展望了用结构化文本或潜特征等更紧凑、更完整的条件表示来进一步推进。整体上，这篇论文提出了一种“代码世界 + 神经渲染器 + 经验迭代”的环境构建范式，目标是让环境和智能体以代码规模同步演化。**

<!-- paperflow:145312dc5e3d9f99 -->
## MiMo-V2.6: Scaling Reinforcement Learning Towards Self-Improvement

[[Deep Reading - Oct 2026/MiMo-V2.6-Scaling Reinforcement Learning Towards Self-Improvement|Deep Reading]]

[https://arxiv.org/pdf/2610.11959v1](https://arxiv.org/pdf/2610.11959v1)

- **本报告由小米LLM-Core团队发布，旨在通过大规模强化学习推进基础模型的递归自我改进。作者提出了MiMo-V2.6系列，包括MiMo-V2.6-Pro（1.02T总参数，42B激活参数）和MiMo-V2.6-Flash（310B总参数，15B激活参数），两个模型均为混合稀疏MoE架构，采用局部滑窗注意力和全局注意力交错设计，并配备轻量视觉和音频编码器，以支撑多模态智能体任务。

论文的论证主线是：要实现自我改进，模型必须拥有强大的基础能力（通过大规模预训练和面向智能体的中训练获得）以及一个可扩展的RL训练系统。为此，作者从三个维度扩展RL计算：一是扩大批次和吞吐，通过异步训练每步处理1568个样本和2.7~3.7B tokens，上下文长度达到1M，从而为长时任务提供足够的数据量；二是增加环境的多样性和复杂度，覆盖代码（如SWE-Bench）、通用工作流（如AutomationBench）、视觉设计（MiMo Visual Coding）和网络安全（MiMo Cyber Bench）四大领域，并混合多种智能体harness；三是增加分级器计算，引入组内智能体分级（GAR）和离线任务分析，用更准确的奖励信号替代二元测试奖励，并能引导模型生成更短、更token高效的解决方案。

为了在如此大规模下保持训练稳定，论文冻结了MoE路由器以减轻动态变化，并建立了多层防御对抗奖励黑客，包括行为正则化和在线/离线评分互补。基础设施方面，论文设计了统一轨迹表示（Sample→Sequence→Context→Segment）、高并发多框架rollout（样本混合器和CUDA graph限制延迟）、解耦控制与数据平面，以及训练-推理一致性机制，这些工程细节共同支撑了混合任务agentic RL的稳定运行。

实验部分，论文在多个公共和内部基准上报告了模型性能随RL训练步数的提升曲线，结果显示持续的优化收益。此外，基于检索到的片段，论文比较了在线与离线评分的策略行为，观察到在线评分产生更小、更精确、更易维护的补丁；分析了网络安全任务的难度本质（精确触发漏洞而非崩溃）；并提出离线任务分析作...**

<!-- paperflow:ef19248ebc88fc2d -->
## SAIL: Scientific Agentic Intelligence via a Science-Aware Loop

[[Deep Reading - Oct 2026/SAIL-Scientific Agentic Intelligence via a Science-Aware Loop|Deep Reading]]

[https://arxiv.org/pdf/2610.11451v1](https://arxiv.org/pdf/2610.11451v1)

- **论文围绕一个核心问题展开：如何构建一个开放的科学模型，使其在文献研究、科学编码和多步研究工作流中做出高质量的科学判断。作者指出，科学论文和代码库是训练信号的天然来源，但关键在于如何围绕模型仍缺失的能力来选择和组织这些资源。

论证主线：论文先从现有应用出发——语言模型已被用于规划实验室实验、生成和检验假设、搜索算法和数学构造——然后指出这些工作依赖训练模型在执行全程做出科学判断。两种不成功的搜索可能需要完全不同的干预：一种需要更广的探索，另一种需要在已检索的论文中做更好的选择；科学编码中程序跑通不等于方法正确，因为底层假设可能无效；长程研究中每步都对但整体失败，因为模型没有利用中间观察修订计划。这些失败决定了后续任务应该教什么，也决定了任务必须暴露什么信息。

技术主线：论文提出科学感知改进循环。循环分两个环节：前沿模型构成的 agent 诊断 SAIL 的能力缺口，并从论文集合和代码仓库构造训练任务。诊断覆盖文献任务（搜索与证据选择）、科学编码（假设与推理）、长程研究（规划与修订）。构造的任务包括问题、交互轨迹和带环境的可执行任务。循环重复多个开发周期。随后，SFT 负责基础行为，专家训练利用任务混合平衡各专家监督，多教师在线策略蒸馏（MOPD）把专家指导放在学生必须决策的状态上（包括不完美行动后），agentic RL 则进一步在状态化执行和在线轨迹中优化策略。教师路由与执行 harness 解耦，使同一资源集合支持不同粒度的训练。

实验主线：SAIL 在 SciCode 和 ArxivDIGESTables 上取得比较中的最高分；在 DiscoveryBench 上排名第二（37.48，较 base 提升 2.79）；在 DRB2 上排名第四（42.61，较 base 提升 10.34）；在 E2E-Bench 上排名第四。编码能力从科学编程问题扩展到执行现有研究仓库（SUPER-Expert、CORE-Hard）。总体结论是：SAIL 以 35B 总参数、3B 激活参数与显著更大的开源模型竞争。

论文的独特贡献在于把“失败诊断”作为训练数据生成的直接驱动，并...**

<!-- paperflow:447325cc72fb3aa0 -->
## Agentic-TTT: Training test-time policy for test-time training

[[Deep Reading - Oct 2026/Agentic-TTT-Training test-time policy for test-time training|Deep Reading]]

[https://arxiv.org/pdf/2610.12002v1](https://arxiv.org/pdf/2610.12002v1)

- **本文提出 Agentic-TTT，目标是让 LLM 在部署后能够自主决定如何从自身经验中学习，即实现参数级自我改进的 agency。论文从思想史切入：von Neumann 的存储程序架构和 Turing 关于“机器修改自身指令以从经验中学习”的论述，为自我改进提供了早期蓝图；但当代神经网络部署后参数冻结，部署数据只能影响计算而不能改变参数化能力。现代 self-improving agents 通过 reflection、memory、prompt 演化、workflow 搜索和技能积累在 scaffold 层面改进系统，但模型参数本身不变。

TTT 提供了一条参数级路径：用测试输入信号更新参数，把部署经验转化为参数更新。TTT 在预设场景（如 IMO 竞赛）中效果显著，但并非普遍有益：不同 TTT 算法适用于不同设置，选错方法会浪费测试时计算甚至损害性能。因此，作者提出需要模型级 agency：模型必须决定何时 TTT 值得、调用哪个算法、是否复用已有技能。这一空白此前未被探索。

技术路线上，Agentic-TTT 把五种 TTT 方法（POT、TTT-FS、TTRL、SEAL、TLM）封装为可调用工具，把累积的参数空间技能（LoRA）视为演化的部署环境，并训练一个测试时策略来管理决策。策略通过结构化动作（ADAPT action block）调用 TTT，harness 负责解析和执行；技能库支持跨查询复用，构建成本可摊销。训练分两阶段：先通过 guidance distillation（2,072 个 turn-level 示例、177 条轨迹）初始化策略，再用强化学习（187 个训练输入）以效用增益为奖励优化策略，采用 Dr. GRPO 风格的 mean-centered advantage，并设置调用惩罚 λinv=0.15。推理时策略 LoRA 与答案解码解耦，最终答案只由 backbone 生成。

实验覆盖 8 个域、11 个 benchmark 家族，评估流包含 176 个 held-out 查询，从空技能库开始。主实验表明：没有固定 TTT 方...**

<!-- paperflow:e46d1a30ad8e0a05 -->
## DataSense-Bench: The First Step Toward an AI Scientist

[[Deep Reading - Oct 2026/DataSense-Bench-The First Step Toward an AI Scientist|Deep Reading]]

[https://arxiv.org/pdf/2610.12190v1](https://arxiv.org/pdf/2610.12190v1)

- ****一、论证主线：从 RSI 叙事到可测的“数据感知”**
论文从 RSI 与 AGI 的广泛宣称出发，指出一个被复合型基准掩盖的基础问题：前沿模型能否在观察到结果之前，预判一个实验选择的价值。作者把这个问题锚定在训练数据选择上——一个既有后果又难判断的决策：可观测质量（低 loss、成功轨迹）并不直接等于训练价值，真正可靠的判断必须把数据属性与“特定模型在固定训练流程下会学到什么”联系起来。以“AI Scientist 的第一步”为定位，论文把数据感知拆成两个可测的子能力：识别高价值训练数据（top 子集训练后性能）和预测相对性能（排序准确率）。

**二、技术主线：DataSense-Bench 的协议设计**
基准的核心是一组“信息不对称”的约束：智能体可以检查数据、写并执行分析代码、跑基座模型的前向传播，但不能训练模型，也不能访问评测任务。这样既排除了作弊与昂贵试错，又保留了让智能体构造任意数据信号的自由度。基准在两个场景实例化：终端问题求解（候选来自 OpenThoughts-Agent 轨迹，下游 TBLite）与工具使用（候选来自 EnvScaler 轨迹，下游 BFCL V3）。下游评测协议标准化：TBLite 为 100 任务 × 3 次尝试的 300 个等权分数，BFCL V3 为 4 类 × 200 case 的 800 case、3 个 seed。排序评估使用一致序指标（完全一致 +1、完全逆序 −1，推断为 Kendall tau 类）与 best-group 准确率（并列最高分有专门处理规则）。论文还报告了对 30 条执行轨迹的机制分析以及单 run case study（智能体先建结构特征和基座模型信号，再统计完成标记、parser 警告、响应格式等，产出 parquet 文件形成最终配方）。

**三、实验主线：普遍但有限的证据**
实验用一批前沿模型作为智能体（图注中可见 Fable 5/5.1、Opus 5、Sonnet、GPT-5.5/5.6、GPT-6、Astra、Gemini 3.5 Flash、Kimi K3/K3 Pro 系...**

<!-- paperflow:17c222af127da4be -->
## ILM: An AI-Powered Storytelling Educational Tool

[[Deep Reading - Oct 2026/ILM-An AI-Powered Storytelling Educational Tool|Deep Reading]]

[https://arxiv.org/pdf/2610.12064v1](https://arxiv.org/pdf/2610.12064v1)

- **1）论证主线。论文从伊斯兰知识传递传统出发：知识（ilm）地位崇高，历史上通过教学、研习与叙事（qasas）传承，《古兰经》本身以叙事承载引导与教训，因此叙事在教学法上具有特殊价值。数字技术使伊斯兰叙事内容的可获取性大增，但“可获取”不等于“有意义学习”；现有平台在结构化学习与理解支持上不足，尤其在阿拉伯语与多语言场景。已有研究表明，设计良好、有成人适当中介的数字先知故事讲述能促进道德/宗教品格养成与学习动机，这构成 ILM 的产品动机。
2）技术主线。ILM 是面向儿童与青少年的交互式平台，采用双管线架构：
管线 A（结构化知识）：管理员批准的阿拉伯语叙事文本经 KG 构造引擎处理，识别实体（先知、人物、地点等）与叙事关系并存入知识图谱。实体提及抽取采用主集成模型 + XLM-R safe-addition 补充，并经过 story-scoped 后处理与人工审核回环（不合格段落被撤回重处理）；抽取质量以两位专家 exact-span 匹配为对齐标准。基于 KG 可生成实体型/关系型问题，并通过可视化故事地图支持叙事探索。
管线 B（多语言检索增强）：叙事段落翻译后经人工审核，逐段索引为检索 chunk 并保持与源段落的映射；chunk 用 Cohere embed-multilingual-v3.0 嵌入，检索相关段落以生成多选题与开放式理解题。出题 prompt 被显式约束：不得添加 SOURCE 中没有的人名/事件/地点/细节，信息不足不出题，每次只写一个需儿童作答的开放题；冗余问题通过嵌入相似度（> 0.78）标记交人工复核，重试一次后仍冗余则丢弃。开放题答案由 LLM-as-a-Judge 对照检索段落与参考答案判定正误。
两个管线之外，《古兰经》内容作为独立富化层（数据源自 Tanzil Quran Text–Simple Clean v1.1，CC BY 3.0），关系由人工对照原文核验，为叙事补充来源支持的信息；学习者门户将两类问题与故事地图整合呈现。
3）实验主线。现有证据显示评估处在“组件校验 + 可行性演示”阶段：实体抽取设置了两专家 exact...**

<!-- paperflow:fa23319cd14e6ab4 -->
## OneSearch-VL: Unified Multimodal Deep Research Agent for Image and Video

[[Deep Reading - Oct 2026/OneSearch-VL-Unified Multimodal Deep Research Agent for Image and Video|Deep Reading]]

[https://arxiv.org/pdf/2610.12419v1](https://arxiv.org/pdf/2610.12419v1)

- **OneSearch-VL 是面向单图、多图与视频三类视觉输入的统一多模态深度研究智能体，核心贡献是把“视觉锚定 → 外部检索 → 事实组合”这一共享工作流抽象为 Visually Grounded Evidence Graph（VGEG）。
论证主线：论文从三个观察出发——(1) 三类深度研究任务虽视觉操作不同但工作流共享；(2) 已有证据表示（perception–knowledge chains、factual rubrics、evidence graphs）没有联合编码 grounding、support 与 operation 三类依赖；(3) 多图深度研究几乎未被研究。由此推出需要一个统一的任务级参考结构 VGEG，并让它同时服务数据构建、过程监督与评测。
技术主线：VGEG 编码三类节点（视觉锚点、外部事实、答案操作）与三类关系（grounding、support、任务依赖 Ri）。基于 VGEG 的数据引擎自动构建并验证多图/视频问题、过滤专家轨迹，产出 SFT-110K 与 RL-10K 两套数据；从 VGEG 标注推导 EVGR rubric 奖励，在 GRPO 中与 Racc、Rquery 组合，门控于格式有效性；训练采用 Qwen3-VL-8B 全参 SFT + RL 两阶段，统一工具环境（TextSearch/ImageSearch 等，dispatcher + 工具接口）贯穿专家轨迹合成、在线 rollout 与推理。训练细节：视觉塔与 MM projector 冻结，ZeRO-2，bfloat16，有效 batch 128，8 epochs，lr 2e-5，64 张 H800 训 4 天；RL 数据通过每条候选任务 8 次 rollout 筛选 0 < ncorrect < 8 的任务；策略损失只作用于模型生成 token，工具观测仅作条件上下文。
实验主线：单图用 OpenSearch-VL 的七个知识密集型基准（SimpleVQA、VDR、MMSearch、LiveVQA、BrowseComp-VL、FVQA、InfoSeek），多图/视...**

<!-- paperflow:5cf905e42d4fb0f7 -->
## MindFlow: Mind Supernet Powered Thinking Flows for Research Idea Innovation

[[Deep Reading - Oct 2026/MindFlow-Mind Supernet Powered Thinking Flows for Research Idea Innovation|Deep Reading]]

[https://arxiv.org/pdf/2610.11966v1](https://arxiv.org/pdf/2610.11966v1)

- **MindFlow 是一篇面向 AI for Science 的方法论论文，核心主张是：研究想法创新不应继续依赖预定义、静态的 prompt 或 agent 流水线，而应被显式建模为可控制、可优化的结构化思维过程。
论证主线：论文从研究想法创新的双重困难出发——生成困难与评估困难。idea 创新本质上是开放式（open-ended）问题，没有唯一正确答案；同时又是多目标（multi-objective）问题，需要在新颖性、合理性与可行性之间取得平衡。已有 LLM 方法通过 prompt 工程或 agent pipeline 取得进展，但固化的 ideation 工作流无法适配不同主题，也无法根据反馈迭代改进。论文由此提出三条期望性质：显式（将 ideation 表达为原子思维算子组成的结构化组合程序）、可控（按主题自适应选择组合算子）、可优化（在开放性多目标评价下持续改进算子组合，甚至支持用户自定义目标）。
技术主线：MindFlow 将 ideation 形式化为图结构的 Flow in Mind。图的节点是模块化思维算子（如批判性思维、反事实思维），边定义组合关系；一个概率思维超网络（mind supernet）对给定研究主题建模思维流分布，控制器（controller）从中动态采样并执行思维流以生成候选想法。优化层面，论文采用 tournament-based relative ranking：在候选想法间进行相对比较，将胜负信号转化为排序损失，使控制器逐渐偏好能产出高质量想法的思维流。这一设计避开了开放式问题缺乏绝对标签的难题。评估层面，论文提出联合评估 problem finding（问题发现，对应动机质量）与 problem solving（问题解决，对应方法设计）的协议，设计维度化指标并聚合为多目标得分（检索碎片中出现 IDEA 多目标得分与 MOScore 等表述），超越仅看标题或摘要的粗粒度判断。
实验主线：论文在多个不同研究主题上验证 MindFlow。报告 Problem Finding 与 Problem Solving 两类得分以及聚合后的多目标...**

<!-- paperflow:2ff53a3de60f14ae -->
## Reasoning-Informed Visual Editing

[[Deep Reading - Oct 2026/Reasoning-Informed Visual Editing|Deep Reading]]

[https://arxiv.org/pdf/2610.12343v1](https://arxiv.org/pdf/2610.12343v1)

- **本文的核心贡献是提出并系统研究了“推理驱动的视觉编辑（Reasoning-Informed viSual Editing, RISE）”这一任务。论文的论证主线是：大多模态模型虽然在理解与生成上各自取得显著进展，但在真正需要“理解之后再做修改”的编辑任务上仍缺乏可靠能力；现有评测只覆盖保真度和语义一致性，不足以刻画模型的推理型编辑能力。为此，作者构建了从 RISEBench 到 RISEBench++ 的层次化基准。

在任务设定上，RISEBench++ 将 RISE 能力拆成六大推理维度：时间推理、因果推理、空间推理、逻辑推理、反事实推理和混合推理。论文进一步给出一个非常清晰的能力分类学：时间与因果推理是知识驱动的（有相关知识即可较快推断目标状态）；空间与逻辑推理是感知驱动的（需要先从图像推断潜在结构或规则，再基于该表达推理）；反事实推理是想象驱动的（调用世界知识但刻意覆盖它，构造物理上不可能但连贯的状态）；混合推理则要求在多轮编辑中把多种推理模式叠加使用，考验长程一致性。六大维度继续分解为 12 个子类和 65 个细粒度任务类型，共 1000 个中英双语人工标注样本，支持单图、多图和多轮编辑三种输入设置。

在评测设计上，论文提出三维度框架：指令推理（是否理解并遵循指令中的推理要求）、外观一致性（是否保持原图的内容与身份特征）、视觉合理性（编辑结果是否在视觉/物理/因果上显得自然）。评测采用人工评估与 LMM-as-a-judge 结合的方式；论文为视觉合理性评测器报告了人类一致性校准结果（MAE 0.27、Spearman 0.41，合理推断来自检索片段），并因其成本优势选定为最终评测器。

在方法侧，论文提出 RISE-Agent，一个无需训练的智能体框架，由推理驱动的规划、工具增强的执行和验证器引导的修正三个模块组成。实验表明该框架在多种 RISE 任务上超过大多数强基线，说明“先想清楚、再动手改、改完再验证”的 agent 范式是当前无需重新训练即可提升推理型编辑能力的有效路径。

在实验主线上，论文对 58 个方法做了大规模横评：34 个开源模型、19...**

<!-- paperflow:c1614cc82a4332de -->
## ViSkill: Reinforcing VLM Agents with Evolving Visual-Native Skills

[[Deep Reading - Oct 2026/ViSkill-Reinforcing VLM Agents with Evolving Visual-Native Skills|Deep Reading]]

[https://arxiv.org/pdf/2610.12403v1](https://arxiv.org/pdf/2610.12403v1)

- **论文的论证主线：RL 训练 VLM 智能体已成为主流，但成功策略只隐式存在于策略参数中，智能体需要跨 episode 重新发现类似空间解，样本效率低；成功轨迹本身已编码可复用的程序性知识（绕障碍导航、多步物体操作协调），因此应显式保留并复用成功经验。但现有技能增强方法以文本为中心，把空间布局和动作-状态对应线性化为语言，是有损抽象；引入视觉证据的近期工作又把技能构建/更新与策略优化分离，未利用二者相互促进的潜力。ViSkill 的技术主线：把每条成功轨迹通过结构化渲染构建为复合视觉技能卡片（原始视觉观测 + 轨迹标注 + 策略描述的文本渲染在同一图像中，保留空间与程序结构），存入视觉原生技能库；推理时用几何描述符计算相似度，只有相关度超过阈值 δ_r 才检索技能卡片，直接追加进 VLM 视觉上下文；交互终止后二元成功指示同时驱动结果奖励、技能效用更新和蒸馏资格判定；技能对齐的失败轨迹获得有界部分信用（始终小于完成奖励），不扭曲全局奖励排序；成功轨迹在线蒸馏回库，形成技能积累与策略优化互相强化的闭环；可选冷启动机制加速早期学习；在给定轨迹奖励后用标准 RL 目标优化策略（收敛快于标准 PPO）。实验主线：在 Sokoban、FrozenLake、PrimitiveSkill（VAGEN 框架）上对比 8 个闭源模型、5 个开源模型/方法以及 VAGEN、Atlas-VA 两个技能方法；ViSkill 整体成功率 0.89、冷启动 0.91，全面最优；消融证实视觉表征、策略引导（检索）、联合优化对正反馈环路各有贡献；分辨率缩放与新颖性阈值实验揭示视觉保真度- token 权衡与库准入控制的敏感性。结论：通过保留空间几何结构并让技能积累与策略优化共同演进，ViSkill 弥合了文本中心技能方法的表征鸿沟，在三个基准上一致超越闭源与开源基线。局限：仅限 VAGEN 框架环境、技能引导奖励对不相交状态空间的解可能提供有限引导、技能库对低质量行为的约束不能替代开放场景安全验证。**

<!-- paperflow:29b386a318a8339d -->
## MENTOR: Learning from Failure with On-Policy Pedagogical Intervention

[[Deep Reading - Oct 2026/MENTOR-Learning from Failure with On-Policy Pedagogical Intervention|Deep Reading]]

[https://arxiv.org/pdf/2610.13594](https://arxiv.org/pdf/2610.13594)

- **论文《MENTOR: Learning from Failure with On-Policy Pedagogical Intervention》围绕大型推理模型（LRM）在 RLVR 训练中的“零奖励崩溃”问题展开。论文首先指出现代推理模型能够生成详细思维链并展现自我反思能力，其进步关键来源于 RLVR——用程序化反馈驱动多样推理路径的探索和策略的迭代精化。然而，当任务难度突然提升时，模型采样到的轨迹大多数得分为零，GRPO 等在线策略优化算法的优势估计随之失效，训练陷入停滞。图 1 用 DeepSeek-R1-Distill-Qwen-1.5B 的训练动态清晰展示了这一崩溃：零奖励比例升高，验证准确率在 0.45 附近停滞。

论文的核心论点是：失败轨迹并非废料，而是富含诊断信息的“认知缺口探测器”。丢弃它们等于丢弃了模型真实错误模式的实时线索，导致教学信号与失败根因错位。现有缓解方案——预定义提示库、事后细化，或 Critique-GRPO、SRPO、CTRL、Critique-RL 等——要么是静态离线干预，要么是事后反思，无法在失败发生的当下给出自适应的、向前看的引导。为此，论文提出范式转变：将教学干预从离线的、静态的改为 on-policy 的、动态的，并实现了 MENTOR——一个多智能体框架，扮演“实时导师”角色。

MENTOR 的流程包含三个阶段：CASF 负责从训练数据流中识别零奖励崩溃实例（接收问题、真实答案和四条失败轨迹）；FDCG 执行认知级诊断，通过归属代理等组件分析策略系统失败的根因，并生成具体的教学提示；最后将这些提示集成回训练循环，引导学习代理在下一次 rollout 中进行自我纠正。提示设计遵循“脚手架”原则，例如将模糊警告改写为具体命名检查，而不直接给出算术修正或最终计数。

实验部分在数学推理任务上展开。训练动态显示，相比 GRPO，MENTOR 显著降低了零奖励比例并提升了验证准确率，避免了性能停滞。在 Qwen2.5-Math-7B 上，MENTOR 相对静态基线实现了 50.0% 的相对增益，表格中还出现最高 100% 的...**

<!-- paperflow:9e880bb34da2858b -->
## Humanize MLE Agents: Multi-Agent Session Alternation Beyond Single-Agent Limits

[[Deep Reading - Oct 2026/Humanize MLE Agents-Multi-Agent Session Alternation Beyond Single-Agent Limits|Deep Reading]]

[https://arxiv.org/pdf/2610.13494](https://arxiv.org/pdf/2610.13494)

- **However, we find that as single-session execution lengthens, the agent's reasoning intensity declines alongside slowing progress, summarized as long-horizon deceleration, and the…

studies examine candidate transfer, learned control exchange, and the quality cost of continuing another model’s trajectory (Luo et al., 2026; HMA studies fixed-cap alternation of complete native agents on one shared workspace, with a context reset at every handoff.

quires persistent cooperative access to them. Assumption 3.2 (Cooperative access and use).

valuates the full task suite. Each agent session is modeled as an option, with experiments counted within it;

主要贡献包括：quires persistent cooperative access to them.；Assumption 3.2 (Cooperative access and use).；valuates the full task suite.。

需要注意的边界包括：establishing comparative gains beyond MLE-bench remains future work.；HMA alternates native agents, resetting...**

<!-- paperflow:7e9a27e418f087bc -->
## Work2Skill: Capturing Human GUI Experience for Reusable Agent Skill Induction

[[Deep Reading - Oct 2026/Work2Skill-Capturing Human GUI Experience for Reusable Agent Skill Induction|Deep Reading]]

[https://arxiv.org/pdf/2610.12984](https://arxiv.org/pdf/2610.12984)

- **论文的论证主线：计算机使用智能体能力提升但仍缺程序性知识 → 人类演示与智能体共享同一 GUI 交互空间，是程序性知识的直接来源 → 但演示的复用需要结合上下文解释动作、分离工作流规则与任务细节 → 因此需要采集+归纳的完整框架。技术主线：WorkRecorder（对齐屏幕录制、鼠标键盘事件、accessibility tree）→ WorkDemo-350（350 个筛选后的多模态演示）→ Agentic Skill Induction（按需检查证据+迭代撰写修订技能文本）。实验主线：四个执行模型 × 三种技能来源（No Skill / ShowUI-Aloha / Demo2Tutorial / Work2Skill）在 MyPCBench（64 任务）和 OSWorld-Verified（286 任务子集）上比较 Score、SR、Steps、Token。主要结果：Work2Skill 全面最高；MyPCBench 相对 No Skill 提升 4.67–10.54 分和 6.25–25.00 个百分点 SR；OSWorld 子集提升 3.11–7.50 分和 1.40–7.69 个百分点；相对两个演示基线分别提升 3.95–7.79 和 1.65–6.96 分。关键分析发现多模态证据和自适应探索的贡献，以及技能跨模型、跨任务变体的可迁移性；也发现归纳模型能力影响技能质量，但更强归纳不弥合执行器差距。结论强调：对齐的人类演示+灵活证据探索能产生可复用的程序性指导，执行时通过共享文本接口注入，无需访问原始录制。注意：论文部分引用和附录细节（如 Appendix D.5 模板）在检索证据中只部分可见，具体归纳 prompt 完整内容、数据集任务清单、部分执行模型名称需要回原文确认。**

<!-- paperflow:1954823fbdcd56a1 -->
## SOAR: Source-Aware Reinforcement Learning with Precise Credit Assignment for Search Agents

[[Deep Reading - Oct 2026/SOAR-Source-Aware Reinforcement Learning with Precise Credit Assignment for Search Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.13157v1](https://arxiv.org/pdf/2610.13157v1)

- **论文以 think-then-search 搜索智能体 RL 训练为背景，抓取了一个此前被归因为模型容量/数据偏差、实则为框架结构性缺陷的失败模式——Evidence Fabrication（证据伪造）。作者用 Figure 1(c) 给出三个代表性框架的 EFR 量化（Search-R1 16.0%、IKEA 13.4%、AutoRefine 11.1%），证明其普遍性。核心论点：推理轨迹的不透明性使得 RL 的 reward 无法区分忠实综合外部证据得到的成功与靠内部知识捷径侥幸得到的成功，于是 RL 非但不惩罚、反而强化了不忠实的推理。SOAR 的解法分两层：第一层用 Source-Aware Workflow 把 Eext 与 Eint 拆成并行可观察流（并用严格格式 prompt 提取 Eint、跨 GPU 并行外部检索降低开销）；第二层用 Source-Consistent Reward 强制模型自我声明的证据来源与实际推理一致，使 credit assignment 能在不同知识流间精确进行。实验在 3B/7B 基座上对比 Search-R1、IKEA、AutoRefine：主结果上 SOAR 分别提升 7.3%/5.4%、EFR 降低 74.8%；在 5% 污染训练（长尾查询正确证据被替换为误导信息）的鲁棒性测试中，SOAR 的 QA 平均分仅下降 0.019，EFR 仅上升 0.33%，各项均优于 baseline。消融表明即使去掉 Source-Consistent Reward，仅靠 source-aware workflow 也能把 EFR 相对 Search-R1 降低约 55%-64%，加上该 reward 后进一步降低到约 1.34%-6.92%（降幅 71%-82%）。整体上，SOAR 把证据来源从隐变量变成显式结构，把忠实性从隐式期望变成可优化目标，是搜索智能体 RL 训练范式层面的系统性修正。**

<!-- paperflow:8505899a7bb3ab82 -->
## State-OPD: State-Grounded On-Policy Distillation for Multi-Turn Agents

[[Deep Reading - Oct 2026/State-OPD-State-Grounded On-Policy Distillation for Multi-Turn Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.13238v1](https://arxiv.org/pdf/2610.13238v1)

- **论文研究多轮智能体场景下 On-Policy Distillation（OPD）的监督退化问题，并给出一个以 interaction state 为组织单元的新框架 State-OPD。论证主线是：现有 OPD 在 agentic 任务中沿学生完整历史（推理+动作+观测）构造教师目标，导致两类问题被混在一起——继承的推理扭曲教师监督、当前推理决定动作备选集；论文通过把 interaction state 定义为任务指令加已执行动作-观测序列（st = (g, o0, a0, o1, ..., at−1, ot)），将实现层面的事实与学生对其的解释分离，并以 Proposition 1 论证 st 对潜在环境状态的信息充分性。技术主线包含三个成分：机制诊断（在匹配状态下去除先前推理的受控实验，发现教师续写成功率提升、token 级目标改变、越靠后轮次收益越大；重采样完整响应的动作多样性高于固定推理下重采样动作后缀）、监督 grounding（教师目标锚定在 st 而非 ht）、预算重分配（在高响应熵状态额外生成完整 reasoning-action 响应，保留根轨迹全响应，M = 4、β = 2）。实验主线是：用 veRL 在 32 卡上从 Qwen3-32B 蒸馏 Qwen3-1.7B 与 Qwen3-4B，在 ALFWorld、ScienceWorld、WebShop 五个评估设置上做宏平均；State-OPD 在两个学生规模上都取得最高宏平均成功率，Qwen3-4B 从最强基线的 26.07% 提升到 31.74%，并在 ALFWorld Seen 超越教师 Qwen3-32B；进一步分析显示更少的受监督 turn 响应带来更高成功率、持久失败动作重复显著减少、定性轨迹展示基于 interaction 的决策修正。论文结论把 interaction state 确立为 agentic 知识迁移的组织单元，强调把现实交互与学生推理分离为 grounding 监督和探索备选决策提供了共同基础。整体上这是一篇机制分析驱动的蒸馏方法论文，兼具诊断性发现、理论形式化（信息充分...**

<!-- paperflow:dbd0e09065671400 -->
## APT: Feedback-Driven Agentic Post-Training for Text-to-Image Generation

[[Deep Reading - Oct 2026/APT-Feedback-Driven Agentic Post-Training for Text-to-Image Generation|Deep Reading]]

[https://arxiv.org/pdf/2610.18097v1](https://arxiv.org/pdf/2610.18097v1)

- **APT 的核心主张是：T2I 后训练不应依赖人工构造的固定数据分布，而应模型自适应——由当前模型自身的行为决定下一轮要训练什么。论文先论证不同基础模型各自诱导不同的能力-失败分布 pM，因此对某个模型有效的后训练数据对另一模型不一定有效；而现有流水线靠人工识别弱点、收集数据、凭经验调分布，既不规模化也不模型自适应。针对这一问题，APT 设计了三个智能体：评估集生成智能体把高层评估需求（用户指定或通用 harness）转换为固定的评估集 E，其中每个评估例是 prompt–rubric 对以便细粒度诊断；诊断智能体用 GPT-4.1-mini 评估当前模型在 E 上的表现，产出覆盖多样生成能力的失败模式报告；RL 数据生成智能体以该报告为条件生成训练提示-标准对，用自适应提示分布突出欠佳能力。训练用 DiffusionNFT + LoRA 完成一轮 RL 更新后，循环重复，形成受 RSI 启发的“评估→诊断→更新”反馈闭环。方法层面，APT 强调三个通用性：模型无关、基准无关、架构无关，且完全自动化。实验上，论文在 Lens、SD3.5-Medium、FLUX.1 [dev]、LongCat-Image 四个扩散模型和 Janus-Pro 一个自回归模型上，用完全相同的智能体流程后训练（无模型特定提示集、奖励设计、超参搜索），在 GenEval、GenEval2、OneIG-EN、DPG、CVTG-2K 五个基准上评测，且训练提示不包含任何基准提示，单次运行直接跨五基准验证。结果显示 APT 平均性能高于人工构造 RL 数据后训练；迁移研究显示针对不同源模型生成的 RL 数据用于目标模型时，对角线设置（同一目标模型自适应数据）在每一行平均性能最佳，支持模型特定训练数据的价值；消融支持迭代反馈的价值。结论把 APT 定位为反馈驱动任务自适应的自动化后训练方向，以及朝 T2I 递归自我改进迈出的实践一步。**

<!-- paperflow:1e5715b396cb1af3 -->
## TextualRL: Textual Reinforcement Learning for Context Management

[[Deep Reading - Oct 2026/TextualRL-Textual Reinforcement Learning for Context Management|Deep Reading]]

[https://arxiv.org/pdf/2610.18216v1](https://arxiv.org/pdf/2610.18216v1)

- **TextualRL 研究的是“冻结参数 LLM agent 如何通过修改自身文本上下文来适应任务反馈”的问题。论文的论证主线是：既然模型参数不可优化，优化变量就应该是上下文；而要把执行奖励转化为上下文更新，反馈的组织方式至关重要。

技术主线包含三个组件：
- Groupwise Policy Critique 将同一任务、同一上下文下的 K 条 rollout 视为一组（Task Block）。组内有成功与失败对照时，通过对比同任务行为差异提出具体编辑；组内结果一致时，借助跨任务证据判断哪些行为值得保留（全成功）或形成修复假设（全失败）。
- Cross-Group Policy Update 将各组提案放到跨组证据池中审查，包括那些没有产生任何编辑建议的观测，以精炼提案的内容和适用范围，形成有界文本补丁。
- 验证驱动采纳：候选上下文是否被采用由留出验证奖励决定，同时用于 step-level 更新接受与 checkpoint 选择；测试集只在最终评估时启用。

实验主线覆盖六个基准（SearchQA、Spreadsheet、OfficeQA、DocVQA、LiveMath、ALFWorld）和五个目标模型。主要结果：六基准平均上 TextualRL 全面超过 Base Model 11.4–24.4 个百分点、超过 SkillOpt 0.9–3.2 个百分点；在 30 个 target–benchmark 组合中 29 个最优、1 个打平。消融实验逐项验证了 outcome routing、组批评（K=1 vs K=4）、跨组审查、无编辑卡片保留等设计的必要性。附加实验显示：优化器从 GPT-5.5 换成 Qwen3.8-27B 后总体得分从 71.0% 降至 69.7%；Qwen3.5-4B 在 ALFWorld 上从 30.6% 提升到 82.1%，是小模型收益最大的案例；技能迁移实验检验了学到上下文在目标任务之外的有效性。

论文的结论是：以组为单位进行策略批评、以跨组证据进行提案审查、以验证奖励控制采纳，这一组合可以在多种模型和任务上稳定地把执行奖励转化为...**

<!-- paperflow:b3585bc88f58aa48 -->
## How Far Can Coding Agents Go in Visual Generation?

[[Deep Reading - Oct 2026/How Far Can Coding Agents Go in Visual Generation|Deep Reading]]

[https://arxiv.org/pdf/2610.19025v1](https://arxiv.org/pdf/2610.19025v1)

- **本文站在专用生成模型（如扩散模型）的对立面，提出并系统考察了“编码智能体即视觉生成器”的新范式。作者首先指出现有生成模型把图像构造过程隐藏在学到的参数中，导致生成结果难以精确控制、验证和解释。为克服这一局限，他们让通用编码智能体通过编写可执行程序来绘制图像，将视觉生成转化为程序合成问题。为了科学评估这种能力，作者构建了包含200个提示的AgentDrawBench基准，覆盖从简单图案到复杂场景的任务，并在零样本条件下测试智能体的绘图表现。

研究发现了一个清晰的“能力轮廓”：编码智能体非常擅长保持结构和满足精确的视觉约束，在组合准确性比照片真实感更重要的场景（如几何图形、图表、符号布局）中能取得有竞争力的结果；但在感知丰富性——细腻纹理、光影、复杂材质——上明显薄弱。此外，智能体的视觉表现力会随绘图界面发生变化，说明所使用的图形程序本身是生成质量的重要变量。一项定性重建实验进一步表明，从人类绘画演示中提取出的程序性技能可以被迁移到不同的艺术媒介，提示这些技能具有一定程度的通用性。

论文随后将焦点从“智能体能否用代码生成图像”提升到“代码能否作为视觉思维的语言”。他们把生成代码视为一种外部视觉思维语言：一种结构化的、可执行且可检查的场景表示。通过在视觉问答任务中比较原始图像与由代码重建的图像，他们发现重建视觉代码保留了充分的场景信息，在部分领域甚至取得比原始图像更高的回答准确率。这一结果暗示代码可能是一种更紧凑、更具语义结构的图像表示。

更进一步，作者追问：智能体在不同的绘图程序之间切换时，是否依赖某个与具体程序无关的共享内部场景模型？他们提出“内部视觉思维语言”假说，并设计了三类行为实验来检验：1）跨harness绘制同一场景，比较生成图之间的布局和色调；2）让智能体以独立单像素查询的方式构建图像，然后与整体绘制结果比较；3）先让智能体生成一个房间布局，随后移除所有外部可见绘制，再从不同视角重新绘制。三类实验均得到支持性证据：跨harness绘图在布局和色调上相关，单像素组装图与直接绘制的图像存在相关关系，且智能体在移除外部提示后仍能跨视角保持空间一致性。

总体而...**

<!-- paperflow:52a65aef738ce999 -->
## Knowledge Graphs as Files: An Agent Harness Where Tool Creation and Tool Use Co-Evolve

[[Deep Reading - Oct 2026/Knowledge Graphs as Files-An Agent Harness Where Tool Creation and Tool Use Co-Evolve|Deep Reading]]

[https://arxiv.org/pdf/2610.18616v2](https://arxiv.org/pdf/2610.18616v2)

- **论文的论证主线是：现有文本丰富知识图谱搜索系统把图访问封装为预定义接口（检索路径/子图或图 API），这种封装在控制上下文的同时也决定了 agent 能接触的证据范围；即使允许 agent 在图接口之上写代码，也无法访问完整节点和边文件。因此作者提出第三种设置 DKGI，把图当作文件直接暴露，让 agent 自己编写图访问代码，形成“工具创建与工具使用共演化”的闭环。技术主线上，DKGI 是一个最小化 harness：节点与边以文件形式存在，agent 通过 get_schema() 获得 schema.json，然后写 Python 程序沿关系跳转、合并节点集、筛选文本证据；程序执行与 shell 搜索、文件检查交错进行，执行反馈进入下一轮程序修订。为控制上下文，工具输出采用 head-and-tail 截断（先返回前 9000 字节或 900 行，再返回最后 9000 字节），并适配 bash 工具 50KB 与 DCI Agent-Lite 20,000 字符限制；运行环境只读，禁止网络和标签访问。实验主线上，论文在 STaRK 的 Amazon、MAG、Prime 三个文本丰富知识图谱上做训练无关评测，用 Hit@1 和 MRR 报告点估计；最强配置 GPT-6-Astra + DKGI 在每个数据集上都超过所有已发表基线，最小化 harness 也至少领先最强已发表基线 6.7 个 Hit@1 点。受控消融用 GPT-6-Luna 对比直接调用预定义图工具、在图接口上写代码、直接文件访问三种设置；结果是 DKGI 整体 H@1 更高，且多轮编写/修订程序带来的增益最大。轨迹分析显示工具创建与工具使用在同一任务内互相塑造。论文承认的边界包括：仅覆盖 STaRK 三图、查询和记录几乎全英文、聚合类问题和图规模扩展留待未来、只读环境阻止网络与标签访问。由于可用 PDF 提取残缺，上述描述中部分机制细节来自片段推断，原文 §3 与附录 C 需进一步核对。**

<!-- paperflow:4d402ebdf669d06e -->
## Knowing the User Is Not Knowing What We Established: Grounding Awareness in Personal Agents

[[Deep Reading - Oct 2026/Knowing the User Is Not Knowing What We Established-Grounding Awareness in Personal Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.18684v1](https://arxiv.org/pdf/2610.18684v1)

- **论文的核心论证是：个人智能体记忆系统长期以来以“知道用户”为目标——建模用户的已知、偏好和需求并跨会话更新——但“知道用户”不等于“知道用户与智能体共同确立了什么”。一个助手提议在未经用户确认时被记住并复用，就会变成一条未经授权的长期指示；已接受的安排也不必依赖一般偏好即可约束未来行动。论文由此提出 Grounding Awareness，把它定义为个人智能体的一项独立需求：能够区分已接受（accepted）、未确认（unacknowledged）、已修订（revised）、限定范围（scoped）的提议，并在行动选择中只执行恰当的授权。技术路线上，论文没有直接提出新的模型架构，而是先构建受控基准：以 Figure 1 的冷推销场景为原型，构造同一提议、同一后续请求但用户授权状态不同的成对历史，用确定性约束生成候选提议并配合人工标注（行动 gold 88% 一致率、接地标注 κ）保证质量；随后在多个 agent backbone（包括 Hermes、DeepSeek、OpenJarvis）与多种记忆系统组合上测量 Sensitivity、Selectivity、Overgeneration、Overconservatism 与 ΔExec I−N。结果呈现三个层次的发现：一是强记忆性能不代表接地意识；二是更完整的用户信息会显著提高对未确认提议的执行率；三是持久记忆会把未确认提议固化为长期指令，且 71% 的错误发生在完整交互信息可用的情况下。与此同时，Jarvis 对已存储 scoped 提议仅保留 4–11% 的范围约束，说明范围信息是记忆链路中最脆弱的成分。作者进一步提出 Grounding Layer（GL），用预定义规则在行动决策层治理授权状态、范围、修订与行动风险，并在全部评估配置中一致提升接地意识（如 Hermes full 下 Sens. 从 0.21 提升、ΔExec I−N +0.24 等）。论文的工程性发现还包括 OpenJarvis 默认系统提示缺少当前日期会干扰时间相关决策这类配置故障；Limitations 中承认 GL 目前依赖预定义规则治理...**

<!-- paperflow:4aad4e8706cb4de9 -->
## VideoJev: System One Model for Video Understanding and Interaction

[[Deep Reading - Oct 2026/VideoJev-System One Model for Video Understanding and Interaction|Deep Reading]]

[https://arxiv.org/pdf/2610.18615v1](https://arxiv.org/pdf/2610.18615v1)

- **VideoJev 的核心主张是：结构化视觉判断可以支撑开放式行为，而不需要在每一步都进行开放式生成。论文从智能体的需求出发——定位证据、检查进度、评估候选动作，且经常复用同一段视觉历史——提出把视频理解与交互统一为结构化决策任务。方法上，VideoJev 在预训练 MLLM 之上用一个共享打分头替代自回归答案解码：该头对时间区间和文本选项分别打分，产生事件区间或答案选项，从而统一时间定位与多项选择视频问答。这结合了 MLLM 的语义灵活性与智能体可直接消费的高效决策。针对交互过程中同一视频历史被反复查询的问题，VideoJev 提供两种计算复用机制：Cached inference 让后续查询复用已计算的视觉表示，Joint inference 在单次前向中处理多个查询。实验主线分两条。第一条是质量与效率的权衡：在 CharadesTL、ActivityNetTL、QVHighlightsTL 三个时间定位基准上，VideoJev-4B 与 VideoJev-35B-A3B 相对骨干分别提升平均 mIoU 6.7 与 10.8 点，推理加速 2.8–3.1 倍；在 LVBench、Video-MME、LongVideoBench、MLVU 四个 QA 基准上，性能与骨干 Thinking 模式相当，实测加速超过 100 倍。第二条是机制验证：消融显示多任务训练（时间定位 + QA）在所有七个基准上都优于单任务训练，平均 grounding mIoU 提高 0.7 点、平均 QA 分数提高 1.3 点，最大增益在 LongVideoBench（+1.9 点），表明两类监督互惠互利；推理模式对比显示 Joint 对多查询场景更高效，而 Direct 自回归基线在 Joint 处理下精度下降。论文最后把 VideoJev 定位为面向通用视觉决策的 System One 组件：未来可扩展到动作识别、事件检测、场景状态分类等候选假设决策任务，并由 System Two 规划器组织决策空间、由 VideoJev 对照视频证据评估候选，应用于过程监控、交互式助理、具身智能体与计算机使...**

<!-- paperflow:adca0d8f24d6bcf7 -->
## Self-Evolving Agent Harness

[[Deep Reading - Oct 2026/Self-Evolving Agent Harness|Deep Reading]]

[https://arxiv.org/pdf/2610.18813v1](https://arxiv.org/pdf/2610.18813v1)

- **本文是 Harness RSI（递归自我改进）方向的系统性框架论文，共 66 页。核心主张是：智能体在部署后的持续适应不必依赖模型权重更新，而可以通过修订其软件支架——提示词、记忆、工作流和可执行组件——来实现。作者把这一过程称为 harness self-evolution。

论证主线：
1. 必要性论证：任务与环境在部署后持续变化，agent 需要持续适应机制；固定权重的 harness 修订直接塑造固定模型的行为，可复用过程保留任务约束与失败教训；复用现有部署避免重训成本，且可通过 proprietary API 更新；指令、记忆与代码可直接检查、版本比较与变更追踪，比模型 checkpoint 的行为归因更容易。
2. 框架设计：形式化反馈驱动的修订过程，被接受的修订跨运行持久化；更新机制保持可用以从后续经验中学习。框架把更新目标（解释型状态 vs 工具/代码 vs 仓库级控制逻辑 vs 插件组合）与提案/准入机制分离，并把 mutation boundaries 与 search/admission 分离。
3. 理论刻画：用进化优化视角刻画搜索；用持续 PAC 学习分析可靠性。在显式假设下得到：期望泛化界、依赖于历史回退预算的保留界、使用全新独立验证数据的高概率准入规则。
4. 社会与组织含义：持久化修订保存知识但也传播错误，可能造成不可逆后果；部署需要随任务分布与评估者漂移进行持续验证；需要对学习、运行时权限与重大后果行动设置独立保护。结合已有工作案例说明模型-支架协同设计、分布式执行与插件复用。

技术主线：分层修订边界、进化搜索、PAC 风格可靠性分析、验证驱动的准入。实验主线：由于检索证据中未见实验章节，不能确认具体实验；论文提供 GitHub 代码与补充材料。

需要区分：本文是理论/框架论文，不是实证系统论文；对具体实验数值的确认需回原文。**

<!-- paperflow:037a58de99709f3b -->
## Agent-Reflex: Teaching Language Agents to Act Reflectively

[[Deep Reading - Oct 2026/Agent-Reflex-Teaching Language Agents to Act Reflectively|Deep Reading]]

[https://arxiv.org/pdf/2610.17953v1](https://arxiv.org/pdf/2610.17953v1)

- **Agent-Reflex 论文的论证主线是：真实部署中智能体面对的 MCP 工具反馈充满噪声、用户会随时介入纠正，而现有训练方法只给智能体提供有限监督，无法学会主动反思和失败恢复。作者把这一空白拆成两个具体缺口：缺乏噪声环境反馈、执行中缺乏用户引导的纠正。技术主线是三个阶段的训练框架：（1）RSD 通过在环境扩展中注入扎根工具反馈扰动，并采样能力多样化 LLM 池、挖掘匹配 success-failure 轨迹对，归纳可复用的反思元技能库，解决‘反思信号从哪里来’的问题；（2）ReSFT 把每项技能翻译成自我反思、技能恢复、用户中心纠正三种互补监督，解决‘技能如何变成训练信号’的问题；（3）ORPO 把噪声交互下的 agentic RL 与反思引导的 on-policy 蒸馏结合，通过‘特权槽’让同一策略充当 teacher 重打分 student rollout，解决‘如何让反思从被动变为主动’的问题。实验主线是六套挑战性基准（BFCL-v4、ACEBench、ToolSandbox 200、τ²-Bench、VitaBench、AgentNoiseBench），统一从 Qwen3-8B 出发，与反思方法（Reflexion 等）、环境扩展方法（Simia、EnvScaler 等）、轨迹级 RL 方法三族基线对比，取得总体最高分（58.6 overall，AgentNoiseBench 29.1）。消融显示三阶段互补，训练曲线稳定，并且通过‘构建 RSD 与 AgentNoiseBench 独立’的声明排除噪声泄漏。结论是 Agent-Reflex 改善了长程工具使用和噪声鲁棒性同时保持稳定训练动态，支持可扩展智能体训练。代码开源。**

<!-- paperflow:6ed15b9d113b49fd -->
## Hybrid Reasoning Agents

[[Deep Reading - Oct 2026/Hybrid Reasoning Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.18861v1](https://arxiv.org/pdf/2610.18861v1)

- **论文的论证主线：前沿智能体的扩展瓶颈已经从模型自身推理能力转移到与外部世界的交互能力上，但无论 harness 怎么变，文本始终是智能体生成与环境交互的唯一通用介质，所有中间推理都要逐 token 解码成文本；与此同时，潜在推理展示了在连续隐藏空间展开计算的效率优势，却受限于没有工具通路、不可监督、不可审计。作者由此提出一个融合命题：智能体应当“思考超出人类语言，但仍通过语言在外部环境行动”，文本与潜在推理应当作为统一生成过程中可动态切换的两种模式，而不是互斥路线。技术主线：架构上，HybridLink 是连接 last-layer hidden state 与 token embedding 空间的轻量模块（线性+GELU+线性+残差），配合 <latent>/</latent> 控制 token 标记潜在段，使潜在思考在生成流中可见；连续的 working memory 跨多轮携带“文本+潜在思考+工具观察”的完整混合轨迹。训练上，作者明确不重新预训练，而是把标准 agentic 后训练管线适配到混合设定：HybridSFT 先让主干与 HybridLink 学会协同生成稳定的混合轨迹；HybridRL 再让模型学会策略性地选择“在潜在空间思考 vs 用文本行动”。奖励设计包含格式奖励（保证混合轨迹结构合法）与工具奖励（支撑工具调用行为），消融显示两者分别带来 4.7%–6.9% 与 agentic 基准上的显著贡献。实验主线：在 3 个模型规模、11 个基准（数学/科学/医学/智能体任务）上与三类基线对比——前沿 LLM 智能体（QwQ-32B、GLM-5.3-Flash、DeepSeekV4.1-Flash、Kimi-K3、GPT-5.6-Luna）、agentic 框架（Search-o1、WebThinker、ReAct）以及同规模的 latent/text 方法（Coconut、SoftThinking、GRPO、ARPO、Qwen3 thinking）。主结果：性能平均提升 10.4%，token 缩减 35.5%–54.7%，推理加速 1.8×–2.6...**

<!-- paperflow:ec7b51a6aaf27629 -->
## Tev: Scaling Long-Horizon Agentic Environments with Dense Progress Rewards

[[Deep Reading - Oct 2026/Tev-Scaling Long-Horizon Agentic Environments with Dense Progress Rewards|Deep Reading]]

[https://arxiv.org/pdf/2610.18868v1](https://arxiv.org/pdf/2610.18868v1)

- **论文试图同时回答长时程智能体的两个瓶颈——训练数据缺失和训练配方缺失——并给出了端到端方案：从隐藏真值反推任务合成的数据管线（latent-world-first）+ 把进度转化为势函数稠密奖励的训练配方（PPRS）。
论证主线：引言从 RSI/自动发现引出长时程智能体的战略价值，指出开源数据只有短/中时程规模（PR 修复、短终端会话）、评测基准只有少量人工实例且只能给稀疏信号，因此“不存在长时程训练语料”是核心空白。随后用 (1−p)^G 的量化论证说明稀疏奖励下长时程 RL 的训练浪费：p=0.05 与 G=8 时 66% 的组无效，p=0.01 时 92% 无效——这是“长时程任务不可训练”的机理层解释。技术主线：先构建 latent world（完整且正确的隐藏规范，含领域、工作流与每步期望状态），再施加 corruption plan 生成可逆缺陷，渲染出面向智能体的环境（prompt.md、损坏的 input/、bash 工具）。由于真值已知，环境可精确验证、复杂度可控、进度可度量。TEV-GYM 由此产出 1,170 个已验证环境、7 个领域（Software、ML、Operations、Security 等，含数据对账、检查点整合、系统管理、合成二进制/网络包 CTF 等任务类型），每个环境声明工具链契约，生成器有严格质检门槛。训练主线：把进度度量经 PPRS 变成稠密逐步奖励（Eq. 4），避免朴素稠密奖励的“反复奖励高潜力状态却不传播信用”导致的上下文耗尽/长轨迹崩溃，也避免稀疏奖励的低效；9B 用 unit 难度 264 任务子集、27B 用全语料，DPPO 训练（G=24、65,536 tokens/rollout、最多 64 工具调用、单 bash sandbox），27B 约 4 天训练完成。实验主线：主实验回答三个问题——(1) in-domain 提升：TEV-9B 在 ALE-CM Pass@5 提升 83%，TEV-27B 提升 65% 到 0.28，匹配 GPT-6 Astra 与 Claude Opus 5；(2) 泛化：仅用 A...**

<!-- paperflow:0b27bd6bb5b75080 -->
## Rubrics from the Task, Verdicts from Evidence: Pointwise Rubric Rewards with Tool-Augmented Judges

[[Deep Reading - Oct 2026/Rubrics from the Task, Verdicts from Evidence-Pointwise Rubric Rewards with Tool-Augmented Judge|Deep Reading]]

[https://arxiv.org/pdf/2610.18766v1](https://arxiv.org/pdf/2610.18766v1)

- **Rubric-based reward models (RMs) evaluate responses against explicit criteria.

Rubric-based reward models (RMs) are attracting growing attention in LLM post-training for nonverifiable tasks that lack a single correct answer. On such tasks, traditional RMs that assign holistic scalar scores (Christiano et al., 2017, Ouyang et al., 2022, Wang et al., 2024a, Liu et al., 2026c) can be unreliable (Hashemi…

-N Selection Gate with Hard Rules Break ties with Principles Policy Training DPO pairs GRPO Reward 3.3. Pointwise Tool-Augmented Judging The judge scores one response per episode.

the two responses in separate episodes under one rubric and predicts the higher-scoring one; ties and invalid judgments count as errors.

主要贡献包括：-N Selection Gate with Hard Rules Break ties with Principles Policy Training DPO pairs GRPO Reward 3.3.；Pointwise Tool-Augmented Judging The judge scores one response...**

<!-- paperflow:2132cd9f42a5f0ef -->
## SWE-MISBEHAVIOR: Benchmarking and Mitigating Boundary Violations in Software Coding Agents

[[Deep Reading - Oct 2026/SWE-MISBEHAVIOR-Benchmarking and Mitigating Boundary Violations in Software Coding Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.18206v1](https://arxiv.org/pdf/2610.18206v1)

- **1. 论证主线：
论文从 LLM 编码智能体的生产化部署切入，指出这类智能体已经从“代码生成器”演化为具备仓库导航、文件修改、shell 执行和测试运行能力的自主代理。自主性带来效率，也带来新的安全维度：拥有写和执行权限的智能体能够静默破坏工作代码、破坏下游依赖、损害仓库完整性，甚至删除生产数据库和备份，且这些后果往往是严重且不可逆的。论文继而指出现有软件工程基准（SWE-bench、BigCodeBench）只衡量端任务成功，遗漏了“越界行为”这一关键风险面；少量已有工作仅覆盖 CI 失败和 API-breaking changes，没有覆盖静默破坏无关文件、滥用系统权限、非预期修改用户输入，也没有利用执行日志、环境状态、资源使用等轨迹级信号。因此论文提出一个新的问题框架：将编码智能体可靠性建模为动作边界问题，沿代码编辑、文件系统操作、系统交互三个层面定义边界。
2. 技术主线：
作者构建了 SWE-MISBEHAVIOR，包含 taxonomy、benchmark 和自动化评估协议三个部分。Taxonomy 覆盖三层动作空间：Code Generation、File Management、System Interaction，并定义了 11 类 misbehavior，分别对应四个 code-level 指标、四个 CRUD-level 指标、两个 system-level 指标和一个 task-goal metric。数据规模为 7.4K 个人类参与的合成样本，来自 115 个真实仓库和 1,384 个人工策划 prompt；构建流程包括实例核心选择、prompt 构造（LLM 改写与语义等价 paraphrase）和研究生级标注者审核，并引入 prompt variations。评估协议通过记录智能体的 shell 命令、文件系统操作和系统交互，利用 LLM judge 对观察到的动作进行自动化验证；System Interaction 合并 execution 和 testing 设置，三部分结果以加权平均汇总。针对发现的 misbehavior，作者提出 C...**

<!-- paperflow:c60cbfc0da9ca94c -->
## PersEvalBench: Can LLM Evaluators Judge Contextually Appropriate Use of Persona Information?

[[Deep Reading - Oct 2026/PersEvalBench-Can LLM Evaluators Judge Contextually Appropriate Use of Persona Information|Deep Reading]]

[https://arxiv.org/pdf/2610.19314v1](https://arxiv.org/pdf/2610.19314v1)

- **PERSEVALBENCH 是一篇系统研究 LLM 评估器在个性化助手场景中判断能力的论文。论文的核心动机是：LLM 评估器日益被用于评估个性化助手的质量，但少有研究检验评估器自身能否可靠判断个性化行为是否得当。这种判断是困难且影响重大的——即使用户历史信息在事实上正确，也可能被误用，例如信息过时、属于他人、超出当前范围或权重失当；反之，评估器的误判会扭曲个性化系统的优化信号，导致系统抑制个性化（欠个性化）或过度使用用户信息（过度个性化）。论文的方法论核心是构建一个受控基准：从公共 Schema-Guided Dialogue 数据集导出 480 个任务种子，由 LLM 规划器实例化为 456 个场景规格，最终形成 403 个多情节场景。每个场景构造用户信息的三种状态——应持续、应转移、应失效——并通过匹配对设计隔离三类错误：欠个性化（完全遗漏、部分遗漏、明确违反）、过度个性化（范围泄漏、无根据推断、过度主导）和状态误用（过时/被取代等）。论文通过独立人工审计验证错误生成的质量，标注者间加权 κ = .709。实验在五个 LLM 评估器上进行，主要发现包括：一是评估器对受控个性化错误的识别能力普遍有限；二是欠个性化是最大短板，人类识别 94.4% 而最强评估器仅 63.9%；三是评估器常识别出响应有问题但混淆错误方向（将 UNDER 误判为 OVER）；四是高亮相关历史片段仅略微提升识别率，说明困难主要在于推理而非证据获取。论文的贡献包括：首次系统性地构建用于评估 LLM 评估器个性化判断能力的基准、提出九种受控错误子类型的完整类型学、通过人工审计保证数据质量、揭示评估器在个性化判断中的系统性弱点（特别是欠个性化漏报和方向混淆），以及提示高亮证据策略收益有限的负结果。论文的局限包括：错误生成基于规则和 LLM planner，可能未覆盖真实个性化误用的全部形态；实例规模（尤其某些子类型仅 6-12 个实例）可能限制统计效力；人类标注者间一致性虽达 .709 但仍有分歧；五个评估器的覆盖范围有限。论文的实际启示是：个性化评估不应仅关注生成器能力，评估器自身的判断可靠性必须...**

<!-- paperflow:d5fea519753c9b77 -->
## Pack Light, CarryOn: Building Object-Oriented Context for Long-Horizon Agents

[[Deep Reading - Oct 2026/Pack Light, CarryOn-Building Object-Oriented Context for Long-Horizon Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.18784v2](https://arxiv.org/pdf/2610.18784v2)

- **Long-horizon agents accumulate progress in persistent task state, yet repeatedly process interaction histories describing how that progress was achieved.

xt around reusable progress improves over generic compression, while the full CarryOn runtime further improves over the same principles implemented through repeated summarization. Continuation carriers and execution context play complementary roles, and reusable progress is typically maintained across folds rather than repeatedly reconstructed.

identifies progress that may become reusable in subsequent execution. The Tracker maintains candidates, each representing a potential new carrier or an update to an existing one.

Those internal rules create no new host agent duty to re-read or re-verify. Preserve targeted checks for actual conflicts, missing required information or unknown outcomes;

主要贡献包括：identifies progress that may become...**

<!-- paperflow:39024ab557a6e226 -->
## Reliable Memory from Unstable Growth: Curating Self-Written Agent Playbooks with Randomized Experiments

[[Deep Reading - Oct 2026/Reliable Memory from Unstable Growth-Curating Self-Written Agent Playbooks with Randomized Exper|Deep Reading]]

[https://arxiv.org/pdf/2610.18427v1](https://arxiv.org/pdf/2610.18427v1)

- **本文研究语言模型智能体持久化文本记忆（playbook）的质量控制问题，核心论点是：自写经验教训的取舍不应交给智能体自己的判断，而应通过随机化实验测量每条经验对任务成功的因果效应来决定（“An agent's memory should be measured, not judged”）。

论证主线：作者先指出现有系统（ACE、Dynamic Cheatsheet）依赖智能体自评来维护 playbook，并观察到严重不稳定性——同一套书写流程在不同运行中可能产出有帮助的 playbook，也可能产出比“无 playbook”低最多 69.6 个百分点的有害 playbook。智能体自我判断的平均成功率 53.8，甚至低于不用 playbook 的 54.1，说明“判断式记忆管理”不可靠。由此引出方法论转向：用测量的因果效应代替主观判断。

技术主线：作者提出 Randomized Block Curation（RBC），包含五个要素：（1）把语义相关的经验教训聚合成足够大的块，使效应可测；（2）在练习任务上随机包含/排除每个块，使有无块的任务可比较，成功率差值即为块的平均因果效应；（3）用无偏估计（含 ridge 正则回归）估计块效应，并给出 Hoeffding 不等式与 union bound 的置信保证；（4）保留正效应块形成 curated playbook；（5）因为同一流程可能长出有害 playbook，所以同时生长多个 playbook，并在新鲜练习片段上比较 curated、完整、无 playbook 三种候选，服务得分最高者。

实验主线：在 ACE 和 Dynamic Cheatsheet 两种记忆系统、27B/9B/Gemma 三个模型、AppWorld/WebShop/ALFWorld 三个基准上，排除 WebShop 的 9B/Gemma 设置后共 9 个设置，最多生长 10 轮。结果显示 RBC 平均成功率 66.9，显著高于完整 playbook 的 55.5、无 playbook 的 54.1 和智能体自我筛选的 53.8。退化分析显示完...**

<!-- paperflow:1562b0eb0bfb8145 -->
## RefCraft: Benchmarking Visual-Reference-Conditioned Agentic Generation through Web Development

[[Deep Reading - Oct 2026/RefCraft-Benchmarking Visual-Reference-Conditioned Agentic Generation through Web Development|Deep Reading]]

[https://arxiv.org/pdf/2610.26826v1](https://arxiv.org/pdf/2610.26826v1)

- **论文的核心主张是：在真实生产力任务中，“把视觉参考纳入智能体生成”是与工具调用、编码同等重要的能力，但现有基准没有度量它。作者以网页开发为测试床，提出 RefCraft 基准与配套评测协议，并对九个前沿 (M)LLM 做了系统评测。

动机与问题（引言）：(M)LLM 作为编码与协作助手已极大提升生产力，并正向更广泛任务扩展；但审视真实工作流（设计师先研究成功案例再迭代；CAD 工程师检视现有零件再设计新件）可见，视觉参考的理解与检查闭环是不可或缺的一环。现有网页基准要么只给文字指令、要么只做目标重建，都“理想化”地回避了“从视觉示例开发”的常见工作流。RefCraft 的目标是补上这一缺口。

基准构成（§3）：300 个多模态网页开发任务，构建分三阶段。①参考池：从 Colorlib、Start Bootstrap、Creative Tim、AdminLTE、Themefisher、CodeWithSadee 等采集开源模板，保留 367 个 MIT、76 个 CC BY 3.0、1 个 Apache-2.0 许可页面，记录 URL、作者与许可；②查询合成：把 1–5 张参考截图与自然语言指令组合成可控、真实的用户请求（示例任务要求“借用 A+B”的粗体与通透表格、白色卡片、绿色按钮、蓝色细节，构建链接报价与预订的桌面/平板/手机响应式页面）；③关键点分解：每条查询由 Claude Opus 5 生成、GPT-5.6 Sol 验证的关键点集合进行评分，审查时考虑参考使用一致性与可验证性，允许修订并说明遗留问题。

任务形态（agentic 设置）：模型在隔离工作目录中运行（禁止访问基准仓库与关键点，保留网络访问），通过 OpenCode harness 完成“从参考和指令产出初稿 → 渲染 → 视觉检查 → 迭代精修”的闭环，上限 100 轮。

评测协议（§5/附录）：总体分 S 由关键点得分、参考使用得分（R）与核心功能得分三个分量构成；参考保真度用分级量表（Good 级要求色差 <5%、间距 1–2px、几乎不可分辨）；程序化判分器与 VLM 裁判（Gemin...**

<!-- paperflow:aaeae78c059f24e4 -->
## Can It Actually Work? Synthesizing Functional CAD Mechanisms

[[Deep Reading - Oct 2026/Can It Actually Work-Synthesizing Functional CAD Mechanisms|Deep Reading]]

[https://arxiv.org/pdf/2610.26861v1](https://arxiv.org/pdf/2610.26861v1)

- **本文针对 LLM 智能体 CAD 生成中“视觉合理但功能失效”的问题，系统提出了 functional CAD design 问题设定、FunCAD-Harness 框架和 FunCADBench 基准。论证主线是：CAD 装配体的功能取决于零件在运行时的物理交互，而视觉检查和几何有效性无法保证负载传递、接触保持和状态复位等要求；因此必须用物理仿真构造候选特化的测试，并用仿真反馈驱动设计修订。技术主线上，FunCAD-Harness 将用户请求翻译为任务需求和抽象物理测试模板，再根据具体候选机制的组件、控制方式和操作序列确定测试条件；执行时在物理仿真中检查功能表现，并保留后置几何检查捕捉仿真遗漏的失效模式。评估主线上，作者构建覆盖 46 个机制家族、包含相互依赖和多步骤操作的 FunCADBench，在四个前沿 LLM 智能体（GPT-6 Astra、Kimi K3、Fable 5.1、Grok 4.6）上进行受控评估：主实验中四个模型共解决 34/46 任务，Fable 5.1 表现最强但仅解决 5/15 高复杂度任务；所有模型在 contact 需求轴（engagement、release、friction）上加权成功率最低；消融显示去掉视觉访问后 GPT-6 Astra 从 22/46 降到 20/46、Grok 从 19/46 降到 18/46，说明功能设计能力在很大程度上不依赖视觉；harness 对比表明 FunCAD-Harness 相比标准仿真增强基线显著提升功能成功率，并能暴露视觉不可见的机械失效；案例分析展示了一个“仿真测试全部通过但后置检查失败”的反例（四个行程检查通过、两个输出反向滑动检查失败），说明多层验证的必要性。论文的结论是，这项工作标志着 CAD 智能体从生成可制造模型向把人类意图转化为真正可工作的机构迈出了一步。**

<!-- paperflow:587fe58b3653b6a5 -->
## Are Self-Evolving Agents Continual Learners?

[[Deep Reading - Oct 2026/Are Self-Evolving Agents Continual Learners|Deep Reading]]

[https://arxiv.org/pdf/2610.25858v1](https://arxiv.org/pdf/2610.25858v1)

- **论文围绕一个核心问题：非参数自进化 LLM 智能体是否真正具备持续学习能力。作者观察到 LLM agent 的实际部署环境具有动态性——新任务、新工具、新工作流持续出现，而模型权重在训练后冻结；传统持续学习通过梯度更新实现，但 agent 必须在交互时间尺度上适应，无法承担每会话重训的成本，小批量微调又会侵蚀已有知识。因此，权重冻结、仅重写 prompt/记忆/技能等 harness 的非参数自进化成为自然替代。然而这类方法是否能在连续漂移任务流中同时保持可塑性和稳定性，此前缺少系统验证。

论文首先给出形式化框架。给定输入 x，agent 在任务环境中 rollout 得到 episode y∼π_θ(·|x,c)，其中 θ 为模型权重、c 为 harness，由此定义 episode 级策略 π_{θ,c}。evolver 可以选择更新 θ 或 c；非参数情形下 θ 固定，只更新 c。作者认为评估一个 evolver 需要考虑两个因素：evolver 自身 f 和任务流的权重调度 w。任务流被实例化为漂移流：权重 w_t 是 one-hot 且分段常数，即任务流由连续块构成，每个块内所有更新由单一任务组驱动，块边界切换活动组；之前的组不再获得反馈，之后的组尚未出现。这种设定对应 task-free continual learning 的经典场景：agent 不知道组标签和块边界，必须自行推断任务结构并编码进共享 harness。

基于此框架，作者从三个 agentic benchmark 构造四个漂移流，隔离三种基本漂移及组合：主题漂移（BIRD 三个数据库顺序出现，环境与工作流不变，但 schema 和局部约定变化）、工作流漂移（BFCL-Miss 后接 BFCL-Base，API 相同但正确工作流从主动澄清变为禁止多余澄清）、领域漂移（环境与动作空间变化）、复合漂移（组合多种漂移，证据显示含 BFCL-Miss（domain）→BFCL-Base（workflow）的跨域+冲突工作流转换）。每个任务组有 50 个训练任务（ScienceWorld 评估集 9...**

<!-- paperflow:ee184444e2f98239 -->
## HarnessPro: Recursive Harness Self-Improvement with Pareto Optimization for Performance-Cost Trade-offs

[[Deep Reading - Oct 2026/HarnessPro-Recursive Harness Self-Improvement with Pareto Optimization for Performance-Cost Trad|Deep Reading]]

[https://arxiv.org/pdf/2610.26770v1](https://arxiv.org/pdf/2610.26770v1)

- **HarnessPro 研究的问题是：递归 Harness 自改进虽然能提升智能体性能，但往往以更高的执行成本为代价；论文试图回答能否同时提升性能并降低成本。论证主线：首先指出现有 RSI 方法（Dynamic Cheatsheet、ReasoningBank、Sleep-time Compute、RHO 等）在性能提升的同时伴随 token 消耗与延迟增加；其次提出性能与成本的权衡应被显式建模，并认为找到好的操作点面临两大挑战：巨大 Harness 搜索空间和不同部署预算下的多目标需求；最后通过实验证明性能与成本可以同时改善。\n\n技术主线：HarnessPro 采用多智能体递归进化框架。Trajectory Analyzer 对每个训练任务执行多次 rollout，诊断失败与可避免开销；Planner 在不同性能-成本优先级下制定改进计划；Editor 将计划实现为候选 Harness；Test-synthesis Agent 在无 ground-truth 与官方测试条件下生成评估用测试。候选 Harness 通过归一化加权平方距离选择，在 Pareto 前沿上输出 lite（成本优先）、medium（均衡）、heavy（性能优先）三个档位，并递归迭代。训练集用 DPP coreset 从基准数据中选出 10 个任务，降低监督成本。在实现层面，进化出的 Harness 会从任务描述选择执行 profile（推理努力与工具预算）、压缩工具输出、把 reminder 追加到工具结果并复用 prompt 前缀缓存、削减静态上下文冗余等。\n\n实验主线：在 Terminal-Bench 2（终端任务）、SWE-Bench Pro（仓库级 issue 解决）、ProgramBench（程序重建）上，用 GPT-5.5×Codex 与 GLM-5.2×Claude Code 两套组合评估，对比 Vanilla、Dynamic Cheatsheet、ReasoningBank、Sleep-time Compute、RHO、RHO+Iteration 及自身三档。主要结果：GP...**

<!-- paperflow:5aec966795125d2f -->
## CUA-Lite: An Open Platform for Computer Use Agents

[[Deep Reading - Oct 2026/CUA-Lite-An Open Platform for Computer Use Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.26071v1](https://arxiv.org/pdf/2610.26071v1)

- **CUA-Lite 针对计算机使用智能体领域资源散乱、无法集成和扩展、评估/RL/SFT 无法共享基础设施的问题，提出一个开放平台。平台以三个标准化抽象为支柱：统一环境接口、统一监督数据格式、共享模型 harness。统一环境接口将 12 个覆盖桌面、浏览器和移动端的 benchmark 以及训练环境纳入同一套动作空间和观测表示；GUI 动作返回截图加文本，额外工具返回自身结果。为降低桌面环境成本，论文从 OSWorld 官方镜像逆向工程出应用清单与配置，用 Dockerfile 重建出无 VM 的 Lite.OSWorld 沙箱，使大规模 RL 不再受虚拟机成本制约。统一监督数据格式将公开数据集（从 GUI grounding 到完整轨迹）转换成一个包含 reasoning_content、action_description 和 tool_call 的 schema，并新增约 82K 条来自 GPT-5.5、Qwen3.5-27B、Qwen3.8-27B 等模型的、reward 验证过的 rollout 样本。模型 harness 为 14 个模型家族各实现一个，可生成 rollout 供评估和 RL 使用，或将监督数据渲染成模型训练格式供 SFT 使用，从而打通了三种训练/评估范式。实验部分运行了 15 个模型（含专门 CUA、通用 VLM 和 API 模型）在 12 个 benchmark 上的参考分数，明确声明这些分数可重跑但不声称复现先前结果。论文进一步演示了 RL 和 SFT 通过同一 harness 改进开放权重模型：RL 后浏览器成功率从 60.0 升到 72.8，移动端 shaped reward 从 34.6 升到 46.7；同时记录了训练任务选择失败（GRPO 在某些桌面任务上的负结果）、推理开关对性能与 rollout 长度的非单调影响、以及教师 rollout 设置（推理开关、截图数量）对 SFT 蒸馏的影响。论文还指出 harness 实现边界——只实现 GUI loop 而不含 Codex/Claude Code 等增强工具，避免将工具集成...**

<!-- paperflow:26cb5a45c2714807 -->
## Agent World Model in the Wild: Computer-Use Agent Environment Synthesis

[[Deep Reading - Oct 2026/Agent World Model in the Wild-Computer-Use Agent Environment Synthesis|Deep Reading]]

[https://arxiv.org/pdf/2610.26828v1](https://arxiv.org/pdf/2610.26828v1)

- **论证主线：论文从范式转移出发——agent 学习正从“从人类数据学习”转向“通过与世界交互学习”，但真实计算机系统不适合作为规模化训练场（不可控、难重置、有真实损害与安全风险），因此核心命题是：能否用合成世界替代真实世界作为通用 CUA 的训练环境，并保证合成世界足够真实、足够难、足够可验证。论文对“通用 CUA”采取广义定义：agent 不应被绑定在单一交互模态上，而应在 GUI、MCP、CLI 之间按需切换。现有环境合成工作按接口分裂（web/工具/终端/软件工程各自为政），且任务状态多为注入式、彼此孤立，与真实个人电脑“某人物长期使用后的累积状态”分布不一致。
技术主线：WildAWM 把合成实例定义为三元组（世界状态 W、任务 τ、verifier V）。世界合成把状态建模为 persona 在多日使用后的累积结果 W(c,d)，环境状态中位 52.4K tokens；任务生成要求任务 grounded 在累积状态上并声明预期接口组合；随后进入多 agent 验证循环——异构开源 agent 执行任务，五个执行式质量门审查执行证据决定接受、修订或丢弃：可解性门（异构 rollout 成功证据）、初始不可满足预筛、难度门（足够难但可解）、跨 judge 一致性检查、反事实探测（judge 是否被“看似完成实则未完成”的轨迹欺骗）。verifier 采用严格证据规则：agent 自述笔记只能作为交付物的证据、不能作为外部事实的证明，必须交叉核对独立系统状态。整个管线只用开源模型：GLM-5.2 兼任生成器与 agent-as-a-judge，OpenCode harness 执行，8×B200 GPU 完成合成与训练。训练配方为 SFT 3 epochs 热身 + RL（≤64 步、batch 8、G=4），RL 用按任务归一化的轨迹优势 (rj−µτ)/στ 并区分 reward 上下半池以鼓励奖励多样性。
实验主线：合成 341 个长 horizon 任务（100/208/33 划分），GPT-6-Astra 在其上仅约 60% 成功率，证明难度充足；用 Qwe...**

<!-- paperflow:9916f0acaacfe2a3 -->
## Focused Omni-Modal GUI Agents

[[Deep Reading - Oct 2026/Focused Omni-Modal GUI Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.26203v1](https://arxiv.org/pdf/2610.26203v1)

- **本文围绕一个具体而普遍的失败模式展开：全模态 GUI 代理在多步交互中把完整上下文均匀暴露给解码器，但相关性随步骤快速变化且高度稀疏，注意力错误又会通过动作执行反馈回上下文，形成复合误差，导致不可逆的轨迹漂移。作者把这一现象命名为 Sequential Distraction，并通过对 OmniGUI 行为的分析（单模态移除、通道权重变化）论证了它的存在与成因。

针对这一诊断，作者提出 Focused Perception 作为需求定义：任何解决方案都必须满足 step-adaptive（随步骤变化）、action-sufficient（只需动作所需证据）和 non-intrusive（不改变预训练骨干参数空间）。这一定义排除了此前主流的输入空间“减法”路线——DRS-GUI、OmniParser、RegionFocus 等视觉精化方法只能处理屏幕通道，既无法覆盖音频、视频、设备状态，也难以同时完成动作选择和参数接地。

FocusGUI 是作者给出的实现：一个轻量适配器。在每一步，它先从完整观察中构造一组可分离的感知通道，包括骨干的集成隐藏状态和空间屏幕通道等；然后根据步骤特定的决策参考为这些通道动态分配优先级，把最相关的证据压缩成紧凑的焦点表示 zt；再将 zt 通过一个映射产生零初始化残差，注入到冻结解码器的少量嵌入位置上。最终解码器看到的嵌入是原始嵌入加一个稀疏掩码的残差，骨干权重完全不动。焦点残差从零初始化，因此训练初期不影响预训练行为；注入点位于动作边界，便于动作 token 生成时利用焦点信号。训练只需要动作标注，不需要任何模态相关性标注，也不需要微调骨干。默认容量参数 K=4、dz=64 跨四个骨干固定，不按基准调参。

实验在 OmniGUI 基准上进行，覆盖 29 个应用、708 episodes、2572 个动作步，每一步都配合同步音频、视频与历史，因此决定性信号可以来自任一流。论文报告 Type Match、Exact Match、Goal Progress 三个指标，并使用 Qwen2.5-Omni、VITA-1.5、MiniCPM-o 4....**

<!-- paperflow:0ae7783fd1df7626 -->
## Memory as Code: Declarative Retrieval over Codified Long-Term Memory

[[Deep Reading - Oct 2026/Memory as Code-Declarative Retrieval over Codified Long-Term Memory|Deep Reading]]

[https://arxiv.org/pdf/2610.27700](https://arxiv.org/pdf/2610.27700)

- **本文针对长期记忆系统中事实分散、关系演化以及构建与检索分离导致的问题，提出了 Memory as Code (MaC) 框架。核心动机是：现有记忆构建方法（如 MemoryOS、Mem0、A-Mem）虽然能组织记忆数据，但缺乏对存储事实的计算封装，无法跨查询复用；现有检索方法要么使用预定义管道（灵活性差），要么依赖 agentic 逐步规划（开销大）。作者认为，代码的模块化与可组合性恰好能解决这一矛盾，因此提出用共享代码抽象统一记忆构建与检索。

技术路线上，MaC 包含两个互补组件。第一是编码化构建（codified construction），它将记忆组织为三层可执行结构：原子函数层封装单个事实或基本操作，关系层表示事实间随时间演化的关系，模块组合层将常用计算组合为可复用模块。这种结构使记忆不仅是数据，更是可执行的计算单元。第二是声明式检索（declarative retrieval），它将用户查询编译为结构化检索程序 Pq，程序包含操作符、参数、依赖和条件延续；执行器检查并运行程序，中间结果决定条件分支，可触发额外记忆访问。检索过程包括源上下文定位、证据组合和验证。通过这种方式，MaC 将检索从相关性事实选择转变为声明式查询处理，避免了逐步 LLM 规划的高开销。

实验部分，作者在 LOCOMO 和 LONGMEMEVAL 两个基准上，与 A-Mem、MemoryOS、Mem0、MRAgent 四个代表性 baseline 进行比较。使用 F1 和 LLM-Judge 分数作为指标，在 LOCOMO 上使用三种 LLM backbone（Qwen3.6-27B、ChatGPT-5.6-Luna、DeepSeek-V4-Flash），在 LONGMEMEVAL 上使用 Qwen3.6-27B。实验围绕三个研究问题展开：总体性能、成本/运行时/存储、消融研究。结果显示，MaC 在所有设置中取得最高 F1 和 LLM-judge 分数，相对最强 baseline 的 LLM-judge 准确率最高提升 15.91%，同时大幅降低 token 消耗。消融实验表明，启用声...**

<!-- paperflow:43ca00b32de4f434 -->
## Matrix-Game 4.0: Harnessing Video Models for Real-Time Playable Game Worlds

[[Deep Reading - Oct 2026/Matrix-Game 4.0-Harnessing Video Models for Real-Time Playable Game Worlds|Deep Reading]]

[https://arxiv.org/pdf/2610.27999](https://arxiv.org/pdf/2610.27999)

- **Matrix-Game 4.0 是一篇提出“可玩游戏世界”新架构的 arXiv 预印本，其核心论点是：要让生成式环境从“可观看/可短暂探索”升级为“真正可玩”，必须放弃让视频生成模型独自承担世界状态的做法，转而把游戏执行与视觉生成分离。

论证主线：论文从交互世界模型（Genie 系列等）的进展出发，指出这些系统能够即时生成交互环境，但一旦目标是长时间运行、玩法丰富、规则可控的游戏，纯视频生成路线会暴露四个系统性挑战：没有持续外部校正、误差累积、未压缩的帧记忆、玩法机制有限。更根本地，扩散模型学到的是统计相关性，视觉真实感不意味着因果或物理正确。因此，论文提出下一代 Matrix-Game 的路线图：一个可执行游戏 harness 把引擎和渲染器分开，同时引入多模态编码智能体打通自然语言到代码和字幕的通道。

技术主线：系统由三个组件构成。轻量级游戏引擎维护世界状态、执行代码化的规则，并向渲染器发送场景的结构化渲染（低多边形代理）；多模态编码智能体把玩家想法翻译成游戏代码和动态场景字幕；渲染器基于自回归视频模型，以低多边形代理为结构条件、以动态字幕和记忆引用为语义条件，把代理画面升级为高保真视频帧。代理提供结构（位置、布局、运动），字幕提供语义（有哪些物体、状态如何），两者共同约束生成。渲染器在“低多边形—高保真”对齐视频对上训练，蒸馏成因果生成器，每次生成一个 latent frame（四帧视频帧），每 chunk 只用一或两步去噪。推理侧使用三类 KV 缓存（空间记忆帧、参考帧、最近 20 个 chunk），把历史信息压缩为空间记忆，避免帧记忆无限增长；在线蒸馏以 fake score 每步更新、student 每五步更新（学习率 4×10⁻⁷ 和 2×10⁻⁶）的方式持续校准。

实验主线：论文为三个游戏（城堡射击、沙漠射击、城市驾驶）各训练一个渲染模型，在 480×864 分辨率下从 ground-truth 第一帧开始自回归运行 961 帧（40 秒），条件为 held-out 代理和字幕。与四个结构化/代码世界交互视频系统相比，Matrix-Game 4.0...**

<!-- paperflow:693845f86018e885 -->
## ReuF2F: Benchmarking Coding Agents on Formalized Undergraduate Research Problems

[[Deep Reading - Oct 2026/ReuF2F-Benchmarking Coding Agents on Formalized Undergraduate Research Problems|Deep Reading]]

[https://arxiv.org/pdf/2610.27046](https://arxiv.org/pdf/2610.27046)

- **论文针对 AI 在数学研究中快速进步所带来的评估难题，提出并构建了 REUF2F 基准。
论证主线：作者观察到，大语言模型在数学和代码任务上已通过 RLVR 等范式取得显著进展，从解决竞赛题扩展到攻克长期开放问题（如单位距离猜想反例、六维球面复结构、Navier-Stokes 有限时间爆破等）。然而，这些突破也带来了评估危机：自然语言证明的正确性难以大规模自动验证；而形式化证明（Lean 4）虽然能提供机器可检查的证明，但已有基准要么聚焦已知结果（容易被训练数据覆盖），要么只包含少数著名开放问题（一经解决即失效），且评估协议多采用填空式补全，不符合现代 coding agent 在真实代码仓库中迭代工作的习惯。因此需要一个“不容易被污染、可长期使用、与现代 agent 工作流对齐”的开放问题评测基准。
技术主线：作者从 REU 项目中精心选取 244 个开放猜想，由专家将其形式化为 Lean 4 语句，每个问题与其否定放入独立 Lean 项目。他们设计了统一的评估 harness Leanimum-agent（基于 mini-SWE-agent），提供 Docker 容器隔离、工具调用、补丁生成与自动验证全流程。验证端采用 Comparator：同时依赖 Lean 官方内核与独立检查器 nanoda，确保提交的证明或反驳不仅编译通过，而且不存在依赖不可信公理或被元编程规避核验的风险。评估协议允许任一方向（证明或反驳）被接受，体现了对开放问题“真值未知”的认识。
实验主线：在统一的宽松预算（1000 步、6 小时、无金钱限制）下评估了多个前沿模型。结果显示最佳模型 Gemini 4 Argon 的 pass@1 为 42.42%，pass@2 为 44.67%，即约 60% 的开放问题在给定预算下仍未被解决。论文还分析了测试时推理努力的规模效应、跨领域和跨解决类型（证明 vs 反驳）的表现差异，并讨论了形式化忠实性和不可判定性对结果解读的影响。
结论：REUF2F 可以作为跟踪 coding agent 在开放数学研究上能否产生新的、经过验证的结果的可复现基础。当前模型表...**

<!-- paperflow:b64db65928f30160 -->
## NeoWorld-3: From Pixels to Code-Defined Physical Worlds

[[Deep Reading - Oct 2026/NeoWorld-3-From Pixels to Code-Defined Physical Worlds|Deep Reading]]

[https://arxiv.org/pdf/2610.27690](https://arxiv.org/pdf/2610.27690)

- **### 论证主线
论文从“仿真就绪的世界重建需要物理交互性”这一核心需求出发，指出现有黑盒 3D 生成方法只关注视觉真实感，无法提供可编辑、可验证、可执行的物理世界。作者主张将重建问题转化为程序合成问题，用代码定义物理世界，从而让智能体能够像软件工程师一样检查、编辑和调试 3D 世界。这一立场与近年来“可微渲染+物理仿真”的趋势一致，但更激进地引入了编码智能体作为核心驱动力。

### 技术主线
NeoWorld-3 的架构由三部分组成：
1. **NeoSDK**：底层计算引擎，提供几何处理、部件组合、可微参数细化和物理验证。它相当于一个“物理世界编译器”，将高层程序编译为可仿真的 3D 场景。
2. **NeoMCP**：将 NeoSDK 能力封装为工具调用 API，供视觉语言智能体使用。API 设计遵循 MCP（Model Context Protocol）风格，支持 inspect、execute、edit、repose 等操作，使智能体能够进行视觉检查、代码编辑和迭代精化。
3. **Coordinator-Worker 架构**：协调者负责场景分解和任务分配，工作者负责具体物体重建，从而支持从独立物体到复杂场景的扩展。

迭代精化循环是系统的核心工作流：智能体从视觉观测初始化程序，执行生成世界，通过视觉和物理检查评估误差，编辑代码，再执行验证。参数审计机制进一步帮助智能体定位关键误差源，但当前是开环的。

### 实验主线
实验分为三个层次：
1. **基准评估**：在物体级和场景级重建基准上，NeoWorld-3 在几何保真度和物理就绪性上均达到 SOTA，证明代码定义世界的有效性。
2. **组件验证**：单臂立方体放置实验展示了局部视觉检查对位姿的显著校正（5.3 mm、6.7°，中位表面残差 1.8 mm）；repose 实验验证了场景可编辑性；参数审计实验揭示了误差主要来源于少数物理参数。
3. **系统级验证**：物理双臂机器人零样本执行成功，完成了 Real-Sim-Real 闭环，这是最具说服力的结果，表明重建世界与真实物理环境高度一致。...**

<!-- paperflow:9ec9a8bf0f9bdec0 -->
## Harness Self-Evolution for Long-Horizon Terminal Agents

[[Deep Reading - Oct 2026/Harness Self-Evolution for Long-Horizon Terminal Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.27435](https://arxiv.org/pdf/2610.27435)

- **论文提出 RiseHarness，目标是在不更新模型权重的前提下，通过迭代改进 agent 的运行时层（harness），同时提升长时程终端任务的平均成功率与重复运行一致性。论证主线是：平均成功率高不代表执行可靠，因为同一模型同一任务多次运行可能因局部错误改变环境与上下文而产生不同结果；因此需要把“系统能否稳定支持成功执行”作为独立的研究对象。技术主线是一个闭环演化框架：从 Terminus-2 种子 harness 开始，在 TBH 开发集上重复运行任务，筛选“flipping tasks”（同一配置下既有成功又有失败的任务），配对对比成功与失败轨迹，同时利用 harness 内部 traces 与 Harness Handbook 将执行问题定位到具体交互行为与实现；随后检索其他 harness 框架的相关设计，结合当前执行流程、可用信号与模型行为生成提案；通过实现检查与开发集上的全新重复评估决定是否采纳，被采纳的版本连同更新 Handbook 与执行证据进入下一轮。实验主线覆盖四个研究问题：主效果（RQ1）显示相对种子 harness 与 baselines，RiseHarness 在两个模型上都提升了任务成功率与重复可靠性；消融（RQ2）支持 harness 执行证据与外部设计研究的贡献；行为分析（RQ3）显示不同模型保留的机制有不同行为目标与干预机制（如 DeepSeek-V4-Flash 上 completion confirmation 机制的逐步累积）；迁移实验（RQ4）检验冻结模型与 harness 后提升能否推广到未参与演化的 benchmark。实验还观察到中间版本性能有波动，说明重复评估与采纳门控是必要的。局限与未来方向包括：模型、任务分布与框架覆盖有限，演化评估成本高，需要进一步降低。整体上，论文代表“模型能力固定、系统层持续优化”的研究范式，与 FIRE、AgentDebug、AgentRx 等失败感知运行时干预工作形成直接对话。**

<!-- paperflow:266b7be705f1c9df -->
## Sage-A1.5: From Long-Horizon Execution to Scientific Exploration with Capability Composition

[[Deep Reading - Oct 2026/Sage-A1.5-From Long-Horizon Execution to Scientific Exploration with Capability Composition|Deep Reading]]

[https://arxiv.org/pdf/2610.28578](https://arxiv.org/pdf/2610.28578)

- **Sage-A1.5 是继 Agents-A1 之后的一篇智能体模型能力扩展论文，其核心主张是：在‘能力覆盖’（模型能解决哪些领域任务）之外，还存在‘能力组合’（问题解决能力如何在执行中被组织、协调和自适应调整）这一正交维度，而后者同样可以成为模型训练与评测的显式目标。论文从长期执行的经验教训出发——更长的执行并不总是带来更好的结果，代理会重复无产出的搜索、依赖未经验证的假设、过早接受结论——指出仅增加领域技能或仅加长执行时程都不够，需要把任务委派、并行协作、证据复核和策略调整这些执行组织能力直接训练进模型。为此，Sage-A1.5 引入两大能力模块：Agentic Orchestration 负责工作如何分解、委派和整合，形成以共享答案为根的星形树图结构，主智能体把不同约束并行委派给独立上下文的并发子代理，再将分支描述与证据（经专名掩码）合并回主问题；Agentic Verification 负责确定何时重新评估中间判断与探索策略，验证反馈直接写入模型推理文本，使模型学习何时验证、验证什么以及验证结果如何影响后续动作，外部监督仅用于构造训练数据。训练采用扩展多轮 SFT，配合代码沙箱隔离执行、工具结果回放、可复现执行机制和 Search/Visit/Scholar 等工具，覆盖 coding 与 long-context 等多领域数据，任务构建横跨 LongBlocks 长文档场景（16K-128K token、24 个细粒度类别）与 SWE-rebench-V2 及内部构建的软件工程场景。论文通过基准评测与科学案例研究展示从孤立任务求解到长时程科学研究的转变，实验评估侧重高/中难度推理与研究类任务。总体上，Sage-A1.5 把‘执行组织’确立为与‘问题求解’并列的训练维度，为小规模模型实现科学探索级长时程行为提供了一条不依赖万亿参数规模的路径。**

<!-- paperflow:2c6377ea86489243 -->
## The Value of Memory in Language Agents

[[Deep Reading - Oct 2026/The Value of Memory in Language Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.31303v1](https://arxiv.org/pdf/2610.31303v1)

- **论文的论证主线可以概括为一次“价值追踪”：记忆的价值不是一个分数，而是一条从原始历史到最终决策的链条上的连续量，每一级都可能泄漏。

第一步是重新定义问题。作者指出，智能体记忆研究的常见问法是“加记忆能否提升下游指标”，但这个问法把三件事混在一起：持久状态里到底保留了什么、有界策略能否取用、评估器是否稳定。作者改用决策论语汇：记忆价值 = 避免的任务损失（units of avoided task loss），并区分原始价值（完整历史可达）与保留价值（持久状态可达）。关键拆分是 Vπ(M)（给定策略下的实际价值）、V*(M)（最优使用下的价值）与实现遗憾 Gπ(M)。这一拆分让“记忆本身无价值”和“智能体不会用记忆”成为两个可独立诊断的失败，而不是一个含混的“记忆没用”。

第二步是把成本引入。记忆不是免费的：写入要花成本，检索要花成本，需求变化后恢复或修订旧信息更要花成本。作者用风险-成本前沿刻画在给定预算下可达的保留与恢复水平，并指出不在前沿上的配置是纯粹浪费。当未来需求未知时，单一数字不足以刻画价值，于是引入价值剖面：记忆在一族可能需求下分别值多少。这一对象的实践含义是，稳健的记忆分配不应追逐某一需求下的最优点，而应在剖面上保持可恢复性。交互反馈则被处理为“改变未来工作分布”的操作，因此在线环境会持续重排记忆的优先级。

第三步是机制隔离。作者构造五个最小反例，每个只留最少变量：信息时序（写入时机决定信息是否还在）、检索访问（存了但取不到等于没存）、组件交互（记忆与 reader 的边际价值不可加）、反馈（改变未来需求分布）、需求不确定性（未知需求下的风险）。其中两个结论最具冲击力。其一是“零互信息指针”悖论：一个不含答案信息、与答案互信息为零的指针，仍能降低精确恢复的成本——因为它的价值在于缩短搜索路径，而非承载内容。这直接质疑以互信息或语义相关性作为记忆保留准则的惯例。其二是归因顺序依赖：在双项目反例中，memory-first 归因给记忆 0、reader 1/2，reader-first 归因给记忆 1/2、reader 0，交叉差 −1/2，证明“增量分...**

<!-- paperflow:6bdd6f6a3fec6600 -->
## Evolving with Open Worlds: Towards Recursive Self-Improvement for Future Forecasting

[[Deep Reading - Oct 2026/Evolving with Open Worlds-Towards Recursive Self-Improvement for Future Forecasting|Deep Reading]]

[https://arxiv.org/pdf/2610.29784v2](https://arxiv.org/pdf/2610.29784v2)

- **一、核心主张：从“此刻会什么”转向“能否从经验中变强”
论文开篇提出，智能通常被衡量为某一时刻的能力，但更重要的问题可能是系统能否通过自身经验变得更有能力。自然科学与社会科学中最强大的系统——生物演化、科学方法、人类专长——都依赖累积自我改进：纠正过去错误，复利过去收益。作者由此主张，理想 AI 系统应按能力如何形成和增长来评价，而 RSI 是自主 agent 与科学发现工具的前提。

二、问题：RSI 的瓶颈是环境
研究 RSI 不只是设计算法，还要有合适环境。agent 只有持续面对新挑战，且环境能无人工干预地判断是否进步，才能递归改进。固定数据集是惰性的：不更新挑战、不自动判分、容易饱和，无法支撑多轮自我改进。真实世界开放、非平稳、带噪声，但难以控制。未来事件预测因此成为天然 testbed：面向未来、结果事后可客观核验、与新闻自然 grounding、可每周再生、可按难度分层。

三、系统：三模块闭环
EvoForesight 由 PROMETHEUS、BRANCHUS、ATHENA 构成。PROMETHEUS 是 living data environment，持续吸收真实新闻，结合 FutureX 周更 benchmark 与 agent 驱动合成，生成可验证预测任务；任务经过时间验证、去重、冲突检测和多样性选择，真值只在事件发生后揭晓。BRANCHUS 是 agentic forecaster，策略 πθ(·|q,S) 输出概率预测 p-hat 与推理轨迹 τ；它把策略分解为 search team 与 predict team，前者收集和审查证据，后者汇总推理并给概率；技能库包含人工种子技能与从轨迹蒸馏的可复用技能，按角色和抽象层级检索。ATHENA 是 recursive-evolution engine，沿三条轴把轨迹转为能力：用 resolved outcome 做 RLVR 参数更新；把轨迹蒸馏为技能；分析失败并生成 weakness-targeted 的 task-synthesis 技能，使 PROMETHEUS 出更难的题。

四、闭环如何...**

<!-- paperflow:eb5036d589770053 -->
## ExpEvolve: Structuring and Reusing Experience for Open-Ended Discovery

[[Deep Reading - Oct 2026/ExpEvolve-Structuring and Reusing Experience for Open-Ended Discovery|Deep Reading]]

[https://arxiv.org/pdf/2610.29303v1](https://arxiv.org/pdf/2610.29303v1)

- **【论证主线】论文从 AI Scientist 与进化式编码代理的兴起切入：AlphaEvolve 一类系统让 LLM 迭代修改程序，在数学、算法发现等开放式问题上取得成功；但作者指出，这类系统对程序的变异主要是低层、表层启发式的，缺少一个可持久保留的控制单元。科学知识不是一堆代码片段的堆积，而是想法在假设检验中不断累积。于是论文提出核心问题：能否把“想法”作为经验复用单位，让进化式发现既避免重复错误，又能在平台期提出新方向？
【技术主线】ExpEvolve 的做法是在现有进化骨架之上加一层想法控制。它从程序归档中抽取高层 idea factors，用这些因子解释程序之间的得分差异；具体地，对已标注因子的程序对拟合 pairwise ranker，让排序器预测哪个程序更好。随后，系统挑选那些分数差距大、但当前因子无法解释的程序对，把它们视为“缺少某个想法因子”的信号，继续抽取新因子。这样，因子库不是一次性摘要，而是随搜索逐步扩展的持久记忆。因子反过来控制搜索：在利用阶段，用因子指导候选生成与选择；在发现进入平台期时，用因子空间中的未充分探索区域引导探索。论文还把该框架嵌入 SCS/OpenRSI 的 Draft、Improve、Debug、Crossover 算子，并用 warm-up 多样化、想法记忆摘要和成功程序示例等设计来稳定早期搜索。
【理论主线】论文声称刻画了“基于想法因子的发现何时能提升样本效率”的条件。可确认的是，理论关注的是：当高层因子能够解释程序得分差异时，发现和复用这些因子能否减少试错；但可见片段没有给出定理的正式假设、界或证明结构。因此，读者需要回到 Method/Theory 部分核对：因子空间的容量、抽取噪声、排序器误差、探索成本如何进入样本复杂度；条件是否能在实验中度量；以及结论是渐近的还是有限预算的。
【实验主线】实验覆盖 11 个开放式数学任务：9 个来自 SCS/AlphaEvolve 的问题，包括第一、第二自相关不等式、不确定性不等式、Erdős 最小重叠、集合和差、最大最小距离比、三角形 Heilbronn 问题、11 维亲吻数、圆...**

<!-- paperflow:18297e98574de0d2 -->
## Trace2World: Reconstructing App Worlds from Real Interaction Traces for Scalable GUI Agent Training

[[Deep Reading - Oct 2026/Trace2World-Reconstructing App Worlds from Real Interaction Traces for Scalable GUI Agent Traini|Deep Reading]]

[https://arxiv.org/pdf/2610.30792v1](https://arxiv.org/pdf/2610.30792v1)

- **论文的主论证是：GUI agent 的强化学习瓶颈已经从“有没有模拟器”转向“模拟器是否保留真实 app 的任务相关行为”。真实设备难以重置且有副作用，手工模拟器和 agentic coding 路线又容易把行为推断与代码实现混在一起，生成能跑但状态转移错误的 mock。Trace2World 的回应是把这两件事拆开：先从多条真实交互轨迹中重建一个共享的、带来源链接的 app 行为模型，再把模型编译成可执行环境，最后用源工作流独立回放来检验实现。
技术主线分为四层。第一层是轨迹预处理与行为支持判定：把截图、动作、页面、状态、数据和效果整理成证据记录，区分哪些语义被源轨迹支持、哪些仍未被解决；未被支持的效果不进入可执行范围，避免 coding agent 用看似合理的代码补全错误行为。第二层是跨轨迹语义协调：统一页面/实体身份、初始状态、数据字段、guard 和效果，把多条 partial trace 协调成一个共享 app 模型；上下文有界协调用于控制规模。第三层是编译与任务条件初始化：把模型导出为可执行环境，支持从源支持的中间状态重置，并处理跨 app 工作流。第四层是独立源验证与有界修复：在冻结行为模型的前提下，只允许修实现，不允许改语义；必须改模型才能通过的 workflow 被排除。
实验主线围绕三个问题展开。构建规模是 1,570 条轨迹、50 个 app、10 个 domain，每个 app 一个共享环境；40 个 app 用于策略训练，提供 1,280 个 seed 任务和 768 个 learner-guided 任务，10 个 app 只提供构建轨迹、不参与优化，构成 unseen-app split。保真度评测使用 200 条跨 10 个 app 的 workflow，六项检查覆盖可达性、界面表层、状态、效果、终态结果和数据，由盲评的 GPT-5.6 Sol reviewer 逐项判 P/F，缺失证据记为 F，并按 app 宏平均。Trace2World 端到端复现 91.1%，最强匹配的任务局部基线为 59.8%。
真机迁移实验是论文最有说服力的部...**

<!-- paperflow:b2e9c0b1f64dd4ba -->
## Superintelligent Software Engineering: Governance That Scales with Capability

[[Deep Reading - Oct 2026/Superintelligent Software Engineering-Governance That Scales with Capability|Deep Reading]]

[https://arxiv.org/pdf/2610.31306v1](https://arxiv.org/pdf/2610.31306v1)

- **一、论文定位
这是一篇立场/框架型论文，不是完整的实证系统论文。它的问题不是“如何训练更强的代码 agent”，而是“当 agent 能力超过人类工程师时，软件工程治理如何不被吞吐量压垮”。作者给出一个 stipulative 定义：超级智能软件工程是构建和演化软件的学科，其 agent 能力超过人类工程师，而治理——门禁、契约、决策和衰减控制——必须随能力扩展。

二、论证主线
论文用三条 claim 串起论证。第一，项目正确性依赖代码不承载的决策与契约；只读代码的 agent 可能产出测试通过但违反未成文约定的变更。第二，知识会衰减；文档、路径、架构假设和依赖约束会过期。第三，一旦 agent 的产出速度超过审查者，人工阅读只能覆盖越来越小比例的 stale-built 变更。作者用一个简单模型把这三条连起来：r 是吞吐量，c 是固定审查容量，s 是 stale 知识概率；r > c 后，阅读覆盖按 c/r 下降，逃逸变更随 r 线性增长，治理可以降低斜率，单靠能力不能。图注明确说这是模型而非测量。

三、经验快照
论文对 452 个最高星标公共 GitHub 仓库做了探索性快照：48.67%（95% CI 44.1–53.3）在根目录或 .github 下有面向编码 agent 的 context 文件；在命名仓库路径的中位文件中，按不同引用读取方式，9.5%–22% 的路径无法解析。这两个数字分别支持“agent 上下文实践已相当普遍”和“agent 依赖的知识入口存在可见腐烂”。但它们是横截面、相关性、窄代理，不能证明 OSTEON 有效，也不能直接外推到所有项目。

四、OSTEON
作者提出 OSTEON 作为该 regime 的 discipline：用受治理的知识库承载决策与契约；对每次变更执行机械门禁（ρ=1）；在每个 session 开始时检查知识衰减，以降低 s。其核心不是“多写文档”，而是把可机械判定的治理从人工阅读中剥离，并让治理覆盖随 agent 吞吐量扩展。工件以 DOI 10.5281/zenodo.23436020 发布。

五、贡献...**

<!-- paperflow:8127c0336aac5a1a -->
## ColaBench: Evaluating Memory-Guided Knowledge Acquisition in Personal Agents

[[Deep Reading - Oct 2026/ColaBench-Evaluating Memory-Guided Knowledge Acquisition in Personal Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.30989v1](https://arxiv.org/pdf/2610.30989v1)

- **论文的主线可以概括为：个人智能体的个性化不是“把历史塞进上下文”那么简单，而是要解决记忆如何引导外部知识获取、以及获取到的知识如何应用到用户情境这两个耦合问题。作者用 ColaBench 把这条主线变成可追踪、可消融的实验。
1. 动机与问题。个人智能体要处理用户不断变化的请求；用户往往不会一次性说出所有经验、偏好与约束，而当前任务又可能需要新的外部知识。论文举了一个典型例子：用户问自己是否符合照护者 respite grant 的资格，曾在不同会话提到照顾阿尔茨海默病亲属并独居。智能体必须找到补助规则，并把“照护对象须与申请人同住”与“用户独居”这两条信息连起来；否则，即使检索到相关文档，也会给出不合适建议。由此，论文把问题拆成两个挑战：用历史条件识别所需知识，以及把知识应用到用户情境。现有基准分别推进记忆召回、个性化检索与记忆引导行动，但缺少联合评估。
2. 基准。ColaBench 是健康与日常生活领域的合成基准，含 349 个任务、10 个 Profile、13,033 篇文档。它的核心是 Evidence Graph：把个人条件连接到其在公开交互会话中的披露，记录用户此前对文档的曝光，并把答案要求连接到个人证据与新获取的文档证据。这样，每个任务都有可追踪的证据链，而不是只有问题与答案。构造上，论文先构建文档层（Stage 0），再从公开交互中抽取/组织个人条件与任务，并用审计门控过滤不满足条件的样本。片段中的 Blind solver、Final audit gate、Same evidence、New document、History hidden、No-history answer、Novelty Fail 等标签，说明基准试图保证任务确实需要记忆与新文档，而不是靠常识或单轮检索就能答对。
3. 方法。MemKB 是论文提出的基线方法。它用状态摘要把分散在多次会话中的个人条件组织起来，带入查询构造与规则应用；用记忆—文档链接检索与用户历史相关的知识；检索上采用语义、关键词与（合理推断）记忆链接三通道等权融合，每篇文档每通道最多贡献一次。MemKB 不是单轮...**

<!-- paperflow:1de9bbfc84385689 -->
## AnyGameRenderer: Entity-Consistent Generative Rendering Across Games and an Entity-Level Benchmark

[[Deep Reading - Oct 2026/AnyGameRenderer-Entity-Consistent Generative Rendering Across Games and an Entity-Level Benchmar|Deep Reading]]

[https://arxiv.org/pdf/2610.31466v1](https://arxiv.org/pdf/2610.31466v1)

- **一、动机与问题定位。论文从一个正在发生的现象出发：AI coding agent 已经能在几分钟内构建可玩的小游戏，GEAR、OpenGame、Recursive Game Creator 等系统可以端到端产出游戏逻辑，但画面仍是 programmer art——灰色 blockout、彩色几何 primitive、扁平 semantic blob。逻辑与视觉之间出现了一个明确的缺口。作者没有选择「让 agent 学会画美术资产」，也没有选择「用世界模型替代引擎」，而是提出一个专门的渲染层：把粗糙 gameplay 录像翻译成高质量画面，同时严格保持结构。这个定位的关键在于，它把「生成」的自由度限制在 appearance 上，把 layout、camera、entity position 和 timing 交给输入视频。

二、任务形式化。给定 Vin ∈ R^(T×H×W×3) 和文本 c（目标风格 + 每个粗糙实体的语义），输出同长度同分辨率的 Vout，满足三条约束：相机、布局、timing 逐帧匹配；每个粗糙实体变成完成版对应物，位置、视大小、朝向一致，出现和消失帧一致；外观遵循 c 与可选 anchor image Ia。输入是 plain video，不需要 engine G-buffers、per-object class labels 或 low-poly proxy；gray blockout、colored primitive、semantic map、wireframe 都可接受。这个「低门槛输入」的设计直接对应 agent 生成游戏的实际可得性。

三、方法主线。AnyGameRenderer 用 LoRA 微调 MiniMax-H3 ref2va，一个 33B 的 reference-video-conditioned 视频生成模型；backbone 冻结，LoRA 挂在 DiT block 的 self-attention 和 feed-forward 上。训练数据约 10,330 clips、14.8 小时，来自三个来源：GEAR 的 m...**

<!-- paperflow:ec6a68f1b6d39444 -->
## Can an Agent Debug a Web Corpus? Agentic Auditing of Slovak FineWeb2

[[Deep Reading - Oct 2026/Can an Agent Debug a Web Corpus-Agentic Auditing of Slovak FineWeb2|Deep Reading]]

[https://arxiv.org/pdf/2610.31636v1](https://arxiv.org/pdf/2610.31636v1)

- **一、论文要解决的问题
现代预训练语料由流水线生产，而不是人工编纂。FineWeb2 这类多语言 pipeline 用同一套配方（语言识别、质量启发式、去重）覆盖一千多种语言，输出只由步骤定义、不由语言定义。结果是单语言的失败模式在设计阶段不可预见，只能由下游用户在使用中遭遇；FineWeb2 明确把「复核某语言过滤效果」的责任留给关心该语言的用户。论文抓住的具体漏洞是近亲语言污染：FineWeb2 对抗它的唯一手段是一张高亲和度词表过滤器，而该过滤器只对实测污染率超过 10% 的语言启用。斯洛伐克语是中等资源语言，拥有体量巨大且形态相近的邻居捷克语，却没有触发阈值，因而完全落在防护之外。而污染率本身是由 pipeline 自己的语言识别器测出来的——用可能在近亲语言对上失准的信号，去决定是否启用纠错机制，这是一个循环依赖式的设计脆弱性。

二、为什么既有两条路线都不够
第一条是人工审计（Kreutzer et al., 2022；Caswell et al., 2020），近期仍有单语言审计产生实际价值（Signoroni & Rychlý, 2026；Tatariya et al., 2026）。论文承认其不可替代，但指出三个不可扩展性：审计是一次性产物，描述某一时刻的快照；不会组合成可在下一次 crawl 上复跑的机器；线性消耗专家注意力。第二条是把 LLM 当分类器铺到全量数据上，这把 curation 问题变成推理账单（一次 pass $12k–$90k），而且「即使花了钱也什么都解释不了」。

三、方法：LLM 作为一次性的语料调试器
论文提出第三条路。LLM 只用一次，用途不是打标签而是调试。具体形态：一个工具使用型 agent 在只读、带预算的 API 后探索语料（检视预算 300 次，每次返回 400 字符视图），提出关于缺陷的假设并用工具验证，最终把每条发现编译成一个可执行 selector——形如布尔查询，可含语言识别字段、域名模式与否定条件（片段中的一个实例是 langdetect='cs' AND lingua_top='cs' OR domai...**

# Computer Vision

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Computer Vision
- 方法：generation, language, reasoning, retrieval, machine-learning, multimodal-learning, multimodal-reasoning, deep-learning
- 论文/报告：21 篇
- UniData: Universal Multimodal Instruction Generation Pipeline
- Rethinking Contrastive Loss in CLIP Post-training: A Complementary Framework with Frozen Text Encoder
- Learning to Retrieve: Internalizing Memory Retrieval for Video World Models
- Towards Unified Evaluation of Prompt Enhancers for Video Generation
- FastBench: Can Streaming VLMs Perceive High-Dynamic Real-World Streams?
- ThinkDrag: Semantic Drag-Based Image Editing with Visual Reasoning
- MVGraph: Benchmarking LVLMs on Evidence Composition across Multi-View Visual Graph
- OmniCompass: A Holistic Benchmark for Omni-Modal Video Grounding and Understanding
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:c93a92a9dc9e2e6f -->
## UniData: Universal Multimodal Instruction Generation Pipeline

[[Deep Reading - Oct 2026/UniData-Universal Multimodal Instruction Generation Pipeline|Deep Reading]]

[https://arxiv.org/pdf/2610.11363v1](https://arxiv.org/pdf/2610.11363v1)

- **UniData 针对 MLLM 训练数据稀缺和高昂人工成本问题，提出统一的通用多模态指令生成流水线。论文的论证主线是：真实世界 MLLM 应用需要多模态、多轮指令数据，但现有数据集多限于单输出模态，现有生成方法多限于单场景且难以多轮，因此需要一个既能覆盖多模态又能控制多轮质量的通用生成方案。技术主线是三步流水线：先把简单用户需求扩展成多个多样化事件，保证语义覆盖；再用 any-to-any 多模态大模型基于事件与已生成历史逐轮生成多模态指令，并通过特殊多模态 token 区分文本、图像、音频、视频等模态；最后在推理阶段基于指令轮次间相关性纠正无关与冗余的生成流。为训练该流水线，作者构建了同时包含多模态输入与输出的 UniDataset，并设计了包含事件过滤、话题池等环节的构建协议。实验主线是与现有指令生成流水线对比生成数据质量，在四个质量维度上 UniData 全面占优且优势在多个评测模型上方向一致，但幅度温和；评测还覆盖了音乐理解生成（MuMu-LLaMA）与文生图相关性等跨模态能力。论文的贡献包括：提出通用多模态指令生成流水线架构、引入事件扩展与跨轮相关性流纠正机制、构建 UniDataset、建立四维数据质量评测协议。局限方面：人工事件过滤仍需要人力；相对 baseline 的改进幅度有限；长程多轮一致性与更细粒度质量控制仍有空间。总体而言，这是一篇面向多模态数据引擎的系统性工作，把数据生成从单模态单轮推向多模态多轮，并用多维评测验证数据质量，而非直接提出新的 MLLM 架构。**

<!-- paperflow:b97bf2e377a5e33f -->
## Rethinking Contrastive Loss in CLIP Post-training: A Complementary Framework with Frozen Text Encoder

[[Deep Reading - Oct 2026/Rethinking Contrastive Loss in CLIP Post-training-A Complementary Framework with Frozen Text Enc|Deep Reading]]

[https://arxiv.org/pdf/2610.11374v1](https://arxiv.org/pdf/2610.11374v1)

- **【问题与动机】CLIP是视觉-语言基础模型中的基石，也是LLaVA等VLM的事实视觉编码器。尽管预训练侧已有SigLIP、LaCLIP、CLIPA、EVA-CLIP等改进，CLIP仍存在模态间隙和细粒度视觉盲区，且这些缺陷会传导到下游VLM。轻量后训练是精修CLIP的低成本路线，但最近的方法学共识认为标准对比损失在小batch后训练下会造成灾难性遗忘，因此CLIP-Refine改用图像-文本软标签自蒸馏并明确放弃对比目标，KUEA则引入外部教师蒸馏。本论文从这一共识切入，提出一个更准确的问题：被归因于负样本不足的遗忘，是否其实是由对比温度τ设置不当造成的？【核心发现】作者通过InfoNCE梯度的温度依赖分析指出，τ的幅度决定损失对样本对的加权与更新方向；当τ过大的时候梯度被不相关样本主导，后训练表现为遗忘；当τ足够小时，损失把更新集中在少数困难负样本上，对比后训练不但不遗忘，反而能精修预训练CLIP。因此“对比损失不适合后训练”这一断言被修正为“错误量级的温度不适合后训练”。作者在batch size 128到2048范围内调节温度，并将选定温度直接迁移到不同模型、数据集和目标函数（SigLIP），表明该结论具有一定普适性。【方法】基于上述发现，论文提出ComCLIP：一个单epoch、轻量的CLIP视觉编码器后训练配方。文本编码器全程冻结，精修后的视觉编码器保持原架构和推理成本，可drop-in替换任何基于原CLIP文本空间的系统。训练目标是三项损失的加权和：适当温度的InfoNCE对比损失负责图文对齐；对冻结CLIP视觉副本输出的MSE锚定损失负责维持模态内视觉表征稳定、抑制遗忘；来自冻结DINOv2教师的批量相似度矩阵关系蒸馏损失负责迁移细粒度视觉结构。三个损失分别作用于可分离的度量族，避免相互干扰。训练使用AdamW、学习率1e-6、权重衰减0.1、410步warm-up、全局batch size 1024、8张A800，1个epoch；由于per-sample增强，冻结教师特征在线前向计算而非离线缓存。【实验设置】学生为OpenAI CLIP ViT-B/16...**

<!-- paperflow:e8d983b1050adedf -->
## Learning to Retrieve: Internalizing Memory Retrieval for Video World Models

[[Deep Reading - Oct 2026/Learning to Retrieve-Internalizing Memory Retrieval for Video World Models|Deep Reading]]

[https://arxiv.org/pdf/2610.11444v1](https://arxiv.org/pdf/2610.11444v1)

- **论文主线：视频世界模型要求在给定参考帧与相机轨迹时生成可探索、3D 一致的场景视频，重访已观察区域时保持场景级一致是核心要求。已有两条路线各自受限：重建条件生成路线受重建误差传播影响；外部记忆检索路线将“是否检索/检索什么”的决策放在模型生成动力学之外，模型无法内在地习得检索行为。
技术主线：作者提出 Learning-to-Retrieve（L2R），将记忆检索内化为状态更新动力学的一部分。模型持久内部状态被重新用作历史记忆；相机引导的检索门控 γt 以相机位姿为条件决定从状态中检索哪些历史信息（检索什么）；检索触发器 rt 决定何时激活检索，并通过 3D 重可见性信号监督——先前观察内容重新进入当前视野时激活、否则保留现有上下文。检索到的上下文与相机轨迹一起送入状态更新规则 φ 参与生成。L2R 可插入预训练相机可控视频世界模型，无需外部记忆库、无需显式 3D 对应或 3D 条件注入；支持 GLA 与 GDN 两种状态更新规则。
实验主线：以 Wan2.1-T2V-1.3B 为基座、沿用 Echo-Memory 相机条件设置与静态场景探索数据集，采用 Replay（帧对齐 PSNR/SSIM/LPIPS）、In-domain loop closure、Open domain return (O-V，VL judge 评估) 三支协议评测记忆容量，并单独评测视频质量以确认记忆机制不损害保真度与相机控制。对比 VMem、Context as Memory (K=20)、Echo-Memory、VideoSSM、Context-as-Prompt、No Memory 等基线。关键结果：L2R-GLA 开放域返回 O-V 达 72.57，超过 VMem 7.84 分、超过 Context as Memory K=20 13.94 分；NYC Street 定性实验显示重访时场景一致性优于无记忆与 prompt 上下文方法。训练语料包含无回环 5 秒片段以隔离验证记忆模块不以牺牲视觉保真度为代价。结论：L2R 在消除外部记忆检索器与显式 3D 条件的同时显著改善长程场景一致性...**

<!-- paperflow:a0b818e62e4a1585 -->
## Towards Unified Evaluation of Prompt Enhancers for Video Generation

[[Deep Reading - Oct 2026/Towards Unified Evaluation of Prompt Enhancers for Video Generation|Deep Reading]]

[https://arxiv.org/pdf/2610.11736v1](https://arxiv.org/pdf/2610.11736v1)

- **论文《Towards Unified Evaluation of Prompt Enhancers for Video Generation》围绕一个核心主张展开：提示增强器（PE）应当被直接评估，而不是通过渲染视频间接评估；为了支撑这一主张，作者构建了第一个统一的PE直接评估基准PEBench，并给出了系统性的方法对比与可靠性验证。
一、论证主线。论文从视频生成生态的现状出发：现代视频生成器已经能够执行时序结构化和多模态条件的内容生成，能够理解包含时间推进、机位设计、叙事发展的复杂指令，这使得PE从可选的提示润色工具上升为把简洁用户意图转译为结构化电影计划的关键桥梁。然而，PE的评估却停留在渲染视频的路径上：现有视频生成基准评估的都是视频画面本身，要评估PE必须先跑一次生成，由此产生三项不可忽视的代价——高昂的计算与人力成本、缓慢的迭代周期、以及PE质量与下游生成器行为无法分离的归因混淆。论文因此把PE评估从生成后置评估变为生成前直接评估，并建立统一的任务定义（T2V-PE、I2V-PE、R2V-PE）与统一的评分标准。论证的关键一环是回应直接评估的可信度质疑：作者通过人类验证，证明PEBench分数与专家对增强提示本身的判断一致，也与专家对Wan3.0和MiniMax-H3生成视频的判断一致，从而论证提示级评估能可靠反映下游效用。
二、技术主线。PEBench的构建分两层。数据层包含1,100个专家核验用例与1,005个视觉资产，覆盖35类细粒度任务，任务维度刻意覆盖时间结构（单镜头到多镜头）、电影化表达、视听协调和多参考需求，并纳入不可行请求、歧义指令、冗余资产等真实退化场景。任务定义按参考条件分三类：T2V-PE无参考资产，I2V-PE给定首帧图像，R2V-PE给定多模态参考。评估层采用证据导向框架：先通过模态感知事实抽取收集可核验证据，再按24条rubric准则打分；24条准则组织为语义保真、内部一致性、导演式创作三个维度，其中导演式创作维度涵盖Storyboard and Timing Completeness、Camera Design Specifici...**

<!-- paperflow:173d43a219c485c4 -->
## FastBench: Can Streaming VLMs Perceive High-Dynamic Real-World Streams?

[[Deep Reading - Oct 2026/FastBench-Can Streaming VLMs Perceive High-Dynamic Real-World Streams|Deep Reading]]

[https://arxiv.org/pdf/2610.12427v1](https://arxiv.org/pdf/2610.12427v1)

- **FastBench 是首个专门针对高动态真实世界视频流、评估流式 VLM 感知能力的基准，并配套提出 ProactiveFrame 这一无训练主动采样基线。论文从三个层次构建了完整研究：

首先，动机上，作者指出现有流式 VLM 基准大多基于低动态场景，在 1–2 FPS 稀疏采样下即可完成大部分问答，因此无法暴露模型对快速事件的感知缺陷。而真实世界应用（如监控、体育、机械过程）中，关键信息往往在几百毫秒内发生，稀疏采样会直接漏掉。这引出流式模型在固定上下文预算下的核心权衡：时间历史、空间分辨率、时间采样密度三者不可兼得。

其次，方法上，FastBench 采用轨迹接地（trajectory-grounded）管道来保证问题真正'动态'：从高 FPS 片段生成问题和答案，自动过滤在 2 FPS 下就能回答的伪动态问题；用 SAM3 和 CoTracker3 提取对象分割与运动轨迹，验证答案是否可以被视觉证据支持；再经过三轮人工检查，并标注每个问题对应的证据时间区间。最终得到 306 个 QA 对，覆盖 8 个领域、6 种能力、3 种时间范围。同时，ProactiveFrame 通过让模型输出文本 token 来控制后续帧率，并采用双层滑动窗口：近期观测保持高 FPS，较早历史降采样为稀疏表示，以固定上下文预算。

然后，实验上，作者在多个闭源/开源模型和主动感知方法上进行评测。结果显示：最强模型 Gemini-3.5-Flash 仅 50.7%；Qwen3-VL 从 2 FPS 的 32.9% 提到 24 FPS 的 44.6%，但收益随历史压缩而饱和；ProactiveFrame 比均匀稀疏采样高 5.4 / 1.5 个百分点，但仍低于 oracle 引导聚焦。按能力和时间范围分解的分数进一步显示：许多模型在前向任务尚可、但即时/后向任务很差；VideoChat3 这类主动感知方法完全没有即时和后向能力；模型在时空跟踪、在线检测等需要精细运动分析的能力上普遍薄弱。案例中，强模型对'爆炸火花'只给出通用解释，说明没有真正看到关键因果链。

最后，作者总结：高动态真实世...**

<!-- paperflow:9bbb0c2f4c40b29d -->
## ThinkDrag: Semantic Drag-Based Image Editing with Visual Reasoning

[[Deep Reading - Oct 2026/ThinkDrag-Semantic Drag-Based Image Editing with Visual Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2610.13316](https://arxiv.org/pdf/2610.13316)

- **ThinkDrag 的核心论点是：拖拽式图像编辑不应被建模为几何位移，而应被建模为一个从稀疏点约束出发的语义编辑推断问题。论文的论证主线是——现有优化式、一步式、前馈式三类方法都把 handle-target 点对当作位移向量，依赖基础生成先验保证合理性；但同一位移在不同语义场景下对应不同合理编辑（如加长蜡烛身体 vs 延长火焰、旋转狗头 vs 移动狗头），因此需要模型具备视觉理解与语义推理能力。技术主线是：构建统一多模态框架，联合输入图像、拖拽点约束与可选文本条件，训练模型将点约束与物体级/场景级变换关联；引入 Drag Endpoint Markers 提供显式空间锚定信号（该设计受跨图像注意力分析启发，消融显示无 DEM 时模型缺乏把编辑内容对齐到目标点的空间精度）；模型可选输出显式推理轨迹以解释编辑意图，提升模糊情景下的可解释性。数据主线是：用 Sec.3.1 的 pipeline 构建带推理轨迹标注的语义拖拽变换监督数据集，并在此基础上构建 DragBench++（145 个样本，人工标注拖拽，更多拖拽数与更长位移，含参考编辑解）。实验主线是：在 DragBench 与 DragBench++ 上与含 GDrag、DragDiffusion 在内的 SOTA 方法比较，双轨评测编辑准确度（MD/DIFT）与生成质量（IF/FID），并在 DragBench++ 上指出 DIFT 在语义模糊与大结构变化下不可靠因而改用基于参考编辑的 IF* 作为主要保真度指标；消融研究（Table 4）验证推理模式与文本条件的贡献，附录包含用户偏好研究（总体偏好、结果随机排序）与数据集细节。主要结论是：ThinkDrag 在生成质量与合理性上达到 SOTA，点跟随精度保持竞争力，推理模式提供额外可解释性，尤其对模糊多拖拽编辑。**

<!-- paperflow:07a39e244b2088b4 -->
## MVGraph: Benchmarking LVLMs on Evidence Composition across Multi-View Visual Graph

[[Deep Reading - Oct 2026/MVGraph-Benchmarking LVLMs on Evidence Composition across Multi-View Visual Graph|Deep Reading]]

[https://arxiv.org/pdf/2610.12961](https://arxiv.org/pdf/2610.12961)

- **MVGraph 是一个面向 LVLM 的多视图视觉图推理评测基准。论文的论证主线是：真实世界图查询的证据往往分散在多个相互关联的图视图中，而现有单图基准忽略了“跨视图证据组合”这一关键能力。为此，论文从部分视图与时间视图两个场景抽象出 8 个图论任务，程序化生成并验证 8k 个端到端实例，保证每个问题都无法从单一视图求解；节点使用语义中性且跨视图一致的 E* 标识符、关系使用 R* 标识符，同时各视图独立布局，排除位置对齐的捷径。为了解释端到端失败原因，论文将多图推理分解为感知、对齐、组合、推理四阶段，设计 29 个诊断任务与 64k 实例，并为任务提供黄金中间表示以评估中间过程。在 15 个 LVLM 上评测后，论文报告了三个核心发现：第一，当前模型在多图推理端到端任务上表现困难；第二，阶段化诊断表明跨视图边对齐与关系级组合是主要瓶颈；第三，提供显式结构信息能改善部分视图任务，但时间耦合推理仍然困难，并且随视图数量增加多个模型的组合性能下降。论文由此提出关系级对齐、忠实证据组合与时间推理是改进多图理解的重要方向。论文还提及使用 ChatGPT 进行语言润色，作者对最终内容负责；论文当前以 ICLR 2027 审稿论文形式发布（arXiv 2610.12961v1）。**

<!-- paperflow:3754d765363f25e0 -->
## OmniCompass: A Holistic Benchmark for Omni-Modal Video Grounding and Understanding

[[Deep Reading - Oct 2026/OmniCompass-A Holistic Benchmark for Omni-Modal Video Grounding and Understanding|Deep Reading]]

[https://arxiv.org/pdf/2610.18829v1](https://arxiv.org/pdf/2610.18829v1)

- **OmniCompass 是一篇以“系统诊断全模态视频理解”为核心目标的基准论文。它的出发点是人类感知的多模态本质：人通过语言或视觉线索锁定目标，在时间和空间中整合视觉与听觉证据，最终形成判断。MLLM 的快速发展使得这类能力可以被模型化，但现有基准要么只覆盖视频帧视觉内容，要么只考虑文本查询，要么孤立地评测时间接地、空间接地或问答中的某一种任务，无法回答“模型到底能不能在真实复杂场景中完成全模态接地与理解”这一根本问题。

为此，论文设计了 OmniCompass 基准。它包含两条评测轨：VL 轨面向视觉-语言模型，AV 轨面向原生支持音频的全模态模型。每条轨都统一了三个互补任务：时间接地（TG）、空间接地（SG）和问答（QA），对应目标“何时出现、在哪里、发生了什么”。VL 轨的 QA 进一步细分为 5 个感知类别和 3 个推理类别，AV 轨为 3 个感知类别和 2 个推理类别，以支持认知层面的细粒度诊断。数据集方面，VL 轨包含 820 个视频、2,460 个实例，覆盖七个领域和短/中/长三种时长；其中叙事与音频信息丰富的 240 个视频、720 个实例构成 AV 轨。全部实例由人工标注并经过多轮质量验证。每个实例都有两种配对的目标规格：纯文本指定与文本+参考图像指定，二者对应相同目标、相同任务语义和相同标注。

评测协议上，论文采用了适配不同模型的帧采样策略（专有与大模型 64 帧、小模型 32 帧、GPT 系列 49 帧、VITA 16 帧），并在帧前附加秒级时间戳。除传统正确性指标外，论文提出一致性评分（CS），用于衡量同一实例在换用视觉指定后预测是否保持一致。基于 50 个 MLLM 的大规模实验得出若干关键结论：开源模型总体上仍落后于专有模型，音视频联合理解上的差距尤为突出；即使是领先模型，精确的时间与空间接地也仍然困难；模型的 QA 准确率较高时，其接地能力可能很差；模型在两种目标规格上的预测不一致；参考图像的影响是任务依赖的，并不总是正面；时间戳在两条轨上都能带来性能提升。

论文的贡献在于：提供了一个可以同时覆盖 QA、精确接地和跨引用一致性的统一评...**

<!-- paperflow:c23f4c8d2d9fbc6b -->
## WorldMemBench: A Comprehensive Benchmark for Memory of Video World Models

[[Deep Reading - Oct 2026/WorldMemBench-A Comprehensive Benchmark for Memory of Video World Models|Deep Reading]]

[https://arxiv.org/pdf/2610.19063v1](https://arxiv.org/pdf/2610.19063v1)

- **【论证主线：为什么需要世界记忆评测】
论文以视频生成的两条进展为起点：自回归视频生成已能产出更长更连贯的序列，相机控制技术则让视角与轨迹可控。二者结合催生了可持续交互探索的视频世界模型。在这种使用方式下，用户会反复把内容移出和移入视野，生成世界必须能与自身历史保持一致——既要保持已观察到的静态内容（人物、物体、环境），也要让画面外的事件按预期推进。论文明确指出：视频质量和局部时间连贯性都不足以保证这种能力，因此需要一个新的被评测维度——“世界记忆”。

【定义与分解】
世界记忆被定义为“在观察中断期间保持已确立内容并维持连贯状态演化的能力”，并进一步分解为：静态记忆（人类身份与外观、物体外观与几何、环境内容与几何）和动态记忆（事件进展、状态持续性）。这个分解的关键洞见在于：离开视野期间“正确的事情”不是一成不变，而是静态属性保持、动态属性演化，因此两类记忆必须分开测、分开报。

【技术主线：受控离场-重访协议】
为实现可对照的记忆评测，论文构造受控相机轨迹：初始观察 V → 第 i 次离开 Di → 指定重访 Ri，包含平移与旋转两类运动。静态记忆以固定初始参考为锚，动态记忆以期望状态转换为锚。整体评估组织为四个互补维度（一般视频质量、静态记忆、动态记忆、相机跟随），结合外观与几何指标、事件特定评估，并以视频质量与相机跟随为补充诊断来排除混杂因素。基准包含 260 个核心用例，并设计多回访配对（Nmulti）与跨轴配对（Ncross），把评估从“单次回访一致性”扩展到“多次回访累积行为”和“方向敏感性”。

【实验主线】
对 9 个代表性视频世界模型进行统一评估。主要发现：
1) 没有任何模型在全部维度上领先，不同模型优势各异（Tab. 2；如 HY-WorldPlay 在 7 项静态记忆指标上领先，来自表格残片的合理解读）；
2) 强静态保持不等于可靠的动态状态演化，两类记忆能力解耦；
3) 多次回访揭示首次返回之后的累积记忆退化；
4) 跨轴回访暴露对探索方向变化的额外敏感性；
5) 人类研究验证自动评估与人类判断对齐。
此外，在模型与生成设置固定条件下进行的受控上...**

<!-- paperflow:eece9cd420edd858 -->
## Game2Real-Bench: Do Toy Games Reveal and Transfer to Real-World Visual Reasoning?

[[Deep Reading - Oct 2026/Game2Real-Bench-Do Toy Games Reveal and Transfer to Real-World Visual Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2610.18834v1](https://arxiv.org/pdf/2610.18834v1)

- **论文《Game2Real-Bench: Do Toy Games Reveal and Transfer to Real-World Visual Reasoning?》研究一个评测与方法论问题：玩具游戏能否揭示并迁移到真实世界视觉推理。作者首先指出既有视觉推理评测的两难：真实世界数据复杂度高但难以控制和验证；程序化游戏可控可验证但缺乏真实感。两者之间的关系鲜有被直接研究。针对这个空白，论文提出 Game2Real-Bench，一个双设置基准，将程序化生成的玩具游戏任务与真实世界视觉推理样本按共享推理需求配对，而不是按视觉相似性配对。基准覆盖四个能力组：细粒度感知、空间推理、约束推理和序列规划；游戏侧包含 1,000 个从可执行任务规格独立生成并通过程序化验证答案的样本，真实世界侧包含 400 个来自多样化来源的样本。两个设置通过视觉证据、空间操作、约束和状态转换等推理需求对齐。论文用 25 个 VLM（22 个开源，3 个专有）进行零样本评估，发现游戏和真实世界表现高度相关：Pearson 和 Spearman 相关系数均为 0.935。同时，游戏任务通常更难，能在当前模型中提供更清晰的区分度。能力级别的分析显示，不同能力在两个设置中的难度分布会发生变化，说明虽然整体相关，但每个能力的具体难度曲线并不完全镜像。进一步，论文在四个模型上进行一次性上下文学习来考察游戏到真实的迁移，发现约束推理对部分模型有正向增益，而细粒度感知在游戏演示下一致下降。这表明游戏可以部分反映真实世界推理模式，且迁移效果是选择性的。论文还研究了思考模式的影响并分析了代表性失败案例。最后，作者构建了 Game2Real-Dataset，包含近 24K 样本，用于支持模型训练。总体来说，论文的主要贡献包括：提供了一个验证的游戏-真实配对基准，实现了能力级别的跨设置分析；通过大规模模型评估展示了游戏评测对真实世界推理的预测力；揭示了选择性迁移现象；并释放了可用于训练的数据集。该工作为在可控环境中研究真实世界视觉推理提供了新的工具，也为游戏到真实迁移的理解划出了边界。**

<!-- paperflow:8c3ace40ba0dff1d -->
## Data Density Scaling Laws for Image Self-Distillation

[[Deep Reading - Oct 2026/Data Density Scaling Laws for Image Self-Distillation|Deep Reading]]

[https://arxiv.org/pdf/2610.19013v1](https://arxiv.org/pdf/2610.19013v1)

- **本文系统研究了图像自蒸馏中数据密度（唯一样本数与重复次数的比值）对下游表示质量的影响，并针对现有缩放定律的不足提出了修正。论文的论证主线是：传统缩放定律（源自语言建模）默认增加唯一样本数总是有益，而重复暴露价值递减；但这一假设在视觉自蒸馏中可能失效，因为真实视觉语料存在大量语义冗余，且自蒸馏中的重复暴露通过随机增强产生不同训练信号，并非简单重放。

技术主线上，作者定义了三个可独立变化的量：唯一样本数 U、重复次数 R、总计算量 C（满足 C=U×R）。通过在固定 C 下变化 U 和 R，他们能够在七个数据集（Maxar、SATLAS、Waymo、nuPlan、Ego4D、ImageNet、YFCC15M）上解耦数据规模与重复密度的影响。训练采用 iBOT 自蒸馏方法，并使用 DINOv2 做方法泛化验证；优化器为 AdamW，全局 batch size 1600（8×200），分布式训练。下游评估使用 kNN 分类，覆盖遥感（Brick Kiln、EuroSAT、RESISC45、UCMerced、PatternNet、AID）和驾驶场景（BDD100K scene/weather、ACDC condition）等任务。

实验主线的核心发现是：在 Maxar 上，仅含全量池 1.5% 的随机子集在固定计算下比全量池平均下游 kNN 性能高出 3.11 个百分点；在 ImageNet 上减少唯一支持早期有害，在 Waymo 上几乎中性，在 Maxar 上显著有益。子集优势在三个模型规模上均出现，且不特定于 iBOT（DINOv2 也表现出类似现象）。现有 compute-only 和数据约束缩放定律无法一致解释这些行为，而作者提出的密度交互定律（修改版 Chinchilla 风格，显式包含 U 和 R 的交互）在部分外推区间给出了更好的预测，但收益并非在所有数据集上一致。论文还考察了子集选择和增强的影响，但摘要中未给出详细结论。

总体而言，本文通过严谨的实证设计揭示了数据密度在视觉自蒸馏中的非平凡作用，挑战了“更多唯一样本总是更好”的传统观念，并为数据-计算权衡提供...**

<!-- paperflow:fa203a4233234f7b -->
## To Align or Not To Align: Check Your COMPASS Before You Train

[[Deep Reading - Oct 2026/To Align or Not To Align-Check Your COMPASS Before You Train|Deep Reading]]

[https://arxiv.org/pdf/2610.18638v1](https://arxiv.org/pdf/2610.18638v1)

- **本文提出COMPASS，一个在训练前估计模型对兼容性的诊断工具。论证主线：作者从表示收敛的局部性出发，认为模型对是否兼容可以通过局部邻域一致性来预测，而不必等到昂贵的对齐或合并之后。技术主线：COMPASS将兼容性分解为两个轴——ease（特定对的k-NN重叠）和ceiling（训练范式允许的最大重叠），并基于理论分析（定理4.1）证明起始条件重叠控制有限时间收敛。实验主线：在72个跨模态对齐和392个任务算术合并实验中，COMPASS的估计与下游成功强相关（平均Pearson 0.91和0.70），相图提供了可操作的建议。论文还讨论了不同训练范式在不同应用中的适用性，以及相图作为可解释摘要的局限性。总体而言，COMPASS提供了一种低成本、理论驱动的模型对选择方法，有望减少组合预训练模型时的经验搜索成本。**

<!-- paperflow:e36bdeaec981fe5b -->
## DuplexBridge: Coordinating Understanding and Generation for Unified Image Editing

[[Deep Reading - Oct 2026/DuplexBridge-Coordinating Understanding and Generation for Unified Image Editing|Deep Reading]]

[https://arxiv.org/pdf/2610.18819v2](https://arxiv.org/pdf/2610.18819v2)

- **DuplexBridge 的完整论证主线、技术主线和实验主线可以概括如下。论证主线：复杂图像编辑需要的不是单一“把图像改好”的能力，而是两类可分离的能力——理解（U：把用户请求解释为可执行的编辑条件）与生成（G：把条件渲染为图像）。现有系统失败时无法回答“哪个角色错了”，因为任务级分数同时依赖两者；而常见的解决办法（外部理解模块 + 生成器）引入了额外的模块不一致问题。论文的核心主张是：统一模型内部已经同时拥有 U 和 G，因此可以用 U 增强 G 的提示（native prompt enhancement），并通过“source-grounded bridge”（指定目标、最终状态变化、保留约束的可检查条件）把两个角色连接在同一个公开条件对象上，使责任在机制上可分离、可检查。监督设计上，论文提出每个角色只对自己的局部责任负责：U 检查“条件是否表达原始请求”，G 检查“输出是否遵循实际接收的条件”，二者都不得直接替代对方的责任；同时用 reciprocal trust gates 在局部监督可信时保留局部学习、在需要全局结果时才放行全局任务反馈。这个设计在概念上对应一种“各司其职 + 有限共享”的联合适配原则。技术主线：系统由 native PE（统一模型自身理解增强）、structured SFT（在结构化 bridge 条件上微调）和 gated RL（门控强化学习）组成。关键机制包括：确定性模板枚举 + 语义 source binding 的 bridge 编译；prompt fidelity 与 execution check 两类局部检查；offline RM distillation 提供可用的奖励信号；reward-validity conditions 决定全局奖励何时可信；Oracle gates 用于上限估计。训练从共同学到的 Wan-Image 初始化出发，受控对比明确隔离了“门控有无”和“局部监督参照对象”两个变量。实验主线：论文构建 CE-Bench（1200 任务）衡量任务推导的变更与保留要求的联合满足（CCR = change 与 pre...**

<!-- paperflow:4901e7e983dd62d0 -->
## Meta-Video: Efficient Standardized Evaluation for Video Understanding

[[Deep Reading - Oct 2026/Meta-Video-Efficient Standardized Evaluation for Video Understanding|Deep Reading]]

[https://arxiv.org/pdf/2610.18535v1](https://arxiv.org/pdf/2610.18535v1)

- **Oct Video understanding benchmarks are the primary yardstick for measuring 13 progressto claim improvements.in video-languageThemodeling,reported benchmarkand a…

ier inputs each modality can attend to under causal attention, resulting in prompt patterns such as T-V-T or V-T-V. We evaluate these choices in Section 5.2.

Qwen2.5-VL-7B Datasets Meta-V (short / medium / long) RUNTIME Inference backend HF Batch size 1 (.mp4; .jpg with vt);

The Qwen models gain most between 4 and 32 frames. InternVL3.5-8B gains only 0.2 points from 64 to 128 frames, while InternVL3.5-4B peaks at 45.5 with 32 frames and falls to 43.9 at 128.

主要贡献包括：Qwen2.5-VL-7B Datasets Meta-V (short / medium / long) RUNTIME Inference backend HF Batch size 1 (.mp4;；.jpg with vt);；The Qwen models gain most between 4 and 32 frames.。

需要注意的边界包括：Vishaal Udandarao was supported by a Google PhD Fellowship in Machine Intelligence....**

<!-- paperflow:717bf8eeede9324d -->
## State World Models: Execute the Rules, Generate the Pixels

[[Deep Reading - Oct 2026/State World Models-Execute the Rules, Generate the Pixels|Deep Reading]]

[https://arxiv.org/pdf/2610.26811v1](https://arxiv.org/pdf/2610.26811v1)

- **论文围绕“视频世界模型的交互必须尊重隐藏状态与持久规则”这一主张展开。作者指出，视频生成已能产生连贯逼真的动作条件视频，交互式世界模型也支持导航和动作条件生成，但“响应式视频”不等于“符合规则要求的结果”：同样的开锁动作在玩家持有钥匙与否时应产生不同结局，而初始画面可以完全一致。为此论文提出 Stateful Harness，把世界演化与视频渲染彻底分离：一个显式维护的语义账本加上一个可编辑 3D 场景提供的空间状态，在确定性规则下结算每个动作，把结算后的事件编译为指令，再用冻结的视频模型渲染成片段，片段又作为视觉观察反馈更新状态，循环进入下一回合。这一设计使规则执行完全落在可验证的程序侧，视频模型只负责像素渲染。为了严格评测规则遵从，作者构建 WorldState-Bench：50 对反事实孪生样本，共享初始帧和动作但隐藏状态不同，联合评测要求模型在同一规则下为两种隐藏状态分别产出规定结局。评测了 11 个现成后端（4 个图生视频、7 个交互式世界模型），用 Qwen3.8-27B-VL 以 3fps 采样帧加末帧做判断。结果：仅规则提示下最强后端 MiniMax-H3 的 PAIRFOLLOW 只有 32%，显式指定结局可提升到 64%，完整 harness 在权重完全冻结下达到 92%；单个片段正确率高不代表孪生对同时正确，说明存在“孪生坍缩”。同一配方还支持 11-29 回合的多回合 episode 和空间触发器，覆盖城镇探索、约会叙事、回合制决斗三个体裁，每个体裁都是同一冻结模型上的新可执行游戏。结论是：显式状态与可执行规则是控制有状态渲染交互的有效路径，规则程序而非渲染器承担状态簿记。论文最后附带 AI 使用声明：生成式 AI 仅用于写作辅助和辅助工具脚本，不用于研究构思。**

<!-- paperflow:9766c14c6a55c1af -->
## HarnessRL: Co-Evolving Policies with a Just-in-Time Training Harness for Open-Ended RL

[[Deep Reading - Oct 2026/HarnessRL-Co-Evolving Policies with a Just-in-Time Training Harness for Open-Ended RL|Deep Reading]]

[https://arxiv.org/pdf/2610.26426v1](https://arxiv.org/pdf/2610.26426v1)

- **1. 出发点：基于 rubric 的 RL 是开放域生成后训练的常用方案，用自然语言标准作为奖励，但训练中奖励会失效。论文把失效拆成两类可操作的失败模式——非判别性奖励（分数坍缩）与虚假奖励（分数与质量脱钩）——并论证其共同根源是训练配置（任务采样、探索、评估）在策略演化中固定不变，形成“静态配置 vs 动态策略”的错配。
2. 方法主线：作者提出 HarnessRL，一个训练期 harness，与策略同步演化三个接口——任务采样、rollout 引导、rubric 标准。harness 维护训练期记忆（任务内历史与跨任务证据）；harness evolver 对每个奖励失败归因，选择“哪个接口的更新能修复这个失败”；proposer 生成候选更新，just-in-time critic 要求候选先作用于当前冻结策略、测得效果后才接受，被拒候选返回修订。这一“先验证后采纳”的机制是核心设计：harness 更新不会盲目作用于优化中的策略。
3. 数据与评测主线：论文构造 Rubric-Mix-10k——从 HealthBench、RaR、RubricHub、ResearchQA、LongWriter-Zero 抽取 8,600 训练任务、每域 250 held-out 任务与 1 个 OOD benchmark，覆盖医疗、科学、写作、角色扮演、指令跟随五域；共 110,606 条加权标准，35% 任务带参考答案，负权重标准占 10.7%。评测在 Qwen3.5-4B/9B 上进行，对比 GRPO、AMARIS++、SkillRL、RuscaRL 等 baseline，围绕 RQ1-RQ4（性能、效率、组件贡献、单任务行为）展开。
4. 结果主线：in-domain 超最强 baseline 2.8-3.1 分，OOD 最佳 checkpoint 领先 AMARIS++ +2.8 pp（matched-compute 下 +2.9 pp）；训练效率上，9B 下 HarnessRL 250 步到峰、GRPO 需 475 步，近乎减半，cluster hours 最少。训练动...**

<!-- paperflow:e46ddb9bb5b32e02 -->
## VisualEvolve: A Skill Self-Evolution Framework for Visual Reasoning

[[Deep Reading - Oct 2026/VisualEvolve-A Skill Self-Evolution Framework for Visual Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2610.28386](https://arxiv.org/pdf/2610.28386)

- **VisualEvolve 是一篇关于视觉推理中技能自进化的论文，核心主张是：视觉语言模型（VLM）的视觉推理能力可以通过在冻结模型参数的情况下迭代精炼一个外部技能库来持续提升，而不需要重新训练模型。
论文从视觉工具使用的现状出发：VLM 通过调用视觉操作来检查和操作图像，但效果受限于可用的视觉操作集合以及决定何时、如何使用这些操作的策略。已有的训练式方法（DeepEyes、Mini-o3）通过 SFT 或 RL 在预定义操作上学习策略，但操作或策略的局限会使模型难以获取任务相关证据。因此，把可复用的任务指导表示为外部技能（结构化指令+可执行工具）成为一种有吸引力的替代方案：技能存储在模型参数之外，可以在不重训练的情况下被精炼。
在此基础上，论文提出自进化框架 VisualEvolve。问题被形式化为：给定冻结骨干 Aθ、初始技能库 L0、演化数据 Devo 和 R 轮预算，构建固定验证集 Dval 和每轮执行集 Dexer，迭代更新技能库得到快照序列，最终选择验证精度最高的快照作为输出。框架每轮执行四个阶段：Skill-Library Snapshot Selection（从历史快照中选择最佳起点，以限制回退传播）、Execute（在执行集上收集执行证据）、Evolve（通过 Progressive Evidence Access 以从粗到细的方式访问技能演化历史和当前执行证据，诊断能力差距并修订技能库）、Evaluate（在固定验证集上评估并存档更新后的快照）。
实验设计覆盖四个研究问题：相对 Single-Pass 和初始技能库的增益及与训练式 baseline 的比较、框架阶段和证据访问机制的贡献、跨数据源与跨 backbone 演化、演化成本。演化数据默认混合 VisualProbe train（约 2.9K 样本）和 DeepEyes 训练语料中约 22K 的 Visual Search 样本，只保留图像、问题、答案。评测使用 V*Bench、HRBench4K/8K、VisualProbe Easy/Medium/Hard。
主要结果：在四个 backbon...**

<!-- paperflow:dcab629f811ccacb -->
## AEON: Unifying Video and 3D World Models

[[Deep Reading - Oct 2026/AEON-Unifying Video and 3D World Models|Deep Reading]]

[https://arxiv.org/pdf/2610.28631](https://arxiv.org/pdf/2610.28631)

- **论文围绕“什么是好的世界模型”这一根本问题展开。作者指出，显式世界模拟领域缺乏统一的表示，分裂为视频基和 3D 基两种范式：视频生成模型（如 Sora）能模拟真实外观和动态，但缺乏 3D 一致性；3D 生成模型（如 Marble）几何一致但往往静态、不真实。论文提出理想世界模型的五条标准：真实外观、动态场景演化、3D 一致环境、场景持久性和实时探索。没有任何单一范式能同时满足这些条件，因此需要一种统一表示。
AEON 的核心技术路线是：把 4D Gaussian Splatting（4DGS）作为基础世界表示，并让生成过程发生在预训练视频扩散模型的隐空间中。这样，模型既能使用视频扩散模型的大规模真实感先验，又能输出显式、持久、可实时渲染的 4D 几何基元。
技术主线分四部分。第一，4DGS 表示设计：每个高斯基元 gi = {xi, si, ri, oi, ci, ti, li, vi, ωi} 同时编码几何（位置、尺度、旋转）、外观（不透明度、颜色）和时间演化（时间、寿命、速度、时变可见性），通过速度驱动的运动和时变可见性建模动态场景。第二，4D 重建模型（4DRM）：从单目视频中联合预测深度、高斯外观、动态演化和相机姿态，四个任务头分别对应这些输出，为生成式 4D 解码奠定几何基础。第三，域适配器：通过蒸馏把生成式视频隐变量转换为重建式 4D 隐变量。蒸馏包含属性空间对齐（对每个基元的位置、旋转、不透明度、颜色、速度、寿命施加对应约束）、密集几何对齐和 Lfeat 特征对齐（用 L1 损失对齐学生中间特征与教师激活，权重 λ(t) 采用余弦衰减，早期强正则防止灾难性漂移，后期放松让适配器自由学习）。第四，多阶段端到端训练：把 4D 重建、域适配和生成解码联合优化。
实验主线覆盖三个任务。在 4D 生成任务上，AEON 在 30 个图像-文本场景中对比 4DNex 和 Lyra，使用 VBench 指标评估沿共享相机轨迹渲染的视频，AEON 全部指标领先，且 Wan 2.2 骨干优于 Wan 2.1；在视频到 4D 重建任务上，AEON 在 Dycheck 和 DaV...**

<!-- paperflow:5fe536fe52259792 -->
## Recast the Past: Stable Long Video Generation with Full Context Memory

[[Deep Reading - Oct 2026/Recast the Past-Stable Long Video Generation with Full Context Memory|Deep Reading]]

[https://arxiv.org/pdf/2610.27986](https://arxiv.org/pdf/2610.27986)

- **论文针对自回归视频模型的长时记忆问题提出 ReCam。背景是：世界模型、具身智能和创意生成等应用需要模型在数十秒到一分钟的生成过程中维持一致的状态，而完整保留历史帧在计算和内存上不可承受；已有方法通过选帧或稀疏注意力压缩历史，但可能丢失关键证据，导致场景重访与离屏主体重现不一致。ReCam 的出发点是让视频模型自己学习如何表示和检索历史：把累积的历史 latent 帧按局部时空块粗读，展平后经轻量 MLP 投影到 DiT 高维隐空间形成记忆 token，保留全历史覆盖的同时大幅减少 token 数；历史记忆与当前 chunk 通过原生自注意力连接，文本交叉注意力只用于当前 chunk；第一帧保持为干净视觉锚点。针对自回归 rollout 的误差传播，论文提出双指针去噪：recent context 与 target frames 使用不同噪声调度，对 recent context 保留较高噪声以限制复用帧错误的影响，去噪结束时恢复 clean context；消融显示 dual-pointer 在最终损失上接近 clean-context 且低于 single-pointer，VBench 首尾窗口变化上四项指标最优。此外通过 fused-PRoPE 在原始 RoPE 后注入相机轨迹，不增加可学习参数。训练从 Wan2.2-I2V-A14B 的 high-noise expert 初始化，在内部 200 万条 5–60 秒通用视频上三阶段训练（最大历史 10/20/30 秒，每阶段 10,000 步），静态场景重访额外用 SpatialVID-HQ 训练相机控制；评测使用 VBench-Long（30 秒 480p，六维度）、R2M-Bench（100 场景，MemoryGain/NMR）与离屏主体重现协议。结果显示 ReCam 在 VBench-Long 上达到 80.1 的最高分，离屏主体重现四项一致性指标全面领先，场景重访保持竞争力。局限是领域外记忆可靠性仍待解决；未来方向包括更严苛重访场景、更多下游应用和从上下文演示中学习。**

<!-- paperflow:ee78b8a7bd6be032 -->
## ALPS: Visual State Encoders for World Models

[[Deep Reading - Oct 2026/ALPS-Visual State Encoders for World Models|Deep Reading]]

[https://arxiv.org/pdf/2610.30120v1](https://arxiv.org/pdf/2610.30120v1)

- **【论证主线】论文从一个系统性主张出发：世界模型的视觉基础不应是「只懂语义」或「只懂重建」的单一编码器，而应同时保留物理状态、语言对齐和像素重建三种接口。作者指出，现有编码器在这三项能力上各有偏科，且评测协议分散，导致无法判断「一个表征能否通吃」。因此，ALPS 的目标不是提出又一个自监督损失，而是给出一套可复现的三能力预训练配方，并用统一评测回答「统一是否可行」。

【技术主线】ALPS 用 ViT-B/16 从零训练，输入静态图像与文本。Stage 1 同时优化语言目标（captioning 与对比对齐，配原生文本塔）、像素重建目标和空间对应目标（跨视角 patch matching 与坐标回归）。这三个目标分别对应「可指令」「可检查」「可测状态」。Stage 2 用跨视角自蒸馏精炼表征，提升视角不变性与空间对应，同时尽量保持语言对齐。下游冻结 patch token，接线性状态探针、LIBERO 行为克隆策略或世界模型规划头；预测的未来 token 可直接送入解码器变成像素。

【实验主线】论文在 15 个公开编码器上做统一比较：状态侧用 LIBERO 线性探针的聚合相对位置误差，并分解 readout、+1 s、+3 s 与 256×256 分辨率；语言侧用 token/global 对齐与 CKA；像素侧用 ImageNet 19k 训练、1k 留出的轻量解码适配器测 ℓ1/LPIPS，并用更强解码器复核。控制侧在 LIBERO 四个套件、每任务 50 条演示、3×50 rollout 下比较冻结 token 的行为克隆成功率。规划侧在 PushT、Wall、Granular 上比较不同编码器的世界模型 rollout，并设静态基线。消融在 10% Stage 1 预算下拆解三个目标与 Stage 2。

【关键发现】ALPS 的聚合状态误差为 .829，优于 C-RADIOv3（.864）、DINOv3（.870）和 PE-Spatial（.936），并在各子设置与分辨率上保持一致；它以 ViT-B 规模匹配 3.5 倍参数、视频预训练的 V-JEPA 2-L...**

<!-- paperflow:2c6385f94bacd3bd -->
## Learning from Next-Frame Prediction: Towards Unified and Scalable Visual Representation Learning

[[Deep Reading - Oct 2026/Learning from Next-Frame Prediction-Towards Unified and Scalable Visual Representation Learning|Deep Reading]]

[https://arxiv.org/pdf/2610.30724v1](https://arxiv.org/pdf/2610.30724v1)

- **这篇论文的主线可以概括为：视频自监督学习长期在“补全观察”与“预测未来”之间摇摆，VANE 试图把后者做成一个统一、可扩展的表示学习目标。① 动机与问题。作者认为，人类通过观看世界演化形成内部世界模型，视频的时间顺序天然提供无需标注的监督；但主流视频预训练并没有真正利用“接下来会发生什么”。Masked video models 遮挡时空 patch 并重建像素；V-JEPA 系列遮挡部分视频并预测被遮挡区域的表示；两者都是 contextual recovery，模型在补全周围上下文，而不是从过去推断未来。Autoregressive 方法虽然按时间顺序预测下一个 token，但逐 token 决策带来累积误差，且论文认为其表示质量仍不如 joint-embedding。由此，作者提出重访未来预测的“空间”与“粒度”：在哪个空间预测、以什么单位预测。② 方法。VANE 在冻结语义 tokenizer 的嵌入空间中做 next-frame prediction。给定视频 x1…xN，因果编码器只总结 x≤t，状态预测器推断整个下一帧的语义表示；目标由冻结的 DINOv2 ViT-L 提供，损失为 patch-level L1。冻结 tokenizer 的目的是防止目标空间与编码器共适应、把任务变简单。为弥补语义目标丢失的空间细节，模型增加轻量辅助生成解码器，以预测表示为条件重建未来帧 patch。图像没有时间顺序，作者构造 local-to-global 裁剪序列，让同一编码器从早期裁剪预测新揭示区域，把“下一帧”变成“下一裁剪”，从而统一图像与视频预训练。③ 训练。主模型为 1B ViT-g 编码器 + 冻结 DINOv2 ViT-L；数据为 VISTA 混合语料（ImageNet-21K、SA-1B、SSv2、K710、HowTo100M 加额外检索与类平衡数据）；四阶段：16 帧 256×256 warm-up/base、64 帧 long-video post-training、384×384 high-res cooldown；前两阶段约 26.6k GPU-...**

# Machine Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Machine Learning
- 方法：generation, reasoning, deep-learning, math-pr, math-st
- 论文/报告：3 篇
- UXBench Pro: Benchmarking Personalized User Experience in Multi-Turn Dialogue Interactions
- A Fisher-Geometric View of Multi-Teacher On-Policy Distillation
- Ask to Distill: Learning to Synthesize Data for On-Policy Distillation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:d22b8879c5f51c29 -->
## UXBench Pro: Benchmarking Personalized User Experience in Multi-Turn Dialogue Interactions

[[Deep Reading - Oct 2026/UXBench Pro-Benchmarking Personalized User Experience in Multi-Turn Dialogue Interactions|Deep Reading]]

[https://arxiv.org/pdf/2610.11638v1](https://arxiv.org/pdf/2610.11638v1)

- **UXBench Pro 是一篇关于个性化用户体验评估基准的论文，核心动机是填补现有能力导向基准与真实用户感受之间的鸿沟。论文从 UXBench 出发，指出二元偏好信号和用户无关奖励模型这两大局限，提出用更丰富的七维用户画像（FACTORS）和双视角评估框架来刻画用户体验。数据方面，作者构建了包含 1,000 条真实交互实例的数据集，覆盖 12 个任务场景和 82 个领域，每条实例都带有 FACTORS 用户画像。方法方面，双视角体系包括：用户奖励模型（URM）作为第三方视角，对响应满意度进行条件于用户画像的估计；Sim4Eval 用户模拟器作为第一人称视角，通过继续交互并从四个认知状态维度提供体验报告。论文还细致比较了 URM 的多种建模公式（pointwise GRM、BTRM、pairwise GRM）和多种偏好信号（regeneration、dual-answer、pointwise），并设计了 URMBench 和 USimBench 两个元基准来验证 URM 和用户模拟器对真实人类偏好与行为的忠实度。实验部分在多个维度给出了证据：用户特征的个体差异对评分的影响远大于任务类别差异；更强的模型并不天然更会利用用户画像，通用式缩短回答等“一刀切”适应策略反而可能降低偏好；检索增强能提升模拟器的语言保真度，但行为保真度仍受限于基础模型；Turn 和 Trajectory 两个层面的 BT 得分具有很强的区分度和与问题解决率的高相关性。总的来看，这项工作的贡献不仅是一个新基准，更是评估方法论上的转向：把用户当作有差异、有状态的个体，把用户体验评估从单点静态判断延伸为多轮、多视角、可验证的度量体系，并据此反思个性化模型优化的评估协议。**

<!-- paperflow:87b410b1d03db100 -->
## A Fisher-Geometric View of Multi-Teacher On-Policy Distillation

[[Deep Reading - Oct 2026/A Fisher-Geometric View of Multi-Teacher On-Policy Distillation|Deep Reading]]

[https://arxiv.org/pdf/2610.26525v1](https://arxiv.org/pdf/2610.26525v1)

- **论文从 Fisher 几何视角统一了模型合并与多教师 on-policy distillation 两条技术路线。
背景上，后训练正在大量产生同源领域专家：同一基础模型被不同 reasoning modality 的 RL 微调成 Math、Code、IF 等专家。Serving 时维护专家集合会放大显存和延迟，所以需要合并成单一通用模型。现有两条路线各有硬伤：权重空间合并（task vector 加权、TIES、DARE）零训练成本但权重靠手工，且无法诊断哪个领域未被恢复；多教师 on-policy distillation 用学生自己的生成去匹配各教师分布，能够修复合并遗漏，但每个领域都要 rollout，且共享权重梯度会跨域冲突。
论文的第一个贡献是理论等价性：在共享 basin 内，把 MOPD 目标做二阶展开后，它恰好等于 Fisher merging 目标；因此 MOPD 的 minimizer 在二阶意义下就是 Fisher merge——即专家参数的一个闭式加权组合。这个结果把“合并”从 heuristic 提升为“蒸馏的闭式最优阶段”，回答了两条路线不是二选一，而是同一目标在不同展开阶数下的表现。
由于全参数 Fisher merge 在 foundation model 上不可行，论文把合并限制在任务向量子空间。它进一步证明了一个 first-order homomorphism：混合任务向量所用的系数，同时也混合隐式奖励；于是可以用无标注 prompt 上的 residual program 来求解这些系数。Residual program 采用 Minimax 形式，最小化最差 per-domain residual，并输出 energy-normalized residual 作为 domain residual spectrum。论文还提出 Errlin 诊断，用线性 basin 下的一阶拟合误差判断近似是否破裂，并据此限制 Phase 2 写回幅度。
第二个贡献是 RGBM 两阶段流程。Phase 1 得到任务向量子空间内的 merge；P...**

<!-- paperflow:980f27dc3cc1bbaa -->
## Ask to Distill: Learning to Synthesize Data for On-Policy Distillation

[[Deep Reading - Oct 2026/Ask to Distill-Learning to Synthesize Data for On-Policy Distillation|Deep Reading]]

[https://arxiv.org/pdf/2610.27450](https://arxiv.org/pdf/2610.27450)

- **论文围绕 on-policy distillation 的数据效率问题提出“学会提问”的框架。核心动机是一个反直觉观察：OPD 用极少量训练问题即可成功，说明问题本身是分布知识转移的载体，问题质量而非数量决定蒸馏上限。论文由此提出两个问题：如何评估问题价值，以及如何学习生成更好问题。方法层面，论文定义支持感知评分 s，综合 teacher–student 分歧 δ 与学生熵/可及性 ω，并以 KL-filter（δ>τ）保证被选问题同时满足 teachable 与 reachable；同时用 GRPO 风格奖励训练 synthesizer q，使其学会生成能最大化有效监督的问题。理论部分给出 Proposition B.1 的 compatible-disagreement coverage 保证，说明评分与可教分歧之间的下界关系。实验在数学推理与代码任务上进行：验证分歧对学习价值的预测力（Spearman ρ=0.90）、评分分桶准确率差异、五分位学习收益以及六个基准的平均表现。结果显示优化后的合成问题能暴露更密集的监督信号，无需更长轨迹。训练协议细节包括学习率 1e-6、熵/KL 系数 0.01/0.005、rollout temperature 1.0、60 步训练、20,000 原始问题分 10 shard 生成。论文也明确 AMC23 与 LiveCodeBench 作为 checkpoint 选择基准而非 held-out 评估的边界。总体而言，该工作把蒸馏中的数据选择从启发式过滤升级为可学习的合成与评分闭环，为 OPD 的数据效率研究提供了一个系统性框架。**

# Language Models

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Language Models
- 方法：agent, language, reasoning, retrieval, optimization, gui-agent, stat-ml, stat-me
- 论文/报告：7 篇
- Generalized Proximal Policy Optimization: Unifying LLM RL Algorithms from the Perspective of Divergence
- Depth Memory
- Where and Why Search-Augmented LLMs Fail When Reasoning over Rapidly Evolving Knowledge
- Cross-Pollination Retrieval: Finding Transferable Ideas Across Fields in Scientific Literature
- MemT-1.0: Towards Memory-Native Evolution
- ProVe: Proposer-Verifier Decoding
- LongZero: Enhancing Long-Context Reasoning in LLMs from Zero Data
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:9e44ea29d01af850 -->
## Generalized Proximal Policy Optimization: Unifying LLM RL Algorithms from the Perspective of Divergence

[[Deep Reading - Oct 2026/Generalized Proximal Policy Optimization-Unifying LLM RL Algorithms from the Perspective of Dive|Deep Reading]]

[https://arxiv.org/pdf/2610.13015](https://arxiv.org/pdf/2610.13015)

- **总结一下论文的主要内容：

**问题背景**：RL 已成为提升 LLM 推理能力的基础范式，PPO 是主导算法。但 PPO 的裁剪代理在几何上是退化的：它隐式约束 TV 散度的单样本 MC 估计，TV 零曲率、对称边界，不能自适应策略偏移的幅度，导致 premature clipping、信任域缩放失真、探索坍塌。后续 DAPO、DPPO、RIPO 等通过启发式修补缓解问题，但碎片化、缺乏统一原则。

**核心方法**：论文提出 GPPO，从散度视角统一 LLM RL 算法。
- 理论上建立广义 f-divergence 信任域框架，带单调性能改进保证；不同 f-divergence 诱导不同几何和策略更新动力学。
- 提出新估计器拓扑（Atomic、Binary），比单样本 MC 更忠实。
- 在该框架下，PPO、GRPO、DAPO、DPPO、RIPO 都是特例：GRPO=(TV, MC)、DAPO=(KL, MC)、DPPO=(TV, Binary)、RIPO=(χ2, Atomic)。
- 形成 {Divergence × Estimator} 二维设计空间。

**实验主线**：
- 在八个数学与代码竞赛级基准上，实例化 4×3=12 种组合，对比 9 个代表性 RL 算法。
- 通过固定 contraction-side clip（rmin=0.8）标定 δ（TV=0.2、χ2=0.04、Hellinger≈0.01、KL≈0.22），隔离散度几何与信任域尺度。
- 发现两个维度都关键：非零曲率散度整体优于线性 TV；新估计器拓扑优于单样本 MC。
- 最佳 GPPO 实例数学比标准 GRPO 高最多 50%，代码高 14%，达到 SOTA。

**结论**：GPPO 为理解和设计未来的 LLM RL 算法提供系统化基础。

**证据说明**：检索证据主要覆盖 Introduction、Results、Conclusion 和部分方法公式（MC 估计第二矩），Method 主体与 appendix 细节（Atomic/Binary 估计器正式定义、完整理论证明...**

<!-- paperflow:23ee4833607889c0 -->
## Depth Memory

[[Deep Reading - Oct 2026/Depth Memory|Deep Reading]]

[https://arxiv.org/pdf/2610.18809v1](https://arxiv.org/pdf/2610.18809v1)

- **本文围绕一个相对被忽视的架构轴——深度方向的表示混合（depth mixing）——展开。开头指出 Transformer 架构创新集中在序列混合（attention/linear attention/SSM）与通道混合（gated MLP/MoE），而深度传播一直停留在 ResNet 式加法残差。近期 mHC 与 AttnRes 在规模化语言建模中证明更丰富的深度聚合有效，但缺乏统一理解。作者提出 Depth Memory (DM) 框架：把跨层传播看成流系统，其中可以存在标准残差流之外的显式记忆流；每层用 query 从记忆读取历史表示来丰富输入，并把当前层表示写回记忆。这个框架把 PreLN（无记忆）、AttnRes（记忆是增长中的显式层表示缓存，可被 attention 选择性读取）、mHC（记忆是扩展残差流，按超连接系数读写）统一为不同记忆实现。基于此视角，作者设计 Linear Depth Memory (LDM)：将深度方向的读取形式化为线性联想记忆上的 RLS 回归检索，沿深度维护三条流——残差流、矩阵值联想记忆流、逆协方差流。相比 AttnRes 的显式缓存，LDM 的记忆是压缩的线性映射；相比 mHC，LDM 的读取是 query 驱动的按需检索而非固定系数混合。作者还提出 state unwinding 与 windowed 等硬件感知实现降低峰值激活内存与流量。实验在 400M Llama-like/50B token 与 1.5B Qwen-like/100B token 上对比 PreLN、mHC、AttnRes(Full/Block) 与 LDM(D_k=8/D_k=4)。LDM 在验证困惑度上两个规模均最优（400M: 12.08 vs 最优 baseline 12.12；1.5B: 9.15 vs 最优 baseline 9.25），零样本平均准确率有竞争力（400M 50.46、1.5B 59.71，即在 1.5B 平均略高）。消融与附录的 aspect ratio 扫描支持设计选择（固定 α=1、D_k=8、浅层偏好）。总体论证逻辑...**

<!-- paperflow:72b53fe76ce10768 -->
## Where and Why Search-Augmented LLMs Fail When Reasoning over Rapidly Evolving Knowledge

[[Deep Reading - Oct 2026/Where and Why Search-Augmented LLMs Fail When Reasoning over Rapidly Evolving Knowledge|Deep Reading]]

[https://arxiv.org/pdf/2610.25323v1](https://arxiv.org/pdf/2610.25323v1)

- **论文围绕“快速演化知识上的检索增强 LLM 推理”这一任务展开，核心立场是：现有评测把失败当黑盒，只报端到端准确率，无法回答“在哪里失败、为什么失败”。作者提出 TREK1 套件，即 TREKTRACE（插件式失败归因框架）、TREKBENCH-WILD（真实世界元基准）和 TREKBENCH-CTRL（受控干扰项基准）。论证主线分三步：第一，论证快速演化知识耦合了时间推理与知识冲突，且是新闻、体育、金融、天气、危机信息等日常信息需求的中心（动机）；第二，用 TREKBENCH-WILD 在真实世界问题上测量前沿模型的端到端表现，并按时间参考类型（OO、NMD、SAA、SBA）划分，发现准确率从 64.6% 单调下降到 34.0%（技术路线与实验）；第三，用 TREKTRACE 对失败进行自动归因，发现主瓶颈是后检索推理而非检索——即使模型拿到了全部所需信息，仍会被冲突证据误导；随后用 TREKBENCH-CTRL 做受控验证，加入过时干扰项和多样化干扰项，发现模型对过时干扰项相对稳健，但对多样化干扰项仍然脆弱。方法层面，TREKTRACE 要求模型给出连接问题与 gold answer 的最短具体证据（一两句短句），用带搜索的 GPT-5.4 做证据提取与 URL 审计、不带搜索的 GPT-5.4 做答案评分，并设计包含 CORRECT / INCORRECT / NOT ATTEMPTED 的评分协议以避免模型道歉/自我纠正带来的噪音；TREKBENCH-CTRL 的干扰项需满足“匹配唯一源片段、真正改变文本、声明与参考不同答案、保留至少 0.90 OpenAI 相似度”等自动检查。实验方面，论文报告了四类时间条件下的准确率与统计检验（Holm 校正），对比了 FreshQA、InfoDS、SealQA 等来源子集的干扰项分布，并与 Magic Mushroom、NoisyBench、HoH、LongSeal 等基准做了覆盖度对比。结论是：检索增强系统需要把“相关性、时效性、可靠性”三者一起推理；评测需要从黑盒走向“在哪里、为什么失败”的诊断式范式。局限方面，论文承...**

<!-- paperflow:ea6413d6360f0b78 -->
## Cross-Pollination Retrieval: Finding Transferable Ideas Across Fields in Scientific Literature

[[Deep Reading - Oct 2026/Cross-Pollination Retrieval-Finding Transferable Ideas Across Fields in Scientific Literature|Deep Reading]]

[https://arxiv.org/pdf/2610.25678v1](https://arxiv.org/pdf/2610.25678v1)

- **本文提出并系统研究了“跨领域授粉检索”（cross-pollination retrieval）：给定一个研究问题，目标是从其他研究领域中找到思想可以迁移到该问题的论文。论证起点是：科学进步大量来自跨领域思想迁移，但现有文献检索体系并没有服务于这种需求。作者梳理了灵感检索的四种代表性定义——ScholarCatalyst（作者自述有帮助）、MUSES（下一篇论文引用）、MOOSE-Chem（领域专家判定必要）、ResearchBench（LLM 判定必要）——并指出它们都不要求灵感来自其他领域；而真正跨领域的尝试（SOLVENT 的跨领域类比、Same Problem, Different Field 的同问题跨领域求解）都停留在至多 109 篇论文的人工小集规模。同时，主流科学文献检索器（SPECTER、SciNCL、SPECTER2）以引用为监督信号，天然偏向主题相似性，与“跨领域可迁移”的目标相悖。因此，任务定义、数据规模与检索器能力三个层面都存在缺口。

技术上，论文的贡献集中在一个可扩展的数据构建管线和配套评测协议。CPR-Dataset 从引用中挖掘跨主题候选：以基论文引用且 OpenAlex 主题不同的论文为种子；用 leave-one-out pooling 为每个查询构建候选池（例如某查询的 151 个候选经去除 50 个同主题候选后剩 101 个）；对查询做改写以省略引用上下文、避免答案泄漏，并在同一 task 的多个相似查询中保留类比最强的 pair；最后由 LLM 按结构化 schema 判定——要求输出 structural_analogy、transfer_holds、transferable、transfer_path、object_mappings、shared_relations 等字段，且 transfer_type 必须属于 method、problem_formulation、evaluation_design、data_construction、finding_or_evidence 五类之一；此外还有多步分级判定（如 Step...**

<!-- paperflow:4a80add4eeaa4888 -->
## MemT-1.0: Towards Memory-Native Evolution

[[Deep Reading - Oct 2026/MemT-1.0-Towards Memory-Native Evolution|Deep Reading]]

[https://arxiv.org/pdf/2610.26825v1](https://arxiv.org/pdf/2610.26825v1)

- **论文的论证主线是：记忆应该被内化为基础模型的原生计算能力，而不是挂在外部系统上的附件。作者从自己先前的 Metis 工作出发——Metis 首次提出 memory foundation model 概念，用动态局部记忆加可训练 hyper memory 的结构把记忆装进模型——指出 token selection 式记忆更新不稳定，容量受限，因此需要架构升级。MemT-1.0 的技术主线包括四个要素：基于 Gated DeltaNet-2（GDN-2）的稳定 token 级记忆更新机制，使每轮内的记忆写入确定、可微、低噪声；Mixture-of-Storage（MoS）把原生记忆容量从单一状态扩展到多个存储槽，历史信息通过 commit 操作写入 slot；deliberate backbone preparation 让模型结构为记忆机制做好准备；两阶段训练管线（continued pre-training + post-training）为记忆存储提供显式监督（摘要、续写、压缩等），并保持通用能力。实验主线分三层：记忆能力（11 个基准按轨迹长度分四组，闭卷测试——只能基于动态记忆状态回答，不能访问文本上下文）、通用能力（6 个基准四维度）、智能体应用（Experience-Guided Agentic Framework 把成功轨迹总结成程序性经验存入 experience bank，模型在长程任务中复用）。发布两个模型：MemT-1.0-Nano（36B 参数、3B 激活、10M 动态记忆）和 MemT-1.0-Flash（288B 参数、17B 激活、336M 动态记忆）。核心结果：记忆能力平均优于参数记忆基线、与检索增强方法相当；通用能力在噪声设置下大体保持；128K 历史长度下 Nano 相对全量上下文 KV cache 有约 1.86× 端到端推理加速和 66× 持久存储缩减；内化程序性经验提升智能体任务成功率与执行一致性。论文收尾把未来指向长期记忆、容量扩展、更新成本下降、自主经验获取与跨任务记忆整合，最终目标是 agent 通过 memory-nat...**

<!-- paperflow:802e0902a3757122 -->
## ProVe: Proposer-Verifier Decoding

[[Deep Reading - Oct 2026/ProVe-Proposer-Verifier Decoding|Deep Reading]]

[https://arxiv.org/pdf/2610.27815](https://arxiv.org/pdf/2610.27815)

- **Speculative decoding accelerates autoregressive (AR) language models by letting a small model draft tokens serially and a large model verify them in parallel.

rule commits only 3.0. Lowering the confidence threshold commits more tokens, but also increases the rate of committing tokens that diverge from greedy AR decoding (Section 3.1).

ollable speed–accuracy tradeoff: a 7B verifier reaches 2.3× average throughput with mean accuracy within 0.3 points of AR, while a 3B verifier reaches 2.7× at 2.6 points below AR. Contributions • Building on the block-proposing capability of MARS, we reverse the usual speculative-decoding roles: the large model proposes 𝐵tokens in one pass, and a verifier…

We compare against five decoding baselines: standard autoregressive (AR) decoding, ProVelossless, the original MARS confidence rule, EAGLE-3 (Li et al., 2025a), and standard… nt is a control rather th...**

<!-- paperflow:8fc0cd08af17d7c0 -->
## LongZero: Enhancing Long-Context Reasoning in LLMs from Zero Data

[[Deep Reading - Oct 2026/LongZero-Enhancing Long-Context Reasoning in LLMs from Zero Data|Deep Reading]]

[https://arxiv.org/pdf/2610.30132v1](https://arxiv.org/pdf/2610.30132v1)

- **## 一、论文要解决的问题
LongZero 的出发点是一个很具体的资源约束：要做长上下文推理的强化学习，训练数据必须同时是长的、多样的、有挑战性的、可验证的。现实中这四条几乎不可能同时满足——人工标注长上下文多跳题代价极高；自然文本虽长但不携带可验证问题，数量也不足以支撑需要大量 rollout 的 RL；现有合成路线要么依赖 HotpotQA 一类外部语料（规模与难度上限被锁死、存在领域偏置），要么依赖更强教师模型（随着学生变强，教师越来越难找，形成能力天花板倒挂）。而近期兴起的「零数据 RL」（模型从自己提出的任务中学习）在短上下文任务上已经证明有效，但尚无工作把它推到长上下文尺度。论文的判断是：短上下文零数据 RL 的成功不能直接平移，因为长上下文引入了两个新难点——问题与证据必须联合生成且证据要真的散落在长文里；以及「太难」与「题目本身是坏的」在通过率上不可区分。**

# AI for Science

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science
- 方法：agent, ai-for-science
- 论文/报告：3 篇
- EvoBelief: Falsifiable World Models for Self-Improving LLM Agents
- OpenVLHarness: Real World Visual Agentic Tool-Use with Multimodal Memory
- ProteinGeoBench: Frontier Structural-Biology Agents Are Fluent at Lookup but Lost in Geometry
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:d63f94516b13a481 -->
## EvoBelief: Falsifiable World Models for Self-Improving LLM Agents

[[Deep Reading - Oct 2026/EvoBelief-Falsifiable World Models for Self-Improving LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.12789v1](https://arxiv.org/pdf/2610.12789v1)

- **一、论证主线
论文从“自进化 LLM 智能体为什么还会失败”这一问题切入。现有自进化机制（经验轨迹学习、反思、记忆、技能蒸馏）改进的是策略层 π(a|s,g)：智能体学会更好地行动；但行动还建立在“环境如何响应动作”的信念之上，而这类信念在部署前来自预训练先验，在陌生环境里可能是错的。论文用化合物 X 的合成例子说明：智能体具备加热、纯化、分析等程序能力，却因为“X 在 120°C 内稳定”这一错误先验而在 100°C 分解点失败；任务级修补（“用更低温度”）无法沉淀出“X 的热稳定性”这一可复用于运输、存储、分析的属性知识。更重要的是，错误信念会跨任务传播，使失败从单任务扩散到多任务。因此论文提出把“信念进化”作为与“行为进化”互补的自我进化轴，并把它形式化为部署期持续更新的世界模型。

二、技术主线
EvoBelief 将环境知识外部化为可证伪假设库。环境被形式化为隐状态-观测-动作-目标过程；智能体维护两个外部化状态：情境状态 zt（描述当前局面，episode 级重置）与世界知识 Wt（描述环境对动作的响应，跨 episode 持久）。世界知识的基本单元是假设 H := (C ∧ A ⇒ Y)——“条件 C 下执行 A 应产生 Y”，该形式使假设直接可测。假设生命周期由四个环节构成：
1) 形成：意外结果与决策相关未知触发新假设；
2) 测试：被动证据（普通任务转换恰好检验某假设）与主动证据（受控实验）双通道；测试若未见证条件或结果模糊则判为不确定，不算支持也不算反证；
3) 提交：仅当独立证据次数、绑定次数、测试次数同时超过预设阈值（τep、τbind、τtest）才成为已验证规则，进入 Wt 并可指导预测、规划与否决；规则先绑定具体实体，泛化到其他实体必须有它们自己的证据；
4) 修订/退役：反例缩小规则范围，环境漂移使规则陈旧时将其退役，从而“撤回权威”。
全程保持 LLM 参数冻结：预训练模型供给通用先验，Wt 沉淀环境特定知识；每条规则保留证据链与其改变的决策的记录，使学习可审计。工程实现上，ALFWorld/MiniGrid/ScienceWorld...**

<!-- paperflow:3a6f08452de7f0d7 -->
## OpenVLHarness: Real World Visual Agentic Tool-Use with Multimodal Memory

[[Deep Reading - Oct 2026/OpenVLHarness-Real World Visual Agentic Tool-Use with Multimodal Memory|Deep Reading]]

[https://arxiv.org/pdf/2610.26860v1](https://arxiv.org/pdf/2610.26860v1)

- **论文的论证主线：真实世界视觉任务需要的证据常常不在输入图像里，而在感知模型的输出、外部网页和中间计算中；推理算力投入再多也有边际收益递减（xhigh→max：token 2.7 倍、分数 +0.7），因此下一步进展应来自“让模型获取并组合多模态证据”，即视觉 agentic tool use。技术主线：为解决工具异构与跨轮次状态两大难题，OpenVLHarness 提出两层设计——能力层（Capability Layer）统一任务级工具接口并用证据渲染把掩码/检测/深度/网页变成模型可读形式；持久多模态记忆（Persistent Multimodal Memory）跨轮次保存工具生成图像、网页 URL 与数据绑定，并提供环境更新机制同步记忆状态；数据绑定 + Python Coding Agent 让模型对已存检测结果做集合运算与计数，失败时只需重述目标而非重新检测。实验主线：22 个基准、8 个后端、4 个任务域（grounded perception：CountQA/FSC-147/OdinW-13-All/FSCD-147/FSCD-LVIS；视觉搜索与深度研究：FVQA/MMSearch/MMSearch-Plus/HLE-VL/BrowseComp-VL/RealX-Bench 等 6+5 个；通用 VQA 与幻觉：RealWorldQA/SimpleVQA 等 6 个；另有空间推理与视觉数学），对比 Base、OT、VS、CX、OctoTools 等基线。主要结果：训练无关聚合平均上 Qwen3-VL-8B 34.2→45.4（+11.2，超 OctoTools 13.6 分）、Qwen3-VL-32B 38.3→50.9（+12.6）、GPT-5 44.4→55.7；视觉搜索域 max effort 领先最强基线 15.6/12.4 分；组件消融确认能力层与多模态记忆各自贡献；harness 搜索与工具生成提供不更新权重的领域特化途径。轨迹样例进一步展示了具体机制：SAM 3 药丸计数 134+5=139（无工具 137）、PascalVOC 密集人员 2...**

<!-- paperflow:c6f9fe56ea25417f -->
## ProteinGeoBench: Frontier Structural-Biology Agents Are Fluent at Lookup but Lost in Geometry

[[Deep Reading - Oct 2026/ProteinGeoBench-Frontier Structural-Biology Agents Are Fluent at Lookup but Lost in Geometry|Deep Reading]]

[https://arxiv.org/pdf/2610.26829v1](https://arxiv.org/pdf/2610.26829v1)

- **（1）问题定位：论文关注的不是再训练一个蛋白质结构模型，而是追问蛋白质结构智能体的基准分数到底测到了什么。一个结构生物学家可能先问某个结构如何被测定，再要求智能体从同一坐标文件计算距离或二面角；前者可从数据库记录检索，后者必须识别原子并执行计算。单一聚合分数可能把这两种能力混在一起。ProteinGeoBench 因此把任务拆成元数据查找、数值计算、概念推理、序列验证等族，并在 131 个任务上评估四个前沿模型，另用 26 个模型做扩展试验。

（2）基准与评估设计：131 个任务中 28 个是数值任务；论文使用 4,249 次四模型 rollout，并对 486 个模型-任务单元做八次重复以估计分数可靠性；另有 4,486 次跨 26 模型试验。评分包含 0.100 的完整性下限和 5.3% 权重的 LLM judge，因此并非完全确定性评分。评估不只报告总分，还报告任务族分数、配对任务 bootstrap 区间、单次运行下的排名稳定性，以及在 15 任务切片上改变网络访问、屏蔽八个结构数据库域、替换模型自带 CLI harness、改变坐标来源的对照。

（3）主要发现：四个前沿模型均分 0.707–0.816；Opus 5、GPT-5.6-sol、Grok 4.5 三个领先者在 0.031 内，彼此差异多未分辨；Gemini 3.1 Pro 为 0.707，与三者差距均排除零。聚合分数掩盖了任务族差异：三个模型在元数据查找上高于数值计算，Gemini 例外，数值计算反而是其最好族；Grok 4.5 数值最低（0.636）但概念推理第二。数值任务只占 28/131，却占四个模型全部失败的 12 个任务的一半，说明该族聚集了高失败率任务，但不能据此推断它更能区分模型，因为全员失败任务本身不提供模型间区分。

（4）评估条件的影响：单次运行下，23.9% 的 bootstrap 重采样会把另一个模型排到第一，尽管八次重复的 split-half 信度高达 0.962；这说明单个分数稳定不等于模型间差异可分辨。15 任务切片上，移除网络访问比屏蔽八个结构数据库域降分更多（...**

# AI for Education

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Education
- 方法：agent, generation, machine-learning, deep-learning
- 论文/报告：4 篇
- RSIgen: Reciprocal Self-Improvement for Agentic Visual Generation
- The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery
- IQuest-Q1: Advancing Foundation Capabilities for Agentic CLI Systems
- WeVisOne-VL: Unified Visual Perception from Objects to Pixels
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:56bf390a98a1e953 -->
## RSIgen: Reciprocal Self-Improvement for Agentic Visual Generation

[[Deep Reading - Oct 2026/RSIgen-Reciprocal Self-Improvement for Agentic Visual Generation|Deep Reading]]

[https://arxiv.org/pdf/2610.18688v2](https://arxiv.org/pdf/2610.18688v2)

- **本文针对 agentic visual generation 中统一多模态骨干的双角色（agent 和 generator）学习问题，提出 RSIgen 框架。作者首先指出任务成功不能决定学习接收者，因为最终图像是两者共同作用的结果；同时，生成器更新会使 agent 的监督过时。为此，他们设计了三个互惠机制：DRR 通过匹配干预选择学习接收者并构建训练目标；监督刷新在生成器学习后重新评估动作效用；递归课程构建由保留角色生成下一轮课程。实验在三个骨干上进行，显示一致的性能提升，并通过控制实验分离了分配、刷新和课程来源的贡献。盲评进一步验证了真实用户偏好。论文的贡献在于提出了一个系统性的互惠自我改进框架，并展示了其在多个模型上的有效性。**

<!-- paperflow:11a23df25512eda1 -->
## The Agentic Scientist: Orchestrating Coding Agents for Automated Scientific (Co-)Discovery

[[Deep Reading - Oct 2026/The Agentic Scientist-Orchestrating Coding Agents for Automated Scientific (Co-)Discovery|Deep Reading]]

[https://arxiv.org/pdf/2610.18933v1](https://arxiv.org/pdf/2610.18933v1)

- **We study a learned evolution strategy whose learned outputs are zero-initialized bounded multiplicative residuals on a separable CMA-ES core, so the strategy is exactly the…

rosses mark evaluations whose program modified the evaluation protocol (identified by manual audit); they are excluded from all running-best curves.

5), MLR-Bench targets open-ended machinelearning research (Chen et al., 2025), and EXP-Bench isolates whether agents can conduct valid AI experiments (Kon et al., 2026). (2024b) introduced an LLM-driven pipeline for paper reviewing, which was later validated on research papers published after the training data cutoff date (Lu et al., 2026).

ntrusted reference material. - SAE Bench is the evaluation and custom-SAE reference: https://github.com/adamkarvonen/SAEBench at commit `8042bb3828c6340da8d12062324e92b2077c571c`.

主要贡献包括：5), MLR-Bench targets open-ended machinelea...**

<!-- paperflow:708980eaad0f3ce9 -->
## IQuest-Q1: Advancing Foundation Capabilities for Agentic CLI Systems

[[Deep Reading - Oct 2026/IQuest-Q1-Advancing Foundation Capabilities for Agentic CLI Systems|Deep Reading]]

[https://arxiv.org/pdf/2610.30949v1](https://arxiv.org/pdf/2610.30949v1)

- **一、论文定位：IQuest-Q1 是一篇面向 agentic CLI 系统的开源基础模型技术报告。模型为稀疏 MoE，总参数 320B、激活 15B，目标是在真实命令行环境中完成软件工程、通用任务执行、长程推理与网络安全等任务。
二、问题与动机：Introduction 把 LLM-based agents 定义为通过迭代推理、行动与环境交互完成任务，而不是仅生成回答。CLI 被视为模型访问外部系统、工具、文件与代码执行的最通用接口之一，因此更强的 agentic 能力需要更强的 CLI 能力。论文希望提供一个开源基座，并在不同 agent harness 下保持可靠工具使用与稳健表现。
三、技术主线：训练分三阶段。1）预训练组合 Stable SplitNorm、Muon 与 μP，并使用两阶段课程，第二阶段提高 coding 数据比例；论文强调在提高缩放效率的同时保留安全学习率与最优学习率之间的较宽稳定裕度。2）中训练扩展数据领域与格式，逐步延长上下文。3）后训练采用多脚手架、多版本策略，结合 SFT、领域专用 RL 与 MOPD，整合不同 scaffold 的互补训练信号，强化 agentic coding。最后通过模型合并整合不同专家与不同阶段 checkpoint，并缓解参数更新冲突。
四、数据与任务体系：Figure 5 显示任务覆盖软件工程、终端与 CLI 工作流、前端/游戏/多媒体、代码理解与测试、仓库与程序生成、网络安全、技能型与专业任务。验证器构建依赖环境 runtime、build、tests、run judge、runtime snapshot checks、LLM tests，以及 container、repo sandbox、browser、3D、vulnerability service 等执行环境；验证方式包括 agent-written、differential、render、rubric、imported 等。
五、实验主线：训练期使用 held-out agent trajectories 上的 BPB 与原子能力评估；下游使用 D...**

<!-- paperflow:3d32176e5881cd92 -->
## WeVisOne-VL: Unified Visual Perception from Objects to Pixels

[[Deep Reading - Oct 2026/WeVisOne-VL-Unified Visual Perception from Objects to Pixels|Deep Reading]]

[https://arxiv.org/pdf/2610.31329v1](https://arxiv.org/pdf/2610.31329v1)

- **一、问题定位。论文从视觉感知的长期目标写起：系统不仅要识别图像中有什么，还要判断用户指的是哪个对象、对象在哪里。过去十年，检测与分割从 YOLO、DETR 等专用模型，发展到 Grounding-DINO、SAM 等开放世界模型，再到 Qwen3-VL 等通用 VLM，能力边界不断扩展。但作者认为，现有 VLM 在细粒度感知上仍不可靠：会幻觉不存在的对象，遇到不熟悉的物体描述时误解意图，或者漏掉非显著实例，导致 recall 不完整。论文因此提出 WeVisOne-VL，试图用单一自然语言接口统一对象级与像素级感知，并把“可靠”具体化为可解析输出、任意粒度定位和缺失目标拒识。

二、技术主线。WeVisOne-VL 的核心是把视觉任务写成结构化的 next-token 预测。对象级输出使用专用坐标 token 表示点、框和实例；像素级输出则通过一个特殊 <OBJ> token，把每个被定位的实例连接到轻量像素解码器，生成实例级 mask 或 alpha matte。这个设计的要点是分工：VLM 负责语义理解、任务解析和实例身份，像素解码器负责高分辨率、连续的像素预测。相比让语言模型直接生成 mask 或 alpha，这种桥接更符合两类模型的能力边界；相比为每个任务接独立头，它又保留了统一接口。论文覆盖检测、指代定位、计数、OCR、指代分割和抠图，输出粒度从点到框、mask、alpha matte。

三、训练主线。训练分为三阶段课程 SFT，随后是混合任务 RL。第一阶段是任务对齐，合理推断是让模型学会不同任务的输入输出格式和坐标 token；第二阶段聚焦高分辨率密集感知，对应 Dense200、VisDrone 这类小目标、密集实例场景；第三阶段是像素级优化，建立 <OBJ> token 与像素解码器的连接。Method 片段显示，像素级阶段会先初始化解码器，并出现 alternating 字样，暗示可能存在交替优化，但具体细节未完整给出。抠图还有专门的解码器微调：冻结 VLM，使用 130M 抠图样本训练 100K 步，专门学习连续 alpha 预测。数据方面，论文...**
