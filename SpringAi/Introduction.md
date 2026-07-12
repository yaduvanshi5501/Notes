    # 🌱 What is Spring AI?

**Spring AI** is an **open-source framework** that helps Java developers integrate **Generative AI** and **Large Language Models (LLMs)** into Spring Boot applications.

It follows the same Spring principles like **Dependency Injection**, **Auto-Configuration**, and **Reusable Abstractions**, making AI integration simple and developer-friendly.

---

# ❓ Why Do We Need Spring AI?

Integrating AI directly with providers like **OpenAI** or **Anthropic** can be difficult because each provider has its own SDK, APIs, and request formats.

Spring AI solves this by:

- Providing a common API for different AI providers.
- Reducing boilerplate code.
- Making it easy to switch between AI providers.
- Allowing developers to focus on business logic instead of AI integration.

---

# ✨ Key Features

### 🔗 Spring Ecosystem Integration

Works seamlessly with:

- Spring Boot
- Spring Data
- Spring Batch
- Spring Integration

---

### 🤖 Unified AI APIs

Provides common interfaces to work with different AI providers.

Supported Providers:

- OpenAI
- Azure OpenAI
- Anthropic
- Google Gemini
- AWS Bedrock
- Hugging Face
- Ollama

---

### 🎯 Multi-Modal Support

Supports multiple AI capabilities like:

- 💬 Chat
- 🧠 Embeddings
- 🖼️ Image Generation
- 🎤 Audio Transcription
- 🔊 Text-to-Speech
- 🛡️ Content Moderation

---

### ⚡ Simplified AI Development

Spring AI provides built-in support for:

- Prompt Templates
- Structured Outputs (Java POJOs)
- Function/Tool Calling
- Request & Response Handling

---

### 📚 RAG (Retrieval-Augmented Generation)

Supports:

- Vector Databases
- Semantic Search
- Document Q&A
- Conversational Memory

---

### 🏢 Enterprise Ready

Provides support for:

- Auto Configuration
- Micrometer Observability
- GraalVM Native Images
- Docker & Testcontainers
- Virtual Threads

---

# 🏗️ Spring AI Architecture

Spring AI consists of **two main parts**:

```
Application
      │
      ▼
  Spring AI
      │
 ┌───────────────┐
 │               │
 ▼               ▼
Key         Model
Abstractions Providers
```

---

## 🤖 Model Providers

Spring AI can connect to different AI providers.

Examples:

- OpenAI
- Azure OpenAI
- Anthropic
- Ollama
- Hugging Face
- AWS Bedrock
- Vertex AI
- Cohere

> **Benefit:** You can switch providers with minimal code changes.

---

## 🧩 Key Abstractions

Spring AI provides common interfaces that hide provider-specific implementation details.

| Abstraction | Purpose |
|-------------|---------|
| **ChatClient** | Send prompts and receive AI responses. |
| **EmbeddingClient** | Convert text into vector embeddings. |
| **ImageClient** | Generate or process images. |
| **AudioClient** | Speech recognition and transcription. |
| **VectorStore** | Store and search embeddings. |
| **PromptTemplate** | Create reusable dynamic prompts. |
| **TokenCounter** | Estimate token usage. |
| **Function Calling** | Allow AI to call external tools or APIs. |

---

# 💡 Common Use Cases

Spring AI can be used to build:

- 💬 AI Chatbots
- 📄 Document Q&A Systems
- 📝 Text Summarization
- 🏷️ Text Classification
- 😊 Sentiment Analysis
- 💻 Code Generation
- ✍️ Content Creation
- 🌍 Language Translation
- 🔍 Semantic Search
- 📊 Decision Support Systems

---