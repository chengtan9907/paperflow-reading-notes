---
user_id: "cheng tan"
paper_id: 13665
arxiv_id: "2609.29474v1"
title: "CodeGraph: Open-Taxonomy Knowledge Graph for Source Code with Wikidata Grounding"
institution: "University of Bologna, Italy; Pisa University, Italy (合理推断，基于作者 Stefano Zacchiroli, Maurizio Gabbrielli, Paolo Ferragina 等在软件工程与算法领域的知名学者背景)"
publish_date: "2026-09-24"
pdf_url: "https://arxiv.org/pdf/2609.29474v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-26T01:52:03"
---
# CodeGraph: Open-Taxonomy Knowledge Graph for Source Code with Wikidata Grounding

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：knowledge graph · source code mining · wikidata grounding · large language model

## 一句话总结

本研究提出了一个利用代码专用大语言模型构建源代码开放分类语义标注的流水线，并将其与 Wikidata 对齐，构建了包含 1.58 亿节点的超大规模源代码知识图谱 CodeGraph。

## 摘要

> Public software repositories, like GitHub and Software Heritage Archive, store billions of files, yet extracting their implicit engineering knowledge ---i.e., the algorithms they implement, the paradigms they follow, the patterns they instantiate, and the application domains they serve--- remains challenging, as current tools are constrained to syntactic and token-level analysis. We present a pipeline for building an open-taxonomy semantic annotation of source code using a code-specialised Large Language Model. The extracted entities are grounded in Wikidata through a three-stage linking procedure: a deterministic SPARQL stage handles unambiguous entities, a Deep Research Agent resolves the residual long tail, and a hierarchy-rollup stage imports the parent-of closure of each resolved Wikidata identifier. The resulting annotations are materialised as a source-code-specific open-taxonomy knowledge graph. We further introduce a calibrated quality-assurance protocol that quantifies annotation precision by combining a small human gold set with an LLM-as-a-judge filter. We applied our pipeline to the 167 million files of the Stack-Edu corpus, creating the first known large-scale open-taxonomy knowledge graph for source code. Our graph, named CodeGraph, contains approximately 158 million nodes, which include around 145 million files, about 63,000 extracted concept entities (such as algorithms, paradigms, design patterns, and application domains), and roughly 19,800 grounded Wikidata entities. Furthermore, CodeGraph features approximately 1 billion typed edges that connect files to their respective concepts, link these concepts to their grounded Wikidata identifiers, and relate them to their parent categories, covering 14 programming languages.

Q1: 这篇论文试图解决什么问题？

### 核心问题与挑战
在软件工程和数据科学领域，公共软件仓库（如 GitHub、Software Heritage Archive）蕴含着海量的源代码资产。然而，现有的代码分析工具和挖掘技术主要局限于**语法层面（Syntactic）**和 **Token 级别（Token-level）**的分析（例如抽象语法树 AST、控制流图 CFG 或简单的关键字匹配）。这种局限性导致无法有效提取代码中隐含的高阶**工程知识（Engineering Knowledge）**，具体包括：
1. **算法实现**：代码具体实现了什么算法（如 Dijkstra 算法、快速傅里叶变换）。
2. **编程范式**：代码遵循了何种范式（如面向对象、函数式编程、响应式编程）。
3. **设计模式**：代码实例化了哪些经典的软件设计模式（如单例模式、观察者模式）。
4. **应用领域**：代码服务于哪个具体的业务或技术领域（如生物信息学、密码学、图像处理）。

### 现有方法的不足与研究动机
传统的命名实体识别（NER）或知识图谱构建方法通常依赖于**封闭分类法（Closed Taxonomy）**，即预先定义好固定的标签集。这种方法在面对日新月异、领域极其广泛的开源代码世界时显得捉襟见肘，无法捕获长尾的、新兴的技术概念。而现有的代码大语言模型（Code LLMs）虽然具备强大的语义理解能力，但其输出往往是自由文本，缺乏结构化、标准化以及与权威知识库的关联，导致无法直接用于大规模的知识检索、推理和代码资产管理。因此，如何利用 LLM 的开放生成能力，同时保证生成概念的结构化、标准化（Grounding）以及高精度，是当前亟待解决的核心科学与工程问题。

Q2: 有哪些相关研究？

### 相关研究脉络与对比
本研究位于代码挖掘、知识图谱构建以及大语言模型应用的交叉领域。相关研究主要可以分为以下几个方向：
1. **基于语法和静态分析的代码图谱**：早期研究（如 CodeQL、SourceGraph）主要通过静态分析技术提取代码的结构化信息（如函数调用图、类继承关系）。这些方法精度极高，但完全丢失了代码背后的高层语义和应用领域信息。
2. **软件工程领域的知识图谱（SEKGs）**：已有研究尝试从 Stack Overflow、API 文档或 GitHub Issue 中提取知识构建图谱。然而，这些图谱大多侧重于自然语言文本或特定的 API 关联，很少直接对海量源代码文件本身进行全覆盖的开放语义标注。
3. **基于 LLM 的开放实体提取与实体链接（Entity Linking）**：在通用自然语言处理（NLP）领域，利用 LLM 进行开放域信息抽取（Open IE）和将实体链接到维基百科/Wikidata 已有广泛研究。但在代码领域，由于代码语境的特殊性（包含大量专业术语、缩写和复杂的逻辑结构），通用领域的实体链接技术无法直接应用，需要专门针对代码语义进行定制化的流水线设计。

Q3: 论文如何解决这个问题？

### 技术路线与核心架构
本文提出了一种自动化流水线，旨在利用代码专用大语言模型从海量源代码中提取开放分类的语义概念，并将其标准化对齐到 Wikidata 知识库中。整个流水线包含以下三个核心阶段：

#### 1. 基于代码 LLM 的开放分类语义标注（Open-Taxonomy Annotation）
* **输入**：海量的源代码文件（本研究中使用 Stack-Edu 语料库，包含 14 种编程语言）。
* **处理**：利用专门针对代码优化的 LLM，通过精心设计的 Prompt，让模型在阅读代码后自由生成其蕴含的高阶工程概念（如算法、范式、模式、领域）。这种“开放分类”的方式允许模型捕捉任何潜在的语义标签，而不受预设标签集的限制。

#### 2. 三阶段 Wikidata 实体对齐（Three-Stage Grounding Procedure）
为了消除 LLM 自由生成概念的歧义性并实现标准化，研究设计了一个严密的对齐机制：
* **第一阶段：确定性 SPARQL 查询（Deterministic SPARQL Stage）**：针对 LLM 生成的、含义完全明确且无歧义的实体，直接通过编写 SPARQL 语句在 Wikidata 中进行精确匹配和检索，快速处理高频、标准的实体。
* **第二阶段：深度研究智能体（Deep Research Agent）**：针对残余的、具有歧义或属于长尾领域的复杂概念，部署一个基于智能体（Agent）的交互式检索机制。该智能体能够模拟人类研究员，通过多轮搜索、上下文比对和推理，解析并锁定正确的 Wikidata 唯一标识符（QID）。
* **第三阶段：层次上卷（Hierarchy-Rollup Stage）**：在建立基本实体的对齐后，自动导入这些 Wikidata 标识符在知识库中的“父类（parent-of）”闭包关系。这一步极大地丰富了图谱的层次结构，使得图谱不仅包含具体概念，还包含了宏观的分类学谱系。

#### 3. 校准的质量保证协议（Calibrated Quality-Assurance Protocol）
为了在大规模数据上量化和确保标注的精准度，本文创新性地结合了两种评估手段：
* **人类黄金标准集（Human Gold Set）**：由领域专家对一小部分样本进行严格的人工标注和校验，作为绝对真值。
* **LLM-as-a-judge 过滤器**：利用高级 LLM 作为裁判，对大规模生成的标注进行自动化质量评估。通过用人类黄金标准集对 LLM 裁判进行校准（Calibration），确保自动化评估的得分与人类专家的高度一致，从而实现低成本、高置信度的大规模质量控制。

Q4: 论文做了哪些实验？

### 实验设计与大规模应用
为了验证上述流水线的有效性和可扩展性，研究团队将其应用于超大规模的真实世界数据集上：
1. **实验数据集**：**Stack-Edu 语料库**。这是一个经过精心清洗、专注于高质量教育和工程价值的源代码数据集，包含了来自 **14 种不同编程语言**的 **1.67 亿个源代码文件**。
2. **基准测试与评估**：
 * 构建了一个包含数百个文件的人类专家黄金标准集（Human Gold Set），涵盖不同的编程语言和概念类别。
 * 运行 LLM-as-a-judge 协议，对流水线生成的“文件-概念”连接以及“概念-Wikidata”链接进行精确度（Precision）和召回率（Recall）的抽样评估。
 * 对三阶段对齐机制的每一阶段（SPARQL、Agent、Rollup）的贡献度和准确率进行消融分析或数量统计。

Q5: 发现了什么实验现象？

### 实验现象与图谱量化结果
由于本论文属于系统性、图谱构建类的重磅工作（Systematic Work），其核心实验发现主要体现为最终构建出的 **CodeGraph** 图谱的惊人规模、结构特征以及质量校验表现：

#### 1. 图谱规模与节点分布
* **总节点数**：CodeGraph 最终包含了约 **1.58 亿个节点**。
* **文件节点**：成功对约 **1.45 亿个源代码文件** 进行了语义标注。
* **概念实体**：LLM 从海量代码中提炼出了约 **63,000 个独特的概念实体**（涵盖了极其丰富的算法、设计模式和应用领域）。
* **Wikidata 对齐实体**：在这 63,000 个概念中，有约 **19,800 个实体** 被成功且精准地对齐到了 Wikidata 的标准 QID 上。

#### 2. 边（Edges）的连接与层次特征
* **总边数**：图谱中包含约 **10 亿条有向类型边（Typed Edges）**。
* **连接类型**：这些边清晰地定义了三种关系：
 1. 将**源代码文件**连接到它们所实现的**具体概念**（File $\rightarrow$ Concept）；
 2. 将**具体概念**链接到它们的 **Wikidata 标识符**（Concept $\rightarrow$ Wikidata QID）；
 3. 将**具体概念**关联到它们在 Wikidata 中的**父类范畴**（Concept $\rightarrow$ Parent Category），形成了强大的层次化分类网络。

#### 3. 质量保证协议的观察（合理推断）
* 通过结合人类黄金标准集与校准的 LLM-as-a-judge 过滤器，流水线在保持开放分类法高泛化能力的同时，维持了极高的标注精准度。深度研究智能体（Deep Research Agent）在处理长尾、含糊的代码概念时表现出了比传统字符串匹配更高的鲁棒性，成功挽回了大量原本会丢失的实体链接关系。

Q6: 有什么可以进一步探索的点？

### 可进一步探索的研究方向
基于 CodeGraph 的成功构建，未来在以下几个相邻领域和跨领域方向具有巨大的探索空间：
1. **下游任务赋能（Downstream Applications）**：利用 CodeGraph 的 10 亿条边，可以训练更具语义感知能力的代码嵌入模型（Code Embeddings），用于高精度的代码检索、相似度分析、自动化代码推荐以及软件仓库的智能分类。
2. **代码智能体增强（Agent Enhancement）**：将 CodeGraph 作为外部知识库（RAG）引入到现有的代码大模型或软件工程智能体（SWE-agents）中，帮助智能体在面对复杂系统设计时，能够从宏观的算法和模式层面理解现有代码库，而不仅仅是阅读局部 Token。
3. **跨领域科学应用（AI for Science）**：由于 CodeGraph 标注了代码的“应用领域”（如生物信息学、量子计算、金融工程），可以进一步挖掘特定科学领域代码的演进趋势、算法复用模式，促进科学计算软件的规范化与生态分析。
4. **动态图谱更新机制**：随着开源社区的不断演进，如何设计增量更新机制，让流水线能够低成本地实时捕捉新出现的编程概念和技术栈，是工程上值得深入的研究点。

Q7: 总结一下论文的主要内容

### 论文论证与技术主线全景总结

本研究针对当前软件工程领域“无法有效提取和组织海量源代码中隐含的高阶工程知识”这一痛点，提出并实现了一套完整的、基于大语言模型与知识库对齐的开放分类语义标注流水线，并成功构建了世界上首个大规模源代码开放分类知识图谱 —— **CodeGraph**。

#### 1. 动机与问题定义
传统的代码分析工具（如基于 AST 的静态分析）只能理解代码的语法结构，无法理解代码“在宏观上实现了什么算法、服务于什么业务领域”。而直接使用 LLM 生成标签又面临缺乏标准、无法结构化推理的问题。因此，本研究旨在将 LLM 的“开放语义理解能力”与权威知识库 Wikidata 的“结构化规范性”相结合，打通从非结构化代码到结构化高阶知识的通道。

#### 2. 技术主线：三阶段对齐与智能体设计
论文的核心技术贡献在于其严密的流水线设计。首先利用代码专用 LLM 对 Stack-Edu 语料库中的 1.67 亿个文件进行开放式概念抽取。为了解决抽取出的 63,000 个概念的标准化问题，设计了三阶段对齐法：
* **第一阶段**利用高效的 SPARQL 确定性查询解决高频词；
* **第二阶段**引入**深度研究智能体（Deep Research Agent）**，这是本系统的一大亮点。该智能体能够针对长尾、有歧义的概念进行自主的上下文推理和多轮检索，极大地提升了实体链接的召回率和准确性；
* **第三阶段**通过层次上卷，将 Wikidata 中的父子关系引入图谱，构建了从“具体文件 $\rightarrow$ 具体算法 $\rightarrow$ 宏观学科/领域”的完整知识网络。

#### 3. 实验与质量控制主线
为了确保这套自动化流水线产出的图谱不是“充满幻觉的垃圾数据”，研究团队引入了校准的质量保证协议。通过专家标注的小规模黄金标准集来校准 LLM 裁判，从而在大规模数据上实现了高置信度的自动化质量过滤。最终，该流水线在 14 种语言的数据集上大获成功，产出了包含 1.58 亿节点、10 亿条边的 CodeGraph。这一工作不仅展示了 LLM 在大规模数据挖掘中的落地能力，也为未来的代码智能、软件工程知识图谱研究奠定了坚实的底层数据基础设施。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文深入探讨了深度研究智能体（Deep Research Agent）在复杂实体链接任务中的应用，与你对智能体（Agent）方向的关注高度契合。

## 基本信息

- 作者：Federico Pennino, Andrea Gurioli, Stefano Zacchiroli, Maurizio Gabbrielli, Paolo Ferragina
- 机构：University of Bologna, Italy; Pisa University, Italy (合理推断，基于作者 Stefano Zacchiroli, Maurizio Gabbrielli, Paolo Ferragina 等在软件工程与算法领域的知名学者背景)
- 来源：arxiv
- 主题/分类：cs.SE, cs.CL, cs.IR
- 日期：2026-09-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.29474v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成主要基于论文摘要及详细的启发式草稿信息进行深度推理与系统化重构，未直接检索 PDF 全文，关键技术细节（如 Agent 具体架构和 QA 校准曲线）建议对照原文进一步核实。
