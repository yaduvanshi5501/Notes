# 💬 ChatClient in Spring AI

## What is ChatClient?

`ChatClient` is the **primary API** in Spring AI used to communicate with **chat-based AI models** such as:

- OpenAI GPT
- Azure OpenAI
- GitHub Models
- Ollama
- Anthropic

It provides a **Fluent API (Method Chaining)** to easily send prompts and receive AI responses.

---

# Why Use ChatClient?

- Simple and clean API
- Provider-independent (same code works with different AI providers)
- Supports synchronous and streaming responses
- Integrates seamlessly with Spring Boot

---

# Understanding Prompt

A **Prompt** is the input sent to the AI model.

A prompt can contain:

- User Message
- System Message
- Prompt Options (Model, Temperature, etc.)

---

## Types of Messages

### 👤 User Message

Represents the user's request or question.

**Example**

```text
Explain Spring AI.
```

---

### 🤖 System Message

Provides instructions that define the AI's behavior.

**Example**

```text
You are a Java expert.
```

Used for:

- Setting AI's role
- Defining response style
- Applying rules or constraints

---

# Prompt Options

Prompt options control how the AI generates responses.

| Option | Purpose |
|---------|---------|
| **Model** | Selects the AI model (GPT-4o, GPT-4.1, etc.) |
| **Temperature** | Controls creativity (Low = More accurate, High = More creative) |

---

# Creating ChatClient

Spring Boot automatically provides a **ChatClient.Builder** bean.

```java
public MyService(ChatClient.Builder builder){
    this.chatClient = builder.build();
}
```

### ChatClient.Builder

- Creates a `ChatClient`
- Reads configuration from `application.properties`
- Automatically configured by Spring Boot

---

# Simple Prompt

Use when only a user message is required.

```java
chatClient
    .prompt("Explain Spring AI")
    .call()
    .content();
```

---

# Structured Prompt

Use when you need system instructions or multiple messages.

```java
chatClient.prompt()
          .system("You are a Java expert.")
          .user("Explain Spring AI")
          .call()
          .content();
```

---

# Understanding the Fluent API

```java
chatClient
    .prompt(prompt)
    .call()
    .content();
```

### `prompt()`

Creates a new prompt.

↓

### `call()`

Sends the request to the AI model.

↓

### `content()`

Returns only the AI-generated text.

---

# ChatClient Response Types

## 1️⃣ content()

Returns only the generated text.

```java
String response =
chatClient.prompt(question)
          .call()
          .content();
```

**Use when:** You only need the AI response.

---

## 2️⃣ chatResponse()

Returns a complete response object.

```java
ChatResponse response =
chatClient.prompt(question)
          .call()
          .chatResponse();
```

It contains:

- AI response
- Token usage
- Model information
- Finish reason

**Use when:** You need metadata along with the response.

---

# Streaming Responses

Normally,

```java
.call()
```

waits until the complete response is generated.

Streaming allows the AI to send the response **token by token**, just like ChatGPT.

---

## Why Streaming?

- Faster response
- Better user experience
- Non-blocking
- Real-time output

---

# What is Flux?

`Flux` is a **Project Reactor** class representing a stream of multiple values over time.

In Spring AI:

```java
Flux<String>
```

means each emitted value is a small piece (token/chunk) of the AI response.

---

## Streaming Flow

```text
AI Model
     │
     ▼
Token 1
     │
Token 2
     │
Token 3
     │
Flux<String>
     │
Server-Sent Events (SSE)
     │
Browser
```

---

# Streaming API

```java
chatClient
    .prompt()
    .user(question)
    .stream()
    .content();
```

### `stream()`

Enables streaming mode instead of waiting for the complete response.

### `content()`

Returns

```java
Flux<String>
```

where each item is one text chunk.

---

# Why Use SSE?

```java
produces = MediaType.TEXT_EVENT_STREAM_VALUE
```

This tells Spring Boot to send each token immediately to the browser.

Without it:

❌ Wait for complete response

With it:

✅ Stream response live (like ChatGPT)

---

# ChatClient Prompt Methods

## 1️⃣ prompt()

Creates an empty prompt.

```java
chatClient.prompt()
```

Use for complex prompts.

---

## 2️⃣ prompt(String)

Creates a prompt using a single user message.

```java
chatClient.prompt("Hello")
```

Use for simple requests.

---

## 3️⃣ prompt(Prompt)

Uses an already created `Prompt` object.

```java
Prompt prompt = new Prompt("Explain AI");

chatClient.prompt(prompt);
```

Use when prompts need to be reused.

---

<br><br>


---
# 💬 ChatClient Demo

## Use Case

Build a simple AI endpoint where the user asks a question and the AI returns an answer.

### Example

**Request**

```
GET http://localhost:8080/ai?question=What is Spring AI?
```

**Response**

```
Spring AI is an open-source framework that helps Java developers integrate AI models into Spring Boot applications using a common API.
```

---

# Project Flow

```
User
   │
   ▼
REST Controller
   │
   ▼
ChatClient
   │
   ▼
OpenAI / Azure OpenAI / GitHub Models / Ollama
   │
   ▼
AI Response
   │
   ▼
User
```

---

# Controller

```java
package com.example.demo.controller;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class AIController {

    private final ChatClient chatClient;

    // Spring injects ChatClient.Builder automatically
    public AIController(ChatClient.Builder builder) {
        this.chatClient = builder.build();
    }

    @GetMapping("/ai")
    public String askAI(@RequestParam String question) {

        return chatClient
                .prompt(question)
                .call()
                .content();
    }
}
```

---

# Code Explanation

## 1️⃣ ChatClient.Builder

```java
public AIController(ChatClient.Builder builder)
```

Spring Boot automatically provides a configured `ChatClient.Builder`.

It reads all AI configurations from `application.properties`.

---

## 2️⃣ Build ChatClient

```java
this.chatClient = builder.build();
```

Creates a ready-to-use `ChatClient`.

Think of it as:

> "Connect me to my configured AI model."

---

## 3️⃣ Receive User Input

```java
@RequestParam String question
```

Reads the user's question from the URL.

Example

```
/ai?question=Explain Spring AI
```

Here,

```
question = "Explain Spring AI"
```

---

## 4️⃣ Create Prompt

```java
.prompt(question)
```

Creates a prompt using the user's question.

---

## 5️⃣ Send Request

```java
.call()
```

Sends the prompt to the AI model.

---

## 6️⃣ Get Response

```java
.content()
```

Returns only the AI-generated text.

---

# Complete Flow

```
User

"What is Spring AI?"

      │

      ▼

prompt(question)

      │

      ▼

call()

      │

      ▼

GPT-4.1 / Ollama

      │

      ▼

content()

      │

      ▼

"Spring AI is an open-source framework..."
```

---

# Sample Output

### Input

```
What is Java?
```

### AI Response

```
Java is a high-level, object-oriented programming language used to build web, desktop, mobile, and enterprise applications.
```

---

# ChatClient Method Chain

```java
chatClient
    .prompt(question)
    .call()
    .content();
```

| Method | Purpose |
|---------|----------|
| `prompt()` | Creates the AI prompt |
| `call()` | Sends the prompt to the AI model |
| `content()` | Returns only the generated text |

---

# Quick Revision

- `ChatClient.Builder` → Creates a configured `ChatClient`.
- `ChatClient` → Sends prompts to the AI model.
- `prompt()` → Creates the user prompt.
- `call()` → Executes the request.
- `content()` → Returns the AI-generated response.