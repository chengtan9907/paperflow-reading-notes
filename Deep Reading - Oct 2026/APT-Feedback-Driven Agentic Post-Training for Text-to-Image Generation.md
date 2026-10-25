---
user_id: "cheng tan"
paper_id: 15910
arxiv_id: "2610.18097v1"
title: "APT: Feedback-Driven Agentic Post-Training for Text-to-Image Generation"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18097v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18097v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T01:57:11"
---
# APT: Feedback-Driven Agentic Post-Training for Text-to-Image Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic post-training · text-to-image generation · reinforcement learning · recursive self-improvement

## 一句话总结

APT（Agentic Post-Training）提出一个由评估集生成、模型诊断和RL数据生成三个智能体协同驱动的全自动文本到图像后训练框架，通过“评估→诊断→更新”的迭代反馈回路，让训练分布随当前模型的薄弱能力自适应演化，从而摆脱人工构造训练数据与固定训练分布的传统范式。

## 摘要

> Post-training is crucial for improving text-to-image models, yet existing pipelinesarXiv:2610.18097v1 typically rely on manually curated training data and fixed training distributions.
> We introduce APT (Agentic Post-Training), a fully automated framework that
> adaptively post-trains a base model according to its evolving capability profile.
> APT coordinates three agents to construct evaluation sets from user requirements,
> diagnose model-specific strengths and failure modes, and generate reinforcement
> learning data with an adaptive prompt distribution that emphasizes underperform-
> ing capabilities. By repeatedly evaluating, diagnosing, and updating the generator,
> APT lets the current model’s behavior shape its next training tasks, forming a feed-
> back loop inspired by recursive self-improvement. For general-purpose settings
> without user-specified evaluation scenarios, we further develop an evaluation har-
> ness that enables broad and diverse evaluation set generation. Experiments across
> multiple base models, benchmarks, and generation architectures demonstrate that
> APT is model-agnostic, benchmark-agnostic, architecture-agnostic, and fully au-
> tomated.
> ∗Work done during internship in MSRA. † Corresponding author.
> 1

Q1: 这篇论文试图解决什么问题？

1. 核心问题：现有 T2I 后训练流水线依赖人工识别模型弱点、人工收集或合成对应训练数据、再凭经验反复调整数据分布。这种工作流劳动密集、难以规模化，且无法随模型、基准和用户需求的多样化而扩展。
2. 更根本的问题：后训练应当模型自适应（model-adaptive）。不同基础模型的能力分布差异显著：一个模型文本渲染强但计数或空间关系弱，另一个可能相反。因此对模型 A 有效的后训练数据集对模型 B 不一定最优。把每个基础模型 M 视为在生成任务上诱导出自身的能力与失败分布 pM，有效的后训练应定位于当前模型表现欠佳之处。
3. 现有方法把训练分布固定在训练前一次性设定，缺少“根据模型当前行为调整下一轮训练任务”的反馈机制。
4. 用户需求与评估场景的多样性问题：当用户没有明确指定评估场景时，如何保证评估集广泛、多样且覆盖全面，是 APT 要解决的另一个子问题。
5. 隐含的方法论缺口：后训练数据有效性高度依赖目标模型自身，但既有工作没有系统地把“模型当前弱点”作为训练数据生成的条件；APT 用智能体协调把这一条件显式建模，并形成循环自我改进闭环。

Q2: 有哪些相关研究？

1. T2I 基础模型进展：正文引言列出视觉质量、提示跟随、文本渲染、组合生成与可控性方面的重要进展（如 Black Forest Labs 2025、OpenAI 2026、Google 2026、SeedDance 2026、Zhao et al. 2026 等）。这些工作为后训练提供了强基础模型，但大规模预训练本身不能消除特定失败模式。
2. 后训练（Post-training）在 T2I 中的已有工作：Xu et al. 2026、Chen et al. 2026a、Jiang et al. 2026a、Feng et al. 2026b、Geng et al. 2025 等。这些流水线通常人工构造训练分布：识别弱点、收集合成数据、依据经验调整数据分布。APT 的定位是把这一过程自动化。实验中的 Lens（Chen et al. 2026a）本身即后训练产物的一个代表。
3. 强化学习（RL）用于生成模型后训练：论文以 DiffusionNFT（Zheng et al. 2026）为默认 RL 算法，属于扩散模型直接偏好/奖励优化的技术路线；同时涉及人工 RL 数据（handcrafted RL data）作为对比基线。
4. 多模态评估器与视觉-语言模型作为评判（judge）：诊断与训练共用 GPT-4.1-mini 作为视觉-语言二元评估器，这与近年来 VLM-as-a-judge 评估范式一致；评估集采用 prompt–rubric 对结构，比单一分数更细粒度。
5. 递归自我改进（Recursive Self-Improvement, RSI）：论文明确把 APT 与 RSI 联系起来，引用 Zelikman et al. 2023、Zweiger et al. 2026，将 RSI 思想引入 T2I 生成：模型当前行为决定下一轮训练任务。
6. 评估基准：GenEval（Ghosh et al. 2023）、GenEval2（Kamath et al. 2025）、OneIG-EN（Chang et al. 2026）、DPG（Hu et al. 2024）、CVTG-2K（Du et al. 2025），分别覆盖组合生成、提示跟随与文本渲染。APT 的关键卖点是训练提示不直接使用任何这些基准，一次 APT 运行后在同一模型上评测全部五个基准。

Q3: 论文如何解决这个问题？

1. 总体流程：评估集生成智能体在后训练前一次性调用，构造固定评估集 E，元素为 prompt–rubric 对；用户可指定评估范围，未指定时使用通用评估 harness 保证广覆盖；随后进入迭代后训练循环。
2. 逐轮学习目标：第 t 轮开始时当前生成器为 Mt，提示为 p、生成图像为 x；Dt 为该轮训练提示数据集，(p, R)∼Dt 表示采样 prompt–rubric 集对；对提示 pi，Ri 为相关 rubric 集，ρ∈Ri 为单条评估标准；诊断与训练使用 GPT-4.1-mini 作为视觉-语言评估器 V，其二元输出 V(x,p,ρ)∈{0,1}，1 表示图像满足该标准。每轮训练数据生成不是固定的，而是以诊断报告的失败模式为条件。
3. 诊断智能体（Diagnosis Agent）：评估当前模型在评估集 E 上的表现，跨多样生成能力识别失败模式，产出细粒度评估报告，传给 RL 数据生成智能体指导后续 RL 更新的训练数据构造。
4. RL 数据生成智能体（RL Data Generation Agent）：根据诊断报告生成 RL 训练数据，采用自适应提示分布，强调当前模型欠佳的能力；提示与 rubric 结构保证训练信号是任务感知、细粒度的。
5. 训练更新：所有模型用 LoRA（rank 64、α 对应设置），默认 RL 算法为 DiffusionNFT；FLUX.1 [dev] 后训练 200 优化步，其余模型 400 步；推理时统一评估设置。
6. 通用性设计：同一套智能体工作流跨所有模型直接使用，不引入模型特定提示集、奖励设计或超参数搜索；宣称（1）基准无关：不需要为单个基准独立设计提示集，一次 APT 运行可在多个 T2I 基准上评估；（2）模型无关：同一智能体工作流适用于不同基础模型；（3）架构无关：适用于扩散架构（Lens、SD3.5-Medium、FLUX.1 [dev]、LongCat-Image）与自回归架构（Janus-Pro）。
7. 与 RSI 的关系：每个更新后循环重复，训练分布随模型演化弱点自适应变化，形成递归自我改进式反馈闭环；论文认为这是向 T2I 递归自我改进迈出的一步。

Q4: 论文做了哪些实验？

1. 基础模型覆盖：四个扩散模型 Lens（Chen et al. 2026a）、SD3.5-Medium（Esser et al. 2024）、FLUX.1 [dev]（Black Forest Labs 2024）、LongCat-Image（Team et al. 2025）；另加自回归图像生成器 Janus-Pro（Chen et al. 2025）验证架构无关性。所有模型使用完全相同 APT 智能体工作流，不引入模型特定提示集、奖励设计或超参数搜索。
2. 基准与指标：五个广泛使用的 T2I 基准——GenEval、GenEval2、OneIG-EN、DPG、CVTG-2K，覆盖组合生成、提示跟随、文本渲染；因各基准分数尺度不同，把 GenEval、OneIG-EN、CVTG-2K 重标到 [0,100] 后计算归一化平均；没有一个基准被用于构造训练提示——单次 APT 运行产出单一后训练模型，然后在五个基准上全部评测，直接检验基准无关性。
3. 实现细节：默认 RL 算法为 DiffusionNFT（Zheng et al. 2026）；LoRA rank 64、α 对应配置；计算步数设置：除非另有说明，FLUX.1 [dev] 后训练 200 步，其他模型 400 步；推理时统一评估程序。
4. 对比与消融（根据证据可推断的部分）：
 - 对比 APT 与人工构造 RL 数据的后训练，报告平均性能更高；
 - 迁移研究（transfer studies）：用针对源模型生成的 RL 数据训练目标模型，考察数据与模型的匹配度；证据片段显示存在 RL 数据由不同源模型生成、对角线为标准 APT 设置（RL 数据为同一目标模型自适应生成）的表格，且对角线设置在每个行内取得最佳平均性能——说明模型特定训练数据的重要性；
 - 消融研究支持迭代反馈的价值（模型特定训练数据和迭代反馈两个因素分别被验证）。
5. 评估集规模：评估集生成智能体把高层评估需求转换为包含 2,609 个 prompt–rubric 对的多样化评估集（合理推断该数值为文中报告规模）。
6. 可扩展性/自动化验证：跨模型、跨基准、跨架构的统一流程本身就是对“完全自动化”的实验证据。

Q5: 发现了什么实验现象？

1. 主结果：在多个模型、多个基准上，APT 的平均性能高于在人工构造 RL 数据上后训练的基线，表明自适应数据分布优于静态人工分布。
2. 迁移/对角线现象：在“源模型生成 RL 数据→目标模型训练”的迁移设置中，对角线（RL 数据为同一目标模型自适应生成，即标准 APT）在每一行内都达到最佳平均性能；这从经验上支持“后训练数据应模型特定”的核心论点——用其他模型的数据训练会带来性能损失。
3. 迭代反馈的价值：消融支持迭代反馈的作用，即当前模型行为进入下一轮训练数据构造能够持续改善性能；去掉反馈或固定训练分布后收益下降（合理推断，原文消融的具体数值证据未在检索片段中完整呈现，需要回原文确认）。
4. 模型无关与架构无关的稳健性：同一套流程不加改动地适用于四个扩散模型和一个自回归模型，说明方法对模型族和架构差异不敏感；这是对智能体工作流通用性的实证。
5. 基准无关性：单次 APT 训练不使用任何一个评估基准的提示，却在五个基准上同步提升，说明评估集生成智能体+通用 harness 构造的 prompt–rubric 集能跨基准迁移。
6. 反直觉点（合理推断）：在没有直接使用目标基准提示的情况下获得跨基准提升，支持“rubric 级细粒度诊断+弱点定向数据”比“基准导向数据”更泛化；同时，模型特定数据优于“更强模型数据”的现象提示数据质量之外还有模型-数据匹配度因素。
7. 证据缺口：检索片段未包含各基准具体分数表格、消融数值和负例；以上关于具体幅度和负结果的描述依赖论文表文的合理推断，数值需回原文确认。

Q6: 有什么可以进一步探索的点？

1. 更强的评估器与更细粒度的 rubric 设计：当前使用 GPT-4.1-mini 作为视觉-语言评估器，未来可探索更强或专门训练的评估模型、连续/分级评分替代二元输出、自动化 rubric 生成与质量校验。
2. 端到端可微的反馈闭环：当前诊断报告由 VLM 文本形式传递，未来可探索把评估信号直接接入奖励建模或可微训练目标，减少中间表示的信息损失。
3. 更多 RL 算法与训练目标适配：论文默认 DiffusionNFT，可探索其他 RL/偏好优化算法在同一反馈框架下的表现与交互。
4. 扩展到其他生成模态与任务：把 APT 从 T2I 迁移到文生视频、文生3D、多模态编辑、统一多模态生成等；不同模态的失败模式空间与评估 rubric 结构需要重新设计。
5. 用户定制评估范围（user-specified evaluation scope）的深入机制：如何从用户自然语言需求半自动/全自动地生成覆盖良好、无偏的评估集，以及如何处理需求冲突或歧义。
6. 规模化与成本优化：多个智能体反复调用 GPT-4.1-mini 和 RL 训练，计算与 API 成本高；可探索蒸馏小评估器、减少评估轮次、课程式调度等。
7. 理论化与边界条件：为什么模型特定 RL 数据优于跨模型数据？数据-模型匹配度的量化指标、APT 收敛性、迭代轮数与性能 Plateau 的关系、过拟合评估集风险。
8. 与社会/安全对齐相关评估：把 rubric 扩展到安全、偏见、有害内容、版权等维度，用反馈闭环持续修正生成模型的价值观对齐问题。
9. 递归自我改进的更完整实现：当前是“固定评估集+迭代训练”，未来可让评估集本身也随模型演化自动扩张，实现真正的自举式改进和自我验证，避免评估集饱和。
10. 基准与评估协议设计：探索如何在无需人工提示集的情况下构建覆盖组合生成、提示跟随、文本渲染之外的更广能力维度（如空间关系、计数、常识、风格一致性）的新基准协议。

Q7: 总结一下论文的主要内容

APT 的核心主张是：T2I 后训练不应依赖人工构造的固定数据分布，而应模型自适应——由当前模型自身的行为决定下一轮要训练什么。论文先论证不同基础模型各自诱导不同的能力-失败分布 pM，因此对某个模型有效的后训练数据对另一模型不一定有效；而现有流水线靠人工识别弱点、收集数据、凭经验调分布，既不规模化也不模型自适应。针对这一问题，APT 设计了三个智能体：评估集生成智能体把高层评估需求（用户指定或通用 harness）转换为固定的评估集 E，其中每个评估例是 prompt–rubric 对以便细粒度诊断；诊断智能体用 GPT-4.1-mini 评估当前模型在 E 上的表现，产出覆盖多样生成能力的失败模式报告；RL 数据生成智能体以该报告为条件生成训练提示-标准对，用自适应提示分布突出欠佳能力。训练用 DiffusionNFT + LoRA 完成一轮 RL 更新后，循环重复，形成受 RSI 启发的“评估→诊断→更新”反馈闭环。方法层面，APT 强调三个通用性：模型无关、基准无关、架构无关，且完全自动化。实验上，论文在 Lens、SD3.5-Medium、FLUX.1 [dev]、LongCat-Image 四个扩散模型和 Janus-Pro 一个自回归模型上，用完全相同的智能体流程后训练（无模型特定提示集、奖励设计、超参搜索），在 GenEval、GenEval2、OneIG-EN、DPG、CVTG-2K 五个基准上评测，且训练提示不包含任何基准提示，单次运行直接跨五基准验证。结果显示 APT 平均性能高于人工构造 RL 数据后训练；迁移研究显示针对不同源模型生成的 RL 数据用于目标模型时，对角线设置（同一目标模型自适应数据）在每一行平均性能最佳，支持模型特定训练数据的价值；消融支持迭代反馈的价值。结论把 APT 定位为反馈驱动任务自适应的自动化后训练方向，以及朝 T2I 递归自我改进迈出的实践一步。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：智能体（agent）方向直接相关：论文把评估、诊断、数据生成三者建模为智能体，并用多智能体协调实现自动化后训练闭环，可作为“LLM/VLM as agent”在生成模型训练侧的应用范例。

## 基本信息

- 作者：Sirui Zhang, Fangyun Wei, Dong Chen, Jinjing Zhao, Yitong Wang, Yunuo Chen, Baining Guo, Zheng-Jun Zha
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18097v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（background/method/results/conclusion 多字段命中），并对其与 heuristic_draft 进行了交叉校准；未检索到的数值细节已在相应字段标注需回原文确认。
