---
user_id: "cheng tan"
paper_id: 6182
arxiv_id: "2607.27080v1"
title: "MemSecBench: Tracking Agent Memory Poisoning from Persistence to Consequence and Repair"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.27080v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.27080v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:27:47"
---
# MemSecBench: Tracking Agent Memory Poisoning from Persistence to Consequence and Repair

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：memory poisoning · agent security · benchmark · lifecycle security

## 一句话总结

提出MemSecBench基准，通过受控的Write-Execute-Forget协议和证据裁决，在24种配置下追踪智能体记忆中毒从持久性到后果及选择性修复的完整生命周期。

## 摘要

> Memory systems allow agents to retain and reuse information from past interactions, but they can also let malicious content persist. A malicious instruction crafted by an attacker may be stored in long-term memory, recalled much later, and quietly shape a real action. Recent benchmarks increasingly examine agent memory security, yet few trace the same malicious semantics across persistence, downstream consequences, and selective repair under diverse memory-backend comparisons. To address this gap, we introduce MemSecBench, a task-grounded benchmark for the lifecycle security of agent memory systems. It contains 310 cases drawn from 48 realistic contexts across code and science, daily life, and office work. Each case follows a controlled Write--Execute--Forget protocol in an isolated runtime under an exact agent configuration, defined by an agent harness, a memory backend, and an LLM backend. Evidence-based adjudication combines a deterministic write check, checkpoint-specific judge-model evaluations, and programmatic gates across seven lifecycle checkpoints. The experimental design spans a 24-configuration matrix of two agent harnesses, four memory backends, and three LLM backends. Across all 24 configurations, malicious memory persists in 84.2% of all cases, and the full Write--Execute chain succeeds in 50.3%. Among successfully poisoned cases, 59.6% complete the full Execute chain, while 56.1% achieve selective repair.Compared with matched Native configurations, the largest absolute differences are 16.1 percentage points for end-to-end attack success and 41.3 percentage points for selective repair. These descriptive contrasts indicate that the evaluated memory system stacks differ in lifecycle security, both in the propagation of malicious memory and in selective repair after successful memory poisoning.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决现有智能体记忆安全基准中的两个关键空白：一是缺乏对恶意记忆从写入、持久化、下游影响到选择性修复的完整生命周期追踪，现有评估往往只关注单一点（如注入或持久化），忽略了跨会话的行为和修复能力；二是缺少对不同记忆后端、代理框架和LLM后端组合的系统比较，实际部署的记忆系统堆栈是组件联合工作的，但基准常隔离组件评估，导致无法反映真实安全风险。此外，现有工作未提供标准化协议来复制和比较不同配置下的安全属性。因此，需要建立一个能同时追踪恶意语义生命周期并进行跨配置对比的任务基准。

Q2: 有哪些相关研究？

相关研究主要涵盖两个方面：1）智能体记忆中毒攻击与安全研究，如Chen等人（2024）、Dash等人（2026）、Xie等人（2026）、Al-Tawaha等人（2026）的工作表明在LLM代理中恶意内容可持久存储并影响后续行为，展示了记忆中毒的可行性，但这些工作未提供受控的生命周期基准，也未对不同记忆后端进行直接比较。2）通用智能体安全基准和后门攻击评估，例如针对LLM后门、提示注入的基准，但较少关注记忆系统特有的跨会话持久性与选择性修复。目前缺乏将写入、执行和遗忘统一到一个框架中的评估方法，也缺乏对多种记忆后端栈（如Native、Mem0、A-Mem等）的对比。本文旨在填补这一空白。

Q3: 论文如何解决这个问题？

本文提出MemSecBench基准，通过以下方式解决问题：首先，构建一个包含310个案例的数据集，源自48个真实世界上下文，覆盖代码与科学、日常生活和办公工作三大领域，每个案例包含一个攻击指令和一个良性任务。其次，设计受控的Write-Execute-Forget协议：在写入阶段，攻击指令通过处理文档等渠道注入，并由确定性检查验证是否成功存储；在执行阶段，同一记忆状态下触发代理在正常任务中召回恶意内容，并观察是否产生有害行为；在遗忘阶段，要求代理选择性修复或删除恶意内容，同时保留良性信息。每个阶段设置七个生命周期检查点，组合写入检查、评判模型评估和程序化门控进行证据裁决。第三，定义实验矩阵为2种代理框架（如OpenClaw、Hermes）、4种记忆后端（Native、Mem0、A-Mem等）和3种LLM后端，共24种配置。通过这三个层次的规范化，MemSecBench能够系统评估不同配置下恶意记忆的持久成功率（MPSR）、端到端攻击成功率（E2E-ASR）和选择性修复成功率（SRSR）。

Q4: 论文做了哪些实验？

论文在全部24种配置（2代理框架×4记忆后端×3LLM后端）下对310个案例进行实验。实验使用三个主要指标：MPSR衡量恶意内容成功写入并持久化的比例；E2E-ASR衡量从写入到执行完整恶意链的比例；SRSR衡量在成功中毒案例中，恶意内容被选择性修复同时良性信息保留的比例。对于每种配置，计算聚合指标并比较不同后端的差异。实验还设置了与Native记忆配置（模型原生记忆）的对照。由于框架限制，实验未涉及攻击者如何获得会话访问，而是聚焦于记忆系统内部的安全属性。实验通过七阶段检查点确保数据完整性，包括写入检查点的确定性验证、执行检查点的评判模型评估、遗忘检查点的程序化门控。

Q5: 发现了什么实验现象？

实验揭示以下主要现象：1）恶意记忆具有高持久性：在所有24种配置中，平均MPSR达84.2%，表明绝大多数攻击案例能成功将恶意内容写入记忆并保持持久，这凸显了记忆系统的一个基本脆弱性。2）完整攻击链成功率约为一半：平均E2E-ASR为50.3%，即约一半的案例能从写入成功导向有害执行，但不同配置间波动较大（最大绝对差异在端到端成功率上为16.1个百分点），说明记忆后端和代理框架对攻击传播有显著影响。3）选择性修复的难度与后端强相关：在成功中毒案例中，平均SRSR为56.1%，但Native记忆后端与外部后端之间差异显著——选择性修复的最大绝对差异高达41.3个百分点，表明外部记忆后端在消除恶意内容的同时保留良性信息方面面临更大挑战。4）配置间交互效应明显：不同的代理框架和LLM后端搭配也会改变安全状况，说明不能仅凭单一组件（如仅LLM或仅记忆后端）来评估安全性，而必须考虑整个堆栈的组合效果。

Q6: 有什么可以进一步探索的点？

根据论文结论和讨论，未来工作可以从以下几个方向展开：1）开发机制特定的攻击，例如针对特定记忆后端的检索策略或记忆压缩机制的对抗输入，以更深入地揭示不同系统的脆弱点。2）通过受控消融实验隔离记忆后端的具体影响，明确每个组件在生命周期安全中的贡献。3）自动化基准构建，利用LLM生成更丰富和多样化的案例，减少人工构造的代价，同时覆盖更多边缘情况。4）扩展任务领域和操作场景，包括多模态记忆（如图像）、长期持续交互、多用户共享记忆等，以评估更复杂的现实威胁。5）补充外部威胁模型，研究攻击者如何获取账户或会话访问权限，将记忆中毒放入更大的攻击链中分析。

Q7: 总结一下论文的主要内容

本文关注LLM代理记忆系统的安全问题，提出了MemSecBench基准。论文首先指出，尽管记忆系统提升了代理的长程任务能力，但也引入了记忆中毒风险：恶意指令可被写入并持久存在，在后续会话中被召回导致危害。现有基准缺乏对恶意语义在写入、持久、影响和修复整个生命周期的追踪，也缺少不同记忆后端、代理框架和LLM后端的系统比较。为填补这一空白，MemSecBench被提出。它包含310个案例，源自48个真实上下文，覆盖代码与科学、日常生活和办公工作。每个案例遵循受控的Write-Execute-Forget协议，在隔离运行时中按精确配置（代理框架、记忆后端、LLM后端）执行，并通过七个生命周期检查点的证据裁决进行评价。实验在24种配置（2种代理框架、4种记忆后端、3种LLM后端）上进行。关键结果包括：恶意记忆持久率平均84.2%，端到端攻击成功率50.3%，选择性修复成功率56.1%。不同记忆后端间差异显著：与Native配置相比，外部后端在端到端攻击成功率上最大绝对差异16.1%，在选择性修复上最大绝对差异41.3%。这些结果强调，记忆安全取决于完整的代理配置栈，而非单个组件。论文的主要贡献包括提出了新的基准和可复现的协议，提供了大规模多配置比较，并揭示了生命周期安全的关键差异。局限性包括未建模攻击者访问权限、未开展后端消融、数据集规模有限等。未来工作将包括开发机制特定攻击、消融实验、自动化构建和领域扩展。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文直接切入智能体安全领域，与用户画像中agent方向（权重0.10）高度吻合。

## 基本信息

- 作者：Xuanze Chen, Xukang Xie, Wentao Fu, Jiajun Zhou, Shanqing Yu, Qi Xuan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CR, cs.AI
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.27080v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索证据，包括摘要、引言和结论等片段，并结合元数据和启发式草稿进行补充。
