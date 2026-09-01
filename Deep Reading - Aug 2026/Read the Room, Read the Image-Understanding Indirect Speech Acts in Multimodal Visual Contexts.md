---
user_id: "cheng tan"
paper_id: 10017
arxiv_id: "2608.30270v1"
title: "Read the Room, Read the Image: Understanding Indirect Speech Acts in Multimodal Visual Contexts"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30270v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30270v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:46:17"
---
# Read the Room, Read the Image: Understanding Indirect Speech Acts in Multimodal Visual Contexts

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：indirect speech acts · pragmatic reasoning · multimodal benchmark · visual question answering

## 一句话总结

本文提出 READI 基准，通过视觉上下文与对话的整合推理来评估多模态模型对间接言语行为的理解能力。

## 摘要

> Indirect speech acts (ISAs) require pragmatic reasoning over context, since directive intent cannot be inferred from surface form alone. Prior text-based studies and multimodal benchmarks largely overlook this, focusing on explicitly encoded context or perceptual recognition particularly in high-context languages such as Korean. We introduce READI, a multimodal benchmark that evaluates ISA understanding through integrated reasoning over visual context and dialogue. Grounded in pragmatic theory, READI models graded indirectness and formulates the task as visual pragmatic question answering (V-PQA), enabling cross-lingual evaluation in English and Korean. Experiments show that even state-of-the-art multimodal models struggle with visually grounded ISAs with performance declining as indirectness increases, revealing fundamental limitations in interpreting indirectness. By proposing an evaluation paradigm for systematically assessing pragmatic understanding in multimodal settings, this study provides directions for improving the pragmatic reasoning abilities of language models.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在视觉上下文中，多模态模型对间接言语行为（ISA）的理解能力严重不足，而现有的文本研究和多模态基准在很大程度上忽略了这一点。具体而言，现有工作往往将上下文限定为显式编码的信息，或者仅关注视觉内容的感知识别，而没有将视觉场景作为社会互动和社会语用上下文的资源来解释。对于非惯例化的间接言语行为，上下文不足可能导致人类沟通者也产生歧义，而 LLM 在缺乏上下文时尤其容易出错。该论文旨在弥合这一空白，通过引入一个专门设计的基准，系统地评估和推动模型在视觉与对话交互情境下的语用推理能力。

Q2: 有哪些相关研究？

相关工作涉及间接言语行为的文本研究与多模态基准。先前的文本研究聚焦于短语级或句子级的间接性，但常局限于惯例化形式，没有系统纳入语言多样性和丰富的视觉上下文。多模态研究证明了图像-语言推理的可行性，但基准任务多强调感知匹配，而非社会语用层面的推断。高语境语言（如韩语）的研究尤为欠缺，因为这类语言更依赖语境来传达间接意图。此外，已有证据表明，对于非惯例化的间接言语行为，上下文缺失会增加意图解读的歧义，且 LLM 在间接性理解上表现不佳，尤其是在需要环境线索时。因此，本文针对这些不足，提出了一个将视觉场景与对话历史结合、并建模分级间接性的多模态基准。

Q3: 论文如何解决这个问题？

论文提出的解决方案是构建 READI 基准，将任务形式化为四选一的多模态视觉问答（VQA）形式，即视觉语用问答（V-PQA）。其设计步骤如下：首先，基于语用学理论，对间接指令言语行为进行分级间接性建模，使任务能区分从惯例化到非惯例化的不同程度。其次，设计韩语和英语的双语对话数据，每个样本包含一个图像和一个对话片段，对话中包含间接指令，正确答案要求从图像和上下文中推断出真实的指令意图。采用四选一格式可以减少开放式歧义，并支持对细粒度语用区分的受控评估。通过这种方法，READI 能评估模型是否真正整合了视觉上下文进行语用推理，而非仅仅依赖文本先验或显式线索。

Q4: 论文做了哪些实验？

从检索到的证据看，实验设计包括：1）在 READI 基准上评估多种最先进的多模态模型，比较它们在英文和韩文数据上的表现；2）分析模型性能随间接性水平增加的变化趋势；3）进行消融研究，特别是针对图像上下文对齐的消融，以检验任务是否真正需要多模态信息。虽然没有具体数值，但可以合理推断实验涉及多个模型、语言条件、间接性条件，并报告了准确率等指标。消融研究旨在证明 READI 不是纯文本任务，而是要求图像与对话整合的 genuinely multimodal 任务。

Q5: 发现了什么实验现象？

实验观察到的关键现象包括：最先进的多模态模型在视觉基础的 ISA 上的表现普遍较差，且随着间接性增加，性能显著下降。这表明模型在解释间接性方面存在根本性限制，尤其当意图需要依靠视觉场景的非字面线索时。消融研究的结果进一步表明，图像上下文对齐对任务至关重要，移除此信息后性能明显恶化，这支持了 READI 不是纯文本推理任务，而是真正需要多模态整合的论点。此外，跨语言对比（英语 vs 韩语）可能揭示了高语境语言中的额外困难，但具体差异证据尚不充分。总的来说，这些观察揭示了当前模型在语用推理上的薄弱环节，并为未来改进提供了方向。

Q6: 有什么可以进一步探索的点？

未来的探索方向包括：1）扩展 READI 到更多语言和文化语境，以验证跨语言的普适性和特殊性；2）丰富视觉场景的类型和复杂性，覆盖更多社交互动场景，如多人互动、隐含情绪、文化符号等；3）设计更细粒度的间接性标注，甚至连续尺度，以更好建模语用复杂度；4）探索如何将语用推理能力注入模型，例如通过专门训练任务、辅助目标或利用大型语言模型的思维链提示；5）研究模型在非惯例化间接言语行为上的失败模式，并开发针对性的可解释性分析；6）将基准扩展到其他语用现象，如讽刺、反语、隐喻等，形成完整的语用推理评估套件；7）结合用户个性化或社交角色信息，模拟真实 human-agent 交互中的语用适应。

Q7: 总结一下论文的主要内容

本文针对多模态环境下间接言语行为理解评估缺失的问题，提出了 READI 基准。间接言语行为要求对话者超越字面意义，结合语境推断真实意图，而现有模型和基准多聚焦于显式内容或感知识别，忽略了社会语用层面。READI 基于语用理论，将任务设计为四选一的视觉语用问答（V-PQA），并要求模型同时解读图像和对话，其中对话包含各级间接性的指令。基准覆盖英语和韩语，以探索高语境语言的特殊性。实验显示，当前最先进的多模态模型在 READI 上表现不佳，随着间接性提升，准确率明显下降，说明模型对间接意图的解释能力存在根本缺陷。消融研究证明图像上下文对齐至关重要，验证了任务的多模态本质。该研究的贡献在于提出了一套系统评估多模态语用理解的范式，并揭示了现有模型在语用推理上的短板，为未来改进提供了明确定位。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作与智能体（agent）研究方向相关，因为智能体需在交互中理解隐含意图，而 READI 提供了评估此类能力的多模态任务。

## 基本信息

- 作者：Jaehee Kim, Ji Hoon Chung, Seoyoon Park, Unsol Kim, Kyungwon Park, Ji Hak Kim, Yi-Jun Chen, Hansaem Kim
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30270v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，主要依据摘要、方法部分和消融实验的片段，并结合启发式草稿进行补全；部分实验细节和数值因证据不足而合理推断。
