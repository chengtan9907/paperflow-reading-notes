---
user_id: "cheng tan"
paper_id: 12677
arxiv_id: "2609.25001v1"
title: "GameHorizon Suite: Multi-Horizon Data and Evaluation in Gameplay"
publish_date: "2026-09-21"
pdf_url: "https://arxiv.org/pdf/2609.25001v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-24T01:09:54"
---
# GameHorizon Suite: Multi-Horizon Data and Evaluation in Gameplay

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：game benchmark · multi-horizon instruction · video understanding · agent evaluation

## 一句话总结

GameHorizon Suite 通过一条可扩展的多时间跨度自动标注流水线（Annotator）、一个 5,000 小时/21 款 AAA 游戏/100 名人类专家玩家的时间对齐数据集（Data），以及离线可复现 + 在线逐步双轨基准（Bench），在不同时间跨度上为多类模型族建立了统一的 gameplay 能力标尺，并用 47 个模型、超过一百万次模型调用完成了首次系统性普查。

## 摘要

> Modern video games provide a measurable testbed for AI models, combining abilities of visual understanding, instruction decomposition, goal planning, and precise action control over multiple temporal horizons. Existing datasets and benchmarks, however, either cover a narrow range of games, lack language instructions, or rely on high-variance online rollouts. To address these challenges, we introduce GameHorizon, a unified data and evaluation suite that measures gameplay capabilities at different horizons for diverse model families. GameHorizon Suite consists of three components. First, GameHorizon-Annotator is a scalable and automated annotation pipeline for multi-horizon instructions. Second, utilizing the pipeline, we construct GameHorizon-Data, the first large-scale AAA gameplay dataset with temporally aligned videos, player actions, and multi-horizon instructions. It comprises 5,000 hours of recordings from 21 games, collected by 100 human expert players. Third, we build GameHorizon-Bench with reproducible offline and stepwise online testing. The offline track enables reproducible evaluation using thousands of standardized questions organized into three primary tasks and a series of diagnostic variants, while the online track tests whether offline scores reflect actual gameplay capabilities and localizes failures to specific steps within long-horizon gameplay. Based on our GameHorizon Suite, we evaluate 47 models through more than one million model invocations, revealing a meaningful hierarchy of task difficulty and pronounced differences in model capabilities. Our work can provide a standardized yardstick for evaluating gameplay capabilities across horizons and model families. We will release our dataset, annotator, and benchmark to facilitate future research.

Q1: 这篇论文试图解决什么问题？

1) 表层问题：缺少一个「跨时间跨度 + 可复现 + 带语言指令」的 gameplay 评测标准。
作者明确指出既有数据集/基准的三个缺陷：(a) 覆盖的游戏范围狭窄；(b) 缺少语言指令；(c) 依赖高方差的在线 rollout。这三者分别对应评测效度、评测维度完整性和评测信度三个不同的方法学问题，而不是简单的「数据不够大」。

2) 第一个缺陷的方法学后果：游戏数量少容易导致模型在特定画面风格、UI 布局、规则体系上过拟合，评测结论难以外推到「通用 gameplay 能力」这一构念（construct）。21 款游戏、3A 制作规格的覆盖，正是针对这一点的规模化回应。

3) 第二个缺陷的方法学后果：没有语言指令，就无法考察「指令分解（instruction decomposition）」与「目标规划」这两个被作者列为 gameplay 核心能力之一的维度，评测会退化为纯视觉/纯控制问题。因此多时间跨度指令是让数据从「录像集」变成「任务集」的关键。

4) 第三个缺陷的方法学后果：在线 rollout 的方差来自环境随机性、模型采样随机性与长视界误差累积。高方差意味着同一模型重复评测得到不同分数，跨模型比较的信噪比低，且失败后难以确定是哪一步开始崩。论文用「离线可复现 + 在线逐步」双轨来同时保留可复现性与真实行为验证。

5) 更深层的评价设计张力（论文自我设定的验证目标）：离线分数不等于真实玩得好。这是 score-behavior gap，即静态问答的正确率与真实交互中的可执行能力之间存在鸿沟。论文把「离线分数是否反映真实 gameplay 能力」直接设为在线轨道要回答的问题，这说明它把基准效度（validity）本身当作研究对象，而不只是刷一个榜单。这一点比单纯的「更大的数据集」更有价值。

6) 隐含假设与可质疑处（部分合理推断，需回原文核对）：
 - 假设自动标注流水线（Annotator）产出的多时间跨度指令质量足够高，且其噪声不会系统性改变模型排名。若自动标注继承了底层 VLM 的偏置，可能出现「用模型给模型出题」的循环偏置。人工校验比例、标注一致性指标是关键证据，摘要未提供。
 - 假设 100 名「专家玩家」的采集分布能代表 gameplay 能力。专家行为分布偏高效、偏进阶策略，可能低估新手路径上的困难（推测）。
 - 假设三个主任务加诊断变体足以覆盖多时间跨度能力空间；「任务→能力」的映射是否唯一、诊断变体是否真的具备因果可分性，需要相关性/消融分析支持。

7) 为什么这是一个系统级问题：数据规模与时间对齐、可扩展标注、评测协议设计、大规模模型普查四者互相制约——数据没有时间对齐就做不了多视界指令，标注不可扩展就做不到 5,000 小时，评测没有在线轨道就无法验证离线结论，没有大规模普查就无法确认难度层级是否稳定。缺任何一环，结论都不可信。

Q2: 有哪些相关研究？

说明：本次生成未获得 PDF 正文与检索证据（retrieved_evidence、field_evidence_map 均为空，sections 为空），以下相关研究梳理基于摘要中明确提到的三类缺口做领域级推演，属于「合理推断」，具体引用的论文名与对比结论必须回原文 Related Work 与对比表格核对；此处不编造具体系统名称与数值。

1) 游戏作为 AI 测试床的谱系。
- 传统路线：以单一游戏或单一类型为主的强化学习环境（如经典 Atari 风格像素级基准、实时策略类环境、以及 Minecraft 系开放世界 agent 环境）。这类工作的特点是有明确奖励信号、可无限 rollout，但通常不包含自然语言指令，且游戏数量有限。
- Agent 路线：以 LLM/VLM 作为决策核心、用自然语言描述目标、输出高层动作或代码/API 调用。与 GameHorizon 最接近，但多数工作聚焦单一游戏（如 Minecraft）或特定任务族，缺少跨 21 款异构 AAA 游戏的横向可比性。

2) 视频理解与视频问答基准。
- 关注短视频片段中的识别、动作分类、时序定位、描述与问答。它们的短板是「无动作性」：答对问题不需要在交互环境中成功执行；因此无法衡量「精确动作控制」这一 gameplay 核心维度。GameHorizon 把时间对齐的动作数据与指令绑定，正是为了补上这一维度。

3) GUI / computer-use agent 基准。
- 结构上与 gameplay 高度相似：输入截图、输出离散或坐标级动作、任务有长视界依赖。差异在时间跨度分布与动作语义：GUI 任务的动作空间更规则、视觉变化更局部；游戏的动作更连续、反馈更密集、随机性更强。二者在方法上可互相借鉴（尤其是失败定位与逐步评测协议）。

4) 大规模自动标注与数据构建。
- 近年主流做法是用强模型做伪标注（caption、问答生成、轨迹切分），再用少量人工校验。论文的 Annotator 属于此类，其创新点被定位为「多时间跨度」（把标注切分到不同时间尺度），而非单纯提高吞吐。风险与前文所述一致：继承上游模型偏置、长尾场景标注质量下降。

5) 评测方法学：离线静态 vs 在线 rollout。
- 离线评测：可复现、成本可控、可覆盖数千题，但有效性问题（是否真的代表能力）。
- 在线 rollout：真实、可暴露误差累积与长视界退化，但方差大、成本高、难以复现。
- 更早的工作通常只选一边；GameHorizon 同时提供两条轨道并用在线轨道去校验离线分数，这在方法学上更接近「基准效度研究」而非「新任务提出」。

6) 与生成模型/世界模型的交叉（与用户关注方向相关）。
- Gameplay 视频 + 动作标注天然是世界模型与可控视频生成的训练素材；反过来，生成模型可用于构造反事实 rollout 或做数据增广。论文是否讨论了这一交叉，摘要未体现，需回原文结论/讨论章节确认。

Q3: 论文如何解决这个问题？

GameHorizon Suite 由三个互补组件构成，形成「标注能力 → 数据资产 → 评测协议」的流水线闭环。

1) GameHorizon-Annotator：面向多时间跨度指令的可扩展自动化标注流水线。
- 目标：把原始 gameplay 录像自动切分并转化为不同时间尺度上的语言指令。
- 定位：这是整个套件的「产能引擎」——没有它就无法从 5,000 小时录像中规模化产出标注。
- 从摘要可确认的属性：可扩展（scalable）、自动化（automated）、多时间跨度（multi-horizon）。
- 未从摘要确认的关键细节（需回原文）：时间跨度的具体分层定义（例如帧级/秒级/分钟级/关卡级）、指令生成使用的模型、是否有人工校验环节与一致性统计、如何保证指令与动作在时间上精确对齐。

2) GameHorizon-Data：首个大规模 AAA gameplay 数据集。
- 规模：5,000 小时录像，21 款游戏，100 名人类专家玩家采集。
- 三类时间对齐模态：视频、玩家动作（player actions）、多时间跨度指令。三者在时间轴上对齐，是支撑「精确动作控制」评测与「长视界规划」分解的结构基础。
- 采集主体是专家玩家而非随机玩家，意味着数据分布偏向高水平的操作与策略（推测其对模型学习的价值更高，但代表性需要检验）。
- 未确认细节：动作空间的记录粒度（键盘/鼠标原始事件还是高层动作原语）、是否包含游戏状态/元数据、版权与授权方式、训练/测试划分方案。

3) GameHorizon-Bench：可复现离线 + 逐步在线双轨评测。
- 离线轨道：数千道标准化问题，组织为三个主要任务（primary tasks）加一系列诊断变体（diagnostic variants）。诊断变体的作用是做能力归因——把「总分」拆解到具体能力维度（例如是否只看视觉、是否只依赖时序线索、是否需要跨视界推理）。
- 在线轨道：逐步（stepwise）交互评测，两个目标明确写在摘要中——(a) 检验离线分数是否反映真实 gameplay 能力；(b) 把失败定位到长视界 gameplay 中的具体步骤。这使在线轨道同时承担效度验证与误差诊断两个功能。

4) 规模化验证协议：47 个模型 × 超过 100 万次模型调用，覆盖多种模型族（摘要表述为 diverse model families，暗示包含 VLM/LLM 及可能的 agent 或视频模型，具体构成需回原文）。这种「普查式」评测在数据类工作中相对少见，其目的是让难度层级与能力差异的结论具有跨模型的可重复性，而不是只报几个 baseline。

Q4: 论文做了哪些实验？

基于摘要可确认的实验设计如下（具体数值与表格结构未在摘要中出现，需回原文核对）。

1) 规模：评测 47 个模型，累计执行超过 1,000,000 次模型调用。这个量级意味着评测本身是一项基础设施级投入，也意味着作者可以给出较稳健的模型间相对排序（合理推断）。

2) 双轨评测设计：
- 离线轨道：使用数千道标准化问题，划分为三个主要任务；每个主要任务下配有诊断变体，用于细粒度能力归因。离线轨道强调可复现性（reproducible）。
- 在线轨道：逐步交互测试，检验离线分数与真实 gameplay 能力之间的一致性，并对失败进行步骤级定位。

3) 模型族覆盖：摘要称覆盖 diverse model families。从任务性质（视频 + 指令 + 动作）合理推断至少包含视觉语言模型与通用 LLM；是否包含视频生成/世界模型、专用 agent 框架、闭源商用 API 与开源权重的对比矩阵，摘要未说明。

4) 未在摘要中提供的实验要素（信息缺口，必须回原文）：
- 三个主任务的具体名称、输入输出形式与评价指标；
- 诊断变体的构造方式（扰动类型：时间打乱、遮挡、指令改写、动作掩码等）与数量；
- 在线评测的环境实现方式（是否在真实游戏引擎中运行、是否有 API 接口、动作执行频率）；
- 人类专家玩家的对照基线（人类在同样任务上的表现）；
- 标注流水线的质量验证实验（人工抽检比例、指令-动作对齐误差）；
- 计算成本、推理预算、是否允许思维链/工具调用等推理配置的统一化处理。

5) 评价设计的特点：把「离线分数 vs 在线表现」的一致性作为实验对象，本质上是一次基准效度（validity）检验实验，而非单纯的排行榜发布。

Q5: 发现了什么实验现象？

摘要明确给出的实验现象只有两条，其余为结构推断，所有具体数值均缺失。

1) 存在一个有意义的任务难度层级（meaningful hierarchy of task difficulty）。
- 含义：不同任务对模型的难度有可分离的高低排序，而不是所有任务都难或都易。
- 解读（合理推断）：难度层级很可能与时间跨度正相关——时间跨度越长、越依赖跨视界记忆与规划的任务越难；而短视界的视觉识别类任务相对容易。但这只是与论文标题「Multi-Horizon」一致的合理推断，摘要未直接给出难度与时间跨度的对应关系，需回原文的按视界难度曲线核对。
- 反例可能：如果难度层级主要由视觉复杂度而非时间跨度决定，则论文的核心叙事需要修正；这一点必须看诊断变体结果。

2) 不同模型能力之间存在显著差异（pronounced differences in model capabilities）。
- 含义：47 个模型的分数分布分散，说明基准具备区分度，没有出现天花板或地板效应（合理推断）。
- 更有信息量但摘要未给的问题是：差异是按模型规模单调缩放，还是按模型族分化（例如某些模型擅长视觉接地、另一些擅长长视界规划）。这是「跨模型族标尺」这一主张能否成立的关键证据。

3) 离线—在线一致性：论文把「离线分数是否反映真实 gameplay 能力」设为在线轨道要回答的问题，说明作者预期这里存在张力。
- 可预期的消融趋势（推测，需原文验证）：离线分数高的模型在线表现未必同比高，因为长视界误差累积会放大微小错误；短视界任务上离线与在线可能一致，长视界任务上可能出现明显背离。
- 步骤级失败定位：在线轨道能把失败归因到具体步骤，这通常会产生一个可观察的现象——多数失败并非模型完全不会，而是在某个特定步骤之后开始持续退化（合理推断）。

4) 尚未揭示的信息（重要缺口）：所有准确率、排名、离线—在线相关系数、各诊断变体的性能落差、负结果与失败案例的定性描述，摘要均未提供。任何关于「哪个模型最好」「哪个任务最难」的具体判断都不能从本材料得出。

Q6: 有什么可以进一步探索的点？

以下方向分为三类：论文自身承诺或暗示的、由方法学缺口衍生的、以及跨领域扩展。

1) 资源发布与可复现性延伸（论文明确承诺）：数据集、Annotator 与基准将开源。值得追踪的是：(a) 21 款 AAA 游戏的版权与数据分发方式如何解决（这是此类工作最大的落地约束）；(b) Annotator 是否可用于新游戏/新游戏的冷启动标注；(c) 评分服务器是否提供公共榜单以防止测试集过拟合。

2) 从评测到训练：论文定位为「标尺」，但 5,000 小时时间对齐的动作 + 指令数据天然可用于训练 gameplay agent（模仿学习、离线 RL、指令条件策略学习）。后续可探索在 GameHorizon-Data 上微调是否能提升在线轨道的真实表现，这将直接检验数据价值而不只是评测价值。

3) 多时间跨度的方法学深化：(a) 给出跨视界的难度—性能曲线，验证「时间跨度是难度主因」这一假设；(b) 研究长视界误差累积的机制，是否可以通过再规划（re-planning）、记忆压缩或层级策略缓解；(c) 诊断变体是否可以做成可控扰动的因果实验栈（时间打乱、状态遮挡、指令消歧），从而把「能力」拆成可操作的变量。

4) 离线—在线效度的量化建模：把离线分数到在线成功率之间的映射当作可学习对象，研究在什么任务类型/什么时间跨度上离线预测在线是可靠的，进而给出「何时可以只用离线评测」的可操作准则。这会显著降低大规模 agent 评测的成本。

5) 人类—模型对照与专家偏置：引入人类玩家在相同任务上的表现作为锚点，并分析 100 名专家玩家的行为分布是否过窄（是否要补充新手/中级玩家数据以覆盖不同策略空间）。

6) 跨领域迁移：gameplay 中形成的多视界指令分解、逐步失败定位协议，可迁移到 GUI 操作、机器人操作、科学实验自动化（与用户关注方向和 ai-for-science 相关）等同样具有长视界与精确动作要求的场景。

7) 与生成模型/世界模型的交叉（与 generation 方向相关）：用生成模型合成反事实 rollout 来扩充评测状态空间；或反过来用 GameHorizon 的动作条件数据训练可控世界模型，评估其作为「可交互评测环境」的保真度。这是一个双向价值通道。

8) 评测经济性：100 万+ 次调用说明了大规模模型普查的成本量级。可探索自适应评测（只对不确定样本追加调用）、分层抽样以在保持排名稳定性的同时降低调用量。

9) 偏置与安全：自动标注流水线可能继承上游模型对视觉风格、文化语境、语言表达的偏置；值得做系统性审计并发布带偏置标签的子集。

Q7: 总结一下论文的主要内容

论文要解决的问题：视频游戏是检验 AI 综合能力的理想测试床，因为它同时要求视觉理解、指令分解、目标规划与精确动作控制，而这些能力分布在不同时间跨度上。作者指出既有数据集与基准存在三个缺口：覆盖游戏范围窄、缺少语言指令、依赖高方差的在线 rollout。前两点导致评测维度不完整且难以泛化，第三点导致评测信度低、结果不可复现、失败难归因。

提出的方案：GameHorizon，一个统一的数据与评测套件，包含三个组件。

组件一，GameHorizon-Annotator：一条可扩展、自动化的多时间跨度指令标注流水线。它是整套数据的产能基础，使得从数千小时原始 gameplay 录像中批量生成跨时间尺度的语言指令成为可能。摘要未披露时间跨度的具体分层定义、所用模型与人工校验比例。

组件二，GameHorizon-Data：作者称其为首个大规模 AAA gameplay 数据集，规模为 5,000 小时录像、21 款游戏，由 100 名人类专家玩家采集。数据包含三类时间对齐的模态：视频、玩家动作、多时间跨度指令。三者的时间对齐是核心设计，它让「动作级精确控制」与「长视界任务规划」可以被放在同一个数据表示下评测。

组件三，GameHorizon-Bench：包含两条评测轨道。
- 离线轨道：使用数千道标准化问题，组织为三个主要任务，并配备一系列诊断变体。其价值主张是可复现性——相同输入产生相同分数，便于横向比较与能力归因。
- 在线轨道：逐步（stepwise）交互测试。它承担两个明确功能：一是检验离线分数是否真的反映真实 gameplay 能力，二是把失败定位到长视界 gameplay 中的具体步骤。这实际上把「基准效度」本身变成了被研究对象，而不只是发布一个新的排行榜。

验证与主要发现：作者基于整套套件评测了 47 个模型，执行超过一百万次模型调用，覆盖多种模型族。结果显示存在一个有意义的任务难度层级，以及不同模型能力之间的显著差异。这两条结论表明基准具有区分度，且任务难度并非同质。摘要未给出任何具体数值、任务定义细节、模型排名或离线—在线一致性的量化结果。

整体定位与贡献：GameHorizon 是一份「基础设施型」工作，其贡献不在于提出新的模型结构，而在于同时提供标注能力、数据资产与评测协议，并把离线可复现性与在线真实性放进同一框架内互相校验。作者承诺开源数据集、标注器与基准。

阅读时需要重点核对的点：(1) 三个主任务与诊断变体的确切定义；(2) 多时间跨度的分层方式与难度层级之间的对应关系；(3) 离线分数与在线表现的相关系数及背离情况；(4) 自动标注的质量控制证据；(5) 21 款 AAA 游戏的数据许可与分发方式。此外，本文元数据中的 arXiv 编号（2609.25001）与发布日期（2026-09-21）在时间上异常，建议在引用前先核实论文真实性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：gameplay 是长视界规划、指令分解与精确动作控制的综合压力测试场，本文的逐步失败定位协议与离线—在线双轨效度检验可迁移到 GUI/机器人 agent 评测。

## 基本信息

- 作者：Yiran Wang, Xingyilang Yin, Junfu Pu, Guangzhi Wang, Kaifeng Li, Mingyu Ouyang, Huiqiang Sun, Lingen Li, Cheng Cheng, Wangbo Yu, Honghao Chen, Xiaodong Cun, Chi-Man Pun, Zhiguo Cao, Ying Shan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-09-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.25001v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未获得 PDF 检索证据（retrieved_evidence 与 field_evidence_map 均为空，sections 全空），所有内容仅基于标题、作者列表与摘要做结构化推演，凡属推断处均已逐条标注「合理推断」「推测」或「需回原文核对」；institution 因元数据未提供且无正文可依据而留空；另需注意元数据中的 arXiv 编号 2609.25001 与发布日期 2026-09-21 在时间上异常，引用前建议先核实论文真实性。
