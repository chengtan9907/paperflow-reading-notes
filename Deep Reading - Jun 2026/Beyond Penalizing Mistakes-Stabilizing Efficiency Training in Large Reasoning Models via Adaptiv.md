---
user_id: "cheng tan"
paper_id: 1250
arxiv_id: "2606.22716"
title: "Beyond Penalizing Mistakes: Stabilizing Efficiency Training in Large Reasoning Models via Adaptive Correct-Only Rewards"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.22716.pdf"
pdf_url: "https://arxiv.org/pdf/2606.22716"
abs_url: "https://arxiv.org/abs/2606.22716"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:28:22"
---
# Beyond Penalizing Mistakes: Stabilizing Efficiency Training in Large Reasoning Models via Adaptive Correct-Only Rewards

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · reasoning efficiency · reward collapse · group relative policy optimization

## 一句话总结

本文提出ACOER（自适应正确-唯一效率奖励）方法，通过正确-唯一长度信号、动态预算归一化和控制环惩罚调整，稳定地训练大型推理模型实现高效推理，避免奖励崩溃和随机压缩失效。

## 摘要

> Training large language models to reason efficiently is a critical challenge. While integrating length-penalizing rewards into Group Relative Policy Optimization (GRPO) aims to reduce verbosity, it frequently triggers reward collapse, severely degrading reasoning capabilities. Through a systematic evaluation of various reward configurations, we identify the root mechanism: GRPO's group normalization creates divergent advantages when incorrect answers receive continuous length penalties. Consequently, methods penalizing the length of incorrect answers are structurally prone to collapse under sustained optimization. Furthermore, restricting penalties exclusively to correct answers avoids this primary failure, but leaves the model susceptible to a stochastic collapse driven by response over-compression. To robustly prevent both failure modes, we propose ACOER (Adaptive Correct-Only Efficiency Reward). ACOER eliminates the structural penalty loop by isolating brevity bonuses to correct completions and prevents stochastic compression via dynamic budget normalization and control-loop penalty adjustments. Evaluated across diverse mathematical reasoning benchmarks, ACOER improves overall accuracy compared to the base model while reducing token generation by over 60%, establishing a fundamentally stable approach for efficiency-aware optimization.

Q1: 这篇论文试图解决什么问题？

1. **核心问题**：训练大型推理模型（如DeepSeek-R1、Qwen3）时，为减少冗长响应引入的长度惩罚奖励在GRPO中经常导致奖励崩溃，严重削弱推理能力。
2. **结构失效机理**：当错误答案持续受到长度惩罚时，GRPO的组归一化会使错误答案的优势变得发散，形成“错误惩罚循环”，使模型倾向于生成越来越短的错误响应，最终崩溃。
3. **随机失效隐患**：若仅对正确答案施加长度奖励（正确-唯一策略），虽然避免结构性崩溃，但可能导致模型过度压缩正确答案，产生随机压缩失效，表现为响应过于简洁而牺牲正确性。
4. **挑战**：需要一种稳定、自适应的奖励机制，同时避免两种失效模式，且不牺牲性能。

Q2: 有哪些相关研究？

1. **推理模型与效率**：DeepSeek-R1 (DeepSeek-AI et al., 2025) 和 Qwen3 (Yang et al., 2025) 通过强化学习生成详细推理链，在数学推理上表现优异，但产生大量冗余token，计算成本高。
2. **效率优化方法**：先前工作尝试在RL训练中引入长度惩罚（如token预算、长度正则化），但未系统分析其对GRPO稳定性的影响。
3. **GRPO失效分析**：本文首次明确指出连续错误答案长度信号是奖励崩溃的结构性驱动因素，并识别出正确-唯一策略下的随机压缩风险。
4. **自适应奖励机制**：现有自适应奖励方法（如动态权重调整）多针对准确性，缺少专门针对效率训练中双重失效的解决方案。

Q3: 论文如何解决这个问题？

本文提出ACOER（Adaptive Correct-Only Efficiency Reward），包含三个协同机制：
1. **正确-唯一长度信号**：仅对正确完成的答案施加简洁奖励（即惩罚长度），对错误答案不施加长度惩罚，从而切断结构性错误惩罚循环。
2. **动态预算归一化**：引入动态token预算，根据问题难度或模型表现自适应调整归一化因子，防止正确响应过度压缩。
3. **控制环惩罚调整**：通过反馈控制循环监测奖励变化，动态调整惩罚强度，进一步稳定训练过程，避免随机波动导致压缩崩溃。
该方法在GRPO框架下实现，直接替代原始长度惩罚项。

Q4: 论文做了哪些实验？

1. **实验设置**：使用Qwen3-1.7B作为基础模型，在多个数学推理基准（如GSM8K、MATH、AIME等）上评估。
2. **对比基线**：包括原始GRPO、GRPO+固定长度惩罚、仅正确-唯一长度奖励（无自适应）等。
3. **评估指标**：准确率（Accuracy）、平均token生成数（Efficiency）、奖励稳定性（是否崩溃）。
4. **实验范围**：主要报告了准确率和效率的trade-off，并记录训练过程中奖励的变化轨迹。
5. **消融研究**：验证三个组件各自贡献，以及不同超参数（如预算阈值、控制环增益）的影响。

Q5: 发现了什么实验现象？

1. **结构性崩溃确认**：原始GRPO+长度惩罚在训练早期准确率提升，但随后奖励迅速崩溃，模型输出趋向极短错误回答。
2. **正确-唯一策略的随机崩溃**：仅对正确答案施加长度奖励时，训练中期出现突然的性能下降，表现为正确答案被过度压缩（输出过短但错误），呈现随机波动。
3. **ACOER的稳定性**：ACOER在整个训练过程中保持奖励平稳上升，准确率持续提高，token生成量减少超过60%，且未出现崩溃。
4. **效率-准确率权衡**：ACOER在减少token的同时，准确率相比基础模型略有提升（约1-2%），主要优势在于token节省（降低至原来40%以下）。
5. **Cross-Benchmark差异**：效果在难题上（如AIME）token减少更显著，在简单题上（如GSM8K）准确率提升较小，表明依赖难度。

Q6: 有什么可以进一步探索的点？

1. **模型规模泛化**：当前仅测试Qwen3-1.7B，需要验证在更大模型（如7B、70B）上ACOER的有效性和稳定性。
2. **其他任务域**：除数学推理外，可探索在编程、科学问答等需要长推理的任务上的应用。
3. **自适应机制的改进**：动态预算归一化和控制环的进一步自动化（如元学习参数）。
4. **跨benchmark分析**：更系统地研究难度依赖的token节省模式，设计针对性策略。
5. **与其他效率技术的结合**：如思维链压缩、蒸馏等，进一步提升效率。
6. **理论分析**：从收敛性和博弈论角度分析双重失效的阿利亚斯悖论。

Q7: 总结一下论文的主要内容

本文聚焦于大型推理模型在效率训练中的稳定性问题。作者识别出两种奖励崩溃模式：结构性崩溃（由对错误答案持续施加长度惩罚引发）和随机崩溃（由仅对正确答案施加长度惩罚导致的过度压缩）。为解决这些失效，提出ACOER，一种自适应正确-唯一效率奖励方法，包含三个机制：正确-唯一长度信号、动态预算归一化和控制环惩罚调整。在Qwen3-1.7B上的数学推理基准实验中，ACOER不仅避免了奖励崩溃，还提高了准确率，同时将token生成减少超过60%。工作强调了效率优化中稳定性设计的重要性，为后续高效推理训练提供了可靠基线。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本文与生成方向相关（用户偏好中generation权重0.1），关注推理效率优化。

## 基本信息

- 作者：Jungseob Lee, Seungyoon Lee, Seongtae Hong, Minhyuk Kim, Chanjun Park, Heuiseok Lim
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.22716`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本生成参考了PDF检索的证据片段，并综合论文摘要、结论等部分进行填充。
