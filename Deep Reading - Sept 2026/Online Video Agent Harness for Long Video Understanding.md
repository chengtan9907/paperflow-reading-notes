---
user_id: "cheng tan"
paper_id: 11410
arxiv_id: "2609.12818"
title: "Online Video Agent Harness for Long Video Understanding"
publish_date: "2026-09-14"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.12818.pdf"
pdf_url: "https://arxiv.org/pdf/2609.12818"
abs_url: "https://arxiv.org/abs/2609.12818"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-14T10:23:15"
---
# Online Video Agent Harness for Long Video Understanding

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long video understanding · video agent · agent harness · tool use

## 一句话总结

VideoXAgent 提出一种“纯在线”的长视频理解 agent harness：它从原始视频文件与用户 query 出发，边规划边按需调用由数据驱动原子能力分类法组织的异构专家工具（脚本、VLM、检测、OCR、ASR、人脸识别等），在客观证据提示与预算感知控制下聚合多模态证据并消解冲突，从而在 Video-MME-Long、LongVideoBench-Long、LVBench、MINERVA 上以约 50k tokens 的 agent 上下文（MINERVA 上约为 1024 帧密集打包 baseline 的 15%）取得与前沿 LMM 和视频 agent 可比的性能。

## 摘要

> Long video understanding often behaves like a visual needle-in-a-haystack problem: query-relevant evidence is sparsely distributed across long temporal spans, while packing dense frames into a single VLM context incurs context rot and high cost. Existing video agents often rely on query-agnostic offline preprocessing or ad hoc tool sets, which can miss query-specific details and waste computation. In this work, we present VideoXAgent, a purely online video-agent harness for long video understanding that starts from the given video file and user query, plans and decomposes the task, invokes specialized expert tools on demand, and aggregates multimodal evidence to produce a final answer while resolving conflicts among observations. To support this on-demand invocation, we design a suite of heterogeneous expert tools guided by a data-driven taxonomy of atomic capabilities, spanning scripts, VLMs, and domain models (e.g., detection, OCR, ASR, face recognition). The harness further enforces objective evidence prompting and budget-aware control to curb hallucination and non-termination. Across Video-MME-Long, LongVideoBench-Long, LVBench, and MINERVA, VideoXAgent is competitive with frontier LMMs and video agents under a smaller context footprint—about 50k tokens of agent context per sample, even on hour-long videos. In particular, on complex video-reasoning benchmarks such as MINERVA, it matches this level while using only about 15% of the context of a 1,024-frame dense-packing baseline. Notably, the harness remains effective with a visually weak or even text-only orchestrator, suggesting that strong long-video understanding can emerge from progressive agentic evidence seeking rather than from packing the full video into a single context.
> Date: September 14, 2026
> Project page: https://go-agent-x.github.io/video\_agent\_harness/
> ![](images/c61776cd63c889ac2c1ce928e4d3b870d7aee8ce3814682fe73a24105522adf9.jpg)
> Figure 1: Two video-agent paradigms. (a) Prior agents first run a query-agnostic preprocessing stage: they split the full video into fixed-length segments, uniformly sample frames, and parse it offline into an intermediate database or hierarchical memory. Query-specific reasoning and retrieval begin only after this index exists, so evidence not preserved by the fixed sampling can be missed, and a full-video cost is incurred before any query. (b) Our purely online agent starts from the raw video file together with the user query, and acquires task-relevant evidence on demand by invoking tools on query-conditioned spans.

Q1: 这篇论文试图解决什么问题？

## 1. 论文自我定位的核心问题

论文要解决的是“长视频理解”在工程与认知两个层面同时失效的问题：query 相关证据在时间轴上高度稀疏（视觉大海捞针），而当前主流做法要么把所有帧压进一个超长上下文，要么在回答之前先做与 query 无关的全局预处理。

## 2. 被点名的两类失效机制

（1）密集打包（dense packing）路线的问题：摘要明确说“packing dense frames into a single VLM context incurs context rot and high cost”。也就是说，问题不只是费用，还包括长上下文本身导致的视觉-语言模型有效推理能力退化。这是论文把“上下文长度”从纯成本变量提升为“性能-成本联合约束”的关键判断。

（2）离线预处理路线的问题：摘要指出已有视频 agent“rely on query-agnostic offline preprocessing or ad hoc tool sets”，后果有两层——可能忽略 query 特有的细节（因为预处理时不知道要问什么），以及浪费计算（因为预处理了对本次 query 无用的内容）。

## 3. Introduction 片段补充的两个更具体论证

根据检索证据中来自 Introduction 的片段，作者对“统一降采样（uniform downsampling）”给出了两点批评：（1）可能错过包含 query 相关证据的关键帧；（2）会引入大量无关帧。前者是召回失败，后者是精度/成本失败，两者叠加说明“固定采样率 + 单次前向”的范式在长视频上存在结构性矛盾：采样越密成本越高且 context rot 越严重，采样越稀关键证据越可能丢失。

同一片段还指出，现有 agent 设计虽然在某些选定场景下有效，“lack a systematic account of what capabilities are intrinsically required by complex and long-tail video understanding scenes”，结果是工具集可能冗余、也可能缺能力。这把问题从“怎么调工具”提升为“应该有哪些原子能力”的工具集设计问题，是本文相对增量式 agent 工作的重要区分点（此判断基于该片段，属论文自述立场）。

## 4. 隐式假设与边界条件

- 假设一：query 在推理时已知，因此可以做到“query-specific”的证据搜寻——这排除了需要预先建立通用索引以备任意未来 query 的场景（如需为视频库建可复用索引的检索式系统）。
- 假设二：可用的专家工具（检测、OCR、ASR、人脸识别等）足够可靠，工具误差不会主导最终答案质量。材料中未给出对工具误差传播的量化分析，属需要回原文确认的缺口。
- 假设三：以固定 token 预算（约 50k）约束 agent 上下文是合理且可迁移的代理指标；但不同基准、不同时长视频下这一预算是否都是最优，材料未提供敏感性分析。
- 反例/失败模式：摘要与片段未给出失败案例的定量刻画（哪些 query 类型会失败、在多少时长上退化），这部分需要回原文实验与讨论章节核实。

Q2: 有哪些相关研究？

重要说明：本次检索证据中未包含论文的 Related Work 章节原文，以下按摘要、Introduction 与 Limitations 片段中出现的对照对象重建研究脉络，属于“基于摘要与片段的重建”，具体引用文献、对比方法名称须回原文核对。

## 1. 长视频理解的“密集帧打包”路线

论文把 1024 帧密集打包作为明确的对照 baseline（摘要明确提到 15% 上下文的对比），这类方法把尽可能多的帧送进一个长上下文 LMM 做一次性推理。论文对它的批评是 context rot 与高成本（摘要明确支持）。这条路线对应的是以 Video-MME、LongVideoBench、LVBench 等长视频基准为主要评测对象的通用 LMM 视频理解工作（基准名称由摘要给出，具体对应哪些模型未在材料中列出）。

## 2. 视频 agent 的“离线预处理”路线

摘要点名已有视频 agent 依赖 query-agnostic 的离线预处理。合理推断，这类工作会先做全局字幕生成、关键帧抽取、场景切分或索引构建，再在回答阶段检索。论文的核心反驳是：preprocessing 发生在知道 query 之前，因此无法保证保留 query 相关证据，同时会为无关内容付出计算。

## 3. 工具增强 / 多模态工具调用的 agent 路线

摘要提到已有工作使用 ad hoc tool sets，Introduction 片段进一步指出这些设计缺乏对“复杂与长尾视频理解场景所需原子能力”的系统性说明，因此可能出现冗余工具。本文的对照点因此是：不以任务定制堆工具，而是以数据驱动的 atomic capability taxonomy 来组织工具集。

## 4. 与 agent harness / 上下文工程的关系

论文使用 harness 一词并强调 objective evidence prompting 与 budget-aware control，合理推断其与近期 agent 系统研究中关于上下文管理、终止条件、幻觉抑制的讨论相关。但材料未给出具体引用的 harness 或 agent 框架文献，需回原文确认其技术谱系。

## 5. 基准侧的相关工作

Video-MME-Long、LongVideoBench-Long、LVBench、MINERVA 四个基准由摘要明确给出，其中 MINERVA 被描述为“复杂视频推理基准”，暗示其考察更侧重多步推理而非单帧识别。这些基准本身的构建动机（长时长、长尾能力覆盖）与本文“taxonomy 应覆盖长尾能力”的动机高度相关，属合理推断的关联。

Q3: 论文如何解决这个问题？

## 1. 总体范式：从“先处理视频再提问”改为“边搜索边回答”

VideoXAgent 被定义为 purely online 的 video-agent harness：输入是原始视频文件 + 用户 query，不要求在回答前完成全视频的离线解析。流程为：规划与任务分解 → 按需调用专家工具 → 聚合多模态证据 → 生成最终答案，并在聚合阶段处理观察之间的冲突（摘要明确支持）。

## 2. 工具层：数据驱动的原子能力分类法

为支撑按需调用，作者设计了一套异构专家工具，覆盖三类：(a) scripts（脚本类工具，摘要明确列出）；(b) VLMs；(c) domain models，例如 detection、OCR、ASR、face recognition（摘要明确列出）。工具选择的组织原则是 data-driven taxonomy of atomic capabilities，即先刻画复杂与长尾视频理解场景所需的原子能力，再据此决定工具集，而不是按任务临时堆工具（taxonomy 的构建数据来源与粒度在材料中未给出，需回原文确认）。

## 3. 控制层：两条抑制机制

- objective evidence prompting：用于抑制幻觉。摘要仅说明其存在与目的，具体提示模板、如何判定“客观证据”、如何约束生成内容与观察一致，材料未提供。
- budget-aware control：用于抑制“不终止”问题，即防止 agent 在证据搜寻中无限循环。摘要明确其目的；具体预算形式与终止判据需回原文确认。

## 4. 状态与决策的形式化（来自检索片段的有限信息）

来自 3.1.1 Problem Formulation 的片段显示：agent 的状态包含历史、观察与证据，以及剩余预算 b_k；基于该状态，agent 要么用 query-specific 的参数调用某个工具，要么直接产出最终答案；状态、工具调用与观察的序列构成一条轨迹。这是本文把“证据搜寻”写成序贯决策问题的直接证据。需要注意：片段被截断，完整的形式化定义（动作空间、奖励或启发式策略、预算递减规则）需回原文核对。

## 5. 上下文与成本设计目标

设计目标是在较小上下文占用下保持竞争力：摘要给出的量级是每个样本约 50k tokens 的 agent 上下文，即便视频长达小时级；在 MINERVA 上约为 1024 帧密集打包 baseline 的 15%。这说明 harness 的核心权衡是“用多次、窄范围的工具调用替代一次、宽范围的上下文填充”（此为对摘要的机制性解读，属合理推断）。

## 6. 编排器可弱化这一设计取向

摘要明确称：harness 在编排器视觉能力弱、甚至纯文本时依然有效。这意味着系统把大部分“感知”外包给专家工具，把编排器职责压缩为规划、调用与证据整合，从而降低对单一强 VLM 的依赖。该结论的适用边界（多弱的编排器会失效）在材料中未给出，需回原文确认。

Q4: 论文做了哪些实验？

## 1. 评测基准

摘要明确列出四个基准：Video-MME-Long、LongVideoBench-Long、LVBench、MINERVA。其中 MINERVA 被描述为“复杂视频推理基准”。四个基准的 split、评测协议、prompt 设置、是否使用官方 judge 等细节，材料中未提供。

## 2. 对比对象

摘要称 VideoXAgent 与 frontier LMMs 和 video agents 进行比较（competitive with frontier LMMs and video agents）。具体对比了哪些模型与 agent 系统、各自帧数/上下文设置，材料未列出，需回原文实验表核对。

## 3. 上下文足迹对比（本文的关键实验设计）

- 总体：约 50k tokens 的 agent 上下文 per sample，覆盖小时级视频。
- 对照：1024 帧 dense-packing baseline；在 MINERVA 上，VideoXAgent 使用约 15% 的上下文达到同等水平。
这是本文最核心的实验叙事：不是只报分数，而是把“分数—上下文成本”作为联合指标。该对比的具体口径（15% 指 token 数还是峰值上下文、是否包含工具调用产生的 token、是否对齐了帧数分辨率）材料未说明，属需要回原文确认的关键计量问题。

## 4. 编排器能力消融（摘要明确提及）

摘要报告 harness 在编排器视觉能力弱、甚至 text-only 时仍有效。这构成一组重要的能力解耦实验：把“感知”交给工具，把“编排”留给语言模型。消融的具体梯度（视觉弱到什么程度、text-only 时分数下降多少）材料未给出。

## 5. 方法内部消融（材料缺口）

摘要与片段提到 taxonomy 指导的工具集、objective evidence prompting、budget-aware control 三个组件，但未提供针对它们的消融数据（例如去掉 budget-aware control 后是否出现不终止、去掉 objective evidence prompting 后幻觉率变化）。这些实验是否存在、结果如何，需回原文核实。这部分是评判断言可信度的关键，目前属于证据缺口。

Q5: 发现了什么实验现象？

## 1. 核心现象：小上下文可与密集打包持平

摘要明确报告，在 MINERVA 上 VideoXAgent 以约 15% 的上下文达到与 1024 帧 dense-packing baseline 相当的水平。这直接支持论文的中心论点：长视频理解不必依赖把全视频塞进单一上下文，栅格式增加帧数带来的收益存在明显边际递减，而其中相当一部分“上下文预算”被无关帧消耗。

## 2. 反直觉/值得追问的结果：弱视觉甚至纯文本编排器仍有效

摘要称 harness 在 visually weak 甚至 text-only orchestrator 下仍然有效。这是一个对主流直觉（长视频理解必须有强视觉编码器）构成挑战的观察：性能似乎主要来自“渐进式 agentic 证据搜寻 + 专家工具”，而非编排器自身视觉能力。可能的机制解释（推测）是：编排器只需要判断“该调用哪个工具、在什么时间窗、用什么参数”，以及整合已经是结构化/文本化的观察结果，因此对视觉表征的依赖被显著卸载。其边界（纯文本编排器在哪类 query 上崩掉）需要回原文确认。

## 3. 成本-效果张力

小时级视频仍维持约 50k tokens 的 agent 上下文，说明上下文占用对视频时长不敏感（摘要明确）。相较之下，密集打包的上下文随时长/帧数近似线性增长。这是一个关于 scaling 行为的定性观察：agent 路线的成本曲线更平，代价是引入多次工具调用的串行延迟（延迟与调用次数在材料中未量化，属缺口）。

## 4. 幻觉与不终止是被显式处理的现象

论文把 objective evidence prompting 与 budget-aware control 作为一等机制写入摘要，合理推断作者在实验中发现无约束的 agent 会出现两类失稳：生成与观察不符的结论（幻觉），以及无法收敛到最终答案（不终止）。但材料未给出这两类失败的发生率、触发条件或反例案例，属需要回原文补充的证据缺口。

## 5. 证据冲突的存在性

摘要提到 harness 会“resolving conflicts among observations”。这暗示实验中出现过不同工具给出不一致观察的情况（如 ASR 与 OCR 对同一片段的信息不一致）。冲突的具体类型分布与消解策略的效果，材料未提供。

Q6: 有什么可以进一步探索的点？

以下方向中，前两条直接来自论文自述或摘要的边界，其余为基于材料的合理推断/推测，需回原文核对作者是否已讨论。

## 1. 从“按需工具”走向“能力自发现”

论文以数据驱动的 atomic capability taxonomy 组织工具，但 taxonomy 一旦固定，就存在长尾覆盖问题。可探索：（a）taxonomy 是否会随新基准/新场景失效，如何自动扩展；（b）能否让 agent 在遇到缺能力时合成脚本工具（摘要提到 scripts 类工具）而非仅调用预置工具；（c）工具冗余与工具缺失的量化诊断方法。

## 2. 预算感知控制的策略学习

budget-aware control 目前的目的被描述为防止不终止。进一步的问题是：预算应如何分配？是先粗后细（coarse-to-fine）扫描，还是依据 query 类型先验跳转？这可以形式化为预算约束下的证据获取策略优化问题（推测性方向）。同时需要研究预算-精度曲线的拐点，以及预算在不同时长视频上的可迁移性。

## 3. 弱编排器的能力下限

摘要观察到 text-only 编排器仍有效。自然的后续是给出“编排器能力—最终性能”的 scaling 曲线，并界定何时必须依赖视觉能力（例如需要跨帧空间关系推理时）。这对低成本部署（用文本 LLM 做编排、用工具做感知）有直接价值。

## 4. 冲突消解与证据可信度建模

如果能显式建模每个工具观察的可信度（依据工具类型、时间窗、置信度），冲突消解可从启发式升级为概率推断。可探索：工具误差传播分析、跨工具交叉验证、以及把冲突消解结果作为可解释证据链输出。

## 5. 与检索/记忆机制的耦合

当前范式是 query 已知条件下的在线搜寻，反过来对“为未知未来 query 预先建索引”的场景不友好。可探索混合范式：轻量可复用索引 + 在线 query-specific 精搜，并研究两者成本/召回的分离点（合理推断的相邻问题）。

## 6. 流式与超长视频

材料只覆盖小时级视频与离线给定文件的情形。可探索流式场景（边到达边回答）、跨天级超长视频、多视频联合问答，以及这些设定下预算控制与证据聚合的重设计（推测）。

## 7. 评测协议改进

需要在协议层面回答：上下文成本如何计量才公平（峰值 vs 累计、是否计入工具调用 token）、串行工具调用的延迟如何纳入比较、以及是否能建立类似“同等上下文预算下比较”的标准设置。这对整个视频 agent 子领域的可比性都很关键（合理推断）。

Q7: 总结一下论文的主要内容

## 一、论文要解决的问题与论证主线

论文从长视频理解的一个结构性困难出发：与 query 相关的证据在时间轴上稀疏分布，类似“视觉大海捞针”；而主流的两条应对路线各有硬伤。第一条是密集帧打包——把大量帧塞进单个 VLM 上下文，摘要明确指出这会带来 context rot 与高成本。第二条是已有视频 agent 普遍采用的 query-agnostic 离线预处理与临时拼凑的工具集，摘要指出这既可能错过 query 特有的细节，也浪费计算。Introduction 片段进一步把“统一降采样”的两点缺陷写清：可能错过关键证据帧，同时引入大量无关帧。

论证主线因此是：如果证据获取可以在推理时依据 query 动态进行，就不必在不知道 query 的情况下预先决定保留哪些内容；如果感知能力由专门的工具承担，编排器就不必是强视觉模型，上下文也就不必承载全部像素级信息。摘要给出的最终结论是：长视频理解能力可以由“渐进式 agentic 证据搜寻”涌现，而不必来自把整段视频放进单一上下文。

## 二、技术主线：VideoXAgent 的 harness 设计

1）范式定位：纯在线（purely online）。系统从原始视频文件和用户 query 出发，边规划边获取证据，而不是先离线解析全视频。

2）流程结构：规划与任务分解 → 按需调用专家工具 → 聚合多模态证据 → 生成答案，并在聚合阶段消解观察之间的冲突。

3）工具集：由数据驱动的 atomic capability taxonomy 指导，覆盖脚本类工具、VLM 与领域模型（detection、OCR、ASR、face recognition 等）。这一设计回应的是 Introduction 片段指出的“现有 agent 缺少对复杂与长尾视频理解所需原子能力的系统性说明，因而工具可能冗余”的问题。

4）控制机制两条：objective evidence prompting 抑制幻觉；budget-aware control 抑制不终止。这两条是把 agent 从“能调用工具”推向“能稳定收敛地给出可靠答案”的关键。

5）形式化（来自 3.1.1 片段）：agent 状态包含历史、观察、证据与剩余预算 b_k；每步决策是“用 query-specific 参数调用工具”或“输出最终答案”；状态、调用与观察的序列构成轨迹。片段被截断，完整形式化定义需回原文。

6）成本设计：以较小的 agent 上下文（约 50k tokens/样本）支撑长视频，把“上下文长度”从性能的线性驱动变量改为受控资源。

## 三、实验主线与关键结果

评测覆盖 Video-MME-Long、LongVideoBench-Long、LVBench、MINERVA 四个长视频基准。摘要给出的结论是：在更小上下文占用下，VideoXAgent 与前沿 LMM 和视频 agent 保持竞争力；小时级视频仍维持约 50k tokens 的 agent 上下文；在 MINERVA 这类复杂视频推理基准上，其上下文约为 1024 帧密集打包 baseline 的 15% 而水平相当。

另一个被摘要单独强调的观察是：即便编排器视觉能力弱、甚至纯文本，harness 仍然有效。这一结果的价值在于把“感知”与“编排”解耦，暗示性能主要来自证据搜寻流程和工具，而非单一大 VLM 的视觉容量。

## 四、可信度校准与证据缺口

需要明确区分本总结的证据强度：上述关于动机、工具组成、控制机制、四个基准、50k tokens、15% 上下文、弱编排器有效性等内容均由摘要直接支持；关于状态形式化（b_k、轨迹）由检索片段支持但片段截断；关于“失败案例、工具消融、冲突消解细节、延迟与成本量化、评分具体数值”等内容在提供的材料中均缺失，属于必须回原文核实的缺口。此外，元数据中的发布日期为 2026-09-14、arXiv 编号为 2609.12818，用户在引用前应自行核实版本与出处信息。整体而言，本文的贡献形态偏向“系统与协议设计”而非单点算法创新，其说服力高度依赖于对比口径（上下文如何计量、工具调用是否计入成本）是否在原文中交代清楚。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像的 agent 方向（权重 0.10）直接重合：本文是典型的 agent harness 设计，关注工具调用、预算控制、终止条件与证据聚合，可作为长视频域 agent 系统设计的参考模板。

## 基本信息

- 作者：Sen Yang, Boqiang Duan, Jing Yang, Weihao Bo, Jie Liu, Boyuan Tong, Ze Feng, Wenkang Zhang, Jingdong Wang, Hua Wu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-09-14
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.12818`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 72 个片段，主要来自 Abstract、Introduction、3.1.1 Problem Formulation 与 Limitations and Conclusion，但 Related Work 与完整实验章节的原文未命中，故相关字段已标注为基于摘要的重建或证据缺口；元数据中的发布日期 2026-09-14 与 arXiv 编号 2609.12818 建议自行核实。
