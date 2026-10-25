---
user_id: "cheng tan"
paper_id: 16428
arxiv_id: "2610.18427v1"
title: "Reliable Memory from Unstable Growth: Curating Self-Written Agent Playbooks with Randomized Experiments"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18427v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18427v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:16:06"
---
# Reliable Memory from Unstable Growth: Curating Self-Written Agent Playbooks with Randomized Experiments

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：persistent memory · llm agent · randomized experiment · playbook curation

## 一句话总结

本文提出 Randomized Block Curation（RBC）：用随机化实验在练习任务上测量智能体自写 playbook 中每块经验教训对任务成功的因果效应，并同时维护多个 playbook 择优服务，以解决自写记忆被同一流程写成“有害”时性能大幅下滑（最多比无 playbook 低 69.6 分）的不稳定问题。

## 摘要

> Persistent textual memory lets language-model agents reuse lessons from past tasks to improve on future multi-step tasks without retraining. Systems such as ACE and Dynamic Cheatsheet keep it as a playbook: lessons the agent writes after each task and reads in its prompt. A key challenge is to decide which lessons improve later tasks. Current systems leave the decision to the agent itself, which makes performance unstable: the same procedure can write a playbook that helps or one that falls up to 69.6 points below no playbook. These findings motivate valuing lessons by their measured effect on task success rather than by the agent's judgment. We propose Randomized Block Curation (RBC), which measures this effect with a randomized experiment on practice tasks. It groups related lessons into blocks large enough to move success measurably and, on each practice task, includes or leaves out every block at random. Random inclusion makes tasks with and without a block comparable, so the gap in their success estimates the block's effect. RBC keeps the blocks that help and, since the same procedure can write a helpful or a harmful playbook, grows several and serves whichever curated, whole or no playbook scores best on fresh practice episodes. Across two memory systems, three models and three benchmarks, RBC raises average success to 66.9, against 55.5 with the whole playbook, 54.1 with no playbook and 53.8 when the agent's own judgment picks the lessons to keep.

Q1: 这篇论文试图解决什么问题？

本文要解决的问题是：语言模型智能体的自写持久化记忆（playbook）不可靠。具体包括以下几个层面：

1. 决策主体错位：现有系统（ACE、Dynamic Cheatsheet）把“哪些经验教训值得保留”的决定交给智能体自己的判断，而智能体对自身经验的价值缺乏可靠判断能力，导致保留的教训可能对后续任务有害。

2. 结果方差极大且存在严重负收益：作者观察到同一套书写流程（the same procedure）在不同运行中可能长成有帮助的 playbook，也可能长成比“无 playbook”低最多 69.6 个百分点的有害 playbook。这说明问题的关键不是“要不要记忆”，而是“记忆内容的质量控制”。

3. 评价信号缺失：只记录任务是否成功无法把贡献归因到具体某条经验教训；任务成败混杂了任务难度、模型随机性、提示词内容等多重因素，单条 lesson 的效应太小，难以直接测量。

4. 效应量太小：单条 lesson 对任务成功的影响可能微乎其微，需要把相关教训聚合成块（block）才能产生可测的成功率差异。

5. 选择偏差与混杂：简单比较“写了某教训的任务”和“没写某教训的任务”会受选择偏差影响，因为任务本身难度不同；需要随机化干预才能得到无偏估计。

6. 动态择优问题：即使能估计每个块的效果，最优 playbook 也不一定是“全部保留”或“全部删除”；当完整 playbook 在某些阶段仍然比 curated 版本好时，系统应当有能力继续服务完整版本。

7. 长期增长的退化：ACE 从不删除教训，playbook 无限增长，token 开销和噪声随之增加。本文在长增长（最多十轮 pass）条件下观察到完整 playbook 收益下降甚至变负（例如 AppWorld 27B 上完整 playbook 46.6 对无 playbook 78.4），说明“只增不减”的记忆管理策略不可持续。

本质上，本文把“playbook 内容选择”重新定义为一个因果推断问题而非文本质量排序问题，主张用测量（measured effect）取代判断（judgment）。

Q2: 有哪些相关研究？

与本文相关的研究脉络可以从以下几个方向梳理（结合摘要、结论片段和引用线索，部分为合理推断）：

1. 持久化文本记忆/playbook 系统：ACE 和 Dynamic Cheatsheet 是直接对手系统。它们都让智能体在任务后写教训并在后续提示中读取；ACE 从不删除教训，导致 playbook 持续增长。本文实验直接复现了这一点，并指出其长增长下的退化。这是本文的主要 baseline 家族。

2. 智能体技能库（agent skill libraries）：引文残片中出现 SkillHunter、Coalition-aware skill reliability for self-evolving agents 等工作，这些工作也维护智能体可复用的技能/经验集合，但选择机制与本文不同。文中还提到 RSEA 的接受规则（Nguyen et al.）作为 baseline，RSEA 属于“判断式/规则式”筛选。

3. 上下文归因（context attribution）：引文出现 ContextCite（Cohen-Wang, Shah, Georgiev, Madry, 2024），它把模型生成归因到上下文片段。本文的块效应估计在精神上类似“对上下文做因果归因”，但 ContextCite 是事后归因而非干预式实验。

4. Top-k 相似度检索式记忆筛选：引文出现 ReasoningBank（Ouyang et al., 2026），该类方法用任务文本相似度从记忆中取最相关的 lessons（Top-k by similarity），是“相关性”而非“因果效应”驱动的选择。本文在实验基线列表中包含 top-k 条目。

5. 随机化实验与实验设计：方法明显受经典实验设计影响（Box, Hunter, Hunter 的 Statistics for Experimenters 出现在引文中）；随机分块设计（randomized block design）是统计学标准工具，本文将其引入 LLM agent 记忆管理，属于跨领域方法论移植。

6. 自我进化智能体（self-evolving agents）：引文出现多篇 2026 年 preprint（如 Self-Evolving、Coalition-aware skill reliability 等），说明该领域近期活跃；本文强调“技能的可靠性应被测量”，与这些工作的可靠性分析方向一致。

7. 无训练持续改进（improvement without retraining）：prompt 级记忆与 fine-tuning 形成对比；引文中也有 fine-tuning 稳定性相关文献（weight initialization、data orders、early stopping），说明作者把记忆管理问题放在“不重训练”的持续学习谱系中。

8. Shapley 值归因：引文出现 Shapley 相关文献，属于用博弈论做贡献分配的路线。本文的块效应估计是更直接的干预式估计，作者可能在相关工作或讨论中比较过 Shapley 归因的计算代价与不可行性。

需要说明：由于检索证据主要是 Conclusion 和引用残片，部分 related work 的具体关系是合理推断；原文 Related Work 章节未在本次证据中完整提供。

Q3: 论文如何解决这个问题？

本文方法 Randomized Block Curation（RBC）的核心思路是把 playbook 内容筛选转化为随机化实验，用实测因果效应替代智能体自我判断。技术路线分为以下几个环节：

1. 分块（Blocking）：把相关经验教训聚合成块（blocks）。单条 lesson 的效应太小难以测量，块要足够大以产生可测量的成功率差异；块内部是语义相关或主题相关的教训（合理推断）。

2. 随机化干预：在练习任务（practice tasks）上，对每个任务随机包含或排除每个块。随机包含是关键识别假设：它使“有块的任务”和“无块的任务”在任务难度和模型噪声上可比，因此任务成功率的差值就是块的平均因果效应。

3. 效应估计：对每个块 j，用包含该块的任务成功率与不包含该块的任务成功率的差（或等价回归；文中提到 ridge penalty，说明可能用了带 L2 正则的回归估计）估计 ˆ∆j，作为块对任务成功的影响。证据片段明确说明“cancels, so ˆ∆j is unbiased for ∆j = θj”，即该估计是无偏的。文中还定义了 µ(x, T) 为任务 x 在服务块集合 T ⊆ [m] 时的期望成功率，V(T) 为候选 playbook 的价值函数，说明作者给出了形式化的优化目标。

4. 理论保证：对每个候选 playbook，真实价值 Vpr(Sc) 与估计 πc 的偏差由 Hoeffding 不等式控制：Pr[|πc − Vpr(Sc)| > ε] ≤ 2 exp(−2nε²)，其中 n 为评估 episode 数；对多候选做 union bound，控制总选择误差。命题包含三个部分：identification（识别）、precision（精度）和 ridge penalty 的影响。

5. 筛选（Curation）：保留效应为正（有帮助）的块，形成 curated playbook。

6. 多 playbook 并行生长与在线择优：由于同一流程可能长出有害 playbook，RBC 同时生长多个 playbook（实验中为 3 个 growth runs，最多 10 轮 pass）；服务前在新鲜的 practice episodes 上评估三类候选——curated playbook、完整 playbook（whole）、无 playbook（none）——选择得分最高者。这个 head-to-head 选择避免了“curated 一定比完整好”的错误假设。

7. Token 开销联合优化：curation 不只选有效块，还天然减少上下文 token；结论指出 “curating adds to choosing with fewer tokens, above all under long growth”，即在长 playbook 下 token 节省最显著。

8. 信号鲁棒性变体：在 AppWorld 上作者还尝试用模型自身的 goal check 代替任务结果来估计块效应，说明方法对“结果信号”的形式有一定鲁棒性（至少在该环境上）。

整体上，RBC 是一个“测量-筛选-服务”闭环：写 playbook 照旧，但读之前先做随机化实验测量每块的因果效应，然后动态选择服务版本。

证据缺口：分块的具体算法（如何“group related lessons”）、ridge 回归的具体形式、以及块效应与 Top-half、Tags 诊断指标的精确定义，在检索证据中只有残片，需回原文 Method 章节确认。

Q4: 论文做了哪些实验？

根据摘要、结论片段和引用残片，本文实验设置可以归纳如下（部分细节为合理推断，因为完整实验章节未在检索证据中）：

1. 系统与基线：
- 两种记忆系统：ACE 和 Dynamic Cheatsheet（DC）；DC 又分 DCself Reviews（自评）和 DCoutcome（以任务结果评判）两个变体，两者都把 curator 当作 judge 运行。
- 三个模型：27B、9B、Gemma（具体型号未在证据中出现，合理推断是某 27B/9B 规模 LLM 与 Gemma 系列模型）。
- 三个基准：AppWorld（AW，含 pass/fail 与 per-test feedback p-t 两种设置）、WebShop（WS）、ALFWorld（ALF）。
- 对比对象：无 playbook（54.1）、完整 playbook（55.5）、agent 自己判断筛选（53.8，即 DC 类 curator-as-judge 或 RSEA 接受规则）、Top-k 相似度选择（类似 ReasoningBank）、Best-Full（RSEA 接受规则）等。

2. 生长协议：
- playbook 通过多轮 pass 在任务序列上生长，每轮 pass 智能体写 lessons；最多 10 轮 pass。
- 三个生长 run（three growth runs）并行，用于体现同一流程的多重实现；whole playbook 的结果在三个 run 上平均。
- 共 9 个设置（nine settings）：两个或更多记忆系统 × 三个模型 × 三个基准的某种组合；WebShop 对 9B 和 Gemma 被排除（据结论片段，因为 27B 的 WebShop playbook 几乎不增长），所以最终为 9 个设置而非 18 个。

3. 块效应测量：
- 证据中出现“over the eight blocks (first practice round)”，说明某个设置第一次 practice round 时有 8 个块。
- 效应定义：points of success with the block included minus left out（包含块与排除块的成功率之差）；Tags 为块内 lessons 标签贡献总和；Top half 为块内 lessons 在 playbook 前一半的比例；另分析块占完整 playbook 的 token 份额。
- 在 AppWorld 上还估计了“由模型自身的 goal check”得到的块效应，与真实任务结果对比。

4. 主要结果数据：
- RBC 平均成功率 66.9；完整 playbook 55.5；无 playbook 54.1；智能体自主筛选 53.8。
- 某子集中完整 playbook 相对无 playbook 更高（证据片段出现 61.3 对 53.8 的对比），但另外的设置（AppWorld 27B per-test feedback）完整 playbook 显著有害（46.6 对 78.4；另有残片出现 17.7 对 78.4，可能是另一模型或设置）。
- “curating adds to choosing with fewer tokens, above all under long growth”：curation 比“选择”更省 token，尤其在长 growth 下。

5. 统计工具：效应估计伴随 Hoeffding 不等式与 union bound 的置信分析；候选 playbook 的分数比较用 Hoeffding 界。

6. 公平性设计：所有候选（curated、whole、no playbook、RSEA 等）在同一批 fresh practice episodes 上评估比较。

需要指出：由于检索证据只包含 Conclusion 片段和引用残片，精确的实验矩阵（每个子设置的具体数字、消融内容）无法完整还原；上述某些条目为合理推断，建议回原文 Experiments 章节核对。

Q5: 发现了什么实验现象？

从证据可归纳的实验现象如下：

1. 自写 playbook 的双峰命运：同一个书写流程可以长出有帮助的 playbook，也可以长出比无 playbook 低最多 69.6 分的有害 playbook。这是本文的出发点，也是对“自我改进记忆”类方法最直接的负面结果。

2. 完整 playbook 并非越多越好：平均上完整 playbook（55.5）略高于无 playbook（54.1），但在具体设置上可以大幅为负——AppWorld 27B per-test feedback 设置下完整 playbook 46.6 对无 playbook 78.4，差距约 -31.8 分；另一残片显示 17.7 对 78.4（可能是另一模型或设置）。这说明“全部保留”策略在长增长后会产生严重负收益。

3. 智能体自主筛选最差：agent 自己判断保留哪些 lessons 的平均成功率 53.8，不仅低于完整 playbook 的 55.5，还低于无 playbook 的 54.1。这是对 curator-as-judge 范式的直接否定：LLM 的判断不仅不能增值，反而平均有害。

4. 测量优于判断：RBC 把平均成功率从 55.5（完整）和 54.1（无）提升到 66.9，提升约 11-13 个百分点；相对智能体自主筛选的 53.8 提升约 13.1 个百分点。

5. 效应方向与表面质量分离：块效应诊断（8 个块）显示块效应有正有负，且与 lesson 在 playbook 中的排名（Top half）和标签贡献（Tags）的关系需要专门分析——说明 playbook 内同时存在“好教训”和“坏教训”，无法靠 lesson 自身文本特征或作者排序区分，必须靠干预测量（合理推断）。

6. 记忆系统变体差异：DC 的两个变体（DCself Reviews 与 DCoutcome）都作为 judge 运行 curator，均未按实测效应排序；说明“用任务结果当信号”也不够，因为结果本身不能归因到具体条目。

7. 长增长下的退化加剧：ACE 从不删除教训，playbook 无限增长；最长 10 轮 pass 后完整 playbook 的帮助消失甚至变负；curation 的 token 节省优势在长增长下最明显（“above all under long growth”）。

8. 任务环境差异：WebShop 27B 的 playbook 几乎不增长，说明“经验积累”本身依赖任务-模型组合，不是所有环境都能长出记忆；因此作者排除 WebShop 的 9B/Gemma 设置。这直接构成 RBC 有效性的边界条件。

9. 结果信号的替代形式：AppWorld 上作者用模型自身的 goal check 替代任务结果做块效应估计也能工作，说明 RBC 不严格依赖外部真实结果标签（至少在该环境上）。

10. 随机化带来可比性：文中明确随机包含使“有无该块的任务可比”，块效应按成功率差计算；第一轮 practice round 的 8 个块上展示了效应、标签贡献、top-half 占比、token 占比等诊断。

注意：以上某些现象（如块内正负效应并存）是从证据片段合理推断的；原文数据表格（Table 1/2）未在检索证据中完整提供。

Q6: 有什么可以进一步探索的点？

在本文基础上可以进一步探索的方向包括：

1. 块构造的自动化与自适应：当前分块方式（相关教训分组）在证据中未给出明确算法。可研究如何自动确定块的大小、粒度与聚类准则，例如用 embedding 聚类、按任务类型分层，或让块大小随估计精度动态调整。

2. 更精细的效应归因：当前是块级估计；可以引入交叉验证、贝叶斯分层模型或对块内 lessons 做 Shapley 分解，把“块级效应”进一步归因到“条目级效应”，降低保留有害单条的风险。

3. 在线/自适应实验设计：当前 practice episodes 与测试任务分离；可研究多臂老虎机式的自适应实验，让练习任务分配随估计不确定性动态变化，提高样本效率。

4. 与检索增强结合：将 RBC 筛选出的块作为候选池，再按任务相似度做 Top-k 检索（类似 ReasoningBank），可能比全量 curated playbook 更省 token 且更聚焦；本文的 token 分析已暗示这种组合的收益。

5. 跨任务迁移与遗忘：研究块效应在不同任务分布上的稳定性——一个块在练习任务上有正效应，在测试分布漂移后是否仍然为正；可引入分布鲁棒估计或时间衰减因子。

6. 多 playbook 的种群策略：本文同时生长 3 个 playbook 并择优；可研究更大种群、交叉/变异策略、以及何时淘汰旧 playbook，形成类似进化算法的记忆管理。

7. 弱监督信号扩展：AppWorld 上已尝试用模型自身 goal check 替代任务结果；可研究在更多环境上用过程监督（process reward）、自我一致性或人工反馈作为效应信号。

8. 理论边界的深化：Hoeffding 不等式与 union bound 给出了候选选择的样本复杂度；可推导块数 m、练习任务数 n、效应量 θj 与最终 regret 之间的精确权衡，并研究相关块（非独立干预）时的估计偏差。

9. 记忆压缩与遗忘：curation 的 token 节省在长增长下最明显，但还可研究把保留块进一步压缩（摘要化、去重、参数化）以减少上下文开销，或周期性“重写”playbook。

10. 与 fine-tuning / 外部知识库结合：将 RBC 筛选出的高价值教训作为微调数据或 RAG 语料，研究“提示级记忆”与“权重级记忆”的互补性（引文中的 fine-tuning 稳定性文献暗示了这一方向）。

11. 更广泛的应用域：当前实验在 AppWorld、WebShop、ALFWorld 三个工具使用/具身环境；可推广到代码生成、科学发现智能体（AI for Science）、多智能体协作等场景，检验“测量优于判断”的普适性。

12. 安全性与对抗鲁棒性：如果 playbook 被注入恶意教训，RBC 能否通过随机化实验检测并排除？该框架天然提供反事实评估能力，可研究对抗性教训的防御。

Q7: 总结一下论文的主要内容

本文研究语言模型智能体持久化文本记忆（playbook）的质量控制问题，核心论点是：自写经验教训的取舍不应交给智能体自己的判断，而应通过随机化实验测量每条经验对任务成功的因果效应来决定（“An agent's memory should be measured, not judged”）。

论证主线：作者先指出现有系统（ACE、Dynamic Cheatsheet）依赖智能体自评来维护 playbook，并观察到严重不稳定性——同一套书写流程在不同运行中可能产出有帮助的 playbook，也可能产出比“无 playbook”低最多 69.6 个百分点的有害 playbook。智能体自我判断的平均成功率 53.8，甚至低于不用 playbook 的 54.1，说明“判断式记忆管理”不可靠。由此引出方法论转向：用测量的因果效应代替主观判断。

技术主线：作者提出 Randomized Block Curation（RBC），包含五个要素：（1）把语义相关的经验教训聚合成足够大的块，使效应可测；（2）在练习任务上随机包含/排除每个块，使有无块的任务可比较，成功率差值即为块的平均因果效应；（3）用无偏估计（含 ridge 正则回归）估计块效应，并给出 Hoeffding 不等式与 union bound 的置信保证；（4）保留正效应块形成 curated playbook；（5）因为同一流程可能长出有害 playbook，所以同时生长多个 playbook，并在新鲜练习片段上比较 curated、完整、无 playbook 三种候选，服务得分最高者。

实验主线：在 ACE 和 Dynamic Cheatsheet 两种记忆系统、27B/9B/Gemma 三个模型、AppWorld/WebShop/ALFWorld 三个基准上，排除 WebShop 的 9B/Gemma 设置后共 9 个设置，最多生长 10 轮。结果显示 RBC 平均成功率 66.9，显著高于完整 playbook 的 55.5、无 playbook 的 54.1 和智能体自我筛选的 53.8。退化分析显示完整 playbook 在长增长下收益消失甚至为负（如 AppWorld 27B per-test feedback 下 46.6 对 78.4），而 curation 的长处不仅在效果，还在 token 节省（尤其长增长下）。作者还在 AppWorld 上验证了用模型自身 goal check 替代任务结果作为效应信号的变体。

结论：任务成功率从 55.5 提升到 66.9 表明“测量优于判断”在记忆管理上有效；记忆的价值应由实测效应决定。

需要说明：检索证据主要来自 Conclusion 和参考文献残片，方法章节和完整实验表格未在证据中，因此部分机制细节（如分块的具体算法、ridge 回归的形式）是基于片段的合理推断。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：方法论相关性：这是一篇典型的“系统性工作优先”论文——把记忆管理从 prompt 工程问题提升为带有识别假设、无偏估计和置信界的因果推断问题，与画像中 preference_systematic_work_over_incremental 高度匹配。

## 基本信息

- 作者：Hoang Pham, Cong-Duy Nguyen, Dong Le, Ponhvoan Srey, Anh Tuan Luu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18427v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（42 chunks，主要命中 Conclusion 区与引用残片），并以其优先校正 heuristic_draft 中的噪声；部分方法细节因证据仅覆盖结论与引用残片，已按合理推断/推测标注并在相应字段注明缺口。
