---
user_id: "cheng tan"
paper_id: 9150
arxiv_id: "2608.23098v2"
title: "Jiuge-Tuiqiao: An Interpretable Human-AI System for Classical Chinese Poetry Refinement"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23098v2.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23098v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:07:14"
---
# Jiuge-Tuiqiao: An Interpretable Human-AI System for Classical Chinese Poetry Refinement

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：classical chinese poetry · tuiqiao · human-ai collaboration · interactive poetry refinement

## 一句话总结

提出九歌-推敲（Jiuge-Tuiqiao），一个以“用户驱动控制—古籍引导证据—AI 辅助生成”三元模型为核心的可解释人机协作古诗推敲系统，将 AI 从自主生成器转变为支持作者迭代修改与声律检查的背景助手。

## 摘要

> Classical Chinese poetry composition has long valued Tuiqiao, the iterative refinement of words, imagery, and prosody. However, many current AI poetry systems follow a one-shot generation paradigm, which reduces users to prompt providers and weakens their creative agency. We present Jiuge-Tuiqiao $^{1}$ , an interactive human-AI collaborative system for classical Chinese poetry composition. The system is designed around a triadic model: user-driven control, ancient-guided evidence, and AI-assisted generation. Users can lock characters or lines, receive real-time prosody feedback, and obtain interpretable refinement suggestions grounded in high-frequency collocations, PPL-ranked classical lines, and structured knowledge extracted from classical encyclopedias. This design turns AI from an autonomous generator into a background assistant that supports the user's own process of poetic refinement. Preliminary experiments and user feedback indicate that Jiuge-Tuiqiao provides controllable refinement mechanisms, traceable literary evidence, and a positively received interactive experience for classical Chinese poetry composition.

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：在古典诗歌创作中，AI 系统如何参与“推敲”而不剥夺诗人的创作主体性。具体拆解为以下几个子问题：
1. 现有 AI 诗歌系统普遍采用一次性生成范式，用户只输入提示词并接收成品，交互更接近内容消费而非创作，削弱了用户的作者感（sense of authorship）。
2. 缺少对“推敲”这一迭代过程的显式建模：字词、意象、声律需要在约束下反复权衡，而一次生成无法体现这种过程。
3. 修改建议缺乏可解释性：用户不知道系统为什么换某个字，难以判断其文学依据，也难以信任和吸收建议。
4. 声律等硬约束缺少实时反馈机制，用户只能事后自行检查，打断创作流。
5. 广义上的“创作权与署名权”问题：当 AI 参与润色时，作品归属如何界定、AI 贡献如何透明化，尚未被现有系统妥善处理。
论文把“推敲”本身作为设计对象，而不是把生成结果当作终点，试图把用户从提示词提供者重新变回创作者。

Q2: 有哪些相关研究？

从检索到的 Related Work 片段看，已有研究尝试构建交互式诗歌生成系统，支持约束生成（constrained generation）和细粒度润色（fine-grained polishing）。Jiuge-Tuiqiao 的增量在于把修改建议背后的证据显性化（makes the evidence behind refinement suggestions visible），让用户不仅得到“改什么”，还能看到“为什么改”。此外，Introduction 部分批评一次性生成范式，强调这种范式让用户更接近内容消费者而非创作者。由于当前可获得的材料有限，无法列出具体代表论文、数据集或基线，这一部分需要结合原文完整核对。合理推断：系统命名 Jiuge-Tuiqiao 与“九歌”系列古典诗歌生成系统可能存在承接关系，属于同一研究脉络的交互化、可解释化扩展。

Q3: 论文如何解决这个问题？

论文采用三元模型（triadic model）来组织系统：
1. 用户驱动控制（user-driven control）：用户可锁定字符或整行，系统在锁定约束下生成或修改候选，保证创作方向始终由人掌握。
2. 古籍引导证据（ancient-guided evidence）：针对任意待修改位置，系统从三个知识源检索证据：
 - 高频共现（frequent co-occurrences）：从古诗语料统计字词搭配，提示常见、自然的用法；
 - 按 PPL 排序的经典诗句（PPL-ranked classical lines）：用语言模型困惑度对候选诗句进行排序，提供符合古典诗语言分布的诗句作为范例；
 - 古代类书的结构化知识（structured knowledge from classical encyclopedias）：从类书中抽取词藻、典故、名物等知识，为替换词提供文学依据。
3. AI 辅助生成（AI-assisted generation）：语言生成引擎与知识检索工具结合，根据用户反馈和知识约束优化输出，而不是直接代替用户写。
此外，系统还集成了动态声律检查机制（dynamic prosody checking），对平仄、押韵等要素提供实时反馈。整体上，系统将自己定位为“背景助手”，用户保留完整的创作主动权；AI 只提供选项和证据，绝不未经用户确认直接把文本插入作品。

Q4: 论文做了哪些实验？

从现有材料（摘要、Introduction、Methods 片段）看，论文只提到“初步实验和用户反馈”（Preliminary experiments and user feedback），没有具体给出实验细节。具体缺失信息包括：
- 用户研究的参与者人数、身份构成（专业诗人、古典文学爱好者、普通人等）；
- 测评任务设计（例如改写既定诗句、完成残句、复现贾岛“推敲”场景等）；
- 客观指标（声律正确率、生成质量自动评估、字级接受率、修改耗时等）；
- 与一次性生成系统或其他交互式系统的对比基线；
- 统计显著性、误差线或失败案例。
因此，本字段的信息完整度受限于可获得文本。如果后续提供全文，需要重点补充实验协议和评测指标。

Q5: 发现了什么实验现象？

从论文摘要的定性表述可以观察到：
1. 可控修改机制有效：用户锁定字/行后，系统能在约束下给出建议，说明“可控的推敲”机制初步可行。
2. 文学证据可追溯：三路知识源（高频搭配、PPL 排序诗句、类书知识）提供的建议带有来源，用户可以看到修改依据。
3. 交互体验正面：用户反馈整体积极。
然而，论文没有报告反直觉结果、负面案例、指标间张力或任何量化数字。可复现性层面的证据不足，需要拿到全文才能判断这些观察的可靠性。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：
1. 形式化评估：设计客观的声律/格律指标和人工评审协议，量化推敲质量与创作体验。
2. 大规模用户研究：比较新手、爱好者、专业诗人在不同交互模式下的创作体验、作者归属感和产出质量。
3. 知识库扩展：引入更多类书、诗话、韵书（如平水韵、词林正韵）、典故库和历代评注，提高证据覆盖度和准确性。
4. 跨文体迁移：把“锁定—反馈—证据”的交互框架推广到词、曲、对联、赋等更多古典文体。
5. 创作过程可视化：记录用户的推敲历史、版本演变和采纳/拒绝模式，形成“创作过程档案”。
6. 人机创作归属与署名伦理：探索 AI 参与润色时的贡献标注、版权归属和透明度机制。
7. 可解释性评测：系统评价“证据是否真的帮助用户理解修改理由”，而不只是展示文本来源。
8. 缩放性与多语言：尝试把类似思路用于其他高度依赖典故、格律和审美传统的诗体或语言。

Q7: 总结一下论文的主要内容

这篇论文聚焦古典诗歌创作中的“推敲”过程，提出名为 Jiuge-Tuiqiao 的交互式人机协作系统。论文的论证主线是：现有 AI 诗歌系统把用户当作提示词提供者，一次性生成结果，导致用户失去创作主动权和作者感；因此需要构建一个让 AI 退居“背景助手”位置、以用户推敲过程为中心的系统。技术主线围绕三元模型展开：用户驱动控制（锁定字/行）、古籍引导证据（高频搭配、按 PPL 排序的经典诗句、类书结构化知识）、AI 辅助生成（根据用户反馈和知识约束优化输出），并辅以动态声律检查机制，让用户在迭代修改中同时把握语义、韵律和审美。实验主线目前只有初步实验和用户反馈，结论是系统提供了可控的修改机制、可追溯的文学证据和积极的交互体验。整体上，论文的价值在于提出了一种“可解释的人机协同推敲”范式，把生成系统的评价维度从“生成质量”扩展到“创作过程中的用户控制力与可解释性”；但其局限也很明显：缺少量化的实验证据和完整的系统评测，很多设计决策的合理性仍需后续工作验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户当前方向中的“生成”（权重 0.10）直接重合，可视为古典诗歌领域的可控生成与人机协同案例。

## 基本信息

- 作者：Yufeng Han, Lifan Deng, Cunliang Kong, Wenhao Li, Xin Cong, Yuzhuo Bai, Kangyang Luo, Maosong Sun
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23098v2`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于 arXiv 元数据与 PDF 语义检索片段（abstract、introduction、related work、methods、ethics），并修正了 heuristic_draft 中的断句残片；所有实验细节缺失处均已明确标注为信息缺口。
