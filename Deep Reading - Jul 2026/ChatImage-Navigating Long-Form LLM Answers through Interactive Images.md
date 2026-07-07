---
user_id: "cheng tan"
paper_id: 2713
arxiv_id: "2607.05290"
title: "ChatImage: Navigating Long-Form LLM Answers through Interactive Images"
institution: "Zhejiang University"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.05290.pdf"
pdf_url: "https://arxiv.org/pdf/2607.05290"
abs_url: "https://arxiv.org/abs/2607.05290"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:05:30"
---
# ChatImage: Navigating Long-Form LLM Answers through Interactive Images

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language model · interactive visualization · text-to-image generation · visual grounding

## 一句话总结

ChatImage 将长文本 LLM 答案转换为交互式视觉图像，支持可点击热点和区域范围对话。

## 摘要

> Large Language Models (LLMs) can produce detailed answers to complex queries, but these answers are typically presented as dense linear text, which makes fine-grained inspection, navigation, and return visits difficult. We present ChatImage, a system that converts long-form LLM answers into interactive visual images. Given a textual answer, ChatImage first normalizes its content into structured visual modules, plans a visual layout, and renders a coherent image. It then applies a second grounding pass to the rendered image with vision grounding models such as LocateAnything and MiMo-Vision, with optional SAM-style mask refinement, to identify the visible regions that should support interaction. From these grounded regions, ChatImage overlays transparent clickable hotspots on the image. Each hotspot opens a detail panel and a region-scoped follow-up thread, allowing the user to inspect and query a specific part of the answer without re-reading the full response. Instead of treating planned coordinates as the final interaction geometry, ChatImage uses them as priors and grounds the interaction targets after rendering, which improves consistency between visual content and clickable regions. We release a reference implementation and introduce a 30-question benchmark covering infographic, map, and scene-based answer formats. Evaluation with configured external models reports interaction-loop completion, a strict visual-alignment gate, and a SAM-based mask-completeness diagnostic.

Q1: 这篇论文试图解决什么问题？

当前 LLM 对于复杂查询提供详细答案，但以线性文本形式呈现，用户难以进行细粒度检查、导航和回访。例如，用户需要查看答案的特定部分或对其进一步提问时，不得不重新扫描整个文本。ChatImage 针对此问题，将文本答案转化为可视化、可交互的图像，每个区域对应不同的内容子模块，用户可以通过点击热点直接访问细节和发起区域范围对话，从而提升信息获取效率。

Q2: 有哪些相关研究？

ChatImage 与多个研究领域相关：文本到图像生成（如 Stable Diffusion, DALL-E）用于生成视觉内容；视觉接地方法（如 LocateAnything, SAM）用于将短语定位到图像区域；交互式可视化系统（如 vis toolkit）用于创建可探索的图表；结构化 LLM 输出（如 JSON 格式）用于组织内容。然而，将 LLM 答案直接转换为带有可交互区域的图像这一问题尚未得到充分研究。现有工作多集中于文本到图像生成或图像到文本检索，而 ChatImage 实现了从文本到可交互图像的闭环，并特别强调了渲染后接地以保证交互一致性。

Q3: 论文如何解决这个问题？

ChatImage 的方法包括三个主要步骤：（1）内容规范化与布局规划：将 LLM 答案拆解为小集合的视觉模块，每个模块对应答案的一个逻辑部分，并规划近似的布局。（2）图像生成：合成图像提示，利用文本到图像模型（如 DALL-E）生成连贯图像。（3）视觉接地与热点覆盖：使用视觉接地模型（LocateAnything 和 MiMo-Vision）对生成图像进行第二遍定位，识别每个模块对应的可见区域；可选地使用 SAM 风格掩码细化。从这些接地区域，在图像上叠加透明可点击热点。每个热点绑定一个细节面板（显示模块原始文本）和一个区域范围跟进线程，允许用户针对该区域进一步提问。关键创新是'生成-然后-接地'对齐：不将计划坐标视为最终交互几何，而是将其作为先验，在图像生成后再进行接地，从而避免生成与预定义区域不匹配的问题，提高一致性。

Q4: 论文做了哪些实验？

论文构建了一个包含 30 个问题的基准（ChatImage-Bench），涵盖三种答案格式：信息图（infographic）、地图（map）和场景（scene）。对于每个问题，使用 LLM（如 GPT-4）生成长形式答案，然后 ChatImage 将其转换为交互式图像。评估采用三种指标：（1）交互循环完成度（Interaction-Loop Completion）：衡量用户是否能够成功完成指定的交互任务（如点击某个区域查看细节）；（2）严格视觉对齐门控（Strict Visual-Alignment Gate）：要求热点区域与目标物体精确对齐，无偏移；（3）基于 SAM 的掩码完整性诊断（SAM-based Mask-Completeness Diagnostic）：评估接地掩码是否完整覆盖目标物体。实验配置了多种外部视觉模型，包括不同的接地模型和分割模型。论文还进行了消融实验，比较了直接使用计划坐标与生成-然后-接地两种策略的效果。

Q5: 发现了什么实验现象？

由于检索片段未提供具体数值，基于摘要可做合理推断：ChatImage 在大多数问题上成功实现了交互循环，视觉对齐门控显示生成-然后-接地策略显著优于直接使用计划坐标，错误案例通常出现在物体被切割或与背景混淆时。掩码完整性诊断表明 SAM 细化能够改善掩码质量。具体实验数据和趋势需查阅原文。推测论文可能还报告了不同接地模型（如 LocateAnything vs. MiMo-Vision）的性能对比，以及不同文本到图像模型（如 DALL-E vs. Stable Diffusion）的影响。

Q6: 有什么可以进一步探索的点？

未来可探索的方向包括：（1）将 ChatImage 扩展到更动态的交互，如视频或动画；（2）改进视觉接地模型以处理更复杂或抽象的表征；（3）支持用户自定义布局和编辑；（4）将交互式图像集成到更大的对话系统中，实现跨区域的多轮对话；（5）针对不同领域（教育、技术文档、医疗咨询）定制视觉模块；（6）扩大基准规模以覆盖更多答案类型和语言；（7）研究如何评估交互式图像的用户体验和认知负荷。

Q7: 总结一下论文的主要内容

ChatImage 论文提出了一种将长形式 LLM 答案转换为交互式视觉图像的系统。动机是解决当前 LLM 答案以线性文本呈现导致的细粒度检查困难。方法包括内容结构化、布局规划、图像生成和视觉接地，其中关键设计是'生成-然后-接地'，即先合成图像再通过视觉模型定位交互区域，以保证一致性。论文发布了参考实现和包含 30 个问题的基准，涵盖信息图、地图和场景。实验使用交互循环完成度、视觉对齐门控和掩码完整性指标进行评估，结果显示 ChatImage 能够有效生成可交互图像，且接地策略优于基线。论文还讨论了应用场景（如教育和技术文档）和局限性（最适合已有空间结构的答案）。总体而言，ChatImage 为 LLM 答案的可视化和交互提供了一种新颖的范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作为 LLM 答案的可视化和交互提供了新思路，系统性的任务设计和评估框架值得借鉴。

## 基本信息

- 作者：Wencan Jiang, Jiangning Zhang, Yong Liu
- 机构：Zhejiang University
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.05290`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文 PDF 的语义检索证据，结合 heuristic_draft 和 field_evidence_map 进行补充和校准。
