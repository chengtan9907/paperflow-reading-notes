---
user_id: "cheng tan"
paper_id: 19008
arxiv_id: "2610.36959"
title: "Live-OPSD: On-Policy Self-Distillation for Proactive Streaming Large Video Models"
publish_date: "2026-10-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.36959.pdf"
pdf_url: "https://arxiv.org/pdf/2610.36959"
abs_url: "https://arxiv.org/abs/2610.36959"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-28T11:51:00"
---
# Live-OPSD: On-Policy Self-Distillation for Proactive Streaming Large Video Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：proactive streaming · on-policy distillation · large video models · privileged hints

## 一句话总结

Live-OPSD 是一种面向主动式流式视频模型的后训练方法：它先让策略在无提示的流式上下文中自行生成「说话/沉默」决策及其后续内容，再用同一模型在仅训练期可用的特权提示（privileged hint）下重新评估这段自我生成的续写，并把教师分布作为 token 级监督回灌给学生，同时用 Decision-Aware Selection 按响应决策分配更新预算；在 Qwen3-Omni-30B-A3B 上，LiveProBench 平均分达到 59.64，比最强 continued-SFT 基线高 5.26 分，OVO-Bench 上为 65.84，仍保持竞争力。

## 摘要

> Streaming multimodal assistants must make response decisions while events are
> still unfolding. They need to recognize when the available evidence warrants a re-2026 sponse, produce an appropriate continuation, and remain silent otherwise. Super-
> vised fine-tuning learns from recorded assistant turns, but does not directly train
> on the response prefixes produced by the evolving policy itself. We introduceOct Live-OPSD, an on-policy self-distillation method that uses privileged hints avail-
> able only during training to supervise the policy on its own outputs. The policy
> first generates a continuation from the streaming context alone. The same model27
> then re-evaluates that continuation with a hint describing the relevant evidence,
> and the resulting distributions guide the unhinted policy. On Qwen3-Omni-30B-
> A3B, Live-OPSD attains an average score of 59.64 on LiveProBench, 5.26 points
> above the strongest continued-SFT baseline, and stays competitive on OVO-Bench
> (65.84). These results indicate that guiding the policy on its own outputs with
> privileged hints improves both when and how a streaming assistant responds.[cs.LG]

Q1: 这篇论文试图解决什么问题？

1. 任务本质是一个耦合了感知与时间决策的问题。流式助手面对的不是「给定完整视频回答问题」，而是「在音视频 chunk 持续到达、证据尚不完整时，判断当下是否构成一次值得响应的时机」。论文把这一点表述为：正确的内容如果出现在错误的时间点，同样是无效帮助；过早开口打断交互，该响应时保持沉默则错失帮助机会。因此系统的输出空间从「答案」扩展为「决策（speak / silence）+ 续写」。
2. 论文识别出三类具体失败模式：(a) 在证据不足时过早响应；(b) 在响应时机已到时仍然沉默；(c) 决策正确但续写内容不恰当（说错、说不全、答非所问）。这三类错误在连续流中代价不对称——打断用户的代价与漏掉事件的代价性质不同，但 SFT 目标并不区分。
3. 作者把根因定位在训练信号与策略自身前缀之间的错配，并称之为 response-level gap：SFT 在人工标注的助手轮次前缀上优化参考续写（teacher forcing），而策略实际推理时会走过自己产生的、可能已经出错的前缀（例如已经提前开口，或在应沉默处已经说出半句话）。这些由策略自身产生的状态从未被显式纠正，随着策略更新，需要被纠正的续写分布也在漂移，而标注前缀是静止的。这与经典的 exposure bias / 分布偏移问题同源，但在流式主动交互中被放大——因为「沉默」这一决策本身就改变了后续可见上下文。
4. 为什么不能直接用强化学习解决：合理推断，主动式响应的奖励高度稀疏（信号只落在决策点附近），「何时说」的监督在连续时间上天然是区间而非单点，沉默样本远多于响应样本导致类别极不平衡，且响应内容质量难以用可验证奖励自动判定。论文没有走 RL 路线，而是选择了「用训练期特权信息构造稠密 token 级监督」的路线。
5. 论文的技术切口是：既然标注前缀无法覆盖策略自身的错误前缀，那就让策略自己在流式上下文下采样前缀，再用一个「事后诸葛亮」式的特权视角（知道相关证据是什么）来评判这段前缀，把「该不该在这里说、该说什么」压缩成同一模型在两种条件下的分布差异，从而得到一个稠密、on-policy、且不需要人工逐 token 标注的监督信号。
6. 隐含假设（需要读者自行审视）：(a) 特权提示在训练期确实可获得且质量足够，否则教师信号本身带偏；(b) 教师与学生同参数、仅条件不同，因此分布差异可以被解释为「证据相关性」的差异，而不是模型容量差异；(c) 「是否该响应」可以被局部地、逐 chunk 地判定，而不需要全局规划——这一假设在长时间跨度的多事件流中可能不成立（推测）。
7. 问题定义层面还有一个未完全展开的边界：论文当前的动作空间基本是二元的 speak / silence，而真实交互中还有打断（interrupt）、纠正（correction）、追问澄清、以及「说了但说早了、需要事后修正」等情形。作者在讨论中承认这些动作需要相应的监督与选择类别，属于当前实例化未覆盖的部分。

Q2: 有哪些相关研究？

（说明：检索证据未命中论文的 Related Work 原文段落，以下按引言中的引文线索与领域常识梳理，凡属推断处均已标注。）
1. 流式 / 在线视频语言模型与连续交互：论文把自身置于「把流式感知、时间对齐对话与显式响应控制结合起来」的一条线上，引用 Chen et al., 2024；Wang et al., 2024；Qian et al., 2025；Wang et al., 2025a；Ding et al., 2025；Azad et al., 2026。这条线的共同点是：模型不再等待完整视频，而是在 chunk 流上增量地维护状态并产生对话轮次。
2. Omni-modal 骨干：引用 Xu et al., 2025a;b，对应 Qwen2.5-Omni 与 Qwen3-Omni，把视觉、声音、语言放在同一模型里。论文的实验正是建立在这两个骨干上，因此其贡献被定位为「在已有感知与交互接口之上，解决训练信号怎么来的问题」。
3. 面向响应时机的监督目标改造（与本文最直接竞争的一类）：(a) StreamPro 的 CB-Stream Loss（Li et al., 2026），(b) AURA 的 Silent-Speech Balanced Loss（Lu et al., 2026）。这两者的共同思路是在 SFT 的交叉熵上重新加权，以缓解沉默/发声的类别不平衡，但正如背景片段所指出的，它们的目标「仍然绑定在标注的动作与前缀上」——即仍是 off-policy 的、在人工前缀上定义的损失，而不是在策略自身产生的续写上定义的损失。这正是 Live-OPSD 要区分的差异。
4. 开源主动式 / 实时多模态系统（作为系统级对比对象）：AURA（Lu et al., 2026）、JoyAI-VL-Interaction（Yao et al., 2026）、ROMA（Tian et al., 2026）、MOSS-VL-Realtime（Wang et al., 待确认）。这些是 LiveProBench 上模型级对比的对手。
5. On-policy 蒸馏与特权信息蒸馏谱系（合理推断，检索证据未直接覆盖）：Live-OPSD 在形式上属于 on-policy distillation / generalized distillation 一类——学生自己采样轨迹，教师在相同前缀上给出分布目标，梯度只经学生 logits 回传；同时它又属于 context distillation / privileged information distillation 一类——教师多看到的是「上下文」而非「更强的模型」，即教师与学生共享参数、只多一个条件。这一组合使它在工程上避免了维护第二个大模型，但把全部压力转移到了 hint 的质量上。
6. 散度方向选择的相关背景：论文用 β-JS 散度的两个极限来定位 forward KL（β→0 时为 D_KL(P‖Q)）与 reverse KL（β→1 时为 D_KL(Q‖P)），并明确表示不会把两个端点当作 JSD 值直接使用，而是实现为两个独立目标。这背后是 mode-covering 与 mode-seeking 行为的经典权衡，在本文语境下直接对应「多说一点（覆盖）」与「果断说（锐化）」的取舍。
7. 评测基准：LiveProBench（主评测）与 OVO-Bench（泛化/保持能力的旁证）。OVO-Bench 是已有的 omni 视频理解基准，被用来检验主动式后训练是否侵蚀通用理解能力；LiveProBench 在本文中被作为主动式流式评测的主场，但其具体构成、指标定义在检索证据中未出现（需回原文确认它是否为本文新提出）。

Q3: 论文如何解决这个问题？

1. 总体框架：Live-OPSD 是后训练（post-training）方法，起点是一个已经在相同流式交互协议上做过 SFT 的模型，因此与三个 SFT 基线共享初始化，差异只在后训练阶段引入的蒸馏信号。这一设计让「增益来自训练信号形态」而非「来自更多数据或更强初始化」的解释更站得住。
2. 数据组织：训练语料包含直接提问与需要主动响应的持续指令（standing instruction，例如「每次我把芒果切下一块就告诉我」这类需要跨时间判断的请求）。对话被展开为「因果决策上下文」（causal decision contexts）：每个上下文只包含在该时刻之前可见的音视频 chunk 与对话轮次，并配对齐的监督决策与 hint。划分以来源视频为边界，保证同一源视频的上下文不跨 split，避免近重复泄漏。
3. 学生（无提示）rollout：给定流式上下文，策略自回归地生成续写。这段续写同时承载两层信息——在决策位置的「说 / 不说」选择，以及选择说之后的响应内容。这使监督天然覆盖了 when 与 how 两个维度（这是相对只做决策分类的做法的一个结构性优势）。
4. 教师（有提示）重评：同一模型副本在「同一上下文 + 特权 hint」条件下，对学生的这段续写逐 token 重新打分，得到 P_teacher(·|prefix)。hint 描述的是「相关证据」，即在当前时刻是否存在足以支持响应的视觉/听觉依据。关键在于教师看到的是学生自己走过的前缀，而不是标注前缀——这正是 on-policy 的含义。
5. 梯度路径：检索证据明确写出「Teacher 的梯度只流经学生的 logits」，即教师端 stop-gradient，学生端最小化自己分布 Q 与教师分布 P 之间的散度。这排除了「两个模型一起塌缩」的退化路径。
6. 散度选择：论文分别实现 forward KL 与 reverse KL 两个独立目标，理由是 β-JS 在 β→0 与 β→1 时分别退化为这两个 KL，如果把端点当作 JSD 数值直接用会失去区分度。这一实现细节在消融中被证明是重要的（见实验现象部分）。
7. Decision-Aware Selection（DAS）：核心是按「响应决策」分配更新预算，并配合「教师打分的选择」（图 (b) 所示的 Teacher-Scored Selection）对候选样本排序。动机是：流中沉默步远多于响应步，若按 token 或按样本均匀采样，梯度会被「保持沉默」主导，模型收敛到保守的「几乎不说话」解；DAS 让两类决策都拿到足够的更新预算，同时用教师分数筛掉那些教师与学生本来就无分歧（信息量低）的样本。
8. 与 SFT 基线的机制差异可以概括为三点：(a) 优化位置从标注前缀移到策略自身前缀；(b) 监督信号从 one-hot 参考续写变为教师分布（更软、携带「哪些 token 上是勉强可接受、哪些是明确该选」的信息）；(c) 样本权重从均匀/类别重加权变为决策感知 + 教师打分的选择。
9. 尚未在证据中出现、需回原文补齐的部分：损失的具体形式与温度、两个 KL 目标的组合权重、hint 的具体文本模板与生成流程（作者称在 Appendix B）、DAS 的预算分配比例与选择阈值、教师与学生在同一 batch 中的前向次数（影响训练成本，推测约为普通 SFT 的 1.5–2 倍）。

Q4: 论文做了哪些实验？

1. 实验的整体设计被作者明确分为三层：within-backbone 比较（检验训练流程本身）、system-level 比较（与已发布的主动式模型并列）、以及消融（分离散度方向与选择规则各自的贡献）。这个三层结构本身值得注意，因为它把「方法是否有效」与「方法在什么条件下有效」分开检验。
2. 骨干与协议：Qwen2.5-Omni-7B 与 Qwen3-Omni-30B-A3B，两者都被适配到同一套流式交互协议——音视频 chunk 与用户/助手轮次交错。训练语料同时包含直接提问与需要主动响应的持续指令。后训练阶段，对话被扩展为带对齐监督决策与 hint 的因果决策上下文；split 以源视频为边界。
3. 监督基线（同一骨干上比较三个目标）：(a) Vanilla-SFT，标准交叉熵，作用在参考助手轮次上；(b) CB-SFT，使用 StreamPro 的 CB-Stream Loss（Li et al., 2026）；(c) SSB-SFT，使用 AURA 的 Silent-Speech Balanced Loss（Lu et al., 2026）。Live-OPSD 从同一个 streaming-SFT 模型出发，再施加第 4 节的后训练流程。这三条基线的选择是有针对性的：它们代表「不改优化位置、只改损失权重」的路线，正好用来隔离「on-policy + 特权提示」这一改动。
4. 模型级对比：在 LiveProBench 上（Table 1）与 AURA、JoyAI-VL-Interaction、ROMA、MOSS-VL-Realtime 对比。这组对比回答的是「相对同期系统是否站得住」，而非「相对同初始化基线是否有增益」，两者不可互相替代。
5. 评测基准：LiveProBench 为主指标（报告平均分），OVO-Bench 作为泛化/保持能力检查（65.84）。这一双基准设计隐含的判断是：主动式后训练的主要风险是「为了抓时机而牺牲理解」，因此必须有一个非主动式的旁证。
6. 消融维度：(a) 散度方向——forward KL、reverse KL、以及 β-JS 端点相关的变体，对应 Table 4 与 Figure 5；(b) 选择规则——去掉 DAS 的 fixed-suffix 变体（w/o DAS），在 Figure 5 与 Appendix G 中报告稳定性；(c) teacher-scored selection 的作用（图 (b) 所示的选择机制）。
7. 证据缺口（需回原文补齐，不应凭空补全）：LiveProBench 的题目构成、指标定义与平均分的聚合方式；7B 骨干的具体数值；训练超参、样本量、教师/学生前向开销（作者称在 Appendix F 报告「从现有材料可核实的运行设置」）；hint 的构造细节（Appendix B）；以及是否报告了误触发率与漏事件率这类分解决策指标——从结论看，论文主要用单一 average score 呈现结果，这会把「多说」与「少说」的取舍隐藏在总分之下。

Q5: 发现了什么实验现象？

1. 主结果：在 Qwen3-Omni-30B-A3B 上，Live-OPSD 在 LiveProBench 取得 59.64 的平均分，比最强的 continued-SFT 基线高 5.26 分。这个增益幅度值得注意——它是在同初始化、同数据协议下，仅改变「在哪段前缀上、用什么信号监督」得到的，因此可较直接地归因于 on-policy 与特权提示的组合，而不是数据规模。
2. 泛化侧：OVO-Bench 上 65.84。作者的表述是「保持竞争力」，而不是「提升」。这在实验现象上意味着主动式后训练的收益并不是全局能力提升，而是集中在决策边界附近——合理推断，因为 OVO-Bench 主要考察理解而非时机，如果训练显著改变了理解能力，反而会是警示信号。
3. 散度方向的非对称现象（最有信息量的观察之一）：forward KL 变体表现出欠触发（under-trigger），即在 LiveProBench 上倾向于少说，作者明确说它「接受更高的 event-miss rate 作为保守权衡」。机制上可以这样解释（合理推断）：forward KL 是 mode-covering，会惩罚教师有质量而学生没有的概率，倾向于把学生的概率质量摊开到教师的多个可接受模式上；对于一个接近低熵的二元决策 token，这会把「说话」的概率稀释，于是行为偏向沉默。相比之下，reverse KL 是 mode-seeking，倾向于锐化到单一模式，行为更果断，但可能有更高的误触发风险。这一现象把「散度方向」从一个实现细节提升为决定系统行为偏置的设计变量。
4. 选择规则的必要性：fixed-suffix 的 w/o DAS 变体被报告为「less stable」（Figure 5、Appendix G）。这说明 DAS 不只是性能增益项，更是训练稳定性项——在没有决策感知预算分配的情况下，沉默样本占优会让优化过程在「全沉默」这一退化解附近震荡。
5. 一个明确的负结果 / 失败案例：作者报告 pilot 检查发现 Qwen3-Omni 作为自身的 hint 生成器「不一致」（检索片段在此处截断，完整表述需回原文），因此最终采用的是离线构造的特权 hint 与录制历史，理由是「offline hints and recorded histories make training tractable」。这是一个诚实的工程性负结果：同模型自提示（self-hinting）在现阶段的可靠性不足，方法对 hint 生产管线有外部依赖。
6. 指标之间的张力：从可获得的片段看，论文报告的是 average score，而 forward KL 变体的讨论暴露了 event-miss rate 与误触发率之间的取舍。这意味着单一平均分会把「保守型」与「激进型」策略压成同一个数字——两类错误代价不对称时，这个聚合方式本身是一个值得质疑的评价设计选择（判断基于片段推断，需回原文确认是否另有分解指标表）。
7. 稳定性趋势：Figure 5 与 Appendix G 被用来支持「forward KL 与固定后缀 w/o DAS 更不稳定」的结论，说明论文把训练曲线而不只是终态分数当作证据。这类证据对复现很重要，但在检索片段中只有定性描述，缺少具体曲线形态与方差数值。
8. 系统级对比的结果细节（与 AURA / JoyAI-VL-Interaction / ROMA / MOSS-VL-Realtime 的具体名次与分差）在检索证据中未完整出现，只能确认对比发生在 LiveProBench 上。

Q6: 有什么可以进一步探索的点？

1. 把离线 hint 换成在线/可学习的 hint：当前实例依赖离线特权提示与录制历史，这既是工程便利也是上限。可探索的方向包括——训练一个专门的事件证据判定器作为 hint 源、用小模型或结构化视觉事件检测器生成 hint、或者用同一模型的多视角（例如不同采样帧率、不同时间窗口）来构造 pseudo-privileged 信息。作者的 pilot 负结果（同模型自提示不一致）提示这条路并不平坦，但也正因如此值得系统研究「hint 的信息量—可靠性—成本」三元权衡。
2. 扩展动作空间：论文明确指出 interrupt 与 correction 类动作需要相应的监督与选择类别。把二元 speak/silence 扩展为 {沉默, 响应, 打断, 纠正, 追问澄清, 简短确认（backchannel）} 会带来新的问题——不同动作的先验极不均衡，DAS 的预算分配需要一个更细的类别设计，且纠正动作的监督信号需要「之前说错了」这一跨轮依赖。
3. 更细粒度的 credit assignment：当前把「决策 token」与「内容 token」放在同一段续写里做 token 级蒸馏，二者的最优权重未必相同。可探索决策位置加权、时间折扣（越晚的 token 越不重要）、以及把决策 token 的损失与内容 token 的损失解耦后用不同散度方向（例如决策用 reverse KL 求果断、内容用 forward KL 求覆盖）——这是一个从现有消融现象直接推得的假设。
4. 与 RL / 可验证奖励结合：OPSD 提供稠密 token 级信号，RL 提供 sequence-level 效用（例如用户满意度、任务完成、打断成本）。二者的组合可能弥补 OPSD「教师也可能错」的弱点：当教师信号与序列级奖励冲突时，用奖励来校正。
5. 散度调度：既然 forward KL 偏保守、reverse KL 偏果断，一个自然的想法是 β 退火——训练早期用覆盖性强的方向探索，后期锐化到果断策略；或者按决策类别自适应选择方向。这是现有 Table 4 / Figure 5 现象的直接延伸。
6. 显式代价建模：把「是否响应」写成带成本的序贯决策（期望帮助 > 打断成本才说），引入可调的用户耐心 / 打断代价参数，使同一模型能在保守—激进之间连续调节，而不是靠散度方向这种隐式偏置。这对产品化尤其重要——不同场景（直播审核 vs 会议助手 vs AR 眼镜）对误触发的容忍度差异很大。
7. 长时程与多事件流：当前上下文是逐 chunk 的因果决策上下文，长时（小时级）会话中的事件累积、话题切换、用户意图漂移尚未被检验。推测这需要引入记忆机制或分层决策，而不仅是把窗口拉长。
8. 评测方法学：需要 latency-aware、代价敏感的指标（例如以打断代价加权的效用曲线），而不是单一 average score；还需要人评来校准「说得对不对」与「说得是不是时候」的相对重要性。长时会话、多用户、隐私敏感场景也在当前范围之外。
9. 跨域迁移与跨骨干泛化：方法在两个 Qwen Omni 骨干上验证，是否适用于非 omni 的纯视觉骨干、纯音频流、机器人第一视角、科学观测流（与 ai-for-science 场景的交集：实验进程监控、异常事件触发）都未验证。
10. 理论侧：为什么「同参数 + hint」的自蒸馏能够改进决策，其信息上界是什么？与 context distillation、generalized distillation 的形式关系、以及与 DAgger 类方法的联系，都还缺少分析——尤其是「hint 不完美时会引入何种偏置」这一问题，目前只有工程性规避（用离线 hint）而没有理论刻画。

Q7: 总结一下论文的主要内容

这篇论文要解决的是流式多模态助手中的「主动性」训练问题。在连续到达的音视频流里，一个好的助手不仅要答得对，还要在正确的时刻开口：证据不足时保持沉默，事件发生时及时响应。作者把这类系统的输出刻画为「决策（speak/silence）+ 续写内容」的复合体，并指出该任务的三类典型失败——过早开口、应说未说、决策正确但内容失当。
论文的论证主线是：现有监督微调之所以修不好这三类错误，根因在于监督发生的位置。SFT 在人工标注的助手轮次前缀上优化参考续写，而策略推理时走的是自己产生的前缀；一旦策略提前开口或在应沉默处说了半句，它就会进入一个从未被标注、也从未被纠正的状态。作者把这一错配称为 response-level gap，并强调随策略演化，需要被纠正的续写分布也在漂移，而标注前缀是静止的。他们也据此区分了自己与同期工作（StreamPro 的 CB-Stream Loss、AURA 的 Silent-Speech Balanced Loss）：后者虽然重新加权以缓解沉默/发声不平衡，但目标仍绑定在标注动作与标注前缀上，仍是 off-policy 的。
技术主线是「用训练期特权信息给策略自己的输出打稠密标签」。Live-OPSD 分两步：学生先仅凭流式上下文自回归生成一段续写（其中包含决策位置与响应内容）；随后同一模型的副本在附带特权 hint（描述当下是否存在足以支持响应的证据）的条件下，对这段学生自己走过的续写逐 token 重新打分，得到教师分布。学生最小化自己的分布与该教师分布之间的散度，梯度只经学生 logits 回传，教师端 stop-gradient。散度方向被显式处理：作者用 β-JS 的两端极限定位 forward KL 与 reverse KL，并坚持把它们实现为两个独立目标，而不是直接取 JSD 端点数值。第二个组件是 Decision-Aware Selection（DAS）：因为流中沉默步远多于响应步，均匀采样会让优化塌向「一直沉默」的退化解；DAS 按响应决策分配更新预算，并配合教师打分对候选样本排序，把更新集中在决策有信息量的位置。整套流程建立在一个已做流式 SFT 的同源模型上，因此可与 SFT 基线做受控比较。
实验主线分三层：within-backbone 比较、system-level 比较、消融。骨干为 Qwen2.5-Omni-7B 与 Qwen3-Omni-30B-A3B，均适配到统一的流式交互协议；训练语料混合直接提问与需要主动响应的持续指令；对话展开为带对齐监督决策与 hint 的因果决策上下文，并按源视频划分 split。基线为 Vanilla-SFT、CB-SFT（StreamPro）、SSB-SFT（AURA），Live-OPSD 从相同的 streaming-SFT 模型出发。系统级对手包括 AURA、JoyAI-VL-Interaction、ROMA、MOSS-VL-Realtime。主指标是 LiveProBench 平均分，OVO-Bench 作为理解能力是否被侵蚀的旁证。
关键结果是：Qwen3-Omni-30B-A3B 上 LiveProBench 59.64，比最强 continued-SFT 基线高 5.26 分；OVO-Bench 65.84，保持竞争力。消融中最有信息量的现象是散度方向的非对称性——forward KL 变体欠触发、偏向沉默，作者称其以更高的 event-miss rate 换取保守；去掉 DAS 的 fixed-suffix 变体更不稳定。这两条把「散度方向」和「选择规则」从实现细节提升为决定行为偏置与训练稳定性的设计变量。此外还有一个诚实的负结果：pilot 检查发现 Qwen3-Omni 作为自身 hint 生成器不稳定，因此最终采用离线构造的特权 hint 与录制历史。
作者在结论中把 Live-OPSD 定位为「特权 on-policy 蒸馏是主动式流式视频模型的一种有前景的训练原则」的证据，而非终局方案。边界也很清楚：动作空间目前基本是二元 speak/silence，interrupt 与 correction 需要额外的监督与选择类别；hint 依赖离线管线；评测集中在两个基准且以平均分呈现，把「多说」与「少说」的代价取舍压缩进了单一数字。论文附有 AI 使用声明，说明生成式 AI 仅用于语言润色与 SFT 数据、离线特权 hint 的合成，未参与训练执行或评测测量。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向的连接：本文处理的正是「何时行动」这一 agent 核心问题——把决策时机与内容生成耦合在一个模型中训练，其 Decision-Aware Selection 思路可迁移到工具调用时机、主动询问、事件触发式 agent 等场景。

## 基本信息

- 作者：Kaixuan Du, Xin Wan, Dai Guan, Ming Chen, Xiaoxi Jiang, Guanjun Jiang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-10-28
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.36959`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要依据 83 条 PDF 语义检索命中片段（Introduction / Results / Conclusion 覆盖较好，Method 与 Related Work 原文段落命中有限）并剔除 arXiv 页眉、图表残片与断行噪声，heuristic_draft 因内容碎片化且存在明显错位，仅作参考未予采信，凡属推断处已标注。
