# 💬 Prompts in Spring AI

## 🤔 Why are Prompts Important?

A **Prompt** is the instruction sent to the AI model. It tells the model:

- 📝 **What** to do
- 🎭 **How** to behave
- 📚 **What context** to use
- 🎨 **How** the response should look

> 💡 **Better Prompt = Better AI Response**

---

# 📌 What is a Prompt?

In Spring AI, a **Prompt** is the complete input sent to the AI model.

A Prompt contains:

- 💬 One or more **Messages**
- ⚙️ Optional **ChatOptions**

```java
chatModel.call(prompt);
```

---

# 🏗️ Prompt Structure

```text
Prompt
│
├── Messages
│     ├── System Message
│     ├── User Message
│     ├── Assistant Message
│     └── Tool Message
│
└── Chat Options
      ├── Temperature
      ├── Max Tokens
      └── Model
```

---

# 🚀 Evolution of Prompts

### 1️⃣ Simple Text

```text
Explain Spring AI
```

↓

### 2️⃣ Formatted Prompt

```text
USER:
Explain Spring AI

ASSISTANT:
```

↓

### 3️⃣ Role-Based Prompt ✅ (Modern Approach)

```text
System:
You are a Java Tutor.

User:
Explain Spring AI.

Assistant:
...
```

Spring AI uses this **modern role-based approach**.

---

# 📦 Prompt API

```java
Prompt prompt = new Prompt(messages);
```

A Prompt contains:

| Component | Purpose |
|-----------|----------|
| Messages | Conversation |
| ChatOptions | AI configuration |

---

# 💬 Message

A **Message** is one part of a conversation.

```java
Message
```

contains

- Content
- Metadata
- Message Type

---

# 🎭 Message Types

## 🧠 System Message

Defines **how the AI should behave**.

Example

```text
You are a Java Expert.
```

Used for

- AI personality
- Rules
- Tone
- Output format

---

## 👤 User Message

Represents the user's question.

Example

```text
Explain Spring AI.
```

---

## 🤖 Assistant Message

Represents the AI's previous response.

Used for

- Conversation history
- Multi-turn chat

---

## 🔧 Tool Message

Contains the output returned by external tools or functions.

Example

```text
Temperature: 35°C
```

---

# 📊 Message Flow

```text
System Message
       │
       ▼
User Message
       │
       ▼
Assistant Message
       │
       ▼
Tool Message (Optional)
```

---

# 🖼️ Multimodal Prompt

Spring AI also supports images and other media.

```
Prompt

↓

Text

+

Image

↓

AI Model
```

Used for

- Image Analysis
- OCR
- Vision Models

---

# 🧠 Why Use Roles?

Roles help the AI understand:

- Who is speaking?
- What are the instructions?
- What is the user's request?
- What is previous conversation?

Result:

✅ Better Context

✅ Better Responses

---

# 💻 Prompt Demo

## 1️⃣ Simple Prompt

```java
chatClient.prompt("Explain Spring AI")
          .call()
          .content();
```

### Flow

```text
String

↓

UserMessage

↓

AI

↓

Response
```

Best for:

- Simple questions
- Quick AI calls

---

## 2️⃣ Structured Prompt

```java
Prompt prompt =
new Prompt(List.of(system,user));
```

```java
chatClient.prompt(prompt)
          .call()
          .content();
```

### Flow

```text
System Message

+

User Message

↓

Prompt

↓

AI
```

Best for

- Custom AI behavior
- Better output

---

## 3️⃣ Multi-Turn Conversation

```java
Prompt(
SystemMessage,

UserMessage,

AssistantMessage,

UserMessage
)
```

### Flow

```text
System

↓

User

↓

Assistant

↓

User

↓

AI
```

This gives the AI **conversation history**.

---

# 💡 Why AssistantMessage?

LLMs are **stateless**.

They don't remember previous conversations.

To provide memory,

we send previous AI responses back using

```java
AssistantMessage
```

---

# 📈 Prompt Comparison

| Prompt Type | Best For |
|-------------|----------|
| Plain String | Simple Questions |
| Structured Prompt | Better AI Control |
| Multi-Turn Prompt | Chat Applications |

---

# 🎯 Best Practice

✅ Use **Simple Prompt** for quick questions.

✅ Use **Structured Prompt** for production applications.

✅ Use **AssistantMessage** to maintain conversation history.

✅ Use **SystemMessage** to define AI behavior.

---

# 📝 Quick Revision

- **Prompt** → Complete input sent to AI.
- **Message** → One part of a Prompt.
- **SystemMessage** → Defines AI behavior.
- **UserMessage** → User's question.
- **AssistantMessage** → Previous AI response.
- **ToolMessage** → Output from external tools.
- **ChatOptions** → Temperature, Model, Max Tokens.
- **Prompt API** → Supports structured, reusable prompts.
- **Multi-turn conversations** → Achieved by sending previous messages again.