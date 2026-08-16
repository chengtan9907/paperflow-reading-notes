---
user_id: "cheng tan"
paper_id: 7178
arxiv_id: "2608.07067"
title: "DocMemo: Dynamic Evidence Discovery via Probabilistic Memory-Guided Retrieval for Multi-Modal Document Understanding"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.07067.pdf"
pdf_url: "https://arxiv.org/pdf/2608.07067"
abs_url: "https://arxiv.org/abs/2608.07067"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-11T01:12:46"
---
# DocMemo: Dynamic Evidence Discovery via Probabilistic Memory-Guided Retrieval for Multi-Modal Document Understanding

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-document docvqa · memory-guided retrieval · bayesian belief update · thompson sampling

## 一句话总结

DocMemo 提出一种基于三层次记忆（文档结构记忆、页面信念记忆、问题情景记忆）的迭代推理框架，通过贝叶斯页面信念更新与 Thompson 采样实现多模态长文档问答中的动态证据探索，以克服静态 top-k 检索无法纠错和跨轮状态失忆的问题。

## 摘要

> Long-document understanding requires locating sparse and heterogeneous evidence across hundreds of pages, yet existing systems remain limited by static retrieval and fragile cross-round memory. Mainstream single-round methods commit to a fixed top-$k$ page set at the outset and struggle to recover from early retrieval errors; recent iterative approaches allow multi-round evidence acquisition, but they do not investigate the propagation mechanism of cross-round states, making it difficult to track the dynamic changes in page relevance. To address these limitations, we propose DocMemo, a memory-guided framework that formulates long-document reasoning as dynamic evidence exploration. DocMemo maintains a tri-level retrieval state consisting of Document Schema Memory, Page Belief Memory, and Question Episodic Memory, which respectively capture structural priors, dynamic relevance estimation, and query-specific reasoning trajectories. During reasoning, DocMemo continuously refines cross-round page selection through Bayesian page belief updating with Thompson sampling, spatial proximity propagation, and structure-aware adaptive-granularity evidence access, while supplementing page-level evidence with fine-grained visual regions. Experiments on 3 benchmarks show that DocMemo achieves state-of-the-art performance and validate the efficacy of structured memory and dynamic page belief updating. Code is available at https://github.com/Harrygof/DocMemo.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在长达数百页的多模态文档中，如何动态、可纠错地发现稀疏且异质的证据，以支撑视觉问答（DocVQA）等长文档理解任务。具体问题可拆解为四点：
1. 证据的稀疏性和异质性：关键答案可能出现在某页的表格角落、图表标注、页眉页脚或混合模态区域，静态检索容易漏检，且不同问题的证据分布差异极大，无法用统一规则覆盖。
2. 静态检索的单轮不可逆性：主流方法（如固定 top-k 页面检索）在推理开始前就锁定候选集，一旦初始检索遗漏关键页，后续推理无论多强都无法找回，造成错误累积且不可恢复。
3. 迭代方法的跨轮状态缺失：近期迭代式检索方法虽然允许多轮获取证据，但各轮之间的状态（如哪些页已被查看、哪些区域已被确认/排除）没有显式建模和传播机制，导致页面相关性估计无法随新证据动态更新，系统难以判断“下一步该看哪里”。
4. 上下文预算与现实约束：现有模型的上下文窗口无法容纳整本长文档，因此不能采取“被动读全文”的简单策略，必须主动选择最具信息量的页面和视觉区域，这本质上是一个序贯决策问题（exploration-exploitation trade-off）。
论文将上述问题统一抽象为“动态证据探索”问题，主张用记忆机制和概率信念更新来替代静态检索与无状态迭代。

Q2: 有哪些相关研究？

从摘要和检索到的相关研究片段看，论文将相关工作大致分为两类，并可在更广的文献脉络中定位：
1. 静态检索增强的文档理解方法：这类方法遵循“检索-然后推理”的范式，先通过某种检索器（基于文本匹配、embedding 相似度或视觉特征）选出固定的 top-k 页面，再交给多模态大模型进行推理。代表方向包括基于版面分析的页面排序、基于 CLIP 类图文匹配的段落选择等。其共性缺陷是候选集在推理前固定，无法根据推理中的新信息调整，因此对初始检索质量高度敏感。检索片段中引用标记 [17, 16, 8] 暗示了此类工作，但具体名称未见。
2. 迭代式多轮检索与推理方法：为弥补单轮缺陷，近期工作允许模型在多轮中交替进行“检索-推理-再检索”，每轮基于当前部分答案获取更多证据。这类方法提高了候选页面的召回率，但按论文观点，它们没有显式研究跨轮状态的传播机制——即上一轮中模型已经看过什么、坚信什么、怀疑什么，这些信息没有被结构化为可更新的记忆，因此页面相关性的变化难以被系统追踪，甚至可能出现重复查看同一页或忽略已排除页面的低效行为。
3. 与记忆增强推理的关联（合理推断）：本文的三级记忆设计与认知科学中的工作记忆、情景记忆概念呼应，也类似于强化学习和上下文 bandit 中利用状态估计指导探索的思路。贝叶斯信念更新和 Thompson 采样在推荐系统、主动学习等领域有成熟应用，但将其用于文档页面选择的动态调整，属于较新的交叉。
4. 与多模态长文档基准的关系：论文在 MMLongBench-Doc 等长文档 DocVQA 基准上评测（检索片段确认 MMLongBench-Doc 包含 1082 个问题、135 篇长文档），这类基准通常包含跨页、跨模态的复杂证据，对检索方法提出了补充挑战。

Q3: 论文如何解决这个问题？

DocMemo 的解决方案是把长文档推理建模为“动态证据探索”，核心是三个层次的状态记忆和随轮次更新的选择机制。
1. 三级记忆结构：
（1）Document Schema Memory（文档结构记忆）：编码文档的固有结构先验，例如页面布局类型（封面页、目录页、正文页）、常见的章节结构、元素空间分布规律等。这部分记忆在推理过程中相对稳定，用于指导“哪里更可能包含答案”，可视为对文档类型知识的长期存储。
（2）Page Belief Memory（页面信念记忆）：为每一页维护一个相关性信念，即“该页包含对当前问题有用证据”的概率分布。该信念不是静态得分，而是随每轮观察到的证据通过贝叶斯规则进行更新。例如，若某页或邻近页出现高度相关关键词，则信念上调；若反复查看未获新信息，则信念下调。
（3）Question Episodic Memory（问题情景记忆）：记录当前问题特有的探索轨迹，包括已经访问过的页面、各页获得的证据片段、已经验证/排除的假设、当前的部分答案置信度等。情景记忆使模型能够避免重复劳动，并支持对之前的错误判断进行回溯和修正。
2. 动态页面选择与信念更新机制：
（1）贝叶斯页面信念更新：每一轮访问页面后，根据观察到的视觉/文本证据，利用似然函数更新页面及邻近页的信念。这使系统能够吸收新证据并传播相关性。
（2）Thompson sampling：在从信念分布决定下一轮访问哪些页面时，采用 Thompson 采样而非简单取 top-1，以保证在利用当前高信念页面的同时，仍有概率探索低信念但可能被低估的页面，从而平衡探索与利用。
（3）空间邻近传播：文档的物理相邻页通常主题连贯，因此若某页被确认为高相关，其邻近页的信念也会获得正向更新。该先验利用文档的线性排布特性，提高证据发现的连续性。
（4）结构感知自适应粒度证据访问：根据文档结构（如目录、表格、图注）决定按整页访问还是按局部区域访问，从而节省上下文预算。例如，对于图表密集页可提取细粒度视觉区域，而非全页输入。
3. 推理循环：整体流程为迭代式——从初始信念出发，经 Thompson 采样选页，读取页面或区域，更新三级记忆，再进入下一轮，直至达到停止条件（如信念收敛、预算耗尽或置信度足够）。细粒度视觉区域作为页面级证据的补充，可精确定位图表中的答案，增强对多模态证据的覆盖。

Q4: 论文做了哪些实验？

根据检索到的实验设置片段，论文在三个长文档 DocVQA 基准上进行了评估，并与静态检索和迭代方法两类基线比较。可确认的信息包括：
1. 基准构成：三个基准中明确提到 MMLongBench-Doc，包含 1082 个问题、覆盖 135 篇长文档，涵盖多样文档类型、证据模态和推理需求；另外两个基准在检索片段中未给出名称，需回原文确认（推测可能是 LongDocVQA、DUDE 或类似长文档视觉问答基准）。
2. 评估协议：基线方法遵循相应 leaderboard 和先前工作的标准协议，以保证可比性；整体性能汇总见论文 Table 3。
3. 比较对象：包含静态检索增强方法和迭代方法两大类，具体模型名称未在片段中出现。
4. 消融与有效性验证：摘要明确提到实验验证了“结构化记忆”和“动态页面信念更新”的有效性，但未提供具体消融条目和数值。合理推断论文会包含：去掉 Document Schema Memory、将 Page Belief 改为静态打分、用固定策略替代 Thompson 采样、去掉空间传播、去掉细粒度区域等消融，以定位每个组件的贡献；同时可能报告不同轮次下的性能变化来分析动态更新带来的增益。
5. 由于未检索到实验数值表格的具体内容，无法报告准确精度；需要直接查看 Table 3 和消融表格获取 SOTA 数字及相对基线的提升幅度。

Q5: 发现了什么实验现象？

检索证据中没有提供具体实验数值、图表或趋势，因此本字段主要基于摘要、结论和已知机制做合理的推断性说明，并明确标注证据缺口。
1. 动态信念更新优于静态 top-k 选择（合理推断）：摘要称“验证了动态页面信念更新的有效性”，推测在召回率和答案准确率上，DocMemo 能显著超过固定候选集的静态基线，尤其在证据跨页分布或初始检索存在干扰项的样本上优势明显。
2. 迭代方法的增益来源并非单纯多轮寻址，而是跨轮状态传播（合理推断）：论文强调现有迭代方法缺少对跨轮状态的研究，推测 DocMemo 与无记忆的迭代基线对比时，能减少重复访问、更早收敛，并在多跳证据问题上取得更高准确率。
3. 空间邻近传播的有效性（推测）：对于主题连续的文档，邻近页信念传播应能提升对相关页面的召回，但可能在主题突变的文档中引入噪声，消融实验或能揭示该机制的适用边界。
4. Thompson 采样的探索-利用平衡（推测）：相对于每轮贪心选择最高信念页面的策略，Thompson 采样可能带来轻微的性能提升和更好的鲁棒性，但可能以轮次增加为代价；具体是否存在这种权衡需原文数据确认。
5. 负结果与失败案例：检索片段未提及任何负结果或失败案例，无法判断论文是否报告了诸如复杂表格推理失败、极长文档中信念漂移等问题。
6. 指标间张力：没有数据说明 DocMemo 在准确率提升的同时是否牺牲了推理轮次或计算开销——这是文档理解系统实际部署中常见的效率-性能张力，需要阅读完整实验章节确认。

Q6: 有什么可以进一步探索的点？

基于论文提出的机制和当前信息，可以合理推断以下值得进一步探索的方向：
1. 跨轮状态传播的理论分析：论文指出现有迭代方法缺乏对跨轮状态传播机制的研究，但自身也未深入理论分析（如信念更新在什么条件下收敛、误差如何传播）。未来可建立形式化模型，分析贝叶斯更新与 Thompson 采样在长文档检索中的 regret 上界。
2. 记忆结构的扩展与压缩：三级记忆在超长文档（如千页级手册）中的存储和更新开销可能很大，可研究记忆的稀疏化、分层压缩或遗忘机制，使框架可扩展到更大规模。
3. 与智能体（Agent）系统的结合：DocMemo 的迭代探索与记忆机制天然适合嵌入到文档问答智能体中，例如与工具调用（调用表格解析器、OCR 局部重识别）结合，使智能体能在文档中自主规划探索路径。
4. 更丰富模态的信念传播：当前空间邻近传播基于页面物理顺序，未来可考虑文档语义结构（目录层级、引用关系、超链接）构建图结构，进行非局部的信念传播。
5. 自适应停止策略：论文未明确停止条件，可探索基于信念熵或边际信息增益的自动停止准则，以在准确率和推理成本之间动态平衡。
6. 跨任务泛化：将记忆引导的动态证据发现推广到长文档摘要、表格问答、法律/医疗文书审查等下游任务，验证结构先验的可迁移性。
7. 对 AI for Science 场景的适配（结合用户画像）：在科学文献综述或实验数据筛选中，证据往往散布在多个图表和附录中，DocMemo 的“动态相关信念”概念可迁移到科学文献的证据链构建，支持可追溯的结论产生。

Q7: 总结一下论文的主要内容

DocMemo 是一篇面向长文档多模态视觉问答（DocVQA）的方法论文，核心主张是：长文档推理不应依赖于一次性的静态检索，也不应是无状态的盲目迭代，而应建模为一个由记忆引导的概率化动态证据探索过程。论文首先指出现有系统的两类缺陷：一类是单轮静态检索方法，在推理前固定 top-k 候选页，一旦初始检索漏掉关键页便无法恢复；另一类是迭代式方法，虽然允许多轮获取证据，但没有显式的跨轮状态传播机制，页面相关性随推理的实时变化被忽视，导致系统难以追踪“自己对哪些页面的判断在如何改变”。为克服这些缺陷，论文提出 DocMemo，其核心贡献是一套三级记忆架构和配套的动态选择算法。三级记忆包括：文档结构记忆，用于编码文档类型与布局的先验知识，使系统对“证据可能在哪里”有长期稳定的预期；页面信念记忆，为每页赋予并不断更新一个相关性概率，反映当前对每页信息价值的动态估计；问题情景记忆，则记录与当前查询相关的探索历史，包括已访问页面、已获取证据和未决假设，确保跨轮推理的连贯性和可回溯性。在推理过程中，DocMemo 采用贝叶斯公式更新页面信念：每轮观察到的证据会改变页面及其邻近页面的相关性估计；为了在探索（尝试低置信度页面）和利用（聚焦高置信度页面）之间取得平衡，它使用 Thompson 采样从信念分布中抽取下一批候选页；同时利用文档的空间排布先验，将高相关页面的信念向物理邻近页传播；进一步地，系统根据文档结构自适应选择访问粒度——整页或局部视觉区域——从而在有限的上下文预算内更精细地获取证据，尤其针对表格、图表等视觉密集区域，用细粒度区域作为页面级证据的补充。这一设计将长文档问答转化为一个连续的、状态丰富的序贯决策过程。实验方面，论文在三个长文档 DocVQA 基准上进行评估，其中已确认 MMLongBench-Doc 包含 1082 个问题、135 篇长文档，另外两个基准未在检索片段中命名；整体结果汇总于 Table 3，与静态检索与迭代基线相比，DocMemo 达到了当时最优（SOTA）性能，摘要还声称验证了结构化记忆与动态页面信念更新的有效性，但具体数值和消融细节需要在原文中查看。论文的实验覆盖了不同类型的文档、证据模态和推理复杂度，基线遵循对应 leaderboard 的标准协议。总体而言，DocMemo 将记忆、概率推理和探索-利用权衡引入长文档理解，为多模态文档问答提供了一种新的范式；其代码已开源（https://github.com/Harrygof/DocMemo），便于复现和后续扩展。值得注意的是，由于本次仅获取到摘要、部分方法片段、结论和实验设置片段，论文中具体的性能数字、消融表格、失败案例分析及与其他迭代方法的详细对比尚不完整，所有数值性结论均需回到原文确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体（agent）方向的关联：DocMemo 的迭代检索-推理-记忆更新循环实质上是一种轻量级智能体行为，其结构化的跨轮状态管理可作为文档问答智能体设计和记忆机制模块的参考。

## 基本信息

- 作者：Hanshu Yao, Janfeng Zhong, Niu Lian, Jinpeng Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.IR, cs.MM
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.07067`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文 PDF 的语义检索证据片段（摘要、引言、方法、结论、实验设置与结果对比），但未获取全文，部分机制细节和实验数值基于摘要与片段的合理推断，具体数字和消融结果需回原文确认。
