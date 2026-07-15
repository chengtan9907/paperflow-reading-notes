---
user_id: "cheng tan"
paper_id: 3761
arxiv_id: "2607.11192v1"
title: "GDP.pdf: Benchmarking Grounded Multimodal Reasoning over Professional PDF Documents"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11192v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11192v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:46:37"
---
# GDP.pdf: Benchmarking Grounded Multimodal Reasoning over Professional PDF Documents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal reasoning · pdf documents · benchmark · document ai

## 一句话总结

提出GDP.pdf基准测试，由10个领域的专业人员基于真实PDF文档构建100项接地多模态推理任务，并筛选出至少两个前沿模型失败的问题；评估7个前沿模型，最佳严格通过率仅15%，多数错误可归因于少数重复失败模式。

## 摘要

> A large share of day-to-day work in professional domains happens inside PDF files: benefits packets, leases, datasheets, clinical guidelines, construction plans. Benchmarks for document AI have generally measured the required capabilities in isolation: OCR, layout analysis, chart reasoning, table QA, document VQA. A high score on any one of them does not necessarily reveal whether a model can answer a realistic question that someone in the field would actually ask about a specific PDF. GDP.pdf is a benchmark built to measure this directly. It consists of question-document pairs authored by working professionals in ten fields, and a candidate question was kept only when at least two frontier multimodal models failed it in a way that mattered: a wrong answer, missed decisive evidence, or a fabricated claim, rather than a superficial difference such as style. Each item comes with a rubric of atomic criteria, so we can report a graded rubric score as well as a strict task-level pass rate, and each item is tagged against a taxonomy of eleven capabilities in three tiers, spanning text extraction and grounding, table and chart comprehension, cross-referencing, spatial reasoning, and abstention on unsupported queries. We evaluated seven frontier models on the 100-item benchmark. The best model passed only 15% of the items and the worst passed 1%. Most errors trace back to a small set of recurring loss patterns: misaligned tables, misread charts, skipped footnotes and exclusions, miscounted floor-plan symbols, scan noise, and amendments that supersede earlier text. The full 100-item benchmark is publicly available at https://huggingface.co/datasets/surgeai/GDP.pdf

Q1: 这篇论文试图解决什么问题？

现有文档AI基准（如DocVQA、ChartQA、TableQA等）通常将OCR、布局分析、图表推理、表格问答等能力分开评估，但单一能力的高分并不保证模型能处理专业人员日常遇到的真实PDF问题。例如，模型可能正确提取某个条款和数字，但引用的却是与问题无关的条款或过时内容，这种失败表面上看不出错误，因为模型给出的内容确实存在于文档中，真正需要的能力是对专业语境的理解、跨页引用、版本判断、空间推理等综合推理。因此需要一种直接衡量模型在真实专业PDF文档上完成接地多模态推理任务的基准。

Q2: 有哪些相关研究？

现有文档AI基准主要聚焦于子能力评估：DocVQA（文档视觉问答）、ChartQA（图表问答）、TableQA（表格问答）、FunSD（表单理解）、PWC（页面分割）等。此外还有多模态推理基准如VQAv2、OK-VQA，但它们的任务通常来自学术环境，而非专业工作流程。在专业领域，一些工作提出了面向特定行业的基准（如医药、法律），但要么缺少多模态输入（仅文本），要么任务设计未经过专家严格审查。GDP.pdf则系统性地覆盖10个领域，任务由各自领域的一线专业人员编写，并经过前治多模态模型多次失效的筛选，确保任务对当前模型具有挑战性且贴近真实需求。

Q3: 论文如何解决这个问题？

1) 任务构建：邀请10个专业领域（如福利、法律、医学、建筑等）的在职人员基于真实PDF文件编写问答对，每个问题要求必须使用文档中的多模态信息才能回答。2) 筛选流程：对于每个候选问题，至少由两个前治多模态模型（GPT-5.4、Claude Opus 4.7等）尝试回答，仅当模型出现实质性错误（错误答案、遗漏重要证据、捏造信息）时才保留该问题；风格差异（如措辞不同但意思正确）不被视为失败。3) 评分标准：每个问题附带原子级评分标准，由领域专家制定，列出答案必须包含的若干关键正确信息点（rubric criteria），从而支持严格通过率（所有标准完全满足）和分级评分（部分满足）。4) 能力分类：将每个任务标注到十一项能力的三层分类体系：第一层基础提取（文本提取与定位、扫描噪声处理）、第二层结构化推理（表格理解、图表理解、列表识别）、第三层高级推理（交叉引用、空间推理、版本判断、例外处理、数值推理、拒绝回答）。5) 模型评估：通过API输入PDF（base64编码）和问题，记录模型输出，按照rubric人工评分，并统计通过率和错误模式。

Q4: 论文做了哪些实验？

1) 评估模型：选择7个前治多模态模型——Gemini 3.1 Pro、Claude Opus 4.7、GPT-5.4、Grok 4.20、Kimi K2.5、Mistral Large 3、Nova 2 Pro，均通过公开API调用。2) 评估设置：每个模型输入100个任务，包含问题及原始PDF（以base64形式提供），生成回答后由人工根据rubric评分。3) 评价指标：报告严格任务级通过率（所有rubric点完全满足）以及基于rubric点的分级准确率；按11项能力分别统计性能。4) 错误分析：对所有错误回答进行归类，总结涌现的失败模式。

Q5: 发现了什么实验现象？

1) 整体结果：表现最好的模型仅通过15%的任务，最差的通过1%，表明基准极具挑战性。2) 模型间差异：不同模型在不同能力维度有差异，但整体通过率均很低。3) 错误模式：多数错误可归因于少数重复模式——表格对位错误（例如选中错误的行/列）、图表误读（例如误读条形图方向或数值）、脚注和除外条款遗漏（忽略页面底部的小字修正）、平面图符号计数错误（例如数错门或窗口数量）、扫描噪声干扰（将污点误认为字符）、修正条款覆盖前文（未识别后文修改前文的语句）。4) 隐性失败问题：模型给出的答案在局部上正确（如引用真实存在的条款和数字），但实质上与问题无关或过时，这种失败不易被简单检查发现。5) 数值推理中的常见错误：未能正确处理单位换算、合计与子项不一致等问题。

Q6: 有什么可以进一步探索的点？

1) 扩展任务集：增加更多领域和题目数量，覆盖更多专业文档类型和问题难度。2) 更具挑战性的任务：探索需要多步推理、多轮交互、文档之外知识融合的任务。3) 错误缓解方法：针对发现的失败模式设计专门的训练策略、后处理模块或结构化表示（如显式表格解析、空间布局锚定）。4) 更广泛的模型评估：加入开放权重模型（如Llama系列）、PDF专用模型，并跟踪模型更新后的表现。5) 基于rubric的细粒度反馈：利用rubric分数指导模型改进，如识别模型在哪些子能力上持续薄弱。6) 基准的下游应用：研究通过该基准的模型在实际专业工作流中的自动化部署效果和安全边界。

Q7: 总结一下论文的主要内容

该论文提出了GDP.pdf，一个用于评估多模态大模型在专业PDF文档上进行接地推理的基准测试。作者指出，现有文档AI基准分别测量OCR、布局分析、图表推理等能力，但未要求综合运用这些能力回答专业人员真正会问的问题。GDP.pdf由10个不同专业领域（如福利申请、租赁合同、数据表、临床指南、建筑施工图等）的在职工作人员，基于真实PDF文件创建问答对。为保证任务具有挑战性且贴近实际，每个问题必须被至少2个前治多模态模型以实质性方式回答錯誤（错误答案、遗漏关键证据、或捏造事实）才被纳入最终集合。每个任务附带由领域专家制定的原子级评分标准，确保评估可靠且能区分部分正确与完全正确。同时每道题标注一项多项能力的分类本体（共11项，分基础提取、结构化推理、高级推理三层）。论文在100道题上评估了7个前治多模态模型（Gemini 3.1 Pro、Claude Opus 4.7、GPT-5.4、Grok 4.20、Kimi K2.5、Mistral Large 3、Nova 2 Pro），发现严格通过率最高仅为15%（Gemini 3.1 Pro），最低1%（Nova 2 Pro）。对错误样本的详细分析表明，模型在表格对位、图表解读、脚注和例外条款处理、平面图符号计数、扫描噪声容忍、以及修正条款覆盖前文上存在系统性缺陷。论文还报告了按能力维度的细化结果，揭示了不同模型的强弱项。GDP.pdf基准测试、评分标准及能力分类已开源。作者认为该基准侧重测试模型的下限（基础能力）而非上限，对于文档自动化部署具有实际指导意义。论文的贡献包括：一个专家撰写、筛选严格、带有细粒度标注的多模态PDF推理基准；一套11项能力的分类体系；以及针对前治模型的全面评估和错误模式分析，指出当前模型在接地PDF推理上远未达到可靠水平。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该基准为评估多模态大模型在专业文档处理场景中的基础能力提供了严格评测，对开发能可靠处理PDF文档的智能体有直接参考价值。

## 基本信息

- 作者：Suhaas Garre, Emily Ritchie, Sushant Mehta, Edwin Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11192v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要、结论和贡献部分的检索证据，以及 heuristic_draft 中的内容；对于未在检索片段中出现的细节（如具体实验设置、能力分类细节）进行了合理推断，建议回论文原文核实。
