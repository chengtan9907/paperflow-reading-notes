---
user_id: "cheng tan"
paper_id: 5732
arxiv_id: "2607.25236v1"
title: "VisualPatchWorld: Code World Models as Latent Structured Representations for Planning"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25236v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25236v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:54:48"
---
# VisualPatchWorld: Code World Models as Latent Structured Representations for Planning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：code world model · planning · model-predictive control · qualitative dynamics

## 一句话总结

VisualPatchWorld 将世界动力学表示为可检查的代码，通过主动探测选择定性动力学形式并拟合参数，从而支持模型预测控制和规划。

## 摘要

> Different research lines use the term world model in different ways, yet they share a common aim: to capture how the world evolves under action in a form that supports perception, simulation, and planning. Two prominent realizations are neural predictors that learn dynamics in continuous vector spaces, and hand-built physics engines that expose explicit state and physical laws. Neural predictors scale from data but leave the form of the dynamics implicit; physics engines are inspectable and editable but difficult to construct at scale. We introduce VisualPatchWorld (VPW), which represents world dynamics as code. VPW first selects a qualitative dynamical form with short active probes, then fits that form's free parameters from recorded state-action traces by minimizing multi-step prediction error. The resulting programs can be rolled forward like a simulator, inspected in source form, and used inside model-predictive control; image-derived scene graphs can supply the live state at replan time. Across comparisons with prior code-based world models, VPW attains 69.0% mean planning success and exceeds the strongest code baseline by 23.5 points. The largest gains arise when choosing the correct qualitative dynamics is essential. Under the same planner, the induced models approach ground-truth engine success on navigation and grasp-rich control; a residual gap remains for contact-rich pushing, and checking a shortlist of promising plans in the engine closes most of that gap. These results establish a practical route toward automatically constructed code world models that are useful for planning. Code is available at https://github.com/HKBU-KnowComp/VisualPatchWorld/.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决从交互轨迹中自动恢复结构化的、可检查的、可执行的世界模型用于规划的问题。现有的世界模型主要有两类：神经网络预测器和手工物理引擎。前者虽然可扩展且有效，但动力学形式是隐式的，当预测错误时难以调试或编辑；后者是可检查且可编辑的，但很难大规模构建。因此，论文探索如何自动构建介于两者之间的代码世界模型，即同时具备可检查性、可编辑性和可执行性，并支持规划。

Q2: 有哪些相关研究？

相关研究包括：（1）神经网络世界模型，使用各种架构（如 RNN、Transformer）在高维连续空间中学习动力学，但缺乏显式结构；（2）代码生成方法，如 Code as Policies，将 LLM 用于生成 robot 策略，但通常不用于学习世界模型；（3）基于符号表示的世界模型，如场景图或状态机，但往往需要预定义的状态表示和转换规则。本文的方法结合了代码生成和参数学习，从轨迹中自动合成结构化的动力学程序。

Q3: 论文如何解决这个问题？

VPW 方法包括四个阶段：（1）场景解析：将每个观察（如图像）转换为结构化的场景图，记录物体姿态等；（2）定性动力学选择（Level 1）：使用主动探测（short active probes）选择正确的动力学形式，如线性运动、力控、抓取门控运动等；（3）参数拟合（Level 2）：从离线状态-动作轨迹中拟合所选动力学形式的参数，通过最小化多步预测误差；（4）模型预测控制：利用拟合的程序作为模拟器，使用交叉熵方法（CEM）滚动预测并选择最优动作。此外，图像派生的场景图可在重新规划时提供实时状态。

Q4: 论文做了哪些实验？

论文在多个模拟机器人任务上评估 VPW 的规划性能。任务包括线性导航（LinearNavigation）、力控滑动（ForceControlledSliding）、抓取门控运动（GripGatedMotion）、关节空间臂运动学（JointSpaceKinematics）以及接触丰富的推动（ContactRichPushing）等。对比基线包括之前的代码世界模型方法（如直接代码生成、参数拟合基线等）。实验设置使用真实物理引擎（如 PyBullet）作为 ground truth，比较规划成功率。另外进行了消融研究，分析 Level 1 选择正确动力学形式的影响，以及 Level 2 参数拟合的重要性。

Q5: 发现了什么实验现象？

主要观察结果：（1）VPW 在总体规划成功率达到 69.0%，超过最强代码基线 23.5 个百分点；（2）最大的改进出现在需要正确选择定性动力学形式的任务上；当动力学形式选择错误时，即使参数拟合得很好，规划也会失败；（3）在导航和抓取门控任务上，VPW 的规划成功率接近使用真实物理引擎的规划器；（4）在接触丰富的推动任务上，VPW 仍然有显著差距，但通过用真实引擎检查少量候选计划（shortlist checking），可以弥补大部分差距；（5）消融实验表明，仅使用参数拟合（Level 2）而不进行正确的形式选择，性能较差，说明 Level 1 的定性选择至关重要。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向：（1）扩展 VPW 到更复杂的动力学形式，如非线性、连续状态依赖的动力学；（2）学习更一般的场景表示，用于非结构化或部分可观察的环境；（3）结合神经网络和代码的混合模型，以处理更细粒度的动力学；（4）将 VPW 应用于真实机器人系统，处理传感器噪声和现实世界的不确定性；（5）自动发现和利用因果结构，以改善样本效率。

Q7: 总结一下论文的主要内容

这篇论文提出了 VisualPatchWorld (VPW)，一种将世界动力学表示为代码的方法。VPW 通过两个级别从交互轨迹中自动构建可执行、可检查的代码程序：Level 1 选择定性动力学形式，Level 2 拟合具体参数。生成的代码可用于模型预测控制。实验表明，VPW 在多个规划任务上显著优于先前的代码世界模型，尤其是在需要正确动力学形式的任务上。VPW 在导航和抓取任务上接近使用真值引擎的规划器，但在接触丰富的推动任务上仍有差距，可通过检查少量候选计划来弥补。论文展示了代码世界模型作为神经网络和物理引擎之间有效折中的实用路线。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文的核心问题——从数据中恢复结构化世界模型——对智能体学习与规划领域具有通用参考价值

## 基本信息

- 作者：Jiaxin Bai, Jiaxuan Xiong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.RO
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25236v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，并优先使用了 field_evidence_map 对应的片段进行各字段信息提取。
