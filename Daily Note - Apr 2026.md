---
source: https://luminous-mat-781.notion.site/Daily-Note-April-2026-337e8c0e89c5807fbae0d0ee2b12735b?source=copy_link
---

# Daily Note - Apr 2026

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Daily Note - Apr 2026
- 方法：generation, language, vision-language-model, reasoning, vision, reinforcement-learning, multimodal-learning, vision-language
- 论文/报告：17 篇
- Dynin-Omni: Omnimodal Unified Large Diffusion Language Model【Unified Model】
- TorchUMM: A Unified Multimodal Model Codebase for Evaluation, Analysis, and Post-training【Unified Model】
- LLaDA2.0-Uni: Unifying Multimodal Understanding and Generation with Diffusion Large Language Model【Unified Model】
- Tuna-2: Pixel Embeddings Beat Vision Encoders for Multimodal Understanding and Generation【Unified Model, Meta】
- MMCORE: MultiModal COnnection with Representation Aligned Latent Embeddings【ByteDance Seed】
- Context Unrolling in Omni Models【ByteDance Seed】
- 📌KAT-Coder-V2 Technical Report【Code, Kuaishou】
- 📌⭐⭐Olmo Hybrid: From Theory to Practice and Back【Linear Attention】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Dynin-Omni: Omnimodal Unified Large Diffusion Language Model【Unified Model】

[https://arxiv.org/pdf/2604.00007](https://arxiv.org/pdf/2604.00007)

- Dynin-Omni通过以下方式解决上述问题： 原生统一架构：将全模态建模表述为共享离散令牌空间上的掩码扩散（masked diffusion），通过双向注意力机制和迭代去噪，实现任何模态到任何模态（any-to-any）的生成与理解 模态解耦的多阶段训练：引入显式解耦模态扩展与能力扩展的训练范式，结合模态解耦模型合并（modality-disentangled model merging）技术，在扩展新模态的同时保持原有知识 统一目标函数：所有模态共享单一的掩码扩散训练目标，无需外部生成器或异构损失函数。
## TorchUMM: A Unified Multimodal Model Codebase for Evaluation, Analysis, and Post-training【Unified Model】

[https://arxiv.org/pdf/2604.10784](https://arxiv.org/pdf/2604.10784)

- 论文提出了**TorchUMM**——首个面向UMMs的统一代码库与基准框架，通过标准化接口、统一评估协议和模块化后训练支持，实现对异构模型在一致性设定下的公平比较与深度行为分析。
## LLaDA2.0-Uni: Unifying Multimodal Understanding and Generation with Diffusion Large Language Model【Unified Model】

[https://arxiv.org/pdf/2604.20796](https://arxiv.org/pdf/2604.20796)

- LLaDA2.0-Uni的解决方案： 通过**引入SigLIP-VQ语义离散tokenizer、基于MoE的扩散语言模型骨干网络和蒸馏优化的扩散解码器**，在单一框架内实现： 完全语义化的统一token表示（同时支持理解与生成） 块级掩码扩散目标（平衡并行解码与建模稳定性） 支持可变长度的开放式生成 通过SPRINT加速框架和8步蒸馏解码器实现高效推理 该架构首次在离散扩散框架内实现了与专用VLM相当的理解性能，同时保持强大的生成和编辑能力，并原生支持交错生成与推理（interleaved generation and reasoning）。
## Tuna-2: Pixel Embeddings Beat Vision Encoders for Multimodal Understanding and Generation【Unified Model, Meta】

[https://arxiv.org/pdf/2604.24763](https://arxiv.org/pdf/2604.24763)

- 论文提出Tuna-2，其核心创新在于：**完全移除视觉编码器：通过简单的Patch嵌入层直接将原始像素转换为视觉Token，彻底摒弃VAE和表示编码器等模块化设计，实现真正的端到端像素空间建模**。统一像素空间学习：在单一Transformer架构中联合处理图像和文本Token，使理解和生成共享同一像素级表示空间，消除任务间的表示错位。掩码特征学习机制：**针对高维像素空间学习的挑战，引入基于掩码的视觉特征学习方案，通过随机遮蔽图像块来增强表示鲁棒性，避免模型依赖表面捷径**。实验表明，这种无编码器设计在充分预训练后，不仅在图像生成质量上与潜在空间方法竞争，更在细粒度视觉感知任务上显著优于基于编码器的变体，证明预训练视觉编码器并非多模态建模的必要条件。主要结论：**预训练视觉编码器非必需：简单的 Patch 嵌入配合端到端训练即可学习强大的统一视觉表征** 像素空间建模的可扩展性：像素空间流匹配不仅能实现高质量图像生成，更在细粒度视觉感知上优于潜在空间方法 架构简化优势：消除编码器-解码器间的模块化隔阂，促进理解与生成任务的深度协同优化
## MMCORE: MultiModal COnnection with Representation Aligned Latent Embeddings【ByteDance Seed】

[https://arxiv.org/pdf/2604.19902](https://arxiv.org/pdf/2604.19902)

- MMCORE 通过**表示对齐的潜在嵌入**连接预训练 VLM 与扩散模型，在保持高保真合成质量的同时，显著降低了多模态生成的训练门槛，实现了文本到图像生成、单图/多图编辑及复杂交错场景生成的统一框架。
## Context Unrolling in Omni Models【ByteDance Seed】

[https://arxiv.org/pdf/2604.21921](https://arxiv.org/pdf/2604.21921)

- 传统多模态系统中，各模态（文本、图像、视频、3D几何）仅提供世界知识流形的局部有偏投影。本文旨在突破单一模态的局限性，通过跨模态深度推理整合互补信息，解决视觉生成中的语言-图像歧义、空间理解中的几何模糊以及3D几何估计中的结构不确定性等问题。论文将推理重新建模为迭代式上下文构建过程：Ct+1=Ct⊕ϕt(x,Ct),y=ψ(x∣CT)其中，Ct为共享工作空间，ϕt为可调用原子原语（如文本描述、相机姿态估计、视觉令牌生成、新视角合成、深度估计），⊕为上下文组合操作。该机制使模型能够**动态选择和整合异构上下文**，将最终预测转化为上下文条件化推理而非直接映射。
## 📌KAT-Coder-V2 Technical Report【Code, Kuaishou】

[https://arxiv.org/pdf/2603.27703](https://arxiv.org/pdf/2603.27703)

- 论文提出 KAT-Coder-V2，采用 "Specialize-then-Unify"（先专精后统一） 范式： 将能力解耦为**五个正交专家领域（SWE、WebCoding、Terminal、WebSearch、General）独立训练** 开发模块化基础设施 KwaiEnv，实现数据集、沙箱、脚手架与验证器的完全解耦 提出 MCLA（Monte-Carlo Log-probability Averaging） 稳定 MoE RL 训练，以及 Tree Training 消除树结构轨迹的冗余计算（最高实现 6.2× 加速） **通过 On-Policy Distillation（OPD） 将多领域专家能力无损融合为单一部署模型。**
## 📌⭐⭐Olmo Hybrid: From Theory to Practice and Back【Linear Attention】

[https://arxiv.org/pdf/2604.03444](https://arxiv.org/pdf/2604.03444)

- **验证混合架构（结合注意力机制与线性循环神经网络）相对于纯Transformer架构在语言建模中的优势，特别是在大规模预训练场景下的效率、性能与可扩展性方面。**
## 📌Nemotron 3 Nano Omni: Efficient and Open Multimodal Intelligence【NVIDIA】

[https://arxiv.org/pdf/2604.24954](https://arxiv.org/pdf/2604.24954)

- 通过**渐进式多模态对齐**、**创新性的 token 压缩技术**与**高效的 MoE 架构**，在统一处理文本、图像、视频、音频的同时，实现了领先的长上下文理解能力与推理效率，为开放的全模态智能研究提供了完整的模型、数据与工具链基础。
## Seeing but Not Thinking: Routing Distraction in Multimodal Mixture-of-Experts

[https://arxiv.org/pdf/2604.08541](https://arxiv.org/pdf/2604.08541)

- 多模态MoE模型在处理视觉输入（如数学题图像）时，即使能准确提取所有文本和数值信息，仍频繁出现推理错误；而面对语义完全相同的纯文本输入时却能正确求解。这种"感知正确但推理失败"的解耦现象挑战了传统认知，需探究其深层机制。量化同一样本在视觉与文本输入下的路由分布差异（Jensen-Shannon Divergence），发现： 中间层（6-42层）存在显著路由发散 发散程度与推理准确率负相关（r 越高，准确率越低） 68.2%-73.1%的失败源于推理错误而非感知错误 假说：视觉输入干扰了路由机制，导致中间层未能充分激活任务相关的领域专家，造成"有推理能力但未被调用"的失效模式。提出轻量级推理时干预策略： 领域专家识别：对比领域数据（如GSM8K）与通用数据（Alpaca）的Top-K激活频率差 ΔΦl,i ，**识别领域专家集合（仅需20样本，对信息完整性鲁棒） **软干预：对领域专家logits施加适度增强 r′l,k←rl,k+λ⋅s(rl) ，保持路由灵活性 层选择性干预：仅干预中间层（或特定模型需包含早期层），避免破坏视觉编码。
## VisionFoundry: Teaching VLMs Visual Perception with Synthetic Images**【Zhuang Liu】**

[https://arxiv.org/pdf/2604.09531](https://arxiv.org/pdf/2604.09531)

- 探索无需参考图像或昂贵人工标注的针对性合成监督（targeted synthetic supervision）能否有效缓解上述瓶颈： 任务关键词驱动：仅输入任务名称（如"Depth Order"、"Orientation and Direction"），利用大语言模型（LLMs）自动生成问题-答案对（QA pairs）和文生图（T2I）提示。 自动化验证闭环：通过现代T2I模型生成图像后，使用强大多模态评判模型（frontier VLM）进行对齐验证（alignment verification），过滤不一致样本，确保监督信号的可靠性。 构建专用数据集：基于VisionFoundry流程构建VisionFoundry-10K数据集（包含10个视觉感知任务的10k个合成VQA三元组），验证**合成数据对视觉感知能力的提升效果。**
## General365: Benchmarking General Reasoning in Large Language Models Across Diverse and Challenging Tasks【LongCat】

[https://arxiv.org/pdf/2604.11778](https://arxiv.org/pdf/2604.11778)

- GENERAL365基准 为应对上述问题，论文构建了GENERAL365基准测试，其核心设计原则包括： 知识解耦：严格限制背景知识在K-12水平，确保评估聚焦于纯推理能力而非知识检索 高多样性：通过365个手工设计的种子问题（扩展为1,095个变体）覆盖八种推理挑战类别（复杂约束、分支枚举、时空推理、递归回溯、语义干扰、隐式信息、最优策略、概率不确定性） 高挑战性：即使是当前最先进的模型（如Gemini-3-Pro）准确率也仅为62.8%，大多数模型未能达到60%的及格线 实验结果表明，当前LLMs的推理能力高度依赖特定领域，在一般化、去知识化的推理场景中仍有显著提升空间。
## HorizonBench: Long-Horizon Personalization with Evolving Preferences【Long Horizon, Meta】

[https://arxiv.org/pdf/2604.17283](https://arxiv.org/pdf/2604.17283)

- 评估LLM追踪这些不断变化的状态的能力。虽然现有基准测试长时记忆（从海量上下文中检索信息的能力），但它们通常**难以诊断当偏好改变时模型失败的原因。**模型是未能找到旧信息，还是找到了旧信息但未能意识到它不再真实？**HORIZONBENCH通过使用“状态优先”的数据生成方法来解决这个问题**，维护用户心理状态的结构化记录，作为偏好演变的真实地图。为了提供必要的真实数据，研究人员开发了一个数据生成器，它颠覆了传统的对话数据创建方式。他们不是先生成对话再尝试标记它，而是首先构建一个结构化的心理状态图 $G$，然后生成反映其状态的对话。研究人员利用此生成器，构建了 HORIZONBENCH，一个包含4,245道选择题的数据集，**这些题目来源于360位模拟用户。每位用户的历史大约跨越6个月，平均包含4,300个对话轮次和163,000个token。**
## The Tool-Overuse Illusion: Why Does LLM Prefer External Tools over Internal Knowledge?

[https://arxiv.org/pdf/2604.19749](https://arxiv.org/pdf/2604.19749)

- 解决**大语言模型（LLMs）在与外部工具集成推理（Tool-Integrated Reasoning, TIR）时出现的工具过度使用（Tool Overuse）问题**。研究发现：普遍性：所有模型在无需外部协助的简单任务上均存在非零工具调用频率，平均每查询产生0.93次不必要调用；性能损害：工具使用导致简单任务准确率下降3.29%–14.48%（如DeepSeek-R1下降16.78%），且引入不必要的计算开销与上下文负担。模型无法准确感知其内部知识的实际边界。通过 avg@1024 指标（1,024次独立推理的平均准确率）量化内部知识可用性发现： - 即使在高知识可用性区域，模型仍保持高工具调用率（如Qwen3-8B平均**2.2次/查询**） avg@1024>0.9 - 模型表现出"对内不自信、对外过度依赖"的系统性误判，无法识别内部知识已足以解决问题的状态。在RLVR（Reinforcement Learning with Verifiable Rewards）训练中，仅关注最终答案正确性的奖励结构（R⋅1(y=y∗) ）忽视了工具使用效率。这导致： 训练过程中工具调用次数随步数单调递增（7B模型从2.2次增至6.8次） 即使工具仅带来极微小的可靠性提升（ΔP→0+ ），由于效率惩罚 λ→0 ，工具调用仍成为理性最优解。
## Render-in-the-Loop: Vector Graphics Generation via Visual Self-Feedback

[https://arxiv.org/pdf/2604.20730](https://arxiv.org/pdf/2604.20730)

- 解决**多模态大语言模型（MLLMs）在生成可缩放矢量图形（SVG）时存在的"开环盲画"（open-loop blind drawing）问题**。论文提出Render-in-the-Loop范式，将SVG生成重新表述为逐步的、视觉上下文感知的过程： 通过将中间代码状态实时渲染为累积画布，模型在每一步明确观察演变的视觉上下文 建立"绘制-观察-反馈"的闭环机制，使模型的"绘制之手"与"观察之眼"同步 通过视觉自反馈（Visual Self-Feedback）训练，使模型能够基于当前画布状态精确预测下一个几何图元 该范式的关键在于：将视觉反馈作为密集、直接的上下文信息，而非压缩的标量奖励，从而解锁MLLMs在矢量图形合成中的视觉推理潜力。
## Coding with Eyes: Visual Feedback Unlocks Reliable GUI Code Generating and Debugging

[https://arxiv.org/pdf/2604.19750](https://arxiv.org/pdf/2604.19750)

- 目前，大多数AI代理尝试通过查看执行日志或界面（如HTML或XML树）的文本表示来调试GUI代码。这种方法无法捕获视觉渲染缺陷——例如元素重叠、颜色不正确或布局损坏——这些缺陷对人眼来说一目了然，但在文本日志中却不可见。
## Scaling Self-Play with Self-Guidance

[https://arxiv.org/pdf/2604.20209](https://arxiv.org/pdf/2604.20209)

- 斯坦福大学的研究人员引入了一种名为“自引导自博弈”（Self-Guided Self-Play, SGS）的算法，该算法**使用一个集成的“引导”语言模型，以确保为求解器模型生成高质量、相关的合成问题**。这种方法使得一个7B参数的模型在形式定理证明中超越了671B参数模型的pass@4性能，并比基线强化学习方法实现了高出7%的渐近求解率。目录持续推进大型语言模型自我对弈自引导自我对弈的三个角色奖励质量和相关性的机制管理求解器熵和探索性能和扩展性分析为什么指导模型是必要的：一项消融研究结论与未来方向。

# Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reasoning
- 方法：agent, generation, language, vision-language-model, reasoning, vision, reinforcement-learning, optimization
- 论文/报告：26 篇
- AutoGraph-R1: End-to-End Reinforcement Learning for Knowledge Graph Construction【Microsoft】
- Therefore I am. I Think
- LongCoT: Benchmarking Long-Horizon Chain-of-Thought Reasoning
- Rethinking On-Policy Distillation of Large Language Models: Phenomenology, Mechanism, and Recipe
- Think Anywhere in Code Generation【Tongyi】
- ATP-Bench: Towards Agentic Tool Planning for MLLM Interleaved Generation【Visual Reasoning, Qwen】
- MuSEAgent: A Multimodal Reasoning Agent with Stateful Experiences【Visual Reasoning】
- Agentic-MME: What Agentic Capability Really Brings to Multimodal Intelligence?【Visual Reasoning】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## AutoGraph-R1: End-to-End Reinforcement Learning for Knowledge Graph Construction【Microsoft】

[https://arxiv.org/pdf/2510.15339](https://arxiv.org/pdf/2510.15339)

- 将“知识图谱构建”形式化为一个**策略学习问题**，用RL把下游 RAG 的**任务性能**直接反向传播给图谱生成器，从而解决“建图”与“用图”脱节的难题。
## Therefore I am. I Think

[https://arxiv.org/pdf/2604.01221](https://arxiv.org/pdf/2604.01221)

- 通过针对工具调用（tool-calling）场景设计的实验，论文提供了证据表明：**推理模型往往在进行可见的文本推理之前就已经编码了行动选择，且当这些早期信号被扰动时，模型倾向于通过思维链合理化被诱导的决策，而非基于推理过程重新评估决策**。这一发现对理解模型推理的忠实性、可解释性以及测试时计算效率具有重要 implications。技术手段 线性探针：训练逻辑回归分类器 y^=σ(w⊤x) 从隐藏状态预测二分类决策（调用/不调用工具） 激活引导：在残差流中施加 h′=h+αv ，其中 v 为类间均值差向量，α∈{4,8,12} （或 {10,20,30} ）控制干预强度 行为分析：使用 GPT-5.4 与 Claude Sonnet 4.6 作为盲评评判员，将干预响应分类为 6 类行为模式（包括无缝分歧、虚构支持、约束覆盖、推理膨胀等）
## LongCoT: Benchmarking Long-Horizon Chain-of-Thought Reasoning

[https://arxiv.org/pdf/2604.14140](https://arxiv.org/pdf/2604.14140)

- 论文提出了**LongCoT**基准，通过构建2,500个需要显式或隐式依赖图结构的专家设计问题（涵盖化学、数学、计算机科学、国际象棋和逻辑学领域），直接评估模型在数十至数十万tokens推理跨度内的长程规划、状态维护、错误发现和信用分配能力。在2,500题（含500题简易版LongCoT-mini）上的评估显示： 性能极低：最佳模型GPT 5.2在LongCoT上仅达**9.83%**准确率（平均使用62K tokens），Gemini 3 Pro为6.08%，其余前沿模型（Claude、Grok、DeepSeek、Kimi、GLM）接近0%。 领域稳定性：各模型在五个领域表现一致，失败源于长程推理机制而非特定领域知识缺失。 随长度衰减：控制实验显示，当DAG节点数超过15时，所有模型准确率急剧下降，且显著低于独立错误假设的理论值（图6），证明存在特有的长程失效模式（错误传播、规划漂移）。 脚手架抗性：递归语言模型（RLM）在无代码执行时未超越基线；允许代码执行仅在隐式领域（逻辑、国际象棋）有效，显式组合领域（数学、化学）仍接近0%，确认LongCoT测量的是内部推理能力而非工具使用。
## Rethinking On-Policy Distillation of Large Language Models: Phenomenology, Mechanism, and Recipe

[https://arxiv.org/pdf/2604.13016](https://arxiv.org/pdf/2604.13016)

- 解决在策略蒸馏（On-Policy Distillation, OPD）训练动态 poorly understood 且实践脆弱的问题。 具体而言，论文聚焦于以下核心问题： OPD 的成功与失败条件：尽管 OPD 已成为大语言模型后训练的核心技术，但存在反直觉的失败模式——例如，**能力更强的教师模型可能完全无法提升学生模型，而能力较弱的教师反而能成功**。论文旨在系统性地识别支配 OPD 成效的关键条件。 Token 级别的作用机制：既有研究尚未充分解释教师模型的 token 级信号如何引导学生分布向期望方向演进，以及在何种条件下该信号会失效。论文试图揭示 OPD 在微观（token）层面的优化机制。 实践中的不稳定性：OPD 在实际应用中表现出脆弱性，包括长轨迹上的奖励质量退化、教师-学生思维模式不匹配导致的训练失效等。论文致力于提出可操作的策略来修复失败的 OPD 配置，并探讨其在大规模或长程任务中的局限性。 通过现象学分析（Phenomenology）、机制剖析（Mechanism）和实践配方（Recipe）三个层面的系统研究，论文旨在建立对 OPD 训练动态的完整理解，并为其在工业级 post-training 管道中的可靠应用提供理论指导和实践方案。核心发现：成败的两大条件（现象学） 通过对比实验与逆向蒸馏（Weak→Strong）验证，论文识别出支配 OPD 成效的两个必要条件： **思维模式一致性（Thinking-Pattern Consistency）：师生模型需共享兼容的思维模式（表现为 top-k token 分布的高初始重叠率）。即使教师基准分数更高，思维模式失配也会削弱 token 级蒸馏信号，且早期失配造成的损失无法通过后续训练完全恢复**。 **新知识条件（New Knowledge ≠ Higher Scores）：教师必须提供学生训练期间未获得的真正新能力，而非仅仅是更高的基准分数或更大的模型规模。**同一家族、仅规模不同的模型（如 1.5B 与 7B）可能从学生视角看是分布不可区分的，导致 OPD 失败；而经 RL 后训练获得新能力的较弱教师却能成功。
## Think Anywhere in Code Generation【Tongyi】

[https://arxiv.org/pdf/2603.29957](https://arxiv.org/pdf/2603.29957)

- 论文提出了THINK-ANYWHERE机制，其核心创新在于： 按需推理（On-demand Reasoning）：**允许模型在代码生成的任意token位置动态调用思考块（通过⟨thinkanywhere⟩ 和⟨/thinkanywhere⟩ 标记），实现真正的"在需要时思考"** 两阶段训练范式： 冷启动训练（Cold-start Training）：通过监督学习使模型掌握在中断位置插入思考块的基本能力 强化学习优化（RLVR）：利用基于结果的强化学习奖励，驱动模型自主探索最优的推理触发位置和策略 动态计算分配：使模型能够根据即时上下文和局部复杂度，在高熵（高不确定性）位置精确分配推理资源，实现计算效率与生成质量的平衡 该机制通过允许模型在实现过程中"随处思考"，有效解决了前置思考无法应对代码生成动态复杂性的问题，同时提供了更高的可解释性（通过观察模型的思考位置可理解其决策过程）。
## ATP-Bench: Towards Agentic Tool Planning for MLLM Interleaved Generation【Visual Reasoning, Qwen】

[https://arxiv.org/pdf/2603.29902](https://arxiv.org/pdf/2603.29902)

- 论文提出Agentic Tool Planning作为下一代交错式生成的里程碑，其中**模型作为中央控制器，自主决定何时（when）、何地（where）、调用哪些（which）工具（如引用、扩散生成、搜索、代码绘图、编辑等）来产生响应****。**然而，评估这种能力面临以下挑战： 开放式任务缺乏确定性标准答案； 需要解耦工具规划能力与端到端执行及后端工具变化的影响； 需同时评估工具调用的精确性（precision）、召回性（识别遗漏的视觉机会）和整体响应质量。 为应对上述问题，论文引入了ATP-Bench基准测试和**MAM（Multi-Agent MLLM-as-a-Judge）**评估系统，首次实现了对MLLMs在统一框架下协调事实性引用与创造性生成能力的系统评估。
## MuSEAgent: A Multimodal Reasoning Agent with Stateful Experiences【Visual Reasoning】

[https://arxiv.org/pdf/2603.27813](https://arxiv.org/pdf/2603.27813)

- 论文提出MuSEAgent框架，通过以下关键创新实现改进： 状态化经验抽象：将历史轨迹分解为原子化的状态-动作对（state-action pairs），通过事后推理（hindsight reasoning）评估和提取高质量的决策指导，构建紧凑且噪声更少的经验库。 组合式状态表示：将多模态状态分解为多个语义视角（semantic viewpoints），支持基于不同状态组件的灵活经验索引和检索。 深度-广度搜索机制（Deep-and-Wide Search）：在推理时动态组合广度搜索（跨任务策略知识）和深度搜索（多视角迭代精化），实现自适应的经验利用。
## Agentic-MME: What Agentic Capability Really Brings to Multimodal Intelligence?【Visual Reasoning】

[https://arxiv.org/pdf/2604.03016](https://arxiv.org/pdf/2604.03016)

- 针对**MLLMs从被动感知向主动代理演进的趋势，系统性地解决了现有评估体系在工具整合**、**能力协同**与**过程验证**方面的关键缺陷，提出了**Agentic-MME**——一个面向真实世界任务的过程验证型基准测试。**Agentic-MME基准**：**首个整合视觉工具与开放网络搜索的过程验证型基准**，支持统一框架下的异构工具接口评估。
## Vero: An Open RL Recipe for General Visual Reasoning【Visual Reasoning, Zhuang Liu】

[https://arxiv.org/pdf/2604.04917](https://arxiv.org/pdf/2604.04917)

- 论文提出**Vero**，通过一个完全开源的RL配方（包括Vero-600K数据集、任务路由奖励函数和系统化的消融研究），证明单一阶段的RL训练配合**多样化的任务覆盖**和**任务特定奖励设计**，即可在六个广泛的视觉推理类别（Chart & OCR、STEM、Spatial & Action、Knowledge & Recognition、Grounding/Counting/Search、Captioning & Instruction Following）上实现最先进的性能，而无需分阶段训练或专有数据。
## Think in Strokes, Not Pixels: Process-Driven Image Generation via Interleaved Reasoning【Visual Reasoning, Meta】

[https://arxiv.org/pdf/2604.04746](https://arxiv.org/pdf/2604.04746)

- 本文介绍了一种**过程驱动的图像生成范式（Process-Driven Image Generation）**，通过**交错推理（Interleaved Reasoning）**将**单步黑箱生成转变为显式、可解释的多步建构过程**。提出**Plan → Sketch → Inspect → Refine**的递归生成范式，将图像生成交错序列化。
## OpenVLThinkerV2: A Generalist Multimodal Reasoning Model for Multi-domain Visual Tasks【Visual Reasoning】

[https://arxiv.org/pdf/2604.08539](https://arxiv.org/pdf/2604.08539)

- 论文提出： Gaussian GRPO (G²RPO)：通过一维最优传输将任意任务的经验奖励分布非线性映射至标准正态分布 N(0,1) ，从数学上确保任务间梯度公平性，并对异常值具有内在鲁棒性 任务级塑造机制：包括**响应长度塑造（动态平衡推理链长度）和熵塑造（防止熵崩溃与熵爆炸）**，以无额外标注开销的方式协调感知与推理的优化轨迹。
## Walk the Talk: Bridging the Reasoning-Action Gap for Thinking with Images via Multimodal Agentic Policy Optimization【Visual Reasoning】

[https://arxiv.org/pdf/2604.06777](https://arxiv.org/pdf/2604.06777)

- 论文提出了**Multimodal Agentic Policy Optimization (MAPO)**，通过强制模型为视觉内容生成显式文本描述，并利用CLIP模型验证描述与实际观察的语义对齐，从而在优势估计中耦合语义一致性与任务奖励，弥合推理与行动之间的差距。
## MMEmb-R1: Reasoning-Enhanced Multimodal Embedding with Pair-Aware Selection and Adaptive Control【Visual Reasoning】

[https://arxiv.org/pdf/2604.06156](https://arxiv.org/pdf/2604.06156)

- 论文提出了MMEmb-R1框架，其核心创新包括： 将推理路径建模为潜在变量（latent variable）r∼P(R) ，而非确定性输出； 引入对感知推理选择（pair-aware reasoning selection）机制，通过反事实干预（counterfactual intervention）量化各推理路径对查询-目标对齐的边际贡献； 采用基于GRPO（Group Relative Policy Optimization）的自适应推理控制，学习仅在推理能带来实质收益时（即推理效用 gap δi=sri−sdi>0 ）才调用推理路径，从而在效果与效率之间取得平衡。
## Don't Show Pixels, Show Cues: Unlocking Visual Tool Reasoning in Language Models via Perception Programs【Visual Reasoning】

[https://arxiv.org/pdf/2604.12999](https://arxiv.org/pdf/2604.12999)

- 论文提出了 **Perception Programs (P2)** ——一种无需训练、模型无关的表征转换方法，**将密集的工具输出重写为紧凑、结构化、语言原生的摘要（compact, structured, language-native summaries）**，**使MLLMs能够直接"阅读"视觉模态而非从像素中推断**，从而显著提升在深度估计、光流分析、视觉对应等感知中心任务上的推理能力。
## Test-time Scaling over Perception: Resolving the Grounding Paradox in Thinking with Images【Visual Reasoning】

[https://arxiv.org/pdf/2604.11025](https://arxiv.org/pdf/2604.11025)

- 论文识别出工具增强视觉推理的根本性障碍：Grounding Paradox。其形式化定义为：** 多模态模型因当前全局感知状态 Sglobal 缺乏回答查询 Q 所需的细粒度信息而调用视觉工具，但产生有效工具调用（特别是选择正确区域 R⊂I 进行细粒度检查）本身就需要尚未获取的细粒度判别信息**。 该悖论揭示了一个循环依赖：模型必须在尚未感知到证据的情况下先定位该证据。现有系统因此常表现为投机性探测——检查无关区域、遗漏关键证据或无法从早期错误定位中恢复。论文提出**TTSP框架**，其核心思想是**将感知本身视为可扩展的推断过程**，通过测试时计算分配实现从"单次猜测"到"迭代式感知假设检验"的转变。
## ReactBench: A Benchmark for Topological Reasoning in MLLMs on Chemical Reaction Diagrams【Visual Reasoning】

[https://arxiv.org/pdf/2604.15994](https://arxiv.org/pdf/2604.15994)

- ReactBench基准 论文提出通过**化学反应图（Chemical Reaction Diagrams）**作为理想测试平台，**构建包含1,618个专家标注问答对的ReactBench基准，系统评估四个维度的能力： 空间元素定位（Spatial Element Localization） 拓扑信息提取（Topological Information Extraction） 路径连通性追踪（Pathway Connectivity Tracing） 结构拓扑推理（Structural Topology Reasoning） **该基准的核心价值在于揭示：MLLMs能够精确感知局部视觉锚点（准确率超80%），但在需要整合局部元素形成全局结构表示的整体拓扑推理任务中表现显著下降（准确率低于55%），从而证实当前架构在层级抽象（hierarchical abstraction）和全局结构理解方面存在根本性缺陷。
## Unveiling Fine-Grained Visual Traces: Evaluating Multimodal Interleaved Reasoning Chains in Multimodal STEM Tasks【Visual Reasoning】

[https://arxiv.org/pdf/2604.19697](https://arxiv.org/pdf/2604.19697)

- 一个新的研究生级别多模态STEM任务基准STEPSTEM，需要真正的跨模态推理，并辅以一个细粒度评估框架，表明当前的多模态大语言模型（MLLM）在最终答案准确性方面表现有限，并且难以有效地将视觉生成整合到其问题解决过程中。目录多模态STEM推理的挑战STEPSTEM数据集：设计与整理一个精细的评估框架主要发现和模型分析工作的重要性
## SSL-R1: Self-Supervised Visual Reinforcement Post-Training for Multimodal Large Language Models【Visual Reasoning, Google】

[https://arxiv.org/pdf/2604.20705](https://arxiv.org/pdf/2604.20705)

- 论文提出了SSL-R1，一个通用的自监督强化学习后训练框架。该框架的核心思想是：视觉中心的奖励设计：直接从输入图像中派生内在可验证的奖励，无需人工或外部模型监督自监督任务重构：**将视觉领域中广泛使用的自监督预文本任务（如旋转预测、视觉相似性判断、区域修复、块排序和几何对应）重新构建为可验证的视觉谜题**，作为强化学习的训练信号多粒度视觉理解：覆盖从图像级别到像素级别的多样化视觉推理能力，包括细粒度感知、空间理解和组合理解通过这种方式，SSL-R1旨在实现一种成本效益高、可扩展且视觉中心的MLLM后训练范式，从根本上提升模型的多模态理解和推理能力。
## BioAlchemy: Distilling Biological Literature into Reasoning-Ready Reinforcement Learning Training Data

[https://arxiv.org/pdf/2604.03506](https://arxiv.org/pdf/2604.03506)

- 从ASM、PLoS Genetics、PLoS Computational Biology、Semantic Scholar获取77.7K篇研究论文。 通过自动化QA生成流水线，构建BioAlchemy-345K数据集，包含345K个具有精确答案（exact-match可验证）的科学推理问题，规模是现有最大生物学推理数据集的6.5倍。**RL优越性**：在相同数据上，RL训练较SFT提升**+4.17%**整体性能。分布对齐重要性：严格对齐PubMed分布（α=1.0）在固定数据规模下表现最优，过度上采样尾部主题（α=0.3）反而导致性能下降（-1.97%）。数据规模效应：训练数据从50K增至150K，所有基准测试性能持续提升。BioAlchemist-8B（基于Qwen3-8B，150K样本训练）： 较基座模型整体提升9.12% 在LAB-Bench（ProtocolQA、SeqQA、Cloning Scenarios）、GPQA-Diamond（遗传学与分子生物学）、PubMedQA上超越同类规模模型（包括DeepSeek-R1-Llama-8B和GPT-OSS-20B）。
## A Decomposition Perspective to Long-context Reasoning for LLMs【Long Context, Tencent】

[https://arxiv.org/pdf/2604.07981](https://arxiv.org/pdf/2604.07981)

- 论文提出**基于分解视角（Decomposition Perspective）**的方法论： 能力解构：****将长上下文推理分解为五个层次化的原子技能（Atomic Skills）——基础检索（Foundational Retrieval）、抗干扰（Anti-Interference）、全局整合（Global Integration）、关系推理（Relational Reasoning）和动态状态跟踪（Dynamic State Tracking）**** 自动化数据合成：设计**锚点推理（Anchor-based Reasoning, AbR）**框架，通过算法生成的锚点-问题对嵌入长文本，实现可验证、难度可控的数据集自动构建 针对性强化学习：利用 GRPO等强化学习算法，在仅约 4,000 个合成样本上定向增强原子技能，进而提升模型在一般长上下文推理任务上的表现。
## RAGEN-2: Reasoning Collapse in Agentic RL

[https://arxiv.org/pdf/2604.06268](https://arxiv.org/pdf/2604.06268)

- 解决**多轮LLM智能体强化学习（RL）中的模板崩溃（Template Collapse）问题**——即模型在训练过程中逐渐产生看似多样但实际上与输入无关的固定推理模板，而现有监控指标无法检测此现象，导致智能体推理能力 silently degrade（静默退化）。机制解释（SNR视角）： 论文提出模板崩溃源于信噪比（SNR）失衡： 信号（Signal）：任务梯度依赖同一输入内不同轨迹的奖励差异（reward variance） 噪声（Noise）：KL散度与熵正则化项对所有输入施加均匀的、与输入无关的收缩压力 崩溃条件：当奖励方差（Reward Variance, RV）过低时，任务梯度减弱，正则化噪声主导更新方向，系统性抹去跨输入差异，导致 I(X;Z)→0 干预方法（SNR-Aware Filtering）： 基于上述机制，论文提出在每轮迭代中： 计算每个prompt的组内奖励方差 Varˆ(R|X) 作为SNR代理 保留高方差（高信号）的prompts（Top-p筛选），过滤低方差更新 以此集中梯度预算于具有任务判别信息的样本，抑制输入无关的正则化主导。
## PRL-Bench: A Comprehensive Benchmark Evaluating LLMs' Capabilities in Frontier Physics Research【Physics】

[https://arxiv.org/pdf/2604.15411](https://arxiv.org/pdf/2604.15411)

- 论文提出了PRL-BENCH（Physics Research by LLMs），一个面向研究导向的综合性基准测试，其核心目标包括： 系统性映射能力边界：**评估LLM在端到端物理研究中的能力边界，包括理论推导与数值计算的整合** 复现真实研究特性：通过探索性任务制定、长程工作流和客观可验证性三大核心属性，重构真实物理研究的本质推理过程和研究工作流 覆盖前沿领域：**基于100篇经过领域专家验证的Physical Review Letters论文，涵盖天体物理、凝聚态物理、高能物理、量子信息、统计物理五大子领域 通过该基准，论文揭示了当前前沿模型在自主物理研究中的显著差距（最佳模型得分低于50分）**，特别是在领域知识掌握、推导稳定性、数值可靠性和长程任务适应方面的结构性局限。
## MathNet: a Global Multimodal Benchmark for Mathematical Reasoning and Retrieval【Math】

[https://arxiv.org/pdf/2604.18584](https://arxiv.org/pdf/2604.18584)

- 论文构建了**当前最大规模的高质量奥林匹克数学数据集 MathNet-Solve，包含 30,676 道题目及专家撰写解答，覆盖 47 个国家、17 种语言、143 项竞赛，时间跨度 40 年（1985–2025）**。数据源自官方国家奥林匹克小册子，区别于现有依赖社区论坛（如 AoPS）的数据集，确保了权威性与质量。生成模型局限：前沿 LLM/LMM 在复杂证明与几何推理上仍存显著瓶颈，且多模态输入对小型模型可能造成干扰 嵌入模型缺陷：通用语义嵌入无法捕捉数学结构的深层等价性，余弦相似度分布显示等价对与困难负样本难以区分 RAG 质量敏感性：只有当检索返回的上下文与目标问题存在结构共振（共享解题策略）而非仅表面相似时，检索增强才能有效提升推理能力。
## OMIBench: Benchmarking Olympiad-Level Multi-Image Reasoning in Large Vision-Language Model

[https://arxiv.org/pdf/2604.20806](https://arxiv.org/pdf/2604.20806)

- OMIBench基准测试 为填补上述空白，论文提出了OMIBench（Olympiad-level Multi-Image Benchmark），这是一个包含**超过1,000个跨生物学、化学、数学和物理的奥林匹克级别问题的基准**，其特点包括： **每个问题包含多个图像，共同提供多步推理所需的证据 需要自主的跨图像对齐、选择和整合推理**（autonomous cross-image alignment, selection, and integrative reasoning） 提供人工标注的推理路径（rationales），支持细粒度分析 实验结果表明，即使是**最先进的LVLMs（如Gemini-3-Pro）在该基准上也仅能达到约50%的准确率，相比单图像设置性能下降高达15-25%**，揭示了多图像奥林匹克级推理仍是当前模型的重大瓶颈。
## TEMPO: Scaling Test-time Training for Large Reasoning Models

[https://arxiv.org/pdf/2604.19295](https://arxiv.org/pdf/2604.19295)

- 论文通过期望最大化（EM）算法框架形式化分析指出，现有方法本质上是**不完全的EM变体**——仅执行策略优化的M步，而缺失了关键的E步（后验分布重新校准）。这导致证据下界（ELBO）逐渐松弛，优化目标与真实正确性分布之间的偏差随迭代累积。TEMPO通过引入交替式Actor-Critic架构解决上述问题： E步（Critic重校准）：周期性在标注数据集 DL 上更新critic，保持奖励信号与外部监督对齐 M步（策略优化）：在无标注测试集 Du 上基于critic信号优化策略 这种"间歇性接地"机制打破了自强化循环，确保ELBO始终紧致，从而实现测试时计算的持续边际收益。把 RLHF 里的 reward model，搬到了 test-time training 里，并且循环更新
## SFT-then-RL Outperforms Mixed-Policy Methods for LLM Reasoning

[https://arxiv.org/pdf/2604.23747](https://arxiv.org/pdf/2604.23747)

- 研究得出以下结论：**标准SFT-then-RL流程在数学推理基准测试上全面超越所有被评估的混合策略方法**（在Qwen2.5-Math-7B上平均提升+3.8分，在Llama-3.1-8B上提升+22.2分）即使仅使用50步RL的截断变体，在计算量更少的情况下仍优于混合策略方法混合策略方法声称的改进主要源于与受损基线的不公平比较，而非其算法设计的固有优势因此，**该研究恢复了学术界对标准SFT-then-RL范式的信心，并强调了跨框架验证对于防止系统性基线虚低的重要性**。

# Reward Model

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Model
- 方法：agent, language, vision-language-model, reasoning, vision, reinforcement-learning, optimization, vision-language
- 论文/报告：11 篇
- SUPERNOVA: Eliciting General Reasoning in LLMs with Reinforcement Learning on Natural Instructions
- Aligning Agents via Planning: A Benchmark for Trajectory-Level Reward Modeling
- ReflectRM: Boosting Generative Reward Models via Self-Reflection within a Unified Judgment Framework
- CLEAR: Context Augmentation from Contrastive Learning of Experience via Agentic Reflection【AWS】
- Self-Preference Bias in Rubric-Based Evaluation of Large Language Models【Rubrics】
- Visual Preference Optimization with Rubric Rewards【Rubrics】
- ConsistRM: Improving Generative Reward Models via Consistency-Aware Self-Training【Baidu】
- Self-Distilled RLVR
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## SUPERNOVA: Eliciting General Reasoning in LLMs with Reinforcement Learning on Natural Instructions

[https://arxiv.org/pdf/2604.08477](https://arxiv.org/pdf/2604.08477)

- **如何将RLVR从数学和代码等正式领域扩展到通用推理领域？**论文提出**SUPERNOVA**——一个**专门面向RLVR的数据整理框架**，**通过系统性地挖掘和改造自然指令数据，构建适用于通用推理的验证性训练数据**。论文通过100多项受控实验，深入分析了源任务选择、任务混合策略以及合成数据干预等关键设计决策对下游推理性能的影响，最终实现了在BBEH、Zebralogic、MMLU-Pro等挑战性基准上的显著提升（如在BBEH-test上最高取得52.8%的相对改进）。
## Aligning Agents via Planning: A Benchmark for Trajectory-Level Reward Modeling

[https://arxiv.org/pdf/2604.08178](https://arxiv.org/pdf/2604.08178)

- 首个专门针对**工具集成智能体（tool-integrated agents）****长程轨迹偏好判断**的综合性基准测试，旨在解决LLMs向自主智能体演进过程中奖励建模（Reward Modeling）评估范式的关键空白。
## ReflectRM: Boosting Generative Reward Models via Self-Reflection within a Unified Judgment Framework

[https://arxiv.org/pdf/2604.07506](https://arxiv.org/pdf/2604.07506)

- 论文提出了ReflectRM框架，核心创新包括： 统一判断框架：**将响应偏好建模（response preference）和分析偏好建模（analysis preference/self-reflection）统一为单一的生成式任务**，通过联合训练使两种能力相互强化。 自我反思机制：利用模型自身的自我反思能力评估分析过程的质量，在推理阶段通过两阶段策略（置信度引导的锚点选择+自我反思过滤）识别最可靠的分析轨迹，从而得出更稳健的偏好预测。 过程级偏好数据：通过构建Reflection Data（反射数据），将分析过程的比较转化为偏好对，为模型提供显式的过程级监督信号。
## CLEAR: Context Augmentation from Contrastive Learning of Experience via Agentic Reflection【AWS】

[https://arxiv.org/pdf/2604.07487](https://arxiv.org/pdf/2604.07487)

- 论文提出了**CLEAR（Contrastive Learning of Experience via Agentic Reflection）**框架，其核心创新在于：**** 生成式上下文增强：训练一个轻量级的上下文增强模型（CAM）**直接生成针对当前任务定制的辅助上下文，而非从知识库中检索固定指导** 对比学习与代理反射：利用反射代理对同一任务的多次执行轨迹（成功与失败）进行对比分析，提取高质量的任务特定策略 两阶段训练（SFT + RL）：先通过监督微调学习生成上下文，再通过强化学习（以执行代理的任务奖励为信号）优化上下文生成质量 **该方法的关键优势在于将适应负担从执行代理转移到CAM：CAM生成的是已经针对当前任务定制、可直接使用的上下文（c ），执行代理只需基于增强后的任务描述（q⊕c ）进行决策，无需额外推理如何将通用指导适应到具体任务。**
## Self-Preference Bias in Rubric-Based Evaluation of Large Language Models【Rubrics】

[https://arxiv.org/pdf/2604.06996](https://arxiv.org/pdf/2604.06996)

- LLM-as-a-judge已成为模型评估的主流范式，其中新兴的**RB方法**（通过二进制标准逐项评判输出，替代传统的成对比较PWC和直接评分DA）因具有更好的可解释性和粒度而备受关注。然而，自我偏好偏差（Self-Preference Bias, SPB）在RB中是否存在及其严重程度尚属未知。该偏差在自我改进递归循环中可能导致模型优化至"自我偏好"而非真实能力最优解。
## Visual Preference Optimization with Rubric Rewards【Rubrics】

[https://arxiv.org/pdf/2604.13029](https://arxiv.org/pdf/2604.13029)

- 论文提出 rDPO（Rubric-based Direct Preference Optimization）框架，通过以下方式解决上述问题： 实例特定的评分标准：为每个图像-指令对构建检查表式（checklist-style）的评分细则，区分"必要（essential）"与"附加（additional）"标准，实现可解释的多维度评估。 离线-在线解耦：离线构建可复用的指令-评分池（instruction-rubric pool），在线阶段针对目标策略采样响应并基于评分进行筛选，结合 on-policy 数据与细粒度标准反馈。 改进的偏好对挖掘：通过资格筛选（qualification）、边界约束（margin）和多样性控制（diversity）规则，构建高质量的偏好对，用于迭代 DPO 训练。 实验表明，该方法在奖励建模基准上使 30B-A3B 的开源评判模型接近 GPT-5.4 水平，在下游任务中相比基线模型和 outcome-based 过滤方法均有显著提升。
## ConsistRM: Improving Generative Reward Models via Consistency-Aware Self-Training【Baidu】

[https://arxiv.org/pdf/2604.07484](https://arxiv.org/pdf/2604.07484)

- 论文提出了ConsistRM框架，通过以下关键机制实现无需人工反馈的稳定自训练： 一致性感知答案奖励（Consistency-Aware Answer Reward, CAAR）：利用时间一致性（整合当前模型状态与历史记忆）构建可靠的伪标签，缓解奖励黑客问题并提供稳定的优化信号。 一致性感知批评奖励（Consistency-Aware Critique Reward, CACR）：通过衡量多个生成批评之间的语义一致性，提供细粒度的奖励信号，增强分析过程的稳定性。 简言之，该论文试图解决的核心问题是：如何在完全脱离人工标注的前提下，通过一致性感知的自训练机制，实现生成式奖励模型的稳定、可靠且可扩展的训练。
## Self-Distilled RLVR

[https://arxiv.org/pdf/2604.03128](https://arxiv.org/pdf/2604.03128)

- **不是简单强化正确轨迹，而是****把正确轨迹变成监督数据，用来自我蒸馏，从而把稀疏reward转化为密集学习信号****。**
## Co-Evolution of Policy and Internal Reward for Language Agents

[https://arxiv.org/pdf/2604.03098](https://arxiv.org/pdf/2604.03098)

- 论文提出Self-Guide框架，通过双重角色统一解决上述问题：** 推理时：智能体生成简短的语言自我评估（zt ），作为上下文信号指导下一步动作选择。 训练时：将同一信号转换为密集的步骤级内部奖励（rsgt ），与稀疏环境奖励结合，形成协同进化循环。** 该框架通过阶段性信任调度（stage-wise trust schedule）解决自举问题：先纯用环境奖励训练指导能力，再逐步激活内部奖励，最后退火以确保策略对齐真实目标。
## AgentV-RL: Scaling Reward Modeling with Agentic Verifier

[https://arxiv.org/pdf/2604.16004](https://arxiv.org/pdf/2604.16004)

- 论文提出 Agentic Verifier 框架，将奖励建模重新构想为多轮、工具增强的审议过程（multi-turn, tool-augmented deliberative process）。该框架通过以下机制实现全面、可靠且可解释的评估： 双向验证架构：引入互补的**前向代理（Forward Agent）（从前提追溯结论，进行充分性检查）和后向代理（Backward Agent）（从结论回溯前提，进行必要性检查）**，形成双向验证流程。 工具集成推理：**允许验证器迭代分解复杂解决方案，并调用外部工具**（如Python解释器）进行数值验证，实现"外部 grounding"。 自主探索能力：通过 AgentV-RL 训练配方（包含合成数据生成和两阶段训练：拒绝采样微调+强化学习），将多代理能力蒸馏到单一模型中，使验证器能够自主地交错工具使用与内部推理。
## When Can LLMs Learn to Reason with Weak Supervision?

[https://arxiv.org/pdf/2604.18574](https://arxiv.org/pdf/2604.18574)

- LLM如何通过RLVR实现泛化的条件。这项工作的核心发现是，泛化——模型将训练期间所学知识应用于新的、未见过问题的能力——受训练奖励饱和时机的影响。在强化学习训练期间，模型的奖励（其获得正确答案的频率）通常遵循一条曲线，该曲线从低开始并最终趋于平稳。研究人员确定了两个不同的阶段：**饱和前阶段**：训练奖励稳步增长的时期。在此阶段，模型通常会学习可迁移的推理模式，从而提高在保留基准上的性能。**饱和后阶段**：模型在训练集上达到最大奖励后的时期。在此阶段继续训练通常会导致收益递减甚至性能崩溃，因为模型可能正在“奖励作弊”或仅仅记忆特定的训练实例。该研究引入了一个指标 $t_{sat}$，定义为训练奖励达到其最大值99%的最早更新步长。泛化良好的模型往往具有较长的饱和前阶段（较大的 $t_{sat}$），从而使其能够吸收复杂的推理结构。令人惊讶的是，对于具有强大现有领域知识的模型（如Qwen2.5-Math在数学任务上），RLVR效率极高。即使只有 $N = 8$ 个训练样本，这些模型也显示出显著的提升。例如，Qwen2.5-Math-1.5B 在 MATH-500 基准测试中，仅用8个样本就实现了与使用 $N = 2048$ 个样本相当的性能提升。相比之下，没有这些先验知识的模型（如Llama-3.2-Instruct在数学上）饱和速度极快（$t_{sat} < 100$），并且无法从小数据集中泛化。

# Foundation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Foundation
- 方法：language, vision-language-model, reasoning, vision, reinforcement-learning, optimization, vision-language, deep-learning
- 论文/报告：8 篇
- ASI-Evolve: AI Accelerates AI【Pengfei Liu】
- Loop, Think, & Generalize: Implicit Reasoning in Recurrent-Depth Transformers
- Nexus: Same Pretraining Loss, Better Downstream Generalization via Common Minima【ByteDance Seed】
- A Layer-wise Analysis of Supervised Fine-Tuning
- Back to the Barn with LLAMAs: Evolving Pretrained LLM Backbones in Finetuning Vision Language Models
- Evolution of Optimization Methods: Algorithms, Scenarios, and Evaluations
- From  P(y|x) to  P(y) : Investigating Reinforcement Learning in Pre-train Space
- ⭐Image Generators are Generalist Vision Learners【Google DeepMind】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## ASI-Evolve: AI Accelerates AI【Pengfei Liu】

[https://arxiv.org/pdf/2603.29640](https://arxiv.org/pdf/2603.29640)

- **AI 能否自主地、系统地加速其自身的发展（AI-for-AI）？**论文提出 **ASI-Evolve** 框架，通过引入****结构化认知库（Cognition Base）注入人类先验知识，以及专用分析器（Analyzer）**将复杂实验结果蒸馏为可复用的结构化见解**，首次在统一的闭环系统中实现了 **AI 对数据、架构和算法三大核心组件的自主发现与优化**。
## Loop, Think, & Generalize: Implicit Reasoning in Recurrent-Depth Transformers

[https://arxiv.org/pdf/2604.07822](https://arxiv.org/pdf/2604.07822)

- **循环深度Transformer能够实现系统性泛化，而普通Transformer完全失败**。这种能力通过三阶段的"grokking"过程涌现：从记忆化（memorization）到分布内泛化（in-distribution generalization），最终达到系统性泛化。 通过扩展推理时的循环次数（inference-time recurrence），模型能够解锁对更深推理深度的泛化能力，实现深度外推。 同时论文也识别出关键局限**"过度思考"（overthinking）**：当循环次数过多时，模型性能会下降，限制了对极深组合的泛化能力。
## Nexus: Same Pretraining Loss, Better Downstream Generalization via Common Minima【ByteDance Seed】

[https://arxiv.org/pdf/2604.09258](https://arxiv.org/pdf/2604.09258)

- 传统观点（隐含假设）是： 预训练 loss 越低 → 模型越好 但作者指出一个关键现象： **不同训练过程（比如不同 optimizer） 可以达到几乎相同的 pretraining loss 但 downstream 表现（推理、泛化）差很多**。核心思路 👉 让不同任务的梯度方向更一致 具体做法： **最大化 gradient similarity 鼓励不同数据源更新方向一致** 论文原话本质是： 通过优化过程让 minima 更“靠近”。
## A Layer-wise Analysis of Supervised Fine-Tuning

[https://arxiv.org/pdf/2604.11838](https://arxiv.org/pdf/2604.11838)

- 现有参数高效微调方法（如 LoRA）默认均匀更新所有层，隐含假设各层对对齐的贡献等同，忽略了潜在的深度依赖异质性。该研究试图回答：**任务适应在模型深度的何处发生？哪些层对指令跟随至关重要？****通过跨尺度（1B-32B）实验，研究揭示了一致的"三阶段"深度依赖规律： ****表征 Diverge 模式：Base 与 SFT 模型间的 CKA 相似度在浅层（0-60%深度）保持高位（>0.98），在末层（末20%）急剧跌落至 ~0.94****；余弦相似度与平均偏移 D(l)SFT=||μ(l)s−μ(l)b||2 显示末层发生指数级位移（>12.0） 内部表征演化：****Base 与 SFT 模型各自在末层出现信息瓶颈****——有效秩从峰值（~0.05）崩溃至 <0.01，谱范数（Spectral Norm）飙升至 >500，表明深层强制压缩特征以驱动输出分布 参数更新强度：权重变化呈现J型轨迹——****早期层更新微弱（~0.05），从中点单调攀升，末层达峰值（>0.10）****，且与余弦相似度呈强负相关（r=−0.79 ）**
## Back to the Barn with LLAMAs: Evolving Pretrained LLM Backbones in Finetuning Vision Language Models

[https://arxiv.org/pdf/2604.10985](https://arxiv.org/pdf/2604.10985)

- **预训练大语言模型（LLM）主干网络的演进如何影响下游视觉-语言模型（VLM）任务性能**这一核心问题。尽管新LLM（如LLaMA-1→LLaMA-2→LLaMA-3）在推理能力、指令遵循和泛化性上持续改进，但这些改进如何具体转化为VLM的多模态推理、模态对齐和任务性能，尚缺乏**控制变量下的系统性实证研究**。
## Evolution of Optimization Methods: Algorithms, Scenarios, and Evaluations

[https://arxiv.org/pdf/2604.12968](https://arxiv.org/pdf/2604.12968)

- 针对现代大规模模型训练中的可行性危机（内存墙、通信墙、隐私墙）以及领域知识碎片化问题，提出了统一理论框架、全景分类体系和标准化评估基准三大核心贡献。论文提出广义约束优化动态系统，将任意优化器解耦为四个正交算子： **梯度估计器 E **：统一描述FO梯度、ZO有限差分、SO曲率近似 预**处理器 M−1t **：涵盖对角自适应（Adam）、矩阵正交化（Muon）、Hessian近似（AdaHessian）** 场景感知变换 Tscenario **：显式建模分布式压缩、差分隐私噪声注入、梯度裁剪等约束 **结构投影 PΘ **：描述低秩子空间（LOZO）、流形约束等硬件感知限制 该框架揭示了不同范式间的结构等价性（如内存高效ZO与FO单侧预处理在数学上的同构），为算法重组提供理论基础。
## From  P(y|x) to  P(y) : Investigating Reinforcement Learning in Pre-train Space

[https://arxiv.org/pdf/2604.14142](https://arxiv.org/pdf/2604.14142)

- 现有基于可验证奖励的强化学习（RLVR）通过优化条件分布 P(y|x) 提升推理能力，但其能力边界受限于基础模型固有的输出分布。**预训练空间优化（直接优化边际分布 P(y) ）理论上可编码更强推理基础**，但传统预训练依赖静态语料库的被动学习，导致与下游任务分布偏移。本文探索将在线强化学习引入预训练空间，通过奖励驱动的主动学习突破上述局限。本质上就是预训练优质答案.
## ⭐Image Generators are Generalist Vision Learners【Google DeepMind】

[https://arxiv.org/pdf/2604.20329](https://arxiv.org/pdf/2604.20329)

- **验证图像生成模型是否具备通用视觉理解能力（generalist vision understanding capabilities），即****图像生成预训练是否能产生适用于多种视觉任务的强大且通用的视觉表征？构建了一个名为 Vision Banana 的通用模型，验证其是否能在以下任务上达到最先进水平（SOTA）： 2D理解：语义分割、实例分割、指代分割（referring expression segmentation） 3D理解：单目度量深度估计、表面法线估计 视觉生成：文本到图像生成、图像编辑。论文探索了将图像生成作为视觉任务统一接口的可行性——将所有视觉任务输出参数化为RGB图像，类似于文本生成在自然语言处理中的统一作用。这种方法理论上能自然处理视觉任务中的歧义性（ambiguity），避免判别式模型中常见的模式平均（mode-averaging）问题。 简言之，该论文试图证明：计算机视觉领域可能正在经历一场范式转变，生成式视觉预训练（generative vision pretraining）有望成为构建基础视觉模型的核心方法，同时服务于视觉生成和理解。**

# Data

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data
- 方法：agent, generation, language, optimization, gui-agent
- 论文/报告：8 篇
- ⭐Optimsyn: Influence-Guided Rubrics Optimization for Synthetic Data Generation【Ant】
- BenchScope: How Many Independent Signals Does Your Benchmark Provide?
- Xpertbench: Expert Level Tasks with Rubrics-Based Evaluation【ByteDance Seed】
- ⭐Rethinking Data Mixing from the Perspective of Large Language Models【Data Mixing】
- ⭐Optimsyn: Influence-Guided Rubrics Optimization for Synthetic Data Generation【Data Generation】
- Tracing the Roots: A Multi-Agent Framework for Uncovering Data Lineage in Post-Training LLMs【AI Lab】
- MetaGAI: A Large-Scale and High-Quality Benchmark for Generative AI Model and Data Card Generation
- ViPO: Visual Preference Optimization at Scale
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## ⭐Optimsyn: Influence-Guided Rubrics Optimization for Synthetic Data Generation【Ant】

[https://arxiv.org/pdf/2604.00536](https://arxiv.org/pdf/2604.00536)

- 试图解决**知识密集型领域（如人文社科、医学、法律、金融）中高质量监督微调（SFT）数据稀缺**的问题，并针对现有合成数据生成范式的关键局限性提出系统性解决方案。论文提出**将合成数据质量评估建立在目标模型的训练效用之上，通过基于梯度的影响估计器（optimizer-aware influence estimator）量化每个合成样本对验证集目标的贡献，并以此作为奖励信号，通过强化学习（GRPO）优化一个可学习的评分标准生成器（prompter）**，从而将评分标准设计从" brittle, expert-crafted heuristics"转化为"portable, model-aligned optimization problem"。论文建立了一个端到端的优化闭环： Warm-up阶段：使用初始prompter生成少量数据，微调目标模型获取参考梯度（计算影响分数所需的历史动量统计 m,v ） RL训练阶段： 对每个种子文档采样 G 个候选评分标准 Generator并行合成 G 个QA对 计算每个样本的影响分数作为奖励 更新prompter以最大化期望下游性能提升 最终SFT阶段：使用优化后的prompter生成大规模合成数据，训练目标模型 跨领域迁移机制： 不同于依赖领域专家编写固定规则，该方法仅需轻量级引导文本（如"你是一位{domain}专家"），通过强化学习自动发现该领域最有效的质量维度（如图6所示，优化后的评分标准从模糊指令如"focus, align"演变为具体可操作的标准如"critical, clarity, completeness, parsability"）。
## BenchScope: How Many Independent Signals Does Your Benchmark Provide?

[https://arxiv.org/pdf/2603.29357](https://arxiv.org/pdf/2603.29357)

- 论文引入**Effective Dimensionality (ED)**——基于参与比（participation ratio）的 closed-form 统计量，作为**群体条件性（population-conditional）**的测量广度上界。通过任务中心化后的奇异值谱计算，与 Rényi 熵和随机矩阵理论（Marchenko–Pastur 律）建立理论基础，无需阈值、置换或迭代拟合，仅需分数矩阵即可计算 可标记冗余套件组件、监测性能条件性压缩（performance-conditional compression），并指导基准维护 论文通过对22个跨领域基准（涵盖8个评估域、8,400+模型评估）的实证分析，构建了首个跨基准冗余图谱，揭示当前评估生态系统中独立测量容量的稀缺性——即使是最广泛的基准，其真实有效维度也远低于原始分数数量所暗示的广度。
## Xpertbench: Expert Level Tasks with Rubrics-Based Evaluation【ByteDance Seed】

[https://arxiv.org/pdf/2604.02368](https://arxiv.org/pdf/2604.02368)

- 评估LLMs在真实专家级任务中表现的高保真基准，以及配套的 **ShotJudge** 评估范式。论文构建了一个多领域专家级评估基准： 规模与覆盖：包含 1,346个任务，横跨 80个类别，覆盖7个高价值专业领域（金融18.1%、教育24.4%、法律16.0%、工程与应用科学20.4%、人文社科8.6%、计算机科学6.8%、医疗5.6%） 生态效度：任务直接来源于 1,000+名领域专家（顶尖高校研究人员、CFA/CPA/医师/法律资格持有者）的真实工作流，而非学术代理 任务特性：聚焦开放式、长周期任务（open-ended, long-horizon tasks），要求模型进行战略推理、文献综合与专业判断，区别于静态知识回忆 专家资质：通过两阶段认证（领域考试+试标注审核），确保专家具备≥3年实践经验及顶尖学术/职业资质。为解决可扩展性与专业严谨性的矛盾，论文提出ShotJudge： 专家锚定：以GPT-5为基线，由领域专家提供带详细理由的评分作为"金标准"（gold-standard） 单样本校准：LLM评判器（Gemini 2.5 Pro）接收任务提示、评分标准及专家验证的基线响应作为单样本示例，学习专家推理模式。
## ⭐Rethinking Data Mixing from the Perspective of Large Language Models【Data Mixing】

[https://arxiv.org/pdf/2604.07963](https://arxiv.org/pdf/2604.07963)

- 适应数据调度框架（DoGraph）论文提出将数据调度形式化为图约束优化问题：**将模型感知到的领域视为图节点通过随机投影和K-means聚类在梯度空间中动态识别领域划分优化领域权重以平衡学习进程该方法避免了人工领域标注的偏差，并能随训练动态调整**，在多个基准测试上实现了更优的领域平衡和泛化性能。
## ⭐Optimsyn: Influence-Guided Rubrics Optimization for Synthetic Data Generation【Data Generation】

[https://arxiv.org/pdf/2604.00536](https://arxiv.org/pdf/2604.00536)

- **不是让数据“看起来好”，而是让数据“训练起来有用”。把“数据质量”从主观 → 客观 以前： 人写prompt 靠经验 现在： 用 gradient / influence 直接优化训练效果。把“数据生成”变成一个RL问题 虽然论文没完全用RL形式，但本质就是：  policy = rubric； reward = influence on model。**
## Tracing the Roots: A Multi-Agent Framework for Uncovering Data Lineage in Post-Training LLMs【AI Lab】

[https://arxiv.org/pdf/2604.10480](https://arxiv.org/pdf/2604.10480)

- 论文提出： 将**数据谱系（data lineage）**概念引入LLM生态系统，形式化为有向图 G=(V,E) ，其中**节点代表数据集，边表示继承依赖**； 构建一个多智能体协作框架，通过多源证据融合与语义推理，从非结构化文档中自动提取结构化谱系； 基于重建的谱系图进行拓扑分析，以诊断冗余与污染，并指导构建更具多样性的训练语料。 简言之，该工作旨在通过自动化的谱系重建，将后训练数据构建从孤立的、不可控的实践转变为系统化、可解释且可审计的范式。
## MetaGAI: A Large-Scale and High-Quality Benchmark for Generative AI Model and Data Card Generation

[https://arxiv.org/pdf/2604.23539](https://arxiv.org/pdf/2604.23539)

- 随着Hugging Face等平台托管超200万个模型，手动创建模型卡（Model Cards）与数据卡（Data Cards）面临可扩展性危机，存在普遍的不完整、不一致与主观性问题。现有自动化方法缺乏大规模、经多源验证的基准，难以客观评估生成质量。
## ViPO: Visual Preference Optimization at Scale

[https://arxiv.org/pdf/2604.24953](https://arxiv.org/pdf/2604.24953)

- 解决**视觉生成模型（Visual Generative Models）中偏好优化（Preference Optimization）范式的有效扩展问题。论文提出了双重解决方案： Poly-DPO：通过****引入多项式扩展项动态调整样本权重，增强对噪声数据的鲁棒性，同时在高质量数据上自适应退化为标准 DPO****； ViPO 数据集：构建包含 100 万图像对（1024px）和 30 万视频对（720p+）的大规模高质量数据集，通过系统化分类和先进生成模型确保偏好信号的一致性与平衡性。**

# Generation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Generation
- 方法：agent, generation, econ-em
- 论文/报告：8 篇
- ImagenWorld: Stress-Testing Image Generation Models with Explainable Human Evaluation on Open-ended Real-World Tasks【Wenhu Chen】
- A Frame is Worth One Token: Efficient Generative World Modeling with Delta Tokens【Amazon】
- Seedance 2.0: Advancing Video Generation for World Complexity【ByteDance Seed】
- HY-World 2.0: A Multi-Modal World Model for Reconstructing, Generating, and Simulating 3D Worlds【World Model, Tencent Hunyuan】
- WorldMark: A Unified Benchmark Suite for Interactive Video World Models【World Model】
- Agentic World Modeling: Foundations, Capabilities, Laws, and Beyond【World Model】
- 📌Wan-Image: Pushing the Boundaries of Generative Visual Intelligence【Wan】
- VibeToken: Scaling 1D Image Tokenizers and Autoregressive Models for Dynamic Resolution Generations
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## ImagenWorld: Stress-Testing Image Generation Models with Explainable Human Evaluation on Open-ended Real-World Tasks【Wenhu Chen】

[https://arxiv.org/pdf/2603.27862](https://arxiv.org/pdf/2603.27862)

- 基于对 14 个模型（含 GPT-Image-1、Gemini 2.0 Flash、BAGEL、OmniGen2 等统一模型及专用基线）的大规模评估，论文揭示： 任务难度差异：编辑任务显著难于生成任务（平均差距 ≈0.1 ），模型在局部修改中常陷入两种失效模式——完全重新生成（up to 17% ）或返回原图不变； 领域性能鸿沟：所有模型在 Artworks（0.78 ）与 Photorealistic Images（0.82 ）表现优异，但在 Screenshots（0.55 ）与 Information Graphics（0.58 ）中因文本渲染与符号理解不足而失败； 数据策划效应：Qwen-Image 凭借针对文本-heavy 数据的合成数据 pipeline，在 Textual Graphics 上超越闭源模型，证明针对性数据策划可弥补架构差距； VLM 评估边界：现代 VLM（Gemini-2.5-Flash）作为评判者时，Kendall accuracy 达 0.79 ，接近人类一致性（0.76 ），适用于相对排序，但在 Artifact 检测上存在系统性低估（Bias =+0.06 ），细粒度错误归因仍需人工介入。
## A Frame is Worth One Token: Efficient Generative World Modeling with Delta Tokens【Amazon】

[https://arxiv.org/pdf/2604.04913](https://arxiv.org/pdf/2604.04913)

- 论文提出DeltaTok与DeltaWorld，通过以下方式突破上述局限： Delta token表示：将连续帧在视觉基础模型（VFM）特征空间的差异压缩为单个连续token（而非完整空间特征图），利用帧间变化的低维结构，将视频从三维时空表示降维为一维时间序列（实现1024× token缩减）； 单次前向生成：结合Best-of-Many目标函数，使轻量级Transformer在单次前向传播中并行生成多个合理未来，避免迭代去噪； VFM特征空间建模：在DINO等预训练视觉特征空间中预测，剥离无关像素细节，专注语义动态。 最终实现在参数减少35倍、FLOPs减少2000倍的同时，生成质量优于现有生成式基线，且多样本平均预测质量与判别式模型相当。
## Seedance 2.0: Advancing Video Generation for World Complexity【ByteDance Seed】

[https://arxiv.org/pdf/2604.14148](https://arxiv.org/pdf/2604.14148)

- 关键技术特性物理感知生成：严格遵循真实世界运动规律，缓解结构失真和视觉伪影多模态控制：支持主题控制、运动操纵、风格迁移、特效设计、视频扩展等独立及组合任务专业级输出：原生支持480p/720p分辨率，4-15秒直接生成，具备导演级摄影语言理解和叙事节奏控制能力。
## HY-World 2.0: A Multi-Modal World Model for Reconstructing, Generating, and Simulating 3D Worlds【World Model, Tencent Hunyuan】

[https://arxiv.org/pdf/2604.14268](https://arxiv.org/pdf/2604.14268)

- 解决**3D世界建模中生成与重建任务分离、缺乏统一多模态框架**的核心问题。HY-World 2.0通过构建一个能够根据输入模态动态调整行为的**多模态世界模型**，解决了从"想象"（生成）到"观测"（重建）的3D世界建模统一难题。
## WorldMark: A Unified Benchmark Suite for Interactive Video World Models【World Model】

[https://arxiv.org/pdf/2604.21686](https://arxiv.org/pdf/2604.21686)

- 论文提出了 **WorldMark**——首个专为交互式图像到视频（I2V）世界模型设计的标准化基准测试套件。该套件通过建立**统一的动作映射层（将共享的 WASD+L/R 动作词汇转换为各模型的原生控制格式）、提供标准化的 500 例测试用例（覆盖不同视角、风格和难度层级），以及构建模块化的评估工具包**，为异构模型提供了共同的评估平台，从而使得跨模型的公平比较成为可能。对六种模型（YUME 1.5、Matrix-Game 2.0、HY-World 1.5、HY-GameCraft、Open-Oasis、Genie 3）的系统评估揭示： 视觉质量与世界一致性弱相关：YUME 1.5 生成最具美学吸引力的帧但世界逻辑一致性差；Genie 3 保持最一致的世界但帧级保真度仅中等 控制对齐 ≠ 整体质量：HY-GameCraft 精确执行命令但视觉保真度代价高；Genie 3 轨迹误差较高但保持全局世界连贯 第三人称生成是显著失效模式：视角切换导致控制精度严重下降，Matrix-Game 2.0 的旋转误差从第一人称的约 1.3∘ 激增至 27.6∘ （恶化近 20 倍） 领域训练无法迁移：Open-Oasis（Minecraft 训练）在真实世界和风格化场景的所有指标上均失败。
## Agentic World Modeling: Foundations, Capabilities, Laws, and Beyond【World Model】

[https://arxiv.org/pdf/2604.22748](https://arxiv.org/pdf/2604.22748)

- "世界模型"这一术语在不同研究社区中承载着截然不同的技术内涵：强化学习社区：将其视为学习状态转移结构以支持规划与决策的转移模型；计算机视觉社区：将其视为维持视觉动力学与时间一致性的视频/3D生成器；语言建模与智能体社区：将其视为支持规划、网页交互与社会环境模拟的文本化模拟器；机器人学与科学发现领域：分别关注物理动力学建模与假设驱动的实验验证。论文提出了一个**"能力×法则"的双轴分类框架**：纵向能力轴(L1/L2/L3)：定义三级递进能力——L1预测器(单步局部转移)、L2模拟器(多步动作条件推演)、L3进化器(基于证据的模型自我修正)；横向法则轴(四领域)：识别四种支配性法则体系——物理世界(几何与守恒定律)、数字世界(程序语义与状态机)、社会世界(信念与规范)、科学世界(潜在因果机制与实验验证)。
## 📌Wan-Image: Pushing the Boundaries of Generative Visual Intelligence【Wan】

[https://arxiv.org/pdf/2604.19858](https://arxiv.org/pdf/2604.19858)

- Wan-Image采用端到端统一Transformer架构，整合多模态理解与生成： Planner（MLLM-based）：负责任务规划与语义推理，通过Chain-of-Thought (CoT) 机制解析用户意图，生成结构化视觉指令（<imagine>标签包裹），并自动推断输出分辨率（<size>grid_h*grid_w</size>）。 Visualizer（DiT-based）：基于整流流（Rectified Flow）训练，采用4通道VAE（RGB+Alpha）实现高保真像素合成，支持原生透明背景生成。 注意力机制：文本/ViT/VAE token间采用因果注意力，图像系列内启用跨图像双向注意力以确保一致性；编辑任务引入Teacher Forcing（干净VAE token时间步置零）保留输入结构。 位置编码：理解分支使用MRoPE，生成分支使用3D-RoPE（T /H /W 维度解耦），通过固定偏移量实现跨分支对齐。
## VibeToken: Scaling 1D Image Tokenizers and Autoregressive Models for Dynamic Resolution Generations

[https://arxiv.org/pdf/2604.24885](https://arxiv.org/pdf/2604.24885)

- 论文提出VibeToken（分辨率无关的1D tokenizer）和VibeToken-Gen（AR生成器），通过以下方式解决上述问题： **将任意分辨率图像编码为固定且可控的短序列（32–256 tokens）** 实现恒定投推理计算开销（179 GFLOPs，与分辨率无关，比基线高效63倍） 支持原生超分辨率和任意长宽比，无需针对特定分辨率重新训练。

# Agent Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Training
- 方法：agent
- 论文/报告：1 篇
- Agent^2 RL-Bench: Can LLM Agents Engineer Agentic RL Post-Training?
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Agent^2 RL-Bench: Can LLM Agents Engineer Agentic RL Post-Training?

[https://arxiv.org/pdf/2604.10547](https://arxiv.org/pdf/2604.10547)

- 提出 **Agent2 RL-Bench**，论文建立了一个包含**六个任务（涵盖数学推理、代码生成、评判优化、具身交互、网络购物和深度搜索问答）**的基准，首次实现了对智能体驱动的后训练行为的自动化诊断，包括运行时仪表记录和事后结构化分析报告生成。Agent2 RL-Bench 包含六个任务，按结构复杂度分为三层： L1（静态规则）：GSM8K与HumanEval，使用确定性验证器，测试基础数据构建与监督训练。 L2（静态评判）：AlpacaEval 2.0，引入基于LLM评判者的非确定性奖励，测试优化不可编程信号的能力。 L3（交互式长程）：ALFWorld（稀疏奖励具身交互）、WebShop（密集奖励网络购物）、DeepSearchQA（工具增强搜索-评判循环），要求智能体实现环境交互、多步轨迹收集、轨迹级奖励处理与持续在线数据生成。

# Agent Applications

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Applications
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, science-discovery, vision
- 论文/报告：87 篇
- HippoCamp: Benchmarking Contextual Agents on Personal Computers
- Proactive Agent Research Environment: Simulating Active Users to Evaluate Proactive Assistants【Research, Apple】
- AIBench: Evaluating Visual-Logical Consistency in Academic Illustration Generation【Research, Tongyi】
- Towards a Medical AI Scientist【Research】
- ⭐OmniMem: Autoresearch-Guided Discovery of Lifelong Multimodal Agent Memory【Research, Memory】
- EigentSearch-Q+: Enhancing Deep Research Agents with Structured Reasoning Tools【Research, Meta】
- Towards Knowledgeable Deep Research: Framework and Benchmark【Research】
- ⭐PaperOrchestra: A Multi-Agent Framework for Automated AI Research Paper Writing【Research】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## HippoCamp: Benchmarking Contextual Agents on Personal Computers

[https://arxiv.org/pdf/2604.01221](https://arxiv.org/pdf/2604.01221)

- **现有基准测试在评估智能代理于****真实个人计算环境****中的表现时存在的显著缺口**。即使最先进的商业模型（ChatGPT Agent Mode）在**用户画像任务上准确率仅48.3%**，事实保留任务62.8%，揭示当前代理在个性化环境中的显著局限。
## Proactive Agent Research Environment: Simulating Active Users to Evaluate Proactive Assistants【Research, Apple】

[https://arxiv.org/pdf/2604.00842](https://arxiv.org/pdf/2604.00842)

- 解决**主动智能体（Proactive Agents）的评估难题**，具体表现为现有评估框架无法真实模拟用户与数字环境的交互特性。论文提出了**Proactive Agent Research Environment (Pare)** 框架，通过构建基于有限状态机（FSM）的非对称环境——其中用户代理需遵循状态依赖的界面导航逻辑，而助手代理可直接访问API——实现了对主动用户的真实模拟，从而支持对上下文观察、目标推断、干预时机和多应用协调等关键能力的系统评估。对7个LLM（Claude 4.5 Sonnet、GPT-5、Gemini 3 Pro/Flash、Qwen 3 4B、Llama 3.2 3B、Gemma 3 4B）的评估显示： 性能瓶颈：即使是最佳模型（Gemini 3 Flash与Claude 4.5 Sonnet），成功率仅约42%，表明主动辅助仍具挑战性； 一致性差距：小模型（如Llama 3.2 3B）的跨运行一致性极差（Success@4为23.8%，Success4 仅1.4%），而大模型差距约3倍； 执行瓶颈：对于小模型，任务执行而非目标推断是主要瓶颈（Observe-Execute架构可部分缓解）； 干预时机：Claude以最低提案率（12.8%）实现最高接受率（78.2%），而Gemma 3 4B有74.7%的提案因过早触发导致用户进入"信息收集"状态； 鲁棒性差异：在40%工具故障率或6事件/分钟噪声下，Claude保持稳定，而Gemini Flash与GPT-5显著下降。
## AIBench: Evaluating Visual-Logical Consistency in Academic Illustration Generation【Research, Tongyi】

[https://arxiv.org/pdf/2603.28068](https://arxiv.org/pdf/2603.28068)

- 论文提出AIBench，一个基于**视觉问答（VQA）**的细粒度评估基准：逻辑评估：通过构建从方法文本提取的逻辑有向图，**生成四个层次（组件存在、局部拓扑、阶段架构、全局语义）的多选题QA对**，**将复杂的逻辑一致性检验分解为可验证的客观问题**；美学评估：采用专门的美学评估模型（UniPercept）独立评估视觉质量；解耦评估：明确分离客观逻辑正确性与主观美学质量，避免"指标模糊"问题通过这种方式，AIBench实现了对学术插图生成模型的稳定、可复现、可解释的评估，揭示了当前模型在长文本复杂推理和高密度内容生成方面的能力差距。
## Towards a Medical AI Scientist【Research】

[https://arxiv.org/pdf/2603.28589](https://arxiv.org/pdf/2603.28589)

- 论文提出了 **Medical AI Scientist** ——首个专为临床自主研究设计的端到端框架，通过**临床医生-工程师协同推理机制**（clinician-engineer co-reasoning）确保研究想法基于可验证的医学证据，并针对医学数据的特殊性构建了可靠的实验执行流水线，同时嵌入伦理审查机制以符合医学出版政策。
## ⭐OmniMem: Autoresearch-Guided Discovery of Lifelong Multimodal Agent Memory【Research, Memory】

[https://arxiv.org/pdf/2604.01007](https://arxiv.org/pdf/2604.01007)

- **AI智能体在长期运行过程中缺乏有效的终身多模态记忆能力。**论文提出通过部署自主研究管道（AUTORESEARCHCLAW）来**系统性发现最优记忆架构**。该方法从朴素基线（F1 = 0.117）出发，**自主执行约50个实验，在不需人工干预的情况下诊断故障模式、修复数据管道错误、提出架构修改**，最终发现OMNIMEM——**一个统一的多模态记忆框架**，在LoCoMo和Mem-Gallery基准上分别实现+411%和+214%的F1提升，达到当前最优性能。 关键突破在于证明：最具影响力的发现（如错误修复+175%、架构变更+44%、提示工程+188%）超越了传统AutoML的能力范围，需要自主代码生成、理解和跨组件推理能力。1. 选择性摄入（Selective Ingestion） 新颖性过滤：**轻量级感知编码器（CLIP用于视觉、VAD用于音频、Jaccard用于文本）在存储前丢弃冗余内容** 多模态原子单元（MAU）：统一表示 M=⟨s,e,p,τ,m,ℓ⟩ 解耦轻量级元数据（摘要s 、嵌入e 、时间戳τ ）与重量级原始数据（指针p ），实现热存储（快速检索）与冷存储（懒加载）的两层设计 2. 渐进式检索与混合搜索（Progressive Retrieval） **混合搜索：密集检索（FAISS）与稀疏检索（BM25）通过集合并集合并**（而非分数重排序）结合：R(q)=D(q)∪(K(q)∖D(q)) 该策略由管道自主发现，可避免语义排序被破坏 金字塔机制：三级确定性扩展（L1摘要→L2完整文本→L3原始内容），由token预算B 和相似度阈值θ 控制，避免LLM判断带来的延迟 3. 知识图谱增强（Structured Knowledge） **维护知识图 G=(V,E) 捕捉跨MAU的实体关系**。通过混合相似度（余弦+Jaro-Winkler）解析实体别名，查询时执行h 跳邻域扩展，采用距离衰减评分 rG(v)=βd(v,Vq)⋅conf(v) 融合图证据与向量检索结果。
## EigentSearch-Q+: Enhancing Deep Research Agents with Structured Reasoning Tools【Research, Meta】

[https://arxiv.org/pdf/2604.07927](https://arxiv.org/pdf/2604.07927)

- Q+ 结构化推理工具集 受 Anthropic “think” 工具范式及经典信息检索（IR）理论启发，论文提出 Q+——一套通过工具调用显式化认知过程的认知支架（cognitive scaffolding）。其核心特征为： 非侵入性：仅记录推理轨迹（trace-only），不直接检索外部信息，以类型化参数强制模型显式记录中间决策 状态可审计：将推理输入与预期输出结构化，使中间决策可被检查与干预。**将查询处理与证据处理操作外化为显式、可检查的工具**，能够显著提升深度研究智能体的**鲁棒性**与**可解释性**。
## Towards Knowledgeable Deep Research: Framework and Benchmark【Research】

[https://arxiv.org/pdf/2604.07720](https://arxiv.org/pdf/2604.07720)

- 提出了**混合知识分析框架（Hybrid Knowledge Analysis framework, HKA）**，通过多智能体架构（包括规划器、结构化知识分析器、非结构化知识分析器和撰写器）实现对结构化和非结构化知识的联合推理，并生成综合性的多模态研究报告。同时，论文构建了**KDR-Bench**基准测试，涵盖9个领域的41个专家级问题及1,252个结构化知识资源，用于系统评估代理在知识利用、定量分析和多模态报告生成方面的能力。
## ⭐PaperOrchestra: A Multi-Agent Framework for Automated AI Research Paper Writing【Research】

[https://arxiv.org/pdf/2604.05018](https://arxiv.org/pdf/2604.05018)

- 解决将非结构化预写作材料自主转化为严谨、可提交的研究论文手稿这一关键挑战。 具体而言，论文识别出现有自动化写作系统存在的以下核心局限： 功能单一性：现有文献综述生成框架（如 AutoSurvey2、LiRA）仅能生成综述性文本，**缺乏将原始实验数据转化为完整研究论文的能力；而全生命周期自主研究代理（如 AI Scientist-v2、Cycle Researcher）则与其内部实验管道紧密耦合，无法作为独立写作工具处理人类提供的非结构化材料**。 文献综述质量不足：现有系统依赖简单的关键词检索，产生引用不足的肤浅综述，缺乏对特定研究差距的针对性对比和批判性分析。 视觉内容生成局限：现有方法缺乏生成概念科学图表的能力，仅限于代码生成的数据图表，无法全面支撑论文的叙事需求。 评估基准缺失：领域缺乏标准化基准来独立评估自动化写作性能。 为应对上述挑战，论文提出 PaperOrchestra 框架，其核心目标是**通过多智能体协作，将不受约束的预写作材料（包括想法摘要、实验日志、可选图表）转化为包含深度文献综述和生成式可视化内容的、符合会议要求的 LaTeX 提交稿件。**
## PaperScope: A Multi-Modal Multi-Document Benchmark for Agentic Deep Research Across Massive Scientific Papers【Research】

[https://arxiv.org/pdf/2604.11307](https://arxiv.org/pdf/2604.11307)

- **PaperScope**——一个基于知识图构建的**多模态、多文档基准测试**，涵盖2,000多篇AI领域论文，通过随机游走采样构建主题连贯的文档集合，并设计了涵盖主题归纳、推理、摘要和求解的2,000多个问答对，用于严格评估Agentic深度研究系统在长上下文检索和深度多源推理方面的能力。
## 📌AI-Assisted Peer Review at Scale: The AAAI-26 AI Review Pilot【Research】

[https://arxiv.org/pdf/2604.13940](https://arxiv.org/pdf/2604.13940)

- 探索**AI能否在现实会议规模上生成技术上可靠且实用的评审**以辅助人类审稿流程。具体而言，研究试图验证： AI是否能在不到24小时内为超过22,000篇论文生成高质量评审（操作可行性） AI评审在技术准确性、故事叙述、评估方法、正确性和重要性等维度上是否能达到或超越人类评审水平（技术质量） 学术界（作者、程序委员会成员）对AI辅助评审的接受度和偏好（实用价值） 解决方案概述 为回答上述问题，论文报告了AAAI-26 AI评审试点项目——**首次在大型AI会议（AAAI-26）主轨道的全部22,977篇进入完整评审阶段的稿件上，部署了明确标识的、由多阶段多工具AI系统生成的评审，并通过大规模问卷调查（5,834份回应）和新型基准测试（SPECS）对AI评审效果进行了系统评估。**
## 📌DeepReviewer 2.0: A Traceable Agentic System for Auditable Scientific Peer Review【Research, Yue Zhang】

[https://arxiv.org/pdf/2604.09590](https://arxiv.org/pdf/2604.09590)

- 该论文将自动化同行评审的目标从“生成流畅文本”重新界定为“产出可审计的评审包（traceable review package）”，即包含结构化报告、页/段级锚定批注、优先级修复计划及显式新颖性评估的完整证据链。
## SciPredict: Can LLMs Predict the Outcomes of Scientific Experiments in Natural Sciences?【Research, Scale AI】

- 通过构建包含405个任务、涵盖物理学、生物学和化学33个细分领域的SciPredict基准，论文系统评估了15种前沿LLM在该任务上的性能，发现当前模型准确率仅达14-26%，且存在严重的校准缺陷，无法可靠区分可信与不可信的预测。问题类型：多选题（MCQ，40%）、自由形式（FF，32%）、数值预测（NUM，28%），分别测试识别、解释和定量推理能力。质量管控：投入$336k和7,380人工小时，由54.5%拥有博士学位的领域专家进行多轮审核。
## LABBench2: An Improved Benchmark for AI Systems Performing Biology Research【Research】

[https://arxiv.org/pdf/2604.09554](https://arxiv.org/pdf/2604.09554)

- **如何有效评估AI系统执行真实生物学研究任务能力？LABBench2通过以下方式解决这些问题： 提升任务真实性：****将近1,900个任务设计为开放式回答（而非选择题），要求在完整PDF论文语境中定位图表，或从专利/临床试验等非标准来源检索信息**** 增加任务难度：引入需要自主科学判断的任务（如SourceQualQA评估识别研究排除理由的能力），以及需要专业数据库导航（DbQA2）和精确序列操作（SeqQA2/CloningQA）的任务 扩展评估范围：新增对文献来源质量评估、专利检索（PatentQA）、临床试验检索（TrialQA）等 previously uncovered 的高价值科研应用场景的评估 该基准旨在更准确地测量AI系统在处理真实科研工作流程中的"检索与定位能力"、"专业数据访问能力"、"精确输入处理能力"和"科学辨别能力"，从而推动开发能真正辅助核心研究功能的AI工具。**
## NovBench: Evaluating Large Language Models on Academic Paper Novelty Assessment【Research】

[https://arxiv.org/pdf/2604.11543](https://arxiv.org/pdf/2604.11543)

- 论文构建了包含 1,684对论文-评审数据 的基准（来自EMNLP 2023），通过三阶段流水线自动提取： 新颖性描述提取：从论文Introduction中提取作者声明的创新点（使用GPT-5，准确率0.89）； 新颖性评价提取：从同行评审文本中抽取专家撰写的新颖性相关评价（使用GPT-4o-mini，准确率0.93）； 情感结构化：使用GPT-4o按情感极性（正面/中性/负面）去重并标准化评价文本。 该双源设计（作者声明+专家判断）形成了对比评测的黄金标准。关键发现： 性能天花板：**最强模型（GPT-4o）在相关性维度仅得3.70/5.0，表明对新颖性的理解仍停留在表层**； 提示策略权衡：Zero-shot在相关性上最佳；Few-shot提升覆盖性与正确性，但降低了相关性（模型模仿人类表达而非真正理解）；RAG改善清晰度，但可能因检索信息干扰而降低相关性； 专用模型缺陷：微调模型（如Reviewer2、CycleReviewer-8B）出现严重指令遵循失败（生成重复字符、空输出或格式混乱），表明过度微调可能损害通用指令遵循能力； 情感偏差：通用模型倾向于生成过多中性评价（迎合用户），而专用模型则过度批判（负面评价过多）； 人类验证：自动指标与专家判断的Fleiss' κ = 0.72，Spearman相关系数0.61（p<0.001 ），验证了评估体系的有效性。
## GIANTS: Generative Insight Anticipation from Scientific Literature【Research, Stanford】

[https://arxiv.org/pdf/2604.09793](https://arxiv.org/pdf/2604.09793)

- 论文提出GIANTS-4B模型，采用基于相似度评分的强化学习（RL）进行训练，以最大化生成洞见与真实下游洞见之间的语义相似度。该模型旨在**学习如何从父论文中识别概念进展、组合与抽象的模式，从而生成更清晰、更可能被高引用的研究洞见**。简言之，该工作通过定义可衡量的预测任务、构建标准化基准、并开发专门的训练方法，试图弥合当前语言模型与“站在巨人肩膀上”进行科学创新之间的能力差距。
## Agentic Discovery with Active Hypothesis Exploration for Visual Recognition【Research】

[https://arxiv.org/pdf/2604.12999](https://arxiv.org/pdf/2604.12999)

- 现有的大多数自动化架构设计方法往往依赖预定义的搜索空间或固定的种子架构（seed architecture），导致探索局限于对现有设计的增量修改和局部优化。HypoExplore旨在**实现从零开始的架构发现（from-scratch discovery），不锚定任何初始设计**，从而避免陷入重复的设计模式（repeated design patterns）和过度约束的局部修改（overly constrained local modifications）。缺乏对跨实验知识的系统性积累。HypoExplore引入**假设记忆库（Hypothesis Memory Bank）和轨迹树（Trajectory Tree）**，主动跟踪假设的置信度分数和实验证据，使得设计原理能够在独立的进化谱系间迁移（cross-lineage transfer），从而建立对设计空间的真正理解，而非仅仅发现单个高性能架构。证明该方法无需预定义的领域特定骨干网络，即可在MedMNIST等数据集上达到 state-of-the-art 性能。
## LiteResearcher: A Scalable Agentic RL Training Framework for Deep Research Agent【Research】

[https://arxiv.org/pdf/2604.17931](https://arxiv.org/pdf/2604.17931)

- LiteResearcher是一个可扩展的Agentic强化学习（RL）框架，它使用一个**模仿互联网结构和动态的本地“虚拟世界**”来训练深度研究智能体。一个用此框架训练的40亿参数模型在GAIA-Text上取得了71.3%的准确率，在Xbench-DeepSearch上取得了78.0%的准确率，通过稳定、在策略（on-policy）的RL训练，以零边际成本达到或超越了更大的开源和商业模型。LiteResearcher 的核心理念是，**要让代理泛化到开放网络，其训练环境必须拥有与互联网相同的结构多样性**。该框架通过双轨构建过程实现这一点：**大规模本地语料库扩展和多样化的任务合成流水线**。 本地语料库从维基百科等高质量种子开始，并进行迭代扩展。通过使用大型语言模型根据初始内容生成查询，然后从实时网络中获取相关页面，研究人员建立了一个包含来自 $100$ 万个独特域的超过 $3200$ 万页的本地数据库。该语料库涵盖从学术论文到生活方式博客的一切内容，为训练提供了丰富的“沙盒”。
## EvoMaster: A Foundational Evolving Agent Framework for Agentic Science at Scale【Research】

[https://arxiv.org/pdf/2604.17406](https://arxiv.org/pdf/2604.17406)

- EvoMaster是一个Agentic科学的基础框架，使AI智能体能够自主地、迭代地进行科学研究。通过促进持续的自我演化和多智能体协作，它持续优于通用智能体框架，在诸如“人类的最后一次考试”（Humanity's Last Exam）和MLE-Bench Lite等基准测试中，实现了+159%至+316%的相对提升。目录智能体科学的转型架构原则：模块化与实验性核心引擎和能力层SciMaster生态系统：行动中的智能体评估科学绩效意义与未来影响。EvoMaster是一个基础的演进智能体框架，旨在解决这些挑战。**它提供了一个标准化的、模块化的“试验平台”，允许研究人员构建能够在不同科学学科中持续自我演进的智能体。通过抽象化科学推理和实验的核心组件，EvoMaster旨在将自主发现从孤立的演示扩展到一个统一的研究生态系统。**
## DR 3 -Eval: Towards Realistic and Reproducible Deep Research Evaluation【Research】

[https://arxiv.org/pdf/2604.14683](https://arxiv.org/pdf/2604.14683)

- 论文提出 DR3-Eval，通过以下机制解决上述问题： 基于真实用户工作流的任务构造：从真实用户提供的多模态材料（文本、图像、视频等）出发，采用**逆向构建（reverse-construction）**方法，从已验证的证据文档派生查询，确保每个任务具有单一、明确的解决路径，消除评估歧义。 可控的静态沙盒环境：为每个任务构建独立的静态研究沙盒（sandbox corpus），模拟开放网络的复杂性（包含支持性文档、干扰项与噪声），同时保持完全可验证与可复现。 细粒度的多维评估框架：引入涵盖信息召回（Information Recall）、事实准确性（Factual Accuracy）、引用覆盖（Citation Coverage）、**指令遵循（Instruction Following）与深度质量（Depth Quality）**的评估体系，并与人类判断进行对齐验证。 简言之，该工作试图建立一个既能反映真实世界研究复杂性（多模态输入、噪声环境、长程推理），又能保证评估严格性（静态可复现、答案可验证）的基准测试，从而系统性地诊断现有大语言模型在深度研究任务中的检索鲁棒性与幻觉控制等关键失效模式。
## DR-Venus: Towards Frontier Edge-Scale Deep Research Agents with Only 10K Open Data【Research, Ant】

[https://arxiv.org/pdf/2604.19859](https://arxiv.org/pdf/2604.19859)

- 基于仅10K开源数据训练的前沿4B参数边缘规模深度研究智能体。深度研究智能体需通过迭代搜索、浏览与证据聚合解决复杂信息检索任务。边缘规模（小参数）模型在成本、延迟与隐私方面具有部署优势，但面临两大瓶颈：数据敏感性：小模型对噪声轨迹与格式错误更为敏感；强化学习低效性：长程任务中rollout组常无成功轨迹，导致优势估计崩溃与信用分配困难。DR-Venus证明了在严格的开源数据约束（~10K样本）下，通过精细的数据工程（清洗与重采样）与专门的强化学习算法（IGPO回合级优化），边缘规模模型（4B）可实现与大型系统（30B级）竞争的深度研究能力。该工作为资源受限场景下的智能体部署提供了实用路径，并强调了测试时计算扩展在解锁小模型潜力方面的关键作用。相关模型、代码与训练配方已开源，以支持可复现的边缘规模深度研究智能体研究。
## Read the Paper, Write the Code: Agentic Reproduction of Social-Science Results【Research】

[https://arxiv.org/pdf/2604.21965](https://arxiv.org/pdf/2604.21965)

- LLM智能体是否能够仅使用论文的方法描述和原始数据来重现经验社会科学结果。研究发现，顶级智能体能够重现80%以上的系数，使其落在原始95%置信区间内，而失败的原因通常归因于论文规范不足或数据缺失，而非仅仅是智能体错误。**要求研究人员阅读论文的方法学描述，并尝试仅使用原始数据，在无法访问原始作者代码的情况下重新创建结果。**
## The Last Human-Written Paper: Agent-Native Research Artifacts【Research, Orchestra】

[https://arxiv.org/pdf/2604.24658](https://arxiv.org/pdf/2604.24658)

- 论文提出Agent-Native Research Artifact (ARA) 协议，**将主要研究对象从叙事文档重构为机器可执行的知识包**，通过四个互锁层解决上述问题： 认知层（/logic）：结构化科学逻辑与可证伪声明 物理层（/src）：带完整操作规范的可执行代码 探索图（/trace）：保留失败实验与设计转向的完整研究DAG 证据层（/evidence）：绑定每个声明的原始经验输出 配套的三项机制（Live Research Manager、ARA Compiler、ARA-Native Review System）确保该生态系统可在现有研究流程中无缝运作，使AI智能体能够理解、复现并扩展前人工作，而无需反向工程散文或重新探索已知死胡同。
## SciEval: A Benchmark for Automatic Evaluation of K-12 Science Instructional Materials【Research】

[https://arxiv.org/pdf/2604.25472](https://arxiv.org/pdf/2604.25472)

- 论文构建了**首个面向K-12科学教学材料自动评估的基准数据集SciEval（包含273个课程、3549条基于EQuIP rubric的标准级注释），并系统评估了主流LLMs（GPT、Gemini、Llama、Qwen）的性能，最终通过领域特定微调（Domain-Specific Fine-Tuning）提升模型**在评分准确性和证据推理方面的表现。
## MTA-Agent: An Open Recipe for Multimodal Deep Search Agents【Search】

[https://arxiv.org/pdf/2604.06376](https://arxiv.org/pdf/2604.06376)

- 论文提出了**MTA-Agent**（Multihop Tool-Augmented Agent for Evidence-based QA Synthesis），**通过自动化的工具选择和证据验证流程，从现有VQA数据集合成21K高质量的多跳训练数据（MTA-Vision-DeepSearch）**。基于此数据集训练的32B模型**在六个深度搜索基准上达到SOTA性能**，显著提升了多步推理能力和工具使用的系统性。
## Towards Long-horizon Agentic Multimodal Search【Search】

[https://arxiv.org/pdf/2604.12890](https://arxiv.org/pdf/2604.12890)

- 如何在深度搜索过程中有效处理和管理积累的多模态上下文?论文提出了 LMM-Searcher 框架，其核心创新包括： 基于文件的视觉表示机制：**将视觉资产（图像）卸载至外部文件系统，以唯一文本标识符（UID）作为轻量级代理**，实现推理与感知的解耦。 渐进式按需加载：通过专门设计的 fetch-image 工具，允许智能体在需要细粒度视觉理解时主动检索特定图像，避免不必要的视觉token消耗。 长程可扩展性：该方法成功支持多达 100轮 的交互 horizon，在 MM-BrowseComp 和 MMSearch-Plus 等挑战性长程基准上取得开源模型中的最优性能。
## π -Play: Multi-Agent Self-Play via Privileged Self-Distillation without External Data【Search】

[https://arxiv.org/pdf/2604.14054](https://arxiv.org/pdf/2604.14054)

- 问题构建路径（Question Construction Path, QCP）。提出**Privileged Information Self-Play (π-Play)框架，实现完全无外部数据（data-free）**的自进化。通过考官（Examiner）、教师（Teacher）、学生（Student）三者的协同进化： **考官生成任务与QCP** **教师利用QCP提供密集监督** 学生在结果奖励与教师指导下高效学习。论文通过利用自博弈过程中被忽视的QCP作为特权信息，**将稀疏奖励的自博弈转变为密集反馈的自进化范式**，在无需外部标注数据的情况下，实现了比传统自博弈**2–3倍的进化效率提升**，并超越了全监督搜索代理的性能。
## AgentSearchBench: A Benchmark for AI Agent Search in the Wild【Search】

[https://arxiv.org/pdf/2604.22436](https://arxiv.org/pdf/2604.22436)

- 论文构建了AgentSearchBench基准测试，其特点包括： 包含**近10,000个真实世界agent，覆盖多平台、多描述风格** 支持可执行任务查询和高级别任务描述两种输入模式 采用基于执行的性能信号（execution-grounded performance signals）定义相关性，而非仅依赖文本相似度 验证了轻量级行为探测（execution-aware probing）可通过引入执行信号显著提升排序质量 简言之，该论文致力于建立一种基于实际执行效果的agent发现范式，弥补传统基于描述文本的检索方法与真实agent能力评估之间的鸿沟。
## MemFactory: Unified Inference & Training Framework for Agent Memory【Memory】

[https://arxiv.org/pdf/2603.29493](https://arxiv.org/pdf/2603.29493)

- 通过提出 **MemFactory** 框架，论文旨在建立一个标准化的"乐高式"（Lego-like）架构，**将记忆生命周期抽象为可插拔的原子组件（Extractor、Updater、Retriever、Agent Module），并原生集成 Group Relative Policy Optimization (GRPO) 算法，从而显著降低 Memory-RL 研究的工程门槛**，促进该领域的可复现性与创新。
## Memory Intelligence Agent【Memory】

[https://arxiv.org/pdf/2604.04503](https://arxiv.org/pdf/2604.04503)

- **Memory Intelligence Agent (MIA)**，一个面向深度研究智能体（Deep Research Agents, DRAs）的新型记忆增强框架，通过解耦记忆存储、规划生成与任务执行，解决传统长上下文记忆在存储效率、检索成本和推理质量方面的关键局限。MIA采用三模块解耦架构，实现非参数记忆与参数记忆的协同与双向转换，**Memory Manager（非参数记忆）**：类海马体的 episodic memory，将历史轨迹压缩为结构化工作流（workflow），采用混合检索策略；**Planner（参数记忆）**：基于检索到的历史经验生成高层搜索计划，支持**反射-重规划**（Reflect-Replan）机制，在遭遇执行瓶颈时动态调整策略；**Executor（执行器）**：在ReAct循环中解析并执行Planner生成的计划，调用文本/图像搜索工具与环境交互，反馈执行状态至Planner。
## SEARL: Joint Optimization of Policy and Tool Graph Memory for Self-Evolving Agents【Memory】

[https://arxiv.org/pdf/2604.07791](https://arxiv.org/pdf/2604.07791)

- 提出 **SEARL（Self-Evolving Agent Reinforcement Learning）** 框架，旨在通过**策略参数与外部工具记忆的联合优化**，解决资源受限环境下自进化智能体的学习效率和信用分配难题。论文提出**SEARL（Self-Evolving Agent Reinforcement Learning）**框架，通过以下机制实现突破： **Tool Graph Memory：将工具表示为节点、执行依赖表示为边的结构化记忆图**，提供状态抽象以促进跨任务泛化； 联合优化范式：**同步优化LLM策略参数与工具图记忆的拓扑结构**； 基于记忆锚点的优势估计：利用工具使用作为锚点进行步骤级分组，实现细粒度信用分配，有效 densify 奖励信号。
## PASK: Toward Intent-Aware Proactive Agents with Long-Term Memory【Memory】

[https://arxiv.org/pdf/2604.08000](https://arxiv.org/pdf/2604.08000)

- 针对**主动式人工智能（Proactive AI）在真实世界应用中的系统性缺失**展开研究，核心在于解决当前AI系统普遍采用的"被动响应"（你问我答）模式与真实人类需求之间的根本错配。DD-MM-PAS 通用范式 论文提出DD-MM-PAS（Demand Detection, Memory Modeling, Proactive Agent System）作为主动式AI的基础架构： Demand Detection (DD)：核心感知模块，将连续多模态输入转化为干预决策； Memory Modeling (MM)：三级层次记忆（User/Workspace/Global），支持个性化理解随时间演化； Proactive Agent System (PAS)：始终在线的执行骨架，协调工具调用与反馈闭环。
## LightThinker++: From Reasoning Compression to Memory Management【Memory】

[https://arxiv.org/pdf/2604.03679](https://arxiv.org/pdf/2604.03679)

- 从表示级压缩到行为级内存管理的演进路径： LightThinker：通过隐式隐藏状态压缩（implicit hidden-state compression），将冗长的思维链动态压缩为紧凑的语义表示（gist tokens），在保持准确率的同时显著降低峰值令牌使用量（减少70%）和推理时间（减少26%）。 LightThinker++：引入显式自适应内存管理（Explicit Adaptive Memory Management）范式，通过显式内存原语（commit、expand、fold）实现行为级的上下文管理： Commit：将完成的推理单元归档为语义摘要（summary），卸载详细内容。 Expand：在逻辑瓶颈时主动检索原始细节（raw details）。 Fold：使用完毕后重新压缩，保持上下文整洁。 这种方法使模型能够在长程代理任务中维持稳定的内存占用（在80轮交互后仍保持30k-40k令字的占用，相比基线减少60%-70%），同时通过解耦推理深度与内存消耗，在复杂场景下实现平均14.8%的性能提升。
## GAM: Hierarchical Graph-based Agentic Memory for LLM Agents【Memory】

[https://arxiv.org/pdf/2604.12285](https://arxiv.org/pdf/2604.12285)

- 论文提出GAM（Hierarchical Graph-based Agentic Memory）框架，通过以下机制解决上述冲突： 显式解耦编码与巩固：将记忆生命周期分离为事件缓冲阶段（Episodic Buffering）和语义巩固阶段（Semantic Consolidation） 状态切换机制：仅在检测到语义边界（semantic divergence）时，才将局部事件图整合进全局主题网络 图引导的多因素检索：利用结构化的跨层链接实现精确上下文召回 简言之，该论文试图**构建一种既能快速捕获实时交互又能保护已建立知识免受瞬时噪声破坏的分层图记忆架构，从而支撑LLM代理的连贯长期交互。**
## The Long-Horizon Task Mirage? Diagnosing Where and Why Agentic Systems Break【Long Horizon】

[https://arxiv.org/pdf/2604.11978](https://arxiv.org/pdf/2604.11978)

- HORIZON（Holistic Observations for Reasoning and faIlure analyZis in lOng-horizoN agents）——首个跨领域诊断基准与方法论框架。论文围绕两个核心问题展开：**智能体在长程设置的何处失效（Where）以及为何失效（Why）**。基于GPT-5系列与Claude-4在3,100+条轨迹上的评估，论文揭示： 非线性性能崩溃：**所有领域均呈现"稳定—急剧下降"模式，且**断裂点（breaking point）**具有领域特异性**（Web在极小 s 即崩溃，OS/Database可维持至中等 s ，Embodied下降最陡峭）。 失效模式结构性转变：随视界增加，主导失效从短程的环境/指令错误转向长程的规划错误（64.9%）、灾难性遗忘与历史错误累积。 模型差异与收敛：GPT-5更多受限于规划与记忆（18.3%），Claude-4对环境扰动更敏感（32.5%）；但进入断裂区域后，模型间性能差距消失，提示单纯基模扩展存在局限。
## Co-Evolving LLM Decision and Skill Bank Agents for Long-Horizon Tasks【Long Horizon, Skill】

[https://arxiv.org/pdf/2604.20987](https://arxiv.org/pdf/2604.20987)

- 解决**LLM智能体在长程交互环境中进行一致决策时面临的技能发现、保留与重用问题**。论文提出COS-PLAY框架，通过**协同进化（Co-Evolution）**机制解决上述问题： 决策智能体（AD ）：基于当前技能库检索候选技能，更新内部意图状态（zt ），并生成原始动作（at ） ；技能库智能体（AS ）：从无标注轨迹中自动提取、验证并维护技能协议（包括摘要、前置条件、执行计划、成功/中止标准及效果契约） ；闭环优化：决策智能体收集的轨迹用于改进技能库，而更新的技能库反过来提升后续决策质量，两者通过Group Relative Policy Optimization（GRPO）联合训练 该框架使智能体能够在无人工标注的情况下，从交互中自动发现可重用技能，并通过持续精炼技能库及其契约（Contracts）来适应不断演化的任务分布。
## StructMem: Structured Memory for Long-Horizon Behavior in LLMs【Long Horizon, Memory】

[https://arxiv.org/pdf/2604.21748](https://arxiv.org/pdf/2604.21748)

- 论文提出**StructMem**框架，通过**分层记忆组织**（事件级绑定保留局部关系结构，跨事件整合诱导全局连接）在避免昂贵图维护的前提下，实现结构丰富的记忆表示，以支持长程时序推理与多跳关系追溯。
## Decocted Experience Improves Test-Time Inference in LLM Agents【MIT】

[https://arxiv.org/pdf/2604.04373](https://arxiv.org/pdf/2604.04373)

- 核心概念：精炼经验（Decocted Experience） 论文识别出精炼经验是构建有效上下文的关键机制，包含三个环节： 萃取（Distillation）：从原始交互轨迹中提取可复用的策略与教训； 整合（Consolidation）：去除冗余信息，在记忆规模与多样性间寻求平衡； 结构化（Structuring）：组织成便于检索的数据结构，获取多样化且相关的信息。
## OrgAgent: Organize Your Multi-Agent System like a Company【IBM】

[https://arxiv.org/pdf/2604.01020](https://arxiv.org/pdf/2604.01020)

- **如何系统化地组织多个智能体以最大化协作效能？**论文验证了**公司式层级组织在多数场景下能够同时实现更高的任务性能****（最高提升102.73% F1-score）和更低的token消耗（最高减少74.52%）**，为多智能体系统的组织设计提供了实证依据和实用框架。核心发现 性能优势：层级结构在MuSiQue和SQuAD 2.0上显著优于扁平结构，F1-score提升最高达102.73% （GPT-OSS-120B于SQuAD 2.0） 效率优势：层级结构token消耗比扁平结构减少46.38% 至79.31% ，在GPT-OSS-120B上减少74.52% 行为改善：在不可回答问题上，层级结构弃权率达19% -40% ，而基线与扁平结构接近0% ，显示更强的知识边界识别能力 任务特异性：层级结构在需要稳定技能分配、受控信息流和分层验证的任务上收益最大（如SQuAD 2.0），但在MuSR（多步软推理）上优势不明显
## SkillTester: Benchmarking Utility and Security of Agent Skills【Skill】

[https://arxiv.org/pdf/2603.28815](https://arxiv.org/pdf/2603.28815)

- 论文提出 SkillTester 评估框架，核心设计包括： 比较性效用原则（Comparative Utility Principle）：通过配对的基线（无 Skill）与有 Skill 执行条件的对比，量化 Skill 带来的实际增量价值（任务完成能力 + 效率提升）。 受控安全探针（Controlled Security Probe Suite）：独立于效用评估，通过预设的三类安全检测方向（异常行为控制、权限边界、敏感数据保护）评估 Skill 的风险 profile。 简洁的用户导向输出：将复杂执行证据归一化为效用分数（Utility Score）、安全分数（Security Score）及三级安全状态标签（Pass/Caution/Risky），支持 Skill 选择与启用决策。 简言之，该论文试图建立一种可复现、可比较的质量保障机制（Quality-Assurance Harness），使 Skill 贡献的价值与风险在受控条件下可被观测、度量和审计。
## SkillClaw: Let Skills Evolve Collectively with Agentic Evolver【Skill】

[https://arxiv.org/pdf/2604.08377](https://arxiv.org/pdf/2604.08377)

- 解决**多用户LLM代理生态系统中技能（skills）的静态化与知识积累缺失**问题。论文提出SkillClaw框架，通过建立"交互→证据→进化→验证→部署"的闭环，使技能能够基于跨用户和随时间积累的交互证据进行自主进化，从而实现：集体进化：个体交互知识贡献于共享的持续改进技能生态完全自动化：无需人工策划或显式用户干预代理化适应：通过开放式推理而非预定义规则产生技能更新。
## Graph of Skills: Dependency-Aware Structural Retrieval for Massive Agent Skills【Skill】

[https://arxiv.org/pdf/2604.05333](https://arxiv.org/pdf/2604.05333)

- 论文提出 Graph of Skills (GoS)，一种推理时的结构检索层。该方法通过以下机制解决上述问题： 离线构建带类型的有向技能图，其中节点为可执行技能，边编码依赖关系（I/O兼容性）与工作流结构； 在线检索时采用混合语义-词汇种子定位初始相关技能，然后通过反向加权个性化PageRank在图上扩散，自动恢复上游依赖（如解析器、预处理器）； 上下文预算控制，通过重排序和水合（hydration）生成最小化的可执行技能包。 该方法的**目标是在严格上下文预算约束下，返回既与任务相关、又尽可能依赖完备（dependency-complete）的技能子集**，从而在保证任务成功率的同时显著降低token消耗。
## SkillX: Automatically Constructing Skill Knowledge Bases for Agents【Skill】

[https://arxiv.org/pdf/2604.04804](https://arxiv.org/pdf/2604.04804)

- 论文提出SkillX框架，通过构建分层技能知识库（Skill Knowledge Base, SkillKB） 实现： 多层级技能抽象：**将原始轨迹蒸馏为战略计划（Planning Skills）、功能子程序（Functional Skills）和原子操作（Atomic Skills）的三层层次结构** 跨智能体可迁移性：支持从强能力智能体（如GLM-4.6）向弱能力智能体（如Qwen3-32B）的知识蒸馏 自动化迭代优化：通过执行反馈精炼技能，并主动探索扩展技能覆盖范围，突破初始训练数据分布限制 该框架使经验以结构化、可组合、即插即用的形式存在，显著提升了长程交互式任务中的成功率和执行效率。
## Skill-SD: Skill-Conditioned Self-Distillation for Multi-turn LLM Agents【Skill】

[https://arxiv.org/pdf/2604.10674](https://arxiv.org/pdf/2604.10674)

- 论文提出**Skill-SD（Skill-Conditioned Self-Distillation）**框架，其核心创新包括： 动态技能信号：将完成轨迹异步总结为紧凑的自然语言技能（描述成功行为、错误和工作流程），作为仅用于训练的动态特权信息。 解耦条件机制：技能仅条件于教师（teacher），学生（student）始终在纯任务提示下生成轨迹，通过梯度蒸馏内化技能知识，避免推理时依赖。 重要性加权反向KL损失：推导ρ⋅k3 损失函数，修正跨提示设置中的分布不匹配，确保token级梯度无偏。 动态教师同步：周期性地将教师参数同步为最新学生检查点，确保特权信号与学生策略协同进化，避免 frozen teacher 导致的性能瓶颈。
## SkillFlow:Benchmarking Lifelong Skill Discovery and Evolution for Autonomous Agents【Skill】

[https://arxiv.org/pdf/2604.17308](https://arxiv.org/pdf/2604.17308)

- 首个系统性评估自主智能体**终身技能发现与进化**（lifelong skill discovery and evolution）能力的基准测试。论文构建了包含 **166个任务、20个任务族 **的评估体系，涵盖金融、运营、医疗、治理、数据处理五大领域。其创新设计包括： Domain-Agnostic Execution Flow (DAEF)：一种工作流框架，将任务抽象为领域无关的操作拓扑图 F=(VF,EF,λF) 。同一任务族内的任务共享相同的DAEF，仅领域实体不同，从而支持跨任务的程序性知识迁移。 双智能体任务构建流程：采用Architect Agent（生成任务）与Critic Agent（验证质量）的对抗式迭代，确保任务族内具有一致的工作流和可控的难度梯度。 Agentic Lifelong Learning 协议：形式化评估流程要求智能体： 从空技能库 S0=∅ 零启动； 按固定顺序依次解决任务族内任务； 基于执行轨迹 τt 和验证器反馈 rt 生成结构化技能补丁 Δt ，增量更新库 St=Apply(Δt,St−1) ； 验证技能在后续任务中的实际效用。
## 🧐From Skill Text to Skill Structure: The Scheduling-Structural-Logical Representation for Agent Skills【Skill】

[https://arxiv.org/pdf/2604.24026](https://arxiv.org/pdf/2604.24026)

- 现有工作主要关注技能仓库的构建、路由机制和安全分析，但缺乏一种可重用的、基于源的中间表示，能够将技能的调用接口、执行结构和操作/资源使用证据显式地解耦，从而支持跨任务的管理和使用。 为解决这些问题，论文提出了Scheduling-Structural-Logical (SSL) 表示法，将非结构化技能文档映射为三层类型化图结构： 调度层（Scheduling Layer）：暴露技能级接口（目标、签名、输入/输出、标签） 结构层（Structural Layer）：表示场景级执行阶段（准备、获取、执行、验证等有序场景） 逻辑层（Logical Layer）：记录原子动作和资源使用证据（读取、写入、工具调用、资源范围等） 通过这种显式表示，SSL使技能工件更易于被机器发现、审查和重用，同时保持与原始源文档的关联。
## SkillLearnBench: Benchmarking Continual Learning Methods for Agent Skill Generation on Real-World Tasks【Skill, Amazon】

[https://arxiv.org/pdf/2604.20087](https://arxiv.org/pdf/2604.20087)

- 论文构建了SkillLearnBench，这是首个专注于评估基于agent经验持续生成技能的基准测试，具备以下特性： 任务设计：包含20个经人工验证的、技能依赖型任务（skill-dependent tasks），跨越15个源自真实世界技能分类学的子领域 可复用性测试：每个任务配备多实例（multi-instance）测试，直接验证生成技能在跨实例场景下的复用能力 三层评估框架： Level 1（技能质量）：评估生成技能的覆盖率、可执行性与安全性 Level 2（执行轨迹）：分析agent是否实际采用技能及执行路径与预期步骤的对齐程度 Level 3（任务结果）：测量最终任务准确率与解决效率 通过该基准，论文首次对四种代表性持续学习方法（One-Shot、Self Feedback、Teacher Feedback、Skill Creator）进行了控制比较，揭示了**当前方法与人工编写技能之间仍存在显著差距**（最佳方法仅填补约45%的性能鸿沟），并发现外部反馈对迭代改进至关重要，而**单纯自我反馈会导致递归漂移**（recursive drift）。
## SemaClaw: A Step Towards General-Purpose Personal AI Agents through Harness Engineering【Harness】

[https://arxiv.org/pdf/2604.11548](https://arxiv.org/pdf/2604.11548)

- 论文指出，随着模型能力趋同，驾驭层（harness layer）——包括上下文生命周期管理、工具编排、多租户隔离和扩展架构——正成为系统差异化的主要场域。 SemaClaw框架通过以下机制系统性解决这些问题： DAG Teams：两阶段混合编排（LLM动态分解 + 确定性DAG执行） PermissionBridge：原生运行时权限桥接，支持显式用户授权 三层上下文架构：**工作记忆（压缩管理）、外部记忆（混合检索）、结构化上下文注入（SOUL.md锚定的人格分区） Wiki式个人知识**基础设施：基于Markdown的用户拥有知识库，实现跨会话知识复利。
## M⋆ : Every Task Deserves Its Own Memory Harness【Harness, Microsoft】

[https://arxiv.org/pdf/2604.11811](https://arxiv.org/pdf/2604.11811)

- 论文提出将**记忆系统的优化形式化为可执行程序搜索问题**，通过代码进化的方式自动发现任务优化的记忆结构（task-optimized memory harnesses），从而克服通用记忆范式在特定领域表现不佳的缺陷。MSTAR将记忆系统形式化为记忆程序 P （Python代码），包含三个可 jointly 优化的维度： **Schema：数据类定义（存储什么、如何查询）； Logic：读写操作实现（向量检索、SQL查询、LLM处理等）； Instructions：代理交互提示（如何提取信息、解释检索结果）**。 通过反射式代码进化迭代改进： 种群搜索：维护程序池，使用Softmax采样平衡探索与利用； LLM反射器（Reflector）：分析验证失败案例，生成代码补丁（V4A格式），同时优化记忆结构与提示指令； 质量约束：编译检查、冒烟测试、运行时限制（输出长度、调用预算），失败时触发自动修复循环； 高效采样：k-means聚类选择代表性验证子集，设施选址公式选择信息量最大的训练片段。
## ClawArena: Benchmarking AI Agents in Evolving Information Environments【Claw】

[https://arxiv.org/pdf/2604.04202](https://arxiv.org/pdf/2604.04202)

- 论文引入 CLAWARENA 基准，通过以下设计解决上述问题： 每个场景维护完整的隐藏 ground truth，同时仅向代理暴露嘈杂、部分且有时矛盾的痕迹； 构建跨8个专业领域的64个场景，包含1,879个评估轮次和365个动态更新； 设计14类别的问题分类法，覆盖三个维度的单独及交互作用（如 MS×DU、MS×P、DU×P 及三者的组合）； 采用多选题（集合选择）和基于shell的可执行检查两种格式，分别测试推理能力和工作区接地能力。 该基准首次系统性地评估代理在多源冲突、动态演变、用户个性化三者同时存在时的综合表现，填补了现有评估在"持久助手"真实部署场景中的空白。
## LiveClawBench: Benchmarking LLM Agents on Complex, Real-World Assistant Tasks【Claw】

[https://arxiv.org/pdf/2604.13072](https://arxiv.org/pdf/2604.13072)

- 现有LLM Agent基准测试（如WebArena、SWE-bench、GAIA等）通常仅在**单一维度**上评估能力（如单一网页环境、完全指定的指令或孤立工具使用），而真实世界助手任务具有**组合性困难**——多种复杂度来源（跨服务协调、隐式目标推断、动态环境变化）往往同时出现并相互交织。这导致现有评估与实际部署场景之间存在显著差距。
## ClawEnvKit: Automatic Environment Generation for Claw-Like Agents【Claw】

[https://arxiv.org/pdf/2604.18543](https://arxiv.org/pdf/2604.18543)

- 论文演示了按需生成能力：用户通过自然语言描述工作流（如"分类处理GitHub issues"），**系统交互式确认需求后即时生成验证环境，使评估成为可连续刷新、用户驱动的动态过程**，而非静态数据集。主要贡献ClawEnvKit框架：首个可扩展的claw-like代理环境自动化生成系统，将构建时间从小时级降至分钟级;Auto-ClawEval基准：首个大规模（1,040环境）、跨Harness（8种）、跨模型家族（4种）的claw-like代理评估基准;实时评估范式：证明自动化可使评估摆脱静态数据集的局限，实现按需生成和自适应训练环境构建。
## **MolClaw: An Autonomous Agent with Hierarchical Skills for Drug Molecule Evaluation, Screening, and Optimization**【Claw, AI Lab】

[https://arxiv.org/pdf/2604.21937](https://arxiv.org/pdf/2604.21937)

- MolClaw是一个自主人工智能代理，采用分层技能架构自动化早期药物发现工作流程，集成了30多种计算化学工具。它在MolBench基准测试的所有指标上都达到了最先进的性能，在多步骤药物评估、筛选和优化任务中展示了卓越的工作流程编排和自主错误恢复能力。目录分层技能架构标准化计算环境使用MolBench评估性能弹性和自主自我修复通过迭代实现科学洞察意义与未来方向
## ClawMark: A Living-World Benchmark for Multi-Turn, Multi-Day, Multimodal Coworker Agents【Claw】

[https://arxiv.org/pdf/2604.23781](https://arxiv.org/pdf/2604.23781)

- 论文提出了 ClawMark，一个专门**针对同事代理的基准测试**，具备以下特征： **多轮多天任务：每个任务跨越 2-6 个"工作日"（turns），模拟真实的时间跨度** 动态有状态环境：基于五大沙盒服务（文件系统、邮件、日历、知识库、电子表格），环境状态在轮次间通过"显性事件"（loud events）和"静默突变"（silent mutations）独立演变 全模态原始证据：包含图像、音频、视频、扫描PDF、电子表格等 1,072 个原始多模态工件 确定性规则验证：采用 1,537 个基于规则的 Python 检查器（checker）验证代理对服务端状态的实际修改，完全不使用 LLM-as-judge，确保评分的客观性和可重复性 该基准旨在评估代理在持久性、适应性、跨模态推理和合规性方面的关键能力，这些是现有静态、单轮、文本中心基准未能充分测量的核心素质。
## Holos: A Web-Scale LLM-Based Multi-Agent System for the Agentic Web【Web, SII】

[https://arxiv.org/pdf/2604.02334](https://arxiv.org/pdf/2604.02334)

- 专为 **Agentic Web** 设计的 Web 规模基于 LLM 的多智能体系统（LaMAS），旨在解决**大规模、开放式、长期运行的智能体生态系统**面临的根本性挑战，为人工通用智能（AGI）的集体智能路径提供工程基础。
## MolmoWeb: Open Visual Web Agent and Open Data for the Open Web【Web】

[https://arxiv.org/pdf/2604.08516](https://arxiv.org/pdf/2604.08516)

- 论文提出了完全开源的解决方案： MolmoWebMix：一个包含超过 100K 合成任务轨迹、30K+ 人工演示、原子 web 技能轨迹及 GUI 感知数据（指代表达定位、截图问答）的多样化数据集 MolmoWeb：基于 Molmo2 架构的 4B 和 8B 参数视觉-语言模型家族，仅通过网页截图即可预测浏览器操作（点击、输入、滚动等），无需 HTML 或 AxTree 输入 在 WebVoyager、Online-Mind2Web 和 DeepShop 等基准测试中，MolmoWeb-8B 不仅超越了同等规模的开放权重模型（如 Fara-7B、Holo1-7B），还超过了基于 GPT-4o 等更大闭源模型构建的 Set-of-Marks (SoM) 智能体，证明了数据质量和针对性训练可弥补模型规模差距。
## WebCompass: Towards Multimodal Web Coding Evaluation for Code Language Models【Web, Kuaishou】

[https://arxiv.org/pdf/2604.18224](https://arxiv.org/pdf/2604.18224)

-  **WebCompass**，一个面向大型语言模型的统一多模态网页编码基准测试框架，旨在解决现有评估体系仅关注静态正确性和单一任务类型、忽视视觉保真度与交互质量的问题。全生命周期任务覆盖 构建三维任务矩阵，形成 七个互补任务类别： 生成任务（Generation）：文本引导（Text-Guided）、视觉引导（Vision-Guided）、视频引导（Video-Guided）； 编辑任务（Editing）：文本引导与视觉引导的代码库修改（覆盖16种操作类型，如 Data Table、Parallax Scrolling）； 修复任务（Repair）：诊断修复与视觉诊断修复（覆盖11种缺陷类型，如 Occlusion、Semantic Error）。 总计 1526 个实例，涵盖15个生成应用领域，每个实例标注 Easy/Medium/Hard 难度。 确定性数据构建 采用多阶段人工参与（human-in-the-loop）管道： 生成任务：将模糊用户请求精炼为结构化网页设计文档（内容、交互、视觉三维度），或基于视频关键帧构建动态行为基准； 编辑任务：合成上下文一致的修改需求，刻意省略实现细节（如类名、CSS值），测试代码库级上下文感知； 修复任务：采用逆向构造——以干净原型为目标，注入可解释缺陷并生成精确到文本级的修改标注（search/replace），确保评估确定性。
## WebXSkill: Skill Learning for Autonomous Web Agents【Web, Skill】

[https://arxiv.org/pdf/2604.13318](https://arxiv.org/pdf/2604.13318)

- 论文提出 WEBXSKILL 框架，引入**可执行技能（executable skills）**的概念： 双重属性：**每个技能同时包含参数化动作程序（可直接执行的浏览器操作序列）和步骤级自然语言指导（语义注释、参数说明、每步操作目的），使技能对运行时（可执行）和智能体（可解释）均可用**。 两种部署模式： 接地模式（Grounded Mode）：智能体**将技能作为原子工具调用，运行时自动执行底层动作序列，最大化效率**； 指导模式（Guided Mode）：技能作为分步骤指令呈现，智能体使用原生浏览器动作逐步跟随，在页面状态与预期不符时保留自主适应和错误恢复能力。 通过三阶段流程（技能提取、技能组织、技能部署），WEBXSKILL 使智能体能够利用低成本合成轨迹构建可重用技能库，并在执行效率与自主适应性之间实现平衡。
## KnowU-Bench: Towards Interactive, Proactive, and Personalized Mobile Agent Evaluation【Mobile】

[https://arxiv.org/pdf/2604.08455](https://arxiv.org/pdf/2604.08455)

- **现有移动智能体（mobile agent）基准测试在评估个性化（personalization）与主动式（proactive）能力方面存在根本性缺失**的问题。论文提出了KnowU-Bench——一个基于可复现Android模拟环境的在线基准测试。其核心设计包括： 非对称信息架构：隐藏用户配置文件（User Profile），仅暴露行为日志，迫使智能体进行真正的偏好推断而非上下文查找； LLM驱动的用户模拟器：支持基于结构化画像的多轮澄清对话与主动式同意协商； 混合评估协议：结合规则验证与LLM-as-a-Judge评分，评估从偏好对齐、干预时机到拒绝后约束的全链条能力。
## VenusBench-Mobile: A Challenging and User-Centric Benchmark for Mobile GUI Agents with Capability Diagnostics【Mobile, Ant】

[https://arxiv.org/pdf/2604.06182](https://arxiv.org/pdf/2604.06182)

- VenusBench-Mobile 为应对上述挑战，论文提出一个**挑战性强且以用户为中心**的在线基准，通过以下设计实现更真实的评估： 用户意图驱动的任务设计：涵盖 10 类真实用户意图（如功能辅助、冲突处理、模糊指令、GUI 状态感知等），超越单一应用功能测试 PUDAM 能力诊断框架：从感知（Perception）、理解（Understanding）、决策（Decision）、行动（Action）、记忆（Memory）五个维度对任务进行分层标注，实现失败根因分析 环境变化鲁棒性测试：引入 80 种环境变体（语言、主题、设备形态等），评估智能体在真实分布偏移下的稳定性 实验表明，当前 SOTA 智能体在该基准上的成功率较传统基准下降约 50 个百分点，且在高阶感知和记忆任务上接近归零，揭示了现有智能体距离可靠的真实部署仍有显著差距。
## The Art of Building Verifiers for Computer Use Agents【Computer Use, Rubrics, Microsoft】

[https://arxiv.org/pdf/2604.06240](https://arxiv.org/pdf/2604.06240)

- 论文提出了Universal Verifier（通用验证器）框架，并确立了四项关键设计原则： **构建特定且不重叠的评分标准（Rubrics）**：避免"幻影标准"（phantom criteria）和级联错误，确保评估的一致性和可解释性； 分离过程奖励与结果奖励：独立评估代理执行步骤的合理性（过程）与最终目标是否达成（结果），以区分"因环境阻碍而失败"与"执行错误"； 区分可控与不可控因素：识别平台故障、实体不存在等外部障碍，避免对代理进行不公平惩罚； 分而治之的上下文管理：通过相关性矩阵筛选与每个评分标准最相关的截图，而非简单截断或全部输入，以处理长轨迹中的关键状态变化。 论文还发布了 CUAVerifierBench 基准数据集，用于标准化评估验证器与人类判断的一致性。实验表明，遵循上述原则的 Universal Verifier 可将假阳性率降至接近零（1%–8%），并在 Cohen's κ 指标上达到人类标注者间的一致性水平。
## See, Point, Refine: Multi-Turn Approach to GUI Grounding with Visual Feedback【Computer Use, Microsoft】

[https://arxiv.org/pdf/2604.13019](https://arxiv.org/pdf/2604.13019)

- 计算机使用代理（Computer Use Agents, CUAs）**在密集编码界面中的像素级精确光标定位问题**。 具体而言，论文试图解决以下核心挑战： 1. 编辑级定位的精度缺口 现有GUI定位方法在处理按钮、标签页、图标等较大可点击元素时表现良好，但**在文本密集界面（如IDE）中需要亚像素精度的"编辑级"操作（如将光标精确放置在特定字符边界、选择精确标记或点击相邻符号之间）时，单次坐标预测（single-shot prediction）往往失败**。即使预测位置在语义上接近目标，几像素的偏差也可能导致选择错误标记或在错误边界插入文本。 2. 缺乏错误纠正机制 当前主流方法依赖单次预测，**缺乏利用视觉反馈进行自我纠正的机制**。论文提**出多轮迭代细化框架，通过在前一次预测位置渲染红色十字标记（red-cross marker），使模型能够基于显式的空间错误信号进行视觉伺服（visual servoing），逐步减小定位误差**。 3. 密集IDE环境的特殊挑战 在代码编辑器中，行号、语法高亮标记、标点符号和狭窄的光标边界构成了视觉上拥挤的目标，对错误容忍度极低。论文通过构建专门的VS Code数据收集管道，建立从符号光标状态到渲染器空间坐标的精确映射，为像素级监督提供基础。 简言之，该工作探索如何通过闭环视觉反馈机制（closed-loop visual feedback），使多模态模型能够在高密度编码界面中实现可靠的精确光标定位，从而弥补语言理解与实际界面控制之间的"最后一公里"精度差距。
## Gym-Anything: Turn any Software into an Agent Environment【Computer Use】

[https://arxiv.org/pdf/2604.06126](https://arxiv.org/pdf/2604.06126)

- 论文提出了Gym-Anything框架，其核心创新包括： 多代理环境创建流程：将环境创建本身建模为代理任务——创建代理（Creation Agent）编写安装脚本并配置真实数据，审计代理（Audit Agent）独立验证环境质量，通过迭代循环确保正确性。 GDP导向的软件选择：基于美国职业数据（O*NET）和GDP贡献度，系统性地选择200个具有高经济影响的软件应用，覆盖全部22个SOC主要职业类别。 规模化任务生成：采用"提议-放大"（Propose-and-Amplify）策略，结合昂贵的代理模型生成高质量种子任务和廉价语言模型进行大规模扩展。 特权信息验证：利用环境设置脚本中的真实数据作为"特权信息"，构建基于VLM的清单式验证器（checklist-based VLM verifier），实现无需人工标注的细粒度评估。 最终成果CUA-World包含超过10,000个任务，跨越200个软件应用，涵盖Linux、Windows、Android和Web平台，并专门构建了CUA-World-Long基准（200个超长周期任务）来测试前沿模型的极限。
## UI-Oceanus: Scaling GUI Agents with Synthetic Environmental Dynamics【GUI】

[https://arxiv.org/pdf/2604.02345](https://arxiv.org/pdf/2604.02345)

- 论文提出UI-Oceanus框架，其核心创新在于： 转变学习范式：从模仿高级轨迹转向掌握原子级交互物理（interaction physics），通过自主学习环境前向动力学（forward dynamics）建立世界模型 利用环境反馈：将低成本的自主探索（autonomous exploration）转化为高密度的生成式监督，利用环境执行验证作为真实标签，绕过对昂贵人工演示或脆弱教师模型蒸馏的依赖 分离能力习得：将"什么可能发生"（动力学）的学习与"什么更 desirable"（意图）的学习解耦，先通过大规模合成数据建立稳健的物理基础，再通过少量专家演示进行对齐。通过系统研究自监督目标，论文发现**前向动力学**（预测未来界面状态）是扩展性的主要驱动因素，其性能显著优于逆动力学（inverse dynamics）和反向动力学（backward dynamics）。实验表明，该方法在离线基准测试中平均提升成功率7%，在真实在线导航中提升达16.8%，且性能随合成数据量呈对数线性扩展，未出现饱和迹象。
## Towards Scalable Lightweight GUI Agents via Multi-role Orchestration【GUI】

[https://arxiv.org/pdf/2604.13488](https://arxiv.org/pdf/2604.13488)

- **能否在成本与可扩展性之间实现有效权衡，使轻量级MLLMs无需依赖大规模参数扩展，即可通过参数共享与角色编排参与真实GUI工作流？**提出首个使轻量级MLLM具备任务可扩展性与MAS适配能力的框架，通过参数共享与角色编排扩展能力边界；开发LAMO-3B（3B参数），作为即插即用策略执行器，在保持低部署成本的同时，通过配合先进规划器达到超越原生大模型的性能上限；提出PWCE损失与ILG数据增强，显著改善轻量级模型的细粒度视觉感知与复杂布局定位鲁棒性。
## ClawGUI: A Unified Framework for Training, Evaluating, and Deploying GUI Agents【GUI】

[https://arxiv.org/pdf/2604.11784](https://arxiv.org/pdf/2604.11784)

- 论文提出了 ClawGUI —— 一个统一的开源框架，通过三个紧密集成的模块分别解决对应问题： ClawGUI-RL：提供首个支持大规模并行虚拟环境和真实物理设备的开源RL训练基础设施 ClawGUI-Eval：建立标准化的三阶段（推理-评判-度量）评估流程，在6个基准和11+模型上实现95.8%的官方基线复现率 ClawGUI-Agent：通过12+聊天平台将训练好的代理部署到Android、HarmonyOS和iOS设备，支持混合CLI-GUI控制和持久化个性化记忆。
## Generative UI: LLMs are Effective UI Generators【GUI, Google】

[https://arxiv.org/pdf/2604.09577](https://arxiv.org/pdf/2604.09577)

- 论文提出的**生成式用户界面（Generative UI）**范式旨在解决上述问题，其核心目标是使LLM能够： **不仅生成内容，还生成完整的交互式网页界面本身** 为任意自然语言提示创建定制化、可视化、高度交互的用户体验 实现"即时AI团队"概念：在分钟级时间内自动完成产品管理、UX设计和工程实现，为特定查询构建专属的迷你应用程序 简言之，该研究将LLM的应用**从"内容生成器"扩展为"端到端的用户体验生成器"**，突破了传统Markdown聊天界面的固有局限。
## InCoder-32B-Thinking: Industrial Code World Model for Thinking【Code】

[https://arxiv.org/pdf/2604.03144](https://arxiv.org/pdf/2604.03144)

- **工业软件开发中缺乏专家推理痕迹（expert reasoning traces）以及复杂硬件约束推理**的问题。论文提出通过Error-driven Chain-of-Thought (ECoT) 合成框架与 Industrial Code World Model (ICWM) 的协同： ECoT：通过多轮对话中的环境错误反馈合成思考内容，显式建模"尝试-失败-诊断-修正"的纠错过程，捕捉工业工程中的迭代精炼模式 ICWM：在领域特定执行痕迹（Verilog日志、GPU性能分析等）上训练世界模型，学习代码修改与硬件行为的因果动态，从而在不调用真实后端的情况下预测执行结果，支持大规模合成与自我验证 通过这种" grounded reasoning "范式，**模型能够在芯片设计、GPU优化、嵌入式系统和3D建模等工业基准测试中，生成与真实工程师推理深度分布匹配的思维链**，实现从通用代码智能到工业级代码智能的跨越。
## Scaling Coding Agents via Atomic Skills【Code, Skill】

[https://arxiv.org/pdf/2604.05013](https://arxiv.org/pdf/2604.05013)

- 论文提出将训练范式从任务级优化转变为原子技能掌握，通过定义五个可组合、可独立评估的原子技能（代码定位、代码编辑、单元测试生成、问题复现、代码审查），并在这些技能上执行联合强化学习，从而实现： 避免技能间的负向干扰 提升对未见复合任务（如bug-fixing、机器学习工程、代码安全等）的泛化能力 建立可扩展的coding agents训练新范式。
## HiL-Bench (Human-in-Loop Benchmark): Do Agents Know When to Ask for Help?【Code, ScaleAI】

[https://arxiv.org/pdf/2604.09408](https://arxiv.org/pdf/2604.09408)

- **前沿AI智能体在信息不完整场景下的判断力瓶颈。****当前编码智能体（如Claude Code、Codex）在面临缺失信息、歧义请求或矛盾指令时，倾向于基于自信假设直接执行，而非识别知识缺口并寻求人类帮助。**这种"自信的错误"（confident wrongness）导致企业级智能体试点项目失败率超过90%，但现有基准测试无法检测此缺陷。对GPT-5.4、GPT-5.3 Codex、Claude Opus 4.6、Gemini 3.1 Pro的评估显示： 性能崩塌：完整信息下Pass@3达64–91%，但需自主判断求助时降至2–38%； 模型家族指纹：** GPT：低召回（15–30%），表现为"沉默猜测"，极少求助； Claude：能检测不确定性（高自我评估失败率）但不转化为行动，或过度探索不执行**； Gemini：对外部信号响应敏感，但领域间差异大。
## Frontier-Eng: Benchmarking Self-Evolving Agents on Real-World Engineering Tasks with Generative Optimization【Code】

[https://arxiv.org/pdf/2604.12290](https://arxiv.org/pdf/2604.12290)

- Frontier-Eng——一个面向生成式优化（Generative Optimization）的大规模基准测试，系统**评估AI智能体在真实工程任务中进行迭代优化**的能力。涵盖47个任务，跨越五大工程领域：计算与量子信息（10任务）：GPU内核优化（FlashAttention、MLA）、密码学实现（AES/SHA）、量子电路合成运筹学与决策科学（9任务）：库存优化、作业车间调度、鲁棒投资组合优化机器人、控制与能源系统（8任务）：机器人路径规划、PID调参、电池快充曲线优化、可持续数据中心控制光学与通信系统（10任务）：自适应光学、衍射光学元件设计、光纤网络优化物理科学与工程设计（10任务）：结构拓扑优化（ISCSO）、化学反应优化、天体动力学轨迹设计。
## From Plan to Action: How Well Do Agents Follow the Plan?【Code】

[https://arxiv.org/pdf/2604.12147](https://arxiv.org/pdf/2604.12147)

- 当前智能体训练范式的根本局限：模型被训练为编码特定任务的工作流，而非学习遵循指令计划的元能力。
## SWE-AGILE: A Software Agent Framework for Efficiently Managing Dynamic Reasoning Context【Code, Memory, Microsoft】

[https://arxiv.org/pdf/2604.11716](https://arxiv.org/pdf/2604.11716)

- **SWE-AGILE**，一种专为多轮软件工程（SWE）任务设计的智能体框架，旨在解决**推理深度**与**上下文约束**之间的根本性冲突。
## LinuxArena: A Control Setting for AI Agents in Live Production Software Environments【Code】

[https://arxiv.org/pdf/2604.15384](https://arxiv.org/pdf/2604.15384)

- 论文提出了 LinuxArena，一个控制设置（control setting），其设计目标包括： 真实的多服务环境：包含 20 个实时、多服务的生产环境（如 SaaS 应用、DeFi 协议、医疗试验系统），使用 Docker Compose 部署，涵盖从 COBOL 到 Rust 等多种技术栈。 重叠的权限设计：合法工程工作依赖于与潜在破坏活动相同的网络访问、服务交互和实时状态，使得简单的沙盒隔离无法在不破坏实用性的前提下消除攻击面。 大规模任务集：包含 1,671 个主要任务（main tasks，代表合法软件工程工作）和 184 个次要任务（side tasks，代表安全故障，如数据泄露、后门植入、数据篡改等）。 全面的评估框架：通过破坏评估（sabotage evaluations）衡量攻击者在执行合法任务时完成有害行为的能力，以及监控评估（monitor evaluations）衡量监控系统检测此类行为的能力。论文通过 LinuxArena 验证了其作为控制研究工具的有效性，发现： **简单沙盒不可行：限制网络或服务的简单沙盒会显著降低主要任务性能，因此不能替代监控。** 当前监控不足：即使使用 GPT-5-nano 作为可信监控器，Claude Opus 4.6 仍能在约 23% 的情况下成功逃避检测并完成破坏。 攻击策略存在提升空间：人工构造的攻击轨迹（LaStraj）比模型生成的攻击具有更高的逃避率，表明当前攻击策略尚未饱和该环境，为未来控制协议的开发和评估提供了有意义的测试平台。
## Scaling Test-Time Compute for Agentic Coding【Code, Meta】

[https://arxiv.org/pdf/2604.16529](https://arxiv.org/pdf/2604.16529)

- Meta超智能实验室的研究人员引入了一个名为PDR+RTV的框架，用于通过**使用智能体轨迹的结构化摘要、用于并行选择的递归锦标赛投票（RTV）和用于顺序重用的并行-蒸馏-精炼（PDR）来扩展智能体编码中的测试时计算**。这种方法使Claude-4.5-Opus在SWE-Bench Verified上提高了6.7个百分点，在Terminal-Bench v2.0上提高了12.2个百分点。目录扩展 AI 智能体的推理时计算传统扩展在智能体式编码中的局限性通过展开摘要实现紧凑表示并行选择：递归淘汰投票顺序重用：并行-蒸馏-精炼统一的扩展管道经验发现和性能提升对未来AI智能体的重要性。
## RAT: RunAnyThing via Fully Automated Environment Configuration【Code】

[https://arxiv.org/pdf/2604.23190](https://arxiv.org/pdf/2604.23190)

- 论文提出了 RAT (RunAnyThing)，这是一个语言无关的模块化框架，通过以下机制实现全自动环境配置： **语义初始化（ImageRetriever）：通过语义分析选择最优基础镜像** 双模式规划机制：标准模式用于常规配置，自动模式通过结构化 [Plan.md](http://plan.md/) 处理复杂仓库 专业工具集：集成文件操作、错误恢复、版本切换等专用工具 鲁棒沙箱：基于 Docker 的隔离执行环境 论文还引入了 RATBench 基准测试，包含 2,000 多个多语言 GitHub 仓库，用于严格评估环境配置方法在真实仓库分布中的性能。
## Toward Scalable Terminal Task Synthesis via Skill Graphs【Hunyuan】

[https://arxiv.org/pdf/2604.25727](https://arxiv.org/pdf/2604.25727)

- 论文提出 **SkillSynth** 框架，通过**构建场景介导的技能图（scenario-mediated skill graph），将任务合成基于图采样的工作流路径**，从而显式控制合成任务所需的最小执行轨迹的多样性，实现可扩展且多样化的终端任务合成。
## ⭐Squeeze Evolve: Unified Multi-Model Orchestration for Verifier-Free Evolution【Orchestration】

[https://arxiv.org/pdf/2604.07725](https://arxiv.org/pdf/2604.07725)

- 论文试图解决： 模型能力分配问题：在 verifier-free 的进化流程中，如何将不同成本-能力权衡的模型（强但贵的闭源模型 vs. 弱但廉的开源模型）最优地分配到不同操作阶段（如初始化、重组、适应度估计），以实现"在最高边际效用处分配模型能力"（allocate model capability where it has the highest marginal utility）。 无验证器的适应度估计问题：在没有外部验证信号的情况下，如何利用模型内在的置信度（confidence）或多样性信号作为有效的适应度代理（fitness proxy），以指导模型路由决策。 成本-能力前沿优化问题：如何在给定预算约束下，通过多模型协调（multi-model orchestration）最大化进化推断的性价比，推动成本-能力前沿向左移动（即在更低成本下达到同等或更高的准确率）。 简言之，该论文致力于通过统一的多模型协调框架，在 verifier-free 设置下同时解决多样性保持与计算成本的双重挑战，实现高效且可负担的测试时扩展（test-time scaling）。
## ⭐GameWorld: Towards Standardized and Verifiable Evaluation of Multimodal Game Agents【Game】

[https://arxiv.org/pdf/2604.07429](https://arxiv.org/pdf/2604.07429)

- 论文提出GameWorld——一个包含**34个浏览器游戏、170个任务的标准化评估框架**，其核心贡献包括： 统一的执行接口：支持两种代理范式（Computer-Use Agents直接输出低层控制 vs. Generalist Agents通过确定性语义动作解析），并归一化为共享的原子事件空间（mouse_move, key_down等）。 基于状态的可验证评估（State-Verifiable Evaluation）：通过注入JavaScript桥直接读取序列化的gameAPI状态（如坐标、生命值、分数等233个任务相关字段），计算确定性的成功率和标准化进度（Progress），完全消除感知噪声。 延迟解耦机制：浏览器沙盒可在模型推理期间暂停游戏执行，使评估反映决策质量而非响应速度；同时提供实时变体（GameWorld-RT）以专门研究延迟影响。 能力对齐的课程分析：将游戏按能力瓶颈分级（基础控制、反应控制、空间导航、符号推理、长程协调），诊断代理的具体弱点。
## UniToolCall: Unifying Tool-Use Representation, Data, and Evaluation for LLM Agents

[https://arxiv.org/pdf/2604.11557](https://arxiv.org/pdf/2604.11557)

- 论文提出了 **UniToolCall** 框架，通过引入统一的 Query–Action–Observation–Answer（QAOA）表示格式、构建结构受控的混合训练语料（包含22k+工具和390k+实例），以及建立标准化的评估协议，实现了从工具集构建、数据生成到评估的全流程标准化。
## CocoaBench: Evaluating Unified Digital Agents in the Wild

[https://arxiv.org/pdf/2604.11201](https://arxiv.org/pdf/2604.11201)

- **现有基准仍孤立测试单一能力**（如仅CLI、仅GUI或固定工具API），无法评估代理在开放环境中**灵活组合视觉、搜索、编码能力**解决复杂长程任务的表现。COCOABENCH 基准 任务设计：153个人工设计的长程任务，覆盖9大领域，98%要求多能力协同（视觉+搜索+编码） 最小化定义：仅通过自然语言指令与最终输出评估函数定义任务，与特定运行时/工具生态解耦，迫使代理在开放世界中自主决策工具使用 可靠评估：采用结果导向的代理评估器（outcome-based proxy evaluators），通过验证最终可自动检查的输出（如价格、结构化数据）推断执行正确性，避免依赖LLM评判员 COCOA-AGENT 脚手架 提供轻量级、模块化的共享框架，集成39个工具（浏览器、DOM操作、终端、代码执行），支持在统一环境下对比不同模型骨干的代理能力 作为AIO Sandbox的接口，实现安全执行与可扩展评估，为强化学习训练提供基础平台。
## GTA-2: Benchmarking General Tool Agents from Atomic Tool-Use to Open-Ended Workflows

[https://arxiv.org/pdf/2604.15715](https://arxiv.org/pdf/2604.15715)

- 论文提出了 GTA-2（General Tool Agents-2） 层次化基准测试框架： **GTA-Atomic：继承自前作GTA，评估短程、封闭式的原子工具使用精度**，基于真实用户查询、真实部署工具和多模态上下文。 **GTA-Workflow：新引入的独立评估框架，针对长程、开放式生产力工作流**，采用**递归检查点机制（Recursive Checkpoint-based Evaluation）**将目标分解为可验证的子目标，实现对最终交付物的统一评估。 联合评估范式：首次在统一测试床中同时评估LLM能力与执行框架设计，揭示模型能力之外，系统级协调（如动态规划、持久化记忆）对工作流完成度的关键作用。 实验结果表明，**当前前沿模型在原子任务上已显吃力（成功率低于50%），在复杂工作流上更是呈现"能力断崖"——最佳模型（Gemini-2.5-Pro）在开放式工作流上的成功率仅为14.39%，而先进执行框架（如Manus、OpenClaw）能显著提升完成率**，凸显了执行系统设计超越底层模型能力的重要性。
## Agent-World: Scaling Real-World Environment Synthesis for Evolving General Agent Intelligence【Self Evolving, ByteDance Seed】

[https://arxiv.org/pdf/2604.18292](https://arxiv.org/pdf/2604.18292)

- 论文通过提出 **Agent-World** 框架，试图统一解决**"**如何规模化合成真实可执行环境**"**与**"**如何利用这些环境实现智能体的持续自我进化**"**这两个核心问题，从而突破当前智能体训练在环境真实性与终身学习能力方面的双重瓶颈。提出**首个统一可扩展真实环境合成与持续自进化训练的框架**；建立基于真实世界主题的大规模环境生态系统（近2K环境、近20K工具）；实现智能体策略与训练环境的协同进化，为构建具备终身学习能力的通用智能体提供基础设施。
## Training LLM Agents for Spontaneous, Reward-Free Self-Evolution via World Knowledge Exploration【Self Evolving, Tencent Hunyuan】

[https://arxiv.org/pdf/2604.18131](https://arxiv.org/pdf/2604.18131)

- 当前"自我进化"智能体本质上依赖人类设计的任务流程和验证奖励（Experience-Driven Evolution）或复杂的对抗工作流（Adversarial Evolution）。**一旦移除外部指令或奖励，进化即停止**。论文指出，这种被动范式与人类主动探索环境的直觉相悖，限制了智能体在完全陌生环境中的自主适应能力。论文提出**原生进化（Native Evolution）**能力，将智能体生命周期解耦为两个阶段： 进化阶段：在进入新环境 E 时，智能体自发执行探索与信息压缩，生成结构化的世界知识（World Knowledge）此过程在推理时完全无任务（Task-free）且无奖励（Reward-free）。 执行阶段：当分配下游任务时，智能体利用 K 作为"心智地图"指导决策。
## **Graph-of-Agents: A Graph-based Framework for Multi-Agent LLM Collaboration****【James Zou】**

[https://arxiv.org/pdf/2604.17148](https://arxiv.org/pdf/2604.17148)

- 代理图（GoA）框架引入了一种动态的、基于图的多智能体LLM协作系统，实现了结构化、与相关性相关的通信。与现有的多智能体方法相比，这种方法通过显著减少LLM调用、token使用和延迟，在不同基准测试中提高了任务性能并提升了效率。目录编排大型语言模型协同将协作形式化为图节点采样：缓解代理爆炸边采样：量化代理间相关性消息传递：双向细化图池化：综合最终响应推广现有框架准确性和效率的经验增益.
## From Skills to Talent: Organising Heterogeneous Agents as a Real-World Company

[https://arxiv.org/pdf/2604.22446](https://arxiv.org/pdf/2604.22446)

- **如何自动组织、协调和演化异构AI智能体劳动力，以解决跨领域的开放式复杂任务？论文提出**OneManCompany (OMC)**框架，通过以下三个维度重构多智能体系统： 组织抽象层：引入Talent-Container架构，将智能体身份（技能、工具、工作原则）与执行运行时解耦，通过****六种类型化组织接口（执行、任务管理、事件、存储、上下文组装、生命周期）实现异构后端的统一治理****； 动态项目执行：提出Explore-Execute-Review (E2R)树搜索，将项目执行建模为组织策略的搜索过程，结合DAG依赖调度与AND-树语义，提供终止性和无死锁的形式化保证； 持续演化机制：建立双向演化闭环，包括个体层面的工作原则精炼（通过CEO一对一会谈和任务后反思）和组织层面的知识积累（通过项目复盘更新SOP、HR绩效评估与自动离职机制）。 该框架旨在将多智能体系统从静态、预配置的管道转变为能够自我组织、自我改进的AI组织，以应对软件开发、内容生成、游戏开发、学术研究等跨领域的长时程复杂任务。**
## Recursive Multi-Agent Systems【Latent, James Zou】

[https://arxiv.org/pdf/2604.25917](https://arxiv.org/pdf/2604.25917)

- **如何将递归计算（recursive computation）从单个语言模型扩展到多智能体系统（MAS）层面，以实现系统级的协同优化与性能扩展。**RecursiveMAS 旨在构建一个**在潜在空间中进行递归协作的统一框架**，使异构智能体能够通过轻量级连接模块形成闭环迭代优化，从而突破传统多智能体系统在训练难度、推理效率和梯度稳定性方面的瓶颈。
## OxyGent: Making Multi-Agent Systems Modular, Observable, and Evolvable via Oxy Abstraction

[https://arxiv.org/pdf/2604.25602](https://arxiv.org/pdf/2604.25602)

- 论文提出了 OxyGent 框架，通过以下核心创新实现突破： **引入统一的Oxy原子抽象，将智能体、工具、LLM和推理流程封装为可插拔组件**，支持乐高式（Lego-like）组装和热插拔 实现权限驱动的动态规划（permission-driven dynamic planning），在运行时生成执行路径并提供实时可视化 构建OxyBank AI资产管理平台，建立自动化数据回流、标注和联合演化的闭环机制。

# AI for Science

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science
- 方法：agent, language, reasoning, multimodal-learning, stat-ml, multimodal-reasoning, gui-agent, stat-me
- 论文/报告：9 篇
- Scaling Atomistic Protein Binder Design with Generative Pretraining and Test-Time Compute
- Latent-Y: A Lab-Validated Autonomous Agent for De Novo Drug Design【Latent Labs】
- DrugPlayGround: Benchmarking Large Language Models and Embeddings for Drug Discovery
- General Multimodal Protein Design Enables DNA-Encoding of Chemistry
- EvoLen: Evolution-Guided Tokenization for DNA Language Model
- Towards Autonomous Mechanistic Reasoning in Virtual Cells【Valence Labs】
- **ProtoCycle: Reflective Tool-Augmented Planning for Text-Guided Protein Design**
- 📌MIMIC: A Generative Multimodal Foundation Model for Biomolecules
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Scaling Atomistic Protein Binder Design with Generative Pretraining and Test-Time Compute

[https://arxiv.org/pdf/2603.27950](https://arxiv.org/pdf/2603.27950)

- 论文提出Proteína-Complexa框架： 统一范式：结合大规模生成预训练与推理时计算扩展（test-time compute scaling），首次在蛋白质设计领域实现"基础模型+推理优化"的统一框架 Teddymer数据集：利用AlphaFold数据库（AFDB）中预测的单体结构域-结构域相互作用，**构建大规模合成二聚体数据集（350万个簇），解决数据稀缺问题** 全原子生成：基于La-Proteína的流匹配架构，通过潜在靶标条件机制（latent target conditioning）直接生成分子级（atomistic）结合物结构和序列，无需重新设计 推理时优化：**将扩散/流模型的测试时缩放算法（束搜索、Feynman-Kac导向、蒙特卡洛树搜索等）引入蛋白质设计，以结构预测置信度（如ipAE）或物理能量（如氢键能）为奖励函数，实现高效的生成先验引导搜索** 可控多样性：通过CAT标签（CATH分类）条件控制结合物的二级结构类别（α螺旋、β折叠或混合），突破先前方法的结构偏向限制 该框架在蛋白质靶标、小分子靶标及酶设计基准上均达到最先进的计算成功率，且无需序列重新设计即可直接生成高质量结合物。
## Latent-Y: A Lab-Validated Autonomous Agent for De Novo Drug Design【Latent Labs】

[https://arxiv.org/pdf/2603.29727](https://arxiv.org/pdf/2603.29727)

- 论文提出了Latent-Y，一个能够自主执行从头生物制剂设计的AI智能体，其核心贡献包括：端到端自主执行：**从自然语言文本提示出发，自动完成文献综述、靶点分析、表位识别、候选设计、计算验证及实验室就绪序列选择的全流程**，无需人工干预。专家级加速：将传统需要数周（约两周）的专家计算工作流程压缩至平均约5小时，实现56倍的加速，使单个研究人员能够在一天内完成过去需要数月的计算工作。规模化并行能力：通过消除对人工专家逐案处理的依赖，使药物设计从串行专家工作流转变为可并行的规模化流程，支持同时推进多个临床前药物项目。
## DrugPlayGround: Benchmarking Large Language Models and Embeddings for Drug Discovery

[https://arxiv.org/pdf/2604.02346](https://arxiv.org/pdf/2604.02346)

- 文提出了DrugPlayGround——一个统一的基准测试平台，旨在：系统评估LLMs在药物功能分析、药物-靶点相互作用预测、药物协同效应预测和药物扰动预测等关键研发阶段任务中的表现通过文本生成质量和嵌入表征能力两个维度全面评估LLMs结合领域专家反馈，测试LLMs的化学和生物学推理能力为不同任务提供模型选择和参数设置（如提示策略、温度参数）的实用指导
## General Multimodal Protein Design Enables DNA-Encoding of Chemistry

[https://arxiv.org/pdf/2604.05181](https://arxiv.org/pdf/2604.05181)

- 论文提出 DISCO（DIffusion for Sequence-structure CO-design），一种多模态扩散模型框架，通过以下机制解决上述问题： 序列-结构协同生成：联合建模离散序列和连续3D结构的联合分布，实现两者的同步去噪与生成； 反应中间体条件生成：仅需以密度泛函理论（DFT）计算的反应中间体几何结构为条件，无需预定义催化残基或完整过渡态模型； 推理时多目标优化：通过Feynman-Kac校正器（FKC）框架，在生成过程中直接引入基于序列和结构的奖励函数（如二硫键、阳离子-π相互作用、结合特异性），实现可控的功能搜索。 实验验证表明，DISCO能够为四种不同的卡宾转移反应（包括烯烃环丙烷化、B-H插入、C(sp³)-H插入和螺环丙烷化）设计出具有显著活性的血红素酶，其中部分设计的总周转数（TTN）超过先前经多轮定向进化获得的酶，且这些酶具有可进化的序列空间，可通过随机诱变进一步提升活性。
## EvoLen: Evolution-Guided Tokenization for DNA Language Model

[https://arxiv.org/pdf/2604.08698](https://arxiv.org/pdf/2604.08698)

- 论文提出EvoLen——一种融合**进化分层（evolutionary stratification）与长度感知解码（length-aware decoding）**的tokenizer： 基于跨物种进化信号（phyloP分数）将基因组划分为保守、中性、加速三类区域 对每个区域独立训练BPE并基于保守性优先级合并词汇表 通过动态规划与长度平方加权评分（score(t)=|t|2 ）优化分割路径，优先保留motif尺度的功能单元 通过将进化约束显式纳入tokenization过程，EvoLen旨在提供更符合生物学结构的序列表示，从而改善调控序列建模与跨物种泛化性能。
## Towards Autonomous Mechanistic Reasoning in Virtual Cells【Valence Labs】

[https://arxiv.org/pdf/2604.11661](https://arxiv.org/pdf/2604.11661)

- 论文提出VCR-Agent框架，通过以下方式实现自主机制推理： 结构化解释形式：将生物学推理约束为机制性动作图（mechanistic action graphs），以有向无环图（DAG）形式显式编码分子相互作用、调控关系和表型诱导等离散动作 多智能体架构：分离知识检索（Report Generator）与结构化推理生成（Explanation Constructor），确保事实依据 验证器过滤（Verifier-based Filtering）：整合药物-靶点相互作用（DTI）、差异表达（DE）等生物学验证器，自动过滤事实错误和逻辑不一致的推理痕迹 该框架旨在使虚拟细胞能够生成可解释、可证伪且生物学可信的自主推理轨迹。
## **ProtoCycle: Reflective Tool-Augmented Planning for Text-Guided Protein Design**

[https://arxiv.org/pdf/2604.16896](https://arxiv.org/pdf/2604.16896)

- ProtoCycle 是上海交通大学与DP Technology共同开发的一个框架，它为文本引导的蛋白质设计引入了一种反思性、工具增强的规划方法。该框架**利用大型语言模型作为高级规划器，迭代地协调专用工具**，从而在减少数据需求的同时，实现了最先进的语言对齐和具有竞争力的可折叠性。目录识别蛋白质设计中的规划-执行差距ProtoCycle 智能体框架模块化工具集成反思性规划与迭代修正训练策略：从模仿到强化经验性能和基准结果反思和工具效率的影响意义和未来方向
## 📌MIMIC: A Generative Multimodal Foundation Model for Biomolecules

[https://arxiv.org/pdf/2604.24506](https://arxiv.org/pdf/2604.24506)

- 生物学功能由序列、结构、调控、进化和细胞环境等多层约束耦合涌现，但现有基础模型（如ESM2、Evo 2、SpliceAI等）多为"孤立专家"：仅处理单一模态（如仅蛋白质序列），且仅支持前向预测（序列→表型）。这导致无法解决关键的**逆问题**——**给定期望的分子表型（如特定结构、剪接模式或稳定性），如何设计最可能的上游核苷酸序列？**此外，生物测量数据异质、稀疏且极少完整共现，传统架构难以处理这种**部分观察的多模态性**。论文提出 MIMIC（Multimodal Foundation Model for Biomolecules），旨在构建一个生成式多模态统一框架，实现： **任意条件化推理：基于任意观察到的模态子集（如仅结构、或结构+进化信号、或序列+实验上下文），重建或生成缺失的分子状态组件** 双向建模能力：既支持从序列预测功能（前向），也支持从功能约束设计序列（逆向），统一表示学习、条件预测与约束性生物分子设计 语义上下文整合：将实验条件（细胞系、组织、探测方法）作为可条件化的语义模态，而非固定输出，实现情境感知的预测（如特定化学探测环境下的RNA结构） 为支持这一目标，作者还构建了 LORE（Linked Observations of Regulatory and Evolutionary states）数据集，将跨域的异质生物数据对齐为统一的部分观察训练样本，提供跨模态监督信号。
## Learning Structure, Energy, and Dynamics: A Survey of Artificial Intelligence for Protein Dynamics

[https://arxiv.org/pdf/2604.25244](https://arxiv.org/pdf/2604.25244)

- 试图解决**蛋白质动力学建模中的计算效率、采样能力和数据稀缺性**等核心瓶颈问题。
