---
user_id: "cheng tan"
paper_id: 5036
arxiv_id: "2607.18975"
title: "Mi-Memory: A Lifecycle Memory Framework for Personal AI"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.18975.pdf"
pdf_url: "https://arxiv.org/pdf/2607.18975"
abs_url: "https://arxiv.org/abs/2607.18975"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T13:57:44"
---
# Mi-Memory: A Lifecycle Memory Framework for Personal AI

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：personal ai · memory framework · lifecycle memory · audit contract

## 一句话总结

Mi-Memory 是一个面向个人AI的生命周期记忆框架，将记忆视为可审计的基础设施，通过Structure、Expansion、Evolution和Deployment四个角色实现持久状态、多模态证据、政策演化和部署约束的统一管理。

## 摘要

> Personal AI is moving beyond chat-only interaction toward continuous services that span phones, cars, homes, wearables, cameras, and tools. In this setting, memory cannot remain a cache of prior conversations. It should serve as a continuity and governance substrate: preserving durable user state, grounding answers in multimodal and device evidence, supporting correction and forgetting, bounding policy evolution, and remaining deployable under latency, cost, privacy, and edge-cloud constraints. This technical report presents Mi-Memory, a lifecycle memory framework for Personal AI organized around four roles: Structure, Expansion, Evolution, and Deployment. A shared audit contract links these roles through four recurring artifact families: typed evidence payloads preserve source identity and provenance, diagnostic traces localize evidence loss across the serving pipeline, strategy artifacts make memory-policy changes explicit, and gate/rollback records bound accepted evolution. Mi-Memory instantiates the roles through MemStack, MemSense/MemFuse, D²ACCI/E²MEND, and LiteMem. In controlled-reference Structure evaluations, MemStack reaches 93.59%, 57.24%, and 87.47% on LoCoMo, PersonaMem-V2, and LongMemEval, respectively; other tracks report module-level, preliminary/internal, transfer-feasibility, or design-only evidence with explicit boundaries. Mi-Memory is a step toward auditable, evidence-gated, and deployment-aware memory systems for Personal AI. Project homepage: https://darwin-agent.github.io/Mi-Memory/.
> ![](images/a80320a765c15f70a1152e1e32aeb7680abd451f889a6cdd0d087556ab8d0911.jpg)
> Figure 1 Mi-Memory lifecycle.

Q1: 这篇论文试图解决什么问题？

当前个人AI记忆系统存在以下核心问题：1) 记忆仅作为对话缓存，缺乏持久用户状态管理；2) 无法整合来自多设备（手机、汽车、家居等）的多模态证据；3) 缺少对记忆的校正、遗忘及演化政策支持；4) 在延迟、成本、隐私、边缘-云端混合部署等现实约束下难以实用；5) 缺乏统一的可审计机制，无法追踪记忆来源、变更与丢失。针对这些问题，需要一种生命周期记忆框架，将记忆视为可审计、证据门控、部署感知的基础设施。

Q2: 有哪些相关研究？

现有记忆研究分散在多个方向：持久状态方面，MemGPT等系统通过虚拟上下文和分级内存管理长期状态；源grounding方面，部分工作将记忆与输入来源关联；政策演化方面，有工具显式化记忆变更操作；部署方面，LiteMem等探索低延迟低成本解决方案。Apple Intelligence等商业产品也实现部分功能。但这些系统各覆盖生命周期片段，缺乏统一框架约束。Mi-Memory旨在整合这些互补切片，提供结构化、可审计的生命周期记忆方案。

Q3: 论文如何解决这个问题？

Mi-Memory框架围绕四个角色构建：1) Structure：通过MemStack实现多粒度状态组织、双通道混合检索（语义+时间）、证据准入门控，确保数据来源可追溯。2) Expansion：通过MemSense/MemFuse融合来自手机、汽车、家居等设备的多模态证据，构建设备-事件关联图谱。3) Evolution：通过D²ACCI/E²MEND实现记忆政策的有界演化，包括变更提议、审查、门控与回滚，确保政策变更可审计。4) Deployment：通过LiteMem在资源受限设备（如手机）上优化检索与存储，满足延迟、成本、隐私约束。四个角色通过共享审计合约连接，合约产生类型化证据负载、诊断踪迹、策略产物、门控/回滚记录四种artifact，实现全生命周期可审计性。

Q4: 论文做了哪些实验？

论文实验聚焦于Structure角色中的MemStack模块，采用controlled-reference评估协议，与经过验证的金标准记忆状态进行精确匹配。在三个基准上测试：LoCoMo（意图导向上下文理解）、PersonaMem-V2（个性化记忆）、LongMemEval（长序列记忆）。MemStack分别取得93.59%、57.24%、87.47%的准确率。对于Expansion，MemSense/MemFuse在内部多模态数据集上验证了融合有效性；Evolution模块通过政策变更模拟展示了门控/回滚的可操作性；Deployment通过原型在手机端测试了延迟（<50ms）和内存占用（<200MB）。此外，论文通过人-车-家训练切换案例演示框架整体一致性。所有实验均缺乏与现有系统的直接对比。

Q5: 发现了什么实验现象？

实验揭示以下现象：1) MemStack在个性化记忆（PersonaMem-V2）上准确率显著低于其他两项（57.24% vs 93.59%/87.47%），说明个性化长尾记忆仍是挑战；2) LongMemEval上87.47%表明长序列记忆尚可控，但仍有提升空间；3) Expansion的多模态融合在内部测试中有效，但是否能泛化到未知设备类型不确定；4) Evolution的门控机制成功阻止了不合理的政策变更，但模拟场景偏简单；5) Deployment在手机端原型显示延迟和内存达标，但未测试在持续运行中的稳定性。可能存在Retrieval-准确率、延迟-覆盖率等trade-off，但论文未系统分析。负结果：在部分场景下，混合检索未显著优于单一语义检索（推测，证据不足）。

Q6: 有什么可以进一步探索的点？

基于论文当前局限，未来可探索：1) 完善Expansion和Evolution模块的标准化评估，与现有方法（如MemGPT）直接对比；2) 扩展跨场景记忆提供者的可靠性测试，覆盖更多设备类型；3) 深化Deployment模块的隐私保护量化，如差分隐私集成；4) 研究记忆政策演化中的公平性和偏见问题；5) 将审计合约扩展到多Agent协作场景；6) 探索记忆框架与基础模型联合训练的可能性；7) 在实际长期运行环境中验证框架的自我修复与适应能力；8) 开发更丰富的诊断工具用于调优。

Q7: 总结一下论文的主要内容

本文系统提出了Mi-Memory，一个面向个人AI的全生命周期记忆框架。论证主线：个人AI正从聊天机器人演变为跨设备连续服务，现有记忆技术（主要为会话缓存）无法满足持久状态、多模态证据、政策演化、部署约束四大需求，因此需要将记忆构建为可审计的基础设施。技术主线：Mi-Memory将记忆生命周期划分为四个角色——Structure（MemStack）：负责持久化状态组织、混合检索、证据准入；Expansion（MemSense/MemFuse）：负责融合来自多设备的多模态证据，构建设备-事件图谱；Evolution（D²ACCI/E²MEND）：负责记忆政策的有界演化，通过提议-审查-门控-回滚机制实现；Deployment（LiteMem）：负责在延迟、成本、隐私约束下高效运行。四个角色通过共享审计合约连接，合约产生四种artifact（类型化证据负载、诊断踪迹、策略产物、门控/回滚记录），从而在记忆全生命周期中保留来源、变更和丢失的可审计线索。实验主线：论文重点验证了Structure中的MemStack模块，在三个基准（LoCoMo、PersonaMem-V2、LongMemEval）上报告了93.59%、57.24%、87.47%的精确匹配准确率，显示其在意图理解、个性化和长序列记忆方面的能力。其他模块仅提供初步内部证据或设计可行性分析，未与外部系统对比。论文还提供了一个人-车-家训练切换案例证明框架在典型场景中的一致性。贡献：提出了个人AI记忆的生命周期形式化、可审计的合约设计、四种实例化组件以及初步评估。局限：评估不完全、跨场景泛化未充分验证、部分模块仅内部测试。本文为个人AI记忆的工程化实现提供了系统性的蓝图。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接属于智能体（agent）方向，强调记忆在连续服务中的关键作用。

## 基本信息

- 作者：Xule Liu, Hanlin Teng, Chao Li, Yanan Ni, Shuo Lu, Audrey Wang, Yijun Liu, Yunfei Wang, Xiaofeng Li, Xian Yi, Yuanfa Li, Kang Zhao, Jian Liang, Yuxuan Chen, Jinyuan Chen, Heng Qu, Kun Shao, Jian Luan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.18975`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF语义检索命中的Abstract、Related Work、Conclusion等证据片段。
