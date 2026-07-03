---
user_id: "cheng tan"
paper_id: 2632
arxiv_id: "2607.02255"
title: "AgenticSTS: A Bounded-Memory Testbed for Long-Horizon LLM Agents"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02255.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02255"
abs_url: "https://arxiv.org/abs/2607.02255"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T22:18:30"
---
# AgenticSTS: A Bounded-Memory Testbed for Long-Horizon LLM Agents

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：bounded memory contract · llm agent · long-horizon decision making · typed retrieval

## 一句话总结

本文提出一种有界记忆合约（bounded memory contract），每个决策由类型检索组装的新用户消息驱动，而非累积历史转录，并在Slay the Spire 2中实例化，释放一个可复现的LLM智能体长程决策测试床。

## 摘要

> Memory for a long-horizon LLM agent is a contract about what each future decision is allowed to see. The simplest contract appends past observations, tool calls, and reflections to every prompt, which makes prior context easy to access but also turns it into a jumbled mixture in which the effect of any single memory component is hard to isolate. We introduce and instrument an alternative bounded contract: every decision is made from a fresh user message assembled by typed retrieval, with no raw cross-decision transcript appended. The prompt thus stays bounded across runs of any length, and any single layer can be ablated in isolation. We instantiate the contract in Slay the Spire 2, a closed-rule stochastic deck-building game whose runs require hundreds of tactical and strategic decisions. A public online benchmark of frontier LLMs on the same game reports zero wins at the lowest difficulty across five configurations, and the developer-reported human win rate at the same difficulty is 16%; the task is hard but not saturated. Within our harness, a fixed-A0 ablation shows the largest observed difference when triggered strategic skills are enabled: the no-store baseline wins 3/10 games and adding the skill layer 6/10. At this sample size the comparison is directional rather than statistically decisive (Fisher exact p\approx0.37); a cross-backbone probe and public accumulating-context baselines are reported as operational comparisons rather than controlled tests of the contract variable itself. We release a reproducible testbed: 298 completed trajectories with condition tags, frozen memory/skill snapshots, prompt records, and analysis scripts -- an agent design and a validated, reusable methodology for studying how explicit memory layers shape long-horizon LLM-agent decisions.

Q1: 这篇论文试图解决什么问题？

当前长程LLM智能体普遍采用将历史观察、工具调用和反思直接追加到下一提示的简单记忆合约。这种方式虽然使先前上下文易于访问，但也导致所有记忆组件混杂在一起，无法独立评估每个组件（如状态记忆、策略技能、行动反思等）对决策的具体贡献。缺乏可消融、有界的记忆接口，使得系统性地研究记忆结构对长程决策的影响变得困难。论文试图设计一种记忆合约，使每个决策的输入都是有界、类型化、可独立消融的，从而为记忆机制提供可控的实验框架。

Q2: 有哪些相关研究？

相关工作围绕四个研究线索展开：
1. Prompt-History智能体：将完整操作历史直接拼入提示，如ReAct、Reflexion、AdaPlanner等，优点是简单但难以隔离记忆组件。
2. 外部化记忆：使用检索增强生成（RAG）或记忆流（MemoryStream）来管理长期记忆，但通常仍保留部分历史在提示中，消融粒度粗。
3. 技能库：如Voyager、Ghost in the Minecraft，通过存储和调用技能程序来模块化行为，但技能提取和触发机制常与记忆合约耦合。
4. 长程游戏测试床：如NetHack、Minecraft、ALFWorld等，但多数缺乏完全的规则封闭性、可枚举性和LLM可读性，且胜率已趋于饱和。
本文针对这些线索的共同问题：哪些先验经验片段进入每个决策，以及该选择是否可系统消融。

Q3: 论文如何解决这个问题？

论文的核心解决方案是引入一种有界记忆合约（bounded memory contract），具体表现为“类型化检索组装”范式：
- 每个决策步骤不再拼接历史转录，而是从预设的五类知识层（L1-L5）中通过类型化检索动态组装成一个“用户消息”。
- 五层知识：L1 操作提示（不可变的角色与协议模板）、L2 状态类型化提示（当前战斗/地图等状态的不可变模式）、L3 状态存储器（动态更新但只保留当前状态）、L4 前运行记忆（跨决策的摘要或技能记录，类型化）、L5 触发策略技能（基于当前状态动态激活的微型程序）。
- 所有层均可独立启用/禁用（消融），且提示大小不随运行长度增长。
- 该合约在Slay the Spire 2中实例化：游戏具有封闭、可枚举、LLM可读的规则空间（576张卡牌、293件遗物等），且需要数百步决策。

Q4: 论文做了哪些实验？

实验在Slay the Spire 2上进行，使用以下设置：
1. 固定A0消融实验：在固定骨干网络（A0）上，比较无存储基线（仅L1-L3）与添加L5技能层（触发策略技能）的表现。每个条件运行10局游戏，评估胜率。
2. 跨骨干网络探测：在不同LLM骨干（如GPT-4、Claude等）上运行相同的条件，比较不同模型对记忆合约的敏感性。
3. 累积上下文基线：将公共在线基准（使用简单历史追加的智能体）的结果作为操作比较参考。
所有实验均在最低难度进行。共释放298条已完成轨迹，附带条件标签、冻结快照和提示记录。

Q5: 发现了什么实验现象？

实验观察到以下现象：
1. 在固定A0消融中，无存储基线（仅基础层）在10局中胜3局，添加L5技能层后胜6局，提升幅度较大但Fisher精确检验p≈0.37，未达统计显著。该差异方向性提示技能层可能有益，但受限于样本量。
2. 公共在线基准显示前沿LLM（如GPT-4、Claude等）在同一游戏最低难度下零胜（5个配置均未获胜），而开发者报告的人类胜率为16%，表明任务对当前LLM仍极具挑战性，远未饱和。
3. 跨骨干网络探测表明不同模型对记忆合约的总体表现有影响，但未提供统计比较，仅作为操作参考。
4. 累积上下文基线（历史追加方法）在类似难度下表现亦不佳，进一步支持当前合约的竞争力。

Q6: 有什么可以进一步探索的点？

1. **扩大样本量和统计效能**：当前固定A0消融仅10局，不足以得出统计结论；需要更大规模实验以确认记忆层效果。
2. **扩展到更多游戏和领域**：有界合约基于turn-based决策，可推广到其他封闭规则游戏（如NetHack、卡牌游戏）或非游戏任务（如代码任务、对话系统）。
3. **视觉输入整合**：当前合约假设状态以文本描述输入，需研究如何融入视觉观察（如截图），同时保持有界性。
4. **连续控制与流式输入**：合约针对turn-based设计，对于连续或流式控制循环需要调整检索触发机制。
5. **训练记忆组件**：当前评估是training-free，未来可微调或强化学习优化L5技能或L4记忆摘要。
6. **更细粒度的消融**：分离L3状态存储器的不同字段，研究哪些状态信息对决策最关键。

Q7: 总结一下论文的主要内容

论文题为《AgenticSTS: A Bounded-Memory Testbed for Long-Horizon LLM Agents》。主要贡献是提出并实现了一种有界记忆合约（bounded memory contract），用于长程LLM智能体的记忆管理，并在Slay the Spire 2游戏中构建了一个可复现的测试床。

**论证主线**：当前智能体记忆机制（如简单历史追加）难以隔离和消融，导致记忆组件影响不可控。作者将记忆重新定义为“每个未来决策允许看到什么的合约”，并设计了一种替代方案：每个决策的输入由类型化检索组装，不保留跨决策的原始转录，从而保证提示大小有界且各层可独立消融。

**技术主线**：
- 定义五类知识层（L1-L5），涵盖角色模板、状态模式、状态记忆、跨决策记忆、触发技能。
- 在Slay the Spire 2中实现该合约：游戏具有封闭规则、可枚举实体（576卡牌、293遗物等），适合LLM决策。
- 释放完整数据包：298条轨迹、记忆/技能快照、提示记录和分析脚本，支持重现和扩展。

**实验主线**：
- 固定A0消融：无存储基线胜率3/10，加技能层6/10，但统计不显著（p≈0.37）。
- 公共基线：前沿LLM零胜，人类16%，验证任务难度。
- 补充跨骨干和累积上下文基线作为操作参考。

**局限**：
- 样本量小，统计推断弱。
- 仅限turn-based游戏，未涵盖连续控制、视觉输入。
- 无训练，依赖LLM固有能力。
- 合约设计针对特定任务结构，泛化性待验证。

总体而言，本文提供了一个精心设计的实验框架和生成资源，但核心发现仍处于方向性验证阶段，需要更大规模实验确认。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文聚焦长程LLM智能体的记忆结构，直接关联智能体研究方向（用户画像中agent权重0.10）。

## 基本信息

- 作者：Xiangchen Cheng, Yunwei Jiang, Jianwen Sun, Zizhen Li, Chuanhao Li, Xiangcheng Cao, Yihao Liu, Fanrui Zhang, Li Jin, Kaipeng Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-07-03
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02255`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括背景、方法、结果和局限性等片段。
