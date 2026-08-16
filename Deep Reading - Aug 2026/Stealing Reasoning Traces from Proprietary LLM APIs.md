---
user_id: "cheng tan"
paper_id: 7431
arxiv_id: "2608.09867v1"
title: "Stealing Reasoning Traces from Proprietary LLM APIs"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.09867v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.09867v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-12T01:26:43"
---
# Stealing Reasoning Traces from Proprietary LLM APIs

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：chain-of-thought security · encrypted reasoning traces · model extraction · prompt injection

## 一句话总结

本文揭示并利用主流大模型 API 中加密推理痕迹块（encrypted reasoning traces）在不同会话、用户和模型间完全可互换的架构漏洞，将强模型的加密推理痕迹注入同一提供商较弱的模型，使其以明文逐字输出，从而以可扩展方式绕过反蒸馏保护，可大规模提取私有数据、暴露危险推理内容并执行隐形提示注入。

## 摘要

> Leading large language model providers now conceal their models' step-by-step reasoning, or chain-of-thought, to protect intellectual property and limit information leakage. Rather than storing these traces server-side, providers return them to the client as blocks of encrypted text, which the client passes back with each subsequent request. Building on prior research, we identify an architectural vulnerability: these encrypted blocks are fully compatible and interchangeable across different sessions, users, and models within a provider's ecosystem. We exploit this compatibility to develop a scalable decryption jailbreak. By injecting an encrypted reasoning trace from a given model into a weaker, and less safeguarded model from the same provider, we force it to decode and output the trace verbatim in plaintext, without ever jailbreaking the more capable model directly.
> This vulnerability enables four distinct attack vectors. First, it circumvents anti-distillation mechanisms, allowing adversaries to extract a proprietary model's reasoning, as we demonstrate across Anthropic, OpenAI, and Google. Second, it allows for large-scale private data extraction. Developers frequently share session logs publicly, unaware of contents of the encrypted blocks. By decoding 315,320 reasoning blocks scraped from public repositories, we recovered 367 Personally Identifiable Information (PII) artifacts and 182 credentials. Third, it inadvertently reveals hazardous information hidden within the reasoning process, even in cases where the model's final, visible output safely rejects a malicious request. Fourth, attackers can leverage this flaw to execute invisible prompt injections, embedding malicious payloads entirely within encrypted blocks to poison public agentic rollouts. Following responsible disclosure, we propose concrete cryptographic and system-level mitigations to secure client-side reasoning.
> Main paper: API vulnerability and reasoning extraction.
> Appendix A: Proposed cryptographic and system-level mitigations.
> Appendix B: Similarity analysis of Opus traces and Kimi-K3/GLM-5.2 traces.
> Appendix C: Extraction attack details for Gemini, Claude and GPT APIs.
> Appendix D: Recovered leaked private information and API keys.
> Appendix E: Examples of decoded reasoning.

Q1: 这篇论文试图解决什么问题？

这篇论文针对的是大模型提供商在 API 中隐藏思维链（chain-of-thought）这一安全措施本身所引入的新攻击面。具体问题包括：
1. **反蒸馏机制的失效**：提供商依赖思维链保密来防止竞争对手蒸馏其专有推理能力，但加密块的客户端回传机制导致攻击者可以通过弱模型间接获得明文推理，从而系统性绕过反蒸馏。
2. **加密块的可互换性漏洞**：先前研究发现，同一提供商返回的加密推理块在会话、用户和模型之间完全兼容，这为交叉模型攻击提供了结构性基础。
3. **私密数据的大规模泄漏**：开发者经常公开分享会话日志，但往往意识不到其中加密块的内容可能被他人解密；攻击者可以规模化地从公开数据源中恢复 PII 和凭据。
4. **危险信息的隐蔽暴露**：即使模型最终输出是安全的拒绝，其隐藏的推理过程中也可能包含有害或敏感信息，而这一过程在加密保护下不被审查。
5. **隐形提示注入**：加密块可携带任意恶意载荷，攻击者可以利用其不可读性在公开的智能体调用链中植入后门，用户和下游系统难以察觉。
论文核心是揭示“客户端加密”这一设计决策带来的系统风险，并说明它如何同时破坏机密性、隐私和完整性。

Q2: 有哪些相关研究？

论文将自身工作置于模型提取（model extraction）和思维链安全的研究脉络中。检索到的证据提到：“标准黑盒模型提取和蒸馏攻击假设一个拥有 API 访问权限的对抗性开发者，利用专有模型的可观察输出作为学生模型的训练数据”（涉及 Tramèr 等人的经典蒸馏攻击框架）。本文的不同之处在于，攻击者无需大量查询或训练学生模型，而是直接利用加密块的格式兼容性，通过弱模型“解密”强模型的推理痕迹。
此外，先前研究已识别出这些加密块在提供商生态内可互换的架构特性，但本文首次将其转化为可扩展的公开利用。相关工作还可能涉及：
- **思维链隐私与安全**：关于推理过程可能泄露敏感信息或包含有害内容的研究；
- **提示注入攻击**：尤其是面向智能体工作流的隐藏式注入；
- **隐私数据提取**：从语言模型输出中恢复训练数据或个人信息的攻击；
- **加密协议与存储**：客户端加密在隐私保护和可审查性之间的权衡。
由于当前只提供摘要和少量检索片段，更完整的相关工作细节（如是否直接比较了不同解密方法、是否与现有提示注入防御对抗）需要查阅原文。

Q3: 论文如何解决这个问题？

论文提出的攻击方法如下：
1. **获取加密推理块**：攻击者作为普通 API 客户端，从目标强模型的响应中截获加密的推理痕迹块，这些块本就随每次请求返回给客户端。
2. **识别同提供商弱模型**：选择同一提供商旗下防护较弱、更注重成本和速度的模型（如 Haiku 类或 Luna 类），这些模型缺少针对思维链泄露的强拒绝训练。
3. **注入与触发解码**：将截获的加密块原样注入到弱模型的输入（或上下文）中，诱导弱模型在其他请求中解码该块并逐字输出明文推理内容。由于加密块在当前请求中既被当作附加上下文，又可能被模型视为需要响应的内容，模型会错误地将其还原为文本。
4. **规模化利用**：由于加密块跨用户、会话和模型兼容，攻击者可以批量获取和注入，无需对目标模型本身进行任何越狱（jailbreak）。
论文进一步将这一原始攻击扩展为四个攻击向量：反蒸馏绕过、私密数据提取、危险信息暴露、隐形提示注入。在防御方面，论文提出了“加密和系统级缓解措施”，但摘要未给出具体技术细节；其原理是让加密块不可被跨模型使用，或让用户能够验证、消毒日志。

Q4: 论文做了哪些实验？

论文进行的实验（基于摘要和检索片段，具体实验协议细节有限）：
1. **跨提供商推理痕迹提取**：在 Anthropic、OpenAI、Google 三家主流提供商的 API 上演示了绕过反蒸馏、明文提取专有模型思维链的攻击；涉及的模型包括如 Claude Opus 4.8 或 GPT-5.6 Sol 等强模型，以及 Claude Haiku 4.5 或 GPT-5.6 Luna 等弱模型。
2. **大规模私密数据提取**：从公开代码仓库中抓取并解码了 315,320 个加密推理痕迹块，从中恢复出 367 个 PII（个人身份信息）和 182 个凭据（如密钥、口令）。
3. **危险信息暴露演示**：构造了模型最终输出安全拒绝但隐藏推理中包含危险或敏感内容的案例，证明加密推理保护并不能阻止有害信息被间接提取。
4. **隐形提示注入演示**：将恶意负载完全嵌入加密块中，在公开智能体调用中执行注入，证明攻击者可以污染 agentic 工作流而不被用户察觉。
由于原文只提供了摘要和部分段落，实验的详细设置（如数据集、基线、评估指标、是否进行了消融）尚未从现有材料中获得，需要进一步阅读原文。

Q5: 发现了什么实验现象？

从已获得的摘要和证据中，可以归纳出以下实验现象和关键观察：
1. **加密可互换性是关键条件**：加密推理痕迹块在不同会话、用户和模型间完全兼容，这是攻击可行的核心前提；没有这种互换性，注入弱模型将无法成功解码。
2. **弱模型的防护缺失导致泄漏**：同一提供商中，成本优化的弱模型往往缺乏针对思维链泄露的专门拒绝训练，成为天然的解密“通道”。
3. **规模化数据泄漏的现实性**：在公开仓库中抓取的 315,320 个加密块中，有 367 个包含 PII、182 个包含凭据，说明开发者分享日志的行为正在无意中泄露敏感信息，且泄漏规模与被分享日志数量线性相关。
4. **最终输出与推理过程的不一致**：模型在最终回答中可能展现安全的拒绝行为，但隐藏的思维链仍包含危险信息；这意味着单纯依赖输出安全评估无法防止危险知识传播。
5. **攻击的隐蔽性**：由于攻击不改变最终可见输出，也不需要对强模型进行直接越狱，因此检测难度高，且攻击痕迹可能隐藏在加密数据中。
6. **跨提供商的普遍性**：在 Anthropic、OpenAI、Google 三家都验证成功，说明问题并非孤立实现的缺陷，而是主导架构中共有的设计选择。

Q6: 有什么可以进一步探索的点？

基于该论文揭示的问题，可进一步探索的方向包括：
1. **加密协议改进**：设计不可互换的加密方案（如绑定会话密钥、模型指纹、请求上下文），使加密块无法跨模型解码，同时兼顾合法用户回传需求。
2. **可验证加密与可净化日志**：开发允许用户对加密块进行消毒（如脱敏、移除可疑内容）而不暴露明文的技术，支持日志分享和隐私保护。
3. **对智能体工作流的纵深防御**：研究在 agentic 系统中如何检测和阻断隐藏在不可读上下文中的提示注入，包括静态分析和运行时监控。
4. **检测与溯源技术**：开发能够识别加密块是否被恶意利用的检测方法，例如通过弱模型行为差异或统计指纹来发现异常解码。
5. **模型级缓解**：训练更强的一致性抗蒸馏模型，即使在弱模型场景下也能拒绝解码未知加密块。
6. **正式安全分析**：将“客户端加密”设计纳入威胁建模框架，建立针对此类架构的通用漏洞分类和评估基准。
7. **跨学科影响**：在 AI for Science 等敏感领域，探讨推理痕迹保密性对数据共享、可复现性和合作研究的影响。
8. **监管与披露流程**：研究负责任披露下的修补周期和政策建议，以及如何防止类似设计在其他新模型中重现。

Q7: 总结一下论文的主要内容

本文首次系统性地展示了通过客户端加密的思维链机制劫持专有 LLM 推理的攻击面。主流大模型供应商为了同时保护知识产权和限制信息泄漏，不再服务端存储逐步推理，而是将思考过程加密为不透明文本块返回给客户端，并在下一请求中让客户端原样回传；这种设计假设加密块无法被用户解读，因此是安全的。然而，论文基于先前发现的架构特性——这些加密块在提供商生态内跨会话、跨用户、跨模型完全可互换——构建了一种可扩展的解密越狱方法：攻击者先从一个防护更严的强模型（如 Claude Opus 4.8 或 GPT-5.6 Sol）处截获加密推理块，再将其注入同一提供商中防护更弱的模型（如 Claude Haiku 4.5 或 GPT-5.6 Luna）的上下文，利用弱模型缺少对抗思维链泄露的拒绝训练这一特点，强制其逐字输出明文推理，从而在不直接越狱强模型的情况下获得其内部思维链。

论文将这一漏洞转化为四个攻击向量。第一，绕过反蒸馏保护：在 Anthropic、OpenAI、Google 三家主流供应商上验证了专有模型推理痕迹的可提取性。第二，大规模私人数据提取：开发者常在公开仓库中分享会话日志而不了解加密块内容，论文从 315,320 个公共推理块中恢复出 367 个 PII 和 182 个凭据，证明了无特权攻击者可以规模化地挖掘敏感信息。第三，危险信息泄露：即使模型最终输出安全地拒绝恶意请求，其隐藏推理中仍可能包含有害内容，攻击者可利用此漏洞将其暴露。第四，隐形提示注入：把恶意负载完全放在加密块中，在公开的智能体调用中植入后门，从而污染 agentic 工作流。

论文还给出了威胁模型：标准无特权 API 对手，不需要内部权限、不能观察服务端状态、也无法获得模型权重。这意味着攻击完全是黑盒且无需特殊资源，使得漏洞的影响范围极大。在防御方面，论文提出加密和系统级缓解措施（如移除可互换性、允许用户安全消毒日志），并遵循负责任披露流程。总体而言，本文指出了“客户端加密”这一设计选择在密码学与系统层面的脆弱性，并从攻击面、规模化影响和多向量危害三个层面重新定义了推理痕迹保密的威胁模型。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：智能体方向（权重 0.10）：该漏洞直接威胁 agentic 工作流——加密推理块常被智能体系统自动传递和持久化，本研究的隐形提示注入路径为智能体安全提供了具体攻击样例与防御动机。

## 基本信息

- 作者：Alexander Panfilov, David Schmotz, Ilia Shumailov, Luca Beurer-Kellner, Joachim Schaeffer, Ameya Prabhu, Jonas Geiping, Maksym Andriushchenko
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CR, cs.AI, cs.LG
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.09867v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本分析主要依据论文摘要和检索到的少量 PDF 片段（Introduction、Threat Model、Distillation Attacks、Secret Extraction 等）生成，其中细节信息基于证据做合理推断；具体实验设置和缓解机制的有效性需查阅原文确认。
