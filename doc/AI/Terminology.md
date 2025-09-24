## 一些术语
### 大语言模型（LLM, Large Language Model）
**定义：** LLM = 一种训练好的模型（比如 GPT-4、LLaMA2、Claude、Qwen）。  
> 注意，这里的GPT-4 并不是 Chatgpt 的意思，而是chatgpt 使用的大模型

**能力：** 主要是“理解语言 + 生成语言”，属于原生模型。

👉 举例：
- LLaMA2（Meta 开源）
- GPT-4（OpenAI 云端）
- Claude 3（Anthropic）
这些都属于 LLM。  


LLM：像 GPT-4、LLaMA2、Claude → 原生模型，大脑。  
**特点：** 
- 会思考、会输出答案，但自己不会去找外部信息或执行任务。  
- 单次请求 → 单次响应。  
- 不会自动帮你规划任务，需要你明确告诉它做什么。  

**使用场景**
- 问答对话
- 文本生成（文章、代码）
- 翻译、总结

#### LLM，Model, Agent
- Model: 指调用open ai时，指定的model。其实就是LLM
 ```json
{
  "model": "gpt-4o-mini",
  "messages": [
    {"role": "user", "content": "帮我写一个Python冒泡排序"}
  ]
}

```
- Agent：在 LLM 上加工具、记忆、逻辑 → 会执行任务的助手。
- ChatGPT：就是一个 Agent 产品（因为它用 LLM + 工具 + 记忆 + UI 封装出来的）。

### Agent
**定义：**  
基于模型之上的“应用逻辑层”。Agent 不是一个新的模型，而是 用模型作为大脑，再加上 记忆、工具调用、任务规划 等能力。

**特点：**  
- 可以自主规划多步任务，而不是只回答一次。  
- 能调用外部工具/插件（搜索、计算器、数据库 API）。  
- 有“状态”和“记忆”，能在多轮对话中保持上下文目标。
- 更像是一个 会思考和行动的助手，而不是单纯的语言模型。

**例子：**  
OpenAI 的 Assistant API 支持 Agent 化：
```json
{
  "assistant": {
    "name": "旅行助手",
    "instructions": "帮用户规划旅行",
    "tools": ["browser", "python"]
  }
}
```
这样它就能根据用户输入：
1. 自动调用浏览器搜索机票 
2. 用 Python 工具计算预算 
3. 再综合生成旅行方案

而不是一问一答了，而是有自己的行为去自己查资料  

**类比理解**  
Model = 大脑  
→ 会思考、会输出答案，但自己不会去找外部信息或执行任务。

Agent = 大脑 + 身体 + 工具  
→ 不仅能思考，还能“行动”：查资料、运行程序、调用 API。

**趋势**  
OpenAI、Anthropic、Google 等厂商都在推动 从单纯的模型调用   
→ Agent 框架。  
未来 AI 可能不是单一对话模型，而是一个能帮你完成复杂任务的 数字员工。

**现实中 Agent 的“出生方式”**  
公司自己研发：  
- 比如微软 Copilot，其实就是把 GPT 模型 + Office 工具 API + UI 结合起来。

基于框架搭建：
- 开发者用 LangChain、LlamaIndex、Haystack 等开源框架，快速定制一个 Agent。

直接用官方 API：
- OpenAI 的 Assistants API 就是官方提供的“Agent 搭建平台”，开发者只要定义好 指令 + 工具 + 数据，模型就能变成一个 Agent。

#### 本地搭建Agent

**有三种方式搭建本地Agent**  
- Function Calling
  - 使用简单，单一的任务
  - 刚出来时的方式
- LangChain Agent
  - 功能较强 
- Assistants API
  - “云端记忆+内置工具，只要丢任务就行” → 快速做持久化助手



下边这个例子是使用 LangChain 框架搭建 agent
**🛠️ 1. 你需要准备的环境**  
编程语言：推荐 Python（生态成熟，AI 框架最多）。  
硬件：普通电脑即可（如果要跑大模型，最好有 GPU；否则可以调用 API）。  
依赖： Python 3.9+  pip 包管理工具

**🧩 2. 核心组件（搭建 Agent 必须的）**  
一个 Agent 一般由以下部分组成：
- 大脑（LLM）  
  - 本地模型：如 LLaMA2、Mistral、Qwen、ChatGLM，可以用 Ollama 或 vLLM 启动。 
  - API 调用：如 OpenAI 的 GPT、Anthropic 的 Claude。

- 工具调用（Tools）
  - 浏览器、数据库、Python 解释器、文件操作等。

- 记忆（Memory）
  - 短期记忆：对话上下文。 
  - 长期记忆：向量数据库（如 Chroma、Weaviate、Milvus）。

- 逻辑框架（Agent 框架）
  - LangChain （最流行，功能全面） 
  - LlamaIndex （偏重知识接入） 
  - AutoGen （微软开源，支持多 Agent 协作）

**⚙️ 3. 本地搭建流程（一个最小可用的例子）**  
假设你用 Ollama + LangChain 在本地跑一个简单 Agent：  
步骤 1：安装环境
```bash
# 安装 ollama（本地模型运行器）
brew install ollama   # Mac
# 或者到 https://ollama.ai 下载

# 安装 LangChain 和依赖
pip install langchain langchain-community chromadb
```

步骤 2：下载一个本地模型
```bash
ollama pull llama2
```

步骤 3：写一个简单的 Agent 脚本
```bash
from langchain.llms import Ollama
from langchain.agents import initialize_agent, load_tools

# 1. 载入本地模型（大脑）
llm = Ollama(model="llama2")

# 2. 加载工具（比如搜索、计算器）
tools = load_tools(["serpapi", "llm-math"], llm=llm)

# 3. 初始化 Agent
agent = initialize_agent(
    tools, llm, agent="zero-shot-react-description", verbose=True
)

# 4. 运行 Agent
agent.run("帮我查一下明天北京的天气，并计算适合穿什么衣服")
```

👉 这样你就有了一个最小的“本地 Agent”，它会：
- 理解你的问题 
- 调用工具（搜索、计算） 
- 给出最终答案

**🚀 4. 进阶玩法**
- 加知识库：用 LlamaIndex 或 Chroma，把公司文档接入，让 Agent 变成“企业客服”。 
- 多 Agent 协作：用 AutoGen 搭建多个 Agent，让它们分工（比如“研究员 Agent + 程序员 Agent”）。 
- 可视化界面：用 Gradio 或 Streamlit 搭建前端界面，让你像用 ChatGPT 一样操作。

**🎯 总结**
- 最小路径：Ollama（本地模型） + LangChain（Agent 框架）。
- 扩展路径：加上向量数据库（长期记忆）、工具 API（行动能力）、界面（用户交互）。 
- 最终效果：你的本地 Agent 不仅能聊天，还能查资料、写代码、回答和你业务相关的问题。


### RAG
RAG 是 Retrieval-Augmented Generation（检索增强生成） 的缩写  
RAG 的核心理念是：
> 在生成回答之前，先从一个外部知识库中检索相关信息，然后再用语言模型生成结合这些信息的答案。

#### RAG 的工作流程如下：
- Query（用户提问）  
用户向系统输入一个问题或请求。

- Retrieval（检索）  
系统使用向量搜索等方法，在一个知识库（可以是文档、网页、数据库等）中找到与问题相关的内容。

- Augmentation（增强）  
检索到的内容被送入语言模型，作为“上下文”或“提示（prompt）”的一部分。

- Generation（生成）  
语言模型基于问题和相关文档生成最终回答。


