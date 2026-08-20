---
user_id: "cheng tan"
paper_id: 8146
arxiv_id: "2608.16502"
title: "When Tool-Backed Skill Retrieval Fails: Source-Style Collapse in Executable Capability Retrieval"
publish_date: "2026-08-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.16502.pdf"
pdf_url: "https://arxiv.org/pdf/2608.16502"
abs_url: "https://arxiv.org/abs/2608.16502"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-19T09:35:28"
---
# When Tool-Backed Skill Retrieval Fails: Source-Style Collapse in Executable Capability Retrieval

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：tool retrieval · source-style collapse · tf-idf routing · executable capability retrieval

## 一句话总结

本文发现并命名了工具检索中的“源风格崩塌”失败模式：在固定工具语料上，微调检索器会因查询来源风格不同而性能骤降，并据此提出基于TF-IDF指纹的源感知路由方法ToolScout，在混合查询流上显著提升覆盖率。

## 摘要

> Large-scale agents increasingly rely on retrieval to access external capabilities. We study this retrieval gate in structured tools and APIs, a measurable class of tool-backed executable skills that must be surfaced before an agent can plan, incorporate, or act. In this setting the retrieval layer can silently fail even when the capability corpus is fixed: on ToolRet, a retriever fine-tuned on one source-specific slice collapses on another source-specific slice of the same benchmark, with FT-1100 falling to 0.7% coverage on APIGen despite its higher lexical overlap with the gold tools. We call this failure mode source-style collapse. Query-side TF-IDF fingerprints flag source styles on which the fine-tuned retriever is likely to fail better than semantic or length-based proxies, giving a cheap signal for mismatch over a fixed tool corpus. We propose ToolScout, a source-aware routing method that uses this signal as a routing guard: on the mixed 4,996-query stream, TF-IDF-based routing raises coverage from 22.3% to 86.1%, and across five collapsed sources 20 matched examples raise the coverage-weighted global top-1 proxy from 1.3% to 53.9%. The same failure and routing behaviors persist when tools are rerendered as executable skill cards, which rules out raw API-schema format as the sole cause.

Q1: 这篇论文试图解决什么问题？

论文试图解决智能体能力检索中的一个关键隐患：大型智能体系统正从短小而手工制作的工具列表转向大规模外部能力池，检索层决定了模型在被重排序、规划、整合或执行之前能看到哪些能力。然而，检索这一入口在工具语料固定不变的情况下也可能静默失效。具体问题可拆解为三点：(1) 揭示一种新的失败模式：微调检索器在某个来源风格的数据上表现良好，却在同一基准中来自另一来源风格的数据上严重崩塌，即使词法重叠度更高也不能解释这种崩塌；(2) 提供一个廉价、可操作的信号来预测这种失败，而无需依赖语义嵌入或长度等代理；(3) 提出一个能利用该信号进行源感知路由、从而在混合查询流中避免崩塌的实用方法。深层挑战在于：真实环境中的查询和工具往往来自多个数据源，源分布差异（如命名习惯、描述语气、API风格差异）会导致检索模型发生域偏移，而这种偏移不是以整体性能下降的形式出现，而是以局部、集中的覆盖失败形式出现，容易被基准平均值掩盖。

Q2: 有哪些相关研究？

论文置于智能体工具调用与检索增强生成（RAG）的交汇处。相关工作可从三方面概括：(1) 工具检索与选择：已有工作如AnyTool (Du et al., 2024) 将工具选择视为智能体系统中的一个层级，ToolScout试图独立出上游路由层本身，在共享的检索与暴露协议下与其他完整智能体栈进行对比；(2) 可执行技能与API检索：随着Model Context Protocol (MCP) 等标准化接口的出现（证据中提及OpenAI 2026和MCP 2026），工具被表示为可执行技能卡片，检索与下游执行分离，本文明确研究这一检索门控；(3) 领域偏移与分布外泛化：在信息检索和NLP中，源风格差异通常被视为域偏移，但本文聚焦于“源风格崩塌”这种在固定语料中因来源切片不同而引发的局部失败，不同于传统的整体性域偏移。检索证据有限，无法确认文中具体对比的方法列表，但可合理推断论文讨论了检索器微调、重排序、工具路由和层级工具选择的相关工作。

Q3: 论文如何解决这个问题？

论文提出一套三步解决方案。(1) 失败模式界定：在ToolRet基准上，通过在不同来源切片上分别评估微调检索器，展示“源风格崩塌”现象——FT-1100在来源A上正常，却在来源B、C上覆盖率近乎归零。(2) 检测信号：提出使用查询侧TF-IDF指纹作为源风格的特征表示，并证明它比语义相似度或查询长度等代理指标能更有效地预测哪些源风格会导致微调检索器失败。TF-IDF指纹计算成本低，适合作为固定工具语料上的实时不匹配信号。(3) 路由方法ToolScout：将TF-IDF指纹信号作为路由守卫，对查询进行源感知分发，将可能不匹配的查询路由到更合适的检索器或后处理流程。在混合查询流中，TF-IDF路由整体提高覆盖率；在已被识别为崩塌的五个来源上，仅需20个匹配样例即可大幅提升覆盖率加权全局top-1代理。该路由方法独立于下游重排序和选择，可插拔地运用于现有agent管道。论文还通过将工具重渲染为可执行技能卡片的实验，排除了原始API schema格式作为唯一诱因。

Q4: 论文做了哪些实验？

论文的实验围绕ToolRet基准展开，具体包括：(1) 跨源切片评测：将微调检索器FT-1100分别在ToolRet中不同来源切片（如APIGen、ToolACE、UltraTool等）上测试，记录覆盖率，发现崩塌现象。摘要中给出APIGen 0.7%、ToolACE 0.0%、UltraTool 8.3%等数值，并对照词法重叠度以展示矛盾。(2) 信号比较：对比查询侧TF-IDF指纹与语义代理、长度代理在预测微调检索器失败源风格上的有效性。(3) 路由实验：在混合的4996条查询流上，使用TF-IDF路由和不使用路由对比；覆盖率从22.3%提升至86.1%。(4) 少样本适配实验：在五个已崩塌的来源上，仅用20个匹配样例进行校准，使覆盖率加权的全局top-1代理从1.3%提升到53.9%。(5) 可迁移性/因果排除实验：将工具重渲染为可执行技能卡片（而非原始API schema）后，重复失败与路由行为，验证现象并非由API schema格式本身引起。证据中提及附录Table 21以及与其他方法如AnyTool的比较框架，但具体基准设置、评估细节和对比方法清单受限于检索片段，未能完整获取。

Q5: 发现了什么实验现象？

实验观察到的关键现象包括：第一，源风格崩塌具有局部性和反直觉性：FT-1100在部分源切片上表现良好，却在另一些源切片上几乎完全失效，且失效源的词法重叠度反而更高，说明词法相似度不能预测实际检索成功。第二，崩塌在多个来源上普遍存在，而非个别例外：APIGen、ToolACE、UltraTool等均出现极低覆盖率，表明这是系统性的分布偏移问题。第三，TF-IDF指纹作为失败预测信号比语义或长度代理更有效，说明查询的浅层词法风格特征足以捕捉检索器擅长的范围。第四，路由干预效果显著：在混合流中覆盖率近4倍提升（22.3%→86.1%），表明预先检测源不匹配并引导到合适处理路径是有效的。第五，少样本恢复具有高样本效率：仅20个匹配样例就能将覆盖率加权top-1代理从近乎为零提升到53.9%，暗示崩溃模型可以在极小数据下快速适应。第六，技能卡片重渲染不改变失败模式，这否定了“工具表示格式”单一原因的假设，将根因指向更深层的查询-工具风格不匹配。值得注意的是，摘要中的覆盖率与top-1代理属于不同指标，前者反映检索到正确工具的比例，后者反映最终选到正确工具的概率，二者提升幅度不同，说明路由不仅提高了召回，也改善了首选决策。

Q6: 有什么可以进一步探索的点？

基于论文的发现与局限，可进一步探索的方向包括：(1) 更广泛的技能类型：当前仅验证了结构化工具和API渲染为可执行技能卡片的情况，对于程序性技能、自然语言指令或多模态工具表示，源风格崩塌是否同样成立尚待研究。(2) 独立多源语料的正向复现：论文本身提到需要另一个独立生成的多源工具语料来验证现象与方法的普遍性，这是最重要的后续试验。(3) 路由机制的深化：当前TF-IDF路由作为守卫，可以探索更细粒度的源分类、动态阈值调整、以及与学习式路由的混合。(4) 与重排序的交互：论文承认多个比较使用共享的检索、暴露和下游选择管道，因此可以研究源感知路由如何与重排序模型协同，或者将源风格信号注入重排序阶段。(5) 在线适应：用极少数匹配样例恢复的能力提示了在线持续学习的机会，例如在部署中检测新源并自动适应。(6) 更细粒度分析：研究哪些具体的词法特征（命名、参数风格、描述词汇）驱动TF-IDF信号，以提供可解释的诊断。(7) 跨任务泛化：将源风格崩塌分析扩展到其他检索场景（如文档检索、代码检索），验证其作为通用失败模式的地位。

Q7: 总结一下论文的主要内容

本文研究大规模智能体系统中工具支撑型可执行技能的检索门控问题。随着agent从短小手工工具列表转向大规模外部能力池，检索决定模型在规划前能看到哪些能力。论文聚焦于结构化工具和API这一可测量的技能类别，系统性地揭示了一个此前未命名的失败模式——源风格崩塌：即使工具语料固定不变，检索器也可能因为查询或工具来自不同数据源（具有不同的风格特征）而在部分来源切片上性能骤降。作者以ToolRet为基准，展示了FT-1100这一微调检索器在APIGen、ToolACE、UltraTool等源上覆盖率分别跌至0.7%、0.0%、8.3%，且这些低性能源具有更高的词法重叠度，排除了简单的词法匹配解释。他们进一步发现，查询侧的TF-IDF指纹能够比语义嵌入或查询长度更好地区分检索器可能失败的源风格，因此提出了一种廉价的不匹配信号。基于此，论文设计并验证了ToolScout——一个源感知路由方法，它将TF-IDF信号作为路由守卫，在混合的4996条查询流上把覆盖率从22.3%提升到86.1%；在五个已识别为崩塌的源上，用仅20个匹配样例进行校准，覆盖率加权的全局top-1代理从1.3%提升至53.9%。最后，论文将工具重渲染为可执行技能卡片后重复实验，观察到相同的失败与路由行为，证明原始API schema格式不是原因。论文作出三个关联贡献：精确界定并命名源风格崩塌、提出并验证TF-IDF失败信号、提供并评估ToolScout路由方法。其局限性包括：多个比较共享检索-暴露-下游选择管道，限制了不同架构间的独立性；缺乏独立生成的多源语料的正向复现；技能卡片结果限于结构化工具和API，不覆盖程序性技能等更广泛能力。总体而言，本文揭示了工具检索评测中一个容易被平均值掩盖的分布偏移陷阱，并给出了一个实用的工程化解法。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的“agent”方向直接重合（权重0.10），论文聚焦于智能体工具检索的关键可靠性问题，可作为tool-use agent系统设计的参考。

## 基本信息

- 作者：Yiqi Liu, Joseph James, Yang Wang, Chenghao Xiao, Chenghua Lin
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.IR
- 日期：2026-08-18
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.16502`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的摘要、引言和局限性片段，结合启发式草稿进行补全。
