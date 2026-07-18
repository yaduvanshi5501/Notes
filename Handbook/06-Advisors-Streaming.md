# 🚀 Spring AI Handbook

# 06 - Advisors & Streaming

> **Goal:** Learn how Spring AI Advisors intercept AI requests and responses, how Streaming works internally, how advisors and streaming fit into the request lifecycle, and when to use each feature in real-world applications.

---

# 📖 Table of Contents

1. What are Advisors?
2. Why Advisors?
3. Advisor Lifecycle
4. Advisor Types
5. Advisor Chain
6. Built-in Advisors
7. Custom Advisors
8. Memory Advisors
9. Logging Advisor
10. Advisor Order
11. Streaming
12. How Streaming Works
13. Streaming Flow
14. Advisors + Streaming
15. Best Practices
16. Common Mistakes
17. Interview Questions
18. Quick Revision
19. Master Mind Map

---

# 1️⃣ What are Advisors?

An **Advisor** is an interceptor that executes **before** and/or **after** an AI request.

Think of it exactly like a Spring MVC Interceptor or Servlet Filter.

Instead of intercepting HTTP requests, it intercepts **AI requests**.

---

## Simple Analogy

```text
HTTP Request

↓

Spring Filter

↓

Controller
```

Spring AI

```text
Prompt

↓

Advisor

↓

ChatModel
```

---

# 2️⃣ Why Advisors?

Without Advisors

```text
Controller

↓

ChatClient

↓

LLM

↓

Response
```

No place exists to

- Log prompts
- Modify prompts
- Save chat history
- Add user context
- Track tokens
- Monitor requests

---

With Advisors

```text
Controller

↓

Advisor

↓

ChatClient

↓

LLM

↓

Advisor

↓

Response
```

---

# 3️⃣ Advisor Lifecycle

Every request passes through Advisors.

```text
User

↓

Controller

↓

Advisor

↓

Prompt

↓

ChatModel

↓

LLM

↓

ChatResponse

↓

Advisor

↓

Controller
```

Notice that Advisors execute both

- Before Request
- After Response

---

# 4️⃣ Advisor Responsibilities

An Advisor can

- Read Prompt
- Modify Prompt
- Add Context
- Read Response
- Modify Response
- Store Conversation
- Log Requests
- Count Tokens

---

# 5️⃣ Advisor Chain

Spring AI supports multiple Advisors.

They execute one after another.

```text
Prompt

↓

Advisor 1

↓

Advisor 2

↓

Advisor 3

↓

ChatModel
```

Response

```text
ChatResponse

↓

Advisor 3

↓

Advisor 2

↓

Advisor 1

↓

Controller
```

Exactly like Spring Filters.

---

# 6️⃣ Built-in Advisors

Common Advisors include

```text
Advisors

├── Simple Logger Advisor
├── Message Chat Memory Advisor
├── Prompt Chat Memory Advisor
├── Vector Store Advisor
└── Custom Advisor
```

---

## Simple Logger Advisor

Purpose

- Logs prompt
- Logs response
- Debugging
- Monitoring

Flow

```text
Prompt

↓

Logger

↓

Console
```

Useful during development.

---

## Message Chat Memory Advisor

Stores the complete conversation.

```text
User

↓

Question

↓

Memory

↓

Next Question

↓

Previous Context Available
```

Example

```text
User:
My name is Satish.

↓

Memory Saved

↓

Later

↓

User:
What is my name?

↓

AI:
Satish
```

---

## Prompt Chat Memory Advisor

Instead of storing complete messages,

it injects previous conversation into the next prompt.

Flow

```text
Old Conversation

↓

Prompt

↓

LLM
```

Useful for contextual conversations.

---

## Vector Store Advisor

Adds external knowledge.

```text
Question

↓

Vector Search

↓

Relevant Documents

↓

Prompt

↓

LLM
```

Useful in RAG (Retrieval-Augmented Generation).

---

# 7️⃣ Custom Advisors

You can create your own Advisor.

Examples

- Authentication
- Authorization
- Analytics
- Custom Logging
- Cost Tracking
- Prompt Sanitization

---

## Example Flow

```text
Prompt

↓

Custom Advisor

↓

Add Company Policy

↓

LLM
```

---

# 8️⃣ Advisor Order

Multiple Advisors execute according to their **order**.

Example

```text
Order 0

↓

Logging Advisor

↓

Order 1

↓

Memory Advisor

↓

Order 2

↓

Security Advisor

↓

ChatModel
```

Lower order executes first.

---

## Response Order

Response travels backwards.

```text
LLM

↓

Security Advisor

↓

Memory Advisor

↓

Logging Advisor

↓

Controller
```

---

# 9️⃣ Why Advisor Order Matters?

Suppose

Logging Advisor

and

Memory Advisor

If Logging executes first

```text
Prompt

↓

Logging

↓

Memory

↓

LLM
```

Logger records the original prompt.

---

If Memory executes first

```text
Prompt

↓

Memory

↓

Logging

↓

LLM
```

Logger records the modified prompt.

Different order

↓

Different behavior.

---

# 🔟 Streaming

Normally

```text
User

↓

Wait...

↓

Entire Response

↓

Display
```

Streaming sends data immediately.

```text
Word

↓

Word

↓

Word

↓

Word

↓

User
```

---

# Why Streaming?

Advantages

- Faster UI
- Better User Experience
- Less Waiting
- Live Typing Effect

---

# 1️⃣1️⃣ Traditional Response

```text
AI

↓

Generate Entire Answer

↓

Return

↓

Display
```

---

# 1️⃣2️⃣ Streaming Response

```text
AI

↓

Token

↓

Token

↓

Token

↓

Token

↓

Browser
```

User sees output while AI is still generating.

---

# 1️⃣3️⃣ Internal Streaming Flow

```text
Controller

↓

ChatClient

↓

ChatModel

↓

LLM

↓

Streaming Tokens

↓

Flux

↓

Browser
```

---

# 1️⃣4️⃣ What is Flux?

Spring AI Streaming uses

```text
Project Reactor

↓

Flux
```

Flux represents

> A stream of multiple values emitted over time.

Example

```text
Token 1

Token 2

Token 3

Token 4
```

---

# 1️⃣5️⃣ Advisors + Streaming

Even while streaming,

Advisors still execute.

Flow

```text
Prompt

↓

Advisor

↓

ChatModel

↓

Streaming

↓

Advisor

↓

Browser
```

So

- Logging still works
- Memory still works
- Security still works

---

# 1️⃣6️⃣ Real World Examples

## ChatGPT

Streaming

```text
Typing...

↓

Sentence appears gradually.
```

---

## Customer Support Bot

Memory Advisor remembers

```text
Customer Name

↓

Previous Issue

↓

Current Question
```

---

## Company AI Assistant

Security Advisor

```text
Employee

↓

Permission Check

↓

LLM
```

---

## RAG System

Vector Advisor

```text
Question

↓

Database Search

↓

Relevant Documents

↓

Prompt

↓

LLM
```

---

# 1️⃣7️⃣ Best Practices

✅ Use Logging Advisor during development.

✅ Use Memory Advisor for conversations.

✅ Keep Advisors lightweight.

✅ Avoid expensive operations inside Advisors.

✅ Stream long AI responses.

✅ Validate prompts before sending them.

✅ Place Advisors in the correct execution order.

---

# 1️⃣8️⃣ Common Mistakes

❌ Wrong Advisor order.

❌ Too many Advisors.

❌ Blocking operations inside Advisors.

❌ Logging sensitive information.

❌ Using Streaming for very short responses.

❌ Forgetting conversation IDs with Memory Advisors.

---

# 1️⃣9️⃣ Interview Questions

## What is an Advisor?

An Advisor intercepts AI requests and responses before and after they reach the LLM.

---

## Why use Advisors?

To

- Log requests
- Store memory
- Modify prompts
- Add security
- Add external context

---

## What is Streaming?

Streaming sends AI output gradually instead of waiting for the complete response.

---

## Why use Streaming?

- Better UX
- Faster perceived response
- Live updates

---

## What is Flux?

Flux is a Project Reactor type that emits multiple values asynchronously over time.

---

## Does Streaming skip Advisors?

No.

Advisors execute even during streaming.

---

## Why is Advisor Order important?

Because each Advisor receives the output of the previous Advisor.

Changing the order changes the behavior of the application.

---

# 2️⃣0️⃣ Quick Revision

```text
User

↓

Controller

↓

Advisor

↓

Prompt

↓

ChatClient

↓

ChatModel

↓

LLM

↓

Streaming Tokens

↓

Advisor

↓

Browser
```

---

# 🌳 Master Mind Map

```text
Advisors
│
├── Logging Advisor
│
├── Memory Advisor
│
├── Prompt Memory Advisor
│
├── Vector Store Advisor
│
├── Custom Advisor
│
├── Request Interception
│
├── Response Interception
│
└── Advisor Order
     │
     ├── Lower Order First
     └── Response in Reverse
│
├── Streaming
│
├── Flux
│
├── Tokens
│
├── Live Responses
│
└── Better User Experience
```

---

# 📚 Summary

After completing this chapter, you should be able to:

- Explain what Advisors are and why they are used.
- Describe the Advisor lifecycle.
- Understand how multiple Advisors execute using ordering.
- Differentiate between Logging, Memory, Prompt Memory, and Vector Store Advisors.
- Explain how Streaming works internally.
- Understand the role of `Flux` in streaming responses.
- Describe how Advisors and Streaming work together in Spring AI.
- Apply Advisors and Streaming effectively in real-world Spring AI applications.

---

## 🧠 Chapter Checklist

- ✅ Understand Advisors
- ✅ Understand Advisor Lifecycle
- ✅ Learn Built-in Advisors
- ✅ Learn Custom Advisors
- ✅ Understand Advisor Order
- ✅ Understand Streaming
- ✅ Learn Flux
- ✅ Learn Real-world Use Cases
- ✅ Ready for Advanced Spring AI Features

> **Next File:** * [📂 07-Multimodality.md](./07-Multimodality.md)
