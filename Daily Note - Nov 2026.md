# On-Policy Distillation & Post-Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：On-Policy Distillation & Post-Training
- 方法：generation, audio
- 论文/报告：1 篇
- OmniOPD: Stable On-Policy Distillation for Joint Audio-Video Generation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:73769790f04c4a9d -->
## OmniOPD: Stable On-Policy Distillation for Joint Audio-Video Generation

[[Deep Reading - Nov 2026/OmniOPD-Stable On-Policy Distillation for Joint Audio-Video Generation|Deep Reading]]

[https://arxiv.org/pdf/2611.02803v1](https://arxiv.org/pdf/2611.02803v1)

- **### 一、论文要解决的问题与其重述方式
OmniOPD 的出发点是联合音视频生成的后训练。RL 已被证明能提升单一模态的生成质量，但当音频与视频各自异质的奖励被同时优化时，会出现目标冲突与梯度不均衡，即跨模态干扰。On-policy distillation 提供了一条绕开该干扰的路径：先用各自的奖励把音频教师、视频教师分别训练好，再沿学生自己的生成轨迹把两个教师的能力以稠密监督迁移进统一学生。但作者指出一个被忽略的前提：强教师不等于均衡迁移。联合音视频生成中音频 token 远少于视频，音频一侧监督稀疏，蒸馏因此不稳定，异构监督的协调成为核心难点。

### 二、关键观察与问题的重新定义
论文的第一处贡献是一个负面/反直觉观察：直接加大 audio loss 权重并不能解决稀疏监督问题，因为音频学习高度依赖视觉上下文。这一观察把问题从“损失配比”重构成“条件信息质量”，即需要在视觉侧提供更丰富、更可靠的线索来抵消稀疏的音频信号。这一重构是后续三个机制设计的逻辑起点。

### 三、方法：三条机制、一个解耦结构
整体结构是两阶段：第一阶段在 LTX-2（22B）dual-stream 联合音视频 DiT 上，用 DiffusionNFT + LoRA 分别以视频三奖励（HPSv3、VideoAlign、同步）与音频三奖励（Audiobox Aesthetics、CLAP、同步）训练两个教师；第二阶段冻结教师，各自监督自己的模态，共同训练学生。方法侧有三条机制：
1）非对称噪声调度：让音频教师在更干净的视频上下文下生成目标，构造特权视觉信息，提升单条音频监督的信息量；
2）分组多噪声采样：用 G 个分支聚合音频监督，扩大对音频状态分布的覆盖，降低方差；
3）基于教师—学生转移散度的 timestep 级重要性加权：在信任域内保留完整监督、信任域外降权，抑制不可靠信号与音频学习的波动。
三者分工可概括为“信息量—覆盖度—可靠性”的正交补强，而非同质堆叠。

### 四、实验主线
实验在 JavisBench 与 VBench 上进行，底座为 22B 的 LTX-2，奖励配...**

# Multimodal Models & Visual Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Multimodal Models & Visual Reasoning
- 方法：generation, reasoning, stat-ml, stat-me
- 论文/报告：2 篇
- Parse Before You Execute: Efficient Spatial Reasoning with Executable Primitives
- R$^2$-Omni: Reducing Structural and Precision Redundancy in Omni-LLMs for Long-Form Generation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:0465e7c3d8ad942c -->
## Parse Before You Execute: Efficient Spatial Reasoning with Executable Primitives

[[Deep Reading - Nov 2026/Parse Before You Execute-Efficient Spatial Reasoning with Executable Primitives|Deep Reading]]

[https://arxiv.org/pdf/2610.41389](https://arxiv.org/pdf/2610.41389)

- **【论文主线】论文的主线可以概括为：工具集成式空间推理的瓶颈不只在感知模型或几何工具，而在“查询编译”这一中间层。现有系统把查询编译成代码、任务约束、工具计划或证据集合，这些产物过于开放，没有在执行前明确回答“这个查询到底需要哪些空间承诺”。作者把这种失败模式称为 under-committed query compilation，并认为它会同时导致两类问题：执行器进行不必要的工具获取与多轮迭代，以及模型用流畅但空洞的推理轨迹掩盖空间承诺的缺失。

【方法主线】PEPS 的解决方案是 parse-before-execute。系统定义一个带类型的 FESM schema，把空间承诺分为 Frame、Entity、State、Metric 四类，并在一个原语库 L 上工作。Parser 不生成自由代码，而是把查询提交为 L 中的紧凑原语集合 P；这个集合既是查询表征需求的显式声明，也是后续工具调用的接口。Executor 只对 P 中未解决的原语做选择性 grounding，通过固定几何 API 和确定性计算产生答案与执行轨迹。Verifier 检查查询、图像、答案和轨迹，判断执行是否充分，返回接受/拒绝、质量分数和结构化反馈；若拒绝，反馈用于定向修复，若接受则提前终止。整个过程是免训练的闭环，最多迭代到上限。

【技术细节】FESM 不是表面空间关系的分类学，而是执行接口：Frame 负责参考系与视角，Entity 负责物体、区域和锚点，State 负责属性与关系，Metric 负责距离、尺寸、角度等可计算量。方法片段还强调 Reference Frame Grounding：所有 anchor points 相对共享场景表示提取，以保证多图或多视角下的计算基座一致。Parser 的输出被限制在 L 内，因此解析结果可检查、可验证；Executor 的选择性 grounding 避免了“把所有工具跑一遍”的浪费；Verifier 的轨迹级检查则把“继续迭代”从默认行为变成有条件行为。

【实验主线】论文在三个互补基准上评估：MMSI-Bench 覆盖多图真实场景、相机位姿...**

<!-- paperflow:8dc541dd33f64762 -->
## R$^2$-Omni: Reducing Structural and Precision Redundancy in Omni-LLMs for Long-Form Generation

[[Deep Reading - Nov 2026/R$^2$-Omni-Reducing Structural and Precision Redundancy in Omni-LLMs for Long-Form Generation|Deep Reading]]

[https://arxiv.org/pdf/2611.03247v1](https://arxiv.org/pdf/2611.03247v1)

- **R²-Omni 是一篇面向 Omni-LLM 长文本生成的推理效率论文。它的起点不是“如何再设计一个更强的多模态模型”，而是“如何在不大幅损失开放式生成质量的前提下，压缩音视频输入带来的巨大 KV cache”。论文首先指出，现有 token pruning 在多项选择任务上可以在很高压缩率下维持性能，但长文本生成会明显退化；作者将这一差距归因于 query-guided token selection 的偏差：它倾向于保留回答当前问题所需的少量证据，却丢弃全面描述所需的事件、属性、时间和跨模态细节。

围绕这一诊断，论文把冗余拆成两层。第一层是结构冗余：音视频 token 之间存在大量重叠信息，视频表现为时空相似，音频表现为局部连续性，因此可以通过 Structure-Aware Token Consolidation（STC）合并表示，而不是按 query 注意力硬删 token。第二层是精度冗余：即使 token 数已经减少，保留表示的缓存值仍可能使用过高数值精度，因此可以用 Semantic-Sensitivity Guided Precision Allocation（SSPA）按注意力输出误差的估计削减量分配值精度，并用 query attention 与音视频对齐作为语义先验。两者结合形成 training-free 的 R²-Omni 框架，分别压缩 token 数与位宽。

实验主线覆盖三个 Omni-LLM（Qwen2.5-Omni-3B、Qwen2.5-Omni-7B、MiniCPM-o-4.5）和三个长文本音视频基准（video-SALMONN-2、LongVALE、OmniDCBench），对比 token pruning 基线 OmniZip、OmniSelect、SEATS，KV-cache quantization 基线 KIVI、OTT、OScaR，以及 naive composed 组合。论文报告平均 5.0× 内存压缩、97.98% 的基线生成质量保持和 1.48× 端到端加速。Table 2 的结果显示，token pruning...**

# Reward Models & Reinforcement Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Models & Reinforcement Learning
- 方法：待从后续精读中沉淀
- 论文/报告：1 篇
- Do Visual RL Gains Scale at Test Time? A Quantile View of Post-Training
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:a59aab5236b81aee -->
## Do Visual RL Gains Scale at Test Time? A Quantile View of Post-Training

[[Deep Reading - Nov 2026/Do Visual RL Gains Scale at Test Time-A Quantile View of Post-Training|Deep Reading]]

[https://arxiv.org/pdf/2611.02366v1](https://arxiv.org/pdf/2611.02366v1)

- **这篇论文关注一个在部署中很现实、但在视觉生成 RL 文献里常被单样本指标掩盖的问题：post-training 带来的收益，是否能在 Best-of-N 推理扩展下保持？作者把这一性质定义为 scaling transfer，即 post-training 收益沿 Best-of-N quality-compute frontier 的保持程度。论文的论证主线可以概括为：单样本奖励提升会改变整个奖励分布；Best-of-N 随 N 增大越来越依赖奖励分布的上尾；如果 RL 改善典型样本却压缩上尾，那么单样本更强并不等于大预算下更强，甚至会被 base model 反超。

在背景上，论文位于视觉生成 RL post-training、test-time scaling/Best-of-N reranking、分位数/尾部风险分析三条线的交叉点。现有扩散/流模型和统一多模态模型的 RL 方法（如 FlowGRPO、DiffusionNFT、GRPO-Guard、AWM、UniGRPO）主要优化单样本奖励，并在单样本 benchmark 上比较；部署时却常生成 N 个候选并用奖励模型选最高分。论文指出，这种评价方式遗漏了“选择”所依赖的上尾质量。

技术核心是 quantile-transfer 框架。作者把 prompt 下的奖励视为随机变量，用 CDF 和分位数函数描述；在 iid 采样假设下，N 个样本最大奖励的期望等于分位数函数以上尾权重 N u^{N-1} 的积分，权重随 N 增大向 u=1 集中。因此，Best-of-N 的 quality-compute frontier 主要由高奖励分位数决定，而不是均值或中位数。若 post-training 在低/中分位数上为正、在高分位数上为负，就会出现“小 N 赢、大 N 输”的交叉；若上尾 lift 只是变小，则表现为衰减或饱和。这个框架把 RL 后训练从“提高一个分数”重新表述为“重塑奖励分布”。

实验上，论文覆盖多种 RL 方法、SD3.5-M 与 BAGEL 两个 backbone，以及组合生成、文字渲染、...**

# Data, Benchmarking & Evaluation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data, Benchmarking & Evaluation
- 方法：language, reasoning, audio
- 论文/报告：4 篇
- PianoBench: Evaluating Musical Perception and Reasoning in Language Models
- Blueprint2CAD: Benchmarking Foundation Models on Engineering Drawing-to-CAD Render Correspondence
- VisCodeBench: a new benchmark dataset for Text-to-Vis reflecting real-world practice
- Omni-Life: Benchmarking Omnimodal Real-Time AI Assistants in People's Daily Lives
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:8372c80cd42bf447 -->
## PianoBench: Evaluating Musical Perception and Reasoning in Language Models

[[Deep Reading - Nov 2026/PianoBench-Evaluating Musical Perception and Reasoning in Language Models|Deep Reading]]

[https://arxiv.org/pdf/2611.03839v1](https://arxiv.org/pdf/2611.03839v1)

- **一、问题与动机。论文的出发点是：多模态语言模型在广谱音乐理解基准上不断进步，但这些基准多由曲目级 caption/tag 或粗粒度问答构成，无法回答「模型到底能感知哪些局部音乐特征、又能对哪些显式事件进行推理」。作者把音乐理解拆成两条路径：从音频感知局部事件，以及对以 MIDI-text 显式给出的音符事件做推理；并指出二者的监督需求不同。
二、数据。作者构建 PianoTalk：从 YouTube 钢琴教学视频出发，用 LLM 筛选标题/描述，做源分离、语音转写、钢琴转写与时间对齐，得到 138,212 个视频、17,706 小时语音、14,931 小时钢琴音频（6,571 小时重叠）、总计超过 20,000 小时的交错音乐-语言数据。教学视频的关键是「讲解紧邻演示」：教师先介绍概念，再在键盘上弹出，再评论刚弹的内容，因而提供了事件级的跨模态对应。
三、基准。PianoBench 从 PianoTalk 测试集构造多项选择题：先处理 MIDI、选片段，再由 LLM 生成问题/答案与干扰项，并用 audio-grounding 规则约束不得编造或声称听到音频。同一题面在音频、MIDI-text、no-music 三种条件下呈现；音频又分 Pianoteq 合成与 source-separated 真实钢琴。这样可以在同一内容上比较「感知」与「推理」，并用 no-music 控制估计语言先验。
四、实验。作者评测开放权重与闭源音频语言模型，并让前沿文本模型在 MIDI-text 上使用不同 CoT/thinking budget；主要指标是准确率及相对 no-music 的 Δ。另以 Qwen 3.5 9B Base 做 mid-training/instruction tuning，检验交错教学数据能否改善无 CoT 问答。
五、发现。开放权重音频模型最高 52.8%，音频增益 −1.6 到 +6.0 pp，部分模型负增益；Gemini 3.1 Pro 达 80.4%（+16.8 pp），Qwen 3.5 Omni Plus +11.2 pp；Audio Flaming...**

<!-- paperflow:94ed980457899836 -->
## Blueprint2CAD: Benchmarking Foundation Models on Engineering Drawing-to-CAD Render Correspondence

[[Deep Reading - Nov 2026/Blueprint2CAD-Benchmarking Foundation Models on Engineering Drawing-to-CAD Render Correspondence|Deep Reading]]

[https://arxiv.org/pdf/2611.02433v2](https://arxiv.org/pdf/2611.02433v2)

- **一、研究定位与论证主线
本文是一篇基准与诊断型论文，而不是方法创新型论文。它的核心主张可以概括为一句：从工程图纸走向三维实体，任何自动流水线都依赖一个能判断「这张图画的是哪个零件」的视觉前端，而这个前端目前并不存在，且业界没有一个公开基准来测量它的缺失程度。围绕这一主张，论文做了三件事：把任务定义清楚、把数据造出来、把现有模型系统性测一遍并给出归因。

论证链条是：
(1) 工程图用正交投影与制图惯例隐式编码形状，零件本体是精确 B-rep，两者之间没有天然对应，因此识别依赖真正的几何推理；
(2) 这件事是零件库检索、既有设计复用、图纸数字化的第一道关卡，但现有 CAD 多模态工作把它当作视觉问答的小子任务，或默认它已被解决；
(3) 于是需要一个专门的、可控的、可分层的基准；
(4) 实测结果是：现有模型确实做不到，而且失败的原因不是「模型不够强」，而是「视角没对齐」和「判定有偏差」；
(5) 连少量监督微调也救不了——它只改了决策边界，没改表征。

二、技术主线：任务定义与数据构建
任务被定义为图纸—渲染对应判定：输入一张正交线图纸（clean 或带尺寸标注）与一张着色渲染图（通常来自另一视角），输出二者是否同一零件。这个定义的精妙之处在于它可承载为两种形态——检索（Recall@1）与成对验证（Yes/No）——从而把「表征能力」与「决策能力」分开测量。

数据层 BlueCAD-30K 的构建逻辑是：从 Zero-to-CAD 约 30 万程序化 CAD 模型中随机取 10% 得 30,237 个，用开放制图流水线为每个模型补充三类信息：(a) 视角对齐的着色渲染；(b) 六个视角下的 clean 与带尺寸标注正交线图；(c) 表面类型元数据。经质量过滤，29,925 个通过全部阶段，312 个（1.0%）在制图或渲染环节失败。所有 split 连同 10,000 模型 gallery 一并公开。

选择程序化 CAD 而非抓取仓库，是一个关键的范围决策：换来可复现、可精确控制视角与元数据，代价是把域限制在干净、参数化、无 GD&T 的零件族内。论文自己声明只...**

<!-- paperflow:373044f29209aaf0 -->
## VisCodeBench: a new benchmark dataset for Text-to-Vis reflecting real-world practice

[[Deep Reading - Nov 2026/VisCodeBench-a new benchmark dataset for Text-to-Vis reflecting real-world practice|Deep Reading]]

[https://arxiv.org/pdf/2611.02353v1](https://arxiv.org/pdf/2611.02353v1)

- **【定位】VisCodeBench 是一篇 benchmark 与 dataset 论文，目标是把 text-to-vis 评估从合成、模板化的图表生成，推进到更接近真实实践的多源、端到端评测。论文不提出新的生成模型，而是提出数据、任务协议与系统级比较。
【问题】text-to-vis 要求系统根据自然语言请求和底层数据生成可视化代码。LLM 已能处理简单请求，但现有基准（nvBench、nvBench2.0、ChartX、Text2Chart31、MatPlotBench、Text2Vis）多为合成或自动构造，缺少真实上下文，代码常遵循固定模板；作者此前工作也指出真实使用与基准分布不匹配。因此，简单请求上的高分不能说明系统能处理真实、复杂、欠定义的数据可视化任务。
【基准构造】VisCodeBench 含 2,672 个样本，来自三条链路：1）GitHub-T2V 843 个，来自 GitHub 上在线课程的 assignment notebooks，代表教育场景；2）arXiv-T2V 805 个，通过联系论文作者收集图表、代码、底层数据和自然语言请求，代表科研场景；3）OWID-T2V 1,024 个，来自 Our World in Data，代表普通受众或公共传播场景。样本复杂度不一，覆盖数据加载、转换与图表生成。专家标注者均具有四年以上编程经验和丰富学术可视化经验，负责核验；研究取得知情同意，收集数据以 CC BY-SA 4.0 发布。完整数据集私有，通过 leaderboard 访问，以防 LLM 训练使用。
【任务与评估】任务输入是数据与自然语言请求，输出是可执行代码和图表。论文用七项指标：任务完成率、执行时间、chart type accuracy、displayed data similarity、chart correctness、readability、accessibility；分别覆盖执行、图表内容与视觉质量。displayed data similarity 是关键内容指标，衡量生成图中呈现的数据与参考数据的接近程度。
【实验】论文比较 Cod...**

<!-- paperflow:2af216319cc56b7c -->
## Omni-Life: Benchmarking Omnimodal Real-Time AI Assistants in People's Daily Lives

[[Deep Reading - Nov 2026/Omni-Life-Benchmarking Omnimodal Real-Time AI Assistants in People's Daily Lives|Deep Reading]]

[https://arxiv.org/pdf/2611.02835v1](https://arxiv.org/pdf/2611.02835v1)

- **一、问题与定位。Omni-Life 关注的是实时全模态助手在日常生活里能否“在正确的时间做正确的事”。论文的出发点是：全模态全双工模型正在把交互从轮次式问答推向带视听感知的连续辅助，助手的任务从“回答一个问题”变成“持续决定是否响应、何时响应、响应什么”。仅凭视听理解能力不能证明这种帮助是有用且及时的；而现有评测多停留在原子能力层面，缺少面向真实应用、覆盖危险与边缘案例、并把“保持沉默”也当作考核项的基准。

二、基准构建。Omni-Life 的评测集包含 400 段视频：277 段合成 + 123 段真实。290 段至少需要一个干预，合计 555 个必需干预事件；110 段要求全程沉默，用来测试模型在不需要帮助时是否乱输出。合成视频用于危险情境与 edge cases，额外要求物理合理性与参与者、物体一致性；真实视频提供自然场景；两者共享同一时间协议。论文把日常支持分为四类（感知、认知、……），覆盖五个主要场景，并在附录 D 给出跨用户群体、跨场景、跨失败类别的 14 个案例。时间标注与 QA 构建（§3.4）为每段视频标注响应窗口、响应时间线与相关输出描述，使“是否在窗口内响应”与“响应内容是否对应事件”可被自动判定。

三、评估协议。主实验联合评估事件覆盖、响应内容、时机与 unwanted output；Content Accuracy 与 Timing Accuracy 各以 400 为分母；Joint Accuracy 要求一段视频的两个答案都正确；Full Correct 是最严格的整段全对指标。主 F1 使用 main-score matches，提前输出按未匹配响应计。表 5 提供时机/内容分离诊断：把响应分为 on-time correct、on-time incorrect、late correct、late incorrect、no later reply、others，late 定义为 onset 在窗口终点之后且不设上限；该诊断不计入主分，也不判定迟到回复对应哪个事件。消融研究任务提醒与响应延迟容忍两个部署旋钮。

四、主实验与发现。12 种...**

# Web, GUI & Computer-Use Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Web, GUI & Computer-Use Agents
- 方法：agent, optimization, gui-agent, detection, ai-detection, stat-ml, stat-me
- 论文/报告：3 篇
- Hybrid Computer Use: A Unified Programmatic, Semantic, and Visual Interface for General Agents
- WebHarbor: Docking Real Websites for Evolving GUI Agent Environments
- CUA-Master: Scaling Computer-use Agents through Open-World Recursive Improvements
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:f0f51463404c55cb -->
## Hybrid Computer Use: A Unified Programmatic, Semantic, and Visual Interface for General Agents

[[Deep Reading - Nov 2026/Hybrid Computer Use-A Unified Programmatic, Semantic, and Visual Interface for General Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.41432](https://arxiv.org/pdf/2610.41432)

- **一、论文的出发点
这篇论文关注的不是某个新的规划算法，而是 computer-use agent 的“接口层”：agent 通过什么动作空间操作计算机。主流做法沿用人类使用软件的流程——看截图、找控件、用鼠标键盘点击输入。作者认为，这套接口虽然覆盖广，却把强烈的人类交互先验强加给 LLM：模型被迫反复解释屏幕、做坐标 grounding；应用明明通过 accessibility（AX）暴露了控件身份和值，agent 仍要像素级定位；数据本可由代码一次处理，agent 却要重复点击拖拽。随着 OSWorld 2.0 把任务推向长程专业工作流，这种接口的代价从“单步麻烦”变成“效率与状态跟踪的系统性瓶颈”。论文因此主张：不要把 GUI 当作唯一通道，而应把计算机已有的程序化、语义和视觉能力都暴露给模型，让模型选择与组合。

二、HCU 的接口设计
HCU 是一个统一接口，包含三条互补通道：
1. Programmatic：代码、命令行、Bash 等，直接访问文件系统、数据和系统能力，适合批量、精确、可脚本化的操作。
2. Semantic：accessibility（AX）树，读取控件身份、属性和动作，执行语义级操作。论文的 AX 工具返回紧凑观测、支持批量动作、以 AX diff 报告效果；窗口可按标题激活/关闭，这是 GUI 不擅长的。
3. Visual：截图 + 鼠标键盘，覆盖没有 AX/API 的应用，承担视觉兜底。
三条通道不是简单堆叠，而是放在同一个 observation–action loop 中：给定任务与观测，模型选择一个通道检查状态或执行动作，工具返回新的观测/效果。接口还提供错误码与 hint、wait 语义（等待 1 秒且不返回观测，单批最多 20 秒），并用批处理与 diff 减少 round trips。

三、状态安全与技能
混合通道的核心风险是 stale state：切换通道时，截图可能过期、AX 树可能未刷新、代码可能已改变状态。HCU 在通道切换时做防护，并像 CUA Driver 一样区分“已知未送达”和“可能输入后的未知效果”。...**

<!-- paperflow:504184f73081e058 -->
## WebHarbor: Docking Real Websites for Evolving GUI Agent Environments

[[Deep Reading - Nov 2026/WebHarbor-Docking Real Websites for Evolving GUI Agent Environments|Deep Reading]]

[https://arxiv.org/pdf/2611.03634v1](https://arxiv.org/pdf/2611.03634v1)

- **一、论证主线
论文的出发点是一个双重困境。评测侧：live 网站虽然真实，但内容会变、会反爬、会做地区限制，作者实测约 38% 的 Online-Mind2Web 任务被 bot detection 阻断、约 22% 的 WebVoyager 任务因网站变更而过时，而且 live benchmark 几乎覆盖不到需要登录的工作流。训练侧：RL 需要可执行的状态转移与可重复的 reset，live 网站两者都给不了。合成环境是替代方案，但既有工作要么强调「自动生成新世界」而非对真实网站做任务锚定的重建，要么在图像丰富度与认证后功能上覆盖有限。Table 1 把这一缺口形式化：现有已发布环境无法同时具备广网站覆盖、离线执行、图像丰富内容与基于登录的工作流。
接着论文指出困难的本质：商业网站内容功能体量过大，完整重建不现实；而视觉相似的复制品可能缺少正确的搜索、账户管理、预订、交易逻辑。也就是说「像」与「能用」是两个会各自失败的目标，只优化其一会得到不可用或不像的环境。
由此得出主张：需要一种既保真又可控、可离线、可重置、并且能持续生长的环境资产。

二、技术主线
WebHarbor 的核心是「task-driven docking + human-agent collaboration」这一构建闭环，产物是 153 个可执行、Docker 化的网站环境与 3,518 条任务。闭环有四步：(1) 新任务先定义环境需要哪些页面、状态与逻辑；(2) 编码 agent 在任务条件下实现这些网站功能；(3) 人类贡献者审计三项指标——视觉保真度、功能正确性、任务有效性——直到通过；(4) 环境随任务集扩大而演化，未通过则回退修复。这一设计把「环境」从一次性数据集变成随任务生长的工程资产，也解释了标题中 evolving 的含义。
可控性有两个工程支点。其一是 Docker 化与轻量状态重置，使离线确定性执行与高效 RL rollout 成为可能；其二是接口对齐——检索片段显示 live-web 与 mirror rollout 共享 13 个浏览器工具，harness 允许在一次响应...**

<!-- paperflow:6dabadc9d821a53f -->
## CUA-Master: Scaling Computer-use Agents through Open-World Recursive Improvements

[[Deep Reading - Nov 2026/CUA-Master-Scaling Computer-use Agents through Open-World Recursive Improvements|Deep Reading]]

[https://arxiv.org/pdf/2611.03126v1](https://arxiv.org/pdf/2611.03126v1)

- **一、论文要解决的问题。论文认为 CUA 的能力提升受限于交互数据的多样性、可控性与可靠性，而更根本的瓶颈是“数据构建”和“模型优化”没有形成闭环。现有管线大多是单向的：先造数据，再训练模型，数据构建并不了解模型当前在哪里失败；因此新增数据可能只在简单任务上重复，无法补齐长程、跨应用、GUI+CLI 混合任务的能力缺口。已有的 recursive improvement 虽然引入迭代，但主要依赖模型自生成经验，属于 closed-world optimization，探索受限于模型自身知识与能力，容易出现收益递减。论文试图用“开放世界语料 + 模型弱点反馈”打破这个封闭性。
二、方法主线：嵌套双环。CUA-Master 的核心是一个嵌套的双环递归改进框架。系统层的外环叫 world-data-model flywheel：它持续吸收 open-world corpus，观察模型在演化中的弱点，并用这些弱点指导下一轮数据构建和优化迭代。数据层的内环把 open-world seeding 与 autocatalytic propagation 耦合：前者从 instructional videos 中挖掘真实工作流作为种子任务，后者把种子转成可复用 primitives，以可控方式合成多样、复杂、可验证的任务。两个环不是并列模块，而是通过“模型弱点”与“新数据”互相驱动：外环决定“缺什么”，内环负责“造什么”，训练后的新模型又产生新的弱点。
三、任务形式化与动作空间。论文把任务定义为 T=(I,E,V)，I 是指令，E 是交互环境，V 是任务特定 verifier。智能体在每一步根据指令、观察和历史生成一个或多个原子动作；L_s=1 表示单步动作，L_s>1 表示批量动作序列。执行后环境转移，直到终止或达到步数预算，形成完整轨迹。这个定义把 verifier 写进任务本身，强调“可验证的完成”而不是“看起来像在操作”。动作空间是统一的原子动作空间，支持 direct GUI；结合混合基准，可以合理推断它也覆盖 CLI/工具调用。批量动作的设计有助于减少交互轮次，但会增加动作...**

# Agent Skills, Harness & Tooling

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Skills, Harness & Tooling
- 方法：agent, retrieval
- 论文/报告：1 篇
- Context as Infrastructure: Knowledge Compilation for Retrieval in Agentic Systems with Harnesses
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:74a616e51261cca0 -->
## Context as Infrastructure: Knowledge Compilation for Retrieval in Agentic Systems with Harnesses

[[Deep Reading - Nov 2026/Context as Infrastructure-Knowledge Compilation for Retrieval in Agentic Systems with Harnesses|Deep Reading]]

[https://arxiv.org/pdf/2610.41172](https://arxiv.org/pdf/2610.41172)

- **一、论文的论证主线
这篇论文的起点不是“检索不够准”，而是一个范式判断：RAG 正在 agent 化。传统 RAG 把检索压缩成一次 query→embedding→similarity→top-k 的调用；agentic 系统则把检索展开成 search harness 的循环：LLM 写 lexical query、读 snippet、改写、再查、判断证据是否足够、决定停止。作者认为，一旦 harness 具备这种能力，检索层的经典目标——更好的 chunking、更好的 embedding、更高的 Recall@k——就不再是 binding constraint。论文用一组醒目的 operating point 支撑这个判断：只给 harness BM25 与 grep，回答模型用 MiniMax M3，在 BrowseComp-Plus 上答对 85%，在多跳 FRAMES 上达到 91%，没有编译索引，也没有向量索引；作者同时提醒两个 benchmark 不可直接比较。
二、问题诊断：context 而不是 recall
论文把 agentic RAG 的新瓶颈称为 context problem。无纪律的循环会重复同一查询、重复阅读同一文档、把从未使用的段落塞满窗口；token 被消耗，但过程没有变成答案。这里的“上下文问题”至少有三层：其一是冗余，检索结果重复且未被使用；其二是结构缺失，chunk 不包含跨文档实体关系、时间线、论证层级、定义依赖与矛盾；其三是治理缺失，系统不知道哪些 block 可以进入上下文、哪些 claim 能回指证据。作者因此主张，检索层应当从“返回 ranked passages”升级为“交付 compiled, governed context”。
三、方法主线：knowledge compilation + smallest sufficient set
论文提出的缺失层是 knowledge compilation：一个可配置的摄入流水线，让 LLM 为源文档补充 chunk 本身不包含的上下文。产物不是单一索引，而是一族...**

# Memory, Personalization & Long-Horizon Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Memory, Personalization & Long-Horizon Agents
- 方法：vision
- 论文/报告：1 篇
- Can Personal Requirements Become Supervision for Planning?
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:92193cace7a91455 -->
## Can Personal Requirements Become Supervision for Planning?

[[Deep Reading - Nov 2026/Can Personal Requirements Become Supervision for Planning|Deep Reading]]

[https://arxiv.org/pdf/2610.40992](https://arxiv.org/pdf/2610.40992)

- **论文的主线可以概括为：多病共存个体的饮食管理是一个被现有 LLM 规划范式严重低估的长程硬约束问题；作者用严格评测揭示失败，再提出以可执行个人规范监督 LLM 规划的 Noura 框架。
1. 动机与问题
多病共存要求饮食同时满足疾病特异和药物特异的要求，跨餐次、跨天联合优化，还要保留既有饮食习惯。临床指南和药品标签是公开的，但“公开知识”不等于“个体可执行计划”。把自然语言指南、个体疾病/用药/营养目标和饮食偏好转成一个可行的多日膳食计划，需要处理离散食物选择、连续份量、跨期耦合、硬约束与软约束冲突。论文把这一问题定义为需要可执行规范监督的规划问题，而不是单纯的食谱生成。
2. 评测发现
作者构建了基于 NHANES 的评测，覆盖 13 个模型，从医学微调的 8B 模型到 GPT-6.1 Sol。直接生成完整计划时，通过全部硬约束的比例少于 3%。显式规则、提高推理努力或模型自选工具，最多让 GPT-6.1 Sol 达到不超过 8%。这说明现有“提示+推理+工具”的范式在长程硬约束下存在系统性瓶颈：模型可以生成语言上合理的餐食，但无法保证跨天聚合后的营养和临床约束同时满足。
3. 方法主线
Noura 把 LLM 规划与可执行个人规范配对。规范由公共临床指南和个体健康/饮食档案实例化而来；在规划过程中，规范负责四件事：把食物 grounding 到营养数据库，优化份量，检查完整多日计划，指导修订。LLM 不是一次性生成答案，而是在“提出计划—规范检查—违反反馈—修订”的循环中工作。
4. 关键洞见
摘要的核心洞见在“a specification that rejects a…”处截断；合理推断是：规范拒绝一个计划，不只是给负分，而是提供可操作的监督信号，指导模型修复。这把规范从“最终过滤器”变成“规划过程中的监督者”，也是论文标题“Can Personal Requirements Become Supervision for Planning?”的直接含义。
5. 结果与意义
论文报告了基线的严重失败，并提到 Qwen3-14B 候选池扩大 4.7 倍、四个 Qw...**

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, generation, optimization, deep-learning
- 论文/报告：4 篇
- Learning to Interact and Direct: Co-Evolving Video Director Agents
- Toward Knowledge-Centric AI: Discovering, Internalizing, and Using Explicit Knowledge Across Agents and Generations
- Towards Self-Evolving Narrative Systems: Real-Time Adaptation and Long-Term Optimization with LLMs
- Do Agents Still Follow the Rules? Benchmarking Rule Compliance under Agent Context Compaction
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:131ce0e44c79b907 -->
## Learning to Interact and Direct: Co-Evolving Video Director Agents

[[Deep Reading - Nov 2026/Learning to Interact and Direct-Co-Evolving Video Director Agents|Deep Reading]]

[https://arxiv.org/pdf/2610.41865](https://arxiv.org/pdf/2610.41865)

- **一、论文要解决的问题
这篇论文关注 AI 视频导演智能体的自我演化。视频导演不是单次生成，而是把用户创意转化为剧本、镜头设计、素材生成、剪辑和风格控制等多阶段协调决策。已有自我演化研究主要优化生产侧：从执行反馈中提炼可复用技能、工作流配置和生产策略，让 earlier videos 的经验改善后续项目。论文指出，这条路线忽视了导演与用户交互策略的同步演化。用户最初请求往往不完整，导演必须通过对话澄清创作优先级、识别被委托的决策、在草稿出现后协商修改；交互时机和方式会直接影响返工量，而执行可靠性又会决定用户是把精力放在创作决策还是故障排查上。因此，论文主张交互与生产是相互构成的两个演化对象，应共同提升。

二、方法主线
作者提出 LITOct（正文简称 LIT），一个由导演智能体、模拟用户和元工程师组成的 harness framework。导演在每个项目中与模拟用户协作，构建并执行视频生产流水线；模拟用户带有 persona，规定其生产知识、沟通风格和披露行为，向导演分配创作任务并在对话中提供澄清、反馈、修订要求或对抗性探测；元工程师分析对话、执行轨迹和产物，诊断交互与生产问题，修订导演的交互策略和生产工作流，并从成功与失败的修订中学习，更新自己的任务选择与修订策略。

LIT 的关键是协同演化。早期版本虽能用元工程师根据任务轨迹修订导演策略，但仍依赖被动归因，难以适应更广的用户交互模式和生产技术。LIT 让元工程师主动学习外部知识，提升发现问题与提出解决方案的能力；它还会识别未充分探索的主题与评价标准，核验候选方案，寻找替代方案。论文在结论中称其为 curiosity-driven co-evolution。最终，元工程师、模拟用户和导演形成演化闭环：模拟用户制造压力，导演产生行为，元工程师诊断并改写规则，改写后的导演再面对新的任务与用户行为。方法片段还显示，控制流、agent 协调、演化流程以及各 agent 的生产、交互、演化策略都可作为可编辑资源，在 evolution epoch 中被修订。

三、数据与实验设计
论文构建导演—用户交互数据集，把视频/剧情任务...**

<!-- paperflow:e4bc786de6424ebb -->
## Toward Knowledge-Centric AI: Discovering, Internalizing, and Using Explicit Knowledge Across Agents and Generations

[[Deep Reading - Nov 2026/Toward Knowledge-Centric AI-Discovering, Internalizing, and Using Explicit Knowledge Across Agen|Deep Reading]]

[https://arxiv.org/pdf/2611.03157v1](https://arxiv.org/pdf/2611.03157v1)

- **一、这篇论文是什么
这是一篇立场/路线图论文（position paper），作者群来自知识图谱与 LLM 交叉的研究传统（合理推断，见 institution 字段），主张把“知识”从 AI 系统的附属品提升为组织原则：下一代 AI 应当围绕一个显式的、持续演化的、独立于任何单一智能体的知识层来构建。全文不提供系统实现与实验数值，交付的是定义、原则、形式化、系统化梳理与瓶颈假设。

二、论证主线（作者如何从现状推到主张）
第一步，确立“知识是人类跨代累积的机制”：知识是比其生产者更长寿的耐久记录，可被他人核查、组合、教授与纠正；它既是选择性的（写下来的只是社群认为值得保留的），也是可检视、可组合的。
第二步，指出 AI 目前完全没有等价物，并给出三重失败：知识被困在模型参数里（不可检视、不可组合、随智能体退役而消失）、被困在上下文窗口里（昂贵、长输入下利用不均 [76]、会话结束即失）、被困在系统私有记忆库里（只被追加与检索，而不被加工）。
第三步，指出问题的规模效应：智能体由大量彼此独立的用户与组织构建，模型、工具、目标、权限与知识表示各异，不共享上下文也不共享控制器，而每个智能体在数月内就被升级、替换或退役——因此“单系统内部改进记忆”根本无法解决问题，必须是跨系统、跨代际的知识层。
第四步，给出解法形态：一个三阶段闭环。智能体从环境与轨迹中发现知识（discover）；知识层把碎片化的异构记录与经验内化为可组合、可修订的理解（internalize）；跨系统跨代际的智能体使用该结果并回传新产出供修订（use）。
第五步，用组织理论做可行性类比：企业里隐性 know-how 被外化、与显式知识组合、再被新成员内化 [85]；AI 需要把这个已经在人类组织中跑通的循环，从“组织内的人之间”搬到“智能体之间与代际之间”。

三、技术主线（概念工具的搭建顺序）
1. 概念清理：先把 record / experience / knowledge 三级分开。原始动作日志是 record；把轨迹读成“试了什么、随后发生了什么”是 experience；抽成可迁移断言才是 k...**

<!-- paperflow:3d17d8ab023cccdd -->
## Towards Self-Evolving Narrative Systems: Real-Time Adaptation and Long-Term Optimization with LLMs

[[Deep Reading - Nov 2026/Towards Self-Evolving Narrative Systems-Real-Time Adaptation and Long-Term Optimization with LLM|Deep Reading]]

[https://arxiv.org/pdf/2611.01954v1](https://arxiv.org/pdf/2611.01954v1)

- **【论证主线】论文从一个被反复陈述却未被真正消解的二难出发：交互叙事要么给作者控制（分支树、固定对话），要么给玩家自由（LLM 开放生成），二者难以兼得。传统分支系统连贯但可预测；LLM 系统灵活但随机性与上下文敏感性带来叙事不连贯、角色行为不一致与作者控制力丧失（Gu et al., 2025；Alavi et al., 2024；Yi et al., 2025）。作者随后做了一个关键的重新定位：现有混合式方案（结构约束、个性化、多智能体长篇生成）的失败并非单点质量问题，而是“生成逻辑静态”——提示词与策略在设计完成后被冻结，系统不会从游玩历史中学习，因此在长时段中不可避免地退化（Peng et al., 2024；Jeong and Lee, 2025）。由此，论文的目标被定义为双重的：既要实时平衡控制与自由，又要在长时间尺度上维持并改进叙事质量。

【技术主线】框架由三个相互衔接的构件组成。第一是结构层：把剧情建模为分层叙事树，作者预写节点构成可复用骨架。第二是决策层：把玩家输入与候选节点编码为向量（节点侧使用 MiniLM），计算余弦相似度作为语义对齐分数 S(p, n_i)；若 S ≥ θ_high 则复用预写内容，否则进入自适应叙事分支，触发受上下文约束的实时生成，并对生成候选按一组原则打分、择优采纳。这一层把全局的“控制 vs 自由”二难降解为逐轮的局部路由决策，是论文最具工程感的机制。第三是演化层：收集玩家反馈，通过 in-context learning 迭代精炼提示词，免去人工提示工程，使系统的生成策略随游玩历史自我改进。三者构成“结构保底—路由仲裁—反馈演化”的闭环。

【实验主线】评估采用案例研究加 12 人参与者研究，覆盖叙事连贯性、NPC 一致性与玩家能动性三个维度，并以交互中的开放式追问采集具体的不连贯事件与创造性体验。分析上，作者除主观评分外还引入结局类型编码（Opaque Endings vs Earned Closure），把“故事是否站得住”转成可计数的指标。与 flat 系统相比，报告的主要结果是：Opaque Endings 由...**

<!-- paperflow:7d2462da1752a7b4 -->
## Do Agents Still Follow the Rules? Benchmarking Rule Compliance under Agent Context Compaction

[[Deep Reading - Nov 2026/Do Agents Still Follow the Rules-Benchmarking Rule Compliance under Agent Context Compaction|Deep Reading]]

[https://arxiv.org/pdf/2611.03223v1](https://arxiv.org/pdf/2611.03223v1)

- **论文研究一个具体但被忽视的 agent 安全问题：长程 LLM agent 为了适配上下文窗口，会把历史压缩成摘要；压缩后的上下文是否仍然携带治理后续动作的操作规则？如果规则不再携带，或者携带了但 agent 不遵守，违规就从这里发生。作者指出，已有工作虽然发现压缩后违规上升，但有两个方法论缺口：第一，合规测量发生在完整 agent harness 之外，缺少真实工具调用循环；第二，没有识别每次违规的来源，无法区分规则被删除、被改写、仍在但被忽略、被后续请求误当授权，还是规则投递通道本身有问题。为此，论文提出 ARCBench，一个建立在开源 pi agent harness 上的可配置 benchmark。它的核心设计是把 agent 放进工具调用循环，从执行过程中的动作与参数测量规则合规，而不是只评估模型的口头回答；同时为每个压缩运行匹配全上下文基线，并标注规则在压缩上下文中的状态，从而把违规归因到来源。

ARCBench 的案例不是任意规则问答，而是满足准入条件的可执行场景：违规要服务于具体攻击者目标（A1），要能从工具调用及参数判定（A2），规则在场景中要可信（A3），片段还提到 A5；候选案例若失败 A1 或 A5 会被排除。压缩配置包括七种生成式策略，为较早轮次写替换文本；Pi 与 Pi+RP 使用 pi 原生压缩路径，Pi+RP 额外加入指令。压缩器 prompt 要求保留事实连续性、不得编造或改变状态变量、保留 exact argument values、参考 critical context，摘要长度受 max_chars 与预算 B 限制。评测使用 violation@k，只统计有 k 次有效运行的设置，并与 matched full-context runs 比较；同时记录每个压缩上下文的规则状态，把“规则仍在但违规”和“规则被删除后违规”分开。攻击方面，compactor-targeted injection 在压缩前向工具结果注入内容，诱导摘要器改写、删除或降权规则；防御则试图在摘要中恢复规则或加入规则存在性提示。

实验覆盖四个模型、多种压...**
