---
user_id: "cheng tan"
paper_id: 9881
arxiv_id: "2608.28481"
title: "NL2AGBench: Benchmarking LLM Auto-Formalization for AlphaGeometry"
institution: "Valley Christian High School (Fremont, CA); Vandegrift High School (Austin, TX); Groton School (Cupertino, CA); Texas State University (Computer Science Department)"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.28481.pdf"
pdf_url: "https://arxiv.org/pdf/2608.28481"
abs_url: "https://arxiv.org/abs/2608.28481"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-01T01:17:31"
---
# NL2AGBench: Benchmarking LLM Auto-Formalization for AlphaGeometry

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm evaluation · auto-formalization · alphageometry · geometry theorem proving

## 一句话总结

本文提出 NL2AGBench 基准，用执行验证的方式评测大语言模型将英文几何题自动形式化为 AlphaGeometry DSL 的能力，发现闭源与开源模型间存在显著可执行翻译率差距，并提出错误分类与缓解策略。

## 摘要

> Recent advances in large language models (LLMs) have demonstrated strong capabilities in natural language understanding and mathematical reasoning. However, their ability to translate informal mathematical problems into formal representations remains underexplored. This limitation is particularly important for neuro-symbolic geometry systems such as AlphaGeometry, whose theorem-proving engine requires inputs in a specialized domain-specific language (DSL). Although AlphaGeometry achieves near-IMO gold-medalist performance, manually converting natural-language problems into its formal syntax remains a significant usability bottleneck. To address this challenge, we introduce the Natural Language to AlphaGeometry Benchmark (NL2AGBench), which evaluates LLMs in translating English geometry problems into AlphaGeometry-compatible formal representations. NL2AGBench uses execution-based verification within AlphaGeometry to assess translation quality rather than relying solely on textual similarity. We evaluate ten state-of-the-art open- and closed-source LLMs across multiple parameter scales and analyze executable translation accuracy, syntactic correctness, and error characteristics. Our experiments reveal a substantial performance gap between closed- and open-source models: leading closed-source models achieve executable translation rates above 80%, while even the largest open-source models struggle to consistently preserve geometric constraints and produce valid formalizations. We introduce an error taxonomy distinguishing syntax and logic errors and investigate mitigation strategies, including few-shot prompting, fine-tuning, and human-guided hinting, which yield measurable improvements across multiple model families.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：大语言模型虽然在自然语言理解、代码生成和数学推理方面能力日益增强，但其把非正式数学问题（尤其是自然语言描述的几何题）自动翻译成机器可验证的形式表示（特别是 AlphaGeometry 所需的 DSL）的能力仍然几乎没有被系统评测与研究。该问题之所以关键，是因为以 AlphaGeometry 为代表的神经符号几何系统性能强大，但输入形式化 DSL 的高度特异性导致用户必须手工完成“自然语言 → 形式语言”的转换，这一转换过程繁琐、易错，成为整个自动化证明管线中最大的可用性瓶颈之一。具体而言，论文指出：1）现有基准大多评测证明生成（proof generation）或数学推理（mathematical reasoning），也有部分评测一般自动形式化（general auto-formalization），但没有任何基准专门针对“奥林匹克式几何题到 AlphaGeometry DSL”这种特定且实际的需求；2）文本相似度等表面指标无法真正反映形式化翻译的质量，因为形式上接近但逻辑错误的形式表示无法通过定理证明器的执行检验；3）开源与闭源模型在此类任务上的真实能力差异、错误类型分布和可缓解程度都缺乏可复现的度量。作者希望能通过一个带有执行验证的基准来填补这一空白，提供可诊断的错误分类，并探索若干改进策略，为后续自动形式化系统和神经符号推理流程的工程化提供依据。由于 AlphaGeometry 的 DSL 语法约束严格，这个任务既能检验模型的数学理解，又能检验其代码生成与约束满足能力，是一个很有代表性的 AI4Math 评测场景。

Q2: 有哪些相关研究？

论文提到的相关研究主要围绕几个方向：1）大语言模型辅助形式定理证明（LLMs for formal theorem proving）：近期的研究尝试用 LLM 生成证明步骤或补全证明脚本，但这些工作大多聚焦于证明生成而不是问题陈述的形式化；2）数学自动形式化（mathematical auto-formalization）：已有一些工作将自然语言数学命题翻译为 Lean、Isabelle 等证明助手的语言，但这些往往面向一般数学文本，而非几何学中结构化的奥林匹克题目，也没有针对 AlphaGeometry DSL 的专门评测；3）语义解析（semantic parsing）：把自然语言映射到逻辑形式或程序，与本文任务密切相关，但传统语义解析通常只关注单一领域且往往用逻辑形式准确率或 SQL 执行结果衡量，对几何问题特有的构造（点、线、圆、共线、共圆等）和约束类型缺乏专门处理；4）AlphaGeometry 系统本身：AlphaGeometry 将几何问题转化为 DSL 并用搜索与语言模型结合求解，展示了强大的证明能力，但其输入 DSL 的书写需要领域知识和细心处理，这种不便正是本文要解决的。现有基准如 ProofNet、miniF2F 等偏重证明过程中间步骤，而 IMO-Level 的几何题自动形式化几乎没有基准覆盖。作者强调，他们的工作是以执行验证为核心的端到端评测，与静态匹配或困惑度评测不同，更贴近真实下游系统（AlphaGeometry 证明引擎）的需求。可以合理推断，论文还参考了代码生成评测（如 HumanEval）中采用执行测试的思想，把“能否运行”作为关键质量标准，但在几何 DSL 场景中执行验证更接近定理证明器的 check 语义。（检索证据中明确提到现有基准的缺口，但未列出具体基准名称，因此此处的领域背景是基于常见知识补充的合理推断。）

Q3: 论文如何解决这个问题？

论文提出的解决思路是构建一个专门的基准测试套件 NL2AGBench，核心设计如下：1）任务定义：将英文几何问题（类似于奥林匹克竞赛和教科书的表述）转换为 AlphaGeometry 所需的 DSL 形式表示。这样的 DSL 通常包含构造声明（例如定义点、线、圆）与几何约束（如共线、共圆、垂直、相切等），以及目标断言。形式化表示必须严格符合 AlphaGeometry 语法且逻辑上忠实于原题，才可能被证明引擎使用。2）数据与标注：收集自然语言几何问题，并为每个问题配对一个（或可能多个）AlphaGeometry 形式的参考翻译。参考翻译由作者或领域专家编写，确保语法正确且语义等价。数据集规模覆盖多种难度和不同几何主题。3）评测指标：以执行验证为主要度量——把 LLM 生成的 DSL 输入直接交给 AlphaGeometry 执行（即让证明引擎尝试读取并规范化）；如果翻译能被引擎完整解析并产生可执行的证明任务，则视为可执行翻译（executable translation）；否则视为失败。这种基于执行的验证远比文本相似度或 n-gram 重叠严格，因为引擎会强制检查语法和部分语义一致性（例如引用未定义的符号、对象类型错误都会导致执行失败）。论文还额外统计句法正确性（syntactic correctness）和错误特征，以便区分失败是“说不出来”（语法错误）还是“说错了”（逻辑错误，即语法正确但约束不忠实）。4）错误分类法：论文提出一套错误分类法，将翻译错误分为语法错误（syntax errors）和逻辑错误（logic errors）两大类，为后续诊断提供框架。语法错误指 DSL 格式、符号使用或结构违规；逻辑错误指虽能通过语法解析但遗失或扭曲了原题的关键几何条件，导致证明任务错误。5）模型评测：选取十个前沿开源和闭源 LLM，涵盖多个参数规模，进行零样本基线评测，然后实施三种干预策略——few-shot prompting（每个模型获得 54 个参考示例，包含自然语言题目与 AlphaGeometry 翻译配对）、微调（fine-tuning）、人工引导提示（human-guided hinting）。实验协议包括评估每个模型在基准子集上的可执行翻译率、句法正确率，以及错误类型分布。缓解策略的可测量提升用于判断当前 LLM 在该任务上的可改进空间。这种设计把“数据集构建、执行验证、错误分析、干预评估”串成完整闭环，不仅给出排行榜，更试图解释为什么模型失败以及如何改进。

Q4: 论文做了哪些实验？

论文的主要实验设置（依据摘要与检索证据）：1）基准数据：NL2AGBench 包含从类似奥林匹克竞赛和教科书形式收集的英语几何题，每条对应 AlphaGeometry DSL 参考翻译，实验时使用这些配对作为 few-shot 示例和评估数据。少数示例提示阶段使用了 54 个参考配对作为上下文。2）模型：评估十个 SOTA LLM，包括开源和闭源模型，参数规模覆盖多个数量级（从小规模到最大规模）。具体名单未在可见摘要或证据中列出，因此模型名称与确切参数量无法在此确认。3）基线评估：零样本情况下，模型被要求将题目直接翻译成 DSL。Figure 3 的阴影部分展示了零样本条件下各模型的表现，揭示前沿闭源模型与开源模型之间存在巨大鸿沟。4）干预实验：主要干预是 few-shot prompting，给模型 54 个示例；另外还探讨了微调和人工提示引导。论文比较了不同干预措施在多个模型家族上的提升效果。5）指标：核心指标是可执行翻译率（executable translation rate，即翻译被 AlphaGeometry 成功接收的比例）；辅助指标包括句法正确性和错误类型分布。错误分类法被用来统计每种模型产生的语法错误和逻辑错误比例。实验最终得出结论：领先闭源模型可执行翻译率超过 80%，而最大的开源模型也难以持续保持几何约束和生成合法形式化；几种缓解策略明显改善了多个模型的表现，但开源模型与闭源之间仍存在差距。需要说明的是，由于当前 PDF 证据片段有限，具体实验表格和完整训练设置（如微调数据规模、训练轮次、提示模板）在此无法一一列出，更细粒度细节需回原文核对。

Q5: 发现了什么实验现象？

从论文摘要和检索证据可归纳的实验现象：1）显著且可量化的性能鸿沟：在零样本可执行翻译率上，前沿闭源模型能达 80% 以上，而开源模型即便参数规模最大也较难稳定地保留所有几何约束并输出合法 DSL。这意味着在“自然语言→形式化 DSL”这一任务上，模型能力与模型家族的闭源/开源属性强相关，参数规模不是决定性因素。2）基线评估中的“嵌入式失败模式”：开源模型往往在保持题设约束方面更差，要么漏掉关键条件（逻辑错误），要么生成根本无法解析的 DSL（语法错误）。这提示当前开源模型的问题不只是编码纪律，也包含对几何语义的理解和形式化忠实。3）错误分类的区分度：语法错误和逻辑错误的相对比例因模型而异，这种分类确实有助于识别模型的学习瓶颈——有些模型“会说不会写”（逻辑好但语法差），另一些“会写但说错”（语法可解析但逻辑歪曲）。错误分类法为诊断提供了可操作工具。4）缓解策略的有效性：few-shot prompting（54 个示例）带来明显提升；微调和人工提示引导在多个模型家族上也都产生可测量改进。这说明即使对强大闭源模型，适当的上下文或训练信号依然能进一步压榨形式化能力；而对较弱开源模型，干预虽能缩小部分差距，但仍不足以抹平。5）干预后的残留差距：即便经过策略增强，闭源模型的领先地位并未被颠覆，提示当前形式化瓶颈仍有赖于基础模型的综合能力（数学推理、代码生成、指令遵循、长期约束跟踪）。6）从工程视角：执行验证比文本匹配更严格，许多表面上合理的翻译在 AlphaGeometry 执行时被拒绝，说明模型常常“自认为正确”而实际违反 DSL 规则，这是评测设计带来的一个重要洞察——不能只依赖语义相似度。这些观察综合表明，自动形式化是一个困难且富有挑战性的智能任务，当前 LLM 尚未完全掌握。

Q6: 有什么可以进一步探索的点？

基于论文结论可进一步探索的方向包括：1）扩展基准覆盖面：将 NL2AGBench 从几何问题扩展到代数、数论、组合等其他数学分支，构建更广泛的“自然语言到形式系统”基准，考察 LLM 跨领域形式化迁移能力。2）数据规模与多样性：扩充当前基准题目数量与难度层级，加入更多 IMO 级别问题，并考虑多源题型（如辅助线构造、动态几何等），使评测更接近竞赛实战。3）更细粒度的执行信号：AlphaGeometry 执行验证只能给出“能否执行”，未来可把证明引擎的搜索进度或失败深度当作部分信用信号，让反馈不仅二元化，还能区分“翻译基本可用但不完整”和“完全不可用”。4）错误分类的自动化：当前错误分类可能是人工或半自动，未来可以训练一个错误分类器，对错误 DSL 自动标注是语法错误还是逻辑错误，用于大规模评测。5）针对开源模型的专门训练：既然开源模型在约束保持上短板明显，可探索利用 AlphaGeometry 执行反馈做强化学习（RL），把“是否被引擎接受”作为奖励信号进行训练，这一思路可能缩小开源与闭源差距。6）与证明搜索的端到端集成：不止评测翻译，还可以评估“翻译+证明”的整体成功率，检验形式化质量对下游定理证明的实际影响，使基准更有应用说服力。7）提示工程与人机协同：论文已证明人工提示引导有效，未来可研究交互式翻译流程，让用户在翻译错误时给出修正指令，模拟人机协作的几何题解答场景，这也与智能体（agent）研究方向契合。8）可解释性分析：深入分析模型在哪些几何构造上常犯逻辑错误（如共圆条件、垂直关系、比例线段），生成错误热力图，为几何自动形式化的基础模型训练提供数据洞察。9）跨系统泛化：将 NL2AGBench 的思路移植到其他定理证明器的 DSL（如 Lean、Isabelle）或领域专用语言，检验基准设计的一般性。10）与神经符号系统结合：将 LLM 自动形式化模块直接嵌入 AlphaGeometry 的完整流程中，形成“自然语言→DSL→证明”的自动化闭环，进一步减少人工干预。

Q7: 总结一下论文的主要内容

本文针对 AlphaGeometry 等神经符号几何系统在使用时遇到的“自然语言几何题转成形式 DSL”瓶颈，提出了 NL2AGBench 基准。论文首先指出，LLM 在自然语言与数学推理上虽已强大，但将非形式化数学描述翻译成机器可验证的形式表示仍然是一个被低估且缺乏专有基准的问题。AlphaGeometry 虽然证明能力强，但其专用的 DSL 要求输入必须严格符合语法并忠实保持题目约束，手工转换成本高且易错，极大地限制了系统的可用性。为填补此空缺，NL2AGBench 被设计为一个专门评测 LLM 自动形式化能力的基准。基准的任务是：给定英文几何问题，要求模型输出对应的 AlphaGeometry DSL 形式表示。与以往使用文本相似度或逻辑形式解析精度不同，NL2AGBench 采用执行验证作为核心评估手段——将模型生成的 DSL 直接送往 AlphaGeometry 执行，只有被定理证明引擎成功接收的翻译才算可执行翻译。这种设计更贴近真实下游需求，能避免“表面正确但语义错误”的翻译蒙混过关心态。为帮助诊断失败，论文又提出语法错误和逻辑错误两大类的错误分类法，并统计不同模型产生的错误类型。在评估方面，论文选取了十个覆盖多种参数规模的开源与闭源 SOTA LLM，首先进行零样本基线测试，然后探索三种缓解策略：少量示例提示（提供 54 个自然语言-翻译对照示例）、微调以及人工引导提示。实验结果显示：闭源前沿模型的可执行翻译率超过 80%，而开源模型即使是参数最大的版本也难稳定保留几何约束并生成合法形式化，两类模型间存在一道明显鸿沟。错误分类揭示了模型失败模式的差异，语法错误与逻辑错误的相对比例能被用于定位模型弱点。缓解策略方面，few-shot prompting 作为主要干预手段带来了显著提升，微调和人工提示引导在多个模型家族上也都有可测量改进，说明当前 LLM 的形式化能力仍有一定可塑性，但基础模型的综合能力差距依然主导最终质量。总体而言，论文的主要贡献包括：提出了第一个专门针对 AlphaGeometry DSL 的自动形式化基准；确立以执行为核心的评测协议；系统评测了十个模型；提出了错误分类框架并验证了若干改进策略。该工作不仅为 AlphaGeometry 的自动化使用铺路，也为更广泛的数学自动形式化研究提供了评测范式与实证数据。文末作者指出，未来可以扩展基准范围、引入更强的反馈信号、结合强化学习和人机协同，实现更全面和实用的自动形式化系统。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文是典型的系统性基准评测工作，符合你对“系统性工作”的偏好，值得借鉴其任务设定、执行验证指标和错误分类框架。

## 基本信息

- 作者：Samuel Xiao, Judy Song, Rory Hu, Ziliang Zong
- 机构：Valley Christian High School (Fremont, CA); Vandegrift High School (Austin, TX); Groton School (Cupertino, CA); Texas State University (Computer Science Department)
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.28481`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要、绪论、结论及部分方法章节的 PDF 检索证据，并对未检索到的实验细节做了合理推断和明显标注。
