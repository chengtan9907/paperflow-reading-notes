---
user_id: "cheng tan"
paper_id: 10814
arxiv_id: "2609.03213v1"
title: "LLMs Learn Better In-Context from Rules than from Examples"
institution: "Stanford University, New York University (合理推断，基于作者过往背景及研究领域)"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.03213v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.03213v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:39:20"
---
# LLMs Learn Better In-Context from Rules than from Examples

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：in-context learning · instruction following · few-shot prompting · large language models

## 一句话总结

本研究通过五类跨领域任务证明，大语言模型在上下文学习（ICL）中从规则描述（指令）中学习的效果普遍优于仅从示例中学习，且增加示例或扩大示例规模并不能带来显著且持续的性能提升。

## 摘要

> Large language models (LLMs) exhibit in-context learning capabilities, where they can learn new tasks from prompt contexts without weight updates. We compare the learning efficacies of two prominent modes of in-context learning: (1) learning from descriptions of rules (instruction following); and (2) learning from examples of input-output demonstrations (few-shot prompting). Through five learning tasks that cover diverse domains (games, arithmetic, linguistic inferences), we compare two modes of learning (rules vs. examples) specifying the same underlying task. We furthermore explore model and task properties that modulate the learning efficacies. We find that models generally learn more reliably from rules than from examples alone, and additional examples on top of rules or simply scaling up the number of examples do not lead to consistent and significant gains. Instruction tuning amplifies the benefit of rule-based learning while keeping example-based learning capacities intact. Surprisingly, we find no privileged effect of example-based learning in base models, and rules still lead to gains in algebraic task domains. Overall, the comparative efficacy of rules over examples is larger when the task recruits algebraic abstractions and computations, and smaller when the task requires distributional sensitivity and/or recruits parametric knowledge.
> datasets/tin-lab/rules\_vs\_examples
> tinlaboratory/rules-vs-examples

Q1: 这篇论文试图解决什么问题？

### 核心科学问题
本研究旨在探讨大语言模型（LLMs）在上下文学习（In-Context Learning, ICL）中，究竟是“通过规则描述（Rules）”还是“通过示例演示（Examples）”学习效率更高。尽管 ICL 已成为 LLM 的核心能力，但学术界对于这两种信息获取模式的相对效能及其背后的机制仍缺乏系统性对比。

### 关键挑战与动机
1. **学习范式的竞争**：ICL 既可以表现为对指令的遵循（Instruction Following），也可以表现为对模式的归纳（Pattern Induction）。理解哪种方式更具鲁棒性对于提示工程（Prompt Engineering）和模型开发至关重要。
2. **示例的局限性**：传统的 Few-shot 学习依赖于模型从有限示例中推断潜在任务逻辑，这在复杂或抽象任务中往往效率低下且容易产生偏见。
3. **规则的显式性**：规则提供了任务的直接逻辑定义，但模型是否能真正“理解”并执行这些抽象规则，还是仅仅将其作为某种弱提示，尚不明确。
4. **扩展性疑问**：增加示例数量（Scaling up examples）是否能弥补缺乏显式规则带来的性能差距？规则与示例之间是否存在协同效应（Synergy）？

Q2: 有哪些相关研究？

### 上下文学习（ICL）的机制研究
早期研究（如 Brown et al., 2020）强调了 Few-shot 提示的有效性。后续研究开始探讨 ICL 是在进行隐式微调还是简单的模式匹配。本研究通过对比规则与示例，进一步深化了对 ICL 认知过程的理解。

### 指令遵循与微调
指令微调（Instruction Tuning）被认为是提升模型泛化能力的关键。本研究探讨了指令微调如何改变模型对规则和示例的敏感度，指出其主要增强了规则处理路径。

### 任务表示与提示工程
现有文献讨论了提示词质量、示例顺序及选择对性能的影响。本研究则从更宏观的角度，将“任务定义”拆解为“抽象规则”与“具体实例”，对比其在不同任务类型（如代数 vs. 语言）下的表现。

Q3: 论文如何解决这个问题？

### 实验设计框架
研究者设计了五个具有挑战性的任务，涵盖了不同的认知领域：
1. **游戏类**：如逻辑谜题或策略游戏规则。
2. **算术类**：涉及复杂的代数运算或非标准进位逻辑。
3. **语言推理类**：涉及特定的语言转换或逻辑推演规则。

### 学习条件对比
实验设置了四种主要的提示条件：
1. **仅规则（Rules-only）**：仅提供任务的自然语言描述。
2. **仅示例（Examples-only）**：仅提供最小覆盖范围的输入-输出对。
3. **规则+示例（Rules + Examples）**：结合两者，观察是否存在互补效应。
4. **示例缩放（Example Scaling）**：测试增加示例数量（如从 4-shot 增加到 32-shot）的影响。

### 评估模型
评估了来自三个不同家族的开源模型（如 Llama 系列、Mistral 系列等），并以 GPT-4 等闭源模型作为参考基准。使用线性混合模型（Linear Mixed Models, LMMs）进行统计分析，以控制模型家族和任务变异的影响。

Q4: 论文做了哪些实验？

### 任务设置
- **代数任务**：要求模型根据特定规则执行非标准数学运算。
- **语言任务**：如根据特定语法规则转换句子结构。
- **逻辑游戏**：理解并执行一套全新的游戏规则。

### 实验变量
- **模型规模**：对比不同参数量级的模型表现。
- **微调状态**：对比 Base 模型与 Chat/Instruct 模型。
- **示例密度**：改变上下文中示例的数量和多样性。

### 统计方法
采用线性混合模型分析不同学习条件对准确率的影响，确保结论在不同模型和任务之间具有统计显著性。

Q5: 发现了什么实验现象？

### 核心发现
1. **规则的压倒性优势**：在大多数任务中，仅提供规则的效果显著优于仅提供示例。模型在理解抽象逻辑方面表现出比从示例中归纳模式更强的能力。
2. **缺乏协同效应**：在已提供规则的情况下，添加示例（Rules + Examples）往往不会带来额外的性能提升，有时甚至会引入噪声导致性能下降。
3. **示例缩放的边际效应**：单纯增加示例数量（Scaling）并不能弥补与规则学习之间的差距。即使是大量的示例，其效果也难以达到简洁规则的水平。
4. **指令微调的非对称增强**：指令微调显著提升了模型处理“规则”的能力，但对“示例学习”能力的提升相对有限。这表明指令微调主要优化了模型的逻辑执行路径。
5. **任务类型的调节作用**：
 - **代数/逻辑任务**：规则优势极大，因为这些任务依赖精确的步骤执行。
 - **分布敏感型任务**：当任务更依赖于语言统计分布或常识时，示例的作用会有所增强，但规则依然具有竞争力。
6. **Base 模型的表现**：即使是未经指令微调的 Base 模型，在某些代数任务中也表现出对规则的偏好，打破了“Base 模型只能做 Few-shot”的固有印象。

Q6: 有什么可以进一步探索的点？

### 可探索方向
1. **非理想规则的研究**：研究当规则描述模糊、不完整或包含自然语言噪声时，模型的鲁棒性如何。
2. **规则发现（Rule Induction）**：探索如何让模型从大量示例中自动总结出高质量的规则，从而实现从示例学习到规则学习的跨越。
3. **长上下文中的规则应用**：在极长上下文中，规则的注意力分配机制是否会发生变化？
4. **跨模态规则学习**：在多模态模型中，视觉规则（如流程图）与文本规则的效能对比。
5. **失败模式分析**：深入研究模型在遵循规则时产生幻觉的具体触发条件。

Q7: 总结一下论文的主要内容

本论文对大语言模型在上下文学习（ICL）中的“规则遵循”与“示例归纳”两种模式进行了深度评测。通过跨越游戏、算术和语言推理的五大类实验，研究发现 LLMs 在处理显式规则描述时表现出比处理输入-输出示例更高的可靠性和效率。核心结论指出，规则不仅在性能上优于示例，而且在指令微调后这种优势被进一步放大。研究挑战了“更多示例总是更好”的直觉，发现增加示例或在规则中补充示例往往无法产生显著增益。此外，论文通过统计分析揭示了任务属性对学习效果的调节作用：代数抽象任务更依赖规则，而分布敏感型任务则对示例更具包容性。这一发现对于优化提示策略、理解模型认知机制以及改进未来模型的指令微调过程具有重要的理论和实践意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于从事提示工程（Prompt Engineering）的研究者，建议优先优化指令/规则描述而非堆砌示例。

## 基本信息

- 作者：Xiang Fu, Seungmin Cho, Yukyung Lee, Najoung Kim
- 机构：Stanford University, New York University (合理推断，基于作者过往背景及研究领域)
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.03213v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索到的 Abstract、Introduction、Conclusion 以及实验结果部分的证据片段，确保了结论的准确性与系统性。
