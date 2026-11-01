---
user_id: "cheng tan"
paper_id: 19339
arxiv_id: "2610.40131v1"
title: "VideoMemBench: How Well Do MLLMs Remember in Video Conversations?"
publish_date: "2026-10-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.40131v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.40131v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-01T23:10:57"
---
# VideoMemBench: How Well Do MLLMs Remember in Video Conversations?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：in-context video memory · multi-turn video question answering · multimodal large language models · benchmark

## 一句话总结

论文提出 VideoMemBench，用带类型跨轮依赖（Episodic Dependency 与 Answer Dependency）的长多轮视频对话来测量 MLLM 的 in-context video memory，并发现 20 多个模型中最佳者总体得分仅 45.7%。

## 摘要

> Multimodal large language models (MLLMs) can now process hour-long videos, making extended multi-turn dialogue over a single video a natural mode of use. Sustaining such a dialogue requires more than answering each question in isolation: the model must retrieve and reuse information established earlier in the conversation, both what it has seen and what it has said, an ability we term in-context video memory. Existing video QA benchmarks leave this ability unmeasured: their questions are mutually independent, never reference one another, and each is answerable from the video alone, so nothing ever requires carrying information across turns. We introduce VideoMemBench, in which each sample pairs a single video with a long multi-turn dialogue, whose turns are connected by typed cross-turn dependencies: an Episodic Dependency refers back to a visual episode described in an earlier turn, probing memory of what the model has seen, while an Answer Dependency builds on the model's own earlier answer, probing memory of what the model has said. Every dependency is verified considering two properties: 1) necessity, meaning the question cannot be answered without its earlier turns, and 2) non-leakage, meaning the question does not restate the information from its earlier turns. To enforce these properties, we design a construction pipeline with three LLM roles: a planner, a generator, and a judger, with each dialogue reviewed by human experts. VideoMemBench comprises 326 videos with 2,476 turns, of which 1,387 depend on earlier turns, including multi-parent dependencies and long-range call-back. Among over 20 evaluated open- and closed-source MLLMs, Claude Opus 5 performs the best with an overall score of only 45.7\%, highlighting the challenge of in-context video memory. Our dataset and evaluation are released.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：当 MLLM 已经能够把小时级视频放入百万 token 上下文，并把单视频长多轮对话作为自然交互方式时，如何测量模型是否真的在对话中维持并使用了 in-context video memory。作者把该能力拆成两条通道：其一是对看过的视觉情节的记忆，即 Episodic Dependency；其二是对自己在前轮说过内容的记忆，即 Answer Dependency。问题不在于单题视频问答的准确率，而在于后续问题是否必须回指早前轮次才能回答。现有视频问答基准的题目彼此独立、互不引用，且每道题都能仅凭视频和当前问题回答，因此模型即使每轮重置也可能取得不错分数，无法诊断跨轮记忆。

构造上的核心难点是必要性与非泄露之间的张力。一个问题若要不泄露前文信息，就不能复述早前情节；但若完全不提供线索，又可能无法保证它真的需要早前轮次。作者给出的反例是：如果视频中只有一个配料，问“还缺什么配料”虽然不直接泄露该配料身份，却仍然可以仅凭视频回答，因此不满足必要性。这说明“看起来像追问”不等于“真正依赖前文”。

评估上的难点包括：模型在对话中会保留自己的历史回答，Answer Dependency 会带来错误传播与自我一致性效应；如果模型前轮答错，后续轮次究竟是在记忆自己说过的内容，还是在正确推理，评分口径需要区分。长视频固定采样 128 帧也会形成视觉信息瓶颈，使记忆失败与感知失败混在一起。开放题由 LLM judge 评分，还会引入判分偏差。

论文的隐含假设包括：依赖图标注可靠、答案唯一、人工审查足够充分、128 帧能够代表视频关键证据、预构造对话接近真实用户交互。研究问题则包括：当前模型的总体天花板在哪里；Episodic 与 Answer 两类依赖难度是否不同；依赖距离、父轮数量、轮位置和模型规模如何影响表现；扩大上下文窗口是否等于提升记忆利用率。

Q2: 有哪些相关研究？

作者把相关工作放在两条主线上，并指出二者之间存在评测缺口。第一条是文本大语言模型中的对话记忆与多轮评测：LoCoMo、LongMemEval 以及 Hsieh et al. (2024)、Kuratov et al. (2024)、Tan et al. (2025)、Wu et al. (2025)、Huet et al. (2025) 等工作，系统测量了长对话、多会话或长上下文中的信息保持与重用。这些基准有跨轮依赖，但不包含视频视觉流，也不要求模型把文本答案锚定到具体视频时间段或视觉实体。

第二条是视频理解基准：作者引用 Fu et al. (2025)、Li et al. (2024)、Mangalam et al. (2023) 等代表性工作，覆盖长视频问答、事件理解与时序定位。它们提供视频证据，但题目通常相互独立，最多是形式上的多轮问答；每道题可以仅凭视频和当前问题回答，不需要回指早前对话。因此，这类基准衡量的是单轮或独立同分布的视频理解，而不是对话中的记忆维持。

作者将缺口定位为：文本记忆基准有跨轮依赖但无视频；视频基准有视频但问题独立；即使把视频问答组织成多轮形式，只要问题彼此不引用，也不能测出 in-context video memory。VideoMemBench 的贡献不是提出新的记忆架构，而是提供带类型依赖图的评测床，可与 memory-augmented MLLM、video RAG、长上下文模型评测和 agent 对话状态评测形成互补。论文的相关工作部分还隐含地区分了“记住视频内容”和“记住自己说过什么”，这两类失败在以往基准中通常混在一起。

Q3: 论文如何解决这个问题？

论文把任务形式化为：给定视频 V 和多轮对话，第 t 轮输入为 V 与之前所有轮次的问答对，即 V ∥ (q1, â1) ∥ … ∥ (qt−1, ât−1) ∥ qt，其中 âi 是模型自己在第 i 轮的回答。模型按对话顺序作答，历史中保留的是模型自己的输出，而不是标准答案。所有模型使用相同的 128 帧采样，帧带时间戳并固定在对话开头提供。

数据设计的核心是类型化跨轮依赖。Episodic Dependency 指向早前轮次描述过的视觉情节，要求模型回看并复用当时观察到的视觉证据；Answer Dependency 建立在模型自己早前的答案上，要求模型记住自己说过什么。依赖图以边列表表示，每条边包含 from、to、type（answer 或 episodic）、evidence（t_start、t_end）与 intent。一个轮次可以有多个父轮，形成多父依赖，也可以跨较长距离形成 long-range call-back。

构建流程包含三个 LLM 角色。Stage 1 由 planner 规划对话与依赖图，确定哪些轮次依赖哪些早前轮次，并标注证据时间段与意图；Stage 2 由 generator 生成具体轮次，检索片段显示使用 Gemini 3.5 Flash 作为生成模型并采用 Turn Generation 提示；Stage 3 由 judger 验证依赖是否满足两个必要条件。每段对话再经人类专家审查。必要性要求问题不能在没有早前轮次时被回答；非泄露要求依赖问题不能复述早前轮次的信息。作者用“视频只有一个配料”的例子说明：不泄露不等于必要，二者必须同时满足。

评测指标方面，片段显示作者报告三个指标，但截断处未给出完整命名；从后文图表标题看，至少包括 independent-turn accuracy 与 dependent-turn accuracy，overall 是摘要中给出的总体口径（合理推断，具体定义需回原文核对）。开放题由 GPT-5.4 mini 判分，多选题抽取预测选项，每题判为正确或错误。数据规模为 326 个视频、2,476 个轮次，其中 1,387 个依赖轮，包含多父依赖与长距离回调；视频来自五个现有基准与另一来源，片段在“and the…”处截断，具体来源需回原文确认。

Q4: 论文做了哪些实验？

论文评测了 25 个 MLLM，包括 8 个闭源模型：Claude Opus 5、Claude Sonnet 5、GPT-5.6-sol、GPT-5.5、GPT-5.4、Gemini 3.8 Flash、Gemini 3.5 Flash、Gemini 3.1 Pro Preview；以及 17 个开放权重模型：Kimi K3、Qwen3.5 系列（0.8B、4B、9B、27B、35B-A3B、122B-A10B、397B-A17B）、Qwen3-VL 系列（4B、8B、30B-A3B、32B、235B-A22B）、Qwen2.5-VL 系列（3B、7B、32B）与 Keye-VL-1.5-8B。所有模型接收相同的 128 帧、相同时间戳与固定帧序列，问题严格按对话顺序呈现，并保留模型自己的历史回答。

实验是统一协议下的评测，没有提出新模型，也没有训练方法；因此主要变量是模型家族、规模、是否闭源，以及依赖距离、父轮数量、轮位置和依赖类型。评估使用 GPT-5.4 mini 作为开放题 judge，多选题抽取选项，报告三个指标；摘要给出的总体最佳成绩为 Claude Opus 5 的 45.7%。

分析维度包括：总体排行榜；Qwen3.5 规模趋势；Qwen3-VL 与 Qwen2.5-VL 在同规模下的比较；依赖距离（当前轮到最早父轮的距离）；父轮数量（一个轮次依赖多少个早前轮次）；轮位置与依赖（Table 3）；独立轮与依赖轮的准确率对比。Figure 4(a) 按依赖距离分组，Figure 4(b) 按父轮数量分组，Figure 4(c) 按对话位置比较独立轮与依赖轮。

数据统计为 326 个视频、2,476 个轮次、1,387 个依赖轮，包含多父依赖与长距离回调；每段对话经过人类专家审查。论文没有报告训练数据规模、标注者一致性、人工审查抽样比例或 LLM judge 与人类的一致性，这些是证据缺口。实验覆盖的模型规模跨度很大，从 0.8B 到数百 B 级，但闭源模型的具体参数未公开，跨家族比较只能按接口版本进行。

Q5: 发现了什么实验现象？

最醒目的现象是总体天花板很低：在 20 多个模型中，最佳 Claude Opus 5 的总体得分只有 45.7%，说明当前 MLLM 在长视频多轮对话中远未可靠地维持 in-context video memory。由于题目被设计为必须依赖早前轮次，这一数字不能简单用单题视频理解能力解释。

依赖距离呈明显衰减。所有被评测模型的平均准确率从距离 1 的 30.2% 降到距离至少 5 的 23.1%。模型间敏感度不同：Gemini 3.8 Flash 从 41.1% 相对平缓地降到 30.2%，而 Qwen3.5-35B-A3B 在距离超过 3 后下降更陡，从 27.8% 降到 19.2%。这说明长距离回调对不同架构的难度并不一致，也提示“上下文窗口足够长”并不等于“能有效检索远处信息”。

父轮数量也带来一致下降。Kimi K3 在单父时最高，为 40.8%，到三父时降到 29.8%；GPT-5.4 从 34.9% 降到 23.7%；Qwen3.5-27B 从 30.1% 降到 17.3%。多父依赖要求模型同时整合多个早前轮次的信息，比单链回指更难。值得注意的是，在父轮数分层比较中 Kimi K3 在每个父轮数上都是最强，而摘要给出的总榜最佳是 Claude Opus 5；两者评测子集或指标口径可能不同，提示总榜与分层结论不能简单互推（合理推断，需核对完整表格）。

规模趋势在 Qwen3.5 家族内成立：总体得分从 0.8B 的 16.2% 上升到 27B 的 34.3%。同规模跨代比较中，Qwen3-VL-32B 比 Qwen2.5-VL-32B 高 6.2 个百分点，说明视觉-语言训练代际进步对记忆任务有实际帮助。但规模提升并没有消除长距离和多父依赖的衰减，最强模型仍低于 50%，呈现“规模有帮助、但不足以解决问题”的趋势。

独立轮与依赖轮的对比是另一个关键观察。图表标题显示作者同时报告 independent-turn accuracy 与 dependent-turn accuracy；合理推断依赖轮准确率更低，但证据片段没有给出具体差值。若独立轮明显高于依赖轮，则说明失败主要来自跨轮信息携带，而不是单题视频理解；若差距不大，则需要重新审视依赖题是否真的必要。

指标之间也存在张力。总体分数会混合独立轮与依赖轮，依赖轮占比高时总体分数对记忆能力更敏感；不同模型在距离、父轮数和总榜上的排序可能不一致。Answer Dependency 还可能奖励“记住自己错误答案”的自我一致性：模型若稳定复述前轮错误，可能在依赖轮上表现得“连贯”但事实错误。论文的证据没有说明评分是否把这种情形单独区分，这是需要回原文确认的评价设计问题。

固定 128 帧采样是潜在混淆因素。长视频中的关键视觉证据可能未被采样，导致 Episodic Dependency 的失败被误判为对话记忆失败；反之，若答案可以从语言常识或当前问题猜测，必要性验证不足又会高估记忆能力。论文用 necessity 与 non-leakage 双约束缓解后者，但前者仍需通过帧采样消融或全帧对照来确认。

Q6: 有什么可以进一步探索的点？

第一，解耦视觉记忆与对话记忆。可以设置四类条件：完整视频加完整历史、完整视频加摘要历史、稀疏视频加完整历史、仅历史无视频，观察依赖轮准确率如何变化，从而判断失败来自视觉证据缺失、对话状态丢失还是整合能力不足。

第二，分离 Episodic 与 Answer 两类依赖的评分。当前总体分数可能混合两种能力；未来应分别报告，并设计反事实干预：替换模型前轮答案，看后续轮次是否随之改变；或在前轮注入错误答案，测量模型是纠正还是盲从。这能区分“记住自己说过什么”与“把正确答案记成错误答案”。

第三，改进依赖构造与验证。用更强的 judger、多人标注一致性、对抗性必要性测试（如仅凭视频能否答对）来校准依赖图；引入隐含依赖、话题连续性、跨视频指代和用户澄清，避免只覆盖显式回指。

第四，发展记忆机制。结构化记忆图、事件时间线、实体卡、可检索的对话记忆库、带时间戳的证据引用，都可能帮助模型在长对话中定位早前证据；训练上可用依赖图做课程学习，从短距离单父依赖逐步扩展到长距离多父依赖，并用强化学习奖励正确回指。

第五，扩展评测生态效度。当前对话是预构造的，未来可加入真实用户追问、话题漂移、指代修正、多视频会话和跨会话记忆；同时扩展到科学实验视频、手术视频、行为观察等 ai-for-science 场景，但需要重新设计领域依赖和专家审查协议。

第六，评价方法学。需要研究 LLM judge 与人类的一致性、开放题答案唯一性、选项设计偏差、总体指标的加权方式，以及依赖轮占比对排行榜的影响。还应报告置信区间和分层样本量，避免在小样本分层上过度解读。

第七，与长上下文和检索增强研究连接。可以比较全帧、稀疏帧、检索帧、分层摘要等输入策略，检验“更多帧”与“更好记忆”的关系；也可以把 VideoMemBench 作为 memory-augmented MLLM、video RAG 和 agent episodic memory 的测试床。

Q7: 总结一下论文的主要内容

论文关注一个正在变得现实的使用场景：MLLM 已能处理小时级视频，用户会围绕同一部电影、同一场比赛或同一段长录像连续追问角色、事件和细节。此时模型不仅要回答当前问题，还要检索并复用对话早前已经确立的信息，包括它看过的视觉情节和它自己说过的答案。作者把这种能力称为 in-context video memory，并指出它没有被现有视频问答基准测量。现有基准的题目彼此独立、互不引用，且每道题都能仅凭视频回答，模型可以在每轮“重置”而不受影响。

VideoMemBench 的设计核心是类型化跨轮依赖。每个样本包含一个视频和一段长多轮对话；轮次之间由 Episodic Dependency 或 Answer Dependency 连接。前者回指早前轮次描述的视觉情节，探测“看过什么”；后者建立在模型自己早前的答案上，探测“说过什么”。每个依赖都要求满足必要性（没有早前轮次无法回答）与非泄露（问题不复述早前轮次的信息）。作者用“视频只有一个配料”的例子说明二者缺一不可：不泄露的问题仍可能仅凭视频回答，因此不构成真正的跨轮依赖。

为规模化构造这种数据，论文设计了三个 LLM 角色的流程：planner 规划对话和依赖图，generator 生成具体轮次，judger 验证必要性与非泄露，最后由人类专家审查每段对话。依赖图包含 from、to、type、evidence 时间段和 intent，支持多父依赖与长距离回调。最终数据包含 326 个视频、2,476 个轮次，其中 1,387 个轮次依赖早前轮次。视频来自五个现有基准与另一来源，具体来源在证据片段中截断。

评测协议统一而严格：所有模型接收相同的 128 帧、带时间戳、固定顺序；问题按对话顺序呈现；历史中保留模型自己的回答，而不是标准答案。开放题由 GPT-5.4 mini 判分，多选题抽取选项，报告三个指标，其中总体分数是摘要强调的口径。论文评测了 25 个 MLLM，包括 Claude Opus 5、Claude Sonnet 5、GPT-5.6-sol、GPT-5.5、GPT-5.4、Gemini 3.8 Flash、Gemini 3.5 Flash、Gemini 3.1 Pro Preview，以及 Kimi K3、Qwen3.5 系列、Qwen3-VL 系列、Qwen2.5-VL 系列和 Keye-VL-1.5-8B。

实验结果描绘出清晰的能力缺口。最佳模型 Claude Opus 5 的总体得分只有 45.7%，说明长视频多轮记忆仍远未解决。所有模型的平均准确率随依赖距离增加从 30.2% 降到 23.1%；Gemini 3.8 Flash 从 41.1% 降到 30.2%，Qwen3.5-35B-A3B 在距离超过 3 后从 27.8% 陡降到 19.2%。父轮数量增加也导致一致下降：Kimi K3 从 40.8% 降到 29.8%，GPT-5.4 从 34.9% 降到 23.7%，Qwen3.5-27B 从 30.1% 降到 17.3%。在 Qwen3.5 家族内，规模带来提升，从 0.8B 的 16.2% 到 27B 的 34.3%；同规模下 Qwen3-VL-32B 比 Qwen2.5-VL-32B 高 6.2 个百分点。

论文的结论是：扩大上下文窗口并不自动带来有效的对话记忆；长距离回调、多父整合和答案链传播仍是难点。VideoMemBench 的意义在于把“记住视频”与“记住自己说过什么”拆成可测量的依赖类型，并提供必要性/非泄露验证与分层分析。它的局限也很明显：固定 128 帧可能成为视觉瓶颈；Answer Dependency 存在前轮错误传播与评分歧义；LLM 生成与 LLM judge 可能引入偏差；预构造对话的生态效度有限；总体指标与分层指标的关系需要完整表格核对。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 记忆高度同构：VideoMemBench 同时测“记住看过的证据”和“记住自己说过的话”，可直接类比 agent 的 episodic memory、对话状态追踪和自我历史一致性。

## 基本信息

- 作者：Zhengdong Hu, Chao Wang, Kaituo Feng, Huanjin Yao, Mingfei Lu, Hehe Fan, Yi Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.40131v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 检索命中的 Introduction、Results、Discussion 片段，并用摘要与结论校正了 heuristic_draft 中混入参考文献噪声的字段；Method 正文缺失，构建流程细节主要来自检索片段，已在相关字段标注证据缺口。
