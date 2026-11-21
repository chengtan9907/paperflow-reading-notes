---
user_id: "cheng tan"
paper_id: 22370
arxiv_id: "2611.22925v2"
title: "Code World Model 2.0: Towards Infinite Controllable Video Generation"
publish_date: "2026-11-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.22925v2.pdf"
pdf_url: "https://arxiv.org/pdf/2611.22925v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-21T15:23:27"
---
# Code World Model 2.0: Towards Infinite Controllable Video Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video world model · proxy-conditioned rendering · few-step distillation · autoregressive video generation

## 一句话总结

论文提出 Invoker-Render：把双向视频创作模型与因果四步流式渲染器统一在同一套「结构代理 + 外观参考 + 语言」条件接口下，并用 Progressive Forcing（先在数据锚定历史上做因果适配与四步蒸馏、再切换到自生成 rollout 上继续训练）缓解步数压缩与自回归漂移的叠加问题，配合 KV 持久化、codec 流水线的服务栈，实现多粒度代理可控、可维持 30 分钟级长程一致性的流式视频渲染。

## 摘要

> Visual proxies decouple explicit world-state simulation from photorealistic video synthesis, offering a controllable interface for both offline clip creation and real-time interactive streaming. Yet a practical renderer must address two challenges. Upstream proxies vary from coarse layouts to fine articulated geometry depending on the interaction, and streaming inference must operate in a few causal steps without accumulating drift across self-generated history. We present Invoker-Render, pairing a bidirectional creation model with a causal four-step streaming model under a shared conditioning interface over structural proxies, an appearance reference, and language. Trained on an aligned approximately 200-hour multi-granularity corpus, Invoker-Render honors fine geometric constraints when supplied while synthesizing plausible local motion under coarse proxies, enabling runtime selection of input detail. To bridge bidirectional creation and real-time streaming without compounding step compression and rollout drift, we introduce Progressive Forcing (ProForcing). ProForcing first trains a causal predictor and distills it into a four-step renderer using histories derived from training videos. Training then shifts to continuous self-generated rollouts. A pipelined serving stack supports efficient streaming inference. Experiments show versatile control across proxy families, granularities, and camera paths, consistent gains over recent streaming baselines, and stable 30-minute racing rollouts that preserve scene and subject appearance.

Q1: 这篇论文试图解决什么问题？

一、范式层面的问题。论文所处的位置是「交互式视频世界模型」：用户动作改变环境，系统必须合成连贯观测。主流做法是在单一网络里同时模拟动力学与外观；本文沿用并强化的是另一条路线——把显式状态维护交给上游引擎（proxy 生成侧），把视觉合成委托给一个生成式渲染器。这样做的动机是可控性：相机路径、布局、实体运动这些结构化量在上游是可编辑、可校验的，而照片级真实感交给视频模型。问题因此从「如何端到端模拟世界」重写成「如何忠实渲染任意质量的代理」。

二、第一个技术挑战：代理粒度可变且语义不同。实际交互中，广域导航只需要粗粒度场景几何与根轨迹（root trajectory），近距离交互则需要关节姿态乃至更细的表面结构。细代理约束更强但构建与传输成本高；粗代理轻量但局部几何与运动欠定。渲染器必须同时忠实解释这两种情形，而且不能把「约束」误读为「冻结」：论文举的例子是，一个跟踪角色根姿态的刚性人体模型（rigid mannequin）应当只约束全局位置与朝向，而不应把僵硬的肢体姿态强加给生成的运动。这实质上是要求模型学会区分代理中「有信息的维度」与「缺省/待补全的维度」，是一个条件信号语义解析问题，而非单纯的图像质量提升问题。

三、第二个技术挑战：流式推理的误差叠加结构。实时渲染要求每个 chunk 只在少数因果步内完成（本文是四步），这需要 few-step 蒸馏；同时流式意味着模型要吃掉自己生成的历史，自回归 rollout 会累积漂移。论文明确指出，直接把「步数压缩」和「rollout 漂移」两件事放在一起优化会互相放大：在训练视频构成的「干净历史」上做蒸馏，模型学到的纠错行为与实际推理时面对的「自生成历史」分布不匹配；而在自生成历史上直接做 few-step 蒸馏又极不稳定。因此需要一个训练课程把两件事解耦再耦合，这正是 Progressive Forcing 要解决的问题。

四、第三类问题（工程/系统层）。即使模型侧达标，若没有 KV 持久化、算子融合、codec 与推理流水线化的服务栈，低延迟分块执行无法落地；长会话还需要有界的推理成本（本文用静态前缀 sink 加滑动 KV 窗口来约束），而这又带来「滑出窗口的视觉细节被驱逐」的新问题。

五、评价层面的隐含假设与空白。论文用 Gemini 3.8 Flash 做盲测成对偏好并折算 Elo，这假设 MLLM 评判与人类偏好足够对齐，且四准则（Visual Quality、Structure & Motion、Ref. Preservation、Overall）能覆盖渲染质量的关键维度；但这类协议对「物理合理性」「长期身份一致性」的判别力在材料中没有被验证。此外，材料未提供推理延迟/FPS/显存等系统指标，因此「实时」这一核心承诺在本篇可获取证据中是定性而非定量的，是需要回原文确认的缺口。

Q2: 有哪些相关研究？

说明：本次可获取的材料中 Related Work 章节正文缺失，以下脉络依据 Introduction、Method 片段、Experiments 与 Conclusion 中出现的引用编号与系统名重构，凡属推断均标注。

1) 交互式视频世界模型（引用 [2, 51, 52, 70]）。这是论文的问题域母体：在用户动作驱动下合成连贯观测。论文的立场是把「动力学模拟」与「外观合成」分开，而非在单一网络内耦合。

2) 解耦式「上游引擎 + 生成渲染器」范式（引用 [8, 20, 56]）。这一类工作维护显式世界状态，把视觉合成下放给生成模型。本文与它们的差别在于不假设上游代理的质量固定，而是把「多粒度」当成一等公民来设计条件接口与训练数据。

3) 视觉/几何代理作为两阶段之间的桥（引用 [7, 26, 27, 41]）。代理承载相机路径、布局、实体运动，把照片级外观留给视频模型。本文在此脉络上追加的贡献是：让粗代理与细代理走同一条条件通路（identical conditioning pathway）并在混合粒度样本上联合训练，从而支持运行时选择细节层级。

4) 结构线索 + 外观参考的离线创作与实时渲染系统（引用 [12, 19, 31, 34, 67]）。这是最直接的竞争簇。本文的三个对照基线正落在这里：SolarRenderer（Causal, 4-step）[12]、StreamRender-H3（2-step）[34]、Streaming CWM（4-step）[28]。其中 SolarRenderer 同时被借用为成对偏好评测协议的参照（“Following SolarRenderer [12]”），说明该协议在这一子领域已有先例。

5) 基础模型与蒸馏技术。方法上，Invoker-Render 是把 MiniMax-H3 [42] 适配成「双向创作模型 + 因果四步流式渲染器」两种形态，因此它与底座模型的能力边界强相关。四步蒸馏同时使用 DMD 与 GAN 式对抗目标，对抗部分采用成对 relativistic logistic loss（引用 [23, 63]），即在相同 (t, ε) 扰动下比较真实窗口与生成窗口的判别 logits。这条技术线属于 few-step 扩散蒸馏与对抗蒸馏的交叉。

6) 长上下文/流式推理的 KV 管理。本文使用静态前缀 sink 加滑动 KV 窗口来控制长会话成本，这一思路与长上下文推理中的 attention sink / 滑窗缓存工作同源，但材料未给出该脉络的具体引用编号，属推断。

7) 与本文构成「竞争范式」的对照：端到端视频世界模型（在单网络内同时学动力学与外观）与纯视频生成模型（无代理条件）。论文的论证策略是承认后者的画质上限，但主张在可控性与长程稳定性上，代理条件 + 显式接口更占优。

Q3: 论文如何解决这个问题？

一、总体架构。Invoker-Render 由两个共享条件接口的模型组成：(a) 双向创作模型，用于离线高质量片段创作；(b) 因果四步流式模型，用于实时交互式渲染。两者都以 MiniMax-H3 [42] 为底座进行适配，都接受三类条件输入：结构代理（structural proxy，可为相机路径、场景布局、根轨迹、关节几何等）、外观参考图（appearance reference）、语言（language）。共享接口的关键收益是「一次训练、两种推理形态」，且创作侧与流式侧的视觉先验不分裂。

二、多粒度条件的设计要点。粗代理与细代理进入同一条条件通路，并在混合粒度样本上联合训练，因此不需要在推理时切换模型或分支。语义上，论文要求模型对代理做「选择性服从」：细几何被提供时严格遵循（材料中提及细代理会配合配对的 RGB 目标与动作文本，教会生成器合成与轨迹一致的自然步态与姿态）；粗代理下则自行补全合理的局部运动（如刚性人体模型只约束全局位置与朝向，不冻结肢体姿态）。这等价于把「代理的完备程度」隐式编码进条件信号本身，让模型在运行时按需选择输入细节层级。

三、Progressive Forcing（ProForcing）：解耦再耦合的训练课程。核心动机是避免「步数压缩」与「rollout 漂移」相乘。三段式设计（依据摘要与结论的表述整理）：
1) 先训练一个因果预测器，完成从双向到因果的适配；
2) 再用从训练视频导出的历史（data-anchored histories）把它蒸馏成四步渲染器，蒸馏同时使用 DMD 与 GAN 式对抗目标；
3) 随后训练转向连续的自生成 rollout（self-generated rollouts），让模型在「自己写出来的历史」上学习有状态的延续（stateful continuation）。
这样，因果适配与四步蒸馏发生在干净、可控的历史分布上，而漂移抑制发生在真实推理会遇到的自生成分布上，两者不互相污染。对抗部分的实现细节在片段中可见：真实窗口 x0 与生成窗口 x̂0 在匹配条件下、共享外观锚点（shared appearance anchor）下用相同 (t, ε) 做 Eq.(2) 的扰动，分别得到判别 logits rϕ 与 qϕ，再用成对 relativistic logistic loss 优化。

四、长程一致性的上下文管理。推理侧采用静态前缀 sink（static prefix sink）+ 滑动 KV 窗口：前缀 sink 锚定外观与场景身份，滑窗把推理成本约束在有界范围，从而让 30 分钟级 rollout 在算力上可行。代价是滑出窗口的视觉细节最终会被驱逐（作者在 Conclusion 中自陈，并提出需要显式长期记忆机制）。

五、服务栈协同设计。为了让四步因果模型真正以 chunk 形式低延迟运行，作者设计了一个融合算子、KV 持久化（KV-persistent）、codec 流水线化（codec-pipelined）的 serving stack，支持分块执行。这是「模型-系统协同设计」的贡献，属于本文区别于纯模型论文的部分；但材料未给出具体延迟、吞吐或显存数字。

六、数据侧。约 200 小时、对齐的多粒度视频-代理语料，是整套方案的前提。片段显示其构造成分包括：为结构代理与外观参考标注角色并配样例 caption；自动校验检查 reference tags、时序顺序、entity-action 一致性；质量控制筛掉损坏或冻结帧、曝光失败、镜头切换（shot cuts）、重投影离群点；并做「质量控制与评测分离」以防泄漏（同一 split 隔离）。这些细节说明多粒度代理数据的主要噪声来源是重投影与镜头切换，而非单纯的视频质量。

Q4: 论文做了哪些实验？

一、评测目标与组织。实验围绕「因果流式渲染」展开，辅以多粒度创作、相机控制、长程流式的定性研究。章节组织为：6.1 held-out benchmark 与评测协议；6.2 四步因果渲染器 vs 代表性流式基线；6.3 跨代理族、结构粒度、相机路径与长 rollout 的控制展示。

二、Benchmark。held-out 测试集为 140 条序列，时长从 6.58 s 到 20 s，覆盖多样 3D 环境、多实体交互、多种代理表示族（proxy representation families），并同时包含单窗口片段（single-window clips）与多窗口流式 rollout 两种形态。这使评测既能看单片段画质，也能看跨窗口的连续性。

三、对照方法。所有方法在每条测试序列上接收完全相同的结构代理视频与外观参考图，保证条件信息对等。三个基线：SolarRenderer（Causal, 4-step）[12]、StreamRender-H3（2-step）[34]、Streaming CWM（4-step）[28]。被测系统为 ProForcing 训练后的 Invoker-Render（Causal, 4-step）checkpoint。需要注意步数并不统一：对手中包含 2 步方法，而本文为 4 步，因此「同等步数下的优劣」在本设置中并未被严格隔离（合理推断：作者可能以「同为 few-step 实时可用区间」为可比性依据，但材料未明说）。

四、评测协议。沿用 SolarRenderer [12] 的盲测成对偏好（blinded pairwise evaluation），由 Gemini 3.8 Flash 在四条准则上判定：Visual Quality、Structure & Motion、Ref. Preservation、Overall。每次查询向评判者呈现 ground-truth 与候选（材料在此处被截断，ground-truth 的具体使用方式需回原文确认）。Elo 分数以均值归一化到 1000，数值越高越好。

五、定性研究。包括：跨代理族与结构粒度的控制（粗/细代理同一通路）；相机路径控制（同一序列的广播视角 broadcast view 与 FPV 跟拍重放，说明给定相机轨迹改变视点而保持场景与主体外观）；长程因果流式（Figure 11 中两段赛车 rollout，在匹配的晚期时间戳 11 s–18 s 与 0 s 外观参考对照）；以及长达 30 分钟的赛车 rollout 稳定性展示。

六、数据与训练设置（可见部分）。约 200 小时对齐多粒度语料；细代理样本配对的 RGB 目标与动作文本用于教步态/姿态；质量控制与评测集严格分离以防泄漏。材料未给出训练算力、分辨率、帧率、chunk 大小、推理延迟等关键系统参数。

七、证据缺口（需回原文核对）：method 章节在本材料中为空，因此模型骨干细节（是否 DiT、VAE/ tokenizer 配置）、条件编码方式（代理如何 token 化或加噪）、ProForcing 各阶段的具体步数与数据量、消融实验（多粒度联合训练、ProForcing 各阶段、sink 与滑窗长度）均无原文支撑；本报告不对这些内容做数值化描述。

Q5: 发现了什么实验现象？

一、总体 Elo 格局（140 条 held-out 序列，均值归一化 1000，↑ 越高越好）：
1) Streaming CWM（4-step）：Visual Quality 653.0、Structure & Motion 626.7、Ref. Preservation 835.3、Overall 692.4。
2) StreamRender-H3（2-step）：935.7 / 1011.4 / 882.5 / 940.2。
3) SolarRenderer（Causal, 4-step）：Visual Quality 1174.9、Structure & Motion 1152.9（Ref. Preservation 与 Overall 在证据片段中被截断，缺数）。
4) Invoker-Render（Ours, Causal, 4-step）：完整四项数值在材料中未直接出现，但可由相对差反推其一：对 StreamRender-H3 的 Overall 领先 +291.8 Elo，即约 940.2 + 291.8 ≈ 1232.0。其余三项相对差未在可见证据中给出。

二、成对胜负结构。与 StreamRender-H3 的对比为 116 胜、4 平、20 负，合计 140 场，与测试集规模一致，胜率 82.9%。这说明增益不是少数极端样本拉动的均值假象，而是覆盖面较广的偏好优势（合理推断：仍可能存在系统性失败子集，材料未给出失败样本画像）。

三、值得注意的指标间张力。Streaming CWM 的 Ref. Preservation（835.3）明显高于其 Visual Quality（653.0）与 Structure & Motion（626.7）——即它可以「把参考身份守住」但画面与结构跟不上；反过来说，参考保持这一指标本身对画质与结构退化的惩罚不足，单看 Ref. Preservation 会误判系统可用性。这也解释了作者为何要以 Overall 为主指标并同时报告四项。类似地，StreamRender-H3 的 Structure & Motion（1011.4）高于其 Visual Quality（935.7），呈现「跟得住但画得不够好」的相反偏置。两种偏置方向不同，说明四准则之间存在可分离的失败模式，而不是单一质量轴。

四、跨粒度与跨代理族的定性现象。粗代理与细代理走同一条件通路、混合粒度联合训练后，模型在细几何被提供时遵循约束，在仅有粗代理时补全局部运动；论文用「刚性人体模型只约束全局位置与朝向、不冻结肢体姿态」作为该行为的具体体现。相机控制上，同一序列可在广播视角与 FPV 跟拍之间重放，视点改变而场景与主体外观保持。这些均为定性证据，缺少对应的定量指标（例如代理跟随误差、关节一致性度量）。

五、长程稳定性。30 分钟赛车 rollout 被报告为在场景与主体外观上保持稳定；Figure 11 在晚期时间戳（11 s–18 s）与 0 s 外观参考对比，用于显示长程不漂移。这是本文最有分量的定性主张之一，因为多数流式方法在数十秒量级就会出现身份漂移。但需注意：材料未提供长程的定量指标曲线（如随时间的 Elo 或身份相似度衰减），因此「稳定」的判据是作者陈述 + 图示，而非可复核的时序指标。

六、失败模式与边界（来自作者自陈）。静态前缀 sink 加滑动 KV 窗口虽能约束成本并维持外观，但滑出活动窗口的视觉细节最终会被驱逐——即在超长会话中，模型对「已离开局部上下文」的内容没有显式记忆，重新进入视野时可能无法还原。其次，当前训练在每个 rollout 窗口内使用固定代理粒度，因此流式过程中动态切换细节层级（mid-stream level-of-detail transition）尚不支持，这与「运行时选择输入细节」的卖点之间存在一个尚未闭合的缝隙（合理推断：运行时可在窗口边界切换，但不能在窗口内切换）。

七、反向/负结果方面：材料未报告任何消融的负结果或失效配置，因此「ProForcing 的两阶段拆分各自贡献多少」「多粒度联合训练是否损害细粒度忠实度」这类关键问题在本材料中无证据，属于必须回原文核对的空白。

Q6: 有什么可以进一步探索的点？

一、作者明确提出的方向。
1) 显式长期记忆机制。当前静态前缀 sink + 滑动 KV 窗口只能「维持」外观而不能「记住」被驱逐的细节，需要引入显式记忆（例如可检索的场景/主体记忆库、压缩的 episode 级状态、或分层缓存）来避免超长会话中的细节遗忘。这是本文最直接、也最有价值的开放问题。
2) 动态 mid-stream 粒度切换。当前训练在每个 rollout 窗口内固定代理粒度，因此「同一会话中从粗导航切到细交互」的控制流尚未实现；需要让模型在流式过程中接受级别变化且不产生跳变或身份漂移。
3) 与上游引擎的耦合（Conclusion 中提及“Coupling Invoker-Render…”但句子被截断）。合理推断：把渲染器与产生代理的模拟/引擎侧联合设计，让代理生成按需提升精度，形成闭环的 level-of-detail 协商。

二、评测与科学问题层面（以下为基于证据缺口的推断性建议）。
4) 去 MLLM-judge 依赖的客观指标。当前主结果是 Gemini 3.8 Flash 的成对偏好 Elo，缺少与代理的几何对齐误差、光流/重投影一致性、身份 embedding 相似度随时间曲线等客观度量；建立与人类偏好相关又能定位失败模式的指标是这一子领域的基础设施问题。
5) 长程时序曲线而非单点 Elo。把 Elo 或偏好胜率画成随 rollout 时间的函数（0–30 min），才能验证「不累积漂移」这一核心主张，并定位漂移起始点。
6) 系统指标。论文承诺实时流式并给出 serving stack，但材料未给延迟/吞吐/显存占用/首帧时延。补齐 chunk 级延迟分解（融合算子、KV 持久化、codec 流水线各自的贡献）会让系统贡献可复核。
7) 代理语义的可解释控制。把「哪些维度被约束、哪些被放开」显式建模（例如按通道/部位的置信度或 mask 条件），而不是依赖模型隐式学会「刚性人体模型不冻结肢体」。这可直接提升跨代理族的鲁棒性并减少对数据分布的依赖。
8) 多实体与交互密集场景的失败模式刻画。140 条序列包含多实体交互，但材料未报告按场景类型的细分结果；按实体数、遮挡率、相机速度分层统计会揭示能力边界。
9) 训练数据规模与粒度配比的研究。约 200 小时语料中粗/细代理的比例、以及多粒度联合训练对细粒度忠实度的影响未被消融，属于可直接开展的 scaling/配比实验。
10) 与物理/因果一致性的结合。当前目标是视觉可信而非物理正确；把上游引擎的接触、碰撞、刚体约束引入渲染监督，是向「可控世界模型」推进的自然一步。

三、跨领域迁移的开放点。
11) 该「代理条件 + 流式 few-step 渲染」框架可迁移到机器人遥操作的可视化、自动驾驶仿真回放、游戏引擎的神经渲染层；这些场景对延迟与几何忠实度的要求高于创作场景，会反向施压方法设计。
12) ProForcing 这种「先在数据锚定历史上蒸馏、再切到自生成 rollout」的课程，原则上可迁移到任何自回归少步生成任务（长音频、长文本流式解码、动作序列生成），其可迁移性本身值得验证。

Q7: 总结一下论文的主要内容

一、论文要解决的问题与立论方式。论文站在「解耦式视频世界模型」这一范式上：显式世界状态由上游引擎维护，视觉合成交给生成式渲染器，二者通过视觉/几何代理（相机路径、布局、实体运动、关节几何）与外观参考图衔接。作者认为这一范式同时服务两种使用场景——离线片段创作与实时交互式流式渲染——因此渲染器必须是一个「共享条件接口的双形态模型」。由此导出两个必须解决的挑战：其一，上游代理的粒度随交互类型剧烈变化，渲染器既要能严格遵循细几何，又要在粗代理下合理补全局部运动，且不能把约束误读为冻结（论文的人体模型例子）；其二，流式推理必须在极少的因果步数内完成（本文为四步），同时不能在自己生成的历史上累积漂移。作者进一步指出，把「步数压缩」与「rollout 漂移」放在同一个优化里会互相放大：干净训练视频上的 few-step 蒸馏与自生成历史下的推理分布不匹配。

二、技术主线。方案名为 Invoker-Render，底座是 MiniMax-H3 [42]，被适配成双向创作模型与因果四步流式渲染器两种形态，共享同一套以「结构代理 + 外观参考 + 语言」为输入的条件接口。多粒度能力不靠分支模型实现，而是让粗代理与细代理走同一条条件通路、在约 200 小时对齐的多粒度视频-代理语料上联合训练，从而支持运行时选择输入细节层级；细代理样本还配有 RGB 目标与动作文本，用于教模型合成与轨迹一致的自然步态与姿态。核心训练方法是 Progressive Forcing（ProForcing）：先训练因果预测器完成双向→因果适配；再用训练视频导出的、数据锚定的历史把它蒸馏成四步渲染器（DMD 与 GAN 式对抗蒸馏并用，对抗部分采用成对 relativistic logistic loss，在相同 (t, ε) 扰动下比较真实窗口与生成窗口的判别 logits）；最后把训练切换到连续的自生成 rollout 上，学习有状态的延续。这样「因果适配 + 四步压缩」发生在可控历史分布上，「漂移抑制」发生在真实推理分布上，二者解耦后再耦合。系统工程侧配套一个融合算子、KV 持久化、codec 流水线化的 serving stack，支撑分块低延迟执行；长会话用静态前缀 sink 加滑动 KV 窗口约束推理成本并锚定外观。

三、数据与质量控制。语料约 200 小时、多粒度且视频与代理对齐；标注侧为结构代理与外观参考标注角色并配样例 caption，自动校验检查 reference tags、时序顺序与 entity-action 一致性；质量控制筛除损坏或冻结帧、曝光失败、镜头切换、重投影离群点；训练/评测严格分离以防泄漏。这暗示该数据构建的主要难点在于代理的重投影精度与镜头连续性，而非单纯视频清晰度。

四、实验主线与结论。评测在 140 条 held-out 序列上进行（6.58 s–20 s，多样 3D 环境、多实体交互、多代理族，涵盖单窗口片段与多窗口流式 rollout），所有方法接收相同的代理视频与外观参考图。对照三个代表性 few-step 因果/流式代理渲染器：SolarRenderer（Causal, 4-step）、StreamRender-H3（2-step）、Streaming CWM（4-step）。指标为沿用 SolarRenderer 协议的 Gemini 3.8 Flash 盲测成对偏好，按 Visual Quality、Structure & Motion、Ref. Preservation、Overall 四准则折算 Elo（均值归一化 1000）。可见数值：Streaming CWM 为 653.0/626.7/835.3/692.4，StreamRender-H3 为 935.7/1011.4/882.5/940.2，SolarRenderer 至少取得 Visual Quality 1174.9 与 Structure & Motion 1152.9（后两项数值在证据中缺失）；本文相对 StreamRender-H3 为 116 胜 4 平 20 负（82.9% 胜率，Overall +291.8 Elo，据此推算 Overall 约 1232）。定性方面展示了跨代理族与跨粒度的控制、广播视角与 FPV 跟拍的相机轨迹重放、以及在匹配晚期时间戳（11 s–18 s）与 0 s 外观参考的一致性对照，并报告 30 分钟赛车 rollout 中场景与主体外观保持稳定。

五、作者的自我限定。结论承认滑出活动窗口的视觉细节终将被驱逐，需要显式长期记忆；并承认当前训练在单个 rollout 窗口内使用固定代理粒度，尚不支持流式中途切换级别细节，需要进一步与上游引擎耦合。此外，本材料缺失 method 章节与全部消融数据，因此「ProForcing 各阶段贡献」「多粒度联合训练的影响」「系统延迟指标」等关键结论在本篇可获取证据中不可复核，需回原文确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的「generation」方向直接重合：这是一篇视频生成/世界模型系统论文，涉及扩散少步蒸馏、自回归流式生成、长程一致性三类可迁移技术。

## 基本信息

- 作者：Ruoyu Wang, Hao Zhong, Yanming Yang, Jiahao Tian, Lin Liu, Biao Gong, Guoshan Lu, Yiding Tian, Chunhua Shen, Zhenzhong Lan, Chi Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-11-17
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.22925v2`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先采用了 PDF 语义检索命中的片段（Introduction/Method/Results/Conclusion 锚点，含 Elo 表与 ProForcing 的对抗损失细节）并结合摘要与结论补全；同时需警示两点证据问题：所给标题（Code World Model 2.0）与正文统一使用的系统名（Invoker-Render）不一致，且 method 章节正文为空、关键系统与消融数据缺失，相关字段已就地标注为缺口而非推测填充。
