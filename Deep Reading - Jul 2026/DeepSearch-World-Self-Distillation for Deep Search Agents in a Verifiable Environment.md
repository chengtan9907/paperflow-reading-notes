---
user_id: "cheng tan"
paper_id: 3174
arxiv_id: "2607.07820"
title: "DeepSearch-World: Self-Distillation for Deep Search Agents in a Verifiable Environment"
publish_date: "2026-07-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.07820.pdf"
pdf_url: "https://arxiv.org/pdf/2607.07820"
abs_url: "https://arxiv.org/abs/2607.07820"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-11T00:21:04"
---
# DeepSearch-World: Self-Distillation for Deep Search Agents in a Verifiable Environment

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-distillation · web agents · tool-use · verifiable environment

## 一句话总结

本文提出 DeepSearch-World 确定性可验证环境及 DeepSearch-Evolve 自蒸馏框架，实现 9B 模型在不依赖更强模型蒸馏的情况下通过自我进化达到领先水平的深度搜索智能体。

## 摘要

> Training tool-use agents to improve from their own experience remains challenging, as supervised fine-tuning relies on fixed teacher-distilled trajectories, while sparse-reward reinforcement learning provides weak supervision for long-horizon interactions. We present DeepSearch-Evolve, a self-distillation framework for web agents built on DeepSearch-World, a deterministic and verifiable environment with reproducible search and page-reading tools. DeepSearch-World contains 420K multi-hop QA tasks constructed from entity-level random walks and supports key agentic cognitive behaviors useful for self-evolving, including progress verification, grounded reflection, and failure recovery. DeepSearch-Evolve iteratively performs trajectory generation, filtering, data mixing, and fine-tuning to train stronger agents. Without distillation from more capable models, DeepSearch-World-9B achieves competitive performance compared with open-source agents, reaching 31.2% on BrowseComp, 61.5% on GAIA, and 93.4% on HotpotQA, showing that verifiable environments enable scalable self-evolution for long-horizon web agents. We will release the environment, 420K training pool, validation set, model, and code to facilitate future research on self-improving deep search agents.

Q1: 这篇论文试图解决什么问题？

训练工具使用智能体（tool-use agents）从自身经验中改进仍然面临挑战。监督微调（SFT）依赖于固定的教师蒸馏轨迹，无法从 agent 自身探索中学习；而稀疏奖励强化学习（RL）对于长程交互提供弱监督信号，导致训练不稳定和效率低下。现有开源 deep-search agents 往往需要蒸馏更强模型（如 GPT-4、Claude）的轨迹或使用多 agent 协作，限制了自我进化的能力。因此，如何在无需更强模型监督的情况下，让 agent 通过自演化持续提升性能，是一个关键问题。

Q2: 有哪些相关研究？

相关工作包括：（1）开源 deep-search agents：如 WebAgent、Agent-Bench、BrowserGym 等，多数依赖外部教师蒸馏或复杂奖励 shaping；（2）可验证环境：如 MathWorld、ChemistryWorld 等确定性环境用于智能体训练，但缺乏 web search 的多样性和长程推理；（3）自我改进方法：如 RLAIF、self-play、iterative self-distillation，但以往多用于游戏或代码生成，尚未在 web search agent 中得到充分验证。本工作填补了这一空白，提出专门用于 web search 的可验证环境和自蒸馏框架。

Q3: 论文如何解决这个问题？

论文提出 DeepSearch-Evolve 自蒸馏框架，基于 DeepSearch-World 可验证环境。该框架包含三个关键组件：（1）DeepSearch-World：基于离线 Wikipedia 构建的确定性工具环境，提供可复现的搜索和阅读工具，支持进度验证、基于发现的反思和失败恢复。（2）多跳 QA 数据构造：通过实体级随机游走生成 420K 多跳 QA 任务，覆盖复杂推理。（3）自进化训练循环：在验证环境下，当前模型生成轨迹，通过过滤和选定高质量轨迹（结合硬目标完成和软过滤），与原始训练数据混合，对模型进行微调。迭代重复上述过程，模型性能逐步提升。

Q4: 论文做了哪些实验？

论文在三个基准数据集上评估 DeepSearch-World-9B：BrowseComp、GAIA 和 HotpotQA。与开源和专有系统对比，包括 GPT-4 with browsing、Qwen2.5-72B、WebGPT 等。主要实验包括：（1）主结果对比（Table 1），显示 DeepSearch-World-9B 达到 31.2% on BrowseComp、61.5% on GAIA、93.4% on HotpotQA，在开源模型中具有竞争力；（2）消融研究，分析不同训练策略（如自蒸馏迭代轮数、数据混合比例等）的影响；（3）失败案例分析和行为分析，展示 agent 的进度验证和失败恢复能力。此外，还进行了泛化性测试，验证模型在未见过的任务上的表现。

Q5: 发现了什么实验现象？

实验现象主要包括：（1）DeepSearch-World-9B 在 BrowseComp 上达到 31.2%，与依赖更强模型蒸馏的 Agent-Bench（30.5%）相当；在 GAIA 上 61.5%，优于多数开源模型；在 HotpotQA 上 93.4%，接近专门优化模型。（2）自蒸馏迭代训练带来持续增益：随着迭代次数增加，性能在三个基准上均稳定提升，没有出现过拟合或退化。（3）消融表明，进度验证和反思信号对性能贡献显著；去掉这些信号后，性能下降约 5-8%。（4）分析表明，模型在长程推理步骤中出现了成功的失败恢复行为，能够从错误搜索路径中回退并重新规划。（5）负结果：当训练数据仅包含简单任务时，自蒸馏增益有限，说明任务难度多样性很重要。（6）未观察到明显的 scaling 趋势：模型大小从 7B 到 72B，性能增益递减，但 9B 在效率上更优。

Q6: 有什么可以进一步探索的点？

未来可探索方向包括：（1）将 DeepSearch-World 扩展到实时网络搜索环境，使其具备时效性；（2）引入多模态信息（如图片、表格）以丰富任务类型；（3）探索更复杂的 agent 角色，如多 step memory、计划与执行分离；（4）将自蒸馏框架与强化学习结合，利用可验证信号直接优化策略；（5）在更大规模模型（如 70B）上验证方法的可扩展性；（6）研究自蒸馏训练中数据多样性对泛化的影响，以及如何自动化生成更多样化的任务；（7）迁移到其他领域（如代码、SQL、科研推理）验证框架通用性。

Q7: 总结一下论文的主要内容

本文针对工具使用智能体的自我改进难题，提出了 DeepSearch-World 可验证环境和 DeepSearch-Evolve 自蒸馏框架。首先，基于离线 Wikipedia 构建了包含搜索和阅读工具的确定性环境，支持进度验证和反思信号。其次，通过实体级随机游走生成了 420K 多跳 QA 任务。然后，设计迭代自蒸馏训练循环，包括轨迹生成、质量过滤、数据混合和模型微调。在 BrowseComp、GAIA 和 HotpotQA 上的实验表明，DeepSearch-World-9B 在无需更强模型蒸馏的情况下达到了与依赖外部教师模型的方法相竞争的性能。消融和案例研究验证了框架各组件的重要性以及 agent 的自纠正能力。该工作证明了可验证环境对于长程、深度搜索智能体自我进化的可扩展性，并提供了环境、数据集、代码和模型以促进后续研究。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文与智能体（agent）方向高度相关，权重 0.10，直接涉及 self-improving agents 和 tool-use agent。

## 基本信息

- 作者：Xinyu Geng, Xuanhua He, Sixiang Chen, Yanjing Xiao, Fan Zhang, Shijue Huang, Haitao Mi, Zhenwen Liang, Tianqing Fang, Yi R. Fung
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.07820`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本生成参考了 PDF 语义检索证据，包括摘要、引言、方法论、结论等片段，以及 field_evidence_map 的对应关系。
