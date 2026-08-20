---
user_id: "cheng tan"
paper_id: 8646
arxiv_id: "2608.18607"
title: "VA-Judger: Reward Modeling from Human Preference Feedback for Joint Video-Audio Generation"
publish_date: "2026-08-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.18607.pdf"
pdf_url: "https://arxiv.org/pdf/2608.18607"
abs_url: "https://arxiv.org/abs/2608.18607"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-20T11:28:52"
---
# VA-Judger: Reward Modeling from Human Preference Feedback for Joint Video-Audio Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reward model · human preference learning · joint video-audio generation · chain-of-thought

## 一句话总结

VA-Judger 通过三阶段训练策略——先学清晰质量差距的成对比较、再经拒绝采样蒸馏近质量差异的偏好解释、最后进行维度级强化学习——构建联合视频-音频生成的人类对齐奖励模型，并配套提出 VAPref-10K 数据集与 VA-Judger-Bench 基准；实验表明该奖励模型在域内外均优于组合式指标基线，且用其进行后训练能显著提升音视频生成质量。

## 摘要

> Using reinforcement learning to post-train joint video-audio generation models requires a reward signal. Existing methods construct this reward by combining metrics for individual quality dimensions, including audio quality, visual fidelity, and synchronization. However, these metrics evaluate perceptual dimensions separately and fail to capture the overall semantic and temporal coherence among the text prompt, video, and audio that shapes human preferences. More critically, they are poorly aligned with actual human judgments. Optimizing models against these metrics encourages reward hacking, generating video-audio content that achieves high scores on these metrics yet appears incoherent or unfaithful to human viewers. To address this problem, we first construct a large-scale human-preference dataset VAPref-10K for joint video-audio generation, comprising 9K prompts and 10.3K fine-grained paired comparisons from open-source generation models. We also introduce the VA-Judger-Bench benchmark with both in-domain and out-of-domain model comparisons to evaluate whether reward models truly align with human preferences. We further propose VA-Judger, a chain-of-thought omni-reward model for joint video-audio generation. In particular, VA-Judger first learns from pairs with clear quality gaps to establish structured output and coarse preference discrimination, then distills reliable preference explanations for harder near-quality comparisons via rejection sampling verified against human annotations, and finally performs dimension-wise reinforcement learning that decomposes human feedback into individual quality dimensions for denser reward signals than a single binary preference label. Experiments show that VA-Judger outperforms metric baselines in predicting human preferences on both in-domain and out-of-domain evaluations. Using its human-aligned rewards for post-training audio-video generation model also yields significant improvements in generation quality.

Q1: 这篇论文试图解决什么问题？

本文试图解决的核心问题是：联合视频-音频生成模型在强化学习后训练阶段缺乏可靠的奖励信号。具体拆解如下：
1. 现有奖励构造方式的缺陷：主流做法是把音频质量、视觉保真度、视音同步性等独立质量指标加权组合为一个奖励分数。这些指标只对单个感知维度做评估，无法捕捉文本提示、视频内容、音频内容三者之间的整体语义连贯性与时间一致性，而后者恰恰是人类偏好的主要来源。
2. 指标与人类判断的错位：这些自动化指标与真实人类主观判断的一致性很差。以它们作为优化目标容易出现 reward hacking：生成结果在指标上得分很高，但人类观看时仍觉得内容不连贯、不忠实于提示。
3. 跨模态偏好不可简单分解：人类对视频-音频内容的偏好不是各单模态判断的简单加和。文本到图像或文本到视频的奖励模型没有观察音频流，因此不能直接迁移到联合音视频任务；而联合判断需要考虑视音之间的语义一致和时序同步等交互维度。
4. 监督信号稀疏性：单个二元偏好标签只告诉模型“哪个更好”，对于质量非常接近的生成对，信息量太低，不足以指导细粒度优化。
5. 评估缺失：缺少专门面向联合音视频生成奖励模型的评测基准，尤其是检验域外泛化能力的协议，使得不同奖励模型与人类偏好的一致性难以公平比较。

Q2: 有哪些相关研究？

1. 奖励建模与人类偏好数据：在图像生成领域，Pick-a-Pic 收集图像偏好数据，ImageReward 学习可用的图像奖励模型；这类方法通常面向单模态。
2. 多模态奖励模型：后续工作把奖励模型扩展到多模态输入，但常见设计仍是分别评估各模态质量再组合，缺乏对模态间一致性的联合建模，不能直接用于联合音视频生成。
3. 联合视频-音频生成模型：最新生成模型已从静音视频转向同时生成视觉运动与同步音频；闭源产品（如 OpenAI、Google DeepMind 的产品）展示了产品级能力，开源模型也在跟进，但这些模型的后训练普遍缺少与人类偏好对齐的奖励信号。
4. 生成模型的强化学习：RLHF 已用于语言模型，近期研究将在线强化学习适配到扩散模型。FlowGRPO 把 GRPO 扩展到文本到图像流模型，DanceGRPO 为文本到图像/视频设计统一 GRPO 框架，Omnint 做模态级 omni 扩散强化学习用于联合音视频生成。这些方法通常仍依赖可分解的奖励函数，没有解决跨模态偏好对齐问题。
5. 本工作与上述研究的区别：强调人类偏好不可分解为独立单模态判断，提出以人类偏好的结构化维度分解为监督信号，并构建专门的数据集与评测基准。

Q3: 论文如何解决这个问题？

VA-Judger 的设计目标是给 text-to-video-audio 生成模型提供与人类判断一致的奖励信号。方法总体如下：
1. 结构化输出：VA-Judger 在给出最终成对偏好决策之前，先产生结构化的逐维质量判断（即链式思维式的中间输出），维度覆盖诸如视觉质量、音频质量、同步性、语义一致性、跨模态连贯性等；这些中间判断既用于解释偏好，也为后续维度级奖励提供素材。
2. 阶段一：清晰差距配对学习。先用质量差距明显的成对比较训练，让模型建立稳定的结构化输出格式并掌握粗略的好坏区分。这一阶段相当于课程学习的入门任务，信噪比高，易于拟合。
3. 阶段二：偏好解释蒸馏。对于质量接近的难配对，利用拒绝采样生成候选的偏好解释，并用人类标注验证这些解释的可靠性；只有通过验证的解释才被用于蒸馏训练，从而使模型学会用语言表达微妙但关键的同步、语义和连贯性失败。
4. 阶段三：维度级强化学习。将人类反馈分解到不同质量维度，为每个维度单独构造奖励信号，从而获得比单一二元偏好标签更密集的监督。这种维度级奖励可缓解单标签信息量不足的问题，使模型更精确地感知其输出质量。
5. 人类判断的锚定：整个训练流程的监督信号都锚定在真实人类判断上，避免依赖易被钻空子的自动指标；同时用结构化中间输出显式引导模型关注跨模态的同步、语义与连贯性失败。
说明：上述对维度数量、具体奖励函数形式及 RL 算法细节未在检索证据中完整呈现，属于对摘要与片段内容的合理推断，具体实现需回查原文。

Q4: 论文做了哪些实验？

1. 数据构建：VAPref-10K，包含 9K 提示和 10.3K 细粒度成对比较，样本来自多个开源生成模型的输出，并由人类标注偏好。
2. 基准：VA-Judger-Bench 共三个分割——400 对 easy（质量差距明显的粗粒度判别）、250 对 in-domain（域内模型比较）、500 对 out-of-domain（域外模型比较）。这样设计可以分别测试奖励模型的粗粒度区分能力、域内泛化性和域外泛化性。
3. 奖励模型评估：以预测人类偏好的一致性为主要指标，对比 VA-Judger 与多种指标基线（音频质量、视觉保真、同步性等单维度指标及其组合），覆盖 in-domain 和 out-of-domain 两个设置。
4. 后训练实验：将 VA-Judger 的奖励信号用于某个音视频生成模型的强化学习后训练，对比训练前后的生成质量，检验奖励模型能否指导生成改进。
5. 评测细节：具体模型名单、基线名称、评估指标数值和显著性检验等关键细节在检索证据中未出现，需要查看论文实验章节。

Q5: 发现了什么实验现象？

1. 域内与域外一致性：在 VA-Judger-Bench 的 in-domain 与 out-of-domain 划分中，VA-Judger 均优于指标基线。这提示简单指标即使在其“适合”的域内也不具备足够的人类对齐性，而经过人类偏好监督的模型泛化性更强。
2. 难例与易例的差异：easy 分割（400 对）主要检验粗粒度偏好判别，难分割则集中于近质量差异；合理推断，指标基线在 easy 上尚可，但在近质量难例上会明显失准，而 VA-Judger 通过中间解释蒸馏保留了对细微失败模式的敏感性。
3. 维度级奖励的价值：单一二元偏好标签无法区分质量接近的候选，而维度级强化学习提供了更密集的奖励信号，推测这是 VA-Judger 在难例评估上取得优势的关键因素。
4. 后训练效果：使用 VA-Judger 的奖励进行后训练能带来显著的生成质量改进，说明该方法不仅是一个分类器，还能有效指导生成模型优化，且至少在一定程度上规避了奖励黑客。
5. 反直觉点：直觉上组合多个单模态指标应能覆盖各维度，但实验结果表明这种分解式方法忽略了跨模态交互，反而成为核心瓶颈；这说明联合音视频偏好不是单模态质量的简单加和。
注意：以上第 2、3 点属于合理推断，第 4 点基于摘要表述，具体消融和错误案例未在检索证据中出现。

Q6: 有什么可以进一步探索的点？

1. 数据扩展：将 VAPref-10K 扩展到更多提示风格、更多语言、更多开源/闭源生成模型，覆盖长视频、复杂场景和更强时间动态的样本，提升奖励模型的泛化基础。
2. 在线与迭代 RL：把 VA-Judger 作为动态奖励接入在线强化学习（如 GRPO 风格）循环，在生成模型迭代过程中持续更新奖励模型，探索奖励模型与生成模型共同进化的设定。
3. 更细粒度与可解释的维度：设计更丰富的中间判断维度（如事件级因果一致性、视听同步延迟、情感一致性），并研究维度设计对奖励质量和可解释性的影响。
4. 奖励鲁棒性：系统评估 VA-Judger 是否仍可被对抗性生成策略攻破（残余 reward hacking），并研究如何通过人类反馈校准和正则化增强鲁棒性。
5. 迁移到其他多模态生成：把“结构化维度判断 + 维度级 RL”的框架推广到文本-音乐、文本-3D、图像-音频等联合生成任务。
6. 与 Agent 偏好学习结合：将维度化人类偏好建模用于 agent 交互轨迹的奖励塑形，缓解稀疏奖励和偏好不一致问题。
7. 自动化解释生成：探索用大型语言模型自动生成偏好解释，减少对人工验证的依赖，同时保持解释可靠性。

Q7: 总结一下论文的主要内容

【背景与动机】
联合视频-音频生成模型（以文本提示同时生成视频画面和同步音频）正从研究走向产品，但这类模型在强化学习后训练中缺乏天然的奖励信号。现有方法常把音频质量、视觉保真度、同步性等单维度指标加权组合成一个标量奖励，作为后训练优化目标。作者指出两条根本性缺陷：第一，这些指标各自只覆盖一个感知维度，无法刻画文本、视频、音频三者之间的整体语义一致性与时间连贯性，而后者恰恰左右人类偏好；第二，指标分数与真实人类主观判断的对齐度很低，优化这类目标容易诱发 reward hacking，即生成结果在指标上高分，但人类看起来不连贯或不忠实。同时，人类对音视频内容的偏好不是各单模态偏好的简单合并，文本到图像或文本到视频现有的奖励模型观察不到音频流，不能直接迁移。另一个实际障碍是监督信号太稀疏——单个“A比B好”的二元标签，不足以告诉模型质量接近的生成对差在哪里。

【方法思路】
为了解决这些问题，作者构建了 VAPref-10K 人类偏好数据集：包含 9K 提示与 10.3K 细粒度成对比较，候选样本来自多个开源生成模型，并由人类标注偏好。同时提出 VA-Judger-Bench 基准，划分 easy（400 对）、in-domain（250 对）、out-of-domain（500 对）三个子集，用于检验奖励模型对粗粒度判别、域内和域外泛化的能力。
方法核心是 VA-Judger，一个链式思维的全模态奖励模型。它在输出最终成对决策前，先产生结构化的逐维判断（如视觉质量、音频质量、同步性、语义一致性、跨模态连贯性等），再综合得到偏好。训练分三阶段：第一阶段用质量差距明显的配对，学习结构化输出和粗糙的好/坏区分；第二阶段针对质量相近的难配对，利用拒绝采样生成偏好解释，并经人类标注验证后把这些可靠解释蒸馏给模型，从而学会识别细微的同步、语义与连贯性失败；第三阶段做维度级强化学习，将人类反馈按维度分解，构造更密集的奖励信号，替代单一二元标签。整个流程始终把监督锚定在人类判断上，避免奖励黑客的风险。

【实验与发现】
实验部分围绕奖励模型评测和后训练效果展开。奖励模型评测基于 VA-Judger-Bench 的三个分割，以预测人类偏好的一致性为主要指标；结果显示 VA-Judger 在 in-domain 与 out-of-domain 两种设置下都优于指标基线，证明以人类偏好直接建模比组合自动指标更可靠。后训练实验中，将 VA-Judger 的奖励用于音视频生成模型的强化学习，生成质量显著提升，说明该奖励模型具备作为优化目标的实际价值。作者由此得出结论：为联合音视频生成建模奖励，必须考虑跨模态整体一致性，而不能依赖分解式指标；维度级奖励和结构化中间推理是获得人类对齐奖励的有效途径。

【整体评价】
论文同时贡献了数据、基准、方法和验证闭环，属于系统性工作。由于检索到的证据有限，具体模型架构、训练超参、消融细节和完整数值未见披露，读者需要查阅原文实验章节以获取更精确的量化比较。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 generation 方向直接相关：论文针对生成模型的奖励建模与 RL 后训练，是生成质量提升的关键环节。

## 基本信息

- 作者：Yinming Huang, Shuyuan Tu, Xi Yan, Zihan Yang, Jianhua Han, Xu Hang, Yu-Gang Jiang, Zuxuan Wu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.18607`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于论文摘要、启发式草稿与 retrieved_evidence 中的语义检索片段（VAPref-10K、VA-Judger-Bench 三个分割、三阶段训练描述等），部分细节如具体维度和数值来自合理推断，证据缺口已在对应字段标注。
