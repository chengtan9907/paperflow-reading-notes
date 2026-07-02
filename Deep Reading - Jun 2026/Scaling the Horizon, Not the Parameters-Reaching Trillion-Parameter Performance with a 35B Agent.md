---
user_id: "cheng tan"
paper_id: 2041
arxiv_id: "2606.30616"
title: "Scaling the Horizon, Not the Parameters: Reaching Trillion-Parameter Performance with a 35B Agent"
institution: "Shanghai Artificial Intelligence Laboratory"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.30616.pdf"
pdf_url: "https://arxiv.org/pdf/2606.30616"
abs_url: "https://arxiv.org/abs/2606.30616"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:41:33"
---
# Scaling the Horizon, Not the Parameters: Reaching Trillion-Parameter Performance with a 35B Agent

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic model · mixture of experts · agent horizon · long-horizon trajectories

## 一句话总结

我们提出了Agents-A1，一个35B混合专家模型，通过扩展智能体决策长度达到了万亿参数级别的性能。

## 摘要

> We introduce Agents-A1, a 35B Mixture-of-Experts Agentic Model that reaches trillion-parameter-level performance by scaling the agent horizon. We investigate agent-horizon scaling from two perspectives: scaling long-horizon trajectories and scaling heterogeneous agent abilities. To support this goal, we build a long-horizon knowledge-action infrastructure that connects external knowledge, actions, observations, and verifier outcomes, producing agentic trajectories with an average length of 45K tokens. Based on this, we train Agents-A1 with a three-stage recipe. First, we perform full-domain supervised fine-tuning to align the base model with broad agentic behaviors. Second, we train domain-level teacher models to capture specialized expertise in each domain. Third, we propose a multi-teacher domain-routed on-policy distillation with salient vocabulary alignment to improve knowledge transfer efficiency across different domains, unifying six heterogeneous domains into one deployable student model. Agents-A1 achieves strong and broad performance for long-horizon agent benchmarks. Compared with 1T-parameter model such as Kimi-K2.6 and DeepSeek-V4-pro, Agents-A1 achieves leading results on SEAL-0 (56.4), IFBench (80.6), HiPhO (46.4), FrontierScience-Olympiad (79.0), and MolBench-Bind (56.8), and remains highly competitive on SciCode (44.3), HLE (47.6) and BrowseComp (75.5). We hope this work provides the community with a practical path for scaling the horizon using a 35B agent that can reach or match the performance of 1T models on long-horizon tasks.

Q1: 这篇论文试图解决什么问题？

现有大型语言模型在长决策任务（如多步搜索、科学研究、工具调用等）中表现不足，主要原因是模型难以在扩展轨迹中规划、正确调用工具、验证证据、整合反馈并从失败中恢复。传统方法通过扩大模型参数规模（如1T参数）来提升能力，但这需要巨大的计算资源和数据，且难以复现。本文旨在探索另一种路径：扩展智能体的决策长度（agent horizon），即通过增加决策步骤和上下文长度，让一个中等规模（35B）的模型通过“更深”的推理和交互达到更大规模模型的能力。具体问题包括如何构建长决策轨迹、如何训练模型在长上下文中有效推理，以及如何融合多个领域的能力。

Q2: 有哪些相关研究？

相关研究包括：（1）大规模语言模型智能体，如ReAct、Toolformer等，通常依赖外部工具但轨迹较短。（2）混合专家模型（MoE），如Mixtral 8x7B，通过稀疏激活提升效率。（3）知识蒸馏技术，如DistilBERT、TinyBERT，将大模型知识压缩到小模型。（4）多任务学习和领域自适应方法。本工作结合了这些方向，但独特之处在于强调通过扩展决策长度而非参数来实现性能提升，并为六个异构领域构建了统一的学生模型。

Q3: 论文如何解决这个问题？

论文提出了一种三阶段训练框架：（1）第一阶段：全领域监督微调（Full-domain SFT），基于Qwen3.5-35B-A3B基础模型，使用包含操作环境交互、工具使用、科学问题求解、指令跟随等行为的数据进行SFT，使模型获得广泛的智能体行为能力。（2）第二阶段：训练领域教师模型（Domain Teacher Training），对每个领域（共6个）分别训练专门的教师模型，通过SFT或强化学习使其在特定领域达到专家水平。教师模型捕获了领域特异的知识。（3）第三阶段：多教师领域路由在线蒸馏（Multi-Teacher Domain-Routed On-Policy Distillation），提出一种蒸馏方法，其中学生模型（从SFT模型开始）通过在线交互生成轨迹，每个轨迹由路由策略分配给最相关的教师模型进行监督。蒸馏过程中还采用了显著词汇对齐（Salient Vocabulary Alignment）技术，提升不同领域间知识迁移的效率。最终将六个异构领域的能力统一到一个可部署的学生模型中。此外，论文还构建了长决策知识-动作基础设施（Knowledge-Action Infrastructure），其中包括知识-动作图、自博弈数据生成等，用于产生平均长度为45K token的轨迹。

Q4: 论文做了哪些实验？

论文在多个长决策基准上评估Agents-A1，包括：SEAL-0（56.4）、IFBench（80.6）、HiPhO（46.4）、FrontierScience-Olympiad（79.0）、MolBench-Bind（56.8）、SciCode（44.3）、HLE（47.6）和BrowseComp（75.5）。对比的基线包括同规模35B模型以及1T参数模型如Kimi-K2.6和DeepSeek-V4-pro。实验还包括对蒸馏方法、教师模型、领域路由等的消融研究（证据中提及但具体数字未提供）。表格中展示了Agents-A1在多数任务上领先，在SciCode、HLE和BrowseComp上竞争力强。

Q5: 发现了什么实验现象？

主要实验现象：（1）35B的Agents-A1在多个长决策基准上达到了与1T参数模型相当或更优的性能，验证了扩展决策长度的有效性。（2）在科学推理任务（如FrontierScience-Olympiad、MolBench）上表现突出，表明长轨迹在复杂推理中的优势。（3）在某些任务（如BrowseComp）上略低于1T模型，可能因为浏览环境需要更广的知识覆盖。（4）消融实验（推测）表明多教师蒸馏和领域路由对性能有显著贡献。

Q6: 有什么可以进一步探索的点？

未来工作方向包括：（1）将当前方法扩展到更多领域和更长的决策长度。（2）改进领域路由机制，使其更自适应且减少教师模型间的冲突。（3）探索更高效的蒸馏策略，减少在线采样的成本。（4）将Agentic模型与更多外部知识源集成。（5）研究扩展决策长度与模型参数规模之间的相互作用，找到最优组合。（6）应用于更真实的机器人或科学研究系统。

Q7: 总结一下论文的主要内容

本文旨在探索一条通过扩展智能体决策长度（agent horizon）而非参数规模来提升性能的技术路径。作者提出了35B参数的MoE模型Agents-A1，其主要贡献包括：构建了长决策知识-动作基础设施，产生平均45K token的轨迹；设计了三个阶段的训练流程（全领域SFT、领域教师训练、多教师域路由在线蒸馏）；在六个异构领域上统一了能力。实验结果表明，Agents-A1在SEAL-0、IFBench、HiPhO、FrontierScience-Olympiad、MolBench-Bind等基准上超越同规模模型并达到或超过Kimi-K2.6、DeepSeek-V4-pro等1T参数模型，验证了扩展决策长度的潜力。这篇工作为构建高效的智能体模型提供了新思路，表明计算资源可以聚焦于决策深度而非模型宽度。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文提出的方法对智能体领域研究有直接参考价值，特别是长决策任务

## 基本信息

- 作者：Lei Bai, Zongsheng Cao, Yang Chen, Zhiyao Cui, Shangheng Du, Yue Fan, Shiyang Feng, Zijie Guo, Haonan He, Liang He, Xiaohan He, Shuyue Hu, Yusong Hu, Songtao Huang, Yichen Jiang, Hao Li, Xin Li, Dahua Lin, Weihao Lin, Fenghua Ling, Dongrui Liu, Zhuo Liu, Runmin Ma, Chunjiang Mu, Haoyang Peng, Tianshuo Peng, Jinxin Shi, Luohe Shi, Boyuan Sun, Zelin Tan, Shengji Tang, Qianyi Wang, Yiming Wu, Yi Xie, Xiangchao Yan, Jingqi Ye, Peng Ye, Fangchen Yu, Jiakang Yuan, Bihao Zhan, Bo Zhang, Chen Zhang, Shufei Zhang, Shuaiyu Zhang, Wenlong Zhang, Yiqun Zhang, Junpeng Zhao, Zhijie Zhong, Bowen Zhou, Yuhao Zhou
- 机构：Shanghai Artificial Intelligence Laboratory
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-30
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.30616`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据片段
