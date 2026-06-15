# Reward Models & Reinforcement Learning

<!-- paperflow-topic-summary:start -->
## PaperFlow Summary
- 概念：Reward Models & Reinforcement Learning
- 方法：reasoning, optimization
- 论文/报告：1 篇
- AdaSR: Adaptive Streaming Reasoning with Hierarchical Relative Policy Optimization
- 画像/前沿：该主题来自当前精读论文与研究画像的交集，供 Wiki 可视化和后续检索使用。
<!-- paperflow-topic-summary:end -->

<!-- paperflow:3a0a72fc30855e03 -->
## AdaSR: Adaptive Streaming Reasoning with Hierarchical Relative Policy Optimization

[[Deep Reading - Jun 2026/AdaSR-Adaptive Streaming Reasoning with Hierarchical Relative Policy Optimization|Deep Reading]]

[https://arxiv.org/pdf/2606.14694v1](https://arxiv.org/pdf/2606.14694v1)

- **这篇论文介绍了 AdaSR，一种旨在解决动态流式环境下大语言模型推理效率问题的框架。传统的“先读后想”模式在处理实时信息流时存在高延迟和灵活性差的问题。AdaSR 允许模型在接收输入流的同时进行思考，并通过新颖的 HRPO 算法优化推理时机和计算量分配。HRPO 通过分层强化学习解决了流式推理中策略偏移和奖励分配的难题。实验结果证明，AdaSR 能够根据输入信息的节奏自适应地调整推理策略，在 GSM-symbolic 等任务上实现了准确率与延迟的更优权衡。该工作为构建能够实时响应、边听边想的智能体提供了重要的技术支撑。**
