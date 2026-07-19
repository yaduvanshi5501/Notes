# 🤖 ChatModel in Spring AI

## What is ChatModel?

`ChatModel` is the **low-level core interface** in Spring AI that directly communicates with AI providers such as:

- OpenAI
- Azure OpenAI
- Anthropic
- Ollama
- Gemini

Think of it as:

> **"The engine behind ChatClient that directly talks to the AI model."**

Unlike `ChatClient`, `ChatModel` gives you **more control** over how requests are sent to the AI.

---

# Why Use ChatModel?

- Direct communication with AI models.
- Provider-independent API.
- More configurable than `ChatClient`.
- Returns detailed responses (`ChatResponse`).
- Suitable for advanced and custom AI applications.

---

# How ChatModel Works

```text
User Prompt
      │
      ▼
    Prompt
      │
      ▼
  ChatModel
      │
      ▼
 AI Provider
(OpenAI / Azure / Ollama)
      │
      ▼
 ChatResponse
```

---

# Multiple Chat Models

Sometimes one application needs multiple AI models.

### Examples

- 🧠 Powerful model for complex reasoning.
- ⚡ Fast model for simple questions.
- 🔄 Backup model if one provider fails.
- 👨‍💻 One model for coding and another for creative writing.

---

# Default Spring AI Behavior

By default, Spring AI:

- Creates **one ChatClient.Builder**
- Connects it to **one ChatModel**
- Uses configuration from `application.properties`

Suitable when using only one AI model.

---

# Multiple ChatModels

If you want multiple models, disable auto-configuration:

```properties
spring.ai.chat.client.enabled=false
```

Then create multiple `ChatModel` and `ChatClient` beans manually.

---

# ChatModel Configuration

```java
@Bean
public ChatModel chatModel() {
    ...
}
```

### Purpose

Creates and configures a reusable `ChatModel` bean.

Configuration includes:

- API Key
- Endpoint
- Deployment Name
- Temperature
- Max Tokens

---

# Important Configuration Options

| Option | Purpose |
|---------|---------|
| **Deployment Name** | Selects the AI model to use. |
| **Temperature** | Controls creativity (Low = Accurate, High = Creative). |
| **Max Tokens** | Limits the maximum response length. |

---

# Multiple ChatClients

```java
@Bean
public ChatClient creativeChatClient(ChatModel creativeChatModel) {
    return ChatClient.create(creativeChatModel);
}

@Bean
public ChatClient preciseChatClient(ChatModel preciseChatModel) {
    return ChatClient.create(preciseChatModel);
}
```

### Example

- 🎨 `creativeChatClient` → Creative writing.
- 📚 `preciseChatClient` → Accurate technical answers.

---

# ChatModel vs ChatClient

| Feature | ChatModel | ChatClient |
|---------|-----------|------------|
| Level | Low-Level | High-Level |
| Purpose | Direct AI communication | Easy AI interaction |
| API Style | Simple API | Fluent API |
| Conversation Memory | ❌ No | ✅ Yes |
| Streaming | Basic | Built-in |
| Tool Calling | Basic | Advanced |
| Structured Output | ❌ Manual | ✅ Supported |
| Best For | Advanced configuration | Most applications |

---

# Which One Should You Use?

### ✅ Use `ChatClient`

- Most Spring Boot applications
- Chatbots
- REST APIs
- Production applications

### ✅ Use `ChatModel`

- Advanced customization
- Multiple AI models
- Framework development
- Low-level AI integration

> **Note:** `ChatClient` internally uses `ChatModel` to communicate with the AI provider.

---

# Quick Revision

- **ChatModel** → Low-level interface for communicating with AI models.
- **ChatClient** → High-level, developer-friendly API built on top of `ChatModel`.
- `ChatModel` returns **ChatResponse** with metadata.
- `ChatClient` provides a **Fluent API**, conversation memory, streaming, and structured output.
- Disable auto-configuration (`spring.ai.chat.client.enabled=false`) when using multiple `ChatModel` instances.
- Use **ChatClient** for most applications and **ChatModel** only when advanced control is required.



<br><br><br>
---
# 🤖 ChatModel Demo

## 🎯 Use Case

This demo shows how to use **ChatModel** (the low-level Spring AI API) to communicate directly with an AI model.

The application:

- Accepts a user prompt from the request.
- Sends it to the AI model.
- Returns the AI-generated response.
- Demonstrates how to retrieve **token usage metadata**.

---

# 🏗️ Flow

```text
User
   │
   ▼
REST Controller
   │
   ▼
ChatModel
   │
   ▼
Azure OpenAI
   │
   ▼
ChatResponse
   │
   ▼
User
```

---

# 🔐 Configuration

Store sensitive values in **Environment Variables** instead of hardcoding them.

```java
@Value("${azure.openai.api-key}")
private String apiKey;

@Value("${azure.openai.endpoint}")
private String endpoint;

@Value("${azure.openai.chat.options.deployment-name}")
private String deploymentName;
```

### Purpose

- API Key → Authentication
- Endpoint → Azure OpenAI URL
- Deployment Name → AI model to use

---

# 🏗️ ChatModel Bean

```java
@Bean
public ChatModel chatModel() {
    ...
}
```

Creates and configures a reusable `ChatModel` bean.

### Default Configuration

| Option | Purpose |
|---------|---------|
| Deployment Name | Selects the AI model |
| Temperature | Controls creativity |
| Max Tokens | Limits response length |

---

# 🌡️ Temperature

Controls how creative the AI should be.

| Value | Behavior |
|-------|----------|
| **0.0** | Very accurate and deterministic |
| **0.3 – 0.5** | Balanced (Recommended) |
| **0.7** | Creative but controlled |
| **1.0** | Highly creative |

---

# 🔢 Max Tokens

Limits the maximum number of tokens the AI can generate.

Benefits:

- Controls response length
- Reduces API cost
- Prevents excessively long outputs

---

# Endpoint 1 : Simple Chat

```java
@GetMapping("/ask")
```

Uses:

```java
chatModel.call(userPrompt);
```

### Flow

```text
User Prompt
      │
      ▼
chatModel.call(String)
      │
      ▼
Azure OpenAI
      │
      ▼
AI Response (String)
```

Use when you only need the generated text.

---

# Endpoint 2 : Chat with Metadata

```java
@GetMapping("/ask-with-metadata")
```

Creates a structured prompt.

```java
Prompt prompt = new Prompt(
    List.of(
        new SystemMessage(...),
        new UserMessage(...)
    )
);
```

Calls:

```java
ChatResponse response = chatModel.call(prompt);
```

---

# ChatResponse

Unlike `call(String)`, this returns a **ChatResponse** object.

It contains:

- AI-generated text
- Prompt Tokens
- Completion Tokens
- Total Tokens
- Model Metadata

---

# Token Usage

```java
response.getMetadata().getUsage()
```

Returns:

- Prompt Tokens
- Completion Tokens
- Total Tokens

### Why are Tokens Important?

Most AI providers charge based on token usage.

Monitoring tokens helps:

- Reduce cost
- Optimize prompts
- Track API usage

---

# Two Ways to Call ChatModel

## 1️⃣ Simple Call

```java
chatModel.call(userPrompt);
```

- Sends a simple user message.
- Returns only the generated text.

---

## 2️⃣ Structured Call

```java
chatModel.call(prompt);
```

- Accepts System + User messages.
- Returns a `ChatResponse` with metadata.

---

# ChatModel vs ChatClient

| ChatModel | ChatClient |
|-----------|------------|
| Low-Level API | High-Level API |
| Direct AI communication | Fluent API |
| More control | Easier to use |
| Returns `ChatResponse` | Returns text or structured response |
| Best for advanced use cases | Best for most applications |

---

# Complete Flow

```text
User Request
      │
      ▼
Controller
      │
      ▼
Create Prompt
(System + User)
      │
      ▼
ChatModel
      │
      ▼
Azure OpenAI
      │
      ▼
ChatResponse
      │
      ├── AI Text
      ├── Prompt Tokens
      ├── Completion Tokens
      └── Total Tokens
      │
      ▼
Return Response
```

---

# 📝 Quick Revision

- **ChatModel** → Low-level API to communicate directly with AI models.
- `call(String)` → Sends a simple user message and returns only text.
- `call(Prompt)` → Sends structured prompts and returns a `ChatResponse`.
- **ChatResponse** → Contains generated text + metadata.
- **Temperature** → Controls creativity.
- **Max Tokens** → Controls response length.
- **Token Usage** → Helps monitor API cost and usage.
- Use **ChatModel** when you need direct control; use **ChatClient** for most Spring AI applications.