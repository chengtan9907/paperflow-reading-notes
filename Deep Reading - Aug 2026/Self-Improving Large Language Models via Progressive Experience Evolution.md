---
user_id: "cheng tan"
paper_id: 6240
arxiv_id: "2608.02139v1"
title: "Self-Improving Large Language Models via Progressive Experience Evolution"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02139v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02139v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T02:00:34"
---
# Self-Improving Large Language Models via Progressive Experience Evolution

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-improving language models · experience distillation · reinforcement learning · on-policy self-distillation

## 一句话总结

SPEE 通过将显式经验演化（提取、验证、渐进演化可转移经验并经特权引导的在线策略自蒸馏内化到参数中）与隐式策略优化（奖励驱动强化学习探索新策略）耦合，形成闭环的自改进后训练框架，在五个数学推理基准和三个模型规模上一致超过测试时与训练时自进化基线。

## 摘要

> Large language models (LLMs) capable of self-improvement require not only effective policy optimization, but also a principled mechanism for transforming transient interaction experience into persistent model capabilities. Existing self-improvement paradigms remain fragmented: test-time methods can explicitly extract experience but cannot internalize it into model parameters, whereas training-time optimization methods can update model parameters but lack an explicit mechanism for accumulating transferable experience. Bridging these two paradigms requires a critical intermediate stage that remains underexplored, namely \emph{experience distillation}. To address this gap, we propose \textbf{SPEE} (\textbf{S}elf-\textbf{P}rogressive \textbf{E}xperience \textbf{E}volution), a unified post-training framework that sequentially performs explicit experience evolution followed by implicit policy optimization. During explicit experience evolution, SPEE reflects on trajectories collected from multiple interactions to extract, verify, and progressively evolve transferable experience, which is subsequently internalized into the policy through privilege-guided On-Policy Self-Distillation (OPSD). During implicit policy optimization, reward-driven reinforcement learning leverages these internalized priors to explore novel solution strategies. In the experience evolution stage, a continuously evolving global experience pool consolidates knowledge from both successful and failed trajectories, filters out low-utility experience, and mitigates post-hoc rationalization induced by individual trajectories. Experiments on five mathematical reasoning benchmarks demonstrate that SPEE consistently outperforms both test-time and training-time self-evolution baselines across three model scales. The source code is available at https://github.com/rrrsj/SPEE.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：LLM 如何在无需人类干预的情况下实现稳定且持续的自进化，特别是如何将瞬时的交互经验沉淀为可复用、持久的模型能力。作者指出现有自改进范式是碎片化的：测试时方法（例如上下文学习、检索增强、提示优化等）能够显式提取经验，但经验仍留在模型参数之外，受限于上下文容量、检索准确性与泛化能力，无法被真正内化；训练时方法（以强化学习为代表）能够更新模型参数，但它们缺乏显式机制去积累可迁移的经验，或者说没有区别对待经验与原始轨迹。因此两者之间缺少一个关键的中间阶段——经验蒸馏。论文还提出两个现有训练时方法的具体缺陷：(1) 完整的轨迹把可迁移知识与实例特有内容纠缠在一起，难以形成紧凑的经验表示；(2) 它们没有提供一条从经验提取到参数内化再到策略探索的完整管线。SPEE 就是为补齐这一缺口而设计的统一后训练框架。

Q2: 有哪些相关研究？

相关研究可分为几类。第一类是测试时自我改进方法：模型在推理时利用额外计算或外部记忆来改进输出，例如上下文学习、检索增强等；这类方法可以显式地使用经验，但经验始终位于参数之外，受上下文容量、检索准确性和泛化能力的限制。第二类是训练时方法，尤其是基于强化学习的策略优化（如 RLHF/PPO 类方法），它们能直接更新参数，但往往缺少对可迁移经验的显式建模和积累。第三类是策略自蒸馏方法，文中特别提到已有的在线策略自蒸馏方法（如 Hübotter 等），说明这类方法提供了一条把策略自身输出蒸馏回策略的途径，但它的初始化与经验演化结合仍不充分。第四类是越来越受关注的自我进化系统（如 self-play、self-improvement 类工作），论文指出现有方法更多按优化目标分类，而不是按能力提升机制分类，而作者认为机制是更本质的维度。总体来说，此前的工作没有把经验演化和策略优化放在同一个闭环里，SPEE 试图填补这一空缺。

Q3: 论文如何解决这个问题？

SPEE 采用先显式经验演化，后隐式策略优化的两阶段闭环设计。第一阶段是显式经验演化：模型在多次交互中获得多条轨迹（同时包含成功与失败），通过反思从这些轨迹中提取紧凑、可迁移的经验——包括可复用的推理策略、任务不变约束、反复出现的失败模式等；随后对经验进行验证和渐进演化；所有经验被放入一个持续演化的全局经验池，该池会合并来自成功和失败轨迹的知识、过滤低效用经验，并缓解由单条轨迹引发的事后合理化（即模型事后给失败或不一致的轨迹编造合理叙事）问题。经过筛选和演化的经验通过 Privilege-Guided On-Policy Self-Distillation（OPSD）被内化到当前策略的参数中。第二阶段是隐式策略优化：在经验蒸馏给出更强的初始化后，用奖励驱动的强化学习进一步优化策略，鼓励模型探索超出既有经验的新求解策略，从而发现更优行为；而新产生的交互轨迹又为下一轮经验演化提供更丰富的素材，形成递进式的自我改进循环。整个框架的经验中心视角强调：经验应去除实例特有细节、能跨问题泛化，并随策略一起演化。

Q4: 论文做了哪些实验？

根据摘要，实验在五个数学推理基准上进行，覆盖三个模型规模，对比基线包括测试时自进化方法和训练时自进化方法。具体数据集名称、模型规模数值、基线列表、训练超参和评估协议在本次检索到的片段中未完整给出（论文 PDF 的 Results 部分为空，引言/结论片段未包含数字细节），因此无法逐一列出。合理推断实验设计会包括：(1) 同一基础模型在不同规模下的 SPEE 微调前后对比；(2) SPEE 与 test-time 自进化（如推理时反馈/反思）以及 training-time 自进化（如 RL/self-distillation）的对照；(3) 可能的消融实验，例如去掉全局经验池、去掉验证步骤、去掉 OPSD 或去掉 RL 阶段。需要回原文确认具体基准和数值。

Q5: 发现了什么实验现象？

从检索到的训练动态片段可以读出两个现象：其一，经验蒸馏能够把策略移向高奖励区域，同时避免过早的策略崩溃，也不会过度限制响应多样性；其二，论文在数据效率标题下讨论了相关发现，片段显示“从密集……中受益”（原句被截断），合理推断 SPEE 相比纯 RL 基线在数据效率上更有优势，即用更少的交互或样本就能达到相当的性能。此外，摘要和引言暗示，显式的经验演化缓解了单条轨迹导致的事后合理化问题，这可能是保证训练稳定性的关键因素。由于没有具体数值，以上均属定性现象；具体趋势（如 scaling 效应、不同规模下的增益差异、失败案例）需查看原文实验图表。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：(1) 将 SPEE 从数学推理扩展到代码生成、工具调用、智能体交互等更广泛的任务，检验经验定义的普适性；(2) 在更大规模模型和更长训练周期下，观察经验池的容量、更新策略与遗忘问题，研究全局经验池的长期演化动力学；(3) 深化对事后合理化缓解机制的分析，例如它具体是如何被全局经验池抑制的，以及是否对所有类型的失败轨迹都有效；(4) 将 OPSD 与其他蒸馏目标（如长度控制、风格控制）结合，或探索不同特权信号的选择对蒸馏效果的影响；(5) 把 SPEE 的显式经验表示可视化，分析模型内化的是否是真正可迁移的通用策略；(6) 将 SPEE 与开放环境、持续学习设定结合，测试其在分布漂移下的自适应性；(7) 研究经验演化与 RL 探索之间的权衡：过多的经验先验是否会限制探索，OPSD 的强度如何随训练阶段自适应调整。

Q7: 总结一下论文的主要内容

论文围绕“LLM 自改进的本质是什么”这一问题展开。作者提出，自改进不应只被看作策略优化问题，而应被看作“把瞬态交互信号转化为可复用、持久模型能力”的问题。他们引入“经验”概念：从交互轨迹中提炼出的紧凑、可迁移知识，包括可复用推理策略、任务不变约束和反复出现的失败模式。基于此，作者指出现有测试时方法和训练时方法各有短板：前者能显式使用经验但不能参数化内化，后者能参数化更新但缺乏可迁移经验的显式积累机制；两者之间缺少“经验蒸馏”这一中间阶段。为填补这一空白，论文提出 SPEE（Self-Progressive Experience Evolution），一个统一的后训练框架。SPEE 先执行显式经验演化：从多次交互收集的成功与失败轨迹中反思、提取并验证经验，通过持续演化的全局经验池过滤低效用经验、缓解事后合理化，然后通过特权引导的在线策略自蒸馏（OPSD）把经验内化进策略参数。接着执行隐式策略优化：用奖励驱动的强化学习在内化先验的基础上探索新策略，产生更丰富的轨迹，从而闭环推进自我改进。实验在五个数学推理基准、三个模型规模上显示 SPEE 一致超过测试时与训练时自进化基线；训练动态分析还表明经验蒸馏在提高奖励的同时避免策略崩溃、保留响应多样性，并可能带来数据效率优势。论文的贡献在于提出了一个经验中心的自进化视角、一个完整的经验蒸馏-策略优化闭环，以及一个可直接使用的开源实现。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：这篇论文与你画像中的智能体（agent）方向直接相关，因为智能体场景天然产生大量交互轨迹，SPEE 的“经验提取-蒸馏-优化”闭环可以直接用于智能体策略的自改进。

## 基本信息

- 作者：Shijie Ren, Xiting Wang, Meng Li, Yujie Guo, Yunhang Yao, Ziheng Peng, Xunlong Wang, Yuetan Chen, Haoyang Zhou, Yunlong Liang, Fandong Meng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02139v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（background/method/results/limitations 等片段）并补全了 heuristic_draft；但检索到的方法和实验细节有限，部分内容基于摘要和片段合理推断，具体数值和实验设置需回到原文核对。
