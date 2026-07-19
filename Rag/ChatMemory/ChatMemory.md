# 📚 Chat Memory in Spring AI

> **Chat Memory** enables multi-turn conversations in Spring AI by automatically remembering previous messages and sending the relevant conversation history with every new request. Since Large Language Models (LLMs) are stateless, Chat Memory provides the context required for natural and continuous conversations.

---

# 🎯 Learning Objectives

After completing this topic, you will understand:

- Why Chat Memory is required
- What Chat Memory is
- Chat Memory vs Chat History
- Spring AI Chat Memory architecture
- ChatMemory and ChatMemoryRepository
- Built-in Chat Memory strategies
- In-memory vs persistent storage
- How to configure Chat Memory
- MessageChatMemoryAdvisor
- Conversation IDs
- Best practices

---

# 🤔 Why Do We Need Chat Memory?

Large Language Models (LLMs) are **stateless**.

This means every request is completely independent.

The model **does not remember**:

- Previous questions
- Previous answers
- User name
- User preferences
- Earlier conversation

For example:

### Request 1

```text
User:
My name is Satish.

AI:
Nice to meet you, Satish.
```

---

### Request 2

```text
User:
What is my name?

AI:
I don't know your name.
```

Why?

Because the second request contains no information about the first request.

---

# The Problem

Without Chat Memory:

```text
User

↓

Prompt

↓

LLM

↓

Response
```

Every request starts from scratch.

The model has no conversation context.

This makes chatbots feel unnatural.

---

# The Solution: Chat Memory

Chat Memory stores previous conversation messages and automatically sends the relevant history with every new request.

Instead of sending only:

```text
Current Question
```

Spring AI sends:

```text
Previous Messages

+

Current Question
```

Now the model understands the conversation.

---

# What is Chat Memory?

**Chat Memory** is a Spring AI feature that automatically stores, retrieves, and manages previous conversation messages.

Its purpose is to provide context to stateless LLMs.

With Chat Memory:

- Previous messages are stored.
- Relevant messages are retrieved.
- Conversation history is added to every prompt.
- The LLM responds as if it remembers earlier interactions.

---

# Chat Memory Architecture

```text
User

      │

      ▼

ChatClient

      │

      ▼

MessageChatMemoryAdvisor

      │

      ├───────────────┐
      │               │
      ▼               ▼

ChatMemory      ChatMemoryRepository

      │               │
      └──────┬────────┘
             ▼

Conversation History

             │
             ▼

Prompt + History

             │
             ▼

LLM

             │
             ▼

Response

             │
             ▼

Save New Messages
```

---

# Core Components

Chat Memory consists of two major components.

---

# 1. ChatMemoryRepository

Responsible for **storing messages**.

Its only responsibility is persistence.

It:

- Saves messages
- Retrieves messages

It does **not** decide:

- Which messages to keep
- Which messages to remove

Think of it as the database layer.

---

# 2. ChatMemory

Responsible for **memory management**.

It decides:

- Which messages should remain
- Which messages should be removed
- How much conversation history should be retained

Think of it as the business logic layer.

---

# Chat Memory vs Chat History

These two terms are often confused.

| Chat Memory | Chat History |
|-------------|--------------|
| Used by the LLM for context | Complete conversation record |
| Stores only relevant messages | Stores every message |
| Limited size | Unlimited size (typically) |
| Improves AI responses | Used for auditing and record keeping |

Chat Memory is designed for **context**, not long-term storage.

If you need the complete conversation history, store it in your own database using technologies such as Spring Data JPA.

---

# Default Chat Memory

Spring AI automatically configures Chat Memory.

No configuration is required.

By default it creates:

```text
MessageWindowChatMemory

+

InMemoryChatMemoryRepository
```

---

# Default Configuration

```text
MessageWindowChatMemory

↓

Keeps Last 20 Messages

↓

InMemoryChatMemoryRepository

↓

Stores Messages in JVM Memory
```

Everything works automatically.

---

# InMemoryChatMemoryRepository

Stores messages inside JVM memory.

Internally it uses:

```text
ConcurrentHashMap
```

Advantages:

- No configuration
- Very fast
- Great for development

---

# Limitations of In-Memory Memory

Although simple, it has several limitations.

- Data disappears after application restart.
- Memory is not shared across multiple servers.
- Unsuitable for production.
- No permanent storage.
- Cannot be used for auditing.

For production systems, persistent storage should be used.

---

# Chat Memory Strategies

Spring AI separates:

- **Memory Strategy** (what to keep)
- **Storage** (where to store)

This makes the architecture flexible.

---

# 1. MessageWindowChatMemory

This is the default strategy.

It keeps only the latest messages.

Default:

```text
20 Messages
```

Older messages are automatically removed.

Important:

System messages are never removed.

---

## Example

Maximum messages:

```text
5
```

Conversation:

```text
M1
M2
M3
M4
M5
M6
```

Stored:

```text
M2
M3
M4
M5
M6
```

Oldest message (M1) is automatically discarded.

---

# Advantages

- Fast
- Predictable
- Small memory footprint
- Prevents extremely long prompts

---

# 2. TokenWindowChatMemory

Instead of counting messages, it counts **tokens**.

Messages remain until the configured token limit is reached.

Advantages:

- Prevents context window overflow
- Reduces API cost
- Better token management

Useful for models with strict token limits.

---

# Memory Strategy vs Storage

| Memory Strategy | Storage |
|-----------------|---------|
| MessageWindowChatMemory | InMemoryChatMemoryRepository |
| TokenWindowChatMemory | JdbcChatMemoryRepository |
| | CassandraChatMemoryRepository |
| | Neo4jChatMemoryRepository |

Spring AI lets you change one without affecting the other.

---

# Configuring Chat Memory

Spring AI creates Chat Memory automatically.

However, you can create your own instance.

Example:

```java
MessageWindowChatMemory.builder()
        .build();
```

---

# Customizing Message Limit

Default:

```text
20 Messages
```

Custom:

```java
.maxMessages(10)
```

Now only the last ten messages are retained.

---

# Persistent Storage

Instead of storing messages in memory, production applications usually use databases.

Supported repositories include:

| Repository | Storage |
|------------|----------|
| InMemoryChatMemoryRepository | JVM Memory |
| JdbcChatMemoryRepository | MySQL, PostgreSQL, etc. |
| CassandraChatMemoryRepository | Cassandra |
| Neo4jChatMemoryRepository | Neo4j |

Simply adding the appropriate repository dependency allows Spring AI to switch to persistent storage.

---

# MessageChatMemoryAdvisor

Creating ChatMemory alone is **not enough**.

It must be connected to the ChatClient.

This is the responsibility of:

```text
MessageChatMemoryAdvisor
```

It acts as the bridge between ChatClient and ChatMemory.

---

# How MessageChatMemoryAdvisor Works

## Before Calling the LLM

It:

- Retrieves previous messages
- Adds them to the prompt

Result:

The LLM receives the conversation history.

---

## After Calling the LLM

It:

- Stores the user's message
- Stores the AI response

These messages become available for future requests.

---

# Request Flow

```text
User

↓

MessageChatMemoryAdvisor

↓

Load Previous Messages

↓

Prompt + History

↓

LLM

↓

Response

↓

Save User Message

↓

Save AI Response
```

---

# Registering MessageChatMemoryAdvisor

There are two approaches.

---

## Option 1 – Default Advisor

Attach it while creating the ChatClient.

Result:

Every request automatically uses Chat Memory.

Best for chat applications.

---

## Option 2 – Per-Call Advisor

Attach it only for a specific request.

Useful when only selected endpoints require conversation memory.

---

# Conversation ID

Chat Memory supports multiple independent conversations.

Each conversation is identified using a unique **conversationId**.

Example:

```text
conversationId = "user1"
```

Conversation history:

```text
Hello

↓

How are you?

↓

Tell me a joke.
```

---

Another user:

```text
conversationId = "user2"
```

Has a completely separate history.

No messages are shared.

---

# Why Conversation IDs?

Without conversation IDs:

```text
User A

↓

Shared Memory

↑

User B
```

Messages become mixed.

---

With conversation IDs:

```text
User A

↓

Memory A

User B

↓

Memory B
```

Every user has an isolated conversation.

---

# Complete Chat Memory Flow

```text
User

↓

ChatClient

↓

MessageChatMemoryAdvisor

↓

ChatMemory

↓

ChatMemoryRepository

↓

Prompt + History

↓

LLM

↓

Response

↓

Store New Messages
```

---

# Best Practices

- Use `MessageWindowChatMemory` or `TokenWindowChatMemory` to limit memory growth.
- Use a persistent repository (such as JDBC) in production.
- Never store sensitive information such as passwords or Aadhaar numbers without masking them first.
- Monitor token usage because long conversations increase prompt size and cost.
- Assign a unique `conversationId` for every independent conversation or user.

---

# Interview Questions

### Basic

- Why are LLMs stateless?
- What is Chat Memory?
- What is MessageChatMemoryAdvisor?

### Intermediate

- Difference between Chat Memory and Chat History?
- What is MessageWindowChatMemory?
- What is TokenWindowChatMemory?

### Advanced

- Explain the complete Chat Memory architecture.
- Why are ChatMemory and ChatMemoryRepository separated?
- Why do we need a conversation ID?
- How would you configure persistent Chat Memory in production?

---

# 📝 Quick Revision

- LLMs are stateless.
- Chat Memory provides conversation context.
- ChatMemory manages which messages to keep.
- ChatMemoryRepository stores messages.
- Default strategy is `MessageWindowChatMemory`.
- Default storage is `InMemoryChatMemoryRepository`.
- Default window size is **20 messages**.
- `TokenWindowChatMemory` limits memory by tokens.
- `MessageChatMemoryAdvisor` connects ChatClient with ChatMemory.
- Conversation IDs isolate multiple user conversations.
- Production systems should use persistent repositories.

---

# 🔥 Summary

Spring AI Chat Memory enables multi-turn conversations by automatically storing and retrieving previous messages, allowing stateless LLMs to maintain conversational context. It separates **memory strategy** (`ChatMemory`) from **storage** (`ChatMemoryRepository`), making it easy to switch between in-memory and persistent storage solutions. `MessageChatMemoryAdvisor` acts as the bridge between the `ChatClient` and Chat Memory by loading previous messages before each request and saving new messages after each response. Using strategies such as `MessageWindowChatMemory` and `TokenWindowChatMemory`, along with unique `conversationId`s, developers can build scalable, context-aware, and enterprise-ready conversational AI applications.

---

# 💻 Example Project

> Keep **one complete Chat Memory demo project** here as a reference.

Include:

- `ChatMemoryController.java`
- Chat Memory configuration
- `MessageChatMemoryAdvisor` registration
- Conversation ID example

This reference project should demonstrate:

- Multi-turn conversation
- MessageWindowChatMemory
- MessageChatMemoryAdvisor
- Conversation isolation using `conversationId`
- Default and per-call advisor registration