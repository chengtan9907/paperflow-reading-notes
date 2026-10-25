---
user_id: "cheng tan"
paper_id: 17477
arxiv_id: "2610.29303v1"
title: "ExpEvolve: Structuring and Reusing Experience for Open-Ended Discovery"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.29303v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.29303v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-25T14:54:34"
---
# ExpEvolve: Structuring and Reusing Experience for Open-Ended Discovery

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：evolutionary coding agents · experience reuse · idea-level control · open-ended discovery

## 一句话总结

ExpEvolve 把进化式编码智能体中对程序低层变异的侧重，提升为可持久复用的“想法因子”提取与控制：用成对排序器解释程序得分差异，并从未被解释的高分差程序对中继续发现新因子，从而同时改善经验利用与平台期探索；在 11 个开放式数学任务、4 个其他 LLM 与 6 个 MLS-Bench 任务上报告最终性能和最高 5.0 倍样本效率提升。

## 摘要

> Evolutionary coding agents such as AlphaEvolve are one of the notable successes in using Large Language Models (LLMs) to build AI Scientists. These agents tackle open-ended scientific problems by iteratively improving and evolving programs, leveraging the prior knowledge and reasoning capabilities of LLMs. Despite their success, mutations over programs are mostly low-level upon surface heuristics, without a persistent unit of control. In contrast, science compounds through ideas. In this work, we propose ExpEvolve, which injects idea-level extraction and control into evolution: it discovers and leverages the high-level factors from programs that explain scoring differences among different programs. We characterize theoretical conditions on when discovery with idea factors improves sample efficiency. We also build a practical framework to show that the extracted high-level idea factors are helpful for guiding exploitation from experience and exploration when discovery comes to a plateau. Across 11 open-ended mathematical tasks, ExpEvolve significantly improves the final performance and sample efficiency up to 5.0x. The improvements also transfer to 4 other LLMs and to 6 machine learning tasks from MLS-Bench. Analysis shows that ExpEvolve can discover idea-level factors to provide better exploration and exploitation for evolutionary discovery.

Q1: 这篇论文试图解决什么问题？

【核心问题】论文要解决的不是“如何再设计一个更强的程序变异算子”，而是开放式科学发现中“经验应以什么单元被保留、检索、组合和迁移”的问题。作者把科学发现看作假设迭代：程序是假设的可执行表示，评分是实验反馈；随着生成规模扩大，验证每个假设的成本上升，而有希望的假设在执行后经常退化。因此，系统必须从历史经验中学习，避免重复错误和次优解，并把有前景的探索方向编码下来。
【具体痛点】1）现有进化式编码代理的变异大多是低层、表层启发式的代码编辑，缺少一个可持久控制的“想法层”，难以做跨代信用分配；2）经验多以程序归档、近期摘要或岛式档案存在，信息碎片化，不能显式回答“哪一类高层选择导致分数差异”；3）开放式搜索存在明显的探索—利用张力：早期需要多样化，后期需要围绕有效方向利用，进入平台期又需要重新探索；4）经验需要跨任务、跨 LLM 迁移，否则每个新问题都要从零试错；5）验证成本是主要瓶颈，样本效率比单次最佳分数更重要。
【论文的研究问题】可以概括为：能否从程序及其得分中抽取可命名、可比较、可累积的高层想法因子？这些因子能否解释程序间得分差异？当现有因子解释不了高分差程序对时，能否据此发现新因子？这些因子能否反过来控制利用与探索，并在有限预算下提升样本效率？
【隐含假设与潜在张力】论文的方法隐含假设包括：想法因子可以被 LLM 稳定抽取；因子在任务内和任务间具有可比较性；得分差异中有足够部分可由高层因子解释；抽取与维护因子的成本低于搜索节省；因子不是单纯的任务特定表面线索。若这些假设不成立，因子可能退化为噪声标签，甚至误导搜索。
【证据校准】上述问题框架由摘要和 Introduction 片段直接支持；“因子可能退化为噪声标签”“跨任务迁移的困难”属于合理推断，可见片段尚未给出系统失败分析。

Q2: 有哪些相关研究？

【可见脉络】论文的 Related Work 在提供片段中没有完整展开，但可确认几条主线。第一条是 LLM 驱动的 AI Scientist：从文献调研、假设生成、数据分析、实验设计到完整发现工作流，已有大量工作；论文在 Introduction 中引用了 Asai et al., 2026、Wang et al., 2024、Yang et al., 2024、Majumder et al., 2025、Li et al., 2025a、Gottweis et al., 2026、Lu et al., 2024、Mitchener et al., 2025、Yamada et al., 2025 等，说明其背景是“LLM 自动化科学发现”这一大范式。
【进化式编码代理】第二条主线是 AlphaEvolve 一类系统：用 LLM 对程序做迭代改进与进化，在数学、算法发现、蛋白质设计等任务上取得成功。论文明确把 SCS（Gideoni et al., 2026）作为 backbone 和主基线，SCS 又被描述为相对更复杂进化编码代理仍有竞争力的简化骨架；ShinkaEvolve（Lange et al., 2026）则维护 island-based archive，并对近期程序的洞见做摘要。OpenRSI 被用作 MLE 任务的进化 harness，包含 Draft、Improve、Debug、Crossover 算子。
【经验复用与进化计算】第三条主线是经验复用本身。经典进化计算也维护种群、档案和多样性；LLM 代理则常用记忆、反思、摘要或检索来复用历史。论文的差异点在于：它不满足于“把近期程序摘要成自然语言”，而是试图抽取可解释得分差异的高层因子，并用成对排序器和未解释程序对把经验变成可检验、可扩展的因子库。
【理论侧】论文还声称与“发现何时提升样本效率”的理论条件对话；可见片段没有给出其理论谱系。合理推断是，它连接了程序搜索、偏好/排序学习与有限预算下的探索—利用分析，但具体依赖的定理框架需回原文核对。
【定位差异】与 AlphaEvolve/SCS 相比，ExpEvolve 的重点不是新的变异算子，而是持久的想法级控制单元；与 ShinkaEvolve 的岛式档案和洞见摘要相比，它更强调用统计排序器定位“当前因子解释不了的高分差对”，把残差作为新因子发现的信号。

Q3: 论文如何解决这个问题？

【总体定位】ExpEvolve 不替代 Draft、Improve、Debug、Crossover 等进化算子，而是在其上加一层“想法控制”：把程序经验压缩为可命名、可比较、可累积的 idea factors，并让这些因子参与候选生成、选择和探索分配。
【1. 经验归档】系统保留程序及其得分，形成可检索的搜索历史。Results 片段显示，在 OpenRSI 的 MLE 实验中，每个任务保留一个固定历史，包含来自早先 OpenRSI 运行的 20 个已完成搜索 slot；这说明 ExpEvolve 的设计依赖一个稳定的程序档案，而不是只看当前代。
【2. 想法因子抽取与标注】从程序归档中抽取高层因子，用自然语言或结构化标签描述“这个程序为什么可能更好”。摘要强调这些因子要能解释不同程序之间的 scoring differences；Introduction 片段则明确提到“annotated idea factors”，说明因子会被标注到程序上，并形成想法记忆。合理推断：抽取由 LLM 完成，可能结合程序代码、评分、历史对比和任务描述；但可见片段没有给出具体 prompt、因子粒度或人工校验流程。
【3. 成对排序器】ExpEvolve 在已标注因子的程序对上拟合 pairwise ranker，让排序器判断两个程序中哪个得分更高。这样，因子不是静态摘要，而是被组织成一个可预测“相对优劣”的模型；排序器同时提供了对因子解释力的量化基础。
【4. 用未解释对驱动新因子发现】系统选择那些分数差距大、但当前想法因子无法解释的程序对。大分数差意味着两者之间存在重要机制差异；无法解释意味着现有因子库缺失了某个维度。论文把这类“unexplained pairs”视为新因子的发现信号，继续抽取和补充因子。这个“残差驱动发现”的闭环是方法的核心创新之一。
【5. 利用与探索控制】因子反过来控制搜索：在利用阶段，用因子指导候选生成与选择，优先围绕有效方向改进；当发现进入平台期时，用因子空间中的未充分探索区域或未解释组合引导探索。Results 片段提到 idea-guided slots 会产生新最优程序，说明因子被落实为具体的搜索 slot 分配，而不是只做事后分析。
【6. 与进化骨架集成】ExpEvolve 被嵌入 SCS/OpenRSI 风格的进化骨架。References 片段还显示存在“无策略指令”变体、从成功程序中均匀随机抽 3 个样例并附想法记忆摘要的变体，以及 warm-up 阶段的多样化设置；这说明作者把策略指令、示例检索、记忆摘要和早期多样化作为可拆解设计，而非单一黑箱。
【7. 理论条件】论文声称刻画了“基于想法因子的发现何时提升样本效率”的条件。可确认的是，理论关注因子解释力与样本效率之间的关系；但可见片段没有给出定理假设、界或证明结构。需要回原文核对：因子空间的容量、抽取噪声、排序器误差、探索成本如何进入样本复杂度，以及条件是渐近的还是有限预算的。
【关键权衡】因子粒度太粗会失去控制力，太细会噪声化且难以迁移；抽取成本必须低于搜索节省；未解释对的选择阈值决定发现新因子的灵敏度与误报率；平台期探索的触发条件决定利用与探索的平衡。

Q4: 论文做了哪些实验？

【任务】论文在 11 个开放式数学发现任务上评估：其中 9 个来自 SCS（Gideoni et al., 2026）并取自 AlphaEvolve（Novikov et al., 2025），包括第一自相关不等式、第二自相关不等式、不确定性不等式、Erdős 最小重叠问题、集合的和与差、最大—最小距离比、三角形 Heilbronn 问题、11 维亲吻数、圆 packing；另外两个是 Hadamard 矩阵问题（Wang et al., 2026）和用线性规划恢复半径的圆 packing 变体。
【预算】每次运行评估 200 个程序。作者使用两种同预算配置：10 代×20 个程序（与 SCS 一致）和 20 代×10 个程序；后者代际更多、每代更小，对“从经验中累积可复用知识”提出更高要求。
【基线】主基线是 SCS，它既是 ExpEvolve 的 backbone，也被描述为相对更复杂进化编码代理仍有竞争力的方法；另与 ShinkaEvolve 比较，后者维护 island-based archive 并总结近期程序的洞见。
【实现与迁移】作者把 ExpEvolve 接入 OpenRSI 的 MLE 进化代理，使用 Draft、Improve、Debug、Crossover 算子；每个任务保留来自早先 OpenRSI 运行的 20 个已完成搜索 slot 的固定历史。摘要还称改进迁移到 4 个其他 LLM，并在 MLS-Bench 的 6 个机器学习任务上测试。可见片段没有给出这 4 个 LLM 的名单、规模，也没有给出 6 个 MLS-Bench 任务的具体名称。
【指标】报告预算结束时的最佳分数，作为多种子运行的均值和标准差，并报告最佳种子。这个指标组合同时关心平均可靠性与最好情况，但也意味着需要区分“平均提升”与“最佳种子提升”。
【消融线索】References/实验片段显示存在“无策略指令”的变体、从成功程序中均匀随机抽 3 个样例并附想法记忆摘要的变体，以及 warm-up 阶段的多样化设置。合理推断这些用于拆解策略指令、示例检索、记忆摘要和早期探索的贡献；片段没有给出具体消融数值。
【证据缺口】可见片段没有提供绝对分数表、置信区间、5.0x 样本效率的计算口径、逐任务胜负统计、计算成本或超参数敏感性；因此实验部分可以确认“做了什么”，但还不能独立复核“提升有多大、为什么”。

Q5: 发现了什么实验现象？

【总体现象】摘要报告 ExpEvolve 在 11 个数学任务上显著提升最终性能和样本效率，最高达到 5.0x；改进还能迁移到 4 个其他 LLM 和 6 个 MLS-Bench 机器学习任务。这说明作者试图证明收益不是单一模型或单一任务的特例。
【机制性现象】Results 片段称，在每个任务上，idea-guided actions 产生约一半的新最优程序；在 Sums and differences 的 10 代×20 个程序示例中，11 个新最优里有 6 个来自 idea-guided slots。作者据此论证想法因子确实在引导有效搜索，而不是只做事后解释。
【关键校准】“约一半新最优来自 idea-guided actions”本身需要与 idea-guided slot 的预算占比对照。如果 idea-guided slots 恰好占一半，那么“一半新最优”只是与预算相称，并不构成超额贡献；只有当 idea-guided slots 占比更低、或单位 slot 命中率更高时，才能说明因子控制有额外价值。片段没有给出 slot 占比，因此这是读原文时要重点核对的地方。
【探索与利用】分析称 ExpEvolve 能发现想法层面的因子，从而提供更好的探索与利用；结合方法描述，这意味着因子既用于围绕高分方向做利用，也用于在平台期指出未解释或未探索的方向。可见片段没有给出平台期前后探索率、因子数量增长或因子命中率的时序曲线。
【消融趋势】片段显示作者拆解了策略指令、示例采样和想法记忆摘要：一个变体不使用策略指令；另一个变体从成功程序中均匀随机抽 3 个样例并附想法记忆摘要；warm-up 阶段采用多样化。合理推断这些设计分别对应“给 LLM 明确控制信号”“给 LLM 具体成功样例”“给 LLM 压缩经验”“早期避免过早收敛”。但片段没有给出各消融的分数变化，无法判断哪一项是主要贡献。
【张力与负结果】1）最终最佳分数与多种子均值/标准差之间可能存在张力：最佳种子提升不一定代表平均可靠性提升；2）200 个程序的预算较小，样本效率的 5.0x 提升需要看相对谁、在哪个分数阈值上计算；3）因子抽取可能引入噪声，错误因子可能造成负迁移，但可见片段没有报告失败案例；4）单次 Sums and differences 示例不能替代跨任务统计。
【证据边界】目前能确认的是“作者报告了正向现象与机制性案例”，还不能确认“想法因子是因果主因”。需要看逐任务表、消融表、因子消融、跨模型一致性和失败分析。

Q6: 有什么可以进一步探索的点？

【1. 因子的操作化与因果验证】未来工作可以把 idea factor 从自然语言标签推进为可干预的实验变量：不仅用排序器解释得分差异，还通过定向修改程序中的因子做干预实验，检验因子与分数之间的因果关系，区分“可解释的相关物”与“真正可复用的机制”。
【2. 理论条件的可检验化】论文给出“何时提升样本效率”的理论条件，后续可以把因子解释力、抽取噪声、排序器误差、探索成本、预算规模写成可测量量，并在实验中拟合这些量与收益的关系；进一步给出有限预算下的样本复杂度界，而不只是渐近结论。
【3. 因子发现机制】当前的未解释对选择依赖分数差与排序器残差；未来可以探索主动学习、贝叶斯实验设计、因果发现或信息增益来选择“最值得标注”的程序对，降低因子抽取成本，提高新因子发现的命中率。
【4. 层次化与组合式因子】开放式发现中的想法往往有层次：从算法范式、数据结构到局部技巧。未来可以研究层次化因子库、因子组合、跨任务因子迁移和因子冲突消解，避免因子库随着任务增多而碎片化。
【5. 探索—利用分配策略】可以把 idea-guided slots 的分配建模为 bandit、MCTS 或预算分配问题，根据因子不确定性、平台期信号和剩余预算动态调整探索；同时研究 warm-up 多样化与后期收敛的最优切换点。
【6. 跨领域迁移】最值得推进的是把方法迁移到生物、材料、化学和实验科学：这些领域的反馈更稀疏、噪声更大、实验成本更高，想法因子可能需要包含机制假设、实验条件和不确定性，而不能只从代码差异中抽取。
【7. 评测与可复现性】建立计算预算匹配、多种子置信区间、逐任务胜负、因子命中率和失败案例的标准化报告；开放 prompt、因子库、排序器、日志和随机种子，才能让“5.0x 样本效率”可复核。
【8. 安全与失败模式】开放式发现代理可能奖励黑客、记忆污染或传播错误因子；未来需要研究因子校验、记忆去毒、异常检测、人类科学家在环审核，以及错误因子导致负迁移时的回滚机制。
【9. 多智能体与人机协作】可以把想法因子作为多智能体之间的共享语言，让不同代理分别负责提出、反驳、实验和总结；也可以研究人类科学家如何编辑因子库，使系统更可解释、更符合领域先验。

Q7: 总结一下论文的主要内容

【论证主线】论文从 AI Scientist 与进化式编码代理的兴起切入：AlphaEvolve 一类系统让 LLM 迭代修改程序，在数学、算法发现等开放式问题上取得成功；但作者指出，这类系统对程序的变异主要是低层、表层启发式的，缺少一个可持久保留的控制单元。科学知识不是一堆代码片段的堆积，而是想法在假设检验中不断累积。于是论文提出核心问题：能否把“想法”作为经验复用单位，让进化式发现既避免重复错误，又能在平台期提出新方向？
【技术主线】ExpEvolve 的做法是在现有进化骨架之上加一层想法控制。它从程序归档中抽取高层 idea factors，用这些因子解释程序之间的得分差异；具体地，对已标注因子的程序对拟合 pairwise ranker，让排序器预测哪个程序更好。随后，系统挑选那些分数差距大、但当前因子无法解释的程序对，把它们视为“缺少某个想法因子”的信号，继续抽取新因子。这样，因子库不是一次性摘要，而是随搜索逐步扩展的持久记忆。因子反过来控制搜索：在利用阶段，用因子指导候选生成与选择；在发现进入平台期时，用因子空间中的未充分探索区域引导探索。论文还把该框架嵌入 SCS/OpenRSI 的 Draft、Improve、Debug、Crossover 算子，并用 warm-up 多样化、想法记忆摘要和成功程序示例等设计来稳定早期搜索。
【理论主线】论文声称刻画了“基于想法因子的发现何时能提升样本效率”的条件。可确认的是，理论关注的是：当高层因子能够解释程序得分差异时，发现和复用这些因子能否减少试错；但可见片段没有给出定理的正式假设、界或证明结构。因此，读者需要回到 Method/Theory 部分核对：因子空间的容量、抽取噪声、排序器误差、探索成本如何进入样本复杂度；条件是否能在实验中度量；以及结论是渐近的还是有限预算的。
【实验主线】实验覆盖 11 个开放式数学任务：9 个来自 SCS/AlphaEvolve 的问题，包括第一、第二自相关不等式、不确定性不等式、Erdős 最小重叠、集合和差、最大最小距离比、三角形 Heilbronn 问题、11 维亲吻数、圆 packing；外加 Hadamard 矩阵问题和用线性规划恢复半径的圆 packing 变体。每次运行评估 200 个程序，采用 10 代×20 个与 20 代×10 个两种预算配置。基线包括 SCS（backbone 和主基线）与 ShinkaEvolve；作者还把 ExpEvolve 接入 OpenRSI 的 MLE 进化代理，在 MLS-Bench 的 6 个机器学习任务上测试，并迁移到 4 个其他 LLM。指标是预算结束时的最佳分数，报告多种子均值/标准差与最佳种子。
【结果】摘要报告最终性能与最高 5.0x 样本效率提升，且改进可迁移到 4 个 LLM 和 6 个 MLS-Bench 任务。Results 片段进一步称，idea-guided actions 产生约一半的新最优程序；在 Sums and differences 的 10 代×20 个程序示例中，11 个新最优里有 6 个来自 idea-guided slots。分析将这些现象解释为：ExpEvolve 发现的想法因子改善了探索与利用。需要注意，这些是作者报告的结果；可见片段没有给出绝对分数、置信区间、5.0x 的计算口径，也没有说明 idea-guided slots 的预算占比，因此“一半新最优”不能直接等同于超额贡献，仍需核对命中率与成本。
【结论与边界】论文的贡献在于把经验复用从“程序/摘要”提升到“想法因子”，并用排序器的残差来驱动新因子发现，形成发现—解释—控制的闭环。其最强主张是样本效率与跨模型/跨任务迁移；最需要外部验证的是因子的因果效度、理论条件的可检验性、抽取成本，以及在生物/实验科学等更稀疏反馈场景中的泛化。整体上，这是一篇把 LLM 进化搜索与科学假设形成机制连接起来的工作，适合按“方法定义—理论假设—预算公平—失败模式”四条线精读。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与画像中的 agent 方向直接重合：论文研究 LLM 进化式编码智能体的经验复用、记忆与探索控制，可作为 agent memory/experience 设计的参考。

## 基本信息

- 作者：Yongqiang Chen, Chenxi Liu, Deyu Zou, Zhenhao Chen, Tongliang Liu, Bo Han, Kun Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-21
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.29303v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Introduction、Results 与 References 片段；由于 Method、理论证明和完整实验表未在证据中展开，文中对机制细节与因果趋势的表述已标注为推断或证据缺口。
