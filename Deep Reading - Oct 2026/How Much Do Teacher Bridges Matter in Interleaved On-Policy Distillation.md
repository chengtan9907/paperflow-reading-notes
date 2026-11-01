---
user_id: "cheng tan"
paper_id: 19310
arxiv_id: "2610.38383v1"
title: "How Much Do Teacher Bridges Matter in Interleaved On-Policy Distillation?"
publish_date: "2026-10-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.38383v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.38383v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-01T23:08:48"
---
# How Much Do Teacher Bridges Matter in Interleaved On-Policy Distillation?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · teacher bridge · interleaved distillation · kl divergence

## 一句话总结

这篇论文在 Qwen3-1.7B 学生与 Qwen3-4B-Instruct-2507 教师的最小 S→T→S 交错 on-policy distillation 设定中，用作者分段梯度和预注册实验检验教师桥接文本的上下文效应与监督目标效应，发现在短程训练与 BF16 推理下二者均未带来超过 ±5 个百分点等价界的准确率增益，教师段梯度很小且与学 生段梯度近乎正交，结论受剂量上限、停止生成失败和数值精度限制。

## 摘要

> On-policy distillation (OPD) trains a student using teacher predictions on student-generated responses. Some methods insert teacher-written segments, or bridges, but disagree on how to train on those tokens. We compare no loss, forward KL and reverse KL on identical trajectories using Qwen3-1.7B as student and Qwen3-4B-Instruct-2507 as teacher. We separate gradients by which model wrote each token. Under our short training runs and BF16 inference, pre-specified tests detected no added accuracy benefit from teacher text as context or as a training target. A one-paragraph bridge after the student's first paragraph is only 2.7% of the tokens and contributes a small gradient nearly orthogonal to the student-token gradient. In this main comparison, the three loss treatments gave similar gradient directions and were statistically equivalent in accuracy within a +/-5 percentage-point margin. An exploratory pilot placing the bridge where failed responses disagreed most with the teacher suggested a benefit from teacher-token loss. A pre-specified replication did not confirm this, and fourfold weighting showed no significant accuracy benefit. Two limitations bound these findings. Among the tested amounts of training on fixed responses, gains peaked after four updates before the student increasingly failed to stop generating. None of the tested online training variants met the criteria for stable stopping and improved accuracy. Our evaluations used BF16 inference, which rounds away most of the small weight changes. When evaluated in exact FP32, the tested replication models frequently reached the response-length limit. Our accuracy findings therefore concern BF16 inference; they do not establish that teacher-token supervision is generally unnecessary.

Q1: 这篇论文试图解决什么问题？

一、核心问题不是笼统的“教师桥接有没有用”，而是“通过哪条因果路径、在多大数据量下有用”。交错 OPD 的轨迹可写成 A→B→C：A 是学生前缀，B 是教师写的一段，C 是学生续写。B 至少通过两条路径影响学习：(1) 上下文路径，B 改变学生后续 token 的条件分布，使 C 的监督发生在教师诱导的状态上；(2) 目标路径，B 本身可作为学生分布的监督目标，例如对 B 计算 forward KL 或 reverse KL。现有方法在是否对 B 计损失、用哪种 KL 上不一致，导致“桥接有没有用”的答案不可比。论文要回答的是：在相同轨迹上，把 B 作为上下文与把 B 作为目标各自贡献多少；这种贡献能否在梯度层面解释；在什么剂量和评测精度下结论成立。

二、为什么这个问题重要。OPD 的昂贵之处在于 teacher forward 与长轨迹；桥接文本会增加 token 成本，也可能把学生推离自身分布。若收益主要来自上下文，则不应在教师 token 上重复计算损失；若收益主要来自目标监督，则桥接位置和损失方向是关键。方法选择直接影响训练稳定性、长度控制和计算效率。

三、现有证据的缺口。已有 TCOD、Re-OPD、Guided-OPD、DASH-OPD、Relay-OPD、FutureBridge-OPD 等变体，分别采用无损失、forward KL、多个 teacher-leg 目标等；也有 loss-direction 与 teacher-loss 消融以及组件梯度测量。但不同论文的轨迹、位置、剂量、评测不同，“教师 token 损失是否必要”仍缺少在同一轨迹上的受控比较。论文的贡献是把这个问题操作化为可预注册的等价检验和作者分段梯度测量。

四、隐含假设与边界。(a) 用一段固定位置的桥接代表“教师桥接”；(b) 固定轨迹上的短程训练能代表在线 OPD 的长期策略效应；(c) 准确率是主要目标，停止/长度是次要约束；(d) BF16 推理足以观察小权重变化。论文的结果恰恰显示 (b)(c)(d) 会改变结论：固定响应 4 次更新后收益见顶并出现停止失败；BF16 会舍入小更新，exact FP32 下频繁触顶。因此不能把 null 结果外推为“教师 token 监督普遍无用”。

五、可证伪性。论文用预注册、±5pp 等价界、Holm 校正和独立复现来约束结论；主研究只对三种损失处理建立统计等价，其他 null tests 只是未检测到显著差异。这个区分很重要：它把“没有证据”与“证据支持无差异”分开。总体上，问题从“桥接是否有效”被细化为“在何种梯度几何、剂量、停止行为和数值精度下有效”，这是本文最有价值的问题重构。

Q2: 有哪些相关研究？

一、On-policy distillation 的基本框架。OPD 让学生在自己的采样轨迹上接受教师 token 级分布监督，区别于完全离线地用教师生成文本做序列级蒸馏。其优势是训练状态更接近学生推理时状态；代价是轨迹质量、长度和停止行为会反过来影响监督信号。论文把 OPD 中的“教师写一段”视为一种特殊的状态干预，因此相关研究不仅是蒸馏算法，也包括轨迹级 credit assignment 和教师-学生分布对齐。

二、交错式教师桥接。引言把现有变体分为几类：TCOD、Re-OPD 让教师写前缀或前段；Guided-OPD、DASH-OPD、Relay-OPD 让教师写交错 turn 或 segment；FutureBridge-OPD 使用经过验证的 bridge。不同方法对教师写出的 token 采用不同损失：TCOD-B2F、Re-OPD 对教师 token 不计损失；Guided-OPD 对教师 token 用 forward KL；Relay-OPD 使用多种 teacher-leg 目标。Seg-OPD 还与“从失败回答中选高熵段”相关。这些工作共同说明教师桥接可能有用，但没有在同一轨迹上分离“上下文效应”和“目标效应”。

三、损失方向与教师 token 监督。forward KL 与 reverse KL 的差异不只是数值方向，而是 mass-covering 与 mode-seeking 的权衡；把教师 token 当目标，可能让学生模仿教师措辞，也可能提供额外信息。已有工作做过 loss-direction 和 teacher-loss 消融，也测量过组件梯度，但结论受轨迹、剂量、任务和评测影响。本文的增量不是提出更强算法，而是把损失处理放到完全相同的 A→B→C 轨迹上比较，并用作者分段梯度解释为什么不同处理可能给出相似结果。

四、梯度归因与负结果的方法学。论文邻近“组件梯度测量”一类工作：把总损失拆成学生 token 与教师 token 的贡献，比较梯度范数占比和余弦。与只报告最终准确率相比，这种测量能揭示“干预是否真的进入了参数更新”。同时，论文使用预注册、等价检验和复现实验，这与近年来对负结果、可复现性和评测数值精度的关注相邻。

五、与本文的关系。本文不是新 SOTA 方法，而是对现有交错 OPD 设计空间的受控审计：它把 TCOD/Guided-OPD/Relay-OPD 等不同选择还原为“是否对教师 token 计损失、用哪种 KL”的因子，并用梯度几何和停止行为解释 null result。因此，相关研究既包括具体 OPD 变体，也包括评测协议和数值精度对结论的影响。

Q3: 论文如何解决这个问题？

一、最小 S→T→S 设定。作者把一条交错轨迹写成 A→B→C：A 是学生写的前缀，B 是一段教师写的 bridge，C 是学生继续写的内容。主实验固定 A、B、C，使不同损失处理看到完全相同的 token 序列，从而避免 rollout 差异污染比较。这样可以把“B 作为上下文”与“B 作为训练目标”分开：前者通过条件分布影响 C 的监督，后者直接对 B 上的学生分布施加损失。

二、对照臂。摘要概括的核心比较是 no loss、forward KL、reverse KL 三种教师 token 损失处理；引言给出的完整对照包括：全位置 reverse KL、对 B 不计损失但保留 B 作为上下文、对 B 的 forward KL、与保留 token 数和终止 EOS 状态匹配的随机 mask、以及共享同一前缀的纯学生轨迹。随机 mask 用于排除“只要对部分 token 计损失就会有效果”的解释；纯学生轨迹用于比较教师桥接上下文相对于学生自己续写的价值。

三、作者分段梯度。论文的关键测量是按 token 作者拆分梯度：分别累计学生作者 token 与教师作者 token 对参数更新的贡献，并报告 token 占比、KL 质量占比、梯度范数占比、段间梯度余弦以及一步参数变化。这样可以直接回答“教师段的监督信号有多大、方向是否与学生段一致”。若教师段梯度很小且与学生段近乎正交，那么即使损失函数在数值上非零，它对实际参数更新的影响也可能有限。

四、统计协议。主研究使用预注册检验和 ±5 个百分点等价界，并报告置信区间；检索片段还显示使用 Holm 校正、E1/E2 评测集和 hard subset（初始学生答错的题）。作者明确区分“等价”与“未检测到显著差异”：主研究对三种损失处理建立统计等价，其他 null tests 只说明在当前样本和剂量下没有显著收益。

五、探索性 pilot 与预注册复现。作者先在“失败回答与教师分歧最大”的位置放置桥接，观察到教师 token 损失可能有益；随后用预注册的复现实验检验该信号。复现未确认 pilot 的收益，四倍教师损失权重也没有显著准确率提升，但梯度方向改变了 29 度。这个设计把探索性发现和确证性证据分开，避免把 pilot 当作结论。

六、剂量与数值精度探针。论文还测试固定响应上的训练更新次数，发现 4 次更新后收益见顶并出现停止生成失败；在线训练变体没有同时满足稳定停止和准确率提升。评测端比较 BF16 与 exact FP32，发现 BF16 会舍入掉大部分小权重变化，而 FP32 下模型更常触及 4096 token 上限、正确数更少。

七、设计边界。固定轨迹上的受控比较适合回答“单步/短程监督信号在哪里”，但不能完整代表在线 OPD 的长期策略效应；一段固定位置、固定长度的桥接也不能覆盖所有桥接设计。论文把这些边界写进结论，而不是把 null result 外推为普遍规律。

Q4: 论文做了哪些实验？

一、模型与任务。学生是 Qwen3-1.7B，教师是 Qwen3-4B-Instruct-2507。主评测以问题-答案正确性计分；检索片段显示主评测涉及 512 道题，另有 64 道 historical questions 用于长度/终止诊断，并划分 E1/E2 与 hard subset（初始学生答错的题）。评测使用推荐采样设置与 common random numbers 的片段也出现在检索证据中。

二、轨迹与桥接构造。核心设定是学生先写一段 A，教师写一段 B，学生再续写 C；B 是一段 one-paragraph bridge，放在学生第一段之后。该桥接只占轨迹 token 的 2.7%。主比较固定同一批 A/B/C 轨迹，避免不同 arm 的 rollout 差异。

三、主实验对照。核心比较 no loss、forward KL、reverse KL 三种教师 token 损失处理；完整对照还包括全位置 reverse KL、对 B 不计损失但保留 B 作为上下文、对 B 的 forward KL、与保留 token 数和终止 EOS 状态匹配的随机 mask、以及共享同一前缀的纯学生轨迹。这样同时检验“教师文本作为上下文”和“教师文本作为目标”两种通道。

四、统计与假设。论文使用预注册检验、±5pp 等价界和置信区间；检索片段显示使用 Holm 校正，且 P1 必须在 E1 257–512 这一半（未参与 pilot 的题）上也为正才算确认。主研究的目标是判断三种损失处理是否在实际意义上等价，而不是只报告 p 值。

五、探索性 pilot 与复现。Pilot 把桥接放在失败回答开始偏离教师的位置，观察到教师 token 损失可能有益；随后进行预注册复现。论文还测试四倍教师损失权重，观察梯度方向变化和准确率收益。

六、剂量与在线变体。固定响应上测试不同训练更新次数，发现 4 次更新后收益见顶，随后学生越来越无法停止生成。在线训练变体使用 fresh rollouts，检索片段还提到 10× 更小学习率，但没有一个变体同时满足稳定停止和准确率提升。

七、数值精度审计。评测比较 BF16 推理与 exact FP32。检索片段中的表格显示 batch 32 等设置，并报告 trained models 在 exact FP32 下更常触及 4096-token cap、正确数更少；seed-1 allRKL 在 64 道 historical questions 中有 42–43 道触及 cap，而初始学生只有 3 道。该片段用于说明停止/长度行为对评测 regime 非常敏感。

八、证据边界。论文没有在所有模型、任务、桥接位置和剂量上穷尽实验；主结论限定于短训练、BF16 推理、一段桥接和特定学生-教师配对。实验设计更像受控审计，而不是大规模 benchmark 竞赛。

Q5: 发现了什么实验现象？

现象 1：主比较是“没有检测到额外收益”，而不是“证明了零效应”。在短训练和 BF16 推理下，教师文本作为上下文或作为训练目标都没有带来预注册检验可检测的准确率提升；三种损失处理（no loss、forward KL、reverse KL）在 ±5pp 等价界内统计等价，梯度方向也相似。作者明确把主研究定义为等价检验，因此可以说“在测试范围内三种处理实际等价”，但不能说“教师 token 监督永远无用”。

现象 2：教师桥接的直接监督信号很弱。一段桥接只占 2.7% 的 token；作者分段梯度显示，教师段贡献的梯度范数很小，且与学生段梯度的余弦接近正交。这提供了一个机制解释：即使教师 token 上有非零损失，它对参数更新的方向和强度也可能不足以改变最终准确率。这里的“近乎正交”是论文明确报告的关键观察，不宜简单理解为教师段完全没有信息。

现象 3：简单放大教师损失权重不会把弱信号变成收益。四倍教师损失权重使梯度方向改变约 29 度，但没有显著准确率收益。这说明问题不只是“权重太小”：放大权重会改变优化方向，可能引入干扰，而不是线性放大有效信号。对于设计 teacher-loss 系数的工作，这是一个重要的反直觉结果。

现象 4：探索性 pilot 与预注册复现出现张力。Pilot 把桥接放在失败回答与教师分歧最大的位置，提示教师 token 损失可能有益；但在预注册复现中该收益没有确认。更细致地说，结论部分提到：在这个位置上，教师文本在推理时修复了更多答案，但作为上下文训练出的学生反而比用学生自己的改写训练更差。也就是说，“即时修复”不等于“学习后更好”，上下文收益和目标收益可能方向相反。

现象 5：训练剂量存在上限，停止行为先崩。固定响应上，准确率收益在 4 次更新后见顶，随后学生越来越无法停止生成。检索片段中 seed-1 allRKL 在 64 道 historical questions 上有 42–43 道触及 4096-token cap，而初始学生只有 3 道；这显示长度/终止失败不是边缘现象，而是剂量放大后的主要失效模式。在线变体也没有一个同时满足稳定停止和准确率提升。

现象 6：BF16 与 FP32 的评测差异会改变结论。BF16 推理会舍入掉大部分小权重变化；在 exact FP32 下，受测复现模型更频繁触及 4096-token 上限，正确数也更少。这意味着 BF16 下的“无准确率收益”可能部分是数值精度造成的可观测性上限，不能直接外推到 exact arithmetic 或更大权重更新。

现象 7：随机 mask 对照提示要区分“教师 authorship”与“对部分 token 计损失”。论文设置了与保留 token 数、终止 EOS 状态匹配的随机 mask；其具体数值需回原文核对，但设计意图是排除“只要对一部分 token 加损失就会有效果”的解释。结合总体 null result，更稳妥的解读是：在当前剂量下，教师 token 的特殊信息没有转化为可检测的准确率优势。

现象 8：准确率与停止行为之间存在权衡。训练初期学生回答变长但仍能终止；剂量增大后，长度继续增长而终止失败增加。对 OPD 来说，这意味着不能只以准确率选 checkpoint，否则可能选到“答得更长但停不下来”的模型。

Q6: 有什么可以进一步探索的点？

一、剂量与停止感知的训练目标。当前最直接的缺口是剂量上限：固定响应 4 次更新后收益见顶，随后停止生成失败。后续应在更大剂量、fresh rollouts 和在线 OPD 下联合优化准确率、终止概率与长度，例如加入长度惩罚、EOS 校准、truncation-aware loss 或约束更新，并报告 accuracy–termination Pareto 曲线，而不是单点准确率。

二、桥接位置与内容选择。Pilot 的失败点放置提示“在哪里插桥”可能比“是否插桥”更重要，但预注册复现未确认。后续可用高分歧、高熵、verifier 失败、学生不确定等信号选择桥接位置，并在独立题集上做预注册复现；同时比较教师原文、学生改写、验证通过的 bridge 和随机片段，避免把 pilot 的选择效应当成真实收益。

三、损失几何与梯度对齐。四倍加权只改变 29 度方向且无显著收益，说明需要研究梯度投影、PCGrad、余弦约束、自适应权重或分层学习率，避免教师段梯度与学生段梯度相互干扰。还应区分“梯度太小”和“梯度方向不对”：前者可靠提高权重，后者需要改变目标函数或参数化。

四、教师 token 的 credit assignment。并非所有教师 token 都同等重要；后续可按 token 影响力、KL 质量、学生分歧或因果贡献选择子集，测试“只在关键 token 上计损失”是否比整段桥接更有效。也可比较一段桥接与多段桥接、不同桥接长度、以及桥接前后学生分布的偏移，判断收益是否被稀释。

五、上下文效应与目标效应的因子化实验。本文用固定轨迹做了初步隔离；后续可在在线 rollout 中做 2×2 因子设计（是否插入 B × 是否对 B 计损失），并测量长期策略偏移。需要区分“B 改变了 C 的监督状态”与“B 直接监督学生分布”的交互，避免把总效应归因给单一通道。

六、评测与可复现性。BF16 与 exact FP32 的差异说明，小权重更新下的准确率结论对数值精度敏感。后续应双报告 BF16/FP32、报告 4096-token cap 触发率、长度分布和终止率；对负结果使用等价检验、预注册和多随机种子；并公开轨迹、mask、损失权重和评测脚本，方便复核。

七、跨模型、跨任务与长程场景。当前只覆盖一个学生-教师配对和短程问答式任务。后续应测试不同规模/家族的学生与教师、数学/代码/多轮对话/工具使用/agent 长程任务，观察“教师桥接梯度正交”是否是普遍现象，还是小模型、短桥接和特定任务的产物。

八、理论解释。可以借用因果中介分析、影响函数或梯度核分析，把总效应分解为 context-mediated effect 与 direct-target effect；并解释为什么教师段梯度会与学生段近乎正交：是信息冗余、分布错位、token 太少，还是损失尺度问题。理论刻画有助于预测什么时候桥接值得插入。

九、应用迁移。对 agent 轨迹蒸馏，教师/环境插入的中间段类似“工具返回”或“人类修正”；本文的上下文 vs 目标区分可直接用于设计 agent 的 trajectory-level credit assignment。对 AI-for-science 的高风险评测，预注册、等价检验和数值精度审计比单纯追求准确率更值得借鉴。

Q7: 总结一下论文的主要内容

一、研究问题。论文从一个被现有方法混在一起的问题出发：在交错式 on-policy distillation 中，教师插入的 bridge 文本到底通过什么机制起作用？一条轨迹可以写成 A→B→C：A 是学生前缀，B 是教师写的一段，C 是学生续写。B 可以作为上下文改变 C 的监督状态，也可以作为目标直接训练学生分布。现有 TCOD、Re-OPD、Guided-OPD、DASH-OPD、Relay-OPD、FutureBridge-OPD 等变体在“是否对教师 token 计损失、用 forward 还是 reverse KL”上选择不同，导致“桥接有没有用”的结论不可比。论文要回答的是：在相同轨迹上，两种通道各自贡献多少；这种贡献能否在梯度层面解释；结论在什么剂量和评测精度下成立。

二、方法主线。作者构造最小 S→T→S 设定，固定同一批 A/B/C 轨迹，比较 no loss（保留 B 作为上下文）、forward KL on B、reverse KL 等教师 token 损失处理；引言还列出全位置 reverse KL、与保留 token 数和终止 EOS 状态匹配的随机 mask、以及共享同一前缀的纯学生轨迹作为对照。关键测量是作者分段梯度：按 token 作者拆分学生段与教师段的梯度贡献，报告 token 占比、KL 质量占比、梯度范数占比、段间余弦和一步参数变化。这样可以直接判断教师段的监督信号是否足够大、是否与学生段同向。统计上使用预注册检验、±5pp 等价界和 Holm 校正，并区分主研究的等价结论与其他 null tests 的“未检测到显著差异”。

三、实验主线。学生是 Qwen3-1.7B，教师是 Qwen3-4B-Instruct-2507；桥接是一段放在学生第一段之后的 one-paragraph bridge，仅占 2.7% token。主实验在固定轨迹上比较损失处理，并在 BF16 推理下评测准确率。作者还做了探索性 pilot：把桥接放在失败回答与教师分歧最大的位置，观察教师 token 损失是否带来收益；随后进行预注册复现，并测试四倍教师损失权重。剂量方面，作者在固定响应上增加更新次数，观察收益是否随剂量增长；在线变体则使用 fresh rollouts。评测方面比较 BF16 与 exact FP32，并记录 4096-token cap 触发情况。

四、主要发现。主比较没有检测到教师文本作为上下文或作为训练目标的额外准确率收益；三种损失处理在 ±5pp 内统计等价，梯度方向相似。教师桥接只占 2.7% token，其梯度范数小，且与学生段梯度近乎正交，这为“直接监督信号太弱/方向不对”提供了机制解释。四倍教师损失权重使梯度方向改变约 29 度，但没有显著准确率收益，说明简单放大权重不是解法。探索性 pilot 曾提示失败点放置的教师 token 损失可能有益，但预注册复现未确认；pilot 中还出现“教师文本在推理时修复更多答案，但作为上下文训练出的学生比学生自己的改写更差”的张力。剂量上，固定响应 4 次更新后收益见顶，随后学生越来越无法停止生成；在线变体没有一个同时满足稳定停止和准确率提升。BF16 会舍入掉大部分小权重变化；exact FP32 下模型更常触及 4096-token 上限、正确数更少，因此准确率结论限定于 BF16 推理。

五、作者的解释与边界。作者把结果解释为：在测试剂量和 BF16 评测下，教师桥接的上下文效应和直接目标效应都没有强到足以改变准确率；教师段梯度小且近乎正交，说明它没有有效地进入参数更新。论文明确不主张“教师 token 监督普遍无用”：主研究只建立三种损失处理的等价，其他 null tests 不构成等价证明；短训练、固定轨迹、一段桥接、单一学生-教师配对和 BF16 推理都是边界。尤其是停止生成失败和 FP32 下的长度触顶，说明优化/评测 regime 会显著改变结论。

六、方法论意义。本文的价值不在于提出新 SOTA，而在于把“教师桥接是否值得”拆成可测量的因果通道和梯度几何，并用预注册、等价检验和复现来约束负结果。它给出一个可复用的审计流程：先测量教师 token 的梯度占比和方向，再决定是否值得加损失；同时必须监控停止行为、长度和数值精度，否则准确率结论可能不可靠。对 OPD、交错式蒸馏和 agent 轨迹学习，这篇论文更像一份边界清晰的负结果报告，而不是通用配方。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：如果你的关注点是 agent 轨迹蒸馏，本文把“教师/环境插入的中间段”拆成上下文与监督目标两种角色，并给出可复用的作者分段梯度诊断；这对长程 credit assignment 设计有直接参考价值。

## 基本信息

- 作者：Leran Chen, Zhiyuan Yao, Lingnan Kong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-10-28
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.38383v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要依据摘要、Introduction、Conclusion 和 PDF 语义检索片段；检索片段存在页眉、表格残片和章节标签错位，已按证据类型归位，并对未明确的数值与方法细节保留“需回原文核对”的边界。
