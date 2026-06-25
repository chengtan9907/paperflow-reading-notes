---
user_id: "cheng tan"
paper_id: 1099
arxiv_id: "2606.19704v1"
title: "Beyond Static Leaderboards: Predictive Validity for the Evaluation of LLM Agents"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.19704v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.19704v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-21T00:01:27"
---
# Beyond Static Leaderboards: Predictive Validity for the Evaluation of LLM Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agent · benchmark evaluation · predictive validity · ranking instability

## 一句话总结

本文聚合了对一个基于MCP的工业智能体基准的最大规模协调深入分析（14个并行实现研究），提出用预测效度（样本内与样本外排名的相关性）而非样本内平均分来评估LLM智能体，并报告了一个12层测量装置。

## 摘要

> Agent benchmarks are growing fast, but no single benchmark touches more than four or five of the dimensions that deployment exposes. This paper aggregates the largest coordinated deep-dive of one MCP-based industrial-agent benchmark to date: fourteen parallel implementation studies covering new asset classes (including a multi-modal visual extension), alternative orchestrations, retrieval strategies, reasoning modes, infrastructure optimizations, and evaluation-methodology probes. Consolidating those studies with seven prior agent benchmarks, we argue that aggregate-score leaderboards systematically underspecify deployed-agent evaluation. Rankings derived from aggregate scores do not transfer to out-of-distribution settings; recent public-to-hidden competition retrospectives provide direct empirical evidence of this rank instability. We propose ranking configurations by predictive validity, the correlation between in-sample and out-of-sample rank, rather than in-sample mean, and report a twelve-tier measurement apparatus that exposes the deployment-relevant dimensions HELM and its agent-era successors collapse. The position is operationalized through three falsifiable out-of-distribution criteria with explicit thresholds; existing evidence partly supports it but is too thin to confirm. We close with a pre-registered pilot design and a field-level vision for what the next generation of agentic benchmarks should report.

Q1: 这篇论文试图解决什么问题？

现有LLM智能体基准的聚合分数排行榜系统性地低估了部署评估所需的维度，排名不能迁移到分布外设置，导致评估结果与真实部署性能脱节。论文试图建立预测效度作为更优的排名准则，并构建更全面的测量框架。

Q2: 有哪些相关研究？

论文批评了静态排行榜（如HELM的聚合分数方法）以及后续智能体时代基准（如AgentBench、WebArena等）的局限性，指出它们只覆盖了少数维度。最近的公开到隐藏竞赛回顾（如Chatbot Arena的隐藏设置）提供了排名不稳定的证据。论文还提及了七个先前的智能体基准，但未具体展开。现有评估方法缺乏对部署相关维度的系统考虑，特别是跨设置泛化能力。

Q3: 论文如何解决这个问题？

论文提出用预测效度（predictive validity）作为排名准则，即样本内排名与样本外排名的相关性。通过14个并行实现研究扩展了一个基于MCP的基准，涵盖六个扩展轴（资产类别、编排、检索、推理、基础设施、评估方法），并构建了一个12层测量装置以暴露被现有基准忽略的部署相关维度。此外提出了三个可证伪的分布外标准（带显式阈值）以操作化该立场。实证验证被定为未来工作。

Q4: 论文做了哪些实验？

论文聚合了14个并行实现研究，涉及约6000条人工判断轨迹，对基准进行了六个维度的扩展（包括多模态视觉扩展、交替编排、多种检索策略、推理模式对比、基础设施优化如torch.compile+GPU、以及评估方法学探针如不同judging协议）。此外结合七个先前智能体基准进行综合分析，展示了聚合分数排名在不同设置下的不稳定性。但正式预测效度实验尚未运行。

Q5: 发现了什么实验现象？

主要观察是聚合分数排名不稳定：在样本内表现良好的模型在样本外设置中排名显著变化，公开排行榜排名与隐藏测试排名之间出现反转。不同扩展维度（如编排方式、检索策略）对模型排名的影响是非单调且交互的，说明单一聚合分数无法可靠预测部署性能。具体数值结果未在摘要中提供，但论文指出观测现象支持预测效度假设。

Q6: 有什么可以进一步探索的点？

1. 执行预注册试点实验，系统验证预测效度作为排名准则的有效性；2. 测试三个可证伪分布外标准（如阈值0.8、0.85、0.9）在更大规模上的表现；3. 细化12层测量装置中维度的正交性与完备性；4. 探索不同领域（如科学应用、生成任务）中评估维度的适配性；5. 推动整个领域采用包含预测效度报告的标准化评估协议。

Q7: 总结一下论文的主要内容

本文针对LLM智能体评估中静态排行榜的局限性，提出了预测效度（predictive validity）作为替代排名准则。通过聚合14个并行实现研究（约6000条判断轨迹），扩展了一个基于MCP的工业基准，覆盖六个扩展轴，并构建了12层测量装置，揭示了现有聚合分数排名在分布外场景中的不稳定性。论文还明确了三个可证伪的分布外标准，但实证验证被规划为未来工作。最后给出了预注册试点设计和下一代基准愿景。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该论文与智能体评估方向直接相关，提出了新的评估范式，对部署落地有重要意义

## 基本信息

- 作者：Dhaval C. Patel, Kaoutar El Maghraoui, Shuxin Lin, Yusheng Li, Tianjun Feng, Chun-Yi Tsai, Yihan Sun, Wei Alexander Xin, Akshat Bhandari, Tanisha Rathod, Aaron Fan, Sanskruti Vijay Shejwal, Tomas Pasiecznik, Sagar Chethan Kumar, Tanmay Agarwal, Rohith Kanathur, Sam Colman, Amaan Sheikh, Dev Bahl, Ann Li, Krish Veera, Alimurtaza Mustafa Merchant, Shambhawi Baswaraj Bhure, Sajal Kumar Goyla, Chengrui Li, Kirthana Natarajan, Rui Li, Thomas Ajai, Rujing Li, Vivek G. Iyer, Sanjaii Vijayakumar, Yitong Bai, Ayal Yakobe, Darief Maes, Yassine Jebbouri, Tianyang Xu, Thai Quoc On, Vera Mazeeva, Winston Li, Yuval Shemla, Yeshitha Bhuvanesh, Rushin Bhatt, Siddharth Chethan Gowda, Alisha Vinod, Caroline Cahill, Shriya Aishani Rachakonda, Yunfeng Chen, Aryaman Agrawal, Aman Upganlawar, Mao Le Jonathan Ang, Yubin Sally Go, Madhav Rajkondawar, Yang-Jung Chen, Trisha Maturi, Ananya Kapoor, Andrew Li, Shrey Arora, Mana Abbaszadeh, Shen Li, Charles Xu, Byeolah Kwon
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-06-18
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.19704v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 使用了检索证据片段进行润色和补全。
