---
user_id: "cheng tan"
paper_id: 14942
arxiv_id: "2610.11959v1"
title: "MiMo-V2.6: Scaling Reinforcement Learning Towards Self-Improvement"
institution: "小米 LLM-Core 团队（Xiaomi LLM-Core Team）"
publish_date: "2026-10-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.11959v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.11959v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-12T03:30:16"
---
# MiMo-V2.6: Scaling Reinforcement Learning Towards Self-Improvement

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning · mixture-of-experts · agentic tasks · multimodal model

## 一句话总结

本报告介绍小米MiMo-V2.6系列（1.02T/42B激活的Pro版与310B/15B激活的Flash版）大规模混合稀疏MoE模型，通过沿批次规模与吞吐、环境多样性与复杂度、分级器计算三个维度规模化强化学习，并配套异步训练、混合任务rollout基础设施与防奖励黑客机制，推动基础模型向递归自我改进迈进。

## 摘要

> 强化学习被视为推动大型基础模型自我改进的核心训练范式。MiMo-V2.6系列在预训练后的混合稀疏MoE架构上进行了多模态中训练，以提供充分的探索空间，并从三个维度扩展强化学习：一是扩大批次与吞吐，采用异步训练每步消耗1,568个样本和2.7~3.7B tokens，上下文长度最高达1M；二是增加环境的多样性和复杂度，涵盖代码、通用工作流、视觉和网络安全领域，并混合多种智能体框架；三是增加分级器计算，通过组内智能体分级为长时任务提供更准确的奖励信号，同时引导模型生成更短、更具token效率的解决方案。为保证规模下的训练稳定性，作者冻结了MoE路由器，并建立了多层防御对抗奖励黑客。此外，论文还构建了支持混合任务智能体强化学习的完整基础设施，包括统一轨迹表示、高并发多框架rollout、解耦控制与数据平面、训练-推理一致性。训练动态、RL环境和框架均已开源，以促进可复现的规模化RL与模型自我改进研究。

Q1: 这篇论文试图解决什么问题？

论文试图解决如何通过大规模强化学习（RL）让基础模型实现递归自我改进（RSI）这一核心问题。具体而言，实现RSI需要智能体与环境交互，从而产生多步轨迹和反馈信号，但当前在扩展RL到复杂智能体任务时面临两大主要挑战：

1. **模型架构与探索空间不足**：RL需要模型具备合适的架构和足够丰富的探索空间，才能从交互中持续获取收益。现有模型在多模态理解、长上下文处理和复杂任务分解方面的能力还不足以支撑高效的智能体探索，因此需要先在广泛的多模态语料上进行中训练（mid-training）以建立强大的基础。

2. **基础设施、环境与分级器的规模化瓶颈**：扩展RL需要同步解决训练基础设施（如大规模异步训练、稳定性和防奖励黑客）、环境多样性（覆盖代码、通用、视觉、网络安全等多个领域）以及奖励信号准确性（长时任务需要细粒度的分级器而非二元测试奖励）等复杂挑战。这些问题若得不到系统性工程解决，RL扩展将导致训练不稳定、奖励信号稀疏或模型利用奖励漏洞。

论文通过构建MiMo-V2.6系列并沿着三个维度（批次/吞吐、环境复杂度、分级器计算）扩展RL，探索了一条实用的规模化路径，同时开源训练动态、环境和框架，意图推动该方向的研究共同体。

Q2: 有哪些相关研究？

由于论文未提供专门的Related Work章节，以下基于摘要、引言片段及当前研究背景进行合理推断：

1. **语言模型强化学习**：RLHF（如PPO）和组相对策略优化（GRPO）等已成为对齐和自我改进的标准技术。MiMo-V2.6采用GRPO类算法，并在其基础上进行扩展，例如引入组内优势重新分配（GAR）以更好地利用奖励信号。

2. **大规模混合专家（MoE）模型**：MoE架构在减少激活参数的同时提升模型容量，已有Mixtral等先例。MiMo-V2.6的特殊之处在于采用混合稀疏注意力（局部滑动窗口与全局注意力交错），并结合多模态编码器，这是对现有MoE架构的组合与扩展。

3. **多模态基础模型**：论文强调对文本、音频和视频的全面理解，这与GPT-4V、Gemini等多模态大模型的研究方向一致，但在架构上选择了更高效的混合注意力。

4. **智能体框架与任务环境**：论文提及使用了多种智能体harness，这类似于ReAct、AgentBench等框架；任务覆盖代码（如SWE-Bench）、通用工作流（如AutomationBench）、视觉设计、网络安全等，与近年agentic任务基准的发展紧密相关。

5. **递归自我改进（RSI）**：这一概念源于AI安全与未来学研究（如自我改进智能体的理论设想），论文将其具体化为通过持续探索、反馈和RL优化实现能力增强的工程实践，是RSI首次在大规模多任务环境中被系统地尝试。

6. **防奖励黑客与训练稳定性**：相关工作包括对奖励模型的对抗攻击、保守估计、KL正则化等，论文提出冻结MoE路由器和多层防御，是这一方向的补充。

总之，论文站在大规模RL、MoE、多模态和智能体研究的交叉点上，强调基础设施工程与系统化扩展，不同于侧重单一算法改进的工作。

Q3: 论文如何解决这个问题？

MiMo-V2.6采用了一套系统化、多层次的规模化RL方案，具体包括以下关键组成部分：

1. **基础模型与中训练**：使用1.02T（Pro）和310B（Flash）参数的MoE架构，激活参数分别为42B和15B，采用混合稀疏注意力（局部滑窗注意力SWA与全局注意力GA交错），并配备轻量视觉和音频编码器。在大规模预训练后，进行面向智能体的多模态中训练，在广泛语料上强化模型的任务分解、工具使用和长上下文能力，为后续RL提供探索空间。

2. **三维RL扩展**：
 - **批次与吞吐维度**：采用异步训练策略，每个训练步处理1,568个样本、2.7~3.7B tokens，支持高达1M的上下文长度，从而在长时任务中获得足够的轨迹样本和梯度更新稳定性。
 - **环境多样化维度**：构建覆盖代码、通用专业工作流、视觉工件和网络安全四类领域的任务环境，每个领域包含多个任务族，并混合多种智能体harness（如编码代理、浏览器代理、终端命令代理等），以提高策略的泛化能力。
 - **分级器计算维度**：引入组内智能体分级（groupwise agentic grading）机制。具体包括离线任务分析（通过静态分析或测试结果生成细粒度质量标签）和在线组内优势重分配（GAR），替代简单的二元测试奖励，为长时任务提供更稀疏但更有监督力的信号，同时引导模型偏向生成更短、更token高效的解决方案。

3. **训练稳定性机制**：
 - 冻结MoE路由器，减少RL过程中的动态变化，防止路由器漂移。
 - 建立多层防御对抗奖励黑客，包括行为正则化（例如惩罚过度修改、限制输出范围）和基于对比的评分校准。
 - 通过在线与离线评分的互补（观察显示在线评分产生更精确、更小的补丁），降低模型对奖励函数的漏洞利用。

4. **混合任务强化学习基础设施**：
 - 统一轨迹表示：按“Sample→Sequence→Context→Segment”四层层次组织轨迹数据，以便统一处理和混合来自不同任务和环境的数据。
 - 高并发多框架rollout：引入样本混合器（Sample Mixer）调度不同来源的prompt，支持多框架并行执行，并通过限制最大并发请求（如CUDA graph捕获）来控制队列延迟和轨迹陈旧性。
 - 解耦控制与数据平面：将训练控制、数据流和rollout执行分离，提高扩展性。
 - 训练-推理一致性：确保训练时的长度、格式和工具调用方式与推理时一致，减少分布偏移。

5. **开源资源**：开源训练动态（loss曲线等）、精选任务环境、验证器、端到端RL框架和可组合的迷你harness，并发布蒸馏版MiMo-V2.6-Distill-Qwen-9B，以支持社区复现和二次开发。

Q4: 论文做了哪些实验？

论文进行了一系列实验来验证规模化RL的效果，涉及多个领域的公共和内部基准，具体实验内容包括：

1. **多域基准评估**：
 - 编码任务：在DeepSWE v1.1、SWE-Bench Pro和MiMo Code Bench上评估，记录随RL训练步数的性能变化。
 - 通用工作流：使用AutomationBench v1.0.6（模拟日常办公任务，如文档处理、表格操作等）评估智能体的多步操作能力。
 - 视觉设计任务：在MiMo Visual Coding基准上测试从自然语言描述生成视觉化代码（如HTML/CSS）的能力。
 - 网络安全任务：在MiMo Cyber Bench上测试漏洞发现、攻击路径规划等安全场景能力。
 所有基准均展示MiMo-V2.6-Pro和Flash在RL训练步数增加时的得分变化曲线（图1），表明持续优化。

2. **评分机制对比实验**（基于检索到的引言片段）：对比了在线评分与离线评分的训练效果，发现在线评分训练出的策略倾向于产生更小、更精确且保持在请求范围内、更易维护的补丁。

3. **任务难度分析**：在网络安全任务中，作者分析了复杂C/C++项目的崩溃路径，指出困难在于触发特定漏洞而非任意崩溃，这要求验证机制能区分预期bug与其他错误，相关的任务设计和分级器设计被用于训练中。

4. **训练稳定性和防奖励黑客实验**：论文介绍了行为正则化机制（如惩罚超出范围的修改）以及冻结路由器的效果，但没有给出具体数值，合理推断是通过消融或稳定性对比评估的。

5. **基础设施验证**：验证了异步训练在1M上下文下每步1568样本的高吞吐，以及混合任务rollout的一致性。这些实验更多是工程性能测量，具体数字未在摘要中披露。

Q5: 发现了什么实验现象？

基于摘要、结论及检索到的引言片段，论文中观察到以下实验现象：

1. **性能随RL训练持续提升**：在编码、工作流、视觉和网络安全等所有评测域中，MiMo-V2.6-Pro和Flash的得分均随RL训练步数增加而上升，表明规模化RL在多任务agentic环境中带来持续收益。

2. **在线评分优于离线评分的策略特性**：使用在线评分的策略倾向于生成更小、更精确、严格限定在请求范围内的补丁，且更易维护；相反，离线评分（可能依赖静态标签）更容易导致模型产生更大范围的修改。这表明评分信号的时实性和判别性对策略行为有显著影响。

3. **任务合成中的难点不是崩溃而是精确触发**：在网络安全任务中，复杂C/C++项目暴露大量可触发的崩溃路径，真正的难点在于触发与描述一致的具体漏洞。模型需要学会区分目标漏洞与其他潜在的崩溃点，这对验证器的设计提出了更高要求——必须能辨别预期bug与其他错误。

4. **离线任务分析提供可重用监督**：将离线任务分析结果作为提示信号或部分奖励，可以捕捉到二元测试奖励无法表达的质量差异（如代码风格、局部性、修改范围），从而为模型提供比单一“程序通过/失败”更细粒度的学习信号。

5. **延迟控制对训练稳定性重要**：通过CUDA graph捕获和限制最大并发请求，可以显著降低队列延迟，减少轨迹陈旧（staleness），进而有助于保障大规模异步训练的稳定性。

6. **行为正则化的效果**：引入行为正则化（如限制修改规模、惩罚过于激进的输出）有助于减少reward hacking，训练出的策略更保守、更符合任务约束，但代价可能是探索效率的轻微下降（推测）。

7. **冻结MoE路由器的稳定性**：冻结路由器后，训练过程未出现明显的路由器漂移或梯度震荡，这被认为是能扩展到1M上下文和大批次的关键前提（合理推断）。

Q6: 有什么可以进一步探索的点？

基于论文已经建立的扩展框架和未解决的问题，可进一步探索以下方向：

1. **更多领域与真实工作负载**：将RL扩展至科学计算（如实验设计、数据解析）、医疗、法律等专业领域，尤其是AI for Science中涉及多步推理和工具调用的任务。

2. **更强和更通用的分级器**：当前组内智能体分级虽然优于二元奖励，但仍是人工设计的验证规则；未来可训练统一的“过程奖励+结果奖励”分级器，或让模型自身参与评分，从而自动化地生成细粒度监督。

3. **递归自我改进的闭环**：论文目前依赖人工构造任务环境；未来可让模型自动生成新任务、新环境和难度递增的课程，形成完整的自我改进循环，真正接近RSI愿景。

4. **更长上下文与更大批次的可扩展性**：探索超过1M上下文、更大批次（如数千甚至上万样本）下的RL稳定性和效率，以及相应的通信优化。

5. **更强的防奖励黑客理论**：当前多层防御是启发式的；可研究基于对抗训练、不确定性估计或可验证奖励的形式化方法来证明训练中不会发生奖励漏洞利用。

6. **蒸馏与部署**：MiMo-V2.6-Distill-Qwen-9B展示了蒸馏可行性；进一步可在更小模型上迁移RL技能，或者将RL训练的知识压缩到特定任务的专用小模型中。

7. **多智能体协作或竞争**：在环境中引入多智能体同时优化，研究合作或对抗情景下的自我改进动态，这也许能带来更丰富的探索信号。

8. **训练-推理一致性的自动化保障**：目前依赖工程手段；未来可通过形式化验证或元学习来保证部署时模型行为与训练时一致，降低分布漂移。

9. **可解释的RL训练动态**：开源训练动态后，可以进一步分析损失平面、注意力模式和任务转移规律，为理解RL如何塑造智能提供工具。

Q7: 总结一下论文的主要内容

本报告由小米LLM-Core团队发布，旨在通过大规模强化学习推进基础模型的递归自我改进。作者提出了MiMo-V2.6系列，包括MiMo-V2.6-Pro（1.02T总参数，42B激活参数）和MiMo-V2.6-Flash（310B总参数，15B激活参数），两个模型均为混合稀疏MoE架构，采用局部滑窗注意力和全局注意力交错设计，并配备轻量视觉和音频编码器，以支撑多模态智能体任务。

论文的论证主线是：要实现自我改进，模型必须拥有强大的基础能力（通过大规模预训练和面向智能体的中训练获得）以及一个可扩展的RL训练系统。为此，作者从三个维度扩展RL计算：一是扩大批次和吞吐，通过异步训练每步处理1568个样本和2.7~3.7B tokens，上下文长度达到1M，从而为长时任务提供足够的数据量；二是增加环境的多样性和复杂度，覆盖代码（如SWE-Bench）、通用工作流（如AutomationBench）、视觉设计（MiMo Visual Coding）和网络安全（MiMo Cyber Bench）四大领域，并混合多种智能体harness；三是增加分级器计算，引入组内智能体分级（GAR）和离线任务分析，用更准确的奖励信号替代二元测试奖励，并能引导模型生成更短、更token高效的解决方案。

为了在如此大规模下保持训练稳定，论文冻结了MoE路由器以减轻动态变化，并建立了多层防御对抗奖励黑客，包括行为正则化和在线/离线评分互补。基础设施方面，论文设计了统一轨迹表示（Sample→Sequence→Context→Segment）、高并发多框架rollout（样本混合器和CUDA graph限制延迟）、解耦控制与数据平面，以及训练-推理一致性机制，这些工程细节共同支撑了混合任务agentic RL的稳定运行。

实验部分，论文在多个公共和内部基准上报告了模型性能随RL训练步数的提升曲线，结果显示持续的优化收益。此外，基于检索到的片段，论文比较了在线与离线评分的策略行为，观察到在线评分产生更小、更精确、更易维护的补丁；分析了网络安全任务的难度本质（精确触发漏洞而非崩溃）；并提出离线任务分析作为可重用的监督源。这些实验揭示了评分信号、任务设计和稳定性机制对RL训练效果的直接影响。

最后，作者开源了训练动态、RL环境、验证器、端到端RL框架和可组合的迷你harness，并发布了蒸馏模型MiMo-V2.6-Distill-Qwen-9B，旨在使规模化RL和模型自我改进更加可接近和可复现。报告在结论中强调，一致的多域RL收益证明了该套件作为共享研究基础的价值，但未详细讨论失败案例或运行的负面结果。整体而言，这是一项以工程实践为主、兼有分析性实验的系统性工作，为复杂智能体任务的大规模RL提供了一个可参考的完整技术栈。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户画像中的“agent”方向直接相关：论文以智能体任务为核心，覆盖多环境、多harness，并讨论轨迹表示与rollout基础设施，是agentic RL的工程基准。

## 基本信息

- 作者：Xiaomi LLM-Core Team, :, Zongming Qiao, Ziyue Hua, Zirui Ou, Zihao Yue, Zihan Jiang, Zhuo Huang, Zhiyang Chen, Zhixian Zheng, Zhipeng Xu, Zhengrui Ma, Yuyang Hu, Yuhang Dong, Yuechen Zhang, Yudong Wang, Yuanxin Liu, Yixin Yang, Yishuo Cai, Yikai Zhao, Yihan Yan, Yifan Zhang, Yifan Song, Xiyu Wei, Xing Zhang, Xin Zhang, Xiaoqian Liu, Xiaodong Ji, Xiangwei Deng, Xueyu Guo, Wenhan Ma, Weimin Xiong, Weikun Wang, Weiji Zhuang, Shuo Liu, Shuhuai Ren, Shuhao Gu, Shimao Chen, Shijie Cao, Shihua Yu, Shicheng Li, Shengjie Zhou, Shaolei Zhang, Rang Li, Qiying Wang, Qingkai Fang, Qianli Chen, Minzheng Wang, Liwen Wang, Linli Yao, Linghao Zhang, Liangyu Cheng, Liang Zhao, Lei Li, Jinhao Dong, Jinyu Xiang, Jianyu Wei, Jiangshan Duo, Huaqiu Liu, Huanjie Fan, Hongyi Guan, Hongshen Xu, Hao Tian, Hanyu Li, Hailin Zhang, Gang Wang, Fuli Luo, Feng Wei, Dong Zhang, Dawei Zhu, Chiheng Lou, Chenhong He, Chenhao He, Chenghua Liu, Bowen Ye, Bowen Shen, Boshen Xu, Bo Yang, Bingquan Xia, Bangjun Xiao, Baixuan Xu, Zhouxiang Mao, Zhiyang Zhang, Zhixiang Xu, Zhenru Lin, Zhengju Tang, Zhaojun Huang, Yuzhe Weng, Yuxing Xiang, Yuxiao Li, Yuheng Yang, Yuhang Wang, Yuchen Liu, Yuanyuan Tian, Yuanliang Dong, Yu Cheng, Yongzhe He, Yongshun Liang, Yong Wang, Yiyan Wang, Yitian Gong, Yijie Zhang, Yanshu Xin, Xun Zhang, Xingjian Zhao, Wenyu Yang, Wenshan Huang, Wenhao Li, Tingwei Huang, Tianyu Yu, Tianyang Lu, Taoyu Yang, Sinan Du, Shutong Tian, Shulin Du, Shengfan Wang, Shanchuan Fang, Qihao Zhang, Qibin Yang, Qian Yu, Qian Tu, Pengrong Xie, Peipei Wang, Peidian Li, Minkun Guo, Mingchen Shao, Luohan Gao, Lijie Wang, Liang Shi, Kaiqi Chen, Kaiming Liu, Kaifei Wang, Kai Yang, Jinlong Xue, Jiechen Zhang, Jiaxuan Liu, Hongxu An, Hao Peng, Hanglong Lü, Guonan Wang, Feiyu Yang, Fanyu Cao, Fangyue Liu, Fan Cui, Cong Wang, Chun Chen, Chenxu Bai, Chengxuan Zhu, Chenghua Wang, Boyi Zeng
- 机构：小米 LLM-Core 团队（Xiaomi LLM-Core Team）
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-08
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.11959v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要、结论及检索到的引言片段，部分相关内容（如相关工作、具体实验细节）因原文未完整提供而基于摘要与片段合理推断。
