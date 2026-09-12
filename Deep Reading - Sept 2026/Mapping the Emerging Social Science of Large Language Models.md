---
user_id: "cheng tan"
paper_id: 10888
arxiv_id: "2609.07598v1"
title: "Mapping the Emerging Social Science of Large Language Models"
publish_date: "2026-09-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.07598v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.07598v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-12T13:21:39"
---
# Mapping the Emerging Social Science of Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language model · computational social science · science mapping · topic modeling

## 一句话总结

该论文用双尺度语料（198 篇全文精读的策展语料 + 从五个文献数据库检索的 47,719 篇正式发表文献）结合句子嵌入、K-means 聚类、簇内 LDA、作者与 LLM 分类以及结构主题模型，把新兴的「大语言模型社会科学」经验性地组织为 LLM as Social Minds、LLM Societies、LLM-Human Interactions 三大领域与 13 个子类，并通过重采样稳定性、人机分类一致性与跨方法对应率对本分类体系做了验证。

## 摘要

> Large language models (LLMs) have moved from specialized text-generation systems into everyday and institutional settings, where they increasingly shape communication, learning, work, creativity, and decision-making. Research on these developments has grown rapidly across disciplines and publication venues, but it remains fragmented and lacks an integrated framework for organizing the field. This study maps the emerging social science of LLMs through a curated corpus of 198 papers reviewed in full and a field-scale corpus of 47,719 formally published papers retrieved from five bibliographic databases. We combine sentence embeddings, K-means clustering, within-cluster Latent Dirichlet Allocation (LDA), author and LLM classifications, and structural topic modeling to identify and validate the field's organization. The analyses recover three domains: LLM as Social Minds, concerning socially interpretable model behavior; LLM Societies, concerning collective dynamics among interacting model-based agents; and LLM-Human Interactions, concerning how people perceive, use, and are affected by LLMs. Within-cluster LDA further resolves these domains into four, four, and five subcategories, respectively, spanning reasoning and theory of mind, personality and bias, political and moral judgment, and strategic influence; behavioral games, collective intelligence, group decision-making, and large-scale simulation; and trust, support, work, creativity, and education. In the curated corpus, the three-cluster solution is highly stable under resampling (mean adjusted Rand index = 0.952), and K-means assignments correspond with author full-text classifications for 77.78% of papers. Disagreements concentrate near semantic boundaries, indicating that the domains are distinguishable but permeable. At field scale, 13 of 15 topics map onto the taxonomy, and K-means and structural-topic-model domains correspond for 73.83% of overlapping papers assigned to a domain in both analyses. LLM–Human Interactions accounts for 78.02% of domain-mapped topic mass, compared with 14.53% for LLM as Social Minds and 7.44% for LLM Societies. This overall dominance masks a marked venue contrast: among highly cited papers in the highest-scoring conference venues, LLM as Social Minds and LLM Societies together account for 66.37%, whereas LLM–Human Interactions remains dominant in the corresponding journal subset at 76.81%. The resulting taxonomy provides a reproducible framework for explaining how model behavior, agent interaction, and institutional context jointly shape the social consequences of LLMs.
> Keywords: large language models; social science; taxonomy; systematic review; human–AI interaction; multi-agent systems; trust; bias; institutions; mind attribution

Q1: 这篇论文试图解决什么问题？

论文要解决的问题可以拆成三层，前两层由摘要与 Introduction 片段明确支持，第三层属于基于片段结构的合理推断。

1）现象层：LLM 已从专用文本生成系统扩散进日常与制度性场景（沟通、学习、工作、创造、决策），由此产生的社会性后果研究在多学科、多发表渠道爆发式增长，但彼此割裂。论文的核心诊断是「fragmented」并且「缺乏整合框架」——即研究者不知道自己所做的工作相对于整个领域处于什么位置，也不知道相邻工作在哪里。

2）概念层：什么算「LLM 的社会科学」？论文在正文中给出了操作性定义：对 LLM 表现出的具有社会意义的行为、LLM 智能体之间涌现的集体动力学、以及人机互动所产生的社会过程的系统性研究（证据来自第 3 节片段）。这个定义的隐含立场值得注意：它把 «LLM 本身» 作为共同被解释对象，而不是把 LLM 仅当作研究工具或仅当作被社会影响的客体。这与「AI 对社会的影响」或「用 LLM 做社会科学」这两种更常见的说法都不同，是一次刻意的对象界定。

3）方法层（合理推断）：要给一个高速演化、跨学科的领域画地图，需要同时解决样本代表性（策展小样本 vs 领域大样本）、分类效度（人工判读 vs 算法聚类）、可复现性（他人能否复用该分类）三个问题。论文的双语料与多方法交叉验证设计，正是对这三个问题的直接回应。

值得注意的是该问题设定的一个内在张力：论文同时声称「三领域可区分」与「边界可渗透」。这不是逻辑矛盾，而是把「可区分但可渗透」当作领域的真实结构属性来建模——分歧不是噪声，而是边界存在的证据。这个立场决定了后续所有效度指标的解读方式（77.78% 一致率不被视为不合格，而被视为边界的量化刻画）。

Q2: 有哪些相关研究？

需要先说明证据边界：本次检索到的证据片段集中在 Abstract、Introduction、第 3 节、Discussion 与结论性段落，并未命中论文的 Related Work 正文，因此以下对研究谱系的归纳属于「合理推断 + 领域常识映射」，具体引用对象需回原文核对，不应作为论文实际引用的证据使用。

1）科学计量学与「科学地图」（science mapping）：用共词、共被引、主题模型刻画学科结构是该领域成熟传统。论文的差异化在于语料尺度分层（198 篇精读 vs 4.7 万篇检索）、以及把「作者全文人工分类」作为效度基准之一，而非仅依赖算法自洽。

2）计算社会科学与文本聚类方法：句子嵌入 + K-means 是近年替代词袋方法的常见路线；论文在其上叠加簇内 LDA（细粒度子类）与结构主题模型（领域规模、可纳入协变量），构成「嵌入—聚类—主题」混合管线，属于方法组合层面的系统化工作，而非单点方法创新。

3）LLM 作为社会科学研究对象的三条既有线索，恰与论文三领域对应（合理推断）：(a) 把模型行为当作可社会性解读的对象（人格、偏见、道德判断、心智理论）；(b) 多智能体 LLM 社会的集体行为（博弈、群体决策、大规模社会模拟）；(c) 人机交互与人受 LLM 影响（信任、劳动、教育、创造力）。论文的贡献在于声称用经验数据验证了这三条线索确实是该领域的主轴，而不是先验分类。

4）LLM 作为研究工具的反身性：论文用 LLM 做部分分类判断（摘要中提到 author and LLM classifications），这使它同时处于「研究 LLM」与「用 LLM 做研究」两种位置。Introduction 片段强调 LLM 的新颖性不在于任何单一前所未有的能力（早期技术也能生成文本、辅助决策、模拟角色、协调计算智能体），而在于对开放式语言的整合——这一判断为该领域的「新在何处」提供了论证基础，也隐含了对「LLM 只是又一个技术」这类质疑的回应。

5）与 AI 伦理、AI 治理、HCI 的邻接关系：论文对「社会过程」的强调使其与 HCI 的用户研究传统、AI 伦理的价值研究传统都存在重叠，但定义上把重心放在「社会性可解释的行为与互动」而非规范评价，这是与伦理研究的一个可辨识分界（推测，需核对原文对边界的讨论）。

Q3: 论文如何解决这个问题？

论文的解决方案是一条「定义—双语料—多方法—交叉验证」的完整链条，可拆为五个环节。

1）给出可操作定义。论文把「LLM 的社会科学」定义为三类对象的系统性研究：LLM 表达出的具有社会意义的行为；LLM 智能体之间的集体动力学；人机互动产生的社会过程。这个定义同时起到纳入与排除作用，是后续所有聚类的语义锚点（证据来自第 3 节片段）。

2）构建两套互补语料。策展语料：198 篇经全文精读的论文，附带作者/人工的全文分类，用作「金标准」与细粒度解释基础。领域规模语料：从五个文献数据库检索到的 47,719 篇正式发表论文，用于检验分类体系在真实文献生态中的可扩展性与覆盖度。两套语料的分工是：小样本保证判读质量与子类语义清晰，大样本保证外部效度与结构代表性。

3）混合方法管线。摘要明确列出：句子嵌入（sentence embeddings）→ K-means 聚类（得到领域级划分）→ 簇内 LDA（在每个簇内部再抽子类，得到 4/4/5 共 13 个子类）→ 作者分类与 LLM 分类（作为对照标注源）→ 结构主题模型（在领域规模上抽取 15 个主题并检验与分类体系的映射）。这条管线的一个设计要点是「簇内」而非全局 LDA：先在语义空间中确定大领域，再在领域内部做主题分解，从而避免全局主题模型把不同领域的高频词混在一起。

4）多角度效度验证。论文至少用了四类一致性检验（摘要明确支持前三类）：(a) 重采样稳定性——三簇解的平均调整兰德指数 ARI = 0.952；(b) 人工效度——K-means 归属与作者全文分类在 77.78% 的论文上一致；(c) 跨方法效度——领域规模上 15 个主题中 13 个可映射到分类体系，K-means 与结构主题模型在同时被赋域的重叠论文上一致率 73.83%；(d) 边界分析——把分歧论文按其是否位于语义边界附近来解释，而不是简单归为误差。

5）从地图到议程。Discussion 片段显示，论文没有止步于分类，而是把结果转成研究议程：主张下一步应解释连接三个领域的机制（例如 LLM 个体层面的社会性行为如何汇聚成集体动力学，又如何反馈到人机互动中）。框架被定位为「经验基础扎实、操作可复现」，并且刻意保留可渗透边界以容纳跨域研究。

一个需要回原文确认的方法细节：K 值选择标准（为何是 3 而非 2 或 4）、嵌入模型与维度、K-means 的距离度量与初始化、LDA/STM 的主题数选择与超参、LLM 分类所用的模型与提示设计在本次证据片段中均未出现，属于信息缺口。

Q4: 论文做了哪些实验？

按摘要与片段可确认的实验组织为两项研究（论文内部称为 Study 1 与 Study 2，Discussion 片段明确支持这一称呼）。

Study 1（策展语料，198 篇全文精读）：
- 语料：经全文审读的 198 篇论文，带有作者的全文分类标签。
- 分析：句子嵌入 + K-means 聚类，比较不同簇数解，选择三簇解。
- 稳定性检验：重采样（resampling），报告平均调整兰德指数 ARI = 0.952，据此判定三簇解「高度稳定」。
- 人工效度检验：把 K-means 归属与作者全文分类逐篇对比，一致率 77.78%；并对不一致论文做定性定位，发现分歧集中在语义边界附近。
- 子类分解：对每个簇做簇内 LDA，得到 4、4、5 共 13 个子类，并给出子类语义命名（推理与心智理论、人格与偏见、政治与道德判断、策略性影响；行为博弈、集体智能、群体决策、大规模模拟；信任、支持、工作、创造、教育）。

Study 2（领域规模语料，47,719 篇）：
- 语料：来自五个文献数据库的正式发表论文。
- 分析：结构主题模型抽取主题（结果为 15 个主题），并与 Study 1 得到的三领域分类体系做映射检验。
- 跨方法一致性：13/15 个主题可映射到分类体系；K-means 与结构主题模型在同时被赋域的重叠论文上领域归属一致率 73.83%。
- 分布分析：计算各领域在「被映射主题质量」中的占比——LLM-Human Interactions 78.02%、LLM as Social Minds 14.53%、LLM Societies 7.44%。
- 影响力/可见度分析：Discussion 片段提到某些子方向在「高被引与更高声望发表渠道子集」中的相对占比更高，说明其学术可见度与其总体体量不成比例。

需要标注的信息缺口：样本筛选与去重流程、五个数据库的具体名单、检索式与时间窗、纳入/排除标准、LLM 分类的具体模型与提示、计算资源与代码/数据开放情况，在本次证据片段中均未出现，需回原文确认。此外，元数据给出的发表时间（2026-09-07）与 arXiv 编号（2609.07598v1）在时间上属于未来日期，建议核实版本与来源的确切性。

Q5: 发现了什么实验现象？

1）三簇解的高稳定性与「可区分但可渗透」的并存。策展语料中三簇解在重采样下平均 ARI = 0.952，这是相当高的稳定性；但同时 K-means 与作者全文分类只在 77.78% 的论文上一致。论文没有把 ~22% 的分歧当作失败，而是定位其分布——分歧论文集中在语义边界附近——并据此得出「领域可区分但边界可渗透」的结论。这是一个值得注意的解读策略：把误差的空间结构转化为对研究对象的实质判断。读者应自行判断这一转化的说服力（是否需要独立的边界论文集合来验证，原文是否报告了边界判定的定量标准，本次证据未覆盖）。

2）跨尺度、跨方法的收敛与不收敛同时存在。领域规模上 13/15 个主题能映射到三领域框架，说明小样本得出的结构在大样本中有对应物；但 K-means 与结构主题模型的领域归属一致率为 73.83%，意味着约四分之一的重叠论文在两种方法下被分到不同领域。这提示「领域归属」对方法选择敏感，尤其在多标签性质强的论文上。

3）极端不均衡的注意力分布。被映射主题质量中，LLM-Human Interactions 占 78.02%，LLM as Social Minds 占 14.53%，LLM Societies 仅占 7.44%。这是一个强信号：在以「人」为对象的研究压倒性主导的同时，以「模型社会/多智能体集体行为」为对象的研究在体量上仍是少数。需要注意「topic mass」的具体定义（是主题概率质量还是文献分配质量）在本次证据中未明确，因此不能直接把 78.02% 读作「78% 的论文」——这是需要回原文确认的口径问题。

4）影响力与体量的脱钩。Discussion 片段指出部分成果「在高被引与更高声望发表渠道子集中的相对占比高于其总体体量」，即学术可见度与体量不成比例。这意味着仅用文献计数绘制的领域地图可能低估某些方向的实际影响，也意味着「热点」与「高影响」在该领域不是同一件事。

5）地图不等于解释。论文自身在 Discussion 中承认，该框架是描述性组织，并明确把「解释连接三领域的机制」列为尚未完成的研究议程。换言之，实验揭示的是结构（有哪些领域、如何分布），而不是机制（为什么会有这些分布、领域之间如何因果耦合）。这是本次分析中最需要读者注意的边界条件。

Q6: 有什么可以进一步探索的点？

1）机制解释（论文明确提出的议程，证据充分）：Discussion 片段直接主张建立「解释连接三个领域机制」的研究议程，例如 LLM 个体层面的社会性行为（Social Minds）如何汇聚为多智能体集体动力学（Societies），又如何通过人机互动（Interactions）反馈回模型与制度。这是从描述性地图走向因果/机制性社会科学的自然下一步。

2）跨越描述性方法的方法论升级（合理推断）：把主题分布、引用可见度等结构性指标与实验、纵向面板、准自然实验结合，检验「哪些方向真的产生了社会效果」而非仅「哪些方向被写得最多」。

3）语料与覆盖面的扩展（合理推断）：扩展到预印本、非英语文献、非西方学术生态、产业界与政策文本，检验三领域框架是否为英语正式发表文献的特有结构。这一点对领域地图类工作尤其关键，因为分类体系的普适性声明高度依赖语料边界。

4）分类体系的动态化与版本化：LLM 领域演化速度极快（元数据本身显示为 2026 年），静态三领域划分的时效性存疑。可探索把该管线做成可定期重跑的监测工具，并报告领域边界随时间的漂移。

5）LLM Societies 这一「低质量占比」方向的深挖（合理推断，且与本次用户画像的 agent 方向直接相关）：仅占 7.44% 主题质量却可能承载最高的机制新意——大规模社会模拟、群体决策、涌现规范等，是结构上被低估但概念上高杠杆的区域。

6）效度方法的改进：当前效度依赖作者自分类与 LLM 分类两套标注，二者本身可能带偏。可探索多标注者协议、边界论文的专项判读、以及把「分类不确定度」本身作为输出（例如软分配概率而非硬标签）。

7）与治理/政策的接口（推测）：论文提到制度性场景与可见度差异，是否可把该地图用于研究资源配置、期刊选题、资助方向判断，是一个有实践意义但论文是否讨论过尚需核对的方向。

Q7: 总结一下论文的主要内容

论文处理的是一项元科学（meta-science）任务：为大语言模型的社会科学研究画一张经验可检验、操作可复现的地图。

问题与动机（摘要明确支持）：LLM 已从专用文本生成系统扩散到日常与制度场景，塑造沟通、学习、工作、创造与决策。相关研究在多学科、多发表渠道快速膨胀，但整体碎片化，缺少整合框架，研究者难以定位自身工作与相邻工作。Introduction 片段进一步给出一个关键的「新在何处」判断：LLM 的新颖性不在于任何单一前所未有的能力（早期技术同样能生成文本、辅助决策、模拟角色、协调计算智能体），而在于对开放式语言的整合——这为本领域作为一个独立研究对象提供了论证基础。

定义（第 3 节片段支持）：论文把「LLM 的社会科学」定义为三类现象的系统性研究——LLM 表达出的具有社会意义的行为；LLM 智能体之间的集体动力学；人机互动所产生的社会过程。该定义以「LLM 或基于 LLM 的智能体本身」为共同被解释对象。

方法与实验设计：论文采用双尺度语料。Study 1 使用 198 篇全文精读的策展语料（带作者全文分类）；Study 2 使用从五个文献数据库检索的 47,719 篇正式发表文献。分析管线为：句子嵌入 → K-means 聚类（领域级）→ 簇内 LDA（子类级）→ 作者分类与 LLM 分类作为对照 → 领域规模上使用结构主题模型。验证包括重采样稳定性、人机分类一致性、跨方法主题映射与领域归属一致性、以及各领域的主题质量分布与引用/渠道可见度差异。

主要发现：
(1) 三领域结构。LLM as Social Minds（可被社会性解读的模型行为）、LLM Societies（智能体互动的集体动力学）、LLM-Human Interactions（人如何感知、使用 LLM 并受其影响）。
(2) 13 个子类。簇内 LDA 给出 4/4/5 的细分，覆盖推理与心智理论、人格与偏见、政治与道德判断、策略性影响；行为博弈、集体智能、群体决策、大规模模拟；信任、支持、工作、创造、教育。
(3) 稳定性与效度。策展语料中三簇解在重采样下平均 ARI = 0.952；K-means 与作者全文分类在 77.78% 的论文上一致；分歧集中在语义边界附近，据此论证领域「可区分但可渗透」。
(4) 领域规模的对应。15 个结构主题中 13 个可映射到分类体系；K-means 与结构主题模型在同时被赋域的重叠论文上一致率 73.83%。
(5) 不均衡结构。LLM-Human Interactions 占被映射主题质量的 78.02%，LLM as Social Minds 为 14.53%，LLM Societies 为 7.44%；并且部分方向在高被引、高声望渠道子集中的相对占比高于其体量，显示可见度与体量的脱钩。

结论与定位：论文把该框架定位为经验基础扎实、操作可复现的领域组织方案，明确保留可渗透边界以容纳跨域研究。同时它承认这是描述性地图而非机制解释，并把「解释三领域之间的连接机制」作为下一步研究议程。

阅读时需要留意的三点：一是「主题质量占比」的具体口径未在证据中明确，78.02% 不能直接等同于论文比例；二是分类体系的 K 值选择标准、嵌入模型、LLM 分类细节等关键方法参数在本次证据中缺失；三是元数据给出的发表时间为 2026 年，属于未来日期，来源与版本需核实。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：）与「agent」方向直接相关：LLM Societies 领域（行为博弈、集体智能、群体决策、大规模模拟）是该论文三域中最具 agent 研究味道的一块，同时它在主题质量中仅占 7.44%，属于「结构上被低估」的高潜力区域

## 基本信息

- 作者：Yi Yang, Xiao Jia, Zeyun Dong, Chenzhang Wang, Zhanzhan Zhao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CY, cs.AI, cs.CL
- 日期：2026-09-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.07598v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先采用了 PDF 语义检索命中的 Abstract、Introduction、第 3 节与 Discussion 片段（含 ARI 0.952、77.78%、13/15 主题映射、73.83%、78.02%/14.53%/7.44% 等关键数字与领域定义），对未命中的 Related Work 与具体方法参数部分已明确标注为合理推断或信息缺口。
