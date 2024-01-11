一个开源框架，旨在简化使用大型语言模型（LLM）的应用程序创建。目标是为各种大型语言模型应用提供**通用接口**，从而**简化**应用程序的**开发流程**。
## 核心组件

LangChain 作为一个大语言模型开发框架，可以将 LLM 模型（对话模型、embedding模型等）、向量数据库、交互层 Prompt、外部知识、外部代理工具整合到一起，进而可以自由构建 LLM 应用。 LangChain 主要由以下 6 个核心模块组成:

- **模型输入/输出（Model I/O）**：与语言模型交互的接口。
- **数据连接（Data connection）**：与特定应用程序的数据进行交互的接口。
- **链（Chains）**：将组件组合实现端到端应用。
- **记忆（Memory）**：用于链的多次运行之间持久化应用程序状态。
- **代理（Agents）**：扩展模型的推理能力，用于复杂的应用的调用序列。
- **回调（Callbacks）**：扩展模型的推理能力，用于复杂的应用的调用序列。