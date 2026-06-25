---
user_id: "cheng tan"
paper_id: 1239
arxiv_id: "2606.23049"
title: "Training Open Models for Agentic Phone Use"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23049.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23049"
abs_url: "https://arxiv.org/abs/2606.23049"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:16:56"
---
# Training Open Models for Agentic Phone Use

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：phone use agent · reinforcement learning · simulated environment · supervised fine-tuning

## 一句话总结

PhoneBuddy提出了一种结合真实应用环境与模拟应用环境PhoneWorld的训练方案，通过共享监督微调后再用强化学习在两种环境上训练，显著提升了开放模型在真实手机上的任务成功率。

## 摘要

> Phones are becoming an important execution surface for general-purpose agents, but training open models for reliable phone use remains difficult because the environment that matters at deployment, real devices running real apps, is slow, stateful, side-effectful, and hard to reset or verify, while scalable mock environments only approximate real behavior. We present PhoneBuddy, a training recipe and open-model line for agentic phone use that combines a real-app environment with a mock-app environment, PhoneWorld, which reconstructs runnable mock apps from real GUI usage structure. PhoneBuddy first builds a shared supervised fine-tuning stage from trajectories collected in both environments, then compares real-app RL against mixed RL across both environments. Across a 150-task human evaluation on real phones spanning apps, mini-apps, and cross-app workflows, task success rate improves from 36.67\% after supervised fine-tuning to 40.67\% after real-app RL and 45.33\% after mixed RL. On AndroidWorld, the same progression rises from 60.3\% to 77.2\% to 83.2\%. These results show that mock-app training is not a replacement for real-app RL, but a complementary source of scalable, resettable, and automatically checked interaction. The gains are strongest on app and mini-app tasks, while long-horizontal cross-app workflows remain an important open challenge.

Q1: 这篇论文试图解决什么问题？

本文试图解决的核心问题是：如何训练开放模型使其在真实手机上能够可靠地完成用户任务，而不仅仅是提升局部动作模仿或基准测试中的交互能力。现有方法面临两难：真实应用环境提供了高保真行为，但收集轨迹、重置状态和验证结果非常昂贵；模拟或重建环境虽然可扩展，但只能近似真实行为，导致训练信号与部署环境之间存在差距。论文的核心矛盾在于现实主义与可扩展性之间的取舍。

Q2: 有哪些相关研究？

相关工作涵盖三大类：1）大型语言模型作为智能体通过软件界面执行任务（如WebAgent、桌面操作系统智能体、工具使用智能体、手机智能体）；2）模仿学习与强化学习在视觉语言行动任务中的应用，特别是离线学习与在线学习的结合；3）模拟环境构建，例如从GUI痕迹重建可运行应用用于训练。PhoneBuddy在方法上区分于纯粹的真实环境训练或纯粹模拟训练，提出同时利用两种互补环境。

Q3: 论文如何解决这个问题？

PhoneBuddy的训练方案包含三个核心阶段：1）共享监督微调（SFT）阶段：使用在真实环境和模拟环境PhoneWorld中收集的轨迹对基础模型进行监督微调，为后续强化学习提供初始化；2）真实环境强化学习（Real-app RL）阶段：在真实手机应用环境中使用RL进一步优化模型，使其适应真实设备行为和副作用；3）混合强化学习（Mixed RL）阶段：同时使用真实环境和PhoneWorld模拟环境进行RL训练，结合高保真信号与可扩展信号。PhoneWorld（Tang et al., 2026b）是基于真实GUI使用结构重建的可运行Android应用模拟器，具有可改变的状态和自动检查规则，支持大规模并行训练。

Q4: 论文做了哪些实验？

进行了两组主要实验：1）真实手机人类评估：包含150个任务，覆盖应用（单应用任务）、小应用（mini-app，单应用内部子任务）和跨应用工作流（跨应用多步骤任务）。评估指标为任务成功率。比较了SFT基线、SFT+真实环境RL、SFT+混合RL三种模型。2）AndroidWorld基准测试：一个标准化的手机智能体评估环境，同样比较上述三种模型。所有实验使用相同的基模型（未指定具体模型，推测为开源视觉-语言模型）。

Q5: 发现了什么实验现象？

关键实验现象：1）真实环境RL在人类评估中带来4个百分点的提升（从36.67%到40.67%），在AndroidWorld上提升16.9个百分点（从60.3%到77.2%），表明在线优化对真实设备行为适应有效。2）混合RL进一步带来显著增益：人类评估中再提升4.66个百分点（至45.33%），AndroidWorld上再提升6个百分点（至83.2%），表明模拟环境提供了无法仅从真实环境获得的补充信号。3）模拟环境训练的增益主要集中在应用和小应用任务上，跨应用工作流任务的长水平特性仍构成挑战，成功率提升有限（推测，需查看全文细节）。4）模拟环境本身不是真实环境的替代，而是互补来源。

Q6: 有什么可以进一步探索的点？

1）解决长水平跨应用工作流：目前混合RL主要提升单应用任务，需要研究如何利用模拟环境支持多步骤跨应用任务。2）运行时编排：训练问题之外，如何在实际部署中实现安全、隐私保护、错误恢复等。3）隐私与安全：手机使用涉及敏感数据，如何确保模型不泄露或误用信息。4）更复杂的模拟环境：提升PhoneWorld对真实世界多样性和动态变化的覆盖。5）扩展到更多应用类型和现实场景。6）探索更高效的RL算法，如离线RL与在线RL的结合。

Q7: 总结一下论文的主要内容

本文提出了PhoneBuddy，一套针对开放模型进行agentic phone use的训练方案和模型系列。核心贡献在于创新性地结合了真实应用环境和模拟应用环境PhoneWorld（基于真实GUI使用结构构建的可运行模拟应用）。训练流程分为两个阶段：首先在两种环境中收集轨迹进行共享监督微调（SFT），然后进行强化学习（RL）优化。通过对比SFT、SFT+真实环境RL、SFT+混合RL，论文系统展示了模拟环境训练并非真实环境RL的替代品，而是互补的、可扩展的信号来源。实验在150个真实手机人类评估任务和AndroidWorld基准上进行，结果显示混合RL显著优于仅真实环境RL和仅SFT，且增益主要来自于应用和小应用任务。跨应用工作流仍是开放挑战。论文还讨论了训练问题与运行时编排、隐私、安全等部署问题的分离，为后续工作提供了清晰边界。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本文聚焦agentic phone use，属于智能体研究方向（用户兴趣权重0.10），具有直接相关性。

## 基本信息

- 作者：Zhengyang Tang, Xin Lai, Pengyuan Lyu, Xinyuan Wang, Tianyi Bai, Chenxin Li, Yiduo Guo, Huawen Shen, Yuxuan Liu, Junyi Li, Zhengyao Fang, Yang Ding, Yi Zhang, Weinong Wang, Xingran Zhou, Liang Wu, Fei Tang, Sunqi Fan, Shangpin Peng, Zheng Ruan, Anran Zhang, Benyou Wang, Ji-Rong Wen, Rui Yan, Chengquan Zhang, Han Hu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23049`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 已参考PDF检索证据，结合heuristic_draft与retrieved_evidence片段进行补充和修正。
