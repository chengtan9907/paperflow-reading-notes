---
user_id: "cheng tan"
paper_id: 12280
arxiv_id: "2609.22069"
title: "OmniVBench: A Benchmark and Large-Scale Dataset for Omni Reference-to-Video Generation"
publish_date: "2026-09-21"
pdf_url: "https://arxiv.org/pdf/2609.22069"
abs_url: "https://arxiv.org/abs/2609.22069"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-22T00:08:45"
---
# OmniVBench: A Benchmark and Large-Scale Dataset for Omni Reference-to-Video Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reference-to-video · video generation · benchmark · dataset

## 一句话总结

论文提出 OmniVBench 评测基准与 Omni-R2V Dataset 训练数据集，用因子化 checklist 评测协议覆盖 7 个任务族 / 18 个细粒度任务，并为 omni reference-to-video 生成提供约 340K 规模的工业级训练样本与可复用的数据构建流程。

## 摘要

> Reference-to-video (R2V) generation is evolving toward increasingly general and versatile reference control, giving rise to the emerging paradigm of omni R2V generation. However, existing benchmarks fall short of these emerging capabilities: their test cases cover limited reference types and compositions, and their evaluation protocols largely assess holistic reference consistency, overlooking whether reference factors are properly preserved, disentangled, and routed. Meanwhile, the high cost of constructing omni R2V training data makes suitable training resources scarce. To address these gaps, we introduce OmniVBench and the Omni-R2V Dataset for evaluating and training omni R2V models. OmniVBench expands R2V evaluation across broader reference types, fine-grained control tasks, and richer reference compositions, covering 7 task families and 18 fine-grained tasks spanning content, motion, style, structure, narrative, and multi-reference settings. We introduce factor-grounded evaluation with 12,172 case-specific checklist items, assessing whether intended reference factors are faithfully preserved, correctly disentangled and bound to their targets, and properly realized according to the instruction. We further introduce the Omni-R2V Dataset, bringing industrial-grade training resources for diverse R2V tasks to the broader research community. Drawing primarily on a large-scale corpus of professional video footage, it comprises 340K processed training samples spanning diverse reference types and multi-reference compositions. We develop task-specific pipelines for reference-target pair construction, offering a practical and scalable recipe for omni R2V data construction. Extensive evaluation of advanced open- and closed-source R2V models reveals clear performance gaps across task families and evaluation dimensions on OmniVBench, highlighting remaining limitations of current R2V models.

Q1: 这篇论文试图解决什么问题？

论文试图解决的是一个“评测缺位 + 数据缺位”的双重问题，其立论前提是：R2V 生成正在从单一、窄口径的参考控制（例如只用一张参考图控制主体身份）走向 omni R2V——即同时接纳图像、视频片段、风格素材、结构信号、叙事文本等多种参考类型，并允许它们在同一段生成视频中以多种组合方式共同生效。作者认为，这一范式的迁移已经跑在了评测与训练资源前面。

第一层缺口是任务覆盖缺口。摘要明确说现有基准的 test cases “cover limited reference types and compositions”，也就是说，评测用例在参考类型的多样性（内容/运动/风格/结构/叙事等）和参考组合的复杂度（单参考 vs 多参考共存、多参考互相约束）上都偏窄。对 omni R2V 而言，真正困难的场景恰恰出现在参考类型变多、参考之间发生交互之后，因此窄口径基准会系统性高估模型能力。

第二层缺口是评测粒度缺口，也是论文最核心的批评点。现有协议“largely assess holistic reference consistency”，即把参考一致性当成一个整体标量来打分，只问“生成结果看起来像不像参考”。这种做法隐含两个未被检验的假设：一是参考信息被当作不可分的整体，二是“像”就等于“用对了”。论文指出它忽略了三件事：参考因子是否被正确保留（preserved）、是否被正确解耦（disentangled，即不同参考携带的属性没有被混成一团、没有互相污染）、是否被正确路由/绑定（routed，即该属性被施加到了正确的对象、区域或时间段上）。这三者是可以独立失败的：一个模型可能整体相似度很高，却把 A 人物的服装穿到了 B 人物身上，或把参考视频的运动模式错误地迁移到背景而非主体。整体一致性指标对这类错误不敏感，因此无法诊断失败原因，也无法指导模型改进。

第三层缺口是训练资源缺口。摘要直言 omni R2V 训练数据“high cost of constructing”，导致合适训练资源稀缺。这既是一个工程经济问题（多参考类型意味着多套标注/配对流程），也是一个可复现性问题（缺乏公开的大规模资源，学术界难以在同一起跑线上比较方法）。论文因此把 340K 规模的 Omni-R2V Dataset 与“task-specific pipelines for reference-target pair construction”一起作为交付物，定位是“把工业级训练资源带给更广泛的研究社区”。

从方法论层面看，论文的隐含假设值得在精读时重点核查：factor-grounded checklist 评测假定参考因子是可显式枚举的、且可以被判定为“绑定到正确目标”；但在多参考设定下，因子之间可能存在合法耦合（例如同一素材同时提供身份与服装），checklist 如何定义“正确解耦”的边界并不清楚（合理推断，需回原文核对任务定义与标注规范）。另一个隐含假设是 checklist 的生成与判定足够可靠——12,172 条 case-specific 条目若由模型自动生成，评测结果的可复现性与判定偏差需要人工一致性验证支撑。此外，基准与训练数据集出自同一团队、可能共享素材来源，存在训练—评测同源的风险，这一点摘要未涉及。

Q2: 有哪些相关研究？

本节只能基于摘要与题目推断论文所处的文献脉络，具体引用清单必须回原文 Related Work 与参考文献核对；以下条目属于“合理推断”，不构成对论文实际引用的断言。

1）视频生成主干与条件控制。R2V 建立在 text-to-video 与 image-to-video 生成的基础之上（例如扩散/流匹配类视频生成模型及其可控变体），其前提是模型具备接受额外条件输入（参考图像、参考视频、结构信号如深度/姿态/边缘）的能力。omni R2V 可以理解为把多种条件通道统一到同一个生成接口下，因此与可控视频生成、多条件组合控制（multi-condition composition）这一支研究直接相邻。

2）主体/身份驱动的视频定制（subject-driven / identity-preserving video customization）。这是 R2V 最成熟的子方向：用一张或几张参考图，把特定人物、物体或宠物的身份保持到生成视频中。该方向的评测通常围绕身份相似度、时序稳定性、与文本指令的兼容性展开，也正是论文批评的“整体参考一致性”评测传统的来源。

3）参考迁移类任务：运动迁移（motion transfer）、风格迁移（style transfer）、结构/布局控制（structure control）。这些任务把“参考”从身份扩展到运动轨迹、视觉风格、空间结构等抽象属性，是 omni R2V 中“参考类型扩展”的直接来源，也是因子化解耦评测最难的一类——因为风格与内容、运动与主体的分离本身就有争议。

4）多参考/多主体组合生成。当同时给出多个参考（多个人物、人物 + 场景、主体 + 风格）时，会出现属性混淆、参考泄漏、参考被忽略、参考间冲突等失败模式。论文的 “multi-reference settings” 任务族显然针对这一支文献，而 “disentangled and bound to their targets” 的表述说明作者关心的是参考到目标的分配问题，这一提法在多主体定制生成（multi-subject personalization）文献中已有先例。

5）视频生成评测基准。T2V/I2V 领域已有大量基准，通常按维度（视觉质量、时序一致性、运动幅度、文本对齐、物理合理性）组织自动指标与人工评测。论文的方法论创新在于把评测单位从“整体视频级分数”下移到“case-specific checklist item”，这是一种更接近细粒度诊断而非单一排名的基准设计思路，与近年来“诊断型基准/细粒度能力分解评测”的趋势一致。

6）数据构建配方（data recipe）。340K 规模、基于专业视频素材、按任务定制参考—目标配对流程，这一路线与工业界构建定制化视频数据的方式接近；论文把 pipeline 描述为“practical and scalable recipe”，说明其贡献既包括数据集本体，也包括可被复用的构造方法论。

Q3: 论文如何解决这个问题？

论文的解决方案由两个互补组件构成，一个面向评测，一个面向训练，二者共享同一套“参考类型 × 任务”的能力分解视角。

一、OmniVBench：任务族与细粒度任务的扩展。
基准在三个方向上做扩展：更广的参考类型、更细粒度的控制任务、更丰富的参考组合。整体组织为 7 个任务族、18 个细粒度任务，覆盖 six 大类场景：content（内容类参考，如主体/物体外观）、motion（运动类参考）、style（风格类参考）、structure（结构/布局类参考）、narrative（叙事类参考，可能涉及多镜头或多事件序列）、以及 multi-reference settings（多参考共存与组合）。任务族与细粒度任务是两级结构，粗粒度族用于给出可比较的宏观画像，细粒度任务用于定位具体能力短板。

二、factor-grounded evaluation（因子落地式评测）。
这是论文评测协议的核心机制。它不产生单一的“整体一致性”分数，而是为每个 case 配备 case-specific checklist 条目，总计 12,172 条。每条 checklist 项对应一个可判定的核查点，评测时分别考察三个层次：
（1）是否被忠实保留（preserved）——参考因子的内容是否真的出现在生成视频中，而不是被忽略或被模型先验覆盖；
（2）是否被正确解耦并绑定到其目标（disentangled & bound to targets）——不同参考携带的属性不能互相污染，且必须落在正确的对象、区域或时间位置上；
（3）是否按指令正确实现（realized according to the instruction）——因子不仅出现，还要在指令规定的语义关系/动作/场景中生效。
这一设计把“像不像”拆成“有没有、有没有用错地方、有没有按指令用”，理论上可以区分“参考被忽略”“参考被误绑定”“指令未遵循”等不同失败模式，也让不同任务族之间的错误类型可比较。

三、Omni-R2V Dataset：训练资源与构建配方。
数据集主要基于大规模专业视频素材语料（professional video footage）构建，包含 340K 处理后训练样本，覆盖多种参考类型与多参考组合。实现路径是“task-specific pipelines for reference-target pair construction”：针对每个任务定义参考与目标之间的配对关系，并据此从源语料中切分/配对/处理出训练样本。作者强调这套流程既是工业级资源，又是一份可扩展、可复用的 recipe，即希望社区能按同样方法继续扩建，而不是只消费一个静态数据集。

四、两者的关系。
从摘要叙述顺序看，OmniVBench 定义“能力空间”，Omni-R2V Dataset 在同一能力空间上提供训练素材，形成“评测—训练”闭环。这种设计使得基准上的失败模式可以直接对应到数据配方中某一任务类型的缺失，是有别于纯评测基准论文的一个结构性特点（此结构性评价为合理推断，论文是否明确讨论该闭环需回原文确认）。

Q4: 论文做了哪些实验？

摘要层面可确认的实验安排只有一条：对“advanced open- and closed-source R2V models”（先进的开源与闭源 R2V 模型）进行 extensive evaluation，并在 OmniVBench 上按任务族与评测维度报告结果。

以下属于需要回原文核对的缺口，本报告不做臆测性填充：
1）具体参与评测的模型清单、版本号、推理配置（分辨率、时长、采样步数、是否使用参考图像数量上限）未知；摘要未给出任何模型名称。
2）评测的执行方式未知：12,172 条 checklist 项是由人工标注判定、由 VLM/LLM judge 自动判定，还是二者混合；若为自动判定，是否报告了与人类判断的一致性。
3）是否包含人类主观评测（MOS/偏好对比）以及人工评测与 checklist 评分的相关性分析，未知。
4）是否包含消融实验（例如去掉某一评测层次、改用整体一致性评分做对比、checklist 粒度变化的影响），未知。
5）是否包含在 Omni-R2V Dataset 上训练/微调模型的实验（数据配方有效性验证），摘要未提及；如果缺少这一环，数据集的“可训练价值”就只能靠规模与多样性论证，这是一个关键证据缺口。
6）是否报告按任务难度分层、按参考数量分层的细粒度数字，未知。

因此对“论文做了哪些实验”可确定的结论是：这是一篇以评测为主、附带数据资源的基准型工作，实验部分的体量集中在大规模模型横向对比上；是否存在训练侧验证，需要读 Experiments 章节确认（合理推断：由于数据集被作为主要交付物之一，作者大概率会给出某种形式的可用性验证，但这只是推测）。

Q5: 发现了什么实验现象？

摘要中唯一明确的实验现象是：在 OmniVBench 上，先进的开源与闭源 R2V 模型在不同任务族与不同评测维度上表现出“clear performance gaps”，即能力分布不均衡，且整体上仍存在明显局限。这一观察的解读价值在于它的比较结构——不是“某模型全面更好”，而是提示存在任务族 × 评测维度的能力矩阵，不同模型在不同象限上有不同短板。

在此基础上，可以列出原文应当回答、但摘要未给出的现象层问题，供精读时逐条核对：
（1）层次间张力：模型是否出现“保留率高但解耦差”的模式，即参考内容确实出现了，却被绑定到了错误的目标（典型如主体交换、属性跨对象泄漏）？反之是否出现“解耦尚可但保留率低”，即模型干脆忽略参考、退回文本先验？这两类失败在整体一致性指标下会得到相似分数，正是论文主张 factor-grounded 评测的理由。若原文能给出层次间的相关性矩阵或退化曲线，将是对其评测设计最强的支撑（此为合理推断的期待，不代表论文实际包含）。
（2）参考类型难度梯度：content 类参考通常最容易，motion 与 style 的“因子边界”最模糊，structure 类依赖空间对齐能力，narrative 类涉及长程语义一致性，multi-reference 类要处理参考间冲突。可预期整体难度呈递增顺序，但这一顺序是否在实验中成立需要核对；若某一任务族出现反直觉结果（例如 style 反而高于 content），很可能是 checklist 定义或判定方式带来的伪影（推测）。
（3）开闭源差距是否在特定维度上反转：闭源模型常在视觉质量与指令遵循上占优，但在严格的身份保留/多参考绑定上未必领先；是否出现“闭源强于整体一致性、开源强于解耦”这类交叉，是值得在原文表格里专门寻找的模式（推测）。
（4）负结果与失败模式：是否报告了参考被完全忽略、参考数量增加导致性能单调下降、指令与参考冲突时模型偏向哪一方等具体案例。多参考冲突场景（两个参考互相矛盾）往往是最能暴露模型机制的实验，摘要未涉及。
（5）时间维度现象：参考因子在视频时间轴上是否稳定（中途漂移、后半段丢失），摘要未涉及，属于已知的 R2V 常见失败面。

需要强调：以上除第一条外均为待验证问题清单，不是本次可确认的实验发现；由于未能获取 PDF 正文，本报告不对任何具体数值、排名或趋势作断言。

Q6: 有什么可以进一步探索的点？

1）数据—评测闭环的实证化。当前论文交付了基准与数据集两个组件，但摘要未显示二者被显式打通。后续可做的最直接工作是：在 Omni-R2V Dataset 上按任务族分层训练/微调模型，验证“补齐某一任务族数据是否能提升对应基准维度”，从而把数据配方从“规模论证”升级为“因果证据”。

2）factor-grounded 评测的可信度工程。12,172 条 case-specific checklist 的生成来源、判定主体与判定一致性是这套协议能否被社区采用的关键。可探索方向包括：checklist 自动生成的质量控制、checklist 判定与人类细粒度标注的一致性上限、checklist 数量与评测稳定性的缩放关系、以及是否存在模型针对 checklist 的“应试”风险（例如生成“看起来包含所有因子但不合理组合”的内容）。

3）参考因子的形式化与边界定义。“正确解耦”在多参考场景中并非总是良定义：同一素材天然同时携带身份与服装、主体与运动往往不可分。后续工作可以把因子定义从硬性枚举推进到带耦合先验的结构化标注（如图结构的因子—目标绑定），并研究“部分耦合是否可接受”的判定标准。

4）任务与模态扩展。可预见的扩展方向包括：音频/语音参考（音色、节奏、环境声迁移）、多镜头与长视频叙事参考、3D/相机轨迹参考、物理属性参考（材质、流体行为）。这些扩展会显著改变“绑定到目标”的判定方式（从空间绑定扩展到时间与场景段落绑定）。

5）冲突、缺失与鲁棒性场景。真实创作中参考之间会互相矛盾、参考本身质量参差（模糊、遮挡、多主体）。系统性地构造冲突参考与低质参考子集，考察模型的偏向策略（偏向文本还是偏向参考、偏向哪一个参考）是很有诊断价值的方向。

6）评测与物理/时序一致性的交叉。R2V 的因子保真不能以牺牲物理合理性与时序稳定性为代价；把 factor-grounded 评分与运动合理性、物体恒常性、长程身份稳定性指标联合分析，可以发现“保真度提升被时序退化抵消”这类掩盖性权衡。

7）数据合规与去偏。基于专业视频素材构建的 340K 样本涉及版权、肖像、地域与人群分布偏差问题。后续可探索可商用来源的可追溯标注、参考类型的均衡采样、以及偏差对下游生成公平性的影响。

8）基准污染与训练同源检测。当训练数据集与评测基准出自同一素材池时，需要研究检测与缓解方案（例如素材级去重、时间切分隔离、近邻检索探测），否则基准排名的解释力会被削弱。这一方向论文摘要未提，属于基于其数据构建方式提出的外部关切。

9）与 agentic / 迭代式生成的结合。R2V 任务的 checklist 结构天然适合作为反馈信号，用于多轮自我修正式生成（先生成、再按 checklist 自检、再修订）。把基准从“单次打分”扩展为“可交互的诊断环境”是一个有潜力的范式演进方向。

Q7: 总结一下论文的主要内容

（说明：本总结基于论文摘要与元数据重构，未获取 PDF 正文；所有非摘要直接支持的内容均已标注推断等级。）

一、论证主线。
论文的推理链条可以概括为“范式迁移 → 评测失灵 → 数据稀缺 → 同步交付基准与数据”。起点是 R2V 生成的技术演进：从早期单一类型的参考控制（用参考图控制主体），走向 omni R2V——同时接纳内容、运动、风格、结构、叙事等多类参考，并允许它们以组合形式共同作用于同一段生成视频。作者随即指出，评测体系没有跟上这一迁移：现有基准的测试用例在参考类型与参考组合上都偏窄，评测协议则停留在整体参考一致性（holistic reference consistency）这一粗粒度指标上。

论文对整体一致性的批评是全文的关键论证。它认为这种评分方式默认了“参考信息是整体不可分的”，因此无法回答三个更本质的问题：参考因子是否被忠实保留（preserved）、是否被正确解耦并绑定到正确的目标（disentangled and bound）、是否按指令被正确实现（realized）。这三个问题对应三类不同的失败：参考被忽略、参考用错对象、指令未遵循。整体一致性指标会把它们压成同一个分数，从而既不能诊断失败，也不能指导改进。论证的第三步转向训练侧：omni R2V 训练数据构造成本极高，公开可用资源稀缺，学术界缺乏在同一资源条件下比较方法的条件。

二、技术主线。
论文给出两个交付物，共享同一套能力分解视角。

OmniVBench 的评测结构是“任务族 × 细粒度任务 × checklist 层次”。任务侧，基准覆盖 7 个任务族、18 个细粒度任务，横跨内容、运动、风格、结构、叙事与多参考六类场景，把“参考类型”和“参考组合复杂度”两个维度同时展开。评测侧，引入 factor-grounded evaluation，为每个 case 配备定制的 checklist 项，共 12,172 条；每条核查项分别落在保留、解耦/绑定、指令实现三个层次上（三者与 checklist 的对应关系是论文的核心机制，具体每个层次由多少条目构成、如何加权，摘要未给出，需回原文核对）。这种设计把评测单位从“整段视频一个分数”下移到“每个 case 一组可判定条目”，是一种诊断型而非排名型的基准设计。

Omni-R2V Dataset 的构建思路是“专业素材 + 任务定制配对流程”。数据集主要基于大规模专业视频素材语料，处理后得到 340K 训练样本，覆盖多种参考类型与多参考组合。关键的方法论贡献在于 task-specific pipelines for reference-target pair construction：为每个任务定义参考与目标之间的配对规则，从而从源语料中系统化地产出训练对，作者将其描述为实用且可扩展的 omni R2V 数据构建配方。这意味着数据集的定位不止是静态资源，还包括一套可被社区继续扩建的方法。

三、实验主线。
论文对先进的开源与闭源 R2V 模型在 OmniVBench 上做广泛评测，并报告在不同任务族与不同评测维度上存在明显性能差距。这是一个“能力矩阵不均衡”的结论，而非“某模型全面领先”。摘要未给出参与评测的模型列表、评测执行方式（人工 or 模型判定）、具体分数、消融实验，也未明确说明是否包含基于 Omni-R2V Dataset 的训练验证。因此关于“该数据配方是否真的能提升 omni R2V 能力”，本文在摘要层面没有提供证据，这是阅读原文时最需要优先补上的缺口。

四、信息缺口与阅读提示。
（1）12,172 条 checklist 的生成与判定流程、是否有人类一致性验证，是判断基准可信度的第一优先级问题。
（2）训练数据集与评测基准若共享素材来源，需要检查是否存在污染与同源风险，摘要未提及去重或隔离策略；这一关切为基于其构建方式的合理推断。
（3）元数据中 publish_date 为 2026-09-21、arXiv 编号 2609.22069，属于未来时间戳，可能是元数据占位或录入异常，阅读时建议核对官方 arXiv 页面与版本记录（本条为对元数据本身的观察，不涉及论文内容判断）。
（4）作者机构在元数据中为空，未能从正文推断，本报告不填写 institution 以避免编造。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 generation 方向直接重合（权重 0.10）：论文是视频生成可控性支线上的基准 + 数据资源，属于可直接复用的基础设施型工作。

## 基本信息

- 作者：Wenxue Li, Peiyan Guan, Haoyang Jiang, Junxian Cai, Hualuo Liu, Chunjie Zhang, Chong Guan, Kai Huang, Songlian Li, Taiyi Wu, Yongjian Yu, Xiaotong Zhao, Alan Zhao, Eric Liu, Xi Chen, Yu Liu, Lei Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.22069`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次未检索到任何 PDF 证据片段（retrieved_evidence 与 field_evidence_map 均为空、sections 全为空），报告完全基于论文摘要与元数据重构，未参考 PDF 检索证据；所有超出摘要的判断均已标注为合理推断或推测，具体数值、模型清单与评测执行细节需回原文核对。
