# 🚀 Spring AI Handbook

# 01 - Spring AI Architecture

> **Goal:** Understand how Spring AI works internally, its architecture, request flow, providers, and the core components required before building any Spring AI application.

---

# 📖 Table of Contents

1. What is Spring AI?
2. Why Spring AI?
3. Features
4. Spring AI Ecosystem
5. Architecture
6. Request Lifecycle
7. AI Providers
8. Core Components
9. High-Level vs Low-Level APIs
10. Component Relationships
11. Project Flow
12. Best Practices
13. Common Mistakes
14. Interview Questions
15. Quick Revision
16. Master Mind Map

---

# 1️⃣ What is Spring AI?

Spring AI is a **Spring Framework project** that helps Java developers integrate **Generative AI** and **Large Language Models (LLMs)** into Spring Boot applications using familiar Spring programming concepts.

Instead of manually calling REST APIs, Spring AI provides Java abstractions like:

- ChatClient
- ChatModel
- Prompt
- PromptTemplate
- ImageModel
- Advisors
- Structured Output
- Streaming APIs

---

# 2️⃣ Why Spring AI?

## Without Spring AI

```text
Application
      │
      ▼
HTTP Client
      │
Authentication
      │
Headers
      │
JSON Request
      │
Error Handling
      │
JSON Parsing
      │
OpenAI API
```

Problems

- Lots of boilerplate code
- Manual REST calls
- JSON parsing
- Authentication handling
- Vendor specific code

---

## With Spring AI

```text
Application
      │
      ▼
ChatClient
      │
      ▼
Spring AI
      │
      ▼
Azure/OpenAI/Ollama
```

Benefits

- Less code
- Easy configuration
- Provider independent
- Spring Boot integration
- Prompt templates
- Structured Output
- Streaming
- Multimodality

---

# 3️⃣ Key Features

- Chat Completion
- Image Generation
- Audio Transcription
- PDF Processing
- Prompt Templates
- Structured Output
- Advisors
- Streaming Responses
- Provider Independence
- Spring Boot Integration

---

# 4️⃣ Spring AI Ecosystem

```text
                     SPRING AI

              Spring Boot Application
                        │
                        ▼
                  Spring AI Layer
                        │
     ┌──────────────────┼──────────────────┐
     │                  │                  │
 Chat APIs         Image APIs         Audio APIs
     │                  │                  │
     └──────────────────┼──────────────────┘
                        │
                 AI Provider Layer
                        │
      ┌──────────┬──────────┬──────────┐
      │          │          │          │
Azure OpenAI   OpenAI    Ollama   Hugging Face
                        │
                        ▼
                  Large Language Model
                        │
                        ▼
                   AI Generated Output
```

---

# 5️⃣ Spring AI Architecture

```text
                    User

                     │

                     ▼

             Browser / Postman

                     │

                     ▼

            Spring Boot Controller

                     │

                     ▼

               ChatClient API

                     │

                     ▼

                   Prompt

                     │

                     ▼

                 ChatModel

                     │

                     ▼

             AI Provider Adapter

                     │

      ┌──────────────┼───────────────┐
      │              │               │
 Azure OpenAI     OpenAI         Ollama

                     │

                     ▼

             Large Language Model

                     │

                     ▼

               ChatResponse

                     │

        ┌────────────┼────────────┐
        │                         │
   content()                 entity()

                     │

                     ▼

                  JSON Output
```

---

# 6️⃣ Complete Request Lifecycle

```text
User

↓

Browser

↓

@RestController

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

AI Response

↓

ChatResponse

↓

content()

↓

Browser
```

---

# 7️⃣ AI Providers

## Azure OpenAI

✔ Enterprise

✔ Secure

✔ Microsoft Azure

✔ Best for Production

---

## OpenAI

✔ GPT Models

✔ Easy Integration

✔ General Purpose

---

## Ollama

✔ Local Models

✔ Offline

✔ Free

✔ No Internet Required

---

## Hugging Face

✔ Open Source Models

✔ Research

✔ Community Models

---

# Provider Comparison

| Provider | Internet | Enterprise | Local | Cost |
|-----------|----------|------------|-------|------|
| Azure OpenAI | ✅ | ⭐⭐⭐⭐⭐ | ❌ | Paid |
| OpenAI | ✅ | ⭐⭐⭐⭐ | ❌ | Paid |
| Ollama | ❌ | ⭐⭐ | ✅ | Free |
| Hugging Face | Depends | ⭐⭐⭐ | Optional | Mixed |

---

# 8️⃣ Core Components

## ChatClient

High-level API.

Responsible for communicating with AI.

Most used class in Spring AI.

### Important Methods

```java
prompt()

user()

system()

call()

stream()

content()

entity()

chatResponse()
```

---

## ChatModel

Low-level abstraction.

Actually communicates with the provider.

ChatClient internally uses ChatModel.

---

## Prompt

Represents complete AI input.

Contains

- UserMessage
- SystemMessage
- AssistantMessage
- Media
- Options

---

## ChatResponse

Represents AI output.

Contains

```java
content()

entity()

metadata()

usage()

finishReason()
```

---

## Message Types

```text
Message

├── UserMessage

├── SystemMessage

├── AssistantMessage

└── ToolMessage
```

---

# 9️⃣ High-Level vs Low-Level API

## High-Level

```text
ChatClient
```

Advantages

- Easy
- Less Code
- Recommended

---

## Low-Level

```text
ChatModel
```

Advantages

- More Control
- Custom Requests
- Internal Processing

---

# 🔟 Component Relationship

```text
ChatClient

      │

      ▼

Prompt

      │

      ▼

ChatModel

      │

      ▼

Provider

      │

      ▼

LLM

      │

      ▼

ChatResponse
```

---

# 1️⃣1️⃣ Typical Spring AI Project Flow

```text
Create Spring Boot Project

↓

Add Dependencies

↓

Configure API Keys

↓

Create Controller

↓

Inject ChatClient

↓

Create Prompt

↓

Call AI

↓

Return Response
```

---

# 1️⃣2️⃣ Best Practices

- Always use ChatClient unless low-level control is required.
- Keep API keys outside source code.
- Use environment variables for secrets.
- Use Prompt Templates for reusable prompts.
- Prefer Structured Output for JSON responses.
- Use Streaming for long responses.
- Validate AI output before processing.

---

# 1️⃣3️⃣ Common Mistakes

❌ Hardcoding API Keys

❌ Using ChatModel unnecessarily

❌ Very long prompts

❌ Ignoring token limits

❌ Not validating AI responses

❌ Mixing business logic inside controllers

---

# 1️⃣4️⃣ Interview Questions

### What is Spring AI?

Framework that integrates Generative AI into Spring Boot applications.

---

### Why use Spring AI instead of REST?

- Less boilerplate
- Provider independent
- Spring integration
- Better abstractions

---

### Difference between ChatClient and ChatModel?

| ChatClient | ChatModel |
|------------|-----------|
| High-level | Low-level |
| Easy | More control |
| Recommended | Internal |

---

### What is Prompt?

Represents complete input sent to AI.

---

### What is ChatResponse?

Represents output returned by AI.

---

# 1️⃣5️⃣ Quick Revision

```text
Spring Boot
      │
Spring AI
      │
ChatClient
      │
Prompt
      │
ChatModel
      │
Azure/OpenAI/Ollama
      │
LLM
      │
ChatResponse
      │
content()
```

---

# 1️⃣6️⃣ Master Mind Map

```text
Spring AI
│
├── Chat API
│   ├── ChatClient
│   ├── ChatModel
│   ├── Prompt
│   ├── ChatResponse
│   ├── UserMessage
│   ├── SystemMessage
│   └── AssistantMessage
│
├── Prompt Engineering
│
├── Structured Output
│
├── Streaming
│
├── Advisors
│
├── Image Generation
│
├── Audio Transcription
│
├── PDF Processing
│
├── AI Providers
│   ├── Azure OpenAI
│   ├── OpenAI
│   ├── Ollama
│   └── Hugging Face
│
└── Spring Boot
    ├── Dependencies
    ├── Configuration
    ├── REST API
    └── Controller
```

---

# 📝 Summary

After completing this chapter, you should be able to answer:

- What is Spring AI?
- Why use Spring AI?
- How does Spring AI work internally?
- What is the request lifecycle?
- What are the core components?
- Difference between ChatClient and ChatModel.
- Which AI providers are supported?
- How the complete architecture is connected.

> **Next File:** [📂 02-Project-Setup.md](./02-Project-Setup.md)