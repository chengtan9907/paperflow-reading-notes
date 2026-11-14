---
user_id: "cheng tan"
paper_id: 20748
arxiv_id: "2611.11997v1"
title: "HyInv Technical Report"
institution: "腾讯混元（Tencent Hunyuan）"
publish_date: "2026-11-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.11997v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.11997v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-14T17:38:52"
---
# HyInv Technical Report

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：inverse folding · protein design · graph neural network · reinforcement learning

## 一句话总结

HyInv 是一个仅 1.96M 参数、以蛋白主链为条件的编码器-解码器图网络，经 Hyra 2.0 组织的随机顺序自回归预训练和结构奖励强化学习后，用分组 draft-and-rewrite 采样器在匹配或更高序列多样性下取得高于十个外部基线的平均 scTM，并在单张 NVIDIA H20 上以 50.6 ms 为 402 残基主链生成 128 条序列。

## 摘要

> Inverse folding designs amino-acid sequences for a given protein backbone, and screening needs many candidates that are both diverse and well folded. We present HyInv, a 1.96-million-parameter backbone-conditioned graph network with an encoder-decoder architecture, trained by Hyra 2.0, the research agent developed by our team: pretrained with random-order autoregressive sequence modeling, followed by structure-reward reinforcement learning. At inference, a grouped draft-and-rewrite sampler that uses separate draft and rewrite temperatures improves refolding TM-score at matched diversity. An optimized implementation with BF16 numerics designs 128 sequences for a 402-residue backbone in 50.6 ms on a single NVIDIA H20 GPU, about 52 times faster than the reference sampler, and an exact FP32 path that returns the reference sequences is 8.7 times faster. Across five benchmark datasets and seven dataset-predictor combinations, HyInv reaches a higher average scTM than all ten external baselines at comparable or higher sequence diversity, with large margins over ProteinMPNN and ProteinZero V2. The same default setting supports de novo backbones, pocket-constrained libraries, enzyme redesign with the catalytic site kept, and loop redesign. We have released our code and model at https://github.com/Tencent-Hunyuan/Hyra-results/tree/main/AI4Science/hyinv.

Q1: 这篇论文试图解决什么问题？

一、任务定义与筛选场景的核心矛盾
逆折叠（inverse folding）给定蛋白主链，要求设计能折叠回该主链的氨基酸序列，是 scaffold 设计、酶工程和序列文库构建的基础环节。论文把问题放在“筛选”而不是“单点最优”场景：实验筛选需要大量候选，模型的价值取决于在给定结构质量下能提供多少序列多样性。因此评价不能只看一条序列的 scTM，而要看“质量-多样性”曲线；这正是论文反复强调“at matched diversity”的原因。

二、现有方法的三个评价盲点
第一，ProteinMPNN 类模型通常用单一采样温度生成，温度同时影响结构一致性和多样性，单温度比较会把“高质量但多样性低”误判为整体更优。第二，自回归/并行解码模型（LM-Design、CMLM、DPLM-2 等）的解码顺序或掩码策略固定，推理时难以灵活地局部重写。第三，ProteinZero 证明用结构预测器做 RL 奖励有效，但奖励模型偏差、采样成本和多样性控制仍是问题。论文据此把“匹配多样性下的 scTM”作为主比较轴。

三、工程瓶颈
筛选场景要在可接受时间内生成成千上万条序列。参考采样器对 402 残基主链生成 128 条序列需要约 2.6 s（片段中的 reference sampler 1×），对大规模文库仍然偏慢；长主链和批量候选会放大成本。论文因此同时优化算法（分组 draft-and-rewrite）和实现（BF16、跨候选共享计算），把时间压到 50.6 ms。

四、论文要补的空白
论文的定位不是提出全新的网络族，而是把“小模型 + 随机顺序预训练 + 结构奖励 RL + 温度解耦采样 + 工程优化”组合成一个系统，并在统一默认设置下覆盖 de novo、口袋、酶和 loop 四类任务。它试图回答：在 1.96M 参数级别，是否仍能在匹配多样性下超过十个外部基线，并把推理成本降到筛选可用。

五、隐含假设与边界
核心假设是 ESMFold2 预测结构得到的 scTM 能代表真实可折叠性；训练奖励也主要来自该预测器，存在 reward hacking 和 predictor bias 风险。论文用 Boltz2/Protenix2 做交叉评估只能部分缓解，因为三个预测器可能共享训练数据与归纳偏差。另一个边界是只处理单链主链，多链界面和膜环境未覆盖。提供的文本中 method/discussion 为空，RL 细节和失败模式无法完整复核，相关判断需回原文确认。

Q2: 有哪些相关研究？

一、基于消息传递的逆折叠模型
论文把 HyInv 放在 Ingraham et al. (2019) 与 ProteinMPNN (Dauparas et al., 2022) 开启的图网络传统中：把主链残基作为节点，用几何消息传递编码局部环境，再逐位置预测氨基酸。ProteinMPNN 让逆折叠 routine 化，其 grouped 解码和低噪声变体也被列为基线。HyInv 沿用主链条件图网络，但只有 1.96M 参数，并用编码器-解码器与随机顺序自回归改造解码过程。

二、语言模型辅助与并行解码
Hsu et al. (2022)、Gao et al. (2023)、Zheng et al. (2023)、Wang et al. (2025a) 被归为“language-model-assisted and parallel decoders”，代表把蛋白语言模型、掩码语言模型或并行解码用于逆折叠的路线。基线中的 LM-Design 650M、CMLM、DPLM-2 3B 属于这一谱系：参数规模远大于 HyInv，利用大规模序列预训练，但推理成本和采样控制方式不同。论文的对照意图是：领域专用的小图网络加 RL 和采样器，能否在匹配多样性下胜过通用大模型。

三、结构奖励强化学习
ProteinZero (Wang et al., 2025b) 展示了用结构预测器做 RL 奖励的价值，ProteinZero V2 (ESM2/ESM1) 是论文最强的外部基线之一。HyInv 的 RL 使用 TM-align 奖励、RLOO 留一法基线和 KL anchor，这些术语与 Ahmadian et al. (2024) 的 REINFORCE-style 优化传统相关。与 ProteinZero 的区别在于 HyInv 先做随机顺序自回归预训练，再用分组采样和温度解耦推理；论文称在匹配多样性下仍有较大 margin。

四、结构预测器与评价指标
scTM 定义为设计序列的预测结构与输入主链之间的 TM-score（Zhang and Skolnick, 2004）。论文主预测器是 ESMFold2 (Candido et al., 2026)，并在 RecentPDB 上额外用 Boltz2 (Passaro et al., 2025) 与 Protenix2 (Zhang et al., 2026) 折叠，且这两个预测器不参与训练。这个设计直接回应“奖励模型过拟合”质疑：如果换预测器仍保持优势，说明提升不只是 ESMFold2 的偏好。

五、采样与解码策略
论文的 draft-and-rewrite 与温度解耦，和 speculative decoding 的“先草稿再修正”在名称上接近，但这里不是用验证器接受/拒绝 token，而是对序列分组做 mask-and-resample；这一点属于合理推断，原文方法段未在提供文本中展开。相关对照是 ProteinMPNN 的 grouped 解码、低噪声采样，以及自回归模型的固定顺序解码。

六、科研智能体与本文关系
作者称训练由团队开发的科研智能体 Hyra 2.0 完成。这把论文同时放进“AI for Science”和“agent 做研究”两条脉络：前者是逆折叠应用，后者是让智能体参与预训练、RL 和实验组织。但提供的文本没有说明 Hyra 2.0 的自主程度、搜索空间、人类监督和算力预算，因此不能把它等同于完全自主发现。

七、空白定位
既有工作分别强调结构感知网络、大规模序列预训练或 RL 微调，但较少同时在“匹配多样性比较”“跨预测器验证”“筛选级推理速度”“统一默认设置覆盖多任务”四个维度组织。HyInv 的贡献更像系统集成与评价协议，而不是单一架构创新。

Q3: 论文如何解决这个问题？

一、总体架构
HyInv 是一个 1.96M 参数的主链条件图网络，编码器-解码器结构，通过消息传递在残基图上编码主链几何环境，再由解码器输出每个位置的氨基酸分布。论文没有在提供文本中给出节点/边特征、层数、隐藏维度、注意力或几何 encoder 的具体配置；从 ProteinMPNN 传统的表述看，使用 N、Cα、C、O 等主链原子构建局部坐标系与相对距离是合理推断，但不能当作原文确认。模型规模刻意做小，目标是把参数效率、采样速度和多样性控制放在一起。

二、第一阶段：随机顺序自回归预训练
预训练目标是在天然结构和预测结构上最大化序列似然，但残基解码顺序是随机的，而不是固定的 N→C 或 C→N。这样做的直接好处是：模型见到各种“已知部分序列、预测剩余位置”的条件组合，推理时可以任意选择 mask 位置和重写顺序。论文把这一步称为 random-order autoregressive sequence modeling；它不同于标准掩码语言模型的固定 mask ratio，也不同于固定方向自回归。具体顺序分布、mask 比例、噪声调度和结构数据配比未在提供文本中说明。

三、第二阶段：结构奖励强化学习
RL 阶段对每个主链采样一组候选，用 ESMFold2 折叠，再用 TM-align 将预测结构与输入主链对齐得到 scTM 类奖励；用 RLOO（leave-one-out）估计基线降低方差，并加 KL anchor 防止策略偏离预训练分布。证据片段中出现“one group at a time”“8 rounds”“T = 0.34”“Update Score RLOO”“KL anchor”“TM-align reward”等字样，但图注被截断，无法确定 8 rounds 是 RL 迭代轮数、采样分组数还是 rewrite 轮数；也不能确认奖励是否只含 scTM、是否有多样性惩罚或序列相似度约束。按 RLOO 的常规做法推断，group 内每个样本的基线是除自身外的平均奖励，这能降低策略梯度方差。

四、推理：分组 draft-and-rewrite
推理不是一次性自回归解码，而是两段式：先以较高的 draft 温度 Td=0.6 生成草稿序列，铺开多样性；再以较低的 rewrite 温度 Tr=0.34 对一组位置做 mask 并重采样，其余位置保持未 mask 作为上下文。每一轮只处理一个 group，多轮迭代后得到最终序列。这样做的直觉是：draft 阶段负责“探索”，rewrite 阶段负责“局部修正”，两个温度解耦后可以在不牺牲多样性的情况下提高结构一致性。它类似 Gibbs 式的迭代条件重采样，但“group”指候选分组还是残基分组，提供文本没有完全说清；从“share work across candidates”看，工程上可能同时利用候选并行。

五、工程实现
论文报告了三个层级的优化：参考采样器 1×（约 2.6 s/128 条 402 残基序列）；跨候选共享工作后 8.7×（约 304 ms），这条精确 FP32 路径能返回与参考采样器一致的序列；进一步用 BF16 数值和优化实现达到 52×（50.6 ms）。BF16 路径可能带来数值差异，但论文把它作为默认快速路径，同时保留 FP32 精确路径用于复现。具体优化包括哪些 kernel、是否用批处理/缓存/并行 rewrite，提供文本未展开。

六、默认设置与任务适配
论文强调同一默认设置支持 de novo 主链、口袋约束文库、保留催化位点的酶重设计和 loop 重设计。口袋和酶任务通常需要在固定功能位点的条件下设计其余区域，loop 任务则只重设计局部片段；随机顺序预训练和 mask-and-rewrite 天然适合这种“部分位置固定、部分位置生成”的条件设计。具体条件注入方式（例如固定残基的 logits 约束、mask 策略、位点选择）未在提供文本中给出。

七、技术权衡
小模型带来速度和参数效率，但容量有限；RL 能直接优化 scTM，但可能过拟合 ESMFold2 的偏好；温度解耦提高匹配多样性下的质量，但引入 Td、Tr、group 大小、轮数等超参数；BF16 提速但需验证数值稳定性。论文的贡献在于把这些权衡打包成一个可用的默认配置，而不是证明每个组件都独立最优。

Q4: 论文做了哪些实验？

一、评测面板
论文使用五个测试面板：CATH4.3 test（1,882 条链）、CATH4.2 test（1,120 条）、TS500（500 条）、TS50（50 条），以及 RecentPDB（来自近期 PDB 实验结构的 121 条链，111 个条目，26,766 个残基，长度 42–978）。所有面板都是标准链、主链完整，并在 2025 年前筛除了与 PDB/CATH 条目同源的序列。RecentPDB 的设计分别用 ESMFold2、Boltz2 和 Protenix2 折叠，后两者不参与训练。

二、指标
主指标是 scTM：设计序列的预测结构与输入主链之间的 TM-score。辅助指标包括 native sequence recovery（与天然序列的一致性）和 sequence diversity（同一 target 的多个设计之间的平均成对差异）。论文明确说单温度评估会掩盖结构一致性与多样性的权衡，因此主比较在“可比或更高多样性”下进行。多样性具体定义、比对方式和统计口径在 Appendix B，提供文本未展开。

三、基线
外部基线共十个：ProteinZero V2 (ESM2)、ProteinZero V2 (ESM1)、ProteinMPNN grouped、ProteinMPNN、ProteinMPNN low-noise grouped、LigandMPNN、PiFold、LM-Design 650M、CMLM、DPLM-2 3B。表中还列出 Native sequence 作为参照。基线覆盖图网络、配体感知、语言模型辅助、并行解码和大规模蛋白语言模型，比较谱系较宽。

四、主实验设计
五个面板 × 预测器组合形成七个评价列：CATH4.3、CATH4.2、TS500、TS50 各用 ESMFold2，RecentPDB 用 ESMFold2/Boltz2/Protenix2，共七列。主表报告 HyInv 的绝对 scTM 以及相对部分基线的差值；差值带星号表示作者认为“clearly positive with and without filter”。从片段看，并非每个基线、每一列都有完整数值，部分单元格在文本提取中缺失。

五、采样器与温度消融
论文用示意图说明：在相同多样性下，分离 draft 与 rewrite 温度比普通采样得到更高 scTM；draft T=0.6 负责“blind, sampled warm”，rewrite T=0.34 更冷，逐组重写。这个消融是论文的核心机制证据之一，但提供文本只有图注片段，没有完整的温度网格、轮数消融或 group size 消融。

六、推理速度实验
参考采样器对 402 残基主链生成 128 条序列约 2.6 s；跨候选共享工作后 8.7×（约 304 ms），且 FP32 路径能精确返回参考序列；进一步 BF16 优化达到 52×（50.6 ms），在单张 NVIDIA H20 上测量。论文还把“128 条序列、402 残基”作为固定 benchmark 配置，未在提供文本中给出不同长度、不同 batch size 或不同 GPU 的 scaling 曲线。

七、应用案例
论文展示四类任务：de novo 主链设计、口袋约束文库、保留催化位点的酶重设计、loop 重设计。口袋案例报告十二个主链上的平均 pass rate 为 43%，pass 定义为 scTM ≥ 0.9 且 RMSD ≤ 1.0 Å（相对天然配体）；酶案例展示催化几何被保留；loop 案例展示局部重设计。案例主要是计算指标和结构可视化，提供的文本中没有湿实验验证。

八、新颖性与可折叠性分析
Results 片段提到“foldable: scTM ≥ 0.9”“no natural homolog detected”“30% identity to nearest natural sequence”“novel and foldable designs per backbone at matched diversity”。这些分析试图说明 HyInv 不是简单复制天然序列，而是能生成新颖且可折叠的序列。但片段截断严重，具体数据集、同源检测方法和统计分母需要回原文核对。

九、缺失的实验信息
提供文本没有给出：RL 训练集规模与来源、reward 设计细节、训练算力、超参数搜索范围、基线调参预算、统计检验方法、按长度/难度分层的结果、失败案例率、多链或膜蛋白实验。这些缺口会影响对“全面优于十个基线”的复核。

Q5: 发现了什么实验现象？

一、主表上的整体优势
在 ESMFold2 评价下，HyInv 在 CATH4.3、CATH4.2、TS500、TS50 的 scTM 分别为 0.840、0.855、0.899、0.920。相对 ProteinZero V2 (ESM2) 的增量为 +0.007、+0.008*、+0.004*、+0.008；相对 ProteinMPNN 为 +0.014*、+0.011*、+0.004、+0.014。RecentPDB 在 ESMFold2/Boltz2/Protenix2 下分别为 0.861、0.874、0.878，七列平均 0.875；相对 PZ V2 (ESM2) 为 +0.011*、+0.010*、+0.011*、+0.008，相对 ProteinMPNN 为 +0.018*、+0.012*、+0.016*、+0.013。作者据此称在可比或更高多样性下平均 scTM 高于全部十个外部基线。

二、跨预测器一致性
RecentPDB 上 Boltz2 与 Protenix2 的 scTM 略高于 ESMFold2（0.874/0.878 vs 0.861），且这两个预测器不参与训练。合理推断是：HyInv 的提升不只是迎合 ESMFold2 的 reward，换预测器后仍保持较好结构自一致性。但要注意，三个预测器可能共享 PDB 训练数据和结构先验，跨预测器一致性只能部分排除 reward overfitting，不能替代实验验证。

三、匹配多样性下的质量提升
论文的核心现象是：在相同或更高序列多样性下，HyInv 的 scTM 高于普通采样和基线。这说明它的提升不是靠“多样性换质量”或“质量换多样性”的单边取舍，而是同时改善了两者的前沿。温度解耦示意图进一步显示：draft 温度较高、rewrite 温度较低时，匹配多样性下的 scTM 更高；如果只用单一温度，质量-多样性曲线更差。这是机制层面最值得关注的现象。

四、小模型与大模型的对比
HyInv 只有 1.96M 参数，却在平均 scTM 上超过 DPLM-2 3B、LM-Design 650M 等大模型基线。这个反直觉结果的可能解释是：逆折叠是强结构条件的局部任务，领域专用几何网络 + 结构奖励 RL 比通用序列预训练更有效；同时，匹配多样性比较和采样器设计可能放大了 HyInv 的优势。也不能排除基线在统一协议下未充分调参，需看原文的基线配置。

五、速度现象
参考采样器 2.6 s → 跨候选共享 304 ms（8.7×）→ BF16 优化 50.6 ms（52×）。速度提升不是单纯的硬件升级，而是“共享工作 + 数值格式 + 分组采样”的累积优化。精确 FP32 路径能返回参考序列，说明 8.7× 加速没有改变采样结果；BF16 路径则可能引入微小数值差异，论文把它作为快速默认路径。

六、应用案例中的现象
口袋设计在十二个主链上的平均 pass rate 为 43%，pass 要求 scTM ≥ 0.9 且配体 RMSD ≤ 1.0 Å；通过的 design 中，口袋保持相同，其余区域可以差异很大。酶重设计案例显示催化几何被保留。loop 重设计展示局部区域可变、其余结构固定。这些现象支持“同一默认设置可做条件设计”的说法，但案例数量、失败模式和统计分母有限。

七、新颖性与可折叠性
片段提到 scTM ≥ 0.9、未检测到天然同源、与最近天然序列约 30% identity 的设计，说明模型能生成远离天然序列但预测可折叠的候选。这是逆折叠应用于新颖 scaffold 和酶设计的关键指标。不过“no natural homolog detected”依赖同源检测数据库和阈值，30% identity 的具体分布也未给出；这些结论需要回原文核对。

八、指标之间的张力
第一，scTM 高不等于真实可折叠，尤其对柔性 loop、金属位点和膜蛋白。第二，序列多样性高不等于文库覆盖好；平均成对差异可能被少数离群序列拉高。第三，native sequence recovery 与 novelty 存在天然张力，论文没有把 recovery 作为主指标，这是合理的选择，但也需要看辅助指标是否显示模型过度偏离天然分布。第四，部分增量很小且未标星（如 TS500 对 ProteinMPNN +0.004），统计显著性存疑。

九、负结果与失败模式
提供文本没有系统列出失败案例，例如哪些主链 scTM 低、哪些设计出现聚集/表达风险、哪些口袋设计虽然 pass 但配体结合模式改变。单链限制意味着多链界面和膜环境可能失败。RL 奖励依赖 ESMFold2，极端情况下可能生成“预测器喜欢但实验不喜欢”的序列。这些是阅读原文时应重点找的反例。

Q6: 有什么可以进一步探索的点？

一、扩展到多链、复合物与膜环境
作者在结论中明确把多链界面和膜环境列为下一步。具体可做：多链复合物的界面逆折叠、抗体-抗原对接后的序列设计、膜蛋白跨膜段的条件生成、以及考虑脂质环境的打分。难点是界面几何、链间耦合和膜嵌入约束，单链模型不能直接迁移。

二、湿实验闭环
目前证据主要是计算指标。下一步应对口袋设计、酶重设计和 de novo 设计做表达、稳定性、结合亲和力、催化活性和晶体/冷冻电镜结构验证；建立“计算 pass rate → 实验成功率”的校准曲线，才能判断 scTM 阈值的实际意义。

三、奖励模型与不确定性
RL 奖励主要来自 ESMFold2，未来可用预测器集成、不确定性惩罚、物理能量项或多任务奖励，降低 reward hacking。还可以研究 reward normalization、KL 系数、RLOO group size 对多样性和质量的影响，给出可复现的 scaling law。

四、采样器理论
draft-and-rewrite 的温度解耦、group 大小、重写轮数和 mask 粒度都值得系统消融。可以把它形式化为迭代条件采样或 Gibbs 采样，分析收敛性、混合时间和多样性-质量前沿；也可以探索自适应温度、按残基置信度选择 mask、以及和 speculative decoding 的更紧密结合。

五、多样性度量与文库设计
平均成对差异只是多样性的一种定义。未来应加入文库级覆盖度、聚类数、新颖性、天然同源距离、可开发性（表达、溶解度、稳定性、聚集倾向）和合成成本。对筛选而言，理想的指标是“给定实验预算下的命中率”，而不是单一 scTM。

六、模型规模与数据
1.96M 参数很小，值得研究 scaling：参数、训练数据、结构多样性、RL 样本量分别如何影响 scTM 和多样性。也可以把 HyInv 作为蒸馏目标，把大模型/大语言模型的结构知识迁移到小模型；或反过来用 HyInv 生成数据增强大模型。

七、更丰富的条件设计
除口袋、酶和 loop 外，可扩展到对称设计、金属结合位点、核酸结合、翻译后修饰、pH/温度依赖构象、别构位点和小分子诱导契合。条件注入方式（固定残基、距离约束、表面属性、配体原子）需要系统比较。

八、与 backbone 生成模型集成
逆折叠通常接在 RFdiffusion、ProteinGenerator 等 backbone 生成模型之后。未来可做端到端联合优化：backbone 生成时即考虑可设计性（designability），逆折叠时反馈 backbone 质量，形成闭环。

九、科研智能体的可复现性
Hyra 2.0 参与训练是本文的特色，但提供文本没有说明其自主程度。未来应报告智能体做了哪些决策、搜索了哪些配置、人类如何监督、失败实验如何记录；否则“agent 做科研”的主张难以复核。

十、评测标准化
建议建立匹配多样性的公开 leaderboard、跨预测器评估、按长度/难度分层、统计显著性检验和失败案例库。单链、单预测器、单温度的旧协议容易高估模型；HyInv 的协议可以作为起点，但需要社区共识。

Q7: 总结一下论文的主要内容

一、问题与动机
逆折叠是计算蛋白设计的核心任务：给定主链，设计能折叠回该主链的序列，服务于 scaffold 设计、酶工程和序列文库构建。论文强调实际筛选需要大量且多样化的候选，因此单条序列的最优性不是唯一目标，模型应在给定结构质量下提供尽可能高的多样性。既有 ProteinMPNN 类方法、语言模型辅助/并行解码方法和 ProteinZero 的 RL 路线各有优势，但单温度评估会混淆质量与多样性，且推理成本对大规模筛选仍然偏高。HyInv 由此把“匹配多样性下的 scTM”和“筛选级速度”作为两个主目标。

二、模型与训练
HyInv 是一个 1.96M 参数的主链条件图网络，编码器-解码器架构，通过消息传递编码主链几何环境，再逐位置输出氨基酸分布。训练由团队开发的科研智能体 Hyra 2.0 组织，分两阶段：第一阶段在天然结构和预测结构上做随机残基顺序的自回归序列建模，使模型适应任意解码顺序和任意 mask 组合；第二阶段用结构奖励做强化学习，对每个主链采样候选，用 ESMFold2 折叠，以 TM-align 得到的结构自一致性为奖励，用 RLOO 留一法基线降低方差，并加 KL anchor 约束策略。论文称奖励主要来自预测结构自一致性，训练池约 50,000（片段截断，具体含义需核对）。

三、推理与工程
推理采用分组 draft-and-rewrite 采样器：先以较高的 draft 温度 Td=0.6 生成草稿，铺开多样性；再以较低的 rewrite 温度 Tr=0.34 对一组位置做 mask 并重采样，其余位置保留为上下文，逐组迭代。这样把“探索”和“局部修正”解耦，在匹配多样性下提高 refolding TM-score。工程上，参考采样器对 402 残基主链生成 128 条序列约 2.6 s；跨候选共享工作后达到 8.7×（约 304 ms），且 FP32 路径能精确返回参考序列；进一步 BF16 优化达到 52×（50.6 ms），在单张 NVIDIA H20 上测量。

四、实验设计与结果
论文在五个面板上评估：CATH4.3 test（1,882 条链）、CATH4.2 test（1,120 条）、TS500（500 条）、TS50（50 条）和 RecentPDB（121 条链，111 个条目，26,766 个残基，长度 42–978）。主指标是 scTM，辅助指标是 native sequence recovery 和序列多样性。RecentPDB 额外用 Boltz2、Protenix2 折叠，二者不参与训练。十个外部基线包括 ProteinZero V2 (ESM2/ESM1)、ProteinMPNN 系列、LigandMPNN、PiFold、LM-Design 650M、CMLM、DPLM-2 3B。HyInv 在 CATH4.3/CATH4.2/TS500/TS50 的 scTM 为 0.840/0.855/0.899/0.920，RecentPDB 三种预测器下为 0.861/0.874/0.878，七列平均 0.875；相对 ProteinZero V2 (ESM2) 和 ProteinMPNN 的多数增量带星号。作者称在可比或更高多样性下平均 scTM 高于全部十个基线。

五、应用展示
同一默认设置支持四类任务：de novo 主链设计、口袋约束文库、保留催化位点的酶重设计、loop 重设计。口袋案例在十二个主链上的平均 pass rate 为 43%，pass 定义为 scTM ≥ 0.9 且配体 RMSD ≤ 1.0 Å；酶案例展示催化几何被保留；loop 案例展示局部重设计。新颖性分析提到 scTM ≥ 0.9、未检测到天然同源、与最近天然序列约 30% identity 的设计，试图证明模型不是简单复制天然序列。

六、边界与意义
论文明确限制在单链主链，多链界面和膜环境是下一步。评价主要依赖结构预测器的 scTM，训练奖励也依赖 ESMFold2，存在 predictor bias 和 reward hacking 风险；Boltz2/Protenix2 交叉评估只能部分缓解。提供的文本没有湿实验验证，method/discussion 为空，RL 超参数、数据池、训练成本和失败模式无法完整复核。其意义在于展示“小模型 + 随机顺序预训练 + 结构奖励 RL + 温度解耦采样 + 工程优化”可以在匹配多样性下取得强结果，并把推理速度推进到筛选可用；同时，Hyra 2.0 的参与使论文成为“agent 参与科研流程”的一个可讨论案例。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与画像中的 AI for Science 直接重合：逆折叠是计算蛋白设计的核心任务，且论文覆盖酶重设计、口袋设计等真实应用场景。

## 基本信息

- 作者：Haowei Lin, Cai Zhou, Xiangxin Zhou
- 机构：腾讯混元（Tencent Hunyuan）
- 来源：arxiv
- 主题/分类：cs.LG, q-bio.BM
- 日期：2026-11-09
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.11997v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先使用了 PDF 语义检索命中的 Introduction/Results/Conclusion 片段，并按 field_evidence_map 对齐字段；由于 method 与 discussion 为空、部分图注截断，涉及机制细节的判断已标注为合理推断或推测，具体数值与实验设置需回原文核对。
