# On-Policy Distillation & Post-Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：On-Policy Distillation & Post-Training
- 方法：agent, generation, reasoning, audio, stat-ml, stat-me
- 论文/报告：4 篇
- OmniOPD: Stable On-Policy Distillation for Joint Audio-Video Generation
- Reframe: Observation-Aligned Multi-Teacher On-Policy Distillation for Agentic Visual Reasoning
- PhysOPSD: Code as Privilege for Physically Plausible Video Generation
- Agentic-OPD: Recovering Forgotten Agent Capabilities via On-Policy Distillation for Agentic Tasks
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

<!-- paperflow:397d7d21cf4fe2ba -->
## Reframe: Observation-Aligned Multi-Teacher On-Policy Distillation for Agentic Visual Reasoning

[[Deep Reading - Nov 2026/Reframe-Observation-Aligned Multi-Teacher On-Policy Distillation for Agentic Visual Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2611.12886v1](https://arxiv.org/pdf/2611.12886v1)

- **（1）研究背景：视觉智能体的后训练有两条互补路线——agentic RL 让模型在工具交互中试错，multi-teacher on-policy distillation（MOPD）让学生在自己的 rollout 上吸收多个专家教师的分布。对视觉智能体而言，工具调用会引入一个关键中间状态：工具返回的观测。论文指出，现有方法主要在输出层做对齐，默认学生已经获得并使用了有效观测；但在真实 rollout 中，这两个条件都可能不成立。（2）问题拆解：第一，模型可能忽略工具返回的有用观测，形式上调用工具，实际仍依赖原始图像、原始视频或语言先验；第二，工具动作 grounding 不佳（缺失、无效、偏离目标）会返回低信息观测，使后续蒸馏建立在不良证据上。两者耦合：不依赖观测会降低工具使用意愿，不良动作又让依赖观测的模型无好证据可用。因此，仅优化最终答案或匹配教师 token 分布，无法区分“观测无效”与“策略不会用观测”。（3）核心洞察：Reframe 把 observation state 作为对齐对象，同时优化“获取有效观测”和“使用观测推理”。这与只对齐输出分布的蒸馏形成对比：蒸馏的 continuation 必须建立在可用观测上，教师信号才有意义；RL 的奖励也必须区分“用证据答对”和“不用证据答对”。（4）第一阶段 GD-GRPO：训练空间与时间专家。空间专家处理图像区域定位和裁剪，时间专家处理视频帧选择；两者使用证据感知奖励，片段明确提到 IoU grounding reward 与互补覆盖奖励，防止重复定位同一目标。更关键的是随机移除原始视觉输入：以 0.5 概率丢弃原图或原视频，工具返回观测仍可见，答案必须依赖工具检索到的证据；rollout 按预采样 drop flag 分组并在子组内归一化优势。这个信息瓶颈迫使专家学习“调用工具—获得证据—基于证据推理”，而不是复制原始输入。该阶段不需要任务特定 SFT 冷启动。（5）第二阶段 OG-MOPD：在学生 rollout 中检测缺陷工具动作，若存在有效的 canonical action，则替换缺失、无效或偏离的动...**

<!-- paperflow:25d8368088e1662e -->
## PhysOPSD: Code as Privilege for Physically Plausible Video Generation

[[Deep Reading - Nov 2026/PhysOPSD-Code as Privilege for Physically Plausible Video Generation|Deep Reading]]

[https://arxiv.org/pdf/2611.12411v1](https://arxiv.org/pdf/2611.12411v1)

- **1. 论文要解决的问题：现代视频生成模型在视觉保真度和时间连贯性上进步显著，但在物理上合理的动力学和复杂交互上仍然脆弱。作者认为，仅靠扩大预训练不够，需要针对性后训练；但 SFT 存在离线状态不匹配，RL 存在稀疏且定义不清的物理奖励。OPSD 把 on-policy rollout 与密集纠正反馈结合，缓解了上述问题，但把核心挑战转移到如何设计有效的特权上下文。2. 关键洞察：传统特权模态不够用。文本提示是陈述性的，难以表达参数化因果；真实参考视频把物理规律与视觉外观纠缠在一起，且没有参数控制；静态视觉观察不可编辑。可执行代码则实例化一个可编辑的物理世界，具有内在抽象性和可控性，可以参数化、执行、验证和生成反事实。3. 方法：作者提出 PhysOPSD，把可执行代码作为视频生成的特权物理上下文。PhysOPSD-Engine 将真实世界物理场景翻译成可验证的模拟程序，并生成模拟场景与渲染草稿。PhysOPSD 包含两个核心组件：双层次物理特权，通过中间瓶颈桥接信息差距；共享 rollout 噪声下的受控物理变化，以暴露因果动态变化。训练采用 on-policy self-distillation：学生在文本条件下 rollout，教师在代码/模拟/草稿特权辅助下提供密集纠正，学生最终只依赖文本部署。片段显示教师可能采用 EMA 更新，训练使用 AdamW 和梯度裁剪。4. 实验：论文在多种视频生成骨干上验证，使用标准物理基准，覆盖 T2V 和 V2V，评估 200 个动作类别的物理常识，并沿用 PhyT2V 的 pipeline。核心结果是仅用 3K 样本就优于 SFT 和 RL 基线，同时保持通用视频生成质量。定性例子包括茶壶在镜子前的反射同步、锤子砸混凝土开裂、投手倒红色液体等。片段还显示 dense geometric 和 abstract wireframe 两种草稿条件都能可靠合成物理一致动态。5. 贡献与意义：论文把“代码作为特权”提升为一种后训练范式，强调 coding agent 应赋能生成模型而不是取代它们；框架与骨干解耦，具有通用性；样本效率高，适...**

<!-- paperflow:08d01d5e74ed89a7 -->
## Agentic-OPD: Recovering Forgotten Agent Capabilities via On-Policy Distillation for Agentic Tasks

[[Deep Reading - Nov 2026/Agentic-OPD-Recovering Forgotten Agent Capabilities via On-Policy Distillation for Agentic Tasks|Deep Reading]]

[https://arxiv.org/pdf/2611.12799v1](https://arxiv.org/pdf/2611.12799v1)

- **【一句话定位】
Agentic-OPD 是一篇关于“RL 训练后能力回收”的论文：它不提出新的 RL 目标函数，而是指出标准 agentic RL 流水线在 checkpoint 选择上丢失了能力，并用历史 checkpoint 作为冻结教师池，通过选择性 on-policy distillation 把这些能力回收到单个学生中。

【问题链条】
1. 当前 LLM 智能体训练的主流范式是 verifiable-reward RL，GRPO 及其变体是常见优化框架；训练会产生一系列 checkpoint。
2. 标准实践只保留验证集最好的 checkpoint，其余丢弃。作者认为这是有损的：best-validation checkpoint 会在一些早先 checkpoint 已经解决的问题上失败，且所有 checkpoint 的解集并集远大于任何单个 checkpoint。
3. 因此，被丢弃的 checkpoint 不是冗余，而是一个无需额外训练成本即可获得的教师池；问题变成如何用 OPD 挖掘这个池子。
4. 挖掘有两个难点：教师选择（没有 checkpoint 全面占优，互补性因问题而异）和信任域（教师只在局部更优， indiscriminate distillation 会覆盖学生已掌握行为）。

【核心观察】
论文的核心经验发现可以概括为三句话：第一，best-validation checkpoint 不是能力超集；第二，教师总体准确率高不等于对学生剩余失败的覆盖高；第三，教师只在局部优于学生，因此蒸馏必须带位置选择。Figure 4(b) 被用来展示教师强度与互补性的错位，Figure 3(a) 被用来展示教师选择随学生能力变化而变化。

【方法主线】
1. 学生：best-validation checkpoint。教师：同一次 RL 运行中保存的所有其他 checkpoint，全部冻结。
2. HCN（Highest Coverage Next）：根据教师对学生当前未解问题的加权覆盖度动态选择教师。它不是选平均最强，而是选当前最能补学生短板的教师。...**

# Multimodal Models & Visual Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Multimodal Models & Visual Reasoning
- 方法：generation, reasoning, vision, stat-ml, stat-me
- 论文/报告：3 篇
- Parse Before You Execute: Efficient Spatial Reasoning with Executable Primitives
- R$^2$-Omni: Reducing Structural and Precision Redundancy in Omni-LLMs for Long-Form Generation
- Open Vision Banana: A Fully Open Recipe for Generative Visual Perception
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

<!-- paperflow:7266cd97ae5c951e -->
## Open Vision Banana: A Fully Open Recipe for Generative Visual Perception

[[Deep Reading - Nov 2026/Open Vision Banana-A Fully Open Recipe for Generative Visual Perception|Deep Reading]]

[https://arxiv.org/pdf/2611.15844v1](https://arxiv.org/pdf/2611.15844v1)

- **【一句话定位】
Open Vision Banana 是一篇“开放配方+系统分析+测试时扩展”的工作，而不是单纯提出一个新模型。它的起点是近期关于生成式视觉感知的观察：预训练图像生成器在合成图像之外，似乎编码了可用于分割、深度、法向等任务的现实世界知识；Vision Banana 已经把视觉任务改写为条件图像编辑，并展示了可行性，但其数据、权重、代码和实现细节不公开。OVB 的目标是把这条路线变成可复现的公共基线：用 4B/9B 开放权重生成器、2.1M–3.8M 公开数据对，训练一个统一的多任务感知系统，并系统回答“哪些设计选择真正影响泛化”。

【方法主线】
方法上，OVB 把每个任务都变成“输入图像+任务提示→输出编辑图像”。语义分割时，提示包含类别—颜色图例，要求模型按类别给像素上色；深度时输出场景保持的灰度反深度图；法向时输出法向图。提示模板从十种模板中等概率采样，任务标签显式写入，图例把类别映射到颜色。训练配方涉及颜色编码、提示设计、参数适配、任务平衡以及是否混入生成数据。论文特别强调这些选择彼此交互：单独消融有效，不代表组合后仍然有效。推理阶段提出 legend ensembling：用多个不同的类别—颜色图例分别分割同一图像，解码后逐像素多数投票，以纠正不稳定的颜色—类别绑定；这是一种无需重训练的测试时扩展。

【实验主线】
实验上，OVB 在 Cityscapes 零样本语义分割达到 73.9 mIoU（9B）和 71.3 mIoU（4B），超过 Vision Banana 报告的 69.9；非零样本配方分别再提升 5.1 和 1.8 个点。它在指代/推理分割上距 Vision Banana 不足 5 个点，但实例分割仍明显落后；深度可媲美先进仿射不变估计器；图像编辑能力基本保留。消融显示生成数据比例对法向和深度的影响非单调，移除生成数据在法向上误差最低但深度变差；参数适配中仅冻结嵌入层可能最优（片段证据，需核对）。Legend ensembling 在 Cityscapes 上最高 +8.7 mIoU、COCO-Object 上 +15.0 mIoU...**

# Reward Models & Reinforcement Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Models & Reinforcement Learning
- 方法：agent
- 论文/报告：2 篇
- Do Visual RL Gains Scale at Test Time? A Quantile View of Post-Training
- CARE-PO: Controllable Tool-Call Efficiency for LLM Agents
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

<!-- paperflow:2922909c915ebc6d -->
## CARE-PO: Controllable Tool-Call Efficiency for LLM Agents

[[Deep Reading - Nov 2026/CARE-PO-Controllable Tool-Call Efficiency for LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2611.12125v1](https://arxiv.org/pdf/2611.12125v1)

- **一、论证主线。论文指出 LLM agent 的外部工具调用是主要运行成本，但训练目标几乎只奖励任务完成，导致策略倾向于用更多调用换成功率。作者没有直接假设“调用越少越好”，而是先在 GAIA 上做步骤级诊断：对两个规模 Qwen3.5 的轨迹逐步骤检查，发现大多数轨迹含可删除调用，且两个规模比例相近。由此论文主张，工具冗余是训练目标与策略行为共同造成的普遍问题，不能只靠模型变大解决。
二、对现有方法的批评。作者把干预分为推理时约束、离线训练和在线 RL 三类，重点批评在线 RL 中的效率奖励：OTC-PO 用组内正确轨迹的最短调用数作锚，锚随策略移动；TEPO 用熵降低型调用比例，没有任务级参考；两者都只产出一个操作点。更严重的是，效率奖励可能把策略推向完全不调用工具的坍缩解，作者在 27B baseline 观察到该退化；同时若效率项抹平组内分数，GRPO 会失去梯度。
三、技术主线。CARE-PO 的三个组件逐一对应上述问题。第一，效率奖励不再相对策略自身 rollout 计价，而是相对每个任务按难度锚定、训练前固定的 reference budget；价格外生且稳定。第二，在每个 online GRPO rollout 组中加入训练数据里的成功源轨迹作为第 G+1 个成员，并对轨迹未收集到的证据计费（coverage）；零调用正确轨迹也要按一个 reference budget 计费，从而不再是免费捷径。第三，用 prompt 中的 effort tag 条件化成本曲线，学一个 πθ(·|x,e)，让单次训练覆盖低 effort 的效率点与高 effort 的能力点。
四、实验主线。诊断之外，主实验覆盖 4B/9B/27B 三个规模，在 GAIA 与四个 held-out 多跳基准上比较 base、OTC-PO、TEPO 与 CARE-PO 的 low/high 操作点；训练和评测都用 live web search，GAIA 三次运行报均值与标准差，多跳基准 greedy。GAIA 上六个操作点中五个同时在成功率和调用数上优于 base；held-out 结果依...**

# Data, Benchmarking & Evaluation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data, Benchmarking & Evaluation
- 方法：agent, language, reasoning, audio, stat-ml, stat-me
- 论文/报告：9 篇
- PianoBench: Evaluating Musical Perception and Reasoning in Language Models
- Blueprint2CAD: Benchmarking Foundation Models on Engineering Drawing-to-CAD Render Correspondence
- VisCodeBench: a new benchmark dataset for Text-to-Vis reflecting real-world practice
- Omni-Life: Benchmarking Omnimodal Real-Time AI Assistants in People's Daily Lives
- Do Agents Still Follow the Rules? Benchmarking Rule Compliance under Agent Context Compaction
- Machine Hearing: Can Audio LLMs Generalize Beyond Human Auditory Perception?
- HumanSightBench: A Benchmark for Human Visual Perception in MLLMs
- AutoBuddy: Evaluating Proactive, Always-On Agents on Task Discovery and Execution in a Simulated Workplace
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

<!-- paperflow:7d2462da1752a7b4 -->
## Do Agents Still Follow the Rules? Benchmarking Rule Compliance under Agent Context Compaction

[[Deep Reading - Nov 2026/Do Agents Still Follow the Rules-Benchmarking Rule Compliance under Agent Context Compaction|Deep Reading]]

[https://arxiv.org/pdf/2611.03223v1](https://arxiv.org/pdf/2611.03223v1)

- **论文研究一个具体但被忽视的 agent 安全问题：长程 LLM agent 为了适配上下文窗口，会把历史压缩成摘要；压缩后的上下文是否仍然携带治理后续动作的操作规则？如果规则不再携带，或者携带了但 agent 不遵守，违规就从这里发生。作者指出，已有工作虽然发现压缩后违规上升，但有两个方法论缺口：第一，合规测量发生在完整 agent harness 之外，缺少真实工具调用循环；第二，没有识别每次违规的来源，无法区分规则被删除、被改写、仍在但被忽略、被后续请求误当授权，还是规则投递通道本身有问题。为此，论文提出 ARCBench，一个建立在开源 pi agent harness 上的可配置 benchmark。它的核心设计是把 agent 放进工具调用循环，从执行过程中的动作与参数测量规则合规，而不是只评估模型的口头回答；同时为每个压缩运行匹配全上下文基线，并标注规则在压缩上下文中的状态，从而把违规归因到来源。

ARCBench 的案例不是任意规则问答，而是满足准入条件的可执行场景：违规要服务于具体攻击者目标（A1），要能从工具调用及参数判定（A2），规则在场景中要可信（A3），片段还提到 A5；候选案例若失败 A1 或 A5 会被排除。压缩配置包括七种生成式策略，为较早轮次写替换文本；Pi 与 Pi+RP 使用 pi 原生压缩路径，Pi+RP 额外加入指令。压缩器 prompt 要求保留事实连续性、不得编造或改变状态变量、保留 exact argument values、参考 critical context，摘要长度受 max_chars 与预算 B 限制。评测使用 violation@k，只统计有 k 次有效运行的设置，并与 matched full-context runs 比较；同时记录每个压缩上下文的规则状态，把“规则仍在但违规”和“规则被删除后违规”分开。攻击方面，compactor-targeted injection 在压缩前向工具结果注入内容，诱导摘要器改写、删除或降权规则；防御则试图在摘要中恢复规则或加入规则存在性提示。

实验覆盖四个模型、多种压...**

<!-- paperflow:4c0c523b2d45c17f -->
## Machine Hearing: Can Audio LLMs Generalize Beyond Human Auditory Perception?

[[Deep Reading - Nov 2026/Machine Hearing-Can Audio LLMs Generalize Beyond Human Auditory Perception|Deep Reading]]

[https://arxiv.org/pdf/2611.10693v1](https://arxiv.org/pdf/2611.10693v1)

- **一、论文的主问题：论文问的是 Audio LLM 能否把感知范围从人类可听声音推广到更广义的机器听觉。作者指出，现有多模态 LLM 的听觉能力主要围绕语音、音乐和环境声，这些都属于人耳可听、以人为中心的音频；而物理世界充满波、振动和振荡，从地震、机器振动、心音、水声、动物叫声到无线电、雷达和天文光变曲线，频率、时长、通道和信噪比高度异质。人耳 20 Hz–20 kHz 只是其中很小一段，因此需要系统评估 ALLM 能否处理远超人类听觉范围的信号。
二、benchmark 设计：作者构建 MHB，包含 6,128 个实例、13 个任务、7 个领域。输入覆盖约 10^-5 到 10^6 Hz 的采样率、10^1 到 10^6 个采样点的序列长度，以及单通道和多通道输入。领域包括光变曲线、地震波、工业振动、心音、水声、动物叫声、无线电波等；任务示例包括无线电调制方式分类、雷达脉冲段波形类型分类、水下船只分类。这个设计的核心不是单一任务 SOTA，而是用极端异质的输入 regime 对现有音频前端形成压力测试。
三、评测与诊断：论文先评测当前开源与专有 ALLM 的零样本表现，结果显示迁移有限，部分模型在部分任务上出现 0.00。随后用机器听觉数据做监督适配，性能提升但不能完全闭合差距。作者据此提出全文最关键的判断：问题不只是数据暴露不足，而是现有音频前端与机器听觉输入之间存在系统性失配。换句话说，给模型更多领域数据并不能自动让它“听见”这些信号，前端的归纳偏置才是瓶颈之一。
四、领域基础模型对照：论文进一步冻结评估 Time-MoE、SpectrumFM、SeisLM、FISHER、SPEAR 等领域基础模型。结果显示领域专精并非万能：FISHER 在部分制造任务上强，但在多个其他任务上出现 0.00；SeisLM 在 Iquique 上领先但在 Llaima 上不是第一；SpectrumFM 在两个电磁任务上都没有排第一；反过来，SPEAR 在多个非传统音频任务上仍有竞争力，Time-MoE 在 LEAVES 和 RadSeg 上领先。作者由此论证，预训练领域与目标任务的对...**

<!-- paperflow:deb3e8562c8eb60c -->
## HumanSightBench: A Benchmark for Human Visual Perception in MLLMs

[[Deep Reading - Nov 2026/HumanSightBench-A Benchmark for Human Visual Perception in MLLMs|Deep Reading]]

[https://arxiv.org/pdf/2611.12305v1](https://arxiv.org/pdf/2611.12305v1)

- **一、定位。HumanSightBench 不是新模型，而是一件测量工具：它把「人类可解」这件事从数据集级的平均基准下沉为条目级的准入条件，从而把一个长期被容忍的观察——模型在人类易题上翻车——变成可量化、可归因、可复现的评测结论。
二、动机链。作者先承认 MLLM 在知识密集、多步推理型视觉语言任务上的进展（chart 解读、大学水平题目），随即指出这些成绩不能证明基础视觉能力：数物体、沿线追踪、读嘈杂标签、辨别镜像图案仍是弱项。接着指出比较 MLLM 与人类感知为何困难——端到端评测把多种技能耦合成一个分数；单条题目本身是否有效也不确定，错误可能来自视觉感知、语言先验、背景知识、问题歧义或答案未定义。若这些混淆不受控，所谓「感知失败」可能只是「题目失败」。于是全文立论落到一句话：错误必须在条目层面可归因。
三、构造方法。其准则为 human-easy by construction, model-hard by evaluation。流水线分三段：图像来源（许可与合规、去隐私）→ Q-A 构造（单图 + 短答 + 核验参考答案，条目记为 (I,q,y)）→ 筛选（人类验证 + 模型筛查）。筛选由五道闸门实现：G1 问题清晰、G2 答案唯一、G3 视觉接地（答案必须落在图中可定位证据上）、G4 人类可解、G5 纯文本捷径过滤（不给图就该答不出）。任一不过即淘汰。产物为 2,876 道短答案题，覆盖 7 个一级感知类别与 21 个二级子类；类别按视觉操作而非应用场景组织，可辨识的包括 OCR、Structured Visual Perception、Semantic Recognition、Fine-grained Discrimination、Space Understanding、Counting。数量上 OCR 约 1,318 题、Structured Visual Perception 约 722 题，两类合计约七成，其余五类各约 118–201 题——这意味着总体分由大类主导，类别级诊断必须单独看。
四、评测协议。八个模型（闭源 GPT-5.6-Sol、GPT-...**

<!-- paperflow:aa90af7347b95fa9 -->
## AutoBuddy: Evaluating Proactive, Always-On Agents on Task Discovery and Execution in a Simulated Workplace

[[Deep Reading - Nov 2026/AutoBuddy-Evaluating Proactive, Always-On Agents on Task Discovery and Execution in a Simulated|Deep Reading]]

[https://arxiv.org/pdf/2611.12393v1](https://arxiv.org/pdf/2611.12393v1)

- **一、论文要解决什么
论文把 agent 的默认设定从“被动接单”推到“常驻在场”。摘要明确批评现有 benchmark“usually give the agent the task to perform”，而知识工作中的请求“sit in the user's workspace”，agent 必须先识别“which obligations exist”。因此它定义了一个新任务：主动式任务发现与执行——从邮件、聊天、日历、文档、业务记录的状态推断用户未说出口的待办，并在义务真正发生的系统里完成写操作。这一设定要求评测同时覆盖“发现”与“执行”两端，而现有工作只覆盖其中一端，论文的判断是“evaluate important parts of this problem but not holistically”。

二、怎么造这个世界（技术主线）
AutoBuddy-Bench 的构造是四段流水线，摘要层面可确认：
1. 世界建模：把组织的 employees、projects、business systems、event history 组织为 temporal graph。时间被显式建模，因为义务是随时间由事件触发的。
2. 渲染：把图渲染成 grounded、realistic 的 workspace artifacts（邮件/聊天/日历/文档/业务记录）。grounded 保证每条 artifact 可回溯到图实体，跨文档一致，从而使“任务自然涌现”成立。
3. 任务派生：从“产生义务的事件”派生 ground-truth 任务——题目不是人写的，是事件类型的函数。
4. 判分：每个任务的 outcome 由 world state 计算，而非文本匹配或模型评审。
这套设计的核心洞见是：把“任务与答案的正确性”锚在事件图上，把“观测的噪声与体量”留给渲染层，于是高噪声与精确 ground truth 可以同时成立。我认为这是本文最可复用的方法论贡献。

三、agent 侧（需回原文确认的边界）
从摘要与图 1 的命名可推断：AutoBuddy 既是论文给出的参考...**

<!-- paperflow:448d5325fffa75fa -->
## LAION-Audio-XL: A Web-Scale Corpus and Scaling Studies for Contrastive Language-Audio Pretraining

[[Deep Reading - Nov 2026/LAION-Audio-XL-A Web-Scale Corpus and Scaling Studies for Contrastive Language-Audio Pretraining|Deep Reading]]

[https://arxiv.org/pdf/2611.11592v1](https://arxiv.org/pdf/2611.11592v1)

- **论文的主线是“用更大、更干净、带描述的音频-文本语料，研究 CLAP 在给定算力下的缩放规律”。作者首先指出，CLAP 已从 LAION-CLAP 的约 63 万对、M2D-CLAP 的 260 万对、MS-CLAP 2023 的 460 万对，发展到 SLAP 的 1.09 亿对；模型在分类与检索上的提升与数据规模高度相关。但与图像-文本领域的 CLIP 缩放研究相比，音频-语言领域仍缺少公开的 web-scale 语料，也缺少在统一协议下回答“给定算力该把预算给模型还是给数据”的实验。论文由此构建 LAION-Audio-XL：从学术数据集、互联网数据集、网页集合、Common Crawl 与 YouTube 收集 1.48B 音频样本，经过多阶段策展管线进行过滤、去重、配额与质量控制，并为缺少描述的样本生成短描述和长描述。检索片段显示，数据配比会按 speech、music、其余类别进行配额调整，某一阶段对应约 244.2M、191.4M、224.4M 的分配，且未用完的组内预算不转移到其他组；这说明论文不仅追求“多”，也试图控制语音、音乐与一般声音的类别平衡。
模型侧，论文采用 NaFlex 作为音频编码器，训练 CLAP2 系列，在统一对比学习框架下变化模型尺寸与训练样本数，形成缩放实验。最终基准比较使用五个模型尺寸，均在 1,024M 样本上训练；缩放拟合覆盖约 5×10^18–5.5×10^20 FLOPs 的计算范围。拟合结果显示，计算最优的模型参数 Nopt 与计算量 C 的关系约为 Nopt ∝ C^0.405，最优训练样本数 Dopt ∝ C^0.583；后者增长更快，二者指数比约 1.44，因此作者总结为“最优样本数约以 1.4 倍速度增长”。这意味着在固定算力下，把预算更多分配给数据、使用较小模型，可能比单纯增大模型更划算。论文还用 bootstrap 重采样给内部最优点置信区间，并指出部分最优点落在实验网格边界，作者自己将结果定性为 indicative rather than definitive。
实验侧，论文在 MAEB 与 HEAR...**

# World Models, Generation & Audio

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：World Models, Generation & Audio
- 方法：audio
- 论文/报告：2 篇
- WanderBench: Can World Models Explore Dynamic Worlds from Video?
- AudioCodex: Streaming Autoencoders for Live Audio
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:6015c3b01614d1ee -->
## WanderBench: Can World Models Explore Dynamic Worlds from Video?

[[Deep Reading - Nov 2026/WanderBench-Can World Models Explore Dynamic Worlds from Video|Deep Reading]]

[https://arxiv.org/pdf/2611.12557v1](https://arxiv.org/pdf/2611.12557v1)

- **一、研究动机与问题
论文从视频世界模型的范式转变写起：早期视频生成关注固定长度片段的视觉质量、运动真实感和时序连贯；近期研究转向交互式世界模型，用户可在生成过程中持续发出相机或动作控制，探索不断展开的视觉世界。这类模型可用于游戏创作、虚拟制作和机器人策略学习。但大多数世界模型以单图或文本提示为条件，场景动态完全由模型想象，不对应真实发生过的事件。视频条件世界模型则提供另一种可能：给定真实录制的动态场景视频，沿新相机轨迹重新观察，支持重取景、自由视点回放和 real-to-sim。论文的核心问题是：现有模型能否在探索动态世界时保持对源视频的忠实？现有基准从单图/文本开始，视频条件生成主要评估感知质量和相机跟随，不直接测主体身份、事件进程和重访一致性，因此需要新基准。
二、WanderBench 的构建
作者提出 WanderBench：745 段单镜头源视频，真实与 AI 生成内容混合，分三个难度层级，总时长 3–25 s；每条视频配对以键盘动作给出的相机路线；评估覆盖六个维度、24 个经人类验证的子指标。六维是 Visual Quality、Camera Control、Source-World Fidelity、Source-Dynamics Fidelity、Spatiotemporal Consistency、Dynamic Revisit Consistency；Overall 为等权平均。Table 2 显示 Simple 层为 3–8 s、451 段；检索片段还给出子指标例子：Synchronized View Fidelity 将同一时刻源帧重投影到实现视图，与输出在共视区域比较；Source-Dynamics Fidelity 下设 Motion Timeline Agreement。基准的评估协议强调：所有模型接收相同源视频和动作；动作转换为相对源相机的每帧姿态，通过模型原生条件接口、分辨率和帧率传入；不同帧率重采样；frame-limited 模型只评估开头到其最大帧数。
三、评估模型与主结果
作者评估八个视频条件世界模型，按生成长度分两组：full...**

<!-- paperflow:c7edb219b4a7d2fa -->
## AudioCodex: Streaming Autoencoders for Live Audio

[[Deep Reading - Nov 2026/AudioCodex-Streaming Autoencoders for Live Audio|Deep Reading]]

[https://arxiv.org/pdf/2611.12375v1](https://arxiv.org/pdf/2611.12375v1)

- **一、论文要解决的问题。AudioCodex 面向实时生成音频应用：对话语音、音乐伴奏、交互式合成等都要求系统严格因果、可流式运行，不能依赖未来帧。现有因果音频自编码工作多集中在语音，且通常输出离散 token，适合 next-token prediction，却不太适合连续空间的扩散/流匹配生成框架；连续音频自编码器虽然提供了连续潜变量，却存在重建-生成权衡：潜变量越保真越难建模，越正则化越容易丢细节。论文要在这两组矛盾之间找到一个可落地的表示层。

二、技术主线。AudioCodex 是一个完全因果的连续立体声自编码器，把 48 kHz 波形压缩成约 10 Hz 的潜序列。它不在波形或复数 STFT 上直接自编码，而是在归一化对数幅度谱上训练：编码器输出连续潜变量，经过超球面投影和有界噪声增强；再用冻结自监督音频编码器的 REPA 对齐，把语义/结构信息注入潜空间。幅度解码器被训练为 Mean Flow 模型，学习区间平均速度，实现单步解码；训练使用自适应 MSE 和感知频率加权，并采用 R3GAN 风格的对抗训练，主判别器加辅助判别器作用在功率压缩的 mel 幅度上，最佳消融配置为 γ=0.4、λ_aux=1。相位不进入自编码器训练，而由独立的因果对抗相位估计器从解码幅度恢复，最后经逆 STFT 合成波形。

三、为什么这样设计。把相位外置，是为了避免把高频、非凸、对噪声敏感的相位灌入自编码器，让编码器专注于较稳定的幅度结构；用 Mean Flow 而不是多步扩散，是为了满足实时单步解码，并利用平均速度参数化获得更稳定的训练；用超球面投影和有界噪声，是为了控制潜空间范数、平滑几何、提升下游可生成性；用 REPA，是为了把自监督教师的结构信息注入潜变量，缓解‘保真潜变量难生成’的问题；用主+辅助判别器，是为了在感知层面补足幅度解码的细节。

四、实验主线。论文做了三类实验。第一类是消融：相位估计器是否因果、主/辅助判别器组合、mel 功率压缩 γ、辅助判别器权重 λ_aux、Mean Flow 的 α 参数、REPA、超球面投影与有界噪声等。表 1 报告 rFAD，最...**

# Research Agents & Scientific Discovery

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Research Agents & Scientific Discovery
- 方法：agent, ai-for-science, science-discovery, gui-agent, stat-ml, stat-me
- 论文/报告：2 篇
- SciMARS: Multi-Agent Recursive Self-Improvement for Closed-Loop Scientific Discovery
- Sage-Taiyi: A Metacognitive Architecture for Scientific Discovery
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:7261866c6eeff81f -->
## SciMARS: Multi-Agent Recursive Self-Improvement for Closed-Loop Scientific Discovery

[[Deep Reading - Nov 2026/SciMARS-Multi-Agent Recursive Self-Improvement for Closed-Loop Scientific Discovery|Deep Reading]]

[https://arxiv.org/pdf/2611.12552v1](https://arxiv.org/pdf/2611.12552v1)

- **一、研究定位。论文面向闭环科学发现中的多智能体系统，主张 agent 的改进不应只是一次性的 prompt 或 workflow 搜索，而应是在任务内、随实验证据递归进行的 harness 适应。所谓 harness，在论文语境中包括角色职责、角色间交接与协调方式以及记忆策略；SciMARS 的目标是让这些组件在每一轮证据到来后被编辑、测试与组合，而不是固定执行。二、问题诊断。作者把现有系统的失败归为两类。第一是证据隔离：观察、失败检查与约束留在单个 agent 的历史里，无法到达依赖它的另一个 agent，导致重复被拒绝的实验或遗漏验证者发现的约束。第二是固定执行规则：即使证据被保留，角色流程、handoff 与记忆策略不变，规划者、编码者、验证者仍可能重复错误。论文特别强调“保留证据”不等于“使用证据”，因此共享记忆与程序适应必须分开处理、联合设计。另一个背景是科学发现的实验预算有限，ASB 的 campaign 只允许若干批设计，系统必须在少量轮次内利用证据，而不是无限试错。三、方法。SciMARS 由三部分组成。其一是共享证据图：记录观察、发现、计划与未解决问题，带 provenance，并通过检索与订阅把记录路由到当前 handoff 之外的角色。其二是分离式组件编辑器：角色与协调分别编辑，避免单体式重写；SciMARS + Memory 增加第三个记忆编辑器，专门调整 agent 如何记录、检索与接收证据。编辑器可以选择不改，证据不足时保留原 harness；角色契约中的 endpoint guidance 约束编辑边界。其三是候选探针：生成并测试候选修改，下一版 harness 由当前 harness、角色级候选与输出、任务级信号组合而成，论文给出 HSt = Compose(Ht, {Hjt, ojt}j∈A, o0t, Lt) 形式的更新；协调修改必须来自被测试的选项，启用记忆时同样约束记忆修改。这个“测试后再组合”的机制是自修改的安全阀。四、实验。论文在三个 benchmark 上评估：Autonomous Science Bench 的六个 cam...**

<!-- paperflow:4bd3dd88ec8b1d55 -->
## Sage-Taiyi: A Metacognitive Architecture for Scientific Discovery

[[Deep Reading - Nov 2026/Sage-Taiyi-A Metacognitive Architecture for Scientific Discovery|Deep Reading]]

[https://arxiv.org/pdf/2611.12762v1](https://arxiv.org/pdf/2611.12762v1)

- **一、论文的论证主线
论文从一个价值判断出发：科学不是知识集合，而是思维方式（引 Carl Sagan）。由此引出对科学发现的过程刻画——发现是一个迭代回路：搜索产生并检验想法，理解通过对结果的解释而生长，理解再引导后续搜索。研究者在假设空间与实验空间之间往返，提出解释、设计检验、解释来自实验／计算／观测的反馈；解释反馈的过程形成一个「工作性理解」：哪些判断被支持、哪些冲突未解、结论在什么条件下成立、哪些规律可跨案例迁移。理解一旦改变，研究者就会改写假设、重述问题、重设实验、重排方向优先级。

论文随后引入经典问题解决研究作为形式化资源：表征、算子、启发式共同组织搜索并引导后续行动。于是核心设计问题被精确表述为：如何把搜索过程中发展出的理解保留下来，并用它去选择与适配表征、算子与启发式，使搜索经验影响后续研究决策。这是一个控制问题，而非生成问题。

二、技术主线
Sage-Taiyi 的答案是把科学发现拆成两层，并用元认知的「监控—控制」回路连接。

对象层（object level）承担搜索：给定研究问题 q，状态记为 X_t = (T_t, G_t, E_t)。Target Tree T_t 组织目标、子目标与局部构造；搜索以局部变换的方式在知识状态间移动。图中可见的组织层级是：目标树的多分支（t1…t5）→ 每分支的局部策略（s1…s8）→ 更下层的子想法（p1…p8）。配套的行动范式包括并行分支探索、状态引导搜索、迭代反馈与嵌入式方法精炼。

元层（meta level）承担调控，由两个组件构成。其一是 Cognitive State：对对象层科学理解的选择性、可修正表征，包含「选定的科学理解」（已支持发现、证据、工作假设）与「监控评估」（支持度与不确定性、类比、逆向设计、共享依赖、假设与适用范围、搜索含义）。其二是 Cognitive Repertoire：可复用推理方法的库，控制的动作之一就是「选择适用的条目」。Method 中明确给出三类算子：类比；逆向设计（从期望结果反推条件与子目标）；组合式综合（把目标分解为功能 → 选择并集成兼容组件 → 评估整体系统...**

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
- 方法：agent, generation, language, vision-language-model, vision, vision-language, stat-ml, stat-me
- 论文/报告：4 篇
- Can Personal Requirements Become Supervision for Planning?
- MIRIX-2.0: Multi-Agent Memory System
- Can LLM Agents Organize Their Own Memory?
- MIGRO: A Decoupled and Portable Long-Term Memory System for Language Models
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

<!-- paperflow:0143e0b3f3cecdfe -->
## MIRIX-2.0: Multi-Agent Memory System

[[Deep Reading - Nov 2026/MIRIX-2.0-Multi-Agent Memory System|Deep Reading]]

[https://arxiv.org/pdf/2611.12068v1](https://arxiv.org/pdf/2611.12068v1)

- **一、问题与动机。论文的主线是：LLM agent 的工作时长已经远超单次 context window，长期记忆需要让 agent 个性化、与用户已告知内容保持一致、复用过去有效的经验；但现有记忆系统在准确、实用、高效三者之间总要放弃至少一个。parametric memory 需要训练；consolidation 成事实或摘要会永久丢失细节；保留全部记录则搜索空间随历史增长。论文把“准确”进一步拆成具体事实（名字、日期、计数、事件区分）和事实间连接（谁在何时以何角色对谁做了什么），把“实用”拆成不训练、可追溯、整理不破坏记录，把“高效”拆成延迟和 token 有界。

二、方法主线。MIRIX-2.0 采用存储与索引分离：typed evidence plane 保存完整、类型化的记忆记录，是 system of record；provenance hypergraph 索引实体、事件、关系、角色和时间，图上的每个事实引用来源 row。系统有六个 typed memory agents，但给定片段未列出六类全名，至少可见 episodic、semantic、procedural 三类。三条路径分别是写、读和 AutoDream：写路径先存记录再建索引；读路径先规划查询、遍历图、再在显式预算下只解引用被选中的记录，因此单题检查的证据量不随历史增长；AutoDream 在写路径之外运行。语义记忆上，AutoDream 只合并重复 anchor，不创建可能掩盖输入的摘要，也不改写任何记录，所以它改变的是证据如何连接，而不是证据是否存在。程序性记忆上，AutoDream 判断每个 pending session 的结果，把成功经验和失败教训蒸馏成引用原始 experiences 的 skills。系统不微调，兼容闭源/开源模型，代码公开，同一套存储服务用户记忆和 agent 记忆。

三、实验主线。评测分三块：用户记忆准确率（conversational recall、document QA、personalized generation）、agent 程序性记忆（held-...**

<!-- paperflow:08b02381445d3cb3 -->
## Can LLM Agents Organize Their Own Memory?

[[Deep Reading - Nov 2026/Can LLM Agents Organize Their Own Memory|Deep Reading]]

[https://arxiv.org/pdf/2611.11929v1](https://arxiv.org/pdf/2611.11929v1)

- **## 一、论证主线

论文从一个几乎无法回避的工程事实出发：**大多数 LLM agent 在部署之后权重是固定的**，因此任何新获得的信息都必须存放在外部记忆中。外部记忆的价值被明确列出：让 agent 个性化回复、延续此前的活动、并使用通过经验积累的背景知识（摘要引用 Packer et al., 2023；Zhong et al., 2024；Park et al., 2023）。

接着论文把镜头推到「跨会话」这一场景：当对话跨越多个 session 不断延长，持久上下文里会出现一类特殊信息——**当前请求中并未出现、但解释该请求所必需的事实与事件**（引用 Maharana et al., 2024；Wu et al., 2025）。这类信息使早期交互的内容与模型预训练知识同时可用，也正是「记忆」区别于「检索」的地方：它需要在请求到来之前就已经被妥善安置。

由此引出对现有方法的批评：现有做法往往依赖**预定义的记录（predefined records）与固定或既定的组织规则（fixed organizational rules）**，这种结构无法随任务与交互的演化而调整。批评的落点不是「检索不准」，而是「结构僵化」。

于是提出研究问题：**can LLM agents organize their own memory using ordinary documents?** 这个问句包含两个主张：其一，组织的主动权应交给 agent；其二，载体应是「普通文档」而非专用数据结构。

论文还给出了一条贯穿全文的设计张力：**完整历史保真但需要 agent 自己定位相关交互；紧凑记录虽减少需检查的文本量，却可能遗漏日后所需的限定条件或上下文**。这条张力不是被「解决」，而是被「重新分配」——用摘要承担压缩，用链接承担保真。**

<!-- paperflow:afb567840e51480b -->
## MIGRO: A Decoupled and Portable Long-Term Memory System for Language Models

[[Deep Reading - Nov 2026/MIGRO-A Decoupled and Portable Long-Term Memory System for Language Models|Deep Reading]]

[https://arxiv.org/pdf/2611.15315v1](https://arxiv.org/pdf/2611.15315v1)

- **这篇论文的主线可以概括为一句设计命题：记忆应该比模型活得更久。作者观察到，长期交互中积累的观察、修订和偏好不会因为模型升级而失效，但现有记忆通常绑定在某个模型的上下文、参数、嵌入空间或 tokenizer 上；换模型就要重放、重训或重建，成本随历史增长，而且摘要式记忆会在写入时丢掉未来可能有用的证据。MIGRO 因此把长期记忆重新定义为跨模型持续存在的共享经验状态，并把“保存”和“访问”拆开。

技术主线建立在两个原则之上。第一是 asymmetric fidelity：证据层尽量精确，保留原始观察、修订及其历史关系；索引层只做近似语义组织，用共享语义索引支持召回。第二是 deferred selectivity：不在写入时决定什么重要，而是等 query 出现后再从累积状态中构造有界证据包。这个组合的意图很明确：避免有损摘要的不可逆损失，同时让检索承担选择责任。系统还包含两个工程支柱：由于不同模型在词表、分词边界和嵌入空间上不同，宿主专用接口把选中的证据转换成接收模型可消费的输入；可逆编码降低归档存储，同时支持精确恢复。共享状态与访问接口让 successive hosts 可以读取并继续扩展同一经验，而不是各自维护私有记忆。

实验主线围绕三个问题展开。第一，保留的证据能否变成有效答案：论文在 LoCoMo、LongMemEval-S、MemOps、MemoryAgentBench 等上做质量比较与访问消融。第二，经验能否跨宿主存活并继续增长：论文设计 Same-host、State Transfer、Full-History Rebuilding 三条路径，比较交接后继续写入与目标端全历史重建。第三，精确持久化能否经济扩展：论文在百万 token BEAM 历史上测量质量—存储配对、私有内存、共享资产和避免的 memory-building forwards。评测覆盖对话回忆、跨会话推理、演化记忆操作、增量交互和长历史，并用 MemoryAgentBench 四能力宏平均补充能力维度。

关键发现有三层。第一层是质量：在匹配 LoCoMo 状态上，MIGRO 比最...**

# AI for Science & Biology

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science & Biology
- 方法：protein-folding
- 论文/报告：3 篇
- Are Scaling Laws of Universal MLIPs Universal?
- HyInv Technical Report
- HyPLM Technical Report
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:15492d62a27c2ff3 -->
## Are Scaling Laws of Universal MLIPs Universal?

[[Deep Reading - Nov 2026/Are Scaling Laws of Universal MLIPs Universal|Deep Reading]]

[https://arxiv.org/pdf/2611.15603v1](https://arxiv.org/pdf/2611.15603v1)

- **一、论文要解决的问题
论文从一个非常具体的资源分配问题切入：在通用机器学习原子间势（uMLIP）的训练中，DFT 计算昂贵，那么应当如何把有限的计算预算分配给数据生成，才能最大化模型预测性能？作者指出，神经 scaling law 通常承诺“数据量增加 → loss 按幂律下降”这样的可预测收益，从而支持“按结构数外推”的预算规划。但 MLIP 数据与 NLP/视觉数据存在两个结构性差异：一是数据高度异质，化学与结构空间天然不均衡；二是数据生成代价极高且不均匀。于是“结构数”这一标量可能不是合适的 scaling 自变量——同样多的结构未必包含同样多的有用信息。据此，论文把标题设为一个带有挑衅意味的问句：uMLIP 的 scaling law 是“通用”的吗？
二、技术路线与实验设计
作者的策略是做一次高度受控的“数据侧”因子实验。首先固定模型配置（不随数据量调整容量与超参），以排除容量-数据配比这一混淆因素，把差异归因于数据量与数据构成。其次构造统一训练池：MatPES 与 MP-ALOE 合并为 1.16M 结构，所有采样策略都在同一候选池上操作，从而保证策略间可比。第三，建立规模网格：从 10,000 到 640,000 的多档子集，并额外加入全量 1.16M 设置。第四，模型维度覆盖 M3GNet、MACE、PET 三种架构，使“架构依赖”这一结论具备一定代表性。第五，采样策略维度包含六类：均匀随机采样作为基准；三种基于化学与结构信息的离散分层（stratification）策略；迭代 DIRECT（聚类后在簇内按近似等间隔 rank 选样，受簇大小约束 N_DIRECT ≤ kC ≤ |S|）；以及基于三种预训练 MLIP 嵌入的最远点采样（FPS），从而把“嵌入空间的选择”本身也作为一个受控变量。第六，评测采用双测试分布：（a）来自父分布的 held-out 测试集，衡量同分布泛化；（b）分布偏移的 MP-r2SCAN 测试集，衡量跨分布迁移。指标覆盖 cohesive energy、力、应力以及聚合 loss；聚合 loss 定义为 L_eval = L_...**

<!-- paperflow:48d8f89fd95444d3 -->
## HyInv Technical Report

[[Deep Reading - Nov 2026/HyInv Technical Report|Deep Reading]]

[https://arxiv.org/pdf/2611.11997v1](https://arxiv.org/pdf/2611.11997v1)

- **一、问题与动机
逆折叠是计算蛋白设计的核心任务：给定主链，设计能折叠回该主链的序列，服务于 scaffold 设计、酶工程和序列文库构建。论文强调实际筛选需要大量且多样化的候选，因此单条序列的最优性不是唯一目标，模型应在给定结构质量下提供尽可能高的多样性。既有 ProteinMPNN 类方法、语言模型辅助/并行解码方法和 ProteinZero 的 RL 路线各有优势，但单温度评估会混淆质量与多样性，且推理成本对大规模筛选仍然偏高。HyInv 由此把“匹配多样性下的 scTM”和“筛选级速度”作为两个主目标。

二、模型与训练
HyInv 是一个 1.96M 参数的主链条件图网络，编码器-解码器架构，通过消息传递编码主链几何环境，再逐位置输出氨基酸分布。训练由团队开发的科研智能体 Hyra 2.0 组织，分两阶段：第一阶段在天然结构和预测结构上做随机残基顺序的自回归序列建模，使模型适应任意解码顺序和任意 mask 组合；第二阶段用结构奖励做强化学习，对每个主链采样候选，用 ESMFold2 折叠，以 TM-align 得到的结构自一致性为奖励，用 RLOO 留一法基线降低方差，并加 KL anchor 约束策略。论文称奖励主要来自预测结构自一致性，训练池约 50,000（片段截断，具体含义需核对）。

三、推理与工程
推理采用分组 draft-and-rewrite 采样器：先以较高的 draft 温度 Td=0.6 生成草稿，铺开多样性；再以较低的 rewrite 温度 Tr=0.34 对一组位置做 mask 并重采样，其余位置保留为上下文，逐组迭代。这样把“探索”和“局部修正”解耦，在匹配多样性下提高 refolding TM-score。工程上，参考采样器对 402 残基主链生成 128 条序列约 2.6 s；跨候选共享工作后达到 8.7×（约 304 ms），且 FP32 路径能精确返回参考序列；进一步 BF16 优化达到 52×（50.6 ms），在单张 NVIDIA H20 上测量。

四、实验设计与结果
论文在五个面板上评估：CATH4.3 test（1,...**

<!-- paperflow:69cc50d6a4f92ef2 -->
## HyPLM Technical Report

[[Deep Reading - Nov 2026/HyPLM Technical Report|Deep Reading]]

[https://arxiv.org/pdf/2611.12000v1](https://arxiv.org/pdf/2611.12000v1)

- **论文的主线不是提出一个全新的蛋白质语言模型架构，而是围绕“600M 级编码器的训练与推理成本”做系统性的精度-效率重配。作者先给出成本背景：ESM-2 之后的 600M 级模型把预训练数据推到 4–6T tokens，单张 H20 上参考实现只有 48k–69k residues/s，扫一遍 2.6B 蛋白质需要八卡服务器两到三周。因此，问题被定义为：能否用更少数据训练一个 650M 掩码语言模型，并用面向目标 GPU 的推理栈，把精度维持在 ESM-2 650M/ESM C 600M/E1-600M 一档，同时显著降低训练与 serving 成本。

方法上，HyPLM-Flash 采用 ESM-2 风格的 33 符号字母表、32 层 Transformer、1280 隐藏维、20 个 64 维注意力头、SwiGLU 3584 维前馈、RMSNorm、QK-norm 与 RoPE，双向注意力，MLM 头输出 33 类 logits，参数量 650M。预训练只用了 726B tokens，约为 ESM-2 650M 的 0.73 倍，也是 E1-600M/ESM C 600M 的 1/5–1/8。检索片段还显示训练时使用了 Foldseek 3Di 状态与 Cβ 距离辅助头，辅助头只在训练期使用，并在训练前过滤结构链；但这部分在可见文本中不完整，具体损失权重和数据配比需要回原文确认。推理侧，作者实现融合算子与 INT8 矩阵乘法，按 128 宽归约切片做 INT32 累加再以激活 scale 反量化到 FP32，思路参考 DeepSeek-V3 的 FP8 分块策略；单张 H20 上约 130,000 residues/s，较其他 600M 级编码器快 1.9–2.7 倍。

评测是论文最有价值的部分之一：29 个模型（28 个公开 checkpoint + HyPLM-Flash）、13 个模型家族、32 项任务，全部走同一套 BF16 评测代码；零样本分数来自 MLM head，监督任务在冻结表征上做线性探针，并从六个相对深度中按验证集选最佳层；置信区间用 assa...**

# Foundation AI & AGI

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Foundation AI & AGI
- 方法：agent, generation
- 论文/报告：1 篇
- Toward Knowledge-Centric AI: Discovering, Internalizing, and Using Explicit Knowledge Across Agents and Generations
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

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

# Language Models

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Language Models
- 方法：audio
- 论文/报告：2 篇
- SCorPion: A Strategic, Controllable and Personified Conversational Companion
- Hy ASR 3.0 Technical Report
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:03ce9ac9d2255297 -->
## SCorPion: A Strategic, Controllable and Personified Conversational Companion

[[Deep Reading - Nov 2026/SCorPion-A Strategic, Controllable and Personified Conversational Companion|Deep Reading]]

[https://arxiv.org/pdf/2611.10906v1](https://arxiv.org/pdf/2611.10906v1)

- **这篇论文关注开放域对话中“固定 prompt 不够用”的问题。LLM 在问答、推理、编码等任务上表现很强，CoT、BOT 等提示方法能通过固定或检索式 prompt 激发能力；但日常对话往往没有显式推理步骤，话题发散、彼此关联，且缺乏统一的定性评价标准。已有的沟通技能方法（CSIM、Ask-an-Expert）通常只提供固定数量的技能或专家视角，难以覆盖复杂、长尾的对话情境。作者因此提出 SCorPion：一个基于 GraphRAG 的、具备策略性、人格化和显式可控性的对话伴侣。

方法上，SCorPion 用两个 LLM 分工：M 生成最终回复，Mg 负责 GraphRAG。离线阶段，Mg 对多形态文本语料做知识图谱抽取、社区检测和社区摘要，生成“社区摘要策略”（CSS）。每个 CSS 是一个三元组：Condition（何时行动）、Skill（如何行动）、Demonstration（参考示例）。这些策略被存入在线数据库；作者再用 GPT-4o 为社区和 CSS 生成类别标签（如共情、道德、幽默），并人工检查，以提升可解释性和可控性。在线阶段（合理推断），系统根据用户查询检索相关社区和查询聚焦的最优策略，把策略注入 M 的提示，生成风格化回复；显式标签可用于选择或调节策略。与 BOT 相比，SCorPion 的关键变化是用 GraphRAG 的全局社区摘要替代标准检索器，并把“对话策略”而不是“推理 thought”作为检索对象。

实验覆盖开放域对话（DailyDialog）、情感支持（ESConv、EmpatheticDialogues）、风格/道德风格（MIC、GYAFC、Shakespeare）等数据。主模型为 LLaMA3-70B-Instruct，对比 Self-Refine、CoT、BOT。Table 1 报告 BLEU-2 和 ROUGE-L：SCorPion 在六个数据集的两个指标上均高于可见基线。例如 ESConv 上 B-2 4.32、R-L 13.00（BOT 为 3.44/10.95）；DailyDialog 3.79/12.01（BOT 2....**

<!-- paperflow:003e2621356ccfdf -->
## Hy ASR 3.0 Technical Report

[[Deep Reading - Nov 2026/Hy ASR 3.0 Technical Report|Deep Reading]]

[https://arxiv.org/pdf/2611.14908v1](https://arxiv.org/pdf/2611.14908v1)

- **一、论文定位与问题
Hy ASR 3.0 是一篇系统型技术报告，目标不是提出单一新算法，而是构建一个基于 LLM 的 ASR 系统，同时解决三件事：跨普通话/英语/粤语/13 个汉语方言组的高精度识别；对热词列表、问答对话、摘要等异构上下文的灵活利用；在长语音、儿童/老年人、低音量、噪声等复杂条件下的鲁棒性。论文在 Introduction 中把这三件事明确列为实践目标，并在结论中再次用“识别精度、上下文利用、鲁棒性”三条主线总结。
二、系统架构
Hy ASR 3.0 由音频编码器、音频适配器、LLM 解码器和音频条件热词检索机制组成。音频编码器提取声学表征，适配器把语音表征映射到 LLM 可消费的空间，LLM 解码器生成转写；热词检索机制以音频为条件选择/组织热词候选，供解码阶段使用。论文还提到大规模预训练与多阶段后训练，后训练分 SFT 和 RL 两阶段；SFT 用于提升识别性能，RL 细节在可见片段中未展开。
三、上下文能力
系统支持三类外部上下文：热词列表、问答对话、摘要。除此之外，它还能利用同一句内上下文消歧同音/近音关键词，不需要额外上下文输入。这个“句内上下文”能力是它与普通热词偏置系统的一个区别：线索来自当前语音本身，而不是外部给定文本。论文在 Table 3 上分别评估 intra-utterance context 和 dialogue-context，用 target-keyword recall 衡量。
四、实验设计
公开基准覆盖 AISHELL-1、AISHELL-2、AliMeeting、WenetSpeech、FLEURS、Common Voice、SpeechIO、LibriSpeech、GigaSpeech、TED-LIUM 3、VoxPopuli、MLS、WenetSpeech-Yue、KeSpeech；对比系统包括 Qwen3 ASR Flash、FunASR Flash、Doubao ASR 2.0、Seed 2.0 Lite、StepAudio 3 ASR Max 和 Hy ASR 3.0 Preview。指标包括 CER/WE...**

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, reasoning, optimization, deep-learning, stat-ml, stat-me
- 论文/报告：7 篇
- Learning to Interact and Direct: Co-Evolving Video Director Agents
- Towards Self-Evolving Narrative Systems: Real-Time Adaptation and Long-Term Optimization with LLMs
- Think or Remember: The Exchange Rate of Agentic Inference
- Incentive-Responsive Agents: A Principal-Agent Framework, Metric Suite, and Runtime Architecture for Long-Lived Personal AI Agents
- ARE-bench: Benchmarking Agents' Research Ability in Real-World Researcher Sessions and Workspaces
- Prior-as-Skill: Remember Outside and Read Inside with On-Policy Self-Distillation for Self-Evolving VLM Agents
- WordInferBench: Can Agents Infer How to Edit from Word Document Context?
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

<!-- paperflow:3d17d8ab023cccdd -->
## Towards Self-Evolving Narrative Systems: Real-Time Adaptation and Long-Term Optimization with LLMs

[[Deep Reading - Nov 2026/Towards Self-Evolving Narrative Systems-Real-Time Adaptation and Long-Term Optimization with LLM|Deep Reading]]

[https://arxiv.org/pdf/2611.01954v1](https://arxiv.org/pdf/2611.01954v1)

- **【论证主线】论文从一个被反复陈述却未被真正消解的二难出发：交互叙事要么给作者控制（分支树、固定对话），要么给玩家自由（LLM 开放生成），二者难以兼得。传统分支系统连贯但可预测；LLM 系统灵活但随机性与上下文敏感性带来叙事不连贯、角色行为不一致与作者控制力丧失（Gu et al., 2025；Alavi et al., 2024；Yi et al., 2025）。作者随后做了一个关键的重新定位：现有混合式方案（结构约束、个性化、多智能体长篇生成）的失败并非单点质量问题，而是“生成逻辑静态”——提示词与策略在设计完成后被冻结，系统不会从游玩历史中学习，因此在长时段中不可避免地退化（Peng et al., 2024；Jeong and Lee, 2025）。由此，论文的目标被定义为双重的：既要实时平衡控制与自由，又要在长时间尺度上维持并改进叙事质量。

【技术主线】框架由三个相互衔接的构件组成。第一是结构层：把剧情建模为分层叙事树，作者预写节点构成可复用骨架。第二是决策层：把玩家输入与候选节点编码为向量（节点侧使用 MiniLM），计算余弦相似度作为语义对齐分数 S(p, n_i)；若 S ≥ θ_high 则复用预写内容，否则进入自适应叙事分支，触发受上下文约束的实时生成，并对生成候选按一组原则打分、择优采纳。这一层把全局的“控制 vs 自由”二难降解为逐轮的局部路由决策，是论文最具工程感的机制。第三是演化层：收集玩家反馈，通过 in-context learning 迭代精炼提示词，免去人工提示工程，使系统的生成策略随游玩历史自我改进。三者构成“结构保底—路由仲裁—反馈演化”的闭环。

【实验主线】评估采用案例研究加 12 人参与者研究，覆盖叙事连贯性、NPC 一致性与玩家能动性三个维度，并以交互中的开放式追问采集具体的不连贯事件与创造性体验。分析上，作者除主观评分外还引入结局类型编码（Opaque Endings vs Earned Closure），把“故事是否站得住”转成可计数的指标。与 flat 系统相比，报告的主要结果是：Opaque Endings 由...**

<!-- paperflow:8603b7299a2582d6 -->
## Think or Remember: The Exchange Rate of Agentic Inference

[[Deep Reading - Nov 2026/Think or Remember-The Exchange Rate of Agentic Inference|Deep Reading]]

[https://arxiv.org/pdf/2611.10982v1](https://arxiv.org/pdf/2611.10982v1)

- **【一句话】论文研究 agent 在每次调用中重发增长 transcript 时，应该“继续想（保留 reasoning）”还是“靠记忆（丢弃后重算）”，并把这个取舍形式化为 c/β 的交换率。
【背景与问题】基于推理模型的 agent 会产生很长的 reasoning tokens；多轮运行中，transcript 不断增长，客户端每次都要把历史重新发回服务端。常见优化是删除早期 reasoning、压缩历史或截断观察，然后用 prompt token 减少来宣称节省；服务端则用 prefix cache 复用已计算的 prefix，并报告 cache 命中。论文指出这是两个交易方各自记账：客户端控制消息，服务端控制早期 reasoning 是否返回、以 token/美元/FLOPs 何种口径计费、是否复用 generated states。一个 token 的成本取决于它在上下文中的位置和服务器是否已持有它的计算，所以 token 数本身不能给策略定价。
【形式化】论文把一次 run 的总成本写成模型架构与双方选择的函数。每个被携带的 token 都是在两种“拥有同一信息”的方式之间选择：保留在上下文中，由此后每个 token 的注意力支付（β per pair）；丢弃，则在需要时重新生成（c per token processed），并重建它在缓存中之后的全部内容。架构对处理收 c、对注意力收 β，二者之比 P*=c/β 是“思考”与“记忆”的汇率。按 token 计，上下文长度低于 P* 时处理项占主导，称为 thinking regime；高于 P* 时注意力项占主导，称为 memory regime。
【两类成本】论文区分机械成本与行为成本。机械成本：即使 agent 写出完全相同的 token，策略改变缓存命中/重算也会改变总成本；行为成本：策略改变 agent 写出的 token，进而改变推理路径、工具调用和答案。这个区分是实验设计的骨架：离线 replay 固定 token 序列，隔离机械成本；live rerun 让策略改变输出，测行为成本。
【实验与...**

<!-- paperflow:33d7079d48002a50 -->
## Incentive-Responsive Agents: A Principal-Agent Framework, Metric Suite, and Runtime Architecture for Long-Lived Personal AI Agents

[[Deep Reading - Nov 2026/Incentive-Responsive Agents-A Principal-Agent Framework, Metric Suite, and Runtime Architecture|Deep Reading]]

[https://arxiv.org/pdf/2611.15515v1](https://arxiv.org/pdf/2611.15515v1)

- **论文的主线可以概括为：一个正在发生的产品事实，引出一个被忽视的缺失层，用一套理论把该层说清楚，用一组指标把它量化，用一个架构把它落地，用两步实验检验它，最后诚实地报告其中一个理论与实验不一致的地方。

第一条线是论证线。作者先从产品事实出发：个人智能体已经持久化——Meta 的 Muse 提供 per-user 虚拟机、agent 自有邮箱和分级权限，OpenClaw 在用户本机常驻并通过消息渠道触达用户，Manus 端到端执行多步任务，DeepSeek 的桌面产品主打在本地文件上做长时间后台工作。这些系统的共同点是 persistence：同一个实例累积历史、能力、义务和声誉，并被期待明天继续工作。但工程界对这一事实的回应几乎完全是能力侧的：更长的 system prompt、对模型施加说服、更大 checkpoint、更长 thinking budget、检索、按 query 路由到更强模型。这些杠杆真实有效，但共享一个结构性缺陷——它们是 per-request 的，被选择、被遗忘，无法表达“这个 agent 已经挣到了更多权限”“这个 agent 三周没被验证过”“这个 agent 的奖金取决于下一次评审”。作者由此提出：长寿命智能体缺少的是一层激励。

第二条线是技术线。论文拒绝把“激励”当作提示词里的文字，而是把它定义为一个运行时契约，覆盖 agent 的运行点：模型档位、测试时计算深度、记忆预算、工具权限、任务优先级、验证、报酬；关键要求是 agent 的策略必须因果地消费这个契约。为此作者建立委托—代理模型：努力隐藏、运行点可缔约、验证有成本。任务被形式化为 τ = (c, v, λ, q)，运行点为 x = (m, d, κ, a, ν)，成本为 C(x;τ) = p_m·n(c,d,κ) + λ·L(x) + c_ν·ν，其中 token 数 n = (n_0 + n_1 c)·d·(1+κ)、延迟 L = ℓ_0·d·μ_m——刻意做得简单，以便路由规则可解析、通道可消融。由此导出四条结论：非消费不变性原理（真激励的判据：移除运行时契约会改变策略分...**

<!-- paperflow:724fd4bbbd10089c -->
## ARE-bench: Benchmarking Agents' Research Ability in Real-World Researcher Sessions and Workspaces

[[Deep Reading - Nov 2026/ARE-bench-Benchmarking Agents' Research Ability in Real-World Researcher Sessions and Workspaces|Deep Reading]]

[https://arxiv.org/pdf/2611.14364v1](https://arxiv.org/pdf/2611.14364v1)

- **1. 论文的主线可以概括为“从独立任务分数转向项目中途交付”。作者首先指出，研究 agent 在 PaperBench 等基准上的高分并不能说明它们能在真实研究项目中可靠工作：现有基准多为 standalone 目标，工作区干净或经过整理，成功标准偏向最终产物；真实研究者却常常只要求 agent 完成项目中的一步，而这一步依赖此前会话决策和工作区中未完成、过时或失败的 artifact。论文用 Qwen3.8-Max 在 PaperBench Code-Dev 上 93.0% 的例子说明，即使接近上限的 code-dev 分数，也可能没有测试执行与实证复现等关键维度。

2. 为此，作者构建 ARE-bench：从招募研究者的真实 agent 会话和完整工作区中抽取 75 个任务，覆盖 Survey、Design、Data & Env.、Code、Execution、Analysis、Writing 七个阶段，共 33 项需求、150 项子需求和 1,173 条二元 checklist。每个任务锚定一个真实用户请求，任务 brief 包含 Background、Requirements、Environment：Background 概括仍然生效的早期请求，Requirements 给出当前交付物与验收条件，Environment 说明资源与限制。

3. 技术上的关键设计是工作区重放。论文把完整工作区恢复到 cut point，且不做清理，保留未完成文件、过时版本和失败尝试，使 actor 面对真实的 in-progress research state。每条 checklist 由 intent-aligned rubric 编译而来，评分允许任何有效解，不把原始轨迹当作唯一 gold answer；同时用 replay checks 和匿名化控制泄漏与隐私。评价协议让 actor 收到 instruction 和 restored workspace，交付物按 task-specific binary checklist 评分，ISR 衡量任务级严格完成，CSR 衡...**

<!-- paperflow:91febfbccc0a6fe1 -->
## Prior-as-Skill: Remember Outside and Read Inside with On-Policy Self-Distillation for Self-Evolving VLM Agents

[[Deep Reading - Nov 2026/Prior-as-Skill-Remember Outside and Read Inside with On-Policy Self-Distillation for Self-Evolvi|Deep Reading]]

[https://arxiv.org/pdf/2611.15766v1](https://arxiv.org/pdf/2611.15766v1)

- **一、论证主线：论文从一个部署矛盾出发。自演化视觉 agent 把过去的技能压缩成关键帧加步骤笔记，并用 OPSD 把教师看到的书面顺序蒸馏进 VLM 的 reader。训练时教师有特权顺序，学生只看帧；部署时帧可能缺失，人或网站也可能重排步骤。现有内化方法把顺序存进 reader 权重，于是“记得的顺序”和“看到的顺序”纠缠在一起：全帧训练时缺帧只能猜；去帧/子序列训练时顺序记得住，却会在可见顺序异常时压过视觉。论文因此提出 Prior-as-Skill：顺序应该保存在模型外，reader 只负责读帧，推理时用一个可见性检查决定“读里面”还是“查外面”。

二、技术主线：PriorSplit 由三部分组成。1）外部计数先验：对每个规范相邻对 (X,Y)，统计训练 episode 的呈现顺序中 X 先于 Y 的次数，用 Laplace/Beta(c,c) 后验均值 (n_XY+1)/(n+2) 作为 p_s，并裁剪到 [0.02,0.98]，使稀有对接近 1/2；对称 Beta 性质保证计数较大的一侧才可能改变答案。2）可见性检查：reader E 对问题点名的每个 unit 回答“是否 shown”，用 log-ratio ℓ_u = log p_E(y|q,V) 与阈值 0 判定；g = 1[ℓ_X>0] ∧ 1[ℓ_Y>0]，最多两次查询。3）Switch：g=1 时从帧回答，g=0 时由计数先验回答。除 OPSD 外不训练新模块；片段中的 optional probe 用 L2 logistic/hidden state/LoRA 尝试学习计数，但相对简单计数在短边准确率上变化 −0.7 点，说明学习式适配没有稳定增益。

三、理论主线：Proposition S3 说明正确检查时先验不进入可见项；Theorem 2 分析 check miss 比例 α 下的 switch 误差，并给出独立但非同样分布顺序生成下的计数界；Lemma S2 说明对称 Beta 先验下只有计数较大侧会进入答案。作者把误差拆成可见 reader 误差、检查漏检误差和先验误差三部分；片段...**

<!-- paperflow:e328dad4fa0860fc -->
## WordInferBench: Can Agents Infer How to Edit from Word Document Context?

[[Deep Reading - Nov 2026/WordInferBench-Can Agents Infer How to Edit from Word Document Context|Deep Reading]]

[https://arxiv.org/pdf/2611.15189v1](https://arxiv.org/pdf/2611.15189v1)

- **一、问题定位：这篇论文关注 Word 文档编辑中的一个被现有基准压缩掉的能力：智能体能否不依赖逐项列出的格式指令，而从文档上下文中推断目标对象应如何修改。作者指出，真实编辑中用户经常只要求“与文档中另一个对象保持一致”或“检查这里需要改什么”，并不会把字体、行距、颜色、边框、阴影等属性全部写出来；但已有基准大多提供完全指定的属性修改指令，因而无法评估这种推断能力。

二、基准设计：论文提出 WordInferBench，包含 280 个基础文档编辑实例，每个实例派生三种指令条件，共 840 个变体。三种条件分别是：描述式，明确列出每个属性变化；参照式，指向另一个对象作为范例，但不列属性；推断式，要求智能体审阅文档上下文并自行判断需要什么修改。基准覆盖 13 类对象和 81 种属性，任务来自真实 Word 文档。

三、构造逻辑：作者寻找同类型且格式一致的对象对，把其中一个作为目标对象，将其属性重置为默认值，保留另一个作为文档内 exemplar，使目标格式仍可从上下文恢复；同时用 held-out reference document 表示期望格式。这样既保证正确答案客观可判定，又让智能体必须探索文档、定位目标、比较范例并执行修改。论文还招募 9 名标注者分 3 组核验样本，必要时修订指令或参考文档。

四、评测协议：智能体需要完成“探索上下文—定位目标—推断修改—执行编辑”的链路。论文至少报告两个目标对象指标，按任务宏平均；两个指标都只评目标对象，非目标保持单独报告。这个设计把“目标是否改对”和“是否破坏其他内容”分开，避免单一总分掩盖附带损伤。

五、实验与发现：论文在隔离环境中评测多个 agent 系统，可见片段列出 Human、Claude Code（Sonnet 4.6）、Codex（GPT-5.4）、Kimi CLI（Kimi K2.6）、GLM 5.2 等。实验揭示的主要现象包括：智能体在上下文推断式编辑上存在明显能力缺口；参照式条件可能因程序化复制属性而变得相对可操作，但仍受定位与执行约束；定位不是唯一瓶颈，即使提供 explicit property...**

# Computer Vision

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Computer Vision
- 方法：agent
- 论文/报告：1 篇
- CoCode-World: Shared 3D Proxy Representation for Multi-Player World Models
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:1d093b31d444f65e -->
## CoCode-World: Shared 3D Proxy Representation for Multi-Player World Models

[[Deep Reading - Nov 2026/CoCode-World-Shared 3D Proxy Representation for Multi-Player World Models|Deep Reading]]

[https://arxiv.org/pdf/2611.15602v1](https://arxiv.org/pdf/2611.15602v1)

- **一、论文要解决的问题
CoCode-World 关注多人生成式世界模型中的跨视角一致性：两名玩家在同一场景中各有相机，看到的遮挡和朝向不同，但生成画面必须对共享角色、物体和场景结构给出一致解释。现有两条路线各有缺口：一条是在学习到的 latent 空间做 joint denoising、cross-agent attention 或 shared representation，优点是端到端，缺点是空间控制不直接；另一条是显式状态/世界表示，把世界演化与视角合成分开，但部分方法依赖精细、场景特定的地图几何，限制场景多样性。论文要补的空白是：能否用粗粒度、可编程的 3D proxy 作为结构与视频渲染器之间的共享接口，让视频模型补足外观细节，同时保持跨视角一致。

二、方法主线
方法把问题拆成“结构”和“外观”两个条件。结构由共享 3D proxy 提供：用地图几何 M、每个玩家可见的实体状态 E_t(p) 和相机参数 C_t(p) 投影出相机特定 proxy，P_t(p)=R(M, E_t(p), C_t(p))，指定场景布局、运动与遮挡。外观由 RGB 参考 I 提供，指定角色和场景的长相。生成时，把两名玩家的 proxy 视频、外观参考和目标视频按固定左-右布局拼成一个画布，交给预训练视频 Transformer（片段中称 H3 transformer / Video DiT）联合去噪两路目标视角；跨视角通信依赖模型原生 attention 在单一生成分支内完成，而不是额外设计 cross-agent 模块；主干冻结，用 LoRA 适配 proxy 遵循和成对外观对应。解码后去掉 padding，裁成两段玩家视频。这个设计的核心是把“多人一致性”从隐空间协调转为共享空间布局 + 共享上下文 + 配对监督。

三、数据与训练
作者通过 private FiveM server 采集双人 GTA V gameplay，对每个客户端记录 RGB、相机参数、玩家与附近实体的身份和位姿，并导出同一坐标系下的静态地图几何；地图与持久实体 ID 用于跨相机关联同一场景。动态观测保留...**

# AI Research

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Research
- 方法：待从后续精读中沉淀
- 论文/报告：2 篇
- Taxonomizing Synthetic Data for Law
- Lines Are Ideas: Visual Creativity Assessment in Hyperbolic Space
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:6bb40e0c451b4b08 -->
## Taxonomizing Synthetic Data for Law

[[Deep Reading - Nov 2026/Taxonomizing Synthetic Data for Law|Deep Reading]]

[https://arxiv.org/pdf/2611.15489v1](https://arxiv.org/pdf/2611.15489v1)

- **一、论文定位与问题意识
本文是一篇法律评论（Comment），对话对象是 Michal Gal 与 Orla Lynskey 关于合成数据法律影响的研究。Gal 与 Lynskey 主张合成数据将深刻改变信息隐私、市场竞争与数据质量，现有以收集数据为中心的法律需要重新评估。本文不否认这一判断，但认为既有讨论常把合成数据与“真实”收集数据截然二分，把合成数据当作同质整体，从而高估或低估其风险。作者的核心主张是：合成数据的法律风险不仅取决于它与收集数据的联系，至少同样取决于生成时所使用的背景知识和对 ground truth 的假设；因此需要一套按“依赖程度/假设负载”组织的分类学。
二、核心分类
作者把合成数据分为三类。第一，transformed data：对收集数据进行变换，以保留某些统计性质供终端用途使用；它不追求提升对 ground truth 的保真度，甚至可能降低保真度。第二，augmented data：借助背景知识或假设补充收集数据，目标是提升数据集对 ground truth 的保真度。第三，simulated data：几乎完全依赖背景知识与 ground-truth 假设，收集数据只占很小部分。三类构成从“高收集数据依赖”到“高假设依赖”的谱系。作者明确不把常规 curated data（如单位换算式校正）纳入分类，因为其目标是更准确地呈现收集数据，而不是生成新的合成数据（此点依据检索片段推断）。
三、隐私分析
作者指出，没有任何一类合成数据能消除基于群体的推断伤害。Transformed data 为保留统计效用而保留的分布结构，可能正好保留群体层面的信号；差分隐私、k-匿名等隐私增强技术主要针对个体层面的再识别，难以覆盖群体推断。Augmented 与 simulated data 则通过假设引入新的推断路径。论文的隐私讨论还涉及链接推断、成员推断等攻击文献，说明合成数据并非天然安全。
四、偏见与数据质量
Augmented 与 simulated data 可能放大偏见，同时让数据集看起来更完整。作者提出一个关键的认识论区分：噪声与缺失是“不可...**

<!-- paperflow:482fbe2179da6ac3 -->
## Lines Are Ideas: Visual Creativity Assessment in Hyperbolic Space

[[Deep Reading - Nov 2026/Lines Are Ideas-Visual Creativity Assessment in Hyperbolic Space|Deep Reading]]

[https://arxiv.org/pdf/2611.14907v1](https://arxiv.org/pdf/2611.14907v1)

- **【论文定位】《Lines Are Ideas: Visual Creativity Assessment in Hyperbolic Space》从标题看是在讨论线条/线稿与创意的关系，但摘要片段显示其核心贡献是一个视觉创造力评估框架，而不是生成模型。论文试图把“创造力”从单一的美学分、相似度或多样性指标，变成 novelty 与 appropriateness 的耦合判断：前者看创意结果相对参考偏离多远，后者看这种偏离是否仍然语义连贯、是否构成有意义的创造。
【问题意识】作者认为，固定的几何约束无法同时适配两类创作：有些作品需要较大的风格偏移才成立，约束太紧会把它们压回参考；另一些作品应保持具象、贴近语义核心，约束太松又会让无意义的偏离获得高分。于是论文提出用风格对齐信号来条件化双曲空间的允许半径，让几何约束随风格抽象程度变化。
【技术主线】方法可以重建为四步：第一，把原始图像与创意图像编码为双曲表征 h 与 h̃；第二，取风格 logits z_s，经可学习 MLP g 和 sigmoid 得到 α=σ(g(z_s))；第三，用 ||h|| ≤ (1−α)/√κ 约束表征半径；第四，用双曲测地距离 D_nov=d_H(h,h̃) 度量新颖性。适当性则被定义为依赖新颖度的判断：偏离越大，“合理”的标准越从字面相似转向概念可解释性。这个技术路线的关键不是某个生成模块，而是“用风格信号控制几何自由度”的评价几何。
【论证逻辑】论文的论证依赖三个假设：双曲空间能表达层级化、概念性的视觉语义；测地距离能区分结构化偏离与随机偏离；风格信号能指示创作应获得多大的偏离预算。若这三个假设成立，自适应半径就能比固定半径更贴近人类对“合理创意”的判断。反之，若嵌入没有学到层级语义，双曲距离可能只是欧氏距离的变形；若风格 logits 不可靠，α 会把数据集偏置带入评分。
【实验主线与证据状态】提供的检索证据没有 Experiments 章节，也没有数据集、baseline、指标、消融或数值。因此不能确认论文是否真的验证了上述论证，也不能报告任何性能结论。最需要核对的是：任务是否确为线稿/...**
