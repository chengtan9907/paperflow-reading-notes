---
user_id: "cheng tan"
paper_id: 9582
arxiv_id: "2608.23705v2"
title: "The Limits of Automatic Evaluation of Creativity in Large Language Models"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23705v2.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23705v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:24:36"
---
# The Limits of Automatic Evaluation of Creativity in Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · creativity evaluation · llm-as-a-judge · automatic evaluation

## 一句话总结

该论文通过构建100篇人类与100篇AI生成短篇故事的平衡数据集，收集人类在11个创造力维度上的评估，并对比自动客观指标与LLM-as-a-Judge评估，发现自动评估与人类判断严重不一致、LLM法官系统性偏好AI生成文本、常用自动指标与人类判断近零相关，从而揭示当前创造力自动评估方法的根本局限。

## 摘要

> Large Language Models (LLMs) are increasingly capable of generating text that challenges human performance in domains requiring creativity, yet evaluating creativity in LLM-generated content remains a significant challenge. Here, we investigate whether current automatic evaluation methods can reliably capture human judgments of creativity. We collect human evaluations of human- and AI-generated short stories from the WritingPrompts dataset across 11 dimensions of creativity, and compare these judgments with automated objective metrics and LLM-as-a-Judge evaluations. Our experiments reveal substantial misalignment between automatic evaluations and human assessments. In particular, LLM-based judges exhibit a systematic preference for AI-generated stories, consistently favoring their stylistic characteristics over the unpredictability and other qualities of human-authored texts. Furthermore, correlation analyses show that widely used automatic metrics exhibit near-zero alignment with human judgments across both human- and AI-generated stories, suggesting that they fail to capture important dimensions of creativity. These findings highlight fundamental limitations in current approaches to the automatic evaluation of creative text and underscore the difficulty of reducing the multidimensional and subjective nature of creativity to computational metrics.
> Keywords: Large Language Models, Creativity Evaluation, Natural Language Generation, LLM-as-a-Judge

Q1: 这篇论文试图解决什么问题？

1. **核心问题**：LLM生成文本的创造力评估是否可靠？具体而言，当前自动评估方法（包括客观指标和LLM-as-a-Judge范式）能否与人类对创造力的主观判断对齐。
2. **创造力定义之争**：论文从哲学角度指出，创造力的定义影响结论——若仅看产物是否新颖、惊奇、有价值，LLM产物可能被认为有创造力；但若考虑创作过程、创作者及环境等潜在方面，结论则相反。这种定义分歧导致评估标准混乱。
3. **自动评估的失效**：传统自动指标（如基于n-gram重叠的指标）无法衡量深层语义和原创性；LLM-as-a-Judge虽然试图模拟人类判断，但可能自带偏好，且缺乏对主观、多维创造力的灵敏度。
4. **人类评估的复杂性**：创造力评估依于背景、文化、个人品味，单一维度无法概括；如何建立可靠而全面的人类评估基线本身就是挑战。
5. **研究缺口**：已有研究多孤立考察某一指标或主观评分，缺乏对多种自动评估与人类评估的系统性、多维度对比。作者采用整体化方法，同时检验客观指标和LLM法官，并引入平衡数据集。

Q2: 有哪些相关研究？

1. **LLM创造力之争**：大量研究讨论机器是否有创造力，或LLM产出是否可视为创造性作品。本论文在哲学层面引入‘产物视角’与‘过程/作者视角’的对立，合理推断其参考了关于创造力的经典定义（如novelty, surprise, value）以及关于计算创造力的讨论。
2. **自动文本评估指标**：传统指标（如BLEU、ROUGE、perplexity等）常被用于生成质量评估，但众多研究表明它们与人类判断相关性低。论文提到“Creativity Index”这类为创造力设计的量化指标，但未在摘要中给出具体公式；合理推断作者对这些指标进行了关联性检验。
3. **LLM-as-a-Judge**：近期工作将LLM作为评估者来打分，宣称可替代人工评估，但已有研究发现LLM评委存在位置偏见、自我偏好等。本论文聚焦于创造力评估中LLM评委的偏见。
4. **写作生成与数据集**：WritingPrompts是常用的创意写作数据集，包含人类写作和基于提示的AI生成文本；相关研究常用其评估生成模型的创造力。
5. **多维度评估**：人类判断通常包含多个维度（如原创性、惊喜度、连贯性等）；论文采用11个维度，合理推断这些维度覆盖了创造力的不同侧面。
6. **相关性分析范式**：用相关系数衡量自动指标与人类判断的对齐程度，是评估领域的常见方法。注意：由于检索证据有限，以上相关文献仅为合理推断，具体引用需查阅原文。

Q3: 论文如何解决这个问题？

论文采用一个整体化、多维度的评估框架，包含四个步骤：
1. **构建平衡数据集**：从WritingPrompts数据集中选取100篇人类写作和100篇AI生成的短篇故事，确保两种来源的故事数量相等、题材分布尽可能匹配（合理推断），以控制来源偏差。
2. **人类评估基线**：邀请人类评估者（数量未在摘要中给出，原文可能报告）在11个创造力维度上对每篇故事评分，例如新颖性、惊喜度、价值、可读性等（具体维度名称未在摘要中列出，合理推断包括这些典型维度）。取多人的平均分或多数投票作为人类判断标签。
3. **自动客观指标计算**：设计或选用若干定量指标，包括论文提到的“Creativity Index”以及其他常规文本质量指标（如词汇多样性、句子长度、n-gram新颖度等，合理推断）。对这些指标与人类评分做相关分析。
4. **LLM-as-a-Judge评估**：让多个LLM（具体模型未在摘要中说明）对故事进行打分，维度与人类评估相同或相似，然后计算LLM评分的分布、与人类评分的相关以及LLM对两类故事（人类vs AI）的平均分差，以检测系统性偏好。
5. **统计分析**：计算皮尔逊或斯皮尔曼相关系数，比较不同评估方法的一致性；通过显著性检验判断偏差是否统计显著。论文重点报告了LLM评委对AI故事的偏好，以及自动指标与人类判断近零相关的发现。

Q4: 论文做了哪些实验？

1. **数据规模**：使用100篇人类+100篇AI生成的短篇故事，总共200篇。
2. **评估维度**：人类评估覆盖11个创造力相关维度。
3. **自动指标**：包括“Creativity Index”和其他未明确列出的自动客观指标。
4. **LLM评委**：采用LLM-as-a-Judge范式，让LLM对故事进行打分（具体模型和提示策略未在摘要中给出）。
5. **对比分析**：
 - 人类评分 vs 自动指标：计算相关性。
 - 人类评分 vs LLM评分：计算相关性。
 - LLM评分的分布：比较LLM对人类故事和AI故事的平均打分差异。
6. **扩展分析**：证据中提及“附录A扩展相关矩阵”，推测作者还展示了多个指标/维度间的完整相关矩阵。
7. **局限分析**：讨论局限于单一任务（短篇故事）和单一数据集（WritingPrompts）。

Q5: 发现了什么实验现象？

1. **LLM系统偏好AI生成文本**：LLM评委给AI生成故事的分数显著高于人类故事（或至少存在稳定偏差），且这种偏好与AI文本的风格化特征有关，而与人类文本的不可预测性等品质无关。这是一个反直觉的发现：LLM作为“智能评委”本应更客观，却表现出对机器文本的偏爱。
2. **自动指标近零相关**：广泛使用的自动指标（包括Creativity Index）与人类判断的相关性接近零，在人类和AI故事两个子集上都成立，说明这些指标无法捕捉人类感知的创造力。
3. **LLM评分与人类评分的错位**：LLM评分未能可靠反映人类判断的深层语义；可能仅在个别维度（如结构）上存在微弱正相关，但整体一致性弱。
4. **指标间差异**：证据提到“与LLM-as-a-Judge分数的相关性较弱，所有其他相关性可忽略”，推测不同自动指标之间也缺乏一致性，或与人类及LLM评分的相关性都很弱。
5. **维度间张力**：创造力的多维性导致某些维度（如原创性）与另一些（如流畅性）可能呈现负相关或复杂关系，简单的单一指标无法概括（合理推断）。
6. **文本来源差异的影响**：人类故事与AI故事在风格上的差异可能是LLM偏好产生的原因——AI模型可能学习到了某种“平均风格”，LLM自我评估时反而偏好这种风格，形成类我偏差（归纳）。

Q6: 有什么可以进一步探索的点？

1. **扩展数据集与任务**：研究仅基于WritingPrompts短篇故事，未来可扩展到其他体裁（如诗歌、剧本）、其他语言、更长篇幅的文本，检验结论的普适性。
2. **改进LLM评委**：探索减少LLM偏见的策略，如校准方法、多模型集成、设置反事实提示、利用更小但更精确的评估器，或设计混合人类-LLM评估流程。
3. **开发新的自动指标**：结合语义表示、常识推理和创造力理论设计更接近人类判断的指标，并验证其泛化能力。
4. **理解偏见来源**：研究LLM对AI文本偏好的机制根因，是否来自训练数据、风格分布或自我参照，将为缓解偏见提供理论指导。
5. **多维度基线的细化**：提供更细粒度的11维度定义，研究维度权重和相互关系，构建标准化的创造力评估量表。
6. **探索评估范式变化**：从打分式评估转向对比式、生成式论证评估，或让人类与LLM交互动态评估。
7. **跨学科验证**：结合认知科学、计算创造力和心理测量学，验证评估框架的有效性和可重复性。
8. **实践准则**：提出自动评估工具的适用范围，例如仅用于结构、清晰度等客观维度，避免在创造性任务中过度依赖自动评估。

Q7: 总结一下论文的主要内容

1. **研究背景与动机**：LLM的文本生成能力逼近人类，尤其在创意写作领域，引发对“机器是否具有创造力”的争论。但评估创造力本身就是难题：现有自动评估方法是否真的能判断创作质量？若自动评估不可靠，则涉及LLM创造力的结论（如“胜过人类”）都值得质疑。作者从哲学角度区分了产物视角和过程视角，指出定义分歧导致评估标准混乱，因此需要实证检验自动评估与人类判断的对齐。
2. **技术路线**：作者设计了一个系统评估流水线：平衡数据（100+100）→ 人类11维评分 → 自动客观指标 → LLM-as-a-Judge评分 → 相关性分析。数据取自WritingPrompts，确保代表性。人类评分作为权威基线；自动指标作为传统方法代表；LLM评分作为新兴自动评估范式代表。通过多方法对照，揭示各自的偏离模式。
3. **实验框架**：实验分为三个层次：a) 自动指标与人类评分的相关；b) LLM评分与人类评分的相关；c) LLM评分对两类故事的分布差异。同时使用扩展相关矩阵展示11维度和多指标间的完整关系。统计分析采用相关系数和显著性检验（推断）。
4. **主要发现**：
 - 自动指标与人类判断几乎无相关性 → 传统客观指标失效。
 - LLM评委显著偏好AI生成的故事，且这种偏好与其风格特征相关 → LLM评委存在系统性偏见。
 - LLM与人类判断整体相关性低，尤其在深层语义维度上。
 - 这些结果共同表明：当前自动评估无法可靠衡量创造力，对LLM创造力水平的宣称需谨慎对待。
5. **论证主线**：论文以“自动评估是否可靠”为问题，以人类判断为参照，通过多层对比证明自动评估（无论传统指标还是LLM）都无法替代人类判断。作者强调创造力的主观性和多维性使得还原为计算指标存在根本困难。
6. **价值与局限**：贡献在于提供了首个系统性、多维度的自动vs人类创造力评估对比（合理推断），并明确指出LLM评委的偏好问题。局限包括单一数据集、单一任务、以及未披露评估者的详细统计和具体LLM型号（这些信息在原文中可能补充）。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该研究与LLM生成文本的质量评估密切相接，尤其适合从事生成任务和自动评估方法开发的读者。

## 基本信息

- 作者：Alessandro Tutone, Giorgio Franceschelli, Mirco Musolesi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.CY
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23705v2`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的摘要、讨论、局限等片段；由于提供的证据仅限于此，部分背景与相关研究内容基于合理推断，具体数值和实验细节需查原文确认。
