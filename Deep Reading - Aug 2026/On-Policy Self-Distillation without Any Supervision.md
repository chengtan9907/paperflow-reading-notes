---
user_id: "cheng tan"
paper_id: 7003
arxiv_id: "2608.06296v1"
title: "On-Policy Self-Distillation without Any Supervision"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.06296v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.06296v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T14:05:08"
---
# On-Policy Self-Distillation without Any Supervision

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-distillation · on-policy learning · self-consistency · majority voting

## 一句话总结

本论文提出 U-OPSD，一种完全无外部监督的 on-policy 自蒸馏方法，利用模型自身多次采样的多数投票共识构造伪解，并将蒸馏聚焦到模型最长错误完成的前缀上，从而在数学推理基准上超越或匹敌依赖 ground truth 的监督基线。

## 摘要

> On-policy (Self-)Distillation (OPD / OPSD) has shown strong potential for post-training large language models (LLMs). However, existing methods still rely heavily on external supervision, including ground-truth signals, environmental feedback, or guidance from larger models, and therefore fall short of genuine "self"-distillation. In this study, we show that on-policy self-distillation can be achieved using only a model's own generations via internal consistency. We propose Unsupervised On-Policy Self-Distillation (U-OPSD). U-OPSD first samples multiple rollouts and constructs a pseudo-solution by majority vote under a self-consistency threshold. It then conditions a teacher distribution on the shortest pseudo-solution and distills it into prefixes of the model's longest incorrect completion, allowing the model to correct itself precisely where it is confidently wrong. Across diverse benchmarks, base models, and training settings, U-OPSD consistently improves over the base models and matches or surpasses supervised methods with ground truth (GT), such as OPSD and GRPO. On AIME24, AIME25, HMMT25, MATH500, and AMC23, U-OPSD improves over the base model by 8.5% and 10.7% on Qwen3 non-thinking mode at the 4B and 8B scales, respectively, and outperforms OPSD by an average of 3.2% and 2.3%. In thinking mode, U-OPSD remains on par with OPSD, outperforming it by 0.9% at 4B and matching it at 8B, while surpassing GRPO by 0.7% and 1.1%, respectively.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：现有的 on-policy 自蒸馏（如 OPSD）虽然名为“self-distillation”，但实际仍严重依赖外部监督信号，包括 ground-truth 答案、环境反馈或更大教师模型的输出。这种依赖带来多重代价：高质量标注难以获取、环境反馈在开放任务中往往不可用、依赖更大模型会引入额外成本和封闭生态，同时也削弱了“自我提升”这一自蒸馏理念的纯粹性。

具体而言，on-policy 自蒸馏的优势在于：它在学生模型当前访问的状态上提供密集的 token 级监督，能缓解 off-policy 蒸馏中训练与推理分布不匹配的问题（Gul 等，2024；Agarwal 等，2024）。但若要获得教师分布，传统做法仍然需要一个“正确解”作为条件，而这个正确解通常来自外部。作者要问的问题是：能否完全去掉这些外部信号，只靠模型自身的多次采样与内部一致性来生成训练信号？

这个问题的难点在于：1）模型自身生成的答案可能包含系统性错误，没有外部信号如何区分对错？2）自我训练容易陷入确认偏误（confirmation bias），模型可能强化已有错误；3）如何设计一种策略，既利用自洽信号，又能避开“模型集体犯错”的风险。作者给出的答案是：利用多数投票的共识作为“软证据”，并且只在模型“自信但错误”的地方进行纠正——即选择最长错误完成序列的前缀施加蒸馏信号，从而把自我纠正集中在最有信息量的位置。

因此，本文不仅是一个无监督后训练算法，更是对“自蒸馏”概念边界的重新定义：真正的 self-distillation 应该不依赖任何外部验证，只依靠模型内部的一致性。该问题对于降低 LLM 后训练对人工标注和外部模型的依赖，具有实际意义。

Q2: 有哪些相关研究？

本工作与多个研究方向密切相关，检索到的相关章节提到了以下脉络：

1. 后训练驱动的推理能力提升：LLM 推理能力的进步在很大程度上由后训练推动，包括监督微调（SFT）、知识蒸馏等（Ye 等，2025；Wen 等，2025）。这些方法通常需要标注数据或教师模型。

2. Off-policy 与 on-policy 蒸馏：off-policy 蒸馏使用静态数据集训练学生，存在训练与推理分布不匹配问题；on-policy 蒸馏让学生在自己采样的状态上学习，能提供密集的 token 级监督并缓解分布偏移（Gul 等，2024；Agarwal 等，2024）。OPSD 进一步让同一个模型在非对称上下文下同时扮演教师和学生，以“已验证解”为条件。

3. 自训练与自蒸馏：自训练利用模型自身预测作为监督，典型工作包括 self-training 迭代框架；Self-distilled Reasoner（Xie 等，2026）则将 on-policy 自蒸馏用于推理，但仍依赖外部验证信号。本文与这类工作的本质区别在于完全移除外部验证。

4. 一致性（consistency）与多数投票：不同采样之间的一致性可作为信号来源。多数投票（Wang 等，2023）、基于自一致性的测试时训练（Zuo 等，2025）都利用了这种一致性信号。本文把多数投票从推理时策略扩展到训练信号构造中。

5. 自奖励与自我改进：self-rewarding 语言模型（如 Yuan 等，2024）由模型自行生成奖励，但通常需要种子标注或人工设计的启发式；本文则完全不需要显式奖励，仅用离散的投票共识。

6. RL 后训练方法：GRPO 等基于群体相对策略优化的方法在数学推理上表现优异，但它们依赖 reward model 或规则奖励，属于外部监督。本文在对比中将 U-OPSD 与 GRPO 对照，展示无监督方法的竞争力。

总体来看，本文处于“无监督自我改进”与“on-policy 蒸馏”的交汇点，核心创新是用内部一致性替代外部正确性验证。

Q3: 论文如何解决这个问题？

U-OPSD 的方法核心是“用模型自己的共识取代 ground truth”，并设计一种有针对性的蒸馏机制。基于摘要与结论部分的证据，可将算法分解为以下步骤：

1. 多轮采样：对同一个输入问题，从当前模型采样多个完整 rollout（即多个候选回答/推理链）。

2. 多数投票构造伪解：在 self-consistency 阈值下对这些 rollout 的最终答案进行多数投票，得到一个高置信的共识答案；只有满足一致性阈值的样本才被采纳为伪解。这一步的关键在于：共识不依赖任何外部正确性判断，而是模型内部的一致程度。

3. 选择最短伪解：在满足共识的 rollout 中，选出最短的那个（或最短的共识解）作为教师条件。作者选择最短伪解可能是为了降低教师句子中的冗余和噪声，让教师分布更聚焦于核心推理步骤（合理推断）。

4. 定位最长错误完成：与伪解不一致的 rollout 中被视作“错误完成”，其中最长的那条被选为学生端的目标前缀。这里“最长”可能意味着模型在该路径上走了最远却最终失败，是比较典型的自信错误。

5. 前缀条件蒸馏：在模型最长错误完成的前缀（即错误轨迹的开头若干 token）上，以最短伪解为条件构造教师分布，并将该分布蒸馏回模型自身。也就是说，模型在“自己已经出错但尚未完全跑偏”的早期状态上，被引导向正确的共识方向。这样既保留了 on-policy 的密集 token 监督优势，又只对错误轨迹进行纠正，避免了无差别强化。

6. 训练目标：模型参数通过最小化学生分布与教师分布之间的 KL 散度（或类似蒸馏损失）进行更新。由于教师和学生是同一个模型的不同上下文实例，因此没有额外的教师网络。

值得注意的是，整个流程不涉及 ground-truth、人工标注、环境奖励或更大的辅助模型；唯一的外部输入是问题文本本身。这种设计使 U-OPSD 成为一种“真正”的自蒸馏，其核心假设是：模型多次采样中的一致共识，即使不是绝对正确，也优于单次错误采样的状态，且对错误前缀进行定向蒸馏能够抵消自训练中的确认偏误。

Q4: 论文做了哪些实验？

摘要中报告的实验设计如下：

1. 基准数据集：AIME24、AIME25、HMMT25、MATH500、AMC23，均为数学推理基准，覆盖竞赛级和标准数学题。

2. 基础模型：Qwen3 系列，规模为 4B 和 8B，覆盖两个不同参数规模。

3. 推理模式：分别评估非思考模式（non-thinking mode）和思考模式（thinking mode），后者允许模型生成较长推理链。

4. 基线方法：包括基础模型（无训练）、有监督 OPSD（使用 ground-truth 的 on-policy 自蒸馏）和 GRPO（基于群体相对策略优化的强化学习方法，使用规则奖励）。

5. 评估指标：各基准上的准确率（accuracy），并报告相对于基础模型的提升、相对于 OPSD 和 GRPO 的平均优势。

具体训练设置（如采样数量、阈值选取、训练步数、学习率、蒸馏损失形式）在摘要中未提供。从方法描述可推测，作者对每个问题采样多条 rollout，并根据一致性阈值筛选参与训练的样本；训练过程可能使用标准的自蒸馏损失。由于全文细节有限，实验设置的完整描述需要查阅原论文。

结果概览：在非思考模式下，U-OPSD 相较基础模型在 4B 和 8B 上分别提升 8.5% 和 10.7%；在五个基准的平均值上，比有监督 OPSD 高出 3.2%（4B）和 2.3%（8B）。在思考模式下，U-OPSD 在 4B 上比 OPSD 高 0.9%，在 8B 上与 OPSD 持平；相较 GRPO，4B 和 8B 分别高 0.7% 和 1.1%。这些结果表明，无监督方法的性能与甚至优于有监督方法，且在不同模式和规模下趋势相对一致。

Q5: 发现了什么实验现象？

根据摘要提供的数值，可以观察到以下现象：

1. 非思考模式下优势显著：U-OPSD 在非思考模式下对基础模型的提升幅度（8.5% 和 10.7%）明显大于思考模式（未给出具体基础提升数字，但与 OPSD 对比优势较小）。这说明无监督自蒸馏对“短回答、低推理深度”模式特别有效。可能的原因（推测）：非思考模式生成较短、更直接，多数投票的共识更可靠；同时，长思考模式下模型本身已能进行较深推理，错误轨迹更长更多样，硬投票共识的边际信号减弱。

2. 无监督方法可以超越有监督 OPSD：非思考模式下 U-OPSD 平均高出 OPSD 3.2%（4B）和 2.3%（8B）。这是一个反直觉结果——去掉 ground-truth 反而更好。可能的原因（合理推断）：OPSD 严格依赖 ground-truth 解作为教师条件，而错误完成的前缀可能包含有效推理步骤；U-OPSD 通过多数投票得到的共识解可能更接近“模型可接受”的分布，从而减少分布外强行对齐，降低训练-推理不匹配。此外，U-OPSD 只针对最长错误完成进行纠正，相当于一种更稀疏但更精准的干预。

3. 思考模式下与有监督持平或微弱超越：4B 上 U-OPSD 比 OPSD 高 0.9%，8B 持平。可能说明在思考模式下，模型自己的共识已经足够好，额外标签不再提供明显增益；或者“最长错误完成”选择策略在长轨迹上变得更困难。

4. 对不同规模都有正收益：4B 和 8B 均观察到一致提升，说明方法具有一定可扩展性，不是规模特异的。

5. 与 RL 方法对比：思考模式中 U-OPSD 稳定超过 GRPO（+0.7%/+1.1%），表明无监督自蒸馏可以作为一种轻量级替代方案，免去奖励建模或规则奖励设计。

6. 未报告的负结果与指标张力：摘要未报告任何失败案例或负结果。需要指出（证据不足）：多数投票共识本身可能包含系统性错误（模型集体犯错），在某些情况下可能强化错误；此外，训练信号只来自一致样本，可能降低数据多样性。这些都需要原文的消融实验来确认。

Q6: 有什么可以进一步探索的点？

基于本文已有方法和结果，可以探索以下方向：

1. 更细粒度的一致性度量：当前使用硬投票和阈值，未来可探索概率化的一致性分数、基于置信度的加权投票，以及自适应阈值，以适应不同难度和不同模型。

2. 扩展到更多任务类型：除数学推理外，可验证该方法在代码生成、多步工具使用、agent 规划、科学问题推理等更开放的任务上是否同样有效。对于 AI-for-Science，自然语言科学推理、实验设计建议等领域可能受益于无标注自蒸馏。

3. 与其他后训练方法结合：U-OPSD 可与 SFT、RLHF、GRPO 结合，例如先进行无监督自蒸馏预热，再用少量标注微调；或将自蒸馏信号纳入 RL 的奖励塑造，减少对奖励模型的依赖。

4. 改进教师构造策略：当前选择最短伪解，可研究不同选择准则（如最长、最一致、中位数长度）对蒸馏效果的影响；也可探索为不同前缀位置分配不同权重的蒸馏目标。

5. 分析自我共识的偏见：研究模型多次采样中系统性共享错误（集体幻觉）的情况，引入不确定性估计或外部启发式来识别这类“自信但集体错误”的样本。

6. 扩大模型规模和训练设置：在更大模型（如 70B 级）、不同模型家族、多语言场景下验证方法，探索 scaling 曲线；也可与长思维链（long CoT）训练结合。

7. 理论分析：从分布匹配角度解释为什么去除 ground-truth 能提升非思考模式性能；分析蒸馏损失上界，以及共识投票引入的方差与偏差如何影响收敛性。

8. 在线与迭代训练：研究在迭代训练中，使用当前模型生成的共识作为动态目标是否会引发模式坍缩，如何通过正则化或重采样策略保持多样性。

9. 部署效率：多轮 rollout 会带来额外推理成本，可研究如何减少采样数量、蒸馏所需 token 长度，或利用投机解码等技术加速。

Q7: 总结一下论文的主要内容

本论文《On-Policy Self-Distillation without Any Supervision》提出了一种完全无监督的 on-policy 自蒸馏范式，目标是用模型自身的内部一致性来替代 all 外部监督信号，重新定义“自蒸馏”的边界。

论证主线：作者首先指出现有 on-policy 自蒸馏（OPD/OPSD）虽然强调“self”，但实际仍依赖 ground-truth、环境反馈或更大教师模型，这些外部信号限制了方法的普适性与可持续性。他们进一步分析 on-policy 蒸馏的优势——在学生状态上提供密集 token 级监督并缓解 off-policy 的分布失配——但这种优势建立在可以获得正确参考解的前提之上。本文的核心论证是：多数投票共识（majority-vote consensus）可以充当这一参考，因为模型自身多次采样中一致出现的答案通常比单次采样更可靠，且只对模型“自信但错误”的轨迹进行定向蒸馏，可以规避自训练中的确认偏误。

技术主线：作者提出 U-OPSD 算法，包含四个关键环节：1）采样多个 rollout；2）通过 self-consistency 阈值下的多数投票构造伪解，并取最短伪解作为教师条件；3）识别与伪解不一致的最长错误完成序列；4）将该错误序列的前缀作为学生状态，以最短伪解为条件构造教师分布并蒸馏回模型。这一设计使得训练信号完全来自模型自身，无需任何外部标签。整个流程在数学上可以理解为：在模型对自己“很有把握但实际跑偏”的状态附近，用自身共识分布的‘重心’拉回生成过程。

实验主线：作者在五个数学推理基准（AIME24、AIME25、HMMT25、MATH500、AMC23）上，使用 Qwen3 4B 和 8B 模型，分别在非思考模式与思考模式下评估。结果分为三个层次：1）相对于基础模型，非思考模式下 4B 提升 8.5%、8B 提升 10.7%，说明无监督自蒸馏能带来显著增益；2）相对于使用 ground-truth 的有监督 OPSD，非思考模式下平均高出 3.2%（4B）与 2.3%（8B），思考模式下 4B 高 0.9%、8B 持平，说明移除标签不仅不损失性能，在某些条件下反而更好；3）相对于基于规则奖励的 GRPO，思考模式下分别高 0.7% 和 1.1%，说明该方法作为强化学习的替代或补充具有竞争力。这些结果跨模型规模、跨推理模式一致为正，表明方法的稳健性。

讨论与局限：作者从自我训练、自蒸馏、一致性等角度定位了方法，并强调与 majority voting（Wang 等，2023）和 test-time training on self-consistency（Zuo 等，2025）的关联。局限方面，由于摘要和检索片段未提供详细实验设置和消融，尚不清楚采样数量、阈值选择、蒸馏损失对性能的影响；方法对数学推理以外的任务适用性也有待验证。此外，多数投票共识可能存在“集体错误”风险，尤其在难题上，模型可能一致地给出错误答案，此时 U-OPSD 会强化错误。本文仅报告平均提升，未展示方差和失败案例。

总体而言，这篇论文的价值在于概念层面的突破：它首次证明 on-policy 自蒸馏可以完全脱离外部监督，仅通过内部一致性产生训练信号，并且能匹配甚至超越有监督强基线。这为低资源环境下的大模型后训练提供了新思路，也为后续研究打开了“无监督自蒸馏”这一子领域。由于是预印本且信息有限，建议读者对照原文核实方法细节与实验设置。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成（generation）方向直接相关：U-OPSD 是一种仅靠自生成信号改进生成模型的后训练方法，可迁移到多种生成任务。

## 基本信息

- 作者：Yijiang Li, Bingyang Wang, Yijun Liang, Yunjie Tian, Di Fu, Nuno Vasconcelos
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.06296v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据，主要命中 Abstract、Introduction、Related Work 和 Conclusion 片段；由于全文细节有限，部分方法细节和实验设置基于摘要和常识推断，已尽量标注。
