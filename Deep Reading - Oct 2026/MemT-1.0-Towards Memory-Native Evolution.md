---
user_id: "cheng tan"
paper_id: 16578
arxiv_id: "2610.26825v1"
title: "MemT-1.0: Towards Memory-Native Evolution"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.26825v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.26825v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:08:34"
---
# MemT-1.0: Towards Memory-Native Evolution

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：memory foundation model · memory-native evolution · gated deltanet · mixture-of-storage

## 一句话总结

MemT-1.0 提出一种将记忆原生内化为模型计算组成部分的记忆基础模型架构，基于 Gated DeltaNet-2 的稳定 token 级记忆更新与 Mixture-of-Storage 扩展原生记忆容量，通过两阶段训练管线发布 36B（3B 激活）Nano 与 288B（17B 激活）Flash 两个模型，在记忆评测上平均优于参数记忆基线、与检索增强方法相当，并基本保持通用能力。

## 摘要

> MemT-1.0 是构建在 Metis 基础上的新一代记忆基础模型架构，目标是把记忆内化为基础模型的原生能力（memory-native evolution），使模型能够持续积累和利用信息，而不是依赖外部上下文或检索系统。架构上，MemT-1.0 采用 Gated DeltaNet-2（GDN-2）实现每轮内稳定的 token 级记忆更新机制，并引入 Mixture-of-Storage（MoS）来扩展原生记忆容量。训练采用两阶段管线：先做 continued pre-training（CPT）建立带原生记忆的语言建模能力，再做 post-training 对齐与能力保持；同时配有高效的训练和推理基础设施。论文发布 MemT-1.0-Nano（36B 参数、3B 激活、10M 动态记忆）和 MemT-1.0-Flash（288B 参数、17B 激活、336M 动态记忆）两个模型，在 11 个记忆基准、6 个通用能力基准上做了全面评测。结果表明：平均优于参数记忆（parametric memory）基线，与检索增强方法（retrieval-augmented）相当，同时大体保持通用能力。在 128K 历史长度下，Nano 相对全量上下文 KV cache 取得约 1.86× 端到端推理加速和 66× 持久存储缩减。通过把检索到的程序性经验内化，MemT-1.0 能提升智能体任务的成功率，是迈向记忆原生进化（memory-native evolution）的实际一步。

Q1: 这篇论文试图解决什么问题？

这篇论文要解决的核心问题是：如何让记忆成为基础模型的原生计算能力，而不是依赖外部上下文窗口、检索系统或显式外部存储。作者把这条路线称为 memory-native evolution，其核心矛盾有三层。第一层是记忆与参数学习的对立：当前主流大模型把历史信息放在 context window 里，长度受限且推理成本随上下文线性增长；检索增强（RAG）虽然能扩展可访问信息，但引入外部系统依赖，检索质量成为瓶颈，且无法把经验内化为模型自身的行为习惯。第二层是记忆更新的稳定性问题：此前 Metis 用 token selection 机制决定哪些 token 进入记忆，选择噪声大、更新不稳定，容易丢失或污染关键信息；论文因此要设计一个每轮内稳定、可控的 token 级记忆更新机制。第三层是记忆容量的扩展问题：把记忆直接写入模型状态后，容量受模型结构限制，需要一种类似 MoE 的存储扩展方案（Mixture-of-Storage）把原生记忆容量做大。此外，训练范式也是问题：一般预训练目标不提供显式监督告诉模型“什么值得记住”，怎样通过两阶段训练（CPT + post-training）让模型学会存储、恢复和利用记忆，同时不损伤通用能力，是论文要解决的工程与算法问题。论文隐含的更大命题是：持续的自主经验积累是智能体进化的前提，只有记忆内化才能支撑跨任务、跨时间尺度的自我进化。

Q2: 有哪些相关研究？

相关研究可以从几条线索梳理。第一条是记忆基础模型：作者此前提出的 Metis 首次把记忆内化为基础模型原生能力，结构上采用动态局部记忆（dynamic local memory）加可训练超记忆（trainable hyper memory），但 token 选择机制不稳定；MemT-1.0 是对它的架构升级。第二条是线性注意力/循环状态空间模型路线：论文明确基于 Gated DeltaNet-2（GDN-2），这类模型把历史信息压缩进固定大小的循环状态，代表包括 DeltaNet 系列、Mamba、RetNet 等，记忆容量受状态维度限制，且更新规则简单、缺乏任务层面的可控性。第三条是参数记忆（parametric memory）基线：这类工作把记忆状态也做成可训练参数，代表如 MemoryLLM、δ-mem、SHINE、Doc-to-LoRA 等，MemT-1.0 在实验中和它们对比。第四条是检索增强记忆：DenseRAG、Graphiti 等把历史信息索引进外部向量库或图结构，查询时检索相关片段拼入上下文；MemT-1.0 要和这类方法对比，并声称在平均表现上相当。第五条是智能体记忆与经验复用：MemoryAgentBench、ExperienceBank 等关注智能体从历史轨迹中提炼程序性经验；论文用自建的 Experience-Guided Agentic Framework 来验证记忆内化对长程任务执行的价值。第六条是长上下文与 KV cache 压缩：论文的 66× 持久存储缩减和 1.86× 推理加速对照对象是全量上下文 KV cache，说明它也被当作一种上下文压缩/推理加速方案来定位。第七条是记忆评测体系：LoCoMo、MemoryAgentBench、ImplicitMemBench、PersonaMem、PS-Bench、MemBench、LongMemEval-S、BEAM-1M 等覆盖从短轨迹到百万级长轨迹的记忆测试，论文把 11 个记忆基准按轨迹长度分四组做系统评测。第八条是记忆训练中的“什么值得记”监督问题：部分工作用摘要、续写、压缩等任务为记忆存储提供显式监督（检索证据显示 Introduction 提到这类 supervision），MemT-1.0 的训练也包含教师模型利用完整文本历史指导记忆状态更新的设计。

Q3: 论文如何解决这个问题？

MemT-1.0 的解决方案由四个相互咬合的部分组成。第一是保留 Metis 的双层记忆结构：动态本地记忆（dynamic local memory）作为工作记忆/短期存储，可训练的 hyper memory 作为长期知识存储，但把 token selection 替换为基于 Gated DeltaNet-2（GDN-2）的稳定 token 级记忆更新机制——每一轮对话内，记忆状态以 token 为粒度被确定性、可微地更新，减少了选择噪声，使“哪些信息进入记忆”变得稳定可控。第二是 Mixture-of-Storage（MoS）：借鉴 MoE 的思路把原生记忆容量扩展，让记忆状态分布在多个存储槽（slot）上，而不是压进单一状态向量；检索证据显示历史信息通过 commit 操作进入 slot，commit 读取输入轮次并把内容写入 slot，这相当于把记忆写入做成显式的存储操作，而不是隐式的状态压缩。第三是深思熟虑的 backbone 准备：不是随便拿一个预训练模型硬加记忆模块，而是先做 backbone preparation，让模型结构和记忆机制匹配。第四是两阶段训练管线：第一阶段 continued pre-training（CPT）用带显式监督的任务（摘要、续写、压缩等）教模型“什么该存、怎么存”，训练模型从记忆状态中恢复和利用已存信息（检索证据明确提到这类训练目标）；第二阶段 post-training 做对齐和能力保持，确保加入原生记忆后不损伤通用能力；训练还配套了高效的训练和推理基础设施。在智能体场景，论文把程序性经验总结成 reusable procedural guidance 存入 experience bank，MemT-1.0 在长程任务执行中存储并复用这些经验。整体设计意图是把记忆从“外部挂载”变成“模型计算的一部分”，让检索到的经验能够内化为模型自身的程序性知识。

Q4: 论文做了哪些实验？

论文实验分两大块：记忆能力评测和通用能力评测，另有智能体应用实验和效率实验。记忆能力方面，用 11 个基准按轨迹长度分四组：16K 以下组包括 MemBench、ImplicitMemBench；16K-32K 组包括 PersonaMem、PS-Bench、LoCoMo；32K-64K 组包括 PerMemSafe、Memora、MemOps；64K 以上组包括 LongMemEval-S、MemoryAgentBench、BEAM-1M。这些基准覆盖长期信息保持与检索、动态记忆更新与管理、个性化理解等能力。评测设置强调“基于动态记忆状态回答问题，不访问先前的文本上下文”，即真正的闭卷记忆测试。通用能力方面，用 6 个基准覆盖四个维度：MMLU-Pro 和 GPQA Diamond（知识与科学推理）、IFEval（指令遵循）、LongBench v2（长上下文理解）、BigCodeBench-Hard 和 LiveCodeBench v6（代码生成）。对比基线包括参数记忆类（MemoryLLM、δ-mem、SHINE、Doc-to-LoRA 等）和检索增强类（DenseRAG、Graphiti 等），以及对应的 backbone。智能体应用方面，论文构造 Experience-Guided Agentic Framework：把历史智能体轨迹总结成可复用的程序性指导，构建 experience bank；对每一条成功轨迹识别关键执行决策及其环境条件，把条件相似的轨迹分组归纳（论文片段提到“experience generation with quality auditing”）。效率实验报告了 128K 历史长度下 Nano 相对全量上下文 KV cache 的 1.86× 端到端推理加速和 66× 持久存储缩减；论文还提到噪声设置（noise setting）下的通用能力对比。

Q5: 发现了什么实验现象？

从摘要、Introduction 和 Results 片段可以归纳以下实验现象。第一，MemT-1.0 平均优于参数记忆基线：在 LoCoMo（LCM）、MemoryAgentBench（MAB）、ImplicitMemBench（IM）等基准上，MemT-1.0-Flash 和 Nano 的分数高于 MemoryLLM、δ-mem、SHINE、Doc-to-LoRA 等参数记忆方法——例如摘要图注可见 LoCoMo 上 85.7 vs 83.4 之类的优势（具体数值以论文图表为准）。第二，MemT-1.0 与检索增强方法（DenseRAG、Graphiti）表现相当：这说明把记忆内化到模型计算中可以在不依赖外部检索的情况下达到接近检索系统的记忆效果，代价是节省了检索延迟和外部存储。第三，通用能力大体保持：Results 片段提到相对对应 backbone 在噪声设置下逐点比较，绿色表示下降、红色表示上升，总体结论是 broad capability preservation——说明加装原生记忆机制对通用能力没有显著伤害，但存在个别能力点的涨跌，需要区分域来看。第四，效率收益显著：128K 历史长度下 Nano 约 1.86× 端到端推理加速、66× 持久存储缩减，说明原生记忆状态把上下文压缩进了固定大小的存储槽。第五，智能体任务成功率提升：通过把检索到的程序性经验内化并复用，任务成功率提升且执行一致性更好。第六，值得注意的张力：memory-native 路线的代价是把记忆容量固定化，语义检索片段也提出“hyper memory 中的知识可以通过 local memory 实例化并适配，因此记忆能力本身可以视为一种高阶形式的学习信息”——这说明作者观察到记忆与参数知识之间存在可互换、可迁移的关系，而这也正是记忆能力评测中最难刻画的抽象性质。关于负结果和消融趋势，当前证据不足，需要回到论文全文确认。

Q6: 有什么可以进一步探索的点？

论文 Conclusion 明确列出三个未来方向：提升长期记忆（improving long-term memory）、扩展记忆容量（scaling memory capacity）、降低记忆更新成本（reducing the cost of memory updates）。在此基础上可以进一步展开的探索点包括：第一，自主经验获取（autonomous experience acquisition）：目前经验需要从成功轨迹中人工/半自动总结进入 experience bank，未来应让模型自己判断什么经验值得存、何时存，形成闭环；第二，跨任务的持续记忆整合（continual memory consolidation）：把不同任务、不同时间尺度获得的记忆碎片整合成结构化知识，避免冲突和遗忘；第三，记忆与参数知识的双向关系：Results 片段提到 hyper memory 中的知识可以通过 local memory 实例化与适配，记忆能力可视为高阶形式的学习信息——这指向一个有意思的研究问题：记忆更新与参数更新（如 fine-tuning/LoRA）之间是否存在等价性或可组合性，能否用记忆机制部分替代参数更新；第四，存储槽的容量与路由策略：MoS 目前扩展了容量，但 slot 的写入、替换、遗忘策略仍可优化，比如按重要性做记忆优先级、按时间衰减、按任务相关性做 slot 分配；第五，长轨迹与小样本记忆：BEAM-1M 这类百万级轨迹场景中记忆的压缩率、检索精度和更新一致性如何权衡，值得专门研究；第六，多模态记忆：论文属于语言模型范畴，把原生记忆扩展到视觉、音频等多模态信息是自然延伸；第七，隐私与安全：记忆内化意味着模型长期持有用户信息，PerMemSafe 这类基准表明安全评测已存在，未来的记忆更新机制需要和遗忘权、隐私保护结合；第八，推理成本优化：66× 存储缩减已很显著，但 1.86× 加速还有提升空间，可以探索记忆状态的稀疏激活、分层存储（快慢记忆）和硬件协同设计；第九，记忆评测体系的规范化：当前 11 个基准各测各的，缺一个统一的记忆能力坐标系（存储容量、更新精度、检索精度、遗忘控制、抗污染），论文的评测框架可以作为起点。

Q7: 总结一下论文的主要内容

论文的论证主线是：记忆应该被内化为基础模型的原生计算能力，而不是挂在外部系统上的附件。作者从自己先前的 Metis 工作出发——Metis 首次提出 memory foundation model 概念，用动态局部记忆加可训练 hyper memory 的结构把记忆装进模型——指出 token selection 式记忆更新不稳定，容量受限，因此需要架构升级。MemT-1.0 的技术主线包括四个要素：基于 Gated DeltaNet-2（GDN-2）的稳定 token 级记忆更新机制，使每轮内的记忆写入确定、可微、低噪声；Mixture-of-Storage（MoS）把原生记忆容量从单一状态扩展到多个存储槽，历史信息通过 commit 操作写入 slot；deliberate backbone preparation 让模型结构为记忆机制做好准备；两阶段训练管线（continued pre-training + post-training）为记忆存储提供显式监督（摘要、续写、压缩等），并保持通用能力。实验主线分三层：记忆能力（11 个基准按轨迹长度分四组，闭卷测试——只能基于动态记忆状态回答，不能访问文本上下文）、通用能力（6 个基准四维度）、智能体应用（Experience-Guided Agentic Framework 把成功轨迹总结成程序性经验存入 experience bank，模型在长程任务中复用）。发布两个模型：MemT-1.0-Nano（36B 参数、3B 激活、10M 动态记忆）和 MemT-1.0-Flash（288B 参数、17B 激活、336M 动态记忆）。核心结果：记忆能力平均优于参数记忆基线、与检索增强方法相当；通用能力在噪声设置下大体保持；128K 历史长度下 Nano 相对全量上下文 KV cache 有约 1.86× 端到端推理加速和 66× 持久存储缩减；内化程序性经验提升智能体任务成功率与执行一致性。论文收尾把未来指向长期记忆、容量扩展、更新成本下降、自主经验获取与跨任务记忆整合，最终目标是 agent 通过 memory-native evolution 自主学习。整体上这是一个“系统工作 + 架构创新 + 评测体系”三位一体的报告，路线意义大于单点技术突破。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向（权重 0.10）直接相关：论文把记忆内化作为长程智能体任务中经验积累的基础，并给出了 Experience-Guided Agentic Framework 和 MemoryAgentBench 评测

## 基本信息

- 作者：Zhiyu Li, Zeyu Zhang, Linyun Liu, Yiwei Bu, Chenyang Xi, Xichong Zhang, Yi Yu, Yu Yu, Zhichen Liu, Yuxiao Yi, Chunyu Li, Haoran Liu, Zhenkai Ding, Le Zhou, Juncheng Zhang, Boyu Chen, Zehao Lin, Guorui Li, Renyu Fu, Ji Li, Youyang Xia, Jiajun Shen, Harvey Li, Jingsong Zhou, Helan Hu, Tong Shen, Tianxiao Xu, Chenye Han, Weijie Li, Zhongquan Zhou, Hongming Xu, Yuekeng Li, Erenkai Zhu, Peng Yu, Wenhao Li, Jingyi Kang, Xixuan Hao, Xuzhou Zhu, Can Chen, Jiawei Liu, Chen Jiang, Tao Jiang, Fangwei Xiang, Zepeng Yao, Kai Chen, Guang Yang, Mingfei Lu, Chunhui Li, Zhengxuan Jiang, Bo Tang, Shiyu Zhan, Yihang Sun, Jie Chu, Wenjun Xiong, Zhenxiang Hua, Chen Lin, Detong Su, Dongyu Deng, Qiuping Ding, Guofeng Ding, Si Chen, Guohua Wen, Xu Chen, Zhi-Qin John Xu, Ying-Cong Chen, Ningyu Zhang, Haofen Wang, Muning Wen, Haotian Chen, Wentao Zhang, Xuanhe Zhou, Rita Zhang, Junchi Yan, Feiyu Xiong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.26825v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（Introduction/Results/Conclusion 片段）并结合论文摘要与元数据；部分具体实验数值取自摘要图注，需回原文核对。
