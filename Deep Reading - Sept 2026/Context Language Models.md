---
user_id: "cheng tan"
paper_id: 14538
arxiv_id: "2609.37725v1"
title: "Context Language Models"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.37725v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37725v1.pdf"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-09T13:04:19"
---
# Context Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：context language model · context management · long-horizon agent · multi-agent systems

## 一句话总结

本文提出上下文语言模型（Context Language Models，CLM），将上下文视为可由模型任意编辑的文件，使语言模型原生地管理自身上下文，在长期智能体任务上超越现有上下文管理策略，并支持技能进化、在线强化学习与缓存复用等高效学习与部署方式。

## 摘要

> We introduce Context Language Models (CLMs), language models that natively manage their own context. We implement this by treating the context as a file and allowing the model to make unrestricted updates to this file. This allows the model to learn what is most important to maintain in context, and naturally extends to multi-agent systems where multiple agent contexts coexist as files. Building CLMs zero-shot with existing models outperforms SOTA context management strategies across a variety of tasks: 11.4% higher accuracy with 21.5% fewer FLOPs on BrowseComp-Plus, 5% higher scores with 59% fewer FLOPs on 12-hour EdgeBench, and 65% greater improvement with the same compute on a 24-hour multi-repository agent-swarm task. Moreover, by shifting context management from external harness control to intrinsic model behavior, CLMs naturally enable both in-context and parametric learning of context-management strategies. We show that CLMs can be steered with natural-language instructions evolved through a standard skill-optimization loop, improving held-out accuracy by up to 35.9 points on a context-management task while reducing compute. We also introduce an online reinforcement learning method for CLMs, improving Qwen3.5-9B performance on BrowseComp-Plus by 47.6% while using 12% fewer FLOPs. Finally, we co-design Suffix Cache Reuse for CLM serving, further reducing server-side compute by 35% relative to standard SGLang at matched performance.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：上下文管理长期以来不是语言模型的原生能力，而是由外部控制系统（harness）承担的。具体而言：

1. 上下文是 LM 在长时间任务中处理和保留信息的基石，但标准 LM 只能做简单的 token 追加（c_{t+1} = c_t ⊕ f_LM(c_t)），无法主动决定哪些信息需要保留、哪些可以丢弃、如何压缩或重组。

2. 现有方法大多依赖外部 harness：有人工精心设计的规则，有离线优化的智能体框架，也有在动作空间中加入受限工具（如压缩、卸载、检索）并配合固定策略的方案。这些方法都受到人类先验知识限制，无法让模型自适应地创造更优的上下文管理策略。论文引用 Sutton 的 Bitter Lesson，强调应让模型自己去搜索和学习超越人工先验的策略。

3. 随上下文中间编辑（in-the-middle edits）而来的服务系统挑战：传统 serving 系统只对匹配前缀复用缓存状态，任何中间位置的编辑都会导致缓存失效，迫使重新预填充，显著增加计算开销。CLM 的“任意更新”能力加剧了这一挑战，因此需要在系统层面设计新的缓存复用方案。

4. 学习方法层面：如何让上下文管理策略可以通过上下文内学习（in-context learning）和参数化学习（parametric learning，如强化学习）被持续改进，而不是只能依赖零样本提示。

论文综合地回应了以上问题：形式化 CLM 为对上下文的任意函数变换，实现为文件式编辑，使其具备原生上下文管理能力；同时引入指标 prefix-reuse FLOPs 来度量实际计算成本，并提出 Suffix Cache Reuse 来适配中间编辑场景。

Q2: 有哪些相关研究？

论文相关研究主要分布在以下几个方向：

1. 基于 harness 的上下文管理：早期工作大多由外部系统控制上下文，例如 Cassano & Rush (2026)、OpenAI (2026a)、Merrill et al. (2026) 等，采用人工设计或启发式规则来管理上下文窗口。这类方法无法随任务自适应变化。

2. 离线优化的智能体 harness：如 Lee et al. (2026) 通过离线优化来改进上下文管理策略，但优化过程与模型推理分离，灵活性仍然有限。

3. 增加受限工具集的智能体方法：近期工作（Yan et al., 2026; Yu et al., 2026; Li et al., 2026c; Liu et al., 2026; Zhang et al., 2025a 等）将压缩、卸载、检索等固定策略的工具纳入智能体的动作空间。虽然扩大了动作空间，但工具本身和策略仍是预先定义的，模型只能在固定选项中抉择。

4. 具体的上下文记忆/压缩方法：论文实验中的对比基线包括 MEM1（Zhou et al., 2026）、Self-Compact（Li et al., 2026b）、ACM（Li et al., 2026c）以及递归语言模型 RLM（Zhang et al., 2025a）。这些方法代表了当前 SOTA 的受限上下文管理策略。

5. 上下文作为认知技能的学习：相关工作中 Automem（Wu et al., 2026）将记忆建模为可自动学习的认知技能，与本文中通过自然语言指令进化上下文管理技能的方向有联系。

6. 服务系统与缓存复用：现有 serving 系统（如 SGLang）通常只对匹配前缀复用 KV 缓存，无法处理上下文中间编辑后的复用问题。本文提出的 Suffix Cache Reuse 针对 CLM 的编辑模式设计了新的缓存复用策略。

论文的核心差异在于：不给模型预设任何受限工具或固定策略，而是给予模型对上下文的“不受限编辑权”，让策略从模型行为中涌现出来。

Q3: 论文如何解决这个问题？

论文提出 Context Language Model（CLM），核心思路是把上下文当作模型可以任意修改的文件。形式化上，CLM 参数化为 θ，上下文状态演化为 c_{t+1} = f_CLM_θ(c_t)，其中 f_CLM_θ 是由模型控制的任意函数；而标准 LM 只是 c_{t+1} = c_t ⊕ f_LM_θ(c_t) 的追加。这一形式化意味着 CLM 可以插入、删除、重写、重排上下文中的任意片段。

实现上，论文将上下文镜像为一个文件，模型通过文件读写接口进行更新。这种设计遵循“less-is-more”原则：harness 层尽量少硬编码，工具可以在推理时灵活定义、添加或修订，从而让模型拥有最大的策略自由度。

论文还提出了三种让现有 LM 成为 CLM 并进一步优化的途径：

1. 零样本 CLM：直接使用现有语言模型，通过提示将其转化为 CLM，无需任何训练。实验表明这种简单方式已经超越多种需要专门设计的 baseline。

2. 技能进化（Skill Evolution）：利用自然语言指令作为上下文管理策略的载体，通过标准的技能优化循环（skill-optimization loop）对指令进行进化，使模型在 held-out 任务上表现更好。检索证据显示，进化后的技能可以严格支配初始点，扩展性能-成本 Pareto 前沿。

3. 在线强化学习：论文提出一种针对 CLM 的在线 RL 方法，直接优化模型参数，使模型学会更好的上下文管理行为。实证中 Qwen3.5-9B 的 BCP 成绩大幅提升且 FLOPs 下降，说明参数化学习上下文管理是可行且有效的。

系统层面，论文提出 Suffix Cache Reuse（SCR）服务优化方案。传统 serving 只对匹配前缀复用缓存，导致中间编辑后必须重新预填充；SCR 则对所有幸存的 token 复用缓存状态，从而减少重新预填充。论文还引入 prefix-reuse FLOPs 作为计算开销指标，用于在标准服务下评估各方法的真实效率。

Q4: 论文做了哪些实验？

论文实验覆盖了长期智能体任务中的多个维度和多种评估设置：

1. 编码与深度研究任务（单智能体）：
- 终端编码基准：TerminalBench 2.1 (TB2.1) 和 TBLite。
- 深度研究基准：BrowseComp-Plus (BCP)。
- 评估设置：所有方法零样本、开箱即用，使用共享的 Mini-SWE-Agent 骨干，模型为 Qwen3.6-27B，上下文预算 32K；报告准确率和 prefix-reuse FLOPs。
- 对比基线包括 MEM1、Self-Compact、ACM、RLM，以及 Codex-style summarization。

2. 长期多智能体任务：
- 12 小时 EdgeBench 任务（单仓库优化）。
- 24 小时多仓库 agent-swarm 任务（开放发现型）。
- 多智能体配置中包括 subagent 和 agent-swarm 两种工作负载。

3. 开放发现任务：
- 覆盖多个数学/几何问题，如 min-max/mindistance 2D、Erdős minimum overlap、Heilbronn triangle problem 等。

4. 技能进化实验：
- 通过 skill-optimization loop 进化自然语言指令，在 held-out 上下文管理任务上评测准确率和计算量。

5. 在线强化学习实验：
- 使用 Qwen3.5-9B 在 BrowseComp-Plus 上评测 RL 方法的效果与 FLOPs。

6. 服务优化实验：
- 对比标准 SGLang 与加入 Suffix Cache Reuse (SCR) 的 CLM serving，在匹配性能下比较服务器端计算量。

需要说明的是，受限于提供的材料，部分实验的完整配置（如训练数据、超参数、消融设计）在摘要和检索片段中未完全展开，具体细节需查阅论文正文和附录。

Q5: 发现了什么实验现象？

实验观察到的核心结果和现象如下：

1. 零样本 CLM 在深度研究任务上显著超越所有基线：在 BrowseComp-Plus 上，CLM 在 32K 上下文限制下取得 59.4% 准确率，比最强基线（Codex-style summarization）相对提升 11.4%；同时 prefix-reuse FLOPs 分别比 Codex-style summarization 和 MEM1 减少 21.5% 和 28.9%。这说明“给予模型不受限的上下文编辑权”本身就比精心设计的外部摘要/压缩策略更有效。

2. 编码任务上，CLM 在 TB2.1 上匹配最强基线（Codex-style summarization），但计算量只用到其 70% 左右（从检索片段“using only 70%…”推断）。这表明在同等甚至更好性能下，CLM 的计算效率更高。

3. 在 12 小时 EdgeBench 上，CLM 分数比基线高 5%，同时 FLOPs 减少 59%，展示出长期运行中的显著效率优势。

4. 在 24 小时多仓库 agent-swarm 任务中，CLM 在相同计算量下改进幅度提升 65%。多智能体场景下，多个上下文文件并存的设计让每个智能体可以独立管理自身上下文，整体任务收益更大。

5. 技能进化实验表明，通过自然语言指令进化的上下文管理策略可以严格支配初始策略，扩展性能-成本 Pareto 前沿；在 held-out 上下文管理任务上准确率最多提升 35.9 个点，同时降低计算量。这说明上下文管理策略可以以技能形式被学习和迁移。

6. 在线强化学习结果：Qwen3.5-9B 在 BrowseComp-Plus 上性能提升 47.6%，同时 FLOPs 减少 12%。这验证了参数化学习上下文管理行为的可行性，且能同时改善性能与效率。

7. 服务优化：标准 SGLang 在 CLM 的多轮编辑场景中需要大量重新预填充，而 Suffix Cache Reuse 通过复用所有幸存 token 的缓存状态，在匹配性能下将服务器端计算减少 35%。但检索片段也指出，SCR 的收益受限于缓存状态存储的粒度，例如存储位置若在消息边界则还有进一步优化空间。

8. 一个值得注意的现象是，CLM 的上下文管理策略是自适应且创造性的，符合“苦涩教训”：让模型自己搜索策略比人工预设更有效。不过，关于策略的具体形态（如模型如何决定何时压缩、删除什么内容），论文材料中没有给出定性示例，属于待查证的细节。

Q6: 有什么可以进一步探索的点？

结合论文内容和检索证据，可进一步探索的方向包括：

1. 细粒度缓存状态存储：论文指出将 recurrent states 存储在更细粒度的位置（例如消息边界）是提高 SCR 效率和可复用性的有前景方向，未来可研究不同存储粒度与缓存命中率之间的权衡。

2. 上下文管理策略的可解释性：CLM 产生的编辑行为是自主涌现的，但目前缺乏对策略形态和决策原因的系统分析。未来的工作可以分析模型何时压缩、删除、重写，从而提炼可迁移的启发式规则或用于监督学习。

3. 训练方法的扩展：论文展示了零样本、技能进化和在线 RL 三条路径，但尚未探索在预训练阶段直接赋予模型原生 CLM 能力，或使用离线 RL、模仿学习等方法进一步优化。将上下文管理能力融入基础模型训练可能是未来方向。

4. 多智能体协作策略：多仓库 agent-swarm 任务已经受益于 CLM，但多个智能体之间如何共享、协调各自的上下文文件、如何避免冗余或冲突，仍值得深入研究。

5. 更广泛的任务验证：当前基准集中在编码、深度研究和数学开放发现，可以扩展到更多需要超长上下文的领域，如科学文献综述、法律文档分析、复杂软件工程、科学发现（AI for Science）等。

6. 安全性与可控性：给予模型不受限的上下文编辑权可能引入风险，例如模型可能误删关键信息或产生不一致状态。如何通过约束、验证或 human-in-the-loop 机制保证 CLM 行为的可靠性和可恢复性，是重要的开放问题。

7. 服务系统适配：SCR 目前是面向 CLM 的初步设计，未来可以与其他系统优化（如投机解码、分布式缓存）结合，并探索在长上下文、多请求并发场景下的扩展性。

Q7: 总结一下论文的主要内容

论文提出并系统研究了 Context Language Model（CLM）这一新范式。核心论点是：上下文管理应当从外部 harness 的控制转移到 LM 的内在行为，让模型自己决定在上下文中保留什么、丢弃什么以及如何组织信息。

论证主线：作者首先指出现有上下文管理方法（人工 harness、离线优化、受限工具集）都受限于人类先验，无法产生超出人工设计的策略。借鉴 Sutton 的 Bitter Lesson，他们认为应该让 LM 自由地搜索和学习上下文管理策略。为此，他们将 CLM 形式化为 c_{t+1} = f_CLM_θ(c_t)，即上下文可以经过任意函数变换，而标准 LM 只能追加 token。实现上，上下文被镜像成一个文件，模型可以对该文件进行不受限的编辑。这种设计遵循 less-is-more 原则，harness 层尽可能少地硬编码。

技术主线包含三个层次：（1）零样本 CLM：直接用现有 LM 通过提示转换，无需训练即可工作；（2）技能进化：通过自然语言指令的 skill-optimization loop 进化上下文管理策略，使之在 held-out 任务上超越初始点并扩展 Pareto 前沿；（3）在线强化学习：用 RL 直接优化 CLM 参数，使上下文管理成为可参数化学习的技能。系统层面，论文提出 Suffix Cache Reuse（SCR）来应对上下文中间编辑导致的缓存失效问题，并引入 prefix-reuse FLOPs 作为公平的计算开销指标。

实验主线覆盖从单智能体到多智能体的多种长期任务：编码（TerminalBench 2.1、TBLite）、深度研究（BrowseComp-Plus）、开放发现（多个数学难题）、EdgeBench 单仓库优化、24 小时多仓库 agent-swarm。在所有任务上，零样本 CLM 以更少的计算量达到了超越或匹敌最强基线的性能：BCP 上相对提升 11.4%、FLOPs 减少 21.5%；EdgeBench 上分数高 5%、FLOPs 少 59%；agent-swarm 上改进提升 65%。技能进化带来 held-out 准确率最多 35.9 点的提升；在线 RL 使 Qwen3.5-9B 在 BCP 上提升 47.6% 且 FLOPs 减少 12%；SCR 在匹配性能下减少 35% 服务器端计算。

论文的结论是：让语言模型原生管理自身上下文不仅可行，而且在性能和效率上都优于精心设计的外部上下文管理策略；同时，这种能力可以通过上下文内和参数化两种方式学习；系统层面的缓存复用可以支撑 CLM 的高效部署。论文还指出了改进空间，例如更细粒度的缓存状态存储。总体而言，这是一项将上下文管理从引擎盖下移到模型手中的系统性探索，对长期智能体研究具有重要参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的智能体方向（权重 0.10）直接相关：CLM 面向长期智能体任务，覆盖单智能体、subagent和 agent-swarm 多智能体场景。

## 基本信息

- 作者：Rulin Shao, Shannon Zejiang Shen, Junjie Oscar Yin, Yuetai Li, Minheng Wang, Hamish Ivison, Radha Poovendran, Nathan Lambert, Teng Xiao, Mike Lewis, Wen-tau Yih, Luke Zettlemoyer, Pang Wei Koh
- 机构：未提供
- 来源：arxiv
- 主题/分类：high_relevant
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37725v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（retrieved_evidence）与启发式草稿，结合论文摘要和节选内容进行整理；部分实验细节因证据缺失已明确标注为需核对原文之处。
