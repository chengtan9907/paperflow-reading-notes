---
user_id: "cheng tan"
paper_id: 440
arxiv_id: "2606.16523v1"
title: "SkillWiki: A Living Knowledge Infrastructure for Agent Skills"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16523v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16523v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:08:37"
---
# SkillWiki: A Living Knowledge Infrastructure for Agent Skills

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：skill infrastructure · agent skills · knowledge management · lifecycle governance

## 一句话总结

SkillWiki 是一个为智能体技能提供大规模生产、治理和持续演化的统一基础设施，将异构知识转化为可复用、可追溯的技能资产。

## 摘要

> 虽然知识通过维基百科管理，软件通过GitHub管理，但智能体技能仍缺乏大规模生产、治理和演化的基础设施。SkillWiki 是一个活的知识基础设施，通过将异构知识转化为与其原始证据相连的可复用技能资产，支持智能体技能的组织、基础化和持续演化。我们的演示展示了完整的技能生命周期，从知识摄取、技能生产到具有溯源探索、治理和执行驱动的演化。SkillWiki 预示了一个知识、技能和执行经验在共享基础设施中共同演化的未来。

Q1: 这篇论文试图解决什么问题？

随着基于大语言模型的智能体从执行孤立工具调用演变为自主执行复杂长程任务，它们需要获取大量可复用技能。然而，当前缺乏一个统一的基础设施来支持技能的大规模生产、治理、版本控制、可追溯性和持续演化。技能集合日益庞大和异构，现有方法在技能获取、记忆和生命周期管理方面各自为政，缺乏整合的治理框架。

Q2: 有哪些相关研究？

相关研究包括技能获取（Wang et al., 2023; Si et al., 2026; Yang et al., 2026）、技能记忆（Zhang et al., 2026b; Lin et al., 2026）、技能演化和生命周期管理（Shen et al., 2026; Liu et al., 2026; Lin et al., 2026）。这些工作使智能体能够持续从经验中学习，但缺乏统一的治理和溯源机制。SkillWiki 通过知识图谱将技能、工具和知识源关联，并提供端到端生命周期管理。

Q3: 论文如何解决这个问题？

SkillWiki 提供了一个统一基础设施，包含两个核心组件：1) 知识接地技能构建：将异构知识（轨迹、文档、API规范、脚本、历史技能）转化为可执行、可验证、可版本化的技能资产，并与原始证据链接。2) 生命周期治理框架：支持技能从生产、验证、发布、执行、修复、版本控制、弃用到归档的完整状态转换（S0-S7），通过依赖图捕获技能生态中的谱系和演化关系。

Q4: 论文做了哪些实验？

论文从两个角度评估SkillWiki：1) 知识到技能的生产：使用125个人工制品（涵盖轨迹、文档、API规范、脚本和历史技能五种来源类型）测试异构知识能否可靠转换为受治理的技能资产。2) 生命周期治理与演化：通过一个完整的案例研究，使用API文档人工制品，跟踪其经历生产、验证、发布、执行、修复、版本控制、弃用和归档的全过程。实验展示了技能遍历所有生命周期状态（S0-S7），证明SkillWiki支持端到端生命周期管理。

Q5: 发现了什么实验现象？

在125个人工制品的知识到技能工作流中，SkillWiki能够从各类异构源可靠地产生技能候选并集成到仓库中，验证了连续技能生产的可行性。在生命周期案例中，技能成功经历了所有状态，包括弃用和归档，展示了治理框架的有效性。论文未报告具体的量化指标（如成功率、效率等），但强调端到端管理的可行性。

Q6: 有什么可以进一步探索的点？

进一步探索点包括：1) 更大规模的知识源和技能集合下的可扩展性；2) 多智能体协作场景下的技能共享与演化；3) 技能质量的自动化评估与保证机制；4) 基于执行反馈的技能自主进化策略；5) 跨领域知识迁移与技能复用；6) 与现有知识管理基础设施（如维基百科）的整合。

Q7: 总结一下论文的主要内容

论文提出SkillWiki，一个用于智能体技能的统一活知识基础设施。目标是解决当前智能体技能缺乏大规模生产、治理和演化基础设施的问题。SkillWiki将异构知识（轨迹、文档、API规范、脚本、历史技能）转化为可复用、可执行、可验证、可版本化的技能资产，并通过知识图谱关联技能与原始证据。它提供了一个完整的生命周期治理框架，覆盖从生产到归档的状态转换（S0-S7）。实验使用125个人工制品验证了知识到技能的生产可行性，并通过一个API文档案例演示了端到端生命周期管理。论文贡献包括：统一基础设施、知识接地技能构建、生命周期治理框架、开源实现。局限性包括：依赖LLM生成质量、技能质量评估尚未全面涉及、大规模部署验证不足。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：系统工作，侧重基础设施而非算法创新，评估系统性。

## 基本信息

- 作者：Dingcheng Huang, Yuda Ding, Bingshuo Liu, Qingbin Liu, Xi Chen, Jiang Bian, Hongliang Sun, Zhiying Tu, Dianhui Chu, Xiaoyan Yu, Dianbo Sui
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16523v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 参考了PDF检索证据
