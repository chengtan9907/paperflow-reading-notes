---
user_id: "cheng tan"
paper_id: 10012
arxiv_id: "2608.30241v1"
title: "PaperBanana-Interact: Scientific Diagram Refinement with Multi-Turn Human Feedback"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30241v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30241v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:45:43"
---
# PaperBanana-Interact: Scientific Diagram Refinement with Multi-Turn Human Feedback

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：diagram quality · diagram generation · scientific diagram · multi-turn · paperbanana-interact · turns · user

## 一句话总结

本文针对科学图表多轮生成中单轮难以满足作者视觉偏好与沟通需求的问题，提出多轮图表生成基准 MTPaperBananaBench 和基于内部批评-改进循环的多智能体系统 PaperBanana-Interact，有效缓解了跨轮次的质量漂移与需求遗忘两大失败模式。

## 摘要

> Recent efforts have aimed to automate scientific diagram generation from paper content (Lin et al., 2026; Zhu et al., 2026a). However, fully satisfying an author's visual and communicative preferences in a single turn is challenging: in our formative user study (N = 14), all participants requested further revisions after viewing an initial draft, and 86% of them rated the refined diagrams as more satisfactory. Despite the clear demand, the multi-turn workflow remains largely underexplored. To bridge this gap, we present MTPaperBananaBench, a benchmark for multi-turn diagram generation containing 292 images annotated with 3,518 user requirements. To reduce expensive human studies and enable scalable benchmarking, we construct a user simulator that, at each turn, identifies unsatisfied requirements and converts k of them into natural language feedback. Evaluating both requirement satisfaction and overall diagram quality reveals two key failure modes shared across baseline multiturn systems: (1) quality drift, where diagram quality progressively declines over turns, and (2) forgetting, where previously implemented features are lost in subsequent turns. To address these issues, we introduce PaperBanana-Interact, a multi-agent system that refines diagrams via an internal critique-and-refine loop. PaperBanana-Interact consistently improves rather than degrades diagram quality across turns, outperforming baselines by 11.9-18.6 points in quality score and reducing forgetting by 3.7-6.2 points.

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：科学图表的自动化生成虽然已取得进展，但单轮生成难以完整满足作者对图表内容、布局和视觉表征的个性化需求。作者通过形成性用户研究（N=14）确认了这种差距：所有参与者都要求进一步修改，表明多轮交互是真实且未满足的需求。然而，现有研究大多聚焦于单轮生成，多轮图表生成工作流“在很大程度上未被探索”（原文语）。该任务面临的独特挑战包括：（1）反馈是累积性的——当前轮次的修改必须同时考虑所有先前轮次提出的需求，而不是只响应最新一条指令；（2）系统需要在较长的多图像交互历史中保持上下文一致性，避免在实现新需求时覆盖或丢失此前已经接受的功能；（3）图表质量需要随轮次保持甚至提升，而不是在反复修改中退化；（4）缺少合适的评测协议——真实用户测试成本高，且很难大规模重复，因此需要可扩展的用户模拟器来驱动评测。论文进一步通过系统化基准测试揭示了基线系统的两种典型失败模式：质量漂移（例如 NanoBananaPro 在轮次中质量分数从 50.3 降至 19.0，PaperBanana-DirectRefine 则从 50.3 降至 47.1）和遗忘（后续细化覆盖了此前已实现的特征），这两者共同构成了多轮科学图表生成的核心瓶颈。

Q2: 有哪些相关研究？

相关工作主要分布在几个方向：（1）文本到图像生成与细化，生成模型在文本引导图像生成方面已具备强大能力（如 Cao et al., 2025; Google DeepMind, 2025; Meta, 2026; OpenAI, 2026），但大多是一步到位式生成，缺少多轮交互式细化；（2）科学图表自动生成，早期工作通过生成代码来渲染图表，例如使用 TikZ（Belouadi et al., 2024a,b, 2025; Zhang et al., 2025）或 PythonPPTX（Pang et al., 2026; Zheng et al., 2025），这类方法利用代码结构保证图表可编辑性，但缺少自然语言反馈的闭环；（3）个性化与顺序文本到图像生成，如 Nabati et al. (2024) 的 Personalized and sequential text-to-image generation，涉及多轮个性化偏好建模，与本文的连续反馈设置相关；（4）反馈细化和智能体系统，已有系统利用多轮对话或批评模型改进生成结果，但较少专门针对于科学图表的长期交互与需求累积场景。本文的贡献在于将多轮人类反馈引入科学图表生成，并系统化地构建基准和评测协议。此外，相关证据指出评测中考虑了内容、布局和视觉表征等方面，且评估在模拟用户固定交互预算下进行。整体来看，本文填补了多轮科学图表生成这一研究空白。

Q3: 论文如何解决这个问题？

论文的方案分为三部分：（1）基准构建——MTPaperBananaBench 包含 292 张图像，每张图像由人类专家标注了多条需求（总计 3,518 条），需求涵盖内容、布局和视觉表征三个维度；在评测时生成系统与模拟用户进行固定轮次的交互，最终输出图表再从需求满足度和整体质量两个维度评估。（2）用户模拟器——为了减少昂贵的人工研究并支持规模化评测，论文训练了一个用户模拟器，它在每一轮查看当前图表和已提出的需求，识别尚未满足的需求，并将其中 k 个转化为自然语言反馈（文中分别考虑 k=1 和 k=3 两种设置）。模拟器可以逐步揭示隐藏需求，模拟真实用户在多轮中不断提出新要求的模式。（3）PaperBanana-Interact——为应对质量漂移和遗忘，论文提出多智能体系统，其核心包括一个多目标批评器，它同时评估多个约束：当前用户请求、所有先前用户请求、对源上下文的遵循程度以及图表表现质量。批评器输出对当前图表的诊断性反馈，然后生成器（或细化器）根据批评进行改进，构成内部批评-改进循环。此外，系统还设计有处理长多图像交互历史的机制（证据片段提到“To handle long, multi-image interaction histories...”），表明有专门的记忆或上下文管理策略。通过这种循环，系统在每一轮都能在保持已有功能的同时纳入新要求，从而避免质量下降和遗忘。

Q4: 论文做了哪些实验？

论文的实验围绕三部分展开：（1）形成性用户研究：招募 14 名有学术写作和科学插图经验的参与者，验证多轮交互的实际需求；结果显示所有参与者都在初稿后要求进一步修改，86% 认为细化后的图表满意度更高。（2）基准测试：在 MTPaperBananaBench 上，使用 k=1 和 k=3 两种用户模拟器设置，系统化评估多种基线，包括直接生成模型（如 NanoBananaPro）和智能体基线（如 PaperBanana-DirectRefine），对比内容需求满足度（Req）和整体图表质量分数。（3）主要结果的报告：论文在正文（Table 1 和 Table 2）中给出了各系统在两种模拟器设置下的详细对比。实验还测算了质量漂移和遗忘的具体程度，例如 NanoBananaPro 的图表质量从 50.3 跌至 19.0，PaperBanana-DirectRefine 从 50.3 微降至 47.1。这些实验设计旨在揭示多轮交互中基线系统的失败模式，并验证 PaperBanana-Interact 能否持续改进。

Q5: 发现了什么实验现象？

实验揭示的主要现象有：（1）需求满足度随轮次稳步提升——当模拟用户逐轮揭示隐藏需求时，多轮交互普遍提升了 Req 指标，说明多轮反馈确实能捕捉更多用户偏好；（2）质量漂移——基线生成模型在轮次推进中图表质量显著下降，NanoBananaPro 从 50.3 跌至 19.0，几乎损失六成质量；即便是专门的多轮细化基线 PaperBanana-DirectRefine 也有小幅下滑（50.3→47.1），表明质量保持是多轮生成的普遍难题；（3）遗忘——后续细化会覆盖先前实现的功能，导致已满足的需求在后续轮次中重新失效；（4）相比之下，PaperBanana-Interact 在多轮中持续提升质量，而不是下降，最终质量分数比最佳基线高出 11.9-18.6，遗忘指标降低 3.7-6.2，说明内部批评-改进循环能够有效缓解两个失败模式。值得注意的是，这些结果是在模拟用户环境下取得的，真实用户反馈的分布可能与模拟器存在差异，因此观察到的改进幅度可能不能完全外推。

Q6: 有什么可以进一步探索的点？

基于论文的框架，可能的进一步探索方向包括：（1）改进用户模拟器的真实性——当前模拟器按固定预算逐步揭示需求，未来可学习真实作者的反馈风格，或引入多模态反馈（如点击区域、拖拽操作、视觉标记）而非纯文本；（2）扩展图表类型和领域——当前基准集中于科学论文图表，可扩展到更广义的数据可视化、海报、幻灯片等场景；（3）更强的记忆与冲突解决机制——论文只初步处理了长交互历史，后续可探索显式知识库、需求冲突检测和主动询问澄清等更复杂的人类-AI 协作界面；（4）结合代码生成与直接图像编辑——现有方法或基于代码（TikZ、PythonPPTX）或直接生成图像，融合两者可能同时获得可编辑性和视觉质量；（5）评估协议升级——从自动指标和模拟器评估转向更大规模的人类评估，研究用户满意度、信任度和效率等指标；（6）将交互式细化应用到其他文档生成任务，如论文表格、算法伪代码示意图等。

Q7: 总结一下论文的主要内容

论文围绕科学图表的多轮生成与细化展开。作者先通过 14 人的形成性用户研究确认了单轮生成的不足——所有参与者都需要修改，且大多数认可多轮修改的价值。为了系统研究这一任务，他们构建了 MTPaperBananaBench 基准，包含 292 张图像和 3,518 条由人类专家标注的需求，需求覆盖内容、布局和视觉表征。为了实现可扩展评测，他们训练了一个用户模拟器，在每个回合识别未满足需求并生成 k 条自然语言反馈，并采用固定交互预算来模拟真实用户的多轮反馈过程。评测包括需求满足度和整体图表质量两个维度。利用该基准，他们系统化测试了多种生成模型基线和智能体基线，发现了两个跨系统共存的失败模式：质量漂移（随轮次图表质量退化）和遗忘（之前实现的功能被后续轮次覆盖）。为此，他们提出 PaperBanana-Interact，一个多智能体系统，其核心是多目标批评器，同时评估当前用户请求、全部先前请求、源上下文遵循程度和图表表现质量，并通过内部批评-改进循环迭代细化。在 k=1 和 k=3 两种模拟器设置下，PaperBanana-Interact 展现出与传统基线不同的行为——它能在轮次中持续提升而非降低图表质量，质量分数比基线高 11.9-18.6 分，遗忘现象减少 3.7-6.2 分。该工作是多轮科学图表生成领域的首个系统性基准和方法，揭示了多轮交互中特有的挑战，并提供了一种可扩展的解决方案。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 References / 2 Related Work 部分。

## 基本信息

- 作者：Xueqing Wu, Ashwin Balasubramanian, Bingxuan Li, Dawei Zhu, Kai-Wei Chang, Yale Song, Yiwen Song, Rui Meng, Tomas Pfister, Nanyun Peng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.CV
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30241v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。
