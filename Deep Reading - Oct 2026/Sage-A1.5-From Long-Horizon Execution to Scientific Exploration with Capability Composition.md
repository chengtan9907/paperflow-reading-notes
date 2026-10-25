---
user_id: "cheng tan"
paper_id: 17285
arxiv_id: "2610.28578"
title: "Sage-A1.5: From Long-Horizon Execution to Scientific Exploration with Capability Composition"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.28578.pdf"
pdf_url: "https://arxiv.org/pdf/2610.28578"
abs_url: "https://arxiv.org/abs/2610.28578"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-21T11:48:46"
---
# Sage-A1.5: From Long-Horizon Execution to Scientific Exploration with Capability Composition

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic orchestration · agentic verification · long-horizon agent · capability composition

## 一句话总结

Sage-A1.5 提出‘能力组合’（Capability Composition）这一新的智能体能力发展维度，通过智能体编排（Agentic Orchestration）与智能体验证（Agentic Verification）两条训练主线，把任务分解、并行协作、分支合并与证据复核显式编码进模型的推理文本，使一个 35B MoE 模型从长时程任务执行扩展到科学探索。

## 摘要

> 论文承接前期工作 Agents-A1（35B MoE 智能体模型，通过扩展智能体时程而非模型参数提升长时程任务性能）与知识-动作基础设施（连接外部知识、工具调用、观测与验证反馈，平均 45K token 的训练轨迹），指出仅靠领域技能（capability coverage）不足以应对软件工程、机器学习优化和科学研究中的复杂任务：智能体需要管理相关子任务、比较不同方案、在新证据出现后修正决策。Sage-A1.5 将能力发展扩展到第二个维度——能力组合（capability composition），即问题解决能力在执行过程中的组织、协调与自适应方式，具体通过 Agentic Orchestration（如何分解、委派、整合多个智能体执行）与 Agentic Verification（何时重新评估中间判断与探索策略、并用评估结果修正后续动作）实现。验证反馈被直接写入模型推理文本，使模型学习何时验证、验证什么以及验证结果如何影响后续动作。论文通过广泛基准评测和科学案例研究展示了从孤立任务求解转向长时程科学研究的能力。

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的问题是：仅靠更强的领域技能（capability coverage）不足以完成复杂的现实任务，长时程执行本身并不保证更好的结果。论文明确指出三类失败模式：第一，单个智能体逐步执行时，随着上下文增长、探索变慢、错误会向后传导，导致后续决策被污染；第二，智能体可能重复无产出的搜索、建立在未经支持的假设之上、或在未检查全部证据前就接受结论，即‘执行越长不代表越好’；第三，强领域能力与任务组织能力之间存在缺口——软件工程、机器学习优化和科学研究要求智能体管理相关子任务、比较不同方案、随新证据修正决策，而当时的主流方法没有把‘执行如何组织’和‘进度如何检查’作为显式的训练目标。因此，论文要解决的核心问题是：如何把任务委派、并行协作、证据复核和策略调整这些执行组织能力，从隐式的 prompt 工程或个人技巧变成模型训练中可学习、可评测、可验证的显式能力，从而在较小模型规模上实现科学探索级别的长时程智能体行为。这实际上是提出一个新的能力维度划分：在‘会做什么’（coverage）之外，还要训练‘如何把多段执行组合起来’（composition）。

Q2: 有哪些相关研究？

论文的相关研究脉络可以从四个层面理解。第一层：直接前作与自有基础设施——Agents-A1 是 35B MoE 智能体模型，主张用扩展智能体时程（agent horizon）替代扩展模型参数来提升长时程任务表现；配套的知识-动作基础设施连接外部知识、工具调用、观测与验证反馈，产出平均 45K token 的训练轨迹，并在 multi-step search、科学推理、指令遵循和工具使用等基准上匹配或超过万亿参数模型。第二层：领域技能与复杂任务的张力——论文引用近期进展 [2-5] 表明，软件工程、机器学习优化和科学研究需要智能体管理相关子任务、比较不同方案、随新证据修订决策，单模型逐步执行会因上下文增长、探索缓慢和错误后向传导而失败，因此需要任务委派、并行协作、证据复核与策略调整等组织能力。第三层：训练范式与执行环境——证据片段显示，Sage-A1.5 使用代码沙箱隔离执行、工具结果回放、可复现执行机制、扩展多轮 SFT，工具集包括 Search、Visit、Scholar，训练数据覆盖 coding、long-context 等领域，这些与 Agents-A1 的知识-动作基础设施一脉相承，并且和当前 agent 训练中‘过程监督 + 可验证环境’的范式吻合。第四层：评测生态——论文相关基准包括 SciCode（科学家整理的科研编码基准）、SWE-rebench-V2（软件工程修复任务）、LongBlocks（16K-128K token 长文档、24 个细粒度任务类别），说明评测横跨科学编码、软件工程和长上下文推理。

Q3: 论文如何解决这个问题？

论文的解决方案是提出‘能力组合’（Capability Composition）作为与‘能力覆盖’（Capability Coverage）正交的第二能力维度，并把它拆成两大可训练模块。第一是智能体编排（Agentic Orchestration）：决定工作如何分解、委派、在多条智能体执行之间整合；其关键洞察是把‘执行如何组织’变成模型训练的显式目标——学习该做什么工作以及如何把结果组合起来，与学习解决单个任务互补。具体机制涉及主智能体（main agent）将不同约束并行委派给多个并发子代理（concurrent subagents），子代理在独立上下文、独立模型/工具轮次中工作，然后把分支描述与证据组合成一个问题。证据片段表明其数据结构是‘围绕共享答案形成的星形树图’（starlike tree graph around the shared answer，Fig. 4b），分支描述和证据被合并进一个问题，专有名称和识别性细节会被掩码处理。第二是智能体验证（Agentic Verification）：决定何时重新评估中间判断与探索策略，并用发现修正后续动作；其机制是把验证反馈直接写入模型推理文本（incorporated directly into the model's reasoning text），让模型学习何时验证、检查什么内容、验证结果如何指导后续动作，而外部监督仅用于构建训练数据。训练上采用扩展多轮 SFT（Extended Multi-Epoch SFT），在高/中难度、推理/研究类任务上训练，配合代码沙箱隔离执行、工具结果回放、可复现执行机制，以及 Search/Visit/Scholar 等工具集，覆盖 coding 和 long-context 等多领域数据。任务构建横跨多个场景：从 LongBlocks 文档（16K-128K token）构造覆盖 24 个细粒度类别的问题，包括跨文档事实整合、时间线等；软件工程任务来自 SWE-rebench-V2 与内部构建任务，统一转换为通用格式。整体上，执行组织与反馈驱动调整成为与领域问题求解并列的显式训练目标。

Q4: 论文做了哪些实验？

论文的实验设计可以按证据强度分为三层。第一层是基础能力评测（来自 Introduction 复述）：在 multi-step search、科学推理、指令遵循和工具使用等 benchmark 上评估 Sage-A1.5，与万亿参数模型对比，这类评测继承 Agents-A1 的评估体系。第二层是长时程与科学探索评测（来自摘要语义）：论文声明通过 broad benchmark evaluations 和 scientific case studies 展示从孤立任务求解到 long-horizon scientific research 的转变，说明评测包含标准基准与科学案例研究两类。第三层是训练与任务构建实验（来自 Method 证据）：训练采用 Extended Multi-Epoch SFT，任务难度标注为 Medium/High，类型包括 Reasoning/Research；执行环境包含代码沙箱隔离执行、工具结果回放、可复现执行；长上下文任务从 LongBlocks 文档构造，覆盖 16K-128K token、24 个细粒度类别（跨文档事实整合、时间线等）；软件工程任务池来自 SWE-rebench-V2 公开数据集与内部构建任务，统一转换为通用格式。需要指出：具体评测集名称清单、对比模型版本、数值结果与消融设计在本轮证据中不完整。

Q5: 发现了什么实验现象？

本轮证据以 Method 为主，可归纳的观察点如下：其一，执行结构上的设计观察——并行子代理在独立上下文、独立模型/工具轮次下工作，最终围绕共享答案形成星形树图（Fig. 4b），说明论文在设计中观察到‘并行分支需要结构化合并’这一需求，且要求对分支描述与证据在合并前做专名与识别细节掩码。其二，训练信号设计上的观察——验证反馈直接写入模型推理文本而非作为外部指令，说明论文的观察是‘验证行为必须内化为推理过程的一部分’，外部监督只用于构造训练数据。其三，任务构建上的观察——长上下文任务需要覆盖 24 个细粒度类别，软件工程任务需要统一异构数据源格式，说明论文观察到单一任务形态不足以训练通用能力组合。其四，前提性观察——Introduction 明确指出执行更长不一定更好，代理会重复无产出搜索、依赖未验证假设、过早接受结论，这是论文最核心的经验性观察。至于具体的量化实验现象（消融趋势、scaling 行为、失败案例）在本轮证据中缺失，需回原文 Results 确认。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：第一，能力组合维度的扩展——除了编排与验证，还可以引入记忆管理、规划预算分配、冲突证据裁决等新的组合能力，使执行组织能力从两个模块扩展为更完整的‘执行操作系统’；第二，组合能力的评测方法论——目前论文强调执行组织与验证是显式训练目标，但如何定量衡量‘组织得好’与‘验证得及时’，需要设计过程级指标（如子代理利用率、无效搜索占比、验证触发时机与最终正确率的关系）而不只是结果级指标；第三，去标识化与信息合并的机制研究——证据显示分支描述与证据合并时会掩码专名和识别细节，可以进一步探索掩码策略对推理质量的影响，以及如何在保护隐私的同时保留分支间的引用一致性；第四，训练轨迹的反馈回路——目前外部监督仅用于构建训练数据，可以探索在线强化学习、推理时验证（inference-time verification）和自生成验证信号，减少对人工构造验证轨迹的依赖；第五，从科学探索任务出发，可以研究多智能体编排在真实科研工作流（文献综述、假设生成、实验设计、结果复核）中的长尾能力，以及 16K-128K token 长上下文与 24 类细粒度任务之外的更长时间线（跨天、跨会话）任务；第六，与小模型蒸馏和大模型协作的交叉——35B MoE 的能力组合框架是否可以蒸馏到更小模型，或作为大模型的编排层。

Q7: 总结一下论文的主要内容

Sage-A1.5 是继 Agents-A1 之后的一篇智能体模型能力扩展论文，其核心主张是：在‘能力覆盖’（模型能解决哪些领域任务）之外，还存在‘能力组合’（问题解决能力如何在执行中被组织、协调和自适应调整）这一正交维度，而后者同样可以成为模型训练与评测的显式目标。论文从长期执行的经验教训出发——更长的执行并不总是带来更好的结果，代理会重复无产出的搜索、依赖未经验证的假设、过早接受结论——指出仅增加领域技能或仅加长执行时程都不够，需要把任务委派、并行协作、证据复核和策略调整这些执行组织能力直接训练进模型。为此，Sage-A1.5 引入两大能力模块：Agentic Orchestration 负责工作如何分解、委派和整合，形成以共享答案为根的星形树图结构，主智能体把不同约束并行委派给独立上下文的并发子代理，再将分支描述与证据（经专名掩码）合并回主问题；Agentic Verification 负责确定何时重新评估中间判断与探索策略，验证反馈直接写入模型推理文本，使模型学习何时验证、验证什么以及验证结果如何影响后续动作，外部监督仅用于构造训练数据。训练采用扩展多轮 SFT，配合代码沙箱隔离执行、工具结果回放、可复现执行机制和 Search/Visit/Scholar 等工具，覆盖 coding 与 long-context 等多领域数据，任务构建横跨 LongBlocks 长文档场景（16K-128K token、24 个细粒度类别）与 SWE-rebench-V2 及内部构建的软件工程场景。论文通过基准评测与科学案例研究展示从孤立任务求解到长时程科学研究的转变，实验评估侧重高/中难度推理与研究类任务。总体上，Sage-A1.5 把‘执行组织’确立为与‘问题求解’并列的训练维度，为小规模模型实现科学探索级长时程行为提供了一条不依赖万亿参数规模的路径。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与你画像中的智能体方向（权重 0.10）直接重合：其核心是 agentic orchestration 与 agentic verification，属于长时程智能体执行组织的前沿问题。

## 基本信息

- 作者：Wenjie Lou, Tianshuo Peng, Shiyang Feng, Shengji Tang, Runmin Ma, Ruizhe Chen, Jinxin Shi, Haoyang Peng, Fangchen Yu, Zhijie Zhong, Yuhao Zhou, Yue Fan, Yixin Chen, Yiming Wu, Yiheng Wang, Yigeng Jiang, Yichen Jiang, Yi Xie, Xu Guo, Xiaohan He, Tao Hu, Songtao Huang, Siyu Zhou, Shuaiyu Zhang, Shangheng Du, Qinglong Cao, Kun Li, Jingqi Ye, Hongbin Zhou, Guohang Yan, Zongsheng Cao, Ziyi Wang, Zhuo Liu, Zhiyin Yu, Zhiwang Zhou, Zelin Tan, Yuqing Zhu, Yaoze Zhang, Yang Chen, Xuming He, Xiuliang Liang, Xingjian Guo, Xiang Zhuang, Weiping Shen, Weida Wang, Tianyuan Zhou, Tianlin Ye, Tengyuan Fan, Siyu Yan, Shuo Li, Shuchen Pu, Shengyuan Xu, Shao Zhang, Ran Wei, Qinyuan Qin, Qingyu Yang, Nianchen Deng, Meng Dai, Kuikun Liu, Kaitao Chen, Juncheng Xiao, Jiangning Liu, Jianbiao Mei, Jia Yu, Haonan He, Hangfan Zhang, Cheng Yang, Chen Zhang, Bihao Zhan, Benteng Cheng, Yuqiang Li, Yihao Liu, Yang Yang, Xujun Fu, Xiaosong Wang, Xiangyu Yue, Wenwei Zhang, Shuyue Hu, Pinlong Cai, Mianxin Liu, Lei Bai, Jie Zhou, Jie Hou, Jiamin Wu, Hui Wang, Haijun Li, Guangyu Wang, Dongzhan Zhou, Chunfeng Song, Chao Yang, Biqing Qi, Xing Xie, Dahua Lin, Bowen Zhou, Xiangchao Yan, Wenlong Zhang, Shufei Zhang, Peng Ye, Bo Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-10-21
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.28578`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据，但证据主要命中 Introduction 与 Method，Results/Conclusion 证据不足，关键结果数值未复述，局限与实验观察部分已标注推断属性。
