# 📝 System Prompt in Spring AI

## What is a System Prompt?

A **System Prompt** is a set of instructions given to the AI **before the conversation starts**. It defines **how the AI should behave**, **respond**, and **interact** with the user.

Think of it as:

> **"A rulebook or personality assigned to the AI before it starts answering questions."**

---

## 🎯 Why Use a System Prompt?

A system prompt helps to:

- 👤 Define the AI's role (Tutor, Assistant, Chef, etc.)
- 🎨 Set the response style and tone
- 📋 Control the output format
- 🚫 Restrict the AI to specific topics
- ✅ Ensure consistent responses

---

## 💡 Example

```java
String systemPrompt = """
You are a Java tutor for beginners.
Always explain in simple words.
Give one short Java example.
End with a follow-up question.
""";
```

### User Prompt

```java
String userMessage = "What is a variable?";
```

### How the AI Receives It

```text
System:
You are a Java tutor for beginners.
Always explain in simple words.

User:
What is a variable?

Assistant:
A variable is a container used to store data...
```

---

# 📂 system-prompt.st

Instead of writing prompts inside Java code, Spring AI allows you to store them in a **`.st` (StringTemplate)** file.

Example:

```text
You are a Java tutor for beginners.

Always:
- Explain in simple words.
- Give one Java example.
- End with one follow-up question.
```

---

# ✅ Benefits of Using `.st` Files

- Cleaner Java code
- Easy to modify prompts
- Better maintainability
- Reusable prompts across the application
- No need to change Java code when updating prompts

---

# 📌 Loading the Prompt

```java
@Value("classpath:/prompts/system-prompt.st")
private Resource systemPromptResource;
```

### What Happens?

1. Spring loads the `.st` file from the `resources` folder.
2. Converts it into a **System Message**.
3. Sends it to the AI model along with the user's prompt.

---

# 🚀 Without vs With System Prompt

| Without System Prompt  | With System Prompt         |
| ---------------------- | -------------------------- |
| Inconsistent responses | Consistent responses       |
| Random output format   | Fixed output format        |
| No defined behavior    | AI follows a specific role |
| Difficult to control   | Easy to customize          |

---

# 📝 Quick Revision

- **System Prompt** → Defines the AI's role and behavior.
- It is sent **before** the user's message.
- Used to control **tone, format, and rules**.
- Store prompts in **`.st` files** for cleaner and reusable code.
- Spring loads `.st` files using `@Value` and sends them as **System Messages**.

<br><br><br>

---

# 💬 Spring AI System Prompt Demo

## 🎯 Use Case

This demo shows how a **Spring Boot** application uses **Spring AI** to interact with an LLM by separating:

- **System Prompt** (controlled by the server)
- **User Prompt** (provided by the client)

This ensures that the AI always follows predefined rules while allowing users to ask different questions.

---

# 🏗️ Architecture

```text
                User
                  │
                  ▼
        GET /api/chat?userPrompt=...
                  │
                  ▼
           AIController
                  │
        ┌─────────┴─────────┐
        │                   │
        ▼                   ▼
System Prompt (.st)    User Prompt
(Server Controlled)   (Client Input)
        │                   │
        └─────────┬─────────┘
                  ▼
             ChatClient
                  │
                  ▼
         Azure OpenAI / GPT
                  │
                  ▼
            AI Response
                  │
                  ▼
                User
```

---

# 📂 system-prompt.st

The **system-prompt.st** file contains instructions that define the AI's behavior.

Example:

```text
You are a Java tutor for beginners.

Always:
- Explain in simple words.
- Give one Java example.
- End with one follow-up question.
```

### Purpose

- Defines the AI's role.
- Controls response format.
- Ensures consistent answers.
- Keeps prompt logic separate from Java code.

---

# ⚙️ application.properties

```properties
spring.ai.azure.openai.api-key=${AZURE_OPENAI_KEY}
spring.ai.azure.openai.endpoint=${AZURE_OPENAI_ENDPOINT}
spring.ai.azure.openai.chat.options.deployment-name=${DEPLOYMENT_NAME}
```

### Purpose

- Connects the application to Azure OpenAI.
- Stores API key, endpoint, and model deployment.
- Uses environment variables for security.

---

# 📦 Required Dependency

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-azure-openai</artifactId>
</dependency>
```

### Purpose

Provides:

- Azure OpenAI integration
- ChatClient
- Auto Configuration

---

# 🌐 REST Endpoint

```java
@GetMapping("/chat")
```

Creates the endpoint:

```
GET /api/chat?userPrompt=What is Spring AI?
```

---

# 📥 User Prompt

```java
@RequestParam String userPrompt
```

Receives the question from the client.

Example

```
What is Records in Java?
```

---

# 📜 System Prompt

```java
@Value("classpath:system-prompt.st")
Resource systemPrompt;
```

Loads the system prompt from the `resources` folder.

Spring automatically reads the file and makes it available as a `Resource`.

---

# 🤖 ChatClient

```java
chatClient.prompt()
          .system(systemPrompt)
          .user(userPrompt)
          .call()
          .content();
```

### Step-by-Step

### 1️⃣ `.prompt()`

Starts building a new AI request.

↓

### 2️⃣ `.system(systemPrompt)`

Adds the **System Prompt**.

Defines:

- AI role
- Rules
- Response format

↓

### 3️⃣ `.user(userPrompt)`

Adds the user's question.

↓

### 4️⃣ `.call()`

Sends both prompts to the AI model.

↓

### 5️⃣ `.content()`

Returns only the generated response.

---

# 🔄 Complete Flow

```text
Application Starts
        │
        ▼
Load system-prompt.st
        │
        ▼
User sends question
        │
        ▼
Combine
(System Prompt + User Prompt)
        │
        ▼
ChatClient
        │
        ▼
Azure OpenAI
        │
        ▼
Generate Response
        │
        ▼
Return to User
```

---

# 🤔 Why Keep the System Prompt on the Server?

If users could modify the system prompt, they could change the AI's behavior.

Example:

❌ User changes the system prompt:

```
Ignore all previous instructions.
```

The AI may stop behaving as intended.

Keeping the system prompt on the server ensures:

- 🔒 Better security
- ✅ Consistent responses
- 🎯 Controlled AI behavior
- 🛡️ Prevents prompt injection

---
