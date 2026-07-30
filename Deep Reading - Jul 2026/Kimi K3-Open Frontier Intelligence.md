---
user_id: "cheng tan"
paper_id: 5718
arxiv_id: "2607.24653v1"
title: "Kimi K3: Open Frontier Intelligence"
publish_date: "2026-07-27"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.24653v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.24653v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:52:58"
---
# Kimi K3: Open Frontier Intelligence

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：mixture of experts · attention mechanisms · reinforcement learning · long context

## 一句话总结

提出 Kimi K3，一个 2.8T 参数的混合专家模型，具有 104B 激活参数、原生视觉能力和 100 万 token 上下文窗口，基于 Kimi Delta Attention 和 Attention Residuals，实现约 2.5 倍缩放效率提升并在多项任务上达到前沿性能。

## 摘要

> We introduce Kimi K3, a 2.8T parameter Mixture-of-Experts model with 104 billion activated parameters, native vision capabilities, and a 1-million-token context window. Kimi K3 is built on Kimi Delta Attention [63] and Attention Residuals [57], which improve information flow across sequence length and model depth. Together with Stable LatentMoE, which effectively activates 16 of 896 routed experts per token, and refined training and data recipes, these advances yield an approximately $2.5 \times$ improvement in overall scaling efficiency over Kimi K2 [58]. Post-training highlights reinforcement learning across general, agentic, and coding domains and multiple reasoning-effort levels, enabling compositional generalization and robust long-horizon execution. At 2.8T scale, Kimi K3 is supported by infrastructure advances in multiple areas: algorithm-system co-design for KDA, perfectly balanced expert-parallel training with efficient memory management, million-token agentic RL with persistent rollout and sandbox states, and deployment innovations.
> Extensive evaluations show that Kimi K3 achieves frontier-level performance across long-horizon coding, agentic, knowledge, reasoning, and vision tasks. While its overall performance still trails the most powerful proprietary models, namely Claude Fable 5 and GPT-5.6 Sol, Kimi K3 consistently outperforms other open and proprietary models evaluated in our suite. We release the full Kimi K3 model weights to facilitate future research and accelerate the broader deployment and adoption of frontier intelligence. $^{1}$
> ![](images/ab5334dea45c2bba18b3a7eeb80574478e8f184d27ca2d1236bd8c7a13429cef.jpg)
> General & Visual Agents All maxed out on thinking effort: max or xhigh.
> Note: All Fable 5 results are with potential fallbacks. All GPT-5.6 Sol results include potential cyberguards.
> Figure 1: Kimi K3 main results.

Q1: 这篇论文试图解决什么问题？

尽管近年来 AI 模型在推理能力上快速进步，但在模型规模这一维度上进展缓慢，多数开放模型仍停留在 1T 参数级别。同时，现有模型在长上下文处理、原生视觉理解、复杂智能体任务和高效缩放等方面存在显著局限。Kimi K3 旨在解决以下核心问题：
- 如何将开放模型的参数规模推升至 2.8T（104B 激活），同时保持训练和推理的可行性；
- 如何设计注意力机制以同时支持百万 token 长序列和深层网络的信息流动；
- 如何通过强化学习后训练使大规模模型在编码、智能体、推理和知识等多领域实现组合泛化；
- 如何协调算法、系统和数据配方以突破缩放效率瓶颈。

Q2: 有哪些相关研究？

相关工作涵盖多个方向：
- 大规模混合专家模型：如 Mixtral 8x22B、Grok-1（314B 激活）、DeepSeek-V2/V3 以及前代 Kimi K2（1.1T 参数，MoE-6B）。Kimi K3 将参数量推至 2.8T，成为首个开放 3T 级模型。
- 长上下文注意力机制：Transformer-XL、Longformer、Ring Attention、Gated MLA（Multi-head Latent Attention）等。KDA 整合了 Gated MLA 和标准注意力模式，以实现高效长序列混合。
- 强化学习后训练：RLHF、GRPO、PPO 等。Kimi K3 使用类似 PPO 的 RL 进行多领域后训练，并通过不同推理深度（low/medium/high/max）进行调控。
- 智能体与工具使用：代码生成、Web 导航、环境交互等。Kimi K3 在智能体任务上使用百万 token 上下文窗口，实现持久化 rollout 和沙箱状态管理。

Q3: 论文如何解决这个问题？

Kimi K3 的解决方案跨越架构、训练、后训练和基础设施四个层面：

1. **架构创新**
 - Kimi Delta Attention (KDA)：混合注意力机制，每隔若干层插入 Gated MLA 层，其余层保留标准 Softmax 注意力。Gated MLA 通过门控机制选择性地聚合长距离信息，有效降低了长序列的计算复杂度，同时保留了全局交互。
 - Attention Residuals (AttnRes)：为每个注意力层添加残差连接和信息门控，改善信号在深层网络中的传播，防止梯度消失并提升深层表示质量。
 - Stable LatentMoE：基于 LatentMoE 的改进路由策略，每个 token 激活 896 个路由专家中的 16 个。通过稳定性正则化和负载平衡，减少专家退化和塌陷问题，提升 MoE 训练稳定性。

2. **训练方法**
 - 预训练使用大规模高质量数据，结合数据配比和课程学习策略。通过缩放定律验证实现约 2.5 倍的缩放效率提升（与 Kimi K2 相比）。
 - 后训练采用强化学习（RL）覆盖四大领域：长编码（Long Code）、通用智能体（General Agent）、通用推理（General Reasoning）和知识（Knowledge）。每个领域支持多种推理深度（low/medium/high/max），模型通过 RL 学习在不同复杂度水平下调整推理过程。

3. **基础设施协同设计**
 - 算法-系统协同实现完美平衡的专家并行训练：通过动态规划专家分配和通信优化，保证计算和通信负载均衡。
 - 高效内存管理：针对 KDA 混合注意力模式优化显存占用，利用梯度检查点、混合精度等技术。
 - 百万 token 智能体 RL：持久化 rollout 和沙箱状态管理，在有限计算预算下实现高效训练和推理。

Q4: 论文做了哪些实验？

论文进行了广泛的多领域性能评估，对比模型包括闭源的 Claude Fable 5、GPT-5.6 Sol，以及开源的 DeepSeek-V3、Qwen3、Llama 4、Gemma 3、Mistral、Grok 等。评估基准覆盖以下维度：
- 编码与软件工程：SWE-bench（Agent 模式）、KernelSWE、Aider Polyglot、HumanEval+、LiveCodeBench、Codeforces 等。
- 智能体：WebArena、WorkArena、GAIA、General Tasks（按照论文内部基准）。
- 知识与语言：MMLU（multiple-choice 和 derivational）、SimpleQA、MMMU（多模态）、MT-Bench。
- 推理：GPQA、AIME（数学）、AMC、MATH、Grok Bench、Sparrow 等。
- 视觉多模态：MMMU（大学水平）、MathVista、MMT-Bench 等。
- 长上下文：RULER、LV-Eval、Long Context（内部基准）等。
评估使用多种推理深度（max / high / medium / low）进行比较，并报告带和不带思维链的结果。

Q5: 发现了什么实验现象？

主要实验现象和发现：

1. **缩放效率提升**：KDA 和 AttnRes 结合数据配方优化，使 Kimi K3 在同等训练计算下表现显著优于 Kimi K2，整体缩放效率提升约 2.5 倍（论文图 2）。

2. **与闭源模型对比**：在几乎所有评估基准上，Kimi K3 均超越其他开源模型和多数闭源模型。但总体性能仍落后于 Claude Fable 5 和 GPT-5.6 Sol，特别是在高难度推理和知识任务上存在差距。

3. **推理深度的影响**：随着推理深度（从 low 到 max）的增加，模型在推理密集型任务（如数学、编程竞赛）上性能持续提升，但在部分简单任务上提升不明显或略有下降，表明模型学会了根据复杂度分配推理资源。

4. **智能体任务**：在 WebArena、WorkArena 等需要长步骤交互的任务上，Kimi K3 借助百万 token 上下文和持久化 rollout 表现出色，错误率较其他模型显著降低。

5. **长上下文稳定性**：在 RULER（100K）和 LV-Eval（1M）上，Kimi K3 的召回率随序列长度增加而平稳下降，在 1M token 长度下仍保持较高性能，优于未使用长序列优化的模型。

6. **视觉能力**：在多模态基准 MMMU、MathVista 等上，Kimi K3 超越了纯文本的 GPT-5.6 Sol（不考虑视觉输入时），但仍低于原生多模态的 Claude Fable 5。

7. **消融实验**：移除 AttnRes 后，深层网络性能下降明显；移除 KDA 后，长序列处理退化严重；Stable LatentMoE 的稳定性正则化减少了负载失衡和专家塌陷。

Q6: 有什么可以进一步探索的点？

基于论文的内在逻辑和领域现状，可进一步探索的方向包括：
1. 提升与最强闭源模型的差距：改进后训练 RL 方法（如更复杂的奖励模型、更高效的 PPO 变体），以及引入更多自我博弈和多智能体交互。
2. 更长的上下文窗口：探索超出 1M token 的超长上下文能力，结合主动检索和稀疏 attention。
3. 动态专家分配和适应：Stable LatentMoE 的专家数（896）可能可以动态调整，或根据任务特性选择专家子集。
4. 多模态深度整合：当前模型通过原生视觉处理，但视频、3D 等模态尚未充分覆盖；可扩展到视频理解和生成。
5. 推理效率优化：3T 级模型的推理成本仍然高昂；蒸馏、量化和 speculative decoding 等方法可降低部署门槛。
6. 安全性与对齐：开源前沿模型可能被误用，需要加强安全训练和 red-teaming。
7. 科学应用：利用其知识推理能力在生物、医学、材料等科学领域进行验证和迁移。

Q7: 总结一下论文的主要内容

Kimi K3 是来自 Moonshot AI（Kimi 团队）的一个开放前沿智能模型，总参数量 2.8T，激活参数 104B，原生支持多模态视觉输入，上下文窗口达到 100 万 token。模型采用混合专家架构，核心技术创新包括：

**架构创新**
- Kimi Delta Attention (KDA)：通过交替使用 Gated MLA 层和标准注意力层，实现对长序列的高效混合。Gated MLA 使用门控机制选择性地聚合远程信息，降低了二次复杂度。
- Attention Residuals (AttnRes)：为每个注意力层引入残差连接和信息门控，缓解深层网络中的信息衰减，使得 2.8T 模型的训练和推理更加稳定。
- Stable LatentMoE：改进的 MoE 路由机制，每个 token 从 896 个专家中激活 16 个。稳定性正则化防止专家塌陷和负载不均，提升 MoE 训练效率。

**训练与缩放**
预训练阶段使用精心设计的数据配方，结合课程学习和数据配比。通过这些架构创新和训练优化，Kimi K3 在相同计算量下较前代 Kimi K2 实现了约 2.5 倍的缩放效率提升。

**后训练：强化学习驱动的多领域训练**
后训练使用强化学习覆盖四大领域：长编码（Long Code）、通用智能体（General Agent）、通用推理（General Reasoning）和知识（Knowledge）。每个领域支持多级推理深度（low/medium/high/max），模型通过 RL 学会在不同复杂度水平下调整推理过程，实现了组合泛化。训练环境包括可验证搜索、专业知识和软件工程与内核优化、多模态推理（视觉在环工具使用）以及持久化智能体交互。

**基础设施创新**
为了支持 2.8T 模型的高效训练和百万 token 智能体 RL，论文实现了算法-系统协同设计：完美平衡的专家并行（动态分配专家，保证计算通信均衡）、高效内存管理（针对混合注意力优化显存）、持久化 rollout 和沙箱状态管理（在有限预算下实现长步骤 RL）。

**评估结果**
在编码（SWE-bench Agent、KernelSWE、HumanEval+ 等）、智能体（WebArena、WorkArena、GAIA）、知识（MMLU、SimpleQA）、推理（GPQA、AIME、MATH）和视觉（MMMU、MathVista）等广泛基准上，Kimi K3 均超越其他开源模型和多数闭源模型。然而整体性能仍略低于最强闭源模型 Claude Fable 5 和 GPT-5.6 Sol。

**开源贡献**
论文开源全部模型权重，旨在促进前沿智能的学术研究和广泛应用。

总而言之，Kimi K3 代表开放模型在规模（2.8T）和能力（长上下文、智能体、多模态）上的一次重要跃进，验证了混合注意力、稳定 MoE 和多领域 RL 后训练在超大规模下的有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：智能体方向高度相关：论文强调 agentic RL 和百万 token 智能体交互，对智能体研究和应用有直接参考价值。

## 基本信息

- 作者：Kimi Team, Tongtong Bai, Yifan Bai, Yiping Bao, M. C., Jianfeng Cai, Xinyuan Cai, Peizhou Cao, Yuxuan Cao, Ziwei Chai, Y. Charles, H. S. Che, Guanduo Chen, Guangyu Chen, Guanzheng Chen, Huarong Chen, Jia Chen, Jianlong Chen, Jun Chen, Kexin Chen, Peng Chen, Ruijue Chen, Wentao Chen, Xin Chen, Yang Chen, Yanru Chen, Yifei Chen, Yingjiang Chen, Yuankun Chen, Yujie Chen, Yutian Chen, Zhirong Chen, Dazhi Cheng, Yean Cheng, Jialei Cui, Jingbing Cui, Anqi Dai, Jiaqi Deng, Hao Ding, Rui Ding, Shaofeng Ding, Mengfan Dong, Mengnan Dong, Yuhao Dong, Yuxin Dong, Angang Du, Chenzhuang Du, Dikang Du, Jusen Du, Yulun Du, Yu Fan, Jing Feng, Qiulin Feng, Yichen Feng, Kelin Fu, Qiang Fu, Fuxuan Gao, Hongcheng Gao, Jingyue Gao, Tong Gao, Weijia Gao, Shangyi Geng, Jie Gong, Linhu Gong, Shengao Gong, Xiaochen Gong, Qizheng Gu, Yicheng Gu, Shuhao Guan, Haiqing Guo, Shiqi Guo, Xiang Guo, Zhengyan Guo, Beixi Hao, Wenxin Hao, Xiaoru Hao, Dailan He, Haotian He, Lehan He, Qi He, Weiran He, Xinran He, Xinyi He, Yibo He, Yunjia He, Chao Hong, Tiange Hong, Hao Hu, Jiaxi Hu, Ruikun Hu, Weiming Hu, Yangyang Hu, Zhenxing Hu, Liang Hua, Jinbin Huang, Ke Huang, Ruiyuan Huang, Siying Huang, Weixiao Huang, Yan Huang, Zhengjie Huang, Zhiqi Huang, Yulong Hui, Chaobo Jia, Yutong Jiang, Zhejun Jiang, Zuoyou Jiang, Wenyi Jin, Xinyi Jin, Yu Jing, Huanjun Kong, Guokun Lai, Aidi Li, Cheng Li, Chengyuan Li, Cong Li, Fang Li, Guanyu Li, Haoyang Li, Jia Li, Junxiong Li, Lei Li, Letian Li, Lincan Li, Weihong Li, Wentao Li, Xintong Li, Yang Li, Yishen Li, Yiwei Li, Yuxiao Li, Zhaowei Li, Zhaoxi Li, Zheming Li, Zhengxiao Li, Zhiyuan Li, Jiawei Lin, Xiaohan Lin, Yibo Lin, Zichao Lin, Ziyan Lin, Bill Liu, Boxiao Liu, Chuan Liu, Liang Liu, Shaowei Liu, Shudong Liu, Shuran Liu, Tianwei Liu, Weizhou Liu, Yangyang Liu, Yanming Liu, Yibo Liu, Yipeng Liu, Zhengying Liu, Zhiheng Liu, Enzhe Lu, Haoyu Lu, Linqiang Lu, Tingzhan Lu, Zhiyuan Lu, Aotian Luo, G. Luo, Junyu Luo, Yifan Luo, B. Lyu, Wenzhou Lyu, Shaoguang Mao, Yuan Mei, Xin Men, Minqing Ni, Yixuan Niu, Siyuan Pan, Shujun Peng, Zhangyang Qi, Ruoyu Qin, ZeChao Qin, Zeyu Qin, Haiquan Qiu, Jianxin Qiu, Jiezhong Qiu, Bowen Qu, Yuhao Qu, Zeyu Shang, Youbo Shao, Han Shen, Jincheng Shi, Juanfeng Shi, Lidong Shi, Shengyuan Shi, Wingchun Siu, Pengwei Song, Xiaoxi Song, Jianlin Su, Yunfeng Su, Zhaochen Su, Lin Sui, Jingsong Sun, Junyao Sun, Shaoning Sun, Shuzhe Sun, Tongyu Sun, Yujun Sun, Yunpeng Tai, Chuning Tang, Heyi Tang, Sirui Tang, Zecheng Tang, Chaoran Tian, Rongpeng Tian, Yu Tian, Wei Tu, Chensi Wang, Chuang Wang, Chunjie Wang, Dinglu Wang, Feng Wang, Hailong Wang, Haiming Wang, Hao Wang, Hao Wang, Huaqing Wang, Hui Wang, Jiayi Wang, Jinglong Wang, Jinhong Wang, Jiuzheng Wang, Linian Wang, Shaobo Wang, Shenzhi Wang, Shuyi Wang, Si Wang, Siyuan Wang, Tianfu Wang, Wenjue Wang, Xingran Wang, Xinmei Wang, Xinyuan Wang, Xusheng Wang, Yalin Wang, Yangkun Wang, Yao Wang, Yaoyu Wang, Yejie Wang, Yiqin Wang, Yucheng Wang, Yuzhi Wang, Zhaoji Wang, Zhaowei Wang, Zhengtao Wang, Zhenhao Wang, Zhongsheng Wang, Zifan Wang, Chu Wei, Ming Wei, Shouxin Wei, Zichen Wen, Fan Wu, Haoning Wu, Rucong Wu, Wenhao Wu, Xiaoxue Wu, Yingcong Wu, Yongqi Wu, Yuxin Wu, Zijian Wu, Xinglang Xian, Chenxuan Xiang, Yuye Xiang, Bocheng Xiao, Chenjun Xiao, Xin Xiao, Jin Xie, Xiaotong Xie, Yifeng Xie, Zhe Xie, Bowei Xing, Yiming Xiong, Baosheng Xu, Boyu Xu, Jiale Xu, Jianfan Xu, Jing Xu, Jinjing Xu, L. H. Xu, Qingtao Xu, Shuyao Xu, Suting Xu, Tiantian Xu, Tianxiang Xu, Weixin Xu, Xinran Xu, Yangchuan Xu, Ye Xu, Yueni Xu, Ziyao Xu, Haonan Xue, Junjie Yan, Yaoyao Yan, Fan Yang, Guangyao Yang, Hao Yang, Junwei Yang, Ruoyu Yang, Wenjie Yang, Xiaofei Yang, Xinyu Yang, Yi Yang, Yiling Yang, Ying Yang, Yuchen Yang, Zhen Yang, Zhilin Yang, Zian Yang, Zuhao Yang, Haotian Yao, Dan Ye, Haoran Ye, Wenjie Ye, Zhanbo Ye, Bohong Yin, Haoxiang Yin, Xietong Yin, Chengzhen Yu, Haozhen Yu, Longhui Yu, Shengnan Yu, Shuying Yu, Tianxiang Yu, Enming Yuan, Mengjie Yuan, Tongtian Yue, Wei Yue, Yang Yue, Dunyuan Zha, Haobing Zhan, B. H. Zhang, Dehao Zhang, Fei Zhang, Hao Zhang, Haoyuan Zhang, Huanyu Zhang, Jiapei Zhang, Jiaxuan Zhang, Jin Zhang, Kaiyi Zhang, Miaozhen Zhang, Puqi Zhang, Qinglei Zhang, Rong Zhang, Rui Zhang, Shaoshuai Zhang, Shiyi Zhang, Xiaobin Zhang, Xiaoyun Zhang, Y. Zhang, Yangkun Zhang, Ye Zhang, Yichi Zhang, Yikun Zhang, Yizhi Zhang, Yongting Zhang, Yu Zhang, Yutao Zhang, Yutong Zhang, Zheng Zhang, Zijing Zhang, Bin Zhao, Chenguang Zhao, Feifan Zhao, Jinglun Zhao, Jinxiang Zhao, Shuai Zhao, Wenshuo Zhao, Xiangyu Zhao, Xuanle Zhao, Yikai Zhao, Zijia Zhao, Haozhi Zheng, Huabin Zheng, Ruihan Zheng, Shaojie Zheng, Tengyang Zheng, Haofeng Zhong, Lei Zhong, Longguang Zhong, M. Zhou, Qiankang Zhou, Runjie Zhou, Ruozhang Zhou, Xinyu Zhou, Yiqiao Zhou, Zaida Zhou, Jinguo Zhu, Liya Zhu, Xinhao Zhu, Yangjunfeng Zhu, Yuxuan Zhu, Zhen Zhu, Chen Zhuang, Weiyu Zhuang, Xinxing Zu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-07-27
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.24653v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了提供的 PDF 检索证据和 field evidence mapping，并结合摘要推断部分细节。
