---
user_id: "cheng tan"
paper_id: 13077
arxiv_id: "2609.27656"
title: "InternW0: A Foundational Physical World Model for Efficient Real-World Interactions"
institution: "Shanghai AI Laboratory（上海人工智能实验室）——依据摘要中“the first instantiation of the InternW physical world model series from Shanghai AI Laboratory”直接推断；具体作者的分属单位（如是否含高校联合培养单位）因未获取 PDF 首页与作者脚注，无法确认。"
publish_date: "2026-09-24"
pdf_url: "https://arxiv.org/pdf/2609.27656"
abs_url: "https://arxiv.org/abs/2609.27656"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-24T12:08:59"
---
# InternW0: A Foundational Physical World Model for Efficient Real-World Interactions

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world model · video-action model · flow matching · asynchronous inference

## 一句话总结

InternW0 是上海人工智能实验室 InternW 物理世界模型系列的首个版本，用非对称视频-动作架构结合 flow matching 联合学习未来视觉动态与连续机器人控制，通过异步多频处理、层级 K/V 复用与观察条件化上下文路由在部分可观测与外部扰动下维持预测的可执行性，并在约 7,200 小时异构机器人与第一人称数据（含 275 小时真实实验室数据 EgoLab）上训练，在仿真基准及 15 阶段金属-有机框架（MOF）合成、5 阶段接触与力感知的通用定量移液等真实科学任务上验证。

## 摘要

> Physical intelligence requires more than predicting how the world may evolve: predictions must remain actionable as the world continues to change. We introduce InternW0, the first instantiation of the InternW physical world model series from Shanghai AI Laboratory, built around omnimodal interfaces, asynchronous multi-frequency processing, and local physical modeling under partial observations and external influences. InternW0 jointly learns future visual dynamics and continuous robot control through an asymmetric video--action architecture with flow matching. A high-capacity video expert provides longer-horizon predictive context, while a lightweight action expert operates at a faster timescale. Instead of regenerating the future for every action update, InternW0 reuses layerwise K/V and adapts it to newly observed states through observation-conditioned context routing. Domain-specific interfaces and soft prompts support heterogeneous embodiments, while contact-aware post-training incorporates force and tactile signals for contact-rich manipulation. We train InternW0 on approximately 7,200 hours of heterogeneous robot and egocentric data, including EgoLab, a 275-hour real-laboratory egocentric dataset. Evaluation spans simulation benchmarks and real-world scientific tasks, including a 15-stage metal--organic framework synthesis workflow and 5-stage contact- and force-aware dexterous manipulation for general-purpose quantitative pipetting. These results advance scalable, asynchronous, and science-native physical world models for universal and efficient real-world interactions.

Q1: 这篇论文试图解决什么问题？

【本字段基于摘要与题录信息重构，正文未获取，凡超出摘要的推断均已标注】

一、论文试图解决的问题内核
摘要开篇给出的是一个“问题陈述”而非单纯的任务陈述：physical intelligence requires more than predicting how the world may evolve: predictions must remain actionable as the world continues to change。这句话把问题从“世界模型能否预测未来”推进到“世界模型的预测能否在被部署、被持续打断、被外部干扰的环境中保持可用”。据此可以拆出至少五层子问题（前两层由摘要直接支持，后三层为合理推断）：

1. 预测与控制的耦合问题：未来视觉动态与连续机器人控制是否需要一体化学习？摘要明确说 InternW0 jointly learns future visual dynamics and continuous robot control，说明作者认为二者应联合建模，而不是“先训世界模型、再训策略”的两段式。

2. 时间尺度不匹配问题：长时程预测（视觉动态，需要大容量模型）与高频控制（动作，需要低延迟）在计算节奏上天然冲突。摘要用 asynchrony 与“高容量视频专家 + 轻量动作专家”的非对称结构回应了这一矛盾。

3. 计算复用的经济性问题：世界模型在每个控制步都重新生成未来在算力上不可承受。摘要给出的解法是不 regenerate，而是复用 layerwise K/V 并用 observation-conditioned context routing 把旧上下文对齐到新观测——这意味着真正的难点被转化为“如何让缓存的预测上下文在状态变化后仍然有效”，即缓存一致性/陈旧上下文纠偏问题。

4. 部分可观测与外部影响（合理推断，摘要明确点到 partial observations and external influences）：真实部署中本体无法观察全局状态，且存在未建模外力、接触、遮挡、人类操作者干扰等。若世界模型把观测当作全知状态，预测会在扰动下迅速漂移。摘要中的“local physical modeling”暗示作者选择局部建模而非全局一致建模，这是一个明确的建模取舍（用局部性换可更新性与鲁棒性），其代价（长期一致性、全局物理守恒）需要原文实验确认。

5. 异构本体与科学任务落地（合理推断）：domain-specific interfaces 与 soft prompts 针对 heterogeneous embodiments；接触感知后训练针对力/触觉；MOF 合成与定量移液意味着评测被推向“多阶段、可验证、对精度敏感”的真实科学流程，而不仅是拾取-放置类操作。科学任务在此不只是应用示例，而是对“预测是否 actionable”的高强度检验（阶段多、误差可累积、容错低）。

二、隐含假设与方法论立场（推断）
- 假设规模化的异构第一人称/机器人数据能带来跨本体迁移；7200 小时与 EgoLab 的存在支持这一取向。
- 假设“不重新生成未来”在精度上可接受，即缓存上下文足以支撑高频动作；这是效率主张的另一面，也是潜在风险点。
- 假设异步多频的频率比可以人工设定或稳定可控；摘要未说明调度策略，需原文核实。
- 假设触觉/力信号通过后训练注入即可，而不必从头联合预训练。

三、待核对的证据缺口
摘要未给出任何定量结果、基线名称、仿真基准名称、失败案例、消融设计或延迟指标，因此“问题是否被解决、在什么条件下被解决”在当前证据下无法判断，必须回到原文的 Experiments 与 Discussion 章节确认。

Q2: 有哪些相关研究？

【重要说明：本次未获取 PDF 正文，论文自身的 Related Work 章节内容不可得。以下不是对论文相关工作的复述，而是依据摘要关键词梳理的“可对照技术谱系 + 原文核对清单”，谱系归类为推测，具体引用与对比关系必须回原文核实。】

一、按摘要关键词可预期的谱系（推测）
1. 生成式视频世界模型：以视频预测作为世界模型主干的一类工作（交互式视频生成、条件视频预测、长时程 rollout）。InternW0 的“video expert 提供长时程预测上下文”与这一谱系直接相关。需要核对：它对比的 video world model 是哪些、评测是否使用同口径的长期一致性指标。
2. 潜在空间世界模型 / model-based RL：把观测压缩到隐状态并做 rollout 的经典路线。InternW0 复用 K/V 而非重算 rollout，与隐空间 rollout 的效率争论存在直接张力，值得核对是否被引用与对比。
3. 视觉-语言-动作（VLA）模型与机器人基础模型：把动作作为生成目标的一类策略模型。InternW0 的“action expert + flow matching + 连续控制”属于这一大类，但通过世界模型式视频预测分支与之区分。核对点：是否与开源 VLA 基线对比、是否共享动作空间与本体接口。
4. 扩散/flow matching 策略：连续动作建模的主流生成式范式。InternW0 明确使用 flow matching，需要核对它作用于动作、视频 latent 还是两者，以及是否与 diffusion policy 类方法做消融比较。
5. 快慢双系统与非对称架构：System 1/System 2、大模型规划 + 小模型执行的范式。InternW0 的“高容量视频专家 + 轻量动作专家 + 异步多频”可视为该思想在世界模型语境下的实现，核对点在于信息如何从慢分支流向快分支（是否只靠 K/V 与路由）。
6. 实时/异步推理与动作 chunk 化：为降低推理延迟而采用的异步执行、动作分块、缓存复用等工程路线（如 real-time chunking 类思路）。K/V 复用与上下文路由与这一谱系高度相关，需核对它是否处理了“陈旧缓存导致的动作不一致”问题。
7. 触觉与力感知操作：visuotactile 策略、力控装配、接触丰富操作。InternW0 的 contact-aware post-training 属于该方向，核对点：传感器类型、力/触觉数据规模、是否与纯视觉版本做受控消融。
8. 具身数据与第一人称数据：跨本体机器人数据集与大规模第一人称人类视频/操作数据。EgoLab（275 小时真实实验室第一人称数据）对应“科学场景第一人称数据”这一相对空白的细分，需核对其采集协议、标注粒度与与机器人动作的对齐方式。
9. AI for Science 与自动化实验室：自驱动实验室（self-driving lab）、自动化合成、自主实验流程。15 阶段 MOF 合成属于该谱系，核对点：这是端到端自主完成还是人机协作、是否有化学表征作为成功判据、失败率如何。

二、原文核对清单（阅读时优先定位）
- Related Work 是否明确指出“预测可执行性”与既有世界模型工作的差异；
- 是否与同期异步推理/缓存复用工作存在方法重合或优先权争议；
- 是否把“局部物理建模”与全局一致性建模（如守恒律、3D 一致表示）做过对比；
- 是否引用了科学实验自动化的既有工作，以及其任务难度定位。

Q3: 论文如何解决这个问题？

【本字段以摘要显式内容为主干，机制层面的解释为合理推断并已标注】

一、总体架构：非对称视频-动作双专家
摘要明确：InternW0 jointly learns future visual dynamics and continuous robot control through an asymmetric video–action architecture with flow matching。可拆为三件事：
1. 两个专家不等权：高容量视频专家承担“更长时程的预测上下文”；轻量动作专家在“更快的时间尺度”上运行。容量与频率同时被非对称化，这是该设计的核心取舍——把昂贵的预测计算摊到长周期上，把高频控制留给小模型。
2. 联合学习而非两阶段：视觉动态预测与动作生成共享训练过程（推断：可能共享表示或通过上下文路由耦合，具体耦合方式需原文确认）。
3. flow matching 作为生成式建模范式：用于生成连续动作（合理推断），也可能用于视频 latent 的生成（不确定）。

二、效率机制：不重生成，而是复用 K/V + 上下文路由
摘要明确给出两条关键设计：
- reuse layerwise K/V：动作更新时不再为每一步重新生成未来，而是复用视频专家逐层缓存的 K/V；
- observation-conditioned context routing：把复用的上下文以“新观测”为条件重新路由，从而适配到最新状态。
机制解读（推断）：这套设计等价于把“世界模型重算”替换为“世界模型缓存的检索与重对齐”，其隐含前提是——相邻控制步之间世界状态变化有限，缓存上下文仍携带有效信息，只需针对新观测做定向修正。这个前提在接触突变、外力冲击、遮挡解除等场景下是否成立，是该方法的关键风险点，需查看原文的失败案例分析。

三、异构本体适配：领域接口 + 软提示
摘要明确：Domain-specific interfaces and soft prompts support heterogeneous embodiments。推断其作用是把不同本体的动作空间/观测空间映射到统一骨干上，用软提示做轻量条件化，避免为每个本体复制整套参数。核对点：软提示是在输入层、层间还是每层插入；不同本体是否共享视频专家。

四、接触感知后训练：力与触觉
摘要明确：contact-aware post-training incorporates force and tactile signals for contact-rich manipulation。注意其措辞是 post-training（后训练）而非从头预训练，推断流程为：先在大规模异构数据上预训练世界模型与动作专家，再用带力/触觉的接触丰富数据做第二阶段对齐。核对点：触觉信号如何进入视频-动作架构（作为额外观测模态还是额外预测目标）、是否会造成视觉泛化退化。

五、数据与训练规模
约 7,200 小时异构机器人数据与 egocentric 数据；其中 EgoLab 为 275 小时真实实验室第一人称数据集。推断第一人称数据主要用于提供人类操作先验与科学场景分布覆盖，但摘要未说明其如何与机器人动作空间对齐（是否只用视频预测分支、是否有伪动作标注）。

六、必须回原文确认的空白（当前无法从摘要判断）
骨干网络与参数量、视频专家与动作专家的层间连接与信息流方向、K/V 复用的时间窗口与淘汰策略、路由的粒度（token/层/头）、异步频率比的具体数值、flow matching 的噪声调度与目标构造、动作 chunk 长度、训练目标中视频与动作损失的相对权重、以及推理时延与吞吐的实测数字。

Q4: 论文做了哪些实验？

【说明：摘要只给出评测的“范围与场景”，未给出任何基准名称、基线、指标数值或消融设计，因此本字段以“实验矩阵框架 + 原文提取清单”的方式组织，避免编造。】

一、摘要可确认的实验构成
1. 训练数据规模与构成：约 7,200 小时异构机器人数据 + 第一人称（egocentric）数据；其中 EgoLab 为 275 小时真实实验室第一人称数据集（该数据集本身可视为本工作的一项数据贡献，推断用于支撑科学场景评测）。
2. 仿真基准评测：摘要表述为 “Evaluation spans simulation benchmarks”，具体基准未披露。
3. 真实世界科学任务评测：
 - 任务 A：15 阶段金属-有机框架（MOF）合成工作流。这是一个多阶段、长时程、化学过程相关的真实实验流程，成功与否应由化学表征/产物质量判定（推断，需核实成功判据）。
 - 任务 B：5 阶段的、接触与力感知的灵巧操作，用于通用定量移液（general-purpose quantitative pipetting）。该任务对精度、液体体积控制与接触力控制同时提出要求（合理推断）。

二、阅读原文时应提取的实验矩阵（清单）
1) 仿真：使用了哪些 benchmark（操作类、长时程类、接触类）；任务数与 episode 数；成功率/完成率指标；是否包含长时程多阶段基准以匹配 MOF/移液的任务结构（推测可能包含 LIBERO/SimplerEnv/robomimic/真机套件一类，但绝不可断言）。
2) 基线对比：与 VLA 类策略、diffusion/flow policy、纯视频世界模型 + 独立策略、以及异步推理方案各自的对比；是否包含同规模自训练基线。
3) 效率实验：推理延迟、控制频率、每秒可支持的动作更新次数、K/V 复用带来的加速比；以及异步频率比（video expert 更新频率 : action expert 更新频率）的扫描。
4) 消融：K/V 复用与否、observation-conditioned context routing 与否、软提示/领域接口的贡献、接触感知后训练的贡献、数据规模（7,200 小时中第一人称数据占比）的 scaling 行为。
5) 真实科学任务：15 阶段 MOF 流程的阶段成功率与端到端成功率、人工干预次数、失败模式；移液的体积误差/精度指标、力控指标、以及跨容器/跨液体泛化。
6) 泛化与鲁棒性：跨本体迁移、未见物体/未见实验室布局、外力扰动与遮挡场景。

三、当前证据状态下无法回答的问题
没有任何定量结果可用，因此无法判断方法相对基线的优势幅度、异步设计带来的精度代价，以及真实科学任务的完成质量。所有数值结论必须依据原文。

Q5: 发现了什么实验现象？

【说明：本次未获取实验章节，摘要中不含数值型现象描述。以下把“摘要中可确认的现象性陈述”与“需要在原文中重点寻找的现象/张力点”分开列出，避免把预期当作发现。】

一、摘要层面可确认的现象性陈述（弱现象）
1. 长时程多阶段任务的可执行性：论文声称能够完成 15 阶段 MOF 合成工作流与 5 阶段移液操作，这至少说明其动作输出在长序列上未迅速崩溃（属于摘要级主张，缺少成功率与失败分布证据）。
2. 效率设计不致命：作者以“不重新生成未来、复用 K/V + 上下文路由”作为核心机制，说明在其设定下缓存上下文足以支撑高频动作——但摘要未披露由此带来的精度损失，这是精度与效率之间的关键张力。
3. 接触/力信息的可注入性：contact-aware post-training 的表述暗示力与触觉信号能够以某种方式接入视频-动作架构，但摘要未说明这对纯视觉能力是否有副作用。

二、原文中应重点寻找的反直觉结果、消融趋势与负结果（核对清单）
1. 速度-精度权衡曲线：K/V 复用时间窗口越长，动作精度如何退化；是否存在“复用窗口上限”这样的临界现象。
2. 异步频率比扫参：video expert 更新更慢时，长时程一致性是否反而更好（更慢但更稳 vs 更快但更抖的张力）。
3. 陈旧上下文失败案例：状态突变、遮挡解除、外力冲击后，路由机制是否出现明显滞后或错误动作，论文是否给出失败模式分类。
4. 触觉后训练的代价：引入力/触觉后是否在无触觉传感器的本体或视觉主导任务上出现性能下降（负迁移）。
5. 数据构成的边际收益：7,200 小时中第一人称数据（EgoLab 275 小时）的贡献是主要来自视觉预测分支还是动作分支；人类第一人称数据能否有效迁移到机器人动作。
6. 仿真与真实的排名一致性：仿真基准上的领先是否在真实科学任务上保持（若不一致，是典型的关键发现）。
7. 长时程误差累积：15 阶段流程中失败集中在哪一阶段，是否存在误差累积导致的非均匀阶段成功率。
8. 任务的性质差异：MOF 合成为离散阶段序列，移液为连续精度与力控，两类任务是否暴露不同的能力瓶颈。

Q6: 有什么可以进一步探索的点？

【说明：以下分为“由论文自身定位可推出的方向”（推断，标注）与“由当前方法设计暴露的开放问题”（推测）。原文的 Discussion/Future Work 未获取，可能有更具体的主张。】

一、系列化与规模化（推断，与 InternW 系列的表述直接相关）
1. InternW 系列后续版本的扩展方向：更广的模态（触觉、力、听觉、化学传感读数）、更多本体的统一、更长的预测时程、以及从“模型”到“系统”的闭环。
2. 数据规模化路径：7200 小时之后，第一人称人类数据与机器人数据的混合比例如何最优；EgoLab 这类科学场景第一人称数据是否应扩展到更多实验规程与更多实验室。

二、异步与缓存机制的理论化（推测，是方法的核心风险点）
3. 异步调度策略的学习化：当前频率比若为人工设定，能否学习“何时更新视频专家、更新到第几层、复用哪个时间窗口的 K/V”。
4. 缓存一致性与陈旧上下文的纠偏理论：在部分可观测与外部影响下，如何界定“缓存上下文仍然有效”的条件；能否给出误差界或一致性指标。
5. 自适应路由粒度：从层/token 级路由推进到以不确定性或接触事件为触发条件的稀疏重算。
6. flow matching 在异步多频设置下的一致性：跨时间尺度生成目标的噪声调度是否需重新设计。

三、物理与世界模型理论层面
7. 局部物理建模的代价边界：局部建模何时会破坏全局一致性（液体守恒、物体持久性、容器约束），是否需要引入轻量物理约束或守恒正则。
8. 部分可观测下的主动感知：让模型主动选择观测动作（移动视角、增加接触探测）以消解不确定性，而非被动接受观测。
9. 与显式 3D/几何表示的互补：局部建模是否可与几何一致表示结合，用于长时程空间记忆。

四、接触与触觉
10. 触觉数据规模化与触觉-视觉联合预训练（而非仅后训练）；力/触觉的跨硬件统一表征。
11. 接触事件的显式事件表征与预测（碰撞、滑动、形变），以支撑装配、插拔、液体处理等任务。

五、AI for Science 方向（与用户画像高度相关）
12. 从单一 MOF 流程扩展到跨化学/生物协议：是否可形成“实验操作协议”的可迁移表征；定量移液之外的精密度敏感操作（配液、稀释、分装、样品转移）。
13. 自驱动实验室闭环：世界模型预测与实验设计（主动学习/贝叶斯优化）的接口；失败实验的自动诊断与恢复策略。
14. 评测体系：真实科学任务缺少标准化基准，能否构建可复现的实验室自动化评测集（含成功判据、仪器接口、容错标准）。
15. 安全与可靠性：化学与生物场景中的错误代价不可逆，如何给出安全约束、弃权机制与人工接管协议。

六、跨领域联系（推测）
16. 与工业机器人（力控装配、打磨）、医疗操作（手术机器人高精度力控）、家务机器人（长时程多阶段任务）的迁移；异步多频 + 缓存复用作为通用“基础模型实时化”范式，可能同样适用于对话/长上下文推理等非机器人领域。

Q7: 总结一下论文的主要内容

【证据范围声明：本次未获取 PDF 正文与任何段落级检索证据，以下总结严格基于论文题录与摘要，外加显式标注的推断；不含任何未披露的数值、基线或数据集细节。另外，题录元数据存在时序异常（arXiv 编号 2609.27656 前缀“2609”对应 2026 年 9 月，与给出的发布日期 2026-09-24 自洽但与当前时间不符），该论文的真实性与最新版本状态建议核对，本总结的结论以后续核实的原文为准。】

一、论证主线（问题—主张—定位）
论文的论证起点是一个对“物理智能”的重新定义：Physical intelligence requires more than predicting how the world may evolve: predictions must remain actionable as the world continues to change。也就是说，世界模型的评价标准不是“预测是否逼真”，而是“预测是否能在世界持续变化、观测不完整、存在外部影响的情况下持续驱动有效动作”。围绕这一标准，作者提出 InternW0——上海人工智能实验室 InternW 物理世界模型系列的第一个实例，其设计三支柱为：omnimodal 接口、异步多频处理、以及部分观测与外部影响下的局部物理建模。最后作者把成果定位为“推进可扩展、异步、科学原生的物理世界模型，面向通用且高效的真实世界交互”，其中 science-native 是一个明确的价值主张：科学实验场景不是演示，而是模型能力的检验场。

二、技术主线（架构—机制—训练—适配）
1. 双专家非对称架构：InternW0 通过 asymmetric video–action architecture 联合学习未来视觉动态与连续机器人控制。视频专家容量大、负责更长时程的预测上下文；动作专家轻量、运行在更快的时间尺度上。二者用 flow matching 建模（具体目标构造需原文核实）。
2. 复用而非重算：核心效率设计是不为每次动作更新重新生成未来，而是复用层级 K/V 缓存（layerwise K/V），并通过 observation-conditioned context routing 把复用的上下文按最新观测重新路由，使陈旧上下文适配新状态。这是全文最值得深挖的技术判断：把“重算世界”替换为“检索并纠偏世界缓存”。
3. 本体适配：domain-specific interfaces 与 soft prompts 用于支持异构本体，推断是以轻量条件化替代按本体复制参数。
4. 接触感知后训练：在接触丰富的操作场景中，用后训练阶段引入力与触觉信号（而非从头联合预训练），这暗示作者把触觉视为在视觉世界模型之上的领域对齐层。
5. 数据：约 7,200 小时异构机器人与第一人称数据，含 EgoLab（275 小时真实实验室第一人称数据集，兼具数据贡献意义）。

三、实验主线（评测范围）
评测被设计为“仿真 + 真实科学”双轨：一方面覆盖仿真基准；另一方面直接进入真实世界科学任务，包括一个 15 阶段的金属-有机框架（MOF）合成工作流，以及一个 5 阶段的、接触与力感知的灵巧操作任务，用于通用定量移液。这两类任务在性质上互补：MOF 流程检验长时程多阶段序列的执行与阶段间衔接；定量移液检验接触/力控精度与连续操作的定量可靠性。

四、关键缺口
摘要未给出任何数值结果、基线、基准名称、消融设置或效率指标，因此无法判断：异步与 K/V 复用带来的精度代价、7,200 小时数据的迁移收益、长期一致性是否被局部建模损害、真实科学任务的成功率与失败模式。上述均需回原文的 Experiments、Ablation、Discussion 与附录确认。

五、结论性判断（校准后的表述）
从摘要可确定的贡献是：一条“世界模型必须可执行、可异步、可复用缓存”的设计主张，一套对应的非对称视频-动作架构与接触感知后训练流程，一份包含 7,200 小时异构数据与 EgoLab 数据的训练语料，以及把评测推进到真实科学实验流程的任务设定。这些主张是否成立、在何种边界条件下成立，当前证据不足以判断。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与画像方向 agent（权重 0.10）直接相关：论文本质上是面向具身智能体的世界模型 + 控制架构，涉及动作生成、长时程预测与闭环交互，是 agent 在物理世界中的基础模型路线。

## 基本信息

- 作者：Jisong Cai, Yao Mu, Ganlin Yang, Zhe Cao, Zhangzheng Tu, Xing Gao, Kailin Li, Xinyu Zhan, Lixin Yang, Yangkun Zhu, Haoxiang Ma, Ming Zhou, Qiaojun Yu, Yufei Xue, Liqun He, Yifei Yao, Yifan Zhu, Long Ling, Bingqi Jiang, Haoyu Guo, Xueyue Zhu, Bowen Zhou, Bin Zhao, Tianfan Xue, Chunhua Shen, Weinan Zhang
- 机构：Shanghai AI Laboratory（上海人工智能实验室）——依据摘要中“the first instantiation of the InternW physical world model series from Shanghai AI Laboratory”直接推断；具体作者的分属单位（如是否含高校联合培养单位）因未获取 PDF 首页与作者脚注，无法确认。
- 来源：arxiv
- 主题/分类：cs.RO, cs.AI
- 日期：2026-09-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.27656`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成未检索到任何 PDF 证据片段（retrieved_evidence 与 field_evidence_map 均为空），全部内容严格基于论文题录与摘要，并在各字段内以“合理推断/推测/需原文核实”逐处标注了证据强度；同时提示题录中的 arXiv 编号 2609.27656 与发布日期 2026-09-24 存在时序异常，建议先核实论文真实性与版本。
