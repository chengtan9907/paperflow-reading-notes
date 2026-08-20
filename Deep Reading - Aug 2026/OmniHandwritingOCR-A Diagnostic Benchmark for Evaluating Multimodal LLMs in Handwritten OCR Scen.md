---
user_id: "cheng tan"
paper_id: 8647
arxiv_id: "2608.18586"
title: "OmniHandwritingOCR: A Diagnostic Benchmark for Evaluating Multimodal LLMs in Handwritten OCR Scenarios"
publish_date: "2026-08-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.18586.pdf"
pdf_url: "https://arxiv.org/pdf/2608.18586"
abs_url: "https://arxiv.org/abs/2608.18586"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-20T11:29:12"
---
# OmniHandwritingOCR: A Diagnostic Benchmark for Evaluating Multimodal LLMs in Handwritten OCR Scenarios

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：handwritten ocr · multimodal llm evaluation · benchmark · mathematical expression recognition

## 一句话总结

本论文提出 OmniHandwritingOCR，一个用于评测多模态大语言模型（MLLM）与 OCR 系统手写识别能力的诊断性基准，覆盖多语言手写文本与难度分层的多行数学公式，并基于 13 个开源/闭源系统、5 种互补指标在统一协议下进行评测。

## 摘要

> Multimodal large language models (MLLMs) are increasingly used as OCR systems in document and knowledge-processing pipelines, but their ability to faithfully read real handwriting remains underexplored. Existing OCR benchmarks focus largely on printed text or clean single-line inputs, leaving limited coverage of realistic handwritten OCR scenarios such as multilingual handwriting, writer errors, and structurally complex mathematical expressions. We introduce OmniHandwritingOCR, a diagnostic benchmark for evaluating MLLMs and OCR systems on handwritten OCR. It
> Permission to make digital or hard copies of all or part of this work for personal or classroom use is granted without fee provided that copies are not made or distributed for profit or commercial advantage and that copies bear this notice and the full citation on the first page. Copyrights for components of this work owned by others than the author(s) must be honored. Abstracting with credit is permitted. To copy otherwise, or republish, to post on servers or to redistribute to lists, requires prior specific permission and/or a fee. Request permissions from permissions@acm.org.
> CIKM '26, Rome, ITALY
> © 2026 Copyright held by the owner/author(s). Publication rights licensed to ACM. ACM ISBN 978-x-xxxx-xxxx-x/YYYY/MM
> https://doi.org/10.1145/nnnnnnn.nnnnnnn
> covers handwritten text recognition and handwritten mathematical expression recognition across six subtasks and twelve subsets, totaling 77.57K labeled images from public datasets and newly collected student writings. A key component is a difficulty-stratified multi-line formula corpus designed to test robustness under increasing structural complexity. We evaluate thirteen open- and closed-source systems with five complementary metrics under a unified protocol. Results show that current systems remain far from faithful transcription: performance drops sharply on complex multi-line formulas, model rankings vary across language and formula settings, and several generative models hallucinate plausible but visually unsupported corrections. OmniHandwritingOCR provides a challenging testbed for diagnosing language, content, structural, and visual-grounding failure modes of multimodal models in handwritten OCR scenarios. The code is available at https://github.com/ECNU-RAIL/OmniHandwritingOCR-CIKM2026.

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：MLLM 被广泛当作 OCR 系统用于文档与知识处理时，其对真实手写内容的忠实识别能力未被充分验证，而现有评测体系存在明显盲区。具体表现在几个方面：
1. 任务覆盖不足：现有 OCR 基准大多围绕印刷文本或干净的单行输入设计，难以反映实际手写场景中常见的多语言书写、书写者个体差异/笔误、以及结构复杂的数学表达式。
2. 诊断性缺失：许多基准只提供排行榜分数，无法定位模型在何种任务或难度上失败，因而难以指导系统改进（合理推断：基于“diagnostic evaluation rather than only leaderboard ranking”的设计原则）。
3. 对 LLM 式 OCR 的适应性不足：LLM 的生成式转录与经典序列识别不同，可能引入幻觉、事实保留失败等新问题，而既有指标与协议未必能捕捉这些问题（基于关键词 hallucination）。
4. 复杂结构鲁棒性未测：多行、嵌套、跨行公式对视觉-语言模型的结构理解能力提出更高要求，现有基准缺乏系统性的难度分层。
5. 评测方法论问题：作者明确指出“这不仅是数据集覆盖问题，更是评测方法问题”，即需要从模型能力诊断的角度重构评测目标，而不是简单扩大数据规模。
综上，该工作试图填补手写 OCR 评测在任务多样性、难度感知和标准化评分上的空白，并揭示生成式模型在该场景下的系统性缺陷。

Q2: 有哪些相关研究？

从摘要与检索片段可归纳的相关研究方向包括：
1. 传统光学字符识别（OCR）基准：大量基准聚焦印刷文本或干净的单行输入，手写场景通常被简化为单行或孤立字符识别，缺乏对多行、多语言和噪声书写的覆盖。
2. 手写文本识别（HTR）数据集与评测：已有若干公开手写数据集，但往往覆盖语言和版式有限，缺乏对书写者错误的系统标注，也较少与 MLLM 结合进行生成式评测。
3. 手写数学表达式识别（HMER）：该方向常被作为独立任务研究，但其评测集与手写文本识别割裂，且通常不包含多行、跨行、嵌套等复杂结构，难度分层意识较弱。
4. 多模态大语言模型评测：近期涌现许多 MLLM 通用评测基准，但对手写 OCR 这一垂直场景覆盖不足，也缺少面向诊断的难度分层设计，多数仍以整体准确率为主。
5. 公式识别与难度分层：少量工作关注公式结构复杂度，但通常未与手写、多语言和多行场景结合，难度定义与下游诊断的联动也较薄弱。
6. 幻觉与事实保留评估：LLM 的幻觉问题在文本生成评测中受关注，但尚未系统迁移到手写 OCR 领域；本论文的关键词包含 hallucination，说明其将幻觉视为该任务的重要失败模式。
7. 评测协议与指标设计：近年来出现若干多指标互补、统一协议的工作，但面向手写 OCR 并融合难度分层的仍较少。
需要说明：摘要并未列出具体参考文献，上述分类是对研究布局的合理推断，具体引用关系需结合论文正文核对。

Q3: 论文如何解决这个问题？

论文的解决方案是构建 OmniHandwritingOCR——一个强调诊断能力而非仅排行榜的基准，并配以统一的评测协议。具体技术路线包括：
1. 任务设计：覆盖手写文本识别（HTR）与手写数学表达式识别（HMER），划分 6 个子任务、12 个子集，以体现现实场景中的多样性。设计的第一原则是任务多样性，因为真实手写 OCR 应用很少只涉及一条干净文本行，可能包含批注、涂改、多种语言混排等复杂情况。
2. 数据来源：整合公共数据集与新收集的学生手写内容，总计 77.57K 张标注图像；覆盖英文文本、中文文本和数学公式三类内容，保证多语言与多模态范围。
3. 难度分层：设计关键组件——多行公式语料库，按结构复杂度分层（如 easy/medium/hard），用于测试模型在复杂度递增下的鲁棒性。检索片段显示其中包含 ml-medium 约 15,488 张、ml-hard 约 12,999 张样本，说明该分层规模可观；具体分层标准在 3.3 节中有数据统计支撑。
4. 评测系统：选择 13 个开源与闭源 MLLM/OCR 系统，覆盖不同模型架构、参数规模和部署形式，保证比较的广泛性。
5. 指标设计：采用 5 种互补指标，在统一协议下评估，以避免单一指标的片面性；指标很可能涵盖字符级/表达式级正确率、结构匹配度以及事实保留程度等维度（基于任务性质合理推断），并支持对幻觉的检测。
6. 诊断导向：基准本身支持从任务、难度、语言等维度拆分结果，帮助研究者定位失败模式，而非只给出一个总体分数。

Q4: 论文做了哪些实验？

论文进行的实验设计如下：
1. 数据规模与构成：总计 77.57K 张标注图像，分为 6 个子任务、12 个子集；数据来自公共数据集以及新收集的学生手写内容，覆盖英文文本、中文文本与数学公式。
2. 公式难度语料：构建难度分层的多行公式语料库；从检索到的数据集统计片段可见，ml-medium 约 15,488 张，ml-hard 约 12,999 张（easy 数量未在检索片段中展示），整体规模足以支撑难度维度的统计分析。
3. 被测系统：13 个开源与闭源系统，涵盖当前主流 MLLM 与 OCR 方法。
4. 评测协议：使用统一协议，配合 5 种互补指标，对不同语言文本和公式识别进行系统评测；协议统一化是保证跨模型可比性的基础。
5. 任务与语言设置：包括英语文本、中文文本和数学公式三大类，并在其中细分单行、多行、复杂结构等条件，以考察不同维度的影响。
6. 对比维度：横跨语言（中英）与公式结构复杂度，观察模型排名的变化情况，这也呼应了摘要中“模型排名随语言和公式设置变化”的结论。
由于摘要与检索片段未提供逐系统逐指标的完整表格，实验细节（如具体模型名单、指标名称、难度分层阈值）需要回原文确认。

Q5: 发现了什么实验现象？

从摘要与检索片段可获得的实验观察包括：
1. 整体忠实性不足：当前系统在忠实转录方面表现远未达标，评测暴露了手写 OCR 任务的挑战性，尤其是面对真实学生作业、事实保留转录和复杂多行公式结构时，差距明显。
2. 难度敏感性：在复杂多行公式上，系统性能出现急剧下降，说明结构复杂度对现有模型是主要瓶颈；这也验证了难度分层语料的设计价值——仅看平均分数无法揭示这类退化。
3. 排名不稳定性：模型排名在不同语言和公式设置下发生变化，表明单一排行榜无法反映真实能力，系统对语言和结构存在领域偏好或过拟合。
4. 幻觉迹象：关键词表中包含 hallucination，结合摘要中“fact-preserving transcription”的表述，可合理推断模型在手写转录中存在事实保留失败或幻觉现象；这可能是生成式 LLM 特有的失败模式，但具体案例需阅读正文。
5. 评测盲区暴露：检索片段强调的限制——真实学生作业、事实保留转录、复杂多行公式——说明现有基准和模型在这些方面都存在系统性缺口，且该缺口会影响文档处理流水线的可靠性。
6. 指标张力：由于采用 5 种互补指标，可以预期不同指标间可能不一致（例如字符正确率高但结构匹配低），从而揭示模型的特定短板；不过具体指标间的关系未在摘要中给出。
需要指出：由于摘要在“and…”处截断，第 4、6 点属于基于关键词和语境的合理推断，具体实验证据需查阅论文完整结果部分。

Q6: 有什么可以进一步探索的点？

基于该基准的设计与发现，可进一步探索的方向包括：
1. 模型改进：利用难度分层数据训练对结构复杂度更鲁棒的 HMER 模型，例如引入结构化解码、公式语法约束或树形预测，提升多行/嵌套公式的识别能力。
2. 评测协议扩展：将统一的诊断性协议扩展到更多语言（如日文、阿拉伯文、数学符号混排）、更多手写风格（草稿、批注）和更多模态（图表、化学式），并纳入语义正确性而非仅字符级匹配。
3. 对抗幻觉：针对手写 OCR 中的幻觉问题，设计专门的对抗样本、校准策略或约束解码，提高转录的忠实性；可利用本基准作为幻觉压力测试集。
4. 课程学习：利用由易到难的公式语料设计课程式训练（curriculum learning），提升模型在复杂多行公式上的表现，并在公开数据集上验证泛化性。
5. 跨语言泛化：研究模型在中文、英文及其他语言手写上的迁移规律，探索多语言预训练目标或低资源手写识别方案。
6. 指标设计：探索更细粒度的、面向任务诊断的指标，例如结构编辑距离、层级混淆分析、事实保留率，并与人工评估对齐。
7. 误差传播分析：将该基准嵌入文档处理智能体（agent）流水线，验证 OCR 模块错误在下游知识提取、问答等任务中的传播效应，从而更全面地评估真实部署价值。
8. 基准维护与扩展：持续更新数据子集，纳入新场景和失败案例，形成类似“动态基准”的机制，防止模型过拟合。

Q7: 总结一下论文的主要内容

本文针对多模态大语言模型（MLLM）在手写 OCR 场景中忠实识别能力缺乏系统评测的问题，提出了 OmniHandwritingOCR 基准。论点的核心是：现有 OCR 基准过度聚焦印刷文本或干净单行输入，未能覆盖真实手写场景中的多语言、书写者错误和复杂数学表达式；同时，多数评测只给排行榜分数，缺乏诊断性，无法定位失败原因。因此，作者将评测目标从“排名”转向“诊断”，提出了围绕任务多样性、难度感知和标准化评分三大需求设计基准。

在数据与任务设置上，基准包含手写文本识别（HTR）与手写数学表达式识别（HMER），共 6 个子任务、12 个子集，总计 77.57K 张标注图像，数据来自公共数据集和新收集的学生手写内容，覆盖英文文本、中文文本和数学公式。尤其关键的是构建了难度分层（如 easy/medium/hard）的多行公式语料库，用于测试模型在结构复杂度递增下的鲁棒性；依据检索到的数据集统计，ml-medium 约 15,488 张、ml-hard 约 12,999 张，这说明该分层不仅有区分度，也有足够的统计样本支持结论。

评测方面，作者在统一协议下评估了 13 个开源与闭源系统，使用 5 种互补指标，以避免单一指标的误导。实验结果显示：当前系统与忠实转录的要求相去甚远；复杂多行公式上的性能急剧下降；模型排名在不同语言和公式设置之间发生变化，说明现有模型的泛化性和稳定性不足。结合关键词中的 hallucination 与摘要中的“fact-preserving transcription”，可合理推断模型在手写转录中还存在幻觉和事实保留问题。检索到的 Introduction 片段也确认，真实学生作业、事实保留转录、复杂多行公式结构是现有模型的突出短板。

总体上，本文的贡献在于：提出了一个面向手写 OCR 的诊断性评测基准，弥补了现有基准在任务覆盖和难度分层上的不足；给出了统一评测协议与多指标框架，并对 13 个系统进行了横向比较；揭示了 MLLM 在复杂结构、多语言和事实保留上的系统性短板。局限方面，数据来源和难度标注方式可能影响外部效度，具体限制需结合论文讨论；未来工作可在此基础上进一步研究复杂公式识别、幻觉抑制、跨语言泛化及面向智能体流水线的评测。对于研究者和工程师而言，该基准提供了一个可复用的评测工具箱，也为文档处理智能体的能力边界提供了更真实的衡量方式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：系统性工作组织范式：该论文以“诊断”而非“刷榜”为目标，设计了任务多样性、难度感知、标准化评分三原则，这种评测组织方式适合迁移到智能体能力评估中。

## 基本信息

- 作者：Zinuo Guo, Min Zhang, Bo Jiang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-08-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.18586`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract、Design Principles、Dataset Statistics、Introduction 与 Conclusion 等片段，并结合摘要与关键词进行合理推断；部分细节（如具体系统名单、完整指标定义）因未检索到原文而存在缺口。
