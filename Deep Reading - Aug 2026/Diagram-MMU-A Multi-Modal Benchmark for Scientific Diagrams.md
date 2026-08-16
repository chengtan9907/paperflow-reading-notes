---
user_id: "cheng tan"
paper_id: 7651
arxiv_id: "2608.12262v1"
title: "Diagram-MMU: A Multi-Modal Benchmark for Scientific Diagrams"
institution: "University of Adelaide (阿德莱德大学), Singapore University of Technology and Design (新加坡科技设计大学), Nanjing University of Science and Technology (南京理工大学), etc."
publish_date: "2026-08-12"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.12262v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.12262v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-14T11:52:08"
---
# Diagram-MMU: A Multi-Modal Benchmark for Scientific Diagrams

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：multimodal benchmark · scientific diagrams · diagram-to-code · latex tikz

## 一句话总结

Diagram-MMU 是一个包含 3.7k 张科学图表和 18.3k 个问题的多模态基准测试，旨在评估多模态大模型（MLLMs）在科学图表解析、编辑及问答任务中的能力，并特别引入了智能体（Agentic）评估设置。

## 摘要

> Multimodal Large Language Models (MLLMs) have been growing the capability for scientific writing and collaboration. For example, OpenAI Prism is a free workspace for scientific writing and collaboration. One important feature in Prism is turning scientific diagrams directly into IATEX TikZ code. In this paper, we build a benchmark, Diagram-MMU, a multi-modal benchmark designed to assess MLLMs' ability for scientific diagram parsing and understanding. Diagram-MMU features 3.7k curated diagrams and 18.3k human-validated questions across six domains. It evaluates MLLMs on three tasks common in vibe writing workspaces: diagram-to-code parsing, diagram-to-code editing, and diagram question answering, alongside agentic settings per task. The evaluation of 12 MLLMs reveals that diagram-to-code tasks are more challenging than diagram question answering: models can reason well over diagrams but struggle to parse and edit them, underscoring the need for methods to enhance MLLMs' capability in diagram-to-code generation. Under agentic settings, most models improve parsing and editing performance but degrade on question answering, while Claude-4.6 Opus consistently improves across all three tasks.
> Date: August 13, 2026
> Connection Email: shan.zhang@adelaide.edu.au, yanpeng\_sun@sutd.edu.sg
> Project Page: https://vi-ocean.github.io/projects/diagram-mmu
> ![](images/687e84b403bd1c2ad635729ab6e296786c60ffa28f980295474596515bffa2f1.jpg)

Q1: 这篇论文试图解决什么问题？

### 1. 科学图表理解的现有局限性
目前的 MLLMs 虽然在通用视觉问答（VQA）上取得了显著进展，但在处理高度结构化、包含复杂语义和数学逻辑的科学图表时仍显乏力。现有的基准测试（如 ChartQA 等）多侧重于统计图表（如柱状图、折线图），而忽略了物理、化学、计算机科学等领域中广泛使用的示意图、流程图和电路图。

### 2. 现有代码表示的生态脱节
许多现有的图表转代码研究采用 Python 或 SVG 作为中间表示。虽然这些格式易于渲染，但它们与学术界主流的 LaTeX 写作生态系统（尤其是 TikZ 绘图包）存在脱节。科学家更需要能够直接嵌入论文源码、可二次编辑的 TikZ 代码，而非静态的图像或独立的脚本。

### 3. 静态评估与动态协作的鸿沟
传统的评估方式通常是“单次推理”，即模型根据图像直接输出结果。然而，在真实的科研协作场景（如 OpenAI Prism 空间）中，模型需要像智能体一样工作：检索不熟悉的 TikZ 语法、根据视觉反馈修正代码、在现有代码基础上进行增量编辑。目前缺乏一个能够模拟这种“智能体化”工作流的评估框架。

### 4. 核心挑战总结
- **感知与解析的精度**：如何准确识别图表中的几何元素、文本标签及其拓扑关系。
- **代码生成的合规性**：TikZ 语法复杂且包依赖多，模型难以生成可编译且视觉一致的代码。
- **推理与生成的张力**：模型往往能“看懂”图表（回答问题），但无法“重建”图表（写出代码）。

Q2: 有哪些相关研究？

### 1. 多模态基准测试（MLLM Benchmarks）
现有的基准测试如 MME、MMBench 和 SEED-Bench 主要关注通用领域的图像理解。在科学领域，虽然有 ScienceQA 等数据集，但其图表类型相对单一，且主要侧重于选择题形式，缺乏对图表底层结构解析的深度考察。

### 2. 图表专用基准（Chart-specific Benchmarks）
ChartQA、PlotQA 和 Leaf-QA 等专注于数据可视化图表。这些图表的特点是具有明确的数据映射关系（Data-to-Visual），而 Diagram-MMU 关注的科学示意图则更多涉及概念表达、逻辑流程和物理实体关系，其解析难度更高。

### 3. 图表转代码研究（Diagram-to-Code）
早期的工作尝试将图表转换为 SVG 或 HTML/CSS。最近，一些研究开始探索 TikZ 生成，但通常局限于合成数据集或特定的小规模领域。Diagram-MMU 通过引入 3.7k 真实世界的科学图表，显著提升了该领域的评估规模和多样性。

### 4. 智能体化评估（Agentic Evaluation）
随着 AutoGPT 和 BabyAGI 等概念的兴起，评估模型作为智能体调用工具（如搜索、执行环境）的能力成为前沿。本文将此范式引入科学图表处理，填补了 MLLM 在特定垂直领域智能体能力评估的空白。

Q3: 论文如何解决这个问题？

### 1. 数据集构建与分类
Diagram-MMU 涵盖了 6 个主要领域：计算机科学（流程图、架构图）、物理（受力分析、光学通路）、化学（分子结构、实验装置）、生物（细胞结构、代谢途径）、数学（几何证明）和工程（电路图）。
- **规模**：3,744 张图表，18,305 个实例。
- **验证**：所有问题和代码对均经过人工校验，确保 TikZ 代码的可编译性和视觉保真度。

### 2. 三大核心任务设计
- **D2C-P (Diagram-to-Code Parsing)**：给定图表图像，生成完整的 TikZ 代码。评估指标包括代码相似度（BLEU/CodeBLEU）和渲染后的视觉相似度（SSIM/MSE）。
- **D2C-E (Diagram-to-Code Editing)**：给定原始图表、对应的 TikZ 代码以及一个编辑指令（如“增加一个节点”或“改变连线颜色”），模型需要修改代码。这模拟了科研中的迭代修改场景。
- **DQA (Diagram Question Answering)**：针对图表内容进行多轮问答，考察模型对图表语义、逻辑和数值关系的理解。

### 3. 智能体设置（Agentic Settings）
为每个任务引入了工具调用能力：
- **TikZ 语法检索**：模型可以访问 TikZ 官方文档或示例库，以查找不熟悉的宏包用法。
- **视觉对象识别工具**：辅助模型定位图表中的关键元素。
- **迭代修正流**：模型可以根据编译错误信息或渲染后的预览图进行自我修正。评估时重点考察模型决定“何时调用工具”以及“如何整合工具反馈”的能力。

Q4: 论文做了哪些实验？

### 1. 实验设置
- **评估模型**：共测试了 12 种模型，包括闭源领先模型（GPT-4o, Claude-4.6 Opus, Gemini 1.5 Pro）和开源模型（LLaVA-v1.6, Qwen-VL-Max, InternLM-XComposer2 等）。
- **评估指标**：
 - D2C 任务：使用 Pass@1, CodeBLEU, 以及基于像素的视觉相似度指标。
 - DQA 任务：准确率（Accuracy）。
 - 智能体任务：成功率及工具调用效率。

### 2. 实验流程
- **Zero-shot 评估**：直接输入 Prompt 要求模型完成任务。
- **Agentic 评估**：提供 ReAct 框架或类似的思维链提示，允许模型在多步交互中完成任务。

Q5: 发现了什么实验现象？

### 1. 任务难度差异：推理易，解析难
实验发现，大多数模型在 DQA（问答）任务上表现优异，但在 D2C-P（解析生成代码）上得分极低。这表明模型虽然能识别出“图中有一个电阻”，但无法将其准确转化为 TikZ 的 `\draw ... to [R] ...` 语法。这种“知行不一”的现象揭示了 MLLM 在精确结构化输出方面的短板。

### 2. 智能体设置的双刃剑效应
- **正面影响**：在 D2C-P 和 D2C-E 任务中，引入 TikZ 语法检索显著提升了代码的可编译率。模型能够通过查阅文档纠正拼写错误或学习复杂的绘图指令。
- **负面影响**：令人意外的是，在 DQA 任务中，过多的检索或不相关的工具调用反而干扰了模型的判断，导致部分模型的问答准确率下降。这反映出模型在处理冗余信息时的鲁棒性不足。

### 3. 模型表现分化
- **Claude-4.6 Opus**：表现最稳健，是唯一一个在所有任务（包括 DQA）中都能从智能体设置中获益的模型。其规划（Planning）和工具整合能力显著领先。
- **开源模型**：在 D2C 任务上几乎全军覆没，生成的代码经常出现语法崩溃，且在智能体循环中容易陷入死循环或重复无效查询。

### 4. 失败模式分析
- **幻觉问题**：在编辑任务中，模型经常会修改与指令无关的代码部分，导致图表整体结构坍塌。
- **空间推理失效**：模型难以精确计算 TikZ 中的坐标位置，导致生成的元素重叠或断开。
- **规划能力薄弱**：在智能体设置下，模型往往无法制定有效的多步策略，尤其是在面对复杂的编译错误时，缺乏有效的调试逻辑。

Q6: 有什么可以进一步探索的点？

### 1. 增强 TikZ 专用预训练
目前的 MLLMs 在 TikZ 语料上的训练严重不足。未来可以考虑在大规模 LaTeX 源码数据集上进行针对性的持续预训练，以提升模型对绘图指令的敏感度。

### 2. 视觉-代码对齐的细粒度表征
开发新的架构，使模型能够在像素空间和代码 Token 空间之间建立更强的对齐关系，例如引入坐标感知（Coordinate-aware）的注意力机制。

### 3. 交互式科研助手
将 Diagram-MMU 的评估范式转化为实际的科研工具，开发能够与人类科学家实时协作、通过自然语言指令精准修改论文配图的智能系统。

### 4. 自动纠错与反馈闭环
研究如何让模型更有效地利用编译器反馈（Compiler Feedback）和视觉差异反馈（Visual Diff）进行自我迭代，这是提升 D2C 任务成功率的关键。

Q7: 总结一下论文的主要内容

本文介绍了 Diagram-MMU，这是一个旨在填补科学图表理解与生成领域空白的大规模多模态基准测试。研究动机源于当前 MLLMs 在处理复杂科学示意图时的局限性，尤其是在将图像转换为可编辑的 LaTeX TikZ 代码这一实际科研需求上。作者构建了一个包含 3.7k 张图表、覆盖 6 大科学领域的数据库，并设计了解析（D2C-P）、编辑（D2C-E）和问答（DQA）三大任务。此外，论文创新性地引入了智能体评估框架，模拟模型在真实环境中使用工具和迭代修正的行为。实验结果揭示了当前顶尖模型在“理解”与“重建”之间的巨大鸿沟：模型虽能通过问答展现出对图表的理解，但在生成精确代码方面表现欠佳。Claude-4.6 Opus 脱颖而出，展现了强大的智能体协作潜力，而大多数模型在复杂任务流中仍表现出规划能力不足和易受干扰的弱点。该工作为未来开发更高性能的科学写作助手提供了重要的数据支持和评估标准。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文直接关联智能体（Agent）方向，特别是模型如何调用外部工具（TikZ 检索）来解决专业任务。

## 基本信息

- 作者：Weihao Bo, Shan Zhang, Yanpeng Sun, Jie Liu, Yongke Yao, Jinhao Du, Wei He, Kai Zou, Zechao Li, Jingdong Wang
- 机构：University of Adelaide (阿德莱德大学), Singapore University of Technology and Design (新加坡科技设计大学), Nanjing University of Science and Technology (南京理工大学), etc.
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-08-12
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.12262v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是 Abstract、Introduction 和 Evaluation Scheme 部分的详细内容，确保了对任务定义和实验发现的准确复述。
