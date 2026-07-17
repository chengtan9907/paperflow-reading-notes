---
user_id: "cheng tan"
paper_id: 4290
arxiv_id: "2607.15193v1"
title: "Plover: Steering GUI Agents through Plan-Centric Interaction"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.15193v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.15193v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-18T00:53:59"
---
# Plover: Steering GUI Agents through Plan-Centric Interaction

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gui automation · plan-centric interaction · vision-based agent · human-ai collaboration

## 一句话总结

Plover是一个以计划为中心的GUI自动化系统，通过外化任务计划和重规划为可检查、可修改的制品，使用户能够通过局部干预（尤其是自然语言指导）修复自主代理的许多失败，这些失败往往源于局部上下文缺失而非任务分解错误。

## 摘要

> Graphical user interface (GUI) automation remains challenging in real-world environments, where dynamic layouts, unexpected dialogs, and evolving interface states can cause autonomous agents to drift from user intent. Recent vision-based multimodal agents improve flexibility by operating directly over screenshots and natural language instructions, but planning and adaptation often remain internal, limiting users' ability to inspect, supervise, or correct system behavior. We present Plover, a plan-centric vision-based GUI automation system that externalizes task plans and replanning as persistent, inspectable, and revisable artifacts. Through a planner--executor architecture, Plover supports explicit supervision of evolving execution, localized correction through editable plans, natural-language guidance, and screenshot-grounded interventions, while preserving prior progress during repair. A formative study with six participants informed the interaction design. We then evaluate Plover through benchmark failure-case repair and scenario-based workflow analyses. Our results show that many autonomous GUI-agent failures are structurally repairable when plans remain visible and interventions are localized, and that explicit replanning helps make GUI automation more transparent, controllable, and adaptable.

Q1: 这篇论文试图解决什么问题？

论文试图解决现有GUI自动化系统在真实环境中的两个核心问题：1）自主代理因动态布局、意外对话框等环境变化而偏离用户意图；2）代理的规划和适应过程内化，用户无法监督和纠正其行为，导致失败不可恢复。需要一种使计划和重规划过程外化、可检查、可干预的自动化范式。

Q2: 有哪些相关研究？

相关工作涵盖GUI自动化代理和可解释AI系统。传统方法基于坐标、DOM树或图像模板，对环境变化鲁棒性差。近期视觉语言模型（VLM）驱动的代理（如AppAgent、CogAgent）通过截图与自然语言指令提升灵活性，但规划和适应仍作为黑箱操作，缺乏计划层次的透明性。混合倡议规划和心智模型外化在人机交互领域有探索，但在GUI代理中应用有限。Plover通过外化计划制品并支持局部修正，弥补了现有系统在可检查性和可干预性上的不足。

Q3: 论文如何解决这个问题？

Plover的系统架构包括两个核心模块：规划器（Planner）和执行器（Executor）。规划器根据用户指令和当前界面状态生成或更新结构化任务计划（步骤序列），并将计划持久化存储。执行器将计划步骤转化为具体GUI操作（点击、输入等），保持与规划器的严格分离，确保执行确定性。关键在于计划作为外化制品呈现给用户，用户可通过界面直观查看计划、修改步骤顺序或内容、添加自然语言指导，或在截图上标注以干扰执行。智能重规划（Intelligent Replanning）机制允许用户在计划更新前预览变更，并选择接受或修订。支持四种干预模式：计划编辑、自然语言指导、多模态标注、完整手动覆盖，系统优先采用轻量级模式。

Q4: 论文做了哪些实验？

实验分为三部分。首先进行形成性研究（n=6），通过半结构化访谈和原型交互了解用户在与GUI代理协同中的常见挑战（计划制定、执行监控、失败恢复），为Plover交互设计提炼需求。然后使用自主代理在公开基准测试中未成功的26个案例进行修复实验：参与者在Plover环境中使用不同干预模式修复每个失败，记录修复成功率和所用方式。最后开展场景工作流分析，让参与者在复杂多步骤任务中（如在线预订、表单填写）使用Plover完成操作，收集任务完成时间、干预次数和主观可用性评价。实验通过日志、屏幕录制和问卷收集数据。

Q5: 发现了什么实验现象？

实验揭示了多个关键现象：1）大部分自主代理失败（26例中的多数）可通过局部干预成功修复，且修复通常仅需简短自然语言指导（如“跳过此对话框”），表明失败根源为局部上下文缺失（如未预见的弹出窗口）而非整体任务分解错误。2）失败模式可分类：遗漏必要步骤、步骤顺序错误、对界面异常状态处理不当等，不同类型倾向最优干预方式不同。3）明确外化计划显著降低了用户重建系统状态的心理负担，用户报告干预次数减少，干预疲劳感降低。4）用户对计划编辑和自然语言指导模式接受度高，但在复杂或高度不确定场景下仍会依赖多模态标注或手动覆盖。

Q6: 有什么可以进一步探索的点？

可能的探索方向包括：1）将Plover扩展到更广泛的GUI领域（移动端、跨平台桌面、工业软件），测试跨域泛化能力。2）集成更强大的规划器（如大语言模型规划模块），自动生成高质量初始计划并利用用户修复反馈进行在线改进。3）开展更大规模用户研究（跨群体、跨任务复杂度），评估不同干预模式的效率与用户偏好。4）探索自适应混合自主程度：根据任务风险、用户熟练度和历史交互动态调整系统自主程度。5）结合强化学习从用户修复中学习失败模式，减少未来需要人工干预的频率。

Q7: 总结一下论文的主要内容

本文提出Plover，一个以计划为中心的GUI自动化系统，旨在解决自主代理在动态真实环境中行为不透明、用户难以介入纠正的问题。核心思路是将任务计划（即步骤序列）从内部推理状态转为持久、可视、可编辑的外化制品。系统采用规划器-执行器分离架构：规划器基于用户指令和当前截图生成结构化步骤，执行器逐步执行具体交互，二者通过计划制品解耦。用户界面始终显示当前计划及执行进度，支持四种干预模式：（1）直接编辑计划步骤，（2）用自然语言添加约束或指引，（3）在截图上标注目标元素或位置，（4）在极端情况下完全手动接管。智能重规划机制允许用户在计划更新前预览差分并确认。为验证设计，作者先进行了6人形成性研究，梳理出计划制定、执行监督、失败恢复三类挑战；然后选取26个自主代理在基准任务上的失败案例，让参与者用Plover修复，发现大部分失败通过简单自然语言引导即可恢复，说明失败多源于局部上下文而非整体规划错误。进一步场景分析展示了系统在真实多步任务中的实用性。主要贡献包括：提出计划中心交互新范式；实现完整系统并开源；实证揭示自主GUI失败的可修复性规律；为混合倡议代理设计提供设计建议。局限在于评估样本较小，尚未验证跨平台泛化，且系统优先轻量干预，对需完全重写的复杂场景支持有限。总体而言，Plover证明了计划透明化和局部干预机制能显著提升GUI代理的可控性和用户信任。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：密切关联智能体（agent）方向，聚焦自主GUI交互与人类监督的协同

## 基本信息

- 作者：Madhumitha Venkatesan, Shicheng Wen, Jiajing Guo, Jorge Piazentin Ono, Liu Ren, Dongyu Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.15193v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了PDF语义检索命中的证据片段（abstract、3.1、4.3、4.5、5.3、6等章节内容），并结合heuristic_draft进行补全。
