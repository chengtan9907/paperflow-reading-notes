---
user_id: "cheng tan"
paper_id: 377
arxiv_id: "2606.16988v1"
title: "Agent trajectories as programs: fingerprinting and programming coding-agent behavior"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16988v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16988v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:07:47"
---
# Agent trajectories as programs: fingerprinting and programming coding-agent behavior

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：coding agent · procedural fingerprint · trajectory analysis · SWE-Bench

## 一句话总结

本工作引入过程指纹（procedural fingerprint）概念，从代码智能体轨迹中恢复可识别行为模式，并以85.7%准确率将未见轨迹归因于正确智能体，同时开发ProcGrep库实现确定性的轨迹搜索与分析。

## 摘要

> 2026
> Benchmark scores tell you what an agent got right; they do not tell you how it got there.
> In this work, we introduce methods for comparing agents procedurally in different contexts,Jun where the model, tasks, and approaches vary. We compare ten agents and find that they are
> identifiable by their behavioral habits, which we define as fingerprints: a probe over these
> procedural signatures attributes an unseen trajectory to the correct agent at 85.7% accuracy.15
> We develop procedural representations for agent problem-solving procedures with an emergent
> vocabulary induction technique which is meant to be maximally compressive to avoid surface-level
> variation while being expressive enough to unveil the quirks of the models’ patterns. We apply
> our framework to the software engineering evaluation dataset SWE-Bench to study the structural
> distinctness of agent trajectories and find that behavior is most similar between models from[cs.SE] similar release periods and those that are distilled from one another (e.g., a distilled student model
> and its teacher have a Jensen-Shannon divergence of 0.25, about half the distance between other
> model pairs). As more models saturate evaluations, we believe that it will be important to probe
> model behavior along more holistic dimensions than success rates alone. We introduce ProcGrep,
> a library for auditing and evaluating agents for how they approach tasks at a procedural level
> given their traces in a top-down fashion. One application of ProcGrep is searching over past
> events. We show that ProcGrep outperforms LLMs on episodic search in both efficiency and
> accuracy, offering a deterministic, programmable approach to searching over traces. We believe
> this work has a range of applications to help developers work with and program coding agents,
> such as task-aware model routing, agent monitoring, and finer-grained cost analysis.1

Q1: 这篇论文试图解决什么问题？

当前代码智能体评估主要依赖基准分数，仅反映结果而不揭示行为过程，导致以下问题：1) 无法区分不同智能体在解决问题方式上的差异；2) 缺少工具对智能体轨迹进行系统化比较和分析，尤其当模型、任务、方法各异时；3) 链式思维虽用于事后解释，但不可作为行动的可信依据（Guan等2025，Ross等2025，Wei等2023）；4) 传统程序分析过于静态，难以支持智能体动态行为的可靠扩展（Austin等2021，Chen等2021等）。因此，需要一种新的表示和分析框架来捕捉智能体轨迹中的过程性行为，使得开发者能够理解、比较和编程智能体的工作风格。

Q2: 有哪些相关研究？

相关工作涵盖几个方向：1) 传统程序分析：静态分析工具如Austin等2021、Chen等2021，以及SWE-Bench等评估基准（Jimenez等2024, Yang等2024），这些方法无法处理智能体行为的动态性和上下文依赖性。2) 链式思维（Chain-of-Thought）：Wei等2023提出用自然语言解释推理过程，但后续研究（Guan等2025, Ross等2025）指出其并非行动的可信记录，易产生事后合理化。3) 集体智慧：Alizadeh等2015、Surowiecki2004等研究利用群体行为模式，本文从中借镜，但重点转向智能体轨迹聚合分析。4) 智能体行为分析：一些工作尝试通过动作序列、工具使用等维度描述智能体，但缺乏统一的过程表示。本文区别于上述工作，首次提出从轨迹中提取程序指纹作为可比较、可审计的抽象表示，并配套开源工具ProcGrep。

Q3: 论文如何解决这个问题？

本文提出一套过程分析工具包。首先定义程序指纹（procedural fingerprint），即从智能体轨迹中提取的可区分特征集合，不依赖模型内部状态或自述报告。指纹通过以下步骤获得：1) 将原始轨迹（工具调用、代码生成、文件修改等）分割为原子动作序列；2) 采用涌现词汇归纳（emergent vocabulary induction）技术对动作进行聚类，生成一组离散的“行为词汇”，旨在最大程度压缩表面变体而保留过程特异性；3) 基于这些词汇构建每个智能体的行为频次分布，形成指纹向量。接着，利用指纹计算任意两个智能体之间的Jensen-Shannon散度，量化过程相似性。此外，开发ProcGrep库，实现自顶向下的轨迹审计：允许开发者编写类似grep的规则对历史轨迹进行模式搜索，无需调用LLM，搜索过程完全确定且高效（例如定位特定代码片段或错误处理模式）。整体上，该方法为智能体行为提供了结构化、可比较的表示，支持归属分析、相似性度量、失败模式诊断以及基于部分轨迹的结果预测。

Q4: 论文做了哪些实验？

实验基于SWE-Bench软件工程评估数据集，涵盖十种不同智能体：包括Claude-4、Claude-3/3.5、GPT-4、SWE-agent-LM-32B及其蒸馏版本、专有模型等。主要实验：1) 指纹归属准确性：将单个轨迹缩小为指纹表示，训练分类器识别其所属智能体，达到85.7%准确率（随机基线11%），其中Claude-4和SWE-agent-LM-32B最可识别（F1>0.70），GPT-4与Claude-3 Opus最易混淆（F1 0.20-0.30）。2) 行为相似性分析：计算各对智能体的JS散度，发现蒸馏学生-教师对散度仅0.25，约为其他典型模型对的一半；相近发布时期的模型（如Claude-3.5与Claude-4）也表现出较高相似性。3) 区分动作对：识别出每个智能体最具区分度的动作序列（例如Claude-4偏好先读取整个项目再编辑，而GPT-4倾向于逐文件修改）。4) 结果预测：利用早期轨迹的部分指纹预测最终任务成功率，准确率提升显著（基模型准确率13%→46%+33%；下一阶段18%→50%+32%），表明过程线索可用于提前终止无用计算。5) ProcGrep搜索性能：在历史轨迹中搜索特定事件（如特定函数修改），ProcGrep相比调用LLM的基线方法，效率提升数个数量级，且准确率持平或更高。

Q5: 发现了什么实验现象？

关键实验现象：1) 程序指纹是真实存在的属性：不同架构的智能体可通过其轨迹痕迹被可靠识别，表明行为模式嵌入模型设计、训练数据和脚手架。2) 蒸馏模型与教师模型行为高度相似（JS散度0.25），远小于其他模型对，说明知识蒸馏不仅传递能力，也传递过程习惯。3) 发布时期相近的模型（如Claude-3.5与Claude-4）更易混淆，暗示智能体行为存在“时代风格”。4) 一些过程特征与成功/失败强相关：例如过度依赖单一工具、搜索范围过小或频繁回退的轨迹最终成功率较低。5) 部分轨迹足够预测结果：仅用前三步动作特征即可显著提升预测准确率，启示基于过程的早期停止策略。6) ProcGrep在搜索任务上表现出确定性：无需随机采样或生成，结果可重复且完全可审计。7) 反直觉现象：GPT-4和Claude-3 Opus虽模型差异大，但行为混淆度最高（F1约0.25），可能因为两者都偏向于“保守修改”模式。

Q6: 有什么可以进一步探索的点？

可探索方向包括：1) 将框架扩展至非代码智能体领域（如对话、机器人操作等），验证指纹的一般性。2) 利用ProcGrep在训练阶段引导模型学到更可取的轨迹模式（例如减少冗余动作）。3) 任务感知的模型路由：基于任务类型与智能体指纹的匹配度，动态选择最佳模型。4) 实时监控：在智能体执行过程中持续检测指纹偏离，触发告警或干预。5) 更细粒度的指针对比：引入动作时间、资源消耗等多模态信息。6) 指纹的鲁棒性研究：面对轨迹长度、噪声、对抗性扰动时的稳定性。7) 跨任务泛化：训练指纹分类器在新任务上识别未见模型。8) 结合人类偏好优化：通过过程而非结果指标对智能体进行反馈。9) 将ProcGrep集成到CI/CD管线，自动化审计智能体行为回归。

Q7: 总结一下论文的主要内容

本文以代码智能体为对象，系统性地提出了从过程层面理解和比较智能体行为的新范式。核心论点：传统以结果为中心的基准无法捕捉解决问题的风格差异，而轨迹中蕴含丰富的过程信息。作者首先定义了程序指纹（procedural fingerprint），即从智能体原始轨迹中通过涌现词汇归纳获得的高阶抽象表示，能够区分不同模型的行为习惯。通过对SWE-Bench上十种智能体的实验，验证了指纹的可归属性（85.7%准确率）、模型间相似性与发布时期和蒸馏关系的关联（如教师-学生对JS散度仅0.25），并识别出每种智能体的独特行为模式。进一步，作者开发了ProcGrep库，提供自顶向下的轨迹搜索能力，在情节搜索上超越LLM方法的效率和确定性。应用方面，展示了利用早期轨迹的部分指纹预测最终成功率，证明过程线索可以用于减少计算浪费。论文还讨论了该框架在任务路由、监控和成本分析等方面的潜力。总体而言，该工作为智能体行为的可解释性、可比较性和可编程性提供了工具基础和理论验证，弥补了从“智能体做了什么”到“如何做”之间的评估鸿沟。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与智能体研究直接相关：提供了从过程角度理解和比较智能体行为的方法。

## 基本信息

- 作者：Hamidah Oderinwale
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SE, cs.LG
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16988v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据，尤其是Abstract、Introduction和Conclusion的片段，并结合heuristic_draft进行润色和补全。
