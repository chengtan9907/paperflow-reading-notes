---
user_id: "cheng tan"
paper_id: 15138
arxiv_id: "2610.11736v1"
title: "Towards Unified Evaluation of Prompt Enhancers for Video Generation"
institution: "论文元数据未提供机构字段，依据作者名单可合理推断部分作者来自中国科学技术大学（如Xueyang Fu、Yang Cao、Wei Zhai、Zheng-Jun Zha等），Yu Liu、Sheng Zhong、Wei Tong等作者可能来自华为诺亚方舟实验室/科大讯飞等工业机构，且被评测系统横跨多家闭源生态（Wan、MiniMax、LTX等）；但完整单位归属需回原文首页确认。"
publish_date: "2026-10-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.11736v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.11736v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T12:40:55"
---
# Towards Unified Evaluation of Prompt Enhancers for Video Generation

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：prompt enhancement · video generation · benchmark · evaluation framework

## 一句话总结

本文提出PEBench，首个覆盖文本到视频、图像到视频和参考到视频三类提示增强任务的统一直接评估基准，以1,100个专家核验用例和24条基于证据的评估准则实现无需渲染视频的提示增强器评测，并通过人类验证证明提示级评分能够可靠反映下游视频生成效用。

## 摘要

> Modern video generators can realize increasingly complex visual narratives, positioning the prompt enhancer (PE) as a critical bridge from concise user instructions and multimodal references to structured cinematic plans. However, existing PE evaluation relies on rendered videos, imposing substantial computational and human costs, slowing PE training and iteration, and conflating PE quality with downstream generator behavior. To address this gap, we introduce PEBench, the first unified benchmark for direct PE evaluation across text-to-video, image-to-video, and reference-to-video prompt enhancement. It comprises 1,100 expert-verified cases and 1,005 visual assets, spanning 35 fine-grained tasks with diverse temporal, cinematic, audiovisual, and multi-reference requirements. In addition, we develop PEBench evaluation, an evidence-grounded framework that combines modality-aware fact extraction with rubric-based assessment across 24 criteria. Our systematic evaluation of representative open- and closed-source PE methods reveals an emerging shift from fine-grained descriptive expansion toward intent-preserving cinematic planning, while the caption-reconstruction and forward-refinement methods show complementary strengths in cinematic coverage and semantic fidelity or internal coherence, respectively. Human validation shows that PEBench scores align closely with expert judgments of enhanced prompts and downstream videos from Wan3.0 and MiniMax-H3, indicating that prompt-level evaluation reliably reflects downstream utility.

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：提示增强器（PE）缺乏统一的直接评估手段，导致PE研究、训练和迭代缺乏可靠信号。具体包含以下相互关联的子问题：
1. 评估对象错位：现有视频生成基准（如T2V质量与提示对齐类、参考条件类、多镜头、音视频、流式生成类）全部以渲染视频为评估对象，而不是增强后的提示本身。要在这些基准上评估PE，必须额外执行一次视频生成，把PE质量与下游生成器行为纠缠在一起，无法归因。
2. 评估成本瓶颈：渲染视频再做人工或自动评估，计算与人力成本高、时延长，直接拖慢PE的迭代速度和训练反馈回路，也阻碍对不同PE方法的公平比较。
3. 任务覆盖不统一：PE实际面对的输入条件差异很大——纯文本指令、首帧图像、参考视频或多张参考图——现有工作没有在同一协议下覆盖T2V-PE、I2V-PE、R2V-PE三个任务，无法回答跨任务能力画像问题。
4. 缺少细粒度诊断能力：现有评估多给出整体分数，无法指出PE在时间规划、机位设计、叙事发展、视听协调等具体维度上的缺陷；论文主张需要准则级（criterion-level）的细粒度评估。
5. 隐含假设的检验：直接评估提示质量是否真的能反映下游视频效用？论文将这一隐含假设显式化，并用人类专家对增强提示和下游视频的双层判断加以验证。
6. 方法范式竞争未决：已有PE方法机制差异巨大（监督训练、偏好优化、检索改写、agentic验证修正、字幕重建、格式映射），但缺少在同一基准下的横向对比，难以判断哪种机制真正有效、哪种只是堆砌长度。论文通过10个系统的统一评测回应此问题。

Q2: 有哪些相关研究？

论文的相关工作梳理可以按两条主线展开：
1. 提示增强方法谱系（PE methods）。现有PE方法大体分为训练式与推理式两类。训练式方法学习从用户意图到详细提示的重写：(a) VPO基于原则化SFT与多反馈偏好优化来训练提示优化器，内部一致性表现突出；(b) Prompt-A-Video（含Cog与OS两个变体）用监督与偏好目标优化LLM重写器；(c) WanPE（397B与35B两个规模）通过压缩提示到详细视频字幕的视频反演对（video-grounded inverse pairs）重建字幕，属于字幕重建（caption-reconstruction）范式；(d) LingBot-Video-PE把扩展后的提示映射为模型特定的结构化字幕，属于强格式约束路线。推理式方法不训练或少训练：(e) RAPO通过检索相关示例进行重写；(f) SCMaPR执行句子重写、原子级语义验证与条件修订，是agentic验证修正范式；(g) LTX-2.5-PE直接利用其集成的Gemma 4文本编码器重写提示；(h) H3-Context-IR采用多阶段多模态工作流，后续实验显示其在视觉条件任务上的明显优势。论文将这些方法细分为字幕重建、前向精炼、agentic工作流、格式映射、检索示例等范式，为解释实验结果提供机制性框架。
2. 视频生成与提示评估基准的演进。视频生成基准从早期T2V质量与文本对齐评估，扩展到参考条件、多镜头、音频-视频、流式生成等设定。但这些基准的评估对象都是渲染视频而非增强提示，无法直接评估PE。把这类基准用于PE评估需要额外生成步骤，不仅增加成本和延迟，还把PE性能与下游生成器行为耦合。论文正是针对这一缺口定位PEBench：它是第一个把评估对象直接设为增强提示本身、并在同一协议下覆盖T2V/I2V/R2V三类任务的统一基准。
3. 评估范式竞争。论文隐含支持从渲染后评估到提示级直接评估的范式转移，主张直接评估具备生成器无关、成本低、可迭代等优势，并用人类验证说明直接评估与下游效用的一致性。相关检索证据还显示任务设计考虑了不可行请求、歧义指令、冗余资产等实际工程问题，这使PEBench更贴近真实使用场景。

Q3: 论文如何解决这个问题？

论文提出的解决方案由三个相互支撑的模块组成：PEBench基准数据、PEBench evaluation评估框架、以及配套的评测协议与分析方法。
1. PEBench数据构建：包含1,100个专家核验用例与1,005个视觉资产，覆盖35类细粒度任务。任务维度设计刻意涵盖时间结构（单镜头到多镜头）、电影化表达（机位、景别、叙事节奏）、视听协调（音效、音乐、对白、画面关系）和多参考需求（多图、多对象、参考视频）。从检索证据看，任务构造还包含了不可行请求、歧义指令、冗余资产等真实退化场景，以及五元组组合（Genre+Subject+Scene+Style+Audio）、文本引导参考提示等复合设定。视觉资产的构建可能借助Seedream、ImageGPT等图像生成模型（证据中的相关字样支持这一推测，但论文正文的完整生成管线需回原文确认）。
2. 三种任务定义：(a) T2V-PE：给定用户提示、无参考资产（R=∅），PE应产出忠实扩展请求视频内容的详细提示；(b) I2V-PE：给定用户提示与首帧图像（R={I0}），PE生成包含首帧条件的增强提示；(c) R2V-PE：给定用户提示与多模态参考资产（参考视频、参考图像等），PE产出融合参考内容的增强提示。
3. PEBench evaluation框架：核心是证据导向的两段式评估。第一段做模态感知事实提取（modality-aware fact extraction），从增强提示和参考资产中抽取可核验的事实，作为后续打分的证据；第二段基于24条准则的rubric做准则级判定，每条准则判定对应问题是否存在。24条准则组织为三个维度：语义保真（Semantic Fidelity，表2中可见GSF、SAP、ANF、DLI、ARP、CCP、LCF、SSP、OTI九条）、内部一致性（Internal Consistency，含SAC、ANL、TSC、SPC、PL、AVS六条）、导演式创作（Directorial Creation，表2该维度的准则列在检索片段中被截断，但根据论文正文可知包含Storyboard and Timing Completeness（STC）、Camera Design Specificity（CDS）等准则）。评分聚合分两层：准则级通过率（无对应问题的用例百分比）与Overall分数（按每个case内问题的数量与严重性聚合为维度总分）。这种设计使评估既可细粒度定位问题，又可整体排名。
4. 评测协议与分析方法：对10个PE系统在T2V-PE上全部评估，多模态能力系统额外评估I2V-PE与R2V-PE；同时设计原提示no-enhancement基线、输出长度分层分析、时间结构与参考复杂度分组分析（涉及Structure Count、Event stages、Reference Group、Images等变量），以及人类专家对增强提示和下游视频（Wan3.0、MiniMax-H3）的双层验证。整体方案强调生成器无关、无需渲染、准则可解释、跨任务可比。

Q4: 论文做了哪些实验？

论文的实验设计围绕验证三类问题展开：PE方法横向对比、评估框架可靠性、以及影响PE表现的复杂度因素。
1. 评测系统集合（10个）：H3-Context-IR（多阶段多模态工作流，闭源）、WanPE-397B与WanPE-35B（字幕重建）、LTX-2.5-PE（内置文本编码器直写）、LingBot-Video-PE（模型特定结构化字幕映射）、SCMaPR（前向精炼+原子级语义验证+条件修订）、VPO（原则化SFT+多反馈偏好优化）、Prompt-A-Video-Cog与Prompt-A-Video-OS（监督+偏好训练的两个变体）、RAPO（检索示例重写）。覆盖训练式与推理式、开源与闭源、直接重写与结构化规划等主要范式。
2. 任务覆盖：全部10个系统在T2V-PE上评估；具备多模态能力的系统（H3-Context-IR、LTX-2.5-PE、LingBot-Video-PE等）额外在I2V-PE与R2V-PE上评估。
3. 主实验（表2）：报告各系统在语义保真、内部一致性、导演式创作三个维度上的准则级通过率与Overall分数。
4. 原提示基线：把未经增强的用户原始提示作为no-enhancement对照，量化提示增强的必要性；结果显示原提示在T2V-PE上语义保真达100.00，但导演式创作仅33.56。
5. 长度分层分析：按输出长度分四分位比较各维度分数，检验长提示是否必然更好；结果显示最长输出四分位比最短四分位在跨任务上低5.19–10.09分（详见附录C.2）。
6. 复杂度影响分析：使用H3-Context-IR作为探针方法，考查时间结构复杂性（Event stages数量、Structure Count）与参考复杂性（Reference Group、Images数量）对PE表现的影响；相关分组统计在检索到的表格残片中可见（如单事件69.60 vs 双事件68.79的语义保真对比等）。
7. 人类验证：组织专家分别对增强提示本身和对下游生成视频（Wan3.0与MiniMax-H3）进行判断，检验PEBench分数与专家判断的一致性，验证提示级直接评估能否反映下游效用。
8. 附加分析：讨论部分还分析了提示增强必要性、输出长度与充分性关系、任务复杂度影响之间的关系（具体分组统计和显著性检验细节在检索片段之外，需回原文确认）。

Q5: 发现了什么实验现象？

论文的实验结果呈现了若干值得注意的现象与反直觉发现：
1. 范式转移证据（Finding 1）：现代提示增强正从细粒度描述扩展转向意图保持的电影化规划。证据是：在更强大的闭源/大规模PE系统上，导演式创作维度的结构化能力（叙事发展、视听协调、镜头组织）表现突出，而单纯堆细节的做法不再占优；这与大型工业PE系统的设计取向一致。
2. T2V-PE上无单一方法在所有维度占优：三个维度分别由不同方法领先。由表2数据可见，语义保真维度SCMaPR最高（SF-Overall 87.92），内部一致性维度VPO领先（IC-Overall 95.07），导演式创作维度WanPE-397B排名第一（正文明确）且语义保真第二，整体平衡性最好；H3-Context-IR在综合排名上同样靠前。
3. 视觉条件任务上的明显优势方：在I2V-PE与R2V-PE上，H3-Context-IR在三个维度全部排名第一（I2V维度的SF-Overall 58.59、IC-Overall 95.05等）。这提示多阶段多模态工作流在处理首帧、参考视频等多模态条件时优势显著，因为视觉条件任务要求的是真正的跨模态推理而非简单的文本扩展。
4. 失败模式分化：LingBot-Video-PE因强格式映射限制，在T2V-PE上导演式创作维度崩溃（SF-Overall仅41.61，其中ANF仅1.00、CCP仅16.80），但内部一致性仍达90.96，说明过度结构化约束会扼杀创造力；RAPO整体最弱（T2V-PE上SF-Overall 15.47），说明检索示例重写在需要生成式规划的任务上作用有限；Prompt-A-Video两个变体在CCP等准则上得分很低（CCP 29.00/33.20），反映偏好训练对电影化覆盖的贡献不足。
5. 互补性发现：字幕重建类方法（WanPE）在电影化覆盖上占优，前向精炼类方法（SCMaPR）在语义保真与内部一致性上占优——两类机制分别解决了PE的不同瓶颈，暗示融合设计方向。
6. 原提示充分性差距与长度悖论：原提示语义保真100.00但导演式创作仅33.56，说明保留用户意图相对容易，PE的真实增值点在电影化与结构性规划；而更长的输出并不带来更高导演式创作分数，最长四分位反而比最短四分位低5.19–10.09分，说明有效PE需要添加结构化、任务相关的规格，而非单纯加长文本。
7. 复杂度顽疾（Finding 3）：时间结构复杂度和多参考复杂度仍是持久挑战，性能在更多事件阶段、更多参考组/图像条件下明显下降，说明PE在时间规划和跨资产grounding方面存在系统性短板。
8. 人类验证结论：PEBench分数与专家对增强提示的判断、以及对Wan3.0和MiniMax-H3下游视频的判断均紧密对齐，支持直接提示级评估可以替代渲染后评估的立场；但检索片段未给出具体的相关系数等统计量，需回原文确认。

Q6: 有什么可以进一步探索的点？

基于论文的发现与遗留问题，可以提炼出以下可进一步探索的方向：
1. 混合方法设计：论文结论直接建议未来PE系统应结合电影化知识与语义/逻辑保障，即把字幕重建方法的电影化覆盖优势与前向精炼方法的语义保真、内部一致性优势融合，设计两阶段或联合训练架构。
2. 以评估为训练信号闭环：PEBench提供了无需渲染的即时评估分数，可直接用作PE训练的奖励函数、RLHF偏好信号或测试时验证信号，实现评估-训练闭环；这是论文评估框架最直接的下游应用。
3. 时间规划专项攻关：Finding 3显示多事件、多镜头结构的PE表现显著下降，值得专门研究时间结构建模（如shot-level planning、event graph）、时间一致性约束注入等方法。
4. 跨资产grounding改进：多参考条件下的性能退化说明PE在多个视觉资产间的语义绑定与冲突消解能力不足，可探索基于图或检索增强的多参考融合策略。
5. 任务与模态扩展：当前PEBench已覆盖文本、图像、参考视频等模态，未来可扩展至纯音频条件、音轨生成规划、长视频/流式生成、多语言与跨文化提示，以及不可行请求拒绝等安全维度。
6. 评估框架自身升级：包括(a) 把模态感知事实提取器从LLM调用升级为专门的证据抽取模型以降低评估成本；(b) 研究24条准则的相对权重与用户偏好对齐；(c) 探索自动化评审模型以替代部分人工核验；(d) 扩展到更多下游生成器验证通用性。
7. 跨生成器与跨版本鲁棒性：目前人类验证只使用了Wan3.0和MiniMax-H3，未来可在更多生成器上验证提示级评估与下游效用的映射稳定性，并研究两者违背时的情形。
8. 长度-质量关系建模：论文发现长度与充分性无单调关系，未来可研究最优长度范围、信息密度指标，以及为不同复杂度任务自适应分配提示长度。
9. 复杂性自适应PE：按任务复杂度（事件数、参考数、风格组合数）动态调整PE的规划深度和计算预算，避免简单任务过度加工、复杂任务规划不足。
10. 方法学迁移：PEBench的准则化证据评估范式（模态感知事实提取+细粒度rubric）可迁移到其他生成评估场景，如图像提示评估、音频提示评估，以及与agent系统结合的内容规划评估（SCMaPR的agentic验证与修正模式本身就是通用agent范式）。

Q7: 总结一下论文的主要内容

论文《Towards Unified Evaluation of Prompt Enhancers for Video Generation》围绕一个核心主张展开：提示增强器（PE）应当被直接评估，而不是通过渲染视频间接评估；为了支撑这一主张，作者构建了第一个统一的PE直接评估基准PEBench，并给出了系统性的方法对比与可靠性验证。
一、论证主线。论文从视频生成生态的现状出发：现代视频生成器已经能够执行时序结构化和多模态条件的内容生成，能够理解包含时间推进、机位设计、叙事发展的复杂指令，这使得PE从可选的提示润色工具上升为把简洁用户意图转译为结构化电影计划的关键桥梁。然而，PE的评估却停留在渲染视频的路径上：现有视频生成基准评估的都是视频画面本身，要评估PE必须先跑一次生成，由此产生三项不可忽视的代价——高昂的计算与人力成本、缓慢的迭代周期、以及PE质量与下游生成器行为无法分离的归因混淆。论文因此把PE评估从生成后置评估变为生成前直接评估，并建立统一的任务定义（T2V-PE、I2V-PE、R2V-PE）与统一的评分标准。论证的关键一环是回应直接评估的可信度质疑：作者通过人类验证，证明PEBench分数与专家对增强提示本身的判断一致，也与专家对Wan3.0和MiniMax-H3生成视频的判断一致，从而论证提示级评估能可靠反映下游效用。
二、技术主线。PEBench的构建分两层。数据层包含1,100个专家核验用例与1,005个视觉资产，覆盖35类细粒度任务，任务维度刻意覆盖时间结构（单镜头到多镜头）、电影化表达、视听协调和多参考需求，并纳入不可行请求、歧义指令、冗余资产等真实退化场景。任务定义按参考条件分三类：T2V-PE无参考资产，I2V-PE给定首帧图像，R2V-PE给定多模态参考。评估层采用证据导向框架：先通过模态感知事实抽取收集可核验证据，再按24条rubric准则打分；24条准则组织为语义保真、内部一致性、导演式创作三个维度，其中导演式创作维度涵盖Storyboard and Timing Completeness、Camera Design Specificity等结构化电影化准则。评分聚合分为准则级通过率与按问题数量和严重性聚合的Overall分数两级，兼顾诊断粒度与排名可比性。评测协议对10个PE系统在T2V-PE上全面评估，多模态系统追加I2V-PE与R2V-PE评估。
三、实验主线。主结果表显示T2V-PE上三个维度各自的最优方法不同，WanPE-397B以导演式创作第一、语义保真第二呈现最佳平衡；H3-Context-IR则在I2V-PE和R2V-PE上三维度全第一，说明多阶段多模态工作流在视觉条件任务上具有显著优势。三个主要发现构成了论文的实证贡献：Finding 1发现现代PE正从细粒度描述扩展转向意图保持的电影化规划；关于长度-充分性关系的分析显示原提示语义保真可达100.00但导演式创作仅33.56，而更长的输出并不带来更高分数，最长四分位反而比最短低5.19–10.09分；Finding 3指出时间与参考复杂度仍是PE的持久挑战。在方法机制层面，字幕重建（WanPE）与前向精炼（SCMaPR）呈现互补优势，分别擅长电影化覆盖与语义保真/内部一致性；LingBot-Video-PE的强格式约束导致导演式创作崩溃，RAPO的检索重写在生成式规划任务上整体最弱，这些失败模式为PE设计提供了重要警示。论文结论认为：直接、统一、证据导向的PE评估已经可行且可靠，未来PE系统应融合电影化知识与语义逻辑保障，并着力解决时间规划与跨资产grounding瓶颈。需要说明的是，检索证据中部分表格残片（如IC维度各准则的完整名称、R2V-PE完整分数、复杂度分组统计的具体口径）未完全覆盖，这些细节建议回原文核对。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：智能体方向弱相关（权重0.10）：SCMaPR的agentic验证与修正范式、H3-Context-IR的多阶段多模态工作流，可作为多模态内容规划agent的参考案例；PEBench的细粒度准则评估方法也可迁移到agent输出评估。

## 基本信息

- 作者：Yawen Shao, Yubo Zhu, Ziyun Dai, Zixun Fang, Kai Zhu, Zeyinzi Jiang, Yufeng Ai, Siyang Sun, Haolan Xue, Yu Shang, Yuxiang Bao, Zoubin Bi, Jingming Luo, Jie Xiao, Chaojie Mao, Zhehan Kan, Hongchen Luo, Yu Liu, Sheng Zhong, Wei Tong, Xueyang Fu, Yang Cao, Wei Zhai, Zheng-Jun Zha
- 机构：论文元数据未提供机构字段，依据作者名单可合理推断部分作者来自中国科学技术大学（如Xueyang Fu、Yang Cao、Wei Zhai、Zheng-Jun Zha等），Yu Liu、Sheng Zhong、Wei Tong等作者可能来自华为诺亚方舟实验室/科大讯飞等工业机构，且被评测系统横跨多家闭源生态（Wan、MiniMax、LTX等）；但完整单位归属需回原文首页确认。
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-08
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.11736v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据（摘要与Introduction、Method、Results、Discussion的命中片段），并结合论文正文表格与讨论信息做了行内校准；对检索未覆盖的统计量与准则细节明确标注了需回原文确认。
