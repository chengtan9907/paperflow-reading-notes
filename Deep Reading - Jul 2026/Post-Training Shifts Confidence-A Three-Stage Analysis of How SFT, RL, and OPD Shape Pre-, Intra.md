---
user_id: "cheng tan"
paper_id: 4062
arxiv_id: "2607.13753"
title: "Post-Training Shifts Confidence: A Three-Stage Analysis of How SFT, RL, and OPD Shape Pre-, Intra-, and Post-CoT Calibration"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.13753.pdf"
pdf_url: "https://arxiv.org/pdf/2607.13753"
abs_url: "https://arxiv.org/abs/2607.13753"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-17T01:00:47"
---
# Post-Training Shifts Confidence: A Three-Stage Analysis of How SFT, RL, and OPD Shape Pre-, Intra-, and Post-CoT Calibration

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · confidence calibration · chain-of-thought · supervised fine-tuning

## 一句话总结

本文通过三阶段校准框架（Pre-CoT、Intra-CoT、Post-CoT）系统分析监督微调（SFT）、强化学习（RL）和在线策略蒸馏（OPD）对数学推理置信度的影响，发现不同方法在不同阶段具有不同优势，并基于位置感知置信度提出PosConf策略，显著提升答案聚合和早停性能。

## 摘要

> Large language models have made strong reasoning gains through supervised fine-tuning, reinforcement learning, and on-policy distillation, yet these post-training methods are usually evaluated only by final-answer accuracy. We study how they reshape confidence during reasoning. We introduce a three-stage calibration framework that evaluates confidence before, during, and after chain-of-thought generation, corresponding to difficulty estimation, early termination, and answer aggregation. Through a controlled comparison on mathematical reasoning benchmarks, we find that OPD provides the most useful pre-reasoning confidence, SFT gives the strongest online signal for early stopping, and RL produces the most reliable trace-level signal for aggregation. We further show that confidence reliability is position-dependent: RL confidence becomes informative after a path-commitment phase, while OPD confidence is useful early but can become inversely calibrated later. Based on this observation, we propose PosConf, a position-aware confidence strategy that uses confidence only from reliable relative-position intervals. PosConf improves RL answer aggregation by 6.1 points over majority voting and consistently improves OPD early stopping under tight token budgets, with gains up to 4.3 points by avoiding its later inverse-calibration region, showing that confidence in reasoning models should be used both stage-wise and position-awarely. Our code is available at https://github.com/EIT-NLP/Post-Training-Calibration.

Q1: 这篇论文试图解决什么问题？

虽然大型语言模型通过监督微调（SFT）、强化学习（RL）和在线策略蒸馏（OPD）等后训练方法在数学推理上取得显著进展，但现有评估几乎完全依赖最终答案准确性，忽略了推理过程中的置信度信息。这些置信度对于任务难度估计、早期停止和答案聚合等下游应用至关重要，然而不同后训练方法如何影响推理各阶段（推理前、中、后）的校准特性尚不清楚。具体问题包括：（1）SFT、RL、OPD产生的置信度在Pre-CoT（难度估计）、Intra-CoT（早停）、Post-CoT（答案聚合）阶段各有什么校准质量？（2）置信度是否随推理位置（相对步数）发生变化？（3）如何利用阶段和位置属性设计更好的置信度利用策略？论文旨在填补这一空白，为后训练方法的选择和推理置信度的使用提供指导。

Q2: 有哪些相关研究？

相关研究分为三条主线：其一，大型语言模型在数学和逻辑推理上的进展（OpenAI et al., 2024; Lewkowycz et al., 2022; Yang et al., 2024），主要聚焦最终答案准确率；其二，后训练方法本身，包括监督微调（SFT）、强化学习（例如PPO，Schulman et al., 2017）和在线策略蒸馏（OPD，从RL策略中蒸馏），这些方法在推理能力提升中扮演关键角色，但很少被从置信度校准角度比较；其三，LLM置信度校准研究（例如基于logit或verbalized confidence），但现有工作多针对分类或问答任务，未系统分析推理链条中的时变置信度。论文首次将三阶段校准框架与后训练方法结合，揭示了不同方法在推理各阶段的独特置信度模式。

Q3: 论文如何解决这个问题？

论文提出三阶段校准框架：（1）Pre-CoT阶段：根据问题输入后模型第一个token或隐状态的置信度进行难度预估；（2）Intra-CoT阶段：在生成推理链期间，使用基于top-k下一token概率的滑动窗口平均作为在线置信度，当置信度低于由预热轨迹导出的阈值时触发早停；（3）Post-CoT阶段：对完整推理轨迹计算总体置信度（如平均token置信度或最终答案token概率），用于答案聚合（选择最高置信度轨迹或过滤低置信度轨迹）。基于控制比较实验，发现置信度可靠性随推理位置变化：RL置信度在路径承诺阶段（推理链后半部分）后变得更加信息量，而OPD置信度早期有用但后期可能呈现反向校准。据此设计位置感知置信度策略（PosConf），仅使用可靠相对位置区间内的置信度进行决策——对RL，只在后期置信度用于聚合；对OPD，仅利用早期置信度进行早停，避开反向校准区域。

Q4: 论文做了哪些实验？

实验以Qwen2.5-7B-Instruct为基础模型，分别进行SFT、RL（使用PPO）和OPD训练，在多个数学推理基准（如GSM8K、MATH等标准测试集）上进行评估。实验包括：（1）Pre-CoT校准评估：比较各方法下初始置信度与问题难度（正确率）之间的相关性；（2）Intra-CoT早停效率：在固定token预算下，比较不同方法使用早停策略时的准确率与推理成本权衡；（3）Post-CoT答案聚合：使用置信度过滤（保留高置信度轨迹）或置信度加权投票，比较多数投票与基于置信度的方法的性能；（4）位置敏感性分析：按相对位置切片（如前20%、中间、后40%等）分析置信度与正确性的关系；（5）PosConf消融：验证在可靠区间内使用置信度的效果，并与整体使用置信度、无置信度基线对比。所有实验重复多次以确保显著性。

Q5: 发现了什么实验现象？

（1）OPD在Pre-CoT阶段表现最优：其初始置信度与问题难度相关系数最高，可用于准确预估任务难度。（2）SFT在Intra-CoT阶段提供最强在线信号：其token级置信度能最可靠地指示推理正确性，早停策略在同等token预算下达到最高准确率。（3）RL在Post-CoT阶段最具区分力：轨迹级置信度能有效分离正确和错误推理链，置信度过滤比多数投票提升6.1个百分点。（4）置信度位置依赖性显著：RL置信度在推理链后半段（路径承诺后）才变得有信息量，早期置信度几乎随机；OPD置信度在前半段非常可靠，但在后半段出现反转——正确轨迹的置信度反而低于错误轨迹。（5）PosConf策略有效：对于RL，仅在后期使用置信度聚合进一步提升性能（相对多数投票+6.1%）；对于OPD，避免后期反向区域后，早停在严格token预算下获得高达4.3个百分点的额外提升。（6）整体上，不同后训练方法不能一概而论，其置信度用途必须分阶段、分位置考虑。

Q6: 有什么可以进一步探索的点？

（1）扩展模型规模：当前实验基于7B模型，更大模型（如70B、MoE）是否保持相同的阶段和位置校准模式需验证。（2）迁移至其它推理任务：数学推理相对结构化，代码生成、定理证明、逻辑推演等任务可能呈现不同的置信度动态。（3）后训练方法多样性：除SFT、RL、OPD外，还应考察DPO、GRPO、拒绝采样蒸馏等方法的校准特性。（4）集成利用：结合多种后训练方法的置信度优势，设计混合策略。（5）动态位置感知：当前PosConf基于静态区间，未来可自适应学习每个模型实例的最佳置信度窗口。（6）与其他推理增强技术结合：如自我一致性、beams搜索、verify模型等，置信度可作为附加信号。

Q7: 总结一下论文的主要内容

本文系统研究了监督微调（SFT）、强化学习（RL）和在线策略蒸馏（OPD）三种主流后训练方法对大型语言模型推理置信度的影响。作者指出，先前评估仅关注最终答案准确性，忽略了推理过程中置信度在难度估计、早停和答案聚合等方面的关键作用。为此，论文提出三阶段校准框架：Pre-CoT（推理前，基于初始token概率预估难度）、Intra-CoT（推理中，基于滑动窗口平均置信度决定早停）和Post-CoT（推理后，基于轨迹置信度进行答案聚合）。通过在数学推理基准上的控制实验（基于Qwen2.5-7B-Instruct），作者发现各后训练方法在不同阶段表现出截然不同的校准优势：OPD提供最可靠的第一阶段信号，SFT在第二阶段（在线早停）中表现最佳，而RL在第三阶段（聚合）中最具鉴别力。进一步分析揭示置信度的位置依赖性——RL置信度在推理链路径承诺期后才具有信息量，而OPD置信度早期有用但后期可能反向校准。基于这一发现，作者提出位置感知置信度策略（PosConf），仅从可靠相对位置区间提取置信度。PosConf在RL聚合任务上超越多数投票6.1个百分点，在OPD早停任务上在严格token预算下再获4.3个百分点提升。论文强调了后训练方法选择应结合推理阶段和位置特性，为高效推理系统中的置信度利用提供了系统性指导。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文主题紧密契合生成（generation）方向：LLM推理置信度校准直接影响生成质量和效率。

## 基本信息

- 作者：Shuhao Li, Guodong Du, Anhao Zhao, Wanyu Lin, Tianyu Yuan, Xiaoyu Shen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.13753`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本文档基于PDF语义检索证据和heuristic_draft生成，其中主要引用了abstract、introduction、discussion、limitations等片段的检索结果。
