---
user_id: "cheng tan"
paper_id: 7660
arxiv_id: "2608.11829v1"
title: "Towards Understanding On-Policy Distillation through the Lens of Test-Time Scaling"
publish_date: "2026-08-12"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.11829v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.11829v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-14T11:53:10"
---
# Towards Understanding On-Policy Distillation through the Lens of Test-Time Scaling

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · test-time scaling · pass@k · sampling efficiency

## 一句话总结

该论文通过测试时扩展（test-time scaling）视角重新评估策略蒸馏（on-policy distillation, OPD），发现OPD主要提升采样效率而非真正扩展学生的推理能力边界，其收益更接近一种“幻象蒸馏”。

## 摘要

> On-policy distillation (OPD) has emerged as a promising post-training technique for enhancing LLM reasoning. It is commonly believed to enable the student model to distill knowledge from a stronger teacher model, thereby expanding capabilities beyond the pre-OPD base model. In this study, we examine this view through the lens of test-time scaling by varying the sampling budget K and evaluating performance with pass@K and avg@K. Specifically, across several OPD variants, we observe that OPD-trained models maintain superior avg@K performance across sampling budgets, while the advantage in pass@K gradually shifts to the pre-OPD base models as K increases. These results suggest that OPD primarily improves sampling efficiency rather than consistently expanding the student's reasoning capability boundary. The pass@K dynamics throughout OPD training further reveal a progressive shift toward stronger small-K performance at the expense of the large-K capability boundary. Furthermore, a problem-level solvability analysis using pass@1024 as the criterion reveals an asymmetry: OPD causes more previously solvable problems to become unsolvable than previously unsolvable problems to become solvable. Together, these findings suggest that, from the perspective of capability expansion, OPD behaves more like an "illusory distillation": its apparent gains arise primarily from improved sampling efficiency rather than from acquiring genuinely new reasoning capabilities from the teacher.

Q1: 这篇论文试图解决什么问题？

1. **常见假设下的矛盾**：主流的LLM推理后训练研究中，OPD被广泛认为是一种有效的知识蒸馏方式，其隐含假设是学生能从更强的教师模型中提取出超越自身的推理能力，从而扩展能力边界。该假设支撑了GKD、MiniLLM等一系列工作。
2. **现有评估协议的盲区**：多数评估采用单次采样或固定采样规模的基准分数（如固定温度下的准确率），无法区分“模型本身具备解决某个问题的能力”与“模型在多次采样中恰好碰出正确答案”这两种截然不同的情况。这种评估方式可能高估OPD的能力扩展效果。
3. **近期不稳定报告的暗示**：近年已有研究（Li et al., 2026; Zhu et al., 2026，检索片段提及）报告OPD在某些情况下并不如预期稳定或有效，但缺乏系统性的视角解释这些现象。
4. **核心研究问题**：本文试图回答——OPD是否真的扩展了学生模型的推理能力边界？还是仅仅改变了输出分布，使其在有限采样预算下更容易命中正确答案（即采样效率提升）？
5. **测试时扩展的切入点**：论文将推理能力边界操作化为“在足够大的采样预算下能否达到正确答案”（pass@大K），将采样效率操作化为“在较小采样预算下的命中率”（pass@小K 与 avg@K）。通过对比大K与小K的表现，可以分离两种效应。
6. **更深层的不对称性问题**：即使OPD总体上提升了平均表现，能力扩展的受益者是谁？是否存在“某个子集问题被解决”但“另一个更大的子集问题被丢失”的权衡？论文通过问题级可解性分析揭示这种不对称性。

Q2: 有哪些相关研究？

1. **策略蒸馏（OPD）方法**：GKD（Agarwal et al.）和MiniLLM通过在学生生成输出上查询教师反馈来减少自回归蒸馏中的训练-测试分布不匹配，MiniLLM强调反向KL优化。这是OPD的代表性工作，也是本文分析的对象之一。
2. **进一步的OPD改进**：后续工作（Xing et al., 2026等）致力于提升OPD的稳定性和性能，或将其推广到更广泛场景。这些工作通常仍基于“蒸馏即能力迁移”的默认假设。
3. **对OPD的质疑**：近期一些研究（Li et al., 2026; Zhu et al., 2026）报告了OPD在某些情况下表现不佳的案例，但对失败模式缺乏统一解释。本文将其归结为“采样效率与能力边界”的混淆。
4. **Off-policy蒸馏相关**：与OPD相对的是off-policy蒸馏，例如DeepSeek-R1-Distill系列，通过在大规模离线数据上进行监督微调实现蒸馏。本文在对比实验中将两类蒸馏放在同一测试时扩展框架下考察，用于检验“能力扩展”的差异性。
5. **测试时扩展（Test-Time Scaling）与推理时计算**：近年来大量工作研究通过采样预算、思维链长度、蒙特卡洛搜索等提升LLM推理表现，但很少将其作为评估后训练方法的诊断工具。本文将测试时扩展定义为采样预算K的变化，并引入pass@K与avg@K两个指标，为后训练方法提供了新的评估维度。
6. **能力边界的度量方法**：先前评估能力边界常用pass@K（如代码生成领域的pass@k指标）或覆盖率指标，但本工作将其系统应用于蒸馏效果的可解性分析，并引入pass@1024作为“可解”的操作定义。
7. **知识蒸馏与推理能力的关系**：已有工作探讨蒸馏中“能力”与“风格”的分离，本文进一步从能力边界角度指出OPD可能只学到了“快速出正确结果”的风格，而非问题求解的底层能力。

Q3: 论文如何解决这个问题？

本文并未提出新的训练算法，而是提出一套系统的分析框架来重新解读OPD的效果，具体方法如下：
1. **测试时扩展视角**：将推理时采样预算K作为自变量，评估两种互补指标——pass@K（在K个采样中至少一次正确的比例，反映能力边界）和avg@K（K次采样平均正确率，反映采样效率与分布质量）。
2. **多个OPD变体的系统性比较**：在多种OPD设置（推测包括GKD、MiniLLM等）下，将OPD训练后的模型与其对应的pre-OPD基础模型在相同采样预算下进行比较。
3. **训练过程中的动态追踪**：记录OPD训练不同阶段的pass@K曲线，观察小K和大K表现的变化趋势。这可以揭示训练过程是持续扩展边界，还是仅在重分配采样分布。
4. **问题级可解性分析**：以pass@1024作为“可解”的判定标准（即1024次采样中至少一次正确则认为该问题可解），统计OPD训练前后每个问题可解性的变化，区分四类：保持可解、变为可解、保持不可解、变为不可解。这能刻画能力边界的转移。
5. **与Off-policy蒸馏的对照**：对DeepSeek-R1-Distill等off-policy蒸馏模型执行同样的测试时扩展分析，以验证“采样效率提升而非能力扩展”这一现象是否OPD特有，还是蒸馏方法的普遍特征。
6. **结论性解释**：综合以上证据，将OPD的表现解释为采样效率的改善（提高正确推理路径的概率质量）而非推理能力边界的拓展，并将此现象命名为“幻象蒸馏”。

Q4: 论文做了哪些实验？

根据摘要与检索片段，本文实验设计大致包括（因未获得完整PDF，以下部分内容为合理推断，具体数值需核对原论文）：
1. **主要评估协议**：对多个OPD变体（推测包含GKD、MiniLLM等）训练后的模型与其基础模型，在多个推理基准上，通过变化采样预算K（从小到大，可能包括1、2、4、8、16、32等）计算pass@K和avg@K，绘制性能-采样预算曲线。
2. **训练动态实验**：在OPD训练的不同检查点（checkpoint）重复上述pass@K测量，观察小K和大K优势的转移过程。
3. **问题级可解性分析**：设定K=1024，对测试集每个问题判断opd前/后的可解性，统计状态转换矩阵，从而量化“提升”与“丢失”。
4. **Off-policy对照实验**：对DeepSeek-R1-Distill-Qwen-1.5B/7B（由Qwen-Math基础模型离线微调得到）进行相同的K-scaling分析，与OPD结果进行对比（见检索片段3.4节）。
5. **可能的实验规模**：模型尺寸推测在1.5B/7B量级，因为off-policy对照使用的是该尺寸（检索片段）；OPD变体可能也在类似尺寸上进行。但具体数据集、基准、OPD变体集合均未在片段中完全明确，需要查阅原文。

Q5: 发现了什么实验现象？

1. **avg@K优势稳定存在**：在所有测试的采样预算下，OPD训练模型相对pre-OPD基础模型的平均正确率（avg@K）都更高。这说明OPD确实将概率质量集中到了正确答案路径上。
2. **pass@K优势随K增大而反转**：在较小的K下，OPD模型的pass@K优于基础模型；但随着K增大，优势逐渐缩小并最终反转，即基础模型在足够大的K下能达到更高的累计通过率。这是核心反直觉现象——学生在大量采样时反而“天花板更低”，说明学生并未获得教师更广的解题能力。
3. **训练动态向小K倾斜**：随着OPD训练进行，模型对较小采样预算的pass@K持续改善，而对较大K的pass@K却逐步下降。这暗示训练过程在系统地重新分配概率质量，而非单纯添加新能力。
4. **问题级可解性的不对称**：以pass@1024为标准，OPD使一些原本不可解的问题变得可解，但同时使更多原本可解的问题变成不可解（“净丢失”）。即OPD在“修复”部分短板的同时更多丢失了长尾能力，边界向小K区域收缩。
5. **与Off-policy蒸馏的对比差异**：初步结果显示off-policy蒸馏同样存在类似现象（检索片段提及“Further evidence”），但具体趋势是否与OPD完全一致，还是程度上有所不同，片段信息不足，需查原文。
6. **整体结论**：所有观测共同指向：OPD更像一种采样分布“锐化”技术，提高了单位采样预算的效率，但没有扩大模型可解问题的集合，故称之为“幻象蒸馏”。

Q6: 有什么可以进一步探索的点？

1. **定义更精细的能力边界指标**：pass@1024仍是一个截断值，构造pass@∞的外推估计或采用问题难度曲线，可以更精确刻画边界位移。
2. **理解“幻象蒸馏”的机制**：为什么OPD会牺牲大K能力？是否因为训练迫使学生模仿教师的短路径/低熵输出，导致熵坍缩，丢失对困难问题的探索多样性？需要理论分析和内部表示研究。
3. **改进OPD训练目标**：设计对抗采样效率-能力边界权衡的正则项，例如在反向KL中加入熵惩罚或混合off-policy数据，以在提升小K表现的同时保持大K边界。
4. **测试时扩展与后训练的联合设计**：将本文的诊断框架用于选择超参数（如温度、采样策略）或后训练算法，使模型在任意预算下都达到最优。
5. **问题难度分层分析**：按问题难度（如推理步骤数、领域）细分可解性转换，识别OPD容易丢失哪些类型的问题，并针对性地构造数据集。
6. **跨方法、跨规模系统性复制**：在更多OPD变体、更多教师/学生组合（不同规模、不同基础模型）、更多基准上验证该现象，探索是否随规模或数据变化而消失。
7. **与强化学习正面对比**：比较RLHF/GRPO等直接优化策略的方法是否也存在类似“采样效率vs能力边界”的权衡，以及蒸馏与RL的交互效应。
8. **理论解释**：从最优传输、信息论或隐变量模型角度解释蒸馏与采样效率的关系，给出为什么反向KL或前向KL会导致不同的边界偏移。
9. **实际应用建议**：如果幻象蒸馏成立，那么依赖OPD后模型进行高预算推理（如复杂测试时搜索）的系统部署方需要重新评估；给出何时应该使用OPD、何时保持基础模型的指导原则。

Q7: 总结一下论文的主要内容

该论文对策略蒸馏（on-policy distillation, OPD）提供了一次系统的行为学诊断，其核心论点是：OPD在LLM推理任务上的明显收益主要来源于采样效率的提升，而非真正扩展了学生的推理能力边界，因此本质上是一种“幻象蒸馏”（illusory distillation）。

**论证主线**：论文从当前对OPD的主流认知出发——即学生模型通过OPD能够从更强的教师模型中蒸馏出新的知识，从而获得超出基础模型的能力。作者认为这一认知可能源于评估协议的模糊性：大多数基准只报告单一采样次数下的准确率，无法区分“模型确实会解”和“模型碰巧多次采样中一次正确”。为解耦这两者，作者引入测试时扩展视角：将采样预算K视作连续变量，采用pass@K（K次采样中至少一次正确，反映能力边界）与avg@K（K次平均正确率，反映整体采样质量）两个指标。核心假设是：如果OPD真正扩展了能力边界，那么其pass@K优势应当随K增大而保持或增强；如果只是提升采样效率，那么优势应集中在较小K，并且在足够大的K下，基础模型的pass@K将反超。

**技术主线**：论文并未提出新算法，而是构建了一套分析工具套件：
1. 对多个OPD变体（如GKD、MiniLLM）与其pre-OPD基础模型进行系统的pass@K和avg@K扫描；
2. 跟踪OPD训练过程中的pass@K曲线，观察小K和大K指标的动态Trade-off；
3. 采用pass@1024作为“可解性”的操作性定义，进行问题级状态转换分析，将能力变化拆解为“不可解→可解”的增益与“可解→不可解”的损失；
4. 以off-policy蒸馏（DeepSeek-R1-Distill系列）作为对照，将同一测试时扩展框架应用于非OPD场景，以判断现象是否具有普遍性。

**实验主线**：实验覆盖多个OPD设置，均观察到一致模式：avg@K在全部K范围保持优势；pass@K从小K时的正收益逐步过渡为大K时的负收益；训练动态表明模型在逐步牺牲大K边界以换取小K表现；问题级分析显示出不对称性——失去的可解问题多于获得的可解问题。这些证据共同支持“采样效率提升而非边界扩展”的结论。与off-policy蒸馏的对比进一步表明类似趋势也存在于off-policy场景，这暗示“采样效率优先”可能是当前蒸馏协议的通病，而非OPD独有。

**结论与影响**：论文将这种表面蒸馏定义为“幻象蒸馏”，提醒研究者和工程师在评估后训练方法时需要区分“快速找到正确答案”与“能解决更多问题”。该工作对OPD的潜在适用场景提出了边界：在采样预算受限的部署中OPD可能确有益处；但在高预算搜素或需要稳定覆盖困难问题的场景下，基础模型可能表现更好。同时，论文也为后续算法设计提供了可量化的评估标准，即同时报告多种K下的pass@K而非单点分数。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：如果你的工作涉及智能体或生成任务中的蒸馏/模仿学习，本文提出的“采样效率vs能力边界”分析框架可以直接迁移到智能体策略蒸馏的评估中，避免被单点基准分数误导。

## 基本信息

- 作者：Xinmu Ge, Zizhuo Zhang, Yu Huang, Jianing Zhu, Lin Yuan, Wanli Gu, Weichang Wu, Weiran Huang, Xiaolu Zhang, Bo Han, Jun Zhou, Jiangchao Yao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-08-12
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.11829v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了提供的摘要、检索证据片段与heuristic_draft，结合合理推断补全；因未获得完整PDF，部分实验细节标注为推断，建议以原文为准。
