---
user_id: "cheng tan"
paper_id: 18456
arxiv_id: "2610.34662"
title: "StreamVibe: Proactive Video Agents for Evolving User Intent"
publish_date: "2026-10-27"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.34662.pdf"
pdf_url: "https://arxiv.org/pdf/2610.34662"
abs_url: "https://arxiv.org/abs/2610.34662"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-27T12:50:55"
---
# StreamVibe: Proactive Video Agents for Evolving User Intent

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：proactive video agent · streaming video understanding · evolving user intent · benchmark

## 一句话总结

StreamVibe 把流式视频中的主动协助形式化为在持续观察与交互中判断“何时说、说什么”的策略问题，并同时提出覆盖显式、隐式、动态意图的 StreamVibe-Bench、经自适应流式 SFT 与局部轨迹 RL 训练的 StreamVibe-VL，以及维护活跃意图与任务状态的 StreamVibe-Harness。

## 摘要

> 摘要原文为空。根据已提供的 Introduction、Results、Discussion 与 Conclusion 片段，可重组出如下“清洁摘要”：论文研究流式视频中的主动交互，指出现有方法多面向显式查询与孤立单轮请求，难以应对三类现实挑战：隐式用户意图需要从持续观察中推断；任务相关状态会因先前交互而改变，长历史不等于最新状态；用户意图会随交互演化。为此，作者提出 StreamVibe 框架：StreamVibe-Bench 统一覆盖显式、隐式与动态意图，并提供 pointwise 与 trajectory 两种评测；StreamVibe-VL 作为主动流式策略模型，使用 adaptive streaming SFT 与 localized trajectory RL 训练；StreamVibe-Harness 将活跃意图与任务进度作为持久交互状态进行跟踪。实验表明该模型在响应时机与内容质量上优于开源基线、与专有模型具有竞争力，并保持通用视频理解；显式状态跟踪还能进一步减少过时、冗余与状态不一致的响应。具体数据规模、训练超参和完整数值需回原文与附录核实。

Q1: 这篇论文试图解决什么问题？

1. 问题定位：从“离线视频理解”转向“持续主动协助”
论文要解决的不是单次视频问答是否答对，而是流式视频中的主动交互策略：在连续观察与交互中，代理需要自行判断当前是否该说话、该说什么，并且这种判断要随着用户意图和环境状态变化而更新。引言以 Dewey 的“心智是活动/有机体与世界之间的动态交互过程”作为 framing（合理推断：作者借此强调主动交互是过程而非一次性识别），把问题从静态内容理解推向持续决策。

2. 三类现实挑战
（1）隐式意图：现有主动交互方法大多假设用户给出显式查询，例如“进球时通知我”。但真实场景中，用户可能只说“我卡住时帮我完成这个任务”，代理必须从持续观察和交互中推断意图、触发条件和协助内容。
（2）任务相关状态跟踪：先前交互会改变环境和任务进度，因此早先合适的响应，之后可能变得不必要甚至错误。现有流式模型虽然能保留较长交互历史，但“有历史”不等于“有最新状态”，容易出现重复响应、过时响应或漏响应。
（3）意图演化：用户意图会随交互展开而变化，后续反馈可能修正、扩展或替换早先目标。代理不能只跟踪环境状态，还必须维护、修订活跃意图集合。

3. 评测范式的错配
论文指出现有基准多面向显式、孤立、单轮请求，难以检验“持续交互中的状态一致性”。因此问题不仅是模型能力不足，也是评测设计不足：pointwise 评测在给定 gold history 下判断局部决策，容易隔离错误传播；trajectory 评测让模型用自生成历史跑完整流，才能暴露误差累积、状态漂移和重复响应。论文因此同时引入两种评测，并把响应时机与响应内容作为两个核心维度。

4. 隐含假设与研究缺口
论文的解法隐含几个假设：用户意图可以被表示为活跃意图集合；任务状态可以被显式跟踪并压缩；压缩后的记忆仍能保留实体、动作、约束和状态变化；模拟的流式基准足以代表真实开放世界。这些假设在片段中没有被完全验证。尤其是“压缩记忆是否丢失关键约束”“显式状态是否真正解决意图冲突”“LLM judge 能否稳定评价内容质量”等问题，仍是需要回原文和附录核实的方法学缺口。

5. 为什么值得作为独立问题看
主动视频代理连接了流式多模态感知、对话/agent 状态管理、长时记忆和强化学习决策。它不同于“视频问答”或“视频描述”：后者的输出是回答，前者的输出还包括沉默、等待、提醒、追问和干预。把“何时响应”与“响应什么”分开建模，是这篇论文最核心的问题重构。

Q2: 有哪些相关研究？

已提供片段未包含完整 Related Work 章节，以下依据 Introduction 的引用线索、baseline 分类和 Conclusion 中的术语重建；具体对比论断需回原文核实。

1. 离线视频理解与视频语言模型
MVBench、Video-MME、OVO-Bench、OVBench 等代表离线/通用视频理解评测。这类工作要求模型对给定视频片段回答问题或完成理解任务，强调感知、时序定位和语义推理，但不要求模型在持续流中自行决定何时介入。StreamVibe 的差异在于把“响应时机”作为一等公民，而不是只评价答案内容。

2. 流式/在线视频语言模型
MMDuet2、Mage-VL、LiveStar、ViSpeak、VideoLLM-Online 等被列为开源流式模型基线。它们支持在线观察、长历史或连续响应，但论文认为这些模型仍难以维持“最新任务状态”，从而产生重复、过时或漏掉的响应。换言之，长上下文能力不等于状态跟踪能力，这是本文与一般流式视频模型的关键分野。

3. 主动交互与个性化主动代理
General Timing、EgoProactive、IPIBench、JoyAI-VL-Interaction 等构成主动交互/个性化主动代理的参照系。已有工作多聚焦显式触发条件、单轮请求或特定个性化场景；StreamVibe 试图把显式意图、隐式意图和动态意图放进统一基准，并用 pointwise 与 trajectory 两种协议分别测局部决策和持续交互。

4. 对话状态跟踪、记忆增强代理与 RL 决策
对话状态跟踪、长时记忆、工具型 agent 和 LLM agent 强化学习是相邻研究线。StreamVibe-Harness 的 A_t/P_t 设计、记忆压缩和“局部轨迹 RL”明显与这些方向相关，但已提供片段没有展开与具体工作的逐项对比。合理推断：本文的差异化主张是“把状态跟踪外置为持久交互状态”，并用轨迹级奖励弥补逐点监督的不足。

5. 与本文最直接的区别
本文不是提出单一模型结构，而是同时提供基准、策略模型和状态跟踪 wrapper。其研究范式更接近“任务形式化 + 评测协议 + 系统级方法”的组合，而不是只在某个视频理解榜单上刷点。这也意味着它的贡献需要从问题定义、基准有效性、模型能力和系统增益四个层面分别评价。

Q3: 论文如何解决这个问题？

1. 总体架构：三层解耦
StreamVibe 由三部分组成：StreamVibe-Bench 定义任务与评测；StreamVibe-VL 是端到端主动流式策略；StreamVibe-Harness 是外置的意图/任务状态跟踪层。三者分别回答“怎么测”“怎么学”“怎么维持状态”。

2. StreamVibe-Bench：统一意图层级与双协议评测
（1）Level 1 显式意图：用户明确指定要监控的条件、事件或状态，代理在条件满足时响应，例如“水开始沸腾时告诉我”。这类任务接近传统事件触发，难点在时序判断。
（2）Level 2 隐式意图：用户不给出显式触发条件，代理需从持续观察和交互中推断何时需要帮助，例如“我卡住时帮我完成这个任务”。这类任务要求意图推断和场景理解结合。
（3）Level 3 动态意图：结论明确提到 benchmark 覆盖 explicit、implicit、dynamic intents。合理推断：动态意图指意图随交互反馈而演化，代理需要修订活跃意图，而不是执行固定触发规则。
（4）评测协议：pointwise 评测在给定 gold interaction history 下逐点判断决策，隔离历史误差；trajectory 评测让模型在完整流上用自己的自生成历史滚动，检验误差累积和持续一致性。指标包括响应时机与内容质量，使用 ±2 s 容差、Content F1 等；片段还出现 Claude Sonnet 4.6，合理推断其用于内容质量评判。

3. StreamVibe-VL：自适应流式 SFT + 局部轨迹 RL
（1）输入与输出：模型观察视频流和交互历史，在每个决策点输出响应或保持沉默。它学习的不是“给定问题找答案”，而是“在流中何时介入、介入时说什么”的策略。
（2）adaptive streaming SFT：用流式决策数据做监督微调，使模型适配在线观察和响应格式。“adaptive”在片段中没有展开，合理推断其涉及自适应历史/上下文组织或训练课程，但具体机制需回原文。
（3）localized trajectory RL：在轨迹层面优化策略，重点改善依赖先前历史的连续决策。“localized”合理推断为围绕局部决策窗口做信用分配，而非只优化整条轨迹的稀疏终局奖励；具体 reward、采样和损失未在片段中给出。
（4）训练效果：Discussion 明确指出 RL 相对 SFT 有进一步提升，且在 dynamic intents 上增益更大；在 trajectory evaluation 中，RL 带来更高 Content F1 和 response quality，说明轨迹级优化对“依赖历史”的持续交互更关键。

4. StreamVibe-Harness：把意图和任务进度变成持久状态
（1）状态表示：Harness 维护活跃意图 A_t 和任务状态 P_t。前者记录当前仍有效的用户目标，后者记录任务进度、环境变化和约束。
（2）记忆压缩：近期信息保留细粒度，较旧内容逐步压缩，但压缩时尽量保留任务相关实体、动作、约束和状态变化。这相当于在有限上下文里维护“任务相关状态”，而不是简单保留全部历史。
（3）响应门控：根据 A_t 和 P_t 判断当前响应是否仍然有效，从而减少过时、冗余和状态不一致的输出。Discussion 指出，显式状态跟踪能进一步降低 outdated/redundant responses，且 Harness 的增益超出端到端策略本身。

5. 设计权衡
（1）显式状态 vs 端到端策略：外置 Harness 提高可解释性和状态一致性，但可能引入压缩误差和模块间不一致；端到端模型更灵活，但难以保证长期状态。
（2）SFT vs RL：SFT 提供稳定的流式行为先验，RL 针对轨迹结果做优化；两者结合比单独 SFT 更好，尤其对动态意图。
（3）pointwise vs trajectory：前者便于诊断局部决策，后者更接近部署；两者结合能区分“模型不会做单步判断”与“模型不会维持长期状态”。

6. 证据缺口
已提供片段没有 Method 正文，因此无法确认具体网络结构、损失函数、reward 设计、训练数据规模、Harness 是否可学习、推理延迟等。上述机制中，凡涉及“如何实现”的细节均应视为基于命名和上下文的合理推断，而不是论文已明确陈述的事实。

Q4: 论文做了哪些实验？

1. 评测矩阵
论文在两个大设置上评测：（i）Proactive Interaction，使用 StreamVibe-Bench、General Timing、EgoProactive、IPIBench；（ii）General Video Understanding，使用 MVBench、Video-MME、OVO-Bench、OVBench。外部套件保留官方协议，说明作者既想验证主动交互能力，也想检查通用视频理解是否退化。

2. 基线覆盖三类模型
（1）专有模型：Gemini-3.1-Pro、Gemini-3.8-Flash、GPT-Realtime-2.1；
（2）开源离线模型：Qwen3-VL-8B、Qwen3.5-9B、JoyAI-VL-Interaction；
（3）开源流式模型：MMDuet2、Mage-VL、LiveStar、ViSpeak、VideoLLM-Online。
这种分层设计可以区分“强感知但非流式”“流式但状态弱”“专有闭源强基线”三类差距。

3. 评测协议
pointwise evaluation 在给定 gold interaction history 下评价每个决策；trajectory evaluation 让模型在完整流上用自己的自生成历史滚动。Table 2 注明 streaming models 不支持 pointwise evaluation，因此只报告 trajectory 结果。这意味着跨模型类别比较时必须注意协议差异：离线模型可以在 gold history 下逐点判断，流式模型则被放在更接近真实部署的轨迹协议中。

4. RQ1：意图感知主动交互
RQ1 问 top-performing baseline 表现如何，以及 StreamVibe-VL 能否在 intent-aware interaction 设置中超过它们。对应结果是 Table 2、Figure 4(a-b) 以及外部基准。作者报告 StreamVibe-VL 在 StreamVibe-Bench 上持续优于开源基线，pointwise 下取得最高 Content F1，response quality 与专有模型具有竞争力；在 EgoProactive 上表现有竞争力，在 IPIBench 上相对其他模型有一致增益。

5. RQ2：持续主动交互的失败模式
RQ2 问当前主动代理在 sustained proactive interaction 中表现出哪些局限。Discussion 的图注和文字指出，现有代理在连续交互中难以维持可靠行为，出现重复、过时或状态不一致的响应；duration sensitivity 分析显示交互时长会影响表现。这部分是论文的“负结果/诊断”贡献，而不仅是排名。

6. RQ3：Harness 的额外增益
RQ3 问 StreamVibe-Harness 相对端到端策略还能带来什么。作者报告显式状态跟踪进一步减少 outdated、redundant、state-inconsistent 输出，说明“把意图和任务进度外置为持久状态”不是装饰性模块，而是对持续交互有独立贡献。

7. 分析实验
已提供片段提到 duration sensitivity、StreamVibe-SFT − Base 等分析，以及 Figure 4 的外部基准结果。合理推断：作者还做了 SFT/RL 对比、长时稳定性分析、Harness 消融和定性案例（Appendix D.2 出现做面包卷、图像加载与去饱和等例子）。但片段没有给出具体数值、显著性检验、数据规模或人工一致性，因此不能据此判断效应量。

8. 评测公平性与缺口
（1）streaming models 只做 trajectory，offline/proprietary 可能做 pointwise，跨类比较需要谨慎；
（2）内容质量依赖 LLM judge（片段出现 Claude Sonnet 4.6），存在裁判偏差风险；
（3）±2 s 容差对时机评价很关键，但片段未说明不同任务是否采用不同容差；
（4）外部基准的“competitive/gains”是方向性描述，缺少具体分数和方差；
（5）未看到计算成本、延迟、吞吐等部署指标。

Q5: 发现了什么实验现象？

1. 开源流式模型在意图感知主动交互上整体弱于 StreamVibe-VL
Table 2 的方向性结论是：StreamVibe-VL 在 StreamVibe-Bench 上持续优于开源基线；pointwise 下 Content F1 最高，response quality 与专有模型具有竞争力。这说明“主动交互”不是单纯靠更强的视频感知就能解决，流式策略和状态建模有独立贡献。

2. RL 的收益集中在动态意图
Discussion 明确说 RL 相对 SFT 有进一步提升，且在 dynamic intents 上增益更大。这是一个有信息量的现象：当意图固定时，SFT 可能已能学到触发模式；当意图随交互变化时，轨迹级优化和信用分配更重要。合理推断：动态意图任务更依赖“根据历史修订决策”，因此 RL 的优势被放大。

3. Trajectory 评测放大模型差异
在 trajectory evaluation 中，RL 带来更高 Content F1 和 response quality，尤其是决策依赖先前历史时。这暗示 pointwise 评测可能高估模型：给定 gold history 时，模型不需要处理自己前一步的错误；一旦用自生成历史，误差累积、状态漂移和重复响应就会暴露。这是论文评测设计最有价值的方法学观察之一。

4. 外部迁移并非全面碾压，而是“有竞争力 + 局部增益”
作者报告在 EgoProactive 上表现有竞争力，在 IPIBench 上相对其他模型有一致增益。这里的措辞是 competitive/gains，而非全面 SOTA。合理推断：不同外部基准的意图分布和交互协议差异较大，StreamVibe 的迁移效果取决于任务与训练分布的接近程度。

5. 通用视频理解没有被明显牺牲
论文把 MVBench、Video-MME、OVO-Bench、OVBench 纳入评测，并结论说模型在改善响应时机与质量的同时保持通用视频理解。这是一个重要的权衡结果：主动交互训练没有导致严重的感知能力遗忘。但片段没有给出具体退化/提升幅度，不能判断“保持”是统计显著还是方向性。

6. Harness 的增益是“减少过时与冗余”，而非彻底解决状态问题
Discussion 指出显式状态跟踪帮助保留有效意图和任务进度，从而减少 outdated、redundant、state-inconsistent 输出。与此同时，RQ2 又指出当前代理在持续交互中仍难以维持可靠行为。两者合起来说明：Harness 是有效缓解，但不是完全解决；长期状态跟踪仍是开放问题。

7. 时长敏感性：交互越长，维持一致越难
片段出现 duration sensitivity 分析。合理推断：随着交互时长增加，历史压缩误差、意图漂移和状态更新延迟会累积，模型表现下降。这与“长上下文不等于最新状态”的问题判断一致。具体下降曲线和拐点未在片段中给出。

8. 指标之间存在张力
论文同时看 response timing 和 content quality，并使用 Content F1、Macro Accuracy 等指标。实验中可能出现“时机对但内容错”“内容好但打断过多”“响应质量高但漏报”等不同失败模式。片段中的定性例子（做面包卷、图像加载与去饱和）说明任务是多步骤的，单看一个指标会掩盖另一类错误。

9. 定性案例展示了“持续协助”而非单轮问答
Appendix D.2 的例子涉及做面包卷、加载与去饱和图像等多步骤过程。合理推断：这些案例用于展示代理如何在任务中途给出提示、何时保持沉默，以及 Harness 如何避免重复提醒。但片段没有给出失败案例的完整对照，需回附录核实。

10. 证据强度限制
上述现象均为片段中的方向性结论；已提供材料没有具体分数、置信区间、显著性检验或人工评估一致性。因此可以据此判断“作者声称什么趋势”，但不能据此判断效应量大小或复现稳定性。

Q6: 有什么可以进一步探索的点？

1. 论文明确提出的方向：意图与环境的共同演化
Conclusion 指出，真实主动交互可能包含比当前设定更丰富的 intent evolution，以及用户需求与环境变化之间更紧的耦合。新观察、任务结果和先前协助可能共同重塑用户需求与环境，产生新目标和新约束。未来应研究这种 co-evolving interaction，尤其是意图与环境状态之间在更长时程上的复杂反馈。

2. 更长时程的状态跟踪与记忆修订
当前 Harness 通过压缩记忆保留任务相关实体、动作、约束和状态变化，但长时交互仍会出现过时、冗余和状态不一致。可探索：可学习的压缩策略、状态冲突检测与修订、遗忘机制、事件级记忆索引、跨任务记忆迁移，以及“什么时候该忘记”的策略。

3. 更丰富的意图表示
显式/隐式/动态三层是有用抽象，但真实意图可能并行、冲突、含糊或临时变化。可探索：意图的不确定性建模、多用户意图、优先级与冲突消解、意图修订的因果记录、用户反馈的信用分配。

4. 评测方法的改进
（1）减少对单一 LLM judge 的依赖，加入人类评估、成对比较和裁判一致性分析；
（2）设计反事实评测：如果某条历史被修改，响应是否应改变；
（3）区分“时机错误”和“内容错误”的细粒度指标；
（4）报告方差、置信区间和显著性；
（5）在真实在线流中评测，而不仅在脚本化 benchmark 上。

5. 训练与优化
可探索：过程奖励模型、局部信用分配、离线 RL 与在线交互 RL、课程学习（从显式到隐式到动态意图）、对沉默/打断的成本敏感奖励、以及 SFT 与 RL 的更优混合比例。片段中的 localized trajectory RL 只是方向，具体 reward 设计仍有很大空间。

6. 部署与系统问题
always-on 视频代理涉及延迟、算力、隐私、误触发成本和用户信任。可探索：边缘端流式推理、分层唤醒、隐私-preserving 状态表示、可解释的响应理由、用户可控的打断策略。这些在片段中未展开，属于合理的相邻问题。

7. 跨领域迁移
合理推测：同样的“意图演化 + 状态跟踪 + 主动干预”框架可迁移到教学辅导、烹饪助手、设备维修、实验室操作监控、远程协作等场景。若关注 AI4Science，可把它看作“实验流程中的主动监测与提示”这一相邻问题，但论文本身并未以科学应用为实验场景。

Q7: 总结一下论文的主要内容

一、论文的论证主线
论文从主动交互范式切入：视觉语言模型正从离线内容理解走向真实流中的持续主动协助，但现有主动交互方法大多面向显式查询和孤立单轮请求，例如“进球时通知我”。这类设定假设用户意图事先明确、稳定，且响应可以在单点判断。作者认为真实开放场景不满足这三个假设，并提出三类挑战：隐式意图需要从持续观察中推断；任务相关状态会因先前交互而改变，长历史不等于最新状态；用户意图会随交互演化。由此，论文把问题重构为“在流中持续判断何时响应、响应什么，并随意图和环境更新”的策略问题。

二、技术主线
作者提出 StreamVibe 三层框架。第一层是 StreamVibe-Bench：覆盖显式意图、隐式意图和动态意图，并提供 pointwise 与 trajectory 两种评测。pointwise 在 gold history 下评价局部决策，trajectory 让模型用自生成历史跑完整流，以检验误差累积和持续一致性；指标包括响应时机与内容质量，使用 ±2 s 容差和 Content F1 等。第二层是 StreamVibe-VL：一个主动流式策略模型，先用 adaptive streaming SFT 学习在线响应行为，再用 localized trajectory RL 优化轨迹级结果。第三层是 StreamVibe-Harness：维护活跃意图 A_t 和任务状态 P_t，近期信息细粒度保留，较旧内容压缩但保留任务相关实体、动作、约束和状态变化，从而减少过时、冗余和状态不一致的响应。

三、实验主线
论文用三个 RQ 组织实验。RQ1 问 StreamVibe-VL 能否在意图感知主动交互中超过强基线；RQ2 问当前主动代理在持续交互中有哪些失败模式；RQ3 问 Harness 相对端到端策略还有什么额外增益。评测覆盖 StreamVibe-Bench、General Timing、EgoProactive、IPIBench 四个主动交互套件，以及 MVBench、Video-MME、OVO-Bench、OVBench 四个通用视频理解套件。基线包括 Gemini-3.1-Pro、Gemini-3.8-Flash、GPT-Realtime-2.1 等专有模型，Qwen3-VL-8B、Qwen3.5-9B、JoyAI-VL-Interaction 等开源离线模型，以及 MMDuet2、Mage-VL、LiveStar、ViSpeak、VideoLLM-Online 等开源流式模型。

四、主要发现
作者报告：StreamVibe-VL 在 StreamVibe-Bench 上持续优于开源基线，pointwise 下 Content F1 最高，response quality 与专有模型具有竞争力；RL 相对 SFT 有进一步提升，且在动态意图上增益更大；trajectory 评测中 RL 带来更高 Content F1 和 response quality，说明轨迹级优化对依赖历史的持续交互更关键；外部基准上，EgoProactive 表现有竞争力，IPIBench 有一致增益；通用视频理解得到保持。Harness 的消融/分析显示，显式状态跟踪能进一步减少过时、冗余和状态不一致的输出。与此同时，RQ2 的诊断表明当前主动代理在长时间连续交互中仍难以维持可靠行为，时长敏感性分析也提示交互越长越难。

五、边界与意义
论文的贡献不是单一模型刷榜，而是把“主动视频交互”拆成任务定义、评测协议、策略学习和状态跟踪四个可分别检验的部分。其最有价值的方法学观点是：pointwise 与 trajectory 评测回答不同问题，长上下文不等于最新状态，显式状态跟踪可以独立于端到端策略提供增益。局限也清楚：真实意图演化与意图-环境共同演化尚未覆盖；持续交互可靠性仍是开放问题；评测依赖脚本化基准和 LLM judge；数据规模、奖励设计、延迟与隐私等部署维度在已提供片段中缺失。整体上，这是一篇适合从“agent 长期状态管理 + 流式多模态决策 + 评测设计”角度精读的系统性工作。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：论文处理的是长期状态、主动决策、何时介入、如何根据反馈修订意图，这些正是工具型/具身型 agent 的核心问题。

## 基本信息

- 作者：Chengzhi Liu, Yuxiang Lai, Yuzhe Yang, Yanqing Liu, Jiheng Hou, Yue Cao, Haonan Wang, Ruixi Fan, Lan Wang, Sheng Liu, Songyou Peng, Yale Song, Kevin Qinghong Lin, Xin Eric Wang, Jui-Hsin Lai, Youbao Tang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-27
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.34662`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Introduction、Results、Discussion、Conclusion 片段；Method 正文与附录未提供，凡涉及训练损失、reward、数据规模和具体数值之处均按片段线索标注为推断或缺口。
