---
user_id: "cheng tan"
paper_id: 6642
arxiv_id: "2608.03340v1"
title: "Benchmarking the Benchmarks: Testing the Predictive Validity of Commonsense Benchmarks"
publish_date: "2026-08-04"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.03340v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.03340v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-06T01:48:31"
---
# Benchmarking the Benchmarks: Testing the Predictive Validity of Commonsense Benchmarks

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：benchmark evaluation · criterion validity · commonsense reasoning · large language models

## 一句话总结

本文系统评估了四个常用常识基准及其重做变体对八个下游任务的预测效度，发现重做基准基本保持原始模型排名但不提升下游预测能力，且常识基准的跨家族预测效度仅局限于少数下游任务。

## 摘要

> Predicting LLM's capabilities on real-world tasks is essential, yet the extent to which performance on commonsense benchmarks predicts downstream performance remains underspecified. To establish the practical usability of widely adopted commonsense benchmarks, we evaluate 23 models from six families on four established commonsense benchmarks, four reworked variants, three non-commonsense controls, and eight downstream tasks requiring implicit social, pragmatic, temporal, or physical reasoning. We compare model rankings, compute controlled correlations, and use leave-one-family-out cross-validation to assess the criterion validity of commonsense benchmarks. Our results show that revised benchmarks largely preserve original model rankings and do not improve downstream predictive power. Commonsense benchmarks show consistent cross-family predictive validity for only a narrow subset of downstream tasks, with smaller or metric-specific gains elsewhere. Overall, standardized commonsense benchmarks provide task-dependent rather than broad evidence of downstream commonsense competence.

Q1: 这篇论文试图解决什么问题？

这篇论文试图回答一个核心问题：现有的标准化常识推理基准（如WinoGrande、HellaSwag、Social IQA、Physical IQA）在多大程度上能够预测LLM在下游真实世界任务上的表现？具体而言，论文关注三个层面：1）常识基准的排名是否与下游任务排名一致，即基准的预测效度（predictive validity / criterion validity）；2）对基准进行重做（reworked variants，可能指修正数据质量、混淆因子或标签问题）后，是否改善了其作为下游能力指标的效用；3）不同模型家族之间基准预测能力是否稳定，即跨家族泛化性。现有文献大多报告基准得分与下游性能存在粗略相关，但缺乏系统控制对比；也没有回答修复基准内部质量问题是否必然带来更强的下游预测力。论文试图用标准化评估协议和统计方法澄清这些问题，从而为基准设计者和模型开发者提供可操作的证据。

Q2: 有哪些相关研究？

相关研究可分为几类：1）常识推理基准的构建与批评，如WinoGrande、HellaSwag、Social IQA、Physical IQA等，这些基准常被用于衡量模型的社会、物理、时间等常识能力，但对其有效性已有质疑；2）评估范式转向，近期工作提出以交互式、任务导向或开放式评估替代静态选择题（如Truong et al., 2026; Kapoor et al., 2026; Ying et al., 2026; Momentè et al., 2025），认为静态基准存在饱和、捷径学习和领域狭窄等问题；3）能力关系的跨模型分析，近年研究通过跨模型相关性和预测比较来考察不同能力之间的关系（如Fan et al., 2026; Tsvilodub et al., 2026），但较少专门针对常识基准的准则效度；4）基准修复和去偏差研究，例如通过反事实扰动、对抗过滤等方法来去除数据集捷径，但这些修复是否提升下游预测能力缺乏检验。本文在上述背景下，首次系统地将常识基准视为预测下游能力的“测试”，并引入控制任务和留一家族交叉验证来分离基准特异性和家族特异性效应。

Q3: 论文如何解决这个问题？

论文采用了一种系统化的评估方法，可拆解为以下步骤：1）选定四个标准常识基准：WinoGrande（代词消解与常识）、HellaSwag（常识与情境补全）、Social IQA（社会常识）、Physical IQA（物理常识）；以及对应的四个重做变体、三个非常识控制任务（如语言建模、语法判断或句子排序等，具体细节需原文确认）和八个下游任务，涵盖社会、语用、时间、物理四类推理；2）评估23个来自六个模型家族的模型（家族可能包含不同规模或版本，具体清单需原文确认），所有任务统一采用基于对数似然的单选题评估设置，以保证跨任务可比性，避免生成式评估带来的噪声；3）统计分析上，计算模型排名的配对Spearman相关，并通过受控相关（partial correlations）控制其他因素，使用留一家族交叉验证来测试基准得分在未见过的模型家族上预测下游任务的稳定性；4）将下游表现视为基准的准则效度检验标准，但明确不假定任何单个下游任务定义常识推理，而是考察基准诱导的模型排名能否迁移到下游任务。这种设计强调预测效度而非内部一致性，并通过跨家族验证增强结论的泛化性。

Q4: 论文做了哪些实验？

论文设计了以下实验：1）模型覆盖：23个模型，来自六个模型家族（可能的家族包括GPT、Llama、Mistral等，具体需原文确认），涵盖不同规模和训练方式；2）评估任务集：四个标准常识基准（WinoGrande、HellaSwag、Social IQA、Physical IQA），四个重做变体，三个非常识控制任务，八个下游任务（涉及社会、语用、时间、物理推理）；3）统一评估协议：所有任务均使用对数似然（log-likelihood）的多选设置，以消除生成长度、解码策略等混淆；4）核心分析：a) 比较原始基准与重做变体的模型排名一致性；b) 计算基准得分与下游任务得分之间的Spearman相关；c) 在控制其他基准影响后计算偏相关；d) 使用留一家族交叉验证（LOO-CV）训练简单回归，预测每个下游任务得分，并检验预测误差；5) 可能还对比了非常识控制任务的预测力，以判断基准特异性。通过这些实验，论文试图分离“基准内容”与“模型能力”对预测的下游表现的贡献。

Q5: 发现了什么实验现象？

论文报告的主要实验现象包括：1）重做基准与原始基准在模型排名上高度一致，即重做变体基本没有改变模型间的相对排序；2）重做基准并未系统性地提升对下游任务的预测能力，反而在某些情况下与原始基准相当甚至更弱，这挑战了“修复数据质量必然改善基准效用”的直觉；3）常识基准的预测效度呈现出任务依赖性：对于少数下游任务，基准得分能够跨模型家族稳定预测，但大多时候预测力有限或仅体现在特定指标上；4）更有趣的是，事件/物理类基准（如Physical IQA）对属于同一概念轴（如物理推理）的下游任务预测力尤其强，说明基准与下游任务之间存在概念对齐效应，但标签层面的子领域划分往往与下游任务实际所需推理不一致——即基准名称所暗示的“社会”“物理”等子域与下游任务真正调用的常识类型经常错位；5）留一家族交叉验证的结果表明，跨家族泛化仅在特定任务上成立，说明基准得分不能作为通用能力代理；6）非常识控制任务的参与有助于判断哪些预测能力是常识特有的，但具体对比结果未在摘要中展开。整体上，负结果多于正结果，论文认为标准化常识基准提供的只是任务依赖的证据。

Q6: 有什么可以进一步探索的点？

基于本研究的发现，可以进一步探索：1）设计更接近真实使用场景的动态或交互式评估范式，替代静态多选基准，以提高下游表现的预测力；2）研究基准与下游任务的概念对齐机制，探索如何将基准分解为更细粒度的能力剖面（如社会、物理、时间、语用），并使用能力剖面而非总分进行预测；3）开发自动化的基准修复方法，并直接以预测效度而非内部一致性作为优化目标；4）扩展模型家族和下游任务覆盖，检验结论在更大规模模型和更广泛任务上的稳健性；5）研究不同评估设置（如生成式 vs. 判别式）对预测效度的影响，以及校准方法能否提升排名迁移性；6）探讨常识基准得分与下游任务之间是否存在中介变量（如模型规模、训练数据分布），从而解释预测效度的异质性；7）借鉴本研究的框架，将类似的准则效度检验应用于其他能力测评（如数学、代码、安全等），建立统一的能力验证方法论。

Q7: 总结一下论文的主要内容

本文以“基准的基准”（Benchmarking the Benchmarks）为视角，检验标准化常识基准对下游任务的预测效度，是评估领域一项系统性的实证研究。论文的论证主线是：常识基准被广泛用作LLM能力的代理指标，但其对现实世界任务的预测能力未被充分量化；因此作者引入准则效度（criterion validity）的概念，将下游任务表现作为基准有效性的判据。技术主线上，论文构建了一个多任务、多模型的评估矩阵：23个模型来自六个家族，覆盖四个标准常识基准、四个重做变体、三个非常识控制任务和八个下游任务；统一采用对数似然多选评估以减少任务间噪声，并通过Spearman相关、偏相关和留一家族交叉验证刻画预测关系。实验主线揭示了三个关键结果：第一，重做基准基本保持原始模型排名，意味着修复数据集内部质量问题并不会实质改变模型相对能力判断；第二，常识基准的跨家族预测效度是任务依赖的，仅在少数下游任务上稳定成立，其余情况增益有限或仅体现在特定度量上；第三，基准标签所暗示的常识子领域（社会、物理、时间等）与下游任务实际所需推理类型经常错位，而事件/物理类基准对同轴任务预测力较强，说明概念对齐而非表面标签决定了预测效度。论文最终主张，标准化常识基准提供的证据是任务依赖的，而非普遍性的常识能力证明，这为基准设计者和模型评测者提了个醒：依赖单一或少数基准总分来衡量模型常识能力可能造成误导。总体而言，本文的方法论贡献在于提出了一个可复用的预测效度检验框架，其实证发现则推动了对静态基准价值的重新审视。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于智能体（agent）方向，本研究的评估框架可直接用于检验智能体使用的工具或子模块在真实任务中的预测效度，避免仅依赖内部基准。

## 基本信息

- 作者：Ine Gevers, Walter Daelemans
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-04
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.03340v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的摘要、引言、相关工作、准则效度测试及结论等片段，并结合heuristic_draft进行了补充和校准；其中部分实验细节（如具体模型清单、重做变体构造）为依据摘要和片段的合理推断，需回原文确认。
