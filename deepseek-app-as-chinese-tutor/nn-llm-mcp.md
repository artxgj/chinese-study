## 1.1. Prompt Repetition & Chinese Essence

**Original Prompt (English):**  

> Provide a comprehensive overview of the MCP protocol. Describe its architecture. How is it used?

**Chinese Essence (简明口语化):**  

> 你让我全面介绍 MCP 协议：它是什么、架构怎么设计的、实际中怎么用。要讲清楚核心概念、组件、通信方式，以及开发者和应用怎么接入。

---

## 2. Bilingual Response (Revised)

### 中文部分 (Chinese Section)

**摘要**  
MCP（模型上下文协议）是 Anthropic 于 2024 年 11 月开源的开放标准，被誉为“AI 世界的 USB-C 接口”[1][3]。它通过标准化的客户端-服务器架构，让大语言模型应用能够统一、安全地连接外部数据源和工具 [2][5]。MCP 采用 JSON-RPC 2.0 作为消息格式，核心由 Host、Client、Server 三部分组成 [4]，Server 通过 Tools（可执行操作）、Resources（数据资源）和 Prompts（提示模板）三大原语向外暴露能力 [7]。使用方式上，开发者既可以直接用现成的 MCP Server，也可以基于官方 SDK（Python/TypeScript/C#/Java 等）自行开发 [5]。

**详细说明**

**1. 什么是 MCP？——AI 的“USB-C”接口**  
在 MCP 出现之前，AI 应用每接入一个外部工具或数据源，都需要写一套专属适配器——用 OpenAI 的 Function Calling 写一套，用 Claude 的 Tool Use 再写一套，导致“N×M”的集成难题 [1]。MCP 正是为解决这个痛点而生：它定义了一套**统一的、开放的协议标准**，让任何兼容的 AI 应用都能用同一种方式调用任何兼容的工具和数据源 [6]。就像一个 USB-C 接口可以连接显示器、硬盘、充电器一样，MCP 让 AI 模型通过一套协议就能访问文件系统、数据库、API、开发工具等各种资源 [5]。截至 2026 年，GitHub 上已有超过 1000 个社区构建的 MCP 服务器 [7]。

**2. 架构：三层解耦，各司其职**  
MCP 采用**客户端-服务器架构**，核心包含三个角色 [2][4]：

- **MCP Host（主机）**：AI 应用本身（如 Claude Desktop、Cursor、IDE 插件等），负责协调和管理多个 MCP 客户端，是整个系统的“大脑”和调度中心 [1]。
- **MCP Client（客户端）**：Host 为每一个 MCP Server 创建的一个专用连接组件，负责与对应的 Server 建立会话、发现能力、转发请求、接收结果 [3]。Host 与 Client 通常运行在同一台机器上。
- **MCP Server（服务端）**：提供具体能力和数据的服务程序，暴露 Tools、Resources、Prompts 三大原语 [4]。Server 可以运行在本地（如文件系统服务），也可以部署在远程（如 Sentry 或云 API 服务）[6]。

Host 与每个 Server 之间通过一个专用的 Client 维持一对一的连接。这种设计保证了**安全隔离**[2]——每个 Server 只能看到自己相关的上下文，无法读取完整对话或其他 Server 的信息。

MCP 在技术上分为**两层** [7]：
- **数据层（Data Layer）**：基于 JSON-RPC 2.0 定义消息结构和语义，包含生命周期管理（初始化、能力协商、终止）、Server 功能（Tools/Resources/Prompts）、Client 功能（采样请求、日志记录）和工具功能（通知、进度追踪）[3]。
- **传输层（Transport Layer）**：管理通信通道和认证，支持 STDIO（主要用于本地 Server）和 Streamable HTTP/SSE（主要用于远程 Server）两种传输方式 [5]。

**3. 三大核心原语：Tools、Resources、Prompts**  
MCP Server 通过三种原语向外暴露能力 [4][7]：

- **Tools（工具）**：可执行的操作接口，类似于函数调用。AI 模型可以在推理过程中调用 Tool 来执行命令、调用 API、修改文件、查询数据库等，通常会改变系统状态 [1]。Tool 包含名称、描述、输入参数 Schema、输出结构、权限约束等信息 [3]。
- **Resources（资源）**：可供 AI 读取的数据，如文件、文档、代码仓库、数据库记录等 [2]。Resources 只提供上下文信息，不直接执行操作。
- **Prompts（提示词模板）**：预定义的交互模板和工作流，帮助用户和 AI 更高效地完成任务 [6]。

**4. MCP 怎么用？——两种角色，三种模式**  
实际使用 MCP 时，你通常扮演两种角色之一 [5]：

**角色一：作为 Client 调用现成的 MCP Server**  
- 在 AI 应用（如 Claude Desktop、Cursor）中配置 MCP Server 的连接信息 [1]。
- 应用启动后，Host 自动创建 Client 并连接 Server，发现 Server 提供的 Tools 和 Resources [2]。
- 当你在对话中提出需求，AI 模型会自动选择合适的 Tool 调用，Client 将请求转发给 Server 执行，结果返回后注入模型上下文继续推理 [4]。

**角色二：作为 Server 提供自己的能力和数据**  
- 使用官方 SDK（支持 Python、TypeScript、C#、Java 等）编写 MCP Server [6]。
- 在 Server 中注册你要暴露的 Tools、Resources 和 Prompts [7]。
- 启动 Server 后，任何兼容的 MCP Client 都可以发现并调用你提供的能力 [3]。

**三种部署/使用模式** [5]：
- **本地模式**：下载 MCP Server 代码在本地运行（类似引入一个 SDK），适用于文件系统、本地数据库等场景。
- **远程模式**：使用已部署的 MCP 服务（类似调用别人的 API），适用于云服务、企业级工具等场景。
- **混合模式**：同时连接多个本地和远程 Server，让 AI 模型获得最全面的能力组合。

---

### English Section

**Abstract**  
The Model Context Protocol (MCP) is an open standard released by Anthropic in November 2024, often dubbed the "USB-C port for AI" [1][3]. It uses a standardized client-server architecture to enable LLM applications to connect to external data sources and tools in a unified, secure manner [2][5]. MCP is built on JSON-RPC 2.0 and consists of three core components: Host, Client, and Server [4]. Servers expose capabilities via three primitives: Tools (executable actions), Resources (data), and Prompts (templated interactions) [7]. Developers can either consume existing MCP Servers or build their own using official SDKs (Python, TypeScript, C#, Java, etc.) [5].

**Detailed Explanation**

**1. What is MCP? – The "USB-C" for AI**  
Before MCP, every time an AI application needed to connect to a new tool or data source, developers had to write custom adapters – one for OpenAI's Function Calling, another for Claude's Tool Use, and so on – creating an "N×M" integration problem [1]. MCP was built to solve this: it defines a **single, open protocol standard** that allows any compatible AI application to call any compatible tool or data source in the same way [6]. Just as a USB-C port can connect to displays, hard drives, and chargers, MCP lets AI models access file systems, databases, APIs, development tools, and more through one unified protocol [5]. As of 2026, there are over 1,000 community-built MCP servers on GitHub [7].

**2. Architecture: Three-Layer Decoupling, Clear Responsibilities**  
MCP follows a **client-server architecture** with three core roles [2][4]:

- **MCP Host**: The AI application itself (e.g., Claude Desktop, Cursor, IDE plugins) that coordinates and manages multiple MCP clients – the "brain" and orchestrator of the system [1].
- **MCP Client**: A dedicated connection component created by the Host for each MCP Server. It handles session establishment, capability discovery, request forwarding, and result reception [3]. Host and Client typically run on the same machine.
- **MCP Server**: A service program that provides specific capabilities and data, exposing Tools, Resources, and Prompts [4]. Servers can run locally (e.g., file system service) or remotely (e.g., Sentry or cloud APIs) [6].

The Host maintains a dedicated one-to-one connection with each Server via a dedicated Client. This design ensures **security isolation** [2] – each Server can only see its own relevant context and cannot read the full conversation or access other Servers' information.

Technically, MCP consists of **two layers** [7]:
- **Data Layer**: Built on JSON-RPC 2.0, it defines message structure and semantics, covering lifecycle management (initialization, capability negotiation, termination), Server features (Tools/Resources/Prompts), Client features (sampling requests, logging), and utility features (notifications, progress tracking) [3].
- **Transport Layer**: Manages communication channels and authentication, supporting STDIO (primarily for local Servers) and Streamable HTTP/SSE (primarily for remote Servers) [5].

**3. Three Core Primitives: Tools, Resources, Prompts**  
MCP Servers expose capabilities through three primitives [4][7]:

- **Tools**: Executable operation interfaces, similar to function calls. AI models can invoke Tools during reasoning to run commands, call APIs, modify files, query databases, etc. – typically changing system state [1]. Each Tool includes name, description, input parameter Schema, output structure, and permission constraints [3].
- **Resources**: Data that AI can read, such as files, documents, code repositories, database records [2]. Resources provide contextual information without directly executing operations.
- **Prompts**: Predefined interaction templates and workflows that help users and AI accomplish tasks more efficiently [6].

**4. How is MCP Used? – Two Roles, Three Modes**  
In practice, you typically play one of two roles [5]:

**Role 1: As a Client consuming existing MCP Servers**  
- Configure MCP Server connection information in your AI application (e.g., Claude Desktop, Cursor) [1].
- After launch, the Host automatically creates Clients, connects to Servers, and discovers available Tools and Resources [2].
- When you make a request in conversation, the AI model automatically selects the appropriate Tool to call; the Client forwards the request to the Server, and the result is injected back into the model's context for continued reasoning [4].

**Role 2: As a Server providing your own capabilities and data**  
- Write an MCP Server using official SDKs (Python, TypeScript, C#, Java, etc.) [6].
- Register the Tools, Resources, and Prompts you want to expose [7].
- Once running, any compatible MCP Client can discover and call your capabilities [3].

**Three deployment/usage modes** [5]:
- **Local Mode**: Run MCP Server code locally (similar to importing an SDK) – suitable for file systems, local databases, etc.
- **Remote Mode**: Use already-deployed MCP services (similar to calling someone else's API) – suitable for cloud services, enterprise tools, etc.
- **Hybrid Mode**: Connect to multiple local and remote Servers simultaneously, giving the AI model the broadest possible combination of capabilities.

---

## 3. References

[1] MCP Official Documentation – Architecture Overview. URL: https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture.md

[2] MCP Specification – Overview. URL: https://modelcontextprotocol.io/specification/2026-07-28/index.md

[3] MCP Official Docs – Architecture (2024-11-05). URL: https://modelcontextprotocol.io/docs/2024-11-05/learn/architecture

[4] MCP Specification – Architecture (2025-11-25). URL: https://modelcontextprotocol.io/specification/2025-11-25/architecture/index.md

[5] 腾讯云 – MCP 协议详解：AI 时代的“USB-C”. URL: https://cloud.tencent.cn/developer/article/2659252

[6] 腾讯云 – MCP 到底是什么？一文带你彻底搞懂. URL: https://developer.cloud.tencent.cn/article/2665432

[7] Wikipedia – Model Context Protocol. URL: https://en.wikipedia.org/wiki/Model_Context_Protocol