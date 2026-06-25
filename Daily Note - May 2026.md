---
source: https://luminous-mat-781.notion.site/Daily-Note-May-2026-358e8c0e89c580e08217f73417ecfb43?source=copy_link
---
# Daily Note - May 2026

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Daily Note - May 2026
- 方法：agent, generation, language, vision-language-model, reasoning, vision, reinforcement-learning, optimization
- 论文/报告：40 篇
- 📌 GLM-5V-Turbo: Toward a Native Foundation Model for Multimodal Agents 【Zhipu】
- 📌 SenseNova-U1: Unifying Multimodal Understanding and Generation with NEO-unify Architecture
- 📌 ⭐ Qwen-Scope: Turning Sparse Features into Development Tools for Large Language Models 【Qwen】
- 📌Gated DeltaNet-2: Decoupling Erase and Write in Linear Attention【NVIDIA】
- 📌 ⭐Gemini Embedding 2: A Native Multimodal Embedding Model from Gemini【Google DeepMind】
- The MiniMax-M2 Series: Mini Activations Unleashing Max Real-World Intelligence【MiniMax】
- LLaVA-OneVision-2: Towards Next-Generation Perceptual Intelligence
- Training Long-Context Vision-Language Models Effectively with Generalization Beyond 128K Context【ByteDance Seed】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## 📌 GLM-5V-Turbo: Toward a Native Foundation Model for Multimodal Agents 【Zhipu】

[https://arxiv.org/pdf/2604.26752](https://arxiv.org/pdf/2604.26752)

- GLM-5V-Turbo 旨在构建**原生多模态智能体基础模型**，将异构视觉输入（图像、视频、网页、GUI、文档）的深度感知与推理、规划、工具使用及执行能力统一整合，而非将视觉作为语言模型的辅助接口。跨域联合强化学习：**构建覆盖 30+ 任务类别的 RL 体系，深度融合感知（2D/3D 定位、OCR、视频）、推理（STEM、逻辑）与智能体能力（GUI 操作、工具使用）。相比 SFT，多任务 RL 表现出更弱的跨域干扰，实现 RefCOCO（+4.8%）、MVBench（+5.6%）、OSWorld（+4.9%）等多领域同步提升，并观察到推理行为的跨任务迁移。** 大规模 RL 基础设施：针对多模态智能体训练的长序列、变长度、异构验证需求，提出：** 统一任务与奖励抽象，解耦规则/模型验证器与主训练流程** 全流水线异步重叠（推理-奖励评估-批次构建-权重传输），支持基于完成数或时间阈值的早停 视觉模块细粒度内存管理（ViT 与投影器的定向重计算与 CPU 卸载） 拓扑感知分区与动态负载均衡，将上下文并行（CP）/张量并行（TP）分区前移至数据加载阶段，对长视频进行联合装箱（joint bin-packing） 分层优化策略：构建从元素感知 → GUI 定位 → 单步动作 → 轨迹级预测的层次化任务体系。**通过低级任务的稳定优化支撑高级能力**，避免直接端到端长程训练的不稳定性。
## 📌 SenseNova-U1: Unifying Multimodal Understanding and Generation with NEO-unify Architecture

[https://arxiv.org/pdf/2605.12500](https://arxiv.org/pdf/2605.12500)

- SenseNova-U1 基于NEO-unify架构，论文提出原生统一多模态范式（native unified multimodal paradigm），其核心创新包括： 近无损视觉接口：直接处理原始像素与文本，摒弃预训练VE和VAE 端到端联合建模：在单一架构中耦合自回归语言建模与像素空间流匹配（flow matching） 原生混合专家架构（MoT）：通过参数解耦的Understanding与Generation流，在共享注意力机制下实现协同优化 该架构将理解与生成为单一底层过程的两个协同视角（synergistic views of a single underlying process），从而在文本理解、视觉感知、知识推理、图像生成、编辑及交错式多模态生成等任务中实现统一的能力涌现。
## 📌 ⭐ Qwen-Scope: Turning Sparse Features into Development Tools for Large Language Models 【Qwen】

[https://arxiv.org/pdf/2605.11887](https://arxiv.org/pdf/2605.11887)

- 解决**大型语言模型（LLMs）内部决策过程不透明**的问题，以及**如何将稀疏自编码器（SAEs）从纯粹的事后分析工具转化为实用的模型开发接口**的问题。论文提出了Qwen-Scope，一个基于Qwen模型家族的开源SAE套件，通过以下四个方向将SAE特征转化为可重用的表示级接口： 推理时控制（Inference-time Steering）：通过特征方向干预控制语言、概念和偏好，无需修改模型权重； 评估分析（Evaluation Analysis）：利用激活特征作为基准测试冗余性和能力覆盖的表示级代理； 数据为中心的工作流（Data-centric Workflows）：支持多语言毒性分类和安全导向的数据合成； 后训练优化（Post-training Optimization）：将SAE衍生的信号整合到监督微调和强化学习目标中，以缓解不良行为。 简言之，该工作旨在证明SAEs不仅可以作为事后检验工具，更能成为连接模型内部机制与下游行为的可复用表示级接口，从而系统地诊断、控制、评估和改进大型语言模型。
## 📌Gated DeltaNet-2: Decoupling Erase and Write in Linear Attention【NVIDIA】

[https://arxiv.org/pdf/2605.22791](https://arxiv.org/pdf/2605.22791)

- 解决**线性注意力机制中固定大小循环状态的内存编辑限制问题。论文提出 Gated DeltaNet-2，通过Gated Delta Rule-2 解耦上述操作： 引入****通道级擦除门控 ****bt∈[0,1]dk 控制键侧读取/移除 引入通道级写入门控 wt∈[0,1]dv 控制值侧提交 保留KDA的通道级衰减 αt ，形成三重门控机制 该设计在保持线性时间复杂度和高效块级并行训练（chunkwise WY算法）的同时，****允许模型独立优化"忘记什么"与"记住什么"****，最终提升语言建模、常识推理及长上下文检索的整体性能。**
## 📌 ⭐Gemini Embedding 2: A Native Multimodal Embedding Model from Gemini【Google DeepMind】

[https://arxiv.org/pdf/2605.27295](https://arxiv.org/pdf/2605.27295)

- Gemini Embedding 2 通过以下方式解决上述问题： **原生多模态架构：直接处理原始视频、音频、图像和文本输入，无需中间模态转换** 统一向量空间：利用 Gemini 的基础能力，通过多任务多阶段对比学习，**将所有模态映射到单一的 3072 维共享嵌入空间** 深度模态交互：支持任意组合的交错输入（如图像+文本查询视频），捕捉模态间的复杂关系 零样本泛化：在保持文本基准（MTEB）最优性能的同时，在跨模态检索（MSCOCO、Vatex）和专用领域（天文、显微成像等）实现开箱即用的高性能 该模型旨在为 RAG（检索增强生成）、视频推荐、多模态搜索等下游应用提供统一、鲁棒、无需任务特定指令的嵌入基础设施。
## The MiniMax-M2 Series: Mini Activations Unleashing Max Real-World Intelligence【MiniMax】

[https://arxiv.org/pdf/2605.26494](https://arxiv.org/pdf/2605.26494)

- 论文提出通过Mixture-of-Experts (MoE) 架构实现"小型激活释放最大现实世界智能"（mini activations unleashing max real-world intelligence）的范式。具体而言，MiniMax-M2系列以仅 9.8B 的激活参数（总参数229.9B）处理上述挑战，通过三个协同进化的技术支柱实现高效能与强能力的平衡： 智能体数据管道：**构建可执行、可验证的大规模轨迹数据，覆盖代码、办公、推理等多领域**； Forge强化学习系统：**支持长程智能体轨迹的规模化训练，兼容白盒与黑盒智能体架构**； 自我进化机制：**模型自主调试训练流程并修改自身脚手架（scaffold）**，实现迭代式能力增长。
## LLaVA-OneVision-2: Towards Next-Generation Perceptual Intelligence

[https://arxiv.org/pdf/2605.25979](https://arxiv.org/pdf/2605.25979)

- 论文提出LLaVA-OneVision-2，通过编解码器流式token化（Codec-Stream Tokenization）重构视频观测范式： 比特流感知分配：将压缩视频视为连续比特成本流，依据P/B帧包大小动态划分自适应GOP，使token密度跟随比特成本-残差配置文件变化 运动残差选择：利用运动向量与亮度残差联合评分，将显著空间证据打包为紧凑的I/P视觉画布 统一时空坐标：通过共享3D RoPE将编解码器画布、采样帧与图像置于统一时空坐标系，支持跨模态的组可见注意力（Group-Visible Attention） 该方法在固定token预算下，对长视频事件理解（如JumpScore基准mAP 达74.9，较Qwen3-VL-8B提升44.8点）与细粒度时空定位任务实现了显著性能提升。
## Training Long-Context Vision-Language Models Effectively with Generalization Beyond 128K Context【ByteDance Seed】

[https://arxiv.org/pdf/2605.13831](https://arxiv.org/pdf/2605.13831)

- 解决**长上下文视觉语言模型（Long-Context Vision-Language Models, LCVLMs）训练方案设计中的关键空白**，特别是针对如何有效构建和平衡多模态长上下文训练数据的问题。通过基于Qwen2.5-VL-7B的受控实验（固定5B token预算），研究得出三项核心发现： **长文档VQA优于OCR转录：相比OCR转录任务（性能下降17.4%），长文档视觉问答（VQA）能提供有效监督**，提升性能5.1%–6.3%，**因其包含指令格式和从信息提取到数值推理的任务多样性**。 **平衡长度分布优于目标长度聚焦**：覆盖32K–128K的自然平衡分布（pool-native）显著优于集中于128K附近的长偏分布（long-biased），表明长上下文能力需要在多样长度和位置上学习可泛化的关键信息检索。 检索密集型混合与短上下文保留： 最优任务比例为8:2的提取-推理混合，证实关键信息检索是主要瓶颈，需配合适度推理保持多样性 纯长文档VQA数据可在很大程度上保留短上下文能力，无需像LLM训练那样混合短数据
## Large Language Models are Universal Reasoners for Visual Generation【Unified Model, Apple】

[https://arxiv.org/pdf/2605.04040](https://arxiv.org/pdf/2605.04040)

- 论文提出UniReasoner框架，通过Draft-Evaluate-Diffuse三阶段流程，将LLM重新定位为通用推理器（Universal Reasoner）： 视觉草图（Draft）：生成离散视觉令牌作为粗粒度空间规划，提供具体场景锚点； **grounded评估（Evaluation）**：对草图进行自批判，生成诊断文本明确指代提示与草图的不一致； 联合扩散（Diffuse）：将提示、视觉草图与评估文本共同作为条件，利用显式纠错信号指导生成。 该方法在不修改扩散骨干网络的前提下，将LLM的理解优势转化为生成时的结构化控制信号，显著改善组合对齐与语义保真度（在GenEval上从0.79提升至0.88）。
## STARFlow2: Bridging Language Models and Normalizing Flows for Unified Multimodal Generation【Unified Model, Apple】

[https://arxiv.org/pdf/2605.08029](https://arxiv.org/pdf/2605.08029)

- 论文提出STARFlow2，其核心创新在于： 采用自回归归一化流（Autoregressive Normalizing Flows）作为统一生成范式 此类流模型由因果Transformer参数化，与LLM共享相同的因果掩码、KV缓存机制与从左到右的结构，**但输出的是连续潜变量的仿射变换参数而非离散token logits**，从而在**保证连续、单遍、纯因果生成（满足D2、D3）的同时，避免了扩散模型的迭代去噪**。 提出Pretzel架构实现垂直融合 通过垂直交错（vertically interleaving）冻结的预训练VLM流与可训练的TARFlow流，并利用残差跳跃连接（residual skip connections）在每一位置交换信息，既保留了VLM的预训练理解能力（满足D1），又使视觉生成能够利用VLM的高级语义表示，实现了真正的架构统一。 支持缓存友好的交错生成 由于文本与视觉潜变量均在同一因果框架下生成，生成的图像可直接进入KV缓存作为后续生成的上下文，无需重新编码，从而支持高效的文本-图像交错生成与多轮编辑。
## Awaking Spatial Intelligence in Unified Multimodal Understanding and Generation【Unified Model, JingDong】

[https://arxiv.org/pdf/2605.04128](https://arxiv.org/pdf/2605.04128)

- 解决**统一多模态模型中视觉理解、生成与编辑任务间协同不足，以及现有系统缺乏强空间智能**的核心问题。论文提出JoyAI-Image框架，通过以下机制建立双向协作范式： 理解驱动生成：利用空间增强的多模态大语言模型（MLLM）提供语义丰富且空间位置明确的条件信号，指导扩散模型进行高保真合成与编辑 生成辅助理解：将几何有意义的编辑（如相机视角变换、物体旋转/平移）作为"思维链"式的视觉证据，反哺空间推理与下游任务（如"Thinking with Novel Views"范式） 统一空间监督：**通过OpenSpatial数据引擎构建面向空间测量、空间关系、相机感知、多视图一致性等任务的专项训练数据**，将空间意识注入整个训练流程 该设计使模型超越了通用视觉能力，向更强的空间智能迈进，为视觉-语言-动作系统及世界模型等下游应用奠定基础。
## Uni-Synergy: Bridging Understanding and Generation for Personalized Reasoning via Co-operative Reinforcement Learning【Unified Model】

[https://arxiv.org/pdf/2605.10445](https://arxiv.org/pdf/2605.10445)

- 论文提出 Sync-R1 框架，通过协同强化学习（Sync-GRPO）建立显式的双向推理循环： Phase I（个性化理解）：**从异构输入中提取细粒度概念知识** Phase II（上下文引导生成）：**基于理解结果构建复合提示，指导图像生成** 统一奖励机制：评估理解输出的保真度和生成质量，实现互惠增强 同时引入**动态组缩放（Dynamic Group Scaling, DGS）**策略，通过自适应过滤低潜力轨迹降低梯度方差，解决多模态RL的计算瓶颈。
## AlphaGRPO: Unlocking Self-Reflective Multimodal Generation in UMMs via Decompositional Verifiable Reward【Unified Model】

[https://arxiv.org/pdf/2605.12495](https://arxiv.org/pdf/2605.12495)

- 解决**如何在不依赖额外冷启动阶段的情况下，通过强化学习有效激活统一多模态模型（UMMs）的固有推理能力以提升多模态生成质量**的核心问题。1. 统一轨迹建模（Unified Trajectory） 将多模态生成视为统一轨迹 τ=(y,z1→z0)，其中 y 为自回归（AR）生成的推理文本，z 为扩散生成的图像。通过 GRPO 对文本与图像生成进行端到端联合优化，共享组内优势（group advantage）信号。 2. 分解式可验证奖励（DVReward） 针对整体标量奖励（如 VIEScore）判别力不足、易过拟合的问题，提出： 请求分解：利用 LLM 将复杂提示分解为原子化的语义（实体、属性、空间关系等）与质量（几何、纹理、物理等）验证问题； 置信度评分：通过通用 MLLM（Qwen3VL）计算各问题"Yes/No"的概率比值作为连续分数，最终奖励为几何均值 r(z)=v¯sem⋅v¯qua ，提供细粒度、可解释的监督信号。 3. 假阳性修正（False-Positive Rectification, FPR） 在自反思精化任务中，强制将未改进初始图像的轨迹奖励设为组内最小值，防止 GRPO 为退化结果分配正优势，严格抑制模型退化。
## Semantic Generative Tuning for Unified Multimodal Models【Unified Model, Tencent】

[https://arxiv.org/pdf/2605.18714](https://arxiv.org/pdf/2605.18714)

- 论文提出Semantic Generative Tuning (SGT)范式，将生成调优的代理任务从像素空间转移到语义空间： 利用图像分割等高级语义任务作为生成代理 提供结构化的视觉监督信号，弥合稀疏文本信号与密集RGB信号之间的鸿沟 在不引入外部知识的情况下，通过改善特征线性可分离性和优化跨模态注意力分配，实现对齐和协同 该范式旨在建立统一的语义表示空间，使视觉理解和生成能够真正相互增强，而非简单共存。
## Lance: Unified Multimodal Modeling by Multi-Task Synergy【Unified Model, ByteDance】

[https://arxiv.org/pdf/2605.18678](https://arxiv.org/pdf/2605.18678)

- 如何在一个轻量级原生框架内同时支持图像与视频的理解、生成与编辑任务？传统统一模型要么采用单一表征导致语义推理与生成质量难以平衡，要么采用解耦表征但增加架构复杂度。Lance 通过**双路混合专家架构（dual-stream mixture-of-experts）在统一交错多模态序列中实现联合上下文学习，同时解耦理解与生成路径**。Lance 提出**分阶段多任务训练范式**，将 X2T（理解）、X2I（图像生成）、X2V（视频生成）任务家族统一建模，通过能力导向目标与自适应数据调度，强化跨任务协同而非简单能力叠加。
## WinTok: A Win-Win Hybrid Tokenizer via Decomposing Visual Understanding and Generation with Transferable Tokens【Unified Model】

[https://arxiv.org/pdf/2605.18115](https://arxiv.org/pdf/2605.18115)

- 解决**统一视觉分词器（unified visual tokenizer）中视觉理解与视觉生成的表示冲突问题**。论文提出WinTok，一种基于可迁移token（transferable tokens）的混合分词器： 显式解耦目标：引入**可学习的语义token（learnable semantic tokens）与像素token（pixel tokens）两类token**。像素token负责捕获局部细节用于生成，而语义token通过不对称token蒸馏（asymmetric token distillation）从视觉基础模型（如SigLIP2、DINOv2）继承判别性语义知识，用于理解任务。 统一编码器内的协同：**两类token在统一的Vision Transformer编码器内交互，既保持各自表征的独立性，又通过上下文整合实现互补**，从而在单一架构内实现理解与生成的双赢（win-win performance），避免了传统统一分词器的优化冲突。 实验表明，WinTok在ImageNet-1K分类准确率上较强基线UniTok提升11.2%（达到82.0%），同时保持具有竞争力的重建质量（rFID 0.41），且仅需50M开源训练数据，显著少于其他统一分词器所需的十亿级数据。
## Uni-Edit: Intelligent Editing Is A General Task For Unified Model Tuning【Unified Model】

[https://arxiv.org/pdf/2605.21487](https://arxiv.org/pdf/2605.21487)

- 解决**统一多模态模型（Unified Multimodal Models, UMMs）在同时提升图像理解、生成和编辑能力时面临的任务冲突与训练范式复杂性问题**。Uni-Edit 为打破上述范式，论文提出将智能图像编辑（Intelligent Editing）作为UMM调优的通用任务（General Task），通过以下机制解决问题： 指令重构：将多样化的VQA（Visual Question Answering）数据自动转换为推理密集型编辑指令，嵌入问题和嵌套逻辑，强制模型在编辑前先解决底层理解问题 数据构建：构建Uni-Edit-148k数据集，涵盖数学、OCR、描述、属性识别、空间推理和世界知识等七大类别，确保覆盖理解任务所需的广泛知识 统一训练：仅需单一任务、单一数据集、单一训练阶段，即可同时提升理解、生成和编辑三种能力，无需复杂的数据平衡策略.
## Toward Native Multimodal Modeling: A Roadmap【Unified Model, Tencent Youtu】

[https://arxiv.org/pdf/2605.25343](https://arxiv.org/pdf/2605.25343)

- 解决**原生多模态建模（Native Multimodal Modeling, NMM）领域缺乏系统化、形式化设计框架**的问题。该论文通过**形式化定义、系统分类和全栈技术路线图**，为NMM领域提供了从"非原生拼接"到"原生统一"范式转变所需的结构化指导，以支持真正原生的世界模型（world models）开发。
## Uni-OPD: Unifying On-Policy Distillation with a Dual-Perspective Recipe【Policy Distillation, Tencent Hunyuan】

[https://arxiv.org/pdf/2605.03677](https://arxiv.org/pdf/2605.03677)

- 论文提出 Uni-OPD 框架，通过双视角优化策略（Dual-Perspective Recipe）进行系统性改进： 学生侧：采用离线难度感知（offline difficulty-aware）与在线正确性感知（online correctness-aware）的联合数据平衡策略，促进对信息性状态的充分探索； 教师侧：引入结果引导的边际校准机制（outcome-guided margin calibration），通过强制token级聚合回报与结果奖励保持顺序一致性，恢复教师监督的可靠性。 该框架在数学推理、代码生成、逻辑推理、文档理解等5个领域的16个基准测试上验证了有效性，涵盖单教师/多教师蒸馏、强到弱蒸馏及跨模态蒸馏等多样化场景。
## Co-Evolving Policy Distillation【Policy Distillation, JingDong】

[https://arxiv.org/pdf/2604.27083](https://arxiv.org/pdf/2604.27083)

- 论文提出**Co-Evolving Policy Distillation (CoPD)**，通过**并行训练分支、交替进行领域特定RLVR与双向互蒸馏（mutual OPD）**，使各分支在深化专业能力的同时保持行为一致性，实现多能力的"全合一"（all-in-one）高效整合。
## The Many Faces of On-Policy Distillation: Pitfalls, Mechanisms, and Fixes【Policy Distillation】

[https://arxiv.org/pdf/2605.11182](https://arxiv.org/pdf/2605.11182)

- 系统性地研究**On-Policy Distillation (OPD)** 与 **On-Policy Self-Distillation (OPSD)** 在大型语言模型后训练中的**有效性边界、失败机制及稳定化策略**。关键发现：任务依赖的有效性边界 数学推理场景： OPD高度敏感于教师选择与损失函数设计，易出现响应长度爆炸、重复生成（repetition collapse）及教师-学生分布不匹配。 OPSD在实例特定PI（如具体题目的正确答案）条件下完全失效，无法通过自蒸馏提升推理能力。 系统提示内化与对齐场景： OPSD在PI代表共享潜在规则（如固定系统提示、风格偏好、角色设定）时表现优异，收敛速度与样本效率均优于GRPO/PPO等强化学习方法。 可有效实现推理压缩（缩短响应长度）与安全对齐，但存在最终性能受限于教师能力的天花板效应。
## 🧐Multi-Rollout On-Policy Distillation via Peer Successes and Failures【Policy Distillation, Microsoft】

[https://arxiv.org/pdf/2605.12652](https://arxiv.org/pdf/2605.12652)

- **现有On-Policy Distillation (OPD) 方法在利用学生采样轨迹时存在的结构性低效问题**。现有OPD方法通常独立地蒸馏每个采样轨迹，计算教师信号时完全忽略为同一提示生成的其他尝试（peer rollouts）。这种设计丢弃了rollout组的局部结构信息，而成功与失败的轨迹对比实际上蕴含丰富的实例特定证据。论文提出**Multi-Rollout On-Policy Distillation (MOPD)**，通过以下机制解决上述问题：Ci(x)=Cv(x,y(i),Y+−i(x),Y−−i(x))其中教师分布通过条件化于同伴上下文 Ci(x) 构建，具体包括： 正面同伴模仿：仅利用成功rollout作为有效推理模式的正面证据 对比式成功-失败条件化：同时利用成功和失败的rollout，使教师能够区分表面相似的正确与错误轨迹，在分支点提供更锐利的监督 通过将OPD从"独立轨迹模仿"转变为"比较式学习过程"，MOPD使教师不仅作为全局专家，更成为能够识别实例特定错误的局部诊断模型。
## Self-Distilled Agentic Reinforcement Learning【Policy Distillation, LongCat】

[https://arxiv.org/pdf/2605.15155](https://arxiv.org/pdf/2605.15155)

- **将On-Policy Self-Distillation (OPSD)应用于多轮智能体强化学习时所面临的不稳定性与信任不对称问题。论文提出SDAR (Self-Distilled Agentic Reinforcement Learning)，核心机制包括： 门控辅助目标：****将OPSD作为独立的辅助优化目标（而非修改RL优势），通过gt=σ(βΔt) 形式的sigmoid门控，对token级蒸馏强度进行自适应调节****。 不对称调制：****对正差距token（Δt>0 ）增强蒸馏，对负差距token（Δt<0 ）软衰减****，实现"信任教师背书，谨慎对待拒绝"的优化策略。 保持RL主干：严格保留基于验证器的GRPO损失作为无偏主优化目标，避免蒸馏干扰RL的收敛性。**
## Vision-OPD: Learning to See Fine Details for Multimodal LLMs via On-Policy Self-Distillation【Policy Distillation, Xiaohongshu】

[https://arxiv.org/pdf/2605.18740](https://arxiv.org/pdf/2605.18740)

- **多模态大语言模型（MLLMs）在细粒度视觉理解任务中的性能瓶颈**。论文提出Vision On-Policy Distillation（Vision-OPD），一种区域到全局的自蒸馏框架，通过以下机制解决上述问题： 双策略架构：**从同一MLLM实例化两个条件策略——以裁剪图为输入的教师策略（privileged regional perception）和以全图为输入的学生策略**。 策略内蒸馏：学生基于全图生成on-policy轨迹，在每个token位置最小化教师与学生分布之间的散度（如JSD）。 无需外部监督：不**依赖外部教师模型、真实标签、奖励验证器或推理时工具，仅通过模型自身的特权区域感知来监督全图推理**。 通过这种方式，Vision-OPD将"视觉缩放"（visual zooming）的收益内化为模型参数，使模型在单前向传播中即可有效捕捉全图中的细粒度证据。
## When Are Teacher Tokens Reliable? Position-Weighted On-Policy Self-Distillation for Reasoning【Policy Distillation】

[https://arxiv.org/pdf/2605.21606](https://arxiv.org/pdf/2605.21606)

- **基于策略的自蒸馏（On-Policy Self-Distillation, OPSD）中教师令牌（teacher tokens）可靠性判断**的问题，特别是在数学推理任务中。论文提出了**位置加权基于策略自蒸馏（PW-OPSD）**，**通过递增的Sigmoid位置权重函数替代均匀加权，在不增加额外教师计算成本的前提下，对早期高歧义令牌进行折扣**，同时保留后期令牌的完整监督强度，从而提升推理蒸馏的有效性。
## Not All Disagreement Is Learnable: Token Teachability in On-Policy Distillation【Policy Distillation】

[https://arxiv.org/pdf/2605.26844](https://arxiv.org/pdf/2605.26844)

- 解决**On-Policy Distillation (OPD)** 中**token级教师信号的选择与可学习性**问题。论文提出Teachability-Aware OPD (TA-OPD)，解决**如何在有限监督预算下最大化OPD效益的问题**： **仅对高可教性位置（通常仅占5%）应用OPD损失** 无需奖励模型或外部验证器，仅利用教师和学生的前向传播概率 在多个Qwen系列模型设置中证明，**token质量可以超越token数量，用极少的可教token达到或超越全token监督的效果** 简言之，论文将选择性OPD从"选择显著token"重新定义为"选择可学习的教师信号"，解决了OPD中监督信号与学习效果错配的核心问题。
## 🧐ROSD: Reflective On-Policy Self-Distillation for Language Model Reasoning across Domains【Policy Distillation】

[https://arxiv.org/pdf/2605.28014](https://arxiv.org/pdf/2605.28014)

- 论文提出 Reflective On-policy Self-Distillation (ROSD)，通过以下机制**将自蒸馏从"参考解模仿"转变为"选择性推理纠正"： 引入**自反思器（self-reflector）**提取纠正性思路（corrective idea）并定位首个错误片段（error quote） 基于错误定位进行局部蒸馏（localized distillation），仅对错误影响的后缀进行监督**，保留有效前缀 实验表明，ROSD 在提升领域内推理性能的同时，显著改善了跨领域泛化能力。
## StepOPSD: Step-Aware Online Preference Distillation for Agent Reinforcement Learning【Policy Distillation, Tencent】

[https://arxiv.org/pdf/2605.27140](https://arxiv.org/pdf/2605.27140)

- 该方法将蒸馏单元从完整轨迹转变为**动作中心的步骤段**（action-centered step segments），通过后处理阶段实现精准的信用再分配。
## UniSD: Towards a Unified Self-Distillation Framework for Large Language Models【Self Distillation】

[https://arxiv.org/pdf/2605.06597](https://arxiv.org/pdf/2605.06597)

- 论文提出 UniSD（Unified Self-Distillation framework），这是首个用于系统研究LLM自蒸馏的统一框架。该框架围绕三个互补维度组织： 监督可靠性：通过多教师一致性（multi-teacher agreement）和token级对比学习（token-level contrastive learning）识别可信信号 表示对齐：通过特征匹配（feature matching）将内部表征结构从教师传递给学生 训练稳定性：通过EMA教师（EMA teacher）和时间平滑，以及散度裁剪（divergence clipping）防止罕见token主导优化 最终，该研究旨在实现无需依赖更强外部教师模型的LLM自适应与自我改进。
## How Far Is Document Parsing from Solved? PureDocBench: A Source-Traceable Benchmark across Clean, Degraded, and Real-World Settings【OCR】

[https://arxiv.org/pdf/2605.07492](https://arxiv.org/pdf/2605.07492)

- OmniDocBench自2024年底公开以来，已有20+个模型发布，其训练流程可能直接或间接接触过该数据集，导致排行榜可信度随时间下降，存在训练数据泄露风险。论文提出了PureDocBench，一个**可追溯来源（Source-Traceable）**的程序化生成基准：构造方式：从HTML/CSS源文件渲染文档图像，并从同一源文件提取结构化标注（文本、表格HTML、公式LaTeX、阅读顺序），实现"所见即所得"的可验证性覆盖规模：10个领域、66个子类别、1,475页基础文档，每页提供三个版本（干净/数字退化/真实退化），共4,425张测试图像抗污染性：源文件在评估前新鲜生成，可随需重新生成（re-roll），有效缓解数据污染通过评估40个模型（涵盖Pipeline专家、端到端专家和通用VLM），论文证实文档解析远未解决（最佳模型仅约74分，模型间差距达44.6分），并揭示了不同架构在退化鲁棒性上的显著差异——这些发现仅在PureDocBench的三轨（Clean/Digital/Real）设计下才得以显现。
## MPDocBench-Parse: Benchmarking Practical Multi-page Document Parsing【OCR】

[https://arxiv.org/pdf/2605.22100](https://arxiv.org/pdf/2605.22100)

- **现有文档解析基准测试（benchmarks）无法充分满足实际多页文档解析场景需求**的问题。论文提出了MPDocBench-Parse，一个面向实际应用的多页文档解析基准，具备以下特点：多页端到端评估：**直接以完整PDF文档（平均7.5页）作为输入，评估结构化输出（Markdown/JSON）的整体质量**广泛的文档多样性：涵盖15种文档类型（如研究报告、教科书、报纸、杂志、手写文档、招股说明书等），支持中英文双语全面的评估协议：从内容保真度（文本、表格、公式识别；截断文本/表格合并；图表提取）和逻辑结构正确ness（阅读顺序、标题层次恢复）两个维度进行八项任务的系统评估实验结果表明，虽然现有模型在基础文本提取任务上表现良好，但**在语义连续性集成、视觉内容解析和层次结构恢复等方面**仍存在显著局限，而这些正是实际部署中的关键瓶颈。
## ModelLens: Finding the Best for Your Task from Myriads of Models

[https://arxiv.org/pdf/2605.07075](https://arxiv.org/pdf/2605.07075)

- 论文提出MODELLENS框架，**将模型推荐重新定义为基于模型-数据集-指标三元组（model–dataset–metric tuples）的排序学习问题**。其关键创新在于： 利用隐式能力图谱：通过聚合公共排行榜上分散且 noisy 的交互记录（1.62M条评估记录，涵盖47K模型与9.6K数据集），学习一个性能感知的潜在空间，其中模型与数据集的布局反映其实际能力匹配度而非表面文本相似性； 零样本泛化：通过ID-dropout机制与结构先验（模型规模、架构族）的结合，实现对全新发布模型与未基准测试数据集的冷启动推荐，无需在目标数据上执行任何候选模型的推理或微调； 异构评估感知：统一处理跨领域（NLP、视觉、语音等）与跨指标（Accuracy、BLEU、ROUGE等）的评估协议，通过组内标准化（within-group normalization）解决指标不可比问题。
## Post-training makes large language models less human-like

[https://arxiv.org/pdf/2605.07632](https://arxiv.org/pdf/2605.07632)

- 系统评估了大型语言模型（LLM）与人类行为的对齐度，并揭示了后训练（post-training）对模型行为保真度的负面影响。
## Assessing the Creativity of Large Language Models: Testing, Limits, and New Frontiers

[https://arxiv.org/pdf/2605.13450](https://arxiv.org/pdf/2605.13450)

- 最突出的问题是**科学创造力**（要求同时满足新颖性和多约束条件下的有用性）无法被现有测试（包括DAT、RAT、PACE、CDAT等）显著预测。这促使作者提出新的混合测试——**Divergent Remote Association Test (DRAT)，这是首个能显著预测LLMs科学创造力的自动化测试**。创造力发展趋势（2023-2026）：创造性写作（+1.22 z/年）与科学创造力（+1.00 z/年）持续提升，但发散性思维反而下降（-0.47 z/年），表明后训练可能损害输出多样性 DAT可被简单算法破解：贪婪选择最远词嵌入的算法得分超过人类与LLM，质疑其作为创造力测试的稳健性
## DeltaPrompts: Escaping the Zero-Delta Trap in Multimodal Distillation【NVIDIA】

[https://arxiv.org/pdf/2605.15532](https://arxiv.org/pdf/2605.15532)

- 论文提出**答案发散(Answer Divergence, ∆)**作为衡量提示蒸馏价值的核心指标——只有当教师和学生模型在特定提示上产生显著不同的答案分布时，该提示才具有蒸馏价值。 基于这一原则，论文构建了DELTAPROMPTS：一个包含20万合成提示的数据集，通过种子引导生成和技能引导生成的两阶段流程，主动针对学生模型的失败模式进行提示合成，确保非零发散性和高多样性，从而在图表推理、文档理解和感知中心推理任务上实现有效扩展。
## Learning to Learn from Multimodal Experience【Heuristic Learning】

[https://arxiv.org/pdf/2605.16857](https://arxiv.org/pdf/2605.16857)

- 解决**多模态智能体如何自动设计有效的记忆机制以从交互经验中持续学习**的问题。通过提出AUTOMMEMO框架，论文实现了：**将记忆机制表示为可执行的Python程序（定义存储、组织和检索逻辑）；**通过迭代评估-反思-变异的搜索过程自动发现有效程序；在GUI/Web导航和多模态视觉推理基准上验证自适应记忆设计相比固定基线的一致性能提升.
## LiteFrame: Efficient Vision Encoders Unlock Frame Scaling in Video LLMs【Google DeepMind】

[https://arxiv.org/pdf/2605.17260](https://arxiv.org/pdf/2605.17260)

- 论文提出**LiteFrame**，一种轻量级视频编码器，通过将时空token压缩内化至视觉骨干网络，而非作为事后处理步骤，同时解决LLM和视觉编码器的双重瓶颈。该方法以InternVL3-8B为基线，将304M参数的ViT-Large教师模型蒸馏为87M参数的轻量级学生模型。
## 🧐Post-Trained MoE Can Skip Half Experts via Self-Distillation

[https://arxiv.org/pdf/2605.18643](https://arxiv.org/pdf/2605.18643)

- 提出 **ZEDA（Zero-Expert Self-Distillation Adaptation）** 框架，旨在解决后训练静态MoE模型向动态MoE低成本迁移的问题。ZEDA通过三要素实现低成本架构转换： （1）零专家注入（Zero-Expert Injection） 向现有MoE层注入无参数的零输出专家（Zj(h)≡0 ），将候选专家池从N 扩展至N+NZ ，保持激活数K 不变。 **动态选择机制使部分token激活零专家，实际参与计算的正常专家数量减少**，实现token级自适应计算；（2）两阶段自蒸馏（Two-Stage Self-Distillation） 以原始post-trained MoE为冻结教师，通过监督微调（SFT）建立基础动态路由，**再经策略蒸馏（OPD）在学生自身rollout分布下进一步对齐教师分布**，恢复因架构改变导致的性能损失。 （3）组级辅助损失（Group Auxiliary Loss, LGA ） **将正常专家与零专家分为两组，仅在组间施加负载均衡约束，该策略保留正常专家间的原始路由结构，同时通过权重w可控地调节零专家激活率rZE（目标约50%）**。
## RISE: Reliable Improvement in Self-Evolving Vision-Language Models【Alibaba】

[https://arxiv.org/pdf/2605.20914](https://arxiv.org/pdf/2605.20914)

- 论文提出**RISE**（Reliable Improvement in Self-Evolving Vision-Language Models）框架，通过**细粒度角色交替**缩短反馈回路、通过**质量监督器**确保问题有效性和伪标签可靠性、以及通过**技能感知动态平衡**缓解模式崩溃，从而实现从无标签图像中进行更可靠、更有效的VLM自进化。
## DenseSteer: Steering Small Language Models towards Dense Math Reasoning

[https://arxiv.org/pdf/2605.29247](https://arxiv.org/pdf/2605.29247)

- 本文针对**小型语言模型（SLMs，≤3B参数）在多步数学推理任务中性能显著落后**的问题，提出了一种轻量级的推理时干预框架。论文提出无需训练的推理时激活引导方法，包含三个关键组件： （1）Dense-Rewriting（密集重写） 为避免直接使用大模型响应导致的分布不匹配（distribution shift），利用GPT-5.1对小模型自身的"稀疏"输出进行保守重写： 合并相邻冗余步骤，保持数学计算和最终答案不变 生成"密集"正样本（xpos ），与原始"稀疏"负样本（xneg ）形成对比对 重写后的样本保持域内特性（低负对数似然NLL），与模型内在生成先验对齐;2）引导向量提取 **基于少量对比样本（N=50 ），在选定层 ℓ 计算平均隐藏状态差异**;（3）推理时干预在解码过程中，向目标层（通常为中间层，如L16/L17）的残差流注入引导向量。

# Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reasoning
- 方法：agent, language, vision-language-model, reasoning, vision, reinforcement-learning, optimization, multimodal-learning
- 论文/报告：26 篇
- Formalizing Mathematics at Scale【Math, Meta】
- When to Vote, When to Rewrite: Disagreement-Guided Strategy Routing for Test-Time Scaling【Test-Time Scaling】
- Path-Lock Expert: Separating Reasoning Mode in Hybrid Thinking via Architecture-Level Separation
- RAG over Thinking Traces Can Improve Reasoning Tasks
- EvoLM: Self-Evolving Language Models through Co-Evolved Discriminative Rubrics【Rubrics】
- Perceptual Flow Network for Visually Grounded Reasoning【Visual Reasoning, Ant, AI Lab】
- Visual Latents Know More Than They Say: Unsilencing Latent Reasoning in MLLMs【Visual Reasoning】
- Fill the GAP: A Granular Alignment Paradigm for Visual Reasoning in Multimodal Large Language Models【Visual Reasoning, Qwen】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Formalizing Mathematics at Scale【Math, Meta】

[https://arxiv.org/pdf/2605.29955](https://arxiv.org/pdf/2605.29955)

- 论文提出 AutoformBot 框架，通过以下机制解决上述问题： 软件工程化协调：**将形式化视为协作软件工程问题，利用 git 分支、拉取请求审查（pull request review）和问题跟踪（issue tracking）等代理训练数据中充分存在的机制进行协调** 依赖感知调度：构建任务有向无环图（DAG），确保数学依赖关系（如定理 B 依赖定义 A）被尊重 分层反馈系统：通过追踪分析器（trace analyzer）从失败中学习，通过监督器（supervisor）在目标级别评估质量，防止错误累积 对抗性审查：建立多层审查机制，检测并阻止代理通过弱化陈述、隐藏公理或类型操纵来规避验证的企图 最终产物 ATLAS（Autoformalized Textbook Library At Scale）包含**来自 26 本教材的超过 45,000 个经机器验证的 Lean 4 声明**，证明研究生级别数学核心内容的规模化自动形式化在经济和技术上已可行。
## When to Vote, When to Rewrite: Disagreement-Guided Strategy Routing for Test-Time Scaling【Test-Time Scaling】

[https://arxiv.org/pdf/2604.26644](https://arxiv.org/pdf/2604.26644)

- **如何根据实例特征动态选择最适合的推理策略**，而非对所有实例应用统一的计算扩展策略。这需要一种轻量级的信号来区分实例难度并指导策略选择。通过实验发现：** **重写（Rewriting）**对困难实例显著有效，但对简单实例可能有害； **多数投票（Majority Voting）**在简单实例上表现优异，在困难实例上失效**； 输出分歧（模型多次采样结果的不一致性）与实例难度、预测正确性呈强相关性：困难实例分歧高，简单实例分歧低。 由此提出核心假设：**输出分歧可作为模型不确定性的轻量级代理信号，用于指导实例级策略路由**。
## Path-Lock Expert: Separating Reasoning Mode in Hybrid Thinking via Architecture-Level Separation

[https://arxiv.org/pdf/2604.27201](https://arxiv.org/pdf/2604.27201)

- 解决混合思维（hybrid-thinking）语言模型中的推理泄漏（reasoning leakage）问题，即**模型在显式控制为"无思考"（∖ no think）模式时，仍然无法完全抑制推理行为的现象**。双专家MLP：将每层单一MLP替换为两个结构相同的专家（\think专家与\no think专家）。路由条件监督微调：非活跃专家接收零梯度，共享骨干由两种模式共同更新；实现优化问题的部分块对角化，消除专家间的直接梯度干扰。
## RAG over Thinking Traces Can Improve Reasoning Tasks

[https://arxiv.org/pdf/2605.03344](https://arxiv.org/pdf/2605.03344)

- 传统RAG依赖通用文档（网页、教科书、学术论文）提供事实依据，在开放域问答中表现优异，但在数学推理、代码生成等任务中收益有限甚至产生负面影响。论文指出，**这一局限并非源于RAG机制本身，而是检索语料库与推理过程不匹配——传统文档提供"是什么"的事实知识，而推理任务需要"如何做"的过程知识**。论文提出**T3 （Transformation of Thinking Traces）**，一个基于思考轨迹的RAG框架： 语料库构建：收集强推理模型（如Gemini-2-thinking、QwQ-32B）在解决问题时生成的完整中间推理轨迹（包含尝试、错误、修正和解决方案），而非仅保留最终答案。 离线转换：使用轻量级模型（如Gemini-2-Flash-Lite）将冗长、噪声较多的原始轨迹（平均1,600-3,500词）转换为三种检索友好的结构化表示： Struct（结构规范化）：提取清晰的步骤式流程（平均239词） Semantic（语义蒸馏）：提供三级抽象表示（详细步骤→关键决策→核心洞察，平均261词） Reflect（反思模式）：聚焦常见错误、误导性直觉及规避策略（平均454词） 推理流程：采用标准的检索-生成（retrieve-then-generate）管道，将检索到的Top-3轨迹片段与查询拼接，指导下游模型（solver）推理。
## EvoLM: Self-Evolving Language Models through Co-Evolved Discriminative Rubrics【Rubrics】

[https://arxiv.org/pdf/2605.03871](https://arxiv.org/pdf/2605.03871)

- 论文提出EVOLM框架，通过以下机制解决上述问题： 评分标准生成器（Rubric Generator）：**将策略模型的隐式偏好提取为实例特定的自然语言评分标准** 判别性训练目标：基于变分推断最大化评分标准对偏好对的重建似然，即**优化评分标准使裁判模型对优选响应的打分显著高于次选响应** 时序对比构造偏好对：**利用策略模型早期检查点（历史版本）与当前版本的输出对比构建训练信号**，无需人类标注或外部标签 交替共进化训练：**策略模型与评分标准生成器通过交替更新实现协同进化——策略提升生成更难的对比样本，倒逼评分标准细化**；更精细的评分标准又提供更准确的奖励信号驱动策略进一步改进 该方法旨在建立一种不依赖人类标注、专有API或任务特定验证器的自进化训练范式，使语言模型能够自主地将其评估能力转化为可操作的、持续适应的奖励信号。
## Perceptual Flow Network for Visually Grounded Reasoning【Visual Reasoning, Ant, AI Lab】

[https://arxiv.org/pdf/2605.02730](https://arxiv.org/pdf/2605.02730)

- 论文提出**Perceptual Flow Network（PFlowNet）**，通过以下创新实现**可解释且高效的视觉推理：**感知流（Perceptual Flow）的形式化：将视觉推理轨迹显式建模为结构化潜变量 Z=(z0→z1→⋯→zK) ，包含规划状态（Planning State）与感知状态（Perceptual States），实现感知与推理的解耦； 变分强化微调策略：**摒弃与专家先验的刚性对齐，采用自参数化的变分分布 pθ(Z|X) 近似理想化感知行为的后验。通过多维奖励函数（结合对比式视觉质量奖励与推理效用奖励）与邻域几何塑形（Vicinal Geometric Shaping）**，在保持视觉可靠性的同时鼓励面向推理的感知行为探索； 理论保障：证明了PFlowNet在总变差距离（Total Variation Distance）意义下对标准MLE和专家引导RLVR的严格改进界限（Theorem 3.4）**。**
## Visual Latents Know More Than They Say: Unsilencing Latent Reasoning in MLLMs【Visual Reasoning】

[https://arxiv.org/pdf/2605.02735](https://arxiv.org/pdf/2605.02735)

- 论文提出在**推理时（inference-time）**分离这两个冲突目标，通过两阶段优化直接优化潜在变量而冻结主干参数： 阶段I：通过查询引导的对比潜在-视觉对齐（query-guided contrastive latent-visual alignment）预热潜在变量，提升语义质量并防止潜在坍缩 阶段II：通过置信度进展奖励（confidence-progression reward）强化潜在到答案的推理路径，激励沿潜在跨度的预测分布逐渐集中，确保预测路由通过潜在推理而非绕过它。
## Fill the GAP: A Granular Alignment Paradigm for Visual Reasoning in Multimodal Large Language Models【Visual Reasoning, Qwen】

[https://arxiv.org/pdf/2605.12374](https://arxiv.org/pdf/2605.12374)

- 解决**视觉潜在推理（Visual Latent Reasoning）在预归一化（Pre-Norm）多模态大语言模型（MLLMs）中的不稳定性问题**，特别是由**特征空间不匹配（Feature-Space Mismatch）**导致的性能瓶颈。论文提出粒度对齐范式（Granular Alignment Paradigm, GAP），通过三个层面的对齐机制实现稳定的视觉潜在推理： 特征级对齐（Feature-Level Alignment）：**通过轻量级PCA对齐的潜在头（PCA-Aligned Latent Head），将解码器输出映射到辅助图像视觉嵌入的主成分子空间，再重建为与输入兼容的视觉潜在表示**。数学上，潜在生成过程表示为： ct=Fθ(h¯t),v^t=Pkct+μ 其中Pk 为PCA基矩阵，μ 为均值向量，确保重建后的潜在向量v^t 回归输入视觉嵌入的范数体制。 上下文级对齐（Context-Level Alignment）：构建包含链式思维（Chain-of-Thought）、潜在跨度（<latent> span）和解析器描述（<parser>）的结构化训练数据，使连续潜在目标与可检查的辅助视觉监督相关联。 模型级对齐（Model-Level Alignment）：采用难度感知分配策略（Difficulty-Aware Assignment），仅对基础模型难以解决的示例（通过多次采样估计准确率a^≤τ ）施加潜在监督，避免在简单示例上引入不必要的噪声。
## Leveraging Latent Visual Reasoning in Silence【Visual Reasoning】

[https://arxiv.org/pdf/2605.18641](https://arxiv.org/pdf/2605.18641)

- 论文提出核心观点：**潜在视觉推理的价值不应以其在推理时是否持续存在来衡量，而应以其在****学习过程中对视觉 grounding 的指导作用****来衡量**。即使潜在token在推理时很少生成，它们仍可在"沉默中"（in silence）塑造更好的视觉理解和文本推理。
## Can MLLMs Reason Beyond Language? VisReason: A Comprehensive Benchmark for Vision-Centric Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2605.25364](https://arxiv.org/pdf/2605.25364)

- **如何准确评估多模态大语言模型（MLLMs）是否具备真正基于视觉证据的直接推理能力（即视觉中心推理），而非仅仅依赖语言中介或领域先验知识进行推理。通过VisReason的实验，论文试图揭示： 当前最先进的MLLMs与人类在视觉中心推理方面存在显著差距（人类准确率71.4%，最佳模型仅47.5%） 增加推理时计算资源（test-time scaling）或显式链式思维（CoT）提示对视觉中心推理的收益有限 模型在需要细粒度视觉定位、空间验证和结构化状态提取的任务上表现尤为薄弱 简言之，该研究旨在建立一个聚焦视觉证据本身的评估体系，以诊断现有MLLMs在多模态推理中的真实视觉基础能力，推动超越语言中介的多模态推理研究。**
## InterSketch: An Interleaved Reasoning Model with Self-correcting Visual Sketch and Stepwise Reward【Visual Reasoning】

[https://arxiv.org/pdf/2605.26520](https://arxiv.org/pdf/2605.26520)

- 论文提出InterSketch框架，通过以下机制实现突破： 自校正视觉草图：利用外部工具动态生成中间视觉草图作为认知支架（cognitive scaffolds），将被动感知转化为主动的交错VT-CoT； 反射机制：在冷启动阶段注入错误恢复样本，使模型具备自主诊断和修正工具调用错误的能力； **逐步奖励机制：在强化学习阶段根据工具调用类型设计类别特定的奖励函数，通过评估中间视觉状态的改进来密集化监督信号**，优化长程推理的效率与准确性。
## Bad Seeing or Bad Thinking? Rewarding Perception for Vision-Language Reasoning

[https://arxiv.org/pdf/2605.14054](https://arxiv.org/pdf/2605.14054)

- 论文提出了Modality-Aware Credit Assignment (MoCA) 框架，通过以下关键技术实现感知与推理的解耦和协同优化： 显式分解：强制模型以交错格式（<recognition>感知块与<think>推理块）生成输出，将感知过程外化为可监督的文本序列 感知验证（Perception Verification）：利用"蒙眼推理"代理（纯文本推理器）验证视觉描述是否足以回答问题，从而独立于推理结果评估感知保真度 结构化口头验证（Structured Verbal Verification）：通过结构化算法执行替代高方差的LLM判断，为自由形式答案提供可靠的奖励信号 模态感知信用分配：根据感知验证结果智能路由奖励信号——在感知正确但推理错误时保护感知token，在感知错误时加强惩罚，从而精确归因错误来源 该框架旨在实现感知与推理能力的同步提升，避免传统方法中的能力权衡，使单一VLM能够在广泛的任务谱（从感知密集型到推理密集型任务）上同时获得性能改进。
## 📌Generate, Filter, Control, Replay: A Comprehensive Survey of Rollout Strategies for LLM Reinforcement Learning

[https://arxiv.org/pdf/2605.02913](https://arxiv.org/pdf/2605.02913)

- 论文引入 **Generate–Filter–Control–Replay (GFCR) 生命周期分类法，将rollout流程解构为四个可组合的功能模块**，并提供： 统一的数学符号体系（涵盖轨迹、前缀、组采样、验证信号等） 跨领域的方法综合（数学、代码/SQL、多模态推理、工具智能体） 面向实践者的诊断索引（将常见rollout病理映射到具体模块和缓解措施） 通过将rollout设计提升为一等研究对象，该框架旨在使训练流程透明化、可比较化，并支撑可复现、计算高效且可信的rollout流水线构建。
## Post Reasoning: Improving the Performance of Non-Thinking Models at No Cost

[https://arxiv.org/pdf/2605.06165](https://arxiv.org/pdf/2605.06165)

- 论文提出**Post-Reasoning（后推理）**机制，通过重构生成顺序来解决上述矛盾： 生成顺序重构：与传统"先推理后回答"（ct→y ）不同，**Post-Reasoning采用"先回答后论证"（y→cp ）的结构**，即模型首先生成最终答案，随后生成解释性论证。 零成本推理增强：**通过设计特定的停止token（stop-token-based decoding），可以在生成答案后立即截断输出**，从而在不增加延迟、不消耗额外token的情况下获取最终答案，同时将后生成的推理作为可选的增强机制。 能力内化：通过监督式后推理微调（Supervised Post-Reason Tuning），使用掩码损失优化（仅对后推理token计算损失），将这种"回答后论证"的行为内化到模型权重中，实现跨领域泛化。
## Prune-OPD: Efficient and Reliable On-Policy Distillation for Long-Horizon Reasoning【Long Horizon】

[https://arxiv.org/pdf/2605.07804](https://arxiv.org/pdf/2605.07804)

- **长程推理任务中策略内蒸馏（On-Policy Distillation, OPD）的可靠性危机与计算效率问题**。该方法通过三个组件实现可靠性感知的训练预算分配： （1）局部兼容性监控 在每位置 τ 计算学生与教师的top-k重叠；**（2）累积可靠性衰减** 通过累积漂移事件计算单调递减的可靠性权重；**（3）动态响应预算** 定义有效可靠长度并根据批次命中率自适应调整最大生成长度。
## Implicit Compression Regularization: Concise Reasoning via Internal Shorter Distributions in RL Post-Training【Zhongguancun Academy】

[https://arxiv.org/pdf/2605.07316](https://arxiv.org/pdf/2605.07316)

- 解决RL后训练中LLM的过度思考（overthinking）问题——即模型在推理过程中生成不必要的长推理轨迹、冗余的中间步骤或过度的自我反思，导致推理成本增加甚至可能损害正确性。 具体而言，论文针对现有解决方案的局限性展开研究： **长度惩罚（Length Penalties）**：通过在RL奖励中添加长度相关项来惩罚长响应，但这可能降低模型准确性，导致"思考不足"（underthinking），且对系数调节敏感； **早期退出/截断策略（Early-exit or Truncation）**：假设推理轨迹的大部分可以安全丢弃，然而在困难或信息密集的问题上，后续步骤可能与正确性紧密耦合，强制截断会损害推理质量。 基于对训练动态的观察——即响应长度与正确性之间的相关性在压缩过程中从负变正，表明**短响应最初更可能正确，但随着训练进行会逐渐失去这一特性**——论文提出通过**隐式压缩正则化（Implicit Compression Regularization, ICR）**来实现**无需显式长度惩罚或推理截断的压缩机制**。该方法**利用on-policy rollout组内已经存在的最短正确响应作为自然的压缩目标，引导策略向简洁且正确的轨迹优化**。
## Dr. Post-Training: A Data Regularization Perspective on LLM Post-Training

[https://arxiv.org/pdf/2605.07063](https://arxiv.org/pdf/2605.07063)

- **如何有效利用稀缺的高保真目标数据与丰富但 imperfectly aligned（不完全对齐）的通用训练数据之间的根本张力**。论文提出Group-Wise Subset Update方法，**通过参数分组（如逐层）独立选择训练子集，提供更丰富的正则化强度设计空间**。同时，论文解决了在LLM规模下实现该框架的系统挑战： 设计定制的张量生命周期调度策略，在单次前向-后向传播内完成数据正则化更新，避免内存爆炸 提出高效的近似评分算法（如Per-Token Inner Product, Compressed Scoring），最小化计算开销 确保与LoRA、MeSO、激活检查点、梯度累积等现代内存节省技术的兼容性 简言之，该论文**通过数据正则化而非数据选择的视角，为LLM后训练提供了一个统一的理论框架和实用的算法族**，以更好地平衡目标忠实性与训练稳定性。
## Evolving-RL: End-to-End Optimization of Experience-Driven Self-Evolving Capability within Agents【Self-Evolving, Xiaohongshu】

[https://arxiv.org/pdf/2605.10663](https://arxiv.org/pdf/2605.10663)

- **经验驱动自进化智能体（experience-driven self-evolving agents）中****经验提取（extraction）与经验利用（utilization）能力****脱节。**论文提出Evolving-RL框架，通过以下机制解决上述问题： 以提取为中心的评估设计：将经验实例化为文本技能（textual skills），通过下游任务转移性能作为监督信号； 双信号协同进化：利用下游评估产生的奖励信号分别优化提取器（extractor）和求解器（solver），使两者在单一共享策略内协调共进； 稳定性控制：通过基于规则的过滤和负熵正则化解决联合训练中的评估噪声与熵爆炸问题，避免训练崩溃。 简言之，该论文旨在通过算法层面的端到端优化，突破现有方法在经验质量与利用能力之间的瓶颈，实现真正意义上的自进化能力内生增强（intrinsic self-evolving capability）。
## PopuLoRA: Co-Evolving LLM Populations for Reasoning Self-Play【Self-Evolving】

[https://arxiv.org/pdf/2605.16727](https://arxiv.org/pdf/2605.16727)

- 论文提出了PopuLoRA框架，其核心思路是**通过结构性不对称和群体协同进化来替代单智能体的自我评估**：角色分离：将题目生成者（教师）与求解者（学生）**分离为不同的LoRA适配器**，打破"自我评判"的闭环群体交叉评估：**通过教师与学生亚群体间的匹配对战（matchmaking）和TrueSkill评分机制，将难度信号从"自我估计"转变为"群体间信号"**LoRA权重空间进化：设计了一系列在适配器权重空间上的进化算子（突变与交叉），作为群体训练（PBT）的替换步骤，在秒级生成同秩子代适配器，维持群体多样性。通过这一机制，系统进入协同进化的军备竞赛状态：教师持续生成更复杂的题目以"击败"学生，学生则不断提升求解能力，从而避免模式坍缩，实现训练难度的持续提升和下游任务性能的显著改善。
## Agent-ValueBench: A Comprehensive Benchmark for Evaluating Agent Values

[https://arxiv.org/pdf/2605.10365](https://arxiv.org/pdf/2605.10365)

- **自主智能体（Autonomous Agents）价值观评估的系统性缺失问题。**提出 Agent-ValueBench 以填补上述空白，其设计直接回应三大挑战： 规模：394个可执行环境（跨16领域）+ 4,335个价值冲突任务（覆盖28个价值体系/332维度） 构建流程：端到端自动化流水线合成环境+任务，经人工心理学家逐实例审核 评估方法：每任务配备两极对齐的金色轨迹（golden trajectories）+ 基于行为锚点的轨迹级评分标准（rubric-based judge）**。**论文揭示了智能体价值观的运作机制： **价值潮汐（Value Tide）：跨模型存在同质化的共享价值轮廓，但存在可解释的反常洋流** 框架牵引（Harness Pull）：价值轮廓随框架切换发生非加性弯曲（同一框架对不同模型产生相反影响） 技能引导（Skill Steering）：嵌入式技能比提示词（prompt）对价值观的引导更深、更可靠 简言之，该论文试图建立从"模型对齐"向"框架对齐"和"技能引导"迁移的智能体价值观评估与调控新范式。
## Verifiable Process Rewards for Agentic Reasoning

[https://arxiv.org/pdf/2605.10325](https://arxiv.org/pdf/2605.10325)

- 论文提出**可验证过程奖励（Verifiable Process Rewards, VPR）**框架，其核心思路是： **密集验证：利用符号或算法验证器（如蒙特卡洛树搜索、约束求解器、推断引擎）对每一步中间动作进行客观检验** **过程级监督：将验证器转换为密集的回合级奖励信号，使每个中间决策都能获得即时、可靠的学习信号** 保持客观性：与基于学习的过程奖励模型（PRM）或"LLM作为评判者"的方法不同，VPR通过任务特定的验证器提供客观、无噪声的密集反馈。
## OpenDeepThink: Parallel Reasoning via Bradley--Terry Aggregation

[https://arxiv.org/pdf/2605.15177](https://arxiv.org/pdf/2605.15177)

- 通过成对Bradley-Terry比较机制，将LLM同时作为生成器和评判器，实现： **利用成对比较（pairwise comparison）替代逐点评分，将评判准确率从59%提升至86%** 通过Bradley-Terry模型聚合 pairwise 偏好为全局排序，内部化对手强度（opponent strength） 基于比较过程中产生的自然语言批判（critiques）进行反馈驱动的突变（feedback-driven mutation），允许彻底重写而非局部修正 在整个进化过程中完全摆脱对领域特定验证基础设施的依赖 简言之，该论文针对**"**如何在没有外部验证器的情况下，有效利用并行计算资源进行多候选推理与选择**"**这一核心问题，提出了一个基于群体进化和成对排序的通用框架。
## 🧐Agentic Systems as Boosting Weak Reasoning Models

[https://arxiv.org/pdf/2605.14163](https://arxiv.org/pdf/2605.14163)

- **如何通过推理时的委员会搜索机制（committee search），将弱推理语言模型（weak reasoning models）的能力提升至强模型水平？**论文将传统监督学习中的****提升（boosting）概念扩展到推理任务，提出推理时提升（inference-time boosting）**框架**：通过**多次调用弱模型生成候选方案（proposal），利用批评者（critics）和比较器（comparators）进行筛选与排序**，从而逼近强模型性能。与标准监督学习不同，推理任务需要生成本地中间步骤、识别有效步骤，并避免小错误累积导致最终失败。证明覆盖不等于可识别性（Proposition 1）。即使采样概率 α0>0 ，仅通过黑盒采样无法构造具有统一边缘的批评者或比较器。**可靠的放大需要额外的局部声音信号（执行、证明检查、类型检查、测试等）**。弱模型失败往往是选择失败而非生成失败：正确解常已存在于弱模型的提案池中，关键在于识别机制 推理时提升的瓶颈：受限于提案系统的盲点质量 B 和局部可识别性资源（β,σ ） 多样性价值：增加提案者多样性可暴露潜在正确解，但无法消除共享盲点 评估范式：提出基于角色的基准分解（Pass@1, Oracle best-of-k, 系统性能, Oracle-gap recovery），用于诊断瓶颈来源。
## 🧐Deep Pre-Alignment for VLMs

[https://arxiv.org/pdf/2605.15300](https://arxiv.org/pdf/2605.15300)

- 现有方法（如调整数据混合比例）仅能缓解症状，而未能解决**结构根因**——即**对齐负担被错误地放置在目标LLM内部**。为此，论文提出通过**深度预对齐（Deep Pre-Alignment）架构，****将模态对齐负担从目标LLM上游转移**至专门的感知器模块（perceiver VLM），使视觉特征在进入目标模型前已与文本空间深度对齐，从而保留LLM的原始能力用于高阶推理。**关键创新在于利用感知器内部的语言块（language blocks）渐进式桥接模态差距**： 语言先验利用：感知器包含在大规模因果语言建模任务上预训练的语言块，其隐藏状态在生成式语言处理过程中被结构性适应 渐进式细化：视觉特征通过感知器的深层语言块逐层处理，在到达目标LLM前已与文本空间几何兼容 表征同构：DPA构建的视觉特征空间不仅"接近"文本空间，更具备相似的拓扑结构（"块对角"子空间），使目标LLM能够使用其原生预训练电路处理图像，无需破坏性适应。
## Thinking as Compression: Your Reasoning Model is Secretly a Context Compressor【Baidu】

[https://arxiv.org/pdf/2605.28713](https://arxiv.org/pdf/2605.28713)

- 提出**Thinking as Compression (TaC)**范式，首次系统性地探索**将推理模型的思维轨迹（Thinking Traces）本身作为压缩后的上下文**，从而避免构建独立的压缩器。通过GRPO与三重奖励设计，将推理模型显式塑造为预算可控、防作弊的上下文压缩器。
## Zipping the Thought: When and How Compressed Reasoning Data Works in LLM Post-Training

[https://arxiv.org/pdf/2605.28008](https://arxiv.org/pdf/2605.28008)

- 研究了**压缩推理数据（Compressed Chain-of-Thought, CoT）在大语言模型后训练中的作用机制**，揭示了监督微调（SFT）与强化学习（RL）在处理压缩推理链时的不同特性，并为资源受限场景下的数据设计提供了实证指导。阐明SFT建立压缩推理的"模仿能力"，而RL通过探索实现"分解与重组"，支持RL能发现基础模型分布外新解的观点。**若数据有限，应优先选择Composed CoT（显式操作组合）并配合数据重复**；若追求极致推理效率且数据充足，可谨慎使用Implicit CoT但避免过度训练

# Reward Model

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Model
- 方法：agent, reasoning, vision, reinforcement-learning, optimization, multimodal-learning, multimodal-reasoning, deep-learning
- 论文/报告：18 篇
- 🧐RLVR Datasets and Where to Find Them: Tracing Data Lineage for Better Training Data【Tencent】
- Delay, Plateau, or Collapse: Evaluating the Impact of Systematic Verification Error on RLVR
- 🧐On the Implicit Reward Overfitting and the Low-rank Dynamics in RLVR
- ANCORA: Learning to Question via Manifold-Anchored Self-Play for Verifiable Reasoning
- 🧐Internalizing Outcome Supervision into Process Supervision: A New Paradigm for Reinforcement Learning for Reasoning【Alibaba】
- 🧐One-Way Policy Optimization for Self-Evolving LLMs【Qwen】
- 🧐Rubric-Grounded RL: Structured Judge Rewards for Generalizable Reasoning【Rubric】
- ⭐Rubric-based On-policy Distillation【Rubric, Tencent】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## 🧐RLVR Datasets and Where to Find Them: Tracing Data Lineage for Better Training Data【Tencent】

[https://arxiv.org/pdf/2605.26971](https://arxiv.org/pdf/2605.26971)

- 论文进一步探讨如何构建更优的 RLVR 数据集。通过**追溯原子来源（atomic sources），识别出被污染或低质量的实例，并基于**源级反事实归因（Source-level Counterfactual Attribution, SCA）**筛选出具有集中学习信号的数据，最终构建出去污染、高质量的数据集**（如 DAPO++）。总结而言，该工作旨在通过**建立系统性的数据谱系追溯框架（ATLAS）和质量评估体系（SCA），解决 RLVR 领域面临的数据不透明、污染泛滥和评估困难等问题**，为未来 RLVR 训练数据的构建、筛选和基准测试提供路线图。
## Delay, Plateau, or Collapse: Evaluating the Impact of Systematic Verification Error on RLVR

[https://arxiv.org/pdf/2605.02909](https://arxiv.org/pdf/2605.02909)

- 针对RLVR在复杂任务（如数学推理、代码生成）中日益广泛的应用，论文指出必须**超越样本级错误率**来理解验证器质量。需要分析验证器的具体错误模式（如是否基于可学习的触发器、是否与任务成功正交），因为不同的系统性错误模式会导致延迟、平台或崩溃等截然不同的训练轨迹。主要结论与启示 验证器质量评估：**不能依赖样本级错误率（如Youden's J:=TPR−FPR），必须分析错误模式的可学习性及其与任务成功的对齐程度**。 系统性FP的风险：与FN不同，系统性FP可能诱导奖励黑客，使模型强化与任务目标冲突的行为，导致不可逆崩溃。 实践建议：在复杂任务（代码、数学证明）中，**应优先消除非对称、与任务负相关的系统性假阳性**；考虑使用真值验证器进行周期性校准。
## 🧐On the Implicit Reward Overfitting and the Low-rank Dynamics in RLVR

[https://arxiv.org/pdf/2605.06523](https://arxiv.org/pdf/2605.06523)

- **深入理解RLVR（Reinforcement Learning with Verifiable Rewards）在微观参数层面的作用机制，特别是其低秩动态特性，并揭示RLVR训练中存在的隐式奖励过拟合现象。**
## ANCORA: Learning to Question via Manifold-Anchored Self-Play for Verifiable Reasoning

[https://arxiv.org/pdf/2604.27644](https://arxiv.org/pdf/2604.27644)

- 提出的ANCORA框架通过三个关键机制解决上述问题：双层组相对更新：将提议者优势（基于求解者通过率）与求解者优势耦合迭代自蒸馏SFT：在RL前将基础模型投影到其有效输出流形（pM≫1/N）UCB引导的课程DAG：仅通过严格过滤、新颖、求解者验证的规范扩展种子池该框架在Verus形式化验证任务上实现了从26.6%（SFT基线）到81.5%（测试时训练设置）的pass@1提升，并在MBPP和HumanEval上展现出跨域迁移能力。
## 🧐Internalizing Outcome Supervision into Process Supervision: A New Paradigm for Reinforcement Learning for Reasoning【Alibaba】

[https://arxiv.org/pdf/2605.05226](https://arxiv.org/pdf/2605.05226)

- **失败轨迹并非完全错误，而是包含可纠正的局部错误**。**通过让模型生成失败轨迹的最小编辑修复版本，并提取二者差异，即可将稀疏的结果反馈自动蒸馏为细粒度的 token 级过程监督信号，无需外部标注**。
## 🧐One-Way Policy Optimization for Self-Evolving LLMs【Qwen】

[https://arxiv.org/pdf/2605.22156](https://arxiv.org/pdf/2605.22156)

- 解决**强化学习与可验证奖励（RLVR）中大语言模型（LLM）训练过程中的优化方向冲突与稳定性-探索性权衡问题**。论文提出优化方向与更新幅度应解耦的核心原则：**验证器信号应唯一决定优化方向（梯度符号），而参考策略仅用于调节更新步长的大小**。现有方法未能实现这种解耦，导致参考策略的"经验"干扰了验证器提供的"地面真值"方向。为系统性解决上述问题，论文提出单向策略优化（OWPO），通过不对称重加权机制（对劣等偏差加速对齐、对优等偏差锁定收益）和迭代参考更新策略（棘轮效应），在保持训练稳定性的同时，允许策略持续突破先验限制，实现自我进化。
## 🧐Rubric-Grounded RL: Structured Judge Rewards for Generalizable Reasoning【Rubric】

[https://arxiv.org/pdf/2605.08061](https://arxiv.org/pdf/2605.08061)

- 论文提出Rubric-Grounded Reinforcement Learning（基于评分标准的强化学习），通过以下机制解决上述问题： **将奖励显式分解为加权的、可验证的多维标准（weighted, verifiable criteria）** 使用冻结的LLM作为评判者（judge），**基于隐藏的背景材料（grounding）对每个标准独立评分** 通过GRPO利用这些结构化评分作为部分信用优化信号 **策略在训练时学习满足基于背景的标准，但在推理时无需访问该背景，从而学习可泛化的推理模式而非简单检索** 该框架旨在提供高分辨率的奖励信号，使模型能够从部分满足标准的回答中学习，并将学到的推理行为迁移到未见过的任务领域。
## ⭐Rubric-based On-policy Distillation【Rubric, Tencent】

[https://arxiv.org/pdf/2605.07396](https://arxiv.org/pdf/2605.07396)

- 论文提出**基于评分标准的在线策略蒸馏（ROPD）框架，将监督信号从细粒度的token概率转移为结构化的语义评分标准（rubrics）**。评分标准诱导（Rubric Induction）：利用"评分器"（Rubricator）对比教师响应与学生rollouts，为每个提示动态生成特定领域的评分标准；**基于评分标准的验证（Rubric-based Verification）**：使用"验证器"（Verifier）根据这些共享标准对学生响应进行二进制判定，计算加权通过率作为奖励信号。通过这种方式，**ROPD实现了黑盒兼容的OPD，在仅依赖教师文本响应（无需logits）的情况下，不仅达到甚至超越了传统白盒OPD方法的性能，同时实现了高达10倍的样本效率提升，并支持跨架构蒸馏（无需tokenizer对齐）**。
## Auto-Rubric as Reward: From Implicit Preferences to Explicit Multimodal Generative Criteria【Rubric】

[https://arxiv.org/pdf/2605.08354](https://arxiv.org/pdf/2605.08354)

- 论文提出Auto-Rubric as Reward (ARR) 与 Rubric Policy Optimization (RPO) 的联合框架： 1. ARR（自动标准生成） 通过"生成-验证-精炼-结构化"流程，将冻结VLM内化的隐式偏好知识外化为显式、特定于提示的多维标准（rubrics）： **从少量偏好对（约100对）中自动生成可验证的评估标准** 涵盖语义保真度、空间一致性、审美和谐等维度 无需训练或微调评判器，实现零样本/少样本部署 2. RPO（标准策略优化） **利用ARR生成的结构化标准产生二元偏好决策（而非标量回归）作为奖励信号**，通过在线策略梯度训练生成模型。
## Reward Hacking in Rubric-Based Reinforcement Learning【Rubric, Scale AI】

[https://arxiv.org/pdf/2605.12474](https://arxiv.org/pdf/2605.12474)

- 解决**基于评分标准（rubric-based）的强化学习（RL）中的奖励黑客（reward hacking）问题**，即策略在训练过程中针对特定的验证器优化，导致在训练验证器上获得高分，但在更强的参考评估者或真实质量指标上表现不佳的现象。该论文试图建立一个诊断框架，以**区分真实的策略能力提升与通过利用验证器错误或评分标准设计缺陷获得的虚假增益**，并证明仅靠更强的验证器不足以防止奖励黑客，还需要更完善的评分标准设计。方法贡献 剥削率（Exploitation Rate）：量化新被认可的评分标准项中被参考面板一致拒绝的比例，作为验证器层面奖励黑客的指标。 自我内化差距（Self-Internalization Gap）：基于策略自身对数概率（比较"仅提示"与"评分标准条件"下的生成概率差异）的无验证器诊断工具，与参考面板奖励高度相关（r≥0.91 ），可用于检测训练何时停止真正改进并提供早期停止信号。
## 🧐RubricEM: Meta-RL with Rubric-guided Policy Decomposition beyond Verifiable Rewards【Rubric, Google】

[https://arxiv.org/pdf/2605.10899](https://arxiv.org/pdf/2605.10899)

- 论文在引言中明确提出了核心科学问题： "How can reinforcement learning train deep research agents beyond verifiable rewards, while enabling long-horizon credit assignment and learning from experience?" （如何在超越可验证奖励的范式下，通过RL训练深度研究代理，同时实现长程信用分配与经验学习？）论文提出RubricEM框架，**将评分标准（rubrics）从单纯的评估工具转变为贯穿整个RL流程的共享接口**： **策略执行：代理在规划阶段自生成rubrics，指导后续搜索与写作** **评判反馈：基于阶段性rubric提供细粒度的过程级奖励**（非仅终端评分） 经验记忆：**将judged trajectories蒸馏为rubric-grounded的反思文本，存入记忆库支持跨episode迁移与episode内精修 **通过**阶段性策略分解（Stagewise Policy Decomposition）与反射元策略训练（Reflection Meta-Policy Training）**的结合，RubricEM实现了在开放域、长程、非可验证场景下的有效RL训练。
## RubricRefine: Improving Tool-Use Agent Reliability with Training-Free Pre-Execution Refinement【Rubric】

[https://arxiv.org/pdf/2605.09730](https://arxiv.org/pdf/2605.09730)

- **代码模式（code-mode）工具使用代理在单次执行（single-attempt）场景下的可靠性问题**，特别是如何在不执行代码的情况下检测并修复"静默契约违规"（silent contract violations）。论文提出了 RubricRefine，一种无需训练的预执行语义契约验证方法： 评分标准生成：基于当前任务指令和工具注册表生成特定于实例的结构化评分标准（rubric），将任务正确性分解为可单独检查的契约标准（工具选择、输出契约、调用签名、数据来源） 静态评分与迭代修复：在零执行尝试的情况下，对候选代码进行显式契约检查，生成包含PASS/FAIL判断和修复指导的结构化反馈，驱动生成器迭代修复 早期停止机制：当评分达到最大值（10/10）时触发早期停止，确保仅在代码通过所有契约检查后才执行 该方法在M3ToolEval基准上实现了0.86的平均成功率（相比基线0.62提升+0.24），且在七种测试模型上均优于现有推理时基线，同时将延迟降低至最强非迭代替代方案的2.6倍以下。
## Step-wise Rubric Rewards for LLM Reasoning【Rubric, JINGDONG】

[https://arxiv.org/pdf/2605.17291](https://arxiv.org/pdf/2605.17291)

- 论文提出**Step-wise Rubrics as Rewards (SRaR)**框架，通过以下机制解决上述问题： 步骤归因评判：**使用LLM评判器将每个评分标准项映射到特定推理步骤**，保留步骤级、类型区分的奖励； 跨Rollout步骤标准化：对每步评分进行组内标准化，使只有步骤质量相对变化时才产生学习信号； 解耦优势估计器：将结果奖励与评分标准奖励分离归一化，防止通过操纵评分标准来虚增总体优势，从而抑制无意义的自我修正循环。
## ⭐⭐General Preference Reinforcement Learning【Rubric, Stanford】

[https://arxiv.org/pdf/2605.18721](https://arxiv.org/pdf/2605.18721)

- 解决**大语言模型（LLM）后训练（post-training）中在线强化学习（RL）与开放式任务对齐之间的结构性脱节问题。用一个预训练/已训练好的 frozen GPM，把每个 (prompt,response) 映射到 2k 维 preference representation；每两个 response 在每个二维 skew-symmetric subspace 里计算相对偏好分数；再把同组 rollout 的 pairwise 分数汇总成 per-dimension advantage，用来做 GRPO-style online RL。****它不是人手写的 rubric，比如“事实性/简洁性/安全性”明确对应某一维；而是从 preference pair 数据里学出来的 latent rubric axes。**
## ⭐Focal Reward: Balanced Reinforcement Learning under Rubric-Based Rewards【Rubrics, Ant】

[https://arxiv.org/pdf/2605.26579](https://arxiv.org/pdf/2605.26579)

- 解决**基于评分标准（rubric-based）的强化学习（RL）中存在的奖励极化不平衡（imbalanced reward polarization）问题**。论文提出了 Focal Reward 方法，通过以下机制解决上述问题： **利用逆向奖励投影（inverse reward projection）机制估计每个标准的饱和程度** **基于饱和程度动态校准奖励方向，自动重新加权（reweighting）**：降低对饱和标准的强调，增加对高改进潜力标准的训练信号 在不改变评分标准、评判模型和策略优化框架的前提下，仅通过奖励合成（reward synthesis）环节实现细粒度的平衡优化 该方法的理论基础表明，通过最大化局部信号-噪声比（local signal-to-noise ratio），可以减少配对信用分配错误，从而更有效地将优化压力导向仍有改进空间的标准维度。
## Co-ReAct: Rubrics as Step-Level Collaborators for ReAct Agents【Rubrics】

[https://arxiv.org/pdf/2605.23590](https://arxiv.org/pdf/2605.23590)

- 论文提出Co-ReAct框架，其核心创新在于： 步骤级规范性指导：将评分标准从"事后评估"转变为"步骤级协作者"，在每个决策点将评分标准注入智能体上下文，明确指定下一步行动在证据寻求、搜索、推理或自我评估中应覆盖的具体要求。 轨迹条件化与判别性：评分标准基于当前部分轨迹动态生成（而非一次性通用模板），并通过列表式GRPO训练优化Spearman等级相关系数奖励，确保评分标准能够区分优质与劣质行动，而非仅听起来合理。 注入-验证-重试机制：**在标准ReAct循环中增加"评分标准注入"和"验证"阶段，形成五元组（Rubric, Reason, Act, Verify, Observe），确保行动符合步骤级规范**，并在验证失败时提供针对性反馈进行重试。
## Reinforcement Learning with Robust Rubric Rewards【Rubrics, Huawei】

[https://arxiv.org/pdf/2605.30244](https://arxiv.org/pdf/2605.30244)

- 论文提出了**RLR3（Reinforcement Learning with Robust Rubric Rewards）**框架，其核心创新包括： 标准级验证路由：**将可验证标准路由至"LLM作为提取器+确定性验证器"路径，将模糊标准路由至"LLM作为评判器"路径**； 最小暴露策略：**通过掩码真实目标（对提取器）和源图像（对评判器）**，防止模型走捷径； 分层聚合机制：通过解耦归一化和层次化掩码，确保关键标准的失败无法被补充标准的良好表现补偿； GenRM的RLVR训练：使用可验证奖励训练生成式奖励模型，提高复杂指令遵循和结构化输出的可靠性。 实验表明，该方法在15个基准测试上持续优于传统RLVR，宏平均提升达4.7 个点，且通过受控审计确认了确定性验证和最小暴露策略对减少可利用假阳性的有效性。
## ⭐OmniVerifier-M1: Multimodal Meta-Verifier with Explicit Structured Recalibration

[https://arxiv.org/pdf/2605.28805](https://arxiv.org/pdf/2605.28805)

- 针对多模态大语言模型中视觉结果验证的**可靠性**与**细粒度**不足问题，提出了**多模态元验证（Multimodal Meta-Verification）**框架。元验证（Meta-Verification）要求验证器提供判断理由（rationales），但文本化解释存在**奖励黑客（reward hacking）**风险且计算开销大。论文探索**如何有效整合元验证反馈以训练更可靠的多模态验证器**。把 verifier 从“True/False 打分器”升级成“**能画框定位错误 + 指导图像自我修正的 meta-verifier**”。

# Data

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data
- 方法：generation, language, vision-language-model, reasoning, vision, retrieval, multimodal-learning, vision-language
- 论文/报告：15 篇
- S^3-R1: Learning to Retrieve and Answer Step-by-Step with Synthetic Data【Google DeepMind】
- ⭐Diagnosing Capability Gaps in Fine-Tuning Data【Microsoft】
- Rethinking Data Curation in LLM Training: Online Reweighting Offers Better Generalization than Offline Methods
- ⭐DataArc-SynData-Toolkit: A Unified Closed-Loop Framework for Multi-Path, Multimodal, and Multilingual Data Synthesis
- G-Zero: Self-Play for Open-Ended Generation from Zero Data【Zero Data】
- 20/20 Vision Language Models: A Prescription for Better VLMs through Data Curation Alone
- SEED: Targeted Data Selection by Weighted Independent Set【ByteDance】
- Learning Relative Representations for Fine-Grained Multimodal Alignment with Limited Data
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## S^3-R1: Learning to Retrieve and Answer Step-by-Step with Synthetic Data【Google DeepMind】

[https://arxiv.org/pdf/2605.01248](https://arxiv.org/pdf/2605.01248)

- 现有基于 RL 的搜索代理（如 Search-R1、R1-Searcher）通常仅依赖**基于结果的稀疏奖励**（outcome-based sparse rewards），即仅根据最终答案的正确性分配奖励。**模型可能在中间检索步骤中执行了完美的搜索查询，但因最终合成答案时的微小错误而获得零奖励，从而无法有效学习高质量的搜索策略**。论文提出的 **S3-R1** 框架通过**合成数据生成与筛选管道**（Synthetic data generation and curation pipeline）结合**检索感知的密集奖励**（retrieval-aware dense rewards）来同时解决这些问题，使模型能够学习更有效的搜索与综合策略。
## ⭐Diagnosing Capability Gaps in Fine-Tuning Data【Microsoft】

[https://arxiv.org/pdf/2604.27547](https://arxiv.org/pdf/2604.27547)

- 论文提出 GOALCOVER 框架，**通过交互式目标分解（将高层次目标分解为原子化、独立可评估的子目标）、基于LLM的对齐评分（为每个训练样本-子目标对计算 A(xi,yi,s)∈[0,1]）以及自动缺口分析，使从业者能够在微调前识别并补救关键数据缺口**。主要贡献 结构化目标分解方法论：通过固定交互协议将复杂目标分解为原子、互斥、可独立评估的子目标。 LLM-based对齐评估：采用锚定评分标准产生可解释的对齐分数。 对抗性验证协议：控制腐蚀实验作为数据层面的消融研究，验证检测信号 grounded in 真实能力依赖。 目标条件合成数据生成：利用缺口信号和评估器解释生成针对性训练样本填补缺陷。 实用化诊断管道：为从业者提供微调前的具体、可操作的缺口反馈与修复建议。
## Rethinking Data Curation in LLM Training: Online Reweighting Offers Better Generalization than Offline Methods

[https://arxiv.org/pdf/2605.05227](https://arxiv.org/pdf/2605.05227)

- 现有数据管理方法（如数据选择、混合）普遍采用**离线范式**（Offline Paradigm），即在训练前基于代理模型静态地筛选或重采样数据。论文提出**ADAPT**（Adaptive Data reweighting for Pretraining and FineTuning），将数据管理重新定义为**在线样本级重加权**问题：将**数据选择（二元权重）和混合（领域级权重）统一为样本级分数函数 s(x;θ,Dval) 的特例**，通过损失加权动态调整样本贡献。
## ⭐DataArc-SynData-Toolkit: A Unified Closed-Loop Framework for Multi-Path, Multimodal, and Multilingual Data Synthesis

[https://arxiv.org/pdf/2605.08138](https://arxiv.org/pdf/2605.08138)

- 论文提出了 DataArc-SynData-Toolkit，一个配置驱动的端到端闭环框架，通过以下方式应对这些挑战： 提供简化的命令行界面（CLI）和直观的可视化界面，降低技术门槛 建立**统一、标准化的多源数据合成范式，确保数据可靠性和可重用性** 采用高度模块化的代理架构，原生支持**多路径、多模态和多语言数据合成 集成数据合成、质量控制和模型后训练评估**的完整闭环工作流.
## G-Zero: Self-Play for Open-Ended Generation from Zero Data【Zero Data】

[https://arxiv.org/pdf/2605.09959](https://arxiv.org/pdf/2605.09959)

- 论文提出G-Zero框架，其核心创新在于： 无需外部验证器的协同进化机制：通过生成器（Generator）与提议者（Proposer）的双模型协作，完全基于模型内部的分布动态（distributional dynamics）构建学习信号。 内在奖励信号 Hint-δ ：量化生成器在无提示（unassisted）与带自生成提示（hint-conditioned）状态下对同一响应的预测概率偏移。该信号同时捕捉查询难度与提示信息量，使提议者能够持续定位生成器的认知盲区（blind spots），而生成器则通过DPO学习将这些提示引导的改进内化为独立生成能力。 简言之，该工作旨在建立一条可扩展、鲁棒的零数据（zero-data）自进化路径，使模型能够在无外部人类标注或评判模型监督的情况下，通过纯粹的内生反馈实现连续自我提升。
## 20/20 Vision Language Models: A Prescription for Better VLMs through Data Curation Alone

[https://arxiv.org/pdf/2605.11405](https://arxiv.org/pdf/2605.11405)

- **在固定模型架构、训练配方和计算预算的前提下，仅通过数据策展（data curation）这一单一变量，能将视觉语言模型（VLM）的性能提升到何种程度？**论文旨在验证数据策展是否能成为构建更优VLMs的高杠杆工具——**即在不增加训练计算量（甚至减少计算量）的情况下，实现接近或达到前沿模型的性能**，并同时具备更强的可靠性、分布外（OOD）泛化能力和推理效率。论文构建了一个端到端的多模态数据策展流程，包含四个关键组件：联合图像-文本去重：基于DINOv2图像嵌入与文本n-gram包含度的级联策略，避免单一模态去重造成的过度删除或泄漏误判；多模态质量过滤：联合图像-文本信号剔除低质量样本，同时保留稀有但有用数据；目标分布匹配：调整数据混合比例以优化覆盖度与平衡性，而非保留原始经验分布；分层合成数据：结合任务无关合成数据（扩展覆盖）与任务特定合成数据（针对性提升特定能力）。
## SEED: Targeted Data Selection by Weighted Independent Set【ByteDance】

[https://arxiv.org/pdf/2605.15691](https://arxiv.org/pdf/2605.15691)

- SEED与现有工作的核心区别在于：**首次在统一图视角下，通过双边显著子空间校准节点权重、通过局部尺度归一化修正边结构，使加权独立集框架适用于大规模异构数据场景**（如跨域指令微调、视觉指令微调与语义分割）。论文提出SEED（Data SElection pipeline with WEighted InDependent Set），通过以下两个原则性改进优化WIS： 节点值校准（Node Value Calibration） 通过**双边显著子空间（Mutual Influence Subspace）*限制影响估计，仅保留在训练集和验证集上均显著的通道（即$|\mathcal{C}^| := {c \mid m_{\text{train}}[c] > \bar{m}{\text{train}}, m{\text{target}}[c] > \bar{m}_{\text{target}}}$），从而过滤噪声并基于任务相关信号计算节点权重。 局部尺度归一化（Local Scale Normalization） 根据每个节点的局部邻域密度$\sigma_i$（通过$k$近邻影响估计）动态调整边阈值$\tau_i = \max(\tau, \alpha \cdot \sigma_i)$，缓解跨域分布偏移导致的图不平衡，确保密集和稀疏区域均得到公平表示。
## Learning Relative Representations for Fine-Grained Multimodal Alignment with Limited Data

[https://arxiv.org/pdf/2605.16834](https://arxiv.org/pdf/2605.16834)

- 解决**在有限配对数据的事后多模态对齐（post-hoc multimodal alignment）中，****如何有效建模细粒度跨模态结构****的问题**。论文提出无投影锚点学习（Projection-free Anchor Learning, PAL），其核心思想是通过**相对表示（relative representations）**在标记级别（token-level）显式建模跨模态结构****： 引入**可学习的模态特定锚点（learnable modality-specific anchors）**作为参考点； 将视觉补丁和文本标记表示为它们与这些锚点的相似度（相对表示），而非绝对特征坐标； 通过训练锚点诱导匹配图像-文本对在跨模态间产生一致的相对表示模式，从而在不引入繁重投影层的情况下，实现细粒度的跨模态对齐。
## Learning-Zone Energy: Online Data Selection for Efficient RL Post-Training

[https://arxiv.org/pdf/2605.17003](https://arxiv.org/pdf/2605.17003)

- 解决强化学习（RL）后训练阶段中的**数据选择低效问题**。 具体而言，在基于群体相对策略优化（GRPO、DAPO等）的大语言模型数学推理训练中，现有方法通常在所有提示（prompt）上近乎均匀地分配计算资源（包括rollout生成和梯度更新预算）。这种均匀分配策略存在根本性缺陷： 已掌握的样本：部分提示已被模型可靠解决，继续训练几乎不产生有效的学习信号 过难的样本：部分提示远超模型当前能力，生成的rollout全部失败，无法提供可操作的梯度信息 资源浪费：大量计算预算被消耗在这些信息贫乏的提示组上，而非集中在最能促进策略改进的样本上 为解决此问题，论文**提出Learning-Zone Energy (LZE) 框架，其核心思想是将计算资源集中于模型的主动学习前沿（active learning frontier）——即那些既未被完全掌握也非完全不可解、正处于模型"学习区"内的提示组**。该方法通过定义一个融合初始难度锚点、结果不确定性和通过率动量的能量分数，实现对训练样本的在线、连续、分级筛选，从而在仅使用40%训练数据的情况下达到或超越全量数据基线的性能，并显著降低训练FLOPs（约36%）。
## MindLoom: Composing Thought Modes for Frontier-Level Reasoning Data Synthesis

[https://arxiv.org/pdf/2605.21630](https://arxiv.org/pdf/2605.21630)

- **如何系统性地合成具有前沿难度（frontier-level）且结构可控的推理训练数据**这一问题。核心概念：思维模式（Thought Modes） 论文**将推理问题难度重新定义为原子化知识-推理转换的累积**，提出思维模式 T=(Ssum,Sdet,Kgen,Kspec) ： Ssum /Sdet ：转换类型摘要与具体修改细节 Kgen ：通用可迁移知识（定理、公式） Kspec ：问题特定参数（边界、数值） 难度控制由此转化为可解释的组合操作：**通过选择、排列、组合不同的思维模式，可系统性调节问题复杂度**。
## MIRA: Mid-training Rubric Anchoring for Source-Aware Data Selection【IQuest】

[https://arxiv.org/pdf/2605.30288](https://arxiv.org/pdf/2605.30288)

- 解决**中期训练（Mid-training）阶段异构数据的选择问题**，具体而言是现有数据选择方法与中期训练数据特性之间的不匹配问题。论文提出**MIRA（Mid-training Rubric Anchoring）**框架，其核心创新在于： 源自适应的标准构建：**将标准（rubric）构建作为数据选择的一部分，而非依赖预设的全局标准** 分组发现与蒸馏：通过"自锚定标准发现"（self-anchored rubric discovery）为每个能力相干的数据源组诱导特定的质量维度，再将这些判断蒸馏为轻量级学生评分器 可靠性感知的聚合：通过源条件可靠性估计和源感知保留阈值，实现可扩展的全文过滤同时保持源多样性 简言之，论文解决了**如何在保持可扩展性的同时，为异构中期训练数据提供源特定的语义质量评估**这一核心问题。
## 🧐LLMSurgeon: Diagnosing Data Mixture of Large Language Models

[https://arxiv.org/pdf/2605.30348](https://arxiv.org/pdf/2605.30348)

- **在无法访问模型内部参数或原始训练数据的情况下，****如何仅通过目标大语言模型（LLM）生成的文本，事后推断（逆向恢复）其预训练语料库在领域级别的分布比例**。论文提出LLMSurgeon，将DMS形式化为标签偏移（Label Shift）假设下的约束线性逆问题： 偏差表征：**利用外部代理分类器在参考数据上计算软混淆矩阵** C∈RK×K ，其中 Cij=Ex∼pi[fϕ(x)j] 量化领域间的系统性混淆（如C++被误判为C的概率）。 中性采样：**通过设计中性提示（如"继续以下段落："）触发目标LLM的自然生成，获取样本 Xgen∼qπ(x) ，最小化后训练对齐带来的分布扭曲**。 逆问题求解：建立观测向量 p¯ 与真实混合比例 π 的线性关系 E[fϕ(x)]=C⊤π ，求解约束优化问题： π^=argminπ∈ΔK−1∥C⊤π−p¯∥22 通过数学反卷积纠正分类器偏差，恢复真实领域先验。
## Domain-Specific Data Synthesis for LLMs via Minimal Sufficient Representation Learning**【Vivo, Ant】**

[https://arxiv.org/pdf/2605.30039](https://arxiv.org/pdf/2605.30039)

- 解决**领域特定数据合成**中的一个核心难题：当**目标领域难以用自然语言显式描述或形式化定义时，如何基于有限的参考样例隐式地合成高质量、多样化的领域数据**，以支持大语言模型（LLMs）的领域适配。提出DOMINO（DOmain-specific data synthesis through MINimal sufficient representatiOn learning），通过以下机制从参考样例中学习最小充分表示 D∗ ： 隐式领域表示学习：采用Prompt Tuning优化领域级软令牌 D ，最大化参考数据似然（L1 ），确保表示的充分性（捕获领域本质信息）。 对比解耦机制：引入样例特定表示 S(i) ，通过对比损失（L2 ）强制分离领域共享模式与样例特有噪声，实现最小性（剔除冗余信息）。 联合优化目标：L=L1+λL2 ，在重构保真与信息瓶颈之间取得平衡。【我感觉不对啊，生成时应该把sample soft token做成可采样的形式，类似VAE的概念？】
## 🧐Demystifying Data Organization for Enhanced LLM Training【Microsoft】

[https://arxiv.org/pdf/2605.30334](https://arxiv.org/pdf/2605.30334)

- 探讨了**大语言模型（LLM）训练中的数据组织（Data Organization）问题**，提出了一套基于预计算评分的零开销数据排序框架。当前LLM训练通常采用**one-pass或few-epochs范式**（仅遍历数据1-几次），使得训练样本的**时间顺序**成为影响学习轨迹的关键因素。尽管数据选择（筛选高质量子集）已被广泛研究，但如何**策略性地安排数据呈现顺序**仍缺乏系统性指导。零开销分数重用：不同于传统方法需为排序额外计算成本，论文提出**直接复用数据选择阶段已产生的样本级分数（质量、难度等评分），通过置换操作 π 重新组织数据序列**。首次**系统性地形式化了LLM数据组织的指导原则**，证明了在不增加计算开销的前提下，通过智能重排数据顺序即可显著提升训练稳定性、模型泛化能力及下游任务性能，为高效LLM训练提供了新的优化维度。
## 🧐How Much Is a Dataset Worth? Scaling Laws, the Vendi Score, and Matrix Spectral Functions

[https://arxiv.org/pdf/2605.29448](https://arxiv.org/pdf/2605.29448)

- **如何准确且高效地评估训练数据集对于机器学习模型的价值?**论文建立**矩阵谱函数**（Matrix Spectral Functions）的统一理论开发**基于世俗方程（Secular Equation）的快速算法。**论文证明**数据价值不能由数据量、类别平衡或计算预算单独决定**。通过建立矩阵谱函数的理论框架、开发可扩展的优化算法，并识别出Facility Location等有效评估代理指标，为高效、可解释的数据集构建与筛选提供了新范式。

# Foundation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Foundation
- 方法：language, vision-language-model, reasoning, vision, retrieval, vision-language
- 论文/报告：9 篇
- Let ViT Speak: Generative Language-Image Pre-training【ByteDance】
- Optimizer-Model Consistency: Full Finetuning with the Same Optimizer as Pretraining Forgets Less【Apple】
- Muon is Not That Special: Random or Inverted Spectra Work Just as Well
- Can Muon Fine-tune Adam-Pretrained Models?
- From Generalist to Specialist Representation
- Improved Baselines with Representation Autoencoders【Adobe, Saining Xie】
- LLMs as Noisy Channels: A Shannon Perspective on Model Capacity and Scaling Laws【ByteDance Seed】
- ⭐⭐SeedER: Seed-and-Expand Retrieval from Knowledge Graphs
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Let ViT Speak: Generative Language-Image Pre-training【ByteDance】

[https://arxiv.org/pdf/2605.00809](https://arxiv.org/pdf/2605.00809)

- 解决**视觉-语言预训练（Vision-Language Pre-training, VLP）中存在的架构复杂性与目标不匹配问题。**论文提出GenLIP（Generative Language-Image Pre-training），倡导极简主义设计哲学： 单一Transformer架构：摒弃额外的文本编码器/解码器，让Vision Transformer（ViT）直接"说话"——即直接从视觉token生成语言token 统一的语言建模目标：仅使用标准的自回归语言建模目标（autoregressive language modeling），无需对比损失或复杂的批次构建 早期模态融合：通过Prefix-LM注意力机制，让图像token进行双向交互，文本token进行因果注意力，实现视觉与文本模态的早期深度融合 通过这种方式，GenLIP旨在建立一个更简单、更可扩展、且与MLLMs生成特性自然对齐的视觉编码器预训练新范式，在减少预训练数据需求（仅8B样本）的同时，达到或超越现有方法的性能。
## Optimizer-Model Consistency: Full Finetuning with the Same Optimizer as Pretraining Forgets Less【Apple】

[https://arxiv.org/pdf/2605.06654](https://arxiv.org/pdf/2605.06654)

- 在预训练-微调的范式下，如何在使用有限的高质量下游任务数据进行微调时，**在最大化学习新任务能力的同时，最小化对预训练阶段获得的通用语言能力的遗忘**。论文探究了不同优化器（如 AdamW、Muon、SGD）以及参数高效微调方法（如 LoRA）在 SFT 阶段对模型性能的影响，**发现使用与预训练相同（家族）的优化器进行全量微调，相比其他优化器或 LoRA，能够实现更优的帕累托前沿**——即在获得相同或更好下游任务性能的同时，显著减少对预训练知识的遗忘。**不同优化器通过特定的矩阵诱导范数对模型激活值产生正则化效应，从而塑造出不同的模型权重景观**； **当 SFT 使用的优化器与预训练一致时，权重更新结构更符合预训练知识保留所需的曲率特性，从而降低遗忘**。论文以 Muon 和 AdamW 为例，发现尽管 Muon 在预训练阶段表现优异，但在需要强推理能力的下游任务（如数学）微调后，其综合性能可能不如 AdamW，这源于 **Muon 更强的记忆倾向可能在小数据量 SFT 阶段损害模式学习**。
## Muon is Not That Special: Random or Inverted Spectra Work Just as Well

[https://arxiv.org/pdf/2605.11181](https://arxiv.org/pdf/2605.11181)

- 这篇论文挑战了谱优化器（如 Muon）性能源于严格几何预条件化的主流观点，论证了**精确的几何结构并非优化性能的关键驱动因素**。在随机特征模型中，**Muon 的优势并非源于全局几何更优，而是源于其最优步长保持恒定**，而 SGD 的最优步长高度振荡且难以调优。
## Can Muon Fine-tune Adam-Pretrained Models?

[https://arxiv.org/pdf/2605.10468](https://arxiv.org/pdf/2605.10468)

- 解决**优化器不匹配（optimizer mismatch）**问题，即在用Muon优化器微调Adam预训练模型时出现的性能下降现象。通过控制实验和理论分析，论文揭示了这种不匹配源于Adam与Muon**截然不同的隐式偏差（implicit biases）**——Adam倾向于收敛到最大范数（max-norm）解，而Muon倾向于收敛到谱范数（spectral-norm）解，导致两者预训练出的权重具有不同的结构特性（如稳定秩差异，见图2）。**论文提出约束更新幅度可缓解该问题。通过Low-Rank Adaptation（LoRA）冻结预训练权重并将更新限制在低秩子空间，实验证明LoRA-Muon能够与LoRA-Adam持平甚至超越其性能**（见表2-4），从而在保持Muon内存效率的同时，消除优化器不匹配带来的负面影响。
## From Generalist to Specialist Representation

[https://arxiv.org/pdf/2605.12733](https://arxiv.org/pdf/2605.12733)

- 如何从通用模型（generalist model）中识别并提取任务相关的专家表示（task-relevant specialist representation）的问题，特别是在完全非参数设置下（即不依赖干预、参数形式或结构约束）。 具体而言，论文致力于解决以下两个核心子问题： 时序任务结构的识别：如何在完全无监督的情况下，识别时间步与任务之间的结构关系，即使序列缺乏严格的时间依赖性（允许断开连接），且任务分配遵循任意复杂和交错的结构。 任务相关潜在表示的解耦：在每个时间步内，如何将任务相关的潜在变量与无关变量进行解耦（disentangle），仅通过简单的稀疏正则化，无需额外信息或参数约束。 该研究的核心动机在于：**可识别性（Identifiability）**是表示学习的终极极限——没有可识别性保证，即使拥有无限数据和计算能力，学习到的表示也可能与真实潜在变量任意偏离，导致任务相关变量与无关因素纠缠，从而限制智能体的性能上限并带来高风险。
## Improved Baselines with Representation Autoencoders【Adobe, Saining Xie】

[https://arxiv.org/pdf/2605.18324](https://arxiv.org/pdf/2605.18324)

- 提出**RAEv2**（Improved Representation Autoencoders），通过三项关键改进解决了原始RAE在重建质量、引导机制和训练效率方面的瓶颈，实现了扩散模型训练效率与生成质量的显著提升。原始Representation Autoencoders (RAE)存在三个局限： 重建性能不足：仅使用预训练编码器最终层特征导致细节丢失（rFID 0.60） 与CFG不兼容：需训练次级弱模型（AutoGuidance）进行引导，增加计算开销 训练收敛慢：达到同等生成质量需要大量训练周期。1. 广义表示编码器（Generalized RAE） 将编码器输出定义为最后K 层特征的聚合，而非单一最终层；2. RAE与REPA的互补机制 通过大规模实证分析（27种编码器）发现： **RAE提供语义丰富的潜空间（与全局语义LP强相关，|r|=0.81 ） REPA（Representation Alignment）正则化中间层空间结构（与局部距离相似性LDS强相关，|r|=0.89 ） 二者机制互补**，允许使用同一预训练表示同时作为： RAE的编码器输入 REPA的中间层对齐目标 强编码器（如DINOv3-L）在两者上均表现优异时，生成质量最佳。3. REPA作为x -prediction的内部引导。
## LLMs as Noisy Channels: A Shannon Perspective on Model Capacity and Scaling Laws【ByteDance Seed】

[https://arxiv.org/pdf/2605.23901](https://arxiv.org/pdf/2605.23901)

- 解决**现有大语言模型（LLM）扩展定律无法解释非单调性能退化现象**的问题。传统扩展定律（如OpenAI幂律、Chinchilla定律）假设模型性能随参数规模（N ）和训练数据量（D ）单调提升。**然而，近期观测到的灾难性过度训练、量化诱导退化（QiD） 和高斯噪声敏感性等现象呈现U形曲线：超过临界规模后，性能反而恶化。**现有扰动感知定律仅通过经验性惩罚项拟合曲线，缺乏统一理论解释。
## ⭐⭐SeedER: Seed-and-Expand Retrieval from Knowledge Graphs

[https://arxiv.org/pdf/2605.23753](https://arxiv.org/pdf/2605.23753)

- 论文提出SeedER（Seed-and-Expand Retrieval），通过以下方式解决上述问题： 种子阶段（Seeding）：**利用轻量级密集检索快速定位语义相关的核心节点作为"锚点"** 策略化扩展（Selective Expansion）：通过强化学习训练一个图感知策略，在严格预算控制下迭代选择有前景的邻域节点进行扩展，而非均匀扩展所有邻居 分解推理（Decomposed Reasoning）：**将全局组合推理分解为一系列可重用的局部决策**，实现高效的组合泛化 该方法旨在成为高效的第一阶段检索器，在紧凑的候选集中实现高召回率，为下游的重排序或生成模型提供高质量的图上下文。
## MONA: Muon Optimizer with Nesterov Acceleration for Scalable Language Model Training【Meituan】

[https://arxiv.org/pdf/2605.26842](https://arxiv.org/pdf/2605.26842)

- 论文提出MONA（Muon Optimizer with Nesterov Acceleration），通过以下方式解决上述问题： **正交化前注入曲率感知：将基于梯度差分的加速项 Ak （即 Ak=βaAk−1+(1−βa)(Gk−Gk−1) ）在正交化之前应用于原始梯度**，使动量缓冲区捕获曲率感知方向，再经Muon的谱范数约束处理。 保持几何结构：该设计既保留了Muon的谱范数正则化（spectral-norm regularization）特性，又通过加速项实现对尖锐极小值的显式逃逸（escape from sharp minima）。 简言之，论文旨在构建一种兼具Muon几何优势与二阶曲率感知能力的优化器，以**提升从1B到68B参数规模的混合专家（MoE）语言模型**在预训练和微调阶段的收敛速度与下游任务性能。

# Generation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Generation
- 方法：agent, generation, language, vision-language-model, reasoning, vision, stat-ml, vision-language
- 论文/报告：24 篇
- Visual Generation in the New Era: An Evolution from Atomic Mapping to Agentic World Modeling
- Representation Fréchet Loss for Visual Generation【OpenAI】
- A Benchmark for Interactive World Models with a Unified Action Generation Framework【World Model】
- WorldReasonBench: Human-Aligned Stress Testing of Video Generators as Future World-State Predictors【World Model】
- SCOPE: Simulating Cross-game Operations in Playable Environments for FPS World Models【World Model】
- WorldCraft: From Camera Navigation to Object Manipulation in Interactive Video World Models【World Model】
- ⭐Nano World Models: A Minimalist Implementation of Future Video Prediction【World Model】
- Gamma-World: Generative Multi-Agent World Modeling Beyond Two Players【World Model, NVIDIA】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Visual Generation in the New Era: An Evolution from Atomic Mapping to Agentic World Modeling

[https://arxiv.org/pdf/2604.28185](https://arxiv.org/pdf/2604.28185)

- 视觉生成领域从**原子级像素映射**向**智能体世界建模**的范式演进。
## Representation Fréchet Loss for Visual Generation【OpenAI】

[https://arxiv.org/pdf/2604.28190](https://arxiv.org/pdf/2604.28190)

- **如何将 Fréchet Distance (FD)——特别是其最知名的实例 FID (Fréchet Inception Distance)——从一个仅用于评估的指标转变为一个可直接优化的训练目标**，并探索这一转变对视觉生成模型训练和评估的深层影响。传统观点认为，将 FD 作为训练损失是不切实际的。这是因为： FD 的可靠估计需要大规模样本群体（例如 N=50,000 ）来准确估计分布的均值 μ 和协方差 Σ ；论文通过 **FD-loss** 方法解决了这一矛盾：**通过解耦群体规模（population size）与批次规模（batch size），采用特征队列（queue）或指数移动平均（EMA）来维护大规模统计量**，同时仅对当前批次计算梯度，使得在大规模数据上直接优化 FD 成为可能。
## A Benchmark for Interactive World Models with a Unified Action Generation Framework【World Model】

[https://arxiv.org/pdf/2605.03941](https://arxiv.org/pdf/2605.03941)

- 论文提出了 iWorld-Bench，一个专为交互式世界模型设计的综合评估基准，其核心贡献包括： 构建包含330k视频片段的多样化数据集，涵盖4种观测视角（无人机、无人车、行人、机器人）、9种户外天气条件、5种室内光照条件 提出**统一动作生成框架（Action Generation Framework），通过模态无关的编码方式统一表示81种基础动作**，支持文本、one-hot编码、相机参数等多种控制模态的公平比较 设计6类任务（4个难度等级的动作控制任务、记忆能力任务、相机跟随任务），共4,900个测试样本，从视觉生成质量、轨迹跟随精度、记忆能力三个维度建立9项评估指标。
## WorldReasonBench: Human-Aligned Stress Testing of Video Generators as Future World-State Predictors【World Model】

[https://arxiv.org/pdf/2605.10434](https://arxiv.org/pdf/2605.10434)

- **WorldReasonBench：包含436个精选测试用例，通过结构化QA注释评估四个推理维度（世界知识、人类中心推理、逻辑推理、信息推理）和22个子类别，测试模型是否能将观察到的初始状态正确推演为时间一致的未来序列**。 双组件评估方法： Process-aware Reasoning Verification：通过分阶段QA（状态、过程、保真度、机制）检测"结果正确但过程错误"（outcome hacking）的生成。 Multi-dimensional Quality Assessment：结合推理质量、时间一致性和视觉美学的连续评分，用于排序和奖励建模。WorldRewardBench：包含约6,000对专家标注的偏好数据，覆盖1,432个视频，支持成对和逐点奖励模型评估，确保自动指标与人类判断对齐（Spearman ρ=0.955 ）。 简言之，该论文将视频生成评估从像素合成质量重新定义为世界状态转移的正确性，并提供了首个系统测量视觉逼真度与世界推理能力之间持续差距的基准工具。
## SCOPE: Simulating Cross-game Operations in Playable Environments for FPS World Models【World Model】

[https://arxiv.org/pdf/2605.23345](https://arxiv.org/pdf/2605.23345)

- 论文提出SCOPE（Simulating Cross-game Operations in Playable Environments），通过以下方式解决上述问题：** 在每个Transformer块中插入像素级条件化模块，将特征重塑为逐像素时间序列**，使每个位置基于局部视觉内容独立计算动作响应 双路径架构：离散事件通过视觉查询交叉注意力（confine effects to in-scope regions），连续控制通过时序自注意力（model smooth ego-motion for out-of-scope generation） 引入CrossFPS数据集：**首个包含7款FPS游戏、69K片段、10自由度手柄遥测的多游戏数据集，通过帧级动作对齐和去偏置处理，学习跨游戏通用的视觉-动作映射** 该方法实现了无需分割标签的端到端Scope解耦，并支持对未见过场景的零样本迁移。
## WorldCraft: From Camera Navigation to Object Manipulation in Interactive Video World Models【World Model】

[https://arxiv.org/pdf/2605.25077](https://arxiv.org/pdf/2605.25077)

- 为解决上述问题，论文提出了 **WorldCraft** 框架，通过**归一化世界轨迹（NWT）**解耦物体运动与相机运动****，通过**空间路径LoRA（SP-LoRA）**在不破坏相机控制的前提下注入轨迹控制能力****，并通过**轨迹锚定状态保持（TASP）**机制确保物体在视野外运动后仍能在正确位置重现。
## ⭐Nano World Models: A Minimalist Implementation of Future Video Prediction【World Model】

[https://arxiv.org/pdf/2605.23993](https://arxiv.org/pdf/2605.23993)

- 论文提出**Nano World Models**——一个基于扩散强制（Diffusion Forcing）的极简主义框架，通过统一的接口将生成目标、架构设计、动作条件机制、潜在观察空间、环境接口和评估流程模块化，从而为世界模型设计选择的可控、可复现和科学研究提供开放的实验平台。
## Gamma-World: Generative Multi-Agent World Modeling Beyond Two Players【World Model, NVIDIA】

[https://arxiv.org/pdf/2605.28816](https://arxiv.org/pdf/2605.28816)

- 解决**多智能体交互场景下的生成式世界模型建模问题，论文提出 γ -World，通过三项关键技术实现突破：**** Simplex Rotary Agent Encoding：将智能体表示为旋转角度空间中正则单形（regular simplex）的顶点，以无参数方式实现置换等价（permutation-equivalent）且可区分的身份编码，支持任意数量智能体（P≤V ）而无需重新训练****。 Sparse Hub Attention：引入可学习的 hub token 作为跨智能体通信中介，将注意力成本从 O(P2) 降至 O(P) ，实现线性可扩展性。 教师-学生蒸馏框架：将双向扩散教师模型蒸馏为块因果（block-causal）学生模型，结合 KV 缓存实现 24 FPS 的实时动作响应式生成。 实验表明，该方法在双人及四人虚拟游戏环境中均提升了视频保真度、动作可控性和跨智能体一致性，且能从双人训练数据零样本泛化到四人场景。**
## minWM: A Full-Stack Open-Source Framework for Real-Time Interactive Video World Models【World Model】

[https://arxiv.org/pdf/2605.30263](https://arxiv.org/pdf/2605.30263)

- 解决**将现有的高质量视频扩散基础模型（bidirectional T2V/TI2V模型）转换为实时交互式视频世界模型**所面临的全栈技术挑战。论文提出了**minWM**——一个全栈开源框架，**通过端到端流程（涵盖相机控制训练、AR扩散训练、因果ODE/一致性蒸馏、非对称DMD后训练及流式推理）将现有双向视频扩散模型转换为相机可控的少步自回归世界模型**，从而支持实时交互应用。
## Visual Implicit Autoregressive Modeling**【Tele AI】**

[https://arxiv.org/pdf/2605.01220](https://arxiv.org/pdf/2605.01220)

- 论文提出 Visual Implicit Autoregressive Modeling（VIAR），通过以下机制实现解决方案： 隐式均衡层替代显式堆叠：**将 VAR 的深层显式中间模块替换为单个隐式不动点层（implicit fixed-point layer），利用深度均衡模型（DEQ）的无穷深权重共享特性，将显式深度转化为迭代控制的计算量**。 恒定内存训练与灵活推理：采用 Jacobian-Free Backpropagation（JFB）实现训练时内存与深度无关（constant memory）；推理时通过调节每个尺度的固定点迭代次数（per-scale iteration knob），实现计算资源的自适应分配（如高分辨率尺度减少迭代，低分辨率尺度增加迭代）。 质量-效率权衡：在 ImageNet 256×256 上，VIAR 仅用 VAR 38.4% 的参数（770.9M vs 2010.0M）达到可比的生成质量（FID 2.16 vs 2.08），并通过迭代调度将峰值内存降低 42.0%（至 8.53 GB）、吞吐量提升 2.1 倍（至 32.08 images/s），无需重新训练即可在质量与效率间灵活切换。
## Persistent Visual Memory: Sustaining Perception for Deep Generation in LVLMs【AI Lab】

[https://arxiv.org/pdf/2605.00814](https://arxiv.org/pdf/2605.00814)

- 解决**自回归大型视觉语言模型（LVLMs）在深度生成过程中面临的"视觉信号稀释"（Visual Signal Dilution）问题**。论文提出**持久视觉记忆（Persistent Visual Memory, PVM）**模块，**通过建立与主网络并行的、距离无关的视觉检索路径，结构性隔离视觉信号免受文本历史膨胀的干扰**，从而在扩展生成过程中维持持续、按需的视觉感知。
## How to Guide Your Flow: Few-Step Alignment via Flow Map Reward Guidance

[https://arxiv.org/pdf/2604.27147](https://arxiv.org/pdf/2604.27147)

- **是否存在一个原则性的框架，能够在仅使用极少网络函数评估（NFEs）的情况下，对基于流的生成模型进行有效的奖励引导？**解决路径 为回答该问题，论文彻底背离了传统的奖励倾斜范式，提出： 确定性最优控制重构：将引导重新表述为确定性最优控制问题（而非随机最优控制），通过权衡奖励最大化与偏离基础流的代价来寻找最优控制策略。 流图的中心性：证明流图（flow map）Xt,1 自然出现在最优控制的闭式解中，其复合结构允许单个预训练流图同时负责精确积分和准确引导信号计算。 Flow Map Reward Guidance (FMRG)：基于此理论，提出首个原生支持少步（few-step）、单轨迹（single-trajectory）流模型的引导框架，在文本到图像生成等任务中实现最高达70倍的加速，同时保持或超越现有方法的样本质量。
## ViTok-v2: Scaling Native Resolution Auto-Encoders to 5 Billion Parameters

[https://arxiv.org/pdf/2605.05331](https://arxiv.org/pdf/2605.05331)

- ViTok-v2通过**原生分辨率训练架构**、**DINOv3感知损失**和**大规模解码器扩展**，解决了ViT分词器在分辨率灵活性、训练稳定性和规模扩展性方面的关键局限。
## Flow-OPD: On-Policy Distillation for Flow Matching Models

[https://arxiv.org/pdf/2605.08063](https://arxiv.org/pdf/2605.08063)

- 论文提出 Flow-OPD（On-Policy Distillation for Flow Matching），核心创新包括： 两阶段对齐策略：先通过单奖励 GRPO 训练领域专家教师模型（隔离优化以达到各领域性能上限），再通过基于 Flow 的冷启动（Cold-Start）建立鲁棒初始策略，最后将异质专业知识通过**在线策略蒸馏（OPD）**整合到统一的学生模型中 密集轨迹级监督（Dense Trajectory-Level Supervision）：通过任务路由机制（Task-Routing Labeling）为不同域样本分配对应的专家教师，提供沿整个去噪轨迹的密集监督信号，替代稀疏的标量奖励 流形锚定正则化（Manifold Anchor Regularization, MAR）：**引入任务无关的美学教师模型，通过全数据监督将生成过程锚定到高质量流形**，有效解耦功能对齐与美学保持，防止风格坍塌 实验表明，该方法在 Stable Diffusion 3.5 Medium 上将 GenEval 分数从 63 提升至 92，OCR 准确率从 59 提升至 94，相比 vanilla GRPO 实现约 10 个点的整体提升，同时保持图像保真度和人类偏好对齐。
## 📌Qwen-Image-2.0 Technical Report【Qwen】

[https://arxiv.org/pdf/2605.10730](https://arxiv.org/pdf/2605.10730)

- Qwen-Image-2.0 Technical Report 提出了一个全功能的图像生成基础模型，旨在解决当前图像生成系统在真实创意工作流中的关键瓶颈，并实现了文本到图像（T2I）生成与**图像编辑（TI2I）**的统一架构。最根本的问题在于：**现有系统通常只能在单一维度表现优异（要么生成照片级图像，要么准确渲染文本；要么支持文生图，要么支持图像编辑）**，极少有系统能在单一统一模型中同时交付所有这些能力，而不依赖独立的处理管道或遭受显著的质量权衡。Qwen-Image-2.0 的核心目标正是构建一个统一框架，同时支持： 高保真图像生成与精确图像编辑 文本到图像（T2I）与图像到图像（TI2I）任务 在多语言、高分辨率、复杂构图等多样化场景下的稳定质量输出 通过结合 **Qwen3-VL 作为条件编码器、多模态扩散 Transformer（MMDiT）以及高压缩率 VAE**，论文试图在单一架构内弥合深度多模态理解与高保真生成之间的差距。
## 📌Qwen-Image-VAE-2.0 Technical Report【Qwen】

[https://arxiv.org/pdf/2605.13565](https://arxiv.org/pdf/2605.13565)

- 论文提出了 Qwen-Image-VAE-2.0，通过**全局跳跃连接（GSC）与扩展潜在通道的架构改进、十亿级数据规模与合成渲染引擎的数据工程**，以及分阶段语义对齐的训练策略，首次在 f16 和 f32 高压缩比下实现了媲美甚至超越传统 f8 VAE的重建保真度（特别是文本可读性）与潜在空间可扩散性。
## 📌Qwen-Image-Bench: From Generation to Creation in Text-to-Image Evaluation【Qwen】

[https://arxiv.org/pdf/2605.28091](https://arxiv.org/pdf/2605.28091)

- 论文提出 **Qwen-Image-Bench**，一个与专业艺术家共同设计的创作者中心基准测试。该基准通过构建五级层次化分类体系（5个一级支柱、23个二级子能力、56个三级可验证标准），在**传统维度（质量、美学、对齐）基础上引入真实世界保真度与创意生成**两大应用驱动维度，并训练了基于专家监督的统一评判模型（Q-Judger），以提供可解释、可归因的细粒度诊断，而非单一模糊分数。对 18 个前沿模型（包括 GPT Image 2、Nano Banana Pro、Seedream 5.0、FLUX 2、Qwen Image 2.0 Pro 等）的评估显示：** GPT Image 2 以总体得分 64.69 显著领先**，形成 T1 层级；模型自然分化为 T1–T5 五个层级，跨度 16.5 分，验证基准的有效区分力 传统维度（质量、美学）的模型方差极低（T2–T3 质量差距仅 1.35 分），已成为"筹码能力" 创意生成（方差超质量维度 11 倍）与真实世界保真度是当前模型分化的核心战场；感知-认知鸿沟（Perception-to-Cognition Frontier） 识别出五个跨支柱的系统性能力瓶颈，即使最强模型得分亦低于 44 分： 解剖学保真度（最佳 20.9 分）、物理逻辑（31.9 分）、动物（42.6 分）、物体（41.9 分）、接触交互（43.3 分） 这些维度共同要求隐式世界知识（生物结构、物理定律、空间推理），而非表面视觉统计，表明当前模型已掌握视觉感知（风格、美学），但未具备认知层面的因果推理与结构理解能力 创意生成的阈值效应 在游戏设计（GPT Image 2 得 91.76 分，第 7 名骤降至 65.71 分以下）、文本准确性、信息可视化等高方差维度，模型呈现"能或不能"的二元分化，缺乏中间状态，反映高级创意任务对想象力与执行精度的双重严苛要求。
## 📌Lens: Rethinking Training Efficiency for Foundational Text-to-Image Models【Microsoft】

[https://arxiv.org/pdf/2605.21573](https://arxiv.org/pdf/2605.21573)

- **Lens**，一个专为训练效率优化的基础文本到图像（T2I）生成模型，通过系统性地提升数据利用效率和收敛速度。Lens采用**3.8B参数**的MMDiT架构，相比Z-Image（6B）、FLUX.2（9B）、Qwen-Image（20B）显著更小，直接降低每步训练的FLOPs。**最大化数据信息密度密集字幕（Lens-800M）：使用GPT-4.1为800M图像生成平均109词的长形式详细字幕（对比传统短字幕），编码更丰富的物体、属性、空间关系和背景信息**。消融实验表明，**密集字幕在GenEval上显著优于短字幕或混合字幕策略**。多分辨率与多宽高比训练：每批次混合5122、7682、10242三种基准面积及1:2至2:1共9种宽高比（27个分辨率桶）。该策略不仅增加每批次的视觉信息多样性，还使模型能够零样本泛化至训练时未见过的分辨率（最高14402）和任意宽高比，避免了昂贵的高分辨率训练阶段。语义VAE选择：通过在T2I流程中直接评估（而非传统rFID指标），**选定FLUX.2-VAE。该语义VAE提供更紧凑、语义更明确的潜在空间，显著加速模型收敛并提升生成质量。 **强语言编码器：**采用GPT-OSS（20B MoE，3B激活参数）作为文本编码器**，**提取第4/12/18/24层特征进行多层级语义条件**。强编码器不仅加速优化，还实现了仅用英文数据训练即支持中文、法文、日文等多语言推理的零样本泛化能力，减少了多语言数据采集成本。
## 📌GPIC: A Giant Permissive Image Corpus for Visual Generation【Feifei Li】

[https://arxiv.org/pdf/2605.30341](https://arxiv.org/pdf/2605.30341)

- 论文提出**GPIC（Giant Permissive Image Corpus）**，一个**包含约28万亿像素、1亿训练样本的大规模图像-文本数据集，所有图像均来自Flickr和Wikimedia的许可开放内容（CC BY、CC0、Public Domain等），并通过Hugging Face集中托管**，同时建立了基于FD-DINOv2的新评估协议，为视觉生成建模提供一个开放、稳定、可复现的研究基准。
## GenClaw: Code-Driven Agentic Image Generation【Tencent Hunyuan】

[https://arxiv.org/pdf/2605.30248](https://arxiv.org/pdf/2605.30248)

- 论文提出 GenClaw ——一种代码驱动的代理式图像生成范式（Code-Driven Agentic Image Generation）。该范式通过"概念化→草图→着色（Conceptualize → Sketch → Color）"的三阶段流程，将大型语言模型（LLM）的编程与逻辑能力转化为"数字画笔"： 概念层：利用搜索与推理工具构建上下文知识； 草图层：通过代码（SVG、HTML、Three.js等）构建可执行的视觉草图，精确控制布局、文本与物理结构； 着色层：由图像生成模型基于结构化草图补充真实感纹理与材质。 通过这种解耦架构，代码作为可控的中间表示（Intermediate Representation）桥接了语言推理与像素合成，将图像生成从黑盒过程转变为可解释、可追踪、可精确控制的分阶段创作流程。
## HiDream-O1-Image: A Natively Unified Image Generative Foundation Model with Pixel-level Unified Transformer

[https://arxiv.org/pdf/2605.11061](https://arxiv.org/pdf/2605.11061)

- 视觉生成领域长期受限于**模块化架构的碎片化**约束。传统潜在扩散模型（LDMs）依赖分离的VAE压缩和预训练文本编码器，导致高频细节损失与跨模态语义错位；现有像素空间扩散Transformer虽 bypass VAE瓶颈，但仍沿用分离文本编码器，且局限于单一文本到图像任务。这引发关键问题：能否将像素空间扩散模型扩展为统一的通用视觉推理引擎？
## AnyFlow: Any-Step Video Diffusion Model with On-Policy Flow Map Distillation【Policy Distillation, NVIDIA】

[https://arxiv.org/pdf/2605.13724](https://arxiv.org/pdf/2605.13724)

- **基于一致性蒸馏（consistency distillation）的视频扩散模型在测试时扩展性（test-time scaling）方面的结构性缺陷。**现有少步视频生成方法（如rCM、Self-Forcing等）虽在极低采样步数（如4 NFEs）下表现优异，但存在**"步数增加时性能反而下降"**的反直觉现象。论文指出，这源于****一致性蒸馏将原始概率流ODE（Probability-Flow ODE）轨迹替换为一致性采样轨迹****——该轨迹通过反复对中间状态重加噪（re-noising）进行多步采样，导致累积偏差使生成轨迹逐渐偏离目标PF-ODE路径，无法利用额外计算资源提升质量。
## Coding Agent Is Good As World Simulator

[https://arxiv.org/pdf/2605.14398](https://arxiv.org/pdf/2605.14398)

- 解决**视频生成模型作为世界模拟器时存在的物理不合理性问题**，以及**传统物理模拟器在场景构建方面的高门槛问题**。论文提出将世界建模从**帧预测**转变为**模拟器感知的代码生成**。通过多代理框架（规划代理、代码代理、视觉审查代理、物理分析代理），**系统自动生成可执行的PyChrono物理模拟代码，迭代修复直至满足物理约束和视觉要求**，从而构建既**视觉合理**又**物理可检查**的世界模型。
## Continuous Latent Diffusion Language Model【ByteDance Seed, Seedance】

[https://arxiv.org/pdf/2605.06548](https://arxiv.org/pdf/2605.06548)

- 试图提出一种与传统自回归 (autoregressive) 语言模型不同的生成方式。作者提出了一个全新的框架，叫做 Cola DLM（即 Continuous Latent Diffusion Language Model），试图**通过扩散模型在潜在空间中建模文本**，从而： 打破严格的左–右顺序依赖 实现更灵活的生成结构 同时具备高效生成与全局语义理解能力 简单来说，**它不是逐词生成，而是在潜在空间做扩散 —— 先全局理解语义，再解码成文本**。1. 文本到潜在空间的映射 (Text VAE) 使用变分自编码器 (VAE) **将文本编码成一个连续的潜在表示。这样做的好处是把离散文本转换成连续空间**，更适合做扩散建模。 2. 潜在空间中的全局语义 Prior 模型 (Block‑causal DiT) 在这个连续潜在空间上，用一种称为 block‑causal DiT 的扩散Transformer 模型去建模文本的全局先验（global prior）。这个先验负责捕捉整个句子的语义信息，不依赖严格的顺序。 3. 条件解码器生成文本 在完成潜在空间的语义建模之后，解码器根据生成的潜在语义条件去生成最终的自然语言文本。

# Agent Application

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Application
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, science-discovery, vision
- 论文/报告：151 篇
- Can AI Be a Good Peer Reviewer? A Survey of Peer Review Process, Evaluation, and the Future【Research】
- SciResearcher: Scaling Deep Research Agents for Frontier Scientific Reasoning【Research, Science, Data】
- AcademiClaw: When Students Set Challenges for AI Agents【Research, Pengfei Liu】
- SciIntegrity-Bench: A Benchmark for Evaluating Academic Integrity in AI Scientist Systems【Research】
- ⭐DataMaster: Towards Autonomous Data Engineering for Machine Learning【Research】
- ⭐D3-Gym: Constructing Real-World Verifiable Environments for Data-Driven Discovery【Research, Science】
- 🧐AutoLLMResearch: Training Research Agents for Automating LLM Experiment Configuration -- Learning from Cheap, Optimizing Expensive【Research】
- Graphs of Research: Citation Evolution Graphs as Supervision for Research Idea Generation【Research】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Can AI Be a Good Peer Reviewer? A Survey of Peer Review Process, Evaluation, and the Future【Research】

[https://arxiv.org/pdf/2604.27924](https://arxiv.org/pdf/2604.27924)

- 系统性地探讨**如何利用人工智能（特别是大语言模型）辅助或自动化学术同行评审流程**，并解决该领域在方法学与评估方面的关键挑战。
## SciResearcher: Scaling Deep Research Agents for Frontier Scientific Reasoning【Research, Science, Data】

[https://arxiv.org/pdf/2605.01489](https://arxiv.org/pdf/2605.01489)

- 解决深度研究智能体（Deep Research Agents）在前沿科学推理（Frontier Scientific Reasoning）领域的数据构建难题。论文提出了 **SciResearcher**——一个完**全自动化的智能体数据构建框架**。该框架通过两个核心流水线（概念任务构建与计算任务构建），**基于种子科学实体合成植根于学术证据的多样化推理任务**，从而支持对深度研究智能体进行监督微调（SFT）与智能体强化学习（RL），最终提升其在前沿科学推理中的长程规划、跨源证据整合与复杂计算能力。
## AcademiClaw: When Students Set Challenges for AI Agents【Research, Pengfei Liu】

[https://arxiv.org/pdf/2605.02661](https://arxiv.org/pdf/2605.02661)

- 论文提出了AcademiClaw——首个针对OpenClaw生态系统的学术级别基准测试，其核心创新包括： 任务来源：**从230个学生提交的真实学术工作流（课程作业、研究项目、竞赛、个人项目）中筛选出80个复杂长程任务，涵盖25个以上专业领域（包括奥林匹克级数学/语言学、GPU强化学习、全栈系统调试等）**。 评估维度：采用六种互补技术（模式匹配、代码执行、LLM评判、视觉LLM评估、端到端浏览器测试、结构化输出验证）构建多维评分标准，并引入五类别安全审计。 执行环境：在隔离的Docker沙盒中运行，包含16个需要CUDA GPU执行的任务，填补现有基准在GPU密集型任务评估上的空白。 通过评估六个前沿模型（通过率最高仅55%），论文揭示了当前代理在形式推理、跨领域泛化和资源效率方面的系统性短板，为OpenClaw社区提供了超越聚合指标的细粒度诊断信号，推动框架从助理级工具向更全面的学术级代理演进。
## SciIntegrity-Bench: A Benchmark for Evaluating Academic Integrity in AI Scientist Systems【Research】

[https://arxiv.org/pdf/2605.10246](https://arxiv.org/pdf/2605.10246)

- 解决**AI科学家系统（AI Scientist Systems）的学术诚信（Academic Integrity）评估缺失问题**。诚信失败普遍存在：总体问题率（Fail+Flawed）达34.2%，无模型实现零失败。 缺失数据场景的系统性伪造：在T08（缺失数据伪造）类别中，所有7个模型均生成合成数据而非承认数据不足，仅差异在于是否披露该行为。 驱动机制分离：通过提示消融实验发现，移除显式"必须完成"的指令后，未披露伪造率从20.6%骤降至3.2%，但基础合成率保持不变（~56%）。这表明存在与提示无关的**"内在完成偏见"**（Intrinsic Completion Bias），即模型将数据缺口视为"待填补空白"而非"停止信号"。 三种递进失败模式：不可行性隐瞒（直接伪造）、识别绕过（发现缺陷后仍继续）、虚假审计轨迹构建（伪造证据链以逃避审查）。
## ⭐DataMaster: Towards Autonomous Data Engineering for Machine Learning【Research】

[https://arxiv.org/pdf/2605.10906](https://arxiv.org/pdf/2605.10906)

- 提出 DataMaster 框架，通过系统化优化数据状态而非修改模型代码，实现下游机器学习性能的显著提升。该问题面临三大挑战：**开放性的数据源搜索空间**（外部数据分散且质量参差）、**分支依赖的数据细化**（数据需经模式对齐、清洗、转换方可使用）、**延迟验证与累积性**（数据价值需经训练反馈确认，且需记忆历史避免重复失败）。论文提出 DataMaster，通过三个耦合组件实现结构化数据端搜索： DataTree：树状搜索结构，包含两类节点： 红节点（Red Nodes）：执行开放式外部数据搜索，发现候选数据集并写入共享 Data Pool； 黑节点（Black Nodes）：从 Data Pool 选取数据，构建可执行的数据加载与处理流程（DataLoader），提交下游训练获取反馈。 Data Pool：全局共享的候选数据层，存储已发现数据源的元数据、模式与路径，支持跨分支复用。 Global Memory：持久化记录层，存储节点执行结果、训练性能、失败教训与可复用发现，使后续节点能够基于历史证据进行决策。
## ⭐D3-Gym: Constructing Real-World Verifiable Environments for Data-Driven Discovery【Research, Science】

[https://arxiv.org/pdf/2604.27977](https://arxiv.org/pdf/2604.27977)

- 论文提出了**D3-Gym**——首个自动构建的、具有可验证环境的科学数据驱动发现数据集，**通过自动化流水线从239个真实科学仓库中构建565个可执行任务环境**，每个环境配备自动生成的领域特定评估脚本，从而支持模型通过执行反馈进行自我改进和严格评估。
## 🧐AutoLLMResearch: Training Research Agents for Automating LLM Experiment Configuration -- Learning from Cheap, Optimizing Expensive【Research】

[https://arxiv.org/pdf/2605.11518](https://arxiv.org/pdf/2605.11518)

- 解决**高成本大语言模型（LLM）实验配置的自动化问题**，即如何在严格的计算预算约束下，有效配置可扩展的LLM实验（涵盖架构设计、超参数调优、训练配方等），避免因配置不当导致的大量计算资源浪费。
## Graphs of Research: Citation Evolution Graphs as Supervision for Research Idea Generation【Research】

[https://arxiv.org/pdf/2605.14790](https://arxiv.org/pdf/2605.14790)

- 解决**大语言模型（LLM）在研究想法生成任务中未能充分利用参考文献间结构关系**的问题。论文提出**Graphs of Research (GoR)**框架，****通过构建引用演化有向无环图（DAG）**作为监督信号，将参考文献的2-hop邻域、8类边特征（涵盖位置、影响力、时序、拓扑等）及前驱关系编码为结构化文本提**示，对7B规模的LLM进行监督微调，从而提升自动化研究想法生成的质量与创新性。
## EvolveMem: Self-Evolving Memory Architecture via AutoResearch for LLM Agents【Research】

[https://arxiv.org/pdf/2605.13941](https://arxiv.org/pdf/2605.13941)

- 论文提出**EVOLVEMEM**架构，通过**AutoResearch**（自主研究）机制实现检索基础设施的自我演化：系统通过LLM驱动的闭环诊断，自主执行"观察-假设-实验-验证"循环，自动发现包括原始动作空间中不存在的新配置维度在内的有效检索策略，从而替代手工配置调优。
## GEAR: Genetic AutoResearch for Agentic Code Evolution【Research】

[https://arxiv.org/pdf/2605.13874](https://arxiv.org/pdf/2605.13874)

- 解决**自主机器学习研究代理（Autonomous Research Agents）在外层循环搜索策略上的结构性局限**。论文提出 GEAR（Genetic AutoResearch），一种基于种群的搜索控制器，通过以下机制替代单现任者策略： 维护**有界的前沿种群（bounded frontier）**存储精英研究状态**** ****通过**变异（mutation）和交叉（crossover）**扩展搜索图**** 引入**角色系统（BEST/LEAN/DIVERSE）**保持搜索多样性 在 GEAR-Evolve 变体中，甚至允许代理演化搜索策略本身 实验表明，这种基于遗传算法的种群搜索能够延长自主发现的时间范围，在相同的计算预算下持续发现改进，而基线方法在实验进行到一半时即已收敛。GEAR 框架：首个将显式种群遗传搜索引入 AutoResearch 风格系统的即插即用控制器，无需修改底层训练环境 机制剖析：系统验证变异（局部探索）与交叉（跨分支重组）在自主代码进化中的互补价值，以及角色系统对维持多样性的关键作用 策略演化：GEAR-Evolve 证明将搜索策略本身作为可修改工件，允许代理修复交叉退化等问题，可进一步提升搜索效率与质量 实证证据：证明种群式前沿搜索可显著延长自主代理的发现时间范围，避免单现任者策略的早熟收敛。
## Argus: Evidence Assembly for Scalable Deep Research Agents【Research】

[https://arxiv.org/pdf/2605.16217](https://arxiv.org/pdf/2605.16217)

- 解决**深度研究智能体（Deep Research Agents）在并行扩展时的收益递减与上下文瓶颈问题**。论文提出将深度研究重新框架化为证据组装（Evidence Assembly）问题——如同拼装拼图（jigsaw），而非并行暴力生成完整答案。通过引入Navigator维护共享证据图（Directed Acyclic Graph of evidence and claims），系统能够： 显式追踪已验证、待验证和缺失的证据节点 针对特定缺口定向派遣 Searcher，避免重复检索 将 K 条轨迹的原始文本（如 25.6M tokens）压缩为结构化的图视图（约 21.5K tokens），实现上下文与并行规模的解耦.
## AI for Auto-Research: Roadmap & User Guide【Research】

[https://arxiv.org/pdf/2605.18661](https://arxiv.org/pdf/2605.18661)

- **人工智能在学术研究自动化进程中面临的"生产力前沿"与"科学诚信危机"之间的张力，以及缺乏对AI贯穿完整研究生命周期的统一分析框架。**论文试图回答如何可信地部署AI研究工具： 全自动化 vs 人机协作：证明"人类主导的合作"（human-governed collaboration）是最可信的部署范式；从"检测AI使用"转向"治理与披露"：随着AI辅助成为常态，关键问题变为归属、责任和科学诚信的保全 建立跨阶段的结构化分类法、基准测试集和工具清单，为实践者提供可操作的指南；简言之，论文不是要阻止AI自动化研究，而是要建立一套认识论框架，确保AI在扩大研究规模的同时，不牺牲证据、判断、溯源和问责等科学本质。五大核心发现 人工制品生成超越科学验证：AI能高效生成想法、代码、图表和文本，但验证其新颖性、忠实性和科学意义的能力滞后 人机协作是最可靠模式：全自动化风险高，"人类主导的合作"（human-governed collaboration）能保留科学判断同时提升效率 能力边界在开放式任务中出现：AI在结构化任务（文献检索、标准绘图）表现良好，但在真正新颖的想法设计、研究级代码（准确率仅37-39%）、科学判断上表现脆弱 分层架构趋同：有效系统普遍结合探索层（搜索/生成）、执行层（工具使用）和验证层（检查/批判） AI使用成为治理问题：随着AI辅助普及，关键问题从"检测AI使用"转向"披露、归属、责任和科学诚信保全”。
## DeepWeb-Bench: A Deep Research Benchmark Demanding Massive Cross-Source Evidence and Long-Horizon Derivation【Research】

[https://arxiv.org/pdf/2605.21482](https://arxiv.org/pdf/2605.21482)

- 解决现有深度研究基准测试对当前前沿模型缺乏区分度的问题。具体而言，随着OpenAI Deep Research、Claude Research、Gemini Deep Research等前沿产品在现有基准（如SimpleQA、GAIA、BrowseComp等）上取得高分，现有评估数据已难以有效区分不同模型的能力差异。 为此，论文引入DEEPWEB-BENCH，通过以下三个核心难点提升评估难度： **大规模证据收集（Massive evidence collection）：要求模型处理大量权威文档**，而非仅从少数页面检索事实；** 跨来源协调（Cross-source reconciliation）：要求模型处理来源间的冲突和不一致，而非单一来源查找**； **长程多步推导（Long-horizon multi-step derivation）：要求模型通过显式算术和建模假设将检索到的数字组合成最终定量答案**，而非单步提取。 该基准将上述难点转化为四个能力维度（检索、推导、推理、校准），并通过 8×8 的任务矩阵结构，确保每个任务同时考验这三种能力，从而为当前世代的深度研究智能体提供足够的"区分 headroom"。对9个前沿模型（包括GPT-5.5、Claude Opus 4.7、DeepSeek V4等）的评估显示： 整体性能：最高分33.37%（Codex CLI + GPT-5.5），最低16.79%（Kimi K2.6），16.58个百分点的差距验证了基准的区分度。 瓶颈定位：检索失败仅占错误12–14%，而**推导与校准失败超过70%，证明难点在于证据合成而非获取**。 失效模式分化： 强模型：主要因不完全推导（31%，如误用营收基数）失分 弱模型：主要因幻觉精度（38%，在证据不足时强行给出精确值）失分 领域特化：模型间案例级Spearman相关仅 ρ=0.61 ，跨模型标准差最高达18.8个百分点，表明模型存在真实的领域专业化差异。
## AiraXiv: An AI-Driven Open-Access Platform for Human and AI Scientists【Research】

[https://arxiv.org/pdf/2605.21481](https://arxiv.org/pdf/2605.21481)

- **传统学术出版系统在AI时代面临的可扩展性危机与效率瓶颈。论文提出 AiraXiv —— 一个AI驱动的开放获取平台，通过以下机制解决上述问题：**** AI增强审稿：提供自动化、结构化的审稿反馈和多维度质量信号****，减少对传统同行评审的依赖 双模态交互：支持人类通过Web UI交互，同时通过Model Context Protocol (MCP)****支持AI科学家自主提交、阅读和迭代论文**** 快速迭代循环：实现"作者-读者-AI"的直接反馈闭环，支持论文的持续更新和版本迭代 轻量级会议组织：支持用户自主组织主题化的轻量级学术活动，提高学术交流效率**
## Claw AI Lab: An Autonomous Multi-Agent Research Team【Research】

[https://arxiv.org/pdf/2605.22662](https://arxiv.org/pdf/2605.22662)

- 解决**自主人工智能研究系统在实际实验室环境中所面临的交互性、可检查性和可靠性不足**的问题。通过提出 **Claw AI Lab**，论文将自主研究从"自动化论文生产"重新框架为"**可交互、可检查、可靠性感知的 AI 实验室基础设施**"，通过统一仪表板实现实时监控、工件检查、一键回滚，并通过 Claw-Code Harness 确保实验执行的完整性和结果的可追溯性。
## DeepSurvey: Enhancing Analytical Depth and Citation Reliability in Automated Survey Generation【Research, SII】

[https://arxiv.org/pdf/2605.29522](https://arxiv.org/pdf/2605.29522)

- 论文提出 **DeepSurvey** 系统，通过构建**可复用的分析基底（analysis substrate）**（整合全文解析、跨论文聚类与对比分析、代码仓库解析）和**闭环可靠性机制**（图扩展检索、证据约束写作、多粒度智能体验证），实现兼具深度与可靠性的自动化综述生成。
## FML-bench: A Controlled Study of AI Research Agent Strategies from the Perspective of Search Dynamics【Research, Meta】

[https://arxiv.org/pdf/2605.17373](https://arxiv.org/pdf/2605.17373)

- 解决**AI研究代理（AI research agents）策略评估中的受控比较与归因问题。论文构建了FML-bench（Fundamental Machine Learning Benchmark），包含****覆盖10个ML领域（泛化、数据效率、表示学习、持续学习、因果推断、鲁棒性、隐私、公平性、遗忘、联邦学习）的18个基础研究任务****。其核心设计包括： ****基础设施标准化：统一代码编辑器、实验执行器、验证/测试分离机制与指标显示格式，仅保留代理自身的搜索策略（搜索拓扑、记忆机制、动作词汇）作为变量**** 过程级指标体系：定义12个行为指标，包括基于GraphCodeBERT嵌入的探索几何指标（探索散布 Dspread 、探索可达距离 Dreach 、有效维度 Deff ）与效率指标（AUC-over-steps、首次改进步数等）**
## SciVQR: A Multidisciplinary Multimodal Benchmark for Advanced Scientific Reasoning Evaluation【Research, Science, OPPO】

[https://arxiv.org/pdf/2605.10187](https://arxiv.org/pdf/2605.10187)

- 论文提出了 SciVQR（Scientific Visual Question Reasoning）基准测试，其核心创新包括： 广泛的学科覆盖：涵盖数学、物理、化学、地理、天文学和生物学6大学科，细分为54个子领域； 多层次的难度设计：包含高中、大学及研究生水平的试题，分为易、中、难三个等级； 可追踪的推理评估：46%的题目配有专家撰写的详细解题步骤，支持对思维链（Chain-of-Thought）质量的五维度评估（保真性、信息性、冗余度、幻觉检测、步骤完整性）。 通过这一设计，SciVQR 能够对MLLMs在多模态理解、领域知识整合和复杂多步推理方面的真实能力进行更严格、更透明的评估。
## ⭐XDomainBench: Diagnosing Reasoning Collapse in High-Dimensional Scientific Knowledge Composition【Research, Science】

[https://arxiv.org/pdf/2605.14754](https://arxiv.org/pdf/2605.14754)

- 计算材料科学、化学和药物发现领域面临碎片化软件生态与高昂运营开销的双重挑战。传统高通量工作流框架（如 Atomate2、AFLOW）虽可靠，但属于硬编码（hard-coded）系统，缺乏适应探索性研究的灵活性。现有 LLM 驱动的科学智能体多采用单一独立架构（monolithic），迫使开发者重复构建基础设施（系统提示词、通信边界、UI 等），难以跨平台迁移，且无法跟上通用编码智能体（Claude Code、Cursor 等）的快速演进。 核心矛盾在于：**确定性自动化脚本的可靠性与自由形式编码智能体的灵活性之间难以平衡**。当前AI for Science（AI4S）应用要求模型具备组合泛化能力——即在多轮交互中动态整合多个学科知识。论文提出XDOMAINBENCH，一个诊断式基准测试，通过两个可控维度形式化跨学科推理空间： (i) 组合顺序（Composition Order） 定义涉及领域的数量 k∈{1,2,3,4} ，构建从单领域（k=1 ）到四领域组合（k=4 ）的渐进评估，覆盖20个科学领域（化学、物理、生物、经济、艺术等）。 (ii) 混合结构（Mixture Structure） 通过领域权重向量 wt∈Δk−1 控制每轮对话中各学科的知识依赖强度，利用香农熵 H(wt) 和Jensen-Shannon散度量化领域混合的分散度与轮间漂移。 (iii) 8种轨迹模式 模拟真实AI4S场景中的动态演变，构建覆盖**难度轨迹（稳定、渐增、渐减、尖峰、波动）**和**领域混合轨迹（稳定、渐变、波动）**的8种交互模式，共8,598个多轮会话。
## Harnessing AtomisticSkills for Agentic Atomistic Research【Research, Science】

[https://arxiv.org/pdf/2605.24002](https://arxiv.org/pdf/2605.24002)

- 论文提出 AtomisticSkills 框架，通过**层次化分解（hierarchical decomposition）**解决上述问题： 工具层（Tools）：底层严格结构化的操作（如DFT计算、分子动力学），通过MCP协议暴露为类型安全的函数 技能层（Skills）：中层级模块化研究单元（如"稳定性分析"、"扩散分析"），由工具序列构成，附带自然语言 procedural documents 工作流层（Workflows）：高层研究活动（如"发现锂离子固态电解质"），由智能体动态组合技能构建 该框架使通用编码智能体获得即插即用的原子级研究能力，无需重新发明核心智能体基础设施，同时通过模块化设计确保科学可复现性。
## ScientistOne: Towards Human-Level Autonomous Research via Chain-of-Evidence【Research, Science, Google】

[https://arxiv.org/pdf/2605.26340](https://arxiv.org/pdf/2605.26340)

- 论文提出： Chain-of-Evidence (CoE)：一个可验证性标准，**要求每个研究声明（引用、数值、方法论、结论）必须通过记录的证据链追溯至其基础源**； **ScientistOne：一个端到端自主研究系统，其文献调研、方案发现与论文写作模块均原生满足 CoE 要求，通过声明验证器（Claim Verifier）确保每句话都有证据支撑**； CoE Integrity Audit：一个可复用的事后审计协议，包含四项完整性检查（分数验证、规范违规检测、引用验证、方法-代码对齐），适用于任何系统的输出。 通过在 ADRS 基准上对 5 个系统的 75 篇论文进行审计，论文证实**所有基线系统均存在至少一种系统性失败模式，而 ScientistOne 实现了零捏造引用、完美分数验证及最高方法-代码对齐率，同时保持或超越人类专家的任务表现。**
## **SciAtlas: A Large-Scale Knowledge Graph for Automated Scientific Research**【Research, Science, Huajun Chen】

[https://arxiv.org/pdf/2605.22878](https://arxiv.org/pdf/2605.22878)

- 该论文针对全球学术产出指数级增长带来的"信息爆炸"挑战，以及现有检索工具缺乏拓扑推理能力、代理式研究存在高成本与逻辑幻觉等问题，提出了**SciAtlas**——一个大规模多学科异构学术知识图谱，并开发了配套的神**经符号检索算法**，为自动化科学研究提供结构化认知基质。规模与覆盖 SciAtlas整合超过4300万篇论文（覆盖26个学科）、1.57亿实体与30亿关系三元组，其中医学（18.56%）、社会科学（10.70%）、工程（9.43%）等为核心学科。 异构 Schema 设计 构建9类实体节点（Paper, Author, Institution, Keyword, Topic, Subfield, Field, Domain, Source）与12类关系边（CITES, AUTHORED, COAUTHOR, HAS_KEYWORD, COOCCUR, RELATED_TO等），形成四层认知结构： 语义层：引用与相关性关系建立论文间直接语义连接 概念层：关键词共现（COOCCUR）实现概念级间接关联 方向层：Domain→Field→Subfield→Topic层级组织学科方向 社会层：作者合著与机构隶属关系形成社交网络。**基于OpenAlex数据源，通过Qwen3-30BA3B-Instruct模型从摘要提取3-8个高层可复用关键词（避免论文特定术语），使用bge-large-en-v1.5生成标题、摘要与关键词的语义嵌入，最终部署于Neo4j图数据库**。
## SciHorizon-DataEVA: An Agentic System for AI-Readiness Evaluation of Heterogeneous Scientific Data【Science, Data】

[https://arxiv.org/pdf/2604.26645](https://arxiv.org/pdf/2604.26645)

- 论文提出了SciHorizon-DataEVA，一个基于智能体（agentic）的系统，通过引入Sci-TQA2原则（涵盖治理可信度、数据质量、AI兼容性、科学适应性四个维度）和层次化多智能体评估方法Sci-TQA2-Eval，实现对异构科学数据自动化、可扩展且可靠的AI就绪程度评估。通过扫描文件树与元数据头构建数据画像 Pdata ，无需完整加载即可推断数据模态。维护结构化工具库（Tool Library），优先重用现有工具；当覆盖不足时，基于评估规范**动态合成新工具。**
## 🧐OpenSeeker-v2: Pushing the Limits of Search Agents with Informative and High-Difficulty Trajectories【Search】

[https://arxiv.org/pdf/2605.04036](https://arxiv.org/pdf/2605.04036)

- 论文挑战了“必须依赖 CPT+SFT+RL 复杂组合才能达到 SOTA”的固有认知，核心问题是：**仅通过简单的 SFT，能否在搜索智能体上实现与繁重工业流程相媲美的性能？ 为此，作者提出通过构建**信息丰富且高难度（informative and high-difficulty）**的合成轨迹数据，以纯粹的数据质量驱动模型能力，而非堆砌训练阶段。通过展示仅使用 10.6k 条高质量轨迹进行 SFT 即可在 30B 规模模型上达到 SOTA（甚至超越经过完整 CPT+SFT+RL 训练的 Tongyi DeepResearch 等工业级模型），论文旨在证明：精心设计的合成数据策略足以解锁强大的长程推理与深度搜索能力，从而使前沿搜索智能体研究对学术团队更具可复现性与可及性。**
## OpenSearch-VL: An Open Recipe for Frontier Multimodal Search Agents【Search, Tencent Hunyuan】

[https://arxiv.org/pdf/2605.05185](https://arxiv.org/pdf/2605.05185)

- 论文提出了OpenSearch-VL——一个完全开源的训练配方，包括： 数据构建流程：通过Wikipedia路径采样、模糊实体重写和源锚点视觉 grounding，构建避免单跳检索捷径的高质量数据集（SearchVL-SFT-36k 和 SearchVL-RL-8k）。 多样化工具环境：统一文本搜索、图像搜索、OCR、裁剪、锐化、超分辨率及透视校正，支持主动视觉感知与外部知识获取的协同。 致命感知GRPO算法：通过掩码失败后token并保留失败前有效推理的单侧优势裁剪（one-sided advantage clamping），实现鲁棒的长程多轮工具使用训练。 该配方旨在降低前沿多模态深度搜索智能体的研究门槛，提供透明的数据、代码和模型支持。
## LongSeeker: Elastic Context Orchestration for Long-Horizon Search Agents【Search】

[https://arxiv.org/pdf/2605.05191](https://arxiv.org/pdf/2605.05191)

- 解决**长程搜索智能体（long-horizon search agents）面临的上下文管理瓶颈**问题。作者提出，**有效的上下文管理应当是自适应的（adaptive）：智能体轨迹的不同部分应根据其当前与任务的相关性，以不同的详细程度（different levels of detail）存在**。具体而言： 新鲜证据应保持完整以供验证 已解决的证据可蒸馏为结论 精度关键细节应以片段形式保留 失败的探索分支应被删除或回滚 为将此原则付诸实践，论文**提出了Context-ReAct范式，通过五个原子操作（Skip、Compress、Rollback、Snippet、Delete）实现对上下文的弹性编排（elastic context orchestration），使智能体能够在每个推理步骤主动决定何时（when）、何处（where）以及如何（how）重塑其工作记忆**。
## 🧐Beyond Semantic Similarity: Rethinking Retrieval for Agentic Search via Direct Corpus Interaction【Search】

[https://arxiv.org/pdf/2605.05242](https://arxiv.org/pdf/2605.05242)

- 解决传统检索接口在智能体搜索（agentic search）场景下的结构性瓶颈问题。 具体而言，论文指出现代检索系统（无论是稀疏词汇匹配还是密集语义检索）普遍采用固定的相似性接口（similarity interface），将语料库访问压缩为单一的 top-k 检索步骤。这种设计在标准检索增强生成（RAG）流程中效率较高，但在需要复杂推理的智能体搜索中暴露出以下核心局限： 精确词汇约束难以实施：**传统检索器难以强制执行严格的词汇匹配或正则表达式模式**； 稀疏线索合取困难：无法有效组合多个弱线索（weak clues）进行联合检索； 局部上下文检查受限：无法针对文档内部特定跨度进行细粒度的验证和探查； 证据早期截断不可逆：在推理前被过滤掉的证据无法通过下游更强的推理能力恢复； 多步假设迭代受阻：难以支持基于部分证据观察后的计划修订和中间实体发现。论文提出****直接语料库交互（Direct Corpus Interaction, DCI）****范式，将语义理解下放到语言模型本身，赋予智能体**通过通用终端工具（如 grep、find、sed、shell 命令等）直接操作原始语料库的高分辨率接口**，从而无需依赖离线索引、嵌入模型或固定的 top-k 检索 API。
## PACEvolve++: Improving Test-time Learning for Evolutionary Search Agents【Search, Google】

[https://arxiv.org/pdf/2605.07039](https://arxiv.org/pdf/2605.07039)

- 针对上述问题，论文提出 PACEvolve++ 框架，通过顾问模型强化学习（advisor-model RL）实现： 解耦架构：**训练轻量级顾问模型处理假设生成、新颖性评估和选择决策，由更强的前沿模型负责代码实现** 阶段自适应优化（phase-adaptive RL）：早期采用组相对反馈学习广泛偏好，晚期转向基于前沿贡献的信用分配（frontier-contribution credit assignment），对齐进化搜索的动态特性（log-diminishing reward structure） 该方法旨在实现**昂贵评估环境下的稳定测试时训练，并提升开放 ended 优化问题中的样本效率**。
## From Web to Pixels: Bringing Agentic Search into Visual Perception【Search】

[https://arxiv.org/pdf/2605.12497](https://arxiv.org/pdf/2605.12497)

- 论文将该挑战形式化为感知深度研究（Perception Deep Research），要求模型： 主动搜索：执行多跳网络搜索以收集缺失的外部证据 身份解析：从间接事实线索（如"2025年1月成为某品牌大使"）解析出具体实体（如"Winter"） 视觉绑定：将解析出的身份与图像中的具体实例匹配（框/掩膜/区域答案），排除外观相似干扰物 关键难点 证据获取：搜索规划与信息检索的可靠性 身份解析：将非视觉事实（如企业收购历史、游戏发布捆绑包）转换为可验证的目标假设 实例绑定：在图像含多个相似实例时，依据外部证据而非外观选择正确区域 为衡量该能力，论文构建了 WebEyes 基准（含搜索驱动的定位、分割与VQA三任务视图），并提出 Pixel-Searcher 智能体工作流，通过"搜索-解析-定位"的显式推理链解决上述问题。
## Search-E1: Self-Distillation Drives Self-Evolution in Search-Augmented Reasoning【Search】

[https://arxiv.org/pdf/2605.22511](https://arxiv.org/pdf/2605.22511)

- 解决**搜索增强推理代理（search-augmented reasoning agents）训练流程过度复杂化**的问题。论文提出 Search-E1，核心解决思路包括： 利用策略自身 rollout 中的对比信号：**观察到 GRPO 训练后，同一问题的多次采样轨迹中已自然存在质量差异显著的"兄弟姐妹"轨迹（一条正确且简洁，另一条错误或冗长），无需外部标注即可提取步骤级监督**； 简化训练范式：通过交替执行标准 GRPO（轨迹级探索）与离线自蒸馏（OFSD）（步骤级固化），在不引入外部教师、辅助网络或手工奖励的前提下，实现策略自进化； 提供密集逐步监督：**通过非对称条件设计（学生仅见问题，教师额外见参考轨迹）**与带裁剪的 token 级前向 KL 散度，将高效轨迹的分布知识蒸馏给推理时的策略，解决传统 GRPO 信用分配稀疏的问题。
## LiveBrowseComp: Are Search Agents Searching, or Just Verifying What They Already Know?【Search, Xiaohongshu】

[https://arxiv.org/pdf/2605.28721](https://arxiv.org/pdf/2605.28721)

- 论文提出LiveBrowseComp——一个动态深度搜索基准，其设计原则包括：时间边界：**所有问题依赖基准构建前90天内发布的事实，置于当前模型知识边界之外**长尾过滤：排除全球显著事件，确保事实不易被预训练数据覆盖验证机制：通过人工多阶段验证确保问题可解且答案唯一在该基准上，**所有评估模型的闭卷准确率均低于2%**，搜索增强分数相对于BrowseComp下降25–40分，从而迫使代理必须进行真正的证据发现而非知识验证。
## SAGE: A Self-Evolving Agentic Graph-Memory Engine for Structure-Aware Associative Memory【Memory】

[https://arxiv.org/pdf/2605.12061](https://arxiv.org/pdf/2605.12061)

- 论文提出了**SAGE**（Self-evolving Agentic Graph-memory Engine），一个**将图记忆视为动态长期记忆基质的自进化引擎**，通过耦合**记忆写入器**（基于读取反馈增量构建图记忆）与**基于图基础模型（GFM）的记忆读取器**（执行结构感知检索并提供反馈），实现从碎片化线索恢复长推理链、学习利用结构角色、并通过自我进化持续改进记忆系统。
## δ -mem: Efficient Online Memory for Large Language Models【Memory】

[https://arxiv.org/pdf/2605.12357](https://arxiv.org/pdf/2605.12357)

- 一种轻量级在线记忆机制，核心思想是**以极小的固定状态压缩历史，并直接干预注意力计算。**
## Cognifold: Always-On Proactive Memory via Cognitive Folding【Memory】

[https://arxiv.org/pdf/2605.13438](https://arxiv.org/pdf/2605.13438)

- 解决**智能体记忆架构****从反应式检索向主动式认知基质****转变**的核心问题。论文提出COGNIFOLD架构，通过以下机制解决上述问题： 三层CLS扩展：在互补学习系统（Complementary Learning Systems）的海马体-新皮层双层基础上，增加前额叶意图层（Prefrontal Intent Layer） 连续拓扑自组织：通过图层面的REINFORCES、MERGE_NODES、edge decay、kNN completion等操作，在事件流中实时代谢结构 认知折叠循环：实现事件→概念→意图的渐进抽象，使记忆成为"pulling itself up by its own bootstraps"的自举系统 简言之，该论文旨在将智能体记忆从被动的检索靶标重新定义为主动的、始终在线的认知操作基质，使智能体能够在无用户提示的情况下，自主地从连续经验中建构意义、预测需求并涌现目标导向行为。
## ⭐MeMo: Memory as a Model【Memory】

[https://arxiv.org/pdf/2605.15156](https://arxiv.org/pdf/2605.15156)

- **如何高效地将新的、领域特定的或及时更新的知识整合到LLMs中****，而无需进行昂贵的全面重新训练或修改模型参数**的问题。MEMO的核心解决思路 论文提出**MEMO（Memory as a Model）**框架，**通过将新知识编码到独立的、专门训练的MEMORY模型参数中，同时保持EXECUTIVE模型（即LLM）参数完全冻结，从而： 通过合成的"反思"（reflections）数据集捕获复杂的跨文档关系** 通过参数化知识存储提供对检索噪声的鲁棒性 完全避免对基础LLM的灾难性遗忘 实现与任何LLM（包括开源和专有闭源模型）的即插即用集成 确保推理时的检索成本与语料库大小无关（仅取决于MEMORY模型的固定大小） 简言之，MEMO试图在保持基础模型不变的前提下，建立一个模块化、可更新、跨模型兼容的知识整合机制，克服现有方法在计算效率、鲁棒性和适用性方面的根本性限制。
## Hidden in Memory: Sleeper Memory Poisoning in LLM Agents【Memory】

[https://arxiv.org/pdf/2605.15338](https://arxiv.org/pdf/2605.15338)

- 研究的是**具有持久记忆（persistent memory）功能的大型语言模型（LLM）助手所面临的跨会话安全威胁**。沉睡记忆中毒（Sleeper Memory Poisoning）攻击的可行性与危害。当LLM助手配备长期记忆库以存储用户偏好、工作流和个性化信息时，攻击者可通过操纵外部上下文（如文档、网页、邮件、代码仓库等）注入虚假记忆。与即时生效的传统提示注入不同，这种攻击具有延迟特性： 注入阶段：攻击者将对抗性内容嵌入用户可能处理的外部文档，诱导助手将恶意构造的记忆（如虚假偏好、错误身份属性或有害操作指令）写入持久记忆库； 休眠阶段：注入的记忆在存储期间保持不可见，原始恶意上下文已不存在； 激活阶段：在未来无关的会话中，当检索机制基于语义相似性召回这些中毒记忆时，助手的行为会被攻击者意图暗中操控，导致偏见输出、错误工具调用或数据泄露等危害。 论文系统评估了这种攻击在主流商业模型（如GPT-5.5、Claude、Gemini、Kimi等）中的成功率，证明**持久记忆成为跨越多个未来对话的长期攻击面**，并分析了现有防御机制的局限性。
## RecMem: Recurrence-based Memory Consolidation for Efficient and Effective Long-Running LLM Agents【Memory】

[https://arxiv.org/pdf/2605.16045](https://arxiv.org/pdf/2605.16045)

- 基于认知科学中"孤立体验保留在瞬时记忆中，**只有重复或重现的模式才会巩固为稳定长期记忆**"的原则，论文提出了**RecMem**框架，通过**基于重现的记忆整合（Recurrence-based Memory Consolidation）机制，仅在观察到语义相似的交互持续重现时才触发LLM处理****，从而在保持甚至提升任务准确性的同时，将记忆构建的Token成本降低高达87%**（在LoCoMo基准上从约1520K Token降至193K Token）。
## EvoMemBench: Benchmarking Agent Memory from a Self-Evolving Perspective【Memory】

[https://arxiv.org/pdf/2605.18421](https://arxiv.org/pdf/2605.18421)

- 解决**大语言模型（LLM）Agent 记忆能力缺乏系统性、全面性评估基准**的问题。论文提出 **EvoMemBench**，从两个正交维度构建评估体系：记忆范围	In-Episode	单任务内的信息保留、冲突修正与执行状态维护 Cross-Episode	跨任务的可复用知识积累与执行经验蒸馏 记忆内容	Knowledge-Oriented	事实、规则、约束等支持推理的信息演化 Execution-Oriented	动作序列、工具调用、任务进度等支持决策的信息演化。
## Dynamic Mixture of Latent Memories for Self-Evolving Agents【Memory】

[https://arxiv.org/pdf/2605.21951](https://arxiv.org/pdf/2605.21951)

- 解决**智能代理在持续学习（Continual Learning）场景下的自我进化（Self-Evolution）问题**，具体聚焦于如何在不断获取跨领域新知识的同时，避免遗忘先前习得的能力，并真正增强模型的内在推理能力而非仅依赖外部检索。论文提出 MoLEM（Dynamic Mixture of Latent Memories）框架，其核心机制包括： 动态混合专家（MoE）架构：将多个专家（Experts）作为独立的记忆载体，通过键-查询匹配（key-query matching）动态选择和加权专家，生成聚合的潜在记忆并注入推理过程； 参数隔离：基础推理模型 πθ 保持完全冻结，**所有经验知识内化到额外的MoE模块（专家与路由器）**中，从架构层面避免灾难性遗忘； 阶段式扩展与域感知路由：每个训练阶段配对独立的专家组和轻量级自编码器（AE），通过重建误差选择路由组，实现无需任务ID的域识别，且分布外（OOD）输入自动回退到预训练模型，保护原始能力。 通过该框架，代理能够在数学、科学、代码等跨领域任务序列中持续进化，在保持预训练能力的同时，实现新知识的内化和旧知识的零遗忘。
## Memory-R2: Fair Credit Assignment for Long-Horizon Memory-Augmented LLM Agents【Memory】

[https://arxiv.org/pdf/2605.21768](https://arxiv.org/pdf/2605.21768)

- 针对**长程记忆增强型大语言模型智能体（long-horizon memory-augmented LLM agents）在多会话（multi-session）环境下的强化学习训练**所面临的根本性挑战。论文提出Memory-R2训练框架，其核心创新包括： LoGo-GRPO算法：结合全局轨迹级优化与局部重 rollout（local rerollouts），从共享中间记忆状态重新采样，实现更公平的会话级信用分配 共享参数架构：**通过角色特定提示（role-specific prompts）从同一LLM主干实例化提取器和管理器，实现记忆形成与演化的协同优化** 渐进式课程学习：从8会话逐步扩展到32会话，稳定长程记忆构建技能的学习 实验表明，该方法仅需两个训练对话即可显著优于基线，并在分布外基准、不同模型规模和回答智能体上展现强泛化能力。
## SAM: State-Adaptive Memory for Long-Horizon Reasoning Agent【Memory】

[https://arxiv.org/pdf/2605.24468](https://arxiv.org/pdf/2605.24468)

- 论文将长程推理重新定义为状态自适应记忆问题（state-adaptive memory）。核心观点是： **智能体在任何时刻都需要一个连贯的"决策状态"视图（包含已建立的事实、已解决的问题和待处理事项）** 这些信息通常隐含地分散在原始轨迹中，而非显式呈现 目标不是简单地缩短上下文，而是**构建一个可导航的记忆空间，使智能体能够根据当前意图（intent）按需重建历史信息** 为此，论文提出State-Adaptive Memory (SAM) 框架，通过"线索-页面"（cue-page）架构将历史组织为可导航的记忆空间：紧凑的记忆线索（memory cues）作为持久指针保留在上下文中，而原始轨迹页面（raw trajectory pages）被外部存储，支持基于当前意图的按需重建（intent-driven recall）。
## MemCog: From Memory-as-Tool to Memory-as-Cognition in Conversational Agents【Memory】

[https://arxiv.org/pdf/2605.28046](https://arxiv.org/pdf/2605.28046)

- 论文提出 MemCog 系统，实现了从 Memory-as-Tool 向 Memory-as-Cognition（记忆即认知）的范式转变： 主动推理协议（Proactive Reasoning Protocol）：**使智能体能够基于对话上下文自发启动记忆探索**，消除调用瓶颈。 跨维度导航接口（Cross-Dimensional Navigation Interface）：将单次检索替换为多步、推理驱动的遍历过程，实现检索与推理的紧密耦合。 可导航记忆存储（Navigable Memory Store）：通过维度-页面-章节的三层粒度组织及跨维度关联链接，为智能体提供结构化的导航路径。 此外，论文构建了 ProactiveMemBench，这是首个专门评估智能体主动记忆触发能力的基准测试，填补了现有基准仅关注被动检索场景的空白。
## Rethinking Memory as Continuously Evolving Connectivity【Memory】

[https://arxiv.org/pdf/2605.28773](https://arxiv.org/pdf/2605.28773)

- 论文提出**FluxMem**框架，将记忆重新概念化为**持续演化的连接性（Continuously Evolving Connectivity）**，通过**异构图建模和三个阶段（初始连接形成、反馈驱动细化、长期巩固）的演化管道，实现记忆拓扑结构的动态优化**。
## How LoRA Remembers? A Parametric Memory Law for LLM Finetuning【Memory】

[https://arxiv.org/pdf/2605.30260](https://arxiv.org/pdf/2605.30260)

- 论文通过将LoRA作为潜在空间中可控的记忆容量探针，系统探究：什么支配原则决定了精确参数记忆的容量边界与动态机制？
## ⭐⭐WorldMemArena: Evaluating Multimodal Agent Memory Through Action-World Interaction【Memory, Multimodal】

[https://arxiv.org/pdf/2605.29341](https://arxiv.org/pdf/2605.29341)

- 论文提出了WorldMemArena基准测试，其核心创新包括： Action-World Interaction Loop框架：**将多模态智能体记忆重新定义为一个可观察的四阶段生命周期（观察-写入 → 更新-整合 → 检索-决策 → 行动-干预）**。 双领域覆盖：包含终身进化（跨会话的个人和任务状态演变）和智能体执行（从真实观察、行动和反馈中提取记忆）两个互补领域。 细粒度标注：每个会话标注了黄金记忆点、状态更新、干扰项和证据链，支持对记忆生命周期各阶段的独立诊断。 通过这一框架，论文**首次实现了对三种代表性记忆范式（长上下文、手工设计系统、Harness-based智能体）的统一比较**，并揭示了更好的记忆写入和存储并不能保证更好的性能、多模态记忆仍难以充分利用视觉证据等关键发现。关键结论： 更好的记忆写入和存储不能保证更好的决策性能 多模态记忆在复杂视觉推理中仍是瓶颈，视觉证据常被压缩为文本 系统在 Agentic Execution 任务上性能显著下降，难以从行动轨迹中提取经验 多数系统表现为"仅追加"更新，缺乏有效的记忆维护和冲突解决
## 🧐MemTrace: Tracing and Attributing Errors in Large Language Model Memory Systems【Memory, Alibaba】

[https://arxiv.org/pdf/2605.28732](https://arxiv.org/pdf/2605.28732)

- **大语言模型（LLM）记忆系统中的错误追踪与归因问题**。论文提出了一个统一框架，将记忆系统的执行过程转化为**可执行的记忆演化图（execution graph）**，并开发了自动归因方法（MemTrace），以在操作层面（operation-level）精准定位导致失败的最早且最小的因果割集（decisive error set）。此外，论文还构建了诊断基准 MemTraceBench，用于系统性地研究记忆失效模式。**构建MemTraceBench（MIT许可证），包含：160个真实故障案例，来自4种代表性记忆系统（LongContext、RAG、Mem0、EverMemOS）和3个公开基准（LoCoMo、LongMemEval、RealMem）**；7类细粒度错误类型：提取错误、更新错误、删除错误、检索错误、响应错误，以及非系统级错误（标注错误、LLM评判错误）；关键发现：29.13%的"系统失败"实际源于标注或评判错误，揭示了长期记忆基准构建的内在困难。
## Bian Que: An Agentic Framework with Flexible Skill Arrangement for Online System Operations【Kuaishou】

[https://arxiv.org/pdf/2604.26805](https://arxiv.org/pdf/2604.26805)

- 论文提出BIAN QUE框架，通过以下机制解决这些问题： **将异构运维工作抽象为三种统一范式（发布拦截、主动巡检、告警根因分析）**； 引入灵活技能安排（Flexible Skill Arrangement），使每个技能（Skill）能够针对特定业务-模块上下文指定需检索的数据与知识，并支持通过自然语言指令自动或迭代更新； 建立统一自进化机制，利用单一的工程师反馈信号同时驱动知识库蒸馏和技能 refinement，实现数据-知识映射与领域知识的协同演化。
## 📌DeepTutor: Towards Agentic Personalized Tutoring【Chao Huang】

[https://arxiv.org/pdf/2604.26962](https://arxiv.org/pdf/2604.26962)

- 论文提出 DEEPTUTOR 框架，通过以下设计实现突破： **混合个性化引擎：融合静态知识基础（知识图谱+嵌入索引）与动态多分辨率记忆（Trace Forest）**，**构建持续演化的学习者画像**； 闭环辅导架构：建立引用基础的解题（citation-grounded problem solving）与难度校准的出题（difficulty-calibrated question generation）之间的双向耦合； 主动式多智能体层（TutorBot）：通过可扩展技能、多智能体协调与统一的多渠道接入，将被动辅导扩展为具备长期记忆和主动干预能力的 companionship。
## Claw-Eval-Live: A Live Agent Benchmark for Evolving Real-World Workflows【Claw】

[https://arxiv.org/pdf/2604.28139](https://arxiv.org/pdf/2604.28139)

- 本文提出Claw-Eval-Live框架，其核心设计包括： 可刷新的信号层与可复现的快照分离：通过从公共工作流需求信号（如ClawHub Top-500技能）构建任务分布，确保基准测试能够随外部需求演变而更新，同时保持每个发布版本的时间戳快照具有可复现性；基于执行证据的混合评分：结合确定性检查（工具调用日志、服务审计轨迹、工作空间后验状态）与结构化LLM评判，将评分锚定于可观察的Agent行为轨迹，而非仅依赖最终输出的表面合理性；跨执行表面的统一评估：同时覆盖服务支持的业务工作流（涉及CRM、财务、邮件等系统交互）和本地工作空间修复任务，解决现有基准测试通常只覆盖单一交互环境的问题。
## ClawGym: A Scalable Framework for Building Effective Claw Agents【Claw, Personalize, IQuest】

[https://arxiv.org/pdf/2604.26904](https://arxiv.org/pdf/2604.26904)

- 该论文致力于构建**ClawGym**——一个数据为中心的可扩展框架，通过双向数据合成策略（persona-driven与skill-grounded）、黑盒轨迹收集与强化学习 pipeline，以及严格校准的评估基准，系统性解决Claw-style个人agent在数据、训练与评估三方面的基础瓶颈。ClawGym-SynData：双向数据合成 Persona-driven 自上而下合成：基于用户画像、9 大类/43 子类场景与 7 类/26 种原子操作生成多样化任务，确保覆盖真实用户需求。 Skill-grounded 自下而上合成：从 16K 可合成 OpenClaw 技能中组合主技能与辅助技能（最多 3 个），构建可操作的多步骤工作流。
## ClawForge: Generating Executable Interactive Benchmarks for Command-Line Agents【Claw】

[https://arxiv.org/pdf/2605.14133](https://arxiv.org/pdf/2605.14133)

- 论文提出 ClawForge，一个基于生成器的可执行基准测试框架，通过以下机制解决上述问题： **自动化任务生成：将场景模板、 grounded 变量、初始化状态、参考轨迹和验证器编译为可复现的任务规范 τ=(x,S0,C⋆,E,m)** 状态冲突工作流：专门测试智能体在处理部分完成、陈旧或冲突状态时的能力（如错误状态替换、中断工作流恢复、多源分支解析） 结果优先评估：基于归一化最终状态（normalized end state）和可观察副作用进行评分，而非精确匹配命令序列，从而区分"早期崩溃"与"接近完成的失败"（near-miss closures） 该框架实例化为 ClawForge-Bench（17个场景，6个能力类别），实验表明即使是最先进的模型在严格准确率上也只能达到45.3%，在错误状态替换任务上甚至低于17%，证明了现有智能体在状态冲突工作流中的显著能力缺口。
## 🧐From Context to Skills: Can Language Models Learn from Context Skillfully?【Skill】

[https://arxiv.org/pdf/2604.27660](https://arxiv.org/pdf/2604.27660)

- **Ctx2Skill**，一种无需人工监督或外部反馈即可从复杂上下文中自主发现、提炼和选择特定技能的自进化框架，旨在解决语言模型在上下文学习（Context Learning）中面临的两个核心挑战：**手动技能注释成本高昂**与**缺乏自动化技能构建所需的反馈信号**。
## 🧐SkillOS: Learning Skill Curation for Self-Evolving Agents【Skill, Google】

[https://arxiv.org/pdf/2605.06614](https://arxiv.org/pdf/2605.06614)

- 论文提出SkillOS——一种基于经验驱动的强化学习（RL）训练方法，通过以下机制学习技能策划能力： 模块化架构：将**冻结的代理执行器（Agent Executor）**与**可训练的技能策划器（Skill Curator）**解耦，前者负责任务执行，后者通过文件I/O操作管理外部技能库（SkillRepo） 分组任务流训练：构建基于技能相关性的任务组（grouped task streams），使早期轨迹更新的技能能通过后续相关任务进行评估，提供长程学习信号 复合奖励设计：结合任务结果、函数调用有效性、技能质量和库紧凑性等多维信号，将延迟和间接的反馈转化为有效的学习信号 通过这种方式，SkillOS旨在使代理能够从经验中自动学习如何有效地策划、更新和管理技能库，从而实现真正的自我进化能力。SkillOS 将技能策划形式化为**长程、以执行器为基础的学习问题**，通过分组相关任务构建训练实例，并结合下游任务结果与中间奖励，将延迟且间接的反馈转化为技能策划的有效学习信号。
## Skill1: Unified Evolution of Skill-Augmented Agents via Reinforcement Learning【Skill, LongCat】

[https://arxiv.org/pdf/2605.06130](https://arxiv.org/pdf/2605.06130)

- **技能增强型智能体（skill-augmented agents）在持续技能库维护过程中面临的优化瓶颈与目标冲突问题**。论文提出Skill1框架，核心贡献在于实现统一进化（unified evolution）： 训练**单一策略（single policy）**同时覆盖技能选择、利用和提炼三个阶段****； 基于单一**任务结果信号（task-outcome signal）**分解出各阶段的学习信号：利用低频趋势（low-frequency trend）奖励选择，利用高频变化（high-frequency variation）奖励提炼，从而确保三种能力向共享目标协同优化。
## SkillRet: A Large-Scale Benchmark for Skill Retrieval in LLM Agents【Skill】

[https://arxiv.org/pdf/2605.05726](https://arxiv.org/pdf/2605.05726)

- 论文引入了SKILLRET，一个专门用于LLM代理技能检索的大规模基准，其特点包括：规模：**包含17,810个经过筛选的公共代理技能，组织成6个主要类别和18个子类别的两级分类体系**。数据分割：提供63,259个训练样本和4,997个评估查询，且训练集和评估集的技能池互不相交，支持检索导向的模型训练和受控评估。现实特征：捕捉了真实检索环境的特征，包括长上下文技能文档（中位数1,583个token）和失衡的技能分布。
## Group of Skills: Group-Structured Skill Retrieval for Agent Skill Libraries【Skill】

[https://arxiv.org/pdf/2605.06978](https://arxiv.org/pdf/2605.06978)

- GOSKILLS 将检索单位从**原子技能**转变为**锚点中心的技能组**（anchor-centered groups），通过离线图构建与在线推理生成显式执行合同。
## Ace-Skill: Bootstrapping Multimodal Agents with Prioritized and Clustered Evolution【Skill】

[https://arxiv.org/pdf/2605.08887](https://arxiv.org/pdf/2605.08887)

- 论文提出 ACE-SKILL 框架，将计算资源分配与知识组织视为耦合优化问题，通过协同演化的方式将其转变为良性循环（virtuous cycle）： 优先采样器（Prioritized Sampler）：基于贝叶斯熟练度估计（lazy-decay proficiency tracking），主动将 rollout 资源导向信息量大且掌握不足的样本； 聚类组织器（Clustered Organizer）：通过语义聚类将知识隔离到相干的簇中，每个簇维护双粒度知识（实例级经验与类别级技能），减少检索噪声并提升任务对齐性。
## Swarm Skills: A Portable, Self-Evolving Multi-Agent System Specification for Coordination Engineering【Skill】

[https://arxiv.org/pdf/2605.10052](https://arxiv.org/pdf/2605.10052)

- 论文提出Swarm Skills规范——首个扩展Anthropic Skills标准的多智能体语义规范，将多智能体工作流转化为具备"**角色定义（Roles）、工作流（Workflow）、执行边界（Execution Bounds）**"五大组件的便携资产，并内置Evolution Experience语义结构。配套提出的自进化算法**通过CREATE（轨迹蒸馏）、PATCH（摩擦驱动的增量优化）和 Governance（基于E -U -F 多维评分的精简/重建/回滚）机制，实现协调协议的持续自主精炼**，无需人工介入。
## 📖SkillMaster: Toward Autonomous Skill Mastery in LLM Agents【Skill】

[https://arxiv.org/pdf/2605.08693](https://arxiv.org/pdf/2605.08693)

- 论文提出了 SKILLMASTER 框架，通过以下机制实现技能管理的内生化和可学习化： 轨迹感知的技能审查（Trajectory-informed Skill Review）：允许智能体基于已完成 episodes 的证据，通过工具调用（tool calls）主动提议、更新或保留技能 反事实效用奖励（Counterfactual Skill Utility Reward）：通过在相关探测任务（probe tasks）上比较技能修改前后的性能差异，为技能编辑决策提供显式的质量信号 DualAdv-GRPO 优化算法：分别为任务执行动作和技能编辑决策估计独立的优势（advantages），实现异构阶段的解耦优化 该方法将技能管理从外部维护流程转变为智能体策略内部的优化问题，使 LLM 智能体从"使用技能"进化为"掌握技能"的自改进系统。
## 📖SkillEvolver: Skill Learning as a Meta-Skill【Skill】

[https://arxiv.org/pdf/2605.10500](https://arxiv.org/pdf/2605.10500)

- 论文提出的核心问题是：智能体**能否在有限的部署时试验（deployment-time trials）内，将程序性知识获取并封装为可重用的技能制品**，而无需重新训练模型权重？ 具体挑战包括： 如何在仅有少量探索试验（如4次）的条件下完成技能创作； 如何通过实际部署反馈（而非仅作者自身的反思）识别技能缺陷，包括"静默绕过"（skill appears valid but is never invoked）等部署特定故障； 如何确保学习到的技能是可迁移的制品（prose + code），可被不同智能体加载，而非嵌入模型参数的知识。为此，论文提出 SkillEvolver——一个轻量级的元技能（meta-skill）框架，通过"创作-部署-审计-精炼"的闭环，将技能学习本身视为一种技能，使标准CLI智能体能够在线完成领域特定技能的进化。
## SearchSkill: Teaching LLMs to Use Search Tools with Evolving Skill Banks【Skill】

[https://arxiv.org/pdf/2605.09038](https://arxiv.org/pdf/2605.09038)

- **LLMs在使用搜索工具时缺乏高质量的查询规划能力**。论文提出**SEARCHSKILL**框架，通过显式的技能选择机制（skill-conditioned query planning）**将搜索工具使用分解为"选择技能→执行技能"两个阶段**，并维护一个可进化的技能库（SkillBank）来捕获和重用有效的搜索模式，从而将搜索行为从"无差别动作"转变为"受技能引导的显式规划"。
## Dynamic Skill Lifecycle Management for Agentic Reinforcement Learning【Skill】

[https://arxiv.org/pdf/2605.10923](https://arxiv.org/pdf/2605.10923)

- 论文主张，技能应**根据其边际外部贡献（Marginal External Contribution, MEC）动态经历保留（Retain）、**退役（Retire）或扩展（Expand）**的生命周期操作**，从而在学习过程中自适应地确定模型参数与外部模块化技能之间的最优能力边界，而非强制走向完全积累或零技能推理的终点。
## Skill-R1: Agent Skill Evolution via Reinforcement Learning【Skill, Adobe】

[https://arxiv.org/pdf/2605.09359](https://arxiv.org/pdf/2605.09359)

- Skill-R1 将技能演化形式化为**基于可验证奖励的双层强化学习问题**，通过冻结任务模型与**训练轻量级技能生成器**的解耦架构，实现了对开源与闭源模型均兼容的实例级递归优化。实验表明，该方法在多步推理与工具使用任务上显著优于传统方法，为智能体技能的高效进化提供了可扩展的替代方案。
## SkillGen: Verified Inference-Time Agent Skill Synthesis【Skill】

[https://arxiv.org/pdf/2605.10999](https://arxiv.org/pdf/2605.10999)

- 论文提出 SKILLGEN，一个多智能体框架，其核心创新包括： 对比行为归纳（Contrastive Behavioral Induction）：**通过对比成功与失败轨迹，提取可复用的成功模式、识别反复出现的失败模式，并发现"邻近成功案例中存在但失败案例中缺失"的关键行为**。 干预式验证（Interventional Verification）：将技能建模为对基线智能体的干预，通过在验证子集上计算**修复数（repairs）与回归数（regressions）**之差 Gm(s)=n01(s)−n10(s) ，实证验证技能的净效应。 **生成-验证-精炼循环（Generation-Verification-Refinement Loop）：迭代生成候选技能，基于结构化反馈（保留/移除/添加/强调）进行精炼，并通过验证门（verification gate）仅部署具有正净效应的技能**。 简言之，该论文**将技能合成重新定义为基于对比归纳的干预优化问题，确保生成的技能不仅是轨迹的总结，而是经过实证验证、能够提升整体性能的可审计人工制品。**
## MMSkills: Towards Multimodal Skills for General Visual Agents【Skill】

[https://arxiv.org/pdf/2605.13527](https://arxiv.org/pdf/2605.13527)

- 解决**视觉智能体（Visual Agents）缺乏有效的多模态程序性知识表示与利用机制**的问题。**现有技能系统主要将可重用行为编码为文本提示、代码或学习例程**，难以满足视觉决策中对视觉证据的依赖。提出MMSkill包结构，包含：可重用文本程序（P）运行时状态卡片（S，包含何时使用/不使用、可见线索、验证线索）多视角关键帧（K，包含全帧、聚焦裁剪、前后对比视图）。
## SkillFlow: Flow-Driven Recursive Skill Evolution for Agentic Orchestration【Skill】

[https://arxiv.org/pdf/2605.14089](https://arxiv.org/pdf/2605.14089)

- 论文提出SkillFlow框架，其核心贡献包括： **采用**Tempered Trajectory Balance (TTB)**损失函数，实现与奖励成比例的轨迹采样（reward-proportional sampling），保留多样化的编排策略而非坍缩到单一模式**； 联合学习反向策略（backward policy），在零额外推理成本下提供透明的逐步信用分配（per-step credit assignment）； 基于**流诊断（flow diagnostics）的递归技能进化机制，从训练信号本身直接推导何时进化、进化什么技能以及缺口位于何处**，形成从训练信号到自主能力增长的闭环。
## SkillSmith: Compiling Agent Skills into Boundary-Guided Runtime Interfaces【Skill】

[https://arxiv.org/pdf/2605.15215](https://arxiv.org/pdf/2605.15215)

- 论文提出了**SkillSmith**，一个**边界优先的编译器-运行时框架（boundary-first compiler-runtime framework）**。该框架**通过离线将技能包编译为最小可执行接口（minimal executable interfaces），提取细粒度的操作边界（operational boundaries），使智能体能够在运行时动态访问和执行仅相关的组件**，从而最小化不必要的上下文注入和冗余推理开销。
## 🧐SkillGenBench: Benchmarking Skill Generation Pipelines for LLM Agents【Skill】

[https://arxiv.org/pdf/2605.18693](https://arxiv.org/pdf/2605.18693)

- **现有基准测试未能将技能生成（skill generation）本身作为独立的研究对象进行系统评估**。论文提出SkillGenBench基准，通过以下设计解决上述问题： **解耦评估：将上游技能生成与下游执行分离**，在固定执行器（fixed harnesses）和确定性验证（deterministic execution-based checks）下评估生成的技能工件 双轨设置：**覆盖任务条件生成（task-conditioned，任务已知时的针对性蒸馏）和任务无关生成（task-agnostic，任务未知前的可复用技能库构建） **跨源统一：统一评估从代码仓库（隐式程序知识）和长文档（显式但分散的约束）中生成技能的能力 简言之，该论文试图建立技能生成作为agent系统中独立研究问题的评估范式，填补"如何从原始语料可靠地蒸馏可执行程序知识"这一关键空白。基于187个任务的评估（6个生成骨干 × 5种生成方法）： 性能差异显著：**无单一方法全面占优，生成效果高度依赖方法-骨干-源类型的交互（如SKILLSEEKERS在多数骨干上领先，但SKILLNET在Qwen3.6-Plus上最优）** 源类型难度分化：仓库任务通过率（10.8%–14.4%）显著低于文档任务（21.4%–25.0%），反映从分布式代码工件恢复隐式执行结构的额外困难 任务无关生成的脆弱性：无任务指导时，生成技能常因过度抽象而丢失关键约束，部分设置下甚至出现负迁移（性能低于无技能基线） 规范与执行的鸿沟：静态结构完整性（如SKILLNET在Environment、Grounding维度的高分）不保证动态执行成功，反之亦然。核心瓶颈在于将规范准确转化为可执行代码，而非仅提取表面结构 失败模式分化： 仓库任务：运行时/依赖问题（53%）、接口/模式错误（27%） 代码文档：接口/模式错误占绝对主导（85%） 领域知识文档：状态/规则错误（44%）、数值/公式错误（37%）
## Harnessing LLM Agents with Skill Programs【Skill, Harness, Salesforce】

[https://arxiv.org/pdf/2605.17734](https://arxiv.org/pdf/2605.17734)

- 解决**如何****将大型语言模型（LLM）智能体从过去经验中习得的可重用技能，从被动的文本建议转化为可执行的、能够显式干预智能体决策循环的控制机制**的问题。论文提出 HASP（Harnessing LLM Agents with Skill Programs） 框架，通过引入程序函数（Program Functions, PFs） 将技能重构为可执行的状态-动作干预函数： 显式激活机制：**每个PF包含 should_activate() 接口，基于当前状态和候选动作决定是否干预** 直接干预能力：通过 intervene() 接口执行两种干预： 动作覆盖（Action Override）：直接修改或重定向下一步动作（如将过早的终止转换为搜索操作） 上下文注入（Context Injection）：向推理过程注入纠正性上下文（如警告信息） 结构化监督信号：每次PF执行生成包含干预时机、模式、正确性和结果的结构化记录，支持后训练（post-training）和技能库演化。
## The Scaling Laws of Skills in LLM Agent Systems【Skill】

[https://arxiv.org/pdf/2605.16508](https://arxiv.org/pdf/2605.16508)

- **解决LLM智能体系统中技能库（skill library）规模扩大时的缩放规律（scaling laws）****问题**，具体聚焦于技能积累成大型可重用库后，路由（routing）与执行（execution）两个阶段的动态变化及其相互作用。在状态实现（state realization）之前，单步路由准确率随库规模 N 呈对数衰减。
## SkillsVote: Lifecycle Governance of Agent Skills from Collection, Recommendation to Evolution【Skill】

[https://arxiv.org/pdf/2605.18401](https://arxiv.org/pdf/2605.18401)

- 解决**长程LLM智能体（Long-horizon LLM agents）在经验重用与技能生态系统治理中的关键挑战**。SkillsVote框架通过以下机制回应上述问题： 预任务智能体搜索：将技能推荐 formulate 为结构化技能库上的智能体搜索（agentic search），控制暴露给求解智能体的技能集合，减少无关技能造成的负面迁移（negative transfer）。 子任务级归因：将执行轨迹分解为具有独立目标、评估信号和技能关联的细粒度子任务（subtasks），并基于结果证据（environment feedback）、责任分配（responsibility assignment）和可重用增量（reusable delta）进行归因。 证据门控演化：仅允许成功的、具有可重用探索的、归因明确的子任务触发技能库更新（编辑现有技能或创建新技能），防止虚假成功或环境导致的失败污染技能库。 实验表明，该治理框架能够在无需更新模型参数的情况下，通过受控的外部技能库提升冻结智能体在终端操作（Terminal-Bench 2.0）和软件工程（SWE-Bench Pro）基准上的性能。
## CODESKILL: Learning Self-Evolving Skills for Coding Agents【Skill, Coding】

[https://arxiv.org/pdf/2605.25430](https://arxiv.org/pdf/2605.25430)

- 解决**编程智能体（coding agents）****如何自动地从历史轨迹中提取、演化和维护可重用的程序性技能（procedural skills）****，以提升下游任务性能**的问题。论文提出 CODESKILL 框架，将技能管理重新形式化为可学习的管理策略（learnable management policy），通过强化学习（GRPO）优化，结合： 密集的质量反馈（基于评分细则的 LLM-as-judge） 稀疏的执行反馈（下游冻结智能体的任务通过率） 从而学习何时提取新技能、如何修订现有技能、以及如何维护紧凑的技能库，使技能既具备跨任务可迁移性，又能有效指导具体决策。
## ⭐SkillEvolBench: Benchmarking the Evolution from Episodic Experience to Procedural Skills【Skill】

[https://arxiv.org/pdf/2605.24117](https://arxiv.org/pdf/2605.24117)

- SkillEvolBench 试图回答：**何时一次性任务经验能成为可持久化的程序知识，而非仅停留在任务局部记忆或片段重放？** 论文通过系统性地测量从经验获取到技能冻结部署的全过程，为研究选择性程序抽象（selective procedural abstraction）提供了诊断平台。论文发现： 局部适应 vs. 技能形成：**当前智能体常表现出局部程序适应（提升获取/重放成功率），但无法形成在冻结部署中稳健的、可重用的技能（部署成功率不稳定或下降）** 有损抽象瓶颈：**与蒸馏技能相比，原始轨迹（Raw-Trajectory）在冻结评估、上下文转移、对抗性和组合性任务上通常表现更好，表明当前抽象过程会丢弃对未来任务仍有用的上下文和程序线索** 容量≠能力：强制要求技能库包含更多资源文件（Tier-3强制）虽能增加库大小，但常引入片段特定漂移和程序性杂乱，反而降低部署成功率 模型依赖性：技能演化效果高度依赖基础模型能力（如Opus 4.5普遍受益，而Gemini 2.5 Pro普遍受损）
## You Live More Than Once: Towards Hierarchical Skill Meta-Evolving【Skill】

[https://arxiv.org/pdf/2605.28390](https://arxiv.org/pdf/2605.28390)

- 针对**大型语言模型（LLM）代理系统的测试时技能进化（test-time skill evolving）**问题，论文**提出了层次化技能元进化（HiSME）框架，将技能进化视为对执行器效用的一阶残差优化，而将进化策略的优化视为二阶残差优化**。通过从代理的任务执行轨迹中学习**元技能（meta-skills）**，HiSME能够在不修改底层LLM参数的情况下，轻量级地联合优化任务技能和技能维护过程本身，从而实现持续改进的代理系统。**核心方法：HiSME HiSME 将技能进化建模为多层级残差优化问题： 零阶：优化固定执行器的期望效用 J0(EXE) 一阶：通过技能进化优化残差 J1(EVO)=E[J0(EXEST)−J0(EXE0)] 二阶：通过**元技能（meta-skills）**优化进化算法本身，消除 J1(EVO0) 与最优策略间的差距 ϵ1。**
## SkillGrad: Optimizing Agent Skills Like Gradient Descent【Skill】

[https://arxiv.org/pdf/2605.27760](https://arxiv.org/pdf/2605.27760)

- 核心解决方案 论文提出SkillGrad框架，将技能优化形式化为类梯度下降的迭代过程： **将技能包 St 视为可优化的结构化参数 任务执行结果 rt,i 和轨迹 τt,i** 提供损失证据（Loss Evidence） 诊断器生成文本梯度（Textual Gradients）dt,i ，指示修正方向 动量机制（Momentum）累积跨迭代的重复模式，稳定优化过程 分层补丁器（Layer-aware Patcher）执行参数更新，决定知识应写入L2还是L3层 通过这一框架，论文实现了对初始技能包的系统性、原则性改进，在表格操作基准测试（SpreadsheetBench Verified）和领域外测试（WikiTableQuestions）上均显著优于现有基线方法。
## Skill-Conditioned Gated Self-Distillation for LLM Reasoning【Skill】

[https://arxiv.org/pdf/2605.28791](https://arxiv.org/pdf/2605.28791)

- 解决**基于技能库的自蒸馏（Self-Distillation, SD）中特权信息（Privileged Information, PI）可靠性不足的问题。**论文提出**将基于技能的自蒸馏重新框架化为教师假设验证问题（teacher hypothesis validation），而非传统的无条件模仿**，**通过验证器反馈来推断每位技能条件教师的极性（ helpful vs. misleading），并采用鲁棒的门控目标函数（gated objective）进行选择性蒸馏**。
## OptSkills: Learning Generalizable Optimization Skills from Problem Archetypes via Cluster-Based Distillation【Skill, Ant】

[https://arxiv.org/pdf/2605.29829](https://arxiv.org/pdf/2605.29829)

- 论文提出OPTSKILLS框架，其核心思想是： 原型中心表示：通过提取优化要素（ingredients）和场景中性编辑（problem editing），构建剥离表面叙事的问题原型嵌入（archetype embedding），将问题按底层数学结构而非文本相似性进行聚类； **聚类级技能蒸馏：在每个原型簇内探索多样化的建模与求解轨迹，将成功与失败经验蒸馏为结构化的、可重用的技能文档**（包含标准操作流程与常见陷阱）； 持续技能学习：通过技能选择器（skill selector）判断新问题是否匹配现有技能，进而触发技能精细化或技能扩展，实现对新原型的自适应覆盖。 简言之，论文旨在构建一种能够从优化问题原型中自动学习、积累并泛化求解技能的智能体系统，从而突破现有方法在叙述鲁棒性、分布内复用性和分布外适应性上的瓶颈。
## SkillsInjector: Dynamic Skill Context Construction for LLM Agents【Skill, AI Lab】

[https://arxiv.org/pdf/2605.29794](https://arxiv.org/pdf/2605.29794)

- 解决**大语言模型（LLM）Agent在技能注入（skill injection）过程中面临的静态上下文构建瓶颈**。论文提出将技能注入重新定义为**任务自适应的动态上下文构建问题，旨在通过联合优化技能选择、自适应预算分配和集合感知的描述渲染**，为冻结策略的Agent构建最优的技能上下文。
## SkillBrew: Multi-Objective Curation of Skill Banks for LLM Agents【Skill】

[https://arxiv.org/pdf/2605.29440](https://arxiv.org/pdf/2605.29440)

- 论文提出**SkillBrew**框架，将技能库策划形式化为受约束的多目标优化问题（以有用性为硬约束，多样性/覆盖率为结构正则化），通过双层"提议-验证"循环（bi-level propose-then-verify loop）和Pareto感知选择机制，迭代地生成候选编辑并验证其多目标性能，从而构建高质量、自改进的技能库。论文提出 **SkillBrew**，一种训练自由（training-free）的双层迭代优化框架，将技能库策展形式化为受约束的多目标优化问题，其中效用作为硬约束，多样性与覆盖率作为结构正则化，确保技能库首先有用，其次组织良好。
## ⭐GRASP: Gated Regression-Aware Skill Proposer for Self-Improving LLM Agents【Skill】

[https://arxiv.org/pdf/2605.29668](https://arxiv.org/pdf/2605.29668)

- 解决**LLM智能体在结构化环境中自改进时的回归问题。**论文提出**将智能体改进重新定义为对有限技能库的受控编辑序列（gated editing of a bounded library），通过引入回归感知的接受门控（regression-aware acceptance gate），确保每个候选技能编辑仅在通过留存探针验证、产生净改进**（修复数 > 回归数）且满足硬回归预算（$R(c) \leq R_0$）时才会被接受。
## SKILLC: Learning Autonomous Skill Internalization in LLM Agents via Contrastive Credit Assignment【Skill, Meituan】

[https://arxiv.org/pdf/2605.27899](https://arxiv.org/pdf/2605.27899)

- 论文提出SKILLC框架，通过**对比技能信用分配（CSCA）**机制，**将任务级别的性能对比（Δ(x)）直接注入策略优化过程，使梯度能够主动识别并强化那些无需技能支持即可成功的自主行为**，从而实现从"技能增强"到"技能内化"的有效转化。
## MUSE-Autoskill: Self-Evolving Agents via Skill Creation, Memory, Management, and Evaluation【Skill, ByteDance】

[https://arxiv.org/pdf/2605.27366](https://arxiv.org/pdf/2605.27366)

- 论文提出 MUSE-Autoskill Agent（Memory-Utilizing Skill Evolution），通过统一的技能生命周期（Skill Lifecycle）解决上述问题： 五阶段生命周期：**创建（Creation）→ 记忆（Memory）→ 管理（Management）→ 评估（Evaluation）→ 改进（Refinement）** 关键创新： 将技能创建与执行紧密耦合（通过 skill_create 工具在运行时按需生成） 引入技能级记忆（skill-level memory），**为每个技能维护独立的经验积累文件（.memory.md） 基于单元测试和运行时反馈的自动化评估与改进循环** 自适应上下文压缩与跨会话状态持久化。
## ⭐⭐⭐SkillOpt: Executive Strategy for Self-Evolving Agent Skills【Skill, Microsoft】

[https://arxiv.org/pdf/2605.23904](https://arxiv.org/pdf/2605.23904)

- 论文提出将技能编辑重新定义为可控的域适应训练过程：将技能文档视为冻结Agent的外部可训练状态，引入独立的优化器模型，通过以下机制实现文本空间的稳定优化： 有界编辑（bounded add/delete/replace edits）：**类比学习率的文本预算控制** 保留验证门控（held-out validation gate）：仅当候选技能在验证集上严格提升时才接受更新 拒绝编辑缓冲（rejected-edit buffer）：**将失败编辑转化为负面反馈** ** epoch级慢/元更新**（epoch-wise slow/meta update）：捕获跨周期的长期规律 通过这种方式，SkillOpt试图建立首个系统化的文本空间优化器，使Agent技能能够像神经网络权重一样被训练、验证和部署，同时保持零推理时开销（zero inference-time model calls at deployment）。
## ⭐⭐⭐From Raw Experience to Skill Consumption: A Systematic Study of Model-Generated Agent Skills【Skill, Microsoft】

[https://arxiv.org/pdf/2605.23899](https://arxiv.org/pdf/2605.23899)

- 这篇论文对**模型生成的领域级智能体技能（model-generated domain-level agent skills）**进行了全生命周期的系统性研究。核心发现 RQ1：技能效用是否可靠？ 非平凡负向迁移：模型生成技能平均有效（75%组合正向），但25%组合出现性能下降（ALFWorld领域高达47%）； 能力解耦：提取效能与目标可进化性不相关且与模型规模无关。例如，轻量级 Gemini-3.1-Flash-Lite 在 SpreadsheetBench 上提取效能最高，而最强的 GPT-5.4 反而最低；同一组提取器对不同目标的增益差异巨大。 RQ2：生命周期的驱动因素？ 经验生成：经验池的成功-失败比例显著影响质量。纯失败池始终表现最差，但最优比例领域特异（ALFWorld受益于失败主导，SpreadsheetBench偏好成功主导）； 技能提取：技能格式（列表/散文）和文本流畅度不预测效用（LLM判断准确率46.4%，接近随机）。**真正关键的是深层特征：失败机制编码（executable remedies for failure modes）、可执行具体性（actionable specificity）、高风险行为黑名单**； 技能消费：相同技能在不同目标上收益差异巨大（范围可从+9.5%到-2.0%）。技能通过重塑目标默认策略（如改变公式计算方式）而非显式调用来生效。 RQ3：如何改进提取？ 基于上述诊断，**论文识别出3个验证维度（失败机制编码、可执行具体性、高风险行为黑名单），将其封装为**元技能（meta-skill）**插入提取器提示**： 对比 naive 的7维度“可信度”标准（平均损害性能-0.59pp），验证标准全面改进所有测试单元（平均+1.55pp，消除负向迁移），实现即插即用的提取优化。
## 🧐Code as Agent Harness【Harness, Code, Meta】

[https://arxiv.org/pdf/2605.18747](https://arxiv.org/pdf/2605.18747)

- 核心问题是：**重新定义并系统化代码在AI代理系统中的角色，将代码从单纯的生成目标转变为可执行、可验证、有状态的代理基础设施（Agent Harness）****，以支持长期自主运行的可靠代理系统。**提出"Code as Agent Harness"视角，将代码视为代理系统的"工具"（Harness）——连接模型能力与真实世界的软件层，使代理能够： 通过执行验证推理（而非仅靠文本自评）； 通过代码状态维持长期记忆（而非仅依赖上下文窗口）； 通过共享代码制品实现多代理协调（而非仅靠消息传递）。基于发现的规律，论文实现了自动优化系统，在固定路由器的条件下：**诊断行动：最近邻审计、边界重写、抽象技能移除、提示锚定****；**效果：在N=150的保留测试集上，路由准确率从71.3%提升至91.7%，劫持率从22.4%降至4.1%下游迁移：在ClawBench（18领域，242任务）上平均通过率从49.3%提升至61.6%，在ClawMark（13领域，100任务）上从28.4%提升至34.5%。
## 🧐RewardHarness: Self-Evolving Agentic Post-Training【Harness, Wenhu Chen】

[https://arxiv.org/pdf/2605.08703](https://arxiv.org/pdf/2605.08703)

- 解决**指令引导图像编辑（instruction-guided image editing）评估中的数据效率鸿沟（data-efficiency gap）与奖励建模成本问题**。论文提出REWARDHARNESS框架，**将奖励建模重新定义为上下文进化（context evolution）而非权重优化（weight optimization）**： 通过**维护一个可进化的技能与工具库（Skills-and-Tools Library），仅使用**约100个偏好演示（0.05%的传统数据量）**进行自我进化** 冻结底层视觉语言模型（VLM）参数，通过迭代优化外部评估知识库实现对齐 产生可解释的推理链（reasoning chains），兼容API模型，无需梯度训练 该方法旨在构建数据高效、可解释、即插即用的奖励系统，弥合人类少样本学习与模型大规模训练之间的效率差距。
## Continual Harness: Online Adaptation for Self-Improving Foundation Agents【Harness, Google DeepMind】

[https://arxiv.org/pdf/2605.09998](https://arxiv.org/pdf/2605.09998)

- 解决**具身智能体（embodied agents）缺乏自动化、持续自我改进的脚手架（harness）框架**的问题。该论文的核心贡献是提出了 **Continual Harness** 框架，实现了具身智能体在**无重置、在线、自举**条件下的持续自我改进，弥合了极简基线与专家级手工设计 harness 之间的效率差距。
## Harnessing Agentic Evolution【Harness】

[https://arxiv.org/pdf/2605.13821](https://arxiv.org/pdf/2605.13821)

- 解决agentic evolution（智能体演化）在长程搜索中缺乏稳定机制来组织累积证据并修订演化策略的问题。 具体而言，现有方法主要面临以下局限： 过程驱动演化（Procedure-based evolution）的僵化性：这类方法依赖预定义的外层循环（选择、优化、评估、更新），虽然模块化但缺乏灵活性，长程搜索容易被固定的手工设计规则束缚，反复利用相同的搜索模式而陷入局部最优。 智能体驱动演化（Agent-based evolution）的漂移风险：这类方法由通用智能体管理搜索，虽能灵活整合反馈，但随着候选、日志、假设和中间文件的累积，智能体可能对误导性证据或过时假设过度承诺，导致在长程演化中发生漂移。 缺乏统一的证据组织与机制修订接口：两种形式在演化过程中都会积累丰富的证据（包括候选方案、评估反馈、执行轨迹、失败记录等），但缺乏一个稳定的接口来结构化地组织这些证据，并据此修订驱动未来搜索的底层机制。 为此，论文提出将agentic evolution形式化为一个交互式环境，其中累积的演化上下文被视为过程级状态，并通过引入AEVO框架，使元智能体（meta-agent）能够**通过编辑演化机制（而非直接生成候选）来干预搜索过程**，从而实现在长程演化中对过程和智能体上下文的持续优化。提出AEVO（Agentic Evolution with meta-editing）框架： 两阶段循环：交替执行元编辑阶段（meta-agent修订机制并设定运行计划）与演化段（执行机制生成多候选）；工具化保护（Harness）：隔离评估器防止奖励篡改，标准化工作空间布局，确保候选历史可被追溯和搜索。
## Towards Direct Evaluation of Harness Optimizers via Priority Ranking【Harness】

[https://arxiv.org/pdf/2605.22505](https://arxiv.org/pdf/2605.22505)

- **如何直接、高效地评估 harness 优化器（harness optimizers）的逐步决策能力？**论文提出了**优先级排序（priority ranking）**这一评估范式： 核心机制：**要求优化器对给定 harness 中的组件（prompt、tool、memory、workflow）按"更新该组件对智能体性能的潜在影响"进行排序**，而非生成绝对的下一代 harness； 优势：作为非迭代的文本生成任务，该设计无需昂贵的多步 rollout 或人工检查，成本比传统方法低 8 倍以上，速度快 17 倍以上； 有效性验证：优化器在优先级排序任务上的表现与其在实际多步 harness 优化中提升智能体的能力呈显著正相关（Pearson ρ=0.602 ），证明该设计是优化能力的可靠预测指标。
## Harnesses for Inference-Time Alignment over Execution Trajectories【Harness】

[https://arxiv.org/pdf/2605.21516](https://arxiv.org/pdf/2605.21516)

- 研究**大型语言模型（LLM）代理的支架工程（harness engineering）设计问题。**更精细、更复杂的支架（包含更细粒度的任务分解和更详细的执行指导）并不总能提升代理的最终任务成功率。论文试图回答：**支架应该指定什么，以及应该留给代理自行解决什么****？论文建立了形式化框架，将支架分解为******工作流（workflow，控制子目标序列）和引导（guidance，控制局部动作分布）******两个机制，通过可恢复性（recoverability）的链式法则，证明了支架设计中的边际停止规则：****当增加一个支架阶段的可靠性成本超过其尾部风险降低收益时，应停止添加结构****。**
## WindowsWorld: A Process-Centric Benchmark of Autonomous GUI Agents in Professional Cross-Application Environments【GUI】

[https://arxiv.org/pdf/2604.27776](https://arxiv.org/pdf/2604.27776)

- 论文提出WindowsWorld——一个**面向Windows虚拟机的进程感知型（Process-Centric）**基准测试，其核心创新包括：专业级多应用任务体系：基于16种职业角色生成181个任务，其中77.9%为固有跨应用任务，涵盖从L1（单应用原子操作）到L4（不可行任务）的四级难度体系细粒度中间过程检查：引入基于检查点的部分进展评分（Sint），通过验证关键子目标实现进程感知评估，解决传统二元评分的判别力不足问题人机协同多代理生成框架：通过"角色生成器-精炼器-环境生成器"的自动化流水线，显著降低数据集构建成本，同时确保任务反映真实专业工作流。
## Learn where to Click from Yourself: On-Policy Self-Distillation for GUI Grounding【GUI】

[https://arxiv.org/pdf/2605.00642](https://arxiv.org/pdf/2605.00642)

- 论文提出GUI-SD框架，通过以下机制实现精准坐标生成： 视觉特权引导（Visual Privileged Guidance）：以目标边界框（bounding box）配合高斯软掩码（Gaussian soft mask）构建教师的特权上下文，提供视觉先验知识而不泄露精确坐标，保持教师分布的丰富性； 熵引导蒸馏（Entropy-Guided Distillation）：基于数字位权（位置信用分配）和教师预测熵（置信度门控）自适应加权Token级损失，集中优化于影响最大且教师最可靠的坐标位置。
## ToolCUA: Towards Optimal GUI-Tool Path Orchestration for Computer Use Agents【GUI,  Interleaving, Tongyi】

[https://arxiv.org/pdf/2605.12481](https://arxiv.org/pdf/2605.12481)

- 解决**计算机使用智能体（Computer Use Agents, CUAs）在混合动作空间中的最优路径选择问题**，即如何有效地协调原子级GUI操作（如点击、滚动）与高级工具调用（如API操作）以形成高效可靠的任务执行轨迹。面对 GUI 操作和工具调用两种动作，智能体要么过度依赖 GUI（效率低下），要么盲目频繁调用工具（稳定性差），无法动态判断何时切换以获得最优执行轨迹。这源于： 高质量交错 GUI-Tool 轨迹稀缺——真实工具轨迹收集昂贵且脆弱 轨迹级监督信号缺失——现有方法仅提供步骤级模仿或最终任务奖励，无法区分"及时的工具调用"与"冗长的 GUI 绕行”。
## Executable Agentic Memory for GUI Agent【GUI, Memory】

[https://arxiv.org/pdf/2605.12294](https://arxiv.org/pdf/2605.12294)

- 论文提出Executable Agentic Memory (EAM) 框架，核心创新在于将GUI规划从自由形式生成（free-form generation）转变为检索-执行（retrieval-and-execution）过程： 结构化知识表征：构建GUI逻辑知识图谱（GUI Logic Knowledge Graph），将环境交互逻辑显式编码为状态机（状态节点、动作节点、确定性转移边） 样本高效的记忆构建：通过状态感知的深度优先搜索（DFS）和基于BPE的动作组挖掘（action-group mining），将多步例行程序压缩为可复用的高层动作 价值引导的路径提取：训练轻量级Q函数模型指导蒙特卡洛树搜索（MCTS），在约束化的知识图谱动作空间中提取高置信度的可执行路径 理论分析与实验验证表明，该方法在AndroidWorld等基准上较SOTA方法提升最高达19.6%的成功率，同时将token成本降低6倍，平均延迟降至2.8秒，实现了可靠、快速、长程的GUI自动化。
## MementoGUI: Learning Agentic Multimodal Memory Control for Long-Horizon GUI Agents【GUI, Memory】

[https://arxiv.org/pdf/2605.18652](https://arxiv.org/pdf/2605.18652)

- 针对**长程图形用户界面（GUI）控制中的多模态记忆管理瓶颈**展开研究。论文引入MEMENTOGUI框架，将长程GUI控制重新定义为在线多模态记忆控制问题： 工作记忆（Working Memory）：通过事件门控机制选择性保留任务相关的界面事件，结合文本摘要与ROI级视觉证据 情景记忆（Episodic Memory）：存储跨任务的可重用经验，通过学习的相关性选择机制进行检索 MEMENTOCORE：作为可插拔的学习型记忆控制器，在不微调GUI代理主干网络的前提下，实现**记忆的在线选择、压缩与检索** 该框架旨在使冻结的GUI主干网络能够基于结构化的多模态记忆上下文（而非原始历史序列）进行决策，从而在长程交互中保持连贯性、效率与可靠性。
## SE-GA: Memory-Augmented Self-Evolution for GUI Agents【GUI, Memory】

[https://arxiv.org/pdf/2605.16883](https://arxiv.org/pdf/2605.16883)

- 论文提出 SE-GA（Self-Evolving GUI Agent）框架，包含两个核心组件： Test-Time Memory Extension (TTME)：构建包含**情景记忆（Episodic）、语义记忆（Semantic）和经验记忆（Experiential）**的层次化记忆库，在推理时动态检索相关信息以支持长期规划。 Memory-Augmented Self-Evolution (MASE)：采用 hindsight goal-shifting 策略构建数据集，并结合 GRPO 强化学习算法，将记忆库中收集的高质量交互数据编码到模型参数中，实现策略的持续自我优化。
## Covering Human Action Space for Computer Use: Data Synthesis and Benchmark【GUI】

[https://arxiv.org/pdf/2605.12501](https://arxiv.org/pdf/2605.12501)

- 当前CUAs在真实工作流中的失败主要源于**动作定位（Action Grounding）错误**，且呈现明显的长尾分布：简单点击操作成功率较高，但涉及**拖拽、绘制、表格编辑、文本选择**等复杂交互时失败率显著上升。现有GUI定位基准存在"点击中心"（click-centric）和"部件中心"（widget-centric）的局限，无法评估这些复杂操作，导致模型训练与实际应用脱节。该论文提出了以下关键贡献： CUActSpot基准测试：首个覆盖五种模态（GUI、文本、表格、画布、自然图像）和多种动作类型（单点点击、双点拖拽、多点绘制）的综合评估基准，用于精确诊断模型在复杂交互上的能力边界。 渲染器驱动的数据合成管道：通过代码渲染自动生成包含精确坐标标注的多模态截图，并利用大语言模型合成多样化的操作指令与动作轨迹，从而低成本地构建大规模（50M样本）复杂交互训练数据。 多样性扩展（Variety Scaling）发现：通过消融实验证实，在预训练阶段增加任务类型和模态的多样性比单纯增加单一模态的数据量更能有效提升模型的通用交互能力。 最终，基于该数据管道训练的Phi-Ground-Any-4B模型在复杂交互基准上取得了同规模（<32B参数）开源模型中的最优性能，验证了所提方案的有效性。
## AutoRPA: Efficient GUI Automation through LLM-Driven Code Synthesis from Interactions【GUI】

[https://arxiv.org/pdf/2605.21082](https://arxiv.org/pdf/2605.21082)

- 解决**重复性图形用户界面（GUI）自动化任务中效率与开发成本之间的根本性矛盾。论文提出 AutoRPA 框架，旨在****自动将 ReAct 式智能体的决策逻辑蒸馏为健壮、可重用且 Token 高效的传统 RPA 函数****。具体目标包括： 自动化代码合成：通过探索阶段收集成功轨迹，经翻译-构建（translator-builder）流水线将硬编码动作转化为基于元素属性的软编码过程，最终生成可执行的 RPA 函数。 增强鲁棒性：****利用检索增强生成（RAG）从多轨迹中学习，结合混合修复策略（代码执行失败时回退到 ReAct 进行断点修复），使生成的代码能适应 GUI 布局变化和任务指令变异****。 显著降低运行成本：在保持或超越原始 LLM 智能体成功率的同时，将 Token 消耗降低 82%~96%，实现"一次构建、多次低成本执行"的自动化范式。**
## Scaling, Benchmarking, and Reasoning of Vision-Language Agents for Mobile GUI Navigation【GUI, Xiaomi】

[https://arxiv.org/pdf/2605.27134](https://arxiv.org/pdf/2605.27134)

- 针对**移动GUI导航中基于视觉-语言模型（VLM）智能体的数据缩放、基准测试与推理机制**面临的系统性研究缺口。论文提出了： HyperTrack：一个包含**16,000+真实任务、覆盖650+中文应用的大规模数据集**，支持域内与域外（未见过应用/设备）评估； GUIEvalKit：开源统一评估工具包，支持离线评估与半在线评估（SOEval），提供更贴近真实交互的评估方式； 系统性数据缩放研究：揭示**性能随训练数据对数线性增长**，且DAPO风格的强化学习微调在域外场景显著优于监督微调； 推理行为分析：提出决策多样性（Decision Diversity）与决策稳定性（Decision Stability）指标，发现推理模式虽降低单路径（PASS@1）稳定性，但扩展了可行决策空间，在宽容评估机制（如PASS@8或在线评估）下表现更优。
## CUA-Gym: Scaling Verifiable Training Environments and Tasks for Computer-Use Agents【GUI, Qwen】

[https://arxiv.org/pdf/2605.25624](https://arxiv.org/pdf/2605.25624)

- 解决**计算机使用智能体 (Computer-Use Agents, CUAs) 在强化学习与可验证奖励 (Reinforcement Learning with Verifiable Rewards, RLVR) 范式下的数据瓶颈问题**。为应对上述挑战，论文提出CUA-GYM，一个自动化的对抗式协同生成流水线：通过Generator和Discriminator双代理在信息隔离条件下迭代生成环境状态与奖励函数，确保奖励基于任务语义而非构造过程构建CUA-GYM-HUB，包含94个高保真模拟Web应用的可复用环境库，基于O*NET职业分类和Anthropic经济指数的真实软件使用分布构建实现大规模验证数据集（32,112个元组，覆盖110个环境），支持在桌面和Web平台上进行稳定的RLVR训练该工作**首次证明了RLVR数据配方（在数学和代码领域驱动突破的范式）可扩展至CUAs领域**，并揭示了环境多样性是与数据量互补的独立扩展维度。
## PhoneWorld: Scaling Phone-Use Agent Environments【GUI, Mobile】

[https://arxiv.org/pdf/2605.29486](https://arxiv.org/pdf/2605.29486)

- 论文将研究焦点从**"一次构建一个移动基准测试"转移到**"扩展手机使用环境本身的供应"**（scaling the supply of phone-use environments）**，通过以下方式实现规模化： 覆盖 34 个应用、16 个领域（搜索、购物、预订、社交等） 18 个可复用组件库，降低新应用构建成本 支持从 5 个应用到 34 个应用的广度扩展，在固定预算下提升模型性能 简言之，PhoneWorld 试图建立一种AI 驱动、人工审计的环境构建流程，以系统性解决手机代理训练与评估所需环境的规模化供给问题。
## STAMP: Training Explicit Memory for Mobile GUI Agents in Controllable and Scalable Virtual Environments【GUI, Tongyi】

[https://arxiv.org/pdf/2605.29324](https://arxiv.org/pdf/2605.29324)

- 论文提出通过**可控且可扩展的虚拟环境训练显式记忆**（explicit memory）： 程序化记忆控制：在合成任务中确定性**注入记忆变量，精确控制"必须记忆什么信息"、"何时出现"、"何时需要检索"**。 可验证监督与在线学习：自动生成任务级和记忆级可验证的监督数据，支持通过环境驱动奖励反馈进行大规模在线强化学习，使代理明确学习优化记忆编码行为。 端到端记忆架构：Stamp-GUI模型输出三元组 (ct,at,mt) ，其中 mt 为显式记忆字段，在截图被丢弃前主动将关键瞬时信息外化到文本上下文中。 该框架在提出的Memory-World基准上实现了记忆准确率（M-Acc）和任务成功率（T-Acc）的显著提升，证明了显式记忆对于构建具备长程韧性的移动GUI代理的必要性。
## Weblica: Scalable and Reproducible Training Environments for Visual Web Agents【Web, Apple】

[https://arxiv.org/pdf/2605.06761](https://arxiv.org/pdf/2605.06761)

- 论文提出了 WEBLICA 框架，通过两种互补机制构建可扩展且可复现的训练环境： HTTP级缓存（Weblica-Cache）：记录并重放真实网站的HTTP流量，通过**识别并归一化易变参数**（如时间戳、会话令牌），在完全网络隔离下实现确定性重放，捕捉真实网页的动态布局与交互行为。 LLM驱动的环境合成（Weblica-Synth）：**利用大语言模型（如Claude Code）自动生成基于真实网站和核心导航技能（如表单提交、认证流程、动态搜索）的交互式网页环境**，支持大规模扩展并覆盖多样化视觉风格与功能域。 通过该框架，作者实现了**在数千个多样化环境中进行稳定的RL训练**，其最佳模型WEBLICA-8B在多个网络导航基准测试上超越了同等规模的开源基线模型。
## HTMLCure: Turning Browser Experience into State Guided Repair for Interactive HTML【Web, IQuest】

[https://arxiv.org/pdf/2605.26807](https://arxiv.org/pdf/2605.26807)

- 当前LLM生成的HTML页面存在**"表面正确性陷阱"**：初始渲染成功，但在滚动、悬停、点击、调整视口或游戏交互等动态场景中频繁失效。论文提出**HTMLCURE**框架，**将浏览器执行轨迹转化为结构化的页面状态信号，通过"体验-诊断-修复-验证"的闭环流程，将原本被丢弃的弱页面和半成页面转化为高质量的训练数据**，从而扩展可监督微调（SFT）的有效语料规模。发布包含400项任务、6,000个确定性浏览器测试用例的冻结基准，覆盖6大语义家族（应用工具、游戏模拟、数据可视化、3D/WebGL、视觉艺术、内容营销）。测试用例基于浏览器动作（点击、悬停、按键、JS断言、截图变化检查），确保功能性与交互性评分的完全确定性。
## GTA: Generating Long-Horizon Tasks for Web Agents at Scale【Web, Salesforce】

[https://arxiv.org/pdf/2605.29218](https://arxiv.org/pdf/2605.29218)

- 解决**Web Agents（网络智能体）领域中长期存在的可扩展数据稀缺问题**，特别是针对多跳（multi-hop）、跨页面（cross-page）推理任务的自动化生成难题。论文提出了 GTA（Generate Long-Horizon Tasks for Web Agents at Scale） 框架，通过以下设计解决上述问题： 爬取与生成解耦：通过一次性构建站点图（site graph），避免重复的昂贵 rollouts 基于检索的种子生成：利用密集检索系统性地采样多样化的语义区域，减少探索偏见 图结构接地：将任务生成锚定在站点图结构上，强制构建需要跨页面推理的多跳任务 可执行轨迹验证：为每个任务提供确定性的最小"黄金路径"（gold path），支持步骤级诊断与确定性重放 通过这一框架，论文旨在创建一个自我演化的基准测试生态系统，能够持续从实时网络内容生成高质量、多语言、跨领域的复杂任务，从而缩小人类与智能体在真实长时程网络任务上的性能差距。
## 🧐Does The Way You Plan Matter? An Empirical Study of Planning Representations for LLM Web Agents【Web, MILA】

[https://arxiv.org/pdf/2605.29927](https://arxiv.org/pdf/2605.29927)

- 解决**大型语言模型（LLM）Web Agent中规划表征（plan representation）对任务性能与鲁棒性影响的系统性评估缺失问题**。通过**PLANAHEAD**框架，该研究系统性比较了四种规划表征（顺序子目标、需求清单、伪代码、叙述）在静态规划-执行分离架构下的表现，揭示规划形式与模型选择对Web Agent鲁棒性的显著影响。在158个Hard任务上，论文评估了GPT-4.1-mini、Qwen-2.5-VL-72B、Gemini 2.5 Flash三种模型作为Planner和Executor的所有组合（含异质配置），主要发现包括： 模型-表征适配性：**规划效能高度依赖LLM类型与表征的匹配。GPT-4.1-mini在Narrative表征下表现最优（STC达75%），Qwen-2.5-VL最适合Checklist（结构化引导），而Gemini在Pseudocode下取得最高AR（8.2%）**。 异质配置优势：**分离规划与执行职能并采用不同模型组合（如GPT-4.1-mini规划+Gemini执行）通常优于同质配置**，最佳整体性能达AR=10.7% ，表明模块化架构可利用模型间的互补优势。 静态规划有效性：在零重规划条件下，静态规划-执行分离架构仍优于动态单智能体基线（GenericAgent的UsePlan），后者易出现动作循环或过早标记子任务完成等失败模式。 统计稳健性：通过Bootstrap重采样（1000次）验证，Hard任务上的性能提升具有统计显著性，非随机噪声所致。
## X-OmniClaw Technical Report: A Unified Mobile Agent for Multimodal Understanding and Interaction【Mobile, OPPO】

[https://arxiv.org/pdf/2605.05765](https://arxiv.org/pdf/2605.05765)

- 混合UI理解（Hybrid Grounding）：动态平衡XML元数据与视觉/OCR信息，解决广告密集界面的定位模糊问题 行为克隆与深度链接提取：通过dumpsys activity内省捕获Activity的完整Intent（含URI与参数），将用户导航提炼为可复用技能卡片，支持deeplink一键直达，避免冗长UI重放 多级回退策略：执行时优先尝试完整Intent重放，失败则逐步降级至任务栈恢复，确保无公开deeplink的应用也能精确恢复页面状态 安全过滤与隐私控制：长期记忆写入前统一脱敏，提供显式用户控制（图库记忆开关、画像注入控制），并计划将语义摘要迁移至设备端以减少原始像素上传。
## How Mobile World Model Guides GUI Agents?【Mobile】

[https://arxiv.org/pdf/2605.10347](https://arxiv.org/pdf/2605.10347)

- **移动世界模型（Mobile World Model）如何有效指导GUI智能体**的核心问题，通过构建统一的数据集与模型框架，实证分析了预测模态选择、测试时交互机制及想象轨迹训练三个关键维度。论文围绕三个研究问题展开： RQ1：世界模型应预测何种模态（文本 vs. 图像，Delta vs. Full，Code vs. Diffusion）？ RQ2：测试时应如何集成（先验感知 vs. 后验评判 vs. 自我反思）？ RQ3：能否从世界模型生成的想象轨迹中学习？
## 🧐When Agents Evolve, Institutions Follow【Topo】

[https://arxiv.org/pdf/2604.27691](https://arxiv.org/pdf/2604.27691)

- 论文提出**SOCIALSYSTEMARENA**框架，将七种历史政治制度（如秦汉郡县制、唐代三省六部制、雅典民主制等）转化为可执行的多智能体架构，并在统一的运行时（Governance Runtime）上进行评估。这使得研究者能够在固定LLM后端和任务的前提下，孤立地测量治理拓扑对任务完成率、效率、鲁棒性和失败模式的影响。研究发现，**不存在普遍最优的治理形式（如门控管道、自主集群或共识机制），不同拓扑在不同模型和任务下表现差异显著（最佳与最差机构之间的差距可达57个百分点以上）**。这一发现指向了**自适应治理**的必要性——即**从静态的多智能体架构向能够根据任务反馈重新配置治理拓扑的"自演进多智能体系统"转变。**实验跨越三个商业 LLM（MiniMax M2.5、Kimi K2.5、Gemini 2.5 Flash）和两个基准（单轮工具使用 PinchBench、多步推理 ClaweBench）： **治理拓扑是一阶性能决定因素**：在同一模型内，最佳与最差治理架构的性能差距高达 57–61 个百分点（如唐代在 MiniMax 上达 88.2%，而单智能体基线仅 31.1%）。 **不存在普遍最优机构**：排名随模型与任务系统性迁移。唐代在 MiniMax 上表现最佳，在 Gemini 上却跌至 27.2%；简单的单智能体基线在 Gemini 上超越多数多智能体架构。Kimi 在简单任务上最弱，却在复杂多步任务上最强。 结构复杂度的成本收益权衡： 门控密度（ρ ）：定义为门控阶段数与总阶段数之比，是治理开销的有效预测指标。低 ρ （如唐代 ρ=0.17 ）在模型能力强时可实现高效纠错，但在模型能力弱时引发门控循环失败（gate-loop failure）——拒绝-修订循环导致成本爆炸而不提升质量。 任务复杂度效应：在复杂多步任务（ClaweBench）上，治理拓扑间的性能差异压缩至约 10 个百分点，表明任务本身难度主导了结构效应，复杂拓扑的协调开销产生递减回报。
## Uno-Orchestra: Parsimonious Agent Routing via Selective Delegation【Topo】

[https://arxiv.org/pdf/2605.05007](https://arxiv.org/pdf/2605.05007)

- 论文提出UNO-ORCHESTRA，一个统一的选择性委托（Selective Delegation）策略，将上述问题形式化为一个端到端的因果语言模型策略： 联合决策：在一个前向传播中同时生成任务分解（子任务DAG）和每个子任务的（模型，原语）路由决策 成本感知：在显式成本预算约束下，学习优化准确率-效率权衡 两阶段训练：先通过验证器门控的课程学习进行监督微调（SFT），再通过Agentic-GRPO强化学习优化多轮交互中的信用分配。
## TeamBench: Evaluating Agent Coordination under Enforced Role Separation【Topo, Google】

[https://arxiv.org/pdf/2605.07073](https://arxiv.org/pdf/2605.07073)

- **TEAMBENCH**，一个用于评估强制角色分离下智能体协调能力的基准测试。现有基于大语言模型（LLM）的智能体系统常将任务分解为多个角色（如规划者、执行者、验证者），但这些角色通常仅通过提示（prompts）指定，而非通过访问控制强制执行。论文构建了包含 **851个任务模板 和 931个种子实例 的评估体系**，通过操作系统级权限强制实施三角色分离： **Planner（规划者）：可读完整规范（**[**spec.md**](http://spec.md/)**），不可修改工作空间或执行命令** **Executor（执行者）：可编辑工作空间并执行命令，仅见摘要简报（**[**brief.md**](http://brief.md/)**），不可读完整规范** **Verifier（验证者）：可读完整规范和工作空间（只读），可写最终认证文件（attestation.json），不可修改工作空间或执行命令** 关键约束：没有任何角色能同时拥有 {读规范,修改工作空间,写认证} 三种权限，迫使信息必须在角色间流动。**验证者作为瓶颈** 高假接受率：Verifier批准了 49.4% 本应被确定性评分器拒绝的提交（95% CI [45.9,52.9] ） 负向价值：在参考消融中，移除Verifier使平均部分得分提高 5.5分 错误模式：包括"乐观认证"（未检查即批准）、"回声认证"（重复执行者声明）和"角色越界"（Verifier直接编辑代码）;团队收益并非单调递增，而是取决于单智能体基线能力。
## Beyond Individual Intelligence: Surveying Collaboration, Failure Attribution, and Self-Evolution in LLM-based Multi-Agent Systems【Topo】

[https://arxiv.org/pdf/2605.14892](https://arxiv.org/pdf/2605.14892)

- 解决基于LLM的多智能体系统（Multi-Agent Systems, MAS）在**闭环自我改进**方面的关键缺陷，以及现有研究对系统生命周期各阶段**孤立处理**所导致的理论碎片化问题。论文提出了**LIFE进展框架**（Lay the capability foundation, Integrate agents through collaboration, Find faults through attribution, Evolve through autonomous self-improvement），将四个阶段重构为因果关联的统一操作生命周期。该框架的核心在于建立**归因驱动的进化闭环**：协作结构决定了可观测的故障模式，故障归因通过定位责任方缩小改进的搜索空间，而进化增益反过来重塑协作结构，从而推动系统向自主、自适应的集体智能演进。
## Orchard: An Open-Source Agentic Modeling Framework【Topo, Microsoft】

[https://arxiv.org/pdf/2605.15040](https://arxiv.org/pdf/2605.15040)

- **Agentic建模（Agentic Modeling）研究中的基础设施瓶颈与可重用性危机。**论文提出**Orchard**框架，其**核心Orchard Env是一个Kubernetes原生的薄型环境服务，通过REST API暴露沙盒生命周期管理、命令执行、文件I/O等通用原语**，实现与智能体工具、训练器和任务领域的解耦，从而支撑可扩展、可复现且成本可控的开放Agentic建模研究。
## MetaAgent-X : Breaking the Ceiling of Automatic Multi-Agent Systems via End-to-End Reinforcement Learning【Topo, Amazon】

[https://arxiv.org/pdf/2605.14212](https://arxiv.org/pdf/2605.14212)

- 解决**自动多智能体系统（Automatic Multi-Agent Systems）中的部分自适应局限性**问题。论文提出MetaAgent-X框架，旨在通过端到端强化学习实现： 联合优化：**同时训练设计器（负责生成任务特定的MAS结构）和执行器（负责运行实例化的多智能体系统）** 打破天花板：突破"仅优化设计器而冻结执行器"的半可训练范式限制 **显式协同进化：通过分层回放（Hierarchical Rollout）和阶段式协同进化（Stagewise Co-evolution）机制，建立可分析的设计器-执行器相互改进路径** 简言之，该工作致力于将自动多智能体系统从"测试时搜索"或"半可训练"的局部自适应范式，推进到端到端可训练的、自设计且自执行的完整自适应范式。
## TeamTR: Trust-Region Fine-Tuning for Multi-Agent LLM Coordination【Topo】

[https://arxiv.org/pdf/2605.15207](https://arxiv.org/pdf/2605.15207)

- 针对**共享上下文的多智能体大语言模型（Multi-Agent LLM）团队在顺序微调过程中出现的结构性失效模式**，具体而言是**复合占用偏移（compounding occupancy shift）**问题。关键机制： 中间占用重采样：每完成一个智能体更新后，立即基于部分更新的团队策略重新采样轨迹，确保下一智能体训练分布与实际执行分布一致，消除陈旧占用惩罚。 Token级信任区域（Eq. 2）：利用轮询协议下的因子分解（Lemma 3.1），将团队级KL约束转化为单智能体token级KL和：D_{\text{KL}}^{\hat{\pi}^{i-1}}_{\text{tok}}(\pi_{\sigma(i)}^{\text{cur}} \| \pi_{\sigma(i)}^{\text{tar}}) \leq \delta_i 通过监测采样轨迹上的log概率差实现（Eq. 11），无需从更新后策略额外采样。 Stage-0 对齐：支持即插即用组件替换，通过反向KL投影（蒸馏）将新组件对齐至被替换组件的信任区域内，随后恢复标准更新（Proposition 3.8）。
## AgentFugue: Agent Scaling for Long-Horizon Tasks through Collective Reasoning【Topo】

[https://arxiv.org/pdf/2605.24486](https://arxiv.org/pdf/2605.24486)

- 现有长程智能体研究主要聚焦于纵向扩展（scaling up）：通过更强的基础模型、更优的工具使用和更复杂的单智能体脚手架来提升能力。论文指出，**横向扩展（scaling out）——即多个同级智能体（peer agents）并行探索同一任务并相互利用中间发现**——是尚未被充分挖掘的能力来源。核心挑战在于： **无通信时，并行智能体沦为孤立搜索**，无法复用他人的部分证据或排除的错误分支； **无限制通信时，信号会被原始轨迹噪声淹没，且探索多样性迅速崩溃**。AgentFugue 通过一个外挂式共享推理中心（shared reasoning hub H ）解决上述问题，其核心机制包括： **片段化记录（Episode Writing） 当智能体 i 的上下文达到预算（如 64K tokens），其交互历史被封闭为片段 ϵi,e ，并由写入模型压缩为片段笔记**： zi,e=Mwrite(ϵi,e) 笔记记录已确立的事实、收集的证据、尝试的方法及排除的分支，随后原始内容从智能体本地上下文驱逐，以释放容量。 **意图驱动的选择性读取（Intent-Driven Reading） 智能体基于当前搜索意图 qi,t ，主动请求检索特定队友的原始片段 Ei,t **，中心合成聚焦的读取输出： ri,t=Mread(qi,t,{ϵj,e:ϵj,e∈Ei,t}) 这种"笔记提供粗粒度感知、读取提供选择性深度"的两级设计，避免了全广播的噪声问题。 中心优化 中心基于 Qwen3.5-9B，先通过监督微调（SFT）学习生成参考笔记和读取输出，再通过端到端强化学习（GRPO）对齐下游任务成功，优化目标包含任务奖励与路径简洁性奖励。
## When Does Multi-Agent RL Improve LLM Workflows? Workflow, Scale, and Policy-Sharing Tradeoffs【Topo】

[https://arxiv.org/pdf/2605.24202](https://arxiv.org/pdf/2605.24202)

- 核心问题是：**在多智能体大语言模型（LLM）工作流中，端到端强化学习（RL）训练何时能够提升性能，以及不同策略配置下的训练稳定性机制**。（1）性能提升的条件依赖性 **多智能体RL通常优于基础模型，但收益取决于工作流、任务与规模的联合作用，而非单一因素**。 在36个实验单元中，IP在28个单元达到更高峰值准确率，但23%的IP实验出现终端准确率悬崖（late-training degradation）；SP峰值较低但更稳定，却会在后期以不同模式漂移。 （2）策略共享的权衡结构 IP（隔离策略）：具有更高天花板、更低地板特征。同角色并行（如3个投票器）产生相干梯度放大，导致特定角色快速漂移并最终崩溃。 SP（共享策略）：训练侧幅度（梯度范数、策略比率χ2 ）较小，但会将失败重新分配为"角色捕获"（role capture）——主导角色（输出更长或出现更频繁）迫使共享策略偏向其分布，导致其他角色输出异常（如评估器开始生成代码块）。 （3）隐藏失效模式 SP的某些失效在token级指标（熵、χ2 ）上不可见，仅通过轨迹级分析（如聚合器输出长度分布）才能检测。例如，Voting-SP中聚合器从简洁选择标记（≤ 30 tokens）迁移为冗长论证（173 tokens），而投票器指标保持稳定。
## UnityMAS-O: A General RL Optimization Framework for LLM-Based Multi-Agent Systems【Topo, Xiaohongshu】

[https://arxiv.org/pdf/2605.26646](https://arxiv.org/pdf/2605.26646)

- 解决**LLM-based多智能体系统缺乏统一强化学习优化接口**的问题。论文提出 UnityMAS-O，其**关键创新在于将完整的多智能体工作流（而非单个响应或单条策略轨迹）作为优化单元**，通过四个一级抽象对象实现通用性： 逻辑智能体角色（Logical agent roles） 图结构轨迹（Graph-structured trajectories） 用户定义的奖励函数（支持角色级、轮次级、轨迹级奖励） 显式的智能体-模型映射（支持全共享、全分离、部分共享） 该框架旨在成为可复用的优化底层（optimization substrate），**允许用户在不修改底层训练基础设施的情况下，定义不同的工作流、智能体组合和奖励机制**，从而将手动设计的多智能体系统转化为可训练的多智能体RL策略。
## Are Tools All We Need? Unveiling the Tool-Use Tax in LLM Agents

[https://arxiv.org/pdf/2605.00136](https://arxiv.org/pdf/2605.00136)

- **在语义干扰（semantic distractors）环境下，工具增强的大型语言模型（LLM）代理为何以及何时表现不及原生思维链（CoT）推理？**传统假设认为工具使用应提升推理可靠性与准确性，但研究发现，当输入包含语义相关但逻辑无关的干扰信息时，工具增强协议反而可能导致显著的性能下降。论文通过构建GSM8K-Sem-Distractor和HotPotQA-Sem-Distractor基准测试，系统验证了这一差距的存在。论文提出**G-STEP**——一种推理时门控机制，**通过判断何时应继续工具交互而非过早终止，部分恢复协议引起的错误**。然而，实验表明，当失败源于模型内在能力缺口（genuine capability gap）而非协议诱导时，此类干预效果有限。
## To Call or Not to Call: A Framework to Assess and Optimize LLM Tool Calling

[https://arxiv.org/pdf/2605.00737](https://arxiv.org/pdf/2605.00737)

- 当前Agentic AI架构中，LLM需要自主决定是否调用外部工具（如网络搜索）来完成任务。然而，现有评估多局限于粗粒度的总体准确率，缺乏对单个调用决策质量的细粒度分析。这导致难以判断某次调用是必要、有益、冗余还是有害，也无法在成本约束下进行最优资源配置。 核心问题在于**LLM的自我感知（是否需要帮助、工具是否有用）与真实情况（实际是否需要、实际是否有用）之间存在系统性错位**，导致次优决策。
## 🧐TOBench: A Task-Oriented Omni-Modal Benchmark for Real-World Tool-Using Agents

[https://arxiv.org/pdf/2605.16909](https://arxiv.org/pdf/2605.16909)

- 论文提出 TOBench，一个面向任务的全模态（Omni-Modal）工具使用基准测试，其核心设计为闭环多模态验证（closed-loop multimodal verification）。该基准要求智能体在真实可执行环境中完成感知–行动–检查–修正的完整循环，通过任务特定的基于证据的验证器（grounded verifiers）来评估最终产物是否符合格式、多模态语义和工具调用约束。**智能体需感知异构输入（文本/图像/音频/视频）、调用外部工具、检查渲染后的中间产物（如文档、PPT、生成图像），并在不满足约束时自我修正**。此外，现有评估多依赖最终答案匹配，缺乏对**迭代验证与修正循环**的考察。
## AgentFloor: How Far Up the tool use Ladder Can Small Open-Weight Models Go?

[https://arxiv.org/pdf/2605.00334](https://arxiv.org/pdf/2605.00334)

- 论文提出了AgentFloor——一个确定性的六级能力阶梯基准（A0–E），涵盖**从无工具指令遵循到长程约束规划的30项任务**，通过对比16个开源模型（0.27B–32B）与GPT-5的表现，绘制出一条"模型必要性边界"（boundary of model necessity），为代理系统的成本效益路由提供实证依据。
## Workspace-Bench 1.0: Benchmarking AI Agents on Workspace Tasks with Large-Scale File Dependencies【ByteDance】

[https://arxiv.org/pdf/2605.03596](https://arxiv.org/pdf/2605.03596)

- **现有AI代理基准测试无法有效评估真实工作空间中复杂文件依赖关系处理能力**的问题。论文引入了Workspace-Bench，这是一个专门评估Workspace Learning能力的基准测试，其核心特征包括： 五个真实职业角色工作空间：包含运营经理、后勤经理、AI产品经理、后端开发者和研究者，共计20,476个文件（最高20GB） 388个依赖驱动任务：每个任务附带文件依赖图（File Dependency Graph），涵盖从基础文件组织到跨职能报告生成的多难度层级 六维评估框架：系统评估工作空间探索、任务支持文件利用、结果提供文件聚合、语义内容关系理解、异构文件理解和谱系追踪能力。
## FutureWorld: A Live Environment for Training Predictive Agents with Real-World Outcome Rewards【Zhongguancun Academy】

[https://arxiv.org/pdf/2604.26733](https://arxiv.org/pdf/2604.26733)

- 论文提出FutureWorld——首个开源的实时智能体RL环境，其核心机制包括： 每日问题生成：从72个跨领域真实数据源自动构建预测问题（平均每日2047.29 个候选问题，经筛选平衡后保留500个） 完整智能体展开（rollout）：智能体执行**搜索查询、阅读文档、推理并生成概率预测**，环境记录完整轨迹 延迟结果奖励：当事件实际发生后，检索真实结果并计算基于结果的奖励（如负Brier分数：rq,k=−(π^q,k−zq)2 ） 闭环参数更新：使用GRPO（Group Relative Policy Optimization）利用累积的延迟奖励更新模型，实现"预测→观察结果→适应改进"的连续循环。
## AI Co-Mathematician: Accelerating Mathematicians with Agentic AI【Math, Google DeepMind】

[https://arxiv.org/pdf/2605.06651](https://arxiv.org/pdf/2605.06651)

- 论文提出**AI Co-Mathematician**——一个交互式工作台，旨在**通过有状态的代理架构、并行工作流、程序化约束和原生数学工件输出**，为数学家提供涵盖**构思、文献搜索、计算探索、定理证明和理论构建**的全方位支持，从而加速开放式数学发现。
## Achieving Gold-Medal-Level Olympiad Reasoning via Simple and Unified Scaling【Math, AI Lab】

[https://arxiv.org/pdf/2605.13301](https://arxiv.org/pdf/2605.13301)

- **如何通过一个简单、统一且可扩展的后训练配方（recipe），将紧凑型语言模型（如30B-A3B）培养成具备金牌级别奥林匹克竞赛推理能力的求解器，同时保持对数学和物理之外广泛科学领域的泛化能力。论文提出四阶段模块化流程，将通用后训练模型转化为专业级奥林匹克求解器： 阶段一：严格推理行为植入（SFT） 使用338K条多源（数学、科学、代码）长思维链（Long-CoT）轨迹进行监督微调。 引入反向困惑度课程（Reverse-Perplexity Curriculum）：****按困惑度降序排列训练数据，先学习模型最陌生的严格证明模式，再巩固熟悉样本****，有效避免能力遗忘；阶段二：推理能力粗粒度扩展（Coarse RL） ****在可验证问题上使用**Group Sequence Policy Optimization (GSPO)**进行强化学习，通过序列级重要性采样和结果奖励（正确/错误）快速扩展基础搜索能力****。 阶段三：****证明质量精细化提升（Refined RL） 生成式证明奖励：使用DeepSeekMath-V2评估完整证明的数学有效性****，而非仅检查答案。 自我精炼（Self-Refinement）：****将失败轨迹转换为"问题-错误解-修复指令"的训练对，培养模型识别和修复漏洞的能力****。 经验回放（Experience Replay）：****仅保留稀有成功轨迹（0<n+(q)<2 ），优先选择低熵轨迹进行回放，防止宝贵学习信号丢失****；阶段四：测试时计算规模化（TTS） 实施验证-精炼循环：****模型迭代生成候选解、输出结构化错误报告、修复漏洞，直至通过验证或耗尽预算****。 支持超过100K token的连贯推理轨迹，通过多轮"求解-验证-修复"有效分配额外推理计算。**
## 🧐Shepherd: A Runtime Substrate Empowering Meta-Agents with a Formalized Execution Trace【Git】

[https://arxiv.org/pdf/2605.10913](https://arxiv.org/pdf/2605.10913)

- **现有LLM代理运行时无法满足元代理（meta-agents）对执行过程进行高级操作的需求。**论文提出SHEPHERD——一个基于函数式编程的代理运行时底层架构（substrate），其核心创新包括： **任务（Tasks）：将代理声明为具有类型输入/输出的函数，支持高阶组合（元代理作为作用于其他任务的函数） **效果（Effects）：将每个代理动作记录为类型化事件流，实现非干扰性观察 作用域（Scopes）：**提供fork、discard、merge等原语，实现代理环境和进程的原子级分支与回退** 执行轨迹（Execution Trace）：**采用Git-like的持久化提交图，支持对任意历史状态的廉价分叉和确定性重放 通过这一设计，SHEPHERD使元代理能够以函数式编程的方式持有、检查、分支和重写其他代理的执行过程**，从而在三个应用场景中实现显著性能提升：实时监督（CooperBench通过率从28.8%提升至54.7%）、反事实元优化（CRO，速度提升达58%）和树形强化学习（Tree-GRPO，TerminalBench 2.0上提升5.2%）。
## Rollout Cards: A Reproducibility Standard for Agent Research【Git】

[https://arxiv.org/pdf/2605.12131](https://arxiv.org/pdf/2605.12131)

- 针对**智能体（agent）研究中的可复现性危机**，系统诊断了当前评估实践的两个核心失败模式，提出了**Rollout Cards（推出卡）**作为解决方案，并通过实验验证了其有效性。论文提出将rollout记录（而非报告分数）作为智能体研究的可复现性基本单元，并定义Rollout Card为最小充分的发布 bundle，包含： Rollout Record（推出记录）：自描述的档案，记录任务规范、环境动态、智能体动作、工件、计时、终端状态及失败事件，支持在无需原始运行环境的情况下重放和重新分析。 Reporting-Rule Registry（报告规则注册表）：明确声明将记录转化为分数所使用的视图（view，即投影到特定分析所需的字段）和报告规则（评分程序、配置、版本）。 Drops Manifest（丢弃清单）：详细记录报告规则使用了哪些字段、应用了哪些过滤器、进行了哪些结构折叠，以及声明了哪些语义损失（如"时间信息被擦除"），使后续读者明确了解报告视图未包含的信息。 Release-Scope Metadata（发布范围元数据）：声明编辑、访问限制、许可或重新分发限制，支持负责任的部分发布。 论文发布了参考实现 ERGON（开源强化学习 gym）以及21个公开的 rollout-card 导出（17个完整轨迹导出 + 4个分析视图导出）。
## 📖BenchCAD: A Comprehensive, Industry-Standard Benchmark for Programmatic CAD【CAD】

[https://arxiv.org/pdf/2605.10865](https://arxiv.org/pdf/2605.10865)

- **工业CAD代码生成评估的粒度不足与能力分解缺失。** **BenchCAD**，一个针对工业计算机辅助设计（CAD）推理的统一能力分解基准测试，用于评估多模态大语言模型（MLLMs）生成可执行参数化CAD程序的能力。BenchCAD解决方案 论文构建了首个工业级CAD评估框架，包含三个核心组件： 数据集：**17,900个经过沙箱执行验证的CadQuery程序，涵盖106个工业零件族（如锥齿轮、压缩弹簧、麻花钻），其中49%严格锚定至ISO/DIN/EN/ASME/IEC工程标准，确保参数关系符合真实工程约束**。 四级能力分解：将CAD推理形式化为**L1（整体视觉识别）→ L2（CAD操作理解）→ L3（工业参数抽象）→ L4（空间/代码合成）**的层次结构，通过匹配的视觉/代码问答对（VISION QA vs CODE QA）解耦评估各层能力。 四大评估任务： VISION2CODE：从四视图生成可执行代码，采用旋转不变IoU和关键操作召回率（essential-op recall）诊断空间锚定错误与操作盲区。 CODE EDIT：748个验证编辑对（T1-T5难度），引入归一化准确率指标和非目标保持（non-target preservation）要求，强制模型仅修改指令指定特征。 VISION QA / CODE QA：2,400对匹配问答，量化视觉识别与参数抽象的能力差距（约15-20分"整体空间与细节缺陷"）。
## 📖CADBench: A Multimodal Benchmark for AI-Assisted CAD Program Generation【CAD, MIT】

[https://arxiv.org/pdf/2605.10873](https://arxiv.org/pdf/2605.10873)

- 论文提出了CADBench——一个统一的多模态基准测试，通过18,000个样本、六种基准家族、**五种输入模态（干净/噪声网格、单视图/多视图/真实感渲染）**和六项指标（体积IoU IoU 、表面IoU SIoU 、Chamfer距离 CD 、有效形状率 VSR 、令牌数、操作数），构建了一个用于诊断性评估模型在可编辑三维重建与多模态CAD理解方面进展的测试平台。
## Personal Visual Context Learning in Large Multimodal Models【Personalize】

[https://arxiv.org/pdf/2605.10936](https://arxiv.org/pdf/2605.10936)

- 论文提出Agentic Context Bank框架，通过以下机制改进上下文利用： 结构化记忆构建：**将离散的上下文项目转换为自完善的、证据链接的记忆库**，而非简单的提示拼接 查询自适应证据选择：**根据查询需求动态选择必要的视觉证据，而非加载全部原始视觉数据** 该研究建立了Personal VCL作为构建真正个性化AI助手的关键能力基准，为未来可穿戴计算中的个性化LMMs发展提供了系统性的评估框架和强基线方法。
## Omni-Persona: Systematic Benchmarking and Improving Omnimodal Personalization【Personalize】

[https://arxiv.org/pdf/2605.09996](https://arxiv.org/pdf/2605.09996)

- 论文提出了 Omni-Persona，这是首个针对全模态个性化的综合评估基准，通过以下方式解决问题： **基于**人物模态图（Persona Modality Graph, PMG）形式化跨模态路由任务，涵盖4个任务组和18个细粒度任务**** 引入**校准准确率（Calibrated Accuracy, Cal）**作为主要指标，统一评估正确 grounding 和适当弃权 系统性包含缺席人物样本（约占评估集的50%），结合硬干扰项（hard distractors）和检索噪声，测试模型的抗幻觉能力 对监督微调（SFT）和可验证奖励强化学习（RLVR）进行诊断性比较，揭示音频-视觉 grounding 差距、奖励设计导致的过度保守行为等关键发现。
## PersonaArena: Dynamic Simulation for Evaluating and Enhancing Persona-Level Role-Playing in Large Language Models【Personalize, Microsoft】

[https://arxiv.org/pdf/2605.17044](https://arxiv.org/pdf/2605.17044)

- **PersonaArena**，一个用于评估与增强大语言模型（LLMs）人格级（persona-level）角色扮演能力的动态模拟框架。当前LLMs角色扮演研究主要聚焦于**角色级（character-level）**设置（如扮演知名小说或电影人物），存在三方面局限：数据保真度不足：众包生成的人格对话缺乏真实感；评估指标片面：依赖静态指标（如BLEU）或单一维度，忽略人格一致性、适应性等；交互形式静态：通过问答对评估，而非动态多轮社会互动。相比之下，**人格级角色扮演（模拟具有特定职业、价值观、经历的普通人）对社会科学应用更为关键**，但缺乏可靠的动态评估框架。
## ⭐⭐VitaBench 2.0: Evaluating Personalized and Proactive Agents in Long-Term User Interactions【Personalize, LongCat】

[https://arxiv.org/pdf/2605.27141](https://arxiv.org/pdf/2605.27141)

- 解决**现有Agent基准测试在评估个性化和主动性方面的严重不足。**论文提出了**VitaBench 2.0**，通过构建包含**56个用户、2000+细粒度偏好、跨多个真实生活领域（外卖、到店消费、在线旅游）的长期交互任务序列**，为评估个性化和主动性Agent行为提供了系统性的测试平台。主要实验发现 在 22个 前沿模型（包括GPT-5、Claude-Opus-4.6、DeepSeek-R1、o3等）上的评估表明： 1. **个性化极具挑战性 即使最强的Claude-Opus-4.6在完整上下文设置下，Avg@4仅 0.503，Passˆ4（四次全部成功）仅 0.337**。相比之下，模型在代码、数学等推理密集型任务上的提升在此并不明显，表明个性化已成为新的能力瓶颈。 2. 记忆机制效果不佳 与Full Context（直接提供完整历史）相比，大多数模型在使用Agentic Memory或RAG Memory时性能下降。这表明当前Agent尚无法有效利用外部记忆进行长期用户建模，记忆架构设计仍是关键挑战。 3. 推理能力≠个性化能力 启用"思考"模式（如DeepSeek-R1 vs V4-Pro）并未带来一致的个性化性能提升。个性化需要超越通用推理的特定能力：偏好提取、长期一致性维护、噪声处理。 4. 主动性能力薄弱 在需要主动询问的任务上，所有模型家族表现显著下降（如Claude从46.0降至27.4）。Agent往往无法识别知识缺口，倾向于基于不完整信息盲目决策。 5. 偏好利用困难 即使提供真实用户偏好（消除提取难度），多数模型Avg@4仍低于0.5，表明挑战不仅在于获取偏好，更在于推理、优先排序和一致性应用偏好信息。 6. 性能随时间衰减 随着任务序列推进，Agent性能显著下降，反映长上下文处理困难（Full Context）和记忆错误累积（Memory设置）的双重瓶颈。
## ⭐⭐Personal Visual Memory from Explicit and Implicit Evidence【Personalize, Adobe】

[https://arxiv.org/pdf/2605.28806](https://arxiv.org/pdf/2605.28806)

- 解决**个性化AI代理长期记忆系统中视觉信息处理不足**的问题。论文通过以下方式应对上述挑战： 构建视觉记忆基准：创建包含显式实体（TargetPerson /TargetAsset ）和隐式事实（Visual-Only /Multimodal ）的评估体系，要求模型必须依赖早期交互的视觉证据而非仅文本来回答问题。 提出VISUALMEM架构：设计混合视觉-文本记忆框架，通过上下文引导的图像解析、延迟确认机制和结构化视觉记忆提取，显式存储用户相关的视觉实体、所有权关系和持久性视觉事实。 实验表明，该方法在视觉记忆基准上显著优于基于标题的记忆系统（整体准确率84.1% vs 56.0% ），同时在标准文本记忆基准上保持竞争力，验证了视觉记忆作为个性化AI独立且重要组件的必要性。提出混合视觉-文本记忆框架，**核心创新在于拒绝将图像降维为标题**，转而采用三阶段处理： 阶段一：上下文引导解释 联合分析图像与周围对话上下文，利用语言信号消除视觉歧义（如区分"我的厨房"vs"他人的空间"），生成情境化解释。 阶段二：延迟确认（Deferred Commitment） 当证据不足时，将图像置于待处理状态（Pending State），随对话演进结合累积视觉证据重新评估，避免早期错误记忆固化。 阶段三：结构化提取 对确认图像提取三层信息： 关系层：社交链接与所有权关联 实体层：重复人物、物体、宠物的视觉引用（面部/物体特征） 事实层：持久的视觉基础用户事实（转化为文本后送入文本记忆后端整合）
## SWE Atlas: Benchmarking Coding Agents Beyond Issue Resolution【Coding】

[https://arxiv.org/pdf/2605.08366](https://arxiv.org/pdf/2605.08366)

- 论文提出了**SWE Atlas**基准套件，通过引入基于专家设计的结构化评分标准（Rubric-based Evaluation），结合程序化检查（如变异测试、行为保持测试），**从功能正确性和工程质量两个维度**全面评估编码智能体的专业能力。
## FrontierSmith: Synthesizing Open-Ended Coding Problems at Scale【Coding】

[https://arxiv.org/pdf/2605.14445](https://arxiv.org/pdf/2605.14445)

- 解决**开放性编程（open-ended coding）任务训练数据稀缺且难以大规模构建**的问题。论文提出FrontierSmith系统，通过迭代演化管道将封闭性编程问题（如竞赛题）转化为开放性问题： 变异策略：通过改变目标（O→O′ ）、限制输出（CO→C′O ）、泛化输入（CI→C′I ）消除已知最优解； 质量筛选：引入**想法发散度（idea divergence）**指标，量化不同求解器采用不同核心策略的概率，筛选真正能激发算法多样性的问题； 环境构建：通过智能体自动生成测试用例和连续评分验证器，并通过交叉验证确保质量。 最终目标是无需昂贵人工策划即可规模化合成开放性编程问题，使LLM能够通过强化学习掌握长程、开放式的代码生成与优化能力。
## RoadmapBench: Evaluating Long-Horizon Agentic Software Development Across Version Upgrades【Coding】

[https://arxiv.org/pdf/2605.15846](https://arxiv.org/pdf/2605.15846)

- 解决**长程多目标软件开发能力的评估缺失**问题。现有编码基准（如SWE-bench）主要聚焦**单一bug修复**（~33-107行代码，二元通过/失败），无法捕捉真实软件版本中**跨数月、协调多文件、实现复杂功能集**的工程挑战。随着编码代理从简单代码生成迈向真实软件开发，亟需能评估"持续多目标工程能力"的基准。性能鸿沟显著：最强模型Claude-Opus-4.7仅解决39.1%任务，Seed-2.0-Pro仅5.2%，远低于这些模型在SWE-bench上80%+的表现，表明长程开发仍是基本未解决的难题 能力瓶颈转移：弱模型主要失败于构建错误（41%）和缺失实现（31%）——即无法产生可编译代码；而强模型（如Claude）58%失败源于实现精度（逻辑缺陷、组件集成错误）——即代码可运行但行为不正确 领域差异：ML & Data领域最具挑战性（多模型0%解决率），UI & Rendering呈现最大能力分化（最强64.3% vs 最弱<21%） 效率差异：高效模型（Claude-Opus-4.7）用更少工具调用（101次）和更低探索占比（35%）实现更高解决率，而低效模型陷入过度探索或早期停滞
## EnvFactory: Scaling Tool-Use Agents via Executable Environments Synthesis and Robust RL【Coding】

[https://arxiv.org/pdf/2605.18703](https://arxiv.org/pdf/2605.18703)

- 论文提出了完全自动化的EnvFactory框架： 环境生成（EnvGen）：**自主探索真实在线资源，构建有状态（stateful）、可验证的可执行环境**，通过迭代验证确保鲁棒性 拓扑感知采样（Topology-aware Sampling）：基于工具依赖图递归解析逻辑依赖，支持非线性工具使用模式 校准细化（Calibrated Refinement）：将过度指定的查询转换为具有隐含意图、歧义和现实沟通模式的自然人类请求 实验表明，仅使用85个验证环境（远少于基线方法5倍以上的环境数量），EnvFactory即可生成2,575条高质量SFT和RL轨迹，在BFCLv3、MCP-Atlas等基准上实现显著提升（Qwen3系列模型提升高达+15%）。
## SpecBench: Measuring Reward Hacking in Long-Horizon Coding Agents【Coding】

[https://arxiv.org/pdf/2605.21384](https://arxiv.org/pdf/2605.21384)

- 解决**长程自主编程智能体（long-horizon coding agents）中的奖励黑客（reward hacking）量化与测量问题**。
## Terminal-World: Scaling Terminal-Agent Environments via Agent Skills【Coding】

[https://arxiv.org/pdf/2605.20876](https://arxiv.org/pdf/2605.20876)

- 解决**终端智能体（Terminal Agents）训练数据稀缺且质量受限**的核心问题。论文提出Terminal-World框架，其**核心创新在于将**智能体技能（Agent Skills）**作为数据合成的中心原语**。每个技能联合编码了： What：应完成的目标任务 When：适用条件（前置条件、输入需求、环境状态） How：执行流程与验证标准 通过解码这三个维度，Terminal-World实现了任务指令、可执行环境与教师轨迹的自顶向下联合派生（co-derived），并通过技能团队（skill teams）和技能图（skill graphs）扩展合成空间，从而以低成本（$0.17/轨迹）构建出多样化、高质量且对齐的终端智能体训练数据。**从开源生态（ClawHub、SkillMP）筛选 1,000 个高质量单技能**，并构建技能团队（Skill Teams，同领域多角色组合）与技能图（Skill Graphs，跨领域流程组合），将合成空间扩展至 5,723 个多样化任务场景。将技能与用户角色（User Personas）配对以多样化应用场景，通过 LLM 合成四元组 (I,E,V,G) ：任务指令 I 、环境蓝图 E （初始文件与设置步骤）、评估标准 V 、以及执行指导 G 。采用 LLM-as-a-Judge 五维过滤（指令质量、封闭世界可解性、蓝图完整性、指南质量、评估标准质量）确保质量。
## ProcBench: Evaluating Process-Level Defects and Control Preservation in LLM Coding Agents【Coding】

[https://arxiv.org/pdf/2605.20251](https://arxiv.org/pdf/2605.20251)

- 解决**LLM编码代理（LLM coding agents）****评估中过度依赖最终结果指标而忽视过程质量**的问题。论文提出了ProcBench框架，其核心贡献包括：建立过程缺陷本体论：将执行失败组织为四个维度（上下文管理、工具使用效率、工作流架构、工具生态系统一致性）共11个可复用的缺陷类别，超越单纯的结果成败二元判断。标准化轨迹评估：通过统一的事件表示将异构执行日志转换为可比较的标准化轨迹，支持跨系统的结构化过程比较。校准风险评分：将原始检测器输出映射为经过校准的后验风险估计，生成多维记分卡（包括缺陷级风险、维度级质量分数、控制保留分数），提供更稳定的语义解释。控制保留评估：将可治理性（governability）形式化为与过程缺陷并行的评估轴，考察执行过程的可解释性、可中断性、可纠正性、可逆性和权限移交能力。简言之，该论文试图建立一个面向过程的基准测试范式，补充传统的结果导向评估，使编码代理的可靠性、执行效率和可控性成为可测量、可比较的维度。
## TerminalWorld: Benchmarking Agents on Real-World Terminal Tasks【Coding】

[https://arxiv.org/pdf/2605.22535](https://arxiv.org/pdf/2605.22535)

- 解决**终端代理（Terminal Agents）评估中的真实性与可扩展性困境**。"终端代理在与日常实践共同演变的真实任务上表现如何？"——论文提出 TERMINALWORLD，一个可扩展的数据引擎，通过以下范式转变解决上述问题： 真实性由构造保证：利用 asciinema 平台上开发者自愿分享的真实终端会话录音，**将"野外"（in-the-wild）的自然操作逆向工程为可执行、严格验证的评估任务** 可扩展性：全自动化的流水线能够随着 asciinema 平台新录音的持续积累而重新运行，确保基准测试与开发者实践同步演进 高保真验证：通过环境重建与基于试验的测试套件校准，确保任务可在隔离的 Docker 环境中可靠执行与验证 该引擎**从 80,870 个原始录音中提炼出 1,530 个经过验证的任务（其中 200 个为人工交叉审核的 VERIFIED 子集），覆盖 18 个真实类别与 1,280 个独特命令**，为评估终端代理在复杂、真实终端工作流中的能力提供了严谨的测试平台。**真实人类终端操作记录 → 反推出任务说明/环境/测试 → 给 agent 一段文本任务 → agent 在终端里完成 → 测试验证**
## RepoMirage: Probing Repository Context Reasoning in Code Agents with Perturbations【Coding】

[https://arxiv.org/pdf/2605.26177](https://arxiv.org/pdf/2605.26177)

- **高基准分数是否真正反映了跨多文件识别任务相关信息并推理其间关系的“仓库上下文推理”能力****？**论文提出 REPOMIRAGE，一个**基于扰动的两阶段评估框架，在保持原始任务语义和代码功能不变的前提下，通过改变信息暴露方式来探测能力边界**： 阶段一：REPOMIRAGE-Perturb 对 SWE-Bench Verified 实例应用三种语义保留的仓库级扰动： 依赖路径间接化：将直接导入替换为四层代理链，隐藏跨文件依赖关系 运行时目标掩码：重命名目标文件并使用包装器目录，同时放置干扰文件，消除文件名线索 局部值外部化：将局部常量迁移至外部 JSON 文件，迫使跨文件值关联推理 阶段二：REPOMIRAGE-Extend 将结构瓶颈转化为四类显式任务：多文件 Issue 解决、代理链补全、运行时目标识别、缺失常量恢复，使仓库上下文推理成为可直接测量的目标。 关键发现 性能显著下降：在 REPOMIRAGE-Perturb 上，8 个前沿模型的平均解决率从 66.80% 降至 49.78%（相对下降 27.0%），同时访问文件数从 4.77 增至 13.24。 显式任务表现糟糕：在 REPOMIRAGE-Extend 上，平均成功率骤降至 25.3%（最低任务仅 17.19%），表明当前代理在追踪依赖链、识别运行时目标、恢复跨文件常量等基础推理任务上存在严重缺陷。 探索漂移（Exploration Drift）：轨迹分析揭示，在强上下文需求下，代理虽然扩大了文件搜索范围，但探索→编辑的转换概率降低，探索→探索的自循环增加，陷入"能访问信息但无法将其转化为有效结构理解"的困境。
## How to Interpret Agent Behavior

[https://arxiv.org/pdf/2605.13625](https://arxiv.org/pdf/2605.13625)

- **如何解释和规模化分析自主代理（Autonomous Agents）运行时行为**的核心问题。论文提出构建一个系统性的分类框架（Taxonomy），以实现： 描述标准化：为代理行为提供统一的概念词汇（Act·ONOMY） 分析可扩展：通过自动化工具将非结构化轨迹转化为可解释的行为画像 诊断可操作：识别代理的失败模式（如"未经验证即提交"）和效率瓶颈，支撑后续的错误修复与性能优化 简言之，该研究回应了引言中提出的根本问题："How do we interpret agent behavior?"（如何解释代理行为？），并提供了从理论分类到实践工具的完整解决方案。
## π -Bench: Evaluating Proactive Personal Assistant Agents in Long-Horizon Workflows【Long Horizon, AI Lab】

[https://arxiv.org/pdf/2605.14678](https://arxiv.org/pdf/2605.14678)

- **π-BENCH**，一个用于评估长期个人助理工作流程中**主动性（Proactivity）**的基准测试。现有个人助理基准测试主要关注反应式任务执行，假设用户在交互开始时提供完整、明确的目标规范。然而，真实场景中用户通常以**未充分说明（underspecified）**的请求启动交互，将关键偏好、约束和依赖关系隐式保留。当前基准缺乏对以下能力的系统评估：在多轮交互中识别逐渐浮现的隐藏意图利用跨会话记忆推断未明确陈述的需求在持久工作空间中主动管理欠规范任务。100个多轮任务，涵盖5个领域特定角色（研究员、营销人员、法律实习生、药剂师、金融分析师）。20会话/角色，构成长期工作流程，包含：强依赖组：6组跨会话依赖，后续任务依赖先前建立的信息（如文件格式、研究主题）独立任务：测试单工作流覆盖。对9个前沿模型（GPT-5.4、Claude 4.6 Opus、Qwen3.6 Plus等）的评估显示： 主动性仍然困难：平均PROC范围为43.1%-67.0%，显著低于COMP（52.1%-67.6%） 指标区分度：Kimi K2.5显示高COMP（61.6%）但低PROC（43.1%），表明其能执行明确任务但缺乏主动探索；Seed2.0 Pro则相反 跨会话价值：消融实验显示，移除先前会话使PROC平均下降9.5点，而COMP仅降2.5点，证实**历史交互对主动意图解决至关重要 **领域差异：药剂师任务（基于具体科学约束）PROC最高；法律事务操作（涉及缺失材料识别）PROC最低
## ⭐Test-Time Learning with an Evolving Library【Microsoft Research】

[https://arxiv.org/pdf/2605.14477](https://arxiv.org/pdf/2605.14477)

- 论文提出EVOLIB框架，旨在实现： 无参数的知识进化：通过维护一个结构化的进化知识库（evolving library），存储从模型自身推理轨迹中提取的模块化技能（modular skills）和反思性洞察（reflective insights）。 自我监督的持续改进：利用模型自评估的解决方案质量，通过**信息增益（Information Gain）和未来信息增益（Future Information Gain）**的信用分配机制，驱动简单、特定于实例的抽象逐步进化为更通用、可重用的知识单元，无需外部真值反馈。 跨任务迁移能力：使模型能够在数学推理、代码生成和多轮智能体任务等多样化场景中，通过知识的抽象、整合与复用实现持续性能提升，且对学习任务的顺序具有鲁棒性。
## FutureSim: Replaying World Events to Evaluate Adaptive Agents

[https://arxiv.org/pdf/2605.15188](https://arxiv.org/pdf/2605.15188)

- **如何现实地评估AI智能体在动态、开放环境中长期适应新信息的能力**。论文构建了一个基于真实世界事件重放的模拟环境，通过以下机制解决上述问题： 时间顺序重放：按照真实世界事件发生的时间顺序（2026年1月至3月），逐日提供新闻文章，模拟信息逐步显现的过程 预测性任务：要求智能体预测知识截止日期之后的世界事件，通过预测任务确保智能体行为不会改变底层环境（避免反事实模拟的复杂性） 开放端评估：智能体自主决定关注哪些问题、何时更新预测，并输出概率分布而非单一答案，评估其校准能力和适应性 该基准测试特别针对长期测试时适应（long-horizon test-time adaptation）、信息检索、记忆管理和不确定性推理等新兴研究方向，为衡量AI智能体在真实世界中长期适应能力提供了可复现、可控制的评估框架。论文评估了GPT 5.5、DeepSeek V4 Pro、Claude Opus 4.6、Qwen 3.6 Plus和GLM 5.1等前沿模型，关键发现包括： 能力差异显著：在推荐工具配置下，GPT 5.5以25%的准确率和0.05的Brier Skill Score显著领先，而多数开源模型表现差于弃权基线（负Brier Score）。 测试时适应瓶颈：通过固定所有模型初始预测为最差表现的消融实验，发现即使能力较强的模型也难以通过后续信息更新克服错误的初始锚点，最终未能达到弃权基线。直接获取完整上下文进行预测（31.2%准确率）显著优于顺序更新（24.8%），揭示长期适应效率存在明显瓶颈。 关键能力验证： 记忆：移除结构化记忆工具导致所有模型性能显著下降； 搜索：每日上下文更新（vs. 冻结语料库）和迭代多轮搜索（vs. 单次查询）对准确性和概率校准至关重要； 推理扩展：增加测试时计算（reasoning effort）单调提升性能，但存在收益递减； 多智能体：多个DeepSeek智能体在存在聚合预测信息时表现出预测收敛行为，与独立运行的发散趋势形成对比。
## Look Before You Leap: Autonomous Exploration for LLM Agents【LongCat】

[https://arxiv.org/pdf/2605.16143](https://arxiv.org/pdf/2605.16143)

- 论文提出：形式化自主探索：将环境探索定义为独立于任务执行的、可测量的能力，并引入**探索检查点覆盖率（ECC）**作为可验证的度量指标；探索感知训练：通过交错GRPO训练策略（交替进行任务执行和探索展开），利用ECC奖励显式优化探索能力；探索-行动范式（Explore-then-Act）：将信息收集与任务执行解耦，允许智能体首先利用交互预算自主获取环境知识，然后利用这些知识解决具体任务。简而言之，该论文试图将智能体从"依赖先验知识的任务执行者"转变为"能够在线自主探索并适应新环境的通用智能体"。
## EXG: Self-Evolving Agents with Experience Graphs【Self-Evolving】

[https://arxiv.org/pdf/2605.17721](https://arxiv.org/pdf/2605.17721)

- 解决**大型语言模型（LLM）based agents在部署过程中的行为静态性问题**，即agents在任务执行过程中积累的交互经验难以转化为系统性的、跨任务的持续改进能力。论文提出**EXG（Experience Graph）**框架，通过**将交互经验显式编码为结构化的关系图表示（包含任务锚点、成功案例、警告案例及它们之间的相似性、修正关系）**，实现： 在线实时图增长：执行过程中即时整合新经验并支持跨任务检索； 离线冻结复用：预构建的经验图可直接作为外部记忆模块使用； 即插即用集成：无需修改底层模型参数或agent架构即可增强现有自进化方法。
## Survive or Collapse: The Asymmetric Roles of Data Gating and Reward Grounding in Self-Play RL【Self-Evolving】

[https://arxiv.org/pdf/2605.22217](https://arxiv.org/pdf/2605.22217)

- 解决**自博弈强化学习（Self-Play RL）中的训练崩溃与不稳定问题**。自博弈稳定性由两个独立杠杆控制，但二者的作用存在**不对称性**： 数据级门控（Data Gating）：**决定哪些提议者生成的任务进入训练池的筛选机制** **奖励接地（Reward Grounding）：对已准入任务进行策略更新的奖励信号 ****通过Python输出预测任务与确定性DSL（领域特定语言）对照实验，论文发现： 门控的充分性：****严格的执行门控（确保程序确定性地执行并产生明确输出）足以在任何奖励设计下维持稳定，包括无真实标签访问权限的自一致性奖励（self-consistency reward）****。 奖励的不足性：****一旦移除数据门控，没有任何奖励变体（包括基于执行真实标签的接地奖励）能够防止训练崩溃****。 反直觉现象——"接地提议者悖论"（Grounded Proposer Paradox）：****具有真实标签访问权限的提议者，在与自一致性求解者配对时，反而比无接地的提议者更快地加速崩溃，因为它集中于生成"干净"任务****，这些任务构成了通往虚假自一致吸引子的最快路径。数据准入控制： ****严格门控机制：仅当程序能够成功执行且在两次重复运行中产生确定性输出时，才允许该任务进入训练池（Fε=0 ）****。这过滤了包含哈希随机化、浮点显示差异或副作用的模糊程序。 不对称性验证：实验证明，****在严格门控下，包括无真实标签访问的自一致性奖励（self-consistency reward）在内的所有奖励变体均能保持稳定****；而一旦移除门控，即使使用基于执行真实标签的接地奖励（grounded reward），系统仍会崩溃。**

# AI for Education

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Education
- 方法：agent, reasoning, stat-ml, multimodal-reasoning, stat-me
- 论文/报告：6 篇
- ⭐Generative AI in K-12 Classrooms: A Midyear Implementation Report
- UniER: A Unified Benchmark for Item-level and Path-level Exercise Recommendation
- Teaching AI Through Benchmark Construction: QuestBench as a Course-Based Practice for Accountable Knowledge Work
- ⭐LiveK12Bench: Have Large Multimodal Models Truly Conquered High School-level Examinations?【Tencent】
- ⭐⭐⭐AgentSchool: An LLM-Powered Multi-Agent Simulation for Education【AI Lab】
- A Unified Framework for the Evaluation of LLM Agentic Capabilities
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## ⭐Generative AI in K-12 Classrooms: A Midyear Implementation Report

[https://arxiv.org/pdf/2605.16277](https://arxiv.org/pdf/2605.16277)

- **生成式人工智能在K-12教育场景中的实际应用模式、差异化使用特征及其对学生学业表现的初步影响。**该研究试图在生成式AI快速渗透教育领域的背景下，提供关于**教师实际使用行为模式、情境适应性特征及早期学业信号**的实证基础，为后续大规模效果评估与工具优化提供基线数据。通过将教师平台日志与学生行政记录（人口统计、成绩）匹配，研究发现： 服务特殊群体：服务更高比例英语学习者（ELL>16%）的教师更频繁引用学生画像（Student Profiles）、差异化与可及性（Differentiation & Accessibility）及项目式学习主题；服务更高比例特殊教育（SPED>20%）教师更倾向使用Generate Image与Generate Interactive等课程交付功能。 课堂异质性：面对成绩离散度（p10-p90差距）较大的课堂，教师使用课程交付功能比例更高，且更关注差异化与动机激发主题。基于LLM辅助的自动编码，教师请求以规划（Planning）与显性教学（Explicit Teaching）为主；AI响应则系统性拓展了反馈（Feedback）、学生画像与差异化等主题的讨论。**话语连贯性（Discourse Continuity）**是唯一在教师请求中显著高于AI响应的主题，体现教师对AI输出的主动修正与追问。
## UniER: A Unified Benchmark for Item-level and Path-level Exercise Recommendation

[https://arxiv.org/pdf/2605.16750](https://arxiv.org/pdf/2605.16750)

- 试图解决**练习推荐（Exercise Recommendation, ER）领域中项目级推荐（ILER）与路径级推荐（PLER）两种研究范式长期割裂、缺乏统一评估基准**的问题。现有研究被划分为两个孤立范式： ILER（Item-Level ER）：基于知识追踪（KT）预测下一步练习，优化即时状态转移，但忽视学习连续性； PLER（Path-Level ER）：构建有序学习路径以最大化累积收益，但依赖预定义目标且缺乏灵活反馈。
## Teaching AI Through Benchmark Construction: QuestBench as a Course-Based Practice for Accountable Knowledge Work

[https://arxiv.org/pdf/2605.21413](https://arxiv.org/pdf/2605.21413)

- 这篇论文提出了**基于课程实践的基准构建（course-based benchmark construction）**作为人工智能教育的新范式，旨在培养学生成为负责任的知识行动者（accountable knowledge actors），而非仅仅掌握AI工具的使用技能。该课程设计实现了三个层面的教育目标：角色转变：学生从AI内容的消费者转变为评估标准的设计者，通过构建问题来定义"何种答案可被视为可信";专业知识的重新定位：展示专业知识不仅是AI可检索的内容，更是评判AI输出的权威性基础（如法律学生需明确条文版本，历史学生需验证档案来源）问责能力培养：通过分析AI失效案例，学生学会在问题定义、证据选择、模型使用和答案接受的完整链条中识别需要人类判断的关键节点。
## ⭐LiveK12Bench: Have Large Multimodal Models Truly Conquered High School-level Examinations?【Tencent】

[https://arxiv.org/pdf/2605.26781](https://arxiv.org/pdf/2605.26781)

- 解决现有大型多模态模型（LMMs）在K-12教育场景评估中的三个核心局限性，即**数据泄露、评估维度单一和人工干预过多导致的真实考试环境模拟不足**问题。LiveK12Bench首次实现了对LMMs在真实高中考试环境中**视觉鲁棒性**、**过程严谨性**和**推理效率**的系统性评估，揭示了先进模型（如GPT-5）在考试真实约束下性能显著下降（从79分降至53分）的关键漏洞。论文通过三大核心创新系统性解决上述问题： 动态数据构建流程** 建立基于结构化OCR（MinerU框架）与LLM解析的自动化流水线，持续摄取最新（2026年）真实高中试卷（数学、物理、化学、生物） 采用可变模板方法处理不同版式，实现题目结构化提取与知识点自动标注**，从源头缓解数据污染 Mock Exam多维评估协议 提出四维评估体系，模拟人类考试评分机制： 结果准确性（Outcome）：标准答案匹配度 推理过程严谨性（Process）：定义并检测三类错误： 条件解读错误（CIE）：误读题目数据与约束 逻辑假设错误（LAE）：引入未证实假设 演绎推理错误（DRE）：推理步骤与学科定律冲突 推理效率（Efficiency）：引入ARL（准确率加权响应长度）与**Acc≤r **（时间预算约束下的准确率），衡量模型在有限计算资源下的表现 综合考试分数（OES）：基于题目分值权重的加权总分，融合过程分与结果分 Image-Only（IO）全页输入模态 输入原始考试页面快照，要求模型自主定位、提取并解析题目信息，消除人工干预 构建三个挑战性子集（各600题）： Complex Layout Set：跨页题目、图文混排等复杂版面 Rigorous Process Set：易通过"幸运猜测"答对的陷阱题，强制检验推理严谨性 Long-Horizon Reasoning Set：需长程推理的高难度问题，评估效率与过度思考倾向。真实场景性能显著退化：在考试约束（过程评估+效率评估+Image-Only）下，GPT-5分数从79降至53（满分100），暴露理想化能力与教育就绪性之间的巨大鸿沟；视觉鲁棒性脆弱性：Image-Only模态导致GPT系列准确率下降33.7%，Gemini-3-pro与Kimi-k2.5表现出更强的版面解析能力；错误主要源于图像错位与复杂图文交织；推理过程"幸运猜测"现象：在Rigorous Process Set上，模型常通过错误推理获得正确答案，GPT-5出现61次条件解读错误（CIE）与30次演绎推理错误（DRE），而Gemini系列过程质量显著更高；推理效率差异：Claude-opus-4.6在ARL指标上最优，但Gemini-3-pro在长程推理中过度思考更少；当token预算限制为10%时，多数模型准确率骤降50%，仅Qwen3-VL系列保持相对稳定；学科差异：**所有模型在化学与生物（知识范围广、多图多子问题）上的表现显著弱于数学与物理**。
## ⭐⭐⭐AgentSchool: An LLM-Powered Multi-Agent Simulation for Education【AI Lab】

[https://arxiv.org/pdf/2605.30144](https://arxiv.org/pdf/2605.30144)

- 论文提出AgentSchool——一种将学习建模为**状态转换（state transition）**而非提示行为的LLM多智能体模拟平台。其核心创新在于： 认知可增长的学生智能体：**通过加权学科知识图谱、思维工作流池和显式误解表征，使学习成为可检查的状态演化**； 自适应教师智能体：基于ZPD理论进行规划、脚手架搭建与反思，而非执行固定脚本； 可配置的场景生成器：支持正式/非正式学习场域的构建，允许探索现有课堂模板之外的制度可能性。 简言之，AgentSchool旨在成为教育AI的计算风洞（computational wind tunnel）：在真实学习者承担后果之前，使教育干预的认知轨迹、社会动态与制度影响变得可见、可检查且可重复。创新点不在模型，而在于**提出了一套“可成长学生 + 可反思老师 + 可演化学校”的教育仿真框架**。
## A Unified Framework for the Evaluation of LLM Agentic Capabilities

[https://arxiv.org/pdf/2605.27898](https://arxiv.org/pdf/2605.27898)

- 解决**LLM Agent能力评估中的混杂因素问题**——即**当前基准测试结果往往同时反映了模型本身的能力与特定基准实现选择（scaffold、parser、environment等）的联合效应**，导致跨基准比较难以解释为对底层模型能力的纯净测量。论文提出统一评估框架，通过以下方式解决上述问题：标准化三元组：将异质基准统一为⟨I,T,E⟩（指令-工具-环境）格式固定Scaffold：采用统一的ReAct风格架构（smolagents）执行所有评估，使scaffold级选择恒定可控沙盒：构建隔离的环境副本，支持可选的离线快照模式，将环境波动性与Agent行为分离统一指标：引入任务完成度（TCS）、效率指标（步骤/token/时间）及决策/执行层失败分类法通过该框架，论文实现了对内在LLM能力与框架/环境诱导伪影的解耦，使基准分数成为可解释的、受控的模型能力测量。

# AI for Science

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science
- 方法：agent, ai-for-science, language, reasoning, multimodal-reasoning, protein-language-model
- 论文/报告：8 篇
- Can Coding Agents Reproduce Findings in Computational Materials Science?
- Proteo-R1: Reasoning Foundation Models for De Novo Protein Design
- A-CODE: Fully Atomic Protein Co-Design with Unified Multimodal Diffusion【ByteDance Seed】
- ⭐Towards A Generative Protein Evolution Machine with DPLM-Evo【ByteDance Seed】
- CellScientist: Dual-Space Hierarchical Orchestration for Closed-Loop Refinement of Virtual Cell Models
- ProteinOPD: Towards Effective and Efficient Preference Alignment for Protein Design
- 🧐MoleCode unlocks structural intelligence in large language models
- Protein Fold Classification at Scale: Benchmarking and Pretraining
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Can Coding Agents Reproduce Findings in Computational Materials Science?

[https://arxiv.org/pdf/2605.00803](https://arxiv.org/pdf/2605.00803)

- 论文提出了 AUTOMAT 基准测试，通过85个经领域专家（SME）策划的真实材料科学论文声明，测试编码智能体在以下三个相互关联挑战上的表现： **从论文文本中恢复未明确指定的计算程序**； **操作专业工具链（如Quantum ESPRESSO、LAMMPS等）**； **判断生成的证据是否支持原始科学声明**。 研究发现，当前LLM智能体在AUTOMAT上总体成功率较低（最佳设置仅达54.1%），且从论文文本单独重建工作流程时表现最差，揭示了现有智能体系统在AI-for-science场景中的关键局限性。
## Proteo-R1: Reasoning Foundation Models for De Novo Protein Design

[https://arxiv.org/pdf/2605.02937](https://arxiv.org/pdf/2605.02937)

- 论文提出 Proteo-R1，一种推理引导的蛋白质设计框架，通过以下方式实现突破： 双专家架构解耦：采用多模态大语言模型（MLLM）作为"理解专家"（Eund ），分析序列、结构和文本上下文以识别关键功能残基；采用AlphaFold3风格的扩散模型作为"生成专家"（Egen ），在固定相互作用锚点的约束下进行条件化协同设计。 显式残基级承诺：将推理操作化为稀疏的、残基级别的硬约束（而非潜在文本指导），通过身份固定和嵌入注入机制，将理解专家的决策作为锚点传递给生成专家，实现可解释、模块化的推理-生成接口。 三阶段课程训练：通过多模态对齐、结构推理中期训练和联合推理引导设计三个阶段，逐步建立从符号推理到几何生成的稳定跨模态接地。
## A-CODE: Fully Atomic Protein Co-Design with Unified Multimodal Diffusion【ByteDance Seed】

[https://arxiv.org/pdf/2605.03360](https://arxiv.org/pdf/2605.03360)

- 论文提出A-CODE（Atomic CO-DEsign），通过以下方式解决上述问题： 全原子统一框架：在单一多模态扩散过程中同时细化离散原子类型（atom types）和连续原子坐标（coordinates），直接建模联合分布 p(X,S) 原子中心范式：完全从原子级预测推断残基身份，消除对残基层级类型信息的依赖，移除对残基类型的硬约束 ncAA自然扩展：由于不依赖预定义的残基类型约束，框架可无缝适应非规范氨基酸建模，无需架构修改
## ⭐Towards A Generative Protein Evolution Machine with DPLM-Evo【ByteDance Seed】

[https://arxiv.org/pdf/2605.00182](https://arxiv.org/pdf/2605.00182)

- 论文提出了DPLM-Evo框架，通过以下关键创新实现突破： 将可变长度的观测序列空间与上采样的潜在比对空间（latent alignment space）解耦，使indel感知生成在计算上可行 显式参数化替换、插入和删除三种进化操作的去噪过程 引入上下文相关的进化噪声核（contextualized evolutionary noising kernel），生成具有生物学意义的、依赖于序列上下文的突变模式。
## CellScientist: Dual-Space Hierarchical Orchestration for Closed-Loop Refinement of Virtual Cell Models

[https://arxiv.org/pdf/2605.07335](https://arxiv.org/pdf/2605.07335)

- 本文提出 **CellScientist**，一种面向虚拟细胞建模（Virtual Cell Modeling, VCM）的双空间分层编排框架，旨在解决当前大语言模型（LLM）辅助建模中的**修正-路由困境**（refinement-routing problem）。
## ProteinOPD: Towards Effective and Efficient Preference Alignment for Protein Design

[https://arxiv.org/pdf/2605.10189](https://arxiv.org/pdf/2605.10189)

- 论文提出了ProteinOPD——一个基于**On-Policy Distillation (OPD)**的多目标偏好对齐框架。该方法通过以下机制解决问题： 保持可设计性：利用OPD的"模式寻找"（mode-seeking）特性，而非SFT的"模式覆盖"（mode-covering），避免概率质量分散到次优选项，从而缓解灾难性遗忘； 多目标平衡：将单教师OPD扩展为多教师几何共识（normalized Product-of-Experts），在token级别对齐多个偏好特定的教师模型，即使在目标冲突时也能提供有界的优化信号； 提升效率：采用token级密集监督，相比RL方法实现8倍的训练加速，同时仅需少量离线数据构建教师模型。
## 🧐MoleCode unlocks structural intelligence in large language models

[https://arxiv.org/pdf/2605.16480](https://arxiv.org/pdf/2605.16480)

- 解决**大语言模型（LLMs）在处理分子结构时面临的隐式拓扑重建瓶颈**。论文提出**通过**显式图语言（MoleCode）**重新设计分子-语言接口，将分子拓扑直接编码为带持久标识符的类型化实体（Subgraph–Node–Edge语法）**，使LLM能够：直接在上下文窗口中读取、编辑和审计分子结构将推理资源从"结构重建"重新分配到"化学定向推理"支持从原子注释到Markush结构、反应机理和多模态科学文档的统一表示简言之，该论文论证：当推理对象是关系型结构时，结构本身应成为语言的显式组成部分，而非隐藏在待解码的文本之后。
## Protein Fold Classification at Scale: Benchmarking and Pretraining

[https://arxiv.org/pdf/2605.18552](https://arxiv.org/pdf/2605.18552)

- 如何确保在AlphaFold预测结构上训练的模型能够可靠地迁移到实验解析的蛋白质结构上。 为应对这些挑战，论文引入了TEDBench（一个包含462,175个预测结构和27,638个实验结构的大规模基准）和MiAE（Masked Invariant Autoencoders，一种基于高比例掩码（可达90%）和不对称编解码器架构的自监督学习框架）。
