---
user_id: "cheng tan"
paper_id: 14980
arxiv_id: "2610.11152v1"
title: "Do LLMs Learn from Rewards in Context? : Rethinking the role of reward in In-Context Reinforcement Learning"
publish_date: "2026-10-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.11152v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.11152v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T03:22:06"
---
# Do LLMs Learn from Rewards in Context? : Rethinking the role of reward in In-Context Reinforcement Learning

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：in-context reinforcement learning · in-context learning mechanism · reward sensitivity · llm agents

## 一句话总结

本文通过四个基准、六个模型的受控实验证明，所谓“直接上下文强化学习”（Direct ICRL）中奖励并未起到真实学习信号的作用——翻转、随机化或删除奖励几乎不影响改进曲线，轨迹的分布与形式而非语义内容驱动提升，因此 Direct ICRL 更应被理解为 ICL 的特例而非推理期 RL。

## 摘要

> LLM agents increasingly improve at inference time by accumulating experience in context rather than by updating parameters. This process is often described as in-context reinforcement learning (ICRL). Whether in-context learning (ICL) can actually play the role of RL, however, has not been tested. We study this question in its simplest form, direct ICRL, where the model conditions directly on raw trajectory-reward pairs, and ask whether the reward acts as a learning signal. Through controlled experiments on four benchmarks across six models, we find that the reward is read, but its effect is small: flipping, randomizing, or removing the reward leaves the improvement curve almost unchanged, and this holds even under meta-prompts that explicitly instruct the model to explore, exploit, or reason over rewards. Trajectories drive improvement, but not through their semantic content: shuffled or corrupted trajectories work as well as real ones. These patterns closely mirror those known in ICL, suggesting that direct ICRL is better understood as a special case of ICL than as inference-time RL. This reframing has implications for agent memory design: ICL factors such as input distribution and demonstrations may matter more than RL elements such as reward shaping and exploration.

Q1: 这篇论文试图解决什么问题？

论文挑战的是当前智能体研究中的一个被默认接受的前提：LLM 智能体通过上下文中的经验-奖励积累获得性能提升，这一过程被普遍称为 in-context reinforcement learning (ICRL)，并被认为确实是某种形式的强化学习。核心问题在于，ICL 与基于梯度的学习机制截然不同：ICL 对输入分布和表面形式敏感，却对标签内容惊人地不敏感。因此作者提出根本性疑问：当 LLM 依据上下文中的经验-奖励对改进时，它到底是在做强化学习，还是仅仅在做 ICL？具体地，论文通过最简可控的 Direct ICRL 设定（只给模型原始轨迹-奖励对，排除记忆存储、选择、摘要等附加环节）回答三个研究问题：(RQ1) 奖励文本是否真正作为学习信号起作用；(RQ2) 奖励的语义内容是否对提升有贡献，还是轨迹本身（及其形式/分布）才是驱动因素；(RQ3) 负奖励/失败经验是否提供有效的回避信号，即是否存在真正的负向信用分配。这一问题的实践意义在于：如果 Direct ICRL 本质是 ICL，那么智能体记忆设计（memory design）的重心应从 RL 要素（奖励塑形、探索）转移到 ICL 要素（输入分布、示范的组织形式）。

Q2: 有哪些相关研究？

论文的相邻研究脉络包括以下几支。第一支是 ICL 机制研究：Min et al. [11] 的核心发现是 ICL 在标签区域随机化时依然有效，说明形式与分布比内容更重要，这是本文最重要的理论锚点；另有一系列工作指出 ICL 与梯度学习运行机制不同 [28]，且否定/负向语义在 ICL 中难以处理 [21]，后者被本文用来解释失败轨迹回避信号微弱的现象。第二支是上下文强化学习 (ICRL) 的实证报告：近期工作声称即使在只给原始轨迹-奖励对的直接设定下 LLM 也能作为上下文 RL 学习器 [19, 3]，本文正是对这些主张的系统性质疑。第三支是经验增强智能体系统：大量 agent 系统通过在上下文中存储、选择、摘要化经验来提升推理性能 [18, 29, 22, 24, 27]，本文讨论部分指出这类系统借用了 RL 术语框架（filtering、self-bootstrapping、reward-based design），但如果 Direct ICRL 本质是 ICL，这些 RL 视角的设计原则需要被重新审视。论文的实验设置还与 RL 的经典 settings 对齐——包括 memory accumulation（记忆累积）与 sequential decision-making/action-level credit assignment（序列决策与动作级信用分配）两种 regime——以便与标准 RL 场景做更接近的类比。整体上，本文试图把“ICRL 是否真在做 RL”这个此前未被直接检验的问题，通过干预实验（intervention experiments，翻转/随机/删除奖励）纳入 ICL 机制研究与 agent 系统研究之间。

Q3: 论文如何解决这个问题？

论文采用“最简设定 + 系统性干预 + 行为与概率双重测量”的实验策略。首先定义 Direct ICRL：模型只拿到原始轨迹-奖励对，不含记忆存储、筛选、摘要等模块，从而把问题压缩到最小可控形式。实验在两个 regime 下进行：一是 Memory Accumulation Setting（记忆累积设定）：每个任务从空记忆 C0=∅ 开始，每个 episode t 模型生成轨迹 τt，得到奖励，存入记忆，下一 episode 在完整记忆上继续生成；二是更接近标准 RL 的序列决策/动作级信用分配设定。干预设计围绕奖励与轨迹两个维度展开：奖励维度的干预包括 ICRL-True（真实奖励）、ICRL-Flipped（完全翻转奖励极性，高奖励标为低奖励、低奖励标为高奖励）、ICRL-Random（奖励随机赋值）、Trajectory Only（完全删除奖励字段）；轨迹维度的干预包括打乱轨迹、破坏/污染轨迹内容，以检验语义内容是否必要。此外还设计了 Meta-Prompt 干预：在提示中显式指示模型去探索、利用或对奖励进行推理，甚至使用 Anti Prompt（明确要求‘换一个动作’）来测试显式指令能否激活负向信用分配。测量层面采用双通道：概率层面在记忆条件下计算目标输出（如目标轨迹 τ+ 或失败轨迹 τ−）的逐 token log-probability（附录 B.2.1 给出 meta-prompt m 的细节），行为层面每个条件采样 500 条生成，计算 Target Replay Rate（模型复现记忆中的目标输出的比例）。通过比较这些干预条件与 Random Sampling (RS) 基线的改进曲线，论文得以分离“奖励被读取”与“奖励作为学习信号”两个层次，并最终把 Direct ICRL 的行为模式与已知 ICL 模式做对照。

Q4: 论文做了哪些实验？

论文在四个 benchmark、六个模型上开展了多层级的受控实验。基准包括：HumanEval+ 与 MBPP+（代码生成，作为任务型基准）、以及另外两个覆盖其他任务形态的基准（完整列表需以原文 Table/附录为准，摘要与检索片段未给出四个基准的逐一名称）。六个模型的具体型号在检索证据中未完全列出（部分模型可合理推断为不同规模/系列的通用 LLM）。实验结构如下：(1) 主效应验证：对比 ICRL-True 与 Random Sampling (RS)，确认 Direct ICRL 确实带来 episode 奖励的清晰提升，复现标准 direct ICRL gain，作为后续需要解释的效应；(2) 奖励干预：ICRL-Flipped（完全反转奖励极性）、ICRL-Random（奖励随机化）、Trajectory Only（删除奖励字段），在三个基准上报告 episode reward 的均值与标准差曲线；(3) 元提示干预：显式指示模型 explore/exploit/reason over rewards，以及 Anti Prompt（要求‘take a different action’），检验显式指令能否改变奖励利用行为；(4) 轨迹内容干预：打乱或破坏轨迹，检验轨迹的语义内容是否是提升的必要条件；(5) 概率与行为双层测量：在记忆条件下计算目标轨迹的逐 token log-probability（附录 B.2.1 定义 meta-prompt m），并在每个条件采样 500 条生成，计算 Target Replay Rate（复现目标输出的比例）；(6) 失败轨迹专项实验：将记忆中的轨迹全部标注为失败（All Failure）或在 Anti Prompt 下，测量模型对最优轨迹 τ+ 与最差轨迹 τ− 的复现率，以检验负向信用分配是否存在。关键对照条件还包括 Problem-only（只给任务描述 q、无记忆）与 Trajectory-only（给轨迹但无奖励字段）的对比，用于分离任务描述、轨迹与奖励各自的贡献。

Q5: 发现了什么实验现象？

实验揭示了一组高度反直觉、且对 RL 视角构成直接否定证据的现象。第一，直接 ICRL 确实能改进：ICRL-True 在全部三个报告基准（HumanEval+、MBPP+ 等）上以清晰幅度超过 Random Sampling，说明提升效应真实存在、可复现。第二，奖励极性完全翻转（ICRL-Flipped）不破坏改进：按 RL 观点性能应崩到 RS 以下，但 Flipped 仍远高于 RS，曲线更接近 True 而非 RS；虽然 Flipped 在 HumanEval 和 MBPP 上略低于 True，但差距很小——奖励被读取了（因此有微小效应），但不足以改变 Direct ICRL 的方向。第三，随机奖励（ICRL-Random）在种子方差内与真实奖励完全匹配；完全删除奖励字段（Trajectory Only）也紧密跟随 ICRL-True 并产生同样的 episode-over-episode 改进——改进既不要求奖励数值正确，也不要求奖励字段存在。第四，元提示干预无力：即使显式指示模型探索、利用或推理奖励，行为模式依然不变。第五，失败轨迹只提供微弱的回避信号：负向信用分配效应可忽略；在最强的 Anti Prompt（明确要求‘take a different action’）下，复现率的降低幅度依然很小；对最低奖励轨迹 τ− 的复现率在 All Failure 下为 2.3%、Anti Prompt 下为 2.4%，与 ICRL 条件下的 2.5% 相比几乎没有变化，说明负向信号的处理几乎完全无效。第六，轨迹的语义内容不必要：打乱或破坏轨迹后效果与真实轨迹相当，提升来自轨迹的分布与形式而非语义。第七，存在一个与 ICL 中否定处理困难相呼应的现象：奖励标签不能轻易替代显式记忆设计，且对任务密集（n-intensive）场景尤其如此。综合来看，所有干预都无法改变改进曲线的形状，说明 Direct ICRL 的收益既不来自奖励的价值内容、也不来自轨迹的语义内容，而更可能来自 ICL 所强调的输入分布与表面形式。

Q6: 有什么可以进一步探索的点？

论文的结论打开了多条后续研究路径。第一，机制层面的验证：用更丰富的测量手段（如注意力归因、logit lens、内部表征分析）定位奖励字段与轨迹字段在模型内部的处理路径，确认‘奖励被读取但不被用作信号’的神经机制基础。第二，边界条件的刻画：找出在什么条件下奖励才能真正进入学习信号——例如任务复杂度、奖励稀疏度、上下文长度、模型规模、训练数据中 RL 数据的占比（RLHF/RLVR 后的模型是否不同）等可能调制奖励敏感性的因素；本文观察到任务密集场景下奖励标签无力，值得系统探索‘奖励何时从 ICL 形式变为 RL 信号’的阈值。第三，从 Direct ICRL 走向真实 ICRL 系统：本文刻意排除了记忆存储、筛选、摘要等模块，后续可以检验当这些 RL 风格机制被引入后，系统提升在多大程度上仍可归因于 ICL 因素，从而为 agent 记忆架构设计提供定量指导。第四，负向信用分配的补救：既然失败轨迹的回避信号几乎无效且与 ICL 的否定处理困难一致，可以探索显式否定改写、对比示范（contrastive demonstrations）、结构化失败摘要等替代方案，把负向信息转化为 ICL 能利用的正向形式。第五，训练侧干预：既然直接 ICRL 在推理期不呈现 RL 行为，可以在训练阶段引入针对性的上下文奖励利用数据或元学习目标，使模型在上下文中真正学会用奖励信号。第六，理论建模：把 Direct ICRL 的改进曲线建模为 ICL 的分布匹配/隐式贝叶斯推断过程，给出可预测的定量理论，并解释为什么随机奖励也能维持改进。第七，实践设计原则的重构：基于‘ICL 因素优先’的结论，系统研究输入分布、示范排序、轨迹表面形式、去重与多样性对 agent 记忆性能的影响，替代当前以 reward shaping 为中心的 memory 设计范式。

Q7: 总结一下论文的主要内容

论文的核心主张是：被广泛接受的‘上下文强化学习（ICRL）’这一说法在 Direct ICRL 这一最简设定下并不成立，其行为模式更接近 ICL 而非 RL。论证主线从 ICL 的已知机制出发：ICL 对输入分布和表面形式敏感、对标签内容不敏感、对否定语义难以处理，这些已知模式为后续实验预测提供了理论坐标。技术主线是构建 Direct ICRL 设定（只给原始轨迹-奖励对），并在记忆累积与序列决策两个 regime 下进行系统干预：翻转奖励、随机化奖励、删除奖励字段、打乱/破坏轨迹、显式元提示（explore/exploit/reason/anti）。实验主线覆盖四个基准、六个模型，测量通道包括 episode reward 曲线、目标输出的逐 token log-probability 与 Target Replay Rate（每条件 500 条采样）。关键发现可归纳为五条：(1) Direct ICRL 确实带来超过 Random Sampling 的改进，效应真实；(2) 奖励被读取但效应很小——翻转、随机化、删除奖励均不改变改进曲线形状，最强干预（完全反转）也只造成微小差距；(3) 显式元提示（包括 Anti Prompt）无法激活奖励利用或负向信用分配；(4) 轨迹驱动改进但非通过语义内容——打乱或破坏轨迹效果不变；(5) 失败轨迹的回避信号微弱到可忽略（τ− 复现率在各条件下仅相差 0.1-0.2 个百分点）。论文把这些模式与 Min et al. 的 ICL 标签随机化发现直接对齐，得出结论：Direct ICRL 不是无参数 RL 系统，而是带有奖励字段的 ICL；因此以 RL 视角研究 ICRL 的既有框架（filtering、self-bootstrapping、reward-based design）需要被重新审视，agent 记忆设计应优先关注 ICL 已识别的核心因素——输入分布与表面形式。讨论部分还指出该结论对任务密集型场景的实践含义：奖励标签无法轻易替代显式记忆设计与训练，这与 ICL 中‘否定难以处理’的既有发现一致，并对 ICRL 的适用范围提出质疑。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户画像中‘agent’方向直接重合：论文研究的是 LLM 智能体推理期经验积累的机制本质，直接关系到 agent 记忆系统的设计原则。

## 基本信息

- 作者：Minchan Kwon, Seunghee Koh, Sunghyun Baek, Minsung Bae, Junmo Kim
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-08
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.11152v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要、Introduction、Results、Discussion 的结构化内容及 72 个 PDF 语义检索片段（field_evidence_map 提供的 background/method/results/limitations/relevance 证据），并基于 heuristic_draft 中可用的实验数字做了核对与补全；对未在证据中出现的基准/模型清单、附录细节与统计显著性做了明确缺口标注。
