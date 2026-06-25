---
source: https://luminous-mat-781.notion.site/Daily-Note-January-2026-2dee8c0e89c580eaaa21d51e20af067f?source=copy_link
---

# Daily Note - Jan 2026

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Daily Note - Jan 2026
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, science-discovery, vision
- 论文/报告：38 篇
- UniCorn: Towards Self-Improving Unified Multimodal Models through Self-Generated Supervision【Unified Model】
- Unified Thinker: A General Reasoning Modular Core for Image Generation【Unified Model】
- **Omni-R1: Towards the Unified Generative Paradigm for Multimodal Reasoning**【Unified Model】
- OpenVision 3: A Family of Unified Visual Encoder for Both Understanding and Generation【Unified Model】
- PyraTok: Language-Aligned Pyramidal Tokenizer for Video Understanding and Generation【Unified Model】
- VisGym: Diverse, Customizable, Scalable Environments for Multimodal Agents【Unified Model】
- Visual Generation Unlocks Human-Like Reasoning through Multimodal World Models【Unified Model, ByteDance Seed】
- ⭐UEval: A Benchmark for Unified Multimodal Generation【Unified Model, Zhuang Liu】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## UniCorn: Towards Self-Improving Unified Multimodal Models through Self-Generated Supervision【Unified Model】

[https://arxiv.org/pdf/2601.03193](https://arxiv.org/pdf/2601.03193)

- UMM存在 **Conduction Aphasia**：理解强、生成弱，内部知识无法转化为忠实图像。提出 UniCorn 后训练框架，同一套参数自演三种角色： Proposer → 生成 5 k 高多样性提示； Solver → 每提示 8 rollout 产图； Judge → 0-10 评分+理由，≥7 分保留。
## Unified Thinker: A General Reasoning Modular Core for Image Generation【Unified Model】

[https://arxiv.org/pdf/2601.03127](https://arxiv.org/pdf/2601.03127)

- **Unified Thinker**：一个与具体生成器解耦、可复用、可升级的通用推理核心，通过显式“结构化计划 → 像素级反馈”闭环，实现逻辑正确且可执行的图像生成与编辑。
## **Omni-R1: Towards the Unified Generative Paradigm for Multimodal Reasoning**【Unified Model】

[https://arxiv.org/pdf/2601.09536](https://arxiv.org/pdf/2601.09536)

- 提出“统一生成式多模态推理”新范式，用**一个模型**、**一套自回归生成接口**完成**放大、框选、标记、辅助线、视觉预测**等多样化推理动作，解决功能图像生成难与交错标注昂贵两大瓶颈。
## OpenVision 3: A Family of Unified Visual Encoder for Both Understanding and Generation【Unified Model】

[https://arxiv.org/pdf/2601.15369](https://arxiv.org/pdf/2601.15369)

- OpenVision 3 的目标是用**单一连续视觉编码器**同时支撑理解与生成，在共享隐空间中一次性提取“既能重建像素、又能对齐语义”的统一表征。冻结 FLUX.1-VAE → 轻量 ViT 得统一 token zu → 两条解耦分支。
## PyraTok: Language-Aligned Pyramidal Tokenizer for Video Understanding and Generation【Unified Model】

[https://arxiv.org/pdf/2601.16210](https://arxiv.org/pdf/2601.16210)

- 提出 PyraTok，三大核心 Language-aligned Pyramidal Quantization (LaPQ)：在编码器多深度横向插入量化块，共享 48K 二进制码本，逐层由文本嵌入引导，实现“粗→细”离散化。 双重语义对齐： – 局部：层次码本损失同时约束视觉承诺、跨层一致、token-文本、码字-文本四项，防止语义漂移。 – 全局：将各层 token 拼接为序列，用 VLM 自回归预测，强化语言-视觉整体连贯。 冻结 VAE + LoRA 适配：仅训练低秩适配器与量化模块，加漂移正则，保证高保真重建且参数高效。PyraTok 用金字塔大码本+全程语言绑定，一次性解决离散视频表征的保真、语义与泛化问题，成为通用视频-语言系统的新基座。
## VisGym: Diverse, Customizable, Scalable Environments for Multimodal Agents【Unified Model】

[https://arxiv.org/pdf/2601.16973](https://arxiv.org/pdf/2601.16973)

- 作者提出 VisGym——一个包含 17 个长周期视觉交互环境的统一评测与训练平台，通过可定制的难度、观测形式、反馈机制与专家求解器，实现： 对前沿模型进行跨领域、控制变量的失败诊断；** 生成高质量多步演示数据，支持监督微调**； 量化揭示 VLMs 在长上下文利用、视觉-语言对齐、部分可观测与未知动力学场景下的系统性缺陷。
## Visual Generation Unlocks Human-Like Reasoning through Multimodal World Models【Unified Model, ByteDance Seed】

[https://arxiv.org/pdf/2601.19834](https://arxiv.org/pdf/2601.19834)

- 在UMM中，视觉生成能力究竟在何时、以何种方式真正提升推理性能？对于** grounded in the physical world** 的任务，**视觉生成作为世界模型比纯语言世界模型更具信息量和先验知识，从而显著提升推理表现**；**反之，若任务状态简单或无需精细视觉模拟，则视觉生成无额外收益**。
## ⭐UEval: A Benchmark for Unified Multimodal Generation【Unified Model, Zhuang Liu】

[https://arxiv.org/pdf/2601.22155](https://arxiv.org/pdf/2601.22155)

- 现有基准仅分别评测 VQA（图→文）或 T2I（文→图），忽视真实场景需联合输出图文。UEval 填补该空白。1 000 道专家筛选题，覆盖 8 类任务：空间、教科书、图表、论文（闭式）；艺术、生活、技术、运动（开式）。 每题配参考图像+文本，人工校验。数据相关 rubric：Gemini-2.5-Pro 针对每题自动生成十余条细则→人工精修，共 10 417 条。
## TangramPuzzle: Evaluating Multimodal Large Language Models with Compositional Spatial Reasoning

[https://arxiv.org/pdf/2601.16520](https://arxiv.org/pdf/2601.16520)

- 七巧板。
## Recursive Language Models【MIT】

[https://arxiv.org/pdf/2512.24601](https://arxiv.org/pdf/2512.24601)

- 作者提出 Recursive Language Models (RLMs)——一种推理时（inference-time）通用框架，把超长 prompt 视为外部环境变量，让 LLM 在 Python REPL 里用代码“窥视、分解、递归调用自身”处理片段。
## Modeling Language as a Sequence of Thoughts【Stanford】

[https://arxiv.org/pdf/2512.25026](https://arxiv.org/pdf/2512.25026)

- 作者提出 Thought Gestalt（TG）模型，把语言同时建模为两个抽象层级： token 层级：照常生成当前句的每个词； sentence“thought”层级：将整句压缩成单一向量写入可微外部记忆，后续句通过交叉注意力读取。TG 在相同参数/数据预算下持续优于 GPT-2，且对关系方向泛化（父子反转）更鲁棒。
## ⭐Encyclo-K: Evaluating LLMs with Dynamically Composed Knowledge Statements【ByteDance Seed】

[https://arxiv.org/pdf/2512.24867](https://arxiv.org/pdf/2512.24867)

- 针对现有大模型评测基准的三大痛点——数据污染脆弱、只能测单知识点、依赖昂贵专家标注——提出“知识陈述级”而非“题目级”构建思路。Encyclo-K 通过“权威教材抽取陈述→模型自动生成干扰陈述→测试时随机组合成题”的动态 pipeline，将上述三点转化为可工程化落地的方案，从而建立可周期性刷新、多知识点综合、无需专家深度参与的新一代评测框架。
## Scaling Open-Ended Reasoning to Predict the Future

[https://arxiv.org/pdf/2512.25070](https://arxiv.org/pdf/2512.25070)

- 如何大规模训练语言模型，使其能够对开放式未来事件进行高质量概率预测？作者提出一套完全自动化的数据合成与过滤流程，将静态新闻语料转化为约 5 万条开放式短答案预测题（OpenForesight），并设计结合准确率与校准度的奖励函数，用 GRPO 强化学习微调 8 B 模型。
## State-of-the-art Small Language Coder Model: Mify-Coder【Code】

[https://arxiv.org/pdf/2512.23747](https://arxiv.org/pdf/2512.23747)

- **在训练策略被严格工程化、数据质量被充分优化、安全对齐被前置的前提下，仅 2.5B 参数的“小”模型能否在代码生成与函数调用等关键任务上达到甚至超越数十亿乃至上百亿参数级“大”模型的前沿性能，同时显著降低训练与部署成本？**
## **X-Coder: Advancing Competitive Programming with Fully Synthetic Tasks, Solutions, and Tests**【Code】

[https://arxiv.org/pdf/2601.06953](https://arxiv.org/pdf/2601.06953)

- 首次证明“**完全不依赖真实代码数据，仅靠合成任务-解法-测试三元组**”即可在竞技编程基准上取得 SOTA 性能，并给出可复现的 SynthSmith 管线与 SFT→RL 训练配方。
## KOCO-BENCH: Can Large Language Models Leverage Domain Knowledge in Software Development?【Code】

[https://arxiv.org/pdf/2601.13240](https://arxiv.org/pdf/2601.13240)

- 解决“大模型在领域化软件开发中如何有效获取并运用全新领域知识”这一核心问题。现有领域专用代码评测集仅检验模型已掌握的知识，却无法衡量模型“学习新知识”的能力，也缺乏可供训练或检索的显式知识库。为此，作者提出 KOCO-BENCH： **首次将可学习的知识语料（框架文档、源码、示例）与评测任务（代码生成、知识理解）成对绑定**。可学习知识语料（框架文档+源码+示例，77 k–400 k 行/域）与评测任务成对绑定的基准，覆盖 6 新兴域、11 框架、25 真实项目；任务分两级： ① **领域代码生成**（函数→模块→项目三级需求 + 单元/集成双测） ② **领域知识理解**（单/多选 Q&A）
## DevBench: A Realistic, Developer-Informed Benchmark for Code Generation Models【Code】

[https://arxiv.org/pdf/2601.11895](https://arxiv.org/pdf/2601.11895)

- 既有代码生成基准依赖公开仓库或竞赛题，任务与真实 IDE 补全场景脱节，诊断粒度粗，且易受训练数据污染。提出 DevBench——基于十亿级开发者遥测、人工校验的合成基准，涵盖 6 语言 × 6 高频高难任务类别共 1,800 实例，采用前缀-补全-后缀结构，支持 FIM 与续写两种模式。Claude 4 Sonnet 功能最强（84.8%），但 judge 评分低于 GPT-4o TypeScript 普遍难，C++ 对 DeepSeek-V3 是短板。
## HE-SNR: Uncovering Latent Logic via Entropy for Guiding Mid-Training on SWE-BENCH【Code】

[https://arxiv.org/pdf/2601.20255](https://arxiv.org/pdf/2601.20255)

- 如何在 mid-training 阶段实时、可靠地评估大模型在复杂软件工程任务（SWE-BENCH）上的潜在能力?论文提出一套基于“熵压缩”理论的 mid-training 评估框架，核心贡献包括： 仅 500 条轨迹（≈12.5 M tokens）的 token 级过滤协议，直接对齐 SWE 分布，排除风格噪声。 揭示高熵 token 的分布存在“Shift to ln 3”现象，并据此给出 Entropy Compression Hypothesis：智能体现在将不确定性压缩到少量（k=2,3…）合理候选的“合理犹豫”状态，而非单纯压低标量损失。 提出 HE-SNR 指标，在 High-Entropy Decision Set 上计算信噪比，对 Long-Context Tax 免疫，与下游 SWE-BENCH 呈严格线性关系，可在 10× 参数规模跨度内保持排序一致性。 解释“Alignment Tax”来源：SFT 在提升全局 PPL 的同时，反而损害高熵决策点的预测质量，牺牲复杂推理以换取表层风格对齐。
## SWE-Spot: Building Small Repo-Experts with Repository-Centric Learning【Code】

[https://arxiv.org/pdf/2601.21649](https://arxiv.org/pdf/2601.21649)

- 提出**Repository-Centric Learning (RCL)** 新范式，**让 SLM 在训练阶段就把目标仓库的“物理规律”参数化地内化**，而非仅靠推理时昂贵的外部检索与试错。通过四单元“仓库中心经验”(RCX) 把静态代码库转化为可交互的学习信号，训练出仅 4B 参数的 SWE-SPOT 系列“仓库专家”，在多项软件工程任务上打破规模定律，**以 1/8 参数量超越 30B 级开源模型，并与商用轻量模型（GPT-4.1-mini、GPT-5-nano）打平或更优**，同时显著降低推理开销。
## Breaking Model Lock-in: Cost-Efficient Zero-Shot LLM Routing via a Universal Latent Space【Routing】

[https://arxiv.org/pdf/2601.06220](https://arxiv.org/pdf/2601.06220)

- 现有 LLM 路由框架一旦训练完成，就只能服务于固定的一组模型，新增模型必须重新训练整个系统，代价高昂且扩展性差。ZeroRouter 旨在打破这一瓶颈，实现： **零样本接入新模型——无需重新训练路由器** 统一、模型无关的查询难度表示——把“查询本身有多难”与“某模型有多强”解耦 轻量级 profiling——仅用极少锚点查询即可估计新模型的准确率、成本、延迟 多目标优化——在准确率、成本、延迟之间按用户策略动态权衡 一句话：让路由系统像“热插拔”一样随时吸纳新模型，同时保持高准确率、低成本和低延迟。**建立模型无关的通用潜空间，把“查询难度”与“模型能力”解耦；新模型只需在少量锚点查询上运行一次即可映射到该空间，实现零样本接入**。
## Beyond Gemini-3-Pro: Revisiting LLM Routing and Aggregation at Scale【Routing, AI Lab】

[https://arxiv.org/pdf/2601.01330](https://arxiv.org/pdf/2601.01330)

- 无限增大单体‘超级模型’是否是通往通用人工智能（AGI）的唯一路径？作者将视角从“单体 Scaling”转向“集体智能”，目标是用**一组开源大模型协同**的方式，在性能与成本双重维度上**超越当前最强的闭源单体模型 Gemini-3-Pro**。
## LLMRouterBench: A Massive Benchmark and Unified Framework for LLM Routing【Routing, AI Lab】

[https://arxiv.org/pdf/2601.07206](https://arxiv.org/pdf/2601.07206)

- 大模型数量爆炸，单模型难以在所有任务上称霸 → 需要动态路由（routing） 现实障碍：①多模型×多数据集实验成本极高；②各研究自建模型池/评测协议，结果无法横向比较。
## STEP3-VL-10B Technical Report【StepFun】

[https://arxiv.org/pdf/2601.09668](https://arxiv.org/pdf/2601.09668)

- 在**10B 参数**预算内打造可**媲美或超越 100B–200B 旗舰**的开源多模态大模型，打破“轻量=低效”固有假设。关键思路 预训练：**1.2 T token 统一全参数训练**，语言对齐 **Perception Encoder + Qwen3-8B 解码器**，一步到位建立视觉-语言内在协同。 后训练：**>1k 轮强化学习（RLVR + RLHF）持续拔高**；引入Parallel Coordinated Reasoning（PaCoRe），用 16 路并行提案-交叉验证-综合，把测试时算力直接转化为性能增益。
## SIN-Bench: Tracing Native Evidence Chains in Long-Context Multimodal Scientific Interleaved Literature【Scientific Reasoning】

[https://arxiv.org/pdf/2601.10108](https://arxiv.org/pdf/2601.10108)

- MLLM是否真的理解长篇幅科学文献？作者提出“海洋捞鱼”（Fish-in-the-Ocean，FITO）新范式，要求模型在原生科学文档的“海洋”中，显式构建跨模态证据链 E，再给出答案 A，即建模 P(A,E|D,Q) 而非简单的 P(A|D,Q) 。
## ARC Prize 2025: Technical Report

[https://arxiv.org/pdf/2601.10904](https://arxiv.org/pdf/2601.10904)

- 前三甲分别提出 7 M 参数零预训练递归网络、自改进演化程序合成、MDL 单任务梯度压缩方案，全部开源。**核心发现：refinement loop 成为主流 无论权重空间（测试时训练、零预训练小网络）还是符号空间（演化程序、链式思维自修正），均依赖“生成→验证→再改进”的迭代循环。**商业模型亦可通过应用层 harness 把准确率再提升 20+ %，但代价是成本成百倍增长。2026 将发布 ARC-AGI-3，首次引入交互式环境，考核探索、计划、记忆、目标获取与对齐，并正式将“学习效率/美元成本”纳入排行榜，以持续压缩“人类易解—AI 难解”区间，直至该区间归零即视为 AGI。
## FutureOmni: Evaluating Future Forecasting from Omni-Modal Context for Multimodal LLMs【Xipeng Qiu】

[https://arxiv.org/pdf/2601.13836](https://arxiv.org/pdf/2601.13836)

- 论文旨在解决现有MLLMs在全模态（omni-modal）未来事件预测任务上的能力空白。具体而言：已有基准主要聚焦回顾性理解（描述已发生事件），而忽视了从音频-视觉联合线索中预测未来事件的能力评估。音频模态在预测任务中被显著忽略，尽管它在现实场景（如自动驾驶中喇叭声提示危险）中至关重要。为此，作者提出FutureOmni，首个专门评测MLLMs在全模态上下文中进行未来事件预测的基准，要求模型完成跨模态因果与时序推理，并有效利用内部知识。
## FutureX-Pro: Extending Future Prediction to High-Value Vertical Domains【ByteDance Seed】

[https://arxiv.org/pdf/2601.12259](https://arxiv.org/pdf/2601.12259)

- 通用大模型智能体在开放域表现优异，但在**金融、零售、公共卫生、自然灾害**四大资本密集、安全关键场景是否仍具备工业级精度与权威数据扎根能力，尚缺系统评估。
## ⭐LLM-in-Sandbox Elicits General Agentic Intelligence【Self-Evolving】

[https://arxiv.org/pdf/2601.16206](https://arxiv.org/pdf/2601.16206)

- 如何在不依赖额外领域专用数据或任务特定微调的前提下，让LLM在数学、物理、化学、生物医学、长文本理解、指令遵循等非代码任务上释放出更强的通用智能？
## ⭐⭐Self-Distilled Reasoner: On-Policy Self-Distillation for Large Language Models【Self-Evolving, Meta】

[https://arxiv.org/pdf/2601.18734](https://arxiv.org/pdf/2601.18734)

- **让同一模型同时扮演“能看到标准答案的教师”与“只能看到题目的学生”，在学生自己生成的轨迹上逐 token 计算分布差异并只更新学生**，实现**无外部教师、在线、密集自蒸馏**。
## Teaching Models to Teach Themselves: Reasoning at the Edge of Learnability【Self-Evolving, Meta】

[https://arxiv.org/pdf/2601.18778](https://arxiv.org/pdf/2601.18778)

- 提出 SOAR 框架，用**双层元强化学习**让预训练大模型在“零初始成功率”的数学难题集上**自生成垫脚石课程**，从而突破稀疏奖励平台期。
## ⭐SAMTok: Representing Any Mask with Two Words【ByteDance】

[https://arxiv.org/pdf/2601.16093](https://arxiv.org/pdf/2601.16093)

- 如何**非侵入式**地为基座 MLLM（如 Qwen-VL）赋予像素级能力，使其**无需修改架构、无需定制损失**，**仅通过标准 next-token 预测与简单 RL 即可完成掩码理解与生成**？提出 SAMTok—— 把任意 2D 掩码双向压缩成 2 个离散文本 token（mask ↔ “新语言”）。 掩码理解与生成统一为纯文本对话格式，可直接用现有 LLM 训练框架（SFT + GRPO）。 首次实现仅用文本匹配奖励优化掩码生成，无需像素级 IoU 计算。
## ⭐Evolutionary Strategies lead to Catastrophic Forgetting in LLMs

[https://arxiv.org/pdf/2601.20861](https://arxiv.org/pdf/2601.20861)

- 大模型部署后需持续学习，但梯度类后训练（SFT、RLHF、GRPO）内存开销大；进化策略（ES）作为梯度无参替代方案近期被重新关注，声称性能可比 GRPO 且极省内存，然而其是否会灾难性遗忘尚缺系统评估。在 Countdown、GSM8K、MATH、OlympiadBench 上，ES 峰值准确率仅落后 GRPO 3–4 个百分点，计算步数相近，确认其“性能可比”。 在同一微调 run 内，ES 新任务收敛后旧任务（HellaSwag）准确率仍持续下降约 10 %；GRPO 几乎无遗忘。 诊断表明 ES 更新 Frobenius 范数比 GRPO 大 1000×，稀疏度 < 20 %（GRPO ≈ 95 %），全局密集漂移导致旧能力被破坏。
## FrontierScience: Evaluating AI's Ability to Perform Expert-Level Scientific Tasks【OpenAI】

[https://arxiv.org/pdf/2601.21165](https://arxiv.org/pdf/2601.21165)

- 作者提出 FrontierScience 这一双轨基准： Olympiad 轨：由国际奥赛金牌/教练原创，考察封闭型、可验证的短答题（物理/化学/生物）。 Research 轨：由活跃于一线的 PhD/博士后/教授设计，模拟真实科研子任务，采用 10 分项细粒度 rubric 评估开放型推理过程。**Olympiad：GPT-5.2 77 %、Gemini-3-Pro 76 %，仍有 23 % 头 room。 Research：GPT-5.2 与 GPT-5 仅 25 %，开放科研任务难度高 3× 以上**。
## **PaddleOCR-VL-1.5: Towards a Multi-Task 0.9B VLM for Robust In-the-Wild Document Parsing****【OCR】**

[https://arxiv.org/pdf/2601.21957](https://arxiv.org/pdf/2601.21957)

- 提出 Real5-OmniDocBench——首个五类物理失真一对一标注基准。 设计 PP-DocLayoutV3——基于 RT-DETR 的实例分割+阅读顺序联合 Transformer，一次前向输出多边形掩码与逻辑顺序。 升级 PaddleOCR-VL-1.5-0.9B——保持 0.9 B 参数，新增印章识别与端到端 text spotting（4 点坐标 token 化），并引入 46 M 图文对预训练 + GRPO 强化难例微调。
## OCRVerse: Towards Holistic OCR in End-to-End Vision-Language Models**【OCR, Meituan】**

[https://arxiv.org/pdf/2601.21639](https://arxiv.org/pdf/2601.21639)

- 作者提出 OCRVerse——首个端到端“整体 OCR”方法，**通过统一架构与两阶段 SFT-RL 训练策略，在 4B 参数规模下同时实现： 字符级识别（文本、公式、表格）； 代码级表征（图表、网页、SVG、几何图、电路、分子结构）**； 从而将 OCR 技术从“分领域专用”推进到“全场景通用”。
## DeepSeek-OCR 2: Visual Causal Flow【OCR, DeepSeek】

[https://arxiv.org/pdf/2601.20552](https://arxiv.org/pdf/2601.20552)

- 现有 VLM 把二维图像硬拆成一维固定光栅顺序，与版面逻辑矛盾，导致复杂文档（表格、公式、多栏）解析错误。**提出 DeepEncoder V2，用 500 M decoder-only LLM 取代 CLIP**，形成“级联两个 1D 因果推理器”的新架构： 视觉 token 双向自注意，保持全局感知； 同等数量可学习因果查询采用三角掩码，逐步重排视觉信息； 仅把查询输出送入 3 B MoE LLM，完成后续自回归生成。
## Youtu-VL: Unleashing Visual Potential via Unified Vision-Language Supervision【Tencent Youtu】

[https://arxiv.org/pdf/2601.19798](https://arxiv.org/pdf/2601.19798)

- 通过 Vision-Language Unified Autoregressive Supervision (VLUAS) 范式，将优化目标从“vision-as-input”转变为“vision-as-target”，在统一词汇表上同时对视觉 token 与文本 token 进行自回归监督，使模型必须显式重建细粒度视觉内容。该方案在不引入任何任务特定模块的前提下，让标准 VLM 直接完成检测、分割、深度估计、姿态估计等视觉中心任务，实现通用多模态能力与密集感知能力的端到端统一。
## ⭐Innovator-VL: A Multimodal Large Language Model for Scientific Discovery【DP】

[https://arxiv.org/pdf/2601.19325](https://arxiv.org/pdf/2601.19325)

- 作者提出 Innovator-VL，通过**“高质量数据精选 + 全参数监督微调 + 面向推理的强化学习”**三阶段策略，在仅 5 M 科学样本、无大规模科学预训练的情况下，实现： 37 项通用、数学、科学基准上的 SOTA 或可比性能； 完全开源的数据、代码、超参数与评估脚本，确保端到端可复现； 显著更高的推理 token 效率，验证“小数据 + 精调”即可激发科学推理潜能。

# Reasoning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reasoning
- 方法：agent, generation, language, vision-language-model, reasoning, vision, reinforcement-learning, optimization
- 论文/报告：47 篇
- Fantastic Reasoning Behaviors and Where to Find Them: Unsupervised Discovery of the Reasoning Process【Google DeepMind】
- VIPER: Process-aware Evaluation for Generative Video Reasoning【Video Reasoning】
- Watching, Reasoning, and Searching: A Video Deep Research Benchmark on Open Web for Agentic Video Reasoning【Video Reasoning】
- VideoLoom: A Video Large Language Model for Joint Spatial-Temporal Understanding【Video Reasoning】
- Molmo2: Open Weights and Data for Vision-Language Models with Video Understanding and Grounding【Video Reasoning】
- Thinking in Frames: How Visual Context and Test-Time Scaling Empower Video Reasoning【Video Reasoning】
- GeoBench: Rethinking Multimodal Geometric Problem-Solving via Hierarchical Evaluation【Visual Reasoning, Junchi Yan】
- **Milestones over Outcome: Unlocking Geometric Reasoning with Sub-Goal Verifiable Reward**【Visual Reasoning, Junchi Yan】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Fantastic Reasoning Behaviors and Where to Find Them: Unsupervised Discovery of the Reasoning Process【Google DeepMind】

[https://arxiv.org/pdf/2512.23988](https://arxiv.org/pdf/2512.23988)

- 无监督地揭示并操控LLM在推理过程中的内部行为机制。作者提出 RISE 框架，通过**稀疏自编码器（SAE）在无标注的推理步骤激活上训练，自动发现“推理向量”——即激活空间中对应特定行为的线性方向，并验证这些向量可用于推理轨迹的因果干预**，从而同时实现**可解释发现**与**可控操纵**。
## VIPER: Process-aware Evaluation for Generative Video Reasoning【Video Reasoning】

[https://arxiv.org/pdf/2512.24952](https://arxiv.org/pdf/2512.24952)

- 现有评估方法只看“最后一帧”或“最佳匹配帧”，导致 结果欺骗（outcome-hacking）：模型通过错误的中间过程却得出看似正确的最终画面，被误判为推理正确。 缺乏专门衡量“过程是否合法”的指标，无法反映视频模型在时序、物理、符号等维度上是否 连贯地、按规则地 完成推理。 因此，当前视频生成模型虽在单帧指标上表现尚可，但其 真实推理能力被严重高估，离“通用视觉推理器”仍有显著差距。 为解决上述问题，论文提出 过程感知评估范式，并给出两大贡献： VIPER 基准：覆盖 6 大领域 16 项任务、309 条样本，强调过程约束。 POC@r 指标：强制同时满足“结果一致性（OC）”与“过程一致性（PC）”，用 VLM-as-Judge 统一打分，显著降低 outcome-hacking 误判。
## Watching, Reasoning, and Searching: A Video Deep Research Benchmark on Open Web for Agentic Video Reasoning【Video Reasoning】

[https://arxiv.org/pdf/2601.06943](https://arxiv.org/pdf/2601.06943)

- 解决**“视频只能提供局部视觉线索，而可验证的答案却散落在开放网络中”**这一现实场景下的视频问答难题。作者提出 VideoDR——首个视频驱动的开放域深度研究基准，系统评估模型能否： 跨帧提取视觉锚点 迭代调用浏览器搜索 在视频-网页联合证据空间做多跳推理
## VideoLoom: A Video Large Language Model for Joint Spatial-Temporal Understanding【Video Reasoning】

[https://arxiv.org/pdf/2601.07290](https://arxiv.org/pdf/2601.07290)

- Video LLM在联合空间-时间理解（joint spatial-temporal understanding）上的不足。作者提出 VideoLoom 套件，通过以下三项工作实现真正的联合时空理解： 构建 LoomData-8.7k：首个大规模长视频数据集，同时提供逐帧时间描述与人物完整轨迹掩码。 设计 VideoLoom 模型：采用 SlowFast 视觉令牌与 MLLM-SAM2 架构，**在单一模型内完成时间定位与空间分割**。 发布 LoomBench 基准：提出 When/Where/Combined 三类问答，对模型的联合时空能力进行系统评估。
## Molmo2: Open Weights and Data for Vision-Language Models with Video Understanding and Grounding【Video Reasoning】

[https://arxiv.org/pdf/2601.10611](https://arxiv.org/pdf/2601.10611)

- 首次以“完全开放”方式（数据+权重+代码）实现并超越专有模型的视频-多图细粒度理解与时空定位能力。
## Thinking in Frames: How Visual Context and Test-Time Scaling Empower Video Reasoning【Video Reasoning】

[https://arxiv.org/pdf/2601.21037](https://arxiv.org/pdf/2601.21037)

- 论文构建两大对立任务谱系——**低视觉变化的离散迷宫导航（MAZENAVIGATION）与高视觉变化的连续七巧板拼搭（TANGRAMPUZZLE）**——来验证视频生成模型能否以**生成的中间帧作为推理步骤**，实现零样本分布外泛化，并探索“视觉测试时缩放定律”。
## GeoBench: Rethinking Multimodal Geometric Problem-Solving via Hierarchical Evaluation【Visual Reasoning, Junchi Yan】

[https://arxiv.org/pdf/2512.24119](https://arxiv.org/pdf/2512.24119)

- 针对现有视觉-语言模型（VLM）在几何问题求解（GPS）评估中的三大缺陷，提出并构建了一个分层诊断基准 GeoBench，旨在系统、细粒度地衡量模型的几何推理能力，而不仅仅关注最终答案对错。
## **Milestones over Outcome: Unlocking Geometric Reasoning with Sub-Goal Verifiable Reward**【Visual Reasoning, Junchi Yan】

[https://arxiv.org/pdf/2601.05073](https://arxiv.org/pdf/2601.05073)

- 作者提出“面向子目标的可验证奖励”范式，**将几何证明分解为一系列可自动检验的数值子目标，并构建对应基准 GeoGoal**，实现： 细粒度评估：通过 Skeleton Rate、Skeleton Completion、Consistency Ratio 等指标同时度量单步正确率与整条链一致性； 密集强化学习：以子目标验证结果作为即时奖励，用 GRPO 优化策略，显著提升几何性能（+9.7%）并跨域迁移至通用数学（+8.0%）与通用推理（+2.8%）。
## BabyVision: Visual Reasoning Beyond Language【Visual Reasoning】

[https://arxiv.org/pdf/2601.06521](https://arxiv.org/pdf/2601.06521)

- 当前多模态大模型在需要丰富知识的高阶评测上表现优异，却在婴幼儿即可完成的“前语言”视觉任务上系统性失败——最佳模型仍落后 6 岁儿童约 20 分。提出 BABYVISION（388 题，22 子类，4 大领域）与 BABYVISION-GEN（280 题视觉生成版），用零知识、纯视觉驱动的题目量化模型与人类不同年龄段的差距，并配套自动评分工具（96 % 人工一致）。
## Thinking with Deltas: Incentivizing Reinforcement Learning via Differential Visual Reasoning Policy【Visual Reasoning】

[https://arxiv.org/pdf/2601.06801](https://arxiv.org/pdf/2601.06801)

- 针对多模态大模型在RLVR范式下的“感知-推理解耦”问题：现有方法仅以文本结果奖励为优化目标，导致**模型把视觉输入当作可旁路的噪声，退化为“盲推理器”**。作者提出 Thinking with Deltas 框架，通过 Differential Visual Reasoning Policy（DVRP）在**训练阶段引入视觉三元组（原图、掩码图、扰动图）的差分监督**，**强制模型对视觉信息既敏感又鲁棒**，从而真正依赖视觉证据进行推理，无需额外标注或外部工具即可在数学与医学等多模态任务上显著超越现有 RLVR 方法。
## Reading or Reasoning? Format Decoupled Reinforcement Learning for Document OCR【Visual Reasoning】

[https://arxiv.org/pdf/2601.08834](https://arxiv.org/pdf/2601.08834)

- OCR 中公式、表格等格式化内容输出熵远高于纯文本，导致先进模型在此类区域不确定性大、错误率高。提出格式解耦强化学习 FD-RL，采用“SFT 先读准 → RL 再推理”两阶段训练： 熵值数据过滤——用 SFT 模型计算每篇文档平均 token 熵，保留高熵样本集中训练； 格式解耦奖励——将输出拆为纯文本、公式、表格三类，分别施加字符串匹配、LaTeX-BLEU、TEDS 结构一致性奖励，引导模型学习格式规则而非记忆 token。OmniDocBench 端到端模型新纪录 90.41，公式 CDM 88.67、表格 TEDS 87.35 均列第一。
## LaViT: Aligning Latent Visual Thoughts for Multi-modal Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2601.10129](https://arxiv.org/pdf/2601.10129)

- 通过“白盒轨迹蒸馏”强制学生在生成文本前，先自回归地重构教师的潜在视觉语义与注意力轨迹，从而**对齐“潜在视觉思维”**，提升视觉定位与复杂推理能力。用 4 个连续潜在令牌 V 作为“视觉思维容器”，通过白盒轨迹蒸馏与课程式感官门控，迫使学生在生成答案前自回归地重构教师的视觉语义 Vsem 与注意力轨迹 Atraj。
## CoF-T2I: Video Models as Pure Visual Reasoners for Text-to-Image Generation【Visual Reasoning】

[https://arxiv.org/pdf/2601.10061](https://arxiv.org/pdf/2601.10061)

- 能否将视频模型直接作为纯视觉推理器，引导高质量文本到图像生成？**把“文本→单张图像”重写成 文本→三帧潜码轨迹 {z₁,z₂,z₃}→末帧解码输出 利用视频模型固有的 Chain-of-Frame (CoF) 先验**，在推理阶段自动完成“语义草稿→语义修正→美学精修”的纯视觉迭代，无需外部验证器或文本 CoT。
## V-Zero: Self-Improving Multimodal Reasoning with Zero Annotation【Visual Reasoning】

[https://arxiv.org/pdf/2601.10094](https://arxiv.org/pdf/2601.10094)

- **VLM在多模态推理任务中对大规模人工标注数据的高度依赖。**作者提出 V-Zero，一个**完全无需任何人工标注、仅利用未标注图像即可实现自我提升的通用后训练框架**，通过 Questioner 与 Solver 的协同演化循环，使模型在零标注条件下持续提升视觉推理能力。**V-Zero 用“直觉-推理反差”奖励驱动 Questioner 出难题，用多数投票伪标签训练 Solver**，两轮循环即可在完全零标注条件下，让 Qwen2.5-VL-7B 在多模态推理基准上反超人工监督，实现低成本、可扩展的 VLM 自我进化。
## AdaReasoner: Dynamic Tool Orchestration for Iterative Visual Reasoning【Visual Reasoning】

[https://arxiv.org/pdf/2601.18631](https://arxiv.org/pdf/2601.18631)

- 提出 AdaReasoner，目标是把“工具使用”从**工具专属、显式监督**的行为转化为**可泛化的通用推理技能，使模型能够：****在零样本情况下推断未见工具的用途；根据任务上下文与中间结果动态编排多工具序列；在多轮交互中自主决定调用、放弃或调整工具使用频率****。**
## ThinkRL-Edit: Thinking in Reinforcement Learning for Reasoning-Centric Image Editing【Visual Reasoning, ByteDance】

[https://arxiv.org/pdf/2601.03467](https://arxiv.org/pdf/2601.03467)

- 作者提出 ThinkRL-Edit，核心目标是将**视觉推理与图像生成分离**，在生成前显式引入规划–反思的链式思维（CoT）采样，扩大推理路径的探索；并设计无偏好的链式排序与二值检查单奖励，实现稳定、可解释的策略更新，从而显著提升指令忠实度、视觉一致性与语义合理性。
## Render-of-Thought: Rendering Textual Chain-of-Thought as Images for Visual Latent Reasoning【Visual Reasoning, Tencent】

[https://arxiv.org/pdf/2601.14750](https://arxiv.org/pdf/2601.14750)

- 作者提出 Render-of-Thought（RoT） 框架，首次**将文本化推理步骤渲染成图像**，**再利用冻结的 VLM 视觉编码器把图像语义注入隐空间**，实现： 3–4× token 压缩与显著推理加速 可视化中间过程，使隐式推理链可被追踪、分析 即插即用：无需额外预训练，仅通过两阶段自蒸馏即可把现有 VLM 升级为“视觉隐式推理”模式。
## EverMemOS: A Self-Organizing Memory Operating System for Structured Long-Horizon Reasoning【Memory】

[https://arxiv.org/pdf/2601.02163](https://arxiv.org/pdf/2601.02163)

- EverMemOS 提出“自组织记忆操作系统”，用**类脑记忆生命周期**（ episodic trace → semantic consolidation → reconstructive recollection ）**把碎片化对话沉淀为结构化语义单元（MemScene）**，**在检索阶段按需必要且足够地重组成上下文**，从而支持长周期一致推理与用户建模。
## MemOCR: Layout-Aware Visual Memory for Efficient Long-Horizon Reasoning【Memory】

[https://arxiv.org/pdf/2601.21468](https://arxiv.org/pdf/2601.21468)

- 为此，作者提出 **MemOCR**，**将记忆从 1D 文本流升级为 2D 视觉画布，通过版式（字体、字号、位置）显式控制各信息片段的视觉显著性，实现“关键信息大字体高可见、辅助信息小字体低可见”的弹性预算分配**，从而在总视觉 token 数受限时仍维持长程推理性能。
## R3L: Reflect-then-Retry Reinforcement Learning with Language-Guided Exploration, Pivotal Credit, and Positive Amplification

[https://arxiv.org/pdf/2601.03715](https://arxiv.org/pdf/2601.03715)

- 核心目标：解决 LLM 强化学习在困难任务上**探索低效、信用粗糙、训练不稳定**三大痛点。R3L **用语言反思把失败变成成功，用关键信用只改错的部分，用正样本放大让优化方向始终指向正确解**，从而在稀疏奖励、长程推理场景实现高效探索与稳定收敛。
## GDPO: Group reward-Decoupled Normalization Policy Optimization for Multi-reward RL Optimization【NVIDIA】

[https://arxiv.org/pdf/2601.05242](https://arxiv.org/pdf/2601.05242)

- 针对“多奖励强化学习（multi-reward RL）”场景下，直接将 Group Relative Policy Optimization（GRPO）用于异构奖励组合时出现的**奖励信号坍缩（reward collapse）**问题。作者提出 Group reward-Decoupled Normalization Policy Optimization（GDPO），核心思想是： **对每个奖励单独做组内归一化，保留跨奖励的相对差异**；** 将归一化后的奖励优势求和，再施加批次级归一化**，保证数值尺度稳定； 在工具调用、数学推理、代码生成三类任务上系统验证，GDPO 在正确性、格式、长度、bug 比例等多项目标上一致优于 GRPO，且训练过程更稳定。
## Dynamic Large Concept Models: Latent Reasoning in an Adaptive Semantic Space【ByteDance Seed】

[https://arxiv.org/pdf/2512.24617](https://arxiv.org/pdf/2512.24617)

- 作者提出 Dynamic Large Concept Models（DLCM），通过以下方式重新分配计算： 在潜在表示空间端到端学习可变长度语义边界，将序列动态分割为“概念”片段； 把原本浪费在冗余token上的算力迁移到压缩后的概念序列，用高容量主干完成推理； 再通过因果交叉注意力把概念表征解码回token预测，保持自回归特性。 最终，在相同推理FLOPs 下，DLCM 把约 1/3 算力转投到高维概念主干，在 12 项零样本基准上平均提升 2.69%，验证“把计算放在语义关键处而非均匀铺 token”是提升规模效率的新路径。
## ⭐⭐The Molecular Structure of Thought: Mapping the Topology of Long Chain-of-Thought Reasoning【ByteDance Seed】

[https://arxiv.org/pdf/2601.06002](https://arxiv.org/pdf/2601.06002)

- **LLM如何学习并表征有效的Long CoT？具体而言，作者观察到：**** 从人类或弱指令模型中蒸馏出的长链式思维数据无法让 LLM 稳定地掌握长链推理能力****； ****只有从强推理模型中蒸馏的数据才能有效提升模型在长链推理任务上的表现****； ****这表明长链式思维并非简单的关键词或格式模仿，而是依赖于某种内在的结构稳定性****。**
## **ReasonAny: Incorporating Reasoning Capability to Any Model via Simple and Effective Model Merging****【AI Lab】**

[https://arxiv.org/pdf/2601.05560](https://arxiv.org/pdf/2601.05560)

- 解决“将通用推理能力（Reasoning）注入到任意领域专用模型（X）”时出现的破坏性性能坍缩（Destructive Performance Collapse）问题。目标是在无需重新训练的前提下，把具备long-CoT的LRM的能力，与已有的领域专用模型（如安全、生物医学、金融）融合，实现“Reasoning + X”的协同。关键发现：**推理能力主要驻留在低梯度敏感区域，而领域知识才对应高梯度参数**。 解决方案：提出ReasonAny框架，通过对比梯度识别（Contrastive Gradient Identification）显式区分低梯度推理参数与高梯度任务参数，再以互斥掩码合并，避免参数冲突。对推理模型取 Bottom-k 低梯度权重 对任务模型取 Top-k 高梯度权重 通过互斥掩码 Nr∖Nt 、Nt∖Nr 消除参数冲突。
## Reinforcement Learning for Chain of Thought Compression with One-Domain-to-All Generalization

[https://arxiv.org/pdf/2601.06052](https://arxiv.org/pdf/2601.06052)

- 核心做法**仅对已完全掌握（通过率=1）且长度超过自身正确解安全区间的样本施加软性长度惩罚**，避免全局一刀切或硬截断带来的奖励黑客与性能崩溃。
## PaCoRe: Learning to Scale Test-Time Compute with Parallel Coordinated Reasoning【StepFun】

[https://arxiv.org/pdf/2601.05593](https://arxiv.org/pdf/2601.05593)

- PaCoRe 的目标就是在上下文长度不变的前提下，把 TTC 的扩展主力从“串行深度”切换到“协调并行广度”，通过多轮消息传递架构，让模型： **每轮并行展开数百条推理轨迹； 把轨迹压缩成极短的“消息”喂回上下文**； 在下一轮基于这些消息再展开新一轮并行探索； 最终合成出超越任何单条轨迹的高质量答案。
## ⭐Consolidation or Adaptation? PRISM: Disentangling SFT and RL Data via Gradient Concentration【SFT&RL】

[https://arxiv.org/pdf/2601.07224](https://arxiv.org/pdf/2601.07224)

- 混合 SFT→RL 流程中，所有数据一刀切导致： 低冲突样本被 RL 过度探索 → 噪声、浪费算力 高冲突样本在 SFT 被固化 → 逻辑错误难纠正。关键洞察（Schema Theory + 机制可解释性）。**梯度空间集中度是内部认知冲突的量化信号：（1）高集中度 ⇔ 局部“知识神经元”大幅更新 ⇔ 需结构调整 → 送 RL；（2）低集中度 ⇔ 参数均匀微调 ⇔ 可模式巩固 → 送 SFT**。
## RubricHub: A Comprehensive and Highly Discriminative Rubric Dataset via Automated Coarse-to-Fine Generation【RLVR】

[https://arxiv.org/pdf/2601.08430](https://arxiv.org/pdf/2601.08430)

- 解决**开放式生成任务缺乏可验证真值**而导致的**监督信号天花板效应**。作者提出**自动化“粗→细”评分标准生成框架，并构建RubricHub（约11万条、多领域、细粒度、高判别力评分标准数据集）**，通过**Rubric-based Rejection Sampling Fine-Tuning (RuFT)** 与 **Rubric-based Reinforcement Learning (RuRL)** 两阶段后训练，显著提升模型在开放式任务上的表现，甚至在医疗基准 HealthBench 上**超越闭源前沿模型 GPT-5**。三阶段自动化生产细粒度标准 **① 原则+回复双锚定生成 → ② 多模型聚合去偏 → ③ 难度演化升级判别力** 输出单条查询 30+ 原子准则，显著缓解“高分饱和”天花板。
## JudgeRLVR: Judge First, Generate Second for Efficient Reasoning【RLVR, Xiaomi】

[https://arxiv.org/pdf/2601.08468](https://arxiv.org/pdf/2601.08468)

- 针对“仅依赖最终答案正确性进行强化学习”带来的低效推理问题，提出 JudgeRLVR 两阶段范式，核心目标可概括为： 抑制无效探索：传统 RLVR 以最终答案正确性为唯一奖励，易诱导模型陷入冗长、反复回溯的试错式生成，造成 token 浪费与信息密度下降。 **先判别后生成：通过先训练“判别器”能力（判断解是否正确），再将该能力迁移到生成阶段，使模型在展开长链推理前就能内部剪枝低价值分支，实现“先生成正确路径，再输出答案”**。 兼顾准确率与效率：在不引入显式长度惩罚的前提下，同时提升数学域内外任务的准确率并显著缩短输出长度，获得更优的“质量–效率”权衡。
## Spurious Rewards Paradox: Mechanistically Understanding How RLVR Activates Memorization Shortcuts in LLMs【RLVR】

[https://arxiv.org/pdf/2601.11061](https://arxiv.org/pdf/2601.11061)

- 揭示并机械化解释“虚假可验证奖励强化学习（RLVR）”为何能让已污染的大模型突然暴涨数学基准——模型并非学会推理，而是被**精确触发**了一条“背答案”捷径。
## Harder Is Better: Boosting Mathematical Reasoning via Difficulty-Aware GRPO and Multi-Aspect Question Reformulation【RLVR】

[https://arxiv.org/pdf/2601.20614](https://arxiv.org/pdf/2601.20614)

- 提出 **MathForge** 框架，通过“算法+数据”双轮驱动，让 RLVR 训练聚焦**可解但更难**的数学问题，从而突破大模型数学推理天花板。MQR 产出“更高难度、答案保真”的数据 ↓ DGPO 用平衡+加权机制，把梯度集中投放到这些难题 ↓ 模型在可解但更具挑战的样本上获得最大更新 ↓ 性能提升后，可继续用更强模型迭代 MQR，形成正向循环
## Are Your Reasoning Models Reasoning or Guessing? A Mechanistic Analysis of Hierarchical Reasoning Models【HRM】

[https://arxiv.org/pdf/2601.10679](https://arxiv.org/pdf/2601.10679)

- 层次化推理模型（Hierarchical Reasoning Model, HRM）究竟是在“推理”还是在“猜”？HRM 的三大反直觉现象： 在极简单谜题上失败，源于固定点假设失效； 存在“grokking”式突变动态，而非渐进式改进； 潜在空间存在多个固定点，模型易被虚假吸引子困住。提出“猜测”视角： **HRM 并非逐步精炼解，而是随机初始化后试图快速撞上一个固定点，一旦落入错误吸引子便难以逃脱**。基于猜测视角设计三种规模化“猜测次数”的策略：**数据增强（提升猜测质量）；输入扰动（利用推理随机性增加猜测次数）；模型自助（利用训练随机性增加猜测次数）**。
## DARC: Decoupled Asymmetric Reasoning Curriculum for LLM Evolution

[https://arxiv.org/pdf/2601.13761](https://arxiv.org/pdf/2601.13761)

- 一种零人工标注即可让大语言模型稳定自进化的两阶段框架。DARC 通过“解耦非对称推理课程”将进化过程拆分为两个阶段，使 **Questioner 在外部语料与显式难度等级监督下稳定生成难度校准问题**，再利用具备文档访问权的**教师 Solver 产生高质量伪标签**，蒸馏给无文档访问的学生 Solver，从而在不依赖人工标注的前提下实现稳定、可扩展的自进化。
## Teaching Large Reasoning Models Effective Reflection【AI Lab】

[https://arxiv.org/pdf/2601.12720](https://arxiv.org/pdf/2601.12720)

- LRM虽会“自我反思”，但多数为**浅层反思**：重复原错、不修正、徒增开销。SCFT（Self-Critique Fine-Tuning） – 零外部标注：模型自生成“解答+批评”，用 ground-truth 过滤保留“指出并修正错误”或“确认正确”样本。 – 监督训练：输入“问题+解答”，最大化高质量批评似然，一体两用（生成/批评）。 RLERR（Reinforcement Learning with Effective Reflection Rewards） – 以 SCFT 模型为初始策略，采用五层反思原则（真实、建设、具体、深度、信息增益）构造密集奖励。 – 结果奖励+反思奖励联合强化，内化自我纠错策略。
## ChartVerse: Scaling Chart Reasoning via Reliable Programmatic Synthesis from Scratch【AI Lab】

[https://arxiv.org/pdf/2601.13606](https://arxiv.org/pdf/2601.13606)

- 作者提出 ChartVerse 框架，**通过可执行程序从无到有地规模化生成复杂图表与可验证推理数据**，以同时提升视觉多样性、分布广度与推理可靠性，从而支撑开源 VLM 在图表推理任务上达到与闭源大模型相媲美的性能。ChartVerse 用“复杂度度量+逆向真值 QA”**首次让小参数开源模型在图表推理上击败大参数闭源教师**，为社区提供了可复现、可扩展、无幻觉的 600 k-SFT & 40 k-RL 高质量训练数据。
## 🧐Learning to Discover at Test Time【Test-time, NVIDIA】

[https://arxiv.org/pdf/2601.16175](https://arxiv.org/pdf/2601.16175)

- 如何让人工智能在**测试阶段**（test time）**针对单一科学问题**发现一条**超越人类现有最佳水平**的解，而不是像传统方法那样仅通过提示冻结的大模型做搜索。作者提出 TTT-Discover（Test-Time Training to Discover），其关键思想是： 把“测试阶段”本身当成一次强化学习环境，让大模型继续在线训练； 训练目标不再是“平均表现好”，而是**只要生成一次超越现有最优（SOTA）的解即可**； 通过熵目标函数与PUCT 复用策略，把算力与梯度更新集中投向最有希望产生突破的解。
## LongCat-Flash-Thinking-2601 Technical Report【LongCat】

[https://arxiv.org/pdf/2601.16725](https://arxiv.org/pdf/2601.16725)

- 在开源社区首次实现 560 B-MoE、27 B 激活 的通用 agentic 推理模型，能在 真实、长周期、多工具、带噪声 环境中稳定泛化，逼近闭源 SOTA。
## Reinforcement Learning via Self-Distillation

[https://arxiv.org/pdf/2601.20802](https://arxiv.org/pdf/2601.20802)

- 论文提出“带丰富反馈的强化学习（RLRF）”新范式，并设计自蒸馏策略优化（SDPO）算法，核心贡献如下：将环境提供的token 级反馈视为可观测状态，突破 RLVR 的标量奖励信息瓶颈；把当前策略在反馈条件下的输出当作“自教师”，通过logit 级 KL 散度将反馈转化为稠密信用分配，无需外部强教师或显式奖励模型。SDPO 把“看过反馈后的自己”当作无成本强教师，通过**logit 级自蒸馏**将任意文本反馈转化为**逐 token 的稠密优势**，在训练与测试阶段均显著突破 RLVR 的稀疏奖励瓶颈。
## R^3: Replay, Reflection, and Ranking Rewards for LLM Reinforcement Learning

[https://arxiv.org/pdf/2601.19620](https://arxiv.org/pdf/2601.19620)

- 在基于组相对策略优化（GRPO）的方法中，同一查询的多次回答若奖励几乎相同，标准差趋于零，优势估计退化为零，梯度消失，训练停滞。 当任务变难时，模型容易输出“全对”或“全错”的同质回答，进一步放大该问题。 传统做法直接丢弃低方差批次，浪费计算；或仅依赖结果奖励，无法对截断、错误回答给出细粒度监督。 **R³ 通过 Replay（跨上下文重放）、Reflection（上下文自省）、Ranking reward（结构熵排序奖励） 三条机制**，人为恢复组内奖励差异，使优势估计始终非零，从而持续提供有效梯度，提升样本效率与最终推理性能。
## Exploring Reasoning Reward Model for Agents【MMLab】

[https://arxiv.org/pdf/2601.22154](https://arxiv.org/pdf/2601.22154)

- 提出 **Agent-RRM**（Agent Reasoning Reward Model）与 **Reagent** 训练框架，解决 Agentic RL 中“稀疏结果奖励无法区分中间推理质量、缺乏可执行改进信号”的核心痛点。Agent-RRM 对每条轨迹输出三元组： <think>：逐步推理轨迹 <critique>：可执行的错误指正 <score>：0–1 整体质量分 无需真值即可提供密集、语言化、可解释的反馈。
## From Meta-Thought to Execution: Cognitively Aligned Post-Training for Generalizable and Reliable LLM Reasoning

[https://arxiv.org/pdf/2601.21909](https://arxiv.org/pdf/2601.21909)

- “思维链监督微调（CoT-SFT）+ 结果导向强化学习（RL）”——在数学推理任务上虽然有效，却与人类真实的问题解决过程存在根本错位。人类认知天然把解题拆成两步：先习得跨问题泛化的抽象策略（meta-knowledge）；再把抽象策略适配到具体实例并执行。现有方法把整个解题轨迹当成最小训练单元，把抽象策略与问题专属的执行细节纠缠在一起，导致模型难以提取可迁移的通用策略，也无法在执行阶段抑制“过度自信的错误”逐级放大。因此，论文试图让后训练流程与人类两阶段认知对齐，以提升泛化性与执行可靠性，并降低训练开销。先生成思维模板，再生成答案。
## **Reasoning While Asking: Transforming Reasoning Large Language Models from Passive Solvers to Proactive Inquirers**

[https://arxiv.org/pdf/2601.22139](https://arxiv.org/pdf/2601.22139)

- 作者提出 Proactive Interactive Reasoning（PIR）新范式，将模型从“被动求解器”转变为“主动询问者”，使其在推理过程中实时检测不确定性并主动向用户发起澄清，从而在源头对齐用户意图，减少无效推理与交互轮次。
## MMFineReason: Closing the Multimodal Reasoning Gap via Open Data-Centric Methods【AI Lab】

[https://arxiv.org/pdf/2601.21821](https://arxiv.org/pdf/2601.21821)

- 作者提出 MMFineReason——**一套 1.8 M 样本、5.1 B tokens 的超大规模开源多模态推理数据集，并配套三阶段构建流程（收集标准化 → CoT 蒸馏 → 质量与难度筛选）**，证明通过高质量数据工程即可让 2 B/4 B/8 B 级开源模型在多项基准上逼近或超越 30 B 级闭源系统。
## Scaling Reasoning Hop Exposes Weaknesses: Demystifying and Improving Hop Generalization in Large Language Models

[https://arxiv.org/pdf/2601.21214](https://arxiv.org/pdf/2601.21214)

- 在CoT推理中，一旦测试所需推理步数（hop）超出训练分布，准确率骤降；错误并非均匀散布，而是集中在少数关键 token 位置。
## **Reuse your FLOPs: Scaling RL on Hard Problems by Conditioning on Very Off-Policy Prefixes****【Meta】**

[https://arxiv.org/pdf/2601.18795](https://arxiv.org/pdf/2601.18795)

- **在极难问题上RL训练LLM时计算浪费严重、学习信号稀疏**的核心痛点。
## CoT-Seg: Rethinking Segmentation with Chain-of-Thought Reasoning and Self-Correction

[https://arxiv.org/pdf/2601.17420](https://arxiv.org/pdf/2601.17420)

- 作者提出 **CoT-Seg**，一个**无需训练**的框架，将链式思维（Chain-of-Thought, CoT）推理与**自纠正**机制引入分割流程。
## POPE: Learning to Reason on Hard Problems via Privileged On-Policy Exploration

[https://arxiv.org/pdf/2601.18779](https://arxiv.org/pdf/2601.18779)

- 人工/oracle 解答即使不模仿，也能当探索提示： **“把前缀拼进题干 → 模型按指令继续写”即可把初始状态搬到更易得奖励的区域**，且行为可借助指令跟随与回溯迁移回无提示场景。

# Data

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Data
- 方法：language, reasoning, optimization, stat-ml, explanation
- 论文/报告：13 篇
- Efficiently Estimating Data Efficiency for Language Model Fine-tuning
- ⭐Can Small Training Runs Reliably Guide Data Curation? Rethinking Proxy-Model Practice【James Zou】
- ⭐**Logics-STEM: Empowering LLM Reasoning via Failure-Driven Post-Training and Document Knowledge Enhancement****【Alibaba】**
- ⭐DatBench: Discriminative, Faithful, and Efficient VLM Evaluations
- ⭐⭐One Sample to Rule Them All: Extreme Data Efficiency in RL Scaling【Pengfei Liu】
- EDCO: Dynamic Curriculum Orchestration for Domain-specific Large Language Model Fine-tuning【Huawei】
- Closing the Data Loop: Using OpenDataArena to Engineer Superior Training Datasets【AI Lab】
- Do explanations generalize across large reasoning models?
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Efficiently Estimating Data Efficiency for Language Model Fine-tuning

[https://arxiv.org/pdf/2512.24991](https://arxiv.org/pdf/2512.24991)

- 如何在不进行大规模标注与反复微调的前提下，准确估计某一特定任务对LLM微调所需的数据效率？提出一种**轻量级、可迁移的预测方法**，仅凭极少量已标注样本（如 32 条）就能估计该任务的“数据效率曲线”，进而给出达到指定性能所需的最小标注规模，从而避免不必要的标注与训练开销。
## ⭐Can Small Training Runs Reliably Guide Data Curation? Rethinking Proxy-Model Practice【James Zou】

[https://arxiv.org/pdf/2512.24503](https://arxiv.org/pdf/2512.24503)

- **小模型代理实验能否可靠地指导大模型预训练的数据配方（data recipe）决策？论文提出修正目标：****数据配方评估应预测“该配方在经充分调参后的大模型上所能达到的最优性能”，而非固定超参数下的表面结果****。**为降低全面调参的成本，作者给出即插即用的补丁方案： 用极小学习率（≈1×10⁻⁶–1×10⁻⁵）训练代理模型，即可在不做大规模调参的前提下，获得与充分调参后大模型高度一致（Spearman ρ>0.95）的数据配方排序。 理论层面，作者在随机特征模型上证明： 当宽度足够大且学习率足够小时，小模型在极小学习率下的验证损失排序与无限宽度极限下的最优可达损失排序一致，从而保证跨尺度一致性。 综上，论文核心贡献是揭示并修复了“小模型代理评估”在数据配方选择中的可靠性缺陷，为工业界提供了一种低成本、高保真的数据决策协议。
## ⭐**Logics-STEM: Empowering LLM Reasoning via Failure-Driven Post-Training and Document Knowledge Enhancement****【Alibaba】**

[https://arxiv.org/pdf/2601.01562](https://arxiv.org/pdf/2601.01562)

- 在“有效性、效率、可扩展性”三方面，构建面向推理模型的数据-算法协同设计引擎究竟需要哪些关键要素？
## ⭐DatBench: Discriminative, Faithful, and Efficient VLM Evaluations

[https://arxiv.org/pdf/2601.02316](https://arxiv.org/pdf/2601.02316)

- 提出将“评测”视为数据精选问题，通过系统性的转换-过滤-精选四步流水线，把现有 33 个数据集重构为：** DATBENCH：高鉴别力子集，平均提速 13×（最高 50×），仍保持完整排名一致性**； DATBENCH-FULL：经严格清洗后的全集，用于最终报告与深度分析。 最终实现在不新建数据的前提下，交付忠实、高区分、低能耗的 VLM 评测基准，并揭示语言先验、推理-感知权衡与“过度思考”惩罚等既往被噪声掩盖的模型行为规律。
## ⭐⭐One Sample to Rule Them All: Extreme Data Efficiency in RL Scaling【Pengfei Liu】

[https://arxiv.org/pdf/2601.03111](https://arxiv.org/pdf/2601.03111)

- 证明仅用一个精心设计的训练样本即可通过RL显著提升大语言模型在数学、物理、化学、生物乃至更通用推理任务上的表现。揭示单一数学推理样本为何能把改进迁移到与数学相距甚远的学科，从而验证“数学技能是通用推理的基石”这一假说。提出“样本工程”（sample engineering）新方向——用算法化方式甄选或合成具备多学科知识融合、技能覆盖全面的“通才样本”（polymath sample），取代传统“堆数据”思路，以更低的数据成本解锁更强的推理能力。
## EDCO: Dynamic Curriculum Orchestration for Domain-specific Large Language Model Fine-tuning【Huawei】

[https://arxiv.org/pdf/2601.03725](https://arxiv.org/pdf/2601.03725)

- 针对“领域专用大模型微调时训练样本顺序固定、无法随模型能力演化而动态调整”这一瓶颈，提出动态课程编排需求。作者提出 EDCO 框架，**以“推理熵”作为样本即时信息量度量，在训练过程中周期性地重估并遴选高熵样本**，实现课程顺序与模型学习状态的实时对齐，从而： 延缓熵塌陷，维持探索； 提升样本利用率与收敛效率； 在监督与强化两种微调范式下均获得一致增益。**动态课程生成**：周期排序 → 选最高熵子集 D^CD_k。统一微调器：即插即用 SFT（交叉熵）或 RLFT（GRPO），仅在高熵子集上更新。
## Closing the Data Loop: Using OpenDataArena to Engineer Superior Training Datasets【AI Lab】

[https://arxiv.org/pdf/2601.09733](https://arxiv.org/pdf/2601.09733)

- 把 SFT 数据构造从“经验拼数据”变成“可度量-可迭代”的优化过程，并发布两套高质量数据集： ODA-Math-460k **聚合 ODA 榜 Top 数学源 → 精确去重+赛题去污染 两阶段 Pass-Rate 过滤（去 trivial & 去 unsolvable）锁定“学习区” 蒸馏+Verifier 校验**，仅留正确 CoT 460k 样本在 AIME、HMMT 等 9 项基准平均 64.1%/68.8%，数据减半仍超 1.2M 级 SOTA ODA-Mixture-100k / 500k Anchor-and-Patch：以效率冠军 LIMO 为锚，按子榜 Rank-1 补数学、代码、通用、推理补丁 100k 难度优先（长序列）（+0.177 效率），500k 多样性随机（+5 分 Overall） 在 18 项通用/数学/代码/推理基准全面领先，500k 版刷新 Qwen2.5-7B 与 Qwen3-8B 的 Overall SOTA。
## Do explanations generalize across large reasoning models?

[https://arxiv.org/pdf/2601.11517](https://arxiv.org/pdf/2601.11517)

- LRM生成的CoT解释是否具备“跨模型泛化性”，即 一条由模型 A 产生的自然语言推理链，能否在模型 B、C、D… 上诱导出与模型 A 自身一致的答案？论文提出并验证了一套**以“跨模型行为一致性”为标尺的 CoT 解释泛化评估框架**，为“什么是一条好解释”提供了与忠实性、正确性并列的新维度。
## Beyond Static Datasets: Robust Offline Policy Optimization via Vetted Synthetic Transitions

[https://arxiv.org/pdf/2601.18107](https://arxiv.org/pdf/2601.18107)

- 针对离线强化学习（Offline RL, ORL）中“静态数据集与所学策略之间存在分布偏移”这一核心障碍，提出一种基于模型的不确定性感知合成框架 MoReBRAC，旨在： 缓解过度保守：传统 ORL 为防止分布外（OOD）动作导致的 Q 值高估，往往对策略施加过强的约束，结果在“随机”或“次优”数据场景下性能受限。 突破数据边界：仅依赖固定离线数据难以拼接出更优轨迹；MoReBRAC 通过学习一个双循环世界模型（LSTM-GRU）主动生成高保真合成转移，从而扩充训练流形。 保证合成可靠性：引入分层不确定性栈（VAE 流形检测 + 敏感度分析 + MC Dropout）对每条合成转移进行三级过滤，确保只有位于高置信区域的样本才被用于策略优化，防止“幻觉”动态累积误差。 综上，论文试图在不牺牲安全性的前提下，用不确定性引导的虚拟探索替代部分真实交互，使 ORL 在低成本、高安全要求的工业控制等场景中，既能利用次优/随机数据，又能稳健地提升策略性能。
## ⭐⭐MergeMix: Optimizing Mid-Training Data Mixtures via Learnable Model Merging

[https://arxiv.org/pdf/2601.17858](https://arxiv.org/pdf/2601.17858)

- **LLM中期训练阶段的数据配比优化**问题。提出一种低成本、高保真的自动化方法，直接以下游任务性能为优化目标，**快速搜索出最优数据混合比例**，而无需进行全量训练试错。**在中期训练的局部参数空间内，模型权重线性插值（model merging）可以高保真地近似数据混合训练的效果**，从而将昂贵的数据空间搜索转化为几乎零成本的权重空间搜索。**MergeMix 通过“用权重插值代替数据混合”这一范式转换，将数据配比优化的计算成本降低两个数量级**，同时保持或超越人工调优的性能。
## **MathMixup: Boosting LLM Mathematical Reasoning with Difficulty-Controllable Data Synthesis and Curriculum Learning**

[https://arxiv.org/pdf/2601.17006](https://arxiv.org/pdf/2601.17006)

- LLM在数学推理任务中因训练数据缺乏**可感知且可控的难度梯度**而导致的性能瓶颈。作者提出 MathMixup 框架，**通过“混合-分解”策略系统合成质量高、难度可控的数学推理题**，并构建带三级难度梯度的 MathMixupQA 数据集，使得： 可直接用于监督微调，显著提升 LLM 数学推理成绩； 可与现有数据集灵活混合，实现数据驱动的课程学习，进一步释放模型潜力。
## ⭐FineInstructions: Scaling Synthetic Instructions to Pre-Training Scale【Huggingface】

[https://arxiv.org/pdf/2601.22146](https://arxiv.org/pdf/2601.22146)

- 把互联网语料自动变成十亿级“指令-回答”对，让大模型从零开始就用“监督式指令学习”完成预训练，同等算力下全面优于传统自监督。
## ⭐Mechanistic Data Attribution: Tracing the Training Origins of Interpretable LLM Units

[https://arxiv.org/pdf/2601.21996](https://arxiv.org/pdf/2601.21996)

- 作者提出 Mechanistic Data Attribution（MDA） 框架，首次把影响函数（Influence Functions）扩展到单个可解释单元的粒度，实现：** 精确定位对某一 head/神经元/特征最具影响力的训练样本**； 通过数据增删干预，因果验证这些样本确实决定该单元的形成； 揭示诱导头（induction head）与上下文学习（ICL）之间的因果耦合； 基于高影响样本的结构性模式，构建可扩展的数据增强管线，在多个模型尺度上加速目标电路的收敛。

# Foundation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Foundation
- 方法：language
- 论文/报告：6 篇
- Efficient Context Scaling with LongCat ZigZag Attention【LongCat】
- How to Set the Batch Size for Large-Scale Pre-training?
- Revisiting Multi-Task Visual Representation Learning【ByteDance】
- A Scalable Measure of Loss Landscape Curvature for Analyzing the Training Dynamics of LLMs【Meta】
- Self-Improving Pretraining: using post-trained models to pretrain better models【Meta】
- Scaling Embeddings Outperforms Scaling Experts in Language Models【LongCat】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Efficient Context Scaling with LongCat ZigZag Attention【LongCat】

[https://arxiv.org/pdf/2512.23966](https://arxiv.org/pdf/2512.23966)

- 解决**超长上下文场景下全注意力（full attention）计算开销随序列长度二次增长**带来的效率瓶颈，作者提出 LongCat ZigZag Attention（LoZA），一种**稀疏注意力机制**，**可在有限算力预算内将任意基于全注意力的模型（特别是采用 MLA 的模型）转化为稀疏版本**。
## How to Set the Batch Size for Large-Scale Pre-training?

[https://arxiv.org/pdf/2601.05034](https://arxiv.org/pdf/2601.05034)

- 解决大规模预训练中“如何设定 batch size”这一核心问题，提出动态 Batch Size Scheduler：以累计数据量为自变量，按拟合曲线 f(N,D) 每 125 B token 平滑提升一次全局 batch（2 M→4 M→5 M→6 M），无需改动 LR。
## Revisiting Multi-Task Visual Representation Learning【ByteDance】

[https://arxiv.org/pdf/2601.13886](https://arxiv.org/pdf/2601.13886)

- **如何在一个统一框架内，利用大规模自动生成的伪标签，联合优化语义、自监督与密集空间三种异质目标，从而学得兼具全局语义与细粒度空间推理能力的通用视觉表征？提出 MTV——多任务视觉预训练框架，用共享 ViT 同时优化： ① 视觉-语言对比（VL） ② 自监督蒸馏+掩码预测（SSL） ③ 伪标签密集监督（区域-文本对齐 + 单目深度）1. 每加一项任务几乎单调提升；SSL 带来最大绝对增益。 2. 任务间协同 >20 %，无干扰。 3. 100 M 样本 MTV-B 在 ImageNet 零样本69.4 %，超越 400 M 训练的 CLIP-Base；细粒度任务（深度、对应、分割）全面领先。 4. 继续放大数据/模型仍有收益，但对应任务在 1 B 后需更专门设计。**
## A Scalable Measure of Loss Landscape Curvature for Analyzing the Training Dynamics of LLMs【Meta】

[https://arxiv.org/pdf/2601.16979](https://arxiv.org/pdf/2601.16979)

- Hessian 最大特征值 λ_max^H 是监控神经网络训练稳定性与“ progressive sharpening”的黄金指标，但在 LLM 尺度下计算需数百次 Hessian-vector product，与 FlashAttention 等高效 kernel 不兼容，导致至今缺乏 7 B 以上模型的在线曲率数据。提出 critical sharpness。仅利用前向传播即可在 <10 次计算内可靠估计，从而首次在 **7 B 参数级预训练与中期训练**中在线监测 progressive sharpening 与 Edge of Stability，并进一步用其指导数据配比、抑制灾难性遗忘。
## Self-Improving Pretraining: using post-trained models to pretrain better models【Meta】

[https://arxiv.org/pdf/2601.21343](https://arxiv.org/pdf/2601.21343)

- 用已后训的强模型当“教师”，**在预训练阶段就通过 RL 把生成质量、安全与事实性注入基座模型**。教师双角色： Rewriter——在线重写低质/不安全后缀，提供高质训练目标； Judge——对“原始｜重写｜K 条 rollout”打分，输出质量-安全-事实奖励。
## Scaling Embeddings Outperforms Scaling Experts in Language Models【LongCat】

[https://arxiv.org/pdf/2601.21204](https://arxiv.org/pdf/2601.21204)

- **当模型足够稀疏时，把参数预算投向 N-gram 嵌入表比继续增加 MoE 专家更能降低损失、提升推理吞吐**，且系统开销可控。

# Generation

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Generation
- 方法：generation, reasoning, multimodal-learning, multimodal-reasoning, stat-ml
- 论文/报告：15 篇
- VINO: A Unified Visual Generator with Interleaved OmniModal Context
- NextFlow: Unified Sequential Modeling Activates Multimodal Understanding and Generation【ByteDance】
- Learning Latent Action World Models In The Wild【World Model, Meta, LeCun】
- Inference-time Physics Alignment of Video Generative Models with Latent World Models【World Model, Meta】
- Advancing Open-source World Models【World Model, Ant】
- Think-Then-Generate: Reasoning-Aware Text-to-Image Diffusion with LLM Encoders【Kuaishou】
- Rethinking Video Generation Model for the Embodied World【ByteDance Seed】
- Scaling Text-to-Image Diffusion Transformers with Representation Autoencoders【Saining Xie】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## VINO: A Unified Visual Generator with Interleaved OmniModal Context

[https://arxiv.org/pdf/2601.02358](https://arxiv.org/pdf/2601.02358)

- 统一的视觉生成框架，在**单一扩散模型**内同时完成图像/视频的生成与编辑，并支持任意组合的文本、图像、视频条件输入。跨任务统一架构：无需为每种任务设计专属模块。异构条件融合：可靠地解耦并优先处理同时出现的文本、图像、视频信号，避免语义冲突。细粒度控制：在保留参考图像/视频身份与属性的前提下，执行长文本描述或短指令式编辑。
## NextFlow: Unified Sequential Modeling Activates Multimodal Understanding and Generation【ByteDance】

[https://arxiv.org/pdf/2601.02204](https://arxiv.org/pdf/2601.02204)

- 针对的是“统一多模态理解与生成”这一核心难题，NextFlow 通过“单解码器、统一离散 token 序列”框架，用 next-scale 预测替代逐像素预测，并引入双码本 tokenizer 与面向粗尺度的前缀 RL，首次在 7B 参数规模下实现： 1024×1024 图像 5 秒生成（≈6× 少于同分辨率 MMDiT 扩散模型 FLOPs） 与专用扩散模型媲美的视觉保真度 原生支持图文交错、链式思维推理、零样本编辑等多模态任务。
## Learning Latent Action World Models In The Wild【World Model, Meta, LeCun】

[https://arxiv.org/pdf/2601.05230](https://arxiv.org/pdf/2601.05230)

- **在无动作标签的大规模自然视频上训练出可直接用于机器人规划与控制的潜在动作世界模型**（Latent Action World Model, LAM），突破传统世界模型对显式动作标注的依赖。
## Inference-time Physics Alignment of Video Generative Models with Latent World Models【World Model, Meta】

[https://arxiv.org/pdf/2601.10553](https://arxiv.org/pdf/2601.10553)

- 将“物理合理性”重定义为**推理阶段对齐**问题——无需重训生成器，而是在采样时搜索/引导更合理的轨迹。把 VJEPA-2 的“惊讶度”变成物理合理性分数 对生成视频滑动窗口，用 VJEPA-2 的编码器-预测器在潜在空间做“上下文→未来”预测，再与真实编码比较，计算 $$r(x)=1-\cos!\bigl(\hat z_{\text{fut}},z_{\text{fut}}\bigr)$$ 分数越高 → 与模型物理预期越一致。
## Advancing Open-source World Models【World Model, Ant】

[https://arxiv.org/pdf/2601.20540](https://arxiv.org/pdf/2601.20540)

- LingBot-World 提出并开源了一套“视频生成→实时交互世界模型”完整范式，继承 14 B Wan2.2，建立通用视频先验。MoE 双专家 + 渐进课程（5 s→60 s）+ 动作 AdaLN 注入，得到分钟级一致、动作可控的 Base 模型。
## Think-Then-Generate: Reasoning-Aware Text-to-Image Diffusion with LLM Encoders【Kuaishou】

[https://arxiv.org/pdf/2601.10332](https://arxiv.org/pdf/2601.10332)

- 把冻结的 LLM 文本编码器变成“会思考”的推理器，先改写提示再驱动扩散模型，实现知识密集型、概念级文本到图像生成。
## Rethinking Video Generation Model for the Embodied World【ByteDance Seed】

[https://arxiv.org/pdf/2601.15282](https://arxiv.org/pdf/2601.15282)

- 解决**机器人视频生成领域缺乏系统评估基准与高质量训练数据**的核心问题。作者提出**RBench**（650 条跨 5 任务、4 形态的细粒度评测集 + 可复现的物理-任务联合指标）与**RoVid-X**（400 万条带光流、深度、任务分割与物理标注的开放视频数据），构成“评测-数据”闭环，推动视频生成模型从“看得美”走向“做得对、做得真”。
## Scaling Text-to-Image Diffusion Transformers with Representation Autoencoders【Saining Xie】

[https://arxiv.org/pdf/2601.16208](https://arxiv.org/pdf/2601.16208)

- 首次证明 Representation Autoencoder（RAE）能无缝扩展至十亿参数级自由文本到图像生成，在收敛速度、生成质量与过拟合鲁棒性上全面超越 SOTA VAE，并天然支持“理解-生成同空间”的统一多模态推理。
## StableWorld: Towards Stable and Consistent Long Interactive Video Generation

[https://arxiv.org/pdf/2601.15281](https://arxiv.org/pdf/2601.15281)

- 解决**交互式长视频生成中的稳定性与时间一致性退化**问题。StableWorld 以**零训练成本、即插即用**的方式，在推理阶段从源头阻断误差累积，为构建**稳定、一致、可交互的长时世界模型**提供了简单有效的通用方案。
## LoL: Longer than Longer, Scaling Video Generation to Hour【ByteDance Seed】

[https://arxiv.org/pdf/2601.16914](https://arxiv.org/pdf/2601.16914)

- oL: Longer than Longer 提出一种无需再训练的轻量级方法，首次在 1.3 B 参数规模上实现实时、无限长、流式视频生成而不塌陷。核心贡献如下：** 揭示 sink-collapse 根因 RoPE 周期性导致多频相位同步，多头注意力同时锁定 sink 帧，引发全局画面重置**。 提出 **Multi-Head RoPE Jitter 对每头基频独立扰动 θ^h=θ0(1+σϵh) ，打破跨头相位对齐**，显著抑制塌陷（Max 指标↓75 %），且保持运动与成像质量。
## A Mechanistic View on Video Generation as World Models: State and Dynamics

[https://arxiv.org/pdf/2601.17067](https://arxiv.org/pdf/2601.17067)

- 视频模型 = 无状态、双向注意力、开环生成 → 长程记忆爆炸、因果推理弱 世界模型 = 观测-状态-动力学三元组，要求持久且可干预。
## AR-Omni: A Unified Autoregressive Model for Any-to-Any Generation

[https://arxiv.org/pdf/2601.17761](https://arxiv.org/pdf/2601.17761)

- 无需任何外部专家解码器的统一AR模型，实现任意模态到任意模态（any-to-any）的生成能力。具体而言，现有“Omni”多模态大模型普遍依赖扩散模型、非自回归声码器等外部组件来完成图像或语音生成，导致训练与推理流程碎片化。AR-Omni 尝试在纯自回归框架内，用单一 Transformer 解码器、单一词表、单一 next-token 目标同时支持： 文本 ↔ 文本 文本 ↔ 图像 文本 ↔ 语音 以及三者任意组合的跨模态理解与生成。**纯自回归**即可在**单一模型**内完成**文本、图像、语音任意方向**的**高质量、实时生成**，无需扩散或声码器，为“omni”多模态大模型提供了**简洁、统一、可扩展**的新范式。
## Scientific Image Synthesis: Benchmarking, Methodologies, and Downstream Utility【AI Lab】

[https://arxiv.org/pdf/2601.17027](https://arxiv.org/pdf/2601.17027)

- 提出**逻辑驱动的“理解→规划→编码”框架**，先由 LLM 抽取实体与约束，再生成确定性 Python 代码执行绘图，实现**结构零幻觉**与**像素级精度**。论证了**高保真科学图像合成**不仅能解决视觉–逻辑背离，还能为 LMM 提供持续且可放大的推理增益。
## SkyReels-V3 Technique Report【Skywork】

[https://arxiv.org/pdf/2601.17323](https://arxiv.org/pdf/2601.17323)

- SkyReels-V3 提出了一套**统一多模态上下文学习框架**，在单一扩散 Transformer 内同时实现三大视频生成任务：参考图→视频、视频续拍/转场、音频驱动说话人。
## One-step Latent-free Image Generation with Pixel Mean Flows【Kaiming He】

[https://arxiv.org/pdf/2601.22158](https://arxiv.org/pdf/2601.22158)

- 实现**一步（one-step）、无潜空间（latent-free）的图像生成，把“多步采样”和“潜空间建模”这两个传统扩散/流模型的核心组件同时去掉，从而用端到端**的方式直接从噪声生成像素级图像。

# Agent Training

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Training
- 方法：agent, generation, reinforcement-learning, optimization
- 论文/报告：5 篇
- ⭐Youtu-Agent: Scaling Agent Productivity with Automated Generation and Hybrid Policy Optimization【Youtu】
- ⭐JudgeFlow: Agentic Workflow Optimization via Block Judge
- BAPO: Boundary-Aware Policy Optimization for Reliable Agentic Search
- Learn Like Humans: Use Meta-cognitive Reflection for Efficient Self-Improvement
- Self-Compression of Chain-of-Thought via Multi-Agent Reinforcement Learning
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## ⭐Youtu-Agent: Scaling Agent Productivity with Automated Generation and Hybrid Policy Optimization【Youtu】

[https://arxiv.org/pdf/2512.24615](https://arxiv.org/pdf/2512.24615)

- 作者提出 Youtu-Agent，目标是在“自动化构建”与“持续优化”之间建立桥梁，实现： 零人工或极少人工的 agent 配置生成（工具代码、提示词、YAML 配置） 不更新参数即可在线积累经验、自我演化 需要大幅提升性能时，提供可扩展且稳定的端到端强化学习 pipeline。
## ⭐JudgeFlow: Agentic Workflow Optimization via Block Judge

[https://arxiv.org/pdf/2601.07477](https://arxiv.org/pdf/2601.07477)

- 现有手工或自动设计的智能体工作流依赖端到端成败信号，无法精确定位瓶颈，导致优化低效、不可解释、难扩展。**Evaluation-Judge-Optimization-Update 四步闭环 **逻辑块抽象：将工作流**拆成三种可复用结构单元（顺序、循环、条件）**，兼顾代码表达力与可追踪性。 块级 Judge：**只对失败样本调用 LLM，按排序式责任分找出“最弱一块”，并收集失败日志**。 靶向优化：**LLM 优化器仅对该块执行 Add/Remove/Modify 三种动作，快速生成新工作流**。 Top-K 维护：多轮迭代保留高分候选，用 softmax 采样继续搜索。
## BAPO: Boundary-Aware Policy Optimization for Reliable Agentic Search

[https://arxiv.org/pdf/2601.11037](https://arxiv.org/pdf/2601.11037)

- 针对“基于强化学习的智能体搜索（agentic search）”提出一个关键可靠性缺陷：经过大规模 RL 训练后，模型几乎从不承认“我不知道”（IDK），即使在证据不足或推理已触及极限时仍会编造看似合理却不可靠的答案。为此，作者提出 Boundary-Aware Policy Optimization（BAPO），目标是在不牺牲准确率的条件下，让模型具备动态识别自身推理边界并适时给出 IDK 的能力，从而提升整体可靠性。核心思路是**只在“确实无法答对”的样本上奖励 IDK**，同时用**自适应调制器**防止模型把 IDK 当成捷径。在 GRPO 框架内增加两项机制： **组级边界感知奖励——仅当同一问题的多条 rollout 全部失败时才给 IDK 轨迹额外奖励，防止把可解问题误判为未知**。 **自适应奖励调制器——训练早期关闭 IDK 奖励避免捷径；平台期按 rollout 多样性动态开关，兼顾探索与边界意识。**
## Learn Like Humans: Use Meta-cognitive Reflection for Efficient Self-Improvement

[https://arxiv.org/pdf/2601.11974](https://arxiv.org/pdf/2601.11974)

- 人类仅需一次系统性的“元认知反思”即可将失败经验转化为后续避免错误的原则性知识（principle-based）与可复制成功的程序性知识（procedural），从而高效完成自我修正。论文提出 MARS（Metacognitive Agent with Reflective Self-improvement）框架，使智能体在单轮循环内完成自我进化： 通过“原则反思”提取通用规则，避免同类错误； 通过“程序反思”推导逐步策略，提高成功概率； 将两类反思合成轻量级、可插拔的提示增强，直接注入原始提示，无需在线多轮交互。
## Self-Compression of Chain-of-Thought via Multi-Agent Reinforcement Learning

[https://arxiv.org/pdf/2601.21919](https://arxiv.org/pdf/2601.21919)

- 大型推理模型在 RL 后训练中因稀疏二元奖励极易“过度思考”，生成冗长、重复的思维链，造成推理延迟高；传统全局长度惩罚会误删关键步骤，牺牲准确率。提出 Self-Compression via MARL——**训练阶段**引入三个参数共享智能体协同压缩，**推理阶段**仅保留推理智能体，零额外开销。

# Agent Applications

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Agent Applications
- 方法：agent, ai-for-science, generation, language, vision-language-model, reasoning, vision, reinforcement-learning
- 论文/报告：72 篇
- HeurekaBench: A Benchmarking Framework for AI Co-scientist【Research】
- **Higher-Order Knowledge Representations for Agentic Scientific Reasoning**【Research】
- SciIF: Benchmarking Scientific Instruction Following Towards Rigorous Scientific Intelligence【Research】
- SciFig: Towards Automating Scientific Figure Generation【Research】
- ⭐⭐Sci-Reasoning: A Dataset Decoding AI Innovation Patterns【Research】
- Beyond Static Tools: Test-Time Tool Evolution for Scientific Reasoning【Research】
- IDRBench: Interactive Deep Research Benchmark【Research】
- MMDeepResearch-Bench: A Benchmark for Multimodal Deep Research Agents【Research】
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## HeurekaBench: A Benchmarking Framework for AI Co-scientist【Research】

[https://arxiv.org/pdf/2601.01678](https://arxiv.org/pdf/2601.01678)

- 如何系统、严谨地评估大语言模型（LLM）智能体在真实科研场景中作为 AI co-scientist 的能力?
## **Higher-Order Knowledge Representations for Agentic Scientific Reasoning**【Research】

[https://arxiv.org/pdf/2601.04878](https://arxiv.org/pdf/2601.04878)

- **如何在真实科学语料上构建可更新、可解释的高阶知识表示，使大模型能够利用多实体共现约束进行可信、可验证的机理推理与材料发现。**
## SciIF: Benchmarking Scientific Instruction Following Towards Rigorous Scientific Intelligence【Research】

[https://arxiv.org/pdf/2601.04770](https://arxiv.org/pdf/2601.04770)

- 现有科学评测基准无法有效衡量大模型“科学指令遵循能力”的核心缺陷。作者构建跨学科基准 **SciIF**，通过 334 道大学水平题目 + 固定约束目录 + 可审计评分规则，独立打分“答案正确性”与“多约束合规性”，揭示模型在组合约束下出现的“合规崩溃”现象，并提供用于微调与强化学习的训练数据，证明严格科学约束训练可同步提升通用指令遵循与科学问答表现。
## SciFig: Towards Automating Scientific Figure Generation【Research】

[https://arxiv.org/pdf/2601.04390](https://arxiv.org/pdf/2601.04390)

- 解决**科研论文中高质量方法示意图（pipeline figure）的自动生成。**论文提出 **SciFig**——首个端到端多智能体系统，目标是将**一段方法描述文本**直接转化为**可编辑、符合出版标准、科学准确且视觉专业**的方法示意图，把耗时从“天”缩短到“分钟”，并建立可复现的评估基准。
## ⭐⭐Sci-Reasoning: A Dataset Decoding AI Innovation Patterns【Research】

[https://arxiv.org/pdf/2601.04577](https://arxiv.org/pdf/2601.04577)

- 解决“科学创新背后的推理过程缺乏结构化数据”这一核心问题。尽管人工智能领域创新速度极快，但研究者如何识别空白、整合前人工作并产生洞见的**思维轨迹**仍属黑箱。现有引用网络仅记录“谁被引用”，却丢失“为何被引用”的**推理细节**，导致无法对创新机制进行系统性或量化研究。**构建首个专门捕捉顶级 AI 研究（NeurIPS、ICML、ICLR 2023-2025 的 Oral/Spotlight 论文）背后推理链路的数据集 Sci-Reasoning，将隐式思维转化为可查询的“血统图”（lineage graphs）**，从而支持对创新模式的定量分析，并为自动化科研发现提供可学习的推理模板。
## Beyond Static Tools: Test-Time Tool Evolution for Scientific Reasoning【Research】

[https://arxiv.org/pdf/2601.07641](https://arxiv.org/pdf/2601.07641)

- 论文核心针对“AI for Science”场景下静态工具库的根本性缺陷： 科学问题空间开放、稀疏、异构，人工预置工具永远不完备； 面对新任务时，现有方法只能被动检索，无法主动创造所需计算原语。 为此**提出 Test-Time Tool Evolution（TTE）范式，将工具从“离线固定资源”转变为“推理时动态演化”的问题驱动产物**，使智能体在测试阶段即可自主合成、验证、迭代可执行函数，从而突破静态库覆盖天花板，实现真正的开放式科学推理。五模块闭环： **结构化任务分解 → 动态检索 → 生成式合成（含三重验证） → 原子拆解/去重/剪枝 → 运行时执行**。 实例化出 TTE-Zero（空库起步）与 TTE-Adapt（跨域迁移）两种模式。**构建 SciEvo：1 590 道科学题 + 925 个演化原子工具，覆盖物理/化学/数学/材料 25 子学科**，首次同时评测推理准确率与工具进化质量。
## IDRBench: Interactive Deep Research Benchmark【Research】

[https://arxiv.org/pdf/2601.06676](https://arxiv.org/pdf/2601.06676)

- 解决现有“深度研究智能体”评测基准的两大盲区：** 忽视动态交互** 现有基准默认用户意图一次性完全给定，仅对最终报告做静态评估；而真实研究目标常因信息不足或随探索演化，需要持续澄清与对齐。 忽视交互代价 现有基准不量化提问次数、token 开销等“人机协作成本”，导致无法权衡“对齐收益”与“用户负担”。 为此，**作者提出 IDRBench，首次系统评测交互式深度研究能力**，联合衡量： 交互收益：报告质量、结构覆盖、意图对齐 交互成本：提问轮次、token 消耗 通过可扩展的参考知识用户模拟器与模块化多智能体框架，揭示交互能显著提升质量与鲁棒性，同时暴露不同模型在效率上的显著差异。
## MMDeepResearch-Bench: A Benchmark for Multimodal Deep Research Agents【Research】

[https://arxiv.org/pdf/2601.12346](https://arxiv.org/pdf/2601.12346)

- 作者提出 MMDeepResearch-Bench（MMDR-Bench）： 140 项专家设计任务，覆盖 21 个领域，**分 Daily（轻量日常）与 Research（密集学术）两种场景，每项任务以图文 bundle 形式给出**，强制要求模型解读图像并引用外部来源。
## Beyond Single-shot Writing: Deep Research Agents are Unreliable at Multi-turn Report Revision【Research】

[https://arxiv.org/pdf/2601.13217](https://arxiv.org/pdf/2601.13217)

- 首次把“多轮用户反馈式报告修订”确立为深度研究智能体（DRA）的新评测维度，并发布统一基准 MR DRE；实验显示**当前 DRAs 虽能 90%+ 采纳反馈，却同时回退 16–27% 已满足内容与引用质量，且简单推理时补救无法根治，亟需训练/架构层面的根本改进**。
## Retrieval-Infused Reasoning Sandbox: A Benchmark for Decoupling Retrieval and Reasoning Capabilities【Research, ByteDance Seed】

[https://arxiv.org/pdf/2601.21937](https://arxiv.org/pdf/2601.21937)

- 提出 DeR2 基准，旨在**把“检索能力”与“推理能力”从端到端 RAG 或 agent 系统中解耦出来**，从而精确诊断大模型在**科学文献场景下的证据驱动推理瓶颈**。
## DSAEval: Evaluating Data Science Agents on a Wide Range of Real-World Data Science Problems【Research】

[https://arxiv.org/pdf/2601.13591](https://arxiv.org/pdf/2601.13591)

- DSAEval 构建了一个包含 285 个真实数据集、641 个任务的基准，并配套多模态沙盒环境与 LLM-as-Judge 三维评估协议，首次实现了对**数据科学智能体在统计推断、时序、NLP、CV、聚类等全域任务**上的细粒度、可执行、可复现的系统性评测。
## Paper2Rebuttal: A Multi-Agent Framework for Transparent Author Response Assistance【Research】

[https://arxiv.org/pdf/2601.14171](https://arxiv.org/pdf/2601.14171)

- 该论文旨在解决**学术同行评审过程中作者回复（rebuttal）阶段的核心痛点**：如何在紧迫的返稿时限内，撰写既**全面覆盖审稿人关切**、又**严格忠实于原稿内容**、且**可验证地引用内外部证据**的高质量反驳文本。现有方法要么直接生成文本，易产生幻觉、遗漏或不可验证的承诺；要么依赖多轮交互式提示，过程不透明、难以审计。为此，**作者提出首个多智能体框架 REBUTTALAGENT，将“写回复”重新定义为显式的决策与证据组织任务，通过可检查的阶段性工件（原子关切列表、证据包、策略计划）实现“先验证后写作”的透明流程，从而显著降低作者认知负担并确保最终措辞由作者完全掌控。**
## DeepSurvey-Bench: Evaluating Academic Value of Automatically Generated Scientific Survey【Research】

[https://arxiv.org/pdf/2601.15307](https://arxiv.org/pdf/2601.15307)

- 主要发现 高表面质量 ≠ 高学术价值：**多数生成综述 LLM 打分 >4，但学术价值均值 <4** 最大短板：**批判性分析**（50 % 低分综述缺陷集中于此） 基准可替代人工，已开源数据集与提示模板，支持“评估→诊断→优化”闭环。
## DSGym: A Holistic Framework for Evaluating and Training Data Science Agents【Research, James Zou】

[https://arxiv.org/pdf/2601.16344](https://arxiv.org/pdf/2601.16344)

- 现有数据科学评测基准无法可靠衡量大模型是否真正具备“基于真实数据文件进行端到端分析”的能力。
## PaperSearchQA: Learning to Search and Reason over Scientific Papers with RLVR【Research】

[https://arxiv.org/pdf/2601.18207](https://arxiv.org/pdf/2601.18207)

- 现有 RLVR 搜索智能体聚焦通用百科问答，缺乏面向**科学文献**的**可验证训练环境**与**大规模事实型问答数据**。构建 PaperSearchQA —— 1600 万 PubMed 摘要检索语料（BM25+e5 索引）, 6 万单实体事实问答（自动 LLM 生成+专家质控，50 % 改写增难），5000 测试集 + 1609 BioASQ-factoid 基准。
## DRPG (Decompose, Retrieve, Plan, Generate): An Agentic Framework for Academic Rebuttal【Research, Jiaxuan You】

[https://arxiv.org/pdf/2601.18081](https://arxiv.org/pdf/2601.18081)

- Decompose：LLM 将整篇审稿拆成原子级关切。 Retrieve：稠密检索为每关切召回最相关段落，长度↓75%。 Plan： – Idea Proposer 脱离论文生成多视角（澄清/辩护）； – 可学习 MLP 打分器选出“最被论文证据支持”的视角，测试准确率 98.6%。 Generate：Executor 用“关切+证据+视角”生成逐点反驳并拼接。
## OffSeeker: Online Reinforcement Learning Is Not All You Need for Deep Research Agents【Research】

[https://arxiv.org/pdf/2601.18467](https://arxiv.org/pdf/2601.18467)

- 核心主张：昂贵在线强化学习并非打造深度研究智能体的必要条件。解决方案 ① DeepForge 数据合成流水线 轻量实体扩展：LLM 随机名词 → 实时搜索 → 网页抽取长尾实体，无需维基转储。 实体图构建：递归采集属性与关系，生成 66 k 多跳、高不确定性问答对，平均 11.2 次工具调用/题。 ② **大规模离线数据集 66 k QA 对**、33 k SFT 轨迹、21 k DPO 偏好对，全部开源。 ③ 离线训练配方 基座 Qwen3-8B → 128 k 上下文 SFT → DPO 偏好优化，零搜索 API 消耗。
## ScholarGym: Benchmarking Deep Research Workflows on Academic Literature Retrieval【Research】

[https://arxiv.org/pdf/2601.21654](https://arxiv.org/pdf/2601.21654)

- 提出一个**静态、可复现**的仿真环境，用于在**57 万篇 arXiv 论文**上对“深度研究型”文献检索流程进行**确定性评估**，解决 live-API 带来的时序漂移与不可重复问题。
## Vision-DeepResearch: Incentivizing DeepResearch Capability in Multimodal Large Language Models【Research, MMLab】

[https://arxiv.org/pdf/2601.22060](https://arxiv.org/pdf/2601.22060)

- **Vision-DeepResearch** 提出一条“**长程-多尺度-多模态**”深度研究路线，把**视觉试错搜索**与**文本深度研究**无缝拼接，并通过**自动化数据工厂 + SFT + RL** 将整套能力内嵌到 8 B–30 B 参数的 MLLM，在 6 大事实密集型基准上取得新 SoTA。
## ⭐⭐Generating Literature-Driven Scientific Theories at Scale【Research】

[https://arxiv.org/pdf/2601.16282](https://arxiv.org/pdf/2601.16282)

- **从科学文献自动合成科学理论，主要发现 – 文献接地显著提升 经验支持（+49–75 %）、预测精度（0.90 vs 0.88）与 召回（+13–300 %）。 – 新颖优先定律风险高：精度跌至 0.34–0.61，但文献接地仍能将精度提高近一倍。 – 文献接地减缓理论饱和，非重复比例提升 3×。**
## Inference-Time Scaling of Verification: Self-Evolving Deep Research Agents via Test-Time Rubric-Guided Verification【Self-Evolving, Tencent】

[https://arxiv.org/pdf/2601.15808](https://arxiv.org/pdf/2601.15808)

- 如何在无额外训练的前提下，于推理阶段自动检测并纠正DRA的错误，从而 scalable 地提升其可靠性与准确率。**构建DRA 失败分类体系 **基于 2 997 步轨迹、555 个人工标注错误，归纳出 5 大类 13 子类失败模式，为后续验证提供细粒度 rubric。设计DeepVerifier 三模块框架 **分解模块**：利用“验证非对称性”把复杂任务拆成可检索的 yes/no 子问题。 **验证模块**：调用子智能体依次回答子问题，获取外部证据。 **评判模块**：综合证据给出 1–4 分评分与结构化反馈，驱动 DRA 重试。
## EvoConfig: Self-Evolving Multi-Agent Systems for Efficient Autonomous Environment Configuration【Self-Evolving, Tencent】

[https://arxiv.org/pdf/2601.16489](https://arxiv.org/pdf/2601.16489)

- 现有方法普遍忽视对配置过程中每一步动作的细粒度分析，导致难以处理级联依赖冲突、工具链失配、部分安装等复杂错误，最终造成环境构建失败。为此，作者提出 EvoConfig，**通过多智能体协作与在线自我演化机制，在有限交互预算内提升环境配置成功率**，并显著增强过程级错误诊断与修复能力。
## Timely Machine: Awareness of Time Makes Test-Time Scaling Agentic【AI Lab】

[https://arxiv.org/pdf/2601.16486](https://arxiv.org/pdf/2601.16486)

- 把“测试时扩展”从 **token 长度** 改写成 **wall-clock 秒数**，让大模型在 **工具延迟不可预测** 的智能体场景里，学会 **看时间办事**。
## AgencyBench: Benchmarking the Frontiers of Autonomous Agents in 1M-Token Real-World Contexts【Pengfei Liu】

[https://arxiv.org/pdf/2601.11044](https://arxiv.org/pdf/2601.11044)

- 现有评测体系无法衡量“长周期、多工具、需人类反馈”的真实世界任务中自主智能体的能力。AGENCYBENCH 通过构建 32 个真实场景、138 个递进式任务，并引入“用户仿真智能体 + Docker 远程沙盒 + 可执行评分脚本”的完全自动化评测管线，首次实现了在长周期、多工具、真实交付物上的可扩展、可复现评估，从而量化当前开源与闭源模型在复杂经济生产任务中的能力差距与行为差异。闭源平均 48.4 %，开源 32.1 %；GPT-5.2 最高 56.5 %，Qwen-3-235B 最低 27.0 % 反馈驱动自修正：GPT-5.2 相对提升 88.9 %，DeepSeek-V3.2 0 % 资源效率：Grok-4.1-Fast token 效率 37.2 % 居首，GPT-5.2 尝试效率 38.7 % 最高 工具行为指纹：Claude/GPT 偏好 shell，Gemini 独用记忆银行，GLM 重度重写文件 Scaffold 耦合：Claude-4.5-Opus 在原生 SDK 提升 +20.5 %，首次量化“主场效应”
## ProSoftArena: Benchmarking Hierarchical Capabilities of Multimodal Agents in Professional Software Environments【GUI】

[https://arxiv.org/pdf/2601.02399](https://arxiv.org/pdf/2601.02399)

- 首次系统回答“大模型到底会不会用 SolidWorks、ChemDraw、ANSYS 等专业工具”。L1 原子操作 → L2 单应用任务 → L3 跨应用管道 → L4 开放创意 → L5 项目级统筹（留待未来）能力断崖：最佳模型 L1 仅 45.9 %，L2 跌至 24.4 %，L3 全部 0 %。 软件差异：Excel/R 高 SR；视觉密集（PS/AI）（CAD/CAE）最低。 错误三大根因：任务规划失误、领域知识缺口、视觉定位失准。
## ShowUI-Aloha: Human-Taught GUI Agent【GUI】

[https://arxiv.org/pdf/2601.07181](https://arxiv.org/pdf/2601.07181)

- 如何让 GUI 代理在真实桌面环境里可靠地完成复杂、多步骤任务？ShowUI-Aloha **通过“录–析–学–执”闭环，把无结构的人类桌面演示自动转化为可复用、可泛化的结构化意图轨迹，再配以带记忆的多步规划器，实现单条演示即可指导代理在全新界面与任务变体上稳定完成 217/361 项 OSWorld 风格任务**，显著高于现有零样本基线。
## WebGym: Scaling Training Environments for Visual Web Agents with Realistic Tasks【GUI】

[https://arxiv.org/pdf/2601.02439](https://arxiv.org/pdf/2601.02439)

- 聚合 10 个基准 → 程序化分解 → 29.2 万任务，覆盖 12.7 万网站，难度 1–10 级。
## MobileDreamer: Generative Sketch World Model for GUI Agent【GUI】

[https://arxiv.org/pdf/2601.04035](https://arxiv.org/pdf/2601.04035)

- 移动 GUI 智能体普遍“短视”，仅依据当前屏幕做单步决策，在长程任务中易失败。构建轻量级世界模型，让智能体在真正点击前先“想象”未来界面。把 TSWM 当函数调用，递归展开深度 d、分支 M 的预测树，一次性比较多步未来，再选最佳动作。
## InfiniteWeb: Scalable Web Environment Synthesis for GUI Agent Training【GUI】

[https://arxiv.org/pdf/2601.04126](https://arxiv.org/pdf/2601.04126)

- 解决**GUI智能体训练环境稀缺**这一核心瓶颈。现有基准（MiniWoB++、WebArena、OSWorld等）依赖人工构建，规模与多样性不足，导致智能体容易过拟合。作者提出**INFINITEWEB**，首次实现**可扩展的自动化功能型Web环境合成**，为强化学习提供**无限、多样、可验证**的训练场景。
## CaMeLs Can Use Computers Too: System-level Security for Computer Use Agents【GUI】

[https://arxiv.org/pdf/2601.09923](https://arxiv.org/pdf/2601.09923)

- 针对“计算机使用智能体（Computer-Use Agents, CUA）”在开放环境中被提示注入（prompt injection）攻击时，控制流完整性（Control-Flow Integrity, CFI）无法保证的问题。首次证明：UI 工作流动态但**结构可预测，无需在线调整计划**。为此提出 Single-Shot Planning：让受信任的 planner 在任何潜在恶意内容被观测前，一次性生成带条件分支的完整执行图，从而 形式化保证 CFI：攻击者无法插入规划之外的任意动作； 同时保留足够效用：在 OSWorld 基准上，小模型提升 19%，大模型保留 57% 性能。
## GUI-Eyes: Tool-Augmented Perception for Visual Grounding in GUI Agents【GUI】

[https://arxiv.org/pdf/2601.09770](https://arxiv.org/pdf/2601.09770)

- 现有 GUI 自动化方法在视觉感知层面的两大瓶颈： 被动感知：**主流方案仅依赖单张静态截图，无法根据任务需求动态调整观察方式**。 数据饥渴：监督微调需要大量人工标注，且泛化到高分辨率、复杂或跨域界面时鲁棒性骤降。提出 GUI-Eyes 框架，**将“何时、是否、如何调用视觉工具（裁剪/缩放）”建模为可学习的强化学习策略**，使智能体在推理过程中主动获取任务相关且信息更丰富的视觉观察，从而在仅 3 k 标注样本条件下显著提升复杂界面的视觉定位精度与数据效率。
## MagicGUI-RMS: A Multi-Agent Reward Model System for Self-Evolving GUI Agents via Automated Feedback Reflux【GUI】

[https://arxiv.org/pdf/2601.13060](https://arxiv.org/pdf/2601.13060)

- 作者提出 MagicGUI-RMS，通过多智能体奖励模型系统将“评估-纠正-再训练”闭环自动化，实现： 细粒度、可解释的轨迹评价； 在线生成多样且难度均衡的奖励数据； 奖励信号驱动的策略持续优化。
## Compress to Focus: Efficient Coordinate Compression for Policy Optimization in Multi-Turn GUI Agents【GUI】

[https://arxiv.org/pdf/2601.11631](https://arxiv.org/pdf/2601.11631)

- 多轮交互中历史截图线性增长，现有截断或静态剪枝方法要么丢失长程依赖，要么破坏动作-空间对应关系，造成定位误差和训练低效。CASC（Coordinate-Aware Spatial Compression）** 每轮 rollout 实时收集坐标类动作（click/scroll 等）的预测与真值坐标 → 聚合生成 ROI bounding box → 裁剪历史图像，丢弃无关区域，最多压缩 55 % 令牌**。 Progressive Rollout 多 rollout 共享同一份“坐标增强历史”，下一轮用更新后的 ROI 继续裁剪，形成“越训越聚焦”的闭环。 Distance-Based Advantage 用平滑的“归一化坐标距离”替代 0/1 奖励，引导策略连续逼近真值位置，提升 grounding。
## EvoCUA: Evolving Computer Use Agents via Learning from Scalable Synthetic Experience【GUI, Meituan】

[https://arxiv.org/pdf/2601.15876](https://arxiv.org/pdf/2601.15876)

- EvoCUA，一种“经验自进化”的原生计算机使用智能体。静态数据集被动模仿无法捕获长时程 GUI 任务的因果动态与环境反馈，导致性能瓶颈；亟需从“数据规模”转向“经验规模”。将**可验证数据合成、高并发异步交互与迭代式经验优化**封装成自循环进化系统，形成“合成→ rollout → 策略更新”闭环。
## Continual GUI Agents【GUI】

[https://arxiv.org/pdf/2601.20732](https://arxiv.org/pdf/2601.20732)

- 传统在静态数据集上训练的 GUI 智能体因过度拟合旧分布， grounding 能力迅速退化，无法持续准确定位交互元素。作者首次系统地将 GUI 智能体置于**域漂移**（Mobile→Desktop→Web）与**分辨率漂移**（1080p→4K）两种持续学习场景，发现现有 SFT/RFT 方法均会因固守静态坐标或尺度线索而失效。为此，提出 GUI-Anchoring in Flux（GUI-AiF）框架，通过强化微调让智能体在界面持续变化中仍能稳定锚定交互点与元素区域，实现真正的持续 GUI 落地。
## GAIA: A Data Flywheel System for Training GUI Test-Time Scaling Critic Models【GUI】

[https://arxiv.org/pdf/2601.18197](https://arxiv.org/pdf/2601.18197)

- 提出 GAIA 数据飞轮系统，通过训练 Intuitive Critic Model（ICM）在动作执行前进行快速二值正确性判别，并借助 Test-Time Scaling 的 Best-of-N 机制筛选高置信动作，从而在不重新训练基础模型的前提下持续提升各类开源与闭源 GUI 智能体的任务成功率。
## OmegaUse: Building a General-Purpose GUI Agent for Autonomous Task Execution【GUI, Baidu】

[https://arxiv.org/pdf/2601.20380](https://arxiv.org/pdf/2601.20380)

- 面向**移动端与桌面端**的**通用 GUI 智能体**，通过**高质量数据构造**、**解耦两阶段训练**与**参数高效 MoE 架构**，在**跨平台 grounding** 与**长序列 navigation** 上取得新 SOTA，并发布离线评测套件 OS-Nav 填补中文安卓与 Ubuntu 桌面评估空白。
## OS-Marathon: Benchmarking Computer-Use Agents on Long-Horizon Repetitive Tasks【GUI, Microsoft】

[https://arxiv.org/pdf/2601.20650](https://arxiv.org/pdf/2601.20650)

- 长时域、重复性桌面工作流（如批量报销、成绩单录入）真实存在，却缺乏正式定义与评测基准；现有 CUA 在此类任务上因“逻辑失序-幻觉-长程漂移”而全面失败。
## GUIGuard: Toward a General Framework for Privacy-Preserving GUI Agents【GUI, Zhongguancun Academy】

[https://arxiv.org/pdf/2601.18842](https://arxiv.org/pdf/2601.18842)

- 首次把“截图上传→服务方偷窥”这一 GUI 代理核心隐私痛点形式化、可度量、可防护，提出三阶段框架 + 大规模基准 + 闭环实验，证明**隐私识别是最大瓶颈**，并给出可落地的本地-远程混合解决方案。
## ExpSeek: Self-Triggered Experience Seeking for Web Agents【Web】

[https://arxiv.org/pdf/2601.08605](https://arxiv.org/pdf/2601.08605)

- 解决现有 web agent 在动态交互过程中“经验注入”过于被动、无法随环境观测变化而精准调整的问题。ExpSeek 提出“自触发经验寻求”范式，核心目标包括：（1）**时机决策**：利用模型自身的步骤级熵值，通过 bootstrap 估计阈值区间，主动判断何时需要经验干预；（2）**内容定制**：构建以“行为-错误-指导”三元组形式组织的经验库，并在触发时由轻量级经验模型实时生成与当前上下文高度相关的步骤级指导。
## WebArbiter: A Principle-Guided Reasoning Process Reward Model for Web Agents【Web】

[https://arxiv.org/pdf/2601.21872](https://arxiv.org/pdf/2601.21872)

- 面向网页代理的原则诱导型推理过程奖励模型，现有 WebPRM 两类： – 标量式：单分、不可解释、易误判。 – 清单式：模板匹配，布局/语义一变即失效，常把表面正确动作判对。WebArbiter 模型：把奖励建模转成文本生成，**动态诱导原则→结构化推理→输出可审计判决**。
## DynaWeb: Model-Based Reinforcement Learning of Web Agents【Web】

[https://arxiv.org/pdf/2601.22149](https://arxiv.org/pdf/2601.22149)

- 如何在不与真实互联网交互的前提下，高效、安全地训练具备在线强化学习能力的网页智能体？作者提出**将“世界模型”从传统推理工具升级为在线 RL 的训练环境**，通过模型化互联网动力学，使智能体在“想象”中完成策略优化，从而把对真实环境的采样需求降到最低，同时保留在线 RL 的探索与信用分配优势。
## ⭐CUA-Skill: Develop Skills for Computer Using Agent【Computer Use, Microsoft】

[https://arxiv.org/pdf/2601.21123](https://arxiv.org/pdf/2601.21123)

- 缺乏可复用、结构化的“技能”抽象，使得现有系统只能把 GUI 交互看作扁平的原始动作序列，不得不针对每个任务从零开始重新摸索完整流程，导致长程任务错误累积、泛化性差、开发效率低。通过提出 CUA-Skill 技能库与对应的 CUA-Skill Agent，**论文首次系统性地将“人类如何用电脑”编码为可复用的结构化技能**，并在 WindowsAgentArena 上取得 57.5% 的最佳成功率，显著缩小与人类水平的差距。
## Do Not Waste Your Rollouts: Recycling Search Experience for Efficient Test-Time Scaling【Search】

[https://arxiv.org/pdf/2601.21684](https://arxiv.org/pdf/2601.21684)

- 如何让测试阶段的搜索具备“记忆”，把历史 rollout 中的有效中间结论和失败经验持续沉淀并复用，从而在不增加额外训练、不依赖外部监督的前提下，以相同或更少的推理算力获得更高的解题成功率。
## NitroGen: An Open Foundation Model for Generalist Gaming Agents【NVIDIA】

[https://arxiv.org/pdf/2601.02427](https://arxiv.org/pdf/2601.02427)

- 缺乏大规模、低成本、带动作标签的跨游戏数据集，导致通用游戏智能体难以像 LLM/VLM 一样通过“互联网预训练”获得广泛泛化能力。
## Deploy-Master: Automating the Deployment of 50,000+ Agent-Ready Scientific Tools in One Day【DP】

[https://arxiv.org/pdf/2601.03513](https://arxiv.org/pdf/2601.03513)

- 针对“科学软件难以大规模部署”这一结构性瓶颈，提出并验证了 DEPLOY-MASTER 系统，旨在把海量异构的开源科研工具从“存在但跑不起来”的状态转化为“可一键执行、可被 AI/Agent 直接调用”的规模化能力。**在一天内自动把 5 万余个异构开源科学仓库转化为容器化、可执行、可检索、可代理调用的标准化能力，并通过对大规模部署痕迹的分析，揭示科学软件可操作化的系统性障碍与改进路径。**
## Mem-Gallery: Benchmarking Multimodal Long-Term Conversational Memory for MLLM Agents【Memory】

[https://arxiv.org/pdf/2601.03515](https://arxiv.org/pdf/2601.03515)

- 解决**MLLM智能体在长期对话场景中缺乏系统化评估基准的问题**。作者提出 Mem-Gallery 基准，首次系统评估 MLLM 智能体在长期、多会话、多模态对话中的记忆能力，覆盖三大核心维度： 记忆提取与测试时适配（Memory Extraction & Adaptation） 记忆推理（Memory Reasoning） 记忆知识管理（Memory Knowledge Management） 通过 1 711 组高质量 QA 对、240 组多会话对话（平均 198 轮、跨 12 会话、含 1 003 张图像），揭示当前多模态记忆系统在信息保留、组织、推理与冲突处理上的关键瓶颈。**显式保留视觉信息平均提升 11.9 % F1；简单多模态 RAG 优于复杂架构。**
## Learning How to Remember: A Meta-Cognitive Management Method for Structured and Transferable Agent Memory【Memory】

[https://arxiv.org/pdf/2601.07470](https://arxiv.org/pdf/2601.07470)

- 长周期决策任务中LLM智能体如何有效积累、组织与复用记忆的核心难题？论文提出 Meta-Cognitive Memory Abstraction（MCMA），**将“记忆抽象”转化为可学习的元认知技能，通过可迁移的 Memory Copilot 显式学习如何结构化、抽象化与复用经验**，实现： 多结构、多层级的记忆组织（树、链、键值、自然语言及其嵌套）； 基于下游任务表现的偏好优化，自动选择最优抽象粒度； 在极端分布偏移场景下，直接迁移 Memory Copilot 本身，而非具体记忆内容，从而持续获得抽象与反思能力。论文提出 **Meta-Cognitive Memory Abstraction（MCMA）**，把“经验复用”从固定表征升级为**可学习的元认知技能**。核心思想是：不直接存储“该记住什么”，而是训练一个 **Memory Copilot** 学会“如何抽象、如何组织、如何复用”，并与任务策略彻底解耦。
## ⭐Conditional Memory via Scalable Lookup: A New Axis of Sparsity for Large Language Models【Memory, DeepSeek】

[https://arxiv.org/pdf/2601.07372](https://arxiv.org/pdf/2601.07372)

- 解决“用动态计算模拟静态知识检索”带来的低效问题。现有大模型（尤其是 MoE）通过条件计算（conditional computation）扩展容量，却**缺乏原生的“知识查找”原语**，只能让 Transformer 层反复重构本可用 O(1) 查表获得的局部模式（如实体、惯用语）。为此，作者提出： conditional memory 作为与 conditional computation 互补的新稀疏轴，**把“静态记忆”从“动态计算”中剥离**； **Engram 模块，将经典 N-gram 嵌入现代化，实现恒定时间、可扩展的稀疏查找**； Sparsity Allocation 问题，在总参数与计算量固定时，最优地把稀疏容量分给 MoE 专家与 Engram 记忆，发现 U 形缩放律。
## RealMem: Benchmarking LLMs in Real-World Memory-Driven Interaction【Memory】

[https://arxiv.org/pdf/2601.06966](https://arxiv.org/pdf/2601.06966)

- 现有对话记忆基准局限于闲聊或短任务，无法衡量“跨会话、多项目、状态持续演化”的真实场景。提出 RealMem 基准——11 个长周期项目、2 000+ 交错会话、1 415 条自然查询，覆盖静态检索、动态更新、主动对齐、时序推理四类任务；配套三阶段合成 pipeline（项目骨架→多智能体对话→闭环记忆/日程管理），使记忆随对话动态生长而非人工注入。
## MemGovern: Enhancing Code Agents through Learning from Governed Human Experiences【Memory】

[https://arxiv.org/pdf/2601.06789](https://arxiv.org/pdf/2601.06789)

- 自主代码智能体常陷“封闭世界”，仅依赖局部上下文或从零修 bug，忽视 GitHub 海量跨仓库人类调试经验；原始 issue–PR–patch 数据噪声大、异构性强，难以直接利用。提出 MemGovern 框架，分两阶段治理与利用经验： 经验治理——分层筛选活跃仓库与闭环三元组，统一标准化为“索引层+解析层”双层卡片 Ei=⟨Ii,Ri⟩ ，经 LLM 检查单质控，输出 135 K 高保真卡片。 智能体经验搜索——提供 Searching/Browsing 双原语，支持多轮查询-浏览-类比迁移；渐进式决策何时搜、搜多少、如何用，避免 RAG 式噪声注入。
## CloneMem: Benchmarking Long-Term Memory for AI Clones【Memory】

[https://arxiv.org/pdf/2601.07023](https://arxiv.org/pdf/2601.07023)

- 首次提出以“非对话数字痕迹”为核心的 AI Clone 长期记忆基准 CLONEMEM，揭示现有摘要式记忆因“保真-有效”权衡而难以追踪个体经验-情感-观点的纵向演化，并给出“证据保真、状态显式、可弃权”设计纲领。**首次系统检验 AI Clone 能否利用 1–3 年的非对话数字痕迹，持续追踪个体经验**、情感与观点的纵向演变，并揭示当前记忆机制在此任务上的根本缺陷。
## ⭐Dr. Zero: Self-Evolving Search Agents without Training Data【Self-Evolving】

[https://arxiv.org/pdf/2601.07055](https://arxiv.org/pdf/2601.07055)

- **零训练数据、多轮搜索场景下的自我进化框架**，使大模型仅凭外部搜索引擎即可迭代提升复杂推理与检索能力。双角色协同进化 **提问器 πθ：合成带难度的多跳 QA 对** **解答器 πϕ：多轮检索→答案，反馈成败信号** 二者共享同一基础 LLM，**仅依赖搜索引擎，零人工问答对**。
## Yunjue Agent Tech Report: A Fully Reproducible, Zero-Start In-Situ Self-Evolving Agent System for Open-Ended Tasks【Self-Evolving】

[https://arxiv.org/pdf/2601.18226](https://arxiv.org/pdf/2601.18226)

- 论文针对开放环境中任务分布持续漂移、外部监督信号稀缺的场景，提出“现场自演化”（In-Situ Self-Evolving）范式，解决传统智能体系统因依赖静态工具集或离线训练而导致的能力边界僵化且未知的问题。核心目标是在**零先验知识（zero-start）**条件下，让系统仅通过连续的任务交互流，自主合成、验证、复用并压缩工具，实现以下三点： 无需 Ground-Truth 即可持续扩展可复用的通用能力； 在跨领域任务中达到或超越闭源基线的性能； 提供可量化的演化收敛指标，替代传统“训练损失”的监控角色。
## ET-Agent: Incentivizing Effective Tool-Integrated Reasoning Agent via Behavior Calibration

[https://arxiv.org/pdf/2601.06860](https://arxiv.org/pdf/2601.06860)

- 提出 ET-Agent 框架，从数据与算法双路径渐进校准 TIR 行为。 Self-Evolving Data Flywheel：**迭代精炼正确轨迹、反思错误轨迹，显著扩大动作空间覆盖**。 Behavior Calibration Training： Action Space Exploration Fine-tuning（RFT）→ 保证多样性与格式正确； Iterative RL：Group-wise Pareto 采样保持梯度差异，课程式奖励逐步压缩冗余，收敛至最优轨迹。
## ToolGym: an Open-world Tool-using Environment for Scalable Agent Testing and Data Curation

[https://arxiv.org/pdf/2601.06328](https://arxiv.org/pdf/2601.06328)

- 手工整理并验证 5 571 个真实 API（204 款常用应用），统一为 MCP 格式； 自动任务引擎生成平均 28.5 轮、跨多应用、带 wild constraints 的长程指令； 可编程 State Controller 注入超时、限流、状态漂移、动态约束等故障，实现可复现的鲁棒性测试。9 款 SOTA 模型评估显示：– 所有模型规划得分高（7.7–8.6），但执行成功率差距最大 60 pp，呈现“规划-执行错位”；– **约束遵循是首要瓶颈，Format 平均仅 39 %**；– DeepSeek-v3.2 鲁棒性最佳，Recovery 90.6 %；– **高工具调用量≠高成功率**，gpt-4o-mini 51 次调用仍仅 Completeness 1.13。
## Current Agents Fail to Leverage World Model as Tool for Foresight【World Model】

[https://arxiv.org/pdf/2601.03905](https://arxiv.org/pdf/2601.03905)

- **当前基于视觉-语言模型（VLM）的智能体能否有效利用外部世界模型（world model）作为“工具”来增强其前瞻性认知（foresight）?**把大型视频/状态生成模型当成**可选项**插入 VLM 智能体决策循环。
## AgentOCR: Reimagining Agent History via Optical Self-Compression【Tongyi】

[https://arxiv.org/pdf/2601.04786](https://arxiv.org/pdf/2601.04786)

- AgentOCR 旨在解决**多轮交互场景下，基于LLM的智能体在RL训练与部署过程中，因历史文本上下文急剧膨胀而导致的 token 预算超限、推理延迟与内存开销激增**这一核心瓶颈。为此，AgentOCR 提出**将历史文本渲染为高密度视觉 token**，并引入两项关键机制： **分段光学缓存（segment optical caching）：通过哈希键值缓存已渲染的文本片段图像，避免重复渲染，实现 20× 以上的渲染加速**。 智能体自压缩（agentic self-compression）：让策略在每一步主动输出压缩率 ct ，并以“仅在成功时奖励压缩”的稀疏奖励机制进行 RL 训练，从而在任务性能与 token 效率之间自适应权衡。
## Terminal-Bench: Benchmarking Agents on Hard, Realistic Tasks in Command Line Interfaces【Code】

[https://arxiv.org/pdf/2601.11868](https://arxiv.org/pdf/2601.11868)

- 作者提出并实现了 Terminal-Bench 2.0：一个由 89 个高难度、真实场景任务组成的命令行环境基准。这些任务覆盖软件工程、系统管理、安全、科学计算等多领域，均配有容器化环境、人工编写的参考解和自动化测试，用于衡量代理在终端中完成专业级工作的能力。实验结果显示，即使是最强的闭源模型组合，任务解决率仍低于 65%，说明该基准对当前前沿模型仍具挑战性。
## CooperBench: Why Coding Agents Cannot be Your Teammates Yet【Code】

[https://arxiv.org/pdf/2601.13295](https://arxiv.org/pdf/2601.13295)

- 论文提出 CooperBench，系统评估代理在“潜在冲突但可兼容”的双人协作任务上的真实表现，并揭示三大协作缺陷：** 沟通失效：信息模糊、时机错位、幻觉频发。 承诺失效：口头承诺与最终代码不符，且无法被伙伴验证。 期望失效：对伙伴计划、观测、代码状态持有错误模型。** 最终结论： “协调诅咒”——**当前最强模型（GPT-5、Claude Sonnet 4.5）在双人协作场景下的成功率比单人完成两项任务低约 50%**；协作瓶颈不在编码能力，而在社会智能缺失。
## Endless Terminals: Scaling RL Environments for Terminal Agents【Code】

[https://arxiv.org/pdf/2601.16443](https://arxiv.org/pdf/2601.16443)

- 终端智能体强化学习训练环境稀缺。作者提出 **Endless Terminals**：一个**无需人工标注即可无限生成可验证终端任务的自主流水线**，并证明仅用极简 PPO 即可在生成的 3255 个任务上实现显著且可迁移的性能提升。
## SERA: Soft-Verified Efficient Repository Agents【Code】

[https://arxiv.org/pdf/2601.20789](https://arxiv.org/pdf/2601.20789)

- 作者提出 SERA（Soft-Verified Efficient Repository Agents），通过“软验证”与“模糊指令”两项简化策略，仅用监督微调即可在 40 GPU 天、约 2000 美元内训练出 32B 代码智能体，在 SWE-bench Verified 上达到 49.5 %（32 K 上下文）与 54.2 %（64 K 上下文）的 SOTA 开源成绩，并可在 1300 美元量级实现对单一私有代码库的特化，显著降低研究门槛。
## SWE-Replay: Efficient Test-Time Scaling for Software Engineering Agents【Code】

[https://arxiv.org/pdf/2601.22129](https://arxiv.org/pdf/2601.22129)

- 作者提出 SWE-Replay—— 一种无需任何 LLM-as-a-Judge 打分、可通用于任意现代智能体的高效测试时扩展技术。其**核心思想是： 把之前跑过的轨迹存档，下次以 0.5 概率“探索”（从头采样），以 0.5 概率“利用”（在存档轨迹的关键中间步骤处断点恢复并重新采样后续动作），通过回收-复用-分支降低重复计算，同时提升解空间覆盖度。**
## Numina-Lean-Agent: An Open and General Agentic Reasoning System for Formal Mathematics【Math】

[https://arxiv.org/pdf/2601.14027](https://arxiv.org/pdf/2601.14027)

- 如何构建一个无需任务专用训练、可灵活扩展、且能覆盖证明之外多种推理需求的通用形式化数学智能体?作者提出“直接用通用代码智能体充当形式化数学推理器”的新范式，并基于** Claude Code + Numina-Lean-MCP** 实现开源系统 Numina-Lean-Agent，以验证该范式在 高难竞赛题（Putnam 2025 12/12 全解）； 真实科研场景（Brascamp–Lieb 定理人机协同形式化）； 上的通用性、可扩展性与性能上限。
## Rethinking the Value of Multi-Agent Workflow: A Strong Single Agent Baseline

[https://arxiv.org/pdf/2601.12307](https://arxiv.org/pdf/2601.12307)

- 在**同质多智能体工作流**（所有 agent 共享同一基座 LLM）中，**一个单智能体通过多轮对话即可无损模拟整个协作过程**，并借助 KV-cache 复用显著降低推理成本；只有在**异质（多基座模型）**场景下，多智能体才具备不可替代的优势。
## LUMINA: Long-horizon Understanding for Multi-turn Interactive Agents

[https://arxiv.org/pdf/2601.16649](https://arxiv.org/pdf/2601.16649)

- 在“多轮、长程、目标驱动”的交互式任务中，究竟是哪些底层能力（规划、状态追踪、上下文压缩）成为限制LLM智能体性能的关键瓶颈？ 为此，作者提出一个反事实框架 LUMINA，**通过“Oracle 干预”精准地赋予智能体某一能力的完美版本**，量化该能力对最终成功率的边际贡献，从而指导未来模型与系统的改进方向。
## AgentDoG: A Diagnostic Guardrail Framework for AI Agent Safety and Security【AI Lab】

[https://arxiv.org/pdf/2601.18491](https://arxiv.org/pdf/2601.18491)

- 解决 AI 智能体（agent）在自主调用工具、与环境交互过程中产生的安全与安保风险缺乏系统诊断与可解释防护的问题。作者提出 AgentDoG 框架，目标是在轨迹粒度上实现： 统一且可扩展的三维风险分类法（风险来源、失效模式、现实危害） 可解释的诊断能力：不仅给出“是否不安全”，还能指出哪一步、哪一句、哪类工具触发了风险，并输出细粒度标签 大规模合成数据与评测基准（ATBench），覆盖 10k+ 工具、100k+ 轨迹，支持对未见工具的泛化评估。
## Agentic Very Long Video Understanding【Meta】

[https://arxiv.org/pdf/2601.18157](https://arxiv.org/pdf/2601.18157)

- 作者提出 EGAgent——**以“实体-场景-图”为中心的 agentic 框架**，将一周级 egocentric 视频转化为带时间戳的实体关系图，并配备规划代理、视觉搜索、音频搜索与图搜索工具，实现可解释、可组合、长程跨模态推理。
## ToolWeaver: Weaving Collaborative Semantics for Scalable Tool Use in Large Language Models

[https://arxiv.org/pdf/2601.21947](https://arxiv.org/pdf/2601.21947)

- ToolWeaver 通过**层次化组合编码**与**协同感知量化**，将词汇膨胀从 O(N) 降至 O(log N)，同时**让功能相关工具共享高层码**，从而把“稀疏工具共现”转化为“稠密码字共现”，使模型在预训练阶段即可习得协同语义。采用 L 层残差量化（RQ-VAE）。
## ⭐Optimizing Agentic Workflows using Meta-tools

[https://arxiv.org/pdf/2601.22037](https://arxiv.org/pdf/2601.22037)

- LLM 智能体在多步任务中反复调用细粒度工具并伴随大量中间推理，导致推理费用高、延迟长、易失败。提出 AWO 框架——**离线将高频工具序列横向合并后纵向抽取为单一确定性 meta-tool**，在线一次性执行，跳过中间 LLM 调用。
## The Script is All You Need: An Agentic Framework for Long-Horizon Dialogue-to-Cinematic Video Generation【Tencent】

[https://arxiv.org/pdf/2601.17737](https://arxiv.org/pdf/2601.17737)

- 首个端到端“对话→电影级长视频”智能体框架，核心思想是 **“脚本即所需”。**
## DeepPlanning: Benchmarking Long-Horizon Agentic Planning with Verifiable Constraints【Qwen】

[https://arxiv.org/pdf/2601.18137](https://arxiv.org/pdf/2601.18137)

- 提出 DeepPlanning 基准，含： 旅行规划 120 任务（中英双语）——分钟级日程，需订交通、酒店、景点、餐厅，满足时空-预算硬约束。 购物规划 120 任务——多商品+优惠券组合，需全局最低价且库存/尺码/时效合规。

# AI for Science

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：AI for Science
- 方法：agent, language, bio-molecular, cross-modal, protein-language-model
- 论文/报告：7 篇
- SeedFold: Scaling Biomolecular Structure Prediction【ByteDance Seed】
- SeedProteo: Accurate De Novo All-Atom Design of Protein Binders【ByteDance Seed】
- Open World Knowledge Aided Single-Cell Foundation Model with Robust Cross-Modal Cell-Language Pre-training【BGI】
- PCEvo: Path-Consistent Molecular Representation via Virtual Evolutionary
- EnzyPGM: Pocket-conditioned Generative Model for Substrate-specific Enzyme Design
- Demystifying Data-Driven Probabilistic Medium-Range Weather Forecasting【NVIDIA】
- BioAgent Bench: An AI Agent Evaluation Suite for Bioinformatics
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## SeedFold: Scaling Biomolecular Structure Prediction【ByteDance Seed】

[https://arxiv.org/pdf/2512.24354](https://arxiv.org/pdf/2512.24354)

- 如何有效扩大（scale）蛋白质折叠模型的容量？现有主流折叠模型（如 AlphaFold 系列）主要堆叠 Pairformer 深度，但 SeedFold 通过系统实验发现，隐藏维度（pair representation width）才是限制表达能力的真正瓶颈。将维度从 128 提升至 512 可显著降低全局 RMSD 并提升局部 lDDT，而继续加深网络仅带来边际收益。论文提出 **Linear Triangular Attention**，利用线性注意力近似 softmax。AlphaFold3 舍弃了 AlphaFold2 的等变归纳偏置，导致小数据场景下泛化不足。SeedFold 通过大规模知识蒸馏将训练集扩充至 **26.5 M 样本**（实验结构 0.18 M 的 147 倍），结合 AFDB 与 MGnify 双源蒸馏，显著提升了模型对长序列、低同源样本的鲁棒性。
## SeedProteo: Accurate De Novo All-Atom Design of Protein Binders【ByteDance Seed】

[https://arxiv.org/pdf/2512.24192](https://arxiv.org/pdf/2512.24192)

- SeedProteo 把“全原子一致性”与“长程可扩展性”拆解为三个可操作的子问题，并给出对应的技术模块。整体思路是：**将 AlphaFold3 折叠网络“逆向”改造成生成网络****，用自条件信号强制几何-序列耦合，用 MRF 全局解码取代局部原子判别，用多阶段训练保证多链 binder 场景泛化**。保留 AF3 的 Embedder + Pairformer + 坐标扩散模块，但把“序列+MSA”输入通道替换为： – 高斯噪声坐标直接经 Embedder 转成 1D 单链特征； – Pairformer 层数从 48 减至 12，降低 O(L³) 复杂度，使噪声依赖的编码器仍可训练。
## Open World Knowledge Aided Single-Cell Foundation Model with Robust Cross-Modal Cell-Language Pre-training【BGI】

[https://arxiv.org/pdf/2601.05648](https://arxiv.org/pdf/2601.05648)

- 针对单细胞多组学（尤其是 scRNA-seq）数据与文本知识融合时的两大核心缺陷——文本知识贫乏与跨模态噪声敏感——提出统一解决方案。作者提出 OKR-CELL： 数据层面：用 LLM+RAG 生成富含开放世界知识的细胞文本描述，并通过可靠性筛选抑制幻觉。 方法层面：设计 Cross-modal Robust Alignment (CRA) 目标，显式评估正样本对可靠性、动态加权负样本、引入课程学习与动量记忆库，实现抗噪的跨模态对齐。
## PCEvo: Path-Consistent Molecular Representation via Virtual Evolutionary

[https://arxiv.org/pdf/2601.19257](https://arxiv.org/pdf/2601.19257)

- 解决**小样本场景下分子表示学习的泛化瓶颈**。具体而言，传统“静态端到端”范式把每个分子视为孤立样本，仅在观测到的分子结构上直接回归性质标签；当带标签数据极度稀缺时，模型容易拟合数据集特有相关性，导致泛化性能剧烈下降。为此，作者提出 PCEvo，**通过虚拟演化路径将稀疏的端点监督信号分解为一系列化学可行的中间编辑步骤，并引入路径一致性约束，迫使模型对同一对分子沿不同路径预测的累计性质变化保持一致**，从而显式学习“局部结构编辑如何累积为全局性质变化”的规律，提升小样本条件下的鲁棒性与预测精度。
## EnzyPGM: Pocket-conditioned Generative Model for Substrate-specific Enzyme Design

[https://arxiv.org/pdf/2601.19205](https://arxiv.org/pdf/2601.19205)

- 解决**底物特异性酶设计**中“口袋-底物相互作用难以建模”的核心瓶颈。具体而言，现有生成式模型虽然能设计功能蛋白，却普遍忽略酶催化所依赖的**底物结合口袋（substrate-binding pocket）**，导致生成的酶无法与给定底物形成精确、无冲突的结合构象，从而丧失催化活性。为此，作者提出 EnzyPGM，首次将“口袋-底物联合建模”显式纳入酶生成流程。
## Demystifying Data-Driven Probabilistic Medium-Range Weather Forecasting【NVIDIA】

[https://arxiv.org/pdf/2601.18111](https://arxiv.org/pdf/2601.18111)

- 无需任何领域专用约束或复杂训练技巧，仅通过**可扩展的通用 Transformer 骨干 + 简单双线性下采样隐空间 + 历史条件局部上投影解码器****即可在 0.25° 分辨率下取得 SOTA 概率预报技巧，并兼容任意概率估计器（随机插值、扩散、CRPS）。简言之，论文试图**“去神秘化”**——证明**“把通用大模型 scale 好，就足以在中期概率天气预报任务上全面超越 IFS 与现有最强开放基线 GenCast
## BioAgent Bench: An AI Agent Evaluation Suite for Bioinformatics

[https://arxiv.org/pdf/2601.21800](https://arxiv.org/pdf/2601.21800)

- 针对“如何可信地评估大语言模型（LLM）在真实生物信息学流程中的代理能力”这一核心问题，提出并验证了 BioAgent Bench 基准。作者构建了一个**包含 10 个典型生物信息学流程（RNA-seq、变异检测、宏基因组等）**的端到端评测套件，并引入 LLM 评委自动打分与多类扰动实验。

# Survey

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Survey
- 方法：agent, language, reasoning
- 论文/报告：3 篇
- Agent-as-a-Judge
- **Toward Efficient Agents: Memory, Tool learning, and Planning****【AI Lab】**
- Agentic Reasoning for Large Language Models
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

## Agent-as-a-Judge

[https://arxiv.org/pdf/2601.05111](https://arxiv.org/pdf/2601.05111)

- 作者提出并系统梳理“Agent-as-a-Judge”范式：让具备规划、工具调用、多 agent 协作与持久记忆的 agent 评委主动分解任务、收集证据、验证正确性、迭代修正，从而实现更稳健、可验证、细粒度的评估。论文通过构建统一分类法、回顾方法论、调研跨领域应用、剖析前沿挑战，为下一代可自主演化的 agent 评审系统提供路线图。
## **Toward Efficient Agents: Memory, Tool learning, and Planning****【AI Lab】**

[https://arxiv.org/pdf/2601.14192](https://arxiv.org/pdf/2601.14192)

- 如何让大语言模型智能体在真实部署中既好用又省资源？
## Agentic Reasoning for Large Language Models

[https://arxiv.org/pdf/2601.13247](https://arxiv.org/pdf/2601.13247)

- **传统大模型在封闭、静态环境中表现良好，但在开放、动态、需持续交互的真实场景中推理能力受限，****缺乏统一框架来指导其如何“像智能体一样”持续思考、行动与学习****。**
