---
user_id: "cheng tan"
paper_id: 8191
arxiv_id: "2608.16876"
title: "AutoSR: Automatic Symbolic Regression by Searching Research States"
publish_date: "2026-08-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.16876.pdf"
pdf_url: "https://arxiv.org/pdf/2608.16876"
abs_url: "https://arxiv.org/abs/2608.16876"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-19T09:37:51"
---
# AutoSR: Automatic Symbolic Regression by Searching Research States

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：automatic symbolic regression · research state · monte carlo tree search · proposer-reviewer agents

## 一句话总结

AutoSR 提出将符号回归从搜索孤立方程升级为搜索持久化的“研究状态”，通过提议-评审智能体在渐进式扩展 MCTS 中实例化研究空间，在九个基准挑战上全部实现代数等价恢复，并输出可审计的最终研究报告，旨在让科学证据与累积研究记忆共同塑造方程的探索与论证。

## 摘要

> We introduce Automatic Symbolic Regression (AutoSR), a fully automated system that instantiates Research-Space Symbolic Regression by searching persistent scientific investigations rather than isolated equations. Finite, noisy data often yield numerically competitive expressions that imply very different behavior outside the observed regime, making numerical fit and syntactic complexity insufficient measures of scientific credibility. Existing approaches largely focus on improving expressions, yet the search typically retains little beyond the resulting formula and score, losing the scientific record, such as motivations and probes, that inform what to try next. AutoSR preserves this record in a Research State, coupling each candidate equation with the reasoning, computational evidence, and independent review developed along its branch. Proposer-reviewer agents develop these states under progressive-widening Monte Carlo tree search (PW-MCTS), which allocates computation across competing investigations, while the accumulated research record is ultimately synthesized into a final report that explains the leading relation and the basis for its selection. Across nine selected challenges from two benchmark suites, AutoSR recovers algebraically equivalent relations in every case, including three cp3-bench problems that no published system recovers and six structurally diverse LSR-Transform problems. Overall, AutoSR extends symbolic regression from equation-level search toward automated scientific investigation, allowing scientific knowledge and accumulated evidence to shape both what is explored and how the resulting equation is justified.

Q1: 这篇论文试图解决什么问题？

1. 传统符号回归（SR）的核心目标是从数据中寻找解析表达式，通常以数值拟合误差和表达式复杂度作为评价标准。2. 然而，有限且带噪的数据往往支持多个数值上几乎同样优秀的表达式，这些表达式在观测范围外可能具有完全不同的动力学或物理行为，因此仅凭数值拟合和语法复杂度无法判断方程的科学可信度。3. 现有 SR 方法通常只返回公式和得分，搜索过程本身——包括动机、假设、尝试过的失败方向、诊断证据——都被丢弃，导致后续迭代无法利用这些科学记录，也难以解释为什么选择某个方程。4. 这引出一个研究范式问题：如何让符号回归不仅寻找方程，还能像科学家一样进行持久、累积的调查研究，并在最终报告中提供可审计的选择依据？5. AutoSR 试图回答这一问题，将 SR 从“方程级搜索”提升到“研究空间搜索”，并使其在计算规模上实现自动化。

Q2: 有哪些相关研究？

1. 传统符号回归方法包括遗传编程、稀疏回归、神经网络符号回归等，通常侧重数值拟合和复杂度正则化。2. 近期涌现的“语义 SR 方法”（如文献 [SMG+25, XSL26, LP26, PZL+26, ZLX+26]）利用上下文知识、计算工具、诊断信息和可复用记忆来改进方程提议，说明该领域正在从单纯优化转向更具认知性的方法。3. 这些方法仍然主要聚焦于“改进下一个方程”，而不是保留和利用整个研究过程。4. MCTS 在符号回归和程序合成中已有应用，但通常用于单个表达式的树搜索，而非管理多个相互竞争的研究分支。5. 多智能体系统（如提议-评审）也出现在自动科学发现中，但 AutoSR 将它们与持久化研究状态和渐进式扩展搜索结合，形成新的架构。

Q3: 论文如何解决这个问题？

AutoSR 的核心是将每个调查分支表示为“研究状态”（Research State）。研究状态不仅包含候选方程，还包含该分支的动机、计算证据、独立评审、失败尝试和累积记忆。系统使用两个角色：提议者（Proposer）生成新的假设或方程变化，评审者（Reviewer）对候选方程进行批评、检查是否符合科学证据，并提出下一步方向的建议。这些智能体在渐进式扩展蒙特卡洛树搜索（PW-MCTS）中协作：PW-MCTS 在多个竞争性调查之间分配计算资源，避免过早聚焦于单一局部最优，允许有前景的方向加深探索。搜索过程中，每个研究状态保存其“发现、失败和未验证假设”的记录，形成可重用的研究记忆。最终，所有累积的研究记录被综合为一份最终报告，该报告解释领先方程与证据、局限性、研究历史以及可信替代方案之间的联系，提供可审计的选择依据。AutoSR 的“全自动”体现在它可以自动运行整个调查流程，而无须人工干预。

Q4: 论文做了哪些实验？

1. 实验选择了两个基准套件中的九个挑战方程：三个来自 cp3-bench，六个来自 LSR-Transform。2. cp3-bench 中的三个问题被特别标注为“此前没有任何已发表系统能恢复”，从而检验 AutoSR 是否具备突破现有方法的能力。3. 对每个问题，AutoSR 运行完整的研究流程，最终输出一个主要方程。4. 评价标准是方程与真值的“代数等价性”，而非仅仅数值拟合或符号距离。5. 论文报告称 AutoSR 在全部九个问题上都恢复了代数等价的方程。由于目前可获得的摘要和引言片段有限，具体的数据生成方式、噪声水平、计算预算、基线对比细节未在证据中体现。

Q5: 发现了什么实验现象？

1. 成功恢复：九个选定挑战全部达到代数等价恢复，包括三个 cp3-bench 难点和六个 LSXR-Transform 问题。2. 突破性发现：对于三个 cp3-bench 问题，AutoSR 是第一个能够恢复它们的系统，显示其研究状态保存和 MCTS 分配策略可能为搜索带来更强的鲁棒性。3. 结构多样性：六个 LSR-Transform 问题结构多样，能够全部恢复说明方法不局限于特定方程形式。4. 需要注意，当前可用的证据中没有提供数值指标、错误率、消融对比或负结果，因此无法评估方法在更广数据集上的失败模式、计算开销或模型对噪声的敏感性，也无法判断 PW-MCTS 相对标准 MCTS 的具体增益。这些细节需要阅读完整原文确认。

Q6: 有什么可以进一步探索的点？

1. 扩展研究状态表示：可以将更丰富的科学元数据（如单位、物理约束、因果图表）纳入研究状态，提升评审者判断的科学严谨性。2. 更大的基准评估：在更多、更复杂的符号回归基准（如 Feynman、ODE 发现任务）上系统评测，并公开失败案例。3. 自动假设生成：让提议者能够主动设计新的实验或探测方案，而不是仅基于现有数据提出方程。4. 报告可审计性：进一步研究如何将自动生成的研究报告与人类可理解的证据链结合，用于科学出版或验证。5. 计算效率优化：PW-MCTS 的并行化和预算分配策略有待深入优化，以处理更高维问题。6. 跨领域应用：将研究状态框架推广到其他科学发现任务（如因果发现、知识图谱推理）中。

Q7: 总结一下论文的主要内容

AutoSR 是一篇提出新符号回归范式的论文。作者指出传统符号回归只追求数值拟合和复杂度约束，但在有限噪声数据下，数值上等价的表达式可能在行为上完全不同，因此科学的可信度需要更多证据支撑。现有方法虽然开始引入上下文知识、计算工具和记忆，但搜索过程本身仍然被丢弃，导致无法保留科学调查的动机、失败和线索。AutoSR 将每一个调查分支封装为“研究状态”，保存候选方程、推理、计算证据和独立评审。提议者-评审者智能体在渐进式扩展的 MCTS 中协同工作，在竞争性的研究方向之间分配计算，并不断更新研究状态。搜索结束后，系统把所有记录综合成最终报告，说明所选方程的理由、局限和替代方案。实验中，AutoSR 在九个跨两个基准套件的挑战问题上都恢复了代数等价的方程，其中三个 cp3-bench 方程是此前没有系统能恢复的，体现了该方法的突破能力。论文的主要贡献是提出研究空间符号回归这一概念框架、实现完整的自动化系统、并在基准上验证了其在“恢复难方程”上的有效性。AutoSR 使符号回归从方程级搜索扩展到自动化科学调查，让科学知识和累积证据反过来指导探索与论证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与“agent”方向高度相关：论文核心是提议-评审智能体协同搜索研究空间，展示了多智能体在科学发现中的应用。

## 基本信息

- 作者：Kejia Zhang, Youran Sun, Xinyu Ren, Chugang Yi, Haizhao Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SC, cs.AI, cs.LG, math.NA
- 日期：2026-08-18
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.16876`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要和引言片段，但未获取完整正文，部分细节基于合理推断，具体实验数值和完整讨论请以原文为准。
