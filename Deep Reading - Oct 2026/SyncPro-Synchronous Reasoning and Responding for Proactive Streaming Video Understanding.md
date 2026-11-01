---
user_id: "cheng tan"
paper_id: 17959
arxiv_id: "2610.32132"
title: "SyncPro: Synchronous Reasoning and Responding for Proactive Streaming Video Understanding"
publish_date: "2026-10-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.32132.pdf"
pdf_url: "https://arxiv.org/pdf/2610.32132v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-02T01:16:28"
---
# SyncPro: Synchronous Reasoning and Responding for Proactive Streaming Video Understanding

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：proactive streaming video · streaming video understanding · dual-stream generation · synchronous decoding

## 一句话总结

SyncPro 通过在同一骨干上解耦 Inner 证据跟踪分支与 Response 回答分支，并以动作 token 实现独立激活和并发解码，使流式视频助手能够随证据变化自适应更新推理并及时回答用户。

## 摘要

> Proactive streaming video understanding requires assistants to track unfolding events and respond when a user's requested condition is met. To make these decisions, assistants must accumulate relevant evidence and reassess response conditions as new observations arrive. Existing work offers complementary capabilities for this process: proactive video assistants learn when to respond, while streaming video reasoning methods generate scheduled intermediate text to organize observations and support subsequent answers. Together, these capabilities provide a foundation for proactive reasoning, but how to coordinate ongoing reasoning with timely responses remains underexplored. In a direct adaptation, fixed reasoning intervals can trigger updates when nothing relevant has changed or postpone useful updates until the next scheduled step. Sharing one generation path also couples two distinct requirements: preserving evidence for future decisions and producing only the response requested by the user. Moreover, responses generated after reasoning must wait for it to finish, even when sufficient evidence is already available. To address these challenges, we introduce SyncPro, a synchronous dual-stream framework with specialized generation paths for evidence tracking and user-facing responses. Its Inner and Response branches share causal history and activate independently, allowing relevant evidence changes to trigger internal updates and responses to proceed concurrently with ongoing reasoning. We construct 189K dual-stream examples and train SyncPro through supervised fine-tuning followed by trajectory-level reinforcement learning. Experiments across three proactive benchmarks show improved response timing and more accurate answers to user requests.

Q1: 这篇论文试图解决什么问题？

一、任务定义与核心矛盾
主动式流式视频理解的核心是：模型只能看到 v1 到 vt 的因果前缀，却要判断用户查询 q 所要求的事件是否已经发生，并在合适时机输出回答。它同时包含两个决策：内部是否更新证据状态、外部是否说话。难点在于二者时间尺度不同：证据可能突然出现，也可能长时间无变化；回答则要求及时、简洁、只包含用户要的内容。这不是“把离线视频 QA 加速”那么简单，而是把“何时想、何时说”变成序列决策问题。

二、现有做法的三个结构性缺陷
1. 固定调度。ThinkStream 等方法按每个输入步或固定间隔生成中间推理，无法根据证据相关性决定“何时想、想多久”。这会导致两类错误：无变化时仍更新，浪费计算并引入噪声；有用证据出现时未到调度点，响应被拖延。
2. 角色耦合。推理文本既要为未来决策保留证据，又要直接服务用户回答；共享一条生成路径时，二者会互相污染：推理可能过度冗长、泄露内部状态，回答也可能被推理格式带偏，难以只输出用户请求的信息。
3. 串行等待。即使证据已经足够，回答仍需等当前推理生成完毕，造成首响应延迟；在实时交互中，这种等待会直接破坏主动性。

三、论文的问题拆解
作者把协调问题拆成三个子问题：自适应时机、角色分离、并发执行。对应 Fig.1 的三条主张：推理应跟随查询相关证据而非固定日程；推理维护查询相关状态、回答只履行用户请求；回答应能与推理并行，而不是排队。这个拆解是论文的核心概念贡献，比单纯增加一个模块更关键。

四、隐含假设与可质疑点
1. 假设 1 fps 采样足以支撑事件触发；对快速动作、细粒度手部操作或音频线索可能不成立。
2. 假设文本化内部推理确实有助于证据跟踪；若查询简单或视觉特征已足够，Inner 分支可能只是额外开销。
3. 假设两条分支可以在 token 级独立生成，再由共享历史同步；这要求分支间不会产生冲突或重复，论文用动作 token 和回合末同步来约束，但没有理论保证。
4. 合成数据管线依赖强教师模型；若教师对“何时该说”的偏好有偏，学生会继承。
5. 评测用 2 秒容差和 GPT-4.1 语义判分，可能奖励“早说但内容泛化”的行为；需要看精确率与人工评估。

五、问题价值
该问题位于 streaming video、proactive assistant 和 agent 决策的交叉点：它把“是否行动”从隐含的对话策略变成可监督的序列决策，为后续做事件驱动、异步工具调用和长时程监控提供了可复用的任务形式。

Q2: 有哪些相关研究？

一、主动式视频助手
这一线关注“何时响应”。VideoLLM-online 让视频 LLM 在流式输入中保持对话；Flash-VStream 用记忆压缩和高效推理处理长视频流；Dispider、StreamForest、MMDuet/MMDuet2 将主动响应、多轮交互和流式记忆结合；Em-Garde、VideoChat3 代表更强的在线视频对话系统。它们的共同点是学习或设计响应触发策略，但通常不显式维护一条独立的证据推理轨迹。

二、流式视频推理
Guan et al. 2026、Zhang et al. 2026 等工作在观察到达时生成中间文本，把当前片段组织成可复用的状态。ThinkStream 最接近本文：它用增量推理决定继续观察还是回答。但 ThinkStream 的推理按固定日程在每个输入步执行，且推理与回答共用一条生成序列，因而存在本文指出的冗余更新、角色耦合和串行等待。

三、离线视频 LLM
Maaz et al. 2024、Lin et al. 2024 等离线视频理解模型在完整视频上做问答，不处理因果流式约束。它们提供视觉编码和视频 token 压缩的基础，但不解决“何时说”。SyncPro 以 Qwen3.5-4B 为基座，属于在通用视频 LLM 上增加流式控制层的工作。

四、基准与评测
StreamingBench-PO 用 Acc@2s 和 Joint@K s 衡量响应时机与内容；OVO-FAR 用 REC/SSR/CRR 覆盖重复计数、步骤顺序和线索充分性，报告 2 秒窗口内的 precision/recall/F1；ProactiveVideoQA 用 PAUC 在 WEB/EGO/VAD 窗口上积分内容分数。这些基准共同把“何时说”和“说什么”分开评测。

五、相关方法论
轨迹级强化学习、动作 token 控制、双流/双分支生成在 agent 和流式对话中都有先例；SyncPro 的区别是把它们组合成一个可训练的双流架构，并用 189K 合成样本做 SFT 冷启动。与纯 prompt 工程或固定调度相比，这更接近系统级方案。

六、定位差异
本文不是提出新的视频编码器，也不是追求离线视频 QA SOTA；它的竞争点是“协调”：让 Inner 与 Response 解耦、独立触发、并发解码，同时用共享历史保持因果一致性。相关工作中最需要直接对比的是 ThinkStream 和 Interleaved 基线。

Q3: 论文如何解决这个问题？

一、形式化
用户指令 q 给出监控目标与所需回答；视频以因果序列 V=(v1,...,vT) 到达，每个观测 vt 是一个 micro-turn。实验按 1 fps 采样，因此 vt 是一帧。模型在 t 时刻只能看到 ct=(q,Ht-1,vt)，Ht-1 是此前保留的信息，未来帧不可见。

二、双分支与动作空间
SyncPro 把生成分成 Inner（I）和 Response（R）两条分支。Inner 跟踪查询相关证据，Response 生成面向用户的输出。每个分支在每一轮先选择一个动作：Inner 为 silent 或 inner，Response 为 silent 或 speak。动作 token 之后接分支特定的自回归内容；silent 立即终止该分支。两条分支各自只关注 ct 和自身已生成 token，因此可以独立激活，也可以在同一轮同时激活。只有 Response 的活动内容返回用户。

三、共享历史与同步
两条分支在同一 micro-turn 内并发解码，回合末把完整输出 o_I^t 与 o_R^t 一起写入共享上下文：Ht=U(Ht-1,o_I^t,o_R^t)。这样 Inner 保留的证据可供后续轮次使用，Response 的回答不会丢失，同时避免了一条分支的中间 token 阻塞另一条。这个设计是“解耦生成、共享状态”：生成路径分开，因果历史不分家。

四、训练数据
作者构建 SyncPro-189K，约 189,403 条双流样本，约 13.52M token（表格片段，需核对），来自七个视频来源（包括 Visual Genome 等）。数据由 Gemini 3.1 Pro Preview、Qwen-VL-Max-Latest、GPT-5.4 参与的合成管线生成，角色包括 proactive query 生成、双流轨迹合成与修订、response 重建、语义判分。片段还显示 keep/adapted 两类目标约 72.8%/27.2%，可能对应“保留历史证据”与“按当前查询重构回答”两种行为；具体定义需回原文确认。

五、训练流程
先以 Qwen3.5-4B 为基座做 SFT，学习动作 token、分支内容和共享历史更新；再用轨迹级 RL 在 800 条平衡覆盖四类行为组的样本上做一遍优化。RL 的目标不是单 token 似然，而是整条双流轨迹的时机与内容质量。四类行为组的具体划分片段未完整给出，合理推测对应两条分支的激活组合，但需核对。

六、为什么能解决问题
1. 自适应时机：动作 token 由当前证据和查询共同决定，模型可以跳过无关帧，也可以在证据出现时立即 inner/speak。
2. 角色分离：Inner 可以保留冗长证据，Response 只生成用户请求的内容；两条路径不共享已生成 token，减少格式和内容污染。
3. 并发：两条分支独立解码，Response 不必等待 Inner 结束，首响应延迟下降。
4. 因果一致：回合末统一更新 Ht，后续轮次仍能看到两条分支的完整输出。

七、设计代价与缺口
双分支会增加推理并行度与显存开销；每轮都要判断两个动作，动作 token 的类别不平衡可能影响训练；共享骨干仍可能让两条分支相互干扰；论文片段未给出损失函数、RL 算法、reward 设计、分支参数是否独立等关键实现细节，复现前需核对 Appendix A。

Q4: 论文做了哪些实验？

一、训练与推理设置
基座为 Qwen3.5-4B；SyncPro-SFT 在 SyncPro-189K 上训练；RL 只对 800 条唯一样本做一遍，样本平衡覆盖四类行为组。主动式评测中，SyncPro-RL、SyncPro-SFT、Interleaved、Response-only 均按 1 fps 运行；VST 与 ThinkStream 按发布实现和协议在 2 fps 下评测。

二、对比方法
包括 VideoLLM-online、Flash-VStream、Dispider、StreamForest、MMDuet、MMDuet2、Em-Garde、VideoChat3。分数主要按 Em-Garde 协议复现，例外是 MMDuet 的 PO 与 VideoChat3 的 PVQA 直接取自原论文。这个细节很重要：跨协议拼表会引入比较偏差，读表时要看脚注。

三、基准与指标
1. StreamingBench-PO：Acc@2s 衡量响应是否在 2 秒容差内触发；Joint@K s 进一步要求内容与触发在同一预测下匹配，是“时机+内容”的联合指标。
2. OVO-FAR：覆盖 REC（重复计数）、SSR（步骤顺序）、CRR（线索充分性），在 2 秒窗口内报告 precision、recall、F1。
3. ProactiveVideoQA：PAUC（ω=0.5）用 GPT-4.1 对 WEB、EGO、VAD 三类响应窗口的内容打分并积分，既看时机也看内容质量。

四、主结果
SyncPro-RL 在 StreamingBench-PO 上达到 41.2% Acc@2s（摘要/结果片段）。OVO-FAR 上，SyncPro-RL 汇总为 34.19，SyncPro-SFT 为 32.37，Interleaved 仅 10.87。分项（P/R/F1）：SyncPro-RL 的 REC 为 16.99/64.58/26.91，SSR 为 17.50/48.17/25.67，CRR 为 35.43/84.95/50.00；SyncPro-SFT 对应 18.09/62.50/28.06、13.14/57.06/21.36、33.12/85.14/47.69；Interleaved 对应 23.18/41.97/29.86、2.38/1.59/1.90、0.52/2.08/0.83。这些数字显示 Interleaved 在 SSR/CRR 上几乎崩溃，而 SyncPro 保持了可用的 F1。

五、消融与对照
论文设置 Interleaved、Response-only、Scheduled reasoning 等对照。Interleaved 代表把推理与回答串行交错的朴素组合；Response-only 推测为去掉 Inner 分支、只保留回答路径的对照；Scheduled reasoning 使用固定 Inner 触发、Response 自行决定是否说话，用于检验自适应触发的价值。片段只显示 Scheduled 的机制描述，具体消融数值未完整出现，属于证据缺口。Appendix F 还展示选择性更新、单独激活与联合激活的定性例子。

六、效率与离线保持
效率表片段显示：两条分支全部完成约节省 114.74 ms，加速 1.3706×；首个响应 token 约节省 199.52 ms，加速 2.2152×。这些数字应理解为相对串行交错生成的延迟收益，具体基线需看表头。离线视频理解方面，三个离线基准（Li et al. 2024、Mangalam et al. 2023、Fu et al. 2025）上两个 SyncPro 变体与基座差距保持在 2.0 分以内，说明主动式训练没有明显破坏通用视频理解。

七、实验覆盖与缺口
覆盖三个主动式基准、多个 baseline、消融、延迟和离线保持，设计较系统。但片段未给出：完整主表、PAUC 的具体数值、Joint@K s 的分项、多次运行方差、训练算力、RL reward 细节、人工评估和失败案例。读原文时应重点补齐这些。

Q5: 发现了什么实验现象？

一、朴素交错推理在 OVO-FAR 上出现“崩溃式”退化
最醒目的现象是 Interleaved 在 OVO-FAR 的 SSR/CRR 上 F1 只有 1.90 和 0.83，而 REC 仍有 29.86。这说明它不是完全不会回答，而是把“推理文本”和“用户响应”混在一条序列后，在需要步骤顺序和线索充分性判断的任务上产生大量不匹配输出。一个合理解释是：固定调度的中间推理不断插入，破坏了响应窗口和内容对齐；但论文片段没有给出错误分析，只能标注为合理推断。

二、SyncPro 的收益主要来自“能触发且能对齐”，不是单纯提高召回
SyncPro-RL 的 CRR 召回 84.95、F1 50.00，SSR 召回 48.17、F1 25.67，REC 召回 64.58、F1 26.91。整体呈现“高召回、低精确”：模型倾向于在证据出现时说话，但不少回答落在错误窗口或包含多余内容。精确率分别只有 16.99、17.50、35.43，说明误触发/多余回答是主要错误来源。对主动式助手而言，这直接对应“频繁打扰用户”的风险。

三、RL 与 SFT 的差异不是全面上涨，而是 precision-recall 权衡
从 SFT 到 RL，OVO-FAR 汇总 32.37→34.19；SSR F1 21.36→25.67，CRR F1 47.69→50.00，但 REC F1 28.06→26.91 反而下降。拆开看，REC 精确率 18.09→16.99 下降，召回 62.50→64.58 上升。也就是说 RL 让模型更愿意说、更少漏答，但更容易误报。这个趋势与“轨迹级 RL 优化整体时机”的直觉一致，也提示奖励设计可能偏向召回。

四、时机指标与内容指标存在张力
StreamingBench-PO 的 Acc@2s 达到 41.2%，说明响应时机有提升；但 Acc@2s 只要求触发在容差内，不保证内容正确。Joint@K s 和 ProactiveVideoQA PAUC 才把内容纳入。片段没有给出 Joint@K s 的完整数值，因此不能仅凭 41.2% 判断“回答质量”；需要看联合指标和 PAUC 分项。

五、并发解码的延迟收益明显，但测量口径需核对
首个响应 token 节省 199.52 ms（2.2152×），两条分支全部完成节省 114.74 ms（1.3706×）。首 token 的加速比更大，符合“Response 不等 Inner”的设计；但“全部完成”的收益较小，说明 Inner 仍然占用相当计算。若实际部署中 Inner 很长，Response 虽可先出首 token，后续仍可能被资源竞争拖慢。

六、离线能力基本保持，但不是零代价
两个变体在三个离线基准上与基座差距在 2.0 分以内。这个结果支持“主动式训练不会灾难性遗忘”，但 2 分以内的下降在 4B 模型上仍可能影响下游；论文没有给出逐基准的差值，无法判断是否某些能力被牺牲得更多。

七、定性现象：选择性更新与单独/联合激活
Appendix F 展示模型会跳过无关帧、只在证据变化时 inner，也有 Response 单独说话或两条分支同时激活的例子。这与“动作 token 学到自适应调度”的假设一致，但定性例子容易挑选；需要看失败案例，例如证据不足时抢先说、证据充分时沉默、Inner 内容漂移等。

八、数据分布可能影响行为
keep/adapted 约 72.8%/27.2%，若 keep 样本占多数，模型可能偏向保留历史证据而非按当前查询重构回答；这与低精确率、高召回的现象可能相关。但片段没有给出按组结果，不能下结论。

九、负结果与边界
Interleaved 的失败说明“有推理”不等于“会协调”；Scheduled reasoning 的固定触发若消融数值不佳，则进一步支持自适应触发。当前片段缺少这些数值，属于需要回原文确认的关键负结果。另一个边界是 1 fps：快速事件可能在帧间被错过，导致召回上限；论文未报告不同帧率下的曲线。

Q6: 有什么可以进一步探索的点？

一、从固定/学习触发走向事件驱动与预算感知
可以研究显式的“证据变化检测”：用轻量模块监测帧间差异，只在候选事件附近激活 Inner；或用 learned halting/optimal stopping 决定推理长度。进一步可做计算预算约束下的调度，在延迟、准确率和打扰成本之间优化。

二、更彻底的角色解耦
当前两条分支共享骨干，仍可能相互干扰。可尝试独立 LoRA/专家、信息瓶颈、对比学习，让 Inner 保留证据、Response 只拿“可回答摘要”。同时研究隐私：Inner 可能包含敏感观察，如何保证不泄露到用户侧。

三、流式推理系统优化
将双分支并发解码与 KV cache 复用、speculative decoding、chunked prefill、边缘设备调度结合；测量首 token 延迟、尾延迟、吞吐和显存。论文的 114.74 ms/199.52 ms 是单点结果，真实部署需要不同硬件和批大小下的曲线。

四、多模态与变速率输入
当前 1 fps 单帧视觉输入可能漏掉音频、语音、字幕、快速动作。可扩展到 audio/ASR、事件相机、可变帧率、主动帧选择；研究“何时采样”与“何时推理”的联合策略。

五、长时程记忆与多查询
真实助手可能同时监控多个条件、跨小时运行。需要层次化记忆、压缩、检索、遗忘和查询切换机制；研究多查询冲突时如何分配 Response 通道。

六、强化学习与信用分配
轨迹级 RL 可以进一步做过程奖励：对“证据出现后多久说”、“内容是否被支持”、“是否多余”分别给奖励；用 verifiable rewards、offline RL、偏好优化降低方差。800 条样本一遍训练很高效，但需要多 seed、更多数据和鲁棒性分析。

七、校准与用户交互
主动式系统必须校准“说”的置信度，支持 abstain、延迟确认、用户打断和纠正。可研究代价敏感的触发阈值：误报成本高时保守，漏报成本高时激进；并做真实用户研究。

八、评测体系
需要更细的指标：触发延迟分布、内容忠实度、证据支撑度、重复回答率、用户打扰率；以及多轮、跨域、真实场景的 benchmark。当前 2 秒容差和 GPT-4.1 判分是合理起点，但不足以覆盖所有失败模式。

九、跨领域迁移
可把 SyncPro 的“内部分支+回答分支”迁移到可穿戴助手、机器人、自动驾驶、监控、直播体育、会议助手、教育辅导和科学实验监控。对 ai-for-science，一个具体方向是把实验视频/仪器流作为输入，Inner 跟踪异常证据，Response 在条件满足时提醒；但本文未验证，属于推测。

十、理论与数据
可用 POMDP/optimal stopping 形式化“何时更新/何时回答”，分析信念状态与延迟的权衡；数据侧减少对 proprietary teacher 的依赖，做 human-in-the-loop 验证、反事实轨迹和因果干预。

Q7: 总结一下论文的主要内容

一、论文要解决的问题
SyncPro 研究主动式流式视频理解：用户给出一个监控条件与所需回答，视频以因果流到达，助手要在事件发生时及时回应，而不是看完整个视频再答题。这个任务要求模型同时做两件事：持续跟踪与查询相关的证据，以及判断当前是否满足回答条件。现有工作分成两条线：主动式视频助手学习“何时响应”，流式推理方法生成中间文本组织观察；ThinkStream 试图结合二者，但按固定日程在每个输入步推理，且推理与回答串行放在一条生成序列里。作者指出三重错配：固定间隔会在无变化时冗余更新、在有用证据出现时延迟更新；共享生成路径把“保留证据”与“只回答用户请求”耦合；串行生成让回答必须等待推理完成。

二、方法主线
SyncPro 在同一骨干上设置两条专用生成路径：Inner 分支跟踪查询相关证据，Response 分支生成用户可见回答。每个 micro-turn 中，两条分支各自先输出动作 token：Inner 选择 silent 或 inner，Response 选择 silent 或 speak；动作后接分支特定的自回归内容。两条分支只关注当前输入 ct=(q,Ht-1,vt) 和自身已生成 token，因此可以独立激活，也可以同时激活；只有 Response 内容返回用户。回合末，两条分支的完整输出共同更新共享历史 Ht=U(Ht-1,o_I^t,o_R^t)，保证后续轮次既能看到证据，也能看到已回答内容。这个“解耦生成、共享因果历史”的设计，是论文的技术核心。

三、数据与训练
作者构建 189K 双流样本（约 189,403 条、13.52M token，表格片段），来自七个视频来源，并用 Gemini 3.1 Pro Preview、Qwen-VL-Max-Latest、GPT-5.4 参与合成：生成 proactive query、合成和修订双流轨迹、重建 response、做语义判分。数据包含 keep/adapted 两类目标，比例约 72.8%/27.2%，可能分别对应保留历史证据与按当前查询重构回答。训练分两段：先以 Qwen3.5-4B 为基座做 SFT，学习动作 token、分支内容和历史更新；再用轨迹级 RL 在 800 条平衡覆盖四类行为组的样本上做一遍优化。

四、实验主线
实验覆盖三个主动式基准：StreamingBench-PO（Acc@2s、Joint@K s）、OVO-FAR（REC/SSR/CRR 的 P/R/F1）、ProactiveVideoQA（PAUC，WEB/EGO/VAD）。对比 VideoLLM-online、Flash-VStream、Dispider、StreamForest、MMDuet、MMDuet2、Em-Garde、VideoChat3，并复现 VST、ThinkStream。主结果中，SyncPro-RL 达到 41.2% Acc@2s；OVO-FAR 汇总 34.19，高于 SyncPro-SFT 的 32.37，远高于 Interleaved 的 10.87。分项看，SyncPro-RL 的 REC 为 16.99/64.58/26.91，SSR 为 17.50/48.17/25.67，CRR 为 35.43/84.95/50.00；Interleaved 在 SSR/CRR 上只有 1.90/0.83 F1，说明朴素串行交错会严重破坏步骤与线索判断。

五、效率与离线保持
并发解码带来延迟收益：首个响应 token 节省约 199.52 ms（2.2152×），两条分支全部完成节省约 114.74 ms（1.3706×）。在三个离线视频理解基准上，两个 SyncPro 变体与基座差距保持在 2.0 分以内，说明主动式训练基本保留了通用理解能力。Appendix F 的定性例子展示选择性更新、单独激活和联合激活。

六、关键发现与解释
1. 最大反直觉结果是 Interleaved 在 OVO-FAR 上崩溃：有推理并不等于会协调，固定串行交错可能比不做 Inner 更差。
2. SyncPro 的 OVO-FAR 呈现高召回、低精确：REC/SSR/CRR 精确率仅 16.99/17.50/35.43，说明主要错误是误触发和多余回答，而非漏答。
3. RL 相对 SFT 不是全面上涨：汇总和 SSR/CRR 提升，但 REC F1 从 28.06 降到 26.91，精确率下降、召回上升，体现 precision-recall 权衡。
4. 时机指标与内容指标存在张力：41.2% Acc@2s 只说明触发在 2 秒内，不能单独证明内容正确；需要 Joint@K s 与 PAUC 分项。
5. 离线能力基本保持但有 2 分以内代价，4B 模型上的具体影响需看逐基准差值。

七、局限与定位
论文的主要局限包括：合成数据依赖 proprietary teacher，存在偏差和复现性问题；只做 1 fps 视觉输入，缺音频和快速事件；只验证 Qwen3.5-4B，缩放性未知；RL 仅 800 条一遍，方差和过拟合风险未报告；OVO-FAR 精确率偏低；评测依赖 2 秒容差和 GPT-4.1 判分；部分 baseline 分数跨协议引用；实现细节（损失、reward、分支参数）在片段中缺失。总体看，SyncPro 的贡献不是新的视频编码器，而是把主动式视频助手拆成“内部证据跟踪”与“用户回答”两条可独立触发、并发解码的生成路径，并用数据与 RL 把这种协调行为训出来。对关心 streaming agent、异步生成和事件触发决策的读者，这是一篇系统性较强的参考工作；对追求离线视频 QA SOTA 的读者，则不是同一赛道。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与 agent 方向直接相关：SyncPro 把“何时更新内部状态、何时采取外部动作”做成可学习动作 token，和 event-triggered agent、异步工具调用、optimal stopping 是同一类问题。

## 基本信息

- 作者：Xueyun Tian, Bingbing Xu, Jiaming Liu, Jinpeng Yu, Yang Shi, Yu Zhao, Ruihua Huang, Yuanzhuo Wang, Huawei Shen
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-26
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.32132`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Method、Results、Conclusion 片段，并对 OCR 噪声、表格断片和缺失数值做了显式校准；未在片段中出现的关键实现与完整指标已标注为证据缺口。
