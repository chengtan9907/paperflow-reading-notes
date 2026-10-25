---
user_id: "cheng tan"
paper_id: 16018
arxiv_id: "2610.19063v1"
title: "WorldMemBench: A Comprehensive Benchmark for Memory of Video World Models"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.19063v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.19063v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:03:42"
---
# WorldMemBench: A Comprehensive Benchmark for Memory of Video World Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video world model · world memory · benchmark · static memory

## 一句话总结

WorldMemBench 通过 260 个受控“离场-重访”视频生成用例，从静态记忆与动态记忆两条主线、外观与几何两类指标，系统诊断视频世界模型的世界记忆能力，并发现强静态保持能力并不对应可靠的动态状态演化。

## 摘要

> As video models enable longer and more realistic interactive experiences, main-
> taining a world consistent with its history becomes essential. This requires both
> preserving previously observed content and ensuring coherent world evolution
> over time. World memory is a critical capability to evaluate alongside visual
> fidelity and video duration. We introduce WorldMemBench, a benchmark forarXiv:2610.19063v1 systematically evaluating world memory in video world models. Across 260 test
> cases, it assesses static memory through the retention of human identity and ap-
> pearance, object appearance and geometry, and environmental content and geom-
> etry, and dynamic memory through event progression and state persistence. Using
> controlled camera departures and multi-revisit trajectories, the benchmark evalu-
> ates static content against fixed initial references and dynamic events against ex-
> pected state transitions. It combines appearance and geometry metrics with event-
> specific assessments, and reports video quality and camera-following accuracy as
> complementary diagnostics. Evaluation of nine representative models shows that
> strong static retention does not imply reliable dynamic state evolution, while re-
> peated revisits reveal memory degradation beyond the first return. Human studies
> support the alignment between our automatic evaluations and human judgments.
> WorldMemBench provides a systematic testbed for diagnosing memory failures
> and guiding the development of persistent, temporally coherent video worlds.
> ∗Equal contribution. †Corresponding author.
> 1
> Preprint

Q1: 这篇论文试图解决什么问题？

【核心问题】
视频世界模型在长程交互式探索中缺少“世界记忆”的明确定义、评估协议与诊断工具。论文所定义的“世界记忆”指：在观察中断（相机移开导致内容出视野）后，模型既要保持已建立的静态内容（人物身份、物体外观、场景布局），又要让未观察时段内的事件按预期推进（动态演化）。

【为什么现有评估覆盖不了】
1) 视觉质量指标（清晰度、美学、FVD 类指标）只刻画帧级或短窗质量，不要求跨时间窗内容自洽；
2) 时长指标只说明能生成多长，不说明生成的世界是否仍然与自身历史一致；
3) 局部时间连贯性只保证相邻帧过渡平滑，无法覆盖内容完全离开视野再返回后的长期一致性；
4) 交互式探索本身会反复把内容移出和移入视野，因此“保持已见内容不变”与“未观察事件的合理演化”成为交互体验的根本前提，而现有评测对此系统性缺失。

【评估世界记忆的设计难点】
1) 需要区分“应保持不变”与“应当演化”：离开视野期间如果事件确实发生，重访时场景变化反而是正确的，评测必须把静态保持与动态演化分开定义，否则会把正确变化误判为记忆错误；
2) 需要可复现的“观察中断”：不能依赖自然长视频中偶然的内容出入，而要主动构造离开-重访轨迹，使“离开前的内容”与“返回后的内容”可对照；
3) 需要参考标准：静态记忆以固定初始观察为参考，动态记忆则必须以“期望状态转换”为参考，这要求对事件进展给出先验预期；
4) 多因素混杂：记忆分数可能被生成画面质量、相机轨迹执行误差污染，因此需要把视频质量和相机跟随作为补充诊断维度分离解释。

【需要回答的开放问题】
1) 当代视频世界模型在哪种记忆上失败、失败集中在静态还是动态、外观还是几何？
2) 记忆退化是单次性还是一回访就衰减的累积过程？是否随探索方向改变而恶化？
3) “静态保持强”是否蕴含“动态演化可靠”？两项能力是否解耦？
4) 自动评估是否能与人类对世界一致性的主观判断对齐？

【本文的收敛方式】
把上述问题落实为一个包含 260 个用例、四维指标、多回访与跨轴对照的系统基准，在 9 个代表性模型上给出可量化的诊断答案。

Q2: 有哪些相关研究？

【基础能力线：长视频生成】
论文引用 Huang et al. (2025b)、Zhu et al. (2026b)、Zhao et al. (2026) 等自回归视频生成工作，说明生成长而连贯视频序列的能力已显著提升。这类工作是“世界记忆”问题出现的物质前提：只有生成长视频能力足够强，跨长距离观察中断的一致性才会成为瓶颈。细节以原文引用为准。

【基础能力线：相机控制】
论文引用 Bai et al. (2025) 的 Recammaster（camera-controlled generative rendering from a single video）等相机可控生成方法（该信息来自检索命中的参考文献条目）。相机路径控制是构造离开-重访轨迹的前提，也是本基准把“相机跟随”列为独立诊断维度的原因。

【基础能力线：视频世界模型】
论文引用 Mao et al. (2025)、Gao et al. (2026)、Yin et al. (2026)、Sun et al. (2025) 等，这些工作把视频生成推进为可持续、可交互探索的生成环境。世界模型是本文的主要评测对象。

【已有记忆相关基准】
1) 重访一致性（revisit consistency）：Zhang et al. (2026)、Gu et al. (2026)、Xu et al. (2026a) 评估内容重访时是否保持一致；
2) 观察中断下的状态演化：Chen et al. (2026)、Ma et al. (2026)、Lu et al. (2026)、Duan et al. (2026) 评估在观察被中断期间世界状态如何演化；
3) LoopBench（Wu et al. 2026）评估跨多回访轨迹的场景回忆。
与本文的关系：已有工作分散在静态重访一致性或动态状态演化的单一环节，而 WorldMemBench 用统一协议同时覆盖静态与动态记忆，并引入多回访（Nmulti）与跨轴（Ncross）配对结构来刻画记忆随回访次数与方向的变化。

【方法论线：分层诊断评估】
论文引用 Yang et al. (2026) 的分层诊断评估思想，作为其四维评估（视频质量、静态记忆、动态记忆、相机跟随）的组织方法论，目的是用可独立观测的分层指标替代笼统的整体分数。

【综述性判断（合理推断）】
从引用语境看，本工作站在两条研究流的交叉点上：一是“评估生成世界的一致性”，二是“把视频生成模型当作可交互世界模型”。其相对增量在于把静态保持与动态演化放进同一套协议并给出多回访退化与方向敏感性的量化手段。

Q3: 论文如何解决这个问题？

【总体协议：受控离场-重访 rollout】
论文构造翻译（translation）与旋转（rotation）两类相机轨迹，使相机先离开先前观察过的区域（记为第 i 次离开 Di），再按指定路径返回该区域（记为对应重访 Ri）。初始观察阶段记为 V。每个测试用例共享这样的时间锚点（V、Di、Ri），从而把“世界记忆”具体化为可复现的受控实验：离开前内容已知，返回后与参考对照即可度量记忆。

【四维评估框架】
受分层诊断评估启发（Yang et al. 2026），评估被组织为四个互补维度：
1) General Video Quality（一般视频质量）：生成画面的基础质量；
2) Static Memory（静态记忆）：离开前后既有内容是否保持；
3) Dynamic Memory（动态记忆）：离开期间事件是否按预期推进；
4) Camera Following（相机跟随）：生成视频是否服从指定相机运动。
这样设计的目的在于把“记忆本身的问题”与“生成质量低、相机控制不准”分开，避免分数混杂。

【静态记忆的分解与度量】
静态记忆覆盖三类内容：人类身份与外观（human identity and appearance）、物体外观与几何（object appearance and geometry）、环境内容与几何（environment content and geometry）。度量方式是对照固定初始参考（initial reference），以重访帧与初始观察的一致性为依据，同时兼顾外观（appearance）与几何（geometry）两类信号。

【动态记忆的分解与度量】
动态记忆覆盖事件进展（event progression）与状态持续性（state persistence）。度量方式是对照期望状态转换（expected state transitions），即离开期间若预期发生事件，则回到视野后状态应相应演化；这与静态记忆“保持原样”的标准形成互补，避免把合理变化判为错误。

【数据与配对结构】
基准由 260 个核心测试用例组成。用例按模型–场景单元组织，构建多回访配对（Nmulti）与跨轴配对（Ncross）：Nmulti 用于度量记忆随着多次回访的累积变化，Ncross 用于度量改变探索方向（跨轴）对记忆的影响。配对结构使比较能在相同场景与模型条件下进行（合理推断）。

【指标体系】
1) 静态内容用外观+几何指标；
2) 动态事件用事件特定评估（event-specific assessments）；
3) 视频质量与相机跟随以“互补诊断”的形式报告，用于解读记忆分数的可信度。

【人类研究】
用人类判断对照自动评估，验证自动指标是否与主观的世界一致性感知对齐。

【上下文访问策略对照实验】
在模型与生成设置完全固定的条件下，控制“时间上下文策略”（temporal context policy），观察不同上下文使用方式如何同时影响记忆性能与推理效率。该实验把“记忆能力”从模型参数中部分剥离，揭示推理时如何使用历史上下文是一个独立且可控的影响因素。

Q4: 论文做了哪些实验？

【基准规模与覆盖】
WorldMemBench 包含 260 个核心测试用例，覆盖静态记忆三小类（人类身份与外观、物体外观与几何、环境内容与几何）和动态记忆两小类（事件进展、状态持续性），并为多回访比较（Nmulti）与跨轴比较（Ncross）构建了配对用例。

【模型评估主实验】
对 9 个代表性视频世界模型，按统一协议进行四维评估：General Video Quality、Static Memory、Dynamic Memory、Camera Following。论文中的 Tab. 2 展示各模型在四维上的成绩分布，揭示不同模型的优势维度。检索证据的表格残片显示每模型行记录了 Tracking support (%)、Environment pose (%)、Q30 paired cases 等指标列，以及形如 “Vis | Dep | Rec” 的三值组合（例如 Evoke 的 Vis .869 | Dep .920 | Rec .696、DreamX World 的 Vis .871 | Dep .294 | Rec .171），说明评测中包含视觉相关、依赖/深度相关与召回相关等多类测量。这些缩写的确切定义以原文为准。

【多回访与跨轴实验】
在单次回访之外，利用 Nmulti 配对评测多次返回造成的累积记忆退化，利用 Ncross 配对评测改变探索方向（跨轴）带来的额外敏感性。这是对“单次重访一致性”类基准的重要扩展。

【人类研究】
将自动评估结果与人类对世界一致性的主观判断进行对照，检验自动指标的有效性。摘要与结论明确指出两者对齐。

【上下文策略对照实验】
在模型和生成设置固定不变的前提下，仅改变时间上下文策略（即模型推理时回顾历史帧/上下文的方式），测量记忆性能与推理效率的变化。该实验隔离出“上下文使用策略”这一变量，说明记忆表现不仅取决于模型参数，还取决于推理时如何访问时间上下文。

【证据缺口提示】
检索证据中未能取得完整实验配置（9 个模型的具体清单、全部指标定义、采样策略、人类实验规模等），上述实验描述的细节部分对应摘要与结论的概括，精确配置需查原文 Methods/Experiments 部分。

Q5: 发现了什么实验现象？

【现象一：静态保持强并不蕴含动态演化可靠】
这是论文最核心的观察：模型的静态记忆（人类身份、物体外观与几何、环境内容与几何重访一致）得分高，不代表其动态记忆（画面外事件推进、状态持续性）可靠。二者在能力上呈现解耦。机制层面的合理解释是：静态记忆更像“检索-重建”任务，模型可以直接对照已编码的参考画面；而动态记忆要求模型在看不见的时间段内进行隐式的物理/因果推演，两种任务的失败模式天然不同。

【现象二：没有模型在全部维度上领先】
9 个模型各有强项：例如检索证据显示 HY-WorldPlay 在所有 7 项静态记忆指标上领先（合理推断来自表格残片），但论文明确总结“没有一个模型在所有维度上持续领先”。这提示当前模型的能力轮廓是碎片化的：某个模型可能静态保持极好但动态推演弱，另一个模型则相反。

【现象三：多次回访的累积退化】
多回访评估显示记忆退化在首次返回之后仍在继续，说明记忆衰减不是一次性“是否记住”的二值事件，而是随着回访次数累积的持续性衰退。该现象对交互式应用的含义是：单次一致性测试会系统性高估模型在真实反复探索中的长期表现。

【现象四：跨轴回访的额外敏感性】
当从不同方向（跨轴）回到同一区域时，模型表现出额外的记忆敏感度，即记忆一致性还依赖探索路径/方向。推测：模型的内部场景表征可能与视角/轨迹方向耦合，从不同方向召回时激活的表征不一致。

【现象五：自动评估与人类判断一致】
人类研究支持自动评估分数与人类对世界一致性的主观判断对齐，说明外观+几何+事件特定指标的分数组合方向正确。

【现象六：上下文策略是记忆表现的独立变量】
固定模型与生成设置时，改变时间上下文策略会同时改变记忆性能与推理效率，说明存在“记忆表现-计算开销”的权衡；更强的记忆不一定来自更大模型，也可能来自更聪明的上下文利用方式。

【指标张力提示（分析，非论文原话）】
把 General Video Quality 和 Camera Following 作为补充诊断报告，本身就承认记忆分数可能被生成质量劣化和相机轨迹执行偏差污染：如果画面变糊或相机跑偏，重访一致性下降可能是质量问题而非记忆问题。阅读结果时应结合这两个维度解读记忆分数。

Q6: 有什么可以进一步探索的点？

【1. 轨迹与场景结构的扩展】
当前以翻译与旋转的离开-重访轨迹为主；未来可以加入更自然的探索模式（遮挡、变速、多次进出、自由视角）和更复杂的 3D 场景，使基准更接近真实交互使用。

【2. 更长时距与终身记忆】
Nmulti 已显示首次返回之后仍有累积退化，未来可进一步加大回访次数与隐藏时长，刻画记忆的遗忘曲线与饱和行为，把“几轮交互内的记忆”推进到“长时间跨会话的一致性”。

【3. 动态因果一致性】
当前动态记忆以“期望状态转换”为参考；更进一步可以要求事件推演满足因果链可解释性，支持反事实检验（如果画面外发生 A，返回时场景应呈现 B）。

【4. 静态与动态记忆解耦建模】
两项能力被证明解耦后，可以尝试显式模块化设计：一个“场景存档”模块负责静态保持（检索-重建），一个“世界推演”模块负责动态演化（模拟-预测），再设计接口让二者协同。

【5. 上下文策略的学习与优化】
上下文访问策略被证明显著影响记忆-效率权衡；未来方向是把上下文策略本身变成可学习的组件，例如用记忆压缩、分层检索、重要性加权来自适应决定“回顾哪些历史帧”，降低推理开销同时保住记忆。

【6. 与 agent/具身应用结合】
世界记忆是交互式 agent 在生成环境中长期作业的前提（地图构建、状态追踪、事件日志）。可把基准扩展为 agent 评估环境：不仅看模型记忆，还看 agent 能否利用记忆完成任务。

【7. 从评测到训练信号】
把记忆损失从诊断工具转化为训练目标（损失项或奖励信号），直接优化模型的长时间自洽性。

【8. 多模态与多智能体扩展】
在纯视觉基础上加入文本、音频、3D 表示；或评估多个智能体共享/更新同一世界记忆时的冲突与协同。

【9. 标准化与规模化】
扩大模型族群（更多厂商、更多架构、更多采样设置），制定标准化的报告格式，使世界记忆成为视频生成模型的标准评测维度（类似 FVD/CLIP 分数但针对长期一致性）。

Q7: 总结一下论文的主要内容

【论证主线：为什么需要世界记忆评测】
论文以视频生成的两条进展为起点：自回归视频生成已能产出更长更连贯的序列，相机控制技术则让视角与轨迹可控。二者结合催生了可持续交互探索的视频世界模型。在这种使用方式下，用户会反复把内容移出和移入视野，生成世界必须能与自身历史保持一致——既要保持已观察到的静态内容（人物、物体、环境），也要让画面外的事件按预期推进。论文明确指出：视频质量和局部时间连贯性都不足以保证这种能力，因此需要一个新的被评测维度——“世界记忆”。

【定义与分解】
世界记忆被定义为“在观察中断期间保持已确立内容并维持连贯状态演化的能力”，并进一步分解为：静态记忆（人类身份与外观、物体外观与几何、环境内容与几何）和动态记忆（事件进展、状态持续性）。这个分解的关键洞见在于：离开视野期间“正确的事情”不是一成不变，而是静态属性保持、动态属性演化，因此两类记忆必须分开测、分开报。

【技术主线：受控离场-重访协议】
为实现可对照的记忆评测，论文构造受控相机轨迹：初始观察 V → 第 i 次离开 Di → 指定重访 Ri，包含平移与旋转两类运动。静态记忆以固定初始参考为锚，动态记忆以期望状态转换为锚。整体评估组织为四个互补维度（一般视频质量、静态记忆、动态记忆、相机跟随），结合外观与几何指标、事件特定评估，并以视频质量与相机跟随为补充诊断来排除混杂因素。基准包含 260 个核心用例，并设计多回访配对（Nmulti）与跨轴配对（Ncross），把评估从“单次回访一致性”扩展到“多次回访累积行为”和“方向敏感性”。

【实验主线】
对 9 个代表性视频世界模型进行统一评估。主要发现：
1) 没有任何模型在全部维度上领先，不同模型优势各异（Tab. 2；如 HY-WorldPlay 在 7 项静态记忆指标上领先，来自表格残片的合理解读）；
2) 强静态保持不等于可靠的动态状态演化，两类记忆能力解耦；
3) 多次回访揭示首次返回之后的累积记忆退化；
4) 跨轴回访暴露对探索方向变化的额外敏感性；
5) 人类研究验证自动评估与人类判断对齐。
此外，在模型与生成设置固定条件下进行的受控上下文访问实验表明，时间上下文策略同时影响记忆性能与推理效率，即记忆不仅由模型参数决定，也由推理时的历史访问方式决定。

【定位与意义】
论文把 WorldMemBench 定位为“评估基准+诊断工具”：既给出现有模型的量化排名，也通过维度分解、多回访/跨轴协议和上下文实验为改进指明方向。该工作补上了视频世界模型评测中长期被忽视的“记忆”维度，为后续把记忆作为训练目标、把上下文策略作为可优化组件的研究提供了测试床。

【证据说明】
本总结基于论文摘要、Introduction 与 Conclusion 的检索证据重建；具体实验配置、全部表格数值与模型清单需要结合原文确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的生成方向（权重 0.10）直接相关：论文把“世界记忆”确立为视频生成质量与时长之外的第三个关键评估维度，对从事视频生成/世界模型研究的用户有直接参考价值。

## 基本信息

- 作者：Nan Zhou, Jianxiong Gao, Qiuyu Wang, Wen Wang, Jiahao Wang, Yufeng Yuan, Shuaiting Li, Hao Ouyang, Yujun Shen, Weiming Lu, Jun Xiao, Yueting Zhuang, Yongliang Shen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.19063v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（76 个 chunk，主要命中 Introduction、References 与 Conclusion）；由于命中多为片段残片，部分实验数值、指标定义与模型清单依赖原文进一步确认。
