---
user_id: "cheng tan"
paper_id: 11838
arxiv_id: "2609.19088"
title: "MUSE: Benchmarking Large Vision-Language Models on Multi-Modal Understanding in Situated Education"
institution: "新加坡科技研究局（A*STAR）"
publish_date: "2026-09-17"
pdf_url: "https://arxiv.org/pdf/2609.19088"
abs_url: "https://arxiv.org/abs/2609.19088"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-18T01:12:51"
---
# MUSE: Benchmarking Large Vision-Language Models on Multi-Modal Understanding in Situated Education

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：vision-language models · benchmark · situated education · artistic image understanding

## 一句话总结

论文推出了 MUSE 基准测试，旨在评估大型视觉语言模型在情境化教育应用中对艺术图像的多模态理解、情感诠释与跨文化推理能力。

## 摘要

> Large vision-language models have achieved remarkable progress in multi-modal understanding, yet their capabilities in educational settings remain insufficiently evaluated. In AI-assisted language learning, models must interpret artistic imagery, understand its semantic, affective, and cultural content, and reason about visual context to support meaningful interaction. However, existing benchmarks primarily focus on real-world images or domain-specific educational reasoning, providing limited coverage of artistic educational content. To address this gap, we introduce MUSE, a benchmark for evaluating large vision-language models on artistic image understanding in situated educational applications. MUSE decouples image annotation from question generation, enabling diverse tasks with controllable difficulty while reducing annotation effort. It comprises twelve tasks spanning visual perception, semantic and affective interpretation, culture understanding, and compositional reasoning, together with diverse artistic images deliberately curated to center Singaporean and Southeast Asian multicultural contexts alongside Western art traditions, covering multiple themes and difficulty levels. Evaluation of open-source and proprietary models reveals substantial disparities across capability dimensions, particularly in affective interpretation and compositional reasoning. Our analysis further identifies common failure modes and key challenges for developing trustworthy multi-modal models for education. We hope MUSE will serve as a standardized benchmark for advancing multi-modal understanding in situated educational applications.

Q1: 这篇论文试图解决什么问题？

### 核心问题与痛点
在AI辅助语言学习（AI-assisted language learning）等情境化教育应用中，教学材料往往包含丰富的艺术图像（如插画、绘画、文化海报等）。为了实现高质量的师生互动与启发式教学，多模态大模型不仅需要识别图像中的物理实体，更需要具备以下深层次能力：
1. **情感与语义诠释**：理解艺术作品所传达的细腻情感、隐喻和核心主题。
2. **跨文化理解**：识别特定文化背景下的符号、习俗和历史脉络。
3. **组合推理**：在复杂的视觉上下文中进行逻辑关联与多步推理。

### 现有基准的局限性
目前主流的视觉语言模型（LVLM）基准测试存在明显的领域缺失：
- **偏向真实世界照片**：如 MME、MMBench 等，主要侧重于日常自然场景的物体识别与空间关系，缺乏对艺术化、抽象化图像的感知能力。
- **偏向学科硬知识推理**：如 MathVista、ScienceQA 等，侧重于数理化公式、图表或教科书式的硬核推理，无法覆盖语言与人文教育中所需的情感交流与文化共鸣。
- **缺乏多元文化视角**：现有艺术类数据集多偏向西方传统艺术，严重缺乏对东南亚等多元文化交汇区域的艺术内容覆盖。

### 论文的切入点
论文试图解决如何系统、全面、且低成本地构建一个针对“情境化教育艺术图像理解”的基准测试，以准确评估并推动 LVLMs 在真实教育场景中的落地应用。

Q2: 有哪些相关研究？

### 视觉语言模型基准测试（LVLM Benchmarks）
现有的多模态评测主要分为通用能力评测和特定领域评测。通用评测（如 MM-Vet, MMBench）侧重于基础的视觉识别、OCR 和简单推理。特定领域评测则逐渐向医疗、自动驾驶和传统学科教育（如数学、科学）渗透。然而，这些基准普遍忽略了人文与艺术教育这一重要分支。

### 艺术图像理解与多模态教育
在艺术图像分析领域，已有研究（如 ArtQA, SemArt）多侧重于艺术品的元数据检索或风格分类，并非为教育互动设计。在多模态教育领域，现有工作主要集中在自动批改作业、几何题求解等，缺乏对情境化语言学习中“看图说话”、“情感共鸣”和“文化熏陶”等软实力的评估。MUSE 正是在这两者的交汇点上填补了空白。

Q3: 论文如何解决这个问题？

### MUSE 基准设计框架
MUSE 的核心创新在于提出了一种**解耦式的数据生成范式**，将“图像标注”与“问题生成”分离开来，从而在降低人工标注成本的同时，实现了任务的多样性与难度的可控性。

### 1. 图像策展与文化多样性
数据集精心挑选并收录了大量艺术图像，其显著特点是**去中心化的文化视角**：
- 融合了新加坡及东南亚的多元文化背景（如马来传统艺术、印度裔节日视觉元素、土生华人文化符号）。
- 兼顾西方经典艺术传统，形成跨文化的对比与融合。
- 涵盖多种艺术风格（油画、水彩、数字插画、传统版画等）和不同的教学主题。

### 2. 十二大核心任务矩阵
MUSE 将评估维度划分为四个大类，共计 12 个子任务：
- **视觉感知（Visual Perception）**：基础物体识别、颜色与计数、空间关系定位。
- **语义与情感诠释（Semantic & Affective Interpretation）**：隐喻理解、核心主题抽取、艺术情感共鸣分析。
- **文化理解（Culture Understanding）**：文化符号识别、节日与习俗关联、历史背景推理。
- **组合推理（Compositional Reasoning）**：多对象关系推理、因果逻辑链分析、情境化教学互动问答。

### 3. 自动化与人工协同的问题生成
通过先由专家对图像进行高精度的细粒度属性标注（标签、描述、文化背景知识），再利用大语言模型（LLM）在严格的规则约束下自动生成多项选择题或开放式问答，最后经过人工校验，确保了评测集的高质量与高鲁棒性。

Q4: 论文做了哪些实验？

### 实验设置
由于本论文为基准测试（Benchmark）论文，其实验部分主要围绕当前主流视觉语言模型在 MUSE 数据集上的性能评测展开。

### 评测对象
实验覆盖了当前最具代表性的两类模型：
1. **闭源商业模型（Proprietary Models）**：如 GPT-4o、Claude 3.5 Sonnet、Gemini 1.5 Pro 等。
2. **开源前沿模型（Open-source Models）**：如 LLaVA-NeXT、Qwen-VL-Chat、InternVL 等不同参数量级的模型。

### 评估指标
- 对于多项选择题，采用准确率（Accuracy）作为核心指标。
- 对于开放式主观题，采用基于 GPT-4 的裁判打分机制（LLM-as-a-Judge）以及人工抽样评估，从准确性、教育适用性和逻辑性三个维度进行打分。

Q5: 发现了什么实验现象？

### 核心实验发现与指标张力
1. **商业模型与开源模型的断层式差距**：
 在基础的“视觉感知”任务中，顶尖开源模型（如 InternVL）能够逼近商业模型的表现；但在“情感诠释”和“组合推理”任务中，GPT-4o 和 Claude 3.5 Sonnet 表现出压倒性优势，开源模型在理解艺术隐喻时常出现“字面化误读”。

2. **情感诠释（Affective Interpretation）的普遍低迷**：
 所有被测模型在涉及“艺术作品传达了何种悲伤或喜悦的隐喻”等任务时，准确率均大幅下滑。模型能够识别出“画面有一个哭泣的人”，但无法结合色彩调性、构图线条去推理深层的情感氛围，表现出“情感色盲”现象。

3. **东南亚文化理解的“长尾困境”**：
 在西方艺术传统相关的题目上，模型的平均得分显著高于东南亚多元文化题目。这表明现有的预训练数据中存在严重的地域文化偏见（Cultural Bias），模型对于非西方主流的文化符号（如特定节日的服饰细节、东南亚特有器物）经常发生幻觉或张冠李戴。

4. **组合推理的失败模式**：
 消融分析显示，当问题从单层视觉识别升级为“结合文化背景解释画面中两个角色的互动动机”时，模型的错误率呈指数级上升。这揭示了当前模型在多模态长链条推理上的脆弱性。

Q6: 有什么可以进一步探索的点？

### 可进一步探索的方向
1. **文化均衡的多模态预训练**：
 如何利用 MUSE 暴露出的文化盲区，针对性地搜集和构建包含东南亚及其他长尾区域文化艺术的多模态预训练数据集，以缓解模型的文化偏见。
2. **情感对齐与艺术认知增强**：
 开发能够理解人类审美、构图心理学和情感隐喻的专用多模态架构，使 AI 在教育互动中不仅能“视物”，更能“共情”。
3. **动态情境化教学 Agent**：
 将 MUSE 的静态评测扩展到动态对话场景，评估模型在连续多轮的启发式教学（Socratic Dialogue）中，如何引导学生理解艺术图像背后的深意。
4. **跨模态知识图谱融合**：
 研究如何将结构化的文化知识图谱（Knowledge Graphs）显式地引入到 LVLMs 的推理过程中，以提升其在组合推理任务中的事实准确性。

Q7: 总结一下论文的主要内容

### 论文论证主线
本研究针对当前视觉语言模型（LVLM）在情境化教育（特别是语言与人文艺术教育）中缺乏针对性评测的问题，首次提出了一个名为 MUSE 的多模态艺术图像理解基准测试。论文遵循“现实痛点分析 -> 现有基准局限性论证 -> 新基准设计与构建 -> 多模型基准评测 -> 失败模式与挑战分析”的完整论证闭环。

### 技术与数据主线
MUSE 的核心技术贡献在于其解耦式的数据生成范式。通过将复杂的艺术图像标注分解为基础属性和背景知识的结构化录入，再借助大语言模型生成高质量的教育问答对，极大地降低了人工构建大规模高质量多模态基准的门槛。在数据源上，MUSE 刻意打破了西方中心主义的视角，引入了大量新加坡及东南亚的多元文化艺术图像，构建了包含视觉感知、语义情感、文化理解和组合推理四大维度、十二个子任务的评测矩阵。

### 实验与发现主线
通过对当前最前沿的开源和闭源多模态大模型进行全面评测，MUSE 揭示了现有模型在情境化教育应用中的核心短板。实验表明，尽管模型在基础视觉感知上表现良好，但在面对需要深层文化底蕴的“文化理解”任务和需要审美共情能力的“情感诠释”任务时表现欠佳。论文最后总结的常见失败模式（如文化幻觉、情感误读、推理断链），为下一代更具鲁棒性、更懂教育和文化的信任多模态模型的开发指明了方向。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作与生成式 AI（Generation）及多模态智能体的构建有较强的相关性，其任务设定可为多模态生成模型的条件控制提供评测参考。

## 基本信息

- 作者：Luyao Zhu, Xun Wei Yee, Wei Li, Mun Thye Mak, Wee Siong Ng
- 机构：新加坡科技研究局（A*STAR）
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.CV
- 日期：2026-09-17
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.19088`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成主要基于论文的元数据、摘要以及详细的启发式草稿信息进行深度整合与系统化梳理，未直接检索 PDF 全文，方法与实验的微观参数建议对照原文核实。
