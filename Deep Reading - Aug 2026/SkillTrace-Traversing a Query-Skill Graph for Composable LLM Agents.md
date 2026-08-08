---
user_id: "cheng tan"
paper_id: 6206
arxiv_id: "2608.02356v1"
title: "SkillTrace: Traversing a Query-Skill Graph for Composable LLM Agents"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02356v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02356v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:53:09"
---
# SkillTrace: Traversing a Query-Skill Graph for Composable LLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：composable skill retrieval · llm agents · query-skill graph · graph traversal

## 一句话总结

SkillTrace 通过构建并遍历 Query-Skill Graph，将可组合技能检索问题建模为查询组合关系、查询-技能匹配和技能依赖关系的三层图问题，在 SkillsBench 和 ALFWorld 上取得 SOTA 成功率。

## 摘要

> Large language model agents increasingly solve complex tasks by composing reusable skills from a library. To address this, the key challenge is not merely to retrieve individually relevant skills, but to identify a complete and executable skill composition. In this paper, we argue that this problem can be solved in a graph with three levels: compositional relations among skill queries, similarity between queries and candidates in the skill library, and the dependencies among the selected candidates. We introduce SkillTrace, which organizes the user query into a semantic hierarchy, matches skill queries and candidates, and propagates over the skill dependencies. Experiments on SkillsBench and ALFWorld demonstrate that SkillTrace achieves state-of-the-art performance, reaching a success rate of 53.17% on SkillsBench and 91.43% on ALFWorld. SkillTrace also delivers consistent improvements across different backbone language models, demonstrating the generality and robustness of graph-based skill retrieval.

Q1: 这篇论文试图解决什么问题？

这篇论文要解决的核心问题是：在给定一个复杂用户查询和一个可复用技能库的情况下，如何检索出一组不仅各自相关、而且能够共同组成可执行流程的技能集合。作者明确指出，这不同于传统的信息检索或 RAG——后者只关心单个文档或知识片段与查询的相关性；也不同于单独的工具选择，因为可组合技能检索需要同时保证技能的“完整性”（覆盖复杂任务的所有必要原子能力）和“可执行性”（技能之间存在正确的调用或依赖顺序）。
论文将挑战拆解为三个层次：第一，复杂用户查询需要被分解为原子技能查询，并且这些原子查询之间存在组合关系（例如子任务、顺序、并行、条件等），丢失这种组合关系会导致检索到的技能集合结构错误；第二，每个原子技能查询需要与技能库中的候选技能进行语义匹配，这一层类似传统检索，但匹配质量直接影响后续组合；第三，被选中的候选技能之间可能存在依赖关系，只有满足依赖关系（如前置技能、输入输出格式、环境状态）的技能组合才能真正执行。三个层次缺一不可。
问题定义片段还指出，穷举评估所有可能的技能组合不可行，因为搜索空间随技能库大小呈指数增长。这说明该问题具有组合优化的性质，需要一种结构化方法来剪枝和引导搜索。此外，作者认为这三个层次可以被统一建模为一个 Query-Skill Graph：图连接了用户查询的内部结构与技能库的组织方式，在图上的搜索提供了一种 principled 的移动方式。这个观点是论文的核心论点：与其把检索看成 flat 的 top-k 选择，不如看成图上的路径/子图发现。

Q2: 有哪些相关研究？

相关工作可以按几条线梳理：1) 传统信息检索（Karpukhin et al. 2020）和检索增强生成（Lewis et al. 2020），它们只处理单轮或段落级相关性，不建模技能间组合；2) 工具选择/工具组合（Patil et al. 2024; Qin et al. 2023），ToolBench 要求智能体选择合适工具并组成合法调用序列，但工具调用通常依赖 API 文档且组合约束较弱；3) 技能库基准，如 SkillsBench (Li et al. 2026c) 将技能表示为结构化程序性知识包，包括指令、脚本和资源，用于专家密集型工作流；ALFWorld (Shridhar et al. 2020) 是具身环境，可复用能力表现为高层面策略；4) 从实验描述看，baselines 中包含 flat 方法和 graph-based 方法，说明已有工作尝试用图建模，但 SkillTrace 宣称在两者之上都有提升。论文的差异点在于同时显式建模查询间组合关系、查询-技能匹配、技能间依赖关系，而以往方法通常只覆盖其中一两个层面。

Q3: 论文如何解决这个问题？

SkillTrace 的流程大致分为三步：第一步，将复杂用户查询组织成语义层次（semantic hierarchy），即把用户意图分解为原子技能查询并保留它们之间的组合关系；第二步，通过查询-技能匹配（query-skill matching）在技能库中识别种子技能（seed skills），即对每个原子查询找到最相关的候选技能；第三步，在技能依赖图上进行遍历（graph traversal），沿着技能之间的依赖关系传播，逐步补全整个技能组合，直到得到完整且可执行的技能集合。论文将这三种关系统一为 Query-Skill Graph：一方面连接查询的内部结构，另一方面连接技能库的组织方式；搜索该图就是检索过程。需要说明的是，可见片段没有给出具体的图构建算法、匹配打分函数、遍历策略或传播细节（例如是否用 LLM 生成依赖边、是否用向量检索做初筛、遍历是深度优先还是 beam search 等），这些方法细节需要阅读原文确认。同样未知的是语义层次如何从查询中抽取——是纯 LLM 分解还是混合规则；以及依赖关系是预定义的还是动态计算的。不过，从问题定义片段“将可组合技能检索形式化为……”来看，作者是将整个检索问题作为一个图搜索问题来定义，而非传统的 top-k ranking。

Q4: 论文做了哪些实验？

实验在两个基准上进行：SkillsBench 和 ALFWorld。SkillsBench 上 SkillTrace 达到 53.17% 成功率，ALFWorld 上达到 91.43%，均超过所有对比方法（包括 flat 检索基线和基于图的基线——原文称 “outperforming all flat and graph-based baselines”）。论文还报告了不同 backbone 语言模型下的一致性改进，以证明框架的通用性和鲁棒性。但可见材料中没有列出具体 backbone（如 GPT、Llama 等）、baseline 的完整名单、训练/推理开销、消融实验、依赖图统计、查询分解质量评估等细节；也没有说明成功率的具体计算口径（例如任务级完全成功还是部分成功）。因此实验部分只能确认 headline 数字和相对位置，细致的实验设计需要回到原文的 Experiments/Results 部分核对。

Q5: 发现了什么实验现象？

从 main results 片段可以观察到的现象包括：1) SkillTrace 在 SkillsBench 上以 53.17% 超过所有 flat 和 graph 基线，说明显式建模依赖并通过图遍历补全技能，比只做扁平相关检索更有效；2) 在 ALFWorld 上 91.43% 的优势同样成立，说明该优势在具身环境（高层面策略技能）中也存在，不限于文档型技能库；3) 跨多个 backbone 的一致提升，说明三层图公式并不依赖某个特定模型，而是带来了可迁移的结构收益。由于片段中没有消融或负结果信息，无法判断哪一层贡献最大；也没有展示失败案例或错误模式。一个值得注意的隐含现象：ALFWorld 成功率远高于 SkillsBench，可能反映基准难度差异——ALFWorld 的技能组合空间更小或更结构化，但这属于推测。

Q6: 有什么可以进一步探索的点？

可进一步探索的点包括：1) 把 Query-Skill Graph 的构建端到端学习化，而不是依赖预定义的语义层次和依赖边；2) 在更大规模、更异构的技能库上验证可扩展性，尤其是在库规模增大时图构建和遍历的复杂度；3) 引入不确定性或置信度传播，处理依赖关系不完整/冲突的情况；4) 与 agent 规划循环集成，将技能检索与执行反馈闭环，实现检索-执行-再检索；5) 对其他 benchmark（如 ToolBench、API-Bank、MiniWoB++ 等）和跨领域（如科学计算工作流、AI for Science 场景）迁移；6) 研究查询分解质量对最终成功率的影响，量化分解错误的传播；7) 扩展到多模态技能（视觉、具身技能）和动态技能库（技能可新增/修改）；8) 分析图遍历策略的搜索效率与最优性权衡，比如 beam width、剪枝规则；9) 提供失败模式分析，例如在哪些查询类型上三种关系建模仍不足。

Q7: 总结一下论文的主要内容

论文的核心主张是：可组合技能检索不能只看单个技能的相关性，而必须把“完整”和“可执行”作为目标，并且这个目标可以通过一个三层关系图自然地求解。作者首先在引言中区分了可组合技能检索与传统信息检索（Karpukhin 等 2020）、RAG（Lewis 等 2020）和工具选择（Patil 等 2024；Qin 等 2023）的不同：前者需要一组能共同完成复杂任务的技能，而不是单个相关结果。同时，他们以 SkillsBench（技能被表示为包含指令、脚本、资源的程序性知识包）、ToolBench（API 调用组合）和 ALFWorld（高层面策略技能）为例，说明尽管技能形态不同，共同需求都是识别并协调一组能力。
在此基础上，论文把挑战拆成三个层次：查询内部原子技能查询的组合关系；查询与候选技能之间的语义匹配；候选技能之间的依赖关系。问题定义片段进一步指出，穷举技能组合不可行，因为搜索空间随技能库大小指数增长，因此需要把可组合技能检索形式化为图搜索问题。这些关系合并成一个 Query-Skill Graph：图既捕捉查询的内部结构，也反映技能库的组织方式；在这个图上搜索就是从查询结构出发，通过匹配和依赖遍历来寻找完整技能子集。
SkillTrace 的流程是：先把复杂用户查询组织成语义层次，保留原子查询之间的组合结构；然后做查询-技能匹配，找到种子技能；最后通过在技能依赖图上遍历和传播，把缺失的技能逐步补全，直到得到可执行组合。论文强调，这个三层建模与以往 flat 或只覆盖部分关系的图方法不同，是它性能提升的主要来源。
实验部分，SkillTrace 在 SkillsBench 上达到 53.17% 成功率，在 ALFWorld 上达到 91.43%，均超过所有 flat 和 graph 基线；并且在多个 backbone 语言模型上保持一致的提升，作者据此认为显式建模查询组合和技能依赖对检索完整可执行技能集很重要，也为构建更强的技能型 agent 提供了方向。
需要指出的是，可见的 PDF 检索证据主要覆盖摘要、引言、问题定义、结论和主结果片段，并未给出完整方法章节、完整实验设置、详细的 baseline 列表、消融、超参数和计算开销。因此上述内容中除了数字和三层公式外，具体算法细节（如依赖图如何构建、遍历如何实现）和实验协议都属于合理推断或未知，需要阅读原文确认。论文的贡献可以概括为：提出三层关系统一图公式、设计 SkillTrace 框架、在两个基准上取得 SOTA 并验证跨 backbone 鲁棒性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 ‘agent’ 方向直接相关：论文正是围绕 LLM agent 的技能检索与组合。

## 基本信息

- 作者：Yue Yao, Shengyuan Wang, Xin Chen, Minke Zhang, Jia He, Bingjun Luo, Tom Gedeon
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02356v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 的语义检索证据片段（abstract/introduction/problem definition/main results/conclusion），并基于这些有限片段做合理推断；方法细节和实验协议存在证据缺口。
