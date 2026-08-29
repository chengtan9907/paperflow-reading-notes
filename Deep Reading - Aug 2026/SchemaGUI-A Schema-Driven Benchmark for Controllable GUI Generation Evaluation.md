---
user_id: "cheng tan"
paper_id: 9587
arxiv_id: "2608.22390v1"
title: "SchemaGUI: A Schema-Driven Benchmark for Controllable GUI Generation Evaluation"
publish_date: "2026-08-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.22390v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.22390v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:26:07"
---
# SchemaGUI: A Schema-Driven Benchmark for Controllable GUI Generation Evaluation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gui generation · benchmark · schema-driven · controllable evaluation

## 一句话总结

SchemaGUI 是一个基于接口 Schema 模板自动合成自然语言指令与确定性函数调用标注的可控 GUI 生成评测基准，通过六个双语场景和五个主流 LLM 的评测揭示出几何空间控制是当前模型的核心瓶颈，且思考模式会降低生成得分。

## 摘要

> Large language models (LLMs) have demonstrated strong potential in graphical user interface (GUI) generation, but reliable evaluation remains challenging due to uncontrolled data distributions, noisy annotations, and limited layout scenario coverage. To address this, we propose SchemaGUI, a template-based benchmark for controllable GUI generation evaluation. By synthesizing paired natural language instructions and deterministic function-call references from parameterized interface schemas, SchemaGUI can generate thousands of deterministically annotated tasks in seconds without human labeling. Based on 1,000 evaluated instances per scenario and language across six representative bilingual scenarios, we benchmark five mainstream models, including the Qwen3.5 family, Qwen3-Coder-30B, and DeepSeek-R1. Our extensive analysis reveals three key insights. First, precise geometric spatial control remains an important bottleneck; while scaling Qwen3.5 from 4B to 27B improves Schema Feasibility from 91.56% to 99.63%, the Geometry score improves more modestly (from 67.05% to 75.30%). Second, generation difficulty is highly sensitive to layout complexity, with current LLMs excelling at simple sequential arrangements but suffering severe coordinate drift in dense grids and multi-region compositions. Third, thinking mode increases token consumption while generally reducing GUI Score, particularly for smaller models. Our code is available at github.com/xdong2002/SchemaGUI.

Q1: 这篇论文试图解决什么问题？

该论文试图解决大型语言模型（LLM）在图形用户界面（GUI）生成任务中可靠评估方法缺失的问题。具体挑战包括：
1. 数据分布不可控：现有评测通常依赖真实网页或手工收集的界面数据，其布局类型、组件密度和风格分布不均匀，导致评测结果难以归因于模型能力。
2. 标注噪声大：人工标注的自然语言指令和对应代码往往存在歧义，且生成目标（如函数调用序列）缺少唯一确定性，影响自动评分的可靠性。
3. 布局场景覆盖有限：已有基准往往只覆盖少数简单布局，无法系统测试模型在复杂网格、多区域组合、稀疏/密集排列等条件下的表现。
4. 缺乏可控性：难以通过控制布局复杂度、语言类型、指令风格等因素来定位模型的具体能力短板。
为解决上述问题，SchemaGUI 提出一种基于参数化接口 Schema 的模板合成方法，自动生成带有确定性标注的评测数据，从而实现对 GUI 生成能力的大规模、低成本、可控且可重复的评估。

Q2: 有哪些相关研究？

相关研究可以分为几个方向：
1. 早期 GUI 生成框架：如 pix2code（Beltramelli, 2017）和 Sketch2Code（Robinson, 2019），它们展示了从截图或草图端到端生成界面代码的可行性，但这些方法依赖固定的 DSL 和有限的数据集，泛化能力受限于视觉特征提取。
2. 基于 LLM 的 GUI 生成：随着大模型的兴起，研究者开始利用 LLM 直接生成 HTML/CSS 或 UI 代码，或通过多模态交互生成界面。这类方法在布局理解和代码合成上取得进展，但缺乏标准化的评测协议。
3. GUI 评测基准：已有一些基准尝试评估 GUI 生成质量，如自动执行测试、截图对比、用户调研等，但它们要么依赖人工评估（成本高、可重复性差），要么基于规则验证（覆盖度有限）。SchemaGUI 与这些工作的区别在于：通过 Schema 模板合成，数据分布和标注均受控，且可扩展到任意规模。
4. 与程序合成和代码生成评测的关系：本方法中的函数调用序列可视为一种代码生成任务，因此与 HumanEval、MBPP 等基准在评估思想上类似，但聚焦于 GUI 场景中的布局约束和语义对齐。
需要说明的是，由于本次检索证据有限，以上部分内容（如其他基准）属于基于摘要背景的合理推断，具体相关工作的详细比较建议回到原文 References 部分核对。

Q3: 论文如何解决这个问题？

SchemaGUI 的核心思路是用模板合成替代人工采集，实现可控且自动化的评测数据生成。具体而言：
1. Schema 定义：首先定义参数化接口 Schema，它描述 GUI 中可能出现的组件（如按钮、输入框、列表）、属性（如尺寸、颜色、位置）、布局模式（如顺序排列、网格、多区域组合）以及交互语义（如点击触发函数）。每个 Schema 相当于一个可实例化的界面模板。
2. 自动合成：从 Schema 中随机采样参数（组件数量、类型、位置、文本等），生成具体的界面配置。同时，基于模板中的规则自动生成对应的自然语言指令（例如“在右上角放置一个蓝色按钮，点击后调用 submit()”）和确定性函数调用引用（即该界面的标准动作序列）。由于所有标注直接从模板派生，不依赖人工标注或模型输出，因此具有完全确定性。
3. 评测协议：在合成数据上定义两个维度：(a) Schema Feasibility，即模型生成的函数调用序列是否符合 schema 约束（如组件是否存在、类型是否正确）；(b) Geometry score，即生成结果中组件的位置、尺寸等几何属性与参考布局的匹配程度（如交并比、坐标距离）。评测时，将自然语言指令输入模型，要求其输出函数调用序列（可能还包含布局参数），再与参考标注对比。
4. 场景与多语言：设置六个代表性双语场景，覆盖常见 GUI 类型（如表单、仪表盘、设置页等），并使用英文和中文两种指令语言，从而评估模型的跨语言理解和生成能力。
5. 可扩展性：整个流程可在数秒内生成超过 10,000 个样本（合理推断，基于摘要“generate more than 10,000 samples in seconds”），且样本规模可任意调整，便于在不同复杂度下进行细粒度评估。

Q4: 论文做了哪些实验？

论文进行了系统性的模型评估，实验设置如下：
1. 评测模型：共五个主流模型，包括 Qwen3.5 系列（至少包含 4B 和 27B 两个尺寸，以观察规模效应）、Qwen3-Coder-30B 以及 DeepSeek-R1。其中 Qwen3-Coder 系列侧重代码能力，DeepSeek-R1 可能具备更强的推理或思考模式。
2. 数据规模：基于六个代表性双语场景（具体场景未在摘要中列出，合理推断为登录页、表单、仪表盘、列表页、设置面板等常用界面），每个场景和语言（EN/ZH）各 1,000 个评测实例，因此每种模型共评估 12,000 个双语实例（6 场景 × 2 语言 × 1,000）。
3. 评估指标：使用 Schema Feasibility 和 Geometry score 两个自动指标，分别衡量生成结果是否符合 schema 约束以及几何布局的准确性。可能还有其他辅助指标（如 token 消耗），但摘要重点报告了这两个。
4. 控制变量：通过比较不同模型尺寸（4B vs 27B）来研究扩展规律；通过设计不同布局复杂度（简单顺序、密集网格、多区域组合）来测试布局敏感性；通过开关“思考模式”来检测推理开销与生成质量的权衡。
5. 实验流程：将自然语言指令输入模型，得到函数调用（可能包含参数），然后与确定性标注进行自动比对，计算各指标的平均分。

Q5: 发现了什么实验现象？

论文的丰富分析揭示了以下几个关键实验现象：
1. 几何空间控制是核心瓶颈：当模型规模从 Qwen3.5-4B 扩展到 27B 时，Schema Feasibility 由 91.56% 大幅提升至 99.63%（提升约 8 个百分点），但 Geometry score 仅从 67.05% 提升到 75.30%（提升约 8 个百分点，但绝对得分依然明显偏低）。这说明即便模型能正确生成符合 schema 的函数调用，其空间坐标预测能力仍然不足，精确的几何控制并未随模型规模同步提升。
2. 布局复杂度强烈影响生成难度：当前 LLM 在简单顺序排列的布局中表现出色，但在密集网格和多区域组合布局中会出现严重的坐标漂移。这一现象暗示模型对局部空间关系的建模能力有限，遇到元素密集或区域划分时容易产生位置偏移和重叠错误。
3. 思考模式产生负面影响：启用思考模式（如推理链或详细推理）会增加 token 消耗，同时普遍降低 GUI 得分，尤其对较小模型影响更大。这可能是因为思考模式引导模型过度解释指令，反而干扰了直接生成函数调用的确定性输出；也可能是 GUI 任务本身不需要深度推理，且长输出增加了出错概率。
4. 参数缩放收益不均衡：虽然模型规模提升显著改善了 Schema 可行性，但几何能力的提升幅度相对有限，提示当前模型的扩展曲线在逻辑生成和空间感知上存在“剪刀差”，未来可能需要更细粒度的位置编码或外部工具辅助。
这些发现为 GUI 生成领域提供了实证基础，但也需注意实验仅基于单一模板合成数据，真实世界界面可能带来额外分布偏移（推测）。

Q6: 有什么可以进一步探索的点？

从论文的局限性和现有发现，未来研究可在多个方向深入：
1. 交互式与动态 GUI 生成：当前基准聚焦静态结构合成，未来应扩展至动态用户交互、状态流转、运行时事件处理（如点击后的行为）以及对话式界面，建立涵盖时序一致性的评测协议。
2. 更广泛的模型家族与规模：论文仅评测了 Qwen 和 DeepSeek 系列，未来可引入更多闭源和开源模型（如 GPT、Claude、Gemini、Llama），并在更大样本规模下进行统计检验，以便刻画不同模型族的 GUI 能力差异。
3. 几何控制的改进方法：既然几何得分显著低于 Schema 可行性，可以探索坐标回归专用模块、使用网格位置嵌入、或结合外部布局引擎（如约束求解器）进行后处理，并评估是否可以弥补 LLM 的先天不足。
4. 复杂布局的系统化建模：针对密集网格和多区域组合，设计更细粒度的难度梯度（如元素密度、区域数量、对齐约束），用于训练数据增强或课程学习，同时也可以作为压力测试集。
5. 思维模式与生成效率的权衡：思考模式增加了 token 消耗却降低得分，未来可以研究何种指令或提示策略能避免过度推理，或设计自适应推理深度控制，兼顾质量与成本。
6. 多模态 GUI 生成：将屏幕截图作为额外输入，评测多模态 LLM 的图像理解与布局推理能力，进一步贴近实际应用场景。
7. 基准本身的扩展：将 Schema 模板抽象为领域无关的通用描述语言，应用于其他可控生成任务（如表单、数据可视化、模型配置界面），从而提升该范式在更广泛智能体任务中的作用。

Q7: 总结一下论文的主要内容

论文提出 SchemaGUI，一种面向可控 GUI 生成评估的模板化基准，旨在解决现有评测中数据分布不可控、标注噪声和场景覆盖不足的问题。核心方法是利用参数化接口 Schema 合成配对数据：从 Schema 模板中随机采样生成界面配置，同时自动生成对应自然语言指令和确定性函数调用引用，整个过程无需人工标注，可在数秒内生成上万样本。所有标注由模板直接派生，因此具有完全确定性，这保证了评测的可重复性。
基于六个代表性双语场景（中/英），每个场景和语言各 1000 个实例，论文系统评估了五个主流模型（Qwen3.5 系列、Qwen3-Coder-30B 和 DeepSeek-R1），使用 Schema Feasibility 和 Geometry score 作为主要指标。实验结果显示三个主要发现：第一，模型规模扩展（4B 到 27B）显著提升了 Schema Feasibility（从 91.56% 到 99.63%），但 Geometry score 提升十分有限（67.05% 到 75.30%），说明精确几何空间控制仍是瓶颈；第二，布局复杂度严重影响生成难度，模型在简单顺序排列上表现良好，但在密集网格和多区域组合中频繁出现坐标漂移；第三，思考模式总体上降低 GUI 得分并增加 token 消耗，尤其对较小模型不利。这些发现揭示了当前 LLM 在 GUI 生成中逻辑能力与空间感知能力的不均衡发展，也为未来模型优化指明了方向。
论文的贡献包括：提出一种可扩展的模板化评测基准，无需人工标注即可实现大规模确定性评测；揭示了跨模型尺寸和布局复杂度的一致趋势；开源了代码（github.com/xdong2002/SchemaGUI）。局限性方面，基准目前仅覆盖静态结构，未涉及动态交互，且模型覆盖范围有限。总体而言，SchemaGUI 为 GUI 生成评估提供了一种有效的基础设施，其发现对提升 LLM 的空间控制能力和思考模式的合理使用具有指导意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作属于“生成”方向，提出了一种可控生成评测框架，可为你当前生成相关的研究提供基准设计参考。

## 基本信息

- 作者：Jiarui Dong, Yin Cai, Zhouhong Gu, Chenmou Wu, Ci Tao, Yiran Chen, Jialing Li, Xiaoran Shi, Juntao Zhang, Zhijun Fang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-23
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.22390v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（主要来自 Abstract、Introduction 和 Limitations 片段），并结合摘要和启发式草稿进行了扩展；部分细节（如具体场景名称、其他相关工作的比较）为基于上下文的合理推断，需回到原文验证。
