---
user_id: "cheng tan"
paper_id: 2714
arxiv_id: "2607.05382"
title: "Search Beyond What Can Be Taught: Evolving the Knowledge Boundary in Agentic Visual Generation"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.05382.pdf"
pdf_url: "https://arxiv.org/pdf/2607.05382"
abs_url: "https://arxiv.org/abs/2607.05382"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:08:00"
---
# Search Beyond What Can Be Taught: Evolving the Knowledge Boundary in Agentic Visual Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual generation · knowledge boundary · agentic generation · search-augmented generation

## 一句话总结

本文提出teach-then-search协同训练框架，通过发现生成器特定的知识边界，使视觉生成模型在应对长尾、演化世界知识请求时自适应地利用搜索工具，避免朴素搜索的噪声注入，在SearchGen-Bench上取得单调性能提升。

## 摘要

> Visual generators excel at rendering, but they confidently fabricate what they do not know. User requests are unbounded, evolving, and deeply long-tailed: new characters, trending entities, post-cutoff events, and more. This world-knowledge bottleneck is structural: generators are trained on fixed corpora, but the visual world is open-ended. We construct SearchGen-20K and SearchGen-Bench, with 20,839 prompts spanning twelve failure categories and twenty-two domains, paired with a pre-executed multimodal SearchGen-Corpus-1M to support offline, reproducible research. On SearchGen-Bench, frontier open generators score only 21 to 28 out of 100, a 40-point collapse invisible to existing benchmarks. The natural remedy is to employ search tools, enabling agentic visual generation. However, we find that naive search fails: it retrieves indiscriminately, injecting noise into prompts the generator already handles. We trace the root cause to a generator-specific, evolving knowledge boundary: the divide between what a generator can internalize through training and what must remain in external context. Although this boundary is hard to specify in advance, we show that it is discoverable through a teach-then-search co-training framework. Even a minimal version of this co-training recipe produces monotonic improvement, laying the foundation for recursive self-improvement in visual generation that can meet world-knowledge-grounded requests. We release the full dataset, co-training corpus, and search corpus as a replayable harness for tool-augmented, world-knowledge-grounded visual generation.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决视觉生成模型在面对超出训练分布的世界知识请求时产生的幻觉问题。具体来说：
1. 用户请求不断出现新角色、新实体、时效性事件等，而生成器的训练语料是固定的，无法覆盖这些长尾知识。
2. 现有基准仅评估生成器在常见概念上的能力，忽视了世界知识瓶颈，导致得分虚高（近40分虚高）。
3. 简单引入搜索工具（朴素搜索）会不加区分地检索，将噪声混入生成器已经能够处理的提示中，反而降低性能。
4. 核心挑战在于确定生成器知道什么和不知道什么，即发现其特定的知识边界，从而实现有选择的知识增强。

Q2: 有哪些相关研究？

相关工作包括以下方向：
1. 视觉生成模型：以Stable Diffusion、DALL-E、Imagen为代表的文本到图像生成模型在标准基准上表现优异，但知识局限于训练语料。
2. 检索增强生成：在语言领域被广泛研究，视觉领域也有如Re-Imagen等工作尝试将图像/知识库检索与生成结合，但大多假设检索内容总是有益的。
3. 智能体系统：赋予生成器工具使用能力（如搜索）的研究方兴未艾，但朴素集成可能导致负迁移。
4. 知识边界评估：已有工作尝试评估模型的知识边界（如模型知不知道自己不知道），但本文首次针对视觉生成提出系统基准和方法。
论文通过对比朴素搜索、无搜索和自适应搜索，揭示了现有方法的不足，并提出了协同训练来学习何时搜索。

Q3: 论文如何解决这个问题？

论文提出teach-then-search协同训练框架，核心步骤如下：
1. 知识边界发现：通过比较生成器在原始提示与搜索增强提示下的输出质量变化，识别生成器难以内化的知识类型。
2. 生成器自适应搜索（Generator-Adaptive Search）：训练一个搜索推理器（search reasoner），在给定提示时决定是否搜索、搜索什么，并通过拒绝采样微调（rejection-sampling finetuning）校准该推理器，使其只在生成器真正需要外部知识时触发搜索。
3. 协同训练：在训练推理器的同时，也更新生成器对搜索结果的利用能力，形成循环改进。最简版本为单次迭代，已能产生单调效果。方法的关键在于噪声抵抗的搜索协议，避免冗余信息干扰。

Q4: 论文做了哪些实验？

论文设计了以下实验：
1. 构建SearchGen-20K数据集：包含20,839条世界知识引导的文本到图像提示，覆盖12种失败类别（如新角色、趋势实体、截止后事件等）和22个领域（如医学、地理、文化等）。
2. 构建SearchGen-Bench：包含人工标注的参考答案和评分协议。
3. 提供预执行的多模态搜索语料库SearchGen-Corpus-1M，支持离线可重复研究。
4. 评估多个开源和商业生成器（如Stable Diffusion、DALL-E 3等）在SearchGen-Bench上的表现，并与现有基准（如T2I-CompBench、DPG-Bench）对比。
5. 对比三种搜索策略：无搜索、朴素搜索（盲目检索并拼接）、生成器自适应搜索（协同训练后）。
6. 使用人类评估和自动指标（可能基于参考图像相似度或语义匹配）评分。

Q5: 发现了什么实验现象？

实验揭示的关键现象：
1. 知识边界差距：在标准提示上得分接近的生成器（例如开源和商业模型均在70-80分），在SearchGen-Bench上得分仅21-28分，差距达40分，证实现有基准存在严重幻象。
2. 朴素搜索失效：朴素搜索不仅不能提升性能，反而会因注入无关噪声使得分下降（从28分降至更低）。
3. 协同训练单调提升：即使是单次迭代的生成器自适应搜索，也能在无搜索和朴素搜索之间取得最佳折中，得分逐步提高，且具有单调性（从无搜索到自适应搜索单调增加）。
4. 失败模式分布：数据集涵盖12种失败类别，可分析生成器在不同知识类型上的弱点（如时空知识、稀有实体等）。
5. 搜索推理器的行为：经过拒绝采样微调后，搜索推理器学会在不必要时保持沉默，在必要时准确检索。

Q6: 有什么可以进一步探索的点？

进一步探索的方向包括：
1. 递归自我改进：当前仅进行了单次迭代，可以研究多轮协同训练是否能进一步锐化知识边界并提升性能。
2. 跨生成器泛化：知识边界是针对特定生成器的，能否学到通用的边界预测器？
3. 多模态搜索源：当前搜索语料为预执行的多模态，未来可集成实时搜索。
4. 更复杂的数据集：扩展SearchGen-20K到更多领域和任务（如视频生成）。
5. 与其他推理策略结合：将协同训练与思维链、推理时扩展等方法结合。
6. 理论分析：知识边界的存在性、规模定律、收敛性等。
7. 应用落地：在事实性要求高的场景（如医疗、新闻图片生成）中实用化。

Q7: 总结一下论文的主要内容

这篇论文系统研究了视觉生成模型的世界知识瓶颈问题。作者首先论证了用户请求的开放性和长尾性，指出生成器的训练数据是固定的，因此对于新实体、趋势事件等无法正确生成，且会自信地产生幻觉。现有基准（如T2I-CompBench、DPG-Bench）只评估常见概念，导致生成器得分虚高（近40分虚高）。为了正视这一问题，作者构建了SearchGen-20K数据集和SearchGen-Bench基准，包含20,839条提示，覆盖12种失败类别和22个领域，并提供了预执行的多模态搜索语料库以支持离线研究。在SearchGen-Bench上的评估显示，前沿开放生成器仅得21-28分，商业模型也仅得41-58分，远低于标准基准的分数。

基于此，作者探索了使用搜索工具进行知识增强的路径。然而，简单的检索-拼接策略（朴素搜索）会不加选择地注入信息，反而干扰生成器的正常输出。作者将原因归结为生成器存在一个特定的、演化的知识边界：某些知识可通过训练内化，某些必须保留在外部上下文中。这个边界难以预先指定，但可以通过teach-then-search协同训练框架发现。

该框架包括两个阶段：首先是“teach”阶段，评估生成器的当前知识边界；然后是“search”阶段，利用拒绝采样微调训练一个搜索推理器，使其只搜索生成器确实不知道的信息，从而避免噪声。经过单次迭代的协同训练后，搜索推理器能够实现生成器自适应搜索，在SearchGen-Bench上产生从无搜索到自适应搜索的单调性能提升。论文将这一框架定位为递归自我改进的基础，未来可以通过多轮迭代进一步优化。

实验部分详细对比了无搜索、朴素搜索和自适应搜索的设置，验证了自适应搜索的有效性。消融研究可能分析了搜索时机、检索数量等因素。总体而言，这篇论文对于推动世界知识引导的视觉生成具有重要价值，提供了基准、方法和开放资源。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体视觉生成方向直接相关，特别是工具使用和自适应推理。

## 基本信息

- 作者：Haozhe Wang, Weijia Feng, Jinpeng Yu, Che Liu, Ping Nie, Fangzhen Lin, Jiaming Liu, Ruihua Huang, Jimmy Lin, Wenhu Chen, Cong Wei
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.05382`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF语义检索证据片段（Abstract、Contributions、Method、Results等部分）以及heuristic_draft，所有内容基于已有证据进行合理推断和改写。
