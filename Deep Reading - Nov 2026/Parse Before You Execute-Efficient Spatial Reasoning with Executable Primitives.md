---
user_id: "cheng tan"
paper_id: 19897
arxiv_id: "2610.41389"
title: "Parse Before You Execute: Efficient Spatial Reasoning with Executable Primitives"
publish_date: "2026-11-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2610.41389.pdf"
pdf_url: "https://arxiv.org/pdf/2610.41389"
abs_url: "https://arxiv.org/abs/2610.41389"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-02T22:02:54"
---
# Parse Before You Execute: Efficient Spatial Reasoning with Executable Primitives

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：spatial reasoning · vision-language models · tool-integrated agents · executable primitives

## 一句话总结

PEPS 是一种免训练的分层智能体框架：它先把自然语言空间查询解析为 Frame、Entity、State、Metric 四类可执行原语，再由固定几何 API 执行并由验证器检查执行充分性，从而在多个空间推理基准上同时提升准确率并降低迭代与工具成本。

## 摘要

> Spatial reasoning in Vision-Language Models (VLMs) has increasingly benefited
> from tool-integrated pipelines that ground computation in explicit geometry. How-
> ever, existing systems often compile queries into relatively open-ended artifacts,2026 such as code, task constraints, tool plans, or gathered evidence, leaving what must
> be resolved before execution under-specified. This can lead to unnecessary tool
> acquisition and iterative refinement, while still producing plausible reasoning tracesOct from incomplete spatial commitments. We identify this failure mode as under-
> 30 committedagentic paradigmqueryParsingcompilation.ExecutableTo addressPrimitivesit, weforproposeSpatial reasoningPEPS, a undertraining-freea typed
> FESM schema over Frame, Entity, State, and Metric. PEPS follows a parse-before-
> execute principle: the Parser commits each query to a compact set of executable
> requirements; the Executor selectively grounds the required primitives through fixed
> geometric APIs and deterministic computation; and the Verifier checks execution
> sufficiency, enabling early termination or targeted repair. Extensive experiments
> across challenging spatial reasoning benchmarks show that PEPS consistently[cs.CV] improves reasoning accuracy while substantially reducing refinement rounds, API
> cost, and end-to-end latency over strong tool-integrated baselines. These results
> demonstrate that explicit requirement commitment can make tool-integrated spatial
> reasoning not only more reliable, but also more execution-efficient. Source code is
> available at https://github.com/Xiwei-web/PEPS.

Q1: 这篇论文试图解决什么问题？

【核心问题】论文试图解决的不是“VLM 看不看得见”这一层面的感知问题，而是工具集成式空间推理在“查询到执行”之间的接口问题：自然语言空间查询在被几何工具执行之前，必须先被翻译成一组明确的空间承诺，包括用哪个参考系、指涉哪些实体、需要哪些状态关系、需要哪些度量。现有系统往往把查询编译成相对开放的产物，例如代码、任务约束、工具计划或待收集证据，却没有把“执行前必须解决什么”显式写清楚。作者将这一失败模式命名为 under-committed query compilation，即查询编译欠承诺。

【失败链条】在欠承诺状态下，执行器不知道哪些工具调用是必要的，容易进行不必要的工具获取与多轮迭代；更隐蔽的是，系统仍然可以生成看似合理的推理轨迹，因为语言模型可以用流畅叙述掩盖空间承诺的缺失。论文认为这会同时造成两类代价：一是可靠性问题，答案可能建立在未经验证的参考系、实体对应或度量假设上；二是效率问题，工具调用、API 成本和端到端延迟被浪费在无关或重复的感知与计算上。

【形式化表述】论文把问题写成：给定图像集合 I={I1,…,IM}、自然语言查询 q、真值答案 a*，以及一个定义在 Frame、Entity、State、Metric 上的类型化原语库 L，目标是恢复一个紧凑且充分的可执行原语集合 P⊆L，使得通过确定性几何计算能够回答 q。这里的关键词是“紧凑”和“充分”：P 不能是全部原语的堆砌，也不能遗漏回答查询所必需的承诺。Parser 返回候选需求集，Executor 通过固定 API 工具箱 S 选择性实例化未解决的原语，Verifier 根据查询、视觉证据和执行轨迹判断候选是否被充分支持，并返回接受/拒绝、质量分数和结构化反馈。

【为什么难】难点在于自然语言空间查询通常隐含参考系、粒度、比较对象和度量单位。例如“能否把沙发搬过门”需要门宽、沙发最大尺寸、能否倾斜等承诺，而大量视觉细节无关。模型既可能漏掉关键承诺，也可能过度承诺、引入无关工具。另一个难点是验证：执行轨迹“看起来合理”不等于答案被充分支持；验证器必须区分“答案正确”与“执行足以支持答案”。

【隐含假设】论文的方法隐含假设包括：空间查询可以被分解为有限类型化原语；固定几何 API 能对这些原语做可靠 grounding；验证器能够根据查询、图像和轨迹判断执行充分性；解析器和验证器的错误不会系统性地破坏闭环。上述假设在开放世界、严重遮挡、动态场景或工具覆盖不足时可能不成立。给定片段没有给出完整的失败案例分析，因此这些边界需要回原文确认。

Q2: 有哪些相关研究？

【说明】检索片段主要覆盖引言、方法、实验设置和结论，未包含完整的 Related Work 章节；以下按论文引言中明确提到的研究脉络与实验 baseline 重组，属于“有证据的重构”，不是对原文 related work 的逐段复述。

【1. 端到端 VLM 空间推理】一类工作直接用专有或开源视觉语言模型回答空间问题，代表模型包括 GPT-4o、o3、Gemini-2.5 系列、Llama-3.2-Vision、LLaVA-OneVision、Qwen2.5/Qwen3-VL、InternVL2.5/InternVL3。这类方法的优势是通用性强、无需显式几何接口，但在精确距离、视角变换、遮挡关系和多图一致性上容易出现语言先验压过视觉证据的问题。PEPS 不与之竞争“更大模型”，而是把几何计算外置到确定性工具。

【2. 训练式 3D-VLM】另一条路线通过引入 3D 表示或 3D 增强推理来提升空间理解，代表工作包括 Video-3D-LLM、LLaVA-3D、VLM-3R、VILASR。它们通常需要额外的 3D 数据、特征对齐或训练流程。PEPS 的定位是免训练的系统层方法，原则上可以与这类 3D 表征结合，但论文在给定片段中没有展开这种组合。

【3. 工具集成与程序合成】工具集成路线把部分推理从自由文本生成转移到显式工具或程序，代表工作包括 ViperGPT、VisProg、VADAR、pySpatial、GCA、MSSR。ViperGPT/VisProg 类方法将查询编译为可执行程序或模块序列；VADAR、pySpatial、GCA 更强调几何 API 或空间工具调用；MSSR 强调为下游推理收集足够证据。这些系统的共同点是“用外部执行提高可靠性”，但中间产物往往较开放：代码、任务约束、工具计划或证据集合。PEPS 的差异在于，它不生成开放式程序，而是要求 Parser 先提交一个受类型系统约束的原语集合，再让 Executor 只实例化必要原语。

【4. 验证与迭代修复】现有 agentic 系统常通过多轮工具调用、反思或证据补全来修正错误，但迭代成本高，而且缺乏“执行是否充分”的显式判据。PEPS 的 Verifier 返回接受/拒绝、质量分数和结构化反馈，支持提前终止或定向修复。与只检查最终答案是否合理的方法相比，这更接近“执行轨迹级”的充分性检查；但验证器本身的可靠性仍是关键，给定片段未给出其校准细节。

【5. 类型化中间表示与神经符号接口】PEPS 的 FESM schema 可以看作一种轻量神经符号接口：自然语言侧由 VLM/LLM 解析，几何侧由固定 API 执行，中间用 Frame、Entity、State、Metric 约束可提交的承诺。论文强调 FESM 不是表面空间关系的分类学，而是面向执行的原语接口。与任意代码生成相比，受限原语集合更可检查、更易验证，但也带来 schema 覆盖范围有限的问题。

Q3: 论文如何解决这个问题？

【总体框架】PEPS 是一个免训练的多阶段闭环系统，核心原则是 parse-before-execute：在调用感知或几何工具之前，先让查询对“需要知道什么”作出显式承诺。系统由 Parser、Executor、Verifier 三个角色组成，围绕一个类型化原语库 L 工作。形式化地，Parser 根据图像 I、查询 q 和原语库 L 产生候选需求集 P；Executor 根据 I、q、P 和固定 API 工具箱 S 产生答案与执行轨迹 T；Verifier 根据 I、q、答案和执行轨迹返回接受/拒绝决定 v、质量分数 s 和结构化反馈 F。若被拒绝，反馈进入下一轮解析或定向修复；若接受或达到最大迭代次数则终止。

【FESM schema】FESM 是 Frame、Entity、State、Metric 四类原语的类型化 schema。按论文命名与上下文，Frame 对应参考系、视角、相机位姿或坐标基准；Entity 对应物体、区域、锚点等可指涉实体；State 对应实体属性、空间关系、可见性、朝向、包含关系等状态承诺；Metric 对应距离、尺寸、角度、数量等可计算度量。论文强调 FESM 不是表面空间关系的 taxonomy，而是执行接口：每个原语都要能被固定几何 API grounding，并参与确定性计算。给定片段没有展开完整原语清单，具体字段需看附录 G。

【Parser】Parser 的职责不是生成自由代码，而是把查询提交为 L 中的原语实例集合。它的输出被限制在 L 内，因此解析结果是一个显式、可检查的“查询表征需求声明”。论文把解析与执行分开，是因为“需要什么”应该在工具调用之前被判断，而不是让执行器在开放计划中边做边猜。Parser 可能检索候选需求集 P，并标记哪些原语尚未解决；后续 Executor 只对这些未解决原语做选择性 grounding。

【Executor】Executor 通过固定几何 API 工具箱 S 实例化所需原语，并做确定性计算。与“把所有可用工具都跑一遍”不同，PEPS 的选择性 grounding 只针对 P 中要求的原语，目的是减少无关工具获取和重复感知。方法片段还提到 Reference Frame Grounding 与共享场景表示：所有 anchor points 相对同一个场景表示提取，为后续计算提供一致基座。这说明 PEPS 不只是调用工具，还试图让多图或多视角下的空间承诺落在同一参考框架中。

【Verifier】Verifier 检查执行充分性，而不是只判断答案表面是否合理。它接收查询、图像、答案和执行轨迹，输出接受/拒绝、质量分数与结构化反馈。若拒绝，反馈 F 可以指导定向修复，例如补做缺失原语、修正参考系或重新 grounding 某个实体；若接受，则可以提前终止，避免不必要的迭代。这个设计把“继续调用工具”从默认行为变成有条件的修复行为。

【设计权衡】PEPS 的优点是中间表示受限、可检查、可验证，且不需要训练；代价是系统依赖 schema 设计、Parser 的提交质量和 Verifier 的判断质量。受限原语库可能覆盖不了开放查询；解析过严可能漏掉必要承诺，解析过松则退化为开放计划。论文声称在准确率与效率上同时获益，但给定片段没有给出完整的消融实验来分离 Parser、Executor、Verifier 各自的贡献，这一点需要回原文核对。

Q4: 论文做了哪些实验？

【评测目标】论文的实验设计同时考察两类指标：空间推理准确率，以及执行效率。摘要明确说 PEPS 在 challenging spatial reasoning benchmarks 上 consistently improves reasoning accuracy，同时 substantially reducing refinement rounds、API cost 和 end-to-end latency。也就是说，实验不仅比较“答得对不对”，还比较“用多少轮修复、多少 API 调用、多少端到端时间答出来”。

【基准选择】论文选了三类互补 benchmark。MMSI-Bench 是人工整理的多图真实场景空间推理基准，覆盖相机位姿、物体布局、区域关系及其组合，并有 multi-step 子集测试更长链条上的一致空间推理。ViewSpatial-Bench 关注视角敏感的空间理解，尤其是 egocentric 与 allocentric 推理，覆盖多个任务类别和变化视角。OMNI3D-Bench 是单视图、单目观测下的细粒度 3D 空间推理基准。三者分别覆盖多图、视角变换和单目 3D，选型上有意避免只在单一场景类型上验证。

【对比基线】论文比较四组方法：1）专有 LLM，包括 GPT-4o、o3、Gemini-2.5 家族；2）开源 VLM，包括 Llama-3.2-Vision、LLaVA-OneVision、Qwen2.5/Qwen3-VL、InternVL2.5/InternVL3；3）训练式 3D-VLM，包括 Video-3D-LLM、LLaVA-3D、VLM-3R、VILASR；4）工具集成 agent，包括 VADAR、pySpatial、GCA、MSSR、ViperGPT、VisProg。这个 baseline 组合覆盖了“直接回答”“3D 增强”“工具/代码/证据 agent”三类竞争范式，比较维度较完整。

【实现与协议】结果片段提到 PEPS 被实现为 modular multi-agent pipeline，但给定片段在“where…”处截断，未给出完整的基座模型、API 清单、迭代预算、提示词和验证器实现细节。方法部分提到附录 G 有 FESM schema，说明论文把 schema 细节放在附录。实验表格片段包含多列数值与误差标记，但表头缺失，无法可靠还原每一列对应的 benchmark 或子任务。

【可复现性线索】摘要给出开源代码链接 https://github.com/Xiwei-web/PEPS。给定片段没有列出训练数据、硬件、API 价格版本或统计显著性检验；由于 PEPS 是免训练方法，复现重点会落在 schema、prompt、工具 API 和验证器阈值上，而不是模型权重。

【证据缺口】给定检索片段没有包含完整的消融实验、失败案例、人工评估、不同基座模型的敏感性分析，也没有展示效率指标的绝对值。论文声称的“减少 refinement rounds、API cost、latency”需要看原文的实验表和测量口径，尤其是 API cost 是否按统一价格表计算、latency 是否包含所有模型调用与工具等待。

Q5: 发现了什么实验现象？

【1. 跨基准准确率提升】摘要与结论都声称 PEPS 在 MMSI-Bench、ViewSpatial-Bench、OMNI3D-Bench 上 consistently improves reasoning accuracy。这种“三个互补基准同时提升”的现象说明收益不太可能只来自某一个数据集的标注偏置；但给定片段没有给出每个基准的完整数值和方差，因此不能量化“一致性”的强度。

【2. ViewSpatial 上的具体对比】检索片段明确给出：PEPS 在 ViewSpatial-Bench 上 overall score 为 52.4%，超过 GCA 的 47.4% 和 MSSR 的 51.8%，并被称为 new best overall score。相对 GCA 的提升约 5.0 个百分点，比较明显；相对 MSSR 仅约 0.6 个百分点，优势较小。这里出现一个值得注意的张力：MSSR 本身是强调证据收集的强 agent，PEPS 对它的准确率优势并不压倒性，论文的主要卖点可能更多在效率或跨基准稳定性上。片段没有给置信区间或显著性检验，0.6 个百分点是否稳定需要回原文确认。

【3. 其他基准上的趋势】片段称“same pattern carries over to OMNI3D-Bench”，并提到 MMSI-Bench 上 PEPS 相比某些工具 agent 有明显优势，但具体数值被截断。结果片段中还出现一组 PEPS 数值：51.3、48.2、52.6、52.9、51.1、51.9、52.9、52.4，以及带 ± 的误差项；由于表头缺失，不能可靠判断这些列分别对应哪些类别或平均值。可以确定的是，PEPS 在不同列上的表现有波动，说明不同子任务难度并不一致。

【4. 效率现象】论文摘要强调 PEPS 大幅减少 refinement rounds、API cost 和 end-to-end latency。这一现象与“欠承诺导致不必要工具获取和迭代修复”的诊断一致：当 Parser 先提交紧凑需求集，Executor 不需要把所有工具都试一遍，Verifier 也可以在执行充分时提前终止。但效率提升的绝对值、测量边界和是否以额外解析调用为代价，给定片段没有展开。

【5. 验证器的双面性】Verifier 支持 early termination 与 targeted repair，这理论上能减少无效迭代；但它也引入新的失败模式：验证器过松会接受“看似合理但承诺不完整”的答案，过严会触发不必要的修复轮次，抵消效率收益。给定片段没有给出 verifier 的接受率、误接受率或修复成功率，因此这部分只能作为合理推断，需原文确认。

【6. 子任务差异与指标张力】从结果片段可见，PEPS 在不同列上的分数大致在 48–53 区间波动，并非所有子任务都大幅领先。这提示“显式承诺”对参考系敏感、实体歧义强的任务可能收益更大；对工具覆盖充分、查询较简单的任务，收益可能被额外解析开销部分抵消。论文若只报告总体平均，可能掩盖这种任务异质性。

【7. 负结果与失败案例缺口】给定片段没有展示失败案例、反例或负结果。按照方法逻辑，可能的失败包括：Parser 漏提交关键原语、Executor 的几何 API 无法 grounding 某个实体、多图共享场景表示不一致、Verifier 误接受不充分轨迹。上述属于基于方法的合理推断，不是论文已报告的实验现象；需要看原文的 error analysis 才能确认。

Q6: 有什么可以进一步探索的点？

【1. schema 覆盖与自动扩展】FESM 目前是人工设计的类型化 schema，未来可以研究如何从失败案例中自动发现缺失原语，或让 schema 随任务域扩展。关键问题是：受限原语库如何在不牺牲可验证性的前提下覆盖开放空间查询。可以探索层级化 schema、组合原语和领域特定原语包。

【2. Parser 的学习与校准】PEPS 是免训练框架，Parser 依赖现成 VLM/LLM。后续可以研究轻量微调、偏好优化或约束解码，让 Parser 更稳定地提交“紧凑且充分”的集合；也可以引入不确定性估计，在解析置信度低时主动请求澄清或增加验证。需要比较“免训练 + 好 prompt”与“小规模训练 + 更强 schema 遵循”的性价比。

【3. Verifier 的可靠性】Verifier 是闭环的关键，但“执行充分性”本身难以标注。未来可以研究基于执行轨迹的反事实检查、工具返回一致性检查、多验证器集成，以及 verifier 的校准与拒答策略。还需要区分“答案正确”与“过程充分”，避免 verifier 只学会判断答案表面合理性。

【4. 不确定性与部分可执行】真实场景中，某些原语可能无法从图像中可靠 grounding。未来可以把 P 扩展为带概率或区间的需求集，让 Executor 在不确定时返回置信度，Verifier 根据风险决定是否接受。这与机器人操作、自动驾驶等安全敏感场景相关。

【5. 主动感知与多视角规划】PEPS 目前主要是在给定图像上选择性调用工具。未来可以把“需要哪个视角/哪次测量”纳入 P，形成主动感知策略：当关键原语无法从当前视角解析时，系统主动请求新视角或新传感器读数。这与具身智能中的主动空间推理直接相关。

【6. 动态与时序场景】当前 benchmark 以静态多图或单目 3D 为主。未来可以扩展到视频、动态场景、可交互环境，处理实体状态随时间变化、遮挡解除、运动轨迹等问题。FESM 的 State 维度可能需要时间索引和事件变量。

【7. 跨域迁移】parse-before-execute 的思想可以迁移到科学工作流、表格问答、CAD/建筑、医学影像测量等需要“先明确计算需求再调用工具”的领域。需要研究领域原语库、工具成本和验证标准的差异。论文本身不是生物医学应用，但其 typed primitive + verifier 模式对 AI for Science 的工具编排有借鉴价值。

【8. 系统与成本优化】可以研究缓存、批处理、工具调用剪枝、并行 grounding、按需升级模型等工程优化，并报告 accuracy-cost Pareto 前沿。也可以比较“少调用强模型”和“多调用弱工具”的策略。

【9. 评测协议】需要更细的评测：按子任务报告、给出方差与显著性、分离解析/执行/验证的消融、报告 API 价格版本与延迟分布、加入失败案例分类。还可以设计专门测试 under-commitment 的诊断集，控制查询歧义度和工具覆盖度。

Q7: 总结一下论文的主要内容

【论文主线】论文的主线可以概括为：工具集成式空间推理的瓶颈不只在感知模型或几何工具，而在“查询编译”这一中间层。现有系统把查询编译成代码、任务约束、工具计划或证据集合，这些产物过于开放，没有在执行前明确回答“这个查询到底需要哪些空间承诺”。作者把这种失败模式称为 under-committed query compilation，并认为它会同时导致两类问题：执行器进行不必要的工具获取与多轮迭代，以及模型用流畅但空洞的推理轨迹掩盖空间承诺的缺失。

【方法主线】PEPS 的解决方案是 parse-before-execute。系统定义一个带类型的 FESM schema，把空间承诺分为 Frame、Entity、State、Metric 四类，并在一个原语库 L 上工作。Parser 不生成自由代码，而是把查询提交为 L 中的紧凑原语集合 P；这个集合既是查询表征需求的显式声明，也是后续工具调用的接口。Executor 只对 P 中未解决的原语做选择性 grounding，通过固定几何 API 和确定性计算产生答案与执行轨迹。Verifier 检查查询、图像、答案和轨迹，判断执行是否充分，返回接受/拒绝、质量分数和结构化反馈；若拒绝，反馈用于定向修复，若接受则提前终止。整个过程是免训练的闭环，最多迭代到上限。

【技术细节】FESM 不是表面空间关系的分类学，而是执行接口：Frame 负责参考系与视角，Entity 负责物体、区域和锚点，State 负责属性与关系，Metric 负责距离、尺寸、角度等可计算量。方法片段还强调 Reference Frame Grounding：所有 anchor points 相对共享场景表示提取，以保证多图或多视角下的计算基座一致。Parser 的输出被限制在 L 内，因此解析结果可检查、可验证；Executor 的选择性 grounding 避免了“把所有工具跑一遍”的浪费；Verifier 的轨迹级检查则把“继续迭代”从默认行为变成有条件行为。

【实验主线】论文在三个互补基准上评估：MMSI-Bench 覆盖多图真实场景、相机位姿、物体布局、区域关系和 multi-step 长链；ViewSpatial-Bench 覆盖 egocentric/allocentric 视角敏感任务；OMNI3D-Bench 覆盖单目细粒度 3D 推理。Baseline 包括专有 LLM、开源 VLM、训练式 3D-VLM 和工具集成 agent 四组。评价不仅看准确率，还看 refinement rounds、API cost 和端到端延迟。论文声称 PEPS 在三个基准上一致提升准确率，并显著降低迭代与工具成本。

【关键发现】给定片段明确给出 ViewSpatial-Bench 上 PEPS overall 52.4%，高于 GCA 的 47.4% 和 MSSR 的 51.8%。相对 GCA 的提升较明显，相对 MSSR 仅约 0.6 个百分点，说明对最强证据型 agent 的准确率优势并非压倒性，效率可能是更重要的差异点。结果片段还显示不同子任务列上的分数有波动，提示收益具有任务异质性。摘要与结论则强调跨三个基准的一致性和效率收益，支持“显式需求承诺能让工具集成空间推理更可靠也更高效”的核心主张。

【论证逻辑】论文的论证链条是：空间推理应按查询选择性组织场景结构；现有工具集成系统虽然外置了几何计算，但查询编译欠承诺；欠承诺导致工具滥用、迭代浪费和看似合理的错误轨迹；用类型化原语集合强制承诺，可以把“需要什么”变成可检查的接口；Parser/Executor/Verifier 的分工让承诺、执行和验证各司其职；实验上同时观察准确率和效率，若两者都改善，则说明问题诊断与系统设计匹配。

【意义与边界】这篇论文的意义在于把 agentic 空间推理的讨论从“工具够不够强”推进到“执行前承诺够不够明确”。它提供了一种可复用的系统范式：受限中间表示 + 选择性 grounding + 轨迹级验证。边界也很明显：方法依赖 schema 设计和 Parser/Verifier 的质量；固定原语库可能覆盖不了开放查询；评测集中在三个 benchmark，真实动态场景和跨域迁移尚未验证；给定片段缺少完整消融与失败案例分析。总体而言，这是一篇系统性强、问题定义清晰、实验覆盖面较广的工作，适合作为工具集成空间推理与 agentic 验证闭环的参考。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：PEPS 展示了“解析—执行—验证”的 agentic 闭环，尤其是用受限中间表示约束工具调用、用验证器决定提前终止或定向修复，这一模式可以迁移到其他工具集成任务。

## 基本信息

- 作者：Xiwei Liu, Yulong Li, Xinling Zhuang, Xuhui Li, Yichen Li, Haolin Yang, Zhixiang Lu, Yutong Xie
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-11-02
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.41389`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要、引言、方法、结果与结论片段，并对 heuristic_draft 中明显的断行、表格残片和错位内容作了剔除与校正。
