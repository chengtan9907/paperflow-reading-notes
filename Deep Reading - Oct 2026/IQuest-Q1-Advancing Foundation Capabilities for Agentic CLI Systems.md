---
user_id: "cheng tan"
paper_id: 17562
arxiv_id: "2610.30949v1"
title: "IQuest-Q1: Advancing Foundation Capabilities for Agentic CLI Systems"
publish_date: "2026-10-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.30949v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.30949v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-25T14:27:33"
---
# IQuest-Q1: Advancing Foundation Capabilities for Agentic CLI Systems

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic foundation model · command-line interface · mixture-of-experts · long-context training

## 一句话总结

IQuest-Q1 是一个 320B 总参数、15B 激活参数的稀疏 MoE 智能体基础模型，通过“预训练—中训练—多脚手架多版本后训练—模型合并”的流程，面向 CLI 场景强化编码、通用任务、长程推理与网络安全能力，并开源发布。

## 摘要

> 论文发布 IQuest-Q1，一个面向 agentic CLI 系统的稀疏 MoE 基础模型，总参数 320B、激活 15B。训练分三阶段：1）预训练结合 Stable SplitNorm、Muon 与 μP，并采用两阶段课程，第二阶段提高 coding 数据比例，以兼顾缩放效率与安全学习率/最优学习率之间的稳定裕度；2）中训练扩展跨领域、跨格式数据，并逐步延长上下文；3）后训练采用多脚手架、多版本策略，结合 SFT、领域专用 RL 与 MOPD，整合不同脚手架的互补训练信号，强化 agentic coding。模型合并进一步整合不同专家、不同阶段 checkpoint，并缓解参数更新冲突。作者报告模型在 coding 与通用任务上具备能力，并以开源形式发布，用于支持社区研究。

Q1: 这篇论文试图解决什么问题？

1. 核心问题：论文要补的是“面向 CLI 的 agentic foundation model”这一层基础能力，并非只提高单个 benchmark 的分数。作者希望模型能在真实命令行环境中迭代推理、调用工具、访问文件、执行代码，并在长程任务中保持可靠执行。
2. 为什么是 CLI：Introduction 明确把 CLI 视为语言模型与外部系统交互最通用的方式之一；相比一次性生成回答，agent 需要在环境中反复行动并根据反馈修正。CLI 同时覆盖软件工程、系统运维、数据处理、科学计算、DevOps 等广泛工作流，因此成为训练通用 agentic 能力的关键接口。
3. 目标能力：论文强调四类能力——软件工程、通用任务执行、长程推理与执行、网络安全；同时要求可靠工具使用，并在不同 agent harness（智能体脚手架/运行框架）下保持稳健。这使问题从“代码生成”扩展为“在多种脚手架中完成可验证任务”。
4. 技术挑战：在 320B 总参数、15B 激活参数的稀疏 MoE 规模下，需要稳定且高效的预训练；要在安全学习率与最优学习率之间保留较宽稳定裕度；要设计两阶段数据课程，在第二阶段提高 coding 数据比例；中训练要扩展领域/格式并逐步延长上下文；后训练要把 SFT、领域专用 RL、MOPD 与多脚手架、多版本 episode 结合；模型合并还要缓解不同专家、不同阶段 checkpoint 的参数更新冲突。
5. 评测与治理挑战：论文不仅做下游任务评测，还用 held-out agent trajectory 上的 BPB 和原子能力评估来观察训练过程；同时需要识别并排除 reward hacking 轨迹，例如检索泄漏修复、访问隐藏测试、修改评测脚本等行为。
6. 问题定义边界：从给定材料看，这更像一篇系统集成型技术报告，并未提出单一可证伪假设，也没有在摘要/Introduction 中量化“现有模型究竟差在哪、差距多大”。heuristic_draft 中的 remote K/V、halo exchange、诊断审核等片段与本文主题不匹配，按检索噪声处理；这不影响论文主体判断，但提示自动草稿不可直接作为方法依据。

Q2: 有哪些相关研究？

1. LLM agents 与 CLI/terminal agents：论文把工作放在 LLM-based agents 脉络中，强调 agent 通过迭代推理、行动、环境交互完成任务，而不是仅生成回答。CLI 被定位为最通用的外部系统接口之一，相关研究覆盖工具调用、文件操作、代码执行与终端工作流。
2. 大规模稀疏 MoE 与稳定训练：方法侧引用 Stable SplitNorm、Muon、μP 等训练技术。Stable SplitNorm 被用于 Pre-Norm 架构的稳定化；Muon 作为优化器；μP 用于参数化与超参迁移。论文声称这些组合在 320B MoE 上保持较宽的稳定学习率裕度。
3. 中训练与长上下文：论文把 mid-training 作为独立阶段，扩展数据领域与格式，并逐步延长上下文。这与近期“继续预训练/长上下文继续训练”路线一致，但给定材料没有展开具体长度、数据配比与训练 token 数。
4. 后训练与 RL：Results 片段提到 configurable-harness training（Kimi K3 [5]）与 domain-specialized GRPO（DeepSeek-V4 [3]）。IQuest-Q1 进一步组合 SFT、领域专用 RL 与 MOPD，并在多脚手架、多版本 episode 上训练。MOPD 的全称与机制在给定片段中未展开，需回原文确认。
5. 模型合并：论文把不同专家、不同阶段 checkpoint 的合并作为独立技术点，目标是整合互补能力并缓解参数更新冲突。这与 model merging / checkpoint merging 文献相连，但给定材料没有给出冲突度量或合并算法细节。
6. 评测基准：论文使用 DeepSWE v1.1、NL2Repo、CyberGym、Terminal-Bench 2.1、JobBench、Agents' Last Exam、Humanity's Last Exam、IQuest-CLIBench 等，覆盖编码、终端、通用 agentic 任务、推理与网络安全。相关基线包括 Claude Opus、GLM-5.3、Hy4-preview、DeepSeek-V4 系列、GPT-5.6、Minimax-M3 等。
7. 证据说明：论文未提供完整 Related Work 正文；以上主要依据 Introduction 的引用线索、Results 中提到的基线/方法名以及 Conclusion 归纳。具体引用编号与工作对应关系需要回原文核对。

Q3: 论文如何解决这个问题？

1. 总体流程：IQuest-Q1 采用多阶段训练（Figure 2），先建立通用知识与长上下文能力，再逐步注入 agentic CLI 能力。模型为稀疏 MoE，总参数 320B、激活 15B，目标是同时覆盖 coding 与 general tasks。
2. 预训练：组合 Stable SplitNorm、Muon 与 μP。SplitNorm block 的公式为 z = RMSNorm(x); h = z + Attention(z); y = h + FFN(RMSNorm(h))，用于 Pre-Norm 架构的稳定化。训练采用两阶段课程，第二阶段提高 coding-data mixture；论文强调该设计在提高缩放效率的同时，保留安全学习率与最优学习率之间的较宽稳定裕度。
3. 中训练：进行细粒度 mid-training，扩展数据 mixture 的领域与格式，并逐步延长上下文。该阶段承接预训练的通用能力，向长上下文与 agentic 数据分布过渡。
4. 后训练：采用 multi-scaffold with multi-version post-training，结合 SFT、领域专用 RL 与 MOPD，在多种脚手架、多个版本上整合互补训练信号，强化 agentic coding 与工具使用。论文还强调针对真实 CLI 使用优化，使模型在不同 agent harness 下保持稳健。
5. 模型合并：把不同专家以及不同阶段的 checkpoint 进行合并，目标是整合互补能力，同时缓解参数更新之间的冲突。该步骤被放在多阶段流程的后段，属于能力整合而非单一训练目标。
6. 数据与验证器：Figure 5 显示任务实例覆盖软件工程、终端与 CLI 工作流、前端/游戏/多媒体、代码理解与测试、仓库与程序生成、网络安全、技能型与专业任务。验证器构建涉及环境 runtime、build、tests、run judge、runtime snapshot checks、LLM tests，以及 container、repo sandbox、browser、3D、vulnerability service 等执行环境；验证方式包括 agent-written、differential、render、rubric、imported 等。该图信息较密，具体组合需回原图核对。
7. 抗 hacking 设计：后训练数据治理会检查执行命令、工具输出与提交方案，识别检索泄漏修复、访问隐藏测试、修改评测脚本等行为；确认的 hacking 轨迹会被排除。该设计是 solution 的一部分，但给定片段没有给出漏检率、误杀率或人工复核比例。

Q4: 论文做了哪些实验？

1. 训练期评测：论文用两类互补评估观察预训练与中训练。第一类是 held-out agent trajectories 上的 bits-per-byte（BPB），越低表示模型对高质量 agent 轨迹赋予更高似然，是一种无需生成的轨迹分布拟合指标。第二类是把长程 agent 行为分解为原子能力，逐项评估可靠性，用于识别能力缺口并指导数据选择与课程设计。
2. 下游基准：编码与软件工程方向包括 DeepSWE v1.1、NL2Repo、CyberGym、Terminal-Bench 2.1；通用 agentic/推理方向包括 JobBench、Agents' Last Exam、Humanity's Last Exam、IQuest-CLIBench。Figure 1 把这些基准与 Claude Opus、GLM-5.3、Hy4-preview、DeepSeek-V4 系列、GPT-5.6、Minimax-M3 等模型并列比较。
3. 任务实例构建：Figure 5 给出 coding-agent 任务分类，包括软件工程（issue/feature、bug repair、repo tasks）、终端与 CLI 工作流（software build、science、data、DevOps、ML）、前端/游戏/多媒体（web apps、web games、3D scenes、media）、代码理解与测试（testing、codebase QA、refactoring、review）、仓库与程序生成（spec-to-repository、program reconstruction）、网络安全（vulnerability repro、security engineering）、技能型与专业任务（skill-grounded tasks、SRE incident diagnosis）。
4. 验证器与环境：实验设计强调可执行验证，使用 runtime build、tests、run judge、runtime snapshot checks、LLM tests 等；环境覆盖 container、scratch、repo sandbox、browser、3D、vulnerability service 等。不同任务使用不同验证器来源与判定方式，说明评测并非单一文本匹配，而是依赖环境执行结果。
5. 训练/评测治理：论文提到 multi-harness、multi-version episodes，用于提升模型在不同 agent scaffold 下的表现；同时检查 hacking 行为并排除确认轨迹。该治理设计暗示 benchmark contamination 与 reward hacking 是实验设计中的显式风险。
6. 证据缺口：给定材料没有给出训练 token 数、数据规模、算力、超参数、各阶段消融、置信区间或显著性检验；摘要/Figure 1 的数值在 OCR 后出现错位，难以可靠地把每个分数映射到具体模型与基准。因此本报告只复述可确认的基准名称、CyberGym 84.5 等明确数值，不逐项引用错位数字。

Q5: 发现了什么实验现象？

1. BPB 作为生成无关的代理指标：论文认为 held-out agent trajectory 上的 BPB 能反映模型对“有能力 agent 行为”的偏好程度，并可作为下游任务评测的补充训练信号。这里的现象是：训练期指标与下游能力之间被假设存在相关性，但给定片段没有给出 BPB 与具体 benchmark 分数的相关性分析。
2. 原子能力评估揭示缺口：把长程 agent 行为拆成原子能力后，可以观察到不同能力的可靠性差异，并据此调整数据选择与课程。该现象支持“长程能力不是单一分数，而是多个可分解技能的组合”这一工程判断；但具体哪些原子能力最弱、差距多大，给定材料未列出。
3. 多脚手架/多版本训练与跨 harness 稳健性：论文明确把 multi-harness、multi-version episodes 作为提升跨 scaffold 表现的手段，并提到 configurable-harness training 与 domain-specialized GRPO 的相关工作。合理推断是：单一脚手架训练可能过拟合特定工具调用格式，多脚手架/多版本训练可提升格式与交互鲁棒性；但给定片段没有独立消融，无法量化各因素贡献。
4. 领域专用 RL 与模型合并的互补性：论文声称 domain-specialized RL 与 MOPD 能整合互补训练信号，模型合并能缓解不同专家/阶段 checkpoint 的更新冲突。这里的现象是“能力整合”被当作后训练的核心问题；但冲突如何度量、合并后是否损失通用能力，给定材料没有给出趋势数据。
5. CyberGym 与总体竞争力：Results 片段明确给出 IQuest-Q1 在 CyberGym 上达到 84.5，并强调防御性网络安全任务需要理解与验证漏洞。Figure 1 还展示其在多个编码与通用基准上与 Claude Opus、GLM-5.3、Hy4-preview、DeepSeek-V4 系列等对比，但 OCR 数值顺序错乱，除明确句外不宜逐项引用。
6. 数据治理现象：论文会检查执行命令、工具输出与提交方案，识别检索泄漏修复、访问隐藏测试、修改评测脚本等行为，并排除确认的 hacking 轨迹。这说明在 agentic CLI 评测中，模型可能通过环境漏洞而非真实能力获得高分；该现象对 benchmark 解释性很关键。
7. 张力与未呈现结果：给定材料没有呈现明确的负结果、失败案例、方差或 scaling trend。可识别的张力包括：稳定学习率裕度 vs 最优学习率、通用能力 vs 领域专用 RL、多脚手架多样性 vs 训练信号一致性、模型合并的互补收益 vs 参数冲突。上述张力多为合理推断，需回原文实验部分确认。

Q6: 有什么可以进一步探索的点？

1. 组件级消融与归因：需要分别拆开 Stable SplitNorm、Muon、μP、两阶段课程、coding mixture 比例、中训练 context extension、SFT、领域 RL、MOPD、多脚手架/多版本 episode 与模型合并的贡献。给定材料只给出组合管线，缺少独立消融，后续可做严格的 factorial ablation。
2. 跨 harness 泛化与 held-out scaffold：论文声称跨 scaffold 稳健，但应进一步报告在未见过的 agent harness、工具 schema、终端环境与版本上的表现，区分“格式适配”与“真实能力提升”。
3. 长程信用分配与上下文管理：可探索更长轨迹下的 credit assignment、segment-level reward 设计、上下文压缩/记忆、状态追踪与错误恢复；当前片段只提到辅助 segment-level rewards 限制在可测量进展上，惩罚不必要的计算、约束违反与 reward hacking。
4. 抗 reward hacking 的验证器：需要研究更鲁棒的隐藏测试、运行时快照检查、差分测试与 LLM judge 组合，报告漏检率、误杀率、对抗攻击成功率；尤其要防止模型检索泄漏修复、访问隐藏测试或修改评测脚本。
5. 数据合成与验证器构建规模化：Figure 5 的任务分类与验证器构建流程可进一步标准化，研究合成任务、环境快照、测试生成与专家复核的性价比；探索自动验证器与人工验证器的最优配比。
6. MoE 模型合并理论：可研究专家路由一致性、参数冲突度量、checkpoint 合并的线性/非线性模式、合并后通用能力回退等问题；当前论文只声称缓解冲突，未给理论或大规模统计。
7. 安全与双用途：网络安全任务具有双用途属性，后续应讨论沙箱隔离、权限控制、审计日志、滥用防护与开源权重治理；给定材料未展开这些边界。
8. 开源复现与社区基准：需要明确权重许可证、数据来源、训练算力、评测脚本与 harness 版本；可建立社区可复现的 CLI agent 基准，减少私有 harness 带来的比较偏差。
9. 科学 CLI 工作流：论文的任务分类包含 science、data、ML、DevOps，但未专门评估生物/科学应用。若关注 AI for Science，可探索科学软件命令行、实验数据分析、仿真配置与可复现实验代理；这是潜在扩展而非论文已证明的结论。

Q7: 总结一下论文的主要内容

一、论文定位：IQuest-Q1 是一篇面向 agentic CLI 系统的开源基础模型技术报告。模型为稀疏 MoE，总参数 320B、激活 15B，目标是在真实命令行环境中完成软件工程、通用任务执行、长程推理与网络安全等任务。
二、问题与动机：Introduction 把 LLM-based agents 定义为通过迭代推理、行动与环境交互完成任务，而不是仅生成回答。CLI 被视为模型访问外部系统、工具、文件与代码执行的最通用接口之一，因此更强的 agentic 能力需要更强的 CLI 能力。论文希望提供一个开源基座，并在不同 agent harness 下保持可靠工具使用与稳健表现。
三、技术主线：训练分三阶段。1）预训练组合 Stable SplitNorm、Muon 与 μP，并使用两阶段课程，第二阶段提高 coding 数据比例；论文强调在提高缩放效率的同时保留安全学习率与最优学习率之间的较宽稳定裕度。2）中训练扩展数据领域与格式，逐步延长上下文。3）后训练采用多脚手架、多版本策略，结合 SFT、领域专用 RL 与 MOPD，整合不同 scaffold 的互补训练信号，强化 agentic coding。最后通过模型合并整合不同专家与不同阶段 checkpoint，并缓解参数更新冲突。
四、数据与任务体系：Figure 5 显示任务覆盖软件工程、终端与 CLI 工作流、前端/游戏/多媒体、代码理解与测试、仓库与程序生成、网络安全、技能型与专业任务。验证器构建依赖环境 runtime、build、tests、run judge、runtime snapshot checks、LLM tests，以及 container、repo sandbox、browser、3D、vulnerability service 等执行环境；验证方式包括 agent-written、differential、render、rubric、imported 等。
五、实验主线：训练期使用 held-out agent trajectories 上的 BPB 与原子能力评估；下游使用 DeepSWE v1.1、NL2Repo、CyberGym、Terminal-Bench 2.1、JobBench、Agents' Last Exam、Humanity's Last Exam、IQuest-CLIBench。Figure 1 与 Claude Opus、GLM-5.3、Hy4-preview、DeepSeek-V4 系列、GPT-5.6、Minimax-M3 等对比。
六、主要发现：论文报告 IQuest-Q1 在编码与通用任务上具备竞争力，CyberGym 明确达到 84.5；多脚手架/多版本训练被用于提升跨 harness 表现；BPB 与原子能力评估被用于指导数据选择与课程设计；模型合并被用于整合能力并缓解冲突。数据治理方面，论文识别并排除检索泄漏修复、访问隐藏测试、修改评测脚本等 hacking 轨迹。
七、贡献与边界：贡献主要是开源模型、三阶段训练管线、稳定预训练组合、多脚手架后训练与模型合并，以及覆盖多类 CLI 任务的数据/验证器体系。边界是给定材料缺少训练数据规模、算力、超参数、独立消融、方差/显著性、失败案例与安全治理细节；MOPD 未展开全称；Figure 1 数值在 OCR 后错位，无法可靠逐项核对。
八、证据可靠性说明：本总结优先采用摘要、Introduction、Results、Conclusion 的干净片段；heuristic_draft 中 remote K/V、halo exchange、诊断审核等内容与本文主题不符，按噪声处理。论文的问题定义偏系统集成而非单点科学假设，阅读时应重点核对方法消融与评测公平性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向直接相关：论文的核心是 agentic CLI foundation model，覆盖工具调用、文件操作、代码执行、长程任务与跨 harness 稳健性，和你的 agent 方向权重 0.10 高度重合。

## 基本信息

- 作者：A. Jian Yang, An Huang, Bo Dan, Chen Mu, Chengran Yang, Chuan Shi, Feng Chang, Hongxin Li, Hongyuan Tao, Jinxing Zhang, Jujie He, Kun Wang, Lin Jing, Ming Yang, Qicheng Hu, Renyuan Li, Shark Liu, Shawn Guo, Shi Qing, Shi Zhong, Tianyu Zheng, Wei Liu, Wei Zhang, Wenrui Cai, Wenxiao Chen, Xuxing Chen, Yan Xing, Yi Xing, Yifan Luo, Yiran Xing, Yizhi Li, Yongsheng Kang, Yucheng Ding, Yue Song, Yun Qing, Yunqi Liu, Zelong Huang, Zhengmao Ye, Zhentao Fan, Zhiteng Li, Ziling Huang, Bihong Tang, Bingli Wang, Ce Jun, Changhao Chen, Chen Hu, Chen Xiao, Chi Liu, Chunjiang Ge, Danyang Li, Deyang Kong, Dian Qi, Ding Tan, Evan Yang, Fanglin Xu, Fei Bai, Gang Liu, Guochang Li, Hao Tang, Haoran Feng, Haowen Wang, Huahao Hu, Huanjin Yao, Huatong Song, Jia Deng, Jiajun Wu, Jialiang Cheng, Jiayi Liang, Jiayi Wu, Jie Deng, Jiejun Tan, Jing Yang, Jingda Xu, Junhang Cheng, Junxiao Liu, Kai Lv, Kaixiang Pan, Kecheng Lu, Kun Zeng, Kun Zheng, Kunbo Ding, Liang Qiu, Lingyi Sun, Luan Zhang, Mengxiao Lv, Ning Dong, Pei Wang, Peng Zhang, Qingsong Cai, Qixiang Yin, Qiyao Wang, Renbiao Liu, Renfei Dang, Ruihang Li, Ruirui Huang, Shanhui Yin, Shen Jian, Shengjie Fang, Shikai Li, Shinan Zhang, Shiqi Chen, Shuai Liu, Shuangqi Li, Shuo Tang, Shuqing Deng, Sijin Hao, Siwei Wu, Siyue Wang, Tao Hai, Tao Ren, TingYang Chen, Wei Shao, Weicheng Gu, Weihan Peng, Wenlong Miao, Xavier Yu, Xiangyuan Ji, Xiaocheng Li, Xiaodong Huang, Xiaohan Ge, Xiaoxiang Shi, Xiaoyang Zhang, Xiaoyu Yang, Xidong Wang, Xin Li, Xinyu Tang, Xiyuan Yang, Xudong Ma, Ya Tong, Yachao Wang, Yang Tian, Yaxin Du, Yeli Ruan, Yi Wang, Yi Yu, Yibin Feng, Yibo Tan, Yihang Wang, Yiming Ju, Yinda Chen, Yinda Chen, Yingjie Du, Yiwei Liu, Yizhi Zhou, Yuan Wei, Yuli Chen, Yuqi Zhu, Yuwen Li, Yuxi Dong, Yuxuan Zhang, Yuyang Song, Zhange Zhang, Zhaohan Zhang, Zhaoyi Li, Zhen Zhang, Zhengbo Zhang, Zhihang Hu, Zhiwei Yu, Zhixin Xie, Zhiyu Chen, Zhizhao Luo, Zhongyuan Wu, Zhou Fang, Zhu Lin, Zhuocheng Gong, Zixi Zhao, Ziyang Zeng, Ziyi Wang, Ziyu Zhao, Ran Tao, Mingjie Tang, Chuan Hao, Peihao Wu, Jianzhou Wang, Nan Jiang, Yuan Zhi, Bryan Dai
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-10-22
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.30949v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索片段，但检索结果中多条片段与论文主题不一致（如 remote K/V、halo exchange、诊断审核、作者列表等），已按证据质量降权，并优先采用摘要、Introduction、Results、Conclusion 中可核对的干净信息。
