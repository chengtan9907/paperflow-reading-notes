---
user_id: "cheng tan"
paper_id: 3622
arxiv_id: "2607.11503v1"
title: "GEIS: A Generation-Evaluation-Improvement Loop of Agent Skills for Long-Form Article Generation"
institution: "清华大学SPMI实验室，TasiTech有限公司"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11503v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11503v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:37:53"
---
# GEIS: A Generation-Evaluation-Improvement Loop of Agent Skills for Long-Form Article Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-form article generation · agent skills · evaluation loop · llm agents

## 一句话总结

GEIS通过将长文本生成分解为可检查、可复用的命名技能并构建生成-评估-改进循环，在维基百科风格文章生成中显著提升了质量，改进主要来自内容质量。

## 摘要

> Long-form article generation remains difficult for large language models because it combines long context, long instructions, and long outputs. Existing multi-agent pipelines such as STORM improve information coverage by simulating role-specialized agents, but their capabilities are often entangled in prompts and fixed procedures, making them hard to inspect, reuse, or iteratively improve. This paper presents GEIS (Generation-Evaluation-Improvement loop of agent Skills), a loop of named and declarative skills for Wikipedia-style long-form article generation. Implemented and evaluated in Tasi Harness, GEIS composes skills for article writing, browser-based evidence and image collection, diagram rendering, PDF-aware pairwise evaluation, and rule-level skill improvement. Its core writing skill follows Request, Plan, Draft, Audit, Refine, and Deliver; the pairwise evaluation skill produces structured quality reports; and the improvement skill maps recurrent findings into permanent patches to the writing skill in our 20-topic experiment. We evaluate GEIS on 20 Wikipedia Featured Article topics. Under the same generation backend, GEIS improves over the Tasi Harness default writer by 8.0 points on a 100-point PDF quality rubric and outperforms STORM on the two comparable writing dimensions, structural quality and content quality. In the 20-topic improvement experiment, the patched writing skill raises the average score from 82.90 to 86.95, with 17 out of 20 topics improved and the gain mainly coming from content quality. These results show that long-form generation can be reframed from a fixed workflow into an inspectable, modular, and evaluation-guided improvement loop.

Q1: 这篇论文试图解决什么问题？

长文本生成（如维基百科风格文章）面临长上下文、长指令和长输出的挑战。现有方法如STORM通过角色专门化智能体改善了信息覆盖，但其能力高度依赖提示工程，角色边界模糊、工具使用与过程纠缠、评估结果无法自然地反馈到可复用的写作规则中。这导致系统难以检查、调试、重用和迭代改进。因此需要一种将生成、评估和改进解耦并形成循环的方法。

Q2: 有哪些相关研究？

相关工作包括多智能体写作系统（如STORM，通过模拟不同角色改善信息覆盖）、协作语言模型（如PEER）、基于LLM的评估方法（LLM-as-a-judge）以及长文本生成的工作。STORM虽然提高了信息覆盖，但其能力纠缠在提示中；PEER提供了协作生成但未强调评估反馈循环；LLM-as-a-judge可提供自动评估但易受裁判方差影响。GEIS在这些基础上通过命名技能和显式改进循环实现更系统的方案。

Q3: 论文如何解决这个问题？

GEIS将长文本生成分解为一组命名且可声明的技能：文章写作技能、浏览器证据收集技能、图像收集技能、图表渲染技能、PDF感知成对评估技能和规则级技能改进技能。写作技能遵循Request（接收指令）、Plan（规划大纲）、Draft（撰写草稿）、Audit（审计草稿）、Refine（精炼）、Deliver（交付）的流程。评估技能对生成的文章进行成对比较，生成结构化质量报告（包括结构质量、内容质量等各项评分）。改进技能分析多次评估中反复出现的问题，将其转化为写作技能的永久规则补丁，从而在下一次生成中自动避免。整个系统形成生成-评估-改进的闭环。

Q4: 论文做了哪些实验？

实验在20个维基百科特色文章主题上进行。生成后端使用GPT-5.4（通过Tasi Harness），评估使用Qwen 3.5 Plus（通过qwen-code）。对比基线包括Tasi Harness默认写作器和STORM。评估指标为100分PDF质量标准，涵盖结构质量、内容质量等维度。主要实验：（1）GEIS vs 默认写作器：GEIS提升8.0分。（2）GEIS vs STORM：在结构质量和内容质量两个可比维度上GEIS更优。（3）改进实验：初始GEIS平均分82.90，经过一次改进（将评估中发现的问题打补丁到写作技能）后，平均分提升至86.95，20个主题中17个得到提升，且提升主要来自内容质量（而非结构质量）。

Q5: 发现了什么实验现象？

实验发现：（1）技能分离有助于长文本生成，将不同责任（写作、检索、图像收集、评估）从长提示中分离出来，使每个技能更专注。（2）改进循环的效果主要体现在内容质量上，结构质量变化不大，说明现有写作技能在结构规划上已经较好，内容质量和事实支持是主要改进点。（3）成对评估和结构化标准有助于减少LLM裁判的方差，但依赖LLM-as-a-judge的局限性依然存在（裁判方差、尺度漂移）。（4）20个主题中有3个未见提升，可能由于主题的特殊性（如需要特定领域知识或图像）或补丁的泛化性不足。

Q6: 有什么可以进一步探索的点？

未来工作包括：（1）减少对LLM-as-a-judge的依赖，引入专家审查和更细粒度的自动评估。（2）探索技能的自动发现和组合，而非手动定义技能集。（3）将GEIS扩展到其他类型的生成长文本（如学术论文、技术报告、新闻文章）。（4）更系统化的改进策略，如基于强化学习的技能优化。（5）提高技能的可迁移性和跨领域适应性。

Q7: 总结一下论文的主要内容

长文本生成是NLP领域的挑战，现有方法如STORM通过多智能体协作提高了信息覆盖，但存在能力纠缠、难以迭代改进等问题。本文提出GEIS，一种生成-评估-改进循环的智能体技能框架，将长文本生成分解为命名技能。论文首先分析现有工作的不足，然后详细描述GEIS的设计：写作技能遵循Request-Plan-Draft-Audit-Refine-Deliver流程，评估技能进行成对比较并输出结构化报告，改进技能从评估中提取模式并修改写作技能的规则。在Tasi Harness平台上实现，作者在20个维基百科特色主题上进行了系统实验。使用GPT-5.4生成，Qwen 3.5 Plus评估。结果显示，GEIS在100分PDF质量标准上比Tasi Harness默认写作器提升8.0分，且优于STORM在结构质量和内容质量上的表现。更重要的是，通过一次改进循环，写作技能的平均分从82.90提升至86.95，其中17个主题得到改进，且提升主要来自内容质量。这些结果表明，将长文本生成范式从固定工作流转向可检查、模块化、评估引导的改进循环是有效的。论文还讨论了依赖LLM-as-a-judge的局限性，并提出了未来方向。总体而言，GEIS为长文本生成提供了一个系统化的框架，强调了技能分离和持续改进的重要性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体（Agent）方向直接相关：论文提出的GEIS框架是一种基于多技能的智能体系统，强调技能的可命名、可检查和可改进。

## 基本信息

- 作者：Jiale Zhang, Juntao Hu, Zhijian Ou
- 机构：清华大学SPMI实验室，TasiTech有限公司
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11503v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据（retrieved_evidence）。
