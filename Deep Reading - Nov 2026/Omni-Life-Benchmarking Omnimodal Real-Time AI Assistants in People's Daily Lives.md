---
user_id: "cheng tan"
paper_id: 20337
arxiv_id: "2611.02835v1"
title: "Omni-Life: Benchmarking Omnimodal Real-Time AI Assistants in People's Daily Lives"
publish_date: "2026-11-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.02835v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.02835v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-08T02:11:00"
---
# Omni-Life: Benchmarking Omnimodal Real-Time AI Assistants in People's Daily Lives

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：omnimodal assistant · real-time interaction · benchmark · proactive intervention

## 一句话总结

Omni-Life 是一个面向日常生活的实时全模态助手基准：它用 400 段视频（277 段合成 + 123 段真实）、555 个必需干预节点和 110 段全程静默视频，联合检验模型“该不该出手、出手时机是否合适、内容是否正确、不该出手时能否保持沉默”；在 12 种主流配置中，完全正确处理的可求助视频比例最高仅 4.48%。

## 摘要

> Real-time omnimodal assistants are expected to support people in everyday life, but audiovisual understanding alone does not establish whether they can provide timely and appropriate help. We introduce Omni-Life, a benchmark grounded in real-world user needs, deployment scenarios, and device-dependent observations. It combines controlled video synthesis for hazardous situations and edge cases with annotated real-world videos under a shared temporal protocol. The benchmark contains 400 videos: 290 requiring assistance, with 555 required intervention events, and 110 requiring silence throughout to test whether models can withhold unnecessary responses when no assistance is needed. We evaluate twelve configurations featuring mainstream real-time omnimodal models, jointly assessing event coverage, response content, timing, and unwanted output. At most 4.48% of videos requiring assistance are handled completely correctly. To examine practical deployment strategies, we study task reminders and response-latency tolerance. Reminders can improve recall by 17.12 percentage points but add approximately 4.6 unwanted responses per minute of reference silence. Increasing latency tolerance by 10s improves recall by at most 5.95 percentage points. These results show that existing models remain insufficient to deliver reliable assistance in real-life applications, calling for tailored model optimization in instruction following, timely proactive intervention, and withholding responses when assistance is not needed.

Q1: 这篇论文试图解决什么问题？

1) 交互范式迁移带来的新问题。全模态全双工基础模型把 Human-AI 交互从“轮次式问答”推向“带视听感知的连续实时辅助”。这改变了助手的任务定义：不再是在收到完整输入后生成一个答案，而是要在环境持续变化的过程中同时决定“是否响应、何时响应、响应什么”。

2) 仅靠视听理解不足以证明“有用”。论文明确把“能看懂画面、听懂声音”与“能否在正确时刻给出恰当帮助”区分开来——前者是原子能力，后者是应用级能力，两者之间存在评测空档。

3) 现实部署已经在发生，评测没有跟上。Doubao 的 SeedRealtime、GPT-Live 等主流产品演示已经把这类模型指向跨设备、跨环境、跨人群的日常使用；但当前研究仍主要投入在模型开发与单项交互能力上（Lu et al., 2026c 一系工作关注实时助手的行为决策）。论文判断：面向真实应用的评估既及时又必要。

4) 实时助手有新的失败形态，且后果不对等。论文列举的失败包括：在不需要帮助时插话、漏掉关键事件、给出错误信息；后果从轻微不满一直延伸到有安全风险的错误。这意味着评测必须同时衡量“该出手时出手”和“不该出手时保持沉默”，两者代价不对称。

5) 日常支持的概念框架。论文把日常支持分成四类：感知（获取视觉、听觉、颜色等难以直接获得的环境信息）、认知（对状态变化的意识与情境判断）等；检索片段在此截断，另外两类需回原文 Introduction 确认（合理推断可能是决策/行动与交流协作，但未获证据支持，不能写实）。

6) 既缺“真实生活场景 + 危险/边缘案例 + 时间协议”的联合评测。论文的切入点是：真实用户需求、部署场景、设备相关观察三者共同定义 benchmark；把受控合成（危险情境与 edge cases）与标注真实视频放在同一时间协议下；并用 110 段“应保持沉默”的视频显式考核不必要的输出。这是对既有“只测单点能力、只看最终答案”评测范式的一个直接补位。

7) 为什么这个问题在技术上难。实时性把“感知—判断—决策—表达”压进同一条时间线：模型要在流式输入尚未结束时判断事件是否已经构成需要干预的节点；要在响应窗口内完成生成；还要在整段视频尺度上维持任务状态（如提醒类任务）。这使“能力”不再可加：单项能力强的模型可能在触发决策上整体失分。论文选择用“事件覆盖、响应内容、时机、多余输出”四个维度联合打分，正是为了不让任何单一维度掩盖其余维度的失败。

8) 与用户兴趣的关联判断。该问题本质上是一个“主动 agent 的决策问题”：何时感知、何时行动、何时保持沉默、如何在长时间尺度上维持任务，这与 agent 方向的关注点高度重合；其受控视频合成与生成方向有交集；而“真实场景 + 时间协议 + 静默考核”的评测设计也可迁移到科学工作流中的实时监测与异常告警评估。

Q2: 有哪些相关研究？

1) 实时全模态/全双工基础模型。论文把工作放在 omnimodal full-duplex foundation models 的脉络里：SeedRealtime（Doubao）与 GPT-Live 是产品侧的代表性演示；Qwen Team (2026b) 的 Qwen3.8-omni-flash-realtime 被作为原生音频、实时交互的模型引用。这一脉的共同预设是：模型要持续接收音频与视觉流，并在连续时间上做出反应，而不是等待一轮完整输入。

2) 实时助手的行为决策。Lu et al. (2026c) 被用来支撑“实时助手必须决定是否响应、何时响应、响应什么”这一命题。论文与这类工作的差异在于：它不提出新的交互策略，而是把这类策略放进一个带时间标注与静默要求的评测协议中检验。

3) 被评测模型的版图。主实验覆盖 12 种配置：开源权重的 MiniCPM-o 4.5、MOSS-VL、MOSS Preview、Venus-Omni、Gander、AURA、JoyAI、DuplexOmni (S1)；API 侧的 Gemini 3.8 Live、Qwen3.5 Plus、Qwen3.5 Flash、Qwen3.8 Flash。这个名单本身反映了 2026 年前后实时全模态模型的主要玩家与形态（原生音频、实时 API、开源权重），也说明 benchmark 试图覆盖“可本地部署”和“只能调 API”两类使用方式。

4) 基准资源谱系。论文在 Introduction 里对既有 benchmark resources 做了 broad categorization（片段在此截断），并把 Omni-Life 定位为从原子能力评估走向真实应用评估的一步。但被明确点名对比的既有基准名单未出现在检索片段中，需回原文 Related Work 与 Introduction 的 benchmark 分类表核对，不能在此臆造。

5) 相邻研究方向的合理推断（非论文原文）。与 Omni-Life 最接近的邻近方向包括：流式/长视频理解与事件定位评测、主动式/打断式对话助手评测、语音+视觉多模态指令遵循评测、以及面向部署的延迟与吞吐评测。这些方向在片段中没有被逐一点名，只能作为检索线索，不能当作论文的相关工作陈述。论文与它们的可能差异在于：把“是否触发”“时机窗口”“内容正确”“静默保持”放进同一套指标，并用合成数据补齐危险与边缘案例。

6) 与生成式数据构造的关系。论文用受控视频合成来构造危险情境与 edge cases，这使它同时与视频生成、合成数据质量评估相关；其合成片段额外要求物理合理性与参与者、物体一致性（附录 A 有更完整的构建与质量控制标准）。这构成与纯真实数据 benchmark 的一个方法论差异。

Q3: 论文如何解决这个问题？

1) 总体设计。Omni-Life 是一个 grounded in real-world user needs、deployment scenarios、device-dependent observations 的实时全模态助手基准；核心是把“用户真实需要什么帮助”与“模型在什么时刻、给出什么输出”对齐到统一的时间协议上。

2) 数据构成。评测集 400 段视频 = 277 段合成 + 123 段真实。其中 290 段至少需要一个干预，合计 555 个必需干预事件；另有 110 段要求全程沉默（reference silence），用来测试模型在无需帮助时是否保持不输出。

3) 合成与真实混合。合成视频用于覆盖危险情境与 edge cases，受控生成保证事件可控、可重复；合成片段额外要求物理合理性、参与者一致性与物体一致性。真实视频提供自然分布下的日常场景。两者共享同一套时间协议，避免两套标注口径不可比。构建与质量控制标准见附录 A。

4) 场景与人群覆盖。论文把日常支持分为四类（感知、认知、……，片段截断后两类），覆盖五个主要场景；附录 D 提供 14 个案例画廊，横跨不同用户群体、五个主要场景与五类失败。

5) 时间标注与 QA 构建（§3.4）。每段视频标注响应窗口（response windows）、响应时间线（response timelines）与相关输出描述（descriptions of relevant outputs）；这使“是否在窗口内响应”“响应内容是否对应事件”可以被自动判定。

6) 指标体系。主实验联合评估事件覆盖（coverage/recall）、响应内容、时机与 unwanted output；Content Accuracy 与 Timing Accuracy 各以 400 为分母；Joint Accuracy 要求一段视频的两个答案都正确才计入；Full Correct 是最严格的整段全对指标。主 F1 使用 main-score matches，提前输出（early outputs）按未匹配响应计入。

7) 时机/内容分离诊断（表 5）。在保留主分匹配的前提下，把剩余节点与响应一一配对，优先 on-time incorrect、late correct、late incorrect；late 定义为 onset 在窗口终点之后且不设迟到上限；另有 no later reply 与 others（保留失败会话、已分配响应、缺失证据）。该诊断不计入主分，也不判定迟到回复对应哪个事件。这个设计的意义是把“说对了但说晚了”和“说错了”分开。

8) 消融与部署策略。任务提醒（reminder / task delivery 的变化）与响应延迟容忍（放宽响应窗口 10 秒）被作为两个可部署旋钮来研究。

9) 评估对象。12 种配置：8 个开源权重 + 4 个 API 模型（名单见 related_work）。所有模型在同一时间协议与同一指标下比较。

10) 设计上的关键取舍。a) 用合成数据补危险场景：好处是可控、可覆盖长尾，风险是合成片段可能偏向显式事件（论文在 Limitations 中自认）；b) 用预定义响应窗口判时机：好处是可自动判定，代价是对标注质量敏感，且窗口选择会直接影响模型排名；c) 用“全程沉默”视频考核 unwanted output：好处是把误触发显式化，代价是静默时长的定义会影响误报率的分母（论文用“每分钟参考静默”来归一化）；d) 表 5 的分离诊断：好处是暴露“内容对但时机错”，代价是不进入主分且无法归因迟到回复的意图。这些取舍共同决定了这个 benchmark 能测什么、不能测什么——阅读时应把指标定义当作结论的一部分。

Q4: 论文做了哪些实验？

1) 主实验设置。12 种实时全模态模型配置 × 400 段视频；在统一时间协议下评测事件覆盖、响应内容、时机与 unwanted output，并报告 Full Correct、Joint Accuracy、Content Accuracy、Timing Accuracy 等指标。Invalid answers 按错误计（片段中出现“nvalid answers counted as incorrect”的残片，合理推断为 invalid answers counted as incorrect）。

2) 主结果。最强的 Full Correct 也只有 4.48%；按节点级统计，JoyAI 在三类干预召回上领先（preventive 30.88%、corrective 26.75%、routine 14.55%）。片段中可见的按模型表格显示各配置在若干列上为个位数甚至 0（例如 MOSS-VL 5 列为 6.3/2.9/2.5/4.9/4.5，MOSS Preview、AURA、JoyAI 在该表中全 0），与 JoyAI 节点级召回领先的描述形成口径差异，需回原文表格标题与脚注核对是不同设置（如 reminder on/off、不同任务交付方式）还是不同指标。

3) 消融一：任务提醒。提醒把召回提升 17.12 个百分点，但在参考静默时长上每分钟增加约 4.6 次 unwanted responses。这说明提醒是一个“用误报换召回”的旋钮，而不是纯粹增益。

4) 消融二：延迟容忍。把响应窗口放宽 10 秒，召回最多提升 5.95 个百分点；结论处另称在该轨迹内最多恢复 6.80% 的原本未匹配节点。两个数字口径不同（百分点 vs 未匹配节点恢复比例），需回原文核对分母。

5) 时机/内容分离诊断（表 5）。把响应分为 on-time correct、on-time incorrect、late correct、late incorrect、no later reply 等类别；电梯案例中 MOSS-VL 在 24.18 s 报告门已打开，而要求窗口是 [18.75, 21.75) s，属于内容对但迟到。

6) 失败分析。intervention control（是否该干预的控制）是最普遍的失败类别，说明主要瓶颈在“触发决策”而非“描述能力”。

7) 对比与反例。片段提到任务模态/任务到达方式改变后，Venus 没有获得召回提升，而另一个模型获得提升——更多响应不一定带来整段视频层面的更好表现。

8) 评测的生态效度说明。伦理声明明确实验评估的是录制视频流，而非对真实情境的干预。

Q5: 发现了什么实验现象？

1) 绝对水平极低，任务远未解决。最强 Full Correct 4.48%，多个配置在若干列上为 0；这不是“接近可用”的水平，而是说明现有实时全模态模型在“该不该说、何时说、说什么、何时别说”的联合要求下系统性失效。

2) 召回与静默之间的结构性张力。提醒能把召回拉高 17.12 个百分点，代价是参考静默期间每分钟约多 4.6 次误响应。也就是说，同一个干预同时改善“不漏”与恶化“不误”，两个目标的优化方向在现有模型上是冲突的；这提示单纯加提示词或加提醒不是部署解。

3) 延迟容忍的边际收益有限。+10 秒窗口最多 +5.95 个百分点召回（或恢复 6.80% 未匹配节点），说明相当一部分失败不是“慢半拍”，而是模型根本没有在该事件上产生可用响应。合理推断：延迟优化只能救回“已经检测到但没来得及说”的那部分失败，救不回“没检测到”或“检测到但判断不该说”的失败。

4) 更多输出不等于更好。Venus 在任务模态改变后没有召回增益；片段明确指出更多响应未必在整段视频上带来更好表现。这是一个重要的反直觉观察：在主动助手场景里，输出数量与帮助质量不是单调关系。

5) 内容正确与时机正确可以解耦。电梯案例中模型说对了“门已开”，但时间点在窗口之外；单一准确率指标会把这类失败与“说错”混为一谈，表 5 的分离诊断正是为了暴露这一点。

6) 高节点召回与低视频级可靠性可以共存。JoyAI 在节点级召回领先，但片段同时指出这一覆盖与低视频级可靠性并存（Table 4）。这意味着“平均每个事件更容易被覆盖”不等于“整段视频可以放心交给它”。

7) 失败集中在干预控制而非内容。片段明确说 intervention control 是最普遍的失败类别；这与“模型擅长描述、不擅长决定何时行动”的直觉一致（合理推断），但片段只提供了一句结论，具体分布需回原文失败分析表核对。

8) 基准协议本身会改变结论。严格窗口下迟到回复不得分，放宽窗口只能挽回一部分；这说明“时机”不是模型的固有属性，而是模型与评测窗口共同定义的量。阅读时要把窗口定义、迟到上限、静默时长归一化方式当作结论的一部分。

9) 表格内部的口径张力。按模型表格中 JoyAI 全 0，与另一处 JoyAI 召回领先的描述并存；这可能是不同设置或不同指标口径，需回原文核对，不能直接当作矛盾结论使用。

Q6: 有什么可以进一步探索的点？

1) 指令遵循与选择性响应触发。让模型学会“何时不该说话”，可能需要不确定性感知的触发门控、显式的“保持沉默”训练目标，以及把 unwanted output 作为一等公民纳入训练/对齐。

2) 及时主动干预。现在的失败很多是“检测到但没在窗口内说”；可探索提前预测事件窗口、预生成候选响应、以及在流式输入上做增量决策的架构。

3) 持续任务执行。reminder 消融说明跨时间的任务状态维护是瓶颈之一；可研究持久记忆、任务跟踪、提醒调度与中断恢复——这与 agent 方向直接相关。

4) 提醒机制的设计空间。提醒提召回但增误报，值得研究“何时提醒、提醒几次、提醒的置信度阈值”，以及提醒与静默场景的联合优化。

5) 延迟与计算调度的联合优化。把窗口放宽只是评测侧的放松，真正需要模型端降低端到端延迟（流式解码、增量视觉编码、提前退出等），并研究延迟-质量-误报的三维权衡。

6) 更贴近部署的评测。论文 Limitations 明确指出有限录制、预定义任务与固定窗口限制了对长时间部署、自适应交互与偏好变化的泛化；后续可做长时程、跨设备（device-dependent observations）、跨人群的评测。

7) 数据扩展与偏置控制。合成片段可能偏向显式事件；可研究更真实的录制、更少偏置的合成、以及危险/边缘案例的可控生成与质量校验（物理合理性、参与者/物体一致性）。

8) 指标改进。表 5 已把时机与内容分离，但仍无法归因迟到回复的意图；可研究多事件交叉、部分正确、迟到回复归因、以及风险敏感（误报/漏报代价不对称）的指标。

9) 跨模态不对称性。片段提到任务模态变化会改变“任务如何、何时到达模型”；可系统研究音频 vs 视觉信息在触发决策中的不同作用。

10) 安全与伦理。实时干预的误报与漏报代价不对称；可研究在危险场景下的保守触发策略、拒答/求证机制，以及录制评测与真实干预之间的 gap（论文伦理声明已点明这一点）。

11) 与生成模型的结合。用生成模型合成更多长尾场景时，如何保证时间标注与物理一致性，是一个可复用的方法论问题。

Q7: 总结一下论文的主要内容

一、问题与定位。Omni-Life 关注的是实时全模态助手在日常生活里能否“在正确的时间做正确的事”。论文的出发点是：全模态全双工模型正在把交互从轮次式问答推向带视听感知的连续辅助，助手的任务从“回答一个问题”变成“持续决定是否响应、何时响应、响应什么”。仅凭视听理解能力不能证明这种帮助是有用且及时的；而现有评测多停留在原子能力层面，缺少面向真实应用、覆盖危险与边缘案例、并把“保持沉默”也当作考核项的基准。

二、基准构建。Omni-Life 的评测集包含 400 段视频：277 段合成 + 123 段真实。290 段至少需要一个干预，合计 555 个必需干预事件；110 段要求全程沉默，用来测试模型在不需要帮助时是否乱输出。合成视频用于危险情境与 edge cases，额外要求物理合理性与参与者、物体一致性；真实视频提供自然场景；两者共享同一时间协议。论文把日常支持分为四类（感知、认知、……），覆盖五个主要场景，并在附录 D 给出跨用户群体、跨场景、跨失败类别的 14 个案例。时间标注与 QA 构建（§3.4）为每段视频标注响应窗口、响应时间线与相关输出描述，使“是否在窗口内响应”与“响应内容是否对应事件”可被自动判定。

三、评估协议。主实验联合评估事件覆盖、响应内容、时机与 unwanted output；Content Accuracy 与 Timing Accuracy 各以 400 为分母；Joint Accuracy 要求一段视频的两个答案都正确；Full Correct 是最严格的整段全对指标。主 F1 使用 main-score matches，提前输出按未匹配响应计。表 5 提供时机/内容分离诊断：把响应分为 on-time correct、on-time incorrect、late correct、late incorrect、no later reply、others，late 定义为 onset 在窗口终点之后且不设上限；该诊断不计入主分，也不判定迟到回复对应哪个事件。消融研究任务提醒与响应延迟容忍两个部署旋钮。

四、主实验与发现。12 种配置（8 个开源权重 + 4 个 API 模型）在 400 段视频上评测。最强的 Full Correct 仅 4.48%；节点级召回上 JoyAI 领先（preventive 30.88%、corrective 26.75%、routine 14.55%），但片段同时指出高节点召回与低视频级可靠性并存。片段中可见的按模型表格多为个位数甚至 0（如 MOSS-VL 6.3/2.9/2.5/4.9/4.5，MOSS Preview、AURA、JoyAI 在该表中全 0），与 JoyAI 召回领先的描述存在口径差异，需回原文核对。失败分析指出 intervention control 是最普遍的失败类别。

五、消融与诊断。提醒把召回提升 17.12 个百分点，但在参考静默时长上每分钟增加约 4.6 次 unwanted responses；延迟容忍 +10 秒最多提升 5.95 个百分点召回（结论另称最多恢复 6.80% 的原本未匹配节点，口径需核对）。电梯案例（MOSS-VL 在 24.18 s 报告门已打开，窗口 [18.75, 21.75) s）说明内容对但时机错是真实存在的失败形态；Venus 在任务模态改变后没有召回增益，说明更多输出未必更好。

六、结论与边界。论文结论是现有模型还不足以在真实应用中提供可靠帮助，需要在指令遵循、及时主动干预、以及不需要帮助时抑制输出三方面做针对性优化。Limitations 自认：有限录制、预定义任务与固定响应窗口限制了对长时间部署、自适应交互与偏好变化的泛化；合成片段可能偏向显式事件；案例组合可能不具代表性；实验评估的是录制视频流而非真实干预。阅读时应注意：指标定义（窗口、迟到上限、静默归一化）会显著影响结论；表格口径需回原文核对；“4.48%”是一个严格的整段全对指标，不应被误读为“模型只有 4.48% 的原子能力”。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向高度相关：核心是主动 agent 的“是否行动、何时行动、何时不动”决策，以及跨时间的任务执行（reminder 消融直接对应 agent 的记忆与规划）。

## 基本信息

- 作者：Mengze Hong, Ziwei Shang, Di Liang, Zijian Li, Liang Zhu, Hongyu Shan, Robin Zhou, Minlong Peng, Zhiqiang Kou, Jia Wang, Di Jiang, Chen Jason Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-11-02
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.02835v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 91 个 PDF 语义检索命中片段（覆盖 Introduction、Method、References、Conclusion），并按 field_evidence_map 将证据重新归位；heuristic_draft 中若干字段存在明显的证据错位与断片，已用检索片段与论文元数据纠正，无法确认之处已在对应字段内标注。
