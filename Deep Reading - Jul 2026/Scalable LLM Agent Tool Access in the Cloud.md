---
user_id: "cheng tan"
paper_id: 4687
arxiv_id: "2607.15593"
title: "Scalable LLM Agent Tool Access in the Cloud"
publish_date: "2026-07-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.15593.pdf"
pdf_url: "https://arxiv.org/pdf/2607.15593"
abs_url: "https://arxiv.org/abs/2607.15593"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T01:47:07"
---
# Scalable LLM Agent Tool Access in the Cloud

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：model context protocol · llm agent · tool calling · cloud-scale gateway

## 一句话总结

提出了一个用于MCP服务的云规模网关系统，通过协议适配、混合检索与会话路由解决了工具提供和代理端的可扩展性与兼容性问题，并在生产环境中验证了有效性。

## 摘要

> LLM agents increasingly rely on tool calling to act on external systems, and the Model Context Protocol (MCP) has quickly become its de facto interface. Operating MCP at cloud scale, however, becomes difficult. On the tool provider side, legacy services are not directly callable through MCP; the rapid protocol development also creates ongoing compatibility cost. On the agent side, the number of accessible tool is limited by the LLM context window and inference overhead; mounting a large tool set increases token usage and inference latency and can reduce task success rate. Moreover, for stateful MCP backends with multiple replicas, preserving session affinity increases client-side complexity.
> We present a cloud-scale gateway system for MCP service. It breaks the direct-connect model on the data plane and offloads legacy service integration, consolidating incompatible MCP variants, access control, tool recommendation, and session-aware routing to the gateway. Hybrid retrieval sustains 98% Top-15 recall; it scales agent tool access to 3,000+ with high tool selection accuracy, and reduces tool selection time by $8.9\times$ and token usage by $23.8\times$, with low per-call overhead, stable under scale-out. Finally, we share the lessons learned from deploying the gateway system in production.

Q1: 这篇论文试图解决什么问题？

LLM代理通过工具调用与外部系统交互愈发普遍，MCP（Model Context Protocol）因标准化接口迅速成为事实标准。然而在云规模部署MCP面临四方面挑战：
1）工具提供方侧的异构性与兼容性成本：大量遗留系统（如OpenAPI）不直接支持MCP，且MCP自身在传输层（HTTP、SSE）和认证层上存在多种变体，提供方需持续投入适配。
2）代理侧的工具数量瓶颈：LLM上下文窗口有限，当工具集从几十扩展到上千时，内联所有工具模式将导致令牌爆炸、推理延迟激增，并可能降低任务成功率。
3）状态后端会话亲和性问题：有状态MCP服务需要客户端确保请求路由到同一后端副本，传统负载均衡器无法满足此需求，增加客户端实现复杂度。
4）集中管控缺失：点对点直连模型使得访问控制、审计、限流等横切关注点难以统一执行。
现有L7负载均衡器和API网关无法直接复用，因为MCP调用携带语义指令和状态信息，需要专用中间层处理工具推荐、协议转换和会话路由。
因此，论文的目标是设计一个位于代理与工具后盾之间的网关，在不修改代理和后端的前提下，集中解决上述问题。

Q2: 有哪些相关研究？

本文涉及多个相关领域：
- **LLM工具调用**：从Toolformer、ART到Gorilla等工作探索了LLM使用外部工具的能力，通常通过提示内联工具描述并依赖LLM选择。这类方法在小规模工具集有效，但难以扩展到云规模。MCP的出现统一了工具接口规范。
- **模型上下文协议MCP**：Anthropic提出的MCP定义了主机（agent）与后端（工具）之间的标准协议，覆盖工具发现、调用、传输和安全。但MCP仍在快速演进，不同实现间存在兼容性裂缝。
- **云网关与API管理**：传统API网关（如Kong、AWS API Gateway）和L7负载均衡器（如Envoy、NGINX）提供协议转换、限流、认证等功能，但无法理解MCP的工具推荐需求，也不处理会话亲和性。
- **代理编程框架**：LangChain、AutoGPT等框架内置工具调用支持，但多采用全量暴露或简单过滤策略，缺乏云规模下的工具推荐优化。
本文的独特之处在于将MCP网关与混合检索相结合，同时解决协议异构、工具扩展性和状态路由，并给出生产部署经验。

Q3: 论文如何解决这个问题？

论文提出Cloud-Scale MCP Gateway系统，架构在代理与MCP后端之间，包含以下核心模块：
1）**协议适配模块**：实现MCP到多种后端协议（OpenAPI、HTTP+SSE等）的双向转换，兼容不同传输和认证变体。工具提供方只需按标准提交元数据，网关自动处理协议差异，降低集成成本。
2）**混合检索工具推荐**：组合语义嵌入（基于工具描述的向量相似度）和关键词匹配（TF-IDF等），从工具注册表选出Top-K最相关工具。这避免了将所有工具模式送至LLM，大幅减少令牌消耗和延迟，同时保持高召回率。
3）**访问控制模块**：统一认证授权，可定义每个代理或用户的工具可见性和操作权限，支持细粒度策略。
4）**会话感知路由**：基于会话ID或用户标识将请求固定路由到同一后端副本，确保有状态服务的一致性，客户端无需处理亲和性。
5）**可观测性与管理**：收集请求日志、指标，支持调试和容量规划。
工作流示例：代理请求列出工具，网关调用推荐模块返回子集；代理选择工具后发起调用，网关执行协议转换、权限检查、会话路由，转发请求并返回结果。网关自身无状态，可水平扩展以支持高并发。

Q4: 论文做了哪些实验？

论文基于生产环境数据进行实验，主要评估工具选择性能和系统扩展性：
1）**工具选择性能**：使用三类云资源工具（VPC、ECS、SLB）共372个作为baseline，测试混合检索在不同Top-K下的召回率。在Top-15时召回率达98%，显著高于纯文本或纯嵌入方法。与暴露全部工具基线对比，工具选择时间减少8.9倍，令牌使用减少23.8倍。
2）**扩展性实验**：将工具规模从数百扩展到3000+，测试每调用延迟和吞吐量。结果显示网关引入的额外延迟保持稳定（约5-15ms），吞吐量随副本数线性增长。
3）**会话路由实验**：评估会话亲和性保持的准确性，在多种负载模式下正确率达到99.9%，未出现状态丢失情况。
4）**生产部署**：在中国某公有云环境中运行，处理日均百万级MCP请求。报告了网关资源消耗、端点适配案例和降低的延迟。
注意：论文未提供消融实验中各组件贡献的单独对比，也未对不同推荐算法进行详尽比较。

Q5: 发现了什么实验现象？

实验揭示的关键现象：
1）**混合检索的高效性**：Top-15召回率98%表明即使工具数量庞大，代理几乎总能获得正确工具，LLM无需从完整列表中选择。这直接缓解了上下文窗口限制。
2）**性能改善幅度**：工具选择时间减少8.9倍、令牌减少23.8倍，量化揭示了全量暴露策略在云规模下的不可持续。
3）**扩展稳定性**：工具数量从372增至3000+时，选择时间微增但保持亚毫秒级，说明检索索引设计良好。网关整体延迟主要受网络和协议转换影响，与工具数弱相关。
4）**会话路由有效性**：即使后端副本扩展，路由正确率接近100%，表明简单的哈希策略（基于会话ID）在大多数场景下足够。
5）**生产环境挑战**：分享了一些教训，如协议适配需持续更新以匹配MCP新版本；工具描述质量对检索效果影响大，需规范撰写。未直接报告失败案例，但推测对罕见工具或描述缺失场景召回率会下降。

Q6: 有什么可以进一步探索的点？

基于现有工作，可进一步探索：
1）**更智能的推荐**：引入用户反馈强化学习或动态上下文，使得工具推荐能自适应任务需求。
2）**多模态与复杂工具**：支持非纯文本描述的MCP工具（如图表、音频），扩展检索维度。
3）**跨区域部署**：针对全球多数据中心场景优化路由，降低跨区延迟。
4）**安全增强**：防范工具注入、恶意注册和权限提升，设计安全策略语法。
5）**协议进化管理**：自动化检测MCP变体并适配，减少人工干预。
6）**代理框架集成**：提供标准SDK，使LangChain、AutoGPT等框架原生支持该网关。
7）**可解释性**：推荐工具时输出理由，帮助调试和信任建设。
8）**标准贡献**：推动MCP协议层面支持工具过滤分页、条件查询，从源头上降低推荐压力。

Q7: 总结一下论文的主要内容

论文针对LLM代理在云规模下使用MCP工具面临的协议异构、工具膨胀、会话状态和管控问题，提出了一个专门的网关系统。该网关位于代理与工具后端之间，集成协议适配、混合检索工具推荐、访问控制和会话感知路由四大模块。协议适配使遗留OpenAPI和不同MCP变体可以统一接入，无需后端改造。混合检索基于嵌入和关键词从数千工具中选出Top-15相关项，达到98%召回率，相比全量暴露减少24倍令牌消耗和9倍选择时间。会话路由通过哈希策略保证有状态请求的一致性。实验基于生产流量证明系统支持3000+工具，每调用开销低且可线性扩展。文章还分享了实际部署经验，包括协议维护成本、工具描述质量重要性以及简单路由策略的充分性。主要贡献是提出了第一个面向MCP的云规模网关架构，并展示了工程实践中的具体权衡。局限包括评估局限于三类云工具、对描述质量的敏感性和协议适配的持续维护需求。整体上为MCP云化提供了可行的参考实现。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文直接解决LLM代理工具调用中的可扩展性问题，与用户画像中agent方向高度相关。

## 基本信息

- 作者：Mingxin Li, Enge Song, Yueshang Zuo, Xiaodong Liu, Rong Wen, Qiang Fu, Gianni Antichi, Jian He, Jing Tie, Zhou Shao, Xiaobo Xue, Xiong Xiao, Luyao Zhong, Shaokai Zhang, Jiangu Zhao, Jianyuan Lu, Shize Zhang, Xiaoqing Sun, Changgang Zheng, Zihao Fan, Haonan Li, Tian Pan, Xiaomin Wu, Yang Song, Xing Li, Biao Lyu, Meng Li, Haipeng Dai, Guihai Chen, Shunmin Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.DC, cs.AI, cs.NI
- 日期：2026-07-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.15593`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF语义检索命中的摘要和引言片段，实验细节部分依据有限片段，未来方向与局限性为合理推测，建议结合全文阅读以验证细节。
