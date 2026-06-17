---
user_id: "cheng tan"
paper_id: 524
arxiv_id: "2606.17289v1"
title: "Nothing from Something: Can a Language Model Discover 0?"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.17289v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.17289v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:10:33"
---
# Nothing from Something: Can a Language Model Discover 0?

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：language model · zero discovery · arithmetic reasoning · generalization

## 一句话总结

本论文研究语言模型（GPT-2规模）能否独立发现数学概念“零”，通过简单加法任务测试其零样本泛化能力，发现模型无法直接泛化但经过少量示例训练后能显著提升，且语言预训练有助于这一过程。

## 摘要

> of mathematical data comparable in kind and complex-
> ity to their test problems (Lightman et al., 2023; Trinh
> AI systems based on artificial neural networks are being et al., 2024). Thus, while indicating exciting progress,
> developed with aspirations of pushing the boundary of
> these benchmark results do not demonstrate an ability
> human mathematical knowledge. A key question for these
> to go beyond the structures the models had already ac-
> systems is how much they can reach beyond their training
> quired during training. This would seem a prerequisite to data. Mathematical discovery requires a strong form of
> out of distribution generalization; the ability to hypothesize extending human mathematics in a meaningful way.
> genuinely new – and potentially logically more powerful – The question of if and when minds are capable of this2026 mathematical structures. It has been hypothesized that kind of generalization has long been studied in cognitive
> language abilities support such generalizations in human science. The developmental psychologist Jean Piaget
> cognition. In this work, we use simple arithmetic as a articulated a general theory of learning that is character-Jun
> case study for examining how modern AI models could ized by exactly this kind of leap from one formal system to
> another (Piaget, 1970). As characterized by Jerry Fodor,15 expandthese modelstheir mathematicalcan independentlyhorizons,discoverevaluatingthe conceptwhetherof this position holds that “[I]f you [characterize the compu-
> “zero”. We show that We show that (1) language models
> tational capacities of the organism] for several different
> of a GPT-2 size are unable to perform this generalization
> time slices of the organism ..., what you would get is
> at test time regardless of language pretraining, but (2)
> a fundamentally different galaxy of constraints on the models can improve substantially after training on tens
> or hundreds of examples of zero. Additionally, we find organism’s concepts. ...[T]he logic instantiated by the[cs.AI] that language pretraining reduces the number of required system of concepts at any ith stage is weaker than the
> examples by approximately 50%, showing that language logic instantiated by the i + 1th stage” (Piattelli-Palmarini
> abilities can scaffold mathematical discovery in neural et al., 1981). Fodor believed this kind of learning to be
> models. impossible. Formally, he argued there is no way to bridge
> the gap between the current universe of concepts and a

Q1: 这篇论文试图解决什么问题？

论文试图回答的核心问题是：现代语言模型（如GPT-2）能否在训练数据中缺少某一概念（如数字0）的情况下，在测试时自主发现并正确使用该概念？这本质上是对模型能否实现强外部泛化（out-of-distribution generalization）的检验，也是评估人工智能系统是否具备数学发现能力的关键。具体而言，论文以“零”为案例，因为零在算术中具有特殊地位（如加法的幺元），且在认知科学中常被作为概念演变的范例。

Q2: 有哪些相关研究？

相关工作涵盖两个层面：1) 认知科学中关于概念演变与语言支架作用的理论，如Piaget的认知发展理论及Fodor对语言支持概念跳跃的讨论；2) 机器学习中数学推理的基准评估（如Lightman et al., 2023; Trinh et al., 2024），这些工作虽展示了模型在大量数学数据上的能力，但未证明其能超越训练时习得的结构。此外，近期研究尝试通过链式思维（Chain-of-Thought）等技术提升推理能力，但同样未直接关注概念层面的零样本发现。论文在算术情境中设置了严格条件，填补了该空白。

Q3: 论文如何解决这个问题？

论文设计了一个受控的加法任务：训练集仅包含两位或三位数加法，且任何操作数或结果中均不出现数字0（除非进位导致结果个位为0，但作为答案的一部分暴露少量0）。模型需在测试时处理包含0的加法问题（例如 3+7=? 答案10中包含0）。实验使用GPT-2规模（约1.5亿参数）的Transformer模型，比较三种初始化条件：随机初始化、在过滤后的OpenWebText上预训练、以及在未过滤的OpenWebText上预训练（后者可能见过0的更常见用法）。训练过程为50000步，每批64个样本，并评估测试集上的准确率和损失。后续还进行了少样本微调实验，即在训练集中加入不同数量的包含0的样本。

Q4: 论文做了哪些实验？

实验包含两个主要部分：1) 零样本泛化实验：模型仅在无0数据上训练，测试时评估其对包含0问题的准确率。所有模型（不同初始化）均未能泛化，准确率接近0%（随机猜测）。2) 少样本微调实验：在训练集中加入少量（如16、64、256个）包含0的样本后重新训练或微调，测量模型对0问题的准确率变化。此外，论文在八进制（base-8）条件下重复实验以排除基数效应，并分析了不同数字表示之间的相似性（如数字0和9的嵌入高相似性）对泛化的影响。

Q5: 发现了什么实验现象？

关键发现包括：(1) 零样本条件下所有模型均无法泛化至0，训练损失曲线清晰分离，证明模型未学到零的概念。(2) 少样本微调后，准确率随示例数量快速增长，且语言预训练模型（尤其是未过滤OpenWebText）的增速更快。例如，仅64个示例（占训练数据0.64%）即可使准确率显著提升，而随机初始化模型需要更多示例。(3) 在八进制设定中观察到类似趋势，表明结果不依赖十进制表示。(4) 数字表示分析发现，数字0和9的嵌入相似度较高，这与“0”可能被混淆为“9”有关，但仍未影响泛化规律。(5) 进位操作对泛化有显著影响，包含进位的题目（如个位相加产生0）难度更高。

Q6: 有什么可以进一步探索的点？

基于当前发现可探索的方向包括：(1) 扩展到更大模型（如GPT-3系列）以检验规模效应。(2) 使用链式思维推理或具有显式中间步骤的模型，观察是否能零样本发现零。(3) 尝试更多数学概念（如负数、无穷大），检验语言模型概念发现的一般性。(4) 设计更精细的训练数据分布，研究概念暴露频率与泛化阈值的关系。(5) 结合认知科学理论，设计干预实验验证语言支架假设（如通过自然语言提示引导模型发现零）。(6) 探索多任务学习或元学习范式是否能在更少示例下实现泛化。

Q7: 总结一下论文的主要内容

本论文以“零”的发现为案例，系统研究了语言模型在数学概念上的强外部泛化能力。论文首先指出，尽管当前AI在数学推理基准上表现出色，但尚未证明其能超越训练数据中的结构——即实现真正意义上的数学发现。受认知科学中Piaget和Fodor理论的启发，作者设定了一个严格控制的加法任务：训练数据中不出现数字0（除进位位外），测试模型在需要输出0或包含0的题目上的表现。实验采用GPT-2规模模型，并在多种预训练条件下进行对比。主要发现包括：零样本时所有模型完全无法泛化（准确率≈0%）；但通过加入少量（16-256个）包含零的示例进行微调，模型能够快速学习使用零，其中语言预训练模型（尤其是未过滤语料）表现出更强的学习效率。论文还在八进制下验证了结论的稳健性，并分析了数字表示相似性与进位操作对结果的影响。总体而言，这项工作为评估语言模型的概念发现能力提供了一个清晰的范式，并揭示了语言预训练在支持这种外推中的潜在作用。论文对未来研究如何实现真正的数学发现、以及如何将认知科学理论引入机器学习提出了重要问题。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与AI for Science方向相关，关注AI能否自主发现科学概念。

## 基本信息

- 作者：Phoebe Zeng, Thomas L. Griffiths, Brenden M. Lake
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.17289v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 基于PDF检索证据生成，主要参考了Results片段和Abstract片段。
