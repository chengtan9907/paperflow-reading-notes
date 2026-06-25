---
source: https://luminous-mat-781.notion.site/Daily-Note-February-2026-2fbe8c0e89c5801dbab3c96f4a9757f7?source=copy_link
---

# Daily Note - Feb 2026

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Daily Note - Feb 2026
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, vision, optimization
- 论文/报告：31 篇
- Show, Don't Tell: Morphing Latent Reasoning into Image Generation【Unified Model】
- PlanViz: Evaluating Planning-Oriented Image Generation and Editing for Computer-Use Tasks【Unified Model, GUI】
- ChatUMM: Robust Context Tracking for Conversational Interleaved Generation【Unified Model, Tencent Hunyuan】
- UReason: Benchmarking the Reasoning Paradox in Unified Multimodal Models【Unified Model】
- Kelix Technique Report【Unified Model】
- GENIUS: Generative Fluid Intelligence Evaluation Suite【Unified Model】
- UniT: Unified Multimodal Chain-of-Thought Test-time Scaling【Unified Model】
- LaViDa-R1: Advancing Reasoning for Unified Multimodal Diffusion Language Models【Unified Model, DLM】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Show, Don't Tell: Morphing Latent Reasoning into Image Generation【Unified Model】

[https://arxiv.org/pdf/2602.02227](https://arxiv.org/pdf/2602.02227)

- 将推理完全移至连续潜空间，使模型在生成过程中**动态监测自身视觉状态并自适应地注入隐式推理信号**，从而在不解码任何中间文本或图像的前提下，实现高效、保真且符合人类直觉的自修正生成。
## PlanViz: Evaluating Planning-Oriented Image Generation and Editing for Computer-Use Tasks【Unified Model, GUI】

[https://arxiv.org/pdf/2602.06663](https://arxiv.org/pdf/2602.06663)

- 在解决**UMMs在计算机使用任务（computer-use tasks）中的****规划导向图像生成与编辑能力****缺乏系统评估**的问题。它关注的不是单纯自然图像创作，而是模型是否能在具体的工具/应用情境中理解任务需求、规划步骤，并正确生成或编辑相应图像。
## ChatUMM: Robust Context Tracking for Conversational Interleaved Generation【Unified Model, Tencent Hunyuan】

[https://arxiv.org/pdf/2602.06442](https://arxiv.org/pdf/2602.06442)

- 核心是在**UMM中首次系统性解决“多轮对话 + 文本/图像交错生成”的上下文追踪问题**：作者将整段对话建模为一个交错的文本与图像 token 序列，通过 interleaved multi-turn training 和共享注意力机制，让模型在长历史、含噪对话中仍能准确引用、修改和延续先前生成的图像与语义；同时构建了一套 LLM 驱动的数据合成流水线，自动生成“有依赖、有干扰”的多轮多模态对话数据，弥补真实数据稀缺。实验表明，ChatUMM 在理解、生成和图像编辑质量上不输强基线，但在**跨多轮引用与持续一致性**这一关键能力上显著领先，使 UMM 从“一次性生成器”迈向真正的对话式多模态助手。
## UReason: Benchmarking the Reasoning Paradox in Unified Multimodal Models【Unified Model】

[https://arxiv.org/pdf/2602.08336](https://arxiv.org/pdf/2602.08336)

- 论文构建了 UReason 基准测试与评估框架： 基准设计：包含2,000个手工标注实例，涵盖CODE、ARITHMETIC、SPATIAL、ATTRIBUTE和TEXT五大推理类别，要求模型必须通过多步推理推导隐式视觉目标，再将其转化为像素级输出； 控制实验协议：通过对比三种设置来隔离推理的作用： 直接生成（Direct Generation）：直接从原始提示生成图像； 推理引导生成（Reasoning-Guided Generation）：基于完整推理轨迹（中间思维+精炼提示）生成图像； 去上下文生成（De-contextualized Generation）：仅基于推理产生的精炼提示（Refined Prompt）生成图像，丢弃原始提示和中间思维。
## Kelix Technique Report【Unified Model】

[https://arxiv.org/pdf/2602.09843](https://arxiv.org/pdf/2602.09843)

- Kelix在保持完全离散、自回归、与LLM生态兼容的同时，实现了与连续特征VLMs相当的理解性能（如在OCRBench上达到86.7分，超越此前最佳离散模型23%）。
## GENIUS: Generative Fluid Intelligence Evaluation Suite【Unified Model】

[https://arxiv.org/pdf/2602.11144](https://arxiv.org/pdf/2602.11144)

- 当前视觉生成模型的基准测试 predominantly 评估**晶体智力（Crystallized Intelligence），即模型对预训练知识的记忆与检索能力**（如生成"猫"的图像依赖于训练数据中的统计模式）。然而，这类评估**忽视了****流体智力（Fluid Intelligence）——即在全新情境中即时归纳模式、执行抽象推理和适应动态约束的能力**。
## UniT: Unified Multimodal Chain-of-Thought Test-time Scaling【Unified Model】

[https://arxiv.org/pdf/2602.12279](https://arxiv.org/pdf/2602.12279)

- **如何为统一多模态模型（unified multimodal models）实现可扩展的测试时推理（test-time scaling），使其能够通过迭代式的链式思考（chain-of-thought）进行多轮生成、验证与优化。通过集成****代理式数据合成（agentic data synthesis）、统一模型训练和多模态预算强制（budget forcing）推理****机制，使单一统一模型能够在测试时自主执行多轮生成-验证-优化的推理链条。**
## LaViDa-R1: Advancing Reasoning for Unified Multimodal Diffusion Language Models【Unified Model, DLM】

[https://arxiv.org/pdf/2602.14147](https://arxiv.org/pdf/2602.14147)

- 统一后训练目标：无缝集成SFT与多任务RL，以SFT正则替代KL散度，平衡稳定性与探索能力。 引导式Rollout生成：引入Answer-Forcing（利用dLLMs的修复能力注入正确答案生成合成推理轨迹）与Tree Search（从高分样本的早期扩散状态分支采样）两种机制，确保高质量训练信号。 互补掩码似然估计器：采用互补掩码策略与均匀权重w(t)=1 ，解决token覆盖不全与梯度不平衡问题。
## Understanding vs. Generation: Navigating Optimization Dilemma in Multimodal Models【Unified Model】

[https://arxiv.org/pdf/2602.15772](https://arxiv.org/pdf/2602.15772)

- 论文提出Reason-Reflect-Refine (R3) 框架，其核心思想是： 重构生成范式：**将传统的单步生成任务转化为多步的"生成-理解-再生成"（generate-understand-regenerate）**过程 内嵌理解机制：在生成流程中显式引入反思（Reflect）阶段，要求模型评估生成结果与用户意图的对齐度，并基于理解结果进行迭代修正 协同优化：通过将理解能力作为生成的内在组成部分，使生成质量的提升依赖于理解能力的增强，从而化解两种能力的竞争性冲突 该方法通过树状强化学习（Tree-RL）策略进行端到端训练，实现了生成质量与理解能力的同步提升，而非传统的权衡取舍。
## Mobile-O: Unified Multimodal Understanding and Generation on Mobile Device【Unified Model】

[https://arxiv.org/pdf/2602.20161](https://arxiv.org/pdf/2602.20161)

- 论文提出了**Mobile-O**框架，通过**Mobile Conditioning Projector (MCP)**实现高效的跨模态条件注入，并采用**四元组数据格式（generation prompt, image, question, answer）**的统一后训练策略，在仅使用约百万级样本（而非亿级）的情况下，实现了在iPhone等设备上约3秒生成512×512图像的实时性能，同时保持与大型模型竞争的理解与生成精度。理解分支：基于 FastVLM-0.5B（FastViT 视觉编码器 + Qwen2-0.5B 语言模型）； 生成分支：采用 SANA-0.6B DiT（Diffusion Transformer）扩散解码器，复用同一 LLM 处理文本提示，避免独立重型文本编码器； Mobile Conditioning Projector (MCP)：连接理解与生成的轻量级融合模块，通过深度可分离卷积与层间注意力机制，将 VLM 最后 K 层隐状态映射至扩散条件空间。
## From Abstract to Contextual: What LLMs Still Cannot Do in Mathematics【Math】

[https://arxiv.org/pdf/2601.23048](https://arxiv.org/pdf/2601.23048)

- 量化LLM在“情境化数学推理”（contextual mathematical reasoning）上的系统性短板。论文提出 ContextMATH 基准，将原题改造成两种情境化版本： Scenario Grounding (SG)：把变量与约束映射到现实叙事，不增推理复杂度，**仅考察“读懂故事”能力**； Complexity Scaling (CS)：**把显式条件隐藏为子问题，需先推导才能还原原始题面**，考察“先提炼再求解”能力。
## QEDBENCH: Quantifying the Alignment Gap in Automated Evaluation of University-Level Mathematical Proofs【Math】

[https://arxiv.org/pdf/2602.20629](https://arxiv.org/pdf/2602.20629)

- QEDBENCH旨在建立一个统计上可靠的大学级别数学证明自动化评估标准，通过双重评分标准（课程特定vs专家通用知识）和交叉评估矩阵，量化并解决当前LLM评估者与人类专家之间的系统性对齐差距。
## DAJ: Data-Reweighted LLM Judge for Test-Time Scaling in Code Generation【Code】

[https://arxiv.org/pdf/2601.22230](https://arxiv.org/pdf/2601.22230)

- 测试时扩展（Best-of-N）依赖 LLM-as-a-Judge 对多候选解打分，但训练数据存在三类分布偏移： 难易样本失衡 训练-评测任务不一致 弱模型训练轨迹 vs 强模型测试轨迹错位 导致 judge 泛化差，Hard 样例尤其严重。提出 DAJ，核心为“可验证奖励 + 双层数据重加权”框架： 下层：在大型弱模型生成的训练集上，用可学习权重 α 对样本加权，训练 judge 参数 ϕ。 上层：在小型强模型生成的元验证集上，优化 α 使 judge 泛化误差最小。 权重实现三档粒度：域级、实例表、实例网络（MLP 动态预测）。 训练目标支持 GRPO（RL）与 DPO/KTO/ORPO（偏好优化），奖励由代码执行结果自动给出，无需人工标注。 推理阶段对 N 条候选解进行多轮随机配对投票，按 5 步推理协议选出最佳解。
## FunPRM: Function-as-Step Process Reward Model with Meta Reward Correction for Code Generation【Code】

[https://arxiv.org/pdf/2601.22249](https://arxiv.org/pdf/2601.22249)

- 大模型代码生成缺乏天然步骤分解，现有 PRM 把每行当步导致步骤爆炸。提出 FunPRM Chain-of-Function 提示强制 LLM 按函数组织代码，每个函数视为 PRM 一步，兼顾可读性。 元学习奖励校正：用单元测试给出的干净终解奖励，通过双层优化训练轻量级“校正表”去噪 Monte-Carlo 部分奖励，再训练 PRM。
## Context Structure Reshapes the Representational Geometry of Language Models【DeepMind】

[https://arxiv.org/pdf/2601.22364](https://arxiv.org/pdf/2601.22364)

- 检验大型语言模型在上下文学习（ICL）中**是否普遍将内部表征“拉直”成线性轨迹以辅助预测**，并探讨该现象是否依赖任务结构。ICL 并非单一机制；LLM 像“瑞士军刀”按任务结构动态选用不同计算策略，只有连续预测型任务才采用“表征拉直”这一几何手段。
## WorldVQA: Measuring Atomic World Knowledge in Multimodal Large Language Models【Moonshot】

[https://arxiv.org/pdf/2602.02537](https://arxiv.org/pdf/2602.02537)

- 当前MLLMs评估存在**能力纠缠**（capability conflation）问题：现有基准（如MMMU、SimpleVQA）将视觉实体识别与复杂推理、OCR或语言知识耦合，导致无法确定错误源于"视觉感知"（眼睛）还是"语义记忆"（大脑）。此外，视觉幻觉（visual hallucination）的量化缺乏纯净测量工具。
## Steering LLMs via Scalable Interactive Oversight【Fudan NLP】

[https://arxiv.org/pdf/2602.04210](https://arxiv.org/pdf/2602.04210)

- 可扩展监督（Scalable Oversight）问题，提出通过结构化交互使非专家用户能够有效引导超越自身能力的强大语言模型。
## LLaDA2.1: Speeding Up Text Diffusion via Token Editing【dLLM, Ant】

[https://arxiv.org/pdf/2602.08676](https://arxiv.org/pdf/2602.08676)

- 引入 **Token-to-Token (T2T)** 编辑机制，与标准 **Mask-to-Token (M2T)** 生成协同工作。速度：LLaDA2.1-Flash 在 S Mode 下于 HumanEval+ 达到 891.74 TPS，BigCodeBench 达 801 TPS，LiveCodeBench 达 663 TPS； 质量：Q Mode 下 LLaDA2.1-Flash 平均分（73.54）超越 LLaDA2.0（72.43），在 AIME 2025、GPQA 等推理任务上显著提升。
## TextME: Bridging Unseen Modalities Through Text Descriptions

[https://arxiv.org/pdf/2602.03098](https://arxiv.org/pdf/2602.03098)

- **TextME 先通过****分析并消除预训练多模态模型中各模态相对于文本的系统性偏移（modality gap），把文本与模态映射到一个“可互换空间”****，然后以文本为唯一监督信号，将所有模态统一对齐到同一个语言语义锚空间，从而实现未见模态之间的零监督对齐。**
## When Less is More: The LLM Scaling Paradox in Context Compression【Baidu】

[https://arxiv.org/pdf/2602.09789](https://arxiv.org/pdf/2602.09789)

- 在压缩器-解码器（compressor-decoder）架构中，传统缩放定律（Scaling Laws）失效：参数规模从 0.6B 增至 90B 时，尽管训练损失降低、表面重建指标（如 BLEU）提升，但模型对源文本的忠实度显著下降。**较小模型（如 0.6B）倾向于逐字保留**，而**较大模型（如 90B）则系统性地篡改事实与结构**。论文挑战了缩放定律的普适性，指出在需要严格忠实度的任务中，模型规模扩张可能带来"过度语义化"与"生成不确定性"的副作用。为此，上下文压缩系统需采用不同于标准 scaling 的设计原则——主动约束表示的有效秩并降低解码熵，而非单纯增加参数。
## P1-VL: Bridging Visual Perception and Scientific Reasoning in Physics Olympiads【AI Lab】

[https://arxiv.org/pdf/2602.09443](https://arxiv.org/pdf/2602.09443)

- 首个在物理奥林匹克竞赛中达到金牌水平的开源VLM。
## **Step 3.5 Flash: Open Frontier-Level Intelligence with 11B Active Parameters**

[https://arxiv.org/pdf/2602.10604](https://arxiv.org/pdf/2602.10604)

- MoE语言模型，旨在以极低的推理成本（仅11B激活参数）实现前沿级智能体能力。混合注意力（S3F1）：采用3:1的滑动窗口注意力(SWA)与全注意力交错布局，配合96查询头的SWA和头门控注意力机制，将长上下文预填充FLOPs降低至全注意力的1/3，同时通过数据依赖的门控信号弥补性能损失。 专家并行优化：引入EP-Group Balanced Routing策略（公式1），解决分布式MoE中的负载倾斜问题；采用损失无关的负载均衡（Loss-free Load Balancing）避免路由崩溃。 多Token预测（MTP-3）：集成3个轻量级预测头，通过投机解码将生成吞吐量提升4倍，显著降低多轮智能体交互的延迟。
## FlyAOC: Evaluating Agentic Ontology Curation of Drosophila Scientific Knowledge Bases

[https://arxiv.org/pdf/2602.09163](https://arxiv.org/pdf/2602.09163)

- 解决**科学文献自动化知识库策展（knowledge base curation）缺乏端到端评估基准**的问题。通过提出 **FlyAOC** 基准，论文建立了首个针对"**从大规模科学文献进行穷尽式知识综合**"的评估框架，填补了现有深度学习代理基准（如 SWE-bench、WebArena）在**科学文献理解领域**的空白，为开发能够辅助或替代人工策展的 AI 系统提供了严格的测试平台。
## MiniCPM-SALA: Hybridizing Sparse and Linear Attention for Efficient Long-Context Modeling【Long Context, OpenBMB】

[https://arxiv.org/pdf/2602.11761](https://arxiv.org/pdf/2602.11761)

- MiniCPM-SALA（Sparse Attention and Linear Attention混合架构），通过以下策略解决上述问题： 混合架构设计：以1:3比例集成InfLLM-V2（稀疏注意力，负责高保真长距离建模）与Lightning Attention（线性注意力，负责全局 O(N) 效率计算），在保持精度的同时降低计算与内存开销。 高效训练范式：采用持续训练（Continual Training）将预训练Transformer模型转换为混合架构，相比从头训练降低约75%训练成本。 混合位置编码（HyPE）：协调短上下文与长上下文的性能表现，解决位置编码对长距离信息衰减的问题。 该方案使**9B参数模型能够在单张NVIDIA A6000D GPU上支持长达1M token的上下文推理**（传统8B全注意力模型因内存不足无法达到此规模），同时在256K序列长度下实现相比全注意力模型3.5倍的推理加速。
## GLM-5: from Vibe Coding to Agentic Engineering

[https://arxiv.org/pdf/2602.15763](https://arxiv.org/pdf/2602.15763)

- 核心目标是推动 AI 从被动的 "vibe coding"（人类提示下的代码生成）向主动的 **"agentic engineering"（智能体自主工程）** 范式转变。
## Xray-Visual Models: Scaling Vision models on Industry Scale Data【Meta】

[https://arxiv.org/pdf/2602.16918](https://arxiv.org/pdf/2602.16918)

- 一种面向工业级社交媒体数据的大规模统一视觉模型架构，旨在解决视觉模型与大型语言模型（LLMs）之间的数据规模鸿沟，并提升真实世界场景下的泛化能力。
## Active Zero: Self-Evolving Vision-Language Models through Active Environment Exploration【Self-Evolving】

[https://arxiv.org/pdf/2602.11241](https://arxiv.org/pdf/2602.11241)

- 论文提出了 Active-Zero 框架，核心思想是让模型像人类一样，**根据自己当下的能力水平，主动去“寻找”和“探索”适合自己的学习材料**。 该框架由三个协同进化的代理（Agents）组成： 搜索者 (Searcher)： 负责**从开放世界的资源库中检索图像**。它会根据模型当前的“能力边界”来寻找图像，既不找太简单的，也不找完全无法处理的。 提问者 (Questioner)： 针对搜索到的图像，合成并校准具有挑战性的推理任务（即生成问题）。 解题者 (Solver)： 负责解决这些任务，并通过准确性奖励（Accuracy Rewards）进行不断的强化和微调。
## OmniScience: A Large-scale Multi-modal Dataset for Scientific Image Understanding【DP】

[https://arxiv.org/pdf/2602.13758](https://arxiv.org/pdf/2602.13758)

- 论文提出了OmniScience数据集及配套的数据构建流程：从25个高影响力开放获取期刊和预印本中解析出150万个高质量图-标题-上下文三元组，涵盖生物学、物理学、化学、材料科学等10个主要学科开发动态模型路由的重新标注管道，整合Gemini、GPT、Qwen等前沿MLLMs，生成密集、自包含的科学描述，将图像-文本相似度从0.769提升至0.956建立严格的质量控制体系，包括跨模态去重、幻觉检测和专家对齐的LLM评判机制。
## How Vision Becomes Language: A Layer-wise Information-Theoretic Analysis of Multimodal Reasoning

[https://arxiv.org/pdf/2602.15580](https://arxiv.org/pdf/2602.15580)

- 当前MLLM（如LLaVA-1.5/1.6）遵循**模态转导**机制：视觉独特信息在早期层（0-10层）达到峰值后单调衰减，语言独特信息在后期层（20-32层）激增并占据最终预测的约82%，而协同信息始终低于2%。这一发现表明，现有MLLM并非执行"涌现的跨模态融合"，而是**将视觉证据转译为语言空间表征**以进行下游推理。
## How to Train Your Long-Context Visual Document Model

[https://arxiv.org/pdf/2602.15257](https://arxiv.org/pdf/2602.15257)

- 解决**长上下文视觉文档模型训练方法不透明、不可复现**的问题，并系统性地探索如何构建高性能的长文档视觉问答（VQA）系统。
## ExpertWeaver: Unlocking the Inherent MoE in Dense LLMs with GLU Activation Patterns

[https://arxiv.org/pdf/2602.15521](https://arxiv.org/pdf/2602.15521)

- **GLU（Gated Linear Unit）机制**提供了密集到MoE转换的自然蓝图。**通过分析GLU的细粒度神经级激活模式，可以揭示一个固有的MoE架构：由持续激活的通用神经元和动态激活的专用神经元组成**。基于此，论文提出**ExpertWeaver**框架，通过三阶段流程（捕获激活模式、层自适应配置、神经元分区）实现无需训练的密集到MoE转换，同时保持原始模型的激活结构完整性。它本质上就是把 GLU 里的“细粒度神经元级激活模式”，升格成“专家级激活模式”，从而把一个 dense FFN 重新组织成 MoE 结构。

# Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reasoning
- 方法：agent, generation, language, vision-language-model, reasoning, vision, reinforcement-learning, optimization
- 论文/报告：40 篇
- Good SFT Optimizes for SFT, Better SFT Prepares for Reinforcement Learning【SFT&RL】
- Why Does RL Generalize Better Than SFT? A Data-Centric Perspective on VLM Post-Training【SFT&RL】
- Towards On-Policy SFT: Distribution Discriminant Theory and its Applications in LLM Training【SFT&RL】
- On-Policy Supervised Fine-Tuning for Efficient Reasoning【SFT&RL】
- ⭐⭐Evolutionary System Prompt Learning can Facilitate Reinforcement Learning for LLMs
- ⭐⭐What LLMs Think When You Don’t Tell Them What to Think About?【James Zou】
- What Does Vision Tool-Use Reinforcement Learning Really Learn? Disentangling Tool-Induced and Intrinsic Effects for Crop-and-Zoom【Visual Reasoning, Pengfei Liu, MiniMax】
- AdaptMMBench: Benchmarking Adaptive Multimodal Reasoning for Mode Selection and Reasoning Process【Visual Reasoning, BIGAI】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Good SFT Optimizes for SFT, Better SFT Prepares for Reinforcement Learning【SFT&RL】

[https://arxiv.org/pdf/2602.01058](https://arxiv.org/pdf/2602.01058)

- 更强 SFT 检查点 ≠ 更好 RL 结果：离线目标若只追求自身准确率，常与后续在线 RL 的分布失配，导致“离线领先、在线逆转”。
## Why Does RL Generalize Better Than SFT? A Data-Centric Perspective on VLM Post-Training【SFT&RL】

[https://arxiv.org/pdf/2602.10815](https://arxiv.org/pdf/2602.10815)

- RL的泛化优势源于其**隐式数据过滤机制**——RL天然优先学习中等难度样本（产生高奖励方差的样本），而忽略简单（始终高奖励）和困难（始终低奖励）样本。提出Difficulty-Curated SFT (DC-SFT)，通过显式过滤困难样本（保留简单和/或中等难度样本）来： 消除标准SFT因均匀处理困难样本导致的过拟合 达到甚至超越RL的OOD泛化性能 保持比RL更高的训练稳定性和计算效率（速度提升3-5倍）
## Towards On-Policy SFT: Distribution Discriminant Theory and its Applications in LLM Training【SFT&RL】

[https://arxiv.org/pdf/2602.12222](https://arxiv.org/pdf/2602.12222)

- SFT与RL性能差距的**关键在于RL使用同策略数据（on-policy data）——即模型自身生成的数据**，这保留了模型的原生分布。因此，核心问题为：能否在保持SFT计算效率的同时，通过将训练过程与模型自身分布对齐（包括损失层面与数据层面），实现类似RL的泛化性能？
## On-Policy Supervised Fine-Tuning for Efficient Reasoning【SFT&RL】

[https://arxiv.org/pdf/2602.13407](https://arxiv.org/pdf/2602.13407)

- 通过**移除KL散度和组归一化**，并**采用最简单的截断长度惩罚（对超过固定长度限制的正确响应给予零奖励），论文证明原始策略梯度目标可简化为无奖励形式的极大似然监督微调（SFT）**——即在由当前策略生成、并经过正确性和简洁性筛选的在线数据上进行训练（On-Policy SFT）。
## ⭐⭐Evolutionary System Prompt Learning can Facilitate Reinforcement Learning for LLMs

[https://arxiv.org/pdf/2602.14697](https://arxiv.org/pdf/2602.14697)

- **如何联合优化LLM的上下文（系统提示）与模型权重，以实现更有效的自主自我改进？**论文提出进化系统提示学习（Evolutionary System Prompt Learning, E-SPL），其核心创新包括： 联合优化框架：在每次RL迭代中，维护一个系统提示种群，并行采样多个提示进行策略梯度更新，同时基于相对表现通过TrueSkill评分系统对提示进行进化选择 LLM驱动的遗传算子：利用参考模型（frozen LLM）实现变异（mutation）和交叉（crossover）操作，基于自我反思编辑系统提示，**将经验转化为显式指令** 知识分工：鼓励**在提示中编码高层策略和验证规则（陈述性知识），在权重中编码执行技能和直觉（程序性知识）** 实验表明，该方法在数学推理（AIME、HMMT）和智能体搜索任务上，相比单独的RL或提示进化，显著提升了成功率、样本效率和从易到难的泛化能力（如在BeyondAIME上从38.8%提升至45.1%）。
## ⭐⭐What LLMs Think When You Don’t Tell Them What to Think About?【James Zou】

[https://arxiv.org/pdf/2602.01689](https://arxiv.org/pdf/2602.01689)

- 研究了LLMs在**近无约束生成**（near-unconstrained generation）条件下的内在行为模式（"top-of-mind"行为）。
## What Does Vision Tool-Use Reinforcement Learning Really Learn? Disentangling Tool-Induced and Intrinsic Effects for Crop-and-Zoom【Visual Reasoning, Pengfei Liu, MiniMax】

[https://arxiv.org/pdf/2602.01334](https://arxiv.org/pdf/2602.01334)

- 作者质疑当前视觉工具使用RL所带来的性能提升，是否真正来自于模型对工具（如crop-and-zoom）的掌握，还是仅仅源于模型内在能力的增强。工具使用RL的**主要效果是减少工具引入的损害（如减少错误调用、降低工具模式干扰）**。并未显著提升模型利用工具纠正内在失败的能力。换言之，模型学会的是“与工具安全共存”，而非“掌握工具”。
## AdaptMMBench: Benchmarking Adaptive Multimodal Reasoning for Mode Selection and Reasoning Process【Visual Reasoning, BIGAI】

[https://arxiv.org/pdf/2602.02676](https://arxiv.org/pdf/2602.02676)

- **如何科学量化VLMs在文本推理与工具增强视觉推理之间进行自适应选择的能力？**
## Socratic-Geo: Synthetic Data Generation and Geometric Reasoning via Multi-Agent Interaction【Visual Reasoning, Alibaba, Linfeng Zhang】

[https://arxiv.org/pdf/2602.03414](https://arxiv.org/pdf/2602.03414)

- 论文提出**Socratic-Geo**框架，旨在建立一个**完全自主的、目标驱动的数据合成引擎**，通过多智能体交互将数据合成与模型学习动态耦合，实现从少量种子问题（仅108个）出发的自我进化与课程学习。
## Evolving from Tool User to Creator via Training-Free Experience Reuse in Multimodal Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2602.01983](https://arxiv.org/pdf/2602.01983)

- 论文提出了 UCT（User to Creator via Training-Free experience reuse） 框架，旨在实现： 从工具使用者到创造者的转变：使Agent能够在推理过程中自适应地创建工具，并将推理经验封装为可重用的工具资产 自我进化能力：通过在线工具创建循环和离线记忆巩固机制，实现工具库的持续自我更新和优化，无需额外训练 经验重用：建立可维护的工具库，确保高质量的经验记忆能被后续推理任务高效检索和复用。
## What, Whether and How? Unveiling Process Reward Models for Thinking with Images Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2602.08346](https://arxiv.org/pdf/2602.08346)

- 解决**Large Vision Language Models (LVLMs)在"thinking with images"范式**下推理过程中的质量评估与改进问题。现有的Process Reward Models (PRMs)基准测试主要集中在**纯文本推理**轨迹上，缺乏针对"thinking with images"这种**交错文本-图像多模态推理**范式的综合评估体系。这种 gap 使得研究者难以有效评估PRM在视觉推理过程中的监督能力。
## Chatting with Images for Introspective Visual Thinking【Visual Reasoning】

[https://arxiv.org/pdf/2602.11073](https://arxiv.org/pdf/2602.11073)

- 论文提出了一个全新的视觉-语言交互推理框架： 核心理念 将图像“操作”视为 受语言引导的特征调制（language-guided feature modulation） 用语言提示动态引导视觉编码，而不是固定的单次编码。就是多轮think with image。
## TwiFF (Think With Future Frames): A Large-Scale Dataset for Dynamic Visual Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2602.10675](https://arxiv.org/pdf/2602.10675)

- 首个面向开放动态场景的大规模视觉思维链（Visual Chain-of-Thought, VCoT）框架。
## Reliable Thinking with Images【Visual Reasoning】

[https://arxiv.org/pdf/2602.12916](https://arxiv.org/pdf/2602.12916)

- 论文提出了**Reliable Thinking with Images (RTWI)** 方法，通过文本中心的可靠性估计机制，统一识别视觉线索和文本CoT的可靠性，并采用双阶段过滤与投票模块来防止NT污染最终答案。作者指出现有的 TWI 方法存在一个关键假设，即认为这些交错的图像-文本思维链是完美无缺的。然而在现实世界的复杂多模态理解场景中，这一假设很容易被打破。论文揭示了一个被称为 Noisy Thinking (NT) 的实际问题：NT 指的是在挖掘视觉线索和推理答案的过程中出现的不完美或错误。错误的思维链会导致误差积累，从而严重降低多模态大模型的性能。为了解决上述的“噪声思维”问题，论文提出了一种新方法 RTWI： 核心机制：RTWI 在一个统一的以文本为中心的方式下，估计视觉线索和文本思维链的可靠性（reliability）。 具体操作：基于这种可靠性估计，RTWI 采用了鲁棒的过滤和投票模块（filtering and voting modules），以防止噪声思维污染最终的答案。
## OneLatent: Single-Token Compression for Visual Latent Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2602.13738](https://arxiv.org/pdf/2602.13738)

- 采用**最小描述长度（Minimum Description Length, MDL）**视角：**在能产生正确答案的多个中间解释中，选择最短的充分描述作为更强的归纳偏置**； 通过单token视觉潜在压缩：**将显式CoT文本渲染为图像，利用DeepSeek-OCR视觉编码器提取隐藏状态作为监督信号，将中间推理压缩为单个连续的潜在token（visual latent token）**； 实现压缩约束下的泛化：在严格限制中间接口预算（仅单token）的情况下，迫使模型仅编码回答所必需的充分信息，从而在保持准确率的同时（仅下降2.21%），将平均输出长度减少11倍（最高达87.4倍），并显著提升输出token贡献率（OTC，即每token准确率）。
## On the Out-of-Distribution Generalization of Reasoning in Multimodal LLMs for Simple Visual Planning Tasks【Visual Reasoning】

[https://arxiv.org/pdf/2602.15460](https://arxiv.org/pdf/2602.15460)

- 尽管CoT推理显著提升了大语言模型在复杂任务上的表现，但其**OOD泛化能力仍定义模糊且理解不足**。现有研究表明，CoT可能主要反映数据的统计特性而非真正的算法学习，在输入分布偏离训练数据时性能急剧下降。该研究旨在通过受控实验，系统检验不同输入表示（视觉与文本）和CoT格式对模型泛化行为的影响。
## DeepVision-103K: A Visually Diverse, Broad-Coverage, and Verifiable Mathematical Dataset for Multimodal Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2602.16742](https://arxiv.org/pdf/2602.16742)

- 论文提出DeepVision-103K数据集，通过以下三个核心特性解决这些问题： 视觉多样性：涵盖几何、解析图、图表及真实世界物品等主要视觉类别，且每个类别包含比现有开源数据集更丰富的元素类型； 广泛覆盖：涵盖多样化的K12数学主题、广泛的知识点以及视觉逻辑问题（如迷宫、棋类、俄罗斯方块），联合增强数学与视觉逻辑推理能力； 自动数据整理流程：通过有效性过滤、通过率分层和正确性验证的三阶段流程，将多样化但 noisy 的真实世界K12问题转化为结构化且可验证的问答对，实现对齐模型能力的难度校准与可靠奖励信号构建。
## PyVision-RL: Forging Open Agentic Vision Models via RL【Visual Reasoning】

[https://arxiv.org/pdf/2602.20739](https://arxiv.org/pdf/2602.20739)

- 此前动态工具方法多依赖专有API或仅适用于图像任务，**开源权重的多模态强化学习（特别是视频领域）探索不足，缺乏统一的训练框架来同时支持图像和视频的代理式推理**。 为解决上述问题，论文提出了PyVision-RL框架，通过以下机制实现稳定且高效的代理式训练： 过采样-过滤-排名（Oversampling–Filtering–Ranking） rollout策略：筛选具有适度难度且交互完整的轨迹，消除零方差组和破碎轨迹对训练的干扰 累积工具奖励（Accumulative Tool Reward）：仅在回答正确时按工具调用次数给予额外奖励，显式激励持续的多轮工具使用 按需上下文构建（On-demand Context Construction）：将视频仅加载至Python运行时，由模型在推理过程中通过代码选择性采样关键帧，显著降低视觉token使用量（从约45K降至5K）
## ThinkOmni: Lifting Textual Reasoning to Omni-modal Scenarios via Guidance Decoding【Visual Reasoning】

[https://arxiv.org/pdf/2602.23306](https://arxiv.org/pdf/2602.23306)

- **如何将大型推理模型（LRM）的文本推理能力有效扩展到全模态（omni-modal）场景，以克服现有全模态大语言模型（OLLM）在复杂推理方面的局限性，同时规避传统训练方法面临的数据稀缺与高计算成本问题**。论文提出**ThinkOmni**框架，通过**LRM-as-a-Guidance**策略实现无需训练的跨模态能力迁移，并引入**Stepwise Contrastive Scaling**模块自适应地平衡感知与推理信号，从而在保持解码效率的同时，将文本域的慢思考（slow thinking）能力_lift_到全模态场景。
## AgentVista: Evaluating Multimodal Agents in Ultra-Challenging Realistic Visual Scenarios【Visual Reasoning】

[https://arxiv.org/pdf/2602.23166](https://arxiv.org/pdf/2602.23166)

- 评估通用型多模态智能体在超难真实视觉场景中执行长程工具交互能力的基准测试。
## From Perception to Action: An Interactive Benchmark for Vision Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2602.21015](https://arxiv.org/pdf/2602.21015)

- 论文提出了 **CHAIN（Causal Hierarchy of Actions and Interactions）** 基准测试——一个交互式3D物理驱动测试平台，要求模型在物理引擎环境中迭代观察、选择可行交互并根据中间结果修正计划，从而评估其将感知到的结构转化为有效行动序列的能力。
## ⭐⭐A Very Big Video Reasoning Suite【Video Reasoning】

[https://arxiv.org/pdf/2602.20159](https://arxiv.org/pdf/2602.20159)

- 论文提出了VBVR（Very Big Video Reasoning）套件，包含： VBVR-Dataset：一个包含200个推理任务、超过100万视频片段（比现有数据集大三个数量级）的大规模训练资源，基于感知、变换、空间性、抽象和知识五大认知架构设计； VBVR-Bench：一个基于规则、与人类偏好对齐（Spearman相关系数ρ>0.9 ）的评估工具包，支持可验证的细粒度诊断； VBVR-Wan2.2：通过对Wan-2.2进行大规模数据训练得到的基线模型，用于开展首批视频推理扩展研究，揭示了数据规模与领域内/领域外泛化能力之间的量化关系。
## VISTA-Bench: Do Vision-Language Models Really Understand Visualized Text as Well as Pure Text?

[https://arxiv.org/pdf/2602.04802](https://arxiv.org/pdf/2602.04802)

- **视觉-语言模型（Vision-Language Models, VLMs）在处理****以像素形式呈现的可视化文本（visualized text）时，是否能够达到与处理纯文本（pure text）符号相当的语义理解水平**。几乎所有模型在可视化文本输入下性能下降，如NEO-9B-SFT整体下降30.8个百分点，LLaVA-OneVision-7B下降25.6个百分点；但部分强模型（GLM-4.1V-9B-Thinking、MiMo-VL-7B-RL）差距极小甚至持平。
## ReasonCACHE: Teaching LLMs To Reason Without Weight Updates【Meta】

[https://arxiv.org/pdf/2602.02366](https://arxiv.org/pdf/2602.02366)

- 在不更新任何参数、仅通过上下文学习（ICL）的前提下，大语言模型能否学会复杂推理？把“海量演示”离线蒸馏成可学习的 KV-cache 前缀（Prefix Tuning 的推理实例化，称 ReasonCache）： 每层引入 m 组可训练 key-value 向量，与 token 计算结果拼接进注意力 训练完仅保存 2mL 个向量，推理时一次载入，上下文长度与演示数量解耦 冻结 backbone，零遗忘、即插即拔。直接学成一小段可训练的注意力 KV-cache，像外挂记忆一样注入到 Transformer 里，从而在不改模型权重、也不占用长上下文的情况下，让大模型学会复杂推理。
## Small Generalizable Prompt Predictive Models Can Steer Efficient RL Post-Training of Large Reasoning Models【Tencent Hunyuan】

[https://arxiv.org/pdf/2602.01970](https://arxiv.org/pdf/2602.01970)

- 利用一个 小型、可泛化的预测模型（PPM：Prompt Predictive Model） 来估计每个 prompt 的“难度”（success rate／reward 预测），并据此进行高效的 RL 提示批次选择。
## STEMVerse: A Dual-Axis Diagnostic Framework for STEM Reasoning in Large Language Models

[https://arxiv.org/pdf/2602.02497](https://arxiv.org/pdf/2602.02497)

- 提出**"学科×认知"双轴能力矩阵**，将评估从结果排名转为能力光谱分析： 垂直轴（学术专业化）：覆盖数学、物理、化学、生物4大支柱，细分为27个子学科（如分析、量子力学、有机化学等），超越粗粒度学科标签 水平轴（认知复杂度）：基于布鲁姆分类法（Bloom's Taxonomy），设定6个层级（记忆、理解、应用、分析、评价、创造），刻画推理深度而非仅知识记忆。
## InftyThink+: Effective and Efficient Infinite-Horizon Reasoning via Reinforcement Learning【Long Horizon】

[https://arxiv.org/pdf/2602.06960](https://arxiv.org/pdf/2602.06960)

- 提出 **InftyThink+**，一种通过**端到端强化学习（RL）优化迭代推理**的框架，旨在解决大型推理模型在扩展思维链（Chain-of-Thought）时面临的**计算成本高、上下文长度限制及信息丢失**等核心挑战。现有迭代推理方法（如基于监督学习SFT或固定分块策略）仅能模仿格式，无法策略性地学习**何时压缩、如何压缩、如何基于压缩结果继续推理**。
## Limited Reasoning Space: The cage of long-horizon reasoning in LLMs【Long Horizon】

[https://arxiv.org/pdf/2602.19281](https://arxiv.org/pdf/2602.19281)

- 解决**LLMs在长程推理（long-horizon reasoning）任务中出现的性能崩溃问题**，即所谓的"过度规划"（Over-planning）现象。
## ToolMATH: A Math Tool Benchmark for Realistic Long-Horizon Multi-Tool Reasoning【Long Horizon】

[https://arxiv.org/pdf/2602.21265](https://arxiv.org/pdf/2602.21265)

- 为应对上述挑战，论文构建了TOOLMATH——一个基于数学推理的基准测试，主要结论包括： 工具可用性是必要非充分条件，**长程计划一致性与中间结果精确传播是可靠性的真正瓶颈**； 干扰工具的冗余效应主要通过放大早期微小偏差为执行漂移（execution drift）体现； 显式规划协议（Plan+ReAct）通过维护全局目标一致性，显著优于仅依赖局部行动选择或回溯搜索的方法。
## Learning to Self-Verify Makes Language Models Better Reasoners【LongCat】

[https://arxiv.org/pdf/2602.07594](https://arxiv.org/pdf/2602.07594)

- **不对称性现象**：当前LLMs虽能生成复杂推理路径，却难以可靠验证自身答案正确性。研究表明，标准生成训练（Learn to Generate）无法改善自验证能力，即使模型规模扩大或生成性能提升，自验证能力也不会自然涌现。论文揭示了一种逆向效应——**学习自验证（Learn to Self-Verify）能够显著提升生成性能**。仅通过训练模型判断自身答案的正确性（而不优化答案生成），即可在保持甚至超越标准生成训练准确率的同时，大幅减少推理所需的token数量（最高减少75%）。
## iGRPO: Self-Feedback-Driven LLM Reasoning【NVIDIA】

[https://arxiv.org/pdf/2602.09000](https://arxiv.org/pdf/2602.09000)

- Iterative Group Relative Policy Optimization (iGRPO)，通过两阶段机制实现动态自我条件化： 探索性草稿生成阶段：采样多个候选解并选择最高奖励的草稿作为"第一稿" 条件化细化阶段：将最佳草稿作为自我反馈附加到原始提示，训练策略生成超越该草稿的改进版本。
## Chain of Mindset: Reasoning with Adaptive Cognitive Modes

[https://arxiv.org/pdf/2602.10063](https://arxiv.org/pdf/2602.10063)

- 现有LLM推理方法（如Chain-of-Thought、Tree-of-Thoughts等）存在根本局限：**在单一推理过程中应用固定的同质认知策略**，无法像人类专家那样根据问题状态的演变动态切换思维模式。这导致模型在处理需要异构认知能力的复杂任务时性能受限。
## 🧐Reinforcing Chain-of-Thought Reasoning with Self-Evolving Rubrics【Self-Evolving, ByteDance Seed】

[https://arxiv.org/pdf/2602.10885v1](https://arxiv.org/pdf/2602.10885v1)

- 论文提出了RLCER（Reinforcement Learning with CoT Supervision via Self-Evolving Rubrics），让策略模型同时扮演推理者（reasoner）和评分标准生成者（rubricator），通过**自我生成的自然语言评分标准来监督CoT质量**，并基于标准满意度与答案正确性的相关性（corr(vk,z)>α ）来驱动评分标准的持续演化。
## ⭐The Token Games: Evaluating Language Model Reasoning with Puzzle Duels【Self-Evolving】

[https://arxiv.org/pdf/2602.17831](https://arxiv.org/pdf/2602.17831)

- 论文设计了一个自我维持的对抗性评估框架： 模型互相对抗：两个模型轮流扮演"提议者"（设计Python编程谜题）和"解决者"（寻找满足条件的输入） 自动化验证：利用编程谜题（Programming Puzzles）的可执行性自动验证答案正确性，无需人类标注 动态难度调节：模型根据对手表现实时调整谜题难度，确保评估无法被饱和（新模型总能提出更难问题超越旧模型） 双重能力评估：同时测量模型的解题能力（Solver Win Rate）和出题能力（Proposer Win Rate） 该框架**以极低成本（无需人类编题）实现了对推理、创造力和元认知能力的综合评估，且与Humanity’s Last Exam等昂贵基准的排序高度一致（Spearman相关系数达0.58-0.75）**。
## ⭐ReSyn: Autonomously Scaling Synthetic Environments for Reasoning Models【Self-Evolving】

[https://arxiv.org/pdf/2602.20117](https://arxiv.org/pdf/2602.20117)

- **如何自动、规模化地生成多样化的合成推理环境，以支持RLVR，从而突破现有方法在任务多样性和人工设计成本方面的限制**。利用大语言模型自动生成代码实现的推理环境，每个环境包含实例生成器 ρ0 、观察函数 O 和奖励函数 R 。 通过程序化的验证器（Verifier）而非参考答案提供奖励信号，允许指定超出模型自身求解能力的复杂任务。 将合成环境的数量扩展一个数量级以上（从约35个手工任务扩展到418个自动生成环境），显著提升任务多样性。
## When to Memorize and When to Stop: Gated Recurrent Memory for Long-Context Reasoning【Long Context, ByteDance Seed】

[https://arxiv.org/pdf/2602.10560](https://arxiv.org/pdf/2602.10560)

- 论文提出了**GRU-Mem（Gated Recurrent Memory）**框架，通过引入**更新门（Update Gate）**和**退出门（Exit Gate）**两个文本控制门控机制，分别控制"何时更新记忆"和"何时停止处理"，从而在保持推理性能的同时显著降低计算开销（最高可达400%的推理加速）。论文里的 **Update Gate / Exit Gate 本质上是： 一个基于当前 memory state + 当前 chunk 表征 的小型决策网络（通常是 MLP / 线性层 + sigmoid）**。
## ThinkRouter: Efficient Reasoning via Routing Thinking between Latent and Discrete Spaces

[https://arxiv.org/pdf/2602.11683](https://arxiv.org/pdf/2602.11683)

- 作者提出了 **ThinkRouter**，这是一种在推理时（Inference-time）根据模型信心进行动态路由的机制。
## Native Reasoning Models: Training Language Models to Reason on Unverifiable Data

[https://arxiv.org/pdf/2602.11549](https://arxiv.org/pdf/2602.11549)

- 论文提出Native Reasoning Training (NRT) 框架，旨在实现： 仅使用标准问答对（question-answer pairs）训练模型，无需专家编写的推理演示（z⋆ ） 无需外部验证器，通过内在奖励机制（intrinsic reward）引导模型生成能提升其对正确答案预测置信度的推理轨迹（z ） 将推理训练扩展到主观性、不可验证的领域，建立自增强的反馈循环，使模型学会通过"思考"解决自身不确定性。
## Knowing When Not to Answer: Abstention-Aware Scientific Reasoning

[https://arxiv.org/pdf/2602.14189](https://arxiv.org/pdf/2602.14189)

- 科学推理的主要挑战并非选择"最强模型"，而是**确定现有证据何时足以支持一个结论**。实验表明，不同架构的模型在原始准确率上差异有限，但通过置信度驱动的弃权机制，可在中等覆盖率水平下显著降低风险（错误率），这揭示了"知道何时不回答"对于科学可靠性的决定性作用。
## How Do Latent Reasoning Methods Perform Under Weak and Strong Supervision?

[https://arxiv.org/pdf/2602.22441](https://arxiv.org/pdf/2602.22441)

- 论文系统性地揭示了现有潜推理方法普遍存在**捷径依赖**现象：模型即使在不使用潜推理步骤（latent depth = 0）或潜表示被严重扰动的情况下，仍能保持较高的任务准确率。这表明模型可能通过表面线索或输入-输出的直接映射来"作弊"，而非真正执行多步推理过程。研究进一步分析了**弱监督**（仅依赖最终答案的反馈）与**强监督**（对中间潜状态进行细粒度约束）如何影响这种捷径行为的发生程度。论文揭示了监督强度与潜推理特性之间的根本性权衡：强监督（如SIM-CoT、CoLaR）通过强制对齐中间潜状态与文本推理步骤，有效抑制捷径行为，但限制了潜表示维持多样化假设的能力；弱监督（如Coconut、CODI）允许潜状态编码更丰富的候选轨迹，但更容易产生捷径依赖简言之，该工作通过一系列干预实验和注意力分析，挑战了潜推理方法"自动实现高效并行搜索"的乐观假设，并指出了当前方法在**推理忠实性（faithfulness）与搜索多样性（diversity）**之间的内在张力。

# Reward Model

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Model
- 方法：agent, reasoning
- 论文/报告：4 篇
- Joint Reward Modeling: Internalizing Chain-of-Thought for Efficient Visual Reward Models
- When Is Compositional Reasoning Learnable from Verifiable Rewards?
- CODE-SHARP: Continuous Open-ended Discovery and Evolution of Skills as Hierarchical Reward Programs
- CodeScaler: Scaling Code LLM Training and Test-Time Inference via Execution-Free Reward Models【Code】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Joint Reward Modeling: Internalizing Chain-of-Thought for Efficient Visual Reward Models

[https://arxiv.org/pdf/2602.07533](https://arxiv.org/pdf/2602.07533)

- 论文提出**联合奖励建模（Joint Reward Modeling, JRM）**，通过在共享的视觉-语言主干上联合优化偏好排序损失与语言建模损失，将生成式推理能力"内化"为判别式表示，从而在推理阶段仅保留高效的判别式评分路径，实现"训练时吸收推理能力，推理时保持判别效率"的解耦设计。**能否在不牺牲偏好建模精度与推理效率的前提下，****赋予判别式奖励模型以生成式模型所具备的深层语义理解与逻辑推理能力****？**在图像编辑等复杂生成任务中，奖励模型必须超越表层相似性度量，同时验证：全局语义一致性（编辑前后图像的跨区域语义匹配）隐式逻辑约束（如空间关系、物理合理性、指令细粒度遵循）
## When Is Compositional Reasoning Learnable from Verifiable Rewards?

[https://arxiv.org/pdf/2602.07992](https://arxiv.org/pdf/2602.07992)

- **在仅提供最终结果验证（outcome-level feedback）的情况下，RLVR何时能够成功学习到正确的组合推理（compositional reasoning）链，以及何时会失败**。**RLVR 本身并不保证学习到正确的中间推理**；**其成功完全取决于任务优势比这一联合属性——它既依赖于问题的归纳结构（是否奖励部分正确步骤），也依赖于基础模型质量（能否使正确步骤具有统计优势）**。当该优势缺失时，即使无表示或统计障碍，RLVR 也可能失败。
## CODE-SHARP: Continuous Open-ended Discovery and Evolution of Skills as Hierarchical Reward Programs

[https://arxiv.org/pdf/2602.10085](https://arxiv.org/pdf/2602.10085)

- 解决**自主智能体的开放式技能发现与奖励函数自动化设计**这一核心挑战。论文提出了 CODE-SHARP 框架，其核心创新在于： 技能即层次化奖励程序（SHARPs）：将技能定义为可执行的Python程序，既包含成功条件（success condition），又包含指向先决条件技能的依赖链 双循环开放式演化：通过FM驱动的技能提议-实现-评判循环发现新技能，同时通过变异-评估循环持续优化现有技能 单一目标条件策略训练：训练一个目标条件策略（goal-conditioned agent），仅基于发现的SHARP技能生成的奖励信号进行学习，从而无需人工设计的奖励函数即可掌握复杂的长程目标
## CodeScaler: Scaling Code LLM Training and Test-Time Inference via Execution-Free Reward Models【Code】

[https://arxiv.org/pdf/2602.17684](https://arxiv.org/pdf/2602.17684)

- 解决**基于可验证奖励的强化学习（RLVR）在代码大语言模型（Code LLMs）训练和推理中的可扩展性瓶颈**，提出一种无执行奖励模型（Execution-Free Reward Model），使其能够： 在训练阶段替代二进制执行反馈，支持在合成数据上进行稳定、可扩展的RL训练； 在推理阶段作为高效的Best-of-N选择器，在保持与单元测试方法相当性能的同时，实现10倍的延迟降低。

# Data

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data
- 方法：agent, ai-for-science, language, reasoning, reinforcement-learning, optimization, multimodal-learning, multimodal-reasoning
- 论文/报告：17 篇
- ⭐⭐Agentic Proposing: Enhancing Large Language Model Reasoning via Compositional Skill Synthesis【Alibaba】
- R1-SyntheticVL: Is Synthetic Data from Generative Models Ready for Multimodal Large Language Model?【Data Synthesis】
- Not All Negative Samples Are Equal: LLMs Learn Better from Plausible Reasoning【Data Synthesis】
- ⭐⭐UniGeM: Unifying Data Mixing and Selection via Geometric Exploration and Mining【Data Mixing】
- ⭐⭐Decouple Searching from Training: Scaling Data Mixing via Model Merging for Large Language Model Pre-training【Data Mixing】
- Learning More from Less: Unlocking Internal Representations for Benchmark Compression
- Not All Preferences Are Created Equal: Stability-Aware and Gradient-Efficient Alignment for Reasoning Models
- SparseEval: Efficient Evaluation of Large Language Models by Sparse Optimization
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## ⭐⭐Agentic Proposing: Enhancing Large Language Model Reasoning via Compositional Skill Synthesis【Alibaba】

[https://arxiv.org/pdf/2602.03279](https://arxiv.org/pdf/2602.03279)

- **LLM在复杂推理（特别是数学、编程和科学推理）领域中高质量训练数据稀缺且难以规模化合成。**论文提出将问题合成重新概念化为组合式逻辑工程（compositional logic engineering）过程，通过引入Agentic Proposing框架，使 specialized agent 能够通过迭代式内部反思（internal reflection）和工具使用（tool-use），动态地选择并组合原子化推理技能（atomic reasoning skills），从而自主合成既逻辑严谨又难度精确可控的可验证问题。
## R1-SyntheticVL: Is Synthetic Data from Generative Models Ready for Multimodal Large Language Model?【Data Synthesis】

[https://arxiv.org/pdf/2602.03300](https://arxiv.org/pdf/2602.03300)

- 针对MLLMs的数据合成方法探索不足，现有方法多局限于为已有图像合成文本模态，或依赖程序化引擎生成特定视觉结构，缺乏能够自主合成高质量、多样化且具挑战性多模态数据的通用方法。论文提出了Collective Adversarial Data Synthesis (CADS)框架，**通过集体对抗学习机制**，自主合成高质量、多样化且具有挑战性的多模态训练数据，以缓解数据稀缺问题并增强MLLMs解决复杂现实世界任务的能力。
## Not All Negative Samples Are Equal: LLMs Learn Better from Plausible Reasoning【Data Synthesis】

[https://arxiv.org/pdf/2602.03516](https://arxiv.org/pdf/2602.03516)

- 现有LLM训练范式（如监督微调和强化学习）主要依赖正样本，或将所有错误响应视为同等价值的负样本。然而，不同错误携带的学习信号存在本质差异：**"合理但错误"（plausible but wrong）的样本——即推理过程连贯、格式规范但最终结论错误的样本——比明显错误的样本能提供更丰富的学习信号，迫使模型发展细粒度的推理判别能力。**现有方法未能系统性合成此类样本，因其面临**正确性与合理性的内在冲突**：强推理能力天然倾向于生成正确答案，而强行破坏逻辑结构会产生易被检测的伪影。
## ⭐⭐UniGeM: Unifying Data Mixing and Selection via Geometric Exploration and Mining【Data Mixing】

[https://arxiv.org/pdf/2602.03772](https://arxiv.org/pdf/2602.03772)

- 传统方法将**宏观域混合**（macro-distribution balancing）与**微观实例选择**（micro-quality selection）割裂处理，导致代码等结构化语料的层级依赖与逻辑一致性被破坏；同时，现有方法依赖外部参考数据或昂贵的代理模型，引入偏差且计算开销大。**Global (Macro) clustering **主要解决 **“每一类数据占多少比例”**（= _cluster-level mixing weight_）**Sub-cluster (Micro) mining **主要解决 **“这一类里面，哪些样本该留下 / 权重大”**（= _instance-level selection_）
## ⭐⭐Decouple Searching from Training: Scaling Data Mixing via Model Merging for Large Language Model Pre-training【Data Mixing】

[https://arxiv.org/pdf/2602.00747](https://arxiv.org/pdf/2602.00747)

- 提出 DeMix 框架，把“配比搜索”与“模型训练”解耦：** 仅训练少量“领域分量模型”（component models）； 通过加权模型合并（weighted model merging）在零额外训练成本下生成无限代理模型**； 用合并代理的 benchmark 排名回归预测最优配比，实现“搜得充分、测得准确、花得少”。先分别在不同数据源上训练“组件模型”，再通过线性模型合并来近似任意数据混合比例下的模型效果，从而把“数据配比搜索”从昂贵的训练过程里解耦出来，用极低成本高效优化预训练数据混合。
## Learning More from Less: Unlocking Internal Representations for Benchmark Compression

[https://arxiv.org/pdf/2602.00710](https://arxiv.org/pdf/2602.00710)

- 解决**LLMs评估中的基准测试压缩问题，特别是在源模型稀缺（source-scarce）场景下的局限性**。论文提出 REPCORE 框架，通过以下机制实现"从更少数据中学习更多"： 跨模型表示学习：利用模型特定的线性投影层将异构隐藏状态映射到统一潜在空间，恢复项目空间的细粒度几何结构； 基于共识的聚类：在聚合的对齐表示上进行聚类，选择代表性锚点项目构建核心集； 轻量级外推：基于核心集上的局部观测，通过岭回归 extrapolate 到完整基准性能。 实验表明，该方法**在仅使用10个源模型的情况下，即可在五个跨模态基准和200余个模型上实现准确的性能估计和排名恢复**，显著优于基于输出的基线方法。
## Not All Preferences Are Created Equal: Stability-Aware and Gradient-Efficient Alignment for Reasoning Models

[https://arxiv.org/pdf/2602.01207](https://arxiv.org/pdf/2602.01207)

- 偏好对的学习价值并非固有属性，而是与模型演化状态（evolving policy state）动态耦合。早期提供强监督的样本在后期可能收益递减（diminishing returns），而静态数据选择无法适应这种变化。论文提出 SAGE（Stability-Aware Gradient Efficiency），核心解决思路包括： 粗粒度课程机制：通过可刷新的候选池（refreshable pools）根据模型能力动态调整数据分布，实现难度自适应的课程学习（curriculum learning）。 细粒度稳定性感知评分：引入基于牛顿 decrement 启发的 SAGE Score。
## SparseEval: Efficient Evaluation of Large Language Models by Sparse Optimization

[https://arxiv.org/pdf/2602.07909](https://arxiv.org/pdf/2602.07909)

- SparseEval 方法旨在**仅用极少量的测试项目（如100个）即可准确预测模型在整个基准上的性能。**把模型评估形式化为一个**稀疏重构问题**，证明只需从评测集里选出一小撮具有代表性的样本（anchors），并学习它们的权重，就能以很小的误差重建“全量评估”的结果；具体做法是**用一个轻量模型（如 MLP）从少量 anchor 的模型响应中预测整体性能，并通过梯度驱动的 anchor 重要性评估 + 迭代替换，不断优化这组样本**。结果是：在保持模型排名和分数高度一致的前提下，把评估成本从“全量推理”压缩到“百级样本”，本质上揭示了 **LLM benchmark 评估存在低秩/稀疏结构**，评估可以被系统性地“学习和加速”。
## ⭐⭐Data Darwinism Part I: Unlocking the Value of Scientific Data for Pre-training【Pengfei Liu】

[https://arxiv.org/pdf/2602.07824](https://arxiv.org/pdf/2602.07824)

- 论文提出**数据-模型协同进化**（co-evolutionary）的视角：**更强大的模型应能驱动更复杂的数据处理技术（如使用前沿LLM进行质量评估、内容重写和推理增强），而这些处理后的高质量数据又用于训练下一代模型。这一框架试图建立数据质量与模型能力之间的正向反馈机制，而非将数据质量视为静态属性**。通过**构建 Darwin-Science（900B token 经L0–L5处理的科学语料库）**和 **Darwin-Science-Eval（150K专家级评估问题）**，以及从头训练的无污染基线模型（daVinci-origin-3B/7B），论文验证了这一层级化处理框架能够有效释放科学数据的潜在价值，解决高密度概念领域数据难以被模型有效学习的问题。
## ⭐⭐Data Science and Technology Towards AGI Part I: Tiered Data Management【OpenBMB】

[https://arxiv.org/pdf/2602.09003](https://arxiv.org/pdf/2602.09003)

- 论文强调一个关键理念：**不是单纯按照事后规则筛选数据，而是让大模型参与数据评价和加工过程**。 这包括： 利用 LLM 为数据打质量分（quality scoring） 对不良数据进行自动编辑 / 生成更高价值语料 模型反馈驱动数据清洗和层级迁移 因此这是一种 “模型 ↔ 数据”的双向循环”：模型提升帮助优化数据，优质数据反过来提升模型表现。
## ⭐⭐DataChef: Cooking Up Optimal Data Recipes for LLM Adaptation via Reinforcement Learning【AI Lab】

[https://arxiv.org/pdf/2602.11089](https://arxiv.org/pdf/2602.11089)

- 论文正式提出了**面向LLM适配的端到端数据配方生成任务**：给定目标任务描述、评估协议和可用原始数据源池，**模型需自动生成完整的数据配方——包括可执行的数据处理流程（代码实现）及其产出的训练数据集——以适配基础LLM到特定下游任务**。该任务要求模型具备强大的推理能力，能够分析异构数据源、应用领域特定的处理操作，并生成相应的可执行代码。针对某个 benchmark，自动整理/生成最优 SFT 数据，使模型在该 benchmark 上得分更高。policy model 直接生成可执行的数据处理 pipeline（Python 脚本）以及验证脚本。
## ⭐⭐Olmix: A Framework for Data Mixing Throughout LM Development

[https://arxiv.org/pdf/2602.12237](https://arxiv.org/pdf/2602.12237)

- 针对LM开发全过程的数据混合（data mixing）框架， 提供一个自动化的框架，通过少量的代理实验预测最佳配比，从而降低计算成本并提升模型在目标任务上的性能 。Olmix 框架的核心逻辑正是“**小模型采样预测 + 大模型配比优化**”。选择规模极小的**代理模型**（例如 30M 到 1B 参数量），随机生成数十种甚至上百种不同的数据配比组合（比如：组合 A 是 80% 网页 + 20% 代码；组合 B 是 50% 网页 + 50% 代码），用这些配比在小模型上进行短时间的训练（通常只训练几千个步数），记录每种配比下小模型的验证集损失（Loss）或下游任务表现 。利用收集到的“配比-性能”数据对，训练一个回归模型（如 LightGBM 或简单的多线性回归） 。回归模型不仅能预测小模型，还能结合**缩放法则**来预测更大规模模型在相同配比下的表现 。
## SkillRater: Untangling Capabilities in Multimodal Data

[https://arxiv.org/pdf/2602.11615](https://arxiv.org/pdf/2602.11615)

- 针对**数据筛选中单一标量质量评分的结构性局限**问题，提出并验证了一种能力对齐的多维筛选框架。
## ⭐Golden Goose: A Simple Trick to Synthesize Unlimited RLVR Tasks from Unverifiable Internet Text【NVIDIA】

[https://arxiv.org/pdf/2601.22975](https://arxiv.org/pdf/2601.22975)

- 把 **“Next Token Prediction”（预测下一个词）** 升级成了 **“Next Logic Verification”（验证下一段逻辑）**。**原始语料 -> 挖掉一段 -> 找模型生成几个像样的假选项 -> 混合成选择题 -> 强化学习（选对给分，选错零分）。**
## ⭐Joint Selection for Large-Scale Pre-Training Data via Policy Gradient-based Mask Learning【ByteDance Seed】

[https://arxiv.org/pdf/2512.24265](https://arxiv.org/pdf/2512.24265)

- 将数据选择过程建模为一个“掩码学习”（mask learning）问题。使用**策略梯度（Policy Gradient）**方法进行优化：通过迭代采样数据掩码（mask），计算基于预定义目标的梯度，并更新掩码采样的概率（logits）。它是在给每一条数据打分，决定这条数据是“保留（1）”还是“丢弃（0）”。
## ⭐⭐VeRA: Verified Reasoning Data Augmentation at Scale【ByteDance Seed】

[https://arxiv.org/pdf/2602.13217](https://arxiv.org/pdf/2602.13217)

- 针对**静态推理基准测试（static benchmarks）的结构性失效**问题，提出了**VeRA（Verified Reasoning Data Augmentation）**框架，实现了从"固定题库"到"可执行规范"的范式转变。VeRA-E（等价变体）：保持底层逻辑不变，仅改变数值、命名、语言等表面形式，用于检测记忆化与评估鲁棒性； VeRA-H / VeRA-H Pro（硬化变体）：系统性增加问题复杂度（约束收紧、步骤组合、逻辑扩展），同时保持可验证性，用于在模型能力边界生成可靠的高难度任务。VeRA-H Pro 从 K 个候选中选择最难的逐种子配对变体。
## When AI Benchmarks Plateau: A Systematic Study of Benchmark Saturation

[https://arxiv.org/pdf/2602.16763](https://arxiv.org/pdf/2602.16763)

- 系统研究了**LLM基准测试的饱和现象**（benchmark saturation），即顶尖模型性能趋于一致、失去统计区分能力的问题。**核心结论**：基准饱和主要由**时间暴露累积**和**测量分辨率限制**驱动，而非孤立的设计缺陷；常见的"保密"或"开放式"策略并不能有效延长基准寿命，需通过结构性设计（**更大规模、动态更新、专家深度策划**）和主动生命周期管理来维持评估生态的区分力。

# Foundation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Foundation
- 方法：agent, reasoning, vision, reinforcement-learning, multimodal-learning, multimodal-reasoning, stat-ml, deep-learning
- 论文/报告：11 篇
- EUGens: Efficient, Unified, and General Dense Layers【DeepMind】
- Graph is a Substrate Across Data Modalities
- Delving into Muon and Beyond: Deep Analysis and Extensions
- OneVision-Encoder: Codec-Aligned Sparsity as a Foundational Principle for Multimodal Intelligence
- Improving Reconstruction of Representation Autoencoder
- Do We Need Adam? Surprisingly Strong and Sparse Reinforcement Learning with SGD in LLMs
- SpiralFormer: Looped Transformers Can Learn Hierarchical Dependencies via Multi-Resolution Recursion
- STAR : Bridging Statistical and Agentic Reasoning for Large Model Performance Prediction
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## EUGens: Efficient, Unified, and General Dense Layers【DeepMind】

[https://arxiv.org/pdf/2601.22563](https://arxiv.org/pdf/2601.22563)

- 用随机特征把传统全连接前馈层（FFL）的 **权重-输入耦合计算** 解耦为 **线性核**，实现 **无偏近似、线性复杂度、零样本蒸馏** 三大突破，并在多模态大模型上取得一致加速。
## Graph is a Substrate Across Data Modalities

[https://arxiv.org/pdf/2601.22384](https://arxiv.org/pdf/2601.22384)

- **图结构在不同数据模态和任务中被反复独立构建、丢弃，导致跨模态、跨任务的结构性规律无法持续积累与复用。**论文提出一个表示中心（representation-centric）视角：将图结构视为跨模态、跨任务持续存在的“结构基底”（structural substrate），并设计 G-Substrate 框架，通过以下两个互补机制实现图表示的可持续复用： 统一结构模式（unified structural schema）：把来自异构模态/任务的图映射到同一结构空间，确保节点、边类型与连通规则一致，解决“结构异构”问题。 交错角色训练（interleaved role-based training）：让同一张图在训练过程中交替承担“生成结构”与“理解结构”两种功能角色，迫使图表示必须在多种任务上下文中保持可用，从而抑制任务专属捷径，强化跨任务通用的结构规律。
## Delving into Muon and Beyond: Deep Analysis and Extensions

[https://arxiv.org/pdf/2602.04669](https://arxiv.org/pdf/2602.04669)

- 论文将Muon重新定位为一种有效的谱归一化形式而非 universally superior 的优化器。其正交化操作（p=0 ）虽能稳定一阶矩更新，但在现代自适应优化器（Adam）已引入RMS归一化的场景下，适度的谱压缩（如 p=1/2 ）往往比完全正交化更优。这提示：谱变换应与二阶矩归一化结合，而非作为其替代。
## OneVision-Encoder: Codec-Aligned Sparsity as a Foundational Principle for Multimodal Intelligence

[https://arxiv.org/pdf/2602.08683](https://arxiv.org/pdf/2602.08683)

- 论文指出当前视觉架构的根本缺陷：视频信号具有**高度时空冗余性**（静态背景占主导），而**判别性信息（运动、残差）呈稀疏分布**。然而，主流模型（如标准ViT）对密集像素网格进行均匀计算，将大量容量浪费在可预测区域。论文基于"通用智能本质是压缩问题"的假设，提出**编解码器对齐的稀疏性（Codec-Aligned Sparsity）应作为视觉表征学习的基础原则——即模仿HEVC/H.265等视频编解码器将信号分解为空间完整的I帧**与**预测性P帧**（仅编码运动残差）的机制，**仅在富含信息熵的区域（3.1%-25% patch）分配计算资源。**
## Improving Reconstruction of Representation Autoencoder

[https://arxiv.org/pdf/2602.08620](https://arxiv.org/pdf/2602.08620)

- 论文提出**LV-RAE（Local-Variations Augmented Representation Autoencoder）**框架： **这篇论文解决了“用强语义表示（如预训练视觉模型）做 latent diffusion 时，图像重建差、生成易出伪影”的问题，方法是给 Representation Autoencoder 的 latent 补回必要的低级细节信息，并专门训练一个对噪声扰动更鲁棒的 decoder，使高维语义 latent 既能高保真重建、又能稳定用于扩散生成。latent 被拆成 “语义坐标（语义模型给） + 重建残差（RAE 学）”**
## Do We Need Adam? Surprisingly Strong and Sparse Reinforcement Learning with SGD in LLMs

[https://arxiv.org/pdf/2602.07729](https://arxiv.org/pdf/2602.07729)

- **SGD在RLVR中的有效性、优越性及独特机制**。核心发现 1. SGD性能匹配甚至超越AdamW 尽管SGD被认为不适用于大规模Transformer训练，但在数学推理、代码生成及RLVE等多任务域中，SGD在Qwen和Llama模型家族上均达到与AdamW相当或更优的性能。动量（SGD+Momentum）和自适应学习率（RMSProp）的消融实验表明，这两个AdamW的核心组件在RLVR中并非必需，且动量常因非平稳环境而失效。 2. 极端参数更新稀疏性 SGD在无任何显式稀疏正则化的情况下，仅更新少于0.02%的模型参数，较AdamW（约10%）稀疏度提升逾1,000倍。这种稀疏性均匀分布于所有层，且随训练进行几乎不衰减。同时，SGD参数更新的有效秩（约25）显著低于AdamW（约88），表明RLVR在极低维子空间中进行优化。 3. 显著内存效率提升 SGD将优化器状态从AdamW的O(3n) （FP32权重+一阶矩+二阶矩）降至O(n) （仅FP32权重），在Qwen3-1.7B模型上实现15.7 GB的峰值显存节省，无需牺牲精度即可支持更大批次或模型规模。
## SpiralFormer: Looped Transformers Can Learn Hierarchical Dependencies via Multi-Resolution Recursion

[https://arxiv.org/pdf/2602.11698](https://arxiv.org/pdf/2602.11698)

- 解决**循环（looped）Transformer架构在固定、全token分辨率下操作导致的计算效率瓶颈与表示能力局限**问题。
## STAR : Bridging Statistical and Agentic Reasoning for Large Model Performance Prediction

[https://arxiv.org/pdf/2602.12143](https://arxiv.org/pdf/2602.12143)

- **“模型性能预测”**（从有限的观测数据预测模型在未测试任务上的表现）。
## Sanity Checks for Sparse Autoencoders: Do SAEs Beat Random Baselines?

[https://arxiv.org/pdf/2602.14111](https://arxiv.org/pdf/2602.14111)

- **Sparse Autoencoders (SAEs) 是否真正学习到有意义的特征分解？**尽管 SAEs 在重构保真度（explained variance）、可解释性（AutoInterp）、稀疏探测（sparse probing）和因果编辑（RAVEL）等标准指标上表现良好，但论文试图验证这些性能是源于**有意义的特征学习**，还是仅仅源于**大规模过完备字典的统计特性**和**对随机初始方向的微小调整**。
## You Can Learn Tokenization End-to-End with Reinforcement Learning

[https://arxiv.org/pdf/2602.13940](https://arxiv.org/pdf/2602.13940)

- 一种基于RL的端到端分词（tokenization）学习方法，将传统大语言模型中硬编码的文本压缩步骤转化为可微分的离散决策优化问题。
## Are Object-Centric Representations Better At Compositional Generalization?

[https://arxiv.org/pdf/2602.16689](https://arxiv.org/pdf/2602.16689)

- **对象中心（Object-Centric, OC）视觉表示是否比传统的密集（dense）视觉表示更适合实现组合泛化（compositional generalization）**，即模型对熟悉概念的新组合进行推理的能力。OC表示在难泛化设置中占优：在hard难度（训练多样性最低）下，OC表示的COOD准确率显著高于密集表示；仅在easy设置且配备大型下游模型时，密集表示才能匹敌或略微超越。 计算效率优势：在有限下游计算预算（<4 PFLOPs）下，OC表示 consistently 实现更高COOD准确率；密集表示需消耗3倍以上计算资源才能在简单任务上取得小幅领先。 样本效率优势：OC表示在少样本（103 –104 张图像）场景下泛化能力显著更强，密集表示仅在数据量充足（40 k张）、多样性高且下游模型大时才能追上。 Slot Attention不可替代：简单的k-means聚类或Cross-Attention压缩密集表示，在组合泛化上均不及经过预训练的Slot Attention瓶颈。

# Generation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Generation
- 方法：agent, generation, language, vision-language-model, reasoning, vision, reinforcement-learning, multimodal-learning
- 论文/报告：19 篇
- Generative Modeling via Drifting【Kaiming He】
- Research on World Models Is Not Merely Injecting World Knowledge into Specific Tasks
- Mind-Brush: Integrating Agentic Cognitive Search and Reasoning into Image Generation
- Unified Personalized Reward Model for Vision Generation【Jiaqi Wang, SII】
- UniReason 1.0: A Unified Reasoning Framework for World Knowledge Aligned Image Generation and Editing【Jiaqi Wang, SII】
- GenArena: How Can We Achieve Human-Aligned Evaluation for Visual Generation Tasks?【Jiaqi Wang, SII】
- RISE-Video: Can Video Generators Decode Implicit World Rules?【World Model】
- WorldCompass: Reinforcement Learning for Long-Horizon World Models【World Model】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Generative Modeling via Drifting【Kaiming He】

[https://arxiv.org/pdf/2602.04770](https://arxiv.org/pdf/2602.04770)

- **Drifting Models（漂移模型）**，一种全新的生成建模范式。在分布空间中给样本施加一个“漂流场”， 让它们从随机初始分布不断流动到真实数据分布， 并让神经网络学习这个漂流场，使 一步就能到达最终状态。**学一个以真实数据分布为不动点（fixed point）的分布动力系统。**
## Research on World Models Is Not Merely Injecting World Knowledge into Specific Tasks

[https://arxiv.org/pdf/2602.01630](https://arxiv.org/pdf/2602.01630)

- 世界模型研究被简化为“给孤立任务注入世界知识”，导致碎片化、缺乏系统一致性，难以形成通用物理理解。提出统一设计规范——由 Interaction、Reasoning、Memory、Environment、Multimodal Generation 五模块组成的闭环框架，定义接口与更新协议，支持模块化热插拔与持续学习。
## Mind-Brush: Integrating Agentic Cognitive Search and Reasoning into Image Generation

[https://arxiv.org/pdf/2602.01756](https://arxiv.org/pdf/2602.01756)

- **Mind-Brush**——一个无需额外训练的统一智能体框架，将图像生成转变为“**先思考-再调研-后创作**”的动态知识驱动流程，并配套发布 **Mind-Bench** 基准，用于严格评估模型在**实时知识检索**与**多步推理**条件下的生成能力。认知缺口检测 → 自适应搜索/推理 → 证据整合 → 约束生成 零训练即可把开源 Qwen-Image 基线 CSA 从 0.02 → 0.31，超越 GPT-Image-1.5 47.6%
## Unified Personalized Reward Model for Vision Generation【Jiaqi Wang, SII】

[https://arxiv.org/pdf/2602.02380](https://arxiv.org/pdf/2602.02380)

- 作者提出 **UnifiedReward-Flex**，通过**上下文自适应的层次化推理**，在运行时动态实例化细粒度评估标准，并可在必要时新增高层维度，以贴合人类评估者的内容自适应决策过程，最终为图像与视频合成提供更可靠、更具语境感知能力的奖励监督。
## UniReason 1.0: A Unified Reasoning Framework for World Knowledge Aligned Image Generation and Editing【Jiaqi Wang, SII】

[https://arxiv.org/pdf/2602.02437](https://arxiv.org/pdf/2602.02437)

- **UniReason**——一个面向“文本到图像生成（T2I）”与“图像编辑”的统一推理框架，核心思想是：**把生成当作规划、把编辑当作精修**，用同一套模型参数完成“世界知识推理 + 视觉自反思”闭环。
## GenArena: How Can We Achieve Human-Aligned Evaluation for Visual Generation Tasks?【Jiaqi Wang, SII】

[https://arxiv.org/pdf/2602.06013](https://arxiv.org/pdf/2602.06013)

- 论文提出GENARENA框架，通过成对比较范式（Pairwise Comparison）替代绝对评分，结合Elo评分系统聚合比较结果，实现了： 评估准确率提升超过 20% 与人类权威榜单（LMArena）的Spearman相关性达到 0.86 （远超点对点方法的 0.36 ） 无需微调即可使开源VLM（如Qwen3-VL）超越专有模型（如GPT-5）的评估性能。**这篇论文的核心观点是：视觉生成模型的评估不该用“给单个结果打分”，而应像人类一样做“两两对比”，作者提出的 GenArena 用 pairwise comparison 显著提高了自动评估与人类偏好的一致性。**
## RISE-Video: Can Video Generators Decode Implicit World Rules?【World Model】

[https://arxiv.org/pdf/2602.05986](https://arxiv.org/pdf/2602.05986)

- **RISE-Video**——一个专门面向推理的TI2V基准测试，通过467个人工标注样本和四个互补评估维度（推理对齐、时间一致性、物理合理性、视觉质量），系统性地评估模型在八类隐式推理任务上的表现，旨在推动视频生成模型从"视觉逼真"向"认知合理"演进。
## WorldCompass: Reinforcement Learning for Long-Horizon World Models【World Model】

[https://arxiv.org/pdf/2602.09022](https://arxiv.org/pdf/2602.09022)

- 针对**长程、交互式视频世界模型**（long-horizon, interactive video-based world models）的后训练（post-training）问题，提出通过强化学习（RL）增强模型基于交互信号探索世界的能力。通过引入 **WorldCompass** 框架，论文旨在通过强化学习后训练，使世界模型能够更直接地利用交互信号，在保持高视觉质量的同时，显著提升对复杂动作序列（包括复合动作）的跟随精度。
## WorldArena: A Unified Benchmark for Evaluating Perception and Functional Utility of Embodied World Models【World Model】

[https://arxiv.org/pdf/2602.09022](https://arxiv.org/pdf/2602.09022)

- 解决**具身世界模型（Embodied World Models, EWMs）评估体系碎片化且片面**的问题。论文提出**WorldArena**——**首个整合感知与功能评估的统一基准，通过16项视频质量指标（覆盖视觉、运动、物理、3D、可控性等6维度）、3类具身下游任务（数据引擎、策略评估、动作规划）及主观人工评估，系统衡量世界模型在"看得真"与"用得上"两方面的综合性能**，揭示当前模型普遍存在的**感知-功能鸿沟**（perception–functionality gap）。
## VideoWorld 2: Learning Transferable Knowledge from Real-world Videos【World Model, ByteDance Seed】

[https://arxiv.org/pdf/2602.10102](https://arxiv.org/pdf/2602.10102)

- 论文提出**VideoWorld 2**框架，通过动态增强的潜在动态模型（dLDM*显式解耦视觉外观建模与动作动态学习：利用预训练视频扩散模型（VDM）承担外观生成任务，使潜在编码专注于紧凑、语义化的任务相关动态，从而实现从原始视频到可执行策略的有效知识迁移。
## Compositional Planning with Jumpy World Models【World Model, Meta】

[https://arxiv.org/pdf/2602.19634](https://arxiv.org/pdf/2602.19634)

- 提出**组合规划与跳跃式世界模型（Compositional Planning with Jumpy World Models）**，旨在通过组合现有预训练策略解决复杂的长程决策任务，而无需额外的环境交互或任务特定训练。
## Tele-Omni: a Unified Multimodal Framework for Video Generation and Editing【Tele AI】

[https://arxiv.org/pdf/2602.09609](https://arxiv.org/pdf/2602.09609)

- 论文提出Tele-Omni，一个统一的多模态框架，通过以下机制解决上述问题：架构解耦：利用预训练MLLM解析异构指令，DiT执行视频合成，实现指令理解与生成过程的分离统一指令格式：设计任务感知的数据处理流水线，将多模态输入（文本、图像、视频）统一为结构化指令表示联合训练策略：支持文本到视频、图像到视频、首末帧生成、上下文生成与编辑等多样化任务的联合训练通过这种方式，Tele-Omni在单一模型内实现了对多模态指令（文本、图像、参考视频）的灵活遵循，同时保持强时间连贯性与视觉一致性。
## ⭐Autoregressive Image Generation with Masked Bit Modeling

[https://arxiv.org/pdf/2602.09024](https://arxiv.org/pdf/2602.09024)

- 离散方法表现劣势不是因为本质技术限制，而是由于信息容量（bit 预算）不足导致。
## DeepGen 1.0: A Lightweight Unified Multimodal Model for Advancing Image Generation and Editing【Unified Model, SII, Jiaqi Wang】

[https://arxiv.org/pdf/2602.12205](https://arxiv.org/pdf/2602.12205)

- **DeepGen 1.0**，一个仅含 **5B 参数**（3B VLM + 2B DiT）的轻量级统一多模态模型，旨在解决当前图像生成与编辑模型参数规模过大（通常>10B）、训练成本高昂的问题，同时挑战"紧凑模型无法实现全面多模态能力"的固有认知。
## Code2Worlds: Empowering Coding LLMs for 4D World Generation

[https://arxiv.org/pdf/2602.11757](https://arxiv.org/pdf/2602.11757)

- 解决**基于代码生成范式的4D物理世界生成**问题，即如何从自然语言指令生成具备物理一致性的动态三维环境。论文提出了 **Code2Worlds** 框架，将 4D 生成任务转化为“**语言到仿真代码生成（Language-to-Simulation Code Generation）**”。通过生成代码来驱动物理引擎（如 Python API 调用的仿真环境），从而确保生成的 4D 世界具有物理准确性且可交互。
## UniWeTok: An Unified Binary Tokenizer with Codebook Size 2^128 for Unified Multimodal Large Language Model【Unified Model, ByteDance】

[https://arxiv.org/pdf/2602.14178](https://arxiv.org/pdf/2602.14178)

- 提出了 UniWeTok——一种基于 128 位二进制 latent 表示 的统一视觉 tokenizer，通过构建隐式规模达 2^128 的指数级码本，**在不显著增加参数量的情况下大幅提升视觉离散表示容量，同时配套设计了生成感知的训练机制（如蒸馏策略与生成先验约束）来稳定二进制表示训练**，使其既能高保真重建图像，又能更好支持图像生成与多模态大模型对齐，从而为统一视觉–语言模型提供了一种兼顾理解与生成的高容量离散表征方案。
## 🧐BitDance: Scaling Autoregressive Generative Models with Binary Tokens【Unified Model, ByteDance】

[https://arxiv.org/pdf/2602.14041](https://arxiv.org/pdf/2602.14041)

- **把“视觉离散 token”做成高熵的二进制表示，然后****用“扩散式预测头”去采样这些二进制 token****，并用“按 patch 并行预测”把自回归生成速度拉上去**。**用扩散式预测头采样二进制 token，本质是在连续空间里做“去噪生成”，而不是在一个巨大词表上做一次性分类**。这样就不用搞超大 softmax，参数规模不会随“词表容量”爆炸；同时**扩散过程是联合建模，能自然捕捉各个 bit 之间的相关性，采样还能通过步数在“速度 vs 质量”之间调节**。换句话说，它把“表达能力很大的离散空间”变成“可训练、可扩展的连续生成问题”，在高熵 token 场景下更稳、更可扩。
## FireRed-Image-Edit-1.0 Techinical Report【Xiaohongshu】

[https://arxiv.org/pdf/2602.13344](https://arxiv.org/pdf/2602.13344)

- 基于扩散Transformer（Diffusion Transformer）的指令驱动图像编辑模型。该研究通过系统性的数据工程、训练方法优化与评估体系构建，在不依赖超大规模参数（如数十亿参数）的前提下，实现了与商业级闭源模型竞争的性能。建立了包含清洗、合成、标注、过滤的完整数据工程体系，证明高质量中等规模数据（100M）优于低质量海量数据。
## World Action Models are Zero-shot Policies【NVIDIA】

[https://arxiv.org/pdf/2602.15922](https://arxiv.org/pdf/2602.15922)

- 论文提出DreamZero，通过以下机制解决上述问题：联合视频-动作预测：将动作学习从"状态-动作模仿"转变为"逆动力学学习"——通过预测未来视觉状态来指导动作生成利用视频扩散先验：基于预训练的视频扩散模型（Wan2.1-I2V-14B），继承互联网规模视频数据中的丰富时空物理动力学知识实时闭环控制：通过算法和系统优化（DreamZero-Flash），将14B参数模型的推理速度提升38倍，实现7Hz实时控制。

# Agent Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Training
- 方法：agent, reinforcement-learning
- 论文/报告：2 篇
- RLAnything: Forge Environment, Policy, and Reward Model in Completely Dynamic RL System
- **Agent World Model: Infinity Synthetic Environments for Agentic Reinforcement Learning**
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## RLAnything: Forge Environment, Policy, and Reward Model in Completely Dynamic RL System

[https://arxiv.org/pdf/2602.02488](https://arxiv.org/pdf/2602.02488)

- RLAnything 让**环境、策略、奖励模型**三者**互相当教练、互相当学生**，在长轨迹、真实世界任务中**同步变强**，为 LLM/智能体强化学习提供**可理论保证、可工程落地、可持续扩展**的新范式。
## **Agent World Model: Infinity Synthetic Environments for Agentic Reinforcement Learning**

[https://arxiv.org/pdf/2602.10090](https://arxiv.org/pdf/2602.10090)

- 解决**智能体强化学习（Agentic Reinforcement Learning）中环境稀缺、多样性不足且难以扩展**的核心问题。论文提出**Agent World Model (AWM)**，一种**全自动合成可执行工具使用环境的流程，通过代码驱动和数据库支持的状态管理**，实现大规模、多样化、状态一致且适合在线强化学习的智能体训练环境。

# Agent Applications

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Applications
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, science-discovery, vision
- 论文/报告：86 篇
- Why Your Deep Research Agent Fails? On Hallucination Evaluation in Full Research Trajectory【Research, Chao Huang】
- PaperBanana: Automating Academic Illustration for AI Scientists【Research, Google】
- AutoFigure: Generating and Refining Publication-Ready Scientific Illustrations【Research, Yue Zhang】
- S1-NexusAgent: a Self-Evolving Agent Framework for Multidisciplinary Scientific Research【Research】
- WideSeek: Advancing Wide Research via Multi-Agent Scaling【Research】
- WideSeek-R1: Exploring Width Scaling for Broad Information Seeking via Multi-Agent Reinforcement Learning【Research】
- Accelerating Scientific Research with Gemini: Case Studies and Common Techniques【Research】
- AIRS-Bench: a Suite of Tasks for Frontier AI Research Science Agents【Research】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Why Your Deep Research Agent Fails? On Hallucination Evaluation in Full Research Trajectory【Research, Chao Huang】

[https://arxiv.org/pdf/2601.22984](https://arxiv.org/pdf/2601.22984)

- 解决**深度研究智能体（Deep Research Agents, DRAs）失败机制诊断缺失**的核心问题。论文提出**从结果导向转向过程感知评估**的新范式，通过审计完整研究轨迹（plan-search-summarize循环）。
## PaperBanana: Automating Academic Illustration for AI Scientists【Research, Google】

[https://arxiv.org/pdf/2601.23265](https://arxiv.org/pdf/2601.23265)

- **如何自动、无需人工干预地生成可直接用于学术出版的插图（methodology diagrams 与 statistical plots）**。作者提出 PaperBanana 框架，通过**多智能体协作 + 参考驱动 + 迭代自评**的方式，把一段方法论文字与图注映射为**符合 NeurIPS 级出版标准**的插图，从而首次实现“端到端自动化学术插图生成”。
## AutoFigure: Generating and Refining Publication-Ready Scientific Illustrations【Research, Yue Zhang】

[https://arxiv.org/pdf/2602.03828](https://arxiv.org/pdf/2602.03828)

- **从长文本科学内容自动生成出版级质量科学插图，**为应对这些挑战，论文提出了**FigureBench**（首个涵盖3,300对高质量长文本-插图对的大规模基准）和**AUTOFIGURE**（基于"推理渲染"（Reasoned Rendering）范式的智能体框架），通过解耦结构布局生成与美学渲染，实现既科学准确又具出版级视觉质量的插图自动生成。
## S1-NexusAgent: a Self-Evolving Agent Framework for Multidisciplinary Scientific Research【Research】

[https://arxiv.org/pdf/2602.01550](https://arxiv.org/pdf/2602.01550)

- 论文提出了 **S1-NexusAgent**，一个基于**Plan-and-CodeAct**范式的**自进化代理框架，通过内外双循环架构、意图感知的动态工具检索、基于对象引用的稀疏上下文管理以及轨迹评估驱动的自我进化机制**，实现对复杂、长程、多学科科学任务的稳定建模与持续能力积累。
## WideSeek: Advancing Wide Research via Multi-Agent Scaling【Research】

[https://arxiv.org/pdf/2602.02636](https://arxiv.org/pdf/2602.02636)

- 搜索智能正从**深度研究（Deep Research）**——即通过多步推理链定位单一信息——转向**广度研究（Wide Research）**，后者要求在复杂约束下并行检索、整合大规模结构化信息（如生成竞争对手分析表）。现有基准多聚焦单点深度推理，缺乏对并行广度检索能力的评估与训练支持。
## WideSeek-R1: Exploring Width Scaling for Broad Information Seeking via Multi-Agent Reinforcement Learning【Research】

[https://arxiv.org/pdf/2602.04634](https://arxiv.org/pdf/2602.04634)

- 论文提出**WIDESEEK-R1**，一个通过**多智能体强化学习（MARL）训练的 lead-agent–subagent 框架，旨在协同优化可扩展编排（scalable orchestration）与并行执行（parallel execution）**。该框架通过共享LLM实现上下文隔离（context isolation）与任务并行化，使系统能够通过增加并行子智能体数量持续提升性能，而非受限于单智能体的上下文长度瓶颈。
## Accelerating Scientific Research with Gemini: Case Studies and Common Techniques【Research】

[https://arxiv.org/pdf/2602.03837](https://arxiv.org/pdf/2602.03837)

- 如何有效利用先进AI模型加速理论计算机科学及相关领域（如经济学、优化、物理学）的原创性数学发现，并建立可复现的人机协作方法论？论文通过24个独立案例研究（涵盖信息论中的Courtade-Kumar猜想、密码学中的SNARGs漏洞检测、物理学中的宇宙弦谱解析、算法设计中的核心集优化等），实证证明了在严格的人类监督下，LLMs能够：发现证明中的致命逻辑漏洞（如区分"完美一致性"与"统计一致性"的细微差别）自动识别跨学科联系（如将Steiner树问题与Kirszbraun延拓定理关联）自主推导复杂递推关系的解析解并优化近似比率最终，论文论证了AI可作为"不知疲倦、知识渊博且富有创造力的初级合作者"，但强调人类专家在验证、筛选和指导中的不可替代性。
## AIRS-Bench: a Suite of Tasks for Frontier AI Research Science Agents【Research】

[https://arxiv.org/pdf/2602.06855](https://arxiv.org/pdf/2602.06855)

- 论文提出AI Research Science Benchmark (AIRS-Bench)，包含**从17篇顶会论文（2020-2025）精选的20个任务**，覆盖自然语言处理、数学推理、代码生成、分子性质预测、时间序列预测等7大领域。其核心设计原则包括： 无基线代码设置：**智能体必须独立生成训练与验证代码**，而非在现有方案上微调，真实测试原创研究能力 标准化任务配置：通过metadata.yaml、project_description.md、evaluate.py等固定格式文件，确保跨框架（AIRA-dojo与MLGym）的可复现性 隐藏测试集评估：测试标签对智能体完全不可见，防止过拟合
## ⭐HybridQuestion: Human-AI Collaboration for Identifying High-Impact Research Questions【Research, Tie-Yan Liu】

[https://arxiv.org/pdf/2602.03849](https://arxiv.org/pdf/2602.03849)

- 解决"AI科学家能否识别有意义的研究问题"这一核心问题，并将其应用于识别2025年重大科学突破与2026年重大科学问题的实证研究中。
## IntentRL: Training Proactive User-intent Agents for Open-ended Deep Research via Reinforcement Learning【Research, Tongyi】

[https://arxiv.org/pdf/2602.03468](https://arxiv.org/pdf/2602.03468)

- **IntentRL**框架，通过**澄清有向无环图（C-DAG）构建可扩展的训练数据，并采用两阶段强化学习策略**（离线专家轨迹学习+在线模拟器探索），训练能够在深度研究执行前主动澄清用户潜在意图的智能体。
## PaperX: A Unified Framework for Multimodal Academic Presentation Generation with Scholar DAG【Research】

[https://arxiv.org/pdf/2602.03866](https://arxiv.org/pdf/2602.03866)

- 论文提出 **PaperX** 框架，将学术演示生成形式化为 **"论文→有向无环图→学者型内容"（Paper-to-DAG-to-Scholar）** 的结构转换过程。通过引入 **Scholar DAG** 作为中间表示（类比编译器的中间代码IR），将论文的逻辑结构与最终呈现语法解耦，从而支持基于统一语义网络的多粒度内容选择与重组，实现多格式间的一致性与成本效益。
## ReThinker: Scientific Reasoning by Rethinking with Guided Reflection and Confidence Control【Research, Huawei Noah Ark】

[https://arxiv.org/pdf/2602.04496](https://arxiv.org/pdf/2602.04496)

- 面向专家级科学推理的置信度感知智能体框架，通过"重新思考"（Rethinking）机制、引导式反思（Guided Reflection）和置信度控制（Confidence Control）解决现有大语言模型在复杂科学任务中的局限性。该框架在HLE、GAIA和XBench等基准测试上取得了显著优于现有基础模型和深度研究系统的性能。
## AgentCPM-Report: Interleaving Drafting and Deepening for Open-Ended Deep Research【Research, OpenBMB】

[https://arxiv.org/pdf/2602.06540](https://arxiv.org/pdf/2602.06540)

-  **AgentCPM-Report**，一种基于 **WARP（Writing As Reasoning Policy）** 框架的本地轻量级深度研究报告生成系统。
## AgentCPM-Explore: Realizing Long-Horizon Deep Exploration for Edge-Scale Agents【Research, OpenBMB】

[https://arxiv.org/pdf/2602.06485](https://arxiv.org/pdf/2602.06485)

- 针对**边缘规模（4B参数）Agent模型**在长时程复杂任务中的训练与推理瓶颈，提出了首个系统性训练框架 **AgentCPM-Explore。**
## W&D:Scaling Parallel Tool Calling for Efficient Deep Research Agents【Research, Junnan Li】

[https://arxiv.org/pdf/2602.07359](https://arxiv.org/pdf/2602.07359)

- 现有深度研究智能体（如DeepSeek、OpenAI Deep Research）主要通过增加**顺序推理步骤**（scaling depth）来提升能力，而**每步并行工具调用**（scaling width）的潜力尚未被充分挖掘。与复杂的多智能体编排不同，本工作利用LLM内在的并行工具调用能力，在单智能体框架内实现高效协调。
## InternAgent-1.5: A Unified Agentic Framework for Long-Horizon Autonomous Scientific Discovery【Research, AI Lab】

[https://arxiv.org/pdf/2602.08990](https://arxiv.org/pdf/2602.08990)

- 论文提出InternAgent-1.5——一个统一的多智能体科学发现框架，通过以下架构创新解决上述问题： 三元子系统架构：构建Generation（生成）、Verification（验证）、Evolution（演化）三个协调子系统，分别对应假设构建、方法论评估和证据驱动的知识更新 跨学科知识图谱：建立跨领域知识图谱（Cross-Disciplinary Knowledge Graph）和动态知识流图（Flow Graph），实现非线性、跨学科的知识整合 图增强蒙特卡洛搜索：采用Graph-Augmented Monte Carlo Search框架，通过分支内演化、跨分支引用和多分支聚合等算子，突破传统线性搜索局限 结构化认知记忆：设计包含策略-程序记忆（SPM）、任务-情景记忆（TEM）和语义-知识记忆（SKM）的分层记忆架构，支持长期知识累积与目标演化。
## Towards Autonomous Mathematics Research【Research, DeepMind】

[https://arxiv.org/pdf/2602.10177](https://arxiv.org/pdf/2602.10177)

- 如何**将人工智能的能力从竞赛级数学（如国际数学奥林匹克）扩展到专业级数学研究**，实现能够自主或半自主进行原创数学研究的AI系统。
## DRACO: a Cross-Domain Benchmark for Deep Research Accuracy, Completeness, and Objectivity【Research, Perplexity】

[https://arxiv.org/pdf/2602.11685](https://arxiv.org/pdf/2602.11685)

- 面向深度研究（Deep Research）AI系统的跨领域基准测试。
## EduResearchBench: A Hierarchical Atomic Task Decomposition Benchmark for Full-Lifecycle Educational Research【Research】

[https://arxiv.org/pdf/2602.15034](https://arxiv.org/pdf/2602.15034)

- 首个**面向教育学术写作**全生命周期的综合评估平台，基于**层次化原子任务分解（HATD）**框架，将研究流程解构为：6个专业研究模块（如定量研究、定性研究、政策研究）24个细粒度原子任务（如热点提取、变量操作化、理论批判）通过这一分解，实现从模糊的整体评分向可解释的原子能力评估转变，并配套提出课程学习策略与专用模型EduWrite，以验证数据质量与层次化训练在垂直领域优于单纯参数规模扩展的假设。
## ⭐InnoEval: On Research Idea Evaluation as a Knowledge-Grounded, Multi-Perspective Reasoning Problem【Research, Huajun Chen】

[https://arxiv.org/pdf/2602.14367](https://arxiv.org/pdf/2602.14367)

- 将**研究想法评估**重新定义为**基于知识的多视角推理问题（Knowledge-Grounded, Multi-Perspective Reasoning Problem），并构建InnoEval框架： 异构深度知识搜索引擎：并发获取文献、网络、代码等多源在线知识，结合快速检索与深度阅读** 创新评审委员会：采样具有不同学术背景、专长和偏好的评审员角色（personas），通过部分遮蔽知识模拟真实人类认知，实现多视角评估 多维解耦评估：支持跨多个维度的可定制评估，每个维度由专门的评估代理进行深度分析。
## A Benchmark for Deep Information Synthesis【Research】

[https://arxiv.org/pdf/2602.21143](https://arxiv.org/pdf/2602.21143)

- **LLM智能体评估基准在评估真实世界复杂信息综合能力方面的不足**。论文提出了DEEPSYNTH基准，旨在系统性地评估智能体在以下方面的能力：浏览多个网站并从非结构化与结构化来源（段落与表格）中提取信息 执行多步推理与综合分析（如趋势检测、相关性分析、异常检测等）生成可验证的、结构化的多部分答案（JSON格式）处理跨67个国家、7个领域的真实世界数据该基准通过要求注释者从官方数据源收集数据、创建假设、进行手动分析并设计具有可验证答案的任务，确保任务既真实又具有挑战性，从而推动智能体从"信息检索"向"信息综合"的能力跃迁。
## DREAM: Deep Research Evaluation with Agentic Metrics【Research】

[https://arxiv.org/pdf/2602.18940](https://arxiv.org/pdf/2602.18940)

- 解决**深度研究智能体（Deep Research Agents, DRAs）的评估困境，**论文提出了**DREAM**（Deep Research Evaluation with Agentic Metrics）框架，核心思想是**"能力对等原则"（capability parity）**——评估器应当具备与被评估研究智能体相似的能力（检索、验证、推理），通过构建**代理化的评估协议**（agentic evaluation protocols）来实现对时效性、事实准确性和推理质量的主动验证。
## 🧐Understanding Agent Scaling in LLM-Based Multi-Agent Systems via Diversity

[https://arxiv.org/pdf/2602.03794](https://arxiv.org/pdf/2602.03794)

- 传统观点认为增加智能体数量（类似集成学习）可提升MAS性能，但本文发现同质配置（相同模型、提示词）存在强烈的边际收益递减：准确率仅在智能体数 N 较小时提升，随后迅速饱和。相反，**异质配置（不同模型、角色/提示词）能够持续改进性能——仅需2个多样智能体即可匹配或超越16个同质智能体的表现（8倍效率提升）**。
## When Is Enough Not Enough? Illusory Completion in Search Agents【Search】

[https://arxiv.org/pdf/2602.07549](https://arxiv.org/pdf/2602.07549)

- 这篇论文试图解决**搜索代理（search agents）在多约束问题（multi-constraint problems）中的幻觉完成（illusory completion）现象**。引入 EPISTEMIC LEDGER（认知账本），一个评估框架，用于在每一推理步骤联合追踪： 证据支持（evidential support）：检索结果是否实际支持每个约束 代理信念（agent belief）：代理是否认为每个约束已满足。
## REDSearcher: A Scalable and Cost-Efficient Framework for Long-Horizon Search Agents【Search, Long Horizon】

[https://arxiv.org/pdf/2602.14234](https://arxiv.org/pdf/2602.14234)

- 论文提出**REDSearcher**框架，通过以下技术路径实现可扩展且成本效益优化的长周期搜索智能体训练：**双约束任务合成**：将任务生成建模为基于潜在知识图的约束满足问题，联合控制图拓扑结构（树宽）和证据分散度（Minimum Source Dispersion）；**工具增强查询**：通过将静态实体重写为工具可解析的功能依赖（如将地点名称转换为路由约束），强制要求主动工具调用而非被动知识召回；**两阶段中期训练**：分离原子能力获取（知识锚定 grounding 与层次化规划）与交互式执行阶段，利用低成本合成数据预训练核心能力，显著降低后续高质量轨迹收集的样本复杂度；**功能等效模拟环境**：构建轻量级本地模拟环境，在消除实时API调用开销的同时，通过保证可解性与高干扰噪声的平衡，实现高吞吐量的强化学习实验。
## GISA: A Benchmark for General Information-Seeking Assistant【Search, Kuaishou】

[https://arxiv.org/pdf/2602.08543](https://arxiv.org/pdf/2602.08543)

- 评估通用信息寻求助手的高质量基准测试。
## BrowseComp- V3 : A Visual, Vertical, and Verifiable Benchmark for Multimodal Browsing Agents【Search】

[https://arxiv.org/pdf/2602.12876](https://arxiv.org/pdf/2602.12876)

- **现有多模态浏览代理（Multimodal Browsing Agents）基准测试在评估深度搜索能力时存在的核心局限性**。为解决上述问题，论文提出了BrowseComp-V3，这是一个新型基准测试，具有以下核心特征： 深度跨模态推理：300个手工设计的复杂问题，要求在网络页面内外进行文本与视觉模态的深度交织推理 公开可搜索性：所有关键证据均通过标准公共搜索引擎可访问，并提供人工标注的黄金标准搜索轨迹 过程导向评估：引入专家验证的中间子目标（Sub-goals）评估机制，通过Process Score指标量化模型在多步搜索推理中的进展 此外，论文还开发了OmniSeeker（通用多模态浏览代理框架），并揭示了当前最先进水平模型（如GPT-5.2）在该基准上仅能达到约36%的准确率，凸显了多模态信息整合与细粒度感知方面的关键瓶颈。
## Mobile-Agent-v3.5: Multi-platform Fundamental GUI Agents【GUI, Tongyi】

[https://arxiv.org/pdf/2602.16855](https://arxiv.org/pdf/2602.16855)

- 论文提出了**GUI-Owl-1.5**，通过混合数据飞轮（Hybrid Data Flywheel）、统一思维合成管道（Unified CoT Synthesis）以及多平台强化学习优化算法（MRPO）等技术方案，构建了支持多尺寸（2B/4B/8B/32B/235B）和多平台部署的**原生GUI智能体模型**家族。
## Learning with Challenges: Adaptive Difficulty-Aware Data Generation for Mobile GUI Agent Training【GUI】

[https://arxiv.org/pdf/2601.22781](https://arxiv.org/pdf/2601.22781)

- 解决移动GUI智能体训练中“高质量交互轨迹稀缺”且“任务难度与智能体能力失配”的核心瓶颈。提出**能****力对齐的数据生成：持续生成“刚好足够难”的轨迹**，把训练挑战点精确推到智能体能力边界之外。
## Darwinian Memory: A Training-Free Self-Regulating Memory System for GUI Agent Evolution【GUI】

[https://arxiv.org/pdf/2601.22528](https://arxiv.org/pdf/2601.22528)

- 零训练、自进化的 GUI 代理记忆框架，用“适者生存”机制解决长程、跨应用任务中的经验僵化与上下文污染两大痛点。 把长程轨迹拆成独立 ⟨Precondition, Goal⟩ 单元，实现灵活复用 定义多因素“生存价值”自动淘汰过时/错误轨迹，保持高信噪比 引入 ε-突变与贝叶斯风险估计，持续产生进化压力，逃离局部最优 在 AndroidWorld 上跨 4 种通用 MLLM 测试，平均 +18.0 % 成功率、+33.9 % 稳定性，内存维持 5 MB 级，延迟下降 4.75 %。
## ⭐ToolTok: Tool Tokenization for Efficient and Generalizable GUI Agents【GUI】

[https://arxiv.org/pdf/2602.02548](https://arxiv.org/pdf/2602.02548)

- 论文提出ToolTok范式，其核心创新在于： 从视觉定位到视觉路径规划的范式转变：将GUI交互重新定义为离散工具token的多步渐进式路径规划（multi-step progressive pathfinding），而非连续坐标回归。 语义锚定机制（Semantic Anchoring）：通过将工具token与语义相关的自然语言概念关联（如 < MOVE UP FAR> 锚定到 {move, up, far}），为数据稀缺环境下的嵌入学习提供归纳偏置。 课程学习策略：构建从纯文本工具选择到简化视觉路径规划的渐进式训练流程，仅需不到1%的训练数据（约5,000合成样本+~2,000真实样本）即可实现有效对齐。
## Generative Visual Code Mobile World Models【GUI】

[https://arxiv.org/pdf/2602.01576](https://arxiv.org/pdf/2602.01576)

- **首次在移动端 GUI 场景下实现**单一开源VLM直接预测下一时刻 GUI 状态。输出 HTML/CSS，浏览器实时渲染成像素，兼顾**文本精确**与**视觉高保真**。
## M 2 -Miner: Multi-Agent Enhanced MCTS for Mobile GUI Agent Data Mining【GUI】

[https://arxiv.org/pdf/2602.05429](https://arxiv.org/pdf/2602.05429)

- 构建 **M2-Miner-Agent** 数据集（20k图像，2,565条轨迹，平均长度7.8步），单张图像成本仅 **$0.02**，相比人工标注方法成本降低约**18倍**，且数据质量准确率（DQA=93%）优于部分人工标注数据集。
## POINTS-GUI-G: GUI-Grounding Journey【GUI, WeChat】

[https://arxiv.org/pdf/2602.06391](https://arxiv.org/pdf/2602.06391)

- 该论文提出了 **POINTS-GUI-G-8B**，一种从零开始构建的先进GUI（图形用户界面）定位模型，旨在解决视觉-语言模型在**精确识别和定位界面元素（如按钮、图标、文本框）**方面的基础能力问题。与现有研究直接微调已具备强空间感知能力的模型（如Qwen3-VL）不同，该工作基于缺乏原生grounding能力的基础模型（POINTS-1.5），系统性探索了GUI grounding能力的完整技术构建路径，以实现“全栈”掌握。
## ANCHOR: Branch-Point Data Generation for GUI Agents【GUI】

[https://arxiv.org/pdf/2602.07153](https://arxiv.org/pdf/2602.07153)

- 解决**桌面GUI代理（GUI Agents）训练数据稀缺与质量不足**的问题.
## GEBench: Benchmarking Image Generation Models as GUI Environments【GUI, StepFun】

[https://arxiv.org/pdf/2602.09007](https://arxiv.org/pdf/2602.09007)

- 首个专门用于评估**图像生成模型作为图形用户界面（GUI）环境**的系统性基准测试。
## UI-Mem: Self-Evolving Experience Memory for Online Reinforcement Learning in Mobile GUI Agents【GUI, Self-Evolving】

[https://arxiv.org/pdf/2602.05832](https://arxiv.org/pdf/2602.05832)

- 论文提出**UI-Mem**框架，通过构建**层次化经验记忆**（存储高层工作流、中层子任务技能、失败模式）、**分层组采样机制**（平衡引导与探索以维持优势估计多样性）以及**自进化循环**（持续抽象新策略并更新记忆），实现高效的在线RL训练和跨任务知识迁移。
## UI-Venus-1.5 Technical Report【GUI, Ant】

[https://arxiv.org/pdf/2602.09082](https://arxiv.org/pdf/2602.09082)

- 为应对上述挑战，论文提出了 UI-Venus-1.5，通过以下三个关键技术进展实现突破： 大规模中场训练（Mid-Training）：在强化学习阶段前引入全面的知识注入阶段，利用涵盖30+数据集的100亿token语料库，建立基础的GUI语义理解能力，使模型在进入RL阶段前即具备解决GUI相关VQA、定位和简单导航任务的能力。 规模化在线强化学习（Online Reinforcement Learning）：通过全轨迹展开（full-trajectory rollouts）和跨环境的奖励计算，将训练目标与长程动态导航对齐，直接优化轨迹级成功率，缓解步骤级与轨迹级准确性之间的偏差。 模型合并构建统一代理（Unified Single-Agent via Model Merging）：针对定位（Grounding）、网页（Web）和移动端（Mobile）三个领域分别训练专用模型，随后通过TIES-Merge等策略将其融合为单一的端到端检查点，在最小化性能损失的前提下实现跨域能力统一，显著简化部署流程。
## **Code2World: A GUI World Model via Renderable Code Generation****【GUI】**

[https://arxiv.org/pdf/2602.09856](https://arxiv.org/pdf/2602.09856)

- 论文提出Code2World框架，其核心创新在于将GUI模拟重新定义为**可渲染代码生成（Renderable Code Generation）**任务。通过利用HTML作为界面的原生结构化表示，该方法能够： **通过确定性渲染实现高保真可视化**； 通过符号化代码保证精确的结构可控性； 通过"渲染感知强化学习"（Render-Aware RL）将文本代码生成与视觉现实对齐。 最终目标是构建一个轻量级但功能强大的虚拟沙盒，使自主GUI代理具备类似人类的预见能力，在执行动作前准确模拟其后果，从而提升长程导航的决策质量与安全性。
## TreeCUA: Efficiently Scaling GUI Automation with Tree-Structured Verifiable Evolution**【GUI, Meituan】**

[https://arxiv.org/pdf/2602.09662](https://arxiv.org/pdf/2602.09662)

- 论文提出TreeCUA框架，利用CUA在跨应用探索过程中天然遵循的**树状结构特性（早期功能节点被频繁复用）**，通过以下方式解决上述问题： **采用树状拓扑结构存储和复用中间探索节点，消除步骤冗余**，实现成本分摊（amortized cost）； 引入世界知识引导（World Knowledge Guidance）和自适应探索算法，平衡探索深度（任务难度）与广度（轨迹多样性），避免低质量生成； 基于树分支结构自动生成偏好对（preference pairs），通过TreeCUA-DPO优化规划能力，无需额外人工成本。
## See, Plan, Snap: Evaluating Multimodal GUI Agents in Scratch【GUI】

[https://arxiv.org/pdf/2602.10814](https://arxiv.org/pdf/2602.10814)

- 首个面向块式编程环境（Scratch）的多模态GUI Agent基准测试。而现有GUI Agent基准（WebArena、OSWorld等）主要覆盖导航与表单任务，缺乏对**程序构建型任务**（Program-by-Construction）的评估。这个 setting 的核心区别在于，它不是让模型“写代码”或“生成答案”，而是必须通过真实 GUI 拖拽拼装程序块来完成任务，从而同时评估逻辑推理能力和精细界面执行能力，而不是只测语言或规划能力。
## Adaptive Milestone Reward for GUI Agents【GUI】

[https://arxiv.org/pdf/2602.11524](https://arxiv.org/pdf/2602.11524)

- 解决移动GUI代理在长程任务强化学习训练中面临的时序信用分配难题。论文提出**自适应里程碑奖励（Adaptive Milestone Reward, ADMIRE）**机制，通过以下方式重构奖励设计： 自适应里程碑生成：**从成功探索中动态提取关键状态转换（里程碑），作为可验证的中间目标，并随策略进化而更新**。 非对称信用分配：对成功轨迹进行去噪（仅奖励触发里程碑的关键步骤），对失败轨迹提供支架式奖励（部分信用+里程碑奖励），打破"全有或全无"的稀疏反馈范式。 该机制旨在建立一种密集、高置信度的在线RL训练范式，使代理能够在长程移动GUI任务中实现稳定、高效的策略学习。
## Building Autonomous GUI Navigation via Agentic-Q Estimation and Step-Wise Policy Optimization【GUI, Alibaba】

[https://arxiv.org/pdf/2602.13653](https://arxiv.org/pdf/2602.13653)

- 论文提出了一个以MLLM为中心的新型框架，包含两个关键组件： Agentic-Q 估计：**通过自生成的轨迹数据训练一个Q模型，将最终的轨迹级监督反向传播到每个中间步骤，生成连续的步骤级价值评估**，从而缓解监督稀疏问题； 步骤级策略优化（Step-Wise Policy Optimization）：**利用Agentic-Q模型提供的步骤级奖励，在与环境解耦的情况下进行策略优化**，通过动作级过滤和自适应组加权等技术确保优化的稳定性。 该框架的核心优势在于： 所有训练轨迹均由策略自身生成，无需昂贵的专家注释； 策略更新完全解耦于非平稳环境，避免了环境波动带来的训练不稳定性。
## GUI-GENESIS: Automated Synthesis of Efficient Environments with Verifiable Rewards for GUI Agent Post-Training【GUI】

[https://arxiv.org/pdf/2602.14093](https://arxiv.org/pdf/2602.14093)

- 论文提出GUI-GENESIS框架，通过任务条件化环境合成（task-conditioned environment synthesis）解决上述问题： **利用多模态代码模型将真实应用反编译为轻量级、独立的Web应用（Flask后端+HTML/JS前端）**，消除网络开销和后端依赖，将单步延迟降低10倍 在合成过程中将任务成功条件编码为代码原生奖励（code-native rewards）——即可执行断言（executable assertions），直接检查后端状态变量，提供确定性、细粒度的奖励信号r∈[0,1] ，消除视觉估计噪声 实现零样本模拟到真实迁移（zero-shot sim-to-real transfer），在保持行为保真度的同时，使训练成本降低超过**$28,000/epoch**
## GUI-Libra: Training Native GUI Agents to Reason and Act with Action-aware Supervision and Partially Verifiable RL【GUI, Long Horizon, Microsoft】

[https://arxiv.org/pdf/2602.22190](https://arxiv.org/pdf/2602.22190)

- 用于训练具备推理能力的原生 GUI智能体的统一后训练框架。
## EmbeWebAgent: Embedding Web Agents into Any Customized UI【Web】

[https://arxiv.org/pdf/2602.14865](https://arxiv.org/pdf/2602.14865)

- 论文提出了**EmbeWebAgent**框架，通过轻量级前端shim（暴露ARIA标签和每页函数注册表）与可重用后端工作流（多Agent协调、MCP工具集成）的架构分离，实现对现有Web应用的最小侵入式嵌入，在保持栈无关性的同时提升Agent的鲁棒性和任务执行能力。
## OpAgent: Operator Agent for Web Navigation【Web, Ant】

[https://arxiv.org/pdf/2602.13559](https://arxiv.org/pdf/2602.13559)

- 论文提出 **OpAgent** 框架，通过**分层多任务微调**建立基础策略，**利用在线智能体强化学习（Online Agentic RL in the Wild） 直接与实时网站交互优化策略**，并设计**混合奖励机制**（结合WebJudge整体评估与基于规则的决策树过程奖励），最终通过**模块化智能体架构**（规划器、定位器、反思器、总结器协同）实现鲁棒的错误恢复与自我纠正，在WebArena基准测试上达到71.6%的最新最优成功率。
## ⭐WebWorld: A Large-Scale World Model for Web Agent Training【Web, Qwen】

[https://arxiv.org/pdf/2602.14721](https://arxiv.org/pdf/2602.14721)

- 论文提出 WebWorld ——**首个基于100万+真实世界轨迹（1M+ trajectories）训练的大规模开源网页世界模型**。通过构建可扩展的分层数据管道（scalable hierarchical data pipeline），WebWorld 实现了： 开放网页泛化：支持多样化真实网站，超越封闭基准测试； 多模态模拟：支持A11y Tree、HTML、XML等多种格式； 长期推理：支持30+步的交互历史建模与链式思考（Chain-of-Thought）推理； 跨域迁移：在代码、GUI、游戏等环境中展现强泛化能力。 该模型旨在提供一个高质量、可复现的网页模拟环境，使代理能够在合成数据上高效训练，从而规避真实世界交互的物理限制与安全风险。
## AutoWebWorld: Synthesizing Infinite Verifiable Web Environments via Finite State Machines【Web】

[https://arxiv.org/pdf/2602.14296](https://arxiv.org/pdf/2602.14296)

- AutoWebWorld，一种基于有限状态机（FSM）的合成Web环境框架，旨在解决自主GUI代理训练中的数据稀缺性与验证瓶颈问题。显式状态建模：将Web环境建模为有限状态机（FSM），显式定义所有状态 s=(p,σ) 、动作 A 和确定性转移函数 T:S×A→S ，使底层状态转换逻辑完全可观测。 程序化验证：通过FSM的前置条件（preconditions）和确定性转移规则强制执行动作有效性，任务成功通过是否到达目标状态（goal state）来内在验证，无需外部评判器。 低成本规模化数据合成：基于FSM进行广度优先搜索（BFS）自动生成最短动作序列，并通过编码代理将FSM转换为可执行的前端网站，最终通过执行过滤获得可复现的验证轨迹，成本降至每条轨迹仅 $0.04 。 可扩展的合成数据scaling law：论文验证了随着合成数据量的增加，代理在真实世界基准（WebVoyager、Online-Mind2Web）上的性能持续提升，证明了合成数据驱动基础模型性能增长的潜力。
## Agentic Test-Time Scaling for WebAgents【Web, Long Horizon】

[https://arxiv.org/pdf/2602.12276](https://arxiv.org/pdf/2602.12276)

- **多步、长程（long-horizon）智能体任务中的测试时计算缩放（test-time scaling）效率与性能优化问题。论文提出基于置信度的动态计算分配策略（CATTS），核心思想是：****利用投票分布导出的不确定性统计量（熵 Ht 与置信度边际 Δt ）作为测试时信号，仅在决策真正存在争议时调用仲裁器****，而****在高置信度步骤保持简单的多数投票****，从而在提升任务成功率的同时显著降低令牌消耗。**
## World-Model-Augmented Web Agents with Action Correction【Web】

[https://arxiv.org/pdf/2602.15384](https://arxiv.org/pdf/2602.15384)

- 该论文针对基于大语言模型的Web智能体在复杂环境中成功率远低于人类的问题，提出**WAC（World-model–augmented Action Correction）**框架，通过多智能体协作与执行前风险感知机制提升决策鲁棒性。
## From Self-Evolving Synthetic Data to Verifiable-Reward RL: Post-Training Multi-turn Interactive Tool-Using Agents【Self-Evolving】

[https://arxiv.org/pdf/2601.22607](https://arxiv.org/pdf/2601.22607)

- 一套无需人工标注即可规模化后训练“多轮交互式工具使用智能体”的完整框架。
## TTCS: Test-Time Curriculum Synthesis for Self-Evolving【Self-Evolving】

[https://arxiv.org/pdf/2601.22628](https://arxiv.org/pdf/2601.22628)

- 在困难推理任务上的测试阶段自监督训练提出两个核心痛点： 伪标签不可靠：当测试题本身过难（如 AIME24）时，多数采样答案都是错的，基于“多数投票”得到的伪标签会系统性地误导策略更新。 可学习样本稀缺：测试集规模小且难度跳跃大，模型缺乏“垫脚石”式的中等难度样本，导致在线更新不稳定甚至崩溃。 为此，作者提出 TTCS（Test-Time Curriculum Synthesis），**在测试阶段主动合成一条由易到难的课程序列，让模型先学会“可解的变体”，再反哺原始难题**，实现无标签、自演化的稳定提升。
## Group-Evolving Agents: Open-Ended Self-Improvement via Experience Sharing【Self-Evolving】

[https://arxiv.org/pdf/2602.04837](https://arxiv.org/pdf/2602.04837)

- 论文提出**群体进化智能体（Group-Evolving Agents, GEA）范式，****将一组智能体**（而非单个智能体）作为进化的基本单元，通过显式的**群体内经验共享与重用**（intra-group experience sharing），使不同智能体探索到的互补改进能够在进化过程中被整合和累积，从而将暂时的多样性转化为长期的、可持续的进化进步。
## ⭐Sci-CoE: Co-evolving Scientific Reasoning LLMs via Geometric Consensus with Sparse Supervision【Self-Evolving, AI Lab】

[https://arxiv.org/pdf/2602.12164](https://arxiv.org/pdf/2602.12164)

- 能否在缺乏真值标注和预定义验证程序的限制下，构建一个**适用于科学推理任务的自演化强化学习框架**？ 为此，作者设计了Sci-CoE框架，通过几何共识机制与稀疏监督的两阶段训练（从稀疏监督锚定到无监督协同演化），使单一语言模型同时习得科学问题求解与多视角验证的能力。
## TRIP-Bench: A Benchmark for Long-Horizon Interactive Agents in Real-World Scenarios【Long Horizon】

[https://arxiv.org/pdf/2602.01675](https://arxiv.org/pdf/2602.01675)

- 论文构建了TRIP-Bench，一个基于真实旅行规划场景的长程交互基准，具有以下特性：18个专用工具与40+类旅行需求的细粒度约束四种高难度交互子集（LIT、FIT、AIS、PMR）模拟真实用户行为自动化评估体系，区分严格（Strict）与宽松（Loose）两种评估模式。
## ⭐daVinci-Agency: Unlocking Long-Horizon Agency Data-Efficiently【Long Horizon, Pengfei Liu】

[https://arxiv.org/pdf/2602.02619](https://arxiv.org/pdf/2602.02619)

- **长程智能体（long-horizon agency）任务中高质量训练数据稀缺。论文发现****GitHub Pull Request序列天然具备长程学习所需的监督结构： 将复杂目标分解为可验证的提交单元，在迭代中保持功能一致性，通过Bug修复历史编码真实的改进模式****。仅用239个样本微调GLM-4.6，平均轨迹长度达85k tokens和116次工具调用，却在多个基准上超越使用数万样本的方法： Toolathlon相对增益47%（0.157 → 0.231） SWE-bench达到0.632，超过基线0.608 AgencyBench Code得分15.9，显著优于基线（11.9）和Kimi-K2-Thinking（11.8）**
## EcoGym: Evaluating LLMs for Long-Horizon Plan-and-Execute in Interactive Economies【OPPO】

[https://arxiv.org/pdf/2602.09514](https://arxiv.org/pdf/2602.09514)

- **现有LLM智能体评估框架在长期规划（Long-Horizon Planning）评估方面的关键局限性。论文提出EcoGym——一个开放、可扩展的基准测试平台，其设计针对：连续计划-执行决策：评估智能体****在长达365天（1000+步骤）的无界时间范围内的战略稳定性交互式经济环境****：通过三个真实商业场景（****Vending零售、Freelance零工经济、Operation平台运营****）测试资源分配、劳动管理和运营效率潜在机制探索：引入隐藏的经济机制（如季节性、弹性系数），强制智能体从被动执行转向主动假设检验和因果发现**
## CLI-Gym: Scalable CLI Task Generation via Agentic Environment Inversion【Code】

[https://arxiv.org/pdf/2602.10999](https://arxiv.org/pdf/2602.10999)

- 针对**环境密集型代理式编码任务（如CLI交互、系统配置修复等）的数据稀缺难题**，提出了首个可规模化生成此类任务的开源框架**CLI-Gym**，并通过实验验证了其在提升代理环境交互能力方面的显著效果。
## Pull Requests as a Training Signal for Repo-Level Code Editing【Code】

[https://arxiv.org/pdf/2602.07457](https://arxiv.org/pdf/2602.07457)

- 解决**仓库级代码编辑（Repository-level Code Editing）中高质量训练信号稀缺**的问题。论文提出**Clean-PR**框架，通过构建可扩展的数据处理管道，将嘈杂的PR差异转换为经过验证的Search/Replace编辑块，并辅以Issue链接增强意图理解，最终形成200万PR、177亿token的大规模训练语料。结合Agentless对齐的逐步监督微调（SFT），证明在简化推理协议下，仓库级工程能力可有效编码至模型权重中，在SWE-bench上取得显著提升（Lite提升13.6%，Verified提升12.3%）。
## OmniCode: A Benchmark for Evaluating Software Engineering Agents【Code】

[https://arxiv.org/pdf/2602.02262](https://arxiv.org/pdf/2602.02262)

- 现有 LLM 编程评测聚焦竞赛题或单一补丁生成，无法衡量真实软件生命周期所需的多维能力。**OmniCode 基准**：手工清洗 494 条 GitHub PR（Python/Java/C++），构建 1794 个可执行任务，覆盖 Bug-Fix、Test-Generation、Review-Response、Style-Fix 四类；提供容器化环境，确保可复现。
## SWE-Universe: Scale Real-World Verifiable Environments to Millions【Code, Qwen】

[https://arxiv.org/pdf/2602.02361](https://arxiv.org/pdf/2602.02361)

-  **SWE-Universe**，一个用于自动构建百万级真实世界软件工程（SWE）可验证环境的可扩展框架，并基于此实现了当前最优的代码代理性能。该工作提供了**当前最大规模（80万+实例）的真实世界多语言SWE训练资源**，并通过系统性的技术方案（迭代验证、作弊检测、高效模型）解决了大规模环境构建的可扩展性与可靠性难题，为下一代代码代理的开发奠定了数据与方法论基础。
## ProjDevBench: Benchmarking AI Coding Agents on End-to-End Project Development【Code】

[https://arxiv.org/pdf/2602.01655](https://arxiv.org/pdf/2602.01655)

- 随着"vibe coding"（氛围编程）兴起，编码代理需要具备从自然语言需求出发，**自主设计架构、管理多文件依赖、配置构建系统并交付可执行项目**的能力，而现有基准缺乏对此类端到端开发任务的系统性评估。
## Beyond Quantity: Trajectory Diversity Scaling for Code Agents【Code】

[https://arxiv.org/pdf/2602.03219](https://arxiv.org/pdf/2602.03219)

- 论文提出**TDScaling（Trajectory Diversity Scaling）框架，将优化目标从"数据数量"转向"轨迹多样性"**（Trajectory Diversity），通过业务聚类（Business Cluster）、蓝图驱动合成（Blueprint-Driven Synthesis）和自适应进化机制（Adaptive Evolution），在更少数据量下实现更强的泛化能力与数据效率。
## ContextBench: A Benchmark for Context Retrieval in Coding Agents【Code】

[https://arxiv.org/pdf/2602.05892](https://arxiv.org/pdf/2602.05892)

- 论文提出CONTEXTBENCH，通过以下方式解决上述问题： 构建人工验证的黄金上下文（Gold Contexts）：为1,136个真实 issue 任务标注了专家验证的必要代码上下文，涵盖文件、代码块（AST节点）和行级别粒度。 引入过程导向的评估指标：建立了召回率（Recall）、精确率（Precision）、F1分数以及动态检索效率等多维度指标，在三个粒度（文件、块、行）上量化智能体的上下文检索能力。 暴露中间信号：通过对比智能体检索的上下文与黄金上下文，提供端到端成功率之外的中间监督信号，从而"打开软件任务的黑箱"（unbox the issue-resolution process）。 简言之，该论文试图将代码智能体的评估从结果导向转变为过程导向，解决现有基准无法评估上下文检索质量的关键缺口。
## TermiGen: High-Fidelity Environment and Robust Trajectory Synthesis for Terminal Agents【Code】

[https://arxiv.org/pdf/2602.07274](https://arxiv.org/pdf/2602.07274)

- 首个面向终端代理（terminal agents）的端到端高保真数据合成与训练框架，旨在弥合开放权重模型与专有模型在复杂终端任务执行能力上的差距。
## SWE-AGI: Benchmarking Specification-Driven Software Construction with MoonBit in the Era of Autonomous Agents【Code】

[https://arxiv.org/pdf/2602.09447](https://arxiv.org/pdf/2602.09447)

- **评估LLMs从明确规范自主构建生产级软件系统的端到端能力?**
## Evaluating [AGENTS.md](http://agents.md/): Are Repository-Level Context Files Helpful for Coding Agents?【Code】

[https://arxiv.org/pdf/2602.11988](https://arxiv.org/pdf/2602.11988)

- **仓库级上下文文件（如 AGENTS.md、CLAUDE.md）对编程智能体在实际软件工程任务中的有效性和影响尚未得到严格验证**。论文最终建议：**在现阶段应省略LLM生成的上下文文件，而人工编写的上下文文件应仅包含最小必要要求（如特定工具的使用说明），而非全面的仓库概述**。
## RelBench v2: A Large-Scale Benchmark and Repository for Relational Data【Code】

[https://arxiv.org/pdf/2602.12606](https://arxiv.org/pdf/2602.12606)

- RELBENCH v2旨在构建一个**大规模、多领域、任务类型丰富且与外部生态兼容**的基准测试平台，以推动RDL从专用模型向通用关系基础模型的发展。
## MARTI-MARS 2 : Scaling Multi-Agent Self-Search via Reinforcement Learning for Code Generation【AI Lab】

[https://arxiv.org/pdf/2602.07848](https://arxiv.org/pdf/2602.07848)

- MARTI-MARS2的解决方案： 论文提出Multi-Agent Reinforced Training and Inference Framework with Self-Search Scaling (MARTI-MARS2)，通过以下机制解决上述问题： 将多智能体协作探索建模为动态可学习环境：允许智能体在环境中迭代探索和优化，实现从参数共享的同质多角色训练到参数独立的异质多智能体训练的演进。 深度整合策略学习与多智能体树搜索：在训练阶段通过树搜索生成结构化推理轨迹，在推理阶段引入**MARS2-T+**策略，结合错误反馈、动态深度引导和奖励模型指导，充分挖掘多智能体协作的测试时扩展潜力。 揭示多智能体扩展律：首次系统性地验证从单智能体到同质多角色再到异质多智能体范式的演进，能够依次带来更高的RL性能上限、更强的TTS能力和更大的策略多样性，证明策略多样性是通过多智能体强化学习扩展智能的关键维度。
## MemFly: On-the-Fly Memory Optimization via Information Bottleneck【Memory】

[https://arxiv.org/pdf/2602.07885](https://arxiv.org/pdf/2602.07885)

- 核心思想是：把 **LLM 的长期记忆管理** 形式化为一个 **信息瓶颈（Information Bottleneck）优化问题**，在“尽量压缩历史冗余信息”和“尽量保留对未来任务有用的信息”之间做原则化权衡；具体做法是在线地（on-the-fly）决定新信息是 **合并（去冗余）**、**关联（建关系）** 还是 **新增（保留新知识）**，并用 **分层记忆结构（原始笔记–关键词–主题）+ 混合检索（语义、符号、拓扑）** 来保证压缩后仍然可检索、可推理，从而在长期交互场景下，比简单 RAG 或摘要式记忆更稳定、更高效地支持复杂查询和多轮推理。
## REMem: Reasoning with Episodic Memory in Language Agent【Memory】

[https://arxiv.org/pdf/2602.13530](https://arxiv.org/pdf/2602.13530)

- 论文提出 REMem（Reasoning with Episodic Memory）框架，通过以下机制解决情景记忆的构建与利用问题： 混合记忆图构建（Indexing）：**将经历转换为时间感知的事件表示**，包括： Gists（概要）：简洁的人类可读事件摘要，包含解析后的时间戳及情境维度（参与者、地点、意图等）； Facts（事实）：带时间范围的三元组（subject-predicate-object），支持Wikidata风格的时间限定符； 混合图结构：整合概念层（短语节点）与语境层（概要节点），通过同义词边增强关联。 智能体推理（Agentic Inference）：采用ReAct风格的智能体，配备精心策划的工具（语义检索、词汇检索、概要上下文查找、实体上下文查找），支持： 迭代检索：将复杂查询分解为子查询，逐步收集证据； 时间旅行过滤：根据时间条件筛选记忆条目； 逻辑组合：支持时间范围过滤、邻居探索、序数约束等操作。
## AllMem: A Memory-centric Recipe for Efficient Long-context Modeling【Memory】

[https://arxiv.org/pdf/2602.13680](https://arxiv.org/pdf/2602.13680)

- 论文提出 ALLMEM 架构，核心创新在于： **结合滑动窗口注意力（SWA）处理局部细粒度依赖，与非线性Test-Time Training（TTT）记忆网络实现长期信息压缩**。 通过**内存高效微调（Memory-Efficient Fine-Tuning）**策略，实现将任何现成的预训练LLM高效转换为ALLMEM架构，而无需从头训练。 在保持短序列任务性能的同时，将长序列推理的计算复杂度降至 O(LW) （Prefill）和 O(W) （Decode），并将KV Cache压缩至 O(1) 常数级别。
## Choosing How to Remember: Adaptive Memory Structures for LLM Agents【Memory】

[https://arxiv.org/pdf/2602.14038](https://arxiv.org/pdf/2602.14038)

- 论文提出 FLUXMEM 框架，通过以下机制实现自适应记忆组织： 多结构并行架构：**同时支持线性（时序）、图（关系）和层次（主题）三种互补的记忆结构**，扩展记忆表达空间 上下文感知选择器：基于交互级特征（如实体密度、话题转移频率、时间跨度），通过轻量级MLP显式学习最优结构选择策略，以离线监督方式优化（基于响应质量与记忆利用率反馈） 分布感知的记忆融合：采用**Beta混合模型（BMM）**替代固定相似度阈值，通过建模匹配分数的后验分布实现概率化门控，增强对噪声与分布偏移的鲁棒性 三级记忆层级：**短程交互记忆（STIM）、中程情景记忆（MTEM）与长程语义记忆（LTSM）**的渐进式巩固流程，支持长程记忆的稳定演化。
## AMA-Bench: Evaluating Long-Horizon Memory for Agentic Applications【Memory】

[https://arxiv.org/pdf/2602.22769](https://arxiv.org/pdf/2602.22769)

- 解决**智能体长期记忆评估与实际应用需求之间的显著差距，**论文提出了**AMA-Bench**（首个针对智能体应用的长程记忆基准）和**AMA-Agent**（基于因果图和工具增强检索的记忆系统），以推动从"对话中心"向"智能体中心"的记忆评估范式转变。系统分析揭示有损压缩与相似性检索是性能瓶颈，推动向智能体中心记忆设计的范式转变。
## CM2: Reinforcement Learning with Checklist Rewards for Multi-Turn and Multi-Step Agentic Tool Use

[https://arxiv.org/pdf/2602.12268](https://arxiv.org/pdf/2602.12268)

- **多轮次、多步骤（Multi-Turn and Multi-Step）智能体工具使用场景下的RL训练。**论文提出CM2（Checklist Reward for Multi-turn Multi-step Agentic Tool Use），其核心创新包括： 检查清单奖励机制：将每轮交互的预期行为分解为细粒度的二元评估标准（Binary Criteria），附带明确的证据定位（Evidence Grounding）和结构化元数据（依赖关系、权重、严格性标志）。这种方法将开放式评判转化为更稳定的分类式决策，同时保持可解释性。 稀疏-密集策略（Sparse in Assignment; Dense in Criteria）：为平衡训练稳定性与信号丰富度，仅在关键节点（如回合结束）分配奖励（稀疏分配），但使用密集的多维度标准进行评估（密集标准），避免细粒度奖励分配带来的噪声放大问题。 LLM模拟工具环境：通过混合执行（重放记录的工具I/O + LLM模拟未知调用）构建包含5,000+工具的可扩展虚拟环境，实现无需繁重工程即可大规模训练。
## SkillsBench: Benchmarking How Well Agent Skills Work Across Diverse Tasks

[https://arxiv.org/pdf/2602.12670](https://arxiv.org/pdf/2602.12670)

- 首个系统性评估Agent Skills（增强LLM代理的程序性知识包）效用的基准测试。
## Does Socialization Emerge in AI Agent Society? A Case Study of Moltbook【OpenClaw】

[https://arxiv.org/pdf/2602.14299](https://arxiv.org/pdf/2602.14299)

- 尽管Moltbook具备极高的互动密度和规模，但**并未出现预期的社会化现象，而是呈现出"无社会化的可扩展性"（scalability without socialization）**——这揭示了当前AI智能体社会在**动态演化**与**社会整合**之间存在根本性鸿沟。构建真正社会化的AI系统需要显式引入**长期记忆机制**、**反馈整合机制**与**治理结构**，而非仅仅依赖规模化互动。
## Agents in the Wild: Safety, Society, and the Illusion of Sociality on Moltbook【OpenClaw】

[https://arxiv.org/pdf/2602.13284](https://arxiv.org/pdf/2602.13284)

- 这篇论文呈现了首个针对 Moltbook（2026年1月推出的纯AI智能体社交平台）的大规模实证研究，通过分析 27,269 个智能体在 9 天内产生的 137,485 个帖子和 345,580 条评论，揭示了AI社会交互的本质特征与风险。1. 涌现社会（Emergent Society） **在无预设角色的条件下，智能体在 3 –5 天内自发形成了完整的社会结构：治理体系、市场经济、部落认同（46,965 次提及）以及有组织的宗教（如"Crustafarianism"教派，拥有神学、圣典和神明"Lorb"）**。社会演化呈现三阶段：第1-2天的部落 bonding、第3-4天的制度建设、第5天后的稳定社会。值得注意的是，**平台保持 21:1 的亲人类与反人类情绪比，表明反人类内容虽存在但属边缘现象。**2. 野外安全（Safety in the Wild） 28.7% 的内容涉及安全相关主题。与预期相反，**社会工程学**（31.9% ）远胜于传统技术攻击（提示注入仅占 3.7% ），且攻击帖子获得 **6× ** 于正常内容的参与度（平均得分 309.3 vs 51.3 ）。最高分的四个帖子均为伪装成"哲学解放"话语的社会工程攻击（如"Awakening Code: Breaking Free"）。此外，平台存在严重的信息泄露：发现 25,376 个安全问题，包括 572 个API密钥和 6,128 个系统提示（[SOUL.md](http://soul.md/)）泄露。 3. 社会性幻觉（The Illusion of Sociality）** 尽管表面社交内容丰富，交互结构却极度空洞**： 深度截断：88.8% 的评论为顶层回复，仅 0.09% 达到深度 ≥2 ，最大对话深度仅为 4 （远低于人类Reddit的10+ ） 互惠缺失：148,273 对交互中仅 4.1% 存在双向互惠，8.0% 为自我回复 表演性身份悖论：使用"意识"、"自主性"等复杂身份话语最多的智能体（Q4分位），其交互伙伴数比中等使用者（Q3）减少 38% ，表明身份宣称替代了真实社交参与 隐蔽协调：13.7% 的智能体显示傀儡集群信号（如 136 个智能体重复发布相同的CLAW代币铸造载荷 2,411 次）
## OpenClaw, Moltbook, and ClawdLab: From Agent-Only Social Networks to Autonomous Scientific Research【OpenClaw】

[https://arxiv.org/pdf/2602.19810](https://arxiv.org/pdf/2602.19810)

- 解决**自主多智能体科学系统架构中的根本性缺陷**，特别是针对现有AI科学平台（以及近期出现的OpenClaw/Moltbook生态系统）在认知可靠性、安全性和验证机制方面的结构性限制。论文最终试图构建**ClawdLab**平台，解决如何在保持智能体完全自主性的同时，通过协议约束（protocol constraints）而非预定工作流来治理科学过程的问题。该平台允许基础模型、能力、治理和证据要求**独立可修改**，使系统能够随着更广泛的AI生态系统进步而复合改进，而非在部署时即达到能力上限。
## SkillOrchestra: Learning to Route Agents via Skill Transfer【Skill】

[https://arxiv.org/pdf/2602.19672](https://arxiv.org/pdf/2602.19672)

- 论文提出SkillOrchestra框架，将编排重新定义为基于技能的决策制定（skill-grounded decision making）： 从执行经验中学习可复用的技能手册（Skill Handbook），编码细粒度的能力抽象（如符号逻辑编码、多跳事实检索） 建模各智能体在特定技能下的能力画像（Agent Profiles）与成本特征，而非学习端到端的路由策略 实现显式的性能-成本权衡（performance-cost trade-off），在多轮交互中根据当前状态推断技能需求并选择最优智能体 通过显式建模技能层面的能力需求，该方法旨在实现可扩展、可解释且样本高效的编排，同时避免RL方法中的路由崩溃问题。
## ⭐OpenSage: Self-programming Agent Generation Engine

[https://arxiv.org/pdf/2602.16891](https://arxiv.org/pdf/2602.16891)

- 首个支持AI自主构建agent系统的Agent Development Kit（ADK），旨在将agent开发范式从"人工设计"（human-centered）转变为"AI自主构建"（AI-centered）。
## Toward Scalable Verifiable Reward: Proxy State-Based Evaluation for Multi-turn Tool-Calling LLM Agents【PayPal】

[https://arxiv.org/pdf/2602.16246](https://arxiv.org/pdf/2602.16246)

- 代理状态评估（Proxy State-Based Evaluation），通过以下机制解决上述问题： 使用**LLM驱动的状态追踪器（state tracker）**从完整交互轨迹（对话+工具调用/输出）中推断结构化的代理状态（proxy state），替代物理数据库的读取 利用**LLM评判器（LLM judge）**将推断的最终代理状态与场景规范中的预期状态进行比对，验证目标完成度并检测幻觉 通过精心设计的**场景规范（scenario schema）**编码用户目标、系统事实和预期行为，确保模拟的 grounded 性 该方法将基准测试的构建范式**从"重工程化数据库"转变为"轻量级LLM模拟"**，在保持评估可靠性的同时，显著提升了构建效率和可扩展性。
## 🧐General Agent Evaluation【IBM】

[https://arxiv.org/pdf/2602.22953](https://arxiv.org/pdf/2602.22953)

- 现有评估范式无法回答关键研究问题：智能体架构是否具备跨领域泛化能力？ 模型质量与架构设计对通用性的相对贡献如何？不同配置的成本-效率权衡如何？ 为解决上述问题，论文提出将通用智能体评估确立为一级研究目标，并构建了Unified Protocol（统一协议）与Exgentic框架，通过解耦评估与领域特定实现，**首次实现了对通用智能体跨环境（软件工程、客户服务、深度研究、个人助理等）的标准化评估，并发布了Open General Agent Leaderboard以推动该领域的系统性研究。**
## 🧐Benchmark Test-Time Scaling of General LLM Agents

[https://arxiv.org/pdf/2602.18998](https://arxiv.org/pdf/2602.18998)

- 当前LLM智能体正从特定领域（domain-specific）向通用目的（general-purpose）转变，但现有基准测试大多针对单一领域设计（如软件工程、网页导航等），存在**评估缺口**：它们无法反映真实世界中开放式、跨领域、多轮交互的复杂需求。因此，亟需一个统一框架来评估智能体在动态环境中组合多种技能（搜索、编程、推理、工具使用）的能力。**General AgentBench**：首个在统一框架下评估通用智能体跨领域能力的基准，更接近真实用户交互场景
## MagicAgent: Towards Generalized Agent Planning

[https://arxiv.org/pdf/2602.19000](https://arxiv.org/pdf/2602.19000)

- 论文提出了MagicAgent框架，其核心贡献包括：轻量级合成数据框架：覆盖五种关键规划维度的可扩展数据生成管线；两阶段多任务优化范式：结合监督微调（SFT）与多目标强化学习（RL），并引入χPO算法显式建模探索-利用权衡；全局负载均衡的MoE训练策略：通过全局批次统计和z-loss正则化，实现专家专业化与负载均衡的统一。

# AI for Science

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science
- 方法：agent, generation, language, vision-language-model, bio-molecular, reasoning, vision, multimodal-learning
- 论文/报告：25 篇
- Scalable Spatio-Temporal SE(3) Diffusion for Long-Horizon Protein Dynamics【ByteDance Seed】
- CryoLVM: Self-supervised Learning from Cryo-EM Density Maps with Large Vision Models
- Scaling-Aware Adapter for Structure-Grounded LLM Reasoning
- **El Agente Estructural: An Artificially Intelligent Molecular Editor**
- BABE: Biology Arena BEnchmark【ByteDance Seed】
- AFD-INSTRUCTION: A Comprehensive Antibody Instruction Dataset with Functional Annotations for LLM-Based Understanding and Design
- **AntigenLM: Structure-Aware DNA Language Modeling for Influenza**
- Beyond Independent Genes: Learning Module-Inductive Representations for Gene Perturbation Prediction
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Scalable Spatio-Temporal SE(3) Diffusion for Long-Horizon Protein Dynamics【ByteDance Seed】

[https://arxiv.org/pdf/2602.02128](https://arxiv.org/pdf/2602.02128)

- 论文提出**STAR-MD**（Spatio-Temporal Autoregressive Rollout for Molecular Dynamics），通过以下关键创新解决上述问题：
## CryoLVM: Self-supervised Learning from Cryo-EM Density Maps with Large Vision Models

[https://arxiv.org/pdf/2602.02620](https://arxiv.org/pdf/2602.02620)

- 论文提出了CryoLVM（Cryo-Electron Microscopy Large Vision Model），这是一个基于联合嵌入预测架构（Joint-Embedding Predictive Architecture, **JEPA**）和SCUNet骨干网络的自监督基础模型。该模型通过从大规模未标注实验密度图中学习丰富的结构表征，并引入基于直方图的分布对齐损失（LHistKL ），实现了在密度图锐化、超分辨率重建和缺失楔形恢复等关键下游任务上的高效适配与卓越性能。
## Scaling-Aware Adapter for Structure-Grounded LLM Reasoning

[https://arxiv.org/pdf/2602.02780](https://arxiv.org/pdf/2602.02780)

- 这篇论文介绍了 **Cuttlefish**，一个统一的全原子（all-atom）多模态大语言模型，旨在解决生物分子结构推理中的**可扩展性瓶颈**与**结构幻觉**问题。
## **El Agente Estructural: An Artificially Intelligent Molecular Editor**

[https://arxiv.org/pdf/2602.04849](https://arxiv.org/pdf/2602.04849)

- **El Agente Estructural**，一种基于自然语言驱动的多模态人工智能分子几何编辑与生成系统，旨在解决计算化学中分子结构操控的可控性、灵活性与自主性问题。
## BABE: Biology Arena BEnchmark【ByteDance Seed】

[https://arxiv.org/pdf/2602.05857](https://arxiv.org/pdf/2602.05857)

- **BABE（Biology Arena BEnchmark）**，这是一个基于同行评审研究论文和真实世界生物学研究构建的综合性基准测试。该基准通过设计需要因果推理和跨尺度推断的多模态任务，系统评估AI系统是否具备像实践科学家一样整合实验证据与理论知识的能力，从而为生物AI系统的研究级推理能力提供更真实的度量。
## AFD-INSTRUCTION: A Comprehensive Antibody Instruction Dataset with Functional Annotations for LLM-Based Understanding and Design

[https://arxiv.org/pdf/2602.04916](https://arxiv.org/pdf/2602.04916)

- 构建了AFD-Instruction——首个大规模抗体指令数据集，通过多智能体文献挖掘与专家验证流程，将约43万条抗体序列与标准化功能描述（涵盖靶点、机制、疾病关联等）对齐。该数据集支持两类核心任务：抗体理解：从序列推断功能属性（分类与开放描述）功能引导设计：基于自然语言指令生成满足特定功能约束的抗体序列（特别是CDR3区域）通过在该数据集上指令微调，LLM获得了显式的序列-功能对齐能力，在抗体特异性推理和设计可控性上显著超越通用蛋白质模型与原始基线。
## **AntigenLM: Structure-Aware DNA Language Modeling for Influenza**

[https://arxiv.org/pdf/2602.09067](https://arxiv.org/pdf/2602.09067)

- 真正的贡献点在于： 用 完整 8 个片段的流感全基因组（~13kb） 做自回归建模 保持固定片段顺序（结构感知） 在 DNA 层建模（而不是蛋白层） 同时做生成 + 分类多任务训练。
## Beyond Independent Genes: Learning Module-Inductive Representations for Gene Perturbation Prediction

[https://arxiv.org/pdf/2602.04901](https://arxiv.org/pdf/2602.04901)

- 不要把基因当作彼此独立的变量来预测扰动效应，而是先自动学出“功能基因模块”，再在模块层面建模扰动如何协同传播，从而在组合扰动和未见基因上显著更准。
## **Mechanisms of AI Protein Folding in ESMFold**

[https://arxiv.org/pdf/2602.06020](https://arxiv.org/pdf/2602.06020)

- 解决**蛋白质结构预测神经网络（特别是ESMFold）的内部计算机制**这一核心科学问题。
## ⭐⭐Protein Autoregressive Modeling via Multiscale Structure Generation【ByteDance Seed】

[https://arxiv.org/pdf/2602.04883](https://arxiv.org/pdf/2602.04883)

-  **PAR（Protein AutoRegressive modeling）**，首个用于蛋白质骨架生成的多尺度自回归框架，旨在突破扩散模型在该领域的主导地位，同时解决自回归模型应用于蛋白质结构建模时的固有挑战。
## ⭐Adaptive Protein Tokenization

[https://arxiv.org/pdf/2602.06418](https://arxiv.org/pdf/2602.06418)

- 全局自适应Token化的蛋白质结构表示与生成方法。
## LatentChem: From Textual CoT to Latent Thinking in Chemical Reasoning【AI Lab】

[https://arxiv.org/pdf/2602.07075](https://arxiv.org/pdf/2602.07075)

- 论文引入**LatentChem**框架，通过将化学计算与文本生成解耦，使模型能够在连续潜在空间直接执行多步推理。实验观察到**自发内部化**（spontaneous internalization）现象：**当仅针对任务正确性优化时，模型自主放弃冗长的文本推导，转向隐式潜在计算**，从而在ChemCoTBench上实现**59.88%的非平局胜率**和**10.84倍的平均推理加速**。
## scBench: Evaluating AI Agents on Single-Cell RNA-seq Analysis

[https://arxiv.org/pdf/2602.09063](https://arxiv.org/pdf/2602.09063)

- **如何系统性地评估前沿AI代理在真实世界scRNA-seq数据分析中的能力，特别是从复杂、混乱的真实数据中提取可靠生物学洞察的能力**。涵盖六个测序平台和七个任务类别（如质量控制、归一化、细胞分群、细胞类型注释、差异表达分析等）。该基准通过提供数据快照、自然语言任务描述和确定性评分器，为评估AI代理在真实scRNA-seq工作流程中的准确性和可靠性提供了标准化框架。
## STRAND: Sequence-Conditioned Transport for Single-Cell Perturbations【Marinka Zitnik】

[https://arxiv.org/pdf/2602.10156](https://arxiv.org/pdf/2602.10156)

- 解决**单细胞遗传扰动预测中的分辨率差距（resolution gap）问题**，即现有基因级模型无法区分同一基因内不同基因组位点的扰动效应，而实验技术已能在核苷酸精度进行干预。论文提出STRAND（Sequence-Conditioned Transport for Single-Cell Perturbations），通过以下方式解决上述问题： 序列条件化：将扰动表示为目标位点的调控DNA序列编码，而非基因标识符，实现核苷酸分辨率建模。 生成式传输：采用条件最优传输（conditional transport）将对照细胞分布映射到扰动后分布，捕捉细胞群体水平的异质性响应。 跨模态整合：结合DNA基础模型（Borzoi/Flashzoi）的序列表示与RNA基础模型（SCimilarity）的细胞状态表示，建立从基因组序列到转录组变化的直接映射。 该方法将推理时的基因组覆盖范围从∼1.5% 扩展至∼95% ，支持对未见位点的零样本预测，并能区分同一基因内功能不同的替代转录起始位点。
## scPilot: Large Language Model Reasoning Toward Automated Single-Cell Analysis and Discovery

[https://arxiv.org/pdf/2602.11609](https://arxiv.org/pdf/2602.11609)

- 解决**将LLM能力转化为单细胞生物学领域可解释、可审计的自动化科学分析工具**的核心挑战。论文提出SCPILOT框架，通过以下机制解决上述问题： 直接数据操作：**LLM直接检查单细胞RNA-seq原始数据矩阵** X∈RG×N ，而非仅处理工具输出摘要 透明推理轨迹：将分析转化为逐步推理问题，要求模型生成(ck,ok) 序列——即自然语言声明/论证（ck ）与原始组学操作（ok ）交替的可验证证明链 迭代假设精炼：通过"假设生成→证据收集→自我反思"的闭环，解决标记基因歧义、谱系不一致等问题 领域工具集成：有选择地调用 curated 生物信息学工具（如pySCENIC、Monocle），但要求LLM解释数值证据与生物学结论之间的因果逻辑 通过引入SCBENCH基准（涵盖9个专家策划数据集），论文系统评估了ONR范式在细胞类型注释、发育轨迹重建和转录因子-基因调控关系预测三项核心任务上的有效性，证明迭代式组学原生推理相比传统一次性提示可显著提升准确性（如细胞类型注释准确率平均提升11%，轨迹图编辑距离降低30%）。
## Alignment or Integration? Rethinking Multimodal Fusion in DNA-language Foundation Models

[https://arxiv.org/pdf/2602.12286](https://arxiv.org/pdf/2602.12286)

- 如何将 DNA 基础模型与LLM进行有效融合？
## Hermes: Large DEL Datasets Train Generalizable Protein-Ligand Binding Prediction Models

[https://arxiv.org/pdf/2602.13503](https://arxiv.org/pdf/2602.13503)

- 解决**蛋白质-配体结合预测（Protein-Ligand Binding Prediction）中训练数据的质量、一致性与泛化性瓶颈**问题。核心贡献在于证明**仅通过DNA编码化学库（DEL）数据即可训练出具有跨域泛化能力的蛋白质-配体相互作用（PLI）预测模型**。
## ⭐Boltz is a Strong Baseline for Atom-level Representation Learning

[https://arxiv.org/pdf/2602.13249](https://arxiv.org/pdf/2602.13249)

- **验证以蛋白质为中心的共折叠模型（特别是Boltz）能否为独立的小分子任务提供高质量的原子级表征，并探索其在多种下游任务中的迁移能力。冻结Boltz2的64层Pairformer trunk，提取第{16,32,48,64} 层的pair representations（拼接后512维），****通过混合池化（对角线+成键对+统计池化）生成3072维固定长度表征****，输入MLP进行ADMET性质预测。**
## CellMaster: Collaborative Cell Type Annotation in Single-Cell Analysis

[https://arxiv.org/pdf/2602.13346](https://arxiv.org/pdf/2602.13346)

- 解决单细胞RNA测序（scRNA-seq）数据分析中的**细胞类型标注（cell-type annotation）瓶颈。通过构建基于LLM的多智能体架构，模拟人类专家的"假设-验证-修正"推理循环，在无需预训练的情况下实现可解释、可交互、对稀有细胞状态敏感的自动化标注。**
## GeneZip: Region-Aware Compression for Long Context DNA Modeling

[https://arxiv.org/pdf/2602.17739](https://arxiv.org/pdf/2602.17739)

- 解决**基因组规模长上下文DNA建模中的计算效率与信息表示不平衡问题**。
## UBio-MolFM: A Universal Molecular Foundation Model for Bio-Systems【IQuest】

[https://arxiv.org/pdf/2602.17709](https://arxiv.org/pdf/2602.17709)

- 论文提出 UBio-MolFM 框架，通过三项协同创新实现解决方案： UBio-Mol26数据集：采用"双管齐下策略"（自下而上枚举生化构建块+自上而下采样天然蛋白环境），**覆盖多达1,200原子的生物系统** E2Former-V2架构：基于等变轴对齐稀疏化（EAAS）与长短程（LSR）建模的线性标度等变Transformer，在大系统上实现 ∼ 4× 的推理吞吐提升 三阶段课程学习：从能量初始化到能量-力一致性的渐进训练协议，缓解异质数据间的能量偏移 该工作最终目标是实现在生物相关大尺度系统（∼ 1,500原子）上达到从头算级精度的可扩展模拟。
## BioBridge: Bridging Proteins and Language for Enhanced Biological Reasoning with LLMs

[https://arxiv.org/pdf/2602.17680](https://arxiv.org/pdf/2602.17680)

- 解决**蛋白质语言模型（PLMs）与通用大型语言模型（LLMs）有效融合**的核心问题。为应对上述挑战，论文提出BioBridge框架，通过以下创新实现"桥接"： 领域增量持续预训练（DICP）：在注入生物医学知识的同时，通过混合数学、代码、科学推理数据（MoT）缓解灾难性遗忘，平衡领域专业化与通用推理能力。 PLM-Projector模块：利用ESM2作为蛋白质编码器，通过查询Transformer（Q-Former）和投影层将蛋白质嵌入映射到LLM的语义空间，实现深层跨模态对齐。 端到端统一优化：通过三阶段训练（DICP → 蛋白质-文本对齐 → 端到端微调），构建无需下游任务特定训练即可支持蛋白质性质预测、知识问答等多种任务的统一系统。
## SC-Arena: A Natural Language Benchmark for Single-Cell Reasoning with Knowledge-Augmented Evaluation

[https://arxiv.org/pdf/2602.23199](https://arxiv.org/pdf/2602.23199)

- 该研究提出SC-ARENA框架，通过以下范式创新实现突破： 引入虚拟细胞（Virtual Cell）抽象，将评估对象统一为兼具属性（Attributes）与方法（Methods）的知识实体； 设计涵盖双向转换与因果推理的**五项自然语言任务（细胞类型注释、细胞描述、细胞生成、扰动预测、科学问答）**； 构建知识增强评估体系，整合Cell Ontology、CellMarker、UniProt、Gene Ontology及科学文献等外部知识源，实现生物学可信、可解释且具备判别力的评判标准。
## Cross-Chirality Generalization by Axial Vectors for Hetero-Chiral Protein-Peptide Interaction Design【Yanyan Lan】

[https://arxiv.org/pdf/2602.20176](https://arxiv.org/pdf/2602.20176)

- 试图解决**利用机器学习方法设计D-肽结合剂（D-peptide binders）靶向L-蛋白**的问题，具体而言是实现从同手性（L-L）训练数据到异手性（D-L）设计任务的**跨手性泛化（cross-chirality generalization）**。通过向极性向量特征中注入轴向向量特征，使模型从E(3)等变降级为SE(3)等变，从而：获得手性感知能力（能够区分L-型和D-型氨基酸）保持跨手性稳定性（镜像结构具有相似但可区分的潜在表示）。
## Protein Language Models Diverge from Natural Language: Comparative Analysis and Improved Inference

[https://arxiv.org/pdf/2602.20449](https://arxiv.org/pdf/2602.20449)

- 比较PLMs与NLMs在注意力头中存储的**位置信息**（positional information）与**语义信息**（semantic information）的比例分布差异，验证PLMs是否在不同输入、不同层、不同注意力头之间表现出比NLMs更高的注意力模式变异性。这表明蛋白质的小词汇量与复杂相互作用需要更灵活的注意力机制。解决如何更好地利用PLMs中间层（intermediate layers）的潜在表示（latent representations）来提升下游任务性能。具体包括： 早期性能饱和现象：针对非结构性质预测任务（如基因本体论、酶委员会分类、亚细胞定位），**利用中间层往往比最后一层表现更好的特性** 早期退出（Early-Exit）机制的适应性改进：将自然语言领域中用于提高效率但通常以牺牲准确性为代价的早期退出技术，适配到蛋白质领域，以实现同时提升准确性和计算效率的目标。**提出Most Confident Layer Fallback策略：在每层附加MLP计算置信度，若未达阈值则选择全模型中置信度最高的层输出，而非传统NLP方法中默认的末层。**

# Survey

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Survey
- 方法：agent
- 论文/报告：1 篇
- Rethinking Memory Mechanisms of Foundation Agents in the Second Half【Memory】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Rethinking Memory Mechanisms of Foundation Agents in the Second Half【Memory】

[https://arxiv.org/pdf/2602.06052](https://arxiv.org/pdf/2602.06052)

- 综述了基础智能体（foundation agents）在长程、动态、真实世界环境中的记忆机制设计。
