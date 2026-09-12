---
user_id: "cheng tan"
paper_id: 10428
arxiv_id: "2609.02074v1"
title: "CHIME: Credit-Aware Hierarchical Memory Evolution for Long-Horizon Agentic Planning"
institution: "阿里巴巴 (Alibaba Group)"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02074v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02074v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:33:22"
---
# CHIME: Credit-Aware Hierarchical Memory Evolution for Long-Horizon Agentic Planning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：agentic planning · self-evolving memory · credit assignment · long-horizon tasks

## 一句话总结

CHIME 提出了一种基于“先归因后记忆”原则的层次化记忆演化框架，通过分离规划与执行记忆库并引入信用分配机制，解决了长程智能体规划中的噪声反馈与信用分配难题。

## 摘要

> Planning is a central capability that enables agents to decompose complex long-horizon tasks into manageable steps. Test-time search and training-based methods improve planning but incur high inference costs or require expensive training data. Self-evolving memory instead accumulates reusable experience from agent interaction outcomes into an external memory bank, so planning capability keeps improving at inference time without parameter updates. However, existing self-evolving memory methods share an inherent credit assignment problem: they rely on final task outcomes as feedback, but such outcomes conflate plan quality with execution errors and environmental factors, so the accumulated planning experience is often biased and noisy. To address this problem, we propose Credit-Aware Hierarchical Memory Evolution (CHIME), a self-evolving memory framework that maintains a separate planning bank and execution bank and follows an attribute-before-memorize principle: CHIME first attributes each task outcome to the plan, the execution, both, or neither, and then updates only the corresponding memory bank. Extensive experiments on four long-horizon agent benchmarks show that CHIME consistently outperforms state-of-the-art training-based and self-evolving memory baselines. Further analyses reveal several interesting findings. For example, CHIME accumulates effective memory with far fewer items. In addition, the learned memory values faithfully reflect downstream utility: high-quality planning memories are more valuable than execution memories. Finally, the accumulated memory effectively transfers across backbone models. Code will be released at https://github.com/ATH-MaaS/Marco-DeepResearch.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：长程规划中的信用分配难题
在长程智能体任务中，任务的成功或失败往往是由一系列复杂的决策和操作共同决定的。现有的自演化记忆方法通常采用“结果导向”的更新策略，即根据任务的最终成败来决定是否存储整个交互轨迹。这种做法存在以下深层问题：
1. **反馈混淆（Feedback Conflation）**：最终的失败可能是因为规划（Planning）逻辑错误，也可能是因为具体的执行（Execution）动作失误，或者是环境的随机性。将失败简单归咎于整个过程会导致记忆库中充斥着错误的负面信号。
2. **噪声积累**：如果一个优秀的规划因为执行阶段的微小扰动而失败，现有的方法可能会将其标记为“坏经验”而丢弃或负向强化；反之，一个拙劣的规划可能因为运气好而成功，从而污染记忆库。
3. **缺乏粒度**：规划（高层逻辑）和执行（底层操作）在知识属性上存在本质差异，统一存储无法实现针对性的优化。

### 现有方案的局限性
* **测试时搜索（Test-time Search）**：如 MCTS，虽然能提升性能，但每一步都需要大量的模型调用，推理成本极高。
* **基于训练的方法**：需要大规模的高质量微调数据，且模型一旦训练完成，在推理阶段的演化能力受限。
* **朴素记忆方法**：缺乏对经验质量的精细化评估，导致记忆库膨胀且检索效率低下。

Q2: 有哪些相关研究？

### 智能体规划与记忆演化
1. **智能体规划（Agentic Planning）**：早期的研究侧重于 Chain-of-Thought (CoT) 或 ReAct 等提示词工程。随后，Tree-of-Thought (ToT) 和搜索算法被引入以处理复杂任务。CHIME 属于在推理阶段通过外部结构增强规划能力的范畴。
2. **自演化记忆（Self-evolving Memory）**：这一领域旨在让智能体从过去的经验中学习。代表性工作包括通过成功轨迹进行上下文学习（ICL）或构建外部技能库。然而，这些方法大多忽略了规划与执行之间的解耦。
3. **信用分配（Credit Assignment）**：在强化学习中这是一个经典问题。本文将这一概念引入到基于大模型的记忆演化中，通过显式的归因逻辑来决定记忆的去向，这在智能体领域是一个相对新颖的切入点。

Q3: 论文如何解决这个问题？

### CHIME 框架设计核心
CHIME 采用了层次化架构和精细的归因机制，具体包含以下三个关键组件：

#### 1. 层次化记忆库（Hierarchical Memory Bank）
* **规划库（Planning Bank）**：存储高层的任务分解逻辑、子目标序列及其对应的价值评分。它关注“做什么”。
* **执行库（Execution Bank）**：存储具体的动作序列、工具调用参数及其在特定环境下的表现。它关注“怎么做”。

#### 2. 信用归因门控（Credit Attribution Gate）
这是 CHIME 的核心创新，遵循“先归因后记忆”原则：
* **多维评估**：在任务结束后，利用 LLM 作为评判者，结合任务目标、规划路径、执行日志和最终结果进行深度反思。
* **四种归因状态**：
 * **规划错误**：仅更新规划库，记录失败教训。
 * **执行错误**：仅更新执行库，记录操作失误。
 * **共同错误/成功**：同时更新两个库。
 * **环境/随机因素**：不进行记忆更新，避免引入噪声。

#### 3. 价值导向的检索与重排（Value-based Retrieval & Reranking）
* **相似度检索**：基于当前任务描述，从库中检索语义相关的记忆片段。
* **价值重排**：不仅考虑相关性，还根据记忆条目在历史中的“贡献度”（Value Score）进行重排，确保最有效、最可靠的经验被优先提取给智能体参考。

Q4: 论文做了哪些实验？

### 实验设置
* **基准数据集**：选择了四个具有挑战性的长程智能体基准，涵盖了不同的任务领域（如 Web 操作、工具使用、科学发现等）。
* **对比基线**：
 * **Base Models**：如 Qwen3.5-Flash, GPT-4o-mini 等。
 * **Training-based**：经过特定规划数据微调的模型。
 * **Self-evolving Baselines**：如 MemPrompt, Voyager 等基于结果反馈的记忆方法。
* **评估指标**：任务成功率（SR）、规划准确度、记忆效率（单位记忆提升的性能）。

### 实验流程
1. **热启动阶段**：智能体在训练集上进行初步交互，积累初始记忆。
2. **演化阶段**：通过 CHIME 的归因机制不断精炼记忆库。
3. **测试阶段**：在未见过的任务（Eval Split）上验证记忆的泛化能力和跨模型迁移能力。

Q5: 发现了什么实验现象？

### 关键发现与实验现象
1. **性能领先**：CHIME 在所有基准测试中均显著优于基于结果的记忆方法，证明了信用分配在减少记忆噪声方面的有效性。
2. **记忆效率（Efficiency）**：CHIME 仅需较少的记忆条目即可达到甚至超过基线方法在大规模记忆库下的表现。这表明“高质量的少量记忆”优于“低质量的海量记忆”。
3. **价值不对称性**：实验发现，**规划记忆的价值通常高于执行记忆**。高质量的规划能显著提升长程任务的成功率，而执行记忆的提升效果在环境多变时相对有限。
4. **跨模型迁移性**：在 Qwen 模型上积累的记忆，直接应用到 GPT 或其他模型时，依然能保持显著的性能提升，说明 CHIME 提取的是通用的规划知识而非模型特定的偏见。
5. **失败模式分析**：在消融实验中，如果关闭归因门控（即退化为传统方法），智能体会因为错误地吸收了执行失败的规划经验，导致在后续任务中出现“过度谨慎”或“逻辑死循环”的现象。

Q6: 有什么可以进一步探索的点？

### 可探索的方向
1. **动态归因粒度**：目前归因是在任务结束后的整体评估，未来可以探索在任务执行过程中的实时、步级（Step-level）信用分配。
2. **记忆压缩与抽象**：随着交互增加，记忆库仍会增长。研究如何将具体的记忆条目抽象为更高层的“策略”或“规则”是进一步提升效率的关键。
3. **多智能体协同演化**：探索多个智能体如何共享和共同演化这个层次化记忆库，实现群体智能的提升。
4. **复杂环境下的鲁棒性**：在极高噪声或部分可观测环境下，如何更准确地识别“环境因素”导致的失败，避免误导记忆更新。

Q7: 总结一下论文的主要内容

本文针对长程智能体规划中的“信用分配”难题，提出了 CHIME 框架。该研究的核心逻辑在于：智能体的失败不应被简单地归结为整体轨迹的失效，而应区分是“想错了”（规划问题）还是“做错了”（执行问题）。通过构建层次化的规划库与执行库，并引入一个基于 LLM 的归因门控，CHIME 实现了对交互经验的精细化筛选与存储。实验证明，这种“先归因后记忆”的策略不仅显著提升了智能体在复杂任务中的成功率，还大幅提高了记忆的利用效率和跨模型的泛化能力。CHIME 为构建能够在推理阶段持续自我进化的智能系统提供了一种高效且低成本的路径，避免了昂贵的重新训练过程，同时解决了传统记忆方法中普遍存在的噪声干扰问题。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文直接关联智能体（Agent）核心能力，特别是长程规划与自我进化。

## 基本信息

- 作者：Yongshi Ye, Tian Lan, Feihu Jiang, Muyang Ye, Bin Zhu, Qianghuai Jia, Longyue Wang, Zhao Xu, Weihua Luo, Xiaodong Shi
- 机构：阿里巴巴 (Alibaba Group)
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.02074v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是关于 CHIME 框架的归因机制、层次化结构以及实验中的记忆价值分析。
