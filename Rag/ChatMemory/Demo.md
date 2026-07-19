# 📚 Chat Memory Demo – Stateless vs Stateful Conversations

> **Objective:** Learn the practical difference between stateless and stateful conversations in Spring AI using `MessageChatMemoryAdvisor`, `MessageWindowChatMemory`, and `conversationId`.

---

# 🎯 Learning Objectives

After completing this demo, you will understand:

- Difference between stateless and stateful conversations
- Why Chat Memory is required
- How `MessageChatMemoryAdvisor` works
- How `MessageWindowChatMemory` stores messages
- How `conversationId` isolates multiple conversations
- How to enable Chat Memory on a per-call basis

---

# 🤔 Why This Demo?

By default, every LLM request is independent.

Without Chat Memory:

- Previous questions are forgotten.
- Previous answers are forgotten.
- User information is lost.
- Follow-up questions become impossible.

This demo compares both approaches side by side.

---

# 🏗 Demo Overview

The project exposes two REST endpoints.

## 1. Stateless Chat

No Chat Memory is attached.

Every request is completely independent.

---

## 2. Stateful Chat

`MessageChatMemoryAdvisor` is attached.

Conversation history is automatically stored and reused.

---

# Architecture

## Stateless Conversation

```text
User

      │

      ▼

ChatClient

      │

      ▼

LLM

      │

      ▼

Response
```

Every request starts from scratch.

---

## Stateful Conversation

```text
User

      │

      ▼

ChatClient

      │

      ▼

MessageChatMemoryAdvisor

      │

      ▼

MessageWindowChatMemory

      │

      ▼

LLM

      │

      ▼

Store Response

      │

      ▼

Future Requests
```

Conversation history is automatically maintained.

---

# Scenario 1 – Stateless Conversation

## Purpose

Demonstrate how the LLM behaves without memory.

No advisor is attached.

Example:

### Request 1

```text
My name is Alice.
```

Response:

```text
Nice to meet you, Alice.
```

---

### Request 2

```text
What is my name?
```

Possible response:

```text
I don't know your name.
```

The model cannot remember previous interactions because each request is independent.

---

## Request Flow

```text
Request

↓

ChatClient

↓

LLM

↓

Response
```

No messages are stored.

---

# Scenario 2 – Stateful Conversation

## Purpose

Enable conversation memory.

This endpoint attaches:

```text
MessageChatMemoryAdvisor
```

The advisor automatically:

- Loads previous messages before calling the LLM.
- Stores the latest user message.
- Stores the AI response.

---

## Example

### Request 1

```text
My name is Alice.
```

Response:

```text
Nice to meet you, Alice.
```

---

### Request 2

```text
What is my name?
```

Response:

```text
Your name is Alice.
```

The model answers correctly because previous messages were included in the prompt.

---

# Request Lifecycle

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

Save New Messages

↓

Return Response
```

Everything happens automatically.

---

# MessageWindowChatMemory

The demo uses:

```text
MessageWindowChatMemory
```

Responsibilities:

- Store conversation messages
- Keep only the latest messages
- Automatically remove older messages

Configured size:

```text
20 Messages
```

When the limit is exceeded:

```text
Message 1

↓

Removed
```

Newest messages are always retained.

---

# Conversation ID

One of the most important concepts demonstrated in this project is:

```text
conversationId
```

Every conversation has its own identifier.

Example:

```text
customer-001
```

History:

```text
Hello

↓

Loan Question

↓

Interest Rate

↓

Repayment
```

---

Another user:

```text
customer-002
```

Has an entirely different history.

No messages are shared.

---

# Conversation Isolation

Without conversation IDs:

```text
Customer A

↓

Shared Memory

↑

Customer B
```

Messages become mixed.

---

With conversation IDs:

```text
Customer A

↓

Memory A

Customer B

↓

Memory B
```

Each user gets an isolated conversation.

---

# Why Use Per-Call Memory?

The ChatClient itself remains stateless.

Memory is attached only when required.

Benefits:

- More flexible
- Some endpoints remain stateless
- Some endpoints become stateful
- Better control over application behavior

---

# Banking Example

The demo uses a banking assistant.

Example conversation:

### Request

```text
I want to apply for a home loan.
```

---

### Follow-up

```text
What documents did you ask me to prepare?
```

Without memory:

The assistant has no idea.

With memory:

The assistant remembers the previous discussion and provides a contextual response.

---

# Components Used

| Component | Responsibility |
|------------|----------------|
| ChatClient | Sends prompts to the LLM |
| MessageWindowChatMemory | Stores recent conversation messages |
| MessageChatMemoryAdvisor | Loads and saves conversation history |
| ChatMemory | Manages conversation context |
| conversationId | Separates multiple conversations |

---

# Key Concepts Demonstrated

- Stateless conversations
- Stateful conversations
- Per-call advisor registration
- Chat Memory integration
- Sliding window memory
- Conversation isolation
- Multi-turn conversations

---

# Best Practices

- Use Chat Memory only where conversational context is required.
- Assign a unique `conversationId` to every user or session.
- Limit memory using `MessageWindowChatMemory` or `TokenWindowChatMemory`.
- Use persistent repositories in production instead of in-memory storage.
- Keep sensitive information out of Chat Memory unless it has been masked.

---

# Interview Questions

### Basic

- What is the difference between stateless and stateful conversations?
- Why do LLMs need Chat Memory?
- What is `MessageChatMemoryAdvisor`?

### Intermediate

- What is `conversationId`?
- Why does each user require a separate conversation ID?
- What does `MessageWindowChatMemory` do?

### Advanced

- Explain the complete request lifecycle with Chat Memory.
- Why is memory attached as an advisor?
- Why does Spring AI separate ChatClient from Chat Memory?

---

# 📝 Quick Revision

- Stateless conversations forget previous requests.
- Stateful conversations remember previous messages.
- `MessageChatMemoryAdvisor` connects ChatClient and Chat Memory.
- `MessageWindowChatMemory` stores recent conversation history.
- Conversation history is automatically loaded before every request.
- User messages and AI responses are automatically stored after every request.
- `conversationId` isolates multiple conversations.
- Different users never share the same memory.

---

# 🔥 Summary

This demo demonstrates the practical difference between stateless and stateful conversations in Spring AI. A stateless `ChatClient` sends each request independently, so the LLM cannot remember previous interactions. By attaching `MessageChatMemoryAdvisor` backed by `MessageWindowChatMemory`, Spring AI automatically loads previous conversation history before each request and stores new messages afterward, enabling natural multi-turn conversations. The demo also highlights the importance of `conversationId`, which isolates conversation history for different users or sessions, making Chat Memory suitable for scalable conversational AI applications.

---

# 💻 Complete Demo Project

> Keep the complete working project below as a reference.

Include:

- `BankingAssistantController.java`

This example demonstrates:

- Stateless conversation
- Stateful conversation
- `MessageWindowChatMemory`
- `MessageChatMemoryAdvisor`
- `conversationId`
- Per-call Chat Memory registration

```
package com.infy.chatmemory.controller;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.MessageChatMemoryAdvisor;
import org.springframework.ai.chat.client.advisor.SimpleLoggerAdvisor;
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.MessageWindowChatMemory;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api/banking")
@CrossOrigin
public class BankingAssistantController {

    // Stores conversation history in memory
    private final ChatMemory chatMemory;

    // Base ChatClient (stateless by default)
    private final ChatClient chatClient;

    public BankingAssistantController(ChatClient.Builder builder) {

        // Keeps only the latest 20 messages per conversation
        this.chatMemory = MessageWindowChatMemory.builder()
                .maxMessages(20)
                .build();

        this.chatClient = builder
                .defaultSystem("""
                        You are a professional banking assistant.

                        Help customers with account inquiries,
                        transactions and financial advice.

                        Mask sensitive account numbers whenever possible.
                        """)
                .defaultAdvisors(new SimpleLoggerAdvisor())
                .build();
    }

    // No ChatMemoryAdvisor -> Every request is independent
    @PostMapping("/chat-without-history")
    public String chatWithoutHistory(

            @RequestParam(defaultValue =
                    "I want to apply for a home loan. I have $50,000 saved and want to buy a $300,000 house")
            String userPrompt) {

        return chatClient.prompt(userPrompt)
                .call()
                .content();
    }

    // ChatMemoryAdvisor remembers previous messages
    @PostMapping("/chat-with-conversation-history")
    public String chatWithMemory(

            @RequestParam(defaultValue =
                    "I want to apply for a home loan. I have $50,000 saved and want to buy a $300,000 house")
            String userPrompt,

            // Same conversationId -> Same conversation history
            @RequestParam(defaultValue = "customer-001")
            String conversationId) {

        return chatClient.prompt(userPrompt)

                // Selects which conversation memory to use
                .advisors(a -> a.param(
                        ChatMemory.CONVERSATION_ID,
                        conversationId))

                // Reads previous messages and stores new ones
                .advisors(
                        MessageChatMemoryAdvisor.builder(chatMemory)
                                .order(1)
                                .build())

                .call()
                .content();
    }

}
```