---
user_id: "cheng tan"
paper_id: 19419
arxiv_id: "2610.40578v1"
title: "VeriCUA: Verification as Agentic Exploration for Computer-Using Agents"
publish_date: "2026-10-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.40578v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.40578v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-01T23:13:47"
---
# VeriCUA: Verification as Agentic Exploration for Computer-Using Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：computer-using agents · verification · agentic exploration · gui agents

## 一句话总结

VeriCUA 将计算机使用智能体的任务完成验证重新表述为“在可用证据源中主动探索并仲裁”的过程：先根据指令与上下文合成可核查的需求清单，再按需选择 GUI 检查或代码/状态检查收集证据，最后按显式证据规则给出 success、fail 或 abstain，从而用同一套平台无关流程覆盖在线与离线验证。

## 摘要

> Building and scaling computer-using agents (CUAs) across desktop, mobile, and web environments requires reliable task-completion verification for data curation, evaluation, and reinforcement learning. Typical automated approaches prompt vision-language models (VLMs) to judge trajectories or generate programs to inspect application state. However, both rely on partial views of complex digital environments. VLM judges are restricted to supplied observations, while generated programs depend on accessible state. These views can omit decisive evidence or leave completion judgments unsupported. Tailoring separate verifiers to different platforms and access settings adds further engineering cost. We present VeriCUA, a platform-agnostic framework for verification through agentic exploration. It treats verification as dynamic evidence gathering and assessment within a shared procedure for online and offline settings across platforms and observation modalities. After inspecting the available context to construct a task rubric, the verifier selects GUI or code checks for unresolved requirements and combines their findings under explicit evidence rules. Experiments show improved agreement with reference labels over the compared baselines across these settings, with the largest offline gains on hard cases and stronger rejection of false completion claims online. Further analyses connect these gains to complementary evidence and test-time exploration.

Q1: 这篇论文试图解决什么问题？

【问题定位】
VeriCUA 处理的核心问题不是“如何让 CUA 更会操作界面”，而是“如何可靠判断一次已经结束的执行是否真的完成了用户指令”。在桌面、移动端和 Web 环境中，任务成功可能取决于视觉呈现、应用内部记录、持久化文件或数据库状态；轨迹里出现点击、输入和看似合理的推理，并不等于这些动作产生了预期效果。论文把“尝试执行的动作”与“动作实际造成的效果”之间的区分，视为验证的中心困难。
【虚假完成的典型来源】
论文在 Introduction 中列出几类高风险情形：编辑未保存但界面看起来已经完成；操作失败却没有可见警告；agent 只完成了部分指令却报告完成；以及固定帧或摘要式轨迹遗漏了决定性状态变化。若把这类执行接受为成功，就会产生 false positive，污染评测、数据筛选和强化学习奖励。
【现有自动验证器的双重“部分观测”】
第一类方法是 VLM judge，例如单次调用的 TRM 式评委：它只根据被提供的观测和轨迹做判断，无法主动获取未给出的证据；固定帧可能恰好漏掉保存成功、文件写入或后台状态变化，评委也可能被 agent 的叙述或视觉表象误导。第二类方法是自动生成验证程序：让模型为任务写一个检查脚本，再对机器状态执行一次；但脚本只能查询“可访问的状态”，遇到封闭应用、权限限制、远程服务或未被暴露的 UI 状态时就无能为力，而且一次生成的查询未必命中真正决定任务完成的那部分状态。两类方法都依赖对复杂数字环境的部分视图，且各自只覆盖一种证据通道。
【平台定制带来的工程成本】
不同平台（Android、桌面 VM、Web）和不同访问设置（在线机器状态、离线保留轨迹）通常需要不同的 checker、不同的接口和不同的成功定义。论文指出，为每个平台单独定制验证器会带来额外工程成本，并限制验证器在数据 curation、benchmark evaluation 和 RL 中的规模化复用。VeriCUA 因此把“平台无关的共享验证流程”作为设计目标，而不是再做一个单平台 checker。
【验证任务的结构性难点】
（1）成功是二值的，但证据往往是不完整的，因此论文允许输出 abstain；（2）证据源异质，包括截图、accessibility tree（a11ytree）、文件、应用记录、系统服务、剪贴板、提交答案等；（3）在线与离线不对称：离线只能看保留的轨迹和记录，在线可以探查执行后的机器状态，甚至使用执行前快照作为参照；（4）需求可能涉及“执行过程中发生的变化”，需要比较前后状态；（5）不同证据的可信度不同，持久化状态与易失观测的证据权重不应相同；（6）参考标签本身可能有噪声，绝对一致率不是唯一判据。
【论文试图回答的研究问题】
可以概括为：能否用一个平台无关的验证器，在在线和离线设置中主动选择 GUI 或代码检查，针对未解决的需求收集互补证据，并在显式证据规则下做出可解释的 success/fail/abstain 判断？这种“验证即智能体探索”的范式，是否比单次 VLM 评判或一次性生成脚本更接近参考标签，尤其在困难样本和虚假完成声明上更可靠？
【隐含假设与边界】
方法隐含假设包括：验证时至少存在若干可访问的证据源；工具调用可以可靠执行；LLM 能从指令与上下文中合成足够好的 rubric；显式证据规则能够处理跨源冲突；benchmark 官方 evaluator 或人工标签可以作为合理 gold。论文的结论受这些假设约束；若状态完全不可访问、工具被权限封锁，或任务本身高度主观，框架的优势可能缩小。上述边界中，有些是论文明确讨论的，有些是依据方法设计做出的合理推断，阅读时应结合附录和失败案例分析确认。

Q2: 有哪些相关研究？

【CUA 环境与 benchmark】
论文建立在近年快速发展的 computer-using agents 研究之上。Introduction 引用 Rawles et al. 2025、Xie et al. 2024、Zhou et al. 2024 等，说明 VLM 已能让智能体通过软件界面观察、规划和行动；实验使用 AndroidWorld（Android 模拟器）、OSWorld（虚拟机桌面）、ScienceBoard（科学桌面环境）以及 WebArena 的离线轨迹。这些 benchmark 提供了任务、执行 scaffold 和官方 evaluator，但论文的关注点不是提出新的 CUA policy，而是验证这些 agent 的执行结果。
【VLM/LLM-as-judge 路线】
最接近的 baseline 是单次调用的 VLM judge，例如 TRM（Sun et al., 2025）：给模型提供选定的观测或轨迹，让它返回 verdict 或 score。OSReward 相关轨迹也使用单调用 judge 进行评分。CUAJudge（Xue et al., 2026a）进一步采用 checklist 式评委，从任务中提取关键点，再判断完成情况。论文认可这类方法成本低、通用性强，但指出它们受限于“被提供的观测”：评委不能主动获取截图之外的状态，固定帧可能遗漏决定性证据，且单次判断难以处理需要多步核实的需求。
【程序化/可执行验证路线】
另一条路线是自动生成验证器（Xue et al., 2026b）：为每个任务生成一个 evaluator script，对机器状态执行一次，返回二值 verdict。它比 VLM judge 更接近“真实状态”，但依赖可访问的应用程序接口、文件系统或系统服务；生成脚本可能查询错误状态、遗漏持久化效果，或在封闭应用中不可执行。论文将这类方法视为“单次、单通道、状态依赖”的对照，并用 VeriCUA 的主动多通道证据收集来补足。
【结构化评委与 rubric/checklist】
除 VLM judge 和自动生成验证器外，实验还比较了 CUAJudge 以及两个结构化评委（提供片段在名称处截断）。这类方法把任务拆成关键点或 checklist，提高了判断的结构性，但通常仍以静态输入为主，缺少在验证过程中动态选择检查、合并冲突证据和显式弃权的机制。VeriCUA 与它们共享“先分解需求再判断”的思想，但把分解后的需求接入 active exploration 和 rule-based arbitration。
【智能体式工具调用与主动感知】
VeriCUA 的“验证即探索”与 agentic tool use、active perception 的思路相邻：验证器不是一次性读取输入，而是把 GUI 检查、代码检查、系统服务查询当作动作，根据已有发现决定下一步查什么。论文通过 GUI-only 与 hybrid 的对比、观测模态消融和多 pass 分析，来支撑“主动收集证据”相对于“被动读取固定观测”的价值。这里与经典 agent planning 的关系属于合理推断，论文片段没有展开其与 ReAct 等具体框架的谱系。
【数据 curation 与强化学习】
Introduction 明确指出，可靠的完成判断不仅用于 benchmark evaluation，还用于 data curation（Sun et al., 2025）和 reinforcement learning（Xue et al., 2026a）。轨迹合成管线会用 verifier score 过滤或排序轨迹；RL 则可能把验证结果当作 reward。由此，验证器的 false positive 会直接变成脏数据或错误奖励。VeriCUA 的定位是这一数据/训练基础设施中的通用验证层，而不是某个 benchmark 的附属 checker。
【与本文的关系】
总体上，论文把已有工作归为“看轨迹的 VLM judge”“看状态的生成程序”“看关键点的结构化评委”三类，并指出它们共同受限于部分观测和平台定制。VeriCUA 的新意不在于发明某个单点工具，而在于把验证组织成一个跨平台、可探索、可仲裁、可弃权的共享流程；其相关研究边界因此同时覆盖 CUA 评测、LLM-as-judge、程序化验证和 agentic exploration。

Q3: 论文如何解决这个问题？

【形式化与输出空间】
给定指令 g，执行 agent 产生轨迹 τ=[o0,a0,…,oT]（包含观测、动作和记录的推理），并把环境留在状态 sT。VeriCUA 在尝试结束后启动，使用验证器可用的证据源 S={S1,…,Sm} 判断执行是否满足 g。任务完成本质上是二值的，但验证器输出 y∈{success, fail, abstain}：当证据不足以支持判断时允许弃权，而不是强行给出二值结论。这个三元输出是框架可靠性的重要组成部分，尤其在离线证据缺失或在线状态不可读时。
【证据源】
可用来源包括：保留的轨迹、记录下来的 accessibility tree（a11ytree）、冻结的机器快照、执行后可访问的机器状态；离线验证主要使用保留观测，在线验证可以检查暴露的机器状态，并同时使用执行记录。如果某个需求涉及“执行过程中发生的变化”，执行前的可用参照也可以作为证据源。方法片段还提到，在兼容应用 schema 下重复执行同一任务时，持久化的 attested read 可以确认需求，而易失观测只能提供佐证；这体现了证据持久性与可复核性的区分。
【阶段一：context inspection】
验证器先检查应用与可用观测，把相关信息收集为上下文 c。这个阶段的目的不是立即判断成败，而是弄清楚“当前能看什么、哪些状态可访问、任务涉及哪些应用与记录”，为后续 rubric 合成和检查选择提供条件。
【阶段二：rubric synthesis】
利用指令 g 和上下文 c，验证器合成一组使指令可核查的需求 R。也就是说，它把自然语言任务分解成若干可被证据支持或反驳的 requirement，而不是直接让评委给一个整体分数。片段提到 hard requirements：任务 verdict 为 success 需要所有 hard requirements 都得到满足；这使最终仲裁可以区分“必要条件”与“辅助证据”。
【阶段三：active exploration】
在第 k 步，检查动作定义为 αk=(vk,xk)∈A(S)，其中 vk 是工具，xk 是目标或参数；验证器针对尚未解决的需求选择 GUI 检查或代码/状态检查，并利用检查结果决定下一步查什么。工具面不仅包括截图和 a11ytree，还包括系统服务与应用状态探针，例如 dumpsys activity、settings_get、pm_list、clipboard_read、submitted_answer 等；代码检查可以读取文件、应用存储、包/活动状态、系统设置、剪贴板或 agent 提交的答案。片段中“one script against one piece of state”的描述说明，代码通道通常是把一个脚本对准某一类状态工件执行，而不是笼统地“跑一个总脚本”。
【阶段四：rule-based arbitration】
探索得到的发现由显式证据规则合并。关键规则包括：一个需求被标记为 met 需要 attested support；标记为 unmet 需要来自不同探针的 corroborating refutations；无法定论的证据使该需求保持 unknown。最终任务 verdict 在所有 hard requirements 满足时为 success；若有硬需求被可靠反驳则为 fail；若关键需求仍 unknown，则根据设置规则 abstain 或拒绝宣称成功。这里的“source-aware”意味着不同证据源的可信度、持久性和可复核性会被纳入仲裁，而不是把所有观察等同看待。
【在线与离线的统一】
同一共享流程在离线设置中导航保留的屏幕帧、动作日志和已记录的 a11ytree；在在线设置中探查可访问的机器状态，并可以结合执行前参照。平台差异被抽象为可用证据源和工具集合的差异，而不是为每个平台重写一套验证逻辑。
【与 baseline 的设计差异】
相比单次 VLM judge，VeriCUA 不是“给什么看什么”，而是主动选择检查；相比一次性生成脚本，它不是只查一个可访问状态，而是把代码检查与 GUI 检查组合，并用证据规则处理冲突；相比静态 checklist 评委，它把 checklist 变成待探索的需求，并允许弃权。
【设计取舍】
框架的代价是验证过程更复杂：需要工具调用、状态访问、rubric 合成和多步仲裁，成本和延迟高于单次 judge；同时，工具权限、应用 schema、状态可读性和 rubric 质量都会影响结果。论文的消融实验正是在检验这些取舍：混合动作空间是否真的带来互补证据，观测模态如何影响 coverage 与 accuracy，多 pass 是否只是增加计算量。

Q4: 论文做了哪些实验？

【评测矩阵】
在线环境包括 AndroidWorld（Android 模拟器）、OSWorld（虚拟机桌面）和 ScienceBoard（科学桌面环境）。每个 episode 由 benchmark 自己的 agent scaffold 执行，验证器在 episode 结束后探查机器状态。离线环境包括 WebArena 的已记录轨迹（来自三个执行 agent）和 OSReward 的人工标注轨迹；这些轨迹覆盖四个平台和多个 agent，用于测试离线验证的泛化。
【协议与公平性】
所有 verifier 和 baseline 主要运行在 gemini-3-flash-preview 上；backbone study 再把它们重跑到 gemini-3.1-pro-preview。在线环境中，gold 是各 benchmark 的官方 evaluator；执行 agent 也使用 gemini-3-flash-preview。WebArena 上，除了 GUI 输入，还向 verifier 和 judges 同样提供 a11ytree 变体，以避免“只给 VeriCUA 额外信息”的不公平比较。其余协议细节放在 Appendix B。
【Baseline】
（1）VLM judge：单次调用评委，例如 TRM（Sun et al., 2025），评估提供的观测并返回 verdict 或 score；（2）Auto-Generated Verifier（Xue et al., 2026b）：为每个任务生成一个 evaluator script，对机器状态执行一次，返回二值 verdict；（3）CUAJudge（Xue et al., 2026a）：checklist 式评委，从任务中提取关键点再判断；（4）两个结构化评委（提供片段在名称处截断）。这些 baseline 覆盖了“看轨迹”“看状态”“看关键点”三条主要路线。
【评价指标】
论文主要报告与参考标签的一致率/准确率，以及 decision rate（覆盖/决策率）、precision、recall、false positive、成本等指标；在线环境还关注对 false completion claims 的拒绝能力。离线分析特别区分 hard subset，并报告 per-agent 结果。讨论部分还使用 blind model adjudicator 检查参考标签噪声。
【消融与分析维度】
（1）GUI-only 与 hybrid（GUI+代码/状态检查）的动作空间对比；（2）观测输入消融：截图、a11ytree、两者结合；（3）单调用 OSReward judge 的最终帧数量变化；（4）验证 pass 数与 visual pass 的影响；（5）backbone 强度（gemini-3-flash-preview 与 gemini-3.1-pro-preview）；（6）per-agent 泛化；（7）hard subset 与失败案例；（8）参考标签噪声与盲审裁决。
【证据缺口】
提供材料没有给出各 benchmark 的完整任务数、执行 agent 的具体配置、所有数值表、显著性检验和成本明细；Appendix B/C 只在片段中被提及。方法片段还出现“重复执行同一任务、persistent attested reads 与 volatile observations”的讨论，但不足以确认它是否作为独立实验条件。因此，本文对实验规模的描述只能停在“多平台、多 agent、多观测模态”的层面，不能编造具体样本量。

Q5: 发现了什么实验现象？

【总体趋势】
VeriCUA 在在线与离线设置中均提升了与参考标签的一致率；摘要和结论强调，离线困难样本上的增益最大，在线则表现出更强的 false completion 拒绝能力和更少的 false positive。这说明收益不只是“平均样本上多对几个”，而是集中在证据缺失、状态复杂或 agent 容易虚报完成的困难场景。
【GUI-only 与 hybrid：代码通道补足状态证据】
在 AndroidWorld 上，把代码/状态检查加入 GUI 检查后，decision rate 与 recall 上升，false positive 保持不变（Figure 3）。配对分析显示，代码通道带来的每一个额外正确决策，都由文件读取或应用存储读取所支持；也就是说，代码检查不是重复 GUI 已经看到的信息，而是访问了 GUI 观测未解决的需求。图 3 的标注还显示混合动作空间带来 Precision +4、Recall +30 的方向性变化（具体是百分点还是相对增幅需回图注核对）。这组结果是“互补证据源”最直接的现象证据。
【观测模态：coverage 与 accuracy 的张力】
在 WebArena 上，把 a11ytree 加入截图会提高 VeriCUA 的 decision rate，但 accuracy 保持不变；只用 a11ytree 时 decision rate 最高、成本最低，但 accuracy 更低（Appendix C.2 的 observation-input ablation）。这说明结构化观测能扩大可判断范围、降低视觉成本，却不一定提高判断正确率；视觉信息与结构化信息各自覆盖不同需求，简单替换并不等价。与 baseline judges 比较时，它们在 tree-only 与 combined 输入下的 accuracy 变化不大，而 VeriCUA 在两种设置下都保持领先（Figure 4b）；per-agent 结果在 Table C.4。
【增加帧数：单调用 judge 的收益与上限】
作者改变提供给单调用 OSReward judge 的最终帧数量（Figure 4a）。增加帧数能提升它在 hard subset 上的 accuracy，但片段在转折处截断，随后应是对该做法局限的讨论；可确认的是“多给几帧”能部分缓解证据瓶颈，却不能等同于主动探索。这是合理推断：如果固定帧足够解决所有问题，VeriCUA 的探索机制就没有必要；而论文的整体结果支持后者在困难样本上更强。
【测试时探索与多 pass】
讨论部分报告，加入 visual pass 后，agreement 与 decision rate 进一步提升，二者均达到 87%（Figure 5a）；在匹配 pass 数的条件下，visual pass 仍然带来提升。这说明收益不只是“多跑几次”的计算量效应，验证过程中新增的视觉复核确实提供了有用证据。该现象与 active exploration 的设计一致：验证器可以根据未解决需求再检查一次，而不是一次性冻结判断。
【Backbone 强度】
把 backbone 从 gemini-3-flash-preview 换到 gemini-3.1-pro-preview 后，accuracy 进一步提升。论文把这作为“更强模型能改善验证”的证据，但也意味着框架的绝对表现受 backbone 能力约束；较小模型上的 rubric 合成、工具选择和证据仲裁可能更脆弱。
【参考标签噪声与盲审支持】
Discussion 片段提到参考标签中存在错误，并出现“83.8%”“88.9%”“80.0%”等数字；一个 blind model adjudicator 在 63% 的 Failure flagged trajectories 上支持 VeriCUA，OSWorld 上最明显。由于片段断行，这些数字各自对应 accuracy、agreement 还是错误率无法可靠还原，引用时必须回原文核对。但方向性信息很清楚：部分“VeriCUA 与标签不一致”的案例可能不是 VeriCUA 错，而是参考标签本身有问题；这会影响对绝对一致率的解读。
【指标间的张力与反例】
（1）a11ytree 单独使用提高 decision rate、降低成本，却降低 accuracy，说明 coverage 与 correctness 不是同一个轴；（2）代码检查提高 recall 但不降低 false positive，说明它主要解决“漏判成功/无法判断”，而不是直接提升所有判断的精确性；（3）增加帧数对单调用 judge 有帮助但有限，说明被动增加观测不等于主动寻找证据；（4）困难样本增益最大，暗示简单样本可能已接近 ceiling，方法优势主要体现在状态复杂、证据分散的任务；（5）abstain 机制可能提高可靠性，但会降低 coverage，提供片段没有给出 abstain rate，需回附录确认其实际取舍。

Q6: 有什么可以进一步探索的点？

【论文明确指向的应用】
结论明确提出，VeriCUA 可支持 CUA evaluation，并值得用于 data curation 和 reinforcement learning。一个直接方向是把验证器接入轨迹合成与筛选管线：用 success/fail/abstain 和证据链过滤训练数据，或把验证结果作为 RL reward。需要进一步研究的是 reward hacking、verifier bias 和 abstain 对训练信号覆盖率的影响。
【Rubric 合成的鲁棒性】
当前流程先把指令分解成 requirements，再针对未解决需求探索。未来可以研究：如何为模糊指令、跨应用任务、长时任务和主观任务合成更好的 rubric；如何区分 hard/soft requirements；如何处理用户未明说的隐含条件；如何检测 rubric 本身的遗漏或错误。若 rubric 错了，后续证据收集再充分也可能判错。
【证据仲裁与校准】
论文使用显式证据规则处理 met/unmet/unknown，但规则驱动的方式可能难以覆盖所有冲突。可探索学习型仲裁、概率化证据融合、源可靠性建模、校准的 abstention 策略，以及“证据充分性”的形式化标准。还可研究持久化 attested read 与易失观测的权重分配，以及如何避免伪造状态或不可信工具污染证据。
【预算感知的主动探索】
Active exploration 目前按需求选择检查，但实际部署需要控制工具调用次数、延迟和成本。未来可以做预算约束下的检查规划、停止准则、信息增益估计、并行探针调度，以及多 pass/visual pass 的最优分配。论文的多 pass 结果说明测试时探索有效，但何时停止、何时弃权仍是开放问题。
【更丰富的工具与状态访问】
方法片段列出了 dumpsys activity、settings_get、pm_list、clipboard_read、submitted_answer 等探针，未来可以扩展到更多系统服务、数据库、日志、网络请求、文件版本和跨应用状态。还需要处理权限受限、应用 schema 变化、封闭系统和远程服务等场景；在线验证尤其需要安全的沙箱与最小权限设计。
【跨平台与跨模态泛化】
论文覆盖 Android、桌面 VM、Web 和科学桌面环境，但平台数量仍有限。可进一步测试 CLI、API-only 应用、混合现实、机器人和多用户共享桌面；也可研究不同屏幕分辨率、语言、主题、无障碍树质量和多模态输入下的稳定性。
【评测方法与标签质量】
参考标签噪声是重要问题。未来可以建立更严格的人工裁决协议、对抗性 false completion 测试集、跨执行 agent 的泛化评测、以及统计显著性/置信区间报告。还应检查 benchmark contamination：验证器是否记住了任务而不是真的检查状态。
【安全、隐私与对抗鲁棒性】
验证器需要读取机器状态和屏幕内容，可能接触敏感信息；GUI 中也可能出现 prompt injection 或伪造状态，诱导验证器误判。未来需要研究验证器的攻击面、状态 attestation、权限隔离、审计日志和可解释证据链。
【相邻领域】
VeriCUA 的思路可迁移到软件工程 agent、Web agent、科学工作流自动化和具身智能的任务验证；与 AI for Science 的结合尤其自然：ScienceBoard 已显示科学桌面环境中的验证需求，未来可扩展到科学软件、数据分析和实验记录的可复核验证。

Q7: 总结一下论文的主要内容

【一、问题与动机】
论文从 CUA 的规模化需求出发：在桌面、移动端和 Web 环境中，agent 通过界面观察、规划和行动，但“轨迹看起来成功”不等于任务真的完成。编辑可能未保存，操作可能静默失败，agent 可能只完成部分指令却报告完成；这些 false positive 会污染 benchmark evaluation、data curation 和 reinforcement learning。现有自动验证主要有两条路线：VLM judge 根据提供的观测评判轨迹，自动生成程序检查应用状态。前者受限于固定观测，后者受限于可访问状态；两者都只看到复杂数字环境的部分视图，且为不同平台分别定制 checker 会增加工程成本。论文因此提出：验证不应是一次性读取，而应是在可用证据源中主动探索并仲裁。
【二、方法主线】
VeriCUA 把验证形式化为：给定指令 g、轨迹 τ 和执行后状态 sT，在可用证据源 S 中判断执行是否满足 g，输出 success、fail 或 abstain。共享流程分为四段。第一，context inspection：检查应用与可用观测，形成上下文 c。第二，rubric synthesis：用 g 和 c 合成使任务可核查的需求 R，片段特别提到 hard requirements。第三，active exploration：针对未解决需求选择检查动作 αk=(vk,xk)，工具可以是 GUI 检查（截图、a11ytree）或代码/状态检查（文件、应用存储、dumpsys activity、settings_get、pm_list、clipboard_read、submitted_answer 等），并用检查结果决定下一步。第四，rule-based arbitration：met 需要 attested support，unmet 需要来自不同探针的 corroborating refutations，无法定论则保持 unknown；所有 hard requirements 满足时判 success，否则根据证据判 fail 或 abstain。在线验证可以探查执行后机器状态，离线验证导航保留轨迹与记录；平台差异被抽象为证据源和工具集合的差异。
【三、实验主线】
在线评测覆盖 AndroidWorld、OSWorld 和 ScienceBoard，每个 episode 由 benchmark 自己的 agent scaffold 执行，验证器在结束后探查状态；离线评测使用 WebArena 的三个执行 agent 轨迹和 OSReward 的人工标注轨迹，覆盖四个平台和多个 agent。所有 verifier 和 baseline 主要跑在 gemini-3-flash-preview 上，backbone study 重跑到 gemini-3.1-pro-preview；WebArena 上向 verifier 和 judges 同样提供 a11ytree，以保持输入公平。Baseline 包括单次 VLM judge（如 TRM）、自动生成 verifier、CUAJudge 以及两个结构化评委。评价关注与参考标签的一致率、decision rate、precision、recall、false positive、hard subset、per-agent 泛化和成本。
【四、主要发现】
总体而言，VeriCUA 在在线与离线设置中均提升了与参考标签的一致率；离线困难样本增益最大，在线对虚假完成声明的拒绝更强、false positive 更少。机制分析给出几条关键现象：在 AndroidWorld 上，加入代码/状态检查提高 decision rate 和 recall，false positive 不变，且每个额外正确决策都由文件或应用存储读取支持，说明代码通道补足了 GUI 未解决的状态证据；在 WebArena 上，a11ytree 加入截图提高 decision rate 但 accuracy 不变，单独使用 a11ytree 时 decision rate 最高、成本最低但 accuracy 更低，显示 coverage 与 correctness 的张力；增加单调用 judge 的最终帧数能改善 hard subset，但存在局限，不能替代主动探索；加入 visual pass 后 agreement 与 decision rate 均达到 87%，匹配 pass 数后仍提升，支持测试时探索的价值；更强 backbone 进一步提升 accuracy。讨论还检查了参考标签噪声，盲审模型裁决在部分失败标记上支持 VeriCUA，OSWorld 最明显。
【五、意义与边界】
论文的贡献不是提出新的 CUA policy，而是把验证组织成平台无关、可探索、可仲裁、可弃权的共享流程，为评测、数据筛选和 RL 提供更可靠的 completion signal。其边界也很清楚：离线受保留观测限制，在线依赖可访问状态和工具权限；rubric、工具可靠性和证据规则都会影响判断；评测主要覆盖若干 benchmark 和有限 backbone；成本、延迟、abstain rate 和完整失败案例在提供片段中不充分。总体上，这是一篇以“验证范式”而非“单点模型”为核心的系统性工作，适合用来理解 CUA 数据飞轮中的关键瓶颈，以及 active evidence gathering 如何补足 VLM judge 与程序化 checker 的部分观测问题。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent 方向高度相关：论文把 CUA 的 completion verification 作为数据筛选、评测和 RL 的基础设施，适合用于 agent 数据飞轮与 reward 设计。

## 基本信息

- 作者：Qiushi Sun, Da Yu, Alexander Knop, Amer Sinha, Lingpeng Kong, Ben Kao, Ravi Kumar, Pritish Kamath
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.CV
- 日期：2026-10-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.40578v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Method、Results、Discussion 片段及 field_evidence_map，并对截断处的数值与统计口径做了显式不确定性标注。
