---
user_id: "cheng tan"
paper_id: 437
arxiv_id: "2606.16802v1"
title: "LabOSBench: Benchmarking Computer Use Agents for Scientific Instrument Control"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16802v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16802v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:08:16"
---
# LabOSBench: Benchmarking Computer Use Agents for Scientific Instrument Control

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：benchmark · computer-use agent · multimodal gui agent · scientific instrument

## 一句话总结

LabOSBench 是一个基于 Web 的科学仪器模拟器套件，用于评估多模态 GUI 智能体在科学仪器控制任务上的表现，覆盖从样品加载到数据采集的完整工作流。

## 摘要

> 当前计算机使用基准主要评估虚拟化系统中的软件操作任务，而科学仪器场景需要协调复杂界面和反馈驱动的参数调整。直接在物理高精度仪器上评估智能体成本高、风险大、可重复性差。为此，我们提出 LabOSBench，一个基于 Web 的科学仪器模拟器基准，通过浏览器直接操作，避免资源密集的 OS 虚拟化，支持灵活的任务配置和执行评估。LabOSBench 包含 8 个仪器模拟器、96 个子任务，覆盖样品加载、对准、参数调优、数据采集和结果检查工作流。我们评估了通用视觉语言模型、专用 GUI 智能体模型和高级智能体框架。实验表明，现有智能体能完成许多结构化的 GUI 子任务，但在反馈驱动操作和长程工作流执行上存在困难。LabOSBench 为推进计算机使用智能体在科学仪器控制中的应用提供了可重复、低成本的测试平台。

Q1: 这篇论文试图解决什么问题？

现有计算机使用基准（如 WebArena、OSWorld）主要针对网页、桌面和移动软件操作，采用虚拟化环境。科学仪器控制（如扫描电子显微镜、原子力显微镜）涉及复杂界面、连续参数调节、视觉反馈驱动的闭环调整以及长程工作流。直接在实际仪器上评估代理高风险、高成本、难以复现。因此，需要一种可扩展、安全、可重复的模拟测试床，既保留科学仪器的操作挑战，又支持自动化评估。

Q2: 有哪些相关研究？

计算机使用智能体从基于 DOM 的网页代理发展到基于截图视觉的代理（Deng et al., Zhou et al., He et al.）。屏幕解析方法（如 ScreenParsing）提升了对复杂视觉中可交互元素的识别。科学智能体系统研究包括实验规划、化学工具使用、机器学习实验和自主发现（Tom et al., Szymanski et al., Boiko et al.）。MyScope 提供基于浏览器的显微镜模拟器用于人类教育，但不支持智能体执行式评估。计算机使用基准如 WebArena、OSWorld、Mobile-Env 等评估代理在软件环境中的表现，但缺乏科学仪器特有的操作挑战。LabOSBench 填补了这一空白，通过模拟器提供可执行评估。

Q3: 论文如何解决这个问题？

LabOSBench 构建了一套基于 Web 的科学仪器模拟器，模拟器在浏览器中运行，避免了完整的 OS 虚拟化。基准包含 8 个仪器模拟器：扫描电子显微镜（SEM）、原子力显微镜（AFM）、能谱仪（EDS）、X 射线衍射仪（XRD）、光致发光光谱仪（PL）、激光共聚焦显微镜（LFM）、原子探针断层扫描（APT）和纳米压痕仪（NI）。总共定义 96 个子任务，分为五大类：样品处理（sample handling）、对准（alignment）、配置（configuration）、执行（execution）和完成（completion）。每个子任务有自然语言描述和一个成功检查器（基于 DOM 状态或图像指标）。代理接收屏幕截图和指令，输出 GUI 动作（点击、输入、滚动等）。使用统一的交互协议，最多 50 步，每个模型-子任务对运行 2 次，报告子任务归一化成功率。还支持端到端工作流评估。

Q4: 论文做了哪些实验？

实验评估了三组基线：通用多模态模型（Qwen3VL-32B、EvoCUA-8B、Claude Sonnet-4.5、Kimi-K2.5、Seed-1.6、GPT-5.5、Claude Opus-4.5）；专用 GUI 模型（UI-TARS-1.5-7B、GUI-Owl-7B）；智能体框架（GTA1 w/ GPT-5.5、VLAA-GUI w/ Opus-4.5、Hippo Agent w/ Opus-4.5）。每个任务最多 50 个交互步，每个子任务运行 2 次。评估指标为子任务成功率（按仪器和任务类别汇总）。额外进行了 SEM 聚焦调整任务的图像质量分析（PSNR），以及端到端工作流评估（GPT-5.5，10 次运行）。

Q5: 发现了什么实验现象？

在 96 个子任务上，Seed-1.6 在样品处理（97.1%）、对准（59.1%）、执行（82.1%）和总体（81.2%）上领先，Kimi-K2.5 在配置任务上最佳（53.7%）。GTA1 w/ GPT-5.5 在反馈驱动的对准和配置任务上表现突出（对准 63.6%，配置 87.0%）。所有模型在样品处理和完成任务上成功率较高，但在对准和配置任务上显著下降，这些任务需要反馈驱动的调整。SEM 聚焦调整任务分析：GPT-5.5 达到最高聚焦成功率和最佳 PSNR，表明它能保持中间科学状态质量。端到端评估：GPT-5.5 在 EDS（70.0%）、APT（80.0%）、XRD（60.0%）上非零成功率，但在 SEM、SPM 等工作流上失败，表明误差积累和长程规划仍具挑战。反直觉发现：即使是先进模型（如 Claude Opus-4.5）在需要精细操作和视觉理解的任务上也不如预期。

Q6: 有什么可以进一步探索的点？

1. 改进科学状态理解：代理需要更好地解释显微镜图像、光谱等中间结果。2. 更精确的空间定位：高精度点击和多分辨率区域选择。3. 闭环反馈控制：在聚焦、对准等任务中迭代调整。4. 长程工作流规划：减少误差累积，提高端到端成功率。5. 扩展到更多仪器和真实仪器验证。6. 开发针对科学 GUI 的专用适应方法（如微调模型或提示策略）。7. 结合实验规划与自动化数据采集。8. 考虑模拟器与现实之间的 sim-to-real 差距。

Q7: 总结一下论文的主要内容

论文提出了 LabOSBench，一个基于 Web 的科学仪器模拟器基准，用于评估多模态 GUI 代理在科学仪器控制任务上的能力。基准包含 8 种常用仪器的模拟器，涵盖 96 个子任务，分为样品处理、对准、配置、执行和完成五大类别。任务设计保留了科学仪器操作的复杂性：密集界面、程序依赖、连续参数调节和视觉反馈闭环。评估了包括 GPT-5.5、Claude Opus-4.5、Seed-1.6、GTA1 等 11 个基线模型/框架。关键发现：现有代理在结构化、离散的 GUI 子任务上表现较好（如样品处理和配置中的简单步骤），但在需要科学状态理解、视觉反馈驱动调整和长程工作流的任务上显著失败。例如，SEM 聚焦调整需要迭代优化图像清晰度，只有少数模型能有效完成。端到端评估显示代理在简单工作流上可达 60-80% 成功率，但在复杂工作流上几乎完全失败。论文揭示了科学仪器控制作为计算机使用代理评估中一个具有挑战性且尚未充分探索的场景，并提供了可复现的评估平台。主要贡献：1）首个面向科学仪器 GUI 控制的可执行基准；2）覆盖 8 种仪器的任务套件；3）系统评估揭示当前代理的局限。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：LabOSBench 直接面向计算机使用代理（CUA）和 AI for Science 两个交叉领域，适合评估和推动科学仪器控制智能体。

## 基本信息

- 作者：Anqi Zou, Han Deng, Chengyu Zhang, Junquan Hu, Yu Wang, Yuxiang Xing, Aokai Zhang, Hanling Zhang, Zhaoyang Liu, Ben Fei, Zhihui Wang, Wanli Ouyang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16802v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了提供的 PDF 语义检索证据和字段证据映射，结合 heuristic_draft 进行润色，确保各字段信息锚定对应证据来源。
