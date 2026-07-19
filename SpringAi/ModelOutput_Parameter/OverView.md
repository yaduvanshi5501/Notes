# 🎛️ Understanding Model Parameters in Spring AI

After learning about **Prompts**, **Prompt Templates**, and **Prompt Libraries**, the next step is understanding **Model Parameters**.

- **Prompt → tells the AI _what_ to do**
- **Model Parameters → tell the AI _how_ to do it**

Think of it like this:

```text
Prompt
   ↓
What should AI answer?

Model Parameters
   ↓
How should AI answer?
```

Example

Prompt:

```text
Explain Java Streams.
```

Different parameters produce different outputs.

```text
Temperature = 0.0
↓

Very factual

--------------------------------

Temperature = 1.5
↓

Creative explanation with analogies
```

---

# 🤔 What are Model Parameters?

Model parameters control how the AI generates responses.

They control:

- 🎨 Creativity
- 🎯 Accuracy
- 📏 Response Length
- 🔀 Randomness
- 🔁 Repetition

Think of them as:

> **"Knobs that tune the AI's behavior."**

Spring AI exposes these settings using:

```java
ChatOptions
```

Provider-specific implementations:

```java
OpenAiChatOptions

AzureOpenAiChatOptions
```

---

# 📋 Common Model Parameters

| Parameter | Purpose | Range |
|------------|---------|-------|
| **temperature** | Controls creativity | 0.0 – 2.0 |
| **topP** | Controls randomness (Nucleus Sampling) | 0.0 – 1.0 |
| **maxTokens** | Limits response length | Depends on model |
| **frequencyPenalty** | Reduces repetition | -2.0 to 2.0 |
| **presencePenalty** | Encourages new topics | -2.0 to 2.0 |
| **n** | Number of responses | 1+ |

---

# ⚙️ How Parameters Work

```text
User Prompt
      │
      ▼
 ChatClient
      │
      ▼
 Prompt
      │
      ▼
 Model Parameters
   • Temperature
   • TopP
   • MaxTokens
   • Penalties
      │
      ▼
 Azure/OpenAI
      │
      ▼
 AI Response
```

---

# 🌍 Global Default Configuration

Define default values in **application.properties**

```properties
spring.ai.azure.openai.chat.options.temperature=0.7
spring.ai.azure.openai.chat.options.max-tokens=1024
spring.ai.azure.openai.chat.options.top-p=1.0
spring.ai.azure.openai.chat.options.n=1
```

These settings are automatically applied to **every AI request** unless overridden.

---

# 🌡️ Temperature

## Purpose

Controls **creativity**.

```text
Low Temperature
↓

Predictable
↓

Accurate
↓

Consistent
```

```text
High Temperature
↓

Creative
↓

Random
↓

Unique
```

### Range

```text
0.0 – 2.0
```

Default

```text
0.7
```

### Temperature Guide

| Temperature | Result |
|-------------|--------|
| **0.0** | Fully deterministic |
| **0.2** | Very accurate |
| **0.5** | Balanced |
| **0.7** | Default |
| **1.2** | Creative |
| **1.5+** | Highly creative |

### Example

Prompt

```text
Write a tagline for a coffee shop.
```

Temperature = **0.0**

```text
Fresh Coffee Every Day.
```

Temperature = **1.5**

```text
Wake up your dreams one sip at a time.
```

---

# 🎲 topP (Nucleus Sampling)

## Purpose

Controls **how many possible words the AI can choose**.

Instead of considering every possible word, the AI only considers the most probable ones.

Example probabilities:

```text
Coffee → 60%

Tea → 20%

Milk → 10%

Pizza → 5%

Car → 5%
```

### topP = 1.0

```text
Coffee

Tea

Milk

Pizza

Car
```

### topP = 0.8

```text
Coffee

Tea
```

Low probability words are ignored.

### Guide

| topP | Result |
|------|--------|
| **1.0** | No restriction |
| **0.9** | Slight filtering |
| **0.7** | Strong filtering |
| **0.5** | Very focused |

### Best Practice

Use **either**

- Temperature

OR

- topP

Avoid tuning both aggressively together.

---

# 📏 maxTokens

## Purpose

Limits the maximum response length.

Example

```text
maxTokens = 50
↓

Very short answer
```

```text
maxTokens = 1000
↓

Detailed explanation
```

### Approximate Token Size

```text
1 Token

≈ 4 Characters

≈ 0.75 Words
```

| Tokens | Output |
|----------|--------|
| 100 | Short answer |
| 300 | Summary |
| 800 | Detailed explanation |
| 1500 | Long article |

---

# 🔁 n (Multiple Responses)

Generates multiple responses for the same prompt.

Example

```java
options.setN(3);
```

Prompt

```text
Suggest a company name.
```

Output

```text
1. TechNova

2. ByteSphere

3. CloudEdge
```

---

# 🔂 Frequency Penalty

## Purpose

Reduces repeated words or phrases.

Without penalty

```text
Java is good.

Java is good.

Java is good.
```

With penalty

```text
Java is a powerful programming language.

It supports object-oriented programming.
```

---

# 🆕 Presence Penalty

## Purpose

Encourages the AI to introduce new ideas.

Without

```text
Java

Java

Java

Java
```

With

```text
Java

Spring Boot

Hibernate

Docker

Microservices
```

---

# 🔧 Override Parameters for One Request

Instead of using the global configuration, you can override parameters for a single request.

```java
AzureOpenAiChatOptions options = new AzureOpenAiChatOptions();

options.setTemperature(1.2);
options.setMaxTokens(500);
options.setTopP(0.9);
options.setN(2);

String response = chatClient
        .prompt(prompt)
        .options(options)
        .call()
        .content();
```

Request-level options take precedence over **application.properties**.

---

# 🔄 Complete Flow

```text
application.properties
       │
       ▼
Default Parameters
       │
       ▼
User Request
       │
       ▼
(Optional)
Override Parameters
       │
       ▼
ChatClient
       │
       ▼
Azure/OpenAI
       │
       ▼
AI Response
```

---

# 🖼️ Image Generation Parameters

```java
ImageOptions.builder()
    .model("gpt-image-1")
    .size("1024x1024")
    .background(ImageOptions.Background.TRANSPARENT)
    .build();
```

Common Parameters

- Model
- Size
- Background
- Style
- Quality

---

# 🎙️ Audio Transcription Parameters

```java
AzureOpenAiAudioTranscriptionOptions options =
AzureOpenAiAudioTranscriptionOptions.builder()
        .deploymentName("gpt-4o-mini-transcribe")
        .language("en")
        .build();
```

Controls

- Model
- Language

---

# 🎯 Recommended Configurations

## Code Generation

```text
Temperature : 0.1
MaxTokens  : 1500
TopP       : 1.0
```

Reason

```text
Need accurate code.
```

---

## Chatbot

```text
Temperature : 0.7
MaxTokens  : 400
TopP       : 1.0
```

Reason

```text
Balanced conversation.
```

---

## Creative Writing

```text
Temperature : 1.3
MaxTokens  : 1200
TopP       : 1.0
```

Reason

```text
Need creativity.
```

---

## Summarization

```text
Temperature : 0.3
MaxTokens  : 300
```

Reason

```text
Need short and accurate summaries.
```

---

## Q&A / RAG

```text
Temperature : 0.0
MaxTokens  : 500
```

Reason

```text
Need factual responses.
```

---

## Sentiment Analysis

```text
Temperature : 0.0
MaxTokens  : 100
```

Reason

```text
Need deterministic output.
```

---

# ✅ Best Practices

- Use **temperature = 0–0.3** for APIs.
- Use **temperature = 0.7** for chatbots.
- Use **temperature >1** for creative writing.
- Always set **maxTokens**.
- Use **deploymentName**, not GPT model names.
- Prefer **Temperature OR topP** for tuning.
- Keep parameter values consistent.
- Limit maxTokens to reduce cost.

---

# ❌ Common Mistakes

| Mistake | Result |
|----------|--------|
| High temperature in APIs | Inconsistent responses |
| No maxTokens | Higher API cost |
| Creative settings for JSON | Invalid JSON |
| Random parameter changes | Difficult debugging |

---

# 🚀 Quick Revision

| Parameter | Controls |
|------------|----------|
| **temperature** | Creativity |
| **topP** | Diversity of token selection |
| **maxTokens** | Response length |
| **frequencyPenalty** | Reduces repetition |
| **presencePenalty** | Introduces new topics |
| **n** | Number of generated responses |

---

# 💡 Interview One-Liners

- **Temperature** → Controls creativity and randomness.
- **topP** → Controls diversity using nucleus sampling.
- **maxTokens** → Limits response length.
- **frequencyPenalty** → Reduces repeated words.
- **presencePenalty** → Encourages new ideas.
- **n** → Generates multiple responses for one prompt.

---

# 📝 Easy Way to Remember

```text
Temperature
↓

Creative or Accurate?

TopP
↓

How many possible words can AI choose?

MaxTokens
↓

How long can AI answer?

Frequency Penalty
↓

Don't repeat yourself.

Presence Penalty
↓

Talk about something new.

n
↓

Generate multiple answers.
```
<br><br>
---


# 🎯 Demo – Model Output Parameters in Spring AI

## 📌 Use Case

This demo shows how to control the behavior of an LLM using **Spring AI Model Parameters**.

It demonstrates **three scenarios**:

1. **Generate a response using global (default) model parameters** configured in `application.properties`.
2. **Override model parameters for a single request** using `AzureOpenAiChatOptions`.
3. **Retrieve token usage and model metadata** such as model name, prompt tokens, completion tokens, and total tokens.

This approach is useful for building chatbots, AI assistants, content generators, and other enterprise AI applications where response behavior must be tuned dynamically.

---

# 📂 Project Structure

```text
src
├── controller
│      ModelParameterController.java
│
├── dto
│      RequestModel.java
│      ResponseModel.java
│
└── resources
       application.properties
```

---

# ⚙️ application.properties

Configure the **default behavior** of the AI model.

```properties
spring.application.name=model-output-parameter

spring.ai.azure.openai.api-key=${AZURE_OPENAI_KEY}

spring.ai.azure.openai.endpoint=${AZURE_OPENAI_ENDPOINT}

spring.ai.azure.openai.chat.options.deployment-name=gpt-4.1-mini

spring.ai.azure.openai.chat.options.temperature=0.7

spring.ai.azure.openai.chat.options.max-tokens=1024

spring.ai.azure.openai.chat.options.top-p=1.0

spring.ai.azure.openai.chat.options.n=1
```

These settings become the **default parameters** for every request.

---

# 🚀 Scenario 1 – Generate Response Using Default Parameters

### Endpoint

```text
POST /model-parameter/generate-with-defaults
```

### Flow

```text
User Request
      │
      ▼
Prompt
      │
      ▼
application.properties
(Default Parameters)
      │
      ▼
Azure OpenAI
      │
      ▼
Response
```

### What Happens?

- Reads the prompt from the request.
- Uses default parameters from **application.properties**.
- Sends the prompt to Azure OpenAI.
- Returns the generated response.

No custom parameters are used.

---

# 🚀 Scenario 2 – Generate Response Using Custom Parameters

### Endpoint

```text
POST /model-parameter/generate-with-custom-parameters
```

### Flow

```text
User Request
      │
      ▼
RequestModel
      │
      ▼
AzureOpenAiChatOptions
      │
      ▼
Override Default Parameters
      │
      ▼
Azure OpenAI
      │
      ▼
Response
```

### What Happens?

Instead of using global settings, this endpoint creates

```java
AzureOpenAiChatOptions
```

and sets values dynamically.

Example

```java
options.setTemperature(...);

options.setMaxTokens(...);

options.setTopP(...);

options.setN(...);
```

These values override the global configuration **only for the current request**.

---

# 🚀 Scenario 3 – Token Usage & Metadata

### Endpoint

```text
POST /model-parameter/token-usage
```

### Flow

```text
Prompt
      │
      ▼
Azure OpenAI
      │
      ▼
ChatResponse
      │
      ▼
Metadata
      │
      ▼
ResponseModel
```

### What Happens?

Instead of returning only text, Spring AI returns

```java
ChatResponse
```

From it we extract

- Generated Response
- Model Name
- Prompt Tokens
- Completion Tokens
- Total Tokens

---

# 📦 RequestModel

Represents the input received from the client.

```java
public class RequestModel
```

Contains

| Field | Purpose |
|--------|----------|
| prompt | User prompt |
| temperature | Creativity |
| topP | Randomness |
| maxTokens | Maximum response length |
| nChoices | Number of responses |

Example

```json
{
  "prompt":"Explain Spring AI",
  "temperature":0.8,
  "topP":0.9,
  "maxTokens":500,
  "nChoices":1
}
```

---

# 📦 ResponseModel

Represents metadata returned by Spring AI.

Contains

| Field | Purpose |
|--------|----------|
| response | AI generated response |
| model | Model used |
| promptTokens | Tokens in prompt |
| completionTokens | Tokens generated |
| totalTokens | Total tokens used |

Example

```json
{
   "model":"gpt-4.1-mini",

   "promptTokens":18,

   "completionTokens":142,

   "totalTokens":160,

   "response":"Spring AI is..."
}
```

---

# 🔍 Controller Endpoints

## 1️⃣ Generate with Defaults

```text
POST /generate-with-defaults
```

Uses

```text
Prompt

↓

application.properties

↓

Azure OpenAI

↓

Response
```

No runtime customization.

---

## 2️⃣ Generate with Custom Parameters

```text
POST /generate-with-custom-parameters
```

Uses

```text
Prompt

↓

AzureOpenAiChatOptions

↓

Override Defaults

↓

Azure OpenAI

↓

Response
```

Allows each request to have different settings.

---

## 3️⃣ Token Usage

```text
POST /token-usage
```

Uses

```text
Prompt

↓

ChatResponse

↓

Metadata

↓

ResponseModel
```

Useful for monitoring token consumption and API costs.

---

# 💡 Why Use Custom Parameters?

Different applications require different AI behavior.

Examples

| Use Case | Temperature |
|----------|-------------|
| Chatbot | 0.7 |
| Code Generation | 0.1 |
| Summarization | 0.3 |
| Creative Writing | 1.5 |
| Sentiment Analysis | 0.0 |

---

# 📊 Default vs Custom Parameters

| Default Parameters | Custom Parameters |
|-------------------|-------------------|
| Defined in `application.properties` | Defined in code |
| Applied to every request | Applied to one request only |
| Easy to maintain | Highly flexible |
| Consistent responses | Different behavior per request |

---

# 🏷️ Why Retrieve Token Usage?

Token usage helps you:

- Monitor API cost
- Analyze model performance
- Debug prompts
- Track response size
- Optimize applications

---

# 🔄 Overall Flow

```text
Client Request
       │
       ▼
ModelParameterController
       │
       ├──────────────► Default Parameters
       │
       ├──────────────► Custom Parameters
       │
       └──────────────► Token Usage
                         │
                         ▼
                    ChatClient
                         │
                         ▼
                  Azure OpenAI
                         │
                         ▼
               ChatResponse
                         │
         ┌───────────────┴──────────────┐
         ▼                              ▼
 Generated Response              Metadata
                                      │
                                      ▼
                               ResponseModel
```

---

# ✅ Key Takeaways

- `application.properties` stores **global model parameters**.
- `AzureOpenAiChatOptions` overrides parameters **per request**.
- `ChatClient` sends prompts to the LLM.
- `ChatResponse` provides both **generated text** and **metadata**.
- `RequestModel` carries prompt and runtime parameters.
- `ResponseModel` returns token usage and model details.
- Retrieving token usage helps monitor API cost and optimize prompts.