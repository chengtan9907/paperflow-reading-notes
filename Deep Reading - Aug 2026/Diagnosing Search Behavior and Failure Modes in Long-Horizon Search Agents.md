---
user_id: "cheng tan"
paper_id: 6234
arxiv_id: "2608.01913v1"
title: "Diagnosing Search Behavior and Failure Modes in Long-Horizon Search Agents"
institution: "清华大学 (Tsinghua University), 中国人民大学 (Renmin University of China), 新加坡国立大学 (National University of Singapore) [合理推断，基于作者背景]"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.01913v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.01913v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:58:22"
---
# Diagnosing Search Behavior and Failure Modes in Long-Horizon Search Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：long-horizon search agents · trajectory diagnosis · retrieval gap · utilization gap

## 一句话总结

本研究通过对长程搜索智能体进行轨迹级诊断，揭示了搜索努力与答案质量之间的弱相关性，并提出了区分检索差距与利用差距的失败模式分析框架。

## 摘要

> Deep search agents answer difficult information-seeking questions by iteratively issuing search queries to gather supporting evidence, but it remains unclear whether and how greater search effort leads to better answers. We study these questions through a trajectory-level diagnosis of long-horizon search agents. Using human-annotated document-level relevance judgments, we evaluate the evidence retrieved at each search step and separate two stages of agent behavior: what evidence an agent retrieves and how effectively it uses that evidence. This distinction further allows us to decompose failures into retrieval gaps, where the necessary evidence is never found, and utilization gaps, where relevant evidence is retrieved but not used correctly. With the retrieval model and evaluation harness held fixed, we compare six agents on BrowseComp-Plus and further validate our findings on BrowseComp with an open-web search API. Across settings, we find that search effort and answer quality are only weakly aligned. Answer accuracy is better correlated with the quality of retrieved evidence, especially cumulative retrieval recall, than with the number of searches or the amount of context consumed. Useful evidence often appears early in the trajectory, yet agents tend to continue searching, producing a long tail of low-yield retrieval steps. At the query level, exploratory reformulations remain useful, but the best-performing agents issue far fewer redundant queries. Overall, by systematically characterizing the search behavior and failure modes of long-horizon search agents, this work points to practical directions for building better deep research systems, including stronger query formulation, more effective evidence selection and context management, and stopping criteria based on whether sufficient supporting evidence has been retrieved.

Q1: 这篇论文试图解决什么问题？

1. **核心假设的质疑**：当前研究通常假设增加搜索步数、扩大上下文窗口或增加工具调用次数能直接提升智能体性能。本文质疑这种“搜索努力”与“输出质量”之间的线性关系。
2. **诊断维度的缺失**：现有的评估方法大多将搜索过程视为黑盒，仅关注最终答案的准确性（End-to-end Accuracy），无法定位失败究竟发生在检索阶段（没搜到）还是推理阶段（搜到了没用好）。
3. **搜索效率与冗余**：长程搜索智能体常陷入过度搜索的陷阱，产生大量低价值的查询和页面访问，如何量化这种“无效努力”并建立科学的停止准则（Stopping Criteria）是亟待解决的问题。
4. **评估基准的局限**：缺乏能够对搜索轨迹中每一步的证据相关性进行细粒度评估的基准测试，导致难以优化智能体的中间决策逻辑。

Q2: 有哪些相关研究？

1. **长程搜索智能体（Long-Horizon Search Agents）**：如 ReAct 框架及其变体，这些系统通过多轮迭代查询解决复杂的信息寻求任务。
2. **检索增强生成（RAG）**：虽然 RAG 关注证据获取，但本文讨论的智能体更强调主动、多步的查询构造和动态页面探索。
3. **智能体行为诊断**：以往研究多关注任务成功率，本文借鉴了传统信息检索（IR）中的轨迹分析（Trajectory Analysis）方法，将其引入 LLM 智能体的评估中。
4. **查询重构与优化**：研究如何通过 LLM 改进搜索词以提高检索精度，本文进一步探讨了查询冗余对系统性能的负面影响。

Q3: 论文如何解决这个问题？

1. **轨迹级诊断框架**：不再将搜索视为单一过程，而是记录并分析搜索轨迹中的每一个动作（Action）、生成的查询（Query）以及检索到的文档片段（Evidence）。
2. **证据-利用二分法（Retrieval-Utilization Dichotomy）**：引入人工标注的相关性判断，将智能体表现拆解为：
 - **检索阶段**：评估智能体是否在整个轨迹中成功覆盖了回答问题所需的“黄金证据”。
 - **利用阶段**：评估在证据已进入上下文的情况下，智能体是否能正确提取并推理出答案。
3. **失败模式分类**：
 - **检索差距（Retrieval Gap）**：关键证据从未被检索到，通常源于查询构造能力不足或检索模型限制。
 - **利用差距（Utilization Gap）**：证据已在上下文中，但智能体未能给出正确答案，通常源于上下文管理失效或推理能力不足。
4. **实验验证体系**：在 BrowseComp-Plus（受控环境）和 BrowseComp（开放网络环境）上对比了 6 种不同的智能体配置，保持检索模型和评估框架固定，仅改变智能体的决策逻辑。

Q4: 论文做了哪些实验？

1. **实验基准**：使用 BrowseComp-Plus 数据集，该数据集包含丰富的人工标注，可用于精确判断文档相关性。
2. **对比对象**：评估了 6 种具有不同提示策略、基础模型或检索逻辑的智能体（基于 ReAct 框架）。
3. **核心指标**：
 - 答案准确率（Accuracy）。
 - 检索质量：累积召回率（Cumulative Recall）、检索步数、查询冗余度。
 - 效率指标：搜索步数、消耗的上下文 Token 数、页面访问量。
4. **验证实验**：在开放网络搜索 API 环境下重复实验，以验证诊断结论在真实互联网场景下的泛化性。

Q5: 发现了什么实验现象？

1. **努力与质量的脱节**：搜索步数、页面访问量或上下文消耗量与最终答案准确率之间仅存在弱相关性。这意味着单纯增加搜索深度并不一定能带来更好的结果。
2. **早期收益递减（Early Yield）**：绝大多数有用的证据在搜索轨迹的前几个步骤（通常是前 3-5 步）就已经出现。后续的搜索往往是低收益的“长尾”，增加了噪声却未显著提升召回率。
3. **检索是决定性瓶颈**：实验显示，一旦智能体检索到了“黄金证据”，其正确回答的概率极高；反之，若未检索到，则几乎无法正确回答。这表明检索召回率是性能的上限。
4. **查询纪律（Query Discipline）**：表现优秀的智能体发出的冗余查询显著更少。失败的智能体往往在无法获取新信息时，陷入重复相似查询的死循环。
5. **上下文淹没现象**：随着搜索进行，旧的有用信息可能被后续检索到的噪声淹没，导致“利用差距”在长轨迹末端反而可能增大（合理推断）。
6. **失败模式分布**：在某些任务中，利用差距占据主导，表明即使搜到了正确信息，智能体也可能因为格式错误或锚定偏差（Mis-anchoring）而失败。

Q6: 有什么可以进一步探索的点？

1. **动态停止准则**：开发基于证据充足性（Sufficiency）的停止机制，当智能体判断已获取足够支撑证据时主动停止，而非耗尽步数预算。
2. **强化查询构造与探索**：提升智能体在初始检索失败时的探索性重构能力，避免冗余查询，学习如何从不同角度切入问题。
3. **精细化上下文管理**：研究如何动态筛选、压缩和保留关键证据片段，防止长上下文带来的干扰和推理性能下降。
4. **验证与答案归一化**：针对“利用差距”，引入专门的验证步骤（Verification）和答案格式标准化模块，减少非知识性的错误。
5. **证据驱动的训练**：利用本文提出的诊断框架，构建针对检索和利用两个阶段的专门训练数据，提升智能体的针对性能力。

Q7: 总结一下论文的主要内容

本文针对长程搜索智能体（Long-Horizon Search Agents）的性能瓶颈进行了系统性的轨迹级诊断。研究的核心发现是：搜索的“量”（步数、Token消耗）并不等同于“质”（答案准确率）。作者通过引入人工标注的相关性判断，将智能体的失败解构为“检索差距”和“利用差距”。实验证明，检索到的证据质量（尤其是累积召回率）是决定成败的关键，而许多智能体在已经获取足够证据后仍盲目搜索，导致效率低下且引入噪声。研究揭示了搜索过程中的早期收益递减现象，并指出表现强劲的智能体具有更高的“查询纪律”，能有效避免冗余。最后，本文为构建更高效的深度研究系统提供了明确方向，包括优化查询构造、实施证据驱动的停止准则以及更精细的上下文管理策略。这一工作为智能体从“盲目搜索”向“精准研究”的转变提供了理论支撑和工程指导。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接关联智能体（Agent）的搜索策略优化

## 基本信息

- 作者：Qi Liu, Jiaxin Mao, Fengbin Zhu, Tat-Seng Chua
- 机构：清华大学 (Tsinghua University), 中国人民大学 (Renmin University of China), 新加坡国立大学 (National University of Singapore) [合理推断，基于作者背景]
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.IR
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.01913v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（Abstract, Introduction, Conclusion, Discussion 等片段）。
