---
user_id: "cheng tan"
paper_id: 14907
arxiv_id: "2610.12427v1"
title: "FastBench: Can Streaming VLMs Perceive High-Dynamic Real-World Streams?"
publish_date: "2026-10-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.12427v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.12427v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T12:46:40"
---
# FastBench: Can Streaming VLMs Perceive High-Dynamic Real-World Streams?

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：streaming video understanding · high-dynamic perception · video large language model · active frame rate control

## 一句话总结

FastBench 是一个专门评估流式视频大模型在真实世界高动态视频流中感知能力的基准，配套提出训练无关的 ProactiveFrame 主动采样基线，实验显示最强模型仅达 50.7%，高动态感知仍远未解决。

## 摘要

> Streaming Video Large Language Models (VLMs) enable continuous video understanding, yet existing benchmarks focus on low-dynamic scenarios. Under bounded context budgets, models must balance temporal history, spatial resolution, and temporal granularity; sparse sampling at 1--2 FPS misses fast events. We introduce FastBench to evaluate high-dynamic perception in real-world video streams. Its trajectory-grounded pipeline combines QA generation from high-FPS clips, filtering of questions answerable at 2 FPS, answer verification using SAM3 and CoTracker3 trajectories, and three rounds of human inspection. FastBench contains 306 QA pairs across eight domains, six capabilities, and forward, instant, and backward temporal scopes, with human-annotated evidence intervals. We also present ProactiveFrame, a training-free baseline that adjusts incoming frame rates through text tokens. A dual-tier sliding window retains recent high-FPS observations while downsampling older ones into sparse history. Experiments reveal substantial limitations: the strongest model, Gemini-3.5-Flash, scores only 50.7%. Denser sampling improves Qwen3-VL-8B from 32.9% at 2 FPS to 44.6% at 24 FPS, but gains saturate as history is compressed. ProactiveFrame outperforms sparse uniform sampling by 5.4 and 1.5 percentage points, yet remains well below oracle-guided focusing, showing that current VLMs struggle to determine from the stream alone when finer temporal perception is needed. FastBench provides a testbed for high-dynamic streaming video understanding. Code and data: https://github.com/Ashone3/FastBench.

Q1: 这篇论文试图解决什么问题？

1. 核心缺口：现有流式视频理解评估基准大多建立在低动态场景上（如日常活动、慢速对话），在低采样率下仍可回答大部分问题；但真实世界中许多关键事件发生在毫秒到几百毫秒尺度，例如火花、气球破裂、高速运动物体、物体瞬时接触等，稀疏采样（1–2 FPS）会直接错过这些关键帧，导致模型给出泛泛的、缺乏证据的回答。
2. 上下文预算的固有矛盾：流式 VLM 在处理无限视频流时必须把有限的上下文预算分配给三方面——时间历史长度、空间分辨率、时间采样密度。提高帧率会增加近期视觉 token 数量，往往不得不压缩历史或降低分辨率，这会损害对长程时间关系的理解。现有工作大多隐式选择均匀低帧率，没有考虑事件动态性。
3. 缺少对'何时需要高帧率'的评测：即使模型有能力处理高帧率，它也需要感知到'现在正在发生值得细看的事情'。这个主动感知决策（proactive perception）是流式理解的核心，但几乎没有基准能测量它。FastBench 试图显式评估模型能否从流本身推断哪些时刻需要更密集观察。
4. 时间范围的复杂性：真实流事件有前向（预测即将发生）、即时（当前瞬间）、后向（回忆刚发生）三种推理需求，现有基准多未做区分。FastBench 将三种时间范围都纳入，并在能力维度上覆盖因果推理、时空跟踪、实体状态变化、在线检测等。
5. 评估质量问题：开放问答式评估容易被'听起来合理但内容笼统'的答案骗过（论文 Figure 1 中 Gemini 和 Doubao 都给出了通用的电学解释，并未真正看到木条抽走后金属棒接触产生火花的细节）。因此 FastBench 要求答案必须能由轨迹证据核实，并排除在低帧率下就可回答的问题，以提高动态感知评估的针对性。

Q2: 有哪些相关研究？

1. 视频理解 VLM 的发展：早期工作如 VideoChat（Li et al., 2025a）和 LLaVA-NeXT-Video（Zhang et al., 2024）展示了在时间受限、离线视频片段上的强大能力。这些方法通常一次接收完整短视频，可密集抽取帧，不面临无限流的在线决策问题。
2. 流式视频 VLM：以 StreamingVLM（Xu et al., 2026）为代表，范式转向处理连续到达的视觉流，需要实时维护长期上下文。这类模型在架构上往往采用按块（chunk）追加视觉输入的方式，并配合记忆管理。
3. 流式评估基准：已有的流式理解基准多集中于低动态、慢变化场景，对高动态事件的敏感度不足。PhoStream（Lu et al., 2026b）将评估扩展到移动场景的开放问答，强调音频-视觉证据和响应时间，但仍未系统针对高动态、需要高 FPS 的事件。FastBench 在此基础上引入轨迹接地、伪动态过滤和时间范围分类，填补高动态感知评测空白。
4. 主动感知/主动采样方法：一些方法（如表格中的 VideoChat3，4B）被归类为主动感知方法，试图根据内容决定采样策略，但在 FastBench 上表现极端：在即时和后向时间范围上得分为 0，说明这类方法可能只处理前向预测而缺乏追溯能力。ProactiveFrame 提供了一种轻量、无需训练的主动帧率调节基线，通过文本 token 指导未来采样密度。
5. 视频目标分割与跟踪工具：FastBench 使用 SAM3（分割）和 CoTracker3（轨迹跟踪）生成客观轨迹证据，用于验证答案是否成立。这与视频物体分割、运动轨迹分析等经典视觉任务相关，也借用了这些工具来构建可靠基准。
6. 评估协议：论文采用 LLM-as-a-Judge 评估开放式答案，并规定早期/晚期响应判 0 分（推测与响应时间窗口有关），这延续了近期 VLM 评估中'模型裁判+时间约束'的做法，也与 PhoStream 的响应时间指标相关。

Q3: 论文如何解决这个问题？

1. FastBench 基准构建（轨迹接地管道）：
 - 数据收集：使用 LLM（Gemini-3.1-Pro）提出搜索关键词，从真实世界采集高动态源视频（覆盖 8 个领域）。
 - 高 FPS 片段截取：从源视频中截取快速事件片段，确保包含毫秒级关键变化。
 - QA 生成：基于高 FPS 片段生成问答对，问题针对只有高帧率才能观察到的细节（例如火花产生瞬间、物体接触顺序、屏障移除后的因果链条）。
 - 伪动态过滤：自动筛选掉在 2 FPS 采样下就能回答的问题，确保剩余问题真正需要高时间分辨率。
 - 答案验证：使用 SAM3 和 CoTracker3 提取对象分割与运动轨迹，验证模型生成的答案是否与轨迹证据一致；只有轨迹支持的回答才保留。
 - 三轮人工检查：邀请人类专家对每个样本进行多轮审核，最终确认所有发布的样本。
 - 标注证据区间：人工标注每个问题对应的视频证据时间段，形成 focus timestamps，用于后续研究主动聚焦。
 - 最终规模：306 个 QA 对，覆盖 8 个领域、6 种能力、三种时间范围（forward/instant/backward）。

2. ProactiveFrame 基线（无训练）：
 - 核心思想：让流式 VLM 在每轮输入时额外接收一段文本 token（例如'Focus_Start'/'Focus_End'或其他指示），模型通过这些文本 token 输出下一时刻所需的帧率等级。
 - 双层级滑动窗口：设置两个缓存层——近期层（recent high-FPS observations）保留最近 1 秒左右的高帧率帧；历史层（sparse history）将更早的帧降采样成稀疏表示并继续保留。这样在上下文预算固定时，既能保留刚发生的细节，又能维持足够长的历史。
 - 聚焦规则：使用低阈值启动聚焦（当不确定高帧率是否有帮助时，优先 Focus_Start，允许不必要聚焦），以保证不漏掉关键瞬间。
 - 与模型配合：ProactiveFrame 可挂在任意流式 VLM 上，如 Qwen3-VL-8B 或 Gemini-3.5-Flash，输出仍由基模型产生，只是采样决策由文本 token 控制。

3. 评估协议：
 - 开放式答案用 LLM-as-a-Judge 打分；早期和晚期响应直接得 0 分（推测用于约束响应时间与事件发生时间的关系）。
 - 除整体准确率外，还按能力类别和时间范围（Fwd./Inst./Back.）分解分数。
 - 通过上下文配方实验（Full 30 FPS、Mixed FPS/Focus oracle 等）研究模型如何使用时间上下文。

Q4: 论文做了哪些实验？

1. 模型集合：
 - 闭源模型：Doubao-Seed-2.0-Lite、GPT-5.6-Sol、Gemini-3.5-Flash。
 - 开源模型：Aura (8B)、Qwen3-VL (8B)、MOSS-VL-Realtime (11B)。
 - 主动感知方法：VideoChat3 (4B)、ProactiveFrame（分别基于 Qwen3-VL 8B 和 Gemini-3.5-Flash）。

2. 能力维度：论文将 306 个 QA 对按能力分组，从表格可见包括 Physical Interaction（物理交互）、Causal Reasoning（因果推理）、Spatiotemporal Tracking（时空跟踪），以及 Visual Perception（视觉感知）、Entity State（实体状态）、Temporal Dynamics（时间动态）、Streaming Online Detection（流式在线检测）等。每个能力都进一步按前向（Fwd.）、即时（Inst.）、后向（Back.）时间范围报告分数。

3. 采样率比较实验：作者将 Qwen3-VL-8B 的采样密度从 2 FPS 提升到 24 FPS，观察性能变化：32.9%（2 FPS）→ 44.6%（24 FPS），并展示在历史压缩下增益饱和。

4. 上下文配方（Formulation）实验：Table 2 比较三种上下文配方：
 - Full 30 FPS：按流的原生帧率处理全部输入；
 - Mixed FPS (Focus)：oracle 引导的聚焦，在关键时间段提供 30 FPS，其他时间稀疏；
 - 其他变体（推测包括 Sparse uniform、ProactiveFrame 等）。用于分析模型如何利用时间上下文。

5. 基线对比：ProactiveFrame 相对于均匀稀疏采样在两种基模型上分别提升 5.4 和 1.5 个百分点；但均明显低于 oracle 聚焦的表现，说明自动聚焦决策是当前瓶颈。

6. 评估细节：开放答案由 LLM-as-a-Judge 自动打分；时间上过早或过晚的回答判 0 分（推测要求回答在事件前后的合理窗口内）。

Q5: 发现了什么实验现象？

1. 高动态感知远未解决：最强模型 Gemini-3.5-Flash 在 FastBench 上仅达 50.7%，说明即使领先模型也会错过大量关键瞬间；闭源模型总体优于开源模型，但差距并不悬殊。
2. 密集采样有效但有上限：Qwen3-VL-8B 从 2 FPS 的 32.9% 提升到 24 FPS 的 44.6%，但继续增加帧率并不线性提升——随着历史被压缩，收益趋于饱和，表现出的正是粒度-历史权衡（granularity–history trade-off）。
3. 主动感知比固定高帧率更好但仍不足：ProactiveFrame 相比均匀稀疏采样提升 5.4 / 1.5 个百分点，说明自适应采样有真实收益；但距离 oracle 引导的聚焦（Mixed FPS / Focus）仍有显著差距，说明模型尚不能可靠判断'何时需要更细时间感知'。
4. 能力维度上的严重分化：
 - 不少模型在 Physical Interaction、Causal Reasoning 上表现尚可，但在 Spatiotemporal Tracking 和 Streaming Online Detection 上明显较差，例如 MOSS-VL-Realtime 在 Streaming Online Detection 的 Back. 上只有 2.9，在多个 Back. 项上为 0。
 - VideoChat3（主动感知方法）虽然在前向任务上有一定分数（如 34.2、36.1），但在 Instant 和 Backward 全部为 0，揭示这类主动感知模型只会向前看、无法回忆或感知瞬时细节。
 - 不同时间范围表现不稳定：很多模型在 Backward 任务上明显弱于 Forward，例如 Qwen3-VL 的 Visual Perception 上 Fwd. 30.0 vs Back. 13.3；说明模型对'刚过去的事件'的短时记忆和推理存在短板。
5. 典型案例：Figure 1 中 Gemini-3.5-Flash 和 Doubao-Seed-Realtime 对'为什么刚才有爆炸火花'都给出了通用的电气解释（短路放电、电容反接），没有捕获'木条被抽走→金属棒接触→短路'这一具体因果链，属于典型的'看见了但没看见'的感知失败。
6. 某些模型在某些能力上分数为 0，可能是评估严格性的体现（early/late response 判 0 分），也可能是模型根本没有输出可判分答案；这提示现有流式模型在在线场景下的响应时机控制同样不足。

Q6: 有什么可以进一步探索的点？

1. 主动时间控制（proactive temporal control）：论文明确指出'决定何时看更细'是核心开放挑战。未来可用 FastBench 提供的人类标注的 focus timestamps 作为监督信号，训练模型学习何时切换高/低帧率。
2. 强化学习或可微采样策略：将帧率选择建模为序贯决策，利用 RL 优化'在有限上下文预算下最大化下游任务准确率'的目标，可能比纯文本 token 指令更有效。
3. 历史压缩算法：实验显示密集采样的收益会被历史压缩抵消；研究更好的长期历史保留（如基于重要事件的记忆、分层 token 压缩、借助外部矢量记忆）可以突破粒度-历史权衡。
4. 多模态线索融合：真实高动态事件常伴随音频（如'啪'声）或文本（弹幕、评论），FastBench 当前以视觉为主；结合音频/语言线索可能帮助模型预测焦点时刻。
5. 更丰富的场景与规模扩展：将 FastBench 扩展到更多领域、更长流、多事件流，并增加对抗性干扰事件（伪动态背景），提高基准难度和生态代表性。
6. 评估协议完善：进一步研究 LLM-as-a-Judge 与轨迹验证的结合，探索自动化和人工检查的成本平衡；设计可复现的在线响应时间指标，让 early/late 判定更加精细。
7. 下游应用驱动：将主动感知能力用到具身智能、自动驾驶、体育赛事分析、机器人操作监控等实时决策场景，验证 FastBench 分数与实际系统性能的一致性。
8. 与空间分辨率联合优化：论文聚焦时间粒度，但空间分辨率也是预算的一部分；未来可探索时空联合的主动采样（在关键时刻提高帧率和分辨率，在其他时刻降低）。

Q7: 总结一下论文的主要内容

FastBench 是首个专门针对高动态真实世界视频流、评估流式 VLM 感知能力的基准，并配套提出 ProactiveFrame 这一无训练主动采样基线。论文从三个层次构建了完整研究：

首先，动机上，作者指出现有流式 VLM 基准大多基于低动态场景，在 1–2 FPS 稀疏采样下即可完成大部分问答，因此无法暴露模型对快速事件的感知缺陷。而真实世界应用（如监控、体育、机械过程）中，关键信息往往在几百毫秒内发生，稀疏采样会直接漏掉。这引出流式模型在固定上下文预算下的核心权衡：时间历史、空间分辨率、时间采样密度三者不可兼得。

其次，方法上，FastBench 采用轨迹接地（trajectory-grounded）管道来保证问题真正'动态'：从高 FPS 片段生成问题和答案，自动过滤在 2 FPS 下就能回答的伪动态问题；用 SAM3 和 CoTracker3 提取对象分割与运动轨迹，验证答案是否可以被视觉证据支持；再经过三轮人工检查，并标注每个问题对应的证据时间区间。最终得到 306 个 QA 对，覆盖 8 个领域、6 种能力、3 种时间范围。同时，ProactiveFrame 通过让模型输出文本 token 来控制后续帧率，并采用双层滑动窗口：近期观测保持高 FPS，较早历史降采样为稀疏表示，以固定上下文预算。

然后，实验上，作者在多个闭源/开源模型和主动感知方法上进行评测。结果显示：最强模型 Gemini-3.5-Flash 仅 50.7%；Qwen3-VL 从 2 FPS 的 32.9% 提到 24 FPS 的 44.6%，但收益随历史压缩而饱和；ProactiveFrame 比均匀稀疏采样高 5.4 / 1.5 个百分点，但仍低于 oracle 引导聚焦。按能力和时间范围分解的分数进一步显示：许多模型在前向任务尚可、但即时/后向任务很差；VideoChat3 这类主动感知方法完全没有即时和后向能力；模型在时空跟踪、在线检测等需要精细运动分析的能力上普遍薄弱。案例中，强模型对'爆炸火花'只给出通用解释，说明没有真正看到关键因果链。

最后，作者总结：高动态真实世界流的感知仍远未解决，核心挑战是让模型学会'何时需要看得更细'；FastBench 及其人工标注的 focus timestamps 希望推动主动时间控制的研究。论文还包含 AI 使用声明，说明用 Gemini-3.1-Pro 辅助构造标注，但最终由人类专家决策。整体上，FastBench 提供了严谨的基准构建流程（轨迹验证+人工审核）、丰富的多维拆解（能力×时间范围×采样策略），以及一个轻量基线，为流式视频理解的前沿提供了清晰的测量尺度和开放问题。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与生成方向（权重 0.10）直接相关：FastBench 评测的是视频生成/理解模型的感知极限，ProactiveFrame 的帧率控制也可作为可控生成的时序采样启发。

## 基本信息

- 作者：Yuxuan Hu, Weikang Shi, Yang Bo, Xudong Lu, Xintong Guo, Shuhan Li, Yuyang He, Huankang Guan, Peiwen Sun, Yunqiao Yang, Wenbo Li, Rui Liu, Hongsheng Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-08
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.12427v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要、元数据和 PDF 语义检索命中的证据片段，并结合字段证据映射对内容进行了归纳与补全。
