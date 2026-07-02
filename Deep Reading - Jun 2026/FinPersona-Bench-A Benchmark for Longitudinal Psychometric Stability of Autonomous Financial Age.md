---
user_id: "cheng tan"
paper_id: 2265
arxiv_id: "2606.31522v1"
title: "FinPersona-Bench: A Benchmark for Longitudinal Psychometric Stability of Autonomous Financial Agents"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.31522v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.31522v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T02:09:11"
---
# FinPersona-Bench: A Benchmark for Longitudinal Psychometric Stability of Autonomous Financial Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · financial agents · mandate salience decay · behavioral consistency

## 一句话总结

论文提出了FinPersona-Bench，一个用于评估自主金融智能体长期行为一致性（Mandate Salience Decay）的仿真基准，通过合成市场实验发现行为指令随市场环境逐渐失效，且模型间存在显著差异。

## 摘要

> Large Language Models (LLMs) are increasingly deployed as autonomous financial agents initialized with explicit behavioral mandates such as "preserve capital" or "avoid speculative bets" that are meant to govern every decision throughout deployment. In practice, however, as market context accumulates over long horizons, these mandates gradually lose their behavioral influence, a phenomenon we formalize as Mandate Salience Decay (MSD). To measure MSD objectively, we introduce FinPersona-Bench, a simulation benchmark in which a synthetic market decouples observable price from hidden fundamental value, enabling falsifiable evaluation across three failure modes: trading without signal in calm markets, panic-selling during crashes, and ignoring fundamental value during speculative bubbles. Evaluating 18 leading frontier and open-source LLMs, each assigned one of three behavioral profiles ranging from strict capital preservation to aggressive growth, shows that MSD compounds over time and is model-dependent. In crash scenarios, the behavioral gap between static agents and those receiving periodic mandate re-grounding grows 4.4x from the first to the final quarter of the simulation. The effects of mandate re-grounding are not uniformly positive: it consistently helps conservative agents in low-signal markets but actively worsens behavior for aggressive agents in the same setting. These findings suggest that reliable long-horizon deployment requires selective, mandate-aware re-grounding based on agent profile and market regime.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决LLM作为自主金融智能体在长期部署中出现的行为指令一致性衰减问题。具体来说，现有研究主要关注LLM在单步或短周期交易任务中的表现，忽视了初始行为指令（如保本、增长）在长期市场互动中逐渐失效的现象。作者将这一行为失败形式化为Mandate Salience Decay，并指出该现象与局部推理错误不同：即使在单个交易中模型能做出理智决策，也可能因指令被上下文稀释而违反核心约束。论文的核心问题是：如何在可控的、可证伪的仿真环境中量化评估这种长期行为漂移？以及不同模型、不同初始行为倾向、不同市场状态下的MSD表现有何差异？

Q2: 有哪些相关研究？

论文将FinPersona-Bench放在三类已有研究背景中：1）金融LLM基准：现有基准大多基于静态问答或单一交易，缺乏对行为指令长期一致性的评估；2）行为漂移：关注LLM在对话或任务中偏离初始设置的现象，但多集中在非金融领域；3）记忆机制：一些工作通过外部记忆或定期提示来维持行为一致性，但缺乏系统性比较。FinPersona-Bench提供了第一个专门评估金融智能体指令显著性衰减的仿真平台，并区分了MSD与一般推理错误的不同。

Q3: 论文如何解决这个问题？

论文提出了FinPersona-Bench，一个合成市场仿真基准。其核心设计包括：1）合成市场：将可观测价格与隐藏基本面价值解耦，允许在平静市场、崩盘和投机泡沫三种场景中测试智能体的行为坚持程度。平静市场中基本面无信号，崩盘中价格暴跌，泡沫中价格远超基本面。2）行为倾向分配：为每个LLM分配三种MBTI衍生行为倾向之一：保守（倾向于保本）、平衡、激进（倾向于增长）。3）失败模式定义：三种可测量的失败模式——无信号交易（在平静市场中没有基本面信号时违规交易）、恐慌抛售（崩盘时不顾保本指令抛售）、忽略基本面（泡沫中忽视高估信号而追涨）。4）评估协议：每个智能体运行多个仿真回合，记录交易决策与指令的偏离程度。作者在两种设置下运行智能体：静态（无重新接地）和动态（每回合提供指令提醒）。通过比较两组的行为差距，量化MSD的程度。

Q4: 论文做了哪些实验？

论文设计了合成市场仿真实验，评估了18个前沿和开源LLM，包括GPT-4o、Claude-3.5、Llama-3-70B等。每个模型被赋予三种行为倾向（严格保本、均衡、激进增长）。实验包括三种市场场景：平静（无显著趋势，基本面价值不变）、崩盘（价格急剧下跌，基本面不变）、泡沫（价格急剧上涨，基本面不变）。每个场景运行多个时间步（模拟季度）。主要实验测量静态智能体与接收周期指令重新接地（re-grounding）的智能体在行为表现上的差距。特别关注行为差距随时间（从第一季到第四季）的变化。此外，还测量了不同场景下MSD的幅度、模型依赖性、以及指令重新接地对不同行为倾向智能体的差异化影响。未提供的数据包括具体交易次数、置信区间等，但定性结果表明差距扩大4.4倍。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：1）MSD随时间累积：所有模型在长期模拟中都出现了指令显著性衰减，且衰减程度随模拟时间增加而增大。2）模型依赖性：不同模型对指令的坚持程度差异显著，GPT-4o相对稳定，而一些开源模型衰减更快。3）崩盘场景中行为差距最大：从第一季到第四季，静态智能体与重新接地智能体的行为差距扩大了4.4倍，表明在市场压力下MSD加剧。4）指令重新接地效果不对称：对于保守型智能体，在低信号（平静）市场中重新接地始终有帮助，能有效防止无信号交易；但对于激进型智能体，重新接地反而主动恶化了行为（可能因为提醒抑制了其本就合理的冒险）。5）在泡沫场景中，重新接地对保守型帮助有限，因为保本指令本身与抑制冒险一致，但激进型在泡沫中更易忽略基本面。这些发现表明，简单的统一重新接地策略不可取，需要智能体感知和市场自适应。

Q6: 有什么可以进一步探索的点？

论文在结论和局限性部分指出了多个未来方向：1）扩展MBTI人格选择，包含神经质等维度；2）测试更短的指令提醒间隔和事件触发的重新注入策略；3）增加更多开源模型，尤其是中文环境模型；4）探索动态重新接地策略，根据当前市场状态和智能体历史行为自适应决定是否提醒；5）将基准扩展到多智能体博弈和真实市场数据；6）研究MSD与模型规模、训练数据分布的关系；7）评估不同记忆增强结构（如外挂记忆系统）对缓解MSD的效果。

Q7: 总结一下论文的主要内容

论文提出了FinPersona-Bench，一个用于评估自主金融智能体长期行为一致性损失的仿真基准。作者形式化了“指令显著性衰减”（MSD）现象，即LLM初始行为指令在长期市场互动中逐渐失去约束力。基准通过合成市场将价格与基本面解耦，定义了三种可证伪的失败模式：无信号交易、恐慌抛售和忽略基本面。作者评估了18个LLM（包括商业和开源模型），每个模型被赋予三种行为倾向（保守、平衡、激进），在平静、崩盘和泡沫三种市场场景中进行模拟。关键发现包括：MSD随时间累积且具有模型依赖性；在崩盘场景中，静态智能体与定期重新接地智能体的行为差距从第一季到第四季扩大了4.4倍；指令重新接地对保守智能体在低信号市场中始终有益，但对激进智能体在相同场景下反而有害。论文还讨论了相关研究、局限性（如人格选择不完整、开源模型覆盖有限）和未来方向（动态重新接地、事件驱动提醒等）。该工作为金融LLM智能体的长期可靠性提供了首个量化评估方法，强调了选择性、智能体感知的指令维持策略的必要性。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该论文的核心主题是LLM作为自主智能体在长期任务中的行为稳定性，直接对应研究画像中的智能体方向（权重0.10）。

## 基本信息

- 作者：Muhammad Usman Safder, Ayesha Gull, Rania Elbadry, Fan Zhang, Yankai Chen, Xueqing Peng, Xue, Liu, Preslav Nakov, Zhuohan Xie
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-06-30
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.31522v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答基于论文摘要、启发式草稿以及PDF语义检索命中的证据片段（包括Abstract、结论、贡献、相关工作和局限性部分）进行生成，优先采用了field_evidence_map中得分较高的片段。
