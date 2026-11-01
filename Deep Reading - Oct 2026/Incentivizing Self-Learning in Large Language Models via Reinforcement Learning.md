---
user_id: "cheng tan"
paper_id: 19358
arxiv_id: "2610.39671v1"
title: "Incentivizing Self-Learning in Large Language Models via Reinforcement Learning"
institution: "上海人工智能实验室（Shanghai AI Laboratory）相关团队（推测；依据 Intern-S2-Preview 模型命名与作者名单，给定片段未显式标注机构，需以原文首页为准）。"
publish_date: "2026-10-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.39671v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.39671v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-01T23:11:30"
---
# Incentivizing Self-Learning in Large Language Models via Reinforcement Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-learning · reinforcement learning · long-context · textbook navigation

## 一句话总结

本文把「自主在长教材中定位、学习并应用知识」形式化为教材驱动的数学证明任务，并用 BookNavigator 接口与细粒度 agentic RL 训练 LLM，使其在未见教材、未见学科及部分长上下文任务上取得提升。

## 摘要

> Large language models (LLMs) draw on parametric knowledge and retrieval-augmented generation (RAG) to solve knowledge-intensive tasks. Yet problem solving in unfamiliar domains calls for tightly coupled knowledge acquisition and reasoning, beyond factual retrieval and aggregation or learning solely from a preassembled context. We study self-learning -- autonomously retrieving, studying, and applying knowledge from long-form reference materials on the fly. To investigate this capability, we formulate a textbook-based mathematical problem-solving task. LLMs must navigate textbook exposition, learn and apply relevant principles, and construct proofs for exercises, with reference solutions withheld. We introduce a BookNavigator interface to support this human-inspired study process and incentivize self-learning through reinforcement learning (RL) with fine-grained rewards. Experiments show that our method improves performance across in-domain and out-of-domain textbooks and subjects, raising Intern-S2-Preview-35B's overall proof accuracy from 51.5% to 79.5% and outperforming RL-trained textbook-free and fixed-retrieval baselines. These gains extend to complex long-context benchmarks involving different task types, with improvements over the base model of 1.7 and 2.3 percentage points on BrowseComp-Plus and CL-bench, respectively. Together, these findings suggest that RL can foster a domain-agnostic self-learning strategy, highlighting the potential of self-learning from long-form corpora to strengthen broader problem-solving capabilities on complex, long-context tasks.

Q1: 这篇论文试图解决什么问题？

一、核心问题不是「LLM 缺少知识」，而是「LLM 缺少在任务进行中自主补足知识并立即用于推理的学习策略」。论文把 self-learning 定义为：模型面对不熟悉问题时，能从长篇参考资料中自主定位相关内容、理解新原理、判断其适用条件，并把学到的知识整合进正在形成的解答。这个定义刻意区分了三件事：参数化知识、检索到的离散事实、以及被模型真正消化并用于推导的知识。

二、现有路线为何不够。RAG 与搜索工具通常把检索当作一次性前置步骤，返回的是事实片段或答案线索，无法保证模型理解定理的前提、适用范围和证明结构；长上下文直接把 208k tokens 量级的教材塞进上下文，会带来注意力稀释、章节依赖丢失和无关内容干扰；context-learning 研究的是从给定材料中获取知识，但材料由外部预先组装，模型不决定「学什么、何时学、学到什么程度」；RL 训练的检索代理多把搜索与推理交错用于答案生成，其奖励仍以最终答案为中心，未必能鼓励持续、连贯的教材研读；meta-RL 与 test-time adaptation 依赖重复尝试或环境反馈，而本文设定是静态参考材料，模型必须自己决定学习路径。

三、技术难点。该任务是一个带稀疏延迟奖励的序贯决策问题：模型每一轮先写推理，再选择教材访问动作，观察返回内容，然后继续推理；最终只有证明正确性这一个主要结果信号。于是出现信用分配问题：哪一次教材访问真正帮助了后续推导？也出现探索问题：模型可能完全不访问教材，退化为纯参数化作答；也可能反复访问、把工具调用当作拖延或刷奖励的手段。教材结构长而复杂，模型还要学会章节定位、跨节依赖追踪和在证据不足时回溯。

四、评价难点。若只看同分布题目，无法区分模型是「学会了学习」还是「记住了教材与题型」。论文因此设置 In-Domain Book、Out-of-Domain Book、Out-of-Domain Subject 三级拆分，并进一步看 BrowseComp-Plus 与 CL-bench 的任务迁移。但最终证明正确性由 LLM-as-judge 判定，证明的严谨性、步骤遗漏、循环论证和幻觉引用仍是效度风险；教材与习题是否出现在预训练语料中，也需要额外核查。

五、隐含假设。论文默认教材包含解题所需知识，结构化章节足以支撑导航，八轮交互足以完成学习，LLM judge 与人类证明质量高度相关，且模型最终能输出自包含证明。这些假设在数学教材场景下较合理，但推广到科研论文、实验手册或动态知识库时都需要重新验证。

Q2: 有哪些相关研究？

一、参数化知识与检索增强生成。论文把 LLM 的预训练知识视为 breadth 的来源，但指出它无法覆盖不熟悉领域的即时学习需求；RAG 和搜索工具扩展了推理时信息访问，却主要解决「找到相关事实」而非「学会并应用原理」。本文与这类工作的关键差别是：检索不是一次性前置阶段，而是与推理交替进行、由模型自主决定的学习动作。

二、RL 训练的检索/搜索代理。已有工作让模型交错进行搜索与推理，并通过 RL 优化最终答案；这类方法证明了工具调用可以被 RL 习得，但目标通常是答案生成，检索粒度和学习深度有限。本文把动作空间限制在结构化教材上，强调章节级导航、长文研读和证明构建，并用轮级 tool credit 缓解稀疏奖励。

三、上下文学习与长上下文模型。Context-learning 研究模型如何从 supplied materials 中获取知识，长上下文模型则试图直接容纳更多材料；但二者通常不要求模型自主选择材料、规划学习顺序或在多轮交互中逐步补齐先修知识。本文把「教材」做成可操作环境，让模型通过 BookNavigator 主动获取信息，而不是被动阅读一整段上下文。

四、Meta-RL 与 test-time adaptation。这类方法让模型从环境观察、动作结果或重复尝试中适应新任务；本文的设定更接近「在静态长文中自主学习」，没有外部教师给出逐步反馈，也没有重复尝试同一题的机会，因此需要不同的探索与信用分配机制。

五、数学证明与 LLM-as-judge。数学证明任务提供可验证的最终答案和清晰的教材依赖，适合作为 self-learning 的试验台；但证明正确性判定比选择题或短答案更复杂，现有工作常依赖 LLM judge 或人工评估。本文使用 LLM-as-judge 评估最终证明，并把参考解答从工具评判中隐藏，以降低泄漏，但 judge 偏差仍是相关研究共同的效度问题。

六、长上下文基准。BrowseComp-Plus 与 CL-bench 被用来检验任务类型迁移；它们涉及长上下文信息整合，但与教材证明的交互协议不同。本文在这两个基准上使用原始推理设置而非 BookNavigator，因此其提升更适合解释为「能力迁移」，而不是「接口直接复用」。

Q3: 论文如何解决这个问题？

一、任务形式化。给定数学证明题 q 与结构化教材 B，模型需要进行多轮「推理—访问教材—观察—再推理」，最终输出自包含证明 y_hat。非终止轮 t 中，策略先生成推理 z_t，再从教材访问动作空间 A 中选择并执行动作 a_t，获得教材信息 o_t；终止轮不再访问教材，直接完成证明。完整轨迹可写为 z_1,a_1,o_1,…,z_T,y_hat。最终证明由 LLM-as-judge 判定正确性。这个形式化的关键是把「学习」显式建模为动作序列，而不是把教材内容一次性放进 prompt。

二、数据构建。作者从 158 本本科与研究生数学教材中抽取 5,183 道题，覆盖分析、代数、数论、几何等方向；每本教材平均 56 节、208k tokens，构成跨章节依赖的长文学习环境。训练集为 132 本教材的 4,557 道题；评测划分为 In-Domain Book（训练中出现的 4 本教材，205 题）、Out-of-Domain Book（未出现的 7 本教材但学科见过，155 题）、Out-of-Domain Subject（概率统计方向 19 本教材，266 题）。论文还使用端到端流水线从原始教材构造题目—教材对，但给定片段未展开清洗、去重、答案隐藏等细节。

三、BookNavigator 接口。BookNavigator 是模型与教材之间的唯一接口，教材被组织为若干 section 的结构化对象；检索片段中出现形如 (ι_k, ℓ_k, x_k) 的 section 表示，合理推断为章节标识、标题与正文，但具体字段定义需回原文确认。接口支持三种互补的学习操作，片段只露出 orienting 开头，完整动作名称与语义需查原文；可以确定的是，模型不是直接拿到整本教材，而是通过受限动作主动获取局部信息。实验中最多允许八轮交互，这既是效率约束，也是学习策略的边界条件。

四、细粒度 RL 奖励。论文的核心训练设计有三层：第一，最终证明正确性提供结果奖励；第二，引入轮级 tool credit，对每次教材访问是否在后续推理中产生作用做细粒度评估；第三，加入退火式探索奖励，鼓励早期探索教材，避免模型过早退化为纯参数化作答。检索片段明确提到 exploration bonus 使用 cosine schedule 并保留正的下界，使早期探索激励更强、后期仍有少量教材访问激励。

五、tool judge 与防泄漏。根据 heuristic_draft 中与 Method 对齐的片段，tool credit 可能由初始策略的冻结副本评估单轮交互，并采用回顾式判断：某轮检索材料是否在后续推理中被使用；仅因没有立即使用而不应直接判负，以适应延迟使用。参考解答与最终正确性 verdict 对 tool judge 隐藏，避免 judge 直接按答案泄漏或结果倒推来给工具调用打分。该细节在检索片段中只部分可见，具体 judge prompt、评分尺度与冻结策略实现需回原文核对。

六、训练与推理缺口。给定片段没有说明具体 RL 算法、KL 正则、采样数量、训练步数、奖励权重、工具调用失败处理、上下文截断策略和计算成本；这些是实现可复现性和公平比较的关键，阅读时应优先查附录与实验设置。

Q4: 论文做了哪些实验？

一、数据与评测划分。论文收集 158 本本科/研究生数学教材、5,183 道题，每本教材平均 56 节、208k tokens。训练使用 132 本教材的 4,557 题；评测分为三类：In-Domain Book 为训练中出现的 4 本教材的 205 道留出题；Out-of-Domain Book 为未参与训练的 7 本教材的 155 题，但学科在训练中出现；Out-of-Domain Subject 为概率统计方向的 19 本教材的 266 题，教材与学科均与任务训练分离。这个设计分别检验新题、新教材、新学科三个层级的迁移。

二、问题解决协议。论文比较 Self-Learning 与 No-Book，以及三种 fixed-retrieval 基线。Self-Learning 通过 BookNavigator 将推理与教材访问交错进行，最多八轮交互；No-Book 不允许访问教材；fixed-retrieval 基线预先检索教材内容再作答，不进行自主多轮导航。摘要明确指出基线是 RL-trained textbook-free 与 fixed-retrieval，因此比较重点不是「RL 有没有用」，而是「在同样 RL 训练下，自主教材导航是否优于无书或固定检索」。

三、主结果。摘要报告 Intern-S2-Preview-35B 的总体证明准确率从 51.5% 提升到 79.5%，并称在 in-domain 与 out-of-domain 教材、学科上均有提升，且优于 textbook-free 和 fixed-retrieval 基线。论文还报告在 BrowseComp-Plus 上相对 base model 提升 1.7 个百分点，在 CL-bench 上提升 2.3 个百分点。给定片段没有给出各 split 的逐项数值、方差、显著性检验和 baseline 的完整配置，因此不能编造更细的表格结果。

四、机制分析。Results 片段显示作者做了参考材料干预：移除与题目相关的教材邻域，并与「随机移除九个邻域外 section」的 section 数量匹配控制组比较；其他评测设置保持不变。该分析用于判断性能是否真的依赖被移除的内容，而不仅是教材变短或格式变化。片段还提到，当参考材料变化时，策略可能从替代教材或源邻域之外寻找先修知识，但被移除内容的位置仍然重要。

五、无工具推理分析。Results 片段明确写到 RL 将 No-Book 准确率从 55.7% 提升，但截断处没有给出提升后的数值。这说明训练后的模型即使不允许访问教材，也可能获得推理能力增益，而不仅是学会调用 BookNavigator；但该数字与 51.5%→79.5% 的总体口径是否可直接比较，需要查表确认。

六、迁移评测协议。BrowseComp-Plus 使用 BM25 检索，CL-bench 使用从给定上下文直接生成且不进行工具调用，二者都沿用原始推理设置而非 BookNavigator。这意味着外部基准上的提升更可能反映模型一般长上下文/推理能力的迁移，而不是教材导航接口的直接迁移；同时 1.7 与 2.3 个百分点的幅度较小，应结合方差和任务难度解读。

Q5: 发现了什么实验现象？

一、自主教材导航带来的主效应很大。总体证明准确率 51.5%→79.5% 是论文最醒目的现象，且作者强调其优于 RL-trained textbook-free 与 fixed-retrieval 基线，说明「让模型自己决定学什么」可能比「把检索结果一次性喂给模型」更有效。但该结论依赖基线是否共享同等 token 预算、同等 RL 训练量和同等教材内容，给定片段不足以完全排除这些混杂。

二、初始模型可能并不擅长使用教材接口。摘要给出 base 的总体准确率为 51.5%，而 Results 片段又提到 No-Book base 准确率为 55.7%；若二者是同一模型、同一总体口径，则初始模型在带教材接口时反而低于不看书，合理推断是长教材导航带来了接口学习成本与上下文负担。RL 后 79.5% 的带书准确率说明训练可能同时教会了模型「使用接口」和「利用教材」，但这一解释需要表格中的逐项对比确认。

三、无工具推理也被提升。片段明确说 RL 将 No-Book 准确率从 55.7% 提升，虽然提升后的数值被截断。这是一个值得注意的现象：增益并非只出现在允许调用 BookNavigator 时，可能说明 RL 训练改善了基础推理、证明组织或自我检查能力。若成立，则论文的因果叙事就不能简单归结为「教材访问带来全部提升」，而应拆分工具使用增益与一般推理增益。

四、跨教材与跨学科迁移存在，但幅度不透明。摘要称 in-domain 与 out-of-domain 教材、学科均有提升，这支持「学到的是一种学习策略而非单纯记住某本教材」的解释；但给定片段没有给出 OOD Book 与 OOD Subject 的具体数字，无法判断迁移是接近 in-domain 的强迁移，还是小幅正向。OOD Subject 是概率统计，仍属数学内部迁移，不能直接外推到物理、生物、法律或人文文本。

五、外部长上下文基准上的增益较小且协议不同。BrowseComp-Plus +1.7、CL-bench +2.3 个百分点是正向信号，但二者没有使用 BookNavigator：BrowseComp-Plus 用 BM25 检索，CL-bench 直接基于给定上下文生成。因此这更像是「RL 训练后的模型在别的长上下文任务上略好」，而不是「BookNavigator 策略直接迁移」。小幅度提升也提醒读者关注统计显著性和任务间异质性。

六、参考材料移除实验揭示了补偿路径。移除相关教材邻域后，策略可能从替代教材或源邻域之外寻找先修知识，说明模型学到了一定的鲁棒搜索与替代路径；但被移除内容的位置仍然重要，且控制组只是随机移除九个邻域外 section，无法完全排除「教材变短/结构变化」的影响。该实验是支持内容依赖性的证据，但不是干净的因果隔离。

七、奖励设计对应两类失败模式。探索奖励针对「模型不使用教材」的退化；tool credit 针对「模型乱调用教材但不用其内容」的退化。检索片段提到，单轮没有立即使用不应直接判负，这说明作者意识到延迟信用分配；但反过来，tool judge 可能把「看起来相关」的检索误判为有用，或奖励冗长、看似勤奋的访问行为，形成 reward hacking。给定片段没有给出工具调用成功率、无效调用比例或失败案例统计，需查附录。

八、指标之间存在需要解释的张力。证明准确率由 LLM-as-judge 给出，而外部基准是答案/任务指标；两者不是同一构念。若 judge 对证明风格、长度或格式敏感，主结果可能部分反映「更像标准证明」而非逻辑更严谨。论文需要用人类专家复核、形式化验证或 judge 一致性分析来补强，否则 51.5→79.5 的幅度虽大，仍需谨慎解读。

Q6: 有什么可以进一步探索的点？

一、机制层面：查清 RL 到底学到了什么。可以做导航策略探针、章节访问序列分析、注意力/检索位置分析和消融，区分「学会定位教材」与「学会更稳的证明生成」；尤其要解释为何 No-Book 准确率也会提升，以及带书增益中有多少来自工具使用、多少来自一般推理。

二、奖励与信用分配：用形式化验证器（如 Lean/Coq）或人类专家给最终证明提供更可靠奖励；用反事实工具调用、信息增益、Shapley 式归因或学习到的 process reward model 替代当前 tool judge；研究延迟使用、跨轮依赖和多工具调用的信用分配，避免奖励「看起来有用」的检索。

三、接口与动作空间：把 BookNavigator 扩展到层次化目录、关键词搜索、跨书引用、笔记记录、图表阅读、公式检索和引用回溯；研究主动停止、回溯重读、做摘要、自我提问等学习行为；比较不同动作粒度对长文导航和推理效率的影响。

四、领域扩展：把教材替换为科研论文、实验手册、法律判例、医学指南或开源代码库，检验 self-learning 是否能跨出数学；进一步加入多模态图表、动态网页和相互冲突的来源，研究模型如何评估来源可靠性与证据冲突。对 AI-for-Science，关键测试是从论文方法部分学习实验流程并应用到新问题，而不只是做证明题。

五、评测设计：建立无污染的新教材/新论文测试集，按出版时间或来源划分，避免预训练记忆；加入人类专家评分、形式化证明检查、对抗性错误证明和 judge 间一致性分析；报告工具调用次数、token 成本、延迟、失败率和方差，而不仅是最终准确率。

六、规模与训练：研究模型规模、教材数量、交互预算、RL 算法和奖励权重之间的 scaling trend；比较不同探索策略、课程学习和冷启动数据对 self-learning 的影响；验证该方法在更小模型上是否仍能学会导航，还是依赖 35B 模型的基座能力。

七、安全与治理：教材/论文来源的版权、可信度和更新问题；模型可能伪造引用、误读权威文本或把错误前提学进证明；需要研究来源溯源、引用验证、知识冲突处理和防 reward hacking 的训练约束。

八、理论理解：把该设定形式化为带信息获取的序贯决策问题，分析探索奖励、延迟奖励和工具信用分配的最优性条件；理解为何「学习如何学习」会迁移到新教材和新任务，以及这种迁移的边界在哪里。

Q7: 总结一下论文的主要内容

一、论文要解决的问题。论文关注 LLM 在不熟悉领域中的 self-learning：模型不能只依赖参数化知识，也不能只做一次性 RAG 或被动阅读预先拼好的长上下文；它需要在一个任务进行过程中，自主决定从长篇参考资料里学什么、何时学、如何把学到的原理用于当前推导。作者把这一能力与「事实检索」「上下文学习」「重复试错式适应」区分开，强调知识获取与推理必须持续相互塑造：一次失败的推导可能暴露缺失的先修知识，而新读到的定理又可能改变解题策略。

二、任务与数据。论文选择教材驱动的数学证明作为试验台：模型拿到一道证明题和一本结构化教材，参考解答不可见，需要通过多轮推理与教材访问构建自包含证明。数据来自 158 本本科/研究生数学教材，覆盖分析、代数、数论、几何等方向，共 5,183 道题；每本教材平均 56 节、208k tokens，形成跨章节依赖的长文环境。训练集为 132 本教材的 4,557 题；评测分为 In-Domain Book（4 本训练教材的 205 道留出题）、Out-of-Domain Book（7 本未训练教材的 155 题，学科见过）、Out-of-Domain Subject（概率统计方向 19 本教材的 266 题），分别检验新题、新教材、新学科。

三、技术主线。作者把交互写成多轮轨迹：每一轮模型先生成推理 z_t，再选择教材访问动作 a_t，获得观察 o_t；终止轮输出最终证明 y_hat。BookNavigator 是模型与教材之间的唯一接口，教材被组织为结构化 section，模型通过受限动作主动获取局部信息，而不是直接吞下整本教材；实验中最多允许八轮交互。

四、训练主线。RL 奖励不是只有最终正确性：作者加入轮级 tool credit，评估某次教材访问是否在后续推理中真正发挥作用；加入退火式探索奖励，使用 cosine schedule 并保留正的下界，使早期更愿意探索教材、后期仍保留少量访问激励。检索片段还显示，tool judge 会回顾式判断检索材料是否支持后续推理，单轮没有立即使用不应直接判负；参考解答与最终正确性 verdict 对 tool judge 隐藏，以降低泄漏和结果倒推。具体 judge 实现、RL 算法和奖励权重在给定片段中不完整，需要回原文核对。

五、实验主线。主实验比较 Self-Learning、No-Book 与三种 fixed-retrieval 基线，且基线也经过 RL 训练，以区分「自主导航」与「单纯 RL」的效应。摘要报告 Intern-S2-Preview-35B 的总体证明准确率从 51.5% 提升到 79.5%，并优于 textbook-free 与 fixed-retrieval 基线；在未见教材、未见学科上也有提升。外部迁移方面，BrowseComp-Plus 提升 1.7 个百分点、CL-bench 提升 2.3 个百分点，但这两个基准沿用原始推理设置而非 BookNavigator，因此是能力迁移而非接口直接迁移。

六、机制分析与关键现象。作者做了参考材料移除实验：移除与题目相关的教材邻域，并与随机移除九个邻域外 section 的 section 数量匹配控制组比较。结果显示策略可以从替代教材或源邻域之外寻找先修知识，但被移除内容的位置仍然重要。另一个重要现象是 RL 也把 No-Book 准确率从 55.7% 提升（提升后数值在片段中被截断），说明增益可能包含一般推理能力，而不只是教材调用能力。若 51.5% 是同一 base 在 Self-Learning 协议下的总体准确率，而 55.7% 是 No-Book base，则初始模型使用教材接口反而可能拖累表现，合理推断 RL 同时降低了接口学习成本。

七、结论与批判性阅读。论文的结论是 RL 可以培养一种领域无关的自学习策略，并提示长文语料可作为 self-supervision 来源。这个叙事有吸引力，但证据边界需要明确：主结果集中在数学教材与 LLM-as-judge；OOD Subject 仍是数学内部；外部基准增益较小且协议不同；没有人类专家或形式化证明验证；教材/习题的预训练污染未在给定片段中说明；tool judge 的偏差与 reward hacking 风险也未展开。因此，这篇论文更适合作为「用 RL 训练长文导航与自我学习」的系统性原型来读，而不是已经把 self-learning 推广到开放科学领域的定论。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent 与 RL 方向：多轮工具调用、退火探索奖励、轮级 tool credit 和延迟信用分配是可直接迁移的设计；尤其适合长文检索、代码库导航和实验操作代理。

## 基本信息

- 作者：Lingkai Kong, Yuzhe Gu, Haiteng Zhao, Zijian Wu, Youqing Fang, Zhouqi Hua, Yanan Sun, Kai Chen, Wenwei Zhang
- 机构：上海人工智能实验室（Shanghai AI Laboratory）相关团队（推测；依据 Intern-S2-Preview 模型命名与作者名单，给定片段未显式标注机构，需以原文首页为准）。
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-10-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.39671v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 56 个 PDF 语义检索片段与摘要，并对 heuristic_draft 中明显错位、截断或与字段不匹配的内容做了纠正；无法由片段确认的动作名称、训练细节和逐项数值均标注为推断或需回原文核对。
