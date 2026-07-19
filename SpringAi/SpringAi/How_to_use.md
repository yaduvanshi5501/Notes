# 🚀 Getting Started with Spring AI

To use **Spring AI**, integrate it into your **Spring Boot** application to communicate with AI models like **OpenAI, Azure OpenAI, GitHub Models, Ollama,** and **Hugging Face**.

---

# Step 1: Choose an AI Provider

Spring AI supports multiple AI providers, including:

- OpenAI
- Azure OpenAI
- GitHub Models
- Ollama (Local LLMs)
- Hugging Face

Choose the provider that best fits your application.

---

# Step 2: Get an API Key

Most cloud AI providers require an **API Key** for authentication.

> **Note:** Ollama runs locally, so it doesn't require an API key.

---

# Step 3: Configure the API Key

Store the API key securely in **environment variables** and reference it in `application.properties`.

```properties
spring.ai.openai.api-key=${AZURE_OPENAI_KEY}
```

> **Best Practice:** Never hardcode API keys in your source code.

---

# Step 4: Add Spring AI Dependency

Add the appropriate Spring AI starter dependency based on your AI provider.

### Azure OpenAI

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-azure-openai</artifactId>
</dependency>
```

> Spring Boot automatically configures the required beans using Auto-Configuration.

---

# Step 5: Use `ChatClient`

`ChatClient` is the main interface for interacting with chat-based AI models.

```java
chatClient.prompt(question)
          .call()
          .content();
```

### Explanation

- **prompt()** → Sends the user input to the AI model.
- **call()** → Executes the request.
- **content()** → Returns the AI-generated response.

---

# 🌟 Why Use ChatClient?

- Less boilerplate code
- Provider-independent API
- Easy to integrate with Spring Boot
- Simple to test and maintain

---

# 🔄 Spring AI Workflow

```text
Choose AI Provider
        │
        ▼
Get API Key
        │
        ▼
Configure application.properties
        │
        ▼
Add Spring AI Dependency
        │
        ▼
Use ChatClient
        │
        ▼
Send Prompt
        │
        ▼
Receive AI Response
```

---