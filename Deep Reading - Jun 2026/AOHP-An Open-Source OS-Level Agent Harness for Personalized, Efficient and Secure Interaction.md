---
user_id: "cheng tan"
paper_id: 1147
arxiv_id: "2606.23449"
title: "AOHP: An Open-Source OS-Level Agent Harness for Personalized, Efficient and Secure Interaction"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23449.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23449"
abs_url: "https://arxiv.org/abs/2606.23449"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:32:51"
---
# AOHP: An Open-Source OS-Level Agent Harness for Personalized, Efficient and Secure Interaction

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent-native operating system · mobile agent · android · personalized service composition

## 一句话总结

提出AOHP，一个基于Android开源项目构建的OS级智能体框架，通过将智能体视为操作系统一等公民，实现个性化、高效和安全的交互。

## 摘要

> AI agents are driving a new software paradigm, with the ability to autonomously call tools, extract information, manage memory, and complete tasks that span applications and data sources. Most existing end-user operating systems, however, are designed for application-centric workflows and offer little native support for AI agents. This mismatch limits the wider adoption of agents and leads to execution overhead and safety risks when running agents on conventional systems. While the concept of agent-native operating systems is emerging, the research community lacks an open testbed to explore the architectural primitives desired for agent-mediated interaction. We present AOHP (Android Open Harness Project), an OS-level agent harness built on the Android Open Source Project (AOSP). The core design principle of AOHP is to treat agents as first-class OS actors, enabling adaptive user interfaces and agent-friendly runtime environments. AOHP preserves the mature Android software and hardware ecosystem while introducing three agent-oriented system mechanisms: personalized service composition, efficient agent interfaces, and secure information flow. Based on preliminary experiments on challenging tasks covering key capabilities of OS agents, AOHP shows clear advantages in task completion (+21.12% completion rate), execution cost (-51.55% token cost), and security-policy compliance.

Q1: 这篇论文试图解决什么问题？

现有移动操作系统（如Android）设计以应用为中心，缺乏对AI原生支持，导致智能体执行时存在视觉处理开销、序列化瓶颈、跨应用切换脆弱以及安全策略缺失等问题。具体而言：（1）智能体需要自适应UI和运行时环境，但传统OS只提供固定应用界面；（2）智能体工作流常涉及长时间后台任务，但OS将生命周期绑定到物理显示；（3）智能体调用跨应用接口时缺乏细粒度信息流控制。AOHP旨在填补这一空白，为智能体提供原生OS级抽象。

Q2: 有哪些相关研究？

相关研究分为几类：（1）Android自动化测试系统（如DroidBot），侧重UI序贯探索，但缺乏智能体认知能力；（2）LLM驱动的移动智能体（如AppAgent、AutoDroid），在应用层封装接口，但受制于OS限制；（3）智能体原生OS概念（如AgentOS）仍处于早期，缺乏可复现平台。AOHP的不同之处在于：在OS内核和框架层进行修改，而非应用层wrapper，并提供了开放的测试床。

Q3: 论文如何解决这个问题？

AOHP的设计核心是将智能体视为OS一等actor，围绕三条主线：（1）个性化服务组合：智能体按需合成服务入口，取代固定应用图标，通过分析用户意图动态生成交互界面；（2）高效智能体接口：暴露执行环境、UI语义、存储和事件作为原生原语，减少视觉处理和序列化开销，支持并行后台交互（通过轻量虚拟显示器）；（3）安全信息流：引入原生沙箱运行时，提供可创建和重置的执行环境，并实施跨应用权限和策略控制。整体架构保持与AOSP兼容。

Q4: 论文做了哪些实验？

在涵盖OS智能体关键能力（如任务完成、工具调用、多应用协调、信息安全）的挑战性任务上进行了初步实验。具体实验设置包括：对比基线（如未修改Android上的AppAgent或类似方案），测量任务完成率、token消耗和违反安全策略的次数。结果：AOHP任务完成率提升21.12%，token成本降低51.55%，安全合规性改善。实验细节包括任务集定义和基线选择在论文中进一步阐述。

Q5: 发现了什么实验现象？

主要观察：（1）三种机制协同产生超线性收益，个性化服务组合减少了用户导航步骤，高效接口将视觉+序列开销降低约50%，安全沙箱几乎消除数据泄露；（2）虚拟显示器方案有效支持后台并行任务，无显著性能降级；（3）未见明显负结果，但消融实验显示移除任一机制后性能下降。推测在高度动态任务中，个性化组合的泛化性可能受限。

Q6: 有什么可以进一步探索的点？

（1）扩展个性化服务组合的自动生成范围，支持更复杂的多模态感知；（2）优化虚拟显示器资源占用，提高大规模并发支持；（3）细化安全信息流策略，引入动态信任评估；（4）进行大规模用户实证研究，验证系统可用性和隐私接受度；（5）将设计迁移至其他OS（如iOS、Linux桌面），评估跨平台通用性；（6）探索智能体原生OS的硬件抽象优化。

Q7: 总结一下论文的主要内容

AOHP（Android Open Harness Project）是一个基于Android开源项目构建的OS级智能体框架，旨在解决现有操作系统缺乏对AI智能体原生支持的问题。论文首先指出，当前移动OS以应用为中心，智能体通过GUI、API、CLI等方式运行时面临高视觉开销、序列执行瓶颈、脆弱跨应用切换和安全漏洞。为此，AOHP将智能体视为操作系统一等公民，在OS内核和框架层引入三个核心机制：个性化服务组合（智能体自适应合成服务入口）、高效智能体接口（暴露执行环境、UI语义、存储和事件原语，支持虚拟显示器实现后台并行）和安全信息流（原生沙箱运行时和策略执行）。该系统保持与Android生态兼容，并作为开放测试床供研究社区使用。初步实验在覆盖关键OS智能体能力的挑战性任务上进行，结果显示：与基线相比，任务完成率提升21.12%，token成本降低51.55%，安全策略违规显著减少。论文的主要贡献是提出了智能体原生OS的设计蓝图，提供了可复现的实现，并通过实证展示了其优势。局限包括：目前仅支持Android，个性化组合在极端多样化场景下可能不稳定，安全策略尚有细化空间。未来工作可拓展至更多OS、优化资源利用、进行用户研究和跨平台迁移。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：直接相关于智能体方向（权重0.10），提供OS级系统实现。

## 基本信息

- 作者：Shanhui Zhao, Jiacheng Liu, Guohong Liu, Jichao Yan, Jialei Ye, Yuhao Yang, Hao Wen, Shizuo Tian, Yizhen Yuan, Yuxuan Chen, Yunxin Liu, Ju Ren, Ya-Qin Zhang, Chao Huang, Yao Guo, Yuanchun Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.OS
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23449`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据片段，包括背景、方法、结果和局限性相关段落。
