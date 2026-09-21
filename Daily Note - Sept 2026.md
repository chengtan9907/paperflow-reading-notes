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
- 方法：generation, language, reasoning, optimization, audio, multimodal-reasoning
- 论文/报告：15 篇
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

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, ai-for-science, generation, language, reasoning, science-discovery, reinforcement-learning, optimization
- 论文/报告：30 篇
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

# Computer Vision

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Computer Vision
- 方法：agent, generation, reasoning, vision, multimodal-reasoning, gui-agent, stat-ml, stat-me
- 论文/报告：11 篇
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

# Machine Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Machine Learning
- 方法：vision, reinforcement-learning, deep-learning
- 论文/报告：4 篇
- Verify Before You Distill: Prompt-Level Teacher Gating for On-Policy Distillation
- You Are What You Read: Misalignment via In-Context Persona Induction
- DataFlex-RL: An Evaluation Platform for RLVR Data Policies
- EvoRS: On-Policy Self-Evolution of Reward Systems for Open-Ended Reinforcement Learning
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

# AI for Education

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Education
- 方法：agent, ai-for-science, language, vision-language-model, vision, reinforcement-learning, vision-language, deep-learning
- 论文/报告：4 篇
- Mapping the Emerging Social Science of Large Language Models
- Elastic Horizon: Discovering the Effective Interaction Frontier in Agentic Reinforcement Learning
- MUSE: Benchmarking Large Vision-Language Models on Multi-Modal Understanding in Situated Education
- ProgramDistill: From Interactive Web Apps to Verifiable Reference-Guided SWE Tasks
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

# AI Research

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Research
- 方法：vision
- 论文/报告：1 篇
- The Emerging AI Paper-Review Arms Race: Adversarial Co-Evolution in Scholarly Publishing
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

# AI for Science

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science
- 方法：agent, ai-for-science
- 论文/报告：2 篇
- SimpleDesign: A Joint Model for Protein Sequence and Structure Codesign
- LabAgent: Customize Any Research Hubs for Scientific Discoveries Using AI Agents
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
