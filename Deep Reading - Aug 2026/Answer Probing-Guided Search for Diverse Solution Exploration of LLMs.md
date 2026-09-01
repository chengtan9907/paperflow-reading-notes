---
user_id: "cheng tan"
paper_id: 10123
arxiv_id: "2608.30345v1"
title: "Answer Probing-Guided Search for Diverse Solution Exploration of LLMs"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30345v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30345v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:51:14"
---
# Answer Probing-Guided Search for Diverse Solution Exploration of LLMs

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：diverse generation · tree search · answer probing · hidden states

## 一句话总结

提出 Answer Probing 方法，通过探测中间推理路径可能到达的答案，利用探测答案的隐藏状态相似性和困惑度引导树搜索，从而在较少影响准确率的情况下一致提升 LLM 生成解的多样性。

## 摘要

> Generating multiple diverse and high-quality solutions is valuable for many applications, such as code-test generation and drug discovery. However, Large Language Models (LLMs) tend to converge on a single high-confidence solution during inference, limiting exploration of alternative valid solution paths. Existing test-time methods promote diversity through tree-like search and prune semantically similar branches using response-level semantic embeddings. However, we find that such embeddings are easily confounded by linguistic and stylistic similarities, making it difficult to distinguish genuinely distinct solution paths. To address this, we introduce Answer Probing, which probes the potential answer an LLM would reach from an intermediate reasoning path. We demonstrate that the hidden states of probed answers more effectively differentiate distinct solution paths than semantic embeddings, and the perplexity of probed answers serves as a practical proxy for reasoning correctness. Based on these findings, we propose Answer Probing-Guided Tree Search (APTS), which guides the tree search by the probed answers' hidden state similarity and perplexity. Experiments on three reasoning tasks across two LLMs show that APTS consistently enhances solution diversity, demonstrating its effectiveness and robustness.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：LLM 在自回归解码时，概率分布集中在少数高置信度模式上，导致多次采样或 beam search 得到的候选解高度同质，尤其在开放生成任务中很难覆盖多条有效路径。
2. 价值背景：多样且高质量的解对于需要备选方案的应用很关键，例如代码测试生成需要多个测试用例覆盖不同分支，药物发现需要探索不同的分子候选。
3. 现有方法的不足：以树搜索为代表的 test-time 方法通过分步探索多条路径来增加多样性，但剪枝操作依赖响应级语义嵌入，将语义上"看似相近"的分支合并；然而这类嵌入对语言表达、措辞、风格等表层特征敏感，容易把本质上不同的推理路径误判为相似，从而过早截断真正多样的分支。
4. 因此，论文试图解决的核心问题是：如何在树搜索中定义一个更本质的"路径差异性"度量，以及如何在不损伤正确性的前提下衡量候选路径的质量，从而在有限计算预算内保留更多真实多样的解。
（注：问题3中的"响应级语义嵌入"是摘要明确提到的；问题4是论文方法的动机。）

Q2: 有哪些相关研究？

1. 提示侧多样化：已有工作通过改变输入提示、示例或指令来促使模型从不同角度看问题（如 Naik et al., 2023; Handa et al., 2026），这类方法简单但受限于模型对提示的敏感程度，且难以保证输出的结构差异。
2. 解码/搜索侧多样化：另一类工作直接作用于解码过程，例如采用多样化的采样策略（temperature、top-p）、候选重排、或树搜索（如 MCTS 变体）来生成多条路径；这些方法通常通过某种相似性度量来合并或剪除冗余分支，其中响应级语义嵌入是常见做法。
3. 语义嵌入剪枝的局限：语义嵌入基于完整响应的句子级表示，当两个分支只是措辞不同但答案方向一致时，嵌入会将其聚在一起，而两个真正解法不同但修辞相似的分支则可能被误组，导致剪枝失效。
4. 与本文的区别：论文提出不依赖完整响应，而是从中间推理状态直接探测"潜在答案"，用答案级隐藏状态和困惑度两个信号服务搜索，从而绕开语言风格混淆问题。
（注：1中的引用来自证据片段；2-3是摘要与片段的结合；4是合理推断。）

Q3: 论文如何解决这个问题？

APTS 由两个关键组件构成：
1. Answer Probing：给定一个尚未完成的推理路径（中间状态），设计一个 probe（探测头）来预测该路径最可能推理出的最终答案。probing 可以从 LLM 的隐藏状态中抽取信息，输出一个"答案表示"。论文的核心发现是：这些探测出的答案的隐藏状态，比原始语义嵌入更能体现路径的本质差异。
2. 两种信号：
 - 多样性信号：探测答案的隐藏状态相似度。该相似度用于判断两个候选分支是否导向真正不同的答案，从而避免重复探索。
 - 质量信号：探测答案的困惑度（PPL）。困惑度近似反映该路径最终产出答案的概率，可作为正确性的代理——更低的 PPL 通常意味着更可能的正确路径。
3. 树搜索过程：从初始节点开始，在每个搜索深度采样 K 个候选推理路径；对每个候选做 Answer Probing，得到其潜在答案及两个信号；根据质量信号筛选出高质量节点，再根据多样性信号选择与已保留节点差异最大的若干节点，形成下一层；重复直至生成完整解。
4. 平衡质量与多样性：搜索中显式控制质量阈值和多样性惩罚之间的权衡，避免为了多样性而牺牲过多正确性。
（注：具体搜索细节（如候选数量、合并策略）未在可用证据中给出，属于合理推断。）

Q4: 论文做了哪些实验？

1. 任务：论文使用三个跨领域推理任务，但具体任务名称与数据集未在摘要中提供。合理推断可能涉及数学推理、常识问答、科学推理等常见 benchmark。
2. 模型：两个 LLM 作为生成器，具体名称未知。
3. 评估指标：多样性指标（如不同解数量、n-gram 多样性、语义多样性等）和准确率（或解答正确率）。
4. 对比方法：可能包括标准解码、beam search、语义嵌入剪枝的树搜索等，但未明确列出。
5. 实验设置：搜索深度、每层采样数、probe 的实现方式等细节缺失。
6. 由于可用证据仅覆盖摘要和方法引言，以上信息均为已知范围，缺失部分需查阅原文确认。

Q5: 发现了什么实验现象？

1. 主要结果：APTS 在三个推理任务和两个 LLM 上均一致提升了解多样性，说明其有效性具有跨任务和跨模型的鲁棒性。
2. 准确率影响：论文指出 APTS 对准确率只有"modest impact"（较小的负面影响），这意味着多样性提升不是以显著牺牲正确性为代价——这是一个重要的正面权衡。
3. 隐含的分析：论文可能还做了对 Answer Probing 有效性的消融实验，比如对比语义嵌入与答案级隐藏状态在区分路径上的能力，但具体证据未在本次检索片段中出现。
4. 没有看到的：未提供具体数值、失败案例或反直觉现象；需要阅读实验章节才能获取。

Q6: 有什么可以进一步探索的点？

1. 计算开销优化：APTS 每个深度都要采样多条路径并做 probing，训练/推理开销较大；可以探索更高效的探测方法，例如轻量探测头或减少采样数量的自适应策略。
2. 更广泛的场景：当前在推理任务上验证，可尝试代码生成、创意写作、对话生成等需要多样性的场景。
3. 与提示多样化结合：将 prompt-side 变化与 APTS 结合，可能进一步提升探索广度。
4. 可解释性分析：研究答案级隐藏状态为何能区分路径，是否编码了语义或规划信息，有助于理论理解。
5. 更好的质量信号：困惑度只是代理，可设计更细粒度的正确性预测器来减少误判。
6. 动态预算分配：根据搜索难度动态调整树宽和深度，以平衡计算和多样性。
（注：以上均为推测，论文本身可能未涉及。）

Q7: 总结一下论文的主要内容

论文围绕 LLM 生成多样且高质量解的问题展开。作者首先指出多样化解在许多应用（如代码测试生成、药物发现）中的重要性，并揭示 LLM 在解码时存在高置信度收敛倾向，导致生成结果趋同。现有 test-time 多样化方法，尤其是树搜索方法，虽然能探索多分支，但通常依赖响应级语义嵌入来剪除相似分支；然而这种嵌入容易混淆语言风格相似而解法不同的情况，使得真正的多样性丢失。基于这一观察，作者提出 Answer Probing，从中间推理路径直接探测 LLM 可能给出的答案，并系统验证了探测答案的隐藏状态比响应级语义嵌入更能区分不同解路径，同时探测答案的困惑度可作为推理正确性的实用代理。基于这两个信号，作者设计了 APTS 树搜索算法：每个搜索深度对候选节点进行 probing，用隐藏状态相似度作为多样性信号，用困惑度作为质量信号，筛选出高质量且彼此差异大的路径继续扩展。这种设计使得搜索过程既能保留真正多样的解题方案，又不至于牺牲过多准确性。在三个跨领域推理任务和两个 LLM 上的实验表明，APTS 稳定提升了解多样性，且准确率损失较小，验证了其有效性和鲁棒性。论文的主要贡献在于提出了一种更本质的路径差异性度量（答案级隐藏状态）和一种可行的质量代理（答案 PPL），并将二者有机整合进树搜索框架，为 test-time 多样化生成提供了新思路。同时，论文也明确讨论了局限性，即每一步需要采样多条路径并进行 probing，计算开销较大。整体而言，该工作对理解 LLM 隐藏状态的内涵以及构建多样化解生成系统具有启发意义，但受限于可用证据，具体任务、模型和数值细节需要查阅原文。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向直接相关，解决了 LLM 多样解生成的核心问题。

## 基本信息

- 作者：Yi Fang, Que Shen, Chengpeng Li, Boyi Deng, Wei Shi, Wenjie Wang, Fuli Feng, Fengli Xu, Dayiheng Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30345v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的证据片段（Abstract、Method、Related Work、Limitations等），并结合论文元数据，未编造具体数值。
