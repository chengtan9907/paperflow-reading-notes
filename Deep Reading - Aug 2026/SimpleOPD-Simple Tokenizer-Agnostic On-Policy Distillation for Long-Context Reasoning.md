---
user_id: "cheng tan"
paper_id: 7903
arxiv_id: "2608.14277"
title: "SimpleOPD: Simple Tokenizer-Agnostic On-Policy Distillation for Long-Context Reasoning"
publish_date: "2026-08-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.14277.pdf"
pdf_url: "https://arxiv.org/pdf/2608.14277"
abs_url: "https://arxiv.org/abs/2608.14277"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-18T02:12:15"
---
# SimpleOPD: Simple Tokenizer-Agnostic On-Policy Distillation for Long-Context Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · long-context reasoning · cross-tokenizer alignment · knowledge distillation

## 一句话总结

本文提出 SimpleOPD，一种与分词器无关的 on-policy 蒸馏方法，将长上下文推理教师模型 SU-01 的证明推理能力迁移到短上下文学生模型，通过共享文本空间下的跨分词器对齐、学生参考 KL 损失以及终止特殊 token advantage 掩码，缓解了分词器不匹配、师生分布漂移和响应长度爆炸等问题，并在多个同族/异族学生模型上取得一致的数学推理提升。

## 摘要

> On-policy distillation (OPD) offers a promising way to transfer reasoning capabilities from stronger teacher models, but applying it to long-context reasoning teachers and short-context students introduces practical challenges, including tokenizer mismatch, teacher-student distribution mismatch, response length explosion, and training instability. In this work, we study this setting by transferring proof-reasoning capabilities from the long-context reasoning model SU-01 to short-context student models. To handle tokenizer differences, we perform OPD in a shared text space and align only tokens that occupy identical text spans under the student and teacher tokenizers. To mitigate the problem of excessive generation length and frequent truncation, we introduce a student reference KL loss and mask the advantages of special termination tokens such as $ $ and $ $ . This strategy constrains the student from drifting excessively from its initial policy, thereby mitigating the teacher-student distribution mismatch problem and fostering steady length growth. Experiments on both same-family and different-family student models, including Qwen3, Qwen3.5, Intern-S2, GLM-4.7, Gemma-4, show consistent gains in mathematical reasoning, especially natural-language math proving. Notably, Intern-S2-Preview improves by 21.2 points on ProofBench, reaching 55.2 and surpassing Gemini-2.5-Pro. It also improves on science benchmarks such as HLE and HiPhO, suggesting that OPD transfers reasoning capabilities that generalize beyond the mathematical training domain.
> Project Page
> Code
> Models
> August, 2026

Q1: 这篇论文试图解决什么问题？

1. **核心问题**：如何将长上下文推理教师模型（如 SU-01）的推理能力有效地蒸馏到短上下文学生模型。这与常见的同构蒸馏（师生具有相似架构和上下文长度）不同，引入了一系列新的实际困难。
2. **分词器不匹配**：教师和学生模型可能使用不同的 tokenizer，导致相同文本在 token 序列上无法直接对齐，标准 OPD 通常依赖共享 token 空间或 logits 对齐，此处失效。
3. **师生分布不匹配**：教师生成的推理过程长度和能力远超学生，直接监督可能导致学生难以模仿，产生分布漂移，即学生策略被拉向教师的高能力区域而失去自身稳定性。
4. **响应长度爆炸**：教师的长推理轨迹如果被直接用作目标，学生可能被迫生成极长响应，超出其上下文长度，导致频繁截断和训练不稳定。
5. **训练不稳定**：上述因素综合导致 OPD 在长→短蒸馏中训练不稳定，表现为 loss 振荡、生成退化等。
6. **特殊终止 token 问题**：教师监督会反复抑制学生的终止思考或终止回答的特殊 token（如 < / think > 和 < |im_end| > ），导致学生难以结束生成，进一步加剧长度失控。
7. **能力泛化问题**：蒸馏得到的推理能力是否只局限于数学训练领域，还是能泛化到更广泛的科学推理任务，是论文关注的评价性问题。

Q2: 有哪些相关研究？

1. **On-policy 蒸馏（OPD）**：OPD 是 LLM 后训练中的一种范式，典型工作包括 Gu et al. (2024) 和 Agarwal et al. (2024)。其核心是从当前学生策略采样响应，由教师评分或生成目标，从而在线更新学生策略，避免 off-policy 分布偏移。
2. **强到弱蒸馏（strong-to-weak distillation）**：相关方向包括 Lu & Lab (2025) 等，研究如何用强模型指导弱模型，但通常不涉及长上下文与短上下文的差异。
3. **多教师蒸馏**：Ma et al. 等工作探索多教师集成，但 OPD 的单教师长→短设定仍较少被研究（Sun et al., 2026 指出该领域尚待探索）。
4. **跨分词器对齐**：本文提出的共享文本空间对齐方法属于跨 tokenizer 知识蒸馏的技术路线，相关工作通常需要在离散 token 空间中桥接不同词表。
5. **长上下文推理模型**：SU-01 是一个基于 Qwen3-30B-A3B 的 30B-A3B 推理模型，具有强大的数学证明能力，在奥林匹克级别评测上达到金牌水平。长上下文推理模型通常能处理更复杂的推理链条，但部署成本高，因此蒸馏到短上下文模型有实际意义。
6. **推理能力迁移**：与直接 SFT 或拒绝采样蒸馏不同，OPD 更强调在线策略优化，本文方法属于该框架下的扩展。

Q3: 论文如何解决这个问题？

1. **共享文本空间 OPD**：针对 tokenizer 不匹配，模型在文本空间（而非 token 空间）进行蒸馏。具体地，将学生采样得到的响应在文本层面切分为与教师 tokenizer 对齐的跨度，只对在学生和教师 tokenizer 下占据完全相同文本跨度的 token 计算蒸馏损失。这样既保留了文本信息，又绕开了词表差异。
2. **学生参考 KL 损失**：为了抑制分布漂移和长度爆炸，引入一个以学生初始策略为参考的 KL 正则项，约束学生更新后的策略不要过度偏离初始策略。这类似于 KL 约束的 on-policy 优化，使蒸馏过程更稳定。
3. **终止 token advantage 掩码**：对特殊终止 token（如 < / think > 和 < |im_end| > ）的 advantage 进行掩码，即不通过对这些 token 的梯度来抑制终止行为。教师监督可能反复惩罚学生提前结束推理或回答，导致学生无法结束生成；掩码后学生可以自主决定终止时机，从而保持合理的响应长度。
4. **训练协议**：实验中使用 SU-01 作为教师，学生从自身策略采样响应，教师对响应进行评分或提供目标，执行 OPD。具体训练数据和实现细节在论文 3.1 和 3.2 节给出（证据中仅列出标题，具体数据未提供）。
5. **方法定位**：本文强调“简单”（Simple），即不引入复杂架构或额外模块，只在 OPD 目标函数和 token 对齐层面做针对性修改。

Q4: 论文做了哪些实验？

1. **训练数据**：论文第 3.1 节描述训练数据，但语义检索未提供具体内容，合理推测为数学证明和推理相关的高质量数据。
2. **实现细节**：第 3.2 节给出 OPD 的具体配置，包括学生响应采样、教师监督方式等。证据仅提到“使用学生策略采样的响应，SU-01 作为教师”，SU-01 是 30B-A3B 模型。
3. **学生模型**：同族模型包括 Qwen3、Qwen3.5、Intern-S2；异族模型包括 GLM-4.7、Gemma-4。这些模型的规模和上下文长度不同。
4. **评测基准**：ProofBench（数学证明）、HLE（人类最后考试）、HiPhO（物理竞赛）等。
5. **对比基线**：包括直接使用教师模型做推理（如 Gemini-2.5-Pro 作为参考）、未蒸馏的 baseline 等。合理推断会有 SFT 或 off-policy 蒸馏的对照。
6. **实验范围**：数学推理，尤其是自然语言数学证明；科学基准 Beyond 数学领域。
7. **稳定性分析**：论文第 4.1 节标题为“训练不稳定性 in On-Policy Distillation”，说明作者专门分析了 OPD 训练中的不稳定现象以及本文方法如何缓解。

Q5: 发现了什么实验现象？

1. **一致收益**：在所有测试学生模型（Qwen3、Qwen3.5、Intern-S2、GLM-4.7、Gemma-4）上，数学推理能力均获得一致提升，证明方法具有跨模型族泛化性。
2. **显著提升案例**：Intern-S2-Preview 在 ProofBench 上提升 21.2 分，达到 55.2，并超过 Gemini-2.5-Pro。这是一个很强的结果，说明 OPD 可以带来大幅改进，而非小幅调优。
3. **跨领域泛化**：在科学基准 HLE 和 HiPhO 上也观察到提升，表明蒸馏出的推理能力不只是记忆数学模式，而是更一般的推理能力，可以迁移到科学问题解决。
4. **长度控制效果**：通过学生参考 KL 损失和终止 token 掩码，响应长度实现“稳步增长”，说明方法有效避免了长度爆炸和过早截断。
5. **训练稳定性**：第 4.1 节专门讨论训练不稳定性，合理推测作者观察到未加这些约束时 OPD 出现 loss 振荡或发散，加入约束后训练稳定。
6. **分布漂移缓解**：学生参考 KL 损失约束学生不偏离初始策略，实验表明这种约束有助于缓解师生分布不匹配。
7. **负结果或权衡**：证据中未提及负结果，但合理推测存在一些模型或任务上提升较小的情况，论文未给出完整细节。

Q6: 有什么可以进一步探索的点？

1. **扩展到更长上下文学生**：当前实验聚焦短上下文学生，未来可将方法用于中等长度或动态上下文场景。
2. **更大规模教师-学生组合**：SU-01 是 30B-A3B 规模，可探索更大教师（如 100B+）蒸馏到 7B 或更小模型的极限。
3. **跨领域蒸馏的机制分析**：论文观察到科学基准的提升，但深层机制仍需研究，如蒸馏是否传递了证明结构、搜索策略或自我纠正能力。
4. **改进终止 token 处理**：当前掩码是一种硬性处理，未来可设计 soft 权重或学习终止阈值。
5. **与其他正则化结合**：学生参考 KL 损失可与 TRPO/PPO 风格约束、梯度裁剪等技术结合，提高稳定性。
6. **多教师 OPD**：在长上下文教师基础上增加多个教师，利用多教师投票或集成来提升蒸馏质量。
7. **动态 length 预算**：为不同难度问题分配不同长度预算，优化推理长度与准确率的权衡。
8. **理论分析**：对跨 tokenizer 对齐、KL 约束和终止 token 掩码给出理论解释，例如与 RLHF 中 KL 惩罚的关系。
9. **实际部署**：将蒸馏得到的短上下文模型部署到边缘或实时场景，评测其推理速度和内存占用。
10. **扩展到非数学推理**：将方法用于代码、法律、医疗等需要长链推理的领域。

Q7: 总结一下论文的主要内容

本文研究从长上下文推理教师模型向短上下文学生模型进行 on-policy 蒸馏（OPD）的挑战与解决方案。论文的核心动机是：长上下文推理模型（如 SU-01）在数学证明等复杂推理任务上表现出色，但其推理过程长、计算开销大，直接使用不现实；而短上下文模型部署友好，但能力不足。OPD 提供了一种在线蒸馏范式，理论上可以传递推理能力，但当教师和学生规格不匹配时，会引入分词器不匹配、师生分布不匹配、响应长度爆炸和训练不稳定等实际问题。论文以 SU-01（30B-A3B，基于 Qwen3-30B-A3B，具有奥林匹克级别数学证明能力）为教师，向多种同族和异族学生模型（Qwen3、Qwen3.5、Intern-S2、GLM-4.7、Gemma-4）蒸馏。

方法上，论文提出三个关键设计：（1）跨分词器对齐：在共享文本空间中执行 OPD，只对齐在教师和学生 tokenizer 下占据相同文本跨度的 token，从而消除词表差异的影响；（2）学生参考 KL 损失：约束学生策略不偏离初始策略，防止教师长输出把学生拉向无法适应的分布，同时抑制响应长度爆炸；（3）终止特殊 token 的 advantage 掩码：对 < / think > 和 < |im_end| > 等终止 token 不施加优势信号，避免教师监督反复惩罚学生终止行为，从而让学生自主决定何时结束生成，帮助长度稳步增长。这些设计都旨在保持训练稳定性和学生自身能力的延续性。

实验方面，论文在数学推理（尤其是自然语言数学证明）和科学基准（HLE、HiPhO）上评测。核心结果是 Intern-S2-Preview 在 ProofBench 上提升 21.2 分到 55.2，超越 Gemini-2.5-Pro；所有学生模型均获得一致性提升，且提升泛化到科学推理任务。论文还专门分析了训练不稳定性问题（实验章节 4.1），说明该工作是针对实际训练难题的工程性研究。

论文的主要贡献包括：首次系统研究长→短上下文 OPD 蒸馏的挑战；提出简单有效的 tokenizer-agnostic 对齐方法、学生参考 KL 约束和终止 token 掩码；在多个同族/异族模型上展示跨领域泛化能力。局限性在于证据中仅能确认部分细节，如训练数据规模、具体超参数、消融实验完整结果等尚未从检索片段获得，需要阅读全文确认。

整体而言，这是一项实践性强的技术报告，解决了一个真实部署难题：如何将昂贵的长上下文推理模型压缩为短上下文模型而不损失推理质量。其方法简单（SimpleOPD），没有引入复杂架构，容易复现，适合作为知识蒸馏和推理能力迁移方向的重要参考。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 AI for Science 方向相关：论文展示了从长上下文推理模型蒸馏到更高效模型的能力，且实验显示在科学基准 HLE 和 HiPhO 上获得提升，说明这种蒸馏可以用于科学推理模型的紧凑化部署。

## 基本信息

- 作者：Haonan He, Haodi Lei, Yun Luo, Haoran Zhang, Shunkai Zhang, Yizhuo Li, Shengji Tang, Zhilin Wang, Runzhe Zhan, Lei Bai, Ganqu Cui, Fangchen Yu, Yafu Li, Peng Ye, Ning Ding, Yu Cheng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-08-17
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.14277`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要基于论文摘要和检索到的语义片段（Abstract、Introduction、Method、Conclusion 等），并结合启发式草稿进行补充；部分实验细节（如训练数据量、精确消融结果）因证据缺失而标注为合理推断或推测，建议回原文核对。
