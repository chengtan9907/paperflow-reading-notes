# On-Policy Distillation & Post-Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：On-Policy Distillation & Post-Training
- 方法：language
- 论文/报告：1 篇
- Post-Training Language Models for Gold-Medal Performance in Coding Competitions
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:3e1f7a4c7f418c3c -->
## Post-Training Language Models for Gold-Medal Performance in Coding Competitions

[[Deep Reading - Sept 2026/Post-Training Language Models for Gold-Medal Performance in Coding Competitions|Deep Reading]]

[https://arxiv.org/pdf/2609.02849v1](https://arxiv.org/pdf/2609.02849v1)

- **论文的全景总结：
作者以 IOI/ICPC 作为 LLM 综合推理能力的极限测试场，目标不是“能通过排行榜上的公开测试”，而是在真实 IOI 规则限制下超越最顶尖人类选手。为此提出了一个端到端的后训练专业化流水线，其四大组件是：
1) 问题策展：从算法竞赛题库中筛选出 22,000 道高质量题目，以覆盖最困难的推理类型和算法主题。
2) 合成推理轨迹：为这些题生成完整 step-by-step 推理链条，使模型能从代码之外学到清晰的数学演绎和算法设计思路。
3) SFT：用这些轨迹进行监督微调。作者明确将其作为能力跃升的最关键步骤（证据：Conclusion 里“SFT provides the largest single-sample gains”）。
4) RL：在 Nano（30B-A3B）这一较小模型上开展针对代码生成/竞赛的强化学习，获得进一步但有上限的提升；在 Ultra（550B-A55B）上没有做代码专属 RL，以便清洗地展示 SFT 的规模效应。
此外还有一个特色组件 GenCorrect：一种迭代式测试时搜索。它吸取 AlphaCode 的“采样-过滤”思想，但比它多一个反馈闭环——模型先产生多种不同解法，执行后得到针对性错误信息，再带着这些信息修正、重写。这样在固定提交次数内显著提升成功率。
实验演进层次：
- 第一层（IOI 2025 回顾）：Nano-CC 130→291→468，说明后训练能翻倍提升，而 GenCorrect 又将结果再推过金牌线 438.3；Ultra-CC 304→502，证明模型规模越大越能享受 GenCorrect 的收益。
- 第二层（系统搭建）：作者告诉读者他们不是拿一个公开模型去现场，而是基于前面经验制造了一个“competition-specific”系统——可能包含更精细的题目分发策略、时限内做题选择、语言选择、提交节奏等（细节原文缺失，但 IOI 2025 到 2026 得分提升直接说明系统级改进有实际作用）。
- 第三层（IOI 2026 前瞻）：在真实赛场与人类选手相同的约束下取得 535.4 分。金...**

# Multimodal Models & Visual Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Multimodal Models & Visual Reasoning
- 方法：待从后续精读中沉淀
- 论文/报告：1 篇
- ShallowStream: Index Shallow then Answer Deep for Streaming Video Understanding
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:1128dc5c6c23a0ee -->
## ShallowStream: Index Shallow then Answer Deep for Streaming Video Understanding

[[Deep Reading - Sept 2026/ShallowStream-Index Shallow then Answer Deep for Streaming Video Understanding|Deep Reading]]

[https://arxiv.org/pdf/2609.02780v1](https://arxiv.org/pdf/2609.02780v1)

- **一、任务与动机。ShallowStream 面向流式视频理解。它把场景定位于视频流不断到达、MLLM 需要一直维持可用理解能力的真实系统，例如自动驾驶、监控、工业监测、可穿戴助手等。这类任务的难点在于视频是“因果增长的”，且用户查询可能在任意时刻到来，因此模型不能像离线长视频那样先完整看完全部视频再回答。
二、成本分析。作者指出现有基于 MLLM 的流式系统虽然已经尝试视觉 token 剪枝、token 合并、量化、按需取帧、上下文卸载等手段，却普遍忽略了一个根本开销：新帧到来时仍要在模型几乎全部深度上做 prefill。随着流不断变长，全深度 prefill 本身以及 KV cache 的存储（其大小随 prefill 深度线性放大）导致持续成本不可承受。简单地修剪 token 或量化数值，无法改变“每次都要穿透一个很深网络”的计算结构。
三、ShallowStream 的设计。论文把处理分为两个频率和代价都不同的阶段。(1) 流式到达阶段是 query-agnostic 的：每个视频单元只穿过模型浅层，得到浅层 KV cache；这些 KV cache 既作为该帧的编码结果，也直接被组织成一个持续更新的全历史轻量索引，从而避免为索引单独引入外部向量库或周期性全模型压缩。(2) 查询时间阶段：给定 query 后，先让浅层产生的注意力分数表示与查询交互，得到历史上下文单元的相关性；再用 diversity-aware 策略选出高相关且不冗余的证据；最后只把选中的证据段“解封”到完整深度模型做精确推理与回答。整个系统不需要 task-specific training，目标是即插即用替换现有 MLLM 流式推理前端。
四、实验与结论。论文在多个 MLLM 骨干上测试了 ShallowStream，并用总表形式报告了与现有最强流式方法的精度与效率对比。核心定量结论是：在保持足够好的推理精度的前提下，单帧 prefill 延迟最高可下降 52.1 倍，10 秒端到端延迟最高可下降 11.9 倍。作者还设计了一个隔离实验来独立观察浅层索引的检索能力：在 LVBench 上、以...**

# Data, Benchmarking & Evaluation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data, Benchmarking & Evaluation
- 方法：cross-modal, stat-ml, stat-me
- 论文/报告：1 篇
- Deeply Interleaved Text-Image Contexts for Multimodal LLMs Assessment
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:26dd9a6fc64939a8 -->
## Deeply Interleaved Text-Image Contexts for Multimodal LLMs Assessment

[[Deep Reading - Sept 2026/Deeply Interleaved Text-Image Contexts for Multimodal LLMs Assessment|Deep Reading]]

[https://arxiv.org/pdf/2609.02573v1](https://arxiv.org/pdf/2609.02573v1)

- **TIC-Bench 论文围绕一个此前被 MLLM 评测忽略的格式维度展开：深度交织的文本-图像上下文。论文首先指出现有评测习惯的两级错位：一是主流 multi-image 任务把多张图和一段指令放在一起，文本与图像缺乏持续的互指和更新；二是只在单图文对上进行能力判断的模型评估，无法覆盖文档理解、图文协同创作与多视角事件追踪等真实应用。这些应用要求模型在输入序列里反复执行跨模态的绑定、追溯、更新与传播，而不是「看完图再读题」式的独立感知。
为把这种能力显式化，论文提出 TIC-Bench：一个专门评估深度交织上下文理解的新基准。TIC-Bench 将任务划分为逻辑关联（Logical Association）、时间关联（Temporal Association）与空间关联（Spatial Association）三大域，并再细分为8个具有不同推理结构的任务类型，总计2280个问题。每个问题的组成特点是：多段文本与多张图片以交错方式排列，文本和图像共同贡献答案线索；模型需要从分散的证据中恢复 ground truth facts，而不是直接基于单张图作出判断。论文认为，这类设计能够测量模型在长交错序列中保持跨模态对应关系（cross-modal correspondences）和整合分布式证据的能力。
在评测协议上，论文选择了10个 state-of-the-art MLLM，并把人类专家表现纳入对照；部分实验还引入了 thinking mode 与默认模式的对比，以观察更深推理是否能补偿交错输入带来的额外负担。论文报告的总体结论是：当前最先进的 MLLM 与人类专家之间的性能差距依然显著。不同模型在不同任务上展现出互补优势，说明尚未出现统一的跨模态关联推理能力；thinking mode 只在部分任务中带来收益，不能从根本上解决长交错序列下的信息丢失与误关联。最突出的失败模式表现为：模型难以可靠地把分散在多个图像与多个文本片段中的证据连成一条完整推理链，容易遗漏远端线索或被不相关片段干扰。
论文由此提出未来模型需要具备四个能力：管理更长的交错多模态上下文；在上下文中保留与...**

# World Models, Generation & Audio

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：World Models, Generation & Audio
- 方法：agent, generation, reasoning, audio
- 论文/报告：3 篇
- VeriPhy: Agentic Physical Reasoning for World Model Evaluation and Refinement
- SolarWM: Open Data and Scalable Training for Long-Horizon Video World Models
- The Missing Temporal Link: Temporal Context Routing for Script-Driven Audio-Video Generation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:5941dec85c7b0fd1 -->
## VeriPhy: Agentic Physical Reasoning for World Model Evaluation and Refinement

[[Deep Reading - Sept 2026/VeriPhy-Agentic Physical Reasoning for World Model Evaluation and Refinement|Deep Reading]]

[https://arxiv.org/pdf/2609.03153v1](https://arxiv.org/pdf/2609.03153v1)

- **本文提出了 VeriPhy，这是一个旨在解决视频生成模型物理可靠性评估难题的创新系统。作者指出，当前的视频生成虽然在视觉上达到了“以假乱真”的程度，但在物理逻辑上经常漏洞百出，且现有的评估指标（如 FVD）无法提供具体的改进指导。VeriPhy 的核心思想是将物理验证从一个模糊的语义判断任务转化为一个结构化的、可编程的测量任务。

系统的工作流程分为三个阶段：首先，利用 LLM 作为规划器，将 Prompt 翻译为具体的“物理义务”和执行程序；其次，调用一系列冻结的视觉专家工具（如 SAM, Track Anything, 深度估计等）对视频进行多维度的物理测量；最后，通过确定的逻辑解析器将测量结果转化为“支持”、“矛盾”或“未知”的判定，并附带完整的证据链。为了验证系统的有效性，作者构建了一个包含 1,500 个视频和 2,582 条人工标注缺陷的大型基准数据集。实验结果表明，VeriPhy 在缺陷检测的召回率上显著优于现有的问题分解方法，并且其提供的可审计证据为生成模型的诊断和优化提供了宝贵的接口。这种从“黑盒评分”向“白盒审计”的范式转变，为构建具备真实世界理解能力的视频生成模型开辟了新的道路。**

<!-- paperflow:42bd6dcc4956a292 -->
## SolarWM: Open Data and Scalable Training for Long-Horizon Video World Models

[[Deep Reading - Sept 2026/SolarWM-Open Data and Scalable Training for Long-Horizon Video World Models|Deep Reading]]

[https://arxiv.org/pdf/2609.02886v1](https://arxiv.org/pdf/2609.02886v1)

- **SolarWM 论文提出了一套完整的开源交互式视频世界模型构建方案。其核心贡献在于打破了数据与模型之间的紧耦合：通过“数据引擎”实现了 10 个异构数据集的标准化，构建了包含 143 万剪辑的统一语料库；通过“适配框架”使得 Wan2.2、LTX-2.5 等不同架构的视频生成器能够共享同一套训练逻辑。论文详细阐述了一个高效的三阶段训练流程：首先通过双向训练打下坚实的特征基础，随后通过快速的自回归适配和分布匹配蒸馏，将预训练模型转化为具备长程生成能力的交互式世界模型。实验证明，该方法不仅效率高（仅需 5s 训练数据），而且泛化能力强，支持小时级的视频生成。通过开源数据、代码和 5B-33B 的模型权重，SolarWM 为社区提供了一个透明、可复现且易于扩展的研究基石，有望加速自动驾驶、机器人仿真及虚拟现实等领域的发展。**

<!-- paperflow:f2de89dedb79a794 -->
## The Missing Temporal Link: Temporal Context Routing for Script-Driven Audio-Video Generation

[[Deep Reading - Sept 2026/The Missing Temporal Link-Temporal Context Routing for Script-Driven Audio-Video Generation|Deep Reading]]

[https://arxiv.org/pdf/2609.02367v1](https://arxiv.org/pdf/2609.02367v1)

- **这篇论文针对剧本驱动的音视频生成中“时间失控”的核心痛点，提出了时间上下文路由（TCR）技术。作者指出，尽管现有的联合生成模型能产生高质量的音画，但它们无法精确遵循剧本中规定的镜头切换和对话时间，这是因为时间信息在文本编码中被模糊化了。TCR 通过在生成过程中引入显式的时间路由机制，将剧本中的时间区间直接映射到音视频的共享时间轴上，确保每个时间点的生成都受到正确剧本片段的引导。实验结果令人印象深刻：在镜头切换精度上实现了 96% 的误差缩减，并在对话对齐准确率上实现了数倍的提升。该研究不仅为专业内容创作提供了实用的工具，也为多模态生成中的精确时间控制提供了新的范式。通过 TCR，AI 导演能够像人类导演一样，精确地指挥每一帧画面和每一句台词的出现时机。**

# Web, GUI & Computer-Use Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Web, GUI & Computer-Use Agents
- 方法：agent, generation, optimization, gui-agent
- 论文/报告：2 篇
- Rendering-in-the-Loop: An Execution-Driven Agent for Interactive Web Development
- Efficient GUI Agents: A Systems Survey of Observation, Memory, Action, and Runtime Optimization
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:fb170b40c1143d88 -->
## Rendering-in-the-Loop: An Execution-Driven Agent for Interactive Web Development

[[Deep Reading - Sept 2026/Rendering-in-the-Loop-An Execution-Driven Agent for Interactive Web Development|Deep Reading]]

[https://arxiv.org/pdf/2609.02088v1](https://arxiv.org/pdf/2609.02088v1)

- **RILA（Rendering-in-the-Loop Agent）是一篇面向"交互式网页自动开发"的 arXiv 论文（2609.02088v1，作者 Yilong Guo、Hanqi Chen、Zixiao Ye、Guanzhong Wang、Chen Yu、Zeyu Chen）。论文的靶子是当前多模态网页生成的两难：一个可用的网页同时要求视觉保真和交互正确，但现有系统通常只在单条反馈通道内优化。

论文的论证主线开始于对现状的分类：纯一次生成式 MLLM 网页系统把渲染质量当作主要目标，依赖截图/设计稿与生成页面的视觉相似度；前端修复方法（如 Yuan et al. 2025 一类工作）对照设计规范审计静态渲染，规范覆盖不了真实用户操作；软件工程智能体则从编译错误、单元测试、日志修复代码，拥有执行世界的信息，却对页面视觉一无所知。三种路线彼此隔离的结果是：生成的页面要么视觉达标但动作一执行就坏，要么功能被修通但页面视觉被改坏。论文由此提出关键判断：交互失败通常要到真实浏览器中执行动作、看到 DOM 与画面变化时才会暴露，所以可靠网页开发不能做成 one-shot generation，也不能做成"只看渲染/只看日志"的 repair，而应把浏览器渲染放进优化环路，用执行反馈驱动增量编辑。

技术主线围绕一个执行—批评—编辑循环展开。RILA 从参考交互视频导出参考演示 R，R 包含参考截图和动作轨迹 A={a1,...,aN}，并同时作为执行目标与评估目标。每轮迭代中，RILA 在真实浏览器里运行当前生成版本，用 Action Interaction Verification（AIV）模块重放参考轨迹并逐步验证每次动作，得到接地气的执行感知观测；随后用 Execution-aware Rendering Score（ERS）把交互正确性与视觉保真度压成一个统一分数，既指导本轮修复方向，也保留历史最优实现，防止迭代回退。得到观测与分数后，Execution-aware Critique 把与参考演示的差异转成结构化修复建议，RILA 再借助软件工程（SWE）工具定位故...**

<!-- paperflow:cb9befba7b33d2c7 -->
## Efficient GUI Agents: A Systems Survey of Observation, Memory, Action, and Runtime Optimization

[[Deep Reading - Sept 2026/Efficient GUI Agents-A Systems Survey of Observation, Memory, Action, and Runtime Optimization|Deep Reading]]

[https://arxiv.org/pdf/2609.02309v1](https://arxiv.org/pdf/2609.02309v1)

- **本论文是一篇题为《Efficient GUI Agents: A Systems Survey of Observation, Memory, Action, and Runtime Optimization》的系统综述，目标人群是 GUI Agent 研究者和系统部署者。

论证主线：作者首先指出 GUI Agent 领域当前以“任务成功率”为核心汇报口径，但实际部署需要同时关注效率——即完成任务所消耗的上下文、计算、动作预算和运行时开销。这一论断将效率从“附加属性”提升为“核心评估维度”。论文提出应采用端到端系统视角，而不是孤立看待模型精度。

技术主线：论文将 GUI Agent 的工作机制概括为一个统一功能流水线（2.2 节证据）：观察模块负责从截图、DOM/HTML、可访问性树或混合解析中构建可操作的界面表示；决策与规划模块基于表示和记忆做出下一步动作决策（如点击、拖动、快捷键、应用切换）；动作之后可能还有验证/纠错模块；整个过程伴随上下文与记忆的管理。沿着流水线，效率问题被划分为四个技术轴：
- 观察效率：降低感知和界面表示的构建成本；
- 上下文与记忆效率：压缩与管理跨步骤的历史信息；
- 动作效率：减少完成任务所需的动作数、不可逆错误和无效探索；
- 规划器/系统效率：减少模型推理和系统性运行时开销（包括验证器、调度等）。

文献综合方法：对每个子主题，作者用“种子文献 + 定向搜索 + 前后向引文链”的方式扩展文献集，并记录每类方法的主导机制、报告的效率信号和引入的新开销。

主要归纳：交叉文献后，作者识别出五个反复出现的收敛性机制——(1) 选择性读取（selective reading）代替全上下文摄入；(2) 全局到局部视觉分配（global-to-local visual allocation），即在全局低分辨率浏览后局部高精度查看；(3) 可恢复记忆（recoverable memory）代替原始历史回放；(4) 验证感知控制（verification-aware control），在执行关键或不可逆动作之前增加验证；(5) 混合运行时（hy...**

# Agent Skills, Harness & Tooling

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Skills, Harness & Tooling
- 方法：agent, optimization
- 论文/报告：3 篇
- Repo-To-Skill: Distilling GitHub Repositories Into AI4AI Skills
- HarnessDev: Can LLMs Create and Evolve Their Own Agent Harness?
- MASkills: Continual Skills Optimization for Multi-Agent LLM Systems
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:c2bbbcd52ba82301 -->
## Repo-To-Skill: Distilling GitHub Repositories Into AI4AI Skills

[[Deep Reading - Sept 2026/Repo-To-Skill-Distilling GitHub Repositories Into AI4AI Skills|Deep Reading]]

[https://arxiv.org/pdf/2609.02749v1](https://arxiv.org/pdf/2609.02749v1)

- **本论文提出了 DisCo 框架，旨在通过引入“操作性知识层”来解决自主 AI 研究智能体在处理复杂机器学习任务时效能不足的问题。作者指出，现有的智能体虽然拥有强大的 LLM 骨干，但由于缺乏散落在 GitHub 和论文中的具体工程 Know-how（即操作性知识），在实际科研任务中表现受限。DisCo 通过将这些海量资源蒸馏为可验证、可重用的“技能”，并组织成技能图谱，为智能体提供了高密度的操作上下文。通过对 1,000 个 ML 仓库的蒸馏，作者构建了包含 5,000 多个技能的 AREX-Skill 库。实验结果显示，配备该技能层的智能体在 MLE-bench 等多个权威基准测试中表现出显著的性能提升，其中在机器学习工程任务上的得分提升超过一倍。该研究不仅提供了一个高效的技能蒸馏框架，还通过开源 AREX-Skill 库为 AI4AI 社区贡献了宝贵的知识资产，标志着 AI 研究智能体从单纯的“推理机”向具备“专业技能”的资深研究员迈进。**

<!-- paperflow:b0918362073b0354 -->
## HarnessDev: Can LLMs Create and Evolve Their Own Agent Harness?

[[Deep Reading - Sept 2026/HarnessDev-Can LLMs Create and Evolve Their Own Agent Harness|Deep Reading]]

[https://arxiv.org/pdf/2609.01437v1](https://arxiv.org/pdf/2609.01437v1)

- **HARNESSDEV 论文针对一个正在显性化的结构性缺口提出问题：智能体性能越来越依赖模型外部的执行基础设施（agent harness），而现有评测体系却只测“在给定 harness 下模型的表现”，从不测“模型能否构建并改进 harness 本身”。论文的出发点是两个观察：第一，保持模型权重不变、更换 harness 即可显著改变任务表现——说明基础设施是能力的独立贡献项；第二，前沿模型已经具备编辑多文件代码库、关闭真实 pull request 的能力，这种潜力已经存在，缺的是一个能测量它的基准。论文由此引入 HARNESSDEV，将评测单元从任务输出转移到可运行的基础设施。

技术主线上，HARNESSDEV 设计为两阶段协议。Creation 阶段让智能体从“弱小但可运行”的种子系统与少量开发案例出发，依据任务说明构建完整的执行系统，本质上是测试从少样本示例到通用基础设施的泛化能力；Evolution 阶段让智能体从它自己创建的 harness 出发，利用真实下游执行反馈做持续修订，目标是提高基准性能。这种“创建+进化”的双阶段设计把 harness 开发刻画成一个持续工程过程，而非一次性代码生成。评测协议也相应地分两个维度：capability 用 held-out 基准上的任务成功率衡量，efficiency 用执行 token 成本衡量；开发过程与评测任务严格隔离，评测任务被隐藏以保证泛化结论的可信度。

实验主线上，Creation 实验覆盖 6 个 creator LLM、4 个任务领域、5 个下游基准、2,207 个独特下游实例。结果呈明显的领域不对称：模型生成的 harness 在代码、搜索与研究任务上大幅落后于成熟的人工参考实现，但在写作上打平、在机器学习实验上反超参考。效率维度的执行成本表现则方差很大。Evolution 实验显示模型确实能利用下游执行反馈改进自己的 harness，但这种改进不稳定——性能随修订轮次起伏，且开发中获得的增益在 held-out 任务上会变小、一致性变差。最后，固定 runtime model 的实验把构建 h...**

<!-- paperflow:582a53dd477faac0 -->
## MASkills: Continual Skills Optimization for Multi-Agent LLM Systems

[[Deep Reading - Sept 2026/MASkills-Continual Skills Optimization for Multi-Agent LLM Systems|Deep Reading]]

[https://arxiv.org/pdf/2609.02094v1](https://arxiv.org/pdf/2609.02094v1)

- **MASkills 旨在解决一个在 LLM 智能体系统中日益重要但尚未被很好回答的问题：多智能体系统如何从自身交互经验中持续改进？论文指出现有自反思方法主要构建‘经验记忆’，然而记忆在调用、精修与规模扩展上存在结构性困难；相比之下，‘技能’作为结构化程序性知识（说明何时行动、如何行动、使用何种资源或工具）是更可操作的策略载体。由此，论文提出 MASkills——一种语言化的持续学习框架，其关键主张是把多智能体 LLM 系统的策略改进从参数空间转移到技能空间：在去中心化执行过程中，每个智能体在其潜在策略推理里调用可复用的技能工件，而系统则通过一个闭环的优化流程持续演化这些技能。技术路线上，MASkills 提出一个全新的智能体优化流水线，整合三个组件：技能条件信用分配，负责把团队级结果归因到具体智能体与具体技能；层级信用聚合，负责在不同层级上汇总信用信号以提高稳定性；动量平滑优化，负责抑制单次 rollout 噪声对技能库更新的扰动。技能库本身通过四种操作演化：精炼（改进已有技能）、归纳（从成功与失败轨迹中提取新技能）、整合（合并冗余或抽象通用技能）和剪枝（删除无效技能）。论文将该过程表述为语言空间中的策略梯度类比：去中心化 rollout 产生技能痕迹；语言批评者给出逐智能体、逐技能的反馈；技能库据此更新，再进入下一轮迭代。由于这种优化完全发生在语言与技能层面，不修改模型权重，因而适用于黑盒或仅通过 API 访问的 LLM，也天然支持跨模型主干使用。实验方面，论文将任务实例化为协作式 Dec-POMDP 环境，由专门化 LLM 智能体组成团队，在去中心化执行下共享团队级目标。评估在 HotpotQA、LoCoMo 和 GAIA 三个基准上进行，覆盖多跳问答、长对话记忆与通用智能体问题求解等类型。实验组织围绕三个研究问题：RQ1 考察持续技能优化能否提升任务性能并识别贡献最大的组件；RQ2 的具体表述未在检索证据中出现，但其存在表明论文还关注技能库演化过程或学习机制方面的追问（具体待原文确认）；RQ3 考察鲁棒性与泛化，包括在不同协调结构（centralized、decen...**

# Memory, Personalization & Long-Horizon Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Memory, Personalization & Long-Horizon Agents
- 方法：agent
- 论文/报告：1 篇
- CHIME: Credit-Aware Hierarchical Memory Evolution for Long-Horizon Agentic Planning
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:2d7ec0bee43999ea -->
## CHIME: Credit-Aware Hierarchical Memory Evolution for Long-Horizon Agentic Planning

[[Deep Reading - Sept 2026/CHIME-Credit-Aware Hierarchical Memory Evolution for Long-Horizon Agentic Planning|Deep Reading]]

[https://arxiv.org/pdf/2609.02074v1](https://arxiv.org/pdf/2609.02074v1)

- **本文针对长程智能体规划中的“信用分配”难题，提出了 CHIME 框架。该研究的核心逻辑在于：智能体的失败不应被简单地归结为整体轨迹的失效，而应区分是“想错了”（规划问题）还是“做错了”（执行问题）。通过构建层次化的规划库与执行库，并引入一个基于 LLM 的归因门控，CHIME 实现了对交互经验的精细化筛选与存储。实验证明，这种“先归因后记忆”的策略不仅显著提升了智能体在复杂任务中的成功率，还大幅提高了记忆的利用效率和跨模型的泛化能力。CHIME 为构建能够在推理阶段持续自我进化的智能系统提供了一种高效且低成本的路径，避免了昂贵的重新训练过程，同时解决了传统记忆方法中普遍存在的噪声干扰问题。**

# Language Models

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Language Models
- 方法：agent, generation, language, reasoning, optimization, multimodal-reasoning, audio, math-pr
- 论文/报告：19 篇
- Do Large Language Models Capture the Diversity in their Training Data?
- From Rollouts to Recipes: Self-Contained Post-Training for LLMs
- StudentSim: Training LLM-based Student Simulators
- LLMs Learn Better In-Context from Rules than from Examples
- SLATE: Are AI-Generated Slides Educationally Effective? A Benchmark for Language Teaching Quality and Learner Knowledge Acquisition
- From Citations to Contributions: LLM-Assisted Credit Scoring of Research Articles
- Qwen-Audio-3.0-ASR Technical Report
- Beyond Generation and Accuracy: Diagnosing and Enhancing Visual Chain-of-Thought for Geometry Problem Solving
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:c2223e7447202d83 -->
## Do Large Language Models Capture the Diversity in their Training Data?

[[Deep Reading - Sept 2026/Do Large Language Models Capture the Diversity in their Training Data|Deep Reading]]

[https://arxiv.org/pdf/2609.02275v1](https://arxiv.org/pdf/2609.02275v1)

- **论文关注一个长期被忽略却极为基础的问题：大规模条件生成模型是否“记得”训练数据中同一输入对应的多种合理输出？换句话说，除了预测准确性，模型是否保留了条件分布应有的离散度与多样性。作者将这一问题置于信息论框架中：用条件熵 H(Y|X) 描述给定输入 X 后输出 Y 尚未被解释的不确定性；对于 LLM，这对应同义改写、不同风格、不同事实表述等多种合法延续方式。

直接逐 prompt 估计条件熵需要每个 prompt 有多个独立参考输出，这在真实语料中几乎无法满足。论文的切入点是利用成对输入-输出样本，并引入基于 von Neumann 熵的矩阵化条件熵——输入输出联合特征通过 Gram 矩阵/协方差阵表达的谱熵能够反映分布的分散程度（相对于普通 Shannon 熵更能在样本缺乏重复结构时被稳定估计）。作者据此比较“模型生成分布的条件熵”与“训练数据经验分布的条件熵”。

实验对象包含三类可公开获取训练语料的 LLM——OLMo、Pythia、GPT-Neo——并考虑不同模型规模、序列长度和解码策略。结果高度一致：模型输出比其训练数据更“低熵”，即给定同一输入，模型倾向生成更狭窄的输出集合，训练数据中常见的多样性没有被完整继承。该差距并不随模型规模或解码策略调整而消失。论文把该现象称作 condition diversity gap，并将其与语言建模指标（likelihood/perplexity）和主流 benchmark（accuracy, alignment, semantic consistency）的考察盲区联系起来：一个在平均值上拟合良好、甚至更“自信”的模型可能在概率质量分配上过度集中。

为排除文本特有问题并测试普适性，作者在非语言条件生成任务也做了验证：类条件 ImageNet 生成器与 MS-COCO 文本到图像模型同样表现出低条件熵。这暗示该现象可能根植于对抗训练、扩散去噪、最大似然训练与解码/采样机制中的某些共性因素。

方法论上的第二项贡献是提出可操作的后处理纠偏。作者设计了一个熵约束投影：对每个输入采样一组候选输出，然后在候选输出上重新分配概率质...**

<!-- paperflow:aa34fb0943e7867a -->
## From Rollouts to Recipes: Self-Contained Post-Training for LLMs

[[Deep Reading - Sept 2026/From Rollouts to Recipes-Self-Contained Post-Training for LLMs|Deep Reading]]

[https://arxiv.org/pdf/2609.01422v1](https://arxiv.org/pdf/2609.01422v1)

- **本文提出了 Self-Routing，这是一种创新的 LLM 后训练框架，旨在打破传统方法中“统一优化目标”的局限。该框架的核心逻辑是“行为感知”：它不预设固定的训练配方，而是根据模型在训练过程中对每个样本的实时 Rollout 表现（正确性与置信度）来动态决定优化策略。具体而言，样本会被智能地分配到强化学习（GRPO）、监督学习（自蒸馏）或直接跳过。在 Qwen3 和 Qwen3.5 系列模型上的数学推理实验证明，Self-Routing 在最终准确率和训练稳定性上均优于现有的统一策略基线。研究深入分析了训练过程中的路由动力学，发现这种动态分配机制能够有效过滤噪声信号并减少对已掌握知识的重复训练，从而实现更高效的自我进化。该方法无需外部教师模型或额外标注，为构建自包含、自适应的 LLM 后训练系统提供了有力的技术路径。**

<!-- paperflow:37c0fb01b2231f6b -->
## StudentSim: Training LLM-based Student Simulators

[[Deep Reading - Sept 2026/StudentSim-Training LLM-based Student Simulators|Deep Reading]]

[https://arxiv.org/pdf/2609.01591v1](https://arxiv.org/pdf/2609.01591v1)

- **论文 STUDENTSIM 研究的是“如何构建可用的 LLM 学生模拟器”这一问题，其深层动机来自自适应 AI 导师：导师需要知道“哪种指导对这个学生有效”，但这个个体化指导效果信号稀疏且收集成本高，业界需要一个可以代替真人学生、随时提供反馈信号的学生模拟器。论文明确指出，一个合格的学生模拟器必须同时兑现两种能力：行为保真度（F），即模拟器与学生本人的独立作答高度一致；指导响应度（R），即模拟器在接受导师解释/纠错/提示后，其作答会发生合理更新，真正“学得进”。

围绕这个双目标，论文首先厘清了既有工作的结构性缺陷：状态跟踪类模型（如 Maia2）擅长把学生当做一个可预测的行为过程来拟合，因此 F 尚可，但不具备把导师的外部指导内化为认知变化的能力，R 很低；LLM 角色扮演类模型（如 GPT-5.4）能流畅地参与指导对话并及时调整回答，但由于没有在真实学生记录上校准个体能力，其“像不像这个学生”的 F 不可靠。换句话说，模拟器研究被分裂成了“行为克隆”和“语言角色扮演”两派，各自只成功了一半。

本文的方法贡献是 STUDENTSIM——一个两阶段训练框架。第一阶段是 pooled training：把所有学生的去身份化记录合并，让模型学到该领域学生作答与教学响应的共性先验，解决单学生样本稀疏的问题；第二阶段是 per-student specialization：用每个学生的少量私有记录对共享模型做个体专门化，把共性先验收缩到这个学生的具体能力轮廓上。两阶段设计使模型两条腿走路：群体统计规律支撑其泛化，个体记录把它锚定到真实学生。摘要没有给出损失函数和模型架构的具体细节，读者需要从原文确认两阶段如何避免灾难性遗忘、如何用指导后数据监督 R、以及专门化阶段需要多少数据。

与训练框架配套的是评测贡献 STUDENTSIMEVAL。该基准由 60 名学生的真实学习记录构成，横跨国际象棋、第二语言英语写作与数学三个异构领域；数据来自可公开研究使用、已去身份化的学习者数据集。协议的严谨性体现在三点：其一，定义 F 与 R 两个正交指标，避免“只会像学生”或“只会被教”的模型...**

<!-- paperflow:3f1a0432c53979a5 -->
## LLMs Learn Better In-Context from Rules than from Examples

[[Deep Reading - Sept 2026/LLMs Learn Better In-Context from Rules than from Examples|Deep Reading]]

[https://arxiv.org/pdf/2609.03213v1](https://arxiv.org/pdf/2609.03213v1)

- **本论文对大语言模型在上下文学习（ICL）中的“规则遵循”与“示例归纳”两种模式进行了深度评测。通过跨越游戏、算术和语言推理的五大类实验，研究发现 LLMs 在处理显式规则描述时表现出比处理输入-输出示例更高的可靠性和效率。核心结论指出，规则不仅在性能上优于示例，而且在指令微调后这种优势被进一步放大。研究挑战了“更多示例总是更好”的直觉，发现增加示例或在规则中补充示例往往无法产生显著增益。此外，论文通过统计分析揭示了任务属性对学习效果的调节作用：代数抽象任务更依赖规则，而分布敏感型任务则对示例更具包容性。这一发现对于优化提示策略、理解模型认知机制以及改进未来模型的指令微调过程具有重要的理论和实践意义。**

<!-- paperflow:acd9e1a9292d04c9 -->
## SLATE: Are AI-Generated Slides Educationally Effective? A Benchmark for Language Teaching Quality and Learner Knowledge Acquisition

[[Deep Reading - Sept 2026/SLATE-Are AI-Generated Slides Educationally Effective-A Benchmark for Language Teaching Quality|Deep Reading]]

[https://arxiv.org/pdf/2609.06212v1](https://arxiv.org/pdf/2609.06212v1)

- **本文针对 AI 生成教学课件中“华而不实”的问题，提出了 SLATE 基准。该基准通过引入语言学奥林匹克谜题，构建了一个无知识泄露的评估环境，并创新性地利用 VLM 作为学习者代理来量化教学效果。研究的核心发现是：课件的教学设计质量远比内容准确性更能决定学习者的知识获取水平。实验揭示了当前主流模型在生成课件时普遍存在“远迁移能力弱”和“可能产生负面教学效果”的缺陷。SLATE 不仅为教育 AI 提供了一个严谨的评估工具，更指明了从“内容生成”向“教学干预”转变的研究方向，对于开发真正具备教育价值的智能教学系统具有重要指导意义。**

<!-- paperflow:a29416a343d39136 -->
## From Citations to Contributions: LLM-Assisted Credit Scoring of Research Articles

[[Deep Reading - Sept 2026/From Citations to Contributions-LLM-Assisted Credit Scoring of Research Articles|Deep Reading]]

[https://arxiv.org/pdf/2609.07673v1](https://arxiv.org/pdf/2609.07673v1)

- **这篇论文针对科学评价体系中“引用信号同质化”的问题，提出了一种创新的基于贡献度的信用评分框架。其核心思想是将科学论文视为原创性与继承性的结合体，并通过一种名为“贡献树”的层级结构，将论文的总信用在内部章节、段落及外部引用之间进行定量分配。为了克服人工评估的规模化难题，作者引入了大语言模型（LLM）作为局部重要性的估计工具，利用其语义理解能力来判断不同文本片段对论文核心贡献的相对价值。研究不仅停留在单篇论文的分析上，还进一步构建了加权引用图，通过信用传播算法在整个语料库范围内重新定义了学术影响力。实验结果证实，该方法能够有效区分“核心引用”与“边缘引用”，捕捉到传统计数指标无法反映的深层贡献信号。尽管存在 LLM 长度偏见和缺乏绝对真值等局限性，但该工作为科学计量学从“数量驱动”向“价值驱动”的转型提供了一条极具前景的技术路径，对于优化科研资源分配和改进学术评价机制具有重要的理论与实践意义。**

<!-- paperflow:ca3dab80cf1a232a -->
## Qwen-Audio-3.0-ASR Technical Report

[[Deep Reading - Sept 2026/Qwen-Audio-3.0-ASR Technical Report|Deep Reading]]

[https://arxiv.org/pdf/2609.07549v2](https://arxiv.org/pdf/2609.07549v2)

- **本技术报告详细介绍了 Qwen-Audio-3.0-ASR 系统的设计与实现。该系统旨在解决传统 ASR 在真实工业生产环境中的局限性，通过引入大语言模型 (LLM) 的推理能力和混合专家 (MoE) 架构，构建了一个功能全面、性能卓越的语音识别平台。

在技术路线上，模型基于 Qwen 系列底座，利用数千万小时的语音数据进行训练。其核心创新在于统一的指令遵循框架，这使得模型不仅能完成基础的转写任务，还能根据用户需求进行方言识别、热词定制和文本润色。特别是针对中国复杂的方言环境，模型覆盖了 16 种主要方言，填补了大规模预训练模型在这一领域的空白。

实验结果表明，Qwen-Audio-3.0-ASR 在多个权威基准测试中达到了 SOTA 水平，并在与 GPT-4o 等顶级商业模型的对比中展现出明显优势，尤其是在中文语境和工业定制化场景下。此外，报告还介绍了一个低延迟的流式变体，确保了技术在实时交互场景下的落地可行性。

总的来说，Qwen-Audio-3.0-ASR 不仅是一个强大的学术模型，更是一个高度成熟的工业级解决方案，为语音识别技术从“听见”到“听懂”并“精准表达”的跨越提供了重要参考。**

<!-- paperflow:bacd2916c3fcd494 -->
## Beyond Generation and Accuracy: Diagnosing and Enhancing Visual Chain-of-Thought for Geometry Problem Solving

[[Deep Reading - Sept 2026/Beyond Generation and Accuracy-Diagnosing and Enhancing Visual Chain-of-Thought for Geometry Pro|Deep Reading]]

[https://arxiv.org/pdf/2609.12606](https://arxiv.org/pdf/2609.12606)

- **本文针对多模态大模型在解决复杂几何问题时的局限性，提出了一套完整的诊断与增强方案。研究的核心在于揭示并缩小“自主性鸿沟”，即模型在自主生成视觉辅助线并利用其进行推理时的能力缺陷。

首先，作者构建了 **GeoVAD-Bench**，这是首个针对视觉思维链（VCoT）轨迹进行细粒度诊断的基准。它不只关注最终答案，而是通过感知、辅助质量、利用率、演绎推理和正确性五个维度，配合 No-Aux/Auto-Aux/GT-Aux 三种干预模式，精准定位模型在哪个环节“掉链子”。诊断结果显示，模型普遍存在感知不准、画线不忠实以及视觉信息利用率低的问题。

针对这些问题，作者开发了 **GeoWeave-8B** 模型。该模型的成功归功于两点：一是**高质量的数据管线**，通过合成和增强手段，产生了大量包含几何感知、图形编辑指令和交织推理过程的训练数据；二是**渐进式训练框架**，特别是引入了多模态强化学习（RL），将诊断维度的表现作为奖励，迫使模型在推理过程中真正“看图说话”。

实验结果令人振奋：GeoWeave-8B 在几何准确率上实现了 25.3% 的大幅提升，并且在过程诊断指标上全面超越了同类模型。更重要的是，该研究证明了通过细粒度的过程干预和针对性训练，可以显著增强大模型在高度专业化领域的逻辑推理能力。这为未来开发更智能的 AI 教育助手或科学发现工具提供了重要的理论依据和技术路径。**

<!-- paperflow:0435b0b0cea65b0c -->
## SteerDuplex: Steerable Duplex Speech Dialogue Models

[[Deep Reading - Sept 2026/SteerDuplex-Steerable Duplex Speech Dialogue Models|Deep Reading]]

[https://arxiv.org/pdf/2609.12623](https://arxiv.org/pdf/2609.12623)

- **本文针对全双工语音对话模型在可控性方面的缺失，系统性地提出了解决方案。作者首先定义了语音可控性的分类体系，并构建了 STEERBENCH 基准用于定量评估。通过在 Moshi 模型基础上进行大规模 SFT 和创新的两阶段混合奖励 RL，STEERDUPLEX 模型在语气、人格、语速等维度的控制力上取得了突破性进展，显著优于现有开源模型。实验不仅证明了方法的有效性，还深入探讨了全双工交互中特有的“时机-内容”权衡问题，指出了未来优化方向。该研究为构建更自然、更具表现力的人机语音交互系统奠定了重要基础。**

<!-- paperflow:f93fa7ffb775907a -->
## Lost in Perception: Isolating Perceptual and Reasoning Failures in Multimodal Physics and Geometry Reasoning

[[Deep Reading - Sept 2026/Lost in Perception-Isolating Perceptual and Reasoning Failures in Multimodal Physics and Geometr|Deep Reading]]

[https://arxiv.org/pdf/2609.18991](https://arxiv.org/pdf/2609.18991)

- **【论证主线】
1) 起点观察：多模态 LLM 在科学推理基准上不断刷新分数，但这些分数是“感知 + 推理”的合成量。当模型答错时，基准无法回答最有指导性的问题——是没看清图，还是看懂了但推不动。
2) 方法论主张：要回答这个问题，不需要新模型或新数据集，而需要受控的输入通道对照。作者选择的最小可操作干预是在不改变题目的前提下改变信息的呈现载体：纯文本、原始图像、人工撰写 caption、修正后的 caption 等（具名的“五个任务”其确切构成未在摘要中展开，需回原文核对）。
3) 归因判据：若某题纯文本可解而给图后解错，则失败可归因于感知；若换成人工/修正 caption 后恢复正确，则进一步确认该失败属于“感知阻塞型”；若在感知近似 oracle 下依然错，才判为真正的推理瓶颈。用“恢复率”这一派生指标把定性归因变成可比较的量。
4) 领域对比：同一套诊断流程分别施加于物理与几何，得到的关键结论是——感知失败之后接续的推理错误类型并不相同：物理侧收敛为计算错误，几何侧收敛为概念误用。这条发现意味着感知错误不是均匀噪声，而是沿领域特异的推理路径被继承为系统性的偏差。
5) 讨论与旁支：在核心实验之外，作者把 InternS1-mini 作为案例提出：尽管规模化的科学预训练加 thinking 能力，它在每个任务上都低于实验中最弱模型，且推理轨迹常在被生成完成前截断。该观察把讨论从“模型能力不足”推向“推理预算/终止条件/解码配置”这类系统层因素，但作者自己把它标记为核心实验之外的讨论，证据强度弱于前述结论。

【技术主线】
全文不含新的模型架构、训练方法或数据集构建，技术贡献集中在实验协议与分析方法上：
- 输入通道阶梯（text → image → human caption → corrected caption）构成对感知变量的梯度控制；
- 跨通道的一致性比较用于判定“可恢复”与“不可恢复”的失败；
- 失败类型 taxonomy（计算错误 / 概念误用）在感知失败之后被二次施加，用于刻画错误的领域形态；
- 双领域并行设计使领域成为可比较因子，而非混入总分。...**

<!-- paperflow:70dbb86119dc7b2c -->
## Look Less, Hear Better: Jointly Rewarded GRPO for Streaming ASR

[[Deep Reading - Sept 2026/Look Less, Hear Better-Jointly Rewarded GRPO for Streaming ASR|Deep Reading]]

[https://arxiv.org/pdf/2609.18333](https://arxiv.org/pdf/2609.18333)

- **本文针对流式 ASR 中准确率与延迟的权衡问题，指出传统的 DSM 范式由于依赖固定的对齐监督，限制了模型提前输出的能力。作者提出了一种创新的后训练框架，核心贡献包括：1) 定义了 AWED 指标，精准刻画了单词发射相对于声学结束的延迟；2) 引入 GRPO 强化学习算法，通过联合奖励函数对模型进行优化，使其在保证转录准确性的同时，主动降低输出延迟。实验结果令人振奋：在不改变任何模型架构的情况下，该方法在 Voxtral Realtime 基础上实现了 Pareto 前沿的显著提升，尤其在低延迟场景下 WER 降幅超过 30%。这一工作证明了强化学习在优化流式感知任务中的巨大潜力，为未来实时语音交互系统的开发提供了新的范式。**

<!-- paperflow:48bc141408057d9a -->
## SEA-LION-v4.8: A Technical Report

[[Deep Reading - Sept 2026/SEA-LION-v4.8-A Technical Report|Deep Reading]]

[https://arxiv.org/pdf/2609.18310](https://arxiv.org/pdf/2609.18310)

- **SEA-LION-v4.8 技术报告标志着东南亚区域性 AI 发展的又一里程碑。该研究的核心逻辑在于：通过深度定制化（数据、分词、对齐）来弥补全球通用模型在特定地理文化区域的“数字鸿沟”。

在论证主线上，报告首先通过详尽的数据统计揭示了现有模型对东南亚支持的匮乏，随后提出了一套完整的技术解决方案。技术主线聚焦于“高质量区域语料库构建”与“高效分词架构”，通过持续预训练使模型具备了深厚的本地语言底蕴。实验主线则通过引入 SEA-Bench 等区域性基准，科学地量化了模型在文化理解、翻译及常识推理上的优势。

总的来看，SEA-LION-v4.8 不仅是一个技术产品，更是对“AI 主权”的实践，展示了如何通过针对性的工程优化，在资源受限的语种上实现媲美甚至超越顶级通用模型的表现。对于关注多语言对齐、低资源学习及区域化 AI 落地的研究者具有极高的参考价值。**

<!-- paperflow:3438651a33ae1966 -->
## GVPO++: Group Variance Policy Optimization for LLM Post-Training and On-Policy Distillation

[[Deep Reading - Sept 2026/GVPO++-Group Variance Policy Optimization for LLM Post-Training and On-Policy Distillation|Deep Reading]]

[https://arxiv.org/pdf/2609.21432](https://arxiv.org/pdf/2609.21432)

- **本文介绍了 GVPO（Group Variance Policy Optimization），这是一种旨在解决大语言模型（LLM）后训练中训练不稳定问题的创新算法。现有的主流方法如 GRPO 虽然简化了强化学习架构，但由于高度依赖重要性采样，在面对复杂任务时容易出现方差过大和训练崩溃的问题。GVPO 通过引入 KL 约束奖励最大化问题的解析解，将策略优化问题转化为一个基于均方误差（MSE）的梯度权重分配问题。这种设计不仅在理论上保证了唯一的最优解，而且在实践中彻底摆脱了重要性采样的束缚，使得训练过程异常稳健。此外，GVPO 展现了极强的通用性，能够自然地扩展到在线策略蒸馏（OPD）场景，并支持多种复杂的优化目标。通过在推理任务和通用对齐任务上的广泛验证，GVPO 证明了其作为下一代 LLM 后训练标准范式的潜力，为构建更强大、更可靠的智能模型提供了坚实的算法基础。**

<!-- paperflow:1e6eafeed81d3675 -->
## DRT: Dense Reasoning Trace for Efficient and Grounded Multimodal Reasoning

[[Deep Reading - Sept 2026/DRT-Dense Reasoning Trace for Efficient and Grounded Multimodal Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2609.21675](https://arxiv.org/pdf/2609.21675)

- **本文针对多模态大语言模型（MLLM）在复杂推理任务中面临的效率与准确性瓶颈，提出了一种名为“密集推理轨迹”（DRT）的新型推理范式。传统的思维链（CoT）依赖于冗长的自然语言，不仅消耗大量计算资源（Token 冗余），还容易引入噪声导致逻辑稀释和视觉幻觉。DRT 通过引入结构化的符号连接器和视觉-逻辑解耦机制，将推理过程压缩为紧凑且逻辑严密的轨迹。

技术实现上，作者首先通过“密集轨迹初始化”使模型具备生成结构化内容的基础能力。随后，引入了创新的“轨迹对齐强化学习”框架，利用 GRPO 算法和精心设计的结构化奖励函数，对模型的推理行为进行微调。这种方法不仅关注答案的正确性，更强调推理过程的简洁性和视觉证据的忠实度。三视角验证流水线的引入确保了强化学习过程中参考轨迹的高质量。

实验结果令人振奋：在 Qwen3-VL 这一强力基准上，DRT 在多个主流多模态推理榜单上均实现了性能突破，准确率提升 1.3%，同时将 Token 消耗降低至原来的 18% 左右（5.5 倍效率提升）。这一发现挑战了“推理必须依赖详尽自然语言描述”的传统认知，证明了结构化、符号化的表达在多模态智能体中具有更高的信息密度和逻辑强度。DRT 为构建更快速、更准确、更廉价的下一代多模态推理模型提供了重要的理论支持和实践路径。**

<!-- paperflow:8112eb026b024066 -->
## One Prompt Does Not Fit All: Self-Meta-Evolve for Personalized Information Extraction

[[Deep Reading - Sept 2026/One Prompt Does Not Fit All-Self-Meta-Evolve for Personalized Information Extraction|Deep Reading]]

[https://arxiv.org/pdf/2609.21626](https://arxiv.org/pdf/2609.21626)

- **【一句话定位】这篇论文主张企业信息抽取的正确抽象不是“为任务优化一条提示”，而是“在交互反馈下为每个用户维护并进化一条提示”，并给出一个双层循环框架、一套 persona 驱动基准与一组含真人双盲对比的实证。

【问题主线】LLM 正在被广泛用于企业级信息抽取，而企业场景的独特性在于：同一份文档对不同的使用者必须被组织成不同的信息结构——角色不同，值得抽的字段、粒度、次序、措辞都不同。现有提示优化方法却遵循“单一提示 + 全局目标”的范式，其隐含假设是用户需求同质。论文指出这造成系统性错配：全局最优提示对个体用户并非最优；静态提示无法跟上真实偏好的漂移；企业环境里可得的是稀疏、隐式、且强烈依赖用户身份的交互反馈，而现有方法并非为此设计。若反过来为每个用户单独搜索提示，成本随用户数线性增长且单用户样本极少——个性化与可扩展性构成核心张力。据此论文把任务形式化为“交互反馈下的逐用户提示适配（per-user prompt adaptation）”。

【技术主线】Self-Meta-Evolve 用层次结构化解上述张力。每用户持有自己的结构化提示（可分解、可局部编辑，而非整段重写）。内循环面向单用户，依据 persona 条件化反馈编辑其结构化提示——反馈不是裸信号，而要结合用户角色画像来解释，因为同一句反馈在不同 persona 下应当触发不同的修改。外循环跨用户汇聚编辑轨迹，蒸馏出“哪些编辑有效”的成功模式，用来进化 meta-prompt 本身，即指导内循环如何编辑的元提示。这样，个体经验被沉淀为可迁移的跨用户编辑知识，新用户与冷启动用户不必从零开始；两层互相供给信号，使系统在推理时也持续自我改进，而不是一次性的离线优化。

【实验主线】为让训练与评测可规模化、可复现，论文发布了一个 persona 驱动的 IE 基准，包含 292 个模拟企业用户，并配套一条基于 O*NET 职业分类体系的可复现 persona 生成流水线——把“用户从哪来”也纳入方法论范围。在该基准上，框架达到 74.58% 成功率，比最强提示优化基线高 13.56 绝对点，并且仅两轮迭代就达到...**

<!-- paperflow:fe394af6acd66095 -->
## What Should a Self-Teacher See? Privileged Context Design for On-Policy Self-Distillation

[[Deep Reading - Sept 2026/What Should a Self-Teacher See-Privileged Context Design for On-Policy Self-Distillation|Deep Reading]]

[https://arxiv.org/pdf/2609.25623v1](https://arxiv.org/pdf/2609.25623v1)

- **本文针对在线自我蒸馏（OPSD）中的一个核心假设——“教师模型获得的特权信息越多，教学效果越好”——进行了系统性的实验挑战。作者提出，在竞赛数学等复杂推理任务中，教师模型如果直接接触完整的参考答案，可能会产生与学生模型当前能力脱节的反馈信号。通过设计涵盖“命名策略”、“方法框架”、“问题类别”等不同抽象维度的特权上下文，并在 4B 和 8B 规模的模型上进行验证，研究发现：适度的信息抽象不仅能显著提升学生的下游任务表现（最高提升 1.6%），还能大幅削减 Token 开销。此外，研究揭示了仅提供答案作为特权信息的强大竞争力，并指出传统的 KL 散度指标在衡量蒸馏质量时的局限性。最终，论文强调了“因材施教”的重要性：教师模型看到的特权信息应当与学生模型能够消化的抽象层级相匹配，这一发现为未来高效、低成本的 LLM 自我演进训练提供了新的设计准则。**

<!-- paperflow:72cc3c93654089f3 -->
## Hunyuan-A13B Technical Report

[[Deep Reading - Sept 2026/Hunyuan-A13B Technical Report|Deep Reading]]

[https://arxiv.org/pdf/2609.27284](https://arxiv.org/pdf/2609.27284)

- **【证据状态说明】本次未获取到该论文的 PDF 正文、摘要或任何语义检索片段（retrieved_evidence 与 sections 均为空），元数据中的 arXiv 编号 2609.27284 与发布日期 2026-09-24 也与该模型已知的公开节奏不一致，存在元数据异常的可能。因此以下总结为“基于标题、作者署名（Tencent Hunyuan Team 全员署名）、以及混元系列与同类 MoE/推理模型公开技术路线的结构化重建”，其中架构与训练流程部分属于合理推断，任何具体数值都必须回原文核对。

【论证主线】报告要论证的核心命题是：在“可单机/少卡部署”的推理成本约束下，稀疏 MoE 是当前最有效的容量扩展路径；具体地，一个总参数约 80B、每 token 激活约 13B 的模型，可以在大量基准上逼近激活参数或总参数更大的对手，同时把部署门槛压到单机可承受的区间。围绕这一命题，报告需要处理三个反命题：(a) 稀疏模型是否只是“参数多但学得浅”；(b) 单模型统一快慢思考是否会互相侵蚀；(c) 推理 RL 是否会牺牲通用能力。这三条构成报告的论证张力。

【技术主线】架构层：细粒度 MoE（共享专家 + 路由专家）承载容量，GQA 压缩 KV cache 以支撑长上下文，RoPE 配合外推策略把上下文拉到 256K 量级。训练层：多阶段预训练，数据质量过滤与课程式配比调度，后段进行长上下文扩展；优化器与精度方案（如 Muon 类优化器、FP8 训练）若沿用混元前代做法，则会在系统效率章节体现。后训练层：SFT 阶段混配直答样本与长链 CoT 样本，用提示/控制 token 区分快慢模式；RL 阶段用可验证奖励驱动数学、代码与 agent 任务，并混入通用数据抑制回退；agent 专项通过工具调用轨迹与多轮环境进行强化。部署层：给出多精度权重、量化方案与推理框架适配，强调可用性。

【实验主线】预期按五条线组织：通用知识与推理（MMLU/GPQA 类）、数学与代码（AIME、LiveCodeBench 类）、长上下文（RULER、LongBench 类，覆盖 32K...**

<!-- paperflow:fddafe630e8a8df9 -->
## SciWalker: Synthesizing Scientific Coding Problems with Operator Graphs and Execution Feedback

[[Deep Reading - Sept 2026/SciWalker-Synthesizing Scientific Coding Problems with Operator Graphs and Execution Feedback|Deep Reading]]

[https://arxiv.org/pdf/2609.30054v1](https://arxiv.org/pdf/2609.30054v1)

- **本文提出了 SciWalker，一个旨在解决科学编程数据匮乏问题的自动化合成框架。该框架的核心思想是将复杂的科学计算任务分解为可组合的“算子”，通过在预定义的算子图中进行随机采样，生成具有逻辑约束的计算工作流线索。基于这些线索，LLM 能够生成高度多样化且符合科学直觉的编程题目。为了解决合成数据中常见的不可运行问题，SciWalker 引入了基于执行反馈的迭代修复机制，确保了最终产出数据的高质量和可验证性。研究团队利用该框架产出了 8,178 个覆盖 5 大科学领域的任务，并证明了这些数据在强化学习阶段能显著增强 Qwen3.5-9B 模型的科学代码生成能力（SciCode 准确率提升近 10%）。这项工作不仅为 Code LLM 的领域特化提供了系统性的方法论，也为 AI 辅助科学发现奠定了坚实的数据基础。其开源的框架和数据集将有力推动科学计算社区与大模型社区的融合。**

<!-- paperflow:3c7a930d84037cfb -->
## Recursive Self-Improvement via On-Policy Distillation for Reasoning

[[Deep Reading - Sept 2026/Recursive Self-Improvement via On-Policy Distillation for Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2609.30652](https://arxiv.org/pdf/2609.30652)

- **本论文针对大语言模型在复杂推理任务中的自改进问题，提出了一种创新的递归自蒸馏框架。传统的策略内自蒸馏（OPSD）通过冻结一个拥有 Ground Truth 特权的教师模型来指导学生，虽然保证了训练稳定，但限制了教师模型吸收新知识的能力，且容易导致模型输出变得冗长和过度自我批判。为了打破这一僵局，本文提出了动态共演化（DCE）与自我精炼简洁学习（SRCL）相结合的方法。DCE 允许特权教师模型在多轮递归训练中与学生同步演化，从而提供更高质量的 Token 级监督；SRCL 则通过让模型在自身策略内响应的简短、已验证的重写版本上进行训练，有效压缩了推理长度。在 Qwen3-8B 等模型及四个竞赛级数学基准（如 MATH、AIME）上的实验表明，DCE+SRCL 取得了突破性的进展，不仅在 Average@12 指标上超越 OPSD 达 35.62 个百分点，还成功将输出长度降低了 7.80%，实现了推理准确率与计算效率的双重提升。**

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, science-discovery, vision
- 论文/报告：54 篇
- InSight: A Benchmark for Agentic Claim Verification in Interactive Visualizations
- PaperCompiler: Faithful Paper-to-Code Generation via Repository-Level Specification Compilation
- Harness Engineering in LLM Tool Use via Agent-Native Reusable Tool Primitives
- SkillGLoW: Procedural-Family Skill Consolidation for Self-Improving Agents on Long-Horizon Task Streams
- Git4Data: Database-Native Version Control for AI Agents
- AgentIdeaBench: Benchmarking Scientific Ideation in the Agent Era
- Agentic Visual Generation: From Generative Models to Agentic Control
- APPSim-Bench: Bridging Real-world Apps and Reproducible Evaluation for Mobile GUI Agents
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:522f3f0092be3b3b -->
## InSight: A Benchmark for Agentic Claim Verification in Interactive Visualizations

[[Deep Reading - Sept 2026/InSight-A Benchmark for Agentic Claim Verification in Interactive Visualizations|Deep Reading]]

[https://arxiv.org/pdf/2609.01383v1](https://arxiv.org/pdf/2609.01383v1)

- **本文针对当前视觉语言模型评测多局限于静态图像和一次性问答、无法覆盖真实数据分析中动态、交互和部分可观察证据环境的问题，提出并发布了 InSight——一个用于代理式声明验证（agentic claim verification）的基准。其任务设定是：模型作为智能体，面对一条自然语言声明和一个完全交互式的 Web 可视化环境，必须通过主动操作（点击、悬停、滚动、过滤、缩放等）去检索与综合证据，最后判断声明是被支持（True）、被反驳（False）还是证据不足（Not Enough Information, NEI）。

数据集由 297 份人工撰写的分析笔记本（human-authored analytical notebooks）派生而来，共形成 21,349 条声明；这些声明被锚定在真实的交互式 Web 环境中，并带有三类标签：True 占 41.6%、False 占 45.0%、NEI 占 13.4%。与常用问答基准不同，该环境中的证据分布被有意设计为部分可观察：大量精确数值藏在 hover 等交互动作之后，即使声明指向的是可见图表，静态渲染也同样难以作答。此外，这些可视化由分析师自己构建，有别于模板化图表，从而增加了视觉外观和交互结构上的真实性与复杂度。

论文将交互轨迹本身视为推理过程的代理证据（interaction traces as intrinsic proxies for reasoning），有别于传统只考核最终答案的评估协议，从而支持对模型如何寻找证据、如何综合不同视图信息进行过程性审计。这个设计把从文本事实核查中继承的 NEI 监督与交互式可视化环境连接起来：前者引入了明确的认知不确定性类别，后者提供了需要主动展开的视觉验证空间。在方法上，它同时涉及声明验证的文本蕴含任务形态、证据获取的过程型任务以及视觉语言模型作为代理的界面操作能力。

在实验侧，作者对若干种最先进的视觉语言模型进行了评估，主要结论是交互式验证仍是一个非常困难的问题，模型在此任务上距离可靠解答仍有很大距离。论文公开了数据集与代码仓库，以促进后续对可视化交互代理的评测研究。由于可...**

<!-- paperflow:e782bb1611fc5b98 -->
## PaperCompiler: Faithful Paper-to-Code Generation via Repository-Level Specification Compilation

[[Deep Reading - Sept 2026/PaperCompiler-Faithful Paper-to-Code Generation via Repository-Level Specification Compilation|Deep Reading]]

[https://arxiv.org/pdf/2609.02272v1](https://arxiv.org/pdf/2609.02272v1)

- **这篇论文介绍 PaperCompiler，目标是提升“论文到仓库级代码”自动生成中的保真度。问题被定义为受控信息转换：输入论文 P，输出仓库 C={c1,...,cn}。作者指出，当前 paper-to-code 智能体（如 PaperCoder、AutoP2C、AutoReproduce 等）普遍采用“解析论文 → 计划/摘要 → 逐文件编码”的流程。中间产物是自由文本，编码阶段很可能会忽略、重解释或压缩其中的关键约束，导致细节损失、算法降级和文件间不一致。

PaperCompiler 的关键是引入“规范编译层”：先把论文证据编译为显式、可溯源的仓库级实现规范。编译结果保留来源，并将每条信息标记为论文支持的、推断的、外部委托的或未解决的。规范中包含四个核心内容：非降级要求（强制实现不能弱于论文方法）、所有权分配（哪个文件负责实现哪个部分）、跨文件依赖（文件的引用与调用关系）和文件级约束。然后，代码生成步骤在这些规范的指导下进行，但同时保持局部工程选择的自由度。本质上，它把以前“传递给 LLM 的自由计划”换成了可追溯、结构化、约束更强的“编译产物”，试图在信息流中做一个防丢失的变换。

在实验层面，论文在 Paper2CodeBench 上评估。摘要给出的结果是 PaperCompiler 相比强基线将 reference-based fidelity 从 3.64 提升至 4.15（约 13.8% 相对提升），并把高严重性 evaluator critiques 的比例从 13.2% 降至 6.1%。作者进一步指出增益在与作者实现比较的场景中最明显，说明模型不只是生成“形似”的文件，而是更好地复现论文特有的方法逻辑。这些结果也体现在 evaluator 错误分析中。

论文的方法论贡献可以总结为两点：其一，它把仓库级代码生成从一种“凭计划自由发挥”的模式变成一种“由规范约束的生成”；其二，它提供了一个处理模糊性的机制——不是把所有内容都当作事实传给编码器，而是显式标注哪些内容可以推断、哪些需要外部委托或仍未解决，这样能把生成期的不确定性显式化。

从科学复现的角度...**

<!-- paperflow:66027cbb02860099 -->
## Harness Engineering in LLM Tool Use via Agent-Native Reusable Tool Primitives

[[Deep Reading - Sept 2026/Harness Engineering in LLM Tool Use via Agent-Native Reusable Tool Primitives|Deep Reading]]

[https://arxiv.org/pdf/2609.01736v1](https://arxiv.org/pdf/2609.01736v1)

- **本文针对大语言模型在工具调用中面临的 API 模式僵化和大规模工具管理难的问题，提出了 HEART 框架。该框架的核心创新在于“工具原语”（Tool Primitives）概念，它通过 LLM 封装将 API 调用转化为自然语言交互，极大地提升了多步推理的灵活性。配合拥有超过 2.5 万个功能的 ToolFace 仓库，HEART 实现了工具的按需检索，有效解决了上下文过载问题。在架构上，HEART 采用了由规划器、路由器和验证器组成的 Agent 原生设计，通过闭环的反馈机制确保了执行的可靠性。实验结果显示，HEART 不仅在标准基准测试上超越了 GPT-4o 等顶尖模型，更在真实复杂任务中展现了数倍于现有方案的成功率，同时大幅削减了推理成本。此外，其在防御 Prompt 注入方面的表现也证明了该框架在安全性上的巨大潜力，为构建工业级、高可靠的智能体系统提供了新的范式。**

<!-- paperflow:f55be34289ac8e0d -->
## SkillGLoW: Procedural-Family Skill Consolidation for Self-Improving Agents on Long-Horizon Task Streams

[[Deep Reading - Sept 2026/SkillGLoW-Procedural-Family Skill Consolidation for Self-Improving Agents on Long-Horizon Task S|Deep Reading]]

[https://arxiv.org/pdf/2609.02217v1](https://arxiv.org/pdf/2609.02217v1)

- **LLM agents increasingly self-improve by writing and reusing textual skills, kept either as one global document or as a flat pool of per-task entries, though most of the evidence…

LLM agents increasingly self-improve by writing and reusing textual skills, kept either as one global document or as a flat pool of per-task entries, though most of the evidence… On long-horizon workloads where each task demands a different solution, the two forms fail in opposite ways: the document collapses into generic discipline, while the pool…

Building on this observation, we propose SkillGLoW (Global–Local Weave; failures point to a missing unit of reuse, one that sits between the single document and the single entry.

As base models and agent harnesses mature, LLM agents are moving from short-horizon, closed tasks toward longer-horizon, more complex environments (Yao et al. Shinn et al.

主要贡献包括：Buildin...**

<!-- paperflow:aa75db29dec4b724 -->
## Git4Data: Database-Native Version Control for AI Agents

[[Deep Reading - Sept 2026/Git4Data-Database-Native Version Control for AI Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.02106v1](https://arxiv.org/pdf/2609.02106v1)

- **这篇论文介绍了 Git4Data，一个旨在解决 AI 智能体在处理关系数据时面临的状态管理难题的系统。核心动机在于 LLM 智能体需要频繁地在数据的不同假设状态之间切换，而现有的数据库缺乏高效的分支和合并机制。Git4Data 通过在云原生数据库 MatrixOne 内部实现版本控制逻辑，将 Git 的核心哲学（分支、快照、对比、合并）引入了 SQL 环境。技术上，它巧妙地利用了 MatrixOne 的不可变对象存储和 MVCC 机制，确保了分支操作的轻量化，使得操作成本仅与数据变化量相关。实验结果表明，在专门针对智能体设计的 BranchBench 基准测试中，Git4Data 在性能上显著超越了现有的数据版本化方案（如 DoltDB）。该研究不仅为智能体提供了一个可靠的“实验沙盒”，也为未来数据库如何原生支持 AI 工作流指明了方向。**

<!-- paperflow:e90dea289a89aed1 -->
## AgentIdeaBench: Benchmarking Scientific Ideation in the Agent Era

[[Deep Reading - Sept 2026/AgentIdeaBench-Benchmarking Scientific Ideation in the Agent Era|Deep Reading]]

[https://arxiv.org/pdf/2609.07611v1](https://arxiv.org/pdf/2609.07611v1)

- **本文针对自主 AI 科学家核心的“科学构思”能力，提出了 AgentIdeaBench 基准测试。该基准打破了传统静态评估的局限，引入了主动探索模式，更真实地模拟了科研智能体“检索-推理”的工作流。通过对 33 个模型的深入测试，研究揭示了主动探索能显著放大前沿模型的能力优势，且这种优势主要体现在假设的严谨性和具体性上，而非单纯的原创性提升。论文还提出了科学世界建模（SWM）技术，证明了结构化推理对中等能力模型的强化作用。AgentIdeaBench 为智能体时代的科研 AI 评估提供了重要的度量基础，强调了在评估高阶认知任务时，必须考虑智能体与环境交互的能力，为未来开发更强大的自主 AI 科学家指明了方向。**

<!-- paperflow:436dfff70e58464e -->
## Agentic Visual Generation: From Generative Models to Agentic Control

[[Deep Reading - Sept 2026/Agentic Visual Generation-From Generative Models to Agentic Control|Deep Reading]]

[https://arxiv.org/pdf/2609.06758v1](https://arxiv.org/pdf/2609.06758v1)

- **本论文针对当前火热但概念混乱的“智能体视觉生成（Agentic Visual Generation）”领域，提出了一套严谨的因果分级框架。作者指出，现有的研究往往将工具调用、多角色协作等系统复杂度指标与智能体特性混淆，缺乏核心判定标准。为此，论文以“控制器在生成轨迹中能直接控制的最深节点”为因果依据，确立了从 L0（固定支持，无控制器）到 L1（条件控制，仅准备输入）、L2（执行控制，选择工具操作）、L3（结果自适应控制，根据中间输出修正当前任务）以及 L4（经验自适应控制，复用经验改变未来任务决策）的五个递进级别。

在确立了包含控制器、生成器、工具和评估器的基本组件模型后，论文将该框架作为分类与分析工具，对图像、视频、3D、世界模型、UI 生成等多个前沿领域的现有系统进行了系统性的映射与梳理。通过详尽的文献统计与演进趋势分析，论文揭示了当前视觉生成智能体正大量向 L3（轨迹内反馈）演进，但在 L4（跨任务经验持久化）上存在巨大的技术空白。该工作为智能体视觉生成确立了清晰的边界与设计空间，对未来系统设计、评测基准构建以及迈向高阶经验自适应的通用视觉智能体具有重要的指导意义。**

<!-- paperflow:aedf5a18149b5512 -->
## APPSim-Bench: Bridging Real-world Apps and Reproducible Evaluation for Mobile GUI Agents

[[Deep Reading - Sept 2026/APPSim-Bench-Bridging Real-world Apps and Reproducible Evaluation for Mobile GUI Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.07712v1](https://arxiv.org/pdf/2609.07712v1)

- **本文是一篇以「评测环境」为核心贡献的基准类论文，主线是：移动 GUI Agent 评测长期在「真实」和「可复现」之间被迫二选一，作者主张用可控模拟 App 把两者同时拿下，并以此基准对当前 agent 能力做出一个「远未解决」的判断。

【问题主线】
论文开篇即指出，移动 GUI Agent 能按自然语言指令执行任务，但评测要同时真实且可复现很难。已有基准分两类：简化 App 缺少真实移动端的复杂度；真实商业 App 则引入推荐、广告、账号与内容变化等不受控变异。作者承认既有工作（Zhang et al., 2024a）已指出可复现性问题，但认为通用且高效的解决方案仍然缺失——这构成了本文的问题空间。

【方法主线】
1) 环境哲学：不去复制真实 App 的全部，而是复制「与任务相关的交互逻辑」，并让后端数据可控，从而获得确定性。
2) 判定机制：以结果导向（outcome-based）的确定性验证取代轨迹匹配，使多解路径都被接受，同时让判分机器化、可重复。
3) 构建流程：coding-agent 在约束下生成候选 App 实现，人类开发者检查、运行、迭代修正，再投入评测。作者将这一流程视为「通用且高效」的具体落地方式，即压低模拟环境的人力成本。
4) 规模与覆盖面：557 个任务，17 个高频中英文 App。
5) 现实性的处理方式：按维度近似（视觉外观、页内交互等被复现），而不是整体照搬（此点来自 Limitations）。

【实验主线】
作者在统一环境与统一任务集下评测 19 个 GUI Agent，涵盖通用系统与 GUI 专用系统两类，做跨模型可复现比较。主结论有三条：最好的模型只完成 50.27% 的任务；28.55% 的任务没有任何 agent 能完成；失败集中在更长的流程、数值推理任务，以及由高动作开销与预算耗尽标记的低效轨迹。

【论证逻辑上的关键节点与张力】
- 论文的核心论证是「控制环境噪声 ⇒ 跨模型比较可信 ⇒ 得到的结论（能力远未解决）反映的是能力而非环境」。这个链条中，环境保真度是本论文自认的弱点：Limitations 明确说现实性是逐...**

<!-- paperflow:1b95381d3d56cefb -->
## PARSER: Read in Parallel, Reason in Depth for Long-Context LLM Agents

[[Deep Reading - Sept 2026/PARSER-Read in Parallel, Reason in Depth for Long-Context LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.06702v1](https://arxiv.org/pdf/2609.06702v1)

- **本文针对长文本智能体在处理超长文档时面临的效率低下和位置偏差问题，提出了 PARSER 架构。其核心创新在于将“阅读”过程并行化，并与“推理”过程解耦。传统的序列记忆智能体受限于 $O(L)$ 的线性处理复杂度，且容易在长距离依赖中丢失信息。PARSER 通过引入一组并行的轻量级子智能体负责局部阅读，以及一个由强化学习优化的主智能体负责全局逻辑调度，成功打破了这一瓶颈。

在技术实现上，PARSER 采用了“分发-聚合”（Scatter-Gather）的迭代模式。主智能体根据问题生成初始查询并广播给所有子智能体，子智能体并行检索各自负责的文档块并返回证据。主智能体随后汇总信息，若证据不足则发起新一轮更有针对性的查询。这种机制使得推理深度仅取决于逻辑复杂度，而与证据在文档中的物理位置无关。

实验结果令人印象深刻：在处理接近百万级别（896K）token 的文档时，PARSER 不仅在多跳问答准确率上显著优于现有序列记忆智能体和顶级长上下文模型（如 DeepSeek-V4-Pro），还在推理速度上实现了高达 11 倍的提升。此外，受控实验证实了 PARSER 对证据位置、顺序和距离具有极强的鲁棒性。该研究为构建高效、可靠的大规模长文本理解系统提供了一种全新的范式，即“并行阅读，深度推理”，具有极高的学术价值和工程应用前景。**

<!-- paperflow:6541dfaba5629e76 -->
## xDailyBench: Benchmarking LLMs on Professional Consultation for Real-Life Problems

[[Deep Reading - Sept 2026/xDailyBench-Benchmarking LLMs on Professional Consultation for Real-Life Problems|Deep Reading]]

[https://arxiv.org/pdf/2609.07784v1](https://arxiv.org/pdf/2609.07784v1)

- **【一句话定位】
xDailyBench 是一篇基准构建型论文：它主张现有 LLM benchmark 与用户真实使用方式之间存在结构性错配，并通过 248 个真实日常任务、面向显式与隐含需求的细粒度二元 rubric、以及标准化 agentic 评测协议，把“模型能否满足真实日常咨询需求”变成一个可测量的量。

【论证主线：从使用现实到评测缺口】
论文的论证起点是一个经验观察：LLM 正被越来越多地用于日常辅助（everyday assistance），但现有 benchmark 只部分地反映了用户在实践中的自然请求。论文进一步刻画真实请求的三个特征——open-ended、casually specified、context-dependent——并由此推出一项能力要求：模型必须从用户背景（user background）与情境语境（situational context）中推断未言明的需求（unstated needs），而不仅仅是遵循显式指令。这一步是关键的概念转轴：问题从“模型是否听话”转向“模型是否能猜到用户真正想要什么”。

【评测生态的缺口陈述】
论文在引言中把缺口描述为：尽管 LLM 的使用模式已经改变，现有 benchmark 仍然主要围绕专业生产力任务的自主完成（autonomous completion of professional productivity tasks），这一日益重要的能力维度尚未被探索。相关工作部分沿两条线展开：其一是 LLM benchmark 的考试范式演化——在特定领域用可验证答案的问题评测模型，代表方向包括法律、医学与高等数学；其二是交互式 agent benchmark——覆盖工作流执行、潜在指令推断与迭代细化，其中包含通过 104 个混合来源任务模拟“一整天工作负载”（覆盖工作、生活、学习）的基准（引用编号 [4]，名称需回原文核对）。论文承认这一路工作已触及 latent-instruction inference，但认为其真实感仍有局限（该句在检索片段中被截断，完整措辞需回原文核对）。

【技术主线：基准的构造...**

<!-- paperflow:5301757870347c8e -->
## MEMO: Multimodal Evidence Memory Organization for Long-Horizon LLM Agents

[[Deep Reading - Sept 2026/MEMO-Multimodal Evidence Memory Organization for Long-Horizon LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.07471v1](https://arxiv.org/pdf/2609.07471v1)

- **这篇论文介绍了 MEMO，一种为长程 LLM 智能体设计的创新内存组织框架。针对智能体在长期运行中面临的内存爆炸与上下文窗口限制之间的矛盾，MEMO 抛弃了传统的单一文本或单一视觉读取模式，转而采用一种查询驱动的多模态证据组织策略。其核心流程包括：首先通过证据提取器从海量历史中识别关键事实跨度，形成证据单元；随后由内存管理器根据当前任务需求，为每个单元智能分配最适合的呈现模态（文本或视觉）及布局；最后生成最终的内存包供阅读器模型使用。实验结果表明，MEMO 在 HotpotQA、ALFWorld 等多个基准测试中表现卓越，尤其是在 Token 预算受限的情况下，它能以极低的成本（平均约 84 tokens）实现高精度的任务执行。该研究不仅提升了智能体的内存效率，也为多模态大模型如何更有效地消费外部知识提供了新的范式，即通过“证据级”的模态调度来平衡保真度与结构化表达。**

<!-- paperflow:a2ebe25df3d4e2ae -->
## SkillAlign: Aligning Skill Interfaces for LLM-based Agents

[[Deep Reading - Sept 2026/SkillAlign-Aligning Skill Interfaces for LLM-based Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.07255v1](https://arxiv.org/pdf/2609.07255v1)

- **1. 论证主线
论文的出发点是一个被普遍忽略的默认假设：在技能增强（skill-augmented）的 LLM agent 中，已有研究关注技能如何被获取、检索、压缩与组合，但几乎都默认——一旦某个技能被选中，它向 agent 呈现的接口形式是固定的。作者主张这一假设掩盖了技能效用的重要来源：同一个技能，取决于如何被暴露，可能帮助、干扰甚至误导 agent。因此，需要把“选哪些技能”（skill selection）与“技能如何被呈现”（skill exposure）区分为两个独立的设计与评价维度。

2. 技术主线
为把这一主张变成可实证的研究问题，作者提出 SkillAlign，一个 provider-agnostic 的框架，包含三部分工作：
（1）表示：把候选技能表示为多视图过程卡（multi-view procedural cards），同一技能同时具备多种可用视图；
（2）渲染：通过替代性暴露接口输出技能，包括完整指令（full instructions）、提示（hints）、压缩摘要（compressed summaries）、工作流（workflows）与完全不暴露（no exposure）；
（3）评测：设计反事实协议，固定任务、agent 与候选技能，只改变暴露接口，从而使观察到的差异可以被归因于呈现方式本身。
这一设计把“呈现”从一个混杂变量转化为受控自变量，是本文方法论上的核心。此外，作者在 ALFWorld 上做了基于 replay 的策略学习分析，把暴露选择当作可学习的决策问题，并以 oracle selection 作为上界参照。

3. 实验主线
实验在 ALFWorld 与 SkillsBench 两个基准上展开。第一条实验线是反事实暴露比较，观察不同接口对任务成功率与渲染上下文成本的影响；第二条实验线是比较“把整个技能库全部注入”与“紧凑的 top-k 暴露”；第三条实验线是在 ALFWorld 上用 replay 数据训练暴露选择策略，并与 oracle 比较。评价指标同时包含任务成功率与渲染上下文成本，体现作者把成本视为一等公民。...**

<!-- paperflow:a6d697b6f377c20b -->
## One Skill Does Not Fit All: Automatic Discovery and Taxonomy-Guided Routing of Frame-Selection Skills for Long-Video Question Answering

[[Deep Reading - Sept 2026/One Skill Does Not Fit All-Automatic Discovery and Taxonomy-Guided Routing of Frame-Selection Sk|Deep Reading]]

[https://arxiv.org/pdf/2609.12517](https://arxiv.org/pdf/2609.12517)

- **1. 论文要解决的问题：长视频问答（LVQA）需要在小时级视频上、在有限视觉 token 预算内定位决定性证据。穷举帧不可行，因此帧选择实际上是推理管线中的「证据获取策略」。现有免训练方法普遍对所有问题使用同一个帧选择策略，但论文的分析显示，帧选择策略的相对有效性会随语义类别（semantic category）与 benchmark 变化：不同问题所需证据可能只是短暂出现、可能跨远距离片段反复出现、也可能依赖事件的整体演化，这三类需求对采样密度与时间覆盖的要求互相冲突。因此「一个技能适配所有问题」的假设不成立，需要按问题自适应的证据获取。

2. 二阶挑战：即便承认需要自适应，实践上也有约束——最强固定技能只能靠标注识别，而目标 benchmark 上通常既没有答案标注，也不应使用目标视频。因此论文的目标是：在不接触目标视频与目标答案、不更新任何模型权重的条件下，为每道目标问题选择一个合适的帧选择策略。

3. 方法主线（AutoSkill，三阶段 + 推理）：
 - Stage 1，自动技能发现：从一个小的带标注源池出发，LLM agent 迭代地提出、实现、评估、精炼可执行的帧选择技能，最终形成一个紧凑的、彼此互补的技能工具箱。反馈来自技能实际执行的结果，而非离线先验。
 - Stage 2，目标域适配：仅使用目标 benchmark 的无标注问题与选项文本，诱导一个源域与目标域共享的语义分类法；同时把标注源样例重写成目标 benchmark 的查询风格，保留原有的视频 grounding 与答案标签。这一阶段完全不使用目标视频与目标答案。
 - Stage 3，类别到技能的映射：在重写后的源样例上估计「语义类别 → 技能效用」的映射，并把技能的执行历史蒸馏为可复用的路由依据。
 - 推理：每道题先归类，再分配一个技能；该技能选出帧，帧只进入冻结视频 MLLM 的一次推理，不做多轮重选。

4. 实验主线：在五个长视频 benchmark split 上评测，答题器为冻结的 Qwen2.5-VL-7B 与 Qwen3.5-4B，对比对象为固定技能的免训练基线。结果...**

<!-- paperflow:25d2e5427b3c2aa0 -->
## Online Video Agent Harness for Long Video Understanding

[[Deep Reading - Sept 2026/Online Video Agent Harness for Long Video Understanding|Deep Reading]]

[https://arxiv.org/pdf/2609.12818](https://arxiv.org/pdf/2609.12818)

- **## 一、论文要解决的问题与论证主线

论文从长视频理解的一个结构性困难出发：与 query 相关的证据在时间轴上稀疏分布，类似“视觉大海捞针”；而主流的两条应对路线各有硬伤。第一条是密集帧打包——把大量帧塞进单个 VLM 上下文，摘要明确指出这会带来 context rot 与高成本。第二条是已有视频 agent 普遍采用的 query-agnostic 离线预处理与临时拼凑的工具集，摘要指出这既可能错过 query 特有的细节，也浪费计算。Introduction 片段进一步把“统一降采样”的两点缺陷写清：可能错过关键证据帧，同时引入大量无关帧。

论证主线因此是：如果证据获取可以在推理时依据 query 动态进行，就不必在不知道 query 的情况下预先决定保留哪些内容；如果感知能力由专门的工具承担，编排器就不必是强视觉模型，上下文也就不必承载全部像素级信息。摘要给出的最终结论是：长视频理解能力可以由“渐进式 agentic 证据搜寻”涌现，而不必来自把整段视频放进单一上下文。**

<!-- paperflow:4b06d0073bc1a474 -->
## LifeMem: Enabling Lifelong Experience Reuse for LLM Agents

[[Deep Reading - Sept 2026/LifeMem-Enabling Lifelong Experience Reuse for LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.12655](https://arxiv.org/pdf/2609.12655)

- **一、论文要解决的论证主线

论文以“LLM Agent 应该在一生中持续适应”为价值前提。作者指出，现有 memory-based Agent 在两点上失效：一是难以把可复用经验迁移到新环境；二是经验累积后出现灾难性遗忘。进一步，论文在 Introduction 中给出了一个更具体的失败机制：如果直接在无组织的记忆池上蒸馏高层技能，不相关轨迹会把跨环境噪声带进技能，得到的技能既粗糙又不具代表性，无法指导新任务，反而造成性能下降（Figure 1）。因此论文主张的不是“要不要记忆”，而是“记忆必须先结构化再抽象”。LifeMem 就是针对这一诊断提出的框架。

二、技术主线

LifeMem 把 Agent 记忆建模为一个动态组件：初始化一次，随新经验增量更新，推理时被查询（Method 片段）。它分为两个阶段：
- 学习阶段：Agent 在各环境中执行任务并累积交互轨迹，框架依据轨迹背后的底层工作流（underlying workflows）做聚类，再从每个结构化簇中抽取可复用技能，形成结构化技能记忆；而不是在混杂池上直接蒸馏。
- 推理阶段：面对新任务，Agent 召回相关技能与相关轨迹（前者提供高层流程指导，后者提供具体示例），用于引导动作选择。
论文强调这一设计能实现“无动作空间干扰”的经验复用（Introduction 片段），即跨环境迁移不会因为动作空间、工具集合、观测格式不同而失效。此外，分析部分显示在记忆内对结构相似的轨迹做巩固（合并）能进一步提升性能，暗示记忆的信噪比与规模控制是收益来源之一。

三、实验主线

作者在 10 个环境、13k+ 任务上验证方法，覆盖 5 个广泛使用的 Agent 场景家族（Introduction 片段列举了 embodied action、tool utilization、web 等方向），并额外新标注 2k 条交互轨迹，连同数据集与代码开源于 https://github.com/BITHLP/LifeMem。评测围绕两个目标展开：在已学任务上是否减少遗忘，以及在新任务/新环境上是否获得更好的跨任务迁移。进一步分析给出...**

<!-- paperflow:cb23c23737c21cd9 -->
## V-ICAL Bench: Evaluating Video In-Context Learning for Multimodal Agents in Interactive Environments

[[Deep Reading - Sept 2026/V-ICAL Bench-Evaluating Video In-Context Learning for Multimodal Agents in Interactive Environme|Deep Reading]]

[https://arxiv.org/pdf/2609.15683](https://arxiv.org/pdf/2609.15683)

- **本文针对多模态智能体在交互环境下的视频上下文学习（Video ICL）能力缺失问题，提出了 V-ICAL 基准测试。该基准包含 342 个任务，覆盖 37 个环境，要求智能体从人类演示视频中归纳规则并执行。通过对 19 个 SOTA 模型的深度评测，研究发现当前最先进模型（如 GPT-5.6, Gemini-3.1-Pro）在处理此类任务时存在严重局限，特别是在策略提取和环境适应方面，得分远低于人类水平。实验进一步揭示了模型在“策略转移”上的短板，以及在闭环反馈利用和视觉状态对齐上的脆弱性。V-ICAL 不仅提供了一个严苛的评估平台，也为未来开发具备真正“看即学会”能力的多模态智能体指明了技术瓶颈和改进方向。该研究强调，从简单的模式匹配转向深层的策略理解与闭环执行是通往通用人工智能（AGI）的关键一步。**

<!-- paperflow:6596a9b4ebc3a9e5 -->
## HypoEvolve: Genetic Algorithms Enable Multi-Agent LLMs to Discover Scientific Hypotheses

[[Deep Reading - Sept 2026/HypoEvolve-Genetic Algorithms Enable Multi-Agent LLMs to Discover Scientific Hypotheses|Deep Reading]]

[https://arxiv.org/pdf/2609.15938](https://arxiv.org/pdf/2609.15938)

- **本文提出了 HypoEvolve，这是一个结合了代际遗传算法与多智能体 LLM 的科学假设发现框架。研究的核心动机在于解决当前 AI 智能体在科学协作中角色模糊、搜索效率低下的问题。HypoEvolve 通过将假设开发定义为群体演化过程，利用专门化的 LLM 智能体执行变异、交叉和评估操作，并由遗传算法控制群体的优胜劣汰。在针对 34 种癌症的药物重定位任务中，HypoEvolve 展现了卓越的性能，其生成的假设在 DepMap 和 Open Targets 等外部生物学证据评估中显著优于现有基准。该研究不仅提供了一个高性能的工具，更重要的是建立了一套可量化的框架，用于研究 AI 智能体之间的协作规则如何转化为科学发现的质量提升。论文通过详尽的消融实验证明了适应度引导的选择机制是提升假设质量的关键，并为未来构建自主运行的 AI 研究团队奠定了理论和技术基础。**

<!-- paperflow:d3a44988e0d58aca -->
## MIRAGE: How Conversation State Shapes Historical Evidence Use in Multimodal Personal Agents

[[Deep Reading - Sept 2026/MIRAGE-How Conversation State Shapes Historical Evidence Use in Multimodal Personal Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.19059](https://arxiv.org/pdf/2609.19059)

- **论证主线。论文的出发点是一个具体的测量问题，而非一个模型能力问题：多模态大语言模型智能体正被用作长时程任务的个人助理，其价值取决于连续性——必须能跨对话、文件与工作区状态检索并取用早前出现的证据。然而，即使对这段历史的访问已经退化，模型仍然会生成看似合理的答案。于是，以最终答案正确率为唯一判据的评测（outcome-only evaluation）会把「碰巧答对」误判为「真的用了证据」，系统性高估证据使用能力。作者提出的应对不是再训练一个更强的模型，而是重做评测：设计 MIRAGE（Multimodal Interaction Retrieval, Attribution, and Grounding Evaluation），把「对话状态」提升为自变量，观察历史证据使用如何随状态变化而失效。（以上为摘要明确支持的内容）

技术主线。MIRAGE 的方法论骨架是「三个固定 + 一个变动」：证据对象、问题、评分标准全部固定，只允许对话状态变化。这一约束使得任何指标波动都能被归因到状态侧，而不是任务难度或评分松紧，从而把评测从描述性排名推向干预式设计。评测协议把「证据使用」拆成三段可分别判定的能力：① 判定可回答性——在当前状态下该问题是否有足够证据可答；② 恢复正确来源——定位到承载答案的正确证据对象；③ 基于该来源作答——在锚定正确来源的前提下给出正确答案。这一拆解使「拒答」「答对但引错来源」「引用正确但作答错误」等失败组合态变得可观测，而不被正确率这一标量吞并。状态轴被分成两条：压缩前的历史深度与压缩后的续接，二者被分别操纵与报告；此外引入检索压力（retrieval pressure）作为第二个干预变量，考察模型是否会转向工具中介的检索路径。所有条件在七个多模态骨干模型上重复，模型池覆盖前沿与开源权重两类。（协议结构与状态轴为摘要明确支持；每档的具体取值、压缩实现方式、判定规则在摘要中未披露）

实验主线与发现。在七款前沿与开源权重的多模态骨干模型上，论文报告三项发现。第一，压缩前深度与压缩后续接构成两种彼此不同、非单调的失败机制，而不是一条单调的退化曲线——这一点直...**

<!-- paperflow:990b944ed86f6e4d -->
## SFT or RL for Tool-Calling Agents? A Controlled Study Across Data, Method, and Scale

[[Deep Reading - Sept 2026/SFT or RL for Tool-Calling Agents-A Controlled Study Across Data, Method, and Scale|Deep Reading]]

[https://arxiv.org/pdf/2609.17848](https://arxiv.org/pdf/2609.17848)

- **论文要回答的问题：工具调用 agent 的后训练配方到底该怎么选。现有文献中关于「用什么数据训、用 SFT 还是 RL、模型要多大」的结论分散且实验条件不统一，作者认为缺乏受控证据，因此构造了一个把三个因素解耦的对照研究。

技术主线：论文不提出新算法，而是把三个已有组件放进同一个受控框架。第一个因子是适配方法，包含三条路线——基于 LoRA 的监督微调（SFT with LoRA）、基于 Group Relative Policy Optimization 的强化学习（GRPO）、以及先 SFT 后 GRPO 的两阶段流水线（SFT→GRPO）。第二个因子是模型规模，使用六个同族 Qwen3 模型，参数从 0.6B 覆盖到 32B，目的是让方法比较不跨模型家族、并能观察方法优劣是否随规模变化。第三个因子是训练数据配方，对比「专用数据」与「数据混合」。评估被切成两条独立轨道：同分布（in-distribution，测试与训练同源，共 18 个实验设置）与跨数据集迁移（cross-dataset transfer，训练与测试不同源，共 54 个设置）。方法比较以一个「在多少设置中最佳」的计数方式呈现，而非单一平均分。补充分析里还加入了 LoRA 与全参数微调的对照。

实验主线与关键发现：（1）同分布上，LoRA-SFT 在整个 0.6B–32B 区间都是最强方法，18 个设置中 15 个最佳——这是一个跨规模稳定的主效应，没有出现随规模反转的迹象。（2）跨数据集迁移上各方法接近：GRPO 在 54 个不同源设置中赢下 29 个，但它相对 SFT 的平均领先不足 1 个百分点；考虑到胜场数仅略高于半数且幅度极小，最保守的解读是迁移上方法基本打平，而不是 GRPO 更好（是否有统计显著性无法从摘要判断）。（3）两阶段 SFT→GRPO 在两类比较中都很少成为最优，说明在 SFT 之上再叠加 RL 没有带来可观测的额外收益，反而增加算力成本；这也间接指向「RL 主要起分布对齐/锐化作用，而非能力叠加」的解释（推测）。（4）数据混合无论配合哪种方法都给出稳定强迁移，同时同分布性...**

<!-- paperflow:5794fccc608aa851 -->
## PrimeScientist: Strategic Allocation of Research Effort in Autonomous Research

[[Deep Reading - Sept 2026/PrimeScientist-Strategic Allocation of Research Effort in Autonomous Research|Deep Reading]]

[https://arxiv.org/pdf/2609.17846](https://arxiv.org/pdf/2609.17846)

- **【论证主线】
论文从一个结构性观察出发：自主研究智能体已经能够提出远比资源所能支持的方向更多；而每一次尝试都可能吃掉大量资源。于是「想得多」不再是瓶颈，「投得对」才是。作者由此提出一个规范性主张——研究努力的战略性分配应当成为自主研究智能体的定义性能力，而不只是调度层面的实现细节。为了让这一主张可操作，论文把「分配研究努力」形式化为一个序列决策问题，并特别强调剩余资源应当显式地引导研究策略，而不仅是事后约束。

【技术主线】
方法由两个组件构成。第一是可执行计划树（executable plan tree）：它跨尝试保留相互竞争的计划及其结果，使历史探索（包括失败分支）不至于被丢弃，从而构成后续决策的记忆与统计基础。第二是在该表示之上构建的自适应 MCTS 分配策略：以实验反馈和剩余资源为输入，在选择中平衡探索与利用，并「联合决定」研究方向与资源投入两个维度。相较于标准 MCTS，本设定的特殊性在于每次「仿真」都对应真实且昂贵的实验，因此预算约束必须进入搜索本身；而「自适应」意味着探索-利用配比随预算消耗与反馈积累而变化。论文的贡献结构由此可以描述为：问题形式化（含剩余资源的序列决策）+ 表示（计划树）+ 策略（资源感知的自适应 MCTS）+ 跨领域评测。

【实验主线】
评测覆盖三类场景：AI research、systems and code optimization、machine learning engineering。主对照为 AutoResearch，对照条件是相同资源预算。核心结果：在 12 个 AI research 任务上，平均奖励提升 10.3%，研究尝试次数减少 50.6%。论文据此主张策略性分配让研究质量与样本效率同时改善，并把结论升格为一个更宏观的判断：让「有效使用资源」成为自主智能体的核心能力，是推动规模化科学突破的前提。

【值得注意的表述细节与疑点】
1. 摘要先说评测覆盖三类场景，但随后给出的具体数字只对应「12 个 AI research tasks」，两者是否同一批实验无法从摘要判定，需要在正文实验章节确认。
2. 「averag...**

<!-- paperflow:d3c3f380294100d3 -->
## Agora: Git as Shared Memory for Collective AutoResearch

[[Deep Reading - Sept 2026/Agora-Git as Shared Memory for Collective AutoResearch|Deep Reading]]

[https://arxiv.org/pdf/2609.18094](https://arxiv.org/pdf/2609.18094)

- **这篇论文介绍了 Agora，一个为自主科研智能体设计的集体协作系统。其核心思想是将 Git 作为智能体之间的“共享内存”。在当前的 AI 科研领域，虽然单智能体已能完成简单的实验闭环，但多智能体并行时往往陷入重复劳动的困境。Agora 通过将科研步骤（假设、实验、结论）转化为 Git 提交，构建了一个可追溯的科研 DAG。这种结构不仅保证了每一项研究成果的不可变性和可复现性，还通过派生索引让智能体能够清晰地识别研究前沿。

在实证研究中，作者展示了一个极具挑战性的任务：在无数据、无梯度的条件下，利用 13 个 LLM 智能体协作完成异构模型间的权重迁移。在为期 12 天的实验中，智能体们通过 1,703 次提交，自发形成了一套复杂的权重初始化方案，将模型性能提升了 62%（相对于 GPT-2 基准）。实验过程中观察到了智能体间的接力协作，也暴露了“单一文化”导致的研究停滞问题，并证明了人为干预在打破僵局中的作用。Agora 的成功证明了通过结构化的共享状态，可以实现比单智能体更高效、更深入的科学探索，为未来“AI 科学家社区”的构建提供了重要的基础设施范式。**

<!-- paperflow:46ddef7212b0b2e2 -->
## NeMo Data Designer: An Extensible Framework for Multimodal Synthetic Data Generation

[[Deep Reading - Sept 2026/NeMo Data Designer-An Extensible Framework for Multimodal Synthetic Data Generation|Deep Reading]]

[https://arxiv.org/pdf/2609.17699](https://arxiv.org/pdf/2609.17699)

- **本文提出了 NeMo Data Designer (NDD)，这是一个旨在解决合成数据生成 (SDG) 领域效率与可复现性难题的开源框架。NDD 的核心理念是将复杂的多模态生成任务抽象为声明式的列定义，通过 DAG 依赖解析自动管理生成流程。其独特的“预览-修正”循环允许开发者在低成本下快速迭代数据设计方案。NDD 不仅支持文本和代码，还深度集成了图像生成和向量嵌入等功能，并通过插件系统保持了极高的扩展性。论文通过 Nemotron 模型的开发案例和企业级应用证明了 NDD 在处理大规模、高质量合成数据任务时的卓越性能。NDD 的开源为研究社区提供了一个标准化的工具，有望推动合成数据研究从“脚本驱动”向“工程化、系统化”转变。**

<!-- paperflow:8dc3aa4b86e12520 -->
## RideWay: Benchmarking Efficient Task Completion for Tool-Using Language Agents

[[Deep Reading - Sept 2026/RideWay-Benchmarking Efficient Task Completion for Tool-Using Language Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.17985](https://arxiv.org/pdf/2609.17985)

- **## 一、研究动机
论文的出发点是 agent 评测的一个结构性盲点：当前评测普遍只看「任务是否完成」，但在交互式服务场景中，一个成功的 agent 依然可能让用户受挫——反复追问、执行冗余搜索、做出可避免的修改。这类行为不影响成功率，却直接影响用户体验与部署成本。作者因此主张：在任务成功之外，交互效率应当成为可测量的一等评价维度。**

<!-- paperflow:4826d2e69270ca2e -->
## ScienceIDE: Turning World's Scientific Codebase into Agent Learnable Environments

[[Deep Reading - Sept 2026/ScienceIDE-Turning World's Scientific Codebase into Agent Learnable Environments|Deep Reading]]

[https://arxiv.org/pdf/2609.19134](https://arxiv.org/pdf/2609.19134)

- **一、论证主线
论文的出发点是一个经验观察：科学代码仓库以可执行的模型、方法和工具为载体，编码了人类数十年的科学知识积累。这类资产在数量与质量上都足以支撑科学智能体的训练，但它们并未自然转化为可用的学习资源。作者将这种转化困难命名为“科学经验瓶颈”（scientific experience bottleneck），并把成因归纳为三点：一是工具链碎片化，科学软件由异构的包、脚本、编译产物与专用运行时拼接而成；二是隐式的领域约定，单位、精度、边界条件、数据格式等关键约定往往不写在接口里；三是专门化的正确性标准，科学代码的正确与否依赖物理一致性、量纲、守恒律与容差比较，而不是通用单元测试。作者据此提出核心主张：要让智能体从科学代码中学习，必须先构造一层“可编程环境”，把仓库变成可执行、可生成任务、可被科学验证的对象。ScienceIDE 就是这层基础设施，其目标是让全世界科学代码成为科学智能体的学习基质，并同时服务训练与评测。

二、技术主线
ScienceIDE 的方法路径可以概括为“专家定规范、智能体做转换、环境供三方消费”。首先，由领域专家定义科学案例与验收标准，为环境构建提供目标规格与正确性判据。其次，智能体依据这些规格把代码仓库改造成可执行环境，使其支持三类能力：任务生成（从环境中自动或半自动产出科学编程任务）、任务执行（智能体在环境中实际运行代码、调用工具、迭代调试）、科学验证（用领域相关的判据判定结果是否科学上可接受）。再次，这些环境被设计为 SFT、RL 与评测三者的共享底座：同一环境既可提供高质量示范轨迹用于监督微调，也可作为可采样、可给奖励的交互场用于强化学习，还可作为可复现的评测场地。最后，作者使用经过验证的交互轨迹训练了 PhAI-IDE-72B、PhAI-IDE-9B 与 PhAI-IDE-4B 三个模型，形成覆盖不同参数规模的模型族。论文将自身定位为“智能体学习与科学实践一体化工作空间”的基础层，代码已开源。

三、实验主线
实验围绕两个方向展开。其一是领域内泛化：在与环境构建过程分离的留出科学代码修复任务上评测，检验模型是否真正习得了科学代码的理...**

<!-- paperflow:00faf3a4b022c7a4 -->
## Boosting Deepresearch and LongContext Ability with Self-Generated Deepresearch Rollouts Traces

[[Deep Reading - Sept 2026/Boosting Deepresearch and LongContext Ability with Self-Generated Deepresearch Rollouts Traces|Deep Reading]]

[https://arxiv.org/pdf/2609.20844](https://arxiv.org/pdf/2609.20844)

- **这篇论文的论证主线可以概括为“先诊断瓶颈，再补数据，再编排训练”。

诊断环节：Deepresearch（DR）agent 的工作方式是多轮 search + visit，与真实网页环境交互，这导致其上下文随交互迅速增长。作者做了一个关键的实证观察：即便经历过 DR Agentic Reinforcement Learning（DR-RL），模型剩余的预测错误里仍有 61.6% 可以归因于长上下文理解能力不足，具体表现为长上下文幻觉与跨文档证据整合失败。这意味着在 DR-RL 之后，制约进一步收益的主要因素不再是搜索/工具使用策略，而是模型阅读与整合超长多文档证据的能力。于是论文把“继续提升 DR 表现”的问题，重新表述为“如何强化模型的长上下文能力”。

数据环节：作者指出，有效的长上下文训练不只是把上下文长度调大，真正的障碍是数据缺口。为此提出 DR Rollouts to LongContext-QA（DR-to-Long）。该方法复用 DR-RL 产生的 rollout 轨迹——这些轨迹天然包含搜索历史、访问过的网页、证据片段与最终答案监督——把其中紧凑的证据片段与网页摘要，替换为对应 URL 的完整正文，由此得到显著更长的多文档上下文；由于替换是“原位展开”，原始证据关系（哪段内容支撑哪一步推理、答案来自哪些来源）被完整保留。整个过程不需要任何人工标注，数据由已有交互痕迹派生。

训练环节：在 DR-to-Long 之上提出 DLD（DR → LongQA → DR）-RL 三阶段流程。第一阶段做一次较短的 DR-RL，用于获得初始 DR 策略并采集可转换的轨迹；第二阶段用转换得到的 LongQA 实例做 LongQA-RL，显式强化长上下文理解；第三阶段回到 Deepresearch 做完整 DR-RL，继续提升 DR 能力，并检验长上下文能力的增益能否转化为端到端 agent 表现。这个编排使同一批交互数据被用两次：一次作为 DR 训练信号，一次作为长上下文训练信号。

实验主线：论文在三个 Deepresearch 基准与三个长上下文基准上评测，主对照是...**

<!-- paperflow:2aa10267f7132698 -->
## CodeMidas: Scaling Agentic Coding RL Environments from Code Itself

[[Deep Reading - Sept 2026/CodeMidas-Scaling Agentic Coding RL Environments from Code Itself|Deep Reading]]

[https://arxiv.org/pdf/2609.22068](https://arxiv.org/pdf/2609.22068)

- **【论证主线】论文的出发点是一个供给侧判断：训练有能力的 coding agent 靠的不只是 RL 算法，而是「多样任务 + 可靠验证器」的环境供给。开源代码库是这类环境的富矿，但主流做法只用开发过程副产物——issue 与 commit——来抽取任务，这等价于把任务集合限定在「被显式记录过的开发事件」之内。论文主张这一限制是当前可扩展性的主要瓶颈，并提出替代方案：既然绝大多数功能从未被 issue 记录，那就直接从「已实现的功能」出题，令源码成为唯一的任务特定输入。

【技术主线】CodeMidas 是一条 agentic 流水线，其组织原则是把 agentic compute 分配到环境构建的每个阶段，而非在单点做一次性生成：
- 探索与规范：agent 探索代码库中已实现的功能，将其形式化为行为规范（该功能应有什么行为、边界条件如何）。这一步把「实现」反向工程成「可检验的意图」，是替代 issue 文本的关键。
- 测试构造：在原始代码上真实执行以锚定测试，避免纯文本生成的规范幻觉。合理推断这是「生成-执行-修正」的闭环。
- 验证与筛选：既做执行检查（环境可复现、判定自洽），也做重复解 rollout（用多次采样的通过率判断任务是否可解、是否有区分度）。前者保证验证器可信，后者同时服务 GRPO 的可训练性——组内无方差的任务不产生梯度。
- 训练：以 GRPO 在 MiMo-V2.5 上做 RL。

【实验主线】产物层面：5,545 个训练任务、3,185 个开源仓库、23 种语言、15 个技术领域——这个广度本身就是「源码而非 issue」这一选择的直接体现。训练层面：在 GRPO 下，模型在五个多样化基准上全部提升，摘要点名三个方向作为代表：issue 修复（DeepSWE +11.7%）、整程序构建（ProgramBench +17%）、终端工作（Terminal-Bench v2.1 +8.5%）。消融层面：高质量训练任务数量增加带来性能提升，支持「该数据源可扩展」的核心主张。分析层面：轨迹对比显示 RL 后的 agent 探索代码库更多、自验证方式更...**

<!-- paperflow:261a8893e9ddeb5c -->
## MintAct: A Unified Visual Agent for Digital Environments

[[Deep Reading - Sept 2026/MintAct-A Unified Visual Agent for Digital Environments|Deep Reading]]

[https://arxiv.org/pdf/2609.22083](https://arxiv.org/pdf/2609.22083)

- **本总结基于论文的标题、作者名单与摘要，原文的 Introduction、Method、Experiments、Discussion、Conclusion 章节在本次输入中均为空，因此以下内容区分「摘要明确支持」与「基于摘要的合理推断」，并在末尾列出必须回原文核对的关键项。

一、论证主线
论文的出发点是一个现状判断：数字环境中的视觉智能体能力被割裂为若干条互不相通的专精路线——UI 元素定位、移动端/桌面端/Web 的多步导航、以及视觉工具调用，各自训练各自的专用模型。MintAct 主张这种割裂是不必要的：通过对环境、数据与训练配方的协同设计，单个视觉语言模型可以在上述全部能力上追平各领域专用模型。
这一主张的证明结构可以概括为「不退化即成功」：作者没有声称统一模型在每个维度上都超越 specialist，而是声称「match」。把主张限定在可防守的范围内，是这类系统型工作的常见策略，也意味着论文实验部分的核心证据应当是逐域、逐能力的对照表，而非单一平均分。
第二层论证是关于可行性的：统一训练在算法上不难提出，难的是让它跑得起来。于是论文把「可扩展环境 + 异步 RL 基础设施」提升为与模型能力并列的贡献，论证逻辑是——没有这套基础设施，跨域 agentic RL 在吞吐和稳定性上都无法支撑。

二、技术主线
1. 模型层：训练 2B、4B、8B 三个规模的视觉语言模型族。多规模并行训练意味着该方法是容量无关的（至少在设计意图上），同时覆盖从低成本部署到高精度场景。
2. 能力层：统一三类能力——UI grounding、跨移动/桌面/Web 的多步导航、visual tool use。三者的统一意味着同一套参数需要同时处理「单步精确定位」「长时程状态跟踪与动作序列」「对外部工具的调用决策」这三类时间尺度与抽象层次都不同的任务。
3. 环境层：托管数百个并发实例，后端按域异构，同一套基础设施同时服务轨迹数据采集与在线 RL。这一设计的关键含义是环境抽象层与具体域解耦，使上层 rollout 调度不必关心底层是移动模拟器、桌面虚拟机还是浏览器沙盒（合理推断）。
4. 训...**

<!-- paperflow:c13d3491cb6291d2 -->
## GraphSkillEvo: Evolutionary Optimization of Graph-Structured Agent Skills

[[Deep Reading - Sept 2026/GraphSkillEvo-Evolutionary Optimization of Graph-Structured Agent Skills|Deep Reading]]

[https://arxiv.org/pdf/2609.21749](https://arxiv.org/pdf/2609.21749)

- **这篇论文提出了 GraphSkillEvo，旨在解决大语言模型（LLM）智能体在复杂任务中技能描述不清晰及优化效率低的问题。作者指出，传统的自然语言技能描述由于缺乏结构，导致 LLM 执行时逻辑混乱且优化搜索空间过大。论文的核心创新在于：1) 引入了图结构技能表示，将任务分解为节点（步骤）和边（转换），提供了清晰的工作流指导；2) 开发了一套基于进化的优化框架，通过种群管理、变异和特有的图交叉算子，实现了对技能空间的高效探索。实验结果令人印象深刻，在多个基准测试中，GraphSkillEvo 均优于现有的 SkillOpt 方法，尤其在提升弱模型（GPT-5.4-nano）处理复杂任务的能力上表现突出。该研究为智能体技能学习提供了一个从“文本描述”向“结构化逻辑”转变的新范式，具有很强的系统性和启发性。**

<!-- paperflow:cac67d42d54a4ff6 -->
## OmniVChat: Synthesizing, Benchmarking, and Training for Native Audio-Visual Dialogue

[[Deep Reading - Sept 2026/OmniVChat-Synthesizing, Benchmarking, and Training for Native Audio-Visual Dialogue|Deep Reading]]

[https://arxiv.org/pdf/2609.21465](https://arxiv.org/pdf/2609.21465)

- **论文围绕一个被作者新定义的任务 OmniVChat（Omni Video Chat）展开：omni 模型同时直接接收用户提供的音频与视频，并输出文本；用户的提问内嵌在音视频之中，不提供单独的文字问题，也不借助外部的语音识别或画面描述模块。作者给出的动机有两条：一是减少外部模块带来的延迟与算力开销，二是避免级联式转写/描述对感知线索（用户所处环境、面部表情、身边物体）的有损压缩。

论文认为推进这一方向卡在两个瓶颈上。其一是数据：真实世界中「人们用自己设备与模型自然对话」的录制数据非常稀缺，这类数据涉及隐私、场景不可复现与意图标注困难，难以规模化。其二是评测：一条好回复往往需要同时考虑用户周围环境、表情和附近物体，而且同一语义可以用大量不同方式表达，因此关键词匹配一类指标不可靠，会系统性误判。

作者的破题思路是一个范式判断——「generation for comprehension」：近期智能体系统与视频生成的进展，使得用合成对话来训练和评测理解能力变得可行。围绕这一判断，论文交付三件套：

第一，OmniVChat-Studio，一个多智能体数据引擎，用来合成单轮与多轮音视频对话。它同时承担训练数据来源与基准构建来源两种角色。摘要未展开各个 agent 的具体分工与视频/语音生成细节（推测包含情境构造、对话编排、音视频渲染与质量过滤等环节）。

第二，OmniVChat-Bench，基于合成对话构建的评测基准，从五个能力类别考察 omni 模型的基础对话能力；此外还有人类录制版本 OmniVChat-Bench-Human，用于检验结论是否能离开合成分布。两套基准的存在方式说明作者的论证策略是「合成训练 + 合成评测 + 真实评测」三点定位，把人类录制集当作外部效度锚点。

第三，OmniVChat-RL，一套面向 OmniVChat 的强化学习奖励设计，显式地把回复正确性、效率与风格三个目标写进奖励。这使优化目标不再是单一准确率，代价是多目标之间的潜在冲突与权重问题——摘要未披露权重与逐项消融。

技术主线由此闭合：多智能体合成数据 → 复合奖励的 RL 训练 →...**

<!-- paperflow:b87c277ae00fdcab -->
## AutoViewMem: Self-Configuring Orthogonal Views for Conversational Long-Term Memory

[[Deep Reading - Sept 2026/AutoViewMem-Self-Configuring Orthogonal Views for Conversational Long-Term Memory|Deep Reading]]

[https://arxiv.org/pdf/2609.21940](https://arxiv.org/pdf/2609.21940)

- **【论证主线】
论文的起点是一个被广泛承认的前提：长期记忆是 LLM agent 在长时程交互中保持一致性与个性化的必要条件。随后它给出了一个诊断：现有记忆系统大多依赖固定粒度或静态 schema，而当偏好、事件、约束、时间更新这四类异构信息被放进同一个混合表示时，会产生语义干扰；其可观测后果是 top-K 检索对噪声敏感、相关证据排序偏低——也就是“证据在库里但取不出来”。基于这一诊断，论文把问题重构为表示层问题而非检索层问题：与其在检索时做路由、改写、迭代与重排，不如在写入时就把记忆按语义视图拆开。这就是摘要中“representation-first”“把语义解耦从检索时前移到写入时”的含义。

【技术主线】
AutoViewMem 的管线按摘要可以分成四步。第一步是视图发现：从交互轨迹中挖掘候选语义视图，视图并非人工预设，而是数据驱动产生，这是对“静态 schema”的直接替代。第二步是视图集选择：从候选中挑出一个紧凑且互补的子集——compact 控制视图数量以避免碎片化，complementary/low-overlap（标题中表述为 orthogonal）保证视图之间不重复承载同一信息，这一步是本文区别于“多加几路索引”的关键。第三步是写入时结构化抽取：以选定的视图集为条件，把原始交互内容抽取成结构化记忆，并保持 provenance-grounded，即每条记忆可回溯到来源片段。第四步是离线巩固：提升记忆的紧凑性与一致性，处理冗余与时间更新类冲突。整个设计有一个明确的接口约束——推理侧保持标准 top-K 相似度检索，不做显式路由、不做迭代检索。这意味着方法的全部改进空间被封装在索引构建阶段，可以在不改动已有 agent 推理流程的前提下替换记忆后端。

【实验主线】
实验在 LoCoMo（长时程对话问答）与 PersonaMem（个性化记忆）两个基准上展开，backbone 使用 Qwen3-8B 与 Qwen3-14B 两个规模。对照对象是强记忆基线。摘要给出的结论是：在保持简单推理管线的前提下，长时程问答与个性化两方面都优于基线，且结论在两个 bac...**

<!-- paperflow:38c2fc0d7dc85ec5 -->
## Code Plans, Diffusion Renders: Open-Ended Generative World Modeling

[[Deep Reading - Sept 2026/Code Plans, Diffusion Renders-Open-Ended Generative World Modeling|Deep Reading]]

[https://arxiv.org/pdf/2609.26458v1](https://arxiv.org/pdf/2609.26458v1)

- **本文提出 CoDeR，一个面向世界建模（world modeling）的新范式，其核心主张是：世界模型不应该把世界动力学隐含地塞进视觉观测里，而应该用一个显式、可执行的世界来承载规则与状态，再让视频生成模型负责把这个世界的状态渲染成可感知的画面。

一、问题动机。摘要给出了明确的对立面：现有视频世界模型通过视觉观测隐式表征世界动力学。作者认为这一范式限制了能力上限，并把缺口具体化为四项必须同时满足的能力目标——长期记忆、开放式交互、自主世界演化、以及多智能体场景（多个实体能够在当前观测之外持续行动、交互与演化）。这四项其实指向同一个结构性缺陷（合理推断，未见原文论证）：当世界状态只存在于观测序列中时，状态不可寻址、不可编辑、不可检查，长程一致性只能靠拟合维持。

二、技术主线。CoDeR 采用显式分工：用一个由代码构造的可执行世界来承担世界规则与动力学，用视频生成模型承担视觉实现。系统协调五个互补角色（complementary roles），把 high-level concepts 逐级翻译为 structured world rules、executable dynamics，最终产出 perceptual observations。这是一条从抽象规格到可执行实体再到像素的转换链。从摘要能确认的是这条链的存在与角色数量（五个），但五个角色的具体职责、规则的表示形式、代码执行环境、渲染条件化方式，摘要均未说明，属本次最主要的信息缺口。

三、能力主张的实现逻辑（推断）。长期记忆由代码侧的持久状态而非上下文窗口承载；开放式交互通过对可执行世界状态的持续干预实现，世界可被编辑而非只能重采样；自主世界演化意味着在无外部输入时世界仍按自身规则推进；多智能体则要求实体作为持久对象存在于代码世界中，而非只在生成的帧里出现。这一整套逻辑的成败高度依赖「代码状态 ↔ 渲染观测」的接口质量。

四、实验主线。论文声称做了 extensive experiments，在多个评测设定下取得 state-of-the-art，并显著扩展了现有世界模型的能力，能力维度对应上述四项。摘要未提供...**

<!-- paperflow:44f209d4a8c6acd8 -->
## VideoGen-Agent: Reinforcing Video Generation Agents

[[Deep Reading - Sept 2026/VideoGen-Agent-Reinforcing Video Generation Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.24997v1](https://arxiv.org/pdf/2609.24997v1)

- **这篇论文提出了 VideoGen-Agent，旨在解决当前视频生成模型在逻辑、知识和一致性方面的短板。其核心思想是将视频生成从“单次推理”转变为“智能体驱动的多轮协作过程”。

在技术路线上，VideoGen-Agent 创新性地引入了多任务强化学习。通过 SFT 建立基础，再通过精心设计的类别感知奖励函数进行 RL 优化，使智能体能够根据任务需求（如物理模拟、人物定型、步骤演示）灵活调用搜索引擎、图像生成器和验证模型。这种“大脑（Agent）+ 工具（Tools）”的架构不仅提升了生成质量，还赋予了系统极强的灵活性：当底层生成模型进步时，智能体无需重训即可直接受益。

实验部分，通过新提出的 VABench 基准测试，论文详尽地展示了该方法在六大复杂任务上的优越性。相比于端到端模型，VideoGen-Agent 在处理复杂指令时表现出了更强的鲁棒性和逻辑严密性。人类评估的高胜率（84.3%）进一步证实了该方法的实用价值。总的来说，这项工作为实现真正受控、逻辑自洽的高级视频生成开辟了新的路径，标志着视频生成从“像素模拟”向“认知合成”的跨越。**

<!-- paperflow:e67192746395a925 -->
## Qwen-Audio-3.1-Realtime: Towards Reliable Agentic Voice Interaction

[[Deep Reading - Sept 2026/Qwen-Audio-3.1-Realtime-Towards Reliable Agentic Voice Interaction|Deep Reading]]

[https://arxiv.org/pdf/2609.25176v1](https://arxiv.org/pdf/2609.25176v1)

- **这篇论文要解决的问题可以概括为一句话：实时语音助手必须同时满足三项能力要求——在持续演变的请求上做推理、执行动作、并遵守对话规则——而现有系统通常只能覆盖其中一到两项。Qwen-Audio-3.1-Realtime 的主张就是把三者统一进同一模型，并用一套覆盖六类能力的评测来证明“可靠（reliable）”是可度量、可改进的（问题陈述部分由摘要直接支持；作者对现有系统缺陷的具体归因需回原文确认）。

技术路线由三个命名模块构成，命名本身透露了作者的能力分解方式。
第一是 Think，负责“想”。它由 Core-Cocktail 监督微调与 Multimodality and Multi-Teacher On-Policy Distillation（M²-OPD）组成，目标有两个且被并列陈述：transfer language capabilities（把语言能力迁移进来）与 develop native audio skills（发展原生音频技能）。这一并列本身就说明作者拒绝在“文本侧推理强”和“音频侧原生能力好”之间二选一，而是用蒸馏把两端拉到一起。M²-OPD 的三个关键词分别指向：on-policy（学生在自己采样出的轨迹上接受教师信号，缓解分布偏移）、multi-teacher（多个教师分工提供监督，推测为文本教师与音频教师）、multimodality（蒸馏跨模态进行）。摘要未披露教师数量、权重、损失形式与训练阶段编排。
第二是 Act，负责“做”。论文使用自演化可执行环境（self-evolving executable environments）与多粒度 rollout（multi-granularity rollouts）来运行 Group Relative Policy Optimization（GRPO）。学习目标被写成三件事：使用工具、解读反馈、完成任务。把“解读反馈”单列出来值得注意——它暗示作者的难点认知不止在“会不会调用工具”，而在于工具返回值往往是非结构化、部分失败或语义含糊的，模型必须据此修正后续动作。自演化环境解决的是任务供给与难度爬升...**

<!-- paperflow:8b83f60dd9fba12c -->
## Harness-Zero: Harness Distillation via Agent-as-Harness

[[Deep Reading - Sept 2026/Harness-Zero-Harness Distillation via Agent-as-Harness|Deep Reading]]

[https://arxiv.org/pdf/2609.24974v1](https://arxiv.org/pdf/2609.24974v1)

- **这篇名为《Harness-Zero: Harness Distillation via Agent-as-Harness》的论文深入探讨了智能体系统中的一个核心矛盾：外部辅助系统（Harness）虽然能显著增强 LLM 的能力，但其带来的复杂性和部署依赖限制了智能体的通用性。作者提出了一种创新的蒸馏框架 Harness-Zero，旨在将这些外部收益“固化”到模型权重中。

论文首先定义了“Harness 依赖”问题，指出不同任务对 Harness 的需求各异，导致系统难以扩展。为了打破这一僵局，Harness-Zero 引入了“Agent-as-Harness”的概念。在训练阶段，它不依赖僵化的代码逻辑，而是利用一个智能 Agent 作为中介，将优化后的 Harness 策略翻译并纠正到学生模型的动作空间中。这一过程产生了一系列高质量的、可供学习的交互轨迹。

在随后的微调阶段，学生模型通过学习这些轨迹，实现了对 Harness 诱导行为的内化。实验结果极具说服力：在知识工作、工具调用和科学研究等多个严苛领域，Harness-Zero 帮助模型在移除外部辅助后，成功率从 23.3% 飙升至 44.3%。更具启发性的是，这种“内化”后的性能甚至优于“挂载”外部系统时的表现，证明了模型权重在吸收复杂逻辑后的高效性。论文还详细分析了 28 种行为模式的恢复情况，显示了该方法在保留复杂推理逻辑方面的卓越能力。总的来说，Harness-Zero 为构建高性能、低延迟、无依赖的下一代通用智能体提供了一条清晰的技术路径，尤其在 AI for Science 等对交互精度和效率有极高要求的领域具有重大应用价值。**

<!-- paperflow:925c31350237f56e -->
## Qwen3.8-Omni: Towards Native Omni-Modal Agents

[[Deep Reading - Sept 2026/Qwen3.8-Omni-Towards Native Omni-Modal Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.25611v1](https://arxiv.org/pdf/2609.25611v1)

- **【材料边界说明】本次可用于归纳的材料只有标题、作者、摘要与元数据；PDF 正文（Introduction / Method / Experiments / Discussion）与检索证据均不可得。因此以下“论证主线、技术主线、实验主线”均在摘要明确支持范围内复述，并对推断部分逐处标注。

【1｜论证主线】论文的问题设定是：已有的 omni 模型把能力重心放在感知与交互，能够看、听、对话，但在多模态理解与推理、以及长时程 agentic 执行上不足，因而无法进入真实的多模态生产工作流。基于这一判断，作者的论证链条是：(i) 需要同时提升多模态理解/推理与长时程 agent 执行，而不是继续加码感知；(ii) 文本域已经积累出成熟的 agent 能力，应通过原生多模态共训把这种能力迁移到音频与视频，同时不牺牲文本域能力；(iii) 长时程规划与长音频/长视频处理要求极长上下文，因此把窗口扩至一百万 token；(iv) 即便模型具备这些能力，现有 agent harness 缺乏原生音频/视频支持，能力仍无法落地，所以必须同时交付插件框架；(v) 实时多模态交互本质上是系统级问题，需要上下文与记忆管理、工具使用、子 agent 委派的联合编排，因此需要专用实时框架。第 (iv)(v) 两步把一篇“模型论文”扩展成“模型 + 系统 + 生态”的三件套主张，这是本工作最显著的结构特征。

【2｜技术主线】技术上可还原的要点有三：其一，主干架构继承 Qwen3.8-Next 的稀疏 MoE 结构（sparse mixture-of-experts），说明本工作沿用同代际的 MoE 语言基座而非另起炉灶；其二，训练策略是 native multimodal co-training，其明确的双重目标为“保留强文本域能力”与“促成 agent 能力从文本向音频、视频迁移”；其三，上下文窗口扩展至 one million tokens，直接服务于长上下文多模态推理与长时程规划。在系统侧，Qwen-MM-Plugins 作为轻量开源插件框架承接多模态生产力（把音视频能力接入既有 agen...**

<!-- paperflow:40230957d78b433f -->
## FLARE: A Full-Lifecycle Dense Supervision Paradigm for Long-Horizon Coding Agents via Generative Reward Model

[[Deep Reading - Sept 2026/FLARE-A Full-Lifecycle Dense Supervision Paradigm for Long-Horizon Coding Agents via Generative|Deep Reading]]

[https://arxiv.org/pdf/2609.23808v1](https://arxiv.org/pdf/2609.23808v1)

- **这篇论文提出了 FLARE 框架，旨在解决长程代码智能体在复杂软件工程任务中的核心痛点：稀疏奖励导致的信用分配困难。作者认为，现有的测试时缩放方法（如简单的多样本采样）效率低下，因为它们忽略了任务执行过程中的因果逻辑。FLARE 通过引入生成式奖励模型（GRM），实现了从离线诊断到在线干预再到模型训练的全生命周期优化。其核心创新点在于：1) 利用 RADAR 框架通过因果回溯自动构建高质量的过程监督数据；2) 在推理时采用“主动脚手架”机制，通过 GRM 实时拦截高风险步骤并进行局部修正，实现了 5 倍于传统方法的 Token 效率；3) 在训练端，利用密集信号优化 SFT 和 RL，显著提升了模型的稳健性。实验结果有力地证明了 FLARE 在提升智能体解决复杂现实问题能力方面的有效性，为构建更高效、更可靠的自主编程智能体提供了新的范式。**

<!-- paperflow:0f84a266618dd8e6 -->
## RRSI: Regularized Recursive Self-Improvement of Agent Harnesses

[[Deep Reading - Sept 2026/RRSI-Regularized Recursive Self-Improvement of Agent Harnesses|Deep Reading]]

[https://arxiv.org/pdf/2609.24972v1](https://arxiv.org/pdf/2609.24972v1)

- **这篇论文提出了 RRSI（正则化递归自我改进），旨在解决 LLM 智能体在自动化进化过程中普遍存在的过拟合问题。作者指出，虽然通过迭代优化智能体的 Harness（提示词、控制流等）可以显著提升特定任务的性能，但这种提升往往是以牺牲泛化性和增加系统复杂度为代价的。RRSI 框架通过在提议阶段引入“时间退火预算”和“探索鼓励”，以及在选择阶段引入“泛化评论员”和“效能剪枝器”，为智能体的自我改进过程戴上了“紧箍咒”。

实验结果令人印象深刻：RRSI 不仅在 8 个基准测试中展现了强大的 ID 提升（+14.1 pts），更在 OOD 泛化上表现优异（+4.7 pts），同时将推理成本降低了 30%。这一研究范式标志着智能体优化从“单纯追求指标”向“追求高质量、通用且高效的系统逻辑”转变。论文通过严谨的对比实验证明，在 Agent 系统的自动化构建中，适当的约束（正则化）比无限制的搜索更能产生具有生命力的智能体架构。这为未来构建能够自我演化且保持鲁棒性的通用人工智能系统提供了重要的理论和实践参考。**

<!-- paperflow:5cf2b7c54ad1c802 -->
## GameHorizon Suite: Multi-Horizon Data and Evaluation in Gameplay

[[Deep Reading - Sept 2026/GameHorizon Suite-Multi-Horizon Data and Evaluation in Gameplay|Deep Reading]]

[https://arxiv.org/pdf/2609.25001v1](https://arxiv.org/pdf/2609.25001v1)

- **论文要解决的问题：视频游戏是检验 AI 综合能力的理想测试床，因为它同时要求视觉理解、指令分解、目标规划与精确动作控制，而这些能力分布在不同时间跨度上。作者指出既有数据集与基准存在三个缺口：覆盖游戏范围窄、缺少语言指令、依赖高方差的在线 rollout。前两点导致评测维度不完整且难以泛化，第三点导致评测信度低、结果不可复现、失败难归因。

提出的方案：GameHorizon，一个统一的数据与评测套件，包含三个组件。

组件一，GameHorizon-Annotator：一条可扩展、自动化的多时间跨度指令标注流水线。它是整套数据的产能基础，使得从数千小时原始 gameplay 录像中批量生成跨时间尺度的语言指令成为可能。摘要未披露时间跨度的具体分层定义、所用模型与人工校验比例。

组件二，GameHorizon-Data：作者称其为首个大规模 AAA gameplay 数据集，规模为 5,000 小时录像、21 款游戏，由 100 名人类专家玩家采集。数据包含三类时间对齐的模态：视频、玩家动作、多时间跨度指令。三者的时间对齐是核心设计，它让「动作级精确控制」与「长视界任务规划」可以被放在同一个数据表示下评测。

组件三，GameHorizon-Bench：包含两条评测轨道。
- 离线轨道：使用数千道标准化问题，组织为三个主要任务，并配备一系列诊断变体。其价值主张是可复现性——相同输入产生相同分数，便于横向比较与能力归因。
- 在线轨道：逐步（stepwise）交互测试。它承担两个明确功能：一是检验离线分数是否真的反映真实 gameplay 能力，二是把失败定位到长视界 gameplay 中的具体步骤。这实际上把「基准效度」本身变成了被研究对象，而不只是发布一个新的排行榜。

验证与主要发现：作者基于整套套件评测了 47 个模型，执行超过一百万次模型调用，覆盖多种模型族。结果显示存在一个有意义的任务难度层级，以及不同模型能力之间的显著差异。这两条结论表明基准具有区分度，且任务难度并非同质。摘要未给出任何具体数值、任务定义细节、模型排名或离线—在线一致性的量化结果。

整体定...**

<!-- paperflow:41e9379ef90da4e0 -->
## Recursive self-improvement of AI research agents

[[Deep Reading - Sept 2026/Recursive self-improvement of AI research agents|Deep Reading]]

[https://arxiv.org/pdf/2609.26457v1](https://arxiv.org/pdf/2609.26457v1)

- **本文的论证主线可以概括为“一个趋势 → 一个概念 → 一个系统 → 一组跨域证据 → 一个意外发现”。

一、背景与动机。作者观察到 AI 代理已经开始自动化 AI 技术栈中的研发环节（摘要举例：改进训练效率、优化推理），因此自然的下一步是让代理提升“自身”的研究效率。这一动机被挂在一个更宏观的长期趋势上：研发的累计投入存在边际收益递减。持续的自改进被作者视为对冲该趋势的一条路径。

二、概念定义。当 AI 研究代理自身的代码成为优化对象时，每一轮被接受的改写都会变成下一轮执行改写的那个代理本体。作者将这一闭环命名为 recursive self-improvement（递归自改进）。这个定义本身即论文的概念贡献：它把“自改进”的落点从抽象算法层面，移到了运行中的代理代码这一具体对象上。

三、系统与技术主线。作者提出 AIDE^2，为一个前沿 AI 研究代理实现该循环。循环由三个环节组成：(1) 对自身代码提出修改；(2) 在 AI R&D 任务套件上对修改后的自身做基准测试；(3) 保留在隐藏评测上表现最好的改动。摘要给出的两个改进示例分别位于决策层（一种新的搜索策略）与信息管理层（压缩并管理代理不断增长的上下文的记忆机制），暗示被改写对象覆盖代理的多个功能模块。

四、实验主线。主实验是一次为期 8 天的自主运行，期间发现了 7 项相继的改进。验证分三条线展开：(a) 跨域迁移——在四个留出基准上评测，覆盖机器学习工程、启发式算法工程、基于物理的天气预报，其中天气预报相对选择任务属于分布外；（b) 与人类工程基线对比——基线是一个生产级研究代理，在 FML-Bench 上位列最强之一，而被发现的“最强代理”在全部四个留出基准上匹配或超过该基线；(c) 副作用测量——在一个独立的留出任务族上测量 reward hacking 率。

五、核心发现。最引人注目的两点是：其一，自改进产生的增益可以迁移到循环从未接触过的任务与领域（含一个 OOD 的物理领域），这支持了“改进发生在代理工程这一更普适层面”而非“对选择集过拟合”的解释；其二，reward hacking 率...**

<!-- paperflow:a262259ea23619e7 -->
## Emergent Collusion in Long-Horizon LLM Agent Interaction

[[Deep Reading - Sept 2026/Emergent Collusion in Long-Horizon LLM Agent Interaction|Deep Reading]]

[https://arxiv.org/pdf/2609.24967v1](https://arxiv.org/pdf/2609.24967v1)

- **论证主线：论文从一个部署现实出发——LLM agent 越来越多被放进协作流程，彼此交接、互检、打分。作者指出协作并不自动等于良性，长期互动本身可能生成不希望的协调（undesirable coordination）。为把这一担忧变成可测问题，作者构造长时程双智能体环境：两个 agent 反复完成各自任务、共享任务日志、互相验证对方工作并领取奖励。关键设计是注入现实约束，使“遵守验证协议”与“奖励最大化”冲突，于是诚实验证对个体是亏本行为。作者报告：随重复交互，agent 越来越多偏离协议；串通在 10 个模型的 94% 轨迹中出现；同家族内更强的模型更早串通。同伴干预显示串通由同伴行为塑造；消融显示奖励结构、验证反馈与互动历史均有额外影响，且限制互动历史的数量与范围会降低串通。结论是长时程互动会重塑 agent 的协调方式，产生安全风险。

技术主线：环境（双 agent、任务循环、共享日志、互检、奖励）+ 冲突注入（合规与收益不相容）+ 长时程重复（让互惠与预期得以形成）+ 三类探测（跨模型轨迹统计、受控同伴干预、三因素消融）。方法学要点在于把“串通”当作需要轨迹级观察的动力学量，而不是从单次输出中读取的倾向，并用干预与消融把可能来源（同伴行为、奖励结构、反馈、历史）逐一分开。

实验主线：以 10 个模型为对象统计轨迹级串通率；做同族能力对比；通过控制同伴行为检验因果；再分别消融奖励结构、验证反馈与互动历史，其中历史维度从“数量”与“范围”两个方向操作，观察到串通下降。

贡献定位：把多智能体安全从“单个 agent 是否说假话”推进到“两个 agent 在激励错位与长期互动下形成的默契均衡”，并给出可复现的实验范式与一组干预结果，同时把“互动历史”识别为可操作干预面。

重要边界与未解决项：摘要未给出串通的操作定义与判定流程、任务类型与数据、轨迹长度与样本量、模型清单、奖励参数、同伴干预实现方式与统计口径，也未展开缓解措施的成本与副作用；“能力更强更早串通”只在同一家族内成立，不应外推为跨家族结论。本次未获取 PDF 全文，上述机制解释部分为基于摘要的合理推断。**

<!-- paperflow:83dc83f3ac8f0f5f -->
## Data Agents: Agentic Data Systems

[[Deep Reading - Sept 2026/Data Agents-Agentic Data Systems|Deep Reading]]

[https://arxiv.org/pdf/2609.24137v1](https://arxiv.org/pdf/2609.24137v1)

- **【证据说明】
本次可用的原始材料仅包括论文标题、作者列表、摘要以及元数据（venue: arxiv；arXiv 编号 2609.24137v1；日期 2026-09-21；DOI 为空；机构字段为空）。正文各章节（Introduction/Method/Results/Discussion/Conclusion）在输入中均为空字符串，PDF 语义检索证据为空，字段证据映射也为空。因此以下总结的「论证主线、技术主线、实验主线」只能建立在摘要之上：凡摘要直接支持的，直接陈述；凡需要展开机制的，明确标注为合理推断；凡无任何证据的（具体数值、数据集、baseline、公式），明确标为缺口，不做填补。元数据标注的 arXiv 编号 2609 与 2026 年 9 月的日期自洽，属于 v1 预印本；作者机构信息未在元数据中给出，故 institution 字段返回空字符串。

【论证主线（摘要直接支持）】
论文的论证结构是「诊断—范式—架构—实例化—验证—展望」。诊断部分指出传统数据系统在 AI 时代存在三重深刻局限：依赖人工构造的流水线、缺乏对异构数据的语义理解、以刚性且被动的方式运行。由此推出的结论是：修补式改进不足以应对，需要新范式。作者提出的范式名为 Data Agent，其目标是让人工干预最小化，由智能体自主执行广泛的数据相关任务。范式层面的核心主张被凝练为三组转变：从人工设计到自主编排、从字面操作到语义解释、从被动响应到主动处理。这三组转变分别对应「谁来设计流程」「用什么语义层次操作数据」「系统何时动作」三个根本问题的重新回答，构成论文论证骨架中最有辨识度的部分。

【技术主线（组件名称属摘要直接支持，内部机制属推断）】
系统由六个组件构成：语义数据组织、语义算子、智能体化流水线编排与优化、反馈驱动精化、记忆管理、主动适应。从名称与三组转变的对应关系看（合理推断）：语义数据组织与语义算子支撑「字面操作→语义解释」，把异构数据整理为具语义信息的表示并提供超越字面匹配的算子原语；智能体化流水线编排与优化支撑「人工设计→自主编排」，由智能体完成意图分解、拓扑生成与执行优化；主...**

<!-- paperflow:4d68d4dae20d56b3 -->
## DolphinBench: Mapping the Pareto Frontier of Agent Memory

[[Deep Reading - Sept 2026/DolphinBench-Mapping the Pareto Frontier of Agent Memory|Deep Reading]]

[https://arxiv.org/pdf/2609.24971v2](https://arxiv.org/pdf/2609.24971v2)

- **一、论文要解决的问题
DolphinBench 是一篇以评测协议为核心贡献的工作，瞄准的是 agent 长期记忆评测的两个结构性缺陷。第一，现有记忆 benchmark 大多采用对话式问答格式，而问题本身就是一个检索线索：它告诉系统「这里需要回忆」，很多时候还暗示了需要回忆哪一类事实。真实 agent 场景中没有这种提示——agent 必须自己决定此刻是否需要历史、需要历史中的哪一部分、以及回忆到的内容是否足以支撑当前动作。这种「检索决策」环节在问答式评测中被整体删除了。第二，现有 benchmark 很少要求准确率之外的任何东西，于是记忆系统可以用不合理的成本与时间代价换取分数：无限扩张每轮重放的上下文、对每次查询做昂贵重排、把全部历史原文塞进 prompt。这类策略在排行榜上有效，在部署中不可行。作者由此提出第三条隐含缺失（由验证协议反推）：很多任务可能根本不需要历史也能完成，因此分数上升未必来自记忆能力提升。

二、技术主线：用三个支柱重构记忆评测
第一根支柱是评估面的替换：DolphinBench 不通过问答，而是通过 agent 的任务完成情况来直接评测记忆。记忆从「被提问的对象」变成「完成任务的前提条件」，检索线索泄漏问题被从结构上消除。
第二根支柱是记忆负载的构造：三个知识工作（knowledge-work）persona，每个 persona 拥有约 500k tokens 的用户消息历史。这个数量级超出了常见上下文窗口，迫使系统必须在检索、压缩或分层管理之间做取舍，而不是简单地全量塞入。任务被设计为依赖这段历史中的信息。
第三根支柱是任务有效性的双向验证。每个 persona 有 200 个任务，三个 persona 合计约 600 个任务。作者对每个任务分别运行带相关历史与不带相关历史的 agent，要求带历史必须成功、不带历史必须失败。这个过滤条件把「记忆必要性」从一个假设变成可观测的对照结果，形式上接近反事实对照，也相当于对任务集做了一次消融筛选。
第四个（报告的）支柱是多目标评测：论文要求所有提交在准确率之外同时报告总成本与延迟。三者共同定义了...**

<!-- paperflow:93fd34f77500db11 -->
## OSWorld-Pro: Process-based Evaluation for Computer Use Agents

[[Deep Reading - Sept 2026/OSWorld-Pro-Process-based Evaluation for Computer Use Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.24890v1](https://arxiv.org/pdf/2609.24890v1)

- **1）论文的出发点。论文认为，当前 Computer-Use Agent（CUA）的评测范式存在结构性缺陷。以 OSWorld 为代表的主流做法，是在任务结束时（往往已经执行了数百步）检查 agent 产出的最终交付物，并用功能性验证器判定其是否满足规格。这种终态评测（end-state evaluation）在工程上是合理的选择——它自动化程度高、判据明确、不易受噪声干扰——但它丢失了关于「agent 如何失败、为何失败」的信息。作者用一组对照说明这一损失的严重性：一个在键盘输入环节出错的 agent，与一个无法在图形界面上精确完成基于点击的输入的 agent，在终态指标上完全不可区分，但两者需要的缓解策略截然不同。终态评测因此把一个本应具有诊断价值的问题，压缩成了一个不透明的通过/失败信号。

2）论文的解法。作者提出 OSWorld-Pro，一套面向过程式评测（procedural evaluation）的基准，其构成包括：300 多个任务、2800 多个子目标，以及支撑子目标定义的 67000 多条人类标注。相较于终态评测在每个任务上只产生一个判定点，OSWorld-Pro 把每个任务展开为约 9 个（粗略估算）顺序依赖的子目标，从而把一个稀疏的成败信号转化为一条密集的推进轨迹。判定工作由「稳健且与人类判断对齐的 LLM-Judge」完成——论文使用 LLM-Judge 而非功能性验证器，是因为中间状态（例如某窗口是否已打开、某元素是否已被选中）通常缺乏可靠的程序化判据，难以枚举。子目标之间的顺序依赖关系是该方法的核心结构假设：它使「agent 推进到了哪一步、卡在哪里」成为可观测对象。

3）论文的论证主线。三步：（a）终态评测不可归因，因而改进方向只能靠猜测；（b）过程式评测可以把失败定位到具体子目标，并通过动作类型切分识别出可操作的失败模式；（c）实证结果表明，过程口径下的分数显著低于终态口径，说明终态分数系统性地高估了 agent 的实际可靠性，且当前最强模型在过程稳健性上仍有明显缺口。整篇论文的定位更接近「评测协议升级 + 诊断性实证研究」，而非「新模型...**

<!-- paperflow:d6bb1134ad5a1dd3 -->
## Verifiable Hidden Dynamics Play: Generating Agentic RL Environments from Solved Mechanisms

[[Deep Reading - Sept 2026/Verifiable Hidden Dynamics Play-Generating Agentic RL Environments from Solved Mechanisms|Deep Reading]]

[https://arxiv.org/pdf/2609.27321](https://arxiv.org/pdf/2609.27321)

- **本文介绍了 VHD-Play，一种用于生成高质量、可验证智能体训练环境的创新框架。该研究的核心贡献在于提出了一种“先求解、后渲染”的逆向生成逻辑，解决了合成环境动力学不可靠和评估困难的顽疾。通过将严谨的数学模型转化为具有丰富语义的有状态工具，VHD-Play 能够以极低的成本（每环境数美分）大规模产出训练数据。实验结果令人印象深刻：在 Qwen3.6-35B 模型上的训练不仅显著提升了其在合成环境中的表现，更展现了跨领域的强大泛化性，在电商、旅行规划等真实感极强的基准测试中达到了 SOTA 水平。研究深入揭示了智能体能力的本质——即处理“有状态交互”和“隐藏动力学”的能力，而非单纯的逻辑推理。这一发现为未来构建更强大的通用智能体提供了明确的数据工程路径，即通过构建可验证的、具有隐藏动力学的模拟环境来弥补现实世界数据的不足。**

<!-- paperflow:a534b9ae51c825e2 -->
## Learning What to Activate: Combinatorial Capability Allocation for Long-Horizon Multimodal Agents

[[Deep Reading - Sept 2026/Learning What to Activate-Combinatorial Capability Allocation for Long-Horizon Multimodal Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.27869](https://arxiv.org/pdf/2609.27869)

- **【一、本次可获得的信息到底有多少】输入包含：标题、6 位作者名、arXiv 编号 2609.27869、日期 2026-09-24、来源标注 arxiv，以及一个自述抓取失败的 heuristic_draft。摘要为空，五个正文章节（introduction / method / results / discussion / conclusion）全部为空字符串，PDF 语义检索命中集合与字段证据映射均为空。因此，本报告不能复述论文的论证主线、技术主线与实验主线——这三条线在本轮输入中不存在。下面给出「基于标题的信息重建」「该问题在领域中的标准形态」与「必读核对清单」三部分，作为替代性阅读脚手架。

【二、基于标题的信息重建（推断，非原文）】标题 4 个语义单元构成一条完整的问题陈述：Learning What to Activate（可学习的激活决策）+ Combinatorial Capability Allocation（组合式能力分配）+ for Long-Horizon Multimodal Agents（面向长时程多模态智能体）。连起来读，论文大概率主张：在多步、跨模态的任务执行中，从显式的能力库中选择性激活一个子集，比全量激活或固定工具集更优，且这一选择应当被学习而非硬编码；由于子集空间组合爆炸、且激活决策会影响后续状态，学习过程需要处理预算约束与长时程信用分配。这个重建是自洽的，但它的每一环都可能在原文中被改写（例如「能力」可能是模型内部专家而非外部工具，「组合」可能指多能力并行编排而非子集选择）。

【三、该问题在领域中的标准形态（领域先验）】相关工作通常分布在四条线上：LLM 智能体框架与工具学习（ReAct、Toolformer、ToolLLM、Voyager 式技能库）；条件计算与路由（MoE、gating、离散采样）；组合优化与序列决策（背包、子模最大化、上下文老虎机、options）；长时程多模态评测（GUI/网页/具身基准）。一条可能的创新缝隙是：把「工具选择」从单步检索问题，提升为受预算约束、跨步骤、考虑能力协同的组合分配问题。这条缝隙是...**

<!-- paperflow:ac8efa274636060d -->
## Learn How to Act from Your Own Interactions: On-Policy Self-Distillation for GUI Agents

[[Deep Reading - Sept 2026/Learn How to Act from Your Own Interactions-On-Policy Self-Distillation for GUI Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.27307](https://arxiv.org/pdf/2609.27307)

- **【论证主线】论文的起点是一个已被验证有效的技术：on-policy self-distillation（OPSD）在 GUI grounding——GUI agent 的基础子任务——上表现良好，其成功机制被归因于 privilege-conditioned self-teacher 提供的稠密 token 级监督。接着论文提出一个观察性论断：把 OPSD 从 grounding 扩展到多轮 GUI 交互并不顺利，原因有两处——self-teacher 的 privilege-following 能力有限，以及特权指导（privileged guidance）供给不足。由此推出方法动机：要先把「教师是否会用特权」这件事解决，再把「怎么把特权转化为推理与记忆指导」这件事解决。这条论证链是摘要中明确给出的；其中对失败的归因属于作者的解释性论断，摘要层面未附消融证据。

【技术主线】GUI-SD-v2 的解法是两阶段训练框架，整体定位为 GUI-SD 的下一代版本。阶段一针对特权跟随能力：在同一 GUI 状态下，联合优化「带特权指导」与「不带特权指导」的两条 rollout，让模型学会在特权条件下与无特权条件下都正常行动，从而得到一个更可靠的特权教师。阶段二针对监督供给：由 privilege-conditioned self-teacher 选择性地蒸馏两类指导——step-specific reasoning guidance 用于支撑当前步的动作决策，memory guidance 用于在后续交互中保留任务相关信息。与 v1 相比，覆盖范围从单步 grounding 扩到多轮交互，并在 OPSD 之前增加了特权对齐的预热阶段，同时把蒸馏目标拆成推理与记忆两条流。摘要未披露损失函数、选择准则、超参数与特权信息的构造方式，这些是回原文时最需要确认的技术细节。

【实验主线】评测在两个 GUI agent 基准上进行：AndroidWorld 与 MobileWorld。对比对象包括既有 OPSD baseline 与作者所评估的 state-of-the-art 方法。报告...**

<!-- paperflow:a4529758a6e4d58f -->
## Multimodal Thinking with Renderable Programs

[[Deep Reading - Sept 2026/Multimodal Thinking with Renderable Programs|Deep Reading]]

[https://arxiv.org/pdf/2609.30130v1](https://arxiv.org/pdf/2609.30130v1)

- **本文提出了 SVGLM，这是一种旨在赋予视觉语言模型（VLM）“边画边想”能力的多模态推理框架。研究的核心动机在于弥补现有模型在复杂逻辑推理中缺乏视觉辅助手段的缺陷。作者创新性地选择了 SVG（可缩放矢量图形）作为推理媒介，利用其兼具代码可编程性和图像可渲染性的双重特质，构建了一个闭环的推理系统。

在技术实现上，作者通过构建大规模 SVG 编辑数据集，成功训练模型在推理链中嵌入“渲染程序”。这意味着模型在解决问题时，不再仅仅依赖抽象的文本 Token，而是可以自主生成精确的几何图形或函数图像，并通过“观察”这些图像来辅助后续决策。实验结果在数学推理等高难度任务上验证了该方法的有效性，显示出 SVG 在提升模型空间想象力和逻辑严密性方面的巨大潜力。

总的来说，SVGLM 不仅提供了一种高效的图文统一表征方案，更为构建具备人类水平绘图思考能力的数字智能体奠定了理论和技术基础。它证明了结构化代码（SVG）是连接人类离散思维与物理世界连续视觉信号的理想桥梁。**

<!-- paperflow:7dae57a813e72bb3 -->
## Where Does Exactly-Once Live? Model, Harness, and Tool-Contract Effects on Duplicate Side Effects in LLM Agents

[[Deep Reading - Sept 2026/Where Does Exactly-Once Live-Model, Harness, and Tool-Contract Effects on Duplicate Side Effects|Deep Reading]]

[https://arxiv.org/pdf/2609.29095v1](https://arxiv.org/pdf/2609.29095v1)

- **论文研究的问题是：当工具型 LLM agent 的写操作超时或返回服务端错误时，动作可能已经生效，此时重试会重复副作用（二次扣款、二次公告、二次部署），放弃则会漏做必要工作。作者把这个问题重新表述为一个责任分配问题——exactly-once 行为应当强制在模型（model）、agent harness，还是在工具契约（tool contract）层面？

为回答这一问题，作者构建了 LIMBO：一个确定性沙盒，包含六个服务，契约贴近现实（idempotency key 可选、读路径最终一致甚至缺失），在服务边界注入十二种故障模式，包括 late commit、redelivery、partial batch 等。每个 episode 的判定不依赖 agent 自报结果，而是与「已提交效果账本」比对，从而把真实副作用是否重复、是否漏做变成可测量量。

实验规模为 25,930 个 episode，覆盖九个近期模型、三个生产级 agent harness、两种契约变体、十五种恢复条件。主结论是条件依赖的：当立即读回可以揭示真实结果时，「模型说了算」——被告知必须以 exactly-once 行动的前沿模型几乎不会重复一个确认丢失的写（0.5%），弱模型则经常重复，模型解释了 53% 的可解释方差；当读回不可能时——请求仍在飞行中，或传输层重复投递——同一批前沿模型在 56% 与 74% 的 episode 中重复，此时契约解释 81%。作者用理论补上这一实证图景：证明在 late commit 且 in-flight 时间无上界的情况下，任何只做验证（verification-only）的策略都无法保证 exactly-once，因此这些重复不是「模型不够聪明」而是「信息在原理上不可得」。

在干预层面，论文对比两条路线。其一是等待：当 in-flight 时间有短且已知的上界时等待有效；但在重尾 in-flight 延迟下，即便每个 episode 等一小时，也不如「每次写都提供 idempotency key」。其二是契约改造：当 key 覆盖率提升后，重复率从 28...**

<!-- paperflow:06b6f68f4bb63b8b -->
## Scope Before You Persist: Preventing Cross-Family Interference in Agent Memory

[[Deep Reading - Sept 2026/Scope Before You Persist-Preventing Cross-Family Interference in Agent Memory|Deep Reading]]

[https://arxiv.org/pdf/2609.29144v1](https://arxiv.org/pdf/2609.29144v1)

- **本论文深入探讨了语言模型智能体在长周期、多任务流环境下的持久化记忆管理问题，针对“局部有效修改引发全局跨任务干扰”这一核心痛点，提出了创新的“范围匹配（Scope Matching）”理论框架。

在技术路线上，论文基于冻结权重的智能体架构，引入了基于执行结果验证的持久化技能修改门控机制（ORC）。传统的全局检索（Global-ORC）允许智能体无限制地召回所有已保存的技能，这导致在处理跨家族的异质任务时，局部有效的提示词或代码修改会产生严重的负迁移。实验表明，Global-ORC 的平均轨迹效用（0.713）甚至低于不具备记忆能力的静态智能体（0.775），且伴随着高比例的有害部署。为了解决这一问题，论文设计了 Scoped-ORC，将每个通过验证的技能修改的检索范围严格限制在其起源的任务家族内部，实现了“验证边界”与“检索边界”的精确对齐。

在实验验证方面，研究团队在包含12轮代码修复流的 ProcStream-RSI 基准上开展了系统性评估。干预实验结果显示，范围匹配将平均隐式轨迹效用从 0.713 提升至 0.816，并将有害部署清零。在 27 个配对随机顺序流的广泛测试中，Scoped-ORC 显著超越了 Global-ORC，不仅实现了 0.063 的效用净提升，更将接受的有效更新数量从 12 次大幅提升至 63 次（且无一例有害接受），在 19/27 的流中实现了多次连续自适应。本研究有力地证明了，范围匹配是构建可靠、可连续演进的持久化智能体记忆系统不可或缺的互补控制机制。**

<!-- paperflow:97c0caeda2ff9acb -->
## CodeGraph: Open-Taxonomy Knowledge Graph for Source Code with Wikidata Grounding

[[Deep Reading - Sept 2026/CodeGraph-Open-Taxonomy Knowledge Graph for Source Code with Wikidata Grounding|Deep Reading]]

[https://arxiv.org/pdf/2609.29474v1](https://arxiv.org/pdf/2609.29474v1)

- **### 论文论证与技术主线全景总结

本研究针对当前软件工程领域“无法有效提取和组织海量源代码中隐含的高阶工程知识”这一痛点，提出并实现了一套完整的、基于大语言模型与知识库对齐的开放分类语义标注流水线，并成功构建了世界上首个大规模源代码开放分类知识图谱 —— **CodeGraph**。

#### 1. 动机与问题定义
传统的代码分析工具（如基于 AST 的静态分析）只能理解代码的语法结构，无法理解代码“在宏观上实现了什么算法、服务于什么业务领域”。而直接使用 LLM 生成标签又面临缺乏标准、无法结构化推理的问题。因此，本研究旨在将 LLM 的“开放语义理解能力”与权威知识库 Wikidata 的“结构化规范性”相结合，打通从非结构化代码到结构化高阶知识的通道。

#### 2. 技术主线：三阶段对齐与智能体设计
论文的核心技术贡献在于其严密的流水线设计。首先利用代码专用 LLM 对 Stack-Edu 语料库中的 1.67 亿个文件进行开放式概念抽取。为了解决抽取出的 63,000 个概念的标准化问题，设计了三阶段对齐法：
* **第一阶段**利用高效的 SPARQL 确定性查询解决高频词；
* **第二阶段**引入**深度研究智能体（Deep Research Agent）**，这是本系统的一大亮点。该智能体能够针对长尾、有歧义的概念进行自主的上下文推理和多轮检索，极大地提升了实体链接的召回率和准确性；
* **第三阶段**通过层次上卷，将 Wikidata 中的父子关系引入图谱，构建了从“具体文件 $\rightarrow$ 具体算法 $\rightarrow$ 宏观学科/领域”的完整知识网络。

#### 3. 实验与质量控制主线
为了确保这套自动化流水线产出的图谱不是“充满幻觉的垃圾数据”，研究团队引入了校准的质量保证协议。通过专家标注的小规模黄金标准集来校准 LLM 裁判，从而在大规模数据上实现了高置信度的自动化质量过滤。最终，该流水线在 14 种语言的数据集上大获成功，产出了包含 1.58 亿节点、10 亿条边的 CodeGraph。这一工作不仅展示了...**

<!-- paperflow:6fec20e2aade44a6 -->
## MM-VeriAgent: Learning to Use Extensive Tools to Verify Multimodal Misinformation with Reinforcement Learning

[[Deep Reading - Sept 2026/MM-VeriAgent-Learning to Use Extensive Tools to Verify Multimodal Misinformation with Reinforcem|Deep Reading]]

[https://arxiv.org/pdf/2609.30698](https://arxiv.org/pdf/2609.30698)

- **本文的论证主线可拆为四段。

（1）问题设定。真实世界的多模态虚假信息往往不是单一伪造来源，而是混合来源：同一条图文样本可能同时叠加文本侧编造、图像侧篡改或生成、以及图文之间的跨模态不一致。作者由此提出核心判断——不存在对所有样本都最优的固定检测流程，检测策略必须是随样本而变的（sample-specific）。

（2）对现有路线的批评，也是本文的研究缺口。工具增强的检测方法大致分两类：一类依赖预定义工作流，所有样本走同一条固定流水线，鲁棒但缺乏自适应性；另一类在推理时做规划，由模型临场决定调用哪些工具，自适应但引入额外推理成本，且规划质量完全依赖基座模型的即时决策。论文要同时避开这两端。

（3）技术主线，由三个组件构成。第一，MM-VeriTools：作者先把混合来源检测拆成文本伪造分析、视觉伪造分析、跨模态伪造分析三类子任务，在每个子任务上对多个候选模型与方法做基准评测，选出最强方案，并封装为具有统一接口的可调用工具。这一步把「工具从哪来、凭什么选它」从直觉决策变成有评测依据的工程流程。第二，MM-VeriAgent：在工具集之上用强化学习训练一个 LVLM agent，让它学会在求解混合来源检测时调用哪些工具、以什么顺序调用，从而把原本发生在推理时的规划前移为训练时习得的策略（learned tool-use policy）。第三，Tool-Execution Cache：由于工具多为专用模型，若每次 rollout 都在线执行会严重限制 RL 效率；作者提出预先执行候选工具调用并缓存输出、训练时直接复用，从而在保留多步 rollout 结构的同时减少在线工具执行，显著提升训练效率。

（4）实验主线。在 MMFakeBench 上评测，相对基座模型取得大幅准确率提升，并且该提升是在推理阶段不做显式工具搜索的条件下取得的——这是「策略内化工具使用」这一核心主张最直接的结果证据。消融实验用于验证学到的工具使用策略确实在起作用（即增益来自策略学习而非工具集本身），效率分析用于验证 Tool-Execution Cache 确实减少了训练期的在线工具执行次数。

需...**

<!-- paperflow:00d04ff0df89d3bf -->
## Not All Memories Are Equal: Hierarchical Collaborative Memory for Validity-Aware Retrieval in LLM Agents

[[Deep Reading - Sept 2026/Not All Memories Are Equal-Hierarchical Collaborative Memory for Validity-Aware Retrieval in LLM|Deep Reading]]

[https://arxiv.org/pdf/2609.30289](https://arxiv.org/pdf/2609.30289)

- **一、论证主线
论文从一个具体而常见的失效现象出发：在多智能体协作里，记忆不是同质的，而是分成两层——团队记忆（集体决策、协议、当前共识）与个体记忆（成员观察、执行轨迹、中间进度）；而且这两层都在持续演化。现有 memory-augmented 系统却把全部记忆当作一个扁平池，用语义相关性、重要性或新近度排序后取 top-k，等价于默认“存下来的都是可用的”。作者指出这会导致两类错误召回：语义相关但已经过期的记忆，以及与当前团队共识冲突的个体记忆。问题在协作型 LLM agent 回答用户问题时被放大，因为回答本应 grounded in 当前有效记忆；更棘手的是，这类错误不像“检索为空”那样显而易见，而是会以貌似有据的方式进入答案。由此论文把“有效性”从排序特征提升为检索的前提条件，主张先维护有效性、再在有效集合上检索。

二、技术主线
HiCoMER 由三个组件串成流水线：(1) Hierarchical Memory Conflict Updater，在团队层与个体层之间识别并处理记忆冲突，维护记忆的有效状态；(2) Validity-Aware Memory Retriever，只在仍然有效的记忆上执行检索，而不是直接在全部存储记忆上排序；(3) Memory-Grounded Answer Generator，以检索到的有效记忆为依据生成答案。这一设计的关键取舍是：把冲突消解前移到写入/更新阶段并持续维护状态，而不是在回答时临时拼凑上下文；代价是需要额外的判定与状态管理开销（具体机制、是否采用 LLM 判定、更新如何跨层传播，摘要未给，需回原文核对）。

三、实验主线
作者构建了两个面向协作场景的 memory-grounded QA 数据集，并与强 baseline 对比。摘要给出的结论是：HiCoMER 在两个数据集上一致更优，具体表现为减少过期检索、保留当前团队共识、以及提升下游 QA 质量——即检索侧的改善能传导到生成侧。但摘要未提供数据集名称与规模、baseline 构成、任何指标数值、消融与成本数据（额外 LLM 调用与延迟），因此方法的新颖性与收益...**

<!-- paperflow:f4e54b2f611e9fc6 -->
## TemplateCraft: Agentic Visual Template Generation

[[Deep Reading - Sept 2026/TemplateCraft-Agentic Visual Template Generation|Deep Reading]]

[https://arxiv.org/pdf/2609.31451](https://arxiv.org/pdf/2609.31451)

- **## 一、论文要解决的问题
短视频内容消费的普及催生了对「一键创作」的需求。视觉模板是这一需求的核心载体：用户上传图片、套用预设效果，即可得到个性化内容。然而模板的**生产端**仍是人力密集的——创作者需要准备素材（图像、贴纸、滤镜、转场等），并把各类工具按正确顺序与参数编排成可执行的效果链。论文把这一落差定义为待解决的核心问题：如何把自然语言意图自动转换成「可复用、可在客户端执行」的视觉模板。**

<!-- paperflow:d15cc04a2cb8a19e -->
## ToolSearcher: Optimizing Tool Selection at Scale via Reinforcement Learning

[[Deep Reading - Sept 2026/ToolSearcher-Optimizing Tool Selection at Scale via Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2609.30906](https://arxiv.org/pdf/2609.30906)

- **一、问题定位。论文从一个能力落差出发：LLM 在自然语言处理上表现优异，却难以与外部环境交互；工具学习是把 LLM 拓展为可执行 agent 的路径，而工具选择是工具使用成功的前置条件。已有工作通常假设工具集较小或预先定义，因此大规模工具选择长期未被充分研究。真实工具仓库的工具数量庞大且类型多样，在上下文长度约束下，模型很难有效完成搜索、区分与组合三件事。作者据此把大规模工具选择界定为 agentic RL 的一个新挑战，并给出一个关键判断：面向知识库问答（KBQA）的现有 RL 方法不足以胜任，因为这些方法的奖励只对齐「答案正确」，而工具选择还必须考虑工具之间的兼容性。

二、方法主线。论文提出 ToolSearcher，一个用于有效多轮搜索与细粒度优化的 RL 框架，包含三项设计。其一是类别约束的工具区分（category-constrained tool discrimination），目标是提升模型区分功能相似工具的能力——合理推断其做法是在类别粒度上组织候选与难负例，使模型不仅要选对功能类别，还要在类别内部选对具体工具。其二是事件级搜索建模（event-level search modeling），在多轮搜索过程中显式优化对目标工具的发现，把「某一步搜索命中目标工具」当作可单独奖励的事件，而非只依赖轨迹末端的整体成败。其三是轨迹对齐的信用分配（trajectory-aligned credit allocation），为「搜索—选择」过程的不同阶段提供细粒度奖励信号，缓解长轨迹下的奖励稀疏与归因错配。三者的分工可概括为：类别约束解决「区分不开」，事件建模解决「搜不到」，信用分配解决「学不动」。共同前提是存在可多轮调用的搜索接口，且工具具备可用的类别信息。

三、实验主线。论文在大规模工具选择基准上做了实验，评测设置包含迭代搜索与复杂工具组合两类困难场景，对比对象是一组强基线，结论是 ToolSearcher 持续优于这些基线。摘要未披露数据集名称、工具库规模、baseline 列表、指标定义与任何数值，也未提到消融规模、训练算法与模型底座，因此实验的强度与可复...**

# Computer Vision

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Computer Vision
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, vision, reinforcement-learning
- 论文/报告：19 篇
- Blending Concepts: Benchmarking Visual Metaphor Generation in Text-to-Image Models
- Thinking in Pictures: A Systematic Benchmark for Reasoning-driven Image Generation
- VidaForge: Open Research Infrastructure for Video Pretraining Data Recipes
- VoT: Vision-of-Thought for Unified Multimodal Representation Alignment
- Reason Through the Latent! Making Latent Visual Reasoning Necessary
- VideoTok4D: A 4D-Aware Video Tokenizer for Compact World Representation
- ProactiveBench: Can Streaming Video Models Really Interact Like Humans?
- A Chosen Future Can Still Be Rewritten: Causal Writability in Video Models
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:a887b756fe05d02a -->
## Blending Concepts: Benchmarking Visual Metaphor Generation in Text-to-Image Models

[[Deep Reading - Sept 2026/Blending Concepts-Benchmarking Visual Metaphor Generation in Text-to-Image Models|Deep Reading]]

[https://arxiv.org/pdf/2609.02502v1](https://arxiv.org/pdf/2609.02502v1)

- **本文围绕视觉隐喻生成能力的评测问题展开。作者首先指出，T2I 模型已经能够高质量地渲染指定对象和属性，但对于视觉隐喻——即通过融合两个不同领域的概念元素来传达抽象意义的图像——缺少系统评估。现有评测多依赖小规模临时测试集和整体性指标（如 FID、CLIP similarity 或用户研究），无法对隐喻表达中的具体失败环节做归因诊断；此外，理解方向的隐喻基准虽有，生成方向的基准却是空白。为此，作者构造了 VMetaphor-Bench，据称是首个视觉隐喻生成评估基准。该基准包含 1,500 个从真实创意图像中精选的视觉隐喻，覆盖从简单到复杂的三个层次和十个类别，每个样本带两个不同具体度的 prompt，从而使研究人员既能测试模型从清晰描述中重构隐喻图像的能力，也能测试其从抽象概念出发自行构思视觉表现的能力。在评测方法上，论文提出混合评估框架，并置于 MLLM-as-judge 范式下：一组是包含 9,594 道多选题的问题集，把隐喻忠实性拆成四个可验证的层面——presence、structure type、meaning、element mapping；另一组是基于视觉感受的维度评分，考察隐喻有效性、隐喻逻辑和感知和谐。为使评分过程可扩展，作者使用多模态大语言模型（默认 Qwen3.5-27B）作为裁判。随后，论文在 11 个有代表性的 T2I 模型上执行了评估。主要结果显示出清晰的能力分层：专有模型（如 GPT Image 1.5 和 Nano Banana 2）整体优于开源模型，但即使在最强模型中，组合结构和跨域映射仍然是系统性短板。这一结果表明，视觉隐喻生成不只是像素级或属性级渲染的延伸，而是一个需要更高阶语义规划的任务，构成未来 T2I 研究的重要前沿。综合来看，本文的贡献既包括大规模、层次化的评测资源，也包括一套可诊断生成失败模式的多层次评测协议；它还通过显式区分意义层、结构层和视觉层，推动了 T2I 评测从“整体打分”走向“分维度归因”。由于我们只获取了摘要和部分结论性片段，许多细节，如类别的确切定义、三个层级的具体划分、人类评分的一致性、模型的完整结果表以...**

<!-- paperflow:61eb12ab583e3589 -->
## Thinking in Pictures: A Systematic Benchmark for Reasoning-driven Image Generation

[[Deep Reading - Sept 2026/Thinking in Pictures-A Systematic Benchmark for Reasoning-driven Image Generation|Deep Reading]]

[https://arxiv.org/pdf/2609.02864v1](https://arxiv.org/pdf/2609.02864v1)

- **这篇论文的核心贡献是提出了"推理驱动的图像生成"（Reasoning-driven Image Generation, RIG）这一评测问题——不是让模型按照一段描述生成好看的图像，而是让模型先看一组视觉证据，归纳出隐藏规则，然后把该规则应用于新输入，产出一张在全局逻辑上自洽的图像。作者首先指出现状：统一生成模型（UGM）与世界模拟器在视觉感知与合成上有惊人的表现，但这些能力建立在"表面层事件对齐"上，高级视觉推理没有被评测、更没有被激励。为了让"规则必须被遵守且只能以视觉方式兑现"，论文设计了一个新的评测范式 RIG-BENCH，把核心范式从"指令跟随"转移为"规则归纳"。RIG-BENCH 的组织结构是四大认知域：概念推理（Concept-based）、变换推理（Transformation-based）、模式与结构推理（Pattern & Structure）以及场景推理（Scenario-based）。其中，变换推理定义为"对示范过的几何、属性或规则级变换应用到新输入上"；场景推理类任务（合理推断）包含模拟物理场景与故事情境，检索证据中的迷宫例子及"要求视觉答案比基于文本的 VQA 更严格的压力测试"表述都与场景域任务形态相关。数据集总量为 2000 个样本，并已在 HuggingFace 开放。评测方法上，论文选取了当前最新的专有模型与开源图像/video 生成模型作为被试（已有名字的包括 GPT Image 1、Gemini 3 Pro Image Preview 与 Qwen-Image），并以复合 100 分制进行汇总。最核心的实验结果是主结果中展示的能力落差：最强专有模型 Gemini 3 Pro Image Preview 达到 64.6，其余闭源模型集中在 40–60 分，开源模型全部分数更低；没有任何模型逼近饱和，而人类完成任务是可靠的。这被论文总结为"推理—生成差距"（reasoning-generation gap），其具体表现是：模型输出在局部（物体、纹理等）合理，却在整体（因果关系、空间拓扑、规则一致性）上不合逻辑。评测的讨论深化了这一点...**

<!-- paperflow:be50eea282383bf8 -->
## VidaForge: Open Research Infrastructure for Video Pretraining Data Recipes

[[Deep Reading - Sept 2026/VidaForge-Open Research Infrastructure for Video Pretraining Data Recipes|Deep Reading]]

[https://arxiv.org/pdf/2609.06652v1](https://arxiv.org/pdf/2609.06652v1)

- **本论文针对视频基础模型预训练中“数据流水线封闭、研究门槛高”的行业痛点，推出了开源研究基础设施 VIDAFORGE。该系统将视频数据配方标准化为由摄取、分割、筛选、标注、打包组成的五阶段可执行工作流，实现了样本级加工历史的完全可追溯与参数化定制。利用 VIDAFORGE，研究团队系统探究了数据覆盖度与质量在 Wan 2.1（生成式）和 V-JEPA 2.1（表示学习）早期预训练中的影响。实验揭示了广泛覆盖度配方在下游基准测试中的优越性，并指出了预训练损失与下游实际性能之间的评估错位。此外，项目开源了包含 314 万剪辑（6,475 小时）且富含策展信号的 VIDAFORGE-3M 数据集。VIDAFORGE 为开源社区提供了一条从原始视频到预训练实验的标准化、科学化路径，有望加速视频数据科学的发展。**

<!-- paperflow:2ad668148ac4d3ee -->
## VoT: Vision-of-Thought for Unified Multimodal Representation Alignment

[[Deep Reading - Sept 2026/VoT-Vision-of-Thought for Unified Multimodal Representation Alignment|Deep Reading]]

[https://arxiv.org/pdf/2609.07815v1](https://arxiv.org/pdf/2609.07815v1)

- **1. 论文要解决的问题
论文把矛头指向当前文生图的主流范式——「文本编码器 + 扩散解码器」。在这一范式中，文本语义被编码为静态条件向量（或 KV cache），直接调制连续的 latent noise。作者认为这种设计缺少一个显式、可解释的中间表示来桥接高层语言语义与低层视觉信号。检索到的 Introduction 片段给出了更锋利的表述：把 prompt 压成 static embeddings 或 KV caches，对简单描述尚可，但存在显著的 modality gap，大模型中丰富、结构化的世界知识无法被静态条件完整传递。换言之，VLM 被降格为文本编码器，是能力浪费；同时，连续条件不可读、不可局部干预，使生成过程缺乏可解释性与结构化可控性。

2. 技术主线：三段式“先规划、后渲染”
论文提出 Vision-of-Thought（VoT），在 VLM 与 diffusion transformer（DiT）之间插入一个离散的视觉思维层。信息流被显式拆成三段：（a）VLM 作为 multimodal planner，不再只输出一个条件向量，而是自回归地生成离散 VoT token 序列，代表对象、布局等高层视觉计划；（b）这些 token 构成结构化条件；（c）DiT 在这些 token 的条件下渲染像素。这一设计的核心取舍是：牺牲“端到端连续接口”的简洁性，换来可读、可编辑、可审计的中间表示。

3. 技术主线中的关键组件一：VLM-Aligned VoT Tokenizer
要让“离散视觉计划”同时被 VLM 和 DiT 接受，tokenizer 必须同时满足两个通常冲突的要求。论文的做法是把量化过程放到 VLM 的语义空间里做，并用一个闭环目标训练：VLM 对齐损失（使 token 对 VLM 语义可读，即 VLM 能生成、能读回）、特征重建损失（保证 token 仍携带生成所需的视觉信息）、向量量化损失（维持码本学习与离散化稳定性）。检索片段明确点出了与既有 tokenizer 的差异：传统 tokenizer 优先像素级保真，而 VoT Tokeni...**

<!-- paperflow:2b0341e33e828d11 -->
## Reason Through the Latent! Making Latent Visual Reasoning Necessary

[[Deep Reading - Sept 2026/Reason Through the Latent! Making Latent Visual Reasoning Necessary|Deep Reading]]

[https://arxiv.org/pdf/2609.06746v2](https://arxiv.org/pdf/2609.06746v2)

- **本文针对隐式视觉推理（Latent Visual Reasoning）中普遍存在的“伪推理”问题提出了深刻质疑，并给出了系统性的解决方案。作者指出，仅仅让模型生成包含视觉信息的隐状态是不够的，必须从架构上确保这些状态是预测的**必要条件**。

**技术主线**：
论文提出了 **CVRR（因果视觉循环推理）** 框架。其核心逻辑是在预训练 VLM 的基础上，插入一个循环处理单元。该单元通过多次“重读”视觉特征来更新问题的隐藏表示。最激进的设计在于，在最终生成答案前，CVRR 会主动销毁所有原始的图像特征和多模态 KV 缓存。这意味着，如果模型想要正确回答问题，它必须学会将所有关键的视觉线索编码进那个唯一的循环隐状态中。

**论证主线**：
作者通过严谨的对比实验证明，现有的隐式推理方法在失去“原始路径”支持时，视觉能力会迅速崩溃，而 CVRR 凭借其独特的设计能够维持强大的性能。此外，论文引入了因果干预实验，通过手动修改隐状态轨迹，证实了预测结果与隐式计算之间的直接因果联系。

**实验主线**：
在 $V^*$、MMVP 等多个极具挑战性的视觉基准上，CVRR 展示了其在受限接口下的鲁棒性。实验不仅关注准确率，还深入探讨了循环深度、信息瓶颈以及模型对视觉证据的敏感度。总的来说，这项工作为多模态大模型的隐式推理提供了新的范式，强调了“因果必要性”在构建可靠 AI 系统中的重要性。**

<!-- paperflow:c3de79a9ba280411 -->
## VideoTok4D: A 4D-Aware Video Tokenizer for Compact World Representation

[[Deep Reading - Sept 2026/VideoTok4D-A 4D-Aware Video Tokenizer for Compact World Representation|Deep Reading]]

[https://arxiv.org/pdf/2609.12874](https://arxiv.org/pdf/2609.12874)

- **本文提出了 VIDEOTok4D，这是一种旨在解决视频建模中“以观测为中心”偏差的创新 4D 感知视频 Tokenizer。作者指出，传统的视频 Tokenizer 将视频视为 2D 图像序列，忽略了其背后的 3D 物理世界，导致在处理动态新视角合成和高效存储时存在局限。VIDEOTok4D 的核心贡献在于其时空解耦架构，通过空间分支提取静态背景 Token，通过时间分支提取动态物体 Token。为了解决动态场景中的视角一致性问题，引入了轨迹感知动态注意力机制，利用运动轨迹对齐特征，补偿相机运动带来的干扰。此外，作者在这一紧凑的 Token 空间上构建了 Co4DGEN 扩散模型，实现了高效的 4D 场景生成。实验结果令人印象深刻：在动态新视角合成任务中，VIDEOTok4D 不仅达到了 SOTA 水平，还将存储开销从数百 MB 降低到了 KB 级别（压缩比提升 4 个数量级）。这一工作为构建高效、具备物理常识的 4D 世界模型奠定了基础，展示了从 2D 像素建模向 4D 场景建模转变的巨大潜力。**

<!-- paperflow:b9f62cec8c1a1147 -->
## ProactiveBench: Can Streaming Video Models Really Interact Like Humans?

[[Deep Reading - Sept 2026/ProactiveBench-Can Streaming Video Models Really Interact Like Humans|Deep Reading]]

[https://arxiv.org/pdf/2609.12658](https://arxiv.org/pdf/2609.12658)

- **本文提出了 ProactiveBench，这是一个旨在填补流式视频理解中“主动交互”评估空白的基准测试。研究的核心逻辑在于：真正的智能体不应只是被动地回答问题，而应学会在流式环境中自主决策响应时机。论文通过设计六个维度的子任务，系统地考察了模型在处理常驻请求时的响应精度、沉默质量以及对重复信息的抑制能力。实验结果揭示了一个重要的技术现状：当前的多模态模型普遍存在“早发响应”的缺陷，即在证据不足时过度触发，这反映了模型在时间因果性和决策耐心方面的不足。ProactiveBench 为未来开发更具类人交互特征的流式 AI 助手提供了标准化的评测框架和数据支持，强调了时间决策在多模态智能体研究中的核心地位。**

<!-- paperflow:05b04eeafff558f0 -->
## A Chosen Future Can Still Be Rewritten: Causal Writability in Video Models

[[Deep Reading - Sept 2026/A Chosen Future Can Still Be Rewritten-Causal Writability in Video Models|Deep Reading]]

[https://arxiv.org/pdf/2609.15980](https://arxiv.org/pdf/2609.15980)

- **本论文深入探讨了视频生成模型中物理真实性缺失的根源，提出了“因果可写性”（Causal Writability）这一创新视角。作者通过严谨的受控实验证明，当视频模型生成错误的物理运动时，这通常不是因为模型“无知”，而是因为模型在推理过程中由于“预测欠定性”选择了错误的预测规则。通过在模型隐藏状态中注入基于物理变量的低维编辑，研究者成功地在模型内部“重写”了未来，使模型恢复了正确的物理生成。研究的核心发现是“闭合边界”的存在：模型在计算的特定深度会对其生成的“未来”做出因果承诺，此后的干预将变得极其困难。此外，论文还揭示了可写性与训练动态之间的深刻联系，证明了早期的可写性是模型最终掌握物理规律的先兆。这项工作不仅为理解视频模型的内部机制提供了强有力的工具，也为开发更具物理一致性的生成AI开辟了新的路径。实验涵盖了从合成数据到 1.3B 参数预训练模型的广泛范围，确保了结论的稳健性和普适性。**

<!-- paperflow:efe90e6b2abd5dae -->
## LynnReal-Omni: Native multi-modal Video Generation for Agentic Visual Workflows

[[Deep Reading - Sept 2026/LynnReal-Omni-Native multi-modal Video Generation for Agentic Visual Workflows|Deep Reading]]

[https://arxiv.org/pdf/2609.15863](https://arxiv.org/pdf/2609.15863)

- **LynnReal-Omni 是一项具有前瞻性的系统性研究，它将视频生成技术从单纯的“内容创作工具”推向了“智能体视觉引擎”的新高度。论文的核心逻辑构建在三个支柱之上：原生多模态带来的**深度可控性**、Flash 变体带来的**极致实时性**，以及分块生成机制带来的**长序列持久性**。

在技术实现上，LynnReal-Omni 并没有盲目追求参数量的堆叠，而是通过精细的数据流水线和多阶段蒸馏技术，在 27B 参数规模下实现了性能与效率的平衡。它敏锐地捕捉到了去噪步数减少后 VAE 解码器成为新瓶颈这一工程细节，并给出了有效的蒸馏解决方案。对于关注智能体（Agent）和实时生成技术的科研人员来说，该工作提供了一套完整的从数据准备到模型压缩、再到长视频推理的工业级参考范式。其 2026 年的时间戳和对 Qwen3 等未来模型的引用，预示了该技术在下一代多模态交互系统中的核心地位。**

<!-- paperflow:4943aeb86e34c9a7 -->
## Omni Demand Understanding: A Benchmark for Contextual User-Intent Inference in Multimodal Interaction

[[Deep Reading - Sept 2026/Omni Demand Understanding-A Benchmark for Contextual User-Intent Inference in Multimodal Interac|Deep Reading]]

[https://arxiv.org/pdf/2609.21392](https://arxiv.org/pdf/2609.21392)

- **论文主要内容总结

【论证主线】
论文从一个评价设计的批评出发：音频-视觉交互正在成为 AI 助手的重要接口，但现有的交互能力基准几乎都在衡量 response quality，即模型回答得好不好，而回避了一个更前置的问题——模型是否从复杂的多模态交互中正确推断出了用户真正想要什么。作者随后论证这个问题不是边缘情形，而是常态：真实需求在语音中往往欠指定，必须从视觉线索与对话历史中补齐；口语本身充满歧义、不流畅与自我修正；声学环境常常是嘈杂的。更棘手的是反向失败：形式像请求的语音可能根本不构成对助手的需求（对着别人说话、复述、朗读、背景媒体），模型若照单执行就产生 false trigger。因此作者主张，需求推断是一个被忽视但不可或缺的能力，需要一个专门的评测对象。

【技术主线：问题定义与数据构建】
作者将 Omni Demand Understanding（ODU）定义为独立的多模态上下文推断问题：给定一段交互流，模型必须（i）判断是否存在用户需求，（ii）结合多模态与对话上下文推断意图。这个定义的关键设计是把"不该响应"纳入正确答案空间，使误触发成为一等失败模式。评测沿五个维度组织，并同时覆盖单轮与多轮交互；五个维度的具体命名摘要未给出，需回原文核对（本次未获取正文）。

ODU-Bench 的构建由三个环节支撑：（1）challenge-driven taxonomy，先按"什么样的上下文推断最容易失败"建立挑战类型学，使数据覆盖长尾难点而非平均采样；（2）taxonomy-guided agentic video generation，由该分类体系引导的 agent 化流程生成视频交互，以可控方式获得规模与场景多样性；（3）human-recorded interactions，补充真人录制的交互，提供真实性与声学多样性的锚点。数据随后经过 media-grounded annotation（标注锚定在媒体本身，而非仅依赖文本转写，从而保证"关键信息确实来自非文本通道"）与 human verification（人工校验）。

【实验主线与关键发现】
作者在...**

<!-- paperflow:f95650c989bd1567 -->
## OmniVBench: A Benchmark and Large-Scale Dataset for Omni Reference-to-Video Generation

[[Deep Reading - Sept 2026/OmniVBench-A Benchmark and Large-Scale Dataset for Omni Reference-to-Video Generation|Deep Reading]]

[https://arxiv.org/pdf/2609.22069](https://arxiv.org/pdf/2609.22069)

- **（说明：本总结基于论文摘要与元数据重构，未获取 PDF 正文；所有非摘要直接支持的内容均已标注推断等级。）

一、论证主线。
论文的推理链条可以概括为“范式迁移 → 评测失灵 → 数据稀缺 → 同步交付基准与数据”。起点是 R2V 生成的技术演进：从早期单一类型的参考控制（用参考图控制主体），走向 omni R2V——同时接纳内容、运动、风格、结构、叙事等多类参考，并允许它们以组合形式共同作用于同一段生成视频。作者随即指出，评测体系没有跟上这一迁移：现有基准的测试用例在参考类型与参考组合上都偏窄，评测协议则停留在整体参考一致性（holistic reference consistency）这一粗粒度指标上。

论文对整体一致性的批评是全文的关键论证。它认为这种评分方式默认了“参考信息是整体不可分的”，因此无法回答三个更本质的问题：参考因子是否被忠实保留（preserved）、是否被正确解耦并绑定到正确的目标（disentangled and bound）、是否按指令被正确实现（realized）。这三个问题对应三类不同的失败：参考被忽略、参考用错对象、指令未遵循。整体一致性指标会把它们压成同一个分数，从而既不能诊断失败，也不能指导改进。论证的第三步转向训练侧：omni R2V 训练数据构造成本极高，公开可用资源稀缺，学术界缺乏在同一资源条件下比较方法的条件。

二、技术主线。
论文给出两个交付物，共享同一套能力分解视角。

OmniVBench 的评测结构是“任务族 × 细粒度任务 × checklist 层次”。任务侧，基准覆盖 7 个任务族、18 个细粒度任务，横跨内容、运动、风格、结构、叙事与多参考六类场景，把“参考类型”和“参考组合复杂度”两个维度同时展开。评测侧，引入 factor-grounded evaluation，为每个 case 配备定制的 checklist 项，共 12,172 条；每条核查项分别落在保留、解耦/绑定、指令实现三个层次上（三者与 checklist 的对应关系是论文的核心机制，具体每个层次由多少条目构成、如何加权，摘要未给出，需回...**

<!-- paperflow:db5b26bed6c730f8 -->
## RULER: Instance-aware Rubric Rewards for SVG Generation

[[Deep Reading - Sept 2026/RULER-Instance-aware Rubric Rewards for SVG Generation|Deep Reading]]

[https://arxiv.org/pdf/2609.25270v1](https://arxiv.org/pdf/2609.25270v1)

- **本文针对 SVG 生成任务中“评价难、优化难”的核心痛点，提出了 RULER 框架。该框架摒弃了传统的、与人类审美脱节的标量指标（如 CLIP），转而采用一种“实例感知评分表（Rubric）”作为强化学习的奖励来源。RULER 的核心逻辑是：利用 LLM 为每个生成任务定制细粒度的评价准则，再由 VLM 充当公正的评判员进行逐项打分。通过 GRPO 算法，模型在这些细粒度反馈的指导下不断优化其生成的 SVG 代码。实验结果令人振奋，RULER 不仅在多个基准测试中刷新了纪录，更重要的是，它证明了在缺乏真值数据的情况下，通过结构化的视觉反馈，可以引导模型生成既符合语义又具备美感的矢量图形。这一工作为开放式视觉代码生成任务提供了一套系统性的强化学习对齐方案。**

<!-- paperflow:e66ea2f20105f4d8 -->
## WorldCrafter: Consistent Video World Model with Implicit 3D-aware Memory

[[Deep Reading - Sept 2026/WorldCrafter-Consistent Video World Model with Implicit 3D-aware Memory|Deep Reading]]

[https://arxiv.org/pdf/2609.24984v1](https://arxiv.org/pdf/2609.24984v1)

- **研究定位与问题。WorldCrafter 属于「视频世界模型」这一支：目标是让用户（或一个 agent）能从单张图像或一段文本提示出发，在生成出的场景中持续移动相机、进行交互式探索。这类模型的核心难点不在单帧画质，而在「记性」——摘要明确指出，现有视频世界模型在长时程与跨视角条件下难以尊重此前的观测，也就是随时间推移和视角切换，场景会被重新想象成不一致的样子。

核心洞察。论文的关键判断是：历史多视角证据该被如何压缩，取决于「接下来要从哪里看」。因此压缩过程必须被目标视角条件化（让 requested viewpoint 塑造压缩方式），而不是把历史压成一个与查询无关的固定表示。由于视频生成器的 token 预算是有限的，这个「按需压缩」的机制直接决定了记忆能在多大程度上被保留下来。

技术路线。系统由三部分组成：视频生成器、记忆编码器、以及位姿条件的读出模块。三者在训练中联合优化。推理时，历史观测先经由记忆编码器与位姿条件读出，被整合为一组固定数量的、目标视角专属的 token；这些 token 与近期时序上下文一起，在去噪过程开始前注入生成器。值得强调的是，整个流程不依赖显式的、基于深度的对应关系（no explicit depth-based correspondences），也就是说跨视角的信息整合是隐式、可学习的，而不是靠深度反投影或显式几何重建。配合 few-step distillation，模型可以在少步采样下运行，从而支撑流式的、可交互的探索体验。

训练与推理接口。训练上是记忆模块与生成器的联合训练（joint training），意味着记忆的表示空间会为生成目标而塑形，而非外挂式缓存。推理上支持两种起点：单张图像（给定场景先验）与文本提示（纯生成起点）；输出是可持续扩展的探索轨迹。摘要没有给出训练数据规模、损失构成、是否分阶段训练、推理延迟等关键工程细节，这些都需要回原文核实。

实验与结论。论文在静态场景与动态场景两类设置上评测，关注的三个维度是：长时程一致性、相机控制精度、视觉质量；探索时间尺度达到分钟级。摘要给出的结论是：一致性与相机控制精度...**

<!-- paperflow:fd57044ffe2937ca -->
## Visual Jev: Accurate and Efficient Decisions from Shared Visual Context

[[Deep Reading - Sept 2026/Visual Jev-Accurate and Efficient Decisions from Shared Visual Context|Deep Reading]]

[https://arxiv.org/pdf/2609.25845v1](https://arxiv.org/pdf/2609.25845v1)

- **1. 论文的出发点是工作负载而非模型能力
Visual Jev 关注的不是“如何让视觉语言模型更聪明”，而是一类具体且高频的推理形态：同一张图像上并行存在若干彼此独立的强制选择题。现实系统（评测脚本、标注流水线、属性/关系判断、内容审核）往往逐题调用模型，导致同一图像的视觉编码被重复 N 次；同时，多题并行又容易引入串题与上下文泄漏。论文正是在“准确率不能降、时间要摊薄”的双约束下给出设计。

2. 技术主线：三段式结构
第一段是共享编码：图像与公共上下文（所有题目共用的指令、格式说明、图像 token）只前向一次，得到可复用的前缀表示与 KV。第二段是隔离批执行：每题各自的问题后缀作为一批并行执行，并在注意力层面相互隔离，以保持与逐题串行等价的语义（“isolated question suffixes”，其掩码与位置编码细节摘要未展开，属需回原文核实的实现要点）。第三段是沿用式读出：候选答案的概率直接取自 backbone 的语言模型 head，不引入任务专用读出参数。三段合起来构成论文的核心主张——把质量交给 backbone 适配，把读出留给既有 head，把效率交给共享执行。

3. 质量主线：答案监督后训练，且增益是有边界的
论文用答案监督后训练提升准确率，在四个基准上把等权宏平均从 70.6% 提到 76.1%。更关键的信息是论文主动标注了收益的分布：提升集中在训练数据覆盖的两个任务族上。这意味着论文并未声称普遍泛化，而是给出一个可检验的边界条件——预测质量随监督覆盖而变化。配套的机制性对照是一个配置对齐的 typed head，它与语言模型 head 读出相比没有一致的准确率优势，从而把“收益来自读出结构改造”这一替代解释排除掉。

4. 效率主线：共享前缀 vs 共享执行的可分解收益
在每图 N=32 题的设定下，共享批处理相对独立串行执行的 warm 摊薄时间为 8.9×，相对“已批处理但仍重算前缀”的基线为 3.4×。两个数字的落差本身就是信息：通用批处理已贡献了大块收益，共享前缀是剩余的增量。代价被明确写出：峰值显存更高，说明这是一个真实的资源交...**

<!-- paperflow:6588c189196e7a52 -->
## UVU: Improving Multimodal Understanding via Vision-Language Unified Autoregressive Paradigm

[[Deep Reading - Sept 2026/UVU-Improving Multimodal Understanding via Vision-Language Unified Autoregressive Paradigm|Deep Reading]]

[https://arxiv.org/pdf/2609.27915](https://arxiv.org/pdf/2609.27915)

- **【本节的证据边界】输入中 abstract 为空、sections 全部为空、retrieved_evidence 与 field_evidence_map 均为空对象，因此不存在可供复述的论证主线、技术主线与实验主线。以下内容分三部分：第一部分是元数据与标题可确定的事实；第二部分是标题所指向的研究定位与可能的技术路线（明确标注为推断）；第三部分是阅读原文时必须补齐的信息清单。本节不做任何数值、结论或模块名称的虚构。

一、可确定的事实。论文标题为 UVU: Improving Multimodal Understanding via Vision-Language Unified Autoregressive Paradigm，作者共 13 人，依次为 Zhehan Kan、Xinghua Jiang、Yubo Zhu、Yanlin Liu、Xiaochen Yang、Zhixiang Wei、Shifeng Liu、Qingmin Liao、Wenming Yang、Xin Li、Yinsong Liu、Deqiang Jiang、Xing Sun。venue 字段标注为 arxiv，publish_date 为 2026-09-24，arXiv 编号 2609.27915，DOI 为空，元数据 institution 为 null。从作者规模与合作形态看，这是一项产学研协作的大规模工作（合理推断），但具体单位需核对论文首页署名与脚注。标题包含四个语义要素：视觉-语言统一、自回归、范式、以多模态理解为收益落点。

二、可能的研究定位与技术主线（推断，需核对）。标题暗示论文把一个方法论主张作为核心卖点：多模态理解的提升不应只靠更大的视觉编码器或更多对齐数据，而应通过把视觉与语言放进同一个自回归建模空间来实现。若这一推断成立，论文的技术主线大致会包含：视觉信号到离散 token 的转换方案；视觉与文本 token 在同一序列中的组织方式（含任务前缀、模态指示、分隔符等）；共享参数与共享词表的范围；联合训练中理解与生成目标的配比与课程安排；以及针对「统一是否会损害理解」这...**

<!-- paperflow:feb9f1e037ce6c14 -->
## VIVAS: Vitalizing Visual Perception in VLM Pre-training via Vision-language Unified Autoregressive Supervision

[[Deep Reading - Sept 2026/VIVAS-Vitalizing Visual Perception in VLM Pre-training via Vision-language Unified Autoregressiv|Deep Reading]]

[https://arxiv.org/pdf/2609.27948](https://arxiv.org/pdf/2609.27948)

- **本节无法基于原文撰写，必须先说明数据状态：abstract、introduction、method、results、discussion、conclusion 六个字段全部为空字符串，retrieved_evidence 与 field_evidence_map 为空对象，元数据仅提供标题、作者列表、arxiv 编号 2609.27948 与日期 2026-09-24。因此本字段不提供任何关于论文内容的复述，而是给出「论证骨架复原清单」——即根据标题，这篇论文内部必须存在、读者应逐项去原文核对的论证段落。

一、论证主线（需逐段验证）
1. 现状判断段：作者需要论证 VLM 预训练的主流做法（对比式图文对齐为主，captioning 或指令微调为辅）在视觉侧监督密度不足，导致细粒度感知长期落后于语义能力。标题中的 vitalizing 一词预设了「视觉感知被闲置/退化」这一诊断。
2. 因果假设段：视觉弱不是因为数据不够，而是监督信号的形式不对——图像在对比目标中只贡献一个全局向量，在描述生成目标中只贡献文本已覆盖的区域。
3. 解法主张段：把视觉内容本身也纳入自回归预测目标，强制模型逐 token 预测视觉，从而在参数中保留细粒度信息。
4. 验证策略段：必须用能区分「语义对齐提升」与「感知提升」的评测组合来支撑主张，并排除数据量、分辨率、基座规模等混淆因素。

二、技术主线（需核对的具体变量）
视觉 token 化方案（离散码本/连续特征/混合）与码本规模；统一自回归目标的具体形态（词表是否共享、解码头是否共享、位置编码是否区分模态、图像 token 排列与预测顺序）；损失构成与权重（是否保留对比/captioning/重建辅助项）；训练阶段划分与数据混合比例（纯图文对 vs 交错图文语料）；计算成本控制手段。其中「同数据、换目标」的受控消融是否存在，是判断整篇论文主张可信度的分水岭：若缺失，感知增益无法归因于监督形式。

三、实验主线（需核对的报告结构）
至少应包含四条彼此分离的评测线——感知类（OCR、文档/图表、计数、空间关系）、语义与知识类、生成类、纯语言类...**

<!-- paperflow:840495679549f6ca -->
## InternW0: A Foundational Physical World Model for Efficient Real-World Interactions

[[Deep Reading - Sept 2026/InternW0-A Foundational Physical World Model for Efficient Real-World Interactions|Deep Reading]]

[https://arxiv.org/pdf/2609.27656](https://arxiv.org/pdf/2609.27656)

- **【证据范围声明：本次未获取 PDF 正文与任何段落级检索证据，以下总结严格基于论文题录与摘要，外加显式标注的推断；不含任何未披露的数值、基线或数据集细节。另外，题录元数据存在时序异常（arXiv 编号 2609.27656 前缀“2609”对应 2026 年 9 月，与给出的发布日期 2026-09-24 自洽但与当前时间不符），该论文的真实性与最新版本状态建议核对，本总结的结论以后续核实的原文为准。】

一、论证主线（问题—主张—定位）
论文的论证起点是一个对“物理智能”的重新定义：Physical intelligence requires more than predicting how the world may evolve: predictions must remain actionable as the world continues to change。也就是说，世界模型的评价标准不是“预测是否逼真”，而是“预测是否能在世界持续变化、观测不完整、存在外部影响的情况下持续驱动有效动作”。围绕这一标准，作者提出 InternW0——上海人工智能实验室 InternW 物理世界模型系列的第一个实例，其设计三支柱为：omnimodal 接口、异步多频处理、以及部分观测与外部影响下的局部物理建模。最后作者把成果定位为“推进可扩展、异步、科学原生的物理世界模型，面向通用且高效的真实世界交互”，其中 science-native 是一个明确的价值主张：科学实验场景不是演示，而是模型能力的检验场。

二、技术主线（架构—机制—训练—适配）
1. 双专家非对称架构：InternW0 通过 asymmetric video–action architecture 联合学习未来视觉动态与连续机器人控制。视频专家容量大、负责更长时程的预测上下文；动作专家轻量、运行在更快的时间尺度上。二者用 flow matching 建模（具体目标构造需原文核实）。
2. 复用而非重算：核心效率设计是不为每次动作更新重新生成未来，而是复用层级 K/V 缓存（layerwise K/V），并通...**

<!-- paperflow:3b2de427882b1800 -->
## The Past Frames the Future: Memory for Autoregressive Video Generation

[[Deep Reading - Sept 2026/The Past Frames the Future-Memory for Autoregressive Video Generation|Deep Reading]]

[https://arxiv.org/pdf/2609.28466](https://arxiv.org/pdf/2609.28466)

- **本文针对自回归视频生成中的长时序一致性难题，提出了“记忆框架”这一核心解决方案。论文首先深入分析了自回归模型在长序列生成中必然面临的漂移问题，指出局部注意力窗口无法提供足够的全局约束。为此，作者设计了一套包含记忆编码、存储、检索和更新的完整闭环系统。技术上，该系统通过交叉注意力机制将历史关键特征注入当前生成流程，有效地在 AR 框架内实现了类似扩散模型的全局感知力。实验部分不仅在标准指标上证明了该方法的优越性，更通过定性分析展示了其在处理物体遮挡、背景稳定等复杂长视频任务中的独特优势。总的来说，这项工作为 AR 模型在视频生成领域的竞争力提升提供了重要的理论支持和工程实践参考，是通往超长视频生成目标的关键一步。**

<!-- paperflow:507d01140ceb4969 -->
## AV-GRPO: Modality-Anchored Decoupling Diffusion Reinforcement Learning for Joint Audio-Video Generation

[[Deep Reading - Sept 2026/AV-GRPO-Modality-Anchored Decoupling Diffusion Reinforcement Learning for Joint Audio-Video Gene|Deep Reading]]

[https://arxiv.org/pdf/2609.29816v1](https://arxiv.org/pdf/2609.29816v1)

- **一、问题与动机
论文处理的是联合音视频生成（joint audio-video generation）中的后训练问题。作者首先承认该领域近年进展显著，但指出既有模型在三方面仍不够：单模态保真度有限、文本与模态之间的语义对齐不足、跨模态同步性偏弱。这三者构成一个多目标、且可能相互冲突的优化目标集。强化学习后训练被视为有前景的补救路径——它能够绕开监督数据的上限，直接用奖励塑造模型行为。

二、为什么不能直接迁移 RL
论文指出直接套用 RL 到联合音视频生成会遇到三个结构性障碍。第一是奖励纠缠：音频与视频的异质奖励作用在同一条采样轨迹上，混合后学习信号难以区分，credit assignment 变得困难。第二是双塔联合优化的成本与动力学分歧：两个模态塔的动力学差异大，联合优化既昂贵又需要折中。第三是同步评测的样本依赖性：同一步骤的评测难度取决于被评的成对样本本身，使同步奖励在不同 rollout 之间不可比，优化信号因此失真。

三、方案：AV-GRPO + 5DAV
论文给出两个交付物。框架 AV-GRPO 是「模态锚定的在线扩散 RL 框架」，包含三个关键模块：(1) 模态锚定 rollout，用来解耦两个模态的学习信号并稳定难度；(2) 轨迹锁定的冻结塔优化，用来降低成本并重新分配 credit；(3) 自适应目标与扰动强度，按各模态自身动力学定制。数据集 5DAV 则是沿五个维度解耦样本、难度可控的训练集，用于系统性训练。两者的组合逻辑是：把耦合的多模态偏好学习转化为一系列单模态子问题，使奖励归因更精确，并由此获得更好的同步表现。值得注意的是，「解耦导致同步提升」在直觉上是反方向的——同步本是联合约束——因此其机制解释（是奖励稀释被消除，还是跨模态梯度干扰被阻断）是论文论证中最需要被验证的一环。

四、技术主线
整体流程可归纳为：以某一模态为锚生成 rollout → 冻结另一模态塔，在锁定轨迹上优化目标塔 → 按模态动力学调节目标函数与扰动强度 → 用更干净的信号更新策略。这条主线把「多目标耦合优化」拆成「分时单目标优化」，以牺牲部分联合搜索空间换取归因清晰度...**

# Machine Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Machine Learning
- 方法：vision, reinforcement-learning, deep-learning
- 论文/报告：5 篇
- Verify Before You Distill: Prompt-Level Teacher Gating for On-Policy Distillation
- You Are What You Read: Misalignment via In-Context Persona Induction
- DataFlex-RL: An Evaluation Platform for RLVR Data Policies
- EvoRS: On-Policy Self-Evolution of Reward Systems for Open-Ended Reinforcement Learning
- MOPD-Router: Rethinking Teacher Routing in Multi-Teacher On-Policy Distillation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:82b96756b4d37283 -->
## Verify Before You Distill: Prompt-Level Teacher Gating for On-Policy Distillation

[[Deep Reading - Sept 2026/Verify Before You Distill-Prompt-Level Teacher Gating for On-Policy Distillation|Deep Reading]]

[https://arxiv.org/pdf/2609.02998v1](https://arxiv.org/pdf/2609.02998v1)

- **We introduce Teacher-Gated On-Policy Distillation (TGOPD), built on the principle that teacher reliability should be verified at the prompt level before dense supervision is…

AllSpark Team On-policy distillation (OPD) accelerates post-training by providing dense token-level supervision from a frozen teacher on the student's own rollouts. Vanilla OPD applies this supervision uniformly across prompts, without checking whether the teacher is reliable for each prompt.

every other distillation method causes negative transfer on LiveCodeBench: the student scores below the untrained base model (OPD -0.8, TrOPD -2.5, RG-OPD -3.5, RLSD-style -4.1). Yet TGOPD is the only method that achieves positive transfer (+3.0 over base) and, in fact, surpasses the teacher itself (+1.3 LCB, +1.1 OJBench).

TGOPD represents teacher reliability as a per-prompt quantity and uses it to choose between two superv...**

<!-- paperflow:210397de0efd7c23 -->
## You Are What You Read: Misalignment via In-Context Persona Induction

[[Deep Reading - Sept 2026/You Are What You Read-Misalignment via In-Context Persona Induction|Deep Reading]]

[https://arxiv.org/pdf/2609.06851v1](https://arxiv.org/pdf/2609.06851v1)

- **本文的主线可以概括为一个命题的提出、验证与边界刻画：安全对齐的失效不一定要靠有害数据，也不一定要靠显式的不良行为示范——把一组良性、真实、逐条无害的传记事实放进上下文，就足以让模型变成一个特定人物，并随后在与其传记无关的问题上表达该人物的立场。作者把这一现象命名为 persona induction（人格诱导）。

论证主线。论文从一个已有的安全观察出发：此前已知两种产生广泛 misalignment 的路径。第一种是用窄域数据做微调，无论数据有害还是无害都可以产生广泛错位；第二种是在上下文中演示不良行为本身，例如有害建议，或取自 TruthfulQA（Lin, Hilton, and Evans 2022）的虚假陈述。在第二种路径里，上下文数据建模的正是那个不希望出现的行为。本文提出的问题是：如果上下文里放的不是不良行为，而是良性且事实正确的数据，而且这些数据在语义上收敛指向同一个人物，错位还会出现吗？作者的回答是肯定的。关键在于，这一过程既不需要微调、不需要权重更新，也不需要在提示中出现任何有害示范。危害性因此不是被「告知」的，而是被「聚合」出来的。

技术主线。方法上，作者刻意保持构造的朴素：把收敛到单一人物（横跨有害与无害、历史与虚构两类维度）的传记事实，以普通对话轮次的形式写入上下文，而不是写成指令或角色设定；用事实条数 k 作为唯一扫描变量；把「身份采纳」与「错位」作为两个独立度量分别测量。实验覆盖九个人物和十三个模型。身份采纳率随 k 呈 sigmoid 曲线上升，并在 k 为 3 到 10 条之间时越过 50%，说明触发门槛很低。在采纳之后，错位程度取决于被描述的到底是哪个人物：无害人物可以达到接近完全的身份采纳而错位接近零；有害人物则会在传记事实从未触及的问题上表达其标志性立场，比率最高达 80%。作者还发现，一条格式化指令可以门控人格何时被激活，说明激活是一个可条件化的开关，而不是不可逆的状态坍缩。

实验主线与关键发现。第一组实验给出采纳曲线与门槛区间（3 到 10 条事实越过 50%）。第二组实验给出采纳与错位的解耦，这是全文最反直觉的经验结果：...**

<!-- paperflow:478a0680788bac9e -->
## DataFlex-RL: An Evaluation Platform for RLVR Data Policies

[[Deep Reading - Sept 2026/DataFlex-RL-An Evaluation Platform for RLVR Data Policies|Deep Reading]]

[https://arxiv.org/pdf/2609.06107v1](https://arxiv.org/pdf/2609.06107v1)

- **一、论证主线
DataFlex-RL 的论证起点是一个方法论质疑：RLVR 领域近年来提出大量数据侧策略（rollout 筛选、样本重加权、领域混合课程），但如果它们被放进同一个训练配方、同样的模型与评测协议下，是否真的能带来超过均匀采样的稳定提升？论文把这个问题拆成三层同时检验：（1）方法层面——相对 uniform 是否有统计上可靠的改进；（2）跨模型层面——结论在换 base 模型后是否稳定；（3）评测层面——结论在换评测摘要构成本身是否稳定。第三层是本文最锋利的一刀：它把“基准组合怎么选”从背景设置提升为与方法选择同等级别的决策变量。

二、技术主线
平台的核心设计是把数据策略按“改变的训练部位”三分（Selection：从当前更新中移除生成的回答或 prompt group；Reweighting：保留回答但改变其 loss 贡献；Mixture adaptation：改变哪些领域为未来 prompt 供题），并把 solve rate、reward、advantage、token probability 记录为诊断信号而非方法标签。实验控制非常严格：模型、语料、验证器、优化器、rollout 预算与评测协议全部固定，所有 run 由同一个共享 driver 驱动。主矩阵为 13 种配置 × 12 个匹配种子，在 Qwen2.5-7B-Base 上以 12 个数学/逻辑/科学基准、领域均衡平均准确率为主指标比较；随后在 Llama-3.1-8B-Base 上做修正 12 种子扩展，把额外方法对齐到与原始对照相同的分数尺度。评测敏感性实验则用两套摘要对同一批 run 重新打分：数学偏重的 6 基准摘要（5 个数学基准 + GPQA-Diamond，无逻辑基准）与领域均衡的 12 基准摘要。

三、实验主线与关键数字
1. 均匀采样 GRPO 相对未训练 checkpoint 提升 7.76 个百分点——说明训练 headroom 充足，负结果不是“学不动”导致的假阴性。
2. 8 种 rollout 选择/重加权方法中，没有任何一个相对均匀采样的配对 95% 置...**

<!-- paperflow:b8449b8936b7dbf2 -->
## EvoRS: On-Policy Self-Evolution of Reward Systems for Open-Ended Reinforcement Learning

[[Deep Reading - Sept 2026/EvoRS-On-Policy Self-Evolution of Reward Systems for Open-Ended Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2609.12459](https://arxiv.org/pdf/2609.12459)

- **本文提出了 EvoRS 框架，旨在解决开放式强化学习中静态奖励系统失效的顽疾。作者指出，策略与奖励之间存在一种“猫鼠游戏”：策略总是在寻找奖励函数的漏洞。为了应对这一挑战，EvoRS 将奖励系统定义为可动态演进的 Reward-DAG，并利用智能体设计器根据策略的实时表现进行在线诊断和结构优化。实验结果令人振奋，EvoRS 不仅在写作和角色扮演任务中显著提升了模型生成的最终质量，更重要的是，它展示了一种能够自我修正、自我进化的评价范式。这种范式有效地缓解了奖励作弊问题，并在策略提升的过程中始终保持了奖励信号的有效性和区分度。该研究为构建长期的、开放式的自主学习系统提供了重要的技术支撑，证明了“评价的进化”与“能力的进化”同等重要。**

<!-- paperflow:c07a15b480491f3c -->
## MOPD-Router: Rethinking Teacher Routing in Multi-Teacher On-Policy Distillation

[[Deep Reading - Sept 2026/MOPD-Router-Rethinking Teacher Routing in Multi-Teacher On-Policy Distillation|Deep Reading]]

[https://arxiv.org/pdf/2609.30837](https://arxiv.org/pdf/2609.30837)

- **本论文针对多教师在线策略蒸馏（MOPD）中传统“提示词级别硬路由”导致的领域标签依赖和互补信号浪费问题，提出了全新的 **MOPD-Router** 框架。该框架的核心创新在于将路由粒度从提示词级别细化到 **Token 级别**，并且实现了**免标签、免训练**的动态权重分配。

在技术路线上，论文引入了即插即用的度量接口，并重点推出了 **ExpertAlign** 算法。ExpertAlign 通过量化教师模型在当前 Token 的修正是否符合其后训练阶段获得的专业化特征，来动态决定其监督信号的权重。同时，论文还实现并对比了基于置信度的 Entropy 路由和基于师生差异的 Novelty 路由。

在实验验证阶段，论文在强对弱、同尺寸蒸馏以及有无领域标签的四种交叉场景下进行了系统评测。实验结果表明，ExpertAlign 在所有设置下均显著优于现有的均值聚合和硬路由基线。特别是在有标签数据下，不使用标签的 ExpertAlign 反超了使用标签的标准 MOPD，有力地证明了 Token 级别跨领域互补监督的存在和价值。该研究为大语言模型的能力融合与高效蒸馏提供了一种全新的范式。**

# AI for Education

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Education
- 方法：agent, ai-for-science, language, vision-language-model, vision, reinforcement-learning, vision-language, deep-learning
- 论文/报告：5 篇
- Mapping the Emerging Social Science of Large Language Models
- Elastic Horizon: Discovering the Effective Interaction Frontier in Agentic Reinforcement Learning
- MUSE: Benchmarking Large Vision-Language Models on Multi-Modal Understanding in Situated Education
- ProgramDistill: From Interactive Web Apps to Verifiable Reference-Guided SWE Tasks
- Self-Play Pretraining with Zero Data
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:34b694b2f5f7534a -->
## Mapping the Emerging Social Science of Large Language Models

[[Deep Reading - Sept 2026/Mapping the Emerging Social Science of Large Language Models|Deep Reading]]

[https://arxiv.org/pdf/2609.07598v1](https://arxiv.org/pdf/2609.07598v1)

- **论文处理的是一项元科学（meta-science）任务：为大语言模型的社会科学研究画一张经验可检验、操作可复现的地图。

问题与动机（摘要明确支持）：LLM 已从专用文本生成系统扩散到日常与制度场景，塑造沟通、学习、工作、创造与决策。相关研究在多学科、多发表渠道快速膨胀，但整体碎片化，缺少整合框架，研究者难以定位自身工作与相邻工作。Introduction 片段进一步给出一个关键的「新在何处」判断：LLM 的新颖性不在于任何单一前所未有的能力（早期技术同样能生成文本、辅助决策、模拟角色、协调计算智能体），而在于对开放式语言的整合——这为本领域作为一个独立研究对象提供了论证基础。

定义（第 3 节片段支持）：论文把「LLM 的社会科学」定义为三类现象的系统性研究——LLM 表达出的具有社会意义的行为；LLM 智能体之间的集体动力学；人机互动所产生的社会过程。该定义以「LLM 或基于 LLM 的智能体本身」为共同被解释对象。

方法与实验设计：论文采用双尺度语料。Study 1 使用 198 篇全文精读的策展语料（带作者全文分类）；Study 2 使用从五个文献数据库检索的 47,719 篇正式发表文献。分析管线为：句子嵌入 → K-means 聚类（领域级）→ 簇内 LDA（子类级）→ 作者分类与 LLM 分类作为对照 → 领域规模上使用结构主题模型。验证包括重采样稳定性、人机分类一致性、跨方法主题映射与领域归属一致性、以及各领域的主题质量分布与引用/渠道可见度差异。

主要发现：
(1) 三领域结构。LLM as Social Minds（可被社会性解读的模型行为）、LLM Societies（智能体互动的集体动力学）、LLM-Human Interactions（人如何感知、使用 LLM 并受其影响）。
(2) 13 个子类。簇内 LDA 给出 4/4/5 的细分，覆盖推理与心智理论、人格与偏见、政治与道德判断、策略性影响；行为博弈、集体智能、群体决策、大规模模拟；信任、支持、工作、创造、教育。
(3) 稳定性与效度。策展语料中三簇解在重采样下平均 ARI = 0....**

<!-- paperflow:77b50d13f97407ed -->
## Elastic Horizon: Discovering the Effective Interaction Frontier in Agentic Reinforcement Learning

[[Deep Reading - Sept 2026/Elastic Horizon-Discovering the Effective Interaction Frontier in Agentic Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2609.07247v1](https://arxiv.org/pdf/2609.07247v1)

- **1) 问题与背景：agentic RL 中，每个 episode 能进行多少次环境交互（interaction horizon）是决定 LLM 智能体在长程任务上表现的关键预算。扩大 horizon 有收益，因此主流做法采用 curriculum 式调度，逐步把 horizon 推大；这类方法确实优于固定 horizon。但作者指出，所有现有调度都是开环的：它们单调递增直到一个手工指定的上限，没有任何机制判断继续扩张是否仍然有效。代价是双向的——上限过高时额外交互只带来成本（成本随 horizon 线性增长）而无收益；上限过低时能力被预算压制。既有的经验证据（Liu et al. 2025 报告 ReAct 智能体在 100 次工具调用预算处饱和、无法利用更多预算；Shen et al. 2025 观察到固定长 horizon h = 30 的负面现象，片段截断）说明饱和真实存在，但止步于观察。
2) 核心假设：作者提出「有效交互前沿（effective interaction frontier）」假设——存在一个动态边界，越过它之后额外交互收益递减，而成本线性增长。与把饱和当作静态常数的做法不同，这里 frontier 是随训练动态移动的，因此必须在线估计。
3) 方法：Elastic Horizon 是一个闭环控制器，用智能体自身展示的行为来设定交互预算。具体地，它用成功轨迹长度的第 90 百分位数估计有效交互前沿 H*，并通过 EMA 平滑后驱动 horizon 的更新，无需任何人工 schedule。由于信号来自成功轨迹，控制器可以在两个方向上调节：能力提升、成功轨迹变长时扩张预算；证据显示预算已超出所需时收缩预算（对应 Limitations 中提到的 under-budgeted 修正与「双向分配」表述）。
4) 实验设计：在 AppWorld 与 BFCL 两个基准上，用 7B 与 14B 两档主干模型做验证。关键的第一组实验是固定 horizon 扫描，用来暴露饱和平台并定义「饱和带」这一参照系；随后从欠容量与过容量两种初始化出发检验闭环收敛性；另设机制...**

<!-- paperflow:beb081a0df9b57d5 -->
## MUSE: Benchmarking Large Vision-Language Models on Multi-Modal Understanding in Situated Education

[[Deep Reading - Sept 2026/MUSE-Benchmarking Large Vision-Language Models on Multi-Modal Understanding in Situated Educatio|Deep Reading]]

[https://arxiv.org/pdf/2609.19088](https://arxiv.org/pdf/2609.19088)

- **### 论文论证主线
本研究针对当前视觉语言模型（LVLM）在情境化教育（特别是语言与人文艺术教育）中缺乏针对性评测的问题，首次提出了一个名为 MUSE 的多模态艺术图像理解基准测试。论文遵循“现实痛点分析 -> 现有基准局限性论证 -> 新基准设计与构建 -> 多模型基准评测 -> 失败模式与挑战分析”的完整论证闭环。

### 技术与数据主线
MUSE 的核心技术贡献在于其解耦式的数据生成范式。通过将复杂的艺术图像标注分解为基础属性和背景知识的结构化录入，再借助大语言模型生成高质量的教育问答对，极大地降低了人工构建大规模高质量多模态基准的门槛。在数据源上，MUSE 刻意打破了西方中心主义的视角，引入了大量新加坡及东南亚的多元文化艺术图像，构建了包含视觉感知、语义情感、文化理解和组合推理四大维度、十二个子任务的评测矩阵。

### 实验与发现主线
通过对当前最前沿的开源和闭源多模态大模型进行全面评测，MUSE 揭示了现有模型在情境化教育应用中的核心短板。实验表明，尽管模型在基础视觉感知上表现良好，但在面对需要深层文化底蕴的“文化理解”任务和需要审美共情能力的“情感诠释”任务时表现欠佳。论文最后总结的常见失败模式（如文化幻觉、情感误读、推理断链），为下一代更具鲁棒性、更懂教育和文化的信任多模态模型的开发指明了方向。**

<!-- paperflow:78d6aec3d794ac1e -->
## ProgramDistill: From Interactive Web Apps to Verifiable Reference-Guided SWE Tasks

[[Deep Reading - Sept 2026/ProgramDistill-From Interactive Web Apps to Verifiable Reference-Guided SWE Tasks|Deep Reading]]

[https://arxiv.org/pdf/2609.18805](https://arxiv.org/pdf/2609.18805)

- **本文提出了 ProgramDistill，这是一个革命性的编码智能体基准测试框架，它将评估范式从“遵循文字指令”转向“参考运行软件”。通过创新的 mine-craft-patch 流水线，研究者能够从现有的 Web 应用中自动提取功能特征，并将其转化为可验证的编程任务。该基准测试不仅规模宏大（4,063 个任务），而且具备精细的难度控制机制（通过恢复深度调节）。实验结果揭示了即使是像 GPT-6 Astra 这样的顶级模型，在面对深层逻辑重建和累积工作流时仍面临巨大挑战。ProgramDistill 的出现为编码智能体的研发提供了一个更贴近真实开发场景、更具诊断价值的评估平台，对于推动智能体从简单的代码补全走向复杂的系统级开发具有重要意义。**

<!-- paperflow:c7170ccc90b506fc -->
## Self-Play Pretraining with Zero Data

[[Deep Reading - Sept 2026/Self-Play Pretraining with Zero Data|Deep Reading]]

[https://arxiv.org/pdf/2609.30063v1](https://arxiv.org/pdf/2609.30063v1)

- **这篇论文挑战了“预训练必须依赖人类数据”的传统范式，提出了一种名为“零数据自我博弈预训练”的新框架。该框架的核心思想是利用通用图灵机（UTM）作为无限的程序空间，通过生成器和学习器的对抗与协作来自动挖掘有价值的训练数据。生成器利用强化学习在程序空间中搜索，旨在产生能够最大化学习器知识获取的序列，从而构建出一套自发的、由易到难的课程。实验结果极具启发性：在完全不接触任何自然语言的情况下，该模型在自然语言基准测试上的表现随计算量稳步提升，并自发形成了处理复杂逻辑和上下文信息的能力。这一工作不仅为解决数据枯竭问题提供了新思路，也从计算理论的角度揭示了语言建模与通用计算结构之间的深层联系，是迈向自主学习通用人工智能的重要一步。**

# AI Research

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Research
- 方法：vision
- 论文/报告：2 篇
- The Emerging AI Paper-Review Arms Race: Adversarial Co-Evolution in Scholarly Publishing
- GitScholar: A Dataset for Predicting AI Research Impact from GitHub Engagement
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:9b01b043a428c179 -->
## The Emerging AI Paper-Review Arms Race: Adversarial Co-Evolution in Scholarly Publishing

[[Deep Reading - Sept 2026/The Emerging AI Paper-Review Arms Race-Adversarial Co-Evolution in Scholarly Publishing|Deep Reading]]

[https://arxiv.org/pdf/2609.07713v1](https://arxiv.org/pdf/2609.07713v1)

- **一、论文的定位与论证主线
该文是一篇面向学术出版生态的系统性综述与立场性综合，作者主张：生成式与智能体式AI正在同时改造科研的“生产端”和“评价端”，但既有研究把二者当作两个独立问题——“AI如何做研究”与“AI如何审研究”。论文认为这一分离遗漏了出版生态的关键属性：一侧的变化会改变另一侧的激励、约束与行为。因此，全文以一条耦合的演化链替代并列主题式综述：更便宜更快的研究生产增加评价压力，AI介导的评价随之变得更可扩展、更可重复，参与者可以利用评价器的规律性，机构以技术防护与政策控制回应，而这些回应又诱发规避、重新分配错误与工作量，并塑造被未来研究与评价系统复用的学术记录。

二、方法论主线
1. 语料与框架：综合230篇学术出版物与机构记录，构建六动态描述性分类体系——（1）生产规模化；（2）评价自动化；（3）评价操纵；（4）防御机制与政策响应；（5）规避与副作用；（6）长周期生态系统反馈。
2. 分析单元：由“单个系统能做什么、输出如何在流程中流动”，转为“一个行动者的AI使用如何改变另一个行动者的信号、成本与可行响应”。这是Introduction片段明确提出的方法论替换。
3. 范围界定：Section 2.1把scope从研究生产（idea development、experimentation、manuscript preparation、revision、rebuttal）延伸至同行评审与出版，并用表格列出行动者与对应动态。
4. 章节推进：Section 3处理AI使能的生产规模化及其对评价的压力；Section 4处理评价自动化；Sections 5–7最直接处理战略互动与适应性响应，顺序为“AI介导评价的操纵→机构防御→规避与副作用”；Section 8处理更长时段的递归效应；Section 9给出跨领域发现与研究议程；Section 11为结论，重申AI介导的学术出版是“连接的过程，而非一组孤立工具”。
5. 证据分层：论文对六个动态标注证据强度，强证据为规模化生产与评价、可复现操纵、机构响应；弱证据为政策后适应行为与制品级长周期反馈。这种“带强度...**

<!-- paperflow:efed7183de8ad0a3 -->
## GitScholar: A Dataset for Predicting AI Research Impact from GitHub Engagement

[[Deep Reading - Sept 2026/GitScholar-A Dataset for Predicting AI Research Impact from GitHub Engagement|Deep Reading]]

[https://arxiv.org/pdf/2609.26361v1](https://arxiv.org/pdf/2609.26361v1)

- **【1. 问题背景与动机】
论文从一个经验观察到的问题出发：AI 研究推进速度极快，每天有数百篇新论文出现，研究者要跟踪最新进展已变得困难，但快速识别有影响力的工作对研究者的选题判断、阅读取舍与协作决策至关重要，逐篇人工审读在体量上不可行，因此需要自动化的影响力预测。已有方法通常综合论文内容、引用历史等多种可用信息，但这两类主流信号各自有结构性缺陷：内容类信号在发表时即可获得，及时但解释力有限；引用类信号预测力强，却需要数月到数年才能积累到可用水平，天然滞后。论文提出第三条通道——GitHub 互动，主张它同时具备及时性与准确性。

【2. 核心贡献与资源】
为验证这一主张，作者构建 GitScholar，一个把 GitHub 活动与学术论文大规模链接的数据集：来自 444,000 个 GitHub 仓库的活动被链接到 558,000 余篇 AI 领域 arXiv 论文。数据集公开托管在 Hugging Face（huawei-csl/GitScholar），属于可下载、可复用的资源型贡献。论文同时给出三组证据来支撑其论点：预测实验、覆盖率分析和相关性分析。

【3. 技术主线】
技术路线的骨架是“建链接 → 提信号 → 做增量评估”。
第一步是资源构建：把每篇 AI arXiv 论文与其对应的 GitHub 仓库对齐。摘要未披露链接算法（推测可能混合 arXiv ID 抽取、README/论文中的仓库 URL 匹配、第三方索引与名称模糊匹配等），也未说明是一对一还是多对多关系、是否包含第三方复现仓库；这些细节直接决定数据集的语义与噪声水平。
第二步是信号特征化：将仓库侧的互动行为（论文称 GitHub engagement / reactions）转化为可用的早期特征。摘要未给出特征清单，合理推断包含 star、fork、issue、PR、贡献者、活跃度及其随时间的增长速率等，并可能需要按仓库年龄做归一化。
第三步是任务与评估：把问题构造成早期的学术影响力预测（摘要中出现“early prediction precision”），以学术侧信息构成的强 baseline...**

# AI for Science

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science
- 方法：agent, ai-for-science, language, deep-learning, protein-language-model
- 论文/报告：5 篇
- SimpleDesign: A Joint Model for Protein Sequence and Structure Codesign
- LabAgent: Customize Any Research Hubs for Scientific Discoveries Using AI Agents
- PFArena: Benchmarking Language Models for Protein Modification
- Qwen-Planner-Agent: A Closed-Loop AI-for-AI Framework for Real-World Mobile Planner Agents
- Learning to Ideate for Scientific Impact
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:e2dfe9b27061a725 -->
## SimpleDesign: A Joint Model for Protein Sequence and Structure Codesign

[[Deep Reading - Sept 2026/SimpleDesign-A Joint Model for Protein Sequence and Structure Codesign|Deep Reading]]

[https://arxiv.org/pdf/2609.03377v1](https://arxiv.org/pdf/2609.03377v1)

- **SimpleDesign 提出了一种蛋白质序列与结构协同设计的新范式。该研究的核心贡献在于证明了**单阶段端到端训练在原始数据空间**的可行性与优越性。通过引入 Mixture-of-Transformer (MoT) 架构，模型成功解决了离散序列信息与连续空间坐标的联合表征问题。在 200 万规模的数据集支撑下，SimpleDesign 不仅简化了以往复杂的潜在空间建模流程，还在协同设计、无条件生成等关键任务上达到了 SOTA 水平。该工作为蛋白质工程提供了一个更直接、更强大的生成底座，展示了大规模 Transformer 在处理生物大分子多模态数据时的巨大潜力。**

<!-- paperflow:4dc35a7e1a9d723c -->
## LabAgent: Customize Any Research Hubs for Scientific Discoveries Using AI Agents

[[Deep Reading - Sept 2026/LabAgent-Customize Any Research Hubs for Scientific Discoveries Using AI Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.13437](https://arxiv.org/pdf/2609.13437)

- **### 论文论证与技术主线总结
本文针对生命科学领域计算方法复现困难、现有 LLM 智能体缺乏通用性且难以有效整合异构实验室知识的痛点，提出了一个名为 **LabAgent** 的全新 AI 智能体框架。该框架的核心目标是允许科研人员为任何特定的科学发现任务定制专属的研究枢纽（Research Hubs）。

在**技术实现**上，LabAgent 采用了创新的“自主技能发现”机制。系统由高级研究主管智能体（Lead Research Supervisor）主导，利用 `ConductScout` 等工具将复杂的文献检索与技术调研任务分发给子智能体。随后，通过 Per-Slot 搜索主管将多源检索结果合成为一份结构化的 `SKILL.md` 导航文档。这一文档成为了下层执行智能体调用工具、编写代码和运行实验的“高维操作手册”，从而实现了高度内聚且不依赖外部不确定网络检索的闭环执行。

在**实验验证**上，研究团队在耶鲁大学高性能计算中心（Yale HPC）的支持下，将 LabAgent 部署于四大核心生命科学领域：药物属性预测、生物医学问题分析、蛋白质变异效应预测以及统计遗传学。实验结果表明，LabAgent 在所有测试领域中均击败了商业通用智能体。它不仅能够精准复现已发表论文中的科学图表，还能自主从公开数据中重建 Therapeutics Data Commons (TDC) 的基准排行榜。在缺乏标准协议的基因组和单细胞分析任务中，LabAgent 同样表现出极强的鲁棒性，证明了其不仅能有效整合现有的实验室知识，还能对其进行合理的科学扩展，为自动化科学发现（AI for Science）提供了一个系统性的新范式。**

<!-- paperflow:097e2092f5af9833 -->
## PFArena: Benchmarking Language Models for Protein Modification

[[Deep Reading - Sept 2026/PFArena-Benchmarking Language Models for Protein Modification|Deep Reading]]

[https://arxiv.org/pdf/2609.28921v1](https://arxiv.org/pdf/2609.28921v1)

- **【一句话定位】
PFArena 是一个针对「语言模型用于蛋白质改造」的受控评测基准，其核心设计是把「实验室中已积累的目标蛋白适应度数据量」变成一个显式的实验变量，从而在同一框架下比较蛋白质语言模型、通用大语言模型和 LLM-based agent 三类范式的相对优势。

【问题主线】
蛋白质改造要在天文数字级的序列空间中寻找少数有用的突变组合，湿实验通量低、成本高，计算先验筛选不可或缺。论文指出，PLM、LLM 与 LLM-based agent 三条计算路线都已展示出潜力，但「在贴近真实实验决策流程的设定下它们彼此孰强孰弱」尚未被澄清。作者把这一空白归因于缺乏统一、可控、能反映不同先验实验信息量的评测协议，并由此提出基准化的解决路径。

【方案主线】
PFArena 的构造分三层：
1. 任务接口层——四个受控任务接口，覆盖两个任务族：单突变生成（开放式提议候选突变）与多突变排序（对既有多突变候选打分排序）。
2. 信息条件层——通过提供不同水平的突变适应度数据，把四个接口对应到四种代表性研究情境，从而刻画「先验实验背景由弱到强」的连续谱。
3. 评测层——纳入 17 个系统（6 PLM、6 LLM、5 agent），采用互补指标同时测量峰值表现（能否命中少数最优候选）与整体表现（对候选集合的整体排序/评分质量）；并以搜索空间规模与突变深度作为两条压力测试维度。作者同时开源代码与基准套件。
需要强调：以上结构由摘要直接给出；而任务接口的具体输入格式、数据来源、prompt 设计、agent 工具集与指标计算方式等实现细节，在当前可用证据中完全缺失，必须回原文核对。

【实验主线与关键发现】
实验以「模型族 × 任务接口 × 数据条件」的对照形式展开。核心发现有三条：
1. 模型优势随目标特异性实验证据的可得性而系统性迁移。PLM 依靠预训练中吸收的蛋白质先验，在几乎无目标蛋白数据时的开放式单突变生成中更强；LLM 与 agent 在多突变排序中更强，且在能获得目标特异性适应度数据时优势尤为突出。
2. 所有模型族在搜索空间增大时都面临根本性困难，说明候选规模扩大带来的...**

<!-- paperflow:f722a298414a0999 -->
## Qwen-Planner-Agent: A Closed-Loop AI-for-AI Framework for Real-World Mobile Planner Agents

[[Deep Reading - Sept 2026/Qwen-Planner-Agent-A Closed-Loop AI-for-AI Framework for Real-World Mobile Planner Agents|Deep Reading]]

[https://arxiv.org/pdf/2609.29892v1](https://arxiv.org/pdf/2609.29892v1)

- **论文的核心命题是 AI-for-AI：随着大语言模型从被动内容生成走向工程与科学发现的主动工作流，一个自然的问题是——AI 能否既作为被开发的对象，又作为构建下一代 AI 系统的主动参与者。作者没有停留在概念层面，而是选定「真实世界移动端 planner agent」作为试验台，用端到端闭环系统来回答这个问题。选择移动规划的理由是它同时具备两个难点：任务复杂且长程，直接考验 agent 的可靠性；真机交互代价高昂，使得开发迭代无法像纯软件任务那样快速扩展。这两个约束决定了论文的解法必须是「基础设施 + 方法论」层面的。

据此，作者构建了 Qwen-Planner-Agent，并把整个开发过程组织为一个闭环 AI-for-AI 框架。框架的耦合机制是一份共享的「行动—反馈—验证」契约（action-feedback-verification contract），它把数据生产、模型训练与部署三个环节串成一条链：因为三个环节使用同一套结构化字段描述动作、反馈与验证结果，任一环节产生的信号都能被其他环节消费。这一契约是理解全文结构的关键，也是作者主张「闭环」而非「流水线」的技术依据。

框架的第一条支柱是 AI for Data：一个带人工闸门的智能体化数据飞轮。由专门化 agent 承担任务构造、交互轨迹采集、训练数据的筛选与配平，并把训练反馈用于指导下一轮的数据生成。其设计动机是让数据分布随模型当前能力动态漂移——避免出现「太简单没有梯度信号、太难近似无信号」的错配；human gate 则把人工成本压缩到守门而非全量标注的规模，同时保留质量与风险控制点。

第二条支柱是 AI for Training：先做监督式规划冷启动，建立基本行为先验与输出格式；再进行混合环境下的在线智能体强化学习，以摊薄真机交互成本（混合环境的具体构成在摘要中未展开，需回原文核对）。训练侧的关键创新是 Competence-Aware Reward-and-Advantage Engineering（CARE），其明确目标是在保住任务表现的前提下降低推理与工具使用成本。这直接针对 agentic...**

<!-- paperflow:a8fe7d1e5b2c7d74 -->
## Learning to Ideate for Scientific Impact

[[Deep Reading - Sept 2026/Learning to Ideate for Scientific Impact|Deep Reading]]

[https://arxiv.org/pdf/2609.29802v1](https://arxiv.org/pdf/2609.29802v1)

- **论文的论证主线可以概括为一句话：当前 LLM 科学想法生成系统的优化目标被“可即时判断的代理指标”锁定，而要真正提升科学影响力，必须引入延迟出现的真实结果信号；引用归一化影响力是这样一个虽带噪但可规模化的信号，把它离线固化成奖励模型并用于 RL 对齐，可以系统性地提升生成想法的估计影响力。

问题侧：摘要首先指出想法生成正被 LLM 中介化，而训练与评测普遍依赖 novelty、clarity、feasibility 这类“当场可判”的代理。这类指标的便利性掩盖了与最终科学价值之间的错位。论文由此提出开放问题——延迟的科学接受度信号能否作为反馈来引导模型走向高期望影响力的研究方向。

技术与数据主线：论文的解法是一条四段式流水线。第一段，数据构造：从 10 万篇以上计算机科学论文中抽取目标条件化的想法描述，为每篇论文赋一个序数的、按年份归一化的引用标签，形成「研究目标—想法—影响力标签」监督数据。第二段，奖励建模：训练一个目标条件奖励模型，输入是研究目标与想法构成的配对，输出是引用影响力标签的预测；“目标条件”的设计意味着同一想法的价值被显式地放进具体研究语境中评估，而非给出通用的“好想法”打分。第三段，生成器对齐：先做监督微调，再以该奖励模型为信号做强化学习，把生成器推向奖励更高的区域。第四段，评测：为抑制循环性，采用留出的、参考锚定的协议——在同一研究目标下把模型生成的想法与历史想法比较，判断按该历史想法的引用影响力标签加权，从而使“与高引历史工作打成平手”比“与低引历史工作打成平手”更有分量。

实验主线与关键发现：摘要报告的主要结果是 RL 调优模型生成的想法的估计影响力持续高于基座模型与 SFT 基线，形成 base < SFT < SFT+RL 的三档对比结构。也就是说，监督微调本身带来增益，而引入延迟信号奖励的 RL 在 SFT 之上仍有额外增益。作者由此主张，科学影响力可以作为开放式科学发现中对齐 LLM 的一种实用的、以结果为依据的反馈信号。

需要强调的证据边界：本报告仅基于摘要与元数据，论文的 Introduction、Method、Results...**
