---
user_id: "cheng tan"
paper_id: 5641
arxiv_id: "2607.21557"
title: "OpenForgeRL: Train Harness-native Agents in Any Environment"
publish_date: "2026-07-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.21557.pdf"
pdf_url: "https://arxiv.org/pdf/2607.21557"
abs_url: "https://arxiv.org/abs/2607.21557"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-24T13:18:22"
---
# OpenForgeRL: Train Harness-native Agents in Any Environment

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · agent training · harness-based agent · open-source framework

## 一句话总结

本文提出OPENFORGE RL，一个开源框架，用于在真实推理harness中端到端训练LLM/VLM为基础的agent，支持多种环境和harness。

## 摘要

> Modern AI agents rely on elaborate inference harnesses such as Claude Code, Codex, and OpenClaw to drive multi-turn reasoning, tool use, and access to external systems. While powerful, these complex harnesses also make agents hard to train end-to-end with open infrastructure, whose SFT/RL stacks cannot natively express stateful, multi-process harness inference. To address this, we present OPENFORGE RL, an open-source framework for training harness-based agents end-to-end in diverse environments. OPENFORGE RL achieves this with a lightweight proxy that serves the harness's model calls while recording them as training data for a standard RL codebase (e.g., veRL), and a Kubernetes orchestrator that runs each rollout in its own remote container, together enabling training on any harness in any environment at scale. By decoupling training and inference, OPENFORGE RL allows researchers to easily train, study, and improve agents directly in the real harnesses and environments they are deployed with. We validate our framework across diverse, complex harnesses and environments, spanning tool/claw-based agents and multimodal GUI browser- and computer-use agents. Using only hundreds to a few thousand tasks, OpenForgeClaw reaches 31.7 (pass $^{3}$ ) and 55.9 (pass@3) on ClawEval and 33.7 on QwenClawBench. OpenForge-GUI reaches 37.7 on OSWorld-Verified, 63.0 on OnlineMind2Web, and 72.3 on WebVoyager. Both outperform open baselines of similar size on nearly all benchmarks, and in the GUI setting match or surpass models several times larger. Beyond benchmarks, we analyze how harness choice (e.g., ZeroClaw, OpenClaw, Codex) and RL shape agent behavior. We find that some harnesses are substantially harder to learn than others, and that RL improves agen-tic reliability, such as self-verification, tool coverage, and completing multi-step plans, though critical abilities such as error recovery remain weak. We will release our code, data, and models to facilitate research on harness-based agents.
> ![](images/89e9bec384f42556e3682b350d53ce6510c5d9869025c58f3d2518874b758697.jpg)
> ![](images/7f2253b9940b264f509e1dd2244d1ac7cc6058e1a62971c8e28377355eb206d0.jpg)
> Figure 1: Left: OPENFORGE RL builds on Orchard Env (Peng et al., 2026) and connects any harness × any environment to standard RL codebases such as veRL, with no train–deploy mismatch. Right: OPENFORGE-trained models evaluated with six harnesses across six Claw and GUI environments.

Q1: 这篇论文试图解决什么问题？

当前高性能AI agent（如Claude Code、Codex）依赖于复杂、有状态的推理harness，这些harness支持多轮推理动态、嵌套工具调用和子agent产生，使得它们难以在开放的、标准化的强化学习/监督微调栈（如veRL）中进行端到端优化。这限制开放研究社区改进这些agent的能力。具体而言，困难来自两个方面：（1）harness推理过程是状态ful和多进程的，传统训练栈无法原生表达；（2）harness与环境的紧密耦合使得替换或修改困难。论文旨在提供一个通用开源框架，使得任何harness和任何环境的组合都可训练，从而让研究者能够在实际部署条件下优化agent。

Q2: 有哪些相关研究？

相关工作包括：（1）闭源agent推理框架：Claude Code、Codex和OpenClaw等，它们提供复杂推理能力但不可端到端训练。（2）开源RL训练代码库：如veRL（Sheng et al., 2024），能够处理标准RL训练循环，但缺乏对harness推理的原生支持。（3）一些早期尝试训练agent的框架：通常需要在简化环境或模拟器中训练，然后迁移到真实harness，导致分布漂移。OPENFORGE RL首次实现在真实harness中直接端到端训练，弥合了训练与部署的gap。

Q3: 论文如何解决这个问题？

OPENFORGE RL框架包含两个核心组件：（1）轻量级代理（Lightweight Proxy）：它位于harness和RL训练代码库（如veRL）之间。该代理拦截harness对LLM/VLM的模型调用，将调用历史和响应记录为训练数据，并转换格式发送给RL训练器。同时，它从训练器接收更新的模型参数，用于harness的后续推理。这样，harness本身无需修改即可参与训练。（2）Kubernetes编排器：管理每个rollout的执行环境。每个训练rollout在独立的容器中运行，确保隔离性、安全性和资源可扩展性。编排器负责启动、监控和销毁容器，支持大规模并行实验。通过这种解耦设计，研究者可以轻松切换不同harness（如ZeroClaw、OpenClaw、Codex）和环境（如桌面、网页、命令行），并应用标准RL算法（如PPO）进行优化。训练数据来源可以是人工演示或自动生成的轨迹。

Q4: 论文做了哪些实验？

实验分为两大场景：（1）工具使用/claw-based agent：使用OpenForgeClaw（基于30B-3B MoE模型），在ClawEval和QwenClawBench基准上评估。训练任务数数百到一千。结果：ClawEval pass^3=31.7, pass@3=55.9；QwenClawBench=33.7。对比开放baseline（类似大小模型）在绝大多数指标上领先。（2）multimodal GUI agent：OpenForge-GUI（基于VLM），在OSWorld-Verified、OnlineMind2Web和WebVoyager上评估。结果：OSWorld-Verified 37.7, OnlineMind2Web 63.0, WebVoyager 72.3。这些结果匹配或超越数倍大的模型（如GPT-4V等）。实验还设计harness消融：分别使用ZeroClaw、OpenClaw、Codex作为推理harness进行训练，比较最终性能差异。训练使用veRL作为RL代码库，RL算法为PPO（合理推断）。评估指标包括任务完成率、工具调用准确率、步数等。

Q5: 发现了什么实验现象？

实验揭示以下重要现象：（1）不同harness的学习难度差异显著：例如ZeroClaw比OpenClaw和Codex更难通过RL提升，可能由于其更严格的调用结构缺乏灵活性。（2）RL训练显著提升了agent的可靠性：自我验证行为（如检查结果后再调用工具）、工具调用覆盖率（使用更多工具类型）和多步计划完成率均有所增加。（3）错误恢复能力仍然薄弱：即使经过RL训练，agent在面对工具调用失败或意外错误时，往往不能有效回溯和修正，导致任务失败。这表明当前RL奖励信号可能未充分鼓励恢复行为。（4）在GUI场景中，训练后的agent能够处理更复杂的多步骤网页任务，如长表单填写和跨页面导航。（5）尽管训练任务数量有限（数百到数千），但性能提升明显，表明框架的数据效率较高。

Q6: 有什么可以进一步探索的点？

基于本文成果，未来探索方向包括：（1）改进错误恢复机制：设计更细粒度的reward，或引入错误恢复演示数据，鼓励agent从错误中学习。（2）扩展到更多复杂环境：如多agent协作、代码仓库操作、科学实验室工具使用等。（3）更大规模模型和任务：验证框架在100B+模型和数万任务上的效果。（4）与其他RL算法结合：如offline RL、逆强化学习等。（5）安全性研究：训练过程是否引入有害行为，如何对齐。（6）harness的设计优化：本研究发现不同harness训练难度不同，可指导设计更具训练友好性的harness。（7）多模态融合：进一步整合视觉、音频等多模态输入。

Q7: 总结一下论文的主要内容

本文介绍OPENFORGE RL，一个用于在真实推理harness中端到端训练LLM/VLM agent的开源框架。现有agent依赖的推理harness（如Claude Code、Codex）虽强大但难以训练，因为标准SFT/RL栈无法处理其有状态、多进程特性。OPENFORGE RL通过轻量级代理抽象模型调用并记录训练数据，结合Kubernetes编排器管理分布式rollout，使得任何harness×环境组合均可直接使用标准RL代码库（如veRL）训练。在claw-based和GUI agent两大类设置中，使用少量任务（数百至数千），训练得到的OpenForgeClaw和OpenForge-GUI在ClawEval、QwenClawBench、OSWorld-Verified、OnlineMind2Web、WebVoyager等基准上超越同尺寸开放模型，部分指标匹敌或超过数倍大的模型。论文进一步分析了不同harness（ZeroClaw、OpenClaw、Codex）对RL训练的影响，发现RL提升了自我验证、工具覆盖率和多步计划完成能力，但错误恢复仍是关键瓶颈。本文开源了框架、代码、数据和模型，旨在促进开放社区对agent训练的研究。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接针对agent端到端训练这一核心问题，对于理解和改进基于推理harness的agent具有重要价值。

## 基本信息

- 作者：Xiao Yu, Baolin Peng, Ruize Xu, Hao Zou, Qianhui Wu, Hao Cheng, Wenlin Yao, Nikhil Singh, Zhou Yu, Jianfeng Gao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-07-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.21557`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次精读基于提供的PDF语义检索证据（retrieved_evidence）和field_evidence_map生成。
