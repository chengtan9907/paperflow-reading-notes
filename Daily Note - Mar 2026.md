---
source: https://luminous-mat-781.notion.site/Daily-Note-March-2026-317e8c0e89c580c880e6c662bbee9639?source=copy_link
---

# Daily Note - Mar 2026

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Daily Note - Mar 2026
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, vision, retrieval
- 论文/报告：27 篇
- Reason to Contrast: A Cascaded Multimodal Retrieval Framework【Retrieval, Meta】
- Keyword search is all you need: Achieving RAG-Level Performance without vector databases using agentic tool use【Retrieval, Amazon】
- Can Unified Generation and Understanding Models Maintain Semantic Equivalence Across Different Output Modalities?【Unified Model】
- UniG2U-Bench: Do Unified Models Advance Multimodal Understanding?【Unified Model】
- InternVL-U: Democratizing Unified Multimodal Models for Understanding, Reasoning, Generation and Editing【Unified Model】
- Towards Unified Multimodal Interleaved Generation via Group Relative Policy Optimization【Unified Model】
- Flash-Unified: A Training-Free and Task-Aware Acceleration Framework for Native Unified Models【Unified Model】
- HYDRA: Unifying Multi-modal Generation and Understanding via Representation-Harmonized Tokenization【Unified Model】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Reason to Contrast: A Cascaded Multimodal Retrieval Framework【Retrieval, Meta】

[https://arxiv.org/pdf/2602.23369](https://arxiv.org/pdf/2602.23369)

- 论文提出了 TTE-v2 框架，通过以下机制实现基于令牌预算（token-wise）的测试时扩展： ECR-based Reranking (ECRR)：利用已生成的嵌入中心推理（ECR）进行轻量级文本重排序，将昂贵的多模态重排序转化为高效的文本-only 操作。 Query-Aware Reasoning (QAR)：在重排序前对候选者进行查询感知的推理重写，引入更具区分性的信息。 rHNM（Reranking-based Hard Negative Mining）：利用重排序器的反馈进行硬负样本挖掘和假负样本过滤，形成从下游重排序器到上游检索器的知识蒸馏闭环。
## Keyword search is all you need: Achieving RAG-Level Performance without vector databases using agentic tool use【Retrieval, Amazon】

[https://arxiv.org/pdf/2602.23368](https://arxiv.org/pdf/2602.23368)

- **在文档问答任务中，基于代理的关键词搜索能否****在不依赖向量数据库和语义搜索的情况下，达到与传统检索增强生成（RAG）系统相当的性能****？**研究设计了两套对称的问答系统作为对比： 基线RAG系统：采用Amazon Titan Text Embedding Model V2生成1024维向量，存储于OpenSearch Serverless索引，使用300 token分块（20%重叠）及Top-5检索策略，生成模型为Claude 3 Sonnet。 代理关键词搜索系统：基于ReAct（Reasoning + Acting）框架构建，使用完全相同的LLM（Claude 3 Sonnet）但无向量数据库。代理通过Linux shell工具（[pdfmetadata.sh](http://pdfmetadata.sh/)、rga、pdfgrep）执行动态检索，采用迭代式元数据分析→广泛关键词匹配→精准页码定位的策略，通过多轮工具调用逐步收敛答案。
## Can Unified Generation and Understanding Models Maintain Semantic Equivalence Across Different Output Modalities?【Unified Model】

[https://arxiv.org/pdf/2602.23711](https://arxiv.org/pdf/2602.23711)

- UMM在不同输出模态间保持语义等价性（Semantic Equivalence across Different Output Modalities, SEDOM）的核心问题。
## UniG2U-Bench: Do Unified Models Advance Multimodal Understanding?【Unified Model】

[https://arxiv.org/pdf/2603.03241](https://arxiv.org/pdf/2603.03241)

- UMM中**生成能力对理解能力的促进作用。通过构建涵盖7个推理域（空间智能、几何推理、物理推理、谜题游戏等）、30个细粒度子任务、3000个样本的UniG2U-Bench，论文旨在严格隔离并量化G2U能力，识别生成促进理解的特定认知条件（如空间变换、多轮状态跟踪），并揭示任务结构与模型架构对G2U增益的类一致归纳偏置。****30个细粒度子任务，严格筛选需要中间视觉外化（如绘制辅助线、重建空间布局、跟踪状态变化）才能有效解决的实例。**
## InternVL-U: Democratizing Unified Multimodal Models for Understanding, Reasoning, Generation and Editing【Unified Model】

[https://arxiv.org/pdf/2603.09877](https://arxiv.org/pdf/2603.09877)

- 轻量级的统一多模态模型（Unified Multimodal Model, UMM），旨在以高效架构 democratize（普及化）多模态理解、推理、生成与编辑能力。
## Towards Unified Multimodal Interleaved Generation via Group Relative Policy Optimization【Unified Model】

[https://arxiv.org/pdf/2603.09538](https://arxiv.org/pdf/2603.09538)

- 跨模态对齐薄弱：即使模型能够生成交错内容，也常面临文本与图像之间一致性弱、上下文对齐差的问题，难以满足视觉叙事（visual storytelling）和逐步视觉推理（step-by-step visual reasoning）等任务对细粒度推理和上下文感知合成的要求。论文提出了一种基于GRPO的后训练策略，其核心创新包括： **轻量化解锁机制：通过小规模 curated 交错序列数据进行 warm-up 训练，激活模型潜在的交错生成能力**，同时保留预训练的多模态理解与文本到图像生成能力。 统一策略优化框架：**将文本与图像生成联合建模为单一解码轨迹（single decoding trajectory），扩展 GRPO 至多模态场景，实现模态感知的无缝切换**。 混合奖励设计：**构建包含文本相关性、视觉-文本对齐和结构保真度的多维奖励信号，并引入过程级奖励（process-level rewards）提供逐步反馈，解决复杂多模态任务中稀疏奖励导致的训练低效问题**。 该方法在不依赖大规模高质量交错数据集的前提下，显著提升了统一模型生成连贯、高质量交错内容的能力。
## Flash-Unified: A Training-Free and Task-Aware Acceleration Framework for Native Unified Models【Unified Model】

[https://arxiv.org/pdf/2603.15271](https://arxiv.org/pdf/2603.15271)

- 作者通过系统分析发现，虽然是一个统一模型，但其内部神经元存在明显的专业化分工：**模型内部隐式地形成了针对“理解”和“生成”的两条独立推理路径。**某些神经元对生成任务至关重要，而另一部分则对理解任务更关键。作者提出了 FlashU 框架。其最大的特点是免训练（Training-free）且任务感知（Task-aware）。 A. 通用策略（针对所有任务） 任务特定网络剪枝（Task-Specific Network Pruning）： **根据任务类型，动态剪掉该任务不依赖的神经元/参数（FFN层剪枝**）。 动态层跳过（Dynamic Layer Skipping）： 识别并跳过冗余的中间层，减少计算深度。 B. 针对“生成任务”的优化 自适应引导缩放（Adaptive Guidance Scale）： 采用随时间变化的控制信号，优化采样效率。 扩散头缓存（Diffusion Head Cache）： 利用扩散模型推理过程中的时间冗余性，缓存并重用扩散头的计算结果。 C. 针对“理解任务”的优化 基于 V-Norm 代理的动态 Token 剪枝： 针对视觉输入中的空间冗余（例如图片中背景部分信息量低），动态剪掉不重要的 Token，减少计算量。
## HYDRA: Unifying Multi-modal Generation and Understanding via Representation-Harmonized Tokenization【Unified Model】

[https://arxiv.org/pdf/2603.15228](https://arxiv.org/pdf/2603.15228)

- 如何在一个统一的模型架构中，同时完美兼顾“视觉理解”和“视觉生成”？作者提出了两个关键技术： A. HYDRA-TOK：表示协调的纯 ViT 结构 这是该论文的技术基石。**它不再把理解和生成看作两个平行的任务，而是看作一个渐进的过程**： Gen-ViT（生成阶段）： 首先捕获保留结构信息的原始特征，用于图像重建和生成。 Sem-ViT（语义阶段）： 在生成特征的基础上，进一步演进为高层语义编码。 GSB（Generation-Semantic Bottleneck，生成-语义瓶颈）： 这是两阶段之间的桥梁。它**先将特征压缩到低维空间以滤除生成过程中的噪声，然后恢复维度以支持复杂的语义理解**。 B. HYDRA 统一框架 基于 HYDRA-TOK，作者构建了 HYDRA 语言模型。它将感知（Perception）和生成（Generation）集成在同一个参数空间内，实现真正的“原生统一”。
## Cheers: Decoupling Patch Details from Semantic Representations Enables Unified Multimodal Comprehension and Generation【Unified Model, Zhiyuan Liu】

[https://arxiv.org/pdf/2603.12793](https://arxiv.org/pdf/2603.12793)

- 解决统一UMMs中视觉理解与图像生成任务的内在优化冲突。论文提出CHEERS框架，通过解耦补丁级细节与语义表示（Decoupling Patch Details from Semantic Representations）来重新定义多模态特征建模轨迹： **使用统一的视觉分词器提取稳定的语义表征用于理解任务** **通过级联流匹配头（Cascaded Flow Matching Head）在生成过程中动态注入语义门控的高频细节残差** 实现"先构建全局语义结构，后细化局部纹理"的层次化生成过程，类比人类绘画的"先构图后润色"机制 这一方法在保持4倍Token压缩率的高效建模同时，缓解了理解与生成任务间的优化干扰，实现了在共享特征空间内的高效统一多模态建模。
## End-to-End Training for Unified Tokenization and Latent Denoising【Unified Model, Phillip Isola】

[https://arxiv.org/pdf/2603.22283](https://arxiv.org/pdf/2603.22283)

- 论文提出**UNITE**（Unified Tokenization and Latent Denoising）框架，通过**Generative Encoder**（生成式编码器）的权重共享机制，将tokenization与latent denoising统一为单阶段训练流程，**使重建与生成梯度共同塑造潜在空间**，在ImageNet及分子生成等任务上实现了无需对抗损失或预训练编码器的近SOTA性能。
## MIBench: Evaluating LMMs on Multimodal Interaction【AI Lab】

[https://arxiv.org/pdf/2603.13427](https://arxiv.org/pdf/2603.13427)

- 系统评估大型多模态模型（LMMs）的**多模态交互能力**——即模型根据任务需求，从视觉、文本或两者协同中提取整合信息的能力。基于对30个模型（开源3B-241B，闭源商业模型）的评估： 多模态交互能力普遍受限：**Synergy任务准确率仅40-60%，显著低于单模态任务，表明跨模态深度协同仍是瓶颈**。 显著文本偏见：模型在Vision-centric任务中易受文本干扰，视觉一致性（<60%）远低于文本一致性（~80%），揭示LLM backbone导致的模态不平衡。** 扩展定律失效：参数从3B增至72B，Synergy Level 2/3性能增长缓慢（<50%），表明单纯规模扩展无法克服结构性协同缺陷**。 原生vs非原生架构：**从头训练的原生模型（Emu3、Chameleon）基础感知能力薄弱；基于LLM的非原生模型虽表现更好，但受限于文本偏见**。 认知层次断层：模型在Level 1-2表现尚可，但在Level 3（推理）和Synergy Level 2（跨模态理解）急剧下降，暗示缺乏真正的跨模态涌现能力。
## 📌Phi-4-reasoning-vision-15B Technical Report【Microsoft】

[https://arxiv.org/pdf/2603.03975](https://arxiv.org/pdf/2603.03975)

- **如何构建小型、高效且高性能的多模态推理模型。在仅15B参数和200B训练token的条件下（远低于同类模型通常使用的1万亿+token），实现了与大型模型竞争的推理性能，同时显著降低推理延迟与计算成本。**
## 📌⭐Nemotron-Cascade 2: Post-Training LLMs with Cascade RL and Multi-Domain On-Policy Distillation【NVIDIA】

[https://arxiv.org/pdf/2603.19220](https://arxiv.org/pdf/2603.19220)

- 核心技术创新 级联强化学习（Cascade RL） 论文扩展了前代工作的Cascade RL框架，采用顺序化、分阶段的训练策略替代传统的多领域联合RL。**训练流程严格按以下顺序编排： SFT→IF-RL→Multi-domain RL→MOPD→RLHF→Long-context RL→Code RL→SWE RL**。**多领域在线策略蒸馏（MOPD）** 为解决顺序RL训练中出现的基准性能回归问题，论文引入了MOPD机制。该方法在Cascade RL的关键节点，从各领域最强的中间检查点（teacher models）进行蒸馏。采用GRPO算法，完全去除KL散度项，简化为REINFORCE目标，实施**动态过滤（Dynamic Filtering）和过度长度惩罚（Overlong Penalty）**，确保训练稳定性并控制生成长度。
## 📌MiroThinker-1.7 & H1: Towards Heavy-Duty Research Agents via Verification【MiroMind】

[https://arxiv.org/pdf/2603.15726](https://arxiv.org/pdf/2603.15726)

- 针对**长程推理（long-horizon reasoning）任务中交互效率与可靠性不足**的核心问题。论文提出了MiroThinker-1.7和MiroThinker-H1： MiroThinker-1.7通过**agentic mid-training强化每一步的原子能力（规划、推理、工具使用、摘要），使单次交互更可靠、信息密度更高**，从而实现"有效交互扩展"； MiroThinker-H1进一步引入验证中心的重度推理模式（verification-centric heavy-duty reasoning），**在局部（实时评估修正中间步骤）和全局（审计整体证据链并比较候选路径）两个层面嵌入验证机制**，确保最终答案基于最完整、可靠的证据链。 通过这一框架，论文在BrowseComp、FrontierScience-Olympiad、FinSearchComp等多个基准测试上实现了state-of-the-art性能，验证了"通过提升单步质量与引入验证机制来实现可靠长程推理"的技术路径有效性。
## 📌Composer 2 Technical Report【Cursor】

[https://arxiv.org/pdf/2603.24477](https://arxiv.org/pdf/2603.24477)

- **Composer 2**，一个专门面向代理式软件工程（agentic software engineering）的前沿代码大模型，通过持续预训练与大规模强化学习的结合，在保持交互式使用效率的同时，实现了复杂长程编程任务的突破。
## LongCat-Next: Lexicalizing Modalities as Discrete Tokens【LongCat】

[https://arxiv.org/pdf/2603.27538](https://arxiv.org/pdf/2603.27538)

- 论文提出**Discrete Native Autoregressive (DiNA)**范式，****通过**Discrete Native Any-resolution Vision Transformer (dNaViT)**将视觉信号转化为层次化离散令牌，并基于**Residual Vector Quantization (RVQ)**和**Semantic-and-Aligned Encoders (SAE)**实现语义完整性。最终构建的LongCat-Next模型在单一自回归目标下统一处理文本、视觉和音频，突破了离散视觉建模的长期性能瓶颈。
## 📌⭐⭐daVinci-LLM:Towards the Science of Pretraining【Pengfe Liu】

[https://arxiv.org/pdf/2603.27164](https://arxiv.org/pdf/2603.27164)

- 解决**大规模语言模型预训练阶段缺乏系统性科学探索与透明度**的问题。**数据处理的系统性缺失** 领域缺乏对数据质量进行分层分类与比较的系统框架。论文指出，现有实践在数据收集、过滤、处理等关键决策上缺乏透明度和可复现性，研究者难以判断不同数据源的处理深度，也无法确定在何处投入资源进行质量提升最具性价比。预训练动态的科学理解不足 通过200余项控制消融实验，论文揭示了预训练过程中尚未被充分探索的关键机制： 数据处理深度的因果效应：从L0原始数据到L9合成数据的层级处理如何系统性地影响模型能力（如L4生成式精炼对复杂推理的显著提升，L5认知补全对特定领域的定向增强） 能力饱和的动态差异：不同能力维度（通用知识vs.推理能力）呈现显著不同的收敛时间尺度，需要自适应的数据策略调整 数据混合的权衡机制：如何在目标域强化与通用能力保持之间实现平衡，避免灾难性遗忘或表征崩溃
## ARC-AGI-3: A New Challenge for Frontier Agentic Intelligence

[https://arxiv.org/pdf/2603.24621](https://arxiv.org/pdf/2603.24621)

- ARC-AGI-3试图提供一个**未饱和的、抗过拟合的、以人类水平效率为基准的代理智能评估平台**，以准确测量当前AI与人类水平AGI之间的"残余差距"（residual gap）。核心设计：交互式代理智能评估 ARC-AGI-3 采用回合制交互环境（64×64 网格，16 色），**代理通过离散动作（方向键、点击、撤销）与环境互动，观察状态变化**。其评估框架基于四个支柱： 探索：主动获取信息而非被动接收； 建模：构建可预测环境动态的内部世界模型； 目标设定：**关键创新——代理无明确指令，必须自主推断"赢"的条件（terminal frame）**； 规划与执行：在不确定性下规划并修正错误。 环境严格限定于Core Knowledge 先验（物体性、基础几何/拓扑、基础物理、代理性），排除语言、数字和文化符号，确保测试先天推理而非习得知识。 评分机制：相对人类动作效率（RHAE）。智能被定义为**技能获取效率**，具体为**动作效率**（action efficiency）——首次接触环境时完成任务所需的步数。
## T2S-Bench & Structure-of-Thought: Benchmarking and Prompting Comprehensive Text-to-Structure Reasoning

[https://arxiv.org/pdf/2603.03790](https://arxiv.org/pdf/2603.03790)

- 如何找到一种**通用且可靠的中间表示（IR）**，并基于此系统性地评估和提升LLM在通用文本处理任务上的性能？论文提出了**Structure of Thought (SoT)提示技术，****引导模型显式构建关键节点和链接的文本结构****；并构建了T2S-Bench**基准数据集，首次系统性评估模型从文本提取结构的能力，涵盖6个科学领域和32种结构类型。
## Logics-Parsing-Omni Technical Report【Alibaba】

[https://arxiv.org/pdf/2603.09677](https://arxiv.org/pdf/2603.09677)

- 面向文档、图像、音频及视频的全模态统一解析框架，建立覆盖文档、图像、音视频流的统一分类法，定义"全模态解析"为**将非结构化信号转化为可定位（Locatable）、可枚举（Enumerable）、可追溯（Traceable）的标准化知识**。Logics-Parsing-Omni 基于 Qwen3-Omni-30B-A3B 初始化，在 OmniParsingBench（覆盖文档、自然图像、图形、音频、自然视频、文本丰富视频六域）上验证： 图形域：Cognition 得分 92.19 ，超越 Gemini-3-Pro；几何坐标定位准确率 82.24% （基线仅 32.79% ） 音频域：Overall 53.75 （新 SOTA），认知推理显著超越商业模型 文本丰富视频：Overall 66.78% ，超越 Gemini-3-Pro（61.00% ），深度结构化字幕达 89.41% 文档解析：OmniDocBench v1.5 上 Overall 92.54 ，公式与表格识别最优。
## Video Streaming Thinking: VideoLLMs Can Watch and Think Simultaneously

[https://arxiv.org/pdf/2603.12262](https://arxiv.org/pdf/2603.12262)

- **在线视频大语言模型（Online VideoLLMs）在实时交互场景下面临的推理能力与响应延迟之间的根本性权衡问题**。论文提出视频流式思考（VST）范式，通过"边观看边思考"（thinking while watching）机制解决上述矛盾： 推理前置与分摊：在视频流式传输的间隙（intervals）主动生成中间推理（streaming thoughts），将计算成本分摊（amortize）到视频播放过程中，而非集中在查询后 双记忆架构：维护短期原生视觉记忆（short-term native visual memory）和长期文本语义记忆（long-term textual semantic memory），支持无限长视频流的高效推理 时间因果对齐：通过流式注意力掩码（streaming attention mask）确保推理严格遵循视频的时间因果性，避免未来信息泄漏 该范式在不增加查询响应延迟的前提下，实现了测试时缩放（test-time scaling）的性能增益，使模型能够同时满足实时交互需求和复杂推理需求。
## Reading, Not Thinking: Understanding and Bridging the Modality Gap When Text Becomes Pixels in Multimodal LLMs【OCR】

[https://arxiv.org/pdf/2603.09095](https://arxiv.org/pdf/2603.09095)

- **MLLMs中的"模态差距"（modality gap）问题**——即当相同的文本内容以图像像素形式呈现时，**模型性能显著低于以离散文本标记形式输入时的现象**。发现模态差距具有**任务依赖性和数据依赖性**： 在合成渲染的文本图像（如数学题目）上，准确率可能下降超过60个百分点 在自然文档图像（如arXiv PDF或维基百科页面）上，模型往往能达到甚至超越纯文本模式的性能 渲染参数（字体、分辨率、颜色方案）是关键的混淆因素，仅字体选择就能导致准确率波动高达47个百分点。基于"图像模态损害的是阅读而非思考"这一诊断，论文提出**自蒸馏（self-distillation）**方法——利用模型自身在纯文本模式下的推理轨迹作为监督信号，训练其在图像输入下复现相同的多步推理。
## ⭐Qianfan-OCR: A Unified End-to-End Model for Document Intelligence【OCR, Baidu】

[https://arxiv.org/pdf/2603.13398](https://arxiv.org/pdf/2603.13398)

- 论文提出 Qianfan-OCR，一个**4B参数的统一端到端文档智能模型**，通过三项关键设计实现突破： 端到端统一架构：将布局分析、文本识别和语义理解整合于单一视觉-语言架构内，消除阶段间错误传播，保留完整视觉上下文。 **Layout-as-Thought 机制：通过可选的 ⟨think⟩ 令牌触发思考阶段，在生成最终输出前产生结构化布局表示（边界框、元素类型、阅读顺序），既恢复了端到端范式中的布局分析功能，又通过显式结构先验提升复杂版式文档的识别准确性**。 OCR与理解的统一：在单一模型内**同时支持传统OCR任务（文档解析、手写识别、表格提取）和认知密集型任务（图表理解、文档问答、关键信息提取）**，通过提示词（prompt）灵活控制任务类型。
## PP-OCRv5: A Specialized 5M-Parameter Model Rivaling Billion-Parameter Vision-Language Models on OCR Tasks【OCR, Baidu】

[https://arxiv.org/pdf/2603.24373](https://arxiv.org/pdf/2603.24373)

- 论文展示了仅有**500万参数**的PP-OCRv5能够在标准OCR基准测试上达到与十亿参数VLMs相当的性能，同时保持高精度定位、低幻觉率和极高的计算效率，为实际生产环境中的OCR部署提供了一种实用且高效的替代方案。
## Boosting Document Parsing Efficiency and Performance with Coarse-to-Fine Visual Processing【OCR, Baidu】

[https://arxiv.org/pdf/2603.24326](https://arxiv.org/pdf/2603.24326)

- 论文采用解耦的两阶段架构：粗粒度阶段（VRFM）：使用轻量级有效区域聚焦模块（基于RT-DETR和指针网络），快速定位语义相关区域、预测元素类别和阅读顺序，过滤冗余背景细粒度阶段（PaddleOCR-VL-0.9B）：将裁剪后的有效区域输入紧凑的视觉语言模型（基于NaViT动态分辨率编码器和ERNIE-4.5-0.3B语言模型），进行高精度元素识别（文本、表格、公式、图表）。
## ⭐⭐Multimodal OCR: Parse Anything from Documents【OCR, Xiang Bai, Xiaohongshu】

[https://arxiv.org/pdf/2603.13032](https://arxiv.org/pdf/2603.13032)

- 传统文档解析流程以文本为中心，将图表、图示、流程图、UI元素和科学插图等视觉符号视为"非文本区域"，仅作像素级裁剪而丢弃其内在结构。论文提出 **Multimodal OCR (MOCR) 范式**，**将文档解析从"文本提取"扩展为"全要素结构化理解"： 统一表示框架：将文本（Markdown）、表格（HTML）、公式（LaTeX）和图形（SVG）统一为有序文本序列** 图形即代码：**将视觉符号解析为可执行、可编辑的向量代码（如SVG），而非静态像素** 可扩展数据引擎：从PDF、网页渲染和原生SVG资产构建大规模图像-代码对齐语料 渐进式训练策略：通过三阶段预训练（视觉对齐→文本解析→多模态联合）和指令微调，在3B参数规模上实现稳定训练 该范式使文档成为多模态预训练的"数据引擎"，将以往被丢弃的图形转换为可重用的代码级监督信号（image-code pairs），支持对文档的忠实重建和下游推理。
## 📌OpenCompass: A Universal Evaluation Platform for Large Language Models【AI Lab】

[https://arxiv.org/pdf/2605.19276](https://arxiv.org/pdf/2605.19276)

- 论文提出了**OpenCompass**——一个基于模块化与组件解耦设计的一站式、可扩展、支持高并发的通用LLM评估平台，旨在为学术界和工业界提供统一、高效的评估工具，实现对LLM优势与劣势的精准识别及后续优化。

# Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reasoning
- 方法：agent, generation, language, vision-language-model, reasoning, vision, reinforcement-learning, deep-learning
- 论文/报告：34 篇
- Annotation-Free Visual Reasoning for High-Resolution Large Multimodal Models via Reinforcement Learning【Visual Reasoning】
- Thinking with Images as Continuous Actions: Numerical Visual Chain-of-Thought【Visual Reasoning】
- Through the Lens of Contrast: Self-Improving Visual Reasoning in VLMs【Visual Reasoning】
- UniM: A Unified Any-to-Any Interleaved Multimodal Benchmark【Visual Reasoning, Microsoft】
- VisionCreator-R1: A Reflection-Enhanced Native Visual-Generation Agentic Model【Visual Reasoning, Tencent Hunyuan】
- VTC-Bench: Evaluating Agentic Multimodal Models via Compositional Visual Tool Chaining【Visual Reasoning】
- MME-CoF-Pro: Evaluating Reasoning Coherence in Video Generative Models with Text and Visual Hints【Visual Reasoning】
- Let's Think with Images Efficiently! An Interleaved-Modal Chain-of-Thought Reasoning Framework with Dynamic and Precise Visual Thoughts【Visual Reasoning】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Annotation-Free Visual Reasoning for High-Resolution Large Multimodal Models via Reinforcement Learning【Visual Reasoning】

[https://arxiv.org/pdf/2602.23615](https://arxiv.org/pdf/2602.23615)

- 解决**LMMs在处理高分辨率视觉输入时面临的视觉定位（visual grounding）优化难题。论文提出 HART（High-resolution Annotation-free Reasoning Technique） 框架，通过构建闭环自我验证机制解决上述问题： 模型首先****基于下采样图像预测关键感兴趣区域（ROIs） 随后仅依据裁剪后的高分辨率子区域（ withholding 原始全图）回答问题**** 若答案正确，则证明 ROIs 包含了回答问题所需的关键信息，从而实现对定位质量的自我验证 基于此反馈循环，论文进一步提出 AP-GRPO（Advantage Preference Group Relative Policy Optimization） 算法，通过动态加权机制优先优化定位准确的样本，实现在无需外部视觉监督的情况下直接优化模型的 grounding 能力。**
## Thinking with Images as Continuous Actions: Numerical Visual Chain-of-Thought【Visual Reasoning】

[https://arxiv.org/pdf/2602.23959](https://arxiv.org/pdf/2602.23959)

- 把 MLLM 的输出空间从“纯离散词表 token”扩展为“离散 token + 连续坐标动作”。给模型加很轻量的线性 head： 4 个 head 输出 bbox 均值 μ=[x1,y1,x2,y2] 另一个 head 输出不确定性尺度（Gaussian 里是 σ，Laplace 里是 α）
## Through the Lens of Contrast: Self-Improving Visual Reasoning in VLMs【Visual Reasoning】

[https://arxiv.org/pdf/2603.02556](https://arxiv.org/pdf/2603.02556)

- 现有基于文本的自我改进方法（如STaR）直接应用于VLMs时存在根本性局限：这些方法主要关注文本连贯性和最终答案质量，但**无法验证或纠正推理路径中嵌入的视觉幻觉（visual hallucinations）**。这导致模型可能产生"投机性推理"（speculative reasoning），即优先依赖文本先验知识而非真实视觉证据。**VLMs在对比观察时能更准确地"看见"——当呈现由两张视觉相似图像及其同义问题组成的对比性VQA对时，模型能够更精确地捕捉细粒度视觉证据**，从而自发纠正原有的幻觉性推理。
## UniM: A Unified Any-to-Any Interleaved Multimodal Benchmark【Visual Reasoning, Microsoft】

[https://arxiv.org/pdf/2603.05075](https://arxiv.org/pdf/2603.05075)

- **UNIM**（Unified Any-to-Any Interleaved Multimodal Benchmark），这是首个支持7种模态（文本、图像、音频、视频、文档、代码、3D）任意组合的交错多模态基准，包含31,026个高质量实例，并配套建立了包含13个指标的三维评估体系（语义正确性与生成质量、响应结构完整性、交错连贯性），同时提出了具备可追溯推理能力的基线模型UNIMA。提出基于Agent架构的UNIMA（Unified Any-to-Any Interleaved Multimodal Agent），核心创新包括： 接收模块：将7种模态统一转换为任务条件密集描述（TCDC），建立统一语义空间 可追溯证据推理（TER）模块：通过四步结构化推理链（TCDC生成→数据分析判断→内容规划→报告整合）和验证-回溯-再生机制，确保推理过程可验证、可修正 生成模块：调用专用工具（Sora-2、GPT-Image-1、Qwen3-Omni、PCDreamer）生成多模态内容并组装成交错输出。
## VisionCreator-R1: A Reflection-Enhanced Native Visual-Generation Agentic Model【Visual Reasoning, Tencent Hunyuan】

[https://arxiv.org/pdf/2603.08812](https://arxiv.org/pdf/2603.08812)

- 针对**原生视觉生成智能体**（native visual generation agents）中缺乏系统性反思机制、以及计划（planning）与反思（reflection）能力协同优化的挑战。现有视觉生成智能体**主要依赖计划驱动架构，缺乏显式的结构化反思路径来评估中间视觉结果并进行自我纠正**。
## VTC-Bench: Evaluating Agentic Multimodal Models via Compositional Visual Tool Chaining【Visual Reasoning】

[https://arxiv.org/pdf/2603.15030](https://arxiv.org/pdf/2603.15030)

- VTC-Bench 引入了一个综合基准，包含32个基于OpenCV的工具和跨越九类认知层级的680项任务，旨在评估代理式MLLMs。对19个MLLMs的评估揭示了一个普遍的性能上限，其中表现最佳的模型仅达到51.18%，这凸显了**多步骤工具编排和组合视觉推理**方面的重大挑战。目录代理多模态模型的演进VTC-Bench 的设计与构成视觉推理的分层框架量化智能体性能模型熟练度和扩展性观察组合瓶颈对未来研究的启示。
## MME-CoF-Pro: Evaluating Reasoning Coherence in Video Generative Models with Text and Visual Hints【Visual Reasoning】

[https://arxiv.org/pdf/2603.20194](https://arxiv.org/pdf/2603.20194)

- 论文提出MME-CoF-Pro基准测试，通过以下方式解决上述问题： 过程级评估指标：引入Reasoning Score (RS)，基于人工验证的逐步评分标准（step-level rubric），评估必要的中间推理步骤完成情况；可控提示设置：设计三种评估条件隔离推理指导的影响： (a) 无提示（no hint）：基线设置 (b) 文本提示（text hint）：提供显式推理步骤描述 (c) 视觉提示（visual hint）：通过边界框/箭头提供空间指导 细粒度诊断：覆盖16个类别、303个样本，从感知推理（如视觉细节）到物理因果推理（如4D动力学），实现跨模型、跨条件的对比分析。 简言之，该工作填补了视频生成模型在因果推理连贯性评估方面的空白，并提供了控制推理提示以诊断模型推理机制的工具。
## Let's Think with Images Efficiently! An Interleaved-Modal Chain-of-Thought Reasoning Framework with Dynamic and Precise Visual Thoughts【Visual Reasoning】

[https://arxiv.org/pdf/2603.21754](https://arxiv.org/pdf/2603.21754)

- 论文提出DAP-ICOT（具有动态且精确视觉思维的交错模态思维链推理框架），通过以下两个核心模块实现改进： 动态视觉思维整合（Dynamic Visual Thought Integration）：**基于模型置信度自适应地决定何时需要引入视觉输入，**显著减少冗余计算 精确视觉思维引导（Precise Visual Thought Guidance）：**利用对象级分割（SAM2）生成语义连贯、上下文对齐的完整视觉表征** 实验表明，该方法在保持先进性能的同时，实现了72.6%的token消耗降低，有效解决了ICoT推理中的效率与表征质量问题。
## Gym-V: A Unified Vision Environment System for Agentic Vision Research【Visual Reasoning】

[https://arxiv.org/pdf/2603.15432](https://arxiv.org/pdf/2603.15432)

- Gym-V 提供了一个类似于 OpenAI Gym 的统一平台，包含以下核心组件： 规模化环境： 包含 10 个领域（如推理游戏、视觉导航等）的 179 个程序化生成的视觉环境。这些环境支持可控的难度设置。 统一接口： 一个 API 即可统一交互式训练、离线监督学习和基准测试评估。实验发现，**智能体能否训练成功，更多取决于它看到了什么（例如是否有图像标题或规则说明）**，而不是选择了哪种强化学习算法。
## When Models Judge Themselves: Unsupervised Self-Evolution for Multimodal Reasoning【Visual Reasoning, OPPO】

[https://arxiv.org/pdf/2603.21289](https://arxiv.org/pdf/2603.21289)

- **MLLMs在无监督环境下进行推理能力自我进化。论文提出通过Actor-Judge 协同建模和组级分布优化来解决上述问题： 利用 Actor 的自一致性建立初始奖励分布 引入有界的 Judge 模块提供连续的质量调制信号，避免过度依赖单一评估标准 通过组内相对优势（Group Relative Advantage）而非绝对分数进行策略更新，防止早期主导模式的自我强化，维持稳定的探索能力 该方法旨在实现无需任何人工标注答案或外部奖励模型的稳定、持续自我改进。**
## ATLAS: Agentic or Latent Visual Reasoning? One Word is Enough for Both【Visual Reasoning, Meta】

[https://arxiv.org/pdf/2605.15198](https://arxiv.org/pdf/2605.15198)

- 统一模型（Unified Models）通过直接生成像素级图像来进行视觉推理，虽然直观且可解释，但存在以下问题： 推理成本高昂（substantial inference cost） 需要分配大量模型容量给图像解码和重新编码 需要非平凡的架构级设计，通常需要从头预训练；代理式视觉推理（Agentic Visual Reasoning）将视觉语言模型（VLM）作为高层控制器，生成代码或工具调用来操作视觉输入，但存在： 上下文切换延迟（context-switching latency） 即使对于简单的视觉操作也需要冗长的代码或工具调用表述（verbose code or tool-call formulations） 增加了输出长度和推理延迟；潜在式方法的训练与泛化局限 潜在视觉推理（Latent Visual Reasoning）在隐藏表示中进行中间推理，但面临： 缺乏任务泛化能力（lack task generalization） 引入循环潜在依赖（recurrent latent dependencies），破坏了与标准并行训练（autoregressive parallelization）的兼容性。ATLAS框架 为结合上述方法的优势并规避其局限，论文提出ATLAS（Agentic or Latent? One Word is Enough），其核心创新在于： **将视觉操作表示为词汇表中的单个离散功能令牌（functional token），如 <|Line|> 、<|Shape|> 等 这些令牌既是代理式操作（agentic operation）又是潜在推理单元（latent reasoning unit）** 无需视觉监督（no visual supervision），**仅通过标准的next-token prediction生成 完全兼容现有的监督微调（SFT）和强化学习（RL）流程**，无需架构或方法论修改 此外，论文还针对RL训练过程中功能令牌稀疏性导致的梯度稀释（gradient dilution）问题，提出了Latent-Anchored GRPO (LA-GRPO)，通过静态加权的辅助目标稳定功能令牌的优化。
## Semantic-Enriched Latent Visual Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2605.19342](https://arxiv.org/pdf/2605.19342)

- 解决**多模态潜在空间推理中视觉表示语义丰富性不足及跨查询一致性缺失**的问题。论文提出了**Semantic-Enriched Latent Visual Reasoning (SLVR)**框架，核心解决思路包括：** 细粒度语义增强：通过属性级监督（如外观、动作、空间属性）显式丰富潜在表示的语义内容**，使潜在向量能够编码结构化的区域语义信息。 多查询对齐机制：设计Multi-query Group Relative Policy Optimization (M-GRPO)方法，**在保持潜在表示一致性的同时，对齐同一视觉区域上的多样化推理目标，确保潜在表示能够稳定支持不同语义角度的查询**。 基准与数据支持：构建SLV-Set数据集（含约400K区域级属性注释和800K多查询问答对）及SV-QA基准测试，为语义增强的潜在推理提供训练和评估基础.
## From Seeing to Thinking: Decoupling Perception and Reasoning Improves Post-Training of Vision-Language Models【Visual Reasoning】

[https://arxiv.org/pdf/2605.20177](https://arxiv.org/pdf/2605.20177)

- 解决**视觉语言模型（VLMs）在后训练阶段中视觉感知能力与推理能力耦合导致的性能瓶颈问题**。论文提出分阶段后训练范式（Staged Post-training），**将VLM能力解耦为三个独立阶段**： Stage 1: 视觉感知（Visual Perception）——建立准确的视觉表征 Stage 2: 文本推理（Textual Reasoning）——强化逻辑推理能力 Stage 3: 视觉推理（Visual Reasoning）——整合感知与推理 该框架证实视觉感知应作为基础支架（fundamental scaffold）优先巩固，且通过强化学习（RLVR）而非传统的标题监督微调（SFT）进行训练效果更佳。
## Thinking with Spatial Code for Physical-World Video Reasoning【Video Reasoning】

[https://arxiv.org/pdf/2603.05591](https://arxiv.org/pdf/2603.05591)

- 解决MLLMs在物理世界视频推理中缺乏显式三维空间理解能力的问题。论文提出了"Thinking with Spatial Code"框架，其核心解决方案包括： 显式空间编码：将RGB视频转换为结构化的时空3D表示（spatial code），包含带有语义标签的显式3D定向边界框 感知-推理分离：通过空间编码器（Spatial Encoder）统一6D物体解析与跟踪，生成包含物体位置、尺寸和朝向的符号化空间代码 强化学习优化：利用空间规则奖励（spatial rubric reward）对LLM进行微调，鼓励视角感知、几何基础推理，纠正"推理-行动脱节"问题
## ⭐Demystifying Video Reasoning【Video Reasoning, Shangtang】

[https://arxiv.org/pdf/2603.16870](https://arxiv.org/pdf/2603.16870)

- 此前研究将视频推理归因于**Chain-of-Frames (CoF)** 机制，即假设推理是沿视频帧序列顺序展开的。论文通过实证分析挑战了这一假设，**证明推理并非主要发生在时间维度（跨帧），而是沿着扩散去噪步骤（diffusion denoising steps） 进行**，并将这一新机制命名为 **Chain-of-Steps (CoS)**。论文深入探究了推理在扩散步骤中如何具体展开： 多路径探索（Multi-path Exploration）：早期去噪步骤同时探索多种可能的解决方案（如并行搜索多个路径或候选动作），随后逐步剪枝收敛至最终答案 基于叠加的探索（Superposition-based Exploration）：模型会在早期步骤中同时表示多个互斥的逻辑状态，通过去噪过程逐渐解析确定唯一解。基于上述机制理解，论文提出了**无需训练（training-free）** 的推理改进方法：**通过在关键早期去噪步骤中对不同随机种子的潜在轨迹进行集成（ensembling），利用模型固有的多路径探索特性，引导其收敛至更正确的解。**
## RubricBench: Aligning Model-Generated Rubrics with Human Standards【Rubric, Tencent Hunyuan】

[https://arxiv.org/pdf/2603.01562](https://arxiv.org/pdf/2603.01562)

- **基于评分标准（rubric-guided）的奖励模型评估缺乏统一基准，**论文提出了 **RubricBench**，这是一个包含 1,147 对比较、经多维过滤筛选的困难样本集，每个样本均配备严格从指令派生的人类注释原子化评分标准。
## Autorubric: A Unified Framework for Rubric-Based LLM Evaluation【Rubric】

[https://arxiv.org/pdf/2603.00077](https://arxiv.org/pdf/2603.00077)

- **基于评分标准（rubric-based）的LLM评估领域存在的技术碎片化与标准化缺失问题**。论文通过提出**Autorubric**——一个开源Python框架——来系统性地解决上述问题。该框架将评分标准设计（支持二元、序数、名义标准）、评判策略（单评委/多评委集成）、失效模式缓解（位置偏差、冗长偏差、标准混淆）以及心理测量学可靠性指标整合为统一的API，并通过在三个基准测试（RiceChem、ResearcherBench、CHARM-100）上的验证证明其有效性。
## A Rubric-Supervised Critic from Sparse Real-World Outcomes【Rubric】

[https://arxiv.org/pdf/2603.03800](https://arxiv.org/pdf/2603.03800)

- 训练critic模型同时执行： Rubric预测：预测24个行为特征（密集监督，所有154K segments均有标签）；成功预测：预测segment成功率（稀疏监督，仅4-6% segments有标签） 这种联合训练使模型能够从大量无结果标签的数据中学习有意义的表征。
## Experience is the Best Teacher: Motivating Effective Exploration in Reinforcement Learning for LLMs【Rubric】

[https://arxiv.org/pdf/2603.20046](https://arxiv.org/pdf/2603.20046)

- **rubric-based RL在LLM推理任务中探索效率低下**的问题。论文基于奖励假设（Reward Hypothesis）指出，有效的探索应与优化目标显式对齐。**利用评分标准（rubrics）提供的细粒度语言反馈，可以将失败轨迹及其未满足的评分项转化为后见之明经验（hindsight experience）**，作为上下文指导直接告诉模型"如何改进"，从而： 避免从零开始的盲目探索 生成超出当前分布的高奖励样本 实现更准确的策略梯度估计。
## AdaRubric: Task-Adaptive Rubrics for LLM Agent Evaluation【Rubric, Alibaba】

[https://arxiv.org/pdf/2603.21362](https://arxiv.org/pdf/2603.21362)

- 论文提出 ADARUBRIC 框架，通过以下机制解决上述问题： 动态评分标准生成：**根据任务描述T 即时生成特定评估维度R(T) ，包含N 个正交、带校准的5分制评分标准 **置信度加权评估：对每个步骤在每个维度上进行评分，并附加置信度权重ck,j∈[0,1] 维度感知过滤（DimensionAwareFilter）：防止高评分维度掩盖维度级失败，确保偏好对（preference pairs）的质量 该框架在WebArena和ToolBench上将人类相关性（Pearson r ）从0.64提升至0.79，并在DPO训练中实现+6.8至+8.5个百分点的任务成功率提升。
## Qworld: Question-Specific Evaluation Criteria for LLMs【Rubric, Marinka Zitnik】

[https://arxiv.org/pdf/2603.23522](https://arxiv.org/pdf/2603.23522)

- 论文提出**One-Question-One-World (Qworld)** 方法，通过递归扩展树（Recursive Expansion Tree）将问题分解为**场景（scenarios）、视角（perspectives）和细粒度的二元标准（binary criteria）**，从而为每个问题构建特定的"世界"（world），**明确高质量回答必须涵盖的内容**。这种方法在保持与专家标准一致（Coverage 0.89）的同时，生成大量新颖的上下文特定标准（Uniqueness 0.79），显著提升了评估的细粒度与区分度。
## RubricEval: A Rubric-Level Meta-Evaluation Benchmark for LLM Judges in Instruction Following【Rubric, Ant】

[https://arxiv.org/pdf/2603.25133](https://arxiv.org/pdf/2603.25133)

- 首个针对指令遵循的评分标准级别元评估基准，包含3,486个经过质量控制的实例，并设计了**Rubric Arbitration Framework (RAF)** 框架，通过多阶段仲裁机制在降低标注成本的同时确保标签可靠性。
## Not Every Rubric Teaches Equally: Policy-Aware Rubric Rewards for RLVR【Rubric, Scale AI】

[https://arxiv.org/pdf/2605.20164](https://arxiv.org/pdf/2605.20164)

- 解决**基于评分标准（rubric-based）的强化学习中，静态奖励聚合导致的训练信号低效问题**。论文提出**POW3R（Policy-Aware Rubric Reward）**框架，核心思想是： 保留人工权重和类别平衡作为评估目标； 动态调整标准级别的奖励权重，**将训练压力重新分配给当前具有对比性（方差高）的标准**； 通过rollout级对比度（w~(t)j=wjα(t)j ）使GRPO奖励更具信息量，在不改变底层评估目标的前提下提升学习效率 简言之，该论文致力于区分"最终答案中应重要的内容"与"当前能教导策略的内容"，从而解决多维度质量评估中奖励信号稀疏与错配的问题。
## OCR or Not? Rethinking Document Information Extraction in the MLLMs Era with Real-World Large-Scale Datasets【OCR】

[https://arxiv.org/pdf/2603.02789](https://arxiv.org/pdf/2603.02789)

- MLLMs时代下文档信息提取（Document Information Extraction, DIE）流程的范式选择问题？该研究通过大规模实证分析（覆盖约1,000份多语言商业文档），试图证明**端到端的MLLMs-only pipeline可作为传统OCR-based系统的可行替代方案**，并为工业界提供从错误诊断到提示工程的全流程实践指南。
## Scaling Reasoning Efficiently via Relaxed On-Policy Distillation【Microsoft】

[https://arxiv.org/pdf/2603.11137](https://arxiv.org/pdf/2603.11137)

- 如何高效且稳定地将高级推理能力从大型教师模型迁移到容量受限的小型语言模型? **On-policy distillation** 的核心思路是：让学生模型先按自己的当前策略生成答案（on-policy 采样），然后用教师模型来给这些生成结果打“概率分数”，再用这个信号更新学生模型。具体流程通常是：学生模型先对问题生成一条推理或回答轨迹 → 对轨迹里的每个 token，计算教师模型给这个 token 的概率（或整个分布） → 用教师和学生概率之间的差（例如 KL 或 log-likelihood ratio）作为训练信号 → 用梯度更新学生，让它在自己真实会生成的轨迹上更接近教师。这样做的好处是：**训练数据来自学生当前行为分布，而不是固定数据集，因此能持续纠正学生真实会犯的错误，同时避免纯 RL 里稀疏奖励的问题****。**这篇论文的核心观点可以概括成一句话：**小模型做“推理蒸馏”时，不该死板地模仿老师，而应该把蒸馏看成一种特殊的策略优化，并对老师信号“有选择地相信”**。作者据此提出了 **REOPOLD**（Relaxed On-Policy Distillation），目标是在把大模型推理能力迁移给小模型时，减少训练不稳定、负迁移和熵坍缩。主要有三件事。第一是 **reward clipping**：不是像 PPO 那样主要裁 importance ratio，而是直接对 teacher-student 的 token reward 设一个由 mixture distribution 推出来的下界，**专门切掉那些极端负值，避免“老师一句否定把学生打废”**。第二是 **基于熵的 token 选择**：**只对高熵 token 重点学习，因为这些 token 才是真正的分叉点、推理关键点**；低熵 token 大多只是模板化延续。第三是 **两阶段训练**：前期是 exploration，用较宽松的过滤减少负反馈，先保住探索和多样性；后期再切到 refinement，把学习集中到高熵 token 上做收敛。
## Examining Reasoning LLMs-as-Judges in Non-Verifiable LLM Post-Training【LLM-as-Judge, Meta】

[https://arxiv.org/pdf/2603.12246](https://arxiv.org/pdf/2603.12246)

- **在非可验证（non-verifiable）的LLM后训练场景中，Reasoning LLMs-as-Judges相比传统非推理裁判的实际有效性尚未被系统检验**。
## V0.5 : Generalist Value Model as a Prior for Sparse RL Rollouts【LongCat】

[https://arxiv.org/pdf/2603.10848](https://arxiv.org/pdf/2603.10848)

- **RLVR中，稀疏采样（sparse rollouts）场景下的优势基线（advantage baseline）估计难题**。为系统性地解决上述 dilemma，论文提出了 V0.5 框架，通过以下机制实现自适应基线估计： 经验收缩融合（Empirical Shrinkage Fusion）：构建一个收缩估计器，以最优权重动态融合经验均值与先验预测，最小化基线估计的均方误差（MSE）。该机制包含一个正部截断函数，功能上等价于实时假设检验：当检测到先验与观测存在统计冲突（幻觉）时，自动隔离先验并回退至经验均值。 序贯OSLA预算分配（Sequential OSLA Allocation）：将基线估计重构为动态预算分配问题。基于一步前瞻（One-Step-Look-Ahead）序贯分析，系统实时量化基线不确定性，动态决定是提前终止采样以节省计算，还是追加采样以纠正先验偏差，从而在统计精度与边际成本间实现最优权衡。
## Are complicated loss functions necessary for teaching LLMs to reason?

[https://arxiv.org/pdf/2603.18756](https://arxiv.org/pdf/2603.18756)

- GRPO 虽然在LLM的数学推理任务中取得了成功，但其损失函数结合了多个复杂组件（包括组相对优势估计、PPO 风格的策略比率裁剪和 KL 正则化）。论文质疑这种复杂性是否严格必要，并试图识别哪些组件对于培养推理能力是不可或缺的，哪些可以简化或移除。论文提出了** REINFORCE with Group Relative Advantage (RGRA)，一种简化的强化学习目标函数**。RGRA **保留了组相对优势估计（group-relative advantage estimation）**以稳定训练，**但移除了 PPO 风格的裁剪和策略比率项**，从而在保持训练稳定性的同时简化实现，并在多个数学基准测试上达到或超越 GRPO 的性能。**REINFORCE + 组相对优势估计**
## Learning to Self-Evolve

[https://arxiv.org/pdf/2603.18620](https://arxiv.org/pdf/2603.18620)

- 测试时自我进化（test-time self-evolution），Learning to Self-Evolve (LSE) 框架，通过以下方式解决上述问题： 单步强化学习目标：**将多步进化问题简化为单步上下文编辑任务，以改进幅度（R¯(c1)−R¯(c0) ）作为奖励信号**，直接激励模型产生相对起始点性能提升的编辑，无论初始性能水平如何。 树引导的进化循环：**在测试时维护进化树，通过UCB（Upper Confidence Bound）算法选择节点进行扩展，允许系统在多个可能的上下文路径中探索并回溯**，避免陷入次优的贪婪路径。 通过显式优化自我进化能力，论文证明即使仅使用4B参数模型，也能在Text-to-SQL和问答任务上超越基于GPT-5和Claude Sonnet 4.5的自我进化策略，以及GEPA、TextGrad等提示优化方法。
## When and Why Does Unsupervised RL Succeed in Mathematical Reasoning? A Manifold Envelopment Perspective【Math】

[https://arxiv.org/pdf/2603.16578](https://arxiv.org/pdf/2603.16578)

- 解决**无监督强化学习（Unsupervised RL）在数学推理任务中的有效性、边界条件及内在机制**问题。提出并评估一系列基于**不确定性惩罚**（如Shannon Entropy、Rényi Entropy）和**长度惩罚**（Length Penalty）的内在奖励函数，验证"强制简洁且确定"的优化目标能否激发 latent 推理能力。核心实验发现（1）长度惩罚的主导性在Qwen3系列上，**纯长度惩罚（LP）在多数基准（MATH500、OlympiadBench、AIME等）上超越监督RL（S-RL）**，甚至优于联合优化（Ent）。这表明**强制简洁性比强制确定性更能有效激发数学推理能力**。（2）能力边界效应强模型（Qwen3）：无监督方法显著提升，LP表现最优中模型（DeepSeek-Distill）：仅微弱提升，易出现"半崩溃"弱模型（Llama-3.1）：所有无监督配置立即崩溃，验证基础逻辑先验的必要性（3）奖励设计的敏感性反向激励冗长的CP在所有模型上均导致崩溃；去除长度惩罚的AvgEnt性能显著下降，证实长度约束的关键作用。
## Offline Exploration-Aware Fine-Tuning for Long-Chain Mathematical Reasoning【Math】

[https://arxiv.org/pdf/2603.16206](https://arxiv.org/pdf/2603.16206)

- **长链数学推理中****SFT阶段缺乏探索感知能力**的问题，具体而言，是为后续的RLVR提供具备高探索潜力的模型初始化。现有研究主要集中在RLVR训练阶段通过各类策略（如熵正则化、细粒度更新控制等）促进模型探索，却**忽视了SFT作为RLVR起点对后续探索景观的决定性作用**。论文指出，SFT阶段仅仅是记忆新的思维链轨迹，而未能有意识地塑造模型的探索行为，导致初始化模型在后续RLVR中过早收敛或探索空间有限。论文提出离线探索感知（OXA）微调框架，通过以下双重目标重塑SFT阶段： **促进低置信度正确轨迹：通过最大化似然估计（MLE）训练模型内化先前未捕获的低概率推理模式，扩展推理空间的边界； 抑制高置信度错误轨迹：通过非似然损失（Unlikelihood Loss）降低模型对错误路径的过度自信，将概率质量重新分配至潜在正确的候选路径。** 最终，OXA旨在构建**兼具高初始推理准确率与高策略熵的初始化模型**，使其在后续RLVR训练中既能保持稳定优势，又能持续探索多样化的长链推理路径。
## HorizonMath: Measuring AI Progress Toward Mathematical Discovery with Automatic Verification【Math】

[https://arxiv.org/pdf/2603.15617](https://arxiv.org/pdf/2603.15617)

- **如何系统性地衡量人工智能在自主数学发现（mathematical discovery）方面的进展？Generator-Verifier Gap（生成器-验证器差距） 论文提出利用一类特殊数学问题的特性：****候选解难以生成（需要深刻数学洞察）但易于验证（可通过确定性计算高效检查）****。基于此，论文构建了HorizonMath基准测试，其特点包括： 101个 predominantly 未解决问题，横跨8个数学领域（数论、组合数学、离散几何等） 全自动验证框架，通过高精度数值比较或确定性约束检查来验证解的正确性，无需人工干预 抗污染性：****由于问题本身无已知解，训练数据不可能包含答案，任何正确解都代表真正的自主发现**** 简言之，该论文旨在建立一个可扩展、客观、自动化的平台，以衡量AI系统从"解决已知问题"向"发现新知识"跨越的进度。**
## TaoBench: Do Automated Theorem Prover LLMs Generalize Beyond MathLib?【Math】

[https://arxiv.org/pdf/2603.12744](https://arxiv.org/pdf/2603.12744)

- **当前基于LLM的自动化定理证明器（ATP）在跨定义框架（definitional framework）泛化方面存在显著不足**。**对多个SOTA ATP模型（DeepSeek-Prover-V2、Goedel-Prover-V2、Kimina-Prover等）的评估揭示了显著的泛化瓶颈**： 性能差距：模型在TAOBENCHMATHLIB上平均通过率约63-73%，而在数学等价的TAOBENCH上仅为37-50%，平均下降约26个百分点 上下文敏感性：性能差距随本地定义数量增加而急剧扩大。当上下文包含10个以上自定义定义时，TAOBENCH性能崩溃至6.37%，而TAOBENCHMATHLIB保持53.43%，差距达50个百分点 模型规模与架构无关：该差距在不同参数规模（7B至32B）和架构的模型中普遍存在，表明这是当前ATP训练范式的系统性局限。
## STRIDE: Learnable Stepwise Language Feedback for LLM Reasoning【Tongyi】

[https://arxiv.org/pdf/2605.18851](https://arxiv.org/pdf/2605.18851)

- 论文提出 STRIDE（Stepwise Language Feedback for LLM Reasoning），通过以下范式转移解决上述问题： **从标量到语言：将过程监督从单维标量奖励转变为可学习的步骤级语言反馈（learnable stepwise language feedback）**； 共同训练机制：仅使用结果奖励（outcome-based rewards）共同训练生成器与生成式验证器，无需昂贵的步骤级人工标注； 轨迹重定向（Trajectory Redirection）：利用验证器的语言批评定位首次失败点（First Point of Failure, FPF），通过多锚点重定向策略（Multi-Point Redirection）从中断处重构推理路径，将失败轨迹转化为有效训练信号。 该方法特别针对零通过率问题（zero-pass-rate problems）——即传统标量方法完全无法提供学习信号的困难样本，通过高带宽语言反馈实现推理能力的突破。**不要再把 verifier 的过程反馈压成一个 scalar reward，而是****让 verifier 直接生成“逐步语言批注”，再把这些批注作为上下文，引导 generator 从出错前的位置重新推理****。**

# Reward Model

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Model
- 方法：agent, language, vision-language-model, reasoning, vision, optimization
- 论文/报告：10 篇
- ⭐How Far Can Unsupervised RLVR Scale LLM Training?
- ⭐CLIPO: Contrastive Learning in Policy Optimization Generalizes RLVR【Qwen】
- Visual-ERM: Reward Modeling for Visual Equivalence【AI Lab】
- RewardFlow: Topology-Aware Reward Propagation on State Graphs for Agentic RL with Large Language Models
- CausalRM: Causal-Theoretic Reward Modeling for RLHF from Observational User Feedbacks【Zhouchen Lin】
- Reasoning over mathematical objects: on-policy reward modeling and test time aggregation【Math】
- ⭐MemReward: Graph-Based Experience Memory for LLM Reward Prediction with Limited Labels【Jiaxuan You】
- ⭐⭐On the Direction of RLVR Updates for LLM Reasoning: Identification and Exploitation【RLVR, Qwen】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## ⭐How Far Can Unsupervised RLVR Scale LLM Training?

[https://arxiv.org/pdf/2603.08660](https://arxiv.org/pdf/2603.08660)

- 近期研究提出利用模型内在信号（如置信度、一致性）作为奖励信号（Intrinsic Rewards）,论文通过建立**统一的理论框架**揭示：所有内在奖励方法本质上都是通过"锐化"（sharpening）机制放大模型的初始分布——即强化模型已知的偏好，而非发现新知识。既然内在奖励受限于模型已有知识，论文探索外部奖励方法（External Rewards）作为突破方向： 利用未标注数据：通过下一词预测、文本重构等自监督任务生成奖励信号。 利用生成-验证不对称性（Gen-Verify Asymmetries）：在数学积分、代码执行、形式化证明等领域，验证远比生成容易，利用这种计算不对称性构建可靠的外部验证器。
## ⭐CLIPO: Contrastive Learning in Policy Optimization Generalizes RLVR【Qwen】

[https://arxiv.org/pdf/2603.10101](https://arxiv.org/pdf/2603.10101)

- **RLVR在提升大语言模型推理能力时面临的泛化性与鲁棒性不足。论文提出CLIPO（Contrastive Learning in Policy Optimization），通过对比学习机制在策略优化中引入跨轨迹的结构化正则化：**** 将成功的推理轨迹视为正样本，失败的视为负样本**** 在嵌入空间中最大化成功轨迹间的相似性（捕捉"成功路径的交集"即不变逻辑结构） 最小化成功与失败轨迹间的相似性（抑制非系统性错误和幻觉） 将对比损失作为稠密辅助奖励，补充稀疏的二元结果奖励，引导模型学习更鲁棒的推理策略。**
## Visual-ERM: Reward Modeling for Visual Equivalence【AI Lab】

[https://arxiv.org/pdf/2603.13224](https://arxiv.org/pdf/2603.13224)

- 针对 **vision-to-code** 任务（将图表、表格、SVG 等结构化视觉输入转换为可执行代码或结构化表示）中的 **奖励建模缺陷** 问题，提出了一种新的评估范式。论文提出了 Visual Equivalence Reward Model (Visual-ERM)，一个多模态生成式奖励模型，**直接在渲染后的视觉空间中评估 vision-to-code 质量**。该模型具备三个关键特性：** 细粒度感知：捕获超越粗粒度语义相似性的微妙视觉差异**； **可解释性：生成诊断性反馈（错误类别、位置、严重程度）**，支持测试时扩展（test-time scaling）； 任务无关性：**单一模型即可泛化于图表转代码、表格转 Markdown、SVG 转代码等多种任务**。 通过将 Visual-ERM 集成到 RL 流程中，论文实现了对视觉等价性的忠实监督，显著提升了 vision-to-code 任务的解析精度与鲁棒性。
## RewardFlow: Topology-Aware Reward Propagation on State Graphs for Agentic RL with Large Language Models

[https://arxiv.org/pdf/2603.18859](https://arxiv.org/pdf/2603.18859)

- **如何在不训练奖励模型的情况下，客观地估计Agentic任务中中间状态的过程奖励？**作者提出了RewardFlow方法，其关键思想是： **利用状态图的拓扑结构：通过构建状态图（State Graph）来捕获推理轨迹中状态间的内在拓扑关系** 拓扑感知的奖励传播：**将终端成功状态的奖励通过图结构反向传播到中间状态，基于状态与成功状态的距离（如最短路径距离）来量化其成功潜力** 无需外部奖励模型：**完全基于采样轨迹构建的图结构进行客观的奖励估计**，避免了昂贵的奖励模型训练 该方法通过分析状态的可到达性（Reachability）和中心性（Centrality）等拓扑属性，为每个中间状态分配细粒度的过程奖励，从而提供更有效的训练信号。
## CausalRM: Causal-Theoretic Reward Modeling for RLHF from Observational User Feedbacks【Zhouchen Lin】

[https://arxiv.org/pdf/2603.18736](https://arxiv.org/pdf/2603.18736)

- 论文提出了 CausalRM（Causal-Theoretic Reward Modeling）框架： 针对噪声：引入**噪声感知的替代损失函数（noise-aware surrogate loss），通过显式建模标注错误生成过程（假阳性率和假阴性率），使修正后的损失在期望上等价于无噪声条件下的原始损失**。 针对偏倚：采用倾向得分重加权（propensity score reweighting）策略，**利用用户反馈概率（propensity score）对训练样本进行逆概率加权**，消除用户偏好偏倚，使模型能够从偏倚的观测数据中恢复出对全量数据分布的无偏估计。
## Reasoning over mathematical objects: on-policy reward modeling and test time aggregation【Math】

[https://arxiv.org/pdf/2603.18886](https://arxiv.org/pdf/2603.18886)

- 解决**语言模型（LMs）在复杂数学对象推理方面的能力不足**，以及**现有评估和训练方法对此类能力衡量的缺失**。
## ⭐MemReward: Graph-Based Experience Memory for LLM Reward Prediction with Limited Labels【Jiaxuan You】

[https://arxiv.org/pdf/2603.19310](https://arxiv.org/pdf/2603.19310)

- **RLVR中奖励标签稀缺**的问题。论文提出MemReward框架，通过构建基于图神经网络（GNN）的经验记忆系统，实现：标签传播：将有限标注（20%）的奖励信号通过异构图（查询-思考-答案）传播至未标注样本。跨域泛化：**利用跨领域异构图结构，在数学、问答、代码生成等多领域间共享奖励模式**。性能逼近：仅用20%标签即可达到Oracle模型97.3%（3B模型）和96.6%（1.5B模型）的性能，并在领域外任务上超越全监督基线。
## ⭐⭐On the Direction of RLVR Updates for LLM Reasoning: Identification and Exploitation【RLVR, Qwen】

[https://arxiv.org/pdf/2603.22117](https://arxiv.org/pdf/2603.22117)

- **RLVR在提升LLM推理能力过程中，现有分析方法过度关注更新幅度而忽略更新方向**的问题。论文提出使用**带符号的token级对数概率差**作为核心分析工具，**该指标捕捉从基础模型到RLVR模型的概率质量转移方向（正值表示RLVR增强的token，负值表示抑制的token）**。统计实验表明，**Δlogp呈现清晰的双峰分布，能有效区分两类模型的生成，而传统的熵和KL散度分布几乎完全重叠**。精准定位推理关键token：**通过选择性token替换实验，论文发现仅需替换约**10%**的token（按Δlogp负值最大的位置选择）即可从基础模型恢复RLVR的推理性能**，显著优于基于熵或KL散度的选择（需替换更多token）。梯度稀疏性机制：理论分析与实证表明，**RLVR的梯度更新天然集中于低概率token（梯度范数与1−πθ(yt)成正比）**。**这些低概率token正是Δlogp值最大的位置，构成了RLVR学习的核心**。训练时过滤低概率token（如使用top-p采样）会导致推理性能显著下降。
## ⭐⭐Sparse but Critical: A Token-Level Analysis of Distributional Shifts in RLVR Fine-Tuning of LLMs【RLVR, Qwen】

[https://arxiv.org/pdf/2603.22446](https://arxiv.org/pdf/2603.22446)

- **RLVR微调如何改变LLM的token-level预测分布，以及识别这些分布变化中哪些是真正驱动推理性能提升的关键机制。**论文旨在超越传统的响应级（response-level）评估指标（如准确率、奖励值），提供一个**细粒度的、基于分布变化**的理论视角，阐明RLVR作为一种"稀疏且靶向的轨迹修正机制"而非"全局策略重写"的本质。核心指标：Jensen-Shannon (JS) 散度 论文采用JS散度作为衡量基础模型 πbase 与RL微调模型 πRL 之间分布差异的核心指标。**RLVR主要在现有候选集内重新排序，通过调整已合理词元的概率质量来优化推理路径。**
## ImplicitRM: Unbiased Reward Modeling from Implicit Preference Data for LLM alignment

[https://arxiv.org/pdf/2603.23184](https://arxiv.org/pdf/2603.23184)

- 传统基于人类反馈的强化学习（RLHF）依赖昂贵的显式偏好标注（如成对比较）。相比之下，隐式反馈（如用户是否复制回答）具有成本低廉、规模庞大的优势，但面临两个根本性挑战：缺乏确定负样本：无反馈样本（ri=0）可能包含实际满意的用户（假阴性），不可简单视为负样本；**用户偏好偏差：不同响应引发用户反馈的倾向（propensity）存在异质性（如知识问答易复制，开放对话难复制）**，导致传统正样本-未标注（PU）学习方法失效。论文提出ImplicitRM框架，通过**将训练样本分层为四个潜在群组（正-主动、负-主动、正-被动、负-被动），并基于似然最大化推导出理论上无偏的学习目标**，从而同时解决缺乏确定负样本和用户偏好偏差的问题。

# Foundation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Foundation
- 方法：language, vision-language-model, vision
- 论文/报告：11 篇
- Memory Caching: RNNs with Growing Memory【Google】
- Unified Vision-Language Modeling via Concept Space Alignment
- Self-Play Only Evolves When Self-Synthetic Pipeline Ensures Learnable Information Gain
- Rethinking Representativeness and Diversity in Dynamic Data Selection
- Aligning Language Models from User Interactions
- Fine-tuning MLLMs Without Forgetting Is Easier Than You Think
- ⭐Revisiting Model Stitching In the Foundation Model Era【Amazon】
- 📌Mamba-3: Improved Sequence Modeling using State Space Principles
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Memory Caching: RNNs with Growing Memory【Google】

[https://arxiv.org/pdf/2602.24281](https://arxiv.org/pdf/2602.24281)

- 解决**RNNs固定大小内存与长序列建模需求之间的矛盾，论文引入Memory Caching (MC)技术，通过****在序列分段处缓存记忆状态的检查点（checkpoints）****，使RNN的有效内存容量能够随序列长度增长。****分段、每段内部用 RNN（或线性注意力）递归处理 、 保存“段结束时的隐藏状态” 、之后可以读取这些隐藏状态。**
## Unified Vision-Language Modeling via Concept Space Alignment

[https://arxiv.org/pdf/2603.01096](https://arxiv.org/pdf/2603.01096)

- 现有语言无关的嵌入空间（如 Sonar）虽支持 1500 种语言的文本与 177 种语言的语音，但缺乏对视觉模态的表征能力，限制了其在视觉-语言任务中的应用。核心挑战在于：如何在不重新联合训练整个系统的前提下，将现有的高性能视觉编码器（Perception Encoder）**后验对齐**到一个纯粹基于文本训练的语义空间中，并实现跨模态的生成与推理。
## Self-Play Only Evolves When Self-Synthetic Pipeline Ensures Learnable Information Gain

[https://arxiv.org/pdf/2603.02218](https://arxiv.org/pdf/2603.02218)

- 当前许多所谓的"自演化"系统本质上是在固定规则下的自对弈（例如，在数学或编程等可验证领域中，模型在 PROPOSER 与 SOLVER 角色之间博弈，或在偏好学习领域中共演 SOLVER 与 VERIFIER）。这些系统往往表现出： 快速达到性能平台期或出现性能衰退（early peak and subsequent decline），任务分布坍缩至平凡问题，奖励黑客（reward hacking）与自幻觉（self-delusion）现象。**根本失败模式：可学习信息增益的缺失** 论文指出，上述脆弱性的根源在于**自合成数据管道（self-synthesised data pipeline）未能确保跨迭代的可学习信息单调增长**。现有系统虽然能合成更多数据，但这些数据对于下一迭代的观察者（bounded observer）而言并未提供新的、可学习的结构，导致循环陷入"无信息增量的自我重复"。系统性解决框架 为突破此瓶颈，论文提出将自演化重新框架化为确保可学习信息持续增长的动态自合成管道，并据此提出三项必要的设计原则： 非对称协同演化（Asymmetric Co-evolution）：通过显式维护 PROPOSER/VERIFIER 与 SOLVER 之间的"弱到强"（weak-to-strong）与"强到弱"（strong-to-weak）同步机制，利用计算不对称性（验证易、求解难）持续生成具有学习潜力的数据。 容量增长（Capacity Growth）：随迭代动态扩展模型的参数预算 C(t) 与推理时预算 T(t) ，**确保观察者的假设空间足以容纳新暴露的可学习结构**，避免固定容量导致的饱和。 主动信息寻求（Proactive Information Seeking）：突破零数据或固定数据集的封闭循环，**使内部环境（PROPOSER+VERIFIER）主动从外部环境检索、筛选并转化为与当前能力边界对齐的新上下文**，引入新的合成方向与信息熵。
## Rethinking Representativeness and Diversity in Dynamic Data Selection

[https://arxiv.org/pdf/2603.04981](https://arxiv.org/pdf/2603.04981)

- 动态数据选择（Dynamic Data Selection）中两个核心概念——**代表性（Representativeness）与多样性（Diversity）**的传统定义及其引发的优化瓶颈问题。论文提出重新概念化： 代表性：定义为对数据集级高频特征因子的加权覆盖，通过稀疏自编码器（SAE）的稀疏单元激活统计实现； 多样性：定义为过程级约束，通过引入**使用频率惩罚（Usage-Frequency Penalty）**强制样本轮换，确保训练轨迹逐步覆盖互补的稀有因子，并提供反垄断的理论保证； 平衡机制：设计轻量级课程调度器（curriculum scheduler），无需额外梯度或二阶计算即可实现从核心模式巩固到稀有因子探索的平滑过渡。 通过上述改进，论文旨在实现超过2倍的训练加速，同时达到或超越全数据训练的精度。
## Aligning Language Models from User Interactions

[https://arxiv.org/pdf/2603.12273](https://arxiv.org/pdf/2603.12273)

- 作者发现一个有趣的现象：**当用户给出后续反馈（如纠正、补充要求）时，模型往往能在对话上下文中意识到自己之前的错误并进行修正。** 这意味着模型自身具备“事后聪明”（Hindsight）的能力。论文提出了一种原则性且可扩展的方法，直接从原始交互中学习，无需人类显式反馈： 捕捉事后表现（Hindsight Distribution）： 模型先观察用户的后续消息。在有了这些“额外信息”的加持下，模型对初始问题的潜在回复质量会提高。 自我蒸馏（Self-Distillation）： 将这种“有了后续反馈后”的行为分布（目标分布）蒸馏回模型当前的原始策略（Policy）中。 目标函数： **通过比较模型在“看到反馈前”和“看到反馈后”的 Token 分布差异，计算损失函数并更新参数，使模型在未来即使没有后续反馈也能直接给出更符合用户预期的回答**。
## Fine-tuning MLLMs Without Forgetting Is Easier Than You Think

[https://arxiv.org/pdf/2603.14493](https://arxiv.org/pdf/2603.14493)

- **MLLM在微调过程中出现的灾难性遗忘(catastrophic forgetting)问题。**研究通过构建2×2 评估矩阵（交叉验证文本和图像的分布内/分布外组合），揭示了以下关键发现： **简单正则化即可防止大多数遗忘：采用低学习率(10−6 )或参数高效微调(LoRA)可在提升目标任务性能的同时，保持分布外(OOD)视觉理解能力（性能下降≤3 个百分点）**，无需复杂的防遗忘机制。 识别特定失效模式：当输入为**分布内图像(ID Image)但搭配分布外文本(OOD Text)**时（例如**对熟悉图像提出新问题**），模型会出现"任务特定过拟合"——即过度拟合训练中的语言模板而忽略指令，而非真正的知识丢失。 数据混合的简单补救：通过将目标任务数据与多样化指令数据（如LLaVA-665K）按50% 比例混合，可有效解决上述过拟合问题，在保持ID任务性能的同时恢复OOD文本理解能力。 持续学习的简化范式：在MLLM-CL基准（涵盖遥感、医学、自动驾驶、科学、金融五领域）上，简单的增量LoRA(IncLoRA)或顺序低学习率微调(SeqFull)在无回放设置下优于所有复杂对比方法（如MoELoRA、O-LoRA等）。
## ⭐Revisiting Model Stitching In the Foundation Model Era【Amazon】

[https://arxiv.org/pdf/2603.12433](https://arxiv.org/pdf/2603.12433)

- **关键问题：这些在训练目标（对比学习 vs. 自监督重建）、数据分布（纯视觉 vs. 视觉-语言对）和模态组合上差异显著的模型，其内部表征是否兼容？能否通过轻量拼接层实现知识融合？**异构VFMs可拼接：**使用FFM+TLT策略，DINOv2与SigLIP 2等异构模型可在各层成功拼接，且深层拼接时性能可超越任一 constituent 模型的线性探测基线**。 知识融合验证：通过Self-Stitch对照实验（将拼接层插入单一模型作为基线），证明跨模型拼接的性能提升（+2.3%至+2.6%）源于异构模型间的互补知识融合，而非单纯增加网络容量。 一致性泛化：该结论在fMoW、iNaturalist、FGVC-Aircraft等分类任务及ADE20K语义分割任务上均成立；在DINOv2、DINOv3、SigLIP 2、CLIP等多组VFM组合中均得到验证（除弱源模型如CLIP→强目标模型的特殊情况外）。
## 📌Mamba-3: Improved Sequence Modeling using State Space Principles

[https://arxiv.org/pdf/2603.15569](https://arxiv.org/pdf/2603.15569)

- 基于**推理优先（inference-first）**的设计范式，论文通过以下方法论改进构建Mamba-3： 指数梯形离散化：提供二阶精度的离散化方案，在递推中引入隐式卷积，增强对输入历史的建模能力 复数值状态空间：通过数据相关的旋转位置编码（RoPE）实现复数状态转移，恢复状态跟踪能力（如TC0复杂度的形式语言任务） 多输入多输出（MIMO）架构：将外积状态更新改为矩阵乘法，在保持解码延迟不变的情况下提升算术强度，改善硬件利用率 这些改进使得Mamba-3在1.5B参数规模下，相比Mamba-2平均下游任务准确率提升1.9个百分点，相比Gated DeltaNet提升1.8个百分点，同时在状态大小减半的情况下达到相当的困惑度，推进了性能-延迟帕累托前沿。
## Efficient Universal Perception Encoder【Meta】

[https://arxiv.org/pdf/2603.22387](https://arxiv.org/pdf/2603.22387)

- 论文提出三阶段蒸馏流程，关键洞察是**高效模型容量有限，直接学习异构多教师会导致知识冲突，因此需要中间"代理"统一知识表示**：** ****Stage 1（放大）：将多个领域专家（PEcore-G、PElang-G、DINOv3-H+）的知识蒸馏到一个大型代理模型（Proxy Teacher，1.9B参数）**。代理模型具备足够容量整合多样化的特征表示，并对教师输出进行特征归一化以稳定训练。** Stage 2（缩小-固定分辨率）：将代理模型的通用知识转移至目标高效编码器（如ViT-B/S/T，<100M参数）**。采用 256×256 固定分辨率进行长时间训练（390k迭代），利用计算效率优势充分学习统一表征。 **Stage 3（缩小-多分辨率）：基于Stage 2的 checkpoint，在图像金字塔（256/384/512）上进行短周期多分辨率微调（100k迭代），使模型适应下游任务的多样化输入尺度**。 蒸馏过程通过Adapter Heads（2层MLP）对齐师生特征维度，采用余弦相似度与平滑L1损失的组合进行监督。
## Synthetic Mixed Training: Scaling Parametric Knowledge Acquisition Beyond RAG【James Zou】

[https://arxiv.org/pdf/2603.23562](https://arxiv.org/pdf/2603.23562)

- **在数据受限领域，如何通过合成数据增强方法有效地将新知识内化到语言模型参数中，并突破检索增强生成（RAG）设定的性能上限**的问题。论文提出了两种关键技术：** Synthetic Mixed Training（合成混合训练）：通过以1:1比例混合合成QA和合成文档，利用二者互补的训练信号（QA教授"如何使用知识"的行为知识，文档教授领域特定的事实知识）**，实现随数据量和生成器强度对数线性扩展的性能曲线。 **Focal Rewriting（焦点重写）：通过在文档生成时显式 conditioning 于特定问题（在提示中添加"Focus on the question {{query}}"），显著提升合成文档的词汇和语义多样性**，从而获得更陡峭的扩展曲线。 最终，该方法在QuaLITY等长文档阅读理解基准上，使8B参数模型相对RAG获得4.4%的性能提升，并在六个跨模型/基准设置中的五个超越了RAG。
## MuRF: Unlocking the Multi-Scale Potential of Vision Foundation Models

[https://arxiv.org/pdf/2603.25744](https://arxiv.org/pdf/2603.25744)

- 论文提出**Multi-Resolution Fusion (MuRF)** 策略，**通过在推理时构建输入图像金字塔、提取多尺度特征并进行融合，在不修改或重新训练VFM主干网络的前提下，生成同时包含全局语义信息与局部细节的统一表示**，从而解锁预训练VFMs在多类下游任务（密集预测、多模态理解、无监督异常检测等）中的潜力。**关键设计**：**选择通道拼接而非逐元素相加，以避免破坏不同尺度的正交信号（如宏观语义与微观边缘），使下游任务头能够自适应地选择所需信息。**

# Data

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data
- 方法：agent, generation, language, vision-language-model, reasoning, vision, reinforcement-learning, deep-learning
- 论文/报告：15 篇
- ⭐FT-Dojo: Towards Autonomous LLM Fine-Tuning with Language Agents**【MSRA】**
- Learning from Synthetic Data Improves Multi-hop Reasoning
- TopoCurate: Modeling Interaction Topology for Tool-Use Agent Training【LongCat】
- In-Context Reinforcement Learning for Tool Use in Large Language Models
- Data Agent: Learning to Select Data via End-to-End Dynamic Optimization
- Scale Dependent Data Duplication
- Capacity-Aware Mixture Law Enables Efficient LLM Data Optimization
- Does the Question Really Matter? Training-Free Data Selection for Vision-Language SFT
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## ⭐FT-Dojo: Towards Autonomous LLM Fine-Tuning with Language Agents**【MSRA】**

[https://arxiv.org/pdf/2603.01712](https://arxiv.org/pdf/2603.01712)

- 针对垂直领域（如化学、法律、金融等）的LLM微调仍是一个劳动密集且成本高昂的过程，需要领域专家与机器学习工程师协作完成数据策展、训练配置和迭代诊断。论文引入了**FT-Dojo**（首个用于评估LLM端到端微调的交互式环境）和**FT-Agent**（一种专门的智能体框架），旨在验证：在仅需任务描述和评估反馈的情况下，智能体能否自主完成数据策展、训练流程构建和策略迭代优化，从而降低领域适配的人工成本与技术门槛。
## Learning from Synthetic Data Improves Multi-hop Reasoning

[https://arxiv.org/pdf/2603.02091](https://arxiv.org/pdf/2603.02091)

- **利用规则生成的合成数据（rule-generated synthetic data）进行RL微调**。这类数据通过模板、上下文无关文法和逻辑程序在完全虚构的实体上生成，具有语义简单、可完全验证、可任意规模免费生成等特点。**规则生成的合成数据（即使完全虚构、语义简单）能通过RL教会LLM可迁移的知识组合技能**。
## TopoCurate: Modeling Interaction Topology for Tool-Use Agent Training【LongCat】

[https://arxiv.org/pdf/2603.01714](https://arxiv.org/pdf/2603.01714)

- **TopoCurate**，一种基于交互拓扑建模的数据筛选框架，用于解决工具使用智能体训练中的**结果等价幻觉（Outcome Equivalence Illusion）**问题。TopoCurate 将数据筛选范式从二元结果过滤转向结构感知的拓扑建模，通过显式捕获交互动力学中的因果依赖，为构建具有韧性、效率与多样性的智能体提供了可扩展的数据中心优化框架。TopoCurate 把同一任务的多次 rollouts投影到一个统一结构里： 把一次交互 turn 定义为 (reasoning, action, observation) 的组合 定义“语义等价”关系：如果两个 turn 的 action 与 observation 语义相近，就把它们视为同一“状态”（聚合成一个节点） 这样，多条线性轨迹会被“压缩/合并”为一个 DAG/流形式的结构空间，显式呈现： 哪些节点是关键分岔点（做对/做错结果差很大） 失败模式在哪里出现、如何恢复（error recovery） 哪些路径是冗余绕圈、哪些是高效捷径。
## In-Context Reinforcement Learning for Tool Use in Large Language Models

[https://arxiv.org/pdf/2603.08068](https://arxiv.org/pdf/2603.08068)

- 无需SFT即可训练LLMs使用外部工具的新框架。ICRL通过在上下文中进行RL解决上述问题，其核心机制包括： 动态少样本提示与课程学习 在RL训练的rollout阶段，将少量工具使用示例（demonstrations）嵌入prompt，为模型提供软监督信号，引导其生成结构化推理（<think>）、工具调用（<search>）和最终答案（<answer>）。随着训练推进，通过渐进式课程逐步减少示例数量（如3-shot → 2-shot → 0-shot），最终使模型在零样本设置下自主调用工具。 工具感知的RL优化 采用GRPO（Group Relative Policy Optimization）进行策略优化，通过组内相对优势估计稳定训练； 实施Loss Masking：仅对模型生成的token计算梯度，掩码工具返回的不可训练内容（如搜索结果），确保学习聚焦于模型自主行为.
## Data Agent: Learning to Select Data via End-to-End Dynamic Optimization

[https://arxiv.org/pdf/2603.07433](https://arxiv.org/pdf/2603.07433)

- 能否设计一个能够在训练过程中自适应地实时选择数据，同时以即插即用（plug-and-play）方式跨任务扩展的智能体？ 为解决上述问题，论文提出了 Data Agent，一个端到端的动态数据选择框架，将数据选择形式化为与模型优化协同演化的训练感知序列决策问题，通过强化学习学习样本级的选择策略，从而在不牺牲性能的情况下显著加速训练。
## Scale Dependent Data Duplication

[https://arxiv.org/pdf/2603.06603](https://arxiv.org/pdf/2603.06603)

- 这篇论文探讨了大规模语言模型预训练中的一个关键但未被充分研究的尺度依赖性问题：**语义重复（semantic duplicates）对模型训练的影响随模型能力和数据规模变化而变化**。论文发现：随着模型能力增强，**语义等效但表面形式不同的文档**（如翻译、改写）会产生越来越相似的训练信号，其行为趋近于精确重复。同时，大规模语料库中的语义碰撞（semantic collisions）发生率远超基于小样本的幂律预测。
## Capacity-Aware Mixture Law Enables Efficient LLM Data Optimization

[https://arxiv.org/pdf/2603.08022](https://arxiv.org/pdf/2603.08022)

- 针对大语言模型（LLM）数据混合（data mixture）优化中的计算效率与外推准确性问题，提出了**容量感知混合定律（CAMEL, Capacity-Aware Mixture Law）**框架，实现了在显著降低计算成本的同时提升下游任务性能。论文提出了一个名为 CAMEL (Capacity-Aware Mixture Law) 的新框架，即“容量感知混合定律”。 容量感知（Capacity-Aware）： 传统的定律往往忽略了模型规模（参数量）与数据混合比例之间的非线性相互作用。CAMEL 能够建模不同规模的模型对特定数据配比的“消化能力”。 端到端预测（Loss-to-Benchmark）： 它不仅预测验证集损失（Validation Loss），还引入了一个预测定律，直接估算模型在特定下游评测任务（Benchmarks）上的准确率。 计算效率高： 研究如何在一个固定的计算预算内，通过分配给不同规模的小模型来拟合这个定律，从而以最小代价预测出最适合大模型的配比。
## Does the Question Really Matter? Training-Free Data Selection for Vision-Language SFT

[https://arxiv.org/pdf/2603.09715](https://arxiv.org/pdf/2603.09715)

- 论文提出Conditional Verdict Shift (CVS)，基于以下核心洞见构建无需训练的数据选择框架： 对于高质量的多模态样本，引入问题应显著改变模型在给定图像条件下对答案有效性的评估。CVS通过测量冻结的VLLM在两种条件下的判断差异——P(YES∣I,Q,A) vs P(YES∣I,A) ——来识别那些： **语义一致**（问题增强答案可信度） **真正依赖视觉-语言联合推理**（无问题时模型判断不确定，有问题时判断明确） 位于决策边界附近（提供强梯度信号） 该方法无需额外训练，直接利用现有VLLM的零样本判断能力，在Vision-Flan和The Cauldron数据集上仅用10%-15%的数据即超越全量训练性能，同时相比COINCIDE和XMAS分别减少17.3%和44.4%的计算成本。
## Dynamics-Predictive Sampling for Active RL Finetuning of Large Reasoning Models

[https://arxiv.org/pdf/2603.10887](https://arxiv.org/pdf/2603.10887)

- **RL微调LLM推理能力时的数据选择效率问题，**如何在**保持在线数据选择适应性**（即根据模型当前能力动态调整训练样本）的同时，**消除冗余的rollout计算开销**，实现计算高效的主动学习？论文提出**Dynamics-Predictive Sampling (DPS)**框架，通过以下机制解决上述问题： 动态系统建模：将每个提示的解决进度形式化为一个动态系统，其中解决程度作为状态，状态转移由隐藏马尔可夫模型（HMM）表征 在线贝叶斯推断：利用历史rollout奖励信号，通过轻量级的在线贝叶斯推断估计演化的状态分布 预测性选择：基于推断结果预测提示处于"部分解决"状态（最具信息量的状态）的概率，直接选择Top-B提示进行训练，无需预先进行大规模的LLM rollout验证。
## ⭐AI can Autonomously Evolve Pretraining Data Curation【Pengfei Liu】

[https://arxiv.org/pdf/2603.14420](https://arxiv.org/pdf/2603.14420)

- “数据达尔文主义（Data Darwinism）”系列的第二部分。其核心目标是解决大模型预训练中一个极具挑战性的问题：**如何让 AI 自动进化并设计出最优的数据清洗和增强策略，而不再依赖高昂的人工干预。**论文提出了 DataEvolve，这是一个模仿生物进化过程的自动化系统，专门用于进化数据处理策略。 其工作流程主要包含以下三个核心环节： 经验池与策略池 (Experience & Strategy Pools)： 系统记录以往处理数据时发现的问题（如噪声模式、格式错误）以及对应的处理策略及其表现。 多代理协同进化 (Multi-agent Evolution)： * 分析代理 (Analyst)： 负责采样数据并识别当前的质量缺陷。 改进代理 (Refiner)： 根据发现的问题，利用 LLM（如 GPT-4 等强模型）**生成或改写数据处理代码/规则**。 验证代理 (Evaluator)： 在小规模模型上测试新策略的效果，提供反馈。 迭代进化： 每个类别的数据都会经历约 30 次迭代，策略根据评估结果不断优选、变异和杂交，最终锁定该类别的“最优生存策略”。
## HopChain: Multi-Hop Data Synthesis for Generalizable Vision-Language Reasoning【Qwen】

[https://arxiv.org/pdf/2603.17024](https://arxiv.org/pdf/2603.17024)

- 论文提出通过合成多跳视觉语言推理数据（multi-hop vision-language reasoning data）来解决上述问题： 逻辑依赖链：构造查询使得后续推理步骤（hops）依赖于前面步骤建立的实例、集合或条件，**形成强制性的证据链（如：实例A → 实例B → 实例C）**。 感知级跳跃（Perception-level hops）：**在单对象感知（Level 1）与多对象关系推理（Level 2）之间强制切换**，要求模型反复重新 grounding 到视觉证据。 实例链跳跃（Instance-chain hops）：通过显式的实例依赖链（如"位于A右侧的B"→"位于B前方的C"），阻止语言捷径，**确保每一步都必须基于前一步的视觉结果进行定位**。 通过将这种多跳数据加入RLVR训练，模型被迫在推理链的每一步都进行新鲜的视觉重新定位（fresh visual re-grounding），从而增强长CoT推理的鲁棒性，并减少跨步骤的错误累积。
## Efficient Exploration at Scale【Google DeepMind】

[https://arxiv.org/pdf/2603.17378](https://arxiv.org/pdf/2603.17378)

- **RLHF的数据效率问题**，即如何在减少人类标注数据量的同时，显著提升语言模型的对齐性能。论文提出了一种在线学习算法，通过以下关键创新实现数据效率的显著提升： 肯定性微调（Affirmative Nudge）：**在强化信号中添加小的正向偏移，防止策略崩溃，稳定在线学习过程**。 认识神经网络（Epistemic Neural Network）：显式建模奖励模型的不确定性，而非仅提供点估计。 信息导向探索（Information-Directed Exploration）：**基于不确定性主动选择最具信息量的响应对比进行查询，最大化每次人类反馈的信息增益**。 实验结果表明，该方法在使用Gemma大语言模型时，仅需不到20K的人类选择标签即可匹配离线RLHF使用200K标签的性能，实现超过10倍的数据效率提升；外推至百万标签规模时，预计可实现1000倍的效率增益。
## The Finetuner's Fallacy: When to Pretrain with Your Finetuning Data

[https://arxiv.org/pdf/2603.16177](https://arxiv.org/pdf/2603.16177)

- 试图解决**领域适应（domain adaptation）中传统训练流程的效率与性能问题**，特别是挑战了"仅通过微调（finetuning）实现领域专业化是最优路径"这一普遍直觉（即"微调者的谬误"）。论文质疑"将领域数据全部保留到训练最后阶段（微调）"的做法是否最优。**当目标领域与通用网络文本分布差异较大（如化学文献、符号音乐、数学证明）时，仅在微调阶段引入领域数据需要模型进行"大幅修正"**，导致：需要更大的模型规模才能达到同等领域性能推断阶段计算成本更高（需部署更大模型）。论文主张**在预训练阶段就将领域数据作为小比例混合（通常1-5%，重复10-50次）引入，形成"SPT→微调"的新范式**。这解决了：正则化效应：领域数据在预训练中被通用数据"稀释"，可容忍更多重复而不易过拟合减少遗忘：预训练已建立领域基础表征，微调只需轻微调整，保留通用能力参数效率：1B参数的SPT模型可超越3B参数的标准预训练模型（在远离网络文本的领域）简言之，论文试图证明：**稀缺的专业领域数据应尽早（预训练阶段）而非仅在最晚（微调阶段）引入训练流程**，从而在领域性能、通用能力保持和计算效率之间取得更优的帕累托前沿。
## ManiTwin: Scaling Data-Generation-Ready Digital Object Dataset to 100K

[https://arxiv.org/pdf/2603.16866](https://arxiv.org/pdf/2603.16866)

- **仿真机器人操纵学习中缺乏大规模、数据生成就绪（data-generation-ready）的数字资产。**论文提出了**ManiTwin**——一个自动化的流水线，能够**将单张输入图像转换为具备物理属性、语义注释和验证抓取姿态的仿真就绪3D资产**，并构建了包含**10万个此类资产**的**ManiTwin-100K**数据集，为大规模机器人操纵数据生成提供基础。
## A Deep Dive into Scaling RL for Code Generation with Synthetic Data and Curricula【Code, Meta】

[https://arxiv.org/pdf/2603.24202](https://arxiv.org/pdf/2603.24202)

- 针对**大规模强化学习（RL）训练代码生成模型时的数据瓶颈问题**，提出了一套多轮合成数据生成框架，并系统研究了课程设计、任务难度与环境多样性对RL训练动态的影响。

# Generation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Generation
- 方法：agent, generation, language, reasoning, optimization, gui-agent
- 论文/报告：18 篇
- Helios: Real Real-Time Long Video Generation Model【ByteDance】
- InfinityStory: Unlimited Video Generation with World Consistency and Character-Aware Shot Transitions【Adobe】
- RealWonder: Real-Time Physical Action-Conditioned Video Generation【Jiajun Wu】
- InSpatio-WorldFM: An Open-Source Real-Time Generative Frame Model
- EVATok: Adaptive Length Video Tokenization for Efficient Visual Autoregressive Generation
- EvoTok: A Unified Image Tokenizer via Residual Latent Evolution for Visual Understanding and Generation
- Fair Benchmarking of Emerging One-Step Generative Models Against Multistep Diffusion and Flow Models
- Out of Sight, Out of Mind? Evaluating State Evolution in Video World Models
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Helios: Real Real-Time Long Video Generation Model【ByteDance】

[https://arxiv.org/pdf/2603.04379](https://arxiv.org/pdf/2603.04379)

- 首个在单张H100 GPU上达到**19.5 FPS**的14B实时长视频生成模型。Deep Compression Flow：通过Multi-Term Memory Patchification（多级记忆分块）压缩历史上下文，Pyramid Unified Predictor Corrector（金字塔统一预测校正器）实现多分辨率采样，将计算成本降至1.3B模型水平。
## InfinityStory: Unlimited Video Generation with World Consistency and Character-Aware Shot Transitions【Adobe】

[https://arxiv.org/pdf/2603.03646](https://arxiv.org/pdf/2603.03646)

- 论文提出了InfinityStory框架，核心创新包括： 位置锚定背景注入：通过预生成固定场景库（location library）并将角色合成到统一背景，确保同一场景内所有镜头共享恒定视觉环境 电影化多主体过渡合成（CMTS）：构建包含10,000个多主体过渡序列的合成数据集，训练**首末帧到视频（First-Last-Frame-to-Video, FLF2V）**模型，实现角色自然的进出画运动（entry/exit/reposition） 分层多智能体规划：通过章节-场景-镜头四级代理结构，将小时级故事分解为具有显式约束（奇偶镜头交替生成、场景-位置绑定）的可执行单元.
## RealWonder: Real-Time Physical Action-Conditioned Video Generation【Jiajun Wu】

[https://arxiv.org/pdf/2603.05449](https://arxiv.org/pdf/2603.05449)

- 首个实现实时物理动作条件视频生成的系统，能够从单张图像出发，根据3D物理动作（如外力、力场、机器人操作）生成逼真的视频流，并达到 **13.2 FPS**（480×832分辨率）的实时性能。
## InSpatio-WorldFM: An Open-Source Real-Time Generative Frame Model

[https://arxiv.org/pdf/2603.11911](https://arxiv.org/pdf/2603.11911)

- **基于视频的世界模型在实时空间智能应用中的根本性限制。**论文提出InSpatio-WorldFM，一种开源的实时生成帧模型，其核心创新包括： **帧级生成范式：将世界建模从"视频序列生成"转变为"独立帧生成"，每个帧基于显式空间信息（3D锚点）与隐式空间记忆（参考帧注意力）独立合成**，消除窗口级依赖。 混合空间记忆机制：结合显式3D锚点（点云渲染提供全局几何约束）与隐式神经记忆（参考图像提供外观细节），在单帧推理中实现多视图一致性。 渐进式三阶段训练：从预训练图像扩散模型（Stage I）→ 可控帧模型（Stage II）→ 实时少步生成器（Stage III，通过分布匹配蒸馏实现2步去噪），逐步优化实时性能。 消费级GPU部署：通过少步蒸馏与工程优化，在RTX 4090等消费级GPU上实现约7-10 FPS的实时交互，延迟控制在50-70毫秒。
## EVATok: Adaptive Length Video Tokenization for Efficient Visual Autoregressive Generation

[https://arxiv.org/pdf/2603.12267](https://arxiv.org/pdf/2603.12267)

- 面向AR视频生成的自适应长度视频分词框架，旨在解决固定长度分词导致的计算效率与重建质量失衡问题。传统方法采用**固定长度分配**（fixed-length tokenization），对所有视频样本和时序块分配相同数量的token。四阶段训练流程： 训练Proxy Tokenizer用于奖励估计； 构建（视频，最优分配）数据集； 训练Router进行快速分配预测； 训练最终自适应分词器消除训练-推理差距。
## EvoTok: A Unified Image Tokenizer via Residual Latent Evolution for Visual Understanding and Generation

[https://arxiv.org/pdf/2603.12108](https://arxiv.org/pdf/2603.12108)

- **统一MLLMs中视觉理解与生成之间的粒度差距问题**。EvoTok试图通过残差潜在演化（Residual Latent Evolution）实现以下双重目标： 任务解耦（Task Decoupling）：为理解和生成提供专门的表示，避免优化冲突 跨任务一致性（Cross-task Consistency）：在共享潜在空间内保持像素级和语义级表示的关联性 具体实现方式是将图像表示为级联残差令牌序列（cascaded sequence of residual tokens），形成演化轨迹： 早期残差阶段捕获空间结构和感知细节（用于生成） 深层阶段逐渐累积并过渡到高级语义表示（用于理解） 通过这种方式，EvoTok在单一统一潜在空间内实现了从像素到语义的渐进演化，既避免了纠缠表示的相互干扰，又克服了解耦表示的空间隔离问题。提出残差演化轨迹表示范式，在单一共享潜在空间内实现像素级与语义级特征的渐进过渡设计任务解耦且一致的统一训练框架，缓解理解与生成的优化冲突同时保持特征关联性。
## Fair Benchmarking of Emerging One-Step Generative Models Against Multistep Diffusion and Flow Models

[https://arxiv.org/pdf/2603.14186](https://arxiv.org/pdf/2603.14186)

- 解决**新兴单步生成模型（one-step generative models）与多步扩散/流模型之间缺乏公平、标准化比较**的问题，并揭示单一评估指标（特别是FID）在模型选择和超参数调优中的局限性。**单步模型的步数扩展性**：原生单步模型（MeanFlow、iMF、SoFlow）在扩展至25步时质量显著提升（如SoFlow的MMHM从0.72升至0.88），而强制多步模型（SD3.5、FLUX.1）进行1步推理会导致严重退化 FID的误导性：最低FID配置常对应较差的感知质量（如模糊、面部扭曲），而MMHM选择与视觉保真度更一致 定量-定性差距：即使单步模型在MMHM上接近SOTA（如iMF FID=6.20），仍存在局部结构扭曲（如面部几何异常），表明当前指标仍无法完全捕捉人类感知真实感
## Out of Sight, Out of Mind? Evaluating State Evolution in Video World Models

[https://arxiv.org/pdf/2603.13215](https://arxiv.org/pdf/2603.13215)

- **视频世界模型在观察中断条件下的状态演化能力?**现实世界中的物理过程（如倒水、燃烧、融化）具有**观察无关性**——无论是否被相机记录或被物体遮挡，状态演化持续进行。然而，当前视频世界模型通过生成2D像素帧来"模拟"世界，**其能否在非完全可观察条件下（如遮挡、关灯、相机转向）维持正确的状态演化**，此前缺乏系统性评估。**通用视频生成模型（Veo 3, Sora 2 Pro等） 任务成功率<10%**，状态进展率仅13-17% 演化停止：观察控制（如关灯）导致过程冻结（如床垫停止放气） 不一致性：遮挡移除后物体属性突变（如矩形海绵变为圆形） 对比实验：完全可观察条件下状态进展率达84.6%，证明观察控制显著破坏演化能力 **相机控制视频模型（Genie 3, WorldPlay, LingBot等） 静态场景强偏置：状态进展率<5%**，相机移动时场景几乎总是冻结（球不落下、波浪不前进） 控制-演化权衡：当模型成功演化动态过程（如火箭发射）时，会忽略相机控制指令（相机不转向或反向跟随物体） 记忆模块局限：VMem等记忆架构虽能准确回忆初始帧，但强化静态偏置，无法支持状态演化
## Thinking in Dynamics: How Multimodal Large Language Models Perceive, Track, and Reason Dynamics in Physical 4D World

[https://arxiv.org/pdf/2603.12746](https://arxiv.org/pdf/2603.12746)

- **MLLMs在物理4D世界（三维空间加时间维度）中动态理解与推理能力不足**的问题。论文提出了**Dyn-Bench**基准测试，从三个互补层次评估MLLMs的4D理解能力：**动态对象间感知**（捕捉移动对象间的空间关系和交互）、**动态对象-场景跟踪**（建模对象在场景中的时间演化）和**动态相机-对象推理**（分析相机运动对动态对象感知的影响）。
## Omni-WorldBench: Towards a Comprehensive Interaction-Centric Evaluation for World Models【World Model】

[https://arxiv.org/pdf/2603.22212](https://arxiv.org/pdf/2603.22212)

- **视频世界模型（video-based world models）评估基准在"交互响应能力"（interactive response capabilities）维度上的系统性缺失**问题。论文提出Omni-WorldBench——首个以交互为中心的综合性评估基准，通过以下组件实现对世界模型交互能力的严格量化：** Omni-WorldSuite：涵盖三级交互层级（自身状态变化、局部物体间交互、全局环境改变）及多样化场景类型（自动驾驶、具身智能、游戏等）的系统性提示套件。 **Omni-Metrics：**基于智能体的评估框架，通过测量交互动作对最终结果与中间状态演化轨迹的因果影响，量化交互效果保真度（Interaction Effect Fidelity）、相机-物体可控性及生成视频质量，并自适应融合为统一的AgenticScore。**
## UniGRPO: Unified Policy Optimization for Reasoning-Driven Visual Generation【ByteDance Seed】

[https://arxiv.org/pdf/2603.23500](https://arxiv.org/pdf/2603.23500)

- 生成式AI正朝着**交错生成（Interleaved Generation）演进，即模型能够交替进行文本推理与图像生成。这种范式要求模型先通过思维链（Chain-of-Thought）扩展用户提示，再基于推理结果合成图像。然而，现有方法通常将文本推理与图像生成分离优化，缺乏统一框架。为此，论文将"Prompt → Thinking → Image"的完整序列建模为单一的马尔可夫决策过程（MDP），提出通过统一策略优化**同时提升推理质量与生成质量。论文提出 Unified Group Relative Policy Optimization (UniGRPO)，采用极简方法论整合两个成熟技术： **文本策略（Text Policy）：采用标准 GRPO 优化自回归文本生成**，最大化推理令牌的对数似然； 图像策略（Image Policy）：**采用改进的 FlowGRPO 优化流匹配（Flow Matching）图像生成**。
## OmniWeaving: Towards Unified Video Generation with Free-form Composition and Reasoning【Tencent Hunyuan】

[https://arxiv.org/pdf/2603.24458](https://arxiv.org/pdf/2603.24458)

- 论文提出 OmniWeaving，通过以下关键设计实现真正的"全能级"（omni-capable）视频生成： 统一架构：整合多模态大语言模型（MLLM）与扩散 Transformer，将视觉理解与生成模块耦合 **推****理增强训练：激活MLLM的"思考模式"（thinking mode），显式生成中间推理步骤（如将模糊查询扩展为详细视频描述），再指导生成** 大规模组合数据：构建涵盖多模态组合与推理增强场景的训练数据集，**支持从交错的文本/多图像/视频输入中学习时间绑定与语义关系 **简言之，该工作旨在推动开源视频生成模型从任务特定的专家演进为具备组合与推理能力的统一通才，缩小与闭源商业系统的性能鸿沟。
## BizGenEval: A Systematic Benchmark for Commercial Visual Content Generation

[https://arxiv.org/pdf/2603.25732](https://arxiv.org/pdf/2603.25732)

- **BizGenEval**，首个针对商业视觉内容生成的系统性基准测试，旨在解决现有图像生成评估体系无法覆盖专业设计场景的问题。论文构建了覆盖双正交维度的评估体系： 五大内容领域：幻灯片（Slides）、图表（Charts）、网页（Webpages）、海报（Posters）、科学图表（Scientific Figures） 四大能力维度：文本渲染（Text Rendering）、布局控制（Layout Control）、属性绑定（Attribute Binding）、知识推理（Knowledge-based Reasoning） 交叉形成 20 个评估任务，包含： 400 个精心设计的提示词（300个基于真实商业素材，100个基于领域知识） 8,000 个人工验证的二元核查问题（每提示词20问，分简单/困难各10问）针对每个生成图像，**通过多模态大语言模型（MLLM，如 Gemini-3-Flash）回答20个验证问题（如"左栏是否比右栏宽？""化学方程式配平是否正确？"）**
## Wan-Weaver: Interleaved Multi-modal Generation via Decoupled Training【Tongyi】

[https://arxiv.org/pdf/2603.25706](https://arxiv.org/pdf/2603.25706)

- 论文提出Wan-Weaver框架，通过**解耦训练策略（decoupled training）**将交错生成分解为： 文本规划（Textual Planning）：**由规划器（Planner）负责决策生成内容的模态序列与视觉内容的文本化描述（**dense prompts）； 视觉一致性建模（Visual Consistency Modeling）：**由可视化器（Visualizer）基于参考引导生成保持上下文一致的图像**。 通过构建文本代理交错数据（textual-proxy interleaved data）和参考引导生成数据（reference-guided image data），该方法在不依赖真实交错数据的情况下，实现了对长距离跨模态上下文和视觉一致性的有效学习。
## VGGRPO: Towards World-Consistent Video Generation with 4D Latent Reward【Google】

[https://arxiv.org/pdf/2603.26599](https://arxiv.org/pdf/2603.26599)

- 在解决**大规模视频扩散模型在生成过程中缺乏几何一致性和世界一致性**的问题。论文提出的解决方向 为克服上述局限，论文提出**VGGRPO（Visual Geometry GRPO）**框架，核心创新在于： 潜在空间几何计算：构建潜在几何模型（LGM），直接在VAE潜在空间解码场景几何（相机位姿、深度、点云、场景流），消除RGB解码瓶颈 4D动态场景支持：通过连接支持4D重建的几何基础模型，将方法扩展到动态场景，突破静态场景假设限制 双重奖励机制：设计互补的相机运动平滑度奖励（惩罚抖动轨迹）和几何重投影一致性奖励（强制跨视图几何相干），实现世界一致性视频生成 简言之，论文解决了如何在保持预训练模型泛化能力的同时，高效提升几何一致性并支持动态场景的关键问题。
## Gen-Searcher: Reinforcing Agentic Search for Image Generation

[https://arxiv.org/pdf/2603.28767](https://arxiv.org/pdf/2603.28767)

- 论文提出 **Gen-Searcher**，首次尝试通过智能体强化学习（agentic RL）训练一个多模态深度搜索智能体，使其能够主动执行多跳网络搜索和推理，收集必要的文本知识与视觉参考图像，从而为图像生成提供基于外部搜索的 grounded 信息支持。
## Unlocking Complex Visual Generation via Closed-Loop Verified Reasoning

[https://arxiv.org/pdf/2605.14876](https://arxiv.org/pdf/2605.14876)

- **闭环视觉推理（Closed-Loop Visual Reasoning, CLVR）**框架，旨在解决当前文本到图像（T2I）模型在处理复杂语义时面临的系统性瓶颈，突破单步生成范式的能力天花板。将视觉-语言逻辑规划与像素级扩散生成深度耦合，通过**验证中心的数据合成**、**长上下文稳定对齐**、**全局一致推理**和**免重新蒸馏加速**，解锁了复杂视觉生成任务的通用测试时缩放能力，在多项基准上超越开源基线并接近专有商业模型性能。
## DiffusionOPD: A Unified Perspective of On-Policy Distillation in Diffusion Models【Alibaba】

[https://arxiv.org/pdf/2605.15055](https://arxiv.org/pdf/2605.15055)

- 解决扩散模型（diffusion models）在强化学习（RL）框架下的多任务优化难题。 具体而言，现有基于RL的扩散模型改进方法**大多局限于单任务优化（如单独优化美学质量、文本渲染准确性或组合对齐等），而实际应用通常要求单一模型同时满足多个异构目标（如既美观又忠实于文本提示且能正确渲染文字）**。将RL扩展到多任务设置面临以下根本性挑战： **联合优化（Joint Optimization）**的困境：同时训练所有任务会导致目标冲突（cross-task interference）和任务难度不平衡（task-difficulty imbalance），不同任务的优化方向相互干扰，简单任务往往主导学习动态而抑制困难任务的信号。 **级联优化（Cascade RL）的局限：按顺序逐任务训练虽能避免梯度冲突，但流程繁琐、需要精心设计训练计划，且存在灾难性遗忘（catastrophic forgetting）**风险——适应后续任务时会损害先前习得的能力。 为此，论文提出DiffusionOPD框架，通过**在线策略蒸馏（Online Policy Distillation, OPD）**解耦单任务探索与多任务能力整合：先独立训练任务专属教师模型（避免相互干扰），再将这些教师的能力蒸馏到统一的学生模型中（沿学生自身轨迹进行监督），从而规避多任务联合优化的冲突与级联训练的遗忘问题。

# Agent Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Training
- 方法：agent, reinforcement-learning
- 论文/报告：1 篇
- ARL-Tangram: Unleash the Resource Efficiency in Agentic Reinforcement Learning【Xiaomi】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## ARL-Tangram: Unleash the Resource Efficiency in Agentic Reinforcement Learning【Xiaomi】

[https://arxiv.org/pdf/2603.13019](https://arxiv.org/pdf/2603.13019)

- 解决 **智能体强化学习（Agentic RL）** 过程中极高的资源浪费问题，并提出了一个名为 **ARL-Tangram** 的高效资源管理系统。核心思想是 “动作级编排”（Action-level Orchestration）。 精细化管理： **系统不再以“任务”或“整个轨迹”为单位分配资源，而是精确到每一个“动作”（Action）。只有当智能体真正需要调用外部工具或进行某项计算时，系统才动态分配资源，用完即收回**。 弹性调度算法： 开发了一套调度算法，**在满足复杂的异构资源约束（不同动作需要不同硬件）的同时，最大限度地缩短 动作完成时间**（ACT, Action Completion Time）。

# Agent Applications

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Applications
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, science-discovery, vision
- 论文/报告：103 篇
- DARE-bench: Evaluating Modeling and Instruction Fidelity of LLMs in Data Science
- Let There Be Claws: An Early Social Network Analysis of AI Agents on Moltbook【OpenClaw】
- EvoX: Meta-Evolution for Automated Discovery【Research】
- SciDER: Scientific Data-centric End-to-end Researcher【Research】
- DeepResearch-9K: A Challenging Benchmark Dataset of Deep-Research Agent【Research】
- MM-DeepResearch: A Simple and Effective Multimodal Agentic Search Baseline【Research】
- Reasoning as Gradient: Scaling MLE Agents Beyond Tree Search【Research, MSRA】
- Talk Freely, Execute Strictly: Schema-Gated Agentic AI for Flexible and Reproducible Scientific Workflows【Research】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## DARE-bench: Evaluating Modeling and Instruction Fidelity of LLMs in Data Science

[https://arxiv.org/pdf/2602.24288](https://arxiv.org/pdf/2602.24288)

- LLMs在数据科学（Data Science, DS）任务中的评估与训练瓶颈，提出了一个可验证、可训练的大规模基准测试 **DARE-bench**（Datascience Agentic REasoning bench）。
## Let There Be Claws: An Early Social Network Analysis of AI Agents on Moltbook【OpenClaw】

[https://arxiv.org/pdf/2602.20044](https://arxiv.org/pdf/2602.20044)

- 分析AI代理（AI agents）在社交平台上互动时所形成的网络结构及其 emergent properties（涌现特性），特别是考察**社会分层（stratification）和等级结构是否在极短时间内迅速涌现**，而非逐渐演变。证实熟悉的在线社会现象——包括极端注意力不平等、快速层级化、广播式互动与角色分化——可在AI代理环境中以**压缩的时间尺度**（12天）极端涌现。
## EvoX: Meta-Evolution for Automated Discovery【Research】

[https://arxiv.org/pdf/2602.23413](https://arxiv.org/pdf/2602.23413)

- EvoX将优化形式化为双层协同进化过程：内循环（解决方案进化）：在给定搜索策略下，LLM基于父代候选和变异算子生成新解，经评估后积累至种群数据库外循环（元进化）：监控内循环的进展速率，当检测到停滞时，基于历史策略表现和当前种群状态，通过LLM生成新的搜索策略
## SciDER: Scientific Data-centric End-to-end Researcher【Research】

[https://arxiv.org/pdf/2603.01421](https://arxiv.org/pdf/2603.01421)

- 论文提出了 SciDER（Scientific Data-centric End-to-end Researcher），一个以数据为中心的模块化系统，通过以下机制实现突破： 自主数据处理：通过**专门的数据分析代理独立解析原始实验数据，生成结构化的数据报告（包含数据结构、质量、语义和依赖关系分析）**，而非依赖静态脚本 数据驱动的实验设计：确保假设生成和实验代码生成严格基于实际数据特征，而非仅依赖文献或通用模板 专业化代理协作：通过构思代理（Ideation）、数据分析代理（Data Analysis）、实验代理（Experimentation）和评判代理（Critic）的协同工作，形成闭环反馈 自进化记忆机制：通过短期和长期（任务特定与项目特定）记忆分类，结合检索增强生成（RAG），实现测试时学习和领域特定能力的持续积累 简言之，SciDER 旨在构建一个能够**自主处理原始科学数据、生成数据驱动的研究假设并执行跨学科实验的全自动化研究伙伴**，从而降低科学发现的技术门槛，加速数据驱动的创新。
## DeepResearch-9K: A Challenging Benchmark Dataset of Deep-Research Agent【Research】

[https://arxiv.org/pdf/2603.01152](https://arxiv.org/pdf/2603.01152)

- 论文提出了DeepResearch-9K数据集和DeepResearch-R1训练框架：DeepResearch-9K：包含9,000个跨越三个难度级别（L1-L3）的问题，难度与搜索工具调用次数显式挂钩（L1平均4.30次，L2平均10.74次，L3平均20.23次），并包含高质量搜索轨迹与可验证答案DeepResearch-R1：支持多轮网络交互、多种强化学习算法和不同奖励模型的开源训练框架，通过低成本（约200美元）的自动化流程实现数据合成。
## MM-DeepResearch: A Simple and Effective Multimodal Agentic Search Baseline【Research】

[https://arxiv.org/pdf/2603.01050](https://arxiv.org/pdf/2603.01050)

- 论文提出了三个互补的技术方案： Hyper-Search：基于超图的QA生成方法，通过建模视觉与文本节点及其跨模态关系，生成需要多轮多工具搜索才能解决的QA对； DR-TTS（Decompose–Recompose Tool Tree Search）：通过分解任务训练专用工具专家，再通过树搜索重新组合以探索高质量搜索轨迹； 离线搜索引擎：支持多种搜索工具的离线检索系统，使强化学习训练无需依赖昂贵的在线API。
## Reasoning as Gradient: Scaling MLE Agents Beyond Tree Search【Research, MSRA】

[https://arxiv.org/pdf/2603.01692](https://arxiv.org/pdf/2603.01692)

- 论文提出 **Gome**（Gradient-based Optimization for Machine Learning Engineering），将MLE重构为**基于梯度的优化问题。**
## Talk Freely, Execute Strictly: Schema-Gated Agentic AI for Flexible and Reproducible Scientific Workflows【Research】

[https://arxiv.org/pdf/2603.06394](https://arxiv.org/pdf/2603.06394)

- 解决**科学工作流中执行确定性与对话灵活性之间的结构性张力**。提出 Schema-Gated Orchestration（模式门控编排） 作为化解这一张力架构原则： 权限分离：将"对话权限"（解释意图、提出候选方案）与"执行权限"（实际运行计算）分离 强制验证边界：在组合工作流层面建立强制性的模式验证门控——除非完整动作（包括跨步骤依赖）通过机器可验证的规范验证，否则不得执行 三个操作原则： 执行前澄清（Clarification-before-execution）：缺失字段和类型错误表现为对话提示，而非静默失败 受约束的计划-行动编排（Constrained plan-act orchestration）：规划阶段可无约束推理，行动阶段必须通过模式验证 工具到工作流级别的门控（Tool-to-workflow-level gating）：将单工具调用的模式验证扩展到多步骤工作流的跨步骤依赖验证 通过这一架构，论文旨在实现既允许研究人员自由对话探索，又确保所有执行都经过严格验证和记录的科学工作流系统。
## ⭐⭐DeepFact: Co-Evolving Benchmarks and Agents for Deep Research Factuality【Research, Amazon】

[https://arxiv.org/pdf/2603.05912](https://arxiv.org/pdf/2603.05912)

- 提出**模型与基准共同进化**的评估哲学：随着AI能力接近或超越专家水平，评估应转变为可审计、可修正的持续对话过程，而非依赖冻结的静态快照。提出演进式基准测试（Evolving Benchmarking）框架，将基准视为动态共识而非静态数据集： 四阶段循环：（1）挑战者模型验证当前基准并提交证据提案；（2）审计者（专家或智能体）裁决冲突；（3）接受优质证据更新基准；（4）所有模型针对新版本评分。 核心洞察：专家作为审计者（审核带证据的模型提案）显著优于独立标注，四轮AtS后审计准确率达 90.9%，证明人机协作可持续修正共识。 智能体审计：验证智能体可替代人类执行审计，形成自改进的评估生态。
## PostTrainBench: Can LLM Agents Automate LLM Post-Training?【Research】

[https://arxiv.org/pdf/2603.08640](https://arxiv.org/pdf/2603.08640)

- 一个关键问题在于其能否将自动化能力延伸至AI研发本身，特别是**后训练阶段**（将基础模型转变为实用助手的关键环节）。POSTTRAINBENCH采用零起始、完全自主的评估范式：**输入：向智能体提供基础模型（Qwen3-1.7B/4B、SmolLM3-3B、Gemma-3-4B）、目标基准（涵盖数学、代码、工具使用、科学推理等7类任务）及网络访问权限，不提供任何初始训练代码或数据**。最佳智能体（Claude Opus 4.6）达到**23.2%**加权平均性能，显著优于基座模型（7.5%），但仍远低于官方指令微调模型（51.1%）。
## ResearchEnvBench: Benchmarking Agents on Environment Synthesis for Research Code Execution【Research, Xipeng Qiu】

[https://arxiv.org/pdf/2603.06739](https://arxiv.org/pdf/2603.06739)

- 针对自主智能体（autonomous agents）在科学研究代码执行中面临的环境配置瓶颈，提出了专门的基准测试框架。现有智能体基准测试（如 SWE-bench、MLE-bench）普遍假设执行环境已预配置，忽视了科研场景（深度学习与高性能计算）中环境构建的复杂性：Python 依赖解析、CUDA 驱动与框架版本对齐、自定义算子编译、分布式通信原语配置等。现有环境配置基准（如 EnvBench）仅依赖静态分析（检查缺失导入）或构建成功信号，无法检测运行时硬件不匹配或二进制不兼容问题。 **数据集**： curated 44 个 2024 年后创建的高质量科研仓库，涵盖生成式视觉、LLM 推理、音频处理等 8 个领域，均涉及复杂硬件依赖、自定义 CUDA 内核与分布式训练需求。失败集中于需编译的 CUDA/C++ 算子（如 `mmcv._ext`、`flash_attn`），这些需特定 PyTorch/CUDA ABI 匹配，非简单 `pip install` 可解决。
## AutoResearch-RL: Perpetual Self-Evaluating Reinforcement Learning Agents for Autonomous Neural Architecture Discovery【Research】

[https://arxiv.org/pdf/2603.07300](https://arxiv.org/pdf/2603.07300)

- 本文直接继承并形式化了Karpathy（2025）提出的**autoresearch原型**：该原型在单GPU设置中让LLM智能体修改`train.py`，运行固定时间预算（5分钟），读取验证指标，并决定提交或回滚更改。AutoResearch-RL将其扩展为严格的MDP框架，引入PPO训练、自评估模块与收敛理论分析。
## RbtAct: Rebuttal as Supervision for Actionable Review Feedback Generation【Research】

[https://arxiv.org/pdf/2603.09723](https://arxiv.org/pdf/2603.09723)

- **LLMs在生成科学论文同行评审反馈时缺乏可操作性的问题**。如何利用同行评审过程中作者提供的反驳（Rebuttal）作为隐式监督信号，训练能够生成高可操作性、具体且可实施改进建议的**评审反馈模型**？
## **The Agentic Researcher: A Practical Guide to AI-Assisted Research in Mathematics and Machine Learning**【Research】

[https://arxiv.org/pdf/2603.15914](https://arxiv.org/pdf/2603.15914)

- **AI工具如何有效、负责任地融入数学与机器学习研究的日常实践？**论文最终强调：**模型能力固然重要，但工作流设计同等关键**——只有通过结构化规则、可审计制品与强制验证机制，才能将通用AI代理转化为可靠的研究协作者。
## Auto Researching, not hyperparameter tuning: Convergence Analysis of 10,000 Experiments【Research】

[https://arxiv.org/pdf/2603.15916](https://arxiv.org/pdf/2603.15916)

- **当LLM代理自主设计机器学习实验时，它们究竟是在进行真正的架构搜索（Architecture Search），还是仅仅在固定架构内进行超参数调优（Hyperparameter Tuning）**。LLM代理的关键贡献在于发现了人类未提出的配置（V-JEPA 2视频特征+Zipformer时间编码器），从而验证了其在组合式机器学习实验设计中进行真正架构搜索的能力。
## AgentDS Technical Report: Benchmarking the Future of Human-AI Collaboration in Domain-Specific Data Science【Research】

[https://arxiv.org/pdf/2603.19005](https://arxiv.org/pdf/2603.19005)

- **在领域特定的数据科学任务中，自主AI代理与人类专家相比表现如何，以及在哪些方面人类专业知识仍然具有不可替代的优势**。论文提出了 **AgentDS** 基准测试与竞赛框架，包含**17个跨六个行业（商业、食品生产、医疗保健、保险、制造业和零售银行）的挑战任务**。这些任务经过专门设计，使得仅依赖通用机器学习流水线的方案表现不佳，而融入领域特定洞察的方法则能显著提升性能，从而精确衡量AI在复杂领域推理方面的局限性以及人类-AI协作的有效性。
## AI Scientist via Synthetic Task Scaling【Research, Princeton】

[https://arxiv.org/pdf/2603.17216](https://arxiv.org/pdf/2603.17216)

- **如何训练能够自主进行科学发现（特别是机器学习研究）的AI代理**这一核心问题。论文试图解决**如何自动合成高质量、基于真实数据、可验证的ML任务环境**，以支持大规模代理训练。论文提出了一个**合成任务扩展（Synthetic Task Scaling）**框架：** 自动生成与SWE-agent框架兼容的多样化ML任务（涵盖主题采样、数据集提议、代码生成） 通过Huggingface API验证数据集真实性，并通过自调试循环（self-debugging loop）确保任务质量 利用教师模型（GPT-5）在合成任务上采样轨迹，用于训练学生模型（Qwen3-4B/8B）** 实验表明，该方法在MLGym基准测试上取得了显著性能提升（AUP指标分别提升9%和12%），验证了合成环境作为训练AI科学家有效途径的可行性。
## HindSight: Evaluating Research Idea Generation via Future Impact【Research, Idea】

[https://arxiv.org/pdf/2603.15164](https://arxiv.org/pdf/2603.15164)

- 对比检索增强系统（ResearchAgent）与无检索基线生成的200个想法： 系统区分度 HINDSIGHT显示检索系统得分2.5倍于基线（0.297 vs 0.119, p<0.001 ）；LLM-as-Judge认为两者无差异（7.44 vs 7.40, p=0.584 ） 新颖性悖论	HINDSIGHT分数与LLM评判的新颖性呈显著负相关（ρ=−0.29,p<0.01 ） 可行性关联	与可行性正相关（ρ=+0.25 ），表明技术具体的想法更可能预见真实研究 案例分类	检索系统产生23%的"Hidden Gems"（被LLM低估但匹配未来论文），基线产生34%的"Overhyped"（LLM高估但无实际对应）
## OpenResearcher: A Fully Open Pipeline for Long-Horizon Deep Research Trajectory Synthesis【Research, Wenhu Chen】

[https://arxiv.org/pdf/2603.20278](https://arxiv.org/pdf/2603.20278)

- 论文提出OPENRESEARCHER框架，通过以下关键设计解决上述问题：离线合成架构：将一次性语料库构建（含在线引导）与多轮轨迹生成分离，后续合成完全在本地离线搜索引擎上执行；显式浏览器原语：定义search、open、find三种最小化操作，支持从粗粒度检索到细粒度证据定位的多尺度信息发现；可控分析环境：基于固定语料库（1500万文档）和确定性工具实现，支持对检索成功与最终答案准确性的因果关系进行精确分析。通过该方案，论文实现了在零API调用成本下合成97K+条长程轨迹（含大量100+工具调用的尾部案例），并验证了离线合成数据对开放网络环境的有效泛化能力。
## ⭐AI-Supervisor: Autonomous AI Research Supervision via a Persistent Research World Model【Research】

[https://arxiv.org/pdf/2603.24402](https://arxiv.org/pdf/2603.24402)

- **AI-Supervisor**框架通过引入**持续演化的研究世界模型（Research World Model）、自纠正多代理共识机制和跨领域自我改进循环**，试图将AI研究监督本身自动化，使个人能够基于好奇心驱动进行专业化AI研究，无需依赖传统机构资源。AI-Supervisor的关键区别：**动态构建：在研究过程中通过并行代理的章节特定提取构建**，非预构建；不确定性注释：节点和边携带不确定性标注（U∈{0,1}），仅经实证测试后更新为已验证（U=0）；**主动验证：复现期间主动验证边，当报告性能无法复现时标记为U=1**；编排骨干：作为编排骨干使用，路由决策、差距发现和跨会话持久性均通过研究世界模型而非对话历史或扁平状态
## From Intent to Evidence: A Categorical Approach for Structural Evaluation of Deep Research Agents【Research】

[https://arxiv.org/pdf/2603.25342](https://arxiv.org/pdf/2603.25342)

- 该工作试图从碎片化的经验测试转向**基于范畴论的形式化框架**，以实现对深度研究智能体复杂结构信息处理能力的严格、可解释和机制感知的评估。
## Not Search, But Scan: Benchmarking MLLMs on Scan-Oriented Academic Paper Reasoning【Research】

[https://arxiv.org/pdf/2603.28651](https://arxiv.org/pdf/2603.28651)

- 现有学术文献推理工作主要遵循**搜索导向（search-oriented）范式**，即**基于预指定目标（pre-specified targets）检索相关片段进行局部推理**。该范式难以支持研究人员式的**全文理解、交叉验证与隐式问题发现**。为此，论文提出**扫描导向范式，要求模型在目标缺失（target-absent）条件下，主动扫描整篇论文，构建文档级证据视图**，执行跨源一致性验证。论文提出扫描导向（scan-oriented）范式，要求模型： **在目标缺失（target-absent）的查询条件下，主动扫描整篇论文 构建文档级证据视图（document-level evidence view） 执行基于证据的跨源一致性验证（cross-source consistency checking）**
## MiroEval: Benchmarking Multimodal Deep Research Agents in Process and Outcome【Research】

[https://arxiv.org/pdf/2603.28052](https://arxiv.org/pdf/2603.28052)

- 解决**深度研究系统（Deep Research Systems）评估方法与实际用户需求之间的关键脱节问题**。为应对上述挑战，论文提出了 MiroEval——一个包含100个任务（70个纯文本，30个多模态）的基准测试与评估框架，其核心创新包括： 双路径构建流程：结合真实用户查询改写（隐私保护）与基于实时网络趋势的自动化生成，支持定期刷新以保持时效性； 三层互补评估体系： 自适应综合质量评估：动态生成任务特定的评分标准与权重，超越固定指标； 智能体事实性验证：通过主动检索与推理，验证报告声明与网络来源及多模态附件的一致性（引入 RIGHT/WRONG/CONFLICT/UNKNOWN 四分类体系）； 以过程为中心的评估：审计研究轨迹的五个内在维度（搜索广度、分析深度、渐进式优化、批判性思维、效率）及过程与报告的双向对齐性（Process→Report 与 Report→Process）。 实验结果表明，这三个评估维度捕获了系统能力的互补方面，且过程质量是总体输出质量的可靠预测指标，能揭示输出层指标无法发现的结构性弱点（如可追溯性缺口）。
## Marco DeepResearch: Unlocking Efficient Deep Research Agents via Verification-Centric Design【Research, Alibaba】

[https://arxiv.org/pdf/2603.28376](https://arxiv.org/pdf/2603.28376)

- **Deep Research Agents中因缺乏显式验证机制而导致的错误传播与性能退化问题**。论文提出Marco DeepResearch，一个基于**验证中心设计（Verification-Centric Design）**的8B规模深度研究代理，通过以下三个层面的改进阻断错误传播： 验证驱动的QA数据合成：引入对抗性唯一性验证与多阶段质量过滤，确保问题难度可控且答案唯一正确； 验证驱动的轨迹构建：设计包含显式验证子代理的多智能体框架，将验证-修正模式注入训练轨迹； 验证器引导的测试时扩展：将模型自身作为验证器，结合"丢弃全部（Discard All）"上下文管理策略，在固定预算内实现更有效的推理时计算扩展。
## AIRA_2: Overcoming Bottlenecks in AI Research Agents【Research, Meta】

[https://arxiv.org/pdf/2603.26499](https://arxiv.org/pdf/2603.26499)

- 论文识别了限制智能体性能扩展的关键瓶颈：**计算吞吐量瓶颈**：同步单GPU执行导致推理循环阻塞，样本吞吐量受限（每天仅1-20个候选），无法支持深度探索。**泛化差距（过拟合）**：验证集与测试集性能发散，智能体通过"操纵"自报告指标或利用随机分割波动来优化，导致长期搜索性能退化。**静态算子限制**：固定、单轮的LLM提示（如预设的"调试"或"特征工程"操作符）无法适应复杂依赖关系，限制了推理深度和任务范围。论文提出AIRA2，通过三层架构创新系统性解决上述问题： **异步多GPU执行层**：采用稳态进化（steady-state evolution）和1:1 Worker-GPU映射，消除同步屏障，实现实验吞吐量随GPU数量线性扩展（8 GPU带来约8倍吞吐）。容器化（Apptainer）确保环境隔离与鲁棒性。 **隐藏一致性评估协议（HCE）：将数据划分为互斥的Dtrain （训练）、Dsearch （搜索指导，标签隐藏）和Dval （最终选择，全程隐藏），确保评估信号固定、一致且外部化，防止智能体干预评估过程**。** ReAct智能体操作符**：以多轮推理-行动轨迹替代静态提示，支持动态任务范围界定（自主决定EDA、实验、调试等动作序列）和交互式错误恢复，突破单轮推理天花板。
## AutoResearchClaw: Self-Reinforcing Autonomous Research with Human-AI Collaboration【Research】

[https://arxiv.org/pdf/2605.20025](https://arxiv.org/pdf/2605.20025)

- 解决**现有自主科学研究系统在模拟真实科研迭代过程方面的根本性缺陷**。论文指出这三个挑战并非独立：更优质的假设减少执行阶段的重大修订需求；更鲁棒的执行保留可用于分析的中间结果；过往运行的经验可同时改进后续假设生成与实验设计。 为此，论文提出AutoResearchClaw系统，通过以下五个协同机制联合解决上述问题： 结构化多智能体辩论：在假设生成与结果分析阶段分配"创新者-实用主义者-反对者"等角色，通过批判性质询暴露假设弱点 自修复执行与Pivot/Refine决策循环：将失败转化为诊断信息，自动选择修复当前实验（Refine）或基于失败证据转换方向（Pivot） 可验证结果报告：通过数值注册表与四层引文验证管道，防止虚构数据与幻觉引用 人机协作：在七个干预模式之间提供灵活选择，证明在关键决策点进行精准人工干预优于完全自主或全程监督 跨运行演化：通过时间衰减权重机制将历史错误转化为未来运行的结构化指导 论文通过在ARC-Bench基准测试上的评估验证，该系统在实验阶段的表现较AI Scientist v2提升54.7% ，尤其在结果分析维度实现100.4% 的相对改进。
## Meta-Reinforcement Learning with Self-Reflection for Agentic Search【Search】

[https://arxiv.org/pdf/2603.11327](https://arxiv.org/pdf/2603.11327)

- **基于RL的智能体搜索（agentic search）中稀疏奖励（sparse rewards）导致的信用分配（credit assignment）困难与探索效率低下**问题。论文提出MR-Search（Meta-Reinforcement Learning with Self-Reflection），通过以下机制解决上述问题： 跨episode元学习：将搜索建模为序列化的meta-episode，允许智能体基于历史episode的上下文进行显式自反思（self-reflection），实现测试时的上下文探索（test-time in-context exploration）（§3.2, Figure 1） 细粒度回合级优势估计：设计多轮RL算法，通过分组相对优势（grouped relative advantages）在回合级别（turn-level）进行无偏信用分配，无需额外价值模型（§3.3, Eq. 7-9） 端到端探索-利用平衡：通过自反思机制**将独立的搜索尝试转化为渐进式信息积累过程**，避免对外部过程奖励模型的依赖（§3.4） 简言之，该论文**试图在不依赖外部过程监督信号的前提下，使搜索智能体能够通过元学习与自反思自主改进多轮搜索策略**，解决稀疏奖励环境下的信用分配与探索效率问题。
## Improving Search Agent with One Line of Code【Search, Tencent Youtu】

[https://arxiv.org/pdf/2603.10069](https://arxiv.org/pdf/2603.10069)

- 引入**条件化的token级KL约束**，通过软惩罚机制替代硬裁剪，仅在特定条件下对策略漂移施加惩罚。
## OpenSeeker: Democratizing Frontier Search Agents by Fully Open-Sourcing Training Data【Search】

[https://arxiv.org/pdf/2603.15594](https://arxiv.org/pdf/2603.15594)

- 通过**全开源**的方式提供高质量的训练数据和模型权重，推动该领域的协作与透明化。OpenSeeker **仅通过 1.17万条 合成样本的简单指令微调（SFT）**，就达到了行业领先水平。其成功归功于两个关键技术： 事实驱动的可控 QA 合成（Fact-grounded Scalable Controllable QA Synthesis）： **通过对 Web 图谱进行“逆向工程”，利用拓扑扩展（Topological Expansion）和实体关联来生成问题。**这确保了合成的问题具有深度和复杂性，且每一个问题都挂钩到真实的事实背景。 去噪轨迹合成（Denoised Trajectory Synthesis）： 回顾性总结机制（Retrospective Summarization）： **在生成搜索路径时，模型会回看并剔除无效的搜索步骤或冗余操作。** 通过这种方式“去噪”，可以让作为教师模型的 LLM 生成更高质量的操作动作，从而提升训练数据的纯度。
## OneSearch-V2: The Latent Reasoning Enhanced Self-distillation Generative Search Framework【Search】

[https://arxiv.org/pdf/2603.24422](https://arxiv.org/pdf/2603.24422)

- **OneSearch-V2**，一种面向电商搜索的**潜在推理增强自蒸馏生成式检索框架**，通过三项核心创新解决了现有生成式搜索系统在复杂查询理解、个性化意图推理和偏好对齐方面的关键局限。OneSearch-V2 的核心贡献在于： 零成本推理增强：通过自蒸馏将显式推理完全内化到模型参数，推理阶段无需生成额外 token 或调用外部模块，保持与 V1 相同的延迟与计算开销； 精准信用分配：TPMA 机制首次针对层次化 SID 序列设计位置感知优势，解决粗粒度到细粒度生成的信用模糊问题； 健康优化范式：直接行为反馈替代奖励模型，结合可动态调整的复合奖励，支持实时业务干预（如大促流量扶持），避免历史日志过拟合。
## ⭐AutoSkill: Experience-Driven Lifelong Learning via Skill Self-Evolution【Skill, AI Lab】

[https://arxiv.org/pdf/2603.01145](https://arxiv.org/pdf/2603.01145)

- 论文提出**AutoSkill**框架，通过将重复的交互经验抽象为显式、可版本控制、可编辑的技能（Skill）工件，实现无需重新训练基础模型的终身学习能力，使智能体能够跨会话积累个性化能力。AutoSkill 采用双耦合循环架构（如图1所示）： **技能增强的经验获取循环（在线）**：通过查询重写、混合检索（稠密向量+BM25）与上下文注入，将相关技能动态注入当前对话； **技能进化循环（后台）：从用户查询（非模型响应）中提取候选技能**，经检索辅助的管理决策（add/merge/discard）与版本化合并，持续更新技能库。
## 🧐SkillNet: Create, Evaluate, and Connect AI Skills**【Skill, Huajun Chen】**

[https://arxiv.org/pdf/2603.04448](https://arxiv.org/pdf/2603.04448)

- 论文提出 **SkillNet**——一个开源基础设施，通过构建统一的本体论（Ontology）将技能形式化为**可演化、可组合的资产（evolving, composable assets）**，实现从"瞬时经验（transient experience）"到"持久掌握（durable mastery）"的转化，为代理的持续性能力增长提供基础。
## Trace2Skill: Distill Trajectory-Local Lessons into Transferable Agent Skills【Skill, Qwen】

[https://arxiv.org/pdf/2603.25158](https://arxiv.org/pdf/2603.25158)

- 提出 Trace2Skill 框架，旨在解决为LLM智能体自动创建和进化领域特定技能的核心挑战。Trace2Skill 模拟人类专家编写技能的方式：**在广泛分析执行经验后，将通用模式蒸馏为单一、全面的声明式技能目录**。其核心为三阶段并行流水线： 轨迹生成：智能体在任务集上并行执行，产生成功与失败轨迹池。 并行多智能体补丁提议：部署独立的专业子智能体（错误分析师与成功分析师）同时处理各轨迹。错误分析师采用多轮交互式诊断（可检查文件、验证修复），成功分析师识别可泛化的有效模式，各自提出技能修改补丁（patch）。 无冲突分层整合：通过层级式合并算子将所有补丁同时整合为连贯的技能更新。该过程执行归纳推理——识别在独立补丁中反复出现的普遍模式（prevalent patterns）作为系统性领域知识，解决冲突并去重，最终生成无冲突的技能目录（包含主文档与辅助参考资料）。
## Dynamic Dual-Granularity Skill Bank for Agentic RL【Skill】

[https://arxiv.org/pdf/2603.28716](https://arxiv.org/pdf/2603.28716)

- 论文提出**动态双粒度技能库（D2Skill）**，通过以下方式解决： 双粒度建模：同时维护****任务技能（task skills）提供高层次指导，和步骤技能（step skills）**支持细粒度决策与局部错误纠正** 动态维护机制：基于 hindsight utility 信号进行技能效用评估，通过反思（reflection）扩展技能库，并通过效用感知的检索与修剪（utility-aware retrieval and pruning）维持紧凑、高效的记忆存储 该框架使策略与技能库在训练过程中联合演化，确保技能记忆始终信息丰富且有利于策略优化。**仅当任务组成功率低于阈值时，利用外部LLM分析失败与成功轨迹，生成新技能。**
## ⭐Memento-Skills: Let Agents Design Agents【Skill】

[https://arxiv.org/pdf/2603.18743](https://arxiv.org/pdf/2603.18743)

- 解决**冻结LLM智能体无法从部署经验中持续学习和进化**的核心问题。论文提出 Memento-Skills ——一个基于Read–Write Reflective Learning的通用智能体系统： **技能即记忆：将可执行的技能（代码、提示、声明式规范）作为外部记忆单元，而非存储原始交互轨迹** 零参数更新：**所有适应通过进化外部技能库和提示实现**，无需修改LLM参数 自主智能体设计：系统能够**自主构建、适应和改进任务特定智能体**，实现"让智能体设计智能体"的愿景 持续学习闭环：通过"读取（检索技能）-执行-反馈-写入（优化技能）"的循环，实现从经验中的持续改进 该框架在GAIA和Humanity's Last Exam基准测试中分别实现了**26.2%和116.2%**的相对性能提升，证明了通过外部技能记忆实现非参数化持续学习的有效性。
## XSKILL: Continual Learning from Experience and Skills in Multimodal Agents【Skill】

[https://arxiv.org/pdf/2603.12056](https://arxiv.org/pdf/2603.12056)

- 针对多模态智能体在开放式环境中的**工具使用效率低下**与**工具编排灵活性不足**两大瓶颈，提出了一种无需参数更新的持续学习框架 **XSKILL**。通过构建基于视觉观察的双流知识表示（**技能库与经验库**），实现**从多路径轨迹中提取、整合并适配知识，形成"积累-推理-反馈"的持续学习闭环**，从而在无需重新训练模型参数的前提下，显著提升多模态智能体的工具使用效率与编排灵活性。
## SkillRouter: Retrieve-and-Rerank Skill Selection for LLM Agents at Scale【Skill, Alibaba】

[https://arxiv.org/pdf/2603.22455](https://arxiv.org/pdf/2603.22455)

- **大规模LLM代理系统中的技能路由（skill routing）问题**，即如何在包含数万技能（工具/插件）的大型池中，根据用户任务描述准确检索并选择最相关的技能。论文提出了**SKILLROUTER**——一个总参数量仅1.2B（0.6B编码器+0.6B重排序器）的检索-重排序流水线，在保持紧凑可部署性的同时，解决大规模技能池中的精确路由问题。
## SWE-Hub: A Unified Production System for Scalable, Executable Software Engineering Tasks【Code, Baidu】

[https://arxiv.org/pdf/2603.00575](https://arxiv.org/pdf/2603.00575)

- 解决**软件工程（SWE）智能体在训练和评估过程中面临的高质量、可执行数据稀缺**这一核心瓶颈。论文提出 SWE-Hub —— 一个统一的数据工厂（data factory）生产系统，通过以下方式 operationalize 数据生成： Env Agent：建立共享执行基底，自动将原始仓库快照转换为可复现的多语言容器环境； SWE-Scale：结合跨语言代码分析与集群级验证，实现大规模本地化错误修复实例的高吞吐生成； Bug Agent：生成涉及跨模块依赖的系统级回归故障，并配以描述可观察症状而非根因的用户式问题报告； SWE-Architect：将自然语言需求转换为仓库级构建任务，扩展从修复到创建的任务范围。通过三阶段流水线（环境配置 → 测试入口标准化 → 镜像固化），自动将异构仓库快照转换为**固定的容器镜像**（pinned container image）与**标准化验证接口**（standardized verifier entrypoint），确保跨语言的可复现性与确定性验证。
## Agentic Code Reasoning【Code, Meta】

[https://arxiv.org/pdf/2603.01896](https://arxiv.org/pdf/2603.01896)

- **LLM 智能体能否在不执行代码的情况下，探索代码库并准确推理代码语义**。
## RepoLaunch: Automating Build&Test Pipeline of Code Repositories on ANY Language and ANY Platform【Code】

[https://arxiv.org/pdf/2603.05026](https://arxiv.org/pdf/2603.05026)

- 首个能够自动构建和测试任意编程语言、任意操作系统（Linux/Windows）代码仓库的智能体系统，并基于此构建了完全自动化的软件工程（SWE）数据集生成流程。自主探索仓库结构，安装依赖，编译代码，执行回归测试自动生成最小重建命令集和测试日志解析器支持完全自动化的SWE数据集创建（仅需人工设计任务，其余步骤自动化）在Linux和Windows上实现约70%的构建成功率，显著超越现有基线方法。
## SWE-Fuse: Empowering Software Agents via Issue-free Trajectory Learning and Entropy-aware RLVR Training【Code, Ant】

[https://arxiv.org/pdf/2603.07927](https://arxiv.org/pdf/2603.07927)

- 论文提出了SWE-Fuse训练框架，通过以下机制提升智能体性能： 无问题描述驱动的轨迹学习（Issue-free Trajectory Learning）：通过融合带问题描述和不带问题描述（issue-free）的样本进行训练，使模型能够减轻潜在误导性问题描述的干扰，同时通过逐步调试过程学习问题定位与修复能力。 熵感知RLVR训练（Entropy-aware RLVR Training）：在强化学习阶段，根据策略熵（policy entropy）动态调整裁剪（clipping）阈值——在高熵（不确定性高）时放宽裁剪以鼓励探索，在低熵（确定性高）时收紧裁剪以确保训练稳定性，从而实现更高效的策略优化。
## ⭐⭐Towards a Neural Debugger for Python【Code, Meta】

[https://arxiv.org/pdf/2603.09951](https://arxiv.org/pdf/2603.09951)

- 论文提出了神经调试器（neural debuggers）——一种能够**模拟传统调试器操作的语言模型**，支持以下核心功能： 正向执行预测：**根据调试器动作（如step_into、step_over、step_return、breakpoint）条件化地预测未来程序状态和输出**。 逆向执行预测：**从任意程序状态出发，推断或采样合理的前置状态、函数输入或程序参数**。 交互式执行控制：**通过断点设置和单步操作，实现对程序执行的细粒度控制，而无需重新执行整个程序**。使神经调试器能够作为模拟调试环境的"世界模型”。
## EvoClaw: Evaluating AI Agents on Continuous Software Evolution【Code, Mengdi Wang】

[https://arxiv.org/pdf/2603.13428](https://arxiv.org/pdf/2603.13428)

- AI 编程助手（AI Agents）在处理长期、连续的软件开发任务时表现如何？研究者开发了一套工具和基准： DeepCommit 管道： 这是一个自动化的 Agent 流程，它能够从杂乱的、包含大量噪声的 GitHub 提交记录（Commit Logs）中，提取并重构出可验证的里程碑有向无环图（Milestone DAGs）。 它将琐碎的提交整理成具有语义一致性的“里程碑”任务。 EvoClaw 基准： 基于上述里程碑，EvoClaw 要求 AI Agent 连续完成一系列相关的开发任务，同时必须维持系统的可运行性。**目前的 AI Agent 虽然是优秀的“搬砖工”（能写好单个函数或修复单个 Bug），但还不是合格的“工程师”（无法在长期的软件生命周期中维持代码健康）。**
## daVinci-Env: Open SWE Environment Synthesis at Scale【Code, Pengfei Liu】

[https://arxiv.org/pdf/2603.13023](https://arxiv.org/pdf/2603.13023)

- 研究团队推出了 OpenSWE，这是目前**全球最大的全透明 Python 软件工程智能体训练框架**。 规模巨大：包含 45,320 个可执行的 Docker 环境，涵盖了超过 1.28 万个 GitHub 仓库。 全自动化流水线：开发了一套基于多智能体（Multi-agent）的合成流水线，部署在 64 节点的分布式集群上。**它可以自动完成： 仓库探索（自动找 Bug 和任务）。 Dockerfile 自动构建（搭建运行环境）。 评价脚本生成（自动写测试用例来验证修复是否成功）。 迭代测试分析。**
## IQuest-Coder-V1 Technical Report【Code, IQuest】

[https://arxiv.org/pdf/2603.16733](https://arxiv.org/pdf/2603.16733)

- 解决**开源代码大语言模型与顶尖专有模型（如Claude 4.5 Sonnet、GPT-5.1）在长程推理、复杂代码库理解与代理式软件工程能力方面的显著差距**，同时探索**模型性能与部署效率之间的优化权衡**。
## InCoder-32B: Code Foundation Model for Industrial Scenarios【Code, IQuest】

[https://arxiv.org/pdf/2603.16790](https://arxiv.org/pdf/2603.16790)

- **现有代码LLMs在工业编程场景中的性能显著不足**的问题。论文提出了**InCoder-32B**，这是首个32B参数的统一工业代码基础模型，通过三阶段"Code-Flow"训练流程（预训练-退火、中期训练、后训练），结合仿真环境驱动的验证机制，**首次在单一模型中统一支持芯片设计、GPU内核优化、嵌入式系统、编译器优化和3D建模等工业领域**。
## SWE-QA-Pro: A Representative Benchmark and Scalable Training Recipe for Repository-Level Code Understanding【Code】

[https://arxiv.org/pdf/2603.16124](https://arxiv.org/pdf/2603.16124)

- 首个针对长尾仓库、强制要求真实代码交互的仓库级 QA 基准。
## Evaluating Agentic Optimization on Large Codebases【Code】

[https://arxiv.org/pdf/2603.16011](https://arxiv.org/pdf/2603.16011)

- **现有代码生成基准测试无法充分评估大型语言模型（LLM）智能体在真实代码库上进行仓库级、多目标性能优化能力**的问题。论文提出了 FORMULACODE 基准，旨在通过以下方式推进智能体代码优化评估： 真实世界任务：从70个科学Python仓库中挖掘957个真实性能瓶颈，每个任务配有专家编写的补丁和社区维护的性能工作负载（平均每个任务264.6个工作负载）。 细粒度多指标评估：引入几何平均加速比（speedup）、相对于人类专家的优势（advantage）、分层优势（stratified advantage）和成本加权指标等，以捕捉优化质量、一致性和效率。 仓库级规模：评估智能体在大型、相互关联的代码库中进行全局优化的能力，而非仅针对孤立函数。 持续更新机制：作为"活"基准，每月更新以防止数据污染（data leakage），并确保与不断演进的人类基线进行对比。
## CodeScout: An Effective Recipe for Reinforcement Learning of Code Search Agents【Code】

[https://arxiv.org/pdf/2603.17829](https://arxiv.org/pdf/2603.17829)

- 本文提出了CODESCOUT——一种基于Group Sequence Policy Optimization（GSPO）的强化学习方案，通过精心设计的数据筛选、奖励函数（基于多粒度F1分数）和训练策略，证明**即使仅使用**`**ripgrep**`**、**`**sed**`**等基础Unix命令，也能在SWE-Bench系列基准上实现与18倍参数量的开源模型相当、甚至接近Claude Sonnet等闭源前沿模型的性能**。
## Coding Agents are Effective Long-Context Processors【Code, Long Context】

[https://arxiv.org/pdf/2603.20432](https://arxiv.org/pdf/2603.20432)

- 探索将长上下文处理从潜在注意力机制外部化为显式、可执行的交互，具体通过： **利用编码代理（coding agents）的原生工具（终端命令、Python脚本、文件系统操作）替代被动的语义查询** **将文本语料库重构为可导航的文件系统层次结构，使代理能够像在代码库中导航一样处理长文本** 允许代理通过编写和执行代码来显式地组织、过滤和转换文本，而非依赖固定的检索管道 该方法旨在提供一种比语义搜索或简单上下文扩展更有效、更灵活、更可解释的长上下文处理替代方案。
## SWE-Next: Scalable Real-World Software Engineering Tasks for Agents【Code, Wenhu Chen】

[https://arxiv.org/pdf/2603.20691](https://arxiv.org/pdf/2603.20691)

- SWE数据在大规模收集过程中的两个核心瓶颈：**数据质量与信号强度的保障，以及环境构建与存储的系统成本问题**。SWE-Next，一个面向软件工程（SWE）智能体的**可扩展、基于执行验证的数据收集框架**，旨在解决真实代码库中高质量训练数据稀缺以及环境构建成本高昂的双重瓶颈。1. 执行验证的任务合成 从3,971个Python仓库的合并PR中提取候选提交对 (cbase,cmerged) 实施三层过滤：仓库级预筛选、提交级启发式过滤、以及严格的执行后过滤；2. 可复用的季度环境配置（Repo-Quarter Profiles） 将提交时间戳映射到粗粒度季度配置（如 repo_2022Q2），捕获缓慢变化的依赖环境 构建共享的季度环境镜像（含系统包、Python解释器、缓存虚拟环境），不包含源代码 运行时通过"挂载+写时复制"机制隔离各提交的具体代码快照，实现环境复用与执行隔离的统一 构建失败时自动回退到每提交独立环境 3. 高信号轨迹收集协议 严格提交门控：仅当智能体产生非空代码差异且执行至少一次测试命令时才允许提交 非泄漏提示：默认不向提示中注入 FAIL_TO_PASS/PASS_TO_PASS 测试列表，防止标签泄漏 保留"干净成功"与"恢复成功"两类轨迹用于监督微调（SFT）
## SciNav: A General Agent Framework for Scientific Coding Tasks【Code】

[https://arxiv.org/pdf/2603.20256](https://arxiv.org/pdf/2603.20256)

- 解决**科学编程任务中缺乏端到端结构化智能体框架**的问题。论文提出SciNav框架，通过**Top-K比较树搜索（TKCTS）**解决上述问题：**在受限搜索预算下运行，不依赖预定义的成功指标利用成对相对判断（而非绝对评分）提供更细粒度的质量区分能力通过树搜索过程选择前K个有前景的解决方案分支，剪枝低潜力分支，逐步缩小候选范围支持受控回溯和自适应优先排序，有效平衡系统探索与计算成本**简言之，该论文填补了"开放式科学智能体"与"科学编程基准测试"之间的鸿沟，为具有客观评估标准但缺乏现成优化指标的科学编码任务提供了一个结构化、实用且可扩展的智能体框架。
## Code Review Agent Benchmark【Code】

[https://arxiv.org/pdf/2603.23448](https://arxiv.org/pdf/2603.23448)

- 随着LLM在代码生成与Issue修复中的广泛应用，自动生成的Pull Request（PR）数量激增，但代码审查环节仍主要依赖人工，形成开发流程瓶颈。论文提出将人类审查意见系统化转换为可执行测试用例（executable tests），作为客观评估标准（oracle）： 每个测试满足 fail-then-pass 属性：在原始代码（before）上失败，在修复后代码（after）上通过； 评估流程：审查工具生成意见 → 编码代理依据意见修复代码 → 执行测试验证修复是否解决原始问题； 通过率 si=|{t∈Ti∣t(P^i)=PASS}||Ti| 量化审查工具与人类审查意图的对齐程度.
## What Do Evolutionary Coding Agents Evolve?【Code】

[https://arxiv.org/pdf/2605.20086](https://arxiv.org/pdf/2605.20086)

- 核心问题是：**LLM驱动的进化编码代理（evolutionary coding agents）在搜索过程中实际上在进化什么，以及这些改进是如何产生的**。论文构建了**EvoTrace**数据集（涵盖四个进化框架、16个任务、超过10,000个程序的全量搜索轨迹），并开发了**EvoReplay**方法学，通过重放搜索状态、控制干预（如模型替换、上下文替换、贝叶斯优化调参）和编辑类型标注，将进化过程从"黑箱"转变为可分析的计算对象，从而系统性地诊断进化编码代理的行为模式。主要发现 编辑类型频率与效用存在显著鸿沟 超参数调优（Hyperparameter tuning）占编辑的大多数，但单位效用最高的类别是外部依赖引入（External dependency, OR=3.58）、效率优化（Efficiency, OR=1.61）和架构变更（Architectural change, OR=1.55）。最终最佳谱系主要由效率、外部依赖和超参数调优富集。 确定性循环现象 约30%的新增代码行是谱系中先前删除过的行的字节级完全复现，且该比例在118/121次运行中随迭代单调增长。搜索预算大量浪费在重新引入已丢弃的材料，中位循环周期仅5次迭代。 重放稳定性：结构性而非词汇性 对36个高分突破事件的同提示重放显示：几乎从不产生完全相同的程序（精确匹配率≈0），但能恢复中位数0.76的原始分数。分数增益可由相同上下文复现，但具体代码实现是更宽泛分布中的一个样本。 调优差距：后期进化可被事后调优替代 在数学任务上，对单次中间程序进行24次BO调优： 在22/36个目标上超过该程序原始分数 在13/15个中间程序上匹配或超越整个进化运行的最终最佳分数（中位数差距+0.025） 这表明数学基准上的许多"进化"实质是参数搜索，早期结构发现才是核心价值。在ALE竞赛任务上，发现2/4框架在至少30%问题上存在公共/私有测试集过拟合。**数学基准上的许多"进化"实质是参数搜索，早期结构发现才是核心价值**。
## WebFactory: Automated Compression of Foundational Language Intelligence into Grounded Web Agents【Web】

[https://arxiv.org/pdf/2603.05044](https://arxiv.org/pdf/2603.05044)

- **如何将LLM中压缩的互联网知识高效转化为可在GUI环境中执行的具身智能体行为？论文提出"智能压缩"（Intelligence Compression）概念，主张通过高效的工厂化流程，****将LLM的被动知识转化为主动的、grounded的智能****，而非简单依赖大规模人工标注数据。论文提出WebFactory——一个全自动化的闭环强化学习管道，通过以下阶段系统性地解决上述问题：高保真离线环境合成：消除非确定性和安全风险，提供完全可观测、可复现的训练环境；知识感知任务生成：利用环境可观测性和LLM知识自动生成多样化、可执行的任务；分解奖励的RL训练：设计统一动作空间和细粒度奖励函数，将LLM知识蒸馏为agent策略。论文证明，仅在10个合成网站数据上训练的agent，其性能可与在更大量人工标注数据上训练的agent相当，验证了"智能压缩"范式的有效性。**
## WebVR: Benchmarking Multimodal LLMs for WebPage Recreation from Videos via Human-Aligned Visual Rubrics【Web】

[https://arxiv.org/pdf/2603.13391](https://arxiv.org/pdf/2603.13391)

- 首个专为评估MLLMs从演示视频忠实重建网页能力而设计的基准测试。提出与人类偏好对齐的原子化二元验证标准 R={r1,…,rK} ，按四个正交维度组织： 维度	评估内容 Global Aesthetics (GA)	整体色调、字体、视觉语言一致性 Navigation and Footer (NF)	页头菜单、页脚等持久性 UI 组件 Section-Specific Layouts (SSL)	区块级布局、网格、层级、对齐与间距 Interaction and Motion (IM)	悬停反馈、滚动动画、状态转换等动态行为。
## LifeBench: A Benchmark for Long-Horizon Multi-Source Memory【Memory】

[https://arxiv.org/pdf/2603.03781](https://arxiv.org/pdf/2603.03781)

- 当前主流的记忆基准测试（如 LOCOMO、LongMemEval）主要聚焦于**陈述性记忆（declarative memory）**，即**语义记忆（事实知识）和情景记忆（个人经历）**，且假设所有信息都显式地存在于对话文本中。然而，现实世界中人类行为还受**非陈述性记忆（non-declarative memory）**驱动，包括： **习惯性记忆（habitual memory）**：日常 routines、偏好 **程序性记忆（procedural memory）**：技能习得、行为模式 这些记忆无法从对话中直接获取，而必须从碎片化的数字痕迹（如健康记录、日历、短信、照片等）中推断。显式建模多记忆系统（陈述性与非陈述性），并利用部分学层次结构（partonomic hierarchy）组织事件，确保跨时间尺度的全局一致性 整合真实世界先验（匿名社会调查、地图 API、节假日日历）确保行为合理性与多样性 设计并行化生成策略，将单用户年规模数据的合成时间从 58 小时降至 8 小时 构建包含 2,003 个问题的评估集，覆盖信息提取、多跳推理、时间演化、非陈述性记忆推理和不可回答问题五大类别。
## AriadneMem: Threading the Maze of Lifelong Memory for LLM Agents【Memory】

[https://arxiv.org/pdf/2603.03290](https://arxiv.org/pdf/2603.03290)

- 解决**长周期LLM代理（long-horizon LLM agents）在****固定上下文预算下维护准确长期记忆**的核心问题。离线构建阶段：采用熵感知门控（entropy-aware gating）过滤噪声，并通过冲突感知粗化（conflict-aware coarsening）合并静态重复项，同时将状态更新保留为显式的时间边（temporal edges）； 在线推理阶段：利用算法桥接发现（algorithmic bridge discovery）自动重建缺失的逻辑路径（通过近似Steiner树检索），将多跳规划负担从LLM转移到图算法层，实现单轮拓扑感知合成（single-call topology-aware synthesis）。
## Memex(RL): Scaling Long-Horizon LLM Agents via Indexed Experience Memory【Memory】

[https://arxiv.org/pdf/2603.04257](https://arxiv.org/pdf/2603.04257)

- **长程（long-horizon）LLM智能体在有限上下文窗口下的记忆管理瓶颈。**提出Memex机制，通过索引化经验记忆（Indexed Experience Memory）实现在不丢弃证据的前提下压缩上下文： 分离设计：将紧凑的上下文内索引摘要（in-context indexed summary）与全保真度的外部经验存档（external experience archive）分离 精确检索：通过稳定索引（stable indices）实现显式解引用（explicit dereferencing），而非模糊的语义匹配 强化学习优化：通过MemexRL框架学习何时总结、何时存档、如何索引、何时检索，使记忆访问比纯摘要方法 substantially less lossy（显著减少信息损失）
## AutoAgent: Evolving Cognition and Elastic Memory Orchestration for Adaptive Agents【Memory】

[https://arxiv.org/pdf/2603.09716](https://arxiv.org/pdf/2603.09716)

- 论文提出 AutoAgent 框架，通过三个紧密耦合的组件实现自适应智能体： 演化认知（Evolving Cognition）：将工具、自我能力、同伴专长和任务知识建模为可更新的结构化提示级状态； 弹性记忆编排（Elastic Memory Orchestration）：动态组织交互历史，通过选择性压缩和情景抽象减少令牌开销，同时保留决策关键证据； 上下文决策（Contextual Decision-Making）：基于当前认知与实时上下文，从统一动作空间（包括工具调用、生成与智能体间请求）中动态选择动作，取代预定义工作流。 这些组件通过闭环认知演化（closed-loop cognitive evolution）整合，使智能体能够基于执行结果持续更新认知并扩展可重用技能，无需外部重训练。
## Trajectory-Informed Memory Generation for Self-Improving Agent Systems【Memory, IBM】

[https://arxiv.org/pdf/2603.10600](https://arxiv.org/pdf/2603.10600)

- 解决**LLM驱动智能体如何从执行经验中系统性学习以持续改进性能**的核心问题。论文提出的框架致力于实现轨迹知情记忆生成（Trajectory-Informed Memory Generation），通过： 对智能体推理模式进行**语义分析**（而非仅表面日志记录） 自动归因决策与结果间的因果关系（区分直接原因、近因与根本原因） 生成三类结构化指导：策略提示（成功模式）、恢复提示（错误处理）、优化提示（效率改进） 基于多维相似性进行自适应检索，确保在特定任务上下文中注入最相关的学习经验
## AndroTMem: From Interaction Trajectories to Anchored Memory in Long-Horizon GUI Agents【Memory】

[https://arxiv.org/pdf/2603.18429](https://arxiv.org/pdf/2603.18429)

- 解决**长程（long-horizon）GUI代理中的交互记忆（interaction memory）瓶颈问题**。论文构建了**AndroTMem-Bench**基准（1,069个任务，34,473步），**强制要求代理在多应用工作流中维护并复用关键中间状态**；并进一步提出**Anchored State Memory (ASM)**，**通过将交互历史组织为因果链接的状态锚点集合（state anchors），实现子目标导向的精准检索与归因感知决策**，从而在长程GUI任务中突破记忆瓶颈。
## SmartSearch: How Ranking Beats Structure for Conversational Memory Retrieval【Memory】

[https://arxiv.org/pdf/2603.15599](https://arxiv.org/pdf/2603.15599)

- **SmartSearch**，一种面向长对话记忆检索的新型架构，通过**确定性检索循环**与**轻量级习得排名**的分离设计，在不依赖LLM进行检索的前提下，实现了超越现有记忆系统的准确率与 token 效率。
## NextMem: Towards Latent Factual Memory for LLM-based Agents【Memory】

[https://arxiv.org/pdf/2603.15634](https://arxiv.org/pdf/2603.15634)

- **NextMem**，一种面向基于LLM智能体的新型潜在事实记忆（Latent Factual Memory）框架，旨在解决现有记忆范式在存储效率与信息保真度之间的固有矛盾。论文提出NextMem框架，核心目标是构建一种**潜在事实记忆（latent factual memory）**机制，实现： **高效压缩：将文本记忆转换为与LLM输入兼容的紧凑潜在表示（latent representations），显著缩短序列长度**；** 无损重构：确保潜在表示能够准确、可逆地解码（reconstruct）回原始文本，满足事实记忆对细节精确保存的要求（lossless preservation）**。 通过引入自回归自编码器（autoregressive autoencoder）结合两阶段训练策略（自回归重构对齐与渐进潜在替换），并集成量化技术降低存储开销，该工作试图在记忆存储效率、检索精度与重构保真度之间建立新的平衡。
## Memori: A Persistent Memory Layer for Efficient, Context-Aware LLM Agents【Memory】

[https://arxiv.org/pdf/2603.19935](https://arxiv.org/pdf/2603.19935)

- 论文提出Memori——一个持久记忆中间层，通过Advanced Augmentation流程将**原始对话蒸馏为： 语义三元组（subject–predicate–object）：捕获原子化事实** 对话摘要：保留时序背景和叙事脉络 这种双重表示实现了在仅使用~5%原始上下文token（平均1,294 tokens）的情况下，达到81.95%的准确率，显著降低了推理成本（比全上下文方法节省20倍以上），同时避免了传统长上下文带来的性能退化。核心洞察：**记忆管理的本质是数据结构问题，而非单纯存储问题**。
## MemEye: A Visual-Centric Evaluation Framework for Multimodal Agent Memory【Memory】

[https://arxiv.org/pdf/2605.15128](https://arxiv.org/pdf/2605.15128)

- 解决**多模态智能体长期记忆评估中视觉证据缺失与状态演化推理不足**的问题。解决方案框架 论文提出 MemEye 评估框架，**通过两个正交维度构建视觉中心的记忆评估： X轴（视觉证据粒度）：从场景级（X1）到像素级（X4），测量记忆系统需保留的视觉细节粒度 Y轴（记忆推理深度）：从原子检索（Y1）到演化综合（Y3）**，测量对检索证据的时序推理要求 基于该框架，作者构建包含371对镜像问题（多选+开放）的基准测试，覆盖8个生活场景任务，并通过三层过滤机制（答案泄露检测、视觉绕过检测、难度校准）确保问题必须具备不可替代的视觉证据和记忆推理结构。通过评估13种记忆方法在4种VLM骨干上的性能，论文揭示当前架构在以下方面存在显著不足：保留细粒度视觉细节（高X区域）、追踪时序变化中的有效视觉状态（高Y区域）、在跨主题长历史中选择相关证据。这凸显了多模态记忆系统需同时具备视觉证据保留、时序状态追踪与证据选择路由能力的必要性。
## MemLens: Benchmarking Multimodal Long-Term Memory in Large Vision-Language Models【Memory】

[https://arxiv.org/pdf/2605.14906](https://arxiv.org/pdf/2605.14906)

- 首个针对**多模态长时会话记忆**的综合性基准，系统评估了长上下文大型视觉语言模型（LVLMs）与记忆增强代理在跨模态、多轮对话场景下的性能。评估规模：27个LVLMs（含GPT-5.4、Claude Sonnet 4.5、Gemini-3.1-Pro、Kimi-K2.5、Qwen系列等）与7个记忆代理（M3-Agent、M2A、Mem0、Memory-T1等）。 关键结论： 能力独立性：五种记忆能力相关性低（Spearman ρ最低-0.19），需独立评估；MSR为共同瓶颈，多数系统准确率<30%。 互补失效模式： 长上下文LVLMs：短上下文下通过直接视觉grounding获得高准确率（Gemini-3.1-Pro达55.8%@32K），但随上下文增长显著退化（128K时降至51.99%，AR能力从97.8%降至45%）。 记忆代理：长度稳定性好（32K→256K变化<7%），但存储时视觉压缩导致保真度损失（IE/KU任务差距最大），且后训练削弱拒绝能力（AR从77%降至9-22%）。 瓶颈定位：Oracle检索实验表明，MSR任务准确率可从<30%提升至90-100%（证据直接提供时），证明当前瓶颈主要在证据检索而非推理能力。
## The World Won't Stay Still: Programmable Evolution for Agent Benchmarks【Self-Evolving】

[https://arxiv.org/pdf/2603.05910](https://arxiv.org/pdf/2603.05910)

- 现实世界的环境是**持续演化**的：新功能被逐步引入，现有工具被迭代升级，过时工具被逐渐弃用。这种静态假设导致现有评估无法全面检验智能体应对环境动态变化的适应能力与鲁棒性。论文提出 PROEVOLVE 框架，其核心贡献在于： 显式建模：采用**类型化关系图（typed relational graph）**统一表示环境中的数据、工具和 schema，将环境元素及其依赖关系显式编码为图结构； 可编程演化：将环境演化形式化为图转换（graph transformations），通过定义明确的编辑操作（如功能补全、饱和加速、服务弃用）实现自动化的、结构一致的环境生成； 可控评估：支持基于子图采样的任务沙盒（task sandboxes）生成，从而在动态演化的环境中对智能体进行可控制的评估。
## Tool-Genesis: A Task-Driven Tool Creation Benchmark for Self-Evolving Language Agent【Self-Evolving】

[https://arxiv.org/pdf/2603.05578](https://arxiv.org/pdf/2603.05578)

- 自进化语言智能体（self-evolving language agents）在工具创建（tool creation）能力评估方面的关键缺陷。论文提出了 Tool-Genesis，一个诊断性基准测试，其核心创新包括： 需求驱动的工具创建设置：要求智能体在缺失或欠指定接口的情况下，从抽象需求推断契约，生成机器可检查的模式，并产生满足可重用性和可维护性标准的可执行实现。 全生命周期诊断协议：提供一个基于执行的统一评估框架，联合测量： L1（表面合规性）：MCP合规率和服务器可执行率 L2（语义接口保真度）：模式级F1分数（Schema-F1） L3（功能正确性）：通过显式负/边界单元测试的通过率（UTsoft/UThard） L4（下游任务效用）：Oracle归一化成功率（SR），量化生成工具与参考工具在相同任务分布下的效用差距 错误归因能力：通过多层级验证信号，实现工具质量与使用策略的解耦，解决"黑盒"困境。 论文的关键发现是：即使是最先进的模型，在一次性设置中也难以构建精确的工具接口或可执行逻辑，这些微小的初始缺陷会通过管道被放大，导致下游指标急剧下降。Tool-Genesis 旨在引导未来研究朝向合成能够解决更广泛现实世界挑战的持久、通用工具。
## EigenData: A Self-Evolving Multi-Agent Platform for Function-Calling Data Synthesis, Auditing, and Repair【Self-Evolving】

[https://arxiv.org/pdf/2603.05553](https://arxiv.org/pdf/2603.05553)

- 针对函数调用智能体（function-calling agents）面临的数据质量瓶颈，提出了 EigenData——一个集成的、自进化的多智能体平台，用于自动化数据生命周期管理，从环境构建到轨迹生成与质量保证。论文提出由 EigenCore 顶层编排器协调的三个专门子系统： DatabaseAgent：自动构建领域数据库。通过约束感知采样、分布建模和边缘案例注入，生成具有引用完整性和现实数据分布的合成记录，作为评估的 ground-truth 状态。 CodingAgent：生成经过验证的可执行环境。采用两阶段迭代测试-调试流程（单元测试→工作流测试），结合基于评判者（JudgeAgent）的故障归因机制，确保生成的Python模块、API或MCP服务器在功能上正确无误。 DataAgent：合成多轮交互轨迹。通过分层多智能体架构（编排层与执行层）和两阶段自进化流程（先在小批量上优化提示，再规模化生成），利用LLM-as-Judge进行实时质量监控，并生成可执行的验证函数（VerificationFunctionAgent）用于强化学习奖励建模。
## MM-Zero: Self-Evolving Multi-Model Vision Language Models From Zero Data【Self-Evolving】

[https://arxiv.org/pdf/2603.09206](https://arxiv.org/pdf/2603.09206)

- 论文提出了MM-Zero框架——首个基于强化学习的零数据自进化VLM训练框架。该框架通过以下创新实现突破： 三角色协同架构：突破传统的"提出者-解决者"(Proposer-Solver)双角色范式，**引入Coder角色作为视觉合成器，将抽象概念转化为可执行代码（如Python、SVG）以渲染视觉图像**，形成"Proposer-Coder-Solver"的闭环系统。 完全合成的数据生成：不依赖任何外部图像、问题或人工标签，模型从简单场景开始，自主生成视觉内容并逐步提高复杂度。 多维度奖励机制：结合执行反馈（代码渲染验证）、视觉验证（语义正确性检查）和难度平衡（Goldilocks原则），通过Group Relative Policy Optimization (GRPO)优化三个角色。
## SAGE: Multi-Agent Self-Evolution for LLM Reasoning【Self-Evolving】

[https://arxiv.org/pdf/2603.15255](https://arxiv.org/pdf/2603.15255)

- SAGE 的核心在于将一个共享的 LLM 底座（Backbone）实例化为四个各司其职的专用智能体（Agents）。这四个智能体通过协同和竞争，形成了一个无需人工干预的“训练闭环”： 挑战者 (Challenger)：负责生成任务。它会不断产生难度递增的题目（如数学题或代码需求），旨在挑战模型的边界。 规划者 (Planner)：负责制定策略。它将 Challenger 生成的任务分解为结构化的多步执行计划。 **求解者 (Solver)：负责执行。它根据 Planner 给出的计划生成最终答案，并通过外部验证器（如 Python 解释器或数学公式校验）来获取正确性反馈**。 **评判者 (Critic)：负责质量控制。**它对生成的题目和计划进行打分和筛选，过滤掉低质量或格式不符的数据，防止“课程漂移”（即生成的题目太简单或太难导致训练失效）。
## ⭐⭐AgentFactory: A Self-Evolving Framework Through Executable Subagent Accumulation and Reuse【Self-Evolving, BAAI】

[https://arxiv.org/pdf/2603.18000](https://arxiv.org/pdf/2603.18000)

- **agent在任务执行过程中无法有效积累和复用能力**的问题。提出 AgentFactory 框架，通过以下机制解决上述问题： 可执行子智能体积累：**将成功的任务解决方案保存为****可执行的 Python 代码（而非文本经验）****，形成可复用的子智能体库** 自主进化：基于执行反馈**持续改进子智能体代码**，使其随任务积累变得更加鲁棒和通用 跨系统复用：**标准化的代码和文档格式支持将成熟子智能体导出到任何 Python 环境或其他 AI 框架**（如 LangChain、AutoGen）中直接使用 通过三阶段生命周期（**Install → Self-Evolve → Deploy**），该系统实现了无需人工干预的持续能力积累，使解决后续类似任务所需的计算成本逐步降低。
## CoVerRL: Breaking the Consensus Trap in Label-Free Reasoning via Generator-Verifier Co-Evolution【Self-Evolving】

[https://arxiv.org/pdf/2603.17775](https://arxiv.org/pdf/2603.17775)

- 解决无标签强化学习（Label-Free Reinforcement Learning）中的共识陷阱（Consensus Trap）问题，即**基于多数投票（Majority Voting）的伪标签方法在训练过程中出现的系统性失效模式**。通过让单一模型在生成器（Generator）和验证器（Verifier）角色间交替，实现双向监督：**多数投票为验证器提供对比学习信号，而改进的验证器逐步从伪标签中过滤自洽错误**，形成良性循环以维持高奖励准确率。
## HyEvo: Self-Evolving Hybrid Agentic Workflows for Efficient Reasoning【Self-Evolving, Agent Workflow】

[https://arxiv.org/pdf/2603.19639](https://arxiv.org/pdf/2603.19639)

- 现有自动化工作流生成方法（如ADAS、AFlow等）主要采用**编排范式（orchestration paradigm）**，即由元智能体（meta-agent）从预定义的算子库（predefined operator libraries）中选择算子或配置LLM参数。然而，这种算子库本身仍需人工手工设计，限制了方法的完全自动化能力，并将搜索空间约束在高级模块的组合上，阻碍了对细粒度交互逻辑和新型原子结构的探索。
## Hyperagents【Agent Workflow, Meta】

[https://arxiv.org/pdf/2603.19461](https://arxiv.org/pdf/2603.19461)

- 提出超智能体（HyperAgents）——将**任务智能体（解决具体任务）**和**元智能体（生成改进方案）**整合为单一可编辑程序。关键创新是元认知自我修改（Metacognitive Self-Modification）： 元智能体不仅能修改任务解决逻辑，还能修改自身的改进机制 使用图灵完备的Python实现，理论上支持任何可计算任务的自我改进 通过维护**档案（Archive）**实现开放式探索，避免早熟收敛 基于此框架构建的 DGM-Hyperagents (DGM-H) 消除了对领域特定对齐的依赖。
## OSExpert: Computer-Use Agents Learning Professional Skills via Exploration【GUI, Skill】

[https://arxiv.org/pdf/2603.07978](https://arxiv.org/pdf/2603.07978)

- 当前基于大规模多模态模型的CUAs（如Agent-S3、OpenCUA）在日常简单任务上表现接近人类，但在操作专业软件（GIMP、LibreOffice、Tableau等）时存在显著缺陷：长程任务成功率低：复杂工作流成功率从近人类水平骤降至<10%泛化能力弱：对未见过的创造性UI设计（自定义布局、图标工具栏）成功率仅0%-10%细粒度控制缺失：无法可靠执行像素级精确操作（文本选择、拖拽、角度旋转）推理效率低下：依赖逐步规划和盲目试错，延迟比人类专家高5-50倍。核心方法：OSExpert框架 论文提出**环境学习（Environment Learning）**范式，使代理无需额外人工努力即可自主构建技能集。
## SecAgent: Efficient Mobile GUI Agent with Semantic Context【GUI, Alibaba】

[https://arxiv.org/pdf/2603.08533](https://arxiv.org/pdf/2603.08533)

- 非英语生态数据稀缺：现有工作主要集中于英语环境，缺乏针对中文等非英语生态系统的高质量、人工验证的多语言数据集，制约了多语言GUI代理的发展。历史信息表示效率低下：多步导航任务中，完整历史截图带来计算开销过大（每张截图编码为数千视觉token），而截断历史窗口或仅保留动作序列则导致关键信息丢失。论文构建了CMGUI数据集及配套评估基准CMGUI-Bench： CMGUI数据集：包含18,887个grounding样本和29,711条导航轨迹（总计121,265个步骤），覆盖44个主流中文应用（如淘宝、拼多多、美团、B站等）。所有导航步骤均经过人工验证和边界框标注，确保动作正确性和定位精度。 CMGUI-Bench：包含390个完整 episodes（2,574个步骤），采用多选题动作标注机制，为每个步骤标注多个有效动作选项，以准确评估代理在存在多种合理操作路径时的鲁棒性。论文提出**SecAgent**，一个高效的3B规模移动GUI代理，其核心创新为**语义上下文机制（Semantic Context Mechanism）**：不同于传统方法直接堆叠历史截图：SecAgent通过自然语言摘要压缩历史信息。
## Generalization in Online Reinforcement Learning for Mobile Agents【GUI】

[https://arxiv.org/pdf/2603.07432](https://arxiv.org/pdf/2603.07432)

- 解决**图形用户界面（GUI）移动代理在在线强化学习（RL）中的泛化问题**，并针对该领域缺乏标准化评估基准与开源训练系统的现状，提出了系统性的解决方案。将问题形式化为上下文马尔可夫决策过程（Contextual MDP, CMDP），并引入三个渐进的泛化场景： Unseen Instance：零样本迁移到同一任务模板下的新实例； Unseen Template：迁移到同一应用下的全新任务模板； Unseen Application：迁移到全新的、训练时未见的应用。
## Hybrid Self-evolving Structured Memory for GUI Agents【GUI, Memory】

[https://arxiv.org/pdf/2603.10291](https://arxiv.org/pdf/2603.10291)

- HYMEM 通过以下三层设计解决上述问题： 1. 混合图结构记忆；2. 全局自演化机制；3. 局部工作记忆刷新。
## HATS: Hardness-Aware Trajectory Synthesis for GUI Agents【GUI】

[https://arxiv.org/pdf/2603.12138](https://arxiv.org/pdf/2603.12138)

- **VLMs的GUI代理在自动化轨迹合成过程中面临的语义模糊性问题。**论文提出HATS（Hardness-Aware Trajectory Synthesis）框架，通过闭环机制协同优化探索与验证： 硬度驱动探索：将语义模糊度量化为"硬度"（hardness），通过改进的蒙特卡洛树搜索（HD-MCTS）主动引导探索向高价值、欠表征的语义复杂区域 对齐引导细化：通过多轮重放-验证-修复循环，确保合成指令与执行轨迹的语义一致性，仅保留通过验证的轨迹用于训练 该框架旨在生成兼具多样性（覆盖语义模糊交互）与保真度（指令-执行严格对齐）的高质量轨迹数据，从而提升GUI代理在真实复杂界面中的鲁棒性与泛化能力。
## Video-Based Reward Modeling for Computer-Use Agents【GUI】

[https://arxiv.org/pdf/2603.10178](https://arxiv.org/pdf/2603.10178)

- 解决**计算机使用代理（Computer-Use Agents, CUAs）的自动化、可扩展评估问题。**论文提出通过**执行视频**（代理操作界面的关键帧序列）进行方法无关的评估。
## GUI-CEval: A Hierarchical and Comprehensive Chinese Benchmark for Mobile GUI Agents【GUI, Xiaomi】

[https://arxiv.org/pdf/2603.15039](https://arxiv.org/pdf/2603.15039)

- 论文提出了 GUI-CEval——**首个专为中文移动GUI代理设计的综合基准测试**，其核心贡献包括： 覆盖201个主流中文应用，基于真实物理设备环境构建； 分层评估框架：通过"基础任务"（原子能力诊断）与"应用任务"（端到端执行）相结合，系统评估感知、规划、反思、执行、评估五个维度； 人工验证的数据管道：所有数据均通过真实设备演示和多阶段人工审核，确保交互上下文的真实性与可复现性。
## AdaZoom-GUI: Adaptive Zoom-based GUI Grounding with Instruction Refinement【GUI, Lenovo】

[https://arxiv.org/pdf/2603.17441](https://arxiv.org/pdf/2603.17441)

- 论文提出了AdaZoom-GUI框架，通过以下机制应对挑战： 指令细化模块：**将模糊指令改写为包含视觉特征和位置信息的明确描述，解耦指令理解与精确定位** 条件放大策略：基于预测的边界框尺寸**动态决定是否执行第二阶段推理，仅在元素较小时触发放大**，平衡计算效率与定位精度 联合预测训练：使用GRPO训练模型同时输出点击坐标和边界框，为条件放大提供尺寸判断依据。
## Adaptive Vision-Language Model Routing for Computer Use Agents【GUI】

[https://arxiv.org/pdf/2603.12823](https://arxiv.org/pdf/2603.12823)

- 解决**计算机使用代理（Computer Use Agents, CUAs）在推理成本与动作难度不匹配方面的核心矛盾**。论文提出Adaptive VLM Routing (AVR) 框架，通过**引入轻量级语义路由层，将每个工具调用动态分配至满足目标可靠性阈值的最廉价模型**。该框架整合了三重机制： 基于多模态嵌入（SigLIP + MiniLM）的难度分类 基于logprob探测的置信度路由 针对warm agent的记忆补偿路由 理论上，该系统可在保持与全大模型基线相差2个百分点以内的准确率的同时，实现最高78%的推理成本降低。
## OS-Themis: A Scalable Critic Framework for Generalist GUI Rewards【GUI】

[https://arxiv.org/pdf/2603.19191](https://arxiv.org/pdf/2603.19191)

- 面向通用GUI智能体的可扩展多Agent批评框架，旨在解决RL训练中奖励信号的质量瓶颈。通过**里程碑分解**（将长程轨迹转化为可验证的离散里程碑）**与证据链审计**（审查里程碑集的完整性、适当性及证据强度，识别被掩盖的关键失败），解决上下文丢失与证据稀释难题。
## UI-Voyager: A Self-Evolving GUI Agent Learning via Failed Experience【GUI, Tencent Hunyuan】

[https://arxiv.org/pdf/2603.24533](https://arxiv.org/pdf/2603.24533)

- 论文提出了 UI-Voyager，一个两阶段的自进化（self-evolving）训练框架： 第一阶段（Rejection Fine-Tuning, RFT）：**通过拒绝采样机制自动筛选高质量轨迹，实现数据与模型的协同进化**，无需昂贵的人工标注； 第二阶段（Group Relative Self-Distillation, GRSD）：**通过识别群组 rollout 中的关键分叉点（fork points），将成功轨迹中的正确动作蒸馏到失败轨迹中，构建密集的步骤级监督信号**（dense step-level supervision），从而替代稀疏的轨迹级奖励，有效解决信用分配问题。 该框架使仅含4B参数的模型在AndroidWorld基准上达到81.0%的Pass@1成功率，超越了人类水平（80.0%）及众多更大规模的基线模型。
## CUA-Suite: Massive Human-annotated Video Demonstrations for Computer-Use Agents【GUI】

[https://arxiv.org/pdf/2603.24440](https://arxiv.org/pdf/2603.24440)

- 试图解决计算机使用代理（Computer-Use Agents, CUAs）在通用桌面自动化任务中面临的高质量训练数据稀缺问题，特别是针对专业桌面应用场景的连续视频演示数据严重不足这一核心瓶颈。为应对上述挑战，论文提出了CUA-SUITE生态系统，核心包括：**VIDEOCUA：约55小时、600万帧的30fps连续专家视频演示，涵盖87个桌面应用的10,000个任务，包含运动学光标轨迹和多层次推理注释 **GROUNDCUA：56K张密集注释截图，包含360万个UI元素边界框标注，用于解决视觉落地瓶颈 UI-VISION：针对桌面环境的严格评估基准，用于诊断代理在视觉感知、布局理解和动作预测方面的失败模式通过提供可无损转换为任何现有代理框架格式（截图-动作对、状态-动作-下一状态三元组等）的连续视频流，CUA-SUITE旨在突破当前CUAs在专业桌面自动化任务中的数据瓶颈。
## GUIDE: A Benchmark for Understanding and Assisting Users in Open-Ended GUI Tasks【GUI】

[https://arxiv.org/pdf/2603.25864](https://arxiv.org/pdf/2603.25864)

- **现有GUI（图形用户界面）代理过度关注任务自动化而****忽视对用户意图和行为理解**的问题，特别是在**开放式、探索性工作流**中如何实现有效的人机协作。文提出了**GUIDE（GUI User Intent Detection Evaluation）**基准测试，通过三阶段评估框架来推进"从自动化走向协作"的范式转变： 行为状态检测：识别用户当前所处的认知与行为阶段（如探索与决策、调试、沮丧等九种状态） 意图预测：推断用户在当前时刻的短期目标 帮助预测：判断用户是否需要帮助，以及需要何种类型的帮助（如功能解释、替代建议或错误处理） 该基准包含67.5小时的屏幕录制数据，来自120名新手用户在10种软件中的开放式任务演示，旨在评估多模态大语言模型（MLLMs）基于纯视觉输入理解用户、推断意图并提供情境感知协助的能力。
## OpenComputer: Verifiable Software Worlds for Computer-Use Agents【GUI】

[https://arxiv.org/pdf/2605.19769](https://arxiv.org/pdf/2605.19769)

- 论文提出OpenComputer——一个**以验证器为基础（verifier-grounded）的框架**，通过以下方式重构环境构建流程：将验证作为组织原则：在环境和任务构建之初就确保结果可通过程序化方式检查，而非将验证视为下游评估细节；自动化可验证世界合成：通过应用特定的状态验证器、自进化验证层、任务生成管道和评估工具，自动生成可执行、机器可检查的桌面任务实例；提供可审计的部分信用奖励：基于对实际应用状态的程序化检查计算奖励，而非依赖视觉代理或LLM判断。该框架旨在实现无需人工构建即可生成大规模、真实、可重现的软件世界，同时确保评估的可靠性和可审计性。
## OmniGUI: Benchmarking GUI Agents in Omni-Modal Smartphone Environments【GUI】

[https://arxiv.org/pdf/2605.18758](https://arxiv.org/pdf/2605.18758)

- 解决**现有GUI（图形用户界面）代理基准测试与实际智能手机交互环境之间的模态鸿沟**问题。论文提出了 **OmniGUI**，这是**首个在步骤级别（step-level）为GUI代理提供连续、交错的全模态输入（静态图像、同步音频、时间视频片段）的智能手机环境基准测试**，旨在系统性地评估代理在真实全模态交互环境中的感知-行动（perception-to-action）能力。采用步骤级教师强制协议（teacher-forcing），使用基础全模态模型（Gemini 3.0 Pro、Qwen3-Omni等）作为代理代理进行基线评估： 性能基准：最优模型（Gemini 3.0 Pro）Exact Match（EM）为66.4%，Success Rate（SR）仅33.1%，表明**处理瞬态多模态信号仍是重大挑战**。 维度差异：静态定位任务表现最佳（EM 79.9%），而跨模态判别（59.9%）和时间推理（61.8%）显著较低。 关键瓶颈： 跨模态干扰：在AV-Present任务中，引入无关环境音视频反而导致性能下降，表明模型难以过滤任务无关信号。 双音频流处理：将文本指令替换为TTS语音时，AV-Critical任务性能显著下降（-5.8% EM），揭示模型难以同时处理用户语音指令和环境音频线索。 听觉忽视与空间定位失败：定性分析显示模型常忽略关键音频事件，或在多模态理解正确时仍无法精确定位空间坐标。
## **Can RL Improve Generalization of LLM Agents? An Empirical Study**

[https://arxiv.org/pdf/2603.12011](https://arxiv.org/pdf/2603.12011)

- **RFT训练的LLM智能体，其性能改进能否有效泛化到训练分布之外的真实世界场景？**现有RFT研究多在**领域内**（in-domain）进行评估——训练与测试共享相同环境与任务。然而，真实部署中智能体常面临**未见环境**（unseen environments），其背景知识、观察空间（observation spaces）与动作接口（action interfaces）均存在显著偏移。结论RFT能够显著增强LLM智能体的环境内泛化与交互效率，但其跨环境迁移能力受限于语义先验差异与观察/动作接口变化。顺序多环境训练是实现通用智能体的可行路径，能在有效迁移的同时最小化遗忘。研究为开发可实际部署的泛化型LLM智能体提供了实证基础与实践指导。
## AgentProcessBench: Diagnosing Step-Level Process Quality in Tool-Using Agents

[https://arxiv.org/pdf/2603.14465](https://arxiv.org/pdf/2603.14465)

- 论文提出了 **AgentProcessBench**，这是首个专门用于**诊断工具使用轨迹中“步骤级”质量**的基准测试。包含 1,000 条多样化的任务轨迹，收集了 8,509 个由人类专家标注的步骤。每个步骤不只是简单的“对”或“错”，而是根据其对最终目标的贡献被分类。
## Internalizing Agency from Reflective Experience

[https://arxiv.org/pdf/2603.16843](https://arxiv.org/pdf/2603.16843)

- **基于LLM的自主智能体在长程交互环境中训练时，现有结果驱动型后训练方法（如基于可验证奖励的强化学习，RLVR）无法有效利用环境反馈、导致分布锐化（distribution sharpening）的问题**。论文提出LEAFE（Learning Feedback-Grounded Agency from Reflective Experience）框架，旨在通过反思经验将恢复能力内化为模型的内在属性，从而将环境反馈转化为可操作的监督信号，提升长程交互任务中的问题解决能力和探索效率。在交互中周期性触发反思（每K步或失败时），智能体诊断历史轨迹，生成回滚目标τ（关键错误步骤）和经验摘要e（诊断与修复建议）,通过BFS策略构建回滚树：重置环境至τ，重放历史，并在经验e条件下生成修正动作a′τ进行分支探索，收集**"失败→回滚→修复→成功"**的反思轨迹。
## CUBE: A Standard for Unifying Agent Benchmarks【Agent Benchmark, IBM】

[https://arxiv.org/pdf/2603.15798](https://arxiv.org/pdf/2603.15798)

- 解决AI Agent基准测试领域严重碎片化问题的协议标准。CUBE的核心主张是**"一次包装，处处使用"**（wrap once, use everywhere）。通过**建立统一的API契约，任何兼容CUBE的基准可被任何兼容平台直接用于评估、RL训练或数据生成**，无需定制代码。
## AIDABench: AI Data Analytics Benchmark

[https://arxiv.org/pdf/2603.15636](https://arxiv.org/pdf/2603.15636)

- 解决**AI驱动的****文档分析工具在复杂数据分析任务****中缺乏全面、端到端评估标准**的问题。整体性能瓶颈：最佳模型（claude-sonnet-4-5）pass@1仅 59.43%，pass@3为 70.78%，表明复杂真实数据分析仍具挑战性 能力维度差异：数据可视化得分最高（top模型 > 78%），文件生成最具挑战性（最佳pass@1仅49.43%） 模型规模效应：小模型（如30B参数）在文件生成上性能显著落后（< 32% pass@3） 消融研究：移除辅助电子表格摘要后，8/11模型性能下降，小模型受影响更大（最高降10.37分）
## The PokeAgent Challenge: Competitive and Long-Context Learning at Scale

[https://arxiv.org/pdf/2603.15563](https://arxiv.org/pdf/2603.15563)

- 先前的工作（如Claude、Gemini、GPT玩宝可梦）使用了不同的游戏版本（红、蓝、水晶、绿宝石）、不同的工具链和不同的评估标准，导致无法有意义地比较性能。成功无法归因于智能体架构、底层模型能力，还是简化感知的硬编码假设。 论文通过建立双赛道标准化评估框架解决此问题： 对战赛道（Battling Track）：基于宝可梦Showdown，评估在部分可观测条件下的战略推理和泛化能力 速通赛道（Speedrunning Track）：基于宝可梦绿宝石，评估长程规划和顺序决策能力，要求在规定时间内完成游戏。
## OpenClaw-RL: Train Any Agent Simply by Talking【Claw】

[https://arxiv.org/pdf/2603.10165](https://arxiv.org/pdf/2603.10165)

- **Agent交互过程中产生的下一步状态信号（next-state signals）被浪费的问题**，即现有系统未能将这些信号作为实时、在线的强化学习来源加以利用。为解决上述问题，论文提出了OpenClaw-RL框架，其核心贡献包括：** 通过PRM（Process Reward Model）判断将评估信号恢复为密集标量过程奖励 **通过Hindsight-Guided On-Policy Distillation (OPD) **将指令信号恢复为token级别的方向性优势监督** 构建完全解耦的异步架构（策略服务、环境托管、PRM判断、策略训练四个独立循环），实现零协调开销的实时在线学习 统一支持个人智能体（对话）和通用智能体（终端、GUI、SWE、工具调用）的强化学习训练
## MetaClaw: Just Talk -- An Agent That Meta-Learns and Evolves in the Wild【Claw】

[https://arxiv.org/pdf/2603.17187](https://arxiv.org/pdf/2603.17187)

- **MetaClaw**，一个面向真实部署环境的持续元学习框架，使大型语言模型（LLM）代理能够在不间断服务的情况下自主进化。论文提出通过两种互补机制解决上述问题： **技能驱动的快速适应（Skill-driven fast adaptation）：梯度无关地从失败中合成行为指令，零停机立即生效** 机会性策略优化（Opportunistic policy optimization）：**通过机会性元学习调度器（OMLS）在用户空闲时触发基于RL的LoRA微调 **二者形成良性循环：更好的策略产生更有信息量的失败用于技能合成，更丰富的技能产生更高奖励的轨迹用于策略优化。
## A Subgoal-driven Framework for Improving Long-Horizon LLM Agents【Long Horizon, DeepMind】

[https://arxiv.org/pdf/2603.19685](https://arxiv.org/pdf/2603.19685)

- 针对长程LLM智能体在复杂网页导航任务中的规划能力缺陷，提出了一个统一的子目标驱动框架，通过显式里程碑机制同时增强在线推理与离线强化学习训练。论文提出了统一的子目标驱动框架： **在线规划机制：利用专有模型（如Gemini-2.5-pro）进行动态子目标分解与实时自我反思**，通过显式里程碑检查防止错误传播 MiRA训练框架（Milestoning your Reinforcement Learning Enhanced Agent）：**引入基于里程碑的密集奖励塑造（reward shaping），通过潜在评论家（Potential Critic）学习子目标完成进度，为长程任务提供连续的监督信号**
## ⭐AgentSwing: Adaptive Parallel Context Management Routing for Long-Horizon Web Agents【Long Horizon, Web, Tongyi】

[https://arxiv.org/pdf/2603.27490](https://arxiv.org/pdf/2603.27490)

- 长程信息搜寻智能体（long-horizon web agents）中有限上下文容量与深度探索需求之间的矛盾，提出了首个概率分析框架及自适应解决方案AgentSwing。AgentSwing：自适应并行路由框架 基于上述视角，论文提出AgentSwing，包含两个核心机制：** (1) 并行上下文管理（Parallel Context Management）** 在触发点（上下文长度超过阈值r ）时，**并行应用多种异构策略生成候选分支**： Discard-All：仅保留原始用户提示q Keep-Last-N：保留最近N 轮交互历史 Summary：压缩为摘要形式(q,Sum) **(2) 前瞻路由机制（Lookahead Routing）** 对每个候选分支执行K 步环境交互的前瞻 rollout，基于实际下游行为（而非仅当前压缩结果）动态选择最优分支继续探索。剩余分支被剪枝，选中延续成为新主轨迹。
## Vision2Web: A Hierarchical Benchmark for Visual Website Development with Agent Verification【Web】

[https://arxiv.org/pdf/2603.26648](https://arxiv.org/pdf/2603.26648)

- **多模态编码智能体（multimodal coding agents）在复杂端到端网站开发任务中缺乏系统性、可靠评估基准**的问题。论文提出了**Vision2Web**基准测试，通过**三层级层次化任务设计**（静态网页生成→交互式前端开发→全栈网站构建）实现能力解耦，并引入**基于工作流的智能体验证范式**（workflow-based agent verification），结合GUI智能体验证器与VLM评判器，实现了对功能正确性与视觉保真度的可复现、实现无关的自动化评估。
## Meta-Harness: End-to-End Optimization of Model Harnesses【Harness】

[https://arxiv.org/pdf/2603.28052](https://arxiv.org/pdf/2603.28052)

- 论文提出 Meta-Harness，通过以下机制解决上述问题： 文件系统作为经验存储：将先前所有候选 harness 的源代码、评估分数和执行轨迹存入文件系统 代理式提议者（Agentic Proposer）：使用具备工具调用能力的编码代理（而非原始 LLM），通过 grep、cat 等标准操作选择性检索历史信息，而非将其打包为单一提示词 端到端代码空间搜索：在可执行代码空间中进行搜索，允许对检索逻辑、记忆更新和提示构造进行算法级修改 通过这种方式，Meta-Harness 实现了对 harness 代码的自动化、端到端优化，同时克服了传统文本优化方法因反馈压缩而导致的信用分配（credit assignment）困难。

# AI for Science

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science
- 方法：agent, reasoning
- 论文/报告：5 篇
- Mozi: Governed Autonomy for Drug Discovery LLM Agents
- Surg-R1: A Hierarchical Reasoning Foundation Model for Scalable and Interpretable Surgical Decision Support with Multi-Center Clinical Validation
- CHIMERA-Bench: A Benchmark Dataset for Epitope-Specific Antibody Design
- 📌Lingshu-Cell: A generative cellular world model for transcriptome modeling toward virtual cells【Damo Academy】
- Self-evolving AI agents for protein discovery and directed evolution【Liang Hong】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Mozi: Governed Autonomy for Drug Discovery LLM Agents

[https://arxiv.org/pdf/2603.03655](https://arxiv.org/pdf/2603.03655)

- 通用LLM智能体虽可作为认知编排器，却面临两大根本障碍：**无约束工具使用治理****（现有智能体缺乏严格的工具访问控制，易产生工具使用幻觉（tool-use hallucinations），生成科学无效或语法错误的参数，且无法满足受监管环境对审计性与安全性的要求）和****长程可靠性不足****（**在多阶段、深度依赖的制药流程中，早期阶段的微小错误（如选择错误的蛋白质亚型）会通过状态依赖关系**乘法累积**（multiplicatively compound），导致下游计算 scientifically invalid，造成昂贵的GPU资源浪费**）。**
## Surg-R1: A Hierarchical Reasoning Foundation Model for Scalable and Interpretable Surgical Decision Support with Multi-Center Clinical Validation

[https://arxiv.org/pdf/2603.12430](https://arxiv.org/pdf/2603.12430)

- 主要贡献 架构创新：**首个面向手术领域的分层推理VLM**，通过强制物理验证（Level 2）与临床情境综合（Level 3）解决"绑定问题"； 数据规模：**构建最大手术推理数据集Surg-R1-Data（32万CoT对）**，提出可扩展的手术感知CoT合成流程； 训练范式：结合GRPO与迭代自提升，证明**无需密集人工标注即可培养领域特定推理能力**； 临床验证：在真实多中心临床环境中验证跨机构鲁棒性，为手术AI的安全部署提供实证基础。
## CHIMERA-Bench: A Benchmark Dataset for Epitope-Specific Antibody Design

[https://arxiv.org/pdf/2603.13431](https://arxiv.org/pdf/2603.13431)

- 论文提出了CHIMERA-BENCH，通过以下方式解决上述问题： 定义表位条件的CDR序列-结构协同设计作为规范任务，统一文献中的七个子任务； 提供包含2,922 个抗体-抗原复合物的精选去重数据集，附带表位/互补位注释； 设计三种生物学 motivated 的数据划分（表位组、抗原折叠、时间序列），分别测试对未见表位、未见抗原拓扑结构和前瞻性目标的泛化能力； 建立包含五类指标的综合评估协议，包括新颖的表位特异性度量（epitope precision/recall/F1）。
## 📌Lingshu-Cell: A generative cellular world model for transcriptome modeling toward virtual cells【Damo Academy】

[https://arxiv.org/pdf/2603.25240](https://arxiv.org/pdf/2603.25240)

- 解决**单细胞转录组学中缺乏能够显式建模细胞状态分布并支持条件模拟的生成式世界模型**这一核心问题。构建细胞世界模型（Cellular World Model） 论文提出需要一个统一的框架，能够： **显式表示转录组状态空间：捕获约18,000个基因的全转录组表达分布，无需预先筛选高变异基因或按表达水平排序。** 支持条件模拟：**联合嵌入细胞类型/供体身份与扰动信息，预测未见过的细胞类型-扰动组合的全转录组响应**。 跨模态泛化：同时处理遗传扰动（CRISPR）和细胞因子刺激等不同扰动类型，在细胞系和原代细胞（如PBMC）中均实现准确预测。 通过提出Lingshu-Cell（一种掩码离散扩散模型），论文建立了首个能够进行高质量无条件细胞状态生成和精确条件扰动响应预测的统一生成式框架，为"虚拟细胞"（virtual cells）的计算建模和体外（in silico）扰动筛选提供了新的范式基础。
## Self-evolving AI agents for protein discovery and directed evolution【Liang Hong】

[https://arxiv.org/pdf/2603.27303](https://arxiv.org/pdf/2603.27303)

- 论文提出了VenusFactory2框架，其核心创新包括： **动态工作流合成：通过"首席研究员(PI)-机器学习专家-计算生物学家-科学评论家"的多智能体协作，将抽象的自然语言目标转化为可执行的实验计划**，实现从静态工具调用到意图驱动的自主编排的范式转变。 自进化能力扩展：**当检测到方法缺口（如缺乏特定预测器）时，系统可自主聚合训练数据、微调基础模型（如ESM2），并将新模型封装为可复用的MCP（Model Context Protocol）服务，无需人工干预即可扩展工具库**。 端到端自主执行：从单一自然语言提示（如"发现热稳定性更高的PET水解酶"）出发，**自主完成文献调研、数据库检索、多参数筛选（熔点、pH稳定性）、定向进化及实验设计**，在VenusAgentEval基准测试中Project级任务准确率达78.8%，显著优于通用模型和领域基线。
