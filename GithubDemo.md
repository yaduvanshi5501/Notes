# 💬 Spring AI Chat Client Demo (GitHub Models)

## 🎯 Use Case

This demo shows how to connect a **Spring Boot** application with **GitHub Models** using **Spring AI**.

The application sends a prompt (question) to an AI model and receives an AI-generated response.

**Example:**

```text
User Prompt  →  Spring AI  →  GitHub Models (GPT-4.1)  →  AI Response
```

---

# 📦 Required Dependencies

## 1. Spring AI OpenAI Starter

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-openai</artifactId>
</dependency>
```

### Purpose

This is the **core Spring AI dependency**.

It provides:

- 🤖 Chat model integration
- 🧠 Embedding support
- ⚙️ Spring Boot auto-configuration
- 💬 `ChatClient` for interacting with AI models

> **Note:** GitHub Models provides an OpenAI-compatible API, so this dependency is used.

---

## 2. Azure OpenAI Starter

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-azure-openai</artifactId>
</dependency>
```

### Purpose

Use this dependency when connecting to **Azure OpenAI Service** instead of GitHub Models or OpenAI.

---

## 3. Spring AI BOM (Bill of Materials)

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-bom</artifactId>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### Purpose

The **BOM (Bill of Materials)** manages compatible versions of all Spring AI dependencies.

### Benefits

- Prevents version conflicts.
- Keeps all Spring AI libraries compatible.
- Allows version updates in one place.

---

# ⚙️ Spring AI Auto-Configuration

When the application starts, Spring Boot automatically:

1. Reads AI configuration from `application.properties`.
2. Creates an AI client.
3. Authenticates using the API key.
4. Creates a `ChatClient.Builder` bean.

This means you don't need to manually configure the AI client.

---

# 💬 ChatClient.Builder

`ChatClient.Builder` is automatically provided by Spring Boot.

### Purpose

- Creates a `ChatClient`.
- Reads configuration from `application.properties`.
- Connects to the configured AI provider.

Think of it as a **factory** that creates a ready-to-use AI client.

---

# 🤖 ChatClient

`ChatClient` is the main interface used to communicate with the AI model.

### Responsibilities

- Send prompts.
- Receive responses.
- Handle communication with the AI model.

Think of it as:

> **A ready-to-use connection between your application and the AI model.**

---

# 📝 Prompt

A **Prompt** is the input or question sent to the AI.

Example:

```java
String prompt = "Tell me a joke.";
```

---

# 🔄 Fluent API

Spring AI uses **method chaining (Fluent API)**.

```java
chatClient.prompt(prompt)
          .call()
          .content();
```

### Explanation

### `prompt(prompt)`

- Creates a new AI request.
- Sends the user's message.

↓

### `call()`

- Sends the request to the AI model.
- Waits for the response.

↓

### `content()`

- Extracts only the AI-generated text.
- Returns it as a `String`.

---

# 🔐 application.properties

```properties
spring.ai.openai.api-key=${GITHUB_TOKEN}
spring.ai.openai.base-url=https://models.github.ai
spring.ai.openai.chat.options.model=openai/gpt-4.1
```

### Purpose

Stores AI configuration such as:

- API Key
- Base URL
- Model Name

> **Best Practice:** Never hardcode API keys. Use environment variables or secure secret managers.

---

# 🚀 Complete Flow

```text
Application Starts
        │
        ▼
Spring Boot Auto Configuration
        │
        ▼
Creates ChatClient.Builder
        │
        ▼
Builds ChatClient
        │
        ▼
User Prompt
        │
        ▼
GitHub Models (GPT-4.1)
        │
        ▼
AI Response
        │
        ▼
Displayed to User
```

---

# 📝 Quick Revision

- **ChatClient.Builder** → Creates a configured `ChatClient`.
- **ChatClient** → Sends prompts and receives AI responses.
- **Prompt** → Input sent to the AI model.
- **call()** → Executes the request.
- **content()** → Returns the AI-generated text.
- **Spring AI BOM** → Manages compatible dependency versions.
- **Auto-Configuration** → Automatically configures the AI client using `application.properties`.