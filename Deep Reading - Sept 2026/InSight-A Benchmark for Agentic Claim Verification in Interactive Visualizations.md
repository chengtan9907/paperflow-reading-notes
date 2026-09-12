---
user_id: "cheng tan"
paper_id: 10440
arxiv_id: "2609.01383v1"
title: "InSight: A Benchmark for Agentic Claim Verification in Interactive Visualizations"
publish_date: "2026-09-01"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.01383v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.01383v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:34:07"
---
# InSight: A Benchmark for Agentic Claim Verification in Interactive Visualizations

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic claim verification · interactive visualization · benchmark · vision language model

## 一句话总结

本文提出 InSight，一个基于 297 篇人工分析笔记中的 21,349 条自然语言声明、要求智能体在完全交互式 Web 可视化环境中通过点击、悬停、滚动等操作主动取证并判定声明为 True、False 或 Not Enough Information (NEI) 的基准，用以评测当前视觉语言模型在动态、部分可观测证据条件下的声明验证能力。

## 摘要

> Vision Language Models have demonstrated remarkable proficiency in interpreting static visual artifacts, but modern data analysis is inherently dynamic, requiring the active interrogation of interactive environments. Existing benchmarks are predominantly constrained to static imagery and one-shot question answering and fail to capture the epistemic demands of this domain, where evidence is frequently occluded, distributed across linked views, or conditionally revealed through user agency. In this paper, we introduce InSight, a benchmark for agentic claim verification over interactive visualizations. The dataset consists of 21,349 claims derived from human-authored analytical narratives and grounded in fully interactive web-based environments. Agents must navigate these environments to determine whether a natural language claim is supported, refuted or not verifiable given the available evidence. Unlike traditional evaluations, InSight treats interaction traces as intrinsic proxies for reasoning, enabling a rigorous audit of how models seek and synthesize visual evidence. We evaluate state-of-the-art models, revealing that interactive verification remains a non-trivial challenge. We release InSight at https://github.com/maevehutch/insight.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：现有视觉语言模型评测几乎都建立在静态图像 + 单轮问答上，但现实数据分析流程（如数据探索、叙事验证）本质上是交互式的：用户需要移动鼠标、点击过滤、缩放视图、联动多个面板、悬停查看工具提示，才可能获得完整证据。
2. 证据形态的特殊性：真实分析场景中证据经常被遮挡（如 tooltip 隐藏精确数值）、分布在联动视图中、或只能通过用户操作条件性地呈现（比如点击某个图例才会显示对应的序列）。这些特性使静态图表快照无法承载回答所需的全部信息。
3. 任务形式缺口：既有的视觉问答 / 图表问答基准通常是“给定一张图 + 一个问题，输出答案”，是一次性的、非交互的；没有机制让模型去主动获取被隐藏的证据，也没有显式建模“证据不足”这一认识论状态。InSight 将任务定义为 agentic claim verification（代理式声明验证），并且引入 NEI 标签来监督不确定性，从而给评测加入一个文本事实核查里已有但视觉领域欠缺的维度。
4. 交互即推理：作者主张，模型在交互环境中的操作轨迹不应只是完成任务的手段，而是推理过程的“内在代理（intrinsic proxy）”，可以用于审计模型如何搜索和综合视觉证据。这改变了传统只看最终正确率的评测逻辑，把过程性行为和工具使用纳入评价视野。
5. 待解决问题具体化为：（1）是否能用自然语言分析叙事自动规模化地生成 grounded 的交互式验证任务？（2）当前最先进视觉语言模型在这种需要多步操作、证据部分可见、带 NEI 标签的任务中表现如何？（3）需要怎样的交互机制和评测协议来公平且可审计地评估这种能力？
6. 注意信息缺口：因可获取片段有限，论文中关于问题背景的更细化论述（如为何忽略静态可解实例、任务设计中的具体方法论权衡）尚无法在本报告中充分展开。

Q2: 有哪些相关研究？

1. 文本声明验证与事实核查（Claim Verification / Fact Checking）：该方向在纯文本设置中被广泛研究，最知名的数据集包括 FEVER、SciFact、FEVEROUS 等。这类工作强调证据检索加文本蕴含，并引入 Not Enough Information (NEI) 标签来表达证据不足的情形。InSight 直接继承这一认识论框架，把“证据不足”作为一个显式类别引入到视觉 / 交互式声明验证当中。
2. 证据获取研究（Evidence Acquisition）：检索到的片段提及了 “evidence acquisition” 这一概念，说明作者在该方向与交互式声明验证之间架桥。证据获取的核心问题是：模型不仅要判断一个声明是否被证据支持，还要主动寻找哪些信息可以作为有效证据。在带交互的视觉场景里，证据获取表现为在 UI 上执行操作来揭示被隐藏的数据。
3. 视觉语言模型评测基准（静态视图）：现有评测把模型放在静态图像或一次性问答协议下，例如常见的 VQA、ChartQA 类基准（但注意：这些具体基准名并未出现在提供的检索片段中，属于合理推断）。InSight 的动机是这些评测没有覆盖部分可观察（partially observable）环境和条件性证据。
4. 代理式 / 工具使用基准：与网页代理、GUI 自动化、交互式 QA 相关的代理评测研究也会与本文相关，但由于检索片段中未出现这类文献的直接描述，只能推测作者会将其作为对照。
5. 可视化分析叙事（Analytical Narratives）：InSight 数据来源于人写的分析笔记本，这些叙事本身承载自然语言声明，因此可视叙事、notebook 分析和 data-driven storytelling 也是相关邻域。作者基于 Human-authored analytical notebooks 生成基准，说明这些叙事提供了真实、语境化的声明来源。
6. 本文的定位：InSight 将声明验证的认识论设计中“是否存在足够证据”（NEI）这一维度，与交互式可视化环境中“通过用户代理去接触被遮挡或分布的证据”这一交互维度结合起来，是上述两个方向（事实核查与证据获取，以及交互可视化之上模型评测）的连接点。
7. 对已有工作的批评：现有基准无法捕捉该领域的认识论需求——真实场景里证据经常被遮挡、跨联动视图分布或只有用户操作才出现，静态图一次问答的设定既不真实也无法测试模型的信息寻求行为。

Q3: 论文如何解决这个问题？

1. 基准任务定义：InSight 将任务设定为“代理式声明验证”。给定一个自然语言声明以及一个完全交互式的 Web 可视化环境，智能体必须执行一系列操作（如点击、悬停、滚动、缩放、过滤等）遍历环境，最终给出三分类判定：支持（True）、反驳（False）、不可验证（NEI）。
2. 数据集构建：数据集包含 21,349 条声明，来源于 297 个人工撰写的分析笔记（human-authored analytical notebooks）。这些分析笔记本来自真实分析叙事，声明自然嵌入在上下文中，而不是由机器模板拼凑，因此更接近分析师会做的表述。每条声明被锚定在完全交互式的 Web 环境里。
3. 标签体系与分布：声明分布于 41.6% True、45.0% False 和 13.4% NEI，基本平衡但 NEI 偏少，这符合真实验证任务中证据不足相对少见的直觉。标签标注方式论文片段未详述，但从来源看很可能依赖笔记原意和展开环境中的实际数据值。
4. 交互环境设计：环境为 Web 可视化，重要特点是部分可观察性：证据有时藏在 tooltips 之后（例如悬停触发的工具提示隐藏精确数值），或需要用户滚动、点击过滤、切换联动视图才能看到。作者特别强调：即使某个声明看起来关于一张可见的图，静态渲染也通常不足以回答问题，因为精确值被交互机制（如 hover-triggered tooltips）遮蔽了。此外，这些可视化是由分析师本人构建的，因此不是合成的普通图表，而带有真实分析任务中的复杂性和布局。
5. 评测框架：InSight 将交互轨迹视为推理的固有代理（intrinsic proxies for reasoning），评测不仅看最终判定是否正确，还记录智能体如何在环境中寻找证据、点击了哪些元素、查看了哪些视图。这样可以审计模型的信息寻求策略，而不是只看结论。论文未公开具体协议细节（例如如何判定操作有效、是否允许幻觉步骤、如何计算轨迹奖励），这是原文待读部分。
6. 基线模型评估：作者用最先进的视觉语言模型进行评估，发现交互式验证仍是一个不平凡的挑战。由于检索片段未能覆盖实验结果表格，本报告无法陈述具体模型名、准确率或对比结果。

Q4: 论文做了哪些实验？

1. 据摘要和结论，作者至少完成了对“state-of-the-art models”在 InSight 上的评估，目标是检验交互式声明验证在当前最强视觉语言模型上的难度。
2. 论文中数据集分析部分报告了 297 个笔记、21,349 条声明以及标签分布 41.6% True / 45.0% False / 13.4% NEI。说明实验章节至少包含这类统计特征分析。
3. 论文设计上强调交互轨迹记录，预期实验会根据模型操作行为分析其证据获取模式，例如依赖哪些交互操作（悬停、点击等）来揭开遮蔽信息；但现有检索片段没有提供具体实验表格或量化指标。
4. 信息缺口：我们不知道具体的基线模型名称（如 GPT-4V、Claude、Gemini 还是开源 VLM）、是否做 few-shot/微调、评测指标（准确率、宏平均 F1、轨迹效率、操作成功率）、是否有消融（如去掉交互 vs 只给静态截图的对照）等。这些必须查阅完整 PDF。
5. 推测：鉴于 NEI 的存在，评测很可能包含按标签的分层结果，把 NEI 分成“声明表达不明确”“环境覆盖不足”“模型未成功找到本来存在的证据”等子类（这部分是推测）。
6. 另外，由于数据来源于分析笔记，可能还包括人机对比或人工验证一致性（如抽样计算人类标注者间一致性）——该项证据在检索中不存在，为合理推断性质。

Q5: 发现了什么实验现象？

1. 最主要的观察：在 InSight 上，当前最先进模型的表现显示“interactive verification remains a non-trivial challenge”（交互式验证仍是不平凡挑战）。这暗示现有 VLM 并不能自然地解决多步操作下的证据查找任务，至少不能像静态图表问答那样高准确率。
2. 数据集内在结构的观察：标签分布并不极端失衡，True 与 False 各约四成多，NEI 占 13.4%。这意味着模型不能通过偏向“True/False”来获得虚假高分，NEI 类别也占了足够比例，避免了对“证据不足”的忽略。
3. 可验证性的隐蔽性观察：hover-triggered tooltips 之类交互机制隐藏精确数值——实例本身显示，静态渲染的图像不足以解决即使是对可见图表的声明。这提示了评测设计中一个关键反直觉点：可视化越“交互友好”，对静态图评测的破坏性就越大。
4. 数据来源影响：visualizations 由分析师构建，不是标准图表模板，这可能会给模型带来额外挑战，比如自定义视觉 encoding、特殊坐标轴或注释，模型未必熟悉 UI 结构。
5. 推测性观察：由于 NEI 需要模型能够判断“所有可能的交互都不足以决定声明”，可以预期模型会把很多本应可判定案例错分为 NEI，反之亦然——这是实验中可能出现的典型失败模式，但检索证据未包含，仅属推测供读者在原文中验证。

Q6: 有什么可以进一步探索的点？

1. 模型侧改进：由于交互式环境要求感知-决策闭环，未来可以针对交互可视化开发专用视觉语言智能体，把网页 DOM 结构、坐标、UI 事件感知融入模型，而不是只处理编码后的截图。
2. 交互轨迹的利用和研究：InSight 将交互轨迹作为推理代理，未来可以考虑基于轨迹的辅助损失、过程奖励模型，或是在测试时用轨迹质量评分来进行推理阶段的选择。
3. 更细的 NEI 语义：可以细化 NEI 类型，例如“环境确实没有证据”“用户操作不足”“声明本身歧义”等，让评测更好地区分是模型能力缺陷还是数据问题。
4. 评测协议扩展：后续可把任务扩展到多轮对话背景下，或与图表问答、可视化纠错等并轨形成统一 agentic-visual-reasoning 基准。
5. 跨域与科学应用：如果数据源可以扩展到生物、医疗等领域的分析笔记，将有助于验证智能体在真实科学工作流中的可信度（该点结合用户画像补充）。
6. 交互动作空间的可泛化性：从 Web 可视化到桌面软件、PDF 报告等不同界面形态的迁移是值得探索的方向。
7. 数据构建流程的扩展：如何自动地从更多分析笔记本构建更大规模且不泄露答案的交互式声明？（如防模型死记硬背等）以及如何解决网页随机变动导致的锚点漂移问题。
8. 可靠性角度：可做智能体操作合法性与安全性的测评，例如模型是否可能触发崩溃、污染数据状态等（推测性建议，非论文原文内容）。

Q7: 总结一下论文的主要内容

本文针对当前视觉语言模型评测多局限于静态图像和一次性问答、无法覆盖真实数据分析中动态、交互和部分可观察证据环境的问题，提出并发布了 InSight——一个用于代理式声明验证（agentic claim verification）的基准。其任务设定是：模型作为智能体，面对一条自然语言声明和一个完全交互式的 Web 可视化环境，必须通过主动操作（点击、悬停、滚动、过滤、缩放等）去检索与综合证据，最后判断声明是被支持（True）、被反驳（False）还是证据不足（Not Enough Information, NEI）。

数据集由 297 份人工撰写的分析笔记本（human-authored analytical notebooks）派生而来，共形成 21,349 条声明；这些声明被锚定在真实的交互式 Web 环境中，并带有三类标签：True 占 41.6%、False 占 45.0%、NEI 占 13.4%。与常用问答基准不同，该环境中的证据分布被有意设计为部分可观察：大量精确数值藏在 hover 等交互动作之后，即使声明指向的是可见图表，静态渲染也同样难以作答。此外，这些可视化由分析师自己构建，有别于模板化图表，从而增加了视觉外观和交互结构上的真实性与复杂度。

论文将交互轨迹本身视为推理过程的代理证据（interaction traces as intrinsic proxies for reasoning），有别于传统只考核最终答案的评估协议，从而支持对模型如何寻找证据、如何综合不同视图信息进行过程性审计。这个设计把从文本事实核查中继承的 NEI 监督与交互式可视化环境连接起来：前者引入了明确的认知不确定性类别，后者提供了需要主动展开的视觉验证空间。在方法上，它同时涉及声明验证的文本蕴含任务形态、证据获取的过程型任务以及视觉语言模型作为代理的界面操作能力。

在实验侧，作者对若干种最先进的视觉语言模型进行了评估，主要结论是交互式验证仍是一个非常困难的问题，模型在此任务上距离可靠解答仍有很大距离。论文公开了数据集与代码仓库，以促进后续对可视化交互代理的评测研究。由于可获得的语义检索片段仅覆盖摘要、引言和结论部分的部分文字，报告无法给出具体模型名与数值指标、标注与构建详情、交互协议实现等细节——这些需要回到原文进一步核对；本总结中对实验过程与数据集内部结构的详细分析超出片段范围的部分属于合理推断或猜测，已在各字段中标注性质。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对代理（agent）研究与视觉智能体方向有直接关联：本基准提供了可操作轨迹、部分可观察证据和交互反馈的真实任务环境，适合用来评测 agent 的推理、工具使用与信息寻求能力。

## 基本信息

- 作者：Maeve Hutchinson, Syed Mahbubul Huq, Mohammad Albinhassan, Radu Jianu, Aidan Slingsby, Pranava Madhyastha
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.CV, cs.HC
- 日期：2026-09-01
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.01383v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据并在各字段中标注了置信度与推断；已按 field_evidence_map 让对应字段优先使用命中片段，但检索命中仅覆盖摘要、引言、结论和少量数据集文段，实验细节、模型名与数值缺失，故多处标记为推测或证据缺口。
