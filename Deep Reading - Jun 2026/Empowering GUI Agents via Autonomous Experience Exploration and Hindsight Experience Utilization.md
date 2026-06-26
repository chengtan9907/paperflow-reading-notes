---
user_id: "cheng tan"
paper_id: 1675
arxiv_id: "2606.27330"
title: "Empowering GUI Agents via Autonomous Experience Exploration and Hindsight Experience Utilization for Task Planning"
publish_date: "2026-06-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.27330.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27330"
abs_url: "https://arxiv.org/abs/2606.27330"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-26T14:29:40"
---
# Empowering GUI Agents via Autonomous Experience Exploration and Hindsight Experience Utilization for Task Planning

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gui agent · task planning · multimodal llm · experience exploration

## 一句话总结

提出规划经验探索与利用（PEEU）方法，通过自主环境探索和事后经验利用生成高质量训练数据，显著提升小型多模态大语言模型在GUI任务规划中的跨网站泛化能力，7B模型准确率达30.6%，超越32B模型。

## 摘要

> Multimodal web agents can assist humans in operating repetitive GUI tasks, where effective task planning is essential for decomposing complex tasks into executable actions. While small open source MLLMs are cost efficient and privacy preserving compared with commercial large models, they suffer from weak planning and limited cross website generalization. To address these limitations, we introduce the planning experience exploration and utilization (PEEU) method, which autonomously explores environments to discover experiences and utilizes hindsight experience to synthesize strictly aligned, high level training data. To quantitatively analyze the generalization behaviors driving this performance, we propose the task decomposition hierarchical analysis framework (TDHAF) to systematically study compositional generalization across three task granularities: low, middle and high levels. Our analysis reveals that mastering low level atomic skills does not guarantee high level planning competence, while high level task training yields stronger OOD generalization. Experiments on real world benchmarks demonstrate PEEU's superior effectiveness: our 7B model achieves 30.6% accuracy, outperforming the much larger Qwen2.5-VL-32B model. These demonstrate constructing hindsight high level tasks and leveraging experiences is crucial for OOD planning abilities of small MLLMs.

Q1: 这篇论文试图解决什么问题？

小型开源多模态大语言模型（MLLM）在GUI任务规划中能力薄弱，主要体现在：(1) 难以将复杂任务分解为可执行的原子动作序列；(2) 跨网站泛化能力有限，在未见过的网站或任务上表现差。虽然大型商业模型性能好，但成本高、隐私有风险。因此，如何在不依赖大规模模型的情况下，提升小模型的规划能力和跨网站泛化能力是核心问题。

Q2: 有哪些相关研究？

相关工作包括：(1) 多模态网页代理：如WebAgent、Adept等，利用视觉语言模型操作GUI；(2) 任务规划方法：基于LLM的规划器、分层规划、利用记忆和经验增强规划；(3) 小型MLLM的泛化提升：通过数据增强、知识蒸馏等；(4) 组合泛化研究：分析低级技能与高级任务之间的关系。本工作聚焦于通过自主经验探索和事后经验利用来弥补小模型的规划缺陷，与先前方法（如MemoryBank、Reflexion）有概念上的联系但更强调结构化经验合成。

Q3: 论文如何解决这个问题？

提出PEEU框架，包含两个阶段：(1) 规划树探索（Planning Tree Exploration）：代理自主在GUI环境中探索，执行各种动作并记录结果，构建包含成功和失败路径的规划树，从而积累原始经验；(2) 事后经验利用（Hindsight Experience Utilization）：对规划树中的轨迹进行事后分析，提取关键决策点、错误修正和高层模式，合成严格对齐的高质量训练数据，用于微调小模型。此外，提出TDHAF（Task Decomposition Hierarchical Analysis Framework）用于系统评估组合泛化，将任务按粒度分为低（原子动作）、中（子任务）、高（完整任务）三级，分析能力迁移规律。

Q4: 论文做了哪些实验？

在真实网站基准上进行评估，包括信息搜索和导航任务。采用Qwen2.5-VL-7B作为基础模型，与Instruct Model基线以及更大模型Qwen2.5-VL-32B对比。在控制数据规模相同的条件下，比较跨网站泛化准确性。同时利用TDHAF进行消融实验，分别测试：(1) 仅用低级数据训练；(2) 仅用高级数据训练；(3) 混合训练，分析不同粒度对OOD泛化的影响。

Q5: 发现了什么实验现象？

(1) PEEU显著提升计划准确性：基于7B模型达到30.6%准确率，远超基线的7.8%，甚至超过32B模型（推测为20%左右但未明确给出）；(2) 组合泛化解耦发现：仅训练低级原子技能无法保证高层规划能力，高层任务训练产生更强的OOD泛化，且高层训练对域内低层任务也有较好覆盖；(3) 经验利用的关键作用：事后合成的数据质量更高，模型倾向于从全局规划而非局部动作中学习规律。

Q6: 有什么可以进一步探索的点？

(1) 扩展至敏感操作场景：当前未涉及登录、支付等安全敏感操作，未来可引入安全约束和模拟验证；(2) 任务类型拓展：从信息搜索和导航扩展到表单填写、购物等更复杂任务；(3) 跨域泛化：测试PEEU在不同领域网站（如医疗、金融）上的表现；(4) 动态网站适应性：处理页面状态变化、动态内容加载等；(5) 结合在线学习：使代理能持续从新交互中更新经验库；(6) 多模态理解增强：改进视觉理解以应对布局变化较大的网站。

Q7: 总结一下论文的主要内容

本文针对小型MLLM在GUI任务规划中规划能力弱、跨网站泛化差的问题，提出自主经验探索与事后经验利用方法PEEU以及分析框架TDHAF。PEEU通过让代理在环境中自由探索并记录轨迹（规划树），再对轨迹进行事后反思和结构化，合成高质量的高级训练数据。该方法无需人工标注，可自动生成多样化经验。TDHAF框架将任务分解为低、中、高三个粒度，系统研究组合泛化。实验在真实网站基准上进行，基于Qwen2.5-VL-7B的PEEU达到30.6%准确率，显著超过相同规模的基线（7.8%）和更大的32B模型。分析发现：低级技能掌握不保证高级规划能力，而高级任务训练带来的OOD泛化更强，且对低级任务也有效。结论表明，利用事后经验和构造高级任务是提升小模型规划能力的关键。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与智能体（Agent）方向直接相关，特别是GUI代理的任务规划与泛化。

## 基本信息

- 作者：Tianyi Men, Zhuoran Jin, Pengfei Cao, Yubo Chen, Kang Liu, Jun Zhao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.CV, cs.LG
- 日期：2026-06-26
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.27330`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据（abstract和introduction），并结合启发式草稿进行补充。
