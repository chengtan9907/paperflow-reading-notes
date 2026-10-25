---
user_id: "cheng tan"
paper_id: 2946
arxiv_id: "2607.06306"
title: "UI2App: Benchmarking Visual Interaction Inference in Executable Web Application Generation"
publish_date: "2026-07-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Jul 2026/2607.06306.pdf"
pdf_url: "https://arxiv.org/pdf/2607.06306v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-10-12T13:30:08"
---
# UI2App: Benchmarking Visual Interaction Inference in Executable Web Application Generation

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：benchmark · web application generation · interaction inference · visual fidelity

## 一句话总结

UI2App 提出了一个从静态多页面截图推断交互行为并生成可执行 Web 应用的基准测试，发现前沿视觉语言模型在视觉保真度与交互实现之间存在显著脱节。

## 摘要

> Large language models (LLMs) have demonstrated growing competence in generating web pages from UI screenshots, which convey both visual structure and cues to application behavior. Yet most screenshot-to-code benchmarks emphasize visual fidelity, while interactive generation benchmarks often supply behavioral specifications or demonstrated transitions. Whether models can infer and realize interactions from static screenshots alone remains insufficiently evaluated. We introduce UI2App to evaluate interaction inference: inferring and realizing application behavior from static visual cues without added behavioral guidance. UI2App comprises 600 screenshots organized into 95 state-coherent sets for runnable multi-route web applications. Our end-to-end pipeline evaluates each artifact along three dimensions: executability, visual fidelity, and interaction inference. The interaction metric (IIS) assesses functional correctness and state-management complexity, crediting valid implementations rather than requiring a match to a single reference. Experiments on six frontier vision-language models reveal a marked mismatch between visual fidelity and interaction realization: the visual-fidelity leader scores only 8.1 on IIS, ranking fourth, while the IIS leader achieves 4.5 times that score. High-complexity interactions such as cross-route state persistence remain a major bottleneck, with five of the six models scoring at most 3.5 on this dimension. Overall, these results highlight interaction inference as a key challenge in generating functional web applications from static screenshots.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：现有 screenshot-to-code 基准大多只衡量视觉保真度，即生成结果与参考图像的相似程度，而不关心生成物是否真正作为可运行的应用程序工作；另一方面，交互式生成基准通常会提供行为规格或演示转换，使模型能够从额外指导中获益。因此，模型能否仅从静态截图中推断出应用的行为逻辑（如点击跳转、状态更新、跨路由数据持久化）并正确实现，尚未得到充分评估。

具体而言，论文指出现有文本驱动的软件开发范式依赖详细提示，难以精确描述视觉布局和多页视觉一致性；而图像到网页生成范式更贴近设计师工作流，但现有基准的评估维度过于单一。UI2App 旨在填补这一空白：它要求模型仅凭一组静态截图（每个截图对应一个路由或视觉状态）推断出完整的应用行为，并在可执行代码中实现这些行为。

该问题的重要性在于：如果模型只能复现外观而不能实现交互，那么生成的产物只是“图片”，不是“应用”。对于非技术用户而言，他们希望从截图直接获得可用的功能应用，而不仅仅是视觉上相似的页面。因此，交互推断能力是衡量视觉语言模型实际应用价值的关键维度。

Q2: 有哪些相关研究？

相关研究可分为几个方向：

1. **文本驱动的代码生成与软件开发**：如 Roziere et al. (2023)、Guo et al. (2024, 2025) 展示了 LLM 在代码生成、任务规划与推理上的进展；Hong et al. (2024)、Qian et al. (2024) 探索了从自然语言指令进行端到端软件开发。但这些方法依赖详细文本提示，难以精确表达视觉需求。

2. **图像到网页生成**：Beltramelli (2018) 的 pix2code 是早期工作，Si et al. (2025)、Laurençon et al. (2024)、Li et al. (2025) 等提出了多个基准，但这些基准主要关注视觉重建，即输出与参考图像的相似度，不评估应用行为。

3. **多页面网站生成**：MRWeb (Wan et al., 2024) 将生成扩展到多页面网站，但输入包含结构化导航链接和资源列表，本质上仍将 UI 生成视为静态渲染问题，不涉及运行时交互。

4. **交互式生成基准**：一些工作提供行为规格或演示转换，例如通过用户操作轨迹或状态转换来指导生成，但这类设置给了模型额外的行为线索，无法评估从静态截图独立推断交互的能力。

5. **视觉语言模型评估**：本文评估的六个模型（Claude Sonnet 4.6、Kimi K2.5、GPT-5.4、Gemini 3.1 Pro、Qwen3.5-397B-A17B、GLM-4.6V）代表了当前前沿水平，但现有基准很少将视觉保真度与交互实现分开评测。

UI2App 的独特之处在于：任务输入仅包含截图，没有任何行为说明；评估指标不仅检查可执行性和视觉相似度，还通过 IIS 量化交互的功能正确性和状态管理复杂度，从而直接衡量“交互推断”能力。

Q3: 论文如何解决这个问题？

UI2App 的解决方案包括三个主要部分：任务定义、数据集构建和端到端评估流水线。

**任务定义**：每个任务只提供一组截图（M=3–14，平均 6.3），每张截图对应目标应用的一个不同路由或视觉状态。模型需要根据这些静态视觉线索推断出应用的行为逻辑，并生成可运行的 Web 应用（框架为 React/Vite，从证据片段可见）。任务不提供任何行为规范、点击轨迹或状态转换说明。

**数据集构建**：包含 600 张截图，组织为 95 个状态连贯的集合，每个集合对应一个可运行的多路由 Web 应用。这些应用覆盖多种交互类型，包括跨路由状态持久化（S3 范围）等高复杂度场景。数据集设计强调状态连贯性，即截图之间隐含的状态关系是推断交互的关键线索。

**评估流水线**：端到端流水线包含六个步骤（从证据片段推断）：(1) 生成代码；(2) 构建与运行检测（EXEC@1、EXEC@3）；(3) 路由匹配，将每个输入截图映射到生成应用的路由；(4) 页面配对，生成 VFS 所需的页面对；(5) 视觉保真度计算，通过 DOM-to-DOM 相似度定义 VFS；(6) 自动交互评估，使用 GPT-6 Luna 作为评判者。

**交互推断指标（IIS）**：IIS 评估功能正确性和状态管理复杂度，采用交互分类法捕捉状态逻辑复杂度。与要求匹配单一参考的指标不同，IIS 认可任何有效的实现，只要功能正确且状态管理合理。这允许模型以不同方式实现相同行为，更贴近真实开发场景。

**实验协议**：为了公平比较，所有模型通过各自官方 API 在统一生成协议下查询。主实验每个应用-模型对生成一次，IIS 由 GPT-6 Luna 评估。为验证自动评分的可靠性，在 45 个应用的子集上进行了人类标注和三次独立生成，并报告评估者一致性和重复生成结果。此外，还评估了四个 Qwen2.5-VL 检查点以研究模型家族内的缩放趋势。

Q4: 论文做了哪些实验？

论文进行了以下实验：

1. **主实验**：在全部 95 个应用上评估六个前沿视觉语言模型：Claude Sonnet 4.6、Kimi K2.5（启用思考模式）、GPT-5.4、Gemini 3.1 Pro Preview、Qwen3.5-397B-A17B 和 GLM-4.6V。每个模型通过官方 API 在统一协议下生成，每个应用-模型对生成一次。评估指标包括 EXEC@1、EXEC@3、VFS（及其子组件：尺寸、文本、位置、颜色）和 IIS。

2. **验证实验**：在 45 个应用的子集上，使用人类标注和三次独立生成来验证自动评分（GPT-6 Luna）的可靠性，并评估生成变异性。评估者一致性和重复生成结果详见附录 I 和 J。

3. **缩放研究**：在同样的 45 应用子集上评估四个 Qwen2.5-VL 检查点，以研究模型家族内的性能缩放趋势（Section 4.5）。

4. **失败分析**：对 EXEC@3 的构建/运行时失败进行分类，识别出 101 个失败案例，其中 38.6% 违反了四个明确的计划提示约束之一。同时报告了指令遵从率（从 100.0% 到 71.6%）。

5. **交互推断深度分析**：通过细粒度的交互分类法，分析模型在不同复杂度交互（特别是跨路由状态持久化 S3 范围）上的表现，揭示瓶颈所在。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：

1. **视觉保真度与交互实现严重脱节**：视觉保真度领先者（Gemini，VFS 73.2）在 IIS 上仅得 8.1 分，排名第四；而 IIS 领先者（Claude，36.8）得分是前者的 4.5 倍。这表明高视觉相似度并不保证功能正确性，两个维度需要独立评估。

2. **模型排序在不同指标上不一致**：Claude 在 EXEC@1（95.8%）和 EXEC@3（100%）上领先，Gemini 在 VFS 上领先，Claude 在 IIS 上领先，而 GLM-4.6V 在所有指标上均垫底。视觉质量排序不能推广到交互质量。

3. **跨路由状态持久化是共同瓶颈**：在 S3 范围的交互（涉及跨路由状态保持）上，六个模型中有五个得分不超过 3.5，最强的模型也仅达到 15.8。这说明模型难以从静态截图中推断出需要跨页面维护状态的应用逻辑。

4. **开源模型在交互推断上可超越闭源模型**：Kimi（19.7）和 Qwen3.5（15.3）在 IIS 上超过了 Gemini（8.1）和 GPT-5.4（6.9），尽管它们在 VFS 上落后。这表明交互推断能力可能与视觉重建能力并非正相关，且开源模型在此维度具有竞争力。

5. **失败模式集中在计划约束违反**：在 101 个 EXEC@3 构建/运行时失败中，38.6% 违反了四个明确的计划提示约束之一，说明模型在遵循生成协议方面存在不足。指令遵从率从 100.0% 到 71.6% 不等，差异显著。

6. **重复生成稳定性**：在 45 应用子集上，Claude 在三次生成中始终领先 IIS（36.86 ± 0.39），平均超过 Gemini 27.91 分（95% 分层自助置信区间 [20.95, 35.06]），表明结果具有统计显著性。

Q6: 有什么可以进一步探索的点？

基于论文的发现，可以进一步探索的方向包括：

1. **改进跨路由状态持久化**：这是当前所有模型的主要瓶颈。可以研究如何从截图中更有效地推断状态管理逻辑，例如通过显式建模页面间的数据流，或利用多模态预训练任务增强状态跟踪能力。

2. **扩展交互类型和复杂度**：当前基准已包含 S3 范围的高复杂度交互，但可以进一步引入更丰富的交互模式，如条件逻辑、异步操作、用户输入验证、权限控制等，以更全面地评估交互推断能力。

3. **改进自动评估方法**：IIS 依赖 GPT-6 Luna 作为评判者，虽然经过人类标注验证，但仍可能存在偏差。未来可以探索更可解释、更细粒度的自动评估协议，或开发基于执行轨迹的验证方法。

4. **模型训练与微调**：UI2App 可作为训练信号，引导模型在生成代码时更注重交互正确性。例如，使用该基准构建偏好数据集，通过强化学习或指令微调提升模型的交互推断能力。

5. **跨领域迁移**：将交互推断能力从 Web 应用迁移到移动应用、桌面软件或 GUI 自动化 agent 场景，研究视觉推断与动作规划的结合。

6. **缩放规律研究**：论文初步探索了 Qwen2.5-VL 家族内的缩放趋势，未来可以系统研究模型规模、数据规模与交互推断性能之间的关系，为资源分配提供指导。

7. **多模态输入融合**：当前任务仅使用截图，未来可考虑结合部分文本提示或用户意图，研究在更实际场景下交互推断的边界。

8. **失败驱动的迭代生成**：利用 EXEC 失败信息（如构建错误）进行自我修正，探索模型能否从错误中学习并改进交互实现。

Q7: 总结一下论文的主要内容

UI2App 论文针对现有 screenshot-to-code 基准只关注视觉保真度、而交互生成基准又提供额外行为指导的问题，提出了一个专门评估“交互推断”能力的基准。核心思想是：仅给模型一组静态截图（每个截图对应应用的一个路由或视觉状态），要求模型推断出应用的行为逻辑并生成可运行的 Web 应用，不提供任何行为规范或演示。

论文首先回顾了相关研究，指出文本驱动开发范式依赖详细提示，难以精确表达视觉需求；图像到网页生成范式虽更贴近设计师工作流，但现有基准只衡量视觉相似度，不评估应用是否真正可用。MRWeb 等扩展到了多页面，但仍将 UI 生成视为静态渲染问题。因此，从静态截图推断交互行为的能力尚未被系统评估。

UI2App 的任务定义非常简洁：输入仅为一组截图（M=3–14，平均 6.3），输出为可运行的 Web 应用。数据集包含 600 张截图，组织为 95 个状态连贯的集合，每个集合对应一个多路由应用。这些应用覆盖了从简单页面跳转到复杂跨路由状态持久化的交互类型。

评估流水线包括三个维度：可执行性（EXEC@1、EXEC@3）、视觉保真度（VFS，通过 DOM-to-DOM 相似度计算）和交互推断（IIS）。IIS 是论文的核心贡献之一，它通过交互分类法捕捉状态逻辑复杂度，评估功能正确性和状态管理，并且不要求匹配单一参考实现，而是认可任何有效实现。这种设计更贴近真实开发场景，也避免了过度约束模型。

实验部分，论文评估了六个前沿视觉语言模型：Claude Sonnet 4.6、Kimi K2.5、GPT-5.4、Gemini 3.1 Pro Preview、Qwen3.5-397B-A17B 和 GLM-4.6V。所有模型通过官方 API 在统一协议下生成，主实验每个应用-模型对生成一次。为了验证自动评分（GPT-6 Luna）的可靠性，在 45 个应用子集上进行了人类标注和三次独立生成，并报告了评估者一致性和重复生成结果。此外，还评估了四个 Qwen2.5-VL 检查点以研究缩放趋势。

主要结果令人惊讶：视觉保真度与交互实现之间存在显著脱节。Gemini 在 VFS 上领先（73.2），但在 IIS 上仅得 8.1 分，排名第四；Claude 在 IIS 上以 36.8 分领先，是 Gemini 的 4.5 倍。GLM-4.6V 在所有指标上垫底。跨路由状态持久化（S3 范围）是共同瓶颈，五个模型得分不超过 3.5，最强也只有 15.8。此外，开源模型 Kimi 和 Qwen3.5 在 IIS 上超过了 Gemini 和 GPT-5.4，尽管它们在视觉保真度上落后，说明交互推断能力与视觉重建能力并非正相关。失败分析显示，101 个 EXEC@3 失败中 38.6% 违反了计划提示约束，指令遵从率从 100% 到 71.6% 不等。

论文的结论是，UI2App 是第一个专门针对交互推断的基准，揭示了视觉保真度不能作为交互实现质量的代理指标。跨路由状态持久化是当前模型的主要短板，为未来研究提供了明确的方向。论文希望 UI2App 能推动视觉语言模型在从静态截图生成功能应用时，同时提升推断和实现能力。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该工作属于生成方向，与用户画像中的“generation”直接相关，且是系统性基准工作，符合用户对系统性工作的偏好。

## 基本信息

- 作者：Grace Man Chen, Litao Guo, Yifan Wu, Yiyu Chen, Yenchi Tseng, Sicheng Liu, Yuyu Luo, Ying-Cong Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-07-08
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.06306`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据片段（Introduction、Results、Conclusion 等），并结合摘要与启发式草稿进行补全；部分细节（如流水线步骤、失败分析）基于证据片段合理推断。
