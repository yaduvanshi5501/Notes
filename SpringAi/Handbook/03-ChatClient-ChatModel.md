# 🚀 Spring AI Handbook

# 03 - ChatClient & ChatModel

> **Goal:** Master the two most important classes in Spring AI—**ChatClient** and **ChatModel**. Learn how they work, their differences, internal architecture, complete request lifecycle, important methods, and best practices.

---

# 📖 Table of Contents

1. Introduction
2. What is ChatClient?
3. Why ChatClient?
4. What is ChatModel?
5. Why ChatModel?
6. ChatClient vs ChatModel
7. Internal Architecture
8. Complete Request Flow
9. ChatClient Builder
10. Important Methods
11. Prompt Creation
12. Response Handling
13. Streaming
14. Structured Output
15. Metadata
16. When to Use ChatClient
17. When to Use ChatModel
18. Best Practices
19. Common Mistakes
20. Interview Questions
21. Quick Revision
22. Master Mind Map

---

# 1️⃣ Introduction

Whenever a user asks an AI question, Spring AI needs a way to communicate with the AI model.

Spring AI provides two APIs for this purpose:

- ChatClient (High-Level API)
- ChatModel (Low-Level API)

Both eventually call the AI provider, but they are designed for different purposes.

---

# 2️⃣ What is ChatClient?

**ChatClient** is the primary API for interacting with Large Language Models (LLMs) in Spring AI.

It provides a fluent API (method chaining) to build prompts, send requests, and receive responses with minimal code.

Think of it as a **wrapper around ChatModel** that makes AI interactions simple.

---

## Responsibilities of ChatClient

- Build prompts
- Send requests
- Handle responses
- Support streaming
- Support structured output
- Integrate advisors
- Simplify AI calls

---

## ChatClient Flow

```text
Application

     │

     ▼

ChatClient

     │

     ▼

Prompt

     │

     ▼

ChatModel

     │

     ▼

AI Provider

     │

     ▼

LLM
```

---

# 3️⃣ Why ChatClient?

Without ChatClient:

```text
Controller

↓

HTTP Request

↓

Headers

↓

Authentication

↓

JSON

↓

Provider API

↓

JSON Parsing
```

A lot of manual work is required.

---

With ChatClient:

```text
Controller

↓

ChatClient

↓

AI Response
```

Advantages:

- Less code
- Fluent API
- Easy to read
- Provider independent
- Built-in Spring integration

---

# 4️⃣ What is ChatModel?

ChatModel is the **low-level abstraction** responsible for communicating directly with the AI provider.

It knows:

- Which model to call
- How to serialize requests
- How to deserialize responses
- Provider-specific implementation

---

## ChatModel Flow

```text
ChatClient

      │

      ▼

ChatModel

      │

      ▼

Azure/OpenAI/Ollama

      │

      ▼

LLM
```

---

# 5️⃣ Why ChatModel?

Spring AI supports multiple AI providers.

Instead of writing provider-specific code everywhere, Spring AI defines the `ChatModel` interface.

Each provider implements it.

```text
ChatModel

├── AzureChatModel

├── OpenAiChatModel

├── OllamaChatModel

└── HuggingFaceChatModel
```

---

# 6️⃣ ChatClient vs ChatModel

| Feature | ChatClient | ChatModel |
|----------|------------|-----------|
| API Level | High-Level | Low-Level |
| Recommended | ✅ Yes | Only when needed |
| Fluent API | ✅ | ❌ |
| Prompt Builder | ✅ | ❌ |
| Streaming | ✅ | Depends |
| Structured Output | ✅ | Manual |
| Simplicity | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Direct Provider Access | ❌ | ✅ |

---

# 7️⃣ Internal Architecture

```text
                Spring Boot

                     │

                     ▼

              ChatClient Bean

                     │

                     ▼

                Prompt Builder

                     │

                     ▼

                ChatModel Bean

                     │

                     ▼

           Provider Adapter Layer

      ┌───────────┬─────────────┬─────────────┐
      │           │             │
 Azure OpenAI   OpenAI       Ollama

                     │

                     ▼

             Large Language Model

                     │

                     ▼

               ChatResponse
```

---

# 8️⃣ Complete Request Lifecycle

```text
User

↓

REST Controller

↓

ChatClient

↓

Prompt

↓

ChatModel

↓

Provider Adapter

↓

HTTP Request

↓

LLM

↓

HTTP Response

↓

ChatResponse

↓

content()

↓

User
```

---

# 9️⃣ ChatClient Builder Pattern

ChatClient uses the Builder pattern to create readable AI requests.

Typical flow:

```text
ChatClient

↓

prompt()

↓

user()

↓

system()

↓

call()

↓

content()
```

Benefits:

- Readable
- Flexible
- Method chaining
- Easy customization

---

# 🔟 Important ChatClient Methods

## prompt()

Creates a new prompt.

---

## user()

Adds the user's message.

---

## system()

Adds system instructions.

Example:

```text
"You are a Java expert."
```

---

## call()

Sends the request to the AI model.

---

## content()

Returns plain text.

---

## entity()

Returns a Java object (structured output).

---

## stream()

Streams the response token by token.

---

## chatResponse()

Returns the complete response object.

---

# 1️⃣1️⃣ Prompt Creation

A prompt can contain multiple messages.

```text
Prompt

├── System Message

├── User Message

├── Assistant Message

└── Tool Message
```

---

### Example Conversation

```text
System:
You are a Java Tutor.

↓

User:
Explain Spring Boot.

↓

Assistant:
Spring Boot is...
```

---

# 1️⃣2️⃣ Response Handling

Spring AI wraps the provider response inside `ChatResponse`.

```text
ChatResponse

├── Content

├── Metadata

├── Usage

├── Finish Reason

└── Generated Messages
```

---

# 1️⃣3️⃣ Streaming

Normally:

```text
AI

↓

Complete Response

↓

User
```

Streaming:

```text
AI

↓

Word

↓

Word

↓

Word

↓

User
```

Advantages:

- Faster UI
- Better user experience
- Lower perceived latency

---

# 1️⃣4️⃣ Structured Output

Instead of returning plain text, Spring AI can map responses directly into Java objects.

```text
LLM

↓

JSON

↓

Spring AI

↓

Java Object
```

Advantages:

- Type safety
- Easier parsing
- Cleaner code

---

# 1️⃣5️⃣ Metadata

A response may include additional information.

Examples:

- Model name
- Token usage
- Finish reason
- Response ID
- Provider information

---

# 1️⃣6️⃣ When to Use ChatClient

Use ChatClient when:

- Building REST APIs
- Chatbots
- AI assistants
- Question answering
- Document summarization
- Translation
- Code generation

This is the recommended choice for most applications.

---

# 1️⃣7️⃣ When to Use ChatModel

Use ChatModel only when:

- You need provider-specific features.
- You are building a custom abstraction.
- You require low-level control.
- You are extending Spring AI.

---

# 1️⃣8️⃣ Best Practices

- Prefer ChatClient over ChatModel.
- Use `system()` to define AI behavior.
- Keep prompts concise.
- Use structured output for JSON data.
- Stream long responses.
- Validate AI-generated content.
- Reuse ChatClient bean.

---

# 1️⃣9️⃣ Common Mistakes

❌ Creating ChatClient manually every request.

❌ Using ChatModel for simple tasks.

❌ Sending extremely large prompts.

❌ Ignoring token limits.

❌ Not handling provider exceptions.

❌ Mixing business logic with controller logic.

---

# 2️⃣0️⃣ Interview Questions

### What is ChatClient?

A high-level Spring AI API for communicating with Large Language Models using a fluent interface.

---

### What is ChatModel?

A low-level abstraction that communicates directly with AI providers.

---

### Why is ChatClient preferred?

Because it reduces boilerplate code, supports fluent APIs, structured output, streaming, and integrates well with Spring Boot.

---

### Can ChatClient work without ChatModel?

No.

Internally, ChatClient delegates requests to ChatModel.

---

### Which class actually communicates with OpenAI or Azure?

ChatModel.

---

### What is the Builder Pattern used by ChatClient?

It allows method chaining to create prompts in a clean and readable way.

---

# 2️⃣1️⃣ Quick Revision

```text
User

↓

Controller

↓

ChatClient

↓

Prompt

↓

ChatModel

↓

Provider

↓

LLM

↓

ChatResponse

↓

content()

↓

User
```

---

# 2️⃣2️⃣ Master Mind Map

```text
ChatClient
│
├── High-Level API
├── Fluent Builder
├── prompt()
├── user()
├── system()
├── call()
├── stream()
├── content()
├── entity()
├── chatResponse()
│
└──────────────┐
               ▼
          ChatModel
               │
               ├── Low-Level API
               ├── Provider Interface
               ├── Azure
               ├── OpenAI
               ├── Ollama
               └── Hugging Face
                    │
                    ▼
             Large Language Model
                    │
                    ▼
              ChatResponse
```

---

# 📝 Summary

After completing this chapter, you should be able to:

- Explain the purpose of ChatClient and ChatModel.
- Describe the differences between them.
- Understand the complete request lifecycle.
- Use important ChatClient methods conceptually.
- Explain how prompts are built.
- Understand response handling, metadata, streaming, and structured output.
- Decide when to use ChatClient versus ChatModel.

> **Next File:** [📂 04-Prompt-Engineering.md](./04-Prompt-Engineering.md)