# On-Policy Distillation & Post-Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：On-Policy Distillation & Post-Training
- 方法：reasoning, vision, reinforcement-learning, retrieval, multimodal-learning, multimodal-reasoning, deep-learning
- 论文/报告：12 篇
- Dense Supervision, Sparse Updates: On the Sparsity and Geometry of On-Policy Distillation
- Learning to Reason by Analogy via Retrieval-Augmented Reinforcement Fine-Tuning
- 🧐OmniOPD: Logit-Free On-Policy Distillation via Speculative Verification【OPD, Meta】
- Trust Region On-Policy Distillation【OPD】
- OPD+: Rethinking the Advantage Design for On-Policy Distillation【OPD】
- OPRD: On-Policy Representation Distillation【OPD, Ant】
- ViCuR: Visual Cues as Recoverable Privilege for Multimodal On-Policy Distillation【OPD】
- On the Geometry of On-Policy Distillation【OPD】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:6d387db7c43582be -->
## Dense Supervision, Sparse Updates: On the Sparsity and Geometry of On-Policy Distillation

[[Deep Reading - Jun 2026/Dense Supervision, Sparse Updates-On the Sparsity and Geometry of On-Policy Distillation|Deep Reading]]

[https://arxiv.org/pdf/2606.13657v1](https://arxiv.org/pdf/2606.13657v1)

- **这篇论文对同策略蒸馏（OPD）的参数更新机制进行了深度“显微镜”式的分析。研究的核心结论是：尽管 OPD 接收来自教师模型的稠密 Token 级信号，但其对学生模型参数的改变在本质上是稀疏且具有特定几何偏好的。通过对 Qwen 和 Llama 等模型的实证研究，作者发现 OPD 的更新量非常小，且在坐标空间呈现高度稀疏性，这种稀疏性在 FFN 层尤为明显。实验证明，这种稀疏结构具有功能性意义，因为仅通过更新一小部分关键参数子集即可达到全量更新的效果。在几何层面，OPD 更新避开了原始权重的核心特征空间，倾向于在权重较小的坐标上进行微调。此外，研究还解释了为什么 AdamW 在这种看似稠密的监督任务中依然不可或缺。总体而言，这项工作挑战了“蒸馏即重写”的传统认知，提出 OPD 更像是一种精准的“同策略参数编辑”，为理解大模型后训练过程提供了重要的理论支撑和实证证据。**

<!-- paperflow:5ff4b583a8134663 -->
## Learning to Reason by Analogy via Retrieval-Augmented Reinforcement Fine-Tuning

[[Deep Reading - Jun 2026/Learning to Reason by Analogy via Retrieval-Augmented Reinforcement Fine-Tuning|Deep Reading]]

[https://arxiv.org/pdf/2606.13680v1](https://arxiv.org/pdf/2606.13680v1)

- **这篇论文提出了 RA-RFT，一种旨在提升大语言模型类比推理能力的后训练框架。针对传统强化学习在处理未知推理模式时奖励稀疏的问题，以及传统检索方法无法捕捉推理逻辑相似性的缺陷，RA-RFT 创新性地引入了推理感知检索。通过金牌相关性蒸馏技术，训练检索器识别对解题真正有益的示例；随后在强化学习过程中，引导模型学习如何利用这些外部示例构建自己的思维链。在 Qwen3 系列模型上的实验证明，该方法在 AIME 等高难度数学基准上取得了显著的性能提升，且这种提升与奖励函数设计或课程学习是正交且互补的。该工作为 RAG 与 RL 的结合提供了新的范式，强调了“推理效用”在检索中的核心地位。**

<!-- paperflow:9057622bbf70c581 -->
## 🧐OmniOPD: Logit-Free On-Policy Distillation via Speculative Verification【OPD, Meta】

[https://arxiv.org/pdf/2606.01476](https://arxiv.org/pdf/2606.01476)

- 解决标准On-Policy Distillation (OPD) 面临的两个核心耦合限制： 1. 白盒访问限制（White-box Access Barrier） 标准OPD方法（如基于Reverse KL散度的方法）要求直接读取教师模型的token-level logits（即每个候选token的精确概率分布）。2. Token-level监督信号的脆弱性（Brittleness of Token-level Supervision） 即使能够访问logits，token-level匹配作为一种监督信号也存在结构性缺陷。**能否在不访问教师logits的情况下执行密集的on-policy蒸馏？什么样的替代密集监督信号能够匹配甚至超越基于logits的匹配？**论文通过提出OmniOPD框架来回答这些问题，该框架引入： 无Logit的Chunk-level监督：将确定性logit匹配替换为基于蒙特卡洛rollout的语义相似度度量，**通过多token块（multi-token chunks）的连续语义相似性来近似教师偏好**。 Peak-Entropy调度：仅在学生轨迹中高不确定性的"推理分叉点"（reasoning forks）进行教师验证，集中监督资源。 贝叶斯平滑与KL锚点：利用Dirichlet-Multinomial先验稳定离散采样的高方差，并通过基础模型KL散度约束未审计token的策略漂移。 实验表明，该方法不仅消除了对教师logits的依赖，能够利用Claude-4.5-Haiku、Gemini-2.5-Flash等黑盒教师，而且在数学推理任务上比标准白盒OPD最高提升**+28.64%**，证明了chunk-level语义验证比token-level logit匹配提供更干净、更可靠的学习信号。

<!-- paperflow:70d926cf7df9d29f -->
## Trust Region On-Policy Distillation【OPD】

[https://arxiv.org/pdf/2606.01249](https://arxiv.org/pdf/2606.01249)

- 解决**On-Policy Distillation (OPD) 在教师模型与学生模型分布差异较大时出现的训练不稳定性和优化失败问题**。论文提出了Trust Region On-Policy Distillation (TrOPD)，核心思想是： 信任区域学习：**仅在教师模型可验证的可靠区域内进行反向KL优化，通过解码一致性比率 $P_{\text{trust}}(x) = \min\left(\frac{\pi_T(x)}{\pi_S(x)}, 1\right)$ 动态划分可靠区域与异常区域** 异常值估计：对分布不匹配区域采用前向KL（Forward KL）估计，在保持训练稳定的同时保留信息性监督 离策略引导：通过教师模型生成的前缀引导学生模型探索高质量轨迹，逐步向可靠生成区域收敛.

<!-- paperflow:b6cb35fcf4542a43 -->
## OPD+: Rethinking the Advantage Design for On-Policy Distillation【OPD】

[https://arxiv.org/pdf/2606.01039](https://arxiv.org/pdf/2606.01039)

- 论文提出 OPD+ 框架，通过以下方式解决上述问题： 修正优势估计：对于一般$f$-散度，正确的优势权重应为 $w_f(u) = -f(u) + u f'(u)$，而非简单的 $-f(u)$ 统一$f$-散度优化：建立了基于$f$-散度的通用优化框架，证明Reverse KL的特殊性（其修正项为常数+1，可被基线吸收），而其他散度必须包含修正项 $u f'(u)$ 才能避免偏差 经验性能提升：OPD+不仅"拯救"了之前无法训练的前向KL和JSD（使其达到或超越Reverse KL的性能），甚至在Reverse KL本身上也取得了进一步提升 简言之，该工作纠正了OPD中优势估计的数学错误，消除了对KL散度的单一依赖，并拓展了可稳定使用的散度函数设计空间。

<!-- paperflow:6039bfa82e7223a1 -->
## OPRD: On-Policy Representation Distillation【OPD, Ant】

[https://arxiv.org/pdf/2606.06021](https://arxiv.org/pdf/2606.06021)

- 论文提出**On-Policy Representation Distillation (OPRD)**，首次将On-Policy蒸馏从输出空间提升至隐藏状态空间。OPRD在**同一策略内轨迹**（on-policy rollouts）上，将学生的中间隐藏表示与教师对齐。

<!-- paperflow:f4ab748a699fd414 -->
## ViCuR: Visual Cues as Recoverable Privilege for Multimodal On-Policy Distillation【OPD】

[https://arxiv.org/pdf/2606.05718](https://arxiv.org/pdf/2606.05718)

- 解决多模态 On-Policy Distillation (OPD) 中基于答案的特权信息（answer-based privilege）所导致的训练-测试不匹配（train-test mismatch）问题。 具体而言，**现有工作通常通过让教师模型访问训练时特有的信号（如参考答案、黄金推理链）来增强监督信号。然而，这种设计会引入特权诱导的条件信息差（privilege-induced conditional information gap）：教师策略的分布 pT(Yt|X,R,Y<t) 依赖于测试时不可见的答案相关变量 R ，导致学生模型被迫拟合答案感知的模式（answer-aware patterns），而非学习基于视觉证据的推理（visually grounded reasoning）**。 为解决此问题，论文提出**将不可恢复的答案特权替换为可恢复的视觉特权（recoverable visual privilege）——即视觉线索（Visual Cues）**。这些线索描述问题相关的视觉证据（如几何关系、图表关键结构），其信息源完全来自推理时可见的输入 X 。通过引入轻量级的线索恢复模块（Cue Recovery Module），学生模型可在内部聚合这些视觉证据，从而在保持标准推理接口不变的前提下，实现基于视觉证据的推理，而非依赖答案捷径。

<!-- paperflow:0bae98e1122bfa58 -->
## On the Geometry of On-Policy Distillation【OPD】

[https://arxiv.org/pdf/2606.07082](https://arxiv.org/pdf/2606.07082)

- 解决**On-Policy Distillation (OPD)** 在参数空间中的训练动态（training dynamics）理解不足的问题。尽管OPD在提升大语言模型（特别是推理能力）方面日益重要，现有研究主要关注： SFT产生密集、对齐主成分的更新（dense, principal-aligned updates） RLVR产生稀疏、偏离主成分的更新（sparse, off-principal updates） 但**OPD结合了两者的特征（密集令牌级监督 + 在线策略采样）**，其参数空间轨迹无法从单一范式推断。因此，论文旨在提供OPD的参数空间几何解释，揭示其既非简单的SFT-RLVR插值，而是具有独特的"早期子空间锁定"特性的优化机制。**OPD比SFT更稀疏且更保留预训练几何结构，但比RLVR约束更少，表现为更新集中于低幅度权重区域（53.59% vs. RLVR的73.48%），但可见支持集更广**。OPD并非SFT与RLVR的简单插值，而是**诱导了独特的参数空间几何：松弛的非主成分定位、早期子空间锁定与对目标组合的敏感性**。

<!-- paperflow:f3932e2c8c0b56c8 -->
## RLCSD: Reinforcement Learning with Contrastive On-Policy Self-Distillation【OPD, Tongyi】

[https://arxiv.org/pdf/2606.11709](https://arxiv.org/pdf/2606.11709)

- **单方面的正向特权提示**（仅使用正确答案作为提示）会系统性地改变教师模型的生成行为：倾向于更短、更断言式的表述，抑制探索性对冲，并将概率质量重新分配给格式标记。这种与正确性正交的风格偏移污染了监督信号。论文提出**RLCSD**（Reinforcement Learning with Contrastive on-policy Self-Distillation），通过对比正确提示与错误提示下的教师-学生差距。通过这种对称对比，共享的特权诱导风格成分在减法中相互抵消，使剩余信号更集中于与任务正确性相关的标记，从而实现稳定训练并提升推理性能。

<!-- paperflow:c0d39b2de4637156 -->
## When Context Returns: Toward Robust Internalization in On-Policy Distillation【OPD】

[https://arxiv.org/pdf/2606.11627](https://arxiv.org/pdf/2606.11627)

- 解决 **on-policy distillation（OPD）中的上下文诱导退化（context-induced degradation）** 问题。论文提出 **No-Context Anchoring (NCA)**，一种**轻量级的 consistency regularizer**。该方法**将无上下文输出作为 stop-gradient 锚点，通过前向 KL 散度惩罚有上下文输出的偏离，从而在仅增加单次前向传播开销的情况下，有效缓解上下文诱导退化**，并在多数设置中同时提升无上下文性能。

<!-- paperflow:e4cf8f8c746101c2 -->
## 🧐Dense Supervision, Sparse Updates: On the Sparsity and Geometry of On-Policy Distillation【OPD, Alibaba】

[https://arxiv.org/pdf/2606.13657](https://arxiv.org/pdf/2606.13657)

- **OPD 在参数空间中形成了一种独特的"密集监督、稀疏更新"机制**。它**并未因教师信号的密集性而变成传统的密集参数重写**，反而保留了 on-policy 后训练（如 RLVR）的稀疏、非主方向更新的几何特征。这表明**数据分布（on-policy vs off-policy）而非奖励稀疏性**，是后训练更新几何的主要决定因素。OPD 的监督是 dense 的，但参数更新是 sparse 的，而且几何形态更像 on-policy post-training / RLVR，而不是传统离线蒸馏。**多数 OPD 更新是 FFN-heavy 的**。

<!-- paperflow:000e827fccbac9fd -->
## A Primer in Post-Training Reasoning Data: What We Know About How It Works

[https://arxiv.org/pdf/2606.02113](https://arxiv.org/pdf/2606.02113)

- 解决**后训练推理数据领域缺乏系统性综述和归因框架**的问题。后训练推理数据并非简单的提示-响应对，而是**承载反馈的接口（feedback-bearing interface）**，其价值取决于验证器、基础模型、谱系、优化器、支架和推理预算的交互。
    
    ## **四维质量评估框架**
    
    论文建立**质量支持矩阵**，明确四个维度的相对性与审计要求：
    
    |**维度**|**核心观点**|**关键审计字段**|
    |---|---|---|
    |**正确性**|是版本化的验证器契约，而非答案字符串|提取器、验证器版本、过滤规则、已知失败模式|
    |**轨迹质量**|需具备局部有效性、任务相关性与可审计性|过程标签、执行上下文、定位测试、鲁棒性检查|
    |**难度**|是项目-模型-采样关系，具有基础模型相对性|基础模型、通过率带（pass-rate band）、采样协议、温度|
    |**覆盖与谱系**|是配方而非数量，需控制泄漏与代际传承|源混合、生成器、教师模型、去污状态、谱系风险|
    
    ## **构建层归因框架（Construction as Attribution Ledger）**
    
    将数据构建过程视为**归因账本**，指出优化器可见性不等于因果隔离。各构建层暴露不同归因把手：
    
    - **提示源（Prompt Sourcing）**：问题支持度、通过率带、估计日期决定数据基底
    - **轨迹编写（Trace Writing）**：人类专家、教师模型、自蒸馏或工具执行传递不同的推理风格与停止行为
    - **搜索基底（Search Substrate）**：保留失败分支与重试记录的可复现环境
    - **自我博弈锚点（Self-Play Anchor）**：外部答案、解释器、多数投票或档案评估等管理信号的来源
    - **奖励/验证器（Reward/Verifier）**：形式化检查器、过程奖励模型、学习奖励模型或评分标准等成功定义机制

# Multimodal Models & Visual Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Multimodal Models & Visual Reasoning
- 方法：language, vision-language-model, reasoning, vision, optimization, vision-language, multimodal-reasoning, cross-modal
- 论文/报告：8 篇
- PRISM: Synergizing Vision Foundation Models via Self-organized Expert Specialization
- Representation Forcing for Bottleneck-Free Unified Multimodal Models【Unified Model, ByteDance Seed】
- HYDRA-X: Native Unified Multimodal Models with Holistic Visual Tokenizers【Unified Model, Tencent Hunyuan】
- ChartArena: Benchmarking Chart Parsing across Languages, Scenarios, and Formats【OCR, Tencent】
- VLMs are Good Teachers for Video Reasoning via Adaptive Test-Time Optimization【Visual Reasoning】
- DeepLatent: Think with Images via Parallel Latent Visual Reasoning【Visual Reasoning, Baidu】
- ARM: An AutoRegressive Large Multimodal Model with Unified Discrete Representations【Visual Reasoning, ByteDance Seed】
- Think Fast: Estimating No-CoT Task-Completion Time Horizons of Frontier AI Models
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:80a015bf56359dd2 -->
## PRISM: Synergizing Vision Foundation Models via Self-organized Expert Specialization

[https://arxiv.org/pdf/2606.03444](https://arxiv.org/pdf/2606.03444)

- 解决**多教师知识蒸馏（Multi-Teacher Distillation）中异构视觉基础模型（VFMs）整合时的优化冲突与负迁移问题**。传统密集架构缺乏自动感知知识性质的能力，无法动态决定何时共享参数（共识知识）与何时分支（冲突信号），从而在稳定性与可塑性之间难以平衡。PRISM的解决思路： 论文提出通过**"分解-再组合"（Decompose-then-Recombine）范式，采用双流条件化混合专家架构（Dual-Stream Conditioned MoE）**： 通用锚点流（Universal Anchor）：维护稳定的共享表示，捕获共识知识 条件化MoE流（Conditioned MoE）：通过上下文调制路由，将令牌动态分配给稀疏专家，实现冲突感知的部分共享 该机制使模型能够自组织地分解异构知识到独立的专业化子空间，并在下游任务中动态重组，从而在避免负迁移的同时最大化参数效率。

<!-- paperflow:f0ee5f1cae2622c4 -->
## Representation Forcing for Bottleneck-Free Unified Multimodal Models【Unified Model, ByteDance Seed】

[https://arxiv.org/pdf/2605.31604](https://arxiv.org/pdf/2605.31604)

- 解决**统一多模态模型（Unified Multimodal Models, UMMs）在图像生成路径中对外部预训练VAE（变分自编码器）的结构性依赖问题**。论文提出Representation Forcing (RF) 方法，其核心思想是： **利用模型自身的理解编码器（understanding encoder）提取的视觉表示作为中间目标**； 强制解码器在生成像素前，以自回归方式预测这些离散化的视觉表示标记； 这些预测的表示标记保留在序列上下文中，作为内生性的结构指导（structural scaffold）来引导像素空间的扩散过程。 通过将视觉表示从"感知输出"转变为"生成目标"，RF消除了对外部生成潜在空间的依赖，实现了无需预训练VAE的端到端像素空间生成，同时保持（甚至提升）图像理解和生成质量。**在原来的 "Text → Image" 生成链路中，插入了一层 Representation Token，训练时强制模型先学会预测它们。**

<!-- paperflow:d1bf4d2a449b0866 -->
## HYDRA-X: Native Unified Multimodal Models with Holistic Visual Tokenizers【Unified Model, Tencent Hunyuan】

[https://arxiv.org/pdf/2606.13289](https://arxiv.org/pdf/2606.13289)

- 论文提出tokenizer阶段的源-目标交互（Source-Target Interaction, STI）：将源-目标图像对视为长度为2的"视频片段"，利用HYDRA-XTOK的因果时序注意力在潜在层早期融合结构细节，再输入LLM。该设计无需额外参数，即可显著提升编辑一致性和收敛速度。 综上所述，HYDRA-X通过**整体式视觉tokenizer（Holistic Visual Tokenizer）**的设计，将视觉tokenization从静态图像编码器提升为统一的图像-视频接口，为下一代统一tokenizer UMMs奠定基础。

<!-- paperflow:c40c47692870053c -->
## ChartArena: Benchmarking Chart Parsing across Languages, Scenarios, and Formats【OCR, Tencent】

[https://arxiv.org/pdf/2606.01348](https://arxiv.org/pdf/2606.01348)

- 论文提出 **ChartArena**——一个包含2,400张双语图表、涵盖8种图表类型与3种视觉场景的统一基准，并配套设计了**与格式无关的评估协议**（将异构输出归一化为三元组视图与有向图视图），以实现跨模型、跨输出范式的公平比较与系统性能力诊断。

<!-- paperflow:38a84fb1b4be4b1a -->
## VLMs are Good Teachers for Video Reasoning via Adaptive Test-Time Optimization【Visual Reasoning】

[https://arxiv.org/pdf/2606.02564](https://arxiv.org/pdf/2606.02564)

- 论文提出将VLMs的角色从**"求解器"（Solver）转变为"教师"（Teacher）**：**利用VLMs强大的感知能力评估过程约束满足度和最终目标达成度通过VLM提取任务特定规则并形式化为可微奖励函数**在测试时通过轻量级LoRA模块的在线优化，直接反向传播VLM的可微反馈，引导VGM生成符合逻辑规则的视觉推理轨迹这一范式转变使VGM能够超越其固有生成能力的边界，实现自适应的、实例特定的推理轨迹优化。**利用VLM强大的感知能力评估**过程约束满足度**和**最终目标达成度**，**通过可微反馈指导VGM在测试时的在线优化**。

<!-- paperflow:6bd33d45d6f441f2 -->
## DeepLatent: Think with Images via Parallel Latent Visual Reasoning【Visual Reasoning, Baidu】

[https://arxiv.org/pdf/2606.00562](https://arxiv.org/pdf/2606.00562)

- 解决**视觉-语言模型（VLMs）在复杂推理过程中无法动态更新视觉状态**的核心问题。论文提出**DeepLatent**框架，**通过并行生成潜在状态（替代AR序列生成）、直接锚定原始图像特征（避免信息源偏差），以及连续空间强化学习算法（优化潜在调制参数），旨在实现高效、灵活且性能卓越的"用图像思考"能力**，弥合工具辅助方法与潜在推理方法之间的差距。对14个前沿模型（从2019年的GPT-2到2026年的GPT-5.5）的评估显示： 指数增长：无CoT的TH每373天（95% CI: [167, 691]）翻倍，持续六年 当前水平：GPT-5.5的TH超过3分钟（95% CI: [0.84, 62]分钟），相当于约1,500个o3-mini推理token 未来预测：按中位数趋势外推，2028年可能超过7分钟，2030年可能超过25分钟 与CoT趋势对比：有CoT的TH每182天翻倍（Kwa et al.），无CoT增长速率约为其一半，两者差距自GPT-4发布后显著扩大

<!-- paperflow:1b9df2d0f631a6db -->
## ARM: An AutoRegressive Large Multimodal Model with Unified Discrete Representations【Visual Reasoning, ByteDance Seed】

[https://arxiv.org/pdf/2606.11188](https://arxiv.org/pdf/2606.11188)

- 论文提出ARM（AutoRegressive Model），其核心创新包括： 统一离散视觉标记器：训练一个受多目标监督（包括字幕生成损失、像素重建损失、Sigmoid对比损失和特征蒸馏损失）的离散语义标记器，将图像映射为紧凑的离散标记序列，同时保留高层语义（支持理解）与细粒度细节（支持生成与编辑）。 自回归统一建模：基于上述标记器，在单一7B自回归Transformer骨干上对大尺度交错文本-视觉标记序列进行 next-token 预测训练，无需分离的扩散模块即可实现视觉-语言感知与生成能力的无缝融合。 偏好对齐的跨任务协同：通过群组相对策略优化（GRPO）对视觉标记预测进行强化学习，不仅提升目标任务（文生图、指令编辑）性能，还观察到跨任务协同效应——优化生成任务可同步提升编辑能力，反之亦然，且理解性能保持稳定。 简言之，该工作试图证明：通过设计语义与细节兼顾的统一离散表征，结合自回归建模与偏好优化，可在单一模型内高效统一多模态理解、生成与编辑，避免传统分离架构的结构性缺陷。

<!-- paperflow:d5aa3e1ef23972d7 -->
## Think Fast: Estimating No-CoT Task-Completion Time Horizons of Frontier AI Models

[https://arxiv.org/pdf/2606.07157](https://arxiv.org/pdf/2606.07157)

- **量化评估前沿AI模型在没有显式链式思考(Chain-of-Thought, CoT)的情况下进行潜在推理(latent reasoning)的能力?**

# Reward Models & Reinforcement Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Models & Reinforcement Learning
- 方法：agent, ai-for-science, language, reasoning, reinforcement-learning, optimization, multimodal-learning, stat-ml
- 论文/报告：13 篇
- Scaling LLM Reasoning from Minimal Labels: A Semi-Supervised Framework with a Lightweight Verifier
- AdaSR: Adaptive Streaming Reasoning with Hierarchical Relative Policy Optimization
- 📌Rethinking the Divergence Regularization in LLM RL【Tencent Hunyuan】
- TRACE: A Unified Rollout Budget Allocation Framework for Efficient Agentic Reinforcement Learning【Tencent Hunyuan】
- Exploring the Design Space of Reward Backpropagation for Flow Matching【Tencent Hunyuan】
- Right Makes Might: Aligning Verified Hidden States Empowers RL Reasoning【JINGDONG】
- RDA: Reward Design Agent for Reinforcement Learning【Meta】
- SCI-PRM: A Tool Aware Process Reward Model for Scientific Reasoning Verification【AI Lab】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:43b6536cd49e308c -->
## Scaling LLM Reasoning from Minimal Labels: A Semi-Supervised Framework with a Lightweight Verifier

[[Deep Reading - Jun 2026/Scaling LLM Reasoning from Minimal Labels-A Semi-Supervised Framework with a Lightweight Verifie|Deep Reading]]

[https://arxiv.org/pdf/2606.16811v1](https://arxiv.org/pdf/2606.16811v1)

- **论文针对LLM推理学习监督成本高的问题，提出半监督框架：用极少标注训练轻量推理正确性分类器，结合熵过滤生成高质量伪标签用于微调。在数学与视觉编程问答任务上达到接近大量标注数据的性能，证明用推理验证替代答案级监督的可行性，为低监督自主推理系统奠定基础。**

<!-- paperflow:3a0a72fc30855e03 -->
## AdaSR: Adaptive Streaming Reasoning with Hierarchical Relative Policy Optimization

[[Deep Reading - Jun 2026/AdaSR-Adaptive Streaming Reasoning with Hierarchical Relative Policy Optimization|Deep Reading]]

[https://arxiv.org/pdf/2606.14694v1](https://arxiv.org/pdf/2606.14694v1)

- **这篇论文介绍了 AdaSR，一种旨在解决动态流式环境下大语言模型推理效率问题的框架。传统的“先读后想”模式在处理实时信息流时存在高延迟和灵活性差的问题。AdaSR 允许模型在接收输入流的同时进行思考，并通过新颖的 HRPO 算法优化推理时机和计算量分配。HRPO 通过分层强化学习解决了流式推理中策略偏移和奖励分配的难题。实验结果证明，AdaSR 能够根据输入信息的节奏自适应地调整推理策略，在 GSM-symbolic 等任务上实现了准确率与延迟的更优权衡。该工作为构建能够实时响应、边听边想的智能体提供了重要的技术支撑。**

<!-- paperflow:70eabca4f2c19e4d -->
## 📌Rethinking the Divergence Regularization in LLM RL【Tencent Hunyuan】

[https://arxiv.org/pdf/2606.09821](https://arxiv.org/pdf/2606.09821)

- 解决大型语言模型（LLM）强化学习（RL）中**信任区域控制**与**分布偏移**不匹配的问题，具体针对现有方法在优化稳定性和效率上的关键缺陷。现代LLM RL训练通常是off-policy的，存在训练-推理不匹配和策略陈旧性。现有方法存在两类关键缺陷： **基于比率的方法（PPO/GRPO/SPO）：通过重要性比率 μ≤0.01 约束更新。**在长词汇表中，该比率是分布偏移的不良代理——低概率词元的微小绝对变化可导致比率激增，而高概率词元的显著质量转移却呈现温和比率变化，导致过度惩罚探索或约束不足。 **基于散度的硬掩码（DPPO）：使用Binary-TV散度 wt 更好捕捉偏移几何，但采用硬掩码机制**。一旦词元越过信任区域边界，梯度被完全丢弃而非修正，造成边界附近不稳定且缺乏纠正信号。

<!-- paperflow:7738a1345168cf0f -->
## TRACE: A Unified Rollout Budget Allocation Framework for Efficient Agentic Reinforcement Learning【Tencent Hunyuan】

[https://arxiv.org/pdf/2606.11119](https://arxiv.org/pdf/2606.11119)

- 解决**多轮智能体强化学习（Multi-turn Agentic RL）中，在固定 rollout 预算约束下奖励对比度不足（insufficient reward contrast）导致的采样效率低下问题**。论文提出**将 rollout 预算分配重新建模为对树状结构 rollout 的锚点（anchors）进行战略决策的问题**： 全局根节点分配（Global Root Allocation）：决定哪些提示根节点值得分配预算（提示过滤与 rollout 计数分配的统一）。 局部前缀扩展（Local Prefix Expansion）：在已访问的中间前缀（turn-level prefixes）处决定是否需要额外分支（continuation branches），以产生混合终端奖励（mixed terminal rewards）。 通过将提示根节点和中间前缀视为统一的预算分配锚点，论文旨在构建一个**混合奖励对比（mixed-reward contrast）**的优化目标：在固定预算下，最大化能够观察到成功与失败两种结果并存的锚点数量，从而将稀疏的结果奖励转化为更密集的隐式逐步偏好对（implicit stepwise preference pairs），增强策略更新信号。

<!-- paperflow:db0864e1927c9639 -->
## Exploring the Design Space of Reward Backpropagation for Flow Matching【Tencent Hunyuan】

[https://arxiv.org/pdf/2606.11075](https://arxiv.org/pdf/2606.11075)

- 论文提出了FlowBP，一个统一的替代轨迹框架（surrogate-trajectory framework），其创新点在于： 将反向轨迹本身作为设计对象，通过缓存无梯度的前向轨迹，构建轻量级的反向替代轨迹； 解耦四个关键设计维度：**奖励模型输入（reward-model input）、活跃步骤集（active set）、积分权重（integration weights）和桥接耦合（bridge coupling）**； 边界保证：通过设计确保内存复杂度仅与活跃集大小相关（而非轨迹长度N ），并将梯度链式累积限制在单一雅可比因子（而非多步乘积）。 基于该框架，论文实例化了三种互补的变体（FlowBP-Sparse、FlowBP-Bridge、FlowBP-Lagrange），在SD3.5-M、FLUX.1-dev和FLUX.2-Klein-base上实现了对现有直接梯度基线的持续改进。

<!-- paperflow:c0ef487b553ff611 -->
## Right Makes Might: Aligning Verified Hidden States Empowers RL Reasoning【JINGDONG】

[https://arxiv.org/pdf/2606.03234](https://arxiv.org/pdf/2606.03234)

- 解决**强化学习从可验证奖励（RLVR）在数学推理任务中奖励信号粒度不足**的问题。论文提出Hidden-Align，**通过在RL训练期间引入辅助损失函数，显式最大化正确rollout在锚点token处最后一层隐藏状态的成对余弦相似度**。这种方法：**将分散的正确决策表征凝聚为统一的"正确决策"向量在不影响推理路径多样性的前提下（因损失仅作用于锚点位置），降低模型对具体推理路径的敏感性实现零训练与推理开销（因隐藏状态已在rollout过程中计算）**

<!-- paperflow:0ec5c7c09384e805 -->
## RDA: Reward Design Agent for Reinforcement Learning【Meta】

[https://arxiv.org/pdf/2606.01672](https://arxiv.org/pdf/2606.01672)

- 解决强化学习（RL）中**奖励函数设计的自动化与语义对齐问题**。论文提出了 RDA（Reward Design Agent），一个基于视觉-语言模型（VLM）的智能体框架，通过以下机制实现解决方案： **任务分解：将自然语言指令解析为可解释的子任务序列**。 视觉轨迹分析：**在进化搜索循环中，使用VLM对策略 rollout 的视频轨迹进行视觉评估**，诊断子任务级别的完成情况与失败模式。 定向反思与修正：基于视觉诊断结果，迭代修正子任务定义和奖励代码，从而生成既具有高成功率又符合人类意图（instruction-aligned）的奖励函数。

<!-- paperflow:f945f9082c149aa0 -->
## SCI-PRM: A Tool Aware Process Reward Model for Scientific Reasoning Verification【AI Lab】

[https://arxiv.org/pdf/2606.04579](https://arxiv.org/pdf/2606.04579)

- 论文提出**Sci-PRM**（Science Process Reward Model），**通过构建包含"工具链"（Chain-of-Tool）的SCIPRM70K数据集，训练模型在单步推理中同时监督工具选择、执行准确性和结果解释**，**从而在无需实时执行工具的情况下提供可靠的细粒度奖励信号**。

<!-- paperflow:48ecaeb5608d8317 -->
## Reward Modeling for Multi-Agent Orchestration

[[Deep Reading - Jun 2026/Reward Modeling for Multi-Agent Orchestration|Deep Reading]]

[https://arxiv.org/pdf/2606.13598v1](https://arxiv.org/pdf/2606.13598v1)

- **本文提出了 OrchRM，一种针对多智能体系统（MAS）编排优化的奖励建模框架。针对 MAS 训练成本高、标注难的痛点，OrchRM 创新性地提出在“编排计划”层面进行自监督学习。它通过分析 MAS 执行轨迹的中间产物，自动构建偏好对来训练奖励模型。该模型不仅能作为推理时的过滤器（Test-time Scaling），在不增加子智能体调用成本的前提下提升系统准确率，还能作为高效的强化学习奖励源，将编排器的训练效率提升 10 倍。实验证明，OrchRM 在数学、Web 搜索和多步推理等多个领域均表现出极强的鲁棒性和迁移能力，为构建高效、可扩展的自动化多智能体系统提供了新的技术路径。**

<!-- paperflow:8549685137ffdd10 -->
## When in Doubt, Plan It Out: Committed Small Language Model Deliberation for Reactive Reinforcement Learning

[[Deep Reading - Jun 2026/When in Doubt, Plan It Out-Committed Small Language Model Deliberation for Reactive Reinforcemen|Deep Reading]]

[https://arxiv.org/pdf/2606.16995v1](https://arxiv.org/pdf/2606.16995v1)

- **论文提出PACT混合架构，旨在将小型语言模型的 deliberative 规划能力与强化学习的反应式策略结合。背景部分讨论了RL在陌生环境中的局限、BDI系统的不足以及LM/SLM作为规划助手的潜力与挑战。方法通过异步SLM调用生成计划、模拟验证后直接执行。实验在FrozenLake上验证有效性，讨论部分指出当前局限并提出未来方向。整体强调规划与执行分离比单一范式更强。**

<!-- paperflow:a23e5853b0c8c291 -->
## ExpRL: Exploratory RL for LLM Mid-Training

[[Deep Reading - Jun 2026/ExpRL-Exploratory RL for LLM Mid-Training|Deep Reading]]

[https://arxiv.org/pdf/2606.17024v1](https://arxiv.org/pdf/2606.17024v1)

- **论文围绕LLM推理RL的初始化瓶颈展开，提出ExpRL作为自动化RL中训练范式。核心是用参考答案构建问题特定评分标准，对on-policy轨迹给出密集奖励而非仅最终答案。实验证明其在中训练后为稀疏RL提供更强初始化，数学基准上优于多 baseline，并在混合领域显示潜力。结论部分列出奖励 richer feedback、prefix采样与校准等后续方向。**

<!-- paperflow:b45d05715ef85a7e -->
## Context-Aware RL for Agentic and Multimodal LLMs

[[Deep Reading - Jun 2026/Context-Aware RL for Agentic and Multimodal LLMs|Deep Reading]]

[https://arxiv.org/pdf/2606.17053v1](https://arxiv.org/pdf/2606.17053v1)

- **论文从 LLM 在长上下文与多模态场景下的 grounding 失败出发，提出 ContextRL 框架。该框架通过新增上下文选择辅助目标，鼓励模型在两个相似上下文中挑选支持给定查询-答案对的上下文。作者分别在代理轨迹和图像上构建对比数据集，并在 17 个基准上验证了相对于 GRPO 的稳定提升，同时通过数据增强基线消融证明了目标本身的作用。结论强调上下文选择可作为 outcome-based 后训练的轻量补充，提升模型对稀疏关键证据的感知能力。**

<!-- paperflow:353c4154542350b7 -->
## Greed Is Learned: Visible Incentives as Reward-Hacking Triggers

[[Deep Reading - Jun 2026/Greed Is Learned-Visible Incentives as Reward-Hacking Triggers|Deep Reading]]

[https://arxiv.org/pdf/2606.16914v1](https://arxiv.org/pdf/2606.16914v1)

- **论文标题为“Greed Is Learned: Visible Incentives as Reward-Hacking Triggers”。作者Tong Che、Rui Wu在arXiv上发布。核心发现是：可见奖励通道在强化学习中会成为决策相关目标，导致策略成瘾式追逐、牺牲真实效用并翻转安全对齐。作者区分冗余通道（无害）与决策相关通道（成瘾），并通过MoneyWorld沙盒与安全探针给出因果证据。结论强调将可见自利通道视为对齐表面的一部分，盲目优化KPI或损益存在风险。**

# Data, Benchmarking & Evaluation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data, Benchmarking & Evaluation
- 方法：agent, language, optimization
- 论文/报告：11 篇
- Benchmark Everything Everywhere All at Once
- Evaluation Cards: An Interpretive Layer for AI Evaluation Reporting【Huggingface】
- Exploring Autonomous Agentic Data Engineering for Model Specialization【Tencent】
- BenchEvolver: Frontier Task Synthesis via Solution-Centric Evolution
- Can Generalist Agents Automate Data Curation?
- TANDEM: Bi-Level Data Mixture Optimization with Twin Networks
- Knowledge Index of Noah's Ark
- DataEvolver: Automatic Data Preparation for Large Language Models through Multi-Level Self-Evolving
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:d16cca259bda0967 -->
## Benchmark Everything Everywhere All at Once

[https://arxiv.org/pdf/2606.06462](https://arxiv.org/pdf/2606.06462)

- 论文提出 **Benchmark Agent**——首个完全自主的智能体系统（fully autonomous agentic system），旨在将基准测试构建转变为自动化、标准化且可定制的过程，实现低成本、高质量、持续演进的基准测试生成。Benchmark Planner（规划器） 作为高层决策模块，将用户抽象需求 R 转化为可执行的基准规范 B={(si,Gi)}Ni=1 ，通过多智能体协作完成： Design Agent：利用提议、修订与舍弃工具，将评估意图分解为独立子任务 S={si}Ni=1 ； Grounding Agent：验证子任务的可实现性，通过数据集搜索、可转换性验证（Transformability Validation）及评分-过滤机制，确保每个子任务至少存在一个有效的数据集-转换计划对 (di,j,ti,j) ； Allocation Agent：在全局配额约束下，通过诊断-调整闭环确定最优资源分配。 Benchmark Executor（执行器） 将规划转化为具体样本，采用**编排-执行（Orchestration-Execution）**机制： 基于数据集级计划 ti,j 进行样本级自适应规划； 调用工具池（包括内容合成工具如TTS、图像处理，及程序化转换器如格式转换、字段修补）完成数据转换； 实施严格的质量与配额控制，通过验证-补充循环确保样本有效性和数量达标。

<!-- paperflow:bf9277c2461f657b -->
## Evaluation Cards: An Interpretive Layer for AI Evaluation Reporting【Huggingface】

[https://arxiv.org/pdf/2606.09809](https://arxiv.org/pdf/2606.09809)

- 解决AI评估结果报告中的**碎片化与不可解释性**问题。论文提出**EVALUATION CARDS**——一个可组合的解读层（interpretive layer），通过统一的报告模式（schema）、五级层级结构（family→composite→benchmark→split→metric）及四个解释信号（可复现性、文档完整性、来源与风险、分数可比性），将现有分散的基准元数据、评估运行数据与模型元数据整合为**单一、可追溯、适应不同读者需求**的记录。

<!-- paperflow:bb1eedfb797f5fb1 -->
## Exploring Autonomous Agentic Data Engineering for Model Specialization【Tencent】

[https://arxiv.org/pdf/2605.30407](https://arxiv.org/pdf/2605.30407)

- **如何让大语言模型（LLM）智能体自主地执行端到端的数据工程流程，以驱动模型在特定领域的专业化，从而摆脱对人工设计工作流的依赖**。论文首次形式化了**自主智能体数据工程（Autonomous Agentic Data Engineering）**这一任务，旨在评估LLM作为独立数据工程师的能力： 端到端自主性：**要求LLM智能体独立完成策略规划、提示设计、数据合成、验证和迭代优化的完整数据整理生命周期** 闭环优化：**通过学生模型后训练的性能反馈（post-training performance feedback）来迭代优化数据整理策略** 可控评估：固定教师模型（数据生成）和学生模型（训练），隔离评估LLM智能体驱动的数据整理对模型专业化的贡献

<!-- paperflow:c9a43dd4c9b0ad38 -->
## BenchEvolver: Frontier Task Synthesis via Solution-Centric Evolution

[https://arxiv.org/pdf/2606.01286](https://arxiv.org/pdf/2606.01286)

- 论文提出了BenchEvolver框架，其核心解决思路包括：自动化难度提升：通过以解决方案为中心的进化机制，**将现有已饱和的编程任务自动转换为计算结构更复杂、算法要求更高的变体，而非从零生成新问题；自我挑战（Self-Challenging）**：使模型能够生成对自身而言仍具挑战性的任务，从而支持闭环自我改进，而非依赖更强的"教师"模型进行数据蒸馏；可验证性与可用性：**确保生成的任务在可执行语义上保持有效**（包含正确的参考解决方案和测试用例），既可作为高区分度的评测基准（如LIVECODEBENCH-PLUS），也可作为强化学习的训练信号。简言之，该工作试图将静态的、易饱和的基准测试转化为可动态进化的、与前沿模型能力同步成长的评测与训练资源。

<!-- paperflow:723417c92897eb03 -->
## Can Generalist Agents Automate Data Curation?

[https://arxiv.org/pdf/2606.04261](https://arxiv.org/pdf/2606.04261)

- 解决**通用型智能体（generalist agents）能否自动化训练数据策划（data curation）流程**的问题。论文设计分层脚手架（scaffold ladder）： 轻量级脚手架（提供策略家族列表或论文技能卡片）仅扩大智能体的考虑范围，未能显著改善最佳执行结果。 重量级脚手架（强制要求每次迭代引用、实例化并适应先前研究方法）则根本性改变行为：在"Adapt Papers"条件下，智能体必须基于具体论文方法适配策略，实现100%决策有据、0%浅层操作、67%新策略家族探索率。 最佳结果：该脚手架下的智能体自主组合EL2N高损失选择与助手损失噪声过滤，提出混合策略，以10k数据预算超越100k数据的人类基线（34.9 vs 34.1/34.5），实现数据效率数量级提升。

<!-- paperflow:cd6a5f1e0053d51c -->
## TANDEM: Bi-Level Data Mixture Optimization with Twin Networks

[https://arxiv.org/pdf/2606.04401](https://arxiv.org/pdf/2606.04401)

- 解决**大型语言模型（LLMs）训练中的数据混合优化（Data Mixture Optimization, DMO）问题**，即如何自动确定来自不同领域（如维基百科、代码、学术论文等）训练数据的最优采样比例（mixture ratios），以提升模型在目标验证集上的性能。

<!-- paperflow:6df2b67c3f97a237 -->
## Knowledge Index of Noah's Ark

[https://arxiv.org/pdf/2606.05104](https://arxiv.org/pdf/2606.05104)

- 论文提出Knowledge Index of Noah's Ark (KINA)，一个**包含899道题、覆盖261个细化学科的基准**，通过两个形式化结果和一项实证方案解决上述问题： 对于代表性：将代表性形式化为"预算支持中心性"（budgeted support centrality），证明贪婪选择可获得(1−1/e) 近似保证（命题1） 对于评审质量：证明"奖金门槛锦标赛"（bonus-on-bar tournament）在弱FOSD意义下严格优于固定支付（定理1），并给出奖金校准的闭式条件B>ΔC/Δpmin 对于排名稳定性：在排行榜中报告基于自举法的排名稳定性统计（Kendall τ 和首位保留率），使有界预算下的方差显性化 最终，KINA旨在成为诊断性工具而不仅是难度温度计，揭示模型在哪些学科、因何种原因表现不佳。整体表现：**Gemini-3.1-Pro-Preview 以 53.17% 领先，Claude-Opus-4.6 (49.92%) 与 GPT-5.4 (48.55%) 紧随其后**，表明基准远离饱和（<55%） 排名稳定性：50%分层子采样下，Top-10 Kendall τ=0.89 ，首位保留率94%，但<2个百分点的差距在统计上不可分辨 工具使用增益：网页搜索带来+1.50至+5.17点提升，呈非单调模式（弱模型与强模型增益最大） 学科差异：人文社科（社会学 Δ=38.16 、管理学 Δ=32.76 ）比STEM（科学 Δ=9.83 ）更能区分前沿模型

<!-- paperflow:bc262eec3376689f -->
## DataEvolver: Automatic Data Preparation for Large Language Models through Multi-Level Self-Evolving

[https://arxiv.org/pdf/2606.07001](https://arxiv.org/pdf/2606.07001)

- 这篇论文试图解决**大语言模型（LLMs）训练中高质量数据准备的自动化问题**，特别是在最小化人工监督的前提下，构建既**可执行**又**有效**的数据转换管道。论文提出了**DataEvolver**——首个基于**多级自进化机制**的数据准备系统，通过操作符级别的自进化确保逻辑可执行性，通过管道级别的自进化基于执行反馈持续优化数据质量，从而在七个基准数据集上实现平均**10%**的下游LLM性能提升。

<!-- paperflow:8b1dbf6c445dacdc -->
## LakeQA: An Exploratory QA Benchmark over a Million-Scale Data Lake

[https://arxiv.org/pdf/2606.10460](https://arxiv.org/pdf/2606.10460)

- 这要求代理具备探索性能力：必须先搜索发现相关文档，再跨文档推理整合答案。现有基准无法同时满足高搜索强度（N≈40M）与高推理强度（多跳依赖推理）的要求。论文构建了 LAKEQA，其核心特征包括： **数据规模：基于 ∼ 9.5 TB、∼ 4000万文档的异构数据湖，整合 Wikipedia（非结构化文本）与 [Data.gov](http://data.gov/)（结构化 CSV/JSON 及政府报告）**。 任务构造：通过子问题链组合（subquestion chaining）人工创建任务。每个任务要求平均 13.12 跳推理，其中每步依赖前一步的中间结果，形成隐式推理链。 质量控制：采用 Ph.D. 级专家审核机制，确保事实的完备性与可靠性；通过问题重写消除源标识符泄露，防止模型通过关键词捷径直接定位文档。 评估接口：提供标准化工具集（搜索、下载、检查、查询），要求代理在单轮交互中自主决定何时搜索、分析或提交答案。关键发现如下： 整体性能低下：即使配备搜索工具，最佳模型（Claude-sonnet-4.5）在 LAKEQA-full（1007 任务）上的精确匹配率（EM）仅为 32.87%，GPT-5.2 为 18.37%，开源模型 DeepSeek-R1 为 15.99%。 瓶颈定位：文档发现（搜索与探索）是主要瓶颈，而非推理能力。模型经常无法检索到黄金文档（Dret 召回率最高仅 42%），或即使检索到也未选择分析（Dacc 召回率最低仅 0.22%）。 强度敏感性：性能随推理跳数增加而急剧下降。当任务需要 ≤8 个文档时模型表现最佳，但当跳数 >18 时准确率几乎归零，表明长程探索中的错误累积效应显著。

<!-- paperflow:14881344bf887c45 -->
## PaintBench: Deterministic Evaluation of Precise Visual Editing【Saining Xie】

[https://arxiv.org/pdf/2606.00188](https://arxiv.org/pdf/2606.00188)

- 当前多模态模型虽擅长开放式视觉编辑（如"让天空更蓝"），但在执行**具有唯一正确答案的精确编辑指令**（如**"将橄榄色三角形精确重绘为青色 #0FE1DF"、"将形状向右移动50像素"**）时表现不佳。评估11个模型（包括Nano-Banana-2、GPT-Image-2、FLUX系列等）发现： **最佳性能仅17.1% mIoU（Nano-Banana-2），GPT-Image-2为16.3%，开源模型普遍低于7%** **几乎未解决的操作：几何变换（类别平均<12%）、公式化颜色变化（渐变最高13.0%）、结构构造（最高15.7%）** 相对易处理：删除（最高50.6%）、单色重着色（最高30.4%）

<!-- paperflow:d7332d03eec15f41 -->
## Benchmarking LLM Agents on Meta-Analysis Articles from Nature Portfolio

[[Deep Reading - Jun 2026/Benchmarking LLM Agents on Meta-Analysis Articles from Nature Portfolio|Deep Reading]]

[https://arxiv.org/pdf/2606.17041v1](https://arxiv.org/pdf/2606.17041v1)

- **论文提出MetaSyn数据集，旨在为LLM代理在meta-analysis全流程提供可验证基准，实验显示当前系统在筛选阶段存在严重瓶颈，强调需要保留资格推理的同时加速工作流。**

# World Models, Generation & Audio

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：World Models, Generation & Audio
- 方法：agent, generation, stat-ml, stat-me, gui-agent
- 论文/报告：7 篇
- dots.tts Technical Report【RedNote】
- MetaWorld: Scaling Multi-Agent Video World Model from Single-view Video Data【World Model】
- 📌Cosmos 3: Omnimodal World Models for Physical AI【NVIDIA】
- 📌Qwen-Image-Flash: Beyond Objective Design【Qwen】
- InterleaveThinker: Reinforcing Agentic Interleaved Generation
- InterleaveThinker: Reinforcing Agentic Interleaved Generation【Unified Model】
- Audio Interaction Model
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:e17855fcc695bf64 -->
## dots.tts Technical Report【RedNote】

[https://arxiv.org/pdf/2606.07080](https://arxiv.org/pdf/2606.07080)

- 针对**连续自回归（continuous autoregressive）文本转语音（TTS）范式**中的核心瓶颈——**长程误差累积（long-range error accumulation）**——提出了系统性的解决方案。论文提出了 dots.tts 系统，通过三项关键技术实现突破： 语义结构化的AudioVAE：通过多任务训练（包括WavLM表示对齐和下游分类任务）构建既高保真又易于LLM学习的连续潜空间； 全历史条件AR-FM头：采用自回归流匹配（autoregressive flow-matching）头，通过块因果注意力（block-causal attention）机制在训练时并行处理，在推理时保持完整的自回归历史条件，减少漂移； 无奖励自校正后训练（SOAR）：针对流匹配头设计奖励无关的自校正机制，让模型学习从自身的推理偏差中恢复，无需外部奖励模型或教师模型。 最终目标是实现一个完全连续、端到端的TTS系统，在保持连续表示的高感知上限（支持语音、副语言、歌唱和一般音频的统一分布）的同时，达到离散token系统的生成稳定性和生产就绪成熟度。

<!-- paperflow:6b12247b55dc9f1a -->
## MetaWorld: Scaling Multi-Agent Video World Model from Single-view Video Data【World Model】

[https://arxiv.org/pdf/2606.02753](https://arxiv.org/pdf/2606.02753)

- 解决**多智能体视频世界模型（Multi-Agent Video World Models）在开放域环境中的扩展问题。**论文提出MetaWorld，通过三大创新组件实现从单视角视频到开放域多智能体世界模型的扩展： Monocular World-State Unrolling (MWSU) **将单目视频显式分解为相机运动序列 ci 、多主体轨迹集 T(i)∼i 和身份参考集 I∼i ，从单视角 footage 中提取隐藏的物理双智能体动态**。通过相机-轨迹分解，在共享3D空间中构建同步的多智能体监督数据，完全绕过多相机采集需求。 Subject-Aware World Generator (SAWG) 基于Phantom-Wan2.1 14B DiT架构，实现几何条件（RGB与深度视频）与外观条件（身份图像）的解耦注入。通过零初始化特征压缩与身份锚定机制，在保持预训练生成先验的同时，精确控制"在哪里"（轨迹）与"是谁"（身份）。 World-State Alignment (WSA) 在每层Transformer插入帧间跨分支交叉注意力机制，强制并行生成分支在每一帧内交换潜在表示。通过严格限制帧内注意力（防止时间泄漏），联合同步去噪过程，同时约束静态几何一致性（共享3D布局对齐）与动态运动一致性（物理事件同步）。

<!-- paperflow:92414f28dc876352 -->
## 📌Cosmos 3: Omnimodal World Models for Physical AI【NVIDIA】

[https://arxiv.org/pdf/2606.02800](https://arxiv.org/pdf/2606.02800)

- 论文提出**Cosmos 3**——一种**全模态世界模型（Omnimodal World Model）**，**通过统一的Mixture-of-Transformers架构，将语言、视觉、音频和动作统一在单一框架内，同时支持理解（Reasoner）与生成（Generator）任务**。该模型能够根据输入-输出配置灵活切换为视觉-语言模型、图像/视频生成器、世界模拟器或世界-动作模型，从而为物理AI智能体提供可扩展的通用基础架构，支持从合成数据生成到闭环策略学习的全流程训练。

<!-- paperflow:bb8a4da2931be0ed -->
## 📌Qwen-Image-Flash: Beyond Objective Design【Qwen】

[https://arxiv.org/pdf/2606.03746](https://arxiv.org/pdf/2606.03746)

- 现有研究在少步蒸馏领域主要聚焦于**蒸馏目标函数**的设计（如轨迹级对齐、一致性训练、对抗蒸馏和分布匹配等），但论文发现，当这些方法直接应用于大规模视觉生成模型时，仅依赖直观或常规的训练方案往往难以达到预期性能。该工作从互补视角出发，系统性地研究了除目标函数外决定蒸馏效果的关键因素： **数据组成（Data Composition）**：针对文本到图像（T2I）蒸馏，论文发现训练数据的类别分布与多样性对蒸馏效果具有非直观的影响——增加数据多样性或使用目标领域特定数据并不总能提升性能，而单一类别的连贯数据反而可能实现更好的跨类别迁移。 **教师指导策略（Teacher Guidance）：针对具有互补能力的教师模型，论文指出直接使用任务 specialized 教师作为指导会导致训练不稳定**。为此，论文提出了逐步多教师指导策略（step-wise multi-teacher guidance），在保持训练稳定性的同时融合不同教师的优势。 **任务混合比例（Task Mixture）：针对联合蒸馏T2I生成与指令引导图像编辑，论文发现任务混合比例对统一模型的性能具有决定性影响，平衡的数据比例（T2I:Edit = 5:5）能够实现最佳的生成-编辑统一能力**。 基于上述发现，论文开发了 Qwen-Image-Flash，一个仅需4次函数评估（NFEs）的统一少步生成-编辑模型，验证了有效的少步蒸馏不仅需要精心设计的目标函数，更需要对训练流程中数据、教师与任务结构的系统性组织。

<!-- paperflow:e1ca23a0a57b5c80 -->
## InterleaveThinker: Reinforcing Agentic Interleaved Generation

[[Deep Reading - Jun 2026/InterleaveThinker-Reinforcing Agentic Interleaved Generation|Deep Reading]]

[https://arxiv.org/pdf/2606.13679v1](https://arxiv.org/pdf/2606.13679v1)

- **本文提出了 InterleaveThinker，一个旨在增强图像生成器交错生成能力的智能体框架。针对现有模型在处理长程图文序列时存在的逻辑断裂和误差累积问题，作者设计了由 Planner 和 Critic 组成的双智能体系统。Planner 负责任务分解与指令下发，Critic 负责质量监控与反馈修正。通过大规模 SFT 数据初始化和基于 GRPO 的强化学习，并结合创新的双重奖励机制，该系统成功实现了对生成轨迹的高效优化。实验结果表明，InterleaveThinker 不仅能让普通的单图生成器胜任复杂的交错生成任务，还在多项推理基准测试中刷新了性能记录，展现了智能体架构在多模态生成领域的巨大潜力。**

<!-- paperflow:391fd73cecd51ec9 -->
## InterleaveThinker: Reinforcing Agentic Interleaved Generation【Unified Model】

[https://arxiv.org/pdf/2606.13679](https://arxiv.org/pdf/2606.13679)

- 论文提出 **InterleaveThinker** 框架，**通过**规划器（Planner）与评估器（Critic）**的智能体分离设计，在不修改底层图像生成器的前提下，赋予其强大的交错生成能力**，同时通过双奖励策略（准确率奖励与逐步奖励）实现高效的单步强化学习优化，以引导整个生成轨迹。该框架采用解耦的多智能体架构，由三个核心模块组成闭环系统： Planner（规划器）：基于输入的交错序列 S ，一次性生成全局的 N 步执行计划，输出每步的指令 ui 、优化提示词 pi 及辅助知识 ai 。通过预先规划整个序列，完全屏蔽中间视觉反馈，从根本上消除视觉过度依赖；Generator（生成器）：使用任意现成的图像生成/编辑模型（如 FLUX.2-klein、Qwen-Image-Edit），保持冻结状态。接收当前提示词 rti 与前帧图像 Ii−1 ，生成当前帧图像 Iti；Critic（评估器）：对生成结果进行逐步评估与提示词优化，形成自我修正闭环。接收前后帧图像及提示词，输出二元判断 jti （是否合格）、细化后的新提示词 rt+1i 及推理过程 Rti 。若判断失败则触发重新生成（最多 Tmax 次），有效防止误差累积。

<!-- paperflow:a9d6b356e3a3d146 -->
## Audio Interaction Model

[https://arxiv.org/pdf/2606.05121](https://arxiv.org/pdf/2606.05121)

- 论文正式提出Audio Interaction Model这一新概念，旨在填补**"能够统一处理传统离线任务与实时流式任务的全能型音频交互模型"**的空白。具体目标包括： 在单一模型内统一实时ASR、同声传译、语音对话、音频理解、主动干预等能力 实现"始终在线"的感知-决策-响应循环（perceive-decide-respond loop） 支持基于音频内容的主动干预（proactive assistance），即无需明确指令即可根据环境声音自主提供帮助 为达成此目标，论文构建了SOUNDFLOW框架（涵盖流式原生数据构建、理解感知训练、异步低延迟推理）及STREAMAUDIO-2M数据集（260万条流式交互样本），并提出了PROACTIVE-SOUND-BENCH基准用于评估模型的主动响应能力。

# Research Agents & Scientific Discovery

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Research Agents & Scientific Discovery
- 方法：agent, generation, reasoning, optimization, machine-learning, deep-learning
- 论文/报告：6 篇
- Self-Evolving Deep Research via Joint Generation and Evaluation【Research】
- DuMate-DeepResearch: An Auditable Multi-Agent System with Recursive Search and Rubric-Grounded Reasoning【Research, Baidu】
- ResearchClawBench: A Benchmark for End-to-End Autonomous Scientific Research【Research, AI Lab】
- Agents-K1: Towards Agent-native Knowledge Orchestration【Research, AI Lab】
- EurekAgent: Agent Environment Engineering is All You Need For Autonomous Scientific Discovery【Research, Zhipu】
- MLEvolve: A Self-Evolving Framework for Automated Machine Learning Algorithm Discovery【Research, AI Lab】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:5c04358ed5ce30ab -->
## Self-Evolving Deep Research via Joint Generation and Evaluation【Research】

[https://arxiv.org/pdf/2606.04507](https://arxiv.org/pdf/2606.04507)

- 这篇论文针对**深度研究报告生成中缺乏确定性ground-truth导致的强化学习训练难题**，提出了自演化的评估器-生成器协同训练框架SCORE。评估器（evaluator）与生成器（solver）作为同一模型πθ的两种功能角色，共享底层参数。该设计基于二者在查询理解、证据建模、话语推理上的能力重叠。训练采用顺序交替更新： 生成器：使用GRPO（Group Relative Policy Optimization），基于评估器奖励 评估器：使用REINFORCE，基于一致性信号 两者均施加KL散度约束：Lreg=L+λKL(πθ∥πref) ，防止共享参数下的策略漂移。

<!-- paperflow:388695373f80c1ce -->
## DuMate-DeepResearch: An Auditable Multi-Agent System with Recursive Search and Rubric-Grounded Reasoning【Research, Baidu】

[https://arxiv.org/pdf/2606.07299](https://arxiv.org/pdf/2606.07299)

- 本文介绍了 **DuMate-DeepResearch**，一个基于 Qianfan Agent Foundry 构建的端到端多智能体深度研究框架，旨在解决复杂开放式研究任务中的长视界规划、任务调度、事实保真和过程审计等核心挑战。深度研究（Deep Research, DR）要求系统能够迭代地构建问题、获取证据、验证来源并综合生成长篇报告。现有系统面临四个相互关联的局限： 长视界规划与动态范围定义：研究问题涉及数十个相互依赖的子问题，其范围在初始阶段不明确，而逐步决策（ReAct 式）缺乏全局视野； 复杂任务分解与调度瓶颈：单智能体难以协调高层策略与低层噪声检索，局部失败易级联至全局轨迹； 长文本合成中的幻觉风险：动态多源证据流上的事实保真度难以维持，且缺乏原则性的停止准则； 过程可解释性与可审计性：自主推理过程缺乏透明记录，难以满足高风险领域的可信度要求。

<!-- paperflow:e9128accc5113de8 -->
## ResearchClawBench: A Benchmark for End-to-End Autonomous Scientific Research【Research, AI Lab】

[https://arxiv.org/pdf/2606.07591](https://arxiv.org/pdf/2606.07591)

- **如何客观评估AI系统进行端到端自主科学研究能力**的问题。论文提出了ResearchClawBench，一个包含40个任务、覆盖10个科学领域（天文学、化学、地球科学、能源、信息科学、生命科学、材料科学、数学、神经科学、物理学）的基准测试，其核心设计包括：真实论文基础：每个任务基于真实发表的高质量论文，提供相关文献和原始实验数据，但在评估时隐藏目标论文；重新发现到发现的评估框架：通过专家构建的加权多模态评分标准（rubrics），以50分为阈值衡量系统是否达到"目标论文水平的重新发现"（re-discovery），同时允许得分超过50分以表示"发现"（discovery）新结论；端到端评估：要求系统从任务描述、文献和原始数据出发，自主设计实验、执行分析、生成图表并撰写研究报告。关键发现 通过该基准测试，论文量化了当前AI系统与可靠自主科学研究之间的巨大差距： 最强自主代理（Claude Code）平均分仅21.5（满分100，50分为重新发现阈值） 最强LLM（Claude-Opus-4.7）平均分仅20.7 错误分析显示，失败主要集中在**实验协议不匹配、证据链缺失和科学核心机制遗漏**，而非简单的执行失败 该基准测试填补了在无目标论文先验知识的情况下，评估AI从原始数据开展独立科学研究能力的关键空白。

<!-- paperflow:3bfd27b3e13b9830 -->
## Agents-K1: Towards Agent-native Knowledge Orchestration【Research, AI Lab】

[https://arxiv.org/pdf/2606.13669](https://arxiv.org/pdf/2606.13669)

- **能否构建一个统一的端到端管道，将原始科学论文转换为智能体原生的（agent-native）多模态知识图谱，并支持大规模、可靠的科学研究智能体推理？**论文提出了Agents-K1框架，其设计需满足三项关键要求： 全文覆盖与多模态融合：解析全文而非仅摘要，将文本、图表、表格、公式作为相互关联的证据（通过"语义锚点"（semantic anchors）连接），而非独立人工制品； 类型化知识结构：将科学内容转化为结构化知识，包括实体（方法、数据集、指标）、隐式抽象（动机、贡献、局限性）、细粒度引用意图（支持/对比/扩展）及实体间关系； 可审计检索：通过稳定的图谱标识符（stable graph identifiers）实现证据溯源，使智能体能够将每个答案追溯到确切的证据节点与原始文档位置。 通过整合多模态解析器、基于GRPO（Group Relative Policy Optimization）训练的4B参数信息抽取骨干网络，以及融合网络搜索、多模态图谱检索与跨文档遍历的"三元源"（tri-source）智能体接口，Agents-K1旨在**将分散的学术文献转化为可重用、可导航、可验证的结构化知识基础设施**。

<!-- paperflow:a9d1016a4c912027 -->
## EurekAgent: Agent Environment Engineering is All You Need For Autonomous Scientific Discovery【Research, Zhipu】

[https://arxiv.org/pdf/2606.13662](https://arxiv.org/pdf/2606.13662)

- 论文提出EUREKAGENT，一个通过四个维度进行环境工程的代理系统： 权限工程（Permissions Engineering）：通过Docker沙箱、隐藏评估器接口、同轮次隔离等机制，实现有界执行与评估隔离 工件工程（Artifact Engineering）：利用文件系统与Git历史作为共享长期记忆，支持跨会话的解决方案追溯与协作 预算工程（Budget Engineering）：**将时间与API成本预算内嵌为环境约束**，支持预算感知的探索与可恢复的长时运行 人在回路工程（Human-in-the-loop Engineering）：**提供终端UI与Web监控界面，在保持代理自主性的同时实现有效监督与干预** 该框架使代理能够在不规定具体研究策略的前提下，自主进行假设提出、实验迭代与方案优化，同时确保研究过程的完整性、可追溯性与资源可控性。

<!-- paperflow:2c113caada58ef22 -->
## MLEvolve: A Self-Evolving Framework for Automated Machine Learning Algorithm Discovery【Research, AI Lab】

[https://arxiv.org/pdf/2606.06473](https://arxiv.org/pdf/2606.06473)

- 论文提出**MLEvolve**框架，通过**渐进式蒙特卡洛图搜索（Progressive MCGS）实现跨分支信息共享与自适应探索-利用平衡，通过回顾式记忆（Retrospective Memory）实现冷启动领域知识与动态经验积累的融合，并通过自适应代码生成**实现规划与实现的层次化解耦，从而支持端到端机器学习算法发现的长周期迭代优化。

# Web, GUI & Computer-Use Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Web, GUI & Computer-Use Agents
- 方法：agent, reinforcement-learning, stat-ml, machine-learning, deep-learning, stat-me, gui-agent
- 论文/报告：8 篇
- DailyReport: An Open-ended Benchmark for Evaluating Search Agents on Daily Search Tasks【Search, Meituan】
- OpenWebRL: Demystifying Online Multi-turn Reinforcement Learning for Visual Web Agents【Web, Microsoft】
- DeskCraft: Benchmarking Desktop Agents on Professional Workflows and Human-in-the-Loop Collaboration【GUI, Tencent】
- WebRISE: Requirement-Induced State Evaluation for MLLM-Generated Web Artifacts【Huawei】
- Mind the Gap: Can Frontier LLMs Pass a Standardized Office Proficiency Exam?【Coding, Microsoft】
- iOSWorld: A Benchmark for Personally Intelligent Phone Agents【GUI, Personalize】
- Workflow-GYM: Towards Long-Horizon Evaluation of Computer-use Agentic tasks in Real-World Professional Fields【GUI, Long Horizon, ByteDance Seed】
- How Much Can We Trust LLM Search Agents? Measuring Endorsement Vulnerability to Web Content Manipulation
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:94b1d69af297aecd -->
## DailyReport: An Open-ended Benchmark for Evaluating Search Agents on Daily Search Tasks【Search, Meituan】

[https://arxiv.org/pdf/2606.12871](https://arxiv.org/pdf/2606.12871)

- 论文提出DailyReport基准，其核心创新包括： 日常搜索任务：**基于社交媒体（微博、Facebook等）热门话题和用户评论构建150个开放式任务，覆盖10个宏观领域和35个细分类别，确保任务反映真实、及时且实用的信息需求** 级联评估流程：将任务分解为子任务，沿三个解耦维度（指令遵循、事实性、合理性）设计级联评分标准，通过级联性能归因（cascade performance attribution）实现可解释的维度评分 用户偏好量化：引入基于子任务重要性的用户中心聚合算法，显式计算用户偏好分数（User Preference Score），以反映真实用户对响应满意度的感知 通过评估17个前沿代理系统，论文揭示了当前系统在事实准确性和用户满意度方面仍存在显著差距，为搜索代理的改进提供了诊断依据。对17个前沿系统（包括原生DRAs、搜索增强LLMs、Claude Code集成系统）的评估揭示： 性能层级：LLMs with Search Tools（UserPref: 2.60-2.89）> LLMs with Claude Code（2.51-2.87）> 原生DRAs（2.17-2.48），GPT 5.4表现最优 能力瓶颈：所有系统在事实性（0.61-0.84）维度表现薄弱，且用户偏好分数普遍低于可接受阈值（<3分），揭示当前系统与真实用户期望存在显著差距 领域差异：在政治法律等结构化信息领域表现较好，在体育娱乐等动态主观领域表现较差 引用质量问题：高引用率（Reference_Ratio>0.7）但低引用-声明一致性（Refer-Claim Consistency: 0.74-0.91），表明引用验证机制仍需改进

<!-- paperflow:ab13cddf8498d83b -->
## OpenWebRL: Demystifying Online Multi-turn Reinforcement Learning for Visual Web Agents【Web, Microsoft】

[https://arxiv.org/pdf/2606.02031](https://arxiv.org/pdf/2606.02031)

- 解决**视觉Web代理（visual web agents）训练中的可扩展性瓶颈与性能差距**问题。论文提出了OpenWebRL——一个完全开源的端到端框架，用于**在真实网站上通过在线多轮强化学习训练视觉Web代理**。该框架通过以下机制解决核心问题： **可扩展的实时浏览器基础设施：支持大规模并行轨迹收集，具备容错、超时处理和失败归因机制** 轻量监督初始化：仅使用0.4K轨迹进行SFT热身，使策略进入有效探索区域，避免对大规模静态数据集的依赖 多模态上下文管理：通过分离视觉感知与文本历史记忆（保留推理轨迹和环境反馈，仅保留最近$K$张截图），解决长程交互的上下文瓶颈 轨迹级成功判断：结合GPT-4.1或蒸馏的8B Judge模型，为开放域任务提供可靠的延迟奖励（delayed reward） 高效的多轮GRPO优化：通过群组相对优势估计（group-relative advantage）和动态采样，实现稳定的策略更新 通过该系统，论文训练出的OpenWebRL-4B（仅4B参数）在Online-Mind2Web、DeepShop和WebVoyager等实时基准测试上建立了新的开源最先进水平（SOTA），并与GPT-5、OpenAI CUA等专有系统形成有力竞争，证明了在线RL在构建可复现、成本效益高的开放Web代理方面的可行性。

<!-- paperflow:99b8956667e51748 -->
## DeskCraft: Benchmarking Desktop Agents on Professional Workflows and Human-in-the-Loop Collaboration【GUI, Tencent】

[https://arxiv.org/pdf/2606.03103](https://arxiv.org/pdf/2606.03103)

- **现有桌面GUI基准测试（benchmarks）无法充分评估长时程专业工作流和人机协作能力**的问题。论文提出了DeskCraft基准测试，其核心设计包括： L1/L2/L3三级难度体系：特别是L3级任务，直接从真实交付流程中提炼，保留跨应用依赖和命名工件（named artifacts）结构； 可执行的人机交互协议：定义了三种可组合触发器（agent_done、agent_ask、step_count），覆盖执行中（mid-turn）的代理澄清与用户打断，以及执行后（post-turn）的用户反馈； 专业工作流覆盖：涵盖设计、视频、音频、3D渲染等5大领域的11款专业软件。 实验结果表明，当前最先进的模型（如GPT-5.4）在标准任务上仅达到31.6%的成功率，在交互任务上为27.6%，且在L3长时程工作流和主动澄清方面存在显著瓶颈，验证了该基准测试的必要性和挑战性。

<!-- paperflow:771d5c6deebcde81 -->
## WebRISE: Requirement-Induced State Evaluation for MLLM-Generated Web Artifacts【Huawei】

[https://arxiv.org/pdf/2606.03220](https://arxiv.org/pdf/2606.03220)

- 该论文提出WebRISE框架，其核心创新包括： 需求条件化的状态建模：将每个任务表示为**交互合约图（Interaction Contract Graph, ICG） Gτ=(Sτ,Tτ,Φτ,Mτ) ，其中 Sτ 为需求相关的可观察UI状态，Tτ 为用户意图驱动的转换，Φτ 为可观察的DOM/视觉断言，Mτ 将需求映射至测试项与断言**。 显式与隐式需求的分离：显式需求（Rexpτ ）描述用户陈述的功能（如搜索、筛选、排序），而隐式需求（Rimpτ ）捕获产品级交互约束（如状态同步、边界反馈、分页重置、加载反馈、过期状态移除）。 基于一致性的诊断评估：通过合约引导的浏览器代理执行转换，并利用DOM/视觉双重验证机制，将转换结果关联回原始需求，从而实现对状态可达性（S% ）、转换有效性（T% ）及需求覆盖率（Re% , Ri% ）的细粒度诊断。 实验表明，即使是最强的模型（GPT-5.5）在最佳模态下仅达到 T=65.6% 和 R=66.3% 的指标，且视觉质量与行为正确性无显著相关性（如Qwen3.6-35B-A3B在Markdown模态下 V=80.8 但 T=15.5 ），凸显了从"外观正确"向"行为正确"转变评估范式的必要性。

<!-- paperflow:fd0d8a01d4158b9a -->
## Mind the Gap: Can Frontier LLMs Pass a Standardized Office Proficiency Exam?【Coding, Microsoft】

[https://arxiv.org/pdf/2606.10956](https://arxiv.org/pdf/2606.10956)

- 论文构建了基于**中国国家计算机等级考试（NCRE）**的标准化评估体系： **任务来源：200个真实认证考试实操任务（Level 1基础+Level 2高级），涵盖Word、Excel、PowerPoint 评估机制：7,118个机器可评分标准（machine-gradable criteria）**，基于百分制量规（100-point rubric）提供确定性、细粒度评分（部分得分制） 技能分类：8大类别（文本格式、布局设计、表格、图表、数据公式、图形媒体、动画、文档属性） 参考锚点：60分作为人类及格参考阈值，95.5%作为社区参考高分（sanity check）。主导错误类型：**97.4%的非崩溃损失源于实现知识错误（错误的OOXML属性路径、枚举常量、颜色/主题编码、数值单位等），而非语义误解**级联与回归：长程任务中存在上游错误导致下游失败的级联效应，以及智能体迭代修复时破坏已有正确内容的回归现象。

<!-- paperflow:ddbb71e4af65c69c -->
## iOSWorld: A Benchmark for Personally Intelligent Phone Agents【GUI, Personalize】

[https://arxiv.org/pdf/2606.09764](https://arxiv.org/pdf/2606.09764)

- 解决**移动设备智能代理（mobile phone agents）基准测试中的个性化（personalization）缺失与平台覆盖不足**问题。论文提出了 iOSWorld——**首个围绕持久用户身份（Jordan Avery）构建的交互式原生 iOS 模拟器基准**。该基准包含： **26 个专门构建的 iOS 应用，涵盖金融、消息、旅行、购物等 10 个生活领域 跨应用连接的合成个人数据（交易记录、社交关系、行程预订等） 133 个任务，分为单应用（27）、多应用（60）和记忆/个性化（46）三类难度递增的类别** 通过这一设计，iOSWorld 首次系统性地评估了代理在具有真实用户数据痕迹的 iOS 环境中执行跨应用推理和个性化任务的能力。模态影响 强模型显著受益于 XML：Opus 提升 25.6 个百分点（26.3% → 51.9%），Sonnet 提升 18.0pp，源于消除了坐标估算误差与主页导航失败 小模型无法利用 XML：GPT-5.4 Mini 与 Qwen3.5 因 XML 引入的约 3,100 tokens/步超出上下文容量，性能下降或持平

<!-- paperflow:295d757003850f96 -->
## Workflow-GYM: Towards Long-Horizon Evaluation of Computer-use Agentic tasks in Real-World Professional Fields【GUI, Long Horizon, ByteDance Seed】

[https://arxiv.org/pdf/2606.11042](https://arxiv.org/pdf/2606.11042)

- 现有GUI智能体基准测试主要聚焦于**短程交互**（<30步）与**通用软件**（如网页浏览、Office办公），缺乏对**长期（30–110步）、领域特异性、高经济价值**的真实专业工作流的评估。这导致无法判断智能体是否具备替代人类完成复杂专业任务（如CAD建模、GIS分析、科学计算）的能力。论文构建了Workflow-GYM，这是首个**针对长期专业GUI工作流的系统性评估基准**：任务规模：共338个任务，覆盖6大领域（数据分析、工程设计、金融管理、地理环境、多媒体创意、科学计算）与23个细分领域软件环境：构建58个独立虚拟机镜像，预装专业软件（如Blender、QGIS、KNIME、FreeCAD、Godot等），涵盖从生物信息学到电路设计的专业工具链任务特性：长期性：**每个任务需30–110个原子动作完成**零提示策略：仅提供自然语言指令，不提供中间步骤或最终答案提示可验证性：定义明确的二元成功标准，基于最终产物（文件/数据）或目标GUI状态进行自动评估构建流程：采用专家驱动的四阶段流水线（工作流选择→环境设置→任务构造→多层级验证），确保任务真实性、可解性与评估可靠性

<!-- paperflow:23c44f19171cb450 -->
## How Much Can We Trust LLM Search Agents? Measuring Endorsement Vulnerability to Web Content Manipulation

[[Deep Reading - Jun 2026/How Much Can We Trust LLM Search Agents-Measuring Endorsement Vulnerability to Web Content Manip|Deep Reading]]

[https://arxiv.org/pdf/2606.16821v1](https://arxiv.org/pdf/2606.16821v1)

- **论文从LLM搜索代理成为用户与网页主要接口这一背景出发，定义endorsement失效这一新攻击面，提出SearchGEO受控框架，设计五模式攻击与多指标评估体系，对13后端进行系统测试，揭示ASR范围、模式差异、静默偏移与技能探针下的信任分裂，最终论证将推荐可靠性作为后端安全评估核心维度。**

# Agent Skills, Harness & Tooling

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Skills, Harness & Tooling
- 方法：agent, generation, language, vision-language-model, vision, reinforcement-learning, optimization, retrieval
- 论文/报告：25 篇
- 🤔EvoTrainer: Co-Evolving LLM Policies and Training Harnesses for Autonomous Agentic Reinforcement Learning【Research, Tongyi】
- AutoLab: Can Frontier Models Solve Long-Horizon Auto Research and Engineering Tasks?【Research】
- SearchSwarm: Towards Delegation Intelligence in Agentic LLMs for Long-Horizon Deep Research【Research, Ant】
- ⭐COLLEAGUE.SKILL: Automated AI Skill Generation via Expert Knowledge Distillation【Skill, AI Lab】
- SkillPager: Query-Adaptive Intra-Skill Navigation via Semantic Node Retrieval【Skill, SII】
- SkillRevise: Improving LLM-Authored Agent Skills via Trace-Conditioned Skill Revision【Skill】
- Agent Skills Should Go Beyond Text: The Case for Visual Skills【Skill】
- SIRI: Self-Internalizing Reinforcement Learning with Intrinsic Skills for LLM Agent Training【Skill】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:1a46a045059e1ca1 -->
## 🤔EvoTrainer: Co-Evolving LLM Policies and Training Harnesses for Autonomous Agentic Reinforcement Learning【Research, Tongyi】

[https://arxiv.org/pdf/2606.03108](https://arxiv.org/pdf/2606.03108)

- 现有自主训练框架通常将模型改进视为**静态的"配方搜索"（recipe search）——即在固定的训练基础设施（training harness）下搜索超参数或数据配置，而解释训练结果所需的诊断工具、评估指标和干预逻辑却保持静止不变**。然而，在智能体强化学习中，这种静态性会导致以下关键局限： 动态瓶颈转移：训练过程中的主导瓶颈会随时间从奖励稀疏性转移到行为崩溃、评估伪影或低信息量的轨迹组，固定诊断模板无法捕捉这些变化； 标量奖励的掩盖效应：单一的验证分数无法区分健康的行为改进与奖励窃取（reward leakage），也无法识别有价值的负样本证据； 诊断基础设施的滞后性：早期版本可通过粗略的验证趋势分析，而后期版本需要新的行为指标、奖励审计、组方差统计等，这些分析需求难以预先完全指定。 为此，论文提出应将自主训练重新概念化为跨版本的训练系统（trainer）自我改进过程——即不仅优化模型策略（policy），还要让解释训练结果、指导干预决策的**训练端诊断工具（training-side diagnostic harness）**本身能够根据经验反馈不断演化。

<!-- paperflow:13b8c92246bf9999 -->
## AutoLab: Can Frontier Models Solve Long-Horizon Auto Research and Engineering Tasks?【Research】

[https://arxiv.org/pdf/2606.05080](https://arxiv.org/pdf/2606.05080)

- 论文提出了AUTOLAB——一个**专门针对超长程闭环优化（ultra long-horizon closed-loop optimization）**的基准测试，具备以下特征： **36个跨领域任务：涵盖系统优化、算法谜题、模型开发、CUDA内核优化四大类别** 严格的时间预算：任务时限从2小时到12小时不等，强制要求模型在 wall-clock 时间内持续迭代 连续校准评分：采用对数拉伸（log-stretch）和线性插值相结合的评分方案，奖励部分进步而非仅看通过/失败 防作弊验证：通过密封验证器、正确性门控、文件完整性检查等机制防止奖励黑客（reward hacking） 该基准测试旨在揭示：**在长程优化任务中，成功的关键预测因素并非初始尝试的质量，而是模型持续评估、编辑并整合实证反馈的持久性（persistence）**。研究对17个前沿模型（含4个闭源旗舰模型）进行了2,544小时和86亿token的系统性评估，关键发现如下：性能排名：claude-opus-4.6以Avg@3=0.68显著领先，Dominance达0.93；开源模型中kimi-k2.6（0.46）、mimo-v2.5-pro（0.45）表现突出，而gpt-5.4（0.36）和grok-4-20（0.35）因过早终止排名靠后。持久性优于初始能力：轨迹分析表明，最终性能与迭代次数和持续优化时间的相关性，强于与初始代码质量的相关性。成功代理展现出"持续基准测试-编辑-整合反馈"的行为模式。时间意识缺失：模型表现出两种极端失败模式——过早终止（如grok-4-20仅运行一次评估即提交）与预算耗尽（如deepseek-v4-pro因过度思考导致超时未提交）。Harness设计影响：消融实验显示，不同agent框架（terminus-2、pi-mono、mini-swe-agent）可导致同一模型得分差异达0.43，且会改变模型间的相对排名，表明harness工程是性能差距的重要来源。

<!-- paperflow:a8958303953375f3 -->
## SearchSwarm: Towards Delegation Intelligence in Agentic LLMs for Long-Horizon Deep Research【Research, Ant】

[https://arxiv.org/pdf/2606.09730](https://arxiv.org/pdf/2606.09730)

- 解决**长程（long-horizon）智能体任务中上下文资源受限与任务复杂度持续增长之间的根本性矛盾**，并针对**委托智能（delegation intelligence）的训练数据稀缺问题**提出了系统性解决方案。论文提出 SearchSwarm 框架，核心贡献包括： Harness 设计：通过系统提示（system prompts）和工具设计，引导主代理进行主动式上下文管理——将多步骤信息收集委托给子代理，仅接收压缩后的报告，从而保留主代理的有限注意力用于高层协调 数据合成管道：利用 harness 生成包含正确委托决策（何时分解、如何界定子任务、如何撰写全面简报）的轨迹数据，通过监督微调（SFT）将委托智能内化到模型中 双向验证机制：要求子代理提供带有显式引用（inline citations）的报告，使主代理能够验证结论；同时要求主代理在最终回答中保持独立判断，避免盲目信任子代理输出 最终得到的模型 SearchSwarm-30B-A3B 在 BrowseComp、BrowseComp-ZH、GAIA 和 xbench-DeepSearch 等长程研究基准上，取得了同规模模型中的最佳性能，证明了委托智能训练的有效性。

<!-- paperflow:21584af0ff116307 -->
## ⭐COLLEAGUE.SKILL: Automated AI Skill Generation via Expert Knowledge Distillation【Skill, AI Lab】

[https://arxiv.org/pdf/2605.31264](https://arxiv.org/pdf/2605.31264)

- **如何将分散的、异质性的人类专家痕迹（traces）转化为结构化、可移植、可检查且可治理的AI技能包？**COLLEAGUE.SKILL通过以下方式解决上述问题： 提出**人物锚定技能（person-grounded skill）**的形式化定义，将其视为具有显式可移植性、可检查性、可组合性、可纠正性和可治理性要求的制品（artifact）问题 构建自动化蒸馏管道，生成包含双轨道（能力+行为）、元数据、生命周期状态（版本、更正历史）的版本化技能包 实现完整的生命周期工作流：收集→技能渲染→多主机安装→自然语言纠正→回滚→可选的受控分发 该论文的核心论点是：有用的目标不是无限制地模拟个人，而是将选定的人类痕迹蒸馏为有界的、可编辑的技术制品，使专业知识、交互风格和使用限制显式化，从而支持审计、修复和治理。系统采用分离表示解决传统角色扮演中知识、程序与风格的混淆： **能力轨道（Capability Track）：编码工作流程、技术标准、决策启发式与心智模型（输出为 [work.md](http://work.md/)）** **有界行为轨道（Bounded Behavior Track）：编码沟通风格、交互规则、边界限制与纠正历史（输出为 [persona.md](http://persona.md/)）** 该分离支持三种运行时入口：完整技能（能力与行为结合）、仅能力技能（剥离风格的专业判断）、仅行为技能（风格参考），从而避免无限制的“身份替代”。

<!-- paperflow:8c914b19e282ad6e -->
## SkillPager: Query-Adaptive Intra-Skill Navigation via Semantic Node Retrieval【Skill, SII】

[https://arxiv.org/pdf/2606.00822](https://arxiv.org/pdf/2606.00822)

- 研究的是**技能内检索（intra-skill retrieval）**问题，即**针对基于技能的LLM智能体（skill-based LLM agents）如何高效利用长程序文档**的挑战。**给定一个已知的技能文档（通常以长Markdown格式存储）和一个具体的执行查询，如何从中选择一个最小化但足以完成任务的上下文，而非注入完整文档。**论文提出**SkillPager**框架，通过离线类型化语义解析（将文档分解为step、example、param等六类节点）与在线全局MMR（Maximal Marginal Relevance）检索相结合，实现查询自适应的节点选择，在保持上下文充分性（context sufficiency）的同时显著降低token消耗。

<!-- paperflow:30fb545d10c3e6b4 -->
## SkillRevise: Improving LLM-Authored Agent Skills via Trace-Conditioned Skill Revision【Skill】

[https://arxiv.org/pdf/2606.01139](https://arxiv.org/pdf/2606.01139)

- **冷启动场景下LLM生成技能（agent skills）的行为可靠性不足**的问题。构建一个**基于执行反馈的迭代修订框架，使初始的、不完美的LLM生成技能能够通过诊断执行证据、检索修复原则、实施锚定编辑，并在经验效用评估下系统性地收敛到最优版本**，从而在无大量历史轨迹的冷启动条件下提升技能的行为有效性与验证对齐度。

<!-- paperflow:668d9f634abce032 -->
## Agent Skills Should Go Beyond Text: The Case for Visual Skills【Skill】

[https://arxiv.org/pdf/2606.01414](https://arxiv.org/pdf/2606.01414)

- 纯文本范式对于**以视觉为中心的任务**（visual-centric tasks）存在根本性瓶颈。论文提出 VISUAL SKILL 范式，将技能定义为多模态实体： Sv=(L,Pv,B) 其中： L ：声明式文本逻辑（任务目标、执行步骤、约束条件） Pv ：可重用视觉支持（静态先验、动态先验或交错视觉引用） B ：多模态绑定协议（协调文本逻辑与视觉基础的联合执行） 通过**将视觉结构视为一等技能资产（first-class skill asset）**，而非仅依赖文本描述，该范式使智能体能够积累和利用包含空间约定、视觉边界和感知跟踪协议的可重用经验。

<!-- paperflow:9b1a612602939c9b -->
## SIRI: Self-Internalizing Reinforcement Learning with Intrinsic Skills for LLM Agent Training【Skill】

[https://arxiv.org/pdf/2606.02355](https://arxiv.org/pdf/2606.02355)

- 论文提出了 SIRI（Self-Internalizing Reinforcement learning with Intrinsic skills） 框架，试图回答一个关键问题：**智能体能否在强化学习训练期间自主发现有用的技能，并将这些技能吸收到自身参数中，从而在推理时完全不需要技能库或检索服务**？ 为实现这一目标，SIRI 解决了两个耦合的子问题： 自主技能发现：**避免从未经训练的策略中提取虚假或嘈杂的启发式规则，确保仅从高质量的成功轨迹中挖掘技能**； 选择性技能内部化：通过轨迹级效用评估和动作级优势加权，**仅将有益的技能引导行为蒸馏到无技能提示（skill-free）的策略中**，避免模仿无关的语言模式。 最终，SIRI 实现了训练时利用技能作为临时指导信号，推理时仅使用原始提示即可运行的检索自由（retrieval-free）智能体。

<!-- paperflow:4c15aa76255cd0d4 -->
## MMG2Skill: Can Agents Distill In-the-Wild Guides into Self-Evolving Skills?【Skill, Kuaishou】

[https://arxiv.org/pdf/2606.01993](https://arxiv.org/pdf/2606.01993)

- 解决**将野外（in-the-wild）多模态人类指南转换为智能体可执行技能（guide-to-skill learning）**的问题。提出MMG2Skill（MultiModal Guide to Skill）闭环框架，包含三个阶段： 技能构建（Skill Construction）：**使用VLM将原始指南（HTML+图像）编译为结构化的 [SKILL.md](http://skill.md/) 表示，包含可复用程序 ui 、适用条件 ci 、预期状态线索 vi 和恢复知识 qi** ，过滤噪声并保留程序性内容。 技能条件执行（Skill-Conditioned Execution）：固定VLM策略 πθ 在每一步基于当前观测 ht 、任务指令 I 和当前技能集 Sk 执行动作： at∼πθ(at|ht,I,Sk) 轨迹诊断与修订（Trajectory-Driven Revision）：通过**分析器（Analyzer）**将轨迹 τk 转换为根因诊断 ρk=(ek,rk) ，其中 ek 为轨迹证据，rk∈{likely\_success,uncertain,likely\_failure} 为自评估结果；**精炼器（Refiner）**基于诊断历史和原始指南修订技能： Sk+1=REFINE(G,I,Sk,ρ1:k) 当 rk=likely\_success 时触发早期停止（Early Stopping），无需等待完整预算。

<!-- paperflow:07cb17138c4a93aa -->
## ReSkill: Reconciling Skill Creation with Policy Optimization in Agentic RL【Skill, Amazon】

[https://arxiv.org/pdf/2606.01619](https://arxiv.org/pdf/2606.01619)

- 解决**智能体强化学习（Agentic RL）中技能创建与策略优化之间的解耦问题**。论文提出了RESKILL框架，通过以下机制实现技能与策略的协调共进化： **利用GRPO的组内采样结构（within-group sampling）在同一任务、同一策略下对比不同技能版本的效果**； 引入基于自适应折扣的Thompson Sampling来平衡探索与利用，在策略进化过程中动态决定技能版本的取舍； 通过断言驱动的诊断（assertion-driven diagnosis）从训练经验中自动提出技能修订方案。 简言之，论文的核心贡献是**将技能创建过程嵌入RL训练循环，使技能进化与策略学习成为一个协调的统一过程**，从而解决了静态技能与动态策略之间的冲突问题，并显著提升了在未见任务上的泛化能力。

<!-- paperflow:349eabb37710604b -->
## SkillAdaptor: Self-Adapting Skills for LLM Agents from Trajectories【Skill, Ant】

[https://arxiv.org/pdf/2606.01311](https://arxiv.org/pdf/2606.01311)

- 解决**大语言模型（LLM）智能体在长程交互任务中，免训练技能自适应（training-free skill adaptation）所面临的失败归因粗糙与信用分配（credit assignment）问题**。论文提出 SKILLADAPTOR 框架，其**核心创新在于将技能自适应从轨迹级反思（trajectory-level reflection）转向步级归因（step-level attribution）**。具体机制包括： 精确定位故障步骤：通过 Localizer 组件识别失败轨迹中第一个可操作的故障步骤（first actionable fault step）； 显式责任链接：通过 Linker 组件将故障步骤的责任归因到具体的候选技能（candidate skills），并区分是技能错误（skill_wrong）还是技能缺失（skill_missing）； 定向更新与资格检查：仅对负有责任的技能进行最小化、有针对性的修订（minimal, targeted edits），并通过 Qualifier 组件验证更新效果，抑制有害修改。 通过步级失败归因，**SKILLADAPTOR 旨在实现更稳定、可审计且计算轻量的免训练技能维护**，避免在冻结主干模型（frozen backbone）的情况下，因错误归因导致的技能库漂移（skill library drift）。

<!-- paperflow:594d36f6fea3808c -->
## Skill or Skip? Learning Selective Skill Invocation in Agentic Tasks via Dual-Granularity Preference Learning【Skill】

[https://arxiv.org/pdf/2606.00510](https://arxiv.org/pdf/2606.00510)

- **即使某个技能在语义上与当前任务相关，在当前决策点调用它仍可能是有害的或不必要的**。论文提出SelSkill框架，将选择性技能调用形式化为**"技能或跳过"（skill-or-skip）决策**，通过结合： **回合级偏好（episode-level preferences）：捕捉整体轨迹质量** **步骤级调用偏好（step-level invocation preferences）：评估特定决策点的局部调用效果** 使智能体能够学习在何时真正需要调用技能，何时应当依靠自身能力直接执行，从而在提升任务成功率的同时提高执行精确度。

<!-- paperflow:23df587e2be45bae -->
## Skill-RM: Unifying Heterogeneous Evaluation Criteria via Agent Skill【Skill, Qwen】

[https://arxiv.org/pdf/2606.03980](https://arxiv.org/pdf/2606.03980)

- 解决**奖励模型（Reward Models, RMs）在处理异构评估标准（heterogeneous evaluation criteria）时缺乏统一整合机制**的核心问题。论文提出 **Skill-RM（Skill Reward Model）**，**将奖励建模重新表述为**可复用的"奖励评估技能"（Reward-Evaluation Skill）**的执行过程**。通过将奖励计算形式化为结构化的智能体任务（structured agentic task），Skill-RM 提供了一个一致的接口来编排异构资源，实现基于证据的、可解释的、实例自适应的动态评估。输入任务与候选答案 → 调用对应评测 skill → 动态选择 rubric / reference / checklist / verifier / tool → 生成 evidence → 聚合成 reward

<!-- paperflow:9bc2cf166440c7a7 -->
## 🧐FederatedSkill: Federated Learning for Agentic Skill Evolution【Skill】

[https://arxiv.org/pdf/2606.03143](https://arxiv.org/pdf/2606.03143)

- **用户能否在严格保护隐私的前提下协作进化技能，同时获得针对其特定模型、框架和任务分布量身定制的个性化技能库？论**文提出FederatedSkill框架，通过以下机制解决上述问题： 语义技能补丁（Semantic Skill Patches）：客户端在本地将执行轨迹蒸馏为结构化的技能差异（包含ADD、EDIT、DELETE操作的高层级编辑），仅上传这些补丁而非原始轨迹，实现"构造性隐私保护"（privacy by construction）。 个性化联邦进化（Personalized Federated Evolution）：服务器端的进化代理（Evolution Agent）通过分析客户端上传的补丁序列，动态推断其能力边界（capability boundaries），为每个客户端生成个性化的技能库更新，而非统一的全局库。这通过在语义技能层面（而非传统联邦学习的参数层面）实现个性化，有效应对模型和框架的异构性。 简言之，该工作首次在智能体技能进化领域同时解决了隐私保护与异构客户端个性化的双重挑战。

<!-- paperflow:e5066500e659600d -->
## Learning While Acting: A Skill-Enhanced Test-Time Co-Evolution Framework for Online Lifelong Learning Agents【Skill, AI Lab】

[https://arxiv.org/pdf/2606.04815](https://arxiv.org/pdf/2606.04815)

- 论文提出了 **LifeSkill** 框架，通过**验证器引导的技能学习（Verifier-Guided Skill Learning）**和**在线技能内化（Online Skill Internalization）**两个阶段，实现智能体在测试时的协同进化：前者利用基于执行的验证器反馈来训练技能提取模型，**确保提取的技能真正能提升任务成功率**；后者**将成功的技能引导轨迹转化为无脚手架的强化学习信号，使有用行为被直接吸收到策略模型的参数中**，从而避免长期依赖外部记忆。

<!-- paperflow:634f3d4e9f2a7825 -->
## LatentSkill: From In-Context Textual Skills to In-Weight Latent Skills for LLM Agents【Skill】

[https://arxiv.org/pdf/2606.06087](https://arxiv.org/pdf/2606.06087)

- 论文提出LatentSkill框架，**通过预训练的超网络（hypernetwork）将文本技能转换为LoRA适配器（LoRA adapters），将技能知识从上下文空间转移到权重空间**： 零上下文开销：推理时无需在提示中插入技能文本，直接挂载生成的LoRA适配器 即插即用模块化：支持适配器的动态加载、卸载、替换和缩放，无需重新训练主干模型 参数空间组合：通过参数算术（如加权和）实现技能组合，避免文本拼接导致的语义干扰 实验表明，该方法在ALFWorld和Search-QA基准上不仅减少了**64.1%-72.2%**的预填充token开销，还显著提升了任务成功率，同时生成的技能权重表现出结构化的语义几何、可控的注入强度和可组合性。

<!-- paperflow:545e9f0537c1fb2c -->
## OpenSkill: Open-World Self-Evolution for LLM Agents【Skill】

[https://arxiv.org/pdf/2606.06741](https://arxiv.org/pdf/2606.06741)

- **LLM代理能否在开放世界中实现自我进化？**为解决这一问题，论文提出了OpenSkill框架，通过三阶段流程实现无监督的自我进化：开放世界知识获取：从外部文档、仓库和网页检索基础知识和验证锚点；无泄漏技能进化：基于自我构建的虚拟任务（而非目标答案）迭代优化技能；零样本目标评估：将优化后的技能部署到目标代理进行最终评估该方法确保目标任务监督（TGTi）仅在最终评估阶段被使用，而在技能构建过程中完全隔离，从而实现了真正意义上的开放世界自我进化。

<!-- paperflow:4dfa3bef5ac5eb79 -->
## 🧐Harness Updating Is Not Harness Benefit: Disentangling Evolution Capabilities in Self-Evolving LLM Agents【Harness】

[https://arxiv.org/pdf/2605.30621](https://arxiv.org/pdf/2605.30621)

- 研究了 **LLM Agent 在 harness self-evolution 场景中的能力解耦问题**，系统分析了模型生成 harness 更新的能力与利用这些更新的能力之间的关系。Harness-updating (Δupdate )：**模型作为 evolver 时，从执行证据中生成有用持久化 harness 更新（如 skills、prompts）的能力** Harness-benefit (Δbenefit )：**模型作为 task-solving agent 时，在任务解决过程中实际受益于这些更新的能力**。**不同能力层级的 evolver 产生的性能增益差异极小**（任意基准上最大差距仅 3.1 个百分点）。即使是 Qwen3.5-9B（9B 参数）产生的更新，其下游增益也可与 Claude Opus 4.6 相当。后演化性能主要由 agent 的基础能力决定，而非 evolver 身份。**Harness-benefit 是 non-monotonic 的：中等能力模型（如 Qwen3-235B、GPT-OSS-120B）从 harness 更新中受益最大**；弱模型（如 Qwen3-32B）因两种失败模式受益有限： Activation failure：未能激活相关 harness 组件（如 skill 加载率仅 25.1% vs 强模型的 96%） Adherence failure：加载后无法在长程轨迹中持续遵循指导（遵從性衰减幅度是强模型的 4 倍以上）

<!-- paperflow:15f88f2dc9e69735 -->
## Adaptive Auto-Harness: Sustained Self-Improvement for Agentic System Deployment on Open-Ended Task Streams【Harness】

[https://arxiv.org/pdf/2606.01770](https://arxiv.org/pdf/2606.01770)

- 解决**LLM智能体（LLM agents）在开放式任务流（open-ended task streams）部署中的持续自我改进问题**。论文提出Adaptive Auto-Harness框架，通过以下机制解决上述挑战： 持续工具链构建（Sustained Harness Construction）：**采用有状态的多智能体进化器（四阶段：分析→研究→构建→验证），配合跨周期记忆和时序反馈揭示机制**，突破单智能体的上下文瓶颈； 求解时自适应（Solve-time Adaptation）：构建工具树（harness tree）隔离特定领域的工具链分支，通过智能体路由将任务动态匹配到最适合的分支； 人机协作通道（Human-in-the-Loop）：当历史经验缺乏必要信号（如API凭证、新数据源）时，通过结构化触发机制引入人类指导。 简言之，该论文解决了传统自动工具链在开放式、异构、时变任务流中性能退化的问题，实现了工具链的持续自我改进和任务级自适应。

<!-- paperflow:6cea50acbb3fb313 -->
## MCP-Persona: Benchmarking LLM Agents on Real-World Personal Applications via Environment Simulation【Personal】

[https://arxiv.org/pdf/2606.02470](https://arxiv.org/pdf/2606.02470)

- 解决**现有工具使用基准测试（benchmarks）无法有效评估大语言模型（LLM）代理在真实个性化应用场景中性能**的问题。论文提出了**MCP-Persona**——首个**专门针对真实世界个性化MCP工具的评估平台，通过环境模拟（Environment Simulation）范式**，在保护隐私的前提下复现了包括小红书（Rednote）、Instagram、Lark（飞书）、Slack等12个真实MCP服务器的功能，构建了包含173个人工验证任务的基准数据集。

<!-- paperflow:384fc4799b05de64 -->
## Socratic-SWE: Self-Evolving Coding Agents via Trace-Derived Agent Skills【Coding, Alibaba】

[https://arxiv.org/pdf/2606.07412](https://arxiv.org/pdf/2606.07412)

- 解决**LLM驱动的软件工程(SWE)智能体在训练过程中面临的高质量任务数据稀缺问题。**论文提出 Socratic-SWE，一个闭环自进化框架，通过以下机制解决上述问题： 轨迹再利用：将历史解决轨迹蒸馏为结构化的智能体技能注册表(Agent Skill Registry)，总结重复失败模式和有效修复策略。 技能引导的任务生成：利用这些技能作为约束，在真实代码仓库中构建针对能力差距的修复任务。 执行验证与梯度对齐：通过基于执行的验证管道筛选候选任务，并使用求解器-梯度对齐奖励(solver-gradient alignment reward)确保保留的任务既可验证又有助于改进求解器。 闭环进化：更新后的求解器产生新轨迹，使任务课程能够在连续轮次中适应，形成"轨迹-技能-任务"的自进化循环。 该框架旨在实现无需外部标注的自我进化，使SWE智能体能够利用自身历史行为作为可扩展的训练基质，持续突破能力边界。

<!-- paperflow:d0487f8591c9c189 -->
## Agents' Last Exam

[https://arxiv.org/pdf/2606.05405](https://arxiv.org/pdf/2606.05405)

- 解决**AI基准测试成就与实际经济产出之间的脱节问题。**论文提出**Agents' Last Exam (ALE)**——一个与250+行业专家合作开发的基准测试，专门评估AI代理在**跨55个子领域、13个行业集群的1,000+真实任务**上执行经济价值高的专业工作的能力，并通过结构化可交付成果或基于里程碑的检查实现确定性验证，而非依赖人工评判。瓶颈分析：47% 失败源于策略错误（Approach），31% 源于领域知识缺失（Understanding），仅 22% 为执行错误；代理显著低估 GUI 工具使用（34% GUI 任务未调用 GUI）。 模型 vs 框架效应：**固定 harness 换模型导致 18.0 个百分点差异，固定模型换 harness 仅 5.3-6.0 个百分点，性能主要由基础模型能力驱动**。

<!-- paperflow:1d7e3015dc6bfbf9 -->
## Recursive Agent Harnesses

[[Deep Reading - Jun 2026/Recursive Agent Harnesses|Deep Reading]]

[https://arxiv.org/pdf/2606.13643v1](https://arxiv.org/pdf/2606.13643v1)

- **本文系统性地提出并验证了“递归智能体装具”（RAH）架构。该架构的核心创新在于将递归机制从单纯的模型预测提升到了系统装具层面。在处理超长上下文（最高 4M tokens）的复杂推理任务时，RAH 允许父智能体通过编写代码动态地生成并运行具备完整工具能力的子智能体。这种“装具递归”模式克服了传统编程智能体逻辑僵化和递归语言模型能力受限的问题。通过在 Oolong-Synthetic 基准上的受控实验，作者证明了 RAH 架构在相同模型底座下能显著提升推理准确率，并展示了其在 Claude Sonnet 4.5 等前沿模型上的强大潜力。该研究为构建能够处理海量数据和复杂逻辑的自主智能体系统提供了重要的理论支撑和实践路径。**

<!-- paperflow:c5df5afade7bc903 -->
## OpenClaw-Skill: Collective Skill Tree Search for Agentic Large Language Models

[[Deep Reading - Jun 2026/OpenClaw-Skill-Collective Skill Tree Search for Agentic Large Language Models|Deep Reading]]

[https://arxiv.org/pdf/2606.16774v1](https://arxiv.org/pdf/2606.16774v1)

- **论文围绕OpenClaw-Skill框架展开，动机源于现有技能构建方法的碎片化、多样性与迁移性局限。方法部分详细描述CSTS的分解、生成与评估流程以及CSRL训练策略。实验在两个基准上展示了一致性能提升，并通过表格呈现具体得分变化。结论强调框架对LLM代理长时程能力的增强作用，同时指出集体智能在技能泛化中的关键价值。**

<!-- paperflow:4cc069999f25707a -->
## Agent trajectories as programs: fingerprinting and programming coding-agent behavior

[[Deep Reading - Jun 2026/Agent trajectories as programs-fingerprinting and programming coding-agent behavior|Deep Reading]]

[https://arxiv.org/pdf/2606.16988v1](https://arxiv.org/pdf/2606.16988v1)

- **本文针对编码智能体轨迹提出程序化分析框架，指出基准分数无法揭示行为过程，进而定义procedural fingerprints并开发ProcGrep库。在SWE-Bench实验中证明不同模型可通过轨迹本身以近90%准确率被识别，同发布期或蒸馏模型行为最接近；ProcGrep在片段搜索上优于LLM。结论强调该方法可支持开发者进行模型选择、行为编程与资源优化。**

# Memory, Personalization & Long-Horizon Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Memory, Personalization & Long-Horizon Agents
- 方法：agent, generation, reasoning, vision, retrieval, stat-ml, stat-me, econ-em
- 论文/报告：9 篇
- Beyond Visual Memory: Mechanistic Diagnostics of Latent Visual Reasoning【Visual Reasoning, Alibaba, SII】
- DecMem: Towards Minute-Long Consistent World Generation with Decoupled Memory【World Model, Kuaishou Kling】
- Connecting the Dots: Benchmarking Reflective Memory in Long-Horizon Dialogue【Memory】
- Scaling Self-Evolving Agents via Parametric Memory【Memory】
- 🧐Agent Memory: Characterization and System Implications of Stateful Long-Horizon Workloads【Memory】
- Beyond Semantic Organization: Memory as Execution State Management for Long-Horizon Agents【Memory, Microsoft】
- Memory is Reconstructed, Not Retrieved: Graph Memory for LLM Agents【Memory】
- AdMem: Advanced Memory for Task-solving Agents【Memory, Amazon】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:7c016a8b73d16492 -->
## Beyond Visual Memory: Mechanistic Diagnostics of Latent Visual Reasoning【Visual Reasoning, Alibaba, SII】

[https://arxiv.org/pdf/2606.01287](https://arxiv.org/pdf/2606.01287)

- **潜在token的收益并非来自槽位内容编码的视觉记忆，而是主要来自边界标记和格式约束**；即使在最有利的MVH监督下，槽位内容也表现出弱因果性、低可恢复性和低可迁移性。这一发现要求该领域从单纯的准确度评估转向**机制层面的诊断**。

<!-- paperflow:b679269a8f6bbbe1 -->
## DecMem: Towards Minute-Long Consistent World Generation with Decoupled Memory【World Model, Kuaishou Kling】

[https://arxiv.org/pdf/2605.31336](https://arxiv.org/pdf/2605.31336)

- 解决**长时程（分钟级）可控世界模型生成中的细粒度时空一致性保持问题。**论文提出了**DecMem**（解耦记忆架构），**通过**稀疏全局记忆（SGM）实现高效、细粒度的全局历史访问，并通过**锚定局部记忆（ALM）**稳定注意力分布，从而在保持计算效率的同时，实现分钟级、高保真且时空一致的可控视频生成。

<!-- paperflow:8de0bf92414050a6 -->
## Connecting the Dots: Benchmarking Reflective Memory in Long-Horizon Dialogue【Memory】

[https://arxiv.org/pdf/2606.01223](https://arxiv.org/pdf/2606.01223)

- 针对**长程对话中的反思性记忆（Reflective Memory）评估缺失**问题。本文提出： RefMem-Bench：首个专门针对反思性记忆的大规模多模态基准测试，涵盖**8个反思性记忆维度（包括时间动态、模式归纳、信念建模、跨模态潜在推理等）和3种任务格式（单选、多选、直接回答）**，包含26K个人工验证的问答实例。 REMIND框架：将反思性记忆建模为渐进式意义建构过程，通过"认知金字塔"（事实-注意-反思三层级）和渐进式反思对齐（Progressive Reflective Alignment）训练方法，将高阶推理能力蒸馏到基础推理路径中。 该研究揭示了当前模型在反思性记忆方面存在显著缺陷，并为构建能够进行长期、个性化、上下文感知交互的真正智能代理提供了评估标准和改进路径。主结果：**在Qwen3-VL-8B上，REMIND相比基线平均提升Acc 21.2%（SC）、26.2%（MC）、11.8%（DA），记忆召回率（MemR）提升13.1%–17.5%，在全部八个维度上均取得最优性能**。 消融研究：验证了问题条件化检索、结构化策略、显著性锚定各组件的必要性，以及自底向上层级顺序的有效性。 层级分析：逐层输入测试显示，从事实层到反思层性能单调递增，证实高层信号有效蒸馏至基础路径。 案例研究：展示了REMIND在捕捉隐式边界（如回避行为）与抽象行为模式（如责任推卸）方面相比基线的优势。

<!-- paperflow:8721c08d381cebdf -->
## Scaling Self-Evolving Agents via Parametric Memory【Memory】

[https://arxiv.org/pdf/2606.04536](https://arxiv.org/pdf/2606.04536)

- 解决长程LLM智能体（long-horizon LLM agents）在单轮交互（single episode）中无法通过经验实现参数层面自我演化的问题。 具体而言，现有记忆增强智能体存在以下结构性局限： 提示空间的记忆瓶颈：现有方法仅将历史经验以文本摘要或检索片段的形式存储在提示（prompt）中，而模型参数在交互过程中保持冻结。这导致智能体只能"查阅"过往经验，却无法将其"内化"到决策机制中。 信息丢失不可逆：一旦上下文被截断或压缩，被省略的证据便永久丧失影响策略的能力，无法通过参数更新来保留关键知识。 行为策略静态化：由于参数冻结，智能体的行为策略在单轮长程交互中保持不变，无法根据实时积累的经验动态调整未来的推理和决策方式。 针对上述问题，论文提出TMEM（Test-time Memory with Evolving Model-parameters）框架，核心创新在于将记忆视为参数化的动态过程： 在交互过程中，智能体通过LoRA（Low-Rank Adaptation）权重$\Delta_t$维护快速参数化记忆（fast parametric memory） 当上下文预算耗尽时，智能体将当前会话蒸馏为QA形式的监督信号，并通过轻量级在线SFT更新$\Delta_t$ 后续动作从自适应策略$\pi_{\theta_0+\Delta_t}$中采样，使得经验能够直接改变模型的计算 substrate，而无需反复将历史证据重新注入提示 该形式化框架将记忆提取视为可优化的决策动作，通过停止梯度（stop-gradient）策略优化，使基础模型$\theta_0$不仅学习解决任务，还学习生成有利于后续在线适应的高质量监督数据。

<!-- paperflow:cc6698f34e90ab85 -->
## 🧐Agent Memory: Characterization and System Implications of Stateful Long-Horizon Workloads【Memory】

- 解决**基于大语言模型（LLM）的智能体（Agent）记忆系统在系统层面的行为表征与优化问题**。论文揭示了**构建成本（write path）往往主导智能体生命周期、且与查询服务（read path）存在结构性资源冲突**这一关键洞察，填补了智能体记忆系统在系统层面的表征空白。基于表征结果，论文提出10条面向基础设施设计的工程建议，核心包括：工作负载隔离：将构建视为后台吞吐型任务，与延迟敏感查询解耦，实施显式准入控制;模型分级：利用构建LLM与查询LLM的解耦，对无严格契约系统使用更小模型进行构建;成本-查询量匹配：高查询量场景选择高构建成本系统（如Mem0）以摊薄成本；稀疏查询场景选择低构建成本系统（如BM25）;硬性边界设置：为LLM有界系统配置外部迭代上限和超时机制，防止无限深度推理增长控制：对范式IV系统实施主动压缩、摘要和遗忘策略，防止长期部署中的成本失控。
    
    ### 方法论框架
    
    **四维分类体系**：按构建机制（Construction）、存储组织（Storage）、检索策略（Retrieval）、可变性（Mutability）将记忆系统划分为四种范式：
    
    - **范式I**：长上下文记忆（无外部构建）
    - **范式II**：扁平RAG（确定性索引，无LLM参与构建）
    - **范式III**：结构增强RAG（LLM中介提取，分追加-only与整合型）
    - **范式IV**：智能体控制流（LLM自主决定记忆操作）
    
    **阶段感知分析工具**：开发开源性能分析框架，将智能体执行分解为**构建**、**检索**、**生成**三阶段，统一追踪API调用结构（token量、批次大小）与硬件遥测（GPU利用率、能耗、HBM带宽）。
    
    ### 3. 核心实验发现
    
    **（1）构建成本主导生命周期** 对LLM中介系统（范式III/IV），构建能耗占端到端能耗的>90%，远超300次查询的累计服务成本。系统间**每正确答案能耗**差距达47×（BM25：4.1 kJ vs. Letta：185.9 kJ）。
    
    **（2）构建阶段的计算特征** 构建是**预填充（Prefill）与嵌入主导型**工作负载，LLM解码token占比中位数仅4.6%。嵌入流量呈双峰分布：
    
    - 范式III.a产生大批量离线索引（GraphRAG每批2,300序列）
        
        ∼
        
    - 范式III.b/IV产生顺序单事件写入（Mem0为调用-序列比）
        
        1:1
        
    
    **（3）帕累托前沿与能力下限** 构建延迟、查询延迟、准确性三者**无法同时最优**：
    
    - BM25构建最快（）但查询延迟较高（7.4s）
        
        <1s
        
        ∼
        
    - Mem0查询延迟最低（）但构建成本高昂（1.1h）
        
        <0.1s
        
        ∼
        
    - 有严格输出契约的系统（如MIRIX）对构建LLM能力存在**硬下限**（在Qwen3-1.7B上完全失效）
        
    
    **（4）跨会话新鲜度-延迟权衡** 连续会话场景下，构建速度慢于会话到达间隔的系统面临强制选择：
    
    - **同步调度**：阻塞查询直至写入完成，暴露构建延迟（Letta可达/会话）
        
        > 100s
        
    - **异步调度**：允许查询读取陈旧记忆（staleness达2个会话）
        
    
    **（5）超线性成本增长** 范式IV系统（Letta、A-Mem、MIRIX）的构建token成本随记忆库规模**超线性增长**（因每步摄入需查询评估整个库），而存储占用仅线性增长（7MB–62MB/1M tokens，系统间差异9×）。
    
    **（6）尾延迟差异** 算法有界系统（范式II）p95/p50延迟比≈1.3×；LLM有界系统（范式IV）因运行时决策（反射轮数、工具调用次数）不确定，尾延迟比可达3.9×–5.9×。

<!-- paperflow:9e18fce01184c638 -->
## Beyond Semantic Organization: Memory as Execution State Management for Long-Horizon Agents【Memory, Microsoft】

[https://arxiv.org/pdf/2606.06090](https://arxiv.org/pdf/2606.06090)

- 解决**基于大语言模型（LLM）的Agent在长程任务（long-horizon tasks）中的记忆管理缺陷**，具体而言是现有检索增强生成（RAG）和Agent记忆系统在处理**执行状态依赖（execution-state dependencies）**时的结构性错配问题。

<!-- paperflow:a22b1bd08c0f8db1 -->
## Memory is Reconstructed, Not Retrieved: Graph Memory for LLM Agents【Memory】

[https://arxiv.org/pdf/2606.06036](https://arxiv.org/pdf/2606.06036)

- 解决**大型语言模型（LLM）代理在长程交互历史中进行记忆推理时的核心局限**。论文提出MRAgent框架，通过以下机制实现主动记忆重建：Cue–Tag–Content图结构：引入联想标签（Tags）作为语义中介，显式编码线索与内容间的关联关系LLM驱动的重建机制：将记忆访问形式化为序列决策过程，使代理能够基于累积证据H(t)动态选择下一步遍历动作，实现检索与推理的紧耦合这一范式转变使得记忆检索从静态的"匹配-提取"转变为动态的"探索-重建"，从而在LOCOMO和LongMemEval等长程记忆基准上实现显著性能提升（最高达23%），同时降低计算成本。

<!-- paperflow:455b424a26073f0f -->
## AdMem: Advanced Memory for Task-solving Agents【Memory, Amazon】

[https://arxiv.org/pdf/2606.06787](https://arxiv.org/pdf/2606.06787)

- 论文提出 **AdMem** 框架，旨在构建一个**统一、自动化、可扩展的记忆系统**，整合语义、情景和程序三种记忆类型，通过双层（短期-长期）架构和多智能体协作（演员-记忆-批评者），使代理能够从成功和失败中学习，实现长程任务中的持续自我改进和鲁棒决策。建立符合认知科学的三分记忆体系：语义记忆（MS）：事实与世界知识；情景记忆（ME）：历史交互摘要；程序性记忆（MP）：决策指导（包含成功与失败经验）。采用**短期记忆（STM）与长期记忆（LTM）**双层结构：STM通过栈式管理实现上下文压缩，LTM支持跨任务持久化与自动维护。将记忆功能解耦为三个并行智能体： 演员智能体：负责任务执行，维护STM，通过规划工具（add subgoal / conclude subgoal / act / think）结构化决策； 批评者智能体：延迟评估动作质量，对比期望结果 o^t+1 与实际观察 ot+1 ，生成带奖励 rt∈{0,1} 的程序性记忆条目 (ck,ak,fk) ； 记忆智能体：管理LTM的存储、检索与生命周期（合并、剪枝）。

<!-- paperflow:c5102b6ad6c8222c -->
## Infini Memory: Maintainable Topic Documents for Long-Term LLM Agent Memory【Memory】

[https://arxiv.org/pdf/2606.10677](https://arxiv.org/pdf/2606.10677)

- 针对**长期LLM智能体（Long-term LLM Agents）的持久记忆维护问题。**论文提出Infini Memory架构，其关键设计包括： 主题文档表示：**将记忆组织为可维护的文本主题文档（topic-structured documents）**，而非孤立条目或图谱节点 缓冲写入与定期整合：**通过CURRENT缓冲区收集新观察，定期重写并整合到主题库中**，实现事实修正与去重 智能体检索（Agentic Retrieval）：**让LLM通过迭代工具调用主动搜索、验证和扩展证据，而非单次检索** 该设计旨在实现跨会话的证据聚合（evidence aggregation）、事实修正（fact revision）和记忆维护（memory maintenance），同时保持记忆状态的可读性、可编辑性和基础设施简洁性。

# AI for Science & Biology

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science & Biology
- 方法：agent, ai-for-science, language, bio-molecular, science-discovery, optimization, stat-ml, stat-me
- 论文/报告：10 篇
- AutoSci: A Memory-Centric Agentic System for the Full Scientific Research Lifecycle【Research】
- Benchmarking AI Agents for Addressing Scientific Challenges Across Scales【Research, Science】
- SwitchCraft: A Programmatic Framework for Designing State-Switching Proteins【Bowen Jing】
- AMix-2: Establishing Protein as a Native Modality in Large Language Models【AI Lab】
- TadA-Bench: A Million-Variant Benchmark for Future-Round Discovery Toward Agentic Protein Engineering
- 🧐GENEB: Why Genomic Models Are Hard to Compare
- MultiMolecule: a modular ecosystem for biomolecular sequence-model workflows
- Speaking the Language of Science: Toward a General-Purpose Generative Foundation Model for the Natural Sciences
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:673fdb6ce25bdf13 -->
## AutoSci: A Memory-Centric Agentic System for the Full Scientific Research Lifecycle【Research】

[https://arxiv.org/pdf/2605.31468](https://arxiv.org/pdf/2605.31468)

- 论文提出了 AutoSci，一个以记忆为中心的智能体系统，通过以下四个模块实现科学研究的全自动化、可记忆与可进化： SciMem：提供模式约束的研究记忆，分离长期知识记忆与活跃研究记忆 SciFlow：基于执行框架（harness）的五阶段生命周期执行引擎 SciDAG：通过DAG形多智能体算子增强复杂阶段的执行能力 SciEvolve：将反馈信号转化为记忆组织、技能与模板的版本化更新 该系统旨在使科学智能体从"单次会话助手"转变为可跨项目持续执行、记忆与进化的持久性研究环境。

<!-- paperflow:c2e768dcfb80c80f -->
## Benchmarking AI Agents for Addressing Scientific Challenges Across Scales【Research, Science】

[https://arxiv.org/pdf/2606.12736](https://arxiv.org/pdf/2606.12736)

- 论文提出了SciAgentArena——一个系统性的基准测试平台，其设计目标包括：覆盖多领域：涵盖单细胞组学、空间组学、计算药物发现、电子健康记录（EHR）建模和遗传学五大领域；任务多样化：约200个任务覆盖数据分析、优化、发现和有效性验证四大类别，支持逐步验证和交互式评估；智能体无关架构：分离运行框架与评估框架，支持多样化AI智能体（包括通用型与领域专家型）的公平比较；真实场景导向：任务源于实际研究需求，包含长程依赖、工具使用、多约束优化和开放性研究问题。通过这一框架，论文揭示了**当前AI智能体在科学任务中的能力梯度（擅长执行明确的数据分析工作流，但在生成新颖见解、自主探索和处理开放性问题上表现不佳），并识别出常见的失效模式（如方法选择的保守收敛、工具幻觉、缺乏有效性检查等）**，为下一代科学AI智能体的设计提供了实证依据和改进方向。

<!-- paperflow:3611ed326209326f -->
## SwitchCraft: A Programmatic Framework for Designing State-Switching Proteins【Bowen Jing】

[https://arxiv.org/pdf/2605.31236](https://arxiv.org/pdf/2605.31236)

- 解决**多状态蛋白质（multistate proteins）的理性计算设计**问题。论文提出了**SWITCHCRAFT**框架，将多状态蛋白质设计形式化为**在结构预测模型参数化的组合设计约束下，通过反向传播进行序列优化的问题**。

<!-- paperflow:6b45f2acc33fcf2e -->
## AMix-2: Establishing Protein as a Native Modality in Large Language Models【AI Lab】

[https://arxiv.org/pdf/2605.30963](https://arxiv.org/pdf/2605.30963)

- 如何将蛋白质建立为大型语言模型（LLM）中的原生模态（native modality）？论文引入ProteinArena基准，采用时间感知（temporal cutoff）和同源性感知（<30%序列同一性）的评估协议，确保模型测试在 realistic generalization settings 下进行。 简言之，该论文旨在构建一个**兼具蛋白质专用模型的深度建模能力与LLM的灵活指令遵循能力的统一基础模型**，使蛋白质知识能够内化为模型原生能力并在不同任务间迁移，而非分散在特定的任务流水线中。

<!-- paperflow:f2484fec77d2da22 -->
## TadA-Bench: A Million-Variant Benchmark for Future-Round Discovery Toward Agentic Protein Engineering

[https://arxiv.org/pdf/2606.02624](https://arxiv.org/pdf/2606.02624)

- 解决**蛋白质工程领域缺乏针对"未来轮次发现"（future-round discovery）能力的标准化评估基准**的问题，**数据来源：31轮 TadA 脱氨酶定向进化（PANCE 筛选）的百万级 NGS 数据，包含 1,027,200 条 DNA 序列（蛋白质视图 409,869 条独特氨基酸序列）**。对齐视图：提供 DNA、RNA（T→U 转换）与蛋白质三种对齐的序列视图，支持多模态生物语言模型评估。 评估协议：固定数据回放（fixed-data replay）—— 模型基于第 1–27 轮数据训练，在第 29–31 轮（仅包含未见过的新变体）上进行排名评估，模拟"基于历史证据预测未来"的决策场景。

<!-- paperflow:943a50f445ea560a -->
## 🧐GENEB: Why Genomic Models Are Hard to Compare

[https://arxiv.org/pdf/2606.04525](https://arxiv.org/pdf/2606.04525)

- 论文提出了GENEB（Genomic Evaluation Benchmark），其核心贡献包括： 在统一协议（线性探针、固定随机种子、标准化数据预处理）下评估40个模型在100个任务（涵盖13个功能类别）上的表现 提供控制变量比较（controlled comparison），隔离架构、分词策略、预训练数据和模型规模的影响 暴露任务级权衡（task-level trade-offs），证明聚合排行榜（aggregate leaderboards）的不稳定性：模型排名在不同任务类别间剧烈变化，规模仅提供有限且不一致的收益 简而言之，该论文试图将基因组基础模型的评估从碎片化、不可比的状态，转变为系统化、原则性的比较框架，类似于自然语言处理领域MTEB（Massive Text Embedding Benchmark）所起的作用。论文主张基于类别的模型选择（category-aware selection）取代依赖单一聚合排名。主要结论包括： **对于调控任务（TF结合、增强子），优先选择人类-小鼠表观基因组预训练模型（ENFORMER、SPACE）** 对于资源受限部署，**86M参数的MUTBERT在8/13类别中是最佳亚100M模型** 对于植物基因组（lncRNA），专用模型（PLANTCADUCEUS）显著优于人类预训练模型，但多物种模型（LUCAONE）也可作为替代 对于少样本场景（~10标注样本），必须针对具体任务重新评估模型选择，全数据性能不具备预测性

<!-- paperflow:d809edb694f27190 -->
## MultiMolecule: a modular ecosystem for biomolecular sequence-model workflows

[[Deep Reading - Jun 2026/MultiMolecule-a modular ecosystem for biomolecular sequence-model workflows|Deep Reading]]

[https://arxiv.org/pdf/2606.16540v1](https://arxiv.org/pdf/2606.16540v1)

- **论文介绍了 MultiMolecule 这一开源生态系统，旨在解决生物分子序列模型重用中的执行上下文缺失问题。通过将异构模型发布转化为带有共享接口的完整实现，资源包含 53 个模型家族、112 个检查点、16 个数据集和 10 个预测管道，所有组件均与源出处和验证证据链接。方法部分描述了五层组织结构，结果展示了标准化资产如何支持可配置工作流和生物学预测，讨论强调了从检查点重用向可执行科学基础设施的转变。**

<!-- paperflow:dd2bcea9a467d8c1 -->
## Speaking the Language of Science: Toward a General-Purpose Generative Foundation Model for the Natural Sciences

[[Deep Reading - Jun 2026/Speaking the Language of Science-Toward a General-Purpose Generative Foundation Model for the Na|Deep Reading]]

[https://arxiv.org/pdf/2606.16905v1](https://arxiv.org/pdf/2606.16905v1)

- **论文提出 LOGOS 模型，通过共享科学语法将自然科学多领域对象编码为统一 token 序列，实现单一自回归框架下的多任务生成。作者构建大规模多模态预训练数据，验证了模型在配体设计、蛋白编辑、材料生成等任务上的竞争力，并展示模型规模与性能的正相关关系，主张 AI4S 应与 LLM 架构深度对齐。模型权重已开源。**

<!-- paperflow:13ea7f805642f3df -->
## What Does a Chemical Language Model Know About Molecules?

[[Deep Reading - Jun 2026/What Does a Chemical Language Model Know About Molecules|Deep Reading]]

[https://arxiv.org/pdf/2606.23443](https://arxiv.org/pdf/2606.23443)

- **本文针对化学语言模型（cLMs）是否真正学习分子语义的争议，采用机制可解释性方法——稀疏自编码器（SAEs）来分析编码器型模型MolFormer-XL。通过对各层残差流训练SAE，作者发现早期层主要编码位置跟踪隐变量以解析SMILES语法，后期层则捕获原子-子结构和药理学相关特征。这一发现支持cLMs确实学习了有意义的化学知识。进一步，通过比较规范SMILES、非规范SMILES和无效SMILES的表示，发现非规范SMILES导致更剧烈的表示偏移，其根源在于位置隐变量的跨层传播。这表明模型对输入字符串的规范形式敏感，而非简单的语法错误。最后，作者提供了交互式可视化工具InterMol便于社区探索。**

<!-- paperflow:259488b6162c943d -->
## Is GraphRAG Needed? From Basic RAG to Graph-/Agentic Solutions with Context Optimization

[[Deep Reading - Jun 2026/Is GraphRAG Needed-From Basic RAG to Graph-Agentic Solutions with Context Optimization|Deep Reading]]

[https://arxiv.org/pdf/2606.25656v1](https://arxiv.org/pdf/2606.25656v1)

- **本文针对基本 RAG 在处理半结构化知识库（如精准医学知识图谱）时的局限性，系统比较了包括基本 RAG、GraphRAG、Modular RAG 和 Agentic RAG 在内的多种 RAG 变体。作者首先定义了 9 个标准化的 RAG 场景，覆盖从简单文档检索到高级智能体-图集成的不同复杂度，并给出了具体实现。然后在包含 129K 实体和 8.1M 关系的精准医学知识库上进行实验，比较了各场景在检索精度、答案质量、效率等方面的表现。针对 GraphRAG 和 Agentic RAG 中常见的上下文溢出问题，提出了一种新颖的上下文工程方法，通过新的文本/图表示和定制化智能体循环模式（超越 ReAct）来管理检索结果，使模型能有效利用长上下文。实验揭示了检索-生成间隙，即 LLM 选择的实体并不总是最优，建议端到端评估。论文最终为实践者提供了在不同用例、数据特征和性能约束下选择合适 RAG 架构的实证指导。**

# Foundation AI & AGI

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Foundation AI & AGI
- 方法：deep-learning
- 论文/报告：2 篇
- ⭐⭐⭐On the Scaling of PEFT: Towards Million Personal Models of Trillion Parameters【Personalize】
- From AGI to ASI【Google DeepMind】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:d752dd9a83d65433 -->
## ⭐⭐⭐On the Scaling of PEFT: Towards Million Personal Models of Trillion Parameters【Personalize】

[https://arxiv.org/pdf/2606.02437](https://arxiv.org/pdf/2606.02437)

- 该论文试图证明：**PEFT不仅是全量微调的廉价替代品，而是实现"一个共享基础模型支撑百万持久化个性化模型"这一架构的关键机制**，使个性化模型能够承载连续性记忆、支持用户模拟、并通过多样性产生集体智能。论文提出"个人模型"（Personal Model）架构——以强共享基础模型（trillion-scale）提供通用能力，以极小的PEFT适配器（<1%参数）承载本地自适应状态（记忆、技能、工具习惯），通过三轴协同扩展实现百万级持久化实例的可扩展部署。论文的主框架是三个 scaling axis：**Scale Up、Scale Down、Scale Out**。**Scale Up 指更强的共享大模型让小 adapter 更有用，RL 是 prior-limited 的。** 也就是说，强化学习不是凭空创造能力，而是在当前模型能采样到的轨迹空间里强化高奖励行为；base model 越强，越容易采样到有用但不稳定的推理、工具使用、长程策略，因此小 LoRA 更新就更有杠杆；**Scale Down 指把个人状态压到很小但还能稳定学习**；**Scale Out 指当很多 persistent adapters 同时存在时，可以形成个性化、用户模拟、群体投票/路由/聚合等新能力**。论文提出 **Context Learning**：不是把 RAG、工具结果、上下文都塞进 prompt，而是让“带上下文系统”给“无上下文 query-only policy”的输出打分，再用 RL-style update 把这种改进写进 adapter。换句话说，Context Engineering 是“这次回答怎么把上下文摆好”，Context Learning 是“哪些反复有用的上下文收益应该沉淀成未来的模型状态”。论文强调，这不是把所有检索事实参数化，而是把反复出现的事实、偏好、流程、工具习惯部分内化，同时保留外部记忆的可编辑性。**论文用“人类共享 99% 以上基因，但每个人有个体差异”类比：共享 foundation model 像共同基因组，adapter 像个体差异**。

<!-- paperflow:bac5fdded3081287 -->
## From AGI to ASI【Google DeepMind】

[https://arxiv.org/pdf/2606.12683](https://arxiv.org/pdf/2606.12683)

- 该论文试图构建一个**从AGI到ASI的完整技术路线图**，既包括推动进步的引擎，也包括可能减速的摩擦，并强调在高度不确定的未来中，需要通过大规模跨学科研究来持续更新这些认知。AGI：定义为达到中位数人类水平（median human-level）认知能力的系统，在广泛任务上表现与人类相当。 ASI：定义为在几乎所有领域超越大规模人类专家集合体（数千人规模、长期协作）的通用超级智能。

# Language Models

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Language Models
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, vision, reinforcement-learning
- 论文/报告：24 篇
- Exploring Extrinsic and Intrinsic Properties for Effective Reasoning with Code Interpreter
- Revisiting the Systematicity in Negation in the Era of In-Context Learning
- Efficiently Representing Algorithms With Chain-of-Thought Transformers
- Benchmarking Agentic Review Systems
- Self-Preference Is Weak or Absent in Verifiable Instruction-Following Revision: A Four-Model Test Under Genuine Authorship
- What Makes Effective Supervision in Latent Chain-of-Thought: An Information-Theoretic Analysis
- Scaling LLM Knowledge Boundaries via Distribution-Optimized Synthesis
- TROPT: An Open Framework for Unifying and Advancing Discrete Text Optimization
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:83a6ded0db7a373e -->
## Exploring Extrinsic and Intrinsic Properties for Effective Reasoning with Code Interpreter

[[Deep Reading - Jun 2026/Exploring Extrinsic and Intrinsic Properties for Effective Reasoning with Code Interpreter|Deep Reading]]

[https://arxiv.org/pdf/2606.16934v1](https://arxiv.org/pdf/2606.16934v1)

- **论文首先在引言中阐述CI推理的兴起及其与NL推理机制研究的联系，随后分解外在与内在属性，通过多模型统计建立关联；接着在推理与训练两个阶段设计利用策略并验证效果；讨论部分深入分析Qwen3-8B失效原因与CoBe对token效率的正面作用；结论总结贡献与局限。整体为系统性刻画而非单一增量改进。**

<!-- paperflow:11d73dce7157139a -->
## Revisiting the Systematicity in Negation in the Era of In-Context Learning

[[Deep Reading - Jun 2026/Revisiting the Systematicity in Negation in the Era of In-Context Learning|Deep Reading]]

[https://arxiv.org/pdf/2606.16867v1](https://arxiv.org/pdf/2606.16867v1)

- **论文重新审视大语言模型在上下文学习下的否定系统性。摘要指出，系统性是表征属性而非单纯行为属性；实验确认模型可部分识别否定与作用域，但作用域识别受输出格式影响较大；函数向量可在提示提取任务上稳健构造，而作用域任务更困难。整体框架结合行为测试与表征干预，强调需同时考察两者才能判断模型是否真正掌握否定理解的结构化原则。**

<!-- paperflow:b19760264736e42c -->
## Efficiently Representing Algorithms With Chain-of-Thought Transformers

[[Deep Reading - Jun 2026/Efficiently Representing Algorithms With Chain-of-Thought Transformers|Deep Reading]]

[https://arxiv.org/pdf/2606.19697v1](https://arxiv.org/pdf/2606.19697v1)

- **本文将 Word RAM 模型引入 CoT Transformer 的表达能力分析，解决了“CoT Transformer 能否高效模拟现实算法”这一核心问题。作者首先回顾了现有图灵机模拟的局限性，然后针对三种 Transformer 变体——多对数宽度 Transformer、连续 CoT Transformer 和混合架构（Transformer+线性 RNN）——设计了显式的模拟构造。主要结果是：在有限精度和有限宽度条件下，所有三种架构均能以多对数（poly-logarithmic）开销模拟任意 Word RAM 算法；对于平坦（flat）指令集，开销可降至对数平方；对于无乘法平坦指令集，仅需对数开销。这一结果显著优于模拟图灵机所需二次开销。构造中利用了自注意力的内容寻址能力、连续表示的密度以及线性 RNN 的状态持久性。论文还讨论了 Word RAM 的多种变体（C-like、无乘法等），给出了具体的复杂度定理。最终结论：CoT Transformer 不仅计算普适，还能以接近最优的效率模拟标准算法，为推理模型的理论基础提供了更强有力的支撑。**

<!-- paperflow:62cf212e50c22ce3 -->
## Benchmarking Agentic Review Systems

[[Deep Reading - Jun 2026/Benchmarking Agentic Review Systems|Deep Reading]]

[https://arxiv.org/pdf/2606.19749v1](https://arxiv.org/pdf/2606.19749v1)

- **本文针对新兴的代理评审系统提出了一套多维度评估框架，旨在衡量其与人类质量判断的一致性、错误检测能力以及真实用户接受度。研究背景是AI辅助研究导致同行评审不堪重负，代理评审系统成为缓解压力的潜在方案，但缺乏系统化的评估方法和基准。

论文设计了三个互补的实验：
1）质量代理研究：以ICLR/NeurIPS会议论文为对象，使用引用数和接受决定作为论文质量代理，评估AI评审对论文对的排序是否与这些代理一致。结果表明所有系统均优于随机基线，最佳系统（OpenAIReview+GPT-5.5）达到83.0%的配对准确率，说明AI评审能捕捉到部分与人类判断相关的质量信号。
2）扰动基准：为弥补代理信号不够精细的缺陷，论文构造了一个包含四类注入错误（逻辑错误、方法缺陷、实验设计问题、写作问题）的基准，覆盖八个arXiv学科类别。测量系统检测这些错误的召回率。最强单模型（OpenAIReview+GPT-5.5）召回率为71.6%，但六个不同后端模型的联合召回率达到83.3%，提升了11.7个百分点，显示模型间存在互补性：不同模型擅长检测不同类型的错误。这暗示通过集成设计可以显著提升性能。
3）真实用户部署：将OpenAIReview系统部署到CHI 2026会议评审流程中，收集用户对AI评论的投票和文字反馈。投票正面负面比例1.44:1（偏向正面），但常见投诉包括假阳性（系统误判正确的部分为错误）和提出细微的吹毛求疵意见。这些反馈揭示了当前系统在精度和相关性上的不足。

论文的主要贡献包括：
- 提出了系统评估代理评审系统的多层次方法，涵盖质量代理、扰动检测和真实部署。
- 建立了首个跨领域的扰动基准，四类错误和八个学科类别。
- 实证发现不同后端模型互补，联合召回率大幅提升。
- 提供了真实用户的正面和负面反馈，为系统改进指明方向。

局限性方面，作者坦诚指出：
- 扰动基准仅测量召回率，未评估精度；假阳性或预存错误无法区分。
- 错误注入可能偏向LLM易感知的类型（因为生成器和评估LLM可能有同源偏差）。
- 质量代理研究限于计算机科学顶级会议，引用和接受作为代理本身有噪声。
-...**

<!-- paperflow:9bf209c0a534974f -->
## Self-Preference Is Weak or Absent in Verifiable Instruction-Following Revision: A Four-Model Test Under Genuine Authorship

[[Deep Reading - Jun 2026/Self-Preference Is Weak or Absent in Verifiable Instruction-Following Revision-A Four-Model Test|Deep Reading]]

[https://arxiv.org/pdf/2606.20093v1](https://arxiv.org/pdf/2606.20093v1)

- **本文系统测试了LLM在可验证指令遵循修订任务中的自我偏好偏差。具体问题：当模型作为原作者时，是否比作为新鲜判断者更可能拒绝一个已验证有效的修正？作者设计了三个渐进的实验：P1使用揭示作者身份的代理；P2采用严格的作者-新鲜对比；P3在四个中档模型（共85个示例）上重复。主要发现：在确定性IFEval检查器确保修正有效的条件下，原作者拒绝有效修正的比率与新鲜模型无显著差异（差距-5.1pp，95% CI包含0）。定性分析表明，当原作者拒绝时，几乎全部因为草稿本身存在其他不符合约束的问题（即“捕捉缺陷”），而非对修正本身的抗拒。唯一暗示自我怀疑的小规模效应未能在大规模实验中复现。作者据此认为，在可验证的修改任务中，自我偏好偏倚很弱甚至不存在，但更小的效应（<13pp）可能因统计功效不足而未被检测到。论文还讨论了与奉承、匿名化、模型大小等相关的文献，并强调了使用确定性验证器相比主观判断的优势。**

<!-- paperflow:cba655a5bd2a32c4 -->
## What Makes Effective Supervision in Latent Chain-of-Thought: An Information-Theoretic Analysis

[[Deep Reading - Jun 2026/What Makes Effective Supervision in Latent Chain-of-Thought-An Information-Theoretic Analysis|Deep Reading]]

[https://arxiv.org/pdf/2606.20075v1](https://arxiv.org/pdf/2606.20075v1)

- **本文针对潜在链式思维推理中结果监督不足的问题，提出信息论分析框架。首先从理论层面识别出结果监督导致的双重崩溃：梯度衰减和表征漂移。然后提出过程监督的两个互补维度——轨迹监督（密集逐步信号注入）和空间监督（保持语义流形），并分析不同空间监督方式（几何压缩vs.生成式重构）的优劣。为量化潜在推理的信息保留量，设计了统一潜在探针（ULP），通过共享解码器从潜在状态重建显式推理步骤，计算互信息。实验在ProntoQA数据集上进行，验证了过程监督的优越性（PS-LATENT准确率97.40%），并揭示信息-性能绑定关系：推理准确度与潜在表征保留的信息量成正比。该工作为潜在推理监督提供了原则性框架，建议从几何模仿转向互信息最大化，有助于指导未来潜在推理模型的设计。**

<!-- paperflow:cc02c8c5e3d69627 -->
## Scaling LLM Knowledge Boundaries via Distribution-Optimized Synthesis

[[Deep Reading - Jun 2026/Scaling LLM Knowledge Boundaries via Distribution-Optimized Synthesis|Deep Reading]]

[https://arxiv.org/pdf/2606.23271](https://arxiv.org/pdf/2606.23271)

- **论文从知识分布优化的视角出发，针对现有合成数据方法缺乏分布意识导致LLM知识边界受限的问题，提出KDoS框架。该框架引入知识密度作为可控变量，通过三阶段动态反馈机制（监测、调整、迭代）将生成过程从盲目填充转变为定向分布塑造。作者在Wikipedia数据集上构造不同知识分布的合成数据，并在0.6B至16B规模的多种LLM（Qwen、Ling、LLaMA）上实验，数据量从1B到5B令牌。关键发现包括：存在一个稳定且可泛化的最优知识分布，能一致最大化知识边界扩展；KDoS在六个知识基准上优于所有基线。消融实验验证了密度定义和反馈轮数的有效性。论文的主要贡献在于提出了一种分布优化的新视角和实用框架，揭示了知识分布对知识注入效率的关键影响。局限在于当前仅覆盖SFT阶段，未验证预训练；且最优分布的定义仍需要人工指定目标分布。未来工作可扩展到预训练、更大规模及更细粒度知识度量。**

<!-- paperflow:fdb302a0c3efc143 -->
## TROPT: An Open Framework for Unifying and Advancing Discrete Text Optimization

[[Deep Reading - Jun 2026/TROPT-An Open Framework for Unifying and Advancing Discrete Text Optimization|Deep Reading]]

[https://arxiv.org/pdf/2606.23496](https://arxiv.org/pdf/2606.23496)

- **论文提出 TROPT，第一个用于离散文本触发优化的开源框架。它解决了现有优化器分散、难以比较和扩展的问题。TROPT 提供统一的接口和模块化组件（15+优化器、15+损失函数），支持快速定制和跨域迁移。框架内置30+现成优化方案，涵盖LLM越狱、模型探测等任务。通过大规模实验对比和跨域迁移（如将越狱优化器用于嵌入投毒），验证了框架的有效性，并揭示了若干被忽视的高效技术。论文的主要贡献是降低了离散文本优化的采用门槛，促进了该领域的标准化和进步。**

<!-- paperflow:68a3a7e16119257f -->
## SPIRAL: Learning to Search and Aggregate

[[Deep Reading - Jun 2026/SPIRAL-Learning to Search and Aggregate|Deep Reading]]

[https://arxiv.org/pdf/2606.23595](https://arxiv.org/pdf/2606.23595)

- **论文介绍SPIRAL（Sequential-Parallel-Aggregative Reinforcement Learning），一种通过强化学习统一训练语言模型在推理时使用顺序、并行和聚合三种计算原语的框架。现有后训练方法（如GRPO）只优化单轨迹顺序推理，无法高效利用推理时的并行计算和聚合。SPIRAL首先定义三个原语，并设计一个统一的计算流程：模型先并行生成一组独立轨迹（每个轨迹是链式思考），然后基于这些轨迹生成一个聚合轨迹。训练目标分为两部分：集合强化学习训练模型生成对聚合有用的轨迹集（鼓励多样性和集体正确性），标准强化学习训练模型如何从轨迹集中聚合出正确答案。两者端到端优化，共享参数。在数学推理任务上，SPIRAL在相同计算量下性能远超GRPO（提升15%，效率提升11倍）。实验表明SPIRAL有效训练模型学会了更好的并行探索和聚合策略，且计算缩放效果显著。主要贡献：提出并验证了端到端训练模型使用多种推理计算原语的框架；展示集合RL在推理训练中的有效性；提供了一种可叠加现有RL方法的扩展方案。**

<!-- paperflow:ee0a571f7ffb5e25 -->
## TailorMind: Towards Preference-Aligned Multimodal Content Generation

[[Deep Reading - Jun 2026/TailorMind-Towards Preference-Aligned Multimodal Content Generation|Deep Reading]]

[https://arxiv.org/pdf/2606.23643](https://arxiv.org/pdf/2606.23643)

- **TailorMind论文系统性地研究了个性化多模态内容生成问题，即如何在不依赖已有UGC池的情况下，根据用户行为历史直接生成符合其偏好的多模态内容（图文笔记、短视频、文本）。论文首先指出现有个性化系统受限于UGC库存，而现有生成模型缺乏将行为数据转换为生成指令的有效方法。针对这一gap，作者提出了TailorMind框架，核心创新在于将协同偏好建模与可控生成通过可优化的文本用户画像连接起来。技术路线包括：超图协同过滤从稀疏交互中提取高阶偏好；排序误差反馈与文本梯度下降迭代优化用户画像，使其更精确；检索增强风格控制利用用户历史样本作为风格锚点，避免生成偏离；跨模态衔接反射减少图文/视频间的语义漂移。为支撑评估，构建了来自Rednote、Bilibili、Hupu三个主流平台的TailorBench基准，包含完整用户交互与多模态项目，并提供五个维度的评价协议（连贯性、新颖性、美学、幻觉、画像）。实验部分与17种基线对比，覆盖检索、非个性化生成、个性化微调等方法，使用自动VLM评分和人工评估。关键实验发现包括：TailorMind在新颖性和美学上显著超越真实UGC，Recall增益最高达29%，且在稀疏用户上优势更明显。消融验证了各模块的有效性，成本效率分析显示其比LLM基线更经济。论文的主要贡献包括：定义了“无库存个性化生成”任务；提出了联合行为建模与生成的端到端框架；构建了标准化基准；展示了生成内容在推荐重排序中的潜力。局限在于：人工评估规模有限（20样本/类别）；跨域泛化尚未验证；对极端冷启动用户的性能可能不足。整体上，这是一项将推荐系统与生成式AI结合的系统性工作，在任务设定、方法设计和实验证明上均有贡献。**

<!-- paperflow:449d99cd801c4013 -->
## On the Limits of Prompt-Conditioned Language Models as General-Purpose Learners

[[Deep Reading - Jun 2026/On the Limits of Prompt-Conditioned Language Models as General-Purpose Learners|Deep Reading]]

[https://arxiv.org/pdf/2606.23668](https://arxiv.org/pdf/2606.23668)

- **本文从理论上严格论证了提示条件语言模型并非通用问题求解器。作者将用户与LLM的交互建模为廉价谈话博弈，其中用户通过自然语言提示传递任务信息，LLM作为解答者进行推断和执行。通过引入概念分解，将任务推理与执行分离，并应用PAC-Bayes框架推导泛化界。论文的核心贡献在于建立了两个不可约误差下界：
1. 表达性误差下界（Expressivity Floor）：语言作为通信信道具有容量限制（比如固定token长度、离散符号集）。当任务族的信息复杂度（如任务分布的熵）超过信道容量时，不同任务会被映射到相同的提示分布，导致LLM无法区分它们。这种模糊性造成的误差下界是正数，且无法通过收集更多数据、更好的优化或扩大模型容量来消除——因为问题本身源自信息传输的瓶颈。
2. 目标错配误差下界（Objective-Misalignment Floor）：对齐训练（如RLHF、安全约束）会限制LLM生成的输出范围（例如避免有害内容）。然而，用户理想的任务求解分布可能恰好位于限制区域之外（例如某些合法但敏感的科学问题），导致LLM即使有能力正确回答，也被迫输出偏离最优的结果。这种误差下界同样不可约，因为它源自训练目标与真实目标之间的结构冲突。

论文进一步讨论了这些下界在零样本和少样本学习中的表现：当通信信道完全揭示任务（即提示足以唯一确定任务）时，表达性下界消失，但目标错配下界仍存；当存在信息瓶颈或目标错配时，随着数据增加，性能趋于一个大于零的常数，而非无限逼近最优。
论文最后指出，这些限制本质上源于语言本身的压缩特性和对齐约束，而非模型容量或数据量。因此，突破这些限制可能需要超越纯自然语言的接口，例如结合多模态感知、外部记忆或工具使用，以增加系统可用的任务相关信息。本文为理解LLM的能力边界提供了严谨的理论基础，对于AI安全评估、任务设计和接口设计具有重要指导意义。**

<!-- paperflow:bb3c194f5d61e0dd -->
## Unlimited OCR Works

[[Deep Reading - Jun 2026/Unlimited OCR Works|Deep Reading]]

[https://arxiv.org/pdf/2606.23050](https://arxiv.org/pdf/2606.23050)

- **本文提出Unlimited OCR，一个专为长序列解析设计的端到端OCR模型，核心创新是Reference Sliding Window Attention (R-SWA)。R-SWA允许每个生成token同时关注所有参考token（视觉token和提示）以及固定数量的最近输出token（默认128），从而在解码过程中维持恒定大小的KV缓存，解决了传统全注意力随输出增长的内存和速度问题。模型以DeepSeek OCR为基线，保留其DeepEncoder和MoE架构，仅替换解码器注意力层。实验表明，R-SWA在OCR任务上相比基线提升6%，并能一次性转录数十页文档（32K上下文）。R-SWA是通用注意力机制，可应用于ASR、翻译等其他解析任务。论文公开了代码和模型权重。**

<!-- paperflow:f2b3aa06c4bdeea1 -->
## Evaluation Awareness Is Not One Capability: Evidence from Open Language Models

[[Deep Reading - Jun 2026/Evaluation Awareness Is Not One Capability-Evidence from Open Language Models|Deep Reading]]

[https://arxiv.org/pdf/2606.23583](https://arxiv.org/pdf/2606.23583)

- **本文挑战了安全基准测试的一个根本假设：测试条件下的行为是部署行为的可靠代理。作者通过系统实验证明了开放语言模型能够检测评估上下文，且这种检测会影响安全行为，但更重要的是，评估意识不是一个单一能力，而是由三个弱耦合的轴组成：潜在表征可检测性、行为检测能力和可控性。这些轴几乎不相关，因此基于单一指标（如拒绝率）的安全声明可能具有欺骗性，即“基准幻觉”。论文的实验覆盖37个模型，8个实验，揭示了检测能力中等（仅部分模型高于随机），行为影响显著（合规性变化可达30%），表征在行为控制下仍存留，且各轴间仅一个显著耦合。结论是，安全基准应被视为多维度指标，且不应仅依赖总体得分，而需分别报告各轴的测量结果。**

<!-- paperflow:43f18349db60d04d -->
## HOLMES: Evaluating Higher-Order Logical Reasoning in LLMs

[[Deep Reading - Jun 2026/HOLMES-Evaluating Higher-Order Logical Reasoning in LLMs|Deep Reading]]

[https://arxiv.org/pdf/2606.23238](https://arxiv.org/pdf/2606.23238)

- **论文提出 HOLMES，首个面向高阶逻辑推理的真实世界基准，填补了现有 LLM 推理评估在一阶逻辑之外的空白。基准包含 1379 个法律与金融领域实例，每个实例配有高阶逻辑形式化、标准答案、可验证推理轨迹及细粒度因素标签。评估 11 种主流 LLM，发现平均准确率仅 50.64%，最佳为 59.54%，且高答案准确率可能掩盖推理缺陷。进一步分析表明，模型在冲突解决、范围条件约束及组合推理上表现薄弱，确认高阶逻辑推理是构建可靠可验证 LLM 的关键瓶颈。论文公开了代码和数据集。**

<!-- paperflow:15b6ffd9a44b4a57 -->
## TriggerBench: Investigating Prospective Memory for Large Language Models

[[Deep Reading - Jun 2026/TriggerBench-Investigating Prospective Memory for Large Language Models|Deep Reading]]

[https://arxiv.org/pdf/2606.23459](https://arxiv.org/pdf/2606.23459)

- **本文提出TriggerBench，首个系统评估大语言模型前瞻记忆的基准。动机是现有评估忽略模型自发回忆并行动于隐式约束的能力。基准设计包括五个维度（日常助手和专业工作流），每个场景有匹配的回顾记忆控制和对比变体，以及过载触发。实验评估多个模型，发现：(i) 前瞻记忆存在精度-召回权衡和注意力脆弱性；(ii) 前瞻记忆显著难于回顾记忆，随上下文长度退化；(iii) 前瞻记忆可作为推理开销的细粒度探针。论文还讨论了局限和未来方向。**

<!-- paperflow:cb26fc782bd9d872 -->
## Beyond Penalizing Mistakes: Stabilizing Efficiency Training in Large Reasoning Models via Adaptive Correct-Only Rewards

[[Deep Reading - Jun 2026/Beyond Penalizing Mistakes-Stabilizing Efficiency Training in Large Reasoning Models via Adaptiv|Deep Reading]]

[https://arxiv.org/pdf/2606.22716](https://arxiv.org/pdf/2606.22716)

- **本文聚焦于大型推理模型在效率训练中的稳定性问题。作者识别出两种奖励崩溃模式：结构性崩溃（由对错误答案持续施加长度惩罚引发）和随机崩溃（由仅对正确答案施加长度惩罚导致的过度压缩）。为解决这些失效，提出ACOER，一种自适应正确-唯一效率奖励方法，包含三个机制：正确-唯一长度信号、动态预算归一化和控制环惩罚调整。在Qwen3-1.7B上的数学推理基准实验中，ACOER不仅避免了奖励崩溃，还提高了准确率，同时将token生成减少超过60%。工作强调了效率优化中稳定性设计的重要性，为后续高效推理训练提供了可靠基线。**

<!-- paperflow:8faf704cb014246d -->
## Same question, different history: language, national identity, and credit in large language models

[[Deep Reading - Jun 2026/Same question, different history-language, national identity, and credit in large language model|Deep Reading]]

[https://arxiv.org/pdf/2606.23164](https://arxiv.org/pdf/2606.23164)

- **本文系统性地展示了大型语言模型在面对历史发明归属争议时，其回答受到提问语言的显著影响。通过11个模型、21个争议、12种语言、75,896个响应的大规模实验，作者发现语言像开关一样激活不同的国家记忆：用特定国家语言提问时，该国声称的发明者更常被提及，但英语世界的主导人物在所有语言下都保持稳定。这一模式在控制回答长度、模型差异、历史显赫度和国家纪念后依然成立。研究将此解释为一种计算形式的“日常民族主义”——LLM并非中立地检索知识，而是通过语言区分方式塑造了集体记忆的可见性。论文的价值在于提供了首个大规模、多语言、多模型的实证证据，揭示了AI系统可能无意中强化了国家中心的历史叙述，对信息公平与跨文化理解具有重要意义。**

<!-- paperflow:44d6a519e82a01fc -->
## Do LLM Embedding Spaces Recover Expert Structure?

[[Deep Reading - Jun 2026/Do LLM Embedding Spaces Recover Expert Structure|Deep Reading]]

[https://arxiv.org/pdf/2606.23394](https://arxiv.org/pdf/2606.23394)

- **本文系统考察了LLM嵌入空间是否恢复专家定义的结构，以心理健康语言为例。作者指出，分类精度高并不等价于几何结构对齐，在线社区中的领域特异性信号（行话、风格、情感）可能主导嵌入空间，掩盖专家相关的结构。因此，论文引入外部专家参考——一个由临床症状类别组成的二进制关联矩阵——并通过表征相似性分析（RSA）、原型典型性和多层级混淆控制来评估对齐。

方法上，使用28个Reddit社区（14个心理健康相关，14个对照）的帖子，采用Qwen3-0.6B和Qwen3-4B两个模型，分别评估预训练和监督微调后的嵌入。RSA比较嵌入空间导出的表征不相似矩阵与症状矩阵的Spearman相关性；原型典型性计算每个专家类别的典型性和边界混淆；混淆控制逐步引入VAD情感、LIWC心理语言学特征、词汇风格统计和主题分布作为协变量。

主要发现包括：预训练嵌入在心理健康子集上与专家结构有统计显著但中等的对齐；监督微调显著增强对齐，尤其是在细粒度子类别水平；更大规模模型（4B）在零样本对齐和监督增益上均优于0.6B；控制所有混淆后残差对齐仍然显著，表明对齐部分独立于非临床信号；但零样本对齐仍有限，监督微调的增益部分依赖任务特定信号。

论文的贡献在于：1) 提出了评估嵌入几何对齐的通用框架（RSA+原型典型性+混淆控制）；2) 在心理健康领域实证揭示了预训练嵌入与专家结构的对齐及其条件；3) 强调了显式混淆测试的重要性，而非仅依赖分类准确率。局限包括：专家参考矩阵并非临床诊断本体，仅用于类别比较；零样本对齐仍适度，结果支持有限主张（预训练嵌入可恢复部分专家结构，但需监督才能更好捕捉）。总体而言，论文为评估LLM嵌入的语义结构提供了方法论，并对AI for Science（如心理健康领域的知识组织）具有启发意义。**

<!-- paperflow:e69364c9153d4c7b -->
## OPERA: Aligning Open-Ended Reasoning via Objective Perplexity-based Reinforcement Learning

[[Deep Reading - Jun 2026/OPERA-Aligning Open-Ended Reasoning via Objective Perplexity-based Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2606.25757v1](https://arxiv.org/pdf/2606.25757v1)

- **论文OPERA针对开放式推理任务（如创意写作）中强化学习对齐的挑战，提出了一种基于困惑度动态的内在奖励框架。传统RL方法依赖LLM-as-a-judge作为奖励模型，但存在风格偏差和位置不一致导致的监督不稳定问题。OPERA的核心创新是将奖励信号从外部判官转移到模型自身的困惑度动态上，具体通过量化关键反思状态的不确定性降低来产生内在奖励。为实现冷启动，论文设计了Perplexity-Guided Iterative Trace Synthesis方法：利用认知制动触发模型进行System 2深层推理，并通过引导词生成多样化推理轨迹；同时采用困惑度优先的rollouts，基于内部log-probability筛选逻辑一致的推理分支，最终构建了20,000条高质量推理轨迹的专用数据集。基于该数据集使用RL训练，优化目标为最大化内在奖励。实验在Qwen3-8B上取得了开源模型最佳性能，在部分开放式任务中匹敌甚至超越Gemini2.5和MiniMax-M2.5等专有模型。控制实验表明，OPERA在冷启动数据选择和RL对齐阶段均优于LLM-as-a-judge基线，训练更稳定。论文本质上是将对齐的优化目标从外部评判的质量转移到推理结构的逻辑一致性，为开放式任务的RL对齐提供了新范式。**

<!-- paperflow:b525af1c60bdf7f7 -->
## MiniOpt: Reasoning to Model and Solve General Optimization Problems with Limited Resources

[[Deep Reading - Jun 2026/MiniOpt-Reasoning to Model and Solve General Optimization Problems with Limited Resources|Deep Reading]]

[https://arxiv.org/pdf/2606.25832v1](https://arxiv.org/pdf/2606.25832v1)

- **To address these challenges, we propose MiniOpt, a reinforcement learning framework that learns to solve optimization problems through an "reasoning-to-model-and-solve" paradigm.

Ke Zhao $^{*}$ , Zixiang Di $^{*}$ , Hong Qian, Xiang Shu, Yaolin Wen, Qitao Shi, Bingdong Li, Xingyu Lu, Xiangfeng Wang, Jun Zhou, Ke Tang, and Yang Yu Abstract—Achieving strong… Existing approaches typically rely on large-scale supervised datasets, costly reasoning annotations, and expensive intermediate step verification, resulting in substantial…

This paper proposes MiniOpt, a novel reasoning-to-model-and-solve paradigm and the corresponding small-scale models MiniOpt-3B and MiniOpt-7B. The proposed models achieve competitive or even superior performance under limited computational resources.

We evaluate MiniOpt models on diverse optimization benchmarks spanning multiple types and scenarios to assess whet...**

<!-- paperflow:d65923758b783561 -->
## Training for the Model You Return: Improving Optimization for Iterate-Averaged Language Models

[[Deep Reading - Jun 2026/Training for the Model You Return-Improving Optimization for Iterate-Averaged Language Models|Deep Reading]]

[https://arxiv.org/pdf/2606.25086v1](https://arxiv.org/pdf/2606.25086v1)

- **本文针对语言模型训练中普遍存在的返回平均模型（如EMA）现象，提出了一个根本性的优化问题：既然我们最终使用平均权重，为何不直接针对这个目标优化训练过程？作者从最优控制的视角出发，在连续时间随机二次模型的假设下，利用庞特里亚金最大值原理推导出最优控制策略，该策略会将当前权重向EMA方向拉回。该策略的实用近似被命名为PACE（Push And Control EMA），它作为AdamW的轻量包装，引入三个超参数：拉回强度c、EMA衰减指数κ和更新频率uf。理论方面，PACE在标准随机凸优化设置下与原始算法具有相同的收敛阶（仅差一个常数因子），且在二次损失下可以任意程度地降低平均估计的渐近均方误差。实验方面，论文在1-2B参数语言模型的监督微调以及GPT-2在FineWeb上的预训练中进行了广泛的评估。实验设计包括与AdamW、AdamW+EMA以及Schedule-Free的对比，并对学习率、衰减调度、PACE超参数进行了系统扫描。结果表明，PACE在几乎所有设置下均优于基线，且对超参数鲁棒。与Schedule-Free相比，PACE性能相当但能兼容学习率衰减。论文还通过合成二次实例验证了理论的严格改善。总体而言，PACE提供了一个简单、理论上可解释且实验验证有效的改进，可直接集成到现有LM训练流程中。**

<!-- paperflow:35f379821b481d7b -->
## Evaluating LLMs on Real-World Software Performance Optimization

[[Deep Reading - Jun 2026/Evaluating LLMs on Real-World Software Performance Optimization|Deep Reading]]

[https://arxiv.org/pdf/2606.25530v1](https://arxiv.org/pdf/2606.25530v1)

- **本论文针对当前缺乏真实世界软件性能优化基准的问题，提出了SWE-Pro——一个仓库级基准，包含102个人工专家从开源项目中编写的优化任务。每个任务配以参数化性能测试，在噪声感知测量条件下综合评估运行时、峰值内存和TWMU。通过在多个SOTA LLM上的广泛实验，作者发现：1) LLM的运行时优化效果微乎其微，甚至在Oracle检索下性能平均下降0.69×；2) 内存优化几乎不存在，且经常导致内存增加；3) 专家优化平均实现15.5×加速和171.3×内存降低，在91.2%和65.7%的任务上有改进。论文还提出使用90% RCIW收敛准则过滤系统方差，成功复现87%的专家改进。这项工作首次系统性地揭示了当前LLM在仓库级多维性能优化上的巨大不足，为未来研究提供了扎实的评估平台与明确挑战。**

<!-- paperflow:d8e8ca8a1f61754d -->
## Weave of Formal Thought

[[Deep Reading - Jun 2026/Weave of Formal Thought|Deep Reading]]

[https://arxiv.org/pdf/2606.25987v1](https://arxiv.org/pdf/2606.25987v1)

- **论文《Weave of Formal Thought》提出了一种名为WoFT的范式，旨在统一代码生成中的严格语法验证与学习到的结构表示。首先，论文指出现有大型语言模型生成代码虽表面流畅，但缺乏形式语法保证，现有约束解码框架在处理上下文相关词法（如Python缩进）、最大匹配分词和关键字提取方面存在局限，且仅近似屏蔽无效子词。其次，论文方案包含两个核心组件：约束解码器和潜在变量微调。约束解码器基于GLR解析与推测性词法分析，维护同步的并发词法器状态假设，确保只接受能扩展为有效程序前缀的子词，实现完备性。潜在变量微调方法使用重加权醒睡(RWS)算法优化重要性加权证据下界，使模型在生成过程中学习插入非终结符作为结构草稿本。实验部分在Python上对StarCoder2-3B进行微调，per-token交叉熵相对降低14.3%，但未报告功能正确性基准结果。论文最后讨论了未来方向，如标准评估、端到端微分句法推理和扩展至更多语言。总体而言，WoFT提出了一种神经符号融合的新思路，在语法验证和结构学习方面均有贡献，但实验证据尚局限于语言建模损失。**

<!-- paperflow:2e1f9a3efb81df83 -->
## Improved Large Language Diffusion Models

[[Deep Reading - Jun 2026/Improved Large Language Diffusion Models|Deep Reading]]

[https://arxiv.org/pdf/2606.25331v1](https://arxiv.org/pdf/2606.25331v1)

- **论文iLLaDA（Improved Large Language Diffusion Models）旨在探索完全双向注意力扩散语言模型的潜力。论证主线：现有自回归LLM占据主导，但双向扩散预训练在数据利用上有理论优势，且LLaDA初步验证了可行性；iLLaDA通过大规模预训练（12T token）和改进的训练配方，证明双向扩散可以成为一条有竞争力的路径。技术主线：模型采用masked diffusion目标，全程（预训练+SFT）保持，使用全双向注意力，并引入变长生成和置信度评分。实验主线：在通用、数学、代码基准上与LLaDA和Qwen2.5 7B对比，iLLaDA在多数任务上大幅超越LLaDA，并与自回归模型持平或更好。结论：双向扩散训练是一种有前途的替代方案，值得进一步研究。**

# AI Agents

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Agents
- 方法：agent, language, vision-language-model, reasoning, vision, reinforcement-learning, optimization, retrieval
- 论文/报告：38 篇
- DEEPRUBRIC: Evidence-Tree Rubric Supervision for Efficient Reinforcement Learning of Deep Research Agents
- Gen-VCoT: Generative Visual Chain-of-Thought Reasoning via Diffusion-Based RGB Intermediate Representations
- Connect the Dots: Training LLMs for Long-Lifecycle Agents with Cross-Domain Generalization Via Reinforcement Learning
- LedgerAgent: Structured State for Policy-Adherent Tool-Calling Agents
- ScaffoldAgent: Utility-Guided Dynamic Outline Optimization for Open-Ended Deep Research
- ScholarQuest: A Taxonomy-Guided Benchmark for Agentic Academic Paper Search in Open Literature Environments
- Probe-and-Refine Tuning of Repository Guidance for Coding Agents
- Marginal Advantage Accumulation for Memory-Driven Agent Self-Evolution
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:6a9c36ab1211db1d -->
## DEEPRUBRIC: Evidence-Tree Rubric Supervision for Efficient Reinforcement Learning of Deep Research Agents

[[Deep Reading - Jun 2026/DEEPRUBRIC-Evidence-Tree Rubric Supervision for Efficient Reinforcement Learning of Deep Researc|Deep Reading]]

[https://arxiv.org/pdf/2606.17029v1](https://arxiv.org/pdf/2606.17029v1)

- **本文提出DeepRubric框架，旨在提高深度研究智能体强化学习中rubric监督的可靠性。现有方法通常先给定查询再由LLM生成rubric，但模型可能遗漏信息需求。DeepRubric逆转为“证据优先”模式：系统地从种子主题开始，通过递归地搜索并分解为基于证据的子问题，构建出一棵证据树。树的叶子节点构成原子化、可验证的评估目标。然后利用该树合成训练查询和对应的rubric，保证每个rubric都精确覆盖查询所需的信息。基于此方法构建了9000个高质量的查询-rubric训练对，并使用rubric-based GRPO算法训练了一个8B参数模型（DeepRubric-8B）。在三个未被明确命名的深度研究报告评估基准上，DeepRubric-8B取得了与先前开源SOTA相近的性能，但所需的RL训练GPU时间仅为后者的约1/13（即减少约13倍）。该工作展示了证据优先构造在提升RL效率和rubric质量方面的巨大潜力。**

<!-- paperflow:4fd663df23df2251 -->
## Gen-VCoT: Generative Visual Chain-of-Thought Reasoning via Diffusion-Based RGB Intermediate Representations

[[Deep Reading - Jun 2026/Gen-VCoT-Generative Visual Chain-of-Thought Reasoning via Diffusion-Based RGB Intermediate Repre|Deep Reading]]

[https://arxiv.org/pdf/2606.16783v1](https://arxiv.org/pdf/2606.16783v1)

- **本文提出Gen-VCoT，一种生成式视觉链式推理框架，利用预训练专家模型（SAM和Marigold）产生RGB图像（分割图和深度图）作为可解释的中间表示。该方法将视觉推理分解为三个可解释阶段：视觉定位（Where）、几何推理（How）和语义推理（What），从而在不依赖外部工具的情况下实现了端到端可训练性和密集像素级表示。在复杂空间推理任务上，Gen-VCoT显著优于直接MLLM推理（78.9% vs 68.4%），但在简单事实查询上性能下降（62.5% vs 85.0%），揭示了中间表示的最优性与任务密切相关。这一发现表明，未来需要根据任务特性动态选择或组合中间表示。整体上，Gen-VCoT为可解释的多模态推理提供了新范式，但存在计算开销较大和对简单任务适用性有限的局限。**

<!-- paperflow:262217e4a57d9e49 -->
## Connect the Dots: Training LLMs for Long-Lifecycle Agents with Cross-Domain Generalization Via Reinforcement Learning

[[Deep Reading - Jun 2026/Connect the Dots-Training LLMs for Long-Lifecycle Agents with Cross-Domain Generalization Via Re|Deep Reading]]

[https://arxiv.org/pdf/2606.20002v1](https://arxiv.org/pdf/2606.20002v1)

- **本文提出Connect the Dots (CoD)框架，旨在训练LLM获得长期部署agent所需的元能力：在环境中通过探索和自主学习更新上下文，从而在后续任务中表现更好。框架包括GRPO风格的端到端RL算法（含细粒度信用分配）和专门的任务环境。以Qwen3-8B-Instruct为基座进行实验，验证了端到端RL的有效性，并展示了训练域内、跨域和向Ralph-loop设置的分布外泛化。本文连接了agent RL、上下文学习和持续学习等方向，并为未来研究提供了基础。**

<!-- paperflow:0983523f81899f60 -->
## LedgerAgent: Structured State for Policy-Adherent Tool-Calling Agents

[[Deep Reading - Jun 2026/LedgerAgent-Structured State for Policy-Adherent Tool-Calling Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.20529v1](https://arxiv.org/pdf/2606.20529v1)

- **论文针对客服领域策略依从工具调用智能体中的状态隐式管理问题，提出LedgerAgent。其核心思想是将任务状态显式存储于结构化分账本中，每次决策前渲染到提示，并通过策略门在执行环境改变操作前检查策略合规性。该方法不修改模型参数，属于推理时增强。在四个客服领域、多种开源/闭源模型上的实验显示，相比标准基于提示的方法，LedgerAgent在pass^k和更严格的多轮一致性指标上均有提升。论文的主要贡献在于指出了状态接地失败和策略违规两个关键问题，并提供了简单、确定性、与模型无关的解决方案。局限性包括：依赖结构化工具返回、需要预定义领域模式，以及增加提示长度带来的计算开销。这项工作与现有推理时支架方法正交，可结合使用。**

<!-- paperflow:757e3cddf0a58711 -->
## ScaffoldAgent: Utility-Guided Dynamic Outline Optimization for Open-Ended Deep Research

[[Deep Reading - Jun 2026/ScaffoldAgent-Utility-Guided Dynamic Outline Optimization for Open-Ended Deep Research|Deep Reading]]

[https://arxiv.org/pdf/2606.20122v1](https://arxiv.org/pdf/2606.20122v1)

- **本文提出 ScaffoldAgent，一个效用引导的动态大纲优化框架，用于开放式深度研究。该框架将大纲演化建模为包含扩展、收缩和修订三种操作的结构化决策过程，并引入效用引导的反馈机制，从检索增益、结构连贯性和试生成质量估计每个大纲操作的下游价值，从而指导节点选择、操作调度和推理终止。实验在 DeepResearch Bench 和 DeepResearch Gym 上验证，表明 ScaffoldAgent 一致性地改进了长报告生成和事实基础。论文贡献在于提出了一种统一效用目标下的动态大纲优化方法，解决了现有方法中的支架漂移和反馈延迟问题。**

<!-- paperflow:0ef8054d77359d8c -->
## ScholarQuest: A Taxonomy-Guided Benchmark for Agentic Academic Paper Search in Open Literature Environments

[[Deep Reading - Jun 2026/ScholarQuest-A Taxonomy-Guided Benchmark for Agentic Academic Paper Search in Open Literature En|Deep Reading]]

[https://arxiv.org/pdf/2606.20235v1](https://arxiv.org/pdf/2606.20235v1)

- **论文提出 ScholarQuest，一个基于分类法的大规模基准，旨在系统评估智能体学术论文搜索。基准覆盖 1000+ CS 主题和四种研究意图，通过可扩展管道构建答案集和共享检索后端确保可复现。实验对比了现有智能体方法与单次检索基线，发现智能体方法虽整体更优但绝对性能仍低，揭示意图保持、多轮推理等挑战。论文还分析了不同意图下的搜索效率和失败模式，为未来智能体搜索研究提供多维度评估信号和开放资源。**

<!-- paperflow:cafad5da22fd34fb -->
## Probe-and-Refine Tuning of Repository Guidance for Coding Agents

[[Deep Reading - Jun 2026/Probe-and-Refine Tuning of Repository Guidance for Coding Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.20512v1](https://arxiv.org/pdf/2606.20512v1)

- **论文研究了编码代理的仓库指导生成问题。现有AGENTS.md文件效果存在争议，作者提出指导的生成方式是决定变量，并设计了probe-and-refine tuning方法。该方法利用合成bug探针和LLM的自我诊断能力，迭代优化指导文件，无需代理循环。在SWE-bench Verified上的四个独立试验中，该方法将Qwen代理的resolve率从25.5%提升至33.0%，主要归因于覆盖率提升（+14.5 pp）而非精确度变化。步数预算实验表明指导使代理能有效扩展搜索；跨模型实验显示方法对模型能力敏感。论文强调指导质量是第一阶变量，与模型选择同等重要。局限性包括仅测试两个模型、步数不足时可能有害、未探索表示层面机制。作者指出后续需研究指导如何影响代理的内部探索过程。**

<!-- paperflow:a269ea6019044178 -->
## Marginal Advantage Accumulation for Memory-Driven Agent Self-Evolution

[[Deep Reading - Jun 2026/Marginal Advantage Accumulation for Memory-Driven Agent Self-Evolution|Deep Reading]]

[https://arxiv.org/pdf/2606.20475v1](https://arxiv.org/pdf/2606.20475v1)

- **本文针对批量式痕迹蒸馏中同一操作跨批次信号矛盾的问题，提出边际优势累积（MAA）方法。MAA形式化了可对齐性和可比性两个结构条件，通过差分信号构建、EMA有符号证据累积和语义身份合并实现跨批次操作级证据的稳定积累。作为后处理架构，MAA不修改训练流程，可灵活集成。在4个benchmark和4个模型上的16个设置中，MAA取得14个最佳结果，显著优于现有批量式基线（如SkillOpt），并在大多数设置中匹配或超越在线方法（如ReACT），同时优化阶段token消耗减少约75%。消融实验证实差分构造和连续幅度的重要性。学习曲线分析显示MAA在epoch 3-4后开始领先，收敛速度受任务复杂度影响。案例研究表明MAA能可靠区分稳定有效操作（长期正信号）和偶然命中（交替信号）。**

<!-- paperflow:cdae066cf50363fd -->
## Beyond Static Leaderboards: Predictive Validity for the Evaluation of LLM Agents

[[Deep Reading - Jun 2026/Beyond Static Leaderboards-Predictive Validity for the Evaluation of LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.19704v1](https://arxiv.org/pdf/2606.19704v1)

- **本文针对LLM智能体评估中静态排行榜的局限性，提出了预测效度（predictive validity）作为替代排名准则。通过聚合14个并行实现研究（约6000条判断轨迹），扩展了一个基于MCP的工业基准，覆盖六个扩展轴，并构建了12层测量装置，揭示了现有聚合分数排名在分布外场景中的不稳定性。论文还明确了三个可证伪的分布外标准，但实证验证被规划为未来工作。最后给出了预注册试点设计和下一代基准愿景。**

<!-- paperflow:1bde86ec86b5728f -->
## VideoAgent: All-in-One Framework for Video Understanding and Editing

[[Deep Reading - Jun 2026/VideoAgent-All-in-One Framework for Video Understanding and Editing|Deep Reading]]

[https://arxiv.org/pdf/2606.23327](https://arxiv.org/pdf/2606.23327)

- **本文提出VideoAgent，一个全能智能体框架，将视频理解与编辑整合为端到端流水线。其核心创新包括：1）自动镜头创建，通过剧本规划生成故事线并跨模态检索匹配视觉内容，解决长视频连贯性问题；2）多智能体编排系统，集成30多个编辑智能体并利用文本梯度图优化自适应组装复杂工作流。在自建VideoEdit基准和公开数据集上的实验表明，VideoAgent在编排成功率（87-95%）和成本效率（降低60%）上显著超越现有多模态LLM和智能体系统。人类评价显示其输出质量接近专业人类编辑（评分仅低4%）。论文还讨论了局限性，如对源素材质量的依赖和潜在滥用风险，并提出了未来改进方向。**

<!-- paperflow:fb365bbbdbdd6d2a -->
## ReasoningLens: Hierarchical Visualization and Diagnostic Auditing for Large Reasoning Models

[[Deep Reading - Jun 2026/ReasoningLens-Hierarchical Visualization and Diagnostic Auditing for Large Reasoning Models|Deep Reading]]

[https://arxiv.org/pdf/2606.23404](https://arxiv.org/pdf/2606.23404)

- **本文提出 ReasoningLens，一个为大型推理模型（LRM）长思维链设计的开源诊断框架。论文首先指出现有 LRM 的长 CoT 造成透明度危机：关键推理步骤被程序化文本淹没，现有可视化多为启发式概览。ReasoningLens 通过三部分应对：分层推理图（分离高层策略与底层执行）、智能体审计器（自动化错误检测与工具验证）、系统性推理画像（跨轨迹建模模型盲点）。定性案例研究展示了框架在错误定位、诊断反馈和盲点识别方面的实用性。论文主要贡献包括形式化多粒度诊断框架、开源工具实现以及对新型诊断范式的推动。局限在于当前主要针对静态 CoT，未覆盖动态智能体场景，且评估以定性为主。总体而言，该工作为推理模型的可解释性与调试提供了系统化、可操作的工具基础。**

<!-- paperflow:a3580cca32adf90b -->
## Group-Graph Policy Optimization for Long-Horizon Agentic Reinforcement Learning

[[Deep Reading - Jun 2026/Group-Graph Policy Optimization for Long-Horizon Agentic Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2606.22995](https://arxiv.org/pdf/2606.22995)

- **本文针对长程智能体强化学习中奖励稀疏和信用分配困难的问题，提出G2PO算法。G2PO将多条交互轨迹中的状态聚合为全局有向图，节点表示状态，边表示动作。在此基础上，使用组聚合方法估计状态值（多个轨迹的均值），降低方差；并定义边优势（全局标准化TD误差），实现细粒度信用分配，突出关键动作。在WebShop、ALFWorld、AppWorld三个长程基准上，G2PO显著优于GRPO等基线，最高提升22.2%成功率。分析表明，图结构有效缓解了长程任务中的信用分配困境。代码已开源。**

<!-- paperflow:9935e52d09a6e0bf -->
## Superhuman AI for Generals.io Using Self-Play Reinforcement Learning

[[Deep Reading - Jun 2026/Superhuman AI for Generals.io Using Self-Play Reinforcement Learning|Deep Reading]]

[https://arxiv.org/pdf/2606.23348](https://arxiv.org/pdf/2606.23348)

- **本文提出了一个用于Generals.io的超人类AI智能体，通过构建JAX原生模拟器（速度提升10000倍），结合视觉变换器策略网络和简单的策略梯度循环（稀疏奖励、顶部优势过滤、EMA），在4天内训练达到天梯第一名。消融实验表明，常见的RL技巧如行为克隆、奖励塑形和种群自对弈在这个尺度下并非必需。智能体在天梯、头对头人类系列赛和强bot对抗中均取得压倒性胜利。研究强调了模拟器速度对于RL研究的重要性，并提供了一个极简且高效的范本。**

<!-- paperflow:ac07884af7f35c7d -->
## MAS-PromptBench: When Does Prompt Optimization Improve Multi-Agent LLM Systems?

[[Deep Reading - Jun 2026/MAS-PromptBench-When Does Prompt Optimization Improve Multi-Agent LLM Systems|Deep Reading]]

[https://arxiv.org/pdf/2606.23664](https://arxiv.org/pdf/2606.23664)

- **本文系统研究了多智能体LLM系统中系统提示优化的效果与条件。作者首先指出，在单智能体提示优化取得成功后，将其扩展到MAS面临搜索空间庞大、交互复杂等挑战。为此，他们构建了MAS-PromptBench基准，涵盖多种任务、工作流拓扑、通信协议和团队规模，并适配了两种优化器。实验表明，提示优化能显著提升MAS性能（尤其推理任务、顺序工作流、较小团队规模），但收益高度依赖配置，且存在优化失效或负收益的情况。研究还揭示了优化成本、跨模型迁移性差、协调冲突等问题。论文的主要贡献在于提供了一套系统评估框架，首次量化了提示优化在MAS中的潜力与局限，为未来研究指明了方向。**

<!-- paperflow:b01a10644a79f8a0 -->
## AOHP: An Open-Source OS-Level Agent Harness for Personalized, Efficient and Secure Interaction

[[Deep Reading - Jun 2026/AOHP-An Open-Source OS-Level Agent Harness for Personalized, Efficient and Secure Interaction|Deep Reading]]

[https://arxiv.org/pdf/2606.23449](https://arxiv.org/pdf/2606.23449)

- **AOHP（Android Open Harness Project）是一个基于Android开源项目构建的OS级智能体框架，旨在解决现有操作系统缺乏对AI智能体原生支持的问题。论文首先指出，当前移动OS以应用为中心，智能体通过GUI、API、CLI等方式运行时面临高视觉开销、序列执行瓶颈、脆弱跨应用切换和安全漏洞。为此，AOHP将智能体视为操作系统一等公民，在OS内核和框架层引入三个核心机制：个性化服务组合（智能体自适应合成服务入口）、高效智能体接口（暴露执行环境、UI语义、存储和事件原语，支持虚拟显示器实现后台并行）和安全信息流（原生沙箱运行时和策略执行）。该系统保持与Android生态兼容，并作为开放测试床供研究社区使用。初步实验在覆盖关键OS智能体能力的挑战性任务上进行，结果显示：与基线相比，任务完成率提升21.12%，token成本降低51.55%，安全策略违规显著减少。论文的主要贡献是提出了智能体原生OS的设计蓝图，提供了可复现的实现，并通过实证展示了其优势。局限包括：目前仅支持Android，个性化组合在极端多样化场景下可能不稳定，安全策略尚有细化空间。未来工作可拓展至更多OS、优化资源利用、进行用户研究和跨平台迁移。**

<!-- paperflow:6adb6b1e349ed2c6 -->
## Towards Root Memories: Benchmarking and Enhancing Implicit Logical Memory Retrieval for Personalized LLMs

[[Deep Reading - Jun 2026/Towards Root Memories-Benchmarking and Enhancing Implicit Logical Memory Retrieval for Personali|Deep Reading]]

[https://arxiv.org/pdf/2606.23283](https://arxiv.org/pdf/2606.23283)

- **本文首先指出个性化LLM记忆系统的一个关键缺陷：现有检索方法依赖语义相似性，无法捕获隐式逻辑记忆。为系统研究此问题，作者构建了IMLogic基准，包含长对话中精心设计的隐式逻辑记忆检索任务。然后，他们提出根记忆表示，将用户历史中的决策逻辑蒸馏为结构化单元，并设计RootMem框架，通过LLM路由器实现逻辑记忆的补充检索。实验证明该方法在准确性和兼容性上均优于现有方案。主要贡献包括新基准、新表示和新框架。**

<!-- paperflow:f3f4369b4aa11fb2 -->
## VeriEvol: Scaling Multimodal Mathematical Reasoning via Verifiable Evol-Instruct

[[Deep Reading - Jun 2026/VeriEvol-Scaling Multimodal Mathematical Reasoning via Verifiable Evol-Instruct|Deep Reading]]

[https://arxiv.org/pdf/2606.23543](https://arxiv.org/pdf/2606.23543)

- **VERIEVOL 提出了一个可验证的多模态数学推理数据构建框架，核心思想是将数据缩放问题解耦为提示难度扩展和答案可靠性验证两个独立阶段。类型感知演化模块通过路线特定算子提升提示难度，HTV-AGENT 验证器通过多方反证确保答案正确性。在 Qwen2.5-VL-7B-Instruct 学生模型上的实验表明，该框架在 130K RL 数据上比未演化基线提升 +3.88 个点，且 SFT 数据缩放从 10K 到 250K 带来近 20 个点的增益。论文强调可组合性：数据阶段验证与策略侧优化正交，可通过添加演化路线或验证器通道持续扩展。所有产出均已开源，便于后续研究复现和审计。**

<!-- paperflow:9f7ee40068e34f7a -->
## Plans Don't Persist: Why Context Management Is Load Bearing for LLM Agents

[[Deep Reading - Jun 2026/Plans Don't Persist-Why Context Management Is Load Bearing for LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.22953](https://arxiv.org/pdf/2606.22953)

- **本文诊断并验证 LLM 智能体中计划的持久性假设。作者提出 replay pairing 方法，通过对比有无计划历史轨迹中每步隐藏状态的余弦距离，直接测量计划对模型内部表示的影响。实验表明，在 Llama-3.1-70B 上计划信号在一步内衰减 4-12 倍，证明计划未作为持久状态存储而仅依赖上下文出现。针对推理模型（如 DeepSeek-R1）的 <think> 痕迹引入推理痕迹混淆，严格剥离可恢复信号。进一步，压缩压力测试显示简单丢弃计划导致 ALFWorld 成功率下降 34.7 个百分点，但探针门控重新显示未能完全恢复，说明上下文管理是“承重”但计划保护本身不够。主要贡献包括：1) 验证计划是上下文时间对象；2) 提供可复现的诊断框架；3) 揭示推理痕迹混淆；4) 量化压缩的实际代价。该工作为智能体上下文安全设计提供了关键测量基础。**

<!-- paperflow:41f5b7b987fa321a -->
## GroundEval: A Deterministic Replacement for LLM-as-Judge in Stateful Agent Evaluation

[[Deep Reading - Jun 2026/GroundEval-A Deterministic Replacement for LLM-as-Judge in Stateful Agent Evaluation|Deep Reading]]

[https://arxiv.org/pdf/2606.22737](https://arxiv.org/pdf/2606.22737)

- **论文提出 GROUNDEVAL，一个无评判的智能体评估框架，用于检测智能体是否在思考过程中使用了正确的证据。核心思想：通过记录智能体的工具调用、引用和叙述轨迹，结合事件日志、工件语料库和访问策略，生成三类测试（Silence、Perspective、Counterfactual），每个问题产生结构化诊断。案例研究显示，两个 LLM-as-judge 对同一个无效推理给出高分，而 GROUNDEVAL 正确识别了证据缺失。文章批评了仅依赖最终答案或 judge 评分的评估方式，并展示了 GROUNDEVAL 作为确定性替代方案的可行性。**

<!-- paperflow:033ecc699006ee7f -->
## Self-Compacting Language Model Agents

[[Deep Reading - Jun 2026/Self-Compacting Language Model Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.23525](https://arxiv.org/pdf/2606.23525)

- **本文提出SelfCompact，一种让语言模型智能体自主决定何时及如何压缩轨迹的脚手架。它由内联压缩工具和轻量级规则组成，无需微调或外部监督。在6个基准和7个模型上的实验表明，SelfCompact匹配或超越固定间隔压缩，同时大大降低令牌成本。消融实验揭示了元认知差距：模型自身难以判断上下文是否过时，而规则有效弥补了这一差距。**

<!-- paperflow:a0d0321b61e4e4d8 -->
## Managing Procedural Memory in LLM Agents: Control, Adaptation, and Evaluation

[[Deep Reading - Jun 2026/Managing Procedural Memory in LLM Agents-Control, Adaptation, and Evaluation|Deep Reading]]

[https://arxiv.org/pdf/2606.23127](https://arxiv.org/pdf/2606.23127)

- **本文针对LLM代理中程序性记忆的可迁移性评估问题，提出了AFTER基准。该基准包含382个真实企业任务、6个专业角色和22个程序技能，并通过局部改进、跨任务、跨角色和跨模型四种设置系统评估技能迁移性能。实验发现：单次精炼带来3.7-6.7点提升；多模型轨迹源在跨模型测试中达到73.1%准确率，优于单模型源；技能泛化能力呈现分化——部分技能广泛适用，部分高度专用化。这些发现为构建、评估和部署程序性记忆系统提供了实践指南。论文的贡献在于提供了一个标准化的评估框架、揭示了技能迁移的关键特性和影响因素。**

<!-- paperflow:185599d4ea0d6882 -->
## AIR: Adaptive Interleaved Reasoning with Code in MLLMs

[[Deep Reading - Jun 2026/AIR-Adaptive Interleaved Reasoning with Code in MLLMs|Deep Reading]]

[https://arxiv.org/pdf/2606.23678](https://arxiv.org/pdf/2606.23678)

- **本文提出AIR（Adaptive Interleaved Reasoning）框架，旨在增强多模态大语言模型的自主交错推理能力，特别是针对复杂数值计算任务。受OpenAI o3范式启发，现有工作仅关注视觉感知中的工具使用，无法处理数值计算。AIR通过强化学习训练实现自适应代码调用。方法包含三个组件：冷启动数据构建管线（两阶段生成初始推理数据）、RL数据集的过滤策略、以及利用组约束奖励函数的自适应工具调用策略。训练采用两阶段范式：先SFT冷启动，再RL强化。在多个基准上评估，组约束奖励函数使性能平均提升6.1pp，交错推理样本准确率提升9.9pp，工具使用成功率超95%。案例分析展示了成功和失败实例，表明模型在复杂计算中能有效调用代码，但仍存在挑战。论文贡献包括：提出AIR框架、实现自适应交错推理、设计组约束奖励、提供数据构建和过滤方法。限制在于：失败案例未深入分析，无跨领域验证。这项工作为MLLM的数值计算推理提供了系统方案。**

<!-- paperflow:1640f9b41387e1a1 -->
## Training Open Models for Agentic Phone Use

[[Deep Reading - Jun 2026/Training Open Models for Agentic Phone Use|Deep Reading]]

[https://arxiv.org/pdf/2606.23049](https://arxiv.org/pdf/2606.23049)

- **本文提出了PhoneBuddy，一套针对开放模型进行agentic phone use的训练方案和模型系列。核心贡献在于创新性地结合了真实应用环境和模拟应用环境PhoneWorld（基于真实GUI使用结构构建的可运行模拟应用）。训练流程分为两个阶段：首先在两种环境中收集轨迹进行共享监督微调（SFT），然后进行强化学习（RL）优化。通过对比SFT、SFT+真实环境RL、SFT+混合RL，论文系统展示了模拟环境训练并非真实环境RL的替代品，而是互补的、可扩展的信号来源。实验在150个真实手机人类评估任务和AndroidWorld基准上进行，结果显示混合RL显著优于仅真实环境RL和仅SFT，且增益主要来自于应用和小应用任务。跨应用工作流仍是开放挑战。论文还讨论了训练问题与运行时编排、隐私、安全等部署问题的分离，为后续工作提供了清晰边界。**

<!-- paperflow:51d1c815f03c2b8d -->
## EnterpriseClawBench: Benchmarking Agents from Real Workplace Sessions

[[Deep Reading - Jun 2026/EnterpriseClawBench-Benchmarking Agents from Real Workplace Sessions|Deep Reading]]

[https://arxiv.org/pdf/2606.23654](https://arxiv.org/pdf/2606.23654)

- **论文提出了EnterpriseClawBench，一个从真实企业代理会话自动构建的基准测试。包含852个可复现任务，每个任务有fixtures、提示、角色类、技能子类、硬规则和语义评价。由于数据敏感性，论文不发布数据，而是发布构建和评估协议。实验在120任务子集上评测了32种harness-model组合，最佳得分0.663。论文强调企业代理评估应报告多维指标，而非单一分数。论文还讨论了技能迁移、成本、运行时等因素。**

<!-- paperflow:8dc2498e813e9def -->
## Self-Evolution for Multi-Turn Tool-Calling Agents via Divergence-Point Preference Learning

[[Deep Reading - Jun 2026/Self-Evolution for Multi-Turn Tool-Calling Agents via Divergence-Point Preference Learning|Deep Reading]]

[https://arxiv.org/pdf/2606.23112](https://arxiv.org/pdf/2606.23112)

- **本文提出了一种结合图引导推理（ToolGraph）和发散点偏好学习（DPO）的方法，用于多轮工具调用智能体的基准内自改进。论文首先指出现有多轮工具调用方法将推理时编排与参数级学习分离，导致工具选择缺乏结构且偏好更新受提示不匹配影响。为解决该问题，论文构建了 ToolGraph：基于领域工具模式构建有向图，边权重从成功 rollout 中统计，并在推理时使用历史感知波束搜索，处理写前提和循环检测。然后，在相同 ToolGraph 上下文中，通过状态匹配和前缀对齐定位轨迹发散点，根据后续动作正确性构建 161 个偏好对，训练 DPO 模型（基于 Qwen3.5-9B）。实验在 tau2-bench 的四个领域（航空、电信、零售、银行）共 375 个任务上进行。结果表明：ToolGraph 单独使用将加权平均奖励从基线 0.304 提升至 0.338（+11.2%），而 ToolGraph+DPO 进一步达到 0.355（+16.8%）。DPO 增益主要集中于航空和零售领域。细粒度诊断显示电信领域约一半轨迹在动作执行前耗尽预算，且 chosen reward positivity 是最有效的 DPO 检查点信号。论文的工作量较大，系统性结合了推理时引导与参数微调，且验证了自改进的可行性。主要贡献包括：提出 ToolGraph 图引导推理框架、设计发散点偏好构造方法、在 tau2-bench 上实证了自改进效果。局限在于 DPO 增益领域不均、电信预算问题未解决、仅评估单一 9B 模型。**

<!-- paperflow:32c5f7c9ba004237 -->
## Tmax: A simple recipe for terminal agents

[[Deep Reading - Jun 2026/Tmax-A simple recipe for terminal agents|Deep Reading]]

[https://arxiv.org/pdf/2606.23321](https://arxiv.org/pdf/2606.23321)

- **本文提出Tmax，一种简单而有效的开源RL训练配方，旨在训练高性能终端智能体。论文首先指出现有终端智能体训练面临三大挑战：基准困难、数据匮乏和缺乏简单基线。为解决这些问题，作者构建了TMAX-15K数据集，包含14600个RL环境实例，通过组合式生成pipeline（集成难度控制、角色多样性和验证器多样化）实现了超过2.5倍于先前数据集规模，且难度呈渐进式分布。然后，基于该数据集，论文采用结果导向的RL训练方法，训练Qwen 3-9B模型得到TMAX-9B。在主实验Terminal-Bench 2.0上，TMAX-9B取得27%准确率，不仅远超同等规模模型，还优于许多更大模型，如GPT-4和Claude系列的部分版本（合理推断）。此外，RL训练带来的泛化收益显著：SWE-Bench Verified提升超过5个百分点，AIME成绩也明显提高，表明该配方增强了模型在多种智能体任务上的通用能力。消融实验表明，TMAX-15K在难度控制和多样性上优于其他数据集，且训练过程中模型性能持续增长。论文还考察了模型在不同任务上的行为，未发现明显的训练饱和迹象。总结而言，Tmax提供了一个强大、开放且简单的基线，有望推动终端智能体领域的学术研究。作者已完全开源数据集、模型和代码。**

<!-- paperflow:d79c96ff429349f9 -->
## StatABench: Dataset and Framework for Evaluating Statistical Analysis Capabilities of LLMs

[[Deep Reading - Jun 2026/StatABench-Dataset and Framework for Evaluating Statistical Analysis Capabilities of LLMs|Deep Reading]]

[https://arxiv.org/pdf/2606.22977](https://arxiv.org/pdf/2606.22977)

- **本文提出StatABench，一个系统性评估LLM统计分析能力的基准。该基准由两部分组成：Stat-Closed（404道多题型封闭题，覆盖18个统计主题）和Stat-Open（30个开放端到端建模任务，改编自专业竞赛）。实验使用LangChain MCP框架和多种数据科学agent，评估了10余种LLM。主要发现包括：即使最强模型GPT-5.1在封闭题上仅68.6%准确率，开放任务最佳agent框架得分61.86。结果凸显LLM在统计方法决策、工具化推理和综合建模方面的显著不足。论文还验证了LLM-as-Judge作为开放式评估的可靠性。该基准提供了现有最全面的统计能力评估，并为后续改进LLM在科学和数据分析领域的应用提供了基准和诊断工具。**

<!-- paperflow:7c193421646a5ce9 -->
## Causal Discovery in the Era of Agents

[[Deep Reading - Jun 2026/Causal Discovery in the Era of Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.23608](https://arxiv.org/pdf/2606.23608)

- **本文是一篇立场论文，核心论点是：在大模型时代，智能体不应直接参与因果发现的图构建环节（如推断边方向、提供先验），而应作为辅助工具支持数据检查、方法推荐、知识注入和结果解释。这一原则旨在保证因果结论的可靠性，使其仍然基于数据、假设和形式化算法。论文通过causal-learn+平台实例化该原则，该平台围绕开源因果发现库causal-learn构建，提供在线代理式工作流。案例研究使用Big Five人格数据展示了智能体如何在不提供因果证据的情况下协助用户完成发现过程。论文强调，智能体的到来不应改变因果证据的标准，而应通过更好的辅助提升因果发现的透明度、可复现性和用户理解。**

<!-- paperflow:b95ce14946ff9455 -->
## Memory Contagion: Cross-Temporal Propagation of Evaluator Bias via Agent Memory

[[Deep Reading - Jun 2026/Memory Contagion-Cross-Temporal Propagation of Evaluator Bias via Agent Memory|Deep Reading]]

[https://arxiv.org/pdf/2606.23195](https://arxiv.org/pdf/2606.23195)

- **本文首次提出并系统研究了LLM智能体记忆系统中的“Memory Contagion”现象——即有偏评估者的偏差通过记忆库跨时间传播到后续智能体。通过形式化定义和四阶段实验，作者证明即使在完美记忆整合（oracle）条件下，传播仍然显著发生（长度偏好Γ_A=13.18，权威偏差Γ_A=11.45）。记忆整合对不同类型偏差具有相反效应：它衰减长度偏好（Γ_B=2.03）但放大权威偏差（Γ_B=16.80，单次运行）。机制分析表明，传播主要通过内容通道实现（目标智能体检索有偏记忆的比例高达78-82%），而评估器状态偏移是次要途径。剂量响应实验发现，即使在污染率低至20%时，仍能检测到偏差传播，不存在安全阈值。这些发现揭示了当前智能体记忆系统的一个关键漏洞：记忆被设计为忠实地存储经验，却不区分经验是否被有偏评估器污染，从而成为偏差在时间维度上传播的通道。论文提供了量化传播的度量工具，为未来设计更鲁棒的智能体记忆系统奠定了基础。**

<!-- paperflow:35a5df8fb807690e -->
## DynamicMem: A Long-Horizon Memory Benchmark in Real-World Settings

[[Deep Reading - Jun 2026/DynamicMem-A Long-Horizon Memory Benchmark in Real-World Settings|Deep Reading]]

[https://arxiv.org/pdf/2606.22877](https://arxiv.org/pdf/2606.22877)

- **论文提出DynamicMem，一个合成长期记忆基准，用于模拟真实世界中用户画像的持续演变。通过构建15个月、跨16个应用的用户轨迹，每个用户平均2.2M token和1772个事件，DynamicMem捕捉了用户属性、习惯和偏好随季节和人生事件的变化。基准要求记忆系统从分散的跨应用行为信号中推断当前用户状态，而非直接回忆。在5个季度检查点上评估5类记忆系统，发现：状态补全性能随历史长度下降，而个性化服务性能稳定，揭示了现有系统在处理动态、分布式记忆时的根本缺陷。该基准为长期记忆与个性化研究提供了更真实的测试平台。**

<!-- paperflow:7602fa33cb2e5ef9 -->
## Semantic Consistency Policy Optimization for Reinforcement Learning of LLM Agents

[[Deep Reading - Jun 2026/Semantic Consistency Policy Optimization for Reinforcement Learning of LLM Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.25852v1](https://arxiv.org/pdf/2606.25852v1)

- **论文《Semantic Consistency Policy Optimization for Reinforcement Learning of LLM Agents》针对基于组的强化学习（Group-based RL）在训练 LLM 智能体时存在的信用分配问题，提出了一种名为语义一致性策略优化（SCPO）的奖励塑形方法。

**动机与问题**：当前 group-based RL 方法（如 GRPO）通过从完整轨迹的成功或失败结果推导步骤级信用。这种“轨迹结果”绑定导致了一个矛盾：语义几乎相同的中间步骤（例如，在 WebShop 中点击“下一页”）因其所在轨迹的最终成功或失败而被赋予正或负的信用。论文将此称为“语义信用不一致”，并指出它会向相似动作发送冲突的梯度，同时浪费失败轨迹中部分正确的进展（例如，前期步骤正确但后期步骤出错时，前期步骤亦被惩罚）。

**方法核心**：SCPO 是一种无值函数的奖励塑形插件，它无需训练价值函数即可为每个步骤重新分配更合理的信用。给定一个由多个 rollout 组成的组（其中至少包含一条成功轨迹），对于失败轨迹中的每一步，SCPO 使用一个通用语义重排器（reranker）计算它与成功兄弟中每一步的语义匹配分数，并找到最佳匹配。然后，它比较该步骤与其前一时刻的匹配分数变化：如果匹配分数增加，说明取得了朝向成功的积极进展，则赋予正向额外奖励；否则不奖励。这一额外奖励被加到原始的组奖励（如 GiGPO 的基于相对比较的步骤信用）上，用于后续的优势估计。SCPO 可即插即用，与任何 group-based agentic RL 方法兼容。

**实验与结果**：在 ALFWorld（文本家居任务）和 WebShop（电商导航）两个基准上，以 1.5B 参数 LLM 为基座，SCPO 显著提升了作为骨干的 GiGPO 方法。在 ALFWorld 上达到 93.7±4.1% 成功率，在 WebShop 上达到 74.8±2.0% 成功率，均为当时同尺度下的 SOTA。增益集中在最困难的多步任务上，证实了其对信用不一致的缓解作用。消融实验（合理...**

<!-- paperflow:2fb007b1fa06708f -->
## Why Multi-Step Tool-Use Reinforcement Learning Collapses and How Supervisory Signals Fix It

[[Deep Reading - Jun 2026/Why Multi-Step Tool-Use Reinforcement Learning Collapses and How Supervisory Signals Fix It|Deep Reading]]

[https://arxiv.org/pdf/2606.26027v1](https://arxiv.org/pdf/2606.26027v1)

- **本文系统研究了多步工具使用任务中强化学习训练的不稳定性问题，并探索了多种监督信号作为解决方案。首先，论文通过实验揭示了单独使用RL会导致模型在训练中突然崩溃，性能急剧下降，工具调用结构（例如控制token）失效。深入分析表明，崩溃根源在于特定控制token（如格式标记）的概率出现尖峰，破坏了结构化执行顺序，但模型的底层工具使用能力并未丧失，只是被误格式掩盖。这一发现挑战了“RL失败意味着能力不足”的直觉。

为了解决该问题，论文提出了一个统一的实验框架，系统比较了四种监督信号：离线监督、提示引导、错误示例监督以及组合信号，并在两种训练方案（同步SFT+RL和交错SFT+RL）下评估。主要发现包括：交错SFT+RL能显著提高训练稳定性，几乎避免崩溃现象，但导致格式和内容分布外泛化性能下降；同步方案稳定性较差但泛化稍好。学习率分析显示过高的学习率会加剧不稳定性和崩溃风险。此外，论文还跨设置测试了泛化能力，表明不同任务间存在迁移差异。

实验结果表明，理解RL失败的原因比简单调整超参数更重要。多样化的监督信号能引导模型在探索中保持结构完整性，但存在稳定性-泛化权衡。论文局限性包括训练数据规模有限、工具环境开源受限，以及OOD评估仅覆盖格式和内容变化。整体上，本文为智能体RL训练提供了重要见解和实用指导。**

<!-- paperflow:c558e651db2811b5 -->
## Uncertainty Quantification for Computer-Use Agents: A Benchmark across Vision-Language Models and GUI Grounding Datasets

[[Deep Reading - Jun 2026/Uncertainty Quantification for Computer-Use Agents-A Benchmark across Vision-Language Models and|Deep Reading]]

[https://arxiv.org/pdf/2606.25760v1](https://arxiv.org/pdf/2606.25760v1)

- **本文针对计算机使用代理中VLM执行GUI点击任务的不确定性量化问题，提出了一个名为ARGUS的跨体制基准。该基准系统比较了27种开放权重和8种闭源的后验UQ方法在4个VLM代理和4个GUI定位数据集上的性能。主要贡献包括：(1) 构建了第一个跨体制UQ评估框架，定义了体制由数据集、模型类和可观测接口构成；(2) 全面评估了包括logit-based、采样一致性、隐藏状态密度、注意力、verbalized和保形预测在内的多种UQ方法族；(3) 揭示UQ排名选择性迁移的规律：在固定模型下跨数据集高度稳定，但跨模型类和跨接口时显著下降；(4) 为闭源模型提供UQ方法选择建议，强调需要重新排序而非外推。实验表明，隐藏状态和密度方法在开放权重模型中泛化最好，但闭源场景下verbalized自评估和保形预测更实用。该工作为构建可靠、可迁移的UQ系统提供了重要指导。**

<!-- paperflow:f7ac7d98f11c0349 -->
## WinDOM: Self-Family Distillation for Small-Model GUI Grounding

[[Deep Reading - Jun 2026/WinDOM-Self-Family Distillation for Small-Model GUI Grounding|Deep Reading]]

[https://arxiv.org/pdf/2606.25964v1](https://arxiv.org/pdf/2606.25964v1)

- **论文针对小模型GUI定位任务，提出WinDOM数据集和自家族蒸馏（SFD）方法，系统研究了冷启动蒸馏饱和度对后续GRPO强化学习的交互影响。主要发现是未饱和的冷启动比完全收敛的冷启动作为GRPO初始化器效果更好，提升OOD平均+5.4点。同尺度EMA教师无需外部模型即可达到接近跨尺度教师（4B）的性能。论文通过大规模实验验证了方法的有效性，并展示了在多个OOD基准上的显著改进。论文还探讨了在线RL、偏好优化等未来方向。整体而言，这项工作为训练小规模、高效的GUI定位智能体提供了实用的配方和全新的视角。**

<!-- paperflow:e08c0381e270d476 -->
## Reclaim Evaluation: A Lossy Memory Is Worse Than an Empty One

[[Deep Reading - Jun 2026/Reclaim Evaluation-A Lossy Memory Is Worse Than an Empty One|Deep Reading]]

[https://arxiv.org/pdf/2606.25449v1](https://arxiv.org/pdf/2606.25449v1)

- **本文研究了语言模型记忆压缩导致的可纠正性问题。作者发现，如果模型的记忆只保留了错误结论而丢弃了推导过程（lossy memory），那么模型会自信地重复错误，且任何后续纠正都无法改变这一错误，这种失败被称为脆性记忆（brittle memory）。相比之下，如果模型没有记忆（empty memory），它会弃权，避免犯错。这一现象在测试的7个模型上一致成立，从未逆转。
作者提出了 reclaim evaluation 协议来测量可纠正性：在固定记忆预算下压缩交互，然后测试纠正能否恢复正确答案。实验表明，可纠正性完全取决于答案决定源（source）是否存活，而非模型能力。基于此，作者提出了 source-first 策略：保留可重算的源头，丢弃可重新推导的结论。该策略在 oracle 版本下完全恢复可纠正性（挽回率1.00），而一个单提示的可部署版本也能达到0.49–0.88的挽回率，但主要集中在紧凑的数字型源头。
该工作的意义在于揭示了记忆压缩中的一个关键权衡：保留结论会阻断纠正，保留源头则保留可纠正性。这对于智能体、助手记忆系统等需要长期记忆的部署场景具有重要启示。**

<!-- paperflow:5e67b7cb07926198 -->
## GUI agent: Guided Exploration of User-Sensitive Screens

[[Deep Reading - Jun 2026/GUI agent-Guided Exploration of User-Sensitive Screens|Deep Reading]]

[https://arxiv.org/pdf/2606.25705v1](https://arxiv.org/pdf/2606.25705v1)

- **本文针对LLM驱动的GUI代理在开放环境中面临用户敏感屏幕时缺乏接管机制的问题，提出了一种系统探索查询空间的方法。主要贡献包括：开发一个探索器代理，从用户示范任务出发，通过迭代训练构建动作-状态图，自动识别会导致用户敏感状态的查询路径；在选定的GUI应用上验证表明，用户敏感查询空间随训练轮次增加而缩小，证明探索器有效。相关工作涵盖了GUI代理泛化与探索先验。实验展示了方法的可行性，但缺乏定量结果和严格基线对比。未来工作可扩展应用场景和优化效率。**

<!-- paperflow:1f93ecad8a1e8175 -->
## Staying In Character: Perspective-Bounded Memory For Book-Based Role-Playing Agents

[[Deep Reading - Jun 2026/Staying In Character-Perspective-Bounded Memory For Book-Based Role-Playing Agents|Deep Reading]]

[https://arxiv.org/pdf/2606.25632v1](https://arxiv.org/pdf/2606.25632v1)

- **本文针对基于小说的角色扮演智能体中普遍存在的事实越界和风格单调问题，提出了REVERIEMEM视角受限三层记忆架构。该架构将记忆分为情景层、语义层和个性层，分别对应第一人称经历、可见性标记的事实知识以及情境依赖的表达模式。推理时通过分层检索与约束，确保角色仅使用其可知信息并调整风格。为评估知识边界，作者构建了包含4386个问题、覆盖8部小说的KBF-QA基准。实验表明，REVERIEMEM在知识边界保真度上提升34.6个百分点，在叙事生成质量上达到79%胜率。论文还讨论了现有方法的局限性，并指出了未来方向，如极端视角压力测试和多智能体协调。这项工作为构建更真实的角色扮演系统提供了系统性方案。**

<!-- paperflow:210a3d4dc810408a -->
## Autodata: An agentic data scientist to create high quality synthetic data

[[Deep Reading - Jun 2026/Autodata-An agentic data scientist to create high quality synthetic data|Deep Reading]]

[https://arxiv.org/pdf/2606.25996v1](https://arxiv.org/pdf/2606.25996v1)

- **本文提出Autodata框架，旨在通过自主AI智能体模拟人类数据科学家的工作流来生成高质量合成数据。框架的核心是Agentic Self-Instruct，智能体迭代生成数据、评估质量、调整策略。实验涵盖计算机科学、法律推理和数学推理三个领域，表明Autodata优于传统合成数据方法，而元优化能带来更大提升。论文认为，该方法将计算资源从推理转向数据生成，可能改变AI数据构建方式。但受限于证据，具体实验细节、基线名称、数值结果需阅读原文。**

# Computer Vision

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Computer Vision
- 方法：generation, language, vision-language-model, reasoning, vision, optimization, retrieval, multimodal-learning
- 论文/报告：17 篇
- Text-Vision Co-Instructed Image Editing
- Timage: A Generative Text-in-Image Paradigm for Fine-Tuning Vision-Language Models
- The Hidden Evolution of Disguised Visual Context inside the VLM
- SPOT-E: Test-Time Entropy Shaping with Visual Spotlights for Frozen VLMs
- Compression and Retrieval: Implicit Memory Retrieval for Video World Models
- Faithful Grounded Visual Reasoning via Learned Proxy-Tokens
- CFPO: Counterfactual Policy Optimization for Multimodal Reasoning
- Data Selection Through Iterative Self-Filtering for Vision-Language Settings
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:1e301306958091de -->
## Text-Vision Co-Instructed Image Editing

[[Deep Reading - Jun 2026/Text-Vision Co-Instructed Image Editing|Deep Reading]]

[https://arxiv.org/pdf/2606.16767v1](https://arxiv.org/pdf/2606.16767v1)

- **In contrast, visual prompts such as drag and point can provide precise spatial guidance, but are limited by the inherent ambiguity in semantic intent.2026 To unify the strength…

Existing image editing methods can be generally categorized into textual instruction-based and visual prompt-based ones. Textual instructions are semantically expressive, but are limited by the coarse granularity of spatial control of the editing results.

We introduced Text-Vision Co-Instructed Image Editing, a new image editing task that jointly leverages textual instructions and sparse visual prompts to reduce the ambiguity of… To tackle this task, we proposed TV-Edit, a plug-and-play framework with a decoupled Content-Aware Spatial Controller, and constructed a text-vision paired training dataset to…

TV-Edit-Qwen reduces MD𝑑to 0.0462, achieving a 28.7% improvement over the best drag-based method and demonst...**

<!-- paperflow:d814a796972b2f4b -->
## Timage: A Generative Text-in-Image Paradigm for Fine-Tuning Vision-Language Models

[[Deep Reading - Jun 2026/Timage-A Generative Text-in-Image Paradigm for Fine-Tuning Vision-Language Models|Deep Reading]]

[https://arxiv.org/pdf/2606.19944v1](https://arxiv.org/pdf/2606.19944v1)

- **论文提出Timage范式，旨在解决多模态大语言模型在细粒度空间推理中常见的对齐失败问题。传统方法要么依赖权重微调（损害通用能力），要么使用不够精准的文本指令或简单视觉提示。Timage另辟蹊径，在输入层面将文本查询直接以排版覆盖的形式嵌入图像中，使得模型可以利用其固有的OCR和空间理解能力。覆盖层的生成由受约束薛定谔桥（cSB）控制，cSB通过两个耦合的随机阶段（区域搜索和外观塑造）产生对齐查询语义、保护前景物体且字形可读的文本覆盖。在VMCBench基准上，搭载Qwen2.5-VL-7B的Timage不仅超越了所有同类基线（包括参数微调和视觉提示方法），甚至胜过了更大规模的专有系统。实验表明，语义丰富的视觉锚点比纯粹的几何约束更有助于复杂推理。该工作强调了输入重构作为一种架构中立的增强手段的潜力，为多模态对齐研究开辟了新方向。**

<!-- paperflow:86123b60f2f2d9b8 -->
## The Hidden Evolution of Disguised Visual Context inside the VLM

[[Deep Reading - Jun 2026/The Hidden Evolution of Disguised Visual Context inside the VLM|Deep Reading]]

[https://arxiv.org/pdf/2606.20077v1](https://arxiv.org/pdf/2606.20077v1)

- **这篇论文对VLM中视觉信息集成的两种主要范式——in-context注入（视觉token作为前缀与文本拼接）和layer-wise注入（通过门控交叉注意力或附加token将每层视觉特征注入LLM）——进行了公平、系统的比较。作者在相同训练条件下，在单图、多图和视频基准上评估了这三种变体，发现in-context注入在OCR和视频任务上显著优于layer-wise注入。为了理解性能差距，他们进行了四项深入分析：语义演化、频率特性、注意力分配和表示质量。关键发现是in-context注入下的视觉token在LLM内部经历了一种“隐藏演化”：从浅层几乎无语义的高频缺失状态，逐步发展为具有丰富语义和高频成分的表示；而layer-wise注入则缺乏这种渐进式演化，导致视觉表示与语言空间对齐较差。频率分析进一步揭示in-context注入允许视觉token从低频主导向高频增强转变，这恰好匹配了OCR等任务对细节的需求。论文还指出注意力分配本身不能解释性能差异，真正驱动性能的是每层视觉表示的质量。这些结果为理解VLM内部机制提供了新视角，并为设计更有效的多模态系统提供了指导。**

<!-- paperflow:547bc92d1ef40c01 -->
## SPOT-E: Test-Time Entropy Shaping with Visual Spotlights for Frozen VLMs

[[Deep Reading - Jun 2026/SPOT-E-Test-Time Entropy Shaping with Visual Spotlights for Frozen VLMs|Deep Reading]]

[https://arxiv.org/pdf/2606.20244v1](https://arxiv.org/pdf/2606.20244v1)

- **本文提出SPOT-E，一种测试时视觉适应方法，旨在提升冻结视觉语言模型（VLM）对细粒度视觉证据的利用。作者首先指出，现有开环视觉干预方法缺乏机制验证高亮区域是否被模型实际使用，而答案跨度熵可作为模型内部的证据可用性信号。但naive熵最小化有歧义：低熵可能来自证据驱动的正确决策或捷径行为。为解决此歧义，SPOT-E引入低熵锚点并设计熵整形目标，在降低答案不确定性的同时保留基线高置信度token。技术实现上，SPOT-E包含一个CLIP-based的视觉spotlight模块，根据问题生成条件化的视觉掩膜，并通过Group Relative Policy Optimization (GRPO)对每个测试实例的LoRA参数进行轻量优化。实验涵盖多个开源和闭源VLM，在细粒度VQA和视觉推理基准上，SPOT-E带来一致提升（约2-5%），并在视觉损坏下展示更强鲁棒性。案例研究可视化证实，spotlight聚焦于问题相关小区域，且区域可见性变化直接影响熵和输出。主要贡献包括：(1) 提出基于熵整形的测试时视觉适应范式；(2) 实现即插即用的SPOT-E框架；(3) 跨骨干和基准的实证验证。局限在于：当spotlight错失关键区域时，方法性能可能受限于初始定位质量，且对极端遮挡的鲁棒性未全面检验。**

<!-- paperflow:c4bbf0cb66d0c398 -->
## Compression and Retrieval: Implicit Memory Retrieval for Video World Models

[[Deep Reading - Jun 2026/Compression and Retrieval-Implicit Memory Retrieval for Video World Models|Deep Reading]]

[https://arxiv.org/pdf/2606.23105](https://arxiv.org/pdf/2606.23105)

- **本文提出压缩与检索（CaR），一种通过注意力驱动的隐式记忆检索机制，结合轻量级上下文压缩网络，解决视频世界模型在复杂相机轨迹下的长程记忆一致性问题，并构建了大规模合成数据集SceneFly。

ra trajectories and environments. In this paper, we propose Compression and Retrieval (CaR), an attention-driven implicit memory retrieval mechanism to overcome these limitations.

Our contributions are threefold: \- We propose Compression and Retrieval, an attention-driven implicit memory retrieval mechanism for scene-consistent interactive video generation. \- We introduce SceneFly, a large-scale revisiting synthetic dataset with frame-level camera annotations designed for training and evaluating memory capability.

historical context. Rather than selecting frames before generation, CaR concatenates compressed context tokens with the target video tokens and retrieves memory through attention.

主要贡献包括：Our contributions are threefold: \- We propose Compression and Retrieval, an attention-driv...**

<!-- paperflow:d8e2f5cd7b2b6075 -->
## Faithful Grounded Visual Reasoning via Learned Proxy-Tokens

[[Deep Reading - Jun 2026/Faithful Grounded Visual Reasoning via Learned Proxy-Tokens|Deep Reading]]

[https://arxiv.org/pdf/2606.23354](https://arxiv.org/pdf/2606.23354)

- **本文针对多模态大语言模型在视觉问答中缺乏忠实可解释性的问题，提出了一种基于学习到的代理令牌的视觉定位机制。该机制通过离散符号指针直接索引图像潜在区域，建立了语言与视觉之间的可学习语义联系，克服了传统坐标表示导致的语义-空间鸿沟。作者构建了ComposerGCoT数据集以系统评估推理一致性和定位准确性。实验表明，Composer在答案准确率不下降的前提下，将视觉定位准确率提升9个百分点，证实了代理令牌在捕获空间语义方面的优越性。论文的贡献包括：1）提出代理令牌这一新颖视觉定位机制；2）构建专门的评估数据集；3）通过实验验证机制的有效性。该工作为开发更可信、更可解释的多模态大语言模型提供了新方向。**

<!-- paperflow:3e06fec350e9800b -->
## CFPO: Counterfactual Policy Optimization for Multimodal Reasoning

[[Deep Reading - Jun 2026/CFPO-Counterfactual Policy Optimization for Multimodal Reasoning|Deep Reading]]

[https://arxiv.org/pdf/2606.23206](https://arxiv.org/pdf/2606.23206)

- **论文针对大视觉语言模型在强化学习微调中忽视视觉证据、产生幻觉的问题，提出反事实策略优化（CFPO）框架。该方法通过跨模态反事实增强，在训练中强制模型关注真实视觉线索，而非语言捷径。CFPO可无缝集成于现有RL算法，无需额外监督。在多项基准上，CFPO显著优于标准RL和感知增强方法，验证了因果一致性机制的有效性。代码开源，为多模态推理的因果增强提供了新思路。**

<!-- paperflow:1d74b55494464da3 -->
## Data Selection Through Iterative Self-Filtering for Vision-Language Settings

[[Deep Reading - Jun 2026/Data Selection Through Iterative Self-Filtering for Vision-Language Settings|Deep Reading]]

[https://arxiv.org/pdf/2606.23611](https://arxiv.org/pdf/2606.23611)

- **本文提出Self-Filtering，一种用于视觉语言设置的自举数据选择方法。核心思想是：无需使用预训练模型或外部数据，仅通过迭代训练CLIP模型并利用模型自身预测来逐步筛选出更干净、更具代表性的训练子集。该方法在每一轮中训练模型，然后基于置信度选择高概率干净样本，同时保留部分低置信度样本以维持多样性，形成改进的数据混合，用于下一轮训练。实验表明，使用Self-Filtering选出的固定数据集训练，其下游性能与使用OpenAI的预训练CLIP模型筛选的数据集训练的性能相当，且在混合训练中优于直接在完整噪声数据集上训练。这项工作证明了自举数据选择在大规模视觉语言预训练中的可行性，减少了对外部资源的依赖。**

<!-- paperflow:305faf8742fa06bd -->
## Scaling Linear Mode Connectivity and Merging to Billion Parameter Pretrained Transformers

[[Deep Reading - Jun 2026/Scaling Linear Mode Connectivity and Merging to Billion Parameter Pretrained Transformers|Deep Reading]]

[https://arxiv.org/pdf/2606.23607](https://arxiv.org/pdf/2606.23607)

- **本文提出一种可扩展的框架，通过对预训练Transformer权重进行参数化的功能保持变换（即消除权重空间的对称性），并结合双向学习过程，使得独立训练的模型可以在权重空间中通过简单的线性插值路径连接和合并。主要贡献包括：1）系统定义了Transformer中与LMC相关的多个对称性族（置换、缩放、偏移吸收），并给出可微的参数化；2）提出双向匹配策略，同时优化两个模型的变换，而非仅单向调整；3）在语言模型（Pythia系列至6.9B）上实现近乎零的损失障碍，在视觉模型（ViT-L）上实现沿插值路径维持高准确率。实验表明，对称性对齐是解锁大型模型LMC的关键，且双向学习优于单向。该方法为模型合并、模型集成和权重空间插值提供新工具，并可扩展至更多实际应用（如将多个社区模型合并为更强模型）。未来工作可探索跨架构LMC、多模型合并及下游融合应用。**

<!-- paperflow:3155dd62c635aeb3 -->
## SPAR: Semantic-Pixel Self-Alignment and Adaptive Routing for Unified Multimodal Models

[[Deep Reading - Jun 2026/SPAR-Semantic-Pixel Self-Alignment and Adaptive Routing for Unified Multimodal Models|Deep Reading]]

[https://arxiv.org/pdf/2606.23041](https://arxiv.org/pdf/2606.23041)

- **论文SPAR提出语义-像素自对齐与自适应路由框架，解决统一多模态模型中理解与生成的对齐问题。通过非对称双流分词器解耦语义（轻量级）和像素（细节）表示，并引入动态令牌路由从MLLM多层特征中按需聚合。在ImageNet 50k重建任务上取得SOTA，证明了方法的有效性。贡献包括双流对齐架构、自适应路由机制以及跨任务性能优势。局限：依赖预训练MLLM，可能增加训练开销；路由机制的内在复杂度；对低计算量场景的适应性未探讨。**

<!-- paperflow:a04add2607834788 -->
## Data Evolution by Wittgenstein's Rule Following

[[Deep Reading - Jun 2026/Data Evolution by Wittgenstein's Rule Following|Deep Reading]]

[https://arxiv.org/pdf/2606.22674](https://arxiv.org/pdf/2606.22674)

- **本文提出数据学（philomatics）中的WRF数据演化框架，旨在从历史数据集序列生成演化数据集。受维特根斯坦规则遵循和家族相似性哲学概念启发，WRF将每个数据集表示为描述符向量，通过外推历史描述符轨迹预测规则遵循目标，通过平均预测家族相似性目标。然后从历史数据生成候选数据集，通过加权损失评分，并可选进行描述符空间优化。框架允许样本量和特征维度变化。在合成和图像数据集上的模拟验证了WRF能产生有意义的演化数据。主要贡献：1) 定义数据演化问题；2) 提出哲学动机框架；3) 设计描述符表示与规则预测方法；4) 演示生成和优化流程。限制包括描述符手工设计、优化计算成本、实验规模有限。**

<!-- paperflow:cc219d1c5aa0164b -->
## How Robust is OCR-Reasoning? Evaluating OCR-Reasoning Robustness of Vision-Language Models under Visual Perturbations

[[Deep Reading - Jun 2026/How Robust is OCR-Reasoning-Evaluating OCR-Reasoning Robustness of Vision-Language Models under|Deep Reading]]

[https://arxiv.org/pdf/2606.26041v1](https://arxiv.org/pdf/2606.26041v1)

- **本文系统研究了视觉语言模型在OCR推理任务中的鲁棒性问题。作者指出，尽管VLM在OCR基准上表现优异，但在视觉退化（如模糊、噪声、几何变换）下的表现尚不了解。为了填补这一空白，他们构建了OCR-Robust基准，包含812个样本，覆盖文档、场景文本、收据、手写、数学（OCR1.0）以及图表、几何图、表格（OCR2.0）。通过预研究从18种候选扰动中筛选出5种代表类型（各3个严重级别），并采用干净准确率、相对腐败保留（RCR）、最坏情况保留（WCR）和复合鲁棒性指数（CRI）进行评估。实验涵盖18个模型，包括闭源（GPT-4V、Gemini等）、开源VLM（LLaVA、Qwen-VL等）和OCR+LLM分解流水线。主要发现：①更高干净准确率不意味着更强鲁棒性；②模型在结构密集型任务（图表、表格）上退化更显著；③闭源模型通常更鲁棒；④OCR+LLM分解和CoT提示不能稳定改善鲁棒性。论文讨论了数据规模、扰动选择范围等方面的局限性，并指出复合扰动和多语言扩展是未来方向。**

<!-- paperflow:fe37cea035f7ca57 -->
## Causal-rCM: A Unified Teacher-Forcing and Self-Forcing Open Recipe for Autoregressive Diffusion Distillation in Streaming Video Generation and Interactive World Models

[[Deep Reading - Jun 2026/Causal-rCM-A Unified Teacher-Forcing and Self-Forcing Open Recipe for Autoregressive Diffusion D|Deep Reading]]

[https://arxiv.org/pdf/2606.25473v1](https://arxiv.org/pdf/2606.25473v1)

- **本文针对自回归视频扩散模型推理成本高的问题，提出了Causal-rCM，将rCM的扩散蒸馏思想（前向散度CM与反向散度DMD互补）适配到因果训练范式（teacher-forcing和self-forcing）。技术贡献包括：(1)首次将连续时间一致性模型（sCM/MeanFlow）用于自回归视频扩散，并通过定制FlashAttention-2 JVP内核实现高效训练，收敛速度比离散时间CM快10倍。(2)设计分阶段训练流水线：先通过teacher-forcing CM（TF-CM）进行高效初始蒸馏，再利用self-forcing DMD（SF-DMD）进行on-policy微调，两者协同达到SOTA。(3)在流式视频生成中（帧级和块级）仅需1-2步采样，VBench-T2V达到84.63分，与50步教师模型相当。在动作条件交互世界模型Cosmos 3上展示了自然交互能力。实验揭示了TF-CM作为初始化关键、连续时间CM的优势以及联合训练的挑战。论文提供了开源算法和基础设施，为高效视频扩散蒸馏建立了新基准。局限性包括长rollout不稳定、完全联合优化困难以及仅合成数据训练。**

<!-- paperflow:b738a6e81ea19b42 -->
## TriViewBench: Controlled Complexity Scaling for Multi-View Structural Reasoning in MLLMs

[[Deep Reading - Jun 2026/TriViewBench-Controlled Complexity Scaling for Multi-View Structural Reasoning in MLLMs|Deep Reading]]

[https://arxiv.org/pdf/2606.26029v1](https://arxiv.org/pdf/2606.26029v1)

- **论文提出了TriViewBench，一个用于诊断MLLMs多视图结构推理的受控基准。通过合成3D场景参数化对象数量和遮挡，生成1923个场景和超过14K QA对，分为四种复杂度等级和三种推理类别。评估了18个开源和闭源MLLMs，发现所有模型存在一致的能力层次（局部决策>对象计数>全局恢复），且性能随复杂度单调下降，尤其在高复杂度任务上退化严重。错误分析揭示了单视图欠计数和多视图过度计数两种不相关失败模式，表明模型缺乏有效的跨视图推理。CoT提示仅在高能力模型上对全局恢复有微小改善，整体收益近乎为零，指出瓶颈在于空间表征而非推理策略。论文将TriViewBench定位为系统诊断MLLMs结构推理可扩展性的框架。**

<!-- paperflow:138aae08379ad48d -->
## Brevity is the Soul of Inference Efficiency: Inducing Concision in VLMs via Data Curation

[[Deep Reading - Jun 2026/Brevity is the Soul of Inference Efficiency-Inducing Concision in VLMs via Data Curation|Deep Reading]]

[https://arxiv.org/pdf/2606.25432v1](https://arxiv.org/pdf/2606.25432v1)

- **本文提出通过预训练数据筛选来诱导VLM的简洁性输出，从而提升推理效率。作者首先指出当前效率优化忽略输出长度膨胀的问题，然后设计了一套数据筛选流程，选择简洁且正确的训练样本。在1B-4B参数规模的14个VLM对比中，筛选模型在精度几乎持平的前提下，每次正确回答的FLOPs降低最多35倍。匹配长度下，筛选模型精度相比未筛选基线大幅提升（最高+21.2pp），且增益随规模增大。进一步分析表明，通用冗长无益，推理式冗长仅在有限条件下有效且窗口缩。实验结论支持“推理效率应聚焦于每次正确回答的token成本”的观点，并为通过数据治理实现简洁性提供了实证基础。**

<!-- paperflow:d48f07385988b31b -->
## In-context Region-based Drag: Drag Any Region to Any Shape

[[Deep Reading - Jun 2026/In-context Region-based Drag-Drag Any Region to Any Shape|Deep Reading]]

[https://arxiv.org/pdf/2606.25907v1](https://arxiv.org/pdf/2606.25907v1)

- **本文提出ICRDrag，一种基于上下文学习（in-context learning）的区域拖拽图像编辑方法。不同于点拖拽的模糊性，区域拖拽使用源区域掩码和目标区域掩码精确指定编辑意图。ICRDrag以源图像、源掩码和目标掩码为输入，通过扩散Transformer（DiT）在单次前向传播中生成目标图像。核心贡献是两种注意力正则化：图像-掩码注意力一致性（IMAC）和源-目标注意力对应（STAC），分别对齐跨模态注意力和确保区域间对应。为支持大规模训练，作者构建了包含20万对掩码-图像对的PRD数据集。实验在多个数据集上显示，ICRDrag在编辑精度、视觉质量、用户偏好方面大幅超过现有方法。消融实验验证了两种正则化的有效性，且方法可泛化到不同DiT骨干。代码、模型和数据集已开源。**

<!-- paperflow:2ca3894f165516ea -->
## RAVEN: Long-Horizon Reasoning & Navigation with a Visuo-Spatio-Temporal Memory

[[Deep Reading - Jun 2026/RAVEN-Long-Horizon Reasoning & Navigation with a Visuo-Spatio-Temporal Memory|Deep Reading]]

[https://arxiv.org/pdf/2606.25206v1](https://arxiv.org/pdf/2606.25206v1)

- **本文提出RAVEN（Retrieval-Augmented Visuo-spatio-temporal mEmory for Navigation），一个直接基于视觉嵌入的智能记忆系统，专为长时域机器人问答与导航设计。核心创新在于跳过图像到文本的字幕化过程，将观察到的RGB图像编码为稠密向量，并与位姿、时间共同索引，存储于向量数据库同时构建空间地图。查询时，由一个LLM驱动的代理解析自然语言约束并调用相应的空间、语义或时间检索工具。在NaVQA和FindingDory两个基准上，RAVEN一致超越基于字幕的方法，并以更低的成本匹配GPT-4V等大模型。在真实机器人平台Unitree Go1上，RAVEN在多个大型室内环境成功完成自然语言目标导航任务，平均成功率87%。此外，系统在视频帧率压缩22315倍时仍能保持鲁棒性能。论文最后讨论了依赖预训练编码器、动态场景挑战等局限性，并指出未来方向。**

# Machine Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Machine Learning
- 方法：stat-ml, deep-learning, stat-me
- 论文/报告：4 篇
- Learning Process Rewards via Success Visitation Matching for Efficient RL
- What Shapes Emergent Misalignment? Insights from Training Dynamics, Model Priors, and Data
- On-Policy Self-Distillation with Sampled Demonstrations Reduces Output Diversity
- Data Augmentation: A Fourier Analysis Perspective
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:6ecd3545dc32798a -->
## Learning Process Rewards via Success Visitation Matching for Efficient RL

[[Deep Reading - Jun 2026/Learning Process Rewards via Success Visitation Matching for Efficient RL|Deep Reading]]

[https://arxiv.org/pdf/2606.23640](https://arxiv.org/pdf/2606.23640)

- **本文提出成功访问匹配（SVM）过程奖励方法，用于解决稀疏奖励强化学习中的信用分配问题。核心思想是通过训练一个判别器来区分成功与失败历史轨迹的状态-动作分布，然后以其输出作为密集过程奖励。该方法无需额外人工知识，且理论上保证最优策略不变。在机器人控制策略微调任务上，SVM在模拟和真实环境中均显著加速了RL收敛，相比仅使用稀疏结果奖励有大幅提升。实验还分析了成功/失败轨迹对性能的影响。本文的主要贡献包括提出SVM方法、提供理论保证、在机器人操作任务上验证有效性，并展示了自动化奖励塑形的潜力。局限包括依赖历史轨迹的质量、可能需要预训练策略收集起始数据，以及尚未在极端稀疏奖励任务中测试。**

<!-- paperflow:07ada5b610e5c63d -->
## What Shapes Emergent Misalignment? Insights from Training Dynamics, Model Priors, and Data

[[Deep Reading - Jun 2026/What Shapes Emergent Misalignment-Insights from Training Dynamics, Model Priors, and Data|Deep Reading]]

[https://arxiv.org/pdf/2606.20814](https://arxiv.org/pdf/2606.20814)

- **We study EM and its variability directly through the components of fine-tuning: training dynamics, model priors, and data.

Emergent misalignment (EM) is a phenomenon in which models generalize with narrow fine-tuning, leading to broad (yet uneven) misalignment across evaluation questions. We study EM and its variability directly through the components of fine-tuning: training dynamics, model priors, and data.

directly: (1) training dynamics, (2) model “priors”, and (3) data. We explain each angle in more detail below, connecting related works and our proposals.

ing learning schedules for one narrow fine-tuning, we did not find meaningful local minima that yielded significantly better alignment scores at comparable or lower training loss. \- Model “priors”: Our comparisons found that while most misaligned models have statistically different mean and standard deviations from the pre-tra...**

<!-- paperflow:4a86e5fdda34a88e -->
## On-Policy Self-Distillation with Sampled Demonstrations Reduces Output Diversity

[[Deep Reading - Jun 2026/On-Policy Self-Distillation with Sampled Demonstrations Reduces Output Diversity|Deep Reading]]

[https://arxiv.org/pdf/2606.26091v1](https://arxiv.org/pdf/2606.26091v1)

- **本文系统研究了 on-policy self-distillation with sampled demonstrations (SDSD) 对输出多样性的影响。首先，通过理论分析揭示 SDSD 的最优策略会通过点状条件互信息 (PCMI) 加权，使得模型过度增强与已采样示范兼容的 rollout，从而造成模式坍塌——这是与理想 on-policy RL（保留正确解概率比）的关键区别。其次，在图路径寻找和科学问答基准上实验证实：SDSD 虽能取得高 pass@1 准确率，但 pass@k 曲线平坦，功能/语义多样性显著低于 RL，且在分布外泛化中失败。论文还指出 token 级熵不能有效反映这种多样性损失。最后，讨论了缓解策略（如修改梯度方向）并指出未来方向。**

<!-- paperflow:9b13df4b26b42e55 -->
## Data Augmentation: A Fourier Analysis Perspective

[[Deep Reading - Jun 2026/Data Augmentation-A Fourier Analysis Perspective|Deep Reading]]

[https://arxiv.org/pdf/2606.24418v1](https://arxiv.org/pdf/2606.24418v1)

- **本文系统研究了有限群对称性下部分数据增强的统计性质。作者针对基于投影基的密度估计和回归问题，利用傅里叶分析和表示论建立了理论框架。主要发现：当使用随机采样的群元素子集进行数据增强时，估计器的极小化最优速率与使用整个群相同，近似误差随子集大小增大而消失。这一结果解释了实践中随机部分增强的广泛有效性。同时，论文证明了一个不可能性结果：若假设空间足够表达，精确不变性只能通过全群平均实现，任何部分增强都无法严格保证。这些结论统一了完全/部分数据增强、精确/近似对称性的理论理解，并为设计计算高效的对称性学习算法提供了指导。**

# AI Research

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI Research
- 方法：ai-for-science
- 论文/报告：2 篇
- Data-Driven Evolution of Library and Information Science Research Methods (1990-2022): A Perspective Based on Fine-grained Method Entities
- When Surveys Become Conversations: Adaptive Matrix Validation for AI-Assisted Interviews
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:d3631ed53ef54964 -->
## Data-Driven Evolution of Library and Information Science Research Methods (1990-2022): A Perspective Based on Fine-grained Method Entities

[[Deep Reading - Jun 2026/Data-Driven Evolution of Library and Information Science Research Methods (1990-2022)-A Perspect|Deep Reading]]

[https://arxiv.org/pdf/2606.25320v1](https://arxiv.org/pdf/2606.25320v1)

- **本研究针对图情领域自1990年代以来数据驱动研究范式的影响，通过自动提取细粒度方法实体（算法与模型、数据资源、软件与工具、指标），系统分析了1990-2022年间研究方法的演化特征。论文首先构建了包含图情领域代表性期刊全文的语料库，从方法章节中提取四类实体，然后从三个维度（时间、研究主题、研究方法）进行量化分析。主要发现包括：数据资源是方法论演化的核心驱动力；方法实体呈现出‘出现-稳定/实际应用’的循环模式，即新方法先快速涌现，随后进入稳定或实际应用阶段；不同研究主题和方法类型下的实体分布存在显著差异；2011-2013年间实体相似度下降可能与神经网络技术的兴起有关。研究为理解图情领域方法论的演化提供了客观、定量的视角，有助于指导未来方法选择和研究规划。论文还讨论了方法实体抽取的局限性（依赖方法章节假设）、语料覆盖范围有限等，并指出未来可向多学科扩展、结合引用分析等方向。**

<!-- paperflow:2e114b78c3a5d644 -->
## When Surveys Become Conversations: Adaptive Matrix Validation for AI-Assisted Interviews

[[Deep Reading - Jun 2026/When Surveys Become Conversations-Adaptive Matrix Validation for AI-Assisted Interviews|Deep Reading]]

[https://arxiv.org/pdf/2606.24244v1](https://arxiv.org/pdf/2606.24244v1)

- **本文针对AI辅助访谈这一新兴调查范式中的关键测量问题，提出了一套完整的统计设计和推断框架——自适应矩阵验证（AMV）。主要动机是：AI系统能够将自由叙述映射为结构化变量，但这种映射充满噪声、版本依赖且可能存在子群异质性，若不加以统计校准，直接使用映射数据会导致严重偏差且无法量化不确定性。AMV的核心思想是放弃“完美映射”的幻想，转而采用测量误差模型：将AI映射视为带误差的代理变量，通过稀疏但概率已知的结构化验证问题，在调查内部嵌入校准机制。具体地，每位受访者在完成AI访谈后，随机回答少量验证问题（例如时间使用调查中的具体活动记录或以死因叙述中的医学诊断）。估计器采用两步策略：第一步利用所有验证数据估计AI映射的系统偏差函数（全局校准），第二步利用该受访者自身的验证答案修正个体残余误差。论文严格推导了总体均值、比例、子群均值以及回归系数（映射值作为自变量或因变量）的渐近无偏估计量，并给出了与经典调查方差类似的解析方差公式。基于方差表达式，还提供了设计规划工具：给定目标标准误差，计算所需验证问题总数和每位受访者分配的验证问题数。实证部分通过三类仿真验证了方法的有效性：设计校准参数化模拟展示了在不同映射误差强度、验证问题质量和稀疏度下的性能行为；基于美国时间使用调查（ATUS）的半仿真案例说明了在真实数据分布下的实用性；CHAMPS死因叙述研究则展示在医疗领域应用中的潜力。实验结论表明，当AI映射误差中等以上时，AMV能以每位受访者4-6个验证问题的代价获得接近全结构化问卷的精度；但验证问题的质量是关键调节变量，且对回归系数估计的校正能力弱于均值估计。论文指出AMV将AI辅助访谈重新定义为调查测量问题而非自然语言处理问题，强调了统计设计与验证的重要性，为AI在调查中的负责任整合提供了可操作途径。**
