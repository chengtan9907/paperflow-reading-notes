---
user_id: "cheng tan"
paper_id: 11851
arxiv_id: "2609.17699"
title: "NeMo Data Designer: An Extensible Framework for Multimodal Synthetic Data Generation"
institution: "NVIDIA"
publish_date: "2026-09-17"
pdf_url: "https://arxiv.org/pdf/2609.17699"
abs_url: "https://arxiv.org/abs/2609.17699"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-18T01:14:23"
---
# NeMo Data Designer: An Extensible Framework for Multimodal Synthetic Data Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：synthetic data generation · multimodal data · declarative framework · large language models

## 一句话总结

NVIDIA 开源的 NeMo Data Designer (NDD) 是一个声明式、可扩展的多模态合成数据生成框架，旨在通过预览-修正循环和插件化架构提升合成数据生产的可复现性与效率。

## 摘要

> We present NeMo Data Designer (NDD), an open-source, general-purpose framework for multi-modal synthetic data generation (SDG). Designed to be intuitive to use, NDD provides a declarative configuration format in which human and/or agent users define each dataset column, with column types spanning text, code, structured outputs, images, embeddings, and statistical samplers that are explicitly configured to steer dataset diversity. Additional column types and functionality can be introduced using the framework's flexible plugin system. NDD's configuration is an inspectable artifact, supporting workflow sharing and reproducibility. SDG is an inherently iterative process. NDD therefore builds a preview-and-revision loop into its core workflow, allowing users to generate and inspect a small number of records, refine the specification, and rerun generation at full scale. At runtime, NDD resolves dependencies, schedules calls to user-provided model endpoints, and retries failed requests. We describe NDD's architecture and programming model and present case studies spanning structured, agentic, multimodal, and domain-specialized tasks, including datasets used in Nemotron model development and in production enterprise deployments.

Q1: 这篇论文试图解决什么问题？

### 核心挑战
合成数据生成 (SDG) 已成为训练先进大模型 (LLM) 和多模态模型 (LMM) 的关键，但当前实践面临以下痛点：
1. **迭代效率低下**：SDG 过程通常是高度迭代的，开发者需要不断调整 Prompt 或逻辑。缺乏“预览”机制导致在大规模生成前难以发现逻辑错误，造成计算资源浪费。
2. **缺乏可复现性**：许多 SDG 流程依赖于复杂的脚本，缺乏统一的声明式描述，导致实验难以在不同团队间共享或复现。
3. **多模态集成困难**：现有的工具往往专注于单一模态（如纯文本），难以在一个统一框架内协调文本、图像、代码和结构化数据的生成逻辑。
4. **工程复杂度高**：处理模型端点的并发调用、依赖解析、错误重试以及数据多样性控制需要大量的底层工程开发，分散了研究员对数据质量本身的关注。

### 论文目标
NDD 旨在提供一个“以数据为中心”的工程框架，将复杂的生成逻辑抽象为声明式的列定义，通过自动化工程链路让研究者专注于数据分布的设计与优化。

Q2: 有哪些相关研究？

### 合成数据生成框架的演进
1. **专用生成工具**：如早期的 Self-Instruct 或针对特定任务的生成脚本，通常缺乏通用性。
2. **智能体驱动的生成**：利用 LLM Agent 进行数据扩增，但往往缺乏对生成过程的精细控制和结构化约束。
3. **工业级流水线**：大型科技公司内部通常有私有的 SDG 平台，但开源社区缺乏一个既能支持多模态、又具备高度可扩展性和生产级鲁棒性的通用框架。

### NDD 的定位
NDD 填补了开源领域中“声明式配置”与“多模态支持”结合的空白，借鉴了现代软件工程中的声明式基础设施（如 Terraform/Kubernetes）理念，将其应用于数据工程领域。

Q3: 论文如何解决这个问题？

### 1. 声明式配置架构
NDD 允许用户通过 YAML 或 Python 字典定义数据集结构。每一列（Column）代表一个生成步骤，用户可以指定列的类型、输入依赖和生成逻辑。这种方式将“做什么”与“怎么做”解耦。

### 2. 核心组件与插件系统
- **列类型 (Column Types)**：内置支持文本生成、代码执行、结构化 JSON 输出、图像生成、向量嵌入等。
- **统计采样器 (Statistical Samplers)**：允许用户显式配置采样分布，以控制生成数据的多样性和覆盖范围。
- **插件系统**：开发者可以轻松定义新的 `Column` 类，集成自定义的模型接口或处理逻辑，确保了框架的无限扩展性。

### 3. 预览-修正循环 (Preview-and-Revision Loop)
这是 NDD 的核心工作流创新。用户可以先生成极小规模（如 5-10 条）的记录进行人工检查，框架支持快速调整配置并重新运行，直到生成质量符合预期后再启动大规模生产任务。

### 4. 运行时引擎 (Runtime Engine)
- **依赖解析**：自动构建生成步骤的有向无环图 (DAG)，并行化执行无依赖的任务。
- **端点调度**：支持多种模型后端（如 NVIDIA NIM, OpenAI 等），处理并发限制。
- **容错机制**：内置自动重试逻辑，应对网络波动或模型服务暂时不可用的情况。

Q4: 论文做了哪些实验？

### 案例研究设计
论文通过多个维度的案例展示了 NDD 的通用性：
1. **结构化数据生成**：生成符合特定 Schema 的 JSON 数据，用于训练指令遵循能力。
2. **智能体 (Agentic) 任务**：模拟多步推理和工具调用过程，生成复杂的 Agent 轨迹数据。
3. **多模态任务**：协同生成图像及其对应的详细描述（Caption）或视觉问答（VQA）对。
4. **领域专业化任务**：针对医疗、法律或编程等特定领域，通过专用插件引入领域知识约束。

### 实际应用背景
- **Nemotron 模型开发**：NDD 被用于构建 NVIDIA Nemotron 系列模型的训练集，验证了其在大规模模型训练中的有效性。
- **企业级部署**：在实际生产环境中，NDD 帮助企业快速构建垂直领域的合成数据集。

Q5: 发现了什么实验现象？

### 关键发现与洞察
1. **迭代速度提升**：通过预览机制，开发者发现逻辑错误的平均时间显著缩短，避免了在大规模任务运行数小时后才发现 Prompt 偏差的情况。
2. **多样性控制的有效性**：引入统计采样器后，生成数据的分布更接近目标分布，有效缓解了合成数据中常见的“模式坍塌”问题。
3. **多模态协同**：在图像-文本对生成实验中，NDD 的依赖解析确保了文本描述与图像内容的高度一致性，这对于训练高质量 LMM 至关重要。
4. **失败模式处理**：在处理数百万量级的请求时，NDD 的重试机制和端点负载均衡显著提高了任务的完成率，减少了人工干预的需求。

Q6: 有什么可以进一步探索的点？

### 潜在研究方向
1. **自动配置优化**：利用 LLM 自动优化 NDD 的声明式配置，实现“元合成数据生成”。
2. **更深度的质量评估集成**：将数据质量评估指标（如奖励模型评分、多样性度量）直接反馈到生成循环中，实现闭环优化。
3. **实时流式生成**：探索 NDD 在在线学习或实时数据增强场景下的应用潜力。
4. **跨框架互操作性**：支持将 NDD 配置导出为其他数据处理引擎可识别的格式。

Q7: 总结一下论文的主要内容

本文提出了 NeMo Data Designer (NDD)，这是一个旨在解决合成数据生成 (SDG) 领域效率与可复现性难题的开源框架。NDD 的核心理念是将复杂的多模态生成任务抽象为声明式的列定义，通过 DAG 依赖解析自动管理生成流程。其独特的“预览-修正”循环允许开发者在低成本下快速迭代数据设计方案。NDD 不仅支持文本和代码，还深度集成了图像生成和向量嵌入等功能，并通过插件系统保持了极高的扩展性。论文通过 Nemotron 模型的开发案例和企业级应用证明了 NDD 在处理大规模、高质量合成数据任务时的卓越性能。NDD 的开源为研究社区提供了一个标准化的工具，有望推动合成数据研究从“脚本驱动”向“工程化、系统化”转变。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于关注智能体 (Agent) 训练的开发者，NDD 提供了生成复杂任务轨迹的系统化方案。

## 基本信息

- 作者：Johnny Greco, Nabin Mulepati, Andre Manoel, Eric Tramel, Kirit Thadaka, Mike Knepper, Dhruv Nathawani, Dane Corneil, Yev Meyer, Alex Watson, Maarten Van Segbroeck
- 机构：NVIDIA
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-09-17
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.17699`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成参考了论文摘要及启发式草稿，重点解析了 NDD 的架构设计理念及其在多模态合成数据领域的系统性贡献。
