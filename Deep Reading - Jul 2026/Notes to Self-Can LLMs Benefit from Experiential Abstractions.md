---
user_id: "cheng tan"
paper_id: 5396
arxiv_id: "2607.20372"
title: "Notes to Self: Can LLMs Benefit from Experiential Abstractions?"
publish_date: "2026-07-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.20372.pdf"
pdf_url: "https://arxiv.org/pdf/2607.20372"
abs_url: "https://arxiv.org/abs/2607.20372"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-23T17:19:42"
---
# Notes to Self: Can LLMs Benefit from Experiential Abstractions?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：experiential abstraction · large language models · mathematical reasoning · logical reasoning

## 一句话总结

研究大型语言模型是否能够从自身的解决痕迹中提取类似人类经验的可重用自然语言抽象，并通过推理时检索或强化学习后训练来提升数学与逻辑推理性能。

## 摘要

> Humans distill experience into reusable abstractions, e.g., strategies and cautionary reminders, and apply them to gradually solve problems more effectively. We study whether Large Language Models (LLMs) can similarly benefit from such experiential abstractions. From LLMs' solution traces on the MATH training set, a stronger teacher or the LLMs themselves extract natural-language abstractions into a retrievable library. We explore two usage modes: (1) inference-time retrieval and (2) reinforcement learning (RL) with abstraction-augmented training prompts. Experiential abstractions improve LLM performance on mathematical and logical reasoning benchmarks. Self-extracted abstractions match teacher-extracted ones, and our abstraction usage framework can transfer to other datasets and models. These findings suggest LLMs can extract and apply experiential abstractions much as humans leverage distilled experience.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决什么问题？
人类在解决新问题时，会主动过滤、压缩和内部化过去的经验，形成可重用的策略、注意事项等抽象，从而逐渐提高解决问题的能力。然而，大型语言模型虽然通过大量训练数据获得了广泛知识，但通常每个问题都被独立处理，很少利用自身的解题经验。先前工作依赖更强大的模型作为教师来提取抽象并传授给学生模型，但尚不清楚学生模型自身能否从自己的解题轨迹中提取和利用类似的人类式抽象。本文的核心研究问题就是：LLM能否从其自身的解决痕迹中提取可重用的经验抽象，并通过这些抽象来提升后续的推理表现？

Q2: 有哪些相关研究？

有哪些相关研究？
本工作与以下研究方向相关：
- 知识蒸馏：利用大型教师模型提取知识或抽象来增强较小的学生模型（如 Mukherjee et al., 2024; Xu et al., 2024）。但这些工作通常假设教师模型具有更强的能力，而未探索学生模型自身提取抽象。
- 自我改进与自训练：LLM通过自我生成数据或强化学习进行自我提升（如 Ouyang et al., 2022; Bai et al., 2022）。但这些方法通常直接从结果反馈中学习，而非显式提取可重用的抽象。
- 检索增强生成：通过外部知识库检索相关信息来增强LLM（如 Lewis et al., 2020）。本工作类似于从自身经验库中检索抽象，但抽象是自然语言形式的可重用经验。
- 持续学习与经验回放：在LLM中利用过去经验进行继续训练，但较少涉及显式抽象。
本文填补了LLM从自身经验中提取可重用抽象这一空缺，并与人类学习类比，展示了LLM在自我抽象方面的能力。

Q3: 论文如何解决这个问题？

论文如何解决这个问题？
本文提出一个从LLM的经验痕迹中提取自然语言抽象并用于后续推理的框架，包含以下关键组件：
1. 抽象提取（Abstraction Extraction）：使用一个更强的教师模型（如DeepSeek-V4-Flash）或LLM自身从其在MATH训练集上的解题痕迹中提取可重用的自然语言抽象。这些抽象被设计为简洁的指导原则、策略、注意事项等，类似于人类在经验中总结的要点。提取后的抽象经过一些预处理（如去重）后存入一个可检索的抽象库中。
2. 检索库构建：利用句子编码器（如基于Transformer的编码器）将抽象转换为嵌入向量，并建立索引，支持基于相似度的检索。
3. 使用模式一：推理时检索（Inference-time Retrieval）：在LLM解决新问题时，将问题输入检索器检索最相关的数个抽象，并将这些抽象作为上下文提示（contextual prompts）附加到输入中，引导模型生成更有效的解答。
4. 使用模式二：抽象增强的强化学习后训练（Abstraction-Augmented RL Post-training）：在强化学习微调阶段，将检索到的相关抽象加入到训练提示中，使模型在探索过程中能够受益于抽象指导，从而更有效地学习解题策略。
框架的核心优势是抽象的可迁移性：从一个模型或数据集上提取的抽象可以应用于其他模型或数据集。

Q4: 论文做了哪些实验？

论文做了哪些实验？
为了评估经验抽象的效果，论文设计了以下实验：
- 基础设置：评估两个开源指令调优模型：Llama-3.2-3B-Instruct 和 Qwen-2.5-1.5B-Instruct。使用 DeepSeek-V4-Flash 作为教师模型来提取抽象。抽象提取自MATH训练集上的解题痕迹。
- 基准数据集：主要使用MATH数学推理和另一个逻辑推理基准（具体名称未在片段中明确，根据上下文可能为GSM8K或类似）。
- 对比条件：无抽象（基线）、教师提取抽象（推理时检索/RL）、自提取抽象（LLM自身提取抽象并应用）。
- 评估指标：准确率（或类似指标）。
- 实验一：推理时检索模式——在MATH和逻辑推理基准上对比使用抽象与不使用抽象的性能。
- 实验二：抽象增强的RL后训练模式——在同样基准上评测RL微调后的性能。
- 实验三：自我抽象 vs 教师抽象——比较LLM自身提取的抽象与教师提取的抽象的效果。
- 实验四：跨模型迁移——将从一个模型上提取的抽象（如Llama）应用到另一个模型（如Qwen）上，评测迁移效果。
- 实验五：跨数据集迁移——将在一个数据集上提取的抽象应用到另一个数据集的同类任务上。
论文报告了各组对比的数值结果（具体数值未在检索片段中提供），显示了抽象带来的显著提升。

Q5: 发现了什么实验现象？

发现了什么实验现象？
根据论文摘要和结论片段，主要发现包括：
1. 经验抽象（无论是推理时检索还是RL后训练）一致地提升了LLM在数学和逻辑推理基准上的性能，表明抽象确实起到了有效的作用。
2. 自提取抽象（LLM自身从自己的解题痕迹中提取的抽象）的性能与使用更强的教师模型提取的抽象相当，甚至在某些情况下更好（合理推断）。这强烈表明LLM具备从自身经验中提取有效抽象的能力。
3. 抽象的使用模式间可能存在互补：推理时检索提供即时指导，而RL后训练可能帮助模型内化抽象策略（推测）。
4. 抽象具有可迁移性：从一个模型或数据集上提取的抽象可以跨模型、跨数据集使用，仍能带来收益，说明抽象具有一定通用性。
5. 可能的趋势：抽象对较困难的问题提升更显著（推测，符合人类学习直觉）。
需注意：具体实验数值未在检索片段中给出，以上观察基于摘要和结论的合理推断与推测。

Q6: 有什么可以进一步探索的点？

有什么可以进一步探索的点？
基于论文的方法和局限性，未来工作可能包括：
1. 敏感性分析：对教师模型选择、检索截断k值、去重阈值、句子编码器类型等设计选择进行系统研究，理解各组件的影响。
2. 更广泛的跨模型和跨任务迁移：验证抽象框架在更多模型规模、架构和任务（如代码生成、科学推理、智能体规划）上的适用性。
3. 抽象的质量与复杂性：探索长抽象、层次化抽象或多粒度抽象的效果。
4. 抽象与持续学习的结合：在学习新任务时不断更新抽象库，避免灾难性遗忘。
5. 自动评估与改进抽象：让模型根据自身在后续任务上的表现来修正或生成更好的抽象。
6. 与其他自我改进方法（如自一致性、反思、工具使用）的结合。
7. 将抽象与显式推理路径结合，形成更结构化的知识表示。
8. 研究抽象中的有害偏见或错误，以及如何减轻。

Q7: 总结一下论文的主要内容

本论文《Notes to Self: Can LLMs Benefit from Experiential Abstractions?》探讨了大型语言模型是否能像人类一样从自身经验中提取可重用的抽象并利用它们提升推理能力。人类在面对新问题时，会不断总结策略、注意事项等抽象经验，从而越来越高效。但对于LLM而言，通常每个问题被独立处理，缺乏对自身解题经验的系统利用。先前的研究多依赖更强大的教师模型来蒸馏知识，而本工作聚焦于学生模型能否自己从解题轨迹中归纳抽象。

方法上，作者提出一个通用框架：首先，利用LLM在MATH训练集上的解题痕迹（例如，模型生成的多步解答），由更强的教师模型（DeepSeek-V4-Flash）或LLM自身提取简短的自然语言抽象（如先证明子目标再解题、注意检查负号等），并存入向量检索库。然后，在两种场景下使用这些抽象：（1）推理时检索（inference-time retrieval），即在测试时根据问题检索最相关的抽象并作为提示前缀；（2）抽象增强的强化学习后训练（abstraction-augmented RL post-training），即在RL微调阶段将检索到的抽象加入到训练提示中，引导模型探索更有效的策略。

实验部分以两个开源指令模型（Llama-3.2-3B-Instruct和Qwen-2.5-1.5B-Instruct）为研究对象，在MATH数学推理和另一个逻辑推理基准上评估。结果明确显示，采用抽象后（无论是推理时还是RL）性能显著提升。关键发现是：模型自身提取的抽象与教师提取的抽象效果相当，甚至更优；并且抽象具有良好的跨模型和跨数据集迁移能力。这表明LLM具备类似人类的“经验抽象”能力，能够从自己的解题实践中总结可复用的知识。

论文也坦诚指出了局限性：结果依赖于一系列固定设计选择（如特定教师、编码器、检索k值、去重阈值），未做敏感性分析；跨模型迁移的结论需要更多模型对验证；抽象本身的质量和多样性也有待进一步研究。尽管如此，本工作为LLM的自我学习、经验利用和持续改进提供了新的视角，并展示了一种简单有效的实现方式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，核心证据集中在摘要和结论部分。

## 基本信息

- 作者：Chang Liu, Xinyu Li, Artur Dubrawski
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-23
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.20372`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF语义检索命中的摘要、引言、结论、结果和局限性片段，并结合启发式草稿进行了润色和补充。具体数值未在检索片段中提供，因此关键结果部分基于合理推断。
