---
user_id: "cheng tan"
paper_id: 22699
arxiv_id: "2611.31621v1"
title: "Self-Improving Long-Horizon Reasoning by Learning Summaries from Local Continuations"
publish_date: "2026-11-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.31621v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.31621v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-28T01:36:17"
---
# Self-Improving Long-Horizon Reasoning by Learning Summaries from Local Continuations

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-horizon reasoning · context summarization · delta-kl · self-improvement

## 一句话总结

论文提出 TruSuCo：冻结推理模型，周期性截断推理轨迹，由独立摘要器把历史压缩成短状态并从“摘要+保留尾部”继续推理；摘要器用 Delta-KL（冻结 solver 在采样续写上的 token 级预测差异）训练，无需答案或外部教师，在 32k 预算的九个数学与 Reasoning Gym 设置上全面优于 vanilla 与未训练摘要器，且优势随预算扩展至 128k。

## 摘要

> Reasoning models degrade as their context grows, so longer reasoning traces do not by themselves give longer effective horizons. One way to extend the horizon is to compress the reasoning history into a compact state from time to time and continue from the state. Existing methods train the state with the final-answer reward or distill it from a stronger model. Neither signal is tailored to measure the contribution of the state alone. We observe that a reasoning model that degrades over long trajectories still behaves reliably in short contexts. We aim to preserve this short-context behavior when compressing the reasoning history. Motivated by this, we train a separate summarizer with Delta-KL, a reward computed from the frozen reasoning model's token-level predictions on sampled continuations, and run inference by truncating the trace, summarizing it, and continuing from the summary (TruSuCo). Training uses a few thousand questions with 4k-token reasoning prefixes and does not need answers or external teachers. Under a fixed 32k-token budget, the trained summarizer improves on vanilla reasoning and on an untrained summarizer in all nine settings across mathematical benchmarks and Reasoning Gym puzzles, by up to 7.7 points and 12 points, respectively. The improvement over vanilla reasoning grows with the budget. At 128k tokens, our method outperforms it by 10 points on AIME 2024--2026. Analyses show that truncation prevents the collapse of vanilla reasoning and that the trained summaries retain more of the reasoning history.

Q1: 这篇论文试图解决什么问题？

1. 核心问题不是“把上下文窗口做得更长”，而是“如何让推理模型在长预算下维持有效推理视野”。论文指出，许多模型虽然支持长上下文，但可靠性会随上下文增长而下降；因此单纯延长 trace 可能只是把更多 token 喂给一个逐渐失稳的模型，形成“上下文很长、有效 horizon 很短”的矛盾。

2. 现有压缩式续推方法的基本结构是：把推理切成若干段，周期性把历史压缩成紧凑 state（摘要或保留部分原轨迹），再从 state 继续。这样可控制活跃上下文长度，但 state 的质量直接决定后续推理。问题由此转化为：如何训练一个 summarizer，使压缩后的 state 足以支撑正确的后续推理。

3. 论文认为现有监督信号没有直接衡量“state 本身的贡献”。最终答案奖励（outcome-based RL）需要 ground-truth 答案或任务验证器，而且奖励归因于整条轨迹，难以区分某个中间 state 是否真的保留了关键信息；从更强模型蒸馏则优化的是“像教师”，而不是“让当前 solver 在 state 下保持其原本可靠的短上下文行为”。两者都不是针对 state-alone 贡献的信号。

4. 论文的关键观察是：一个在长轨迹上会退化的推理模型，在短上下文中仍然表现可靠。于是目标不是让摘要器生成“人类觉得好”的摘要，而是让 solver 在“摘要+tail”条件下表现得像在完整历史下一样。这使问题变成一个条件分布匹配问题：用冻结 solver 在采样续写上的 token 级预测，比较“完整历史条件”和“摘要状态条件”的差异。

5. 技术难点包括：如何构造反事实监督信号而不需要答案；如何把 token 级差异聚合为摘要器可优化的奖励；如何避免摘要器利用某些 token 的偶然增益抵消其他 token 的损失；如何让只在 4k 前缀上训练的摘要器外推到 128k 预算；以及如何区分“截断/重置上下文”本身的收益与“学习摘要”带来的增量。

6. 论文的隐含假设是：短上下文下的 solver 行为是值得保留的目标；冻结 solver 的 token 级续写分布足以代理 state 对后续推理的价值；文本摘要加少量 tail 足以承载长程推理所需信息；并且局部续写分布匹配可以迁移到更长的多轮压缩。上述假设在论文的实验范围内得到支持，但在开放任务、异构 solver、更强分布漂移下仍需检验。

7. 需要校准的是：给定材料主要来自摘要、引言、方法与结论片段，作者对“为何排除 outcome reward 与教师蒸馏”的完整论证、Delta-KL 的精确公式以及失败案例分析仍需回原文核对；这里的分析是基于论文明确表述与检索证据的重建，而非对未给出细节的补写。

Q2: 有哪些相关研究？

1. 测试时计算扩展（test-time scaling）：论文引用 Snell et al. 2024、Muennighoff et al. 2025，将自身置于“通过更长推理、采样或搜索提升 LLM 推理”的路线中。区别在于，论文强调 test-time compute 的收益受有效 horizon 限制，不能仅靠增加 token 预算。

2. 长上下文退化：论文引用 Hsieh et al. 2024、Du et al. 2025，指出模型支持长窗口不等于在窗口全 range 内可靠推理；性能可能随上下文增长而恶化。这是论文问题成立的经验前提，也是 TruSuCo 采用周期性截断的直接动机。

3. 上下文管理与分段续推：论文将 Yan et al. 2025, 2026、Wu et al. 2026、Aghajohari et al. 2025、Kontonis et al. 2026、Yang et al. 2026 等归为同一类方法：把推理组织成 segment，周期性减少 continuation 所用的上下文，例如生成摘要或保留原轨迹的一部分。论文与这类工作的共同点是“从紧凑 state 继续”，差异在于 state 的训练信号。

4. 基于任务结果的 state 训练：一类方法用最终答案奖励或任务 verifier 训练 state/summarizer。优点是直接对齐任务成功率，缺点是需要答案或验证器，且 outcome reward 对中间 state 的贡献归因较弱，长轨迹下方差和信用分配问题更突出。论文明确将自己与这一路线区分。

5. 基于教师蒸馏的 state 训练：另一类方法从更强模型蒸馏摘要或状态。它能提供密集监督，但优化目标是匹配教师行为，而非匹配当前 solver 自身在 state 下的续写分布；同时依赖外部教师，可能引入教师风格与能力边界。论文的 Delta-KL 试图避免这两类外部依赖。

6. 摘要、记忆压缩与上下文压缩：更广的相关研究包括对话摘要、长文档摘要、agent memory、KV cache 压缩、检索增强等。论文的具体设定是“文本摘要作为推理状态”，不是 KV cache 级压缩，也不是检索式记忆；但二者在“保留什么信息给后续步骤”上是相邻问题。

7. 分布匹配与 KL 型目标：Delta-KL 的名称和构造暗示其与 KL 散度、概率比、分布匹配有关；论文用冻结 solver 的 token 级预测构造奖励，可视为一种基于自身续写分布的自监督信号。给定片段未展开其与标准 KL、forward/reverse KL、序列级 vs token 级目标的完整关系，需回原文核对。

8. 相邻路线还包括 process reward model、step-level verifier、self-consistency、tree search 等。这些方法多关注“如何评价或搜索中间步骤”，而论文关注“如何压缩历史并让压缩状态可继续”。两者可以组合，但论文的给定材料没有系统比较这些组合。

Q3: 论文如何解决这个问题？

1. 总体框架：TruSuCo 使用两个模型——冻结的 solver πso 和可训练的 summarizer g。推理时，solver 不再无条件读取完整历史，而是周期性地从压缩 state st 继续；训练时只更新 summarizer，solver 保持冻结，用作“行为参照”。

2. 推理流程：设 K 为局部推理窗口长度，k 为保留尾部长度。第 t 轮，solver 从 state st 继续；st 末尾的 retained tail 与新生成的推理共同组成局部窗口 at。初始 s1 为空，因此第一个窗口只含新推理。当 at 达到 K（例如 4096）token 时，截断轨迹，保留最后 k（例如 128）token 作为新 tail，只把 at 的前 K−k 个 token 交给 summarizer 压缩。每段摘要的预算远小于被替换的推理，例如 512 token。若 solver 完成或耗尽总预算，则停止且不再更新 state。

3. 关键实现细节：state 不是作为新的 user message 插入，而是直接追加到 solver 已经打开的 assistant turn 之后，位于其 chat template 格式化后的 instruction 与 question 之后。这样 solver 会把摘要和 tail 当作自己正在进行的推理的一部分，从那里自然续写。

4. state 更新有两种形式（检索片段中可见）：append 形式为 mt ∼ g(X, (at)1:K−k)，st+1 = m1 ⊕ … ⊕ mt ⊕ latest tail，即累积此前所有摘要并加最新 tail；roll 形式为 mt ∼ g(X, mt−1, (at)1:K−k)，st+1 = mt ⊕ latest tail，即只保留最新摘要加 tail。两者都保留最新 tail，区别在历史摘要是否累积。

5. Delta-KL 训练信号：对采样得到的局部续写，比较冻结 solver 在两种条件下的 token 级预测——条件一是完整局部历史，条件二是“摘要+tail”状态。对逐 token 差异做加权平均；检索片段明确提到正差异先截断到零，以避免某些 token 上的增益抵消其他 token 上的损失；每个 probe 再按总权重归一化。直观上，这惩罚“摘要把 solver 带偏”的 token，奖励“摘要能让 solver 像看过完整历史一样续写”的摘要器。

6. 为什么不是答案奖励或教师蒸馏：Delta-KL 只询问“给定这个 state，冻结 solver 的续写分布是否接近其在完整历史下的分布”，因此直接针对 state-alone 的贡献；它不需要 ground-truth answer、任务 verifier 或更强教师。训练数据只需几千道题和 4k token 推理前缀，由 solver 自身产生推理，属于一种 self-supervised/self-distilled 的摘要学习。

7. 训练与评估的长度安排：摘要器只在约 4k token 的推理前缀上训练，约相当于对每个任务特定训练集过一遍；评估时在更大预算（如 32k、128k）上直接使用，不再训练。这是论文强调的泛化点：局部续写上学到的压缩行为可以支撑更长多轮压缩。

8. 需要校准：给定片段只展示了 Delta-KL 的聚合思路（加权平均、非负截断、按 probe 归一化），没有给出完整公式与优化算法（例如是否用策略梯度、如何采样 probe、奖励如何归一化到稳定尺度）。因此这里描述的是方法骨架，具体实现细节应回原文核对。

Q4: 论文做了哪些实验？

1. 研究问题：论文主要检验三件事——训练后的摘要器是否优于 vanilla reasoning 与未训练摘要器；优势是否随推理预算增长；以及收益是否来自“截断防止崩溃”与“摘要保留历史”这两个机制。

2. 任务与设置：实验覆盖数学基准与 Reasoning Gym 谜题，共九个设置。数学基准包括 AIME 2024–2026（至少在 128k 预算的对比中出现）；Reasoning Gym 包含需要持续多步推理的谜题类任务。论文在固定 32k token 总预算下报告主结果，并进一步测试 128k 预算。

3. 训练数据与规模：摘要器训练使用几千道题，配合 4k token 推理前缀；不需要答案、验证器或外部教师。检索片段还显示，训练约相当于对每个任务特定训练集过一遍，随后在更大预算上不再训练直接评估。

4. 对比基线：主对比是 vanilla reasoning（不压缩、直接长上下文继续）与 untrained summarizer（同样截断+摘要，但摘要器未训练）。分析中还出现“给 solver 一个清晰状态时的 working accuracy”作为参照，用来衡量训练后的摘要器离“理想清晰状态”有多近。给定材料没有显示与 outcome-based RL、教师蒸馏或 process reward 的系统对比。

5. 模型与采样配置：检索片段提到 R1-Distill-Qwen-1.5B 作为 solver 的一个配置，solver 采样 temperature 0.6、top-p 0.95；summarizer 推理采样 temperature 0.7、top-p 0.8、top-k 20。该片段可能来自附录配置表，不能据此断言所有实验都只用这一模型；但可确认论文至少在一个 1.5B solver 上做了系统实验。

6. 评价指标：主指标是任务准确率。摘要内容分析使用四个由固定程序计算的指标，不需要人类或模型 judge；其中 overlap 指标比较 solver 在“摘要后”与“完整上下文后”的续写重叠程度。其余三个指标的具体定义未在给定片段中展开。

7. 消融与分析设计：论文对比 trained summarizer、untrained summarizer、vanilla reasoning，并分析截断对 vanilla 长轨迹崩溃的影响；还比较训练摘要与未训练摘要保留推理历史的能力。给定材料未展示 K、k、摘要长度、append/roll 等超参数的完整敏感性分析，需回原文确认。

8. 统计与可复现性缺口：检索片段提到部分实例多次采样（例如 16 次）并出现 shaded bands/置信带式展示，但给定材料没有给出每个基准的逐项数值、方差、显著性检验或完整实验协议。代码与训练细节是否公开也未在元数据中体现。

Q5: 发现了什么实验现象？

1. 全面优于两个基线：在固定 32k 预算下，训练后的摘要器在数学基准与 Reasoning Gym 谜题的九个设置上全部优于 vanilla reasoning 与 untrained summarizer。相对 vanilla 最多提升 7.7 分，相对未训练摘要器最多提升 12 分。这个“全设置覆盖”比单点最高分更重要，说明方法不是只在一个任务上偶然有效。

2. 优势随预算增长：论文明确报告，相对 vanilla 的提升随推理预算增大而扩大；在 128k token 预算下，于 AIME 2024–2026 上领先 vanilla 10 分。结合 32k 时最多 7.7 分的提升，可看出长预算下 vanilla 的退化更严重，而压缩+学习摘要的收益更大。

3. 训练 4k、评估 128k 的外推：摘要器只在 4k token 推理前缀上训练，却在 32k 与 128k 预算上直接使用并取得更好结果。检索片段还显示，准确率在超过训练前缀长度后仍继续改善。这表明学到的不是某个固定长度的摘要模板，而是一种可重复使用的压缩行为；但外推机制仍需理论解释。

4. 截断本身能防止崩溃：分析表明，vanilla reasoning 在长轨迹上会出现崩溃/退化，而周期性截断可以避免这种崩溃。也就是说，一部分收益来自“重置上下文、回到短上下文可靠区间”，并非全部来自摘要内容。论文据此把“截断”与“摘要质量”作为两个需要区分的机制。

5. 训练摘要保留更多历史：在摘要内容分析中，训练后的摘要比未训练摘要保留更多推理历史；overlap 指标显示，solver 在摘要后的续写与在完整上下文后的续写更接近。这支持“Delta-KL 确实让摘要更贴近 solver 所需状态”，而不仅是生成了更短或更流畅的文本。

6. 接近清晰状态参照：检索片段显示，作者把 solver 在“清晰状态”下的 working accuracy 作为参照，训练后的摘要器在训练后接近该参照。这暗示摘要器已经较好复现了 solver 在短上下文中的工作行为；但“清晰状态”的具体构造与上界性质需回原文确认。

7. 指标间的张力：最终准确率提升与摘要内容指标改善方向一致，但 overlap 只是行为相似度的程序化代理，不等价于任务正确性；摘要可能在某些 token 上更接近完整历史，却在关键推理步骤上丢失信息。论文用四个程序化指标缓解这一点，但仍缺少对“摘要因果必要性”的逐例分析。

8. 负结果与失败模式：给定材料没有系统列出失败案例、负面结果或哪些设置提升最小；也没有报告摘要漂移、错误累积、摘要 hallucination 的具体频率。可以合理推断：多轮压缩会累积误差，tail 截断可能丢掉关键中间结果，Delta-KL 可能被某些 token 的局部匹配“奖励 hack”；这些都需要回原文的实验分析或附录确认。

Q6: 有什么可以进一步探索的点？

1. 理论刻画 Delta-KL：说明它优化的是序列级 KL、token 级 KL、还是某种 occupancy measure 的近似；分析非负截断与 probe 归一化带来的偏差-方差权衡；给出“局部续写匹配”何时能保证长程任务效用。

2. 自适应压缩策略：当前 K、k、摘要长度是固定超参数；可研究根据不确定性、任务难度、剩余预算或摘要置信度动态调整窗口与压缩率，避免在关键步骤过度压缩。

3. 结构化与多粒度 state：文本摘要之外，可探索分层摘要、结构化记忆（变量、子目标、证明状态、实验记录）、latent state、memory token、KV cache 压缩等；比较不同 state 形态对长程推理的影响。

4. 摘要器与 solver 的联合/交替训练：当前 solver 冻结，摘要器单独训练；可研究交替更新、co-evolution、在线自我改进，但要注意自我强化错误与分布漂移。

5. 与 outcome reward、process reward、verifier 结合：Delta-KL 提供无答案的密集信号，可与稀疏任务奖励或 step-level verifier 组合；需要检验组合后是否优于任一单独信号，以及是否引入 reward hacking。

6. 跨领域迁移：论文只验证数学与 Reasoning Gym 谜题；应测试开放域 agent 任务、代码修复、科学实验规划、长文生成、多模态推理等。不同领域的“关键历史信息”结构差异很大，摘要器可能需要领域适配。

7. 更大模型与更长预算：当前证据主要来自 1.5B solver；需验证 7B/70B 模型、1M token 预算、更多压缩轮次下的 scaling 行为，以及摘要器容量是否成为瓶颈。

8. 推理系统协同：把 TruSuCo 与 KV cache 管理、prefix caching、流式推理、工具调用结合；分析额外 summarizer 调用带来的延迟、显存与费用，给出端到端成本模型。

9. 评估体系：除准确率外，加入摘要忠实性、关键信息召回、因果必要性、对抗扰动、跨任务泛化、失败分类、人类可读性等指标；特别要区分“截断收益”与“摘要收益”的析因设计。

10. 安全与可靠性：研究摘要漂移、错误累积、摘要器幻觉、solver 对摘要的过度信任、多轮压缩中的自我确认偏差；在高风险场景中是否需要摘要验证器或回滚机制。

11. 数据效率与课程学习：论文用几千道题和 4k 前缀；可研究更少数据、零样本摘要器、合成数据、由短到长的课程学习，以及训练分布与测试分布不匹配时的退化边界。

12. 跨领域连接：Delta-KL 可视为用冻结模型自身分布作 reward 的通用 state 学习方法，可能与 model-based RL、world model 压缩、agent memory、知识蒸馏、序列决策中的 information bottleneck 等问题相连。

Q7: 总结一下论文的主要内容

一、问题与定位
论文关注长程推理中的一个核心矛盾：test-time scaling 让模型可以生成更长的推理轨迹，但模型的有效推理 horizon 并不随上下文窗口线性增长。相反，推理模型在长上下文中会退化，出现“上下文越长、后续步骤越不可靠”的现象。因此，论文不把问题定义为“如何支持更长上下文”，而是“如何让模型在长预算下持续处于可靠的短上下文工作区间”。

二、现有路线与缺口
一条自然路线是上下文管理：把推理切成段，周期性把历史压缩成紧凑 state，再从 state 继续。现有方法通常用最终答案奖励训练 state，或从更强教师蒸馏 state。论文指出，这两种信号都没有直接衡量 state 本身的贡献：outcome reward 需要答案/验证器，且把结果归因于整条轨迹；教师蒸馏优化的是匹配教师，而非匹配当前 solver 在 state 下的行为。于是论文提出：应该直接训练 summarizer，使 solver 在“摘要状态”下的续写分布接近其在“完整历史”下的续写分布。

三、方法与训练信号
推理流程名为 TruSuCo（Truncate, Summarize, Continue）。系统包含冻结 solver πso 与可训练 summarizer g。设局部窗口长度为 K、保留尾部长度为 k；solver 从 state st 继续，st 的 tail 与新推理组成局部窗口 at；当 at 达到 K 时，保留最后 k 个 token 作为新 tail，把前 K−k 个 token 压缩成短摘要；摘要预算远小于被替换的推理（例如 512 token）。state 直接追加到 solver 已打开的 assistant turn 中，使 solver 把摘要当作自己正在进行的推理。state 更新有 append（累积摘要）与 roll（只保留最新摘要）两种形式。

训练信号是 Delta-KL：对采样续写，比较冻结 solver 在完整历史与“摘要+tail”两种条件下的 token 级预测；对逐 token 差异加权平均，正差异截断到零以避免某些 token 的增益抵消其他 token 的损失，并按 probe 归一化。直观上，摘要器被奖励去生成“能让 solver 像看过完整历史一样续写”的摘要，而不是生成人类偏好的摘要。训练只需几千道题和 4k token 推理前缀，不需要答案、验证器或外部教师；solver 冻结，因此 supervision 来自 solver 自身，标题中的 self-improving 更准确地说是 self-supervised/self-distilled summary learning。

四、实验与发现
论文在数学基准与 Reasoning Gym 谜题共九个设置上评估。固定 32k 预算下，训练摘要器全面优于 vanilla reasoning 与 untrained summarizer，分别最多提升 7.7 分与 12 分。优势随预算增长；在 128k 预算下，于 AIME 2024–2026 上比 vanilla 高 10 分。值得注意的是，摘要器只在 4k 前缀上训练，却在更大预算上直接使用并继续受益，说明局部压缩行为可以外推到更长多轮压缩。

分析层面，论文报告截断可以防止 vanilla 长轨迹崩溃，训练摘要比未训练摘要保留更多推理历史；overlap 等程序化指标显示，solver 在摘要后的续写更接近完整上下文后的续写。作者还把“清晰状态下的 working accuracy”作为参照，训练后的摘要器接近该参照。这些结果共同支持：收益一部分来自截断/重置上下文，另一部分来自 Delta-KL 训练出的高质量摘要。

五、贡献与边界
论文的主要贡献是：把长程推理问题重新表述为“压缩 state 的分布匹配”问题；提出 TruSuCo 推理流程；提出 Delta-KL 这一无需答案/教师的 state-alone 训练信号；并在多个数学与谜题设置上展示预算 scaling。其边界也很明显：任务覆盖仍以数学和 Reasoning Gym 为主，模型规模证据以 1.5B solver 为主，摘要器与 solver 未联合进化，Delta-KL 的精确理论性质与失败模式尚未充分展开；此外，截断收益与摘要收益的析因分离、计算开销、统计显著性、与 outcome RL/教师蒸馏的直接比较，都需要回原文或后续工作补足。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对长程 agent 的直接启发：TruSuCo 可视为 agent context compaction 的一种实现——周期性截断、压缩历史、保留 tail 并从摘要继续；Delta-KL 提供了无 verifier 场景下的压缩质量训练信号。

## 基本信息

- 作者：Haoqun Cao, Tengyang Xie
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-11-23
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.31621v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要依据论文摘要、Introduction、Method、Conclusion 与 PDF 语义检索命中的方法/分析片段；heuristic_draft 是碎片化拼接、多处语义不完整，未作为事实依据，Delta-KL 完整公式与逐项实验数值仍需回原文核对。
