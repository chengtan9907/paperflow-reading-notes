---
user_id: "cheng tan"
paper_id: 18347
arxiv_id: "2610.35229"
title: "Joint Latent Reasoning and Observation for Agentic Video Understanding"
publish_date: "2026-10-27"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.35229.pdf"
pdf_url: "https://arxiv.org/pdf/2610.35229.pdf"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-27T13:12:06"
---
# Joint Latent Reasoning and Observation for Agentic Video Understanding

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic video understanding · latent reasoning · video agents · knowledge distillation

## 一句话总结

论文提出 LARVA，用可联合训练的连续 latent 观察接口替代文本描述，把教师多轮视频 agent 的推理与采集行为蒸馏到 4B/7B 小模型，使观察表征同时服务于答题与后续“看哪里”的决策。

## 摘要

> 视频 agent 不仅要回答，还要决定下一步看哪里。将这种能力迁移到小模型困难在于：每次观察既支撑当前答案，又指导后续证据采集；文本描述能说明“看到了什么”，却可能漏掉“下一步该找什么”的线索。LARVA 让 observer 把选定帧编码为紧凑连续表示，直接传给 reasoner，由后者选择工具调用或输出答案；action 与 answer 监督穿过该接口，训练 observer 传递对下游决策有用的信息。方法还利用文本教师轨迹做 observation reconstruction 与 reasoning-distribution alignment 作为辅助监督。作者在五个视频理解基准上评估 4B/7B 主干，报告 7B 模型在 VideoHolmes、Video-MME、LVBench 上分别比同教师轨迹训练的文本 CoT 学生高 5.1、2.5、3.7 个百分点，并强调联合学习“如何表示证据”与“如何基于证据行动”的价值。

Q1: 这篇论文试图解决什么问题？

1. 任务背景：长视频理解中只有少部分帧与问题相关，近年的 agentic video 系统因此把“全量编码”改成“迭代采集”：模型先观察一段视频，形成推理，决定调用什么工具、看哪个时间区间，再把新观察纳入上下文，直到给出答案。论文要解决的不是单轮视频问答，而是这种多轮“看—想—再去看”的能力如何蒸馏到较小模型。

2. 核心矛盾：每次 observation 有双重用途。它既要为当前答案提供证据，又要为下一步采集提供线索。文本描述通常擅长回答“当前画面里有什么”，却未必保留“下一步该看哪里”的信息，例如时间定位、人物/物体的持续状态、不确定性、被遮挡的线索、与问题相关的异常点。论文明确把这一点作为缺口：文本接口可能捕捉可见内容，但省略后续采集所需的 cues。

3. 蒸馏接口错配：教师轨迹包含 action 和 answer，但学生若用文本 CoT 作为中间接口，observer 被训练成“写描述”，而不是“写对下游决策有用的表示”。离散文本既不可微，也难以让最终答案和动作的监督信号直接回传到观察编码器；observer 与 reasoner 的优化目标因此可能不一致。论文的判断是，关键不只是“用教师轨迹做 SFT”，而是要让观察接口本身参与联合优化。

4. 多轮信用分配：早期观察的价值往往延迟体现——它可能改变后续 tool call，进而改变最终答案。若只监督最终答案，observer 难以知道哪些细节真正影响了决策；若只模仿教师动作，又可能学到教师的表面行为模式而非可迁移的证据表征。LARVA 让 action 与 answer 监督穿过 latent interface，正是对这类延迟信用分配的一种工程化回应（合理推断）。

5. 效率与部署约束：逐帧编码长视频代价高，小模型上下文更有限；连续 latent tokens 既是信息瓶颈，也是降本手段。论文主配置每个推理/观察块用 8 个 latent tokens、最多 8 轮采集，说明其目标是在“足够表达”和“可部署”之间取平衡。

6. 隐含假设与边界：方法假设紧凑 latent 能同时编码“答题证据”和“搜索线索”；假设教师轨迹质量足够高；假设冻结的文本 CoT 学生能提供有用的分布对齐信号；假设共享主干加独立 LoRA 足以分离 reasoner/observer 角色。这些假设在论文片段中未被逐一验证。边界还包括：只处理采样帧、不处理音频；最多 8 轮可能限制需要长链搜索的问题；教师轨迹错误会传播；训练时按教师动作回放、推理时学生自选动作，存在分布偏移（合理推断）。

Q2: 有哪些相关研究？

1. Agentic video understanding：论文把近年工作归入“让模型迭代决定看什么”的范式。VideoSeek 采用 think–act–observe 循环和多粒度观察工具；EVA 用单个 MLLM 通过监督与强化学习规划看哪里、取帧并反思；LensWalk、VideoSeek 属于 reasoner–observer 设计，由语言模型 reasoner 选择时间范围和采样策略，视觉语言 observer 返回证据；LongVideoAgent 协调 grounding 与 vision agents；VideoExplorer 在多模态工具集之上放一个纯文本推理模型；VideoARM 用层次化记忆累积工具输出。论文还指出许多系统是 training-free 的。这些系统的共同点是：推理与感知通过显式接口通信，中间推理以文本生成，模块化 observer 常把视觉证据转成文本描述。

2. 显式文本接口的利弊：文本接口可解释、易组合、便于人工检查，但会成为信息瓶颈。对于“下一步看哪里”，关键信息可能不是一段可读 caption，而是时间位置、视觉不确定性、对象持续性和与查询相关的显著性。论文的出发点正是：文本描述可能“说对了当前画面”，却“说漏了搜索线索”。

3. Latent reasoning / 连续 CoT：一条相邻脉络是用连续隐状态替代显式文本推理，以减少离散化损失和上下文开销。LARVA 的差异在于它不是只压缩推理链，而是把 observation 也变成 latent，并让 action/answer 监督穿过这个观察接口，联合训练“如何表示证据”和“如何行动”。论文片段没有展开与所有 latent reasoning 工作的逐项对比，完整谱系需回原文核对。

4. 蒸馏与行为克隆：相关做法是从教师 agent 轨迹中学习动作和答案，常见目标是 action supervision、answer supervision、过程对齐或分布对齐。LARVA 同时使用教师动作预测、最终答案监督、observation reconstruction 和 CoT-distribution alignment；其中 reconstruction 防止 latent 丢失可描述信息，distribution alignment 用冻结文本 CoT 学生约束推理分布。它与纯文本 CoT 蒸馏的关键区别是把监督传到 observer，而不是只训练 reasoner 的文本输出。

5. 多模态 token 压缩与信息瓶颈：把帧压成少量 latent tokens 与视觉 token 压缩、连续记忆、跨模态通信等研究方向相邻。LARVA 的工程选择是每个观察块 8 个 tokens，并以时间戳配对；这既是效率设计，也是信息瓶颈设计。

6. 长视频基准：论文在 VideoHolmes、Video-MME、LVBench、LongVideoBench、MLVU 上评估，并明确不使用 Video-MME 的 captions 作为模型输入。这些基准覆盖长视频问答与证据定位，但论文片段没有给出各基准的完整 leaderboard 与 baseline 列表，需回原文确认比较范围。

7. 研究缺口：既有工作多把“观察”转成文本再交给 reasoner，或只训练统一 MLLM 的文本推理；较少工作把 observer 的连续表示与 reasoner 的多轮决策联合优化。LARVA 试图填补的正是这一接口层缺口。

Q3: 论文如何解决这个问题？

1. 形式化交互：给定视频 V 和问题 Q，agent 在推理、采集、观察之间交替。记第 t 步之前的交互上下文为 H_{t-1}；reasoner 先产生一个 latent reasoning block Z_t，再决定输出工具调用 A_t 或终止答案。非终止动作指定工具类型、适用时的时间区间、可选查询；执行后从 V 中选出帧 X_t，observer 为每帧输出一组 latent observation tokens O_{i,t}，并与该帧时间戳一起进入上下文。前一轮的 Z_t 也会保留到下一轮决策。用 T 表示非终止采集轮数，终止答案在第 T+1 步，条件于包含所有已完成观察的 H_T。

2. Reasoner：以共享视觉语言主干为基础，加独立 LoRA adapter，负责 latent reasoning 和动作/答案生成。训练时 reasoner 预测教师动作；执行教师动作得到的帧再喂给 observer 和下一轮决策。推理时 reasoner 自己选动作，不再依赖教师。这个“训练用教师动作、推理用自身动作”的安排是典型的 teacher forcing / replay 设计，附录 A.3 应有细节。

3. Observer：不输出 caption，而是把选定帧编码成紧凑连续表示。主配置每个推理块和观察块各 8 个 latent tokens；观察 token 与时间戳配对，直接条件化后续推理。这样做的目的不是“更短的文本”，而是让 observer 学习一种面向下游决策的通信码。

4. 训练目标：论文明确列出四类信号。其一是 action supervision：在每一轮预测教师动作。其二是 answer supervision：对最终答案做监督。其三是 observation reconstruction：从 latent observation tokens 重建文本观察/描述，作为辅助监督，防止连续表示丢掉可验证的视觉信息。其四是 CoT-distribution alignment：用一个冻结的文本 CoT 学生做分布对齐，让 latent reasoning 不偏离文本推理所承载的语义。论文正文片段只给出目标名称，没有给出各损失的具体公式、权重、对齐所用散度类型和温度，需回原文与附录确认。

5. 联合优化：关键不是分别训练 reasoner 和 observer，而是让 action 与 answer 的监督穿过 latent interface，回传到 observer。这样 observer 被激励去传递“对下一步动作和最终答案都有用”的信息，而不是仅仅生成人类可读的描述。reconstruction 与 distribution alignment 则提供来自文本教师轨迹的辅助正则，降低纯任务监督在小数据上的崩塌风险。

6. 数据与实现：训练集由 20,000 条交互轨迹构成，来自 Video-Marathon、VideoHolmes 训练集和 LongVideo-Reason；过滤掉最终答案错误或工具调用无效的轨迹。教师 reasoner 为 DeepSeek-V4.1-Flash，教师 observer 为 Qwen3.8-27B。学生用 Qwen2.5-VL-7B 与 Qwen3-VL-4B，共享主干、分离 reasoner/observer LoRA。主配置最多 8 轮采集，工具与优化细节在附录 A.5。

7. 设计权衡：连续接口减少文本瓶颈和上下文开销，但牺牲可读性；固定 8 个 latent tokens 提供强压缩，但可能限制细粒度证据；共享主干提高参数效率，却可能带来角色表征干扰；教师动作回放保证训练稳定，却造成训练/推理分布偏移。这些权衡是理解方法边界的关键。

Q4: 论文做了哪些实验？

1. 实验目标：论文把 LARVA 当作完整的 observation–reasoning–action 闭环来评估。作者先问：在相同教师轨迹下，LARVA 是否比文本 CoT 学生更能提升视频理解？再问：latent reasoning、latent observation、联合优化各自贡献多少？最后评估推理成本，以及采集到的证据是否真的有用。结果部分还设计了 answerer swaps、人类对增量证据的评估、与教师轨迹的对比，作为采集行为的补充视角。

2. 训练数据：20,000 条交互轨迹，来自 Video-Marathon、VideoHolmes 训练集和 LongVideo-Reason；过滤最终答案错误或工具调用无效的轨迹。教师 reasoner 为 DeepSeek-V4.1-Flash，教师 observer 为 Qwen3.8-27B。

3. 学生与实现：Qwen2.5-VL-7B 和 Qwen3-VL-4B 两个主干；共享 backbone、分离 reasoner/observer LoRA adapter；主配置每个推理/观察块 8 个 latent tokens，最多 8 轮采集；初始化、优化设置和工具见附录 A.5。这个设置让“同主干、同数据、不同接口”的比较相对干净。

4. 基准：VideoHolmes、Video-MME、LVBench、LongVideoBench、MLVU 五个视频理解基准；Video-MME 的 captions 不作为模型输入。评价指标以 accuracy 为主（合理推断，片段只出现 accuracy 一词）。论文结论片段称“三个基准”，摘要称“五个基准”，可能主对比在三个基准、附加评估在另外两个，需回原文核对。

5. 主要比较：与“在相同教师轨迹上训练的文本 CoT 学生”比较，这是最关键的受控 baseline；此外还与现有方法比较（结论称 outperforms existing methods，但片段未列出完整 baseline 表）。摘要报告 7B 模型在 VideoHolmes、Video-MME、LVBench 上分别比文本 CoT 学生高 5.1、2.5、3.7 个百分点；4B 模型也报告 strong performance，但具体数值未在提供片段中出现。

6. 消融：结果部分明确要考察 latent reasoning、latent observation、joint optimization 的贡献。按常规设计，消融可能包括去掉 latent reasoning、把 latent observation 换回文本、分别训练而非联合训练等；但片段没有给出具体变体定义和逐项数值，不能编造趋势。可以确认的是，论文把“联合优化”本身作为独立因素，而不仅是“用不用 latent”。

7. 效率评估：结论称 LARVA 在三个基准上优于现有方法并降低推理延迟；结果部分也明确评估 inference cost。但片段没有给出延迟、token 数、FLOPs 或吞吐的具体数字，需回原文确认效率提升的来源（更少文本 token、更少上下文、还是更少工具调用）。

8. 证据行为分析：answerer swaps 用来检验采集到的证据是否独立于原 answerer 也有用；人类评估增量证据，检验每一步观察是否真的带来新信息；教师轨迹对比检验学生的采集行为是否更接近教师。这三类分析补足了单纯 accuracy 不能回答的“证据质量”问题。

9. 公平性注意点：文本 CoT 学生与 LARVA 使用相同教师轨迹，这是强对照；但两者参数量、接口、训练目标不同，不能把全部增益归因于 latent 接口。还需看文本 CoT 学生是否经过同等调参、是否共享相同工具预算、是否同样最多 8 轮。片段不足以判断这些细节。

Q5: 发现了什么实验现象？

1. 主现象：在相同教师轨迹下，连续 latent 接口的 7B 学生优于文本 CoT 学生，说明“把观察写成 caption”不是无损的蒸馏方式。增益在三个基准上分别为 5.1、2.5、3.7 个百分点，幅度不算均匀；VideoHolmes 上最大，Video-MME 上最小。合理推断是：越是依赖多步证据采集、越需要“下一步看哪里”的任务，latent 接口越占优；而单帧可答或文本先验较强的题目，接口差异会缩小。这个解释需要看逐题分析和基准构成才能确认。

2. 反直觉点：论文的核心主张不是“latent 比文本更保真”，而是“文本可能把对当前描述有用、但对后续采集无用的信息写得很充分，同时漏掉对搜索有用的线索”。这意味着 caption 的“可读性好”不等于“决策信息好”。如果消融显示 latent observation 单独去掉后掉点明显，而只去掉 latent reasoning 掉点较小，就能支持“观察接口是主要瓶颈”；片段未给出该排序，不能下结论。

3. 消融逻辑：latent reasoning、latent observation、joint optimization 三者被分开考察。可以预期的趋势是：只把 observation 变 latent 但不联合训练，observer 可能退化成普通视觉编码器；只把 reasoning 变 latent 但不改观察接口，收益有限；两者联合且让 action/answer 梯度穿过接口，才产生最大增益。但这是机制上的合理推断，不是片段报告的结果。实际消融可能出现非加性、某一项负贡献或训练不稳定，需回原文看表。

4. 证据采集现象：answerer swaps 的意义在于排除“证据只对原 answerer 有用”的解释。如果换一个 answerer 仍能利用同一批证据答对，说明学生采集的是可转移的证据，而不是与特定推理路径耦合的伪影。人类对增量证据的评估则看每一步是否带来新信息；若很多轮被判定为冗余，说明 8 轮上限或停止策略可能不是瓶颈，真正的问题是早期轮次的检索质量。教师轨迹对比显示学生更接近教师的采集行为，但这既可能是好事（学到了有效策略），也可能是坏事（模仿教师风格而非真正搜索）；需要结合 accuracy 和 evidence usefulness 判断。

5. 效率与 accuracy 的张力：论文同时报告更低推理延迟和更高 accuracy，若成立，说明 latent 接口减少了文本生成与上下文开销，而不是用更多计算换精度。但延迟收益可能来自更短的中间文本，也可能来自更少的工具调用；片段没有拆分。固定 8 个 latent tokens 和最多 8 轮是强约束，可能在小部分难题上限制上限，却在平均成本上占优。

6. 失败模式与负结果线索：论文明确承认无音频，因此涉及对话、环境声、声音事件的题目会失败。教师轨迹错误会通过 action/answer 监督传播；冻结 CoT 学生可能给分布对齐设上限；训练时教师动作回放、推理时学生自选动作，可能出现 exposure bias，早期动作一旦偏离教师分布，后续观察分布就变了。这些是方法边界，不一定是论文报告的负结果，但属于应重点检查的失败模式。

7. 规模趋势：4B 与 7B 都报告 strong performance，说明方法不依赖单一规模；但片段没有给出两者相对文本 CoT 的增益差。若 4B 增益更大，说明 latent 接口对小模型尤其有价值；若 7B 增益更大，说明容量仍是瓶颈。这个 scaling 问题值得回原文核对。

8. 指标间张力：accuracy 提升不自动等于“更会看”。更接近教师轨迹也不自动等于更好，因为教师也可能有冗余采集。论文用 answerer swaps 和人类评估来补足这一点，是实验设计上的亮点；但这些分析的样本量、标注者一致性、统计显著性在片段中未知。

Q6: 有什么可以进一步探索的点？

1. 音频与多模态观察：论文明确把无音频列为限制。最直接的扩展是把 observer 从帧编码器扩展到音频、语音、文本屏幕、传感器流，并让 latent tokens 携带时间对齐的多模态证据。需要研究的是：音频线索如何与视觉 token 融合，是否要单独的 observer adapter，以及“看哪里”是否要扩展为“听哪段/读哪屏”。

2. 自适应 latent 预算与停止策略：主配置固定 8 个 latent tokens、最多 8 轮。可以探索按问题难度、证据不确定性、边际信息增益动态分配 token 数和轮数；学习停止策略，避免过早终止或冗余采集。也可把工具粒度（帧、片段、多帧、全视频摘要）纳入动作空间，让 reasoner 学“看多细”。

3. 更强的训练信号：当前依赖教师轨迹与冻结 CoT 学生。可引入 on-policy distillation / DAgger 缓解 exposure bias；用 process reward、counterfactual action reward 或 RL 直接优化“采集到的证据是否足以答题”；用信息增益奖励鼓励每一步观察带来新信息；研究教师噪声建模与鲁棒损失。

4. 可解释性与可控性：连续 latent tokens 不可读，部署时难以审计。可以训练探针把 latent 解码成时间区间、对象、置信度或自然语言摘要；研究 latent 与文本的混合接口，在需要解释时输出文本，在追求效率时走 latent；还要评估 latent 是否携带偏见、隐私或不可控内容。

5. 跨领域泛化：把 LARVA 迁移到具身视频、GUI 操作、手术视频、显微镜/实验记录、遥感时序等场景。不同领域的“证据”定义不同，工具也不同；需要验证 latent 接口是否依赖教师轨迹的领域先验。对 ai-for-science 而言，关键问题是能否从实验视频中主动定位异常帧、读取仪器读数、追踪样本状态。

6. 混合通信协议：文本与 latent 不必二选一。可以研究选择性 caption：高不确定性或需要人类复核时输出文本，其余走 latent；或让 observer 同时输出 latent 和低成本文本摘要，reasoner 自适应使用。还可研究多 observer 并行采集、层次化记忆、跨轮缓存，以支持更长视频。

7. 理论理解：为什么联合训练有效？可以用信息瓶颈、率失真、决策信息论解释“观察表示”应保留多少答题信息、多少搜索信息；分析 action supervision 与 answer supervision 的梯度如何塑造 observer；研究 latent 维度、token 数、轮数与任务复杂度之间的 scaling law。

8. 评测体系：需要更细的指标，不只看 accuracy，还看证据召回、时间定位、采集成本、冗余轮次、答案可验证性、对教师轨迹的依赖程度。构建反事实证据基准：换掉关键帧后模型是否改答案；加入干扰帧后是否仍能定位；评估跨 answerer 的证据可转移性。人类评估应报告样本量、一致性和显著性。

9. 系统与部署：在真实应用中，视频流是连续的，工具调用有延迟和费用。可以研究流式 LARVA、缓存复用、并行工具调用、失败重试、预算约束下的采集规划；以及小模型在端侧部署时的内存和延迟优化。

Q7: 总结一下论文的主要内容

论文的主线可以概括为：把“视频 agent 如何看、如何想、如何再去看”从大模型教师蒸馏到小模型时，不能只蒸馏文本推理链，还必须蒸馏观察接口本身。

1. 问题：长视频中只有少量帧与问题相关，agentic video 系统用多轮工具调用选择证据。小模型要学会的不只是答题，还包括“下一步看哪里”。难点在于每次 observation 有双重用途：既支撑当前答案，又指导后续采集。文本 caption 能描述当前可见内容，却可能漏掉搜索所需的线索；而且文本接口离散、不可微，最终答案和动作的监督难以直接训练 observer。

2. 方法：LARVA 让 reasoner 产生 latent reasoning block，决定工具调用或最终答案；observer 把选中的帧编码成紧凑 latent tokens，与时间戳一起进入上下文。与文本通信不同，observer 不写 caption，而是传递连续表示。训练时 reasoner 预测教师动作，执行教师动作得到帧，再让 observer 编码并进入下一轮；推理时 reasoner 自选动作，最多 8 轮。主配置每个推理/观察块 8 个 latent tokens。

3. 训练目标：action supervision 让每一轮动作对齐教师；answer supervision 监督最终答案；observation reconstruction 从 latent tokens 重建文本观察，防止视觉信息丢失；CoT-distribution alignment 用冻结的文本 CoT 学生约束推理分布。最关键的是联合优化：action 和 answer 的梯度穿过 latent interface 回传 observer，使 observer 学会传递“对下游决策有用”的信息，而不是仅生成可读描述。数据为 20,000 条过滤后的教师轨迹，来自 Video-Marathon、VideoHolmes 训练集和 LongVideo-Reason；教师 reasoner 是 DeepSeek-V4.1-Flash，教师 observer 是 Qwen3.8-27B；学生是 Qwen2.5-VL-7B 与 Qwen3-VL-4B，共享主干、分离 LoRA。

4. 实验：在 VideoHolmes、Video-MME、LVBench、LongVideoBench、MLVU 五个基准上评估，Video-MME 不使用 captions。核心受控比较是“相同教师轨迹训练的文本 CoT 学生”。摘要报告 7B 模型在 VideoHolmes、Video-MME、LVBench 上分别高出 5.1、2.5、3.7 个百分点；4B 也表现强。论文还做 latent reasoning、latent observation、joint optimization 的消融，评估推理成本，并用 answerer swaps、人类增量证据评估、教师轨迹对比来分析采集行为。

5. 发现与解释：连续 latent 接口优于文本 CoT 蒸馏，说明 caption 瓶颈真实存在；联合训练让 observer 的表征同时服务于答题和搜索；学生采集行为更接近教师，且推理延迟下降。增益在三个基准上不均，说明任务对“搜索线索”的依赖程度不同。论文把这些结果解释为“联合学习如何表示证据、如何基于证据行动”的价值。

6. 局限：只处理采样帧，无音频；依赖教师轨迹和冻结 CoT 学生，监督噪声会传播；固定 8 tokens/8 轮可能限制复杂问题；训练用教师动作回放、推理自选动作，存在分布偏移；latent 接口可解释性弱；共享主干加 LoRA 的容量与角色干扰仍需验证；评测以长视频 QA 为主，开放世界、具身、科学视频尚未覆盖。结论片段称“三个基准”，摘要称“五个”，具体口径需回原文核对。

7. 定位：这篇论文属于 agentic video understanding 与 latent distillation 的交叉：把“观察”从文本通信层解放出来，变成可训练、可压缩、面向决策的连续接口。它的贡献不只是几个点的 accuracy，而是提出了一种蒸馏范式——教师轨迹不仅教学生“怎么答”，还通过接口设计教学生“怎么把看到的东西传给下一步决策”。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：对 agent 方向：这篇论文直接研究多轮工具使用中的 observation interface 与 credit assignment，适合关注 agent 记忆、工具调用、停止策略和蒸馏的读者。

## 基本信息

- 作者：Junzhang Liu, Hao Yuan, Qi Zhang, Yiming Wu, Long Chen, Zhecan Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-27
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.35229`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的摘要、引言、方法、结果与结论片段；对片段未覆盖的具体数值、损失公式和实验设置缺口已在相应字段中显式标注。
