---
user_id: "cheng tan"
paper_id: 2109
arxiv_id: "2606.31650v1"
title: "ECHO: Prune to act, trace to learn with selective turn memory in agentic RL"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.31650v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.31650v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T01:24:35"
---
# ECHO: Prune to act, trace to learn with selective turn memory in agentic RL

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：language agents · reinforcement learning · context management · selective memory

## 一句话总结

ECHO是一个选择性轮次记忆框架，通过源索引重构同时解决长程智能体中的历史坍塌和可追溯学习问题。

## 摘要

> Long-horizon language agents must repeatedly interact with tools, accumulate evidence, and make decisions under bounded context windows. Existing context-management methods make such rollouts feasible by truncating distant history, folding past turns into summaries, or selecting compact memory states. However, these breakthroughs introduce two coupled limitations. First, as the number of turns grows, historical observations are progressively removed or collapsed into compressed states, making it harder for the policy to reuse fine-grained evidence. Second, once the original turns are no longer source-addressable, outcome-based RL loses an explicit path for aligning policy updates with the evidence that supported a successful final answer. To this end, we propose ECHO, a selective turn-memory framework that jointly addresses history collapse and traceable learning through source-indexed reconstruction. Specifically, ECHO compresses each completed environment turn into a compact memory record, reconstructs bounded policy contexts by selecting from these records, and reuses the selected source indices to route positive outcome credit to the evidence and selection actions that support successful answers. On BrowseComp-Plus, ECHO reaches 43.4% held-out accuracy, outperforming GRPO (28.9%) and the rolling-summary baseline SUPO (36.1%), while using fewer turns and lower trajectory volume than SUPO (Figure 1). Additionally, the trained policy improves zero-shot generalization across multi-objective QA, code generation, and deep information-seeking benchmarks on both dense and MoE backbones.
> ![](images/962d1c0c1807ce6ad8921cda828bd5e55abe0f293b2902719775ba417dd07929.jpg)
> ![](images/d28f29fb99e7edd480e3a928267a7bf60fc7a11cd7f22857fc43e78537db4504.jpg)
> ![](images/5ef1fbf139c3f4063f8306d68a305f7414876ef08c979629dfc91138e5e0204d.jpg)
> Figure 1: Held-out accuracy, tool-use turns per rollout, and trajectory volume over training on BrowseComp-Plus with the Qwen3-32B-Instruct backbone for ECHO (purple), GRPO (orange), and SUPO (green). ECHO traces the upper-left frontier: rising accuracy without the turn and volume growth seen for SUPO.

Q1: 这篇论文试图解决什么问题？

长程语言智能体在有限上下文窗口下，现有方法（截断、折叠为摘要、选择紧凑记忆）导致两个耦合问题：1）历史观测被逐步移除或压缩，策略难以重用细粒度证据；2）原始轮次不可溯源后，基于结果的强化学习失去显式路径来对齐策略更新与支持成功答案的证据。

Q2: 有哪些相关研究？

现有上下文管理方法包括截断历史（truncation）、折叠为摘要（summarization滚动摘要方法如SUPO）、选择紧凑记忆状态（memory selection）。此外，上下文压缩技术（如Zhu et al. 2024）也被用于工具使用场景。但这些方法在保持可追溯性方面存在不足。

Q3: 论文如何解决这个问题？

ECHO框架包含三个组件：1）压缩每个完成的环境轮次为紧凑记忆记录；2）通过学习的选择器从记忆池中重建有界策略上下文；3）重用所选源索引，将正向结果信用路由到支持成功答案的证据动作和选择动作，实现可追溯的基于结果的强化学习。

Q4: 论文做了哪些实验？

在BrowseComp-Plus（747训练样本）上评估，对比基线为GRPO和SUPO。另在零样本设置下测试多目标QA、代码生成和深度信息搜索基准任务。骨干网络使用Qwen3-32B-Instruct。进行了记忆组件消融实验（选择策略和记忆格式）。

Q5: 发现了什么实验现象？

ECHO在BrowseComp-Plus上达到43.4%保持准确率，优于GRPO（28.9%）和SUPO（36.1%），同时使用更少的工具轮次和轨迹量。消融实验表明，学习的选择策略优于静态语义top-k检索，紧凑格式（最终隐状态）优于完整最后轮次文本。零样本泛化在多任务上均提升，且在稠密和MoE骨干上一致。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：1）将ECHO扩展到更长上下文（如100轮以上）以验证记忆压缩极限；2）探索更细粒度的信用分配（如动作级别而非轮次级别）；3）在更广泛的任务（如具身智能、多智能体协作）中验证框架通用性；4）结合其他优化目标（如偏好优化）改进选择策略。

Q7: 总结一下论文的主要内容

论文提出ECHO，一个选择性轮次记忆框架，用于在有限上下文窗口下训练长程语言智能体。核心思想是让上下文重建同时服务于动作和可追溯学习：压缩每个环境轮次为紧凑记录，通过可学习选择器重建策略输入，并利用源索引将成功结果信用分配给支持证据。在BrowseComp-Plus上达到43.4%准确率，显著优于GRPO和SUPO，且零样本泛化到多领域任务。消融实验验证了记忆组件设计的有效性。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：智能体方向：直接解决长程工具使用智能体的上下文管理问题，核心方法可迁移。

## 基本信息

- 作者：Zijun Xie, Binbin Zheng, Enlei Gong, Jihua Liu, Yuyang You, Lingfeng Liu, Jiayao Tang, Guanqun Zhao, Aoqi Hu, Zeyu Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.31650v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成本报告时参考了PDF语义检索命中的证据片段。
