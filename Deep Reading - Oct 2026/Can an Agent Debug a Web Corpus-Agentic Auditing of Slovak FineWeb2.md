---
user_id: "cheng tan"
paper_id: 17613
arxiv_id: "2610.31636v1"
title: "Can an Agent Debug a Web Corpus? Agentic Auditing of Slovak FineWeb2"
publish_date: "2026-10-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.31636v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.31636v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-02T01:37:24"
---
# Can an Agent Debug a Web Corpus? Agentic Auditing of Slovak FineWeb2

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：corpus auditing · fineweb2 · llm agent · data curation

## 一句话总结

论文把 LLM 智能体当作一次性的“语料调试器”，在只读、限额 API 上审计 2650 万文档的斯洛伐克语 FineWeb2，发现七项问题并把它们编译成 0.7 秒内可全量扫描的可执行测试；核心结论是智能体在发现数量上并不优于静态抽样基线，但能提供全语料精确流行度和可复用的验证资产。

## 摘要

> 论文面向多语言 web 语料管线“一套配方套用上千语言”带来的语言特异性失效，提出把 LLM 一次性用作“语料调试器”而非分类器：一个工具使用型智能体通过只读、限额 API 探索语料，把每个发现编译成可执行测试，再由母语者验证。对象是 FineWeb2 的 26.5M 文档斯洛伐克语子集；三次运行每次 $2.21–$2.84，共得到四类现象下的七个发现。最大发现是一个 1,408,437 文档（5.32%）的机器生成内容农场通过了管线过滤，其中 86.6% 连更严格阈值也能通过。盲评标签支持 7 个发现中的 6 个（缺陷率 .53–1.00，随机对照 .08），不支持 1 个。编译后的测试套件无需 GPU、0.7 秒扫描全语料，而一次 LLM 分类扫描需 $12k–$90k。最佳静态抽样基线以不到一半成本验证了比任何智能体运行更多的 selector；智能体的主要增量在于每个 selector 都附带全语料精确流行度，以及可复用的可执行测试。

Q1: 这篇论文试图解决什么问题？

【问题定位】
论文要解决的不是“再训练一个更好的过滤器”，而是：当 FineWeb2 这类多语言管线把同一套配方压到上千种语言上时，如何低成本、可复核地发现某一语言的系统性失效，并把发现沉淀为可重复运行的质量测试。管线按语言识别、质量启发式、去重等步骤产出语料，跨语言统一配方意味着单个语言的失败模式不会被设计者预先枚举。

【具体盲区】
论文给出一个可操作的例子：管线对抗近缘语言污染的主要机制是高亲和词表过滤，但只在测得污染超过 10% 的语言上启用；斯洛伐克语与捷克语高度接近，却因未超过阈值而不被覆盖。这里的风险不是“斯洛伐克语里出现几个捷克语词”，而是近缘语言污染在阈值以下长期不可见，进而影响语言识别、质量过滤和下游评测。

【为什么现有路径不够】
（1）人工审计（Kreutzer et al., 2022; Caswell et al., 2020）价值高，但是一次性产物：它描述某一时点的语料，不能自动组合成下一次抓取可复用的机器，且专家注意力线性消耗。（2）把 LLM 当分类器跑全量数据，会把数据治理问题变成推断账单：一次分类扫描 $12k–$90k，而且“仍然解释不了什么”。（3）只靠静态抽样，能发现现象，但难以给出全语料精确流行度，也难以形成可执行回归测试。

【论文的切口】
作者提出第三条路：LLM 只用一次，作为“语料调试器”而非分类器。一个工具使用型智能体通过只读、限额 API 探索语料，把每个发现编译成可执行测试，再由母语者盲评。这样做的目标不是让智能体替代专家，而是把专家的一次性审计转化为“可执行、可验证、可全量扫描”的资产。

【隐含假设与可质疑点】
（1）假设智能体通过元数据 API 就能发现语义上有意义的问题；但所有保留 selector 仅用元数据，文本依赖 selector 需要外推，语义深度受限。（2）假设母语者盲评是可靠 ground truth；实际是单编码者，且捷克语/斯洛伐克语歧义未测一致性。（3）假设“精确流行度”足以补偿发现数量的不足；但论文自己报告最佳静态基线以不到一半成本验证更多 selector，说明“发现”与“表征”是两种价值，不能混为一谈。（4）标题问“Can an Agent Debug a Web Corpus?”，论文的诚实回答更接近“能，但不是以更低成本发现更多，而是以可执行测试和精确流行度取胜”。

Q2: 有哪些相关研究？

【1. 多语言 web 语料构建】
FineWeb2（Penedo et al., 2025）是论文的直接对象：它把一套多语言过滤配方应用到上千种语言，产出可复用的预训练语料。相关工程传统包括 C4/mC4、OSCAR、CCNet 等 web 语料管线；共同结构是语言识别、质量启发式、去重、污染/毒性过滤。论文的批判点不是管线没有过滤，而是“统一配方”把语言特异性失效留给了下游用户。

【2. 人工审计传统】
Kreutzer et al. (2022) 与 Caswell et al. (2020) 代表多语言语料的人工审计与语言识别诊断；近期还有 Signoroni and Rychlý (2026)、Tatariya et al. (2026) 等单语言审计。论文承认这类工作不可替代，但指出其形态是一次性 artefact：描述某一时点、不能组合成下一次 crawl 的机器、专家成本线性增长。

【3. LLM 作为分类器/标注器】
把 LLM 跑在数据上做质量分类或标注，是数据 curation 的常见替代方案。论文的批评是成本与解释性：一次全量分类 pass 需 $12k–$90k，而且只给标签，不解释失败机制；对“调试管线”而言，这等于把治理问题变成推断账单。

【4. 智能体与工具使用】
论文落在 agentic tool use / LLM agent 传统：智能体在只读、限额 API 上探索，调用工具、形成假设、编译测试。与 AutoML、data science agent、code agent 相邻，但任务不是建模，而是语料审计。关键设计是“发现即测试”：每个 finding 必须落成可执行 selector，而不是停留在自然语言报告。

【5. 近缘语言污染】
语言污染过滤常用高亲和词表/词表匹配；FineWeb2 只在测得污染 >10% 的语言上启用该机制。斯洛伐克语—捷克语是典型近缘语言对，词表方法可能在高污染时有效，但在阈值以下不触发。论文把这一点作为“统一配方”遗漏语言特异性的具体证据。

【6. 评价传统】
论文借用数据审计的评价工具：盲评标签、缺陷率、随机对照、held-out 不变式、成本与 wall-clock。与普通 benchmark 不同，它评价的是“审计工作流”而非单一模型：发现数量、验证通过率、精确流行度、可执行性、成本都要同时看。

【7. 本文差异】
最直接的区别是：论文不提出新的分类器，也不主张 agent 在发现上更强；它提出“LLM 一次调试 + 可执行测试 + 母语者验证”的流程，并用静态抽样基线做对照，报告 agent 发现更少、更贵的负结果。这个定位使它与“agent 万能”叙事保持距离，更接近方法论/系统论文。

【证据缺口】
检索片段只覆盖 Abstract 与 Conclusion，Related Work 的完整分类与引用列表未出现；以上归类基于摘要中明确出现的引用和论文问题域的合理归纳，具体章节表述需回原文核对。

Q3: 论文如何解决这个问题？

【总体架构】
论文把系统拆成四段闭环：（1）智能体在只读、限额 API 上探索语料；（2）把每个 finding 编译成可执行 selector/test；（3）在全语料上测精确流行度；（4）由母语者对抽样文档盲评，决定保留/否决。核心不是“让 agent 自由浏览 26.5M 文档”，而是把探索限制在预算化接口内，使发现可复现、可验证。

【接口与预算】
智能体只能通过 read-only API 看语料，不能直接下载/修改；预算以 inspection 次数计，片段提到 300-inspection budget，另有一次 Claude Opus 运行 147 turns、$8.17。三次便宜运行每次 $2.21–$2.84。这个设计把“agent 的探索”变成可计费、可比较的实验条件，而不是无限工具调用。

【发现即测试】
每个 finding 必须编译成 executable test，通常是元数据上的布尔 selector，例如片段中的 langdetect='cs' AND lingua_top='cs' OR domain LIKE '%.web.app' 一类查询。测试不是自然语言描述，而是可在全语料上运行的 predicate；因此可以给出命中数、流行度、反例，并可被后续 crawl 复用。

【精确流行度】
所有保留 selector 仅使用元数据，所以流行度是精确的，不需要抽样外推；片段给出一个例子：prevalence 0.120%（31,795 documents, exact）。这是 agent 相对静态抽样的关键增量：静态基线只从 300 文档 × 400 字符视图判断 precision，agent 的每个 selector 都带全语料精确计数。

【母语者验证】
每个 finding 抽 25 文档，由母语者盲评；用 defect rate 与随机对照（.08）比较。7 个 finding 中 6 个获支持，缺陷率 .53–1.00；1 个不被支持。验证不是走过场：工作流可以 fail 一个 finding，且两个 verdict 后来按既定规则、在恢复制表丢失的标签后被修正。

【全量扫描】
编译后的测试套件扫描全语料 0.7 秒 wall-clock、无需 GPU；对照是一次 LLM 分类 pass $12k–$90k。这个效率来自 metadata-only selector，而不是全文语义模型；论文明确区分：text-dependent selector 会用文本子样本，需要外推。

【与静态基线的关系】
最佳静态抽样基线让同一模型读 300 文档、每篇 400 字符，验证的 selector 数量比任何 agent run 都多，成本不到一半。论文没有掩盖这一点，而是把 agent 的贡献重新定义为“exact prevalence + executable tests + 可证伪流程”，而非“更高发现产出”。

【设计权衡】
（1）元数据 selector 快且精确，但语义浅；（2）预算化 API 可比较，但限制探索空间；（3）母语者验证提高可信度，但单编码者、近缘语言歧义带来噪声；（4）agent 发现少，但每个发现附带全量表征。论文的立场是：审计的价值不只在“找到”，更在“找到后可测量、可复用、可失败”。

【证据缺口】
片段未给出完整工具集、prompt、selector 语言、模型名称（除 Opus 外）、25 文档抽样策略、随机对照构造；这些是实现复现的关键，需回原文/附录核对。

Q4: 论文做了哪些实验？

【实验对象】
FineWeb2 的斯洛伐克语部分，26.5M documents。选择 Slovak 的理由是 mid-resource、与 Czech 近缘、且未被 10% 污染阈值的高亲和词表过滤覆盖；这是一个“管线自认为没问题、实际可能有系统性问题”的测试床。

【智能体运行】
三次运行，每次成本 $2.21–$2.84；合计得到 7 个 findings，归入 4 类现象。另有一次 Claude Opus 运行：147 turns、300-inspection budget、$8.17，因配置/成本不同单独报告。片段未说明三次便宜运行所用模型，也未说明三次是否是同一模型的不同随机种子。

【静态抽样基线】
最佳静态基线让模型读 300 文档，每篇 400 字符视图；它在不到 agent 一半成本下验证了更多 selector。这个对照是论文最重要的实验设计：如果只看“发现数量/验证 selector 数”，agent 并不占优；如果看“每个 selector 是否带全语料精确流行度”，agent 占优。

【人工验证】
盲评标签：每个 finding 抽 25 文档，母语者判断是否属于目标缺陷；defect rates .53–1.00，随机对照 .08；6/7 findings 获支持，1 个不被支持。单编码者设计，未测捷克语/斯洛伐克语歧义下的一致性。两个 verdict 在恢复制表丢失标签后按规则修正。

【测试套件效率】
编译后的 suite 全语料扫描 0.7 s wall-clock，无 GPU；对照 LLM 分类 pass $12k–$90k。这个数字是论文最强的工程卖点，但只适用于 metadata selector；全文 selector 需要文本子样本和外推，不能直接类比。

【Held-out 检验】
片段提到 invalid-confidence invariant 在 held-out data 上仍失败，失败率 40.23%；identifier-uniqueness invariant 通过。这说明测试不是只在发现集上过拟合：一个不变式稳定失败，另一个稳定通过。具体 invariant 定义需回原文。

【评价指标】
论文同时看：validated selector 数量、defect rate、随机对照、精确 prevalence、全量扫描时间、LLM 成本、可执行性、可证伪性。没有单一 winner；不同指标指向不同结论，这是论文的方法论价值。

【缺失实验】
（1）没有多语言复制，只有 Slovak；（2）没有多模型系统比较，只有一次 Opus 单独报告；（3）没有 inter-annotator agreement；（4）没有 text-dependent selector 的完整实验；（5）没有把母语者时间、工程时间计入总成本；（6）没有在 profanity-ranked 样本上运行模型，adult spam 的比较不完整；（7）三次运行的方差/统计功效未报告。

【实验设计评价】
优点是诚实、对照清晰、含负结果、有 held-out 检验和盲评；缺点是 scope 窄、单编码者、比较不完全对称。论文自己也把结论限定为“one language, one corpus and three runs of one model”。

Q5: 发现了什么实验现象？

【现象一：机器生成内容农场】
最大发现是 1,408,437 documents（5.32%）的 machine-generated content farm 通过管线过滤；其中 86.6% 连更严格阈值也能通过。这个现象反直觉的地方在于：不是“少量漏网”，而是百万级、成体系地进入语料；更严格的质量阈值也没有挡住大部分。它揭示 surface quality heuristics 的可规避性：生成内容可以模仿高质量文本的统计特征，阈值加严只改变边际。对下游而言，5.32% 的污染足以影响语言模型训练数据分布，尤其是重复模板可能放大。

【现象二：agent 发现不占优】
最佳静态抽样基线以不到一半成本验证了比任何 agent run 更多的 selector。这是明确的负结果：agent 的探索没有带来更多可验证发现。可能原因（合理推断）：静态样本由同一模型阅读，信息密度高；agent 受限于 300 次 inspection 和元数据 API，探索空间被预算切碎；agent 倾向围绕少量强信号深挖，而不是覆盖更多现象。

【现象三：agent 的价值在表征而非发现】
每个 agent selector 都带全语料精确 prevalence；静态基线只从 300 文档 × 400 字符视图判断 precision。比如一个 selector 的 prevalence 0.120%（31,795 documents, exact）。这意味着 agent 更适合回答“这个缺陷在语料中占多少”，而不是“还有多少没被发现”。指标之间的张力很清楚：发现数量 vs 表征精度，成本 vs 可复用性。

【现象四：验证通过率与文档级纯度不一致】
6/7 findings 获支持，但 defect rates 最低 .53，意味着即使“被支持”的 finding，文档级命中也可能近半数不是目标缺陷。Finding-level precision 与 document-level precision 是两回事；论文用随机对照 .08 说明信号真实，但不能保证 selector 干净。这对下游使用很重要：精确 prevalence 不等于精确标注。

【现象五：工作流可以证伪】
一个 finding 未被盲评支持；两个 verdict 在恢复制表丢失的标签后按规则修正。这说明流程不是“agent 说了算”：可执行测试 + 盲评 + 裁定规则可以 fail 掉发现。这是论文反复强调的贡献：把一次性审计变成可失败、可修正的机器。

【现象六：不变式的稳定性】
invalid-confidence invariant 在 held-out data 上仍失败，40.23%；identifier-uniqueness invariant 通过。一个失败、一个通过，说明测试套件不是全盘噪声：有些质量属性是系统性的，换数据仍成立；有些则可能过拟合。具体 invariant 的语义需回原文，但“held-out 仍失败”是较强的泛化证据。

【现象七：静态策略可达 agent 发现】
结论说 agent 验证的每个 defect 都在某个静态策略可达范围内；adult spam 由 profanity-ranked sample 触达，但他们没有在该样本上运行模型。这暗示 agent 的发现并不“独特”，简单词表/排序基线也能触及；agent 的增量是精确测量和测试化，而非独有洞察。

【现象八：元数据依赖与覆盖偏差】
所有保留 selector 仅用元数据，因此 prevalence 精确、扫描极快；但这也意味着 agent 可能抓的是 domain、langdetect、lingua_top 等元数据模式，而非文本语义。片段还出现“never a lukewarm sample from the thresholded…”的残句，提示 agent 可能偏好阈值极端样本，而非代表性中间案例；若如此，prevalence 虽精确，slice 的覆盖面可能偏窄（合理推断，需回原文）。

【现象九：重复簇与隐私】
片段提到最大的 duplicate clusters 规模在 1,349–1,370,347 之间（具体口径需核对），以及“Real email…”；前者指向去重可能留下超大规模重复簇，后者指向真实邮箱等个人数据。两者都是管线过滤的边界 failure：去重与 PII 过滤并不因为“统一配方”就自动可靠。

【现象十：单编码者与近缘语言歧义】
盲评由单编码者完成，未测 ambiguous Czech/Slovak text 的一致性。斯洛伐克语与捷克语高度接近，验证者可能把捷克语文本判成斯洛伐克语或反之；这会直接影响 contamination 类 finding 的 defect rate。论文把它列为 limitation，读者应把它当作标签噪声上界。

【指标张力总结】
（1）发现更多 ≠ 表征更好；（2）成本更低 ≠ 总成本更低（未计专家时间）；（3）finding 被支持 ≠ selector 文档级精确；（4）prevalence 精确 ≠ 语义覆盖完整；（5）0.7 秒扫描 ≠ 全文审计。论文的贡献恰恰是把这些张力摊开，而不是用单一指标宣布 agent 胜利。

Q6: 有什么可以进一步探索的点？

【1. 跨语言与跨语料复制】
当前只有 Slovak FineWeb2、三次运行、一个模型。最直接的后续是把同一工作流搬到捷克语、波兰语、乌克兰语等近缘语言，以及其他 mid-resource 语言和不同管线（C4/mC4、OSCAR、HPLT）。重点不是“再跑一遍”，而是检验：哪些 failure mode 是语言特异的，哪些是管线通用的；10% 污染阈值在不同语言对下是否合理。

【2. 智能体工作流改进】
（1）探索策略：从“深挖少数信号”转向“覆盖多现象”，例如分层采样、active learning、不确定性驱动的 inspection；（2）预算分配：把 300 次 inspection 分成“发现/验证/表征”三段，避免早期锁定；（3）与静态抽样结合：用静态样本做广覆盖发现，用 agent 做精确 prevalence 和测试化；（4）让 agent 输出“未发现/负结果”，而不仅是 finding。

【3. 评估设计】
需要等预算、等模型、等样本量的比较：多次运行、多个模型、统计功效、预注册指标；把母语者时间、工程时间、提示设计成本计入总成本；报告 inter-annotator agreement，尤其是 Czech/Slovak 歧义文本；对 finding-level 与 document-level precision 分开报告。

【4. 文本依赖 selector】
目前所有保留 selector 是 metadata-only，prevalence 精确但语义浅。后续应研究 text-dependent selector 的抽样外推：如何用分层抽样估计 prevalence 及置信区间；如何在不扫描全文的情况下做近似全量测试；如何把 embedding/classifier 作为 selector 但保持可解释与可复核。

【5. 管线集成与回归测试】
把编译后的测试套件接入语料管线 CI：每次 crawl 后自动跑 0.7 秒级 metadata 测试；对失败项报警；对新增 selector 做版本化；把 finding 与 pipeline 版本、日期、语言绑定。这样“审计”才真正变成“机器”，而不是一次性报告。

【6. 内容农场与对抗演化】
1.4M 文档、86.6% 通过更严格阈值，说明内容农场不是简单阈值问题。后续可研究：domain/provenance 信号、生成痕迹、模板重复、跨文档一致性、adversarial evasion；以及当过滤加严后，内容农场如何迁移。

【7. 人机协作与裁定规则】
论文的两次 verdict 修正说明裁定规则很重要。后续可研究：多编码者、盲评界面、分歧解决、规则优先级、可审计的裁定日志；以及“母语者 + agent”如何分工，才能减少专家线性注意力。

【8. 安全、隐私与发布】
真实邮箱等 PII、轨迹脱敏、第三方数据版权都需要明确协议。后续可研究：PII selector 的精确率/召回率、redaction 对复现的影响、差分隐私式发布、以及如何在不泄露个人数据的前提下共享审计工件。

【9. 理论问题】
什么条件下 agentic audit 优于静态抽样？论文给出的初步答案是：当目标是“精确 prevalence + 可执行测试”时；当目标是“发现更多现象”时，静态抽样可能更划算。后续可以把“发现”和“表征”形式化为不同效用函数，做预算约束下的最优审计设计。

【10. 更广的失败模式】
除污染、内容农场、重复、adult spam、PII 外，还可审计：benchmark contamination、版权文本、toxicity、machine translationese、OCR 噪声、模板化 SEO、语言识别置信度校准。论文的 workflow 是通用的，但每个 failure mode 需要自己的 selector 与验证协议。

Q7: 总结一下论文的主要内容

【一、研究问题】
论文从一个结构性矛盾出发：FineWeb2 这类多语言语料管线用一套配方覆盖上千种语言，效率极高，但单个语言的失败模式不会被预先枚举。对斯洛伐克语，管线对抗近缘语言污染的主要机制是高亲和词表过滤，且只在测得污染超过 10% 的语言上启用；Slovak 与 Czech 高度接近，却因未超阈值而不被覆盖。于是问题变成：如何审计一个已经上线的管线在某一语言上的盲区，并把审计结果变成下一次 crawl 还能用的机器？

【二、现有路径与论文定位】
人工审计（Kreutzer et al., 2022; Caswell et al., 2020）有价值，但是一次性 artefact：描述某一时点、不能组合、专家成本线性增长。把 LLM 当分类器跑全量数据，则把 curation 变成 inference bill：一次 pass $12k–$90k，而且不解释失败机制。论文提出第三条路：LLM 只用一次，作为 corpus debugger。一个工具使用型智能体通过只读、限额 API 探索语料，把每个 finding 编译成 executable test，再由母语者盲评。目标不是让 agent 替代专家，而是把一次性审计转化为可执行、可验证、可全量扫描的资产。

【三、方法主线】
系统闭环有四段：（1）agent 在 read-only、budgeted API 上探索，片段提到 300-inspection budget，另有一次 Opus 运行 147 turns、$8.17；（2）每个 finding 编译成元数据上的布尔 selector，例如 langdetect、lingua_top、domain 等条件；（3）selector 在全语料上运行，给出精确 prevalence；所有保留 selector 仅用元数据，所以不需要抽样外推；（4）每个 finding 抽 25 文档，母语者盲评，defect rate 与随机对照 .08 比较。验证后保留/否决，两个 verdict 后来按规则修正。

【四、实验与关键数字】
对象是 FineWeb2 的 26.5M 文档 Slovak 子集。三次 agent 运行，每次 $2.21–$2.84，得到 7 个 findings，归入 4 类现象。最大发现是 1,408,437 文档（5.32%）的机器生成内容农场通过管线过滤，其中 86.6% 连更严格阈值也能通过。盲评支持 6/7 findings，defect rates .53–1.00，随机对照 .08；1 个 finding 不被支持。编译后的 suite 全语料扫描 0.7 秒、无需 GPU；对照 LLM 分类 pass $12k–$90k。一个 selector 的精确 prevalence 为 0.120%（31,795 文档）。Held-out 上 invalid-confidence invariant 仍失败，40.23%；identifier-uniqueness invariant 通过。

【五、最重要的负结果】
最佳静态抽样基线让同一模型读 300 文档、每篇 400 字符，在不到 agent 一半成本下验证了比任何 agent run 更多的 selector。论文没有回避这一点：agent 在“发现数量”上不占优。它的增量是每个 selector 都带全语料精确 prevalence，以及可执行、可复用、可失败的测试资产。结论还指出，agent 验证的每个 defect 都在某个静态策略可达范围内；adult spam 由 profanity-ranked sample 触达，但他们没有在该样本上运行模型。这意味着 agent 的发现并不“独特”，其价值在表征与测试化。

【六、验证与裁定】
盲评不是装饰：6/7 支持，1 个失败；两个 verdict 在恢复制表丢失的标签后按既定规则修正。工作流可以 fail 一个 finding，这是作者强调的贡献。但验证设计也有明显边界：单编码者、25 文档/finding、未测 Czech/Slovak 歧义下的一致性；defect rate 最低 .53 说明 finding 级支持不等于文档级纯净。

【七、限制与边界】
论文自述 scope 是 one language, one corpus and three runs of one model；所有保留 selector metadata-only，text-dependent selector 需要文本子样本与外推；0.7 秒扫描不能推广到全文测试；成本只算运行，不含母语者、工程、提示设计；与静态基线的比较不完全对称（基线 300×400 字符，agent 有全量 prevalence）；发布轨迹/摘录脱敏，限制复现；单编码者与近缘语言歧义是标签噪声来源。

【八、结论】
论文的贡献不是“agent 能替代人工审计”，而是“agent 可以把一次审计变成可执行测试”。它用负结果校准了 agent 叙事：在发现更多失败模式上，静态抽样更便宜；在精确测量流行度、形成可回归测试、支持证伪上，agent 工作流有独立价值。对语料治理的启示是：不要只问“过滤器有没有漏”，还要问“漏了之后能不能测、能不能复跑、能不能被证伪”。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：智能体（权重 0.10）：论文是 agentic workflow 的诚实评估，含负结果，适合关注 agent 何时有用的读者。

## 基本信息

- 作者：Marek Šuppa, Ivan Vykopal, Jaroslav Kopčan
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-22
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.31636v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract 与 Conclusion 片段；片段存在明显双栏混排、断行和截断，凡超出这些片段的实验细节均已在对应字段标注为合理推断或证据缺口。
