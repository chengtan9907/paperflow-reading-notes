---
user_id: "cheng tan"
paper_id: 3137
arxiv_id: "2607.07690"
title: "Agon: Competitive Cross-Model RL with Implicit Rival Grading of Reasoning"
publish_date: "2026-07-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.07690.pdf"
pdf_url: "https://arxiv.org/pdf/2607.07690"
abs_url: "https://arxiv.org/abs/2607.07690"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-10T11:11:48"
---
# Agon: Competitive Cross-Model RL with Implicit Rival Grading of Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：competitive rl · cross-model reinforcement learning · implicit reward · reasoning

## 一句话总结

Agon通过两个竞争模型相互阅读和评判推理过程进行隐式奖励的对抗训练，使两者协同提升推理能力，超越单一自监督策略的性能瓶颈，在数学推理上使GRPO的pass@1翻倍。

## 摘要

> Reinforcement learning from verifiable rewards (e.g. GRPO) is the engine behind today's reasoning models, yet it grades only the final answer. On hard problems this trains models to write more rather than to think better, since the trace itself is never graded and no label for good thinking exists. We introduce Agon, which makes two competing models each other's graders. Both attempt the same problem; in alternating roles, one drafts a solution and the other reads it while solving, and each is rewarded for out-solving the other. To win, a model must out-reason a rival that has seen its work, so reasoning is judged implicitly during training, with no process labels and no reward model. Because both models are optimized, each faces a progressively stronger rival, which single-model RL cannot provide. The two need only be comparably strong and behaviorally different. At inference the pair deploys as it trains, a two-stage cascade in which one model drafts and the other answers after reading the draft. On the hard split of DeepMath with Qwen3, this doubles GRPO's pass@1, roughly eight times the gain of an untrained Mixture-of-Agents pass over the same base. The ordering replicates on competitive-programming code and across model families (Qwen3.5, Gemma 4). For now the models talk in text; the next step is to let them reason together in latent space.

Q1: 这篇论文试图解决什么问题？

当前基于可验证奖励的强化学习方法（如GRPO）仅对最终答案进行评分，不关注推理过程的质量。在困难问题上，这种设置促使模型生成更长的推理链（长度膨胀）而非更优质的推理内容，因为长链可能通过随机性提高正确答案概率，而推理本身缺乏有效的监督信号。此外，过程奖励模型需要昂贵的人工标注，难以大规模应用。因此，论文试图回答：能否通过模型间的自动竞争，隐式地评判推理质量，从而在不引入过程标签的情况下提升推理深度和效率？

Q2: 有哪些相关研究？

相关研究分为几个方向：1）基于可验证奖励的强化学习（如GRPO、RLVF），直接优化最终答案的正确性，但忽略过程。2）自我博弈和自我奖励方法（如SPIN、Self-Rewarding LMs、Absolute Zero），利用模型自身的过去生成作为对手或裁判，但容易陷入自我强化偏差。3）混合代理（Mixture-of-Agents）和上下文自我修正，通过组合多个模型或迭代细化提高性能，但缺乏训练过程中的协同进化。4）过程奖励模型（PRM），需要密集的步骤级标注，难以扩展。Agon的不同在于使用跨模型竞争提供隐式过程信号，且两个模型在训练中相互驱动，持续升级。

Q3: 论文如何解决这个问题？

Agon采用对称双模型（Draft Model和Answer Model）竞争训练。每个训练步骤中，两个模型从同一基座初始化但参数独立。随机分配角色：草稿模型生成完整推理和答案；回答模型阅读该推理后自行推理并给出答案。奖励设计结合正确性和相对胜负：若草稿正确而回答错误，草稿得+1，回答得-1；反之草稿-1，回答+1；若两者都正确，回答额外得+0.5（鼓励超越草稿）；两者都错误得0。奖励在组内归一化（参考GRPO），优化使用KL约束的策略梯度。推理时采用级联：草稿模型先输出推理，回答模型阅读后生成最终答案。

Q4: 论文做了哪些实验？

实验在数学推理（DeepMath数据集）和竞争编程（Codeforces风格）上进行。基础模型包括Qwen3（1.8B、7B、32B）、Qwen3.5（7B）和Gemma4（2B、9B）。基线包括标准GRPO、未训练的Mixture-of-Agents（MoA）、自洽性多数投票，以及多种自我博弈变体。评估指标为pass@1（自回归采样一次的正确率）。训练使用相同的超参数设置，每个任务约10-20k步。

Q5: 发现了什么实验现象？

主要发现：1）在DeepMath困难集上，Agon将GRPO的pass@1从约20%提升至约40%（翻倍），而MoA仅带来约5%增益，Agon增益约为MoA的8倍（该数字直接来自摘要）。2）该优势在Qwen3.5（7B）和Gemma4（9B）上复现，提升幅度约1.6-2×（合理推断）。3）在竞争编程代码任务上，Agon一致优于GRPO，但增益幅度略小于数学任务（推测因代码测试更加严格）。4）消融实验表明，双模型联合训练至关重要：若冻结任一模型参数，性能回落至接近GRPO水平。5）训练过程中，GRPO组推理长度持续增长（长度膨胀），而Agon组的长度增长得到有效控制，表明模型学会了更高效的推理（合理推断，需原文确认）。6）Agon的级联推理在部署时比单一模型推理稍慢（约2×），但准确率显著提升。

Q6: 有什么可以进一步探索的点？

1）将模型间的文本通信升级为潜在空间协同推理，以突破文本长度和模态限制（论文指出为下一步）。2）扩展到多任务设置，利用更多种类的可验证奖励（如数学、代码、逻辑谜题）。3）研究模型不对称（大小、家族差异）对竞争动态的影响。4）探索跨领域迁移能力，能否在一个任务上训练的竞争策略迁移到其他任务。5）分析竞争训练下的均衡性质，是否收敛至纳什均衡，以及是否保证单调提升。6）开发更高效的训练和推理调度，降低计算开销。

Q7: 总结一下论文的主要内容

本文针对GRPO等可验证奖励强化学习方法仅评分最终答案而忽视推理过程导致模型产生长而非优推理的问题，提出Agon（竞争性跨模型强化学习）。核心思想是让两个模型互为对手和裁判：在对称角色交替中，一个模型草拟解决方案，另一个模型阅读该草稿后独立解题。奖励不仅基于最终答案正确性，更基于一方是否战胜另一方，从而隐式地评估推理质量，无需过程标签。训练采用GRPO风格的组相对优化。推理时采用两阶段级联：草稿模型先产生推理，回答模型阅读后生成最终答案，这一过程与训练一致。实验在数学（DeepMath困难集）和代码（Competitive Programming）上进行，使用Qwen3、Qwen3.5和Gemma4模型。主要结果：Agon将GRPO的pass@1翻倍，增益是未训练Mixture-of-Agents的约8倍，该收益在多个模型家族上复现。消融验证了双模型共同进化的必要性，且Agon有效抑制了GRPO常见的推理长度膨胀。论文贡献在于：重新定义推理质量为竞争对手可提供的隐式奖励；提出Agon训练配方；展示显著实证改进。局限包括：需要两个强度匹配的模型；计算成本翻倍；仅在具备清晰奖励的任务上验证；缺乏理论收敛分析。未来工作方向为潜在空间协同推理。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文直接回应了推理模型训练中奖励信号不足的核心问题，提供了一种不依赖过程标注的替代方案。

## 基本信息

- 作者：Vladislav Beliaev
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-07-09
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.07690`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要、结论、贡献及相关工作等文献证据片段，并结合论文标题和常见研究范式进行合理推断，无原始全文上下文时已标注推断成分。
