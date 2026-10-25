---
user_id: "cheng tan"
paper_id: 14576
arxiv_id: "2609.38288v1"
title: "AREX-2: Advancing Self-Improving Agents through Long-Horizon Reflective Tasks"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.38288v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38288v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-12T12:56:18"
---
# AREX-2: Advancing Self-Improving Agents through Long-Horizon Reflective Tasks

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-improving agents · long-horizon refinement · test-time scaling · reflection

## 一句话总结

AREX-2 提出并验证了一条通向 self-improving LLM agents 的路线：在机器学习工程与算法编程等可验证反馈领域中合成 long-horizon improvement trajectories 并训练 27B 规模模型，使其学会在多轮测试时迭代中自行判断并改进方案；该能力不仅带来源域领先成绩，还零数据迁移到深度研究任务，并随轮次预算增长持续提升。

## 摘要

> We present AREX-2, an effort to advance the self-improving capability of LLM agents, which we define as the ability to iteratively refine a solution at test time. This ability rests on two complementary capabilities: reflection, which produces a solution better than the current one, and long-horizon execution, which keeps the iteration effective over many rounds. We hypothesize that both capabilities are domain-agnostic, and can therefore be learned in scenarios that are well suited for supervision. Accordingly, we synthesize long-horizon improvement trajectories from machine learning and algorithmic programming tasks, two domains that offer verifiable feedback and reward sustained iteration. Trained on this data, our agent, built on Qwen3.8-27B, achieves strong results on MLE-bench Lite (81.8) and Frontier-CS (70.7), transfers to deep research with 84.0 on BrowseComp, 52.6 on HLE, 92.2 on GAIA, and 93.8 on DeepSearchQA, and keeps improving as its budget of rounds grows. These results show that long-horizon reflective data is an effective route toward self-improving agents.

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题：把「改进循环」从系统脚手架（scaffold）移入模型内部。\n\n1. 现状困境：难题很少一次尝试就能解决——训练 pipeline、打分函数下的程序、研究问题都依赖「尝试-度量-修订」的循环。现有系统中的循环主要由 scaffold 实现：harness 决定何时重试、保留什么，而模型只被要求每次产出一个尝试。\n2. 本文的问题定义：self-improvement 被定义为——给定更多轮次（rounds），agent 凭借自己对「改什么」的判断，把更多轮次转化为更好的解决方案。这本质上是单任务内的 test-time scaling：轮次预算越多，解应越好。\n3. 能力拆解：该定义可分解为两个互补能力——reflection（每轮产出比当前更好的方案，即提高每轮增益）与 long-horizon execution（让迭代在多轮后仍保持有效，即维持正增益的轮数够长）。形式化地用 st = max S(yk)（历史上最好分数）与 rt = E[st−st−1]（第 t 轮的期望增益）刻画，T*≤T 为 rt>0 的轮数；如果 rt 在几轮后就衰减为零，增加预算就没有意义。\n4. 数据困境：reflection 与 long-horizon execution 都无法从「成品答案」（finished solutions）中学会——最终解不包含失败、试错、回退与恢复过程，而这些过程恰恰是需要学习的行为。因此需要构建「记录工作过程」的数据：agent 在多轮中逐步改进方案的轨迹。\n5. 领域选择问题：什么样的场景适合监督这些能力？论文选择机器学习工程与算法编程，理由有二——它们的反馈无歧义（验证分数或 judge 判定直接显示修改是否有帮助）；它们留有持续改进的空间（不是一次到位就能满分）。这里的关键假设是：两种能力是领域无关的，可以在适合监督的场景学会，再迁移到其他领域。\n6. 待验证命题：长程改进轨迹能否带来源域强性能（MLE 工程与算法编程）？学到的能力能否迁移到深度研究（未训练领域）？增加轮次预算是否持续转化为进步？

Q2: 有哪些相关研究？

1. 循环式系统背景：引言引用了 Jiang et al. 2025、Novikov et al. 2025、Lu et al. 2024、Yang et al. 2026 等工作，它们都把「训练 pipeline、打分函数下的程序、研究问题」的 try-measure-revise 循环作为系统设计核心。本文的批评点在于：这些系统的循环大部分实现在 scaffold 中，而不是在模型内——本文正是要移动这个边界。\n2. 前代 AREX 系列：论文明确 AREX-2 相对 previous AREX models 在 deep research 上取得显著提升（BrowseComp 84.0、HLE 52.6、GAIA 92.2、DeepSearchQA 93.8），但检索片段未给出前代模型的具体数值，需要在原文结果表中确认提升幅度。\n3. 评测基准谱系：Frontier-CS（Mang et al., 2025，开放式算法编程、分级打分、多轮提交改进）；MLE-bench（Chan et al., 2025，端到端 ML 工程工作流）；BrowseComp（Wei et al., 2025，深度研究/浏览检索）；HLE（Phan et al., 2025，人类最后考试）；GAIA（Mialon et al., 2023，通用助手级任务）；DeepSearchQA（Gupta et al., 2026，深度搜索问答）。\n4. 规模对比对象：图 2 展示了 4B–10T 参数区间的模型规模-性能曲线；检索片段明确提及 DeepSeek-V4-Pro 与 Kimi-K2.6 作为大规模对比系统。\n5. 研究脉络定位（合理推断）：该工作处于 self-improvement / test-time scaling / reflection（自我修正）与 agent 长程任务（deep research agents）的交汇处。值得注意的定义差异：本文把「自改进」限定为单任务内多轮改进，而不是通常意义上的「用模型自身输出训练下一代模型」的数据飞轮；这一边界设定是理解全文的钥匙。\n6. 方法脉络（推测）：用可验证反馈领域 + 轨迹级监督训练的组合，与近年 RLVR（可验证奖励强化学习）和过程监督/结果监督文献有潜在关联，但检索证据未给出相应引用与对比讨论，需回原文确认。

Q3: 论文如何解决这个问题？

总体思路：与其在 scaffold 中实现重试循环，不如在「反馈可验证、能奖励持续迭代」的领域中合成 long-horizon improvement trajectories，训练模型学会在多轮中自己判断改什么、以及从失败和回退中恢复。\n\n1. 形式化框架（Section 2.1）：环境 e=(σ, S)，σ 是任务，S 是打分函数（如训练模型的验证指标、程序通过的隐藏测试比例），分数越高越好。第 t 轮提交 yt 并收到反馈 ft（分数 + 环境报告的其他信息：日志、错误、计时）。「一轮」包含提交前的一切动作——读、编辑、运行、搜索。n 轮轨迹记为 τ=(y0,f0,y1,f1,...,yn,fn)；历史最优 st = max_{k≤t} S(yk)；第 t 轮期望增益 rt = E[st−st−1]；T*≤T 为 rt>0 的轮数（有效水平）。这一框架把「自改进」变成可度量的量：总改进 = Σ rt，而 T* 衡量长程执行能力。\n2. 环境构建：从 GitHub 仓库与在线评测（online judges）两个来源构造环境；只有通过执行与验证（execution and validation）的环境才被保留。这一步保证合成轨迹是可运行、可打分的，排除构造不出有效反馈的环境。\n3. 轨迹合成：让 agent 在构造好的环境中多轮改进方案。过程中按需向在线资源查询，获取 operational knowledge（操作知识）供后续使用；任务特定技能（task-specific skills）通过检索与任务相关的信息构建，并且「与任务本身的材料」被排除，以避免数据泄漏。失败的尝试和回退（regressions）保留在 context 中，让模型看到它必须从中恢复的状态；训练 loss 施加在这些失败/回退之后的决策上（检索证据明确：loss is applied to the decisions that follow them）——这是让模型学会「从错误中恢复」的关键设计。\n4. 预算感知训练：agent 被明确告知预算——轮次数与 wall-clock 时间都远大于单次尝试所需（每任务量级为数小时、数百次工具调用）。「Knowing the budget changes how the agent works」：预算信息会改变 agent 的工作方式，即在何时尝试、何时反思、何时收尾之间做策略分配（具体机制需原文展开，检索片段只给出结论性表述）。\n5. 基座与训练：agent 基于 Qwen3.8-27B 构建（摘要表述）；检索片段未提供优化器、学习率、数据规模、正负样本比例等训练细节，这些需要回原文 Method 与附录确认。\n6. 技能组织（由消融反推）：训练内容至少包含 general skills（通用实践）与 task-specific skills（按任务检索构建）两个层面，消融显示它们各自贡献提升，且通用技能单独就能把 base model 从 28.8 提到 41.2。

Q4: 论文做了哪些实验？

1. 总体设计：六个基准覆盖四种能力——算法编程（Frontier-CS，开放式题目、分级打分，agent 通过反复实现-测试-提交改进方案）；机器学习工程（MLE-bench Lite，端到端 ML 工作流：数据分析、实验、实现、模型评估）；深度研究与通用 agentic reasoning（BrowseComp、HLE、GAIA、DeepSearchQA，覆盖深度研究、工具增强推理、信息收集、多步任务完成）。\n2. 指标协议：DeepSearchQA 报告 F1；MLE-Lite 报告 Any Medal（agent 赢得奖牌的竞赛占比的平均）；其余基准报告 accuracy，遵循标准评测协议。\n3. 总览结果：AREX-2 在 MLE-bench Lite 81.8、Frontier-CS 70.7；深度研究：BrowseComp 84.0、HLE 52.6、GAIA 92.2、DeepSearchQA 93.8。图 2 绘制 4B–10T 参数规模区间的模型规模-性能曲线，显示 27B 的 AREX-2 匹配或超过大得多的模型。\n4. 预算扩展实验（对应形式化的直接检验）：问题为「更多预算是否产生更好方案、有效水平 T* 是否长」。两个设置——一个有正确性信号（Frontier-CS：改进持续到第五个小时）；一个没有 correctness signal（BrowseComp：没有正确性信号的情况下仍随预算提升）。\n5. 分阶段消融：base model 得分 28.8；加入 general skills 后提升到 41.2；再加 task-specific skills（检索片段在此截断，最终数值未知）；随后是训练与更大预算的贡献。结论：operational knowledge、训练、更大的预算「各自都有贡献」（each contribute）。\n6. 深度研究迁移实验：训练数据完全不包含深度研究任务，直接测试四个深度研究基准，并与前代 AREX 模型及更大规模系统（如 DeepSeek-V4-Pro、Kimi-K2.6）对比。\n7. 信息缺口：检索证据未覆盖所有 benchmark 的逐项对比表、推理成本统计、轨迹长度分布等，完整实验矩阵需回原文确认。

Q5: 发现了什么实验现象？

1. 规模-性能反常：AREX-2 仅 27B 参数，却在 BrowseComp 上超过 DeepSeek-V4-Pro 与 Kimi-K2.6，并在 DeepSearchQA 上于表格所列模型中排名第三，四个基准上整体匹配或超过远大于它的模型。这说明 long-horizon reflective data 带来的行为能力在相当程度上可以「替代参数规模」，是本文最引人注目的现象。\n2. 跨域迁移现象：深度研究基准（BrowseComp/HLE/GAIA/DeepSearchQA）没有任何新增训练数据，分数却显著高于前代 AREX 模型——直接支持「reflection 与 long-horizon execution 是领域无关能力」的核心假设。迁移不是靠任务重叠，而是靠通用的「多轮改进行为」。\n3. 预算-性能关系：Frontier-CS 的改进持续到第五个小时，说明有效水平 T* 很长，正增益轮次没有在早期耗尽；BrowseComp 在「没有正确性信号」的条件下仍随预算提升——这一现象暗示模型学会的改进判断并不完全依赖外部奖励，而是能利用日志、错误、执行结果等上下文信息自主决定改什么（推测：轨迹训练把这种内部判断内化到了模型权重中）。\n4. 消融走势：28.8 → 41.2 的大幅跳跃来自通用操作技能（operational knowledge）这一单一成分；之后任务特定技能、训练、更大预算再继续加分。没有单一成分起决定性作用（「each contribute」），与「能力可组合叠加」的叙事一致。\n5. 指标间张力与信息缺口：HLE（52.6）与 GAIA（92.2）之间差距很大，可能反映两基准的难度分布与评估协议差异，但可得片段中论文未对此展开讨论；检索证据也没有覆盖失败案例、T* 较短的任务类型、或 reflection 产生退化的负结果分析，这些是值得回原文补读的部分。

Q6: 有什么可以进一步探索的点？

1. 论文明示的路径（结论段）：拓宽训练领域（widen the training domains）、拉长 horizon、闭环（close the loop）——让模型自身产生的轨迹成为它自己的训练数据，即自举式的轨迹飞轮。\n2. 自我轨迹飞轮的质量控制：模型自生成轨迹直接作为训练数据需要解决过滤、去重、分布漂移问题；是否需要验证器/奖励模型对自生成轨迹打分，以及如何在「正确性信号缺失」的领域维持数据质量，都是开放问题。\n3. 预算感知的深化：当前做法是「告诉 agent 预算」，可以进一步把预算做成显式条件变量（budget-conditioned policy），让模型学会更精细的预算-策略映射，甚至自主规划预算分配。\n4. 更远领域的迁移验证：论文只验证了 ML 工程与算法编程到深度研究的迁移。与用户画像相关的方向——生物/科学模拟实验、数学证明、代码评审等具有可验证反馈的场景——是否适用同一配方，是可直接开展的实证问题（合理推断：可验证模拟器与 ML 工程在反馈结构上同构，但论文未做此验证）。\n5. 无正确性信号条件下持续改进的机理研究：BrowseComp 现象表明模型发展出了某种内部评价/自我验证机制，单独研究这一机制（它依赖什么上下文线索、何时失效）有较高价值。\n6. 失败模式研究：哪些任务上 T* 短、reflection 何时产生回退、失败与回退保留在 context 带来的长上下文成本如何随轨迹长度增长，以及能否用记忆/摘要压缩替代完整保留。

Q7: 总结一下论文的主要内容

一、论证主线：论文从一个观察出发——难题靠「尝试-度量-修订」的循环解决，但现有系统把循环实现在 scaffold 中，模型只是循环里的单次尝试生成器。论文主张把循环移入模型内部，并把这种能力命名为 self-improvement：给定更多轮次，agent 依靠自己的判断把轮次变成更好的解。作者把该能力分解为 reflection（每轮增益大）与 long-horizon execution（正增益轮数多），并给出形式化刻画：轨迹 τ=(y0,f0,...,yn,fn)，历史最优 st=max S(yk)，每轮期望增益 rt=E[st−st−1]，有效水平 T*≤T 为 rt>0 的轮数。形式化的意义在于把「自改进」从口号变成可测的量：总改进等于各轮增益之和，预算增长是否有用取决于 T* 是否长。\n\n二、技术主线：既然 reflection 与 long-horizon execution 无法从成品答案中学到，就需要记录工作过程的轨迹数据。论文选择 ML 工程与算法编程两个领域合成数据，理由是反馈无歧义、可奖励持续迭代，并假设两个能力领域无关，可在这些便于监督的场景中学会。技术上分三步：从 GitHub 仓库和在线评测构建环境（只有通过执行与验证的环境才保留）；让 agent 在多轮中改进方案，按需获取 operational knowledge、检索任务特定信息（排除任务本身材料以防泄漏），失败与回退保留在 context 中，loss 施加在失败/回退之后的决策上，从而教会模型从错误状态恢复；训练时明确告知 agent 预算（轮次数与 wall-clock 时间，量级为数小时与数百次工具调用），因为「知道预算是如何工作的前提」。基座为 Qwen3.8-27B。\n\n三、实验主线：六个基准、四种能力——Frontier-CS（算法编程）、MLE-bench Lite（ML 工程）、BrowseComp/HLE/GAIA/DeepSearchQA（深度研究与 agentic reasoning）。结果：MLE-Lite 81.8、Frontier-CS 70.7、BrowseComp 84.0、HLE 52.6、GAIA 92.2、DeepSearchQA 93.8（F1）；图 2 显示 27B 的 AREX-2 在四基准上匹配或超过 4B–10T 的更大模型，并在 BrowseComp 上超越 DeepSeek-V4-Pro 和 Kimi-K2.6、在 DeepSearchQA 上排第三。预算扩展实验显示 Frontier-CS 到第五小时仍在改进，BrowseComp 在没有正确性信号的条件下仍随预算提升，说明有效水平 T* 长且不依赖外部奖励。分阶段消融显示通用操作技能（28.8→41.2）、任务特定技能、训练、更大预算各自贡献。深度研究四个基准没有新增训练数据，相对前代 AREX 模型显著提升，验证了跨域迁移。\n\n四、结论与展望：长程反射能力在其可监督的领域学会后，能迁移到未训练领域。作者给出的前路是拓宽训练领域、拉长 horizon、闭环——让模型自己的轨迹成为其训练数据。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户「agent」方向直接重合：论文核心就是 self-improving LLM agents，讨论 scaffold 与模型内部循环的边界、test-time scaling、多轮工具调用行为，是该方向的前沿实证

## 基本信息

- 作者：Hongjin Qian, Chaofan Li, Kun Luo, Wenqing Wei, Jianlyu Chen, Shuqi Lu, Yuyang Hu, Hongwang Xiao, Hui Wang, Chaozhuo Li, Qiwei Ye, Zhicheng Dou, Defu Lian, Zheng Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-09-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38288v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（39 个 chunk，覆盖 Introduction、Method、Results、Conclusion 与形式化公式），并结合论文元数据与摘要完成；检索未覆盖的细节已在相应字段内标注为合理推断或推测。
