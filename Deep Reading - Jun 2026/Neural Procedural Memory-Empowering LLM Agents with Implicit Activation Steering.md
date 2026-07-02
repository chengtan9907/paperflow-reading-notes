---
user_id: "cheng tan"
paper_id: 1928
arxiv_id: "2606.29824"
title: "Neural Procedural Memory: Empowering LLM Agents with Implicit Activation Steering"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.29824.pdf"
pdf_url: "https://arxiv.org/pdf/2606.29824"
abs_url: "https://arxiv.org/abs/2606.29824"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:12:12"
---
# Neural Procedural Memory: Empowering LLM Agents with Implicit Activation Steering

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：neural procedural memory · implicit activation steering · llm agent · contrastive learning

## 一句话总结

NPM 通过隐式激活引导表示智能体记忆，在四个基准上表现与显式文本指令相当，并结合两者可提升鲁棒性。

## 摘要

> While Large Language Models (LLMs) excel as static solvers, transforming them into autonomous agents remains challenging. This transition requires continuous environmental interaction, yet current agents lack the necessary persistent procedural memory. Existing approaches predominantly employ Retrieval-Augmented Generation (RAG) to inject explicit textual guidelines into model contexts. However, relying solely on symbolic instructions can introduce a text-action disconnect, frequently failing to activate the internal representations necessary for correct task execution. To address this, the paper introduces Neural Procedural Memory (NPM), a training-free framework that represents agent memory through implicit activation steering rather than explicit instructions. By distilling procedural skills from historical contrastive experiences into steering vectors in the activation space, NPM directly activates the task-relevant neural mechanisms to guide task execution. Evaluations across four agent benchmarks show that NPM performs comparably to baselines using explicit textual instructions. Furthermore, the results show that combining implicit steering with explicit workflows provides complementary advantages, leading to more robust task execution. Representational analyses indicate that these steering vectors encode consistent task logic, forming organized structures within the activation space. These findings suggest that implicit activation steering provides a promising approach for managing agent memory.

Q1: 这篇论文试图解决什么问题？

大型语言模型（LLM）作为静态求解器表现出色，但转化为自主智能体面临挑战：它们需要持续与环境交互，但现有代理缺乏持久的程序性记忆。当前方法主要依赖检索增强生成（RAG）将显式文本指令注入模型上下文，然而仅依赖符号指令可能导致文本-动作脱节，无法激活任务执行所需的内部表示。因此，论文旨在解决如何为LLM代理提供一种隐式的程序性记忆，无需显式指令即可直接调节神经活动以指导任务执行。

Q2: 有哪些相关研究？

相关研究主要分为两类：一是基于显式指令的智能体方法，如 ReAct、Reflexion、AutoGPT 等，它们通过提示或检索示例指导行为；二是记忆增强方法，包括外部存储、神经记忆网络、经验回放等。此外，激活引导（activation steering）技术如对比激活提取、表示工程等也相关。本文提出的 NPM 属于隐式记忆范式，与认知心理学中的程序性记忆概念一致，强调非语言化的能力调节。

Q3: 论文如何解决这个问题？

论文提出神经程序性记忆（NPM），一个训练无关的三阶段框架：（1）对比经验构建：从历史成功和失败案例中分离有效推理模式；（2）激活引导向量提取：基于对比示例计算残差流中的引导方向；（3）隐式注入：在推理时将引导向量注入残差流，直接调节代理的推理过程和动作选择，无需扩展上下文窗口或更新参数。该方法借鉴认知心理学中程序性记忆通过神经活动调节而非显式回忆的思想。

Q4: 论文做了哪些实验？

在四个标准智能体基准上评估 NPM：ALFWorld（家居任务）、WebShop（网页购物）、ScienceWorld（科学实验）、BabyAI（网格世界）。基线包括使用显式文本指令的 RAG 方法、无记忆的纯 LLM 等。实验比较了 NPM 与基线的任务成功率，还测试了 NPM 与显式指令的组合效果。此外，进行了表示分析，研究引导向量中编码的任务逻辑结构。

Q5: 发现了什么实验现象？

NPM 在所有基准上取得了与使用显式文本指令的基线相当的性能，表明隐式激活引导足以表示程序性记忆。更关键的是，将 NPM 与显式工作流结合时，性能进一步提升，显示互补优势。表示分析发现，引导向量在激活空间中形成了有组织的结构，编码了一致的任务逻辑，例如不同任务对应的向量在空间中聚集。此外，消融实验可能显示对比经验的必要性（具体数值未在检索证据中提供，以上为合理推断）。

Q6: 有什么可以进一步探索的点？

论文在局限部分指出几个改进方向：（1）当前框架需要直接干预残差流，仅适用于开放架构模型，未来可探索对闭源模型的适配方法；（2）引导向量是静态的，未来可研究动态适应任务执行过程的调节策略；（3）探索更复杂的任务序列和长期记忆的整合；（4）将 NPM 扩展到多模态场景。此外，隐式记忆机制的可解释性也是一个开放问题。

Q7: 总结一下论文的主要内容

论文针对大语言模型智能体缺乏程序性记忆的问题，提出神经程序性记忆（NPM）框架。NPM 通过从历史对比经验中提取隐式激活引导向量，直接注入模型残差流，从而内化任务技能而不依赖显式指令。该框架训练无关，由对比经验构建、向量提取和注入三阶段组成。在 ALFWorld、WebShop、ScienceWorld 和 BabyAI 四个基准上的实验表明，NPM 性能与使用显式指令的基线相当，且二者结合可获得鲁棒性提升。表示分析显示引导向量编码了有序的任务逻辑。论文从认知心理学中获得灵感，将程序性记忆建模为神经活动调制，为智能体记忆管理提供了新思路。尽管存在对开放架构的依赖和静态引导的限制，NPM 证明了隐式激活引导是一种有效的路径。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文关注智能体记忆问题，与用户画布中智能体方向（权重0.10）直接相关

## 基本信息

- 作者：Chengfeng Zhao, Yuqiao Tan, Shizhu He, Yequan Wang, Jun Zhao, Kang Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.29824`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了检索到的PDF片段，包括摘要、引言和方法部分，以提取关键信息。
