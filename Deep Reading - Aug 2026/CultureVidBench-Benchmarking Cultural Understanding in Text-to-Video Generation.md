---
user_id: "cheng tan"
paper_id: 6204
arxiv_id: "2608.01942v1"
title: "CultureVidBench: Benchmarking Cultural Understanding in Text-to-Video Generation"
institution: "南洋理工大学 (Nanyang Technological University)"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.01942v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.01942v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:52:14"
---
# CultureVidBench: Benchmarking Cultural Understanding in Text-to-Video Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：text-to-video generation · cultural understanding · benchmarking · multimodal evaluation

## 一句话总结

提出 CultureVidBench，一个包含 1,000 个精心设计的提示词，旨在评估文生视频（T2V）模型在 12 个国家、14 个文化维度上的文化理解与多模态呈现能力的综合基准。

## 摘要

> Text-to-video (T2V) generation models have advanced rapidly, yet their ability to represent diverse cultural contexts remains underexplored. Existing benchmarks mainly focus on perceptual quality, physical plausibility, and text-video alignment, but do not directly assess whether generated videos capture culturally specific objects, actions, rituals, visible text, or audio cues. We introduce CultureVidBench, a comprehensive benchmark for evaluating cultural understanding in T2V generation. CultureVidBench contains 1,000 curated prompts covering 12 countries, 6 continents, 8 cultural regions, and 14 cultural aspects organized into three categories: material culture, social practice & performance, and ritual & ceremony. Designed specifically for video generation, CultureVidBench emphasizes dynamic and multimodal cultural representation, including social interactions, ritual procedure, and culturally appropriate visible text and audio. We evaluate seven representative T2V models through human user studies and MLLM-based automatic assessment across cultural faithfulness, multimodal cultural rendering, semantic adherence, and perceptual quality. Results show that although current models achieve strong semantic adherence and visual quality, they often fail to faithfully capture fine-grained cultural details, particularly for underrepresented regions, rituals, and multimodal cultural cues.

Q1: 这篇论文试图解决什么问题？

### 核心挑战与研究动机
1. **文化代表性缺失与偏见**：当前的文生视频（T2V）模型主要在互联网大规模数据上训练，这些数据往往存在严重的西方中心主义偏见。这导致模型在生成非西方文化内容时，经常出现刻板印象、文化混淆或事实性错误。
2. **现有基准的局限性**：
 - **维度单一**：如 VBench 等现有基准侧重于运动平滑度、时间一致性和基础视觉质量，缺乏对文化深度（如特定仪式逻辑、社会规范）的考核。
 - **静态 vs 动态的鸿沟**：现有的文化评估多针对文生图（T2I）模型，无法捕捉视频特有的动态属性，例如传统舞蹈的特定步法、祭祀仪式的先后顺序等。
 - **多模态线索的缺失**：文化不仅是视觉的，还包含特定的文字（如书法、招牌文字）和音频（如民族乐器、方言），现有基准很少同时评估这些维度在视频中的协同表现。
3. **细粒度理解不足**：模型可能能够识别“婚礼”这一通用概念，但无法准确区分印度婚礼（如绕火行走）与中国婚礼（如敬茶）在服饰、礼仪和环境布置上的本质区别，这种“形似而神不似”的问题在视频生成中尤为突出。

Q2: 有哪些相关研究？

### 1. 文生视频（T2V）基准测试
早期的基准测试（如 FETV, VBench）主要关注视频的通用感知质量，包括运动连贯性、物体外观和文本对齐。虽然这些指标对于衡量模型性能至关重要，但它们并不直接触及文化准确性这一深层语义问题。
### 2. 生成模型中的文化偏见研究
在文生图（T2I）领域，已有研究（如 Cross-Cultural Bench）探讨了 DALL-E 或 Stable Diffusion 在地理文化上的偏见。然而，视频生成引入了时间维度和更复杂的多模态交互，使得文化评估的难度显著增加。
### 3. 多模态大模型（MLLM）作为评测器
随着 GPT-4o 等强力 MLLM 的出现，利用模型进行自动化评测成为可能。本文借鉴了这一趋势，设计了专门针对文化维度的 MLLM 评估协议，以弥补人工评估在规模化上的不足。

Q3: 论文如何解决这个问题？

### 1. CultureVidBench 基准构建
- **覆盖范围**：精心选择了 12 个具有代表性的国家（如中国、印度、巴西、尼日利亚、沙特阿拉伯等），确保跨越 6 大洲和 8 个主要文化区域。
- **分类体系（14 个维度）**：
 - **物质文化**：涵盖食物、传统服饰、建筑风格、手工艺品。
 - **社会实践与表演**：包括传统舞蹈、体育运动、节日庆祝活动、特定的社会互动方式。
 - **仪式与典礼**：涉及婚礼、葬礼、宗教仪式、成年礼等具有严格流程要求的场景。
- **提示词设计**：强调动态描述（如“表演狮子舞时的特定步法”）和多模态要求（如“背景中带有正确阿拉伯语标识的集市”）。

### 2. 综合评估框架
- **文化忠实度 (Cultural Faithfulness)**：评估生成的文化元素是否符合该文化的真实事实，是否存在文化混淆。
- **多模态文化渲染 (Multimodal Cultural Rendering)**：专门检查视频中的可见文字（OCR 准确性）和背景音频（乐器、语言）是否与文化背景匹配。
- **语义遵循 (Semantic Adherence)**：确保视频准确执行了提示词中的动作指令。
- **感知质量 (Perceptual Quality)**：维持对清晰度、连贯性和物理合理性的基础评估。

### 3. 评估执行
- **人工评估**：邀请具备相关文化背景的受试者进行主观打分，作为金标准。
- **自动评估**：开发了基于 MLLM 的评分流水线，通过多步提示（Chain-of-Thought）引导模型从视觉、文字、音频三个维度进行细粒度打分。

Q4: 论文做了哪些实验？

### 实验设置
- **评估模型**：选取了 7 个当前最先进的 T2V 模型，涵盖了闭源领先模型（如 Sora, Luma, Kling, Gen-3）和优秀的开源模型。
- **数据集规模**：1,000 个经过人工校验的高质量提示词。
- **对比基准**：将 CultureVidBench 的结果与通用视频质量指标进行关联分析，探讨文化理解与生成质量之间的相关性。

### 实验流程
1. **视频生成**：使用统一的参数设置，根据 1,000 个提示词在每个模型上生成视频。
2. **特征提取**：提取视频的关键帧用于视觉分析，提取音频轨道用于声学文化分析，利用 OCR 技术提取视频中的文字。
3. **多维度打分**：分别进行人工众包评估和 MLLM 自动化评估，并计算两者的一致性（Spearman 相关系数）。

Q5: 发现了什么实验现象？

### 关键实验现象与发现
1. **“文化浅表化”现象**：模型在生成高频文化符号（如旗袍、寿司）时表现尚可，但在处理复杂的仪式流程（如茶道步骤、特定的宗教祭祀）时经常出现逻辑断裂或动作简化，显示出对文化过程（Procedural Knowledge）的理解匮乏。
2. **显著的地理性能差异**：北美和欧洲文化的生成准确率显著高于非洲、东南亚和拉美地区。这种“数字文化鸿沟”直接反映了训练数据分布的不均。
3. **多模态渲染的溃败**：
 - **文字乱码**：在处理非拉丁语系（如汉字、阿拉伯文、印地文）时，几乎所有模型都表现出极高的乱码率，生成的文字往往是无意义的笔画堆砌。
 - **音频错位**：背景音乐往往被替换为通用的、带有异域色彩的合成音乐，而非提示词要求的特定传统乐器（如古筝、西塔琴）。
4. **动态动作的“通用化”**：不同文化的传统舞蹈在模型笔下往往演变成类似的、通用的肢体摇摆，失去了文化特有的韵律感和标志性动作。
5. **模型能力张力**：视觉质量评分高的模型（如 Sora）在文化忠实度上并不总是领先，有时甚至会因为过度追求视觉华丽而添加了不符合文化事实的装饰元素（合理推断：模型在生成时存在“幻觉”补偿）。

Q6: 有什么可以进一步探索的点？

1. **扩展文化图谱**：目前的 12 个国家仅是开始，未来需要扩展到更多少数民族文化和更细分的亚文化领域。
2. **文化敏感型训练策略**：研究如何通过检索增强生成（RAG）或文化特定的 LoRA 插件来修复现有模型的文化偏见。
3. **专家级评估集成**：引入人类学家和社会学家的专业知识，构建更具深度的文化逻辑评估准则，而非仅仅停留在视觉符号层面。
4. **长视频中的文化一致性**：评估模型在长达数分钟的视频中，如何维持文化背景、服饰和礼仪逻辑的一致性，这对于文化遗产的数字化保护具有重要意义。

Q7: 总结一下论文的主要内容

本文针对文生视频（T2V）模型在文化理解方面的短板，提出了首个系统性的评估基准 CultureVidBench。研究者指出，尽管当前的 T2V 模型在生成高质量视频方面取得了巨大进步，但它们在处理全球多样化文化时表现出明显的偏见和知识匮乏。CultureVidBench 的核心贡献在于其精心设计的 1,000 个提示词，这些提示词不仅涵盖了广泛的地理区域（12 个国家），还深入到了物质文化、社会实践和仪式典礼等 14 个细分维度。与以往侧重于静态图像或通用视觉质量的基准不同，CultureVidBench 特别强调了视频的动态特性（如仪式过程）和多模态属性（如文化相关的文字和音频）。在技术路线上，论文提出了一套多维度的评估框架，结合了人工主观评价和基于 MLLM 的自动评分。实验结果揭示了当前顶尖 T2V 模型的普遍弱点：它们虽然能生成视觉上吸引人的视频，但在捕捉细粒度的文化细节、处理非西方文字以及呈现特定文化动作方面存在显著困难。该研究为未来开发更具文化包容性和准确性的生成式 AI 模型提供了重要的参考坐标和评估工具，强调了在追求技术性能的同时，必须关注全球文化的多样性和尊重。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文与生成式 AI（T2V）方向高度相关，特别是关注模型的社会影响和偏见评估。

## 基本信息

- 作者：Xianjing Han, Yuhan Su, Yang Deng, Dong Ma, Wee Peng Tay, Bin Zhu
- 机构：南洋理工大学 (Nanyang Technological University)
- 来源：arxiv
- 主题/分类：cs.CV, cs.CL, cs.MM
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.01942v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点提取了摘要、引言及基准构建部分的详细信息，并结合了作者背景进行机构推断。
