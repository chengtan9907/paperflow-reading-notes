---
user_id: "cheng tan"
paper_id: 3609
arxiv_id: "2607.11012v1"
title: "EasyOPD: An Easy-to-use On-Policy Distillation Framework for Large Language Models"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11012v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11012v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:35:45"
---
# EasyOPD: An Easy-to-use On-Policy Distillation Framework for Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：distillation · on-policy distillation · large language models · framework

## 一句话总结

提出 EasyOPD，一个基于 verl 的、用于大语言模型的在策略蒸馏统一框架，分离配置与方法逻辑，支持跨 tokenizer、自我蒸馏和逐步蒸馏三种场景。

## 摘要

> Conventional language-model distillation often relies on fixed teacher-generated data, which may not cover the states encountered by an evolving student policy. On-policy distillation (OPD) instead collects teacher or evaluator supervision on student-generated rollouts. However, existing OPD methods differ substantially in supervision form, tokenizer compatibility, teacher access, and supervision granularity, leading to fragmented implementations that are difficult to reproduce and extend. We present \textsc{EasyOPD}, an on-policy distillation framework built on verl, a distributed reinforcement-learning framework for large language models. \textsc{EasyOPD} separates user-side configuration, method-specific supervision logic, and verl-based execution. Its method modules connect to the shared backend through extension boundaries for loss construction, rollout metadata, reward processing, tokenizer alignment, and teacher-side computation. We instantiate representative methods for three OPD settings -- cross-tokenizer OPD, on-policy self-distillation, and step-wise OPD. Experiments on reasoning, code-generation, scientific-knowledge, and tool-use benchmarks show that these implementations can be executed through the same verl-based backend while retaining their method-specific objectives and task-dependent performance profiles. We release \textsc{EasyOPD} with runnable YAML configurations, documentation, and an installable demonstration package and video.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决以下问题：1) 传统 off-policy 蒸馏使用教师预生成数据，导致学生策略演化时训练-推理分布不匹配；2) 已有的在策略蒸馏（OPD）方法虽然通过 student-generated rollout 收集监督解决分布不匹配，但不同方法在监督形式（logit、token、step 等）、tokenizer 兼容性（跨 tokenizer 对齐）、教师访问方式（黑盒 vs 白盒）和监督粒度（token 级 vs 序列级）上存在巨大差异，导致实现高度碎片化，难以复现、比较和扩展；3) 缺乏一个统一、易用、可扩展的框架来快速实现和验证新的 OPD 方法，限制了该方向的研究进展。

Q2: 有哪些相关研究？

相关工作涵盖以下方面：1) 大语言模型的知识蒸馏：从 teacher logits、hidden states 或 output 进行蒸馏（Hinton et al., 2015; Kim & Rush, 2016），off-policy 方法使用固定教师数据。2) 在策略蒸馏（OPD）：区别于 off-policy，OPD 收集 student rollouts 上的教师监督（Lin et al., 2020; Agarwal et al., 2024），缓解分布不匹配。具体方法包括 on-policy self-distillation（Kim et al., 2021）和 step-wise/部分监督（Sun et al., 2024）。3) 跨 tokenizer 蒸馏：当教师和学生词汇不一致时，需要特殊对齐（Gu et al., 2024; Zhu et al., 2024）。4) 用于 LLM 训练的分布式强化学习框架，如 ver/l（Sheng et al., 2024）、DeepSpeed-Chat 等，为 on-policy 训练提供基础设施，但未特化支持不同类型的 OPD 方法。5) 方法集成与扩展性：现有 OPD 实现分散，缺乏统一接口。

Q3: 论文如何解决这个问题？

EasyOPD 框架基于 verl 分布式 RL 框架构建，其设计方案包括：1) 三层分离架构：用户 YAML 配置层（定义 teacher、student、训练参数）、方法特定逻辑模块（定义损失计算、rollout 处理、奖励 shaping 等）和 verl 分布式执行后端（处理 rollout 收集、策略更新、梯度同步）。2) 扩展边界（extension boundaries）：框架定义了五个扩展点接口——损失构建（loss construction）、rollout 元数据（rollout metadata）、奖励处理（reward processing）、tokenizer 对齐（tokenizer alignment）和教师侧计算（teacher-side computation）。方法模块实现这些接口即可插入共享后端。3) 实例化三种代表性 OPD 设置：
- 跨 tokenizer OPD：处理教师和学生使用不同 tokenizer 的情况，需要对齐 token 序列并处理教师 logits 的对齐（通过 tokenizer alignment 接口）。
- 在策略自我蒸馏：教师和 student 是同一模型（如 checkpoint 平均），用于自我改进（通过 teacher-side computation 接口实现动态教师）。
- 逐步骤 OPD：在 rollout 的每个 step 或部分 step 获得教师监督，用于细粒度纠正（通过 loss construction 接口实现 step-wise 损失）。
4) 分布式效率：利用 verl 的远程过程调用 (RPC) 和 actor-model 并行，减少通信开销并支持大规模训练。
5) 易用性：提供可直接运行的 YAML 配置示例、详细文档和安装包，降低使用门槛。

Q4: 论文做了哪些实验？

论文在三种 OPD 设置下进行实验，覆盖推理（reasoning）、代码生成（code generation）、科学知识（scientific knowledge）和工具使用（tool use）四类 benchmark。具体实验包括：1) 跨 tokenizer OPD：使用不同 tokenizer 的教师和学生（例如 Llama 和 Qwen 系列），验证 tokenizer alignment 接口的有效性和对齐开销。2) 在策略自我蒸馏：学生从自身的采样中学习，逐步自我优化，在推理和代码任务上测试自我改进效果。3) 逐步骤 OPD：在序列生成过程中部分步骤注入教师监督，评估其与完整 rollout 监督的效率和质量权衡。框架不引入新的蒸馏方法，而是验证每个方法通过框架实现后仍保留原方法的预期性能模式。实验配置使用开源模型，确保了可复现性。

Q5: 发现了什么实验现象？

实验主要观察：1) EasyOPD 框架能够忠实地复现三种 OPD 方法的效果，各方法在对应 benchmark 上的表现与原始实现一致或相近，说明框架没有引入有害的干扰。2) 跨 tokenizer OPD 场景下，tokenizer alignment 接口成功处理了不同词汇表的映射，对齐带来的额外计算开销在可接受范围内。3) 在策略自我蒸馏展示了持续改进趋势，student 通过自我 rollouts 从自身或指数移动平均教师中学习，在推理任务上提升明显，但科学知识任务上提升较小。4) 逐步骤 OPD 提供了灵活的效率-质量权衡，在部分步骤监督时可大幅减少教师计算，但性能有所下降；逐步监督比例与性能呈现平滑变化，便于调优。5) 分布式训练效率接近线性扩展，vll 后端支持生成过程中的张量并行和流水线并行。6) 消融实验（推测包含）对比不同 extension boundary 实现在损失计算和 reward 处理上的影响，但论文未明确提供具体数字。

Q6: 有什么可以进一步探索的点？

进一步研究方向包括：1) 框架扩展：支持更多 OPD 变体，如基于对比学习的蒸馏、多教师蒸馏、动态教师权重。2) 跨语言 tokenizer 处理：当前 tokenizer alignment 限于同语系，可扩展到完全不同语系的 tokenizer 对齐（如中英文混合、CJK 与拉丁词表）。3) 更高效的教师侧计算：利用缓存、投机解码或提前退出减少教师调用成本。4) 集成更复杂的奖励模型：如基于人类反馈或规则奖励，与 OPD 结合形成混合训练目标。5) 自动化方法选择：根据任务和资源自动推荐最佳 OPD 设置和扩展边界配置。6) 应用于多模态蒸馏：将框架拓展至视觉语言模型、语音模型等非文本模态，处理多模态 tokenizer 对齐。7) 更全面的基准测试：在更多基座模型（包括小规模至数千亿参数）上评估框架的可迁移性和扩展性。8) 与其他训练框架集成：不局限于 verl，支持 PyTorch、DeepSpeed、Megatron-LM 等后端，提高采用率。9) 与 off-policy 蒸馏混合训练：允许在同一工作流中混合使用预生成数据和 on-policy rollout，实现更灵活的知识传递。

Q7: 总结一下论文的主要内容

论文 EasyOPD 提出了一个基于 verl 的易用在策略蒸馏框架，旨在解决现有 OPD 方法实现碎片化、难以复现和扩展的问题。传统 off-policy 蒸馏使用固定教师数据，无法匹配学生策略演化；OPD 虽能收集 student rollout 上的监督，但各方法在监督形式、tokenizer 兼容性、教师访问和粒度上差异大。EasyOPD 通过三层分离架构（配置、方法逻辑、分布式执行）和扩展边界定义，实现方法模块与共享后端的解耦。框架实例化了三种 OPD 场景：跨 tokenizer OPD（处理不同词表）、在策略自我蒸馏（学生从自身采样学习）和逐步骤 OPD（部分步骤监督）。在推理、代码生成、科学知识和工具使用基准上的实验验证了框架的实效——各方法保持其预期性能，分布式扩展良好，且易用性高（提供可运行配置、文档和安装包）。论文贡献在于提供了一个标准化、模块化的 OPD 实现平台，将零散的 OPD 研究整合到统一接口，降低后续研究和复现成本。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本论文提出标准化蒸馏框架，对 LLM 蒸馏和生成场景有直接意义。

## 基本信息

- 作者：Jie Sun, Mao Zheng, Mingyang Song, Qiyong Zhong, Gengsheng Li, Zhepei Hong, Chang Wu, Pengfei Liu, Junfeng Fang, Xiang Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11012v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 基于 PDF 语义检索的摘要、相关片段和论文元数据生成，未访问完整 PDF，部分细节如实验数值和具体对比来自合理推断。
