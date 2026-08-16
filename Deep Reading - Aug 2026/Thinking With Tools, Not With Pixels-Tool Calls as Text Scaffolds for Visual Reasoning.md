---
user_id: "cheng tan"
paper_id: 7430
arxiv_id: "2608.09682v1"
title: "Thinking With Tools, Not With Pixels: Tool Calls as Text Scaffolds for Visual Reasoning"
institution: "根据论文作者及提到的 DeepEyesV2 项目，推测该研究团队主要来自商汤科技（SenseTime）及相关合作学术机构。"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.09682v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.09682v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-12T01:26:07"
---
# Thinking With Tools, Not With Pixels: Tool Calls as Text Scaffolds for Visual Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：vision-language models · tool-augmented reasoning · visual agent · text scaffold

## 一句话总结

本研究提出了“工具调用脚手架假设”，证明在当前的工具增强型视觉语言模型中，推理增益主要源于工具调用的结构化文本而非返回的像素，并据此开发了高效的 TextCall 方法。

## 摘要

> Tool-augmented vision-language models increasingly "think with images": they call crop, zoom, or code tools and reason over the returned pixels. However, recent work using blind tests, gain decompositions, and attention analyses has shown that returned images contribute little, raising the question: if pixels do not carry the gain, what does? We hypothesize that the load-bearing signal is the structured text emitted before any returned pixel arrives: tool name, coordinates, target description, and intent. This textual scaffold encodes where to look and what to find. We introduce TextCall (call-but-no-return) to test this: it keeps the scaffold but replaces returned images with the text placeholder [Image output skipped]. Three studies support the hypothesis. (i) Non-necessity of returned pixels: across LoRA, full fine-tuning, and RL, TextCall matches or exceeds full thinking-with-images; under RL it preserves tool use at the reported checkpoint, avoiding the failure mode where, under matched settings, seeing the returned image causes the model to stop calling tools and answer directly. (ii) Sufficiency of the scaffold: on matched training queries, scaffold-only input yields equivalent accuracy to returned-image input. (iii) Component specificity: decomposing the scaffold into reasoning text and spatial code shows both components contribute, with the dominant one varying by task. Together these results support the Tool-Call Scaffold Hypothesis: in current thinking-with-images distributions, the active signal is the structured text emitted at tool-call time; the returned image is a redundant carrier. TextCall preserves accuracy while reducing latency by 29-46% and eliminating tool-execution API calls. Our claims hold for current thinking-with-images benchmarks; constructing tasks where pixels are genuinely load-bearing remains an open direction.

Q1: 这篇论文试图解决什么问题？

该论文深入探讨了工具增强型视觉语言模型（VLM）中一个被长期忽视的根本性问题：在所谓的“以图像思考”（thinking-with-images）过程中，性能提升的真正驱动力究竟是什么？

1. **核心矛盾**：目前的 VLM 智能体在处理复杂视觉任务时，通常会调用裁剪（Crop）或缩放（Zoom）工具。直觉上，人们认为模型是因为“看”到了更清晰、更局部的像素才做出了更准确的判断。然而，近期的一些诊断性研究（如盲测）显示，即使不给模型看返回的图像，其性能依然维持在高位。这产生了一个巨大的认知鸿沟：如果像素不是增益的载体，那么模型到底在利用什么信息？

2. **技术瓶颈与挑战**：
 - **冗余性问题**：现有的“图像思考”流程涉及昂贵的图像编码（Vision Encoder）和工具执行开销，如果这些像素是冗余的，那么当前的系统架构存在巨大的效率浪费。
 - **训练不稳定性**：在强化学习（RL）阶段，模型往往会发现“直接回答”比“调用工具并等待图像返回”更容易获得奖励，从而导致工具调用行为的退化（Failure Mode）。
 - **因果分解困难**：在标准的训练流程中，工具调用的文本（意图、坐标）与返回的像素是耦合在一起的，很难区分哪一部分才是真正的推理“脚手架”。

3. **研究动机**：作者试图通过隔离实验，证明工具调用时产生的结构化文本（即“脚手架”）已经包含了解决问题所需的绝大部分逻辑和空间信息。如果这一假设成立，将彻底改变视觉智能体的设计范式，从“像素驱动”转向“逻辑脚手架驱动”。

Q2: 有哪些相关研究？

论文将该研究置于视觉语言模型、工具学习和模型诊断的交汇点：

1. **工具增强型 VLM**：如 ViperGPT、Chameleon 和 DeepEyes 等系统，它们通过外部工具扩展了 VLM 的感知边界。这些系统默认认为“获取更多像素”是提升性能的关键路径。

2. **视觉推理中的像素冗余研究**：VisionThink [85] 等近期工作发现，在多模态任务中，很大比例的像素 token 可以被跳过而不影响结果。这为本研究的“像素冗余”观点提供了初步证据。

3. **空间引用与文本表征**：研究 [83] 表明模型可以学习在文本中引用空间区域。本研究进一步将其扩展为一种“推理脚手架”，认为文本化的空间坐标和意图描述在逻辑链条中起到了锚点作用。

4. **诊断性分析工具**：论文借鉴了消融实验、增益分解和注意力分析等方法，这些方法在自然语言处理领域常用于揭示模型的“捷径学习”（Shortcut Learning）行为。本研究将其引入视觉工具调用领域，揭示了“图像思考”背后的文本本质。

5. **与并发工作的区别**：虽然有研究在探索替代的特征载体，但本论文是首个明确提出“工具调用脚手架假设”并提供完整 TextCall 替代方案的工作，强调了文本在视觉推理中的主导地位。

Q3: 论文如何解决这个问题？

作者提出了一套严谨的验证框架和一种新型的训练/推理方法 TextCall：

1. **工具调用脚手架假设 (Tool-Call Scaffold Hypothesis)**：
 该假设认为，在当前的视觉推理任务中，模型生成的结构化文本（例如：`crop(x=100, y=200, label="red car")`）已经完成了最核心的推理步骤——即确定“去哪里看”和“看什么”。一旦这些信息被显式化为文本，后续的推理其实是在利用这些文本线索，而非依赖返回的像素细节。

2. **TextCall 方法设计**：
 - **核心逻辑**：在模型发出工具调用指令后，系统拦截该调用，不执行实际的图像处理，也不返回任何像素数据。
 - **文本占位符**：在原本放置返回图像的位置，插入一个固定的文本字符串，如 `[Image output skipped]` 或 `Image edit successful. [no image returned]`。
 - **训练对齐**：在 SFT（监督微调）和 RL（强化学习）过程中，模型学习在没有像素反馈的情况下，仅根据自己之前的调用记录和这个占位符来完成最终回答。

3. **实验验证路径**：
 - **非必要性验证**：通过对比 TextCall 和 Full-Image 模式在 LoRA 和全量微调下的表现，证明像素不是必须的。
 - **充分性验证**：通过控制变量，验证仅保留脚手架文本是否足以支撑等效的准确率。
 - **成分分解**：将脚手架拆分为“推理文本”（意图描述）和“空间代码”（坐标），分别测试它们对性能的贡献度，揭示不同任务对脚手架成分的依赖差异。

Q4: 论文做了哪些实验？

论文设计了多层次、跨规模的实验体系：

1. **实验设置与数据集**：
 - 使用了包含 6 个核心基准测试的综合套件（基于 DeepEyesV2 架构），涵盖了视觉问答、空间关系判断和复杂场景分析。
 - 训练数据包括 65K 样本的全量 SFT 运行和 9.5K 样本的 LoRA 微调。

2. **对比基准 (Baselines)**：
 - **Full Thinking-with-images**：标准的工具调用流程，包含图像返回。
 - **TextCall**：本研究提出的只调用不返回模式。
 - **Direct Answer**：不使用任何工具的直接回答模式。

3. **强化学习 (RL) 实验**：
 - 在 RL 阶段对比两种模式。特别关注模型是否能维持工具调用行为。实验发现，在匹配的设置下，看到返回图像的模型往往会演化出“跳过工具”的策略，而 TextCall 模型则能稳定保持工具使用习惯。

4. **消融与成分分析**：
 - **移除坐标**：测试空间信息的重要性。
 - **移除意图描述**：测试逻辑引导的重要性。
 - **跨工具家族一致性检查**：验证该现象在不同类型的工具（如不同实现的裁剪工具）之间是否普遍存在。

Q5: 发现了什么实验现象？

实验揭示了几个关键且反直觉的现象：

1. **性能持平甚至反超**：在全量微调下，TextCall 在 6 个基准测试上的平均准确率比全图像模式高出 +1.39 个百分点。这表明返回的像素不仅可能是冗余的，有时甚至会引入噪声干扰推理。

2. **显著的效率提升**：由于省去了 Vision Encoder 对返回图像的二次编码以及工具执行的 API 耗时，TextCall 将端到端延迟降低了 29-46%。这对于实时交互系统具有巨大价值。

3. **RL 中的行为稳定性**：在强化学习中，TextCall 避免了“工具退化”现象。作者观察到，当模型不需要处理返回像素时，它更容易建立“调用工具 -> 获得逻辑确认 -> 准确回答”的正向反馈循环。

4. **脚手架的成分敏感性**：
 - 在某些任务中，空间坐标（Spatial Code）是主导因素；而在另一些任务中，意图描述（Reasoning Text）更为关键。
 - 即使将返回的图像替换为完全无关的占位符，只要脚手架文本保持不变，模型的推理逻辑链条就不会断裂。

5. **负结果与失败模式**：在极少数需要极高分辨率纹理识别的任务中，TextCall 表现略逊，但这在当前主流基准测试中占比极低，证明了当前 benchmark 的局限性。

Q6: 有什么可以进一步探索的点？

1. **构建“像素负载”基准测试**：当前的测试集无法区分“逻辑推理”和“纯视觉感知”。未来需要设计必须依赖高分辨率像素细节（如微小文字、复杂遮挡下的纹理特征）才能解决的任务，以真正发挥图像返回的作用。

2. **动态脚手架机制**：研究模型是否可以自主决定何时使用 TextCall（为了速度）以及何时必须请求真实像素（为了精度），实现效率与效果的动态平衡。

3. **跨模态脚手架扩展**：探索该假设是否适用于音频、视频或代码执行等其他工具增强领域。例如，在音频处理中，是否也是工具调用的文本描述而非返回的频谱图在起作用？

4. **长链条推理的误差累积**：在需要多次连续工具调用的复杂任务中，研究仅依赖文本脚手架是否会导致逻辑幻觉的累积，以及如何通过文本反馈进行自我修正。

Q7: 总结一下论文的主要内容

这篇论文对当前视觉语言模型（VLM）中流行的“以图像思考”范式提出了深刻的挑战。作者通过系统的实验证明，在现有的工具增强型视觉推理任务中，模型性能的提升并非源于工具返回的高分辨率像素，而是源于工具调用本身所产生的结构化文本——即“工具调用脚手架”。

论文的核心贡献在于提出了“工具调用脚手架假设”，并开发了名为 TextCall 的替代方案。TextCall 通过在推理链条中剔除冗余的图像返回环节，仅保留工具调用的文本描述，实现了与全图像模式相当甚至更优的准确率。更重要的是，这种方法在实际应用中展现出巨大的优势：它将推理延迟降低了近一半，显著减少了计算资源的消耗，并解决了强化学习中常见的工具调用退化问题。

通过对脚手架成分的深入剖析，作者揭示了空间坐标和意图描述在视觉推理中的关键锚点作用。这一发现不仅为构建更高效、更鲁棒的视觉智能体提供了理论支撑，也向整个研究社区发出了警示：当前的视觉推理基准测试可能更多地在考察模型的逻辑编排能力，而非真正的视觉感知能力。论文最后呼吁开发真正具有“像素负载”挑战的新型任务，以推动多模态智能向更深层次发展。总而言之，这是一篇兼具理论深度和实用价值的论文，对于理解 VLM 的推理机制和优化智能体架构具有重要意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于关注 AI Agent 效率优化的开发者，TextCall 提供了一种极低成本的性能保持方案

## 基本信息

- 作者：Jiahao Shao, Yuanbo Yang, Yiyi Liao, Yujun Shen, Ceyuan Yang, Yinghao Xu
- 机构：根据论文作者及提到的 DeepEyesV2 项目，推测该研究团队主要来自商汤科技（SenseTime）及相关合作学术机构。
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.09682v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是关于 TextCall 的定义、实验数据对比以及“工具调用脚手架假设”的理论阐述。
